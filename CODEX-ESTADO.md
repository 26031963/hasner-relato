# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 11:13** · publicado em **18/11/2026 11:15** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 11:13 · sessao 43c34e30b4b5 · - · - · origem=Ronald (tmux)

A investigação dos caminhos externos foi enviada ao agente principal, com referências para a régua CCT, os PDFs, a exportação da folha e o cron de horas extras.

## 18/11 11:13 · sessao 26e290adc033 · - · - · origem=Ronald (tmux)

A rastreabilidade do motor foi enviada ao agente principal, incluindo o conjunto vazio instalado pelo construtor, os dois sítios herdados que consultam feriado, os sítios de outras classes que não são herdados e os caminhos de seleção de período e esmeril. As conclusões estão apoiadas nas linhas citadas nas mensagens enviadas.

## 18/11 11:13 · sessao 6ae4800e14a8 · - · - · origem=Ronald (tmux)

A análise do calculador foi enviada ao agente principal, incluindo os caminhos pelo fechamento, pelo comando de comparação e pela releitura de períodos, com as respectivas evidências de código.

## 18/11 11:10 · sessao c2aba1a0842f · ESPURIA-TELA · verificador · origem=Code

**Conclusão:** o c9 remove, por análise do código e do diff fornecido, as duas incompatibilidades de dublês que explicam os três vermelhos relatados; isso não prova que os testes ou a suíte ficaram verdes, pois não há resultado de execução do c9 no material fornecido. As alterações estão em [e3_lu3_apoio.py:100](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/e3_lu3_apoio.py:100) e [test_palavra_parcial_e_pendente.py:35](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/tests/test_palavra_parcial_e_pendente.py:35). Resultado da execução: **[não verificado]**.

### A. Os três vermelhos

1. **`test_MORDE_quando_a_FOLHA_cobre_o_pendente_e_IGNORADO`**: o teste passa `_Aus` como cobertura; o ramo de cobertura lê `cobertura.pk`. O diff fornecido mostra que o dublê no c8 não tinha `pk` e que o c9 acrescenta `pk=None`, removendo esse `AttributeError` específico. A asserção continua verificando o resultado da palavra. [test_palavra_parcial_e_pendente.py:35](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/tests/test_palavra_parcial_e_pendente.py:35), [test_palavra_parcial_e_pendente.py:95](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/tests/test_palavra_parcial_e_pendente.py:95), [dia_decidido.py:473](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:473).

2. **`test_nao_perguntado_intencional_preserva_o_juiz_da_oferta`**: o diff fornecido mostra que o `do_dia` falso devolvia apenas três chaves; o calendário acessa `quem` diretamente. O c9 acrescenta as três chaves da trilha ao dublê, removendo esse `KeyError` específico. [e3_lu3_apoio.py:100](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/e3_lu3_apoio.py:100), [calendario.py:757](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/colaboradores/services/calendario.py:757), [test_e3_degrau_dono_e_porta.py:788](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/test_e3_degrau_dono_e_porta.py:788).

3. **`test_M8_carga_inteira_nao_engole_relogio`**: usa o mesmo dublê. O `KeyError` no contexto é capturado pelo `except Exception` da oferta, que devolve `None` para o bloco; isso explica o caminho para `nao_sei` descrito no dado. Acrescentar as chaves remove essa causa específica, sem demonstrar sozinho o resultado final do teste. [e3_lu3_apoio.py:105](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/e3_lu3_apoio.py:105), [calendario.py:156](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/colaboradores/services/calendario.py:156), [test_e3_degrau_dono_e_porta.py:752](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/test_e3_degrau_dono_e_porta.py:752).

O **diff c9 fornecido não altera asserções**: altera o dublê de retorno, o construtor de `_Aus` e comentários/docstring. A cópia atual mostra as asserções em [test_palavra_parcial_e_pendente.py:95](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/tests/test_palavra_parcial_e_pendente.py:95) e [test_e3_degrau_dono_e_porta.py:752](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/test_e3_degrau_dono_e_porta.py:752). A comparação independente com o objeto Git do c8 ficou **[não verificada]**.

### B. Todos os retornos de `do_dia`

Todos os cinco ramos devolvem exatamente as seis chaves **`veredito`, `palavra`, `cor`, `quem`, `quando`, `ausencia_id`**, inclusive quando os valores da trilha são `None`. [dia_decidido.py:464](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:464).

| Ramo, na ordem do código | Valores principais |
|---|---|
| Sem cobertura e com pendente | `veredito=v`; palavra e cor do pendente; três valores da trilha `None`. [dia_decidido.py:465](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:465) |
| `v == TRABALHOU` | `veredito=v`; palavra e cor vazias; trilha `None`. [dia_decidido.py:471](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:471) |
| Com cobertura | `veredito=v`; rótulo da cobertura; cor de `v`; trilha da cobertura. [dia_decidido.py:473](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:473) |
| Decisão com veredito igual a `v` | `veredito=v`; palavra, cor e trilha da decisão. [dia_decidido.py:477](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:477) |
| Retorno final | `veredito=v`; palavra e cor calculadas de `v`; trilha `None`. [dia_decidido.py:481](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:481) |

### C. Escritores, leitores e significado

`v` vem de `veredito_do_dia`. A tabela acima identifica, por ramo, os escritores de `palavra` e `cor`; os três campos da trilha saem de `_ninguem`, da cobertura ou da decisão, conforme o mesmo ramo. [dia_decidido.py:451](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:451), [dia_decidido.py:464](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:464).

Os leitores **diretos** das seis chaves do retorno encontrados na cópia são:

- Calendário: lê as seis e as publica como `veredito_dia`, `palavra_dia`, `cor_dia` e `ausencia_decidida_por/em/id`. [calendario.py:748](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/colaboradores/services/calendario.py:748).
- Espelho: lê as seis; também usa `veredito` para escolher o ícone da classe. [espelho.py:341](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/espelho.py:341).

Entre os leitores indiretos, a grade e o espelho passam `quem` e `quando` ao partial; a API transporta os seis campos renomeados e converte o instante para string local ISO na resposta. [\_calendario_grade.html:109](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/templates/colaboradores/partials/_calendario_grade.html:109), [espelho.html:192](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/templates/ponto/espelho.html:192), [api/views.py:1392](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/api/views.py:1392), [api/views.py:1405](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/api/views.py:1405). **Não sei** afirmar que esta relação esgota todos os leitores indiretos de apresentação; censo completo **[não verificado]**.

Na trilha, `quem_quando` grava **username** do aprovador, ou do registrador, ou `'sistema'`; grava `aprovada_em` ou, independentemente, `criado_em`. O ID é o `pk` da ausência usada naquele ramo, que pode ser diferente entre cobertura e decisão. [dia_decidido.py:549](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:549), [dia_decidido.py:442](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:442).

O partial explicita que o nome pode ser de quem lançou, e formata `quando` como data e hora; a API converte o datetime para hora local ISO. Não identifiquei nesses leitores uma troca de ausência ou uma interpretação de username como nome civil. [\_decidiu_icone.html:8](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/templates/ponto/partials/_decidiu_icone.html:8), [api/views.py:1405](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/api/views.py:1405). Porém, como os fallbacks de pessoa e instante são independentes, o código **não garante** que ambos descrevam o mesmo ato quando os campos estão parcialmente preenchidos; ocorrência em dados reais **[não verificada]**. [dia_decidido.py:555](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:555).

### D. Produtores de `cobertura` e `decisao`

- **Calendário:** passa `_aus_folha_map.get(data)` e `_decididos.get(data)`, carregados respectivamente por `cobertura_ausencia_periodo(..., para='folha')` e `decididos_do_periodo`. [calendario.py:561](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/colaboradores/services/calendario.py:561), [calendario.py:748](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/colaboradores/services/calendario.py:748).
- **Espelho:** obtém a cobertura da mesma função, guarda cobertura e decisão em campos de transporte e depois os passa a `do_dia`. [espelho.py:602](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/espelho.py:602), [espelho.py:658](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/espelho.py:658), [espelho.py:884](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/espelho.py:884), [espelho.py:330](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/espelho.py:330).

Nesses caminhos, a cobertura vem de queryset de `ponto.models.Ausencia`; `_cobertura_pura` põe a própria instância no mapa. Não encontrei nesses dois produtores cobertura de outro tipo. [turnos.py:2764](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/turnos.py:2764), [turnos.py:2781](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/turnos.py:2781).

A decisão vem de `decididos_do_periodo` → `cartao_do_dia`, que inclui as seis chaves perguntadas: `veredito`, `rotulo`, `cor`, `quem`, `quando`, `ausencia_id`. Os dois chamadores citados não fornecem o parâmetro opcional `ausencias`, portanto usam o queryset da função. [dia_decidido.py:561](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:561), [dia_decidido.py:578](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:578).

### E. Outros dublês antigos

**Não sei garantir um censo completo de todos os dublês em `app/**/tests/**`.** Na busca realizada, não confirmei outro dublê de `do_dia`, cobertura ou decisão que ainda tenha o contrato antigo e alcance um dos leitores. Os dois corrigidos aparecem em [e3_lu3_apoio.py:100](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/e3_lu3_apoio.py:100) e [test_palavra_parcial_e_pendente.py:35](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/tests/test_palavra_parcial_e_pendente.py:35); inexistência de outros e eventual passagem vazia permanecem **[não verificados]**.

### F. Exceção no contexto em produção

Nos dois produtores rastreados, o retorno real de `do_dia` inclui sempre as três chaves, a cobertura vem de `Ausencia` e a decisão vem do cartão com as chaves exigidas. **Não identifiquei, nesses caminhos, um `KeyError` ou `AttributeError` causado pela ausência das novas chaves.** [dia_decidido.py:464](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:464), [turnos.py:2764](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/turnos.py:2764), [dia_decidido.py:561](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:561).

Isso não demonstra ausência de outras exceções nem o estado dos dados reais: **[não verificado]**. O `except` continua capturando qualquer `Exception` do contexto e retornando `None` para o bloco. [calendario.py:156](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/colaboradores/services/calendario.py:156).

### G. Checklist

- **Segundo juiz, segundo escritor fora da porta, relógio fora da borda e derivação paralela:** não confirmei ocorrência nos trechos do diff fornecido; uma auditoria conclusiva contra todo o registro de juízes e todos os fluxos é **[não verificada]**. A seleção da classe nova usa o `veredito` já devolvido. [dia_decidido.py:198](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:198), [espelho.py:351](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/espelho.py:351).
- **Selo/RED sem mordida:** há asserções que verificam os valores da trilha e sua remoção, mas não fiz teste de mutação; portanto, a mordida efetiva e a existência de outros selos vazios são **[não verificadas]**. [test_espelho_decidiu_icone.py:63](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/tests/test_espelho_decidiu_icone.py:63), [test_espelho_decidiu_icone.py:108](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/tests/test_espelho_decidiu_icone.py:108).
- **Campo com dois significados:** há o risco de pessoa e instante de atos distintos pelos fallbacks independentes descritos em C; não há prova de ocorrência no dado real. [dia_decidido.py:555](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:555).
- **Band-aid:** o diff da obra acrescenta `.get` nas três leituras da trilha no transporte da API, o que transforma chave ausente em `None`; isso pode mascarar uma ausência inesperada de chave naquele ponto. A intenção de admitir dias fora da janela está documentada ao lado, mas não demonstra que toda ausência de chave seja esse caso. [api/views.py:1385](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/api/views.py:1385), [api/views.py:1398](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/api/views.py:1398).

## 18/11 10:59 · sessao 96a1461bdf52 · E4C-F6F7 · verificador · origem=Code

1. **Fila — o descarte do item 1 procede quanto à diferença identificada, mas a conclusão “nenhum caso piora” não está demonstrada.** A face por pergunta chama `_pela_decisao` diretamente; a tarefa por dia aplica antes guardas como vínculo, isenção, tranca, lâmpada e ausência ([tarefa_do_dia.py:220](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/tarefa_do_dia.py:220), [tarefa_do_dia.py:297](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/tarefa_do_dia.py:297), [tarefa_do_dia.py:318](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/tarefa_do_dia.py:318), [tarefa_do_dia.py:436](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/tarefa_do_dia.py:436)). A autoridade atribuída pelo brief e pela remedição é **dado declarado**, pois esses documentos não estão na cópia [não verificado]. Para `RESPOSTA` com texto, inclusive numa conversa, a face devolve ação de ADMIN e o bool é `True`, como no valor antigo declarado ([tarefa_do_dia.py:168](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/tarefa_do_dia.py:168), [tarefa_do_dia.py:468](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/tarefa_do_dia.py:468), [test_f6_u4_aguarda_admin_pela_tarefa.py:1](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:1)). Não encontrei, nos elementos fornecidos, sequência concreta que demonstre piora causada pelas guardas ignoradas; a afirmação universal e o comportamento do APK para outros valores ficam [não verificado].

2. **Fila — o descarte do item 2 procede para a sequência descrita.** Após `VALIDAÇÃO → RESPOSTA(mantém) → DEVOLUÇÃO → RESPOSTA(mantém)`, o laço para na `DEVOLUÇÃO`, não encontra uma validação imediatamente anterior à cauda e trata a última resposta como resposta comum ([tarefa_do_dia.py:163](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/tarefa_do_dia.py:163)). A porta de devolução limpa `validada_em` e registra `DEVOLUÇÃO`; com resposta e instante válidos, o estado projetado é `RESPONDIDA`, e ambos os critérios dão `True` ([validacao.py:269](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/validacao.py:269), [models.py:3152](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/models.py:3152), [tarefa_do_dia.py:168](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/tarefa_do_dia.py:168)). A realização exata da sequência por portas de produção depende das pré-condições das duas escritoras de resposta que mantém a validação [não verificado] ([disputa_emissao.py:1664](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/disputa_emissao.py:1664), [disputa_emissao.py:1694](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/disputa_emissao.py:1694)).

3. **Fila — o motivo do descarte do item 4 corresponde ao código atual; sua antiguidade na base está [não verificado].** No formato hora, o texto anterior é mostrado quando existe, mesmo sem `resposta_enviada`; a data exige `resposta_enviada`, e o comentário declara que o valor anterior não é pré-carregado ([`_card_atendimento.html:134`](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/templates/chamados/partials/_card_atendimento.html:134)). O usuário pode, portanto, ver “Você respondeu” sem data para texto antigo: isso é comportamento visível, mas a alegação de que já existia em `dd489c627` depende do anexo e não pode ser conferida nesta cópia [não verificado].

4. **Fila — o descarte do item 5 procede quanto ao alcance do teste, com a mesma ressalva histórica.** A costura real atribui `resposta_enviada` pelo inverso de `espera_o_texto_do_colaborador` ([fio.py:178](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/fio.py:178), [fio.py:356](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/fio.py:356)). Os testes F7 do card constroem o objeto com esse atributo fornecido: verificam a renderização, não a integração dessa costura ([test_f7_validada_conservada.py:56](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/tests/test_f7_validada_conservada.py:56), [test_f7_validada_conservada.py:70](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/tests/test_f7_validada_conservada.py:70)). Há testes separados que exercitam o card com a costura e seu juiz ([test_e3_item1b.py:150](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/tests/test_e3_item1b.py:150)). Que a costura seja anterior a `dd489c627` permanece [não verificado].

5. **Fila — o descarte extra sobre ordenação procede para `_aguard`.** Sua ordem não é usada diretamente: a lista fornece existência, conjunto de IDs e contagem; a ordem de `_fio` vem da outra consulta ([views_cobrar.py:178](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/views_cobrar.py:178)). A função de perguntas vivas ordena por `pk` ([juizes.py:1505](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/juizes.py:1505)).

6. **Fila — os dois mutantes são mordidos por inspeção.** Se o ramo passivo retornar sempre `True`, falha o `assertFalse` com `conservado=False` ([test_f6_u4_aguarda_admin_pela_tarefa.py:80](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:80)). Se o ramo D05 for substituído pela regra `any ADMIN`, as duas entradas D05 com dono `SISTEMA` retornam `False` e violam o `assertTrue` ([test_f6_u4_aguarda_admin_pela_tarefa.py:87](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:87), [tarefa_do_dia.py:453](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/tarefa_do_dia.py:453)). Isso é análise das asserções, não execução dos mutantes [não verificado].

7. **Fila — escritores e leitores.** No delta v2 fornecido, a única alteração declarada é a classe de testes; ele não altera escritores nem leitores de produção [não verificado]. Na cópia, `aguarda_admin` é montado como campo JSON pela API a partir da face por pergunta e de `aguarda_admin_pela_tarefa`; o consumidor no APK não foi fornecido ([views.py:2538](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/api/views.py:2538), [views.py:2554](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/api/views.py:2554)). Para `resposta_mantem_validacao`, a cópia confirma `False` na resposta humana, `True` nas duas portas citadas e no backfill, e leitura em `_pela_decisao` ([disputa_emissao.py:1525](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/disputa_emissao.py:1525), [disputa_emissao.py:1679](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/disputa_emissao.py:1679), [disputa_emissao.py:1701](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/disputa_emissao.py:1701), [models.py:4139](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/models.py:4139), [tarefa_do_dia.py:163](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/services/tarefa_do_dia.py:163)). A mudança do leitor em relação à base é declarada nos testes F6; uma comparação independente com o código de `dd489c627` fica [não verificado] ([test_f6_t_resposta_mantem_validacao.py:1](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/tests/test_f6_t_resposta_mantem_validacao.py:1)).

A conservação F7 da projeção validada também aparece no modal e no contador, com teste para linha de outro instante ([modal_fio.html:345](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/templates/chamados/partials/modal_fio.html:345), [backfill_decisoes_pergunta.py:94](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/management/commands/backfill_decisoes_pergunta.py:94), [test_f7_contador_validada_sem_linha.py:42](/home/ronald/codex_ws/f4477f4b50985dd0586ab11b542db5ffadc22b58/app/chamados/tests/test_f7_contador_validada_sem_linha.py:42)). Não há, nos dados fornecidos, confirmação de execução desses testes ou medida atual de produção [não verificado].

## 18/11 10:56 · sessao 1b76dc79eaee · E4C-F6F7 · verificador · origem=Code

**Conclusão: há um bloqueio na equivalência atribuída à tarefa do dia e lacunas de teste que deixam casos relevantes sem prova.** A análise usa o diff fornecido e a cópia; a cópia não contém metadados Git, portanto a identidade do HEAD e a comparação direta com a base são **[não verificado]**. A ocorrência dos casos em produção e a execução dos testes também são **[não verificado]**. [nao verificado]

### Achados

1. **Bloqueia — a face por pergunta não reproduz necessariamente a tarefa publicada para o dia.** `exigencia_das_perguntas` busca os PKs recebidos e chama `_pela_decisao` diretamente, enquanto a carga por dia só inclui `conversa=False` e `data_evento` dentro da janela; a consulta do app admite perguntas vivas sem esses dois filtros. Assim, uma conversa viva com ato `RESPOSTA` e texto recebe `aguarda_validacao` pela face, embora não pertença ao universo da carga por dia. Para perguntas incluídas, a face também não passa pelas decisões anteriores de `_tarefa_pura`, como competência trancada, lâmpada acesa, vencimento e ausência. Isso demonstra diferença entre a **face** e a tarefa publicada; não demonstra que `_pela_decisao`, chamada com os mesmos fatos, devolva resultados diferentes. ([tarefa_do_dia.py:436](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:436), [tarefa_do_dia.py:506](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:506), [tarefa_do_dia.py:230](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:230), [tarefa_do_dia.py:291](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:291), [pergunta_no_app.py:45](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/pergunta_no_app.py:45)).

2. **Fila — o laço de resposta que mantém validação não cobre interrupções da sequência.** Em `VALIDACAO → RESPOSTA mantém → DEVOLUCAO → RESPOSTA mantém`, ele escolhe a última resposta, pois o ato anterior à cauda é `DEVOLUCAO`: com texto, retorna `aguarda_validacao`, enquanto `DEVOLUCAO` retorna `materializacao_recusada`; com texto vazio, retorna ação do colaborador. A devolução limpa a validação projetada, e o backfill pode construir resposta com texto vazio. Se essa sequência específica pode ser produzida pelas portas reais e qual ato deve prevalecer nela são **[não verificado]**; portanto, a diferença é comprovada, mas um erro de produção nessa sequência não está demonstrado. Sem validação anterior, uma resposta `mantém` isolada é tratada como resposta comum, como o teste prevê. ([tarefa_do_dia.py:158](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:158), [validacao.py:269](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/validacao.py:269), [models.py:4139](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/models.py:4139), [test_f6_t_resposta_mantem_validacao.py:30](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/tests/test_f6_t_resposta_mantem_validacao.py:30)).

3. **Fila — os ramos novos de `aguarda_admin` têm mudanças sem cobertura suficiente.** Para passivo, `conservado` é exatamente a comparação removida do app, `_estado == RESPONDIDA`: conserva o **booleano anterior**, inclusive quando `validada_em` faz `estado` ser `VALIDADA` ou `RESOLVIDA`; não significa “há resposta projetada”. `acoes is None` vira `False`; D05 vira `True` mesmo para ação do sistema. D05 pode divergir do booleano anterior se houver linha de validação sem validação projetada e sem resposta projetada; a existência desse caso no app é **[não verificado]**. O caso ordinário com `validada_em` projetada não prova a regressão no JSON, pois C6 o exclui das vivas. Os testes U4 cobrem resposta, silêncio e passivo respondido, mas não esses casos. ([views.py:2554](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/api/views.py:2554), [models.py:3152](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/models.py:3152), [tarefa_do_dia.py:453](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:453), [juizes.py:1403](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/juizes.py:1403), [test_f6_u4_aguarda_admin_pela_tarefa.py:53](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:53)).

4. **Fila — há leitor que ainda apresenta texto antigo como resposta.** Para pergunta viva de tipo hora, o card mostra “Você respondeu” quando há `resposta_colab`, mesmo se `resposta_enviada` é falso por ausência de instante; o novo bloqueio de pré-preenchimento usa `resposta_enviada` nos outros formatos. Pode ser histórico intencional, mas a frase não o identifica como tal. ([fio.py:178](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/fio.py:178), [_card_atendimento.html:141](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/templates/chamados/partials/_card_atendimento.html:141)).

5. **Fila — há testes sem mordida para partes dos defeitos.** O RED de C1 morde a regressão simples `VALIDACAO → RESPOSTA mantém`, mas passa com a implementação atual que não resolve a sequência interrompida do achado 2. O teste de passivo U4 só verifica o caso cujo conservado é `True`: uma implementação que devolvesse sempre `True` para passivo passaria nesse teste. Os testes do card injetam `resposta_enviada` manualmente e não provam a costura real do fio. Já o teste do contador com validação em outro instante morde a substituição indevida da correspondência por mera existência de uma validação. ([test_f6_t_resposta_mantem_validacao.py:13](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/tests/test_f6_t_resposta_mantem_validacao.py:13), [test_f6_u4_aguarda_admin_pela_tarefa.py:64](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:64), [test_f7_validada_conservada.py:64](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/tests/test_f7_validada_conservada.py:64), [test_f7_contador_validada_sem_linha.py:42](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/tests/test_f7_contador_validada_sem_linha.py:42)).

### Escritores, leitores e significado

- **`resposta_mantem_validacao`:** a porta humana grava `False` e limpa validação; resposta automática e carimbo pela trilha gravam `True` sem limpá-la; a reconstrução da projeção também grava `True`. `_pela_decisao` é o leitor novo. O significado declarado é preservar uma validação vigente, mas a escolha do ato em sequências interrompidas permanece limitada como no achado 2. ([disputa_emissao.py:1485](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/disputa_emissao.py:1485), [disputa_emissao.py:1674](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/disputa_emissao.py:1674), [disputa_emissao.py:1694](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/disputa_emissao.py:1694), [models.py:4072](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/models.py:4072)).

- **Projeção `validada_em`, `resposta_colab`, `respondida_em`:** as portas de resposta escrevem o par resposta/instante; validação escreve o carimbo e ato correspondente; `carimbar` também pode completar instante e validação. Modal e card administrativo mostram validação projetada; o contador pergunta ao mesmo predicado de correspondência do backfill se falta o ato no mesmo instante. Essa conservação atende à regra fornecida para F7; não é prova de que a sequência esteja completa. ([disputa_emissao.py:1674](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/disputa_emissao.py:1674), [validacao.py:161](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/validacao.py:161), [models.py:3222](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/models.py:3222), [modal_fio.html:345](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/templates/chamados/partials/modal_fio.html:345), [_pergunta_card.html:20](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/templates/chamados/partials/_pergunta_card.html:20), [backfill_decisoes_pergunta.py:94](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/management/commands/backfill_decisoes_pergunta.py:94)).

- **`resposta_enviada`:** é atributo transitório posto pelo fio como negação de `espera_o_texto_do_colaborador`; o card o lê para a confirmação e para o pré-preenchimento alterado, com a ressalva do texto de hora no achado 4. ([fio.py:178](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/fio.py:178), [fio.py:356](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/fio.py:356), [_card_atendimento.html:100](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/templates/chamados/partials/_card_atendimento.html:100)).

- **`aguarda_admin`, `_pela_decisao` e `porta_da_tarefa`:** o primeiro é montado no JSON pela face por pergunta; `_pela_decisao` também é chamado dentro da tarefa por dia; a porta publicada é lida, entre outros, por Folha, vigia e beco. Não se pode tratar a face por pergunta como equivalente à porta por dia sem considerar os filtros e guardas do achado 1. ([views.py:2537](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/api/views.py:2537), [tarefa_do_dia.py:282](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:282), [folha/fora_da_folha.py:71](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/folha/fora_da_folha.py:71), [vigia_fio.py:594](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/vigia_fio.py:594), [furo_sem_canal.py:98](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/furo_sem_canal.py:98)).

- **`_item_validar` e `aguardando_colab`:** a view de mensageria passa um único `agora` a `_item_validar`; `validar_inline` agora conta perguntas vivas que o juiz diz não respondidas. O helper de vivas ordena por PK, enquanto o diff removido ordenava por `data_evento, pk`: a ordem dos itens do fio pode mudar; impacto visível concreto **[não verificado]**. ([views_mensageria.py:540](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/api/views_mensageria.py:540), [views_cobrar.py:168](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/views_cobrar.py:168), [juizes.py:1505](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/juizes.py:1505)).

### Carga, demais leitores e classes de reprovação

A extração preserva, pelo diff fornecido, os blocos de consulta e os cortes por `agora` da carga: decisões, passivo, marcos, batidas e fila continuam alimentados com os mesmos parâmetros, inclusive os IDs dos colaboradores da carga. Alteração da contagem efetiva de queries ou do resultado em execução é **[não verificado]**. A diferença de universo nasce na nova face por pergunta, descrita no achado 1. ([tarefa_do_dia.py:385](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:385), [tarefa_do_dia.py:496](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:496)).

A mensageria continua selecionando respondidas com texto e vivas, uma pergunta diferente da posse calculada pela nova face; o PDF recebe procedência que pode chamar uma resposta de validada por `validada_em` **ou** `materializada_em`, enquanto o badge F7 usa `validada_em`. A equivalência dessas apresentações para todos os casos é **[não verificado]**. ([juizes.py:1761](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/juizes.py:1761), [procedencia_batida.py:65](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/ponto/services/procedencia_batida.py:65), [pdf_espelho.py:946](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/relatorios/pdf_espelho.py:946), [modal_fio.html:345](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/templates/chamados/partials/modal_fio.html:345)).

| Classe | Avaliação do diff |
|---|---|
| 1. Segundo juiz | **Risco demonstrado de face divergente:** chama o corpo de T fora das guardas da tarefa por dia; se o registro formal considera essa face um segundo juiz é **[não verificado]**. ([tarefa_do_dia.py:436](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:436)) |
| 2. Segundo escritor fora da porta | **Não identificado no diff:** as adições examinadas leem fatos ou renderizam; exaustividade além do diff **[não verificado]**. ([tarefa_do_dia.py:377](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:377), [backfill_decisoes_pergunta.py:94](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/management/commands/backfill_decisoes_pergunta.py:94)) |
| 3. Relógio fora da borda | **Não identificado nas funções alteradas:** as duas views passam `agora`; `_item_validar` o recebe. ([views.py:2539](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/api/views.py:2539), [views_mensageria.py:540](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/api/views_mensageria.py:540)) |
| 4. Derivação paralela | **Sim, risco funcional:** a face aplica `_pela_decisao` a universo e contexto diferentes dos da tarefa publicada; ver achado 1. ([tarefa_do_dia.py:446](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:446)) |
| 5. Selo/RED sem mordida | **Sim, para os casos não cobertos**, em particular passivo sempre verdadeiro e sequência interrompida; o contador tem caso que morde correspondência de instante. ([test_f6_u4_aguarda_admin_pela_tarefa.py:64](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:64), [test_f7_contador_validada_sem_linha.py:42](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/tests/test_f7_contador_validada_sem_linha.py:42)) |
| 6. Ordem de trava | **Não identificada alteração de trava no diff fornecido**; comportamento concorrente **[não verificado]**. |
| 7. Pouso antes do pré-requisito | **[não verificado]**: o dado fornecido não comprova a ordem real de deploy ou backfill. | [nao verificado]
| 8. Dois significados | **Sim, diferença de unidade/contexto:** pergunta do app pode ser conversa em dia civil, enquanto a carga T seleciona fatos por dia do turno; ver achado 1. ([pergunta_no_app.py:115](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/pergunta_no_app.py:115), [tarefa_do_dia.py:506](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:506)) |
| 9. Fallback | **Sim, decisão explícita a revisar:** `acoes is None` é convertido em `False`, enquanto passivo usa o booleano conservado; se isso esconde um caso alcançável no app é **[não verificado]**. ([tarefa_do_dia.py:457](/home/ronald/codex_ws/8542d4e728be8c4abc64de233727ac5bdba52bfb/app/chamados/services/tarefa_do_dia.py:457)) |

## 18/11 10:55 · sessao 002efbc32450 · SONDA-DIA-IMPAR-1811 · sonda · origem=Code

A: `   pago_min=671 realizado_do_dia.minutos=720` (dado raspado, linha 13); B: `   achados por pk=0 por ticket_id_glpi=0 de 17` (linha 204); C: `CHAMADOS_NA_CLASSE {'chamados_abertos_na_celula': 505, 'dias_com_chamado_aberto': 473}` (linha 246); D: `D_EX col27 2026-10-27 pago=713 soma=720 cad=60 fonte=escala sem intervalo ind=True veredito=concorde` (linha 264).

Numero as linhas do dado a partir de `== anexo: intervalo.out ==`, incluindo a linha em branco. A sonda terminou com `FIM_SONDA_INTERVALO` e `rc_sombra=0` (linhas 273–274). O arquivo da sonda não está presente na cópia examinada; sua definição de classe é a fornecida no pedido, e a confirmação independente do hash do HEAD fica **[não verificado]**.

### A — caso de 49 minutos

O par exibido é 07:48–19:48; o realizado é **720 min**, o pago é **671 min**, diferença de **49 min** (dado, linhas 12–13). O juiz entrega literalmente:

`{'pausas': '[]', 'minutos': 0, 'fonte': 'escala sem intervalo', 'cadastrado_min': 60, 'cadastrado_fonte': 'duracao declarada', 'indenizavel': True, 'inicio': 'None', 'fim': 'None'}` (dado, linha 10).

O cadastro declara **60 min por duração**, com `intervalo_indenizavel=True`, mas o DNA não tem `hii` nem `hfi`, e `pausas_do_dia` está vazio (dado, linhas 6–8). O juiz separa o intervalo cadastrado da resposta de intervalo do dia: sem marcos de pausa, responde zero e `escala sem intervalo` ([juiz_batida.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/juiz_batida.py:286)).

A diferença tem um caminho plausível no HEAD: o realizado trunca as batidas ao minuto e, sem janela fixa de pausa aplicável, mantém o par; o motor, por sua vez, pode recortar a saída pela janela de HE antes de calcular os minutos trabalhados, que são acumulados no DiaPago ([turnos.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/turnos.py:740), [motor_calculo_v2.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:1731), [dia_pago.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_pago.py:173)). O marco de saída do DNA é 19:00 e a saída exibida é 19:48; os timestamps usados pelo motor podem conservar segundos ([motor_calculo_v2.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:602); dado, linhas 8 e 12). **A causa exata dos 49 min permanece [não verificado]**: faltam a configuração e a decisão aplicáveis da janela de HE, os segundos e as pontas efetivamente usadas naquele cálculo.

### B — col868, col934 e os 17 chamados

| Pessoa | Marcos e cadastro | Flag |
|---|---|---|
| col868 | `hii=01:00`, `hfi=02:00`; **60 min** por janela declarada | `False` (dado, linhas 18 e 22) |
| col934 | `hii=05:00`, `hfi=06:00`; **60 min** por janela declarada | `False` (dado, linhas 147 e 151) |

**Os 17 chamados não foram encontrados**, tanto por PK de `ChamadoColaborador` quanto por `ticket_id_glpi`; a sonda imprime as seguintes linhas literalmente (dado, linhas 203–204 e 223–239):

```text
   CH 39641 NAO_ENCONTRADO
   CH 39642 NAO_ENCONTRADO
   CH 39643 NAO_ENCONTRADO
   CH 39644 NAO_ENCONTRADO
   CH 39645 NAO_ENCONTRADO
   CH 39646 NAO_ENCONTRADO
   CH 39647 NAO_ENCONTRADO
   CH 39648 NAO_ENCONTRADO
   CH 39649 NAO_ENCONTRADO
   CH 39650 NAO_ENCONTRADO
   CH 39651 NAO_ENCONTRADO
   CH 39652 NAO_ENCONTRADO
   CH 39653 NAO_ENCONTRADO
   CH 39654 NAO_ENCONTRADO
   CH 39655 NAO_ENCONTRADO
   CH 39656 NAO_ENCONTRADO
   CH 39664 NAO_ENCONTRADO
```

O marco faltante **de cada chamado** é, portanto, **[não verificado]**. Existem `PerguntaDisputa` com esses mesmos números de PK e motivos impressos pela sonda, mas elas apontam para outras pessoas e datas; a coincidência de números não identifica os chamados solicitados (dado, linhas 206–222). No HEAD, números podem aparecer na PK implícita de `ChamadoColaborador`, em `ticket_id_glpi`, ou na PK implícita de `PerguntaDisputa`; há também relações como `celula`, `disputa` e `chamado_destino`, sem prova de que guardem a identidade desses 17 chamados ([models.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:27), [models.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:2519), [models.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3527)).

Para os dias mostrados, o juiz expressa falta de **`hi, hf`**, com os quatro papéis apagados, e também pergunta sobre intervalo sem batida completa; isso não permite atribuir um marco a cada chamado não encontrado (dado, linhas 21–23 e 150–152). As linhas `RETENCAO` e `CLASSIFICAR_EXPORT` mostram **ambas as pessoas fora por `furo_espelho`**: col868 tem dias retidos por `espelho_furo`, `espelho_cobrado` e `espelho_discordante`; col934, por `espelho_furo` e `espelho_cobrado` (dado, linhas 144–145 e 201–202). A afirmação específica de que **18 furos** são a causa exclusiva dessa retenção é **[não verificado]**: essas linhas incluem outros vereditos e não identificam os 17 chamados.

### C — frota da competência 11

A sonda conta **3.320 pessoa × dia** na sua classe, em 11.200 células de trabalho; nela há **567** classificados por `_furo_so_da_pausa`, **461** com `nao_retem_pela_pausa` e **539** com veredito que acusa (dado, linha 242).

| Recorte da sonda | Resultado |
|---|---|
| `indenizavel` na classe inteira | `True`: **1.882**; `False`: **1.438** (linha 243) |
| `so_pausa=True` | `True`: **3**; `False`: **564**; todos com `ciclo=None` (linhas 243–244) |
| Ciclo da classe inteira | `ciclo=None`: **3.320**; isso não discrimina o tipo de escala solicitado (linha 244) |
| Vereditos | **2.780 concorde**, 461 `furo/FURO_PARCIAL`, 24 `cobrado`, 16 `fato_em_ausencia`, 28 `nunca_bateu`, 2 `indefinida`, 7 `discordante`, 1 `coberta` e 1 `furo` com protesto adicional (linha 245) |
| Chamados na classe inteira | **505** abertos em **473** dias (linha 246) |
| Perguntas na classe inteira | **1.925** sem resposta em **934** dias: 511 `orfao_14h`, 354 `saida_sem_entrada`, 439 `intervalo_volta`, 435 `intervalo_saida`, 166 `batida_fora_escala_grave`, 5 `foto_ausente`, 14 `sem_posto`, 1 `liveness_indisponivel` (linha 247) |
| Pessoas e exclusão | **415** com dia na classe; **9** fora somente por dias da classe; **13** fora com dia da classe e outros; **306** fora por `furo_espelho` no total (linhas 248 e 258) |

Esses números **não são a resposta exata à classe pedida em C**. A classe descrita para a sonda exige entrada e saída acesas, cadastro positivo e fonte diferente de `batido`; ela inclui, por exemplo, 2.780 dias `concorde`, portanto não exige que exista um único furo e que ele seja o par de intervalo (pedido, definição de classe; dado, linhas 242 e 245). Já `_furo_so_da_pausa` rejeita dia desalinhado, aberto ou incerto e exige ao menos um papel apagado, todos os apagados sendo de pausa; não exige cadastro positivo nem testa a fonte ([export.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/export.py:1148)).

Os **9** são pessoas retidas exclusivamente por **dias da classe ampla**, não uma contagem comprovada de pessoas fora somente por furo de intervalo: entre os motivos impressos estão `espelho_cobrado`, `espelho_indefinida`, `espelho_discordante` e `espelho_fato_em_ausencia` (dado, linhas 249–258). Faltam à sonda os cruzamentos de **chamados, perguntas e pessoas fora** exclusivamente para a classe exata de C e a abertura por **tipo de escala**; esses números são **[não verificado]**.

### D — caminhos de desconto e limites do histograma

A lei registra L-032 como **PELA-METADE** e aponta `_intra_real` como o caminho que soma o intervalo batido ([LEIS.md](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:193)). Os caminhos identificados são:

| Caminho | Efeito quando falta batida de intervalo |
|---|---|
| `realizado_dos_turnos` | Se não há intervalo batido aceito, o modo não é livre e existe janela fixa `hii`–`hfi`, subtrai **a parte da janela que se sobrepõe ao turno**. Sem janela, em modo livre, ou sem sobreposição, não subtrai cadastro ([turnos.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/turnos.py:756)). `realizado_do_dia` busca modo e intervalo do vínculo ([turnos.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/turnos.py:787)). |
| Juiz → laço do motor | O juiz pode informar minutos cadastrados sem batida, mas o laço só transfere pausas com início e fim correspondentes a batidas reais; não subtrai `cadastrado_min` da jornada ([juiz_batida.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/juiz_batida.py:286), [motor_calculo_v2.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:616), [motor_calculo_v2.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:685)). |
| `_intra_real` e motores 12×36/comercial | `_intra_real` soma as janelas recebidas; os motores subtraem esse resultado da duração. Lista vazia significa **zero de desconto de intervalo**, ainda que haja cadastro ([motor_calculo_v2.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:1892), [motor_calculo_v2.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:2771), [motor_calculo_v2.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:3242)). O par único sem marcos de intervalo no DNA, como A, é um caso dessa forma de resposta do juiz; a execução exata do período de A permanece **[não verificado]** (dado, linhas 8–12). |
| `_intra_minima` e `_limite_com_suprimido` | O primeiro calcula o mínimo para apuração do suprimido; o segundo pode **acrescentar** ao limite de HE até o intervalo já descontado do previsto. Nenhum dos dois subtrai, por si, o cadastrado da duração trabalhada ([motor_calculo_v2.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:1803), [motor_calculo_v2.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:1805)). |
| Previsto e cadastro de escala | `minutos_previstos_do_dia` subtrai pausas declaradas completas do intervalo previsto, independentemente de batida; sem marcos de pausa, mantém o trecho bruto. A criação de cadastro também deriva jornada descontando pausas ou duração em seus ramos próprios; são cálculos de **previsto/cadastro**, não prova de desconto no pago ([utils.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:2291), [cadastro_tipo.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/cadastro_tipo.py:283)). |
| PDF, DiaPago e folha | O PDF alimenta `realizado_do_dia` com modo e intervalo; `DiaPago` acumula minutos já calculados pelo motor; a folha lê rubricas e usa o declarado no limite de HE. Esses pontos não demonstram uma segunda subtração autônoma do cadastrado do pago ([pdf_espelho.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/pdf_espelho.py:563), [dia_pago.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_pago.py:173), [porta_export.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/porta_export.py:545)). |

O `D_HIST` registra **487** como `pago==soma_pares(NAO desconta)`, **15** como `pago==soma-cadastrado(DESCONTA)`, **2.058** como `outro` e **760** sem pago ou sem soma; dos 487, 359 têm `ind=True` e 128 `False` (dado, linha 260). **487 é uma classificação numérica da sonda, não uma contagem comprovada de execuções do caminho sem desconto**: o motor pode recortar pontas antes de subtrair intervalos e trabalha com instantes que conservam segundos, enquanto outros cálculos truncam ao minuto ([motor_calculo_v2.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:2759), [turnos.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/turnos.py:740)). A distribuição de **cada ramo executado** e o número exato de pessoa × dia que nele não descontam estão **[não verificado]**: faltam as pontas efetivas e os intervalos efetivamente usados por período.

Quanto aos campos, o escritor de `intervalo.minutos`, `fonte`, `cadastrado_min` e `cadastrado_fonte` é `periodos_do_dia`; ele também copia para `intervalo.indenizavel` a flag do `TipoEscala` ([juiz_batida.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/juiz_batida.py:305)). A flag persistida é declarada em `TipoEscala` ([models.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/models.py:108)). O diagnóstico de cadastro lê `intervalo.minutos`; o laço do motor lê início, fim e pausas, não esses totais ([cadastro_realidade.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/cadastro_realidade.py:241), [motor_calculo_v2.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:689)). A busca literal no código de produção encontrou o escritor de `cadastrado_min` e `cadastrado_fonte`, mas nenhum leitor com esses nomes; leitores dinâmicos são **[não verificado]**. A flag cadastral também é lida em apresentação/escala, enquanto o motor declara que não a lê para decidir a intrajornada ([models.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/models.py:574), [hasner_filters.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/templatetags/hasner_filters.py:195), [motor_calculo_v2.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:822)).

## 18/11 10:54 · sessao 9b96c36b2372 · - · - · origem=Ronald (tmux)

Achados enviados ao agente principal, com os caminhos do juiz, os leitores identificados, a gravação no `DiaPago` e a hipótese condicionada da janela de HE para a diferença de 49 minutos. [app/ponto/juiz_batida.py:269](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/juiz_batida.py:269), [app/ponto/motor_calculo_v2.py:1714](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:1714), [app/ponto/services/dia_pago.py:173](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_pago.py:173).

## 18/11 10:54 · sessao d65b5a016ac4 · - · - · origem=Ronald (tmux)

A análise dos leitores, dos escritores da projeção e dos testes F7 foi enviada ao agente principal, incluindo a leitura indireta da pergunta pelo PDF e as referências de arquivo e linha. [nao verificado]

## 18/11 10:54 · sessao dda16edd2c89 · - · - · origem=Ronald (tmux)

Concluí a análise da carga, do universo da face por pergunta e dos ramos de `aguarda_admin` e enviei os resultados ao agente principal, incluindo a ressalva de que perguntas com `validada_em` projetada não chegam normalmente ao JSON porque C6 as considera não vivas (`app/chamados/juizes.py:1403-1406`; `app/chamados/services/pergunta_no_app.py:45-73`).

## 18/11 10:54 · sessao 31e2b7c169eb · - · - · origem=Ronald (tmux)

Resultados enviados ao agente principal, com referências aos caminhos de folha, DiaPago, PDF, escala e aos modelos de chamados e perguntas.

## 18/11 10:54 · sessao eff5671bbf24 · - · - · origem=Ronald (tmux)

A análise das sequências e dos escritores foi enviada ao agente principal, com referências de arquivo e linha e as limitações de alcance identificadas.

## 18/11 10:54 · sessao 7e2832a5ea44 · - · - · origem=Ronald (tmux)

Achados de `ponto/motor_calculo_v2.py` e `ponto/turnos.py` enviados ao agente principal, com os limites da inferência para o caso A. [app/ponto/motor_calculo_v2.py:1731](app/ponto/motor_calculo_v2.py), [app/ponto/turnos.py:756](app/ponto/turnos.py)

## 18/11 10:46 · sessao 671f115eebf8 · SONDA-DIA-IMPAR-1811 · sonda · origem=Code

```text
SONDA achadoA_casos agora=18/11 10:38:15 ultima_batida_sombra=18/11 03:55:21
============================================================
CASO C004 col281 08/11  (1.4s)
 ESPELHO n_marcacoes=2: 18:55 E orfa=True origem=app proc=None just= ; 06:58 S orfa=False origem=app proc=None just=
 APURAVEIS d-1 12h..d+1 12h n=4: 07/11 18:59:04 E pk=148830 orig=app itv=False apr=True tr= canal=apk perg=None ; 08/11 07:00:09 S pk=149145 orig=app itv=False apr=True tr= canal=apk perg=None ; 08/11 18:55:42 E pk=149472 orig=app itv=False apr=True tr= canal=apk perg=None ; 09/11 06:58:35 S pk=149781 orig=app itv=False apr=True tr= canal=apk perg=None
 RETRATADAS na janela n=0
 TURNO data_turno=2026-11-08 entrada=08/11 18:55:42 saida=09/11 06:58:35 aberto=False fim_fechado=09/11 06:58:35 batidas=18:55E 06:58S
   intervalos_do_turno=[]
 turnos de data_turno=2026-11-08 no pareamento da janela inteira: 1 (total na janela 14)
 REALIZADO(todos turnos)=(723, False, False, False, 0, False)
 REALIZADO(so turnos com saida)=(723, False, False, False, 0, False)
 borda_posicional=False
 CELULA: {"id": "137834", "colaborador_id": "281", "data": "2026-11-08", "origem": "gerada", "escala_geradora_id": "246", "dna": "{\"marcos\": null, \"lampadas\": null, \"dna_versao\": 2, \"tipo_ciclo\": \"12x36\", \"vinculo_id\": 246, \"tipo_escala_id\": 179, \"intervalo_livre\": true, \"minutos_jornada\": 720}", "gerada_em": "2026-10-21 08:50:22.986233+00:00", "julgada_em": "2026-11-09 21:57:11.865832+00:00", "impressao": "efd0e9f5a5cc829b1ea09876e6c6d9a33293dd2bc1b08c0ce8092a3058a96355", "veredito": "fato_sem_previsao", "veredito_via": "cartorio", "veredito_em": "2026-11-09 21:57:11.865832+00:00", "protestos": "[\"FATO_SEM_PREVISAO\"]", "ata": "{\"orfas\": [\"18:55\"], \"n_orfas\": 1, \"lampadas\": [], \"tipo_dia\": \"folga\", \"n_celulas\": 0, \"n_missing\": 0, \"sem_ancora\": false, \"precedencia\": {\"faixa\": \"folga\", \"texto\": \"\", \"rotulo\": \"Folga\", \"suprimidos\": [], \"tipo_do_dia\": \"folga\", \"era_dia_previsto\": false}, \"minutos_previstos\": 0, \"minutos_realizados\": 723}", "insumos_em": "2026-11-09 21:57:11.865832+00:00", "status_dia": "folga_trabalhada", "status_rotulo": "Trabalhou em dia sem jornada prevista"}
 EC.marcos_do_dia=(datetime.time(19, 0), datetime.time(7, 0), None, None) tipo_escala=12x36 · 19:00-07:00
 JUSTIFICATIVAS data_turno=2026-11-08 n=0 []
 DIAPAGO versao=motor pago_do_dia_h=None min=None | {"id": "2368660", "colaborador_id": "281", "mes": "11", "ano": "2026", "data": "2026-11-08", "tipo": "dia", "versao": "motor", "versao_motor": "legais|p5h=0|hr=1|fd=0", "evento_origem": "recalcular_fechamento_mes", "lavrado_em": "2026-11-18 10:01:18.646726+00:00"}
 DIAPAGO versao=oraculo pago_do_dia_h=None min=None | {"id": "2368677", "colaborador_id": "281", "mes": "11", "ano": "2026", "data": "2026-11-08", "tipo": "dia", "versao": "oraculo", "evento_origem": "recalcular_fechamento_mes", "detalhe": "{\"calculador_nao_decide\": [\"horas_atraso\", \"horas_extras_100\", \"horas_extras_100_feriado\", \"horas_extras_100_noturna\", \"horas_extras_50\", \"horas_extras_50_noturna\", \"horas_falta\", \"horas_noturnas\", \"horas_reflexo_dsr\", \"horas_saida_antecipada\", \"saldo_banco_horas\"]}", "lavrado_em": "2026-11-18 10:01:18.771705+00:00"}
 TELA(dia dict): pago_h=None veredito_dia='folga' palavra_dia='Folga' cor='#94a3b8' realizado_aberto=False realizado_h=12.1 minutos_realizados=723 previsto=[] ausencia=None falta_frase='' falta_marcos=[] rubricas={'lavrada': True, 'extras_50': 0.0, 'extras_100': 0.0, 'atraso': 0.0, 'antecipada': 0.0, 'noturnas': 0.0, 'intra': 0.0, 'trabalhadas': 0.0, 'minutos_extra': 0.0}
 TELA(dia dict) turnos_motor=[('08/11 18:55:42', '09/11 06:58:35', False, ['Intrajornada suprimida — 60min pagos como hora extra (Art.71 §4 CLT / Sumula 437 TST). Intra real: 0min.', 'DIA SEM JORNADA PREVISTA -- 723 min fora da janela de HE (L-097, bloqueio total): pendencia na Gestao de HE ate Autorizar.', 'FOLGA TRABALHADA — 08/11/2026 18:55 (dia de folga no ciclo 12x36 (ancora 26/07/2026)). Registro mantido conforme Portaria MTE 671/2021. Horas apuradas: 0.0h. Requer validacao do responsavel antes do fechamento.'])]
 PDF_LINHA: 08/11 dom | 19:00 07:00 | MARCACOES:S06:58 + *18:55 | Folga | HE fora da janela: 723 min em dia sem jornada prevista — nao conta sem decisao |  |  | —
============================================================
CASO C009 col902 28/10  (1.3s)
 ESPELHO n_marcacoes=3: 07:58 E orfa=False origem=app proc=None just= ; 12:06 S orfa=False origem=app proc=None just= ; 18:07 S orfa=False origem=app proc=None just=
 APURAVEIS d-1 12h..d+1 12h n=8: 27/10 12:10:13 S pk=138670 orig=app itv=False apr=True tr= canal=apk perg=None ; 27/10 13:10:05 E pk=138758 orig=app itv=False apr=True tr= canal=apk perg=None ; 27/10 18:00:44 S pk=139051 orig=app itv=False apr=True tr= canal=apk perg=None ; 28/10 07:58:46 E pk=139547 orig=app itv=False apr=True tr= canal=apk perg=None ; 28/10 12:06:03 S pk=139697 orig=app itv=False apr=True tr= canal=apk perg=None ; 28/10 18:07:37 S pk=140070 orig=app itv=False apr=True tr= canal=apk perg=None ; 29/10 07:58:05 E pk=140537 orig=app itv=False apr=True tr= canal=apk perg=None ; 29/10 11:58:37 S pk=140667 orig=app itv=False apr=True tr= canal=apk perg=None
 RETRATADAS na janela n=0
 TURNO data_turno=2026-10-28 entrada=28/10 07:58:46 saida=28/10 18:07:37 aberto=False fim_fechado=28/10 18:07:37 batidas=07:58E 12:06S 18:07S
   intervalos_do_turno=[]
 turnos de data_turno=2026-10-28 no pareamento da janela inteira: 1 (total na janela 19)
 REALIZADO(todos turnos)=(549, False, False, False, 60, False)
 REALIZADO(so turnos com saida)=(549, False, False, False, 60, False)
 borda_posicional=False
 CELULA: {"id": "132139", "colaborador_id": "902", "data": "2026-10-28", "trabalha": "True", "origem": "gerada", "escala_geradora_id": "1101", "dna": "{\"marcos\": {\"hf\": \"18:00\", \"hi\": \"08:00\", \"hfi\": \"13:00\", \"hii\": \"12:00\"}, \"lampadas\": [{\"hora\": \"08:00\", \"tipo\": \"E\", \"chave\": \"hi\", \"instante\": \"2026-10-28T08:00\"}, {\"hora\": \"12:00\", \"tipo\": \"S\", \"chave\": \"hii\", \"instante\": \"2026-10-28T12:00\"}, {\"hora\": \"13:00\", \"tipo\": \"E\", \"chave\": \"hfi\", \"instante\": \"2026-10-28T13:00\"}, {\"hora\": \"18:00\", \"tipo\": \"S\", \"chave\": \"hf\", \"instante\": \"2026-10-28T18:", "gerada_em": "2026-10-21 08:50:15.435733+00:00", "julgada_em": "2026-11-02 07:25:34.015313+00:00", "impressao": "96f692673e51b232b853c86945e57e6ef725ca1cab3957f800b4d2315f088d12", "veredito": "furo", "veredito_via": "cartorio", "veredito_em": "2026-10-28 21:07:38.775318+00:00", "protestos": "[\"FURO_PARCIAL\"]", "ata": "{\"orfas\": [], \"n_orfas\": 0, \"lampadas\": [{\"luz\": \"07:58\", \"hora\": \"08:00\", \"tipo\": \"E\", \"acesa\": true, \"tipo_real\": \"E\"}, {\"luz\": \"12:06\", \"hora\": \"12:00\", \"tipo\": \"S\", \"acesa\": true, \"tipo_real\": \"S\"}, {\"luz\": null, \"hora\": \"13:00\", \"tipo\": \"E\", \"acesa\": false, \"tipo_real\": null}, {\"luz\": \"18:07\", \"hora\": \"18:00\", \"tipo\": \"S\", \"acesa\": true, \"tipo_real\": \"S\"}], \"tipo_dia\": \"trabalho\", \"n_celulas\"", "tem_entrada": "True", "tem_saida": "True", "insumos_em": "2026-11-02 07:25:34.015313+00:00", "status_dia": "irregular", "status_rotulo": "Batida faltando, fora do horario previsto ou em ordem inesperada"}
 EC.marcos_do_dia=(datetime.time(8, 0), datetime.time(18, 0), datetime.time(12, 0), datetime.time(13, 0)) tipo_escala=5x2 · Seg/Ter/Qua/Qui/Sex 08:00-18:00
 JUSTIFICATIVAS data_turno=2026-10-28 n=0 []
 DIAPAGO versao=motor pago_do_dia_h=10.0 min=600 | {"id": "2378566", "colaborador_id": "902", "mes": "11", "ano": "2026", "data": "2026-10-28", "tipo": "dia", "versao": "motor", "horas_trabalhadas": "10.0", "horas_intra_indenizada": "1.0", "minutos_previstos": "540.0", "minutos_realizados": "540.0", "versao_motor": "legais|p5h=0|hr=0|fd=1", "evento_origem": "recalcular_fechamento_mes", "lavrado_em": "2026-11-18 10:02:49.144170+00:00"}
 DIAPAGO versao=oraculo pago_do_dia_h=10.0 min=600 | {"id": "2378590", "colaborador_id": "902", "mes": "11", "ano": "2026", "data": "2026-10-28", "tipo": "dia", "versao": "oraculo", "horas_trabalhadas": "10.0", "horas_intra_indenizada": "1.0", "minutos_previstos": "540.0", "minutos_realizados": "540.0", "evento_origem": "recalcular_fechamento_mes", "detalhe": "{\"calculador_nao_decide\": [\"horas_falta\", \"horas_reflexo_dsr\", \"saldo_banco_horas\"]}", "lavrado_em": "2026-11-18 10:02:49.362282+00:00"}
 TELA(dia dict): pago_h=10.0 veredito_dia='trabalhou' palavra_dia='' cor='' realizado_aberto=False realizado_h=9.2 minutos_realizados=549 previsto=[] ausencia=None falta_frase='falta entrada 13:00' falta_marcos=[{'tipo': 'E', 'hora': '13:00'}] rubricas={'lavrada': True, 'extras_50': 0.0, 'extras_100': 0.0, 'atraso': 0.0, 'antecipada': 0.0, 'noturnas': 0.0, 'intra': 1.0, 'trabalhadas': 10.0, 'minutos_extra': 0.0}
 TELA(dia dict) turnos_motor=[('28/10 07:58:46', '28/10 18:07:37', False, ['Intrajornada suprimida — 60min pagos como hora extra (Art.71 §4 CLT / Sumula 437 TST). Intra real: 0min.'])]
 PDF_LINHA: 28/10 qua | 08:00 18:00 | MARCACOES:E07:58 S12:06 S18:07 | HE fora da janela: 2 min antes do marco 08:00 — nao conta (Art. 4º §2º) | HE fora da janela: 7 min depois do marco 18:00 | 1h | 10h
============================================================
CASO C015 col89 08/11  (1.7s)
 ESPELHO n_marcacoes=4: 06:55 E orfa=True origem=app proc=None just= ; 12:32 S orfa=False origem=app proc=None just= ; 13:02 E orfa=False origem=app proc=None just= ; 14:00 S orfa=False origem=app proc=None just=
 APURAVEIS d-1 12h..d+1 12h n=5: 08/11 06:55:32 E pk=149114 orig=app itv=False apr=True tr= canal=apk perg=None ; 08/11 12:32:52 S pk=149266 orig=app itv=False apr=True tr= canal=apk perg=None ; 08/11 13:02:55 E pk=149285 orig=app itv=False apr=True tr= canal=apk perg=None ; 08/11 14:00:04 S pk=149307 orig=app itv=False apr=True tr= canal=apk perg=None ; 09/11 09:55:13 E pk=149917 orig=app itv=False apr=True tr= canal=apk perg=None
 RETRATADAS na janela n=0
 TURNO data_turno=2026-11-08 entrada=08/11 06:55:32 saida=08/11 14:00:04 aberto=False fim_fechado=08/11 14:00:04 batidas=06:55E 12:32S 13:02E 14:00S
   intervalos_do_turno=[('08/11 12:32:52', '08/11 13:02:55')]
 turnos de data_turno=2026-11-08 no pareamento da janela inteira: 1 (total na janela 24)
 REALIZADO(todos turnos)=(395, False, False, False, 0, False)
 REALIZADO(so turnos com saida)=(395, False, False, False, 0, False)
 borda_posicional=False
 CELULA: {"id": "123501", "colaborador_id": "89", "data": "2026-11-08", "trabalha": "True", "origem": "gerada", "escala_geradora_id": "1039", "dna": "{\"marcos\": {\"hf\": \"18:20\", \"hi\": \"10:00\", \"hfi\": \"13:00\", \"hii\": \"12:00\"}, \"lampadas\": [{\"hora\": \"10:00\", \"tipo\": \"E\", \"chave\": \"hi\", \"instante\": \"2026-11-08T10:00\"}, {\"hora\": \"12:00\", \"tipo\": \"S\", \"chave\": \"hii\", \"instante\": \"2026-11-08T12:00\"}, {\"hora\": \"13:00\", \"tipo\": \"E\", \"chave\": \"hfi\", \"instante\": \"2026-11-08T13:00\"}, {\"hora\": \"18:20\", \"tipo\": \"S\", \"chave\": \"hf\", \"instante\": \"2026-11-08T18:", "gerada_em": "2026-10-06 10:20:42.792209+00:00", "julgada_em": "2026-11-09 21:15:15.737185+00:00", "impressao": "8cd0ee03c41e7ad6fdf2fe26a5053e3235298638c77c8314f577ce6eb04eab5f", "veredito": "concorde", "veredito_via": "cartorio", "veredito_em": "2026-11-08 21:50:06.197830+00:00", "ata": "{\"orfas\": [\"06:55\"], \"n_orfas\": 1, \"lampadas\": [{\"luz\": \"12:32\", \"hora\": \"10:00\", \"tipo\": \"E\", \"acesa\": true, \"tipo_real\": \"S\"}, {\"luz\": \"13:02\", \"hora\": \"12:00\", \"tipo\": \"S\", \"acesa\": true, \"tipo_real\": \"E\"}, {\"luz\": \"14:00\", \"hora\": \"13:00\", \"tipo\": \"E\", \"acesa\": true, \"tipo_real\": \"S\"}, {\"luz\": null, \"hora\": \"18:20\", \"tipo\": \"S\", \"acesa\": false, \"tipo_real\": null}], \"tipo_dia\": \"trabalho\", \"n_c", "tem_entrada": "True", "insumos_em": "2026-11-09 21:15:15.737185+00:00", "status_dia": "ok", "status_rotulo": "Marco cumprido com batida — todas as batidas previstas registradas"}
 EC.marcos_do_dia=(datetime.time(10, 0), datetime.time(18, 20), datetime.time(12, 0), datetime.time(13, 0)) tipo_escala=ESCALA ARTES · 6x1 · 10:00-18:20
 JUSTIFICATIVAS data_turno=2026-11-08 n=0 []
 DIAPAGO versao=motor pago_do_dia_h=6.574583888888889 min=394 | {"id": "2383042", "colaborador_id": "89", "mes": "11", "ano": "2026", "data": "2026-11-08", "tipo": "dia", "versao": "motor", "horas_trabalhadas": "6.574583888888889", "horas_intra_indenizada": "0.49908749999999996", "minutos_previstos": "440.0", "minutos_realizados": "395.0", "versao_motor": "relogio|p5h=1|hr=0|fd=1", "evento_origem": "recalcular_fechamento_mes", "lavrado_em": "2026-11-18 10:03:36.467889+00:00"}
 DIAPAGO versao=oraculo pago_do_dia_h=6.5745838888888875 min=394 | {"id": "2383073", "colaborador_id": "89", "mes": "11", "ano": "2026", "data": "2026-11-08", "tipo": "dia", "versao": "oraculo", "horas_trabalhadas": "6.5745838888888875", "horas_intra_indenizada": "0.49908749999999996", "minutos_previstos": "440.0", "minutos_realizados": "395.0", "evento_origem": "recalcular_fechamento_mes", "detalhe": "{\"calculador_nao_decide\": [\"horas_falta\", \"horas_reflexo_dsr\", \"saldo_banco_horas\"]}", "lavrado_em": "2026-11-18 10:03:36.699900+00:00"}
 TELA(dia dict): pago_h=6.57 veredito_dia='trabalhou' palavra_dia='' cor='' realizado_aberto=False realizado_h=6.6 minutos_realizados=395 previsto=[] ausencia=None falta_frase='falta saida 18:20' falta_marcos=[{'tipo': 'S', 'hora': '18:20'}] rubricas={'lavrada': True, 'extras_50': 0.0, 'extras_100': 0.0, 'atraso': 0.0, 'antecipada': 0.0, 'noturnas': 0.0, 'intra': 0.49908749999999996, 'trabalhadas': 6.574583888888889, 'minutos_extra': 0.0}
 TELA(dia dict) turnos_motor=[('08/11 06:55:32', '08/11 14:00:04', False, ['Intrajornada suprimida — 29min pagos como hora extra (Art.71 §4 CLT / Sumula 437 TST). Intra real: 30min.'])]
 PDF_LINHA: 08/11 dom | 10:00 18:20 | MARCACOES:S12:32 E13:02 S14:00 + *06:55 |  |  | 0h30 | 6h34
============================================================
CASO C015 col89 15/11  (0.0s)
 ESPELHO n_marcacoes=4: 06:55 E orfa=True origem=app proc=None just= ; 12:00 S orfa=False origem=app proc=None just= ; 12:32 E orfa=False origem=app proc=None just= ; 14:03 S orfa=False origem=app proc=None just=
 APURAVEIS d-1 12h..d+1 12h n=8: 14/11 12:01:43 S pk=154912 orig=app itv=False apr=True tr= canal=apk perg=None ; 14/11 13:00:09 E pk=154943 orig=app itv=False apr=True tr= canal=apk perg=None ; 14/11 18:14:16 S pk=155150 orig=app itv=False apr=True tr= canal=apk perg=None ; 15/11 06:55:08 E pk=155519 orig=app itv=False apr=True tr= canal=apk perg=None ; 15/11 12:00:14 S pk=155653 orig=app itv=False apr=True tr= canal=apk perg=None ; 15/11 12:32:30 E pk=155680 orig=app itv=False apr=True tr= canal=apk perg=None ; 15/11 14:03:44 S pk=155731 orig=app itv=False apr=True tr= canal=apk perg=None ; 16/11 09:55:05 E pk=156388 orig=app itv=False apr=True tr= canal=apk perg=None
 RETRATADAS na janela n=0
 TURNO data_turno=2026-11-15 entrada=15/11 06:55:08 saida=15/11 14:03:44 aberto=False fim_fechado=15/11 14:03:44 batidas=06:55E 12:00S 12:32E 14:03S
   intervalos_do_turno=[('15/11 12:00:14', '15/11 12:32:30')]
 turnos de data_turno=2026-11-15 no pareamento da janela inteira: 1 (total na janela 24)
 REALIZADO(todos turnos)=(396, False, False, False, 0, False)
 REALIZADO(so turnos com saida)=(396, False, False, False, 0, False)
 borda_posicional=False
 CELULA: {"id": "123508", "colaborador_id": "89", "data": "2026-11-15", "trabalha": "True", "origem": "gerada", "escala_geradora_id": "1039", "dna": "{\"marcos\": {\"hf\": \"18:20\", \"hi\": \"10:00\", \"hfi\": \"13:00\", \"hii\": \"12:00\"}, \"lampadas\": [{\"hora\": \"10:00\", \"tipo\": \"E\", \"chave\": \"hi\", \"instante\": \"2026-11-15T10:00\"}, {\"hora\": \"12:00\", \"tipo\": \"S\", \"chave\": \"hii\", \"instante\": \"2026-11-15T12:00\"}, {\"hora\": \"13:00\", \"tipo\": \"E\", \"chave\": \"hfi\", \"instante\": \"2026-11-15T13:00\"}, {\"hora\": \"18:20\", \"tipo\": \"S\", \"chave\": \"hf\", \"instante\": \"2026-11-15T18:", "gerada_em": "2026-10-06 10:20:42.801509+00:00", "julgada_em": "2026-11-16 21:15:34.708287+00:00", "impressao": "36a7df7a96d4fce7b201973b1c5796436395ec251b17330b70c43b4453ed6889", "veredito": "concorde", "veredito_via": "cartorio", "veredito_em": "2026-11-15 21:50:05.107581+00:00", "ata": "{\"orfas\": [\"06:55\"], \"n_orfas\": 1, \"lampadas\": [{\"luz\": \"12:00\", \"hora\": \"10:00\", \"tipo\": \"E\", \"acesa\": true, \"tipo_real\": \"S\"}, {\"luz\": \"12:32\", \"hora\": \"12:00\", \"tipo\": \"S\", \"acesa\": true, \"tipo_real\": \"E\"}, {\"luz\": \"14:03\", \"hora\": \"13:00\", \"tipo\": \"E\", \"acesa\": true, \"tipo_real\": \"S\"}, {\"luz\": null, \"hora\": \"18:20\", \"tipo\": \"S\", \"acesa\": false, \"tipo_real\": null}], \"tipo_dia\": \"trabalho\", \"n_c", "tem_entrada": "True", "insumos_em": "2026-11-16 21:15:34.708287+00:00", "status_dia": "ok", "status_rotulo": "Marco cumprido com batida — todas as batidas previstas registradas"}
 EC.marcos_do_dia=(datetime.time(10, 0), datetime.time(18, 20), datetime.time(12, 0), datetime.time(13, 0)) tipo_escala=ESCALA ARTES · 6x1 · 10:00-18:20
 JUSTIFICATIVAS data_turno=2026-11-15 n=0 []
 DIAPAGO versao=motor pago_do_dia_h=6.605381666666667 min=396 | {"id": "2383049", "colaborador_id": "89", "mes": "11", "ano": "2026", "data": "2026-11-15", "tipo": "dia", "versao": "motor", "horas_trabalhadas": "6.605381666666667", "horas_extras_100": "6.605381666666667", "horas_extras_100_feriado": "6.605381666666667", "horas_intra_indenizada": "0.4620066666666666", "minutos_previstos": "440.0", "minutos_realizados": "396.0", "versao_motor": "relogio|p5h=1|hr=0|fd=1", "evento_origem": "recalcular_fechamento_mes", "lavrado_em": "2026-11-18 10:03:36.468043+00:00"}
 DIAPAGO versao=oraculo pago_do_dia_h=6.605381666666667 min=396 | {"id": "2383080", "colaborador_id": "89", "mes": "11", "ano": "2026", "data": "2026-11-15", "tipo": "dia", "versao": "oraculo", "horas_trabalhadas": "6.605381666666667", "horas_extras_100": "6.605381666666667", "horas_extras_100_feriado": "6.605381666666667", "horas_intra_indenizada": "0.4620066666666666", "minutos_previstos": "440.0", "minutos_realizados": "396.0", "evento_origem": "recalcular_fechamento_mes", "detalhe": "{\"calculador_nao_decide\": [\"horas_falta\", \"horas_reflexo_dsr\", \"saldo_banco_horas\"]}", "lavrado_em": "2026-11-18 10:03:36.700054+00:00"}
 TELA(dia dict): pago_h=6.61 veredito_dia='trabalhou' palavra_dia='' cor='' realizado_aberto=False realizado_h=6.6 minutos_realizados=396 previsto=[] ausencia=None falta_frase='falta saida 18:20' falta_marcos=[{'tipo': 'S', 'hora': '18:20'}] rubricas={'lavrada': True, 'extras_50': 0.0, 'extras_100': 6.605381666666667, 'atraso': 0.0, 'antecipada': 0.0, 'noturnas': 0.0, 'intra': 0.4620066666666666, 'trabalhadas': 6.605381666666667, 'minutos_extra': 396.3229}
 TELA(dia dict) turnos_motor=[('15/11 06:55:08', '15/11 14:03:44', False, ['Intrajornada suprimida — 27min pagos como hora extra (Art.71 §4 CLT / Sumula 437 TST). Intra real: 32min.', 'Feriado em 15/11/2026 — todas as 6.6h pagas em dobro (Sumula 146 TST).', 'Feriado em 15/11/2026 — todas as 6.6h pagas em dobro (Sumula 146 TST).'])]
 PDF_LINHA: 15/11 dom ★ | 10:00 18:20 | MARCACOES:S12:00 E12:32 S14:03 + *06:55 |  | 6h36 | 0h28 | 6h37
============================================================
CASO C017 col32 22/10  (1.2s)
 ESPELHO n_marcacoes=3: 07:00 E orfa=False origem=app proc=None just= ; 12:26 S orfa=False origem=app proc=None just= ; 19:00 S orfa=False origem=app proc=None just=
 APURAVEIS d-1 12h..d+1 12h n=3: 22/10 07:00:23 E pk=133877 orig=app itv=False apr=True tr= canal=None perg=None ; 22/10 12:26:07 S pk=134164 orig=app itv=False apr=True tr= canal=None perg=None ; 22/10 19:00:23 S pk=134711 orig=app itv=False apr=True tr= canal=None perg=None
 RETRATAD

[truncado em 20000 de 68679 caracteres]

## 18/11 10:39 · sessao 5eee9c2e3d72 · E4C-F4 · verificador · origem=Code

**1. A nova asserção é verdadeira por análise do código; execução do teste [não verificado].** A pauta prevista no segundo comando contém: [nao verificado]

- `(self.saida.pk, 'sem dia do fato')`
- `(self.volta.pk, '10/2026')`

Os valores numéricos dos PKs são atribuídos pelo banco de teste e estão **[não verificado]**. A fixture cria saída e volta para o mesmo colaborador e com data inicial de 25/09/2026 ([test_bug_par_projetado.py:28](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_projetado.py:28), [test_bug_par_projetado.py:75](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_projetado.py:75), [test_bug_par_projetado.py:87](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_projetado.py:87)).

O caminho é:

1. `_par_163` coloca ambas em U60 e U163; U103 permanece vazio desde a inicialização ([test_bug_par_volta.py:25](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:25), [test_bug_par_volta.py:501](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:501)).
2. O teste entrega 10/2026, confirma ambas nesse balde e depois altera somente a data da saída para `None` ([test_bug_par_volta.py:496](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:496), [test_bug_par_volta.py:605](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:605), [test_bug_par_volta.py:616](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:616)).
3. A testemunha consulta a própria pergunta; nesse caminho, não procura a irmã. O contador põe a saída sem dia em `bug_par_103_sem_dia` e a volta entregue em `bug_par_103_pauta_dp` ([validacao.py:209](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/services/validacao.py:209), [materializacao.py:341](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/services/materializacao.py:341)).
4. Os laços anteriores do comando não incluem essas perguntas: U103 está vazio e o outro laço exclui U163. O comando acrescenta os dois baldes, associa `col<id>` a cada PK e imprime a pauta; a asserção compara apenas PK e rótulo, ordenados ([bug_par_103.py:54](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/management/commands/bug_par_103.py:54), [bug_par_103.py:73](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/management/commands/bug_par_103.py:73), [bug_par_103.py:81](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/management/commands/bug_par_103.py:81), [bug_par_103.py:143](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/management/commands/bug_par_103.py:143), [test_bug_par_volta.py:622](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:622)).

**2. Os três alvos são únicos na respectiva fonte:** contei uma ocorrência textual de cada alvo, nas linhas [bug_par_103.py:148](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/management/commands/bug_par_103.py:148), [bug_par_103.py:86](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/management/commands/bug_par_103.py:86) e [materializacao.py:343](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/services/materializacao.py:343). Os três apontam para o mesmo teste ([bug_par_mutantes.py:113](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:113)).

Por análise estática:

- `u163_l5_fora_da_pauta` deixa a segunda pauta só com `(self.volta.pk, '10/2026')`; a comparação da linha 626 deve falhar por asserção ([bug_par_mutantes.py:120](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:120), [test_bug_par_volta.py:626](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:626)).
- `u163_fora_da_pauta` elimina a inclusão dos baldes e deve falhar na primeira comparação da pauta, antes da alteração da data ([bug_par_mutantes.py:116](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:116), [test_bug_par_volta.py:611](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:611)).
- `u163_l1_no_pendente` impede a classificação L1/L5 nesse ramo; após a entrega, as duas passam para `outras`, compondo pendentes, e a asserção de zero deve falhar ([bug_par_mutantes.py:113](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:113), [materializacao.py:352](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/services/materializacao.py:352), [test_bug_par_volta.py:605](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/test_bug_par_volta.py:605)).

A execução efetiva e a confirmação de `falhas > 0`, `erros = 0` para cada mutante ficam **[não verificado]**: são mutantes de integração, cujo executor usa o runner Django de testes ([bug_par_mutantes.py:163](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:163), [bug_par_mutantes.py:170](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:170), [bug_par_mutantes.py:237](/home/ronald/codex_ws/748868e074d9a050fa4d57efe71ae30f9685086a/app/chamados/tests/bug_par_mutantes.py:237)).

## 18/11 10:37 · sessao 9a9ccfe2565c · SONDA-DIA-IMPAR-1811 · sonda · origem=Code

Há traceback em `/app/relatorios/pdf_espelho.py:722`; faltam `FIM_SONDA_CASOS` e `rc_sombra=0` [nao verificado].

```text
SONDA achadoA_casos agora=18/11 10:37:10 ultima_batida_sombra=18/11 03:55:21
Traceback (most recent call last):
  File "/app/manage.py", line 22, in <module>
    main()
    ~~~~^^
  File "/app/manage.py", line 18, in main
    execute_from_command_line(sys.argv)
    ~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^
  File "/usr/local/lib/python3.13/site-packages/django/core/management/__init__.py", line 442, in execute_from_command_line
    utility.execute()
    ~~~~~~~~~~~~~~~^^
  File "/usr/local/lib/python3.13/site-packages/django/core/management/__init__.py", line 436, in execute
    self.fetch_command(subcommand).run_from_argv(self.argv)
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^
  File "/usr/local/lib/python3.13/site-packages/django_tenants/management/commands/tenant_command.py", line 48, in run_from_argv
    klass.run_from_argv(args)
    ~~~~~~~~~~~~~~~~~~~^^^^^^
  File "/usr/local/lib/python3.13/site-packages/django/core/management/base.py", line 420, in run_from_argv
    self.execute(*args, **cmd_options)
    ~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.13/site-packages/django/core/management/base.py", line 464, in execute
    output = self.handle(*args, **options)
  File "/usr/local/lib/python3.13/site-packages/django/core/management/commands/shell.py", line 247, in handle
    exec(options["command"], {**globals(), **self.get_namespace(**options)})
    ~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "<string>", line 1, in <module>
  File "<string>", line 72, in <module>
  File "/app/relatorios/pdf_espelho.py", line 722, in _build_story
    _dir = Paragraph('Emitido %s%s' % (dados['gerado_em_str'], ('<br/><font color="#9a3412">' + _anom_txt + '</font>') if _anom_txt else ''), style_meta)
                                       ~~~~~^^^^^^^^^^^^^^^^^
KeyError: 'gerado_em_str'
rc_sombra=1
```

## 18/11 10:34 · sessao f9447a60588b · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

**Passo 1 — cópia do dado raspado**

```text
=== SOMBRA (sombra_db, leitor) 18/11 10:33:03 ===
sonda_leitura: sonda_i4.py [dbc76cca2877ec2c] no cpuset 4-7, lendo sombra como leitor (LEITURA)

(1, 1, 5, 1367)
(1, 2, 5, 1758)
(1, 3, 5, 1264)
(1, 4, 5, 17)
(1, 5, 5, 500)
(1, 6, 5, 737)
(1, 8, 5, 65)
(2, 1, 0, 0)
(2, 2, 0, 0)
(2, 3, 0, 0)
(2, 4, 0, 0)
(2, 5, 5708, 244)
(3, 5, '1_ate_1min', 1)
(3, 5, '3_ate_1dia', 4)
(3, 5, '4_mais_1dia', 239)
(5, 'classe2_sem_log_auditoria', 0)
(6, 'lavras_total', 47318)
(7, 'candidata_A_foto_historica_move', 0, 'marcos_do_lote nao le DP (veredito_lavrado.py:151-208), lavras que mudam = 0 por construcao')
(8, 'candidata_B_provada_classe2', 0)
(9, 'cota_classe1', 0)
(9, 'cota_classe2', 0)
(9, 'cota_classe4', 0)
(9, 'cota_qualquer_1_2_4', 0)
(10, 'total_dp', 5708)
rc_sombra=0
```

Linhas copiadas: **28**, incluindo a linha em branco. [não verificado]

**Passo 2 — leitura da sonda**

A classe 2 foi definida nesta sonda como ato `resposta` com log `carimbar_resposta_pela_trilha` na pergunta; o código registra esse evento após criar a decisão. ([disputa_emissao.py:1685](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1685), [disputa_emissao.py:1701](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1701))

| Classe 2 | Resultado informado |
|---|---:|
| Total | 0 [não verificado] |
| Inversões por pk | 0 [não verificado] |
| Faixas de lag inserção − `ato_em` do log | Nenhuma linha de faixa para a classe 2 [não verificado] |
| Sem log de auditoria | 0 [não verificado] |

A classe 5 contém o restante, distribuído pelos **códigos de ato tal como aparecem na sonda**; a correspondência entre esses números e os nomes dos atos não consta do dado fornecido. [não verificado]

| Ato | Classe 5: quantidade |
|---:|---:|
| 1 | 1.367 [não verificado] |
| 2 | 1.758 [não verificado] |
| 3 | 1.264 [não verificado] |
| 4 | 17 [não verificado] |
| 5 | 500 [não verificado] |
| 6 | 737 [não verificado] |
| 8 | 65 [não verificado] |
| **Total** | **5.708** [não verificado] |

A linha 2 informa 244 inversões para a classe 5; as linhas 3 informam 1 na faixa `1_ate_1min`, 4 em `3_ate_1dia` e 239 em `4_mais_1dia`. A definição executada dessas faixas não está disponível na cópia apresentada. [não verificado]

As classes 1, 3 e 4 por prefixo de motivo **não são mensuráveis nesta sombra**, pois `ato_motivo` é mascarado; sua medição por motivo fica para produção. O código contém escritores com motivos relevantes para essa distinção. ([sombra_regras_pessoais.py:142](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra_regras_pessoais.py:142), [disputa_emissao.py:1679](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1679), [models.py:4119](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4119), [backfill_decisoes_pergunta.py:323](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_decisoes_pergunta.py:323))

**Passo 3 — candidatas e limite**

- **A — foto histórica:** a sonda informa **0 lavras movidas**, por construção; `marcos_do_lote` lê marcos de batidas, células e perguntas, sem ler `DecisaoPergunta`. Isso descreve essa candidata, não uma medição de inserções posteriores. ([veredito_lavrado.py:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:151))
- **B — provadas pela classe 2:** a sonda informa **0 lavras movidas provadas**, no critério de inserção posterior a `julgado_em`; a classe 2 também tem total 0 na saída. Isso não mede as demais classes. [não verificado]
- **Cota superior, linha 9:** a saída informa **0** para cada uma das classes 1, 2 e 4 e **0** para a união `qualquer_1_2_4`. É o limite reportado pela sonda, não uma contagem de movimentos provados; sua definição de cálculo não está no dado fornecido e as classes por motivo estão mascaradas na sombra. [não verificado]

Falta a coluna de **instante de inserção** em `DecisaoPergunta` para comparar diretamente esse instante com `julgado_em`; o modelo declara `ato_em` como relógio do ato. ([models.py:4019](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4019), [models.py:4051](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4051))

**Passo 4 — pergunta ao Ronald**

Ronald, a sombra informa 0 lavras movidas para A, 0 provadas para B e cota 0 na linha 9; as classes 1, 3 e 4 por motivo estão mascaradas. [não verificado]  
Como deseja tratar esses números na decisão do item 4, considerando a medição por motivo em produção e a ausência de instante de inserção em `DecisaoPergunta`? ([sombra_regras_pessoais.py:142](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra_regras_pessoais.py:142), [models.py:4051](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4051))

## 18/11 10:32 · sessao d60dbd8c2c65 · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
=== SOMBRA (sombra_db, leitor) 18/11 10:28:42 ===
sonda_leitura: sonda_i5.py [9504050b83aadff2] no cpuset 4-7, lendo sombra como leitor (LEITURA)

agora 2026-11-18T10:28:45.391321-03:00 SILENCIO 11 REABERTURA 29
SILENCIO pk16360 col42 dia=2026-08-25 no_app_hoje=True razao=[] acoes=[] | HOJE rota=porta_acao fio=admin nao_sei=False fila=0 | APOS_F5 rota=porta_acao fio=admin nao_sei=False fila=0 | porta_da_tarefa_hoje=ok
SILENCIO pk24694 col925 dia=2026-09-05 no_app_hoje=True razao=['silencio_decidido'] acoes=[('resolver_dia', 'admin')] | HOJE rota=porta_acao fio=admin nao_sei=False fila=0 | APOS_F5 rota=porta_acao fio=admin nao_sei=False fila=0 | porta_da_tarefa_hoje=ok
SILENCIO pk29676 col925 dia=2026-09-05 no_app_hoje=True razao=['silencio_decidido'] acoes=[] | HOJE rota=porta_acao fio=admin nao_sei=False fila=0 | APOS_F5 rota=porta_acao fio=admin nao_sei=False fila=0 | porta_da_tarefa_hoje=ok
SILENCIO pk31420 col925 dia=2026-09-05 no_app_hoje=True razao=['silencio_decidido'] acoes=[] | HOJE rota=porta_acao fio=admin nao_sei=False fila=0 | APOS_F5 rota=porta_acao fio=admin nao_sei=False fila=0 | porta_da_tarefa_hoje=ok
SILENCIO pk31421 col925 dia=2026-09-05 no_app_hoje=True razao=['silencio_decidido'] acoes=[] | HOJE rota=porta_acao fio=admin nao_sei=False fila=0 | APOS_F5 rota=porta_acao fio=admin nao_sei=False fila=0 | porta_da_tarefa_hoje=ok
SILENCIO pk43761 col510 dia=2026-10-14 no_app_hoje=True razao=[] acoes=[] | HOJE rota=None fio=nada nao_sei=False fila=0 | APOS_F5 rota=None fio=nada nao_sei=False fila=0 | porta_da_tarefa_hoje=None
SILENCIO pk46070 col510 dia=2026-10-19 no_app_hoje=True razao=[] acoes=[] | HOJE rota=None fio=nada nao_sei=False fila=0 | APOS_F5 rota=None fio=nada nao_sei=False fila=0 | porta_da_tarefa_hoje=None
SILENCIO pk46440 col510 dia=2026-10-20 no_app_hoje=True razao=[] acoes=[] | HOJE rota=None fio=nada nao_sei=False fila=0 | APOS_F5 rota=None fio=nada nao_sei=False fila=0 | porta_da_tarefa_hoje=None
SILENCIO pk53787 col42 dia=2026-11-04 no_app_hoje=True razao=['silencio_decidido'] acoes=[] | HOJE rota=None fio=nada nao_sei=False fila=0 | APOS_F5 rota=None fio=nada nao_sei=False fila=0 | porta_da_tarefa_hoje=None
SILENCIO pk53837 col42 dia=2026-11-04 no_app_hoje=True razao=['silencio_decidido'] acoes=[] | HOJE rota=None fio=nada nao_sei=False fila=0 | APOS_F5 rota=None fio=nada nao_sei=False fila=0 | porta_da_tarefa_hoje=None
SILENCIO pk53875 col42 dia=2026-11-04 no_app_hoje=True razao=['silencio_decidido'] acoes=[] | HOJE rota=None fio=nada nao_sei=False fila=0 | APOS_F5 rota=None fio=nada nao_sei=False fila=0 | porta_da_tarefa_hoje=None
SILENCIO TOTAL 11
SILENCIO    ('apos', 'porta_acao', 'admin') 5
SILENCIO    ('apos', None, 'nada') 6
SILENCIO    ('hoje', 'porta_acao', 'admin') 5
SILENCIO    ('hoje', None, 'nada') 6
SILENCIO SEM_PORTA hoje (rota None): 6 | apos F5 (rota None): 6
REABERTURA pk21482 col822 dia=2026-08-28 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=None fio=nao_sei nao_sei=True fila=0 | APOS_F5 rota=None fio=nao_sei nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk21483 col822 dia=2026-08-28 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=None fio=nao_sei nao_sei=True fila=0 | APOS_F5 rota=None fio=nao_sei nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk21484 col822 dia=2026-08-28 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=None fio=nao_sei nao_sei=True fila=0 | APOS_F5 rota=None fio=nao_sei nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk32973 col822 dia=2026-09-18 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_app fio=colab nao_sei=True fila=0 | APOS_F5 rota=porta_app fio=colab nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk36273 col446 dia=2026-09-25 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_pergunta fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_pergunta fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk36383 col169 dia=2026-09-25 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk39989 col836 dia=2026-10-04 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_app fio=colab nao_sei=True fila=0 | APOS_F5 rota=porta_app fio=colab nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk42735 col836 dia=2026-10-11 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_app fio=colab nao_sei=True fila=0 | APOS_F5 rota=porta_app fio=colab nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk45597 col836 dia=2026-10-18 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_app fio=colab nao_sei=True fila=0 | APOS_F5 rota=porta_app fio=colab nao_sei=True fila=0 | porta_da_tarefa_hoje=ok
REABERTURA pk45691 col848 dia=2026-10-18 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk45776 col363 dia=2026-10-19 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk47212 col908 dia=2026-10-22 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_pergunta fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_pergunta fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk49033 col822 dia=2026-10-22 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk49034 col822 dia=2026-10-22 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk49035 col822 dia=2026-10-22 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk49036 col822 dia=2026-10-22 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk49668 col247 dia=2026-10-24 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk50478 col148 dia=2026-10-26 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk51099 col902 dia=2026-10-28 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk51329 col363 dia=2026-10-23 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk51667 col822 dia=2026-10-29 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk51685 col822 dia=2026-10-29 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk51763 col822 dia=2026-10-29 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk53093 col902 dia=2026-11-02 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk53139 col902 dia=2026-11-02 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk53172 col902 dia=2026-11-02 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk53250 col902 dia=2026-11-02 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk53554 col848 dia=2026-11-03 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA pk55305 col1020 dia=2026-11-08 no_app_hoje=False razao=['passivo_sem_linha'] acoes=[] | HOJE rota=porta_fila fio=admin nao_sei=True fila=1 | APOS_F5 rota=porta_fila fio=admin nao_sei=True fila=1 | porta_da_tarefa_hoje=ok
REABERTURA TOTAL 29
REABERTURA    ('apos', 'porta_app', 'colab') 4
REABERTURA    ('apos', 'porta_fila', 'admin') 20
REABERTURA    ('apos', 'porta_pergunta', 'admin') 2
REABERTURA    ('apos', None, 'nao_sei') 3
REABERTURA    ('hoje', 'porta_app', 'colab') 4
REABERTURA    ('hoje', 'porta_fila', 'admin') 20
REABERTURA    ('hoje', 'porta_pergunta', 'admin') 2
REABERTURA    ('hoje', None, 'nao_sei') 3
REABERTURA SEM_PORTA hoje (rota None): 3 | apos F5 (rota None): 3
rc_sombra=0
```

linhas copiadas: 61, incluindo a linha em branco.

Os números abaixo são contagens do **dado fornecido acima**; a execução e a origem da sonda não foram verificadas de forma independente. [não verificado]

| Classe | Cenário | Com dono e rota | Sem porta | Total |
|---|---|---:|---:|---:|
| SILENCIO | HOJE | 5 admin, `porta_acao` | 6 `nada` | 11 |
| SILENCIO | APOS_F5 | 5 admin, `porta_acao` | 6 `nada` | 11 |
| REABERTURA | HOJE | 4 colab, `porta_app`; 22 admin, 20 `porta_fila` e 2 `porta_pergunta` | 3 `nao_sei` | 29 |
| REABERTURA | APOS_F5 | 4 colab, `porta_app`; 22 admin, 20 `porta_fila` e 2 `porta_pergunta` | 3 `nao_sei` | 29 |

A tabela conta a **rota da tarefa do dia**, conforme impressa pela sonda, inclusive quando `nao_sei=True`; esse indicador pode coexistir com uma rota. Um envelope de `porta_da_tarefa` com rota `None` também não é uma rota publicada: por isso os três casos `nao_sei` contam como sem porta, embora a sonda imprima `porta_da_tarefa_hoje=ok`. [app/chamados/services/tarefa_do_dia.py:718](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:718); [não verificado]

**Sem porta nos dois cenários**, segundo o dado fornecido:

- SILENCIO: pk43761, pk46070, pk46440, pk53787, pk53837, pk53875.
- REABERTURA: pk21482, pk21483, pk21484. [não verificado]

Nenhuma das 40 perguntas passa a ter dono que não tinha, nem deixa de ter: a rota e o fio impressos são iguais nos dois cenários para cada pk. A razão exata de cada uma das cinco portas administrativas de SILENCIO não é toda demonstrada pelas ações da instância impressa; a tarefa agrega ações de todas as instâncias e também pode receber `resolver_dia` da autoridade do calendário. Para pk24694 essa ação aparece na própria saída; para as demais, sua origem específica é [não verificado]. [app/chamados/services/tarefa_do_dia.py:611](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:611); [app/chamados/services/tarefa_do_dia.py:643](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:643).

**As 29 REABERTURA não mostram ação `responder` na instância da pergunta.** Todas imprimem `passivo_sem_linha` e `acoes=[]`: em `_pela_decisao`, a verificação de passivo vem antes da regra que, para uma REABERTURA não interceptada, retornaria `responder`. O código exige tanto a pergunta em `_pendentes_app` quanto uma ação `responder` do colaborador naquela instância para `colab_tem_porta`. Assim, colocar essas perguntas no conjunto não cria essa ação. As quatro rotas `porta_app` publicadas pertencem à tarefa do dia; qual outra instância as origina não aparece no recorte. [app/chamados/services/tarefa_do_dia.py:154](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:154); [app/chamados/services/tarefa_do_dia.py:163](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:163); [app/chamados/services/tarefa_do_dia.py:657](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:657); [não verificado]

Para SILENCIO, `_pela_decisao` retorna `silencio_decidido` com ações vazias, mas isso **não fecha necessariamente a porta da tarefa inteira**: o código exclui a pergunta encerrada de `portas_pergunta`, enquanto a âncora ainda pode entrar em `portas` se estiver na fila, sujeita à seleção por verbo; outras ações também podem dar dono ao dia. No dado, os 11 imprimem `fila=0`, portanto ele não demonstra uma âncora de fila alcançando esses casos. A ação `resolver_dia` impressa em pk24694 é compatível com o acréscimo posterior pela tarefa. [app/chamados/services/tarefa_do_dia.py:176](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:176); [app/chamados/services/tarefa_do_dia.py:643](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:643); [app/chamados/services/tarefa_do_dia.py:690](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:690); [não verificado]

O contrafactual é **fiel ao efeito informado sobre a pertinência dessas 40 perguntas em `_pendentes_app`**, condicionado às premissas fornecidas de que estão vivas e de que a F5 mudaria suas partições como descrito: `_ler_vida` toma os pendentes da partição e ainda filtra por `pergunta_viva`. Ele não executa a F5 real nem comprova seus demais efeitos, a partição de outras perguntas, mudanças em outras entradas da carga ou o comportamento de outras telas. O brief F5 e o código da sonda não estavam disponíveis na cópia; a implementação efetiva e a fidelidade integral são [não verificado]. [app/chamados/services/tarefa_do_dia.py:530](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:530); [app/chamados/services/pergunta_no_app.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/pergunta_no_app.py:76).

Pergunta para Ronald: **“Avaliamos a F5 sabendo que, nesta sonda, os 11 SILENCIO permanecem em 5 com dono e 6 sem porta e as 29 REABERTURA em 26 com dono e 3 sem porta, sem mudança de dono em nenhum dos 40 casos?”** [não verificado]

## 18/11 10:31 · sessao 6596f28a6d0e · RESPOSTAS-DE-LEI-1811-1010 · sonda · origem=Code

```text
---- DADO RASPADO (sonda/anexos) ----
=== SOMBRA (sombra_db, leitor) 18/11 10:30:44 ===
sonda_leitura: sonda_i4.py [cae9871ac1d5b64e] no cpuset 4-7, lendo sombra como leitor (LEITURA)

(1, 1, 5, 1367)
(1, 2, 5, 1758)
(1, 3, 5, 1264)
(1, 4, 5, 17)
(1, 5, 5, 500)
(1, 6, 5, 737)
(1, 8, 5, 65)
(2, 1, 0, 0)
(2, 2, 0, 0)
(2, 3, 0, 0)
(2, 4, 0, 0)
(2, 5, 5708, 244)
(3, 5, '1_ate_1min', 1)
(3, 5, '3_ate_1dia', 4)
(3, 5, '4_mais_1dia', 239)
(5, 'classe2_sem_log_auditoria', 0)
(6, 'lavras_total', 47318)
(7, 'candidata_A_foto_historica_move', 0, 'marcos_do_lote nao le DP (veredito_lavrado.py:151-208), lavras que mudam = 0 por construcao')
(8, 'candidata_B_provada_classe2', 0)
(9, 'cota_classe1', 0)
(9, 'cota_classe2', 0)
(9, 'cota_classe4', 0)
(9, 'cota_qualquer_1_2_4', 0)
(10, 'total_dp', 5708)
rc_sombra=0
---- FIM DO DADO ----
```

linhas copiadas: 30, incluindo delimitadores e linha vazia.

Os números abaixo são a transcrição e operações aritméticas sobre o dado fornecido; a execução e o conteúdo da sonda não são verificáveis nesta cópia: **[não verificado]**.

| Escritor / classe de motivo | Total DP | `ato_em` anterior à inserção provada pelo log | Inversões por pk — **proxy** | O que não se mede |
|---|---:|---:|---:|---|
| 1 — projeção sem linha | 0 | Não medido | 0 | Inserção real e atraso; a projeção fornece `ato_em` histórico ([models.py:4122](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4122)). |
| 2 — pela trilha | 0 | **0** | 0 | Não há linhas para medir atraso ou faixas; sem log: 0. O escritor grava a DP e depois emite o evento ([disputa_emissao.py:1685](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1685)). |
| 3 — resposta automática | 0 | Não medido | 0 | Inserção real e atraso ([disputa_emissao.py:1655](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1655)). |
| 4 — backfill E1 de silêncio | 0 | Não medido | 0 | Inserção real e atraso; o backfill usa `materializada_em` ([backfill_decisoes_pergunta.py:323](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_decisoes_pergunta.py:323)). |
| 5 — demais | 5.708 | Não medido | 244 | Inserção real e atraso por log não foram medidos para esta classe; ela reúne vários escritores, entre eles [resposta](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1525), [reabertura/recusa](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1632), [validação](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:173), [devolução](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:279), [silêncio](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:2013), [retratação](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:2145) e [absorção](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1048). |

A soma é **5.708 = `total_dp`**. Das 244 inversões por pk, 1 está em até 1 minuto, 4 na faixa `3_ate_1dia` e 239 em mais de 1 dia; inversão é comparação entre `ato_em` e atos de pk menor, não prova do instante de inserção. **[não verificado: saída da sonda fornecida]**

Das **47.318 lavras** do recorte informado **[não verificado: saída da sonda fornecida]**:

- **A — manter a foto histórica:** **0 lavras movidas por DP, por construção**; `marcos_do_lote` não consulta DP ([veredito_lavrado.py:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:151)). O efeito de colocar na F3 somente o marcador do placar é **[não verificado]**.
- **B — marco por `Max(pk)`/`Count` de DP:** **0 lavras provadas** pela classe 2 e **cota do recorte 1/2/4 = 0**; o número total de lavras que B moveria, seja com `ato_em <= ate`, seja sem filtro, é **[não verificado]**. O marco atual e sua comparação com `julgado_em` estão em [veredito_lavrado.py:201](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:201) e [veredito_lavrado.py:230](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:230).

A cota **0** conta lavras com DP do mesmo colaborador nas classes 1, 2 ou 4 e `ato_em <= julgado_em`, independentemente de se provar inserção posterior; ela não cobre a classe 5 nem estabelece o efeito total de B; para verificar a ordem de inserção faltaria uma coluna de criação em `DecisaoPergunta`, junto aos campos em [models.py:4051](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4051), ou um snapshot histórico comparável **[não verificado para os registros existentes]**.

Ronald, você escolhe **A**, manter a foto histórica e usar na F3 só o marcador do placar — **0 lavras movidas por DP por construção** ([veredito_lavrado.py:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:151)) — ou **B**, acrescentar marco `Max(pk)`/`Count` das DP, com `ato_em <= ate` ou sem filtro — **0 provadas pela classe 2, cota 0 nas classes 1/2/4, total movido [não verificado]** ([models.py:4051](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4051))? **[não verificado: números da sonda fornecida]**

## 18/11 10:31 · sessao c02f51090e3f · RESPOSTAS-DE-LEI-1811-1010 · leitura · origem=Code

A cópia examinada não tem metadados Git; portanto, sua identidade como HEAD `dd489c627` está **[não verificado]**. As referências a arquivos abaixo são da cópia aberta. Nos anexos, a numeração começa na primeira linha de conteúdo de cada bloco.

A conclusão técnica é que **nenhum dos três escritores existentes registra qualquer uma das duas marcas pedidas com seu significado e universo**. Um escritor novo é a proposta para ambas, mas a lei ainda precisa decidir se “nenhuma” é um ato persistido ou a leitura da ausência de decisão, e quais efeitos e transições pertencem a “trabalho reconhecido”. O brief exige uma porta única de decisão e condiciona a abertura aos cortes L1 e L2. (brief_dst.md:L2–L3, L24, L28–L30, L32–L34, L48–L49; [he.py:159](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:159), [celula.py:572](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:572), [retratar_humano.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:150))

### 1. Matriz de transições

`RED` abaixo significa **caso a especificar**, não teste existente conferido. Para todas as linhas DST, o juiz e a operação completos estão **[não verificado]**; a tabela distingue o comportamento pedido do que o código já faz. `decisao_do_dia` lê veredictos humanos de Resolver dia, cujo vocabulário é outro. ([veredito_celula.py:18](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:18), [veredito_celula.py:85](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:85))

| Estado / transição | Juiz | Operação | O que persiste | Retorno / recusa | Prova |
|---|---|---|---|---|---|
| **A: sem marca → nenhuma** | Juiz DST **[não verificado]**. `decisao_do_dia` devolve `{}` quando não encontra veredito humano; isso não demonstra uma marca persistida “nenhuma”. | Escritor DST **[não verificado]**; depende da lei sobre ausência versus ato. | Marca, modelo e trilha DST **[não verificado]**. | Retorno e recusa **[não verificado]**. | [veredito_celula.py:104](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:104); **RED A1:** distinguir ausência de linha e marca, se a lei exigir marca. |
| **A: nenhuma → outra decisão** | Juiz DST **[não verificado]**. | Transição DST **[não verificado]**; HE, correção de grade e retratação são atos distintos. | Antes/depois da decisão DST **[não verificado]**. | Estados permitidos e recusa **[não verificado]**. | brief_dst.md:L30; **RED A2:** trocar para cada saída aprovada e conservar a trilha. |
| **A: repetida 2×** | Juiz DST **[não verificado]**. | Operação DST **[não verificado]**. | Uma trilha e nenhuma duplicata são o contrato de desenho do brief, ainda não demonstrado para A. | Retorno específico **[não verificado]**. | brief_dst.md:L28; **RED A3:** repetir a mesma entrada e verificar escrita e trilha. |
| **A: revogada** | Juiz de vigência DST **[não verificado]**. | Revogação DST **[não verificado]**. | O brief exige conservar decisão tomada na trilha quando o fato muda; campos e significado de revogar “nenhuma” **[não verificado]**. | Retorno e estado posterior **[não verificado]**. | brief_dst.md:L29; **RED A4:** conservar história e definir leitura após revogação. |
| **B: sem decisão → trabalho reconhecido** | Juiz DST **[não verificado]**. O admin é o decisor previsto no brief. | Escritor DST **[não verificado]**. Resolver dia com `trabalhou` recusa quando a grade calculada tem `tipo_dia != trabalho`; HE é decisão separada. | Registro DST **[não verificado]**; `DecisaoHE` guarda decisão sobre minutos fora da janela, não reconhecimento do trabalho. | Resolver dia retorna `ok=False` nesse ramo; HE recusa minutos não positivos. | brief_dst.md:L30; [veredito_celula.py:243](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:243), [he.py:186](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:186); **RED B1**. |
| **B: reconhecido → escala corrigida** | Juiz da troca DST **[não verificado]**; vínculo da edição é obtido por `vinculo_do_dia_carregado`. | `editar_dia(..., trabalha=True)` já faz a correção da célula; não demonstra troca de decisão DST. | `CelulaDia`: trabalho, DNA, vínculo, origem, autor e instante; trilha da edição com antes/depois e motivo. | `editada`, `igual`, `barrada` ou `sem_vinculo`; transição DST **[não verificado]**. | [celula.py:572](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:572); **RED B2**. |
| **B: reconhecido → batida invalidada** | Juiz da troca DST **[não verificado]**. | `retratar` existe por batida, mas a troca da decisão por pessoa × dia **[não verificado]**. | A batida recebe `aprovada=False` e `retratada_em`; trilha da retratação. Persistência da troca DST **[não verificado]**. | `retratar` devolve `(ok, mensagem)`; exige permissão e motivo ativo. | [retratar_humano.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:150), [registro_batida.py:287](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/registro_batida.py:287); **RED B3**. |
| **B: repetida 2×** | Juiz DST **[não verificado]**. | Operação DST **[não verificado]**. | Uma trilha e zero duplicata são requisitos do desenho, não comportamento demonstrado para B. | Retorno específico **[não verificado]**. | brief_dst.md:L28; **RED B4**. |
| **B: revogada** | Juiz de vigência DST **[não verificado]**. `vigentes()` é juiz da decisão **HE**, não da DST. | Revogação DST **[não verificado]**. | Trilha da decisão DST e seus campos de revogação **[não verificado]**; o brief pede retenção da decisão quando o fato muda. | Efeito sobre HE, grade e batidas **[não verificado]**. | brief_dst.md:L29; [models.py:1265](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/models.py:1265); **RED B5**. |

### 2. Encaixe nos três escritores

As respostas sobre **universo** significam se a operação aceita *todos* os dias da unidade pedida, não se ela pode ser chamada em algum exemplo desse universo. (brief_dst.md:L3)

| Escritor | Vocabulário contém A / B? | Aceita o universo DST? | Trilha e repetição |
|---|---|---|---|
| `decidir_he` | **Não / não.** Apenas `autorizado` e `nao`, relativos à HE fora da janela. | **Não integralmente.** Exige minutos positivos fornecidos pelo chamador, além de permissão e, para autorizar, motivo; não valida a unidade DST como tal. | Guarda estado, minutos, motivo, autor e datas; tenta trilha antes/depois. A trilha é **best effort**, portanto não garantida. Mesma decisão vigente retorna `no_op`, sem nova trilha. ([models.py:1283](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/models.py:1283), [he.py:175](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:175), [he.py:89](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:89), [he.py:195](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:195)) |
| `editar_dia` | **Não / não.** Aceita `trabalha=True/False`, que altera a grade. | **Não integralmente.** Pode editar dia sem trabalho, mas exige vínculo, autor e motivo e tem guarda de competência; não exige batida apurável. | Guarda autor, instante, motivo e antes/depois; falha da trilha desfaz a edição. Edição já equivalente retorna `igual` sem escrever. ([celula.py:572](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:572), [celula.py:597](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:597), [celula.py:632](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:632)) |
| `retratar` | **Não / não.** Registra que uma marcação não foi trabalho. | **Não como decisão de dia.** Recebe uma batida individual; pode atuar numa batida do universo, mas não representa a decisão sobre pessoa × dia. | Exige ator autorizado e motivo ativo; passa motivo e antes/depois à auditoria, que pode falhar sem barrar a gravação. Repetir sobre batida retratada retorna sucesso sem nova trilha. ([retratar_humano.py:131](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:131), [retratar_humano.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:150), [registro_batida.py:255](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/registro_batida.py:255), [auditoria.py:17](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/auditoria.py:17)) |

- **A — EXIGE escritor novo se “nenhuma” tiver de ser uma marca persistida.** Nenhum dos três escreve essa marca; se for apenas a leitura da ausência de decisão, a necessidade de escritor para A depende da lei. ([models.py:1287](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/models.py:1287), [celula.py:575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:575), [retratar_humano.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:150))
- **B — EXIGE escritor novo para a decisão própria**, sujeito ao corte da lei: HE decide minutos, a edição muda a grade e a retratação invalida uma batida. O escritor novo proposto pode coordenar os efeitos existentes que a lei escolher, dentro da porta única prevista no brief. (brief_dst.md:L24, L30; [he.py:159](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:159), [celula.py:572](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:572), [retratar_humano.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/retratar_humano.py:150))

### 3. Frota demonstrada pelos anexos

- **Medida da sombra, 17/11:** dos 170 dias `sem_trabalho` com órfã, o relato fecha **93 pagos sem decisão + 72 `ESTRUTURA_DINHEIRO` + 3 com porta/pergunta + 1 HE autorizada + 1 HE negada = 170**. Dos 72, 50 tinham duas ou mais batidas; o relato informa zero com relógio positivo. Essa é uma medida distinta do retrato gravado da folha. (relato_72.txt:L7–L8)
- **Recorte detalhado do retrato:** as 55 linhas pessoa × dia de `retrato_a2` correspondem às datas das linhas selecionadas de `retrato_a`; **5** têm simultaneamente `trabalha=False` e batidas apuráveis positivas: os cinco dias de `col348`. As outras **50** não satisfazem as duas condições *nesses campos impressos*. (retrato_a.saida.txt:L4–L24; retrato_a2.compacto.txt:L1–L55, em especial L47–L51)
- **Limite da contagem pelo juiz:** das outras 50, **25** têm zero batidas apuráveis e **25** têm `trabalha=True` e batidas positivas. Para estas últimas, o anexo não imprime `ata.tipo_dia` nem o carimbo necessário da ausência parcial: `dia_sem_trabalho` também pode considerar a ata. Logo, **5 são confirmados pelo `trabalha=False`; o total segundo o juiz completo e a classificação definitiva dos outros 25 com batidas estão [não verificado]**. Não se pode extrapolar as 55 linhas selecionadas para todas as linhas do retrato. (retrato_a2.compacto.txt:L1–L55; [juizes.py:445](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:445); [retrato_folha.py:98](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/retrato_folha.py:98))
- **Leitor e rótulo:** no retrato gravado, `col348` aparece como `furo_espelho`, fio `ninguem`, balde `sem_acao_disponivel`, sem ações; o rótulo desse balde é **“Sem ação disponível”**. `tarefa_do_dia` informa, para esses cinco dias, `fio=None`; o retrato é uma leitura por pessoa e não comprova um rótulo DST específico para cada dia. (retrato_a.saida.txt:L20; retrato_a2.compacto.txt:L47–L51; [retrato_folha.py:191](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/retrato_folha.py:191), [tarefa_do_dia.py:718](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:718))

**Perguntas de lei para Ronald:** “nenhuma” deve ser uma marca persistida, com ato e revogação, ou o juiz deve devolvê-la quando não houver decisão vigente? “Trabalho reconhecido” é decisão própria independente de autorização de HE; que efeitos, trocas e revogação ela produz, inclusive quando não há minutos positivos de HE? Permanecem também os cortes L1 e L2 exigidos para abertura. (brief_dst.md:L30, L32–L34, L48–L49; [he.py:186](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:186))

FRASE DE CORTE DST-L1: (A) a marca 'nenhuma' [não verificado: depende da pergunta de lei acima] ; (B) 'trabalho reconhecido' nasce como escritor novo decidir_dstl1 [proposta sujeita ao corte de lei]; juiz novo decisao_admin_dia_sem_trabalho [proposta]; trilha quem, quando, motivo, antes/depois e revogação [campos propostos]; universo pessoa × dia do turno cuja célula diz sem trabalho e há batida apurável; frota 72 de 170 na sombra, retrato detalhado 5 confirmados, 25 fora por zero batidas e 25 dependentes da ata.

_6913 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
