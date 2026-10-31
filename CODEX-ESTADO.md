# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 23:56** · publicado em **30/10/2026 23:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 23:56 · sessao 1b93bacd51ca · - · - · origem=Ronald (tmux)

Q4 — mapa consolidado, somente por leitura:

| Atributo observado | Quem escreve | Quem lê no arnês / produção |
|---|---|---|
| `minutos_extra`, `_50`, `_100`, `_100_feriado` | Defaults do `PeriodoCalculo`; motor v2 escreve, por exemplo, dobra e excesso; `reler` zera pares julgados e escreve o último portador. (`app/ponto/motor_calculo_v2.py:166`, `:3031`, `:3078`; `app/ponto/calculador/periodo.py:758`, `:767`, `:838`, `:845`) | P-SOMA lê os quatro; `leitura` soma os quatro; snapshot P-IDEM inclui os quatro. Produção: propriedades do `ResultadoMes`, `dia_pago`, `espelho`, fechamento. (`app/ponto/tests/test_v3_e1_valor.py:98`; `app/ponto/tests/test_o280_bateria_da_lei.py:179`, `:4328`; `app/ponto/motor_calculo_v2.py:274`; `app/ponto/services/dia_pago.py:180`; `app/ponto/services/espelho.py:112`; `app/ponto/services/fechamento.py:894`) |
| `minutos_extra_50_noturna`, `_100_noturna` | Motor v2 em `_aplicar_he_noturna`, com resets mensais; `reler` não escreve esses campos. (`app/ponto/motor_calculo_v2.py:952`, `:958`, `:960`, `:2875`; `app/ponto/calculador/periodo.py:101`) | **Não são observados pelo arnês**, nem P-SOMA, nem `leitura`, nem snapshot P-IDEM. Produção os lê nas propriedades mensais e em `dia_pago`/fechamento. (`app/ponto/tests/test_v3_e1_valor.py:98`; `app/ponto/tests/test_o280_bateria_da_lei.py:179`, `:4325`; `app/ponto/motor_calculo_v2.py:288`; `app/ponto/services/dia_pago.py:181`; `app/ponto/services/fechamento.py:896`) |
| `minutos_atraso`, `minutos_saida_antecipada` | Motor v2 e seu teto; `reler` zera se recebeu `marcos`, depois escreve o portador. (`app/ponto/motor_calculo_v2.py:2157`, `:2164`, `:1033`, `:1132`; `app/ponto/calculador/periodo.py:634`, `:639`, `:809`) | `leitura` e snapshot; produção: propriedades mensais, espelho, crédito parcial e fechamento. (`app/ponto/tests/test_o280_bateria_da_lei.py:183`, `:4329`; `app/ponto/motor_calculo_v2.py:297`, `:301`; `app/ponto/services/espelho.py:115`; `app/ponto/services/credito_parcial.py:36`; `app/ponto/services/fechamento.py:969`) |
| Trabalhado, intra descontada/indenizada | Motor v2 origina; `reler` substitui nos pares julgados, inclusive FT. (`app/ponto/motor_calculo_v2.py:2713`, `:2722`, `:2723`; `app/ponto/calculador/periodo.py:606`, `:742`) | `leitura`, snapshot; propriedades mensais e `dia_pago`. **`ft_min` é soma do trabalhado FT reescrito pela v3**, embora o docstring do arnês diga que passa intocado. (`app/ponto/tests/test_o280_bateria_da_lei.py:173`, `:174`, `:4326`; `app/ponto/motor_calculo_v2.py:245`, `:249`; `app/ponto/services/dia_pago.py:178`, `:186`; `app/ponto/tests/test_v3_e1_valor.py:18`) |
| Noturno relógio/legal | V2 escreve; v3 substitui apenas com `cct` e par relido. (`app/ponto/motor_calculo_v2.py:941`; `app/ponto/calculador/periodo.py:745`, `:752`) | `leitura`, snapshot; produção lê legal para total mensal e dia pago. (`app/ponto/tests/test_o280_bateria_da_lei.py:177`, `:4327`; `app/ponto/motor_calculo_v2.py:271`; `app/ponto/services/dia_pago.py:179`) |
| `minutos_fora_janela` | Motor v2 escreve; v3 só zera na cópia duplicada e lê para teto de pontualidade. (`app/ponto/motor_calculo_v2.py:1410`, `:2725`; `app/ponto/calculador/periodo.py:184`, `:736`, `:786`) | `leitura`, snapshot; alimentação do calculador. (`app/ponto/tests/test_o280_bateria_da_lei.py:185`, `:4327`; `app/ponto/calculador/alimentacao.py:83`) |
| Pontas, `turno_aberto`, `hora_extra_100` | Motor v2 origina pontas/aberto; v3 redefine o booleano HE100, ligado somente na dobra. (`app/ponto/motor_calculo_v2.py:160`, `:1778`, `:2691`; `app/ponto/calculador/periodo.py:772`, `:842`) | Snapshot lê pontas efetivas com fallback cru, FT, aberto e HE100. Snapshot é helper de teste, não atributo produtivo. (`app/ponto/tests/test_o280_bateria_da_lei.py:4333`, `:4345`, `:4348`, `:4353`) |

`estado_dos_periodos` constrói um multiset ordenado: duas pontas em epoch, identificação FT, aberto, HE100 e 12 rubricas arredondadas para duas casas. Ele não cobre os campos HE noturna nem alertas, intervalos ou outros dados do resultado. (`app/ponto/tests/test_o280_bateria_da_lei.py:4325`, `:4344`, `:4350`, `:4354`)

`Censo` tem defaults declarados em `app/ponto/calculador/periodo.py:225` e cada chamada cria nova instância em `:591`. Seus escritores funcionais estão em `reler`:

- `cct_medida`: `:592`; `pontualidade_medida`: `:604`; `sem_dia`: `:610`; `abertos`: `:642`; `sem_carimbo`: `:646`.
- `efetivo_invertido` / `invertidos`: `:697`; `par_duplicado` / `duplicados`: `:734`; `periodos_noturno`: `:754`; `relidos`: `:774`; `dias_ft`: `:776`.
- `dias_sem_pontualidade_por_lei`: `:789`; `cxr`: `:790`; `dias_sem_pontualidade_por_par_nao_lido`: `:804`; `atraso_em_dia_so_ft`: `:807`; `dias_pontualidade`: `:812`.
- `dias`: `:815`; `dias_sem_previsto_com_trabalho`: `:821`; `dias_feriado_dobra`: `:843`.

O arnês imprime seis campos (`cct_medida`, `pontualidade_medida`, `relidos`, `abertos`, `sem_dia`, `sem_carimbo`) e compara **todos** via `dataclasses.asdict` no P-IDEM; não exige qualquer contador igual a zero ou flag verdadeira antes de anunciar VERDE. (`app/ponto/tests/test_v3_e1_valor.py:110`, `:155`, `:172`)

Não foi encontrado leitor produtivo de `reler` nas buscas em `.py` do repositório; os importadores/chamadores encontrados são os dois arquivos de teste. O próprio contrato o declara ensaio sem caminho de produção. Os leitores produtivos dos atributos acima recebem os resultados do motor v2. (`app/ponto/tests/test_v3_e1_valor.py:83`, `:88`, `:170`, `:173`; `app/ponto/tests/test_o280_bateria_da_lei.py:4375`, `:4382`; `app/ponto/calculador/periodo.py:543`)

Observação adicional: `RUBRICAS_DA_V3` é apenas definida; `_caso` não a consulta ao imprimir log ou falhas. A prometida separação das “duas cestas no texto” não está implementada. (`app/ponto/tests/test_v3_e1_valor.py:20`, `:50`, `:155`, `:159`)

## 30/10 23:56 · sessao 3dcaa8a24202 · - · - · origem=Ronald (tmux)

**2. Há perda de asserção no modo propriedade.** Com `_coleta` ativa, `subTest` captura `self.failureException`, mas só registra em `MODO == 'valor'`; em propriedade, descarta. O mesmo ocorre para asserção fora de `subTest`. Se já houve releitura e `_props` está vazio, `_caso` retorna sem fail nem skip (`app/ponto/tests/test_v3_e1_valor.py:125`, `app/ponto/tests/test_v3_e1_valor.py:135`, `app/ponto/tests/test_v3_e1_valor.py:146`, `app/ponto/tests/test_v3_e1_valor.py:149`).

Em modo valor, falhas capturadas entram na coleta e terminam em fail para VALIDADO ou skip para PENDENTE; atualmente `VALIDADOS = {}` (`app/ponto/tests/test_v3_e1_valor.py:47`, `app/ponto/tests/test_v3_e1_valor.py:137`, `app/ponto/tests/test_v3_e1_valor.py:148`, `app/ponto/tests/test_v3_e1_valor.py:162`). Exceções que não sejam `self.failureException` ou `SkipTest` não são capturadas: propagam como erro, sem conversão em skip (`app/ponto/tests/test_v3_e1_valor.py:131`, `app/ponto/tests/test_v3_e1_valor.py:143`).

O marcador de valor também admite VERDE sem releitura, desde que não tenha asserções falhadas: imprime `sem releitura` e retorna; o próprio MORDE exercita caso vazio verde (`app/ponto/tests/test_v3_e1_valor.py:155`, `app/ponto/tests/test_v3_e1_valor.py:276`).

**3. As propriedades conseguem falhar, com limites:**

| Propriedade | Pode ficar vermelha | Mutação despercebida |
|---|---|---|
| P-SOMA | Sim: divergência extra versus he50+he100, ou feriado acima de he100, entra em `_props` e causa fail. Trocar a atribuição de he50 por `x.extra_50_min + 1` seria detectável (`app/ponto/tests/test_v3_e1_valor.py:98`, `app/ponto/tests/test_v3_e1_valor.py:152`, `app/ponto/calculador/periodo.py:846`). | Zerar conjuntamente extra/he50/he100/he100_fer: os valores permanecem coerentes, embora errados. Só identidades são verificadas (`app/ponto/tests/test_v3_e1_valor.py:102`, `app/ponto/tests/test_v3_e1_valor.py:105`). |
| P-IDEM | Sim: trocar `p.minutos_trabalhados = d.minutos_trabalhados` por `+=` altera o segundo estado e seria detectado (`app/ponto/calculador/periodo.py:742`, `app/ponto/tests/test_v3_e1_valor.py:172`). | Fixar `p.minutos_trabalhados = 123.0` dá erro determinístico e idempotente. Compara primeira v3 com segunda v3, sem oráculo de correção (`app/ponto/tests/test_v3_e1_valor.py:172`). Alterações inferiores à precisão observada podem desaparecer: o estado arredonda rubricas a duas casas (`app/ponto/tests/test_o280_bateria_da_lei.py:4354`). |
| P-PREV | Sim, se a régua externa observada mudar, por exemplo se `reler` passar a alterar cadastro. Não verifica a régua realmente usada dentro de `reler` (`app/ponto/tests/test_v3_e1_valor.py:176`, `app/ponto/calculador/periodo.py:513`). | Em `reler`, substituir a régua recebida por `dataclasses.replace(regua_de(dia), previsto_min=0)` deixa o cadastro externo intacto e passa na comparação, embora o cálculo use outro previsto (`app/ponto/calculador/periodo.py:614`, `app/ponto/tests/test_v3_e1_valor.py:177`). |

No lote 3, P-PREV só compara dias com snapshot anterior; sem `_bater`, como L16, `_reguas_antes` fica vazio e nenhuma igualdade é verificada (`app/ponto/tests/test_v3_e1_valor.py:215`, `app/ponto/tests/test_v3_e1_valor.py:236`, `app/ponto/tests/test_v3_e1_valor.py:177`, `app/ponto/tests/test_e1_lote3.py:621`).

Só P-SOMA tem MORDE dedicado neste arquivo; ele testa o helper com objetos artificiais (`app/ponto/tests/test_v3_e1_valor.py:287`).

## 30/10 23:55 · sessao 2331bd004654 · - · - · origem=Ronald (tmux)

P5 — há conflito de expectativas, não apenas diferença de nomes: C17 fixa COM `08–17`, pausa declarada `12–13`, previsto 480, sem pausa gozada, e exige `trab=540`, `intra_indenizada=60`, `extra=he50=60` (`app/ponto/tests/test_e1_lote1_valor_do_dia.py:72`, `app/ponto/tests/test_e1_lote1_valor_do_dia.py:482`, `app/ponto/tests/test_e1_lote1_valor_do_dia.py:499`).

Para esses números, `excesso_do_dia` produz limite `480 + min(60,60)=540` e `extra=0`: conclusão por leitura estática do corpo (`app/ponto/calculador/periodo.py:410`, `app/ponto/calculador/periodo.py:414`, `app/ponto/calculador/periodo.py:416`).

Testes que afirmam os dois comportamentos: [nao verificado]

| Comportamento | Teste e evidência | [nao verificado]
|---|---|
| HE paga também os 60 minutos sem pausa de COM `08–17` | `test_E1_C17_comercial_sem_intervalo_indeniza_sessenta`: exige extra/he50=60 além da intra=60 (`app/ponto/tests/test_e1_lote1_valor_do_dia.py:499`). |
| Acrescentar suprimido ao limite de COM | `test_v3_A6_e_A6b_a_hora_acima_do_limite_legal_segue_DEVIDA`: COM `08–22`, trab840/intra60/extra300, isto é, limite540 (`app/ponto/tests/test_o280_bateria_da_lei.py:3611`, `app/ponto/tests/test_o280_bateria_da_lei.py:3624`). |
| Fórmula `previsto + min(suprimido,declarado)` explicitamente | `test_v3_excesso_do_dia_e_PURO_e_a_reparticao_SOMA`: com previsto480/declarado60 e trab630/suprimido30, exige limite510/extra120 (`app/ponto/tests/test_o280_bateria_da_lei.py:3660`, `app/ponto/tests/test_o280_bateria_da_lei.py:3672`). |
| Mesma fórmula integrada, pausa parcial | F7d: COM trab630, intra30, extra120 (`app/ponto/tests/test_o280_bateria_da_lei.py:3511`, `app/ponto/tests/test_o280_bateria_da_lei.py:3518`). |
| Sem pausa não gera HE correspondente à intra, em D12 | `test_RED_doze_horas_sem_intervalo_NAO_tem_hora_extra`: `07–19`, intra60 e extra0 (`app/ponto/tests/test_he_intra_dobrada.py:77`, `app/ponto/tests/test_he_intra_dobrada.py:83`). |
| Excedente além do limite continua pago, em D12 | `test_MORDE_treze_horas_sem_intervalo_TEM_uma_hora_de_extra`: `07–20`, intra60/extra60 (`app/ponto/tests/test_he_intra_dobrada.py:88`). |
| Pausa gozada não aumenta limite | `test_MORDE_intervalo_gozado_nao_ganha_folga_no_limite`: intra0/extra60 (`app/ponto/tests/test_he_intra_dobrada.py:95`). |

O teste antigo `test_comercial_sem_intra_registrada` usa exatamente batidas `08–17`, porém só afirma `intra > 0`; não decide HE60 versus HE0 (`app/ponto/tests/test_motor.py:1041`, `app/ponto/tests/test_motor.py:1045`, `app/ponto/tests/test_motor.py:1049`).

C17 também é herdado pelas classes de valor e propriedade do arnês; com `VALIDADOS={}`, eventual divergência de valor torna-se skip, enquanto o modo propriedade não cobra o valor esperado da rubrica (`app/ponto/tests/test_v3_e1_valor.py:47`, `app/ponto/tests/test_v3_e1_valor.py:146`, `app/ponto/tests/test_v3_e1_valor.py:149`, `app/ponto/tests/test_v3_e1_valor.py:164`, `app/ponto/tests/test_v3_e1_valor.py:249`).

## 30/10 23:34 · sessao 6f377c5d4b5c · DISJUNTOR · verificador · origem=Code

O desenho precisa corrigir **a fonte do B4, a identificação da volta e a integração do piso com o Stop**. O leitor atual não publica os campos necessários ao orçamento diário, e o avaliador de contexto ainda aplica os três gatilhos do v2 (`bin/codex_limite_estado.py:118`; `bin/orcamento_contexto.py:209`).

Abaixo, distingo defeitos presentes de cenários condicionais de implementação. Para `contrato.txt`, numerei o cabeçalho como linha 1, as cláusulas como linhas 2–6 e o MEDIDO como linha 7.

1. **Item 1 — o texto Bash não basta para determinar papel e volta.**  
   O wrapper recebe `CODEX_PAPEL` do **ambiente efetivo**; suas linhas de registro não contêm `CODEX_VOLTA` (`bin/codex_pergunta.sh:44`; `bin/codex_pergunta.sh:53`; `bin/codex_pergunta.sh:88`). Cenários:

   - Papel herdado ou `CODEX_PAPEL="$P"` não pode ser resolvido apenas procurando atribuições literais.
   - FINAL declarada em ambiente herdado, sem atribuição visível no comando, vira `meio` pela regra proposta: **nega uma FINAL legítima** quando o orçamento diário estiver estourado.
   - Uma busca textual pode aproveitar `CODEX_VOLTA=final` de comentário, string ou outra invocação e **liberar uma chamada do meio**.
   - Um Bash contendo duas chamadas, uma permitida e outra proibida, exige decisões por invocação; uma única classificação do texto inteiro não resolve essa diferença. O parser e essa integração ainda são **[não verificado]**; a autoridade atual do papel é o ambiente recebido pelo wrapper (`bin/codex_pergunta.sh:44`; `contrato.txt:2`).

2. **Item 3 — a fonte “que o ESTADO já publica” não fornece o orçamento diário.**  
   `ler_semanal()` devolve somente `plan_type`, `used_percent` e `window_minutes`: descarta `resets_at`, timestamp e histórico. Portanto, consumir sua saída atual resulta sempre em “sem resets_at/sem registro de hoje”; pelo contrato, **libera o meio mesmo quando deveria negar**. É preciso ampliar esse leitor para obter primeiro/último registro do dia e reset correspondente (`bin/codex_limite_estado.py:99`; `bin/codex_limite_estado.py:118`; `contrato.txt:4`).

3. **Item 3 — “último” atualmente significa arquivo com maior mtime.**  
   O leitor ordena arquivos por mtime, examina apenas quarenta e escolhe a última linha contendo `rate_limits` no primeiro arquivo aproveitável; não compara timestamps das medições. Um arquivo com uso antigo de 10% tocado depois de outro com 86% **abre indevidamente**; o inverso pode **negar inclusive FINAL**, ao aplicar a restrição de 85% sobre dado velho. Um registro relevante no 41º arquivo também fica invisível (`bin/codex_limite_estado.py:105`; `bin/codex_limite_estado.py:110`; `contrato.txt:4`).

4. **Item 3 — faltam regras para reset e precedência.**  
   No dado anexado, 54−12 = 42 pp, acima dos 13,3 pp/dia e abaixo de 85%: esse caso exige negar meio e preservar FINAL de verificador (`contrato.txt:4`; `contrato.txt:7`). Mas o contrato não fecha estes casos:

   - Reset durante o dia: primeiro registro 90%, último 5% produz −85 pp; a subtração atravessou duas janelas.
   - Orçamento estourado e uso ≥85%: `escada` de papel `leitura` satisfaz “todo papel”, mas não “papel verificador”, se ambas as restrições forem acumuladas.
   - Uso ≥85% sem `resets_at`: conflitam “só escada” e “não sei, não nega”.

   Esses resultados precisam de precedência explícita; o leitor atual nem conserva o reset para separar as janelas (`contrato.txt:4`; `bin/codex_limite_estado.py:118`).

5. **Item 3 — ANEXO-ENXUTO por nome textual permite escape e falso bloqueio.**  
   O wrapper aceita caminhos arbitrários e concatena o conteúdo real. `--anexo "$A"` apontando para `LEIS.md`, ou uma cópia chamada `/tmp/a.txt`, escapa de reconhecimento literal; procurar nomes indiscriminadamente pode negar um comando que apenas os menciona. Também fica indefinido se `escada` pode levar esses anexos: a negativa especifica somente `meio`, mas a explicação diz “só na 1 e na final” (`bin/codex_pergunta.sh:32`; `bin/codex_pergunta.sh:50`; `bin/codex_pergunta.sh:111`; `contrato.txt:4`).

6. **Itens 1/3 — autorização antes da fila não garante limite na execução.**  
   Se seis chamadas passam a 84%, podem continuar depois de a primeira levar o uso a 85%: o wrapper espera gate/slots e chama o CLI sem revalidar orçamento. O timeout cobre somente o `codex exec`; a espera pelo gate e o laço dos slots não têm prazo global. Assim, o desenho pode **liberar execução já proibida** e deixar chamada aguardando indefinidamente (`bin/codex_pergunta.sh:137`; `bin/codex_pergunta.sh:140`; `bin/codex_pergunta.sh:142`; `bin/codex_pergunta.sh:157`).

7. **Itens 1/2 — “primeiro Stop” mede contexto depois do trabalho, não o piso imediatamente após compactar.**  
   A medida atual pega o último evento `assistant` e soma entrada mais caches. Um ciclo pode executar muitos turnos de ferramentas antes do primeiro Stop; seu “piso” fica inflado e pode ultrapassar 200 mil, desligando C1 justamente nesse ciclo. O anexo distingue primeiro turno pós-compact ≤107.621 de valores de primeiro Stop maiores; não são a mesma medida (`bin/orcamento_contexto.py:89`; `bin/orcamento_contexto.py:101`; `bin/orcamento_contexto.py:110`; `contrato.txt:7`).

8. **Item 2 — 200 mil não é a janela declarada para todos os modelos.**  
   O código declara 1 milhão para opus/sonnet/fable e 200 mil para haiku-4-5. Com piso 135.592, a fórmula proposta dispara em 167.796 — aproximadamente 16,8% da janela de 1 milhão. Com piso 865.942, desliga C1, embora o contexto ainda esteja dentro daquela janela. Isso pode ser um teto operacional deliberado, mas não pode ser rotulado como limite da janela do modelo (`bin/orcamento_contexto.py:35`; `bin/orcamento_contexto.py:44`; `contrato.txt:3`; `contrato.txt:7`).

9. **Itens 1/2 — o ciclo atual mistura sessões.**  
   `ultimo_clear.json` é global por raiz. Guarda `session_id`, mas o Stop extrai apenas `transcript_path` e usa o marco sem casar a sessão. Cenário: A compacta; B faz o próximo Stop; um registrador encaixado nesse fluxo pode gravar tokens de B como piso pós-compact de A. Além disso, um clear seguido de compact antes de qualquer Stop sobrescreve o marco anterior: não haverá medida para “cada ciclo” intermediário (`bin/marco_de_clear.py:20`; `bin/marco_de_clear.py:44`; `bin/marco_de_clear.py:61`; `bin/hook_stop_fila1.py:794`; `bin/hook_stop_fila1.py:1018`).

10. **Itens 1/2 — posição do registro e ausência de piso mudam o comportamento.**  
    Se `registrar_piso` entrar apenas dentro do orçamento, não verá necessariamente o primeiro Stop: o checador do RELATO pode bloquear e sair antes. E um `return` antecipado por “sem piso” ou “piso ≥ teto” dentro de `avaliar()` desligaria também os gatilhos de seis horas e três deploys, que hoje são independentes. O desligamento precisa atingir somente C1 (`bin/hook_stop_fila1.py:1029`; `bin/hook_stop_fila1.py:1034`; `bin/hook_stop_fila1.py:1037`; `bin/orcamento_contexto.py:209`; `bin/orcamento_contexto.py:212`; `bin/orcamento_contexto.py:214`).

11. **Itens 1/2/3 — uma nova negativa no Stop pode contornar sua válvula de saída.**  
    Um `block` por B4≥85 ou erro de piso, colocado antes de PAREI/ESPERANDO/TETO e repetido incondicionalmente, pode impedir inclusive a saída para clear e **prender a sessão**. Hoje o orçamento contém exceções e devolve ao caminho comum; o contador só é atualizado depois. Já existe uma exceção à promessa geral de falha aberta: falha persistente do checador do RELATO bloqueia antes do teto (`bin/hook_stop_fila1.py:886`; `bin/hook_stop_fila1.py:1007`; `bin/hook_stop_fila1.py:1029`; `bin/hook_stop_fila1.py:1092`).

    A linha `DISJUNTOR B4-CODEX` também não é uma transição reconhecida pelo Stop atual. Se for colocada acima de um PAREI ou ESPERANDO legítimo, esconde essa declaração, porque o leitor considera somente a primeira linha útil (`bin/hook_stop_fila1.py:400`; `bin/hook_stop_fila1.py:413`; `bin/hook_stop_fila1.py:938`; `contrato.txt:3`).

12. **Itens 1/4 — escritor único não garante piso único nem prova externa.**  
    Dois Stops concorrentes, ou append seguido de falha antes de persistir “ciclo registrado”, podem duplicar o piso; a operação precisa de identidade do ciclo e serialização. O orçamento atual já serializa leitura/decisão/gravação (`bin/hook_stop_fila1.py:733`; `bin/hook_stop_fila1.py:755`).

    Há ainda um encaixe perigoso: `pisos.log` não está na lista de arquivos proibidos como prova de ESPERANDO. Se seu mtime já foi observado e o próprio Stop apenda um piso antes de conferir a espera, `ESPERANDO: agente-x | arquivo=logs/sessao/pisos.log` pode obter **prova produzida pelo próprio hook** e liberar indevidamente (`bin/hook_stop_fila1.py:556`; `bin/hook_stop_fila1.py:605`; `bin/hook_stop_fila1.py:1051`).

13. **Item 4 — duas linhas não são duas chamadas; “ini” também não prova execução do Codex.**  
    O contador atual acerta ao contar somente `ini`; contar `ini` e `fim` no placar novo duplica chamadas concluídas. Porém `ini` é escrito antes de archive e raspagem: falhas rc3/rc4 entram como “chamadas ao Codex” sem alcançar o CLI. O rótulo atual mede tentativas iniciadas pelo wrapper, não exclusivamente execuções do modelo (`bin/codex_limite_estado.py:64`; `bin/codex_limite_estado.py:76`; `bin/codex_pergunta.sh:88`; `bin/codex_pergunta.sh:99`; `bin/codex_pergunta.sh:105`; `bin/codex_pergunta.sh:157`).

14. **Item 4 — a junção existente pode perder ou duplicar atribuição de tokens.**  
    A chave `(obra, hash12, pid)` não inclui identificador único da chamada. Se o PID for reciclado, o último `fim` sobrescreve o anterior; somar tokens por cada `ini` pode atribuir a mesma sessão a duas chamadas. No caminho LIMITE, o wrapper sai antes de extrair `thread_id`, registra `-`, e o mapa exclui essa sessão: eventual consumo anterior ao erro desaparece da atribuição por papel (`bin/codex_limite_estado.py:63`; `bin/codex_limite_estado.py:67`; `bin/codex_limite_estado.py:96`; `bin/codex_pergunta.sh:59`; `bin/codex_pergunta.sh:171`; `bin/codex_pergunta.sh:175`).

    Também há divergência configurável de destino: o wrapper escreve em `CODEX_LOGD`, enquanto o leitor procura `logs/codex/chamadas.log` sob a raiz (`bin/codex_pergunta.sh:51`; `bin/codex_limite_estado.py:22`; `bin/codex_limite_estado.py:52`).

15. **Item 4 — alguns rótulos prometem medidas ainda não demonstradas.**  
    O leitor chama a apresentação de “uso semanal”, mas aceita qualquer `window_minutes`; uma principal de 300 minutos seria apresentada sob esse título (`bin/codex_limite_estado.py:133`; `bin/codex_limite_estado.py:142`). Para “tokens do dia”, usar o total final de uma sessão que atravessa meia-noite exige definir em qual dia contabilizá-lo; presença e semântica de `total_token_usage` nos registros reais fornecidos são **[não verificado]** (`contrato.txt:5`).

    “Não há fonte estruturada” também não está demonstrado: já existe leitura estruturada de `tool_use/input` dos transcripts Claude. O leitor disponível escolhe somente o transcript mais novo e descarta dimensões necessárias ao placar por modelo/tipo; ele não basta para esse placar, mas sua insuficiência não prova inexistência da fonte (`bin/gerar_sessao.py:33`; `bin/gerar_sessao.py:65`; `bin/gerar_sessao.py:71`).

16. **Item 5 — desligamento precisa existir no processo que decide.**  
    `ORCAMENTO_CONTEXTO=0` já é lido pelo hook, antes de sua trava. Colocar a variável somente no ambiente de outro comando não altera automaticamente esse ambiente. A implementação de `DISJUNTOR=0`, sua abrangência e a leitura de `cota.json` são **[não verificado]**; a cota conhecida pelo handoff atual vem do último aval textual `COTA-*` em PROMPTS (`bin/hook_stop_fila1.py:741`; `bin/handoff_sessao.sh:207`; `contrato.txt:6`).

Quanto à autoria e leitura dos arquivos:

| Arquivo | Quem escreve | Quem lê |
|---|---|---|
| `logs/sessao/pisos.log` | O contrato propõe `orcamento_contexto.registrar_piso`, chamado pelo Stop; implementação **[não verificado]** (`contrato.txt:2`). | Avaliador e ESTADO são consumidores previstos; implementação **[não verificado]** (`contrato.txt:3`; `contrato.txt:5`). |
| `logs/codex/chamadas.log` | Wrapper: `ini` e `fim` (`bin/codex_pergunta.sh:59`; `bin/codex_pergunta.sh:88`). | `codex_limite_estado`; ESTADO consome esse leitor, e CODEX-ESTADO usa seu mapa de sessões (`bin/codex_limite_estado.py:55`; `bin/gerar_estado.py:269`; `bin/gerar_codex_estado.py:176`). |
| `~/.codex/sessions/**/*.jsonl` | O wrapper inicia o CLI; o código que efetivamente grava esses arquivos não está demonstrado: **[não verificado]** (`bin/codex_pergunta.sh:157`). | Leitor de limites e gerador de respostas finais (`bin/codex_limite_estado.py:101`; `bin/gerar_codex_estado.py:80`). |
| `cota.json` | **[não verificado]**. | **[não verificado]**; não confundir com a leitura textual de cota do handoff (`bin/handoff_sessao.sh:207`). |
| `logs/sessao/ultimo_clear.json` | `marco_de_clear.py`, por substituição atômica (`bin/marco_de_clear.py:61`). | `orcamento_contexto.ler_marco`, também chamado pelo Stop (`bin/orcamento_contexto.py:145`; `bin/hook_stop_fila1.py:794`). |
| `orcamento_estado.json` e `orcamento.log` | Hook de Stop (`bin/hook_stop_fila1.py:693`; `bin/hook_stop_fila1.py:717`). | `orcamento_contexto.ler`, consumido pelo ESTADO (`bin/orcamento_contexto.py:228`; `bin/orcamento_contexto.py:235`; `bin/gerar_estado.py:274`). |

A instalação real de PreToolUse/statusLine, o conteúdo de `cota.json` e os selos v3 com mutantes permanecem **[não verificado]**. A ausência desses artefatos nesta cópia não prova ausência no host: `logs/` e `.claude/` são ignorados pelo Git (`.gitignore:53`; `.gitignore:66`).

## 30/10 23:32 · sessao 5e2fcac27123 · - · - · origem=Ronald (tmux)

Achados C1/Stop, somente leitura:

1. **O teto 200.000 do v3 não é a janela do código atual.** Os modelos opus/sonnet/fable usam 1.000.000, haiku-4-5 usa 200.000; o gatilho atual é 60% (`bin/orcamento_contexto.py:35`, `bin/orcamento_contexto.py:44`, `bin/orcamento_contexto.py:209`). Piso v3 de 135.592 → gatilho 167.796: em opus isso dispara perto de 16,8% da janela declarada, não de 60%; piso 865.942 do anexo desliga C1 apesar de ainda caber na janela opus de 1M. Os números dos pisos são apenas o dado anexado [não verificado].

2. **“Piso pós-compact” medido no primeiro Stop mede outra coisa.** O leitor pega o último evento assistant e soma input+cache, não o primeiro turno após compact nem o contexto imediatamente após compact (`bin/orcamento_contexto.py:89`, `bin/orcamento_contexto.py:101`, `bin/orcamento_contexto.py:110`). Um ciclo que trabalha muito antes do primeiro Stop ganha piso inflado; se cruzar 200k, v3 desliga C1 justamente nesse ciclo. O próprio dado anexado diferencia primeiro turno ≤107.621 de pisos 119.361/135.592/155.871 [não verificado].

3. **Sessões diferentes podem contaminar o piso.** O marco é um arquivo global por raiz (`bin/marco_de_clear.py:19`, `bin/marco_de_clear.py:20`), embora grave session_id (`bin/marco_de_clear.py:44`). O leitor devolve session_id (`bin/orcamento_contexto.py:149`), mas Stop extrai somente transcript_path e usa o marco sem casar a sessão (`bin/hook_stop_fila1.py:1018`, `bin/hook_stop_fila1.py:794`, `bin/hook_stop_fila1.py:822`). Cenário: A compacta; B chega ao primeiro Stop; registrar piso sobre esse encaixe pode registrar os tokens de B como pós-compact de A.

4. **Adicionar pisos.log abre uma prova falsa de ESPERANDO se não ampliar a proteção.** `_prova_arquivo` não inclui pisos.log na lista de arquivos do próprio hook (`bin/hook_stop_fila1.py:556`). Cenário: `ESPERANDO: agente-x | arquivo=logs/sessao/pisos.log` já teve mtime observado; um novo ciclo apenda piso antes de avaliar espera; o próprio hook produz o avanço, que passa como prova (`bin/hook_stop_fila1.py:605`, `bin/hook_stop_fila1.py:644`) e libera Stop (`bin/hook_stop_fila1.py:1051`). Isso é cenário da implementação proposta, não ocorrência medida.

5. **O primeiro Stop pode nem chegar ao registro de piso.** O checador do RELATO bloqueia e sai antes do orçamento (`bin/hook_stop_fila1.py:1029`, `bin/hook_stop_fila1.py:1034`, `bin/hook_stop_fila1.py:1037`). Se registrar_piso entrar apenas em `_orcamento_travado`, “primeiro Stop” vira primeiro Stop com RELATO aprovado; o trabalho de correção cresce antes da medida.

6. **“Sem piso C1 não liga” precisa preservar os outros gatilhos.** O orçamento atual é OR entre contexto, seis horas e três deploys (`bin/orcamento_contexto.py:209`, `bin/orcamento_contexto.py:212`, `bin/orcamento_contexto.py:214`). Um retorno antecipado de `avaliar` por ausência de piso também desligaria tempo e marcos. Isso é cuidado de integração, pois registrar_piso e pisos.log não existem na cópia [não verificado].

7. **A trava e o teto atuais precisam continuar envolvendo a nova medição.** Orçamento espera no máximo ~1s pelo lock e então cai no caminho comum (`bin/hook_stop_fila1.py:757`, `bin/hook_stop_fila1.py:764`); exceção também cai nesse caminho (`bin/hook_stop_fila1.py:886`). Um novo leitor de piso com `open` comum pode prender em FIFO; o helper existente evita isso com O_NONBLOCK e exige arquivo regular (`bin/orcamento_contexto.py:57`). Um novo bloqueio por erro antes do contador também pode prender indefinidamente: o contador só roda depois (`bin/hook_stop_fila1.py:1092`), e só libera com `n > 5` (`bin/hook_stop_fila1.py:1104`).

8. **O limite geral do Stop já tem exceção sem teto.** Falha persistente de `relato_afirma_com_prova` bloqueia todos os Stops antes de atualizar contador (`bin/hook_stop_fila1.py:1007`, `bin/hook_stop_fila1.py:1029`). Logo “falha aberto como v2” não descreve qualquer falha do Stop inteiro.

9. **Escritor único não basta para evitar piso duplicado.** O orçamento atual serializa ler/decidir/gravar (`bin/hook_stop_fila1.py:755`, `bin/hook_stop_fila1.py:766`) e grava estado antes da trilha (`bin/hook_stop_fila1.py:717`, `bin/hook_stop_fila1.py:725`). Se registrar_piso apendar antes de persistir “ciclo registrado”, uma falha posterior permite reapendar no próximo Stop; se for chamado fora da trava, dois Stops concorrentes podem registrar o mesmo ciclo. Implementação v3 ausente [não verificado].

Autores/leitores relevantes:

- **pisos.log**: v3 propõe `orcamento_contexto.registrar_piso` como escritor, invocado pelo Stop; avaliador e ESTADO seriam leitores. Nenhum desses usos existe na cópia [não verificado].
- **ultimo_clear.json**: escreve `marco_de_clear.py`, preservando os últimos vinte clear_horas e substituindo atomicamente o arquivo (`bin/marco_de_clear.py:55`, `bin/marco_de_clear.py:57`, `bin/marco_de_clear.py:61`); lê `orcamento_contexto.ler_marco` (`bin/orcamento_contexto.py:145`).
- **orcamento_estado.json / orcamento.log**: escreve o hook (`bin/hook_stop_fila1.py:710`, `bin/hook_stop_fila1.py:686`); lê `orcamento_contexto.ler` (`bin/orcamento_contexto.py:228`, `bin/orcamento_contexto.py:235`), chamado por gerar_estado (`bin/gerar_estado.py:274`).
- A ligação real ao harness está fora da cópia: o RELATO informa que `.claude/settings.json` é local e ignorado (`app/docs/RELATO.md:196`); instalação atual [não verificado].

Não executei os selos: eles criam arquivos temporários (`bin/tests/test_orcamento_contexto.sh:29`, `bin/tests/test_orcamento_contexto.sh:38`).

## 30/10 23:32 · sessao 455436574ba9 · - · - · origem=Ronald (tmux)

- **Wiring e `cota.json` sem prova:** não foram localizados na cópia `bin/disjuntor.py`, `bin/statusline_cota.py`, configuração de `PreToolUse`/`statusLine`, `cota.json` ou seus escritores/leitores **[não verificado]**. A ausência na cópia não prova ausência no host: `logs/` e `.claude/` estão ignorados (`.gitignore:53`, `.gitignore:66`). O ESTADO atual chama apenas os leitores do Codex e do orçamento (`bin/gerar_estado.py:267-276`), nas seções “Codex (limite)” e “Orcamento de contexto” (`bin/gerar_estado.py:321-327`). A “cota vigente” do handoff vem do último aval textual `COTA-*` de PROMPTS, não de `cota.json` (`bin/handoff_sessao.sh:207-211`).

- **Texto Bash não determina papel e volta:** o wrapper usa `CODEX_PAPEL` do ambiente efetivo (`bin/codex_pergunta.sh:44-48`); seu registro contém papel, mas nenhuma volta (`bin/codex_pergunta.sh:53-60`, `bin/codex_pergunta.sh:88-89`). Com `CODEX_VOLTA=final` exportado anteriormente e chamada sem atribuição literal, o contrato assume `meio` e pode negar a FINAL. Variáveis, `source`, helpers e múltiplas chamadas no mesmo Bash tornam insuficiente uma decisão baseada apenas em texto; o parser ainda é **[não verificado]**.

- **Falsos positivos e bypass pelo texto:** `echo 'CODEX_VOLTA=final ...bin/codex_pergunta.sh...'` não executa wrapper; `bash helper.sh` pode executá-lo sem mostrar seu nome no comando externo. Duas chamadas, uma `meio` e outra `final`, no mesmo comando Bash exigem decisões diferentes, enquanto a autorização da ferramenta é para o comando inteiro **[não verificado quanto à implementação do hook]**. A autoridade atual do papel continua sendo o ambiente recebido pelo wrapper (`bin/codex_pergunta.sh:44-48`).

- **ANEXO-ENXUTO por nome literal não controla conteúdo:** o wrapper aceita caminhos de anexos e lê seus conteúdos (`bin/codex_pergunta.sh:32-35`, `bin/codex_pergunta.sh:49-50`, `bin/codex_pergunta.sh:111`). `--anexo "$A"` ou uma cópia de `JUIZES.md` chamada `/tmp/a.txt` podem escapar de um detector literal; uma referência textual que não execute pode ser negada indevidamente. A regra de equivalência de caminhos/conteúdos é **[não verificado]**.

- **Corrida entre autorização e execução:** o wrapper espera gate e slots sem revalidar orçamento (`bin/codex_pergunta.sh:137-153`). Se seis chamadas forem autorizadas a 84%, elas podem continuar executando depois de a primeira elevar o consumo a 85%; o wrapper fixo não consulta B4 novamente (`bin/codex_pergunta.sh:156-158`). O timeout começa somente no `codex exec`, não na espera dos slots (`bin/codex_pergunta.sh:140-158`).

- **“Não há fonte estruturada” não foi demonstrado:** há leitor de transcript estruturado, com `tool_use` e `input` (`bin/gerar_sessao.py:45-75`). Ele escolhe somente o transcript mais novo (`bin/gerar_sessao.py:33-35`) e retorna ferramenta, descrição e comando, sem modelo/tipo (`bin/gerar_sessao.py:71-75`). Usar esse painel como “agentes Claude 24 h por modelo/tipo” perderia sessões e dimensões; a disponibilidade do schema real de Agent para completar a métrica é **[não verificado]**.

## 30/10 23:31 · sessao 76df57d6f03c · - · - · origem=Ronald (tmux)

Achados para consolidação:

1. **A fonte “já publicada” não contém os dados do B4 diário.** `ler_semanal()` devolve somente `plan_type`, `used_percent` e `window_minutes`; descarta `resets_at`, timestamp e registros anteriores. Consumir essa saída conforme o contrato produz “não sei” e libera a volta do meio mesmo com orçamento estourado. O primeiro/último registro do dia exige outro leitor ou ampliação deste. (`bin/codex_limite_estado.py:99`, `bin/codex_limite_estado.py:118`)

2. **“Último” é mtime de arquivo, não horário da medição.** Ordena arquivos pelo mtime, limita a 40 e pega a última linha contendo a substring `rate_limits`; não compara timestamps entre eventos. Um arquivo com medição velha que receba escrita posterior pode substituir a medição nova e abrir/fechar indevidamente. Se os 40 primeiros não contiverem sinal, ignora o 41º. (`bin/codex_limite_estado.py:105`, `bin/codex_limite_estado.py:110`)

3. **“Uso semanal” pode ser janela de cinco horas.** A apresentação sempre começa com “uso semanal”, mas aceita qualquer `window_minutes`, inclusive 300, e não valida que `primary` é semanal. Portanto usar `primary` como orçamento semanal exige confirmar a janela do registro. O selo contém exatamente primary 10080 e secondary 300, mas isso não prova universalidade em produção. (`bin/codex_limite_estado.py:133`, `bin/codex_limite_estado.py:142`; `bin/tests/test_codex_papel_estado.sh:21`)

4. **O texto Bash não basta para papel/volta efetivos.** O wrapper recebe papel do ambiente e valida o valor efetivo; `CODEX_VOLTA` não integra seu protocolo nem suas linhas de registro. Um comando com `CODEX_PAPEL="$P"` ou papel herdado não permite deduzi-lo só do texto; `CODEX_VOLTA=final` herdado será classificado como “meio” pela regra proposta e poderá negar uma final legítima. Busca textual também confunde strings citadas, comandos múltiplos e atribuições em outro escopo com a invocação efetiva. (`bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:48`, `bin/codex_pergunta.sh:53`, `bin/codex_pergunta.sh:88`)

5. **ANEXO por nome no comando não identifica conteúdo.** O wrapper aceita caminhos quaisquer e concatena o conteúdo; `--anexo "$A"` pode entregar `LEIS.md` sem nome literal visível, e uma cópia com outro nome entrega o mesmo conteúdo. Inversamente uma substring em comentário/pergunta não é argumento `--anexo`. (`bin/codex_pergunta.sh:32`, `bin/codex_pergunta.sh:50`, `bin/codex_pergunta.sh:111`)

6. **Há duas linhas, mas o contador atual não duplica chamadas.** Conta apenas `ini`; contar todas as linhas no placar novo duplicará chamadas concluídas. Porém o rótulo atual conta tentativas que nunca chegaram ao Codex: registra `ini` antes de archive/raspagem, cujas falhas terminam rc3/rc4; o CLI é chamado bem depois. (`bin/codex_limite_estado.py:64`, `bin/codex_limite_estado.py:76`; `bin/codex_pergunta.sh:88`, `bin/codex_pergunta.sh:99`, `bin/codex_pergunta.sh:105`, `bin/codex_pergunta.sh:157`)

7. **Casamento por PID permite reaproveitamento e dupla soma futura.** A chave é `(obra, hash12, pid)` sem hora/identificador de chamada; `fins[chave]` sobrescreve o fim anterior. Duas chamadas com PID reciclado podem receber a mesma sessão final; somar seu total por cada `ini` duplica tokens e atribui o fim errado. (`bin/codex_limite_estado.py:63`, `bin/codex_limite_estado.py:67`, `bin/codex_limite_estado.py:87`)

8. **Sessões com rc LIMITE podem desaparecer da medida por papel.** A saída 75 precede a extração de `thread_id`; a linha `fim` registra `-`, e o mapa de sessões exclui `-`. Uma execução que consumiu tokens antes de falhar por limite fica sem atribuição por essa junção. (`bin/codex_pergunta.sh:59`, `bin/codex_pergunta.sh:171`, `bin/codex_pergunta.sh:175`; `bin/codex_limite_estado.py:96`)

9. **`total_token_usage` não é implementado neste leitor.** As funções atuais só contam inícios, associam fins e extraem limites; não calculam tokens por chamada/dia. A semântica cumulativa, cache, subagentes e presença de `token_count` no dado de produção fornecido são **[não verificado]**. (`bin/codex_limite_estado.py:49`, `bin/codex_limite_estado.py:71`, `bin/codex_limite_estado.py:99`)

10. **Diretório configurável rompe a observação fixa.** O wrapper escreve em `CODEX_LOGD`, enquanto o leitor procura `logs/codex/chamadas.log` sob a raiz. Se configurado outro destino, chamadas reais somem do placar e do casamento por sessão. (`bin/codex_pergunta.sh:51`, `bin/codex_pergunta.sh:60`, `bin/codex_pergunta.sh:89`; `bin/codex_limite_estado.py:22`, `bin/codex_limite_estado.py:52`)

Proveniência: o wrapper escreve `chamadas.log` (`bin/codex_pergunta.sh:59`, `bin/codex_pergunta.sh:88`); `codex_limite_estado.py` lê (`bin/codex_limite_estado.py:55`); `gerar_estado.py` consome esse leitor (`bin/gerar_estado.py:269`); `gerar_codex_estado.py` também o usa para origem/papel (`bin/gerar_codex_estado.py:175`). O wrapper inicia o CLI (`bin/codex_pergunta.sh:157`), mas a implementação do escritor de `~/.codex/sessions` não está demonstrada no material auditado **[não verificado]**; os leitores estão em `bin/codex_limite_estado.py:101` e `bin/gerar_codex_estado.py:80`.

## 30/10 23:16 · sessao 8d1c73f2a341 · O261-E-F1 · verificador · origem=Code

**Eu bloquearia o delta pelos achados 1–5.** Os contraexemplos abaixo vêm dos caminhos do código; ocorrência e quantidade em produção, execução da suíte e RED contra `5ff9ec4b`: **[nao verificado]**.

1. **`app/chamados/models.py:212` — guarda sobre coleção velha — bloqueia.**  
   O plano entrega `_f1_ancoradas` ao núcleo, que reutiliza a lista sem consultar novamente (`app/chamados/models.py:359`, `app/chamados/models.py:218`). Contraexemplo: A começa `cancelar()` e encontra zero perguntas; B reaponta uma pergunta viva para A enquanto A ainda está vivo e confirma; A consome a lista vazia e grava `cancelado`. A costura aceita destino vivo e salva imediatamente (`app/chamados/services/fio.py:750`, `app/chamados/services/fio.py:828`, `app/chamados/services/fio.py:830`); o cancelamento não toma uma trava que coordene essa leitura (`app/chamados/models.py:728`). **Rejulgar os objetos da lista não descobre a pergunta que entrou depois.**

2. **`app/chamados/models.py:279` — escrita antes do pré-requisito/atomicidade — bloqueia.**  
   A relavra S2 agora persiste antes do `atomic` que salva o chamado (`app/chamados/models.py:286`, `app/chamados/veredito_lavrado.py:231`). Contraexemplo: `reabrir()` sem transação externa recebe chamado fechado não-furo e pergunta cujo slot não é derivável; S2 grava o veredito usando o destino vivo em memória; o `save()` seguinte do chamado falha. Permanecem chamado fechado e lavra calculada para chamado vivo. Essa entrada chama diretamente o núcleo, e o juiz efetivamente lê o estado do destino (`app/chamados/models.py:1362`, `app/chamados/juizes.py:699`, `app/chamados/juizes.py:700`).

3. **`app/chamados/models.py:279` — ordem de trava invertida — bloqueia.**  
   Há dois caminhos com ordem oposta:
   - A: `abrir()` mantém transação, renasce o encerrado e chega a **pergunta → chamado** (`app/chamados/models.py:843`, `app/chamados/models.py:862`, `app/chamados/models.py:960`, `app/chamados/models.py:279`, `app/chamados/models.py:291`).
   - B: `renovar_contexto()` atualiza o chamado; S143 roda inclusive nesse save sem mudança de status e atualiza a pergunta: **chamado → pergunta** (`app/chamados/services/contexto.py:100`, `app/chamados/services/contexto.py:102`, `app/chamados/signals.py:81`, `app/chamados/signals.py:92`, `app/chamados/services/disputa_emissao.py:1583`).

   Contraexemplo: chamado não-furo fechado, pergunta não materializada em disputa aberta, célula absolvida. A segura a pergunta e espera o chamado; B segura o chamado e espera a pergunta. A guarda de reabertura por premissa regularizada é condicionada a chamado de furo (`app/chamados/models.py:1278`); a célula absolvida permite a B ultrapassar C6 (`app/chamados/juizes.py:1064`, `app/chamados/services/disputa_emissao.py:1547`).

4. **`app/chamados/services/limbo_folgas.py:228` — reata antes do fechamento atômico — bloqueia.**  
   O reaponte acontece antes de entrar na transação de `reconciliar()` (`app/chamados/services/limbo_folgas.py:230`, `app/chamados/models.py:610`), e a porta da costura já salva cada pergunta (`app/chamados/services/fio.py:830`). Contraexemplo: p1 reata; o save de p2 ou do chamado falha. P1 permanece no LIMBO, o furo permanece vivo e a próxima tentativa retorna imediatamente porque `agrupado_em` já foi persistido (`app/chamados/services/limbo_folgas.py:217`, `app/chamados/services/limbo_folgas.py:219`, `app/chamados/services/contexto.py:102`). O contrato declarado exige reata e fechamento no mesmo ato (`app/chamados/models.py:390`).

5. **`app/ponto/services/cartorio.py:758` — consumidor recebe datas fora de ordem — bloqueia.**  
   Fechamentos imediatos entram em `rejulgar` durante o corpo; os adiados entram depois (`app/ponto/services/cartorio.py:1201`, `app/ponto/services/cartorio.py:759`). Contraexemplo: dia 18 adiado e dia 25 fechado imediatamente produzem `[25/10, 18/10]`. A segunda passada usa primeiro/último como limites: início **24/10**, fim **21/10**, `n_dias=-5` (`app/ponto/services/cartorio.py:846`, `app/ponto/services/cartorio.py:871`, `app/ponto/services/cartorio.py:872`, `app/ponto/services/cartorio.py:889`). A grade percorre `range(dias)` e o cartório pula os dias ausentes, portanto não entrega a relavra pretendida (`app/escala/utils.py:1257`, `app/ponto/services/cartorio.py:1111`).

6. **`app/ponto/services/cartorio.py:755` — fechamento indireto escapa da segunda passada — fila.**  
   Contraexemplo: o chamado adiado também é pai da disputa. E2 fecha essa disputa; o sinal reconcilia o pai por outra instância (`app/chamados/reconciliador.py:635`, `app/chamados/reconciliador.py:636`, `app/chamados/signals.py:32`, `app/chamados/signals.py:42`). Quando o laço chama `x.reconciliar()`, ele recarrega o estado terminal e retorna falso; `_fech` fica vazio, sem contabilização nem `rejulgar` (`app/chamados/models.py:632`, `app/chamados/models.py:635`, `app/chamados/models.py:613`, `app/ponto/services/cartorio.py:756`). O sinal do cartório também retorna durante esse julgamento, deixando a impressão calculada com o chamado aberto até outra passagem (`app/chamados/signals.py:174`, `app/ponto/services/cartorio.py:1099`, `app/ponto/services/cartorio.py:1459`).

7. **`app/ponto/services/cartorio.py:1108` — cura sem invalidar julgamento anterior — fila.**  
   A regra mudou em `:1174`, mas `REGRA_VERSOES` continua em 2 e essa versão integra a impressão (`app/ponto/services/cartorio.py:100`, `app/ponto/services/cartorio.py:342`). Contraexemplo: célula já lavrada como `cobrado`, marcos acesos, fio vivo e impressão igual. Sem `forcar`, o fluxo pula **antes** da cura e não chega ao adiamento/E2 (`app/ponto/services/cartorio.py:1108`, `app/ponto/services/cartorio.py:1193`). Relavratura operacional prévia que cubra esses casos: **[nao verificado]**.

8. **`app/chamados/tests/test_o261e_f1_selo_encerramento.py:37` — selo com brecha de mordida, herdada — fila.**  
   O scanner rejeita constantes `False/None/0`, mas aceita presença sintática de chamada e `raise` (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:39`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:43`). Contraexemplo confirmado em memória: acrescentar `and []` à condição do tripwire mantém o selo verdadeiro, embora a guarda nunca execute. Isso limita **esse selo**, sem demonstrar vacuidade da suíte comportamental, que também exige recusa efetiva (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:86`).

O mapa de escrita e leitura dos dados de domínio envolvidos é este:

| Campo ou valor | Quem escreve/produz | Quem lê/julga; outros escritores |
|---|---|---|
| `status_local`, estado anterior/novo | Núcleo: `app/chamados/models.py:222`, persistência em `:291`. Troca temporária/restauração: `:335`, `:348`. | Cartório `app/ponto/services/cartorio.py:1085`; juiz do destino `app/chamados/juizes.py:700`. Conjuntos oficiais: `app/chamados/catalogo/motor.py:24`, `:30`, `:31`. |
| `resolvido_em`, `via_encerramento`, `respondido_por` | `app/chamados/models.py:225`, `:238`, `:239`, `:242`, `:243`. | Reabertura `app/chamados/models.py:1272`; painel `app/chamados/services/painel.py:243`, `:252`; detalhe `app/templates/chamados/detalhe.html:56`. |
| `urgencia`, `urgencia_original`, `atualizado_em` | Ritual `app/chamados/models.py:436`; restauração `:244`; save do núcleo `:223`, `:291`. | O próprio ritual lê em `:445`, `:447`; reabertura recarrega em `:1305`. Outro gate existente, `rebaixar()`, usa o mesmo ritual (`:456`, `:461`). |
| `_f1_ancoradas` | Plano `app/chamados/models.py:359`; limpezas `:211`, `:400`, `:647`. | Tripwire `app/chamados/models.py:210`; relavra `:270`. É memória; problema no achado 1. |
| `_f1_dedup`; payload `dia`, `sobrevivente`, `motivo`, `status_local` | `app/chamados/models.py:699`; inicialização `:641`. | Consumo/limpeza `:644`; emissão da trilha `:652`. |
| `chamado_destino` | Costura `app/chamados/services/fio.py:828`, `:830`. **Também** identidade existente e nascimento em `app/chamados/models.py:2372`, `:2398`. | Âncora `app/chamados/services/fio.py:117`; F1 `app/chamados/models.py:338`; juiz `app/chamados/juizes.py:699`. Os dois arquivos escritores já estão declarados no contrato (`app/core/tests/test_contract_escritores_estado.py:61`). |
| `data_evento`, `celula`, identidade da pergunta | Nascimento/identidade `app/chamados/models.py:2357`, `:2398`; data pela costura `app/chamados/services/fio.py:819`. | Competência F1 `app/chamados/models.py:341`; C6 `app/chamados/juizes.py:1017`, `:1066`; validade da costura `app/chamados/services/fio.py:821`. |
| `disputa.chamado`, `disputa.colaborador`, empresa | Construção da disputa em `app/chamados/services/disputa_emissao.py:527`; relações consumidas pela carga em `app/chamados/models.py:325`. | Âncora `app/chamados/services/fio.py:116`; F1 `app/chamados/models.py:339`, `:341`; novo fechamento da folga `app/chamados/services/validacao.py:249`. |
| `validada_em`, `validada_por` | Validação `app/chamados/services/validacao.py:131`. | Filtro F1 `app/chamados/models.py:323`; C6 `app/chamados/juizes.py:1006`. **Há outros escritores existentes**, inclusive `app/chamados/services/disputa_emissao.py:1575`; estão declarados em `app/core/tests/test_contract_escritores_estado.py:183`. |
| `respondida_em`, `resposta_colab`, tipo/motivo da pergunta | Resposta `app/chamados/services/disputa_emissao.py:1247`; nascimento `app/chamados/models.py:2398`; materializador também carimba `app/chamados/services/materializacao.py:1192`. | C6 `app/chamados/juizes.py:1020`; derivação do slot `:329`; marco perguntado `:842`. |
| `materializada_em`, `materializada_por`, `via_resolucao` | E2 `app/chamados/reconciliador.py:617`; porta admin `app/chamados/models.py:2444`; S143 pela porta existente `app/chamados/services/disputa_emissao.py:1562`. | C6 `app/chamados/juizes.py:1044`, `:1050`; idempotência admin `app/chamados/models.py:2435`. São múltiplas portas preexistentes declaradas no contrato (`app/core/tests/test_contract_escritores_estado.py:83`). |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar()` em `app/chamados/veredito_lavrado.py:227`; save em `:231`. | Validade do cache em `app/chamados/veredito_lavrado.py:179`, `:182`, `:189`. Backfill também declarado como escritor em `app/core/tests/test_contract_escritores_estado.py:77`. |
| `contexto_json.agrupado_em`, `defeito_cadastro` | Solicitação `app/chamados/services/limbo_folgas.py:219`; porta `app/chamados/services/contexto.py:99`, `:102`. | Retry `app/chamados/services/limbo_folgas.py:217`; pai vivo `app/chamados/models.py:758`; fechamento `:688`. |
| `contexto_json.marcos_faltantes`, `dna.marcos`, `ata.lampadas` | Emissor espelha contexto em `app/ponto/services/cartorio.py:1440`; ata produzida em `:1142` e entregue à porta em `:1444`. | Juiz `marcos_acesos`: `app/ponto/services/lastro.py:170`; novo filtro `app/ponto/services/cartorio.py:1174`. |
| `veredito`, `protestos`, `veredito_via`, `veredito_em`, `ata` da célula | Porta `app/ponto/portas/celula.py:690`. | C6 `app/chamados/juizes.py:1054`; E2 `app/chamados/reconciliador.py:594`; a escolha de `cobrado` está em `app/ponto/services/cartorio.py:1174`. |
| `impressao`, `julgada_em`, `ausencia`, `tem_entrada/saida`, `insumos_em` | `app/ponto/portas/celula.py:721`. | Pulo por impressão `app/ponto/services/cartorio.py:1108`; produção dos insumos `:1099`, `:1458`. |
| `_adiados_pela_lampada`, `rejulgar`, contadores e `cods` | `app/ponto/services/cartorio.py:1196`, `:1201`, `:1136`, `:757`. | Consumidos em `app/ponto/services/cartorio.py:745`, `:754`, `:762`; sobra descartada/contada em `:767`. Problemas nos achados 5–6. |
| `hoje`, `agora`, `apply_`, `forcar`, `registrar` | Entradas do ato `app/ponto/services/cartorio.py:709`; borda unitária `:1595`; decisão do fechamento `app/chamados/models.py:630`. | Teto `app/ponto/services/cartorio.py:1065`; pulo `:1108`; escrita `:1441`; plano/trilha `app/chamados/models.py:645`, `:650`. |
| `fechada_em/por`, `resolucao`, `motivo_fechamento` da disputa | Porta `app/chamados/models.py:1791`. | E2 `app/chamados/reconciliador.py:635`; sinal do pai `app/chamados/signals.py:28`. |
| Eventos `estado_chamado`, `fechamento_por_dedup` e notas | Chamadas `app/chamados/models.py:287`, `:652`; escritor da auditoria `app/core/observ.py:134`; nota após fechamento `app/ponto/services/veredito_celula.py:183`. | A própria composição condiciona o dedup à aprovação das guardas (`app/chamados/models.py:645`, `:650`); teste das irmãs confere a nota (`app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:185`). |

Quanto às classes pedidas:

- **Segundo juiz que duplica juiz registrado: nenhum identificado no delta.** Ele chama C6, C15, motor e lastro existentes (`app/chamados/models.py:345`, `app/ponto/services/cartorio.py:1090`, `app/ponto/services/cartorio.py:1174`; registro em `app/core/juizes.py:234`, `:238`, `:247`).
- **Segundo escritor novo fora da porta: nenhum identificado.** As novas chamadas usam costura, fechamento e lavra existentes; `chamado_destino = self` na relavra é memória, pois o save grava somente os três campos S2 (`app/chamados/models.py:278`, `app/chamados/veredito_lavrado.py:231`). Os escritores adicionais preexistentes estão discriminados na tabela.
- **Relógio novo lido fora da borda: nenhum identificado no delta.** E2 usa `now()` como carimbo; seu filtro exige célula que não acusa e sem pendências, fazendo C6 retornar antes do ramo temporal (`app/chamados/reconciliador.py:598`, `:607`, `:617`; `app/chamados/juizes.py:1064`).
- **Derivação paralela de fonte única: nenhum com divergência concreta demonstrada neste delta.** O filtro novo reutiliza `marcos_acesos`; a classificação da competência reutiliza a composição existente (`app/ponto/services/cartorio.py:1174`, `app/chamados/models.py:341`, `app/chamados/services/vigia_fio.py:469`).
- **Selo/RED sem mordida:** brecha específica no achado 8. O teste do evento cobra fechamento final, mas seu espião observa depois de `_gravar_estado` retornar e não mede literalmente cada escrita intermediária (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:378`, `:383`, `:390`). RED executado sem a cura: **[nao verificado]**.
- **Ordem de trava:** achado 3.
- **Pouso antes do pré-requisito:** achados 2 e 4; alcance do acervo no achado 7. Ordem real de deploy/passivo: **[nao verificado]**. [nao verificado]

O lookup também está envelhecido: ainda diz “disputa fecha e SLA: SEM juiz”, enquanto o registro vivo aponta `pode_fechar` e `prazo_estourou`; para esta revisão, valem esses endereços vivos (`app/docs/JUIZES.md:54`, `app/docs/JUIZES.md:14`, `app/core/juizes.py:239`, `app/core/juizes.py:241`).

## 30/10 23:14 · sessao f4fbc1b704dd · - · - · origem=Ronald (tmux)

Achados em `models.py`, todos por leitura; ocorrência em produção [nao verificado].

1. **Pouso antes do pré-requisito — bloqueia.** S2 salva a lavra em `app/chamados/models.py:279`, antes do `atomic` que salva o chamado (`app/chamados/models.py:286`). A escrita efetiva ocorre em `app/chamados/veredito_lavrado.py:231`. `reabrir()` chama esse núcleo sem transação própria (`app/chamados/models.py:1362`). Contraexemplo: chamado fechado não-furo, pergunta não-validada com slot não-derivável; a lavra usa o destino já vivo em memória, depois `save()` do chamado falha. O banco conserva o chamado fechado e a lavra do estado vivo. O juiz usa precisamente esse estado em `app/chamados/juizes.py:699`.

2. **Ordem de trava invertida — bloqueia.** Caminho A: `abrir()` mantém transação (`app/chamados/models.py:843`), encontra encerrado e renasce (`app/chamados/models.py:862`), chama `reabrir()` (`app/chamados/models.py:960`): UPDATE pergunta (`app/chamados/models.py:279`) antes de UPDATE chamado (`app/chamados/models.py:291`). Caminho B: `renovar_contexto()` mantém transação e atualiza chamado (`app/chamados/services/contexto.py:100`), S143 executa mesmo sem alteração de status (`app/chamados/signals.py:81`), e atualiza pergunta (`app/chamados/services/disputa_emissao.py:1583`). Contraexemplo: chamado não-furo fechado, pergunta não-materializada em disputa aberta e célula absolvida; A segura pergunta e espera chamado; B segura chamado e espera pergunta. Um emissor real desse tipo é `app/chamados/services/adesao.py:74`.

3. **Cache enfraquece o tripwire — bloqueia.** O plano guarda lista em `app/chamados/models.py:359`; o núcleo reutiliza sem consultar novamente (`app/chamados/models.py:212`). Contraexemplo: A inicia `cancelar()`, lê zero ancoradas; B reaponta pergunta viva do mesmo colaborador/dia para A ainda vivo e confirma (`app/chamados/services/fio.py:828`); A fecha com a lista vazia. S143 encontra a pergunta, mas preserva a viva (`app/chamados/services/disputa_emissao.py:1547`). Antes do delta, a consulta adicional do tripwire podia detectar esse reapontamento ocorrido antes dele. Nenhuma trava coordena essas duas portas (`app/chamados/models.py:728`; `app/chamados/services/fio.py:814`).

Mapa para consolidação:

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `_f1_ancoradas` | `app/chamados/models.py:359`; limpa em `:211`, `:400`, `:647` | `app/chamados/models.py:210`, `:270` |
| `_f1_dedup` | `app/chamados/models.py:641`, `:699` | `app/chamados/models.py:644`, `:650` |
| `status_local` | núcleo `app/chamados/models.py:222`, persistência `:291`; troca temporária `:335`, restauração `:348` | núcleo `:201`, `:213`, `:265`; gates `:394`, `:633`; juiz `app/chamados/juizes.py:700` |
| Âncora, `validada_em`, `data_evento` | destino existente: `app/chamados/services/fio.py:828`; nascimento e dedup: `app/chamados/models.py:2372`, `:2398` | `app/chamados/models.py:323`, `:338`, `:341`; autoridade da âncora `app/chamados/services/fio.py:117` |
| Lavra: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `app/chamados/veredito_lavrado.py:227` | `app/chamados/veredito_lavrado.py:179`, `:182`, `:189` |
| Pergunta viva | autoridade registrada em `app/core/juizes.py:238` | composição F1 em `app/chamados/models.py:345` |
| Cobrança viva do dia | consulta `app/chamados/juizes.py:1621`; identidade registrada em `app/core/juizes.py:240` | `app/chamados/models.py:367`, `:691` |

**Segundo escritor persistente introduzido pelo trecho de modelos: nenhum.** `chamado_destino = self` é memória (`app/chamados/models.py:278`); a lavra salva somente seus três campos (`app/chamados/veredito_lavrado.py:231`). A troca temporária de status está declarada nos dois selos (`app/chamados/tests/test_contract_status_nucleo.py:9`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:40`).

**Segundo juiz introduzido pelos métodos de modelos: nenhum identificado.** A composição chama C6 (`app/chamados/models.py:345`) e competência entregue pela composição existente (`app/chamados/models.py:341`; `app/chamados/services/vigia_fio.py:469`).

**Callback síncrono tornando o cache velho: nenhum identificado.** O receiver de pergunta agenda o trabalho para `on_commit` (`app/chamados/signals.py:202`); o contraexemplo do achado 3 exige concorrência.

## 30/10 23:14 · sessao 15cd36045653 · - · - · origem=Ronald (tmux)

Achados no cartório:

1. **Fila da segunda passada perde ordem — bloqueia.** Dias já reconciliados entram em `rejulgar` no corpo (`app/ponto/services/cartorio.py:1201`); os adiados entram depois (`app/ponto/services/cartorio.py:758`). Contraexemplo: dia 18 adiado e dia 25 encerrado imediatamente produzem `[25,18]`. O consumidor toma primeiro/último como limites (`app/ponto/services/cartorio.py:846`), gerando início 24/10, fim 21/10 e `n_dias=-5` (`app/ponto/services/cartorio.py:871`, `app/ponto/services/cartorio.py:889`). A grade enumera `range(dias)` (`app/escala/utils.py:1257`), e o cartório pula dia ausente (`app/ponto/services/cartorio.py:1111`). Validei as atribuições extraídas por AST, sem banco nem arquivos. Existência desse lote em produção: [nao verificado].

2. **E2 pode fechar o adiado antes do contador/rejulgamento — fila.** E2 fecha a disputa (`app/chamados/reconciliador.py:635`), cujo signal reconcilia o chamado-pai por outra instância (`app/chamados/signals.py:32`, `app/chamados/signals.py:42`). Se esse pai é o chamado adiado, o posterior `x.reconciliar()` (`app/ponto/services/cartorio.py:755`) recarrega o terminal e devolve `False` (`app/chamados/models.py:632`, `app/chamados/models.py:635`, `app/chamados/models.py:613`). `_fech` fica vazio: não conta nem pede segunda passada (`app/ponto/services/cartorio.py:756`). O signal do cartório também retorna durante julgamento (`app/chamados/signals.py:174`). Resultado concreto: impressão anteriormente gravada com chamado aberto permanece até outro julgamento (`app/ponto/services/cartorio.py:1099`, `app/ponto/services/cartorio.py:1459`).

3. **Cura não alcança impressão antiga intacta — fila.** A regra da lâmpada muda (`app/ponto/services/cartorio.py:1174`), mas a versão continua 2 (`app/ponto/services/cartorio.py:100`). A impressão inclui essa versão (`app/ponto/services/cartorio.py:342`) e o pulo ocorre antes da cura (`app/ponto/services/cartorio.py:1108`). Contraexemplo: célula já lavrada como `cobrado`, todos os marcos acesos, fio vivo e insumos intactos: cron comum pula, não executa adiamento/E2. A própria documentação identifica esse mecanismo de congelamento (`app/ponto/services/cartorio.py:104`). Quantos casos existem: [nao verificado].

Mapa de donos do trecho:

| Dado | Escritor | Leitor/juiz |
|---|---|---|
| `status_local` | núcleo chamado, `app/chamados/models.py:614` | `VIVOS`, registro `app/core/juizes.py:233`; cartório `app/ponto/services/cartorio.py:1085` |
| Perguntas do fio | relação destino `app/chamados/models.py:2596` | `fio_vivo`, cartório `app/ponto/services/cartorio.py:1090`; autoridade `app/core/juizes.py:247` |
| Marcos cobrados, `ata.lampadas` | ata produzida `app/ponto/services/cartorio.py:1142`; persistida pela porta `app/ponto/portas/celula.py:690` | `marcos_acesos`, `app/ponto/services/lastro.py:170` |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata` | `app/ponto/portas/celula.py:690` | C6 `app/chamados/juizes.py:1054`; E2 `app/chamados/reconciliador.py:594` |
| `impressao`, `julgada_em`, `ausencia`, `tem_entrada/saida`, `insumos_em` | `app/ponto/portas/celula.py:721` | pulo `app/ponto/services/cartorio.py:1108` |
| `materializada_em/por`, `via_resolucao` | E2 `app/chamados/reconciliador.py:617` | C6 `app/chamados/juizes.py:1044`; respostas pendentes `app/chamados/models.py:1713` |
| `fechada_em/por`, `resolucao`, `motivo_fechamento` | porta disputa `app/chamados/models.py:1791` | signal pai `app/chamados/signals.py:28` |

No trecho do cartório: **segundo juiz duplicado: nenhum; segundo escritor fora da porta: nenhum; relógio de decisão novo fora da borda: nenhum**. O filtro novo chama o juiz existente `marcos_acesos` (`app/ponto/services/cartorio.py:1174`, `app/ponto/services/lastro.py:153`). E2 usa `now()` como carimbo (`app/chamados/reconciliador.py:617`); seu caminho C6 retorna antes do ramo de janela temporal porque exige célula que não acusa e sem marcos pendentes (`app/chamados/reconciliador.py:594`, `app/chamados/reconciliador.py:607`, `app/chamados/juizes.py:1064`). Não inflaria isso como defeito novo de relógio.

## 30/10 23:14 · sessao 5df2ea80c0a6 · - · - · origem=Ronald (tmux)

Achado bloqueia: `app/chamados/services/limbo_folgas.py:228` reata perguntas antes e fora da transação do fechamento; `reapontar_resolvedora` persiste imediatamente em `app/chamados/services/fio.py:828`, enquanto o `atomic` de `reconciliar` só começa em `app/chamados/models.py:610`. Contraexemplo: p1 reata; o save de p2 ou do fechamento lança erro; p1 permanece no LIMBO, o furo permanece vivo e a próxima tentativa retorna antes de reparar porque `agrupado_em` já foi gravado (`app/chamados/services/limbo_folgas.py:217`, `app/chamados/services/contexto.py:99`). Classe: escrita antes do pré-requisito/atomicidade da reata com fechamento; a própria porta exige esse par no mesmo ato (`app/chamados/models.py:390`). Ocorrência em produção: [nao verificado].

Achado fila: o selo aceita guarda comprovadamente inoperante. Extraí e executei em memória a função de `app/chamados/tests/test_o261e_f1_selo_encerramento.py:24`; tanto a condição original quanto mutantes `and []` e `if novo == "__estado_inexistente__"` retornaram `True`. Ele só rejeita constantes `False/None/0` (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:37`) e depois verifica presença sintática da chamada e do raise (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:39`). Isso é insuficiência específica do selo; os testes comportamentais ainda podem detectar esses mutantes (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`).

O espião do evento observa depois de `_gravar_estado` voltar, portanto não demonstra literalmente “em nenhuma escrita” (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:378`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:383`). Porém S143 atual só materializa perguntas que C6 já considera mortas, logo não comprova mascaramento da violação nessa fixture (`app/chamados/services/disputa_emissao.py:1547`). Não classificaria o RED inteiro como sem mordida: ele exige fechamento final, e a base anterior mantém o ciclo que deixa o chamado aberto (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:390`); execução contra a base: [nao verificado].

Mapa dos campos destes serviços:

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `contexto_json.agrupado_em`, `defeito_cadastro` | Solicitação em `app/chamados/services/limbo_folgas.py:219`; persistência em `app/chamados/services/contexto.py:99` | Guarda de repetição em `app/chamados/services/limbo_folgas.py:217`; pai vivo em `app/chamados/models.py:758`; via em `app/chamados/juizes.py:1166` |
| `chamado_destino` | Porta chamada em `app/chamados/services/limbo_folgas.py:229`; atribuição/save em `app/chamados/services/fio.py:828` | Âncora em `app/chamados/services/fio.py:117`; legitimidade pelo juiz em `app/chamados/juizes.py:1687` |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` da reata | Lavra/save da mesma porta em `app/chamados/services/fio.py:829` | C6 consulta seu resultado por `app/chamados/juizes.py:791` [leitores adicionais não recenseados nesta subanálise] |
| `perg.disputa.colaborador`, `perg.data_evento`, `autor` | O delta recebe esses valores em `app/chamados/services/validacao.py:249` | Novo fechamento do fio em `app/chamados/services/validacao.py:271`; seleção por colaborador/data em `app/ponto/services/veredito_celula.py:174` |
| `validada_em`, `validada_por` | Porta de validação em `app/chamados/services/validacao.py:131` | C6 em `app/chamados/juizes.py:1006` |
| `materializada_em`, `materializada_por`, `via_resolucao` | `fechar_por_admin` em `app/chamados/models.py:2444`, chamada pelo fio em `app/ponto/services/veredito_celula.py:177` | Idempotência em `app/chamados/models.py:2435`; C6 em `app/chamados/juizes.py:1039` |
| `status_local` | Fio chama a porta `reconciliar` em `app/ponto/services/veredito_celula.py:183`; núcleo em `app/chamados/models.py:614` | Universo vivo em `app/ponto/services/veredito_celula.py:148` |
| Nota de fechamento | Só após reconciliar retornar verdadeiro em `app/ponto/services/veredito_celula.py:183` | Teste existente exige uma nota para quatro aceites em `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:185` |

Nos dois serviços examinados, nenhum segundo escritor direto novo: o delta chama as portas existentes de contexto, reata e fechamento (`app/chamados/services/limbo_folgas.py:219`, `app/chamados/services/limbo_folgas.py:229`, `app/chamados/services/validacao.py:271`). O teste existente das quatro irmãs já cobra chamado resolvido e uma nota, dando testemunha ao novo fechamento repetido da folga (`app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:167`).

Demais classes neste recorte: segundo juiz duplicado — nenhum; relógio novo fora da borda — nenhum; derivação paralela — nenhum; ordem de trava — nenhum. As novas operações relevantes chamam autoridades existentes e não acrescentam aquisição explícita de trava (`app/chamados/services/limbo_folgas.py:227`, `app/chamados/services/validacao.py:270`).

## 30/10 23:07 · sessao 86789d0f7e2d · DISJUNTOR · verificador · origem=Code

**O encaixe direto quebra principalmente na liberação de CLEAR, nas guardas de trabalho em curso e na identidade da sessão.** A máquina atual ainda usa os gatilhos e a janela antigos. (`bin/hook_stop_fila1.py:827`, `bin/hook_stop_fila1.py:866`, `bin/orcamento_contexto.py:44`)

Os cenários abaixo são contraprovas por leitura do código; ocorrência efetiva em produção: **[não verificado]**.

1. **C1 dispara pela medida errada.** Hoje dispara por **60% da janela OU 6 horas OU três deploys**, não por 150.000 tokens. Cenário: marco recente, nenhum deploy, último assistente principal com 150.000 tokens e janela de 1 milhão → não dispara; Haiku com janela de 200.000 e 120.000 tokens → dispara antes do contratado. (`bin/orcamento_contexto.py:36`, `bin/orcamento_contexto.py:41`, `bin/orcamento_contexto.py:209`, `bin/orcamento_contexto.py:212`)

2. **Libera CLEAR sem handoff novo — ou sem handoff algum.** Stop S1 grava `disparo`; antes de S2 escreve-se somente `PAREI: ! CLEAR` no topo. S2 libera verificando o disparo e o prazo, sem consultar `HANDOFF-SESSAO.md`. Portanto, apenas trocar a mensagem por “SO bin/handoff_sessao.sh” não cria a condição de liberação. (`bin/hook_stop_fila1.py:878`, `bin/hook_stop_fila1.py:827`, `bin/hook_stop_fila1.py:835`)

3. **Continua liberando depois dos dez minutos.** Com disparo ativo, S2 vê CLEAR em t0; S3 em t0+11 minutos ainda libera. O prazo atual é 30 minutos e começa no primeiro Stop que observa o PAREI. (`bin/orcamento_contexto.py:47`, `bin/hook_stop_fila1.py:777`, `bin/hook_stop_fila1.py:829`, `bin/hook_stop_fila1.py:836`)

4. **As guardas ficam depois da liberação de um disparo ativo.** Cenário: S1 dispara sem trabalho em curso; começa uma suite/push; S2 encontra CLEAR e libera antes de consultar trabalho em curso. A avaliação de ESPERANDO/suite/push protege o primeiro disparo, mas não essa liberação posterior. Acrescentar DIFF somente junto dessas guardas repetiria o defeito. (`bin/hook_stop_fila1.py:827`, `bin/hook_stop_fila1.py:850`, `bin/hook_stop_fila1.py:866`)

5. **O sinal novo não cobre automaticamente o DIFF existente.** `diff_janela_he_total.sh` toma `logs/sombra.lock`; seu caminho mostrado não publica `logs/sessao/diff_apply/<obra>`. Cenário: esse DIFF roda, o sinal novo está ausente e chega Stop com gatilho alto → a guarda baseada somente no sinal novo deixa passar o disparo no meio do DIFF. O handoff já consulta a trava efetivamente usada. (`bin/diff_janela_he_total.sh:17`, `bin/diff_janela_he_total.sh:38`, `bin/sombra_trava.sh:16`, `bin/sombra_trava.sh:26`, `bin/handoff_sessao.sh:223`) Escritor e limpeza do novo sinal: **[não verificado]**.

6. **Agente sem entrega não deixa C1 PENDENTE hoje.** Cenário: existe `subagents/agent-X.jsonl` sem entrega, topo sem ESPERANDO e nenhuma suite/push; o avaliador alcança `DISPARO`, porque os adiamentos atuais são ESPERANDO provado, PAREI comum e suite/push. Não há leitura desses JSONL nessa decisão. (`bin/hook_stop_fila1.py:866`, `bin/hook_stop_fila1.py:878`) Formato de “turno final”, escritor desses JSONL e sobrevivência ao `/clear`: **[não verificado]**.

7. **CLEAR de outra sessão pode atender o disparo errado.** Cenário condicional: A dispara; B usa a mesma RAIZ e grava SessionStart `clear`; próximo Stop de A aceita o marco de B, registra ATENDIDO e elimina o disparo de A. O Stop extrai somente `transcript_path`; embora o marco guarde `session_id`, não há comparação com a identidade do Stop. (`bin/hook_stop_fila1.py:1018`, `bin/marco_de_clear.py:20`, `bin/marco_de_clear.py:45`, `bin/hook_stop_fila1.py:802`, `bin/hook_stop_fila1.py:810`, `bin/hook_stop_fila1.py:821`)

8. **Compact encerra e rearma sem `/clear`.** S1 dispara; ocorre SessionStart `compact`; S2 encontra marco novo, registra RESET e apaga o disparo anterior. Assim, conservar a máquina atual permite escapar da cobrança de CLEAR por compact. O painel também passa a mostrar tempo desde compact sob o rótulo “desde o clear”. (`bin/marco_de_clear.py:32`, `bin/marco_de_clear.py:44`, `bin/hook_stop_fila1.py:819`, `bin/hook_stop_fila1.py:821`, `bin/orcamento_contexto.py:245`, `bin/orcamento_contexto.py:265`)

9. **Estado ilegível permite contar novamente o mesmo ciclo.** S1 grava DISPARO; `orcamento_estado.json` fica truncado antes de S2; a medida continua alta. S2 transforma a leitura inválida em `{}` e grava outro DISPARO. O flock serializa acessos, mas não recupera a identidade perdida do disparo. (`bin/hook_stop_fila1.py:779`, `bin/hook_stop_fila1.py:783`, `bin/hook_stop_fila1.py:878`, `bin/hook_stop_fila1.py:880`)

10. **“Caminho de hoje com TETO” não garante saída.** Se o checador do RELATO retorna erro ou excede o timeout, cada Stop bloqueia **antes** de C1 e do contador do TETO. Repetir essa condição prende o encerramento independentemente do tratamento de exceção do disjuntor. (`bin/hook_stop_fila1.py:1007`, `bin/hook_stop_fila1.py:1009`, `bin/hook_stop_fila1.py:1029`, `bin/hook_stop_fila1.py:1034`) Separadamente, SessionStart pode esperar indefinidamente pelo flock do marco: sua aquisição é bloqueante, sem prazo no código. (`bin/marco_de_clear.py:38`, `bin/marco_de_clear.py:40`)

11. **“Trabalho em curso” pode ser de outra obra, ou ficar invisível.** O detector varre `ps` global por texto, sem identidade de obra/sessão. Uma suite alheia mantém C1 PENDENTE e impede a saída pelo TETO; falha de `ps` vira `False`, podendo autorizar o disparo durante trabalho real não observado. (`bin/hook_stop_fila1.py:386`, `bin/hook_stop_fila1.py:392`, `bin/hook_stop_fila1.py:395`, `bin/hook_stop_fila1.py:388`, `bin/hook_stop_fila1.py:869`, `bin/hook_stop_fila1.py:1104`)

12. **O painel atual não entrega as medidas prometidas.**
    - Soma ATENDIDO/SEGUIU da trilha inteira, sem janela de 24 horas; não conta DISPARO. Um evento antigo continua no total. (`bin/orcamento_contexto.py:235`, `bin/orcamento_contexto.py:239`)
    - Publica percentual/modelo/janela, sem os tokens e o horário da última observação. Stop barrado pela prova, orçamento desligado ou lock ocupado conserva uma medida anterior, embora o rótulo proposto diga “último Stop”. (`bin/orcamento_contexto.py:255`, `bin/hook_stop_fila1.py:823`, `bin/hook_stop_fila1.py:1034`, `bin/hook_stop_fila1.py:753`, `bin/hook_stop_fila1.py:765`)
    - Subtrair CLEAR-LIBERA de ATENDIDO mede intervalo entre observações por Stops. Exemplo: primeiro Stop vê PAREI às 12:05, `/clear` às 12:08, próximo Stop às 12:20 → diferença dos eventos dá 15 minutos, não três. ATENDIDO não registra a hora exata do clear; ela está no marco. (`bin/hook_stop_fila1.py:831`, `bin/hook_stop_fila1.py:817`, `bin/hook_stop_fila1.py:694`, `bin/marco_de_clear.py:56`)
    - O formatador atual não calcula cache diário nem média de espera; `gerar_estado.py` chama esse formatador. O valor de referência “390M” e sua deduplicação não são demonstrados pelo dado fornecido. (`bin/orcamento_contexto.py:265`, `bin/gerar_estado.py:274`) **[não verificado]**

13. **B3 exige uma convenção que o verificador atual não exige.** A instrução pede “id da obra”, sem obrigar `OBRA=<id>`. Cenário permitido: prompt “verifique a obra O261…” e resposta cuja primeira linha é `REPROVADO`; o reconhecimento contratado, condicionado a `OBRA=O261`, perde essa reprovação. (`.claude/agents/verificador.md:3`, `.claude/agents/verificador.md:18`) Payload efetivo de SubagentStop, recuperação do prompt e deduplicação por agente/entrega: **[não verificado]**.

14. **B1/B2/B4 ainda não têm comportamento auditável nesse material.** Não localizei `bin/disjuntor.py`, `bin/statusline_cota.py`, `bin/tests/test_disjuntor.sh` ou `.claude/settings.json`; implementação, instalação e desligamento por `DISJUNTOR=0`: **[não verificado]**. O contrato fornecido não define medida/limiar de B1, regra de B2 ou limiar de B4; sem isso não há como construir um caso numérico de “deveria negar”. **[não verificado]** A redação de frescor de B4 também precisa precisão: comparar literalmente mtime com um `resets_at` futuro classificaria como velha uma amostra recém-gravada antes do reset; payload e comparação efetivos: **[não verificado]**.

Quanto aos donos dos arquivos, a distinção entre código existente e intenção do contrato é esta:

| Arquivo | Quem ESCREVE | Quem LÊ |
|---|---|---|
| `logs/sessao/orcamento_estado.json` | Stop, `_orc_salva`, substituição atômica. (`bin/hook_stop_fila1.py:717`) | Stop e leitor do painel. (`bin/hook_stop_fila1.py:779`, `bin/orcamento_contexto.py:228`) |
| `logs/sessao/orcamento.log` | Stop, `_orc_trilha`. (`bin/hook_stop_fila1.py:693`) | `orcamento_contexto.ler`, chamado por `gerar_estado.py`. (`bin/orcamento_contexto.py:235`, `bin/gerar_estado.py:276`) |
| `logs/sessao/ultimo_clear.json` | `marco_de_clear.py`; também lê o anterior para conservar clears. (`bin/marco_de_clear.py:50`, `bin/marco_de_clear.py:61`) | `orcamento_contexto.ler_marco`, usado pelo Stop e painel. (`bin/orcamento_contexto.py:145`, `bin/hook_stop_fila1.py:794`, `bin/orcamento_contexto.py:232`) |
| `app/docs/HANDOFF-SESSAO.md` | `handoff_sessao.sh`. (`bin/handoff_sessao.sh:296`, `bin/handoff_sessao.sh:300`) | O selo exige `cat` em SessionStart clear/compact; configuração instalada **[não verificado]**. (`bin/tests/test_handoff_sessao.sh:69`, `bin/tests/test_handoff_sessao.sh:72`) |
| `logs/sessao/diff_apply/<obra>` | Não identificado no código inspecionado. **[não verificado]** | Guarda pretendida dos disjuntores. Implementação **[não verificado]**. |
| `subagents/agent-*.jsonl` | Escritor efetivo **[não verificado]**. | Leitor pretendido de entrega em C1; implementação **[não verificado]**. |
| `logs/sessao/cota.json` | Contrato atribui à statusLine; implementação **[não verificado]**. | Contrato atribui a B4; implementação **[não verificado]**. |
| `logs/sessao/disjuntor_estado.json` | Contrato atribui ao disjuntor com flock; implementação **[não verificado]**. | Reavaliação/contadores do disjuntor; implementação **[não verificado]**. |
| `logs/sessao/disjuntor.log` | Contrato atribui ao disjuntor; implementação **[não verificado]**. | Leitura nova por `gerar_estado.py` **[não verificado]**; a integração atual chama o leitor do ORÇAMENTO. (`bin/gerar_estado.py:274`) |

Há ainda uma exceção que precisa permanecer explícita: após disparar, sem topo CLEAR, a máquina volta ao caminho normal e pode liberar pelo TETO no sexto bloqueio contado, sem handoff. Isso corresponde à válvula existente, mas impede interpretar “PAREI só libera com handoff” como garantia de **toda** liberação do Stop. (`bin/hook_stop_fila1.py:850`, `bin/hook_stop_fila1.py:1097`, `bin/hook_stop_fila1.py:1104`, `bin/hook_stop_fila1.py:1113`)

## 30/10 23:06 · sessao c03f8040050b · - · - · origem=Ronald (tmux)

Achados para consolidar, distinguindo comportamento atual de integração ainda não verificável:

1. **O sinal de DIFF proposto não acompanha ao menos um DIFF atual.** `diff_janela_he_total.sh` adquire a trava da sombra (`bin/diff_janela_he_total.sh:17-18`); o escritor mantém `logs/sombra.lock` por descritor/flock (`bin/sombra_trava.sh:16`, `bin/sombra_trava.sh:26-38`). O handoff lê essa trava (`bin/handoff_sessao.sh:223-233`). Cenário: esse DIFF está rodando, não existe `logs/sessao/diff_apply/<obra>`, chega Stop acima do limiar; uma implementação que só consulta o sinal novo considera ausente a proteção enquanto o DIFF existente continua. Escritor/leitor atuais do novo sinal: **[não verificado]**.

2. **O estado de CLEAR não é isolado por sessão.** Stop extrai apenas `transcript_path` do payload (`bin/hook_stop_fila1.py:1016-1023`); estado e trilha são únicos por RAIZ (`bin/hook_stop_fila1.py:675-676`). `marco_de_clear.py` grava o `session_id`, mas no mesmo arquivo global (`bin/marco_de_clear.py:20`, `bin/marco_de_clear.py:44-61`). Cenário: sessão A dispara; sessão B dá `/clear`; próximo Stop A lê outro `marco_ref` e pode registrar ATENDIDO pelo clear B; compact B produz RESET e rearmamento (`bin/hook_stop_fila1.py:800-821`). Ocorrência real em produção: **[não verificado]**.

3. **O reconhecimento B3 proposto exige um identificador que a instrução atual do verificador não obriga.** A descrição pede “id da obra”, sem exigir a sintaxe `OBRA=<id>` (`.claude/agents/verificador.md:3`); a saída exige primeira linha APROVADO/REPROVADO (`.claude/agents/verificador.md:18`). Cenário permitido pela instrução atual: prompt “verifique a obra O261…” e resposta `REPROVADO`; um contador condicionado a `OBRA=O261` não associa essa reprovação. Payload efetivo de SubagentStop e implementação do contador: **[não verificado]**.

4. **Não há prova da instalação dos novos hooks nesta cópia.** `.claude/settings.json` não foi encontrado; a configuração é local e ignorada pelo Git (`.gitignore:66`; `bin/tests/test_handoff_sessao.sh:15-19`). Esse selo verifica Stop, PreCompact e SessionStart clear/compact (`bin/tests/test_handoff_sessao.sh:49-79`), sem validar PreToolUse, SubagentStop ou statusLine nesse trecho. Instalação real do contrato: **[não verificado]**.

5. **B1/B2/B4 e a contagem B3 não podem receber veredicto de implementação atual.** `bin/disjuntor.py`, `bin/statusline_cota.py` e `bin/tests/test_disjuntor.sh` não foram encontrados na inspeção: **[não verificado]**. O contrato fornecido também não apresenta a medida/limiar de B1, definição operacional de B2 ou limiar de B4: **[não verificado]**. Não atribuir ao harness campos, reentregas ou garantias que o dado não mostrou.

Escritores/leitores dos estados novos:

| Arquivo citado | Escritor/leitor verificável |
|---|---|
| `logs/sessao/cota.json` | Statusline escritor; disjuntor leitor somente no contrato; implementação **[não verificado]**. |
| `logs/sessao/disjuntor_estado.json` | Disjuntor com flock somente no contrato; implementação **[não verificado]**. |
| `logs/sessao/disjuntor.log` | Disjuntor escritor e `gerar_estado.py` leitor somente no contrato; implementação **[não verificado]**. |
| `logs/sessao/diff_apply/<obra>` | Contrato não nomeia escritor; leitor/implementação **[não verificado]**. |
| `subagents/agent-*.jsonl` | Harness escritor e consumidor de “turno final” **[não verificado]**; o dado não inclui exemplar nem definição de entrega. |

Complementos verificáveis: `logs/sessao/ultimo_clear.json` é lido e substituído pelo escritor `marco_de_clear.py` (`bin/marco_de_clear.py:50-61`); `orcamento_estado.json` é escrito pelo Stop (`bin/hook_stop_fila1.py:710-726`); `orcamento.log` recebe os eventos após a gravação do estado (`bin/hook_stop_fila1.py:724-726`).

## 30/10 23:05 · sessao 8e779091d3c1 · - · - · origem=Ronald (tmux)

Achados de C1 e Stop, por inspeção estática:

1. **O gatilho atual mede outra regra.** São 60% da janela **ou** 6 h **ou** três deploys, com expiração de 30 min: `bin/orcamento_contexto.py:44`, `bin/orcamento_contexto.py:209`. Com janela de 1 milhão e 150 mil tokens, idade abaixo de 6 h e menos de três deploys, C1 não dispara; com Haiku de 200 mil e 120 mil tokens, dispara abaixo dos 150 mil contratados: `bin/orcamento_contexto.py:36`, `bin/orcamento_contexto.py:41`, `bin/orcamento_contexto.py:110`.

2. **CLEAR libera sem HANDOFF e por 30 min.** Stop S1 grava `disparo`; S2 encontra `PAREI: ! CLEAR` e libera exclusivamente pela existência do disparo, sem verificar HANDOFF nem a idade dele; S3 aos 11 min ainda libera: `bin/hook_stop_fila1.py:827`, `bin/hook_stop_fila1.py:835`, `bin/hook_stop_fila1.py:836`, `bin/orcamento_contexto.py:47`. O HANDOFF é escrito em `app/docs/HANDOFF-SESSAO.md` por `bin/handoff_sessao.sh:18`; essa exigência não aparece na decisão de liberação: `bin/hook_stop_fila1.py:827`.

3. **As guardas só protegem o primeiro disparo.** ESPERANDO/suite/push são consultados depois do tratamento de `disparo` ativo e CLEAR: `bin/hook_stop_fila1.py:827`, `bin/hook_stop_fila1.py:850`, `bin/hook_stop_fila1.py:866`. Cenário: S1 dispara sem suite; inicia-se suite; S2 com CLEAR libera antes de consultar a suite: `bin/hook_stop_fila1.py:835`. DIFF/apply e entrega de subagents não são consultados na decisão de disparo, cujos únicos adiamentos são ESPERANDO, PAREI comum e suite/push: `bin/hook_stop_fila1.py:866`.

4. **TETO pode liberar o ciclo sem HANDOFF ou CLEAR.** Após S1 disparar, Stops seguintes sem CLEAR caem no caminho normal com lembrete: `bin/hook_stop_fila1.py:850`, `bin/hook_stop_fila1.py:1116`. Mantendo HEAD e sem suite, o contador cresce e libera quando `n > 5`, isto é, no sexto Stop contado; não verifica HANDOFF nessa saída: `bin/hook_stop_fila1.py:1097`, `bin/hook_stop_fila1.py:1104`, `bin/hook_stop_fila1.py:1113`.

5. **Há bloqueio sem TETO anterior ao instrumento.** Se `relato_afirma_com_prova` falha, excede timeout ou retorna não zero, o Stop bloqueia antes de avaliar C1 e antes de incrementar o TETO: `bin/hook_stop_fila1.py:1007`, `bin/hook_stop_fila1.py:1009`, `bin/hook_stop_fila1.py:1029`. Repetir esse estado repete bloqueio indefinidamente; portanto “caminho de hoje com TETO” não garante saída dessa sessão: `bin/hook_stop_fila1.py:1034`.

6. **Processos de outra obra podem manter C1 pendente e desativar TETO.** A detecção examina todos os argumentos de `ps`, sem escopo de obra/sessão, por substring `manage.py test` ou `git push`: `bin/hook_stop_fila1.py:386`, `bin/hook_stop_fila1.py:392`, `bin/hook_stop_fila1.py:395`. Um desses processos alheios causa PENDENTE e impede liberação por TETO enquanto persistir: `bin/hook_stop_fila1.py:869`, `bin/hook_stop_fila1.py:1092`, `bin/hook_stop_fila1.py:1104`. Se `ps` falha, retorna False e pode disparar durante trabalho real não observado: `bin/hook_stop_fila1.py:387`.

7. **Um CLEAR de outra sessão atende e rearma a sessão errada.** O payload Stop usa somente `transcript_path`: `bin/hook_stop_fila1.py:1018`. O marco é global e recebe o `session_id` do escritor: `bin/marco_de_clear.py:20`, `bin/marco_de_clear.py:45`. Cenário: A dispara; B dá `/clear`; o próximo Stop de A encontra nova referência, registra ATENDIDO pelo clear de B e apaga o disparo de A, sem comparar identidades: `bin/hook_stop_fila1.py:802`, `bin/hook_stop_fila1.py:810`, `bin/hook_stop_fila1.py:817`, `bin/hook_stop_fila1.py:821`. Se A continua acima do limiar, ainda pode disparar novamente nessa avaliação: `bin/hook_stop_fila1.py:822`, `bin/hook_stop_fila1.py:878`.

8. **Estado ilegível permite redisparo/contagem duplicada.** `orcamento_estado.json` truncado ou JSON inválido vira `{}`: `bin/hook_stop_fila1.py:779`, `bin/hook_stop_fila1.py:783`. Se S1 já registrou DISPARO, o estado corrompe antes de S2 e a medida continua alta, S2 registra outro DISPARO no mesmo ciclo; flock não recupera a memória perdida: `bin/hook_stop_fila1.py:878`, `bin/hook_stop_fila1.py:880`.

9. **“Contexto no último Stop” pode ser medida antiga.** `ultima_medida` só atualiza dentro da avaliação do orçamento: `bin/hook_stop_fila1.py:822`. Stop bloqueado por prova, orçamento desligado ou lock ocupado não atualiza a medida: `bin/hook_stop_fila1.py:1034`, `bin/hook_stop_fila1.py:753`, `bin/hook_stop_fila1.py:765`. O painel reutiliza essa medida e não mostra seu horário: `bin/orcamento_contexto.py:243`, `bin/orcamento_contexto.py:253`.

10. **Os contadores publicados não são disparos em 24 h nem espera média.** O leitor soma todas as linhas ATENDIDO/SEGUIU da trilha sem corte temporal e publica os totais; não calcula duração: `bin/orcamento_contexto.py:235`, `bin/orcamento_contexto.py:239`, `bin/orcamento_contexto.py:265`. Além disso, “desde o clear” pode usar compact, porque a base é qualquer marco e o rótulo permanece clear: `bin/orcamento_contexto.py:199`, `bin/orcamento_contexto.py:265`.

Escritores/leitores atuais pertinentes:

- `logs/sessao/orcamento_estado.json`: escreve o hook por substituição atômica e lê o próprio hook; `orcamento_contexto.ler` lê para publicação: `bin/hook_stop_fila1.py:717`, `bin/hook_stop_fila1.py:779`, `bin/orcamento_contexto.py:228`.
- `logs/sessao/orcamento.log`: escreve o hook; `orcamento_contexto.ler` lê; `gerar_estado.py` chama esse leitor: `bin/hook_stop_fila1.py:693`, `bin/orcamento_contexto.py:235`, `bin/gerar_estado.py:274`.
- `logs/sessao/ultimo_clear.json`: escreve e lê o marco para conservar os últimos clears; lê o avaliador, chamado pelo hook e pelo painel: `bin/marco_de_clear.py:50`, `bin/marco_de_clear.py:61`, `bin/orcamento_contexto.py:145`, `bin/hook_stop_fila1.py:794`.
- Locks do orçamento e do marco: adquiridos pelos respectivos escritores; o orçamento usa aquisição não bloqueante, o marco usa espera bloqueante: `bin/hook_stop_fila1.py:755`, `bin/hook_stop_fila1.py:759`, `bin/marco_de_clear.py:38`, `bin/marco_de_clear.py:40`.

## 30/10 23:05 · sessao c9d7251a31dd · - · - · origem=Ronald (tmux)

Achados de medição e estado, por cenário hipotético derivado do código:

1. **C1 não tem o gatilho contratado de 150.000 tokens.** Continua com 60% da janela, 6 horas ou 3 marcos (`bin/orcamento_contexto.py:44`, `bin/orcamento_contexto.py:209`). Assim, último assistente principal de 150.000 tokens no modelo de janela 1M, marco recente e nenhum deploy não dispara; 120.000 no Haiku 200k dispara antes de 150.000 (`bin/orcamento_contexto.py:36`, `bin/orcamento_contexto.py:41`). Modelo desconhecido com 200.000 tokens retorna tokens mais “não sei”, mas não percentual: não dispara por contexto (`bin/orcamento_contexto.py:115`, `bin/orcamento_contexto.py:209`).

2. **Janela é de 30 minutos; não 10.** Disparo ativo + `PAREI: ! CLEAR` visto em t0: Stop em t0+11 min ainda libera; o código usa `EXPIRA_CLEAR_MIN=30` e comparação `<= janela` (`bin/orcamento_contexto.py:47`, `bin/hook_stop_fila1.py:777`, `bin/hook_stop_fila1.py:836`). O início também é o primeiro Stop que lê PAREI, e não o disparo nem a escrita da linha (`bin/hook_stop_fila1.py:829`).

3. **O painel não mede os rótulos prometidos.** Ele conta ATENDIDO/SEGUIU da trilha inteira, sem corte de 24h e sem contar DISPARO; apresenta percentual/modelo/janela, sem os tokens da última medida; não calcula média de espera nem cache diário (`bin/orcamento_contexto.py:233`, `bin/orcamento_contexto.py:255`, `bin/orcamento_contexto.py:265`). `gerar_estado.py` usa precisamente esse leitor/formatador (`bin/gerar_estado.py:274`). Cenário: ATENDIDO de ontem segue no contador de hoje; DISPARO atual sem desfecho não aparece como contagem de disparos (`bin/orcamento_contexto.py:239`).

4. **`orcamento.log` não contém os dois horários exatos necessários à média PAREI→/clear.** CLEAR-LIBERA é carimbado pelo Stop que viu PAREI; ATENDIDO só é apendado no Stop que percebe o novo marco, com detalhe sem o horário do clear (`bin/hook_stop_fila1.py:831`, `bin/hook_stop_fila1.py:817`). Cenário: PAREI escrito 12:00, primeiro Stop 12:05, /clear 12:08, próximo Stop 12:20; subtrair os eventos daria 15 minutos, enquanto escrita→clear foi 8. O horário real está em `ultimo_clear.json`, não na linha ATENDIDO (`bin/marco_de_clear.py:44`, `bin/marco_de_clear.py:56`).

5. **“Desde o clear” também significa desde compact.** O escritor grava tanto `clear` como `compact`; o leitor toma sempre `marco['hora']` como base e a frase imprime “desde o clear” (`bin/marco_de_clear.py:32`, `bin/marco_de_clear.py:44`, `bin/orcamento_contexto.py:245`, `bin/orcamento_contexto.py:265`). Cenário: seis horas sem /clear, compact agora; painel apresenta quase zero horas “desde o clear” (`bin/orcamento_contexto.py:247`).

6. **Estado global permite que um clear de outra sessão atenda o disparo.** Marco e estado são únicos por RAIZ (`bin/marco_de_clear.py:20`, `bin/hook_stop_fila1.py:675`); session_id participa da referência, mas o hook não exige igualdade com a sessão do Stop e aceita qualquer clear posterior ao disparo (`bin/orcamento_contexto.py:149`, `bin/hook_stop_fila1.py:802`, `bin/hook_stop_fila1.py:810`). Cenário condicional: A dispara; B grava SessionStart clear na mesma RAIZ; próximo Stop de A registra ATENDIDO e rearma A (`bin/hook_stop_fila1.py:817`, `bin/hook_stop_fila1.py:821`). Existência de sessões concorrentes na produção: [não verificado].

7. **Retenção limitada de clears pode mudar o desfecho.** Escritor conserva só os últimos 20 horários (`bin/marco_de_clear.py:57`). Se um clear em janela é seguido por 20 clears tardios antes do próximo Stop, o primeiro desaparece; o hook passa a escolher um clear tardio e registra SEGUIU (`bin/hook_stop_fila1.py:810`, `bin/hook_stop_fila1.py:813`). Quantidade real de clears: [não verificado].

**Donos dos estados existentes:**

| Arquivo | Escreve | Lê |
|---|---|---|
| `logs/sessao/ultimo_clear.json` | `marco_de_clear.py`, leitura+gravação sob seu flock, substituição atômica (`bin/marco_de_clear.py:38`, `bin/marco_de_clear.py:61`) | `orcamento_contexto.ler_marco`, chamado pelo hook e painel (`bin/orcamento_contexto.py:145`, `bin/hook_stop_fila1.py:794`, `bin/orcamento_contexto.py:232`) |
| `logs/sessao/orcamento_estado.json` | hook, `_orc_salva` por substituição atômica (`bin/hook_stop_fila1.py:708`, `bin/hook_stop_fila1.py:717`) | hook e `orcamento_contexto.ler`, usado pelo painel (`bin/hook_stop_fila1.py:779`, `bin/orcamento_contexto.py:228`, `bin/gerar_estado.py:276`) |
| `logs/sessao/orcamento.log` | hook, `_orc_trilha` (`bin/hook_stop_fila1.py:687`) | `orcamento_contexto.ler`, usado pelo painel (`bin/orcamento_contexto.py:235`, `bin/gerar_estado.py:276`) |
| `app/docs/HANDOFF-SESSAO.md` | `handoff_sessao.sh`: corte direto ou `mv` do temporário (`bin/handoff_sessao.sh:296`, `bin/handoff_sessao.sh:300`) | contrato de configuração esperado pelo teste é `cat` em SessionStart clear/compact (`bin/tests/test_handoff_sessao.sh:69`, `bin/tests/test_handoff_sessao.sh:72`); configuração efetivamente instalada: [não verificado] |

`cota.json`, `disjuntor_estado.json`, `disjuntor.log`, `diff_apply/<obra>`, `statusline_cota.py`, `disjuntor.py` e `.claude/settings.json` não foram encontrados nesta cópia por busca de arquivos/referências; donos operantes e acionamento real: [não verificado]. O código atual do painel só conhece a trilha/estado do ORÇAMENTO nessa seção (`bin/gerar_estado.py:271`).

## 30/10 22:57 · sessao abbc739c1d90 · FIO-ESTADO · verificador · origem=Code

**O desenho deixa três lacunas centrais: o esperado novo não está demonstrado, o leitor atual ignora `alcance` e não existe `cor` na sua saída.** (`app/chamados/management/commands/vigiar_fio_morto.py:91`; `bin/fio_estado.py:69–92`.) O valor **388** aparece como requisito de teste no contrato; uma medição atual de produção permanece **[não verificado]**.

1. **Quem escreve e quem lê cada campo**

   | Campo do carimbo | Escritor | Leitor atual / leitura pretendida |
   |---|---|---|
   | `alcance.alcance_universo`, `alcance.alcance_cego` | No anexo, `censo_alcance` produz; `vigiar_fio_morto` grava `alcance=alc`. (`anexo_f5.txt:200–225`; implementação integral **[não verificado]**.) | O leitor atual ignora ambos; usa `censo.cego`, que pertence ao universo antigo. (`bin/fio_estado.py:69–92`.) |
   | `alcance.fio_morto_alcance`, `alcance.alcance_a`…`alcance_e` | Mesmo produtor e escritor do anexo. (`anexo_f5.txt:200–225`; classificação completa **[não verificado]**.) | Hoje não são lidos; o contrato pretende usá-los como manchete e partição **[não verificado: leitor proposto não fornecido]**. (`bin/fio_estado.py:86–92`, `:101–104`.) |
   | `alcance.alcance_porta_fila`, `alcance_porta_pergunta`, `alcance_de_hoje_pra_frente` e os quatro `alcance_<balde>` | Mesmo fluxo do anexo. (`anexo_f5.txt:200–225`; incrementos **[não verificado]**.) | Hoje ignorados; seriam as parcelas da conferência do universo **[não verificado: implementação futura]**. (`bin/fio_estado.py:69–92`.) |
   | `alcance.alcance_competencias`, `alcance_e_detalhe`, `alcance_dias` | O anexo os inclui no dicionário carimbado. (`anexo_f5.txt:200–225`; preenchimento integral **[não verificado]**.) | Hoje ignorados; o contrato não especifica quais desses campos serão publicados **[não verificado]**. (`bin/fio_estado.py:82–92`.) |
   | `censo.fio_morto_alcance` | A F5 copia `alc['fio_morto_alcance']` para o censo carimbado. (`anexo_f5.txt:200–225`.) | Hoje só participaria de `fora_do_esperado` **se também existir a chave em `esperado`**; não é exportado como contador próprio. (`bin/fio_estado.py:70–73`, `:82–92`.) |
   | `esperado.fio_morto_alcance` | **Não há escritor demonstrado para essa chave:** a declaração atual não a contém, e o trecho F5 apenas copia `ESPERADO_DO_CARIMBO`. (`app/chamados/management/commands/vigiar_fio_morto.py:86–91`; `anexo_f5.txt:200–225`.) | A comparação genérica atual a leria automaticamente, caso existisse; a manchete continuaria antiga. (`bin/fio_estado.py:70–73`, `:101–108`.) |

   Portanto, o texto “esperado 0” de `linhas_alcance` não demonstra `esperado.fio_morto_alcance` no JSON **[não verificado: declaração F5 não fornecida]**. O contrato exige esse dado no carimbo; usar `0` como substituto no leitor contrariaria o desenho atual de expectativa transportada pelo escritor. (`bin/fio_estado.py:14–22`, `:70–73`.)

2. **Estados do carimbo e sua apresentação**

   A coluna “contrato” abaixo descreve a especificação fornecida; sua implementação futura está **[não verificado]**. Hoje o JSON não contém `cor`: “amarelo/verde” aparecem como convenções nos comentários e no selo. (`bin/fio_estado.py:67–92`; `bin/tests/test_fio_estado.sh:14`, `:56–60`.)

   | Estado | Código atual | Contrato |
   |---|---|---|
   | Ausente, sem permissão ou JSON sintaticamente inválido | `medido=False`, texto **NUNCA MEDIDO**. (`bin/fio_estado.py:63–68`, `:97–100`.) | AMARELO. |
   | Antigo: sem `alcance` | Publica o contador antigo normalmente; não reconhece “F5 não pousou”. Pode satisfazer o caso chamado verde pelo selo. (`bin/fio_estado.py:101–104`; `bin/tests/test_fio_estado.sh:40–60`.) | Aviso “carimbo sem alcance”; nunca VERDE. |
   | Novo, íntegro, com universo positivo | Continua publicando `censo.fio_morto`; ignora a partição. Só acusa o contador novo se ele estiver em `esperado`. (`bin/fio_estado.py:70–73`, `:101–108`.) | Manchete de alcance contra esperado lido; VERDE depende das validações. |
   | Novo, cego ou universo zero/ausente | Ignora a cegueira de alcance; só escreve **CEGO** quando `censo.cego` é verdadeiro. (`bin/fio_estado.py:91`, `:109–111`.) | VERMELHO. |
   | Partição quebrada ou espelho `censo` divergente | Não há essas conferências. (`bin/fio_estado.py:69–92`.) | VERMELHO. |
   | Rodada com `--empresa` | Não altera o carimbo: o painel continua lendo a última fotografia de frota, ou fica nunca medido se ela não existir. (`app/chamados/management/commands/vigiar_fio_morto.py:194–212`; `bin/fio_estado.py:62–68`.) | Não constitui um formato novo de carimbo. |
   | Temporalmente velho | Acima de 26 horas, marca `velho=True` e avisa no texto. Data ausente/inválida deixa idade desconhecida e `velho=False`. (`bin/fio_estado.py:56`, `:74–84`, `:112–117`.) | A precedência entre idade, erro estrutural e cor não foi especificada **[não verificado]**. |

3. **“Nenhum caminho levanta exceção para o ESTADO” não vale hoje**

   A proteção cobre abertura e decodificação, mas não valida os tipos depois disso: JSON `[1]` falha no `.get`; `censo=[1]` falha na comparação; `medido_em=7` causa `TypeError`, enquanto o tratamento captura apenas `ValueError`. (`bin/fio_estado.py:63–80`.)

   Essas exceções chegam à composição do ESTADO porque importação, leitura e formatação do fio são chamadas sem proteção nessa seção. (`bin/gerar_estado.py:260–261`.) O contrato precisa cobrir também estrutura, tipos e composição da linha; tratar apenas JSON ilegível não basta. (`bin/fio_estado.py:69–120`.)

4. **Há consumidores que mudam de comportamento**

   `gerar_estado` consome o dicionário Python por `ler`/`linha`, não o JSON serializado nem uma chave `cor`; a nova manchete chegará ao painel pela alteração de `linha`. Apenas acrescentar `cor` não a publica. (`bin/gerar_estado.py:260–261`, `:313–315`, `:350`.)

   O consumidor explícito do JSON encontrado é o selo, que lê `medido`, `velho` e as chaves de `fora_do_esperado`. Acrescentar campos preserva essa interface; alterar esses campos muda o teste. Consumidores externos à cópia: **[não verificado]**. (`bin/tests/test_fio_estado.sh:51`.)

   Reaproveitar a chave pública `fio_morto` para guardar alcance mudaria sua semântica: hoje ela significa exatamente `censo.fio_morto`. Preservá-la e acrescentar `fio_morto_alcance` mantém a distinção entre os universos. (`bin/fio_estado.py:86`, `:101–104`.)

5. **Há uma ambiguidade entre manchete nova e regressão antiga**

   Mesmo com alcance correto, o leitor atual continua comparando **todas** as expectativas antigas. Assim, alcance conforme e contador antigo divergente ainda produzem `fora_do_esperado`. (`bin/fio_estado.py:70–73`; `app/chamados/management/commands/vigiar_fio_morto.py:86–91`.)

   O contrato manda rotular o antigo como regressão, mas não determina se essa regressão participa da nova `cor` **[não verificado]**. Essa decisão precisa ficar explícita; trocar apenas a manchete preserva um veredito com escopo antigo. (`bin/fio_estado.py:101–108`.)

6. **Conferir a soma pode ser propriedade fixa, sem virar segundo censo**

   Conferir, entre campos já carimbados, `a+b+c+d+e == fio_morto_alcance`, as partes contra o universo e a igualdade dos dois exemplares do contador é validação de integridade. A L-110 nomeia expressamente “partes somam o total” como propriedade fixa. (`CLAUDE.md:45–51`.)

   A fronteira da LEI-AKITA 2 é reconstruir universo ou classificação: percorrer `alcance_dias`, reaplicar datas, decidir causas ou corrigir os contadores seria derivação própria. (`CLAUDE.md:20–22`; `bin/fio_estado.py:29–34`.) Uma soma que fecha também não prova que os dias foram classificados corretamente; o trecho integral dessa classificação não foi fornecido **[não verificado]**.

7. **Onde pode aparecer “VERDE sem medida”** [nao verificado]

   Hoje, estes casos satisfazem o critério `medido=True`, `velho=False`, `fora_do_esperado={}` que o selo chama verde — embora o leitor não emita uma cor propriamente dita. (`bin/tests/test_fio_estado.sh:51–60`.)

   - `{}` ou `null`: tornam-se registro vazio, com `medido=True`, expectativa vazia e idade desconhecida. (`bin/fio_estado.py:65`, `:69–84`.)
   - `esperado` ausente/vazio: nenhuma comparação ocorre, mesmo havendo contador não zero. (`bin/fio_estado.py:70–73`.)
   - Carimbo antigo com contadores conformes: nenhuma medida de alcance é exigida. (`bin/fio_estado.py:69–92`.)
   - Alcance cego ou universo vazio com censo antigo conforme: a cegueira nova é ignorada. (`bin/fio_estado.py:86–92`, `:109–111`.)
   - `censo.cego=True`: aparece o aviso textual, mas não entra automaticamente em `fora_do_esperado`; o escritor atual não inclui `cego` em `esperado`. (`app/chamados/management/commands/vigiar_fio_morto.py:80–91`; `bin/fio_estado.py:72–73`, `:109–111`.)
   - Medição sem data válida: `velho=False` não significa que a fotografia seja recente. (`bin/fio_estado.py:74–84`.)

   Há ainda um desencontro possível de destino: o escritor aceita `VIGIA_FIO_CARIMBO`, enquanto o leitor usa caminho fixo; o selo compara somente as constantes. Se houver redirecionamento, o painel pode continuar lendo outra fotografia. Configuração de produção: **[não verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:97–107`; `bin/fio_estado.py:55`, `:62`; `bin/tests/test_fio_estado.sh:93–102`.)

8. **O selo atual e a promessa de pre-push não fecham a garantia nova** [nao verificado]

   O selo fabrica somente formato antigo; não consulta `cor`; seu caso cego exige apenas a palavra **CEGO**. Ele pode continuar passando sem provar alcance, vermelho por cegueira ou aviso de formato antigo. (`bin/tests/test_fio_estado.sh:40–51`, `:104–107`.) As provas por mutação dos casos novos estão **[não verificado]**; a regra exige que o selo morda a costura. (`CLAUDE.md:350–355`.)

   Colocar o novo selo em `bin/tests/test_*.sh` o inclui na régua, mas **não garante execução direta no pre-push**: o próprio script declara que esse loop não roda ali. (`bin/regua.sh:138–145`; `bin/pre-push.sh:12–16`.)

9. **O primeiro carimbo novo não basta para apagar o ramo antigo**

   Uma rodada de recorte não substitui o arquivo; o escritor atual ainda produz o formato antigo. Portanto, observar um primeiro carimbo F5 não demonstra que nenhum antigo possa voltar a existir. (`app/chamados/management/commands/vigiar_fio_morto.py:194–212`.) Essa impossibilidade operacional permanece **[não verificado]**; a L-111 exige retirar o sítio quando ele perde o último uso, junto com testes e pendência. (`CLAUDE.md:341–344`.)

## 30/10 22:55 · sessao bfc8fe859b37 · - · - · origem=Ronald (tmux)

- **Soma como propriedade fixa:** `CLAUDE.md:45-51` define expressamente “partes somam o total” como propriedade fixa da L-110. Conferir igualdades entre contadores já carimbados valida integridade; reconstruir universo, classificar dias ou corrigir números seria derivação própria proibida por `CLAUDE.md:20-22`. Há precedente no selo do produtor: `app/chamados/tests/test_o261_vigia_fio.py:647-668`.

- **Selo atual pode continuar verde sem validar alcance:** gera exclusivamente carimbos antigos (`bin/tests/test_fio_estado.sh:40-48`), observa somente `medido`, `velho` e `fora_do_esperado` (`bin/tests/test_fio_estado.sh:51`) e considera casos antigos “VERDE” (`bin/tests/test_fio_estado.sh:56-91`). Acrescentar `cor=AMARELO` sem alterar essas três saídas não faria esses casos falharem (`bin/tests/test_fio_estado.sh:51`).

- **Cegueira atual não prova vermelho:** o caso apenas procura `CEGO` no texto, sem conferir campo `cor` ou veredito (`bin/tests/test_fio_estado.sh:104-107`). O selo exige caso que morda a costura; texto presente sozinho não demonstra a cor exigida (`CLAUDE.md:350-355`).

- **Mutações prometidas:** o arquivo atual encerra declarando sete casos, mas não executa mutações de leitor (`bin/tests/test_fio_estado.sh:109`). A comprovação por mutação dos novos casos permanece **[não verificado]**.

- **L-111:** apagar o ramo antigo exige demonstrar que perdeu seu último uso; primeiro carimbo novo isoladamente não demonstra impossibilidade de carimbo antigo retornar **[não verificado]**. A lei manda retirar sítio sem chamador, testes e pendência no mesmo commit (`CLAUDE.md:341-344`). [nao verificado]

- **Entrada dos selos:** a régua enumera `bin/tests/test_*.sh` (`bin/regua.sh:138`); o pre-push documenta que esse diretório não roda diretamente ali (`bin/pre-push.sh:15`).

## 30/10 22:55 · sessao 70a74bc5d59a · - · - · origem=Ronald (tmux)

- O consumidor de produção encontrado é `gerar_estado`: importa `ler`/`linha`, passa o dicionário inteiro a `linha` e publica o texto resultante. Não consulta `cor`, `alcance` ou contadores diretamente; a mudança de manchete chega ao ESTADO pela alteração de `linha`. (`bin/gerar_estado.py:260-261`, `bin/gerar_estado.py:313-315`, `bin/gerar_estado.py:350`)

- O consumidor do JSON encontrado é o selo atual: consulta somente `medido`, `velho` e as chaves de `fora_do_esperado`; também importa `CARIMBO`. Acrescentar campos preservando esses campos mantém essa interface; renomeá-los/removê-los rompe o selo. (`bin/tests/test_fio_estado.sh:51`, `bin/tests/test_fio_estado.sh:97`)

- O JSON público atual contém `fio_morto=censo.fio_morto`; a linha também lê esse campo. Reutilizar `fio_morto` para conter alcance mudaria sua semântica; acrescentar `fio_morto_alcance` e preservar o antigo evita essa troca. Consumidores externos à cópia: [não verificado]. (`bin/fio_estado.py:86`, `bin/fio_estado.py:101-104`, `bin/fio_estado.py:129`)

- F5 só escrever `esperado.fio_morto_alcance` e `censo.fio_morto_alcance` já faria o leitor atual acrescentar esse desvio a `fora_do_esperado`, porque ele itera todas as expectativas. Isso não muda a manchete, que continua no contador antigo. (`bin/fio_estado.py:69-73`, `bin/fio_estado.py:101-108`)

- O selo antigo só cria carimbos sem `alcance`. Seus casos chamados VERDE verificam `True|False|-`; não verificam `cor`. Portanto acrescentar `cor=AMARELO` para formato antigo pode preservar esses testes, mas o selo continuará chamando esses casos de VERDE sem testar o novo significado. (`bin/tests/test_fio_estado.sh:40-48`, `bin/tests/test_fio_estado.sh:51`, `bin/tests/test_fio_estado.sh:56-60`, `bin/tests/test_fio_estado.sh:82-86`)

- A régua executa automaticamente `bin/tests/test_*.sh`; o pre-push não executa esse loop e declara isso expressamente. Colocar o novo selo em `bin/tests/` garante sua descoberta pela régua, mas não sua execução direta a cada pre-push. O hook instalado real é [não verificado]. (`bin/regua.sh:138-145`, `bin/pre-push.sh:12-19`, `bin/pre-push.sh:61-66`)

- O instalador vigente usa `bin/pre-push.sh` como fonte e gera wrapper que executa esse caminho; a cópia histórica de `app/core/hooks/pre-push.sh` não é o alvo declarado por esse instalador. (`bin/hooks.sh:9-11`, `bin/hooks.sh:51-56`)

## 30/10 22:54 · sessao 9ae75d7ba88f · O261-V-FASE · verificador · origem=Code

**Sem achado procedente no delta examinado:** C9 e C10 passam pelo mesmo mapa dono→porta; BATIDA abre regularização, ESTRUTURA fica sem URL e somente CADASTRO recebe a porta específica da assinatura (`app/escala/services/cadastro_realidade.py:481`, `app/escala/services/cadastro_realidade.py:490`, `app/escala/services/cadastro_realidade.py:518`).

1. **Quem escreve e quem lê**

- **C9:** `lavrar_teto_do_dia` chama medição e persistência; a medição pergunta a `InsumosDoDono.dono()` e grava `dono` no detalhe do snapshot (`app/ponto/management/commands/lavrar_teto_do_dia.py:28`, `app/ponto/management/commands/lavrar_teto_do_dia.py:34`, `app/ponto/services/teto_na_lista.py:86`, `app/ponto/services/teto_na_lista.py:113`). `_teto_do_dia()` lê a lavra e copia esse valor para `dono_do_dia` (`app/escala/services/cadastro_realidade.py:320`, `app/escala/services/cadastro_realidade.py:325`).
- **C10:** o censo monta `InsumosDoDono` com os dias de fase daquela corrida, pergunta o dono de cada dia e grava `dono_do_dia`; `fase_12x36.lavrar()` persiste o detalhe (`app/ponto/management/commands/censo_fase_12x36.py:260`, `app/ponto/management/commands/censo_fase_12x36.py:262`, `app/ponto/management/commands/censo_fase_12x36.py:272`, `app/escala/services/fase_12x36.py:90`). `_fase_deslocada()` lê a lavra até `hoje` e preserva o dono (`app/escala/services/cadastro_realidade.py:343`, `app/escala/services/cadastro_realidade.py:346`).
- **Juiz comum:** `InsumosDoDono.dono()` chama `dono_da_divergencia()`; a lista recebe o resultado lavrado de cada assinatura e calcula `acao`/`url` (`app/ponto/management/commands/e6_oraculo.py:166`, `app/ponto/management/commands/e6_oraculo.py:173`, `app/escala/services/cadastro_realidade.py:657`, `app/escala/services/cadastro_realidade.py:661`, `app/escala/services/cadastro_realidade.py:664`).
- **Tela/PDF:** a view fornece a mesma lista aos dois destinos. O template lê dono, ação e URL; o PDF imprime dono e ação, sem hyperlink da porta (`app/escala/views.py:490`, `app/escala/views.py:492`, `app/escala/views.py:495`, `app/templates/escala/cadastro_x_realidade.html:28`, `app/escala/services/cadastro_realidade.py:794`, `app/escala/services/cadastro_realidade.py:797`).
- **Vigia:** lê `dono_do_dia` por `dias_na_lista()`, sem consultar o texto do botão. C9/C10 conflitantes resultam em `None`; somente BATIDA/CADASTRO entram em `fio_do_admin`, mantendo ESTRUTURA no alarme (`app/escala/services/cadastro_realidade.py:728`, `app/escala/services/cadastro_realidade.py:731`, `app/escala/services/cadastro_realidade.py:753`, `app/chamados/services/vigia_fio.py:259`, `app/chamados/services/vigia_fio.py:266`, `app/chamados/services/vigia_fio.py:314`).

2. **Mapa, rótulos e leitores antigos**

Não restaram dois mapas nesses caminhos. As diferenças entre C9 e C10 são os destinos de **CADASTRO**: template/vínculo na C9; fio ou “só listado”, conforme `aplicavel`, na C10. BATIDA passa à regularização inclusive antes do piso; ESTRUTURA e dono ausente não recebem porta de cadastro (`app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:484`, `app/escala/services/cadastro_realidade.py:519`).

O novo rótulo “o do juiz do dia, e6_oraculo…” alimenta `por_assinatura` e aparece no **title do selo superior**. O PDF imprime o dono efetivo do dia — BATIDA/CADASTRO/ESTRUTURA — e não essa frase agregada (`app/escala/services/cadastro_realidade.py:118`, `app/escala/services/cadastro_realidade.py:699`, `app/templates/escala/cadastro_x_realidade.html:19`, `app/escala/services/cadastro_realidade.py:796`).

Não localizei leitor executável dependente dos textos antigos. Os testes C9 encontrados prendem dono e URLs; “Regularizar a saida do dia” permanece no relato histórico (`app/ponto/tests/test_col129_teto_vira_lista.py:215`, `app/ponto/tests/test_col129_teto_vira_lista.py:226`, `app/docs/RELATO.md:151`). A prosa alterada explicita a morte de `fora_de_fase` e a leitura da lavra (`app/chamados/services/proposta_no_fio.py:22`, `app/core/tests/test_selo_performance.py:141`).

3. **Os testes mordem?** [nao verificado]

**Sim, para os dois defeitos visados:** um cria batida extra real, percorre censo→lavra→lista e exige dono BATIDA com URL de regularização; o outro exige identificação do juiz no rótulo agregado (`app/escala/tests/test_o261_v_fase_na_lista.py:193`, `app/escala/tests/test_o261_v_fase_na_lista.py:206`).

A cobertura nova é parcial: não prende ESTRUTURA, dono ausente, BATIDA antes do piso nem renderização do title/PDF; o teste do rótulo aceita uma substring (`app/escala/tests/test_o261_v_fase_na_lista.py:193`, `app/escala/tests/test_o261_v_fase_na_lista.py:209`).

Exercitei em memória 20 combinações de C9/C10, piso e dono, coerentes com o mapa; a comparação isolada reproduziu base→fio e cópia→regularização para C10/BATIDA (`app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:511`). **Execução dos testes Django e renderização real: [não verificado]**; faltam as dependências neste ambiente.

## 30/10 22:53 · sessao d7a63fdce16d · - · - · origem=Ronald (tmux)

Nenhum defeito concreto encontrado neste recorte.

- Os dois testes novos exercitam comportamento: Batida extra real → censo/lavra → lista → dono BATIDA e URL de regularização; outro verifica a identificação do juiz no rótulo agregado. `app/escala/tests/test_o261_v_fase_na_lista.py:193`, `app/escala/tests/test_o261_v_fase_na_lista.py:206`.
- Confirmei por execução de AST em memória, usando `python3 -B`, 32 combinações entre base/alvo, C9/C10, quatro donos e piso aplicável/inaplicável: base encaminhava C10 aplicável BATIDA/ESTRUTURA/sem dono ao fio; alvo encaminha BATIDA à regularização, ESTRUTURA/sem dono a nenhuma URL, preservando CADASTRO conforme piso. Código exercitado: `app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:511`. Não executei a integração Django: `[não verificado]`.
- Lacunas dos dois testes adicionados: não verificam ESTRUTURA, ausência de dono, BATIDA anterior ao piso, renderização do `title` nem extração do PDF; a asserção do rótulo é somente substring. `app/escala/tests/test_o261_v_fase_na_lista.py:193`, `app/escala/tests/test_o261_v_fase_na_lista.py:209`.
- O texto novo aparece no `title` do selo agregado, lido de `por_assinatura`; a linha da tela usa dono efetivo. `app/templates/escala/cadastro_x_realidade.html:19`, `app/templates/escala/cadastro_x_realidade.html:28`.
- O PDF não imprime o rótulo novo agregado: imprime `a['dono']` por dia e `a['acao']`, após a sobreposição do dono lavrado. `app/escala/services/cadastro_realidade.py:660`, `app/escala/services/cadastro_realidade.py:666`, `app/escala/services/cadastro_realidade.py:794`.
- Não achei leitor/teste do texto antigo exato; única ocorrência restante de “Regularizar a saida do dia” está no relato histórico. `app/docs/RELATO.md:151`. Os testes C9 existentes dependem das URLs e donos, preservados: `app/ponto/tests/test_col129_teto_vira_lista.py:215`, `app/ponto/tests/test_col129_teto_vira_lista.py:226`.
- A prosa alterada declara a morte de `fora_de_fase` e a leitura da lavra; o parágrafo anterior permanece como explicação histórica de custo. `app/chamados/services/proposta_no_fio.py:15`, `app/chamados/services/proposta_no_fio.py:22`, `app/core/tests/test_selo_performance.py:141`.

_810 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
