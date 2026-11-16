# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 02:49** · publicado em **16/11/2026 02:51** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 16/11 02:39 · sessao a02a7cbd1c80 · - · - · origem=Ronald (tmux)

Inventário completo por AST: há **um chamador produtivo direto** de `ponto.services.ausencia.decidir`, o wrapper `_a4_decidir`; ele apenas repassa argumentos e relança `TransicaoInvalida`. (`app/ponto/views.py:85`, `app/ponto/views.py:88`, `app/ponto/views.py:90`)

| Chamador do wrapper | Efeito do novo `atomic` |
|---|---|
| `aprovar_ausencia_lote` | Transação por item; `ok += 1` vem depois do retorno. Captura apenas `DoesNotExist` e `AusenciaInvalida`; erro inesperado aborta a requisição, preservando itens anteriores já concluídos. (`app/ponto/views.py:2292`, `app/ponto/views.py:2302`, `app/ponto/views.py:2304`, `app/ponto/views.py:2305`) |
| `rejeitar_ausencia_lote` | Mesmo comportamento por item; contador não sobe quando a porta levanta. (`app/ponto/views.py:2333`, `app/ponto/views.py:2338`, `app/ponto/views.py:2342`, `app/ponto/views.py:2343`) |
| `cancelar_ausencia_lote` | Transação por item; log adicional de cancelamento fica depois, fora da transação da porta. (`app/ponto/views.py:2370`, `app/ponto/views.py:2378`, `app/ponto/views.py:2387`, `app/ponto/views.py:2390`) |
| `aprovar_ausencia` | Já envolve todos os irmãos em `atomic`; agora cada chamada abre savepoint interno. Exceção sai do bloco coletivo antes do tratamento e desfaz todos os irmãos; pushes vêm depois. (`app/ponto/views.py:2674`, `app/ponto/views.py:2676`, `app/ponto/views.py:2679`, `app/ponto/views.py:2694`) |
| `editar_periodo_ausencia` | Período, trilha e invalidação passam a compor a transação da porta. Captura `AusenciaInvalida`, `ValueError` e `DoesNotExist`; demais erros sobem. (`app/ponto/views.py:2709`, `app/ponto/views.py:2710`) |
| `rejeitar_ausencia` | Cada irmão ganha transação própria, mas continua sem transação coletiva; erro posterior conserva irmãos anteriores. O tratamento amplo devolve mensagem de erro. (`app/ponto/views.py:2735`, `app/ponto/views.py:2737`, `app/ponto/views.py:2755`) |
| `cancelar_ausencia` | Decisão fica atômica; log adicional permanece fora da porta. Captura somente ausência inexistente e `AusenciaInvalida`. (`app/ponto/views.py:2773`, `app/ponto/views.py:2778`, `app/ponto/views.py:2784`, `app/ponto/views.py:2787`) |

Não encontrei chamador produtivo na API da cópia. `saas_core` corresponde às rotas `config.urls_core → api.urls_core → views_core`, cuja lista não contém decisão de ausência. (`app/config/urls_core.py:6`, `app/api/urls_core.py:5`, `app/api/urls_core.py:7`, `app/api/views_core.py:1`)

Há **29 chamadas diretas em testes**, distribuídas nestes arquivos: `app/ponto/tests/test_porta_ausencia_ferias.py:113,114`; `app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:168`; `app/ponto/tests/test_a4_estados_com_porta.py:171,180,259,269,328,345,357`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:186,306,315,320,330,335,346,380,400`; `app/escala/tests/test_cobertura_parcial_periodo.py:164,180,189,193,198,227`; `app/api/tests/test_atestado_por_minutos.py:114`; `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:69,137`; `app/chamados/tests/test_s4_pendente_suspende_cobranca.py:87`.

Pontos relevantes para consolidar:

- A exceção que **escapa** depois da transição agora desfaz transição, carimbos, período e demais escritas participantes; o bloco cobre todo `_decidir_no_ato`, inclusive acordador final. (`app/ponto/services/ausencia.py:1079`, `app/ponto/services/ausencia.py:1088`, `app/ponto/services/ausencia.py:1104`, `app/ponto/services/ausencia.py:1122`)
- `post_save` continua síncrono: o cartório roda no primeiro save e novamente no segundo, quando existente. `atomic` adia os callbacks `on_commit`, sem adiar os receivers. (`app/ponto/services/ausencia.py:988`, `app/ponto/services/ausencia.py:1104`, `app/ponto/signals.py:143`, `app/ponto/signals.py:147`, `app/ponto/signals.py:272`)
- Os dois saves continuam registrando dois callbacks, sem deduplicação. Ambos capturam a mesma instância; agora observam seus valores finais ao executar depois do commit. Rollback descarta callbacks registrados no trecho desfeito. (`app/ponto/signals.py:252`, `app/ponto/signals.py:254`, `app/ponto/signals.py:272`, `app/ponto/signals.py:275`, `app/chamados/services/trava_do_colaborador.py:12`)
- Rollback não restaura os atributos Python atribuídos antes dos saves; o teste novo recarrega o objeto antes de conferir o estado. (`app/ponto/services/ausencia.py:976`, `app/ponto/services/ausencia.py:1091`, `app/ponto/services/ausencia.py:1102`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:402`)
- O `registrar_log` real engole erros, e `evento` protege sua escrita com savepoint próprio. O teste novo injeta uma exceção no mock de `registrar_log`, portanto demonstra rollback diante de exceção propagada, não falha real da auditoria propagando. (`app/core/auditoria.py:25`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:394`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:398`)
- `invalidar_fechamento` ainda engole falhas com savepoints próprios e retorna zero: continua possível concluir a decisão com fechamento carimbado se a invalidação falhar; ocorrência produtiva **[nao verificado]**. (`app/ponto/services/fechamento.py:1648`, `app/ponto/services/fechamento.py:1651`, `app/ponto/services/fechamento.py:1661`, `app/ponto/services/fechamento.py:1665`)
- `_julgar_dias` engole sem savepoint explícito, mas não confirmei retorno silencioso de `decidir` com rollback: depois dos saves há obrigatoriamente consulta ao banco pelo snapshot final, que expõe conexão abortada antes do incremento de sucesso. (`app/ponto/signals.py:85`, `app/ponto/signals.py:88`, `app/core/observ.py:90`, `app/ponto/services/ausencia.py:1122`, `app/ponto/services/ausencia.py:138`, `app/ponto/views.py:2304`)

Execução Django e incidência produtiva: **[nao verificado]**.

## 16/11 02:39 · sessao 6339b5894009 · LOTE3-C6 · verificador · origem=Code

**O achado da volta 1 está parcialmente fechado: resta 1 PROCEDENTE na reabertura, antes de `criar_ausencia` devolver o objeto.** (`app/ponto/services/ausencia.py:350`, `app/ponto/services/ausencia.py:1200`, `app/ponto/views.py:2492`)

Execução dos testes, ocorrências em produção e estado efetivo do storage: **[não verificado]**. [nao verificado]

**1. Fronteira de rollback e limpeza**

O caso originalmente testado está coberto: depois que `criar_ausencia` retorna, uma falha em `_gravar_colab` sai do `atomic`; a limpeza compara o nome mantido no objeto com o documento da mesma PK relida após o rollback. Isso contempla linha nova e reabertura já devolvida. (`app/ponto/views.py:2465`, `app/ponto/views.py:2468`, `app/ponto/views.py:2485`, `app/ponto/views.py:2496`)

**PROCEDENTE — falha intermediária em `_reabrir` deixa o upload órfão.** O caminho é:

1. `criar_ausencia` encontra uma rejeitada e delega a `_reabrir`. (`app/ponto/services/ausencia.py:315`, `app/ponto/services/ausencia.py:350`)
2. `_reabrir` salva o documento novo; depois ainda pode salvar a descrição e mudar o estado, com outro `save`. (`app/ponto/services/ausencia.py:1200`, `app/ponto/services/ausencia.py:1210`, `app/ponto/services/ausencia.py:1211`, `app/ponto/services/ausencia.py:900`)
3. Se uma dessas escritas posteriores falhar, o `atomic` da view desfaz a troca do documento, mas a atribuição `ausencia = criar_ausencia(...)` não terminou: `ausencia` continua `None`. (`app/ponto/views.py:2462`, `app/ponto/views.py:2465`, `app/ponto/views.py:2468`)
4. A limpeza externa é pulada; a interna só cobre exceção do próprio `save` do documento. O banco volta ao documento anterior e o arquivo novo permanece no storage. (`app/ponto/views.py:2492`, `app/ponto/services/ausencia.py:1199`, `app/ponto/services/ausencia.py:1202`)

Quanto a **apagar arquivo ainda referenciado**, a comparação pós-rollback preserva o documento restaurado da mesma linha; não há consulta global por outras linhas com o mesmo nome. Compartilhamento real desses nomes entre linhas: **[não verificado]**. (`app/ponto/views.py:2496`, `app/ponto/services/ausencia.py:169`)

A limpeza também não oferece garantia diante de falha da consulta pós-rollback ou da exclusão física: a consulta antecede o helper, e o helper captura falha de `delete` e retorna `False`. Ocorrência efetiva: **[não verificado]**. (`app/ponto/views.py:2496`, `app/ponto/services/ausencia.py:172`, `app/ponto/services/ausencia.py:174`)

**2. Censo dos outros chamadores com `atomic` externo**

| Chamador e fronteira | Escrita/falha possível depois | Upload novo órfão neste ato? |
|---|---|---|
| `regularizar_turno_orfao`: `app/ponto/services/regularizacao.py:89` → `garantir_ausencia`:118 | Analisar chamado e anexar nota:137,139 | Não passa documento:118. |
| Abandono provisório: `app/ponto/management/commands/marcar_abandono_provisorio.py:22` → `garantir_ausencia`:76 | Próximas iterações:70; rollback explícito:98 | Não passa documento:76. |
| Veredito da célula: `app/ponto/services/veredito_celula.py:224` → `criar_ausencia`:337 | Fechar fio:347; lavrar:352 | Não passa documento:337. |
| Materialização: `app/chamados/services/materializacao.py:1226` → escolha:1232 → declaração:1084 → criação:491 | Reconciliar:1085; carimbar ou levantar recusa:1283,1285 | Não passa documento:491. |
| Espelho de férias: `app/ferias/models.py:449` → sincronização:394 → `app/ponto/services/ausencia.py:1275` | Outros dias:1270; exclusões:1287; pauta:1290 | Não passa documento:1275. |

`garantir_ausencia` também pode alcançar `_reabrir`, mas nesses chamadores o documento continua `None`; a escrita de upload da reabertura exige `if documento`. Portanto podem desfazer linhas ou reaberturas, mas não deixam arquivo **escrito neste ato** por esses caminhos. (`app/ponto/services/ausencia.py:180`, `app/ponto/services/ausencia.py:350`, `app/ponto/services/ausencia.py:523`, `app/ponto/services/ausencia.py:1191`)

As fronteiras transitivas do espelho de férias são:

| Operação | `atomic` | Ato que dispara o espelho | Escrita posterior |
|---|---|---|---|
| Agendar com abono | `app/ferias/services.py:297` | :306 | :316 |
| Gozo histórico | `app/ferias/services.py:519` | :547 | :551 |
| Executar agendamento | `app/ferias/services.py:567` | :581 | :587 |
| Trocar janela | `app/ferias/services.py:706` | :729 | :731 |
| Aprovar solicitação | `app/ferias/services.py:755`, :774 | :786 | :789 |
| Corrigir datas pela tela | `app/ferias/views.py:524` | :531 | :533 |
| Alinhar status pela data | `app/ferias/services.py:951` | :956 | Continuação do espelho: `app/ponto/services/ausencia.py:1284` |

Os demais pontos de upload identificados não abrem `atomic` externo envolvendo a chamada: criação pela API/PWA, reenvio pela API e anexações administrativa/API. A reversão humana também não abre essa fronteira e não passa upload. (`app/api/views.py:1812`, `app/api/views.py:1845`, `app/api/views.py:3117`, `app/ponto/views.py:2603`, `app/ponto/views.py:2159`, `app/ponto/views.py:2651`)

**3. Quem escreve e quem lê**

**Escritores do campo:** `criar_ausencia`, `anexar_documento` e `_reabrir`; o destino físico declarado é `ausencias/documentos/`. (`app/ponto/services/ausencia.py:410`, `app/ponto/services/ausencia.py:479`, `app/ponto/services/ausencia.py:923`, `app/ponto/services/ausencia.py:925`, `app/ponto/services/ausencia.py:1198`, `app/ponto/services/ausencia.py:1200`, `app/ponto/models.py:777`)

A normalização muda o nome do upload antes do armazenamento; a exclusão física identificada está em `_apagar_anexo_escrito_agora`. O admin de Ausencia está configurado somente para leitura. (`app/ponto/services/ausencia.py:658`, `app/ponto/services/ausencia.py:172`, `app/ponto/admin.py:38`, `app/core/admin.py:62`)

**Leitores físicos dos arquivos:**

- Identificação do tipo real: `app/ponto/models.py:861`.
- Entrega HTTP, seguindo `documento_referencia`: `app/ponto/views.py:2122`, `app/ponto/views.py:2129`.
- Exportação ZIP: `app/ponto/services/atestados_lote.py:157`, `app/ponto/services/atestados_lote.py:158`.

**Leitores do campo, nome ou presença:**

| Uso | Referências |
|---|---|
| Limpeza e captura do nome anterior | `app/ponto/services/ausencia.py:167`, :476, :922, :1197; `app/ponto/views.py:2496` |
| Estado, gates e reabertura | `app/ponto/services/ausencia.py:606`, :671, :691, :1190 |
| Chamado e trilha | `app/ponto/services/ausencia.py:713`, :730, :739, :938 |
| API | `app/api/views.py:1061`, :1843 |
| Cartas de troca | `app/ponto/portas/celula.py:702`, :722 |
| Dia e histórico | `app/ponto/services/dia_decidido.py:424`, :497, :609, :623, :641 |
| Contadores e rejuízo | `app/ponto/services/vigia_ausencia.py:115`, :215; `app/ponto/management/commands/rejulgar_status_por_tipo.py:28`, :37 |
| Lote, nome exportado e CSV | `app/ponto/services/atestados_lote.py:76`, :96, :129, :148 |
| Templates | `app/templates/ponto/ausencias.html:168`, :170; `app/templates/ponto/partials/painel_ausencia.html:71`, :72, :93, :205 |

O import externo do helper está em `app/ponto/views.py:2494`; os outros usos identificados dele são internos ao serviço. Há precedente de importar função privada de outro módulo na própria view, como `_is_gestor` e `_rotulo_da_escala`. Isso, isoladamente, é **observação**. (`app/ponto/services/ausencia.py:481`, :927, :1202; `app/ponto/views.py:121`, :2062)

**4. Os testes mordem?** [nao verificado]

**Sim, no fluxo atual:** o teste de falha envia upload, força erro em `gravar` e exige mensagem, ausência desfeita, situação preservada e inventário de arquivos igual. O caminho feliz exige nome, arquivo adicional, existência física e situação afastado; outro teste confere o storage isolado. Resultado executado: **[não verificado]**. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:151`, :158, :166, :168, :169, :171, :179, :182, :183)

**Limite:** o teste de falha, isoladamente, não afirma positivamente que o arquivo já existia quando o mock lançou. Uma antecipação da chamada a `gravar` poderia fazê-lo passar sem escrever upload. Os novos casos também não preparam ausência rejeitada nem provocam falha intermediária de `_reabrir`. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:164`, :166, :176; `app/ponto/services/ausencia.py:1200`, :1211)

A asserção nova de sobreposição elimina a passagem vazia por uma recusa anterior sem a mensagem esperada: exige “já existe”, além da contagem e situação preservadas. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:112`, :116, :117, :118)

O registro `3307` aponta à expressão atual de `regularizar_retorno`, e a catraca congela a localização. A alteração do registro é de linha; a explicação histórica integral do deslocamento `3273 → 3307` é **[não verificado]**. (`app/core/tests/test_medida_turno_do_par.py:339`, :557; `app/ponto/views.py:3307`)

**5. Checklist**

- **Segundo juiz:** a guarda de desligamento reutiliza `_guarda_da_linha` e `esta_desligado`. Como observação fora da cura, o PWA conserva veto próprio de sobreposição antes do produtor, enquanto este aplica `linha_que_veta`. (`app/colaboradores/services/colaborador.py:102`, :62; `app/ponto/views.py:2592`, :2593; `app/ponto/services/ausencia.py:382`)
- **Segundo escritor fora da porta:** os escritores identificados de documento estão no serviço; a escrita de situação da view passa por `gravar`. (`app/ponto/services/ausencia.py:410`, :923, :1198; `app/ponto/views.py:2485`)
- **Relógio fora da borda:** persistem leituras internas para prazo e carimbo; a limpeza nova não acrescenta relógio. (`app/ponto/services/ausencia.py:398`, :408, :1216; `app/ponto/models.py:883`; `app/ponto/views.py:2486`)
- **Derivação paralela:** a view continua derivando `afastado` pelo tipo lançado; existe também a autoridade de cobertura `afastado_hoje` e o reversor que escreve pela porta. É observação preexistente. (`app/ponto/views.py:2461`, :2485; `app/ponto/turnos.py:2737`; `app/ponto/management/commands/reverter_situacao_afastado.py:73`)
- **Ordem de trava:** no INSS, Colaborador é travado antes da Ausencia; `gravar` repete a mesma guarda. A ordem global implementada começa por Colaborador. (`app/ponto/views.py:2467`, :2468; `app/colaboradores/services/colaborador.py:90`, :133; `app/chamados/models.py:2744`, :2753)
- **`documento.name` com dois significados:** no serviço, a comparação usa o nome anterior ao save; na limpeza externa, usa o nome persistido depois do rollback. A distinção protege o documento restaurado, mas depende de a instância ter sido devolvida — justamente a lacuna PROCEDENTE. (`app/ponto/services/ausencia.py:476`, :1197, :169; `app/ponto/views.py:2492`, :2496`)

**Lista final de achados:**

- **PROCEDENTE — 1:** upload salvo por `_reabrir`, seguido de falha antes do retorno, escapa da limpeza externa após rollback. (`app/ponto/services/ausencia.py:1200`, :1211; `app/ponto/views.py:2468`, :2492`)
- **Observações:** cobertura incompleta dos testes para reabertura; limpeza sujeita a falhas operacionais e sem verificação global de referências; demais pontos do checklist descritos acima. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:166`; `app/ponto/services/ausencia.py:169`, :174; `app/ponto/views.py:2496`)

## 16/11 02:38 · sessao ac4dd33f978d · - · - · origem=Ronald (tmux)

O selo passa no fonte atual: o replay encontrou nove escritores e `faltam=[]`; a checagem de `_acorda=False` também devolveu `[]`. Executei em memória os 11 testes estruturais com `unittest.TestCase`, substituindo apenas a infraestrutura Django e lendo `CAMPOS_DO_INSUMO` por AST: zero falhas/erros. Isso verifica os testes AST, sem verificar a execução Django. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:221`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:263`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:274`, `app/ponto/services/ausencia.py:119`)

Os exemplos antigos do parecer são cobertos pelos novos mutantes e passaram nesse replay. Porém, estes escapes novos também passam:

1. **`parametro_vale` esquece o valor recebido pelo parâmetro.**

```python
def _m(tipo_ausencia, x):
    from ponto.models import TipoAusencia
    if x:
        tipo_ausencia = TipoAusencia.objects.get(pk=x)
    tipo_ausencia.save()
```

Com `x=False` e `tipo_ausencia` sendo uma `Ausencia`, executa a escrita sem acordador. O replay não inclui `_m` como escritor. `_de_outro_modelo(..., parametro_vale=True)` considera somente as reatribuições encontradas, todas de `TipoAusencia`, e não considera o caminho que preserva o argumento inicial. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:135`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:138`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:247`)

2. **A correção de fábrica parâmetro falha com alias global.**

```python
from ponto.models import TipoAusencia as TA

def _m(TA):
    t = TA()
    t.save()
```

Chamar `_m(lambda: ausencia)` salva a ausência sem acordar. O censo atribui a construção à entidade canônica `TipoAusencia`; a regra procura esse nome nos parâmetros, mas o parâmetro chama-se `TA`. O `save` posterior é suprimido porque a construção já contou. O replay não inclui `_m` como escritor. (`app/core/censo_escritas.py:285`, `app/core/censo_escritas.py:372`, `app/core/censo_escritas.py:409`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:243`)

3. **O selo novo não examina `_acorda` dentro de `**kwargs`.**

```python
def _m(a):
    mudar_estado(a, 'rejeitada', **{'_acorda': False})
```

O replay retorna `cala_sem_acordador=[]`. A regra exige `k.arg == '_acorda'`; nesse formato, `k.arg` é `None`. A função também não escreve diretamente, portanto o selo de escritores não a cobra. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:213`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:214`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:235`)

4. **A análise de fluxo ainda aceita acordador inexequível.**

```python
def _m(a):
    a.save()
    if True:
        return a
    _acordar_projecao(a.colaborador, {}, {}, motivo='x')
```

O replay enumera `_m`, mas devolve `faltam=[]`. `_vivos` interrompe somente o bloco que contém diretamente `return`; o retorno do ramo não interrompe a visita aos comandos seguintes do bloco externo. `False and _acordar_projecao(...)` também passa, pois não há tratamento do curto-circuito de `BoolOp`. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:161`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:178`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:189`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:191`)

Não encontrei falso positivo no fonte atual. Há um **latente** introduzido pelo descarte indiscriminado de função interna: `a.save(); def _f(): _acordar_projecao(...); _f()` recebe `faltam=['_m']`, embora execute o acordador. O controle `while True: break; else: _acordar_projecao(...)` recebe corretamente `faltam=['_m']`, porque o `else` não executa nesse caminho. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:176`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:185`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:263`)

## 16/11 02:38 · sessao eb3e46a667e9 · - · - · origem=Ronald (tmux)

**O teto fecha o contraexemplo futuro da volta 2, em uma leitura estável da mesma empresa/corte.** Busca a maior competência carimbada por `(ano, mes)` e alcança o fim dela; as janelas são ordenadas cronologicamente, e a linha aberta cobre continuamente de `data` ao teto. (`app/ponto/services/ausencia.py:147`, `app/ponto/services/ausencia.py:151`, `app/ponto/janelas.py:15`, `app/ponto/turnos.py:2803`, `app/ponto/turnos.py:2811`)

- **Mesmo significado de competência:** o escritor importa `janela_fechamento` e calcula `mes/ano` como mês/ano do fim da janela; grava exatamente os parâmetros usados nessa janela. O teto pergunta o fim da janela pelos mesmos `mes/ano`. (`app/ponto/services/fechamento.py:15`, `app/ponto/services/fechamento.py:621`, `app/ponto/services/fechamento.py:898`, `app/ponto/services/ausencia.py:151`, `app/ponto/janelas.py:11`)
- **Mesma empresa:** escritor usa `colab.empresa`, teto usa `colaborador.empresa`, invalidante passa essa empresa para `competencias_do_intervalo`, que usa `janela_atual`. Não há uso da empresa histórica do vínculo ou do posto nesse cálculo de janela. (`app/ponto/services/fechamento.py:619`, `app/ponto/services/fechamento.py:621`, `app/ponto/services/ausencia.py:151`, `app/ponto/services/fechamento.py:1599`, `app/ponto/services/dia_pago.py:716`)
- **Sem competência intermediária indevida:** cada lado manda apenas seu trecho; o invalidante enumera as competências efetivamente tocadas e altera somente registros carimbados. Tipos sem fim que não pertencem a `TIPOS_EM_ABERTO` continuam ocupando apenas `data`, apesar da consulta do teto. (`app/ponto/services/ausencia.py:186`, `app/ponto/services/ausencia.py:192`, `app/ponto/services/ausencia.py:195`, `app/ponto/services/dia_pago.py:715`, `app/ponto/services/fechamento.py:1653`, `app/ponto/turnos.py:2805`)
- **Leitor real da folha:** consulta ausências com a janela daquele colaborador; range aberto entra quando `data <= fim`; a cobertura por dia usa a mesma janela. (`app/ponto/services/fechamento.py:639`, `app/ponto/services/fechamento.py:683`, `app/ponto/turnos.py:2543`, `app/ponto/turnos.py:2546`)
- **Outros escritores:** os carimbos explícitos não nulos encontrados ficam nos dois ramos do escritor canônico. A porta de restauração não altera `mes/ano/colaborador_id` e derruba o carimbo por padrão. (`app/ponto/services/fechamento.py:1143`, `app/ponto/services/fechamento.py:1189`, `app/ponto/services/fechamento.py:1380`, `app/ponto/services/fechamento.py:1391`)

**Ressalva histórica, sem evidência produtiva:** `FechamentoMensal` não guarda empresa/corte da derivação; a transferência pode trocar a empresa atual do colaborador. Portanto não se pode afirmar que a empresa atual seja a mesma usada para produzir um fechamento antigo. (`app/ponto/models.py:405`, `app/ponto/models.py:408`, `app/ponto/models.py:485`, `app/colaboradores/views.py:1389`)

Exemplo condicional dessa ressalva: empresa antiga corte 28, afastamento aberto iniciado em 25/10, fechamento de outubro carimbado; depois transferência para empresa corte 21 e rejeição. Outubro antigo abrangia 28/09–27/10; com a empresa atual outubro acaba em 20/10, e o trecho iniciado em 25/10 só invalida novembro em diante. A incidência e existência desse cadastro/carimbo são **[nao verificado]**. A derivação das datas e do caminho decorre de `app/ponto/janelas.py:15`, `app/ponto/janelas.py:27`, `app/ponto/services/ausencia.py:192`, `app/ponto/services/fechamento.py:1599`; a transferência é permitida em `app/colaboradores/views.py:1389`.

## 16/11 02:37 · sessao 6c4f3b8174ea · - · - · origem=Ronald (tmux)

- **Testes novos mordem o caso declarado por inspeção.** O teste de falha envia upload real, força `gravar` a falhar e exige mensagem, ausência desfeita, situação preservada e inventário de arquivos igual ao anterior (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:151`, `:162`). O caminho feliz exige linha, nome, arquivo adicional, existência física e situação afastado (`:173`). O teste separado confere que o storage usa o diretório isolado (`:158`). Execução e resultado efetivo: **[não verificado]**.

- **A asserção de sobreposição elimina a passagem vazia por recusa anterior ao produtor.** Ela exige o texto produzido pela recusa de sobreposição, além de preservar uma única ausência e situação ativo (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:112`, `:116`; mensagem em `app/ponto/services/ausencia.py:385`). Isoladamente, o teste de rollback do anexo ainda não prova que o upload chegou a ser escrito antes da falha; o caminho feliz verifica a entrega do documento no fluxo normal (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:166`, `:178`).

- **Lacuna relevante: reabertura falhando antes do retorno.** Os novos casos criam colaborador sem ausência anterior e injetam falha somente em `gravar` (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:164`, `:166`, `:176`). Não cobrem `_reabrir` salvar o documento (`app/ponto/services/ausencia.py:1200`) e falhar depois no save da descrição (`:1210`) ou do estado (`:1211`, `:900`). Nesse caso `criar_ausencia` não retorna (`:350`), a variável da view continua `None` (`app/ponto/views.py:2462`, `:2468`) e a limpeza externa é pulada (`:2492`): **PROCEDENTE**, conforme identificado pelo pai.

- **Registro 3273 → 3307:** o registro atual aponta para a mesma ocorrência `regularizar_retorno`, cuja expressão está em `app/ponto/views.py:3307`; a catraca compara somente o inventário de linhas (`app/core/tests/test_medida_turno_do_par.py:339`, `:557`). Contudo, o anexo fornecido mostra saldo de **+14** linhas em views, enquanto o registro muda **+34**; o histórico que explica as outras vinte linhas é **[não verificado]**.

- **Checklist:** a guarda usa o mesmo juiz, sem predicado novo (`app/colaboradores/services/colaborador.py:62`, `:94`; `app/colaboradores/services/desligamento.py:23`). A trava do Colaborador precede `criar_ausencia`, e o `gravar` repete a mesma trava (`app/ponto/views.py:2465`, `:2485`; `app/colaboradores/services/colaborador.py:90`, `:133`); a ordem global está implementada em `app/chamados/models.py:2744`, `:2753`, `:2766`, `:2770`. A escrita de situação passa pela porta, mas a derivação por tipo continua paralela à autoridade de cobertura (`app/ponto/views.py:2461`, `:2485`; `app/ponto/turnos.py:2737`). Relógios internos preexistentes continuam em `app/ponto/services/ausencia.py:398`, `:408`, `:1216` e `app/ponto/models.py:883`; a cura não acrescenta leitura de relógio (`app/ponto/views.py:2462`). Cron instalado e contagens citadas nos comentários: **[não verificado]**.

## 16/11 02:36 · sessao 370f0f9ef572 · - · - · origem=Ronald (tmux)

Censo estático, somente leitura; execução e produção [não verificado].

| Chamador/encadeamento com `atomic` externo | Escrita/falha depois da chamada | Anexo órfão neste ato? |
|---|---|---|
| `regularizar_turno_orfao`: `app/ponto/services/regularizacao.py:89` → `garantir_ausencia`:118 | `ch.analisar`:137, nota:139, auditoria:150 | Não escreve arquivo: chamada omite `documento` em :118–122; padrão `None` em `app/ponto/services/ausencia.py:180`. |
| `marcar_abandono_provisorio.handle`: `app/ponto/management/commands/marcar_abandono_provisorio.py:22` → `garantir_ausencia`:76 | Próximas iterações:70; saída:88; rollback explícito do dry run:98 | Não escreve arquivo: argumentos :76–84 omitem documento; padrão em `app/ponto/services/ausencia.py:180`. |
| `veredito_celula`: `app/ponto/services/veredito_celula.py:224` → `criar_ausencia`:337 | Fechar fio:347, lavrar:352, auditoria:356 | Não escreve arquivo: argumentos :337–339 omitem documento. |
| `_materializar_uma`: `app/chamados/services/materializacao.py:1226` → ramo escolha:1232 → `_materializar_resposta_escolha`:1084 → `_declarar_ausencia_de_pergunta` → `criar_ausencia`:491 | Reconciliar:1085; carimbar:1283; recusa levanta:1285 | Não escreve arquivo: argumentos :491–499 omitem documento. |
| Espelho de férias: `app/ferias/models.py:449` → `_sync_ausencia`:394 → `espelhar_ferias` → `criar_ausencia`: `app/ponto/services/ausencia.py:1275` | Próximos dias:1270, exclusões:1287, pauta:1290 | Não escreve arquivo: argumentos :1275–1280 omitem documento. |

`garantir_ausencia` delega para `criar_ausencia` em `app/ponto/services/ausencia.py:523`; este pode delegar para `_reabrir` em :350. Nos chamadores acima, a reabertura recebe `documento=None`; a escrita de arquivo de `_reabrir` depende de `if documento` em :1191. Logo podem desfazer a reabertura, mas não deixam **upload novo** órfão nesses atos (`app/ponto/services/ausencia.py:350`, :1191–1200).

Os principais `atomic` externos transitivos do espelho de férias são:

- `registrar_gozo_historico`: `app/ferias/services.py:519`; `ag.save`:547; escrita do período depois:551.
- `executar_agendamento`: `app/ferias/services.py:567`; criação:581; escrita do período depois:587.
- `agendar_ferias_com_abono`: `app/ferias/services.py:297`; execução:306; escrita do abono depois:316.
- `_trocar_janela`: `app/ferias/services.py:706`; `ag.save`:729; escrita do período depois:731.
- `aprovar_solicitacao`: `app/ferias/services.py:755` e :774; `ag.save`:786; escrita do período depois:789.
- Correção administrativa: `app/ferias/views.py:524`; correção:531; trilha depois:533.
- `alinhar_status_por_data`: `app/ferias/services.py:951`; save que pode reconstruir ausências faltantes:956; o espelho ainda executa seu laço e exclusões após cada criação (`app/ponto/services/ausencia.py:1270`, :1284–1290).

Fora desse formato:

- API cria com documento em `app/api/views.py:1812–1819`, anexa no reenvio em :1845 e recebe anexo em :3117; esses corpos não abrem `atomic` externo envolvendo essas chamadas.
- PWA cria com documento em `app/ponto/views.py:2603–2609`; anexação administrativa em :2159; reversão em :2651–2652; esses corpos também não abrem `atomic` externo envolvendo as chamadas.
- `_materializar_ausencia_de_resposta` chama `criar_ausencia` **sem documento** em `app/chamados/services/materializacao.py:527–535`. Seu chamador é `app/chamados/services/disputa_emissao.py:1555`, depois de sair do `atomic` :1517–1526. O `atomic` próprio das absorções começa posteriormente, em `app/chamados/services/materializacao.py:563`.
- O copiloto chama sua porta em `app/core/views_ia.py:985`; ela chama `criar_ausencia` sem documento em `app/ponto/services/checar_ausencia.py:46–48`.

A referência a `materializar_pergunta_confirmada`, `_materializar_nota` ou documento-string vindo do chamado não corresponde aos trechos desta cópia: a chamada efetiva de :527–535 não passa documento (`app/chamados/services/materializacao.py:527`). A existência desses outros caminhos nesta versão fica [não verificado].

**Resultado deste censo:** os outros chamadores identificados que efetivamente envolvem a chamada em `atomic` externo omitem o documento; não reproduzem o órfão de upload novo do achado original (`app/ponto/services/regularizacao.py:118`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76`, `app/ponto/services/veredito_celula.py:337`, `app/chamados/services/materializacao.py:491`, `app/ponto/services/ausencia.py:1275`).

## 16/11 02:36 · sessao c7d6f48750be · - · - · origem=Ronald (tmux)

Censo somente leitura:

- **Escritores de `Ausencia.documento`:** `criar_ausencia` passa o arquivo ao modelo e salva em `app/ponto/services/ausencia.py:409`, `:446`, `:479`; `anexar_documento` atribui e salva em `:923`, `:925`; `_reabrir` atribui e salva em `:1198`, `:1200`. O destino é `ausencias/documentos/` (`app/ponto/models.py:777`). Os uploads chegam pelas views web `app/ponto/views.py:2159`, `:2471`, `:2605` e API `app/api/views.py:1814`, `:1845`, `:3117`.
- **Escrita de nome antes do storage:** `normalizar_nome_documento` altera a extensão do upload em `app/ponto/services/ausencia.py:658`. A configuração SaaS usa `FileSystemStorage` (`app/config/settings/saas.py:109`).
- **Deleção física:** `_apagar_anexo_escrito_agora` executa `doc.delete(save=False)` em `app/ponto/services/ausencia.py:172`; chamadas internas em `:481`, `:927`, `:1202`, e externa em `app/ponto/views.py:2495`.
- **Importação privada:** o único import externo encontrado dessa função é `app/ponto/views.py:2494`. Há precedentes de importar funções privadas de outros módulos no mesmo arquivo: `_is_gestor` (`app/ponto/views.py:121`) e `_rotulo_da_escala` (`:2062`). Isso, isoladamente, é observação, não defeito.

**Leitores físicos dos arquivos:**

- Tipo real do anexo: `app/ponto/models.py:857`, `:861`.
- Endpoint que abre/serve o documento, incluindo raiz de `documento_referencia`: `app/ponto/views.py:2122`, `:2125`, `:2129`.
- ZIP abre e lê: `app/ponto/services/atestados_lote.py:157`, `:158`; nome do arquivo exportado em `:96`.

**Leitores do campo/nome/presença:**

- Serviço de ausência: limpeza `app/ponto/services/ausencia.py:167`; captura anterior `:476`, `:922`, `:1197`; rejuízo pelo tipo `:606`; gates `:671`, `:691`; chamado `:713`, `:730`, `:739`; trilha `:938`; reabertura `:1190`.
- API: `app/api/views.py:1061` e `:1843`.
- Cartas de troca: `app/ponto/portas/celula.py:702`, `:722`.
- Apresentação do dia e histórico: `app/ponto/services/dia_decidido.py:424`, `:497`, `:609`, `:623`, `:641`.
- Contadores: `app/ponto/services/vigia_ausencia.py:115`, `:215`.
- Comando: `app/ponto/management/commands/rejulgar_status_por_tipo.py:28`, `:37`.
- Lote/CSV: `app/ponto/services/atestados_lote.py:76`, `:77`, `:129`, `:148`.
- Templates: `app/templates/ponto/ausencias.html:168`; `app/templates/ponto/partials/painel_ausencia.html:71`, `:93`, `:205`.
- Admin registrado como somente leitura: `app/ponto/admin.py:38`; bloqueios de escrita em `app/core/admin.py:59`, `:62`, `:65`; leitura genérica dos campos em `:68`.

**PROCEDENTE — reabertura pode escrever anexo e falhar antes de devolver a instância.** `criar_ausencia` retorna diretamente `_reabrir` (`app/ponto/services/ausencia.py:350`). `_reabrir` salva o documento em `:1200`, mas depois ainda salva descrição em `:1210` e muda estado em `:1211`, cujo `save` ocorre em `:900`. Uma falha nesses passos sai sem retorno. Na view, `ausencia` permanece `None` (`app/ponto/views.py:2462`, `:2468`), e a guarda `:2492` pula a limpeza após o rollback. Portanto o novo arquivo pode ficar órfão enquanto o banco volta ao documento anterior; a limpeza interna cobre somente falha do `save` do próprio documento (`app/ponto/services/ausencia.py:1199`).

**Observação — segurança da limpeza é comparação local de nome.** O helper preserva nome igual ao recebido em `nome_antes` (`app/ponto/services/ausencia.py:169`); a view passa o nome da mesma PK relida após rollback (`app/ponto/views.py:2496`). Não há busca global por outras linhas que referenciem aquele nome no helper (`app/ponto/services/ausencia.py:167`). Existência real de nomes compartilhados em produção: [não verificado].

**Observação — falha de storage deixa arquivo.** A exclusão pode falhar; o helper registra a exceção e retorna `False` (`app/ponto/services/ausencia.py:174`). Ocorrência em produção: [não verificado].

## 16/11 02:32 · sessao 57ff070fbbff · ESPELHO-VERDADE-FECHO-V0 · leitura · origem=Code

Em **16/11**, o consolidado fecha em **306 dias divergentes na 10/2026** e **329 na 11/2026**. O BATE é, respectivamente, **95,920543927476336%** e **94,823788546255507%**. [nao verificado]

As contas abaixo usam exclusivamente os anexos colados, na ordem **P1 + P2 + P3 + P4 + P5 + P6**, preservando os decimais de `PARCIAL` e `PARCIAL_TOTAL`. As referências ao código identificam a cópia consultada; os resultados dos anexos levam `[nao verificado]`. [nao verificado]

Para **10/2026**, cada chave de `[1]` soma: [nao verificado]

| Chave | Conta por parte = total | Referência |
|---|---|---|
| dias_comparados | 1180 + 1268 + 1236 + 1234 + 1285 + 1298 = **7501** | [nao verificado] |
| bate_ate_10min | 1140 + 1227 + 1169 + 1186 + 1215 + 1258 = **7195** | [nao verificado] |
| esp_zero_e6_trabalho | 1 + 2 + 12 + 6 + 9 + 5 = **35** | [nao verificado] |
| e6_zero_esp_trabalho | 3 + 4 + 6 + 1 + 3 + 2 = **19** | [nao verificado] |
| diverge_10_60 | 11 + 7 + 2 + 5 + 3 + 1 = **29** | [nao verificado] |
| diverge_acima_60 | 25 + 28 + 47 + 36 + 55 + 32 = **223** | [nao verificado] |
| dia_batida_impar | 33 + 63 + 65 + 55 + 78 + 54 = **348** | [nao verificado] |
| impar_que_BATE | 15 + 33 + 23 + 21 + 29 + 26 = **147** | [nao verificado] |
| impar_que_DIVERGE | 18 + 30 + 42 + 34 + 49 + 28 = **201** | [nao verificado] |
| impar_dentro_da_faixa | 15 + 24 + 33 + 30 + 42 + 25 = **169** | [nao verificado] |
| impar_FORA_da_faixa | 3 + 6 + 9 + 4 + 7 + 3 = **32** | [nao verificado] |
| impar_no_conjunto_do_dia | 18 + 30 + 42 + 33 + 49 + 28 = **200** | [nao verificado] |
| impar_SO_POR_CORTE_do_oraculo | 0 + 0 + 0 + 1 + 0 + 0 = **1** | [nao verificado] |
| dia_sem_linha_no_espelho | 0 + 0 + 0 + 0 + 0 + 0 = **0** | [nao verificado] |
| dia_sem_lavratura_no_espelho | 1727 + 1563 + 1422 + 1276 + 1533 + 1396 = **8917** | [nao verificado] |
| dia_isento_ou_ausencia | 316 + 353 + 342 + 315 + 275 + 277 = **1878** | [nao verificado] |
| dia_sem_trabalho_ambos | 34 + 24 + 22 + 18 + 45 + 11 = **154** | [nao verificado] |
| esp_desconta_janela_declarada | 43 + 62 + 38 + 37 + 72 + 18 = **270** | [nao verificado] |
| esp_lido_pelo_pago_h | 0 + 0 + 0 + 0 + 0 + 0 = **0** | [nao verificado] |

Para **11/2026**, cada chave de `[1]` soma: [nao verificado]

| Chave | Conta por parte = total | Referência |
|---|---|---|
| dias_comparados | 1008 + 1067 + 1059 + 983 + 1128 + 1111 = **6356** | [nao verificado] |
| bate_ate_10min | 946 + 1011 + 1004 + 951 + 1045 + 1070 = **6027** | [nao verificado] |
| esp_zero_e6_trabalho | 14 + 1 + 8 + 7 + 5 + 2 = **37** | [nao verificado] |
| e6_zero_esp_trabalho | 7 + 3 + 7 + 0 + 4 + 4 = **25** | [nao verificado] |
| diverge_10_60 | 8 + 3 + 2 + 0 + 6 + 2 = **21** | [nao verificado] |
| diverge_acima_60 | 33 + 49 + 38 + 25 + 68 + 33 = **246** | [nao verificado] |
| dia_batida_impar | 49 + 69 + 51 + 50 + 88 + 60 = **367** | [nao verificado] |
| impar_que_BATE | 18 + 20 + 16 + 25 + 33 + 30 = **142** | [nao verificado] |
| impar_que_DIVERGE | 31 + 49 + 35 + 25 + 55 + 30 = **225** | [nao verificado] |
| impar_dentro_da_faixa | 21 + 45 + 28 + 21 + 46 + 25 = **186** | [nao verificado] |
| impar_FORA_da_faixa | 10 + 4 + 7 + 4 + 9 + 5 = **39** | [nao verificado] |
| impar_no_conjunto_do_dia | 31 + 48 + 33 + 24 + 55 + 29 = **220** | [nao verificado] |
| impar_SO_POR_CORTE_do_oraculo | 0 + 1 + 2 + 1 + 0 + 1 = **5** | [nao verificado] |
| dia_sem_linha_no_espelho | 396 + 408 + 396 + 376 + 408 + 396 = **2380** | [nao verificado] |
| dia_sem_lavratura_no_espelho | 1354 + 1404 + 1334 + 1229 + 1292 + 1257 = **7870** | [nao verificado] |
| dia_isento_ou_ausencia | 233 + 201 + 205 + 264 + 214 + 268 = **1385** | [nao verificado] |
| dia_sem_trabalho_ambos | 44 + 28 + 26 + 34 + 40 + 23 = **195** | [nao verificado] |
| esp_desconta_janela_declarada | 34 + 54 + 49 + 28 + 80 + 14 = **259** | [nao verificado] |
| esp_lido_pelo_pago_h | 0 + 0 + 0 + 0 + 0 + 0 = **0** | [nao verificado] |

As contas dos donos de **10/2026**, usando as horas de `PARCIAL`, são: [nao verificado]

| Dono | Medida | Conta por parte = total | Referência |
|---|---|---|---|
| ESTRUTURA | dia-colab | 10 + 5 + 3 + 3 + 3 + 2 = **26** | [nao verificado] |
| ESTRUTURA | horas | 9.116283817777775 + 11.084600277777778 + 15.209666666666665 + 15.566319135833336 + 29.23270916666667 + 8.15 = **88.359579064722224** | [nao verificado] |
| ESTRUTURA | colabs | 3 + 3 + 3 + 3 + 3 + 1 = **16** | [nao verificado] |
| CADASTRO | dia-colab | 10 + 8 + 7 + 23 + 19 + 8 = **75** | [nao verificado] |
| CADASTRO | horas | 38.355213611111104 + 20.397520712222224 + 35.88209174 + 107.60888681222221 + 52.325450786111105 + 40.4628 = **295.031963661666643** | [nao verificado] |
| CADASTRO | colabs | 3 + 6 + 4 + 8 + 8 + 2 = **31** | [nao verificado] |
| BATIDA | dia-colab | 20 + 28 + 57 + 22 + 48 + 30 = **205** | [nao verificado] |
| BATIDA | horas | 110.21011656222223 + 136.0465618775 + 297.66026132194446 + 101.15787729194444 + 197.29829774694443 + 127.75368121722221 = **970.12679601777777** | [nao verificado] |
| BATIDA | colabs | 13 + 18 + 24 + 17 + 26 + 17 = **115** | [nao verificado] |
| TOTAL | dia-colab | 40 + 41 + 67 + 48 + 70 + 40 = **306** | [nao verificado] |
| TOTAL | horas | 157.681613991111109 + 167.528682867500002 + 348.752019728611125 + 224.333083239999986 + 278.856457699722205 + 176.36648121722221 = **1353.518338744166637** | [nao verificado] |
| TOTAL | colabs | 17 + 23 + 29 + 26 + 29 + 20 = **144** | [nao verificado] |

As contas dos donos de **11/2026** são: [nao verificado]

| Dono | Medida | Conta por parte = total | Referência |
|---|---|---|---|
| ESTRUTURA | dia-colab | 10 + 3 + 6 + 1 + 8 + 3 = **31** | [nao verificado] |
| ESTRUTURA | horas | 35.86154078361111 + 23.15 + 17.8227 + 9.770812222222222 + 54.16690254361111 + 10.610499166666667 = **151.382454716111109** | [nao verificado] |
| ESTRUTURA | colabs | 4 + 3 + 5 + 1 + 6 + 2 = **21** | [nao verificado] |
| CADASTRO | dia-colab | 22 + 15 + 9 + 4 + 29 + 6 = **85** | [nao verificado] |
| CADASTRO | horas | 118.50376287222223 + 38.47013154777778 + 32.931705 + 17.068988648888887 + 98.72113534194445 + 52.750124444444445 = **358.445847855277792** | [nao verificado] |
| CADASTRO | colabs | 6 + 6 + 6 + 3 + 10 + 5 = **36** | [nao verificado] |
| BATIDA | dia-colab | 30 + 38 + 40 + 27 + 46 + 32 = **213** | [nao verificado] |
| BATIDA | horas | 174.57531034805555 + 178.25105356722221 + 194.35440025833333 + 160.93444128833335 + 239.22640724611108 + 161.85087350722222 = **1109.19248621527774** | [nao verificado] |
| BATIDA | colabs | 15 + 21 + 21 + 14 + 25 + 21 = **117** | [nao verificado] |
| TOTAL | dia-colab | 62 + 56 + 55 + 32 + 83 + 41 = **329** | [nao verificado] |
| TOTAL | horas | 328.94061400388889 + 239.87118511499999 + 245.10880525833333 + 187.774242159444459 + 392.11444513166664 + 225.211497118333332 = **1619.020788786666641** | [nao verificado] |
| TOTAL | colabs | 20 + 28 + 27 + 16 + 34 + 25 = **150** | [nao verificado] |

`TOTAL colabs` soma as uniões informadas por parte, considerando a disjunção declarada no pedido. [nao verificado]

As conferências agregadas ficam: [nao verificado]

| Conferência | 10/2026 | 11/2026 | Referência |
|---|---|---|---|
| UNIVERSO_PARTE × UNIVERSO | 110 + 109 + 102 + 96 + 107 + 100 = **624**, igual a **624** | 99 + 102 + 99 + 94 + 102 + 99 = **595**, igual a **595** | [nao verificado] |
| Janela | 2026-09-21 a 2026-10-20 | 2026-10-21 a 2026-11-20 | [nao verificado] |
| BATE % | 7195 ÷ 7501 × 100 ≈ **95,920543927476336%** | 6027 ÷ 6356 × 100 ≈ **94,823788546255507%** | [nao verificado] |
| Comparados − BATE | 7501 − 7195 = **306** | 6356 − 6027 = **329** | [nao verificado] |
| Classes divergentes | 35 + 19 + 29 + 223 = **306** | 37 + 25 + 21 + 246 = **329** | [nao verificado] |
| Dias dos donos | 26 + 75 + 205 = **306** | 31 + 85 + 213 = **329** | [nao verificado] |
| Horas dos donos | 88.359579064722224 + 295.031963661666643 + 970.12679601777777 = **1353.518338744166637** | 151.382454716111109 + 358.445847855277792 + 1109.19248621527774 = **1619.020788786666641** | [nao verificado] |

Os diagnósticos por parte abrangem as duas competências. “INCOMPLETO: não” abaixo significa ausência do marcador no texto fornecido. [nao verificado]

| Parte | rc_sombra | INCOMPLETO | Linhas TRATADA_LOCAL | Ocorrências TRATADA_LOCAL somadas | Erros que atravessaram frame | erros_no_espelho 10 / 11 | SOMA True / False | Referência |
|---|---:|---|---:|---:|---|---|---|---|
| P1 | 0 | não, marcador ausente | 27 | **846173** | nao consta na saida | 0 / 0 | 6 / 0 | [nao verificado] |
| P2 | 0 | não, marcador ausente | 27 | **909946** | nao consta na saida | 0 / 0 | 6 / 0 | [nao verificado] |
| P3 | 0 | não, marcador ausente | 28 | **874751** | nao consta na saida | 0 / 0 | 6 / 0 | [nao verificado] |
| P4 | 0 | não, marcador ausente | 27 | **817688** | nao consta na saida | 0 / 0 | 6 / 0 | [nao verificado] |
| P5 | 0 | não, marcador ausente | 26 | **922428** | nao consta na saida | 0 / 0 | 6 / 0 | [nao verificado] |
| P6 | 0 | não, marcador ausente | 26 | **808182** | nao consta na saida | 0 / 0 | 6 / 0 | [nao verificado] |

A abertura completa de `TRATADA_LOCAL`, preservando os endereços e tipos impressos, é: [nao verificado]

| Endereço impresso | Tipo | P1 | P2 | P3 | P4 | P5 | P6 | Referência |
|---|---|---:|---:|---:|---:|---:|---:|---|
| app/escala/alimentacao.py:279 | GeneratorExit | 253166 | 273276 | 257813 | 239769 | 273687 | 237235 | [nao verificado] |
| app/escala/models.py:1282 | GeneratorExit | 98 | 61 | 296 | 637 | 46 | 116 | [nao verificado] |
| app/escala/services/fase_12x36.py:114 | GeneratorExit | 1130 | 1558 | 1707 | 1276 | 1924 | 1375 | [nao verificado] |
| app/escala/services/leitor_celula.py:25 | ValueError | 6304 | 6837 | 5857 | 6019 | 7806 | 4633 | [nao verificado] |
| app/escala/services/leitor_celula.py:81 | GeneratorExit | 52 | 61 | 41 | 50 | 80 | 58 | [nao verificado] |
| app/escala/utils.py:1500 | GeneratorExit | 2 | 4 | 4 | 1 | 2 | 2 | [nao verificado] |
| app/escala/utils.py:1687 | GeneratorExit | nao consta na saida | 6 | 2 | 36 | 14 | 10 | [nao verificado] |
| app/escala/utils.py:1991 | GeneratorExit | 6216 | 6781 | 6880 | 6962 | 7884 | 6500 | [nao verificado] |
| app/escala/utils.py:2033 | GeneratorExit | 3 | 2 | 4 | 8 | nao consta na saida | nao consta na saida | [nao verificado] |
| app/escala/utils.py:452 | GeneratorExit | 4 | 6 | 6 | 3 | 6 | 4 | [nao verificado] |
| app/escala/utils.py:524 | GeneratorExit | 3677 | 4986 | 4644 | 3790 | 5649 | 3762 | [nao verificado] |
| app/escala/utils.py:525 | GeneratorExit | 3677 | 4986 | 4644 | 3790 | 5649 | 3762 | [nao verificado] |
| app/escala/utils.py:888 | GeneratorExit | 594 | 593 | 602 | 334 | 340 | 602 | [nao verificado] |
| app/folha/causas.py:58 | GeneratorExit | 181 | 187 | 182 | 171 | 192 | 180 | [nao verificado] |
| app/ponto/juiz_batida.py:191 | GeneratorExit | nao consta na saida | nao consta na saida | 17 | 1226 | nao consta na saida | nao consta na saida | [nao verificado] |
| app/ponto/juiz_batida.py:75 | ValueError | 468158 | 495937 | 482764 | 445946 | 498705 | 448020 | [nao verificado] |
| app/ponto/motor_calculo_v2.py:1131 | GeneratorExit | 610 | 618 | 595 | 649 | 855 | 559 | [nao verificado] |
| app/ponto/motor_calculo_v2.py:2604 | GeneratorExit | 2829 | 3000 | 3064 | 2833 | 3248 | 2890 | [nao verificado] |
| app/ponto/motor_calculo_v2.py:677 | GeneratorExit | 172 | 219 | 174 | 186 | 215 | 180 | [nao verificado] |
| app/ponto/services/dia_pago.py:892 | GeneratorExit | 12196 | 13372 | 12978 | 12772 | 14074 | 12704 | [nao verificado] |
| app/ponto/services/espelho.py:473 | GeneratorExit | 130 | 108 | 117 | 113 | 174 | 94 | [nao verificado] |
| app/ponto/services/espelho.py:657 | GeneratorExit | 4055 | 3977 | 4228 | 4030 | 4284 | 4254 | [nao verificado] |
| app/ponto/services/fechamento.py:1044 | GeneratorExit | 192 | 200 | 192 | 180 | 198 | 189 | [nao verificado] |
| app/ponto/services/fechamento.py:1045 | _GradeSemCelula | 5 | 2 | 4 | nao consta na saida | nao consta na saida | 4 | [nao verificado] |
| app/ponto/turnos.py:1295 | GeneratorExit | 23417 | 26387 | 24546 | 24689 | 28387 | 21328 | [nao verificado] |
| app/ponto/turnos.py:1656 | GeneratorExit | 4604 | 5880 | 4783 | 3726 | 3745 | 3840 | [nao verificado] |
| app/ponto/turnos.py:1699 | GeneratorExit | 37849 | 41086 | 40289 | 39600 | 44131 | 38829 | [nao verificado] |
| app/ponto/turnos.py:2653 | GeneratorExit | 16 | nao consta na saida | nao consta na saida | nao consta na saida | 2 | nao consta na saida | [nao verificado] |
| app/ponto/turnos.py:679 | GeneratorExit | 16836 | 19816 | 18318 | 18892 | 21131 | 17052 | [nao verificado] |

Somando somente as linhas exibidas: **27 + 27 + 28 + 27 + 26 + 26 = 161 linhas**; **846173 + 909946 + 874751 + 817688 + 922428 + 808182 = 5179168 ocorrências TRATADA_LOCAL**. [nao verificado]

As linhas `SOMA` atuais ficam transcritas abaixo: [nao verificado]

| Parte | Competência | SOMA dias | SOMA HORAS | SOMA COLABS_UNIAO | Referência |
|---|---|---|---|---|---|
| P1 | 10/2026 | 40 — True | 157.681613991111109 — True | 17 — True | [nao verificado] |
| P1 | 11/2026 | 62 — True | 328.94061400388889 — True | 20 — True | [nao verificado] |
| P2 | 10/2026 | 41 — True | 167.528682867500002 — True | 23 — True | [nao verificado] |
| P2 | 11/2026 | 56 — True | 239.87118511499999 — True | 28 — True | [nao verificado] |
| P3 | 10/2026 | 67 — True | 348.752019728611125 — True | 29 — True | [nao verificado] |
| P3 | 11/2026 | 55 — True | 245.10880525833333 — True | 27 — True | [nao verificado] |
| P4 | 10/2026 | 48 — True | 224.333083239999986 — True | 26 — True | [nao verificado] |
| P4 | 11/2026 | 32 — True | 187.774242159444459 — True | 16 — True | [nao verificado] |
| P5 | 10/2026 | 70 — True | 278.856457699722205 — True | 29 — True | [nao verificado] |
| P5 | 11/2026 | 83 — True | 392.11444513166664 — True | 34 — True | [nao verificado] |
| P6 | 10/2026 | 40 — True | 176.36648121722221 — True | 20 — True | [nao verificado] |
| P6 | 11/2026 | 41 — True | 225.211497118333332 — True | 25 — True | [nao verificado] |

São **36 SOMA True e 0 SOMA False**; por competência, **18 True e 0 False**. As conferências `ESTRUTURA_DIAS_LINHAS` também exibem `True` nas seis partes: na 10, **10 + 5 + 3 + 3 + 3 + 2 = 26**; na 11, **10 + 3 + 6 + 1 + 8 + 3 = 31**. [nao verificado]

As linhas de ESTRUTURA vêm agregadas a seguir. **Horas por dia: nao consta na saida**, em todas elas. Campos literalmente chamados `causa` ou `rotulo`: **nao consta na saida**; estão preservados os campos existentes `classe`, `paridade` e `veredito`. [nao verificado]

O código acumula `abs(df)` antes de arredondar os minutos da linha individual; por isso os minutos impressos não fornecem as horas exatas de cada dia. `app/ponto/management/commands/e6_oraculo.py:380`, `app/ponto/management/commands/e6_oraculo.py:401`

Para **10/2026**, todas as 26 linhas são: [nao verificado]

| Parte | Colab | Dia | esp_min | e6_min | teto_min | dif_min | classe | paridade | batidas | veredito | Referência |
|---|---|---|---:|---:|---:|---:|---|---|---|---|---|
| P1 | col252 | 2026-09-23 | 548 | 594 | 608 | 46 | diverge_10_60 | par | 06:56 12:35 12:49 17:04 | trabalhou | [nao verificado] |
| P1 | col252 | 2026-09-24 | 552 | 598 | 611 | 46 | diverge_10_60 | par | 06:51 12:37 12:51 17:03 | trabalhou | [nao verificado] |
| P1 | col252 | 2026-09-25 | 556 | 604 | 616 | 48 | diverge_10_60 | par | 06:51 12:37 12:50 17:07 | trabalhou | [nao verificado] |
| P1 | col252 | 2026-09-28 | 535 | 581 | 595 | 46 | diverge_10_60 | par | 07:12 12:38 12:53 17:07 | trabalhou | [nao verificado] |
| P1 | col252 | 2026-10-01 | 545 | 590 | 605 | 45 | diverge_10_60 | par | 07:00 12:37 12:51 17:05 | trabalhou | [nao verificado] |
| P1 | col252 | 2026-10-02 | 545 | 593 | 606 | 48 | diverge_10_60 | par | 06:59 12:40 12:53 17:04 | trabalhou | [nao verificado] |
| P1 | col252 | 2026-10-06 | 551 | 597 | 611 | 46 | diverge_10_60 | par | 06:58 12:38 12:52 17:09 | trabalhou | [nao verificado] |
| P1 | col252 | 2026-10-09 | 544 | 592 | 604 | 48 | diverge_10_60 | par | 07:06 12:48 13:00 17:10 | trabalhou | [nao verificado] |
| P1 | col864 | 2026-10-12 | 509 | 449 | 509 | -60 | diverge_10_60 | par | 15:01 18:10 19:10 23:30 | feriado | [nao verificado] |
| P1 | col876 | 2026-09-21 | 550 | 665 | 722 | 115 | diverge_acima_60 | par | 18:29 04:39 05:36 06:31 | em_aberto | [nao verificado] |
| P2 | col37 | 2026-10-05 | 241 | 0 | 0 | -241 | e6_zero_esp_trabalho | par | vazio | em_aberto | [nao verificado] |
| P2 | col37 | 2026-10-09 | 241 | 0 | 0 | -241 | e6_zero_esp_trabalho | par | vazio | em_aberto | [nao verificado] |
| P2 | col451 | 2026-09-23 | 707 | 718 | 718 | 11 | diverge_10_60 | par | 07:00 18:58 | em_aberto | [nao verificado] |
| P2 | col451 | 2026-09-25 | 705 | 718 | 718 | 13 | diverge_10_60 | par | 07:02 19:00 | em_aberto | [nao verificado] |
| P2 | col841 | 2026-10-20 | 681 | 522 | 701 | -159 | diverge_acima_60 | par | 11:00 18:46 21:44 22:41 | em_aberto | [nao verificado] |
| P3 | col218 | 2026-10-15 | 105 | 0 | 0 | -105 | e6_zero_esp_trabalho | par | vazio | em_aberto | [nao verificado] |
| P3 | col746 | 2026-09-24 | 289 | 193 | 350 | -96 | diverge_acima_60 | par | 11:00 13:02 15:39 16:49 | trabalhou | [nao verificado] |
| P3 | col854 | 2026-10-04 | 718 | 7 | 7 | -711 | diverge_acima_60 | par | 18:54 19:01 | trabalhou | [nao verificado] |
| P4 | col129 | 2026-10-12 | 0 | 779 | 779 | 779 | esp_zero_e6_trabalho | par | 17:30 06:30 | em_aberto | [nao verificado] |
| P4 | col369 | 2026-10-12 | 476 | 416 | 476 | -60 | diverge_10_60 | par | 07:04 12:47 13:46 15:00 | feriado | [nao verificado] |
| P4 | col723 | 2026-10-08 | 440 | 345 | 500 | -95 | diverge_acima_60 | par | 09:00 10:44 13:20 17:20 | trabalhou | [nao verificado] |
| P5 | col382 | 2026-09-30 | 364 | 0 | 0 | -364 | e6_zero_esp_trabalho | par | vazio | trabalhou | [nao verificado] |
| P5 | col478 | 2026-09-25 | 719 | 8 | 8 | -711 | diverge_acima_60 | par | 18:57 19:04 | trabalhou | [nao verificado] |
| P5 | col868 | 2026-09-22 | 0 | 679 | 733 | 679 | esp_zero_e6_trabalho | par | 18:48 02:02 02:57 07:01 | em_aberto | [nao verificado] |
| P6 | col515 | 2026-10-09 | 244 | 0 | 0 | -244 | e6_zero_esp_trabalho | par | vazio | em_aberto | [nao verificado] |
| P6 | col515 | 2026-10-11 | 245 | 0 | 0 | -245 | e6_zero_esp_trabalho | par | vazio | em_aberto | [nao verificado] |

Para **11/2026**, todas as 31 linhas são: [nao verificado]

| Parte | Colab | Dia | esp_min | e6_min | teto_min | dif_min | classe | paridade | batidas | veredito | Referência |
|---|---|---|---:|---:|---:|---:|---|---|---|---|---|
| P1 | col114 | 2026-11-08 | 439 | 429 | 499 | -10 | diverge_10_60 | par | 09:59 14:50 16:00 18:18 | trabalhou | [nao verificado] |
| P1 | col114 | 2026-11-15 | 499 | 440 | 499 | -59 | diverge_10_60 | par | 09:58 15:05 16:05 18:17 | feriado | [nao verificado] |
| P1 | col252 | 2026-11-09 | 551 | 597 | 610 | 46 | diverge_10_60 | par | 06:53 12:38 12:52 17:04 | trabalhou | [nao verificado] |
| P1 | col252 | 2026-11-10 | 551 | 598 | 611 | 47 | diverge_10_60 | par | 06:56 12:39 12:52 17:07 | trabalhou | [nao verificado] |
| P1 | col252 | 2026-11-11 | 550 | 598 | 611 | 48 | diverge_10_60 | par | 06:54 12:39 12:52 17:04 | trabalhou | [nao verificado] |
| P1 | col252 | 2026-11-13 | 551 | 597 | 611 | 46 | diverge_10_60 | par | 06:52 12:35 12:49 17:03 | trabalhou | [nao verificado] |
| P1 | col516 | 2026-10-23 | 361 | 0 | 0 | -361 | e6_zero_esp_trabalho | par | vazio | folga | [nao verificado] |
| P1 | col516 | 2026-10-28 | 372 | 0 | 0 | -372 | e6_zero_esp_trabalho | par | vazio | trabalhou | [nao verificado] |
| P1 | col996 | 2026-10-28 | 0 | 580 | 580 | 580 | esp_zero_e6_trabalho | par | 21:18 06:57 | folga | [nao verificado] |
| P1 | col996 | 2026-11-10 | 443 | 1026 | 1116 | 583 | diverge_acima_60 | par | 06:57 13:00 14:30 15:50 21:16 06:59 | em_aberto | [nao verificado] |
| P2 | col37 | 2026-11-14 | 241 | 0 | 0 | -241 | e6_zero_esp_trabalho | par | vazio | trabalhou | [nao verificado] |
| P

[truncado em 20000 de 28677 caracteres]

## 16/11 02:28 · sessao b7d26c2d3ecb · LOTE7-C6 · verificador · origem=Code

**O delta fecha os dois contraexemplos funcionais originais, mas ainda deixa uma transição gravada sem acordar em caso de falha intermediária e mantém escapes no selo.** (`app/ponto/services/ausencia.py:1060`, `app/ponto/services/ausencia.py:1076`, `app/ponto/services/ausencia.py:1094`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:135`)

1. **Fechamento dos achados 1–6 da volta 1**

   | Achado | O delta fecha? |
   |---|---|
   | **1 — aprovação com período invalidava duas vezes** | **Fecha o caso original:** captura antes da transição, suprime o acordador interno e acorda no fim. Contudo, introduz a janela de exceção descrita no item 3 abaixo. (`app/ponto/services/ausencia.py:1059`, `app/ponto/services/ausencia.py:1060`, `app/ponto/services/ausencia.py:1094`) |
   | **2 — competência intermediária indevida** | **Fecha setembro→novembro:** cada lado fornece seu trecho, sem construir o envelope entre eles. Permanece a limitação para competências futuras de linha aberta, descrita abaixo. (`app/ponto/services/ausencia.py:171`, `app/ponto/services/ausencia.py:177`, `app/ponto/services/ausencia.py:180`) |
   | **3 — escapes do selo** | **Parcial:** os exemplos literais antigos ganharam tratamento e mutantes, mas há variantes aceitas sem acordador executável. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:102`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:135`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:247`) |
   | **4 — julgamento substituído por mock** | **Fecha essa substituição:** agora chama o juiz real com células existentes. A asserção ainda não prova o resultado do julgamento. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:408`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:413`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:418`) |
   | **5 — provas menores que os nomes** | **Parcial:** verifica Justificativa pendente, saldo e correção de concluída; ainda não compara observações, trilhas anteriores e conjunto completo das ausências. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:392`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:499`) |
   | **6 — relógio interno/solto** | **Parcial:** congela a classe de férias e evita ler o relógio para snapshots iguais; continua lendo `localdate()` internamente quando um lado alterado tem fim nulo. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:433`, `app/ponto/services/ausencia.py:169`, `app/ponto/services/ausencia.py:175`) |

2. **Cobertura de `_acordar_projecao`**

   Entrada e saída estão cobertas porque percorre a união das PKs e considera cada lado existente; mudança de range considera ambos; mudança apenas de tipo/minutos/horas também entra porque compara a tupla inteira antes de extrair as datas. Trechos idênticos são deduplicados. (`app/ponto/services/ausencia.py:119`, `app/ponto/services/ausencia.py:139`, `app/ponto/services/ausencia.py:168`, `app/ponto/services/ausencia.py:178`)

   Para ranges válidos fechados e dias únicos, não encontrei competência omitida nem competência intermediária acrescentada pelo novo algoritmo: `dias_da_ausencia` produz dias contíguos inclusivos, e cada intervalo enviado corresponde a um dos lados. (`app/ponto/turnos.py:2798`, `app/ponto/turnos.py:2803`, `app/ponto/turnos.py:2811`, `app/ponto/services/ausencia.py:180`)

   **Existe omissão condicional para linha aberta:** empresa com corte 21; hoje 05/11/2026; afastamento aprovado desde 15/10, sem fim; fechamento de dezembro já carimbado; chamada `decidir(a, 'rejeitada')`. O leitor da folha inclui essa ausência em dezembro, mas o acordador expande o lado antigo somente até 05/11 e não invalida dezembro. O código permite pedir recalculação de outro mês e gravar seu carimbo; portanto “competência futura não tem projeção” não está garantido por essa porta. A existência desse cadastro/fechamento em produção é **[nao verificado]**. (`app/ponto/turnos.py:2543`, `app/ponto/turnos.py:2546`, `app/ponto/services/ausencia.py:177`, `app/ponto/views.py:1240`, `app/ponto/views.py:1255`, `app/ponto/services/fechamento.py:898`, `app/ponto/services/fechamento.py:1143`)

   A deduplicação é por **trecho**, não por competência: mover 03/11→04/11 chama o invalidante duas vezes para a mesma competência. Sem recálculo intercalado, a segunda chamada atualiza zero porque exige `derivado_em` preenchido; portanto o retorno não duplica o fechamento, embora haja duas chamadas. (`app/ponto/services/ausencia.py:178`, `app/ponto/services/ausencia.py:180`, `app/ponto/services/fechamento.py:1653`)

3. **`decidir` com `_acorda=False`**

   **Há caminho sem acordar:** transição gravada no primeiro `save`; segundo `save` dos carimbos falha; acordador final não é alcançado. `decidir` não envolve a composição em transação própria nem recupera essa exceção. A aprovação em lote fornece `decisor`, obrigando a segunda escrita, e não abre `atomic` próprio. Em autocommit, uma falha nessa segunda escrita deixa a aprovação persistida e o fechamento carimbado; ocorrência produtiva é **[nao verificado]**. (`app/ponto/services/ausencia.py:972`, `app/ponto/services/ausencia.py:1060`, `app/ponto/services/ausencia.py:1062`, `app/ponto/services/ausencia.py:1076`, `app/ponto/services/ausencia.py:1094`, `app/ponto/views.py:2292`, `app/ponto/views.py:2302`)

   A aprovação individual mostrada protege essa composição com `atomic`, deixando a exceção sair do bloco antes do tratamento externo. (`app/ponto/views.py:2674`, `app/ponto/views.py:2676`, `app/ponto/views.py:2694`)

   Período todo `None` apenas pula sua aplicação; continua até o acordador. O retorno de `mudar_estado` por estado já igual também devolve o controle a `decidir`, que prossegue. Período inválido e transição inválida são recusados antes da escrita da transição. (`app/ponto/services/ausencia.py:954`, `app/ponto/services/ausencia.py:956`, `app/ponto/services/ausencia.py:1051`, `app/ponto/services/ausencia.py:1053`, `app/ponto/services/ausencia.py:1094`)

   Os chamadores produtivos encontrados são:

   | Chamador | kwargs |
   |---|---|
   | `rejulgar_status_pelo_tipo` | `por`, `request`, `motivo`; default `_acorda=True`. (`app/ponto/services/ausencia.py:677`, `app/ponto/services/ausencia.py:940`) |
   | `anexar_documento` | `por`, `motivo`, `request`; default `True`. (`app/ponto/services/ausencia.py:1018`, `app/ponto/services/ausencia.py:940`) |
   | `decidir` | `por`, `motivo`, `request`, **`_acorda=False`**. (`app/ponto/services/ausencia.py:1060`) |
   | `escalonar_aguardando_documento` | somente `motivo`; default `True`. (`app/ponto/services/ausencia.py:1151`, `app/ponto/services/ausencia.py:940`) |
   | `_reabrir` | `por`, `request`, `motivo`; default `True`. (`app/ponto/services/ausencia.py:1291`, `app/ponto/services/ausencia.py:940`) |

   As chamadas diretas restantes encontradas são testes, sem `_acorda`: usam `por`, e uma usa também `motivo`. (`app/ponto/tests/test_ausencia_tipo_rejulga.py:56`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:224`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:225`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:185`, `app/ponto/tests/test_varredura_classe_b_ausencia.py:115`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:177`)

4. **Selo: escapes, falso positivo e mutantes**

   Ainda passam estes escritores sintéticos acrescentados à porta:

   - **`_so_dict`:** `a = {}` seguido de `match items: case [a]: a.save(update_fields=["status"])`. Com `items=[ausencia]`, grava Ausencia; capturas de `match` não entram em `_atribuicoes`, então a isenção continua vendo apenas `{}`. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:77`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:102`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:171`)
   - **`_de_outro_modelo`:** `def _m(TipoAusencia): t = TipoAusencia(); t.save()`. Chamar com `lambda: ausencia` grava Ausencia; a regra confia no nome lexical da fábrica e só verifica se **o receptor `t`** é parâmetro. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:107`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:120`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:179`)
   - **Atribuição por ramos:** `if x: t=TipoAusencia.objects.get(...); else: t=a; t.save()`. O censo fixa a primeira origem reconhecida; a isenção `ent != Ausencia` descarta a escrita antes da nova checagem de todas as atribuições. (`app/core/censo_escritas.py:339`, `app/core/censo_escritas.py:347`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:166`)
   - **Fluxo:** acordador em `if not True`, depois de `return`, ou dentro de função interna nunca chamada ainda satisfaz o selo. Métodos de classes aninhadas também ficam fora da enumeração. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:64`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:131`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:145`)

   Não encontrei falso positivo no fonte atual: o replay AST reconheceu nove escritores e nenhum faltante. Há falso positivo latente em `while False: pass; else: _acordar_projecao(...)`: o analisador descarta inclusive o `else`, que executaria. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:141`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:193`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:205`)

   Os mutantes fornecidos mordem pela razão estrutural esperada: exigem exatamente o escritor acrescentado ou aquele cujo acordador foi retirado; os controles verificam isenções. Não cobrem os escapes acima nem provam execução do acordador. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:233`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:263`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:269`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:282`)

5. **Renomeação de `gravar_tipo_ausencia`**

   Não encontrei chamador em `app/` usando o antigo `tipo=`. A edição produtiva passa o objeto posicionalmente; criação usa `codigo`/`campos`; os testes encontrados seguem essas formas. A renomeação não quebra esses chamadores. (`app/ponto/views_tipos_ausencia.py:86`, `app/ponto/views_tipos_ausencia.py:94`, `app/ponto/tests/test_porta_ausencia_ferias.py:149`, `app/ponto/tests/test_porta_ausencia_ferias.py:150`, `app/ponto/tests/test_porta_ausencia_ferias.py:151`, `app/ponto/services/ausencia.py:1732`)

6. **Testes novos e regressões que ainda passariam** [nao verificado]

   - **Mordida correta dos dois casos centrais:** aprovação com período conta chamadas da invalidação real via `wraps`; setembro→novembro exige outubro ainda carimbado. Os testes H12 de competências adjacentes, isoladamente, passariam com o envelope antigo. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:91`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:338`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:353`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:275`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:292`)
   - **Julgamento real, resultado insuficientemente exigido:** o espião chama a função real, mas aceita qualquer retorno não nulo. Retirar `julgar_colab` e conservar `return out` continuaria satisfazendo essa asserção; não há conferência do veredito/carimbo final das células. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:413`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:418`, `app/ponto/services/cartorio.py:2039`, `app/ponto/services/cartorio.py:2040`, `app/ponto/services/cartorio.py:2054`)
   - **Relógio dos testes congelado; regressão da leitura antecipada não detectada:** `_Base`, férias de tela e H12 têm congelamento. Restaurar `localdate()` antes da comparação dos snapshots passaria pelas asserções atuais de idempotência, que observam chamadas/carimbo, não leitura do relógio. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:59`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:319`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:333`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:433`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:36`)
   - **“Nada mudou” ainda admite alteração lateral:** preservar saldo, impedir `cancelado` e não deixar ausências rejeitadas basta para o helper passar mesmo com observações ou trilhas anteriores alteradas. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`)

Execução Django, RED contra a base e incidência dos contraexemplos na produção: **[nao verificado]**.

## 16/11 02:28 · sessao 5df2cbed0eec · - · - · origem=Ronald (tmux)

P1 a P4 conferidos independentemente: **846173; 909946; 874751; 817688** ocorrências. [nao verificado]

A soma dos seis totais informados pelo agente principal confere: **5179168**; a soma das linhas confere: **161**. A transcrição independente de P5/P6 ficou inconclusiva, portanto não fornece segunda confirmação desses dois totais. [nao verificado]

## 16/11 02:26 · sessao d8400d8ce927 · - · - · origem=Ronald (tmux)

O selo fecha os contraexemplos literais da volta 1, mas continua sendo prova de presença sintática, sem garantir que toda escrita execute o acordador. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:61`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:135`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:182`)

Reproduzi em memória os oito testes do selo, com `unittest.TestCase` no lugar de `SimpleTestCase` e `_monitorados` lendo a constante por AST: oito passaram; o fonte atual apresentou nove escritores, sem faltantes. Isso verifica a análise AST; execução Django permanece **[nao verificado]**. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:193`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:200`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:227`)

Escapes novos confirmados:

- **`_so_dict` ignora capturas de `match`.** O trecho abaixo é aceito sem escritor, embora `_m([ausencia])` grave `Ausencia`: `_atribuicoes` não registra `MatchAs`, e `_so_dict` enxerga somente o literal `{}`. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:77`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:102`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:171`)

  ```python
  def _m(items):
      a = {}
      match items:
          case [a]:
              a.save(update_fields=["status"])
  ```

- **`_de_outro_modelo` confia no nome lexical da fábrica.** O trecho abaixo também passa; `_m(lambda: ausencia)` grava a ausência. A regra verifica se `t` é parâmetro, mas não se `TipoAusencia` é parâmetro ou foi redefinido. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:107`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:120`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:179`)

  ```python
  def _m(TipoAusencia):
      t = TipoAusencia()
      t.save()
  ```

- **A checagem de ramo morto ainda aceita `if not True`, acordador depois de `return` e acordador dentro de função interna nunca chamada.** Ela só interpreta constantes diretamente e percorre os demais descendentes indiscriminadamente. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:131`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:138`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:145`)

- **Classes aninhadas continuam fora da lista de funções.** `_funcoes` inclui somente métodos diretamente pertencentes a classes do módulo. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:64`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:68`)

- **A isenção anterior ao novo helper ainda permite atribuição mista por ramo.** Em `if x: t = TipoAusencia.objects.get(...); else: t = a; t.save()`, o censo fixa a primeira origem, e o selo descarta `ent != Ausencia` antes de consultar `_de_outro_modelo`. Replay aceitou o trecho. (`app/core/censo_escritas.py:339`, `app/core/censo_escritas.py:347`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:166`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:179`)

Não encontrei falso positivo no código atual: o teste de isentos passou no replay e nenhum escritor ficou faltante. Existe falso positivo latente em `while False: pass; else: _acordar_projecao(...)`: o `else` executa, mas o analisador retorna `False` para o `while` inteiro. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:141`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:205`)

Os mutantes existentes mordem pela escrita acrescentada ou pela remoção do acordador: exigem precisamente `['_m']`, `['_K.m']` ou `[nome]`, e os controles não mordem. Eles não cobrem os escapes acima nem demonstram execução do acordador. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:233`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:263`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:269`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:276`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:282`)

## 16/11 02:26 · sessao 8f0b10d0c8bd · - · - · origem=Ronald (tmux)

A pergunta 3 tem um novo caminho sem acordar: `decidir` suprime o acordador da transição, grava os carimbos numa segunda escrita e só depois invalida; não possui `atomic`, `try/finally` nem recuperação entre essas operações. Se a segunda escrita falhar, a transição já ocorreu, mas o acordador final não é alcançado. (`app/ponto/services/ausencia.py:1041`, `app/ponto/services/ausencia.py:1060`, `app/ponto/services/ausencia.py:1076`, `app/ponto/services/ausencia.py:1094`, `app/ponto/services/ausencia.py:972`)

Há chamador produtivo exposto a essa composição: `aprovar_ausencia_lote` chama `decidir` via `_a4_decidir`, fornece `decisor` e não abre transação própria; esse argumento exige o segundo `save`. Incidência de falha real em produção: `[nao verificado]`. (`app/ponto/views.py:2278`, `app/ponto/views.py:2292`, `app/ponto/views.py:2302`, `app/ponto/views.py:85`, `app/ponto/services/ausencia.py:1062`, `app/ponto/services/ausencia.py:1075`)

A aprovação individual de irmãos protege essa janela com `atomic`: a exceção atravessa o bloco e chega ao tratamento exterior. (`app/ponto/views.py:2673`, `app/ponto/views.py:2676`, `app/ponto/views.py:2694`)

`hora_ini=hora_fim=None` não causa retorno antecipado: apenas pula validação/aplicação de período; o acordador final continua obrigatório. O retorno “estado já igual” de `mudar_estado` também retorna ao corpo de `decidir`, que prossegue até o acordador. Transição inválida e período inválido são recusados antes da gravação da transição. (`app/ponto/services/ausencia.py:1051`, `app/ponto/services/ausencia.py:1073`, `app/ponto/services/ausencia.py:1094`, `app/ponto/services/ausencia.py:954`, `app/ponto/services/ausencia.py:956`, `app/ponto/services/ausencia.py:1053`)

O log normal de `mudar_estado` não fornece, por si, a exceção intermediária: `registrar_log` captura `Exception`. (`app/ponto/services/ausencia.py:979`, `app/core/auditoria.py:17`, `app/core/auditoria.py:25`)

Chamadores produtivos de `mudar_estado`, por grep e AST de todos os `.py` em `app/`:

| Chamador | kwargs |
|---|---|
| `rejulgar_status_pelo_tipo` | `por`, `request`, `motivo`; `_acorda` usa default `True`. (`app/ponto/services/ausencia.py:677`, `app/ponto/services/ausencia.py:940`) |
| `anexar_documento` | `por`, `motivo`, `request`; default `True`. (`app/ponto/services/ausencia.py:1018`, `app/ponto/services/ausencia.py:940`) |
| `decidir` | `por`, `motivo`, `request`, **`_acorda=False`**. (`app/ponto/services/ausencia.py:1060`) |
| `escalonar_aguardando_documento` | somente `motivo`; default `True`. (`app/ponto/services/ausencia.py:1151`, `app/ponto/services/ausencia.py:940`) |
| `_reabrir` | `por`, `request`, `motivo`; default `True`. (`app/ponto/services/ausencia.py:1291`, `app/ponto/services/ausencia.py:940`) |

As chamadas diretas restantes encontradas são testes, todos sem `_acorda`: `por` em `test_ausencia_tipo_rejulga.py:56`, `test_h12h06b_rederivar_minutos.py:224`/`:225`, `test_lote7_fato_de_ausencia_acorda_projecao.py:185`, `test_varredura_classe_b_ausencia.py:115`; `por,motivo` em `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:177`. (`app/ponto/tests/test_ausencia_tipo_rejulga.py:56`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:224`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:225`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:185`, `app/ponto/tests/test_varredura_classe_b_ausencia.py:115`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:177`)

Na pergunta 5, **não encontrei quebra por keyword no código de `app/`**: a edição produtiva passa o objeto posicionalmente; a criação passa apenas `codigo`/`campos`. Os testes seguem as mesmas formas; nenhum chamador encontrado usa o antigo `tipo=`. (`app/ponto/views_tipos_ausencia.py:86`, `app/ponto/views_tipos_ausencia.py:94`, `app/ponto/tests/test_porta_ausencia_ferias.py:149`, `app/ponto/tests/test_porta_ausencia_ferias.py:150`, `app/ponto/tests/test_porta_ausencia_ferias.py:151`, `app/ponto/services/ausencia.py:1732`)

Não executei Django nem banco; a verificação acima foi leitura e censo AST em memória, sem gravar arquivos. `[nao verificado]` [nao verificado]

## 16/11 02:26 · sessao 98fc73d71901 · - · - · origem=Ronald (tmux)

- **Achado 4: fecha o defeito específico do mock.** O teste cria cinco células, chama o `julgar_celula` real dentro do espião e exige retorno diferente de `None` nos cinco dias; trocar os `save()` por `update()` silencioso volta a retirar essas chamadas. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:405`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:413`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:418`, `app/ponto/signals.py:143`, `app/ponto/services/ausencia.py:1469`)

- **Limite residual desse teste:** só verifica `r is not None`, sem recarregar/verificar a célula ou os contadores; remover a chamada a `julgar_colab` de `julgar_celula`, conservando `return out`, continuaria satisfazendo essa condição. Além disso, a fixture não cria batidas, exercitando o ramo `adesao is None`, que retorna antes do ramo normal com adesão. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:62`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:193`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:414`, `app/ponto/services/cartorio.py:1203`, `app/ponto/services/cartorio.py:1211`, `app/ponto/services/cartorio.py:1318`, `app/ponto/services/cartorio.py:2040`, `app/ponto/services/cartorio.py:2054`)

- **Achado 5: parcialmente fechado.** A Justificativa agora é consultada novamente e seu status `pendente` é exigido; o saldo do período é recarregado/conferido; a terceira tela, correção de concluída, ganhou teste com recusa e status final `concluido`. Porém `_nada_mudou` ainda não compara observações nem trilhas anteriores, e tampouco exige igualdade exata do conjunto de ausências/status originais. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:392`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:499`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:513`)

- **Achado 6: congela os testes, mas não elimina o relógio interno.** `_Base` e `FeriasTelaTrilhaTest` estão congelados; o teste H12 também está. O acordador continua lendo `localdate()` quando encontra lado alterado sem fim. Não há asserção de ausência de leitura do relógio: os testes de idempotência observam invalidação/carimbo, portanto recolocar a leitura antecipada antiga passaria por essas asserções. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:59`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:433`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:36`, `app/ponto/services/ausencia.py:175`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:319`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:333`)

- **Os testes centrais novos atacam os defeitos certos:** aprovação com período conta chamadas da invalidação real via `wraps`; movimento setembro→novembro exige que outubro permaneça carimbado. Os testes H12 de movimento entre competências adjacentes, isoladamente, continuariam passando com o envelope antigo. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:87`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:338`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:353`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:275`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:292`)

- **Mocks acrescentados não substituem os caminhos positivos centrais:** o espião do cartório chama a função real; o mock da trilha injeta especificamente o `None` de falha e delega os demais eventos à função real. Execução Django, RED contra a base e incidência produtiva: **[nao verificado]**. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:412`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:464`)

_5912 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
