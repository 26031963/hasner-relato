# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 02:22** · publicado em **07/11/2026 02:23** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 02:22 · sessao 8827481b08b7 · - · - · origem=Ronald (tmux)

Achados concretos:

1. **Há novo valor em dia lavrado que não é feriado em dobra.** Caso: COM, d0=06/10 folga e não feriado; batidas d0 13–23, d1 03–05; d1=07/10 feriado, vínculo folga no feriado e escala certa. O intervalo de 4h produz cauda que herda d0 pela regra ≤6h; ela é FT de d0 (`app/ponto/motor_calculo_v2.py:2403`, `:2407`, `:2448`, `:2468`). Entretanto `MotorComercial.calcular_periodo` julga a cauda pelo dia civil d1 e escreve HE100 noturna=120 (`:3264`, `:3268`, `:3294`). O fechamento seleciona `ft_feriado` pelo mesmo civil d1 (`app/ponto/services/fechamento.py:935`, `:947`, `:949`), e as linhas novas passam a acrescentar **2h** no mensal e no DiaPago de d0 (`:962`; `app/ponto/services/dia_pago.py:245`, `:252`). Baseline: a seleção errada já existia, mas o acréscimo de HE100 noturna era zero; volta6: 2h. Medido em memória com AST do `calcular_periodo`, helpers e prefixo da lavra; não foi execução integral ORM. (`app/ponto/motor_calculo_v2.py:3268`; `app/ponto/services/dia_pago.py:252`.)

2. **Promoção FT do partido continua sem HE100 noturna.** `MotorTurnoPartido.calcular_periodo` escreve trabalho e noturno, devolvendo sem split (`app/ponto/motor_calculo_v2.py:2893`, `:2896`, `:2897`). O cálculo diário percorre apenas `resultado.periodos`; FT recebe somente mapa de dia (`:2928`, `:2944`, `:2951`, `:2955`, `:3083`). Caso NPART 21–00 em feriado no qual o vínculo folga: 180 trabalhados, 120 noturnos; a promoção paga 3h HE100 feriado e lê **0h** HE100 noturna, onde a regra exige **2h** (`app/ponto/services/fechamento.py:954`, `:964`; `app/ponto/services/dia_pago.py:250`, `:252`). A retirada do escritor do partido explica a lacuna; o leitor novo não a resolve (`app/ponto/motor_calculo_v2.py:2897`, `:2955`).

3. **O split novo usa par bruto e pode pagar noturno inexistente no trabalhado efetivo.** Caso COM feriado 08–12/13–23, janela corta saída às22, HE extra declarada de300 conforme fixture B-SPLIT retirada: efetivo08–22, trabalho780, noturno0; a dobra grava HE100780 e chama split sobre bruto08–23, produzindo **HE100 noturna60**, violando `noturnas/fator=0` (`app/ponto/motor_calculo_v2.py:3224`, `:3238`, `:3260`, `:3357`, `:3361`, `:1015`). Medido por AST das funções reais: `he_noturna(08–23, HE100780, intervalo12–13)=60`; `minutos_noturnos(08–22)=0` (`:440`, `:441`).

4. **Recorte longo continua contando noturno duas vezes e agora a dobra diária escreve esse resultado.** `minutos_noturnos` estende o primeiro segmento até `fim` e soma também22–24 (`app/ponto/motor_calculo_v2.py:111`, `:112`, `:116`). Medido: feriado04–23, toda duração HE1001140 → split **1200**, regra1140; feriado04–01(+1), HE1001260 → split **1440**, regra1260 (`:440`, `:441`, `:448`). Os loops novos chamam esse split (`:3104`, `:3361`). O noturno principal também permanece duplicado; igualdade contra o próprio campo não certifica o minuto contado uma vez (`:983`, `:986`).

5. **Os totais HE100/HE100 feriado são conservados nos caminhos modificados por dia**, pois o novo loop distribui `minutos_trabalhados` de cada período e o mensal soma os campos; não há cálculo novo de duração (`app/ponto/motor_calculo_v2.py:3100`, `:3102`, `:3357`, `:3359`, `:329`, `:332`). A promoção mantém as atribuições anteriores de total e adiciona apenas noturna (`app/ponto/services/fechamento.py:955`, `:956`, `:962`; `app/ponto/services/dia_pago.py:249`, `:252`). O 12x36 permanece com dobra por período seguida de split (`app/ponto/motor_calculo_v2.py:2809`, `:2855`).

Nenhuma execução integral de banco/fechamento, produção, DIFF de frota ou validação do hash foi feita. [não verificado]

## 07/11 02:21 · sessao 4c69df77b36c · - · - · origem=Ronald (tmux)

Achado concreto no leitor de alertas: A6 diz “HE idêntica em 3+ dias”, mas conta períodos, sem agrupar nem deduplicar datas. Redistribuir a dobra cria falso positivo: três períodos do mesmo feriado com 180 min cada produzem `A6=[d,d,d]`; antes `[0,0,540]` não produzia A6. Executei em memória o trecho real extraído por AST: resultados `{}` antes e três datas iguais depois. Exemplo completo pelo pareador [não verificado]. Fontes: `app/ponto/services/esmeril_espelho.py:20`, `:174-180`; contador mostrado como ocorrências em `:199`; novos escritores `app/ponto/motor_calculo_v2.py:3097-3104` e `:3354-3361`.

Mapa dos campos:

| Campo | Escritores | Leitores |
|---|---|---|
| `minutos_extra`, `_100`, `_100_feriado` | 12x36 `motor_calculo_v2.py:2810`; partido reset `:2933`, dobra `:3100`, excedente `:3153`; comercial período `:3269`, reset `:3332`, dobra `:3357`, excedente `:3416` | Soma mensal `:325-344`; DiaPago `dia_pago.py:180-184`; calculador `alimentacao.py:102-106`; espelho `espelho.py:112-114`; invariante export `porta_export.py:542`; alerta A6 `esmeril_espelho.py:176` |
| `_50_noturna`, `_100_noturna` | Casca canônica `motor_calculo_v2.py:1014-1022`; resets partido `:2937-2938`, comercial `:3336-3337` | Soma mensal `:340-344`; DiaPago `dia_pago.py:181`, `:184`, `:252` |
| `FechamentoMensal.horas_extras_100_noturna` | `fechamento.py:924`, promoção `:962-964`; persistência `:1169` | Comparação com DiaPago pela porta `porta_export.py:388`; TXT não emite esse subconjunto separadamente `folha/export.py:424-447` |
| `DiaPago.horas_extras_100_noturna` | `dia_pago.py:184`, promoção `:252`; eventual substituição pelo calculador `:322-330` | Campo incluído no universo lavrado `:36`; presença de dinheiro `:863` |

Os leitores de soma aceitam múltiplos períodos: `ResultadoMes` soma todos em `motor_calculo_v2.py:325-344`; a alimentação entrega todas as pontas em `calculador/alimentacao.py:102-106`; `calculador/regras.py:827-835` chama `he_noturna` por ponta e soma. Não há exigência de último período nesses trechos. O produtor alternativo v3 ainda grava a dobra inteira no último em `calculador/periodo.py:841-844`; isso é um escritor próprio preexistente, não uma suposição desses leitores.

Mensal e DiaPago recebem a mesma partição `ft_feriado`, disjunta dos outros grupos em `fechamento.py:975-981`, e ambos somam o mesmo campo do período: `fechamento.py:962-964` e `dia_pago.py:244-253`. A igualdade numérica em produção [não verificado].

Há outra divergência de contrato já existente: motor mede o split no par **cru**, `motor_calculo_v2.py:1015`; calculador recebe o par **efetivo**, `calculador/alimentacao.py:83`, `:106`, e o mede em `regras.py:832`. As novas chamadas passam a expor esse descompasso nos feriados curados. Contraprova com janela real depende do restante da auditoria [não verificado].

Tela, calendário e PDF leem totais lavrados e não somam os subconjuntos novamente: `templates/ponto/espelho.html:279-283`; `colaboradores/services/calendario.py:651-656`; `relatorios/pdf_espelho.py:837-859`; `dia_pago.py:914-928`. TXT mantém HE100 total/feriado e adicional noturno, sem nova linha para HE noturna: `folha/export.py:420-447`.

O invariante da porta ainda agrupa períodos pela data civil da entrada, `porta_export.py:538-544`, enquanto a lavratura usa o mapa da jornada em `dia_pago.py:174`. Redistribuir HE entre pares que cruzam a meia-noite muda os agrupamentos desse contador; falso positivo concreto dele [não verificado].

## 07/11 02:21 · sessao 2195e8a35400 · - · - · origem=Ronald (tmux)

- Referência órfã: `_dia_do_F10h` diz “batidas do F10f”, mas esse teste foi retirado; corrigir o comentário em `app/ponto/tests/test_o280_bateria_da_lei.py:1580`. Ausência do símbolo confirmada por busca; demais órfãos [não verificado].
- Censo coerente: contrato restaurado para **10** ajudantes em `app/ponto/tests/test_contract_juiz_batida.py:326`; AST contou exatamente 10 em `MotorBase`. MATRIZ preserva apenas F11/F11b/F11c/F11d/F11r e F10h/F10i, todos exercidos por comparações de valores (`app/ponto/tests/test_o280_bateria_da_lei.py:210`, `:212`, `:1272`, `:1287`).
- RED estático da base: F11 **0→120 min**, F11b **0→60 min**, F11c mensal/DiaPago **0→1 h**, F11d **0→360 min**; o diff fornecido mostra os ramos anteriores sem split, e as expectativas estão em `app/ponto/tests/test_o280_bateria_da_lei.py:1670`, `:1708`, `:1748`, `:1779`. Execução na base [não verificado].
- F11d “Era 240” descreve a primeira cura, que chamou split só no último período; **19aeede1 tinha zero**, pois dava `continue` sem split. Convém explicitar a versão em `app/ponto/tests/test_o280_bateria_da_lei.py:1776`.
- F11r é referência preexistente **300 min**, não RED do item (`app/ponto/tests/test_o280_bateria_da_lei.py:1676`, `:1692`).
- F10h gêmeo fixa **570 HE100 feriado / 390 noturna**, incluindo cauda zero (`app/ponto/tests/test_o280_bateria_da_lei.py:1602`, `:1606`). O pino compara valores, mas `expectedFailure` aceita outras falhas; também exige **660 HE100 feriado**, mudança de pertença retirada do escopo (`:1611`, `:1624`). O gêmeo exercita a mesma fixture, portanto o conjunto não é vazio (`:1601`, `:1623`).
- F10i exige **90/102,86**, comportamento preservado e guarda de outro escopo; não é RED do item (`app/ponto/tests/test_o280_bateria_da_lei.py:1643`).
- Os testes de mutantes longos recuperaram a mordida anterior, coerente com a retirada do teto em `minutos_noturnos`: `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:365`, `:368`, `:376`; regra ainda sem teto em `app/ponto/motor_calculo_v2.py:110`. Suite/banco não executados [não verificado].

## 07/11 01:07 · sessao 9e4c27bca95a · FIO-E0-V4-E1 · verificador · origem=Code

**Não considero a cura integralmente fechada.** Restam contraexemplos de reabertura repetida, domínio reconstruído antes da porta canônica e ordem de travas Chamado ↔ Colaborador. A preservação do legado também depende de não reutilizar a instância após rollback. (`app/chamados/services/disputa_emissao.py:1485`; `app/chamados/services/disputa_emissao.py:404`; `app/chamados/services/fio.py:766`; `app/chamados/services/disputa_emissao.py:1342`)

1. Para cada **PROCEDENTE-CURADO**, minha conferência é:

| Achado da triagem | Veredito sobre a cura |
|---|---|
| Perda do legado no primeiro uso | **Fecha a sobrescrita sequencial normal:** captura antes de alterar e registra junto da resposta nova. **Não fecha retry da mesma instância após rollback**, explicado no item 2. (`app/chamados/services/disputa_emissao.py:1341`; `app/chamados/services/disputa_emissao.py:1362`) |
| Reabertura sem decisão | **Fecha irmã muda absorvida e validação sem texto.** Porém `desfazer_absorcao=True` torna o predicado incondicional e permite outra linha numa segunda chamada sobre pergunta já pendente. (`app/chamados/services/disputa_emissao.py:1475`; `app/chamados/services/disputa_emissao.py:1485`) |
| Ressuscitação apaga projeções | **Fecha a perda das projeções selecionadas:** registra antes de descarimbar, no mesmo `atomic`. Não adiciona uma decisão própria de ressuscitação; preserva respostas/validações preexistentes. (`app/chamados/models.py:1512`; `app/chamados/models.py:1513`; `app/chamados/models.py:1515`) |
| Absorção não atômica | **Fecha o par carimbo/ABSORCAO de cada irmã:** ambos estão dentro do mesmo `atomic`. A função inteira continua contendo trabalho anterior e várias unidades por irmã. (`app/chamados/services/materializacao.py:521`; `app/chamados/services/materializacao.py:532`; `app/chamados/services/materializacao.py:557`) |
| Autoria descartada | **Fecha os dois chamadores apontados:** correção pelo colaborador transmite autor/motivo; devolução da ausência transmite usuário sistema/motivo. (`app/chamados/models.py:2165`; `app/chamados/services/materializacao.py:673`) |
| Ciclo Disputa ↔ Colaborador | **Fecha os caminhos alterados; não estabelece ordem global.** Resta Chamado ↔ Colaborador, detalhado no item 4. (`app/chamados/services/disputa_emissao.py:507`; `app/chamados/services/disputa_emissao.py:945`; `app/chamados/services/disputa_emissao.py:1208`; `app/chamados/services/fio.py:766`) |
| DRY agrupa NULL que o índice não iguala | **Fecha a apresentação enganosa:** a colisão sai explicitamente como `SEM-DIA(o indice nao ve)`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:116`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:117`) |
| DRY quebra com dia nulo | **Fecha esse caminho:** não chama competência para `dia=None`, usa balde próprio e trata a ordenação/impressão. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:51`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:94`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:95`) |
| DRY mede qualquer linha, não correspondência | **Fecha a divergência de predicado:** usa o mesmo helper do escritor. Herda, porém, suas limitações de significado e correspondência. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:45`; `app/chamados/models.py:3412`) |
| Lote da declaração deriva domínio paralelo | **Fecha esse lote:** ele chama a autoridade comum. Outros filtros manuais permanecem, inclusive antes do nascimento. (`app/chamados/services/declaracao_texto.py:547`; `app/chamados/services/disputa_emissao.py:388`; `app/chamados/services/fabrica_da_celula.py:118`) |
| RED não cobre primeira sobrescrita legada | **Fecha a lacuna específica do cenário:** monta projeção sem decisões e exige o texto antigo no histórico após sobrescrever. Execução RED/GREEN: **[não verificado]**. (`app/chamados/tests/test_e1_volta2.py:43`; `app/chamados/tests/test_e1_volta2.py:58`; `app/chamados/tests/test_e1_volta2.py:66`) |
| Inventário/máscara quebrados | **Cura estática coerente:** nome/campos acompanham o rename; escrita deixa de usar o queryset que recusa `bulk_update`. Execução na sombra: **[não verificado]**. (`bin/sombra_regras_pessoais.py:139`; `bin/sombra_mascara_pessoal.py:62`; `app/chamados/models.py:3320`) |
| Conversa reutilizada como fato | **Fecha na porta canônica**, que agora exclui conversa. Não fecha leitores anteriores à porta que ainda montam sua própria coleção. (`app/chamados/models.py:2466`; `app/chamados/services/disputa_emissao.py:404`; `app/chamados/services/fabrica_da_celula.py:118`) |
| Unidade do teto | **Comentário corrigido:** custo por chamada a `_nascer`, inclusive reuso. O delta quantitativo alegado continua **[não verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:172`; `app/ponto/tests/test_selo_chokepoint_escrita.py:175`) |

2. Os escritores dos quatro campos lidos pelo helper são:

| Campo | Escritores persistentes identificados |
|---|---|
| `resposta_colab` | Resposta humana: `app/chamados/services/disputa_emissao.py:1342`; automática: `app/chamados/services/disputa_emissao.py:1509`; limpeza na reabertura: `app/chamados/services/disputa_emissao.py:1459`, aplicada em `app/chamados/services/disputa_emissao.py:1487` ou `app/chamados/services/disputa_emissao.py:1489`. |
| `respondida_em` | Humana: `app/chamados/services/disputa_emissao.py:1343`; automática: `app/chamados/services/disputa_emissao.py:1510`; recuperação da trilha: `app/chamados/services/disputa_emissao.py:1531`; carimbo: `app/chamados/models.py:2691`; limpeza na reabertura: `app/chamados/services/disputa_emissao.py:1459`; ressuscitação: `app/chamados/models.py:1515`. |
| `validada_em` | Validação: `app/chamados/services/validacao.py:134`; devolução: `app/chamados/services/validacao.py:199`; resposta nova: `app/chamados/services/disputa_emissao.py:1355`; carimbo: `app/chamados/models.py:2694`; reabertura: `app/chamados/services/disputa_emissao.py:1459`; ressuscitação: `app/chamados/models.py:1515`. |
| `validada_por` | Validação: `app/chamados/services/validacao.py:135`; devolução: `app/chamados/services/validacao.py:200`; resposta nova: `app/chamados/services/disputa_emissao.py:1356`; carimbo: `app/chamados/models.py:2695`; reabertura: `app/chamados/services/disputa_emissao.py:1459`; ressuscitação: `app/chamados/models.py:1515`. |

Há atribuições somente em memória na simulação da ressuscitação e na restauração após tentativa recusada de carimbo; elas não devem ser contadas como outro escritor persistente. (`app/chamados/models.py:1462`; `app/chamados/models.py:1463`; `app/chamados/models.py:2722`; `app/chamados/models.py:2724`)

Os leitores identificados que afetam o contrato são:

| Campo | Leitores e usos |
|---|---|
| `resposta_colab` | Helper: `app/chamados/models.py:3418`, `app/chamados/models.py:3421`; vitalidade: `app/chamados/juizes.py:1114`, `app/chamados/juizes.py:1152`, `app/chamados/juizes.py:1259`; fechamento: `app/chamados/models.py:1797`; carimbo: `app/chamados/models.py:2689`; display: `app/chamados/models.py:3006`; declaração: `app/chamados/services/declaracao_texto.py:377`; materialização: `app/chamados/services/materializacao.py:278`, `app/chamados/services/materializacao.py:400`; foto da reabertura: `app/chamados/services/disputa_emissao.py:1479`; guardas automática/trilha: `app/chamados/services/disputa_emissao.py:1504`, `app/chamados/services/disputa_emissao.py:1527`. |
| `respondida_em` | Helper: `app/chamados/models.py:3409`, `app/chamados/models.py:3416`; vitalidade: `app/chamados/juizes.py:1152`, `app/chamados/juizes.py:1259`; estado/propriedade: `app/chamados/models.py:2605`, `app/chamados/models.py:2983`; fechamento: `app/chamados/models.py:1796`; carimbo: `app/chamados/models.py:2690`; validação/recusa: `app/chamados/services/validacao.py:381`, `app/chamados/services/validacao.py:555`; reabertura/trilha: `app/chamados/services/disputa_emissao.py:1479`, `app/chamados/services/disputa_emissao.py:1527`; materialização: `app/chamados/services/materializacao.py:1361`; DRY: `app/chamados/management/commands/backfill_decisoes_pergunta.py:37`. |
| `validada_em` | Helper: `app/chamados/models.py:3409`, `app/chamados/models.py:3419`; vitalidade: `app/chamados/juizes.py:1137`; estado/SQL: `app/chamados/models.py:2601`, `app/chamados/models.py:2895`; coleção ancorada/fechamento: `app/chamados/models.py:331`, `app/chamados/models.py:1795`; carimbo: `app/chamados/models.py:2693`; resposta nova: `app/chamados/services/disputa_emissao.py:1345`; reabertura: `app/chamados/services/disputa_emissao.py:1480`; validação/devolução/recusa: `app/chamados/services/validacao.py:147`, `app/chamados/services/validacao.py:195`, `app/chamados/services/validacao.py:384`, `app/chamados/services/validacao.py:552`; declaração: `app/chamados/services/declaracao_texto.py:221`; materialização: `app/chamados/services/materializacao.py:1362`; reconciliador: `app/chamados/reconciliador.py:620`, `app/chamados/reconciliador.py:1039`; DRY: `app/chamados/management/commands/backfill_decisoes_pergunta.py:37`. |
| `validada_por` | Helper: `app/chamados/models.py:3421`; foto do carimbo: `app/chamados/models.py:2686`; foto da resposta/reabertura: `app/chamados/services/disputa_emissao.py:1345`, `app/chamados/services/disputa_emissao.py:1480`; foto da validação: `app/chamados/services/validacao.py:147`; devolução da ausência: `app/chamados/services/materializacao.py:667`; procedência da batida: `app/ponto/services/procedencia_batida.py:72`; painel: `app/chamados/services/painel.py:260`; contagem por autor: `app/chamados/services/verbos.py:240`, `app/chamados/services/verbos.py:244`; auditoria: `app/chamados/management/commands/auditar_invariantes_chamados.py:1074`. |

Também há consumidores fora dessas portas:

- APIs leem texto/instante, e a API de correção usa o carimbo como condição. (`app/api/views.py:1189`; `app/api/views.py:1191`; `app/api/views.py:1203`; `app/api/views.py:2596`; `app/api/views.py:2598`; `app/api/views_mensageria.py:567`; `app/api/views_mensageria.py:569`)
- Histórico lê dinamicamente os dois timestamps e o texto; métricas usam resposta/validação e autoria. (`app/chamados/services/historico.py:99`; `app/chamados/services/historico.py:101`; `app/chamados/services/historico.py:105`; `app/chamados/services/historico.py:109`; `app/chamados/services/verbos.py:311`; `app/chamados/services/painel.py:268`)
- Cobrança, avisos, becos, relatório, raio X e vigia usam `respondida_em`. (`app/chamados/services/cobranca.py:229`; `app/chamados/services/aviso_pendencia.py:13`; `app/chamados/services/becos.py:30`; `app/chamados/relatorios_disputa.py:21`; `app/chamados/services/raiox.py:136`; `app/chamados/services/vigia_fio.py:675`)
- Canal, explicador, calendário, ficha, prévia da folha, retrato do dia e cartório usam o texto. (`app/chamados/canal.py:65`; `app/chamados/services/explicador.py:79`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`; `app/ponto/services/dia_do_colab.py:70`; `app/ponto/services/cartorio.py:1590`)
- Lavra, suprajuiz e reconciliação de grade usam `respondida_em`; vigia do carimbo e flip usam `validada_em`. (`app/chamados/veredito_lavrado.py:167`; `app/ponto/supra_juiz.py:545`; `app/ponto/management/commands/reconciliar_grade.py:461`; `app/chamados/services/vigia_carimbo.py:98`; `app/ponto/services/flip_auto.py:130`)
- Apresentação lê projeção diretamente e também por `resposta_display`. (`app/templates/chamados/partials/_card_atendimento.html:100`; `app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:31`; `app/templates/chamados/partials/modal_fio.html:218`)

Para os campos produzidos, estes são os escritores da sequência, todos passando pelo INSERT de `registrar`:

| Origem | Campos fornecidos |
|---|---|
| Helper, resposta recuperada | `ato`, `ato_em`, `ato_texto`, `ato_motivo`; autor fica no default nulo. (`app/chamados/models.py:3417`; `app/chamados/models.py:3418`; `app/chamados/models.py:3359`) |
| Helper, validação recuperada | Os cinco, usando `validada_por_id`. (`app/chamados/models.py:3420`; `app/chamados/models.py:3421`; `app/chamados/models.py:3422`) |
| Resposta humana | Ato, instante, autor, texto; motivo fica no default vazio. (`app/chamados/services/disputa_emissao.py:1366`; `app/chamados/models.py:3361`) |
| Resposta automática / trilha | Ato, instante, texto, motivo; autor fica nulo. (`app/chamados/services/disputa_emissao.py:1513`; `app/chamados/services/disputa_emissao.py:1534`; `app/chamados/models.py:3359`) |
| Validação | Ato, instante, autor, texto; motivo vazio. (`app/chamados/services/validacao.py:139`; `app/chamados/models.py:3361`) |
| Devolução / reabertura / recusa | Os cinco campos. (`app/chamados/services/validacao.py:205`; `app/chamados/services/disputa_emissao.py:1476`) |
| Silêncio / retratação | Os cinco campos. (`app/chamados/models.py:1870`; `app/chamados/models.py:1990`) |
| Absorção | Ato, instante, autor, texto; motivo vazio. (`app/chamados/services/materializacao.py:560`; `app/chamados/models.py:3361`) |

`registrar` consome esses dicionários sem deduplicação; `ato/ato_em` alimentam correspondência e ordenação, `ato` dirige o histórico, e o template consome os cinco campos. A máscara reescreve texto/motivo na sombra. (`app/chamados/models.py:3428`; `app/chamados/models.py:3413`; `app/chamados/models.py:3374`; `app/chamados/models.py:2994`; `app/templates/chamados/partials/modal_fio.html:208`; `app/templates/chamados/partials/modal_fio.html:211`; `bin/sombra_regras_pessoais.py:139`)

**A correspondência não possui significado uniforme entre escritores:**

- Resposta humana e validação usam o instante de processamento do ato. (`app/chamados/services/disputa_emissao.py:1337`; `app/chamados/services/validacao.py:134`)
- Upload automático compartilha o `agora` obtido antes do lote. (`app/api/foto_recebida.py:42`; `app/api/foto_recebida.py:48`)
- Recuperação usa `criado_em` da primeira nota encontrada, não o instante da recuperação. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:34`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`)
- `carimbar` usa instante do carimbo, fornecido ou obtido internamente, para preencher resposta/validação ausentes. Essa porta não registra esses dois atos na sequência. (`app/chamados/models.py:2683`; `app/chamados/models.py:2691`; `app/chamados/models.py:2694`; `app/chamados/models.py:2711`)

O helper transforma todas essas origens em `resposta`/`validacao`, com motivo genérico. Sua igualdade compara apenas pergunta, ato e instante: **não verifica texto, autor nem natureza do evento**. (`app/chamados/models.py:3396`; `app/chamados/models.py:3412`; `app/chamados/models.py:3417`; `app/chamados/models.py:3420`)

Os contraexemplos são:

- **Duas consultas antes de inserir:** ambas calculam a mesma linha ausente; ambas podem inseri-la. O SELECT não reserva correspondência, `registrar` faz `bulk_create`, e não há UNIQUE dessa tripla. (`app/chamados/models.py:3412`; `app/chamados/models.py:3375`; `app/chamados/models.py:3428`)
- **Pergunta repetida no lote:** `[p, p]` produz duplicatas porque `ja` não é atualizado a cada `append`. Chamador atual passando esse lote: **[não verificado]**. (`app/chamados/models.py:3412`; `app/chamados/models.py:3415`; `app/chamados/models.py:3417`)
- **Mesmo instante, conteúdo diferente:** uma linha existente com a tripla impede preservar outra projeção com texto/autor diferente. Estado real assim em produção: **[não verificado]**. (`app/chamados/models.py:3413`; `app/chamados/models.py:3416`; `app/chamados/models.py:3419`)
- **Trilha 2x:** mesma instância ou releitura atualizada para na guarda; duas instâncias antigas com timestamp nulo podem passar e registrar duas vezes a mesma correspondência. (`app/chamados/services/disputa_emissao.py:1527`; `app/chamados/services/disputa_emissao.py:1531`; `app/chamados/services/disputa_emissao.py:1534`)
- **Automática 2x:** mesma instância atualizada para pelo texto; duas instâncias antigas não são serializadas por essa função. (`app/chamados/services/disputa_emissao.py:1504`; `app/chamados/services/disputa_emissao.py:1508`; `app/chamados/services/disputa_emissao.py:1513`)
- **Reabertura da absorção 2x:** queryset estável por PK registra outra reabertura mesmo após a primeira limpar tudo, devido ao `or desfazer_absorcao`. O chamador da ausência normalmente recompõe o universo pelo carimbo; isso protege sua repetição sequencial normal, mas não a porta com queryset estável nem duas capturas anteriores à limpeza. (`app/chamados/services/disputa_emissao.py:1485`; `app/chamados/services/materializacao.py:650`; `app/chamados/services/materializacao.py:662`; `app/chamados/services/materializacao.py:673`)
- **Rollback seguido de retry da mesma instância:** parta de legado `08:00/T0`; a tentativa altera `p` para `09:00/T1`; `registrar` levanta e o banco volta, mas não há restauração de `p`. Na repetição, o helper lê `09:00/T1` da memória e pode sobrescrever `08:00/T0` sem preservá-lo. Na automática, a repetição pode simplesmente retornar `False` pelo texto que só ficou em memória. Ocorrência real: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1341`; `app/chamados/services/disputa_emissao.py:1342`; `app/chamados/services/disputa_emissao.py:1362`; `app/chamados/services/disputa_emissao.py:1504`; `app/chamados/services/disputa_emissao.py:1509`; `app/chamados/models.py:3407`)

3. `do_fato`, `linhas_dos_fatos` e o lote **agora compartilham o predicado**: colaborador em coluna, pares exatos motivo/dia, instância calculada, conversa excluída e vias fora do fato excluídas. (`app/chamados/models.py:2445`; `app/chamados/models.py:2465`; `app/chamados/models.py:2466`; `app/chamados/models.py:2467`; `app/chamados/services/declaracao_texto.py:547`)

O DRY contém dois universos diferentes: `linhas_a_criar` recupera projeções respondidas/validadas, inclusive fora do domínio de identidade; `grupos_de_colisao` recompõe conversa/vias e agrupa a instância **armazenada**. Com instância divergente do catálogo, essa segunda consulta não equivale estritamente ao helper que exige instância calculada. Existência desse acervo: **[não verificado]**. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:37`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:45`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:73`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:74`; `app/chamados/models.py:2465`)

**Leitores manuais sobrevivem e afetam produção:**

- `gerar_perguntas_disputa` faz dois `exists()` por disputa/motivo/dia antes de chamar `abrir`. Remover um motivo mantendo outro na disputa, e depois recolocá-lo no mesmo dia, deixa a retratada satisfazer o `exists`; a nova pergunta é suprimida, embora `do_fato` exclua aquela linha. (`app/chamados/models.py:1987`; `app/chamados/models.py:1994`; `app/chamados/services/disputa_emissao.py:388`; `app/chamados/services/disputa_emissao.py:404`; `app/chamados/services/disputa_emissao.py:423`; `app/chamados/models.py:2467`)
- A fábrica monta `ja` excluindo somente duplicata: retratada, conversa ou instância divergente podem calar o motivo. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`)
- `_candidata_reusavel` e `_ja_perguntou` também recompõem a coleção por disputa/motivo/dia. (`app/chamados/services/declaracao_texto.py:236`; `app/chamados/services/disputa_emissao.py:1290`)

4. A porta comum, o agrupador e a âncora foram corrigidos localmente. `ChamadoColaborador.abrir` e a abertura de formulário também pedem Colaborador primeiro. (`app/chamados/services/disputa_emissao.py:507`; `app/chamados/services/disputa_emissao.py:945`; `app/chamados/services/disputa_emissao.py:1208`; `app/chamados/models.py:872`; `app/chamados/se

[truncado em 20000 de 30793 caracteres]

## 07/11 01:03 · sessao 8a23f7624596 · - · - · origem=Ronald (tmux)

Todos os locais pedidos são leitores de `PerguntaDisputa`, confirmados pela origem do queryset/relacionamento:

| Leitor | Campos e efeito |
|---|---|
| API de chamados | `resposta_colab` em `app/api/views.py:1189`; `respondida_em` em `:1191`; exige todas respondidas para solicitar correção em `:1203`. |
| API de disputa | Texto em `app/api/views.py:2596`; timestamp em `:2598`. |
| Fila de validação | `app/api/views_mensageria.py:567` lê texto; `:569` lê timestamp; `:570` calcula dias esperando. |
| Histórico | Queryset explícito de PD em `app/chamados/services/historico.py:96`; escolhe dinamicamente `respondida_em`/`validada_em` em `:99`/`:101`, lê via `getattr` em `:105` e texto em `:109`. |
| Métrica de cobrança respondida | `app/chamados/services/verbos.py:309` consulta PD; filtra/projeta `respondida_em` em `:311`/`:312`; compara com última cobrança em `:318`. |
| Calendário | `app/colaboradores/services/calendario.py:473` consulta PD; `:474` exclui texto vazio; `:476`/`:477` usa vitalidade para bloquear último recurso. |
| Ficha | `app/colaboradores/services/ficha.py:166` recebe perguntas do app; `:171` deriva “sem resposta” pelo texto. |
| Prévia da folha | `app/folha/previa.py:142` lê texto; `:143` classifica como “resposta do colaborador”. |
| Suprajuiz | `app/ponto/supra_juiz.py:543`/`:545` e `:547`/`:548` percorrem perguntas dos relacionamentos e vetam execução CSF quando `respondida_em` existe. |
| Retrato do dia | Queryset PD em `app/ponto/services/dia_do_colab.py:64`; serialização do texto em `:70`. |
| Cartório | `app/ponto/services/cartorio.py:1586`/`:1590` define presença de resposta pelo texto; recebe perguntas em `:1594` e veta reconciliação em `:1597`. |

Há consequência concreta para “campo com dois significados”: o histórico rotula qualquer `respondida_em` como ato de “o colaborador” e qualquer `validada_em` como ato de “o admin”, independentemente do escritor automático (`app/chamados/services/historico.py:99`, `:101`, `:113`, `:114`). Isso é incompatível com interpretar automaticamente todos esses timestamps como instantes de ação humana, pois `PerguntaDisputa.carimbar` também os escreve (`app/chamados/models.py:2691`, `:2694`). Não foi demonstrado caso real nessa cópia/dado [não verificado].

O inventário anterior de leitores era parcial: essa leitura encontrou inclusive acesso indireto por `getattr(p, campo)`, que uma busca apenas por atributos diretos perde (`app/chamados/services/historico.py:105`). Exaustividade absoluta de leitores indiretos/dinâmicos permanece [não verificado].

## 07/11 01:02 · sessao 8a23f7624596 · - · - · origem=Ronald (tmux)

1. Escritores dos campos da projeção, em produção/comandos:

| Campo | Quem escreve |
|---|---|
| `resposta_colab` | Resposta humana: `app/chamados/services/disputa_emissao.py:1342`; automática: mesmo arquivo `:1509`; limpeza na reabertura: `:1459`, aplicada por `:1487`/`:1489`. |
| `respondida_em` | Resposta humana: `disputa_emissao.py:1343`; automática: `:1510`; recuperação da trilha: `:1531`; carimbo automático condicionado a texto: `app/chamados/models.py:2691`; limpeza na reabertura: `disputa_emissao.py:1459`; ressuscitação: `models.py:1515`. |
| `validada_em` | Validação: `app/chamados/services/validacao.py:134`; devolução: `:199`; resposta nova desfaz: `disputa_emissao.py:1355`; carimbo automático: `models.py:2694`; reabertura: `disputa_emissao.py:1459`; ressuscitação: `models.py:1515`. |
| `validada_por` | Validação: `validacao.py:135`; devolução: `:200`; resposta nova desfaz: `disputa_emissao.py:1356`; carimbo automático: `models.py:2695`; reabertura: `disputa_emissao.py:1459`; ressuscitação: `models.py:1515`. |

Há também atribuições exclusivamente em memória: simulação da ressuscitação em `models.py:1462`/`:1463`; restauração da fotografia anterior à tentativa de `carimbar` em `models.py:2722`/`:2724`. Os defaults/campos ficam em `models.py:2256`/`:2258` e `:2913`.

O comando de acervo que recupera `respondida_em` delega a `carimbar_resposta_pela_trilha`, passando `nota.criado_em`: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`. Não encontrei outro escritor direto desses quatro campos nos arquivos Python de produção examinados; escritas dinâmicas não detectáveis por nomes literais permanecem [não verificado].

2. Leitores relevantes por campo, incluindo autoridades e consumidores que distinguem seu significado:

- `resposta_colab`: C6/juízes em `app/chamados/juizes.py:1114`, `:1152`, `:1259`; carimbo em `models.py:2689`; display em `models.py:3006`; materialização em `services/materializacao.py:278`, `:400`, `:562`; lote/declaração em `services/declaracao_texto.py:377`; fotografia de reabertura em `disputa_emissao.py:1479`; recuperação da trilha em `:1527`/`:1535`; helper em `models.py:3418`/`:3421`.
- `respondida_em`: C6 em `juizes.py:1152`/`:1259`; estado da pergunta em `models.py:2605`/`:2608` e propriedade `:2983`; entrada da validação em `validacao.py:381`; carimbo em `models.py:2690`; foto da reabertura em `disputa_emissao.py:1479`; guarda da recuperação em `:1527`; helper em `models.py:3409`, `:3416`, `:3417`; DRY em `management/commands/backfill_decisoes_pergunta.py:37`.
- `validada_em`: C6 em `juizes.py:1137`; estado em `models.py:2601`; SQL de vitalidade em `models.py:2895`; guarda da validação em `validacao.py:384`; devolução em `:195`; carimbo em `models.py:2693`; foto da reabertura em `disputa_emissao.py:1480`; helper em `models.py:3409`/`:3419`/`:3420`; DRY em `backfill_decisoes_pergunta.py:37`.
- `validada_por`/`_id`: foto da validação em `validacao.py:147`; foto antes de resposta nova em `disputa_emissao.py:1345`; foto de reabertura em `:1480`; carimbo em `models.py:2686`; helper em `models.py:3421`; procedência da batida em `app/ponto/services/procedencia_batida.py:72`; painel distingue usuário sistema em `services/painel.py:260`; verbos contabilizam autores staff em `services/verbos.py:240`/`:244`; auditoria distingue usuários sistema em `management/commands/auditar_invariantes_chamados.py:1074`.

Essa lista cobre os leitores relevantes ao contrato auditado; inventário integral de todos os leitores de apresentação/filtros, incluindo templates e leitores indiretos, não foi completamente consolidado [não verificado].

3. Campos produzidos e consumidores:

| Campo | Produção no helper | Consumo |
|---|---|---|
| `ato` | `models.py:3417`/`:3420` | `registrar`, `models.py:3428`; correspondência em `:3413`; leitor histórico em `:2994`/`:2998`; DRY em `backfill_decisoes_pergunta.py:61`; template em `app/templates/chamados/partials/modal_fio.html:208`. |
| `ato_em` | `models.py:3417`/`:3420` | `registrar :3428`; correspondência `:3413`; ordenação `:3374`; template `modal_fio.html:209`/`:211`. |
| `ato_por` | Só validação, via `ato_por_id`, `models.py:3421`; resposta recuperada não recebe autor e usa NULL (`:3359`). | `registrar :3428`; prefetch em `services/fio.py:232`; template `modal_fio.html:211`. |
| `ato_texto` | `models.py:3418`/`:3421` | `registrar :3428`; template `modal_fio.html:209`/`:211`; máscara em `bin/sombra_regras_pessoais.py:139`. |
| `ato_motivo` | `models.py:3418`/`:3422`, constante `:3396` | `registrar :3428`; template `modal_fio.html:211`; máscara `bin/sombra_regras_pessoais.py:139`. |

O ramo `resposta` do template mostra texto e instante, mas não mostra `ato_motivo`; o motivo “projeção sem linha” fica oculto nesse ramo (`modal_fio.html:208`/`:211`).

4. Os instantes não têm o mesmo significado:

- Humana: relógio do processamento do ato, `_tz.now()`, em `disputa_emissao.py:1337`, compartilhado com projeção e linha em `:1343`/`:1366`.
- Automática do upload: um `agora` obtido antes do lote em `app/api/foto_recebida.py:42`, passado a cada resposta em `:48`; escritor grava o mesmo instante em `disputa_emissao.py:1510`/`:1513`.
- Recuperação: instante histórico da primeira nota encontrada, ordenada por `criado_em`, em `curar_perguntas_em_limbo.py:31`/`:34`, passado em `:67`; gravação em `disputa_emissao.py:1531`/`:1534`.
- Carimbo: instante de materialização, fornecido pelo chamador ou relógio interno, em `models.py:2683`; preenche `respondida_em` e `validada_em` em `:2691`/`:2694`, sem registrar `RESPOSTA`/`VALIDACAO` nessa porta (`:2711`/`:2716`).
- Validação humana: relógio de validação em `validacao.py:134`, também usado na linha em `:139`.

Portanto, o helper normaliza origens diferentes como `resposta`/`validacao`, com motivo genérico que admite “carimbo sem ato” (`models.py:3396`, `:3417`/`:3422`). A igualdade por `(pergunta, ato, ato_em)` é uma heurística de correspondência; não demonstra igualdade de texto, autor ou natureza do evento, pois só consulta esses três valores (`models.py:3412`/`:3413`).

5. Duplicação/omissão e repetição:

- **Reabertura da absorção 2x: duplicação certa por inspeção.** Com queryset estável, a segunda chamada com `desfazer_absorcao=True` registra de novo mesmo com texto/timestamps/carimbo já vazios, porque o predicado termina em `or desfazer_absorcao` (`disputa_emissao.py:1485`). A inserção antecede a limpeza em `:1475`/`:1489`; não há unicidade da correspondência nas constraints (`models.py:3375`/`:3385`). O teste chama somente uma vez (`app/chamados/tests/test_e1_volta2.py:202`).
- **Helper não é deduplicador do próprio lote.** `[p, p]` devolve duas linhas por ato ausente: `ja` é calculado uma vez e nunca atualizado durante os `append` (`models.py:3412`/`:3422`). Não encontrei chamador atual que passe esse lote duplicado [não verificado].
- **Dois cálculos antes de registrar duplicam.** Duas chamadas ao helper antes de qualquer inserção devolvem ambas a mesma pendência; `registrar` insere sem resolução de conflito (`models.py:3428`). O teste verifica o caso estritamente sequencial “calcular, registrar, calcular” (`test_e1_volta2.py:90`/`:92`), não esse intercalamento. [nao verificado]
- **Trilha 2x com duas instâncias antigas:** ambas podem entrar com `respondida_em=None`, passar a guarda `disputa_emissao.py:1527`, e inserir a mesma correspondência em `:1534`. Na mesma instância ou recarregando após a primeira escrita, a guarda evita a segunda chamada (`:1527`/`:1531`). Concorrência/ocorrência real [não verificado].
- **Automática 2x na mesma instância ou recarregada:** texto existente produz no-op (`disputa_emissao.py:1504`/`:1505`). Duas instâncias antigas sem texto não são serializadas nem recarregadas antes de salvar/inserir (`:1508`/`:1514`).
- **Falha seguida de retry na mesma instância perde a cura do legado:** o core altera `p` antes do `atomic` (`disputa_emissao.py:1342`/`:1360`); se `registrar` levantar em `:1366`, o banco volta mas o objeto Python continua novo. Retry lê essa projeção falsa no helper (`models.py:3407`/`:3418`) e pode deixar de preservar o legado real do banco. Em `responder_automaticamente`, a primeira tentativa também muta antes do atomic (`:1509`/`:1511`); após rollback, retry na mesma instância retorna `False` por texto já preenchido em memória (`:1504`). Trata-se de contraexemplo inferido do código; execução/prod [não verificado].
- **Omissão por correspondência insuficiente:** existindo linha com mesmos três valores, texto/autor divergentes não motivam nova linha; esses campos não participam da consulta (`models.py:3412`/`:3422`). Existência desse estado em produção [não verificado].

Não usei banco, logs nem testes com escrita; os contraexemplos acima são análise estática [não verificado]. [nao verificado]

## 07/11 01:02 · sessao b6f748cc49ac · - · - · origem=Ronald (tmux)

3. **As três consultas passaram a compartilhar o domínio; o DRY tem dois universos distintos.** `do_fato` delega a `linhas_dos_fatos`; esta exige coluna `colaborador`, `conversa=False`, par exato motivo/dia, instância derivada do motivo e exclusão de `VIAS_FORA_DO_FATO`. O lote chama a mesma função. (`app/chamados/models.py:2445`, `app/chamados/models.py:2465`, `app/chamados/services/declaracao_texto.py:547`)

- `linhas_a_criar` do DRY procura qualquer projeção respondida/validada, inclusive conversa e pergunta retirada do fato: sua finalidade é recuperar decisões, e chama `linhas_da_projecao_sem_linha`. Não corresponde ao domínio de identidade. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:37`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:45`)
- `grupos_de_colisao` recompõe manualmente conversa/vias e agrupa a **instância armazenada**, enquanto `linhas_dos_fatos` só encontra a instância calculada de cada motivo. Portanto não são rigorosamente o mesmo domínio para acervo com instância divergente; ocorrência no dado fornecido [não verificado]. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:73`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:74`, `app/chamados/models.py:2465`)
- **Contraexemplo novo:** a geração ainda usa `exists()` manual por disputa/motivo/dia e desiste antes de chegar ao gate. Retire um motivo mantendo outro na disputa aberta: a pergunta vira `motivo_retratado`; se o mesmo motivo voltar no mesmo dia, a retratada satisfaz o `exists` e impede a nova pergunta, embora `do_fato` a exclua. (`app/chamados/models.py:1987`, `app/chamados/models.py:1994`, `app/chamados/services/disputa_emissao.py:388`, `app/chamados/services/disputa_emissao.py:404`, `app/chamados/services/disputa_emissao.py:423`, `app/chamados/models.py:2467`)
- Outra derivação manual permanece na fábrica: monta `ja` por disputa/colaborador e dia, excluindo **somente duplicata**; retratada, conversa ou instância divergente podem calar a emissão de motivo. (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:126`)
- `_candidata_reusavel` e `_ja_perguntou` também continuam montando coleções por disputa/motivo/dia sem usar o domínio central. (`app/chamados/services/declaracao_texto.py:236`, `app/chamados/services/disputa_emissao.py:1290`)

4. **A cura fecha os três caminhos alterados, mas não estabelece ordem global.** A porta comum, o agrupador e a âncora passaram a pedir Colaborador antes da Disputa. `ChamadoColaborador.abrir` e a abertura de formulário também começam pelo Colaborador. (`app/chamados/services/disputa_emissao.py:507`, `app/chamados/services/disputa_emissao.py:945`, `app/chamados/services/disputa_emissao.py:1208`, `app/chamados/models.py:872`, `app/chamados/services/abertura.py:164`)

**Contraexemplo residual Chamado ↔ Colaborador, inferido do fluxo de produção:**

- B: a tela chama `chamado.analisar()`; este grava o status em `atomic`. O `UPDATE` de Chamado ocorre antes do receiver síncrono que chama o cartório. (`app/chamados/views.py:1513`, `app/chamados/models.py:767`, `app/chamados/models.py:773`, `app/chamados/models.py:278`, `app/chamados/models.py:283`, `app/chamados/signals.py:164`, `app/chamados/signals.py:180`)
- B: em dia passado cuja célula acusa um marco ainda sem pergunta, cartório → fábrica → emissão pede Colaborador enquanto o Chamado continua travado pelo `UPDATE`. (`app/ponto/services/cartorio.py:1989`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:174`, `app/chamados/services/fabrica_da_celula.py:176`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/disputa_emissao.py:507`)
- A: `_nascer` segura Colaborador; ao reutilizar pergunta e trocar seu destino para esse Chamado, `_costura_nao_mata_o_fio` pede `SELECT FOR UPDATE` do Chamado. B espera Colaborador de A; A espera Chamado de B. (`app/chamados/models.py:2507`, `app/chamados/models.py:2509`, `app/chamados/models.py:2538`, `app/chamados/models.py:2547`, `app/chamados/models.py:2548`, `app/chamados/services/fio.py:765`, `app/chamados/services/fio.py:766`, `app/chamados/services/fio.py:767`)

Esse é um contraexemplo de ordem pelo código; execução concorrente e ocorrência em produção [não verificado].

O censo “dois sítios de trava da Disputa” também omite `desfazer_carimbo_sem_lastro`, que trava conjuntamente Pergunta e Disputa. Não provei que seu atomic isolado peça Colaborador depois; portanto sua simples existência não prova outro ciclo. (`app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:221`)

A trava de matrícula em `Colaborador.save` seleciona a última matrícula existente quando a instância não possui matrícula; não há nesse corpo nascimento de pergunta ou chamado. Sua transação interna termina antes de `super().save`, salvo transação externa do chamador. (`app/colaboradores/models.py:427`, `app/colaboradores/models.py:429`, `app/colaboradores/models.py:431`, `app/colaboradores/models.py:436`, `app/colaboradores/models.py:439`)

## 07/11 01:01 · sessao 1ceada648b2e · - · - · origem=Ronald (tmux)

6. **Sombra: cura estática coerente; execução não verificada.** O rename da migração coincide com a regra: `DecisaoPergunta`, `ato_texto`, `ato_motivo` (`app/chamados/migrations/0051_decisao_pergunta.py:41`; `bin/sombra_regras_pessoais.py:139`; `app/chamados/models.py:3356`). Máscara e selo resolvem o mesmo modelo pela lista, usando `_meta.db_table` no selo (`bin/sombra_mascara_pessoal.py:46`; `bin/sombra_selo_pessoal.py:24`). Não encontrei referência ativa a `RespostaAnterior` nesse inventário; permanece apenas comentário histórico (`bin/sombra_regras_pessoais.py:132`).

`_base_manager` contorna **todos** os overrides do queryset customizado, inclusive recusa de `delete`, além de `update`/`bulk_update`; a máscara somente usa leitura e `bulk_update` (`app/chamados/models.py:3314`; `app/chamados/models.py:3317`; `app/chamados/models.py:3320`; `bin/sombra_mascara_pessoal.py:51`; `bin/sombra_mascara_pessoal.py:62`). Não contorna constraints do banco; os checks continuam declarados (`app/chamados/models.py:3375`). O acesso continua dentro de `schema_context('juliani')` e `atomic`, com guarda do nome do banco (`bin/sombra_mascara_pessoal.py:10`; `bin/sombra_mascara_pessoal.py:16`). `bulk_update` já era a operação anterior, portanto pular `save`/signals não é uma consequência nova exclusiva da troca do manager. Não localizei outra garantia customizada de escrita nos managers dos modelos inventariados: `PautaQuerySet` oferece filtros explícitos e `DecisaoHEQuerySet` declara o manager sem filtro (`app/pautas/models.py:45`; `app/ponto/models.py:1250`). Execução real da máscara, inventário e selo: **[não verificado]**.

7. **Mordida dos testes: parcial e localizada.** [nao verificado]

- Os testes de resposta legada e validação legada realmente verificam o conteúdo histórico que a sobrescrita perderia; remover o resgate prévio quebra as asserções respectivas (`app/chamados/tests/test_e1_volta2.py:58`; `app/chamados/tests/test_e1_volta2.py:70`).
- Correspondência por instante é testada com uma linha em outro instante e com nova consulta após registrar; isso demonstra a sequência “consultar → registrar → consultar”, não duas listas calculadas antes do registro, lote com pergunta repetida ou corrida (`app/chamados/tests/test_e1_volta2.py:80`; `app/chamados/tests/test_e1_volta2.py:88`). O teste E1 escolhe deliberadamente horas diferentes, sem cobrir colisão de instante (`app/chamados/tests/test_e1_volta2.py:95`).
- A devolução verifica preservação da validação legada (`app/chamados/tests/test_e1_volta2.py:101`). Não há, nesse arquivo, caso exercitando `_carimbar_validacao`, resposta automática, ressuscitação, rollback de `registrar`, máscara ou DRY sem dia; a lista de testes termina na reabertura da absorção (`app/chamados/tests/test_e1_volta2.py:208`).
- O teste chamado “retratada não está no domínio do lote” **não executa `insumos_em_regularizacao`**: compara diretamente `linhas_dos_fatos` com `do_fato`, agora delegador da primeira. Ele prova o helper contra a expectativa `[viva.pk]`, mas a ligação do consumidor fica por conta do teste AST (`app/chamados/tests/test_e1_volta2.py:128`; `app/chamados/tests/test_e1_volta2.py:134`; `app/chamados/tests/test_e1_volta2.py:140`).
- O AST do lote morde o filtro original literal com `.objects`, mas só exige presença do atributo `linhas_dos_fatos` e ausência de qualquer atributo `objects`; não verifica que o resultado desse helper alimenta `por_fato`. Um helper chamado e descartado mais uma consulta manual por alias escaparia (`app/chamados/tests/test_e1_volta2.py:146`).
- O teste SQL da ordem examina somente `SELECT ... FOR UPDATE` de **duas tabelas**. Não vê locks de `UPDATE`, locks anteriores no chamador ou uma terceira tabela; o cenário nasce sem disputa existente. Portanto prova a ordem de consultas daquele caminho, não a ordem global nem ausência de deadlock (`app/chamados/tests/test_e1_volta2.py:155`; `app/chamados/tests/test_e1_volta2.py:163`; `app/chamados/tests/test_e1_volta2.py:168`).
- O teste AST do agrupador exige a trava como primeira instrução do primeiro `with`, mas não acompanha seus chamadores nem transações externas (`app/chamados/tests/test_e1_volta2.py:179`).
- Reabertura da irmã muda realmente morde a perda do carimbo sem linha, verificando descarimbo e autoria da linha; não verifica `ato_motivo` (`app/chamados/tests/test_e1_volta2.py:193`; `app/chamados/tests/test_e1_volta2.py:207`).
- O selo de escritor único examina apenas `APP`, exclui testes/migrations e reconhece nomes textuais `DecisaoPergunta`/`decisoes`. Apelidos e o `M._base_manager` da máscara não entram nesse detector; o mutante cobre somente `update` e `delete` literais (`app/chamados/tests/test_e1_entradas.py:196`; `app/chamados/tests/test_e1_entradas.py:205`; `app/chamados/tests/test_e1_entradas.py:224`).

**Classe “segundo escritor”: permanece.** JUIZES declara `validar_pergunta` escritor único de `validada_em`, mas `PerguntaDisputa.carimbar(..., resposta='validar')` escreve esse campo e autor diretamente, sem registrar validação (`app/docs/JUIZES.md:38`; `app/chamados/models.py:2693`). A triagem chamar isso de anterior à E1 não altera o contraexemplo estrutural; a lei cobra um escritor por estado e exceção com porta/trilha (`CLAUDE.md:32`).

**Classe relógio:** continua leitura interna nos atos; `_carimbar_validacao` não recebe instante e lê `timezone.now`, e a devolução também (`app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:134`; `app/chamados/services/validacao.py:205`). Nenhum teste dessa volta demonstra relógio injetado pela borda (`app/chamados/tests/test_e1_volta2.py:25`; `app/chamados/tests/test_e1_volta2.py:55`).

**Classe pré-requisito:** a 0051 declara explicitamente que o índice depende de tratar colisões e conversas antigas; o default de conversas antigas permanece `False` (`app/chamados/migrations/0051_decisao_pergunta.py:4`; `app/chamados/migrations/0051_decisao_pergunta.py:99`). Não há prova de acervo saneado: **[não verificado]**. O patch separado não deve ser considerado garantia já instalada.

8. **Teto: justificativa qualitativa confere com o diff; medição não confirmada.** O comentário descreve um SELECT adicional constante no portão da disputa e corrige a unidade anterior para chamada a `_nascer`, inclusive reuso (`app/ponto/tests/test_selo_chokepoint_escrita.py:172`; `app/ponto/tests/test_selo_chokepoint_escrita.py:174`). O código novo chama o helper imediatamente antes do SELECT da disputa; o helper executa uma única consulta sem abrir outro atomic (`app/chamados/services/disputa_emissao.py:507`; `app/chamados/models.py:2427`). Os novos tetos são 140 e 206 (`app/ponto/tests/test_selo_chokepoint_escrita.py:180`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`). A alegação de que o diff das listas de queries contém **somente** esse SELECT e o delta real é exatamente +1: **[não verificado]**; não há logs acessíveis nesta cópia. O próprio selo cobra apenas `medido <= teto`, sem identificar a causa do delta (`app/ponto/tests/test_selo_chokepoint_escrita.py:240`).

## 07/11 00:21 · sessao 10bc0fe98df9 · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: bloqueia considerar o commit como E1 concluída; o patch do índice também precisa de correções nos caminhos que alteram a identidade.** Há decisões sem linha, perda da resposta legada, absorção sem atomicidade completa e inversão de travas. A ausência do índice no commit é deliberada e foi tratada como unidade separada. (app/chamados/models.py:1508; app/chamados/models.py:2658; app/chamados/services/disputa_emissao.py:1351; app/chamados/services/materializacao.py:550; app/chamados/services/disputa_emissao.py:1129; app/chamados/migrations/0051_decisao_pergunta.py:3)

Resultados executados de testes, conteúdo do acervo, colisões reais, histórico de migrações em produção e igualdade dos hashes FM/TXT 09–10: **[não verificado]**. [nao verificado]

1. **Campos, escritores, leitores e significados**

   O escritor físico da sequência é `registrar → bulk_create`. Os produtores dos valores são os atos abaixo; portanto “uma porta de INSERT” não significa “todos os escritores da projeção registram decisões”. (app/chamados/models.py:3364; app/chamados/models.py:2658)

   | Produtor | Campos de `DecisaoPergunta` fornecidos |
   |---|---|
   | Encerramento da disputa | `ato`, `ato_em`, `ato_por`, `ato_texto`, `ato_motivo` — app/chamados/models.py:1862 |
   | Remoção do motivo | Os cinco anteriores — app/chamados/models.py:1982 |
   | Resposta do colaborador | `ato`, `ato_em`, `ato_por`, `ato_texto` — app/chamados/services/disputa_emissao.py:1351 |
   | Reabertura/recusa | Os cinco primeiros e `validada_original_por` — app/chamados/services/disputa_emissao.py:1460 |
   | Resposta automática | `ato`, `ato_em`, `ato_texto`, `ato_motivo`; autor fica nulo — app/chamados/services/disputa_emissao.py:1493 |
   | Recuperação pela trilha | Os mesmos da automática — app/chamados/services/disputa_emissao.py:1514 |
   | Validação | `ato`, `ato_em`, `ato_por`, `ato_texto` — app/chamados/services/validacao.py:138 |
   | Devolução | Os cinco primeiros — app/chamados/services/validacao.py:203 |
   | Absorção | `ato`, `ato_em`, `ato_por`, `ato_texto`, `absorcao_escape`, `absorcao_ausencia` — app/chamados/services/materializacao.py:555 |

   | Campo | Leitores da sequência fora dos testes | [nao verificado]
   |---|---|
   | `ato` | Seleção do histórico: app/chamados/models.py:2961,2965; ausência de decisão no DRY: app/chamados/management/commands/backfill_decisoes_pergunta.py:31; apresentação: app/templates/chamados/partials/modal_fio.html:208,211; checks: app/chamados/models.py:3344,3349 |
   | `ato_em` | Ordenação: app/chamados/models.py:3341; apresentação: app/templates/chamados/partials/modal_fio.html:209,211 |
   | `ato_por` | Carregamento: app/chamados/services/fio.py:232; apresentação: app/templates/chamados/partials/modal_fio.html:211 |
   | `ato_texto` | Apresentação: app/templates/chamados/partials/modal_fio.html:209,211 |
   | `ato_motivo` | Apresentação: app/templates/chamados/partials/modal_fio.html:211 |
   | `validada_original_por` | Carregamento: app/chamados/services/fio.py:232; apresentação: app/templates/chamados/partials/modal_fio.html:211; check: app/chamados/models.py:3351 |
   | `absorcao_escape`, `absorcao_ausencia` | Checks: app/chamados/models.py:3344,3346. O leitor de negócio continua reconstruindo a absorção pelos carimbos: app/chamados/services/materializacao.py:595,615,619 |

   A migração também escreve esses campos: renomeia texto/instante/autor/motivo anteriores, atribui `ato='reabertura'` às linhas antigas e introduz as ligações de absorção nulas. A fotografia anterior do validador já existia na 0050. (app/chamados/migrations/0051_decisao_pergunta.py:41,49,67; app/chamados/migrations/0050_resposta_anterior.py:26)

   | Campo novo de `PerguntaDisputa` | Escritores | Leitores |
   |---|---|---|
   | `colaborador` | Derivação na inserção: app/chamados/models.py:2923,2935; preenchimento do acervo: app/chamados/migrations/0051_decisao_pergunta.py:25; fixture direta: app/chamados/tests/test_e1_entradas.py:313 | Dedup: app/chamados/models.py:2430; conferência de nascimento: app/chamados/models.py:2934,2936,2938; DRY: app/chamados/management/commands/backfill_decisoes_pergunta.py:34,36,54; teste: app/chamados/tests/test_e1_entradas.py:174 |
   | `instancia_marco` | Default e derivação: app/chamados/models.py:2241,2941; migração: app/chamados/migrations/0051_decisao_pergunta.py:27,96; fixture: app/chamados/tests/test_e1_entradas.py:314 | Dedup/derivação: app/chamados/models.py:2431,2939; DRY: app/chamados/management/commands/backfill_decisoes_pergunta.py:54; teste: app/chamados/tests/test_e1_entradas.py:175 |
   | `conversa` | Default: app/chamados/models.py:2242; portas e INSERT: app/chamados/models.py:2450,2461,2546; todo o legado recebe `False`: app/chamados/migrations/0051_decisao_pergunta.py:99 | Trava/dedup/derivação: app/chamados/models.py:2474,2495,2939; DRY: app/chamados/management/commands/backfill_decisoes_pergunta.py:55; testes: app/chamados/tests/test_e1_entradas.py:176,182 |

   A fotografia genérica dos testes lê todos os campos dos dois modelos. O índice anexado acrescenta leitura de `colaborador`, `data_evento`, `motivo`, `instancia_marco`, `conversa` e `via_resolucao`. (app/chamados/tests/test_e1_entradas.py:281,282; app/chamados/migrations/0052_pergunta_unica_por_fato.py:19,20 — anexo)

   **Não há um significado uniforme para todas as projeções:**

   - `respondida_em` pode ser o instante da resposta recebida, um instante histórico recuperado da trilha ou um carimbo automático colocado pelo encerramento; `validada_em` pode representar validação do admin ou preenchimento automático por `carimbar(resposta='validar')`. **Fila**, porque os leitores que tratam presença do carimbo como ato humano não distinguem essas origens. Contraexemplo: encerramento por fato preenche `validada_em` sem passar por `_carimbar_validacao`. (app/chamados/services/disputa_emissao.py:1325,1511,1681; app/chamados/models.py:2658,2661; app/chamados/services/validacao.py:133)
   - `materializada_em` significa tanto efeito aplicado quanto encerramento sem efeito. **Fila:** pergunta de hora respondida e depois retratada ganha carimbo sem exigir batida, mas `estado_aplicacao_pergunta` devolve `aplicada`, excetuando apenas paridade. (app/chamados/models.py:1987; app/chamados/services/materializacao.py:434,438,442; app/chamados/services/fio.py:336)
   - `conversa=False` significa nascimento pela porta de fato nas linhas novas e simplesmente default da migração nas antigas. **Bloqueia usar a coluna como classificação histórica comprovada.** Duas conversas antigas do mesmo motivo/dia passam a parecer duas linhas do fato. (app/chamados/models.py:2450,2461; app/chamados/migrations/0051_decisao_pergunta.py:4,99)
   - `instancia_marco=''` reúne motivos sem marco e conversas; não identifica ocorrência de batida, pausa ou episódio. A própria derivação declara que duas pausas ainda não são separadas. (app/chamados/services/declaracao_texto.py:47,52,57; app/chamados/models.py:2939)
   - Os carimbos são `DateTimeField`, com configuração de fuso `America/Sao_Paulo` e `USE_TZ=True`; `data_evento` é uma data. Não encontrei uma troca explícita de unidade nesses campos novos. Validade de todos os instantes históricos recebidos em execução: **[não verificado]**. (app/chamados/models.py:2230,2249,2250,3325; app/config/settings/base.py:93,95)
   - O contexto conserva a distinção dia civil/dia do turno: conversa nasce pela sua própria porta, enquanto o rechaveamento existe justamente para retirar do acervo o segundo significado “dia da saída”. A migração 0051 não corrige esse acervo. (app/chamados/models.py:2453; app/chamados/services/fio.py:862; app/chamados/migrations/0051_decisao_pergunta.py:25)

2. **Há caminhos de produção sem linha em `DecisaoPergunta`**

   | Escritor persistente | Campos afetados | Registra linha? |
   |---|---|---|
   | Resposta normal | Texto, resposta, eventual limpeza da validação e descarimbo | **Sim**, apenas a resposta nova — app/chamados/services/disputa_emissao.py:1326,1339,1344,1348,1351 |
   | Reabertura/recusa | Texto, resposta e validação; opcionalmente materialização/via | **Condicional:** só se houver texto ou `respondida_em` — app/chamados/services/disputa_emissao.py:1444,1460,1466,1468,1470 |
   | Resposta automática | Texto e resposta | **Sim** — app/chamados/services/disputa_emissao.py:1489,1493 |
   | Carimbo pela trilha | Resposta | **Sim** — app/chamados/services/disputa_emissao.py:1511,1514 |
   | `_carimbar_validacao` | Validação | **Sim** — app/chamados/services/validacao.py:133,138 |
   | `devolver_validacao_recusada` | Limpa validação | **Sim**, quando havia validação — app/chamados/services/validacao.py:194,197,203 |
   | `carimbar` | Materialização/via e, opcionalmente, resposta/validação | **Não na porta** — app/chamados/models.py:2655,2658,2661,2678 |
   | `carimbar_identidade` | Materialização/via | **Não na porta** — app/chamados/models.py:2705,2708,2713 |
   | `descarimbar` | Limpa materialização/via | **Não** — app/chamados/models.py:2717,2721 |
   | `descarimbar_em_lote` | Limpa materialização/via e campos adicionais | **Não** — app/chamados/models.py:2768 |
   | `encerrar_por_fato_no_espelho` | Troca via | **Não** — app/chamados/models.py:2778 |
   | `relavrar` | Troca via | **Não**; registra `evento`, outra trilha — app/chamados/services/relavra_via.py:123,130 |

   Os chamadores de carimbo também precisam ser diferenciados:

   | Caminho | Linha correspondente |
   |---|---|
   | Encerramento da disputa por admin/desligamento | `SILENCIO` — app/chamados/models.py:1859,1862 |
   | Remoção de motivo | `RETRATACAO` — app/chamados/models.py:1982,1987 |
   | Absorção | `ABSORCAO`, após carimbar — app/chamados/services/materializacao.py:550,555 |
   | Resolver o dia | **Nenhuma** para a pergunta encerrada — app/ponto/services/veredito_celula.py:174,177 |
   | Aceite de folga | Há `VALIDACAO`; o fechamento adicional não tem linha própria — app/chamados/services/validacao.py:279,280,306,315 |
   | Duplicata | **Nenhuma** — app/chamados/services/fio.py:952 |
   | Paridade/fato ocupado | **Nenhuma** — app/chamados/services/materializacao.py:909,1084 |
   | Sem turno/célula absolveu | **Nenhuma** — app/chamados/reconciliador.py:405,586 |
   | Materialização normal | **Nenhuma na chamada de carimbo**, inclusive validação automática — app/chamados/services/materializacao.py:1262 |
   | Pendentes aprovadas pelo juiz | **Nenhuma** — app/chamados/services/materializacao.py:1416 |
   | Encerramento pelo fato/sem credor | **Nenhuma**, inclusive carimbo automático de resposta/validação — app/chamados/services/disputa_emissao.py:1681,1755 |
   | Reabertura por competência/fantasma | **Nenhuma** — app/chamados/services/competencia_trancada.py:254; app/chamados/management/commands/reabrir_orfas_enterradas.py:147 |
   | Desfazer carimbo sem lastro | **Nenhuma** — app/chamados/services/relavra_via.py:231 |
   | Encerrar por fato no espelho | **Nenhuma** — app/chamados/management/commands/encerrar_perguntas_com_fato.py:68 |

   **Bloqueia — perda do legado no primeiro uso.** Contraexemplo: pergunta com `08:00`, `respondida_em` antigo e nenhuma decisão recebe `09:00`. O código sobrescreve a projeção e insere somente `09:00`; `respostas_anteriores` devolve lista vazia. O DRY não preserva previamente `08:00`. (app/chamados/services/disputa_emissao.py:1326,1351; app/chamados/models.py:2960; app/chamados/management/commands/backfill_decisoes_pergunta.py:33)

   **Bloqueia — reabertura sem decisão.** Contraexemplo: irmã muda absorvida, com decisão `ABSORCAO`, é devolvida após rejeição da ausência. O filtro que produz decisões ignora a irmã sem texto e sem resposta, mas o `UPDATE` remove seu carimbo. O mesmo predicado deixa passar validação sem texto/resposta sem fotografá-la. (app/chamados/services/materializacao.py:668; app/chamados/services/disputa_emissao.py:1466,1468)

   **Bloqueia — ressuscitação apaga decisões projetadas.** Limpa `respondida_em`, `validada_em`, materialização e via, sem inserir decisão; a trilha existente é `LogAuditoria`. (app/chamados/models.py:1508,1522,2768)

   **Bloqueia — absorção pode ficar sem sua linha.** No caminho web/API, a transação da resposta termina antes da absorção. `q.carimbar` salva em seu próprio atomic e o INSERT da decisão vem depois. Contraexemplo: falha de `bulk_create` após o carimbo deixa a pergunta materializada sem `ABSORCAO`. (app/chamados/views.py:1283; app/api/views.py:2659; app/chamados/services/disputa_emissao.py:1347,1381; app/chamados/services/materializacao.py:550,555; app/chamados/models.py:2925)

   **Fila — autoria disponível é descartada.** A reabertura pelo colaborador conhece `autor`, mas chama a porta sem `por`; a devolução da ausência conhece motivo, mas também não o transmite. (app/chamados/models.py:2146,2158; app/chamados/services/materializacao.py:665,668)

   Não localizei chamada explícita de produção que apague diretamente `PerguntaDisputa` ou `DisputaSupervisao`. A FK disputa→pergunta é `CASCADE`; pergunta com decisão, pergunta usada como escape e ausência usada na absorção são protegidas. Assim, apagar a disputa pode apagar perguntas **sem decisões**, enquanto a existência de decisão impede o apagamento correspondente. (app/chamados/models.py:2223,3323,3333,3335)

   A imutabilidade não é integral: autores usam `SET_NULL`. Exclusão de usuário pode alterar autoria histórica sem passar pelos métodos bloqueados; ocorrência desse fluxo em produção: **[não verificado]**. (app/chamados/models.py:3326,3331,3355)

3. **Corrida e ordem das travas**

   **Entre dois nascimentos datados que passam por `_nascer`, a trava serializa o dedup.** Ela é adquirida antes de `do_fato`; dentro desse recorte, o segundo ato consulta depois do primeiro. A conversa não adquire essa trava. (app/chamados/models.py:2473,2475,2495,2498)

   **A trava sozinha não garante unicidade da árvore.** Rechaveamento, datação posterior e descarimbo não participam da mesma trava; uma criação direta também passa apenas pela derivação de identidade do `save`. O teste do próprio DRY constrói uma segunda linha diretamente. (app/chamados/services/fio.py:836,887,893; app/chamados/models.py:2768,2923; app/chamados/tests/test_e1_entradas.py:313)

   **Com o patch, o banco impede a segunda identidade indexada, mas os caminhos concorrentes passam a poder levantar `IntegrityError` sem recuperação.** Conversas e vias fora do fato são excluídas; datas nulas continuam fora de uma garantia de igualdade entre chaves nulas. (app/chamados/migrations/0052_pergunta_unica_por_fato.py:19,20 — anexo; app/chamados/services/fio.py:851,904; app/chamados/models.py:2230)

   **Bloqueia — ciclo de travas concreto, nos dois pacotes:**

   - A declaração abre transação, trava `DisputaSupervisao` e depois pede `Colaborador`. (app/chamados/services/declaracao_texto.py:312,320,336; app/chamados/services/disputa_emissao.py:501; app/chamados/models.py:2475)
   - O agrupador de órfãos pode ler a disputa sem trava, abrir pergunta mantendo `Colaborador` e depois atualizar a mesma disputa. (app/chamados/services/disputa_emissao.py:934,971,1088,1129)
   - Intercalação: A segura Disputa e espera Colaborador; B segura Colaborador e espera Disputa. O comentário anterior já reconhece a ordem Disputa→Colaborador; o novo comentário “colaborador primeiro” não descreve a ordem global. (app/chamados/models.py:847,2470)

   Mudança posterior de motivo, instância, conversa ou colaborador não é impedida pelo `save`: a conferência só ocorre na inserção, e instância explicitamente não vazia é aceita sem confronto com o motivo. Não localizei escritor de produção que faça essas mudanças posteriores; existência de tal caminho dinâmico: **[não verificado]**. (app/chamados/models.py:2923,2934,2939)

4. **Migrações 0051 e 0052**

   **0051: compatível estruturalmente com o legado descrito; segurança no acervo real [não verificado].** O rename preserva as linhas, o novo ato recebe `reabertura`, a foto antiga continua permitida e as ligações de absorção nascem nulas. O `SET CONSTRAINTS ALL IMMEDIATE` está antes do `ALTER ... NOT NULL`, tratando os eventos diferidos produzidos pelo preenchimento da FK. (app/chamados/migrations/0051_decisao_pergunta.py:41,49,67,85,101,104)

   **Fila — a migração não identifica conversas antigas nem corrige o dia antigo.** Contraexemplo: duas conversas históricas iguais em colaborador/dia/motivo tornam-se colisão do índice, porque ambas recebem `conversa=False`. (app/chamados/migrations/0051_decisao_pergunta.py:4,25,99)

   **O congelado bate com o vivo:** os quatro pares são `saida_sem_entrada→inicio`, `orfao_14h→fim`, `intervalo_saida→int_ida`, `intervalo_volta→int_volta`; há comparação direta em teste. (app/chamados/migrations/0051_decisao_pergunta.py:14; app/chamados/services/declaracao_texto.py:42,52; app/chamados/tests/test_e1_entradas.py:259)

   **0052: bloqueia aplicar como está sem resolver os caminhos do item 8 e classificar o acervo.** Zero colisões no DRY não foi fornecido. Além disso, o DRY agrupa `data_evento=NULL`, enquanto o índice não torna essas chaves nulas iguais. Uma colisão criada entre DRY e DDL faria a instalação do índice falhar; não seria corrupção silenciosa. (app/chamados/management/commands/backfill_decisoes_pergunta.py:54,56; app/chamados/migrations/0052_pergunta_unica_por_fato.py:19,20 — anexo; **[não verificado]** quanto ao acervo)

5. **`backfill_decisoes_pergunta`**

   **Não escreve modelos e lê a autoridade correta.** Faz consultas e saída em `stdout`, sem opção de apply; consulta `folha.export.competencia_entregue`, autoridade `_K6`, que considera o TXT vigente não invalidado. A competência do fato vem do adaptador existente. (app/chamados/management/commands/backfill_decisoes_pergunta.py:25,51,67,71,76; app/core/juizes.py:312; app/folha/export.py:580; app/ponto/services/fechamento.py:1595)

   **Fila — não suporta todo o domínio permitido pelo modelo.** Contraexemplo: pergunta legada respondida sem `data_evento` chega a `competencia_do_fato(None)` e à comparação `None < None`; duplicatas sem data e sem resposta chegam a `dia.isoformat()`. Presença real dessas linhas: **[não verificado]**. (app/chamados/models.py:2230; app/chamados/management/commands/backfill_decisoes_pergunta.py:45,90; app/ponto/services/fechamento.py:1588,1590)

   O DRY mede ausência de qualquer ato daquele tipo, não se a decisão existente corresponde à projeção vigente. Portanto uma primeira resposta E1 já presente faz o DRY deixar de apontar a resposta legada sobrescrita do item 2. (app/chamados/management/commands/backfill_decisoes_pergunta.py:31,33; app/chamados/services/disputa_emissao.py:1351)

6. **Classes de falha solicitadas**

   | Classe | Veredito e contraexemplo |
   |---|---|
   | Segundo juiz registrado | **Não caracterizado nas adições.** A instância inverte o catálogo; competência chama o juiz existente. O espelho SQL é declarado como continente, não substituto exato do C6. (app/chamados/services/declaracao_texto.py:52; app/core/juizes.py:312; app/chamados/models.py:2842) |
   | Segundo escritor fora da porta | **Bloqueia.** `carimbar` escreve validação fora do escritor indicado em `JUIZES.md` e não cria decisão. Contraexemplo: encerramento por fato com `resposta='validar'`. (app/docs/JUIZES.md:38; app/chamados/models.py:2661; app/chamados/services/disputa_emissao.py:1681) |
   | Relógio fora da borda | **Fila.** Há leituras dentro dos atos, sem instante recebido por parâmetro, inclusive duas leituras na mesma retratação para contexto e decisão. Não apareceu novo juiz puro lendo relógio nesta mudança. (app/chamados/models.py:1965,1978; app/chamados/services/validacao.py:133,203) |
   | Derivação paralela da fonte única | **Bloqueia.** O lote de declaração não aplica o novo domínio de `do_fato`; uma retratada validada bloqueia o formulário no lote, enquanto a consulta avulsa permite. (app/chamados/services/declaracao_texto.py:434,436,546; app/chamados/models.py:2432) |
   | Selo/RE

[truncado em 20000 de 49082 caracteres]

## 07/11 00:14 · sessao 70b6448f994a · - · - · origem=Ronald (tmux)

Achados confirmados e referências já enviados ao agente principal:

- **Bloqueia:** resposta nova de pergunta legada sem decisões guarda apenas o texto novo; a antiga desaparece da fonte histórica. `app/chamados/services/disputa_emissao.py:1326`, `app/chamados/services/disputa_emissao.py:1351`, `app/chamados/models.py:2960`.
- **Bloqueia:** `carimbar` escreve resposta/validação sem linhas correspondentes; existem chamadores de produção. `app/chamados/models.py:2658`, `app/chamados/models.py:2661`, `app/chamados/services/disputa_emissao.py:1681`, `app/chamados/services/materializacao.py:1262`.
- **Bloqueia:** ressurreição limpa carimbos e validação sem decisão; reabertura da irmã muda absorvida também não registra o ato. `app/chamados/models.py:1508`, `app/chamados/models.py:2768`, `app/chamados/services/disputa_emissao.py:1466`, `app/chamados/services/materializacao.py:668`.
- **Bloqueia:** absorção pelo núcleo da resposta salva o carimbo antes de registrar a ligação, sem transação envolvendo ambos. Falha no registro deixa carimbo persistido sem `ABSORCAO`. `app/chamados/services/disputa_emissao.py:1381`, `app/chamados/services/materializacao.py:550`, `app/chamados/services/materializacao.py:555`.
- **Bloqueia:** Resolver dia encerra perguntas sem registrar `SILENCIO`. `app/ponto/services/veredito_celula.py:174`, `app/ponto/services/veredito_celula.py:177`, `app/chamados/models.py:2591`.
- **Fila:** `materializada_em` significa tanto efeito aplicado quanto encerramento sem efeito. Uma pergunta de hora retratada pode aparecer como aplicada. `app/chamados/services/materializacao.py:434`, `app/chamados/services/materializacao.py:442`, `app/chamados/services/fio.py:336`.
- **Fila:** reabertura pelo colaborador e devolução por ausência rejeitada conhecem autor/motivo, mas não os passam à decisão. `app/chamados/models.py:2158`, `app/chamados/services/materializacao.py:665`, `app/chamados/services/materializacao.py:668`.
- **Limite da imutabilidade:** autoria usa `SET_NULL`; exclusão do usuário pode modificar uma decisão histórica. Existência de tal exclusão em produção não foi verificada. `app/chamados/models.py:3326`, `app/chamados/models.py:3331`.
- Não encontrei chamada explícita de produção a `PerguntaDisputa.delete()` ou `DisputaSupervisao.delete()`. A relação disputa→pergunta usa `CASCADE`, mas pergunta com decisão recebe proteção por `PROTECT`; colaborador também é protegido pela coluna nova. `app/chamados/models.py:2223`, `app/chamados/models.py:2240`, `app/chamados/models.py:3323`.

Enviei inventário de referências de produção, incluindo comandos e templates, para os cinco campos de projeção e todos os campos de `DecisaoPergunta`. Os campos `absorcao_escape` e `absorcao_ausencia` ainda não têm leitor de negócio; o leitor da absorção continua reconstruindo o grupo pelo carimbo. `app/chamados/services/materializacao.py:555`, `app/chamados/services/materializacao.py:595`, `app/chamados/services/materializacao.py:619`.

## 07/11 00:13 · sessao 01b79c614c78 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos por inspeção estática; ocorrência em produção e cardinalidades do acervo permanecem [não verificado].

1. **Bloqueia — ordem de trava invertida, com ciclo concreto.** `declarar_hora_colab` abre transação, trava a disputa por `_abrir_ou_agregar_disputa` e depois pede a trava do colaborador ao abrir pergunta (`app/chamados/services/declaracao_texto.py:312`, `app/chamados/services/declaracao_texto.py:320`, `app/chamados/services/declaracao_texto.py:336`, `app/chamados/services/disputa_emissao.py:501`, `app/chamados/models.py:2475`). O agrupador de órfãos abre uma transação maior, lê disputa existente sem trava, abre pergunta — mantendo a trava do colaborador — e depois salva os motivos da disputa (`app/chamados/services/disputa_emissao.py:934`, `app/chamados/services/disputa_emissao.py:971`, `app/chamados/services/disputa_emissao.py:1088`, `app/chamados/services/disputa_emissao.py:1129`). Contraexemplo: A segura Disputa e espera Colaborador; B segura Colaborador e espera o UPDATE da mesma Disputa. O comentário anterior já declara Disputa→Colaborador, enquanto `_nascer` agora anuncia Colaborador primeiro (`app/chamados/models.py:847`, `app/chamados/models.py:2470`). O índice não corrige isso.

2. **Bloqueia — segunda derivação da identidade divergiu da fonte.** O leitor avulso da projeção usa `do_fato`, que agora exclui duplicata/retratada e exige instância; o lote do fio/API continua agrupando por colaborador da disputa, motivo e data, incluindo as linhas retiradas do fato (`app/chamados/models.py:2430`, `app/chamados/services/declaracao_texto.py:433`, `app/chamados/services/declaracao_texto.py:546`). A escolha final aceita a última linha não reutilizável e consulta sua validação (`app/chamados/services/declaracao_texto.py:251`, `app/chamados/services/declaracao_texto.py:266`). Contraexemplo: única pergunta antiga, validada e depois retratada, para um marco atualmente faltante. Consulta avulsa recebe lista vazia e permite declarar; o lote inclui a retratada validada e recusa/esconde o formulário (`app/chamados/services/declaracao_texto.py:434`, `app/chamados/services/declaracao_texto.py:436`, `app/chamados/services/declaracao_texto.py:438`). O índice não cura.

3. **Bloqueia para o patch — `datou` contorna a consulta de colisão.** `reapontar_resolvedora` data uma pergunta sem data e salva sem consultar `do_fato` nem travar Colaborador (`app/chamados/services/fio.py:834`, `app/chamados/services/fio.py:850`). Seus gatilhos existentes incluem reemissão, histórico e derivação de destino (`app/chamados/reconciliador.py:944`, `app/chamados/reconciliador.py:1628`, `app/chamados/reconciliador.py:1641`). Contraexemplo: p1 sem data e p2 já no destino `(colab,D,motivo,instância)`; datar p1 em D cria segunda linha sem índice, ou levanta `IntegrityError` sem tratamento com o patch. O acervo atual contendo p1 é [não verificado].

4. **Bloqueia para o patch — reentrada de retratada/duplicata pelo descarimbo.** `_universo_da_ressuscitacao` seleciona materializadas sem texto, sem excluir vias de identidade; limpa a via na cópia ANTES de perguntar ao C6 (`app/chamados/models.py:1438`, `app/chamados/models.py:1459`). A porta depois limpa os campos reais em lote (`app/chamados/models.py:1508`, `app/chamados/models.py:2770`). Contraexemplo: p1 muda retratada; o fato volta e p2 nasce; cartório/reabertura ressuscita p1 porque seu marco está apagado. Sem índice, p1 e p2 voltam ao mesmo fato; com índice, o UPDATE falha. Gatilhos de produção: cartório, reabertura e desarquivamento (`app/ponto/services/cartorio.py:1622`, `app/chamados/models.py:1406`, `app/chamados/views_arquivar.py:86`).

5. **Fila/bloqueia conforme promessa — `rechavear` ainda tem corrida fora da trava comum.** Trava apenas a própria pergunta e consulta o destino antes de salvar; não participa da trava Colaborador de `_nascer` (`app/chamados/services/fio.py:886`, `app/chamados/services/fio.py:893`, `app/chamados/services/fio.py:905`). Contraexemplo: rechaveamento verifica destino vazio; nascimento simultâneo cria ali; rechaveamento salva depois. Sem índice duplica; com índice impede duplicação por erro sem recuperação. Dois rechaveamentos de perguntas distintas para o mesmo destino têm o mesmo problema (`app/chamados/services/fio.py:887`, `app/chamados/services/fio.py:893`).

6. **Bloqueia — conversa e fato ainda se misturam em `do_fato`.** O índice e o DRY excluem `conversa=True`, mas `do_fato` não filtra conversa (`app/chamados/models.py:2430`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; patch fornecido `e1_indice_0052.patch:19`). Contraexemplo: conversa administrativa de `posto_desativado`, instância `''`, no dia D; emissão datada posterior do mesmo motivo/D devolve a conversa como pergunta existente (`app/chamados/services/acoes_disputa.py:247`, `app/chamados/services/acoes_disputa.py:260`, `app/chamados/models.py:2498`). O problema independe do índice.

7. **Fila para o modelo; bloqueia aplicar índice sem classificar acervo — instância vazia não significa necessariamente “dia inteiro”.** Somente quatro motivos têm instância: entrada, saída e os dois lados do intervalo (`app/chamados/services/declaracao_texto.py:42`, `app/chamados/services/declaracao_texto.py:57`). Fora deles, catálogo inclui `sem_posto`, `em_ferias`, `posto_desativado`, `colisao_escala`, `ordem_substituicao_invalida`, `batida_fora_escala_grave`, `gps_ausente`, `foto_ausente`, `gps_mock`, `device_comprometido`, `liveness_indisponivel`, `ancora_ausente` (`app/chamados/catalogo/perguntas.py:36`, `app/chamados/catalogo/perguntas.py:118`). Foto, mock, device e liveness são anomalias de BATIDA: duas batidas do mesmo turno podem apresentar duas ocorrências legítimas do mesmo motivo (`app/api/views.py:479`, `app/api/views.py:488`, `app/api/views.py:504`, `app/api/views.py:519`). O contexto carrega o ID da batida e a data do emissor vem dessa batida, mas essa ocorrência não entra na identidade (`app/api/views.py:788`, `app/chamados/services/disputa_emissao.py:379`). O dedup atual já colapsa esses fatos; o índice cristaliza a limitação (`app/chamados/models.py:2498`). Conversas legítimas antigas também recebem `conversa=False` na migração, embora reabertura administrativa permita qualquer motivo e crie conversa nova (`app/chamados/migrations/0051_decisao_pergunta.py:99`, `app/chamados/services/acoes_disputa.py:247`, `app/chamados/services/acoes_disputa.py:260`).

8. **Mudança de continência: correta isoladamente.** C6 e SQL passam a consultar a mesma tupla, com materialização presente e antes da resposta; portanto a exclusão nova não esconde pergunta considerada viva por esse degrau do C6 (`app/chamados/juizes.py:1147`, `app/chamados/models.py:2855`, `app/chamados/models.py:2862`). Isso não constitui prova executada de toda a continência da árvore [não verificado].

9. **Outros leitores alterados por retratada/identidade.**
   - Materialização de fato passa a aceitar retratada sem testemunha de batida, por identidade (`app/chamados/juizes.py:648`, `app/chamados/juizes.py:658`).
   - `carimbar` passa a recusá-la e `carimbar_identidade` passa a aceitá-la (`app/chamados/models.py:2646`, `app/chamados/models.py:2703`).
   - `q_carimbo_desfazivel` deixa de oferecê-la à porta de desfazer; a porta relê esse filtro travado (`app/chamados/services/relavra_via.py:179`, `app/chamados/services/relavra_via.py:221`).
   - Auditor reconhece a via e a classifica como identidade (`app/chamados/management/commands/auditar_invariantes_chamados.py:47`, `app/chamados/management/commands/auditar_invariantes_chamados.py:195`, `app/chamados/management/commands/auditar_invariantes_chamados.py:206`).
   - Filas de correção/validação mudam indiretamente por consultarem C6 (`app/chamados/juizes.py:1394`, `app/chamados/juizes.py:1401`).

Inventário dos três campos novos, com ocorrências qualificadas:

| Campo | Escritores/defaults | Leitores |
|---|---|---|
| `colaborador` | declaração FK `app/chamados/models.py:2240`; inserção deriva em `:2935`, acionada em `:2923`; migração deriva DS `app/chamados/migrations/0051_decisao_pergunta.py:25`, coluna nullable `:92`, ALTER obrigatório `:105`; fixture explícita `app/chamados/tests/test_e1_entradas.py:313` | `do_fato` `app/chamados/models.py:2430`; consistência de inserção `:2934`, `:2936`, `:2938`; DRY `app/chamados/management/commands/backfill_decisoes_pergunta.py:34`, `:36`, `:54`; teste `app/chamados/tests/test_e1_entradas.py:174` |
| `instancia_marco` | default `app/chamados/models.py:2241`; derivação `:2941`; default migração `app/chamados/migrations/0051_decisao_pergunta.py:96`; backfill `:27`; fixture `app/chamados/tests/test_e1_entradas.py:314` | `app/chamados/models.py:2431`, `:2939`; DRY `app/chamados/management/commands/backfill_decisoes_pergunta.py:54`; teste `app/chamados/tests/test_e1_entradas.py:175` |
| `conversa` | default `app/chamados/models.py:2242`; portas passam False/True `:2450`, `:2461`; INSERT `:2546`; default de TODO acervo `app/chamados/migrations/0051_decisao_pergunta.py:99` | `_nascer` `app/chamados/models.py:2474`, `:2495`; derivação `:2939`; DRY `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; testes `app/chamados/tests/test_e1_entradas.py:176`, `:182` |

A fotografia genérica `.values()` dos testes lê também os três campos (`app/chamados/tests/test_e1_entradas.py:282`). O patch acrescenta leitores das três colunas na constraint; os testes anexos novos escrevem colaborador/instância e consultam a constraint (`e1_indice_0052.patch`, referências de linhas exatas do arquivo completo [não verificado]).

Não encontrei `PerguntaDisputa.objects.create` de produção fora do `cls.objects.create` de `_nascer`; o exemplo fora da porta está no teste e é aceito sem índice (`app/chamados/models.py:2545`, `app/chamados/tests/test_e1_entradas.py:313`). A derivação no `save` protege inserções normais, mas não torna a chave imutável: só roda quando `_state.adding`; instância não vazia fornecida nem é validada contra o motivo (`app/chamados/models.py:2923`, `app/chamados/models.py:2939`). Escritor de produção que mude motivo/instância/conversa depois de nascer: [não verificado].

## 07/11 00:11 · sessao 793560f8174a · - · - · origem=Ronald (tmux)

4. **0051: estruturalmente compatível com o legado descrito; segurança do acervo real [não verificado].** Renomeia tabela/campos, atribui `reabertura` às decisões antigas e cria ligações de absorção nulas, compatíveis com os checks; preenche o colaborador pela disputa e a instância pelo motivo. O `SET CONSTRAINTS ALL IMMEDIATE` antecede o `ALTER ... NOT NULL`, evitando os eventos de FK pendentes que o comentário identifica. Referências: `app/chamados/migrations/0051_decisao_pergunta.py:25`, `:41`, `:49`, `:67`, `:76`, `:101`, `:104`, `:105`.

   **Fila — conversa antiga vira fato na coluna.** Todas recebem `conversa=False`, sem classificação histórica. Contraexemplo: duas conversas antigas do mesmo motivo/dia entram no grupo de colisão e impedem a 0052, embora conversas novas sejam excluídas. A própria migração declara essa limitação. Referências: `app/chamados/migrations/0051_decisao_pergunta.py:4`, `:98`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`.

   O congelado bate com o vivo: os quatro pares são exatamente `saida_sem_entrada→inicio`, `orfao_14h→fim`, `intervalo_saida→int_ida`, `intervalo_volta→int_volta`; há teste direto de igualdade. Referências: `app/chamados/migrations/0051_decisao_pergunta.py:14`; `app/chamados/services/declaracao_texto.py:42`, `:52`; `app/chamados/tests/test_e1_entradas.py:259`.

   **0052: aplicação segura no acervo [não verificado].** Não há resultados do DRY no dado fornecido. O patch exige zero colisões, enquanto a 0051 preserva conversas antigas como `False`. Referências: anexo `e1_indice_0052.patch`, migração adicionada, linhas 3–5 e 19–21; `app/chamados/migrations/0051_decisao_pergunta.py:98`.

5. **O command não escreve e consulta a autoridade correta.** Apenas consulta modelos e escreve em `stdout`; não possui flag de apply. Chama `competencia_entregue`, autoridade registrada como `_K6`, cujo código considera a exportação vigente não invalidada. Referências: `app/chamados/management/commands/backfill_decisoes_pergunta.py:25`, `:51`, `:67`, `:71`, `:76`; `app/core/juizes.py:312`; `app/folha/export.py:556`, `:580`.

   **Fila — o DRY não suporta data nula.** Contraexemplo: pergunta legada respondida, sem decisão e com `data_evento=None`; `linhas_a_criar` chama `competencia_do_fato(None)`, chegando a `None < None`. Duas perguntas nulas sem resposta também chegam a `dia.isoformat()` ao listar colisões. O campo permite nulo; ocorrência real [não verificado]. Referências: `app/chamados/models.py:2230`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:33`, `:45`, `:90`; `app/ponto/services/fechamento.py:1588`, `:1590`, `:1595`.

   O agrupamento por competência usa o adaptador existente e o corte da empresa, sem derivação própria. Referências: `app/chamados/management/commands/backfill_decisoes_pergunta.py:45`; `app/ponto/services/fechamento.py:1595`; `app/ponto/services/dia_pago.py:700`, `:714`.

9. A alteração de dependência corrige instalações novas: o `RunPython` pede `LogAuditoria`, criada em `core.0002`. A única mudança da 0057 está na dependência; suas operações permanecem no mesmo `RunPython`. Referências: `app/ponto/migrations/0057_a4_backfill_pendente.py:23`, `:70`, `:78`; `app/core/migrations/0002_log_auditoria.py:16`.

   Dada a premissa de migração já aplicada, a expectativa é que o executor não repita seu `RunPython` apenas pela nova dependência; comportamento do executor não está demonstrado pelo código da cópia [não verificado]. Estado real do histórico de migrações em produção [não verificado]. [nao verificado]

10. **Fila — a justificativa descreve a mudança, mas erra a unidade do custo.** O `+6` corresponde estruturalmente a duas chamadas contendo `atomic` + `SELECT ... FOR UPDATE`; os números medidos apresentados no comentário não foram verificados. O comentário diz custo por “[nome]”, mas a trava é adquirida antes do dedup e também custa quando retorna uma pergunta existente. Contraexemplo: emitir duas vezes o mesmo fato; a segunda chamada bloqueia/consulta e retorna `criada=False`, sem nascimento. Referências: `app/ponto/tests/test_selo_chokepoint_escrita.py:167`, `:172`, `:174`, `:181`; `app/chamados/models.py:2473`, `:2475`, `:2498`, `:2542`.

   “Conversa não trava” descreve corretamente a ausência do `SELECT FOR UPDATE`; o `atomic` envolve também a conversa. Referências: `app/chamados/models.py:2473`, `:2474`.

6. Dentro deste recorte, não apareceu segundo juiz: competência exportada chama `_K6`, competência do fato reaproveita o adaptador existente e instância é inversão do catálogo. Referências: `app/chamados/management/commands/backfill_decisoes_pergunta.py:28`, `:45`, `:71`; `app/core/juizes.py:312`; `app/chamados/services/declaracao_texto.py:52`.

   O teste de corrida comprova somente o caminho comum por `abrir`, com duas threads e a mesma identidade; seu título promete unicidade “NO BANCO”, mas o commit sem patch não possui essa constraint. Contraexemplo externo ao domínio do teste: inserção direta repetida, que o próprio teste do DRY constrói com sucesso esperado. Referências: `app/chamados/tests/test_e1_identidade_corrida.py:1`, `:49`, `:74`, `:81`; `app/chamados/tests/test_e1_entradas.py:309`, `:313`; `app/chamados/models.py:2942`.

## 06/11 23:35 · sessao 0c388599c44e · V2-BUGS-i2 · verificador · origem=Code

**Não aprovaria o encerramento da volta 3.** P1/P2/P3 foram corrigidos nos caminhos mensais correspondentes, mas P4 continua incompleto entre pares da mesma jornada. O helper novo também contém derivação civil, contrariando a justificativa do censo; leitores do export ainda usam outro dia. (`app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:3319`, `app/ponto/services/fechamento.py:942`, `app/ponto/motor_calculo_v2.py:1048`, `app/ponto/motor_calculo_v2.py:964`, `app/folha/porta_export.py:104`)

**1. Destino de P1–P5**

| Achado | Verificação |
|---|---|
| **P1 — dobra 12x36 civil** | **Curado no caminho mensal com chave:** o laço grava o dia antes de calcular o período, e a dobra agora o lê. Não elimina o fallback civil nem o segundo julgamento preexistente de pertença. (`app/ponto/motor_calculo_v2.py:2494`, `app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:2849`, `app/ponto/motor_calculo_v2.py:964`, `app/ponto/motor_calculo_v2.py:2443`) |
| **P2 — FT comercial sem dobra/split** | **Curado nesse caminho:** o corpo comercial consulta o dia mapeado, escreve a dobra e depois aplica o split. A cauda exclusivamente posterior às 05h ainda perde prorrogação herdada de outro par. (`app/ponto/motor_calculo_v2.py:3319`, `app/ponto/motor_calculo_v2.py:3324`, `app/ponto/motor_calculo_v2.py:1048`) |
| **P3 — filtro de escala certa civil** | **Curado na origem do filtro:** filtro e promoção consultam `_dia_de` com o mesmo mapa. (`app/ponto/services/fechamento.py:940`, `app/ponto/services/fechamento.py:942`, `app/ponto/services/fechamento.py:954`) |
| **P4 — HE noturna prorrogada** | **Parcial:** recupera o contexto do próprio par; não recupera o contexto da jornada composta por outros pares. (`app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:1048`) |
| **P5 — testes forçavam escala certa** | **Curado para a fronteira pedida:** F11i responde conforme o dia recebido e exige sete horas de feriado. Não valida os critérios internos reais de escala certa nem cobra o noturno nessa variante. (`app/ponto/tests/test_o280_bateria_da_lei.py:1819`, `app/ponto/tests/test_o280_bateria_da_lei.py:1918`, `app/ponto/tests/test_o280_bateria_da_lei.py:1919`) |

**2. Escritores, leitores e significado dos campos**

O mapa novo não contém exclusivamente a resposta do juiz registrado: `_dia_do_turno` lê `data_turno`, mas o laço pode substituí-lo pela herança de até seis horas. Partido e comercial depois remontam agrupamentos diretamente pelo mapa do juiz. Existem, portanto, duas procedências para “dia da jornada”. (`app/ponto/motor_calculo_v2.py:909`, `app/ponto/motor_calculo_v2.py:2439`, `app/ponto/motor_calculo_v2.py:2443`, `app/ponto/motor_calculo_v2.py:2982`, `app/ponto/motor_calculo_v2.py:3382`)

| Dado | Quem escreve e quem lê |
|---|---|
| **`data_jornada` / mapa por entrada** | O laço calcula `data_jornada`, grava `_dia_jornada_do_par[localizar(entrada)]` e chama `calcular_periodo`. O helper lê esse mapa nos corpos 12x36/comercial. (`app/ponto/motor_calculo_v2.py:2440`, `app/ponto/motor_calculo_v2.py:2494`, `app/ponto/motor_calculo_v2.py:2495`, `app/ponto/motor_calculo_v2.py:964`, `app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:3319`) |
| **Mapa por período** | A base grava `id(periodo) → data_jornada`; `_aplicar_teto_pontualidade` publica/mescla em `resultado.dia_da_jornada`. Partido também publica mapa próprio; fechamento mescla mapas das fatias e entrega o resultado ao DiaPago. (`app/ponto/motor_calculo_v2.py:2497`, `app/ponto/motor_calculo_v2.py:1108`, `app/ponto/motor_calculo_v2.py:3012`, `app/ponto/services/fechamento.py:315`, `app/ponto/services/fechamento.py:1225`) |
| **`minutos_extra_100_feriado`** | Escrito nas dobras por período, FT partido e dobras diárias repartidas. ResultadoMes soma regulares; fechamento acrescenta trabalhado dos FT promovidos; DiaPago lê regulares e promove FT na sua linha. (`app/ponto/motor_calculo_v2.py:2852`, `app/ponto/motor_calculo_v2.py:3007`, `app/ponto/motor_calculo_v2.py:3155`, `app/ponto/motor_calculo_v2.py:3327`, `app/ponto/motor_calculo_v2.py:3414`, `app/ponto/motor_calculo_v2.py:337`, `app/ponto/services/fechamento.py:962`, `app/ponto/services/dia_pago.py:183`, `app/ponto/services/dia_pago.py:250`) |
| **`minutos_extra_100_noturna`** | `he_noturna` calcula; `_aplicar_he_noturna` escreve retorno positivo. Resets/bloqueio zeram. ResultadoMes soma regulares; FM e DiaPago acrescentam o campo dos FT promovidos. (`app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:1053`, `app/ponto/motor_calculo_v2.py:1509`, `app/ponto/motor_calculo_v2.py:2978`, `app/ponto/motor_calculo_v2.py:349`, `app/ponto/services/fechamento.py:970`, `app/ponto/services/dia_pago.py:252`) |
| **Noturno relógio/legal** | Não existe campo genérico `minutos_noturnos` no período: há relógio e legais. `_escrever_noturnas` recebe a conta; legais aplicam o fator. ResultadoMes inclui regulares e FT; FM e DiaPago leem esses valores. (`app/ponto/motor_calculo_v2.py:219`, `app/ponto/motor_calculo_v2.py:220`, `app/ponto/motor_calculo_v2.py:1016`, `app/ponto/motor_calculo_v2.py:1033`, `app/ponto/motor_calculo_v2.py:326`, `app/ponto/services/fechamento.py:918`, `app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:224`) |
| **Persistência FM/DiaPago** | FM persiste no fechamento; `lavrar` substitui as linhas da versão do DiaPago. A restauração também pode atualizar somente FM. (`app/ponto/services/fechamento.py:1152`, `app/ponto/services/fechamento.py:1168`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/fechamento.py:1400`) |

**Os leitores não compartilham integralmente o mesmo dia:**

| Leitor | Contrato encontrado |
|---|---|
| **Fechamento/DiaPago** | Filtro, promoção e lavratura usam o mapa da jornada; `_dia_de` também possui fallback civil se faltar mapa/chave. (`app/ponto/services/fechamento.py:942`, `app/ponto/services/fechamento.py:954`, `app/ponto/services/dia_pago.py:129`, `app/ponto/services/dia_pago.py:132`, `app/ponto/services/dia_pago.py:245`) |
| **Calendário** | Dinheiro vem do DiaPago por data; geometria vem de `data_turno`. Pode divergir da base quando esta sobrescreve a resposta do juiz pela herança de seis horas. (`app/colaboradores/services/calendario.py:221`, `app/colaboradores/services/calendario.py:224`, `app/colaboradores/services/calendario.py:655`, `app/ponto/motor_calculo_v2.py:2443`) |
| **Tela/espelho/PDF** | Cards usam o juiz do dia; rubricas diárias leem DiaPago. O resumo provisório do espelho ainda recorta períodos pela data civil; o PDF chama essa soma sem o recorte. (`app/ponto/services/espelho.py:458`, `app/ponto/services/espelho.py:813`, `app/ponto/services/espelho.py:84`, `app/ponto/services/espelho.py:439`, `app/relatorios/pdf_espelho.py:502`, `app/relatorios/pdf_espelho.py:837`) |
| **`porta_export`** | Uma testemunha agrega pela data civil local; outra pela data crua de entrada. Não seguem a atribuição por jornada do escritor. (`app/folha/porta_export.py:104`, `app/folha/porta_export.py:538`) |
| **Calculador** | A alimentação exige mapa por período; sem ele declara `sem_dia_da_jornada` e ignora o período. O split chama a mesma `he_noturna`, por par, preservando a limitação de contexto. (`app/ponto/calculador/alimentacao.py:78`, `app/ponto/calculador/alimentacao.py:81`, `app/ponto/calculador/regras.py:827`, `app/ponto/calculador/regras.py:832`) |
| **TXT** | Lê FM: HE100 normal/feriado e adicional noturno total. Não emite evento separado para HE100 noturna; alteração somente nesse subconjunto não muda esses eventos. (`app/folha/export.py:420`, `app/folha/export.py:424`, `app/folha/export.py:443`, `app/folha/export.py:447`) |

**3. O fallback é derivação paralela, apesar do comentário**

Quando a chave falta, `_dia_da_jornada_do_par` devolve a data civil local da entrada. Não consulta o mapa do juiz disponível na instância, não declara ausência de atribuição e não demonstra que aquele par constitui sozinho uma jornada. (`app/ponto/motor_calculo_v2.py:964`, `app/ponto/motor_calculo_v2.py:2433`)

**Contraexemplo reproduzido em memória:** entrada em **07/10 às 06h**, mapa novo vazio e `_dia_turno_juiz_calc` atribuindo essa entrada a **06/10**. O helper retorna **07/10**. Assim, sua resposta pode contradizer uma atribuição já disponível; “não decide dia nenhum” é falso nesse ramo. (`app/ponto/motor_calculo_v2.py:964`, `app/ponto/tests/test_contract_juiz_batida.py:329`)

No caminho mensal normal, a chave é preenchida antes da chamada. Testes que instanciam motor e chamam `calcular_periodo` diretamente alcançam o fallback. Existe ainda chamada produtiva auxiliar no crédito de aberto, fora daquele preenchimento; sua entrada só cai no fallback se não tiver sido previamente mapeada. Fixture produtiva integrada comprovando essa ausência: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:2494`, `app/ponto/motor_calculo_v2.py:2495`, `app/ponto/tests/test_12x36_com_escala_noturna.py:38`, `app/ponto/tests/test_12x36_com_escala_noturna.py:41`, `app/ponto/motor_calculo_v2.py:2590`)

O auxiliar empresta somente trabalhado e fora da janela; sua dobra/split não são copiados. Portanto esse chamador, isoladamente, não prova dinheiro novo incorreto. (`app/ponto/motor_calculo_v2.py:2591`, `app/ponto/motor_calculo_v2.py:2592`, `app/ponto/motor_calculo_v2.py:2596`)

**4. P4 fora de feriado**

A mudança não recebe nem consulta feriado. A reprodução abaixo extraiu funções por AST e restaurou em memória somente o trecho anterior de `he_noturna`; não executou o commit pai completo. (`app/ponto/motor_calculo_v2.py:414`, `app/ponto/motor_calculo_v2.py:445`, `app/ponto/motor_calculo_v2.py:453`)

| Par / condição | HE100 noturna: corpo anterior → atual |
|---|---:|
| 22–07, HE100 final de 60, prorrogação ligada | **0 → 60**. Correção no contexto do próprio par. (`app/ponto/motor_calculo_v2.py:453`) |
| 22–07, HE100 final de 60, pausa 06–06h15 | **0 → 60**; o trecho recua por trabalho e desconta a pausa. (`app/ponto/motor_calculo_v2.py:445`, `app/ponto/tests/test_o280_bateria_da_lei.py:2003`) |
| 22–07, HE100 final de 60, prorrogação desligada | **0 → 0**. (`app/ponto/tests/test_o280_bateria_da_lei.py:2005`) |
| Cauda própria 06–07, HE100 de 60 | **0 → 0**, mesmo quando deveria continuar a jornada noturna do par anterior. (`app/ponto/motor_calculo_v2.py:114`, `app/ponto/motor_calculo_v2.py:1048`) |
| 22–01 dois dias depois, 27h, HE100 final de 60 | **60 → 0**, inclusive sem prorrogação. O zero decorre do teto do recorte inteiro. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:201`, `app/ponto/motor_calculo_v2.py:453`) |

**O resto de P4 é concreto:** numa jornada com par noturno até 05h e cauda 06–07, herdar o dia não transporta a noturnidade. Tanto o adicional total quanto o split continuam medidos por pares isolados. Pela regra fornecida de prorrogação da jornada, a cauda de 60 minutos permanece sem os 60 devidos. (`app/ponto/motor_calculo_v2.py:2444`, `app/ponto/motor_calculo_v2.py:1013`, `app/ponto/motor_calculo_v2.py:1048`, `app/ponto/calculador/regras.py:85`)

Para **mais de 26h**, o zero é uma guarda de sanidade, não demonstração do valor da regra: a função zera o todo e limita a parte a esse zero. O pareamento normal declara teto de 26h; ocorrência produtiva além dele: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:201`, `app/ponto/motor_calculo_v2.py:209`)

Para **Londrina cl.38-d**, o afastamento da redução troca o fator para 1; não elimina minutos noturnos de relógio nem decide prorrogação. O split de 60 permanece 60 quando a prorrogação está ligada. A configuração concreta dos flags na produção fornecida: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:1022`, `app/ponto/motor_calculo_v2.py:1051`, `app/core/regua_cct.py:466`, `app/core/regua_cct.py:470`)

**5. Sítios restantes e dinheiro**

- **Testemunha do export por dia civil:** no desenho F11h, compara períodos **3h em d / 4h em d+1** contra DiaPago **7h em d / 0h em d+1**. Acusa diferença de atribuição como divergência de quantidade. Essa reconciliação permanece fora de `falhas`. (`app/folha/porta_export.py:104`, `app/ponto/services/dia_pago.py:245`, `app/ponto/tests/test_o280_bateria_da_lei.py:1889`, `app/folha/porta_export.py:696`)
- **Invariante HE/intra por data crua:** consulta previsto e intervalo no dia de `_p.entrada.date()`, que pode inclusive ser UTC. A redistribuição por par pode mudar a acusação; esse contador integra `falhas` e pode afetar a liberação do TXT. Ocorrência na frota: **[não verificado]**. (`app/folha/porta_export.py:538`, `app/folha/porta_export.py:548`, `app/folha/porta_export.py:560`, `app/folha/porta_export.py:689`)
- **Alerta com dia errado:** decide folga semanal por `data_jornada`, mas imprime o dia da semana da entrada civil. Cauda de segunda pertencente à jornada de domingo pode dizer “folga semanal (Segunda)”. Aqui o efeito direto é textual. (`app/ponto/motor_calculo_v2.py:2511`, `app/ponto/motor_calculo_v2.py:2516`, `app/ponto/motor_calculo_v2.py:2521`)
- **Janela anterior à dobra ainda civil:** empresa e marcos são consultados por `entrada.date()`. Corrigir o dia da dobra não corrige esse recorte anterior do par efetivo, que participa da duração paga. Contraexemplo integrado novo: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:1562`, `app/ponto/motor_calculo_v2.py:1570`, `app/ponto/motor_calculo_v2.py:2797`)
- **Banco/DSR comerciais:** ainda usam data civil para extras/presença/denominador semanal. DSR subtrai HE100 feriado, conservando zero para a dobra repartida; os demais usos continuam temporalmente divergentes. Efeito concreto na frota: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:3502`, `app/ponto/motor_calculo_v2.py:3520`, `app/ponto/motor_calculo_v2.py:3543`, `app/ponto/motor_calculo_v2.py:3549`)

Há também uma divergência preexistente com a formulação **literal e sem exceção** do pedido: o código continua condicionando dobra a `feriado_em_dobra`, e a fábrica desliga esse flag para 12x36 por padrão; o TXT também suprime a parcela nesse regime sem a cláusula. Logo a cópia não implementa universalmente “feriado trabalhado = HE100”. (`app/ponto/motor_calculo_v2.py:949`, `app/ponto/motor_calculo_v2.py:3678`, `app/ponto/motor_calculo_v2.py:3749`, `app/folha/export.py:422`)

**6. Mordida dos REDs e censo**

Nenhum dos seis REDs aceita qualquer resultado: `confere` e `cobra_a_lei` usam igualdade dos campos esperados. Isso também significa que campos omitidos da expectativa não são certificados. (`app/ponto/tests/test_o280_bateria_da_lei.py:1309`, `app/ponto/tests/test_o280_bateria_da_lei.py:1321`)

| RED | Valor exigido / limite |
|---|---|
| **F10d/e** | Cobram zero de feriado; F10e saiu de `expectedFailure`. Invertem efetivamente a proteção dos 90 indevidos. (`app/ponto/tests/test_o280_bateria_da_lei.py:1593`, `app/ponto/tests/test_o280_bateria_da_lei.py:1596`, `app/ponto/tests/test_o280_bateria_da_lei.py:1608`) |
| **F10f** | Cobra 660 de dobra, incluindo a cauda. **Não cobra o noturno da cauda**, portanto deixa passar o resto de P4. (`app/ponto/tests/test_o280_bateria_da_lei.py:1624`) |
| **F10g** | Exige uma cauda distinta em FT e cobra 120 trabalhados/feriado/noturna. (`app/ponto/tests/test_o280_bateria_da_lei.py:1654`, `app/ponto/tests/test_o280_bateria_da_lei.py:1657`) |
| **F11i** | Cobra 7h de feriado com resposta de escala certa dependente do dia recebido. Não cobra noturno nessa variante. (`app/ponto/tests/test_o280_bateria_da_lei.py:1918`, `app/ponto/tests/test_o280_bateria_da_lei.py:1919`) |
| **N-PRORROG** | Cobra **60/60/0**. Na reprodução pura, o corpo anterior reconstruído deu **0/0/0**: há mordida numérica demonstrada. Só cobre contexto dentro do mesmo par. (`app/ponto/tests/test_o280_bateria_da_lei.py:2001`, `app/ponto/tests/test_o280_bateria_da_lei.py:2006`, `app/ponto/motor_calculo_v2.py:453`) |

F10d/e/f/g e F11i exercitam os ramos alterados; execução integrada desses REDs no pai completo e na cópia atual: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:3319`, `app/ponto/services/fechamento.py:942`)

**O censo 10→11 é aritmeticamente legítimo para seu contrato estrutural**, que conta métodos privados e vigia portas públicas do dia. **Não legitima a justificativa semântica:** o contador examina nomes, não se o método deriva datas; passa mesmo com o fallback civil. “Não decide dia nenhum” encobre justamente esse ramo. (`app/ponto/tests/test_contract_juiz_batida.py:140`, `app/ponto/tests/test_contract_juiz_batida.py:319`, `app/ponto/tests/test_contract_juiz_batida.py:329`, `app/ponto/tests/test_contract_juiz_batida.py:330`, `app/ponto/motor_calculo_v2.py:964`)

**7. Classes estruturais**

- **Segundo juiz / derivação paralela:** permanece a sobrescrita por até seis horas; o helper novo acrescenta derivação civil quando falta chave. A pertença já tem autoridade registrada. (`app/ponto/motor_calculo_v2.py:2443`, `app/ponto/motor_calculo_v2.py:964`, `app/core/juizes.py:196`)
- **Campo com dois significados — F1 permanece:** v2 corrigido deposita a quantidade própria em cada par; `reler` zera os pais HE, preserva splits e recoloca toda dobra no último. O primeiro par do NPART pode terminar com **HE100=0 e HE100 noturna=120**. Efeito produtivo dessa releitura: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:3151`, `app/ponto/calculador/periodo.py:761`, `app/ponto/calculador/periodo.py:770`, `app/ponto/calculador/periodo.py:841`)
- **Porta de escrita:** as alterações de FM/DiaPago permanecem nos serviços existentes. O risco preexistente de persistir FM e capturar falha posterior do DiaPago continua. (`app/core/portas.py:500`, `app/ponto/services/fechamento.py:1152`, `app/ponto/services/fechamento.py:1207`, `app/ponto/services/fechamento.py:1246`, `app/ponto/services/dia_pago.py:359`)
- **Selo sem mordida:** os novos REDs têm mordida nos campos cobrados; o censo não prova ausência de juiz semântico, e F10f deixa descoberta a prorrogação da cauda. (`app/ponto/tests/test_o280_bateria_da_lei.py:1309`, `app/ponto/tests/test_o280_bateria_da_lei.py:1624`, `app/ponto/tests/test_contract_juiz_batida.py:140`)
- **Relógio/travas:** os trechos novos usam instantes recebidos, sem nova leitura de “agora”. Ordem de travas e execução concorrente completa: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:452`, `app/ponto/motor_calculo_v2.py:963`, `app/ponto/services/fechamento.py:942`)

A validação executada foi estática e por funções puras em memória. Suíte Django, pai completo, banco, logs e novo DIFF de frota/TXT sobre **b6b1e3d5**, especialmente fora de feriado: **[não verificado]**.

## 06/11 23:32 · sessao fbda007c5eeb · - · - · origem=Ronald (tmux)

P1/P2 corrigem os dois sítios por período quando o laço mensal forneceu a chave: mapa preenchido antes da chamada em `app/ponto/motor_calculo_v2.py:2494`; 12x36 lê em `:2835` e dobra em `:2849`; comercial lê em `:3319` e dobra em `:3324`. A correção não elimina a derivação paralela preexistente: a base sobrescreve o dia do juiz pela herança de intervalo ≤6h em `:2439–2444`; partido remonta seu dia pelo juiz em `:2982`, inclusive FT em `:2995`; comercial refaz o agrupamento dos regulares em `:3382`. Portanto “sempre o mesmo juiz” não está demonstrado pelo código. (`app/ponto/motor_calculo_v2.py:2439`, `app/ponto/motor_calculo_v2.py:2443`, `app/ponto/motor_calculo_v2.py:2982`, `app/ponto/motor_calculo_v2.py:3382`)

P4 é parcial: cura a perda da prorrogação dentro do próprio par, mas não transporta contexto noturno entre pares da mesma jornada. A casca continua entregando somente as pontas efetivas daquele período; data herdada não leva informação de prorrogação. (`app/ponto/motor_calculo_v2.py:1048`, `app/ponto/motor_calculo_v2.py:2444`)

Reproduzi as funções por extração AST e execução em memória, sem Django/arquivos. “Pai reconstruído” abaixo restaura somente o corpo anterior de `he_noturna` fornecido no diff; não é execução do commit pai inteiro. (`app/ponto/motor_calculo_v2.py:445`, `app/ponto/motor_calculo_v2.py:453`)

| Entrada/saída, prorrogação ligada | HE100 | Pai reconstruído → atual |
|---|---:|---:|
| 22–07 seguinte | 60 | 0 → 60 |
| 22–07 seguinte | 120 | 0 → 120 |
| 00–07 | 120 | 0 → 120 |
| 04–07 | 120 | 0 → 120 |
| Cauda própria 06–07 | 60 | 0 → 0 |
| 22–01, dois dias depois, 27h | 60 | 60 → 0 |
| 22–06, dois dias depois, 32h | 120 | 120 → 0 |

As mudanças independem de feriado; a função não recebe esse atributo. Os quatro primeiros resultados vêm da medição do trecho no contexto do próprio par. A cauda própria permanece zero porque começa depois das 05h e não contém o gatilho 00–05; caso seja continuação da jornada noturna anterior, falta o contexto exigido pela regra do pedido. (`app/ponto/motor_calculo_v2.py:414`, `app/ponto/motor_calculo_v2.py:114`, `app/ponto/motor_calculo_v2.py:453`)

Nos pares >26h o zero novo vem da guarda de sanidade, não de comprovação do valor legal: o noturno do todo é zerado e o medidor da parte o limita a esse zero. O pareamento normal declara teto de 26h; ocorrência produtiva além desse teto: [não verificado]. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:201`, `app/ponto/motor_calculo_v2.py:204`)

Londrina cl.38-d não exige redução do split: ele mede minutos de relógio. O afastamento da hora reduzida altera o fator para 1; a prorrogação é outro parâmetro. Para 22–07 com HE final 60, o split atual é 60 se prorrogação ligada, zero se desligada; o fator da cl.38-d não modifica essa classificação. A configuração produtiva concreta desses flags não foi fornecida. (`app/ponto/motor_calculo_v2.py:1022`, `app/ponto/motor_calculo_v2.py:1051`, `app/core/regua_cct.py:466`, `app/core/regua_cct.py:470`; configuração: [não verificado])

O fallback novo **deriva dia civil** quando falta a chave, inclusive quando existe mapa da autoridade disponível na instância: ele não consulta `_dia_turno_juiz_calc`, apenas `_dia_jornada_do_par`. Contraexemplo puro: entrada `13/10 06h`, mapa novo vazio, mapa do juiz atribuindo essa entrada a `12/10`; o helper devolve `13/10`. Logo a afirmação documental “não decide dia nenhum” omite esse ramo. (`app/ponto/motor_calculo_v2.py:964`, `app/ponto/motor_calculo_v2.py:2433`, `app/ponto/tests/test_contract_juiz_batida.py:329`)

Há chamador produtivo fora do laço preenchedor: os pares auxiliares do crédito de aberto passam por `calcular_periodo` em `:2590`. Ele só cai no fallback se sua entrada não foi entrada de um período previamente registrado em `:2494`; não reproduzi um fixture integrado que demonstre essa ausência. O crédito copia somente trabalhado/fora da janela, descartando a HE/noturno calculados no auxiliar; não atribuir efeito monetário à dobra auxiliar sem prova adicional. (`app/ponto/motor_calculo_v2.py:2494`, `app/ponto/motor_calculo_v2.py:2590`, `app/ponto/motor_calculo_v2.py:2591`, `app/ponto/motor_calculo_v2.py:2596`; fixture integrado: [não verificado])

Chamadas diretas dos testes chegam ao fallback porque constroem o motor e chamam `calcular_periodo` sem `calcular_mes`; exemplos concretos em `app/ponto/tests/test_12x36_com_escala_noturna.py:38` e `:41`, além de `:50` e `:53`. O censo 10→11 é coerente com a pergunta estrutural “quantos ajudantes privados do dia existem”, mas não certifica ausência de derivador paralelo: o contrato cobra contagem/classe e ausência de porta pública; seu comentário adicional de pureza é falso no fallback. (`app/ponto/tests/test_contract_juiz_batida.py:319`, `app/ponto/tests/test_contract_juiz_batida.py:330`, `app/ponto/tests/test_contract_juiz_batida.py:332`, `app/ponto/motor_calculo_v2.py:964`)

Restam leitores civis monetários no próprio comercial: extras semanais do banco usam a data civil da entrada; DSR reconstrói presença, semana das extras e denominador de dias trabalhados pela mesma data civil. Assim não seguem integralmente o dia que o motor escreveu no mapa. Um par de domingo à noite cuja cauda cai na segunda pode atravessar a fronteira semanal nesses leitores. Efeito na frota fornecida: [não verificado]. (`app/ponto/motor_calculo_v2.py:3502`, `app/ponto/motor_calculo_v2.py:3520`, `app/ponto/motor_calculo_v2.py:3545`, `app/ponto/motor_calculo_v2.py:3549`)

## 06/11 23:32 · sessao b9ba20b70125 · - · - · origem=Ronald (tmux)

Achados e mapa de leitores, somente pela cópia:

1. **P3 está curado no sítio:** filtro de escala certa e promoção usam `_dia_de(p, resultado.dia_da_jornada)`, antes de separar FT certa/feriado/sem escala. A mesma partição segue para `lavrar`. (`app/ponto/services/fechamento.py:941`, `app/ponto/services/fechamento.py:954`, `app/ponto/services/fechamento.py:988`, `app/ponto/services/fechamento.py:1211`)

2. **A testemunha da exportação continua comparando dias diferentes.** `dias_fora_do_dia_pago` soma períodos pela data civil local da entrada; DiaPago foi agrupado por jornada. No F11h, períodos de 3h em d e 4h em d+1 produzem espelho civil 3h/4h versus DiaPago 7h/0h, acusando divergência apesar da atribuição correta ao turno. Esse contador continua fora de `falhas`. (`app/folha/porta_export.py:104`, `app/ponto/services/dia_pago.py:245`, `app/ponto/tests/test_o280_bateria_da_lei.py:1889`, `app/folha/porta_export.py:696`)

3. **Outro invariante do export usa data civil crua:** `_p.entrada.date()` agrupa HE/intra/trabalhado, podendo inclusive usar dia UTC; previsto e intervalo declarado são consultados nessa data. A redistribuição da dobra de um portador para vários períodos pode mudar essa acusação. Ele integra `falhas`, portanto pode afetar a liberação do TXT; ocorrência na frota é [não verificado]. (`app/folha/porta_export.py:538`, `app/folha/porta_export.py:548`, `app/folha/porta_export.py:560`, `app/folha/porta_export.py:689`)

4. **Alerta ainda mistura dia julgado e dia civil:** a escolha “folga semanal” consulta `data_jornada`, mas imprime o nome do dia pelo `weekday()` da entrada do par. Cauda de segunda pertencente à jornada de domingo pode dizer “folga semanal (Segunda)”. Isso muda texto, sem escrita direta de rubrica nesse ramo. (`app/ponto/motor_calculo_v2.py:2511`, `app/ponto/motor_calculo_v2.py:2516`, `app/ponto/motor_calculo_v2.py:2521`)

5. **Janela continua civil antes da dobra:** `_janela_do_dia` consulta empresa e marcos por `entrada.date()`. A dobra agora usar o mapa não corrige esse recorte anterior do par efetivo. Trata-se da mesma inconsistência temporal, embora a pergunta ali seja janela/marcos, não identificação de feriado; pode alterar duração e HE. O próprio calculador documenta pares invertidos por essa ancoragem, mas os números comentados não foram reproduzidos nesta análise: [não verificado]. (`app/ponto/motor_calculo_v2.py:1562`, `app/ponto/motor_calculo_v2.py:1570`, `app/ponto/calculador/periodo.py:660`)

**Escritores e leitores dos campos**

| Campo | Escrita e leitura |
|---|---|
| `minutos_extra_100_feriado` | Dobras por período 12x36/comercial; FT partido; dobras diárias repartidas. ResultadoMes soma regulares; fechamento acrescenta FT promovida. DiaPago lê regulares e, para FT feriado, promove trabalhado. (`app/ponto/motor_calculo_v2.py:2852`, `app/ponto/motor_calculo_v2.py:3007`, `app/ponto/motor_calculo_v2.py:3155`, `app/ponto/motor_calculo_v2.py:3327`, `app/ponto/motor_calculo_v2.py:3414`, `app/ponto/motor_calculo_v2.py:337`, `app/ponto/services/fechamento.py:962`, `app/ponto/services/dia_pago.py:183`, `app/ponto/services/dia_pago.py:250`) |
| `minutos_extra_100_noturna` | Casca `_aplicar_he_noturna` escreve retorno positivo de `he_noturna`; resets e bloqueio zeram. ResultadoMes soma regulares; fechamento e DiaPago acrescentam campo dos FT promovidos. (`app/ponto/motor_calculo_v2.py:1048`, `app/ponto/motor_calculo_v2.py:1053`, `app/ponto/motor_calculo_v2.py:1509`, `app/ponto/motor_calculo_v2.py:2978`, `app/ponto/motor_calculo_v2.py:349`, `app/ponto/services/fechamento.py:970`, `app/ponto/services/dia_pago.py:252`) |
| Noturno relógio/legal | `_escrever_noturnas` lê `calcular_noturnas`; legais = relógio × fator. ResultadoMes soma regulares e FT; FM recebe o total; DiaPago soma os campos nos respectivos dias. Não há um campo genérico `minutos_noturnos` no dataclass. (`app/ponto/motor_calculo_v2.py:219`, `app/ponto/motor_calculo_v2.py:220`, `app/ponto/motor_calculo_v2.py:1013`, `app/ponto/motor_calculo_v2.py:1016`, `app/ponto/motor_calculo_v2.py:1033`, `app/ponto/motor_calculo_v2.py:326`, `app/ponto/services/fechamento.py:918`, `app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:224`) |
| FM / DiaPago persistidos | FM recebe totais e persiste por `save`/`update`; `lavrar` apaga/recria a versão do DiaPago. Falha posterior da lavratura é capturada após persistência do FM. Restauração atualiza somente FM. (`app/ponto/services/fechamento.py:1152`, `app/ponto/services/fechamento.py:1168`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/fechamento.py:1246`, `app/ponto/services/fechamento.py:1400`) |

**Os leitores não compartilham integralmente o mesmo contrato de dia.**

- Calendário lê dinheiro do DiaPago por data; suas batidas/cards vêm de `turnos_do_colab` pela `CargaDaJanela`, agrupados por `t.data_turno`, não por data civil. Assim, quando a base sobrescreve o dia do juiz pela herança de seis horas, dinheiro e geometria podem ter origens diferentes. (`app/colaboradores/services/calendario.py:221`, `app/colaboradores/services/calendario.py:224`, `app/colaboradores/services/calendario.py:610`, `app/colaboradores/services/calendario.py:655`, `app/ponto/motor_calculo_v2.py:2443`)
- Espelho também agrupa cards pelo juiz `dia_das_batidas`, mas lê rubricas do DiaPago. Seu resumo provisório ainda recorta períodos pela data civil local; o PDF chama a mesma soma sem esse recorte. (`app/ponto/services/espelho.py:450`, `app/ponto/services/espelho.py:458`, `app/ponto/services/espelho.py:813`, `app/ponto/services/espelho.py:84`, `app/ponto/services/espelho.py:439`, `app/relatorios/pdf_espelho.py:502`)
- Tela diária e PDF imprimem rubricas lavradas; não remontam o split. O PDF ainda consulta os períodos para decidir presença de coluna. (`app/templates/ponto/espelho.html:268`, `app/templates/ponto/espelho.html:283`, `app/relatorios/pdf_espelho.py:837`, `app/relatorios/pdf_espelho.py:854`)
- TXT lê FM: HE100 normal/feriado e adicional noturno total. Não emite evento separado para HE100 noturna; portanto mudanças apenas no subconjunto não alteram essas linhas do TXT. Mudança de `horas_noturnas` altera a entrada do adicional. (`app/folha/export.py:420`, `app/folha/export.py:424`, `app/folha/export.py:443`, `app/folha/export.py:447`)

**V3 conserva campo com dois significados, já F1 da fila.** A alimentação lê dia da jornada e pares portadores de HE; sem mapa declara contador e ignora o período. `reler`, porém, zera pais HE em todos os pares, preserva splits e recoloca a dobra inteira no último. No NPART corrigido, o primeiro pode terminar HE100=0 e HE100 noturna=120. É subconjunto maior que pai por período. Chamadas encontradas de `reler` na cópia são testes; efeito monetário produtivo decorrente é [não verificado]. (`app/ponto/calculador/alimentacao.py:78`, `app/ponto/calculador/alimentacao.py:102`, `app/ponto/calculador/periodo.py:611`, `app/ponto/calculador/periodo.py:761`, `app/ponto/calculador/periodo.py:770`, `app/ponto/calculador/periodo.py:841`, `app/ponto/tests/test_o280_bateria_da_lei.py:4678`)

Banco e DSR ainda agrupam extras por data civil; DSR subtrai HE100 feriado, conservando zero para a dobra repartida. A6 ainda conta ocorrências sem deduplicar dia. Esses restos já constam da fila fornecida; impacto concreto na frota é [não verificado]. (`app/ponto/motor_calculo_v2.py:3502`, `app/ponto/motor_calculo_v2.py:3543`, `app/ponto/motor_calculo_v2.py:3545`, `app/ponto/services/esmeril_espelho.py:177`, `app/ponto/services/esmeril_espelho.py:179`)

## 06/11 23:31 · sessao 9f3eff5d28e6 · - · - · origem=Ronald (tmux)

Os seis REDs cobram valores explícitos; nenhum aceita qualquer resultado ou exceção: `confere` compara o subconjunto esperado por igualdade, e `cobra_a_lei` faz a mesma comparação diretamente. (`app/ponto/tests/test_o280_bateria_da_lei.py:1306`, `app/ponto/tests/test_o280_bateria_da_lei.py:1321`)

| RED | Valor exigido e mordida |
|---|---|
| F10d | Dois pares, 660 trabalhados, HE100/feriado zero; troca a caracterização indevida de 90 pela regra. (`app/ponto/tests/test_o280_bateria_da_lei.py:1593`) |
| F10e | HE100 feriado zero, sem `expectedFailure`; é praticamente a mesma fronteira de F10d, com uma única rubrica. (`app/ponto/tests/test_o280_bateria_da_lei.py:1596`, `app/ponto/tests/test_o280_bateria_da_lei.py:1608`) |
| F10f | Dois pares, 660 trabalhados e 660 de HE100/feriado. **Não cobra noturno da cauda**; portanto pode passar mesmo que a prorrogação herdada continue perdida. (`app/ponto/tests/test_o280_bateria_da_lei.py:1624`) |
| F10g | Exige exatamente uma cauda em FT e cobra 120 trabalhados/feriado/HE100 noturna nela. A fixture exercita efetivamente um par distinto após meia-noite. (`app/ponto/tests/test_o280_bateria_da_lei.py:1654`, `app/ponto/tests/test_o280_bateria_da_lei.py:1657`) |
| F11i | A autoridade simulada responde `dia == fer`; exige FM feriado 7h, folga zero e DiaPago feriado 7h. Exercita a data passada ao filtro, mas **não cobra noturno**, nem valida os critérios reais internos de `escala_certa_no_dia`. (`app/ponto/tests/test_o280_bateria_da_lei.py:1819`, `app/ponto/tests/test_o280_bateria_da_lei.py:1918`, `app/ponto/tests/test_o280_bateria_da_lei.py:1919`) |
| N-PRORROG | Cobra `(60,60,0)` para prorrogação ligada sem pausa, ligada com pausa e desligada. Cobre contexto interno do mesmo par 22–07; não cobre prorrogação herdada de outro par. (`app/ponto/tests/test_o280_bateria_da_lei.py:2001`, `app/ponto/tests/test_o280_bateria_da_lei.py:2006`) |

Reproduzi em memória as funções puras, sem importar Django nem gravar arquivo. O corpo atual deu **`(60,60,0)`**; recolocando somente o trecho anterior de `he_noturna`, reconstruído pelo diff fornecido, deu **`(0,0,0)`**. Isso demonstra mordida numérica de N-PRORROG no corpo anterior, **não execução do pai inteiro**. (`app/ponto/motor_calculo_v2.py:445`, `app/ponto/motor_calculo_v2.py:453`, `app/ponto/tests/test_o280_bateria_da_lei.py:2006`; execução do pai inteiro: **[não verificado]**)

Para F10d/e/f/g e F11i, os pontos alterados correspondem à condição exercitada pelos REDs; execução integrada no pai e na cópia atual: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:3319`, `app/ponto/services/fechamento.py:942`)

**Censo 10 → 11:** reproduzi por AST **11 privados, 35 públicos e 19 portas**. O incremento é aritmeticamente legítimo para o contrato efetivo de ausência de nova **porta pública do DIA**; `_dia_da_jornada_do_par` é privado e recebe o par. O teste conta nomes que contêm `dia`/`jornada`, não examina a semântica de suas decisões. (`app/ponto/tests/test_contract_juiz_batida.py:132`, `app/ponto/tests/test_contract_juiz_batida.py:313`, `app/ponto/tests/test_contract_juiz_batida.py:330`)

**A justificativa do incremento é falsa em seu absoluto:** “não decide dia nenhum” só vale quando a chave existe. A expressão `.get(_e, _e.date())` deriva o dia civil quando falta a chave, enquanto a pertinência da batida já tem autoridade registrada em `turnos_do_colab`. O censo não distingue leitor de mapa de derivador paralelo; passa com ambos. (`app/ponto/tests/test_contract_juiz_batida.py:329`, `app/ponto/motor_calculo_v2.py:964`, `app/core/juizes.py:196`)

O laço mensal grava a chave antes da chamada normal, mas existe outra chamada produtiva no crédito de aberto sem gravação correspondente nesse ponto. O novo helper pode decidir por fallback nesse caminho; o período auxiliar depois empresta somente trabalhado/fora da janela, portanto isso isoladamente não prova dinheiro novo incorreto. (`app/ponto/motor_calculo_v2.py:2494`, `app/ponto/motor_calculo_v2.py:2590`, `app/ponto/motor_calculo_v2.py:2596`)

A base também conserva o segundo julgamento preexistente: recebe o dia do juiz e pode substituí-lo pela herança de intervalo até seis horas. O mapa novo passa adiante essa resposta composta, não necessariamente apenas o veredito do juiz registrado. (`app/ponto/motor_calculo_v2.py:2439`, `app/ponto/motor_calculo_v2.py:2443`, `app/ponto/motor_calculo_v2.py:2494`)

As adições do diff usam instantes recebidos e datas derivadas deles; não introduzem leitura de relógio nas funções alteradas. Isso não certifica concorrência nem ordem de travas da execução completa. (`app/ponto/motor_calculo_v2.py:452`, `app/ponto/motor_calculo_v2.py:963`, `app/ponto/services/fechamento.py:942`; execução concorrente: **[não verificado]**)

## 06/11 22:48 · sessao 7c8459a8b853 · FIO-E0-V4-E1 · censo · origem=Code

O `remover_motivo` citado é **`DisputaSupervisao.remover_motivo`, em `app/chamados/models.py:1900`**. Ele apaga perguntas em `app/chamados/models.py:1976`; a exclusão também alcança `RespostaAnterior` por `CASCADE`, definido em `app/chamados/models.py:3179`.

O relatório abaixo é um censo estático da cópia. Esquema instalado, execução das portas, existência das trilhas e passivo de produção: **[nao verificado]**.

**1. Decisões sobrescritas ou apagadas**

“Há trilha” abaixo significa que o código tenta produzi-la; não garante sua presença em produção. O escritor de eventos admite falha sem derrubar o ato, em `app/core/observ.py:94` e `app/core/observ.py:105`.

| Escritor | Campos gravados na mesma pergunta | Anterior preservado? |
|---|---|---|
| `responder_pergunta_disputa_core` | Substitui `resposta_colab` e `respondida_em`; limpa `validada_em/por`; pode retirar materialização sem lastro. `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1300` | **Não cria RA.** Guarda a validação desfeita em evento, mas não fotografa a resposta substituída. O log contém a resposta nova truncada; a nota contém a nova resposta/display e é best-effort. `app/chamados/services/disputa_emissao.py:1309`, `app/chamados/services/disputa_emissao.py:1318`, `app/chamados/services/disputa_emissao.py:1355` |
| Escape no mesmo core | Substitui `recusa_motivo` por “Colaborador respondeu…”. `app/chamados/services/disputa_emissao.py:1340` | Sem fotografia do motivo anterior e sem RA nesse bloco. `app/chamados/services/disputa_emissao.py:1331`, `app/chamados/services/disputa_emissao.py:1342` |
| `responder_automaticamente` | Escreve texto e instante se o texto estava vazio; pode substituir um instante existente nesse meio-estado. `app/chamados/services/disputa_emissao.py:1431` | Sem RA ou evento próprio na função. `app/chamados/services/disputa_emissao.py:1422` |
| `carimbar_resposta_pela_trilha` | Preenche `respondida_em` somente quando nulo, usando o instante fornecido. `app/chamados/services/disputa_emissao.py:1448` | Não perde carimbo preenchido; registra instante/motivo em evento. `app/chamados/services/disputa_emissao.py:1452` |
| `_carimbar_validacao` | Atribui `validada_em=now()` e `validada_por=autor`. `app/chamados/services/validacao.py:131` | O fluxo normal recusa pergunta já validada e registra log, mas não cria RA por aceite. `app/chamados/services/validacao.py:366`, `app/chamados/services/validacao.py:440` |
| `devolver_validacao_recusada` | Limpa `validada_em/por`, conservando a resposta. `app/chamados/services/validacao.py:180`, `app/chamados/services/validacao.py:188` | Evento guarda antes/depois da assinatura, motivo e fechamento da disputa; sem RA. `app/chamados/services/validacao.py:205` |
| `recusar_pergunta` | Substitui `recusa_motivo`; delega a limpeza da resposta/validação à reabertura. `app/chamados/services/validacao.py:541`, `app/chamados/services/validacao.py:547` | Tem fotografia da resposta, instante e recusa anteriores; nota e evento. A reabertura cria RA, mas a chamada **não passa autor/motivo** para ela. `app/chamados/services/validacao.py:551`, `app/chamados/services/validacao.py:555`, `app/chamados/services/validacao.py:547` |
| `reabrir_para_correcao` | Limpa texto, resposta e validação; com `desfazer_absorcao=True`, limpa também materialização, autor e via. `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1413` | **Cria RA antes do update, no mesmo atomic**, somente se havia texto ou carimbo de resposta. A fotografia não inclui materialização, via, pendência ou recusa. `app/chamados/services/disputa_emissao.py:1401`, `app/chamados/services/disputa_emissao.py:1406` |
| `_materializar_resposta_hora` | Substitui `pendencia_admin` e `recusa_motivo`. `app/chamados/services/materializacao.py:820`, `app/chamados/services/materializacao.py:825` | Sem RA ou fotografia anterior nesse bloco. `app/chamados/services/materializacao.py:822` |
| `_carimbar_recusa` | Troca `recusa_motivo` quando diferente; colisão também carimba `fato_ocupado`. `app/chamados/services/materializacao.py:1064`, `app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1078` | Nota contém a recusa atual traduzida, não a anterior; depende de haver chamado-destino. Sem RA. `app/chamados/services/materializacao.py:1079` |
| `carimbar` | Substitui `materializada_em/por/via_resolucao`; pode preencher resposta/validação nulas. `app/chamados/models.py:2583`, `app/chamados/models.py:2587` | A fotografia serve para rollback **em memória**, não para histórico persistente. Sem RA. `app/chamados/models.py:2595`, `app/chamados/models.py:2609`, `app/chamados/models.py:2616` |
| `carimbar_identidade` | Troca carimbo/via e, opcionalmente, recusa. `manter_carimbo=True` conserva instante/autor, mas troca a via. `app/chamados/models.py:2636`, `app/chamados/models.py:2641` | Sem histórico próprio; preservação depende do chamador. `app/chamados/models.py:2625`, `app/chamados/models.py:2644` |
| `descarimbar` / `descarimbar_em_lote` | Limpam materialização, autor e via; o lote aceita campos adicionais. `app/chamados/models.py:2646`, `app/chamados/models.py:2691` | Sem RA ou trilha própria nessas primitivas. `app/chamados/models.py:2652`, `app/chamados/models.py:2699` |
| `encerrar_por_fato_no_espelho` | Substitui apenas a via por `fato_materializado`. `app/chamados/models.py:2707` | Sem fotografia anterior ou RA na função. `app/chamados/models.py:2702` |
| `relavrar` | Substitui via derivada e cache de veredito/relógio. `app/chamados/services/relavra_via.py:121` | Evento conserva via/veredito anteriores e novos; não conserva todos os carimbos nem cria RA. `app/chamados/services/relavra_via.py:130` |
| `conferir_data_do_fato` | Preenche `data_conferida_em/por`; não altera `data_evento`. `app/chamados/services/validacao.py:615` | Guarda justificativa e os dias comparados no log; carimbo já preenchido é recusado. Sem RA. `app/chamados/services/validacao.py:620` |
| `lavrar` / `gravar_lavras_em_lote` | Substituem `veredito_lavrado`, `julgado_em`, `janela_expira_em`. `app/chamados/veredito_lavrado.py:207`, `app/chamados/veredito_lavrado.py:227` | São cache regravável, sem sequência anterior por pergunta nessas funções. `app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:211` |

Os atos abaixo passam pelas primitivas anteriores; continuam relevantes porque escolhem **qual decisão sobrescrever ou retirar**:

| Ato delegado | Efeito e preservação |
|---|---|
| `fechar_por_admin` | Troca outra via por `admin_decidiu`; a guarda protege somente repetição do próprio carimbo. Sem RA. `app/chamados/models.py:2513`, `app/chamados/models.py:2522` |
| `retratar_duplicata` | Troca via/recusa para apontar a canônica; pode conservar instante/autor antigos. Não fotografa a via/recusa substituídas. `app/chamados/services/fio.py:891` |
| `ressuscitar_perguntas` | Limpa materialização, validação e resposta temporal do universo selecionado sem texto. Log conserva IDs/rótulos, não fotografia integral das assinaturas apagadas. `app/chamados/models.py:1434`, `app/chamados/models.py:1504`, `app/chamados/models.py:1518` |
| `encerrar_fantasma`, `reabrir_fantasma`, `reabrir_por_resposta`, `reabrir_por_competencia` | Carimbam ou retiram carimbos; não criam RA por esses atos. `app/chamados/models.py:2721`, `app/chamados/models.py:2732`, `app/chamados/models.py:2749`, `app/chamados/models.py:2756` |
| `desfazer_carimbo_sem_lastro` | Relê sob lock e descarimba; evento guarda instante/via anteriores, mas não o autor anterior da materialização. `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:229` |

Na **disputa**, os escritores adicionais de decisão/remoção são:

| Escritor | Perda e trilha |
|---|---|
| `remover_motivo` | Apaga perguntas de todo aquele motivo, inclusive respondidas; apaga RA por cascata. `contexto.revalidacoes` guarda evento/instante/motivo removido, não as perguntas apagadas. `app/chamados/models.py:1955`, `app/chamados/models.py:1976`, `app/chamados/models.py:3179` |
| `aparar_motivo_respondido` | Retira motivo e sua chave do contexto, conservando perguntas; sem fotografia própria do contexto anterior. `app/chamados/models.py:1893` |
| `fechar` | Grava `fechada_em/por`, `resolucao`, `motivo_fechamento`. Em desligamento/admin_encerra também encerra perguntas vivas por `admin_decidiu`; eventos não fotografam integralmente seus carimbos anteriores. `app/chamados/models.py:1852`, `app/chamados/models.py:1856`, `app/chamados/models.py:1861` |
| `reabrir_admin` / `reabrir_sistema` | Limpam fechamento, autor, resolução e motivo; admin incrementa contador. Nota anterior de resolução depende do receiver do fechamento chegar a copiá-la. `app/chamados/models.py:2063`, `app/chamados/models.py:2087`, `app/chamados/signals.py:42` |
| `limpar_resolucao_residual` | Apaga resolução de disputa aberta; tenta copiar o texto anterior para nota quando há chamado. `app/chamados/models.py:2170` |

O rol `motivos/contexto` também é regravado na emissão/agregação, sem versionamento integral nesses blocos: helper comum, agrupador de órfãos e âncora. Isso é distinto de registrar uma decisão sobre a resposta. `app/chamados/services/disputa_emissao.py:477`, `app/chamados/services/disputa_emissao.py:492`, `app/chamados/services/disputa_emissao.py:922`, `app/chamados/services/disputa_emissao.py:970`, `app/chamados/services/disputa_emissao.py:998`, `app/chamados/services/disputa_emissao.py:1089`, `app/chamados/services/disputa_emissao.py:1190`.

**Silêncio:** existe encerramento humano com motivo; ele é tratado como decisão sobre perguntas mudas e passa por `fechar`. A antiga expiração automática por conversa parada foi removida, segundo a lápide do comando. `app/chamados/models.py:1872`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`. Expiração da janela do juiz continua existindo por leitura do relógio, sem nascer uma decisão imutável equivalente. `app/chamados/juizes.py:1197`, `app/chamados/veredito_lavrado.py:95`.

**2. RespostaAnterior**

O modelo guarda:

| Campo | Papel |
|---|---|
| `pergunta` | FK com `CASCADE`. `app/chamados/models.py:3179` |
| `resposta_anterior` | Texto retirado da pergunta. `app/chamados/models.py:3180` |
| `respondida_original_em` | Instante da resposta retirada. `app/chamados/models.py:3181` |
| `validada_original_em/por` | Assinatura copiada da pergunta. `app/chamados/models.py:3182` |
| `versao_reaberta_em/por` | Instante e autor da reabertura. `app/chamados/models.py:3185` |
| `motivo_reabertura` | Texto até 200 caracteres. `app/chamados/models.py:3188` |

O único escritor encontrado é `reabrir_para_correcao`: `bulk_create` antes da limpeza, no mesmo `atomic`; só cria versão quando havia texto ou carimbo de resposta. Ordenação é por reabertura e PK descendentes. `app/chamados/services/disputa_emissao.py:1401`, `app/chamados/services/disputa_emissao.py:1406`, `app/chamados/models.py:3190`.

Os seis chamadores encontrados **omitem `por` e `motivo`**, deixando os defaults vazios: colaborador, view de reabertura, ação do fio, recusa, devolução da absorção e command de limbo. `app/chamados/models.py:2146`, `app/chamados/views.py:1436`, `app/chamados/services/acoes_disputa.py:103`, `app/chamados/services/validacao.py:547`, `app/chamados/services/materializacao.py:658`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`, `app/chamados/services/disputa_emissao.py:1362`.

Leitores encontrados: helper de consulta, prefetch do fio e modal. O modal lê texto, instante original, autores, instante e motivo da reabertura; `validada_original_em` é armazenado, mas não aparece nessa apresentação. `app/chamados/services/disputa_emissao.py:1723`, `app/chamados/services/fio.py:227`, `app/templates/chamados/partials/modal_fio.html:208`.

Para virar a sequência única pedida, faltam, por comparação com os escritores:

- Registro em **cada** resposta, aceite, recusa, reabertura, retratação e decisão de silêncio; hoje RA nasce somente na reabertura. `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1406`, `app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:545`, `app/chamados/models.py:1852`.
- Tipo do fato, decisão/resposta à qual ele se aplica, identidade da instância do marco e relógio próprio de cada decisão; esses campos não constam do esquema atual de RA. `app/chamados/models.py:3179`.
- Preservação de recusa, via e absorção; a fotografia atual copia somente texto, resposta e validação. `app/chamados/services/disputa_emissao.py:1407`.
- Proteção contra exclusão em cascata e contra alteração posterior; a classe/migration não estabelecem proteção append-only. `app/chamados/models.py:3179`, `app/chamados/models.py:3190`, `app/chamados/migrations/0050_resposta_anterior.py:16`.

O histórico atual também não resolve essa lacuna: reconstrói eventos pelos **carimbos e texto atuais** da pergunta, sem consultar RA. `app/chamados/services/historico.py:96`.

**3. Identidade e acha-ou-cria**

Há **um nascimento central efetivo**, `cls.objects.create`, precedido de `.last()` por `(colaborador via disputa, motivo, data_evento)`. Célula e destino não integram essa chave; uma pergunta reaproveitada conserva sua disputa original. `app/chamados/models.py:2392`, `app/chamados/models.py:2431`, `app/chamados/models.py:2435`, `app/chamados/models.py:2450`, `app/chamados/models.py:2476`.

| Sítio de produção | Busca/chave local antes de `abrir` |
|---|---|
| `gerar_perguntas_disputa` | Sem data: disputa+motivo; datadas: disputa+motivo+dia. Depois aplica o dedup global do modelo. `app/chamados/services/disputa_emissao.py:299`, `app/chamados/services/disputa_emissao.py:370`, `app/chamados/services/disputa_emissao.py:386`, `app/chamados/services/disputa_emissao.py:389` |
| `_criar_pergunta_se_couber` | `_ja_perguntou` usa disputa+motivo+dia e vitalidade; depois `abrir`. `app/chamados/services/disputa_emissao.py:765`, `app/chamados/services/disputa_emissao.py:770`, `app/chamados/services/disputa_emissao.py:1237` |
| Emissão de âncora | Procura destino+`ancora_ausente` não materializada; chama `abrir` sem data. `app/chamados/services/disputa_emissao.py:1149`, `app/chamados/services/disputa_emissao.py:1195` |
| Reabertura administrativa | Escolhe última disputa fechada do chamado; cria confirmação sem data, sem dedup próprio de pergunta. `app/chamados/services/acoes_disputa.py:252`, `app/chamados/services/acoes_disputa.py:259` |
| Declaração do colaborador | Primeira reutilizável por disputa+motivo+dia; se nenhuma, `abrir`. `app/chamados/services/declaracao_texto.py:226`, `app/chamados/services/declaracao_texto.py:313`, `app/chamados/services/declaracao_texto.py:325` |
| Aplicação de declaração pelo admin | Mesma candidata reutilizável; fallback para `abrir`. `app/chamados/services/declaracao_texto.py:611`, `app/chamados/services/declaracao_texto.py:614` |

São **seis chamadores diretos do nascimento central**, nos endereços acima. Não foi encontrada chamada concreta de `get_or_create/update_or_create` para essa entidade; completude além da busca estática: **[nao verificado]**.

A conversa é uma exceção relevante: `data_evento=None` na entrada desliga o dedup e vira `timezone.localdate()` antes de gravar. Assim, consultas posteriores não distinguem por esse campo uma conversa de uma pergunta datada do mesmo dia. `app/chamados/models.py:2424`, `app/chamados/models.py:2476`.

**Unicidade:** não há constraint composta da pergunta declarada no `Meta`; a migration criadora também não a estabelece. Há PK/FKs e índices simples do cache, que não protegem a identidade solicitada. Índices/constraints realmente instalados em produção: **[nao verificado]**. `app/chamados/models.py:2857`, `app/chamados/migrations/0014_perguntadisputa.py:19`, `app/chamados/migrations/0014_perguntadisputa.py:30`, `app/chamados/models.py:2346`.

**Locks:** não existe trava da identidade dentro de `abrir`; seu `.last()→create` permanece separado. O `atomic` de `save` protege cada gravação, não torna a busca anterior exclusiva. `app/chamados/models.py:2431`, `app/chamados/models.py:2476`, `app/chamados/models.py:2853`.

| Caminho | Proteção encontrada |
|---|---|
| Helper de disputa | Trava disputa **existente** por colaborador+aberta; a geração normal acontece depois do retorno do helper. `app/chamados/services/disputa_emissao.py:466`, `app/chamados/services/disputa_emissao.py:598`, `app/chamados/services/disputa_emissao.py:605` |
| Declarações | Têm atomic externo e usam o helper de disputa; isso não substitui unicidade da pergunta nem cobre a ausência de uma disputa existente. `app/chamados/services/declaracao_texto.py:302`, `app/chamados/services/declaracao_texto.py:310`, `app/chamados/services/declaracao_texto.py:605`, `app/chamados/services/disputa_emissao.py:467` |
| Âncora | Atomic e lock de disputa existente; teste de pergunta ocorre antes desse lock. `app/chamados/services/disputa_emissao.py:1148`, `app/chamados/services/disputa_emissao.py:1149`, `app/chamados/services/disputa_emissao.py:1156` |
| Agrupador de órfãos | Atomic, mas seleção da disputa sem `select_for_update`. `app/chamados/services/disputa_emissao.py:898`, `app/chamados/services/disputa_emissao.py:935` |
| Reabertura admin | Sem lock próprio nessa sequência de seleção/reabertura/nascimento. `app/chamados/services/acoes_disputa.py:252`, `app/chamados/services/acoes_disputa.py:259` |

A janela estática da corrida está comprovada; sua reprodução concorrente em banco: **[nao verificado]**. `app/chamados/models.py:2431`, `app/chamados/models.py:2476`.

**Instância de marco candidata:** o dado já oferece as chaves `hi/hii/hfi/hii2/hfi2/hf`, qualificadas pela célula, com ordem canônica e exposição `(tipo, chave, hora)`. A pergunta ainda não persiste essa chave. Usar célula+chave é uma inferência de encaixe; estabilidade através de regeneração do DNA: **[nao verificado]**. `app/escala/models.py:1652`, `app/escala/models.py:1659`, `app/escala/models.py:1672`, `app/chamados/models.py:2209`.

Hoje há perda concreta dessa distinção: declaração reduz marcos internos a ida/volta pelo tipo, e o juiz admite ambiguidade com duas pausas. Cartório e pauta já consomem o vocabulário de marcos da célula. `app/chamados/services/declaracao_texto.py:118`, `app/chamados/services/declaracao_texto.py:300`, `app/chamados/juizes.py:1106`, `app/ponto/services/cartorio.py:586`, `app/pautas/services.py:417`.

**4. Absorção**

A ligação atual é **reconstruída**, não uma FK/fato explícito do ato: o materializador obtém a ausência, captura um `agora` e tenta carimbar as perguntas-hora da mesma disputa/dia com esse instante. `app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:529`, `app/chamados/services/materializacao.py:531`, `app/chamados/services/materializacao.py:549`.

O leitor:

1. Restringe colaborador, dia, tipo de pergunta e vias de absorção. `app/chamados/services/materializacao.py:605`.
2. Procura uma resposta de escape compatível com o tipo da ausência e extrai `(disputa_id, materializada_em)`. `app/chamados/services/materializacao.py:602`, `app/chamados/services/materializacao.py:609`.
3. Recupera as irmãs daquele grupo e exclui perguntas com batida-origem viva. `app/chamados/services/materializacao.py:614`, `app/chamados/services/materializacao.py:616`.

| Campo | Outros escritores |
|---|---|
| `disputa` | Nascimento em `abrir`; não foi encontrada transferência posterior. Reapontamento muda destino, não disputa. `app/chamados/models.py:2476`, `app/chamados/services/fio.py:845` |
| `data_evento` | Nascimento; reapontamento preenche somente quando estava vazia. `app/chamados/models.py:2476`, `app/chamados/services/fio.py:832`, `app/chamados/services/fio.py:847` |
| `materializada_em` | Atribuído pelo helper de carimbo; persistido por `carimbar`, `carimbar_ide

[truncado em 20000 de 36686 caracteres]

## 06/11 22:46 · sessao ee0df63a3e82 · FIO-E0-V4-E2 · censo · origem=Code

**O censo mostra cinco respostas de escopos diferentes:** existência de chamado, vitalidade de pergunta, necessidade de emitir pergunta, alcance pela fila e atribuição fora do fio. Nenhuma delas, isoladamente, responde por todas as ações exigíveis de cada instância de marco. (`app/chamados/juizes.py:1886`, `app/chamados/juizes.py:1135`, `app/chamados/services/fabrica_da_celula.py:74`, `app/chamados/juizes.py:1341`, `app/chamados/services/vigia_fio.py:274`)

**A bateria C-E não fecha nesta cópia.** As referências do aval a `BACKLOG:1797/1813/1821` apontam aqui para outras obras; localização e descrição individual dos dez casos FIO-E0: **[nao verificado]**. (`app/docs/BACKLOG.md:1797`, `app/docs/BACKLOG.md:1813`, `app/docs/BACKLOG.md:1821`)

**1. Os cinco respondedores**

**`cobranca_viva_do_dia(colaborador, modulo_origem, dia, *, excluir=None)`**

- **Responde:** `ChamadoColaborador | None`; escolhe o chamado mais novo dos módulos pedidos, em `VIVOS`, cujo dia forte coincide com `dia`. Não confere marco faltante, efeitos, ausência ou silêncio. (`app/chamados/juizes.py:1886`, `app/chamados/juizes.py:1894`)
- **Lê:** chamado `colaborador`, `modulo_origem`, `status_local`, `contexto_json.data_turno`, `celula.data`, `criado_em`, `pk`; o desempate é `-criado_em,-pk`. O dia forte prioriza a célula e depois as informações declaradas do chamado. (`app/chamados/juizes.py:1897`, `app/chamados/juizes.py:80`)
- **Relógio:** recebe o dia; esse percurso não consulta o instante atual. `criado_em` ordena candidatos. (`app/chamados/juizes.py:1886`, `app/chamados/juizes.py:1899`)
- **Todos os chamadores diretos de produção encontrados**, incluindo alias:

| Chamador | Local da chamada |
|---|---|
| `ChamadoColaborador._plano_pergunta_viva` | `app/chamados/models.py:373` |
| `ChamadoColaborador._gate_premissa_do_fechamento` | `app/chamados/models.py:711` |
| `_julgar_colab_corpo`, via `_cobr_c8` | `app/ponto/services/cartorio.py:1604` |
| `resolver_cobranca_da_entrada` | `app/ponto/services/entrada_sincronizada.py:28` |
| `_autoresolve_f2_f4` | `app/ponto/services/triagem_batida.py:589` |

**`pergunta_viva(p)`**

- **Responde:** booleano, delegando a `_pergunta_viva_calc`, com memo `_pv_memo` escrito na instância. Portanto, não é pura pelos argumentos explícitos: consulta outras fontes e relógios internos. (`app/chamados/juizes.py:822`, `app/chamados/juizes.py:835`, `app/chamados/juizes.py:1197`)
- **Ordem decisiva:** validação mata; tranca aplicável mata; resposta com texto permanece viva salvo decisão administrativa/concordância com a luz; identidade materializada encerra; materialização com marco aceso encerra; célula acusadora mantém viva; depois entram teto, classificação da falta e conferência de materialização de fato. (`app/chamados/juizes.py:1135`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1170`, `app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1194`)
- **Lê diretamente:** pergunta `validada_em`, `respondida_em`, `resposta_colab`, `via_resolucao`, `materializada_em`, `data_evento`, `tipo_resposta`, `motivo`, `celula`, `disputa.colaborador`, `chamado_destino`; célula `veredito`, `veredito_via`, `dna.marcos`, `ata.lampadas` — `hora`, `tipo`, `acesa`, `luz`, pendência temporal. (`app/chamados/juizes.py:1135`, `app/chamados/juizes.py:886`, `app/chamados/juizes.py:1099`)
- **Lê transitivamente:** empresa/corte da competência, tranca e toque humano posterior; batidas ligadas à pergunta, retratação, slot esperado, batida no slot, turnos, ausências, vínculo do dia e estado do destino em um ramo legado. (`app/chamados/juizes.py:935`, `app/chamados/juizes.py:1065`, `app/chamados/juizes.py:718`, `app/chamados/juizes.py:743`, `app/escala/servico_jornada.py:340`)
- **Relógios ocultos:** `_dentro_da_janela_viva` usa `date.today()`; `classificar_falta` usa `_tz.now()` contra o fim do turno. A assinatura não recebe `agora`. (`app/chamados/juizes.py:556`, `app/chamados/juizes.py:562`, `app/escala/servico_jornada.py:375`)

**Todos os chamadores diretos de produção encontrados**, com aliases resolvidos:

| Arquivo | Chamadores e linhas das chamadas |
|---|---|
| `app/chamados/juizes.py` | `pergunta_sem_lastro_ou_viva:1225`; `perguntas_vivas_de:1230`; `fio_vivo:1242`; `alimentar_portas:1302`; `perguntas_respondidas_a_validar:1403`; `_dias_das_perguntas_vivas:2031` |
| `app/chamados/models.py` | `_vivas_entre:351`; `_universo_da_ressuscitacao:1458`; `pendentes_do_colaborador:1739`; `DisputaSupervisao.fechar:1837`; `PerguntaDisputa.estado:2502`; `carimbar:2596`; `viva:2809` |
| `app/chamados/views.py` | `responder_pergunta_disputa:1242,1257`; `reabrir_disputa_supervisao:1417` |
| `app/api/views.py` | `api_disputa_responder:2631` |
| `app/chamados/relatorios_disputa.py` | `classificar:18` |
| `app/chamados/reconciliador.py` | `lavrar_sem_turno_aberto:398`; `perguntas_absolvidas_pela_celula:651`; `_viva_em_competencia_aberta:748`; `veredito_orfa:1075`; `reatar_pergunta_orfa:1579` |
| `app/colaboradores/services/calendario.py` | `contexto_calendario:476` |
| `app/ponto/services/dia_do_colab.py` | `dia_do_colab:71` |
| `app/chamados/services/fio.py` | `montar_chamados_fio:234`; `processar_post_fio:602`; `_costura_nao_mata_o_fio:775`; `migrar_resolvedoras_vivas:932,935` |
| `app/chamados/services/pergunta_no_app.py` | `perguntas_do_app:53`; `_marco_ja_perguntado:111` |
| `app/chamados/services/acoes_chamado.py` | `montar_contexto_detalhe:348` |
| `app/chamados/services/disputa_emissao.py` | `resolver_perguntas_por_chamado_resolvido:1597,1679` |
| `app/chamados/services/competencia_trancada.py` | `medir:122,132,142` |
| `app/chamados/services/relavra_via.py` | `desfazer_carimbo_sem_lastro:227` |
| `app/chamados/services/aviso_pendencia.py` | `perguntas_pendentes_do_colab:13` |
| `app/chamados/services/vigia_fio.py` | `viva_em_disputa_fechada:755`; `carimbos_sem_lastro_por_dono:813` |
| `app/chamados/management/commands/disparar_perguntas_competencia.py` | `handle:148` |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py` | `handle:83` |
| `app/chamados/management/commands/auditar_invariantes_chamados.py` | `_contraprova_f1t:202`; `_i6_pares:540`; `handle:858,894,1650,1707,1766,1779` |

A tabela identifica chamadas da função. Há também entrada indireta por `.viva`, que delega integralmente ao juiz, e por `.estado`; eliminar apenas os imports explícitos deixaria essas entradas ativas. (`app/chamados/models.py:2492`, `app/chamados/models.py:2798`)

**`motivos_a_perguntar(cel, hoje, agora=None, *, carimbadas=None, julgada_antes=_JULGADA_DA_CELULA)`**

- **Responde:** lista de motivos que ainda precisam ser emitidos; opcionalmente acrescenta IDs à lista `carimbadas`. Não devolve todas as ações pendentes de perguntas existentes. (`app/chamados/services/fabrica_da_celula.py:74`, `app/chamados/services/fabrica_da_celula.py:123`)
- **Lê:** célula/colaborador — `data`, `trabalha`, `ata`, `veredito`, `julgada_em`, `situacao`; dia sem trabalho; marcos vencidos; parcial destinada à lista administrativa; perguntas anteriores do colab-dia. (`app/chamados/services/fabrica_da_celula.py:88`, `app/chamados/services/fabrica_da_celula.py:98`, `app/chamados/services/fabrica_da_celula.py:118`)
- **Deduplicação atual:** qualquer pergunta anterior do mesmo motivo bloqueia emissão, exceto `VIA_DUPLICATA`. Não consulta vitalidade nem discrimina a instância do marco. Duas pausas podem ser comprimidas no mesmo motivo. (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:59`)
- **Entradas transitivas:** `_ata_do_dia`/`_marcos_faltantes`; vínculo e marcos encadeados; `parcial_so_lista`; julgamento anterior e fatos novos de batida, ausência, justificativa, edição/regeneração da célula ou vencimento de marco. (`app/chamados/services/disputa_emissao.py:818`, `app/chamados/services/declaracao_texto.py:88`, `app/ponto/services/cartorio.py:329`, `app/ponto/services/cartorio.py:370`)
- **Relógio:** `hoje` e `agora` por parâmetro; sem `agora`, usa `timezone.localtime()`. O ramo sem ata/marcos pode chamar `classificar_falta`, que ainda lê seu próprio `now()`. (`app/chamados/services/fabrica_da_celula.py:93`, `app/chamados/services/fabrica_da_celula.py:55`, `app/escala/servico_jornada.py:377`)
- **Únicos chamadores diretos encontrados:** `perguntar_pela_celula` e `motivos_que_o_evento_deixou`. (`app/chamados/services/fabrica_da_celula.py:164`, `app/chamados/services/fabrica_da_celula.py:199`)

**`porta_alcancavel_do_dia(celula_id, *, alimentacao=None)`**

- **Responde:** `None` ou `{rota, fila, verbos, fio}`. Soma chamado da célula na fila com pergunta viva no app, sob âncora na fila e sem resposta carimbada vazia. O fio prioriza `cobrar→colab`, depois `validar→admin`, senão `chamado`. (`app/chamados/juizes.py:1327`, `app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1375`)
- **Lê:** partição global dos chamados vivos, pertença à fila global, perguntas da célula julgadas pelo C6, célula→colaborador e perguntas do app por colaborador. Chamados fornecem `id`, `modulo_origem`, `silenciado_ate`, `criado_em`, `urgencia`; perguntas fornecem resposta, destino e chamado da disputa, além das entradas transitivas do C6. (`app/chamados/juizes.py:1282`, `app/chamados/juizes.py:1309`, `app/chamados/services/verbos.py:44`, `app/chamados/services/fio.py:116`)
- **Relógio:** não recebe instante; `particionar_verbos` e `qs_na_fila` fazem leituras distintas de `timezone.now()`. (`app/chamados/services/verbos.py:35`, `app/chamados/services/painel.py:188`)
- **Todos os chamadores diretos encontrados:** `censo_alcance`, `cadastro_realidade.beco_em_lote` e `fora_da_folha._porta`. Esta última atende `_fio_do_dia` e a lista de chamados de `o_que_segura`. (`app/chamados/services/vigia_fio.py:635`, `app/escala/services/cadastro_realidade.py:460`, `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:77`, `app/folha/fora_da_folha.py:225`)

**`dono_fora_do_fio(colab, data, perguntas, lista, com_papel=False, *, oferecido)`**

- **Responde, nesta ordem:** `decisao_do_dia_pendente`, `aguarda_decisao_de_ausencia`, `escala_nao_descreve_o_dia`, `fio_do_admin` ou `None`; com `com_papel=True`, devolve também o papel. (`app/chamados/services/vigia_fio.py:274`, `app/chamados/services/vigia_fio.py:304`)
- **Lê:** `oferecido`; ausência cobrindo a data para cobrança e status em `ABERTAS`; perguntas com `estado==VALIDADA` e resposta `FOLGA_CONTESTA`; presença e dono do dia na lista administrativa. (`app/chamados/services/vigia_fio.py:306`, `app/chamados/services/validacao.py:216`)
- **Relógio:** não lê diretamente. O calendário que produz `oferecido` lê seu instante; a lista recebe `hoje/agora` e os repassa ao carregamento. (`app/colaboradores/services/calendario.py:159`, `app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:216`)
- **Todos os chamadores diretos encontrados:** `censo` e `censo_alcance`. (`app/chamados/services/vigia_fio.py:431`, `app/chamados/services/vigia_fio.py:654`)

**2. União das entradas**

| Família | Fatos e insumos necessários evidenciados pelo código |
|---|---|
| **Pessoa e contexto** | Identidade, situação, empresa e corte da competência; vínculo do dia, inclusive geradora da célula e tipo da escala. (`app/chamados/services/fabrica_da_celula.py:88`, `app/chamados/juizes.py:931`, `app/ponto/services/cartorio.py:840`) |
| **Célula e instâncias de marco** | Data do turno, `trabalha`, DNA e geradora, sequência dos marcos com tipo/hora e encadeamento noturno, edição/regeneração. Hoje a pergunta mapeia motivo→chave e, nas pausas múltiplas, agrupa várias luzes; isso não fornece identidade individual suficiente. (`app/chamados/juizes.py:867`, `app/chamados/juizes.py:893`, `app/ponto/services/cartorio.py:365`, `app/ponto/services/cartorio.py:845`) |
| **Batidas e efeitos** | Universo apurável, timestamp, tipo, associação ao turno/marco, pergunta de origem, criação e retratação; conferência do slot e do efeito real. A porta apurável exclui `retratada_em` preenchida. (`app/ponto/turnos.py:2844`, `app/chamados/juizes.py:718`, `app/ponto/services/cartorio.py:352`) |
| **Ausências e justificativas** | Tipo, status, faixa de datas, período/minutos da parcial e decisões/carimbos; distinguir cobertura do dia inteiro e do resto exigível. (`app/ponto/turnos.py:2536`, `app/ponto/services/cartorio.py:355`, `app/ponto/services/cartorio.py:370`) |
| **Decisões da pergunta/dia** | Resposta, validação, recusa/reabertura/retração, via, instante, alvo e decisão administrativa; conferir os efeitos ligados à decisão. Hoje são lidos campos correntes e `RespostaAnterior`, não está demonstrada uma sequência única imutável de decisões E1. (`app/chamados/juizes.py:1135`, `app/chamados/services/fio.py:216`, `app/chamados/models.py:2513`; sequência E1 completa: **[nao verificado]**) |
| **Chamados e portas** | Módulo, célula/dia, estado, silêncio, âncora/destino e chamado da disputa; universo global relevante ao crédito; verbos, presença no app, “Resolver dia” e lista administrativa por dia/dono. (`app/chamados/juizes.py:1282`, `app/chamados/juizes.py:1378`, `app/chamados/services/vigia_fio.py:258`, `app/chamados/services/vigia_fio.py:313`) |
| **Competência e atos aplicáveis** | Tranca, instante da tranca e toque humano posterior sobre o colab-dia. Hoje esse toque depende de `LogAuditoria`; não foi consultado dado de log nesta análise. (`app/chamados/juizes.py:935`, `app/chamados/juizes.py:954`, `app/chamados/juizes.py:1065`) |
| **Tempo** | Um `agora` para vencimento dos marcos, silêncio e janela; `dia_do_turno` deve conservar o eixo da jornada, inclusive meia-noite. (`app/ponto/services/cartorio.py:831`, `app/chamados/services/verbos.py:48`, `app/ponto/turnos.py:3013`) |

**Dependências atuais de estado/projeção, distintas dos fatos de origem:**

- **`veredito_lavrado`: dependência direta da porta, pelo crédito dos verbos.** O crédito exclui pergunta materializada cujo veredito gravado é `VL_SIM`; o código admite leitura desse campo desatualizado até o cron. (`app/chamados/services/raiox.py:110`, `app/chamados/services/raiox.py:133`)
- **`CelulaDia.veredito`, `veredito_via`, ata e julgamento anterior:** influenciam vitalidade, emissão e encaminhamento da parcial. São resultados/carimbos já gravados usados na decisão atual. (`app/chamados/juizes.py:1147`, `app/chamados/juizes.py:1181`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:102`)
- **`materializada_em` e `via_resolucao`:** têm função de encerramento, inclusive por identidade; validação encerra o C6 antes da conferência do efeito. Não se pode tratar todo `materializada_em` como prova de batida. (`app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1170`, `app/chamados/juizes.py:1207`)
- **`.estado` não é coluna persistida:** é propriedade derivada de validação, materialização, resposta e C6. Mesmo assim, `dono_fora_do_fio` depende do rótulo exato `VALIDADA`. (`app/chamados/models.py:2492`, `app/chamados/services/vigia_fio.py:311`)
- **Estado dos chamados:** o C8 usa `status_local`; a porta usa esse estado mais silêncio e crédito. Um ramo da prova de materialização ainda consulta destino encerrado. (`app/chamados/juizes.py:1897`, `app/chamados/services/painel.py:186`, `app/chamados/juizes.py:743`)

**3. `CargaDaJanela` e carregamento por janela**

**Existe:** `CargaDaJanela(colaboradores, ini, fim)`, em `app/ponto/turnos.py:2922`. Carrega células com pad, geradoras, candidatos/vínculos, tipos e atas; carrega batidas apuráveis e última batida sob demanda; memoriza turnos por colaborador. Oferece `turnos`, `dias_encerrados` e `dia_das_batidas`. (`app/ponto/turnos.py:2936`, `app/ponto/turnos.py:2959`, `app/ponto/turnos.py:2974`, `app/ponto/turnos.py:3001`)

**Os nomes literais `precedencia_cobertura_teto` e `portas_com_universo_global` não foram localizados como funções nesta cópia: [nao verificado].** As composições existentes são:

| Composição atual | Granularidade e consultas |
|---|---|
| Precedência | `fatos_do_dia` delega ao período `[dia,dia]`; `fatos_do_periodo` já atende uma janela por colaborador, mas consulta ausências e folgas desse colaborador. (`app/ponto/precedencia.py:242`, `app/ponto/precedencia.py:259`, `app/ponto/precedencia.py:273`) |
| Cobertura | `ausencia_cobre` faz consulta pontual por colab-dia; `cobertura_ausencia_periodo` já carrega a janela por colaborador com o mesmo desempate. (`app/ponto/turnos.py:2536`, `app/ponto/turnos.py:2733`) |
| Teto | `vencidos_no_instante` chama `marcos_vencidos` por célula/dia. Há tratamento próprio para passado sem marcos; não equivale a simplesmente testar “dia encerrado”. (`app/chamados/services/fabrica_da_celula.py:15`, `app/ponto/services/cartorio.py:831`) |
| Portas globais | `alimentar_portas` particiona `qs_vivos()` global e lê `qs_na_fila()` global, depois agrupa perguntas/células pedidas. Sem alimentação, a porta abre esse carregamento para uma célula. (`app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1290`, `app/chamados/juizes.py:1375`) |

**Hoje os leitores principais das portas já reutilizam lote:** vigia, Cadastro×Realidade e folha passam alimentação; não há justificativa estática para chamar novamente a partição global a cada colab-dia. Permanecem consultas internas do C6 e carregamento do app por colaborador. (`app/chamados/services/vigia_fio.py:604`, `app/chamados/services/vigia_fio.py:635`, `app/escala/services/cadastro_realidade.py:456`, `app/folha/fora_da_folha.py:218`, `app/chamados/juizes.py:1260`, `app/chamados/juizes.py:1309`)

**Para carregar por janela — inferência de implementação:** ampliar a alimentação com precedência/cobertura/teto, perguntas e decisões por instância, efeitos e portas; realizar uma partição global compartilhada; fornecer o mesmo `agora`; impedir que o corpo puro volte ao ORM, ao relógio ou ao memo de derivação antigo. Os pontos atuais a remover do corpo estão evidenciados nas consultas da precedência, cobertura, C6 e portas. (`app/ponto/precedencia.py:273`, `app/ponto/turnos.py:2552`, `app/chamados/juizes.py:1197`, `app/chamados/juizes.py:1282`)

**Custo medido nesta cópia, 2,95 queries/colab-dia, meta ≤1 e complexidade efetiva da frota: [nao verificado].** O anexo os fornece como referência/meta; o censo estático não prova esses resultados.

**4. Divergências entre os cinco**

São contraexemplos permitidos pelos predicados; ocorrência e quantidade em produção: **[nao verificado]**.

| Mesmo cenário de colab-dia | Respostas diferentes pelo código |
|---|---|
| Único chamado `registrado` ou com silêncio vigente; nenhuma outra porta | C8 pode devolver o chamado porque usa `VIVOS`; porta pode devolver `None`, pois a fila exige `COBRANCA` e exclui silêncio futuro. (`app/chamados/juizes.py:1897`, `app/chamados/services/painel.py:186`, `app/chamados/juizes.py:1382`) |
| Pergunta respondida com texto, sem validação/tranca/decisão que a encerre, sob âncora terminal | C6 pode dizer viva; porta rejeita a âncora fora da fila. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1378`) |
| Pergunta com `respondida_em`, texto vazio e célula acusadora, sem outra porta | C6 pode dizer viva; porta rejeita `respondida_sem_texto`. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1183`, `app/chamados/juizes.py:1254`, `app/chamados/juizes.py:1380`) |
| Marco continua faltante, pergunta já validada sem plantio | C6 diz morta imediatamente; fábrica não emite porque o motivo já existe; a falta pode permanecer na ata. (`app/chamados/juizes.py:1136`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/declaracao_texto.py:109`) |
| Próprio marco aceso, outro marco do dia faltante, pergunta sem materialização | C6 pode continuar viva pelo veredito acusador do dia: nesse ramo, a luz do próprio marco só encerra com `materializada_em`. A fábrica não reemite o motivo existente. (`app/chamados/juizes.py:1177`, `app/chamados/juizes.py:1181`, `app/chamados/services/fabrica_da_celula.py:123`) |
| Competência trancada, sem

[truncado em 20000 de 27062 caracteres]

## 06/11 22:44 · sessao 7735e49f02c8 · - · - · origem=Ronald (tmux)

Censo de portas fechado por regra explícita: contar **funções que executam a persistência da pergunta** (`save`, `update`, `create`, `delete` ou `bulk_update`), sem contar novamente o helper que só atribui em memória, o chamador que apenas delega, nem ramos/arestas como portas diferentes. O código separa atribuição em memória e persistência em `app/chamados/models.py:2556-2560`, `app/chamados/models.py:2585-2609`, `app/chamados/veredito_lavrado.py:197-207`.

**Resultado: 15 funções para ciclo/estado derivado; 17 incluindo cache; 21 incluindo estado operacional de recusa, pendência e conferência.** São os sítios enumerados abaixo; **“16 portas” do aval não é o total desse recorte vivo**. A lista P13 atual enumera campos/arquivos, não 16 funções, e seu próprio comentário histórico distingue contagem por arquivo de contagem por mão. `app/core/tests/test_contract_escritores_estado.py:61-82`, `app/core/tests/test_contract_escritores_estado.py:83-103`, `app/core/tests/test_contract_escritores_estado.py:133-173`, `app/chamados/services/validacao.py:127-133`.

Não existe `status` persistido em `PerguntaDisputa`: `estado` deriva quatro rótulos de `validada_em`, `materializada_em`, `respondida_em` e `pergunta_viva`. Adotei P=pendente, A=respondida, V=validada, R=resolvida. A fórmula exata é: com validação, V/R conforme exista materialização; sem validação, se viva, P/A conforme exista resposta; se morta e materializada, R; caso contrário P/A. `app/chamados/models.py:2377-2378`, `app/chamados/models.py:2491-2506`.

| # | Persistência de ciclo/estado | Origem → destino permitido pelo código |
|---|---|---|
| 1 | `PerguntaDisputa.abrir` | inexistente → normalmente P; reaproveitada → estado recalculado pela nova célula/destino; `**campos` pode alterar os valores iniciais. `app/chamados/models.py:2398-2405`, `app/chamados/models.py:2430-2477` |
| 2 | `PerguntaDisputa.carimbar` | carimba materialização/via, podendo preencher resposta e validação; só persiste se o C6 disser morta depois da aplicação, portanto destino R. Recusa desfaz em memória e não persiste. `app/chamados/models.py:2562-2614` |
| 3 | `PerguntaDisputa.carimbar_identidade` | estado anterior → estado resultante da via de identidade; não garante R: resposta não validada ainda pode manter A. Mantém instante/autor antigos quando `manter_carimbo=True`. `app/chamados/models.py:2625-2644` |
| 4 | `PerguntaDisputa.descarimbar` | remove materialização/autor/via → V se validação permanecer; caso contrário P/A segundo `respondida_em`. `app/chamados/models.py:2646-2653`, `app/chamados/models.py:2499-2506` |
| 5 | `PerguntaDisputa.descarimbar_em_lote` | mesma retirada dos três campos, mais `outros` definidos pelo chamador; destino depende desses campos adicionais. `app/chamados/models.py:2691-2700` |
| 6 | `PerguntaDisputa.encerrar_por_fato_no_espelho` | troca apenas a via por `fato_materializado`; normalmente pergunta já materializada, podendo mudar vitalidade/rotulagem sem mudar instante. `app/chamados/models.py:2702-2711` |
| 7 | `_carimbar_validacao` | estado sem validação → V se sem materialização, R se materializada. A guardas de resposta/validação anterior estão em `validar_pergunta`, não nesse helper. `app/chamados/services/validacao.py:127-133`, `app/chamados/services/validacao.py:363-368`, `app/chamados/models.py:2499-2501` |
| 8 | `devolver_validacao_recusada` | V → normalmente A; só desfaz validação de resposta de hora não materializada com motivo de recusa. Também pode reabrir a disputa. `app/chamados/services/validacao.py:155-190`, `app/chamados/services/validacao.py:195-212` |
| 9 | `responder_pergunta_disputa_core` | P/A/V/R → resposta nova, instante novo, validação retirada; remove materialização somente das vias sem lastro. Destino normalmente A, podendo permanecer R se materialização sobreviver e C6 disser morta. `app/chamados/services/disputa_emissao.py:1282-1301`, `app/chamados/models.py:2735-2749`, `app/chamados/models.py:2499-2506` |
| 10 | `reabrir_para_correcao` | limpa texto/resposta/validação; com `desfazer_absorcao=True` limpa também materialização/via/autor. Nesse ramo → P. No padrão, materialização pode sobreviver: o destino P prometido pelo comentário não é incondicional pela fórmula de `estado`. `app/chamados/services/disputa_emissao.py:1386-1418`, `app/chamados/models.py:2499-2506` |
| 11 | `responder_automaticamente` | pergunta sem texto → texto+instante; normalmente P→A. Não limpa validação/materialização anteriores; não atua quando já há texto. `app/chamados/services/disputa_emissao.py:1422-1436` |
| 12 | `carimbar_resposta_pela_trilha` | texto existente sem instante → instante da nota; normalmente P→A. Já carimbada ou sem texto é no-op. `app/chamados/services/disputa_emissao.py:1439-1455` |
| 13 | `reapontar_resolvedora` | destino antigo→novo; data nula→data fornecida; relavra cache no mesmo `save`. Não altera resposta/validação/materialização, mas muda insumos da vitalidade. `app/chamados/services/fio.py:829-849` |
| 14 | `relavrar` | via derivada antiga→via atual; relavra cache junto. Com materialização e sem validação, pode alterar R↔P/A pela vitalidade; carimbos de resposta/validação/materialização permanecem. `app/chamados/services/relavra_via.py:104-126`, `app/chamados/models.py:2499-2506` |
| 15 | `DisputaSupervisao.remover_motivo` | pergunta existente→apagada; inclui suas `RespostaAnterior` por cascade. `app/chamados/models.py:1976`, `app/chamados/models.py:3179` |

As duas portas adicionais de **cache** são `lavrar`, que grava `veredito_lavrado`, `julgado_em`, `janela_expira_em`, e `gravar_lavras_em_lote`, que executa o `bulk_update`; o command `backfill_veredito_lavrado` apenas prepara os valores e delega a gravação. Elas não mudam diretamente a property `estado`, mas mudam o estado materializado usado pelos leitores SQL. `app/chamados/veredito_lavrado.py:194-231`, `app/chamados/management/commands/backfill_veredito_lavrado.py:134-140`.

As quatro portas adicionais de **estado operacional** são:

| # | Porta | Origem → destino |
|---|---|---|
| 18 | `_materializar_resposta_hora` | pendência/motivo anteriores → pendência `tipo_nao_conferido` e motivo produzidos pelo cálculo. A mesma função também usa a porta de identidade para paridade. `app/chamados/services/materializacao.py:820-826`, `app/chamados/services/materializacao.py:899` |
| 19 | `_carimbar_recusa` | motivo anterior → motivo atual; na colisão usa também `carimbar_identidade(VIA_FATO_OCUPADO)`. `app/chamados/services/materializacao.py:1058-1077` |
| 20 | `recusar_pergunta` | motivo anterior→recusa humana, depois delega a retirada da resposta/validação para `reabrir_para_correcao`. `app/chamados/services/validacao.py:537-558` |
| 21 | `conferir_data_do_fato` | divergente não conferida→conferida; grava instante/autor, liberando a materialização. Não muda `data_evento`. `app/chamados/services/validacao.py:611-628` |

**Wrappers semânticos separados da contagem acima:** há 16 funções que chamam diretamente as primitivas `carimbar`, `carimbar_identidade`, `descarimbar` ou `descarimbar_em_lote`, excluídas as próprias primitivas e `reabrir_para_correcao`, que já persiste diretamente no ramo padrão. Esse 16 é uma contagem de **chamadores imediatos**, não 16 escritores independentes. Dois deles, `_materializar_resposta_hora` e `_carimbar_recusa`, já aparecem no recorte operacional por outras escritas próprias. `app/chamados/models.py:1504`, `app/chamados/models.py:2522`, `app/chamados/models.py:2721-2756`, `app/chamados/services/materializacao.py:825-826`, `app/chamados/services/materializacao.py:1074-1078`.

| Wrapper | Ato escolhido e referência |
|---|---|
| `ChamadoColaborador.ressuscitar_perguntas` | Limpa carimbo, validação e resposta temporal das perguntas sem texto selecionadas pelo C6; →P. `app/chamados/models.py:1434-1458`, `app/chamados/models.py:1492-1504` |
| `PerguntaDisputa.fechar_por_admin` | Via `admin_decidiu`; idem mesma via+carimbo é no-op. `app/chamados/models.py:2508-2523` |
| `PerguntaDisputa.encerrar_fantasma` | Via `fantasma`, sem tocar texto; C6 pode recusar. `app/chamados/models.py:2713-2721` |
| `PerguntaDisputa.reabrir_fantasma` | Retira exclusivamente carimbo de fantasma. `app/chamados/models.py:2723-2733` |
| `PerguntaDisputa.reabrir_por_resposta` | Retira somente carimbo das vias sem lastro, sem salvar; save pertence ao escritor da resposta. `app/chamados/models.py:2735-2749`, `app/chamados/services/disputa_emissao.py:1300-1301` |
| `PerguntaDisputa.reabrir_por_competencia` | Retira exclusivamente via histórica `competencia_trancada`. `app/chamados/models.py:2751-2757` |
| `retratar_duplicata` | Via `duplicata`, conserva instante/autor se já carimbada e escreve a canônica no motivo. `app/chamados/services/fio.py:881-895` |
| `resolver_perguntas_por_chamado_resolvido` | Dois sítios na mesma função: carimbo pela via do fato com auto validação; ramo sem credor carimba com resposta apenas quando há texto. `app/chamados/services/disputa_emissao.py:1617-1618`, `app/chamados/services/disputa_emissao.py:1684-1692` |
| `_materializar_ausencia_de_resposta` | Absorção do dia, vias declaração/ausência, `resposta='validar'`. `app/chamados/services/materializacao.py:548-549` |
| `_materializar_resposta_hora` | Via de identidade `paridade_recusada`. `app/chamados/services/materializacao.py:899` |
| `_carimbar_recusa` | Via de identidade `fato_ocupado` quando aplicável. `app/chamados/services/materializacao.py:1074-1075` |
| `_materializar_uma` | Via declaração ou via sem declaração depois de produzir/conferir efeito. `app/chamados/services/materializacao.py:1243-1254` |
| `materializar_pendentes_aprovadas` | Via `juiz_aprovou`. `app/chamados/services/materializacao.py:1393-1406` |
| `desfazer_carimbo_sem_lastro` | Retira carimbo de pergunta que o C6 já considera viva, sob lock. `app/chamados/services/relavra_via.py:221-231` |
| `lavrar_sem_turno_aberto` | Via `sem_turno_aberto`, sujeita ao C6 com carimbo aplicado. `app/chamados/reconciliador.py:405` |
| `retratar_perguntas_absolvidas_por_celula` | Via `celula_absolveu`, sujeita ao C6. `app/chamados/reconciliador.py:586` |

Os commands não viram escritores extras ao chamar essas portas: encerrar por fato chama `encerrar_por_fato_no_espelho`; reabrir órfãs chama `reabrir_fantasma`; curar limbo chama `carimbar_resposta_pela_trilha`/`reabrir_para_correcao`; relavrar I6 chama `relavrar`. `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:147`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67-69`, `app/chamados/management/commands/relavrar_via_i6.py:100-102`.

**Estado pode mudar sem escrita na pergunta:** tranca da competência, decisão administrativa/veredito da célula, lâmpada do marco, batida concordante com resposta, classificação da falta, janela temporal e lastro externo são lidos pelo C6. Portanto um censo de `save` na pergunta não fecha todas as mudanças da vitalidade, e relógio/expiração não equivalem a uma decisão append-only já existente. `app/chamados/juizes.py:1135-1218`, `app/chamados/veredito_lavrado.py:95-118`, `app/chamados/veredito_lavrado.py:173-185`.

**Leitores/semântica relevantes ao item 6:**

- `estado` lê os três carimbos e o C6; `.viva` delega diretamente ao C6. `q_vivas` é deliberadamente um prefiltro generoso e não uma implementação equivalente. Logo “estado P/A”, “viva” e “casou q_vivas” não são sinônimos. `app/chamados/models.py:2491-2506`, `app/chamados/models.py:2760-2796`, `app/chamados/models.py:2798-2809`.
- `respondida_em` sozinho decide `.respondida`, enquanto o C6 exige também texto não vazio para o ramo da resposta viva; `responder_automaticamente` testa apenas texto para decidir no-op. Há significados diferentes de “tem resposta”. `app/chamados/models.py:2862-2864`, `app/chamados/juizes.py:1150-1162`, `app/chamados/services/disputa_emissao.py:1431-1435`.
- O materializador seleciona pela nulidade de `materializada_em`; já o C6 pode declarar viva uma pergunta carimbada e morta uma sem carimbo. O help text “batida criada + chamado fechado” não cobre vias fantasma, duplicata, admin e ausência. `app/chamados/services/materializacao.py:1344`, `app/chamados/juizes.py:1135-1218`, `app/chamados/models.py:2827-2829`, `app/chamados/models.py:2625-2632`, `app/chamados/models.py:2713-2721`.
- `validada_em` é instante da decisão no serviço; a porta genérica também pode preenchê-lo automaticamente com o instante do carimbo. Portanto sua existência não prova, sozinha, validação humana. O painel exclui o usuário sistema ao contar validação humana. `app/chamados/services/validacao.py:127-133`, `app/chamados/models.py:2587-2594`, `app/chamados/services/painel.py:255-258`.
- O cache interpreta `julgado_em` como instante da avaliação e `janela_expira_em` como **data** de vencimento. `expira_em` parte de `datetime.date.today()`, enquanto `esta_stale` usa `timezone.now()` e `.date()`. Isso prova referências temporais diferentes no código, não prova divergência concreta de produção. `app/chamados/veredito_lavrado.py:95-118`, `app/chamados/veredito_lavrado.py:173-185`, `app/chamados/veredito_lavrado.py:227-229`.
- `data_evento` é declarado como snapshot do evento, usado como dia do fato para recalcular e como chave de célula; conversa sem data recebe `timezone.localdate()` de nascimento. Assim o mesmo campo já representa dia do fato/turno e dia civil da conversa. `app/chamados/models.py:2218-2220`, `app/chamados/services/validacao.py:404-408`, `app/chamados/models.py:2422-2430`.
- `recusa_motivo` é lido como causa para desfazer validação, mas também guarda referência de duplicata e pendência de tipo; não é sequência histórica nem um único gênero de decisão. `app/chamados/services/validacao.py:180-185`, `app/chamados/services/fio.py:891-895`, `app/chamados/services/materializacao.py:820-826`.
- A conferência da data lê a divergência entre dia do fato e dia apresentado pelo chamado e registra justificativa; não converte `data_evento` nem unifica os relógios. `app/chamados/services/validacao.py:611-625`.

O banco instalado, a execução efetiva dessas portas em produção, o passivo e qualquer número citado em comentários como medição histórica permanecem **[nao verificado]**.

## 06/11 22:44 · sessao 720860fb5d12 · - · - · origem=Ronald (tmux)

Todos os caminhos abaixo são relativos à raiz; nenhuma escrita executada. O esquema efetivamente instalado em produção permanece [nao verificado].

**(3) Identidade: um `create` central, seis chamadas de produção.**

| Sítio | Busca/chave anterior à criação | Destino |
|---|---|---|
| `app/chamados/models.py:2398` `abrir` | `do_fato`: colaborador via disputa + motivo + `data_evento`, ordenado por PK (`:2392`); pega **última** (`:2431`). Célula é consultada por colaborador+data (`:2430`), mas não integra a chave. | `cls.objects.create` em `:2476`; reaproveitamento muda célula/destino, sem transferir a disputa (`:2435`, `:2450`, `:2474`). |
| `app/chamados/services/disputa_emissao.py:389` `gerar_perguntas_disputa` | Sem data: disputa+motivo (`:299`); motivos datados: disputa+motivo+data (`:370`, `:386`); depois passa pelo dedup global de `abrir`. | Data derivada do evento ou fallback (`:396`). |
| `app/chamados/services/disputa_emissao.py:770` `_criar_pergunta_se_couber` | `_ja_perguntou` (`:765`) usa disputa+motivo+data (`:1237`) e `fio_vivo`, não simples existência (`:1243`). | `abrir` com data/destino (`:771`, `:776`). |
| `app/chamados/services/disputa_emissao.py:1195` âncora | Existe âncora não materializada por chamado_destino+motivo (`:1149`); disputa aberta por colaborador (`:1156`). | `abrir` **sem data**, motivo `ancora_ausente` (`:1197`). |
| `app/chamados/services/acoes_disputa.py:259` reabertura admin | Última disputa fechada do chamado (`:252`), sem dedup de pergunta próprio. | `abrir` sem data (`:260`): confirmação de conversa. |
| `app/chamados/services/declaracao_texto.py:325` declaração colab | `_candidata_reusavel` (`:313`) pega **primeira reutilizável** de disputa+motivo+dia (`:226`), depois fallback a `abrir`. | Motivo reduzido de posição/tipo (`:299`, `:300`), dia informado (`:326`). |
| `app/chamados/services/declaracao_texto.py:614` aplicar declaração admin | Mesma candidata (`:611`), depois `abrir`. | Motivo e dia da proposta (`:614`). |

Na contagem: **1 algoritmo central de acha/cria, 6 chamadores**; nenhuma chamada concreta a `PerguntaDisputa.objects.get_or_create/update_or_create` encontrada [nao verificado como garantia além da busca estática]; central efetivo é `app/chamados/models.py:2431` / `:2476`.

A conversa nasce sempre: o parâmetro original `data_evento=None` desliga o dedup e é convertido ao **dia civil local da emissão**, `timezone.localdate()` (`app/chamados/models.py:2424`). Logo o mesmo campo guarda dia de fato/turno e dia civil da conversa; não há flag persistida distinguindo isso (`:2427`, `:2476`).

**Constraint e locks.** `PerguntaDisputa.Meta` só declara nomes e ordenação (`app/chamados/models.py:2857`); a migration criadora também só esses options (`app/chamados/migrations/0014_perguntadisputa.py:31`). Nenhuma unicidade composta da pergunta está declarada ali; presença real de índice/constraint em PROD [nao verificado]. Há PK e FKs (`app/chamados/models.py:2209`, `:2211`), e índices simples de cache (`:2348`, `:2352`, `:2358`), não a identidade pedida.

`abrir` não trava a busca `last` nem envolve busca+create em atomic; `save` apenas envolve cada gravação individual em atomic (`app/chamados/models.py:2431`, `:2476`, `:2853`). Há locks **externos e parciais**:

- `_abrir_ou_agregar_disputa`: atomic + `select_for_update` na disputa **existente**, por colaborador+aberta (`app/chamados/services/disputa_emissao.py:466`). `criar_ou_atualizar_disputa` só gera as perguntas **depois** do retorno desse helper (`:598`, `:605`). Portanto o helper não estabelece por si só lock cobrindo `last→create` da pergunta.
- Declarações colab/admin mantêm atomic externo (`app/chamados/services/declaracao_texto.py:302`, `:605`), e usam `disputa_para_declaracao` (`:310`, `:608`), que chama o helper travado (`app/chamados/services/disputa_emissao.py:563`).
- Âncora: atomic + lock na disputa existente (`app/chamados/services/disputa_emissao.py:1148`, `:1156`), mas teste de existência da pergunta ocorre antes do lock (`:1149`).
- Agrupador de órfãos tem atomic (`app/chamados/services/disputa_emissao.py:898`) mas a seleção da disputa atual é **sem** `select_for_update` (`:935`), apesar da docstring prometer lock (`:881`).
- Reabertura admin não declara lock próprio (`app/chamados/services/acoes_disputa.py:252`, `:259`).
- Command `disparar_perguntas_competencia`: dedup de existência colab+motivo+dia (`:211`), depois chama fábrica (`:280`), com atomic em apply (`:288`); a fábrica chama `criar_ou_atualizar_disputa` e `gerar_perguntas_disputa` (`app/chamados/services/fabrica_da_celula.py:139`), sem lock próprio da identidade da pergunta.
- Command `smoke_chamados` também chama helper e depois gerador (`app/chamados/management/commands/smoke_chamados.py:99`, `:102`).

A corrida não foi executada/provada em banco [nao verificado]; a janela estática `last→create` é visível em `app/chamados/models.py:2431` / `:2476`.

**Instância de marco candidata.** A pergunta só tem FK célula, motivo e data, sem campo de marco/ordem (`app/chamados/models.py:2209`, `:2216`, `:2218`). O vocabulário existente apropriado é a chave do DNA da célula, **`hi/hii/hfi/hii2/hfi2/hf`**, na ordem canônica declarada por `CelulaDia.chaves_do_turno` (`app/escala/models.py:1652`, `:1659`), exposta por `marcos_presentes` como `(tipo,chave,hora)` (`:1672`). Candidato de desenho, não campo já existente: `instancia_marco = chave` qualificada pela célula; a decisão sobre identidade através de regeneração do DNA [nao verificado].

Há prova concreta de perda da instância: `_MOTIVO_POR_MARCO` só distingue início/fim/ida/volta (`app/chamados/services/declaracao_texto.py:42`); marcos internos são todos reduzidos a `int_ida` ou `int_volta` pelo tipo (`:118`). O juiz admite que duas pausas deixam “duas luzes para uma resposta” e retorna indeterminado (`app/chamados/juizes.py:1106`). Leitores do vocabulário de marco: cartório lê `chaves_do_turno` (`app/ponto/services/cartorio.py:586`); pauta lê `marcos_presentes` (`app/pautas/services.py:417`).

**(4) Absorção: reconstrução por coincidência, sem fato/FK do ato.**

`_materializar_ausencia_de_resposta` cria/retorna ausência por colab+data+tipo (`app/chamados/services/materializacao.py:520`), captura **um** `agora` (`:529`), varre perguntas-hora não materializadas da mesma disputa/data (`:531`) e tenta carimbar cada uma com esse instante (`:549`). O escape e irmãs mudas usam via `ausencia_dia`; quem declarou hora usa `declaracao` (`:548`). O carimbo pode ser recusado pelo C6, e a irmã então fica fora do grupo, com evento `irma_viva_na_ausencia` (`:550`, `:554`).

O leitor `perguntas_absorvidas_por` limita colab/dia/tipo de pergunta/via (`app/chamados/services/materializacao.py:605`), encontra escapes compatíveis com o tipo da ausência e extrai pares `(disputa_id,materializada_em)` (`:602`, `:609`), recupera o grupo (`:614`) e exclui perguntas com batida-origem viva (`:616`). Portanto a ligação é hoje **inferida** por `(disputa,data_evento,materializada_em)` + tipo/dia da ausência, não registrada como ato explícito (`:588`).

Esmeril adicional: a docstring chama `(colab,tipo,data)` de “chave única da ausência” (`app/chamados/services/materializacao.py:595`), mas o modelo atual declara unicidade **parcial somente não rejeitada** (`app/ponto/models.py:827`). Ao recriar/reabrir, o serviço escolhe a última rejeitada do tipo/dia (`app/ponto/services/ausencia.py:254`). Assim o trio não identifica inequivocamente uma ausência histórica rejeitada; a ligação histórica por FK permanece ausente (`app/chamados/services/materializacao.py:588`).

O ato que precisaria carimbar ligação explícita é a própria absorção bem sucedida: ausência efetiva devolvida por `_criar_ausencia` + pergunta escape + irmãs aceitas pela porta + instante comum (`app/chamados/services/materializacao.py:520`, `:529`, `:549`). Hoje só a irmã **recusada** recebe evento com IDs explícitos de escape/ausência (`:554`).

**Quem mais escreve os três campos:**

- `disputa`: atribuição no nascimento (`app/chamados/models.py:2476`); não encontrei transferência posterior da FK da pergunta. Reapontar altera `chamado_destino`, não disputa (`app/chamados/services/fio.py:845`).
- `data_evento`: nascimento (`app/chamados/models.py:2476`); `reapontar_resolvedora` preenche apenas se vazia (`app/chamados/services/fio.py:832`, `:834`, `:847`). `None` no ramo de recusa é só desfazer em memória (`:843`).
- `materializada_em`: atribuição única em `_escrever_carimbo` (`app/chamados/models.py:2558`), persistida por `carimbar` (`:2609`), `carimbar_identidade` (`:2644`), `descarimbar` (`:2652`) e limpeza em lote (`:2699`). Chamadores com outros atos compartilham o campo:
  - fechar admin (`app/chamados/models.py:2522`);
  - fantasma/reabrir fantasma/por resposta/por competência (`:2721`, `:2732`, `:2749`, `:2756`);
  - descarimbo na ressurreição (`:1504`);
  - duplicata (`app/chamados/services/fio.py:891`);
  - reabertura correção (`app/chamados/services/disputa_emissao.py:1414`);
  - chamado resolvido (`:1617`) e resolução sem resposta (`:1691`);
  - paridade/colisão/materialização/juiz aprovou (`app/chamados/services/materializacao.py:899`, `:1074`, `:1252`, `:1406`);
  - sem turno/célula absolveu (`app/chamados/reconciliador.py:405`, `:586`);
  - desfazer carimbo sem lastro (`app/chamados/services/relavra_via.py:231`).
  - revalidação externa monta `descarimbar(salvar=False)` em memória (`app/chamados/models.py:1455`) e restaura estado para consultar juiz; checar contexto antes de contar como gravação.
  
O help_text diz “batida criada + chamado fechado” (`app/chamados/models.py:2829`), enquanto o campo também representa ausência, fantasma, duplicata e recusa (`app/chamados/services/materializacao.py:549`, `app/chamados/models.py:2721`, `app/chamados/services/fio.py:891`, `app/chamados/services/materializacao.py:899`). O significado não é uniforme; usar esse instante como identidade permanente de absorção depende de um campo reutilizado e apagável (`app/chamados/models.py:2699`).

**Leitores para mapa do item (6):**

| Campo | Leitores comprovados |
|---|---|
| colaborador via disputa | identidade `app/chamados/models.py:2392`; app `app/chamados/services/pergunta_no_app.py:46`; folha agrupa `app/folha/previa.py:67`; absorção `app/chamados/services/materializacao.py:606`. |
| disputa / disputa_id | candidata por disputa `app/chamados/services/declaracao_texto.py:227`; absorção agrupa `app/chamados/services/materializacao.py:609`; explicador recorta disputa `app/chamados/services/explicador.py:122`; juiz usa colaborador da disputa `app/chamados/juizes.py:1200`. |
| motivo | identidade `app/chamados/models.py:2392`; candidata `app/chamados/services/declaracao_texto.py:227`; fábrica agrupa/dedup por motivo `app/chamados/services/fabrica_da_celula.py:122`; declaração reduz marco a motivo `app/chamados/services/declaracao_texto.py:300`. |
| data_evento | identidade `app/chamados/models.py:2393`; materializador escolhe dia da batida `app/chamados/services/materializacao.py:344`; absorção `:606`; folha agrupa dia `app/folha/previa.py:65`; expiração `app/chamados/veredito_lavrado.py:112`; juiz janela `app/chamados/juizes.py:1197`; fábrica dia da célula `app/chamados/services/fabrica_da_celula.py:118`; badge/modelo `app/chamados/models.py:373`; app dia `app/chamados/services/pergunta_no_app.py:92`. |
| celula | gate reaponta `app/chamados/models.py:2435`; juiz usa veredito admin e acusação `app/chamados/juizes.py:1147`, `:1181`; outbox escolhe célula ou fallback colab+dia `app/chamados/models.py:2681`. |
| materializada_em | `estado` (`app/chamados/models.py:2501`); C6 (`app/chamados/juizes.py:1171`, `:1177`, `:1207`); SQL vivas (`app/chamados/models.py:2784`); absorção (`app/chamados/services/materializacao.py:609`); frescor da lavra via `Max` (`app/chamados/veredito_lavrado.py:162`); fila materialização (`app/chamados/services/materializacao.py:1347`). |
| chamado_destino | guarda de costura (`app/chamados/services/fio.py:838`); reuso declaração (`app/chamados/services/declaracao_texto.py:314`); absorção decide reconciliar (`app/chamados/services/materializacao.py:563`); juiz/estado usa FK por outras funções [nao verificado para lista completa]. |
| marco/chave/ordem | `CelulaDia.marcos_presentes` (`app/escala/models.py:1679`), cartório (`app/ponto/services/cartorio.py:586`), pauta (`app/pautas/services.py:417`); declaração perde chave ao reduzir posição+tipo (`app/chamados/services/declaracao_texto.py:118`). |

Mistura temporal adicional: o decodificador de contexto prioriza `data_turno`, mas também aceita `data_batida`, data textual de timestamp de entrada e horário previsto (`app/chamados/data_evento.py:27`); `date.fromisoformat(v[:10])` extrai a data escrita sem conversão de timezone (`:11`). O relógio de expiração usa `datetime.date.today()` (`app/chamados/veredito_lavrado.py:112`), conversa usa `timezone.localdate()` (`app/chamados/models.py:2427`), e batida materializada combina `data_evento` com HH:MM e depois ajusta overnight (`app/chamados/services/materializacao.py:414`, `:420`). Equivalência universal de dia civil/dia de turno não está comprovada e o código mostra domínios distintos nesses pontos.

_2646 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
