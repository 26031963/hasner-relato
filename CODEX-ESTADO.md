# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 09:47** · publicado em **18/11/2026 09:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 09:47 · sessao 878c5682bc91 · [nome]-NAO-DESFAZ-O-PAPEL · verificador · origem=Code

1. **A cura não cobre toda entrada malformada.** `_eh_hhmm` filtra `None`, `''`, `·S` e os rótulos posicionais, mas aceita caracteres para os quais `isdigit()` é verdadeiro e `int()` falha: com marco `²²:00`, célula `missing` e órfã `11:31`, o contador levanta `ValueError`. Também aceita `24:00` e `99:99`; estes não levantam exceção, mas entram no cálculo circular como horas válidas e podem gerar contagens falsas. [app/escala/orfa_classe.py:31](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/orfa_classe.py:31), [app/escala/orfa_classe.py:125](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/orfa_classe.py:125), [app/escala/orfa_classe.py:130](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/orfa_classe.py:130).

2. **Órfãs malformadas ainda podem derrubar o classificador e o contador.** Em dia de trabalho com célula `missing` e marco válido, órfã `·S` causa `ValueError` e órfã `None` causa `TypeError`, pois a classificação chama `_minutos` sem validar a órfã; o contador também ordena órfãs por `_minutos`. A lista de células mais curta que a régua e uma célula `None` são ignoradas com segurança nesse trecho. Os emissores P1/P2/P3 encontrados formatam horários como `HH:MM` ou copiam a hora de uma célula; não há evidência no dado raspado de órfã malformada em produção. [app/escala/orfa_classe.py:53](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/orfa_classe.py:53), [app/escala/orfa_classe.py:147](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/orfa_classe.py:147), [app/escala/utils.py:631](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/utils.py:631), [app/escala/utils.py:1730](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/utils.py:1730), [app/escala/utils.py:1841](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/utils.py:1841). Ocorrência em produção: **[não verificado]**.

3. **O limite (i) exagera ao dizer que a distância circular é “certa para P1”.** P1 é associado ao dia usado para montar o turno em ambos os caminhos examinados, mas isso não recupera a data dos instantes: dentro de um turno longo, `00:10` e `23:30` podem estar separados por 23h20, enquanto o contador calcula 40 minutos. Logo, o rótulo do dia do turno, por si só, não garante distância correta nem impede falso alarme P1. A ocorrência desse cenário nos dados de produção é **[não verificado]**. [app/escala/orfa_classe.py:89](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/orfa_classe.py:89), [app/escala/orfa_classe.py:130](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/orfa_classe.py:130), [app/escala/utils.py:784](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/utils.py:784), [app/escala/utils.py:1712](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/utils.py:1712), [app/escala/utils.py:1768](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/utils.py:1768).

4. **O limite (i) também generaliza incorretamente P3 como “data civil”.** Sobras P3 são atribuídas pela data civil, mas P3 de ponta de override e excedente de slots é acrescentada à lista de órfãs do turno antes de esta ser guardada em `orfas_por_data[d]`. Os limites (ii) e (iii) correspondem ao tratamento examinado: colunas posicionais não têm hora prevista na régua e uma ponta de override sem coluna não recebe célula nessa régua. [app/escala/orfa_classe.py:90](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/orfa_classe.py:90), [app/escala/utils.py:1401](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/utils.py:1401), [app/escala/utils.py:1719](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/utils.py:1719), [app/escala/utils.py:1730](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/utils.py:1730), [app/escala/utils.py:1769](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/utils.py:1769), [app/escala/utils.py:1827](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/utils.py:1827).

5. **A triagem do achado 9 é sustentada pelo teste, com uma ressalva de execução.** `test_com_a_cura_p1_e_zero` exige P1 zero nos dois valores da flag; o teste seguinte usa o mesmo cenário e simula a reversão fazendo `_protegida_da_guarda` retornar `False`, exigindo P1 maior que zero em ambos. Portanto, se esse teste de mutação passar nos dois modos, a asserção de P1 zero falha nos dois com essa reversão. Não executei os testes de integração nesta análise somente de leitura e sem banco; o resultado efetivo em cada modo é **[não verificado]**. [app/escala/tests/test_contador_espurias.py:155](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/tests/test_contador_espurias.py:155), [app/escala/tests/test_contador_espurias.py:163](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/tests/test_contador_espurias.py:163), [app/escala/tests/test_contador_espurias.py:169](/home/ronald/codex_ws/ae3051f9937386873a974a8993a2124b2d19dcb3/app/escala/tests/test_contador_espurias.py:169).

## 18/11 09:45 · sessao 6f2e6e03dc61 · [nome]-NAO-DESFAZ-O-PAPEL · verificador · origem=Code

1. **Procedente — o contador pode lançar exceção em uma régua válida.** O builder por turno cria colunas posicionais de intermitente com `hora='·I1'`, `·I2` etc. e pode marcar essas posições como `missing` ([utils.py:1402](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1402), [utils.py:1553](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1553)). Se houver órfã no mesmo dia, o contador aceita o rótulo como hora e tenta convertê-lo em inteiro ([orfa_classe.py:31](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:31), [orfa_classe.py:135](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:135)). Reproduzi `ValueError` com uma entrada pura que contém `·I1`, `missing` e uma órfã; a frequência dessa combinação em produção é **[não verificado]**.

2. **Procedente — 90 minutos circulares não são a regra de casamento.** O contador usa sempre 90 minutos e reduz as horas a um relógio circular de 24 horas ([orfa_classe.py:100](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:100), [orfa_classe.py:118](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:118)). No caminho DP, `tol_min` delimita o **envelope**, enquanto o alinhamento compara datetimes e não impõe raio de 90 por marco; o caminho por papel tem outra regra de atribuição ([utils.py:216](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:216), [utils.py:511](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:511), [utils.py:635](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:635)). Os builders também aceitam `tol_min` diferente de 90 ([utils.py:2095](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:2095)).

   Contraexemplo de data: uma sobra **P3 de 00:10 no dia civil D** e um marco vazio **23:30 de D** distam 1.400 minutos como instantes, mas o contador as considera a 40 minutos; reproduzi a contagem sobre essa entrada ([utils.py:1825](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1825), [orfa_classe.py:142](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:142)). Em um turno que realmente cruza meia-noite, 23:30 em D e 00:10 em D+1 também dão 40 minutos: o HH:MM isolado não distingue os dois casos ([utils.py:1414](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1414)).

3. **Procedente — `dia['orfas']` tem semânticas de data diferentes.** P1 pode pertencer ao dia de entrada do turno; as sobras P2 e P3 são atribuídas pela data civil local ([utils.py:694](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:694), [utils.py:801](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:801), [utils.py:1449](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1449), [utils.py:1825](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1825)). Outro leitor declara que a órfã pertence à data do dia, enquanto usa deslocamentos explícitos para células noturnas ([turnos.py:2658](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/ponto/turnos.py:2658)). Portanto, **não** está demonstrado um significado único de dia para todos os itens. O produtor indica o sítio, mas não recupera a data perdida ([orfa_classe.py:103](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:103)).

4. **Dúvida — alinhamento por índice é intenção do builder, mas não foi provado universalmente.** O builder simples alinha por `(tipo, HH:MM)`; o legacy multi-vínculo realinha cada subgrade à união; o por turno também alinha por chave e usa slots para intervalos e posições de intermitente ([utils.py:788](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:788), [utils.py:1034](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1034), [utils.py:1573](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1573), [utils.py:1715](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1715)). Nos overrides sem coluna, uma ponta casada vira P3 e um marco vazio sem coluna não aparece como célula alinhada ([utils.py:1724](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1724)). Assim, mesmo onde o índice aponta para a coluna correta, **a régua não necessariamente contém todos os marcos efetivos do dia**. Invariante para todo builder e dado possível: **[não verificado]**.

5. **Dúvida — o “esperado 0” pode alarmar por resíduos fora da cura.** O contador agrega todo P1 próximo de vazio, sem distinguir dono ou a classe executável dos 37 dias ([orfa_classe.py:148](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:148)). O anexo `lei.md` informa que a esteira segue com P1 inteiro e que resíduos de cadastro e batida podem permanecer; esses números e sua classificação não são verificáveis pelo código ou por banco/log nesta sessão: **[não verificado]**. Isso é uma limitação declarada do indicador, não prova de decisão indevida.

6. **Censo de emissores e transporte — nenhum outro emissor confirmado, com limite de verificação.** Os sete sítios do diff estão carimbados: P1 no papel e no DP, P2 na sobra civil e P3 nos quatro ramos por turno ([utils.py:631](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:631), [utils.py:673](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:673), [utils.py:810](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:810), [utils.py:1733](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1733), [utils.py:1744](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1744), [utils.py:1843](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1843)). A ordenação e as cópias rasas preservam os objetos; overlays podem remover a lista, e a reconciliação remove um item igual pelo texto ([utils.py:814](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:814), [utils.py:1822](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1822), [utils.py:1852](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1852), [utils.py:2014](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:2014)). Não identifiquei recriação de item antes dos retornos inspecionados; a exaustividade para **qualquer módulo e transporte indireto** permanece **[não verificado]**.

7. **Leitores e efeito do carimbo — nenhuma mudança decisória confirmada; censo exaustivo [não verificado].** `Orfa` herda `str` e não sobrescreve igualdade, hash ou representação ([orfa_classe.py:103](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:103)). Os leitores inspecionados incluem:

   - Cartório: aceita `isinstance(h, str)` para lâmpadas e grava `str(h)` na ata, descartando o atributo ([cartorio.py:628](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/ponto/services/cartorio.py:628), [cartorio.py:730](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/ponto/services/cartorio.py:730)).
   - Propositor, supra-juiz e folha: examinam horas, presença ou quantidade de órfãs, sem ler `produtor` nos trechos consultados ([propositor.py:24](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/ponto/propositor.py:24), [supra_juiz.py:527](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/ponto/supra_juiz.py:527), [causas.py:43](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/folha/causas.py:43)).
   - Calendário e PDF usam conjuntos e pertinência por hora; o template imprime o texto ([calendario.py:409](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/colaboradores/services/calendario.py:409), [pdf_espelho.py:174](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/relatorios/pdf_espelho.py:174), [espelho.html:240](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/templates/ponto/espelho.html:240)).
   - O SHA-256 do cartório usa partes de insumos, não a lista de órfãs inspecionada ([cartorio.py:471](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/ponto/services/cartorio.py:471), [cartorio.py:556](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/ponto/services/cartorio.py:556)). O teste cobre igualdade, chave de dicionário, JSON, pickle e `str`, mas não prova todos os leitores, `copy`/`deepcopy`, cache ou comparações exatas de tipo ([test_contador_espurias.py:92](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/tests/test_contador_espurias.py:92)).

   Na busca pelos usos do novo contador e de `produtor`, não encontrei chamada de produção ao contador nem leitura do atributo por dinheiro, ata, lâmpada, chamado ou pergunta; a conclusão global **“nenhum caminho existe” permanece [não verificado]** ([orfa_classe.py:144](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:144), [orfa_classe.py:148](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:148)).

8. **Checklist das classes de reprovação.**

   - **Segundo juiz:** **não identificado** para a pergunta de atribuição; o contador lê `missing`, enquanto `_match_marcos` é o juiz registrado ([core/juizes.py:221](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/core/juizes.py:221), [orfa_classe.py:135](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:135)). A classificação de proximidade é, porém, cálculo próprio do vigia; sua equivalência ao casador é **não**, conforme o achado 2.
   - **Segundo escritor fora da porta:** **não identificado** no contador, que constrói e retorna dados novos; cobertura global **[não verificado]** ([orfa_classe.py:125](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:125)).
   - **Relógio fora da borda:** **não** no contador inspecionado; ele recebe a grade ([orfa_classe.py:148](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:148)).
   - **Derivação paralela:** **sim**, quanto à distância circular e ao raio fixo; se isso viola a lei da obra para um recorte de vigia distinto da atribuição, **[não verificado]** ([orfa_classe.py:118](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/orfa_classe.py:118), [utils.py:216](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:216)).
   - **Campo com dois significados:** **sim**, órfã de dia do turno versus sobra de dia civil, como no achado 3; `marcos[].hora` também pode conter rótulo posicional ([utils.py:1404](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/utils.py:1404)).
   - **Pouso antes do pré-requisito:** **[não verificado]**; a cópia não contém histórico Git, e não há evidência executável aqui da ordem de pouso. [nao verificado]

9. **Testes — há mordidas úteis, mas não uma prova completa.** Os testes puros cobrem P1, separação P2, borda 90/91, marco cheio, folga, rajada, ausência de produtor e compatibilidade básica de `str`/JSON/pickle ([test_contador_espurias.py:48](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/tests/test_contador_espurias.py:48)). Os testes de builder verificam o golden, simulam a cura revertida e verificam P2 e presença de carimbo em um cenário ([test_contador_espurias.py:153](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/tests/test_contador_espurias.py:153), [test_contador_espurias.py:173](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/tests/test_contador_espurias.py:173)). Os testes puros **passariam independentemente da reversão da cura**, pois não chamam o builder; o teste explicitamente nomeado como reversão espera resultado positivo com `mock` e, sozinho, também não exige a cura ativa ([test_contador_espurias.py:159](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/tests/test_contador_espurias.py:159)). A mordida conjunta efetiva contra a reversão e a cobertura de cada ramo P3 são **[não verificado]**: não executei a suíte nem mutantes. Os casos de régua posicional, meia-noite ambígua e `tol_min` alternativo não aparecem nesses testes ([test_contador_espurias.py:25](/home/ronald/codex_ws/2255b3cada54a137bfad7074e9eec06a0eee4a8c/app/escala/tests/test_contador_espurias.py:25)).

A inspeção foi somente de leitura. O diff foi conferido contra o anexo fornecido; a identidade do pai e do HEAD por Git é **[não verificado]**, pois esta cópia não contém `.git`.

## 18/11 09:44 · sessao d8d98fb95e44 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO-RESIDUO · verificador · origem=Code

**Veredito: o delta não demonstra uma ordem de trava segura.** Há uma inversão identificável entre transações que alcançam as mesmas células e folgas: a sincronização do calendário adquire primeiro células e depois escreve folgas; a geração mensal da foto escreve folgas e depois tenta relavrar células. Isso é um **risco de deadlock**, condicionado à sobreposição das linhas e à intercalação das transações; ocorrência em produção está **[não verificado]**. [escala/folgas.py:150](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:150), [escala/folgas.py:193](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:193), [escala/folgas.py:215](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:215), [escala/folgas.py:617](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:617).

## 1. Escritores e ordem de trava

Os seguintes são os sítios **identificados** na cópia. Não posso afirmar que sejam todos os efeitos indiretos de aliases, cascatas e chamadores externos: exaustividade **[não verificado]**.

| Sítio de produção | Aquisição ou escrita identificada |
|---|---|
| Alcance futuro | `travar=True` prepara `select_for_update` de `CelulaDia`; a aquisição ocorre quando o queryset é avaliado pelos chamadores. [cadastro_futuro.py:19](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:19), [cadastro_futuro.py:48](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:48), [folgas.py:620](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:620). |
| Regeneração de vínculo | Lê células sem `select_for_update`, percorre datas crescentes e adquire travas de escrita nas linhas que altera ou cria. Não há trava explícita dia a dia nessa função. [celula.py:225](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:225), [celula.py:265](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:265), [celula.py:279](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:279), [celula.py:293](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:293), [celula.py:310](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:310). |
| Edição e troca de plantão | `editar_dia` trava uma célula explicitamente e cria ou salva; a troca ordena os dois lados por `(colaborador_id, data)` antes de chamar a edição. [celula.py:630](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:630), [celula.py:690](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:690). |
| Outras operações da porta | Reversão salva a célula; geração de período cria ou salva; `salvar_e_emitir` salva para lavra e carimbos; carimbo de insumos usa `bulk_update`. [celula.py:826](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:826), [celula.py:914](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:914), [celula.py:957](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:957), [celula.py:1220](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:1220). |
| Foto mensal de folgas | `gerar_foto_folgas` abre `atomic`; o escritor apaga a foto de calendário, faz `bulk_create` e chama a relavra, que alcança a regeneração futura. A ordem é **FolgaDia → CelulaDia** quando ambas são tocadas. [folgas.py:150](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:150), [folgas.py:180](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:180), [folgas.py:215](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:215), [folgas.py:290](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:290). |
| Folga manual | Lançamento usa `get_or_create`; alteração e lote chamam `_foto_do_dia`, que pode apagar, criar ou salvar; depois relavram células no mesmo ato. O reparentamento também salva a folga antes da relavra. [folgas.py:332](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:332), [folgas.py:343](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:343), [folgas.py:377](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:377), [folgas.py:405](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:405), [folgas.py:513](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:513). |
| Salvar calendário | Um `atomic` envolve todos os vínculos; para cada um, a sincronização trava células futuras e depois reconcilia `FolgaDia`: **CelulaDia → FolgaDia**. [folgas.py:571](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:571), [folgas.py:600](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:600), [folgas.py:617](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:617). |
| Comando gerador | O comando envolve medição, regeneração de hoje e geração de período num `atomic`. A primeira percorre colaboradores ordenados, mas a geração de período percorre vínculos sem ordenar por `(colaborador_id, data)` globalmente. [gerar_celulas.py:319](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/management/commands/gerar_celulas.py:319), [gerar_celulas.py:345](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/management/commands/gerar_celulas.py:345), [celula.py:883](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:883). |
| Restauração de snapshot | Trava vínculos por PK, depois células e apaga vínculos. A exclusão alcança `FolgaDia` por `CASCADE` e a geradora de `CelulaDia` por `SET_NULL`. [vinculo.py:1017](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/colaboradores/services/vinculo.py:1017), [models.py:1585](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/models.py:1585), [models.py:1692](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/models.py:1692). |
| Signals | Os signals de vigência e âncora chamam a porta e a regeneração futura; outro chama o reparentamento de folgas. Os signals de batida e ausência chamam o cartório, cujo carimbo passa pela escrita da célula. A transação externa depende do ato chamador **[não verificado]**. [escala/signals.py:67](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/signals.py:67), [escala/signals.py:119](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/signals.py:119), [escala/signals.py:185](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/signals.py:185), [ponto/signals.py:131](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/signals.py:131), [celula.py:1181](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:1181). |

**Não é possível certificar todos os pares de transações como compatíveis.** Além da inversão acima, o salvamento do calendário mantém uma transação ao percorrer vínculos na ordenação padrão `-data_inicio`, e os meses da sincronização vêm de um `set`; nenhum dos dois estabelece uma ordem global por colaborador e mês. A ordem de aquisição efetiva do `DELETE` de múltiplas folgas também está **[não verificado]**. [folgas.py:578](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:578), [folgas.py:600](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:600), [folgas.py:626](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:626), [models.py:1000](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/models.py:1000).

**Comparação histórica:** o diff fornecido mostra que em `bb40f26c5` as duas chamadas já faziam `select_for_update` antes da escrita subsequente; o delta move sua declaração para a raiz e acrescenta ordenação explícita. Portanto, a inversão célula → folga identificada na sincronização não nasce simplesmente dessa movimentação. O SQL anterior e todos os demais caminhos de `bb40f26c5` estão **[não verificado]**; a comparação com `dd489c627` está **[não verificado]**, pois essa revisão não consta do diff fornecido nem do histórico Git desta cópia. [dado fornecido: `c4.diff`, hunks de `escala/folgas.py` e `ponto/services/cadastro_futuro.py`]; [folgas.py:617](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:617).

## 2. Semântica do queryset

Pela composição pretendida do queryset, `filter` restringe o conjunto, `values_list` muda a projeção e `order_by('data')` substitui a ordenação anterior; esses encadeamentos não pedem a remoção de `select_for_update`. **SQL final efetivamente produzido e conjunto efetivamente travado no banco: [não verificado]**. [cadastro_futuro.py:19](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:19), [cadastro_futuro.py:42](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:42).

Na regeneração, a substituição de `(colaborador_id, data)` por `data` preserva a ordem relevante **dentro dessa consulta**, porque ela já fixa um único colaborador; há unicidade por colaborador e data. Isso não estabelece ordem global entre várias consultas ou transações. [cadastro_futuro.py:42](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:42), [models.py:1830](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/models.py:1830).

Os predicados mostrados no diff permanecem os mesmos nos dois pontos alterados, mas **não afirmo identidade exata das tabelas travadas com `bb40f26c5`** sem o SQL: antes havia ordenação padrão por `colaborador`, cujo modelo ordena por nome; um JOIN de ordenação e seu efeito sob `FOR UPDATE` estão **[não verificado]**. O delta usa explicitamente `colaborador_id` na sincronização. [dado fornecido: `c4.diff`]; [models.py:1834](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/models.py:1834), [colaboradores/models.py:400](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/colaboradores/models.py:400), [cadastro_futuro.py:19](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:19).

## 3. Catraca A e JOIN

A dispensa de `CelulaDia` é **coerente com o escopo sintático declarado** da catraca: ela procura os quatro modelos de `TRAVADOS` ou raiz não resolvida; registra imports locais e resolve o nome `CelulaDia`. Isso elimina o falso positivo de raiz desconhecida, mas **não prova** que a ordem envolvendo células e os quatro modelos seja segura. [test_e1_v7_catracas.py:5](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/chamados/tests/test_e1_v7_catracas.py:5), [test_e1_v7_catracas.py:70](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/chamados/tests/test_e1_v7_catracas.py:70), [test_e1_v7_catracas.py:98](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/chamados/tests/test_e1_v7_catracas.py:98).

Os dois usos atuais de `travar=True` não acrescentam `select_related`; o uso com `select_related` no feriado deixa `travar=False`. Um JOIN com `FOR UPDATE` sem `of=` **pode** ampliar as tabelas travadas, mas a presença e o efeito de JOIN no SQL concreto, inclusive em `editar_dia`, estão **[não verificado]**. A própria catraca declara limites dos observadores de ordem e de linhas. [cadastro_futuro.py:42](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:42), [cadastro_futuro.py:73](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:73), [folgas.py:620](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:620), [celula.py:631](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:631), [test_e1_v7_catracas.py:36](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/chamados/tests/test_e1_v7_catracas.py:36).

## 4. Chamadores e significado

O alcance comum significa células **existentes**, de `origem='gerada'`, com `data > hoje`, opcionalmente de um colaborador, sem teto superior. `travar` altera a intenção de bloqueio, não o predicado declarado. Os quatro usos diretos encontrados são: regeneração e sincronização com trava; feriado e vigia sem trava. Não encontrei encadeamento direto restante de `select_for_update` nesses chamadores. [cadastro_futuro.py:8](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:8), [cadastro_futuro.py:42](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:42), [cadastro_futuro.py:73](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:73), [folgas.py:620](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:620), [vigia_cadastro_futuro.py:43](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/vigia_cadastro_futuro.py:43).

O vigia declara que mede sem alterar a célula; sua leitura sem trava não é, sozinha, prova de defeito, e a consistência da fotografia concorrente está **[não verificado]**. No feriado existe uma janela concreta: ele lê e agrupa geradoras antes de chamar a regeneração que trava; mudança concorrente nesse intervalo pode tornar a seleção inicial antiga. A consequência efetiva está **[não verificado]**. [vigia_cadastro_futuro.py:1](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/vigia_cadastro_futuro.py:1), [cadastro_futuro.py:73](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:73), [cadastro_futuro.py:83](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:83).

## 5. Checklist das classes

- **Segundo juiz:** o delta seleciona alcance e delega a decisão de vínculo a `vinculos_dos_dias`/`vinculo_do_dia`; duplicação de juiz pelo delta não identificada. Auditoria completa de todos os juízes **[não verificado]**. [cadastro_futuro.py:49](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:49), [cadastro_futuro.py:78](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:78).
- **Segundo escritor fora da porta:** o delta delega a escrita da célula a `regenerar_celulas_vinculo`; a conciliação de `FolgaDia` usa o escritor existente. Exaustividade de escritores em toda a árvore **[não verificado]**. [cadastro_futuro.py:55](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:55), [folgas.py:637](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:637).
- **Relógio fora da borda:** o alcance e a regeneração exigem `hoje`; o salvamento do calendário lê uma vez e repassa aos vínculos. Outras leituras em caminhos chamados, inclusive a relavra de folga, existem; conformidade integral da obra **[não verificado]**. [cadastro_futuro.py:17](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:17), [folgas.py:597](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:597), [folgas.py:278](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:278).
- **Derivação paralela:** os quatro usos encontrados compartilham o alcance; o vigia acrescenta deliberadamente células não geradas ao selo de julgamento. Duplicações fora desses usos **[não verificado]**. [cadastro_futuro.py:8](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/cadastro_futuro.py:8), [vigia_cadastro_futuro.py:30](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/vigia_cadastro_futuro.py:30).
- **Selo/RED sem mordida:** há testes de mutação da catraca e testes dos selos no código; execução e resultado nesta cópia **[não verificado]**. [test_e1_v7_catracas.py:398](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/chamados/tests/test_e1_v7_catracas.py:398), [test_cadastro_futuro_c5.py:253](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/tests/test_cadastro_futuro_c5.py:253).
- **Ordem de trava:** **risco identificado**, conforme a inversão entre foto mensal e sincronização; cobertura de todos os pares **[não verificado]**. [folgas.py:193](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:193), [folgas.py:617](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:617).
- **Campo com dois significados:** o vigia documenta que o parâmetro `fim` não limita mais o universo e usa `ultimo_dia_medido` na saída, evitando reutilizar a chave histórica `fim`; consumidores e snapshots históricos completos **[não verificado]**. [vigia_cadastro_futuro.py:30](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/vigia_cadastro_futuro.py:30), [vigia_cadastro_futuro.py:91](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/services/vigia_cadastro_futuro.py:91).

Não há, no dado fornecido, resultado de execução concorrente, SQL capturado, banco ou logs; essas conclusões permanecem limitadas à leitura estática e ao diff anexado **[não verificado]**.

## 18/11 09:42 · sessao de278bb1377d · - · - · origem=Ronald (tmux)

Conclusões e caminho da cópia `dd489c627` enviados ao agente principal.

## 18/11 09:42 · sessao 65d352a3a5ca · - · - · origem=Ronald (tmux)

O inventário dos locais identificados foi enviado ao agente principal. A busca textual por mutações em objetos chamados `cel`, `celula`, `fd`, `folga` e variantes encontrou as escritas diretas listadas em [ponto/portas/celula.py](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/ponto/portas/celula.py:225) e [escala/folgas.py](/home/ronald/codex_ws/f1665d857058270ffd7a6575b2dc4b6862c60715/app/escala/folgas.py:150); a exaustividade de mutações por outros aliases e cascatas permanece [não verificado].

## 18/11 09:41 · sessao 7aab56254b79 · - · - · origem=Ronald (tmux)

Investigação dos chamadores, do relógio e dos selos enviada ao agente principal, com referências de arquivo e linha.

## 18/11 09:40 · sessao b4c194a9e722 · E4C-F2-P2 · verificador · origem=Code

**Veredito: não aprovo a identidade de recusa com o HEAD como demonstrada.** O lote conserva os IDs anteriores à trava para a guarda, conforme a opção A; porém, com o relido não vazio, a obra acrescenta uma gravação de decisão e de evento **antes** da guarda. Uma falha nessa gravação pode preceder o veto que o HEAD alcançaria. Há também um relógio anterior à trava em `reabrir_para_correcao` que pode inverter a ordem interpretada pela nova leitura da sequência. [app/chamados/models.py:1620](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1620), [app/chamados/models.py:1694](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1694), [app/chamados/models.py:4337](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:4337), [app/chamados/services/disputa_emissao.py:1629](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1629)

A cópia não tem metadados Git. A comparação com `dd489c627` abaixo usa o patch e o censo fornecidos; a identidade da cópia com o hash e os resultados de execuções nos commits históricos são **[não verificado]**.

1. **Comparação dos três chamadores**

| Chamador | `recusa_par_ilegal` | `exigir_colaboradores_travados` e guarda | UPDATE |
|---|---|---|---|
| `ressuscitar_perguntas` | Os dois valores são fornecidos: julga **uma vez** o par `(None, '')`, sem percorrer perguntas. | Recebem o queryset formado pelos IDs anteriores à trava; a guarda exclui as vias fora do fato na consulta feita depois. | HEAD: queryset dos IDs anteriores, excluídas as vias fora do fato. Árvore atual: interseção desses IDs com os IDs relidos materializados e fora das vias excluídas, novamente excluindo as vias fora do fato. |
| `reabrir_para_correcao` | Uma vez, sobre `(None, '')`. | `qs` recebido; guarda exclui vias fora do fato. | `qs`, excluídas as vias fora do fato: `escrever_so=None`. |
| `devolver_instante_sem_texto_l130` | Uma vez, sobre `(None, '')`. | Queryset da pergunta relida após a trava; guarda exclui vias fora do fato. | Esse queryset, excluídas as vias fora do fato: `escrever_so=None`. |

A mecânica está em [models.py:3454](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:3454), [models.py:3473](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:3473), [models.py:3484](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:3484), [disputa_emissao.py:1617](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1617) e [validacao.py:428](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/validacao.py:428). A descrição do HEAD vem do patch fornecido **[não verificado por checkout]**.

**Severidade alta — identidade de recusa incompleta.** No ponto do lote, a mudança de conjunto do UPDATE é a prevista pelo contrato. `alvo_lido`, contudo, é um queryset: os **IDs** foram selecionados antes, mas as linhas e a exclusão de `VIAS_FORA_DO_FATO` são avaliadas posteriormente. Assim, “conjunto lido” não é uma foto congelada dos campos. [models.py:1613](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1613), [models.py:1620](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1620), [models.py:3478](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:3478).

2. **Ressuscitação: recusa e escritas**

- **Relido vazio:** não há retorno antecipado; as listas de fotos e compensações ficam vazias e o lote ainda chega à guarda com os IDs antigos. Uma via que passou a ser fora do fato continua excluída da guarda, como o filtro do lote determina. [models.py:1650](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1650), [models.py:1700](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1700), [models.py:4332](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:4332), [models.py:3479](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:3479).
- **Relido menor e não vazio:** a guarda do lote ainda considera os IDs antigos, sujeitos à exclusão atual de vias. Mas antes dela `registrar(_compensa)` faz `bulk_create` e emite `EventoCelula`; se alguma dessas operações lançar exceção antes de o veto ser anotado, `executar_sob_cerca` propaga a exceção em vez de devolver `(None, vetadas)`. O HEAD, segundo o patch fornecido, não tinha essa REABERTURA adicional. A ocorrência de tal falha em produção é **[não verificado]**; o caminho de exceção é demonstrado pelo código. [models.py:1686](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1686), [models.py:4337](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:4337), [celula.py:945](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/ponto/portas/celula.py:945), [fio.py:1449](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/fio.py:1449).
- A foto da projeção também é registrada antes da guarda, mas essa posição já consta do comportamento anterior fornecido; agora ela usa o relido. Não há prova de identidade de todas as falhas possíveis dessa etapa quando o conjunto muda. [models.py:1650](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1650), [models.py:1654](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1654).
- Para os efeitos pedidos, não encontrei uso dos IDs antigos **como conjunto de escrita**: foto, REABERTURA, evento da pergunta, disputa e trilha usam `alvo`/`ids` relidos; `n` vem do UPDATE restrito. A aquisição inicial das travas usa o conjunto antigo, deliberadamente. A trilha também inclui `pulados` e `mortas_c6` calculados antes da trava: esses diagnósticos não são IDs de perguntas escritas e não foram relidos. [models.py:1632](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1632), [models.py:1650](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1650), [models.py:1700](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1700), [models.py:1710](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1710), [models.py:1726](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1726).

3. **Escritores, leitores e significado**

O mapa seguinte identifica os pontos diretamente relevantes ao diff; a enumeração exaustiva de **todos** os escritores e leitores de cada campo, nas 1.280 linhas do censo citado mas não fornecido, fica **[não verificado]**.

| Campos | Escrita e leitura verificadas |
|---|---|
| `materializada_em`, `materializada_por`, `via_resolucao` | `_escrever_carimbo` atribui o trio; o lote o limpa; `encerrar_por_fato_no_espelho` também altera a via. Ressuscitação seleciona materialização e via; C6 lê carimbo e via. [models.py:3293](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:3293), [models.py:3484](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:3484), [models.py:3489](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:3489), [models.py:1542](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1542), [juizes.py:1389](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/juizes.py:1389). |
| `validada_em`, `validada_por` | A validação os grava; resposta humana e devoluções podem limpá-los; o lote também pode limpá-los. A nova leitura interpreta a sequência e o passivo, enquanto C6 lê a projeção temporal. [validacao.py:161](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/validacao.py:161), [disputa_emissao.py:1484](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1484), [validacao.py:345](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/validacao.py:345), [models.py:4295](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:4295), [juizes.py:1403](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/juizes.py:1403). |
| `respondida_em`, `resposta_colab` | Resposta humana, automática e carimbo pela trilha escrevem o par ou seu instante; a ressuscitação limpa ambos. A nova leitura reconstitui o par; o juiz de respondida exige instante e texto; C6 usa a projeção cortada por `agora`. [disputa_emissao.py:1480](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1480), [disputa_emissao.py:1680](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1680), [disputa_emissao.py:1692](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1692), [models.py:1702](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1702), [juizes.py:1544](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/juizes.py:1544). |
| `da_ressuscitacao` | A ressuscitação grava `True` na REABERTURA; o predicado lê ato e marca, e a carga da tarefa exclui essas linhas. [models.py:1694](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1694), [models.py:4221](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:4221), [tarefa_do_dia.py:421](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/tarefa_do_dia.py:421). |
| `resposta_mantem_validacao` | Humana grava `False`; automática, trilha e foto gravam `True`. A leitura Python e sua face SQL usam o campo; 0056 preenche o histórico a partir do motivo. [disputa_emissao.py:1532](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1532), [disputa_emissao.py:1686](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1686), [disputa_emissao.py:1707](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1707), [models.py:4260](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:4260), [models.py:4280](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:4280), [models.py:3551](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:3551), [0056:12](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12). |
| `ato_motivo` | É texto escrito por várias portas; a devolução BUG-PAR escreve prefixo que `rotulo_bug_par_103` interpreta. A migração também interpreta o preenchimento de motivo em RESPOSTA para o backfill. Logo, a afirmação de que é *somente* texto de trilha não vale para todos os leitores. [validacao.py:354](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/validacao.py:354), [materializacao.py:265](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/materializacao.py:265), [0056:12](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12). |

**Severidade alta — relógio antes da borda:** `reabrir_para_correcao` captura `_agora` antes de adquirir a trava do colaborador, depois usa esse instante na REABERTURA e limpa a projeção. Se uma resposta adquirir a trava e gravar entre esses dois momentos, a REABERTURA pode ter `ato_em` anterior à resposta que efetivamente limpa; a nova `leitura` ordena por instante e poderá apresentar a resposta como vigente após o UPDATE tê-la zerado. A existência dessa intercalação em produção é **[não verificado]**. [disputa_emissao.py:1629](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1629), [disputa_emissao.py:1639](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1639), [disputa_emissao.py:1655](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1655), [models.py:4308](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:4308).

A distinção entre **instante e dia** também é real: a nova leitura não recebe teto temporal, enquanto C6 corta campos futuros pelo instante `agora`; a carga da tarefa corta atos pelo mesmo tipo de teto. Não é possível concluir que sejam intercambiáveis em consultas históricas. [models.py:4295](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:4295), [juizes.py:1382](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/juizes.py:1382), [tarefa_do_dia.py:418](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/tarefa_do_dia.py:418).

4. **Classes de reprovação**

- **Segundo juiz registrado:** não identifiquei clone no trecho examinado: a resposta continua chamando `pergunta_respondida`, e a face de validação reimplementa em SQL a regra da sequência. Ausência global de duplicação no diff inteiro: **[não verificado]**. [models.py:3551](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:3551), [juizes.py:1544](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/juizes.py:1544).
- **Segundo escritor fora da porta:** a nova REABERTURA passa por `registrar`; o UPDATE passa por `descarimbar_em_lote`. Ausência global de outro escritor: **[não verificado]**. [models.py:1694](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1694), [models.py:1702](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1702).
- **Relógio fora da borda: presente**, alta, no caminho de reabertura descrito acima. O diff corrige o instante da resposta humana para depois da trava, mas não corrige esse outro caminho. [disputa_emissao.py:1520](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1520), [disputa_emissao.py:1629](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1629).
- **Derivação paralela de fonte única:** existe face SQL própria para a leitura da validação; ambas usam os mesmos tipos de ato e campo, mas a equivalência em todos os estados e execuções é **[não verificado]**. [models.py:3540](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:3540), [models.py:4314](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:4314).
- **RED sem mordida:** por inspeção, o teste I cobre relido vazio e espera veto; o II observa guarda com dois IDs e escrita com um. O I não testa falha da nova escrita anterior à guarda com relido não vazio; a foto dele também não inclui `EventoCelula`. A execução efetiva no HEAD e na volta 3 é **[não verificado]**. [test_f2_p2_recusa_sob_a_cerca.py:87](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/tests/test_f2_p2_recusa_sob_a_cerca.py:87), [test_f2_p2_recusa_sob_a_cerca.py:97](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/tests/test_f2_p2_recusa_sob_a_cerca.py:97), [test_f2_p2_recusa_sob_a_cerca.py:117](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/tests/test_f2_p2_recusa_sob_a_cerca.py:117).
- **Ordem de trava:** no caminho da ressuscitação, presente a aquisição C → D → chamado → pergunta antes das novas escritas; auditoria de todos os caminhos indiretos: **[não verificado]**. [models.py:1632](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1632), [models.py:2852](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:2852).
- **Pouso antes do pré-requisito:** ausente na dependência de migração: 0056 depende de 0055. A condição operacional de nenhum processo anterior ao P1 vivo é declarada, mas seu cumprimento é **[não verificado]**. [0056:6](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:6), [0056:19](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:19).
- **Campo com dois significados: presente**, em `ato_motivo`: texto exibível e prefixo interpretado pelo leitor BUG-PAR; o backfill ainda usa preenchimento como regra histórica para RESPOSTA. Isso é uma constatação de uso, não prova de que a obra tenha criado o leitor de prefixo. [materializacao.py:265](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/materializacao.py:265), [0056:12](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12).

5. **PK em `escrever_so` fora de `qs`:** não é atualizado e não provoca erro por essa razão: o UPDATE exige simultaneamente `pk__in=escrever_so` e `pk__in=qs.values('pk')`. Na ressuscitação, `ids` é derivado de `alvo`, e os outros dois chamadores deixam `escrever_so=None`; outro chamador concreto que passe PK externo é **[não verificado]**. [models.py:1650](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:1650), [models.py:3484](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/models.py:3484), [disputa_emissao.py:1656](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/disputa_emissao.py:1656), [validacao.py:440](/home/ronald/codex_ws/9500f8063931c654c465f7138de73848b6ae0009/app/chamados/services/validacao.py:440).

## 18/11 09:40 · sessao b3fb66ab052c · DIA-SEM-TRABALHO-PORTA-L2 · verificador · origem=Code

A troca faz `_protesta` consultar o juiz registrado, mas não torna `_protesta` equivalente à guarda inteira de reconciliação: eles respondem perguntas diferentes e podem olhar datas diferentes. O selo AST também tem uma brecha concreta. [app/ponto/services/cartorio.py:1464](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1464), [app/chamados/models.py:1313](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/models.py:1313), [app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:155](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:155).

O diretório não expõe metadados Git; portanto, a identidade do conteúdo com o hash informado está **[não verificado]**. Também não executei a suíte, pois os testes apresentados criam dados em banco; resultados de execução e números de produção estão **[não verificado]**. [app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:43](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:43).

1. **Pergunta antiga, nova e divergências**

   **PROCEDE a mudança de pergunta.** Antes, para cada chamado, perguntava se `trabalha is not False` e se os marcos **não** estavam comprovadamente acesos. Agora pergunta se o juiz **não** declara dia sem trabalho e mantém a mesma condição sobre marcos. O texto da forma anterior vem do diff fornecido; a forma atual está em [app/ponto/services/cartorio.py:1464](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1464). A forma anterior no diff: **[não verificado em Git]**.

   Para a mesma célula e ata, o componente de natureza do dia concorda exatamente com o juiz: `_protesta` usa sua negação. A tabela abaixo é a resposta de `dia_sem_trabalho`; `_protesta` ainda exige `_acesos(x, ata) is not True`. [app/chamados/juizes.py:432](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/juizes.py:432), [app/chamados/juizes.py:445](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/juizes.py:445), [app/ponto/services/cartorio.py:1464](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1464).

   | `trabalha` | `tipo_dia` | `cobertura_parcial` | Juiz: sem trabalho |
   |---|---|---|---|
   | `False` | qualquer dos valores pedidos | qualquer | Sim |
   | `True` ou `None` | ausente ou `trabalho` | qualquer | Não |
   | `True` ou `None` | `folga` ou `falta` | qualquer | Sim |
   | `True` ou `None` | `ausencia` | verdadeiro | Não |
   | `True` ou `None` | `ausencia` | ausente ou falso | Sim |

   **PROCEDE a divergência do resultado completo em relação ao juiz:** com dia de trabalho e marcos comprovadamente acesos, o juiz responde `False` e `_protesta` também retorna `False`; isso decorre da segunda condição, não de uma cópia do juiz. [app/ponto/services/cartorio.py:1464](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1464).

   **PROCEDE a divergência em relação a `premissa_regularizada`:** ela também aceita justificativa aprovada, veredito administrativo e marcos inexistentes, entre outros caminhos; além disso, exclui `turno_aberto` da aplicação direta desse ramo do juiz. Logo, não é o complemento lógico de `_protesta`, nem garante fechamento: `reconciliar` tem outras guardas. [app/chamados/models.py:1313](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/models.py:1313), [app/chamados/models.py:1326](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/models.py:1326), [app/chamados/models.py:677](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/models.py:677).

2. **Classes de reprovação**

   - **Segundo juiz duplicado: NÃO PROCEDE no diff.** O novo trecho chama a autoridade `_C18` registrada e não reimplementa suas cláusulas. [app/core/juizes.py:302](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/core/juizes.py:302), [app/ponto/services/cartorio.py:1462](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1462).
   - **Segundo escritor fora da porta: NÃO PROCEDE no trecho alterado.** Ele constrói um objeto de insumo e lê o juiz; a lavra continua pela porta da célula. [app/ponto/services/cartorio.py:1464](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1464), [app/ponto/services/cartorio.py:1861](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1861).
   - **Relógio lido fora da borda: NÃO PROCEDE no trecho alterado.** A nova chamada não lê relógio. [app/ponto/services/cartorio.py:1462](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1462).
   - **Derivação paralela de fonte única: PROCEDE como observação, fora do diff.** A ata em curso é construída na linha 1436 e a ata para lavra é reconstruída pela mesma composição na linha 1859; não verifiquei uma divergência efetiva entre ambas. [app/ponto/services/cartorio.py:1436](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1436), [app/ponto/services/cartorio.py:1859](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1859).
   - **Selo sem mordida: PROCEDE parcialmente.** Ele morde os dois mutantes explícitos, mas não impede todas as formas de manter um predicado próprio; exemplo no item 5. [app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:182](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:182).
   - **Ordem de trava ou pouso antes do pré-requisito: NÃO PROCEDE como defeito demonstrado pelo diff; [não verificado] quanto ao pouso real.** O caminho adia o lote quando aplicável e a decisão de fechamento conserva as guardas. [app/ponto/services/cartorio.py:1528](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1528), [app/chamados/models.py:677](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/models.py:677).
   - **Campo com dois significados: PROCEDE como risco de interface existente, fora do diff.** O parâmetro `chamados_vivos` do classificador recebe aqui a quantidade que *protesta*, enquanto `cobrancas_vivas` recebe a quantidade com fio; não são contagens intercambiáveis. A docstring do classificador descreve essa distinção. [app/ponto/services/cartorio.py:1471](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1471), [app/ponto/supra_juiz.py:226](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/supra_juiz.py:226).

3. **Escritores, leitores e significado**

   **PROCEDE que há mais de um contexto temporal e de data.**

   - `cel.trabalha`: a porta gera/regenera e edita o valor; `_protesta` o fornece ao juiz. É a natureza prevista da célula, com `None` indeterminado. [app/ponto/portas/celula.py:293](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/portas/celula.py:293), [app/ponto/portas/celula.py:638](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/portas/celula.py:638), [app/escala/models.py:1686](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/escala/models.py:1686).
   - `tipo_dia` e `cobertura_parcial`: a grade marca folga, falta ou ausência e a parcial; `ata_do_dia` transporta os valores, e o juiz os lê. A ausência parcial continua rotulada `ausencia`, embora cobre o resto. [app/escala/utils.py:1147](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/escala/utils.py:1147), [app/escala/utils.py:1210](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/escala/utils.py:1210), [app/ponto/services/cartorio.py:730](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:730), [app/chamados/juizes.py:458](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/juizes.py:458).
   - `_ata_agora.ata` é a ata **em curso**, exposta por colaborador e data antes da lavra; o coletor da premissa usa essa projeção quando disponível e, fora desse julgamento ou para outra data, usa a célula gravada. [app/ponto/services/cartorio.py:1431](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1431), [app/chamados/juizes.py:407](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/juizes.py:407).
   - `_protestam` alimenta `chamados_vivos` no classificador; o filtro sobre `_com_fio` alimenta `cobrado`; `_nao_protestam` alimenta o lote `_adiar` e as tentativas `_porm`. `_porm` contém apenas transições que a porta aceitou, não todos os candidatos. [app/ponto/services/cartorio.py:1467](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1467), [app/ponto/services/cartorio.py:1509](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1509), [app/ponto/services/cartorio.py:1540](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1540).
   - O cartório agrupa chamados pela data declarada no catálogo e julga `cel.data`; a premissa usa `dia_do_fato(fortes=True)`, que prioriza a célula vinculada. Se as fontes de data divergirem, não há garantia de que consultem a mesma célula. O cartório também explicita que batidas pertencem ao dia-turno, inclusive na madrugada. [app/ponto/services/cartorio.py:112](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:112), [app/chamados/juizes.py:94](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/juizes.py:94), [app/ponto/services/cartorio.py:1419](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1419).

4. **`_ata_agora.ata` pode ser `None` ou não-dict?**

   **NÃO PROCEDE no caminho normal mostrado:** `ata_do_dia` constrói e retorna um dict; `dispensar_por_ausencia` devolve a mesma ata; `_ata_ate_o_teto` a conserva ou retorna outro dict. Uma exceção anterior interromperia o caminho. [app/ponto/services/cartorio.py:730](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:730), [app/ponto/services/cartorio.py:781](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:781), [app/ponto/services/cartorio.py:907](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:907).

   **PROCEDE como limite da API do juiz:** `None` ou outro valor falso vira `{}`; um não-dict verdadeiro sem método `.get` provoca erro, inclusive antes da cláusula `trabalha is False`, pois a função da parcial é chamada primeiro. [app/chamados/juizes.py:442](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/juizes.py:442), [app/chamados/juizes.py:454](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/juizes.py:454).

5. **Testes e selo** [nao verificado]

   **PROCEDE parcialmente.** G03 monta `trabalha=True`, ausência integral e chamado legado sem marcos; as asserções procuram a retirada de `COBRANCA_EM_DIA_COBERTO` e de um veredito acusador, coerentes com a mudança do insumo ao classificador. A falha na versão velha e a passagem na nova estão **[não verificado]** por execução. [app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:65](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:65), [app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:94](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:94), [app/ponto/supra_juiz.py:544](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/supra_juiz.py:544).

   **NÃO PROCEDE que todos devam falhar na forma velha:** B06, B07 e B08 são controles de comportamento preservado; A01 verifica estabilidade e ausência do código na segunda passada, mas não prova sozinho que a transição inicial ocorreu pelo motivo correto. [app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:107](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:107), [app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:120](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:120).

   **PROCEDE que o selo morde o mutante misto apresentado**, pois ele encontra `.trabalha` fora do valor direto de um argumento nomeado `trabalha`. **PROCEDE que é burlável:** `getattr(_cel, 'trabalha') is not False and not _dst(N(trabalha=_cel.trabalha))` chama o juiz e mantém o predicado próprio, mas a leitura por `getattr` não é um nó `ast.Attribute` com atributo `trabalha`; a chamada ao juiz também pode ter seu resultado ignorado. Essa conclusão é análise estática; execução do mutante **[não verificado]**. [app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:155](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:155), [app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:190](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:190).

6. **Leitores e números afetados**

   **PROCEDE que há números potencialmente alterados**, condicionados aos casos alcançados e ao resultado das demais guardas; o tamanho e a direção líquida em produção são **[não verificado]**. [app/ponto/services/cartorio.py:1467](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1467), [app/chamados/models.py:677](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/models.py:677).

   - Diretamente, `classificar_dia` recebe menos protestantes e pode deixar de emitir `COBRANCA_EM_DIA_COBERTO`, `COBRANCA_EM_DIA_SEM_TRABALHO` ou outro código condicionado a essa contagem; o contador `out['protestos']` conta dias com códigos, não chamados. [app/ponto/supra_juiz.py:480](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/supra_juiz.py:480), [app/ponto/supra_juiz.py:507](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/supra_juiz.py:507), [app/ponto/supra_juiz.py:544](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/supra_juiz.py:544), [app/ponto/services/cartorio.py:1708](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/cartorio.py:1708).
   - Indiretamente, uma mudança de veredito acusador ou de fechamento pode alterar o censo de ausência, o censo de fio vivo e a retenção de folha. Esses leitores não consomem `_protesta` diretamente. [app/ponto/services/vigia_ausencia_veredito.py:41](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/ponto/services/vigia_ausencia_veredito.py:41), [app/chamados/services/vigia_fio.py:286](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/chamados/services/vigia_fio.py:286), [app/folha/export.py:1273](/home/ronald/codex_ws/eff8b9659441339d8393aa05eb572767188fde6d/app/folha/export.py:1273).

## 18/11 09:17 · sessao f7974ea77078 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

O script faz análise estática, separa os resultados de `tests/` ao final e registra falhas de leitura ou sintaxe em stderr. Resolução dinâmica e cobertura efetiva da árvore: [não verificado].

```python
#!/usr/bin/env python3
import ast
import csv
import re
import sys
import tokenize
from pathlib import Path

EXACT_FIELDS = {"resposta_colab", "respondida_em"}
FIELD_PREFIXES = ("validada_", "materializada_")
ORM_CALLS = {"filter", "exclude", "Q", "F", "values", "order_by"}
WRITE_CALLS = {"create", "update"}
TEMPLATE_TOKEN = re.compile(r"{{.*?}}|{%.*?%}", re.DOTALL)
TEMPLATE_FIELD = re.compile(
    r"(?<![\w])(?:\w+\.)*"
    r"(resposta_colab|respondida_em|validada_\w+|materializada_\w+)"
    r"(?!\w)"
)


def field_from_name(value):
    if not isinstance(value, str):
        return None
    name = value.lstrip("-").split("__", 1)[0]
    if name in EXACT_FIELDS or name.startswith(FIELD_PREFIXES):
        return name
    return None


def dotted(node):
    if isinstance(node, ast.Name):
        return node.id
    if isinstance(node, ast.Attribute):
        parent = dotted(node.value)
        return parent + "." + node.attr if parent else None
    return None


def clean(value):
    return " ".join(str(value).split())


def is_test(path):
    return any(part == "tests" for part in path.parts)


class ScopeBindings(ast.NodeVisitor):
    """Collect bindings in one lexical scope, without entering child scopes."""

    def __init__(self):
        self.aliases = {}
        self.values = {}

    def visit_Import(self, node):
        for alias in node.names:
            local = alias.asname or alias.name.split(".", 1)[0]
            self.aliases[local] = alias.name if alias.asname else local

    def visit_ImportFrom(self, node):
        prefix = "." * node.level + (node.module or "")
        for alias in node.names:
            if alias.name != "*":
                self.aliases[alias.asname or alias.name] = (
                    prefix + "." + alias.name if prefix else alias.name
                )

    def visit_Assign(self, node):
        for target in node.targets:
            if isinstance(target, ast.Name):
                self.values[target.id] = node.value
                name = dotted(node.value)
                if name:
                    self.aliases[target.id] = name
        self.generic_visit(node.value)

    def visit_AnnAssign(self, node):
        if isinstance(node.target, ast.Name) and node.value is not None:
            self.values[node.target.id] = node.value
            name = dotted(node.value)
            if name:
                self.aliases[node.target.id] = name
        if node.value is not None:
            self.visit(node.value)

    def visit_FunctionDef(self, node):
        pass

    def visit_AsyncFunctionDef(self, node):
        pass

    def visit_ClassDef(self, node):
        pass

    def visit_Lambda(self, node):
        pass


class Census(ast.NodeVisitor):
    def __init__(self, path, source):
        self.path = path
        self.source = source
        self.lines = source.splitlines()
        self.scope_names = []
        self.bindings = []
        self.rows = []
        self.seen = set()

    def add(self, node, field, mode):
        line = getattr(node, "lineno", 1)
        segment = ast.get_source_segment(self.source, node)
        if not segment:
            segment = self.lines[line - 1] if line <= len(self.lines) else ""
        scope = ".".join(self.scope_names) or "<modulo>"
        row = (str(self.path), line, scope, field, mode, clean(segment))
        if row not in self.seen:
            self.seen.add(row)
            self.rows.append(row)

    def push_scope(self, body): [nao verificado]
        bindings = ScopeBindings()
        for statement in body:
            bindings.visit(statement)
        self.bindings.append(bindings)

    def resolve_alias(self, name):
        if not name:
            return ""
        seen = set()
        while name and name not in seen:
            seen.add(name)
            head, separator, tail = name.partition(".")
            replacement = None
            for bindings in reversed(self.bindings):
                if head in bindings.aliases:
                    replacement = bindings.aliases[head]
                    break
            if not replacement or replacement == head:
                break
            name = replacement + (separator + tail if separator else "")
        return name

    def resolve_value(self, node, seen=None):
        if not isinstance(node, ast.Name):
            return node
        seen = set() if seen is None else seen
        if node.id in seen:
            return node
        seen.add(node.id)
        for bindings in reversed(self.bindings):
            if node.id in bindings.values:
                return self.resolve_value(bindings.values[node.id], seen)
        return node

    def strings(self, node, seen=None):
        """Yield literal strings from a statically visible expression."""
        seen = set() if seen is None else seen
        if isinstance(node, ast.Name):
            if node.id in seen:
                return
            seen.add(node.id)
            resolved = self.resolve_value(node)
            if resolved is not node:
                yield from self.strings(resolved, seen)
        elif isinstance(node, ast.Constant) and isinstance(node.value, str):
            yield node, node.value
        elif isinstance(node, (ast.List, ast.Tuple, ast.Set)):
            for element in node.elts:
                yield from self.strings(element, set(seen))
        elif isinstance(node, ast.Dict):
            for key in node.keys:
                if key is not None:
                    yield from self.strings(key, set(seen))
        elif isinstance(node, ast.BinOp) and isinstance(node.op, ast.Add):
            yield from self.strings(node.left, set(seen))
            yield from self.strings(node.right, set(seen))

    def record_strings(self, node, mode):
        for location, value in self.strings(node):
            field = field_from_name(value)
            if field:
                self.add(location, field, mode)

    def record_dict_keys(self, node):
        node = self.resolve_value(node)
        if isinstance(node, ast.Dict):
            for key, value in zip(node.keys, node.values):
                if key is None:
                    self.record_dict_keys(value)
                else:
                    self.record_strings(key, "S")
        elif isinstance(node, ast.Call) and self.call_name(node) == "dict":
            for keyword in node.keywords:
                if keyword.arg is None:
                    self.record_dict_keys(keyword.value)
                else:
                    field = field_from_name(keyword.arg)
                    if field:
                        self.add(keyword.value, field, "S")

    def call_name(self, node):
        name = self.resolve_alias(dotted(node.func))
        return name.rsplit(".", 1)[-1] if name else ""

    def visit_Module(self, node):
        self.push_scope(node.body) [nao verificado]
        self.generic_visit(node)
        self.bindings.pop()

    def visit_ClassDef(self, node):
        for decorator in node.decorator_list:
            self.visit(decorator)
        for base in node.bases:
            self.visit(base)
        for keyword in node.keywords:
            self.visit(keyword.value)
        self.scope_names.append(node.name)
        self.push_scope(node.body) [nao verificado]
        for statement in node.body:
            self.visit(statement)
        self.bindings.pop()
        self.scope_names.pop()

    def visit_function(self, node):
        for decorator in node.decorator_list:
            self.visit(decorator)
        for default in node.args.defaults:
            self.visit(default)
        for default in node.args.kw_defaults:
            if default is not None:
                self.visit(default)
        if node.returns is not None:
            self.visit(node.returns)
        self.scope_names.append(node.name)
        self.push_scope(node.body) [nao verificado]
        for statement in node.body:
            self.visit(statement)
        self.bindings.pop()
        self.scope_names.pop()

    visit_FunctionDef = visit_function
    visit_AsyncFunctionDef = visit_function

    def visit_Lambda(self, node):
        self.scope_names.append("<lambda>")
        self.bindings.append(ScopeBindings())
        self.visit(node.body)
        self.bindings.pop()
        self.scope_names.pop()

    def visit_Attribute(self, node):
        field = field_from_name(node.attr)
        if field:
            if isinstance(node.ctx, ast.Load):
                self.add(node, field, "L")
            elif isinstance(node.ctx, ast.Store):
                self.add(node, field, "S")
        self.generic_visit(node)

    def visit_AugAssign(self, node):
        if isinstance(node.target, ast.Attribute):
            field = field_from_name(node.target.attr)
            if field:
                self.add(node.target, field, "L")
        self.generic_visit(node)

    def visit_Call(self, node):
        name = self.call_name(node)

        if name in {"getattr", "setattr"} and len(node.args) >= 2:
            mode = "L" if name == "getattr" else "S"
            self.record_strings(node.args[1], mode)

        if name in WRITE_CALLS:
            for keyword in node.keywords:
                if keyword.arg is None:
                    self.record_dict_keys(keyword.value)
                else:
                    field = field_from_name(keyword.arg)
                    if field:
                        self.add(keyword.value, field, "S")
            if name == "update":
                for argument in node.args:
                    self.record_dict_keys(argument)

        if name == "save":
            for keyword in node.keywords:
                if keyword.arg == "update_fields":
                    self.record_strings(keyword.value, "S")
                elif keyword.arg is None:
                    mapping = self.resolve_value(keyword.value)
                    if isinstance(mapping, ast.Dict):
                        for key, value in zip(mapping.keys, mapping.values):
                            if (
                                isinstance(key, ast.Constant)
                                and key.value == "update_fields"
                            ):
                                self.record_strings(value, "S")

        if name == "carimbar":
            for keyword in node.keywords:
                if keyword.arg == "resposta":
                    self.add(keyword.value, "resposta_colab", "S")
                elif keyword.arg is None:
                    mapping = self.resolve_value(keyword.value)
                    if isinstance(mapping, ast.Dict):
                        for key, value in zip(mapping.keys, mapping.values):
                            if (
                                isinstance(key, ast.Constant)
                                and key.value == "resposta"
                            ):
                                self.add(value, "resposta_colab", "S")

        if name in ORM_CALLS:
            for argument in node.args:
                self.record_strings(argument, "O")
            for keyword in node.keywords:
                if keyword.arg is None:
                    mapping = self.resolve_value(keyword.value)
                    if isinstance(mapping, ast.Dict):
                        for key in mapping.keys:
                            if key is not None:
                                self.record_strings(key, "O")
                else:
                    field = field_from_name(keyword.arg)
                    if field:
                        self.add(keyword.value, field, "O")

        self.generic_visit(node)


def python_rows(root, path):
    relative = path.relative_to(root)
    try:
        with tokenize.open(path) as stream:
            source = stream.read()
        tree = ast.parse(source, filename=str(relative))
    except (OSError, UnicodeError, SyntaxError) as exc:
        print(f"[nao verificado] {relative}: {exc}", file=sys.stderr)
        return []
    census = Census(relative, source)
    census.visit(tree)
    return census.rows


def template_rows(root, path):
    relative = path.relative_to(root)
    try:
        source = path.read_text(encoding="utf-8")
    except (OSError, UnicodeError) as exc:
        print(f"[nao verificado] {relative}: {exc}", file=sys.stderr)
        return []
    rows = []
    for token in TEMPLATE_TOKEN.finditer(source):
        for match in TEMPLATE_FIELD.finditer(token.group()):
            line = source.count("\n", 0, token.start() + match.start()) + 1
            rows.append(
                (
                    str(relative),
                    line,
                    "<template>",
                    match.group(1),
                    "T",
                    clean(token.group()),
                )
            )
    return rows


def main():
    if len(sys.argv) != 2:
        print("Uso: python3 censo.py RAIZ", file=sys.stderr)
        return 2

    root = Path(sys.argv[1])
    if not root.is_dir():
        print(f"[nao verificado] Raiz nao e diretorio: {root}", file=sys.stderr)
        return 2

    rows = []
    for path in root.rglob("*"):
        if not path.is_file():
            continue
        if path.suffix == ".py":
            rows.extend(python_rows(root, path))
        elif path.suffix == ".html" and "templates" in path.relative_to(root).parts:
            rows.extend(template_rows(root, path))

    rows.sort(key=lambda row: (is_test(Path(row[0])), row[0], row[1], row[2], row[3], row[4], row[5]))
    writer = csv.writer(sys.stdout, delimiter="\t", lineterminator="\n")
    writer.writerow(("arquivo", "linha", "funcao qualificada", "campo", "modo", "trecho", "teste")) [nao verificado]
    for row in rows:
        writer.writerow((*row, "sim" if is_test(Path(row[0])) else "nao"))
    return 0


if __name__ == "__main__":
    raise SystemExit(main())
```

## 18/11 09:17 · sessao de38741cfd0b · ESPINHA-REMEDIDA-1811 · censo · origem=Code

O censo abaixo é estático; referências dinâmicas e o efeito de código que não está no dado fornecido ficam [nao verificado]. `brief.md:92`; `matriz.md`, U6 e P8.

```python
#!/usr/bin/env python3
import ast
import re
import sys
import tokenize
from pathlib import Path

FIELDS = (
    "resposta_colab",
    "respondida_em",
    "validada_em",
    "materializada_em",
    "via_resolucao",
)
FIELD_SET = set(FIELDS)
ORM = {
    "filter", "exclude", "get", "Q", "F", "values", "values_list",
    "order_by", "distinct", "only", "defer", "select_related",
    "prefetch_related", "annotate", "aggregate",
}
WRITES = {"create", "update", "get_or_create", "update_or_create", "bulk_update"}
FIELD_PATTERN = re.compile(
    r"(?<![A-Za-z0-9_])(" + "|".join(FIELDS) + r")"
    r"(?=$|__[A-Za-z0-9_]+|[^A-Za-z0-9_])"
)
TEMPLATE_EXPR = re.compile(r"{{.*?}}|{%.*?%}", re.DOTALL)


def fields_in(text):
    return {m.group(1) for m in FIELD_PATTERN.finditer(text)}


def name_of(node):
    if isinstance(node, ast.Name):
        return node.id
    if isinstance(node, ast.Attribute):
        prefix = name_of(node.value)
        return (prefix + "." if prefix else "") + node.attr
    return ""


def literal(node):
    if isinstance(node, ast.Constant) and isinstance(node.value, str):
        return node.value
    return None


def is_test(path):
    return "tests" in path.parts or path.name.startswith("test_")


def clean(text):
    return " ".join(text.replace("\t", " ").split())


class Census(ast.NodeVisitor):
    def __init__(self, path, rel, source):
        self.path = path
        self.rel = rel
        self.source = source
        self.lines = source.splitlines()
        self.scope = []
        self.parents = []
        self.aliases = [{}]
        self.rows = set()

    def emit(self, node, field, mode):
        line = getattr(node, "lineno", 1)
        excerpt = self.lines[line - 1] if 0 < line <= len(self.lines) else ""
        function = ".".join(self.scope) or "<modulo>"
        self.rows.add(
            (str(self.rel), line, function, field, mode, clean(excerpt),
             "sim" if is_test(self.rel) else "nao")
        )

    def resolve(self, name):
        parts = name.split(".")
        for table in reversed(self.aliases):
            if parts[0] in table:
                return ".".join([table[parts[0]], *parts[1:]])
        return name

    def call_name(self, call):
        raw = name_of(call.func)
        return self.resolve(raw).split(".")[-1]

    def visit(self, node):
        self.parents.append(node)
        try:
            return super().visit(node)
        finally:
            self.parents.pop()

    def visit_Import(self, node):
        for alias in node.names:
            local = alias.asname or alias.name.split(".")[0]
            self.aliases[-1][local] = alias.name if alias.asname else local

    def visit_ImportFrom(self, node):
        module = "." * node.level + (node.module or "")
        for alias in node.names:
            if alias.name == "*":
                continue
            local = alias.asname or alias.name
            self.aliases[-1][local] = (
                module + ("." if module and not module.endswith(".") else "")
                + alias.name
            )

    def scoped(self, node):
        self.scope.append(node.name)
        self.aliases.append({})
        for child in ast.iter_child_nodes(node):
            self.visit(child)
        self.aliases.pop()
        self.scope.pop()

    visit_FunctionDef = scoped
    visit_AsyncFunctionDef = scoped
    visit_ClassDef = scoped

    def visit_Attribute(self, node):
        if node.attr in FIELD_SET:
            if isinstance(node.ctx, ast.Load):
                self.emit(node, node.attr, "L")
            elif isinstance(node.ctx, (ast.Store, ast.Del)):
                self.emit(node, node.attr, "S")
        self.generic_visit(node)

    def visit_Name(self, node):
        resolved = self.resolve(node.id).split(".")[-1]
        if resolved in FIELD_SET and node.id != resolved:
            if isinstance(node.ctx, ast.Load):
                self.emit(node, resolved, "L")
            elif isinstance(node.ctx, (ast.Store, ast.Del)):
                self.emit(node, resolved, "S")

    def strings(self, node, mode):
        for child in ast.walk(node):
            value = literal(child)
            if value is not None:
                for field in fields_in(value):
                    self.emit(child, field, mode)

    def visit_Call(self, node):
        method = self.call_name(node)

        if method in {"getattr", "setattr"} and len(node.args) >= 2:
            value = literal(node.args[1])
            if value in FIELD_SET:
                self.emit(node.args[1], value, "L" if method == "getattr" else "S")

        if method in ORM:
            for arg in node.args:
                self.strings(arg, "O")
            for kw in node.keywords:
                if kw.arg:
                    for field in fields_in(kw.arg):
                        self.emit(kw.value, field, "O")
                else:
                    self.strings(kw.value, "O")

        if method in WRITES:
            for arg in node.args:
                self.strings(arg, "S")
            for kw in node.keywords:
                if kw.arg:
                    for field in fields_in(kw.arg):
                        self.emit(kw.value, field, "S")
                # Includes defaults={...}, create_defaults={...}, and **{...}.
                self.strings(kw.value, "S")

        if method == "save":
            for kw in node.keywords:
                if kw.arg == "update_fields":
                    self.strings(kw.value, "S")

        if method == "carimbar":
            for kw in node.keywords:
                if kw.arg == "resposta":
                    self.emit(kw.value, "resposta_colab", "S")
                elif kw.arg:
                    for field in fields_in(kw.arg):
                        self.emit(kw.value, field, "S")
                else:
                    self.strings(kw.value, "S")

        self.generic_visit(node)


def python_rows(path, rel):
    try:
        with tokenize.open(path) as stream:
            source = stream.read()
        tree = ast.parse(source, filename=str(rel), type_comments=True)
    except (OSError, SyntaxError, UnicodeError) as exc:
        print(f"ERRO\t{rel}\t{exc}", file=sys.stderr)
        return set()
    visitor = Census(path, rel, source)
    visitor.visit(tree)
    return visitor.rows


def template_rows(path, rel):
    try:
        source = path.read_text(encoding="utf-8")
    except (OSError, UnicodeError) as exc:
        print(f"ERRO\t{rel}\t{exc}", file=sys.stderr)
        return set()
    lines = source.splitlines()
    rows = set()
    for expr in TEMPLATE_EXPR.finditer(source):
        for match in FIELD_PATTERN.finditer(expr.group()):
            offset = expr.start() + match.start()
            line = source.count("\n", 0, offset) + 1
            excerpt = lines[line - 1] if line <= len(lines) else ""
            rows.add(
                (str(rel), line, "<template>", match.group(1), "T",
                 clean(excerpt), "sim" if is_test(rel) else "nao")
            )
    return rows


def main():
    if len(sys.argv) != 2:
        print("uso: python3 censo.py RAIZ", file=sys.stderr)
        return 2

    root = Path(sys.argv[1])
    if not root.is_dir():
        print(f"raiz nao e diretorio: {root}", file=sys.stderr)
        return 2

    rows = set()
    for path in root.rglob("*"):
        if not path.is_file():
            continue
        rel = path.relative_to(root)
        if path.suffix == ".py":
            rows.update(python_rows(path, rel))
        elif path.suffix == ".html" and "templates" in rel.parts:
            rows.update(template_rows(path, rel))

    print("arquivo\tlinha\tfuncao qualificada\tcampo\tmodo\ttrecho\tteste")
    for test_flag in ("nao", "sim"):
        for row in sorted((r for r in rows if r[-1] == test_flag),
                          key=lambda r: (r[0], r[1], r[2], r[3], r[4], r[5])):
            print("\t".join(map(str, row)))
    return 0


if __name__ == "__main__":
    raise SystemExit(main())
```

**Selo de queries.** O dado identifica `ponto/tests/test_procedencia_batida.py:126-128`: `assertNumQueries(1)` para cinco batidas, com fixture sem `respondida_em` e `validada_em`; ele não mede a variação de N com R e V carimbadas (`matriz.md`, U1b e P5). O brief relata não encontrar `assertNumQueries` nos testes de colaboradores para `contexto_calendario`, mas não verifica outros apps (`brief.md:92`). A lista completa de testes com `assertNumQueries` ou `CaptureQueriesContext`, e a existência de algum outro que varie N com R e V carimbadas, ficam **[nao verificado]**.

**Resposta futura no calendário.** O dado informa que a forma R3/F1 chama `_pv(_q, agora=_agora)` e associa `_pv` a `_perg_viva` (`colaboradores/services/calendario.py:506`; `matriz.md`, U6b). Ele não mostra o corpo do juiz nem o percurso completo até a grade. Portanto, se uma pergunta com `respondida_em > agora` é considerada viva e o efeito final na grade ficam **[nao verificado]**; o próprio brief deixa a exposição desse caso pelo calendário em aberto (`brief.md`, seção 4, caso 2).

## 18/11 09:17 · sessao 311b363a29e9 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

O censo é estático e pode incluir ocorrências de campos homônimos; resolução dinâmica e a completude do resultado ficam [não verificado].

```python
#!/usr/bin/env python3
"""Censo estático, somente leitura, de campos da projeção.

Uso: python3 censo.py /caminho/para/app

A saída reúne primeiro os arquivos fora de tests/ e depois os de tests/.
Ocorrências sintáticas de campos homônimos também são incluídas.
"""

import ast
import re
import sys
from pathlib import Path

CAMPOS = {
    "resposta_colab",
    "respondida_em",
    "validada_em",
    "validada_por",
    "materializada_em",
    "via_resolucao",
}

LOOKUPS = {
    "filter", "exclude", "get", "Q", "F",
    "values", "values_list", "order_by",
}
ESCRITAS = {"create", "update", "get_or_create", "update_or_create"}
RESULTADOS = set()


def trecho(linhas, numero):
    if 1 <= numero <= len(linhas):
        return linhas[numero - 1].strip().replace("\t", " ")
    return ""


def registrar(arquivo, linhas, linha, funcao, campo, modo):
    if campo not in CAMPOS or not linha:
        return
    teste = "sim" if "tests" in Path(arquivo).parts else "nao" [nao verificado]
    RESULTADOS.add(
        (teste, arquivo, linha, funcao, campo, modo, trecho(linhas, linha)) [nao verificado]
    )


def campo_lookup(valor):
    if not isinstance(valor, str):
        return None
    partes = valor.lstrip("-").split("__")
    for parte in partes:
        if parte in CAMPOS:
            return parte
    return None


def nome_chamada(no):
    if isinstance(no, ast.Name):
        return no.id
    if isinstance(no, ast.Attribute):
        return no.attr
    return None


def constantes(no):
    """Percorre literais e concatenações literais simples."""
    if isinstance(no, ast.Constant) and isinstance(no.value, str):
        yield no.value, no
    elif isinstance(no, (ast.List, ast.Tuple, ast.Set)):
        for item in no.elts:
            yield from constantes(item)
    elif isinstance(no, ast.Dict):
        for chave in no.keys:
            if chave is not None:
                yield from constantes(chave)
    elif isinstance(no, ast.BinOp) and isinstance(no.op, ast.Add):
        try:
            valor = ast.literal_eval(no)
        except (ValueError, TypeError, SyntaxError):
            return
        if isinstance(valor, str):
            yield valor, no


class Censo(ast.NodeVisitor):
    def __init__(self, arquivo, linhas):
        self.arquivo = arquivo
        self.linhas = linhas
        self.escopo = []
        self.aliases = [{}]
        self.valores = [{}]

    def qualificada(self):
        return ".".join(self.escopo) or "<modulo>"

    def emitir(self, no, campo, modo):
        registrar(
            self.arquivo, self.linhas, getattr(no, "lineno", 0),
            self.qualificada(), campo, modo,
        )

    def obter(self, pilha, nome):
        for escopo in reversed(pilha):
            if nome in escopo:
                return escopo[nome]
        return None

    def expandir(self, no, vistos=None):
        """Segue variáveis locais com atribuição literal, inclusive dicionários."""
        vistos = set() if vistos is None else vistos
        if isinstance(no, ast.Name) and no.id not in vistos:
            vistos.add(no.id)
            valor = self.obter(self.valores, no.id)
            if valor is not None:
                yield from self.expandir(valor, vistos)
                return
        if isinstance(no, ast.Dict):
            for chave, valor in zip(no.keys, no.values):
                if chave is None:
                    yield from self.expandir(valor, vistos.copy())
                else:
                    yield from constantes(chave)
            return
        yield from constantes(no)

    def nome_resolvido(self, no):
        if isinstance(no, ast.Name):
            return self.obter(self.aliases, no.id) or no.id
        if isinstance(no, ast.Attribute):
            return no.attr
        return None

    def visit_Import(self, no):
        for item in no.names:
            nome = item.asname or item.name.split(".")[0]
            self.aliases[-1][nome] = item.name.split(".")[-1]

    def visit_ImportFrom(self, no):
        for item in no.names:
            if item.name != "*":
                self.aliases[-1][item.asname or item.name] = item.name

    def visitar_escopo(self, no):
        self.escopo.append(no.name)
        self.aliases.append({})
        self.valores.append({})
        self.generic_visit(no)
        self.valores.pop()
        self.aliases.pop()
        self.escopo.pop()

    visit_FunctionDef = visitar_escopo
    visit_AsyncFunctionDef = visitar_escopo
    visit_ClassDef = visitar_escopo

    def visit_Assign(self, no):
        for alvo in no.targets:
            if isinstance(alvo, ast.Name):
                self.valores[-1][alvo.id] = no.value
        self.generic_visit(no)

    def visit_AnnAssign(self, no):
        if isinstance(no.target, ast.Name) and no.value is not None:
            self.valores[-1][no.target.id] = no.value
        self.generic_visit(no)

    def visit_Attribute(self, no):
        if no.attr in CAMPOS:
            if isinstance(no.ctx, ast.Load):
                self.emitir(no, no.attr, "L")
            elif isinstance(no.ctx, ast.Store):
                self.emitir(no, no.attr, "S")
        self.generic_visit(no)

    def visit_Call(self, no):
        nome = self.nome_resolvido(no.func)

        if nome in {"getattr", "setattr"} and len(no.args) >= 2:
            for valor, origem in self.expandir(no.args[1]):
                if valor in CAMPOS:
                    self.emitir(origem, valor, "L" if nome == "getattr" else "S")

        if nome in LOOKUPS:
            for argumento in no.args:
                for valor, origem in self.expandir(argumento):
                    campo = campo_lookup(valor)
                    if campo:
                        self.emitir(origem, campo, "O")
            for palavra in no.keywords:
                if palavra.arg is not None:
                    campo = campo_lookup(palavra.arg)
                    if campo:
                        self.emitir(palavra.value, campo, "O")
                else:
                    for valor, origem in self.expandir(palavra.value):
                        campo = campo_lookup(valor)
                        if campo:
                            self.emitir(origem, campo, "O")

        if nome in ESCRITAS:
            for palavra in no.keywords:
                if palavra.arg in CAMPOS:
                    self.emitir(palavra.value, palavra.arg, "S")
                elif palavra.arg in {"defaults", "create_defaults"} or palavra.arg is None:
                    for valor, origem in self.expandir(palavra.value):
                        if valor in CAMPOS:
                            self.emitir(origem, valor, "S")
            if nome == "update":
                for argumento in no.args:
                    for valor, origem in self.expandir(argumento):
                        if valor in CAMPOS:
                            self.emitir(origem, valor, "S")

        if nome == "save":
            for palavra in no.keywords:
                if palavra.arg == "update_fields":
                    for valor, origem in self.expandir(palavra.value):
                        if valor in CAMPOS:
                            self.emitir(origem, valor, "S")

        if nome == "carimbar":
            for palavra in no.keywords:
                if palavra.arg == "resposta":
                    for campo in (
                        "resposta_colab", "respondida_em",
                        "validada_em", "validada_por",
                        "materializada_em", "via_resolucao",
                    ):
                        self.emitir(palavra.value, campo, "S")

        self.generic_visit(no)


def ler_python(caminho, relativo):
    try:
        import tokenize
        with tokenize.open(caminho) as arquivo:
            texto = arquivo.read()
        arvore = ast.parse(texto, filename=relativo)
    except (OSError, UnicodeError, SyntaxError) as erro:
        print(f"{relativo}: leitura/análise falhou: {erro}", file=sys.stderr)
        return
    Censo(relativo, texto.splitlines()).visit(arvore)


def ler_template(caminho, relativo):
    try:
        texto = caminho.read_text(encoding="utf-8")
    except (OSError, UnicodeError) as erro:
        print(f"{relativo}: leitura falhou: {erro}", file=sys.stderr)
        return

    linhas = texto.splitlines()
    nomes = "|".join(sorted(map(re.escape, CAMPOS), key=len, reverse=True))
    campo_re = re.compile(r"\.\s*(" + nomes + r")\b")
    # Tags e filtros também podem ler propriedades, além de {{ ... }}.
    bloco_re = re.compile(r"{{.*?}}|{%.*?%}", re.DOTALL)
    for bloco in bloco_re.finditer(texto):
        for ocorrencia in campo_re.finditer(bloco.group()):
            posicao = bloco.start() + ocorrencia.start(1)
            linha = texto.count("\n", 0, posicao) + 1
            registrar(
                relativo, linhas, linha, "<template>",
                ocorrencia.group(1), "T",
            )


def main():
    if len(sys.argv) != 2:
        print("uso: python3 censo.py RAIZ", file=sys.stderr)
        return 2

    raiz = Path(sys.argv[1])
    if not raiz.is_dir():
        print(f"raiz não é diretório: {raiz}", file=sys.stderr)
        return 2

    for caminho in sorted(raiz.rglob("*")):
        if not caminho.is_file():
            continue
        relativo = caminho.relative_to(raiz).as_posix()
        if caminho.suffix == ".py":
            ler_python(caminho, relativo)
        elif caminho.suffix.lower() == ".html" and "templates" in caminho.parts:
            ler_template(caminho, relativo)

    print("arquivo\tlinha\tfuncao qualificada\tcampo\tmodo\ttrecho\tteste")
    for teste, arquivo, linha, funcao, campo, modo, texto in sorted( [nao verificado]
        RESULTADOS,
        key=lambda item: (
            item[0] == "sim", item[1], item[2],
            item[3], item[4], item[5], item[6],
        ),
    ):
        print(f"{arquivo}\t{linha}\t{funcao}\t{campo}\t{modo}\t{texto}\t{teste}") [nao verificado]
    return 0


if __name__ == "__main__":
    sys.exit(main())
```

## 18/11 09:14 · sessao 29157680a737 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## A Perguntas de lei

1. **Q1 — Absorção pelo ato ou pelo carimbo:** **DECIDIDO PELA LEI CORTES-F2-F9-1611.** M9b e M10 ficam no carimbo na F4; o predicado de absorção pelo ato pertence à E4d. Na cópia, a identificação continua usando texto e carimbo da projeção. [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509), [materializacao.py:1112](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1112).

2. **Q2 — Guardas de reparo do limbo na projeção ou no ato:** **DECIDIDO PELA LEI CORTES-F2-F9-1611.** E11, E14 e E17 ficam na projeção pela allowlist; sua retirada pertence à E4d. A guarda principal de E17 já usa o juiz de respondida, mas a identificação do limbo ainda lê a projeção. [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509), [disputa_emissao.py:1846](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1846), [disputa_emissao.py:1864](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1864).

A L-130 também decide o significado de “respondida” para materialização: instante e texto não vazio. A pergunta separada sobre ressuscitação devolver ao colaborador segue registrada como não decidida, mas não é uma das duas perguntas da F4 no brief e na matriz. [LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151), [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509).

perguntas de lei: 0 — nenhuma

## B Alcance nao verificado restante

- **Cascas do estado de aplicação — localizadas.** O fio calcula o estado e o modal o apresenta; a tag chama a mesma testemunha para o cartão incluído em `detalhe_local`. Tanto o fio quanto a tag têm tratamento próprio quando a testemunha retorna `None` e há texto. [fio.py:375](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:375), [modal_fio.html:230](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:230), [chamados_tags.py:131](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/templatetags/chamados_tags.py:131), [_pergunta_card.html:43](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_pergunta_card.html:43), [detalhe_local.html:310](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/detalhe_local.html:310).

- **Explicador — localizado.** `por_que_nao_fecha` percorre as perguntas; `explicar_pergunta` consulta a testemunha no ramo de pergunta considerada materializada de fato. O modal administrativo o alimenta com as perguntas carregadas. [explicador.py:91](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/explicador.py:91), [explicador.py:171](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/explicador.py:171), [fio.py:334](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:334).

- **Chamadores e leitores adicionais pós-BUG-PAR — localizados, sem afirmar exaustividade.** Além dos três caminhos antigos do derivador, `par_do_intervalo` o chama para as duas metades; há chamadas internas da testemunha para recusas de paridade e espera por declarado versus gravado. A gravação da irmã também chama `_materializar_uma`. [materializacao.py:214](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:214), [materializacao.py:222](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:222), [materializacao.py:339](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:339), [materializacao.py:757](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:757), [materializacao.py:1731](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1731).

- **Escritores e arquivos cruzados — localizados os pontos que alteram o censo antigo.** A porta `carimbar` continua sendo chamada com `resposta=` pela absorção, pelo resolver e pela conclusão da materialização; a foto da projeção declara `resposta_mantem_validacao=True`, assim como as portas automáticas, enquanto a humana declara `False`. `passivo_validado` conserva a foto de reversão e chama a unidade de materialização; `relavra_via` conserva suas portas próprias. [models.py:3222](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3222), [materializacao.py:1046](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1046), [disputa_emissao.py:2096](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:2096), [materializacao.py:1969](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1969), [models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143), [disputa_emissao.py:1525](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1525), [disputa_emissao.py:1679](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1679), [disputa_emissao.py:1701](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1701), [passivo_validado.py:64](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/passivo_validado.py:64), [passivo_validado.py:537](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/passivo_validado.py:537), [relavra_via.py:106](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/relavra_via.py:106).

- **[nao verificado] Censo global completo de R/V/C/VIA e chamadores.** Falta classificar, referência por referência, os acessos de produção diretos e indiretos, filtros ORM, aliases e portas de modelo contra o censo antigo; os pontos localizados acima não demonstram que essa enumeração está completa. [models.py:3371](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3371), [materializacao.py:2089](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:2089).

- **[nao verificado] Contrato F1 citado pela matriz.** Falta abrir o código de `atos_pergunta.py` da revisão F1 citada no anexo para conferir a assinatura e a passagem de `decisoes`/`agora` contra aquela revisão. Na cópia atual, a costura verificável ainda lê a projeção no parse de `data_hora` e chama o derivador para hora. [juizes.py:364](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:364), [juizes.py:375](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:375).

alcance nao verificado restante: 2

## 18/11 09:14 · sessao 4f326a9a3e3a · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## A Perguntas de lei

- **Q1 — O que conta como resposta e o que fazer com os pares incompletos? DECIDIDO PELA LEI L-130.** Respondida exige instante e texto não vazio; os pares incompletos são ilegais. A face SQL atual ainda lê a projeção, portanto a decisão da lei não comprova o caso de ato com projeção zerada. [LEIS.md:151](app/docs/LEIS.md#L151); [juizes.py:1575](app/chamados/juizes.py#L1575).

- **Q2 — Manter C na projeção ou criar agora um ato de conclusão? DECIDIDO PELO AVAL L1-L2-E4C-F1.** O ato de conclusão e seu escritor único pertencem à E4d; a data de morte da escrita dupla é o Contract. [BACKLOG.md:2009](app/docs/BACKLOG.md#L2009).

- **Q3 — A face SQL de validação é juiz novo? DECIDIDO PELO CORTE CORTES-F2-F9-1611.** É face SQL do juiz existente, condicionada a prova de equivalência por pergunta; publicar a função ou usar uma face existente onde servir é escolha técnica. O autor usado por I18 precisa continuar representado na solução: o filtro atual exclui usuários de sistema pela projeção. [CORTES.md:509](app/docs/CORTES.md#L509); [auditar_invariantes_chamados.py:1078](app/chamados/management/commands/auditar_invariantes_chamados.py#L1078).

- **Q4 — Reparos que medem a integridade da própria projeção devem ler a projeção? DECIDIDO PELO CORTE CORTES-F2-F9-1611.** O corte mantém as guardas de reparo da projeção por allowlist exata até a E4d. [CORTES.md:509](app/docs/CORTES.md#L509).

- **Q5 — A retirada do comando de reabertura e de sua porta exige uma lei nova? DECIDIDO PELA LEI L-111.** A cópia registra a retirada do comando e da porta com lápides. [LEIS.md:137](app/docs/LEIS.md#L137); [crons.py:835](app/config/crons.py#L835); [models.py:3429](app/chamados/models.py#L3429).

- **Q6 — `curar_perguntas_em_limbo` deve continuar reparando meio-estado da projeção ou passar a selecionar ato sem projeção? PERGUNTA DE LEI:** são **duas candidatas com universos diferentes**. O corte de Q4 decide reparos *da própria projeção*, mas não estabelece que este comando deva mudar de finalidade; hoje ele seleciona a projeção e exige igualdade com `MEDIDAS`. [CORTES.md:509](app/docs/CORTES.md#L509); [curar_perguntas_em_limbo.py:46](app/chamados/management/commands/curar_perguntas_em_limbo.py#L46). A matriz fornecida chama a mudança de universo de pergunta de desenho [nao verificado: linhas do anexo não disponíveis na cópia].

perguntas de lei: 1 — Q6

## B Alcance nao verificado restante

Os números abaixo contam **quatro itens de levantamento ainda abertos**, não uma quantidade de sítios R/V/C.

1. **Censo completo de escritores e leitores R/V/C nos arquivos cruzados — [nao verificado].** Há escritores e leitores fora dos comandos: a validação grava V em [validacao.py:168](app/chamados/services/validacao.py#L168); a API expõe R em [views.py:1163](app/api/views.py#L1163); templates leem R e V em [modal_fio.html:219](app/templates/chamados/partials/modal_fio.html#L219). Falta abrir e classificar **todos** os resultados de busca por campo, incluindo seleções ORM, acessos indiretos, serviços, views e templates, com seus chamadores.

2. **Importadores e selo global — [nao verificado].** Os três importadores nomeados estão em [relavrar_via_i6.py:47](app/chamados/management/commands/relavrar_via_i6.py#L47), [dedup_perguntas_fato.py:105](app/chamados/management/commands/dedup_perguntas_fato.py#L105) e [worklist.py:37](app/ponto/worklist.py#L37). O selo localizado cobre apenas `materializada_em` e `validada_em` em [test_contract_leitores_vitalidade.py:14](app/chamados/tests/test_contract_leitores_vitalidade.py#L14). Falta fechar o levantamento de todos os importadores e leitores das funções tocadas e classificar os demais selos; a existência desses três não prova exclusividade. A dependência proposta `services/atos_pergunta.py` **não foi localizada nesta cópia** [nao verificado: ausência em outras cópias].

3. **Campos novos e leituras de REABERTURA — [nao verificado].** A busca local encontrou declarações de `resposta_mantem_validacao` em [models.py:4143](app/chamados/models.py#L4143) e [disputa_emissao.py:1527](app/chamados/services/disputa_emissao.py#L1527), [disputa_emissao.py:1681](app/chamados/services/disputa_emissao.py#L1681) e [disputa_emissao.py:1703](app/chamados/services/disputa_emissao.py#L1703). O predicado da marca está em [models.py:4103](app/chamados/models.py#L4103) e a carga da tarefa o chama em [tarefa_do_dia.py:427](app/chamados/services/tarefa_do_dia.py#L427). Falta examinar os caminhos indiretos de `registrar`, as migrações e **todos** os leitores que interpretam REABERTURA para fechar o censo sem depender apenas da busca literal.

4. **Duas cascas e último ato dos casos raspados — [nao verificado].** A cópia mostra o payload da API em [views.py:1163](app/api/views.py#L1163), mas falta abrir o código da outra casca e verificar seus consumidores. O dado fornecido informa que 24 e 68 contam existência de ato, sem identificar o último ato; falta receber uma separação por último ato para interpretar esses casos. Essa separação não pode ser inferida do código nem dos totais fornecidos [nao verificado].

alcance nao verificado restante: 4

## 18/11 09:12 · sessao 3482713b3345 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Remedição incompleta.** Não consegui percorrer e classificar todas as premissas e todos os 37 degraus. As contagens globais de CONFERE, MOVEU e CAIU permanecem **[nao verificado]**. Os anexos `brief.md` e `matriz.md` foram fornecidos no texto da solicitação; não localizei esses arquivos na cópia, portanto suas linhas de arquivo também permanecem **[nao verificado]**. Abaixo, identifico os itens pelos números da matriz fornecida.

- **P1 — matriz D01: CAIU o prefiltro antigo.** A grade usa `q_respondida_com_texto()` em [reconciliar_grade.py:460](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_grade.py:460). Essa função ainda consulta `respondida_em` e `resposta_colab` da projeção, em [juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575). Assim, a troca não comprova o caso proposto de ato RESPOSTA com projeção zerada.
- **P2 — matriz D07–D10b: CAIU o comando de reabertura.** A lápide está em [crons.py:835](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/crons.py:835); a porta `reabrir_fantasma` também foi retirada, conforme [models.py:3429](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3429).
- **P3 — matriz D12/D13: CONFERE a seleção da projeção e a trava `MEDIDAS`**, em [curar_perguntas_em_limbo.py:46](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/curar_perguntas_em_limbo.py:46) e [curar_perguntas_em_limbo.py:48](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/curar_perguntas_em_limbo.py:48).
- **P4 — matriz D14: CAIU a leitura direta do texto para `com_resp`.** O rótulo passou a usar `pergunta_respondida`, em [dedup_perguntas_fato.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/dedup_perguntas_fato.py:76).
- **P5 — matriz D22, D26 e D34: CAÍRAM as leituras R descritas na matriz.** Esses pontos agora chamam `pergunta_respondida`, respectivamente em [auditar_invariantes_chamados.py:176](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:176), [auditar_invariantes_chamados.py:592](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:592) e [auditar_invariantes_chamados.py:1660](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1660).
- **P6 — matriz D32/D33: CAÍRAM os predicados R descritos.** I21 usa a face positiva em [auditar_invariantes_chamados.py:1576](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1576); C2 usa a negativa em [auditar_invariantes_chamados.py:1605](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1605), que inclui instante ausente **ou** instante sem texto, conforme [juizes.py:1583](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1583).
- **P7 — matriz D29, D35 e D36: MOVEU a numeração; os filtros V de projeção continuam**, em [auditar_invariantes_chamados.py:1077](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1077), [auditar_invariantes_chamados.py:1770](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1770) e [auditar_invariantes_chamados.py:1784](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1784).
- **P8 — matriz P7: CONFERE apenas o selo parcial de vitalidade.** Seus campos continuam sendo `materializada_em` e `validada_em` em [test_contract_leitores_vitalidade.py:14](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:14); a classificação de todos os selos existentes no repositório permanece **[nao verificado]**.

## 2 O que mudou

O patch fornecido cobre **fbc5c76ce → dd489c627**; ele não cobre, por si só, toda a distância entre a base do brief e o HEAD **[nao verificado]**. Isso importa porque I21, C2 e outros leitores já aparecem alterados no HEAD, embora seus hunks não constem do patch fornecido; os sítios atuais estão em [auditar_invariantes_chamados.py:1576](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1576) e [auditar_invariantes_chamados.py:1605](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1605).

No intervalo do patch, a grade trocou o filtro por instante pela face SQL do juiz L-130, mas continua dependente dos campos da projeção ([reconciliar_grade.py:460](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_grade.py:460), [juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575)). O backfill acrescentou `respondida_em` ao `only` usado na escolha da canônica, em [backfill_decisoes_pergunta.py:504](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_decisoes_pergunta.py:504).

O patch também introduziu `q_nao_respondida`, usada pela ressuscitação em [models.py:1529](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1529), e estendeu a guarda de anuência aos três ramos de materialização ([materializacao.py:1269](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1269), [materializacao.py:1342](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1342), [materializacao.py:1577](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1577), [materializacao.py:1661](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1661)).

Quanto aos campos novos de ato, `linhas_da_projecao_sem_linha` declara `resposta_mantem_validacao=True` em [models.py:4142](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4142); escritores em `disputa_emissao` declaram valores em [disputa_emissao.py:1527](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1527) e [disputa_emissao.py:1681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1681). A carga da tarefa lê `da_ressuscitacao` pelo predicado em [tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427). O censo completo de escritores e leitores novos permanece **[nao verificado]**.

## 3 Nao verificado de ALCANCE

- **[nao verificado]** Falta fechar o censo de escritores e leitores dos campos R/V/C em todos os arquivos cruzados e nas duas cascas. É necessário abrir os resultados de busca por esses campos e seus chamadores, incluindo views, serviços, templates e código da outra casca, se estiver na cópia.
- **[nao verificado]** Falta percorrer todos os importadores e leitores das funções tocadas. Os três importadores citados na matriz foram localizados em [relavrar_via_i6.py:47](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/relavrar_via_i6.py:47), [dedup_perguntas_fato.py:105](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/dedup_perguntas_fato.py:105) e [worklist.py:37](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/worklist.py:37); isso não prova que sejam os únicos.
- **[nao verificado]** A sonda fornecida conta existência de ato, não o último ato; faltaria a separação por último ato para interpretar os 24 e 68 casos. Não há acesso ao banco ou aos logs nesta tarefa.
- **[nao verificado]** Falta determinar todas as leituras de `resposta_mantem_validacao` e todos os caminhos que escrevem ou leem `da_ressuscitacao`; os sítios já abertos estão em [models.py:4066](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4066) e [tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427).

**alcance nao verificado restante: [nao verificado]** — não concluí o inventário necessário para uma contagem confiável.

## 4 Perguntas de lei

- **DECIDIDO PELA LEI L-130:** “respondida” exige instante e texto não vazio; a lei também trata os dois pares incompletos como ilegais, em [LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151).
- **DECIDIDO PELA LEI L-111:** a retirada de função sem chamador de produção está prevista em [LEIS.md:137](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:137); a lápide específica da porta está em [models.py:3429](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3429).
- **[nao verificado]** Não concluí o confronto de todas as alternativas da F9 com `LEIS.md`, `CORTES.md` e `DOSSIES.md`; não é possível afirmar que a lista de perguntas esteja fechada. O corte localizado sobre as respostas F2–F9 está em [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509).

perguntas de lei: [nao verificado]

## 18/11 09:12 · sessao f35777716841 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Remedição incompleta:** não percorri todos os itens do brief e da matriz. Os juízos abaixo são apenas os que consegui conferir; não sustentam uma contagem total de CONFERE, MOVEU e CAIU. [não verificado]

P1. **MOVEU — matriz M1, `_tipo_pelo_marco`**: o leitor de `resposta_colab` estava indicado em `materializacao.py:280`; está em [materializacao.py:546](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:546).

P2. **MOVEU — matriz M2, porta de escape**: a leitura de texto indicada em `materializacao.py:359` está em [materializacao.py:625](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:625).

P3. **MOVEU — matriz M5, hora fonte**: a leitura indicada em `materializacao.py:412` está em [materializacao.py:678](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:678).

P4. **MOVEU — matriz M7, estado de aplicação**: a função está em [materializacao.py:852](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:852); agora consulta `pergunta_respondida` antes de derivar o estado.

P5. **MOVEU — matriz M9b, registro de absorção**: a linha registra `ABSORCAO` e o texto da projeção em [materializacao.py:1048](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1048).

P6. **MOVEU — matriz M10, identificação de absorvidas**: a seleção por texto e carimbo está em [materializacao.py:1112](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1112).

P7. **MOVEU — matriz M23, fila de materialização**: a fila está em [materializacao.py:2089](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:2089); inclui as faces de respondida com texto e instante sem texto, esta última para recusa com dono.

P8. **MOVEU — matriz E11 e E14, guardas de reparo**: continuam lendo a projeção em [disputa_emissao.py:1664](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1664) e [disputa_emissao.py:1694](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1694).

P9. **CAIU parcialmente — matriz E17**: a guarda principal deixou de testar somente `respondida_em` e passou a chamar `pergunta_respondida`; a identificação de limbo continua lendo instante e texto da projeção em [disputa_emissao.py:1846](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1846) e [disputa_emissao.py:1864](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1864).

P10. **CONFERE com mudança de linha — matriz J1/J2, costura**: `_slot_esperado` chama o derivador para hora e conserva parse separado de `data_hora`, ambos ainda lendo a projeção em [juizes.py:364](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:364) e [juizes.py:375](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:375).

**Contagens totais:** [não verificado] — faltou julgar os demais sítios do brief e os demais degraus da matriz. Fornecer números fechados aqui seria inventá-los.

## 2 O que mudou

No patch fornecido para `520d7b557 → dd489c627`, a mudança central para F4 é a aplicação da **L-130**: respondida exige instante **e** texto; a lei aparece em [LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151) e o juiz em [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544).

- A recusa por falta de anuência passou a ser compartilhada pelos três ramos de materialização: hora, data e hora, e escolha. A fila também inclui o caso de instante sem texto para produzir essa recusa; isso altera o RED que pressupunha leitura apenas pelo carimbo. [materializacao.py:1269](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1269), [materializacao.py:2087](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:2087).
- `recobrar_questionario` passou à face SQL negada do juiz, e a guarda de reabertura do credor passou ao juiz de resposta. A cerca de competência também passou a envolver o resolvedor por chamado. [disputa_emissao.py:1737](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1737), [disputa_emissao.py:1846](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1846), [disputa_emissao.py:1904](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1904).
- Os escritores de RESPOSTA passaram a declarar `resposta_mantem_validacao`: a porta humana declara `False`, enquanto as duas portas automáticas e a foto da projeção declaram `True`. Isso afeta a interpretação futura dos atos da F4. [disputa_emissao.py:1525](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1525), [disputa_emissao.py:1679](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1679), [disputa_emissao.py:1701](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1701), [models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143).
- Entre os leitores novos ou alterados do par estão `canonica_do_grupo` e `merito_perdedor`, além da validação e da recusa de perguntas. [fio.py:1274](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1274), [fio.py:1287](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1287), [validacao.py:681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:681), [validacao.py:901](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:901).
- A cópia atual ainda lê texto da projeção no derivador e no parse `data_hora`; o contrato antigo de leitura pelo ato não pode ser tratado como já implementado nesses sítios. [materializacao.py:678](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:678), [materializacao.py:1580](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1580).

O levantamento de **todos** os escritores e leitores novos do patch permanece incompleto. [não verificado]

## 3 Nao verificado de ALCANCE

A tela do fio chama `estado_aplicacao_pergunta`, mas há outros leitores identificados: a tag de template e o explicador. A análise de ambas as cascas e de todos os caminhos que chegam a esses leitores ficou aberta; falta percorrer os chamadores e templates correspondentes. [fio.py:376](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:376), [chamados_tags.py:134](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/templatetags/chamados_tags.py:134), [explicador.py:122](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/explicador.py:122).

O censo completo dos campos R/V/C/VIA e dos chamadores das funções F4 ficou aberto: falta confrontar todas as referências de produção, inclusive acessos indiretos e consultas ORM, com o censo do brief. [não verificado]

**alcance nao verificado restante: [não verificado]** — não concluí a enumeração necessária para dar um número.

## 4 Perguntas de lei

- **DECIDIDO PELA LEI CORTES-F2-F9-1611:** absorção continua no carimbo na F4; o predicado de absorção pelo ato pertence à E4d. [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509).
- **DECIDIDO PELA LEI CORTES-F2-F9-1611:** as guardas de reparo do limbo ficam na projeção pela allowlist, com retirada prevista na E4d. [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509).
- U163(a) e A4 foram fornecidas como **respondidas** no anexo do pedido e não foram contadas como perguntas; a chave de colisão no instante exato está em [registro_batida.py:144](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/registro_batida.py:144).

Não concluí o exame de todas as premissas e, por isso, não posso afirmar que não surgiu outra pergunta de lei. [não verificado]

perguntas de lei: [não verificado]

## 18/11 09:12 · sessao ff0997790630 · E4C-F2-P2 · censo · origem=Code

**1. Padrões do script fornecido.** O anexo `censo_ast.py` não está como arquivo na cópia; as linhas abaixo são contadas a partir do texto fornecido. [nao verificado]

| Caso | Resultado |
|---|---|
| a) `f = PerguntaDisputa.descarimbar_em_lote; f(qs)` | **Pega a atribuição como `REF`**: o atributo tem nome-alvo e não é a função de uma chamada. A chamada posterior `f(qs)` não é identificada como chamada da função original. [censo_ast.py:81; censo_ast.py:82; censo_ast.py:83; censo_ast.py:90] |
| b) `from x import descarimbar_em_lote as d` | **Pega como `IMPORT:as d`**, pelo nome original do alias. [censo_ast.py:84; censo_ast.py:85] |
| c) `getattr(PD, 'descarimbar_em_lote')` | **Pega como `STRFUNC`**, pois a constante é igual ao nome-alvo. [censo_ast.py:87; censo_ast.py:88] |
| d) `from .fio import executar_sob_cerca` em `__init__.py` | **Pega como `IMPORT`**, inclusive sendo reexportação: a regra examina o alias de importação. [censo_ast.py:84; censo_ast.py:85] |
| e) `functools.partial(executar_sob_cerca, ...)` | **Pega como `REF`**: o nome-alvo é argumento de `partial`, não a função chamada. [censo_ast.py:81; censo_ast.py:82; censo_ast.py:83] |

**2. Referências na árvore.** Confirmei os seis imports sem `as`: quatro de `executar_sob_cerca` em [reconciliador.py:542](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:542), [reconciliador.py:1688](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:1688), [becos.py:65](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/becos.py:65) e [disputa_emissao.py:1904](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1904); dois de `vigiar_escrita_de_perguntas` em [models.py:3398](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3398) e [models.py:3565](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3565).

Na busca pelos nomes literais das seis funções em Python de `app/` e `bin/`, sem testes e migrations, **nenhuma referência indireta adicional concreta foi encontrada**. Referência construída dinamicamente sem o nome literal: [nao verificado]. O resolvedor genérico por caminho em [intake.py:481](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/intake.py:481) existe, mas sua ligação com uma das seis funções não foi demonstrada. [nao verificado]

**3. Os quatro arquivos de `bin/`.**

- `censo_folha.py` documenta uso no `saas_core` e consulta `PerguntaDisputa` por contagem; também grava um CSV em `/tmp`. Execução efetiva em PROD: [nao verificado]. [censo_folha.py:2](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/censo_folha.py:2); [censo_folha.py:28](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/censo_folha.py:28); [censo_folha.py:44](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/censo_folha.py:44).
- `sombra_regras_pessoais.py` fornece as regras que incluem `resposta_colab` e `ato_motivo`; a escrita genérica é feita por `sombra_mascara_pessoal.py`, inclusive por `bulk_update`. [sombra_regras_pessoais.py:81](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra_regras_pessoais.py:81); [sombra_regras_pessoais.py:142](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra_regras_pessoais.py:142); [sombra_mascara_pessoal.py:44](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra_mascara_pessoal.py:44).
- O cron declarado chama `sombra.sh --refazer`, que executa a máscara com settings de sombra; esses settings fixam banco `sombra` e host `sombra_db`. O deploy consulta `--conferir`. Escrita efetiva em PROD por outro acionamento: [nao verificado]. [crons.py:350](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/crons.py:350); [sombra.sh:182](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra.sh:182); [sombra.sh:272](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra.sh:272); [sombra.py:47](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/settings/sombra.py:47); [deploy.sh:163](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/deploy.sh:163).
- `esmeril_chamado_censo.py` declara análise AST sem Django nem banco; `reabrir_sistema` integra um conjunto de nomes de portas, não uma chamada. Execução efetiva: [nao verificado]. [esmeril_chamado_censo.py:31](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/esmeril_chamado_censo.py:31); [esmeril_chamado_censo.py:57](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/esmeril_chamado_censo.py:57).

A cópia não contém metadados Git para verificar o hash por `git rev-parse`: [nao verificado].

censo fechado: sim

## 18/11 09:12 · sessao 5e0199049006 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## A Perguntas de lei da F7

O brief e a matriz foram fornecidos como anexos no pedido, sem arquivo numerado na cópia; suas linhas são [nao verificado]. As candidatas abaixo reúnem os casos de lei e as escolhas de comportamento que eles apresentam. A busca foi feita nos quatro documentos indicados; a ausência de uma decisão específica é limitada ao texto encontrado nesses documentos.

- **Q1 — O que significa “respondida”, inclusive no caso de carimbo sem texto? DECIDIDO PELA LEI L-130.** Exige instante **e** texto não vazio; os dois meios-pares são ilegais para novas gravações. A lei também determina que reabrir ou ressuscitar zere o par inteiro e preserve o texto anterior na trilha. [LEIS.md:151](app/docs/LEIS.md:151).
- **Q2 — A conferência A20 acompanha qualquer par posterior? DECIDIDO PELO CORTE LU-08.** Ela vale somente para o par conferido. [CORTES.md:487](app/docs/CORTES.md:487).
- **Q3 — As fases públicas podem ser substituídas pela exigibilidade? DECIDIDO PELO E4C-D04-D05.** O contrato público conserva as fases; exigibilidade é campo novo aditivo, e validação sem efeito permanece em `aguarda_admin` até o efeito. [BACKLOG.md:2137](app/docs/BACKLOG.md:2137).
- **Q4 — R1b: `validada_em` projetado sem linha correspondente deve aparecer como “Validada”? PERGUNTA DE LEI — duas candidatas não decididas especificamente.** A leitura conservada apresentada no brief mostra a validação; a leitura pura apresentada no mesmo anexo não a mostra [nao verificado]. O D05 decide **conservar as fases do contrato público**, mas seu texto não determina qual das duas leituras governa essa divergência de dados nem diz que toda validação projetada sem linha deve ser conservada como valor; portanto, não basta para escolher a expectativa individual de R1b. [BACKLOG.md:2137](app/docs/BACKLOG.md:2137). O gerador existente identifica uma validação projetada sem linha correspondente, mas isso, por si, não escolhe o leitor público. [models.py:4122](app/chamados/models.py:4122).
- **Q5 — DEVOLUÇÃO deve reabrir o gate mesmo com validação projetada legada, e REABERTURA deve retirar resposta antiga? DECIDIDO PELA LEI L-130 e pelo E4C-D04-D05, dentro de seus respectivos alcances.** A lei manda zerar a resposta inteira na reabertura; D04 reconhece atos humanos separados por devolução e D05 conserva as fases. A implementação e a escolha da porta de leitura para produzir essas telas são dependências de código, não uma decisão adicional demonstrada por esses registros. [LEIS.md:151](app/docs/LEIS.md:151), [BACKLOG.md:2137](app/docs/BACKLOG.md:2137).
- **Q6 — `RESOLVIDA` deve significar efeito por identidade ou qualquer `materializada_em`?** A candidata é herdada da F2, segundo o brief [nao verificado]; o D05 conserva fases, mas não especifica esse predicado. O comportamento atual dá precedência a `validada_em` e `materializada_em`; a decisão histórica específica alegada pelo brief não foi localizada nos quatro documentos consultados [nao verificado]. Não a conto como pergunta própria da F7, pois o próprio escopo fornecido a atribui à F2. [models.py:3152](app/chamados/models.py:3152), [BACKLOG.md:2137](app/docs/BACKLOG.md:2137).
- **Q7 — Ressuscitar ou tratar resposta vazia deve devolver a ação ao colaborador? PERGUNTA DE LEI herdada, fora da F7.** A L-130 registra `RESSUSCITAR-DEVOLVE-AO-COLABORADOR` como não decidida; o corte F5 também registra a pergunta sobre resposta vazia junto dela. Não entra na contagem própria da F7. [LEIS.md:151](app/docs/LEIS.md:151), [CORTES.md:509](app/docs/CORTES.md:509).

A escolha do arquivo do acessor, prefetch, colisões, testes e medição de frota são questões de implementação ou evidência, não predicados novos de lei nos anexos [nao verificado]. [nao verificado]

perguntas de lei: 1 — Q4

## B Alcance nao verificado restante

1. **Cascas do card — caminho estático fechado; execução pendente.** `meu_atendimento` inclui o card nos dois loops e estende `base_tpl`; o context processor escolhe `base_app.html` ou `base.html`. A página de detalhe e as páginas de painel, benefícios e retrato estendem `base.html`; o lote abre no drawer do painel e o retrato encaminha ações ao modal do fio. [meu_atendimento.html:1](app/templates/chamados/meu_atendimento.html:1), [meu_atendimento.html:127](app/templates/chamados/meu_atendimento.html:127), [context_processors.py:210](app/core/context_processors.py:210), [detalhe_local.html:1](app/templates/chamados/detalhe_local.html:1), [painel_gestao.html:14](app/templates/chamados/painel_gestao.html:14), [fila_solicitacoes_beneficio.html:1](app/templates/colaboradores/fila_solicitacoes_beneficio.html:1), [retrato_folha.html:1](app/templates/folha/retrato_folha.html:1), [retrato_folha.py:990](app/folha/retrato_folha.py:990). Falta comprovar a renderização e os cliques nas duas cascas e nos fragmentos: **[nao verificado]**.

2. **Escritor de reabertura — fechado para a chamada questionada.** O corpo de `reabrir_para_correcao` limpa resposta e validação, registra diretamente uma decisão com a foto dos valores anteriores e chama a porta de limpeza em lote; ele **não chama** `linhas_da_projecao_sem_linha`. Assim, a foto de reabertura não deve ser confundida com a criação das linhas RESPOSTA/VALIDACAO ausentes. [disputa_emissao.py:1610](app/chamados/services/disputa_emissao.py:1610), [disputa_emissao.py:1632](app/chamados/services/disputa_emissao.py:1632), [disputa_emissao.py:1649](app/chamados/services/disputa_emissao.py:1649), [models.py:4122](app/chamados/models.py:4122).

3. **Leitores cruzados — parcialmente identificados; censo integral pendente.** O juiz de respondida lê o par projetado; `estado` prioriza carimbos; `resposta_display` e os filtros de apresentação ainda consultam a projeção. A API também publica texto e instante projetados. [juizes.py:1544](app/chamados/juizes.py:1544), [models.py:3152](app/chamados/models.py:3152), [models.py:3699](app/chamados/models.py:3699), [chamados_tags.py:132](app/chamados/templatetags/chamados_tags.py:132), [api/views.py:2561](app/api/views.py:2561). Falta classificar toda a árvore, inclusive consultas ORM, aliases e chamadas indiretas, para fechar a lista completa de leitores e escritores: **[nao verificado]**.

4. **Contrato F1 antigo — pendente.** A matriz fornecida descreve a assinatura e o fold de `atos_pergunta.leitura` [nao verificado]; a implementação histórica e seus leitores não foram localizados no código pesquisado desta cópia. O leitor efetivo de `respondida` aqui é a propriedade que delega ao juiz do par projetado. Falta abrir a árvore histórica ou receber o conteúdo daquele contrato para conferir implementação e chamadores. [models.py:3674](app/chamados/models.py:3674), [juizes.py:1544](app/chamados/juizes.py:1544).

5. **Frota U1/U2 — pendente.** O dado fornecido não contém a medição dessas duas populações [nao verificado]. Falta uma medição de produção com a definição e a porta de leitura efetivamente aplicáveis; os registros da L-130 sobre outros pares ilegais não são uma contagem U1/U2. [LEIS.md:151](app/docs/LEIS.md:151).

alcance nao verificado restante: 4

## 18/11 09:10 · sessao 58e2cbee3fbf · ESPURIA-TELA · censo · origem=Code

O consumo direto identificado de `do_dia` lê as três chaves pelo nome; não encontrei nesse consumo direto comparação, iteração ou serialização do dict inteiro [app/ponto/services/espelho.py:340, app/colaboradores/services/calendario.py:748, app/ponto/tests/test_palavra_parcial_e_pendente.py:74]. Há, porém, **um teste que serializa e compara o espelho inteiro**, incluindo seus dicts de dia: uma chave adicional pode fazê-lo falhar se tiver valores diferentes nas duas montagens [app/api/tests/test_atestado_por_minutos.py:104, app/api/tests/test_atestado_por_minutos.py:126, app/relatorios/pdf_espelho.py:602].

A coluna “quebra” abaixo considera **acrescentar uma chave sem alterar as chaves e valores existentes**. “Condicional” significa que a adição não basta, por si só, para provocar falha.

### Retorno direto de `do_dia`

| Chamador — arquivo:linha | Consumo | Quebra com chave nova? |
|---|---|---|
| Produção: `app/ponto/services/espelho.py:340` | `_r`; lê `veredito`, `palavra` e `cor` individualmente em `:344–345`. | Não: seleciona três nomes. |
| Produção: `app/colaboradores/services/calendario.py:748` | Import local com apelido `_do_dia` em `:738`; `_dd` fornece as três chaves individualmente em `:757`. | Não: seleciona três nomes. |
| Teste: `app/ponto/tests/test_palavra_parcial_e_pendente.py:74` | Atribui a `r` e lê valores por chave em `:76–77`. | Não: acessos nominais. |
| Teste: `app/ponto/tests/test_palavra_parcial_e_pendente.py:82` e `:83` | Atribui a `sem` e `com`; compara valores individuais em `:85–88`. | Não: acessos nominais. |
| Teste: `app/ponto/tests/test_palavra_parcial_e_pendente.py:92` | Atribui a `r`; lê valores por chave em `:94–95`. | Não: acessos nominais. |
| Teste: `app/ponto/tests/test_palavra_parcial_e_pendente.py:98` | Compara uma **tupla de três valores**, em `:99`, não o dict. | Não: a tupla seleciona nomes. |

O espelho grava as três chaves derivadas em cada dia em `app/ponto/services/espelho.py:344–345`; seu placeholder está em `:895`. O calendário constrói outro dict de dia com essas chaves em `app/colaboradores/services/calendario.py:754–757`.

### Leitores das chaves derivadas e do dia inteiro

| Leitor — arquivo:linha | Consumo | Quebra com chave nova no dict de dia? |
|---|---|---|
| API: `app/api/views.py:1388–1395`, `:1533–1543`, `:1595–1603` | Lê três campos do dia do espelho e monta dicts novos para o payload. Não espalha o dia original nesses pontos. | Não nesses pontos: seleciona campos por nome. |
| PDF: `app/relatorios/pdf_espelho.py:602`, `:636–645`, `:923–924` | Preserva os dicts de dia na lista coletada e lê `palavra_dia` por nome ao desenhar. | Não na leitura da palavra; o coletor **transporta** a chave adicional. |
| Tela do espelho: `app/ponto/views.py:160–162`, `:196`; `app/templates/ponto/espelho.html:151`, `:189`, `:298` | Repassa dias ao contexto e o template lê campos e listas internas. | Não nos acessos examinados: não exige conjunto exato de chaves. |
| Grade: `app/templates/colaboradores/partials/_calendario_grade.html:67–68`, `:97–98`, `:127`, `:135` | Itera a lista de dias e listas internas; lê os três campos por nome. | Não nesses acessos. |
| Comandos: `app/ponto/management/commands/e6_oraculo.py:404`; `app/ponto/management/commands/e6_piso_autopsia.py:145` | `.get('veredito_dia')`. | Não: acesso nominal. |
| Teste: `app/api/tests/test_atestado_por_minutos.py:104–106`, `:117`, `:126` | Serializa com `json.dumps(..., default=str, sort_keys=True)` o resultado do coletor, que inclui dias inteiros; compara as strings. | **Condicional**: valores novos diferentes entre as montagens alteram a igualdade; valores iguais não. |
| Teste: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:104`, `:116`, `:119–120` | Passa o dia inteiro ao template e faz `dict(dia, status_rotulo=...)`. | Não por adicionar chave: a cópia a preserva. |
| Testes: `app/api/tests/test_o21_app_diz_o_que_foi_decidido.py:92`, `:106`, `:138`, `:170`, `:180` | Leem e comparam campos nomeados ou tuplas desses valores. | Não: não comparam o dict inteiro nesses pontos. |
| Testes: `app/colaboradores/tests/test_calendario_mostra_o_carro.py:81`, `:140`, `:146`, `:164`; `app/ponto/tests/test_saida_orfa_dia_em_aberto.py:103` | Leem campos nomeados. | Não nesses acessos. |
| Testes: `app/relatorios/test_espelho_ausencia_pela_autoridade.py:54`, `:62`; `app/relatorios/tests/test_rotulo_ausencia_pdf.py:31` | Leem `palavra_dia` por nome. | Não nesses acessos. |
| Testes: `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:214`, `:243`, `:303`; `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:202`, `:277`, `:300` | Leem campos nomeados; `:214` itera o mapa de dias, não as chaves de cada dia. | Não nesses acessos. |
| Testes: `app/relatorios/tests/test_palavra_do_dia.py:131–141`, `:161–191`, `:201–214`, `:234–252`, `:266–274`, `:290–304`, `:371–401` | Leem e comparam valores das três chaves por nome. | Não nesses acessos. |

Não foi identificada iteração Django da forma `dia.items`, `dia.keys` ou `dia.values`; a grade itera `dias` em `app/templates/colaboradores/partials/_calendario_grade.html:67`. A ausência de iteração equivalente com apelidos arbitrários em todo JavaScript, inclusive código de terceiros, permanece **[não verificado]**. O `ausencia_id` encontrado nos templates é lido de outros objetos, por exemplo `h` em `app/templates/ponto/partials/painel_ausencia.html:191` e contexto de chamado em `app/templates/chamados/partials/_card_atendimento.html:70`.

A existência de outros caminhos indiretos que comparem o conjunto exato de chaves, serializem em cache ou pickle, ou recebam o dia por aliases não rastreados permanece **[não verificado]**. Em particular, a passagem do dia inteiro torna insuficiente concluir que todo consumidor posterior ignora novas chaves a partir apenas dos acessos nominais acima [app/relatorios/pdf_espelho.py:602, app/ponto/views.py:196].

### Colisões de nome

- Nos literais de primeiro nível do dia examinados, não aparecem `quem`, `quando`, `ausencia_id`, `decidido_quem`, `decidido_quando` ou `decidido_ausencia_id` [app/ponto/services/espelho.py:845, app/ponto/services/espelho.py:870–895, app/colaboradores/services/calendario.py:754–785]. A ausência de escrita indireta posterior de todas essas chaves permanece **[não verificado]**.
- Há nomes em estruturas **aninhadas**, que não são colisões no primeiro nível: o calendário inclui `veredito_humano` [app/colaboradores/services/calendario.py:770], cuja estrutura relacionada contém `quem` e `quando` [app/ponto/services/veredito_celula.py:129–131]. O espelho transporta `_decisao_do_dia` [app/ponto/services/espelho.py:871]; a estrutura relacionada contém `ausencia_id`, `quem` e `quando` [app/ponto/services/dia_decidido.py:537–539], e a chave de transporte é removida quando `limpar=True` [app/ponto/services/espelho.py:346–349].

### Script AST

O script abaixo foi executado por stdin, sem criar arquivo. Ele resolve os imports estáticos indicados, imprime chamadas e destinos, acessos às três chaves e **candidatos** para inspeção manual. Sua análise de nomes é conservadora: não resolve fluxo entre funções, aliases dinâmicos, reatribuições por escopo ou acesso reflexivo; portanto, os candidatos não constituem prova de censo fechado [não verificado]. A execução produziu saída truncada pela ferramenta; sua conclusão integral não foi observada [não verificado].

```python
import ast
from pathlib import Path
from collections import Counter

K = {'veredito_dia', 'palavra_dia', 'cor_dia'}
c = Counter()

for f in sorted(Path('app').rglob('*.py')):
    try:
        t = ast.parse(f.read_bytes(), filename=str(f))
    except (SyntaxError, UnicodeDecodeError) as e:
        print('ERRO', f, e)
        continue

    imp = {}
    parent = {}
    for n in ast.walk(t):
        for x in ast.iter_child_nodes(n):
            parent[x] = n
        if isinstance(n, ast.Import):
            for a in n.names:
                imp[a.asname or a.name.split('.')[0]] = (
                    a.name if a.asname else a.name.split('.')[0]
                )
        if isinstance(n, ast.ImportFrom):
            for a in n.names:
                imp[a.asname or a.name] = (n.module or '') + '.' + a.name

    def q(n):
        if isinstance(n, ast.Name):
            return imp.get(n.id, n.id)
        if isinstance(n, ast.Attribute):
            return q(n.value) + '.' + n.attr
        return ''

    tracked = set()
    for n in ast.walk(t):
        if (isinstance(n, ast.Call)
                and q(n.func) == 'ponto.services.dia_decidido.do_dia'):
            p = parent.get(n)
            c['calls'] += 1
            print(f, n.lineno, 'CALL', ast.unparse(
                p if isinstance(p, (ast.Assign, ast.AnnAssign, ast.Return))
                else n
            ))
            if isinstance(p, (ast.Assign, ast.AnnAssign)):
                targets = p.targets if isinstance(p, ast.Assign) else [p.target]
                for x in targets:
                    tracked.update(
                        y.id for y in ast.walk(x) if isinstance(y, ast.Name)
                    )

        value = None
        if (isinstance(n, ast.Subscript)
                and isinstance(n.slice, ast.Constant)
                and n.slice.value in K):
            value = n.value
        if (isinstance(n, ast.Call)
                and isinstance(n.func, ast.Attribute)
                and n.func.attr == 'get'
                and n.args
                and isinstance(n.args[0], ast.Constant)
                and n.args[0].value in K):
            value = n.func.value
        if value is not None:
            c['key'] += 1
            print(f, n.lineno, 'KEY', ast.unparse(n))
            tracked.update(
                x.id for x in ast.walk(value) if isinstance(x, ast.Name)
            )

    for n in ast.walk(t):
        candidate = isinstance(
            n, (ast.Compare, ast.For, ast.comprehension, ast.Call, ast.Dict)
        )
        names = {x.id for x in ast.walk(n) if isinstance(x, ast.Name)}
        if candidate and tracked.intersection(names):
            c['candidate'] += 1
            print(
                f, getattr(n, 'lineno', '?'), 'CANDIDATO',
                ast.unparse(n).replace('\n', ' ')[:500]
            )

print('TOTAIS', dict(c))
```

`N_quebra=1` — contado como **um leitor com quebra condicional identificada**, o teste de serialização e igualdade [app/api/tests/test_atestado_por_minutos.py:104–106, :126]; não é um número comprovado de falhas inevitáveis nem um limite superior para leitores ainda não verificados. [nao verificado]

`censo fechado: nao`

## 18/11 09:10 · sessao 9a2fed870b4c · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Resultado:** a leitura direta de R caiu em U3, U6, U7 e U8, mas os substitutos consultam o juiz L-130 sobre a **projeção**, não a face histórica de atos descrita no brief e na matriz ([cartorio.py:1642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1642), [calendario.py:505](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:505), [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544)).

Os anexos `brief.md` e `matriz.md` foram fornecidos no texto da pergunta, sem arquivos ou numeração de linhas disponíveis na cópia: suas linhas físicas são **[não verificado]**. Identifico abaixo cada sítio pelo número U e pela linha histórica que o próprio anexo informa. Para as contagens, cada linha P1–P34 é uma unidade: P1–P24 cobrem os 24 degraus da matriz e as nove unidades do brief; P25–P34 cobrem os dez itens do contrato de saída. **CAIU** significa que a premissa descrita deixou de valer ou que o requisito ainda não está demonstrado, conforme indicado em cada linha.

| # | Premissa e sítio no brief/matriz | Julgamento no dd489c627 |
|---|---|---|
| P1 | U1a, matriz `procedencia:62`: exclui batida de app. | **CONFERE** — [procedencia_batida.py:62](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:62). |
| P2 | U1b, `:70–71`: busca perguntas em lote; teste antigo mede uma query. | **CONFERE** — [procedencia_batida.py:70](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:70), [test_procedencia_batida.py:126](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_procedencia_batida.py:126). O teste não prova custo de uma futura leitura de atos conservada. |
| P3 | U1c, `:72`: autor V vem da projeção. | **CONFERE** — [procedencia_batida.py:72](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:72). |
| P4 | U1d, `:72`: autor C é fallback da projeção. | **CONFERE** — [procedencia_batida.py:72](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:72). |
| P5 | U1e, `:74`: instante V vem da projeção. | **CONFERE** — [procedencia_batida.py:74](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:74). |
| P6 | U1f, `:74`: instante C é fallback da projeção. | **CONFERE** — [procedencia_batida.py:74](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:74). |
| P7 | U1g, `:86/90/93`: frases para carimbo, ausência de carimbo e ausência de pergunta. | **CONFERE** — [procedencia_batida.py:86](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:86). |
| P8 | U2a, `flip_auto:129`: filtro de via de paridade recusada. | **CONFERE** — [flip_auto.py:129](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:129). |
| P9 | U2b, `:126/130`: relógio interno e filtro de V. | **CONFERE** — [flip_auto.py:126](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:126). |
| P10 | U2c, `:132`: regex lê motivo técnico da projeção. | **CONFERE** — [flip_auto.py:132](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:132); escritor técnico em [materializacao.py:1689](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1689). |
| P11 | U2d, `:148`: deduplicação por batida. | **CONFERE** — [flip_auto.py:144](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:144). |
| P12 | U3x, `cartorio:1641/1648`: leitura direta de R isenta o lado x. | **CAIU** — o uso existe, mas chama o juiz em [cartorio.py:1642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1642), usado em [cartorio.py:1649](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1649). |
| P13 | U3y, `:1641/1665`: mesma leitura direta isenta o lado y. | **CAIU** — juiz em [cartorio.py:1642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1642), uso em [cartorio.py:1666](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1666). |
| P14 | U4, `cartorio:1650`: todas as vias são duplicata. | **MOVEU** — [cartorio.py:1651](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1651). |
| P15 | U5a, `veredito_celula:180–181`: universo `q_vivas()`. | **CONFERE** — [veredito_celula.py:180](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:180). |
| P16 | U5b, `:196`: SILENCIO recebe o carimbo C do ato recém-fechado. | **CONFERE** — [veredito_celula.py:190](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:190), [veredito_celula.py:196](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:196). |
| P17 | U5c, `:197`: SILENCIO copia R da projeção. | **CONFERE** — [veredito_celula.py:197](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:197). |
| P18 | U5d, matriz: `registrar` é idempotente. | **CAIU** — a proteção da repetição está na porta `fechar_por_admin`, chamada em [veredito_celula.py:190](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:190); `registrar` usa `bulk_create` em [models.py:4152](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4152). |
| P19 | U6a, brief E3 `calendario:504`: `.exclude(resposta_colab='')`. | **CAIU** — substituído pela face SQL do juiz em [calendario.py:499](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:499) e [calendario.py:505](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:505). |
| P20 | U6b, E3 `:506/509`: juiz de vida recebe relógio e oferta conhecida. | **MOVEU** — [calendario.py:507](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:507), [calendario.py:510](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:510). |
| P21 | U7a, `ficha:171`: contagem lê R diretamente. | **CAIU** — usa o juiz em [ficha.py:171](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:171). |
| P22 | U7b, matriz `models:1873/1893`: método cruzado lê projeção. | **MOVEU** e mudou o predicado de R — [models.py:1898](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1898), [models.py:1918](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1918), [models.py:1932](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1932). |
| P23 | U8, `previa:141–142`: lê R diretamente. | **CAIU** — usa o juiz em [previa.py:141](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/previa.py:141). |
| P24 | U9, matriz `dia_do_colab:69–70`: serializa R da projeção. | **MOVEU** — continua direto em [dia_do_colab.py:70](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_do_colab.py:70). |
| P25 | Contrato da matriz P1: todos usam face pública conservada de atos. | **CAIU** — as faces históricas não foram encontradas nesta cópia; os leitores migrados usam o juiz da projeção, [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544), [juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575). |
| P26 | Contrato P2: paridade dos cinco leitores inclusive para espaços. | **CAIU** — U9 devolve texto cru em [dia_do_colab.py:70](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_do_colab.py:70); os leitores migrados usam instante e texto aparado, [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544). Comparar retorno textual com booleano exige definir a interpretação do teste. |
| P27 | Contrato P3: motivo técnico permanece na projeção. | **CONFERE** — [flip_auto.py:132](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:132). |
| P28 | Contrato P4: via e C permanecem na projeção. | **CONFERE** — [flip_auto.py:129](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:129), [cartorio.py:1651](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1651), [procedencia_batida.py:72](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:72). |
| P29 | Contrato P5: prova de custo constante em N perguntas carimbadas. | **CAIU a suficiência da prova apontada**, não uma medição de custo: o teste antigo mede a implementação atual; custo de uma futura face conservada em N carimbadas **[não verificado]** ([test_procedencia_batida.py:126](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_procedencia_batida.py:126)). |
| P30 | Contrato P6: SILENCIO usa R corrente da face conservada. | **CAIU** — ainda copia a projeção em [veredito_celula.py:197](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:197). |
| P31 | Contrato P7: U2 recebe `agora` do chamador. | **CAIU** — chama `_tz.now()` internamente em [flip_auto.py:126](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:126). |
| P32 | Contrato P8: selo AST impede leituras diretas fora da allowlist. | **CAIU como propriedade do código atual** — U5 e U9 ainda leem R diretamente ([veredito_celula.py:197](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:197), [dia_do_colab.py:70](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_do_colab.py:70)). Existência de um selo com a allowlist proposta **[não verificado]**. |
| P33 | Contrato P9: pinos registrados para os três sítios. | **CONFERE quanto aos valores presentes** — [test_medida_turno_do_par.py:275](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/tests/test_medida_turno_do_par.py:275), [test_medida_turno_do_par.py:319](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/tests/test_medida_turno_do_par.py:319), [test_medida_turno_do_par.py:330](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/tests/test_medida_turno_do_par.py:330). |
| P34 | Contrato P10: F8 sobre R3 e a antiga F1 conservada. | **CAIU como premissa conjunta** — a forma R3 com `agora` está em [calendario.py:188](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:188), mas a face histórica de atos não foi encontrada; a face SQL atual lê projeção ([juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575)). A ordem histórica de pousos **[não verificado]**. |

**CONFERE 17, MOVEU 4, CAIU 13.** Essas contagens são das 34 unidades explicitadas acima; não são uma contagem de cada afirmação histórica de frota ou de cada teste citado nos anexos. [nao verificado]

## 2 O que mudou

O patch fornecido compara `fbc5c76ce` com `dd489c627`; a cópia não tem `.git`, portanto a comparação aqui é a do patch anexado, confrontada com a árvore atual. O diffstat e o patch são dados fornecidos, sem arquivo:linha na cópia **[não verificado]**.

- **Quatro leitores F8 mudaram:** cartório, calendário, ficha e prévia passaram de texto direto para `pergunta_respondida` ou sua face SQL ([cartorio.py:1642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1642), [calendario.py:505](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:505), [ficha.py:172](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:172), [previa.py:142](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/previa.py:142)). O juiz exige instante **e** texto após trim, ambos da projeção ([juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544)). Assim, a mudança já afeta texto sem instante e espaços; ela não implementa a antiga leitura da sequência de atos.
- **O leitor cruzado da ficha mudou junto:** `sem_veredito_entre` substituiu o antigo “instante ou texto” pelo juiz, mas continua usando V e C da projeção ([models.py:1918](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1918)).
- **Novos leitores do juiz no patch** incluem API ([api/views.py:2578](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2578), [api/views.py:2622](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2622)), telas de chamados ([chamados/views.py:1281](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1281)), validação ([validacao.py:682](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:682)) e materialização ([materializacao.py:872](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:872)). A nova guarda de anuência chama `_carimbar_recusa`, de modo que também pode escrever o motivo técnico lido por U2 ([materializacao.py:1287](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1287), [materializacao.py:1689](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1689)).
- **O inventário “dois escritores” do brief não deve ser tratado como dois sítios de código:** além da recusa administrativa, há a porta técnica e uma atribuição técnica direta ([validacao.py:916](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:916), [materializacao.py:1689](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1689), [materializacao.py:1352](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1352)).
- **F2 acrescentou marcas na decisão:** a fotografia sintética escreve `resposta_mantem_validacao=True`; escritores de resposta humana e automática declaram seus valores ([models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143), [disputa_emissao.py:1527](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1527), [disputa_emissao.py:1681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1681)). A tarefa passou a excluir da sequência a reabertura marcada pelo predicado novo ([models.py:4103](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4103), [tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427)).
- O patch também altera o fluxo da porta de exportação e o tratamento de feriado do motor intermitente ([folha/views.py:128](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/views.py:128), [motor_calculo_v2.py:3578](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:3578)). Não há, nesses trechos, uma troca dos leitores R/V/C de F8.

Os números raspados — inclusive 187 perguntas, seis batidas e as contagens da fila — são observações dos anexos sobre árvores e momentos anteriores; seu valor em produção ou nesta árvore **[não verificado]**. Não é possível concluir neutralidade atual de frota apenas a partir do código.

## 3 Nao verificado de ALCANCE

- **Fechado: leitor cruzado da ficha.** `ficha_do_colab` chama `respostas_sem_veredito`, que delega a `sem_veredito_entre`; este lê V/C e o juiz de R ([ficha.py:169](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:169), [models.py:1898](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1898), [models.py:1932](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1932)).
- **Fechado para os chamadores citados do calendário e procedência:** tela, drawer, relatório e tarefa chegam ao calendário; calendário e espelho chamam procedência ([colaboradores/views.py:1990](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/views.py:1990), [drawer.py:123](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/drawer.py:123), [furos.py:164](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/furos.py:164), [tarefa_do_dia.py:619](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:619), [calendario.py:651](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:651), [espelho.py:756](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:756)).
- **Fechado para as duas cascas citadas:** mês e drawer incluem a grade; a grade lê procedência, e o PDF lê a frase de papel ([calendario_partial.html:67](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/colaboradores/calendario_partial.html:67), [_drawer_diagnostico.html:35](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/colaboradores/partials/_drawer_diagnostico.html:35), [_calendario_grade.html:127](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/colaboradores/partials/_calendario_grade.html:127), [pdf_espelho.py:950](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/pdf_espelho.py:950)).
- **Aberto 1 — censo global completo de escritores e leitores dos campos.** Os sítios acima e os novos leitores do patch foram localizados, mas não foi refeito o censo global citado pelo brief. Falta percorrer todas as ocorrências e aliases de `resposta_colab`, `respondida_em`, V, C, via e motivo em `app/`; há inclusive leitores diretos em templates de chamados ([modal_fio.html:219](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:219)).
- **Aberto 2 — alcance do selo de queries para N perguntas carimbadas.** Há testes de custo do calendário ([test_e4cal_l1_calendario_le_o_d4.py:230](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:230)), mas não ficou demonstrado um teste que varie especificamente N perguntas com R e V carimbadas. Falta abrir e confrontar os testes de queries dos demais apps com esse cenário.
- **Aberto 3 — efeito de `respondida_em > agora` no calendário.** O calendário passa `agora` ao juiz de vida ([calendario.py:507](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:507)), enquanto a face de resposta não recebe relógio ([juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/jui

[truncado em 20000 de 21243 caracteres]

## 18/11 09:10 · sessao b24a819bcba2 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Critério da contagem:** cada P corresponde a um dos 18 degraus da matriz, incluindo as 12 unidades do brief, o ramo APAGA e os dois itens SOME. **CONFERE** significa que o sítio e a operação descritos permanecem; **MOVEU** inclui deslocamento de linha ou substituição da operação no mesmo fluxo; **CAIU** significa que não foi encontrado sítio correspondente. A classificação descreve o código atual, não afirma que o alvo F6 foi implementado.

Os anexos `brief.md` e `matriz.md` foram fornecidos no texto da pergunta, sem arquivos físicos numerados na cópia. Identifico cada linha pela unidade e pelo sítio antigo literalmente declarado no anexo; o número da linha *do próprio anexo* é **[nao verificado]**.

- **P1 — U1a, CONFERE.** Brief U1 e matriz U1a, `views_mensageria.py:567`: `_item_validar` ainda entrega `resposta_colab` cru. [app/api/views_mensageria.py:567](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views_mensageria.py:567).
- **P2 — U1b, CONFERE.** Brief U1 e matriz U1b, linha antiga 569: `respondida_em.isoformat()` continua sem guarda. O universo atual exige instante e texto; o 500 previsto para a F1 antiga não está demonstrado nesse universo atual. [app/api/views_mensageria.py:539](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views_mensageria.py:539); [app/chamados/juizes.py:1761](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1761).
- **P3 — U1c, CONFERE.** Brief U1 e matriz U1c, linha antiga 570: o leitor ainda usa `timezone.now()` e subtrai o instante cru. [app/api/views_mensageria.py:570](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views_mensageria.py:570).
- **P4 — U2, CONFERE.** Brief U2 e matriz U2, linhas antigas 1163–1165: o JSON ainda lê texto e instante da projeção e `resposta_display` da propriedade. [app/api/views.py:1163](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:1163); [app/chamados/models.py:3699](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3699).
- **P5 — U3, CONFERE.** Brief U3 e matriz U3, linhas antigas 2561–2563: o JSON ainda lê os mesmos campos da projeção. [app/api/views.py:2561](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2561).
- **P6 — U4, MOVEU.** Brief U4 e matriz U4, linha antiga 2553: a comparação com `estado == RESPONDIDA` permanece, mas `estado` agora usa a propriedade `respondida`, que delega ao juiz L-130; ainda não lê a tarefa do dia. [app/api/views.py:2550](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2550); [app/chamados/models.py:3152](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3152); [app/chamados/models.py:3674](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3674).
- **P7 — U5, MOVEU.** Brief U5 e matriz U5, linha antiga 2577: `respondidas` usa `pergunta_respondida`, não mais apenas instante não nulo. [app/api/views.py:2578](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2578); [app/chamados/juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544).
- **P8 — U6, MOVEU.** Brief U6 e matriz U6, linhas antigas 2620 e 2637: `ja_respondida` usa o juiz; o JSON ainda lê o instante recém-gravado. A escolha também afeta o log e a tag do follow-up. [app/api/views.py:2622](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2622); [app/api/views.py:2639](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2639); [app/chamados/services/disputa_emissao.py:1547](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1547); [app/chamados/services/disputa_emissao.py:1562](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1562).
- **P9 — U7a, MOVEU.** Brief U7 e matriz U7a, linha antiga 169: `pendentes` usa o juiz e alimenta a porta de validação. [app/chamados/views_cobrar.py:168](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:168); [app/chamados/views_cobrar.py:203](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:203).
- **P10 — U7b, MOVEU.** Brief U7 e matriz U7b, linha antiga 175: `_aguard` usa `q_nao_respondida`; continua sem filtro de vitalidade e conta também perguntas mortas que satisfaçam a face SQL. [app/chamados/views_cobrar.py:176](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:176); [app/chamados/juizes.py:1583](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1583).
- **P11 — U8, MOVEU.** Brief U8 e matriz U8, linha antiga 291: o sítio passou à linha 293 e continua lendo `resposta_colab` cru. [app/chamados/views_cobrar.py:293](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:293).
- **P12 — U9a, MOVEU.** Brief U9 e matriz U9a, linha antiga 1280: a view web passa o resultado do juiz ao core, com o mesmo efeito sobre o rótulo persistido de P8. [app/chamados/views.py:1280](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1280); [app/chamados/services/disputa_emissao.py:1562](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1562).
- **P13 — U9b / APAGA, CONFERE.** Brief APAGA e matriz U9b, linhas antigas 1256–1264: o ramo ainda existe; a porta anterior elimina a pergunta não viva, sem mutação entre as duas verificações. Portanto a remoção pedida ainda resta. [app/chamados/views.py:1238](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1238); [app/chamados/views.py:1256](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1256).
- **P14 — U10, MOVEU.** Brief U10 e matriz U10, linha antiga 1416: `_sem_resp` usa `q_nao_respondida`; o resultado e `perguntas_corrigiveis` escolhem recobrança ou reabertura. [app/chamados/views.py:1417](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1417); [app/chamados/juizes.py:1753](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1753).
- **P15 — U11, MOVEU.** Brief U11 e matriz U11, linha antiga 64: canal usa a face negada do juiz, cujo universo inclui texto sem instante e instante sem texto, em vez de `resposta_colab=''`. [app/chamados/canal.py:64](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/canal.py:64); [app/chamados/juizes.py:1591](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1591).
- **P16 — U12, MOVEU.** Brief U12 e matriz U12, linha antiga 142: o filtro está na linha 145 e a guarda de texto cru na 166; o classificador também lê a projeção. [app/chamados/templatetags/chamados_tags.py:145](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/templatetags/chamados_tags.py:145); [app/chamados/juizes.py:2034](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:2034).
- **P17 — S1 / SOME, CONFERE.** Brief SOME e matriz S1, linhas antigas 11–28: o relatório delega a `pode_fechar` e `aguarda_o_colaborador`; `coletar` filtra `fechada_em`. [app/chamados/relatorios_disputa.py:23](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/relatorios_disputa.py:23); [app/chamados/relatorios_disputa.py:46](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/relatorios_disputa.py:46).
- **P18 — S2 / SOME, CONFERE.** Brief SOME e matriz S2, linhas antigas 1174–1181: `pode_solicitar_correcao` segue delegado a `fio.aguarda_o_colaborador`. [app/api/views.py:1174](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:1174).

**CONFERE 8, MOVEU 10, CAIU 0.** Essas contagens são dos 18 sítios acima; não contam como concluído o alvo de migração dos que ainda leem projeção.

## 2 O que mudou

O patch fornecido é o diff `fbc5c76ce..dd489c627`; a cópia não fornece `.git`, portanto a comparação desta seção vem do patch anexado e da conferência dos sítios atuais. O outro hash mencionado no pedido, `520d7b557`, não tem patch comparativo próprio no dado fornecido: **[nao verificado]** o seu delta específico.

- **Juiz e leitores:** a alteração relevante para F6 troca os filtros de U5, U6, U7a, U7b, U9a, U10 e U11 para `pergunta_respondida` ou sua face negada. O juiz exige **instante e texto após trim**; as duas metades isoladas não contam como resposta. Isso altera também `estado` e as condições de exibição dos dois templates. [app/chamados/juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544); [app/chamados/models.py:3152](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3152); [app/templates/chamados/partials/_pergunta_card.html:24](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_pergunta_card.html:24); [app/templates/chamados/partials/modal_fio.html:217](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:217).
- **Leitores novos no delta:** além das views e do canal, a face negada passou a alimentar a contagem de `recobrar_questionario` e o universo de ressuscitação; `perguntas_corrigiveis` passou à face positiva. Assim, a seleção do escritor em U10 e a contagem interna da recobrança mudam juntas. [app/chamados/services/disputa_emissao.py:1737](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1737); [app/chamados/models.py:1529](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1529); [app/chamados/juizes.py:1753](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1753).
- **Campos novos da decisão:** `resposta_mantem_validacao` é declarado pela resposta humana como falso e pelas respostas automática, pela trilha e pela foto da projeção como verdadeiro. `da_ressuscitacao` é lido pela carga da tarefa por `es_da_ressuscitacao`; o código o descreve como inerte até um escritor posterior marcar verdadeiro. Isso muda a preparação da autoridade que U4 pretende ler, não faz U4 já lê-la. [app/chamados/models.py:4067](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4067); [app/chamados/models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143); [app/chamados/services/disputa_emissao.py:1527](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1527); [app/chamados/services/disputa_emissao.py:1681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1681); [app/chamados/services/disputa_emissao.py:1703](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1703); [app/chamados/services/tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427).
- **Cerca e remoção lateral:** o patch acrescenta vigilância de competência na escrita e no lote e cerca o resolver; também remove `reabrir_fantasma` com lápide. São mudanças nas portas cruzadas, não a remoção U9b, que permanece. [app/chamados/models.py:3394](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3394); [app/chamados/models.py:3559](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3559); [app/chamados/services/disputa_emissao.py:1901](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1901); [app/chamados/models.py:3426](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3426); [app/chamados/views.py:1256](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1256).
- **Fora da F6:** o hunk de `core/juizes.py` baixa um pendente de feriado intermitente; o patch não mostra nesse hunk alteração dos leitores da pergunta. [app/core/juizes.py:492](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/juizes.py:492).

A antiga premissa “F6 é só leitura, nada persiste” precisa ser lida por degrau: as views U6/U9 passam um booleano que altera trilha persistida, U7 chama validação e U10 escolhe portas com efeitos. Isso já era apontado como correção pela matriz e permanece no código. [app/chamados/services/disputa_emissao.py:1547](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1547); [app/chamados/views_cobrar.py:203](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:203); [app/chamados/views.py:1421](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1421).

## 3 Nao verificado de ALCANCE

- **Caddy: fechado na cópia.** As rotas especiais de batida vão a `core`; o proxy restante vai a `ui`, abrangendo as rotas F6 citadas no brief. Configuração efetivamente implantada em produção: **[nao verificado]**. [Caddyfile:8](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/Caddyfile:8); [app/api/urls.py:73](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/urls.py:73); [app/api/urls.py:92](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/urls.py:92).
- **Dono F5 e leitores das duas cascas: fechados na cópia.** `particao_do_app` hoje usa o juiz, não mais só o instante. O filtro é chamado nos dois templates; o card é incluído em detalhe local e o modal é renderizado pela view. O modal também exibe `resposta_colab` diretamente: migrar só o filtro não cobre essa leitura. [app/chamados/services/pergunta_no_app.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/pergunta_no_app.py:76); [app/templates/chamados/partials/_pergunta_card.html:43](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_pergunta_card.html:43); [app/templates/chamados/partials/modal_fio.html:219](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:219); [app/templates/chamados/partials/modal_fio.html:274](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:274); [app/templates/chamados/detalhe_local.html:310](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/detalhe_local.html:310); [app/chamados/views.py:927](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:927).
- **Contrato APK: fechado por registro de corte, não por leitura independente do aplicativo.** O complemento registra que `respondida_em` é opcional e `dias_esperando` não é lido; o código Kotlin correspondente não foi aberto nesta cópia: **[nao verificado]** independentemente. [app/docs/CORTES.md:512](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:512).
- **Aberto 1 — censo completo de escritores e leitores cruzados:** foram identificadas as portas e os leitores citados acima, mas não foi refeito o censo AST integral de todos os usos dos quatro campos na árvore atual. Falta executar um censo somente de leitura e abrir os resultados por função, inclusive atribuições indiretas e ORM. **[nao verificado]** se a lista de P1–P18 e dos leitores novos esgota todos os consumidores cruzados. [app/chamados/services/disputa_emissao.py:1675](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1675); [app/chamados/models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143).
- **Aberto 2 — contrato antigo `atos_pergunta.leitura`:** a matriz descreve uma F1 antiga; não foi encontrado esse módulo na árvore pesquisada. Falta abrir o artefato exato da F1 antiga para comparar seu contrato com o desenho vigente; não se pode atribuir o P5 ou o 500 hipotético daquela versão ao HEAD apenas pela matriz. O universo atual da mensageria exige instante e texto. **[nao verificado]** o contrato daquela cópia antiga. [app/chamados/juizes.py:1761](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1761).

**alcance nao verificado restante: 2**

## 4 Perguntas de lei

- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 6:** a pergunta original da matriz sobre `aguarda_admin` nos estados omitidos usa o dono declarado pela tarefa: admin verdadeiro, colaborador falso e ausência de dono falso. O código atual ainda compara `estado` com `RESPONDIDA`, de modo que decisão e implementação são coisas distintas. [app/docs/CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509); [app/api/views.py:2553](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2553).
- **DECIDIDO PELA LEI L-130:** para P5, texto sem instante não constitui pergunta respondida; o corte substitui a antiga JUIZ-RESPONDIDA e a pergunta F6(2). A compatibilidade de `null` foi registrada no complemento posterior. [app/docs/LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151); [app/docs/CORTES.md:512](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:512).
- **Silêncio:** o corte anexado ao brief registra que a F5 deve passar a cobrar o admin e declara F5/F6 resolvidas; o código atual ainda devolve `silencio_decidido` sem ações. É uma diferença entre corte e implementação, não uma nova pergunta de lei demonstrada pelo material. [app/chamados/services/tarefa_do_dia.py:176](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:176).

perguntas de lei: 0

_6857 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
