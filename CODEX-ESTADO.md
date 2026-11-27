# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 13:20** · publicado em **27/11/2026 13:23** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 13:20 · sessao 39cf3f7e0f5f · PAR-PELO-TURNO · construtor · origem=Code

**PAR-PELO-TURNO c1v10 entregue na cópia, sem commit.** Os 7 REDs passaram após a cura. As 8 reversões em memória reproduziram as falhas. Também passaram 22 testes dos selos e 207 no recorte `vizinhos`. O filtro `^\+.*except` contou **0** linhas, incluindo os arquivos novos. [Log dos REDs](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v10_red.md:8) · [Auditoria](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v10_red.md:148) [nao verificado]

| Item | RED na base → cura | Reversão em memória |
|---|---|---|
| **2 · Segunda virada** | Folha com **0 h e 1 aberto → 26 h e 0 abertos**. No PDF, restaurei `[D,D]`. A apuração passou a receber os dois PKs e a publicar o par no dia D. [RED da folha](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_pelo_turno_c1v10.py:30) · [RED original do PDF](/home/ronald/wt-codex-par-pelo-turno-111717/app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:72) | `margem` perde a saída nas duas portas. `pad_largo` inclui o par vizinho e produz 28 h. `pdf_janela_visual` troca o fim pedido pelo fim visual. [Log](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v10_red.md:78) | [nao verificado]
| **3 · Retratação entre leituras** | `KeyError: 2` → uma leitura do conjunto de marcas, fechamento com **2 h e 0 abertos**. [Fechamento](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/fechamento.py:635) · [RED](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_pelo_turno_c1v10.py:48) | `duas_leituras` restaura o `KeyError`. [Log](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v10_red.md:100) |
| **4 · Selo da tela** | `SyntaxError` → importação pelo caminho normal e execução de `test_MORDE_o_selo_distingue_JANELA_de_DIA`. Os três trechos voltaram a ASCII. [Trecho](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:147) · [Selo](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:233) | `ascii` restaura o erro na importação. [Log](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v10_red.md:106) |
| **5a · Censo e desenho do PDF** | O censo passou a resolver `_apurar_por_fatia` e acusou a chamada real. Na base, o PDF executava uma **apuração própria** com os insumos que montava, embora reutilizasse a regra de fatiamento. Agora o coletor lê o resultado e as linhas da mesma resposta de `espelho_do_colab`, na janela efetiva do PDF. [Censo](/home/ronald/wt-codex-par-pelo-turno-111717/app/relatorios/tests/test_pdf_nao_calcula.py:34) · [Coletor](/home/ronald/wt-codex-par-pelo-turno-111717/app/relatorios/pdf_espelho.py:269) | `apelido` torna a chamada invisível. `pdf_calcula` restaura o coletor da base e o censo acusa a apuração. [Log](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v10_red.md:110) | [nao verificado]
| **5b · Justificativas** | Para `14:57 E / 19:01 S / 20:04 E`, o GET continua indicando um aberto e passa a exibir **14:57**, a entrada do turno julgado. [API](/home/ronald/wt-codex-par-pelo-turno-111717/app/api/views.py:2024) · [RED](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_pelo_turno_c1v10.py:81) | `paridade` volta a exibir **20:04**. [Log](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v10_red.md:138) |

`ponto/turnos.py::carga_de_batidas_para_motor` usa a leitura de contexto do juiz. A carga do motor mantém `ini−1..fim+1` e acrescenta, por PK, as marcas dos **pares fechados cujo dia julgado pertence à janela**. A mesma seleção produz a lista e o mapa usados por `recalcular_fechamento_mes` e `autoridade_do_periodo`. A passada órfã mantém sua leitura própria. [Carga e mapa](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/turnos.py:3027) · [Autoridade do espelho](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/espelho.py:1003) · [Passada órfã](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/fechamento.py:810)

**Reprodução a partir de `app/`.** A base e os mutantes esperam `rc=1`. A cura e os recortes esperam `rc=0`. [Executor](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v10_verificacao.py:174)

```bash
for modo in folha pdf retratacao tela censo desenho api; do
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.par_pelo_turno_c1v10_verificacao "$modo" --base
  printf '%s rc=%s\n' "$modo" "$?"
done

for modo in todos selos margem pad_largo duas_leituras ascii apelido pdf_calcula pdf_janela_visual paridade; do
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.par_pelo_turno_c1v10_verificacao "$modo"
  printf '%s rc=%s\n' "$modo" "$?"
done

PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.par_pelo_turno_c1v9_verificacao vizinhos
```

Os ensaios locais usaram dublês de infraestrutura. Suíte Django completa, integração com ORM, renderização do PDF, Ruff e dados de produção **[nao verificado]**. A herança de 6 h segue no escopo da RAMO2-B. A certificação cabe ao Code e ao opus final. [Executor local](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v10_verificacao.py:63) · [Herança no motor](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/motor_calculo_v2.py:2516) · [L-126](/home/ronald/wt-codex-par-pelo-turno-111717/app/docs/LEIS.md:147) [nao verificado]

## 27/11 13:07 · sessao 8d4709a7387f · FERIADO-UM-JUIZ · construtor · origem=Code

**FERIADO-UM-JUIZ c2 entregue parcialmente. PAREI no K1.** A sonda sem banco ainda encontrou 470 min de HE100 e zero de intra. Para chegar a 440 + 30, o corte exige compartilhar a apuração do suprimido dentro do motor, arquivo vedado nesta volta. O RED ficou com a tag `feriado_k1_pendente`. [Corte K1](/home/ronald/wt-codex-feriado-um-juiz-121621/app/docs/PROMPTS.md:3016) · [RED pela porta](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/test_feriado_um_juiz_porta.py:269)

| Item | Resultado |
|---|---|
| **1. TXT e homologação** | Na c1, a montagem perdeu os 400 centésimos e `cadastro_zero` reteve a FT isolada. Os dois casos passaram sem banco na c2. Adotei 11/2026 como início da leitura direta do mensal e preservei a emissão histórica pelo 59-A. [Montagem](/home/ronald/wt-codex-feriado-um-juiz-121621/app/folha/export.py:704) · [Classificação](/home/ronald/wt-codex-feriado-um-juiz-121621/app/folha/export.py:783) · [Homologação](/home/ronald/wt-codex-feriado-um-juiz-121621/app/folha/export.py:1015) |
| **3. Partição, DIFF e intermitente** | Mensal, oráculo e DIFF usam `particionar_ft`. Os REDs locais de intermitência por base e da regra avulsa de dia de trabalho passaram. DIFF e lavras integradas [nao verificado]. [Partição](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/regras.py:384) · [Oráculo](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/alimentacao.py:391) · [Contexto por fatia](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/services/espelho.py:1008) · [DIFF](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/management/commands/diff_calculador.py:61) |
| **4 e 5. Split e dia do par** | O período bruto ficou preservado e a partição limitou o split de 28 h a 26 h. A dobra usa o dia do juiz do turno. A lavra continua agrupando pelo mapa da jornada. Virada com troca de praça pela porta [nao verificado]. [Partição](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/regras.py:384) · [Lavratura](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/services/dia_pago.py:243) · [RED da praça](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/test_feriado_um_juiz_porta.py:304) |
| **6. Arredondamento** | As atribuições de produção deram **8,04 h → 8,03 h** para 241 + 241 min na sonda. Igualdade com as duas lavras em dois recálculos [nao verificado]. [Fechamento](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/services/fechamento.py:963) · [RED da relavra](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/test_feriado_um_juiz_porta.py:282) |

**Verificação local:** 25 testes passaram sem banco. Os 21 mutantes locais falharam por asserção após o controle verde. O diff tem **+0/−0 `except`**. Os cinco mutantes integrados, os REDs pela porta e o `makemigrations --check` seguem [nao verificado]. A inicialização parou por falta de `django.core`. O registro contém o antes/depois, os labels da pista e o comando de cada mutante. [Registro de testes](/home/ronald/wt-codex-feriado-um-juiz-121621/app/docs/FERIADO_UM_JUIZ_C2_TESTES.md:3) · [Comandos](/home/ronald/wt-codex-feriado-um-juiz-121621/app/docs/FERIADO_UM_JUIZ_C2_TESTES.md:22) [nao verificado]

**Fora desta volta:** para a sessão, `lei: exportada fora da guarda segue L-113`. Falha entre `delete` e `bulk_create`, concorrência TXT × recálculo, medição nominal dos 75 dias/42 colaboradores, apply, reversão e DIFF do TXT 11 [nao verificado]. [Guarda e L-113](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/services/fechamento.py:564) · [Gravação das lavras](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/services/dia_pago.py:358)

## 27/11 13:05 · sessao 68fb1a5776c7 · PAR-PELO-TURNO · verificador · origem=Code

# REPROVADO — PAR-PELO-TURNO c1v9

A volta cura o caso de 8 h do laudo, mas a herança de 6 h ainda tira ou acrescenta horas à competência com `ini−1..fim+1`. A janela curta também perde um par válido na segunda virada, e o selo da tela quebra na importação. `app/ponto/tests/test_par_c1v8_reds.py:238–250` · `app/ponto/motor_calculo_v2.py:2516–2545` · `app/ponto/turnos.py:321–334,1878–1882` · `app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:1,147–148,234`

Em somente leitura, rodei **11 REDs verdes e 206 testes verdes no recorte `vizinhos`**. Os **15 modos de reversão** falharam por asserção ao trocar código de produção em memória. Nove rodaram diretamente. Seis usaram o trecho anterior do diff anexo porque a cópia veio sem `.git`. A identidade dos hashes e da ref `main`, a suíte completa, os 29 casos reais e o DIFF de frota da 11 ficam **[nao verificado]**. `app/ponto/tests/par_pelo_turno_c1v9_verificacao.py:19–41,49–121,145–168` · `app/docs/LEIS.md:112`

## 1. Itens 2–7 do laudo

| Item | Cura na origem e mordida |
|---|---|
| **2 — O95 na folha** | **Sim, para o caso do laudo.** A folha passou a chamar `dia_das_batidas` e recebe o provedor da última batida apurável. `test_3` chama `recalcular_fechamento_mes` e exige saldo `0,00` com ou sem as marcas deslocadas. `folha` restaura `−193,60` nos dois subcasos. `o95_provedor` restaura `−193,60` com o fato de 25/10, fora do contexto carregado. `test_3b` também morde quando o último dia é excluído do movimento. Há uma falha nova entre as duas leituras, descrita abaixo. `app/ponto/services/fechamento.py:223–225,635,657–660` · `app/ponto/turnos.py:1944–1949,3063–3068` · `app/ponto/tests/test_par_c1v8_reds.py:67–100,160–198` |
| **3 — PK na troca de vínculo** | **Sim, nos casos exercitados.** O segmento seguinte julga primeiro e reserva os PKs dos turnos que lhe pertencem. `turnos=` deixa de substituir o julgamento. `test_1` cobre casca, lote e consulta individual. A entrada de 23/10 às 01:30 fica em **22/10** com o DNA noturno da véspera e em **23/10** quando há jornada diurna declarada. Na troca, a saída de 22/10 às 20:00 fica em **22/10**. `pk_dna`, `pk_contexto`, `pk_turnos` e `pk_segmentos` mordem. `app/ponto/turnos.py:287–301,2027–2057,3056–3068,3113–3116` · `app/ponto/tests/test_par_c1v8_reds.py:115–145` |
| **4 — Guarda da pausa e separador** | **Sim, nos dois comportamentos testados.** Os laços com e sem bordas usam `saida_encerra`. `test_2` exige um par com pausa de 60 min após uma saída degenerada de 30 s. `pausa` volta a partir o turno em dois. `test_2b` exige que o eco de 20:04 sirva de evidência da volta da pausa, preservando o turno aberto. `separador` volta a fechá-lo às 19:01. A prova do evento do vigia às 03:10 é **[nao verificado]**. `app/ponto/turnos.py:406–444,450–516` · `app/ponto/tests/test_par_c1v8_reds.py:147–158,308–317` · `app/ponto/tests/test_ponta_pequena_prazo_evento.py:384–405` |
| **5 — Cadastro antes do par** | **Sim.** `classificar_falta` consulta o par antes de consultar o cadastro para a classificação. O leitor usa `data_saida` publicada por `_mk`. `test_4` falha com a função antiga para vínculo diurno ou ausente na saída. `test_4b` detecta a consulta ao cadastro quando se reverte a ordem. `app/escala/servico_jornada.py:214–220,318–322` · `app/ponto/turnos.py:318–334` · `app/ponto/tests/test_par_c1v8_reds.py:200–236` |
| **6 — Testemunha, soma e contagem** | **Sim, nos três pontos locais.** `test_6a` detecta o argumento `fim+1` na chamada da testemunha. É uma prova de chamada. `test_6b` detecta 8 h que somem com mapa vazio ou ausente. `test_6c` detecta a volta da contagem civil, que dá `(8 h, 0 turnos, 0 abertos)` em vez de `(8 h, 2 turnos, 1 aberto)`. A classificação civil de contingência é compartilhada por `_dia_de`. `app/folha/porta_export.py:67–71,100–119` · `app/ponto/services/espelho.py:76–87,437–453` · `app/ponto/services/dia_pago.py:126–132` · `app/ponto/tests/test_par_c1v8_reds.py:252–306` |
| **7 — Suíte e catracas** | **Não.** C8b, C8c e a catraca passaram no recorte de 206 testes. O selo da tela declara ASCII e ganhou texto UTF‑8. Reproduzi `SyntaxError` ao importar pelos bytes do arquivo. O recorte `vizinhos` omite esse módulo. No PDF, a nova B1 vem de `.date()` sobre os limites da **carga**. Ao reverter só esse trecho, o censo dá B1 `75→74`, B2 `188→188` e `B2−B1` `113→114`. Isso explica a mudança de classe, sem evidência de um novo juiz do turno naquele trecho. Ruff e os testes Django do app **[nao verificado]**. `app/ponto/tests/par_pelo_turno_c1v9_verificacao.py:153–163` · `app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:1,147–148,233–240` · `app/relatorios/pdf_espelho.py:280–292` · `app/core/medida_turno_do_par.py:432–436` · `app/api/tests/test_espelho_app_fonte_e_geometria.py:279–326` |

Os 15 reversores executam o teste contra funções de produção trocadas. `ultimo_dia` injeta `< fim` e prova a inclusão do último dia. `janela` repõe o pad largo e prova a perda das 8 h do caso original. A reversão completa do commit por Git fica **[nao verificado]**. `app/ponto/tests/par_pelo_turno_c1v9_verificacao.py:49–121,149–168` · `app/ponto/tests/test_par_c1v8_reds.py:188–198,238–250`

## 2. Janela do motor e RAMO2-B

| Porta | Lado `730c115e5` do diff anexo | c1v9 |
|---|---|---|
| Fechamento | `ini−4..fim+2`, `fechamento.py:655` no diff | `ini−1..fim+1`, inclusive na passada órfã. `app/ponto/services/fechamento.py:635,811` |
| PDF | `ini−4..fim_efetivo+2`, `pdf_espelho.py:281–282` no diff | `ini−1..fim_efetivo+1`. O fim efetivo continua sob `max(data_ini, _ate_ontem(...))`. `app/relatorios/pdf_espelho.py:276–283` |
| Espelho | `ini−4..fim+2`, `espelho.py:1031–1032` no diff | `ini−1..fim+1` na autoridade. A tela chama com `vis_ini..hoje`. `app/ponto/services/espelho.py:1024–1030,1226–1230` |

**A c1v9 estreita a carga do motor nas três portas em relação ao lado antigo do diff.** O pad padrão de dois dias produz `ini−4..fim+2`. Para o mapa por PK, a casca e o lote somam outros dois dias à direita para julgar o contexto. A publicação normal tem seu próprio recorte. `app/ponto/turnos.py:55–57,1878–1882,3019–3029,3060–3068,3097–3117,3134–3139`

A herança continua porque o motor atualiza o fim e o dia do período anterior **antes** de descartar períodos fora da competência. Um período **fechado** com vão de `0..6 h` basta. Reproduzi pela folha, com o `Motor12x36` real e dublês de infraestrutura, os seguintes casos. `app/ponto/motor_calculo_v2.py:2512–2545` · `app/ponto/tests/test_par_c1v8_reds.py:67–108`

- **Esquerda, 00:00–06:00 em `ini−1`.** Sozinho, esse par termina pelo menos 18 h antes do primeiro instante de `ini`. Com `ini 03:00–11:00`, a folha deu **8 h**. Com um par intermediário em `ini−1 11:00–21:00`, os vãos de 5 h e 6 h encadeiam a herança e a folha deu **0 h**. O caso mais curto, `ini−1 18:00–22:00` seguido de `ini 03:00–11:00`, também deu **0 h**. `app/ponto/motor_calculo_v2.py:2516–2543`
- **Direita, em `fim+1`.** `fim 22:00 → fim+1 06:00` deu **8 h**. Acrescentando um par distinto `fim+1 11:00–19:00`, a geometria manteve dois turnos em dias distintos e a folha computou **16 h em `fim`**. As marcas de `fim+1` entraram na carga e o vão de 5 h fez o segundo período herdar `fim`. `app/ponto/turnos.py:305–334,1878–1882` · `app/ponto/motor_calculo_v2.py:2516–2545,2571–2573`

O registro da cópia ainda lista RAMO2-B pendente. O pouso atual da RAMO2-B é **[nao verificado]**. O bloqueio técnico continua demonstrável no motor desta cópia, inclusive com a janela reduzida. `app/docs/RELATO.md:1` · `app/docs/BACKLOG.md:2159` · `app/ponto/motor_calculo_v2.py:2516–2545`

## 3. Quem escreve e quem lê

| Nome | Quem produz | Quem consome |
|---|---|---|
| `dia_das_batidas` | `_dia_das_batidas_puro` monta `{PK: data_turno}` para casca e lote. PK sem atribuição usa a data local da marca. `app/ponto/turnos.py:3063–3068,3097–3117,3213–3217` | Consulta individual, fechamento, composição por fatia, espelho, app e justificativas. PDF recebe as linhas do espelho. `app/ponto/turnos.py:3083–3088` · `app/ponto/services/fechamento.py:223–225,271–274,657–659` · `app/ponto/services/espelho.py:462–472` · `app/api/views.py:1443–1466,2041–2054` · `app/relatorios/pdf_espelho.py:590–597` |
| `_data_do_turno` e `Turno.data_saida` | `_data_do_turno` decide `data_turno`. `_mk` grava a data civil da saída em `data_saida` para o par fechado e deriva `cross_meianoite` da diferença. `app/ponto/turnos.py:235,272–334` | `_mk` chama `_data_do_turno`. O mapa por PK, o motor e o calendário leem `data_turno`. O guarda de falta lê `data_saida` como limite da cobertura. `app/ponto/turnos.py:316,3113–3116` · `app/ponto/motor_calculo_v2.py:911–922` · `app/colaboradores/services/calendario.py:249–253` · `app/escala/servico_jornada.py:214–220` |
| `dia_da_jornada` | `MotorBase` preenche o mapa por `id(periodo)` e o publica pelo teto de pontualidade. Comercial e Partido também alimentam a publicação. A folha mescla os mapas das fatias. `app/ponto/motor_calculo_v2.py:1112–1119,2571–2573,2831,3058–3075,3311,3443–3447,3625` · `app/ponto/services/fechamento.py:324–329` | Folha e lavratura, testemunha, rubricas e contadores do espelho, calculador e comando de DIFF. `app/ponto/services/fechamento.py:932–946,1199–1216` · `app/folha/porta_export.py:100–105` · `app/ponto/services/espelho.py:437–453` · `app/ponto/calculador/alimentacao.py:73,193,239` · `app/ponto/calculador/periodo.py:596` · `app/ponto/management/commands/diff_calculador.py:214` |
| `_dia_de` | Devolve o dia publicado no mapa. Sem entrada devolve `None`. Com entrada e sem chave devolve a **data civil**, inclusive se o mapa estiver presente e incompleto. `app/ponto/services/dia_pago.py:109–132` | `lavrar`, fechamento, `dias_fora_do_dia_pago`, `somar_periodos` e `montar_dias`. `app/ponto/services/dia_pago.py:174,235,245,260` · `app/ponto/services/fechamento.py:932–946` · `app/folha/porta_export.py:100–105` · `app/ponto/services/espelho.py:85,443` |
| `dias_fora_do_dia_pago` | Produz em memória divergências entre períodos da autoridade e `DiaPago`, ou um motivo de não medição. `app/folha/porta_export.py:67–119` | `medir` acumula as divergências e os motivos. `app/folha/porta_export.py:587–601` |

O **app do espelho** usa o mapa por PK nas marcas, a geometria para o card e o resumo do espelho nos KPIs. O **calendário** agrupa por `t.data_turno` e lê o `DiaPago`. O **PDF** desenha o dia das linhas da tela e ainda calcula seu resultado local por `apurar_por_fatia` ou `calcular_mes`. A data civil continua chegando à **testemunha, ao espelho e à lavratura** pela contingência de `_dia_de`. `app/api/views.py:1361–1362,1429–1466,1499–1512` · `app/colaboradores/services/calendario.py:249–253,319,688` · `app/relatorios/pdf_espelho.py:411–432,495–497,590–597` · `app/ponto/services/dia_pago.py:126–132,174`

## 4. Classes de reprovação e contraexemplos

- **Segundo juiz / dois significados.** A projeção por PK e o dia pago ainda divergem quando o motor sobrescreve o dia geométrico pelo vão de 6 h. O par de `fim+1 11:00–19:00` reproduz essa divergência. `data_saida` e `data_turno` têm campos próprios. Já os valores de `dia_da_jornada` e do mapa por PK admitem data civil quando a atribuição está ausente. `app/ponto/motor_calculo_v2.py:2516–2522` · `app/ponto/turnos.py:331–334,3113–3116` · `app/ponto/services/dia_pago.py:126–132`
- **Derivação paralela / segundo escritor.** A cópia da O95 saiu. `limites_da_carga` calcula instantes de borda e `saida_encerra` compartilha a guarda de duração. `_mk` é o escritor do novo `data_saida`. As mudanças examinadas publicam mapas em memória e continuam entregando a lavratura de dinheiro à porta existente. `app/ponto/services/fechamento.py:657,1199–1216` · `app/ponto/turnos.py:411–412,331–334,3019–3029` · `app/ponto/services/dia_pago.py:359–360`
- **Relógio e troca de vínculo.** A pausa usa os instantes das marcas. O PDF usa o `agora` recebido no teto, e a classificação conserva a comparação do instante com o fim previsto quando chega ao cadastro. A troca de vínculo do `test_1` e a saída com vínculo diurno ou ausente do `test_4` passaram. O teste integrado do vigia entre 02:50 e 03:10 fica **[nao verificado]**. `app/ponto/turnos.py:411–412` · `app/relatorios/pdf_espelho.py:274–283` · `app/escala/servico_jornada.py:318–345` · `app/ponto/tests/test_par_c1v8_reds.py:115–145,200–236` · `app/ponto/tests/test_ponta_pequena_prazo_evento.py:390–405`
- **Dia 20, isolada, eco e feriado.** Em memória, o par simples **20/10 22:00 → 21/10 06:00** deu **8 h no dia 20 e 0 h na janela seguinte**. Deu **0 h de HE100** com feriado só na saída e **8 h de HE100** com feriado só no dia do turno, na configuração 12x36 ensaiada. A entrada isolada às 08:00 do último dia produziu um aberto e saldo negativo no `test_3b`. No caso do eco, o `test_2b` mantém os três PKs no mesmo turno aberto. A paridade `E > S` segue em **`api_justificativas`**, embora o dia das marcas venha do juiz. `app/docs/LEIS.md:117` · `app/ponto/motor_calculo_v2.py:941–961,2516–2545,2911` · `app/ponto/tests/test_dupla_contagem_dia20.py:30–44` · `app/ponto/tests/test_par_c1v8_reds.py:188–198,308–317` · `app/api/views.py:1948,2041–2064`

## Procedentes e REDs de aceite

1. **Herança ainda altera o dinheiro nas duas bordas.** Pela porta `recalcular_fechamento_mes` → `apurar_por_fatia`, exigir **8 h** para `ini 03:00–11:00` com o par anterior `18:00–22:00`. Fechar também duas janelas adjacentes com `dia 20 22:00 → dia 21 06:00` e o par distinto `dia 21 11:00–19:00`, exigindo **8 h em cada dono**. O `test_5` atual fica como controle do caso original. `app/ponto/services/fechamento.py:635,756–758` · `app/ponto/motor_calculo_v2.py:2516–2545` · `app/ponto/tests/test_par_c1v8_reds.py:238–250`
2. **Segunda virada perde a saída na borda direita.** Com janela pedida `[D,D]` e par `D 23:30 → D+2 01:30`, a folha deu **0 h e um aberto**. Com o pad largo reposto em memória, o mesmo ensaio deu **26 h e zero abertos**. O RED deve preservar `[D,D]` no PDF livre e na folha e exigir o par fechado no dia `D`. O teste alterado passou a pedir `[D,D+1]`, o que põe a saída de volta na carga. `app/ponto/turnos.py:321–334,1878–1882` · `app/ponto/services/fechamento.py:635` · `app/relatorios/pdf_espelho.py:278–283` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:72–99`
3. **Duas leituras podem interromper o fechamento.** Simulei a ausência de um PK na segunda leitura e reproduzi `KeyError` na porta real. O RED deve entregar uma marca à carga do motor e simular sua retratação antes de montar o mapa, exigindo fechamento com um conjunto coerente de marcas. Ocorrência em produção **[nao verificado]**. `app/ponto/services/fechamento.py:635,657–659,271–274` · `app/ponto/turnos.py:1908–1912,3008–3015`
4. **Selo da tela quebra na coleta.** O RED deve importar o módulo por seu caminho normal e rodar `test_MORDE_o_selo_distingue_JANELA_de_DIA`. Na cópia, a importação falha antes de chegar às asserções. `app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:1,147–148,233–240`
5. **Resíduos nos leitores e seus selos.** No PDF, um RED do censo deve capturar `_apurar_por_fatia` pelo nome real e flagrar a chamada no caminho de desenho. Hoje o resolvedor de apelidos só reconhece os nomes cadastrados, e `apurar_por_fatia` falta na lista. Em `api_justificativas`, um RED deve oferecer o caso `14:57 E / 19:01 S / 20:04 E` em que o juiz mantém um aberto, apesar de existir `S`, e exigir a resposta pela geometria. O teste atual do eco cobre o pareador. `app/relatorios/tests/test_pdf_nao_calcula.py:34–43,63–79` · `app/relatorios/pdf_espelho.py:411–429` · `app/api/views.py:2041–2064` · `app/ponto/tests/test_par_c1v8_reds.py:308–317`

## 27/11 12:56 · sessao f324b31a115f · PAR-PELO-TURNO · construtor · origem=Code

**PAR-PELO-TURNO c1v9 — recibo do construtor sobre `730c115e5`.** Em memória, `base` saiu com código 1 por falhas de asserção, `cura` passou 11 testes e `vizinhos` passou 206. As 15 reversões de produção saíram com código 1 por falha de asserção. [Executor e base](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v9_verificacao.py:12)

**Decisão técnica** — Fechamento, PDF e autoridade do espelho entregam ao motor a carga de dias locais `ini−1..fim+1`. O contexto ampliado serve ao julgamento por PK. O RED pela folha mediu **0,00 h → 8,00 h** em 21/10 e voltou a 0,00 h na reversão. [Fechamento](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/fechamento.py:632) · [PDF](/home/ronald/wt-codex-par-pelo-turno-111717/app/relatorios/pdf_espelho.py:281) · [Espelho](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/espelho.py:1026) · [RED](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:238)

| Cura | Antes → depois | Reversões vermelhas |
|---|---|---|
| `apurar_por_fatia` e `recalcular_fechamento_mes` leem `dia_das_batidas`. A O95 usa o fato posterior, inclusive quando ele está fora da carga do mapa. | Saldo **−193,60 h → 0,00 h** pela face pública da folha. [Leitura](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/fechamento.py:657) · [RED](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:160) | `folha`, `o95_provedor` |
| `_data_do_turno`, `_turnos_do_colab_puro` e `dia_das_batidas` resolvem o dia e a disputa pelo PK entre segmentos. | A entrada de **23/10 às 01:30** fica em **22/10** nos leitores e nas janelas do RED. Na troca de vínculo, a saída isolada das 20:00 fica em **22/10**. [Pareador](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/turnos.py:2025) · [RED](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:115) | `pk_dna`, `pk_contexto`, `pk_segmentos`, `pk_turnos` |
| `_marca_intra_duracao` usa a mesma guarda de saída degenerada nos dois laços e volta a considerar o eco como evidência de retorno da pausa. | Dois trechos sem pausa → um par com **60 min de pausa**. O cenário de flush volta a manter o turno aberto. [Separador](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/turnos.py:396) · [RED da pausa](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:147) · [RED do flush](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:308) | `pausa`, `separador` |
| `_mk` publica o dia da saída. `_turno_noturno_completo` e `classificar_falta` leem o par antes do cadastro. | A segunda virada passa com vínculo noturno, diurno ou ausente na saída. [Leitor](/home/ronald/wt-codex-par-pelo-turno-111717/app/escala/servico_jornada.py:214) · [REDs](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:200) | `noturno`, `noturno_ordem` |
| `dias_fora_do_dia_pago` pede o fim exato. `somar_periodos` e `montar_dias` usam `_dia_de`. | A testemunha perde o `fim+1` extra, o período sem mapa conserva **8 h** e o resumo publica **8 h, 2 turnos, 1 aberto** no recorte do RED. [Testemunha](/home/ronald/wt-codex-par-pelo-turno-111717/app/folha/porta_export.py:71) · [Soma](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/espelho.py:85) · [REDs](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:252) | `testemunha`, `soma`, `contagem` |

O controle do último dia já passava na base e ficou vermelho ao trocar `<=` por `<`. O censo registrou **9 pareadores, 78 derivações, A1=6, B1=75, composição=56 e B2−B1=113**. Ruff saiu com código 0 e a contagem de `^\+.*except` no diff deu **0**. [Controle](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:188) · [Catracas](/home/ronald/wt-codex-par-pelo-turno-111717/app/core/tests/test_medida_turno_do_par.py:108) · [Selo Ruff](/home/ronald/wt-codex-par-pelo-turno-111717/app/core/tests/test_contract_esmeril.py:22)

**Comandos de reprodução**, a partir de `app/`. [Modos e reversões](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v9_verificacao.py:55)

```bash
for modo in base cura vizinhos folha o95_provedor pk_dna pk_contexto \
  pk_segmentos pk_turnos pausa separador noturno noturno_ordem \
  testemunha soma contagem janela ultimo_dia; do
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.par_pelo_turno_c1v9_verificacao "$modo"
  printf '%s rc=%s\n' "$modo" "$?"
done
```

**Suíte para o Code na pista.** A tentativa local parou no import de `django.core`. A suíte integrada, inclusive a fixture original da ponta pequena e os ensaios de API e PDF com ORM, fica **[nao verificado]**. [Entrada Django](/home/ronald/wt-codex-par-pelo-turno-111717/app/manage.py:11) · [Fixture da ponta pequena](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_ponta_pequena_prazo_evento.py:388) · [Configuração da pista](/home/ronald/wt-codex-par-pelo-turno-111717/app/config/settings/ci.py:6) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto core chamados escala folha relatorios api --settings=config.settings.ci
```

Ficaram na fila do opus a herança do motor condicionada à RAMO2-B, os dois sentidos de `dias_inconsistentes`, a apuração paralela e o teto temporal do PDF, e o smoke de pauta. Os 29 casos reais, a Gestão de HE dos quatro casos do aval e o DIFF, hash, reversão e contador da frota da competência 11 ficam **[nao verificado]**. [Herança do motor](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/motor_calculo_v2.py:2521) · [PDF](/home/ronald/wt-codex-par-pelo-turno-111717/app/relatorios/pdf_espelho.py:278) · [Escopo dos casos reais](/home/ronald/wt-codex-par-pelo-turno-111717/app/docs/BACKLOG.md:2027)

## 27/11 12:47 · sessao 5fe4148eac5b · FERIADO-BOTAO-CORTADO · construtor · origem=Code

### Conta da c4 em três linhas

Na batida, o gatilho termina em `y=135,046875`. Com 2 px de folga, o alvo é `137,046875`. A medida bate com `137 − 685 = −548`. [Anexo `falhas_chromium.txt`, drawer de batida]
Nos três drawers medidos, o X bate com a coordenada arredondada do gatilho mais `266 = 1366 − 1100`. Nos dois feriados, o Y bate com `round(576 − 2 − 187,625) − 143 = 243`. [templates/core/_drawer_generico.html:15](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_drawer_generico.html:15)
Os deltas são compatíveis com coordenadas da viewport aplicadas na origem local do drawer rolado. O painel de `247,39 px` também veio sem o limite do drawer de `260 px`. A c4 lia o JS do `STATIC_ROOT`. O arquivo efetivamente servido e o `scrollTop` do job ficam [nao verificado]. [core/tests/test_smoke_chromium.py:80](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_chromium.py:80) [nao verificado]

### Cura e diff

**Decisão técnica** — `abre()` move o mesmo painel para `document.body` enquanto aberto, `posiciona()` aplica coordenadas da viewport diretamente e `fecha()` devolve o nó ao lugar original e restaura o foco quando ele estava no painel. [static/js/popover-acao.js:44](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:44)

- `test_smoke_clique_feriado_vira_trabalho.py::_serve_popover_js` serve o JS da cópia. `SONDA` monta o fragmento numa raiz nomeada e registra se encontrou o campo, o form e o gatilho. O anexo da `base_app.html` em página parou nessa localização. Para `base.html` em página, o anexo traz apenas o `FAIL`. A causa dos dois casos fica [nao verificado]. [core/tests/test_smoke_clique_feriado_vira_trabalho.py:98](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:98) [core/tests/test_smoke_clique_feriado_vira_trabalho.py:281](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:281)
- `test_selo_popover_acao.py::GeometriaPopoverChromiumTest._pagina` espera o `toggle`. O selo e `test_smoke_retratar_1366.py::SONDA` guardam a referência ao painel antes de abrir. O predicado e os `132px` seguem no template da grade. [core/tests/test_selo_popover_acao.py:124](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_selo_popover_acao.py:124) [core/tests/test_smoke_retratar_1366.py:54](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_retratar_1366.py:54) [templates/colaboradores/partials/_calendario_grade.html:68](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:68)

```bash
git diff -- static/js/popover-acao.js core/tests/test_selo_popover_acao.py \
  core/tests/test_smoke_clique_feriado_vira_trabalho.py core/tests/test_smoke_retratar_1366.py
```

### O que o smoke mede

Nos cinco casos, o smoke clica no gatilho, mede o painel dentro da viewport com margem de 8 px, testa cinco pontos contra recorte e exige distância de até 16 px do gatilho. Também confere o botão do formulário e a devolução do foco por Escape. [core/tests/test_smoke_clique_feriado_vira_trabalho.py:144](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:144)

| Casca | Local | Verificação específica |
|---|---|---|
| `base.html` | Página | POST de feriado e rolagem até `17:00`, com o gatilho visível |
| `base.html` | Drawer | Mesmas verificações pela aba Espelho |
| `base_app.html` | Página | POST de feriado e rolagem até `17:00`, com o gatilho visível |
| `base_app.html` | Drawer | Mesmas verificações pela aba Espelho |
| `base_app.html` | Drawer de 260 px | POST de inversão da batida `08:00` e rolagem interna com o painel aberto |

[core/tests/test_smoke_clique_feriado_vira_trabalho.py:326](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:326)

### Comandos para a pista

O RED `painel-sem-compensacao` retira a ida ao `body` e reaplica coordenadas da viewport dentro do drawer transformado. No modelo local, ele devolveu a batida para `y≈−548`. Os outros dois mutantes JS derrubaram, respectivamente, a rolagem interna e a volta do foco no modelo. [core/tests/test_smoke_clique_feriado_vira_trabalho.py:46](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:46)

```bash
smoke() {
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    core.tests.test_smoke_clique_feriado_vira_trabalho --settings=config.settings.ci
}

smoke
SMOKE_FERIADO_MUTANTE=painel-sem-compensacao smoke
SMOKE_FERIADO_MUTANTE=painel-sem-limite-drawer smoke
SMOKE_FERIADO_MUTANTE=painel-sem-devolver-foco smoke
SMOKE_FERIADO_MUTANTE=painel-distante smoke
SMOKE_FERIADO_MUTANTE=painel-recortado smoke
SMOKE_FERIADO_MUTANTE=gatilho-cortado smoke
SMOKE_FERIADO_MUTANTE=summary-display-none smoke
SMOKE_FERIADO_MUTANTE=porta-trocada smoke

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  core.tests.test_selo_popover_acao core.tests.test_smoke_retratar_1366 \
  --settings=config.settings.ci
```

Na cópia, passaram `git diff --check`, a checagem de sintaxe e os quatro testes estruturais do selo executados via `unittest`. Chromium, suíte do Code e smoke do Ronald ficam [nao verificado]. [core/tests/test_selo_popover_acao.py:74](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_selo_popover_acao.py:74) [nao verificado]

## 27/11 12:43 · sessao 41fd8bf08b27 · FERIADO-UM-JUIZ · verificador · origem=Code

**REPROVADO.** A promoção do 6x1 sem escala certa chama o juiz do motor e chega ao mensal e às duas lavras. O TXT pode eliminar essa mesma HE100 se a competência terminar em 12x36. A promoção também alcança exportadas fora da guarda e paga 470 minutos de HE100 no caso de turno partido em que o corte exige 440 de HE100 e 30 de intra. (`app/ponto/services/fechamento.py:362–381,988–1003,1219–1225,1296–1313`; `app/folha/export.py:357–380,420–423,739–748`; `app/ponto/services/fechamento.py:601–637`; `app/docs/RELATO.md:255`; `app/docs/PROMPTS.md:3016`)

## Aval, rubricas e fontes

O diff anexo preserva o arquivo do motor e `CICLOS_FERIADO_SIMPLES={'12x36'}`. A apuração **já** rodava um motor por fatia. O acréscimo é guardar `(motor, vínculo)` pelo `id` do período de folga trabalhada e usar esse contexto na classificação. Isso cabe no aval, inclusive quando dois períodos têm valores iguais. A cláusula CCT existente do 12x36 continua no caminho regular, e a nova promoção de folga exclui esse ciclo. (`FERIADO-UM-JUIZ-2711-1210.txt:1`; `diff.txt:1`; `app/ponto/services/fechamento.py:299–321,335–372`; `app/ponto/motor_calculo_v2.py:3740,3810–3821`; `app/core/regua_cct.py:473–479`)

Para um período de folga **com minutos já apurados**, a matriz é esta. Em particular, BO-B(3) com escala certa e `feriado_vira_folga=False` conserva `horas_folga_trabalhada`, rubrica de 100%. A L-097 anterior à partição ainda pode zerar o período sem autorização quando a escala é certa e o previsto é zero. Portanto, “escala certa só escolhe a rubrica” vale na partição dos minutos que passaram pelo motor. (`app/ponto/services/fechamento.py:362–373`; `app/ponto/calculador/regras.py:370–376`; `app/folha/export.py:439–447`; `app/ponto/motor_calculo_v2.py:1562–1597`)

| Condição | Destino dos minutos |
|---|---|
| Escala certa, juiz dá dobra e `feriado_vira_folga=True` | HE100 feriado. (`app/ponto/services/fechamento.py:369–373`; `app/ponto/calculador/regras.py:374–375`) |
| Escala certa, `feriado_vira_folga=False` | Folga trabalhada. O teste de porta declara 4 h nessa rubrica e zero em HE100 feriado. (`app/ponto/calculador/regras.py:374–375`; `app/ponto/tests/test_feriado_um_juiz_porta.py:101–109`) |
| Escala não certa | HE100 feriado se o juiz dá dobra e o ciclo admite a promoção. Nos demais casos, trabalhadas. (`app/ponto/services/fechamento.py:366–373`; `app/ponto/calculador/regras.py:313–321,376`) |
| Ciclo intermitente em `periodos_ft` | Trabalhadas. O motor normalmente manda o intermitente para `periodos`; o teste força a entrada em FT. (`app/ponto/motor_calculo_v2.py:2474–2478,2558–2566,2581–2607`; `app/ponto/calculador/regras.py:366–373`; `app/ponto/tests/test_feriado_um_juiz.py:90–95`) |

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| Minutos trabalhados, calendário, escala certa e dia | O v2 escreve `minutos_trabalhados`, pontas efetivas, intervalos e o mapa da jornada. O calendário vem do alcance nacional/estadual/municipal pela praça do vínculo do dia. `escala_certa_no_dia` lê LIMBO e assinaturas A2/A8 da competência. `_particionar_ft` lê tudo isso pelo dia de `_dia_de`. (`app/ponto/motor_calculo_v2.py:1947–1955,2513–2522,2571–2574,3353–3374`; `app/core/feriados.py:57–67,159–167`; `app/ponto/services/fechamento.py:710,798–803,362–381`; `app/escala/services/escala_certa.py:19–51`) |
| Trabalhadas, folga, HE50, HE100 total, HE100 feriado e split noturno | O fechamento lê a base de `resultado.periodos` e soma as três listas de FT nas rubricas correspondentes. Persiste o mensal por `save` ou `update`. A lavra `motor` recebe as mesmas listas e soma por dia. `soma_da_competencia` agrega as linhas por versão. (`app/ponto/motor_calculo_v2.py:294–344`; `app/ponto/services/fechamento.py:964–1003,1155–1188,1219–1225`; `app/ponto/services/dia_pago.py:173–186,234–267,550–563`) |
| `rubricas_ft` e `lavrar(rubricas=...)` | `alimentacao` projeta a partição por dia e recoloca a contribuição dos períodos normais daquele dia. `do_dia` consome a projeção, mas ainda calcula adicional noturno legal e intra. A mesma `lavrar` recebe `rubricas=` para `oraculo` a partir de 10/2026, valida os donos declarados na chamada, substitui valores declarados e escreve pelo mesmo `delete`/`bulk_create` por versão. (`app/ponto/calculador/alimentacao.py:353–380,503–504,551–568`; `app/ponto/calculador/regras.py:689–740,876–880`; `app/ponto/services/fechamento.py:1283–1315`; `app/ponto/services/dia_pago.py:50–75,312–360`) |
| Horas pagas do dia, rubricas exibidas e TXT | `pago_do_dia` soma trabalhadas, folga e `detalhe.folga_feriado_min/60`. Esse detalhe identifica só a FT promovida, pois a HE100 feriado também inclui feriado de período normal. Espelho, calendário e testemunha do export leem as lavras. O TXT lê o mensal, subtrai HE100 feriado do total para obter a HE100 comum e emite o adicional noturno à parte. (`app/ponto/services/dia_pago.py:244–258,833–856,867–928`; `app/ponto/services/espelho.py:823–831`; `app/colaboradores/services/calendario.py:688–689`; `app/folha/porta_export.py:113–124`; `app/folha/export.py:420–448,739–748`) |

## Procedentes e REDs

1. **Vínculo trocado no meio do mês perde dinheiro no TXT.** Reproduzi em memória: com 4 h classificadas como HE100 feriado pela fatia 6x1, a função de eventos dá `feriado=4, comum=0` com ciclo 6x1 e `feriado=0, comum=0` com 12x36 sem cláusula. A montagem real resolve o ciclo e a cláusula pelo último dia da competência. Há outro caso de perda: se o par promovido é o único trabalho do mês e o previsto é zero, o novo `horas_trabalhadas=0` pode acionar `cadastro_zero` e tirar o colaborador do TXT. O golden chama a função de eventos com ciclo fixo. **RED:** emitir o TXT real de 6x1 → 12x36 e exigir as 4 h da primeira fatia. Em outro caso, cadastrar só a folga com par de 4 h, código e rubrica válidos, previsto zero, e exigir a linha do feriado. (`app/folha/export.py:357–380,420–423,725–748,911–976`; `app/folha/views.py:214–219`; `app/ponto/tests/test_feriado_um_juiz_porta.py:63–71`)

2. **Exportada fora da guarda pode mudar.** A guarda efetiva cobre 09/2026. Nas demais exportadas o recálculo genérico registra aviso e segue, inclusive quando disparado por fato retroativo. A L-113 explica o comportamento geral, e o aval desta fatia veda tocar exportada. **RED:** exportar uma competência fora da guarda, guardar mensal, ambas as lavras e hash do TXT, disparar o evento retroativo da FT 6x1 e exigir igualdade. Usar a 11 aberta como controle que promove. Alteração efetiva em produção [nao verificado]. (`FERIADO-UM-JUIZ-2711-1210.txt:1`; `app/docs/LEIS.md:156`; `app/ponto/services/fechamento.py:36–50,601–637,1155–1225,1817–1843`)

3. **Há derivação paralela na auditoria e no oráculo sem projeção.** O registro F3 continua apontando para o motor, e `regras.py` usa o método emprestado. Para FT projetada, a lavra `oraculo` lê a partição. Já `diff_calculador` omite a partição, deriva FT de novo com `feriado_em_dobra` de um motor do recorte e classifica toda FT do lado “motor” como folga. Reproduzi 4 h na partição e zero no avulso com o flag do 12x36 final. Dias sem projeção continuam julgando feriado, com possibilidade de consulta nacional. O diff também passa a suprimir a dobra no caminho normal do oráculo para ciclo intermitente, além da FT. No cadastro sintético `tipo_base=intermitente`, `tipo_ciclo=6x1`, reproduzi juiz do motor falso e oráculo com 4 h de HE100 feriado. Presença desse cadastro na frota [nao verificado]. **RED:** comparar DIFF, mensal e lavras com os mesmos períodos em 6x1 → 12x36, e cobrar zero HE100 feriado nas duas versões para intermitência por ciclo e por base. (`app/core/juizes.py:318–321`; `app/ponto/calculador/alimentacao.py:419–436,551–568`; `app/ponto/calculador/regras.py:297–310,366–376,689–710,779–791`; `app/ponto/management/commands/diff_calculador.py:151–163,263–268,417–424`; `app/ponto/motor_calculo_v2.py:1000–1011,3694–3698,3764–3767`)

4. **Segundo escritor em memória e split acima do total.** O v2 escreve `PeriodoCalculo.minutos_extra_100_noturna`. O fechamento passa a sobrescrever o mesmo campo para FT promovida. Reproduzi um partido com `(HE100 bruta, HE100 feriado bruta, split) = (0, 0, 180 min)` após a partição. Reproduzi também no `MotorPlantao` um par de 26 h com prorrogação: 1.560 min de HE100 feriado e 1.680 de split, que a promoção reaproveita. A sobreposição da janela já existia no motor. A escrita persistente continua pela porta `lavrar`. **RED:** exigir que a partição preserve o período bruto, transporte o split da promoção e satisfaça `0 ≤ HE100 noturna ≤ HE100` em par longo e em partido com intervalo. No teste de troca de vínculo, comparar também `horas_noturnas` legais nas duas lavras, pois o oráculo usa a régua do recorte. (`app/ponto/motor_calculo_v2.py:82–118,151–160,1051–1066,3647–3652`; `app/ponto/services/fechamento.py:374–381,995–997`; `app/ponto/calculador/alimentacao.py:101–106,419–436,568`; `app/ponto/calculador/regras.py:716–736`; `app/ponto/services/dia_pago.py:223–225,249–253,359–360`)

5. **Pré-requisitos de intra e dia do turno.** No partido `10:00–14:00 + 14:30–18:20` o laço que apura intra entre pares percorre períodos normais, e a promoção soma os 470 min brutos. O corte K1 exige 440 de HE100 + 30 de intra. Na data, o caminho novo passa o mapa à partição e às lavras, mas `data_jornada` pode herdar a data anterior pelo intervalo de até 6 h, enquanto a dobra regular consulta o dia do juiz pela entrada. Uma divergência antes restrita à atribuição do dia passa a poder escolher se a FT dobra. **REDs:** reproduzir K1 pela porta e exigir 440 + 30 no mensal e nas lavras. Fazer dois turnos distintos com intervalo menor que 6 h na virada de um feriado e cobrar o dia do juiz em cada período, inclusive ao trocar a praça. A cópia registra a sequência E1 → K1 e a pendência do dia do par. Pouso da c1 antes dessas curas [nao verificado]. (`app/docs/RELATO.md:252–255`; `app/docs/PROMPTS.md:2956,3016`; `app/docs/BACKLOG.md:1998,2005`; `app/ponto/motor_calculo_v2.py:963–978,2513–2522,3081–3118`; `app/ponto/services/fechamento.py:362–372,990–997`; `app/ponto/services/dia_pago.py:126–132`)

6. **Partição disjunta, igualdade mensal incompleta e falha de relavra.** Cada ocorrência de FT entra numa lista. A HE100 feriado e o split são subconjuntos da HE100 total e o leitor diário usa só o detalhe da FT promovida para somar a hora uma vez. Ainda assim, 241 min normais + 241 min de FT comum dão **8,04 h** no mensal pelo arredondamento em duas etapas e **8,03 h** na soma lavrada. Em sucesso sequencial a relavra substitui as linhas por versão e repete os valores, com recriação dos registros. No caminho genérico, o mensal é salvo antes das lavras, que podem falhar separadamente. O diff não introduz ordem nova de travas nem leitura nova de relógio. **REDs:** usar os 241 + 241 min, repetir o recálculo e exigir igualdade em ambas as versões. Injetar falha no `bulk_create` depois do `delete` e exigir que o conjunto anterior permaneça íntegro. Para concorrência, intercalar entrega do TXT e recálculo retroativo da mesma competência. (`app/ponto/services/fechamento.py:362–383,964–965,1000–1003,1155–1209,1256–1259,1316–1327,1817–1843`; `app/ponto/services/dia_pago.py:173–184,259–267,359–360,550–563,894–898`; `app/ponto/models.py:380–387`; `app/folha/export.py:556–562`)

## Cobertura e prova do pronto

- **Véspera, municipal e abono.** A véspera com segundo par em D+1 tem mutante que troca o mapa pelo dia civil, e o teste de porta declara os dois sentidos da virada. Para municipal, o código usa a praça do vínculo do dia, mas o golden de porta só cadastra feriado nacional. **RED:** dois vínculos/postos, feriado municipal válido em um e feriado nacional como controle para ambos. Para folga + abono, o abono vem da grade e é `previsto − min(realizado, previsto)` em ausência. **RED:** a mesma FT em feriado com e sem ausência aprovada e previsto zero deve manter a HE e abono zero. Em dia previsto de 480 min com 240 realizados, exigir só os 240 restantes de abono. (`app/ponto/tests/feriado_um_juiz_mutantes.py:53–55`; `app/ponto/tests/test_feriado_um_juiz_porta.py:30–33,111–127`; `app/core/feriados.py:57–67,159–167`; `app/escala/services/leitor_celula.py:397–423`; `app/ponto/services/dia_pago.py:386–402`; `app/ponto/services/fechamento.py:1078–1082,1230–1231,1304–1305`)

- **Os mutantes mordem código de produção.** Executei os **16 controles sem banco** e os **12 mutantes**. Cada mutante passou no controle e falhou por asserção após a troca da função em memória. O mutante principal da escala certa exercita `do_dia` avulso. A bateria da porta Django e o RED contra o commit-base [nao verificado]. O modo `base` requer `git show`, e a cópia é um arquivo de trabalho sem `.git`. **RED de integração:** recolocar a exigência de escala certa no caminho do fechamento e exigir que o caso de 4 h falhe no mensal, nas duas lavras e no TXT emitido. (`app/ponto/tests/feriado_um_juiz_mutantes.py:30–72,100–126`; `app/ponto/tests/test_feriado_um_juiz_porta.py:47–99`; `CLAUDE.md:375–379`)

- **Produção [nao verificado].** Os 75 dias/42 colaboradores são o universo citado no aval. O material entregue não demonstra a medição nominal anterior como hora comum, o pouso desta c1, o apply da 11, a reversão, os hashes das exportadas ou o antes × depois por colaborador e rubrica com DIFF do TXT 11. O registro de pouso do FERIADO-POR-POSTO refere-se a outra entrega. (`FERIADO-UM-JUIZ-2711-1210.txt:1`; `CLAUDE.md:453–466`; `app/docs/RELATO.md:721`)

## 27/11 12:42 · sessao 83c2930181f4 · - · - · origem=Ronald (tmux)

**REPROVADO.** A partição preserva os minutos de cada FT em uma única lista por identidade, com `horas_extras_100_feriado` e o split noturno como recortes de HE100. O mensal e as duas lavras recebem as mesmas três listas. [fechamento.py:349](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:349) [fechamento.py:988](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:988) [fechamento.py:1219](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:1219) [fechamento.py:1293](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:1293)

- **Troca de vínculo perde a rubrica no TXT.** Uma FT de 4 h na fatia 6x1 sem escala certa passa a HE100 feriado. Se o mês termina em 12x36 sem cláusula, o exportador zera o feriado do mês inteiro. A via da tela usa o fechamento recalculado em memória, portanto a perda independe de apply. RED com 6x1 no feriado e 12x36 no último dia, cobrando a linha do TXT com as 4 h. Reproduzi em memória a mudança do evento de 4 h para zero ao aplicar o ciclo final. [fechamento.py:364](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:364) [folha/export.py:357](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/folha/export.py:357) [folha/export.py:420](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/folha/export.py:420) [folha/export.py:739](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/folha/export.py:739) [folha/views.py:214](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/folha/views.py:214)
- **Segundo escritor do split.** O fechamento altera `p.minutos_extra_100_noturna`, que o motor escreve. Em FT de turno partido, pode escrever 180 no objeto com HE100 bruta zero, já que a dobra do motor percorre só os períodos normais. RED preserva os campos do período antes/depois da partição e exige os 180 minutos na rubrica lavrada. [fechamento.py:374](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:374) [motor_calculo_v2.py:1051](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/motor_calculo_v2.py:1051) [motor_calculo_v2.py:3197](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/motor_calculo_v2.py:3197)
- **Split pode exceder a própria HE100.** Reproduzi com `MotorPlantao` real em memória, FT de 26 h, início às 22h do feriado, prorrogação ativa e escala incerta. A promoção leva 1.680 min noturnos para um total de 1.560 min. A sobreposição da prorrogação com a janela seguinte é um defeito herdado, agora exposto no ramo promovido. RED exige split ≤ HE100 em mensal e lavras, como a propriedade P4 já exige do período. Ocorrência na frota [nao verificado]. [motor_calculo_v2.py:105](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/motor_calculo_v2.py:105) [motor_calculo_v2.py:151](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/motor_calculo_v2.py:151) [fechamento.py:995](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:995) [test_o280_bateria_da_lei.py:2909](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/tests/test_o280_bateria_da_lei.py:2909)
- **O DIFF avulso ainda deriva FT.** `diff_calculador` omite a partição, a alimentação monta uma CCT única e o avulso chama a dobra. Reproduzi `feriados=()` com nacional mockado verdadeiro. FT marcou zero e dia de trabalho marcou 4 h, pois vazio no primeiro é calendário injetado e no segundo admite consulta nacional. RED compara avulso, DIFF e lavra na troca de vínculo e exige que vazio explícito tenha a mesma semântica. [diff_calculador.py:417](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/management/commands/diff_calculador.py:417) [alimentacao.py:419](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/calculador/alimentacao.py:419) [regras.py:689](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/calculador/regras.py:689) [regras.py:777](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/calculador/regras.py:777)
- **Igualdade mensal × soma tem contraexemplo herdado.** 241 min normais + 241 min FT comum resultam em 8,04 h no mensal por duplo arredondamento e 8,03 h na soma das lavras. RED de porta com as duas parcelas de 241 min. Recalcular com sucesso preserva os valores e substitui as linhas por versão. Falha entre `delete` e `bulk_create` pode deixar o mensal salvo com a lavra ausente no caminho sem atomic externo. RED injeta a falha e cobra a preservação do snapshot. [fechamento.py:964](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:964) [fechamento.py:1000](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:1000) [dia_pago.py:359](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/dia_pago.py:359) [dia_pago.py:550](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/dia_pago.py:550) [fechamento.py:1155](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:1155) [fechamento.py:1256](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:1256)

**Demais casos.** A transferência de contexto em `apurar_por_fatia` fica dentro do objetivo, porque registra o motor da fatia sem recalcular. `12x36` fica fora da promoção, inclusive quando o motor conhece uma cláusula; `feriado_vira_folga=True` nesse ciclo não é um cenário real do predicado. A ida e a volta através da meia-noite estão nos dois testes de porta. BO-B(3) com certa e `feriado_vira_folga=False` preserva 4 h em FT com zero HE100 feriado. O abono usa o mesmo mapa da grade para as três gravações e o caso conjunto com FT permanece [nao verificado]; RED exige trabalho uma vez e abono limitado ao previsto descoberto, zero na folga sem previsto. [fechamento.py:308](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:308) [fechamento.py:369](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:369) [escala/models.py:32](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/escala/models.py:32) [escala/models.py:1400](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/escala/models.py:1400) [test_feriado_um_juiz_porta.py:101](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/tests/test_feriado_um_juiz_porta.py:101) [dia_pago.py:386](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/dia_pago.py:386)

**Prova local.** Rodei sem escrita o controle com 16 testes verdes e os 12 mutantes. Cada mutante fez seu teste falhar por asserção com controle verde. O mutante da exigência de escala certa chama `do_dia` diretamente; o runner não verifica o TXT nem as escritas da porta. [feriado_um_juiz_mutantes.py:29](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/tests/feriado_um_juiz_mutantes.py:29) [feriado_um_juiz_mutantes.py:112](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/tests/feriado_um_juiz_mutantes.py:112)

## 27/11 12:42 · sessao d8d413482ec1 · - · - · origem=Ronald (tmux)

**REPROVADO.** A nova partição promove as horas da fatia 6x1, mas o TXT pode perder toda a promoção quando o vínculo no último dia da competência é 12x36 simples. Reproduzi sem banco 4 h de HE100 feriado apuradas e 0 h emitidas tanto em HE100 feriado como em HE100 normal. A montagem do TXT obtém ciclo e cláusula no fim da janela, depois usa esse ciclo para suprimir o subtotal mensal inteiro. **RED:** 6x1 com 4 h de folga em feriado e escala não certa, migração para 12x36 simples no mesmo mês, TXT real conserva as 4 h da primeira fatia. O teste atual chama o emissor diretamente com ciclo único. `app/ponto/services/fechamento.py:363-372,990-997`; `app/folha/export.py:341-370,420-423,739-748`; `app/ponto/tests/test_feriado_um_juiz_porta.py:63-71`.

- **Segunda derivação no DIFF.** A lavra do oráculo recebe a partição do fechamento e a usa em dia FT. O comando `diff_calculador` omite a partição e, mesmo com os períodos do fechamento, chama o ramo avulso que recompõe a decisão com a flag CCT de um único motor e o ciclo do vínculo do dia. O lado chamado “motor” do mesmo DIFF ainda classifica todo FT como folga trabalhada. **RED:** 6x1 FT em feriado, sem escala certa, exige 4 h HE100 feriado no fechamento, lavra e lado motor do DIFF. Repetir com troca 6x1 → 12x36 prova o erro de alimentação do lado calculador. `app/ponto/services/fechamento.py:1283-1313`; `app/ponto/calculador/regras.py:689-710,780-783,876-879`; `app/ponto/calculador/alimentacao.py:419-436,521-530,551-568`; `app/ponto/management/commands/diff_calculador.py:151-163,263-268,417-424,527-542`.
- **Escrita em memória duplicada.** `fechamento` escreve `PeriodoCalculo.minutos_extra_100_noturna`, que o motor escreve e zera. O helper é o mesmo, com argumentos distintos. A partição usa pontas efetivas e só faz isso se o período bruto não veio marcado como feriado em toda a duração. O resultado em memória pode carregar split promovido sem carregar o respectivo total no próprio período. **RED:** promoção FT de turno partido conserva o período bruto e entrega 180 min noturnos pelo registro explícito da partição; selar a ausência de nova atribuição externa ao campo bruto. `app/ponto/services/fechamento.py:374-381`; `app/ponto/motor_calculo_v2.py:1051-1065,3052-3054,3452-3454`.
- **Mensal versus lavras.** O mensal arredonda `horas_trabalhadas` antes de somar FT comum e depois arredonda novamente. Um minuto comum e um minuto de FT comum rendem 0,04 h no mensal e 0,03 h na soma das lavras. A fórmula é preexistente. Em FT, o oráculo ainda recompõe adicional noturno legal e intra e pode usar a régua da última escala em toda a janela, enquanto o motor usa a CCT da fatia. **REDs:** comparar mensal e lavras para dois minutos distribuídos entre as duas parcelas e, em fixture 6x1 → 12x36, comparar adicional legal, split, intra e abono dia a dia. `app/ponto/services/fechamento.py:964-965,1000-1003,786-803`; `app/ponto/services/dia_pago.py:178,223-225,264,559-563`; `app/ponto/calculador/regras.py:716-736`; `app/ponto/calculador/alimentacao.py:419-436,568`; `app/core/regua_cct.py:153-166`.
- **Porta, idempotência e trava.** O diff não acrescenta escritor persistente nem inversão de travas. As duas versões entram pelo `lavrar`, que substitui por chave de versão depois de validar donos declarados no conjunto de dias. Repetição bem-sucedida substitui as linhas. A porta não garante consistência após falha no caminho direto ou por `on_commit`, pois o mensal grava antes, as exceções diárias são absorvidas e a substituição faz `delete` seguido de `bulk_create`. **RED:** `TransactionTestCase`, falha injetada na criação das linhas depois da exclusão, exigindo mensal e duas lavras anteriores intactos. O risco é preexistente. `app/ponto/services/fechamento.py:1155-1188,1217-1259,1296-1320,1826-1844`; `app/ponto/services/dia_pago.py:312-334,359-360`.
- **Data e relógio.** A chave da partição, projeção e lavras vem de `dia_da_jornada[id(p)]`; `_dia_de` cai no dia civil da entrada quando o id não tem mapeamento. Os mutantes provam que trocar o caso mapeado para civil muda o resultado. Nenhuma nova leitura corrente de relógio nesses ramos. **RED complementar:** o par da madrugada tem dia mapeado e todos os leitores mantêm a data da véspera, e ausência de mapeamento de FT é detectada. `app/ponto/motor_calculo_v2.py:911-922,1112-1119,2571-2573`; `app/ponto/services/fechamento.py:362-375,1235,1306`; `app/ponto/calculador/alimentacao.py:360-374`; `app/ponto/services/dia_pago.py:109-132,234-267`; `app/ponto/tests/feriado_um_juiz_mutantes.py:53-55`.
- **Linhagem dos leitores.** Mensal parte dos `ResultadoMes.periodos`, que excluem FT nos totais de HE e trabalho, e soma a lista exclusiva correspondente. Lavra motor lê as mesmas três listas, e lavra oráculo recebe `Rubricas.valores` para substituir os campos declarados pela mesma porta. `pago_do_dia` lê trabalhadas + folga + minutos de folga-feriado, enquanto `rubricas_do_dia` lê HE100 total sem somar subconjuntos. Leitores diretos incluem espelho, calendário, testemunha do export e app pela casca do espelho. `app/ponto/motor_calculo_v2.py:294-344`; `app/ponto/services/fechamento.py:964-1003,1217-1245,1283-1313`; `app/ponto/services/dia_pago.py:173-186,234-267,312-334,867-928`; `app/ponto/services/espelho.py:132-136,823-831`; `app/colaboradores/services/calendario.py:688-698`; `app/folha/porta_export.py:106-124`; `app/api/views.py:1453,1537-1538`.
- **Provas.** Controle sem banco executado, 16 verdes. Os 12 mutantes executados separadamente, cada um com controle verde e uma falha de asserção sem erro após alterar a função produtiva em memória. `exige_escala_certa` verifica só `do_dia` avulso. Nenhum desses mutantes executa a ponta persistente/TXT. O runner `base` depende de `git show`; execução do RED histórico, da suíte com banco e evidências de pré-medição, apply, reversão, TXT de produção e ordem de pouso são [nao verificado]. `app/ponto/tests/feriado_um_juiz_mutantes.py:30-72,104-126`; `CLAUDE.md:421-431,453-466`.

## 27/11 12:42 · sessao 956c8f679aa0 · - · - · origem=Ronald (tmux)

**REPROVADO.** O caso simples de FT 6x1 sem escala certa é promovido pelo juiz v2. A escala certa conserva o BO-B, com HE100 feriado quando `feriado_vira_folga=True` e folga trabalhada quando é `False`. A lista de ciclos simples e a exceção CCT do 12x36 permanecem no motor e na régua. [fechamento.py:350](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:350), [regras.py:370](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/calculador/regras.py:370), [motor_calculo_v2.py:3740](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/motor_calculo_v2.py:3740), [regua_cct.py:473](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/core/regua_cct.py:473)

- **K1 é pré-requisito material.** O cenário 10:00–14:00 e 14:30–18:20 tem 470 minutos trabalhados e 30 de intra suprimida. O partido só apura o suprimido em `resultado.periodos`, e esta promoção soma os 470 brutos. O RED deve exigir 440 de HE100 feriado e 30 de intra no mensal e nas duas lavras. K1 tem aval separado, com uso do juiz de intra nos dois caminhos. [PROMPTS.md:3016](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/docs/PROMPTS.md:3016), [RELATO.md:255](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/docs/RELATO.md:255), [motor_calculo_v2.py:3078](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/motor_calculo_v2.py:3078), [fechamento.py:990](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:990)
- **Troca de vínculo perde verba no TXT.** O contexto novo por identidade apenas conserva o motor da fatia que já rodava e fica dentro do escopo para FT. O exportador lê o ciclo no último dia. Se a primeira fatia foi 6x1 e a última 12x36 sem CCT, o exportador retira as 4 h da HE100 comum e zera o evento feriado. RED pela geração real de TXT com a troca, feriado municipal da primeira praça, controle nacional e 4 h emitidas na rubrica feriado. [fechamento.py:299](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:299), [core/feriados.py:159](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/core/feriados.py:159), [folha/export.py:341](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/folha/export.py:341), [folha/export.py:420](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/folha/export.py:420), [folha/export.py:739](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/folha/export.py:739)
- **FT promovida pode virar conta vazia e sair do TXT.** Com único par promovido e zero previsto na janela, `horas_trabalhadas==0` passa pelo gate e vira `cadastro_zero`, embora haja HE100 feriado gravada. RED com esse único par, vínculo curto de folga, rubrica e código válidos, exigindo `entra` e a linha no TXT real. O teste atual chama `eventos_do_fechamento` diretamente. [folha/export.py:839](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/folha/export.py:839), [folha/export.py:911](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/folha/export.py:911), [folha/export.py:955](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/folha/export.py:955), [test_feriado_um_juiz_porta.py:63](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/tests/test_feriado_um_juiz_porta.py:63)
- **Intermitente e oráculo.** A regra inteira sem FT do oráculo muda para excluir intermitentes, fatia antes registrada separadamente. Reproduzi em memória outro desvio com `tipo_base=intermitente`, `tipo_ciclo=6x1`, feriado e 4 h em dia previsto. O v2 escolhe `MotorIntermitente` e responde `False`, e o oráculo afirma 4 h de HE100 feriado porque seu detector lê só ciclo. RED de integração com esses dois atributos independentes, exigindo 4 h normais e zero HE100 feriado em ambas as versões. [BACKLOG.md:2247](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/docs/BACKLOG.md:2247), [motor_calculo_v2.py:3694](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/motor_calculo_v2.py:3694), [motor_calculo_v2.py:3764](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/motor_calculo_v2.py:3764), [regras.py:366](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/calculador/regras.py:366), [regras.py:779](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/calculador/regras.py:779)
- **Exportada.** A regra passa pelo recálculo por evento retroativo. Só a 09/2026 está protegida pela guarda do gravado. RED com exportada fora da guarda exigindo mensal, DiaPago e hash estáveis depois do evento, com controle positivo na 11 aberta. Medição prévia nominal, pouso dessa fatia, apply, reversão, lista antes/depois e DIFF TXT 11 [nao verificado]. [fechamento.py:36](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:36), [fechamento.py:620](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:620), [fechamento.py:1817](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:1817), [CLAUDE.md:453](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/CLAUDE.md:453)
- **Dia do turno.** O golden cobre uma cauda já mapeada. O motor ainda pode sobrepor a resposta do juiz com herança de 0–6 h, divergência em fila. A promoção agora consome o mapa para decidir dinheiro. RED de dois turnos distintos separados por menos de seis horas na virada do feriado, exigindo que cada um siga a data do juiz, em contraste com a cauda verdadeira. [motor_calculo_v2.py:2516](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/motor_calculo_v2.py:2516), [BACKLOG.md:1998](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/docs/BACKLOG.md:1998), [fechamento.py:362](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/services/fechamento.py:362)
- **Mutantes.** Rodei os 16 controles sem banco e os 12 mutantes. Todos os mutantes fizeram o teste nomeado falhar por asserção após controle verde. Eles compilam funções de produção, mas a promoção central tem como alvo o caminho avulso. Execução dos testes Django e comparação contra a base [nao verificado]. [feriado_um_juiz_mutantes.py:30](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/tests/feriado_um_juiz_mutantes.py:30), [feriado_um_juiz_mutantes.py:104](/home/ronald/codex_ws/4a18bdfedb20550713008c180d844c8e95222a0a/app/ponto/tests/feriado_um_juiz_mutantes.py:104)

## 27/11 12:33 · sessao f4c8c5fec76b · FERIADO-UM-JUIZ · construtor · origem=Code

A cura do **FERIADO-UM-JUIZ c1** está na cópia. O HEAD segue em `59600163ccc9cffb8f3ae148beede7cd7866ae86`. A proibição explícita de `git commit` prevaleceu sobre o item (iv). [Diff completo](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz.patch:1) · [Log:411](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:411)

### Cura

| Arquivo e funções | Mudança |
|---|---|
| `fechamento.py::apurar_por_fatia`, `_particionar_ft`, `recalcular_fechamento_mes` | Cada período usa o motor do próprio vínculo e o dia de início da jornada. A partição soma a HE100 de feriado uma vez, com o subconjunto noturno, e alimenta as duas lavras. [fechamento.py:312](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/services/fechamento.py:312) · [fechamento.py:349](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/services/fechamento.py:349) · [fechamento.py:988](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/services/fechamento.py:988) |
| `regras.py::_ContextoFeriado.__init__`, `dobra_de_feriado_do_dia`, `intermitente_do_tipo`, `rubrica_da_folga`, `do_dia` | A regra compartilhada exclui o intermitente e usa a escala certa para escolher a rubrica. A ampliação preserva o 12x36 e o tratamento BO-B(3) da escala certa. [regras.py:297](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/regras.py:297) · [regras.py:366](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/regras.py:366) · [regras.py:690](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/regras.py:690) |
| `alimentacao.py::_rubricas_da_particao_ft`, `rubricas_por_dia` | O oráculo projeta a partição do fechamento por dia e conserva a parcela dos períodos normais em dias com FT. [alimentacao.py:353](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/alimentacao.py:353) · [alimentacao.py:557](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/alimentacao.py:557) |

### REDs e mutantes

Na base, o 6x1 sem escala certa entregou **0 h de HE100 feriado** em vez de 4 h. O noturno entregou **0 h** em vez de 3 h. O intermitente recebeu **4 h de HE100 feriado** em vez de zero. Na cura, **16 testes em memória passaram**. O diff tem **0 cláusulas `except` adicionadas e 0 removidas**. [Log:19](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:19) · [Log:27](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:27) · [Log:37](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:37) · [Log:66](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:66) · [Log:410](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:410) [nao verificado]

Os **12 mutantes** falharam por asserção. Cada link abre o comando completo e o resultado correspondente. [Arquivo de mutantes](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_mutantes.py:29)

| O que o teste pega | Comandos e resultados | [nao verificado]
|---|---|
| Escala certa volta a impedir a dobra do 6x1 | [`exige_escala_certa`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:72) |
| Intermitente ganha HE100 feriado | [`intermitente_em_ft`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:99) · [`intermitente_no_oraculo`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:130) |
| Calendário nacional ou motor da última fatia toma o lugar do contexto do período | [`calendario_nacional`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:157) · [`aplica_motor_da_ultima_fatia`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:184) · [`perde_contexto_da_primeira_fatia`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:220) |
| Segundo par da véspera dobra pela data civil | [`aplica_dia_civil`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:246) |
| FT do 12x36 ganha HE100 feriado | [`promove_12x36`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:277) · [`promove_12x36_no_oraculo_avulso`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:308) |
| A promoção perde o noturno ou o oráculo perde uma parcela do pagamento | [`perde_noturno`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:333) · [`oraculo_copia_so_folga`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:358) · [`oraculo_perde_periodo_normal`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:383) |

### Fora desta volta

Os testes de persistência para LIMBO, A2, A8, noturno partido e véspera ficaram para o Code. Eles comparam o mensal com a soma das duas lavras e conferem o evento de folha e os leitores do dia. O teste legado manteve o controle 12x36 e ganhou o 6x1 com o aval citado. A execução desses testes com banco e a certificação Code/Opus ficam **[nao verificado]**. [Teste da porta:58](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/test_feriado_um_juiz_porta.py:58) · [Teste legado:227](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/test_he100_dia_sem_previsto.py:227) [nao verificado]

O anexo registra **75 dias de 42 colaboradores** como hora comum e projeta **67 dias de 35 colaboradores**, somando **449,74 h**, para HE100 feriado. O apply da 11, a reversão, o antes × depois por colaborador e o diff do TXT ficam **[nao verificado]**. `antes_m1.md:3`

## 27/11 12:32 · sessao 19ca023221ab · FERIADO-BOTAO-CORTADO · construtor · origem=Code

**FERIADO-BOTAO-CORTADO c4.** A correção está na cópia. `posiciona()` mede o painel em `left/top = 0` e após um deslocamento de 64 px. Com essas leituras, converte a posição desejada na viewport para a origem e a escala reais do `fixed`, tanto sob o `backdrop-filter` da página quanto sob o `transform` do drawer. [Popover](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:81) · [`.hx-shell`](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/css/hasner-ponto.css:275) · [Drawer](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_drawer_generico.html:3)

O encaixe usa a interseção da viewport, com 8 px de margem, com a área útil dos ancestrais que recortam o painel. Se faltar altura, o painel fica no lado com mais espaço e rola por dentro. O formulário permanece no `details`. A mudança de produto está no JS compartilhado. [Área visível](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:48) · [Encaixe](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:109) · [Rolagem](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:169)

### Diff e smoke

- `red_painel_popover_feriado.py` foi removido após a incorporação das asserções ao smoke coletado. Ele mede margem, recorte por hit-test e distância de até 16 px sem sobreposição nos quatro cenários do feriado. O quinto cenário abre **Inverter tipo/Retratar** no drawer de `base_app.html`, limitado a 260 px no ensaio, e verifica também que a rolagem interna mantém o painel aberto. [Asserções](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:143) · [Cenários](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:302)
- O selo existente ficou como está. Ele exige posicionamento por medida e testa a geometria na última coluna em duas larguras. [Selo](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_selo_popover_acao.py:104) · [Geometria](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_selo_popover_acao.py:166)

```bash
git --no-optional-locks diff a2414b6605ff01965417728ccb271aa82a294f04 -- \
  static/js/popover-acao.js \
  core/tests/test_smoke_clique_feriado_vira_trabalho.py \
  core/tests/red_painel_popover_feriado.py
```

### Comandos para a sessão de Chromium

Na pista isolada, com `STATIC_ROOT` atualizado. Os mutantes de JS alteram somente a cópia do script carregada pela página temporária. [Carregamento de estáticos](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_chromium.py:80) · [Injeção](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:260)

```bash
smoke() {
  PYTHONDONTWRITEBYTECODE=1 python manage.py test \
    core.tests.test_smoke_clique_feriado_vira_trabalho "$@" \
    --settings=config.settings.ci --noinput
}

SMOKE_FERIADO_MUTANTE= smoke core.tests.test_selo_popover_acao
SMOKE_FERIADO_MUTANTE=gatilho-cortado smoke
SMOKE_FERIADO_MUTANTE=summary-display-none smoke
SMOKE_FERIADO_MUTANTE=porta-trocada smoke
SMOKE_FERIADO_MUTANTE=painel-recortado smoke
SMOKE_FERIADO_MUTANTE=painel-distante smoke
SMOKE_FERIADO_MUTANTE=painel-sem-compensacao smoke
SMOKE_FERIADO_MUTANTE=painel-sem-limite-drawer smoke
```

| Mutante | Asserção esperada no RED |
|---|---|
| `gatilho-cortado`, `summary-display-none` | `gatilhoVisivel`. [Sonda](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:115) |
| `porta-trocada` | `action`. [Troca da porta](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:291) |
| `painel-recortado` | `painelSemRecorte`. [Hit-test](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:148) |
| `painel-distante`, `painel-sem-compensacao` | `painelEncostado`. [Posição sem compensação](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:46) · [Distância](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:143) |
| `painel-sem-limite-drawer` | `painelSemRecorte` e `painelRolavel` no cenário da marcação. [Mutante](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:53) · [Sonda](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:174) |

Validei a sintaxe Python e JS e executei isoladamente as quatro verificações estruturais do selo. Em DOM simulado, o mutante de compensação perdeu a ancoragem na página e no drawer, e o mutante do limite voltou a recortar o painel no drawer curto. [Selo estrutural](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_selo_popover_acao.py:74) · [Compensação](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:95) [nao verificado]

**[nao verificado]** Smoke e mutantes no Chromium, casos geométricos do selo, `bin/node_check.sh`, suite do Code, revisão final do Opus e smoke de tela do Ronald.

## 27/11 12:25 · sessao 85323386be77 · E4C-F5-INSTANTE · verificador · origem=Code

**Cura4 reprovada pelo aval literal.** Na resposta comum, o `post_save` emite e `DecisaoPergunta.registrar` suprime a própria emissão. A contagem fica em um, mas o aval atribui a emissão ao `registrar`. Há ainda um contraexemplo em que uma decisão nova pode gravar **zero eventos** ao consumir um recibo da transação anterior. `aval_item6.txt:1` (anexo), `app/chamados/services/disputa_emissao.py:1562–1567`, `app/chamados/signals.py:261–265`, `app/chamados/models.py:3653–3669,4833–4839`.

### 1. PROCEDENTE — o emissor do par está invertido

A tabela conta os inserts do outbox da pergunta por célula encontrada, em caminhos concluídos com sucesso. Todos passam pelo mesmo escritor físico, `emitir_evento_celula`. Sem célula pela FK ou pelo par colaborador/dia, são zero inserts. `app/chamados/models.py:3605–3636`, `app/ponto/portas/celula.py:981–991`.

| Caminho | Quem aciona o insert |
|---|---|
| `save` + `registrar`, mesma instância e mesmo atomic | **1, `post_save`**. O `registrar` consome o recibo pendente. Vale para a resposta humana, a automática, a resposta pela trilha e os pares de carimbo e validação. `app/chamados/signals.py:255–265`, `app/chamados/models.py:3520–3523,4832–4839`, `app/chamados/services/disputa_emissao.py:1562–1567,1760–1763,1794–1797`, `app/chamados/services/validacao.py:174–178,312–318,389–395`. |
| `save` sem `registrar` | **1, `post_save`**, se o save for inteiro ou nomear algum campo do conjunto. Save parcial fora do conjunto ou `raw` dá **0**. `app/chamados/signals.py:255–265`. |
| `registrar` sem `save` e sem recibo | **1, `registrar`**, para lote comum não vazio. Lote vazio dá **0**. `app/chamados/models.py:4796–4805,4824–4839`. |
| `created` | **0 pelo `post_save`**. `_nascer` chama `registrar` e dá **1** se houver assinatura para registrar, **0** se o lote vier vazio. `app/chamados/signals.py:255–257`, `app/chamados/models.py:3322–3327,4540–4559,4799–4803,4835–4839`. |
| `reabrir_para_correcao` | **2** na célula que entra tanto no lote de atos quanto nas linhas capturadas. O `registrar(pergunta_id=...)` emite primeiro e a porta emite de novo depois do `update`. Se só a captura contém a célula, **1**, pela porta. O recibo erra ao contar sempre um. A ressuscitação e a devolução L-130 também têm emissão explícita além do `registrar`. `app/chamados/services/disputa_emissao.py:1680–1682,1695–1716`, `app/chamados/models.py:1728–1733,3709–3711`, `app/chamados/services/validacao.py:481–494`. |
| `fechar_por_admin` | **1, `post_save`**. `carimbar_identidade` faz um save e retorna. Nos chamadores que registram `SILENCIO` logo depois com a mesma instância, esse `registrar` consome o recibo daquele save. O caminho com `SILENCIO` cai de **2 para 1**, não de 3 para 2. `app/chamados/models.py:2177–2184,3363–3377,3562–3571,4832–4839`, `app/ponto/services/veredito_celula.py:194–202`. |
| Retratação de motivo | **2** quando o carimbo grava. Primeiro emite `registrar(RETRATACAO)`, depois o `post_save` do carimbo de identidade. A ordem é inversa à do `SILENCIO`, e já eram dois inserts na base. `app/chamados/models.py:2315–2323,3562–3571`, `app/chamados/signals.py:255–265`. |
| Estoque (`entrada_origem=ESTOQUE`) | **0 pelo `registrar`**. A porta consome eventual recibo e pula a emissão. Um save anterior pode já ter emitido pelo signal. Os backfills antigos citados usam atos comuns e seguem o ramo emissor quando o lote contém atos. `app/chamados/models.py:4833–4860`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:357–361`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:161–164`. |

**Dá para cumprir o aval pela porta existente.** Uma opção é `registrar` receber a operação de salvar a projeção, executá-la sob a trava e no mesmo atomic, e sinalizar ao receiver de outbox, durante aquele save, que a emissão pertence ao `registrar`. O escopo da sinalização acaba em `finally`. Saves avulsos continuam pelo receiver e o receiver do beco continua recebendo o `post_save`. O custo é alterar a assinatura e migrar os pares de chamadas, preservar a ordem das travas, tratar explicitamente lotes vazios e cobrir rollback e falha de emissão. Se a porta também assumir as reaberturas, precisa receber as células capturadas antes do `update`, pois o filtro pode perder as perguntas depois. A intenção fica conhecida antes do save. `app/chamados/models.py:3605–3608,3891–3907,4796–4839`, `app/chamados/signals.py:229–240,254–265`, `app/chamados/services/disputa_emissao.py:1680–1682,1713–1716`.

### 2. PROCEDENTE — a marca mistura recibo de emissão com identidade da escrita

| Valor | Quem escreve e quem lê |
|---|---|
| `recibo` e `ja_acordadas` | O signal abre `{}`. `acordar_celulas` põe `celula.pk → evento.pk` após o insert. `guardar_recibo_da_escrita` copia o mapa para a instância. `ja_acordadas_na_escrita` produz o conjunto de células e o `registrar` o devolve a `acordar_celulas`, que o usa para iniciar `vistos`. `app/chamados/signals.py:261–265`, `app/chamados/models.py:3621–3636,3650–3669,4835–4839`. |
| `_acordou_na_escrita` | `guardar_recibo_da_escrita` mescla os IDs no `__dict__`. Outro save da mesma célula sobrescreve o ID anterior. `ja_acordadas_na_escrita` tira a marca inteira com `pop`, inclusive no caminho de estoque. O callback também faz `pop` da chave inteira. A marca registra **qual evento um save anterior emitiu** e depois serve de prova de **que o ato atual faz parte da mesma escrita**. Falta identidade do ato, da conexão ou do savepoint nessa prova. `app/chamados/models.py:3647–3669,4824–4839`. |
| `EventoCelula.processado_em` | O insert nasce com o campo nulo. O consumidor grava a baixa quando conclui e a retenção apaga linhas antigas. A cura lê `pk` e `processado_em IS NULL`. A fila viva lê também `tentativas < MAX_TENTATIVAS_EVENTO`. Assim, **sem baixa** e **apto a ser consumido** são condições distintas. A cura usa a primeira para decidir a supressão. `app/ponto/portas/celula.py:990–991`, `app/escala/models.py:1871–1877`, `app/chamados/reconciliador.py:1389–1404,1546–1563`, `app/chamados/models.py:3667–3669`. |
| `PerguntaDisputa.CAMPOS_QUE_ACORDAM` | O modelo declara o conjunto imutável. O receiver lê sua interseção com `update_fields`. A lista cobre resposta, validação, carimbo, dia e vínculos, com nomes de FK e seus `_id`. O receiver testa **campos nomeados pelo save**. A outra constante homônima pertence à porta da célula e cobre `veredito` e `ata`. `app/chamados/models.py:3595–3608`, `app/chamados/signals.py:255–260`, `app/ponto/portas/celula.py:975–1002`. |

### 3. PROCEDENTE para vazamento de estado, semântica e lacuna do RED

- **PROCEDENTE — estado em instância.** O rollback descarta callbacks da transação ou do savepoint, mas a marca foi escrita diretamente no objeto. O `pop` do `registrar` também ocorre antes do fim do atomic e sobrevive se esse `registrar` for desfeito. Se duas threads compartilham o objeto, leem e alteram a mesma marca, sem uma chave de conexão. Compartilhamento em produção [nao verificado]. `app/chamados/services/trava_do_colaborador.py:16–23`, `app/chamados/models.py:3650–3669,4807,4824–4839`.
- **PROCEDENTE — dois acionadores e uma derivação frágil.** Signal e `registrar` podem encomendar o mesmo evento. A cura usa a presença de uma linha sem baixa para inferir que ela pertence à escrita atual. Já `evento(unica=True)` procura o registro pelos atributos do próprio ato sob a serialização do chamador. `app/chamados/signals.py:261–265`, `app/chamados/models.py:3662–3669,4835–4839`, `app/core/observ.py:104–106,138–153`.
- **NAO PROCEDE — segundo juiz registrado, segundo insert fora da porta, relógio novo ou inversão de trava introduzida pelo diff.** C6 e C21 continuam nas autoridades registradas. O insert segue em `emitir_evento_celula`, e a sequência segue no `bulk_create` do `registrar`, depois da conferência do dono. A operação nova é um `SELECT` simples do recibo depois do insert, sem tomada explícita de trava nem leitura de relógio. `app/core/juizes.py:298,313`, `app/chamados/models.py:3632–3635,3667–3669,4807–4818`, `app/ponto/portas/celula.py:981–991`.
- **PROCEDENTE — lacuna do RED.** A resposta pública testa o ato e a quantidade de eventos. A autoria do evento exigida pelo aval fica sem asserção. `app/chamados/tests/test_e4c_f5_c4_red.py:634–646`.

### 4. PROCEDENTE — há duplicatas e há um caminho para zero

Contagem para a mesma célula, com os atos comuns aceitos e as travas exigidas satisfeitas. `app/chamados/models.py:3623–3636,4796–4818,4835–4839`.

| Contraexemplo | Resultado |
|---|---|
| Save desfeito por savepoint antes do `registrar` | **1 sobrevivente**, do `registrar`. O ID do evento revertido fica na memória, mas a consulta deixa de encontrá-lo. Se o save antecede o savepoint e sobrevive, fica **1**, o original. O caso testado desfaz o save. `app/chamados/models.py:3662–3669`, `app/chamados/tests/test_e4c_f5_c4_red.py:696–703`. |
| Dois `registrar` na mesma instância | Após um save elegível, **2 no total**. O signal emite, o primeiro `registrar` consome a marca e o segundo emite. Se o primeiro `registrar` for desfeito e repetido, a marca consumida continua ausente. A repetição também deixa **2** eventos, embora só um `registrar` sobreviva. `app/chamados/signals.py:261–265`, `app/chamados/models.py:3662–3669,4807,4824–4839`. |
| Instância recém-carregada do banco | **2**, mesmo no mesmo atomic. O recibo está no objeto salvo. Passar só `pergunta_id` também deixa o `registrar` sem acesso à marca. `app/chamados/models.py:3653–3656,4804,4824–4839`. |
| Save chamado fora de atomic externo | **1** do save avulso. O próprio `PerguntaDisputa.save` abre atomic e o commit executa a limpeza. Um `registrar` válido posterior dá **2 no total**. Se o helper for chamado genuinamente fora de atomic, retorna antes de agendar callback. `app/chamados/models.py:3650–3656,3891–3907,4813–4814`. |
| `on_commit` anterior à limpeza, reutilizando a mesma instância | **0 eventos novos para a decisão nova**, se o evento anterior ainda estiver sem baixa. Agende antes do save avulso um callback que, após o commit, abra outra transação, pegue a trava e chame `registrar` com o mesmo objeto. Esse callback pode consumir o recibo antes da limpeza agendada pelo save. A consulta encontra o evento da transação anterior e silencia a nova emissão. É um contraexemplo de código. Ocorrência em produção [nao verificado]. `app/chamados/models.py:3653–3669,4807–4818,4824–4839`, `app/chamados/services/trava_do_colaborador.py:58–68,76–84`. |
| Evento do recibo já processado | **2 inserts no total**, com **1 novo pendente**. A baixa invalida o recibo para fins de supressão. `app/chamados/models.py:3667–3669`, `app/chamados/tests/test_e4c_f5_c4_red.py:713–719`. |
| `bulk`/`update` sem `post_save` | **0** se a escrita vier sozinha, inclusive se ela alterar um campo que deveria acordar. Com `registrar` comum, **1** pela porta. Com `registrar` e emissão explícita, **2** na interseção, como na reabertura. `app/chamados/signals.py:254–265`, `app/chamados/models.py:3709–3711,4835–4839`, `app/chamados/services/disputa_emissao.py:1680–1682,1695–1716`. |

### 5. NAO PROCEDE para mutante vazio. PROCEDENTE para cobertura insuficiente do aval

O harness exige um caso executado, sem skip ou erro, com a falha na asserção de negócio nomeada, e controle verde antes e depois. Conferi as quatro reescritas dos seis mutantes na fonte. Cada âncora ocorre uma vez e a fonte resultante é sintaticamente válida. Resultado em execução e logs do recibo [nao verificado]. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:27–67,230–238,787–831`.

| Mutante | O que deve fazer falhar |
|---|---|
| M47 | Reemite na resposta pública. O teste espera **1**, o mutante produz **2**. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:787–789,809–811`, `app/chamados/tests/test_e4c_f5_c4_red.py:634–646`. |
| M48 | Reemite na célula já coberta do lote. **(1, 1) → (2, 1)**. O teste salva só `p1`. Duas perguntas salvas na mesma célula antes do lote já produziriam dois inserts pelo signal. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:812–815`, `app/chamados/tests/test_e4c_f5_c4_red.py:686–694`, `app/chamados/signals.py:261–265`. |
| M49 | Aceita o ID revertido como prova. **1 → 0** sobreviventes. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:792–794,816–819`, `app/chamados/tests/test_e4c_f5_c4_red.py:696–703`. |
| M50 | Aceita o ID já processado. **2 → 1** inserts. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:792–794,820–823`, `app/chamados/tests/test_e4c_f5_c4_red.py:713–719`. |
| M51 | Retira o callback de limpeza. **2 → 1**. O teste executa os callbacks capturados dentro de `TestCase`, simulando a limpeza. Um commit real e a ordem entre callbacks ficam sem cobertura. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:797–800,824–827`, `app/chamados/tests/test_e4c_f5_c4_red.py:705–711`, `app/chamados/tests/cortes_f2_f9_apoio.py:58`. |
| M52 | Cala **todo** `post_save` que passa `recibo`. O save avulso cai de **1 → 0**. Isso testa o silêncio indiscriminado. A coordenação prevista no aval, em que só o save já assumido pelo `registrar` fica calado, permanece sem teste de autoria. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:803–806,828–831`, `app/chamados/tests/test_e4c_f5_c4_red.py:674–684`. |

### 6. NAO PROCEDE — os fakes preservam a prova existente de H2

O `save` falso já só anotava gravações. O fake novo devolve um conjunto vazio de recibos, e os dois emissores falsos passam a aceitar kwargs. H2 continua verificando a gravação e a chamada do outbox instrumentado, a propagação de `outbox indisponivel` e o rollback simulado da gravação. Sua cobertura é o caminho do `registrar` sem recibo. `app/chamados/tests/test_e4_expand.py:34–50,69–89,396–431`.

O selo antigo **M-H2-evento**, por sua vez, falha na âncora antes de executar o teste. Conferi **zero ocorrências** da âncora em `8ea46953e`, `8529b6003` e `59600163c`. O mesmo vale para **M-C11-reversao**. `app/chamados/tests/e4_verificar.py:21–24,33–37,58–64`.

## 27/11 12:21 · sessao 936455fd919d · FERIADO-BOTAO-CORTADO · construtor · origem=Code

A c3 atualiza o smoke de clique e guarda o RED de geometria do painel em um módulo fora da descoberta padrão do runner. [Smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:217) · [RED do painel](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/red_painel_popover_feriado.py:1)

### Asserções por cenário

Os quatro cenários — página em `base.html`, página em `base_app.html`, drawer em `base.html` e drawer em `base_app.html` — executam as mesmas checagens no feriado de 20/11. [Fixture](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:217) · [Matriz de cenários](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:252)

- O `summary` nasce fechado, tem área positiva dentro da célula e da viewport, e o hit no centro retorna o próprio `summary` ou um filho. O clique no alvo abre o formulário. [Sonda de abertura](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:90)
- O centro do submit atinge o próprio botão, que está habilitado e pertence ao formulário. O `action` bate com `reverse('ponto:feriado_vira_trabalho')` e o método é `POST`. [Submit](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:121) · [Contrato do formulário](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:234)
- A região mensal contém `08:00`, `12:00`, `13:00` e `17:00`, nessa ordem. A sonda acrescenta espaço sob as três primeiras marcações após medir o submit, para exigir deslocamento. No fim da rolagem, a marcação de `17:00` fica visível e o `summary` continua visível e atingível pelo centro. [Rolagem](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:136)

### Mutantes e comandos para o Code

| `SMOKE_FERIADO_MUTANTE` | Asserção que deve cair |
|---|---|
| `gatilho-cortado` | `gatilhoVisivel`, pelo deslocamento para fora da célula. [Smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:39) |
| `summary-display-none` | `gatilhoVisivel`, pela caixa sem área. [Smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:41) |
| `porta-trocada` | `action`, pelo caminho `/porta-mutante/`. [Smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:238) |
| `painel-recortado` | `painelSemRecorte`, no RED guardado. [RED](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/red_painel_popover_feriado.py:10) |
| `painel-distante` | `painelEncostado`, no RED guardado. [RED](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/red_painel_popover_feriado.py:22) |

```bash
rodar() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test \
    --settings=config.settings.ci --noinput "$1"
}
smoke=core.tests.test_smoke_clique_feriado_vira_trabalho
red=core.tests.red_painel_popover_feriado

SMOKE_FERIADO_MUTANTE='' rodar "$smoke"
SMOKE_FERIADO_MUTANTE=gatilho-cortado rodar "$smoke"
SMOKE_FERIADO_MUTANTE=summary-display-none rodar "$smoke"
SMOKE_FERIADO_MUTANTE=porta-trocada rodar "$smoke"

SMOKE_FERIADO_MUTANTE='' rodar "$red"
SMOKE_FERIADO_MUTANTE=painel-recortado rodar "$red"
SMOKE_FERIADO_MUTANTE=painel-distante rodar "$red"
```

### Diff

```bash
git --no-optional-locks diff 9bcb7447e -- core/tests/test_smoke_clique_feriado_vira_trabalho.py
git --no-optional-locks diff --no-index -- /dev/null core/tests/red_painel_popover_feriado.py
```

Conferi a sintaxe Python e JavaScript, o padrão de descoberta e os caminhos de falha com DOM simulado. A execução do smoke, do RED e dos mutantes em Chromium, assim como o calendário de produção, fica [nao verificado].

## 27/11 12:15 · sessao b30fa6c0a058 · FERIADO-UM-JUIZ · censo · origem=Code

# FERIADO-UM-JUIZ · Dossiê antes do construtor

Leitura estática da cópia. Hash Git, execução dos testes, impacto de frota e situação da competência 11 em produção [nao verificado]. [nao verificado]

## 1. Mapa

- **Hoje.** O fechamento monta o motor por fatia de vínculo com o calendário do colaborador. `eh_dia_trabalho(...) is False` manda o período para `periodos_ft`, com o dia da jornada no mapa. O fechamento filtra `_ft_certa`, procura `_ft_feriado` **dentro** desse grupo via `feriado_vira_folga` e põe o restante sem escala certa em `horas_trabalhadas` sem 100%. A partição é por identidade do período. `app/ponto/services/fechamento.py:668`, `:744`, `:953`, `:970`, `:1004` · `app/ponto/motor_calculo_v2.py:2560`, `:2573`, `:2605`.
- **Juiz.** `_feriado_dobra_no_dia` lê `feriado_em_dobra` e `eh_feriado` no dia de início da jornada. `get_motor` resolve o padrão por `tipo_ciclo`, com `12x36` em `CICLOS_FERIADO_SIMPLES`. Os quatro ramos de dobra usam o mesmo método. `app/ponto/motor_calculo_v2.py:941`, `:1000`, `:2925`, `:3208`, `:3385`, `:3467`, `:3740`, `:3810`.
- **Como perguntar.** Usar o dia de `dia_pago._dia_de(p, resultado.dia_da_jornada)`, os minutos apurados e o contexto **do motor da fatia daquele período**. `dobra_de_feriado_do_dia` já delega ao método do motor e devolve minutos. Hoje seu contexto permite fallback nacional, enquanto o calendário injetado no motor responde sozinho. A adaptação precisa preservar essa diferença ou consultar o próprio motor da fatia. `feriado_em_dobra_do_ciclo` expõe o padrão do ciclo, e `cct_do_motor` lê a configuração efetiva. O composto expõe o mapa de dias e só o último motor. Os totais de HE do resultado leem `periodos`, e o campo bruto do FT pode vir preenchido no comercial e vazio no partido. `app/ponto/calculador/regras.py:297`, `:312`, `:323`, `:358` · `app/ponto/calculador/nucleo.py:319` · `app/ponto/services/dia_pago.py:109` · `app/ponto/services/fechamento.py:331` · `app/ponto/motor_calculo_v2.py:328`, `:1000`, `:2993` · `app/ponto/tests/test_o280_bateria_da_lei.py:1810`.

## 2. Censo

- **Mensal.** `recalcular_fechamento_mes` deriva os seis campos. A promoção soma a HE a `horas_extras_100_feriado`, `horas_extras_100` e `horas_extras`, leva o subconjunto a `horas_extras_100_noturna` e retira o período de `horas_folga_trabalhada` ou `horas_trabalhadas`, conforme a origem. A porta persiste por `save` ou `update` do aprovado. `restaurar_fechamento` grava valores fornecidos para ajuste ou reversão. `app/ponto/services/fechamento.py:922`, `:939`, `:978`, `:1004`, `:1168`, `:1184`, `:1382`.
- **Lavra.** `DiaPago.motor` recebe as três listas prontas. `lavrar` soma as rubricas e grava `detalhe.folga_feriado_min` ou `detalhe.folga_sem_escala_min`. **O derivador paralelo é o oráculo:** `rubricas_por_dia` consulta a escala certa, `do_dia` decide com `folga_e_feriado and _certa`, e `lavrar(rubricas=...)` pode sobrescrever os valores. Mensal e oráculo precisam consumir o mesmo juízo de dobra e a mesma classificação. `app/ponto/services/fechamento.py:1225`, `:1289` · `app/ponto/services/dia_pago.py:234`, `:244`, `:259`, `:322`, `:359` · `app/ponto/calculador/alimentacao.py:520` · `app/ponto/calculador/regras.py:674`, `:694` · `CLAUDE.md:20`.
- **Leitores.** TXT e homologação leem os eventos do mensal. `_suprime_feriado` zera a parcela de 12x36 sem cláusula CCT. `hora_extra_feriado_diurna` usa o código cadastrado, separado de `folga_trabalhada_diurna`. HE100 noturna é subconjunto, sem linha própria. Conferir também o crivo `conta_vazia` quando o mês só tem horas promovidas. Outros leitores são ranking HE, cartão/resumo, telas mensais, Inteligência e a comparação de soma do dia com o mensal. `app/folha/export.py:420`, `:439`, `:739`, `:840`, `:911`, `:993`, `:1011` · `app/folha/services/ranking_he.py:33`, `:139`, `:224` · `app/relatorios/cartao_pela_celula.py:174` · `app/templates/ponto/_fech_linha.html:30` · `app/relatorios/views.py:98` · `app/inteligencia/views.py:118` · `app/folha/porta_export.py:457`.
- **Dia na tela, PDF e app.** `pago_do_dia` soma trabalhadas, folga e `folga_feriado_min`. O PDF usa `pago_h` para realizado. A faixa de rubricas da tela usa `rubricas_do_dia`, cujo `trabalhadas` lê só o campo homônimo e cujo `extras_100` lê o total. App e testemunha de export também leem a lavra. `app/ponto/services/dia_pago.py:897`, `:914` · `app/ponto/services/espelho.py:829` · `app/templates/ponto/espelho.html:275` · `app/relatorios/pdf_espelho.py:901`, `:984` · `app/api/views.py:1537` · `app/folha/porta_export.py:120`.

## 3. Leis e textos a atualizar

- **L-086:** corrigir o enunciado ainda abrangente. O rodapé já registra a revogação da cláusula de feriado em 27/09 03:xx. Domingo previsto segue a regra própria. Nos cortes, registrar o alcance novo sobre FT e remeter o CLASSE3 de 19/09 e o piso de 27/09 05:xx à **folga comum**. BO-B(3) mantém a rubrica de feriado no caso que nomeia. ESCALA-CERTA-DETERMINISTICA mantém a fonte por competência. BUG-FERIADO-DOIS-ESCRITORES mantém a partição disjunta. `app/docs/LEIS.md:118` · `app/docs/TICKETS.md:1134` · `app/ponto/services/fechamento.py:991` · `app/docs/PROMPTS.md:1370`, `:1418`, `:1424`.
- **Guardas herdadas:** L-085 fixa o início da jornada. O corte de 17/11 exclui o intermitente. A janela L-097 já pode zerar um par de folga antes do fechamento, com tratamento próprio de escala certa e autorização. A promoção recebe os minutos que sobreviveram a essa etapa. `app/docs/LEIS.md:117` · `app/docs/PROMPTS.md:2536` · `app/ponto/motor_calculo_v2.py:1562`, `:1569`, `:1585`, `:3694`.

## 4. Contrato e matriz RED

Unidade **colaborador × período FT**, com minutos pagáveis apurados, dia da jornada, motor da fatia, calendário daquele vínculo e resposta de escala certa para a rubrica. Validar mensal, `DiaPago.motor`, `DiaPago.oraculo` e evento TXT. `app/ponto/services/fechamento.py:299`, `:953`, `:1225`, `:1295` · `app/core/feriados.py:135` · `app/folha/export.py:439`.

| Caso | Esperado |
|---|---|
| 6x1, escala certa, vínculo folga por causa do feriado | Já promove toda a duração a HE100 feriado. Manter uma única rubrica. `app/ponto/services/fechamento.py:958`, `:970`, `:978` |
| 6x1, LIMBO / A2 / A8, feriado | Hora comum hoje. RED exige HE100 feriado pela duração positiva, sem repetir o período em trabalhadas ou FT. `app/escala/services/escala_certa.py:19` · `app/ponto/services/fechamento.py:958`, `:1004` |
| 12x36 de piso, certa e sem escala certa | HE100 **feriado** zero nas duas. A folga real certa tem 100% **de folga** hoje. A sem escala certa fica em trabalhadas. Há exceção CCT explícita no código, fora da mudança. `app/ponto/tests/test_o280_bateria_da_lei.py:1385` · `app/ponto/services/fechamento.py:989`, `:1004` · `app/core/regua_cct.py:473` |
| Intermitente em feriado | Trabalho convocado segue pelo caminho comum, fora de FT e da dobra automática. `app/ponto/motor_calculo_v2.py:2474`, `:2560`, `:3694` |
| Folga comum sem escala certa | Hora em trabalhadas, zero HE100, com noturno e intra quando devidos. `app/ponto/tests/test_dia_pago_soma.py:245` |
| FT noturno promovido | HE100 noturna acompanha a promoção. Cobrir também o partido, cujo FT não traz o split pronto, usando `he_noturna` com a régua e os intervalos do motor. `app/ponto/motor_calculo_v2.py:409`, `:1051`, `:2993`, `:3208` · `app/ponto/tests/test_o280_bateria_da_lei.py:1720` |
| Véspera cruza para feriado | Zero dobra de feriado, inclusive no segundo par da mesma jornada. `app/docs/LEIS.md:117` · `app/ponto/tests/test_o280_bateria_da_lei.py:1564`, `:1580` |

## 5. Testes existentes [nao verificado]

O teste de FT em feriado sem escala certa que já passa pela porta usa **12x36** e só exige pagamento e paridade entre lavras. Preservar esse controle e criar o gêmeo **6x1** com valores exatos. A asserção a inverter na porta do dinheiro é `HE100 feriado = 0; trabalhadas = duração` para `HE100 feriado = duração; trabalhadas = 0`, isolando o período. F3b fixa zero **no agregado do motor** e 480 no campo cru do FT. Esse selo fica válido com a promoção no fechamento. A bateria L-114 deve reunir F3/F4, F10/F10e, F11c/F11d, BO-B de contagem única e S6 intermitente. `app/ponto/tests/test_he100_dia_sem_previsto.py:40`, `:227` · `app/ponto/services/fechamento.py:958`, `:1004` · `app/ponto/tests/test_o280_bateria_da_lei.py:1385`, `:1479`, `:1580`, `:1720`, `:1775`, `:1810` · `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:127` · `app/ponto/tests/test_s6_intermitente_feriado.py:133` · `app/docs/LEIS.md:154`.

## 6. Padrão

`padrao=single source of truth` / **um juiz por pergunta**. A partição alimenta as projeções mensal e diária. A lavra principal já tem uma porta de escrita compartilhada. `app/docs/GLOSSARIO.md:11` · `app/docs/PLANO-PADROES.md:53`, `:61` · `app/ponto/services/fechamento.py:1225` · `app/ponto/services/dia_pago.py:359`.

## 7. Arquivos e pouso [nao verificado]

Superfície proposta: `fechamento.py`, `calculador/regras.py`, `calculador/alimentacao.py`, testes e textos do item 3. `dia_pago.py` já aceita a partição. `folha/export.py` entra como leitor a certificar, evitando a interseção informada com E4C. PAR-PELO-TURNO cruza pelo contrato de período e dia da jornada em `ponto/turnos.py`. Interseção exata entre árvores em voo [nao verificado]. `app/ponto/services/fechamento.py:1225`, `:1295` · `app/ponto/services/dia_pago.py:135` · `app/docs/RELATO.md:1057`, `:13678`.

Para o pouso, bateria verde e DIFF antes do apply de 11, com reversão e recorte por empresa pelo K6. O recálculo genérico pode gravar exportada fora da guarda cadastrada, inclusive por evento. A exclusão de exportadas exigida neste aval precisa valer também nesse caminho. `app/docs/LEIS.md:125`, `:142`, `:154`, `:156` · `app/folha/export.py:565` · `app/ponto/services/fechamento.py:559`, `:580`, `:1834`.

## 8. Pergunta de lei

1. **6x1 com escala certa, folga semanal que coincide com feriado e `feriado_vira_folga=False`** — o juiz confirma a dobra. A rubrica continua `folga_trabalhada_diurna` ou passa a `hora_extra_feriado_diurna`? O BO-B(3) nomeia expressamente o vínculo que folga por causa do feriado, e a partição atual conserva FT quando esse predicado é falso. `app/docs/PROMPTS.md:1370` · `app/escala/models.py:1384`, `:1400` · `app/ponto/services/fechamento.py:970`, `:989`.

## 27/11 12:14 · sessao bf3f4123005e · FERIADO-BOTAO-CORTADO · verificador · origem=Code

**FERIADO-BOTAO-CORTADO — PRONTO não demonstrado.** O predicado foi preservado. O teste novo tem mordida estrutural, mas a visibilidade com quatro batidas no navegador, a regressão visual da rolagem e o smoke do Ronald no calendário seguem [nao verificado]. `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/colaboradores/tests/test_feriado_botao_cortado.py:71`

**1. Predicado — descartada alteração.** Antes, `_calendario_grade.html:131` no blob `896fad490` do anexo. Depois, `app/templates/colaboradores/partials/_calendario_grade.html:96` no blob `019b86999` da cópia. A expressão é literalmente `pode_editar_dia and dia.feriado_vira_trabalho_oferecido` nos dois. Os três blobs novos conferem com o anexo, que contém dois templates e o teste. A oferta continua vindo de feriado do colaborador com célula existente e `trabalha is False`. O POST consulta o mesmo campo. `app/colaboradores/services/calendario.py:791` `app/ponto/views.py:3607` `app/ponto/views.py:3624`

**2. Botão com 0..N batidas — descartado bloqueio no HTML por quantidade. Procedente a lacuna de prova visual.**

- A ação é renderizada antes do bloco de batidas e retratadas. Vale para zero ou qualquer quantidade, dada a oferta. O rótulo efetivo é **“Tornar dia de trabalho”**. O perfil usa `mes` em `base.html` e o diagnóstico usa `fita`. As duas cascas incluem o drawer genérico, que injeta o parcial por `fetch`. Isso comprova o caminho compartilhado do drawer, com visibilidade nas duas cascas [nao verificado]. `app/templates/colaboradores/partials/_calendario_grade.html:95` `app/templates/colaboradores/partials/_calendario_grade.html:130` `app/templates/ponto/partials/_feriado_vira_trabalho.html:6` `app/templates/colaboradores/detalhe.html:1` `app/templates/colaboradores/detalhe.html:215` `app/templates/colaboradores/calendario_partial.html:67` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/templates/base.html:399` `app/templates/base_app.html:154` `app/templates/core/_drawer_generico.html:19`
- **Mutante que morde.** Voltar a ação para depois das marcações, mantendo o resto, derruba o `assertLess` nos dois layouts. Retirar `overflow-y:auto` derruba a asserção de `mes`. **Mutante que escapa.** Acrescentar `display:none` ao `<summary>` preserva todas as asserções do teste e esconde o gatilho. O teste injeta 0 e 4 batidas e renderiza o include isolado nos dois layouts. A execução do RED e do verde ficou [nao verificado], pois a importação local parou em `django.template`. `app/colaboradores/tests/test_feriado_botao_cortado.py:4` `app/colaboradores/tests/test_feriado_botao_cortado.py:35` `app/colaboradores/tests/test_feriado_botao_cortado.py:92` `app/colaboradores/tests/test_feriado_botao_cortado.py:109`

**3. Escritores e leitores — descartada mudança de contrato.**

| Variável | Quem escreve ou passa | Quem lê |
|---|---|---|
| `layout`, `dias`, `dia`, `cal_offset`, `resumo` | `contexto_calendario` entrega os dados. Perfil passa `mes`, drawer passa `fita` e `cal.dias`/`cal.resumo`, Furos passa só `dias`/`resumo`. | A grade escolhe o layout, itera `dias` como `dia` e usa o offset e os totais. `app/colaboradores/services/calendario.py:838` `app/templates/colaboradores/calendario_partial.html:67` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/templates/relatorios/furos.html:42` `app/templates/colaboradores/partials/_calendario_grade.html:65` `app/templates/colaboradores/partials/_calendario_grade.html:155` |
| `pode_editar_dia`, `dia.feriado_vira_trabalho_oferecido` | View do perfil e `contexto_drawer` consultam `tem_acao(..., 'editar_escalas')`. `contexto_calendario` escreve a oferta do dia. | A grade lê ambos para o gatilho e o CSS. O POST consulta a mesma permissão e lê a oferta no contexto do dia. `app/colaboradores/views.py:2007` `app/colaboradores/services/drawer.py:159` `app/colaboradores/services/calendario.py:791` `app/templates/colaboradores/partials/_calendario_grade.html:68` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/ponto/views.py:3607` `app/ponto/views.py:3624` |
| `colaborador.pk`, `dia.data`, `volta_url` → `colab_pk`, `data`, `volta` | O serviço publica colaborador e data. O drawer passa `colab` como `colaborador`. Perfil e drawer montam a URL de volta. A grade passa os três aliases. | A grade usa a data também no `aria-label`. O parcial escreve `colab`, `dia` e `volta` no formulário, e a view os lê. `app/colaboradores/services/calendario.py:755` `app/colaboradores/services/calendario.py:839` `app/colaboradores/views.py:2009` `app/colaboradores/services/drawer.py:162` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/templates/colaboradores/partials/_calendario_grade.html:97` `app/templates/ponto/partials/_feriado_vira_trabalho.html:9` `app/ponto/views.py:3614` |
| Campos da célula envolvidos pelo novo `<div>` | `contexto_calendario` publica status, palavra, ausência, feriado, batidas, retratadas, badges e `resolver_dia_oferecido`. Perfil e drawer fornecem `pode_veredito` via `regularizar_batida`. | A grade lê esses valores nos ícones, horários, ações de veredito e badges, agora dentro do mesmo bloco. `app/colaboradores/services/calendario.py:754` `app/colaboradores/services/calendario.py:811` `app/colaboradores/views.py:1992` `app/colaboradores/services/drawer.py:133` `app/colaboradores/services/drawer.py:157` `app/templates/colaboradores/partials/_calendario_grade.html:101` `app/templates/colaboradores/partials/_calendario_grade.html:130` `app/templates/colaboradores/partials/_calendario_grade.html:137` |

**4. Classes de reprovação — descartados derivação paralela, duplo sentido e juiz duplicado.** As três leituras da condição controlam o gatilho e dois estilos. A oferta continua sendo um fato do dia, e a permissão continua sendo do usuário. O juiz registrado `_TT11` segue respondendo só por “Resolver dia”. O cadastro da oferta de feriado no registro de juízes já está apontado em fila. **Selo sem mordida estrutural descartado. Lacuna de mordida visual procedente**, como mostra o mutante `display:none`. `app/docs/LEIS-UI.md:23` `app/templates/colaboradores/partials/_calendario_grade.html:68` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/core/juizes.py:745` `app/core/juizes.py:759` `app/docs/BACKLOG.md:2038` `app/colaboradores/tests/test_feriado_botao_cortado.py:95`

**5. Outros includes — descartada abertura da ação em Furos.** O `only` passa `dias` e `resumo`. Sem `pode_editar_dia` e `layout`, Furos usa a grade padrão e recebe o `<div>` extra sem os estilos de rolagem. O drawer recebe a ação antes das batidas e o rótulo com quebra de linha. A fita continua com rolagem horizontal. O efeito visual nos dois consumidores segue [nao verificado]. `app/templates/relatorios/furos.html:42` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/templates/colaboradores/partials/_calendario_grade.html:65` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/templates/ponto/partials/_feriado_vira_trabalho.html:5`

**6. Rolagem separada — procedente a lacuna de verificação.** No mensal elegível, o botão fica fora da região rolável de uma célula de `132px`. A região tem `tabindex="0"` e engloba também ícones, chamado, “Resolver dia” e badges. O selo confere declarações de CSS e a estrutura do HTML. Alcance da última marcação, permanência do gatilho visível enquanto se rola e smoke do Ronald no calendário [nao verificado]. `app/templates/colaboradores/partials/_calendario_grade.html:68` `app/templates/colaboradores/partials/_calendario_grade.html:95` `app/templates/colaboradores/partials/_calendario_grade.html:101` `app/templates/colaboradores/partials/_calendario_grade.html:137` `app/colaboradores/tests/test_feriado_botao_cortado.py:99`

## 27/11 12:12 · sessao d4acb9eb4d2e · FERIADO-UM-JUIZ · sonda · origem=Code

## FERIADO-UM-JUIZ — competência 11/2026

**Confere: 75 dias de 42 colaboradores. Todos os 75 saíram como hora comum. Pela regra proposta, 67 dias de 35 colaboradores passariam a HE100 feriado, somando 449,74 h.** `extrato:3–78`

As referências `extrato:N` apontam para o dado raspado da mensagem, contando `sonda_leitura` como linha 1. A apuração usa a saída fornecida. Reexecução na sombra e conferência independente com a lista do admin [nao verificado].

### (a) e (b) — Universo e classificação atual

| Empresa | Dias | Colaboradores | Dias com `he100fer=0` e `sem_escala_h > 0` | Horas comuns | Fonte |
|---|---:|---:|---:|---:|---|
| 2 | 65 | 37 | 65 | 459,75 h | `extrato:3–67` |
| 3 | 10 | 5 | 10 | 74,20 h | `extrato:68–77` |
| **Total** | **75** | **42** | **75** | **533,95 h** | `extrato:3–78` |

**Nenhum dia foge do padrão.** Em todas as linhas `DIA`, `ht = sem_escala_h`, `he100 = he100fer = folga = 0` e `folga_feriado_min = None`. No código atual, a seleção para HE100 feriado parte de `_ft_certa`. Os períodos sem escala certa entram em `horas_trabalhadas` tanto no fechamento quanto na lavra diária. `extrato:3–77` · [fechamento.py:958](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:958) · [fechamento.py:970](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:970) · [fechamento.py:1004](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1004) · [dia_pago.py:259](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/dia_pago.py:259)

### (c) — Impacto da regra proposta

| Empresa | Dias que mudariam | Colaboradores afetados | Horas que passariam a HE100 feriado | Dias que ficam | `12x36` | `intermitente` | Ciclo `None` | Fonte |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| 2 | 57 | 30 | 375,54 h | 8 | 5 | 3 | 0 | `extrato:3–67` |
| 3 | 10 | 5 | 74,20 h | 0 | 0 | 0 | 0 | `extrato:68–77` |
| **Total** | **67** | **35** | **449,74 h** | **8** | **5** | **3** | **0** | `extrato:3–77` |

A projeção aplica `dobra_do_ciclo=True` e `ciclo != intermitente` e soma o `sem_escala_h` exibido, já arredondado por dia. Os 67 dias elegíveis são todos `6x1`. O total calculado a partir dos minutos originais e o novo fechamento mensal ficam [nao verificado]. `extrato:3–77` · [regras.py:312](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/calculador/regras.py:312) · [motor_calculo_v2.py:3740](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:3740)

Os únicos IDs sem dias que mudariam são da empresa 2. Em `12x36`, ficam **366, 415, 788, 1031 e 1035**, com um dia cada. Em `intermitente`, ficam **830** com dois dias e **975** com um dia, embora o extrato traga `dobra_do_ciclo=True` para os três dias. `extrato:14,20,29,36–37,52,64–65`

### (d) — FECH atual por colaborador

Valores mensais em horas. Os IDs **727** e **788** já têm `he100fer` positivo no mês, e os dias sondados desses dois IDs têm `he100fer=0`. `extrato:24,29,90,93`

| Empresa | ID | `ht` | `he100` | `he100fer` | `folga` | Fonte |
|---|---:|---:|---:|---:|---:|---|
| 2 | 43 | 184,06 | 0,00 | 0,00 | 0,00 | `extrato:79` |
| 2 | 203 | 182,34 | 0,00 | 0,00 | 0,00 | `extrato:80` |
| 2 | 206 | 170,16 | 0,00 | 0,00 | 0,00 | `extrato:81` |
| 2 | 207 | 169,23 | 0,00 | 0,00 | 0,00 | `extrato:82` |
| 2 | 278 | 178,12 | 0,00 | 0,00 | 0,00 | `extrato:83` |
| 2 | 366 | 65,85 | 0,00 | 0,00 | 0,00 | `extrato:84` |
| 2 | 369 | 179,55 | 0,00 | 0,00 | 0,00 | `extrato:85` |
| 2 | 384 | 192,04 | 0,00 | 0,00 | 0,00 | `extrato:86` |
| 2 | 415 | 179,90 | 0,00 | 0,00 | 0,00 | `extrato:87` |
| 2 | 583 | 153,08 | 0,00 | 0,00 | 0,00 | `extrato:88` |
| 2 | 622 | 143,48 | 0,00 | 0,00 | 0,00 | `extrato:89` |
| 2 | 727 | 167,77 | 7,05 | 7,05 | 0,00 | `extrato:90` |
| 2 | 735 | 164,36 | 0,00 | 0,00 | 0,00 | `extrato:91` |
| 2 | 784 | 173,02 | 0,00 | 0,00 | 0,00 | `extrato:92` |
| 2 | 788 | 153,65 | 14,23 | 14,23 | 0,00 | `extrato:93` |
| 2 | 789 | 161,32 | 0,00 | 0,00 | 0,00 | `extrato:94` |
| 2 | 822 | 170,30 | 0,00 | 0,00 | 0,00 | `extrato:95` |
| 2 | 830 | 117,08 | 0,00 | 0,00 | 0,00 | `extrato:96` |
| 2 | 899 | 169,79 | 0,00 | 0,00 | 0,00 | `extrato:97` |
| 2 | 903 | 184,17 | 0,00 | 0,00 | 0,00 | `extrato:98` |
| 2 | 924 | 120,57 | 0,00 | 0,00 | 0,00 | `extrato:99` |
| 2 | 944 | 158,02 | 0,00 | 0,00 | 0,00 | `extrato:100` |
| 2 | 961 | 184,11 | 0,00 | 0,00 | 0,00 | `extrato:101` |
| 2 | 962 | 179,47 | 0,00 | 0,00 | 0,00 | `extrato:102` |
| 2 | 967 | 81,98 | 0,00 | 0,00 | 0,00 | `extrato:103` |
| 2 | 968 | 172,80 | 0,00 | 0,00 | 0,00 | `extrato:104` |
| 2 | 975 | 32,87 | 0,00 | 0,00 | 0,00 | `extrato:105` |
| 2 | 988 | 213,94 | 3,04 | 0,00 | 0,00 | `extrato:106` |
| 2 | 990 | 179,52 | 0,00 | 0,00 | 0,00 | `extrato:107` |
| 2 | 991 | 167,69 | 0,00 | 0,00 | 0,00 | `extrato:108` |
| 2 | 1000 | 145,13 | 0,00 | 0,00 | 0,00 | `extrato:109` |
| 2 | 1023 | 62,94 | 0,00 | 0,00 | 0,00 | `extrato:110` |
| 2 | 1026 | 95,85 | 0,00 | 0,00 | 0,00 | `extrato:111` |
| 2 | 1031 | 71,52 | 0,00 | 0,00 | 0,00 | `extrato:112` |
| 2 | 1035 | 67,94 | 0,00 | 0,00 | 0,00 | `extrato:113` |
| 2 | 1036 | 73,09 | 0,00 | 0,00 | 0,00 | `extrato:114` |
| 2 | 1050 | 8,32 | 0,00 | 0,00 | 0,00 | `extrato:115` |
| 3 | 82 | 196,97 | 0,00 | 0,00 | 0,00 | `extrato:116` |
| 3 | 114 | 163,38 | 0,00 | 0,00 | 0,00 | `extrato:117` |
| 3 | 749 | 141,63 | 0,00 | 0,00 | 0,00 | `extrato:118` |
| 3 | 911 | 180,50 | 0,00 | 0,00 | 0,00 | `extrato:119` |
| 3 | 1024 | 103,97 | 0,00 | 0,00 | 0,00 | `extrato:120` |

## 27/11 12:08 · sessao 4f65f40651ed · FERIADO-BOTAO-CORTADO · leitura · origem=Code

**a. As quatro falhas têm a mesma causa:** o JS mede o gatilho na viewport, mas aplica `left/top` em um painel `fixed` cujo ancestral muda a referência dessas coordenadas. (`popover-acao.js:38,44,75`)

- **Página, nas duas cascas:** gatilho em `(788,97; 580)`, painel em `(802; 445)`. O painel cabe inteiro e **sobrepõe** o gatilho. A distância registrada é **0 px** e a sonda rejeita a sobreposição. O `backdrop-filter` de `.hx-shell` muda a referência do painel. (`smoke_falhas.txt:41,97`, `test_smoke_clique_feriado_vira_trabalho.py:119`, `base.html:335`, `base_app.html:93`, `hasner-ponto.css:283`)
- **Drawer, nas duas cascas:** gatilho em `(890,28; 576)`, painel em `(1156; 243)`. A distância é **197,82 px** e a direita do painel ultrapassa a viewport em **20 px**, o que também derruba o teste de recorte. O `translateX(0)` mantém o drawer como referência do painel mesmo depois de aberto. (`smoke_falhas.txt:74,130`, `_drawer_generico.html:3,15,16`) [nao verificado]

**b. O defeito de posicionamento antecede o c1.** O c1 mudou a posição e a quebra de linha do gatilho e separou a rolagem das marcações. A geometria pré-c1 em execução é [não verificado]. Os outros usos são **Resolver dia** e **Inverter tipo/Retratar batida**, na grade do perfil, no Espelho do drawer e na fita do diagnóstico. Eles têm o mesmo risco sob esses ancestrais. Ocorrência em produção [não verificado]. (`diff_c1.patch`, `_feriado_vira_trabalho.html:5`, `_calendario_grade.html:127,135`, `detalhe.html:215`, `_drawer_generico.html:140`, `_drawer_diagnostico.html:35`)

O smoke de **Excluir feriado** passa pelo `hxConfirm` do cadastro e verifica POST, exclusão e auditoria. Ele não mede proximidade. (`feriado_form.html:52`, `test_smoke_clique_excluir_feriado.py:54,110`)

**c. Cabem duas curas, com o mesmo predicado.**

- **Template/CSS, só no feriado:** botão `popovertarget` e formulário `popover` na camada superior, com âncora CSS e inversão de posição. Preserva os outros popovers. Exige adaptar a sonda do `summary`. Compatibilidade nos navegadores usados [não verificado]. (`_calendario_grade.html:131`, `_feriado_vira_trabalho.html:5`, `test_smoke_clique_feriado_vira_trabalho.py:228`)
- **JS:** compensar a origem do painel medida em `left:0; top:0`. É a menor mudança para os deslocamentos observados. O painel continua sujeito ao recorte do ancestral, e uma mudança compartilhada alcança os demais popovers. (`popover-acao.js:44,53,75`, `_drawer_generico.html:3`)

**d. Sim, 16 px é razoável para a abertura normal.** O JS prevê **2 px** abaixo ou acima do gatilho, com ajuste horizontal. O fallback que cola o painel no topo pode fugir desse limite. (`popover-acao.js:55,64,70`)

## 27/11 12:07 · sessao 914f194adfe1 · ORFAS-VOLTOU · sonda · origem=Code

- **Defeito.** A sonda mostrou dois `reg10`, das rubricas `0025` e `0243`, com `00000VIGIA` no campo `matric`. O layout exige que os dez caracteres desse campo passem em `isdigit()`. As letras explicam os dois erros `reg10 campo matric nao-numerico`. [app/folha/export.py:272](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:272)
- **Escritor e fonte.** `montar_linhas_export` lê `fech.colaborador.codigo_dominio` na linha 737. O `strip().zfill(10)` preenche com zeros e preserva as letras. No mapa exemplificado na cópia, `0025` é adicional noturno e `0243` é intrajornada, ambos emitidos pela chamada a `montar_linha_10` na linha 747. O serializador grava a matrícula na linha 45. O mapa ativo da empresa 2 na sondagem é [nao verificado]. [app/folha/export.py:737](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:737) [app/folha/export.py:747](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:747) [app/folha/export.py:45](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:45) [app/folha/tests/test_sm1_monster_export.py:75](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/tests/test_sm1_monster_export.py:75)
- **Dono pela L-099: CADASTRO.** O valor inválido vem do campo cadastral `Colaborador.codigo_dominio`. `classificar_export` só verifica se esse campo está vazio, então a exigência de matrícula numérica é aplicada na validação do TXT. `dono_da_divergencia` julga divergências de dia por colaborador e fica fora desse caminho. Quem gravou o valor é [nao verificado]. [CLAUDE.md:183](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/CLAUDE.md:183) [app/colaboradores/models.py:311](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/colaboradores/models.py:311) [app/folha/export.py:881](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:881) [app/ponto/management/commands/e6_oraculo.py:53](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/e6_oraculo.py:53)
- **TXT real de 11/2026, empresa 2.** Com o `sem_medicao` visto na sombra, o clique solicita a conferência e a porta segura a geração até ficar `pronta`. Mesmo com a porta pronta, basta que a montagem real reproduza uma dessas linhas para `gerar_dominio_txt` recusar por falha de layout, antes de entregar ou registrar o arquivo. A porta recalcula os fechamentos antes de montar. O estado da porta real hoje, as linhas que esse recálculo produziria e o resultado efetivo da tentativa ficam [nao verificado]. [app/folha/porta_export.py:817](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:817) [app/folha/porta_export.py:1423](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:1423) [app/folha/views.py:135](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:135) [app/folha/views.py:214](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:214) [app/folha/views.py:275](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:275) [app/folha/views.py:282](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:282)

## 27/11 12:03 · sessao 22ad6ec9cbe8 · ORFAS-VOLTOU · sonda · origem=Code

Na montagem em memória do TXT de **11/2026**, a sonda identificou **87 células com o colaborador fora**, **5 com o colaborador dentro e a data sem `reg11`** e **337 que acionaram `furo_espelho`**. A presença no TXT ficou **[nao verificado]** para as 561 células da empresa 2. `sonda.py`, saída fornecida, linhas 4–7.

| Empresa | Células | Colab. fora do TXT | Colab. dentro, data sem `reg11` na janela | Células que retêm | Efeito direto em rubrica/horas | Fonte |
|---|---:|---:|---:|---:|---:|---|
| 2 | 561 | [nao verificado] | [nao verificado] | 286 | 0 | `sonda.py`, saída linha 4 |
| 3 | 91 | 87 | 4 | 51 | 0 | `sonda.py`, saída linha 5 |
| 4 | 1 | 0 | 1 | 0 | 0 | `sonda.py`, saída linha 6 |
| **Total** | **653** | **87 verificados** | **5 verificados** | **337** | **0** | `sonda.py`, saída linha 7 |

**As somas fecham nas 653 células**, com zero fora das empresas 2–4. Por colaborador, são 5 dentro + 87 fora + 561 com presença indeterminada. Por data, são 137 fora da janela + 0 com `reg11` verificado + 5 sem `reg11` com colaborador dentro + 68 sem `reg11` com colaborador fora + 443 datas indeterminadas na janela. As 337 retenções se sobrepõem a essas contagens. A sonda encontrou 1.895 perguntas, 189 colaboradores no filtro, 179 com célula e 12 perguntas sem FK de célula. A referência do anexo traz 184 colaboradores e 658 células. A identidade entre os dois conjuntos de células é **[nao verificado]**. `sonda.py`, saída linhas 1–8. [app/folha/export.py:977](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:977)

A saída trouxe `layout_invalido` na empresa 2 e `sem_medicao` nas três empresas. A retenção vem da classificação, que exclui o colaborador inteiro da montagem quando o motivo é `furo_espelho`. O erro específico de layout da empresa 2 e a emissão efetiva do TXT são **[nao verificado]**. `sonda.py`, saída linhas 4–6. [app/folha/export.py:266](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:266) [app/folha/export.py:725](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:725) [app/folha/export.py:977](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:977) [app/folha/porta_export.py:1368](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:1368)

O **zero direto** vale para fechar apenas a pergunta, mantendo os insumos da folha. O `reg11` registra data de falta. A contribuição de um dia sem `reg11` às rubricas mensais e o efeito de materializar uma resposta ou decidir o dia são **[nao verificado]**. [app/folha/export.py:215](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:215) [app/folha/export.py:734](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:734) [app/chamados/services/validacao.py:777](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/validacao.py:777)

## 27/11 12:02 · sessao aac47beae035 · FERIADO-BOTAO-CORTADO · construtor · origem=Code

Adicionei um único arquivo, [test_smoke_clique_feriado_vira_trabalho.py](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:1). A fixture usa datas fixas e confere que a função real oferece a ação em 20/11/2026, com a célula em folga e quatro batidas. [Fixture e conferência](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:144)

- **Página × `base.html` e `base_app.html`** — renderiza o calendário mensal real no conteúdo de cada casca. [Renderização](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:209)
- **Drawer × `base.html` e `base_app.html`** — abre o drawer genérico e clica em Espelho, com as respostas das duas views fornecidas em memória. [Abertura do drawer](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:60)

Nos quatro cenários, o Chromium confere o `summary` dentro da célula e da viewport, clica no elemento atingido no centro do botão e verifica abertura, painel inteiro na viewport, distância de até 16 px e `action` do formulário igual à porta real. A rolagem só registra deslocamento, evento e fechamento. [Medições](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:89)

O **RED esperado no drawer** vem de `popover-acao.js:44` e `:75–76`. O script aplica `position:fixed` e grava coordenadas da viewport, mas o `transform` do drawer muda a referência do `fixed`. O `backdrop-filter` de `.hx-shell` também merece leitura nas duas páginas. O efeito geométrico em ambos os casos segue [nao verificado]. [Posicionador](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:38) [Drawer](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_drawer_generico.html:16) [CSS da casca](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/css/hasner-ponto.css:283)

**Comandos para o Code**, a partir de `app/`. Os mutantes alteram o HTML do ensaio. [Mutantes CSS](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:38) [Mutante da porta](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:192)

```bash
alvo=core.tests.test_smoke_clique_feriado_vira_trabalho
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
SMOKE_FERIADO_MUTANTE=gatilho-cortado PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
SMOKE_FERIADO_MUTANTE=painel-recortado PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
SMOKE_FERIADO_MUTANTE=painel-distante PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
SMOKE_FERIADO_MUTANTE=porta-trocada PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
```

A sintaxe Python passou. A execução na cópia parou no import de `django.core`, antes do banco de teste e do Chromium. Resultado dos quatro cenários, rolagem, mutantes, envio do POST e comportamento em produção seguem [nao verificado]. [Import do Django](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/manage.py:11) [nao verificado]

_8667 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
