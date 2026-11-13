# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 17:46** · publicado em **13/11/2026 17:48** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 17:46 · sessao a92e0d0adb4b · E3-FORMA-SQL · leitura · origem=Code

**Resultado: 0 regressões demonstradas pelos casos E3 escritos; os 79 ficam em “sem caso escrito para a combinação observada”.** Isso deixa a decisão normativa desses 79 em aberto; a perda de porta está demonstrada, mas sua classificação como violação não. (`v5_det.txt:1–79`; `app/chamados/tests/test_e3_portas_tarefa.py:21–127`)

Referências abreviadas:

- **V:n** = `v5_det.txt:n`, contando **somente as linhas `V5_DET`** fornecidas.
- **T:n** = [app/chamados/tests/test_e3_portas_tarefa.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/test_e3_portas_tarefa.py):n.
- **S:n** = [app/chamados/services/tarefa_do_dia.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py):n.
- **J:n** = [app/chamados/juizes.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py):n.
- **F:n** = [app/chamados/tests/e3_fixtures.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/e3_fixtures.py):n.

**1–2. Classes, contagens, exemplos e cobertura.**

A classificação abaixo agrupa multiplicidade e ordem das razões; preserva seus conjuntos, módulo/status e diferenças de âncora e ações. `P=passivo_sem_linha`, `A=aceso`, `R=aguarda_resposta`, `W=dia_sem_trabalho`, `V=veredito_sem_lavra_vigente`, `J=encerrada_pelo_juiz`. (`V:1–79`)

Na coluna âncora: **C** = presente, `fila_ids=true/fila_cel=true`; **G** = presente, `true/false`; **X** = presente, `false/false`; **—** = nenhuma pergunta/âncora exposta nas instâncias. Isso não comprova inexistência de perguntas suprimidas pela tarefa. (`V:1–79`; `S:262–271`)

Módulos: **TA**=`turno_aberto`, **BA**=`batida_ausente`, **DS**=`disputa_supervisao`, **AC**=`ausencia_colab`, **PR**=`par_relampago`, **TA24**=`turno_aberto_24h`; status **a**=`aberto`, **e**=`em_analise`. A coluna descreve os chamados relacionados no V5; módulo/status de âncoras não relacionadas ali permanecem **[não verificado]**. (`V:1–79`)

**A coluna de pks é exaustiva; seus primeiros dois elementos são os exemplos. Classes unitárias só têm um exemplo disponível.** (`V:1–79`)

| Classe | Rota ATUAL | Módulo/status | Razões; âncora; ações | n | celula_pk — lista completa e evidência | Cobertura |
|---|---|---|---|---:|---|---|
| C01 | fila | TA/e | A,R; X; responder/colaborador/null | 1 | 141714 (V:1) | SC-R |
| C02 | fila | TA/e | A,P; C; vazias | 2 | 130162 (V:2), 135778 (V:17) | SC-P |
| C03 | fila | BA/e | A,P,R; R em X, P em X e C; responder/colaborador/null | 1 | 118931 (V:3) | SC-RP |
| C04 | fila | BA/a | A,P; C; vazias | 6 | 140522 (V:4), 140524 (V:5), 140707 (V:7), 119517 (V:10), 122073 (V:65), 128516 (V:69) | SC-P |
| C05 | fila | BA/a | P; C; vazias | 12 | 140928 (V:8), 104784 (V:9), 129384 (V:12), 104040 (V:13), 116436 (V:23), 106963 (V:33), 124364 (V:43), 124370 (V:44), 123033 (V:46), 132144 (V:61), 106350 (V:64), 124774 (V:73) | SC-P |
| C06 | fila | BA/e + DS/a | A,P; C e G, incluindo linha sintética; vazias | 1 | 139432 (V:6) | SC-P |
| C07 | fila | TA/e | A; —; vazias | 9 | 140331 (V:11), 135067 (V:18), 110506 (V:25), 138672 (V:26), 138912 (V:39), 138914 (V:40), 138916 (V:41), 140580 (V:42), 106594 (V:63) | SC-A |
| C08 | fila | BA/e | A,P; C; vazias | 14 | 133364 (V:14), 111346 (V:15), 111845 (V:16), 116432 (V:22), 129745 (V:28), 129749 (V:29), 129751 (V:30), 137166 (V:31), 33784 (V:47), 128519 (V:70), 141797 (V:74), 143313 (V:77), 143393 (V:78), 143395 (V:79) | SC-P |
| C09 | fila | AC/a | A; —; vazias | 2 | 129083 (V:19), 129090 (V:20) | SC-A |
| C10 | fila | AC/e | A; —; vazias | 1 | 129093 (V:21) | SC-A |
| C11 | fila | TA/a | A; —; vazias | 1 | 137154 (V:24) | SC-A |
| C12 | pergunta | DS/e | A,P; G; vazias | 1 | 113307 (V:27) | SC-P |
| C13 | fila | TA/a | A,R; X; responder/colaborador/null | 1 | 129598 (V:32) | SC-R |
| C14 | fila | BA/a | A; —; vazias | 9 | 130931 (V:34), 130933 (V:35), 130935 (V:36), 130939 (V:38), 129567 (V:54), 129569 (V:55), 129573 (V:57), 129577 (V:58), 130781 (V:60) | SC-A |
| C15 | fila | BA/e | A; —; vazias | 4 | 130937 (V:37), 129559 (V:50), 129561 (V:51), 130026 (V:66) | SC-A |
| C16 | fila | TA/e + DS/a | A; —; vazias | 1 | 129217 (V:45) | SC-A |
| C17 | pergunta | DS/a | P,W; G; vazias | 2 | 130346 (V:48), 130348 (V:49) | SC-PW |
| C18 | fila | BA/e + PR/a | A; —; vazias | 1 | 129563 (V:52) | SC-A |
| C19 | fila | BA/a | A,P; C, em linhas sintéticas; vazias | 1 | 129565 (V:53) | SC-P |
| C20 | fila | BA/e + DS/a | A; —; vazias | 2 | 129571 (V:56), 130044 (V:67) | SC-A |
| C21 | fila | BA/e | A,V; C; vazias | 1 | 130771 (V:59) | SC-V |
| C22 | fila | BA/a + TA24/a | A,P; C; vazias | 1 | 132052 (V:62) | SC-P |
| C23 | fila | BA/e | P; C e G; vazias | 1 | 120325 (V:68) | SC-P |
| C24 | pergunta | DS/a | P; G; vazias | 2 | 128533 (V:71), 127477 (V:72) | SC-P |
| C25 | fila | BA/e | A,R; X; responder/colaborador/null | 1 | 141810 (V:75) | SC-R |
| C26 | fila | TA/e + BA/e | A,J; C; vazias | 1 | 142013 (V:76) | SC-J |
| **Total** | | | | **79** | **V:1–79** | |

Os códigos de cobertura significam:

| Cobertura | Caso escrito confrontado | O fato que falta para aplicar o caso | Decisão normativa pelos testes E3 | [nao verificado]
|---|---|---|---|
| SC-A | `test_rota_so_fila_sem_pergunta`, T:32–38 | O teste monta marco **apagado** e exige `perguntar/sistema/emissor`; aqui todas as instâncias expostas estão acesas, sem ações. | **Sem caso escrito** para estes fatos; ATUAL/FUTURO indeterminado. | [nao verificado]
| SC-P | `test_porta_fila_quando_ancora_e_da_celula`, T:67–73; caso A/B, T:21–47 | Falta um caso que imponha porta **apesar de passivo sem linha e ações vazias**. A fixture do teste cria pergunta sem resposta/validação; não constrói passivo. (`F:65–78`) | **Sem caso escrito** para esta combinação; ATUAL/FUTURO indeterminado. | [nao verificado]
| SC-R | `test_fila_A_pergunta_sob_B`, T:21–47 | O caso exige B como âncora viva e ação com porta B; no V5 a âncora está fora de `fila_ids`, com porta da ação `null`. | **Sem caso escrito** para B fora da fila; ATUAL/FUTURO indeterminado. |
| SC-RP | Mesmos casos, T:21–47,67–73 | Faltam simultaneamente o caso de B fora da fila e o de passivo com ações vazias no mesmo dia. | **Sem caso escrito** para a combinação. |
| SC-PW | Caso A/B, T:21–47; âncora da célula, T:67–73 | Falta cenário de dia sem trabalho com passivo, âncora global e nenhuma ação; também não há A residual na fila da célula. | **Sem caso escrito** para a combinação. |
| SC-V | T:67–73 | Falta cenário de validação sem lavra vigente que determine porta mesmo sem ação. | **Sem caso escrito** para a combinação. |
| SC-J | T:67–73 | Falta cenário de encerramento pelo juiz que determine porta mesmo sem ação. | **Sem caso escrito** para a combinação. |

O caso de isenção não se aplica aos fatos expostos: ele exige `razao='isento_ponto'`, ausente nos 79. Os testes de relógio também exigem valores de silêncio e instantes específicos não fornecidos para essas âncoras. (`T:57–65,84–111`; `V:1–79`) [nao verificado]

**3. REGRESSÃO: lista completa demonstrável = `[]`; quantidade = 0.** Nenhuma classe reproduz integralmente um caso E3 que imponha porta. Aplicar T:67–73 apenas pela presença da âncora descartaria o passivo e as decisões que distinguem os fatos. (`T:21–127`; `F:65–78`; `V:1–79`)

O caminho concreto que elimina a porta está identificado, mesmo sem obrigação normativa demonstrada:

| Classes | Onde a cópia deixa de publicar porta |
|---|---|
| C07, C09–C11, C14–C16, C18, C20 | A linha nasce com ações vazias; marco aceso recebe `aceso` e encerra sua passagem pelo laço. (`S:258–271`) |
| C02, C04–C06, C08, C12, C17, C19, C22–C24 | Passivo devolve `acoes=None`; a tarefa mantém a lista vazia e marca `nao_sei`. Exposição administrativa segue a mesma recusa de inventar ação. (`S:130–131,249–255,308–313`) |
| C01, C13, C25 | A âncora só vira porta se estiver na fila; fora dela, a ação `responder` recebe `porta=None`. (`S:128–133`) |
| C03 | Combina os dois caminhos anteriores: resposta sem porta e passivo sem ação. (`S:128–133,308–313`; `V:3`) |
| C21 | Sem lavra vigente, retorna razão e `acoes=None`. (`S:143–146,308–313`) |
| C26 | Encerramento pelo juiz retorna ações vazias. (`S:149–151,313`) |
| Todas | `_portas_da_tarefa` aceita somente portas inteiras pertencentes a `fila_ids`; sem portas, escreve `rota=None`; `porta_da_tarefa` devolve `None`. (`S:547–565`) |

Há contratos E2 que corroboram essas distinções: passivo exige ações vazias; marco aceso dispensa ação; âncora silenciada admite `responder` com `porta=None`. O motivo concreto de exclusão das quatro âncoras V5 da fila permanece **[não verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:396–409,487–495,867–873`; `V:1,3,32,75`)

**4. Restantes: 79 sem caso escrito completo; 0 demonstrados como “caso E3 que manda FUTURO”.** A lista completa dos 79 é a coluna de pks das classes C01–C26, sem exclusões; a lista do segundo conjunto é `[]`. (`V:1–79`; `T:21–127`)

A conferência factual fecha em **30 somente acesos; 41 com passivo e eventual aceso; 2 com passivo e dia sem trabalho; 3 com aceso e resposta; 1 com aceso, passivo e resposta; 1 sem lavra vigente; 1 encerrado pelo juiz = 79**. Há **75 dias com ações vazias e 4 com ações não vazias**. (`V:1–79`)

**5. REGRESSÃO demonstrada com dono ESTRUTURA: 0; lista `[]`.** Isso decorre do conjunto de regressões demonstradas estar vazio, sem necessidade de redistribuir os donos registrados na volta 4. (`V:1–79`; `T:21–127`)

O agregado **ESTRUTURA 44 / CADASTRO 6 / BATIDA 29** não fornece o pareamento individual dos 79 com seus donos. Uma lista individual dos 44 ESTRUTURA, derivada apenas desse agregado, seria **[não verificado]**. (`a1v4_julga.r:16`, contando as linhas textuais do anexo fornecido)

**6. Quem escreve, quem lê e diferenças de significado.**

| Campo | Quem escreve/calcula | Quem lê |
|---|---|---|
| `ancora` exibida no V5 | A tarefa **não escreve esse campo nas instâncias**; escreve `pergunta_id` e `acoes`. Calcula localmente `anc=chamado_que_lista(p)`. A implementação do enriquecimento da sonda é **[não verificado]**. (`S:128,258–260,318–320`) | A tarefa resolve a âncora pela pergunta, sem ler `instancia['ancora']`. (`S:127–129`) |
| Identidade da âncora | `chamado_que_lista` retorna `chamado_destino_id`, com fallback para `disputa.chamado_id`. (`app/chamados/services/fio.py:97–117`) | A carga consulta a fila dessas âncoras; `_pela_decisao` usa sua pertença para preencher a porta. (`S:415–418,128–133`) |
| `acoes[].acao/de/porta` | `_acao` monta os campos; `_pela_decisao` escolhe ações; `_tarefa_pura` as publica nas instâncias. (`S:114–162,249–255,302–313`) | `_portas_da_tarefa` lê porta e dono; o backfill lê razões e ações, inclusive humanos sem porta. (`S:547–556`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:97–117`) |
| `fila_ids` / `fila_cel` | `alimentar_fila` constrói a fila global com verbo e o agrupamento por célula. (`J:1558–1567`) | C17 admite chamado da célula e âncora global; tarefa usa a fila global para autorizar porta e a fila da célula para escolher rota/subconjunto `fila`. (`J:1677–1687`; `S:548–555`) |
| Verbo do ATUAL | `particionar_verbos` calcula o mapa por módulo, créditos das perguntas e silêncio; `alimentar_fila` executa a partição sobre `qs_vivos()`. (`app/chamados/services/verbos.py:30–52`; `J:1558`) | C17 copia o mapa; a tarefa copia o mesmo mapa para as portas selecionadas pelas ações. (`J:1678,1682`; `S:549`) |
| Porta publicada | `_portas_da_tarefa` escreve rota/fila/verbos/fio; `porta_da_tarefa` projeta esses campos ou `None`. (`S:547–565`) | Folha, pauta, vigia, beco e furo sem canal consomem essa projeção. (`app/folha/fora_da_folha.py:71–89`; `app/ponto/services/pautas_dos_discordantes.py:79–84`; `app/chamados/services/vigia_fio.py:640–654`; `app/escala/services/cadastro_realidade.py:492–503`; `app/chamados/services/furo_sem_canal.py:93–101`) |

As diferenças semânticas verificáveis são:

- **Chamado do dia versus chamado da pergunta:** a folha publica identificadores somente de `porta['fila']`, isto é, chamados da célula; as demais chaves de `verbos` podem incluir âncoras externas. Isso está explicitado no leitor. (`app/folha/fora_da_folha.py:235–242`; `S:549–551`)
- **Fila global versus fila da célula:** uma âncora G pode autorizar `porta_pergunta`; não precisa pertencer a `fila_cel`. Esses conjuntos têm funções distintas no código. (`S:548–555`; `J:1677–1687`)
- **Verbo versus dono:** C17 calcula fio pelos verbos; a tarefa calcula fio pelos donos das ações. O teste admite expressamente `responder` junto de verbo `validar`; o vigia preserva essa distinção. (`J:1633–1639,1688`; `S:552–556`; `T:75–82`; `app/chamados/services/vigia_fio.py:649–653`)
- **Ausência de porta versus ninguém esperando:** a documentação da pauta diz “não estão esperando NINGUÉM”, mas seu predicado efetivo lê ausência de porta. A tarefa pode ter ação humana sem porta ou `nao_sei`; a folha já descreve essa limitação. (`app/ponto/services/pautas_dos_discordantes.py:47–56,79–84`; `S:129–133,544,562–565`; `app/folha/fora_da_folha.py:52–58`)
- **Rótulo do instrumento versus caso escrito:** `casos_do_par` chama de `so_fila_sem_pergunta` qualquer `fila and not vivas`; não exige marco apagado nem ação `perguntar`. Esse rótulo, sozinho, não comprova cobertura por T:32–38. (`app/chamados/management/commands/diff_e3_sombra.py:63–70`; `T:32–38`)

## 13/11 17:45 · sessao a0cfb94c0d12 · - · - · origem=Ronald (tmux)

Contagem conferida: **79 colab-dias, 75 com ações vazias e 4 com ações não vazias**. Os quatro são `141714`, `118931`, `129598`, `141810`. (`v5_det.txt:1,3,32,75`, contando somente linhas `V5_DET`; demais linhas V5:1–79)

A tabela preserva módulo/status e separa misturas e linhas sintéticas. Ignora somente multiplicidade e ordem das razões. Abreviações: `A=aceso`, `P=passivo_sem_linha`, `R=aguarda_resposta`, `W=dia_sem_trabalho`, `V=veredito_sem_lavra_vigente`, `J=encerrada_pelo_juiz`; `e=em_analise`, `a=aberto`. `mesma` significa âncora em ambas as filas; `global` significa `fila_ids=True/fila_cel=False`; `fora` significa ambos falsos. Módulos abreviados por prefixo inequívoco. (`v5_det.txt:1–79`)

| # | Rota | Módulo/status | Razões/âncora/ações | n | Lista completa com linha V5 |
|---|---|---|---|---:|---|
| 1 | fila | turno_aberto/e | A,R; R fora; responder/null | 1 | 141714:1 |
| 2 | fila | turno_aberto/e | A,P; mesma; vazias | 2 | 130162:2, 135778:17 |
| 3 | fila | batida_ausente/e | A,P,R; R fora; P fora e mesma; responder/null | 1 | 118931:3 |
| 4 | fila | batida_ausente/a | P ou A,P; mesma; vazias | 18 | 140522:4, 140524:5, 140707:7, 140928:8, 104784:9, 119517:10, 129384:12, 104040:13, 116436:23, 106963:33, 124364:43, 124370:44, 123033:46, 132144:61, 106350:64, 122073:65, 128516:69, 124774:73 |
| 5 | fila | batida_ausente/e + disputa_supervisao/a | A,P; mesma+global, sintética; vazias | 1 | 139432:6 |
| 6 | fila | turno_aberto/e | A; sem âncora; vazias | 9 | 140331:11, 135067:18, 110506:25, 138672:26, 138912:39, 138914:40, 138916:41, 140580:42, 106594:63 |
| 7 | fila | batida_ausente/e | P ou A,P; mesma; vazias | 14 | 133364:14, 111346:15, 111845:16, 116432:22, 129745:28, 129749:29, 129751:30, 137166:31, 33784:47, 128519:70, 141797:74, 143313:77, 143393:78, 143395:79 |
| 8 | fila | ausencia_colab/a | A; sem âncora; vazias | 2 | 129083:19, 129090:20 |
| 9 | fila | ausencia_colab/e | A; sem âncora; vazias | 1 | 129093:21 |
| 10 | fila | turno_aberto/a | A; sem âncora; vazias | 1 | 137154:24 |
| 11 | pergunta | disputa_supervisao/e | A,P; global; vazias | 1 | 113307:27 |
| 12 | fila | turno_aberto/a | A,R; fora; responder/null | 1 | 129598:32 |
| 13 | fila | batida_ausente/a | A; sem âncora; vazias | 9 | 130931:34, 130933:35, 130935:36, 130939:38, 129567:54, 129569:55, 129573:57, 129577:58, 130781:60 |
| 14 | fila | batida_ausente/e | A; sem âncora; vazias | 4 | 130937:37, 129559:50, 129561:51, 130026:66 |
| 15 | fila | turno_aberto/e + disputa_supervisao/a | A; sem âncora; vazias | 1 | 129217:45 |
| 16 | pergunta | disputa_supervisao/a | P,W; global; vazias | 2 | 130346:48, 130348:49 |
| 17 | fila | batida_ausente/e + par_relampago/a | A; sem âncora; vazias | 1 | 129563:52 |
| 18 | fila | batida_ausente/a | A,P; mesma, sintéticas; vazias | 1 | 129565:53 |
| 19 | fila | batida_ausente/e + disputa_supervisao/a | A; sem âncora; vazias | 2 | 129571:56, 130044:67 |
| 20 | fila | batida_ausente/e | A,V; mesma; vazias | 1 | 130771:59 |
| 21 | fila | batida_ausente/a + turno_aberto_24h/a | A,P; mesma; vazias | 1 | 132052:62 |
| 22 | fila | batida_ausente/e | P; mesma+global; vazias | 1 | 120325:68 |
| 23 | pergunta | disputa_supervisao/a | P; global; vazias | 2 | 128533:71, 127477:72 |
| 24 | fila | batida_ausente/e | A,R; fora; responder/null | 1 | 141810:75 |
| 25 | fila | turno_aberto/e + batida_ausente/e | A,J; mesma; vazias | 1 | 142013:76 |
| **Total** | | | | **79** | V5:1–79 |

**Atenção de precisão:** classes 4 e 7 agrupam conjuntos `P` e `A,P`; se “padrão” exigir igualdade exata do conjunto de razões, devem ser subdivididas. A tabela preserva os fatos no texto de cada classe e permite duas amostras nos grupos de n≥2; grupos unitários não têm segundo exemplo. (`v5_det.txt:4–10,12–16,22–23,28–31,33,43–47,61,64–65,69–70,73–74,77–79`)

Consolidação ampla útil: **30 somente A; 41 com P e eventual A; 2 P,W; 3 A,R; 1 A,P,R; 1 A,V; 1 A,J =79**. (`v5_det.txt:1–79`)

## 13/11 17:43 · sessao 0fcc9646b8a7 · - · - · origem=Ronald (tmux)

| Campo | Escritor | Leitor e significado |
|---|---|---|
| Âncora da pergunta | `app/chamados/services/fio.py:97–117`: `chamado_destino_id`, com fallback para `disputa.chamado_id`. A tarefa a calcula em variável local, `app/chamados/services/tarefa_do_dia.py:127–129`. | `_pela_decisao` atribui essa âncora à porta da ação somente quando pertence à fila recebida, `tarefa_do_dia.py:129`. A carga forma essa fila consultando `qs_na_fila` sobre as âncoras das perguntas, `tarefa_do_dia.py:415–418`. |
| Campo `ancora` do V5 | Não consta da construção das instâncias em `tarefa_do_dia.py:258–260,318–320`. O V5 o descreve como enriquecimento da sonda; implementação dessa escrita **[não verificado]**. | A tarefa não lê um campo `instancia['ancora']`; calcula a âncora pelo objeto pergunta, `tarefa_do_dia.py:127–129`. |
| `acoes[].acao/de/porta` | `_acao` monta os três campos, `tarefa_do_dia.py:114–115`; `_pela_decisao` escolhe ações conforme decisão/passivo, `:128–162`; `_tarefa_pura` publica nas instâncias, `:249–255,302–313`. | `_portas_da_tarefa` achata as ações, mantém portas inteiras pertencentes à fila global, lê o verbo do painel e calcula fio pelos donos, `:547–556`. O backfill lê razões/ações/portas, inclusive humanos com `porta=None`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:97–117`. |
| `fila_ids` e `fila_cel` | `app/chamados/juizes.py:1558–1567`: `fila_ids` contém a fila global com verbo; `fila_cel` agrupa chamados por `celula_id` nas células pedidas. | C17 lê chamados da célula e soma âncoras da pergunta pertencentes à fila global, `juizes.py:1677–1682`. A tarefa usa `fila_ids` para autorizar porta e `fila_cel` somente para determinar `porta_fila` e o subconjunto `fila`, `tarefa_do_dia.py:548–555`. |
| Verbo do ATUAL | `app/chamados/services/verbos.py:30–52`: partição usa módulo, créditos de perguntas e silêncio. `juizes.py:1558` executa sobre `qs_vivos()`. | C17 copia esses verbos em `juizes.py:1678,1682`; traduz verbo em fio por `:1633–1639,1688`. A tarefa copia o mesmo mapa para portas escolhidas pelas ações, mas calcula fio pelos donos das ações, `tarefa_do_dia.py:549,552–556`. |

Leitores de produção da porta publicada: [nao verificado]

- Folha lê `porta_da_tarefa`, fio e `fila`, sem reconstruir âncora: `app/folha/fora_da_folha.py:71–89,233–242`.
- Pauta lê o fio projetado pela folha: `app/ponto/services/pautas_dos_discordantes.py:79–84`.
- Vigia lê rota/fio; explicita que verbo do painel não decide quem age: `app/chamados/services/vigia_fio.py:640–654`.
- Beco exige ausência de porta da tarefa e mantém a âncora da pergunta separadamente: `app/escala/services/cadastro_realidade.py:492–503`.
- Furo sem canal lê presença de porta/rota e distingue `nao_sei`: `app/chamados/services/furo_sem_canal.py:93–101`.
- Instrumento de sombra compara C17 com a projeção `porta_da_tarefa`: `app/chamados/management/commands/diff_e3_sombra.py:127–130`.

Há descrição mais ampla que o predicado na pauta: “não estão esperando NINGUÉM” e “pergunta viva [...] chamado vivo JA tem dono esperando”, embora o código efetivo teste ausência de porta publicada; esta pode existir com ações humanas sem porta. (`app/ponto/services/pautas_dos_discordantes.py:47–56,79–84`; `app/chamados/services/tarefa_do_dia.py:129,133,548–565`)

A folha limita o identificador nomeado ao chamado da célula publicado em `fila`, preservando a distinção entre chamado do dia e âncora da pergunta; não nomeia todas as chaves de `verbos`. (`app/folha/fora_da_folha.py:235–242`; `app/chamados/juizes.py:1654–1657`)

Cuidado normativo: `test_porta_fila_quando_ancora_e_da_celula` constrói pergunta sem resposta/passivo e exige verbo `cobrar`; não escreve o cenário `passivo_sem_linha + acoes=[] + validar/decidir`. O caso A/B exige B vivo na fila e ação com porta B. Os quatro dias humanos do V5 têm âncora fora de ambas as filas, portanto não reproduzem esse caso. (`app/chamados/tests/test_e3_portas_tarefa.py:21–47,67–73`; `v5_det.txt:1,3,32,74`, contando somente `V5_DET`; conferir numeração com a recontagem do agente principal)

## 13/11 17:43 · sessao ae8ca03f0083 · - · - · origem=Ronald (tmux)

Os fatos V5 não demonstram regressão coberta integralmente por `test_e3_portas_tarefa.py`:

| Padrão V5 | Aplicabilidade estrita do caso E3 |
|---|---|
| `passivo_sem_linha`, âncora na fila/célula, ações vazias | **Sem caso escrito E3 completo.** O teste de âncora da célula cria pergunta sem resposta/validação; não cria passivo. (`app/chamados/tests/test_e3_portas_tarefa.py:67–73`; `app/chamados/tests/e3_fixtures.py:65–78`) |
| Tudo `aceso`, perguntas/âncoras ausentes, ações vazias | **Sem caso escrito E3 completo.** “Só fila sem pergunta” monta marco apagado e exige ação `perguntar/sistema/emissor`; os dias inteiramente acesos não reproduzem essa entrada. (`app/chamados/tests/test_e3_portas_tarefa.py:32–38`) |
| `aguarda_resposta`, âncora existente mas fora `fila_ids`, `responder/colaborador/None` | **Sem caso escrito E3 completo.** O caso A/B exige B como âncora **viva** e publica porta B; B fora da fila não reproduz esse fato. (`app/chamados/tests/test_e3_portas_tarefa.py:21–28,40–47`) |
| `veredito_sem_lavra_vigente` ou `encerrada_pelo_juiz`, ações vazias | **Sem caso escrito E3 completo:** os testes de porta não constroem essas decisões/lavras. (`app/chamados/tests/test_e3_portas_tarefa.py:21–111`) |

Há contratos E2 que explicitamente impedem tratar esses resultados como regressões apenas pela existência de âncora:

- Passivo: `passivo_sem_linha`, ações vazias e `nao_sei=True`. (`app/chamados/tests/test_e2_tarefa_do_dia.py:487–495`)
- Marco aceso: nenhuma ação, inclusive havendo pergunta respondida. (`app/chamados/tests/test_e2_tarefa_do_dia.py:867–873`)
- Âncora silenciada: permanece `responder/colaborador`, porém `porta=None`. (`app/chamados/tests/test_e2_tarefa_do_dia.py:396–409`)

O caminho de ausência de porta é explícito: passivo devolve `None` antes de publicar ação; marco aceso encerra a instância; âncora fora da fila vira `porta=None`; a publicação aceita apenas portas inteiras presentes em `fila_ids`. (`app/chamados/services/tarefa_do_dia.py:128–133,262–271,308–313,547–565`)

Portanto, pela leitura estrita dos casos E3 solicitados, **zero regressões demonstradas**, sem concluir que toda perda esteja normativamente correta. A existência de um caso E3 que determine porta para essas combinações permanece **[não verificado]**.

## 13/11 17:42 · sessao 2c7360c1be05 · O168 · verificador · origem=Code

**REPROVA.** Há dois bloqueantes: a constância de custo continua sem prova para várias respostas pendentes, e CSF/retratação ainda respondem diferente ao mesmo caso trancado. (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47`; `app/chamados/models.py:1902`; `app/chamados/juizes.py:2035`)

1. **Os seis itens do opus v6**

| Item | Resultado da cura |
|---|---|
| 1 — mapa desatualizado | **Curado na cópia.** Os dois endereços estão no mapa; executei o selo, que passou, e comparei o resultado do gerador em memória: igualdade exata. Estarem no mesmo commit é [não verificado]. (`app/docs/MAPA-JUIZES.md:106`, `:114`; `bin/tests/test_mapa_juizes.sh:66`; `bin/gerar_mapa_juizes.py:66`) |
| 2 — frota não medida | **Há medição fornecida**, com universo não vazio e categorias compatíveis com B. A execução pelo juiz real permanece [não verificado]: o script da sonda não acompanha os anexos nem foi localizado na cópia. (anexos `frota_b_head.out:4`; `frota_b_v8.out:4`) |
| 3 — contador lendo só trilha | **Curado para o contraexemplo da resposta recusada.** Agora exige também `not pode_executar_csf(_ch)`; existe teste que mantém a trilha antiga, recusa a resposta e exige contador zero. Execução Django desse teste [não verificado]. (`app/chamados/juizes.py:2530`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:187`, `:195`) |
| 4 — custo constante | **Não curado integralmente.** A consulta por disputa saiu, mas permanecem consultas por pergunta pendente; o teste mantém apenas uma pendente quando aumenta N. Contraexemplo no item 9. (`app/ponto/supra_juiz.py:558`; `app/chamados/models.py:1902`; `app/chamados/tests/test_o168_custo_da_guarda_constante.py:47`) |
| 5 — “MESMO predicado” falso | **Curado documentalmente.** MD e JSON marcam expressamente a afirmação como superada. (`app/docs/CORTES.md:448`; `app/docs/CORTES.json:1405`) |
| 6 — nota contraditória/redundância | **Redundância removida; sequência explicada.** A nota só nasce após `_pend` não vazio. O comentário distingue recusa da cascata e fechamento posterior pelo desligamento; esse fechamento continua fora da guarda. (`app/chamados/models.py:720`, `:735`; `app/colaboradores/services/desligamento.py:151`, `:163`) |

2. **O diff realiza B e somente B?**

No gate de `reconciliar`, implementa a seleção de B: resposta validada ou materializada deixa de segurar; resposta sem ambos segura, ressalvada a tranca. Defesa 1 chega a esse gate. O fechamento ainda depende também das guardas existentes de premissa e pergunta viva. (`app/chamados/models.py:683`, `:686`, `:689`, `:1898`; `app/chamados/signals.py:43`)

**BLOQUEANTE — a “mesma pergunta” da retratação continua divergente.** Contraexemplo: pergunta creditada ao chamado, respondida, não validada, não materializada, com dia trancado. O CSF libera porque respeita a tranca; a retratação segura porque passa `ignorar_tranca=True`. O executor consulta o primeiro e depois chama a segunda. (`app/chamados/models.py:1902`; `app/chamados/juizes.py:2033`, `:2035`; `app/chamados/reconciliador.py:448`, `:519`; `app/ponto/management/commands/supra_juiz.py:261`, `:286`)

Executei o predicado extraído em memória: esse caso devolveu **zero pendentes** normalmente e **uma pendente** com `ignorar_tranca=True`. Portanto houve unificação do corpo, mas não da resposta efetiva exigida pelo aval. (`app/chamados/models.py:1898`; `app/chamados/juizes.py:2035`; anexo `aval.txt:2`)

Há fechamentos fora desse juiz: desligamento, aceite de vínculo, regularização externa e competência trancada usam outras portas. `encerrar` consulta pergunta viva, sem consultar a guarda da conversa. Existe também desligamento por cron. (`app/colaboradores/services/desligamento.py:163`; `app/colaboradores/services/vinculo.py:388`; `app/chamados/services/regularizacao_ext.py:206`; `app/chamados/services/competencia_trancada.py:209`; `app/chamados/models.py:1664`; `app/config/crons.py:440`)

O selo declara essas exceções por nome e cita outro aval; a autorização literal dessas exceções é **[não verificado]**, pois esse aval não foi fornecido. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:3`, `:27`)

3. **Frota B e dia trancado**

A soma fornecida fecha: **862 com conversa = 513 segurados + 349 liberados**; os liberados se dividem em **345 toda julgada + 4 pendentes**. Isso é compatível com o predicado atual, que exclui validadas, materializadas e perguntas trancadas. Não comprova execução pelo juiz real nem fechamento efetivo. (anexo `frota_b_v8.out:4`; `app/chamados/models.py:1898`; `app/ponto/management/commands/supra_juiz.py:286`)

A classificação dos quatro como “dia trancado” não aparece nos campos do JSON fornecido: **[não verificado]**. Se forem os casos descritos, podem ser liberados pelo juiz e novamente retidos pela retratação, conforme o contraexemplo anterior. (`app/chamados/juizes.py:2035`; `app/chamados/reconciliador.py:519`)

A exceção não está enumerada no aval B literal. Ela é **herdada do juiz existente**, documentada como regra anterior da tranca; não nasceu nesta cura. (anexo `aval.txt:2`; `app/chamados/models.py:1896`; `app/chamados/juizes.py:978`)

4. **Quem escreve e quem lê**

| Campo/valor | Escritores | Leitores e significado |
|---|---|---|
| `resposta_colab`, `respondida_em` | Resposta normal grava o par; resposta automática por outro canal também; recuperação pela trilha escreve só o carimbo; reabertura limpa ambos. (`app/chamados/services/disputa_emissao.py:1467`, `:1593`, `:1652`, `:1674`) | O juiz aceita carimbo **OU texto**. Já `estado`/`respondida` dependem do carimbo; materialização exige texto. (`app/chamados/models.py:1900`, `:3101`, `:3552`; `app/chamados/services/materializacao.py:844`) |
| `validada_em` | Validação escreve; devolução e nova resposta limpam; `carimbar(resposta='validar')` também pode preencher automaticamente. (`app/chamados/services/validacao.py:157`, `:222`; `app/chamados/services/disputa_emissao.py:1480`; `app/chamados/models.py:3190`) | Juiz considera qualquer preenchimento suficiente para não segurar; materializador usa o campo para selecionar trabalho. Logo o campo isolado não prova julgamento humano. (`app/chamados/models.py:1899`; `app/chamados/services/materializacao.py:1375`) |
| `materializada_em` | Atribuidor central `_escrever_carimbo`, usado pelas portas de carimbo e desfazimento. Também recebe encerramento por identidade/admin sem criar batida. (`app/chamados/models.py:3153`, `:3120`, `:3241`, `:3259`) | Juiz deixa de segurar quando preenchido. Portanto “materializada” não equivale necessariamente a batida criada. (`app/chamados/models.py:1899`, `:3106`) |
| `fechamento_recusado` | `_registrar_recusa` grava a trilha; o gate fornece motivo `conversa_exige_decisao_humana`. (`app/chamados/models.py:397`, `:408`, `:723`) | Contador usa a trilha como universo histórico e consulta o juiz atual. A nota visível é outra escrita, pela porta de notas. (`app/chamados/juizes.py:2525`, `:2531`; `app/chamados/models.py:735`; `app/chamados/services/nota.py:75`) |
| `pais_segurados_pela_conversa` | Não persiste contador: calcula fila ∩ histórico de recusa ∩ juiz atual segurando. (`app/chamados/juizes.py:2525`, `:2530`) | Leitor encontrado é o teste; não representa toda a frota segurada, pois exige recusa histórica e presença na fila. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:75`; `app/chamados/juizes.py:2515`) |
| `respostas_sem_veredito` / `sem_veredito_entre` | Não gravam estado; o método registrado delega ao corpo compartilhado. (`app/chamados/models.py:1886`, `:1889`) | Fechamento/prontidão, desligamento, ficha, reconciliador, fila por causa, tranca, retratação e CSF. Alguns passam `ignorar_tranca=True`. (`app/chamados/models.py:1909`, `:1942`; `app/colaboradores/services/desligamento.py:141`; `app/colaboradores/services/ficha.py:169`; `app/chamados/reconciliador.py:1164`; `app/chamados/services/fila_por_causa.py:226`; `app/chamados/services/competencia_trancada.py:126`; `app/chamados/juizes.py:2035`; `app/ponto/supra_juiz.py:567`) |
| `perguntas_que_seguram_csf` | Calcula lista ordenada, unindo origem e destino; não grava. (`app/ponto/supra_juiz.py:567`) | Bool registrado e gate/nota leem a mesma lista. (`app/ponto/supra_juiz.py:582`; `app/chamados/models.py:719`, `:738`) |

**Há incompatibilidade concreta de significado:** texto `'07:00'` sem `respondida_em`, sem validação/materialização, segura pelo juiz, mas validar retorna `sem_resposta` e recusar retorna `ja_pendente`. A nota nova manda justamente validar/devolver. Existe reparo pela trilha; sua disponibilidade nesse fluxo de tela é [não verificado]. (`app/chamados/models.py:1900`, `:736`; `app/chamados/services/validacao.py:416`, `:603`; `app/chamados/services/disputa_emissao.py:1661`)

5. **LEI-AKITA e `padrao=`**

O histórico `main..ac8fbe5cc` não está acessível: a pasta não é um repositório Git. Linhas de cada commit, `juizes novos=0`, `padrao=` e presença das curas no mesmo commit: **[não verificado]**.

Pelo critério funcional proposto, **há funções novas compartilhadas**: `sem_veredito_entre` e `perguntas_que_seguram_csf`. Entretanto são extração do corpo existente e composição dos universos; não encontrei segunda cópia da condição material de resposta sem veredito. O padrão realizado é centralização/delegação, com a divergência parametrizada da tranca já apontada. (`app/chamados/models.py:1886`, `:1898`; `app/ponto/supra_juiz.py:561`; `app/chamados/juizes.py:2035`)

6. **Corte na cópia**

**Sim.** Lendo o arquivo da cópia, a frase pedida de nascimento de `DisputaSupervisao.respostas_sem_veredito` está na linha O168-JUIZ-DO-VEREDITO. O registro vivo aponta para o mesmo endereço: `chamados/models.py::DisputaSupervisao.respostas_sem_veredito`. (`app/docs/CORTES.md:449`; `app/core/juizes.py:294`)

7. **Dinheiro**

Não há prova de alteração de entra/fora da folha, rubrica ou valor dos 349; competências e efeitos reais: **[não verificado]**. A classificação de exportação consulta outros insumos, que não foram comparados nos anexos. (`app/folha/export.py:765`, `:830`)

Também **não é demonstrável impacto financeiro zero**: retratar pode resolver perguntas e fechar disputa; fechar disputa agenda retomada de regularizações confirmadas; essa retomada pode criar, corrigir ou remover batidas. (`app/chamados/reconciliador.py:537`; `app/chamados/services/disputa_emissao.py:1987`; `app/chamados/signals.py:139`, `:146`; `app/chamados/services/regularizacao_ext.py:78`, `:137`, `:225`)

Os anexos não informam quantos dos 349 percorrem essa cadeia, nem se atingem competência 10 ou 11. **[não verificado]**

8. **Classes de reprovação**

Há **parâmetro que produz duas respostas à mesma pergunta**, **campo com significados distintos entre leitores** e **selo novo que deixa escapar mutante óbvio**. Os dois primeiros estão nos contraexemplos da tranca e do texto sem carimbo. (`app/chamados/juizes.py:2035`; `app/chamados/models.py:1902`; `app/chamados/services/validacao.py:416`)

No selo da regularização, executei em memória o método AST com original e mutante `if _disp` → `if not _disp`: **ambos passaram**. Ele confere ordem e presença de `return`, sem conferir a polaridade. Há, porém, teste comportamental existente incompatível com esse mutante; não afirmo fuga da suíte inteira. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:217`; `app/chamados/services/regularizacao_ext.py:62`; `app/chamados/tests/test_guard_disputa_reg_externa.py:40`)

Há ainda relógio transitivo fora da borda no contador novo: não recebe `agora`, chama a fila sem instante, e esta usa `timezone.now()`. Contraexemplo de composição: fila avaliada em `t` e contador depois do vencimento de `silenciado_ate` dão universos diferentes. Divergência ocorrida em produção é [não verificado]. (`app/chamados/juizes.py:2511`, `:2530`; `app/chamados/services/painel.py:190`; `app/docs/PLANO-PADROES.md:92`)

Não identifiquei nova escrita direta de estado fora das portas nem nova aquisição de trava no trecho acrescentado; isso não certifica concorrência executada. (`app/chamados/models.py:723`, `:735`; `app/ponto/supra_juiz.py:552`; `app/chamados/juizes.py:2530`)

9. **Custo — bloqueante remanescente**

**O teste não prova constância para três disputas pendentes.** Ele deixa pendente apenas `i == 0`; as demais ficam validadas e pulam a leitura da tranca. (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47`; `app/chamados/models.py:1899`)

Contraexemplo: mantenha o fixture e deixe as **N respostas pendentes**. Cada uma chama a tranca, que acessa `p.disputa.colaborador`; os universos são carregados sem `select_related`. Mesmo sem data, essas FKs são avaliadas antes do retorno por dia ausente. Executei o predicado em memória: uma pendente chamou a tranca uma vez; três chamaram três vezes. O número exato de SQL em banco é [não verificado], mas o caminho por item permanece. (`app/ponto/supra_juiz.py:558`, `:568`; `app/chamados/models.py:1902`; `app/chamados/juizes.py:991`, `:1024`)

Os tetos passaram para **219/279**, com justificativa textual e referência a medições; os artefatos dessas medições não foram fornecidos, portanto seus resultados são [não verificado]. O próprio comentário limita o cenário medido a zero respostas que consultam tranca: ele não cobre o contraexemplo pendente. (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`)

## 13/11 17:42 · sessao 3fd4fd38ee19 · BO-[nome]-1311-ROTEIRO · sonda · origem=Code

`BYTES_DA_SAIDA 214375` e o aval anexo chegaram. A correspondência 01→col1019, 02→col996, 03→col1032, 04→col246 e 05→col265 é consistente entre UNIVERSO, FICHA e EXTRA recebidos. [nao verificado]

**Não houve nova execução nesta sessão, nem reproduzo abaixo a saída integral.** O arquivo executável da sonda não foi localizado na cópia; recebi sua transcrição raspada. [nao verificado] O wrapper executa a sonda antes de chamar o Codex e entrega seu resultado pelo prompt (`bin/codex_pergunta.sh:15`; `bin/codex_pergunta.sh:324`; `bin/codex_pergunta.sh:357`).

O extrato diagnóstico recebido foi este; ele **não substitui os blocos FICHA/EXTRA integrais**. [nao verificado]

```text
2026-11-13 17:32:55,831 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=46757 resposta=16:01 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 17:32:56,007 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=55375 resposta=14:30 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 17:32:56,026 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=55411 resposta=15:50 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
ERROS {}
BYTES_DA_SAIDA 214375 (teto do wrapper 300000 com os anexos)
```

O mapa final veio vazio, mas existem três diagnósticos `ERROR`; não certifico aplicação correta das perguntas 46757, 55375 e 55411. [nao verificado] O código registra a divergência e continua escolhendo `t1 or t2`; portanto, receber uma previsão depois do diagnóstico não comprova concordância entre as fontes (`app/chamados/services/materializacao.py:302`; `app/chamados/services/materializacao.py:310`; `app/core/observ.py:73`).

Os dados individuais abaixo estão somente na transcrição recebida, sem arquivo local numerado; por isso levam `[nao verificado]`. As referências de código identificam o comportamento conferido na cópia.

**Critério comum:** nenhuma pergunta será classificada **a — VALIDA-HOJE**. `estado_aplicacao_pergunta` prevê derivação, reuso e paridade, mas não projeta a nova lavra nem a retenção posterior (`app/chamados/services/materializacao.py:449`; `app/chamados/services/materializacao.py:463`). O juiz real da retenção é `motivos_retencao_celula`, pelo laço `_dias_sem_decisao`: lê veredito acusador, protestos e exceção de pausa (`app/folha/export.py:1212`; `app/folha/export.py:1273`; `app/folha/export.py:1293`). A exceção exige protestos exatamente `{'FURO_PARCIAL'}` e confirmação de que só faltam marcos de pausa (`app/folha/export.py:1173`; `app/folha/export.py:1148`).

“Não medido” também será usado quando faltar declaração ou diagnóstico de cadastro: validar pergunta sem resposta é recusado, e isso não comprova falta de vínculo ou de marcos (`app/chamados/services/validacao.py:393`). Assim, a classe **d — CADASTRO** não será presumida.

## Caso 01 -- col1019

**Comp 11: nenhum dia retido; nenhuma pergunta viva.** [nao verificado]

| Pergunta | Dia | Declarado | `estado_aplicacao` | Disputa | Classe |
|---|---|---|---|---|---|
| 54117 | 03/11 | hora / nulo | nulo | 7877 fechada | não medido; histórica. [nao verificado] |
| 54118 | 03/11 | hora / nulo | nulo | 7877 fechada | não medido; histórica. [nao verificado] |
| 54119 | 05/11 | hora / nulo | nulo | 7877 fechada | não medido; histórica. [nao verificado] |

As três estão com `viva=false`, via `celula_absolveu`, materializadas em 13/11 às 07:16:02; o chamado 37070 e a disputa 7877 estão encerrados. [nao verificado]

O recorte não contém batidas. Há abonos aprovados nos dias 03, 05, 07, 09, 10 e 12/11; as células correspondentes aparecem como `ausencia`, com veredito `concorde`. [nao verificado]

**Roteiro:** não há dia retido a retirar nesta fotografia. Os vínculos e as regenerações constam do dado, mas a queixa de tela permanece sem print e sem diagnóstico adicional. [nao verificado]

## Caso 02 -- col996

**Comp 11: quatro dias retidos — 21/10, 30/10, 03/11 e 12/11 — todos por `espelho_furo`. Há 16 perguntas vivas, todas na disputa 7263 aberta. A pergunta 46751 é de 20/10, fora da comp 11, e continua incluída por estar viva.** [nao verificado]

Nesta tabela, **D** significa **b — ESPERA OBRA DECLARADO-X-GRAVADO**, existente em `app/docs/BACKLOG.md:1936`.

| Pergunta | Dia / motivo | Tipo / declarado | `estado_aplicacao` recebido | Classe exclusiva |
|---|---|---|---|---|
| 46751 | 20/10 · saída sem entrada | hora / 07:50 | `bloqueada_paridade`: vizinha E 07:00 | **c — DECISÃO DO ADMIN**. [nao verificado] |
| 46756 | 21/10 · intervalo saída | hora / 15:00 | `aplicavel`: E 15:00 | **b — D**. [nao verificado] |
| 46757 | 21/10 · intervalo volta | hora / 16:01 | `aplicavel`: E 14:30; diagnóstico de divergência | **b — D**. [nao verificado] |
| 46758 | 21/10 · `orfao_14h` | hora / 21:05 | `aplicavel`: S 21:05 | **não medido**: saída da retenção não projetada. [nao verificado] |
| 48773 | 22/10 · intervalo saída | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |
| 57211 | 22/10 · intervalo volta | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |
| 52035 | 30/10 · `orfao_14h` | hora / 15:50 | `aplicavel`: S 15:50 | **não medido**: saída da retenção não projetada. [nao verificado] |
| 53565 | 03/11 · intervalo volta | hora / nulo | nulo | **não medido**: falta declaração; existe E 14:29:51 no dado. [nao verificado] |
| 53644 | 03/11 · `orfao_14h` | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |
| 53920 | 04/11 · intervalo saída | hora / 13:01 | `ja_satisfeita`: S 12:54 existente | **c — DECISÃO DO ADMIN**. [nao verificado] |
| 55313 | 08/11 · saída sem entrada | hora / 07:00 | `ja_satisfeita`: E 06:58 existente | **c — DECISÃO DO ADMIN**. [nao verificado] |
| 55375 | 08/11 · intervalo saída | hora / 14:30 | `aplicavel`: S 13:00; diagnóstico de divergência | **b — D**. [nao verificado] |
| 55411 | 08/11 · intervalo volta | hora / 15:50 | `aplicavel`: E 14:30; diagnóstico de divergência | **b — D**. [nao verificado] |
| 56923 | 11/11 · intervalo volta | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |
| 57210 | 11/11 · intervalo saída | hora / 13:00 | `aplicavel`: S 13:00 | **não medido**: o dia já não retém na fotografia. [nao verificado] |
| 57214 | 12/11 · `orfao_14h` | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |

**DECLARADO-X-GRAVADO.** O código substitui a hora respondida pela hora do marco nos intervalos (`app/chamados/services/materializacao.py:409`). O tipo prioriza o marco do chamado-destino sobre o da ata, inclusive quando divergem (`app/chamados/services/materializacao.py:274`; `app/chamados/services/materializacao.py:302`). A saída de pausa tem papel S e a volta, E (`app/escala/models.py:1652`). Na 46756, o dado contém declaração de hora, sem declaração separada de E/S; o desencontro demonstrado é entre o motivo de saída de pausa e a previsão E. [nao verificado]

Há também passivo já gravado:

| Pergunta | Declarado | Batida vinculada recebida |
|---|---|---|
| 52619 | 14:28 | 153845 · E 14:30. [nao verificado] |
| 53947 | 14:29 | 153848 · E 14:30. [nao verificado] |
| 55006 | 14:31 | 153851 · E 14:30. [nao verificado] |

Esses exemplos cabem no complemento de **DECLARADO-X-GRAVADO-PASSIVO-11**, obra existente em `app/docs/BACKLOG.md:1937`. O estado `aplicada` não comprova fidelidade ao declarado: nesse ramo a função retorna apenas pela presença de `materializada_em` (`app/chamados/services/materializacao.py:440`; `app/chamados/services/materializacao.py:448`).

**Decisões do admin.**

- **46751:** conferir o fato de 20/10 e a entrada E 07:00; a previsão recusa a aplicação por paridade. [nao verificado] A tela **Resolver dia** oferece Trabalhou, Faltou e Justificado; a view chama a porta de decisão do dia (`app/templates/ponto/partials/_veredito_dia.html:6`; `app/templates/ponto/partials/_veredito_dia.html:21`; `app/ponto/views.py:3340`). O êxito de uma dessas decisões neste caso não foi medido. [nao verificado]
- **53920 e 55313:** decidir se aceita o fato já registrado ou se precisa corrigir o fato. [nao verificado] Validar pelo fio pode quitar a pergunta, mas `ja_satisfeita` expressamente preserva o espelho; não grava a hora declarada (`app/chamados/services/materializacao.py:453`; `app/templates/chamados/partials/modal_fio.html:279`; `app/chamados/services/fio.py:585`).

**Dias retidos e roteiro.**

| Dia / célula | Dependência identificada | O que efetivamente tira a retenção |
|---|---|---|
| 21/10 · 141797 | 46756/46757: **b — DECLARADO-X-GRAVADO**; 46758 sem efeito posterior medido. [nao verificado] | Tratar o desencontro antes de validar; depois conferir nova lavra e retentor. A combinação que libera o dia não foi medida. [nao verificado] |
| 30/10 · 141806 | 52035 prevê S 15:50; existe S 21:28:20 no dado. [nao verificado] | **Não medido**: não foi calculado o veredito/retentor após o plantio. [nao verificado] |
| 03/11 · 141810 | 53565 e 53644 sem declaração. [nao verificado] | O colaborador informa o ocorrido e os horários; depois se confere aplicação e nova retenção. **Não medido** qual resposta libera o dia. [nao verificado] |
| 12/11 · 141819 | 57214 sem declaração. [nao verificado] | O colaborador informa o ocorrido e o horário pedido. **Não medido** o desfecho do dia. [nao verificado] |

A porta de resposta pertence ao colaborador e exige interação com o relógio para resposta de hora (`app/chamados/views.py:1228`; `app/chamados/views.py:1265`; `app/chamados/views.py:1274`).

**08/11 e 11/11 não retêm**, embora tenham perguntas vivas e células com `furo`; não acrescento esses dias à contagem. [nao verificado]

Para cobrir também as **71 perguntas históricas citadas**, todas recebem **não medido para nova validação**; permanecem na disputa 7263 aberta, mas individualmente estão `viva=false`. [nao verificado]

- **Hora, estado `aplicada` — pk=declarado:** 46755=12:55; 49271=15:51; 49272=07:00; 49273=13:00; 49650=13:00; 57212=14:30; 50103=15:50; 50104=07:00; 50105=13:00; 51082=FOLGA; 51124=13:00; 51142=14:30; 51218=21:18; 51747=15:50; 52221=07:03; 52265=13:00; 52283=14:30; 52367=15:50; 52542=06:57; 52594=13:00; 52619=14:28; 52727=15:56; 53947=14:29; 54029=15:52; 54245=07:00; 54292=13:01; 54317=14:29; 54393=15:51; 54592=FOLGA; 54634=FOLGA; 54664=FOLGA; 54936=06:57; 54980=13:00; 55006=14:31; 55104=15:55; 55518=15:52; 55786=13:00; 55812=14:30; 55950=15:50; 56313=13:00; 56342=14:30; 56466=15:50; 57131=07:00; 57184=13:00; 57199=14:30. [nao verificado] [nao verificado]
- **Hora, declarado nulo, estado nulo:** 48771, 48772, 49648, 49649, 50399, 50400, 50401, 50402, 50734, 50786, 50906, 51748, 51913, 53496, 53544, 53880, 54748, 55739, 56249, 56724, 56922 e 57553. [nao verificado]
- **Escolha, declarado nulo, estado nulo:** 53040. [nao verificado]
- **20/10, hora, históricas citadas apenas na FICHA:** 46752=13:01, 46753=07:00 e 46754=15:55. Estão materializadas via declaração; seu `estado_aplicacao` não veio no EXTRA. [nao verificado]

## Caso 03 -- col1032

**Comp 11: nenhum dia retido; nenhuma pergunta viva.** [nao verificado]

| Perguntas | Dia | Tipo / declarado | `estado_aplicacao` | Disputa | Classe |
|---|---|---|---|---|---|
| 57457, 57458, 57459, 57460 | 11/11 | hora / nulo | nulo | 8120 fechada | não medido; históricas. [nao verificado] |
| 57019 | 12/11 | escolha / nulo | nulo | 8097 fechada | não medido; histórica. [nao verificado] |
| 57129 | 12/11 | escolha / nulo | nulo | 8100 fechada | não medido; histórica. [nao verificado] |
| 57157 | 12/11 | escolha / nulo | nulo | 8102 fechada | não medido; histórica. [nao verificado] |
| 57279 | 12/11 | escolha / nulo | nulo | 8108 fechada | não medido; histórica. [nao verificado] |
| 57446 | 13/11 | hora / nulo | nulo | 8118 fechada | não medido; histórica. [nao verificado] |

As nove foram materializadas via `celula_absolveu`. As quatro batidas de 11/11 estão retratadas e não aprovadas; há abono aprovado 5574. Em 12/11 há quatro batidas aprovadas, e a célula consta `ok/concorde`; 13/11 consta `folga/concorde`. [nao verificado]

**Roteiro:** nenhum dia retido a retirar nesta fotografia. A divergência de tela aguardando print permanece sem diagnóstico adicional. [nao verificado]

## Caso 04 -- col246

**Comp 11: retém somente 01/11, célula 136215, por `espelho_cobranca_indevida`. Há duas perguntas vivas na disputa 7755 fechada.** [nao verificado]

| Pergunta | Motivo | Tipo / declarado | `estado_aplicacao` | Disputa | Classe exclusiva |
|---|---|---|---|---|---|
| 52485 | batida fora de escala grave | escolha / nulo | nulo | 7755 fechada | **b — ESPERA OBRA FIO-E0-V4-E3-LEITORES**, com conferência do passivo E4B. [nao verificado] |
| 52715 | `orfao_14h` | hora / nulo | nulo | 7755 fechada | **b — ESPERA OBRA FIO-E0-V4-E3-LEITORES**, com conferência do passivo E4B. [nao verificado] |

O complemento cabe na **E3-LEITORES**, que reúne fio, tela e app sob `tarefa_do_dia`, e na **E4B**, que cobre o passivo do fio morto (`app/docs/BACKLOG.md:1899`; `app/docs/BACKLOG.md:1921`). A linha da E4B registra pouso; isso não demonstra execução ou cura deste caso.

**Qual chamado faz a cobrança?** O dado lista o **36424**, `turno_aberto`, em análise, associado a 01/11; a justificativa 3432 está rejeitada. O pai 36324 está resolvido. [nao verificado] O cartório obtém as cobranças pelos módulos de furo e pelo dia do contexto, não simplesmente pelo pai da disputa (`app/ponto/services/cartorio.py:127`; `app/ponto/services/cartorio.py:135`).

**O elo exato pergunta→chamado não foi medido:** faltam `chamado_destino`, relacionados, contexto, ata e protestos. Não certifico a FK **52715→36424**, nem que 52485 seja a autora da cobrança. [nao verificado] O veredito `cobranca_indevida` pode derivar de dois códigos diferentes; a sonda não os discriminou (`app/escala/models.py:1613`).

**Por que `celula_absolveu` não encerrou:**

1. `cobranca_indevida` é veredito acusador (`app/escala/models.py:1767`).
2. A seleção de perguntas absolvíveis exclui células acusadoras (`app/chamados/reconciliador.py:642`).
3. O juiz C6 mantém viva a pergunta sob esse veredito (`app/chamados/juizes.py:1450`).
4. **A disputa fechada não é a causa dessa exclusão:** `celula_absolveu` alcança também disputas fechadas (`app/chamados/reconciliador.py:572`). Já a alternativa de órfãs por lâmpada filtra somente disputas abertas (`app/chamados/reconciliador.py:1050`).

**Sítio que deveria encerrar a cobrança:** quando reconhece `COBRANCA_SEM_FURO` ou `COBRANCA_EM_DIA_SEM_TRABALHO`, o cartório chama a reconciliação canônica e agenda rejulgamento (`app/ponto/services/cartorio.py:1565`; `app/ponto/services/cartorio.py:1573`; `app/ponto/services/cartorio.py:1576`). Há também encerramento por marco satisfeito (`app/ponto/services/cartorio.py:1539`).

**Roteiro do dia 01/11 — não medido:** conferir a âncora das duas perguntas e o resultado de `tarefa_do_dia`/alcance E4B; identificar a cobrança efetiva, conferir sua reconciliação e o rejulgamento; então consultar novamente o retentor. O resultado dessas portas e a razão histórica de não terem encerrado 36424 não constam do dado. [nao verificado] O backfill E4B não executa rejulgamento: registra `rejulgar_janela`, portanto não basta presumir que ele liberará a folha (`app/chamados/management/commands/backfill_fio_morto_e4b.py:19`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:148`).

As nove históricas citadas têm declarado e estado nulos, `viva=false`, classe **não medido para nova validação**. [nao verificado]

| Perguntas | Tipo | Disputa fechada |
|---|---|---|
| 49603; 49645 | escolha; hora | 7452. [nao verificado] |
| 52482 | hora | 7755. [nao verificado] |
| 54190; 54192 | hora; escolha | 7891. [nao verificado] |
| 54880; 54882; 54986; 55097 | hora; escolha; hora; hora | 7956. [nao verificado] |

## Caso 05 -- col265

**Comp 11: retém somente 31/10, célula 135811, por `espelho_cobrado`. Há três perguntas vivas.** [nao verificado]

| Pergunta | Dia / motivo | Tipo / declarado | `estado_aplicacao` | Disputa | Classe exclusiva |
|---|---|---|---|---|---|
| 52184 | 31/10 · batida fora de escala grave | escolha / Confirma | nulo | 7715 fechada | **c — DECISÃO DO ADMIN**. [nao verificado] |
| 52362 | 31/10 · `orfao_14h` | hora / nulo | nulo | 7715 fechada | **b — ESPERA OBRA RAIZ-DA-DISPUTA**. [nao verificado] |
| 55248 | 08/11 · batida fora de escala grave | escolha / Confirma | nulo | 7984 aberta | **c — DECISÃO DO ADMIN**. [nao verificado] |

**52184 e 55248:** o admin decide se aceita a confirmação, pelo botão **Validar** no fio (`app/templates/chamados/partials/modal_fio.html:279`). A disputa fechada não impede, por si, validar uma pergunta viva: a porta recupera sua própria disputa e permite o ato sem reabrir a raiz (`app/chamados/services/fio.py:585`; `app/chamados/services/fio.py:600`).

Para escolhas, `estado_aplicacao=None` significa fora do escopo dessa previsão (`app/chamados/services/materializacao.py:438`). A materialização da escolha não cria nem remove batidas e reconcilia destino/clones (`app/chamados/services/materializacao.py:1069`; `app/chamados/services/materializacao.py:1082`). Portanto, nenhuma das duas recebe classe a.

**52362:** falta declaração e a dona está fechada. [nao verificado] A consulta do app coloca em pendentes somente perguntas sem resposta **e** em disputa aberta; as demais vivas ficam no histórico (`app/chamados/services/pergunta_no_app.py:61`; `app/chamados/services/pergunta_no_app.py:71`). O complemento cabe em **RAIZ-DA-DISPUTA**, que já contém a decisão sobre dona fechada — reabrir, nova ou Pauta — sem criar id (`app/docs/BACKLOG.md:1931`).

**Roteiro do dia 31/10 — não medido:** decidir 52184 e restabelecer o caminho exigível da 52362 são ações identificadas; não foi demonstrado que elas bastem para liberar a célula. [nao verificado] O cartório só escolhe `cobrado` nesse ramo quando há fio que protesta; isso depende dos marcos e da ata (`app/ponto/services/cartorio.py:1458`; `app/ponto/services/cartorio.py:1479`; `app/ponto/services/cartorio.py:1502`). Esses insumos e o julgamento posterior não vieram na sonda. [nao verificado]

Não atribuo automaticamente a retenção ao chamado 36213 nem à pergunta 52362: o dado não fornece os vínculos e a ata necessários para essa identificação. [nao verificado]

**08/11 não retém**, apesar da 55248 viva em disputa aberta. O atestado 5594 de 13–14/11 está aguardando decisão, mas não aparece como retenção no resultado fornecido. [nao verificado]

As nove históricas citadas recebem classe **não medido para nova validação**:

| Perguntas | Tipo / declarado | `estado_aplicacao` | Disputa fechada |
|---|---|---|---|
| 50066 | escolha / Confirma | nulo | 7504. [nao verificado] |
| 50096 | hora / nulo | nulo | 7504. [nao verificado] |
| 51554, 51648, 51662, 51744 | hora / nulo | nulo | 7639. [nao verificado] |
| 52174 | hora / nulo | nulo | 7715. [nao verificado] |
| 55244, 55505 | hora / nulo | nulo | 7984, atualmente aberta. [nao verificado] |

## Contagem do lote

Contagem das **perguntas vivas**, com uma classe por pergunta. [nao verificado]

| Caso | a | b | c | d | não medido | Total |
|---|---:|---:|---:|---:|---:|---:|
| 01 | 0 | 0 | 0 | 0 | 0 | 0 |
| 02 | 0 | 4 | 3 | 0 | 9 | 16 |
| 03 | 0 | 0 | 0 | 0 | 0 | 0 |
| 04 | 0 | 2 | 0 | 0 | 0 | 2 |
| 05 | 0 | 1 | 2 | 0 | 0 | 3 |
| **Total** | **0** | **7** | **5** | **0** | **9** | **21** |

Contagem dos **colab-dias retidos**, classificando a dependência demonstrada; o efeito de liberação permanece não medido onde indicado no roteiro. [nao verificado]

| Caso | a | b | c | d | não medido | Total |
|---|---:|---:|---:|---:|---:|---:|
| 01 | 0 | 0 | 0 | 0 | 0 | 0 |
| 02 | 0 | 1 | 0 | 0 | 3 | 4 |
| 03 | 0 | 

[truncado em 20000 de 20509 caracteres]

## 13/11 17:40 · sessao e9788c95541f · - · - · origem=Ronald (tmux)

Classe presente: o contador novo não recebe `agora` e chama `qs_na_fila` sem passá-lo (`app/chamados/juizes.py:2511`, `2530`); a função então lê `timezone.now()` (`app/chamados/services/painel.py:190`). Isso contraria o transporte de instante exigido para julgamento (`app/docs/PLANO-PADROES.md:92-94`).

Não o classificaria como bloqueante principal: seu único chamador encontrado é o teste (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:75`); divergência em produção **[não verificado]**.

Contraexemplo de composição: julgamento captura instante `t`; um pai está silenciado até `t+1s`; contador roda em `t+2s`. A fila em `t` exclui o pai, mas o contador inclui. Sua assinatura impede receber `t` (`app/chamados/juizes.py:2511`; `app/chamados/services/painel.py:190`). O congelamento global no teste oculta essa diferença, advertência expressa do padrão (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:35`; `app/docs/PLANO-PADROES.md:91`).

## 13/11 17:39 · sessao 2ff33ed395d2 · BO-[nome]-1311-ROTEIRO · sonda · origem=Code

```text
sonda_leitura: sonda_roteiro.py nao existe
```

Essa é a saída inteira da tentativa local; terminou com código 1, antes da execução da sonda, na verificação de existência do arquivo. `bin/sonda_leitura.sh:71`

Recebi `BYTES_DA_SAIDA 232192`, `ERROS {}` e os anexos de aval e juízes. Esses são registros recebidos na mensagem, sem arquivo local com linhas; abaixo ficam identificados por **[nao verificado]**. Não houve nova leitura de produção nem saída integral de `sonda_roteiro_b.py` produzida nesta sessão. **[nao verificado]**

A correspondência recebida é 06→col982, 07→col1004, 08→col523, 09→col866 e 10→col859; FICHA e EXTRA não apresentam troca entre esses pares. A identidade subjacente não foi conferida. **[nao verificado]**

O juiz da retenção é `motivos_retencao_celula`, que chama `_dias_sem_decisao`: lê veredito, protestos e a exceção de furo somente de pausa. Portanto, pergunta encerrada ou `estado_aplicacao=aplicada` não demonstra, sozinho, que o dia deixou de reter. `app/folha/export.py:1176`, `app/folha/export.py:1212`, `app/folha/export.py:1273`, `app/folha/export.py:1293`

## Caso 06 -- col982

As cinco perguntas vivas recebidas estão abaixo; quatro pertencem à comp 11 e uma, de 20/10, está fora dela. A disputa #6546 permanece aberta. **[nao verificado]**

| Pergunta | Dia | Tipo / declarado | estado_aplicacao | Classe |
|---|---|---|---|---|
| #47164 | 20/10 | hora / FOLGA | invalida: escape FOLGA_CONTESTA | **c — DECISAO DO ADMIN** |
| #47163 | 21/10 | hora / FOLGA | invalida: escape FOLGA_CONTESTA | **c — DECISAO DO ADMIN** |
| #50866 | 27/10 | hora / FOLGA | invalida: escape FOLGA_CONTESTA | **c — DECISAO DO ADMIN** |
| #50968 | 27/10 | hora / FOLGA | invalida: escape FOLGA_CONTESTA | **c — DECISAO DO ADMIN** |
| #51712 | 29/10 | hora / FOLGA | invalida: escape FOLGA_CONTESTA | **c — DECISAO DO ADMIN** |

Os valores individuais da tabela vêm do EXTRA recebido. **[nao verificado]** A classe c decorre da porta real: FOLGA_CONTESTA segue para aceite humano de folga antes da materialização de hora. O admin decide se aceita a contestação da escala; o aceite pode declarar folga ou reconhecer uma folga já lançada. `app/chamados/services/validacao.py:234`, `app/chamados/services/validacao.py:239`, `app/chamados/services/validacao.py:280`, `app/chamados/services/validacao.py:320`, `app/chamados/services/validacao.py:412`

A tela é o modal do fio: ação de validar a resposta, ou rejeitá-la com motivo obrigatório. A disponibilidade efetiva para o usuário de produção não foi medida. `app/templates/chamados/partials/modal_fio.html:279`, `app/templates/chamados/partials/modal_fio.html:289` **[nao verificado]**

**Retenção:** `retem_comp11=[]`; nenhum colab-dia retido neste caso no retrato recebido. Não há dia da comp 11 a liberar. **[nao verificado]**

**Regeneração e os três dias.** As referências antigas do aval não apontam para recusas nesta cópia: `leitor_celula.py:235` é a docstring de `remapear_na_regua`; `portas/celula.py:88` é comentário sobre a ata. `app/escala/services/leitor_celula.py:234`, `app/ponto/portas/celula.py:87`

A guarda atual de competência lavrada, em `regenerar_celulas_vinculo`, testa se o dia pertence a uma janela de `_exp` e o pula. Essas janelas vêm de `_competencias_exportadas`. `app/ponto/portas/celula.py:221`, `app/ponto/portas/celula.py:284`

O cálculo usa `janela_competencia` → `janela_fechamento` → `corte_da_empresa`, que lê `Empresa.dia_inicio_competencia`. Com corte 21, comp 10 significa **21/09–20/10**, e comp 11 significa **21/10–20/11**; essa guarda não usa mês de calendário. `app/ponto/management/commands/gerar_celulas.py:185`, `app/folha/export.py:47`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:54`

O dado não identifica os três dias recusados, o retorno da chamada nem o sítio efetivamente percorrido. Logo, **recusa concreta: nao medido; bug de mês de calendário: não provado**. A ausência desses dias em `celulas_regen` também não fornece o motivo de uma recusa. **[nao verificado]**

**Reatividade das quatro perguntas da comp 11.** As células de 21/10, 27/10 e 29/10 aparecem regeneradas, como folga/concorde, enquanto as quatro respostas FOLGA continuam vivas. **[nao verificado]** Isso é compatível com o C6: resposta preenchida é examinada antes da absolvição pela célula; a exceção de concordância com a luz depende de uma hora comparável, e sem ela retorna `None`. `app/chamados/juizes.py:1368`, `app/chamados/juizes.py:1417`

Portanto, a classe demonstrável é **resposta textual preservada para decisão humana após mudança da escala**; falha do evento de regeneração não foi medida. Já existe reação por evento e a obra **CONTRATO-20-DOIS-EVENTOS**, documentada como E2 no ar. `app/ponto/portas/celula.py:528`, `app/docs/BACKLOG.md:1711` **[nao verificado]**

Há também a obra de tela **FOLGA-CONTESTA-DIZ-MANDAR-PARA-CADASTRO**. Ela cabe como complemento sobre a apresentação dessas respostas; uma obra específica que determine encerrá-las automaticamente pela escala corrigida permanece **[nao verificado]**. `app/docs/BACKLOG.md:1517`

## Caso 07 -- col1004

Não há pergunta viva nem dia retido na comp 11 no retrato recebido. A disputa #7574 fechou em 13/11, por `veredito`; a #7565 já estava fechada como `vazia`. **[nao verificado]**

| Perguntas citadas | Dia | Tipo / declarado | estado_aplicacao | Situação / classe hoje |
|---|---|---|---|---|
| #50873 | 27/10 | hora / null | null | não viva; **nao medido**, sem validação pendente demonstrada |
| #50874, #50875 | 27/10 | hora / null | null | não vivas; **nao medido**, sem validação pendente demonstrada |
| #50879 | 27/10 | hora / null | null | não viva; **nao medido**, sem validação pendente demonstrada |
| #52386 | 31/10 | hora / null | null | não viva; **nao medido**, sem validação pendente demonstrada |
| #53312 | 02/11 | hora / null | null | não viva; **nao medido**, sem validação pendente demonstrada |

Os estados da tabela são os recebidos; não entram na contagem de perguntas vivas. **[nao verificado]**

**“Pergunta invalida”.** O detalhe local tem o sítio que recusa perguntas de disputa fechada: filtra `disputa__fechada_em__isnull=True` e devolve essa mensagem quando não encontra a pergunta. O GET também seleciona disputa aberta. `app/chamados/views.py:493`, `app/chamados/views.py:499`, `app/chamados/services/acoes_chamado.py:345`

A outra tela é o fio/modal: carrega perguntas vivas inclusive em disputas fechadas e permite a validação nessa condição se `pergunta_viva` confirmar. O formulário fica no modal. `app/chamados/services/fio.py:212`, `app/chamados/services/fio.py:595`, `app/templates/chamados/partials/modal_fio.html:279`

Assim, **a divergência entre portas está provada no código; qual tela respondeu historicamente ao usuário não foi medida**. O complemento cabe em **FIO-LE-O-JUIZ**, na fila 2 indicada pelo aval recebido. `app/docs/BACKLOG.md:793` **[nao verificado]**

**27/10 — JUSTIFICATIVA-PAGA-O-PERIODO.** O dado mostra justificativa #3331 `esqueci_saida`, aprovada, sem saída informada nem batida gerada; mostra somente a batida S #139122, às 18:59:53, e célula #143004 `ok`. Minutos realizados, período pago e marcos dispensados não vieram na sonda. **[nao verificado]**

O enquadramento desse dia já está documentado em **HJ-093** e **JUSTIFICATIVA-PAGA-O-PERIODO**, incluindo a afirmação documental de realizado zero. Isso sustenta **espera da obra para o problema do período pago**, sem converter as perguntas já encerradas em pendências atuais. `app/docs/BACKLOG.md:742`, `app/docs/BACKLOG.md:1872`

Há ainda código que coleta justificativas aprovadas por data e limpa os marcos ausentes do dia de trabalho. O cálculo financeiro efetivo de 27/10 continua **nao medido** neste dado. `app/escala/utils.py:1196`, `app/escala/utils.py:1254` **[nao verificado]**

## Caso 08 -- col523

| Pergunta | Dia | Tipo / declarado | estado_aplicacao | Disputa | Classe |
|---|---|---|---|---|---|
| #50057 | 25/10 | escolha / null | null | #7497 aberta | **b — ESPERA OBRA CONTRATO-DE-ENTRADA-DO-FIO** |

A pergunta está viva, com `celula_data=null` e `batida_origem=null`; o chamado é #34736. **[nao verificado]** O id da obra já existe no BACKLOG. `app/docs/BACKLOG.md:2100`

`foto_ausente` pertence à família BATIDA e pergunta pela presença no posto. `estado_aplicacao=null` aqui significa fora do escopo da função de aplicação de resposta de hora; não demonstra possibilidade de gravar ou validar. `app/chamados/catalogo/motivos.py:43`, `app/chamados/catalogo/perguntas.py:93`, `app/chamados/services/materializacao.py:438`

O defeito de contrato da família está localizado: o emissor retroativo grava `foto_ausente.batida_id`, enquanto o nascimento procura `ultima_batida.id`; o gerador consulta esse juiz de nascimento. `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`, `app/chamados/models.py:3461`, `app/chamados/services/disputa_emissao.py:325`

O contexto individual de #50057 e seu emissor não foram fornecidos; não está provado que essa pergunta nasceu naquele comando específico. **[nao verificado]**

**Retenção:** `retem_comp11=[]`; nenhum colab-dia retido a liberar. A pergunta sem episódio identificado fica como complemento da obra existente, sem id novo. **[nao verificado]** `app/docs/BACKLOG.md:2100`

## Caso 09 -- col866

As três perguntas vivas recebidas têm resposta vazia e pertencem a disputas fechadas. **[nao verificado]**

| Pergunta | Dia | Tipo / declarado | estado_aplicacao | Disputa | Classe |
|---|---|---|---|---|---|
| #46494 | 21/10 | hora / null | null | #7238 fechada | **nao medido** |
| #49113 | 23/10 | hora / null | null | #7382 fechada por desligamento | **nao medido** |
| #49440 | 23/10 | hora / null | null | #7382 fechada por desligamento | **nao medido** |

Os valores são os recebidos. **[nao verificado]** Não há declarado para comparar com uma gravação; validar sem resposta é recusado pela porta com “Aguardando resposta do colab”. O destino correto de cada pergunta — nova declaração, decisão sobre o dia ou correção cadastral — não foi medido. `app/chamados/services/validacao.py:393` **[nao verificado]**

As perguntas citadas sobre novembro estão assim: **[nao verificado]**

| Pergunta | Dia | Declarado | estado_aplicacao / lastro recebido | Classe hoje |
|---|---|---|---|---|
| #53718 | 02/11 | hora / 18:00 | aplicada; batida S #154171 às 18:00, vinculada à pergunta; não viva | **nao medido**, já aplicada | [nao verificado]
| #53714 | 04/11 | hora / 18:00 | aplicada, via `celula_absolveu`; não viva; sem batida vinculada a ela no dado | **nao medido** | [nao verificado]

A disputa #7840 está fechada. O dado de #53718 demonstra a saída declarada já gravada; não constitui uma validação a fazer hoje. **[nao verificado]**

Para #53714, `aplicada` não prova gravação de 18:00: a função retorna esse estado pela existência de `materializada_em`, salvo a exceção de paridade, antes de conferir o plantio. `app/chamados/services/materializacao.py:440`

**Família TIPO.** Em 04/11 há três batidas S: #145440 às 05:59:55, #145450 às 06:06:53 e #146051 às 17:55:28. O dado não demonstra qual delas deveria ser E, nem traz os marcos da ata ou trilha de inversão; isso é **nao medido**. **[nao verificado]**

O complemento de tipo E/S cabe na família já registrada por **O213** e nos leitores de tipo; a troca de tipo sem aviso possui a obra de fila 2 **TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO**. **TIPO-NA-FONTE** trata formato/acesso da ata, não determina por si a inversão dessas batidas. `app/docs/BACKLOG.md:1462`, `app/docs/BACKLOG.md:1070`, `app/docs/BACKLOG.md:741`, `app/docs/BACKLOG.md:1833`

**Calendário que oculta saídas.** O serviço separa entradas e saídas pelo tipo gravado, mas o template abre o bloco das marcações somente quando há entrada ou retratada. Um dia só com saídas, sem retratadas, não alcança o laço que mostra horários e “Inverter tipo”. Esse defeito está provado no código. `app/colaboradores/services/calendario.py:658`, `app/templates/colaboradores/partials/_calendario_grade.html:118`, `app/templates/colaboradores/partials/_calendario_grade.html:121`

A tela é `calendario_ponto`, que renderiza `calendario_partial.html`; este inclui a grade. O complemento cabe em **E4-CALENDARIO-L1**, com fila 2 conforme o aval recebido. `app/colaboradores/views.py:1938`, `app/colaboradores/views.py:1965`, `app/templates/colaboradores/calendario_partial.html:62`, `app/docs/BACKLOG.md:855` **[nao verificado]**

**Retenção:** `retem_comp11=[]`; nenhum colab-dia retido neste caso. **[nao verificado]**

## Caso 10 -- col859

A disputa #2391 está aberta. Há **41 perguntas vivas**: 24 anteriores à comp 11 e 17 dentro dela. **[nao verificado]**

As 24 históricas têm `tipo=hora`, `declarado=null`, `estado_aplicacao=null`, e permanecem vivas no EXTRA: **[nao verificado]**

| Dia | Perguntas vivas | Classe |
|---|---|---|
| 22/08 | #27518, #27519, #27520, #27521 | **nao medido**, as quatro |
| 24/08 | #27514, #27515, #27516, #27517 | **nao medido**, as quatro |
| 26/08 | #20961, #31399, #31400, #24778 | **nao medido**, as quatro |
| 28/08 | #27510, #27511, #27512, #27513 | **nao medido**, as quatro |
| 30/08 | #29885, #31397, #31398, #24748 | **nao medido**, as quatro |
| 15/09 | #41395, #41396, #41397, #41398 | **nao medido**, as quatro |

Não há declaração nem ficha desses dias que permita escolher o encaminhamento individual. A porta de validação exige resposta; não se pode prometer gravação ou liberação com esses campos vazios. `app/chamados/services/validacao.py:393` **[nao verificado]**

As perguntas da comp 11 estão assim: **[nao verificado]**

| Dia | Pergunta(s) / motivo | Tipo / declarado | estado_aplicacao | Classe |
|---|---|---|---|---|
| 05/11 | #54426 intervalo_saida; #54440 intervalo_volta | hora / FOLGA | invalida: escape | **c**, ambas |
| 07/11 | #55087 saida_sem_entrada; #55143 intervalo_saida; #55157 intervalo_volta; #55225 orfao_14h | hora / FOLGA | invalida: escape | **c**, as quatro |
| 09/11 | #55927 saida_sem_entrada; #55991 intervalo_saida; #56014 intervalo_volta; #56103 orfao_14h | hora / FOLGA | invalida: escape | **c**, as quatro |
| 11/11 | #56960 intervalo_saida; #56974 intervalo_volta; #57049 orfao_14h | hora / FOLGA | aplicada; via admin_decidiu; vivas | **c**, as três | [nao verificado]
| 12/11 | #57278; #57422 batida_fora_escala_grave | escolha / null | null | **nao medido**, ambas |
| 12/11 | #57587 intervalo_saida; #57588 intervalo_volta | hora / null | null | **nao medido**, ambas |

Os estados individuais vêm do dado recebido. **[nao verificado]** As treze respostas FOLGA são **DECISAO DO ADMIN**: decidir se aceita a contestação ou a rejeita com motivo. A porta de aceite antecede a materialização de hora, pode recusar a declaração ou declarar folga. `app/chamados/services/validacao.py:239`, `app/chamados/services/validacao.py:305`, `app/chamados/services/validacao.py:320`, `app/chamados/services/validacao.py:412`

O local é o modal do fio, nos controles de validar e rejeitar resposta com motivo. Nas três de 11/11, a tarefa é conferir e concluir o julgamento das respostas que a sonda ainda declara vivas; `aplicada/admin_decidiu` não autoriza contá-las como encerradas. `app/templates/chamados/partials/modal_fio.html:279`, `app/templates/chamados/partials/modal_fio.html:289`, `app/chamados/juizes.py:1417` **[nao verificado]**

A citada #56904, também de 11/11, declara FOLGA, consta aplicada por `admin_decidiu`, validada e **não viva**; fica como decisão já registrada, fora da contagem de vivas. **[nao verificado]** [nao verificado]

Para as quatro de 12/11, falta resposta; o resultado da escolha ou os horários de intervalo não foram medidos. O modal apresenta espera da resposta e a validação sem resposta é recusada. `app/templates/chamados/partials/modal_fio.html:318`, `app/chamados/services/validacao.py:393` **[nao verificado]**

**Dias que retêm e o que os tira:**

| Dia / célula | Retenção recebida | Encaminhamento |
|---|---|---|
| 07/11 / #127789 | espelho_cobrado | **c — decisão sobre a contestação FOLGA**, representada pelas quatro perguntas desse dia |
| 09/11 / #127791 | espelho_furo | **c — decisão sobre a contestação FOLGA**, representada pelas quatro perguntas desse dia |

Somente esses dois dias constam em `retem_comp11`. **[nao verificado]** O caminho de aceite é declarar folga pela porta; rejeitar a resposta não demonstra liberação do dia. Depois do ato, quem decide a saída da retenção continua sendo `motivos_retencao_celula`, por `_dias_sem_decisao`. **O efeito posterior não foi medido; nenhum dos dois é classe a.** `app/chamados/services/validacao.py:320`, `app/folha/export.py:1212`, `app/folha/export.py:1293` **[nao verificado]**

Em 05/11 e 12/11 há célula acusando, mas o retorno recebido não inclui esses dias na retenção. Os protestos necessários para explicar individualmente a isenção não vieram; a causa precisa é **nao medido**. **[nao verificado]** O código admite exceções para protestos não cobráveis e furo somente de pausa. `app/folha/export.py:1285`, `app/folha/export.py:1293`

**Complemento DECLARADO-X-GRAVADO.** A obra existe, com passivo de comp 11 em id próprio. O código substitui a hora livre de intervalo pela hora do marco. `app/docs/BACKLOG.md:1936`, `app/docs/BACKLOG.md:1937`, `app/chamados/services/materializacao.py:409`

Neste lote, porém, os intervalos vivos têm FOLGA ou resposta vazia. FOLGA é barrada pelo escape antes da substituição de horário; `aplicada` nas duas respostas de intervalo de 11/11 deriva do carimbo e não prova batida. Portanto, **não há delta declarado-versus-gravado demonstrado para esses intervalos**. O complemento é registrar essa limitação da amostra, sem classificá-los automaticamente como espera dessa obra. `app/chamados/services/materializacao.py:359`, `app/chamados/services/materializacao.py:438`, `app/chamados/services/materializacao.py:440` **[nao verificado]**

## Contagem do lote

Contagem sobre os campos `viva` e `retem_comp11` recebidos; perguntas citadas já não vivas ficam fora do denominador de perguntas vivas. **[nao verificado]**

| Caso | a | b | c | d | nao medido | Perguntas vivas |
|---|---:|---:|---:|---:|---:|---:|
| 06 | 0 | 0 | 5 | 0 | 0 | 5 |
| 07 | 0 | 0 | 0 | 0 | 0 | 0 |
| 08 | 0 | 1 | 0 | 0 | 0 | 1 |
| 09 | 0 | 0 | 0 | 0 | 3 | 3 |
| 10 | 0 | 0 | 13 | 0 | 28 | 41 |
| **Total** | **0** | **1** | **18** | **0** | **31** | **50** |

A soma é **0 + 1 + 18 + 0 + 31 = 50** perguntas vivas: 25 na comp 11 e 25 anteriores. **[nao verificado]**

| Classe | Colab-dias retidos na comp 11 |
|---|---:|
| a — VALIDA-HOJE | 0 |
| b — ESPERA OBRA | 0 |
| c — DECISAO DO ADMIN | 2 |
| d — CADASTRO | 0 |
| nao medido | 0 |
| **Total** | **2** |

Os dois colab-dias são col859–07/11 e col859–09/11; a classificação c identifica a decisão necessária, **sem afirmar que sua execução já foi aceita ou liberou a retenção**. **[nao verificado]**

## 13/11 17:39 · sessao e355e2da10f6 · - · - · origem=Ronald (tmux)

- **Gate e signal implementam B:** `_decisao_do_fechamento` chama `_gate_conversa_do_pai`, que consulta a mesma lista do CSF e deixa passar respostas validadas/materializadas; Defesa 1 usa `ato=ch.reconciliar`. (`app/chamados/models.py:686`, `app/chamados/models.py:718`, `app/chamados/models.py:1898`, `app/chamados/signals.py:43`)
- **B não vale globalmente:** desligamento, aceite de vínculo, regularização externa e tranca fecham por `encerrar`/`encerrar_por_competencia_trancada`, sem o gate da conversa; `encerrar` pergunta apenas pela pergunta viva. (`app/colaboradores/services/desligamento.py:163`, `app/colaboradores/services/vinculo.py:388`, `app/chamados/services/regularizacao_ext.py:206`, `app/chamados/services/competencia_trancada.py:209`, `app/chamados/models.py:1664`)
- Esses caminhos estão declarados em lista de exceções, citando outro aval; o literal desse outro aval não foi fornecido. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:3`, `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:27`) [não verificado]
- Há cron efetivo de desligamento: agenda chama comando, comando chama serviço, serviço fecha fora da guarda O168. (`app/config/crons.py:440`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`, `app/colaboradores/services/desligamento.py:163`)
- **Contraponto concreto à frota:** pergunta respondida, sem validação/materialização, com dia trancado e creditada ao chamado produz `pode_executar_csf=True`; porém o executor chama `retratar`, cuja retenção usa `ignorar_tranca=True` e devolve `False`. Portanto “liberado pelo juiz” não significa “efetivamente fechado”; os quatro trancados podem continuar retidos. (`app/chamados/models.py:1902`, `app/chamados/juizes.py:2033`, `app/chamados/juizes.py:2035`, `app/chamados/reconciliador.py:448`, `app/chamados/reconciliador.py:519`, `app/ponto/management/commands/supra_juiz.py:286`)
- A tranca é regra herdada do juiz existente, não novo predicado criado na obra; a retratação opta explicitamente por ignorá-la. (`app/chamados/models.py:1896`, `app/chamados/juizes.py:974`, `app/chamados/juizes.py:2024`)
- **Selo novo fraco demonstrado em memória:** executei o método AST `test_regularizacao_ext_so_fecha_sem_disputa_aberta` extraído, com `open` substituído por `StringIO`: original passou e mutante `if _disp` → `if not _disp` passou. O teste só confere ordem de linhas e presença de `Return`, sem conferir polaridade da condição. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:207`, `app/chamados/tests/test_o168_fechadores_fora_do_gate.py:217`, `app/chamados/services/regularizacao_ext.py:62`)
- Essa lacuna isolada **não prova fuga da suíte inteira**: já existe teste comportamental exigindo zero batidas com disputa aberta, além do caso sem disputa que exige uma batida; o mutante seria incompatível com ambos. Não executei Django. (`app/chamados/tests/test_guard_disputa_reg_externa.py:40`, `app/chamados/tests/test_guard_disputa_reg_externa.py:47`)

## 13/11 17:39 · sessao e9788c95541f · - · - · origem=Ronald (tmux)

**Bloqueante: item 4 do v6 permanece aberto.** A cura elimina a consulta por disputa para carregar perguntas, mas mantém consultas por resposta pendente: o queryset não usa `select_related` (`app/ponto/supra_juiz.py:558`); cada pendente chama a tranca (`app/chamados/models.py:1898-1902`), que acessa `p.disputa.colaborador` (`app/chamados/juizes.py:991`). O teste mantém exatamente **uma** pendente tanto com 1 quanto com 3 disputas (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47-48`), portanto não demonstra constância quando as três disputas possuem resposta pendente.

Contraexemplo: mesmo fixture, alterando apenas para deixar **todas** as N respostas sem validação. Cada pergunta carregada tem suas FKs frias; haverá leitura da disputa e do colaborador para cada uma. Mesmo sem `data_evento` — campo nullable (`app/chamados/models.py:2479`) e omitido no fixture (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:43`) — essas duas FKs são avaliadas antes de a tranca devolver falso (`app/chamados/juizes.py:991-992`, `1024-1027`). Inferência estática: 2 consultas de universo + 2 por pendente, isto é, 4 versus 8 no caso 1/3; números executados no banco **[não verificado]**.

Os tetos estão em **219/279**, com justificativa textual de +2 e referências a medição (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `210`). A medição referenciada **[não verificado]**; o texto reconhece “tranca só lida para resposta sem veredito; neste cenário 0”, logo não prova o caso pendente do contraexemplo (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`).

Há funções novas compartilhadas que satisfazem o critério literal proposto pelo pedido: `sem_veredito_entre` responde quais perguntas aguardam veredito e é lida pelo método registrado, pela retratação e pelo CSF (`app/chamados/models.py:1886`, `1889`; `app/chamados/juizes.py:2035`; `app/ponto/supra_juiz.py:567-568`); `perguntas_que_seguram_csf` responde a lista que segura e é lida pelo gate e pelo bool registrado (`app/ponto/supra_juiz.py:561`, `582`; `app/chamados/models.py:719`). Isso é extração de corpo e composição, **sem duplicação do predicado**, pois a condição material vive somente em `sem_veredito_entre` (`app/chamados/models.py:1898-1902`). Histórico e `padrao=` dos commits **[não verificado]**.

Verificações sem escrita executadas: `bin/tests/test_mapa_juizes.sh` passou; seu contrato cobra presença das referências no mapa (`bin/tests/test_mapa_juizes.sh:21-53`). O censo AST atual também coincidiu com sua lista fechada, usando as funções extraídas em memória (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:133-166`).

Limites dos selos: o censo só reconhece duas portas e não reconhece `ch.encerrar()` direto; o mutante sintético desse fechamento devolveu `Counter()` (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:68-87`, `124`). O detector do juiz acompanha auxiliares por chamadas diretas, mas não por alias local; um alias para auxiliar que lê `respondida_em`, acompanhado de chamada-isca ao juiz, escapou dos dois predicados AST (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:295-361`). Isso demonstra alcance limitado das catracas; bypass atual de produção por essas formas **[não verificado]**.

Não há nova leitura de relógio nas funções de produção acrescentadas auditadas: elas consultam modelos e autoridades existentes (`app/ponto/supra_juiz.py:552-582`; `app/chamados/models.py:699-740`, `1889-1902`; `app/chamados/juizes.py:2511-2531`).

## 13/11 17:38 · sessao 828a4e1c1441 · - · - · origem=Ronald (tmux)

Escritores/leitores auditados:

| Campo | Escritores | Leitores e significado |
|---|---|---|
| `resposta_colab` / `respondida_em` | Resposta normal grava par; resposta nova limpa validação anterior: `app/chamados/services/disputa_emissao.py:1467-1482`. Resposta automática de fato entregue por outro canal também grava par: `:1638-1657`. Reposição pela trilha grava somente carimbo: `:1661-1681`. Reabertura limpa texto, resposta e validação: `:1593-1633`. | Juiz conta carimbo **OU** texto: `app/chamados/models.py:1899-1902`. `respondida` e `estado` ainda exigem carimbo: `:3552-3554`, `:3097-3104`. Materialização exige texto como anuência: `app/chamados/services/materializacao.py:828-845`. |
| `validada_em` | Decisão humana: `app/chamados/services/validacao.py:150-164`; devolução desfaz: `:218-225`. Resposta nova desfaz: `app/chamados/services/disputa_emissao.py:1470-1482`. Porta de carimbo também preenche automaticamente por simetria, com texto e `resposta='validar'`: `app/chamados/models.py:3186-3193`. Reaberturas também limpam: `:1554-1555`, `:1613`. | Juiz considera qualquer valor como veredito: `app/chamados/models.py:1899`. Materializador seleciona resposta e validação preenchidas, materialização nula: `app/chamados/services/materializacao.py:1375-1385`. Portanto “validada” não comprova autor humano isoladamente. |
| `materializada_em` | Atribuidor único `_escrever_carimbo`: `app/chamados/models.py:3153-3157`; portas `carimbar`, `carimbar_identidade`, `descarimbar`: `:3159`, `:3224`, `:3259`. | Juiz deixa de segurar se preenchido: `:1899`. Campo também recebe encerramento por identidade/admin, sem criar batida: `:3106-3120`, `:3241-3246`; seu `help_text` “batida criada + chamado fechado” é mais estreito que os escritores: `:3449-3451`. |

Contra-exemplo concreto de significado/porta: `resposta_colab='07:00'`, `respondida_em=None`, `validada_em=None`, `materializada_em=None` segura pelo juiz (`models.py:1899-1902`), mas `validar_pergunta` devolve `sem_resposta` (`validacao.py:416-418`) e `recusar_pergunta` devolve `ja_pendente` (`:603-606`). O rótulo `respondida` também é falso (`models.py:3552-3554`). Existe reparo pela trilha (`disputa_emissao.py:1661-1681`); disponibilidade desse reparo na tela [não verificado]. A nova nota orienta ações incapazes de resolver esse estado (`models.py:731-735`).

Leitores do juiz existente: fechamento e prontidão da disputa (`models.py:1909`, `:1942-1950`), desligamento (`app/colaboradores/services/desligamento.py:141-143`), ficha (`app/colaboradores/services/ficha.py:169`), fila por causa (`app/chamados/services/fila_por_causa.py:225-227`), reconciliador (`app/chamados/reconciliador.py:1164-1173`) e competência trancada, esta explicitamente usando `ignorar_tranca=True` (`app/chamados/services/competencia_trancada.py:124-138`).

DINHEIRO: liberar no juiz não equivale a executar CSF. Executor consulta juiz (`app/ponto/management/commands/supra_juiz.py:257-263`), exige execução e desativação da trava T6.1 antes de retratar (`:279-295`). Retratação reconcilia, pode rejeitar justificativa, resolve perguntas e anexa nota (`app/chamados/reconciliador.py:519-541`).

O resolver não fabrica batida diretamente (`app/chamados/services/disputa_emissao.py:1884-1892`), mas carimba perguntas (`:1982-1987`) e pode fechar disputa. Fechar disputa agenda retomada de regularização externa (`app/chamados/signals.py:122-146`); a retomada materializa regularizações confirmadas (`app/chamados/services/regularizacao_ext.py:211-227`) e pode escrever batidas por `registrar_batida` (`:71-83`). Portanto não se pode certificar impacto financeiro zero apenas pela ausência de alteração no cálculo. Quantos dos 349 chegam a essa cadeia, quais competências e quais valores mudam [não verificado].

O juiz do export lê fechamento, escala e demais critérios de exportação (`app/folha/export.py:765-815`, `:830-878`); O168 não fornece evidência de antes/depois desses insumos. Competência 10 exportada versus 11 aberta dos 349 [não verificado].

## 13/11 17:37 · sessao a05fd392c053 · - · - · origem=Ronald (tmux)

O juiz de retenção é `motivos_retencao_celula`, que delega a `_dias_sem_decisao`; a decisão usa veredito acusador, protestos e a exceção da pausa, e não o simples estado `aplicavel` da pergunta (`app/folha/export.py:1212`; `app/folha/export.py:1273`).

- A exceção da pausa exige protestos exatamente `{'FURO_PARCIAL'}` e confirmação por `periodos_do_dia`: sem desalinhamento, sem ponta aberta, sem desconhecimento e com lâmpadas apagadas somente de pausa (`app/folha/export.py:1173`; `app/folha/export.py:1148`).
- `estado_aplicacao_pergunta` verifica derivação de tipo/hora, reutilização e paridade. Não projeta uma nova ata, veredito ou resultado de retenção (`app/chamados/services/materializacao.py:431`).
- A validação agenda recálculo; o cartório consulta `classificar_dia`, deriva e lavra o veredito. Só depois o retentor consegue julgar o novo estado (`app/chamados/services/validacao.py:444`; `app/ponto/services/fechamento.py:1807`; `app/ponto/services/cartorio.py:1464`; `app/ponto/services/cartorio.py:1851`; `app/ponto/portas/celula.py:970`).

Classificação proposta para as 16 perguntas vivas de col996; dados individuais abaixo vêm somente do texto fornecido, sem arquivo correspondente. [nao verificado]

| Pergunta | Classe proposta | Motivo |
|---|---|---|
| 46751 | c — DECISÃO DO ADMIN | `bloqueada_paridade`: conferir o fato de 20/10 e a vizinha E 07:00. [nao verificado] |
| 46756 | b — DECLARADO-X-GRAVADO | Intervalo de saída declarado 15:00; previsão E 15:00, incompatível com o papel S da saída da pausa. [nao verificado] |
| 46757 | b — DECLARADO-X-GRAVADO | Declarado 16:01; previsão E 14:30. [nao verificado] |
| 46758 | não medido | Previsão S 21:05 coincide com a hora; liberação do dia não foi projetada. [nao verificado] |
| 48773 | não medido / d se cadastro incluir resposta | Falta resposta da saída de intervalo de 22/10. [nao verificado] |
| 57211 | não medido / d se cadastro incluir resposta | Falta resposta da volta de intervalo de 22/10. [nao verificado] |
| 52035 | não medido | Previsão S 15:50 coincide; liberação de 30/10 não foi projetada. [nao verificado] |
| 53565 | não medido / d se cadastro incluir resposta | Falta resposta da volta de intervalo de 03/11; existe E 14:29:51 no dado. [nao verificado] |
| 53644 | não medido / d se cadastro incluir resposta | Falta resposta de `orfao_14h` de 03/11. [nao verificado] |
| 53920 | c — DECISÃO DO ADMIN | Declarado 13:01; `ja_satisfeita` por S 12:54. Aceitar a batida existente apenas quita a pergunta; corrigir o fato exige decisão. [nao verificado] |
| 55313 | c — DECISÃO DO ADMIN | Declarado 07:00; `ja_satisfeita` por E 06:58. Mesma distinção. [nao verificado] |
| 55375 | b — DECLARADO-X-GRAVADO | Declarado 14:30; previsão S 13:00. [nao verificado] |
| 55411 | b — DECLARADO-X-GRAVADO | Declarado 15:50; previsão E 14:30. [nao verificado] |
| 56923 | não medido / d se cadastro incluir resposta | Falta resposta da volta de intervalo de 11/11. [nao verificado] |
| 57210 | não medido | Previsão S 13:00 coincide; 11/11 já não consta retido. [nao verificado] |
| 57214 | não medido / d se cadastro incluir resposta | Falta resposta de `orfao_14h` de 12/11. [nao verificado] |

`DECLARADO-X-GRAVADO` existe em `app/docs/BACKLOG.md:1936`; o passivo de comp 11 tem obra própria em `app/docs/BACKLOG.md:1937`. O código efetivamente substitui a hora declarada pela hora do marco nos intervalos (`app/chamados/services/materializacao.py:409`). O tipo da pausa é S para saída e E para volta (`app/escala/models.py:1652`).

Para perguntas sem resposta, validar retorna `sem_resposta`; o colaborador informa a hora pela porta de resposta. Isso não prova ausência de cadastro de escala — recomendo **não medido** se a classe d significa estritamente cadastro de vínculo/marcos (`app/chamados/services/validacao.py:393`; `app/chamados/views.py:1228`; `app/chamados/views.py:1265`; `app/chamados/views.py:1274`).

A tela da decisão humana é **Resolver dia**, com Trabalhou/Faltou/Justificado; a view chama o despachante por data (`app/templates/ponto/partials/_veredito_dia.html:6`; `app/templates/ponto/partials/_veredito_dia.html:21`; `app/ponto/views.py:3312`; `app/ponto/views.py:3340`). A aceitação de batida existente sem alterar espelho está explícita em `app/chamados/services/materializacao.py:453`.

Os quatro dias retidos são 21/10, 30/10, 03/11 e 12/11. 08/11 e 11/11 não retêm no EXTRA, embora tenham perguntas vivas/furo. [nao verificado]

**Contagem conservadora das perguntas:** a=0, b=4, c=3, d=0, não medido=9; total=16. A contagem deriva das linhas acima e dos dados textuais fornecidos. [nao verificado]

## 13/11 17:36 · sessao 82f11e3d3f22 · - · - · origem=Ronald (tmux)

Caso 09: o defeito de tela está provado no código. O serviço separa `entradas` e `saidas` pelo tipo gravado (`app/colaboradores/services/calendario.py:658`); o template só abre o bloco de horários quando há entrada ou retratada (`app/templates/colaboradores/partials/_calendario_grade.html:118`). Portanto um dia somente com saídas fica sem horários e sem o botão “Inverter tipo”, situado dentro desse bloco (`app/templates/colaboradores/partials/_calendario_grade.html:121`). A view é `calendario_ponto` (`app/colaboradores/views.py:1938`), renderiza `calendario_partial.html` (`app/colaboradores/views.py:1965`), que inclui a grade (`app/templates/colaboradores/calendario_partial.html:62`). Complemento cabe em `E4-CALENDARIO-L1` (`app/docs/BACKLOG.md:855`), com fila 2 determinada pelo aval recebido. [nao verificado]

A família de tipo E/S deve ser distinguida de `TIPO-NA-FONTE`, que trata formato/acesso da ata (`app/docs/BACKLOG.md:1833`). Para E/S existem `O213`, sobre tipo do motivo contra tipo da ata (`app/docs/BACKLOG.md:1462`), e `GRUPO-1-LISTA-CODEX-T11`, sobre os leitores de tipo (`app/docs/BACKLOG.md:1070`). `HJ-092` foi encerrada como BATIDA e preserva o marco cobrado (`app/docs/BACKLOG.md:740`); o aviso da troca ficou em `TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO`, fila 2 (`app/docs/BACKLOG.md:741`). A sonda não fornece os marcos da ata nem trilha de flip de 04/11; qual saída estaria errada é “não medido”. [nao verificado]

`estado_aplicacao_pergunta` retorna `aplicada` para pergunta de hora com resposta e `materializada_em`, salvo a via específica de recusa por paridade (`app/chamados/services/materializacao.py:438`). Assim, `53714 = aplicada` não demonstra que declarou e gravou a mesma batida: a função encerra a leitura antes de derivar timestamp/tipo (`app/chamados/services/materializacao.py:448`). No dado, a via é `celula_absolveu`, e não há batida com `pergunta_origem=53714`. [nao verificado]

Caso 10: `DECLARADO-X-GRAVADO` já existe (`app/docs/BACKLOG.md:1936`), com passivo de comp 11 em obra própria (`app/docs/BACKLOG.md:1937`). O código efetivamente substitui a hora declarada do intervalo pela hora do marco (`app/chamados/services/materializacao.py:409`). Entretanto, as respostas `FOLGA` deste lote caem antes no bloqueio de escape, sem derivar batida (`app/chamados/services/materializacao.py:359`). Nenhum delta de hora pode ser provado para essas respostas. [nao verificado]

Crucial: `estado_aplicacao=invalida` não descreve toda a porta “Validar”. O validador reconhece `FOLGA_CONTESTA` (`app/chamados/services/validacao.py:234`) e desvia para `_aceitar_a_folga` antes da materialização (`app/chamados/services/validacao.py:408`). Essa é decisão humana sobre contestação da escala (`app/chamados/services/validacao.py:239`), compatível com classe **c**, sem afirmar classe a. Dia já folga recebe carimbo/fecho sem nova escrita do dia (`app/chamados/services/validacao.py:280`); nos demais, a porta consulta `checar_declaracao` e pode recusar (`app/chamados/services/validacao.py:305`), ou declara folga pela porta existente (`app/chamados/services/validacao.py:320`). O resultado posterior no juiz de retenção não foi medido. [nao verificado]

A tela do fio oferece validar (`app/templates/chamados/partials/modal_fio.html:279`) e rejeitar com motivo obrigatório (`app/templates/chamados/partials/modal_fio.html:289`); a apresentação chama o validador único (`app/chamados/services/acoes_disputa.py:13`). Para os retidos 07/11 e 09/11, o admin decide se aceita a contestação de folga ou a rejeita com motivo; a sonda não mede o resultado dessa decisão. [nao verificado]

“Resolver dia” no calendário possui opções e efeitos no template (`app/templates/ponto/partials/_veredito_dia.html:8`), e a view chama o serviço (`app/ponto/views.py:3340`). Porém sua exibição depende de `resolver_dia_oferecido` (`app/templates/colaboradores/partials/_calendario_grade.html:125`), falso nos dados enviados; portanto não afirmar que o botão está disponível nesses dias. [nao verificado]

A porta fecha perguntas e chamados pela função `fechar_o_fio_do_dia` (`app/ponto/services/veredito_celula.py:160`), mas resposta do colaborador pode continuar viva antes da leitura de via de identidade, salvo decisão explícita do admin na célula ou concordância com a luz (`app/chamados/juizes.py:1417`). Isso explica por que se deve preservar o `viva` da sonda, inclusive nas perguntas carimbadas `admin_decidiu`, sem recalcular sua classe a partir apenas dessa via. (`app/chamados/juizes.py:1425`)

## 13/11 17:36 · sessao 717053fe02e8 · - · - · origem=Ronald (tmux)

Caso05: recomendação de classes exclusivas: **52184=c; 52362=b RAIZ-DA-DISPUTA; 55248=c; dia retido31/10=nao medido quanto à ação que efetivamente o libera**. O fundamento de cada classificação segue abaixo; o dado de produção está apenas na mensagem, sem arquivo:linha [nao verificado].

- **52184**, escolha `Confirma`, viva, disputa7715 fechada; **55248**, escolha `Confirma`, viva, disputa7984 aberta: o admin decide se aceita a confirmação. A tela do fio oferece **Validar** em `app/templates/chamados/partials/modal_fio.html:279`; o processamento aceita viva de disputa fechada sem reabrir em `app/chamados/services/fio.py:585`, especialmente `:600-609`. A fechada oculta somente rejeitar/corrigir, conforme `app/templates/chamados/partials/modal_fio.html:289`. Portanto, não são bloqueadas pela mera raiz fechada. **Classe c** para ambas.
- Para escolhas, `estado_aplicacao_pergunta=None` significa fora do escopo dessa previsão: o retorno precoce está em `app/chamados/services/materializacao.py:438`. Validar escolha resolve **sem criar/remover batida** e reconcilia destino/clones, conforme `app/chamados/services/materializacao.py:1069` e `:1082-1086`. Não classificar a nem prometer que a célula deixe de reter.
- **52362**, hora, declarado nulo, viva na disputa7715 fechada: sem resposta, a validação recusa em `app/chamados/services/validacao.py:393`. A consulta do app coloca em pendentes somente pergunta sem resposta **e disputa aberta**; as outras vivas vão ao histórico, em `app/chamados/services/pergunta_no_app.py:61`, especialmente `:70-75`. **Classe b: ESPERA OBRA RAIZ-DA-DISPUTA**, por canal da pergunta em dona fechada; a obra já inclui decidir reabrir/nova/Pauta em `app/docs/BACKLOG.md:1931`. Não dizer que a sonda provou qual dessas opções será adotada.
- O complemento de05 cabe na obra existente **RAIZ-DA-DISPUTA**, elo raiz-filhos e reação do agregado, `app/docs/BACKLOG.md:1931`; **RAIZ-DA-DISPUTA-F0F1** registra que o bug de produção continua atéF2, `app/docs/BACKLOG.md:1909`. Não precisa de id novo.

**Dia31/10:** o dado informa célula135811, `cobrado`, único dia retido, mas não informa ata, protestos, `chamado_destino`, relacionados nem julgamento projetado [nao verificado]. Logo, não provar qual filho causou esse `cobrado` nem que validar52184 encerra52362 e libera o dia.

O juiz real da retenção lê célula acusadora e as exceções de protestos/pausa em `app/folha/export.py:1273`, com retenção em `:1293-1294`. O cartório escolhe `cobrado` quando não há códigos e há fio que protesta, conforme `app/ponto/services/cartorio.py:1479` e `:1502-1503`; `_protesta` consulta os marcos acesos em `:1458-1460`. A previsão da transição desse dia não consta da sonda: **nao medido**.

Existe alternativa humana de decidir o fato do dia em **Resolver dia** — trabalhou/faltou/justificado — `app/templates/ponto/partials/_veredito_dia.html:6`, opções `:21-25`, view `app/ponto/views.py:3312` e chamada do juiz `:3340`. Isso é porta existente; não afirmar sucesso antecipado para31/10.

A vitalidade não depende de disputa aberta: resposta sem validação pode permanecer viva em `app/chamados/juizes.py:1417-1429`, e célula acusadora também mantém viva em `:1450-1451`. Fechar a raiz deveria consultar o C7 em `app/chamados/models.py:1849`; não podemos reconstruir o estado das perguntas no instante do fechamento apenas pelos carimbos atuais [nao verificado].

## 13/11 17:36 · sessao cde62b864733 · - · - · origem=Ronald (tmux)

Caso 04, col246 — laudo de código, somente leitura.

- **Retenção:** `cobranca_indevida` pertence a `VEREDITOS_ACUSAM`; os códigos que produzem esse veredito são `COBRANCA_EM_DIA_SEM_TRABALHO` e `COBRANCA_SEM_FURO`. Fontes: `app/escala/models.py:1767`, `app/escala/models.py:1613`.
- **Quem protesta:** o cartório carrega chamados dos módulos `batida_ausente`, `turno_aberto`, `intervalo_pendente`, `entrada_bloqueada_turno_aberto`, agrupados pelo dia obtido do contexto pelo juiz `data_do_chamado`; `disputa_supervisao` não integra esse vocabulário. Fontes: `app/ponto/services/cartorio.py:127`, `app/ponto/services/cartorio.py:135`, `app/chamados/catalogo/furo_modulos.py:15`.
- No dado fornecido, **36424 é o único chamado vivo de módulo de batida associado a 01/11**; as perguntas **52485 e 52715 vivem na disputa 7755 já fechada**, com declarado e estado_aplicação nulos. Isso consta somente do texto recebido, sem arquivo de evidência local: **[nao verificado]**.
- **Não provar FK inexistente:** a sonda não entrega `contexto_json`, `ata/lampadas`, `protestos`, `chamado_destino` nem `chamados_relacionados_ids`. Assim, o elo exato `52715 → 36424`, a cobrança de marco específica e o código bruto lavrado não foram medidos: **[nao verificado]**. O código realmente precisa desses insumos: `app/ponto/services/cartorio.py:135`, `app/ponto/services/lastro.py:170`, `app/ponto/management/commands/supra_juiz.py:246`.

**Por que `celula_absolveu` não encerrou agora:**

1. `qs_perguntas_absolviveis` exclui células com qualquer `VEREDITOS_ACUSAM`; portanto exclui `cobranca_indevida`. Fonte: `app/chamados/reconciliador.py:642`.
2. O C6 mantém viva a pergunta cujo dia possui esse veredito acusador; carimbar não pode vencer o juiz: a porta desfaz o carimbo se a pergunta continuar viva. Fontes: `app/chamados/juizes.py:1450`, `app/chamados/models.py:3144`.
3. **Disputa fechada não é filtro do `celula_absolveu`**: ele alcança também perguntas em disputas fechadas. Logo, atribuir a falha desse carimbo apenas ao fechamento de 7755 estaria errado. Fonte: `app/chamados/reconciliador.py:572`.
4. A alternativa `fantasma` por lâmpada, para `orfao_14h`, consulta apenas perguntas em disputa aberta: a 52715 fechada fica fora desse universo. Fontes: `app/chamados/reconciliador.py:1041`, `app/chamados/reconciliador.py:1050`.
5. O resolver de chamado encerrado também filtra disputa aberta e ainda preserva perguntas vivas pelo C6. Fontes: `app/chamados/services/disputa_emissao.py:1909`, `app/chamados/services/disputa_emissao.py:1966`.

**Sítio que deveria encerrar a cobrança e relavrar:**

- O cartório, ao reconhecer `COBRANCA_SEM_FURO` ou `COBRANCA_EM_DIA_SEM_TRABALHO`, chama `transicionar_com_pauta(... ato=x.reconciliar)` e agenda rejulgamento. Fontes: `app/ponto/services/cartorio.py:1565`, `app/ponto/services/cartorio.py:1573`, `app/ponto/services/cartorio.py:1576`.
- Para cobrança de marco já satisfeito, há também fechamento em `app/ponto/services/cartorio.py:1539`. A sequência dos adiados é lavrar → E2 → reconciliar, seguida de segunda passada: `app/ponto/services/cartorio.py:1000`, `app/ponto/services/cartorio.py:1011`, `app/ponto/services/cartorio.py:1021`.
- Não afirmar que “regenerar resolveria”: a sonda não mede a execução dessas portas nem suas recusas, e o gate pode impedir fechamento se a premissa continuar viva. Fonte: `app/chamados/models.py:739`, `app/chamados/models.py:768`; resultado concreto **[nao verificado]**.

**Complemento nas obras existentes:**

- **FIO-E0-V4-E3-LEITORES**, `app/docs/BACKLOG.md:1899`: fio, tela, API/app e indicadores devem ler `tarefa_do_dia`.
- **FIO-E0-V4-E4B**, `app/docs/BACKLOG.md:1921`: passivo do fio morto; a linha registra o pouso do código, mas isso não comprova aplicação no caso.
- A porta E4B para pergunta viva sem resposta sob credor terminal é `reabrir_credor_de_pergunta_viva`; possui modo de leitura `executar=False`, recusa sem tentar outro credor e não carimba a pergunta. Fontes: `app/chamados/services/disputa_emissao.py:1805`, `app/chamados/services/disputa_emissao.py:1821`, `app/chamados/services/disputa_emissao.py:1849`.
- **Limite relevante:** o backfill E4B **não executa rejulgamento**; apenas registra `rejulgar_janela`. Portanto, alcançar o fio não comprova que o dia sai da retenção. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:19`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:148`.

**Classificação sugerida para consolidação:** as duas perguntas e o colab-dia podem ser apresentados como **(b) ESPERA OBRA FIO-E0-V4-E3-LEITORES**, com complemento de alcance/passivo na **FIO-E0-V4-E4B**; porém, declarar que essa obra concreta elimina a retenção seria **[nao verificado]**, pois faltam a classificação de `tarefa_do_dia`, a âncora de cada pergunta e o resultado das portas. Se o critério da contagem exigir a cura comprovada, usar **“nao medido”** para os três elementos. Fontes do escopo: `app/docs/BACKLOG.md:1899`, `app/docs/BACKLOG.md:1921`; falta de efeito medido **[nao verificado]**.

## 13/11 17:36 · sessao f61e3b552f5f · - · - · origem=Ronald (tmux)

Caso 07 — col1004:

- O sítio do detalhe local recusa validação em disputa fechada com “Pergunta invalida.”: `app/chamados/views.py:493-500`. Seu GET também só seleciona disputa aberta: `app/chamados/services/acoes_chamado.py:345-358`; apresentação em `app/templates/chamados/detalhe_local.html:307-310`.
- Fio e modal carregam perguntas vivas inclusive de disputas fechadas: `app/chamados/services/fio.py:212-239`. A validação nessa condição passa pelo juiz `pergunta_viva`: `app/chamados/services/fio.py:595-609`. Cascas: `app/chamados/views.py:659-674` e `app/chamados/views.py:951-954`; botão do modal em `app/templates/chamados/partials/modal_fio.html:279-284`.
- Qual tela efetivamente produziu a mensagem histórica não está medido na sonda. [nao verificado]
- Complementar a obra existente **FIO-LE-O-JUIZ**, `app/docs/BACKLOG.md:793`, com essa divergência de portas; não criar id.
- O enquadramento de 27/10 em **JUSTIFICATIVA-PAGA-O-PERIODO** já consta explicitamente com col1004, célula 143004 e justificativa 3331 em `app/docs/BACKLOG.md:742` e `app/docs/BACKLOG.md:1872`. Minutos realizados e marcos dispensados são afirmações dessas linhas documentais; não são campos medidos pelo dado raspado apresentado. [nao verificado]
- O código limpa todos os marcos ausentes e órfãs de dia de trabalho com justificativa aprovada em `app/escala/utils.py:1254-1259`; a coleta inclui justificativas aprovadas por data em `app/escala/utils.py:1196-1199`. O motor considera o conjunto de datas justificadas para faltas e abono em `app/ponto/motor_calculo_v2.py:2600-2608`.

Caso 08 — col523:

- Classe proposta para #50057: **(b) ESPERA OBRA CONTRATO-DE-ENTRADA-DO-FIO**, id existente em `app/docs/BACKLOG.md:2100`.
- A identidade de `foto_ausente` é família BATIDA: `app/chamados/catalogo/motivos.py:43`; a pergunta pede confirmação de presença e oferece “Confirma”/“Não estava”: `app/chamados/catalogo/perguntas.py:93-98`.
- O emissor retroativo entrega `contexto['foto_ausente']['batida_id']` em `app/ponto/management/commands/marcar_foto_ausente_retro.py:96-104`; o nascimento lê somente `contexto['ultima_batida']['id']` em `app/chamados/models.py:3461-3468`. O gerador consulta esse juiz em `app/chamados/services/disputa_emissao.py:325-328`. É a incompatibilidade de contrato descrita pela obra. `app/docs/BACKLOG.md:2100`
- O dado raspado não fornece o contexto da disputa nem identifica o emissor individual de #50057; não afirmar que essa pergunta veio daquele cron. [nao verificado]
- `estado_aplicacao=null` para escolha sem resposta significa fora do escopo dessa função, não uma previsão de validação: `app/chamados/services/materializacao.py:438-439`.
- Atenção geral à leitura da sonda: a função devolve `aplicada` por existir `materializada_em`, exceto na via de paridade; não confere ali se o declarado produziu uma batida correspondente. `app/chamados/services/materializacao.py:440-448`

## 13/11 17:35 · sessao 38ea1e7c6ee3 · - · - · origem=Ronald (tmux)

Caso 06 — conclusões de código para consolidação:

- As referências antigas do aval não localizam a recusa neste hash: `app/escala/services/leitor_celula.py:235` é a docstring de `remapear_na_regua`; `app/ponto/portas/celula.py:88` é comentário sobre donos da ata.
- A recusa real por competência lavrada fica em `app/ponto/portas/celula.py:284`: verifica se `d` pertence a alguma janela de `_exp`, incrementa barrados e pula o dia; `_exp` vem de `_competencias_exportadas` em `app/ponto/portas/celula.py:221`.
- Esse helper usa `janela_competencia(emp, mes, ano)` em `app/ponto/management/commands/gerar_celulas.py:185`. A função delega para `janela_fechamento` em `app/folha/export.py:47`, que usa `corte_da_empresa` em `app/ponto/janelas.py:54`. O corte lê `Empresa.dia_inicio_competencia` em `app/ponto/janelas.py:50`. Portanto essa guarda usa competência por corte da empresa, não mês de calendário.
- O dado fornecido não identifica os três dias recusados nem traz o resultado da chamada que os teria recusado; sítio efetivamente acionado, datas e motivo concreto: **[nao verificado]**. Não há prova de bug de mês calendário.
- As quatro perguntas vivas da comp11 são #47163 (21/10), #50866/#50968 (27/10), #51712 (29/10); há ainda #47164 em 20/10. Todas declaram FOLGA e têm `estado_aplicacao=invalida`, disputa #6546 aberta; isso está no EXTRA fornecido, sem arquivo:linha local **[nao verificado]**.
- A sobrevivência das respondidas FOLGA é explicada pelo juiz: resposta preenchida vem antes da absolvição pela célula e permanece viva salvo concordância com a luz, `app/chamados/juizes.py:1417`; sem hora comparável, a concordância devolve `None`, `app/chamados/juizes.py:1368`. Assim folga/concorde na célula não encerra automaticamente essa resposta textual pelo ramo atual.
- Regeneração já tem reação por evento: agenda rejunção e `feriado_reavaliar.reavaliar_dias` em `app/ponto/portas/celula.py:528`. Obra existente: `CONTRATO-20-DOIS-EVENTOS`, `app/docs/BACKLOG.md:1711`, documentada como E2 no ar. A sonda não prova falha desse evento **[nao verificado]**.
- Obra específica de tela aplicável: **FOLGA-CONTESTA-DIZ-MANDAR-PARA-CADASTRO**, `app/docs/BACKLOG.md:1517`, substitui a oferta de validar por encaminhamento a cadastro.
- **PERGUNTA-RESPONDIDA-E-MARCO-ACESO**, `app/docs/BACKLOG.md:1675`, já está no ar e trata concordância de hora com batida; seu código declara que respostas sem hora comparável ficam vivas, `app/chamados/juizes.py:1333`. Obra específica para encerrar FOLGA porque a escala corrigida confirmou a resposta: **[nao verificado]**.
- `retem_comp11` está vazio no EXTRA fornecido; nenhum dia de col982 retido na comp11 naquele retrato **[nao verificado]**.

## 13/11 17:34 · sessao 3fdf6792be15 · E3-FORMA-SQL · leitura · origem=Code

**A recontagem fecha em 260 colab-dias: 181 `A1resto` e 79 `A2entra`. Os 79 perderam uma porta existente no ATUAL; nenhum é “dia novo que o C17 não alcançava”. O anexo não demonstra quais dessas perdas contrariam os casos escritos, porque omite a porta de cada ação e sua ligação à âncora.** (`a1v4_det.txt:1–260`; `a2_entram_saem.txt:1–79`; `app/chamados/tests/test_e3_portas_tarefa.py:21–47,67–73`)

Referências abreviadas usadas abaixo:

- **V:n** = `a1v4_det.txt:n`, contando somente as 260 linhas `V4_DET` fornecidas.
- **T:n** = [app/chamados/tests/test_e3_portas_tarefa.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/test_e3_portas_tarefa.py):n.
- **S:n** = [app/chamados/services/tarefa_do_dia.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py):n.
- **J:n** = [app/chamados/juizes.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py):n.
- **P:n** = [app/chamados/management/commands/diff_e3_sombra.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py):n.

**1. Recontagem por label e dono registrado no anexo.** (V:1–260)

| Label | ESTRUTURA | CADASTRO | BATIDA | None | Total |
|---|---:|---:|---:|---:|---:|
| A1resto | 27 | 116 | 38 | 0 | **181** |
| A2entra | 44 | 6 | 29 | 0 | **79** |
| Total | **71** | **122** | **67** | **0** | **260** |

O par abaixo é o **fio publicado aos leitores**, incluindo a projeção `nada` quando não há porta. Não confundo esse `nada` com o fio interno da tarefa: a projeção descarta o fio interno quando `porta_da_tarefa` retorna `None`. (P:46–59; S:552–565) [nao verificado]

| Label | Subgrupo | ATUAL → FUTURO | ESTRUTURA | CADASTRO | BATIDA | Total |
|---|---|---|---:|---:|---:|---:|
| A1resto | B1 | colab → colab | 5 | 4 | 26 | **35** |
| A1resto | B2 | admin → colab | 15 | 111 | 11 | **137** |
| A1resto | B3 | chamado → colab | 7 | 0 | 0 | **7** |
| A1resto | B4 | admin → admin | 0 | 1 | 1 | **2** |
| A2entra | B5 | colab → nada | 13 | 0 | 4 | **17** |
| A2entra | B6 | admin → nada | 26 | 6 | 20 | **52** |
| A2entra | B7 | chamado → nada | 5 | 0 | 5 | **10** |

Contagens calculadas sobre V:1–260. As mudanças explícitas de rota subdividem esses pares assim:

| Label | Subgrupo de rota | n | Evidência |
|---|---|---:|---|
| A1resto | porta_fila → porta_pergunta | 3 | 130217/B1 (V:12), 134521/B4 (V:157), 120408/B2 (V:217) |
| A1resto | Sem diferença de rota registrada | 178 | V:1–260, excluídas as três linhas acima e A2entra |
| A2entra | porta_fila → sem porta | 74 | V:1–260, linhas A2entra exceto as cinco abaixo |
| A2entra | porta_pergunta → sem porta | 5 | 113307 (V:78), 130346 (V:161), 130348 (V:162), 128533 (V:216), 127477 (V:240) |

O V4 fornece diferenças, não os dois objetos completos. Nas linhas sem mudança de rota, seu valor concreto não está explicitado; reconstruir integralmente todos os pares de objetos permanece **[não verificado]**. (V:1–260)

**2. Cobertura pelos casos escritos e combinações de razões.**

Estes são os contratos efetivamente escritos no arquivo solicitado:

| Caso escrito | Fatos necessários | Resposta exigida pelo teste | [nao verificado]
|---|---|---|
| `test_rota_so_fila_sem_pergunta` | Marco apagado, sem pergunta; ação `perguntar/sistema/emissor` | **FUTURO:** nenhuma porta humana, fila vazia. (T:32–38) |
| `test_fila_A_pergunta_sob_B` | A residual na célula; pergunta ancorada em B fora dela; ação com porta B | **FUTURO:** porta da pergunta, somente B. (T:21–47) |
| `test_verbos_painel_A_B_lidos_no_universo` | Identificação de A/B e seus verbos no universo do painel | Preservar exatamente o verbo de B. (T:49–55) |
| `test_porta_fila_quando_ancora_e_da_celula` | Pergunta cuja âncora é o chamado da própria célula | **Ter porta:** `porta_fila`, chamado da ação e verbo `cobrar`. (T:67–73) |
| `test_verbo_nao_e_traducao_da_acao_da_tarefa` | B com `modulo_origem='gps_fora_geofence'` | Admite `responder/colaborador` e verbo `validar` simultaneamente. (T:75–82) |
| `test_isento_fila_residual_nao_cria_fio` | `isento_ponto=True` | **FUTURO:** nenhuma ação, rota ou fio. (T:57–65) |
| Testes de relógio/silêncio | Instantes e `silenciado_ate` especificados | Respostas dependentes desse relógio. (T:84–111) | [nao verificado]

Nas tabelas seguintes:

- **SC/H** = **sem caso escrito identificado para os fatos completos; ATUAL/FUTURO indeterminado**. Falta ligar cada pergunta/ação à identidade e localização da âncora, à sua porta e à pertença na fila global. Os pares `[ação,de]` do anexo não fornecem isso. (V:1–260; T:21–47,67–82; S:128–142,547–556)
- **SC/F** = **sem caso escrito para a combinação apresentada; ATUAL/FUTURO indeterminado**. Não está demonstrado o cenário `sem_pergunta + perguntar/sistema/emissor`, nem isenção, nem uma ação com a âncora exigida pelos testes de porta. (V:1–260; T:32–73) [nao verificado]
- Em grupos com **n=1**, apresento o único exemplo existente; um segundo pk não existe naquele grupo. (V:1–260)

Legenda das razões, preservando multiplicidade e ignorando somente a ordem:  
`A=aceso`, `P=passivo_sem_linha`, `R=aguarda_resposta`, `L=aguarda_validacao`, `N=nao_venceu`, `Q=sem_pergunta`, `D=devolvida_ao_colaborador`, `M=materializacao_recusada`, `W=dia_sem_trabalho`, `V=veredito_sem_lavra_vigente`, `J=encerrada_pelo_juiz`. (V:1–260)

**A1resto — combinações dentro de cada par.** Todas as contagens vêm de V:1–260.

| Par | Combinação | n | Exemplos de celula_pk | Caso/decisão |
|---|---|---:|---|---|
| B1 | AR³ | 10 | 141754 (V:1), 123295 (V:67) | SC/H |
| B1 | A³R | 3 | 138821 (V:31), 127911 (V:209) | SC/H |
| B1 | R⁴ | 6 | 129002 (V:35), 132452 (V:173) | SC/H |
| B1 | AP²R | 1 | 128690 (V:38) | SC/H |
| B1 | APR² | 1 | 138879 (V:52) | SC/H |
| B1 | AR | 6 | 130217 (V:12), 133458 (V:61) | SC/H |
| B1 | A²R² | 6 | 134428 (V:77), 138363 (V:99) | SC/H |
| B1 | A²DR | 1 | 129300 (V:85) | SC/H |
| B1 | ANR² | 1 | 143677 (V:259) | SC/H |
| **B1 total** | | **35** | | |
| B2 | R² | 18 | 124787 (V:4), 124794 (V:5) | SC/H |
| B2 | AP²R | 5 | 123615 (V:7), 114553 (V:59) | SC/H |
| B2 | PR | 5 | 117853 (V:8), 117087 (V:88) | SC/H |
| B2 | AL²R | 1 | 127078 (V:9) | SC/H |
| B2 | P³R | 4 | 140773 (V:14), 111053 (V:58) | SC/H |
| B2 | AR | 2 | 131733 (V:19), 131740 (V:21) | SC/H |
| B2 | RQ | 1 | 131734 (V:20) | SC/H |
| B2 | A²R² | 1 | 131151 (V:27) | SC/H |
| B2 | R⁴ | 53 | 131155 (V:28), 131160 (V:29) | SC/H |
| B2 | PR³ | 10 | 116157 (V:43), 137107 (V:101) | SC/H |
| B2 | A³R | 1 | 133551 (V:56) | SC/H |
| B2 | ALR² | 3 | 130970 (V:60), 132926 (V:62) | SC/H |
| B2 | A²MR | 1 | 115080 (V:86) | SC/H |
| B2 | A²PR | 5 | 114007 (V:91), 121906 (V:92) | SC/H |
| B2 | D³P | 5 | 131546 (V:95), 116101 (V:112) | SC/H |
| B2 | LR³ | 4 | 129663 (V:98), 111971 (V:156) | SC/H |
| B2 | D²R² | 6 | 116098 (V:111), 116112 (V:114) | SC/H |
| B2 | D³R | 2 | 116120 (V:117), 137851 (V:118) | SC/H |
| B2 | P²R² | 3 | 119780 (V:124), 108301 (V:175) | SC/H |
| B2 | D²P² | 1 | 116524 (V:151) | SC/H |
| B2 | APR² | 1 | 105147 (V:208) | SC/H |
| B2 | NR³ | 2 | 128414 (V:230), 127484 (V:241) | SC/H |
| B2 | LR | 1 | 127730 (V:231) | SC/H |
| B2 | AR³ | 1 | 127626 (V:237) | SC/H |
| B2 | A²LR | 1 | 143673 (V:257) | SC/H |
| **B2 total** | | **137** | | |
| B3 | R⁴ | 7 | 110014 (V:70), 110016 (V:71) | SC/H |
| **B3 total** | | **7** | | |
| B4 | A⁴L | 1 | 134521 (V:157) | SC/H |
| B4 | A²L² | 1 | 142942 (V:249) | SC/H |
| **B4 total** | | **2** | | |
| **A1resto total** | | **181** | | |

O caso 131734 contém `sem_pergunta`, mas também uma instância `aguarda_resposta`; portanto não reproduz integralmente o teste “só fila sem pergunta”. Sua cobertura parcial é T:32–38; a decisão do dia ainda depende da porta da ação humana. (V:20; S:302–304,547–556) [nao verificado]

Os 137 `admin→colab` não demonstram erro somente porque o verbo publicado é `validar`: o teste GPS admite expressamente essa combinação. Identificar esse caso nos registros exige o `modulo_origem` correspondente, ausente do V4. (V:1–260; T:75–82) [nao verificado]

**A2entra — combinações dentro de cada par.** Todas as contagens vêm de V:1–260.

| Par | Combinação | n | Exemplos de celula_pk | Caso/decisão |
|---|---|---:|---|---|
| B5 | A³R | 2 | 141714 (V:2), 141810 (V:247) | SC/H |
| B5 | A⁴ | 4 | 137154 (V:55), 129217 (V:136) | SC/F |
| B5 | A³P | 1 | 137166 (V:87) | SC/F |
| B5 | A² | 10 | 138912 (V:107), 138914 (V:108) | SC/F |
| **B5 total** | | **17** | | |
| B6 | A³P | 9 | 130162 (V:3), 140522 (V:10) | SC/F |
| B6 | AP² | 1 | 139432 (V:13) | SC/F |
| B6 | A²P² | 2 | 140707 (V:15), 129565 (V:167) | SC/F |
| B6 | P⁴ | 12 | 140928 (V:16), 104784 (V:17) | SC/F |
| B6 | AP | 7 | 119517 (V:25), 111845 (V:46) | SC/F |
| B6 | A⁴ | 8 | 140331 (V:26), 130931 (V:102) | SC/F |
| B6 | AP³ | 3 | 111346 (V:37), 122073 (V:195) | SC/F |
| B6 | A² | 5 | 135067 (V:48), 110506 (V:64) | SC/F |
| B6 | P² | 3 | 116436 (V:54), 124364 (V:126) | SC/F |
| B6 | WP | 2 | 130346 (V:161), 130348 (V:162) | SC/F |
| **B6 total** | | **52** | | |
| B7 | AP²R | 1 | 118931 (V:6) | SC/H |
| B7 | A⁴ | 3 | 129083 (V:49), 129090 (V:50) | SC/F |
| B7 | AP | 3 | 129745 (V:81), 129749 (V:82) | SC/F |
| B7 | AR³ | 1 | 129598 (V:90) | SC/H |
| B7 | A³V | 1 | 130771 (V:187) | SC/F |
| B7 | AJ | 1 | 142013 (V:248) | SC/F |
| **B7 total** | | **10** | | |
| **A2entra total** | | **79** | | |

**3. Regressão versus universo novo nos 79 A2entra.**

| Classe demonstrável | n | Lista |
|---|---:|---|
| Regra escrita demonstradamente exige porta, FUTURO não publica | **0 demonstrados** | Vazia; quantidade real de regressões **[não verificado]**. Faltam os fatos de âncora exigidos em T:40–47,67–73. |
| Dia novo que o C17 não alcançava | **0** | Vazia: todos têm porta no ATUAL. (`a2_entram_saem.txt:1–79`) |
| Perda de porta observada, obrigação normativa não demonstrada | **79** | Lista completa abaixo. (V:1–260, linhas A2entra) |

Não cabe fechar uma divisão binária `regressão + universo novo = 79`: o segundo conjunto é vazio, e a obrigação normativa do primeiro não foi demonstrada pelos fatos disponíveis. (V:1–260; T:21–82)

| Label / classe pendente | celula_pk — lista completa |
|---|---|
| A2entra — B5, 17 perdas `colab→nada` | 141714 (V:2), 137154 (V:55), 137166 (V:87), 138912 (V:107), 138914 (V:108), 138916 (V:109), 129217 (V:136), 129559 (V:164), 129561 (V:165), 129567 (V:168), 129569 (V:169), 129571 (V:170), 129573 (V:171), 129577 (V:172), 130781 (V:188), 130044 (V:207), 141810 (V:247) |
| A2entra — B6, 52 perdas `admin→nada` | 130162 (V:3), 140522 (V:10), 140524 (V:11), 139432 (V:13), 140707 (V:15), 140928 (V:16), 104784 (V:17), 119517 (V:25), 140331 (V:26), 129384 (V:30), 104040 (V:34), 133364 (V:36), 111346 (V:37), 111845 (V:46), 135778 (V:47), 135067 (V:48), 116432 (V:53), 116436 (V:54), 110506 (V:64), 138672 (V:68), 113307 (V:78), 106963 (V:93), 130931 (V:102), 130933 (V:103), 130935 (V:104), 130937 (V:105), 130939 (V:106), 140580 (V:125), 124364 (V:126), 124370 (V:127), 123033 (V:147), 33784 (V:148), 130346 (V:161), 130348 (V:162), 129563 (V:166), 129565 (V:167), 132144 (V:191), 132052 (V:192), 106594 (V:193), 106350 (V:194), 122073 (V:195), 130026 (V:205), 120325 (V:212), 128516 (V:213), 128519 (V:214), 128533 (V:216), 127477 (V:240), 124774 (V:245), 141797 (V:246), 143313 (V:252), 143393 (V:253), 143395 (V:254) |
| A2entra — B7, 10 perdas `chamado→nada` | 118931 (V:6), 129083 (V:49), 129090 (V:50), 129093 (V:51), 129745 (V:81), 129749 (V:82), 129751 (V:83), 129598 (V:90), 130771 (V:187), 142013 (V:248) |

O caminho da cópia que produz a ausência de porta é verificável: `_portas_da_tarefa` publica somente portas inteiras das ações pertencentes à fila global; sem nenhuma, define `rota=None`; `porta_da_tarefa` retorna `None`, projetado como `nada`. (S:547–565; P:49–59)

Dos 79, **75 não têm ações nas instâncias**. Os outros quatro têm `responder/colaborador`: 141714, 118931, 129598 e 141810. Isso não basta para exigir porta: a própria criação da ação admite `porta=None`, e há teste E2 esperando esse resultado durante silêncio. (V:2,6,90,247 e demais linhas A2entra; S:128–133; `app/chamados/tests/test_e2_tarefa_do_dia.py:396–409`)

**4. ATUAL obrigatório com dono ESTRUTURA — bloqueios L-099 demonstrados.**

| Label | n demonstrado | Lista completa demonstrável |
|---|---:|---|
| A1resto | **0** | `[]` — falta demonstrar o contrato aplicável às âncoras. (V:1–260; T:21–82) |
| A2entra | **0** | `[]` — mesma lacuna; único ESTRUTURA com ação humana é 118931. (V:6; T:40–47,67–73) |

**A lista real de bloqueios e sua quantidade permanecem [não verificado].** Zero demonstrados não libera a E3 nem transforma os 71 ESTRUTURA em comportamentos corretos. (V:1–260; T:21–82)

Para o caso ESTRUTURA 118931, o caminho concreto é: âncora só vira porta da ação se pertencer à fila; depois a publicação exige pertença também à alimentação global; sem porta publicada, a projeção vira `nada`. O anexo não fornece a âncora para decidir qual dessas condições ocorreu. (V:6; S:128–133,547–565; P:49) [nao verificado]

Nas mudanças de fio de A1resto, há outra diferença explícita: C17 calcula fio pelos **verbos**; a tarefa calcula fio pelos **donos das ações**. Essa diferença produz `admin/chamado→colab` sem, por si, demonstrar violação dos testes. (J:1624–1639,1687–1688; S:552–556; T:75–82) [nao verificado]

**5. As +9626 exceções e o agregado 26→28.**

**Existe `RespondidaSemTexto`, mas a hipótese “cada `True` do leitor levanta essa exceção” não corresponde ao comportamento do predicado de leitura.** `respondida_sem_texto` apenas retorna booleano; a guarda de escrita é outra função. (J:1515–1518; `app/chamados/services/fio.py:1595–1605`)

| Pergunta | Constatação pelo código |
|---|---|
| Qual exceção existe? | `RespondidaSemTexto`, subclasse de `ResolvedoraCruzada`, por sua vez subclasse de `ValueError`. (`app/chamados/models.py:2293,2325`) |
| Onde é levantada? | A guarda consulta o predicado em `fio.py:1602`; o **raise está em `:1604`**. (`app/chamados/services/fio.py:1595–1605`) |
| Quem chama a guarda? | Nascimento de pergunta; resposta automática; transição do par no `save`; descarimbo em lote. (`app/chamados/models.py:2864–2867,3288–3298,3502–3509`; `app/chamados/services/disputa_emissao.py:1653–1664`) |
| Só conta? | **Não nesse caminho:** recusa a escrita antes de salvar. Há testes verificando estado preservado após a recusa. (`app/chamados/models.py:3471–3477`; `app/chamados/services/disputa_emissao.py:1657–1664`; `app/chamados/tests/test_e3_10_cura_no_escritor.py:155–177,193–206`) |
| Quem captura na sonda das +9626? | **[não verificado]**. O anexo não contém classe nem pilha; o código da sonda e seu contador não estão disponíveis nesta cópia. |
| Há captura local nos caminhos citados? | Nascimento sob contrato e resposta automática deixam a exceção propagar nesses trechos. (`app/chamados/models.py:2840–2843`; `app/chamados/services/disputa_emissao.py:1657–1665`) |
| E o instrumento `diff_e3_sombra`? | Captura genericamente `Exception` e interrompe com `CommandError`; esse trecho não transforma a falha em mera contagem. (P:101–106) |
| A tarefa levanta essa exceção ao ler uma decisão vazia? | Não nesse ramo: produz razão textual `respondida_sem_texto` e ação `responder/colaborador`. (S:135–138) |

A igualdade numérica **9626 = histograma `True`** está no anexo, com o mesmo histograma na base e na cópia. Como os histogramas por classe estão vazios, atribuir o delta à classe `RespondidaSemTexto`, ao seu capturador ou a um efeito exclusivamente contábil permanece **[não verificado]**. (`a2_excecoes_por_leitor.txt:8–16,21–27`)

O predicado booleano pode participar de decisão de porta: C17 o consulta antes de acrescentar a âncora da pergunta. Isso é diferente de levantar a exceção de escrita. Não há evidência anexada de que uma exceção capturada tenha alterado porta, fila ou retenção nessas execuções. (J:1679–1688; **[não verificado]** quanto ao efeito das exceções medidas)

**O agregado 26→28 não compara o mesmo identificador de leitor:** a base registra `vigia.censo_alcance::observado::alimentar_portas`; a cópia registra `vigia.censo_alcance::observado::carga.tarefa`. (`a2_excecoes_por_leitor.txt:35,48–54`)

**6. Escritores, leitores e significados adicionais dos campos.**

As razões `sem_pergunta`, `devolvida_ao_colaborador` e `materializacao_recusada` aparecem no V4. A ausência delas no V3 bruto permanece **[não verificado]**, pois foi fornecido o julgamento V3, não suas linhas originais. (V:20,85–86,95,111–118,151,180–183)

| Campo/fato do V4 | Quem escreve | Quem lê / significado |
|---|---|---|
| `razao=sem_pergunta`; `perguntar/sistema` | A tarefa escreve a razão e a ação com porta `emissor`. (S:293–304) | A publicação lê a ação; a porta string não entra no conjunto de portas inteiras. O backfill reconhece `perguntar`. (S:547–556; `app/chamados/management/commands/backfill_fio_morto_e4b.py:100–113`) |
| `razao=devolvida_ao_colaborador` | `_pela_decisao`, por último ato `RECUSA` ou `REABERTURA`, escreve ação `responder/colaborador`. (S:139–140) | Publicação lê ação/porta, não essa razão; backfill agrega razões no diagnóstico. (S:547–556; `app/chamados/management/commands/backfill_fio_morto_e4b.py:116–117`) |
| `razao=materializacao_recusada` | Último ato `DEVOLUCAO` escreve ação **validar/admin**. Não escreve ação `materializar`. (S:141–142) | Mesmo caminho de publicação das ações e diagnóstico. (S:547–556; `app/chamados/management/commands/backfill_fio_morto_e4b.py:100–117`) |
| `lampada` de marco normal | Cartório monta `ata.lampadas[].acesa`; leitor preserva `acesa_lavrada`; tarefa copia o valor. (`app/ponto/services/cartorio.py:672–678,711–738`; `app/escala/services/leitor_celula.py:134–137`; S:87–105,258–260) | A tarefa distingue `True`, `None` e apagada; a dispensa por acesa precede o teto. (S:262–282) |
| `vencido` de marco normal | Cartório calcula `quando + 30 minutos <= agora`; carga repassa; tarefa casa por tipo/hora. (`app/ponto/services/cartorio.py:862,882–893`; `app/ponto/turnos.py:3060–3066`; S:74–105) | `None` significa teto não casado; `False`, ainda não vencido. (S:276–282) |
| `lampada=None`, `vencido=None` em linha fora da grade | A tarefa constrói linha sintética e depois chama `expor_admin`. Exemplos: 139432, 134521 e 129565. (V:13,157,167; S:314–323) | Esses `None` significam ausência de slot nessa linha; **não percorrem** os ramos `lampada_nao_sabe`/`marco_fora_do_turno` do laço anterior. (S:256–278,314–323) |
| `acoes` dessas linhas sintéticas | `expor_admin` consulta `_pela_decisao`; publica ações administrativas, ou registra razão indeterminada sem inventar ação. (S:243–255,318–323) | `_portas_da_tarefa` lê porta e dono das ações; o backfill reconhece também humanos sem porta. (S:547–556; `app/chamados/management/commands/backfill_fio_morto_e4b.py:100–115`) |

Há duas distinções semânticas verificáveis:

- **`lampada=None` de marco normal:** o leitor usa uma interpretação de `None` como acesa para `missing`, mas preserva `None` em `acesa_lavrada`, consumido pela tarefa. (`app/escala/services/leitor_celula.py:119–137`; S:87–105,272–275)
- **Ausência de porta:** os leitores da folha e da pauta consultam a porta publicada, não os campos `razao/lampada/vencido/acoes` individualmente. Assim, uma ação humana sem porta pode virar `sem_fio`; a descrição da pauta “não estão esperando NINGUÉM” é mais ampla que seu predicado efetivo. (`app/folha/fora_da_folha.py:71–89`; `app/ponto/services/pautas_dos_discordantes.py:46–56,79–84`; S:559–565)

## 13/11 17:32 · sessao be11735563d0 · BUG-PAR · verificador · origem=Code

Reprovo a volta pelo **encerramento indevido após o novo rollback do par**: a irmã é desfeita, mas a pergunta pode receber `fato_ocupado` por essa mesma batida desfeita. [app/chamados/services/materializacao.py:1233](/home/ronald/codex_ws/b7f6efa8b632ac52fa87e5b325ba6d4031dc57b6/app/chamados/services/materializacao.py:1233), `app/chamados/services/materializacao.py:1639`, `app/chamados/services/materializacao.py:1399`.

Suite completa, execução dos mutantes e incidência dos contraexemplos na produção: **[nao verificado]**. [nao verificado]

**1. BLOQUEANTE — o rollback apaga a irmã, mas a colisão com ela encerra a pergunta.**

O novo `_Reprovada` desfaz o plantio da irmã quando a outra metade encontra conflito. Entretanto, conserva `colisao tipo-oposto` na mensagem; fora do savepoint, o `finally` entrega essa mensagem a `_carimbar_recusa`. O catálogo responde `COLISAO`, e a porta grava `VIA_FATO_OCUPADO`, sem exigir uma batida sobrevivente. `app/chamados/services/materializacao.py:1228`, `app/chamados/services/materializacao.py:1625`, `app/chamados/services/materializacao.py:1639`, `app/chamados/catalogo/recusa.py:74`, `app/chamados/services/materializacao.py:1393`, `app/chamados/models.py:3194`.

Contraexemplo concreto:

- Template 19h–07h, intervalo previsto 01h/02h; chão E18h30/S06h30; chamado com marcos faltantes **S01h30/E01h32**; duas respostas já validadas. O plantio usa as horas do chamado, preservadas pelo ajuste noturno; a irmã validada dispensa nova avaliação de implausibilidade. `app/chamados/services/materializacao.py:195`, `app/chamados/services/materializacao.py:645`, `app/chamados/juizes.py:322`.
- A projeção aceita S01h30/E01h32; E01h32 grava primeiro. S01h30 encontra essa E dentro da janela de colisão e fora da tolerância do seu marco próprio 01h00. `app/chamados/services/materializacao.py:219`, `app/chamados/services/materializacao.py:227`, `app/chamados/services/materializacao.py:1433`, `app/ponto/registro_batida.py:21`, `app/ponto/registro_batida.py:35`, `app/ponto/registro_batida.py:145`.
- E01h32 volta no rollback, mas S01h30 fica carimbada como `fato_ocupado`. O RED2b simula **competência fechada**, cuja tradução não produz esse carimbo; portanto não exercita o defeito. `app/chamados/services/materializacao.py:1233`, `app/chamados/services/materializacao.py:1399`, `app/chamados/tests/test_bug_par_projetado.py:672`.

A contraprova em memória, usando a expressão real do novo erro e o corpo real de `_carimbar_recusa`, produziu `COLISAO / fato_ocupado / CARIMBADA`; execução integral com ORM: **[nao verificado]**. `app/chamados/services/materializacao.py:1233`, `app/chamados/services/materializacao.py:1383`.

**2. BLOQUEANTE residual, preexistente — batida retratada ainda decide a paridade.**

`_vizinha_mesmo_tipo` consulta batidas sem excluir `retratada_em`, embora o modelo declare a retratada fora da geometria e apuração. A projeção continua usando essa vizinhança. `app/chamados/services/materializacao.py:108`, `app/chamados/services/materializacao.py:112`, `app/chamados/services/materializacao.py:228`, `app/ponto/models.py:64`.

Contraexemplo: chão vivo E18h30/S00h30/S06h30, **E00h45 retratada**, par pretendido S01h/E02h. A E retratada esconde a S00h30 da guarda; o par pode passar apesar da sequência viva resultante conter S00h30–S01h. No ensaio em memória do corpo real, com a retratada a guarda retornou `None`; retirando-a, retornou a S00h30 ofensora. Plantio integral desse cenário: **[nao verificado]**. `app/chamados/services/materializacao.py:108`, `app/chamados/services/materializacao.py:117`, `app/chamados/services/materializacao.py:121`.

Isso não é atributo faltando na namedtuple: é o universo consultado pela guarda, preservado no delta fornecido. `app/chamados/services/materializacao.py:90`, `app/chamados/services/materializacao.py:108`.

**3. NAO-BLOQ — situação dos sete itens procedentes da triagem.**

| Item | Cura presente no código | Limite encontrado |
|---|---|---|
| **1** | Fora de intervalo e ausência de resposta retornam impedimentos textuais; `_gravar_irma_do_par` os repassa. `app/chamados/services/materializacao.py:186`, `app/chamados/services/materializacao.py:1425` | Não identifiquei contraexemplo restante para o falso “a irmã gravou” nessa saída. Os REDs verificam retorno e recusa. `app/chamados/tests/test_bug_par_projetado.py:589`, `app/chamados/tests/test_bug_par_projetado.py:598` |
| **2** | A testemunha consulta DATA_EM_DISPUTA e a autoridade `competencia_fechada`, inclusive para ambos os instantes do par. `app/chamados/services/materializacao.py:700`, `app/chamados/services/materializacao.py:667`, `app/chamados/services/materializacao.py:718`, `app/chamados/services/materializacao.py:729` | A cura dessas duas guardas está presente; o novo tratamento de conflito contém o bloqueante 1. `app/chamados/services/materializacao.py:1228` |
| **4** | Laço usa `atomic`, helper compartilhado de trava e releitura da pergunta. Lote chama o mesmo helper. `app/chamados/services/materializacao.py:1743`, `app/chamados/services/passivo_validado.py:379` | Corrida ou inversão remanescente causada por essa extração: **[nao verificado]**. |
| **5** | Os dois ramos do modal mostram `aplicavel`; bloqueios de data/competência também aparecem. `app/templates/chamados/partials/modal_fio.html:230`, `app/templates/chamados/partials/modal_fio.html:238`, `app/templates/chamados/partials/modal_fio.html:252` | O teste do ramo validado admite o mutante descrito abaixo. `app/chamados/tests/test_bug_par_projetado.py:731` |
| **6** | Foto inclui `materializada_por_id`; `evento() is None` levanta dentro do `atomic`. `app/chamados/services/validacao.py:264`, `app/chamados/services/validacao.py:285`, `app/chamados/services/validacao.py:288` | O RED confere rollback e ausência de DEVOLUCAO. `app/chamados/tests/test_bug_par_projetado.py:776` |
| **7** | Nome contém empresas/UUID e abertura exclusiva `'x'`. `app/chamados/management/commands/bug_par_103.py:74`, `app/chamados/management/commands/bug_par_103.py:77` | Código curado; abertura exclusiva sem mordida específica. `app/chamados/tests/test_bug_par_projetado.py:794` |
| **9** | Selo exige as quatro tabelas em ordem, incluindo Pergunta, antes da escrita. `app/chamados/tests/test_bug_par_projetado.py:556`, `app/chamados/tests/test_bug_par_projetado.py:710` | Confere tabela/ordem, sem conferir os PKs das duas metades. `app/chamados/tests/test_bug_par_projetado.py:560` |

**4. NAO-BLOQ — dois mutantes óbvios escapam dos REDs novos.**

- **RED5:** apagar somente o bloco `aplicavel` da pergunta validada. O teste valida apenas a saída; a volta continua imprimindo a mesma frase pelo ramo não validado. A busca no HTML inteiro continua satisfeita. `app/chamados/tests/test_bug_par_projetado.py:734`, `app/chamados/tests/test_bug_par_projetado.py:737`, `app/templates/chamados/partials/modal_fio.html:238`, `app/templates/chamados/partials/modal_fio.html:252`.
- **RED7:** trocar `'x'` por `'w'`. Dois UUIDs distintos continuam criando dois arquivos; o teste não provoca colisão de nome. `app/chamados/management/commands/bug_par_103.py:76`, `app/chamados/management/commands/bug_par_103.py:77`, `app/chamados/tests/test_bug_par_projetado.py:803`.

Essas fugas são inferências das fixtures e asserções; execução dos mutantes: **[nao verificado]**. Os REDs dos itens 1, 2, 4/9 e 6 têm asserções diretamente contrárias à retirada das respectivas curas. `app/chamados/tests/test_bug_par_projetado.py:595`, `app/chamados/tests/test_bug_par_projetado.py:627`, `app/chamados/tests/test_bug_par_projetado.py:642`, `app/chamados/tests/test_bug_par_projetado.py:565`, `app/chamados/tests/test_bug_par_projetado.py:779`.

**5. NAO-BLOQ — namedtuple compatível com os leitores encontrados; brancos não retiram respostas do contador final.**

A tupla oferece `pk`, `colaborador`, `timestamp` e `tipo`. Os consumidores encontrados usam `tipo/timestamp` e, nas mensagens, `pk`; não exigem `.colaborador_id`, `.retratada_em` nem métodos de `Batida`. A hora desses consumidores passa por `hora_local`, que converte datetimes aware para o fuso local. `app/chamados/services/materializacao.py:90`, `app/chamados/services/materializacao.py:230`, `app/chamados/services/materializacao.py:1164`, `app/chamados/services/materializacao.py:1174`, `app/core/fmt.py:10`.

O SQL deixa de carregar respostas só de brancos. Contudo, no delta fornecido, `_recusa_e_de_paridade` já exigia `.strip()`; o contador continua aplicando esse predicado antes de contar, e a elegibilidade103 também depende dele. Portanto a exclusão SQL não demonstra mudança do **universo final** das 163. `app/chamados/juizes.py:1520`, `app/chamados/juizes.py:1551`, `app/chamados/services/materializacao.py:261`, `app/chamados/services/materializacao.py:278`, `app/chamados/services/materializacao.py:323`.

A mudança efetiva está em `_situacao_da_irma`: irmã com carimbo de resposta e texto só de brancos passa a `sem_carimbo_nao_respondida`. Isso muda `irma/por_irma`, sem alterar por si o total. `app/chamados/services/materializacao.py:299`, `app/chamados/services/materializacao.py:329`, `app/chamados/services/materializacao.py:337`.

Consumidores: comando lê contador/seleção103; porta reconsulta elegibilidade sob trava; modal lê a testemunha; rótulo103 lê DEVOLUCAO; tarefa do dia usa DEVOLUCAO para devolver a ação ao admin. Contagem efetiva de 163 nesta produção: **[nao verificado]**. `app/chamados/management/commands/bug_par_103.py:44`, `app/chamados/services/validacao.py:259`, `app/chamados/services/fio.py:373`, `app/chamados/services/materializacao.py:285`, `app/chamados/services/tarefa_do_dia.py:141`.

**6. NAO-BLOQ — registro operacional impreciso; linha LEI-AKITA do commit indisponível.** [nao verificado]

`FORA_DE_PIPELINE` mistura o contador163 com o universo operado: o comando chama a devolução somente para `c['bug_par_103']`. Contraexemplo: uma das 60 já sem carimbo integra o contador, mas não recebe essa devolução. `app/config/crons.py:773`, `app/chamados/management/commands/bug_par_103.py:45`, `app/chamados/services/materializacao.py:343`.

Flags reais: `--empresa` repetível, `--autor` STAFF obrigatório no apply, `--apply`; default DRY; reversão gravada antes das chamadas à porta. “Só com aval” é instrução operacional, sem flag de conferência do aval. `app/chamados/management/commands/bug_par_103.py:26`, `app/chamados/management/commands/bug_par_103.py:65`, `app/chamados/management/commands/bug_par_103.py:77`, `app/chamados/management/commands/bug_par_103.py:95`, `app/config/crons.py:774`.

A cópia não disponibilizou histórico Git; conferir a **linha LEI-AKITA do commit b7f6efa8b** ficou **[nao verificado]**. Pelo delta fornecido, `_competencia_que_barra` apenas agrega consultas à autoridade existente; não inventa critério. “Juízes novos=0” é compatível com essa alteração. `app/chamados/services/materializacao.py:667`, `app/ponto/janelas.py:75`.

A busca textual em `CORTES.md` não encontrou nascimento nominal para `_competencia_que_barra`, `par_do_intervalo`, `paridade_absolvida_bug_par_103`, `metade_irma_do_intervalo`, `_data_em_disputa`, `estado_aplicacao_pergunta`, `texto_vazio` ou `q_respondida_com_texto`. Não identifiquei autoridade nova introduzida **neste delta** que exija outra frase; conferência por `git grep`/histórico: **[nao verificado]**. A exigência de corte está em `CLAUDE.md:74`.

**7. DINHEIRO — NAO-BLOQ do delta, mas permanece o caminho preexistente que reescreve exportada.**

Validar agenda recálculo **antes** de materializar; a recusa posterior por competência fechada não desfaz automaticamente esse agendamento. O callback recalcula o fechamento da competência do fato, e o escritor atualiza inclusive os campos calculados do fechamento não aberto. Contraexemplo: validar resposta da comp10 exportada, receber recusa de plantio e ainda executar recálculo de comp10. É o item 3 já classificado FORA na triagem fornecida. `app/chamados/services/validacao.py:544`, `app/chamados/services/validacao.py:555`, `app/chamados/services/validacao.py:570`, `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1160`.

Para comp11 aberta, o caminho do par pode criar batidas e disparar recálculo; para comp10 exportada, a criação administrativa direta continua barrada por `competencia_fechada`. Mudança efetiva de **entra/fora, rubrica, valor ou batida**, por competência, no dado de produção fornecido: **[nao verificado]**. `app/chamados/services/materializacao.py:1220`, `app/ponto/registro_batida.py:132`, `app/ponto/registro_batida.py:157`.

O `--apply`103 diretamente descarimba a pergunta e escreve decisão/auditoria. Entretanto, seu `save` também acorda beco e outbox; por isso não há base para prometer ausência absoluta de efeitos indiretos sobre o gravado. `app/chamados/services/validacao.py:268`, `app/chamados/services/validacao.py:273`, `app/chamados/services/validacao.py:276`, `app/chamados/signals.py:229`, `app/chamados/signals.py:252`.

Contraexemplo condicional: elegível103 em disputa aberta, pai resolvido, dia trancado e nenhuma outra pendência; o beco fecha a disputa, cujo sinal retoma regularização externa confirmada. Essa regularização chama `registrar_batida` sem `fato_de_chao=False`; o default deriva `True`, podendo atravessar a guarda de competência e recalcular. Existência desse conjunto na frota: **[nao verificado]**. `app/chamados/models.py:1847`, `app/chamados/juizes.py:1405`, `app/chamados/services/becos.py:33`, `app/chamados/services/becos.py:57`, `app/chamados/signals.py:139`, `app/chamados/services/regularizacao_ext.py:225`, `app/chamados/services/regularizacao_ext.py:78`, `app/ponto/registro_batida.py:130`.

**8. NAO-BLOQ — arquivo de reversão pode conter uma foto anterior à efetivamente removida.**

O comando fotografa sem trava; a porta trava e relê depois. Contraexemplo: elegível103 com `validada_em=None`; depois da foto, outro admin valida; a devolução remove essa validação, ausente no JSON. A auditoria registra o antes correto, mas o arquivo de reversão isolado fica desatualizado. Ocorrência real: **[nao verificado]**. `app/chamados/management/commands/bug_par_103.py:48`, `app/chamados/management/commands/bug_par_103.py:77`, `app/chamados/services/validacao.py:259`, `app/chamados/services/validacao.py:263`, `app/chamados/services/validacao.py:271`, `app/chamados/services/validacao.py:544`.

**9. NAO-BLOQ — mapa dos campos e significados sobrepostos.**

| Campo | Quem escreve | Quem lê no caminho examinado |
|---|---|---|
| `resposta_colab` | Resposta humana e automática; reabertura limpa. `app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1595` | Derivação, elegibilidade103 e classificação da recusa. `app/chamados/services/materializacao.py:595`, `app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:261` |
| `respondida_em` | Resposta humana/automática; reconstrução pela trilha; carimbo com texto; reabertura limpa. `app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1660`, `app/chamados/services/disputa_emissao.py:1681`, `app/chamados/models.py:3156`, `app/chamados/services/disputa_emissao.py:1595` | Validação, C6, fila e contador. `app/chamados/services/validacao.py:495`, `app/chamados/juizes.py:1400`, `app/chamados/services/materializacao.py:1733`, `app/chamados/juizes.py:1556` |
| `materializada_em` | Setter do carimbo, chamado por carimbar/identidade/descarimbar. `app/chamados/models.py:3123`, `app/chamados/models.py:3155`, `app/chamados/models.py:3211`, `app/chamados/models.py:3234` | Tela, contador, fila e C6. `app/chamados/services/materializacao.py:688`, `app/chamados/services/materializacao.py:325`, `app/chamados/services/materializacao.py:1735`, `app/chamados/juizes.py:1444` |
| `via_resolucao` | Mesmo setter; relavra também altera a via. `app/chamados/models.py:3127`, `app/chamados/services/relavra_via.py:125` | Elegibilidade103, tela e C6. `app/chamados/services/materializacao.py:276`, `app/chamados/services/materializacao.py:692`, `app/chamados/juizes.py:1438` |
| `recusa_motivo` | Escape, materializador, recusa humana e duplicata. `app/chamados/services/disputa_emissao.py:1497`, `app/chamados/services/materializacao.py:1402`, `app/chamados/services/validacao.py:705`, `app/chamados/services/fio.py:1399` | Catálogo de paridade, seleção e foto103. `app/chamados/services/materializacao.py:262`, `app/chamados/services/materializacao.py:251`, `app/chamados/services/validacao.py:266` |
| DEVOLUCAO | Porta103 chama a porta única da sequência. `app/chamados/services/validacao.py:276`, `app/chamados/models.py:4029` | Rótulo103 e tarefa do admin. Histórico de respostas não a inclui. `app/chamados/services/materializacao.py:285`, `app/chamados/services/tarefa_do_dia.py:141`, `app/chamados/models.py:3599`, `app/chamados/models.py:3955` |

`materializada_em` significa também **encerramento sem plantio**; `respondida_em` não prova fala humana; `recusa_motivo` reúne impedimento técnico, escape, rejeição humana e duplicata. Esses significados já coexistem e exigem leitura pelas autoridades próprias. `app/chamados/services/materializacao.py:689`, `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1497`, `app/chamados/services/validacao.py:705`, `app/chamados/services/fio.py:1399`.

Não encontrei segundo escritor de Batida introduzido pela tupla: o plantio continua em `registrar_batida`, e a allowlist desse gate permanece vazia. A ampliação do selo da validação nomeia a função exata da devolução, sem liberar o módulo inteiro. `app/chamados/services/materializacao.py:90`, `app/chamados/services/materializacao.py:1219`, `app/ponto/tests/test_chokepoint_batida_gate.py:21`, `app/chamados/tests/test_porta_validacao_pergunta.py:82`.

## 13/11 17:31 · sessao 46c38e4be95e · BO-[nome]-1311-ROTEIRO · sonda · origem=Code

**Saída integral da sonda: não disponível.** O conteúdo recebido termina dentro da FICHA 05, sem o aval literal, sem `sonda_roteiro.py` e sem os casos 06–10. Não executei outra sonda. A execução e a raspagem pertencem ao envelope anterior ao Codex; o resultado chega pelo texto do pedido. (`bin/codex_pergunta.sh:15`, `bin/codex_pergunta.sh:325`; anexos ausentes [nao verificado])

As três mensagens de erro presentes no fragmento recebido são estas, reproduzidas integralmente; não constituem a saída inteira da sonda. [nao verificado]

```text
2026-11-13 17:24:34,808 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=46757 resposta=16:01 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 17:24:34,921 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=55375 resposta=14:30 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 17:24:34,933 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=55411 resposta=15:50 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
```

**O roteiro abaixo é parcial.** As linhas baseadas no texto raspado recebem `[nao verificado]` porque não existe arquivo local correspondente. A correspondência entre `item` e posição no UNIVERSO do aval não pode ser conferida sem o aval; nos casos 01–04, FICHA e EXTRA concordam entre si quanto a `item` e `colab`. [nao verificado]

O juiz da retenção é `motivos_retencao_celula` → `_dias_sem_decisao`: célula acusadora retém, salvo as exceções dos protestos não cobráveis e da pausa. A exceção da pausa lê `periodos_do_dia`, exige ausência de desalinhamento, período aberto e dúvida, e exige que todos os papéis apagados sejam pausa. (`app/folha/export.py:1212`, `app/folha/export.py:1273`, `app/folha/export.py:1293`, `app/folha/export.py:1148`, `app/folha/export.py:1173`)

`estado_aplicacao=aplicavel` não mede esse resultado posterior: a escrita ainda pode sofrer rollback pelo juiz de lastro e pelo carimbo C6. Portanto, nenhuma pergunta abaixo recebe **(a) VALIDA-HOJE** apenas por apresentar `aplicavel`. (`app/chamados/services/materializacao.py:449`, `app/chamados/services/materializacao.py:1238`, `app/chamados/services/materializacao.py:1283`)

## Caso 01 -- col1019

A sonda fornecida mostra **zero perguntas vivas e zero dias retidos na comp 11**. As três perguntas históricas estão sem declaração, com `estado_aplicacao=null`, materializadas por `celula_absolveu`, na disputa 7877 fechada. [nao verificado]

Há seis abonos aprovados, em 03, 05, 07, 09, 10 e 12/11; não há batidas listadas. O chamado 37062 permanece aberto em cadastro, subtipo `nunca_bateu`. Não há dia retido a liberar neste caso. Print e comportamento observado na tela: [nao verificado]

## Caso 02 -- col996

**Não conclui os itens afetados pelos três ERRORs:** perguntas 46757, 55375 e 55411. As classes abaixo indicam encaminhamento; não certificam sucesso de validação. [nao verificado]

Todas as 16 perguntas vivas abaixo pertencem à **disputa 7263 aberta**. Tipo de resposta: `hora`. Os tipos E/S vêm do motivo e da projeção informada, conforme identificado na tabela. [nao verificado]

| Pergunta | Dia | Declarado | `estado_aplicacao` fornecido | Classe única |
|---|---|---|---|---|
| 46751 | 20/10 | entrada 07:50 | `bloqueada_paridade`: existe E 07:00 | **(c) DECISAO DO ADMIN** [nao verificado] |
| 46756 | 21/10 | intervalo saída 15:00 | `aplicavel`: E 15:00 | **(b) ESPERA OBRA DECLARADO-X-GRAVADO** [nao verificado] |
| 46757 | 21/10 | intervalo volta 16:01 | `aplicavel`: E 14:30; ERROR | **(b) ESPERA OBRA DECLARADO-X-GRAVADO** [nao verificado] |
| 46758 | 21/10 | saída 21:05 | `aplicavel`: S 21:05 | **nao medido**: retirada da retenção após o ato [nao verificado] |
| 48773 | 22/10 | intervalo saída `null` | `null` | **(d) CADASTRO**: colab informa a saída efetiva da pausa [nao verificado] |
| 57211 | 22/10 | intervalo volta `null` | `null` | **(d) CADASTRO**: colab informa a volta efetiva [nao verificado] |
| 52035 | 30/10 | saída 15:50 | `aplicavel`: S 15:50 | **nao medido**: retirada da retenção após o ato [nao verificado] |
| 53565 | 03/11 | intervalo volta `null` | `null` | **(d) CADASTRO**: colab informa a volta efetiva [nao verificado] |
| 53644 | 03/11 | saída `null` | `null` | **(d) CADASTRO**: colab informa a saída efetiva [nao verificado] |
| 53920 | 04/11 | intervalo saída 13:01 | `ja_satisfeita`: S 12:54; espelho não muda | **(c) DECISAO DO ADMIN** [nao verificado] |
| 55313 | 08/11 | entrada 07:00 | `ja_satisfeita`: E 06:58; espelho não muda | **(c) DECISAO DO ADMIN** [nao verificado] |
| 55375 | 08/11 | intervalo saída 14:30 | `aplicavel`: S 13:00; ERROR | **(b) ESPERA OBRA DECLARADO-X-GRAVADO** [nao verificado] |
| 55411 | 08/11 | intervalo volta 15:50 | `aplicavel`: E 14:30; ERROR | **(b) ESPERA OBRA DECLARADO-X-GRAVADO** [nao verificado] |
| 56923 | 11/11 | intervalo volta `null` | `null` | **(d) CADASTRO**: colab informa a volta efetiva [nao verificado] |
| 57210 | 11/11 | intervalo saída 13:00 | `aplicavel`: S 13:00 | **nao medido**: resultado posterior; dia já não retém [nao verificado] |
| 57214 | 12/11 | saída `null` | `null` | **(d) CADASTRO**: colab informa a saída efetiva [nao verificado] |

A obra **DECLARADO-X-GRAVADO** existe em `app/docs/BACKLOG.md:1936`; o passivo da comp 11 tem **DECLARADO-X-GRAVADO-PASSIVO-11**, em `app/docs/BACKLOG.md:1937`. O código substitui a hora respondida de intervalo pela hora do marco faltante; o tipo prioriza o marco do chamado e registra divergência com a ata. (`app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:302`, `app/chamados/services/materializacao.py:409`)

Nas decisões administrativas, é necessário decidir qual horário deve prevalecer: a batida existente ou uma correção fundamentada. O modal do fio oferece **Rejeitar resposta**, com motivo, e **Corrigir**, com hora e motivo; a correção chama novamente a validação e pode ser recusada. Isso não certifica a liberação do dia. (`app/templates/chamados/partials/modal_fio.html:292`, `app/templates/chamados/partials/modal_fio.html:303`, `app/chamados/services/acoes_disputa.py:334`, `app/chamados/services/acoes_disputa.py:379`)

Os únicos dias retidos na comp 11 são estes; perguntas vivas nos demais dias não acrescentam retenção ao universo. [nao verificado]

| Dia retido | Motivo | O que falta para encaminhar a saída |
|---|---|---|
| 21/10 | `espelho_furo` | Obra para os intervalos divergentes; saída declarada disponível. Liberação posterior não medida; pergunta 46757 com ERROR. [nao verificado] |
| 30/10 | `espelho_furo` | Saída declarada disponível; falta medir a transação completa e o juiz de retenção depois dela. **nao medido**. [nao verificado] |
| 03/11 | `espelho_furo` | Colab informar volta da pausa e saída; depois, decisão/materialização e nova leitura da retenção. **CADASTRO**. [nao verificado] |
| 12/11 | `espelho_furo` | Colab informar a saída; depois, decisão/materialização e nova leitura da retenção. **CADASTRO**. [nao verificado] |

## Caso 03 -- col1032

A sonda fornecida mostra **zero perguntas vivas e zero dias retidos na comp 11**. As nove perguntas históricas têm declarado e `estado_aplicacao` nulos, foram materializadas por `celula_absolveu` e pertencem a disputas fechadas. [nao verificado]

As quatro batidas de 11/11 estão retratadas e não aprovadas; há abono aprovado nesse dia. As quatro batidas de 12/11 estão aprovadas, e a célula está `concorde/ok`. Não há dia retido a liberar. Print e comportamento observado na tela: [nao verificado]

## Caso 04 -- col246

| Pergunta viva | Dia | Tipo/declarado | `estado_aplicacao` | Disputa | Classe única |
|---|---|---|---|---|---|
| 52485 | 01/11 | escolha / `null` | `null` | 7755 **fechada** | **(b) ESPERA OBRA FIO-E0-V4-E3-LEITORES** [nao verificado] |
| 52715 | 01/11 | hora, saída / `null` | `null` | 7755 **fechada** | **(b) ESPERA OBRA FIO-E0-V4-E3-LEITORES** [nao verificado] |

O único dia retido é **01/11**, célula 136215, por `espelho_cobranca_indevida`. O chamado de turno ainda vivo mostrado nesse dia é **36424**, `turno_aberto/em_analise`. A sonda não fornece `chamado_destino`, ata ou protestos: **não prova a ligação exata de uma dessas perguntas ao chamado 36424**, nem qual protesto produziu o veredito. [nao verificado]

O cartório carrega chamados de `FURO_MODULES` por dia; `COBRANCA_SEM_FURO` exige trabalho, realizado suficiente, todas as células casadas e chamado vivo. Esse protesto mapeia para `cobranca_indevida`. (`app/ponto/services/cartorio.py:127`, `app/ponto/services/cartorio.py:135`, `app/ponto/supra_juiz.py:467`, `app/escala/models.py:1613`)

**Por que `celula_absolveu` não encerrou:** `cobranca_indevida` é veredito acusador, e a seleção de perguntas absolvíveis exclui células acusadoras. O sítio que efetivamente carimba a absolvição é `retratar_perguntas_absolvidas_por_celula`, em `p.carimbar(VIA_CELULA_ABSOLVEU)`. O fechamento do chamado também recusa célula acusadora; o ramo separado de órfãs exige disputa aberta. (`app/escala/models.py:1767`, `app/chamados/reconciliador.py:642`, `app/chamados/reconciliador.py:596`, `app/chamados/reconciliador.py:1028`, `app/chamados/reconciliador.py:1050`)

Encaminhamento: complementar **E3-LEITORES**, existente e aberta, com esse fio inconsistente e a medição dos vínculos ausentes. **E4B existe, mas o BACKLOG a registra como pousada**; não cabe apresentá-la como obra aberta. O ato exato que retirará 01/11 da retenção permanece [nao verificado]. (`app/docs/BACKLOG.md:1899`, `app/docs/BACKLOG.md:1921`)

## Caso 05 -- col265

A FICHA termina dentro da lista de células, antes das perguntas, disputas e do EXTRA. Quantidade de perguntas vivas, declarações, estados, disputas e dias retidos: **[nao verificado]**.

O complemento solicitado cabe em **RAIZ-DA-DISPUTA**, existente: trata a relação raiz–filhos, escritores externos ao agregado e disputa que não reage. Não é possível atribuir perguntas ou colab-dias deste caso à obra sem a continuação da sonda. (`app/docs/BACKLOG.md:1931`; aplicação individual [nao verificado])

## Caso 06 -- colNNN [nao verificado]

Identidade, três dias recusados, quatro perguntas vivas e vínculos: **não recebidos**. Não há classificação individual nem contagem deste caso. [nao verificado]

Nesta cópia, `leitor_celula.py::grade_da_celula` é leitor; a recusa encontrada está em `ponto/portas/celula.py`, que pula datas contidas nas janelas fechadas enumeradas. (`app/escala/services/leitor_celula.py:350`, `app/ponto/portas/celula.py:221`, `app/ponto/portas/celula.py:284`)

**Não está provado bug de mês civil nesse sítio.** As janelas vêm de `janela_competencia`, baseada em `Empresa.dia_inicio_competencia`; com corte 21, a comp 10 é 21/09–20/10. Sem as três datas e o cadastro da empresa do caso, seu enquadramento individual permanece [nao verificado]. (`app/ponto/management/commands/gerar_celulas.py:185`, `app/folha/export.py:47`, `app/ponto/janelas.py:50`, `app/colaboradores/models.py:46`)

Há reatividade implementada para rejulgar células tocadas e agendar reconciliação no commit. Existem as obras relacionadas **CONTRATO-20-DOIS-EVENTOS**, **CONTRATO-20-E2-QUAL-ATO** e **E2-D3-TRABALHA-FORA-DA-IMPRESSAO**; escolher qual cobre as quatro perguntas exige os dados ausentes. (`app/ponto/portas/celula.py:460`, `app/ponto/portas/celula.py:528`, `app/docs/BACKLOG.md:1711`, `app/docs/BACKLOG.md:1071`, `app/docs/BACKLOG.md:988`)

## Caso 07 -- colNNN [nao verificado]

A tela efetivamente utilizada e os dados individuais não foram recebidos. [nao verificado]

O código demonstra a diferença: o **detalhe do chamado** exige disputa aberta e pode devolver “Pergunta invalida.”; o **fio do colaborador**, também usado pelo modal, permite validar pergunta viva em disputa fechada com `sem_reabrir=True`. (`app/chamados/views.py:493`, `app/chamados/views.py:500`, `app/chamados/views.py:550`, `app/chamados/services/fio.py:595`, `app/chamados/services/fio.py:600`, `app/chamados/views.py:889`)

O BACKLOG registra **HJ-093**, col1004, pergunta 50873 e célula 143004 de 27/10: distingue a decisão humana sobre a pergunta da obra **JUSTIFICATIVA-PAGA-O-PERIODO**. Isso é registro documental; confirmar que corresponde à posição 07 do aval e ao estado da sonda permanece [nao verificado]. (`app/docs/BACKLOG.md:742`, `app/docs/BACKLOG.md:1872`)

## Caso 08 -- colNNN [nao verificado]

Identidade, perguntas, disputas e retenção não foram recebidas. [nao verificado]

**CONTRATO-DE-ENTRADA-DO-FIO** já existe. O desencontro é comprovado no código: o emissor retroativo entrega `foto_ausente.batida_id`, enquanto o nascimento procura `ultima_batida.id`; sem episódio, a geração deduplica por disputa/motivo. Cabe complementar essa obra, sem novo id. (`app/docs/BACKLOG.md:2100`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`, `app/chamados/models.py:3464`, `app/chamados/services/disputa_emissao.py:325`)

## Caso 09 -- colNNN [nao verificado]

Identidade, perguntas, tipos declarados e dias retidos não foram recebidos. [nao verificado]

A família existente inclui **TIPO-NA-FONTE** e **TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO**, esta em fila 2. (`app/docs/BACKLOG.md:1833`, `app/docs/BACKLOG.md:741`)

**O ocultamento dos horários de dia só com saídas é comprovado no template:** a view `calendario_ponto` monta o contexto e renderiza o calendário; o bloco de horários exige `dia.entradas or dia.retratadas`, e só dentro dele percorre `dia.batidas`. Assim, somente S, sem retratadas, não imprime os horários. Cabe complemento de UI na fila 2; associação aos dias deste caso [nao verificado]. (`app/colaboradores/views.py:1938`, `app/colaboradores/views.py:1949`, `app/colaboradores/views.py:1965`, `app/templates/colaboradores/partials/_calendario_grade.html:118`, `app/templates/colaboradores/partials/_calendario_grade.html:121`)

## Caso 10 -- colNNN [nao verificado]

Identidade, intervalos, perguntas, disputas e retenção não foram recebidos. O destino documental pedido é **DECLARADO-X-GRAVADO**, com passivo da comp 11 em **DECLARADO-X-GRAVADO-PASSIVO-11**; aplicação individual e classificação permanecem [nao verificado]. (`app/docs/BACKLOG.md:1936`, `app/docs/BACKLOG.md:1937`)

## Contagem parcial por classe

Universo efetivamente disponível para esta contagem: **casos 01–04**, com 18 perguntas vivas e cinco colab-dias retidos. Perguntas históricas encerradas não entram na contagem de vivas. [nao verificado]

| Classe | Perguntas vivas | Colab-dias retidos |
|---|---:|---:|
| (a) VALIDA-HOJE | 0 | 0 |
| (b) ESPERA OBRA | 6 | 2 |
| (c) DECISAO DO ADMIN | 3 | 0 |
| (d) CADASTRO | 6 | 2 |
| nao medido | 3 | 1 |
| **Total parcial** | **18** | **5** |

Conferência parcial: perguntas **0 + 16 + 0 + 2 = 18**; dias retidos **0 + 4 + 0 + 1 = 5**. Os encaminhamentos de obra não representam itens concluídos, especialmente nas três perguntas com ERROR. [nao verificado]

**A soma do universo dos dez casos não pode ser conferida:** faltam o restante do caso 05, os casos 06–10 e o aval literal. Não há base para completar esses números ou declarar o roteiro integral concluído. [nao verificado]

_4715 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
