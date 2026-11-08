# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 09:30** · publicado em **08/11/2026 09:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 09:30 · sessao 1eb7712af56e · ATA-X-TURNO-PAUSA · verificador · origem=Code

**Parecer: bloqueia o pouso.** Há falsa acusação com duas pausas, perda dos dados da pausa na lista do admin e divergências entre os caminhos que deveriam consultar o mesmo juiz (`app/ponto/motor_calculo_v2.py:2780`; `app/ponto/services/cxr_na_lista.py:30`; `app/escala/utils.py:2132`; `app/ponto/turnos.py:1659`).

A execução da suíte integrada, o impacto financeiro em produção e a história dos 821 commits permanecem **[nao verificado]**. As reproduções mencionadas abaixo executaram funções reais extraídas por AST em memória; não equivalem à execução completa com Django/banco **[nao verificado]**. [nao verificado]

**Achados — A, C e E**

1. **BLOQUEIA — duas pausas corretas produzem `l084_pausa` falsa.** O laço obtém apenas `hii/hfi` de `marcos_do_dia` e compara **todos** os intervalos reais com esse primeiro par. O main já possui `pausas_do_dia` para devolver todos os pares declarados (`app/ponto/motor_calculo_v2.py:2771`; `app/ponto/motor_calculo_v2.py:2780`; `app/escala/models.py:1166`).

   Caso concreto: previstas **09:15–09:45 e 14:15–15:40**, reais exatamente iguais. A segunda pausa é comparada com 09:15–09:45 e gera deltas **+300/+355 minutos**, suficientes para acusar cadastro incorreto. A reprodução isolada confirmou essa linha; o predicado exige ambas as distâncias absolutas maiores que 180 (`app/ponto/motor_calculo_v2.py:1699`; `app/ponto/motor_calculo_v2.py:1713`; `app/ponto/motor_calculo_v2.py:1989`).

   Isso também pode mudar o dono do dia para CADASTRO, porque E6 consulta a presença do dia na lista sem distinguir a causa; BATIDA ainda precede essa decisão (`app/ponto/management/commands/e6_oraculo.py:104`; `app/ponto/management/commands/e6_oraculo.py:110`; `app/ponto/management/commands/e6_oraculo.py:170`).

2. **BLOQUEIA — CXR-TELA perde os horários e deltas da pausa.** O produtor escreve `pausa_real`, `pausa_cadastrada`, `delta_ida_min` e `delta_volta_min`; a allowlist da lavra não copia esses campos. O formatador C12 reconhece somente os formatos antigos de janela HE e entrada/saída (`app/ponto/motor_calculo_v2.py:1713`; `app/ponto/services/cxr_na_lista.py:30`; `app/ponto/services/cxr_na_lista.py:86`; `app/escala/services/cadastro_realidade.py:394`).

   Para a linha do caso col841, o detalhe resultante é **“entrada real - x cadastrada - (None min), saida real - x cadastrada - (None min)”**. A causa sobrevive na lavra, mas esse ramo nem a apresenta; o admin recebe a acusação sem sua evidência (`app/escala/services/cadastro_realidade.py:398`; `app/escala/services/cadastro_realidade.py:404`).

3. **BLOQUEIA — a cura continua fora do juiz registrado de ocupação do marco.** `_match_marcos` é a autoridade registrada; o bloco novo muda sua resposta posteriormente, somente na grade. A Zona 5 e `marco_ocupado_pela_batida` continuam consultando a fonte sem essa reconciliação (`app/core/juizes.py:200`; `app/escala/utils.py:427`; `app/escala/utils.py:2132`; `app/ponto/motor_calculo_v2.py:3127`).

   Caso concreto do próprio teste: jornada **07–17**, pausa **12:38–13:38**, batidas **06:54 E / 12:38 S / 12:54 E / 17:05 S**. A grade curada exige **hfi=12:54, delta −44**. Na reprodução isolada, `marco_ocupado_pela_batida` com as quatro batidas devolveu `(None, None)` para 12:54; com somente o segundo período, devolveu `hii,+16`. Esse último é o formato passado por `cumpriu_a_volta_do_intervalo`, que recusa a classificação como volta (`app/escala/tests/test_ata_x_turno_pausa.py:139`; `app/escala/tests/test_ata_x_turno_pausa.py:154`; `app/escala/regua_defesa.py:491`; `app/escala/regua_defesa.py:493`).

   **Não é inteiramente redundante com o main:** o alinhamento por papel já senta os pares, mas seu cluster final ainda pode retirar uma perna; a nova reconciliação volta a acendê-la. Há duas respostas para a ocupação, em vez de uma cura completa na fonte (`app/escala/utils.py:561`; `app/escala/utils.py:577`; `app/escala/utils.py:2132`).

4. **BLOQUEIA — “uma entrada só” ainda recebe universos diferentes.** O motor remove duplicatas **antes** de produzir o papel; `turnos_do_colab` produz o papel sobre as batidas carregadas. Isso altera a guarda que exige exatamente uma batida por minuto de luz (`app/ponto/motor_calculo_v2.py:882`; `app/ponto/motor_calculo_v2.py:893`; `app/ponto/turnos.py:1659`; `app/ponto/turnos.py:1942`).

   Contraexemplo reproduzido isoladamente: DNA **07–17 / pausa 12–13**, ata com quatro luzes, cruas **E07:00:01, E07:00:05, S12:00, E13:00, E17:00**, última E gravada cumprindo a saída na ata. Com cruas, o papel fica vazio e surgem três turnos abertos; após o dedupe do motor, o papel é `E/Xi/Xi/S` e surge um turno fechado. A segunda E é removida pelo predicado de duplicata (`app/ponto/motor_calculo_v2.py:921`; `app/ponto/turnos.py:1659`; `app/ponto/turnos.py:1833`; `app/ponto/turnos.py:1380`).

5. **BLOQUEIA a declaração de neutralidade financeira da mudança (3).** O acusador, isoladamente, apenas acrescenta relato; a alteração do papel alimenta caminhos monetários: substituição de períodos, atribuição do dia, julgamento L084 da janela e crédito dos pares de turnos abertos (`app/ponto/motor_calculo_v2.py:1713`; `app/ponto/motor_calculo_v2.py:685`; `app/ponto/motor_calculo_v2.py:2396`; `app/ponto/motor_calculo_v2.py:2447`; `app/ponto/motor_calculo_v2.py:1594`; `app/ponto/motor_calculo_v2.py:2561`).

   Forma concreta noturna reproduzida: DNA **19–07 / pausa 03–04**, luzes **18:56/23:59/03:00/07:01**, cruas **E18:56:01, E18:56:05, S23:59, E03:00, E07:01**. Sem papel, a cauda das 03:00 fica em D+1; com papel após dedupe, fica em D, dentro do turno fechado. O mapa alterado participa dos filtros e da classificação; comercial/partido também o usam para agrupar e recalcular rubricas (`app/ponto/motor_calculo_v2.py:905`; `app/ponto/motor_calculo_v2.py:2392`; `app/ponto/motor_calculo_v2.py:2986`; `app/ponto/motor_calculo_v2.py:3390`). **Valor líquido e rubricas efetivamente alteradas nesse exemplo: [nao verificado].**

6. **FILA — outro contador continua tratando qualquer CXR como isenção de pontualidade.** `diff_reclassificar_partido` coleta toda a lista, soma `len()` e anuncia dias que “deixam de gerar atraso/saida antecipada”. A nova pausa aumenta esse número, embora apenas acuse; seus exemplos também procuram campos de entrada/saída inexistentes (`app/ponto/management/commands/diff_reclassificar_partido.py:188`; `app/ponto/management/commands/diff_reclassificar_partido.py:260`; `app/ponto/management/commands/diff_reclassificar_partido.py:262`; `app/ponto/management/commands/diff_reclassificar_partido.py:269`).

7. **FILA — a chamada nova consulta vínculo apesar do comentário “ALIMENTAÇÃO, NUNCA CONSULTA”.** A omissão de `escalas=` faz `papel_por_minuto_da_ata` chamar `_escalas_da_ata`, com consulta e escolha por `ativa/data_inicio`, em vez de receber diretamente o vínculo da fatia. A dívida dessa escolha por período já está documentada no carregador (`app/ponto/motor_calculo_v2.py:885`; `app/ponto/motor_calculo_v2.py:893`; `app/ponto/turnos.py:1596`; `app/ponto/turnos.py:1544`; `app/ponto/turnos.py:1561`). Impacto de desempenho: **[nao verificado]**.

**B — quem escreve e quem lê**

| Campo/valor | Escritores | Leitores e significado |
|---|---|---|
| `dias_cadastro_x_realidade` | Nasce no resultado; lista privada reiniciada e publicada: `app/ponto/motor_calculo_v2.py:289`, `:2326`, `:2784`. | Fatias concatenam, sem interpretar causa: `app/ponto/services/fechamento.py:96`, `:294`. |
| Itens `dia/causa` | L084 do dia: `app/ponto/motor_calculo_v2.py:1634`; janela HE: `:1682`; pausa: `:1713`; L084 antigo sem `causa`: `:2035`. | E6 projeta qualquer `dia` não nulo em conjunto de datas: `app/ponto/management/commands/e6_oraculo.py:170`. |
| Linha `l084_pausa` | Laço passa explicitamente **dia do turno**; horários são ISO com deslocamento, deltas são minutos: `app/ponto/motor_calculo_v2.py:2781`, `:1714`, `:1716`. | CXR converte `dia→data`, filtra competência e deduplica itens; perde os campos novos: `app/ponto/services/cxr_na_lista.py:86`, `:88`, `:95`, `:107`. |
| `_l084_pausa_acusada` | Criação/adição no acusador e reset por cálculo: `app/ponto/motor_calculo_v2.py:1706`, `:1709`, `:2327`. | Consulta de dedupe por dia no próprio acusador; nenhum outro consumidor encontrado: `app/ponto/motor_calculo_v2.py:1707`. |
| `_turnos_juiz_calc` | Atribuído por cálculo: `app/ponto/motor_calculo_v2.py:2440`. | Janela L084 `:1595`; crédito de aberto `:2561`; acusador novo `:2767`. |
| `_dia_turno_juiz_calc` | Atribuído por cálculo: `app/ponto/motor_calculo_v2.py:2441`. | Janela `:1594`; fallback da pausa `:1703`; relato L084 `:2040`. |
| `papel_por_minuto` | Produzido de ata/DNA: `app/ponto/turnos.py:1568`, `:1808`, `:1809`, `:1833`; alimentação nova: `app/ponto/motor_calculo_v2.py:893`. | Passado ao pareador em `app/ponto/turnos.py:1538`; consultado/aplicado em `:903`, `:1308`, `:1314`, `:1380`. |
| Órfãs e células do matcher | `app/escala/utils.py:615`, `:621`, `:655`, `:662`. | Alinhamento às colunas e mapas do dia: `app/escala/utils.py:1726`, `:1778`, `:1779`. |
| `dias_out[…]['celulas'/'orfas']` | Criação `app/escala/utils.py:1822`, `:1828`; conservação civil `:1853`, `:1857`, `:1859`; pontas `:2064`; pausa nova `:2131`. | Ata provisória `:1917`; guardas `:1992`, `:2111`; cartão `:1107`; cartório `app/ponto/services/cartorio.py:701`; espelho `app/ponto/services/espelho.py:685`; supra-juiz `app/ponto/supra_juiz.py:514`. |
| Overlays das mesmas células/listas | Isenção `app/escala/utils.py:858`; folga `:1173`; ausência `:1238`, `:1266`, `:1277`. | A ata provisória é construída depois desses overlays: `app/escala/utils.py:1895`, `:1917`. |

Os leitores **não atribuem todos o mesmo efeito à causa**: E6 usa pertencimento ao dia; CXR conta itens; C12 deveria apresentar evidências; os leitores de HE filtram causas específicas. Não encontrei conversão nova de dia do turno para dia civil nesses leitores; encontrei perda do conteúdo da pausa e o contador incorreto descritos acima (`app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/services/cxr_na_lista.py:126`; `app/escala/services/cadastro_realidade.py:394`; `app/ponto/janela_he.py:281`).

- **CXR muda número legitimamente:** aumenta `total`, `por_causa` e numerador do snapshot; o contrato declara contagem de **itens**, inclusive vários no mesmo dia (`app/ponto/services/cxr_na_lista.py:14`; `:123`; `:126`; `:147`).
- **`diff_janela_he`: não encontrei defeito novo no filtro.** Exclui explicitamente `l084_pausa` (`app/ponto/management/commands/diff_janela_he.py:72`).
- **Espelho/HE:** `pontas_do_relato` aceita causas contendo “janela de HE”; o resumo acima do teto aceita somente a causa de chegada. A pausa não entra nesses subconjuntos (`app/ponto/janela_he.py:281`; `app/ponto/services/espelho.py:1260`).
- **FechamentoMensal/lavra financeira:** não encontrei persistência da lista nem consumo direto dela para rubricas; a composição transporta a lista e o instrumento documenta que o fechamento não a persiste (`app/ponto/services/fechamento.py:97`; `app/ponto/services/fechamento.py:294`; `app/ponto/management/commands/diff_reclassificar_partido.py:169`).
- **Fallback `dia=None`: nota.** O mapa real indexa batidas gravadas E, mas a ida da pausa costuma ser S; o teste direto injeta artificialmente essa S no mapa. O chamador de produção evita essa falha passando `dia=` explicitamente (`app/ponto/motor_calculo_v2.py:905`; `app/ponto/tests/test_ata_pausa_l084_cxr.py:23`; `app/ponto/motor_calculo_v2.py:2781`).

**C — caminhos com e sem células**

| Caminho | Alimentação |
|---|---|
| Espelho | Recebe células: `app/ponto/services/espelho.py:1131`. |
| PDF | Recebe células: `app/relatorios/pdf_espelho.py:405`. |
| Fechamento/lote, fatias normais | Recebe células: `app/ponto/services/fechamento.py:750`. |
| Fechamento, batidas órfãs de vínculo | Também recebe células: `app/ponto/services/fechamento.py:825`. |
| `esmeril_espelho` | Cria/calcula sem alimentar células: `app/ponto/services/esmeril_espelho.py:161`, `:165`. |
| `vigiar_motor_autoridade` | Idem: `app/ponto/management/commands/vigiar_motor_autoridade.py:57`. |
| `diff_extrato_parcial` | Idem: `app/relatorios/management/commands/diff_extrato_parcial.py:61`, `:68`. |

O papel novo permanece `None` sem alimentação, sem células daquele colaborador ou sem identificador; ata desalinhada ou sem bordas suficientes também faz o produtor se abster. Portanto, fechamento/lote **não são genericamente o caminho sem células** no main atual (`app/ponto/motor_calculo_v2.py:886`; `:888`; `:891`; `app/ponto/turnos.py:1639`; `:1685`; `app/ponto/services/fechamento.py:750`).

**D — checklist**

- **Segundo juiz/derivação paralela: encontrado**, no pós-processamento da ocupação e nos universos diferentes do papel, achados 3 e 4 (`app/core/juizes.py:200`; `app/escala/utils.py:2132`; `app/ponto/motor_calculo_v2.py:882`).
- **Segundo escritor persistente fora da porta: não encontrei novo em produção.** O bloco altera a grade em memória; a persistência segue cartório → `lavrar_veredito` → porta (`app/escala/utils.py:2132`; `app/ponto/services/cartorio.py:1802`; `app/ponto/portas/celula.py:1028`).
- **Relógio novo fora da borda: não encontrei no diff.** Os acréscimos derivam papel, instantes dos marcos e relato (`app/ponto/motor_calculo_v2.py:882`; `:1699`; `:2777`).
- **Ordem de trava: não encontrei aquisição ou alteração de trava nos trechos novos** (`app/escala/utils.py:2084`; `app/ponto/motor_calculo_v2.py:2764`; `app/ponto/turnos.py:1537`).
- **Pré-requisito “juiz único”: não satisfeito**, pelo contraexemplo de ambiguidade no minuto (`app/ponto/turnos.py:1659`; `app/ponto/motor_calculo_v2.py:893`).
- **ESMERIL-SEMÂNTICA: encontrado**, tanto no contador que chama itens de pausa de dias isentos quanto na comparação da segunda pausa com a primeira (`app/ponto/management/commands/diff_reclassificar_partido.py:262`; `app/ponto/motor_calculo_v2.py:2780`).
- **Testes:** os testes da ata verificam grade final e convergência; os outros dois arquivos verificam predicado/dedupe e presença da acusação no espelho. Não certificam a igualdade com `_match_marcos`, os caminhos sem células, CXR-TELA ou neutralidade das rubricas (`app/escala/tests/test_ata_x_turno_pausa.py:105`; `:193`; `app/ponto/tests/test_ata_pausa_l084_cxr.py:27`; `:56`; `app/ponto/tests/test_juiz_do_par_uma_entrada.py:54`). RED por retirada/mutação e suíte completa: **[nao verificado]**.

**E — demais contraexemplos**

- **D+1 civil:** a escolha da lista usa o instante e a origem registrada em `_orfa_civil`; o relato continua com `data_turno`. Não encontrei erro novo nessa seleção para o caso normal de uma pausa noturna (`app/escala/utils.py:1847`; `:2092`; `app/ponto/motor_calculo_v2.py:2781`).
- **Célula/slot ausente:** interrompe a cura; **não encontrei escrita parcial** nessas guardas (`app/escala/utils.py:2116`; `:2125`).
- **Marco ocupado por outra hora:** impede a ascensão das duas pernas. A identificação de “já ocupada por esta batida” continua somente por HH:MM; colisão completa entre datas permanece **[nao verificado]** (`app/escala/utils.py:2119`; `:2125`).
- **Duas pausas na ata:** além da falsa acusação bloqueante, a cura visual pula qualquer caso com dois pares ou seis marcos; não cobre essa classe já suportada pelo main (`app/escala/utils.py:2087`; `app/escala/models.py:1166`).
- **FILA — expectativa reaplicada incompatível com o alinhamento atual:** no cenário com E02:05 extra e par conhecido 21:44–22:41, a fonte senta o par antes das soltas; a reprodução isolada deixa 02:05 órfã e `hii` ausente. O teste exige 02:05 sentada e nenhuma célula ausente. Resultado integrado desse fixture: **[nao verificado]** (`app/escala/utils.py:561`; `:564`; `:577`; `app/escala/tests/test_ata_x_turno_pausa.py:297`).

## 08/11 09:28 · sessao f4c15bc06c56 · - · - · origem=Ronald (tmux)

Achados da escala/ata:

1. **Bloqueia — cura fora do juiz registrado mantém respostas diferentes para o mesmo marco.** O juiz registrado é `_match_marcos` (`app/core/juizes.py:200`); o novo bloco reescreve sua resposta depois, em `app/escala/utils.py:2131`. No fixture 07:00–17:00, pausa 12:38–13:38, batidas 06:54 E / 12:38 S / 12:54 E / 17:05 S, a grade nova acende **hfi=12:54, delta −44** (`app/escala/tests/test_ata_x_turno_pausa.py:139`, `:153`). Executei os ASTs reais em memória: `marco_ocupado_pela_batida` responde **(None,None)** usando as quatro batidas; usando só o segundo período responde **hii,+16**. Esse segundo formato é precisamente o insumo de `cumpriu_a_volta_do_intervalo`, que retorna falso (`app/escala/regua_defesa.py:491`, `:493`). A Zona 5 consulta `_match_marcos` diretamente e seleciona P2 pelo hfi, sem a reconciliação nova (`app/ponto/motor_calculo_v2.py:3122`, `:3127`, `:3129`). Valor financeiro líquido resultante: **[nao verificado]**.

2. **Fila — expectativa antiga permanece em teste novo.** Com o turno conhecido contendo o par 21:44–22:41 e uma E 02:05 solta, o main senta o par antes das soltas (`app/escala/utils.py:561`, `:564`); E 02:05 perde a disputa do hfi já ocupado. O cluster depois deixa hii missing (`:577`). AST real de `_match_marcos` devolveu hi18:46/hii missing/hfi22:41/hf07:04, órfãs21:44 e02:05. O teste exige nenhuma missing, apenas21:44 órfã e02:05 sentada (`app/escala/tests/test_ata_x_turno_pausa.py:294`, `:297`). Pareador completo e suíte: **[nao verificado]**.

3. **Fila — duas pausas ficam fora da cura.** O bloco exige exatamente um par e quatro marcos (`app/escala/utils.py:2087`), embora o main produza a cadeia de todas as pausas e alinhe pares em ordem (`:1630`, `:1636`, `:539`). Exemplo condicionado a dois pares reconhecidos: jornada07–17, pausas10–10:15/14–14:15, batidas E07/S08/E08:30/S15/E15:30/S17. AST de `_alinhar_por_papel` deixa08/08:30 órfãs e primeira pausa missing; o bloco novo necessariamente pula (`:2087`). Reconhecimento desse turno pelo pareador completo: **[nao verificado]**.

4. **Nota — bloco ainda é vivo no golden; não basta removê-lo como redundante.** No golden, AST real deixa hii missing e21:44 órfã; hfi já recebe22:41. A causa é o cluster final (`app/escala/utils.py:577`). A nova escritura cura hii (`:2131`). O teste golden prende esse comportamento (`app/escala/tests/test_ata_x_turno_pausa.py:101`, `:105`).

Censo agrupado:

| Dado | Escritores | Leitores |
|---|---|---|
| Células/orfãs do matcher | `app/escala/utils.py:615`, `:621`, `:655`, `:662` | alinhamento das colunas `:1726`; mapas `:1778` |
| `dias_out[..]['celulas'/'orfas']` | criação `:1822`, `:1828`; conservação civil/I `:1853`, `:1857`, `:1859`; ordenação `:1863`; pontas `:2064`; pausa `:2131` | ata provisória `:1917`; reconciliação `:1992`, `:2111`; cartão `:1107`, `:1131` |
| Overlays das mesmas listas | folga `:1173`; ausência `:1238`, `:1266`, `:1273`, `:1277`; isenção `:858` | ata provisória montada depois, `:1917` |
| Grade final | — | cartório `app/ponto/services/cartorio.py:701`, `:708`, `:1373`; supra-juiz `app/ponto/supra_juiz.py:230`, `:305`, `:514`; espelho `app/ponto/services/espelho.py:361`, `:685` |

**Não encontrei nova escrita persistente de produção fora da porta.** A escrita nova é sobre listas da grade em memória (`app/escala/utils.py:2131`); a persistência permanece cartório→`lavrar_veredito` (`app/ponto/services/cartorio.py:1802`; `app/ponto/portas/celula.py:969`, `:1001`). Os testes salvam ata diretamente (`app/escala/tests/test_ata_x_turno_pausa.py:206`; `app/ponto/tests/test_juiz_do_par_uma_entrada.py:49`).

Guardas: célula/slot ausente impede cura (`app/escala/utils.py:2116`); marco ocupado por outra hora impede ambas as pernas (`:2119`, `:2125`); D+1 escolhe lista civil usando a origem da órfã e o instante (`:2092`). A comparação da célula já acesa continua só HH:MM (`:2119`); contraexemplo completo com colisão entre datas **[nao verificado]**.

Os testes da ata mordem a retirada do bloco no golden e na volta curta, mas verificam a grade final (`app/escala/tests/test_ata_x_turno_pausa.py:105`, `:146`). A convergência grava e relê a ata sem comparar o juiz de marcos (`:193`). Os outros dois arquivos prendem acusação/ligação no espelho, sem equivalência financeira ou caminhos sem células (`app/ponto/tests/test_ata_pausa_l084_cxr.py:55`; `app/ponto/tests/test_juiz_do_par_uma_entrada.py:53`). Não encontrei comparação nova contra `_match_marcos`, `cumpriu_a_volta` ou Zona 5 nesses três arquivos; execução da suíte **[nao verificado]**.

## 08/11 09:28 · sessao 4a100c439430 · - · - · origem=Ronald (tmux)

Achados da fatia motor/caminhos:

1. **bloqueia — o juiz ainda recebe universos diferentes.** `_turnos_do_juiz` remove duplicatas antes de derivar o papel; `turnos_do_colab` deriva o papel das batidas carregadas, sem esse dedup (`app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:893`, `app/ponto/turnos.py:1942`). A guarda de ambiguidade exige exatamente uma batida em cada minuto de luz (`app/ponto/turnos.py:1659`).

   Contraexemplo executado em memória com funções reais extraídas por AST: DNA 07–17, pausa 12–13, quatro luzes na ata; batidas `E07:00:01 E07:00:05 S12:00 E13:00 E17:00`, última E gravada cumprindo S na ata. Com cruas: papel `{}` e três turnos abertos; com dedup: mapa `E/Xi/Xi/S` e um turno fechado 07–17. A segunda E é retirada pelo predicado de mesmo tipo e gap inferior a três minutos (`app/ponto/motor_calculo_v2.py:70`, `app/ponto/motor_calculo_v2.py:921`). Portanto “uma entrada só” não está satisfeito nessa forma (`app/ponto/turnos.py:1659`, `app/ponto/motor_calculo_v2.py:893`).

2. **bloqueia a declaração de neutralidade monetária da mudança (3) — seus resultados alimentam dinheiro.** O novo mapa é montado antes de `montar_periodos` e usado para excluir períodos substituídos pela ata; também classifica o dia, decide dispensa da janela L084 e credita períodos abertos (`app/ponto/motor_calculo_v2.py:2392`, `app/ponto/motor_calculo_v2.py:2396`, `app/ponto/motor_calculo_v2.py:685`, `app/ponto/motor_calculo_v2.py:2449`, `app/ponto/motor_calculo_v2.py:1594`, `app/ponto/motor_calculo_v2.py:2561`). Comercial e partido chamam esse juiz novamente para agrupar períodos e recalcular HE/intrajornada/feriado (`app/ponto/motor_calculo_v2.py:2986`, `app/ponto/motor_calculo_v2.py:3007`, `app/ponto/motor_calculo_v2.py:3030`, `app/ponto/motor_calculo_v2.py:3390`, `app/ponto/motor_calculo_v2.py:3415`).

   Forma concreta também exercitada por AST: DNA 19–07, pausa 03–04; luzes `18:56/23:59/03:00/07:01`; cruas `E18:56:01 E18:56:05 S23:59 E03:00 E07:01`. Sem papel, a cauda E03:00 fica em D+1; com papel após dedup, fica em D, num turno fechado único. Isso altera o insumo usado pelos ramos monetários acima; valor financeiro final desse caso **[nao verificado]**. Não confundir com `_acusar_pausa_l084`, cujo corpo apenas acrescenta relato (`app/ponto/motor_calculo_v2.py:1694`, `app/ponto/motor_calculo_v2.py:1713`).

3. **fila — “ALIMENTAÇÃO, NUNCA CONSULTA” não corresponde à chamada nova.** A chamada nova omite `escalas=`, levando obrigatoriamente a `_escalas_da_ata`, que consulta e escolhe vínculo por `ativa/data_inicio`; não usa diretamente o vínculo da fatia (`app/ponto/motor_calculo_v2.py:893`, `app/ponto/turnos.py:1596`, `app/ponto/turnos.py:1561`, `app/ponto/turnos.py:1563`). A dívida de escolha por período está documentada no próprio carregador (`app/ponto/turnos.py:1544`). Comercial/partido podem repetir essa carga na segunda chamada do juiz (`app/ponto/motor_calculo_v2.py:2986`, `app/ponto/motor_calculo_v2.py:3390`). Impacto de desempenho medido **[nao verificado]**.

4. **fila — os testes novos não certificam dinheiro nem todos os bocais.** O teste da entrada única só exige presença da acusação no espelho após lavar a ata; não afirma minutos, HE, atraso, intra ou noturno, nem fechamento/lote, nem cruas versus dedup (`app/ponto/tests/test_juiz_do_par_uma_entrada.py:54`, `app/ponto/tests/test_juiz_do_par_uma_entrada.py:61`). Os testes do acusador exercitam predicado/dedupe e presença no espelho, sem comparar essas rubricas (`app/ponto/tests/test_ata_pausa_l084_cxr.py:27`, `app/ponto/tests/test_ata_pausa_l084_cxr.py:36`, `app/ponto/tests/test_ata_pausa_l084_cxr.py:43`, `app/ponto/tests/test_ata_pausa_l084_cxr.py:56`). O teste direto injeta artificialmente a S da ida no mapa de dia, enquanto o produtor real só indexa batidas gravadas E; o laço real evita o problema passando `dia=` explicitamente (`app/ponto/tests/test_ata_pausa_l084_cxr.py:23`, `app/ponto/motor_calculo_v2.py:905`, `app/ponto/motor_calculo_v2.py:2781`). Eficácia por mutação/remover cura **[nao verificado]**.

Censo dos campos solicitados:

| Campo | Escritores | Leitores |
|---|---|---|
| `_turnos_juiz_calc` | atribuído em cada cálculo, `app/ponto/motor_calculo_v2.py:2440` | janela L084 do dia `:1595`; crédito aberto `:2561`, `:2566`; novo acusador `:2767` |
| `_dia_turno_juiz_calc` | `app/ponto/motor_calculo_v2.py:2441` | janela `:1594`; fallback acusador `:1703`; relato L084 `:2040` |
| `_dia_juiz_para_marco` | entrega `app/ponto/motor_calculo_v2.py:2396`; limpa em `finally` `:2400` | filtro de períodos/intervalos `:684`–`:687` |
| `papel_por_minuto` | produzido por `app/ponto/turnos.py:1568`, saídas `:1808`, `:1809`, `:1833`; `_papel` novo no motor `app/ponto/motor_calculo_v2.py:886`, `:893` | repassado `app/ponto/motor_calculo_v2.py:894`, `app/ponto/turnos.py:1538`; leitor `_papel_da_ata` `:888`, consulta `:903`; aplicação no pareador `:1308`, `:1314`, `:1380` |

Caminhos:

- **Espelho/PDF/fechamento/lote normal têm alimentação:** `app/ponto/services/espelho.py:1131`; `app/relatorios/pdf_espelho.py:405`; `app/ponto/services/fechamento.py:750`; caminho órfão `:825`. A apuração por fatia entrega colaborador, vínculo e datas ao motor (`app/ponto/services/fechamento.py:286`).
- **Sem alimentação:** `esmeril_espelho` cria motor e calcula sem atribuir células (`app/ponto/services/esmeril_espelho.py:161`, `:165`); `vigiar_motor_autoridade` idem (`app/ponto/management/commands/vigiar_motor_autoridade.py:57`); `diff_extrato_parcial` idem (`app/relatorios/management/commands/diff_extrato_parcial.py:61`, `:68`). Nesses caminhos o novo papel permanece `None`; células ausentes/vazias, cid ausente, ou nenhuma célula do cid também deixam `None` (`app/ponto/motor_calculo_v2.py:886`, `:888`, `:891`). Ata desalinhada ou dia sem bordas legíveis faz o produtor se abster (`app/ponto/turnos.py:1635`, `:1686`).
- **Bocal já existente:** `turnos_via_autoridade` já derivava e passava papel, com limites `AUT_*`; a obra migra o outro caminho do juiz, não cria o primeiro fornecimento de papel ao motor (`app/ponto/motor_calculo_v2.py:494`, `:500`, `:506`). Porém a receita nova não é literalmente a mesma: `turnos_de_batidas` usa os limites padrão do juiz; o outro caminho passa `AUT_INTRA_MAX_S/AUT_CONT_MAX_S` (`app/ponto/turnos.py:1537`, `app/ponto/motor_calculo_v2.py:504`).

Não encontrei relógio novo, aquisição de trava ou escrita persistente nova nessa fatia; o trecho acrescentado deriva papel e relato em memória (`app/ponto/motor_calculo_v2.py:882`, `:1694`, `:2764`, `app/ponto/turnos.py:1537`). A suíte Django não foi executada: o interpretador disponível não possui Django **[nao verificado]**; as sondas AST acima não executaram ORM nem criaram arquivos.

## 08/11 09:26 · sessao f14fc2900b56 · - · - · origem=Ronald (tmux)

Achados B:

1. **Bloqueia — nova acusação chega à C12 sem evidência da pausa.** O produtor grava `pausa_real`, `pausa_cadastrada`, `delta_ida_min` e `delta_volta_min`, mas a allowlist da lavra não inclui esses campos; a cópia conserva apenas `causa` e metadados. A apresentação C12 conhece apenas os formatos janela de HE e entrada/saída. Caso col841: aparece **“entrada real - x cadastrada - (None min), saida real - x cadastrada - (None min)”**, sem horários, deltas ou indicação textual da pausa. Reproduzi isso em memória com AST das funções reais e insumo sintético do caso, sem Django, banco ou arquivo criado. Fontes: `app/ponto/motor_calculo_v2.py:1713`, `app/ponto/services/cxr_na_lista.py:30`, `app/ponto/services/cxr_na_lista.py:86`, `app/escala/services/cadastro_realidade.py:394`.

2. **Fila — outro contador interpreta pausa como dia isento de pontualidade.** `diff_reclassificar_partido` captura toda a lista, soma `len()` e anuncia “dia-colab ... deixam de gerar atraso/saida antecipada”; uma pausa agora aumenta esse contador, embora o acusador não retire pontualidade. O exemplo ainda procura campos de entrada/saída inexistentes na linha da pausa. Fontes: `app/ponto/management/commands/diff_reclassificar_partido.py:188`, `app/ponto/management/commands/diff_reclassificar_partido.py:260`, `app/ponto/management/commands/diff_reclassificar_partido.py:262`, `app/ponto/management/commands/diff_reclassificar_partido.py:269`, `app/ponto/motor_calculo_v2.py:1713`.

3. **Nota — testes novos não cobrem essa integração com CXR-TELA.** O teste do espelho confere apenas `dia` e `causa` da lista em memória; não passa pela lavra C12 nem confere os campos/detalhe. Fonte: `app/ponto/tests/test_ata_pausa_l084_cxr.py:56`.

Mapa dos escritores:

- Campo público nasce no `ResultadoMes`; lista privada é zerada a cada cálculo e publicada no resultado. Fontes: `app/ponto/motor_calculo_v2.py:289`, `app/ponto/motor_calculo_v2.py:2326`, `app/ponto/motor_calculo_v2.py:2784`.
- Escritor L084 do dia: `{dia, causa='l084_do_dia', entrada/saida ISO, deltas em minutos}`. Fonte: `app/ponto/motor_calculo_v2.py:1634`.
- Escritor janela HE: `{dia, causa, real/cadastrado HH:MM, minutos_fora arredondados}`. Fonte: `app/ponto/motor_calculo_v2.py:1682`.
- Escritor pausa: `{dia, causa='l084_pausa', pares ISO, deltas em minutos}`; o laço passa explicitamente `data_turno`, inclusive para pausa civil D+1. Fontes: `app/ponto/motor_calculo_v2.py:1713`, `app/ponto/motor_calculo_v2.py:2781`.
- Escritor pontualidade L084 antigo: `{dia do mapa do juiz, entrada/saida ISO, deltas em minutos}`, sem `causa`. Fonte: `app/ponto/motor_calculo_v2.py:2035`.
- Composição de fatias concatena a lista, sem reclassificar causas. Fontes: `app/ponto/services/fechamento.py:96`, `app/ponto/services/fechamento.py:294`.
- Lavra CXR converte `dia` em `data`, filtra pela competência, copia campos permitidos, remove itens iguais e grava snapshot. Fontes: `app/ponto/services/cxr_na_lista.py:86`, `app/ponto/services/cxr_na_lista.py:87`, `app/ponto/services/cxr_na_lista.py:95`, `app/ponto/services/cxr_na_lista.py:107`, `app/ponto/services/cxr_na_lista.py:145`.

Mapa dos leitores:

- **E6:** lê qualquer `dia` não nulo, ignora `causa`/instantes/minutos e pergunta por pertença ao conjunto do dia. BATIDA permanece anterior a CADASTRO, portanto estar na lista não garante dono CADASTRO quando há batida ímpar/solta. Fontes: `app/ponto/management/commands/e6_oraculo.py:170`, `app/ponto/management/commands/e6_oraculo.py:195`, `app/ponto/management/commands/e6_oraculo.py:104`.
- **CXR-TELA:** conta itens, não dias; nova pausa aumenta `total`, `por_causa` e numerador lavrado legitimamente pelo rótulo declarado. Contudo perde os dados da pausa — achado 1. Fontes: `app/ponto/services/cxr_na_lista.py:14`, `app/ponto/services/cxr_na_lista.py:123`, `app/ponto/services/cxr_na_lista.py:126`, `app/ponto/services/cxr_na_lista.py:147`.
- **CXR lavrado para dono:** projeta os `data` dos itens em conjunto de datas; ignora causa e minutos. Fonte: `app/ponto/services/cxr_na_lista.py:178`.
- **C12:** lê a lavra e apresenta item por item; usa `data`, vínculo/template e dono lavrados. Fontes: `app/escala/services/cadastro_realidade.py:393`, `app/escala/services/cadastro_realidade.py:401`.
- **Espelho/pontas HE:** passa a lista a `pontas_do_relato`, que aceita apenas causas contendo “janela de HE”; pausa é ignorada. O resumo acima do teto aceita apenas “chegada fora da janela de HE”. Fontes: `app/ponto/services/espelho.py:741`, `app/ponto/janela_he.py:278`, `app/ponto/janela_he.py:281`, `app/ponto/services/espelho.py:1260`.
- **Diff janela:** exclui especificamente `l084_pausa`; não encontrei falha nova nesse filtro. Fonte: `app/ponto/management/commands/diff_janela_he.py:72`.
- **Fechamento/lavra de dinheiro:** o caminho por fatias transporta a lista composta; não encontrei persistência dela em `FechamentoMensal` nem consumo direto para rubricas. O próprio instrumento documenta que o fechamento não persiste a lista. Fontes: `app/ponto/services/fechamento.py:97`, `app/ponto/services/fechamento.py:294`, `app/ponto/management/commands/diff_reclassificar_partido.py:169`.
- **Calculador v3:** a captura de `_cadastro_x_realidade` em contexto de pontualidade vai para seu `Censo.cxr`, campo separado; não é segundo escritor do resultado público de produção. Fontes: `app/ponto/calculador/regras.py:275`, `app/ponto/calculador/periodo.py:283`.

`_l084_pausa_acusada`: escritor/leitor de dedupe no acusador, reinicialização por `calcular_mes`; não encontrei outro consumidor. Fontes: `app/ponto/motor_calculo_v2.py:1704`, `app/ponto/motor_calculo_v2.py:1707`, `app/ponto/motor_calculo_v2.py:1709`, `app/ponto/motor_calculo_v2.py:2327`.

Quanto a dia do turno versus civil, o novo laço passa `data_turno`; CXR e E6 conservam a chave recebida e não redatam a pausa pelo instante civil. Não encontrei conversão nova incorreta de unidade nesses leitores; a falha verificada é o descarte completo dos campos da pausa na C12. Fontes: `app/ponto/motor_calculo_v2.py:2781`, `app/ponto/services/cxr_na_lista.py:88`, `app/ponto/management/commands/e6_oraculo.py:170`.

## 08/11 09:25 · sessao c8d28fdc59f6 · FIO-E0-V4-E1 · verificador · origem=Code

**Parecer: REPROVA.** Há contraexemplo à igualdade entre o universo eleito pela porta e o universo de seus membros; além disso, o reuso admitido pelo juiz continua sujeito a leitores que tratam a data gravada como dia do turno. (`app/chamados/services/fio.py:1391`; `app/chamados/services/declaracao_texto.py:244`; `app/chamados/juizes.py:336`; `app/chamados/juizes.py:1988`.)

Os cenários abaixo são contraprovas construídas sobre o código. Sua ocorrência em produção, execução integrada dos testes e números SQL observados estão **[nao verificado]**. [nao verificado]

1. **Campos e memo — procedente, gravidade alta quanto ao significado de `data_evento`.**

   | Campo/valor | Quem escreve | Quem lê e qual significado usa |
   |---|---|---|
   | `data_evento` | `_nascer` grava o parâmetro; `abrir_conversa` fornece dia civil; `reapontar_resolvedora` data a existente; `rechavear_data_evento` troca a data. (`app/chamados/models.py:2707`; `app/chamados/models.py:2824`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:959`.) | Busca/índice usam a data gravada; o fecho passa a derivar a identidade pelo juiz. C6, validação de destino, plantio e expiração continuam lendo a gravada como dia relevante. (`app/chamados/models.py:2663`; `app/chamados/models.py:3298`; `app/chamados/services/fio.py:1235`; `app/chamados/juizes.py:336`; `app/chamados/juizes.py:1988`; `app/chamados/services/materializacao.py:344`; `app/chamados/veredito_lavrado.py:229`.) |
   | `batida_origem_id` | `_nascer` aceita explícita ou deriva do contexto; o `save` de inserção deriva quando ausente. (`app/chamados/models.py:2751`; `app/chamados/models.py:3254`.) | Identifica episódio no índice, busca, plano e agrupamento; `_dia_do_fato` carrega a Batida para perguntar seu dia ao juiz. É FK do episódio, não uma data. (`app/chamados/models.py:2632`; `app/chamados/models.py:3295`; `app/chamados/services/fio.py:1020`; `app/chamados/services/fio.py:1122`; `app/chamados/services/fio.py:1234`.) |
   | `instancia_marco` | `_chave_de_identidade` deriva do motivo e recusa explícita divergente; há backfill histórico. (`app/chamados/models.py:3243`; `app/chamados/models.py:3252`; `app/chamados/migrations/0051_decisao_pergunta.py:27`.) | Busca/índice/base do fecho usam posição do marco; `_dia_do_fato` usa seu preenchimento para reconhecer linha de marco. (`app/chamados/models.py:2663`; `app/chamados/models.py:3289`; `app/chamados/services/fio.py:1205`; `app/chamados/services/fio.py:1237`.) |
   | `celula_id` | Nascimento e reuso escolhem célula pela **data pedida**; rechaveamento escolhe pela nova data. (`app/chamados/models.py:2748`; `app/chamados/models.py:2782`; `app/chamados/models.py:2825`; `app/chamados/services/fio.py:956`; `app/chamados/services/fio.py:960`.) | C6 só aceita essa alimentação se `celula.data == data_evento`; outbox prefere a FK. `_dia_do_fato` não lê `p.celula.data`: passa `p.data_evento` ao conversor da célula. (`app/chamados/juizes.py:638`; `app/chamados/models.py:3047`; `app/chamados/services/fio.py:1239`.) |
   | `chamado_destino_id` | Nascimento/reuso em `_nascer`; reapontamento em `reapontar_resolvedora`. (`app/chamados/models.py:2825`; `app/chamados/models.py:2796`; `app/chamados/services/fio.py:868`.) | Âncora da listagem, validação da costura e julgamento do destino. O significado de FK de destino é consistente nesses caminhos. (`app/chamados/services/fio.py:117`; `app/chamados/models.py:2786`; `app/chamados/services/fio.py:856`; `app/chamados/services/fio.py:761`.) |

   **Não existe acordo global sobre `data_evento`.** O comentário novo declara chave gravada versus identidade derivada, mas `chave_do_dia` ainda se documenta como identidade, `_dia_de_turno_da_pergunta` devolve a gravada e o plantio combina a hora com ela. A declaração local não migrou esses leitores. (`app/chamados/services/fio.py:1219`; `app/chamados/services/fio.py:1198`; `app/chamados/juizes.py:332`; `app/chamados/services/materializacao.py:414`.)

   **Memo misto — não procedente a suspeita de colisão ou iteração incompatível.** Vizinhos usam chave de três elementos `(pk, chave_do_dia, bo)`; dia e colaborador usam chaves de dois elementos `('dia_do_turno', bo)` e `('colab', cid)`. Os consumidores acessam chaves específicas; não encontrei consumidor que percorra esse dicionário supondo valores exclusivamente de vizinhança. (`app/chamados/services/fio.py:1230`; `app/chamados/services/fio.py:1246`; `app/chamados/services/fio.py:1258`; `app/chamados/services/fio.py:1264`; `app/chamados/services/fio.py:1278`.)

2. **Propriedade fixa entre porta e membros — procedente, gravidade alta.**

   Contraexemplo: mesmo colaborador, motivo `gps_ausente`, nenhuma linha de acervo; alfa e beta são episódios distintos cujo juiz responde **18/10**. Cada episódio tem uma linha gravada em **10/09** e outra em **18/10**. Essa disposição cabe no índice: o horizonte é 21/09 e a unicidade do episódio só inclui linhas a partir dele. A premissa de duas batidas julgadas no mesmo dia está no teste novo. (`app/chamados/models.py:2313`; `app/chamados/models.py:3294`; `app/chamados/tests/test_e1_v13.py:59`.)

   | Consulta | Universo resultante |
   |---|---|
   | `universo_da_linha(A0)` ou `A1` | `{A0, A1}` |
   | `universo_da_linha(B0)` ou `B1` | `{B0, B1}` |
   | `universo_do_fato(colab, gps_ausente, 18/10, bo=None)` | `{A0, A1, B0, B1}` |

   A causa é concreta: sem episódio, `do_fato` traz as duas linhas gravadas em 18/10; ambas entram em `datadas`; o fecho preserva todas as sementes, sem exigir conectividade entre elas. Com episódio, a busca só liga o próprio episódio ao acervo sem episódio, inexistente neste cenário. Os resultados foram reproduzidos em memória com funções extraídas do código e ORM/juiz substituídos por stubs; integração e produção **[nao verificado]**. (`app/chamados/models.py:2630`; `app/chamados/models.py:2632`; `app/chamados/models.py:2636`; `app/chamados/services/fio.py:1320`; `app/chamados/services/fio.py:1381`; `app/chamados/services/fio.py:1391`.)

   **Os consumidores realmente divergem:** `_candidata_reusavel` chama sem episódio; `grupos_de_colisao` detecta as duas chaves de episódio e expande cada uma por fecho individual, produzindo dois grupos. O dedup consome esses grupos; a migração e `_colisao_ao_datar` começam pelo universo da linha. (`app/chamados/services/declaracao_texto.py:244`; `app/chamados/services/fio.py:1479`; `app/chamados/services/fio.py:1507`; `app/chamados/management/commands/dedup_perguntas_fato.py:58`; `app/chamados/services/fio.py:1634`; `app/chamados/services/fio.py:891`.)

   Há caminho de emissão sem episódio inferido: `gerar_perguntas_disputa` admite contexto sem `ultima_batida`, aplica seu dedup local e pode chegar a `abrir` com dia do contexto. Uma disputa nova não fica impedida pelas perguntas de outras disputas. A ocorrência desse cenário em produção está **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:325`; `app/chamados/services/disputa_emissao.py:327`; `app/chamados/services/disputa_emissao.py:372`; `app/chamados/services/disputa_emissao.py:444`.)

   **Assert de dois dias por chamador real — não demonstrado.** O agrupamento expande sementes individuais; `_colisao_ao_datar` começa por uma linha; a migração entrega ao plano membros do universo encontrado. Não obtive contraprova desse assert nesses caminhos. (`app/chamados/services/fio.py:1507`; `app/chamados/services/fio.py:891`; `app/chamados/services/fio.py:1634`; `app/chamados/services/fio.py:1641`.)

3. **Porta e célula — dois achados procedentes, gravidade alta.**

   **Nascimento sem linha anterior não verifica o dia da batida pedida.** Se o episódio ainda não possui perguntas, `universo_do_fato` não encontra linha para julgar. A recusa exige uma linha recusada daquele episódio; ausente essa linha, nasce pergunta com a data pedida e célula dessa data. Portanto `abrir(D0, batida cujo juiz diz DIA)` pode criar em D0; uma chamada direta posterior igual pode recusar a própria linha pelo juiz. (`app/chamados/services/fio.py:1362`; `app/chamados/services/fio.py:1376`; `app/chamados/models.py:2766`; `app/chamados/models.py:2748`; `app/chamados/models.py:2824`.)

   O caller de emissão não garante igualdade: grade/contexto têm precedência, e o juiz da última batida só é consultado quando falta a data anterior. Envolver `_dia_p` em `dia_do_turno_da_celula` não confere essa igualdade: o conversor devolve o valor recebido. (`app/chamados/services/disputa_emissao.py:371`; `app/chamados/services/disputa_emissao.py:378`; `app/chamados/services/disputa_emissao.py:424`; `app/chamados/services/disputa_emissao.py:451`; `app/ponto/turnos.py:2933`.)

   **Reuso correto pelo juiz pode falhar na costura.** Tome alfa gravada em 10/09, juiz em 18/10, como no teste novo; acrescente destino de escopo DIA declarado em 18/10. O proxy do pedido passa. A porta elege alfa e prepara célula de 18/10, mas conserva `ex.data_evento=10/09`. A segunda validação lê essa data antiga e lança `ResolvedoraCruzada`, pois a diferença supera um dia. O teste de reuso não fornece destino. (`app/chamados/tests/test_e1_v13.py:200`; `app/chamados/tests/test_e1_v13.py:203`; `app/chamados/models.py:2737`; `app/chamados/models.py:2782`; `app/chamados/models.py:2786`; `app/chamados/juizes.py:1988`; `app/chamados/juizes.py:2016`; `app/chamados/juizes.py:2023`.)

   Sem essa recusa, o estado persistido pode ficar com célula DIA e data gravada D0; C6 descarta a célula como alimentação e continua julgando pelo dia gravado. Logo atribuir a célula certa não reconcilia os leitores. (`app/chamados/models.py:2820`; `app/chamados/juizes.py:638`; `app/chamados/juizes.py:671`; `app/chamados/juizes.py:679`.)

   **Correção do inventário de callers:** os sítios de produção de `abrir` não passam `batida_origem_id` explicitamente; `_nascer` a infere. `reapontar_resolvedora` e `_colisao_ao_datar` não chamam `abrir`: usam a porta de existente. (`app/chamados/services/disputa_emissao.py:444`; `app/chamados/services/disputa_emissao.py:834`; `app/chamados/services/declaracao_texto.py:348`; `app/chamados/services/declaracao_texto.py:636`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:849`; `app/chamados/services/fio.py:868`.)

   **Não procede acusar automaticamente o agrupador de passar dia civil:** seu `localtime(...).date()` alimenta a classificação; a criação recebe `resolver_data_evento(...batida_origem=...)`, que consulta o juiz. (`app/chamados/services/disputa_emissao.py:1108`; `app/chamados/services/disputa_emissao.py:1114`; `app/chamados/services/disputa_emissao.py:1123`; `app/escala/servico_jornada.py:488`.)

4. **Juiz, derivação e relógio — não procedente como clonagem nova; inconsistência de leitores permanece.**

   `_dia_do_fato` delega a batida ao recorte do juiz registrado: `dia_do_turno_da_batida → dia_das_batidas → turnos_do_colab`. Não reproduz pareamento nem deriva dia do timestamp por conta própria. A ponte é a implementação expressamente descrita para linhas sem dia próprio. (`app/core/juizes.py:196`; `app/chamados/services/fio.py:1235`; `app/ponto/turnos.py:2925`; `app/ponto/turnos.py:2903`; `app/chamados/services/fio.py:1227`.)

   Os acréscimos de identidade/memo/contador não introduzem leitura direta de relógio. A catraca de calendário cobre `.date()` sobre timestamp e lookups `timestamp__date`; isso não detecta o conflito semântico entre o novo juiz e leitores antigos de `data_evento`. A correspondência histórica dessa catraca com `935c8d96` está **[nao verificado]**. (`app/chamados/services/fio.py:1229`; `app/chamados/services/fio.py:1268`; `app/chamados/services/fio.py:1410`; `app/ponto/tests/test_contract_no_batida_date.py:175`; `app/ponto/tests/test_contract_no_batida_date.py:203`; `app/chamados/juizes.py:336`.)

   Para marco, o código declara `p.data_evento` como dia de célula sem consultar a FK. Erro concreto adicional dependeria de marco com data gravada divergente da célula soberana; presença desse caso está **[nao verificado]**. (`app/chamados/services/fio.py:1237`; `app/chamados/services/fio.py:1239`.)

5. **Custo — procedente como amplificação, gravidade média; estouro de teto [nao verificado].**

   Considere **B** batidas existentes distintas examinadas, incluindo as alcançadas na ponte e recusadas:

   | Operação | Chamadas ao juiz pesado |
   |---|---|
   | Um fecho com memo novo | Uma por batida distinta examinada. |
   | Um `abrir` | Zero com busca vazia; caso contrário, B no universo/ponte/fecho compartilhado. |
   | Recusa `FatoEmOutroDia` | Mais uma para o episódio, pois o rótulo abre outro memo. |
   | `fecho_cruza_dias` | Soma dos B de cada fecho executado; não compartilha memo entre componentes. |

   Essas contagens decorrem das chaves por batida, do compartilhamento local e da criação de `_memo_t` separado. Batida inexistente custa a busca, mas não chama o juiz. (`app/chamados/services/fio.py:1230`; `app/chamados/services/fio.py:1234`; `app/chamados/services/fio.py:1339`; `app/chamados/services/fio.py:1392`; `app/chamados/models.py:2772`; `app/chamados/services/fio.py:1428`.)

   **Memo por batida não basta para compartilhar trabalho por turno/janela:** cada batida distinta reconstrói `dia_das_batidas(d−1,d+1)`, carregando turnos e batidas novamente, mesmo com janelas sobrepostas. (`app/ponto/turnos.py:2925`; `app/ponto/turnos.py:2903`; `app/ponto/turnos.py:2905`; `app/ponto/turnos.py:1863`; `app/ponto/turnos.py:1884`.)

   Os selos citados têm coberturas diferentes: chokepoint impõe tetos SQL; modal monta 330 perguntas sem episódio; `test_porta_chamado_e1` mede atos/no-ops de Chamado; cadeia/ordem verificam transação e precedência de travas, sem impor teto de consultas. Não são prova conjunta do novo custo da porta por episódio. (`app/ponto/tests/test_selo_chokepoint_escrita.py:397`; `app/chamados/tests/test_selo_teto_modal.py:68`; `app/chamados/tests/test_porta_chamado_e1.py:57`; `app/chamados/tests/test_e1_v13_cadeia.py:32`; `app/chamados/tests/test_e1_v13_ordem.py:59`.)

6. **Candidatas de `fecho_cruza_dias` — não procedente a falta de cobertura, sob as invariantes de nascimento.**

   Todo passo entre linhas distintas ocorre por chave gravada ou por episódio/motivo. O contador seleciona chave com múltiplas linhas e episódio com múltiplas chaves; assim cobre ambas as formas de aresta. Colaborador e instância são normalizados no nascimento. (`app/chamados/models.py:2630`; `app/chamados/models.py:2632`; `app/chamados/models.py:2636`; `app/chamados/services/fio.py:1416`; `app/chamados/services/fio.py:1421`; `app/chamados/models.py:3236`; `app/chamados/models.py:3243`.)

   Dois episódios distintos na mesma data, sem acervo, são candidatas excedentes, mas seus fechos não recusam vizinhos e não entram no resultado. A confirmação usa o fecho real. (`app/chamados/models.py:2632`; `app/chamados/services/fio.py:1428`; `app/chamados/services/fio.py:1431`.)

   O contador, porém, acrescenta membros quando existe **qualquer** recusa; não verifica novamente se sua causa foi outro dia, outra base ou ponte ambígua. Garantia irrestrita para acervo que viole as invariantes, ou ocorrência de falso positivo nesse acervo: **[nao verificado]**. (`app/chamados/services/fio.py:1340`; `app/chamados/services/fio.py:1431`.)

7. **Classes de reprovação e selos.**

   - **Segundo escritor fora da porta — não procedente no censo AST de `data_evento`.** Executei em memória o scanner existente: retornou somente `_nascer`, `reapontar_resolvedora` e `rechavear_data_evento`, sem sítio fora das portas. Isso verifica localização da escrita, não correção do significado. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:30`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:122`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:143`.)
   - **Ordem de trava — nenhuma inversão nova demonstrada.** `_nascer` trava Colaborador antes do universo; as leituras novas de Batida/Colaborador não pedem `FOR UPDATE`. Execução dos selos de ordem e concorrência: **[nao verificado]**. (`app/chamados/models.py:2724`; `app/chamados/models.py:2764`; `app/chamados/services/fio.py:1234`; `app/chamados/services/fio.py:1252`.)
   - **RED v13 sem mordida — não procedente contra a variante simples por data gravada.** O teste exige B0, gravada D0, dentro do fato DIA; voltar a decidir pela gravada a exclui. Também exige reuso de A0 em outra data e recusa de beta pedido em D0. (`app/chamados/tests/test_e1_v13.py:108`; `app/chamados/tests/test_e1_v13.py:121`; `app/chamados/tests/test_e1_v13.py:157`; `app/chamados/tests/test_e1_v13.py:198`.)
   - **Lacunas dos novos selos — procedente, gravidade média.** O cenário de união de episódios usa acervo como ponte; o teste de reuso omite destino. Não cobrem as contraprovas dos itens 2 e 3. (`app/chamados/tests/test_e1_v13.py:78`; `app/chamados/tests/test_e1_v13.py:203`.)
   - **Preservação byte a byte dos v5/v8/v10/v11, RED executado, mutante executado e passagem dos tetos:** **[nao verificado]**.
   - **Campo com dois significados não reconciliados — procedente, gravidade alta:** a identidade nova usa o juiz, enquanto destino, C6 e plantio continuam usando a data gravada como dia do fato. (`app/chamados/services/fio.py:1235`; `app/chamados/juizes.py:1988`; `app/chamados/juizes.py:336`; `app/chamados/services/materializacao.py:414`.)

## 08/11 09:21 · sessao 7f8fd01fd110 · - · - · origem=Ronald (tmux)

Procedente, alta: a porta ainda une componentes desconectados quando chamada sem episódio. Para `gps_ausente`, use A0/A1 do episódio alfa, gravadas respectivamente em 10/09 e 18/10, B0/B1 do episódio beta nas mesmas datas, ambos julgados como 18/10, sem linha de acervo. Por linha, os universos são `{A0,A1}` e `{B0,B1}`; `universo_do_fato(...,18/10,batida_origem_id=None)` retorna as quatro. A seleção sem episódio traz ambas as linhas de 18/10; `datadas` conserva ambas e `fecho_do_fato` retém todas as sementes independentemente da conectividade. `app/chamados/models.py:2636`, `app/chamados/models.py:2663`, `app/chamados/services/fio.py:1320`, `app/chamados/services/fio.py:1381`, `app/chamados/services/fio.py:1391`.

O chamador real `_candidata_reusavel` omite episódio. O agrupamento real detecta duas chaves de episódio e chama o universo de cada uma separadamente, produzindo dois grupos; portanto porta e backfill/dedup não compartilham o universo anunciado. `app/chamados/services/declaracao_texto.py:244`, `app/chamados/services/fio.py:1122`, `app/chamados/services/fio.py:1479`, `app/chamados/services/fio.py:1501`, `app/chamados/services/fio.py:1507`.

O cenário admite índice ativo: o horizonte é 21/09 e a unicidade de episódio só inclui linhas gravadas a partir dele; cada episódio possui uma linha dentro e outra fora. A premissa de dois episódios no mesmo dia do juiz existe no teste novo. `app/chamados/models.py:2313`, `app/chamados/models.py:3294`, `app/chamados/tests/test_e1_v13.py:59`.

Confirmei os resultados executando somente em memória as funções reais extraídas por AST, com ORM e juiz substituídos por stubs explícitos; execução integrada e presença desse cenário em produção são [nao verificado].

Não procedente para linhas conformes: o universo candidato do contador cobre toda aresta possível do `do_fato`: vizinhança pela chave gravada ou pelo mesmo episódio/motivo. Candidatas excedentes são confirmadas pelo fecho real, evitando contar dois episódios na mesma data sem acervo. `app/chamados/models.py:2630`, `app/chamados/models.py:2632`, `app/chamados/models.py:2636`, `app/chamados/services/fio.py:1415`, `app/chamados/services/fio.py:1421`, `app/chamados/services/fio.py:1428`.

Não encontrei sementes de dois dias vindas dos consumidores internos com grupos normalmente produzidos: o agrupamento parte pelos fechos individuais; `_colisao_ao_datar` começa por uma linha; a migração entrega ao plano membros do universo dessa linha. Outros estados ou concorrência que produzam o assert são [nao verificado]. `app/chamados/services/fio.py:891`, `app/chamados/services/fio.py:1504`, `app/chamados/services/fio.py:1634`, `app/chamados/services/fio.py:1641`.

## 08/11 09:20 · sessao fb39c0000d12 · - · - · origem=Ronald (tmux)

Inventário por campo e achados:

| Campo | Escritores | Leitores / significado |
|---|---|---|
| `data_evento` | Nascimento datado e conversa: `app/chamados/models.py:2696`, `:2707`, `:2824`. Datar existente: `app/chamados/services/fio.py:838`, `:840`, `:870`. Rechaveamento: `app/chamados/services/fio.py:959`, `:965`. Proxy e restauração apenas em memória: `app/chamados/models.py:2737`; `app/chamados/services/fio.py:852`, `:861`. | Chave gravada de busca/índice: `app/chamados/models.py:2630`, `:2663`, `:3289`; `app/chamados/services/fio.py:1193`, `:1205`. Identidade agora derivada pelo juiz: `app/chamados/services/fio.py:1229`, `:1235`, `:1330`, `:1375`. Outros leitores ainda a tratam como dia do turno: `app/chamados/juizes.py:332`, `:336`, `:1988`; `app/chamados/services/materializacao.py:344`, `:414`; `app/chamados/services/pergunta_no_app.py:93`; `app/chamados/services/validacao.py:269`, `:446`; janela: `app/chamados/veredito_lavrado.py:229`; apresentação/divergência: `app/chamados/services/fio.py:87`. |
| `batida_origem_id` | `_nascer` aceita valor explícito ou deriva: `app/chamados/models.py:2751`, `:2753`. `save` de inserção deriva se ausente: `app/chamados/models.py:3226`, `:3254`. Derivador lê foto do nascimento no contexto e valida dono: `app/chamados/models.py:3269`, `:3273`. | Identifica episódio: `app/chamados/models.py:2632`, `:3295`; `app/chamados/services/fio.py:1020`, `:1122`. Alimenta juiz do dia: `app/chamados/services/fio.py:1229`, `:1234`. Forma chave do cache: `app/chamados/services/fio.py:1258`; candidatos do contador: `app/chamados/services/fio.py:1413`, `:1418`. Não representa data nem instante: é FK da batida cujo instante o juiz consulta (`app/chamados/models.py:2350`; `app/chamados/services/fio.py:1234`). |
| `instancia_marco` | Derivação/validação no nascimento: `app/chamados/models.py:3243`, `:3246`, `:3252`. Backfill histórico: `app/chamados/migrations/0051_decisao_pergunta.py:27`. | Posição do marco, derivada do motivo: `app/chamados/models.py:3234`. Chave de busca/índice: `app/chamados/models.py:2663`, `:3289`; `app/chamados/services/fio.py:1205`. Discriminador da linha de marco em `_dia_do_fato`: `app/chamados/services/fio.py:1237`. |
| `celula_id` | Nascimento pela data pedida: `app/chamados/models.py:2748`, `:2825`. Reuso pela data pedida: `app/chamados/models.py:2781`, `:2782`, `:2820`. Rechaveamento pela nova data: `app/chamados/services/fio.py:956`, `:960`, `:965`. | FK da célula da pergunta: `app/chamados/models.py:2323`. C6 verifica se célula corresponde ao `data_evento` antes de alimentar julgamento: `app/chamados/juizes.py:634`, `:638`. Outbox prefere FK e só usa `(colaborador,data_evento)` como fallback: `app/chamados/models.py:3034`, `:3047`, `:3050`. `_dia_do_fato` NÃO lê `p.celula.data`; passa a data gravada à conversão de célula (`app/chamados/services/fio.py:1237`, `:1239`). |
| `chamado_destino_id` | Nascimento: `app/chamados/models.py:2825`. Reuso na porta: `app/chamados/models.py:2796`, `:2820`. Porta de reapontamento: `app/chamados/services/fio.py:868`, `:870`. Simulações sem persistência: `app/chamados/models.py:367`; `app/chamados/services/fio.py:779`, `:783`. | Âncora da listagem, com fallback ao chamado da disputa: `app/chamados/services/fio.py:117`. Gate do destino: `app/chamados/models.py:2784`, `:2786`; `app/chamados/services/fio.py:856`. C6/encerramento/outbox: `app/chamados/models.py:310`, `:361`, `:3028`. Mesmo significado de FK de destino nesses leitores (`app/chamados/models.py:3191`, `:3197`; `app/chamados/services/fio.py:100`). |

**Procedente, alta: a identidade aceita pelo fecho continua podendo ser recusada pelo leitor do destino.** Cenário derivado da própria fixture: episódio alfa gravado `D0=2026-09-10`, juiz retorna `DIA=2026-10-18`; `abrir(...DIA, chamado_destino=chDIA)` encontra e elege alfa, mas conserva `ex.data_evento=D0` (`app/chamados/tests/test_e1_v13.py:200`; `app/chamados/tests/test_e1_v9.py:37`; `app/chamados/models.py:2777`, `:2782`, `:2820`). O proxy do pedido passa, depois o gate de `ex` compara a data gravada com o destino e lança `ResolvedoraCruzada` para esse intervalo superior a um dia (`app/chamados/models.py:2737`, `:2741`, `:2786`, `:2788`; `app/chamados/juizes.py:1988`, `:2007`, `:2016`, `:2023`). O teste novo de reaproveitamento não fornece destino (`app/chamados/tests/test_e1_v13.py:203`).

**Procedente, alta: a porta não valida o dia do episódio no nascimento sem linha preexistente.** A recusa requer universo vazio **e** uma recusada do mesmo episódio; sem linhas, chega ao `create` com a data pedida e célula dessa data (`app/chamados/models.py:2748`, `:2766`, `:2824`). Assim, um pedido `D0` para batida cujo juiz diz `DIA` pode nascer `D0`; a emissão seguinte pode recusar a própria linha pelo juiz (`app/chamados/services/fio.py:1375`, `:1380`; `app/chamados/models.py:2766`). Ocorrência em produção: [nao verificado].

**Callers concretos de `abrir`.** Há quatro sítios de produção; nenhum passa explicitamente `batida_origem_id`, que a porta infere do contexto (`app/chamados/services/disputa_emissao.py:444`, `:834`; `app/chamados/services/declaracao_texto.py:348`, `:636`; `app/chamados/models.py:2751`).

- `gerar_perguntas_disputa`: grade/contexto têm precedência; só consulta juiz da última batida se o fallback estiver vazio; o destino ainda pode fornecer `_data_ev` prioritária (`app/chamados/services/disputa_emissao.py:350`, `:371`, `:378`, `:391`, `:424`, `:451`). Contraexemplo de chamada possível: contexto de `gps_ausente` com `data=D0` e `ultima_batida=b1` de `DIA`; nasce com a divergência acima. O merge preserva chaves antigas ausentes no novo valor (`app/chamados/services/disputa_emissao.py:555`, `:556`). Existência desse contexto em produção: [nao verificado].
- Agrupador: o `localtime(...).date()` serve à classificação, mas o dia passado à criação é o retorno de `resolver_data_evento(...batida_origem=...)`; portanto, não procede afirmar que esse caller passa automaticamente dia civil (`app/chamados/services/disputa_emissao.py:1108`, `:1109`, `:1114`, `:1123`; `app/escala/servico_jornada.py:488`).
- Declaração: ambos passam dia da célula ao conversor registrado (`app/chamados/services/declaracao_texto.py:347`, `:349`, `:635`, `:637`).
- `reapontar_resolvedora` e `_colisao_ao_datar` **não chamam `abrir`**: usam a porta de existente, `fato_livre_para`, `universo_da_linha` e plano; a escrita ocorre no save de reapontamento (`app/chamados/services/fio.py:849`, `:850`, `:891`, `:904`, `:868`, `:870`).

**Procedente, média/alta: dois significados não foram reconciliados em todos os leitores.** `_dia_de_turno_da_pergunta` continua retornando `data_evento` cru para perguntas de fato e é consumido pelo julgamento do C6 e tranca (`app/chamados/juizes.py:332`, `:336`, `:375`, `:671`, `:763`, `:948`, `:1216`). A célula nova `DIA` também deixa de servir à dieta do julgamento se a data gravada permanecer `D0` (`app/chamados/models.py:2782`; `app/chamados/juizes.py:638`). A declaração no comentário novo de `_dia_do_fato` não altera esses leitores (`app/chamados/services/fio.py:1218`; `app/chamados/juizes.py:336`, `:1988`).

**Não demonstrado como erro autônomo: marco usar data gravada.** `_dia_do_fato` não lê a FK da célula; passa `p.data_evento` ao juiz da célula (`app/chamados/services/fio.py:1237`, `:1239`). Isso só produz dia errado se uma linha de marco tiver data gravada divergente do dia soberano pretendido; presença desse caso no dado dado: [nao verificado].

## 08/11 09:19 · sessao cc2c08306ed4 · - · - · origem=Ronald (tmux)

Achados de custo/selos, somente por inspeção:

1. **Procedente, média: memo por batida evita repetição dentro de um fecho, mas não evita recomputar a janela inteira para cada batida.** Cada `bo` novo faz `SELECT Batida` e chama o juiz; este executa `dia_das_batidas(d-1,d+1)`, que carrega turnos e batidas da janela. Duas batidas do mesmo turno pagam duas reconstruções sobre janelas sobrepostas. (`app/chamados/services/fio.py:1229-1236`; `app/ponto/turnos.py:2917-2926`; `app/ponto/turnos.py:2901-2907`; `app/ponto/turnos.py:1861-1888`.)

2. **Contagem de chamadas ao juiz:**
   - Por `fecho_do_fato`: uma por `batida_origem_id` distinta examinada nas sementes, vizinhos e explorações da ponte; inclui vizinhos recusados, pois `_dia_do_fato(q)` precede o filtro de base. (`app/chamados/services/fio.py:1326`; `app/chamados/services/fio.py:1339-1342`; `app/chamados/services/fio.py:1278-1286`.)
   - Por `abrir`: zero se `do_fato` vier vazio; caso contrário, uma por batida distinta examinada pelo universo/ponte/fecho, com o mesmo memo. A batida **pedida**, quando ainda não tem linha, não é julgada por esse caminho. (`app/chamados/services/fio.py:1362-1392`; `app/chamados/models.py:2751-2765`.)
   - Na exceção `FatoEmOutroDia`: **mais uma chamada** para o episódio recusado, pois o rótulo abre `_memo_t = {}` em vez de aproveitar o memo anterior. (`app/chamados/models.py:2771-2776`.)
   - Por `fecho_cruza_dias`: soma das batidas distintas de **cada fecho executado**, sem memo global. O contador chama `fecho_do_fato([p])` sem `_viz`; uma batida alcançada como vizinha recusada pode ser julgada de novo no componente seguinte. (`app/chamados/services/fio.py:1423-1432`; `app/chamados/services/fio.py:1317`.)
   - Número SQL efetivamente observado, tempo e estouro dos tetos: **[nao verificado]**.

3. **Não procedente: os selos citados não são todos selos de custo deste caminho.**
   - Chokepoint efetivamente compara consultas com teto, inclusive ramos FICA de 144/217. (`app/ponto/tests/test_selo_chokepoint_escrita.py:180`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`; `app/ponto/tests/test_selo_chokepoint_escrita.py:239-250`; `app/ponto/tests/test_selo_chokepoint_escrita.py:397-430`.)
   - Modal mede 330 perguntas, mas a fixture não fornece `batida_origem`; ela não representa o novo custo por episódio. (`app/chamados/tests/test_selo_teto_modal.py:68-79`; `app/chamados/tests/test_selo_teto_modal.py:81-97`.)
   - `test_porta_chamado_e1` mede no-op de atos de campos de **Chamado**, não `PerguntaDisputa.abrir`. (`app/chamados/tests/test_porta_chamado_e1.py:1-5`; `app/chamados/tests/test_porta_chamado_e1.py:57-62`; `app/chamados/tests/test_porta_chamado_e1.py:117-122`.)
   - `test_e1_v13_cadeia` captura SQL para detectar escrita sem trava na transação; `_ordem` verifica precedência de trava/escrita e mutantes do registro. Nenhum desses impõe teto de consultas. (`app/chamados/tests/test_e1_v13_cadeia.py:32-59`; `app/chamados/tests/test_e1_v13_ordem.py:59-82`.)

4. **Não procedente: RED v13 sem mordida contra a comparação por data gravada.** A fixture coloca B0 com `data_evento=D0` e batida beta; o teste exige B0 dentro do fato DIA. A variante simples que volta a decidir por `p.data_evento` necessariamente exclui B0 e falha esse assert. Também há reutilização de A0 gravada em outra data e recusa de beta pedido em D0. (`app/chamados/tests/test_e1_v13.py:108-124`; `app/chamados/tests/test_e1_v13.py:157-165`; `app/chamados/tests/test_e1_v13.py:198-208`.)

5. **Lacuna, média: execução explícita do mutante exigido pelo aval [nao verificado].** O arquivo possui testes semânticos que distinguem a variante, mas termina sem teste que instale o mutante `data_evento`; a execução RED/mutante não veio no dado. (`app/chamados/tests/test_e1_v13.py:116-124`; `app/chamados/tests/test_e1_v13.py:315-320`; aval anexo **[nao verificado quanto à execução]**.)

6. **Ordem de trava: nenhuma nova aquisição explícita no diff inspecionado.** `_nascer` continua travando o colaborador antes de chamar o universo; os acréscimos usam leituras comuns para Batida/Colaborador. O runtime completo e eventual regressão nos selos de ordem são **[nao verificado]**. (`app/chamados/models.py:2720-2724`; `app/chamados/models.py:2762-2765`; `app/chamados/services/fio.py:1234`; `app/chamados/services/fio.py:1248-1252`.)

7. **Preservação dos selos antigos:** o diff fornecido só declara mudanças em `models.py`, `fio.py` e `test_e1_v13.py`; não exibe alteração dos v5/v8/v10/v11. Identidade byte a byte com os commits e execução desses selos: **[nao verificado]**. [nao verificado]

## 08/11 04:49 · sessao 7108433fc78b · CXR-TELA-LE-O-MOTOR · verificador · origem=Code

**A v3 corrige a vacuidade anterior do C11 e unifica o instante usado pelo command e pelo serviço. A prova continua parcial: falta o estado “coberto, com lista vazia”, e o teste de relógio substitui justamente o espelho que precisaria verificar em profundidade.** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:391`, `:417`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:30`; `app/ponto/services/cxr_na_lista.py:183`)

Suite Django completa e execução dos mutantes nessa suite: **[nao verificado]**. As contraprovas abaixo distinguem leitura estática de ensaio isolado em memória. [nao verificado]

**1) C11 chega ao dono; o mutante anterior agora é atingido.**

O caminho está conectado: `lista()` chama `_folga_nao_definida_sem_porta()`, esta chama `beco_em_lote()`, que constrói `InsumosDoDono` com `sem_motor=True, cxr=...` e chama `.dono(d)`. (`app/escala/services/cadastro_realidade.py:864`, `:573`, `:525`, `:531`)

A fixture verifica pergunta viva e ausência de porta; o teste exige exatamente o C11 daquele dia com dono `CADASTRO`, além de zero chamadas ao espelho e a `MotorBase.calcular_mes`. Uma exceção engolida por `lista()` retiraria o C11 esperado e falharia na asserção; o selo deixou de passar com o beco vazio. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:367`, `:371`, `:391`, `:393`; `app/escala/services/cadastro_realidade.py:866`)

Há uma distinção entre os dois casos:

- **Com lavra:** `_cxr` já contém o dia; `dono()` chega a `cxr()`, que devolve o conjunto carregado. (`app/ponto/management/commands/e6_oraculo.py:147`, `:163`, `:195`)
- **Sem lavra:** o juiz substituto devolve `ESTRUTURA`; `dono()` encontra `sem_motor=True` e devolve `None` antes de chamar `cxr()`. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:374`, `:395`; `app/ponto/management/commands/e6_oraculo.py:189`)

Portanto, remover os dois argumentos de `beco_em_lote:526` agora leva ao espelho sob o juiz forçado, contrariando as asserções de conteúdo e chamadas. É conclusão pelo fluxo; execução desse mutante no runner completo: **[nao verificado]**. (`app/escala/services/cadastro_realidade.py:526`; `app/ponto/management/commands/e6_oraculo.py:168`, `:190`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:391`)

**Existe mutação plausível sobrevivente aos dois cenários novos.** Antes da lógica atual de `InsumosDoDono.dono()`, introduzir:

```python
if self._sem_motor and self._cxr == set():
    self._cxr = None
    self._sem_motor = False
```

Isso introduz um fallback para uma lavra válida que respondeu “nenhum dia”. Os testes novos fornecem conjunto não vazio ou `None`; nenhum fornece conjunto vazio. Esse terceiro estado é produzido legitimamente por `cxr_lavrado()`. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:386`, `:397`; `app/ponto/services/cxr_na_lista.py:183`)

No ensaio isolado em memória dos métodos atuais e dessa alteração, os dois cenários novos mantiveram `CADASTRO/0 chamadas` e `None/0 chamadas`; o cenário coberto vazio, com juiz real respondendo `ESTRUTURA`, passou de zero para **uma chamada ao espelho**. Isso demonstra a lacuna dos cenários; aprovação do mutante pela classe Django completa: **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:162`, `:182`)

Também falta uma asserção direta de passagem pela autoridade: substituir a expressão `ins[...].dono(d)` por uma decisão local baseada na presença do dia na lavra reproduziria as duas respostas esperadas. Seria um segundo juiz; os dois resultados finais, sozinhos, não distinguem essa implementação. (`app/escala/services/cadastro_realidade.py:531`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:391`, `:398`)

**2) A fixture serve ao contrato do leitor, mas não representa uma lavra produzida integralmente naquele estado.**

O colaborador do beco é criado separadamente; as batidas da fixture base pertencem aos outros dois colaboradores. O teste injeta cobertura e item para o beco chamando `lavrar()` diretamente. Já `medir()` só percorre colaboradores encontrados em `Batida` dentro da competência. Assim, **esse snapshot não sairia de `medir()` com esse estado da fixture**. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:70`, `:77`, `:342`, `:386`; `app/ponto/services/cxr_na_lista.py:68`, `:71`)

A estrutura necessária ao leitor está correta: `cobertos` contém colaborador, competência e início; os itens contêm colaborador, competência e data. `cxr_lavrado()` usa precisamente esses campos. A escrita passa pela função canônica, embora o conteúdo seja montado pelo teste. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:386`; `app/ponto/services/cxr_na_lista.py:142`, `:176`, `:180`)

Há ainda uma artificialidade deliberada no juiz: o C11 exige `folga_nao_definida`, e o juiz real já responde `CADASTRO` por esse insumo, independentemente da lista do motor. O mock ignora isso para forçar a situação que exigiria consultar CXR. Logo, o teste sem lavra **não demonstra que um beco real dessa fixture receberia `None`**; demonstra o comportamento quando a lista seria necessária à decisão. (`app/escala/services/cadastro_realidade.py:518`; `app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:374`)

Isso dá mordida ao teste de proteção contra motor, mas limita seu alcance como prova de integração produtor → lavra → leitor. A correspondência com algum estado real de produção permanece **[nao verificado]**. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:384`)

**3) Relógio: datas de referência alinhadas; carimbo de persistência separado.**

| Trecho | Comportamento |
|---|---|
| Command | Lê `timezone.now()`, deriva `hoje` desse instante, passa `agora` a `medir` e `hoje` a `lavrar`. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:30`, `:40`) |
| `medir` | Deriva `hoje` de `agora`; usa esse dia nas competências e passa os mesmos valores ao espelho e a `InsumosDoDono`. (`app/ponto/services/cxr_na_lista.py:60`, `:63`, `:78`, `:99`) |
| Janelas | `janela_atual/anterior` calculam a partir da data recebida e do corte da empresa; não consultam relógio. (`app/ponto/janelas.py:60`, `:66`) |
| Espelho | Recebe `agora`, passa-o a `montar_dias`; a autoridade do cálculo recebe datas. A chamada a `calcular_mes` não recebe parâmetro `agora`. (`app/ponto/services/espelho.py:1227`, `:1262`; `app/ponto/services/fechamento.py:286`) |
| `InsumosDoDono` | Guarda o instante recebido; uma eventual consulta ao espelho o reutiliza. Na lavra, recebe também o espelho já calculado. (`app/ponto/management/commands/e6_oraculo.py:132`, `:169`; `app/ponto/services/cxr_na_lista.py:99`) |
| Snapshot | `calculado_em` tem `auto_now=True`; `lavrar` não o preenche com o instante da borda. (`app/inteligencia/models.py:96`; `app/ponto/services/cxr_na_lista.py:145`) |

**Sim:** no command atual, `data_ref`, o dia usado nas competências e o `agora` entregue ao espelho derivam do mesmo instante. **A expressão “agora do motor” é imprecisa:** o cálculo recebe datas; o instante explícito segue pelo espelho para a montagem da grade. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:30`; `app/ponto/services/cxr_na_lista.py:60`; `app/ponto/services/espelho.py:1266`; `app/escala/utils.py:2358`)

**Não há garantia de uma única leitura literal durante toda a persistência:** `calculado_em` é um carimbo automático de salvamento. Além disso, `ultimo_lavrado()` o publica como `medido_em`; esse campo não preserva o instante inicial passado ao espelho. (`app/inteligencia/models.py:96`; `app/ponto/services/cxr_na_lista.py:160`)

O teste de meia-noite tem mordida contra o defeito anterior: exige que todos os argumentos `agora` observados sejam exatamente `t1`, e compara seus dias com `data_ref`. Uma leitura extra depois da primeira não apaga essa exigência. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:412`, `:425`, `:427`)

Mas sua cobertura termina na entrada do espelho: ele substitui a função inteira e devolve lista vazia, impedindo também a criação de `InsumosDoDono` pelo serviço. Uma regressão dentro do espelho ficaria oculta desse teste; a catraca AST examina apenas o command e `cxr_na_lista`. Também não há asserção sobre `ini/fim`, e a virada escolhida é 05→06, sem mudança de competência. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:417`, `:421`, `:447`; `app/ponto/services/cxr_na_lista.py:81`, `:98`)

**4) Quem escreve e quem lê.**

| Elemento | Escritor/origem | Leitores/destino |
|---|---|---|
| `medir(agora=)` | O command captura e fornece o instante. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:30`) | O serviço deriva `hoje` e alimenta espelho e insumos. (`app/ponto/services/cxr_na_lista.py:60`, `:78`, `:99`) |
| `lavrar(d, hoje)` | Recebe a medida e escreve `data_ini`, `data_ref`, contadores e `detalhe` via `update_or_create`. (`app/ponto/services/cxr_na_lista.py:142`) | `ultimo_lavrado()` recupera o snapshot; C12 formata seus itens; `cxr_lavrado()` fornece dias aos leitores do dono. (`app/ponto/services/cxr_na_lista.py:152`, `:174`; `app/escala/services/cadastro_realidade.py:393`, `:523`, `:1021`) |
| `cobertos` | `medir()` acrescenta a chave somente depois de concluir a leitura do colaborador/competência; falhas saem antes. (`app/ponto/services/cxr_na_lista.py:111`, `:118`) | `cxr_lavrado()` distingue ausência de cobertura (`None`) de cobertura sem dias (`set()`). (`app/ponto/services/cxr_na_lista.py:176`, `:183`) |
| `CLASSIFICAR` | Registro estático no teste; a entrada nova pertence a **`B2_menos_B1`**. (`app/core/tests/test_medida_turno_do_par.py:391`, `:452`) | O teste compara esse registro ao censo calculado pelo instrumento. Não é allowlist de execução. (`app/core/tests/test_medida_turno_do_par.py:577`; `app/core/medida_turno_do_par.py:436`) |

No censo textual da cópia, o chamador encontrado de `cxr_na_lista.medir` é o command já atualizado; não encontrei chamada desse serviço com `hoje=`. O `medir(hoje=...)` de `lavrar_teto_do_dia` pertence a outro serviço. Chamadas externas à cópia: **[nao verificado]**. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:17`, `:32`; `app/ponto/management/commands/lavrar_teto_do_dia.py:28`)

**5) Classes de reprovação.**

| Classe | Parecer sobre a cópia |
|---|---|
| Segundo juiz | O código atual continua chamando a autoridade existente. O mock limita a capacidade dos novos testes de detectar sua substituição por decisão local. (`app/escala/services/cadastro_realidade.py:531`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:374`) |
| Segundo escritor fora da porta | O command e o teste novo usam `cxr_na_lista.lavrar`. A escrita direta da célula é preparação de fixture. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:40`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:366`, `:386`) |
| Relógio fora da borda | A dupla leitura command/serviço foi removida. O carimbo automático permanece; os testes não auditam dependências transitivas. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:30`; `app/inteligencia/models.py:96`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:447`) |
| Derivação paralela | A conversão de instante em data usa a mesma entrada; a inclusão em `CLASSIFICAR` é informativa, não cria autoridade. (`app/ponto/services/cxr_na_lista.py:60`; `app/core/tests/test_medida_turno_do_par.py:12`) |
| Selo sem mordida | A vacuidade anterior foi fechada. Continua faltando o cenário de cobertura vazia no request e a verificação do espelho real pelo selo temporal. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:391`, `:417`; `app/ponto/services/cxr_na_lista.py:184`) |
| Campo com dois significados | `None` e conjunto vazio estão separados corretamente no código. `medido_em` representa `calculado_em`, não o instante inicial da medição. (`app/ponto/services/cxr_na_lista.py:160`, `:184`) |

A documentação de L-040 ainda afirma ausência de leitor, embora C12 leia a lavra na implementação. O desenho `escopo_id=None` com a constraint anterior também permanece. Fechamento dos requisitos de produção — col841, divergência zero na frota, idempotência concorrente e conservação das classes do vigia — **[nao verificado]**. (`app/docs/LEIS.md:166`; `app/escala/services/cadastro_realidade.py:871`; `app/ponto/services/cxr_na_lista.py:146`; `app/inteligencia/models.py:103`)

## 08/11 03:36 · sessao 5b1d37290c70 · CXR-TELA-LE-O-MOTOR · verificador · origem=Code

**O delta cura (a)–(d) por inspeção. Não encontrei novo defeito bloqueante nos trechos alterados e nos leitores dos campos. O aval completo de produção permanece [não verificado].** Evidências abaixo.

**1) Curas e mapa de leitura/escrita**

- **(a) Chave corrigida:** os dois chamadores passam `ini`; C11 conserva esse início e corta somente `fim`. Isso elimina a escolha equivocada pelo mês do fim cortado. [cadastro_realidade.py:526](app/escala/services/cadastro_realidade.py:526), [cadastro_realidade.py:568](app/escala/services/cadastro_realidade.py:568), [cadastro_realidade.py:1025](app/escala/services/cadastro_realidade.py:1025).
- **(b) Selo fortalecido:** o teste agora exige contagem `(0, 0)` das chamadas ao espelho e ao motor; capturar o `AssertionError` dentro da lista não basta para deixá-lo passar. [test_cxr_tela_le_o_motor.py:132](app/ponto/tests/test_cxr_tela_le_o_motor.py:132), [test_cxr_tela_le_o_motor.py:138](app/ponto/tests/test_cxr_tela_le_o_motor.py:138).
- **(c) Testes passam pela lavra:** “responde como o espelho” chama efetivamente `cxr_lavrado`; o conflito C9/C12 exige `CADASTRO` em `dono_do_dia`, com juiz sensível ao insumo CXR. [test_cxr_tela_le_o_motor.py:252](app/ponto/tests/test_cxr_tela_le_o_motor.py:252), [test_cxr_tela_le_o_motor.py:311](app/ponto/tests/test_cxr_tela_le_o_motor.py:311).
- **(d) Texto corrigido:** a docstring declara filtro, deduplicação e exclusão por erro; o rótulo conta itens. [cxr_na_lista.py:11](app/ponto/services/cxr_na_lista.py:11), [cxr_na_lista.py:131](app/ponto/services/cxr_na_lista.py:131).

| Papel | Escritor/leitor |
|---|---|
| Produz competência e cobertura | `medir`: competência pelo fim integral; cobertura `[pk, comp, início ISO]`. [cxr_na_lista.py:61](app/ponto/services/cxr_na_lista.py:61), [cxr_na_lista.py:115](app/ponto/services/cxr_na_lista.py:115) |
| Persiste e recupera | `lavrar` grava o dicionário em `detalhe`; `ultimo_lavrado` recupera-o. [cxr_na_lista.py:142](app/ponto/services/cxr_na_lista.py:142), [cxr_na_lista.py:156](app/ponto/services/cxr_na_lista.py:156) |
| Interpreta `cobertos` | `cxr_lavrado`: `(colab, início) → competência → conjunto de datas`. [cxr_na_lista.py:173](app/ponto/services/cxr_na_lista.py:173), [cxr_na_lista.py:180](app/ponto/services/cxr_na_lista.py:180) |
| Consome a chave nova | `beco_em_lote` e `um_dono_por_dia`. [cadastro_realidade.py:526](app/escala/services/cadastro_realidade.py:526), [cadastro_realidade.py:1025](app/escala/services/cadastro_realidade.py:1025) |
| Mostra C12 | `_cxr_do_motor` lê `itens`, sem interpretar `cobertos`. [cadastro_realidade.py:393](app/escala/services/cadastro_realidade.py:393) |

**Nenhum leitor produtivo encontrado ainda exige o par `[colab, comp]**`: o leitor estrutural exige três elementos. O par antigo aparece no teste negativo e resulta em `None`; portanto, snapshots antigos perdem cobertura até nova lavra, explicitamente. [cxr_na_lista.py:173](app/ponto/services/cxr_na_lista.py:173), [test_cxr_tela_le_o_motor.py:305](app/ponto/tests/test_cxr_tela_le_o_motor.py:305).

**2) Descartes versus aval**

Não promoveria os descartes funcionais a bloqueadores deste delta. Em particular, erro parcial fica contado, exclui cobertura e o comando publica antes de sair 2; sem cobertura, o ramo dependente de CXR retorna `None`, sem motor. [cxr_na_lista.py:108](app/ponto/services/cxr_na_lista.py:108), [lavrar_cxr_do_motor.py:37](app/ponto/management/commands/lavrar_cxr_do_motor.py:37), [e6_oraculo.py:189](app/ponto/management/commands/e6_oraculo.py:189).

**A pendência `l084_pausa` impede declarar comprovado o PRONTO (1).** O leitor copia a publicação; não consegue trazer uma acusação que o produtor não entregou. Isso não demonstra defeito novo no leitor nem autoriza recalcular na tela. Os sete dias efetivamente publicados e exibidos: [não verificado]. [cxr_na_lista.py:75](app/ponto/services/cxr_na_lista.py:75), [motor_calculo_v2.py:2719](app/ponto/motor_calculo_v2.py:2719).

**3) Outro ponto relevante ao aceite**

O PRONTO (5) exige cuidado: **nem toda saída de `d` vai para `e/dia_na_lista_dono_*`**. Dias reconhecidos como mesa do admin podem entrar antes em `fio_do_admin`; os demais dias presentes na lista chegam ao detalhamento de `e`. Portanto, afirmar fechamento somente em `e` sem comparar os destinos seria incorreto. [vigia_fio.py:654](app/chamados/services/vigia_fio.py:654), [vigia_fio.py:677](app/chamados/services/vigia_fio.py:677), [vigia_fio.py:684](app/chamados/services/vigia_fio.py:684). A comparação nominal antes/depois e sua soma: [não verificado].

Executei sete verificações puras da função de leitura, incluindo competências distintas, cobertura vazia, ausência e formato antigo: passaram. [cxr_na_lista.py:162](app/ponto/services/cxr_na_lista.py:162). Suite Django e selos da frota: [não verificado].

## 08/11 03:35 · sessao aab5013a25cc · - · - · origem=Ronald (tmux)

Não encontrei descarte que deva virar **defeito bloqueante novo** no delta; os pontos abaixo limitam a conclusão de aceite:

- **Erro parcial:** CXR exclui a competência com falha de `cobertos`, registra erro e publica as demais; o command grava e termina com código 2. É a mesma política do C9. Sem cobertura, `sem_motor=True` devolve `None` quando a lista do motor seria necessária, sem chamá-lo. `app/ponto/services/cxr_na_lista.py:108`, `app/ponto/services/cxr_na_lista.py:115`, `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`, `app/ponto/management/commands/lavrar_teto_do_dia.py:34`, `app/ponto/management/commands/e6_oraculo.py:189`.

- **Zero/não medido:** `ultimo_lavrado()` distingue ausência de medição, mas C12 só consome `itens`, perdendo essa distinção na apresentação. Isso permanece no molde já escolhido; não encontrei requisito específico novo que transforme o descarte em bloqueio do delta. `app/ponto/services/cxr_na_lista.py:154`, `app/escala/services/cadastro_realidade.py:393`.

- **Item sem dia:** nasce sem dono na lavra, e a ação continuará “a próxima lavra o traz”; próxima lavra não necessariamente resolverá enquanto o motor continuar sem dia. É limitação real já descartada, compartilhada pela apresentação por dono. A afirmação “sem_dia=0 na frota” não pode ser validada pelos anexos fornecidos. `app/ponto/services/cxr_na_lista.py:81`, `app/ponto/services/cxr_na_lista.py:84`, `app/escala/services/cadastro_realidade.py:759`; número de produção [não verificado].

- **Cron:** a lavra está declarada como elo após `vigia_pareador_celula`, com dependência de `processar_cartorio`; a leitura da manhã usa a lavra anterior por decisão documentada no código. Não há evidência nova para promover a enumeração incompleta do placar a bloqueio funcional deste delta. `app/config/crons.py:1757`, `app/config/crons.py:1765`.

- **`l084_pausa`/PRONTO (1):** os escritores examinados publicam `l084_do_dia`, as causas de janela ou item sem `causa`; C12 copia a publicação, portanto não pode produzir por si os sete exemplos ausentes. O atendimento nominal dos sete dias permanece [não verificado], sem caso fornecido que demonstre falha deste leitor. `app/ponto/motor_calculo_v2.py:1623`, `app/ponto/motor_calculo_v2.py:1670`, `app/ponto/motor_calculo_v2.py:1992`, `app/ponto/motor_calculo_v2.py:2719`, `app/ponto/services/cxr_na_lista.py:75`.

**Cuidado com o PRONTO (5):** não declarar genericamente que cada saída de `d` aparecerá em `e/dia_na_lista_dono_*`. O vigia encaminha previamente dias na lista com BATIDA/CADASTRO para `fio_do_admin`; somente os que permanecem sem outro balde podem entrar em `e/dia_na_lista_dono_*`. A nova C12 calcula o dono com o próprio espelho e pode devolver CADASTRO. A soma exigida precisa de comparação nominal antes/depois; ela não está nos anexos. `app/chamados/services/vigia_fio.py:313`, `app/chamados/services/vigia_fio.py:654`, `app/chamados/services/vigia_fio.py:677`, `app/chamados/services/vigia_fio.py:684`, `app/ponto/services/cxr_na_lista.py:96`, `app/ponto/management/commands/e6_oraculo.py:110`; fechamento da soma [não verificado].

## 08/11 03:35 · sessao 8fade55f0993 · - · - · origem=Ronald (tmux)

Não achei regressão concreta na chave nova nem leitor produtivo que ainda desempacote `cobertos` como par; o único leitor estrutural exige três elementos. `app/ponto/services/cxr_na_lista.py:173`.

Mapa dos campos:

- `medir()` produz `comp` pelo fim integral e grava `[pk, comp, ini.isoformat()]`; devolve `cobertos` no resultado. `app/ponto/services/cxr_na_lista.py:61`, `app/ponto/services/cxr_na_lista.py:115`, `app/ponto/services/cxr_na_lista.py:130`.
- `lavrar()` persiste o resultado inteiro em `detalhe`; `ultimo_lavrado()` recupera esse detalhe. `app/ponto/services/cxr_na_lista.py:142`, `app/ponto/services/cxr_na_lista.py:156`.
- `cxr_lavrado()` converte os triplos em `(colab, início) → competência` e busca os dias por `(colab, competência)`. Formatos antigos ficam sem cobertura, retornando `None`. `app/ponto/services/cxr_na_lista.py:173`, `app/ponto/services/cxr_na_lista.py:177`, `app/ponto/services/cxr_na_lista.py:180`.
- Os dois consumidores produtivos passam `ini`: C11 em `beco_em_lote()` e reconciliação em `um_dono_por_dia()`. `app/escala/services/cadastro_realidade.py:526`, `app/escala/services/cadastro_realidade.py:1025`.
- C11 preserva o início da janela e corta somente o fim; a reconciliação deriva a janela da data do item pela autoridade `janela_atual`. `app/escala/services/cadastro_realidade.py:568`, `app/escala/services/cadastro_realidade.py:569`, `app/escala/services/cadastro_realidade.py:1017`.
- `InsumosDoDono` recebe o conjunto ou `None`; sem cobertura e sem motor, o ramo que precisaria do CXR devolve `None`, sem fallback. `app/ponto/management/commands/e6_oraculo.py:147`, `app/ponto/management/commands/e6_oraculo.py:189`.
- C12 lê apenas `itens`, sem interpretar `cobertos`. `app/escala/services/cadastro_realidade.py:393`.
- O único par antigo explícito é a fixture negativa que exige `None`; os testes novos também distinguem os inícios das competências 10 e 11. `app/ponto/tests/test_cxr_tela_le_o_motor.py:302`, `app/ponto/tests/test_cxr_tela_le_o_motor.py:305`.
- O homônimo `cobertos` de `core/views.py` é um contador de postos, independente do snapshot CXR. `app/core/views.py:882`, `app/core/views.py:891`.

Execução dos testes e estado efetivo dos snapshots de produção: [não verificado]. [nao verificado]

## 08/11 03:19 · sessao d8ec781d1406 · CXR-TELA-LE-O-MOTOR · verificador · origem=Code

**Não aprovo a v1 como pronta.** Há publicação de lavra parcial como execução concluída, lacunas nos selos e ausência de prova do caso obrigatório de pausa. O caminho principal da lista, entretanto, passou a usar `sem_motor=True`. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:37`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:130`; `app/escala/services/cadastro_realidade.py:525`; `app/escala/services/cadastro_realidade.py:1024`)

Verificação estática, com contraprova da seleção de competência executada somente em memória. Resultado de suíte, sete dias de col841, equivalência na frota e fechamento da soma d→e: **[nao verificado]**. [nao verificado]

**1. Escritores, leitores e significado dos campos**

O percurso é motor → resultado do espelho → `medir` → `lavrar` → snapshot → formatador C12/portador do dono → lista/PDF/vigia. O resultado entregue pelo espelho vem da janela **visual**, `vis_ini..hoje`, e não exclusivamente de `apur_ini..apur_fim`. (`app/ponto/motor_calculo_v2.py:2719`; `app/ponto/services/espelho.py:1225`; `app/ponto/services/espelho.py:1227`; `app/ponto/services/espelho.py:1294`; `app/ponto/services/cxr_na_lista.py:71`; `app/escala/services/cadastro_realidade.py:393`)

| Campo | Quem escreve; quem lê; significado |
|---|---|
| Snapshot `chave`, `escopo`, `escopo_id` | `lavrar`: `cxr_do_motor`, `frota`, `None`. `ultimo_lavrado` procura chave/escopo, sem filtrar `escopo_id`. (`app/ponto/services/cxr_na_lista.py:137`; `app/ponto/services/cxr_na_lista.py:147`) |
| `data_ini`, `data_ref` | `lavrar` grava **a data da medição** nos dois; não são os limites das competências. `ultimo_lavrado` ordena por `data_ref` e a devolve como texto ISO. (`app/ponto/services/cxr_na_lista.py:138`; `app/ponto/services/cxr_na_lista.py:148`; `app/ponto/services/cxr_na_lista.py:153`) |
| `numerador`, `denominador`, `valor`, `detalhe` | `lavrar` grava respectivamente `total`, `universo`, `None` e o dicionário inteiro. Portanto numerador é quantidade de itens distintos; denominador é quantidade de colab–competência examinados, não dias nem colaboradores únicos. (`app/ponto/services/cxr_na_lista.py:65`; `app/ponto/services/cxr_na_lista.py:118`; `app/ponto/services/cxr_na_lista.py:139`) |
| `calculado_em`, `medido_em` | O modelo atualiza `calculado_em` ao salvar; `ultimo_lavrado` o expõe como `medido_em`. É carimbo da gravação, posterior à captura `_agora_do_ato`. (`app/inteligencia/models.py:96`; `app/ponto/services/cxr_na_lista.py:52`; `app/ponto/services/cxr_na_lista.py:152`) |
| `itens[].colab`, `empresa` | `medir` acrescenta os PKs do colaborador e de sua empresa na seleção. O formatador agrupa por `colab`; a filtragem final da lista usa a empresa **atual**, carregada por `_vinculos`, e não `itens[].empresa`. (`app/ponto/services/cxr_na_lista.py:61`; `app/ponto/services/cxr_na_lista.py:77`; `app/escala/services/cadastro_realidade.py:401`; `app/escala/services/cadastro_realidade.py:769`; `app/escala/services/cadastro_realidade.py:899`) |
| `itens[].competencia` | `medir` escreve `MM/AAAA` do fim da janela canônica. Formatador usa no detalhe; `cxr_lavrado` usa na chave de consulta. O corte vem da empresa; 21→20 é o default, não constante do serviço novo. (`app/ponto/services/cxr_na_lista.py:30`; `app/ponto/services/cxr_na_lista.py:57`; `app/ponto/services/cxr_na_lista.py:170`; `app/ponto/janelas.py:36`; `app/escala/services/cadastro_realidade.py:404`) |
| `itens[].data` | `medir` converte `dia` em ISO e exige intervalo **inclusivo** `ini <= d <= fim`; ausência de `dia` permanece `None`. Lista, reconciliação e vigia usam essa data como identidade do dia. (`app/ponto/services/cxr_na_lista.py:80`; `app/ponto/services/cxr_na_lista.py:88`; `app/ponto/services/cxr_na_lista.py:95`; `app/escala/services/cadastro_realidade.py:1003`; `app/escala/services/cadastro_realidade.py:1061`) |
| `ec`, `template_id` | Acrescentados pela lavra consultando o vínculo **do dia**. O formatador renomeia para `ec_do_dia`/`template_do_dia`; a ação C12 usa o template histórico. O agrupamento e o template mostrado na linha continuam sendo os de hoje. (`app/ponto/services/cxr_na_lista.py:94`; `app/escala/services/cadastro_realidade.py:402`; `app/escala/services/cadastro_realidade.py:923`; `app/escala/services/cadastro_realidade.py:782`) |
| `dono` | Acrescentado pela lavra mediante `InsumosDoDono.dono(d)`, usando o espelho já obtido. O formatador o transforma em `dono_do_dia`; a reconciliação pode posteriormente sobrescrevê-lo. (`app/ponto/services/cxr_na_lista.py:92`; `app/ponto/services/cxr_na_lista.py:96`; `app/escala/services/cadastro_realidade.py:403`; `app/escala/services/cadastro_realidade.py:1028`) |
| `causa`, `real`, `cadastrado`, `minutos_fora` | O produtor HE escreve causa, horários locais `HH:MM` e minutos arredondados. `minutos_fora` é a **sobra após extra declarada**, enquanto `cadastrado` continua sendo o marco cru. A lavra copia; o formatador imprime. (`app/ponto/motor_calculo_v2.py:1662`; `app/ponto/motor_calculo_v2.py:1670`; `app/ponto/motor_calculo_v2.py:1702`; `app/ponto/services/cxr_na_lista.py:79`; `app/escala/services/cadastro_realidade.py:394`) |
| `entrada_real`, `entrada_cadastrada`, `saida_real`, `saida_cadastrada` | Os produtores L-084 escrevem instantes ISO com offset, e não apenas `HH:MM`. Lavra copia como texto; formatador imprime sem conversão. (`app/ponto/motor_calculo_v2.py:1624`; `app/ponto/motor_calculo_v2.py:1998`; `app/ponto/services/cxr_na_lista.py:39`; `app/escala/services/cadastro_realidade.py:398`) |
| `delta_entrada_min`, `delta_saida_min` | Minutos com sinal, arredondados a uma decimal: entrada = real − prevista; saída = prevista − real. Logo saída posterior produz delta negativo. Copiados pela lavra e impressos pelo formatador. (`app/ponto/motor_calculo_v2.py:1973`; `app/ponto/motor_calculo_v2.py:1975`; `app/ponto/motor_calculo_v2.py:2002`; `app/escala/services/cadastro_realidade.py:399`) |
| `cobertos` | `medir` escreve `[colab, competencia]` somente após concluir aquela leitura, inclusive quando nenhum item sobra. `cxr_lavrado` distingue coberto vazio (`set()`) de não coberto (`None`). (`app/ponto/services/cxr_na_lista.py:104`; `app/ponto/services/cxr_na_lista.py:110`; `app/ponto/services/cxr_na_lista.py:166`; `app/ponto/services/cxr_na_lista.py:174`) |
| `fora_da_competencia` | Conta ocorrências descartadas pelo filtro, por leitura de competência. O command imprime; o produto C12 não expõe esse contador. Não representa quantidade de dias distintos. (`app/ponto/services/cxr_na_lista.py:88`; `app/ponto/services/cxr_na_lista.py:109`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:33`; `app/escala/services/cadastro_realidade.py:393`) |
| `repetidos_do_motor` | Conta itens iguais **depois da projeção/conversão**, dentro de um colab–competência. O command imprime; a lista recebe somente o item deduplicado. (`app/ponto/services/cxr_na_lista.py:79`; `app/ponto/services/cxr_na_lista.py:100`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:33`) |
| `total`, `colabs`, `universo`, `sem_dia` | `medir` calcula itens distintos, colaboradores com itens, colab–competência selecionados e itens sem data. O command imprime. O contador C12 da tela é recontado sobre assinaturas efetivamente exibidas. (`app/ponto/services/cxr_na_lista.py:118`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:30`; `app/escala/services/cadastro_realidade.py:933`; `app/escala/services/cadastro_realidade.py:960`) |
| `por_competencia`, `por_dono`, `por_causa` | Agregações dos itens sobreviventes, escritas por `medir`; command imprime as duas primeiras. Causa ausente ganha o rótulo `l084_sem_causa` somente na agregação. (`app/ponto/services/cxr_na_lista.py:111`; `app/ponto/services/cxr_na_lista.py:115`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:32`) |
| `janelas`, `competencias`, `erros`, `erros_amostra`, `rotulo`, `onde_olha` | Escritos por `medir`, armazenados no detalhe. Command usa `erros` para retornar 2; formatador C12 lê apenas `itens`, sem levar cobertura, erro ou frescura à saída da lista. (`app/ponto/services/cxr_na_lista.py:58`; `app/ponto/services/cxr_na_lista.py:121`; `app/ponto/services/cxr_na_lista.py:126`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:39`; `app/escala/services/cadastro_realidade.py:393`) |
| `sem_medida` | Produzido por `ultimo_lavrado` quando não há snapshot; ignorado pelo formatador C12. (`app/ponto/services/cxr_na_lista.py:149`; `app/escala/services/cadastro_realidade.py:393`) |

`InsumosDoDono._cxr` nasce `None`, recebe conjunto de datas por injeção ou pelo espelho e é consultado por pertinência em `dono`. `_sem_motor` é escrito pelo construtor e lido nas duas guardas. Sem cobertura, outros insumos ainda podem decidir CADASTRO/BATIDA; apenas a resposta residual que dependeria de CXR fica `None`. (`app/ponto/management/commands/e6_oraculo.py:133`; `app/ponto/management/commands/e6_oraculo.py:146`; `app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/management/commands/e6_oraculo.py:189`)

| Assinatura | Escritor original de `dono_do_dia` |
|---|---|
| C9 | Serviço do teto escreve `dono`; formatador renomeia. (`app/ponto/services/teto_na_lista.py:86`; `app/escala/services/cadastro_realidade.py:379`) |
| C10 | Censo pergunta ao mesmo portador com os dias de fase da própria corrida e grava `dono_do_dia`. (`app/ponto/management/commands/censo_fase_12x36.py:287`; `app/ponto/management/commands/censo_fase_12x36.py:300`) |
| C11 | Beco pergunta ao portador durante a leitura; não é dono previamente lavrado diariamente. (`app/escala/services/cadastro_realidade.py:531`) |
| C12 | Lavra pergunta ao portador com o espelho da própria medição; formatador renomeia. (`app/ponto/services/cxr_na_lista.py:92`; `app/ponto/services/cxr_na_lista.py:96`; `app/escala/services/cadastro_realidade.py:403`) |

Todos podem ser sobrescritos por `um_dono_por_dia`: reaproveita C11 ou pergunta novamente ao juiz. Assim, na saída, `dono_do_dia` significa **dono reconciliado**, não necessariamente o dono originalmente gravado naquele snapshot. Ações, contadores e vigia leem esse resultado. (`app/escala/services/cadastro_realidade.py:1009`; `app/escala/services/cadastro_realidade.py:1026`; `app/escala/services/cadastro_realidade.py:1028`; `app/escala/services/cadastro_realidade.py:923`; `app/escala/services/cadastro_realidade.py:1063`)

**Não há significado temporal inteiramente uniforme:** HE escreve `entrada.date()`, enquanto L-084 escreve o dia atribuído pelo juiz. Além disso, motor usa `settings.TIME_ZONE`; lavra/portador usam timezone corrente. O default é São Paulo; divergência efetiva de fuso em produção é **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:1528`; `app/ponto/motor_calculo_v2.py:1997`; `app/ponto/motor_calculo_v2.py:73`; `app/ponto/management/commands/e6_oraculo.py:149`; `app/config/settings/base.py:93`)

**2. Caminhos de request e leitores da lista**

**No grafo da lista CxR, não encontrei caminho sobrevivente que execute o espelho/motor.** As duas montagens potencialmente perigosas usam `sem_motor=True`, cuja guarda antecede a chamada ao espelho. (`app/escala/services/cadastro_realidade.py:525`; `app/escala/services/cadastro_realidade.py:1024`; `app/ponto/management/commands/e6_oraculo.py:166`)

Caminhos localizados:

- View HTML/PDF → `lista`; PDF recebe os dados montados e renderiza. (`app/escala/views.py:487`; `app/escala/views.py:489`; `app/escala/services/cadastro_realidade.py:1067`)
- `lista` → C11 → `beco_em_lote` → portador sem motor. (`app/escala/services/cadastro_realidade.py:864`; `app/escala/services/cadastro_realidade.py:573`; `app/escala/services/cadastro_realidade.py:525`)
- `lista` → `um_dono_por_dia` → portador sem motor. (`app/escala/services/cadastro_realidade.py:885`; `app/escala/services/cadastro_realidade.py:1024`)
- `vigiar_fio_morto` → `ListaDoAdmin` → `lista` → `dias_na_lista`. (`app/chamados/management/commands/vigiar_fio_morto.py:167`; `app/chamados/services/vigia_fio.py:216`; `app/chamados/services/vigia_fio.py:217`)
- Commands `fechar_avisos_regua_na_lista` e `fechar_chamados_vinculo_na_lista` → mesma `lista`. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`)
- Consumidores externos do beco: pauta de competência exportada e reatador chegam ao mesmo serviço alterado. (`app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:105`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:54`)

Fora desse grafo, as próprias telas de espelho continuam chamando `espelho_do_colab`; portanto “zero motor em qualquer request do sistema” seria falso. (`app/ponto/views.py:158`; `app/api/views.py:1369`)

**3. Classes de reprovação**

- **Segundo juiz de dono:** não identifiquei duplicação da regra CADASTRO/BATIDA/ESTRUTURA; ela continua no juiz existente. Os novos ramos mudam a obtenção do insumo e a resposta desconhecida. (`app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:110`; `app/ponto/management/commands/e6_oraculo.py:195`)
- **Segundo escritor:** o escritor localizado para a nova chave é `cxr_na_lista.lavrar`, chamado pelo command. Ele não escreve a lista interna do motor. A frase do modelo “escrito só por inteligencia.metricas” ficou documentalmente incompatível com esse padrão, que já é usado por C9. (`app/ponto/services/cxr_na_lista.py:134`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`; `app/inteligencia/models.py:82`; `app/ponto/services/teto_na_lista.py:113`)
- **Relógio fora da borda:** o command captura `localdate()` e o serviço captura outro `now()`. Caso: atravessar 20→21 entre as chamadas mantém competência/data_ref de ontem com instante de hoje. A cadeia também conserva o relógio próprio de C2 e outro no PDF. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:28`; `app/ponto/services/cxr_na_lista.py:52`; `app/escala/services/cadastro_realidade.py:252`; `app/escala/services/cadastro_realidade.py:1076`)
- **Derivação paralela:** C12 não recalcula distância, mas filtra, projeta e deduplica a publicação. Portanto “item por item, sem filtro e sem conta” não descreve o código. O censo `CLASSIFICAR` acrescentado pelo diff é informativo, não certificado dessa equivalência. (`app/ponto/services/cxr_na_lista.py:11`; `app/ponto/services/cxr_na_lista.py:79`; `app/ponto/services/cxr_na_lista.py:88`; `app/ponto/services/cxr_na_lista.py:100`; `app/core/tests/test_medida_turno_do_par.py:12`)
- **Ordem de trava:** o elo toma o `flock`, verifica conclusão própria e espera o predecessor antes de executar. Não identifiquei inversão nova nesse caminho; concorrência entre execuções manuais e cron não está provada pelo teste sequencial. (`bin/encadeado.sh:78`; `bin/encadeado.sh:82`; `bin/encadeado.sh:89`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:118`)
- **Pouso antes do pré-requisito:** o corte exige acusador de pausa no motor, e o golden da obra é `l084_pausa`; os produtores localizados nesta cópia são HE, `l084_do_dia` e L-084 sem causa. Os sete dias em produção permanecem **[nao verificado]**. (`app/docs/CORTES.md:304`; `app/docs/BACKLOG.md:775`; `app/ponto/motor_calculo_v2.py:1623`; `app/ponto/motor_calculo_v2.py:1670`; `app/ponto/motor_calculo_v2.py:1992`)
- **Campo com dois significados:** há `dia` civil versus dia do turno; `fim` completo versus fim cortado; dono original versus reconciliado. O segundo caso tem contraprova abaixo. (`app/ponto/motor_calculo_v2.py:1528`; `app/ponto/motor_calculo_v2.py:1997`; `app/escala/services/cadastro_realidade.py:569`; `app/ponto/services/cxr_na_lista.py:173`; `app/escala/services/cadastro_realidade.py:1028`)

Os testes **não mordem tudo o que anunciam**: [nao verificado]

- O selo sem motor arma `AssertionError`, mas a lista captura `Exception`, inclusive em C11. Uma chamada indevida nesse ramo pode ser engolida, C11 desaparecer e o teste continuar verde porque exige apenas C12 e um PDF válido. Não há `assert_not_called`. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:130`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:134`; `app/escala/services/cadastro_realidade.py:866`)
- “`cxr_lavrado` responde como o espelho” **não chama `cxr_lavrado`**: injeta manualmente o mesmo dia nas duas montagens. Não testa `cobertos`, filtro ou competência. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:284`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:286`)
- O teste real cobre HE 18:56×21:00; os demais usam publicações artificiais. Não certifica pausa nem os sete dias obrigatórios. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:92`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:154`)
- Idempotência compara quantidade de snapshots e `itens`; o selo PDF procura cada data esperada no texto, sem excluir datas extras. São verificações parciais das promessas “mesmo estado” e “mesmos dias”. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:122`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:151`)

**4. Filtro, cobertura e equivalência para `dono()`**

**Os conjuntos completos não são iguais.** Para a mesma publicação, o espelho fornece todos os dias; a lavra conserva a interseção com a competência. Entretanto, para um dia consultado **dentro daquela competência**, o filtro sozinho preserva a pertinência. Descartar agosto não é contraexemplo suficiente para `dono(outubro)`. (`app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/services/cxr_na_lista.py:88`; `app/ponto/management/commands/e6_oraculo.py:195`)

Em `um_dono_por_dia`, a janela vem de `janela_atual(dia)`. Logo há equivalência de pertinência **condicionada à mesma publicação, cobertura e identidade de competência**; o código não prova equivalência entre a lavra diária e um espelho recalculado posteriormente. (`app/escala/services/cadastro_realidade.py:1017`; `app/ponto/services/cxr_na_lista.py:147`; `app/ponto/services/cxr_na_lista.py:174`)

**Contraexemplo efetivo da API:** em 25/10, C11 corta a janela corrente para 21/10–24/10 e passa esse `fim`. `cxr_lavrado` busca `10/2026`, embora a competência corrente termine em 20/11. Com itens 02/10 na competência 10 e 22/10 na 11, a consulta retorna `{02/10}` em vez de `{22/10}`. (`app/escala/services/cadastro_realidade.py:568`; `app/escala/services/cadastro_realidade.py:569`; `app/escala/services/cadastro_realidade.py:526`; `app/ponto/services/cxr_na_lista.py:173`)

**Isso não muda hoje o dono de C11:** o beco exige `folga_nao_definida=True`, e esse insumo já decide CADASTRO antes de CXR. É erro de contrato mascarado pela precedência atual. (`app/escala/services/cadastro_realidade.py:518`; `app/ponto/management/commands/e6_oraculo.py:98`)

Há ainda diferença de universo: cobertura exige batida no intervalo civil `[ini 00h, fim+1 00h)`, enquanto os portadores/autoridade carregam bordas e o juiz pode atribuir batidas à véspera. Assim, cobertura civil não prova cobertura de todos os dias de turno. Um caso real afetado em produção é **[nao verificado]**. (`app/ponto/services/cxr_na_lista.py:59`; `app/ponto/services/cxr_na_lista.py:61`; `app/ponto/management/commands/e6_oraculo.py:150`; `app/ponto/services/espelho.py:1020`; `app/ponto/turnos.py:306`)

**5. Contratos do ENC e registros**

A declaração está integrada: gatilho `vigia_pareador_celula`, dependência `processar_cartorio`, estágio `auditoria`, `--apply` e papel `lavra`. O teto 900 limita a espera pelo predecessor, não a duração do próprio lavrador. (`app/config/crons.py:1765`; `app/config/crons.py:2232`; `bin/encadeado.sh:92`; `bin/encadeado.sh:102`)

Não falta registrar em `FORA_DE_PIPELINE`: `todas_as_entradas()` já inclui `ENCADEADOS`, e o contrato “command tem casa” usa essa fonte. O contrato de duração também alcança encadeados e exige ausência de `sched`. (`app/config/crons.py:1783`; `app/chamados/tests/test_contract_crons.py:220`; `app/core/tests/test_contract_crons_duracao_horario.py:43`)

**Falta cobertura no consumidor `pipeline_placar`:** ele enumera somente `CRONS`. Caso concreto: o novo lavrador nun

[truncado em 20000 de 23391 caracteres]

## 08/11 03:15 · sessao 326fdbbc8265 · - · - · origem=Ronald (tmux)

Mapeamento e achados para consolidação:

- O resultado consumido **não é recortado à competência pedida**: `espelho_do_colab` chama `autoridade_do_periodo(colaborador, vis_ini, hoje)` e devolve seu `resultado`; apenas outras partes visuais/totais recebem `apur_ini/apur_fim`. Portanto o filtro novo é uma transformação efetiva da publicação, não cópia literal. Fontes: `app/ponto/services/espelho.py:1225`, `:1227`, `:1229`, `:1294`; filtro em `app/ponto/services/cxr_na_lista.py:88`.

- **O campo `dia` mistura dia civil e dia do turno**: HE usa `entrada.date()` e publica `j['dia']`; L084 publica o mapa do juiz ou `_dt_b`. A lavra interpreta todos como mesmo `data`, competência e vínculo. Fontes: `app/ponto/motor_calculo_v2.py:1528`, `:1711`, `:1749`, `:1623`, `:1997`; `app/ponto/services/cxr_na_lista.py:80`, `:88`, `:94`. Caso possível: cauda `E03:00→S11:00` em 21/10 de jornada iniciada em 20/10, cadastro 21–09, saída acima do teto; a acusação HE usa 21/10 embora o turno possa pertencer a 20/10. A herança da jornada e o mapa existem no próprio motor: `app/ponto/motor_calculo_v2.py:2403`, `:2407`, `:2455`. Execução desse cenário completo `[nao verificado]`.

- **Não usar a remoção dos dias externos como contraexemplo isolado de `dono(d)`**: para mesma publicação e `d∈[ini,fim]`, o filtro preserva pertinência. `cxr()` retorna os dias de todo o espelho, enquanto a lavra guarda a interseção; a diferença fora da janela não muda a pergunta interna. Fontes: `app/ponto/management/commands/e6_oraculo.py:170`, `:195`; `app/ponto/services/cxr_na_lista.py:88`, `:170`, `:174`.

- **Contraexemplo concreto da API C11**: em 25/10, a janela corrente 21/10–20/11 vira 21/10–24/10; `cxr_do(cid,fim)` procura `10/2026`, embora o dia 22/10 pertença a `11/2026`. Com lavra coberta nas duas competências e item somente 22/10 na 11, a função devolve `[]` para fim24/10 e `['2026-10-22']` para fim20/11. Executei somente a função extraída por AST com `python3 -B`, sem Django ou escrita. Fontes: `app/escala/services/cadastro_realidade.py:569`, `:525`; `app/ponto/services/cxr_na_lista.py:173`. **Impacto no dono C11 é neutralizado hoje**: o chamador exige `folga_nao_definida`, e o juiz retorna CADASTRO antes de ler CXR. Fontes: `app/escala/services/cadastro_realidade.py:518`; `app/ponto/management/commands/e6_oraculo.py:98`.

- **Cobertura também usa outra geometria**: universo da lavra exige qualquer batida no intervalo civil `[ini00h, (fim+1)00h)`, enquanto Insumos e autoridade incluem bordas e atribuem dia pelo turno. Um colaborador com todas as batidas em 21/10, atribuídas ao turno20/10, pode ficar sem cobertura na competência10 apesar de existir dia20 no espelho. A ocorrência desse caso em produção `[nao verificado]`. Fontes: `app/ponto/services/cxr_na_lista.py:59`, `:61`, `:64`, `:110`; `app/ponto/management/commands/e6_oraculo.py:150`; `app/ponto/services/espelho.py:1019`; atribuição à véspera em `app/ponto/turnos.py:299`, `:306`.

- **Unidades e formatos**: HE publica `real/cadastrado` como `HH:MM`, `minutos_fora` inteiro arredondado e já descontada a extra declarada; `cadastrado` continua marco cru. L084 publica timestamps ISO locais com offset e deltas em minutos com uma decimal; entrada é `real-prevista`, saída é `prevista-real`. Fontes: `app/ponto/motor_calculo_v2.py:1670`, `:1662`, `:1702`, `:1742`, `:1624`, `:1974`, `:1976`, `:1998`. Fuso do motor é fixo `settings.TIME_ZONE`, configurado São Paulo; lavra e Insumos usam o timezone corrente. Divergência efetiva de timezone em produção `[nao verificado]`. Fontes: `app/ponto/motor_calculo_v2.py:28`, `:73`; `app/config/settings/base.py:93`; `app/ponto/services/cxr_na_lista.py:59`; `app/ponto/management/commands/e6_oraculo.py:149`.

- **Golden de pausa sem produtor demonstrado**: não há `l084_pausa` nem `acusar_pausa_l084` em Python de produção desta cópia, pela busca completa de `app`. Os escritores efetivos encontrados publicam `l084_do_dia`, HE e L084 sem causa; `resultado` recebe a lista no final. Fontes positivas: `app/ponto/motor_calculo_v2.py:1623`, `:1670`, `:1992`, `:2719`. O prerequisite é nomeado em `app/docs/CORTES.md:304` e o golden em `app/docs/BACKLOG.md:775`; o relato de commit anterior não prova que esse código esteja nesta cópia (`app/docs/RELATO.md:1778`). Existência dos sete dias em produção `[nao verificado]`.

- **Teste real não cobre pausa**: a fixture real e a asserção exercitam HE de entrada18:56 contra21:00; os demais casos publicam dados por stub. Não comprovam o golden `l084_pausa` nem equivalência completa com `cxr_lavrado`: o teste de equivalência passa uma lista manual diretamente a `InsumosDoDono`. Fontes: `app/ponto/tests/test_cxr_tela_le_o_motor.py:42`, `:76`, `:92`, `:154`, `:284`.

- **Falha parcial substitui snapshot completo antes de informar erro**: o comando grava a medida e só depois sai com código2; cada falha de colaborador remove seus itens e sua cobertura. Caso: snapshot bom tem C12 de A e B; rodada seguinte falha apenas B, grava somente A e a tela perde B. Fontes: `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`, `:39`; `app/ponto/services/cxr_na_lista.py:104`, `:106`, `:110`; `ultimo_lavrado` escolhe o mais recente em `:147`.

## 08/11 03:14 · sessao f8b4a879ff68 · - · - · origem=Ronald (tmux)

**Cron: a declaração do elo está integrada, mas há uma lacuna de observabilidade e um problema no tratamento de falha parcial.** (`app/config/crons.py:1765`, `app/core/management/commands/pipeline_placar.py:38`, `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`).

- **Gatilho/estágio/papel:** `lavrar_cxr_do_motor` espera `vigia_pareador_celula`, usa `--apply`, estágio `auditoria`, teto de espera 900 e dependência declarada de `processar_cartorio`; seu papel é `lavra`. (`app/config/crons.py:1765`, `app/config/crons.py:2232`).
- **O teto de 900 não limita a duração do próprio lavrador:** limita a espera pelos arquivos de fim do gatilho; depois o comando é executado sem timeout nesse script. (`bin/encadeado.sh:84`, `bin/encadeado.sh:92`, `bin/encadeado.sh:102`).
- **Ordem por arquivo:** a corrente usa o campo `gatilho`, percorre os elos sequencialmente e cada elo espera o carimbo de seu predecessor; o novo elo fica depois do pareador e, transitivamente, depois do cartório. (`app/config/crons.py:1817`, `app/config/crons.py:1835`, `bin/encadeado.sh:56`, `bin/encadeado.sh:89`).
- **Não falta pôr o command em `FORA_DE_PIPELINE`:** `todas_as_entradas()` já inclui `ENCADEADOS`, e o contrato de command com casa usa essa função; duplicá-lo em `FORA_DE_PIPELINE` seria entrada podre. (`app/config/crons.py:1783`, `app/chamados/tests/test_contract_crons.py:220`, `app/chamados/tests/test_contract_crons.py:230`).
- **Sombra e diagrama recebem o novo elo automaticamente:** `bloco_da_manha()` inclui os encadeados e o gerador do diagrama também; a alteração em `ARQUITETURA.mmd` corresponde ao mecanismo gerador. (`app/config/crons.py:1910`, `app/core/management/commands/gerar_diagrama.py:182`, `app/docs/ARQUITETURA.mmd:61`, `app/docs/ARQUITETURA.mmd:112`).
- **Contrato de duração/horário:** o teste exige que todo encadeado conte em qualquer horário e não tenha `sched`; isso é satisfeito pela forma declarada, e `corrida_no_horario()` devolve `True` para nomes fora de `CRONS`. (`app/core/tests/test_contract_crons_duracao_horario.py:43`, `app/config/crons.py:2303`, `app/config/crons.py:2316`).
- **Não existe duração cadastrada para o novo lavrador nesta cópia:** a declaração não o põe em `_DUR_DECLARADA`, e o JSON passa de `lavrar_badge_navbar` para `lavrar_deslogados`; futuras amostras entram pelo medidor de nomes do placar. Isso não exige inventar duração manual para este último elo. (`app/config/crons.py:125`, `app/config/crons_duracao.json:38`, `bin/crons_duracao.py:73`).

**Achado: o placar de pipeline ignora o novo ENC.** `esperados()` enumera somente `CRONS`, `DEPS` também, e o relatório só classifica esses esperados; registros de encadeados aparecem como `ORFAO no jsonl sem cron`. Caso concreto: o lavrador nunca executa e o placar não produz `NAO_RODOU lavrar_cxr_do_motor`; se executa, sua corrida fica órfã, sem classificação normal de dependência. É defeito preexistente do consumidor que atinge o novo elo, não falta de adicionar outro literal de cron. (`app/core/management/commands/pipeline_placar.py:9`, `app/core/management/commands/pipeline_placar.py:12`, `app/core/management/commands/pipeline_placar.py:38`, `app/core/management/commands/pipeline_placar.py:88`, `app/core/management/commands/pipeline_placar.py:108`, `app/core/management/commands/pipeline_placar.py:128`).

**Achado: erro técnico de leitura vira lavra parcial concluída pelo cron.** O serviço retira completamente o colaborador/competência que falhou, mas continua; o command grava o resultado parcial antes de sair com código 2. O envelope entende 2 como alarme de negócio, escreve carimbo de conclusão e não abre cron quebrado; outra tentativa encadeada no mesmo dia termina imediatamente por esse carimbo. Caso concreto: ontem C12 tinha um dia do colaborador; hoje a leitura desse espelho lança exceção; a nova lavra perde esse dia e é considerada concluída para fins da corrente. (`app/ponto/services/cxr_na_lista.py:101`, `app/ponto/services/cxr_na_lista.py:103`, `app/ponto/services/cxr_na_lista.py:105`, `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`, `app/ponto/management/commands/lavrar_cxr_do_motor.py:39`, `bin/cron_run.sh:19`, `bin/cron_run.sh:22`, `bin/cron_run.sh:32`, `bin/encadeado.sh:81`).

**Relógio com duas leituras:** o command captura `hoje` em `timezone.localdate()` e o serviço captura outro instante com `timezone.now()`; como recebeu `hoje`, não o deriva desse instante. Caso concreto de borda: o command captura 20/11 às 23:59:59 e o serviço entra em 21/11, com janela/data_ref de ontem e instante de hoje. Ocorrência real na produção `[nao verificado]`. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:28`, `app/ponto/services/cxr_na_lista.py:52`, `app/ponto/services/cxr_na_lista.py:53`, `app/ponto/services/cxr_na_lista.py:138`).

**Congelamento de cron:** a marca CRON15 contém proibição literal de “cron novo ou mudança de horário” enquanto existir; o backlog a repete. Entretanto, o aval anexado de 06/11 autoriza expressamente esta obra com cron de lavra, e o backlog da própria CXR registra contador + cron + serviço. Não recomendo reprovar automaticamente por autorização ausente: há autorização específica posterior no dado fornecido; eventual precedência entre os dois cortes além disso é `[nao verificado]`. (`app/core/tests/marca_cron15.py:13`, `app/docs/BACKLOG.md:747`, `app/docs/BACKLOG.md:775`; aval.txt fornecido, sem numeração local `[nao verificado]`).

**Mordida dos contratos de cron é limitada:** vários testes de existência, estágio, DAG e cobertura diária percorrem só `CRONS`; não validam diretamente estes mesmos atributos dos novos `ENCADEADOS`. O contrato de duração percorre todos os encadeados, e “command tem casa” também os alcança pela função canônica; portanto não cabe afirmar que todos os contratos de cron provaram este elo. (`app/chamados/tests/test_contract_crons.py:25`, `app/chamados/tests/test_contract_crons.py:77`, `app/chamados/tests/test_contract_crons.py:269`, `app/chamados/tests/test_contract_crons.py:386`, `app/chamados/tests/test_contract_crons.py:431`, `app/core/tests/test_contract_crons_duracao_horario.py:45`, `app/chamados/tests/test_contract_crons.py:225`).

Não executei suíte nem banco. Uma importação pura com `python3 -B`, sem escrita, confirmou papel `lavra`, presença na corrente do cartório e no bloco da manhã; resultado de suíte GREEN `[nao verificado]`. [nao verificado]

## 08/11 03:14 · sessao 6a0b79b484ad · - · - · origem=Ronald (tmux)

Rastreio de request e donos, somente por leitura estática; execução em banco/testes `[nao verificado]`. [nao verificado]

1. Não encontrei chamada a `espelho_do_colab` nem a `MotorBase.calcular_mes` nas cadeias da lista depois do diff. As duas construções de `InsumosDoDono` dentro desse módulo passam `sem_motor=True`; a única chamada interna ao espelho fica protegida por essa flag. `app/escala/services/cadastro_realidade.py:525`, `app/escala/services/cadastro_realidade.py:1024`, `app/ponto/management/commands/e6_oraculo.py:166`.

   Cadeias localizadas:

   - HTML/PDF: `cadastro_x_realidade → lista`; PDF recebe o resultado já montado e só o renderiza. `app/escala/views.py:487`, `app/escala/views.py:489`, `app/escala/services/cadastro_realidade.py:1067`.
   - Vigia: `vigiar_fio_morto → ListaDoAdmin → lista → dias_na_lista`. `app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:217`.
   - Outros leitores diretos de `lista()`: comandos `fechar_avisos_regua_na_lista` e `fechar_chamados_vinculo_na_lista`. `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`.
   - Leitores do beco fora da lista: pauta da competência exportada e reatador do beco; ambos chegam ao mesmo `beco_em_lote`. `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:105`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:54`, `app/escala/services/cadastro_realidade.py:439`, `app/escala/services/cadastro_realidade.py:573`.

2. **Selo C3 tem brecha de mordida.** O teste arma `AssertionError` e verifica apenas que C12 continua presente e PDF existe. A lista captura `Exception` nos ramos, inclusive C11; se uma chamada indevida ao motor for introduzida no beco, a exceção pode ser engolida, C11 desaparecer, C12 continuar e esse teste passar. Caso concreto: recolocar uma chamada ao espelho no ramo C11; a fixture não exige C11 e não há `assert_not_called`. `app/ponto/tests/test_cxr_tela_le_o_motor.py:130`, `app/ponto/tests/test_cxr_tela_le_o_motor.py:134`, `app/escala/services/cadastro_realidade.py:864`, `app/escala/services/cadastro_realidade.py:866`.

   O teste de conflito C9×C12 é mais forte contra essa chamada: a reconciliação ocorre fora dos `try/except` da lista, portanto uma chamada indevida nesse caminho propagaria a exceção. `app/ponto/tests/test_cxr_tela_le_o_motor.py:247`, `app/escala/services/cadastro_realidade.py:885`, `app/escala/services/cadastro_realidade.py:1026`.

3. **O teste intitulado “cxr_lavrado responde como o espelho” não chama `cxr_lavrado`.** Ele injeta manualmente `[DIA]` em um lado e um espelho artificial com o mesmo único dia no outro. Não alcança `cobertos`, partição por competência, filtro, ausência de cobertura ou conversão de `fim`. `app/ponto/tests/test_cxr_tela_le_o_motor.py:284`, `app/ponto/tests/test_cxr_tela_le_o_motor.py:286`, `app/ponto/tests/test_cxr_tela_le_o_motor.py:287`.

4. **Q4 exige distinguir igualdade do conjunto completo de igualdade de pertinência dos dias perguntados.** O conjunto completo não é igual em geral: `medir` descarta datas externas a `[ini,fim]`, mas `InsumosDoDono.cxr()` guarda todos os `dia` publicados. O próprio teste com agosto/outubro comprova a diferença de construção. `app/ponto/services/cxr_na_lista.py:88`, `app/ponto/management/commands/e6_oraculo.py:170`, `app/ponto/tests/test_cxr_tela_le_o_motor.py:226`.

   Entretanto, `um_dono_por_dia` escolhe `janela_atual(dia)`. Para um dia perguntado que pertence àquela janela, o filtro sozinho preserva a pertinência, supondo mesma publicação e cobertura correta; não sustenta um contraexemplo de dono apenas porque os conjuntos completos diferem. `app/escala/services/cadastro_realidade.py:1017`, `app/ponto/services/cxr_na_lista.py:88`, `app/ponto/services/cxr_na_lista.py:174`.

5. **`fim` tem dois significados na ligação C11→CXR.** C11 passa o final da janela cortado em ontem; `cxr_lavrado` interpreta o mês/ano desse parâmetro como a identidade da competência. Exemplo: em 22/10, a competência corrente 21/10–20/11 é cortada em 21/10, e a consulta busca `10/2026`, embora a lavra corrente tenha `11/2026`. `app/escala/services/cadastro_realidade.py:568`, `app/escala/services/cadastro_realidade.py:569`, `app/escala/services/cadastro_realidade.py:526`, `app/ponto/services/cxr_na_lista.py:173`.

   **Não atribuir mudança efetiva de dono C11 a esse defeito:** o beco só chama o juiz depois de verificar `folga_nao_definida=True`, e esse insumo já produz `CADASTRO` antes de CXR. Hoje o erro de competência fica mascarado por essa precedência. `app/escala/services/cadastro_realidade.py:518`, `app/escala/services/cadastro_realidade.py:531`, `app/ponto/management/commands/e6_oraculo.py:98`.

6. Fluxo de `dono_do_dia`:

   - C9: escrito pelo serviço do teto chamando `InsumosDoDono.dono`; formatador converte `dono` para `dono_do_dia`. `app/ponto/services/teto_na_lista.py:86`, `app/escala/services/cadastro_realidade.py:379`.
   - C10: escrito pelo censo, com os dias de fase da própria corrida injetados no mesmo portador; formatador lê `dono_do_dia` diretamente. `app/ponto/management/commands/censo_fase_12x36.py:287`, `app/ponto/management/commands/censo_fase_12x36.py:300`, `app/escala/services/cadastro_realidade.py:424`.
   - C11: escrito no request pelo beco chamando o mesmo portador; não é dono diário previamente lavrado. `app/escala/services/cadastro_realidade.py:531`.
   - C12: escrito na lavra usando o espelho da própria medição; formatador converte `dono` para `dono_do_dia`. `app/ponto/services/cxr_na_lista.py:92`, `app/ponto/services/cxr_na_lista.py:96`, `app/escala/services/cadastro_realidade.py:403`.
   - Em conflito, `um_dono_por_dia` pode sobrescrever todos esses valores: reaproveita C11 presente ou pergunta ao mesmo juiz com CXR lavrado. Assim, o `dono_do_dia` final significa “dono reconciliado pela lista”, não necessariamente “dono original daquele snapshot”. `app/escala/services/cadastro_realidade.py:1009`, `app/escala/services/cadastro_realidade.py:1026`, `app/escala/services/cadastro_realidade.py:1028`.
   - A apresentação deriva `dono`, ação e contadores; `dias_na_lista` e vigia leem o `dono_do_dia` reconciliado. `app/escala/services/cadastro_realidade.py:923`, `app/escala/services/cadastro_realidade.py:930`, `app/escala/services/cadastro_realidade.py:1063`, `app/chamados/services/vigia_fio.py:228`.

7. `_cxr`: inicializado como `None`; recebe conjunto ISO por injeção ou pela leitura do espelho e é lido por `dono`. `_sem_motor`: escrito no construtor e lido nas guardas de `cxr` e `dono`. `None` representa “não carregado/não coberto”; conjunto vazio representa “coberto, motor não publicou dia”; sem cobertura, somente o ramo residual `ESTRUTURA` vira `None`, enquanto outros insumos ainda podem decidir CADASTRO/BATIDA. `app/ponto/management/commands/e6_oraculo.py:133`, `app/ponto/management/commands/e6_oraculo.py:146`, `app/ponto/management/commands/e6_oraculo.py:148`, `app/ponto/management/commands/e6_oraculo.py:170`, `app/ponto/management/commands/e6_oraculo.py:189`.

8. **Relógios paralelos preexistentes permanecem na cadeia:** C2 lê `date.today()` e crava corte 21; a lista lê um instante próprio; PDF chamado pela view lê outro instante porque a view não o repassa. Caso concreto: pedido atravessando meia-noite pode medir C2 com data distinta e imprimir outro carimbo. Isso não é introduzido pelas linhas do diff, mas impede afirmar “um relógio por request” para a cadeia inteira. `app/escala/services/cadastro_realidade.py:252`, `app/escala/services/cadastro_realidade.py:253`, `app/escala/services/cadastro_realidade.py:794`, `app/escala/services/cadastro_realidade.py:1076`, `app/escala/views.py:489`.

9. **C12 não propaga ausência/erro/frescura da lavra para o produto.** `ultimo_lavrado` possui `sem_medida`, `medido_em`, `data_ref` e conserva `erros`, mas `_cxr_do_motor` só consome `itens`; a saída da lista publica metadados do previsto cego e esmeril, sem equivalente C12. Caso concreto: lavra inexistente e lavra válida com zero itens produzem a mesma ausência de C12; lavra parcial com erros também não ganha indicação própria. `app/ponto/services/cxr_na_lista.py:149`, `app/ponto/services/cxr_na_lista.py:151`, `app/escala/services/cadastro_realidade.py:393`, `app/escala/services/cadastro_realidade.py:970`, `app/escala/services/cadastro_realidade.py:975`.

10. Não identifiquei novo predicado concorrente de dono: a decisão continua em `dono_da_divergencia`, que recebe paridade, cadastro, fase e pertinência CXR. Os novos ramos mudam carregamento e ausência de informação, não repetem a regra CADASTRO/BATIDA. `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:110`, `app/ponto/management/commands/e6_oraculo.py:195`.

## 08/11 01:34 · sessao 58c2a164fdf2 · FIO-E0-V4-E1 · verificador · origem=Code

**PROCEDENTE: a garantia “exportada fica FORA e não é escrita” está incompleta.** Há caminho de carimbagem indireta após o commit, veto anterior à trava no migrador e exclusão de perdedoras abertas quando compartilham plano com uma exportada. (`app/chamados/signals.py:225`; `app/chamados/services/disputa_emissao.py:1838`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:294`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:285`)

A análise abaixo é estática. Execução dos testes, morte efetiva dos mutantes, publicação do DIFF/RELATO, estado das migrações e ocorrência desses caminhos em produção: **[nao verificado]**. [nao verificado]

**1(a). Quem define e quem chama os vetos — NAO PROCEDE haver outro juiz novo no helper acrescentado.**

O registro vivo aponta K6 para `competencia_entregue`; `JUIZES.md` declara que é consulta e remete ao registro vivo. O helper novo compõe autoridades existentes. (`app/core/juizes.py:312`; `app/docs/JUIZES.md:10`; `app/chamados/services/fio.py:1077`)

| Função definida | Chamadores diretos de produção |
|---|---|
| `fio.veto_de_competencia`, definição em `app/chamados/services/fio.py:1070` | `aplicar`, por plano e por silêncio: `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`, `:294`; classificação e migração: `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:257`, `:294`. |
| `vigia_fio.competencia_do_dia_entregue`, definição em `app/chamados/services/vigia_fio.py:822` | `_vivas_entre`: `app/chamados/models.py:364`; `_costura_nao_mata_o_fio`: `app/chamados/services/fio.py:776`; `veto_de_competencia`: `app/chamados/services/fio.py:1080`; `_dia_fora_do_alcance_c`: `app/chamados/reconciliador.py:744`; `limbo_vai_a_pauta`: `app/chamados/services/fabrica_da_celula.py:198`; `carimbos_sem_lastro_por_dono`: `app/chamados/services/vigia_fio.py:816`; `_cerca`: `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:115`. |
| `folha.export.competencia_entregue`, definição em `app/folha/export.py:556` | Chamadores discriminados na tabela seguinte. |

| Chamador direto de `competencia_entregue` | Local |
|---|---|
| `competencia_do_dia_entregue`, consulta sem memo e alimentação do memo | `app/chamados/services/vigia_fio.py:841`, `:843` |
| Backfill: relatório das projeções sem linha | `app/chamados/management/commands/backfill_decisoes_pergunta.py:399` |
| Backfill: `_exportada`, para agrupar/rotular o acervo | `app/chamados/management/commands/backfill_decisoes_pergunta.py:426` |
| `competencia_fechada_para_celula` | `app/folha/export.py:610` |
| `txt_vigente_x_gravado` | `app/folha/export.py:647` |
| `preview_exportacao` | `app/folha/views.py:354` |
| Ranking HE, `_carimbo` | `app/folha/services/ranking_he.py:89` |
| `marcos_da_competencia` | `app/ponto/services/fechamento.py:1530` |
| `competencias_a_lavrar` | `app/ponto/services/he_pendente_lavrado.py:223` |
| Espelho, `janela` | `app/ponto/services/esmeril_espelho.py:58` |
| `lavrar_status_competencia.handle` | `app/ponto/management/commands/lavrar_status_competencia.py:43` |
| Pauta folga não definida, `_rodar` | `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:93` |
| Pauta pago × devido, `_medir` | `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124` |

As três funções consultadas não gravam o estado de entrega: K6 lê `ExportacaoDominio`, aplica vigência e rejeita o vigente invalidado. Os escritores desse estado são `registrar` e `invalidar`. (`app/folha/export.py:580`; `app/folha/export.py:583`; `app/folha/models.py:158`; `app/folha/models.py:182`)

**Há outros caminhos que usam entrega para decidir atos sobre perguntas/chamados:**

| Caminho | Consequência |
|---|---|
| `_costura_nao_mata_o_fio` | A competência aberta participa da recusa de costurar pergunta viva em destino terminal; exportada passa por essa guarda. (`app/chamados/services/fio.py:776`; `app/chamados/services/fio.py:784`) |
| `_vivas_entre` | Exportadas são retiradas das perguntas que seguram o encerramento do chamado. (`app/chamados/models.py:364`; `app/chamados/models.py:383`) |
| `_dia_fora_do_alcance_c` | Entrega deixa a pergunta fora do alcance novo da reemissão/costura do reconciliador. (`app/chamados/reconciliador.py:744`; `app/chamados/reconciliador.py:755`) |
| `limbo_vai_a_pauta` | No limbo exportado, emite pauta e retorna antes de perguntar ao colaborador. (`app/chamados/services/fabrica_da_celula.py:181`; `app/chamados/services/fabrica_da_celula.py:198`) |

Esses caminhos leem o compositor existente; não são novas cópias do predicado K6. O backfill também mantém uma composição para **relatório**, mas a classificação autoritativa do apply vem da porta. (`app/chamados/services/vigia_fio.py:836`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:422`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:463`)

**1(b). Contadores e retorno de `aplicar`.**

| Dado | Quem escreve | Quem lê |
|---|---|---|
| `CONTADORES_DO_APPLY` | Declaração inclui os dois novos contadores. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:129`) | Dedup separa contadores de recusas; resumo do backfill percorre a tupla. (`app/chamados/management/commands/dedup_perguntas_fato.py:107`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:514`) |
| `exportada_fora` | Plano vetado acrescenta **todas as retratadas**; silêncio acrescenta seu PK; `_fora` ordena/deduplica. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:285`; `:298`; `:345`) | Renderizador do backfill, resumo, dedup e testes. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:378`; `:514`; `app/chamados/management/commands/dedup_perguntas_fato.py:121`; `app/chamados/tests/test_e1_v13d_exportada_fora.py:65`; `:80`; `:93`; `:103`; `:111`; `app/chamados/tests/test_e1_v6_horizonte.py:347`; `:355`) |
| `exportada_fora_diff` | Dicionários de retratação/silêncio; `_fora` conserva a primeira entrada por PK. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:286`; `:299`; `:340`) | Renderizadores do backfill/dedup e testes do conteúdo. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:372`; `app/chamados/management/commands/dedup_perguntas_fato.py:116`; `app/chamados/tests/test_e1_v13d_exportada_fora.py:67`; `:68`; `:81`; `:82`) |
| `sem_competencia_recusada` | `_recusas`; ramo `None` dos planos e dos silêncios. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:140`; `:283`; `:296`) | Impede escrita e provoca rollback geral; CLI/dedup traduzem para saída 3; teste força esse retorno. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:304`; `:327`; `:500`; `app/chamados/management/commands/dedup_perguntas_fato.py:110`; `app/chamados/tests/test_e1_v13d_exportada_fora.py:103`) |
| Antigo `exportada_recusada` | Não integra o contrato atual de retorno, que usa `_fora` e `_recusas`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:331`; `:333`) | Resta a referência histórica e a asserção de ausência da chave no teste novo. (`app/chamados/tests/test_e1_v13d_exportada_fora.py:6`; `:66`) |

O retorno preserva `exportada_fora` e seu DIFF mesmo quando outra recusa desfaz as escritas; os contadores de escrita voltam a zero. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:329`)

**PROCEDENTE — preservar no retorno não garante publicar:** dedup sai por recusa antes de renderizar o DIFF; o backfill também trata `horizonte_diverge` antes de chamar seu renderizador. (`app/chamados/management/commands/dedup_perguntas_fato.py:107`; `:116`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:494`; `:499`)

| Consumidor solicitado | Resultado |
|---|---|
| Backfill `handle` | DRY chama a porta em rollback e publica os FORA; apply chama a porta e traduz recusas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:484`; `:487`; `:493`; `:500`) |
| Dedup | Apply consome o retorno; seu DRY apenas conta o plano e não passa por `aplicar` nem publica essa classificação de exportadas. (`app/chamados/management/commands/dedup_perguntas_fato.py:91`; `:99`; `:105`; `:116`) |
| Migração 0052 | **NAO PROCEDE** como leitora desses contadores: apenas adiciona `batida_origem` anulável. (`app/chamados/migrations/0052_pergunta_batida_origem.py:17`) |
| Migração 0053 | **NAO PROCEDE** como leitora do retorno: conta colisões diretamente e recusa sua própria execução. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:36`) |
| `_migrar_sob_trava` | **NAO PROCEDE** como leitor de `aplicar`: consome `plano_de_retratacao`/`NAO_RETRATA`, preenche seu próprio `out` e chama a porta de retratação. (`app/chamados/services/fio.py:1568`; `:1574`; `:1590`) |
| ESTADO | O caminho demonstrado lê o carimbo de `vigiar_fio_morto`, com outro conjunto de contadores; não consome o retorno do apply. (`bin/gerar_estado.py:253`; `bin/fio_estado.py:65`; `app/chamados/management/commands/vigiar_fio_morto.py:88`; `:96`) Outra integração: **[nao verificado]**. |

**1(c). Campos e artefatos do migrador.**

| Dado | Quem escreve | Quem lê |
|---|---|---|
| `ln['fora']` | Inicialização; motivos de classificação; desconhecida/exportada. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:132`; `:158`; `:259`; `:261`) | Seleção migra/FORA, saída, partição da medida e guarda de classificação. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:196`; `:202`; `:224`; `:255`) |
| `ln['comp']` | String de competências formatadas, ordenadas por ano/mês. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:250`; `:254`) | Saídas MIGRA, FORA e EXPORTADA_FORA. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:199`; `:202`; `:264`) |
| `exportada_fora` | Texto calculado por `len(fora)` na classificação; não é o retorno do backfill. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:247`; `:262`; `:267`) | Testes de saída. (`app/chamados/tests/test_e1_v13d_exportada_fora.py:173`; `:187`; `:197`; `app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:204`) |
| `descarimbaria` / `desc` | `_descarimbaria` conta perguntas elegíveis que o juiz considera vivas; atribuição da medida comum. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:225`; `:324`; `:327`) | Impressão e bloqueio do apply. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:231`; `:234`) |
| `descarimbaria_exportada_fora` / `desc_exp` | Mesma função, alimentada pelas linhas classificadas como exportadas. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:224`; `:226`) | Impressão e testes; não participa do bloqueio. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:232`; `:234`; `app/chamados/tests/test_e1_v13d_exportada_fora.py:185`; `app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:207`) |
| Arquivo de reversão | Apply, antes do atomic; JSON exclusivo, permissão 600. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:216`; `:219`; `:276`; `:278`; `:281`) | `--reverter`, que lê o JSON, chama a porta, confere célula e desfaz tudo se houver falha. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:183`; `:340`; `:350`; `:355`; `:359`) |

**PROCEDENTE — classificação e resultado final podem divergir:** uma entrega detectada apenas no segundo veto entra em `recusas`, mas não atualiza `ln['fora']`. Portanto não entra no `exportada_fora` já publicado nem na partição `desc_exp`. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:267`; `:294`; `:296`; `:224`)

A reversão contém datas/células das candidatas iniciais, inclusive das depois recusadas; não contém fotografias dos efeitos sobre irmãs, disputas ou chamados. O arquivo também sobrevive ao rollback do banco, porque nasce antes do atomic. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:216`; `:219`; `:276`; `:294`)

**2. Exportada escrita ou aberta bloqueada por exportada — PROCEDENTE, com estes recortes.**

- **Escrita direta de retratação/silêncio:** quando o veto classifica o item como exportado, ele é retirado das listas que serão escritas. A exportação, sozinha, deixou de ser recusa global do apply. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:284`; `:291`; `:297`; `:303`; `:306`; `:312`)
- **Aberta bloqueada dentro de plano misto:** basta uma retratada exportada para `v=True`; todas as retratadas daquele plano ficam FORA, inclusive as abertas. É tudo-ou-nada **do plano**, embora não seja rollback do apply inteiro. (`app/chamados/services/fio.py:1080`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`; `:285`)
- **Exportada carimbada indiretamente:** retratar uma aberta salva a pergunta; o signal agenda fechamento de beco; depois do commit, o beco chama o resolvedor, que alcança outras perguntas e outras disputas abertas do colaborador sem veto de entrega nesses ramos. (`app/chamados/services/fio.py:1107`; `app/chamados/models.py:2993`; `app/chamados/signals.py:225`; `app/chamados/services/becos.py:54`; `app/chamados/services/disputa_emissao.py:1764`; `:1805`; `:1838`)
- **Continuação da cascata:** fechar a disputa também aciona a materialização de pendentes, que pode carimbar outras perguntas aprovadas. (`app/chamados/services/disputa_emissao.py:1865`; `app/chamados/signals.py:163`; `app/chamados/services/materializacao.py:1418`; `:1431`)
- **`data_evento` pela reversão:** `--reverter` desvia antes da classificação e chama `rechavear_data_evento` diretamente; essa porta trava e escreve sem consultar o veto de competência. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:183`; `:350`; `app/chamados/services/fio.py:947`; `:965`)

Um contraexemplo construtível para a cascata é: aberta escrita numa disputa com pai/destinos terminais, sem pergunta visível e fechável; irmã exportada morta para C6, ainda sem materialização e elegível ao resolvedor. A classificação de beco e a carimbagem permitem esse percurso. (`app/chamados/services/becos.py:25`; `:33`; `:45`; `:54`; `app/chamados/services/disputa_emissao.py:1724`; `:1744`; `:1764`) Existência desse arranjo em produção: **[nao verificado]**.

O laço de migração não descarimba diretamente: sua gravação altera data, célula e lavra. O descarimbo real continua na porta de julgamento, que pode ser acionada por cascatas e não contém separação por exportada nesse trecho. (`app/chamados/services/fio.py:959`; `:965`; `app/chamados/signals.py:203`; `app/chamados/services/fabrica_da_celula.py:175`; `app/chamados/services/relavra_via.py:238`; `:246`) Um descarimbo exportado efetivamente provocado por esta execução: **[nao verificado]**.

**3. Canônica e retratada em competências diferentes.**

| Situação | Resultado direto da porta |
|---|---|
| Canônica aberta; retratada exportada | Retratação fica FORA. A canônica não é salva por esse ato; a retratada permanece sem a nova marca de duplicata. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:281`; `:285`; `app/chamados/services/fio.py:1107`) |
| Canônica exportada; retratada aberta | A canônica não participa do veto. A aberta pode receber o carimbo de duplicata; o PK da canônica entra no motivo. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:281`; `:314`; `app/chamados/services/fio.py:1107`; `:1110`) |
| Canônica também candidata a silêncio | É um item independente. Se aberta, pode receber SILENCIO mesmo com a retratação exportada FORA; se exportada, seu silêncio fica FORA. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:293`; `:297`; `:307`) |

**NAO PROCEDE exigir a canônica no veto para impedir uma escrita direta dela pela retratação:** esse caminho só salva a perdedora. Também não registra uma `DecisaoPergunta` de retratação; o registro de SILENCIO é outro caminho. (`app/chamados/services/fio.py:1107`; `app/chamados/models.py:2985`; `:2993`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:307`)

**PROCEDENTE exigir proteção dos alvos indiretos:** incluir somente a canônica no veto ainda deixaria irmãs e outras disputas expostas à cascata. (`app/chamados/services/disputa_emissao.py:1805`; `:1813`; `:1838`)

**4. Veto novamente dentro do atomic e sob a trava.**

| Caminho | Veredito |
|---|---|
| Backfill | **PROCEDENTE a insuficiência da releitura:** o veto roda sob as travas de colaborador/disputas/perguntas, mas `_memo` é único para todo o apply. A segunda pergunta da mesma competência pode receber o resultado antigo sem nova consulta a K6. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:203`; `:215`; `:220`; `:281`; `:294`; `app/chamados/services/vigia_fio.py:842`) |
| Migrador | **NAO PROCEDE dizer que não repete:** repete com memo novo dentro do atomic da linha. **PROCEDENTE dizer que não é sob a trava:** o veto vem antes de chamar a porta que adquire a trava. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:257`; `:291`; `:294`; `:298`; `app/chamados/services/fio.py:947`) |
| Estado de entrega | **PROCEDENTE a janela:** K6 faz leitura comum; `registrar` insere a entrega sem compartilhar a trava do colaborador usada nesses caminhos. Não há nova consulta entre o veto e o save da migração. (`app/folha/export.py:581`; `app/folha/models.py:182`; `app/chamados/services/fio.py:965`) |

Além disso, o migrador pergunta sobre `ln['velho']` e a empresa capturados na classificação; a porta depois relê a pergunta e obtém seu dia efetivamente gravado, sem refazer o veto para essa releitura. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:294`; `app/chamados/services/fio.py:949`; `:950`)

**5. DRY do backfill e alcance do rollback.**

**NAO PROCEDE haver escrita durável no caminho ORM normal demonstrado:** o DRY envolve `aplicar` em atomic explicitamente desfeito; isso abrange decisão, pergunta, outbox síncrono e auditoria no banco padrão. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:484`; `:486`; `app/chamados/models.py:3748`; `:3225`; `app/chamados/signals.py:237`; `app/core/observ.py:133`)

| Efeito | Tratamento |
|---|---|
| `DecisaoPergunta` | `bulk_create` dentro da transação. (`app/chamados/models.py:3746`) |
| Carimbo da perdedora | Save transacional da pergunta. (`app/chamados/models.py:2993`; `:3225`) |
| `EventoCelula` | Signal síncrono chama a porta do evento; participa da transação. (`app/chamados/signals.py:244`; `app/chamados/models.py:3051`; `app/ponto/portas/celula.py:890`) |
| `LogAuditoria` | INSERT em atomic aninhado, sem commit independente no código mostrado. (`app/core/observ.py:133`) |
| Usuário sistema, se criado | `get_or_create` comum, sem alias explícito de outro banco. (`app/core/utils.py:18`) |
| Beco em `on_commit` | É agendado pelo save; não chega ao commit no DRY desfeito. (`app/chamados/signals.py:225`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:486`) |

**PROCEDENTE contra a frase absoluta “rollback desfaz tudo”:** falha da auditoria pode incrementar `_ENGOLIDOS` em memória e emitir ERROR imediatamente. O perfil saas encaminha esse ERROR a `errors.log`; transação de banco não desfaz esses efeitos. (`app/core/observ.py:75`; `:79`; `:105`; `app/config/settings/saas.py:139`; `:183`)

O perfil saas declara somente `default`; configuração efetivamente implantada, outro banco ou extensão externa não presente na cópia: **[nao verificado]**. (`app/config/settings/saas.py:83`; `:93`)

**6. O que mede `descarimbaria_exportada_fora`.**

**PROCEDENTE: não é DIFF causal da migração da exportada.** Ela não migrou; a função conta, no estado atual, carimbos elegíveis que C6 considera vivos. Mede o que **um ato posterior de descarimbo** derrubaria, não quanto esta migração alterou nessa linha. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:196`; `:226`; `:324`; `:327`; `app/c

[truncado em 20000 de 29916 caracteres]

## 08/11 01:28 · sessao ddc6f16190eb · - · - · origem=Ronald (tmux)

Achados para itens 2–5, por leitura estática:

1. **PROCEDENTE — veto do migrar ocorre antes da trava.** `_migrar` chama o veto dentro de `atomic`, mas só depois chama `rechavear_data_evento`; a trava Colaborador→Disputa→Pergunta acontece dentro dessa porta. Portanto existe intervalo entre consulta e aquisição da trava, e a porta relê `p`, mas não repete o veto para essa leitura. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:291`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:294`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:298`, `app/chamados/services/fio.py:947`, `app/chamados/services/fio.py:949`.

2. **PROCEDENTE — backfill não consulta entrega novamente para cada item.** Os alvos são relidos sob a trava, mas `_memo` é criado antes do atomic e compartilhado por todos os colaboradores/planos; o compositor devolve resposta guardada por empresa/mês/ano. O juiz de entrega faz consulta comum, sem trava de `ExportacaoDominio`. `app/chamados/management/commands/backfill_decisoes_pergunta.py:203`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:294`, `app/chamados/services/vigia_fio.py:839`, `app/chamados/services/vigia_fio.py:842`, `app/folha/export.py:581`.

3. **PROCEDENTE — há caminho indireto para carimbar outra pergunta exportada.** Retratar salva a perdedora; o signal agenda fechamento de beco após commit; esse fechamento chama o resolvedor, que alcança outras perguntas/disputas do colaborador e carimba elegíveis sem consultar entrega. Fechar disputa também dispara materialização de pendentes aprovadas, novamente sem esse veto. Trata-se de caminho demonstrável; existência de alvo real elegível em produção é **[nao verificado]**. `app/chamados/services/fio.py:1107`, `app/chamados/models.py:2993`, `app/chamados/signals.py:214`, `app/chamados/signals.py:225`, `app/chamados/services/becos.py:41`, `app/chamados/services/becos.py:54`, `app/chamados/services/disputa_emissao.py:1764`, `app/chamados/services/disputa_emissao.py:1805`, `app/chamados/services/disputa_emissao.py:1838`, `app/chamados/services/disputa_emissao.py:1865`, `app/chamados/signals.py:163`, `app/chamados/services/materializacao.py:1418`, `app/chamados/services/materializacao.py:1431`.

4. **Granularidade direta da canônica:**

| Situação | Resultado direto |
|---|---|
| Canônica aberta, retratada exportada | Plano fica FORA; retratada não recebe carimbo; canônica não é escrita pela retratação; não nasce Decisão por retratar. `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:285`, `app/chamados/services/fio.py:1107` |
| Canônica exportada, retratada aberta | Canônica não participa do veto; retratada pode ser carimbada duplicata; canônica só fornece pk/texto; não nasce Decisão por essa retratação. `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:314`, `app/chamados/services/fio.py:1107`, `app/chamados/services/fio.py:1110`, `app/chamados/models.py:2985`, `app/chamados/models.py:2993` |
| Canônica também candidata a SILENCIO | É item independente: veto por pk; aberta pode receber SILENCIO mesmo com retratada exportada fora; exportada fica fora. `app/chamados/management/commands/backfill_decisoes_pergunta.py:293`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:307` |
| Várias retratadas, uma exportada | Todas as retratadas do plano ficam FORA, inclusive abertas; isso é exclusão por plano, não rollback do apply inteiro. `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:285` |

Não é necessário incluir a canônica no veto para proteger uma escrita **direta** dela, pois a retratação não a salva; é necessário proteger todos os alvos indiretos das cascatas. Incluir só a canônica não cobre outras irmãs. `app/chamados/services/fio.py:1107`, `app/chamados/models.py:2993`, `app/chamados/services/disputa_emissao.py:1805`, `app/chamados/services/disputa_emissao.py:1838`.

5. **DRY: rollback cobre as escritas ORM observadas no caminho normal, mas “desfaz tudo que toca” é falso em sentido absoluto.** O atomic externo é explicitamente desfeito; SILENCIO usa `bulk_create`, retratação salva Pergunta, outbox cria `EventoCelula` sincronamente e auditoria cria `LogAuditoria` em savepoint aninhado. O callback de beco fica agendado, portanto não executa com esse rollback. `app/chamados/management/commands/backfill_decisoes_pergunta.py:484`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:486`, `app/chamados/models.py:3748`, `app/chamados/models.py:3225`, `app/chamados/signals.py:237`, `app/ponto/portas/celula.py:890`, `app/core/observ.py:133`, `app/chamados/signals.py:225`.

   Se auditoria falha, `engolir` altera contador Python e emite ERROR imediatamente; esse efeito não pertence à transação. O perfil saas encaminha o ERROR ao arquivo `errors.log`. A ocorrência concreta durante DRY é **[nao verificado]**. `app/core/observ.py:75`, `app/core/observ.py:79`, `app/core/observ.py:90`, `app/config/settings/saas.py:136`, `app/config/settings/saas.py:139`, `app/config/settings/saas.py:183`.

   `get_user_sistema` faz `get_or_create` comum, sem cache ou alias de outro banco; o perfil saas declara somente `default`. Configuração efetivamente implantada é **[nao verificado]**. `app/core/utils.py:18`, `app/config/settings/saas.py:83`, `app/config/settings/saas.py:93`.

6. **PROCEDENTE — ordem da transação com vários colaboradores.** `aplicar` mantém atomic único, trava C1→Disputa/Pergunta e só depois adquire C2. Ordena colaboradores por pk, mas não trava todos os colaboradores antes das tabelas seguintes, como exige a própria documentação da trava. Deadlock ocorrido é **[nao verificado]**. `app/chamados/management/commands/backfill_decisoes_pergunta.py:209`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:210`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:220`, `app/chamados/models.py:2564`.

7. **Testes deixam as cascatas após commit sem cobertura.** Os casos do backfill herdam `_Base(TestCase)` e verificam estado direto; não executam explicitamente os callbacks. A prova “a exportada não foi escrita” nesses casos não cobre a carimbagem indireta posterior ao commit. `app/chamados/tests/test_e1_v6_horizonte.py:63`, `app/chamados/tests/test_e1_v13d_exportada_fora.py:31`, `app/chamados/tests/test_e1_v13d_exportada_fora.py:58`, `app/chamados/tests/test_e1_v13d_exportada_fora.py:70`, `app/chamados/signals.py:225`.

Não executei testes; resultado executado e mutantes reais são **[nao verificado]**. [nao verificado]

## 08/11 01:28 · sessao 594aeb1122f1 · - · - · origem=Ronald (tmux)

**1(b) — tabela de escritores/leitores dos contadores**

| Campo/contrato | Quem escreve | Quem lê |
|---|---|---|
| `CONTADORES_DO_APPLY` | Tupla declarada em `app/chamados/management/commands/backfill_decisoes_pergunta.py:129` | Dedup distingue contador de recusa por essa tupla (`app/chamados/management/commands/dedup_perguntas_fato.py:104` e `:107`); resumo do backfill a percorre, excluindo `_diff` da contagem textual (`app/chamados/management/commands/backfill_decisoes_pergunta.py:514`). |
| `exportada_fora` | `aplicar` acumula todos os membros de um plano vetado em `app/chamados/management/commands/backfill_decisoes_pergunta.py:285`; acumula pk de silêncio em `:298`; `_fora` ordena e deduplica em `:338`–`:345`; integra retorno de sucesso e recusa em `:329`–`:335`. | Renderizador do backfill, `:370`–`:378`, chamado no DRY `:487` e apply `:499`; resumo `:514`; dedup `app/chamados/management/commands/dedup_perguntas_fato.py:121`; testes novos `app/chamados/tests/test_e1_v13d_exportada_fora.py:65`, `:80`, `:93`, `:103`, `:111`; testes anteriores adaptados `app/chamados/tests/test_e1_v6_horizonte.py:347` e `:355`. |
| `exportada_fora_diff` | Dicionários de retratação em `app/chamados/management/commands/backfill_decisoes_pergunta.py:286`, de silêncio em `:299`; `_fora` mantém a primeira entrada por pk em `:340`–`:345`; ambos os retornos carregam em `:332` e `:335`. | Renderizador `app/chamados/management/commands/backfill_decisoes_pergunta.py:372`; dedup `app/chamados/management/commands/dedup_perguntas_fato.py:116`; testes novos `app/chamados/tests/test_e1_v13d_exportada_fora.py:67`, `:68`, `:81`, `:82`; tupla impede classificá-lo como recusa (`backfill_decisoes_pergunta.py:131`, `dedup_perguntas_fato.py:107`). |
| `sem_competencia_recusada` | Lista inicial `app/chamados/management/commands/backfill_decisoes_pergunta.py:140`; retratação `:283`; silêncio `:296`; retorno `:332` ou lista vazia por `_recusas()` em `:335`. | `any(rec.values())` impede escrita/força rollback em `:304` e `:327`; CLI sai com 3 em `:500`–`:503`; dedup sai com 3 em `app/chamados/management/commands/dedup_perguntas_fato.py:110`; teste `app/chamados/tests/test_e1_v13d_exportada_fora.py:103`. |
| Antigo `exportada_recusada` | Na cópia atual, resta referência histórica na docstring do teste, não escritor do retorno (`app/chamados/tests/test_e1_v13d_exportada_fora.py:6`); ausência absoluta de referência dinâmica fora do censo textual [nao verificado]. | Teste exige ausência da chave em `app/chamados/tests/test_e1_v13d_exportada_fora.py:66`; leitores de produção atuais usam a substituta em `backfill_decisoes_pergunta.py:500` e `dedup_perguntas_fato.py:110`. |

**PROCEDENTE — o retorno preserva o FORA numa recusa, mas nem todo leitor o publica:** dedup levanta `SystemExit` antes do laço do DIFF (`app/chamados/management/commands/dedup_perguntas_fato.py:107`–`:116`); backfill faz o mesmo quando `horizonte_diverge` (`app/chamados/management/commands/backfill_decisoes_pergunta.py:494`–`:499`).

**Migrações e `_migrar_sob_trava`:**

- **NÃO PROCEDE como leitores desses contadores:** 0052 apenas adiciona `batida_origem` anulável (`app/chamados/migrations/0052_pergunta_batida_origem.py:17`–`:21`); 0053 mede colisões diretamente no acervo e recusa sua própria execução (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:36`–`:46`). `_migrar_sob_trava` lê as categorias de `plano_de_retratacao`/`NAO_RETRATA` e escreve seu próprio `out`, sem consumir o retorno de `aplicar` (`app/chamados/services/fio.py:1568`–`:1590`).
- **FILA — pré-requisito de 0053 continua aberto quando houver colisão exportada:** a migração não exclui exportada do índice nem de `_guarda` (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:31`–`:46`, `:55`–`:66`); o teste novo preserva duas linhas desse grupo (`app/chamados/tests/test_e1_v13d_exportada_fora.py:73`–`:80`). Existência dessa colisão na produção e qualquer pouso efetivo [nao verificado].
- **ESTADO não recebe esses novos contadores pelo caminho demonstrado:** seu fio lê `bin/fio_estado.py` (`bin/gerar_estado.py:253`–`:261`), cujo dado é o carimbo de `vigiar_fio_morto` (`bin/fio_estado.py:62`–`:65`); o contrato de esperados desse escritor não inclui os contadores do apply (`app/chamados/management/commands/vigiar_fio_morto.py:88`–`:99`). Outra integração fora desse caminho [nao verificado].

**7 — selos/RED: leitura estática de cada teste novo; execução e mutantes mortos [nao verificado].** [nao verificado]

| Teste, em `app/chamados/tests/test_e1_v13d_exportada_fora.py` | Julgamento da mordida demonstrada |
|---|---|
| `:58` aberta aplica/exportada fora | **NÃO PROCEDE** selo vazio: exige decisão na aberta, nenhuma decisão na exportada, pk e ato no DIFF (`:62`–`:68`). Não compara todos os campos das perguntas (`:62`–`:68`). |
| `:70` colisão exportada + silêncio aberto | **NÃO PROCEDE** selo vazio: exige silêncio aberto e ambas as vias exportadas preservadas, além da canônica no DIFF (`:77`–`:82`). Não cobre plano com perdedoras em competências distintas (`:73`–`:74`). |
| `:84` mérito | **NÃO PROCEDE** selo vazio: exige recusa com zero silêncio e ausência de decisão na aberta (`:91`–`:94`). O teste declara todas as competências não exportadas pelo mock (`:91`). | [nao verificado]
| `:96` sem dia | **PROCEDENTE** alcance menor que o nome: usa pergunta com data `ABERTA` e força `veto=None`; testa a tradução de `None`, não o trajeto real de pergunta sem dia (`:99`–`:104`). |
| `:106` idempotência | **NÃO PROCEDE** para duplicação de decisão: verifica segunda escrita zero e uma decisão total (`:109`–`:112`); igualdade do estado completo [nao verificado]. |
| `:117` CLI apply | **NÃO PROCEDE** para comportamento principal: exige aberta escrita, exportada sem decisão, contador/DIFF e ausência de recusa (`:121`–`:127`). |
| `:129` CLI DRY | **PROCEDENTE** para pretensão “não escreve tudo”: observa somente `DecisaoPergunta`, não auditoria, outbox, usuário de sistema, arquivos ou logs (`:133`–`:138`). |
| `:140` dedup | **NÃO PROCEDE** para tudo-ou-nada antigo: exige retratação aberta, duas vias exportadas preservadas e contador (`:145`–`:148`). |
| `:164` migrar mista | **NÃO PROCEDE** para tudo-ou-nada antigo: compara datas de duas abertas e da exportada e exige DIFF/contador (`:167`–`:174`). Imutabilidade completa da exportada não é testada (`:170`). |
| `:176` materializada exportada | **NÃO PROCEDE** para trava global pelo descarimbo: exige `descarimbaria=0`, medida separada 1, aberta migrada e datas exportadas intactas (`:183`–`:191`). Não compara campos de carimbo (`:190`–`:191`). |
| `:193` migrar DRY | **PROCEDENTE** para a abrangência anunciada: confere somente a data de `p_mat` e saída (`:196`–`:198`). O teste anterior é mais amplo: compara fotografias de todas as perguntas, totais de batidas/auditoria e ausência de arquivo (`app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:136`–`:147`). |
| `:200` veto único | **PROCEDENTE** selo insuficiente para unicidade e trava: exige apenas cinco chamadas (`:203`–`:207`); deslocar chamadas para fora da trava ou acrescentar decisão paralela pode preservar esse número [nao verificado por mutante]. |
| `:209` virou exportada | **NÃO PROCEDE** selo vazio para classificação→linha: altera a resposta após terminar `_competencias`, exige recusa e datas preservadas (`:216`–`:224`). Não exercita mudança depois do veto e antes da trava/escrita, pois a bandeira já está alterada antes de `_migrar` (`:218`–`:221`). |

**Relógio — PROCEDENTE no trajeto exercitado pelos testes novos de backfill:** `_Caso` herda `_Base` sem congelamento declarado nessas classes (`app/chamados/tests/test_e1_v13d_exportada_fora.py:32`; `app/chamados/tests/test_e1_v6_horizonte.py:63`–`:80`); `_sem_ato` chama `fechar_por_admin` sem instante (`app/chamados/tests/test_e1_v13d_exportada_fora.py:38`–`:40`), e essa porta lê `_tz.now()` (`app/chamados/models.py:2867`). Os testes chamam `aplicar` sem `agora` (`app/chamados/tests/test_e1_v13d_exportada_fora.py:61`, `:76`, `:145`); a retratação recebe `None` (`app/chamados/management/commands/backfill_decisoes_pergunta.py:314`) e lê `timezone.now()` no service (`app/chamados/services/fio.py:1100`–`:1101`). A classe de migração está congelada explicitamente (`app/chamados/tests/test_e1_v13d_exportada_fora.py:151`). Esses relógios de produção são anteriores ao diff fornecido; serem introduzidos por este commit [nao verificado].

**Segundo escritor — NÃO PROCEDE no ato específico “retratar duplicata”:** `retratar_para_canonica` reaplica o plano e é quem chama `retratar_duplicata` (`app/chamados/services/fio.py:1083`–`:1090`); backfill e `_migrar_sob_trava` passam por ela (`app/chamados/management/commands/backfill_decisoes_pergunta.py:314`; `app/chamados/services/fio.py:1590`). A catraca declara e testa essa exclusividade, com limites explícitos de formas indiretas/SQL (`app/chamados/tests/test_e1_v7_catracas.py:16`–`:18`, `:39`–`:44`, `:309`–`:347`).

**Juiz registrado — NÃO PROCEDE para nascimento de outro juiz de “entregue”:** K6 aponta a `folha/export.py::competencia_entregue` (`app/core/juizes.py:303`–`:312`); o helper novo chama `competencia_do_dia_entregue`, sem consultar exportação diretamente (`app/chamados/services/fio.py:1077`–`:1080`). Censo de todos os caminhos alternativos fica com o agente principal [nao verificado por este subagente].

## 08/11 01:28 · sessao 93b692bdbfb0 · - · - · origem=Ronald (tmux)

Achados principais — somente leitura; ocorrência real em produção [nao verificado].

| Tema | Veredito | Evidência |
|---|---|---|
| Veto novamente dentro do atomic | **NAO PROCEDE** dizer que não repete: classificação chama em `migrar_data_evento_dia_do_turno.py:257`; `_migrar` repete com memo novo em `:291-298`. |
| Veto sob a trava | **PROCEDENTE**: o veto ocorre em `:294`; a trava só vem dentro de `rechavear_data_evento`, em `fio.py:947-949`. Atomic não equivale a trava. |
| Janela exportar → escrever | **PROCEDENTE**: a consulta entregue é SELECT sem trava em `folha/export.py:581-586`; entre o veto `migrar...py:294` e o save `fio.py:965` não há nova consulta. Também se verifica `ln['velho']`/empresa da classificação, enquanto a porta relê a pergunta depois, em `fio.py:949-950`. |
| Reversão escreve exportada | **PROCEDENTE**: `--reverter` desvia antes da classificação em `migrar...py:183-184`, lê JSON em `:340-341`, chama diretamente a porta em `:350-351`; `rechavear_data_evento` não consulta veto no corpo `fio.py:941-970`. |
| Escrita indireta após commit | **PROCEDENTE**, como caminho construtível: save `fio.py:965` agenda `becos` por `on_commit` em `signals.py:214-225`; `becos.py:45-54` varre destinos das irmãs e chama o resolver; este carimba sem veto de exportação em `disputa_emissao.py:1724-1765`, e varre outras disputas abertas do colaborador em `:1805-1838`. | [nao verificado]
| `descarimbaria_exportada_fora` é DIFF da migração | **PROCEDENTE** a objeção: não é diferença antes/depois da migração. A exportada não migra (`migrar...py:196`); a medida relê estado atual e conta carimbo elegível que o juiz diz vivo (`:309-333`). Mede um eventual **ato de descarimbo** separado. |
| Cobertura completa por `desc` | **PROCEDENTE** a insuficiência: mede antes do commit (`:219-238`), portanto antes dos callbacks `signals.py:225`; mede só dias velho/novo dos registros recebidos (`:317-324`), enquanto o resolver pode carimbar outras perguntas/disputas (`disputa_emissao.py:1805-1838`). | [nao verificado]
| Exclusão automática de candidato compartilhado | **NAO PROCEDE**: a partição é de linhas de entrada, mas a query mede perguntas por `(colaborador,dia)`, não por PK (`:317-324`). Se uma pergunta cai no recorte aberto e no exportado, pode aparecer em ambos os números; não se garante disjunção. |
| Ordem de trava por transação inteira | **PROCEDENTE**: atomic externo em `:219`, loop em `:289`, cada porta trava um colaborador em `fio.py:947`; entrada ordenada por PK de pergunta (`:189`), não PK de colaborador. Não há trava inicial do conjunto, embora `models.py:2564-2576` preveja esse caso. Reversão repete o padrão (`:344-350`). |

Contraexemplo indireto construtível: pergunta aberta migra numa disputa cujo pai e destinos são terminais, sem perguntas visíveis ao colaborador, e cuja irmã exportada está morta para C6, sem materialização e sem tranca. O save agenda `fechar_beco_da_disputa`; após commit, a classificação de beco passa (`becos.py:25-33`), `fechar_beco` chama resolver para os destinos (`:45-54`), e a irmã exportada pode receber carimbo (`disputa_emissao.py:1724-1765`). Não há filtro de competência exportada nesse caminho; existência desse arranjo em produção [nao verificado]. [nao verificado]

Tabela completa local dos campos:

| Dado | Escritores | Leitores |
|---|---|---|
| `ln['fora']` | Inicial `None`: `migrar...py:132-133`; motivos de fonte/ambiguidade/colisão/aval: `:151-158`; sem competência/exportada: `:257-262`. | Seleção migra/fora: `:196-197`; saída: `:201-204`; partição `desc`: `:224-226`; evita reclassificação: `:255-256`. |
| `ln['comp']` | String `MM/AAAA[,MM/AAAA]`, derivada só para rótulo: `:250-254`. | Saída MIGRA `:199-200`, FORA `:202-203`, EXPORTADA_FORA `:264-266`. |
| `exportada_fora` do migrar | Não é retorno de `aplicar`: é texto calculado de `len(fora)` local em `_competencias`, `:247`, `:262`, `:267-268`. | Testes: `test_e1_v13d_exportada_fora.py:173`, `:187`, `:197`; `test_migrar_data_evento_dia_do_turno.py:204`. | [nao verificado]
| `desc` / `descarimbaria` | `_descarimbaria` incrementa por pergunta viva elegível: `:324-329`; resultado atribuído em `:225`. | Saída `:231`; bloqueio e rollback `:234-238`. |
| `desc_exp` / `descarimbaria_exportada_fora` | Resultado separado em `:226`, mesma função `:309-333`. | Apenas saída em `:232`; testes `test_e1_v13d_exportada_fora.py:184-186`, `test_migrar_data_evento_dia_do_turno.py:206-207`; não bloqueia o apply (`migrar...py:234`). | [nao verificado]
| Arquivo de reversão | Só apply autorizado localmente e havendo migra: `:215-218`; pasta/nome/JSON/permissão: `:270-282`. | `--reverter`: `:183-184`, `:335-363`; teste conteúdo/permissão e restauração: `test_migrar_data_evento_dia_do_turno.py:162-174`. | [nao verificado]

Detalhes do arquivo: é escrito **antes** do atomic de migração (`:216-219`), por isso não é desfeito pelo rollback. Inclui todas as linhas classificadas em `migra`, mesmo as depois recusadas pelo segundo veto (`:276-280`, `:294-297`). Guarda apenas PK, colaborador rotulado, data antiga, célula antiga e novo dia (`:276-277`); não guarda efeitos posteriores em irmãs, disputas, chamados, carimbos ou novas auditorias. A reversão devolve data pela porta e confere célula (`:350-356`), com rollback geral se falhar (`:359-360`).

A exportação que surgir entre classificação e segundo veto vira `RECUSA`, mas não atualiza `ln['fora']` (`:294-297`): o `exportada_fora` publicado anteriormente (`:267-268`) e o agrupamento `desc_exp` (`:224-226`) continuam refletindo a classificação inicial. [nao verificado]

O caminho direto da porta altera data, célula e lavra (`fio.py:959-965`), emite evento para célula velha (`:966-967`) e auditoria (`:968-970`); não altera diretamente o carimbo nessa gravação. Contudo, o save dispara os callbacks e o outbox (`signals.py:214-225`, `:237-244`); o outbox inclui dia/célula escritos (`models.py:3023-3025`) e é síncrono (`signals.py:232-244`).

Sobre semântica: `comp` local do migrar é string declarada e usada só como rótulo (`:244`, `:254`, `:264-266`); o fato de outro dict de outro fluxo usar tupla não prova colisão semântica por si só. `fora` é um discriminador de motivos, incluindo texto detalhado e sentinela exata `competencia_exportada` (`:158`, `:259-261`); a igualdade textual controla a partição da proteção em `:224-226`, portanto mudança de motivo sem migração desse leitor seria relevante.

_2839 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
