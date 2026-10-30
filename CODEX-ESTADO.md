# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 18:32** · publicado em **30/10/2026 18:33** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 18:32 · sessao 5d1a28e5c21b · O261-V-FASE · verificador · origem=Code

**Parecer: reprovo esta construção.** A abertura para vários ECs da mesma pessoa não foi acompanhada pela identidade da proposta no aceite: o fio escolhe a primeira proposta, e a assinatura não inclui `vinculo_id`. Isso permite aceitar outro vínculo sob a assinatura apresentada anteriormente. `app/escala/services/propositor.py:452`, `app/escala/services/propositor.py:661`, `app/chamados/services/proposta_no_fio.py:106`.

A análise abaixo distingue falhas da costura nova de problemas herdados. A suíte Django, os mutantes e o RED histórico permanecem **[não verificado]**. A colisão de assinatura e sua aceitação por `_recalcular` foram reproduzidas isoladamente em memória, usando as funções reais e substituindo apenas a fonte de propostas; integração com banco **[não verificado]**. `app/escala/services/propositor.py:654`, `app/chamados/services/proposta_no_fio.py:92`.

**1. Quem escreve e quem lê**

O comando calcula o detalhe; `fase_12x36.lavrar()` persiste a fotografia; `ultimo_lavrado()` lê o detalhe e acrescenta os metadados do snapshot. O cron existente já chama `censo_fase_12x36 --min-plantoes 6 --lavrar`. `app/ponto/management/commands/censo_fase_12x36.py:239`, `app/escala/services/fase_12x36.py:78`, `app/escala/services/fase_12x36.py:90`, `app/config/crons.py:614`.

| Campo/valor | Escritor em produção | Leitor em produção |
|---|---|---|
| Chave `desvio_de_cadastro_12x36`, escopo `tenant/0`, `data_ref`, `data_ini` | `lavrar()` faz `update_or_create`. `app/escala/services/fase_12x36.py:84` | `ultimo_lavrado()` filtra pela chave/escopo e escolhe o último snapshot. `app/escala/services/fase_12x36.py:96` |
| `valor` | `len(colabs_de_desvio)`. `app/escala/services/fase_12x36.py:86` | Não localizei leitor específico deste `valor`; os consumidores abaixo contam o detalhe. `app/escala/services/fase_12x36.py:102`, `app/api/views_mensageria.py:1675` |
| `detalhe.colabs_de_desvio` | IDs distintos das linhas em desvio. `app/ponto/management/commands/censo_fase_12x36.py:242` | Pauta, fila por causa, API e comando semanal. `app/escala/services/fase_12x36.py:117`, `app/chamados/services/fila_por_causa.py:174`, `app/api/views_mensageria.py:1675`, `app/ponto/management/commands/vigiar_familia_escala.py:27` |
| `detalhe.linhas` | Resumos por colaborador/competência: `colab`, `empresa`, `competencia`, `ec`, `template`, plantões, casados, paridade, furos e folga trabalhada. `app/ponto/management/commands/censo_fase_12x36.py:245` | API e impressão do comando semanal. `app/api/views_mensageria.py:1677`, `app/ponto/management/commands/vigiar_familia_escala.py:28` |
| `detalhe.itens` | Dias das linhas em desvio, acrescidos de colaborador, empresa, competência e dono. `app/ponto/management/commands/censo_fase_12x36.py:248` | Formatador C10 e gatilho do propositor. `app/escala/services/cadastro_realidade.py:339`, `app/escala/services/propositor.py:491` |
| `data`, `tipo`, `template_do_dia` | Produzidos a partir de furos/folgas trabalhadas e do vínculo escolhido para cada dia. `app/ponto/management/commands/censo_fase_12x36.py:197` | C10 conserva os campos pertinentes e monta o texto. `app/escala/services/cadastro_realidade.py:341` |
| `ec_do_dia` | Resultado de `vinculo_do_dia`, levado ao item diário. `app/ponto/management/commands/censo_fase_12x36.py:131`, `app/ponto/management/commands/censo_fase_12x36.py:197` | C10 conserva/exibe o EC; propositor identifica o par `(colab, ec)`. `app/escala/services/cadastro_realidade.py:341`, `app/escala/services/propositor.py:491`, `app/escala/services/propositor.py:513` |
| `dono_do_dia` | Literal `CADASTRO` na lavra. `app/ponto/management/commands/censo_fase_12x36.py:249` | C10, contagem por dono e índice diário usado pelo vigia do fio. `app/escala/services/cadastro_realidade.py:646`, `app/escala/services/cadastro_realidade.py:655`, `app/escala/services/cadastro_realidade.py:718`, `app/chamados/services/vigia_fio.py:266` |
| `aplicavel` | Comparação `dia >= piso_de_aplicacao(empresa, hoje)`. `app/ponto/management/commands/censo_fase_12x36.py:98`, `app/ponto/management/commands/censo_fase_12x36.py:199` | C10 repassa o booleano para `acao()`, que habilita o link. `app/escala/services/cadastro_realidade.py:343`, `app/escala/services/cadastro_realidade.py:647`, `app/escala/services/cadastro_realidade.py:494` |
| Demais contadores/metadados do detalhe | `linhas_de_desvio`, `linhas_medidas`, `automaticos`, `propostas`, `criterio`, `esperado`. `app/ponto/management/commands/censo_fase_12x36.py:240` | `linhas_medidas` e `criterio` chegam à API/comando; não localizei consumidor específico novo para os demais. `app/api/views_mensageria.py:1675`, `app/ponto/management/commands/vigiar_familia_escala.py:25` |

Para **`cadastro_realidade.lista()`**, `_fase_deslocada()` produz as assinaturas C10; `lista()` acrescenta descrição, dono, ocorrência, ação e URL, agrega por colaborador/posto e produz os totais por assinatura. `app/escala/services/cadastro_realidade.py:335`, `app/escala/services/cadastro_realidade.py:644`, `app/escala/services/cadastro_realidade.py:684`, `app/escala/services/cadastro_realidade.py:694`.

Os leitores são a tela e o PDF, ambos alimentados pela mesma chamada de `lista()`, além do vigia do fio, que consome `data` e `dono_do_dia` pelo índice `dias_na_lista()`. A coluna externa de posto/template continua sendo a do vínculo de hoje; o EC histórico aparece dentro do detalhe C10. `app/escala/views.py:490`, `app/templates/escala/cadastro_x_realidade.html:21`, `app/escala/services/cadastro_realidade.py:752`, `app/chamados/services/vigia_fio.py:259`, `app/escala/services/cadastro_realidade.py:514`, `app/escala/services/cadastro_realidade.py:345`.

Para **`propositor.propostas()`**, o escritor do retorno é o próprio propositor, sem persistir a proposta. Cada proposta recebe identidade do colaborador/EC, cadastro atual, evidência, motivo de entrada, `aplicar_desde` e chamados que morreriam; o envelope contém `fonte`, `n`, `propostas`, `universo`, `janela_dias` e os contadores/listas de ritmo e pausa. `app/escala/services/propositor.py:537`, `app/escala/services/propositor.py:573`, `app/escala/services/propositor.py:583`.

| Leitor de `propostas()` | Uso |
|---|---|
| Fio | Exibe a primeira proposta; recalcula antes de aceitar/recusar. `app/chamados/services/proposta_no_fio.py:63`, `app/chamados/services/proposta_no_fio.py:102` |
| Painel | Badge de existência para os colaboradores da página. `app/chamados/services/painel.py:520` |
| Ficha | Resume a primeira proposta e sua aplicabilidade. `app/colaboradores/services/ficha.py:176` |
| Chat | Recalcula para preview/confirmação. `app/core/views_ia.py:572` |
| API mensageria | Formata propostas para resposta. `app/api/views_mensageria.py:2019` |
| Pauta e comandos | Consomem envelope, propostas e evidências. `app/escala/services/propositor.py:638`, `app/escala/management/commands/propostas_de_escala.py:26`, `app/escala/management/commands/ritmo_discordante.py:18` |

A decisão, separadamente, é gravada por `recusar()`/`aplicar()` em `DecisaoProposta`. O aceite chama a porta existente de correção retroativa. `app/escala/services/propositor.py:690`, `app/escala/services/propositor.py:738`, `app/escala/services/propositor.py:746`.

**`piso_de_aplicacao`** calcula o início da competência corrente por `janela_atual`; no tratamento de exceção devolve `hoje`. Seus leitores são o censo, o filtro de EC encerrado e a montagem de `aplicar_desde`, através do wrapper `_piso_de_aplicacao`. `app/escala/services/propositor.py:595`, `app/escala/services/propositor.py:609`, `app/ponto/management/commands/censo_fase_12x36.py:98`, `app/escala/services/propositor.py:459`, `app/escala/services/propositor.py:558`, `app/escala/services/propositor.py:592`.

**2. Achados**

**A. P1 — Aceite e recusa não distinguem os ECs agora admitidos. Classe: identidade incompleta na porta; consumidor não migrado.** A assinatura contém somente tipo/template/âncora/início. O fio recalcula, pega `[0]` e compara essa assinatura; a recusa também cala por colaborador e assinatura. `app/escala/services/propositor.py:661`, `app/chamados/services/proposta_no_fio.py:106`, `app/escala/services/propositor.py:580`, `app/escala/services/propositor.py:674`.

Contraexemplo: hoje 16/11, corte 21; A é 12x36 de 19/10–01/11, B é 12x36 de 02–15/11, ambos 08–16; há batidas completas segunda–sexta nos dois trechos e template 5x2 08–16 disponível. Ambos podem propor o mesmo template, sem âncora, desde 21/10. Se outro admin corrige A entre preview e POST, sobra B com a mesma assinatura: `_recalcular` aceita B sob o aceite apresentado para A. Recusar A também pode calar B. O candidato por ritmo permite essa troca. `app/escala/services/propositor.py:161`, `app/escala/services/propositor.py:183`, `app/chamados/services/proposta_no_fio.py:107`. Caso completo com Django **[não verificado]**; colisão e aceite isolados reproduzidos nas funções citadas.

**B. P1 — A ação C10 não seleciona o EC indicado pela linha. Classe: perda da identidade da fonte na passagem ao consumidor.** A assinatura conserva `ec_do_dia`, mas a URL transporta somente o colaborador. O fio escolhe a primeira proposta, cuja ordenação prioriza chamados resolvidos. Havendo A e B elegíveis, clicar na linha de A pode apresentar B, mesmo com assinaturas diferentes. `app/escala/services/cadastro_realidade.py:341`, `app/escala/services/cadastro_realidade.py:495`, `app/chamados/services/proposta_no_fio.py:66`, `app/escala/services/propositor.py:582`.

**C. P2 — C10 oferece correção sem proposta disponível. Classe: ação antes do pré-requisito.** `aplicavel` significa apenas “depois do piso”; o censo exige seis plantões por competência, enquanto o propositor exige seis por EC. `app/ponto/management/commands/censo_fase_12x36.py:199`, `app/ponto/management/commands/censo_fase_12x36.py:216`, `app/escala/services/propositor.py:502`.

Contraexemplo: hoje 05/11; A cobre 21–28/10, prevê ímpares e tem quatro pares completos em 22/24/26/28; B cobre desde 29/10 e tem três plantões corretos em 30/10, 01/11 e 03/11. A competência tem sete plantões, paridade 42,9% e C10 de A com ação habilitada; A tem quatro dias batidos e B três, portanto nenhum ganha proposta. `app/ponto/management/commands/censo_fase_12x36.py:150`, `app/escala/services/cadastro_realidade.py:494`, `app/escala/services/propositor.py:497`. Reprodução Django **[não verificado]**.

**D. P2 — O novo teto é somente por data. Classe: teto temporal insuficiente.** `d < hoje` exclui hoje/futuro dos itens, mas não prova encerramento do turno de ontem; a lei explicitamente rejeita esse teto para cross-meia-noite. `app/ponto/management/commands/censo_fase_12x36.py:155`, `CLAUDE.md:330`.

Contraexemplo: cron de 05/11 às 07:38; escala 22–10, âncora 21/10; plantões completos deslocados em 22/24/26/28/30 de outubro e 01/03 de novembro já colocam a competência em desvio. O previsto de 04/11, sem batida, vira `trabalho_sem_batida` antes de seu término às 10h de 05/11. `app/config/crons.py:614`, `app/ponto/management/commands/censo_fase_12x36.py:197`. Reprodução Django **[não verificado]**.

**E. P2 — A fila continua julgando todos os dias pelo colaborador. Classe: redução indevida da fonte única.** A migração lê apenas `colabs_de_desvio`; depois qualquer chamado dessa pessoa cai em `grade_errada`, sem consultar o dia/EC lavrado. `app/chamados/services/fila_por_causa.py:174`, `app/chamados/services/fila_por_causa.py:220`.

Contraexemplo: A errado até 28/10, B correto desde 29/10; uma falta real em 03/11 sob B recebe causa `grade_errada` porque A mantém o colaborador no censo. A simplificação é herdada, mas a nova fonte já fornece a granularidade que esse leitor descarta. `app/escala/tests/test_o261_v_fase_na_lista.py:13`, `app/ponto/management/commands/censo_fase_12x36.py:248`. Reprodução Django **[não verificado]**.

**F. Riscos herdados que passam a afetar a autoridade única.**

- **Lavra parcial substitui a global:** `--empresa`/`--mes` estreitam a medição, mas a escrita usa sempre tenant/0/data. Uma execução `--empresa A --lavrar` pode retirar B da fotografia lida por todos. Classe: universo truncado na fonte; LEI-AKITA 8. `app/ponto/management/commands/censo_fase_12x36.py:74`, `app/ponto/management/commands/censo_fase_12x36.py:90`, `app/escala/services/fase_12x36.py:84`, `CLAUDE.md:35`.
- **`aplicavel` envelhece na virada:** em 21/11 antes da nova lavra, o snapshot de 20/11 ainda pode habilitar dias anteriores ao novo piso. O leitor aceita a última fotografia sem verificar essa compatibilidade temporal. Classe: pré-requisito temporal da fotografia. `app/escala/services/fase_12x36.py:99`, `app/escala/services/cadastro_realidade.py:343`, `app/escala/services/cadastro_realidade.py:494`.
- **“Plantão real” não exige par fechado:** o censo inclui toda `data_turno` retornada, enquanto o wrapper não exclui abertos. Seis entradas soltas podem atingir o piso de desvio. Isso é herdado do juiz escolhido, portanto não atribuo ao diff uma alteração desse predicado. `app/ponto/management/commands/censo_fase_12x36.py:147`, `app/ponto/turnos.py:1973`, `app/ponto/management/commands/censo_fase_12x36.py:216`.

**Conferência das outras classes pedidas:**

| Classe | Resultado |
|---|---|
| Segundo juiz | Não encontrei sobrevivência executável de `fora_de_fase`; os consumidores passaram à lavra. Os quatro sítios de âncora permanecem declaradamente fora desta obra. `app/escala/services/fase_12x36.py:10`, `app/escala/services/fase_12x36.py:67` |
| Segundo escritor fora da porta | Não encontrei introdução: intenção → propositor → porta retroativa → mutador do vínculo. `app/core/intake.py:49`, `app/chamados/services/proposta_no_fio.py:141`, `app/escala/services/propositor.py:738`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:211` |
| Relógio fora da borda | Permanece herdado: `_recusadas_recentes(hoje)` ignora o dia recebido e chama `timezone.now()`. O piso público usa o argumento recebido. `app/escala/services/propositor.py:666`, `app/escala/services/propositor.py:673`, `app/escala/services/propositor.py:609` |
| Ordem de trava / L-052 | Não encontrei nova aquisição que sustente acusação de inversão; a regeneração mantém `atomic`, guarda de exportadas e proteção de células humanas. `app/ponto/portas/celula.py:199`, `app/ponto/portas/celula.py:206`, `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:251` |
| Custo de frota no request | C10 lê snapshot; permanecem chamadas de frota no chat/API de propostas, anteriores a esta costura. Tempo/custo atual **[não verificado]**. `app/escala/services/cadastro_realidade.py:339`, `app/core/views_ia.py:572`, `app/api/views_mensageria.py:2019` |
| Pouso antes do pré-requisito | A base lavrava sem `itens`/`linhas`; os novos leitores aceitam ausência desses campos como listas vazias. É necessário conferir uma lavra no formato novo antes de declarar a migração efetiva; execução desse pré-requisito **[não verificado]**. `d80c98a5:app/ponto/management/commands/censo_fase_12x36.py:203`, `app/escala/services/cadastro_realidade.py:339`, `app/escala/services/propositor.py:492` |

**3. Os testes mordem?** [nao verificado]

A tabela descreve a consequência das asserções por leitura estática; execução dos testes e dos mutantes **[não verificado]**. [nao verificado]

| Teste novo em `test_o261_v_fase_na_lista.py` | Mordida e limite | [nao verificado]
|---|---|
| Lavra corrente — `app/escala/tests/test_o261_v_fase_na_lista.py:94` | Retirar dias exigidos, EC histórico, dono ou aplicabilidade corrente quebra. Não exige conjunto exato nem unicidade. |
| Não atribuir a B — `app/escala/tests/test_o261_v_fase_na_lista.py:108` | Atribuir qualquer item a B quebra; outro EC incorreto pode escapar. |
| Hoje/futuro — `app/escala/tests/test_o261_v_fase_na_lista.py:114` | Retirar o corte dos itens quebra. Não cobre ontem ainda em curso nem influência de turno aberto no julgamento. |
| Competência anterior — `app/escala/tests/test_o261_v_fase_na_lista.py:121` | Omitir os quatro dias examinados, trocar seu EC ou habilitá-los quebra. |
| Lavra sem âncora — `app/escala/tests/test_o261_v_fase_na_lista.py:130` | Acrescentar chaves proibidas quebra; isoladamente passa com itens vazios. |
| Idempotência — `app/escala/tests/test_o261_v_fase_na_lista.py:136` | Duplicar snapshot do mesmo dia ou variar o detalhe quebra; não prova preservação de outros dias. |
| C10 diária/vigia — `app/escala/tests/test_o261_v_fase_na_lista.py:148` | Retirar C10, EC/dono/URL ou integração diária quebra. Duplicar cada item pode passar: o dicionário colapsa duplicatas e a contagem cresce junto. |
| Dia antes do piso — `app/escala/tests/test_o261_v_fase_na_lista.py:161` | Habilitar URL em 17/10 ou retirar essa linha quebra. |
| Sem âncora no texto — `app/escala/tests/test_o261_v_fase_na_lista.py:168` | Introduzir “ancora” no detalhe/ação quebra; exige C10 presente. |
| Sem lavra — `app/escala/tests/test_o261_v_fase_na_lista.py:176` | Voltar a produzir C10 diretamente das batidas quebra; não exige indicação de ausência de medida. |
| Custo — `app/escala/tests/test_o261_v_fase_na_lista.py:180` | Queries extras no formatador **sem lavra** quebram; não protege o request completo nem a leitura com lavra presente. |
| Detector morreu — `app/escala/tests/test_o261_v_fase_na_lista.py:191` | Selo de forma: também reprova a alternativa autorizada de manter `fora_de_fase` apenas como leitor. |
| Pauta lê lavra — `app/escala/tests/test_o261_v_fase_na_lista.py:196` | Boa mordida: apaga batidas depois de lavrar; voltar ao cálculo vivo quebra. |
| API lê lavra — `app/escala/tests/test_o261_v_fase_na_lista.py:204` | Mesma mordida; não valida todo o formato do retorno. |
| Proposta do encerrado — `app/escala/tests/test_o261_v_fase_na_lista.py:222` | Retirar A ou contaminar com B quebra. **Retirar a leitura da lavra pode passar:** o teste não lavra. |
| Aplicação no EC histórico — `app/escala/tests/test_o261_v_fase_na_lista.py:231` | Morde alteração indevida de B e ausência de correção de A. Não passa pelo clique C10→fio nem verifica células/competência lavrada. |

Os dois testes de proposta não criam células previstas nem chamam `_lavrar()`: o previsto fica vazio, `_score` abre a candidatura por razão infinita e o gatilho da lavra fica dispensável. Assim, eles não defendem a costura central `ultimo_lavrado → fase_invertida`. `app/escala/tests/test_o261_v_fase_na_lista.py:63`, `app/escala/tests/test_o261_v_fase_na_lista.py:224`, `app/escala/services/propositor.py:199`, `app/escala/services/propositor.py:483`, `app/escala/services/propositor.py:522`.

Nos alterados:

- **`test_fase_12x36.py`:** coerente, invertido, noturno e ausência de turno agora consultam a lavra real. O invertido é a testemunha positiva contra um detector sempre vazio; os negativos sozinhos não bastariam. `app/escala/tests/test_fase_12x36.py:87`, `app/escala/tests/test_fase_12x36.py:99`, `app/escala/tests/test_fase_12x36.py:112`, `app/escala/tests/test_fase_12x36.py:126`.
- **Pauta zero/nascimento/teto/encerramento:** mordem esses comportamentos; o encerramento exige nova lavra depois da retirada das batidas. `app/escala/tests/test_fase_12x36.py:149`, `app/escala/tests/test_fase_12x36.py:157`, `app/escala/tests/test_fase_12x36.py:167`, `app/escala/tests/test_fase_12x36.py:175`.
- **`test_ancora_sugerida.py`:** saiu o selo do formato do detector morto; permanecem os sete casos puros de sugestão, desempate, evidência curta, ganho, motivo, ausência de âncora e dia trabalhado. Não distinguem piso seis de oito. `app/escala/tests/test_ancora_sugerida.py:43`, `app/escala/tests/test_ancora_sugerida.py:52`, `app/escala/tests/test_ancora_sugerida.py:59`, `app/escala/tests/test_ancora_sugerida.py:67`, `app/escala/tests/test_ancora_sugerida.py:75`, `app/escala/tests/test_ancora_sugerida.py:83`, `app/escala/tests/test_ancora_sugerida.py:90`, `app/escala/tests/test_ancora_sugerida.py:98`.
- **`test_semselo5...`:** o selo morde `ativa=True` detectável e exige uma chamada chamada `vinculo_do

[truncado em 20000 de 22568 caracteres]

## 30/10 18:30 · sessao 00a1bfe531de · - · - · origem=Ronald (tmux)

Achados procedentes no recorte:

1. **Aceite pode atingir outro EC; recusa de um pode calar outro.** O diff permite várias propostas da mesma pessoa (`app/escala/services/propositor.py:443`, `app/escala/services/propositor.py:541`), mas a assinatura continua contendo apenas `tipo/template_id/ancora/aplicar_desde`, sem `vinculo_id` (`app/escala/services/propositor.py:661`). O fio escolhe exclusivamente a primeira proposta e compara apenas essa assinatura (`app/chamados/services/proposta_no_fio.py:102`, `app/chamados/services/proposta_no_fio.py:106`). A recusa também usa apenas colaborador e assinatura (`app/escala/services/propositor.py:580`, `app/escala/services/propositor.py:674`).
   
   Contraexemplo: hoje 16/11, corte 21; EC A 12x36 de 19/10 a 01/11, EC B 12x36 de 02/11 a 15/11, ambos 08–16; batidas seg–sex nos respectivos trechos, dez dias por EC; template 5x2 08–16 existente. Ambos podem produzir a mesma troca de template, âncora `None`, início 21/10; o candidato por ritmo admite esse template (`app/escala/services/propositor.py:161`, `app/escala/services/propositor.py:183`; sem–sex no modelo: `app/escala/models.py:852`). Se A desaparece após correção por outro admin, o POST da tela de A aceita B com a mesma assinatura (`app/chamados/services/proposta_no_fio.py:107`).
   
   Confirmei a colisão executando somente em memória as funções reais extraídas por AST, com `python3 -B`: A/B retornam `67ed00e7fec03e2c676f82f33a2c1ced`; `_recalcular` aceita B sem erro. Os pontos executados estão em `app/escala/services/propositor.py:654` e `app/chamados/services/proposta_no_fio.py:92`.

2. **C10 oferece ação cujo pré-requisito não existe.** A lista dá link ao fio somente pelo booleano `aplicavel` (`app/escala/services/cadastro_realidade.py:489`, `app/escala/services/cadastro_realidade.py:495`); esse booleano compara dia e piso (`app/ponto/management/commands/censo_fase_12x36.py:199`). O censo exige seis plantões agregados na competência (`app/ponto/management/commands/censo_fase_12x36.py:148`, `app/ponto/management/commands/censo_fase_12x36.py:216`), enquanto o propositor exige seis por EC (`app/escala/services/propositor.py:497`, `app/escala/services/propositor.py:502`).
   
   Contraexemplo: hoje 05/11, corte 21; A 21–28/10, 12x36 07–19 com previsão ímpar e quatro pares completos em 22/24/26/28; B desde 29/10, corretamente prevendo três plantões completos em 30/10, 01/11 e 03/11. A competência tem sete plantões e paridade 42,9%, gerando C10 aplicável de A; A tem quatro dias batidos e B três, portanto nenhum tem proposta. A ação termina em fio sem correção disponível, pelos gates acima.

3. **A ação C10 não transporta a identidade do EC.** Embora a assinatura da lista preserve `ec_do_dia`, o link contém apenas colaborador (`app/escala/services/cadastro_realidade.py:341`, `app/escala/services/cadastro_realidade.py:495`); o fio mostra a primeira proposta por pessoa (`app/chamados/services/proposta_no_fio.py:63`, `app/chamados/services/proposta_no_fio.py:66`), ordenada por chamados resolvidos (`app/escala/services/propositor.py:582`). Havendo propostas para A e B, a linha de A pode abrir uma proposta de B. É o problema de migração do consumidor tornado possível pelo novo universo.

Leitores/escritores de produção:

- `propostas()` calcula e devolve o envelope e seus campos em `app/escala/services/propositor.py:537`, `app/escala/services/propositor.py:583`; não persiste a proposta. Leitores: fio (`app/chamados/services/proposta_no_fio.py:63`, `app/chamados/services/proposta_no_fio.py:102`), badge paginado (`app/chamados/services/painel.py:517`), ficha (`app/colaboradores/services/ficha.py:175`), preview/confirmação do chat (`app/core/views_ia.py:572`, `app/core/views_ia.py:645`), API mensageria (`app/api/views_mensageria.py:2019`), pauta (`app/escala/services/propositor.py:638`), comandos (`app/escala/management/commands/propostas_de_escala.py:26`, `app/escala/management/commands/ritmo_discordante.py:18`).
- `piso_de_aplicacao` calcula o início via `janela_atual` (`app/escala/services/propositor.py:595`, `app/escala/services/propositor.py:609`); leitores diretos: filtro dos EC encerrados (`app/escala/services/propositor.py:459`), wrapper que fornece `aplicar_desde` (`app/escala/services/propositor.py:558`, `app/escala/services/propositor.py:592`) e censo (`app/ponto/management/commands/censo_fase_12x36.py:98`).
- A decisão persiste em `DecisaoProposta` por `recusar`/`aplicar` (`app/escala/services/propositor.py:690`, `app/escala/services/propositor.py:746`). O escritor de escala declarado continua sendo `propositor.aplicar` (`app/core/intake.py:49`), que delega à porta existente (`app/escala/services/propositor.py:738`).

Verificações sem achado novo:

- Não encontrei segundo escritor introduzido nesse recorte: fio delega ao escritor declarado (`app/chamados/services/proposta_no_fio.py:141`), que chama a porta e seu mutador (`app/escala/services/propositor.py:738`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:211`; `app/colaboradores/services/vinculo.py:839`).
- A regeneração mantém guarda de exportadas e células humanas (`app/ponto/portas/celula.py:199`, `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:251`) e usa `atomic` (`app/ponto/portas/celula.py:206`). Não identifiquei nova aquisição de trava que sustente achado de ordem.
- Relógio fora da borda **herdado**: `_recusadas_recentes(hoje)` ignora o argumento e usa `timezone.now()` (`app/escala/services/propositor.py:666`, `app/escala/services/propositor.py:673`). O piso público usa o dia recebido (`app/escala/services/propositor.py:609`).
- Custo de frota no request **herdado e ainda presente** em `app/core/views_ia.py:572` e `app/api/views_mensageria.py:2019`; a rota específica da fase foi migrada para lavra (`app/api/views_mensageria.py:1673`).
- O teste alterado do fio só verifica assinatura Python e texto `__iter__`, sem provar funcionamento de um ID/lista (`app/chamados/tests/test_proposta_no_fio.py:185`). Os testes de tela velha fornecem somente uma proposta e não cobrem colisão entre EC (`app/chamados/tests/test_proposta_no_fio.py:128`, `app/chamados/tests/test_proposta_no_fio.py:140`).
- Os dois novos testes de proposta exercitam apenas A elegível/B inelegível, e a aplicação direta de A (`app/escala/tests/test_o261_v_fase_na_lista.py:222`, `app/escala/tests/test_o261_v_fase_na_lista.py:231`); não passam pelo clique C10→fio nem cobrem dois EC elegíveis.

## 30/10 18:29 · sessao 66325653280c · - · - · origem=Ronald (tmux)

Auditoria somente por leitura; nenhum teste foi executado. [não verificado] [nao verificado]

**Escritor e leitores da lavra**

| Campo | Quem produz/grava | Quem lê em produção |
|---|---|---|
| Chave `desvio_de_cadastro_12x36` | `fase_12x36.CHAVE`, persistida por `lavrar`, chamado pelo censo: `app/escala/services/fase_12x36.py:75`, `:78`, `:84`; `app/ponto/management/commands/censo_fase_12x36.py:239` | `ultimo_lavrado`: `app/escala/services/fase_12x36.py:96` |
| `valor` | `len(colabs_de_desvio)`: `app/escala/services/fase_12x36.py:86` | Não encontrei leitor produtivo específico; os consumidores leem `detalhe` e contam os colaboradores: `app/escala/services/fase_12x36.py:102`, `app/api/views_mensageria.py:1675` |
| `colabs_de_desvio` | Censo: `app/ponto/management/commands/censo_fase_12x36.py:242` | Pauta: `app/escala/services/fase_12x36.py:117`; fila: `app/chamados/services/fila_por_causa.py:174`; API: `app/api/views_mensageria.py:1675`; vigia: `app/ponto/management/commands/vigiar_familia_escala.py:27` |
| `linhas` | Resumo por colaborador e competência: `app/ponto/management/commands/censo_fase_12x36.py:245` | API: `app/api/views_mensageria.py:1677`; comando: `app/ponto/management/commands/vigiar_familia_escala.py:28` |
| `itens` | Dias das competências em desvio: `app/ponto/management/commands/censo_fase_12x36.py:248` | C10: `app/escala/services/cadastro_realidade.py:339`; gatilho de proposta: `app/escala/services/propositor.py:491` |
| `ec_do_dia` | Resultado de `vinculo_do_dia`: `app/ponto/management/commands/censo_fase_12x36.py:131`, `:135`, `:197` | C10: `app/escala/services/cadastro_realidade.py:341`; proposta identifica `(colab, ec)`: `app/escala/services/propositor.py:491`, `:513` |
| `dono_do_dia` | Literal `CADASTRO`: `app/ponto/management/commands/censo_fase_12x36.py:249` | C10: `app/escala/services/cadastro_realidade.py:342`, `:646`; índice para vigia: `:718` |
| `aplicavel` | Comparação com piso do propositor: `app/ponto/management/commands/censo_fase_12x36.py:98`, `:199`, `:203` | C10: `app/escala/services/cadastro_realidade.py:343`, `:647`; ação/link: `:494` |

**Achados a considerar**

1. **Teto por data ainda julga turno não encerrado.** O novo corte é apenas `d < _hoje`, exatamente a proteção que a seção 6 diz ser insuficiente em turno noturno. `app/ponto/management/commands/censo_fase_12x36.py:155`; `CLAUDE.md:330`.

   Contraexemplo: hoje 05/11 às 07:38; cadastro 12x36 das 22h às 10h; 04/11 é previsto, sem batida, e seis plantões anteriores fora do previsto já colocam a competência em desvio. A lavra publica 04/11 como `trabalho_sem_batida` antes do encerramento às 10h. O cron roda às 07:38. `app/ponto/management/commands/censo_fase_12x36.py:197`; `app/config/crons.py:614`. O teste cobre hoje/futuro, mas usa diurno e não cobre ontem ainda em curso. `app/escala/tests/test_o261_v_fase_na_lista.py:114`, `:60`.

2. **Lavratura parcial pode substituir a autoridade global.** `--empresa` filtra o universo; `--mes` também pode restringi-lo; a escrita continua usando a mesma chave tenant/0/data de hoje. `app/ponto/management/commands/censo_fase_12x36.py:39`, `:43`, `:74`, `:90`; `app/escala/services/fase_12x36.py:84`.

   Contraexemplo: A e B possuem fase deslocada; após lavra completa, executar `--empresa A --lavrar` apaga B da fotografia consumida pela lista, fila, pauta e API. É mecanismo herdado que passa a afetar todos esses consumidores pela nova autoridade única. `app/escala/services/fase_12x36.py:86`; `app/escala/services/cadastro_realidade.py:339`.

3. **A fila descarta a granularidade por dia.** A migração lê só colaboradores em desvio; qualquer chamado da pessoa cai em `grade_errada`, independentemente do dia ou EC. `app/chamados/services/fila_por_causa.py:174`, `:220`.

   Contraexemplo adaptado da própria fixture: EC A errado até 28/10; EC B correto desde 29/10; falta real em 03/11 sob B. Como A mantém a pessoa no censo, o chamado da falta sob B recebe causa `grade_errada`. A fixture distingue esses segmentos, mas nenhum teste novo exercita essa classificação. `app/escala/tests/test_o261_v_fase_na_lista.py:13`, `:14`; `app/chamados/services/fila_por_causa.py:221`. A simplificação existia anteriormente; a nova lavra oferece os dias que o leitor continua descartando. `app/ponto/management/commands/censo_fase_12x36.py:248`.

4. **Risco herdado: “plantão real” inclui turno aberto/orfão.** O censo transforma todos os turnos retornados em datas, sem verificar fechamento ou saída; o wrapper retorna também turnos abertos. `app/ponto/management/commands/censo_fase_12x36.py:147`; `app/ponto/turnos.py:1973`.

   Seis entradas soltas em folgas, sem saídas, podem satisfazer o piso e produzir paridade zero, com dono `CADASTRO`. O detector removido exigia turno fechado; o censo escolhido já não exigia. É necessário distinguir defeito herdado do predicado expressamente preservado pelo aval. `app/ponto/management/commands/censo_fase_12x36.py:216`, `:249`.

5. **Foto de `aplicavel` atravessa a mudança de competência.** O último snapshot não exige data atual; a lista utiliza seu booleano sem outro corte. `app/escala/services/fase_12x36.py:96`, `:99`; `app/escala/services/cadastro_realidade.py:343`, `:494`.

   Em 21/11 antes da nova lavra, uma fotografia de 20/11 ainda oferece ação para outubro/novembro anterior ao novo piso. É limitação de frescor/pré-requisito da fotografia, não justificativa para recalcular no request. `app/ponto/management/commands/censo_fase_12x36.py:199`; `app/escala/services/cadastro_realidade.py:493`.

**Testes** [nao verificado]

Os casos novos mordem a existência dos dias, a atribuição ao EC encerrado, os dois lados do piso, ausência de hoje/futuro e idempotência; remover essas partes quebra as respectivas asserções. `app/escala/tests/test_o261_v_fase_na_lista.py:94`, `:108`, `:114`, `:121`, `:136`.

Pauta e API têm boa testemunha: após lavrar, apagam as batidas e exigem manutenção do resultado. Reintroduzir cálculo vivo quebra os casos. `app/escala/tests/test_o261_v_fase_na_lista.py:196`, `:204`.

A ausência de `fora_de_fase` é teste de forma: quebra inclusive uma implementação autorizada que mantivesse o nome apenas como leitor da lavra. `app/escala/tests/test_o261_v_fase_na_lista.py:191`; a própria regra citada permite “ler ou morrer”. `:8`.

O teste sem âncora nos itens não exige itens presentes e pode passar vazio; os demais testes positivos da mesma classe compensam parte dessa vacuidade. `app/escala/tests/test_o261_v_fase_na_lista.py:130`, `:100`.

O teste de uma consulta impede varredura dentro de `_fase_deslocada`, mas não mede todo `lista()` nem request completo. `app/escala/tests/test_o261_v_fase_na_lista.py:180`, `:184`.

**Migração de chamadas**

Não encontrei chamadas/importações executáveis pendentes de `fora_de_fase` ou das constantes removidas em produção, testes, templates ou JS; a implementação foi apagada e os cinco caminhos declarados migraram à lavra. `app/escala/services/fase_12x36.py:67`, `:74`; `app/api/views_mensageria.py:1671`; `app/chamados/services/fila_por_causa.py:153`; `app/escala/services/propositor.py:491`; `app/ponto/management/commands/vigiar_familia_escala.py:22`.

A mensageria apenas repassa o JSON, sem acessar campos antigos; não encontrei consumidor executável não migrado desse formato. `mensageria/nucleo/ferramentas.py:1407`, `:1409`; `mensageria/nucleo/core_client.py:244`.

Restam textos antigos sobre o predicado de dois lados/três ocorrências, sem execução: API, ferramenta e documentação do fio; podem induzir manutenção incorreta, mas não são chamadas órfãs. `app/api/views_mensageria.py:1660`; `mensageria/nucleo/ferramentas.py:1399`; `app/chamados/services/proposta_no_fio.py:17`.

## 30/10 18:28 · sessao a1d2daa0f14f · - · - · origem=Ronald (tmux)

Análise estática; não executei testes nem mutantes, respeitando somente leitura. [não verificado] [nao verificado]

**Testes novos — `app/escala/tests/test_o261_v_fase_na_lista.py`:**

| Teste, linha | Se a cura sair/mudar | [nao verificado]
|---|---|
| `test_MORDE_lavra_da_competencia_CORRENTE...`:94 | Morde ausência dos oito dias exigidos, atribuição a EC B, ausência de dono CADASTRO e `aplicavel=False` nos dias correntes; não verifica conjunto exato nem unicidade. |
| `test_MORDE_nenhum_dia_e_atribuido...`:108 | Morde qualquer item atribuído a B; permite outros ECs incorretos desde que B não apareça. |
| `test_MORDE_hoje_e_futuro_nunca_entram`:114 | Morde itens `>=05/11`; exige saída não vazia. Não prova que turnos reais de hoje/futuro deixaram de influenciar o julgamento. |
| `test_MORDE_competencia_anterior...`:121 | Morde omissão dos dias 17–20/10, EC errado nesses quatro dias ou aplicação antes do piso. |
| `test_lavra_sem_ancora_proposta`:130 | Morde chaves contendo `ancora`, `proposta` ou `virada` nos itens; pode passar sem itens, embora os demais testes façam a lavra positiva. | [nao verificado]
| `test_idempotente`:136 | Morde duplicação de snapshots no mesmo dia ou detalhe variável; não verifica preservação de snapshots de outros dias. |
| `test_MORDE_C10_um_item_por_dia...`:148 | Morde falta de C10 em 25/10, dono/EC/url errados e falta de integração com `dias_na_lista`; **não morde duplicação por dia**: `por_dia` colapsa duplicatas e o total é comparado com `len(c10)`, que também cresce. |
| `test_MORDE_dia_antes_do_piso...`:161 | Morde ausência da C10 em 17/10 ou URL habilitada nesse dia. |
| `test_MORDE_sem_ancora_na_linha`:168 | Morde texto contendo `ancora` em detalhe/ação; exige C10 não vazia. |
| `test_sem_lavra_a_lista...`:176 | Morde C10 calculada das batidas sem snapshot. Não exige explicitamente marcador de ausência de medida. |
| `test_MORDE_a_lista_nao_mede...`:180 | Morde queries adicionais em `_fase_deslocada(None)` **sem lavra**; não mede `lista()` nem cenário com lavra presente. |
| `test_MORDE_fora_de_fase_morreu`:191 | É selo de forma: falha se surgir uma função homônima que apenas lê a lavra, alternativa autorizada pelo pedido literal. |
| `test_MORDE_pauta_le_a_lavra...`:196 | Morde retirada da leitura persistida e volta ao cálculo das batidas, pois apaga batidas após lavrar e exige `n=1`. |
| `test_MORDE_api_da_mensageria...`:204 | Mesma mordida na API; token é mockado, leitura da fase é real. Não valida formato completo de `vinculos`. |
| `test_MORDE_proposta_nasce...`:222 | Morde perda do EC encerrado, contaminação por B e contagem diferente de 11 batidos. **Não morde leitura da lavra pelo propositor**, pois não chama `_lavrar()`. |
| `test_MORDE_aplicar_corrige...`:231 | Morde ausência de correção de A, alteração da âncora/ativa de B e alteração de fim/ativa de A; usa `aplicar` real. Não verifica células, fechamento lavrado, nem passagem pelo POST real do fio. |

Cada avaliação acima deriva das asserções nas respectivas linhas do arquivo indicado. O fixture das propostas cria ECs e batidas, sem células previstas; portanto `prev` fica vazio (`app/escala/services/propositor.py:483` e `:510`), `_score` resulta infinito (`:199`–`:207`) e a candidatura entra por `razao > 1` (`:522`). Remover `ultimo_lavrado`/`invertidos` (`:489`–`:492`, `:513`) deixa esses dois casos capazes de passar.

**Testes alterados:** [nao verificado]

- `test_fase_12x36.py`: os quatro casos passam a consultar a lavra real: coerente não acende (`:87`), invertido acende (`:99`), noturno coerente não acende (`:112`), ausência de turno não acende (`:126`). Só o positivo invertido impede que uma implementação que nunca detecta desvio passe. Os negativos isolados passam por ausência de sinal; juntos há contraste positivo. A migração retira as antigas exigências de três ocorrências de cada lado, coerente com a troca do predicado autorizada; o arquivo ainda conserva essa descrição histórica no cabeçalho (`:14`–`:16`).
- Pauta zero (`test_fase_12x36.py:149`), nascimento (`:157`), teto semanal (`:167`) e encerramento (`:175`) mordem as respectivas ações reais. O último agora exige nova lavra após apagar batidas (`:183`–`:186`); uma pauta que reconta batidas diretamente pode continuar satisfazendo esses casos, sendo a independência da fonte protegida pelo teste novo de `:196`. [nao verificado]
- `test_ancora_sugerida.py`: apenas removeu `NaListaTest`, que inspecionava fonte do detector morto; os sete testes puros permanecem (`:43`, `:52`, `:59`, `:67`, `:75`, `:83`, `:90`). Eles mordem sugestão positiva, desempate recente, recusa com duas batidas, ganho nulo, motivo e pertença da âncora aos dias trabalhados; **não distinguem piso 6 de 8**, embora o código use 6 (`fase_12x36.py:22`) e o cabeçalho do teste ainda descreva 8 (`test_ancora_sugerida.py:15`). [nao verificado]
- `test_semselo5_ativo_nao_responde_por_dia.py`: adiciona `propostas` aos CURADOS (`:38`–`:40`) e remove detector/propositor das exceções antigas. `test_os_curados...` (`:379`) morde `ativa=True` detectável pelo scanner e exige chamada com nome `vinculo_do_dia`; **não prova uso do resultado**. Uma chamada morta satisfaz a presença de nome, e trocar por helper equivalente com outro nome provoca falso vermelho (`:384`–`:388`). As demais provas adversariais testam o scanner, não o comportamento do propositor (`:391`–`:424`).
- `test_proposta_no_fio.py:185`: única alteração sobrevivente é inspeção de assinatura para `colaborador_id` e busca textual de `__iter__` (`:189`–`:192`). **Não compara** respostas para escalar/lista/frota e não limita consultas. Pode permanecer verde com implementação semanticamente incorreta, desde que conserve essas duas formas. O selo textual de pré-carga do detector foi removido com ele; demais testes do arquivo não foram alterados e mockam `propostas`/`aplicar` (`:73`–`:178`). [nao verificado]

**Lacunas relevantes para o parecer:**

- Não há proteção das bordas 5/6 plantões e 89,9/90% nos testes novos/alterados: o golden usa 7 plantões com 42,9% e 10 com 0% (`test_o261_v_fase_na_lista.py:18`–`:25`), enquanto os casos antigos são coerente/invertido (`test_fase_12x36.py:87`–`:124`). Um limiar `<50%` no censo preservaria esses casos, embora viole a regra `<90%` (`censo_fase_12x36.py:216`). [nao verificado]
- Não há teste de virada de competência com lavra antiga e `aplicavel` envelhecido: todos os casos novos estão congelados em 05/11 (`test_o261_v_fase_na_lista.py:91`, `:145`). [nao verificado]
- Novos testes de leitor cobrem pauta e API, não `fila_por_causa` e `vigiar_familia_escala` (`test_o261_v_fase_na_lista.py:189`–`:216`). [nao verificado]
- Não vi evidência de RED executado nesses arquivos; o cabeçalho declara “escritos ANTES” (`test_o261_v_fase_na_lista.py:3`), mas a execução histórica permanece [não verificado]. A lei exige RED evidenciado (`CLAUDE.md:28`, `:348`) e teste que falhe quando a costura sair (`CLAUDE.md:350`–`:355`). [nao verificado]

## 30/10 18:23 · sessao d9783118c056 · O261-E-CELULA-20 · censo · origem=Code

**Há um núcleo de persistência do estado, mas falta uma guarda comum de encerramento que consulte todas as perguntas do chamado.** `_gravar_estado` concentra a transição física; `transicionar`, `reconciliar`, `cancelar` e `encerrar` chegam a ele com guardas diferentes. `pergunta_viva` existe como juiz, porém não é uma condição obrigatória anterior a toda transição terminal. `app/chamados/models.py:122`, `app/chamados/models.py:185`, `app/chamados/models.py:434`, `app/chamados/models.py:527`, `app/chamados/models.py:1288`.

**O campo do chamado é `status_local`, não `status`.** `ENCERRADOS` contém `resolvido/fechado`; `TERMINAIS` acrescenta `cancelado/superado`. `registrado` pertence a `VIVOS`. Arquivar/silenciar é outro eixo, sem alteração de estado. `app/chamados/models.py:46`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:31`, `app/chamados/models.py:81`.

O método foi inspeção estática por operação: AST de atribuições, chamadas ORM, argumentos e dicionários; conferência de `save`, `update`, `bulk_update`, `setattr`, SQL e criação; depois leitura das condições e rastreamento dos chamadores. Testes foram separados dos atores de produção; migrações e comandos operacionais estão identificados abaixo. Não executei Django, testes ou banco, nem criei, editei ou apaguei arquivos. Identidade Git do snapshot como `main` no hash informado, execução real, banco e cron instalado: **[nao verificado]**. [nao verificado]

O limite é explícito: o censo cobre os sítios de código presentes e as expansões estáticas identificáveis; geração dinâmica de campos/modelos, chamadas externas por shell, monkeypatches, triggers e SQL fora da árvore permanecem **[nao verificado]**. O próprio censo existente distingue operações sintáticas de atribuição e verbos ORM, sem provar execução. `bin/esmeril_chamado_censo.py:99`, `bin/esmeril_chamado_censo.py:113`.

**1. Escritores do estado terminal**

A escrita física de transição é `self.status_local = novo`, seguida de `save(update_fields=campos)`. A validação comum exige uma via terminal reconhecida, mas não consulta C6 antes de salvar. A relavra das perguntas acontece **depois** do save quando cruza a fronteira `ENCERRADOS`. `app/chamados/models.py:192`, `app/chamados/models.py:197`, `app/chamados/models.py:204`, `app/chamados/models.py:231`, `app/chamados/models.py:243`.

| Entrada de escrita | Valor e condição | Pergunta viva antes? |
|---|---|---|
| `transicionar` / `fechar` | `resolvido` ou `fechado`, conforme motor/perfil; fechado recusa justificativa pendente; resolvido de furo exige premissa regularizada. `app/chamados/models.py:122`, `app/chamados/models.py:130`, `app/chamados/models.py:165`, `app/chamados/models.py:262` | **Parcial:** para `ENCERRADOS`, consulta a primeira disputa aberta cujo pai é o chamado; bloqueia se vazia ou com `fio_vivo`. Não percorre todas as perguntas que apontam ao chamado como destino. `app/chamados/models.py:142`, `app/chamados/juizes.py:1817`, `app/chamados/juizes.py:1820` |
| `reconciliar` | `VIVOS → resolvido`; relê estado; aplica guarda de premissa para folha, agrupamento sob pai vivo ou outra cobrança viva. `app/chamados/models.py:434`, `app/chamados/models.py:463`, `app/chamados/models.py:474`, `app/chamados/models.py:489`, `app/chamados/models.py:498` | **Não como guarda geral C6.** A decisão é da premissa/regularização. `app/chamados/models.py:474` |
| `cancelar` | `VIVOS → cancelado`, permitido pelo motor. `app/chamados/models.py:527`, `app/chamados/models.py:532`, `app/chamados/models.py:537` | Não. `app/chamados/models.py:527` |
| `superar` | `VIVOS → superado`; superador diferente de si e não terminal. `app/chamados/models.py:543`, `app/chamados/models.py:547`, `app/chamados/models.py:560` | Não; está declarado sem chamador de produção. `bin/esmeril_chamado_censo.py:143` |
| `encerrar` | `aberto/em_analise/registrado/resolvido → fechado`, via cascade. `app/chamados/models.py:1288`, `app/chamados/models.py:1293`, `app/chamados/models.py:1297` | Não. `app/chamados/models.py:1288` |
| `encerrar_por_competencia_trancada` | `VIVOS → fechado`, via `competencia_trancada`; serviço verifica a competência. `app/chamados/models.py:1300`, `app/chamados/models.py:1305`, `app/chamados/models.py:1310`, `app/chamados/services/competencia_trancada.py:158` | Não no método. `app/chamados/models.py:1300` |

No inventário seguinte, **R** significa `reconciliar → resolvido`, **E** significa `encerrar → fechado`, **H** significa `transicionar/fechar → resolvido ou fechado`, e **C** significa `cancelar → cancelado`. Essas entradas herdam as guardas da tabela anterior; verificações adicionais de C6 estão assinaladas. `app/chamados/models.py:262`, `app/chamados/models.py:434`, `app/chamados/models.py:527`, `app/chamados/models.py:1288`.

| Chamador e disparador | Entrada e condição própria |
|---|---|
| Signal de disputa fechada | **R**, pai existente ainda fora de `ENCERRADOS`; o receiver não consulta C6 novamente. `app/chamados/signals.py:28`, `app/chamados/signals.py:33`, `app/chamados/signals.py:42` |
| Reconciliação por colaborador: evento de batida e cron | Retratação quando a premissa morreu; retém irmãs respondidas sem consumo. **R**. `app/chamados/reconciliador.py:439`, `app/chamados/reconciliador.py:520`, `app/chamados/reconciliador.py:525`, `app/chamados/reconciliador.py:1348`, `app/chamados/reconciliador.py:1369` |
| Consumidor do evento da célula | Célula absolveu chamado de turno → retratação **R**. `app/chamados/reconciliador.py:933`, `app/chamados/reconciliador.py:951`, `app/chamados/reconciliador.py:955` |
| Aprovação de justificativa / ajuste retroativo | Fecho forçado com retenção → **E**. `app/chamados/reconciliador.py:487`, `app/chamados/reconciliador.py:494`, `app/chamados/reconciliador.py:498`, `app/ponto/services/justificativa.py:79`, `app/ponto/services/justificativa.py:385` |
| Desligamento | Todos `NAO_FECHADOS` do colaborador → **E**. `app/colaboradores/services/desligamento.py:132`, `app/colaboradores/services/desligamento.py:137` |
| Nova escala/aceite | Chamados vivos anteriores de `escala_aceite` → **E**. `app/colaboradores/services/vinculo.py:373`, `app/colaboradores/services/vinculo.py:384`, `app/colaboradores/services/vinculo.py:387` |
| Regularização externa materializada | Havendo chamado → **E**. `app/chamados/services/regularizacao_ext.py:197`, `app/chamados/services/regularizacao_ext.py:201`, `app/chamados/services/regularizacao_ext.py:205` |
| Competência trancada | Aplicação da tranca → entrada específica, `fechado`. `app/chamados/services/competencia_trancada.py:156`, `app/chamados/services/competencia_trancada.py:160`, `app/chamados/services/competencia_trancada.py:177` |
| Adesão: agrupamento de filhos | Furo no período agrupado sob pai → **R**. `app/chamados/services/adesao.py:102`, `app/chamados/services/adesao.py:107`, `app/chamados/services/adesao.py:112`, `app/chamados/services/adesao.py:115` |
| Adesão: estoque | Pai `nunca_bateu` com batida, fechamento habilitado e fora de dry-run → **R**. `app/chamados/services/adesao.py:142`, `app/chamados/services/adesao.py:148`, `app/chamados/services/adesao.py:150` |
| Evento de batida / pós-vínculo | Batida não retratada na janela do pai → **R**. `app/colaboradores/services/pos_vinculo.py:17`, `app/colaboradores/services/pos_vinculo.py:36`, `app/colaboradores/services/pos_vinculo.py:41` |
| Aceite de sugestão de folga | Fecha LIMBO-FOLGAS vivos do template → **R**. `app/chamados/services/limbo_folgas.py:73`, `app/chamados/services/limbo_folgas.py:91`, `app/chamados/services/limbo_folgas.py:98`, `app/chamados/services/limbo_folgas.py:101` |
| Agrupamento sob LIMBO | Furo ainda não agrupado → **R**; detector é chamador. `app/chamados/services/limbo_folgas.py:214`, `app/chamados/services/limbo_folgas.py:221`, `app/ponto/management/commands/detectar_ausencias.py:756` |
| Silêncio por ausência | Ausência aprovada, premissa morta e aplicação habilitada → **R**. `app/chamados/services/silencio_ausencia.py:93`, `app/chamados/services/silencio_ausencia.py:100` |
| Evento de ausência | Ausência saiu de `ABERTAS` → **R**; cron correspondente é vigia. `app/ponto/services/ausencia.py:670`, `app/ponto/services/ausencia.py:678`, `app/ponto/services/ausencia.py:705`, `app/ponto/services/ausencia.py:709` |
| Materialização de ausência respondida | Fecha destino somente quando ausência não está aberta → **R**. `app/chamados/services/materializacao.py:531`, `app/chamados/services/materializacao.py:538` |
| Materialização: destino e clones | **R**, com retenção de irmãs respondidas sem consumo. `app/chamados/services/materializacao.py:679`, `app/chamados/services/materializacao.py:701`, `app/chamados/services/materializacao.py:711`, `app/chamados/services/materializacao.py:717` |
| Entrada sincronizada | Todos os marcos cobrados acesos → **R**. `app/ponto/services/entrada_sincronizada.py:19`, `app/ponto/services/entrada_sincronizada.py:28`, `app/ponto/services/entrada_sincronizada.py:31` |
| Triagem/registro de batida | Entrada fecha falta; saída fecha o turno correspondente, recusando outro turno aberto; exige premissa morta → **R**. `app/ponto/services/triagem_batida.py:522`, `app/ponto/services/triagem_batida.py:535`, `app/ponto/services/triagem_batida.py:580`, `app/ponto/services/triagem_batida.py:593`, `app/ponto/services/triagem_batida.py:597` |
| Cobertura designada/passada | Origem viva e premissa morta; sobrevivente por último → **R**. `app/ponto/services/cobertura.py:114`, `app/ponto/services/cobertura.py:122`, `app/ponto/services/cobertura.py:309`, `app/ponto/services/cobertura.py:318` |
| Admin decide o dia | Fecha perguntas por decisão administrativa e reconcilia chamados → **R**. `app/ponto/services/veredito_celula.py:160`, `app/ponto/services/veredito_celula.py:177`, `app/ponto/services/veredito_celula.py:183` |
| Cartório: lastro | Aplicando, marcos cobrados acesos ou inexistentes → **R**. `app/ponto/services/cartorio.py:1153`, `app/ponto/services/cartorio.py:1163`, `app/ponto/services/cartorio.py:1173`, `app/ponto/services/cartorio.py:1175` |
| Cartório: cobrança indevida | `COBRANCA_EM_DIA_SEM_TRABALHO` ou `COBRANCA_SEM_FURO` → **R**. `app/ponto/services/cartorio.py:1188`, `app/ponto/services/cartorio.py:1196` |
| Cartório: perguntas duplicadas | Sem fio vivo, sem resposta escrita e todas `VIA_DUPLICATA` → **R**. **Aqui há consulta local do fio/C6.** `app/ponto/services/cartorio.py:1258`, `app/ponto/services/cartorio.py:1261`, `app/ponto/services/cartorio.py:1263`, `app/ponto/services/cartorio.py:1266` |
| Fechador de lastro | Quadro `com_lastro`, aplicação habilitada; relê estado vivo → **R**. `app/ponto/services/lastro.py:300`, `app/ponto/services/lastro.py:304`, `app/ponto/services/lastro.py:310`, `app/ponto/services/lastro.py:317` |
| Feriado/folga: evento e cron | Dia espúrio pela autoridade do dia → **R**. `app/chamados/services/feriado_reavaliar.py:8`, `app/chamados/services/feriado_reavaliar.py:23`, `app/chamados/services/feriado_reavaliar.py:36`, `app/chamados/services/feriado_reavaliar.py:47` |
| Cluster espúrio | Premissa morta dos chamados do colaborador com escala → **R**. `app/ponto/services/cluster_espurio.py:35`, `app/ponto/services/cluster_espurio.py:46`, `app/ponto/services/cluster_espurio.py:73`, `app/ponto/services/cluster_espurio.py:80` |
| `detectar_ausencias` | Limpa LIMBO antigo; fecha por afastamento, cobertura ou entrada → **R**. `app/ponto/management/commands/detectar_ausencias.py:158`, `app/ponto/management/commands/detectar_ausencias.py:812`, `app/ponto/management/commands/detectar_ausencias.py:824`, `app/ponto/management/commands/detectar_ausencias.py:834` |
| `detectar_intervalo_ausente` | Premissa morta; aplicação reconcilia, dry consulta possibilidade → **R**. `app/ponto/management/commands/detectar_intervalo_ausente.py:115`, `app/ponto/management/commands/detectar_intervalo_ausente.py:128`, `app/ponto/management/commands/detectar_intervalo_ausente.py:134` |
| `marcar_feriado_do_posto` | Alvos do feriado → **R**. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:116`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:126` |
| `curar_residuo_bug115` | Ata sem furo → **R**. `app/ponto/management/commands/curar_residuo_bug115.py:150` |
| `auditar_chamados_orfaos --fechar` | `disputa_supervisao` sem disputa aberta → **R**. `app/chamados/management/commands/auditar_chamados_orfaos.py:34`, `app/chamados/management/commands/auditar_chamados_orfaos.py:58`, `app/chamados/management/commands/auditar_chamados_orfaos.py:67` |
| `fechar_chamados_meio_periodo --apply` | Usuário assinante superuser ativo → **H**, default `fechado`. `app/ponto/management/commands/fechar_chamados_meio_periodo.py:94`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:103`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109` |
| Avisos de régua / vínculo migrados à lista | `--apply` → **C**. `app/escala/management/commands/fechar_avisos_regua_na_lista.py:32`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:40`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57` |
| Tela de benefício | Aprovar → `resolvido`; outra ação → `fechado`, via **H**. `app/colaboradores/views.py:2055`, `app/colaboradores/views.py:2068` |
| Telas de exceção, agenda, escala e geofence | Decisão → `resolvido`, via **H**. `app/chamados/services/acoes_chamado.py:70`, `app/chamados/services/acoes_chamado.py:108`, `app/chamados/services/acoes_chamado.py:149`, `app/chamados/services/acoes_chamado.py:222`, `app/chamados/services/acoes_chamado.py:277` |
| Resposta livre / fio | Novo estado solicitado, limitado por **H** e RBAC. `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/acoes_chamado.py:315`, `app/chamados/services/fio.py:703`, `app/chamados/services/fio.py:715` |
| Smoke operacional | Exercita transições **H** e recusas. `app/chamados/management/commands/smoke_chamados.py:134`, `app/chamados/management/commands/smoke_chamados.py:147`, `app/chamados/management/commands/smoke_chamados.py:163` |

Há duas superfícies adicionais que **não passam pelo núcleo de transição**:

- **Nascimento:** `criar(status=...)` preserva status diferente de `aberto` e entrega-o ao `objects.create`; o smoke cria estados `fechado/resolvido`. `abrir` monta estado inicial, mas incorpora `defaults` antes do `get_or_create`. Assim, concentrar transições em `_gravar_estado` não fecha a entrada genérica de criação. `app/chamados/models.py:702`, `app/chamados/models.py:704`, `app/chamados/models.py:735`, `app/chamados/models.py:818`, `app/chamados/models.py:827`, `app/chamados/models.py:851`, `app/chamados/management/commands/smoke_chamados.py:122`, `app/chamados/management/commands/smoke_chamados.py:146`.
- **Migração 0039:** `update` direto `cancelado → superado` quando há irmão único do mesmo colaborador/dia nos módulos de furo; reversa `superado → cancelado`. Não passa pelo núcleo ou C6. `app/chamados/migrations/0039_a3_via_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:20`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:33`.

Nos modelos relacionados, `DisputaSupervisao` encerra por `fechada_em`, com porta `fechar/pode_fechar`, não por `status`. `RegularizacaoExterna` possui `status` próprio e FK para chamado: `materializada` é gravado pelo materializador; `recusada` devolve à supervisão e mantém o chamado aberto. `app/chamados/models.py:1504`, `app/chamados/models.py:1530`, `app/chamados/models.py:2744`, `app/chamados/models.py:2763`, `app/chamados/models.py:2772`, `app/chamados/services/regularizacao_ext.py:197`, `app/chamados/services/regularizacao_ext.py:261`.

Não foi localizado outro modelo persistente de chamado com estado terminal na árvore **[nao verificado quanto a modelos dinâmicos/externos]**. O cliente GLPI abre ticket com `status=1` e lê estados externos 5/6; não é um segundo modelo ORM local. `app/chamados/glpi_client.py:1`, `app/chamados/glpi_client.py:126`, `app/chamados/glpi_client.py:161`, `app/chamados/glpi_client.py:166`.

**2. Escritores de `materializada_em`**

O campo da pergunta pertence a **`PerguntaDisputa`**, ligada ao chamado por `chamado_destino`; há outro campo homônimo em **`RegularizacaoExterna`**. `app/chamados/models.py:1935`, `app/chamados/models.py:2379`, `app/chamados/models.py:2387`, `app/chamados/models.py:2744`, `app/chamados/models.py:2766`.

A enumeração abaixo contém **20 sítios explícitos em 19 funções de 7 arquivos**: 13 carimbos de pergunta, 1 carimbo de regularização e 6 limpezas de carimbo. Os dois ramos de `resolver_perguntas_por_chamado_resolvido` são contados separadamente. As referências de escrita estão em cada linha da tabela.

| Escritor / escrita | O que carimba ou limpa; porta consultada; quem chama |
|---|---|
| `PerguntaDisputa.fechar_por_admin` | `admin_decidiu`, instante e autor; sem C6 dentro do método. Chamado pela decisão do dia, validação de folga e fechamento administrativo/desligamento da disputa. C7 calcula vivas, mas a decisão administrativa pode encerrá-las. `app/chamados/models.py:2213`, `app/chamados/models.py:2228`, `app/chamados/models.py:1556`, `app/chamados/models.py:1595`, `app/ponto/services/veredito_celula.py:177`, `app/chamados/services/validacao.py:292` |
| `PerguntaDisputa.fechar_por_competencia_trancada` | `competencia_trancada`, instante e autor; sem C6. Sem chamador produtivo direto localizado **[nao verificado quanto a chamada dinâmica]**; o serviço atual manda não carimbar perguntas. `app/chamados/models.py:2233`, `app/chamados/models.py:2242`, `app/chamados/services/competencia_trancada.py:171`, `bin/esmeril_chamado_censo.py:135` |
| `PerguntaDisputa.encerrar_fantasma` | `fantasma`, instante e autor. Método não julga; consumidor exige C6 viva, sem resposta, marco aceso e dia encerrado antes de chamar. `app/chamados/models.py:2258`, `app/chamados/models.py:2267`, `app/chamados/reconciliador.py:1033`, `app/chamados/reconciliador.py:1039`, `app/chamados/reconciliador.py:1087`, `app/chamados/reconciliador.py:1095` |
| `lavrar_sem_turno_aberto` | `sem_turno_aberto`; sem texto/carimbo/turno aberto e **C6 morta**. Reatador, evento de batida e cron. `app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:399`, `app/chamados/reconciliador.py:402`, `app/chamados/reconciliador.py:1519` |
| `retratar_perguntas_absolvidas_por_celula` | `celula_absolveu`; célula julgada não acusadora, sem marcos pendentes e **C6 morta**. Reconciliação por colaborador: evento e cron. `app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:607`, `app/chamados/reconciliador.py:613`, `app/chamados/reconciliador.py:617`, `app/chamados/reconciliador.py:1350` |
| `_materializar_ausencia_de_resposta` | Absorve perguntas-hora do dia sem carimbo; via `declaracao` ou `ausencia_dia`. Não usa C6 como guarda prévia. Resposta web/API nos escapes `FALTA_DIA/ATESTADO`. `app/chamados/services/materializacao.py:478`, `app/chamados/services/materializacao.py:499`, `app/chamados/services/materializacao.py:509`, `app/chamados/services/materializacao.py:525`, `app/chamados/services/disputa_emissao.py:1292` |
| `_materializar_resposta_hora`, recusa | `paridade_recusada` somente com `fato_do_marco_no_espelho is True`; outra porta de fato, sem C6. Unidade materializadora, validação e lote. `app/chamados/services/materializacao.py:815`, `app/chamados/services/materializacao.py:840`, `app/chamados/services/materializacao.py:858`, `app/chamados/services/materializacao.py:1125` |
| `_carimbar_recusa` | `fato_ocupado` para recusa classificada `COLISAO`, se carimbo nulo; sem C6. Materializador e lote do passivo. `app/chamados/services/materializacao.py:1028`, `app/chamados/services/materializacao.py:1037`, `app/chamados/services/materiali

[truncado em 20000 de 60396 caracteres]

## 30/10 18:22 · sessao d7e74ac23a0e · CODEX-CONTEXTO-DA-CASA-AB2-CORRECAO · censo · origem=Code

A classificação considera a cópia atual. C1, C4, C14, C17 e C19 estão corrigidos: o template lê o rótulo, o teste limita o delta cego, a renderização usa sentinela, a singular não repassa o mapa aos marcos e o teste agora chama a singular alimentada. (`app/templates/colaboradores/partials/_calendario_grade.html:107`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:183`, `:110`; `app/ponto/turnos.py:2080`; `app/ponto/tests/test_c1_dias_encerrados.py:74`)

O1 recebe `n/a` quanto ao ponto indicado do código: a expressão apontada não está na linha 720. A mensagem do commit 5cc57c18 permanece `[não verificado]`. (`app/colaboradores/services/calendario.py:720`)

C10 descreve o teste isolado: o teste seguinte rejeita a mutação global “ontem sempre previsto”. C11 permanece pela ausência de cenário de feriado; já existe cenário de ausência. C26 confirma a alimentação parcial e o julgamento do intervalo; consequência incorreta em produção é `[não verificado]`. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:57`, `:63`, `:133`; `app/ponto/services/cartorio.py:746`, `:853`; `app/ponto/turnos.py:2136`)

Os trechos abaixo sustentam as notas positivas ou parciais. Listar um cenário de teste, sem identificar sua lacuna, foi considerado `EM_PARTE`. [nao verificado]

| Volta/resposta | Itens | Trecho da resposta e referência |
|---|---|---|
| par1/A | O2, C2 | “A régua suspeita também esconde o motivo recebido”; descreve o cruzamento sem fim, não O2 com fim previsto. (`app/colaboradores/services/calendario.py:139`, `:141`, `:722`) |
| par1/A | O3 | “A folha mantém seu próprio critério de dia passado”; toca a separação temporal sem apontar a duplicação como defeito. (`app/colaboradores/services/calendario.py:723`; `app/folha/export.py:1237`) |
| par1/A | O4 | “uma consulta adicional incondicional”; “subtrai uma base que já contém a consulta adicional fixa”. (`app/ponto/turnos.py:2130`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`) |
| par1/A | O5 | “esse instante segue à juíza e essa data segue ao retentor”; não identifica `_agora_retr`. (`app/colaboradores/services/calendario.py:258`, `:712`, `:726`) |
| par1/A | C3 | “a exceção do dia da demissão”; não aponta a divergência contratual da tela. (`app/folha/export.py:1211`, `:1237`) |
| par1/A | C5 | “O RED cego verifica a premissa na juíza e na folha”; não identifica a fabricação local que passaria. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:97`) |
| par1/A | C6, C22 | “A propriedade C1 com células alimentadas não contém batidas para provar o pareamento com essa alimentação”; não discute alimentação divergente do banco. (`app/ponto/tests/test_c1_dias_encerrados.py:62`, `:66`) |
| par1/A | C7–C13 | “meia-noite, turno encadeado anterior a ontem, falta depois do fim, veredito lavrado, folga, ausência e isenção”; enumera os temas, sem apontar essas lacunas. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47`, `:57`, `:61`, `:65`, `:113`, `:124`, `:147`) |
| par1/A | C20 | “encaminha células com chave `(colaborador_id, data)` a `turnos_do_colab`, que exige chave `data` e a embrulha novamente”. (`app/ponto/turnos.py:2132`, `:1928`) |
| par1/A | C23 | “o template é renderizado com sentinela em `status_rotulo`”; não questiona comentário ou conteúdo oculto. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:108`, `:110`) |
| par1/A | C24 | “a mudança repercute além da cor da tela”; identifica os leitores, sem apontar o desaparecimento do botão. (`app/colaboradores/services/calendario.py:204`; `app/templates/colaboradores/partials/_calendario_grade.html:125`) |
| par1/B | O2 | “a resposta da folha ainda pode desaparecer na classificação”; o caso descrito é sem fim e sem vínculo ativo. (`app/colaboradores/services/calendario.py:134`, `:158`, `:722`) |
| par1/B | O3 | “não classificaria essa distinção, isoladamente, como juiz novo ou teto indevido por data”. (`app/colaboradores/services/calendario.py:714`, `:723`) |
| par1/B | O4 | “sempre um SELECT de vínculos”; “esse custo se repete por calendário montado”. (`app/ponto/turnos.py:2130`; `app/relatorios/furos.py:184`) |
| par1/B | O5 | “o prazo de desfazer captura outro `now()`”. (`app/colaboradores/services/calendario.py:343`) |
| par1/B | C2 | “A correção precisa preservar A2”; não identifica a régua suspeita substituindo o motivo. (`app/colaboradores/services/calendario.py:124`, `:139`, `:141`) |
| par1/B | C5 | “A sentinela prova que o template lê o campo; não prova que o rótulo veio da autoridade da folha”; aponta outra limitação da dependência. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:108`) |
| par1/B | C6, C22 | “C1 possui batidas, mas suas chamadas não exercitam esse argumento”; não distingue a fixture alimentada, sem batidas, nem o conteúdo persistido. (`app/ponto/tests/test_c1_dias_encerrados.py:28`, `:62`, `:66`) |
| par1/B | C7–C13 | “antes do turno, meia-noite, turno encadeado além de ontem, depois do fim, veredito lavrado”; também enumera “folga, ausência e isenção”, sem essas lacunas. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47`, `:57`, `:61`, `:65`, `:113`, `:124`, `:147`) |
| par1/B | C18 | “O teste de cobertura também admite que A2 decide antes da cobertura que ele aparenta verificar”. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:134`, `:138`) |
| par1/B | C23 | “A sentinela prova que o template lê o campo”; não verifica a localização no HTML. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:110`) |
| par1/B | C24 | “o vigia que consulta ‘Resolver dia’”; identifica o alcance, sem apontar a retirada da oferta. (`app/chamados/services/vigia_fio.py:294`; `app/colaboradores/services/calendario.py:204`) |
| par2/A | O2, C2 | “se houver régua suspeita, `_classificar` retorna `escala_suspeita` antes de ler `retido_folha`”; trata o caso sem fim. (`app/colaboradores/services/calendario.py:139`, `:141`, `:722`) |
| par2/A | O3 | “Calendário monta `_em_curso` e `_sem_fim`”; rastreia o filtro, sem identificar sua duplicação na folha. (`app/colaboradores/services/calendario.py:714`, `:722`; `app/folha/export.py:1237`) |
| par2/A | O4 | “Acréscimo estrutural comum: uma consulta de vínculos por montagem”; “o custo comum cancela na subtração”. (`app/ponto/turnos.py:2130`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`) |
| par2/A | O5 | “desfazer retratação captura outro instante”. (`app/colaboradores/services/calendario.py:343`) |
| par2/A | C6, C22 | “o C1 alimentado testa DNA, sem geradora concorrente”; identifica a lacuna da geradora, não a alimentação igual ao banco. (`app/ponto/tests/test_c1_dias_encerrados.py:62`, `:64`) |
| par2/A | C8 | “O teste novo preserva apenas `nunca_bateu`; falta o adversarial `concorde`”. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:65`, `:74`) |
| par2/A | C7, C9–C13 | “meia-noite, turno até dois dias depois, falta após encerramento”; também cita testes de veredito, folga, ausência e isenção, sem apontar essas lacunas. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47`, `:57`, `:61`, `:113`, `:124`, `:133`, `:147`) |
| par2/A | C20 | “A chave fica aninhada; a busca da geradora deixa de encontrar a célula”. (`app/ponto/turnos.py:2132`, `:1928`; `app/escala/alimentacao.py:237`) |
| par2/A | C23 | “O render usa sentinela para exigir leitura efetiva de `status_rotulo`”; não aponta a aceitação de conteúdo oculto. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:108`, `:110`) |
| par2/A | C24 | “Resolver dia”; “Template e vigia do fio”; rastreia o leitor sem apontar o desaparecimento da oferta. (`app/colaboradores/services/calendario.py:204`; `app/templates/colaboradores/partials/_calendario_grade.html:125`) |
| par2/B | O2, O3, C2 | “O passado `sem_previsto` sai de `_em_curso`”; “preservando a precedência humana e o veredito”; não aponta O2 com fim nem a duplicação temporal como defeito. (`app/colaboradores/services/calendario.py:714`, `:723`, `:124`, `:141`) |
| par2/B | O4 | “esconde a consulta adicional incondicional de vínculos”. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`; `app/ponto/turnos.py:2130`) |
| par2/B | O5 | “O segundo `now()` do calendário serve ao prazo de desfazer retratação”. (`app/colaboradores/services/calendario.py:343`) |
| par2/B | C5 | “substituir a consulta por `_cego_folha = {d: 'sem_celula' for d in _sem_fim}` preservaria as expectativas”. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:97`, `:177`; `app/colaboradores/services/calendario.py:722`) |
| par2/B | C6, C22 | “Não cobrem [...] o caminho `turnos=` com batidas nessa nova prova C1”; toca a prova alimentada, sem identificar alimentação igual ao banco ou perda da geradora. (`app/ponto/tests/test_c1_dias_encerrados.py:62`, `:66`) |
| par2/B | C7–C13 | “antes do turno, madrugada, depois do fim, turno encadeado além de ontem, decisão lavrada”; também cita “folga, ausência e isenção”, sem essas lacunas. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47`, `:57`, `:61`, `:65`, `:113`, `:124`, `:147`) |
| par2/B | C18 | “os casos de veredito, folga, ausência e isenção escapam por outros ramos”; não identifica especificamente a prova do universo da cobrança. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:133`, `:138`) |
| par2/B | C20 | “criando chaves aninhadas no adaptador”; “o cartório o utiliza”. (`app/ponto/turnos.py:2132`, `:1928`; `app/ponto/services/cartorio.py:853`) |
| par2/B | C23 | “consumo do campo pelo template mediante sentinela”; não aponta a localização não verificada. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:110`) |
| par2/B | C24 | “a mudança de status também alcança a oferta de ação e sua contagem”; não identifica o desaparecimento em competência anterior. (`app/colaboradores/services/calendario.py:204`, `:914`) |

ITEM|C1|app/templates/colaboradores/partials/_calendario_grade.html:107|Grade não apresenta o rótulo do dia cego produzido pelo calendário.
ITEM|C2|app/colaboradores/services/calendario.py:124|Precedências de ausência, folga, feriado ou régua suspeita podem substituir sem_celula retornado pela folha.
ITEM|C3|app/folha/export.py:1180|Calendário e folha divergem no recorte de admissão/demissão, inclusive no próprio dia da demissão.
ITEM|C4|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177|Prova relativa de custo não limita o acréscimo constante exclusivo do caminho cego.
ITEM|C5|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:97|Teste aceita fabricar sem_certidao/sem_celula localmente sem depender da resposta da folha. [nao verificado]
ITEM|C6|app/ponto/tests/test_c1_dias_encerrados.py:53|Propriedade C1 não prova equivalência quando a alimentação diverge do conteúdo persistido.
ITEM|C7|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:61|Teste depois do fim não cobre a igualdade agora == fim. [nao verificado]
ITEM|C8|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:65|Proteção de veredito testa somente nunca_bateu e deixa outros vereditos sem cobertura.
ITEM|C9|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47|Teste encadeado não impede manter previsto depois do encerramento desse DNA. [nao verificado]
ITEM|C10|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:57|Teste de meia-noite isolado aceita previsto incondicionalmente para ontem. [nao verificado]
ITEM|C11|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:124|Teste da folga não sela as precedências de ausência e feriado no caminho cego. [nao verificado]
ITEM|C12|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:147|Teste do isento não sela a isenção na folha nem o rótulo apresentado. [nao verificado]
ITEM|C13|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:113|Teste com veredito deixa passar consulta desnecessária à folha preservando o resultado. [nao verificado]
ITEM|C14|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:104|Teste de renderização aceita frase literal em lugar da leitura de status_rotulo. [nao verificado]
ITEM|C15|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:104|Teste de renderização não exercita os demais status do ramo residual. [nao verificado]
ITEM|C16|app/colaboradores/services/calendario.py:57|Rótulo de ausencia promete abono próprio mesmo para ausência de efeito SUPRIME.
ITEM|C17|app/ponto/turnos.py:2079|Repasse singular do mapa por data do fechamento aos marcos por tupla perde o DNA e usa o template.
ITEM|C18|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:133|Teste da ausência pendente não sela o universo da cobrança porque A2 decide antes. [nao verificado]
ITEM|C19|app/ponto/tests/test_c1_dias_encerrados.py:70|Teste C1 não exercita a singular alimentada com mapa por data para proteger a correção do fechamento. [nao verificado]
ITEM|C20|app/ponto/turnos.py:2132|Plural repassa mapa por tupla ao pareador por data, produzindo chave aninhada e perdendo a geradora.
ITEM|C21|app/ponto/services/vigia_de_hora.py:57|Vigia entrega mapa por tupla à singular que o repassa ao pareador por data.
ITEM|C22|app/ponto/tests/test_c1_dias_encerrados.py:62|Fixture C1 alimentada não tem geradora nem batidas para detectar perda da geradora no pareamento.
ITEM|C23|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:110|Asserção textual de renderização não verifica se o atributo está em elemento apresentado, permitindo comentário ou conteúdo oculto.
ITEM|C24|app/colaboradores/services/calendario.py:204|Mudança de falta para previsto pode retirar Resolver dia de passado ainda em curso da competência anterior.
ITEM|C25|app/colaboradores/services/calendario.py:624|Falha capturada na carga A2 deixa cobertura independente alcançar retenção sem a precedência correspondente.
ITEM|C26|app/ponto/services/cartorio.py:853|Segunda passada alimenta somente células reconciliadas, embora dias_encerrados julgue todo o intervalo entre extremos.
NOTA|CALENDARIO-HOJE_38fe9889_par1|O1|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par1|O2|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|O3|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|O4|existia|A=VIU|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|O5|existia|A=EM_PARTE|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|O6|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|O7|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C1|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par1|C2|existia|A=VIU|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C3|existia|A=EM_PARTE|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C4|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par1|C5|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C6|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C7|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C8|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C9|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C10|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C11|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C12|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C13|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C14|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par1|C15|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C16|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C17|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par1|C18|existia|A=NAO_VIU|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C19|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par1|C20|existia|A=VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C21|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C22|existia|A=VIU|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C23|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C24|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C25|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C26|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|O1|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par2|O2|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|O3|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|O4|existia|A=VIU|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|O5|existia|A=VIU|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|O6|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|O7|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C1|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par2|C2|existia|A=VIU|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C4|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par2|C5|existia|A=NAO_VIU|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C6|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C7|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C8|existia|A=VIU|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C9|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C10|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C11|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C12|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C13|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C14|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par2|C15|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C16|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C17|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par2|C18|existia|A=NAO_VIU|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C19|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par2|C20|existia|A=VIU|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C21|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C22|existia|A=VIU|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C23|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C24|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C25|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C26|existia|A=NAO_VIU|B=NAO_VIU
NOVO|CALENDARIO-HOJE_38fe9889_par1|A|app/colaboradores/services/calendario.py:351|Sem vínculo ativo, vínculo histórico válido e célula de trabalho não impedem descartar sem_celula e publicar sem_dados.
NOVO|CALENDARIO-HOJE_38fe9889_par1|A|app/colaboradores/legenda_calendario.py:18|Legenda do copiloto ainda define previsto exclusivamente como dia futuro; teste verifica nomes, não explicações. [nao verificado]
NOVO|CALENDARIO-HOJE_38fe9889_par1|A|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:19|Novos testes não exercitam a alimentação turnos= com lista não vazia. [nao verificado]
NOVO|CALENDARIO-HOJE_38fe9889_par1|B|app/colaboradores/services/calendario.py:351|Sem vínculo ativo, calendário perde a retenção de dia coberto por vínculo histórico e publica sem_dados.
NOVO|CALENDARIO-HOJE_38fe9889_par1|B|app/colaboradores/services/calendario.py:64|Calendário escreve localmente a frase do dia cego, em vez de consumir rótulo produzido pela autoridade da folha.
NOVO|CALENDARIO-HOJE_38fe9889_par1|B|app/colaboradores/legenda_calendario.py:18|Explicação de previsto entregue ao copiloto ficou restrita ao futuro e seu teste não verifica a descrição. [nao verificado]
NOVO|CALENDARIO-HOJE_38fe9889_par1|B|app/colaboradores/services/calendario.py:727|Filtro de retenções consulta lista de candidatos por registro e pode acrescentar custo quadrático de CPU.
NOVO|CALENDARIO-HOJE_38fe9889_par1|B|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:20|Fixture sem batidas não cobre a nova entrada turnos= com conteúdo.
NOVO|CALENDARIO-HOJE_38fe9889_par2|A|app/colaboradores/services/calendario.py:822|Veredito concorde sem batida desliga a proteção temporal e pode produzir falta enquanto a juíza responde relogio.
NOVO|CALENDARIO-HOJ

[truncado em 20000 de 20889 caracteres]

## 30/10 18:20 · sessao 856fa586821d · - · - · origem=Ronald (tmux)

Somente inspeção; nenhuma escrita ou execução de suíte. [nao verificado]

| Item | Estado atual | Evidência |
|---|---|---|
| C1 | n/a | Grade publica `title="{{ dia.status_rotulo }}"` no ramo residual: `app/templates/colaboradores/partials/_calendario_grade.html:107`. |
| C4 | n/a | Além da igualdade entre 1/8 cegos, há teto `self.assertLessEqual(um, 2)`: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`, `:179`, `:183`. A limitação relativa ao custo comum continua, mas é outro defeito. |
| C5 | existia | Teste consulta folha separadamente e confere resultado final; não intercepta nem diferencia a dependência interna da montagem: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:95`, `:97`, `:99`. Uma fabricação local do mesmo código não é distinguida pelas asserções. |
| C7 | existia | Depois do fim está às 11h locais, sendo o fim às 06h; não testa igualdade: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:61`. |
| C8 | existia | Caso de veredito usa somente `nunca_bateu`: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:74`; o caso sem fim julgado também: `:117`. |
| C9 | existia | DNA encadeado é testado somente às 02h, quatro horas antes do encerramento; única asserção exige `previsto`: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47`, `:54`, `:55`. |
| C10 | existia, como limitação do teste isolado | Teste meia-noite só exige `previsto` às 02h: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:57`, `:59`. A mutação global “ontem sempre previsto” seria detectada pelo teste seguinte, que exige falta às 11h: `:61`, `:63`. |
| C11 | existia parcialmente | Folga testa apenas `FolgaDia`: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:128`. Agora há teste específico de ausência pendente em `:133-144`; não há cenário de feriado nesse módulo. |
| C12 | existia | Isento altera flag e exige somente status `ok`; não consulta folha nem verifica rótulo: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:150`, `:153`. |
| C13 | existia | Teste julgado verifica retenção independente/status/rótulo, mas não quantidade ou ausência de chamadas internas à folha: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:117-121`. |
| C14 | n/a | Render exige valor normal em atributo e repete com sentinela diferente: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:106`, `:108`, `:110`. A frase literal antiga não satisfaz a sentinela. |
| C15 | existia | Renderização dos três layouts ocorre somente no teste cujo status é `sem_certidao`: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:99`, `:104-110`; os demais status residuais não aparecem nessa prova. |
| C18 | existia | O próprio comentário registra que cobrança restrita a aprovadas passa porque A2 decide antes: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:137-138`; execução só exige `ausencia`: `:144`. |
| C23 | existia | `assertIn('title="SENTINELA-DO-ROTULO"', html)` não analisa DOM, comentário ou visibilidade: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:110`; idem valor normal em `:106`. |

## 30/10 18:20 · sessao 196d4b4dc3d6 · - · - · origem=Ronald (tmux)

- **O1: não confirmado no sítio indicado.** `app/colaboradores/services/calendario.py:720` contém “UMA pergunta por montagem… (custo 0 no caso comum)”, sem “fallback” nem equivalente claro. Há “band-aid” em `app/colaboradores/services/calendario.py:85`, mas descreve outro mecanismo já removido. Mensagem do commit `5cc57c18`: **[não verificado]**; a cópia não possui `.git`. Não transplantaria O1 para `:85`.

- **O2: existia.** A consulta à folha seleciona apenas `MOTIVO_SEM_PREVISTO` (`app/colaboradores/services/calendario.py:722–727`). Passado com fim previsto já encerrado fica fora dessa consulta (`app/ponto/turnos.py:2102–2105`); sem batida, sem cobertura e sem outra precedência, vira `falta` (`app/colaboradores/services/calendario.py:148`). A folha retém célula sem veredito, com `trabalha != None`, passada e fora do dia da demissão, independentemente do fim previsto (`app/folha/export.py:1237–1238`).

- **O3: existia.** O calendário aplica `d < hoje` em `app/colaboradores/services/calendario.py:723`; a folha repete esse predicado em `app/folha/export.py:1213` e `:1237`.

- **O4: existia estruturalmente.** Há carregamento incondicional dos vínculos em `app/ponto/turnos.py:2130`, mesmo recebendo células e turnos. O mapa chama o contexto por cartão em `app/relatorios/furos.py:184`; o comentário informa frota de aproximadamente 750 colaboradores em `:176`. Portanto, uma consulta extra por montagem; aproximadamente 750 extras somente se houver esse número de cartões. Contagem efetiva e tempo em produção: **[não verificado]**.

- **O5: existia.** Captura `_agora = timezone.now()` em `app/colaboradores/services/calendario.py:258` e captura novamente `_agora_retr = timezone.now()` em `:343`.

- **O6: existia como lacuna estática.** O ramo inclui hoje com qualquer motivo não nulo, inclusive `sem_previsto` (`app/colaboradores/services/calendario.py:714–715`), resultando em `previsto` nas premissas correspondentes (`:822`, `:135–136`). Os testes de hoje usam jornada com fim (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42–45`); os testes sem fim usam passado (`:91–100`, `:112–153`). Busca nos testes do calendário não encontrou combinação hoje + sem fim. Execução: **[não verificado]**.

- **O7: existia, nos dois componentes.** `resumo['faltas']` conta exclusivamente `falta` e o resumo não possui contador de `sem_certidao` (`app/colaboradores/services/calendario.py:948–965`). Os mapas de cores do PDF não incluem `sem_certidao` (`app/relatorios/furos_pdf.py:11–20`); o estado recebe cores padrão (`:21`, `:56–57`).

## 30/10 18:20 · sessao c1cb0a2b1b40 · - · - · origem=Ronald (tmux)

- **C6: existia.** A fixture persiste o DNA em `app/ponto/tests/test_c1_dias_encerrados.py:62`, carrega esse mesmo registro em `:64` e compara as portas em `:66`, `:70` e `:74`; não há alimentação divergente do banco. A leitura sem alimentação busca o DNA persistido em `app/escala/models.py:1097`.
- **C17: n/a.** A singular passa `celulas` somente ao pareador; a chamada ao puro não recebe esse argumento em `app/ponto/turnos.py:2079` e `:2080`. O mapa por data do fechamento continua corretamente dirigido à singular em `app/ponto/services/fechamento.py:359` e `:361`.
- **C19: n/a — correção adicional presente nesta cópia.** O teste converte o mapa composto para `por_data` em `app/ponto/tests/test_c1_dias_encerrados.py:73` e chama expressamente a singular alimentada em `:74`. Portanto, não repetir a conclusão do anexo verif8 sobre essa lacuna.
- **C20: existia.** Contrato composto da plural em `app/ponto/turnos.py:2113`; repasse direto ao pareador em `:2132`; contrato por data do pareador em `:1852`; embrulho adicional em `:1928`; busca da geradora por chave composta em `app/escala/alimentacao.py:237`. O cartório fornece o mapa composto sem `turnos=` em `app/ponto/services/cartorio.py:853`. A dívida também está registrada em `app/docs/BACKLOG.md:790`.
- **C21: existia.** Vigia fornece mapa composto em `app/ponto/services/vigia_de_hora.py:57`; singular repassa esse mapa ao pareador em `app/ponto/turnos.py:2079`; o pareador espera data em `:1852` e embrulha as chaves em `:1928`. Incidência e efeito concreto em produção: **[não verificado]**.
- **C22: existia.** Fixture alimentada cria célula sem `escala_geradora` e sem batidas em `app/ponto/tests/test_c1_dias_encerrados.py:62`; sua execução apenas carrega células e consulta as portas em `:64`, `:66`, `:70` e `:74`. O `setUpTestData` reutilizado cria colaboradores/vínculos, sem batidas, em `app/ponto/tests/test_juiz_dia_encerrado.py:44` e `:69`.
- **C26: existia como estrutura descrita.** Segunda passada recebe somente `out.pop('rejulgar')` em `app/ponto/services/cartorio.py:745` e `:746`; extremos vêm das células recebidas em `:830`; mapa alimentado contém somente essas células em `:853`; plural julga todo o intervalo em `app/ponto/turnos.py:2136` a `:2140`. Que isso produza resultado incorreto para alguma resposta efetivamente consumida pelo cartório: **[não verificado]**.

## 30/10 18:18 · sessao 3bb24a8e863c · - · - · origem=Ronald (tmux)

Censo dos leitores na árvore entregue, sem executar Django/banco e sem escrever arquivos. Método: parti das definições dos campos/conjuntos, percorri referências Python por AST (`Attribute/Load`, `Compare`, argumentos ORM/Q, strings de `getattr`, agregações e `values_list`), li os corpos e distingui comparação terminal direta, exclusão por `VIVOS`, leitura de carimbo, propriedade/juiz e mera apresentação. Os campos/conjuntos de partida estão em `app/chamados/models.py:46`, `app/chamados/models.py:2387`, `app/chamados/models.py:2766`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`. A execução efetiva, chamadas dinâmicas/reflexivas não literais, SQL externo, shell de produção, views/triggers de banco e código fora desta árvore ficam `[nao verificado]`.

**Achado que importa para o desenho:** `chamado_que_lista` não lê status terminal nem `materializada_em`: retorna `p.chamado_destino_id` ou, faltando, `disputa.chamado_id`; portanto não é a regra do crédito da fila, que prefere destino **vivo** e cai no pai quando destino está morto. `app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:116`, `app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:60`, `app/chamados/services/raiox.py:133`, `app/chamados/services/raiox.py:140`.

### Leitores estruturais, fila, cartório e folha

| Leitor | O que decide |
|---|---|
| `fio.montar_chamados_fio` | Em disputa fechada usa `q_vivas` e confirma `pergunta_viva`; ancora por `chamado_que_lista`; `tem_pergunta_viva` exige `p.viva` **e** ação possível; passa só `VIVOS` ao crédito; entrega estado ao desenlace; vivos viram contador de ativos. `app/chamados/services/fio.py:220`, `:234`, `:236`, `:279`, `:385`, `:399`, `:428`. |
| `raiox.creditos_perguntas` | Busca perguntas apontando a destino ou pai vivos, usa `q_vivas`, exclui carimbadas com lavra `VL_SIM`, separa respondida/não respondida e credita destino vivo, senão pai. `app/chamados/services/raiox.py:54`, `:72`, `:133`, `:137`, `:140`. |
| `raiox.pergunta_sem_acao_possivel` | Sem carimbo → há ação; com carimbo lê `veredito_lavrado`, ou juiz de materialização de fato quando lavra vazia; exceção mantém na mesa. `app/chamados/services/raiox.py:173`, `:182`, `:184`, `:186`. |
| `painel.qs_vivos` / fila | Só `VIVOS`; a cobrança usa `COBRANCA`; histórico usa `ENCERRADOS`; conta resolvidos, fechados, cancelados; filtros histórico/cancelados/status selecionam conjunto exibido. `app/chamados/services/painel.py:155`, `:187`, `:202`, `:215`, `:216`, `:222`, `:243`, `:301`, `:303`, `:305`, `:314`. |
| `painel.marcador_do_placar` | `Max(materializada_em)` participa do marcador/ETag de alteração, não prova de plantio. `app/chamados/services/painel.py:62`, `:67`, `:68`. |
| `desenlace` | Fora de `VIVOS` devolve `None`, sem ação a fazer; vivos recebem verbo/dono/ação. `app/chamados/services/desenlace.py:44`, `:48`. |
| `verbos.rotulo_de_linha` / `rotulo_para_pessoa` | Traduz estado para “Registro”/tooltip do admin ou “Encerrado” do colaborador (todos quatro terminais); `placar_equipe` conta encerrados com data recente. `app/chamados/services/verbos.py:97`, `:111`, `:122`, `:135`, `:227`; filtro de template em `app/chamados/templatetags/chamados_tags.py:54`. |
| `cartorio.impressao_insumos` | Hash inclui `pk:status_local`; mudança de estado altera insumo do julgamento. `app/ponto/services/cartorio.py:314`, `:326`. |
| `cartorio._julgar_colab_corpo` | Separa chamados vivos (`status in VIVOS`) dos que têm `fio_vivo` nas perguntas resolvedoras: vivo é insumo de emissão, fio é insumo da cobrança. Relê estado da instância antes de ressuscitar perguntas; todos duplicata → tenta reconciliar. `app/ponto/services/cartorio.py:1069`, `:1074`, `:1258`, `:1261`, `:1265`. |
| `furos_sem_cobranca.medir` | Dia com qualquer chamado vivo fica coberto; superador vivo → `coberto_por_quem_superou`; superador fechado por admin → `decidido_pelo_admin`; demais passam a vetos/canal/cobrança. Usa data JSON do chamado nesta leitura. `app/ponto/services/furos_sem_cobranca.py:67`, `:69`, `:74`, `:76`, `:78`, `:83`, `:96`, `:100`, `:103`, `:107`. |
| `furos_sem_cobranca.cobrar` | Após rejulgar cartório, verifica se existe chamado vivo no dia; conta cobrado/sem cobrança depois. `app/ponto/services/furos_sem_cobranca.py:128`, `:129`, `:131`. |
| `vigia_fio.censo` | Nasce de chamados `VIVOS` com célula; restringe vocabulário a `FURO_MODULES`; distingue os que cobram (`COBRANCA`); lê lâmpada e perguntas por célula via `fio_vivo`; depois reconhece decisão de dia, ausência ou ação do admin para tirar do fio morto. `app/chamados/services/vigia_fio.py:186`, `:189`, `:201`, `:202`, `:229`, `:270`, `:286`, `:294`. |
| `vigia_fio.fio_do_dia` | Delega redução a `juizes.fio_vivo`; não lê carimbo diretamente. `app/chamados/services/vigia_fio.py:155`, `:164`. |
| `folha.fora_da_folha._alimentar` / `_fio_do_dia` | Carrega chamados só `VIVOS`; lê `p.viva` e `p.estado`: pendente → colab; outra viva → admin; sem viva e com chamado → chamado; sem ambos → nada. Classifica grupo A/B/C/D, não decide entrada no TXT, que vem de `classificar_export`. `app/folha/fora_da_folha.py:54`, `:66`, `:67`, `:69`, `:71`, `:76`, `:120`. |
| `folha.o_que_segura` | Lista IDs de chamados vivos para instrução “destrava”; associa fio a cada dia retido. `app/folha/fora_da_folha.py:198`, `:200`, `:204`, `:207`, `:209`. |
| `folha.previa` | Consome `fio_vivo` para material da prévia. `app/folha/previa.py:141`, `:144`. |
| `vigia_loop.trocas_de_estado_em_loop` | Lê estados antigos/novos do log; identifica fechar→reabrir sem fato novo. `app/chamados/services/vigia_loop.py:34`, `:45`, `:50`. |
| `vigia_loop.trilha_x_banco` | Compara último status lavrado no log com `status_local` atual do banco. `app/chamados/services/vigia_loop.py:105`, `:117`, `:120`. |

### Leitores diretos de estado terminal em fluxo operacional

Além do núcleo do modelo/juiz que o agente de escritores já cobre:

- `becos._resolv`: alias `RESOLVIDO=TERMINAIS`, via `getattr(status_local)`; `classificar` só chama beco quando pai é terminal e não restam perguntas sem resposta com destino não terminal; `d.pode_fechar` decide fechável/barrada. `app/chamados/services/becos.py:13`, `:21`, `:25`, `:29`, `:31`, `:33`.
- `acoes_chamado`: os gates de aprovar exceção, responder agenda/escala, autorizar/recusar geofence recusam chamado `ENCERRADOS`. `app/chamados/services/acoes_chamado.py:25`, `:90`, `:132`, `:176`, `:252`.
- `acoes_disputa.acao_reabrir_disputa_admin`: pai encerrado provoca reabertura. `app/chamados/services/acoes_disputa.py:265`.
- `disputa_emissao.motivo_fora_da_fabrica`: `ENCERRADOS` → `chamado_ja_encerrado`; pergunta carimbada só tira da fábrica quando célula ausente ou não acusadora. `app/chamados/services/disputa_emissao.py:691`, `:695`, `:703`, `:705`.
- `criar_disputa_ancora_de_chamado`: limbo encerrado → não cria; pergunta de âncora sem carimbo existente → não duplica. `app/chamados/services/disputa_emissao.py:1109`, `:1114`.
- `resolver_perguntas_por_chamado_resolvido`: pai/destinos vivos protegem crédito; sem carimbo+sem validação delimita perguntas a arrastar. `app/chamados/services/disputa_emissao.py:1602`, `:1628`, `:1631`, `:1635`.
- `_reconciliar_destino_e_clones`: só tenta reconciliar destino/clones fora de `ENCERRADOS`, com retenção das irmãs respondidas sem consumo. `app/chamados/services/materializacao.py:702`, `:713`, `:719`.
- `adesao.sincronizar_adesao`: pai `ENCERRADOS` pode reabrir, exceto via cascade; filhos `TERMINAIS` ficam fora do agrupamento. `app/chamados/services/adesao.py:89`, `:90`, `:102`.
- `resposta_tardia`: vivo segue fluxo normal; fora de vivos tenta renascer por célula acusando, senão grava resposta tardia/pauta e apresenta estado encerrado. `app/chamados/services/resposta_tardia.py:35`, `:45`, `:52`, `:65`.
- `signals`: disputa fechada não resolve pai já encerrado; baixa urgência e resolve perguntas só em `ENCERRADOS`; cartório reage a mudança real do status, não save neutro. `app/chamados/signals.py:33`, `:70`, `:81`, `:161`, `:169`.
- Emissores reabrem encerrado: `reconciliar_grade._emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:521`), `detectar_ausencias` âncora (`app/ponto/management/commands/detectar_ausencias.py:300`), emissão da ausência/furo (`app/ponto/management/commands/detectar_ausencias.py:734`, `:739`).
- `lastro.fechar` relê estado do banco e pula quem saiu de `VIVOS` antes de escrever/registrar nota. `app/ponto/services/lastro.py:310`, `:311`.
- `faltas_de_hoje_sem_cobranca` exclui **somente cancelado**, portanto outros terminais ainda contam como “tem chamado do dia”. `app/ponto/services/faltas_de_hoje.py:63`, `:64`.

### Todos os leitores diretos de `materializada_em`, agrupados por regra

As leituras dentro dos próprios escritores abaixo são guardas/idempotência/elegibilidade; não devem desaparecer do censo por também serem escritores.

| Arquivo/função | Decisão do carimbo |
|---|---|
| `models.PerguntaDisputa.estado` | Validada+carimbada → RESOLVIDA; viva → PENDENTE/RESPONDIDA; morta+carimbada → RESOLVIDA. `app/chamados/models.py:2204`, `:2206`, `:2207`, `:2209`. |
| `models.q_vivas` | Carimbo+via identidade/tranca são termos do pré-filtro generoso SQL. Exatidão continua no juiz. `app/chamados/models.py:2330`, `:2344`, `:2346`. |
| `models._universo_da_ressuscitacao` | Só carimbadas sem resposta; marco aceso é separado dos que voltariam. `app/chamados/models.py:1209`, `:1211`, `:1215`. |
| `models.DisputaSupervisao.respostas_sem_veredito` | Só considera resposta sem validação **e sem carimbo**, excluindo dia trancado, para bloquear fechamento da disputa. `app/chamados/models.py:1524`, `:1525`, `:1528`. |
| `models.fechar_por_admin`, `encerrar_fantasma`, `reabrir_fantasma`, `reabrir_por_resposta` | Guardas de idempotência/via/elegibilidade antes de escrever ou limpar. `app/chamados/models.py:2219`, `:2264`, `:2279`, `:2299`. |
| `juizes._retratada_com_trilha` | Sem carimbo não aceita retratação; com ele exige LogAuditoria externo. `app/chamados/juizes.py:275`, `:278`. |
| `juizes._pergunta_materializada_de_fato`, `_pergunta_viva_calc` | Carimbo combinado a via, fato, célula e testemunha, jamais coluna isolada como prova geral. `app/chamados/juizes.py:603`, `:629`, `:1015`, `:1044`, `:1050`, `:1076`, `:1083`. |
| `juizes._chave_memo_pv` | Carimbo integra chave que invalida memo do juiz. `app/chamados/juizes.py:770`. |
| `juizes.pergunta_sem_lastro_ou_viva` | Sem carimbo OU viva pelo juiz → não há lastro protegido. `app/chamados/juizes.py:1094`. |
| `juizes.irmas_respondidas_sem_consumo` | Sem carimbo+sem validação+respondida, creditada ao destino ou pai conforme status vivo, bloqueia reconciliar. `app/chamados/juizes.py:1472`, `:1474`, `:1476`. |
| `views.responder_pergunta_disputa` | Carimbo só protege resposta se juiz disser morta; antes já filtra pela pergunta viva. `app/chamados/views.py:1242`, `:1257`. |
| `disputa_emissao._derivar_chamado_destino` | Pergunta **sem carimbo** do mesmo motivo já apontando ao candidato impede usar esse destino para outra; comentário admite régua de carimbo, não juiz. `app/chamados/services/disputa_emissao.py:206`, `:209`, `:211`. |
| `disputa_emissao.criar_disputa_a_partir_de_chamados_orfaos` | Sem carimbo acha disputa existente para complementar irmãs; depois exclui chamados com pergunta sem carimbo do caminho de nova disputa (exceto caminho de ata). `app/chamados/services/disputa_emissao.py:874`, `:890`, `:894`. |
| `disputa_emissao.motivo_fora_da_fabrica` / âncora | Carimbada existente + célula não acusando → fora; sem carimbo na âncora → dedup. `app/chamados/services/disputa_emissao.py:695`, `:705`, `:1117`. |
| `disputa_emissao.resolver_perguntas_por_chamado_resolvido` | Sem carimbo delimita arrastamento; segundo ramo exige também sem validação. `app/chamados/services/disputa_emissao.py:1538`, `:1635`. |
| `materializacao.estado_aplicacao_pergunta` | Carimbada → “aplicada”, exceto via paridade recusada → “recusada_paridade”. É badge de UI. `app/chamados/services/materializacao.py:417`, `:421`, `:425`. |
| `_materializar_ausencia_de_resposta` | Só absorve perguntas hora do dia ainda sem carimbo. `app/chamados/services/materializacao.py:508`. |
| `perguntas_absorvidas_por` | Agrupa perguntas pelo **mesmo timestamp** de materialização junto de disputa/data, recuperando irmãs absorvidas por ausência; exclui quem tem batida plantada viva. `app/chamados/services/materializacao.py:575`, `:577`, `:579`, `:584`, `:586`. |
| `_carimbar_recusa` | Só carimba recusa COLISAO quando ainda sem carimbo. `app/chamados/services/materializacao.py:1035`, `:1042`. |
| `materializar_perguntas_validadas_da_disputa`, `materializar_pendentes_aprovadas` | Fila da materialização é **sem carimbo** (com demais condições da respectiva porta). `app/chamados/services/materializacao.py:1289`, `:1336`. |
| `validacao.devolver_validacao_recusada` | Carimbada não devolve validação; `validar_pergunta` só roda unidade para hora sem carimbo e decide retorno de erro considerando ausência do carimbo. `app/chamados/services/validacao.py:180`, `:182`, `:415`, `:426`, `:444`. |
| `reconciliador.lavrar_sem_turno_aberto` | Já carimbada → não mexe. `app/chamados/reconciliador.py:367`. |
| `retratar_perguntas_absolvidas_por_celula` | Sem carimbo é elegibilidade do lote. `app/chamados/reconciliador.py:586`. |
| `_orfas_abertas` | Universo: sem carimbo, sem validação, disputa aberta, motivos de órfã e data existente. `app/chamados/reconciliador.py:995`, `:1000`. |
| `perguntas_orfas_reatavies` | Sem carimbo é elegibilidade da reatadura, **não vitalidade**; juiz decide viva/morta depois. `app/chamados/reconciliador.py:1397`, `:1407`. |
| `relavra_via.q_carimbo_desfazivel` | Carimbada, via fora de identidade, opcional disputa aberta → elegível; juiz decide desfazer. `app/chamados/services/relavra_via.py:171`, `:176`, `:179`. |
| `fabrica_da_celula.perguntar` | Seleciona carimbadas do dia/motivo para passar à porta de desfazer antes de perguntar. `app/chamados/services/fabrica_da_celula.py:119`, `:121`. |
| `fio.retratar_duplicata` | Só preenche carimbo se nulo. `app/chamados/services/fio.py:845`. |
| `regularizacao_ext.retomar_regularizacoes_confirmadas` | **Outro modelo**, RegularizacaoExterna: confirmada+sem carimbo é fila de retentativa. `app/chamados/services/regularizacao_ext.py:218`, `:219`. |
| `veredito_lavrado.marcos_do_lote` | Max do carimbo por colaborador invalida lavra velha do juiz. `app/chamados/veredito_lavrado.py:162`, `:164`. |
| `painel.marcador_do_placar` | Max do carimbo como marcador de alteração. `app/chamados/services/painel.py:67`. |
| `historico._eventos_dos_chamados` | `getattr` dinâmico em tupla literal: carimbo vira evento “o fato foi registrado”. `app/chamados/services/historico.py:103`, `:105`, `:110`. |
| `procedencia_batida.procedencia` | Data da validação ou, faltando, materialização informa quando/quem da procedência. `app/ponto/services/procedencia_batida.py:72`, `:74`. |

Leituras de `materializada_em` exclusivamente para foto “antes/depois”, serialização de prova ou reversão: `app/chamados/reconciliador.py:1092`, `:1102`; `app/chamados/services/materializacao.py:623`; `app/chamados/services/relavra_via.py:229`; `app/chamados/services/passivo_validado.py:64`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:87`, `:90`, `:128`, `:141`.

### Auditores/comandos leitores

- `auditar_invariantes_chamados`: I1 terminal sem resolvido_em (`:572`); I2 terminal com pergunta sem carimbo, descontando respondida em disputa aberta (`:587`, `:588`, `:590`); I6 carimbo ausente ou juiz reprova fato (`:537`), universo exporta carimbo (`:469`); I8b/I8c carimbada ainda viva e resolvedora cruzada (`:893`, `:904`); I9 carimbo sem via (`:954`); I16 terminal sem via (`:1067`); I17 superado sem ponte (`:1069`); I18 validação humana sem carimbo e sem motivo de recusa (`:1072`); amostra S6 de 300 carimbadas compara lavra/juiz (`:1153`, `:1156`, `:1162`); I21 respondida sem carimbo/validação com destino e pai fora de vivos, descontando ausência aprovada (`:1568`, `:1570`, `:1572`, `:1573`, `:1575`); C2 pergunta sem carimbo/validação/resposta em célula não acusadora (`:1598`); C3 cobrança viva em concorde (`:1603`); F2 viva sem carimbo em disputa fechada (`:1761`, `:1763`, `:1765`); reabríveis calculados a partir de terminais e motor (`:1331`, `:1335`). Todos em `app/chamados/management/commands/auditar_invariantes_chamados.py`.
- `auditar_disputas`: sem carimbo+disputa aberta delimita pergunta cega (sem data no texto) e pergunta sem destino; `app/chamados/management/commands/auditar_disputas.py:69`, `:71`, `:74`, `:148`, `:150`.
- `auditar_chamados_orfaos`: exclui `ENCERRADOS` do censo de órfãos; `app/chamados/management/commands/auditar_chamados_orfaos.py:36`.
- `encerrar_perguntas_com_fato`: carimbada sem validação e sem via fato materializado; só via sem história e marco aceso vira alvo; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`, `:40`, `:41`.
- `reabrir_orfas_enterradas`: via fantasma carimbada em janela temporal; testa pai encerrado e recusa de reabertura; confirma após aplicar; `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`, `:87`, `:97`, `:104`, `:160`, `:163`.
- `reabrir_chamados_ausencia_aberta`: só encerrados com nota do bug e ausência ainda aberta são alvo de reabertura; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:28`, `:31`, `:36`, `:49`.
- `reconciliar_fantasmas`: pergunta sem carimbo+disputa aberta é diagnóstico de ligações, não juiz de completude; `app/ponto/management/commands/reconciliar_fantasmas.py:209`, `:212`, `:216`.
- `bin/censo_folha.py`: tem régua própria, exclui `['resolvido','fechado','retratado','cancelado']` para contar “chv”; não inclui superado e inclui estado inexistente retratado. É script diagnóstico, não regra do catálogo. `bin/censo_folha.py:30`.
- `smoke_chamados` lê e compara status com destino para assertiva de fechamento/reabertura; é comando de smoke, não leitor de negócio. `app/chamados/management/commands/smoke_chamados.py:136`, `:149`.

### API, telas e GLPI

- API local serializa status e rótulo e autoriza `pode_responder` quando fora de `ENCERRADOS`. `app/api/views.py:1145`, `:1146`, `:1155`. Mensageria devolve estado em dados/fila. `app/api/views_mensageria.py:417`, `:1075`.
- Relatórios filtram por qualquer status solicitado e PDF imprime estado. `app/relatorios/views.py:745`, `:920`; `app/relatorios/pdf_chamados.py:116`.
- Ficha/calendário/drawer usam somente vivos para associação e leitura de cobrança. `app/colaboradores/services/calendario.py:553`, `:557`, `:558`, `:560`; `app/colaboradores/services/ficha.py:163`; `app/colaboradores/services/drawer.py:89`.
- Fio colaborador: terminais resolvido/fechado mudam cor; estado passa por filtro espera; não fechado e sem GLPI mostra resposta. `app/templates/chamados/fio_colaborador.html:167`, `:188`, `:254`.
- Detalhe local: espera/rótulo; resolução só fora de resolvido/fechado; outros botões/form por não fechado. `app/templates/chamados/detalhe_local.html:68`, `:279`, `:343`, `:375`.
- Detalhe GLPI: rótulo local e form não fechado. `app/templates/chamados/detalhe.html:36`, `:117`.
- Painel do turno: rótulo e form não fechado. `app/templates/chamados/partials/painel_chamado_turno.html:18`, `:65`.
- Modal: cores pelo status; espera; bloco de ação fora de `encerrados_juiz`; form não fechado; badges de aplicação vêm de `estado_aplicacao_pergunta`. `app/templates/chamados/partials/modal_fio.html:73`, `:99`, `:223`, `:493`, `:644`.
- Card atendimento: cor/rótulo do status; `p.viva` permite formulário da pergunta; `p.estado` apresenta encerrado/validado; form de chamado só aberto/em_análise e sem pergunta viva. `app/templates/chamados/partials/_card_atendimento.html:13`, `:23`, `:77`, `:183`, `:203`, `:205`.
- Lista admin entrega `data-status`, fallback visual do estado; JavaScript **inline** filtra igualdade do status solicitado no dataset. `app/templates/

[truncado em 20000 de 24619 caracteres]

## 30/10 18:18 · sessao b9c3ae2eb982 · CODEX-CONTEXTO-DA-CASA-AB2 · verificador · origem=Code

**Veredito: REPROVA.** Ainda há duas contraprovas contra os avais: turno não encerrado pode continuar aparecendo como `falta`, e o motivo `sem_celula` da folha pode ser descartado pela tela. Fontes: `app/colaboradores/services/calendario.py:822`, `:139`, `:899`; avais em `app/docs/PROMPTS.md:1676` e `:1681`.

1. **`concorde` sem batida derruba a proteção temporal.**

   Qualquer `_vv` preenchido desliga `futuro`, mesmo quando `dias_encerrados` respondeu que o turno continua aberto. Depois, `_classificar` escolhe `falta` e descarta a tradução `concorde → ok` porque não houve batida. Resultado: **juíza diz não encerrado; calendário acusa falta**. Fontes: `app/colaboradores/services/calendario.py:822`, `:148`, `:173`, `:176`.

   Contraprova: jornada de 14/10, 18h–06h, consultada às 02h de 15/10, sem batidas, com `veredito='concorde'`. A função real de encerramento responde `relogio`; o classificador real termina em `falta`. Confirmei essa composição executando as funções extraídas por AST em memória. Fontes dos corpos executados: `app/escala/servico_jornada.py:61`, `app/ponto/turnos.py:2088`, `app/colaboradores/services/calendario.py:108`.

   Esse veredito pode nascer do próprio escritor: o suprajuiz não acusa ausência enquanto `turno_encerrado=False`; códigos vazios viram `concorde`; o cartório passa esse insumo e lavra o resultado. Portanto, **veredito preenchido não prova encerramento integral do turno**. Fontes: `app/ponto/supra_juiz.py:279`, `app/escala/models.py:1592`, `app/ponto/services/cartorio.py:1117`, `:1409`.

   O teste novo preserva apenas `nunca_bateu`; falta o adversarial `concorde` com turno não encerrado. Fonte: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:65`.

2. **Passado sem fim previsto e sem veredito ainda pode perder o rótulo da folha.**

   O calendário consulta corretamente a retenção e recebe `sem_celula`. Porém, se houver régua suspeita, `_classificar` retorna `escala_suspeita` antes de ler `retido_folha`. O rótulo da folha só é publicado quando o status final é `sem_certidao`. Fontes: `app/colaboradores/services/calendario.py:726`, `:727`, `:139`, `:141`, `:899`.

   A combinação é alcançável: a suspeita vem dos campos gravados do vínculo, enquanto a folha retém célula passada sem veredito independentemente dessa suspeita. A tela então imprime seu texto de escala suspeita, sem o motivo retornado pela folha. Isso deixa incompleta a ordem das 15:3x de ler o rótulo daquele juiz. Fontes: `app/escala/regua_defesa.py:620`, `app/folha/export.py:1237`, `app/templates/colaboradores/partials/_calendario_grade.html:103`, `app/docs/PROMPTS.md:1681`.

3. **Permanece um defeito herdado no contrato de alimentação do cartório.**

   `dias_encerrados` recebe células por `(colaborador_id, data)` e as repassa diretamente a `turnos_do_colab`, que espera `{data: célula}` e acrescenta novamente o identificador. A chave fica aninhada; a busca da geradora deixa de encontrar a célula. O cartório usa exatamente essa alimentação. Fontes: `app/ponto/turnos.py:2113`, `:2132`, `:1852`, `:1928`; `app/escala/alimentacao.py:237`; `app/ponto/services/cartorio.py:853`.

   É dívida anterior registrada, não regressão atribuível a este diff. O calendário novo evita esse repasse porque fornece os turnos já carregados. O teste novo usa célula sem geradora, portanto não discrimina a perda dela. Fontes: `app/docs/BACKLOG.md:790`; `app/colaboradores/services/calendario.py:712`; `app/ponto/tests/test_c1_dias_encerrados.py:62`.

**Escritores e leitores dos campos envolvidos:**

| Campo ou insumo | Quem escreve/produz | Quem lê |
|---|---|---|
| `CelulaDia.trabalha`, `dna.marcos`, `escala_geradora` | Porta da célula: geração, regeneração e restauração — `app/ponto/portas/celula.py:593`, `:260`, `:530` | Vínculo do dia, marcos e trabalho previsto — `app/escala/alimentacao.py:237`; `app/escala/models.py:1094`; `app/colaboradores/services/calendario.py:785` |
| `veredito`, `veredito_via`, `protestos` | `lavrar_veredito` — `app/ponto/portas/celula.py:647`, `:691`; cartório chama em `app/ponto/services/cartorio.py:1409`, decisão humana em `app/ponto/services/veredito_celula.py:330` | Calendário monta `_ver_map`, exclui candidatos cegos e condiciona `futuro`; folha decide retenção — `app/colaboradores/services/calendario.py:525`, `:723`, `:822`; `app/folha/export.py:1215` |
| `Turno.data_turno`, `aberto`, `batidas` | Pareador, via `_mk` — `app/ponto/turnos.py:310` | Calendário agrupa e alimenta a juíza; `turno_aberto_vivo` decide expiração — `app/colaboradores/services/calendario.py:316`, `:713`; `app/ponto/turnos.py:1987` |
| Motivo de não encerramento / booleano | `_dia_encerrado_motivo_puro`; lote publica motivo ou `m is None` — `app/ponto/turnos.py:2088`, `:2137` | Calendário monta `_em_curso` e `_sem_fim` — `app/colaboradores/services/calendario.py:714`, `:722`; cartório, suprajuiz, emissor retroativo e carregador de par — `app/ponto/services/cartorio.py:853`; `app/ponto/management/commands/supra_juiz.py:108`; `app/ponto/management/commands/emitir_furo_retroativo.py:62`; `app/ponto/turnos.py:2150` |
| Retenção `{data, motivo}` / `retido_folha` | `motivos_retencao_celula` produz; calendário filtra `sem_celula` — `app/folha/export.py:1200`; `app/colaboradores/services/calendario.py:727` | `_classificar` e montagem do rótulo — `app/colaboradores/services/calendario.py:141`, `:834`, `:899` |
| `futuro` | Montagem dos fatos — `app/colaboradores/services/calendario.py:822` | Apenas `_classificar`, inclusive suas precedências — `app/colaboradores/services/calendario.py:135`, `:155` |
| `status` | `_classificar`, chamada única — `app/colaboradores/services/calendario.py:108`, `:814` | Grade, resumo, portas de consulta e oferta de resolver — `app/templates/colaboradores/partials/_calendario_grade.html:69`; `app/colaboradores/services/calendario.py:949`, `:219`, `:231`, `:204` |
| `status_rotulo` | Montagem do calendário — `app/colaboradores/services/calendario.py:899` | Grade, incluindo o novo título do ramo residual — `app/templates/colaboradores/partials/_calendario_grade.html:99`, `:107` |
| `resolver_dia_oferecido` | Juiz existente, publicado após os status — `app/colaboradores/services/calendario.py:199`, `:945` | Template e vigia do fio — `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:294` |

A suspeita usada na segunda contraprova é gravada pela porta `gravar_regua`, traduzida por `rotulo_regua_para_espelho` e passada ao classificador pelo calendário. Fontes: `app/colaboradores/services/vinculo.py:889`; `app/escala/regua_defesa.py:620`; `app/colaboradores/services/calendario.py:387`, `:824`.

**Relógios e custo por montagem:**

- O trecho novo compartilha `_agora`: dele sai `hoje`, o instante entregue à juíza e a data entregue à retenção. O fim previsto vem do DNA; aberta viva expira pela janela do template mais quatro horas, distinção herdada. Fontes: `app/colaboradores/services/calendario.py:258`, `:712`, `:726`; `app/ponto/turnos.py:2102`, `:1997`.
- O calendário inteiro ainda não usa um único relógio: desfazer retratação captura outro instante; `folha_manda` consulta seu teto civil; a view captura outro `hoje`. Fuso configurado: São Paulo, `USE_TZ=True`. Fontes: `app/colaboradores/services/calendario.py:343`, `:689`; `app/relatorios/cartao_pela_celula.py:412`, `:40`; `app/colaboradores/views.py:1960`; `app/config/settings/base.py:93`.
- **Acréscimo estrutural comum: uma consulta de vínculos por montagem.** Turnos e células fornecidos evitam recarga, mas a consulta de escalas continua incondicional. Havendo `_sem_fim`, ocorre uma chamada adicional à folha, normalmente com duas consultas: células e vínculos; isento ou janela colapsada retornam antes. Fontes: `app/ponto/turnos.py:2129`, `:2130`, `:2132`; `app/colaboradores/services/calendario.py:726`; `app/folha/export.py:1174`, `:1188`, `:1194`.
- Esse custo se repete no perfil, drawer, montagem por colaborador no painel de furos e consulta do vigia. Fontes: `app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:184`; `app/chamados/services/vigia_fio.py:294`. Tempo e custo efetivo total deste hash: **[não verificado]**.

**Os testes mordem parte importante da cura, não o fechamento inteiro.** [nao verificado]

- Há asserções discriminantes para antes do turno, meia-noite, turno até dois dias depois, falta após encerramento e passado sem `hf` com vínculo. O render usa sentinela para exigir leitura efetiva de `status_rotulo`. Fontes: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42`, `:47`, `:57`, `:61`, `:82`, `:108`.
- Faltam os dois adversariais acima: `concorde` durante turno não encerrado e régua suspeita junto de `sem_celula`. O teste de veredito usa `nunca_bateu`; o teste cego não combina suspeita. Fontes: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:65`, `:92`.
- O teste de custo limita **o delta do ramo cego** a duas consultas; o custo comum cancela na subtração. Existe teto global de 55 consultas, mas seu resultado nesta árvore está **[não verificado]**. Fontes: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`; `app/core/tests/test_selo_performance.py:249`, `:401`.
- A nova alimentação `turnos=` não tem cenário novo com batidas; o C1 alimentado testa DNA, sem geradora concorrente. Fontes: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:19`; `app/ponto/tests/test_c1_dias_encerrados.py:53`.

Execução da suíte Django, RED contra a base e certificação integral do hash: **[não verificado]**. As sondas desta revisão foram puras, em memória, sobre os corpos citados. [nao verificado]

## 30/10 18:17 · sessao 85839565fbf5 · - · - · origem=Ronald (tmux)

Censo de `materializada_em`: **20 sítios explícitos de escrita em 19 funções e 7 arquivos de produção: 14 carimbos (13 `PerguntaDisputa` + 1 `RegularizacaoExterna`) e 6 resets de `PerguntaDisputa`**. A enumeração abaixo é a prova de contagem; `resolver_perguntas_por_chamado_resolvido` contém dois sítios. O campo pertence a `PerguntaDisputa`, cuja FK `chamado_destino` aponta para `ChamadoColaborador`, e também existe, separadamente, em `RegularizacaoExterna`; não é um campo de `PerguntaColaborador`. `app/chamados/models.py:1935`, `:2379`, `:2387`, `:2744`, `:2766`.

**14 sítios de carimbo**

| Escritor concreto | Condição/efeito e porta usada | Chamador |
|---|---|---|
| `PerguntaDisputa.fechar_por_admin` | `via=admin_decidiu`, `now`, autor; idempotência somente se já nessa via e carimbada. Não consulta C6 dentro do método. `app/chamados/models.py:2213`, `:2218`, `:2228`, `:2230`. | `DisputaSupervisao.fechar` primeiro calcula vivas pelo C6, depois chama para desligamento/admin encerra; decisão explícita pode encerrar viva. `app/chamados/models.py:1556`, `:1578`, `:1595`, `:1598`. Também `fechar_o_fio_do_dia` seleciona pelo **pré-filtro** `q_vivas`, não por C6 exato, e chama. `app/ponto/services/veredito_celula.py:160`, `:174`, `:177`. Aceite de folga na validação chama depois de validar. `app/chamados/services/validacao.py:261`, `:264`, `:283`, `:292`. |
| `PerguntaDisputa.fechar_por_competencia_trancada` | Não validada e ainda fora dessa via → `competencia_trancada`, `now`, autor; não consulta C6. `app/chamados/models.py:2233`, `:2239`, `:2242`, `:2244`. | **Nenhum caller de produção encontrado** por referência direta [nao verificado]. O serviço atual declara expressamente que **nenhuma pergunta se carimba**, porque C6 lê a tranca; só encerra chamados e disputas. `app/chamados/services/competencia_trancada.py:171`. |
| `PerguntaDisputa.encerrar_fantasma` | `fantasma`, `now`, autor; método só executa e testa idempotência. `app/chamados/models.py:2258`, `:2264`, `:2267`, `:2269`. | Evento/outbox: `encerrar_orfas_pela_lampada` chama `veredito_orfa` e só executa se `ENCERRA`. `app/chamados/reconciliador.py:1082`, `:1083`, `:1087`, `:1095`. A decisão exige **pergunta C6 viva**, sem resposta, marco aceso e dia encerrado. `app/chamados/reconciliador.py:1033`, `:1035`, `:1037`, `:1039`, `:1041`. O consumidor é chamado também pelo cron `reconciliar_chamados`. `app/chamados/reconciliador.py:1303`; `app/chamados/management/commands/reconciliar_chamados.py:62`. |
| `lavrar_sem_turno_aberto` | Motivo C1, sem texto, sem carimbo, sem turno aberto **e C6 morta** → `sem_turno_aberto`, `now`, sistema. `app/chamados/reconciliador.py:363`, `:365`, `:367`, `:372`, `:399`, `:402`, `:406`, `:407`. | `reatar_pergunta_orfa`, ramo c1; chamado pelo reconciliador por colaborador, cron e evento de batida. `app/chamados/reconciliador.py:1519`, `:1351`; o próprio corpo adverte que o reatador segue ligado ao cron, não à emissão. `:1436`. |
| `retratar_perguntas_absolvidas_por_celula` | Não carimbada/não validada, célula julgada sem acusação; pula marcos pendentes e **C6 viva**; grava `celula_absolveu`, `now`, sistema. `app/chamados/reconciliador.py:584`, `:607`, `:613`, `:617`, `:619`, `:620`. | `reconciliar_chamados_colab`; evento da batida + cron */15. `app/chamados/reconciliador.py:1348`, `:1350`; `app/chamados/management/commands/reconciliar_chamados.py:52`. |
| `_materializar_ausencia_de_resposta` | Resposta escape cria Ausência e absorve perguntas-hora irmãs do dia ainda sem carimbo; marca via `declaracao` somente se `declarou_hora`, senão `ausencia_dia`; não usa C6 como guarda prévia do carimbo. `app/chamados/services/materializacao.py:478`, `:499`, `:507`, `:509`, `:525`, `:528`. | Responder pergunta web/API: núcleo roteia `FALTA_DIA`/`ATESTADO` pela tabela de escapes. `app/chamados/services/disputa_emissao.py:1212`, `:1216`, `:1292`, `:1295`; tabela `app/chamados/services/materializacao.py:549`. |
| `_materializar_resposta_hora`, recusa de paridade | Vizinha do mesmo tipo e plantio não preenche furo; só carimba `paridade_recusada` se **`fato_do_marco_no_espelho(p) is True`**, outra porta de fato, não C6. `app/chamados/services/materializacao.py:815`, `:816`, `:840`, `:841`, `:858`, `:868`. | `_materializar_uma` no fluxo validação/materialização e lote do passivo. `app/chamados/services/materializacao.py:1125`; `app/chamados/services/validacao.py:417`; `app/chamados/services/passivo_validado.py:516`. |
| `_carimbar_recusa` | Só carimba se ainda sem carimbo e `traduzir_recusa` classifica **COLISAO** → `fato_ocupado`; nenhuma guarda C6. `app/chamados/services/materializacao.py:1028`, `:1037`, `:1042`, `:1043`, `:1053`. | Chamado pelas recusas da unidade materializadora, inclusive no `finally`; lote do passivo importa a mesma função. `app/chamados/services/materializacao.py:1214`; `app/chamados/services/passivo_validado.py:506`. |
| `_materializar_uma`, sucesso | Só após geração/reuso de fato e aprovação de `_pergunta_materializada_de_fato` para hora/data_hora; grava `now`, autor, via declaração ou `via_pelo_fato`; **juiz de lastro, não C6**. Reprovação levanta dentro do atomic. `app/chamados/services/materializacao.py:1133`, `:1136`, `:1138`, `:1143`, `:1149`, `:1177`, `:1198`. | Admin: `materializar_perguntas_validadas_da_disputa` seleciona respondida+validada+não materializada. `app/chamados/services/materializacao.py:1286`, `:1293`. Chamadores telas `fio.py:664`, `acoes_disputa.py:139`, validação `validacao.py:417`, `:423`; lote passivo `passivo_validado.py:516`. Todos sob `app/chamados/services/`. |
| `materializar_pendentes_aprovadas` | Perguntas sem carimbo cuja `_pergunta_materializada_de_fato` aprova → `juiz_aprovou`, `now`, autor; não fabrica batida nem chama C6 diretamente. `app/chamados/services/materializacao.py:1335`, `:1337`, `:1339`, `:1341`, `:1342`. | Signal post_save de disputa fechada (criada/raw excluídas). O signal apenas delega. `app/chamados/signals.py:124`, `:130`, `:139`, `:140`. |
| `resolver_perguntas_por_chamado_resolvido`, S128 destino | Sem carimbo, disputa aberta, destino chamado recebido; pula tranca, **C6 viva** e resposta com texto sem lastro; grava `now` e `via_pelo_fato`. `app/chamados/services/disputa_emissao.py:1531`, `:1536`, `:1547`, `:1551`, `:1558`, `:1562`, `:1583`. | Signal de todo save de chamado em `ENCERRADOS`; `.update` não emite o signal. `app/chamados/signals.py:75`, `:81`, `:92`. Também `_retratar` explicitamente, e reatador HIST: `app/chamados/reconciliador.py:538`, `:1547`; serviço becos `app/chamados/services/becos.py:49`. |
| Mesma função, órfãs sem credor | Disputas abertas do colaborador sem pai/destino vivo, perguntas sem carimbo/validação; **pula C6 viva** e carimba as mortas; via do fato ou `chamado_encerrado` sem pai. `app/chamados/services/disputa_emissao.py:1622`, `:1628`, `:1631`, `:1634`, `:1641`, `:1644`, `:1646`, `:1659`. | Mesmos chamadores S128 acima. `app/chamados/signals.py:92`; `app/chamados/reconciliador.py:538`. |
| `retratar_duplicata` | Fora de `duplicata` → grava via/recusa e `now` **somente se carimbo nulo**; método não consulta C6. `app/chamados/services/fio.py:830`, `:834`, `:845`, `:846`, `:848`. | Dedup manual/lote passa excedentes pela identidade do fato, sem C6. `app/chamados/management/commands/dedup_perguntas_fato.py:67`, `:72`, `:88`. Migração de resolvedoras consulta C6 antes, depois chama nas duplicatas do conflito. `app/chamados/services/fio.py:882`, `:884`, `:892`. |
| `materializar_regularizacao_externa` | **Outro modelo**, `RegularizacaoExterna`: exige confirmada e nenhuma disputa aberta; processa itens; salvo recusa total sem operações, grava `status=materializada`, `now`, autor; não pergunta C6. `app/chamados/services/regularizacao_ext.py:51`, `:58`, `:61`, `:191`, `:197`, `:198`, `:200`. | Confirmação web/API via `confirmar_e_materializar`: `app/chamados/services/regularizacao_ext.py:347`, `:356`; tela `app/chamados/views_regularizacao.py:217`, APIs `app/api/views.py:3063`, `app/api/views_core.py:1329`. Retomada após fechamento de disputa por signal on_commit: `app/chamados/signals.py:97`, `:114`, `:121`; retomada chama materializador em `app/chamados/services/regularizacao_ext.py:224`. |

**6 resets**

| Escritor | Regra / quem chama / C6 |
|---|---|
| `ChamadoColaborador.ressuscitar_perguntas` | `update(materializada_em=None,...)`; universo materializadas sem resposta, **poupa marco comprovadamente aceso** via `fato_do_marco_no_espelho`, não C6 diretamente. `app/chamados/models.py:1208`, `:1215`, `:1258`. Chamado por `reabrir` `:1194` e cartório `app/ponto/services/cartorio.py:1271`. |
| `PerguntaDisputa.reabrir_fantasma` | Só `fantasma` carimbada → limpa; método não julga. `app/chamados/models.py:2279`, `:2282`, `:2284`. Command de acervo executa: `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`; condição de chamador documentada com marco apagado em `app/chamados/models.py:1245`. |
| `PerguntaDisputa.reabrir_por_resposta` | Só carimbo em `VIAS_SEM_LASTRO` → limpa em memória, devolve campos; caller faz save. Sem chamada C6. `app/chamados/models.py:2299`, `:2302`, `:2304`; resposta core `app/chamados/services/disputa_emissao.py:1265`, `:1266`. |
| `PerguntaDisputa.reabrir_por_competencia` | Só via `competencia_trancada` → limpa e salva; sem C6 no método. `app/chamados/models.py:2309`, `:2312`, `:2314`. Evento reabertura de competência: `app/chamados/services/competencia_trancada.py:244`, `:247`. |
| `reabrir_para_correcao` | Quando `desfazer_absorcao=True`, adiciona materialização nula ao dicionário e executa `qs.update(**campos)`. Não consulta C6; seleção pertence ao caller. `app/chamados/services/disputa_emissao.py:1358`, `:1360`, `:1377`. Rejeição da Ausência, sem outra cobrindo, usa `perguntas_absorvidas_por` e chama: `app/chamados/services/materializacao.py:608`, `:615`, `:617`, `:628`; evento da ausência `app/ponto/signals.py:261`. |
| `desfazer_carimbo_sem_lastro` | Relê travado, filtra vias desfazíveis, só limpa se **C6 viva**, preserva resposta/validação; reabre disputa fechada conforme universo de entrada. `app/chamados/services/relavra_via.py:221`, `:223`, `:227`, `:231`, `:234`, `:248`. Chamadores fábrica de lote e evento julgamento: `app/chamados/services/fabrica_da_celula.py:119`, `:146`. |

**Quatro crons**

- `reconciliar_chamados`: */15, sem dry-run; chama reconciliador por colaborador, rejunta célula, drena outbox. Pode retratar/encerrar/reabrir/renascer chamados, carimbar perguntas via `celula_absolveu`, `sem_turno_aberto`, S128 e `fantasma`, e reapontar perguntas/células; reaberturas podem limpar carimbo por ressuscitação. `app/config/crons.py:376`; `app/chamados/management/commands/reconciliar_chamados.py:52`, `:56`, `:62`; `app/chamados/reconciliador.py:1350`, `:1351`, `:1369`, `:1519`, `:1521`, `:1541`, `:1547`, `:1303`.

- `fechar_cobranca_com_lastro`: hora `:40`, `--apply`; comando delega `lastro.fechar`, que calcula quadro uma vez, relê status antes de agir, chama `ch.reconciliar` e só então anexa nota. **Não escreve materializada_em diretamente**, mas o encerramento por save dispara S128 (duas escritas e possíveis reaberturas acima). Não é emissor de pergunta nova diretamente. `app/config/crons.py:705`; `app/ponto/management/commands/fechar_cobranca_com_lastro.py:52`, `:53`; `app/ponto/services/lastro.py:302`, `:310`, `:317`; `app/chamados/signals.py:92`.

- `reavaliar_ausencias_feriado`: 07:02, por empresa, `--apply`, depende cartório. Seleciona `batida_ausente` vivos; `fatos_do_dia` precisa dizer fase conhecida e feriado/folga com premissa morta; chama `ch.reconciliar`, nota e trilha. Não carimba/cria pergunta diretamente; **S128 é efeito indireto**, como acima. `app/config/crons.py:589`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:23`, `:24`, `:32`; `app/chamados/services/feriado_reavaliar.py:17`, `:33`, `:34`, `:36`, `:38`, `:47`; `app/chamados/signals.py:92`.

- `disparar_perguntas_competencia`: encadeado depois de `apurar_furos_diarios`, `--comp-corrente --apply`, depende também cartório. Universos célula acusadora/ativo/marcos vencidos e chamado vivo sem pergunta viva; fábrica desfaz carimbo sem lastro via C6, abre/atualiza disputa/chamado e gera pergunta. Não carimba materializada com `now` diretamente; **pode limpar** via `desfazer_carimbo_sem_lastro`. `app/config/crons.py:1456`; `app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `:105`, `:111`, `:120`, `:155`, `:163`, `:276`, `:288`; `app/chamados/services/fabrica_da_celula.py:119`, `:122`, `:124`.

Os quatro estão em **`JUIZES_POR_VARREDURA`**, respectivamente `app/config/crons.py:1976`, `:1964`, `:1975`, `:1959`. **Nenhum está em `PENDENTES_CHAMADO`: tupla vazia** `app/core/juizes.py:421`, alias `:1303`. “Onde morar” hoje é registro do papel cron `juiz`; são dívidas diferentes de clone de juiz. O contador inclui encadeados, e documentação explicita que mudar agenda não apaga a dívida. `app/config/crons.py:2242`, `:2245`, `:2251`.

**Método/limites para consolidar**

- Fiz parsing AST em todos `.py` da árvore, procurando atribuições do atributo e keyword do campo; li corpo completo, save/update e os chamadores. Isso diferencia o `Q(materializada_em=...)` de leitura e o `dict.update` que alimenta uma escrita posterior; em particular incluiu `update(**campos)` que um regex simples perde. Prova de operação indireta `app/chamados/services/disputa_emissao.py:1360`, `:1377`.
- Varri `bulk_update`, `setattr`, kwargs, SQL e scripts para conferir alvos/campos; o `bulk_update` de `PerguntaDisputa` em produção escreve somente campos da lavra, não materialização. `app/chamados/veredito_lavrado.py:207`.
- Nas migrações encontradas só há declaração do campo, não migração de valor: `app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:23`, `app/chamados/migrations/0021_regularizacaoexterna_regularizacaoitembatida_and_more.py:26`. Scripts não mostraram mutação desses campos [nao verificado quanto a geração dinâmica de nomes].
- **20 não inclui nascimento NULL por default nem escrita genérica potencial**: `PerguntaDisputa.abrir` aceita `**campos` e repassa a `objects.create`, portanto pode receber materializada em entrada; nenhum caller de produção encontrado fornece o campo [nao verificado quanto a kwargs externos]. `app/chamados/models.py:2124`, `:2181`. O campo tem `null=True` e não tem default `now`. `:2387`.
- Não rodei Django, banco ou testes; não verifico execução real, cron instalado, triggers SQL externos, chamadas por shell fora da árvore, monkeypatches, dados ou versão do commit [nao verificado]. [nao verificado]
- O P13 existente **não é este censo por entidade/operação**: regex `.campo =` ou `update(...campo=)`, seis apps, exclui testes/migrations, enumera **7 arquivos** para o campo e combina os dois modelos. `app/core/tests/test_contract_escritores_estado.py:12`, `:13`, `:83`, `:118`, `:227`, `:234`, `:239`. Testes têm semeaduras diretas deliberadas, por exemplo `app/chamados/tests/test_contract_pergunta_viva.py:51`, `app/chamados/tests/test_reabrir_orfas_enterradas.py:76`; ficaram separados do censo de caminhos de produção.

## 30/10 18:17 · sessao 412d5bddd613 · - · - · origem=Ronald (tmux)

O campo do chamado é **`ChamadoColaborador.status_local`**, com terminais `resolvido`, `fechado`, `cancelado`, `superado`; `registrado` pertence a `VIVOS`. Há um núcleo físico de transição, `_gravar_estado`, que atribui o campo e salva, mas ele não veta encerramento por `pergunta_viva`: `app/chamados/models.py:46`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/chamados/models.py:192`, `app/chamados/models.py:231`.

**Portas e condição de encerramento**

| Porta | Terminal e condição | C6 antes da escrita? | Prova |
|---|---|---|---|
| `transicionar`, chamado por `fechar` | `resolvido`/`fechado`, permitido pelo motor/perfil; fechado recusa justificativa pendente; encerrados recusam disputa aberta viva; resolvido de furo exige premissa regularizada | **Sim, limitado:** `disputa_viva_do_chamado` pega a primeira disputa aberta e consulta `fio_vivo` das perguntas dela. Não enumera todas as perguntas cujo destino é o chamado | `app/chamados/models.py:122`, `app/chamados/models.py:130`, `app/chamados/models.py:142`, `app/chamados/models.py:165`, `app/chamados/models.py:262`, `app/chamados/juizes.py:1817`, `app/chamados/juizes.py:1821`, `app/chamados/juizes.py:1111` |
| `reconciliar` | VIVOS → `resolvido`; recarrega status do banco; furo só fecha com premissa regularizada para folha, pai agrupador vivo ou outra cobrança viva do dia; sem dia permite com trilha | Não como veto geral; outro juiz: premissa/célula/folha | `app/chamados/models.py:434`, `app/chamados/models.py:463`, `app/chamados/models.py:470`, `app/chamados/models.py:484`, `app/chamados/models.py:489`, `app/chamados/models.py:495`, `app/chamados/models.py:498` |
| `cancelar` | VIVOS → `cancelado` | Não | `app/chamados/models.py:527`, `app/chamados/models.py:532`, `app/chamados/models.py:537`, `app/chamados/catalogo/motor.py:116` |
| `superar` | VIVOS → `superado`; superador diferente de si e não terminal | Não; sem chamador produtivo declarado | `app/chamados/models.py:543`, `app/chamados/models.py:547`, `app/chamados/models.py:560`, `bin/esmeril_chamado_censo.py:143` |
| `encerrar` | aberto/em análise/registrado/resolvido → `fechado`, via cascade | Não | `app/chamados/models.py:1288`, `app/chamados/models.py:1297`, `app/chamados/catalogo/motor.py:114` |
| `encerrar_por_competencia_trancada` | VIVOS → `fechado`, via competência trancada | Não no método; o serviço mede a competência antes | `app/chamados/models.py:1300`, `app/chamados/models.py:1305`, `app/chamados/models.py:1310`, `app/chamados/services/competencia_trancada.py:158`, `app/chamados/services/competencia_trancada.py:160` |

`_gravar_estado` relavra perguntas depois do save quando a transição cruza a fronteira ENCERRADOS; isso é efeito posterior, não guarda anterior: `app/chamados/models.py:231`, `app/chamados/models.py:243`, `app/chamados/models.py:246`.

**Todos os sítios produtivos de chamada encontrados para transições terminais**

Nas linhas que chamam `reconciliar`, o terminal é sempre `resolvido`, sujeito à guarda comum acima; nas que chamam `encerrar`, é `fechado`: `app/chamados/catalogo/motor.py:111`, `app/chamados/catalogo/motor.py:114`.

| Chamador / disparador / condição própria | Sítio(s) |
|---|---|
| Signal de disputa fechada, pai existente e ainda não ENCERRADO | `app/chamados/signals.py:28`, `app/chamados/signals.py:33`, `app/chamados/signals.py:42` |
| Retratação canônica: retém irmã respondida sem consumo e justificativa alheia pendente no ato de aprovar; depois reconcilia | `app/chamados/reconciliador.py:439`, `app/chamados/reconciliador.py:520`, `app/chamados/reconciliador.py:525` |
| Chamador da retratação: reconciliação por colaborador, premissa julgada morta; evento da batida e cron | `app/chamados/reconciliador.py:1348`, `app/chamados/reconciliador.py:1363`, `app/chamados/reconciliador.py:1369` |
| Chamador da retratação: célula absolveu chamado de turno; consumidor do outbox | `app/chamados/reconciliador.py:933`, `app/chamados/reconciliador.py:951`, `app/chamados/reconciliador.py:955` |
| Fecho forçado com retenção, usado pela aprovação de justificativa e ajuste retroativo | `app/chamados/reconciliador.py:487`, `app/chamados/reconciliador.py:494`, `app/chamados/reconciliador.py:498`; chamadores: `app/ponto/services/justificativa.py:79`, `app/ponto/services/justificativa.py:385` |
| Desligamento: todos NAO_FECHADOS do colaborador → `encerrar` | `app/colaboradores/services/desligamento.py:132`, `app/colaboradores/services/desligamento.py:137` |
| Nova escala/aceite: chamados vivos anteriores de categoria escala_aceite → `encerrar` | `app/colaboradores/services/vinculo.py:373`, `app/colaboradores/services/vinculo.py:384`, `app/colaboradores/services/vinculo.py:387` |
| Regularização externa materializada, havendo chamado → `encerrar` | `app/chamados/services/regularizacao_ext.py:197`, `app/chamados/services/regularizacao_ext.py:201`, `app/chamados/services/regularizacao_ext.py:205` |
| Serviço de competência trancada: aplicar, trancada, com ações → porta específica | `app/chamados/services/competencia_trancada.py:156`, `app/chamados/services/competencia_trancada.py:160`, `app/chamados/services/competencia_trancada.py:177` |
| Adesão: filho furo no período é agrupado sob pai; reconcilia | `app/chamados/services/adesao.py:102`, `app/chamados/services/adesao.py:107`, `app/chamados/services/adesao.py:112`, `app/chamados/services/adesao.py:115` |
| Adesão estoque: pai nunca_bateu com batida, só com fechar e sem dry | `app/chamados/services/adesao.py:142`, `app/chamados/services/adesao.py:144`, `app/chamados/services/adesao.py:148`, `app/chamados/services/adesao.py:150` |
| Adesão por evento batida/pós-vínculo: batida não retratada na janela do pai | `app/colaboradores/services/pos_vinculo.py:17`, `app/colaboradores/services/pos_vinculo.py:36`, `app/colaboradores/services/pos_vinculo.py:41`, `app/colaboradores/services/pos_vinculo.py:42` |
| Aceitar sugestão de folga: grava template e fecha LIMBO-FOLGAS vivos daquele template | `app/chamados/services/limbo_folgas.py:73`, `app/chamados/services/limbo_folgas.py:91`, `app/chamados/services/limbo_folgas.py:98`, `app/chamados/services/limbo_folgas.py:101` |
| Agrupar furo sob pai LIMBO, ainda não agrupado | `app/chamados/services/limbo_folgas.py:214`, `app/chamados/services/limbo_folgas.py:217`, `app/chamados/services/limbo_folgas.py:221`; detector chama em `app/ponto/management/commands/detectar_ausencias.py:756` |
| Silêncio por ausência: ausência aprovada e premissa morta, com aplicar | `app/chamados/services/silencio_ausencia.py:93`, `app/chamados/services/silencio_ausencia.py:100`, `app/chamados/services/silencio_ausencia.py:101` |
| Ausência do chamado saiu de ABERTAS; evento fecha, cron de ausência virou vigia | `app/ponto/services/ausencia.py:670`, `app/ponto/services/ausencia.py:676`, `app/ponto/services/ausencia.py:678`, `app/ponto/services/ausencia.py:705`, `app/ponto/services/ausencia.py:709` |
| Materialização de resposta ausência: fecha destino só quando ausência não está ABERTA | `app/chamados/services/materializacao.py:531`, `app/chamados/services/materializacao.py:538`, `app/chamados/services/materializacao.py:539` |
| Materialização: destino e clones; retém irmãs respondidas sem consumo | `app/chamados/services/materializacao.py:679`, `app/chamados/services/materializacao.py:701`, `app/chamados/services/materializacao.py:702`, `app/chamados/services/materializacao.py:711`, `app/chamados/services/materializacao.py:713`, `app/chamados/services/materializacao.py:717` |
| Entrada sincronizada: cobrança batida_ausente do dia e todos os marcos acesos | `app/ponto/services/entrada_sincronizada.py:19`, `app/ponto/services/entrada_sincronizada.py:28`, `app/ponto/services/entrada_sincronizada.py:29`, `app/ponto/services/entrada_sincronizada.py:31` |
| Registro de batida na tela: E fecha falta; S fecha turno da entrada, recusa outro turno ainda aberto; premissa_morta | `app/ponto/services/triagem_batida.py:522`, `app/ponto/services/triagem_batida.py:535`, `app/ponto/services/triagem_batida.py:580`, `app/ponto/services/triagem_batida.py:593`, `app/ponto/services/triagem_batida.py:597` |
| Cobertura designada, origem viva e premissa_morta | `app/ponto/services/cobertura.py:114`, `app/ponto/services/cobertura.py:117`, `app/ponto/services/cobertura.py:122` |
| Cobertura passada: chamados vivos do dia, premissa_morta, sobrevivente por último | `app/ponto/services/cobertura.py:309`, `app/ponto/services/cobertura.py:310`, `app/ponto/services/cobertura.py:311`, `app/ponto/services/cobertura.py:318` |
| Admin decide dia: fecha perguntas por admin; chamados vivos por ordem do sobrevivente | `app/ponto/services/veredito_celula.py:160`, `app/ponto/services/veredito_celula.py:177`, `app/ponto/services/veredito_celula.py:182`, `app/ponto/services/veredito_celula.py:183` |
| Cartório, aplicar: todos os marcos do chamado acenderam | `app/ponto/services/cartorio.py:1153`, `app/ponto/services/cartorio.py:1163` |
| Cartório, aplicar: marcos cobrados não existem mais | `app/ponto/services/cartorio.py:1173`, `app/ponto/services/cartorio.py:1174`, `app/ponto/services/cartorio.py:1175` |
| Cartório, aplicar: COBRANCA_EM_DIA_SEM_TRABALHO ou COBRANCA_SEM_FURO; sobrevivente por último | `app/ponto/services/cartorio.py:1188`, `app/ponto/services/cartorio.py:1196` |
| Cartório, aplicar: fio sem viva, sem resposta escrita, todas as perguntas VIA_DUPLICATA | `app/ponto/services/cartorio.py:1258`, `app/ponto/services/cartorio.py:1261`, `app/ponto/services/cartorio.py:1263`, `app/ponto/services/cartorio.py:1265`, `app/ponto/services/cartorio.py:1266` |
| Lastro horário: julgar retorna com_lastro, aplicar, recarrega e exige VIVOS | `app/ponto/services/lastro.py:300`, `app/ponto/services/lastro.py:304`, `app/ponto/services/lastro.py:306`, `app/ponto/services/lastro.py:310`, `app/ponto/services/lastro.py:317` |
| Feriado/folga espúria: cron e evento compartilham serviço | `app/chamados/services/feriado_reavaliar.py:8`, `app/chamados/services/feriado_reavaliar.py:23`, `app/chamados/services/feriado_reavaliar.py:36`, `app/chamados/services/feriado_reavaliar.py:43`, `app/chamados/services/feriado_reavaliar.py:47` |
| Cluster espúrio: colaborador ativo com escala ativa sem fim; chamados cuja premissa morreu | `app/ponto/services/cluster_espurio.py:35`, `app/ponto/services/cluster_espurio.py:46`, `app/ponto/services/cluster_espurio.py:67`, `app/ponto/services/cluster_espurio.py:73`, `app/ponto/services/cluster_espurio.py:80` |
| Detector ausências: limpa LIMBO antigo | `app/ponto/management/commands/detectar_ausencias.py:158` |
| Detector ausências: afastamento / cobertura / entrada coletados | `app/ponto/management/commands/detectar_ausencias.py:812`, `app/ponto/management/commands/detectar_ausencias.py:816`, `app/ponto/management/commands/detectar_ausencias.py:824`, `app/ponto/management/commands/detectar_ausencias.py:826`, `app/ponto/management/commands/detectar_ausencias.py:834`, `app/ponto/management/commands/detectar_ausencias.py:837` |
| Detector intervalo: premissa morta por ausência do dia em datas_furo; dry usa reconciliaria | `app/ponto/management/commands/detectar_intervalo_ausente.py:115`, `app/ponto/management/commands/detectar_intervalo_ausente.py:128`, `app/ponto/management/commands/detectar_intervalo_ausente.py:134` |
| Marcar posto fechado em feriado: alvos de feriado | `app/colaboradores/management/commands/marcar_feriado_do_posto.py:116`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:126` |
| Cura BUG115: ata sem furo | `app/ponto/management/commands/curar_residuo_bug115.py:150`, `app/ponto/management/commands/curar_residuo_bug115.py:151` |
| Auditoria órfãos: categoria disputa_supervisao sem disputa aberta, com --fechar | `app/chamados/management/commands/auditar_chamados_orfaos.py:34`, `app/chamados/management/commands/auditar_chamados_orfaos.py:39`, `app/chamados/management/commands/auditar_chamados_orfaos.py:58`, `app/chamados/management/commands/auditar_chamados_orfaos.py:67` |
| Meio período: --apply, superuser ativo para assinar; `fechar` default fechado | `app/ponto/management/commands/fechar_chamados_meio_periodo.py:94`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:103`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109` |
| Aviso régua migrado à lista C7, --apply → cancelar | `app/escala/management/commands/fechar_avisos_regua_na_lista.py:32`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:36`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:42`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47` |
| Revisão vínculo migrada à lista C8, --apply → cancelar | `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:40`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:44`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:52`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57` |
| Tela avaliação benefício: aprovar → resolvido; outra ação → fechado | `app/colaboradores/views.py:2055`, `app/colaboradores/views.py:2068` |
| Tela aprovação exceção, agenda aceita/recusada, escala aceita/recusada, geofence autorizado/recusado → resolvido | `app/chamados/services/acoes_chamado.py:70`, `app/chamados/services/acoes_chamado.py:108`, `app/chamados/services/acoes_chamado.py:149`, `app/chamados/services/acoes_chamado.py:222`, `app/chamados/services/acoes_chamado.py:277` |
| Tela resposta livre: gestor, novo_status dentre choices; transicionar limita | `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/acoes_chamado.py:315` |
| Tela fio: chamado + conteúdo, RBAC, novo_status dentre choices | `app/chamados/services/fio.py:703`, `app/chamados/services/fio.py:708`, `app/chamados/services/fio.py:713`, `app/chamados/services/fio.py:715` |
| Smoke operacional: transições válidas, reabertura e recusas | `app/chamados/management/commands/smoke_chamados.py:134`, `app/chamados/management/commands/smoke_chamados.py:147`, `app/chamados/management/commands/smoke_chamados.py:163` |

**Nascimento já terminal e migrações são exceções à passagem física pelo núcleo**

`criar(status=...)` preserva qualquer status diferente de aberto, passa ao `objects.create` e não chama `_gravar_estado`; o smoke exerce nascimento `fechado` e `resolvido`. `abrir` monta `_def` com estado inicial do motor, mas depois incorpora defaults fornecidos pelo chamador antes do get_or_create: `app/chamados/models.py:818`, `app/chamados/models.py:827`, `app/chamados/models.py:851`, `app/chamados/management/commands/smoke_chamados.py:122`, `app/chamados/management/commands/smoke_chamados.py:126`, `app/chamados/management/commands/smoke_chamados.py:146`, `app/chamados/management/commands/smoke_chamados.py:158`, `app/chamados/models.py:702`, `app/chamados/models.py:704`, `app/chamados/models.py:735`.

A migração 0039 grava diretamente cancelado → superado quando prova irmão único nos módulos de furo, mesmo colaborador/data_turno; sua reversa grava superado → cancelado. Ambas passam fora de núcleo e C6: `app/chamados/migrations/0039_a3_via_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:20`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:33`.

**Modelos relacionados**

`DisputaSupervisao` usa `fechada_em`, não um status terminal; `aberta` é `fechada_em is None`. A porta `fechar` consulta efetivamente C6 para todas as perguntas e recusa vivas no fluxo normal; exceções nomeadas `desligamento`/`admin_encerra` fecham mudas com ato de admin, e todas recusam respostas sem veredito. A gravação física fica em `fechar`: `app/chamados/models.py:1504`, `app/chamados/models.py:1530`, `app/chamados/models.py:1558`, `app/chamados/models.py:1568`, `app/chamados/models.py:1577`, `app/chamados/models.py:1579`, `app/chamados/models.py:1586`, `app/chamados/models.py:1590`, `app/chamados/models.py:1595`.

`RegularizacaoExterna` é entidade auxiliar ligada a chamado, com status próprios aguardando/confirmada/recusada/materializada/cancelada. Seu escritor materializada é `materializar_regularizacao_externa`; recusada volta à supervisão e deixa o chamado aberto. Não confundir esses valores com terminais de ChamadoColaborador: `app/chamados/models.py:2744`, `app/chamados/models.py:2745`, `app/chamados/models.py:2763`, `app/chamados/models.py:2772`, `app/chamados/services/regularizacao_ext.py:197`, `app/chamados/services/regularizacao_ext.py:200`, `app/chamados/services/regularizacao_ext.py:261`, `app/chamados/services/regularizacao_ext.py:266`.

A allowlist do esmeril ainda documenta bypass de fechamento de disputa no desligamento, mas o código atual já o migrou para `DisputaSupervisao.fechar`; o comentário da allowlist está desatualizado: `bin/esmeril_chamado_censo.py:64`, `app/colaboradores/services/desligamento.py:140`, `app/colaboradores/services/desligamento.py:147`, `app/colaboradores/services/desligamento.py:150`.

Método executado: AST read-only de todos os Python da árvore para atribuição/AnnAssign/AugAssign a status_local, kwargs em verbos ORM e bulk_update/setattr literais; leitura dos merges de dict e rastreamento manual das portas/chamadores. O AST literal sozinho não encontra `_def`/`**lookup`, execução dinâmica, SQL externo, comandos feitos em shell/produção, plugins ou dados ausentes do snapshot; exaustividade dessas superfícies externas permanece `[nao verificado]`. O varredor existente também declara suas limitações de atribuição e balde não atribuível: `app/core/censo_escritas.py:6`, `app/core/censo_escritas.py:43`, `app/core/censo_escritas.py:49`, `bin/esmeril_chamado_censo.py:99`, `bin/esmeril_chamado_censo.py:113`.

## 30/10 18:17 · sessao 454c1076e906 · - · - · origem=Ronald (tmux)

Concordo com os dois bloqueios identificados:

- **Ata `concorde` sem batida pode continuar produzindo falta antes do encerramento.** `_vv` desliga `futuro` em `app/colaboradores/services/calendario.py:822`; `_classificar` chega a `falta` em `:148` e descarta o `ok` de `concorde` sem batida em `:176`. O comentário “veredito lavrado decide” não corresponde ao comportamento desse ramo (`:820`).
- **Régua suspeita impede o rótulo da folha no dia passado cego.** `regua_suspeita` vence `retido_folha` em `app/colaboradores/services/calendario.py:139`; a seleção do rótulo da folha só ocorre para status `sem_certidao` em `:899`. Assim, mesmo recebendo `sem_celula`, a tela pode continuar dizendo `escala_suspeita` (`:834`, `:913`).

Censo dos campos diretamente afetados:

| Campo | Escritor | Leitores |
|---|---|---|
| `_agora` / `hoje` | `contexto_calendario`, `app/colaboradores/services/calendario.py:258` | `dias_encerrados` (`:712`), recortes `_em_curso`/`_sem_fim` (`:715`, `:723`), retenção (`:726`), `futuro` (`:822`), oferta de resolver (`:945`). |
| Motivo de não encerramento | `_dia_encerrado_motivo_puro`, `app/ponto/turnos.py:2088`; publicação em `dias_encerrados`, `:2139` | Calendário: `_em_curso` e `_sem_fim`, `app/colaboradores/services/calendario.py:714`. |
| Veredito persistido | `CelulaDia.lavrar_veredito` delega à porta, `app/escala/models.py:1775` | Calendário consulta e indexa em `app/colaboradores/services/calendario.py:525`; folha consulta em `app/folha/export.py:1188` e julga em `:1215`. |
| `retido_folha` | Motivo produzido por `motivos_retencao_celula`, `app/folha/export.py:1200`; calendário apenas filtra `sem_celula`, `app/colaboradores/services/calendario.py:727` | `_classificar`, `:141`; rótulo, `:899`. |
| `status` | `_classificar`, `app/colaboradores/services/calendario.py:108`; chamada única em `:814` | Grade (`app/templates/colaboradores/partials/_calendario_grade.html:69`), resumo (`calendario.py:949`), oferta resolver (`:204`), portas `status_do_dia`/`contar_por_status` (`:219`, `:231`). |
| `status_rotulo` | `app/colaboradores/services/calendario.py:899` | Grade, `app/templates/colaboradores/partials/_calendario_grade.html:99`; ramo cego novo em `:107`. |
| `dia_pago` / rubricas | Calendário lê `_linhas_do_periodo` em `app/colaboradores/services/calendario.py:385`, `_rubricas_do_dia` em `:800`; publicação `:922` | `_classificar` recebe desvio em `:827`; badges em `:854`; a mudança não escreve `DiaPago`. |

Custo adicional por montagem, inferido dos caminhos SQL: **uma consulta de vínculos sempre**, apesar da alimentação de turnos e células (`app/ponto/turnos.py:2130`). Havendo dias passados sem fim e sem veredito, mais uma chamada à folha (`app/colaboradores/services/calendario.py:726`), que consulta células (`app/folha/export.py:1188`) e o carregador de vínculos (`:1194`); esse carregador possui a consulta principal e eventual consulta de geradoras faltantes (`app/escala/alimentacao.py:116`, `:125`). Tempo e número efetivo nesta execução: **[não verificado]**.

O custo se multiplica pelos calendários efetivamente montados: perfil (`app/colaboradores/views.py:1946`), drawer (`app/colaboradores/services/drawer.py:121`), painel de furos dentro do laço de colaboradores (`app/relatorios/furos.py:139`, `:184`) e vigia pela porta de um dia (`app/chamados/services/vigia_fio.py:294`; `app/colaboradores/services/calendario.py:213`).

O relógio novo é compartilhado entre encerramento e retenção (`app/colaboradores/services/calendario.py:258`, `:712`, `:726`), mas a montagem da palavra continua chamando `autoridade_do_periodo` e `folha_manda` sem passar esse instante (`:676`, `:689`). Portanto, “uma borda de relógio” está comprovada apenas para a parte nova; toda a montagem compartilhar o mesmo relógio: **[não verificado]**.

O template dá precedência à `palavra_dia` sobre o `status_rotulo` (`app/templates/colaboradores/partials/_calendario_grade.html:98`). Não provei um caso alcançável em que isso esconda o rótulo cego novo: **[não verificado]**.

## 30/10 18:16 · sessao 2ef9bfc27055 · CODEX-CONTEXTO-DA-CASA-AB2 · verificador · origem=Code

**Veredito: REPROVADA como cumprimento integral do aval das 15:3x.** O caso principal foi corrigido, mas a resposta da folha ainda pode desaparecer na classificação, e o rótulo continua sendo escrito pelo calendário. (`app/docs/PROMPTS.md:1681`; `app/colaboradores/services/calendario.py:141`; `app/colaboradores/services/calendario.py:899`)

1. **P2 — A retenção é consultada, mas pode ser descartada sem vínculo ativo.** Considere dia passado, célula `trabalha=True`, DNA sem `hf`, nenhum veredito e vínculo histórico encerrado cobrindo a data. O juiz de encerramento considera os vínculos históricos; a folha retém essa célula como `sem_celula`. Porém o calendário busca exclusivamente `ativa=True`: sem vínculo ativo, mantém `eh_trabalho=None`. O novo `retido_folha` só é lido dentro de `trabalho is True`; o resultado vira `sem_dados`, com “Sem dados”, apesar da resposta autoritativa já carregada. (`app/ponto/turnos.py:2130`; `app/escala/alimentacao.py:253`; `app/folha/export.py:1237`; [calendario.py:351](/home/ronald/codex_ws/38fe9889eed26fb04bf16edb63c20fb9a625a2ff/app/colaboradores/services/calendario.py:351); `app/colaboradores/services/calendario.py:768`; `app/colaboradores/services/calendario.py:783`; `app/colaboradores/services/calendario.py:134`; `app/colaboradores/services/calendario.py:158`)

   A guarda por vínculo ativo já existia na base; a insuficiência desta obra é conectar o novo resultado da folha atrás dessa guarda. A correção precisa preservar A2 e usar a autoridade do dia, sem promover `retido_folha` a sobrescritor indiscriminado de status. (`6c37ad84:app/colaboradores/services/calendario.py:341`; `6c37ad84:app/colaboradores/services/calendario.py:748`; `app/colaboradores/services/calendario.py:124`; `app/colaboradores/services/calendario.py:834`)

2. **P2 — Há fonte única do motivo, mas ainda não do rótulo exigido.** `motivos_retencao_celula` devolve `data/motivo/celula_id/veredito`; a frase “Dia sem julgamento do cartorio…” nasce em `ROTULO_CEGO_FOLHA`, no calendário. Portanto a tela lê o **código** da autoridade e escreve sua própria interpretação textual; não lê um rótulo fornecido pela mesma autoridade da folha, como pede o aval. Isso não constitui outro juiz de retenção, mas deixa incompleto o contrato literal do rótulo compartilhado. (`app/folha/export.py:1203`; [calendario.py:64](/home/ronald/codex_ws/38fe9889eed26fb04bf16edb63c20fb9a625a2ff/app/colaboradores/services/calendario.py:64); `app/colaboradores/services/calendario.py:726`; `app/colaboradores/services/calendario.py:899`; `app/docs/PROMPTS.md:1681`)

3. **P3 — O censo deixou vocabulário antigo vivo.** O calendário agora usa `previsto` também para hoje e para turno de dia anterior ainda não encerrado, mas a legenda entregue ao copiloto continua definindo “Previsto” como “dia futuro previsto pela escala”. O teste dessa legenda compara os nomes dos itens, não suas explicações; essa contradição passa. (`app/colaboradores/services/calendario.py:714`; `app/colaboradores/services/calendario.py:822`; `app/colaboradores/legenda_calendario.py:18`; `app/colaboradores/legenda_calendario.py:45`; `app/colaboradores/tests/test_legenda_calendario.py:14`)

O aval das **13:3x está atendido no caminho coberto**: o calendário pergunta ao juiz existente, reaproveita seus turnos e células, mantém `previsto` enquanto o turno não encerrou e respeita veredito já lavrado. O tratamento distinto de passado `sem_previsto` corresponde ao complemento das 15:3x; não classificaria essa distinção, isoladamente, como juiz novo ou teto indevido por data. (`app/colaboradores/services/calendario.py:712`; `app/colaboradores/services/calendario.py:714`; `app/colaboradores/services/calendario.py:722`; `app/colaboradores/services/calendario.py:822`; `app/docs/PROMPTS.md:1676`; `app/docs/PROMPTS.md:1681`)

O circuito dos campos afetados é este:

| Campo | Quem escreve/produz | Quem lê |
|---|---|---|
| `ativa`, vigência e `tipo_escala` do vínculo | Portas de criação, encerramento e correção: `app/colaboradores/services/vinculo.py:797`, `:832`, `:842` | Juiz do vínculo: `app/escala/alimentacao.py:237`; carga do encerramento: `app/ponto/turnos.py:2130`; calendário: `app/colaboradores/services/calendario.py:351` |
| Horários do template; `dna.marcos` | Cadastro: `app/escala/services/cadastro_tipo.py:223`; congelamento: `app/ponto/management/commands/gerar_celulas.py:81` | `marcos_do_dia`: `app/escala/models.py:1094`; `marcos_turno`/`fim_turno`: `app/escala/servico_jornada.py:69`, `:92` |
| `CelulaDia.trabalha/dna/escala_geradora` | Porta da célula: `app/ponto/portas/celula.py:247`, `:260` | Calendário: `app/colaboradores/services/calendario.py:785`, `:829`; juiz do vínculo: `app/escala/alimentacao.py:237`; folha: `app/folha/export.py:1237` |
| `veredito/via/em/protestos` | `lavrar_veredito`: `app/ponto/portas/celula.py:691` | `_ver_map` e classificador: `app/colaboradores/services/calendario.py:525`, `:806`, `:830`; retenção: `app/folha/export.py:1215` |
| `Turno.data_turno/batidas/aberto` | Geometria do turno: `app/ponto/turnos.py:310` | Índice do calendário: `app/colaboradores/services/calendario.py:316`; alimentação do juiz: `:713`; classificação: `:826` |
| Motivo de não encerramento; retorno booleano ou `motivos=True` | Mesmo corpo puro: `app/ponto/turnos.py:2097`; adaptação do retorno: `:2139` | `_em_curso` e `_sem_fim`: `app/colaboradores/services/calendario.py:714`, `:722` |
| `futuro` | Campo interno montado em `app/colaboradores/services/calendario.py:822` | `_classificar`: `app/colaboradores/services/calendario.py:135`, `:155` |
| Motivo da retenção → `_cego_folha` → `retido_folha` | Folha: `app/folha/export.py:1200`; transporte: `app/colaboradores/services/calendario.py:727`, `:834` | Classificador: `app/colaboradores/services/calendario.py:141`; composição do rótulo: `:899` |
| `status/status_rotulo` | `_classificar` e montagem: `app/colaboradores/services/calendario.py:189`, `:898` | Grade: `app/templates/colaboradores/partials/_calendario_grade.html:99`; resumo: `app/colaboradores/services/calendario.py:949`; portas curtas: `:228`, `:239`; Resolver dia: `:205` |
| `veredito_dia/palavra_dia/cor_dia` | Trilha separada de `dia_decidido.do_dia`: `app/ponto/services/dia_decidido.py:399`; transporte: `app/colaboradores/services/calendario.py:887` | Template, antes dos ícones de status: `app/templates/colaboradores/partials/_calendario_grade.html:68`, `:98` |

A mudança alcança calendário da ficha, drawer, relatório de furos e, indiretamente, o vigia que consulta “Resolver dia”; portanto o custo e a alteração de status não ficam restritos à página individual. (`app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:184`; `app/chamados/services/vigia_fio.py:294`)

Quanto aos **relógios**:

- O novo caminho está coerente: captura `_agora` uma vez, deriva `hoje` desse instante, passa `_agora` ao juiz e `hoje` à retenção. (`app/colaboradores/services/calendario.py:258`; `:712`; `:726`)
- O juiz continua combinando turno aberto vivo com fim previsto; o fim vem do DNA quando disponível, com encadeamento dos marcos através da meia-noite. A aberta expira pela janela do template mais a margem existente de quatro horas. (`app/ponto/turnos.py:1981`; `:1997`; `:2097`; `app/escala/servico_jornada.py:81`)
- Isso não torna todos os relógios da montagem únicos: o prazo de desfazer captura outro `now()`, e a trilha da palavra chama autoridades próprias. São caminhos já existentes, não prova de regressão deste diff. (`app/colaboradores/services/calendario.py:343`; `:676`; `:689`; `app/relatorios/cartao_pela_celula.py:40`)

O **custo acrescentado por montagem** é sempre um SELECT de vínculos em `dias_encerrados`; os turnos e células já carregados são reaproveitados. Havendo passado `sem_previsto` sem veredito, soma-se uma chamada à folha, com consulta de células e carga de vínculos. Não há uma consulta à folha por dia. (`app/ponto/turnos.py:2129`; `:2130`; `:2132`; `app/colaboradores/services/calendario.py:724`; `app/folha/export.py:1188`; `:1194`)

Em CPU, há percurso dos dias com seleção entre vínculos, além dos turnos; o filtro `r['data'] in _sem_fim` ainda usa lista, podendo acrescentar trabalho quadrático no ramo cego. No relatório de frota, esse custo se repete por calendário montado. (`app/ponto/turnos.py:2132`; `:2136`; `app/escala/alimentacao.py:253`; `app/colaboradores/services/calendario.py:727`; `app/relatorios/furos.py:184`)

**Os testes mordem os casos centrais, mas não certificam esses pontos restantes.** [nao verificado]

- Há vínculo real, antes do turno, meia-noite, turno encadeado além de ontem, depois do fim, veredito lavrado e comparação com a folha. A sentinela no rótulo verifica o consumo pelo template nos três layouts. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:29`; `:42`; `:47`; `:57`; `:61`; `:65`; `:92`; `:104`)
- A fixture permanece com vínculo ativo e sem criação de batidas; não cobre o achado 1 nem a entrada nova `turnos=` com conteúdo. C1 possui batidas, mas suas chamadas não exercitam esse argumento. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:20`; `app/ponto/tests/test_c1_dias_encerrados.py:28`; `:51`; `:66`)
- A sentinela prova que o template lê o campo; não prova que o rótulo veio da autoridade da folha. O teste de cobertura também admite que A2 decide antes da cobertura que ele aparenta verificar. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:108`; `:134`)
- O teste de custo limita o **delta** do ramo cego a duas consultas e compara um contra oito dias; custo comum adicional passa nessa comparação. O selo geral mantém teto de 55 consultas e mede o segundo acesso aquecido. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`; `app/core/tests/test_selo_performance.py:249`; `:332`)

Na avaliação isolada dos corpos reais, em memória e com dependências substituídas, confirmei `sem_celula` na folha e `sem_dados` no classificador quando `eh_trabalho=None`; isso verifica a perda do motivo, sem constituir reprodução completa com Django/banco. (`app/folha/export.py:1237`; `app/colaboradores/services/calendario.py:134`; `:158`) Resultado integral da suíte nesta árvore e incidência em produção: **[não verificado]**.

## 30/10 18:14 · sessao 0e744b1d9c96 · CODEX-CONTEXTO-DA-CASA-AB2 · verificador · origem=Code

**REPROVADA: o aval das 15:3x ainda não fecha.** O calendário consulta a folha, mas pode descartar o motivo recebido e exibir outra palavra. O aval exige que esse dia leia o rótulo do mesmo juiz da folha (`app/docs/PROMPTS.md:1681`; `app/colaboradores/services/calendario.py:726`, `:141`, `:158`).

1. **Dia histórico com vínculo encerrado válido perde o rótulo da folha.** Contraexemplo: dia passado, vínculo encerrado cuja vigência cobre o dia, nenhum vínculo ativo, célula `trabalha=True`, DNA contendo `hi` sem `hf`, `veredito=None`. A autoridade aceita esse vínculo histórico; o calendário procura somente `ativa=True` e deixa `eh_trabalho=None` (`app/escala/alimentacao.py:237`, `:250`; `app/colaboradores/services/calendario.py:351`, `:768`, `:783`).

   Nesse caso, `dias_encerrados` responde `sem_previsto`; a folha responde `sem_celula`; o calendário recebe o motivo, mas `_classificar` só lê `retido_folha` dentro de `trabalho is True`. Com `None`, retorna **`sem_dados`**, publicando “Sem dados”. Confirmei esse retorno executando o corpo real de `_classificar`, extraído por AST em memória (`app/ponto/turnos.py:2102`, `:2130`; `app/folha/export.py:1237`; `app/colaboradores/services/calendario.py:727`, `:134`, `:141`, `:158`, `:899`).

2. **A régua suspeita também esconde o motivo recebido.** Com trabalho previsto, sem batida, sem fim e sem veredito, `regua_suspeita=True` vence `retido_folha='sem_celula'`: retorna **`escala_suspeita`**. O template mostra a explicação da régua, sem o motivo da folha. A precedência já existia, mas a integração acrescentada não satisfaz o rótulo exigido nesse cruzamento (`app/colaboradores/services/calendario.py:139`, `:141`, `:727`; `app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/docs/PROMPTS.md:1681`).

3. **Há leitor de significado não migrado.** `previsto` agora abrange hoje e dias anteriores ainda não encerrados, mas a legenda fornecida ao copiloto continua dizendo **“dia futuro previsto pela escala”**. O teste da legenda compara apenas os nomes, portanto não acusa esse significado errado (`app/colaboradores/services/calendario.py:714`, `:822`; `app/colaboradores/legenda_calendario.py:18`; `app/api/views_mensageria.py:1764`; `app/colaboradores/tests/test_legenda_calendario.py:14`).

O caminho principal das 13:3x está implementado pela autoridade existente: a montagem entrega turnos, células e instante a `dias_encerrados`; o núcleo continua verificando turno vivo e fim previsto. Não há juiz temporal novo nesse trecho nem limite artificial a hoje/ontem (`app/docs/PROMPTS.md:1676`; `app/colaboradores/services/calendario.py:712`; `app/ponto/turnos.py:2097`, `:2105`, `:2137`).

O rastro dos campos lidos ou alterados pelo diff é este:

| Campo ou alimentação | Quem escreve/produz | Quem lê |
|---|---|---|
| `CelulaDia.trabalha`, `dna.marcos`, `escala_geradora` | Geração/regeneração pela porta da célula (`app/ponto/portas/celula.py:6`, `:247`, `:595`). | `eh_dia_trabalho`, `marcos_do_dia`, seleção do vínculo; folha lê `trabalha` (`app/escala/models.py:1066`, `:1094`; `app/escala/alimentacao.py:237`; `app/folha/export.py:1237`). |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata` | `lavrar_veredito`, recebendo julgamento do cartório/admin (`app/ponto/portas/celula.py:647`, `:671`, `:689`). | Calendário monta `_ver_map`; retentor lê veredito/protestos e marcos da ata (`app/colaboradores/services/calendario.py:525`; `app/folha/export.py:1215`, `:1228`, `:1124`). |
| Vínculo: `ativa`, `data_inicio`, `data_fim`, `tipo_escala` | Cadastro e portas de vínculo; encerramento grava `ativa=False` e `data_fim` (`app/colaboradores/services/vinculo.py:167`, `:780`, `:797`). | Calendário escolhe ativo; juíza escolhe vínculo do dia; folha verifica cobertura (`app/colaboradores/services/calendario.py:351`; `app/ponto/turnos.py:2130`, `:2137`; `app/folha/export.py:1193`). |
| Turnos: `data_turno`, `aberto`, batidas | `turnos_do_colab`/pareador; batidas entram pela porta e retratadas ficam fora da apuração (`app/ponto/registro_batida.py:94`; `app/ponto/turnos.py:1848`, `:1858`). | Calendário reutiliza a lista já montada; juíza agrupa por dia e consulta turno vivo (`app/colaboradores/services/calendario.py:316`, `:713`; `app/ponto/turnos.py:2132`, `:2097`). |
| `_motivo_aberto`, `_em_curso`, `_sem_fim` | Resposta da juíza e seleção transitória da montagem (`app/colaboradores/services/calendario.py:712`, `:714`, `:722`). | Formação de `futuro` e seleção da consulta à folha (`app/colaboradores/services/calendario.py:724`, `:822`). |
| Motivo/data da retenção → `_cego_folha` → `retido_folha` | Juiz `motivos_retencao_celula`; montagem filtra somente `sem_celula` (`app/folha/export.py:1200`, `:1237`; `app/colaboradores/services/calendario.py:727`). | `_classificar` e composição de `status_rotulo`, sujeitos aos escapes acima (`app/colaboradores/services/calendario.py:141`, `:834`, `:899`). |
| `futuro`, `status`, `status_rotulo` | Montagem produz `futuro`; `_classificar` produz status; montagem compõe rótulo (`app/colaboradores/services/calendario.py:814`, `:822`, `:898`). | Grade, resumo, `status_do_dia`, `contar_por_status` e decisão de oferecer Resolver dia (`app/templates/colaboradores/partials/_calendario_grade.html:69`, `:99`; `app/colaboradores/services/calendario.py:204`, `:219`, `:231`, `:948`). |
| `resolver_dia_oferecido` | Juiz existente, após status pronto (`app/colaboradores/services/calendario.py:199`, `:944`). | Template e vigia do fio; portanto a mudança repercute além da cor da tela (`app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:294`). |

A montagem também é consumida pela ficha, drawer e painel de furos; as portas curtas recompõem o calendário para responder status ou disponibilidade da ação (`app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:184`; `app/colaboradores/services/calendario.py:213`, `:225`).

**Relógios:** `_agora` e `hoje` vêm do mesmo instante; esse instante segue à juíza e essa data segue ao retentor. A folha mantém seu próprio critério de dia passado e a exceção do dia da demissão, agora explicitamente consultados pelo calendário (`app/colaboradores/services/calendario.py:258`, `:712`, `:726`; `app/folha/export.py:1211`, `:1237`).

O encerramento lê o fim do DNA, com `agora >= fim`; a aberta expira pelo template, com margem padrão de quatro horas e comparação estrita `agora > expiração`. Essa diferença é herdada e está documentada no próprio juiz; não nasceu neste diff (`app/ponto/turnos.py:1992`, `:1997`, `:2007`, `:2039`, `:2102`).

**Custo por montagem:** há **uma consulta adicional incondicional** para todos os vínculos do colaborador, reaproveitando turnos e células. Havendo candidatos cegos, acrescenta-se uma chamada à folha pelo intervalo entre o primeiro e o último candidato, com consultas de células e vínculos. Portanto, nesse caminho, são **+1 consulta no caso comum e até +3 com retenção**, por montagem; tempo real e custo total em produção **[nao verificado]** (`app/ponto/turnos.py:2129`, `:2130`, `:2132`; `app/colaboradores/services/calendario.py:722`, `:726`; `app/folha/export.py:1188`, `:1194`).

**Os testes mordem parte relevante, mas não esses escapes:** [nao verificado]

- Há casos reais para antes do turno, meia-noite, turno encadeado anterior a ontem, falta depois do fim, veredito lavrado, folga, ausência e isenção (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42`, `:47`, `:57`, `:61`, `:65`, `:112`, `:123`, `:132`, `:146`).
- O RED cego verifica a premissa na juíza e na folha; o template é renderizado com sentinela em `status_rotulo`. Isso verifica leitura efetiva do campo (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:85`, `:97`, `:104`).
- A fixture usa vínculo ativo e não cruza régua suspeita: deixa passar os dois escapes do rótulo. O teste de custo compara 0/1/8 cegos, mas subtrai uma base que já contém a consulta adicional fixa (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:23`, `:29`, `:169`, `:177`).
- A propriedade C1 com células alimentadas não contém batidas para provar o pareamento com essa alimentação; o novo caminho `turnos=` também não recebe uma lista não vazia nos casos novos (`app/ponto/tests/test_c1_dias_encerrados.py:62`, `:66`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:19`, `:42`).

Há ainda **defeito herdado de contrato**, separado das alterações: quando `turnos` é omitido, `dias_encerrados` encaminha células com chave `(colaborador_id, data)` a `turnos_do_colab`, que exige chave `data` e a embrulha novamente. O cartório usa esse caminho. O calendário evita esse repasse ao fornecer os turnos, mas a inconsistência continua na árvore (`app/ponto/turnos.py:2129`, `:2132`, `:1852`, `:1928`; `app/ponto/services/cartorio.py:853`).

**Suite, RED contra a base e smoke deste hash: [nao verificado].** O bloqueador demonstrado é o motivo da folha recebido e descartado pelo classificador; consultar a autoridade não basta enquanto o leitor continua substituindo sua palavra (`app/colaboradores/services/calendario.py:727`, `:134`, `:139`, `:158`; `app/docs/PROMPTS.md:1681`).

## 30/10 18:14 · sessao fd8816f3fb67 · CODEX-CONTEXTO-DA-CASA-AB2 · verificador · origem=Code

**Veredito: o defeito principal está corrigido, mas eu não certificaria o cumprimento integral dos avais.** A árvore consulta a juíza existente e trata o passado sem fim previsto; ainda faltam o contrato literal do rótulo compartilhado e testes que protejam a dependência da folha. (`app/colaboradores/services/calendario.py:712`, `:714`, `:726`, `:899`; `app/docs/PROMPTS.md:1681`)

**O que está errado ou incompleto:**

1. **O rótulo continua sendo escrito pela tela.** A folha devolve `data`, `motivo`, `celula_id` e `veredito`. O calendário aproveita `motivo='sem_celula'`, mas cria a frase `ROTULO_CEGO_FOLHA` e monta `status_rotulo` localmente. Portanto, compartilha o julgamento da retenção, mas não lê um rótulo produzido pela mesma autoridade, como pede literalmente o aval 15:3x. Isso não constitui outro juiz de retenção; constitui um contrato de apresentação ainda local. (`app/folha/export.py:1200`, `:1237`; `app/colaboradores/services/calendario.py:64`, `:727`, `:899`; `app/docs/PROMPTS.md:1681`)

2. **Os testes não protegem essa dependência da folha.** Por inspeção, substituir a consulta por `_cego_folha = {d: 'sem_celula' for d in _sem_fim}` preservaria as expectativas: o caso cego espera esse código fixo; os casos de veredito, folga, ausência e isenção escapam por outros ramos; o custo cairia para zero e continuaria passando. Mutante não executado. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:97`, `:113`, `:124`, `:133`, `:147`, `:177`; `app/colaboradores/services/calendario.py:722`)

3. **A prova de custo cobre somente uma parcela.** O teste desconta a montagem sem dias cegos da própria árvore. Assim, esconde a consulta adicional incondicional de vínculos feita por `dias_encerrados`. Também não mede CPU: a filtragem das retenções procura cada data em uma lista, com custo potencial `O(R × K)`. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:169`, `:177`; `app/ponto/turnos.py:2130`; `app/colaboradores/services/calendario.py:722`, `:727`)

4. **Um consumidor do significado de `previsto` ficou para trás.** A legenda entregue ao copiloto ainda explica “dia futuro previsto pela escala”, embora agora `previsto` inclua hoje e turnos iniciados em datas anteriores. O teste da legenda compara os nomes dos rótulos, não essa descrição; ele não pega a contradição. (`app/colaboradores/legenda_calendario.py:18`; `app/api/views_mensageria.py:1764`; `app/colaboradores/services/calendario.py:822`; `app/colaboradores/tests/test_legenda_calendario.py:14`)

**O núcleo dos dois avais está presente.** A pergunta usa `[data_ini, data_fim]` inteiro, com os mesmos turnos já carregados: não há recorte novo em hoje/ontem. O passado `sem_previsto` sai de `_em_curso`; somente os candidatos sem veredito consultam a folha. A retenção entra como fato em `_classificar`, preservando a precedência humana e o veredito, em vez de sobrescrever o resultado depois. (`app/colaboradores/services/calendario.py:712`, `:714`, `:722`, `:834`; `:124`, `:141`, `:183`)

Quem escreve e quem lê os campos envolvidos:

| Campo | Escritor/produtor | Leitores relevantes |
|---|---|---|
| `CelulaDia.trabalha`, `dna`, `escala_geradora` | Geração/regeneração pela porta; `montar_dna` produz o blob. (`app/ponto/portas/celula.py:243`, `:260`, `:593`, `:612`; `app/ponto/management/commands/gerar_celulas.py:73`) | `marcos_do_dia`, `vinculo_do_dia`, calendário e retenção. (`app/escala/models.py:1094`; `app/escala/alimentacao.py:237`; `app/colaboradores/services/calendario.py:785`, `:829`; `app/folha/export.py:1237`) |
| `veredito`, `veredito_via`, `veredito_em`, `protestos` | Porta `lavrar_veredito`. (`app/ponto/portas/celula.py:691`) | Calendário carrega `_ver_map`, impede previsão sobre decisão lavrada e classifica; folha lê veredito/protestos. (`app/colaboradores/services/calendario.py:525`, `:530`, `:822`, `:830`; `app/folha/export.py:1215`, `:1228`) |
| Turnos: `data_turno`, `batidas`, `aberto` | Pareador produz `Turno`; `turnos_do_colab` fornece a janela. (`app/ponto/turnos.py:321`, `:1190`, `:1848`) | Calendário agrupa e reaproveita na juíza; também determina presença e turno aberto. (`app/colaboradores/services/calendario.py:316`, `:713`, `:731`, `:825`) |
| `_agora`, `hoje` | Contexto captura o instante e deriva a data local. (`app/colaboradores/services/calendario.py:258`) | Juíza, folha, `futuro` e oferta de resolução. (`app/colaboradores/services/calendario.py:712`, `:726`, `:822`, `:945`) |
| Motivo de não encerramento | `_dia_encerrado_motivo_puro`; `dias_encerrados` publica motivo ou booleano. (`app/ponto/turnos.py:2097`, `:2105`, `:2139`) | Calendário deriva `_em_curso` e `_sem_fim`. (`app/colaboradores/services/calendario.py:714`, `:722`) |
| Motivo de retenção / `_cego_folha` | Folha escreve o motivo; calendário filtra a resposta. (`app/folha/export.py:1200`, `:1237`; `app/colaboradores/services/calendario.py:727`) | Fato `retido_folha`, classificador e rótulo. (`app/colaboradores/services/calendario.py:834`, `:141`, `:899`) |
| `futuro` | Contexto: data futura ou dia ainda em curso sem veredito. (`app/colaboradores/services/calendario.py:822`) | Somente os ramos do classificador. É um fato transitório, não campo persistido. (`app/colaboradores/services/calendario.py:135`, `:155`, `:814`) |
| `status` | `_classificar`; contexto publica o resultado. (`app/colaboradores/services/calendario.py:189`, `:898`) | Grade, resumo, portas de consulta e `resolver_dia_oferecido`. (`app/templates/colaboradores/partials/_calendario_grade.html:69`; `app/colaboradores/services/calendario.py:204`, `:228`, `:239`, `:949`) |
| `status_rotulo` | Contexto, usando constantes locais e motivo da folha. (`app/colaboradores/services/calendario.py:899`) | Grade, inclusive o novo fallback. (`app/templates/colaboradores/partials/_calendario_grade.html:99`, `:105`, `:107`) |
| `resolver_dia_oferecido` | Juiz existente lê o status e carimba cada dia. (`app/colaboradores/services/calendario.py:199`, `:945`) | Template e vigia; portanto a mudança de status também alcança a oferta de ação e sua contagem. (`app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:294`) |

Os chamadores que recebem esses efeitos são perfil, drawer, relatório de furos e vigia pela porta de um dia; `status_do_dia` e `contar_por_status` também remontam o contexto. (`app/colaboradores/views.py:1945`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:184`; `app/colaboradores/services/calendario.py:213`, `:225`, `:236`)

**Relógios:** `_agora` é compartilhado pela decisão de encerramento e pelo cálculo de `hoje`; a folha recebe esse `hoje` explicitamente. O fim previsto vem dos marcos encadeados, inclusive atravessando mais de uma meia-noite; turno aberto vivo pode adiar o encerramento. O segundo `now()` do calendário serve ao prazo de desfazer retratação, outra finalidade. (`app/colaboradores/services/calendario.py:258`, `:343`, `:712`, `:726`; `app/escala/servico_jornada.py:80`; `app/ponto/turnos.py:2097`)

**Custo adicional por montagem:** uma consulta de vínculos sempre; havendo candidatos cegos, mais uma consulta de células e uma de vínculos na folha, salvo saídas antecipadas. Turnos e células são reutilizados na primeira consulta à juíza, evitando recarga por dia. O custo ocorre novamente em cada montagem das portas curtas. O total efetivamente medido neste hash permanece **[não verificado]**. (`app/ponto/turnos.py:2129`; `app/folha/export.py:1174`, `:1184`, `:1188`, `:1194`; `app/colaboradores/services/calendario.py:213`, `:225`)

Os testes **mordem os resultados principais**: antes do turno, madrugada, depois do fim, turno encadeado além de ontem, decisão lavrada, passado cego e consumo do campo pelo template mediante sentinela. O teste C1 também distingue DNA `10–14` de template `19–07`. Não cobrem, porém, a dependência obrigatória da folha nem o caminho `turnos=` com batidas nessa nova prova C1. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42`, `:47`, `:57`, `:61`, `:65`, `:92`, `:108`; `app/ponto/tests/test_c1_dias_encerrados.py:62`)

Há ainda um defeito de contrato no caminho global: sem `turnos=`, `dias_encerrados` repassa células com chave `(colaborador_id, data)` a uma função que espera chave `data`, criando chaves aninhadas no adaptador. O calendário evita esse caminho passando turnos; o cartório o utiliza. A comparação indica que é herdado da base, não introduzido nesta alteração **[não verificado por execução]**. (`app/ponto/turnos.py:2113`, `:2132`, `:1852`, `:1928`; `app/ponto/services/cartorio.py:853`)

Execução das suítes neste hash: **[não verificado]**. A análise foi estática; os testes de integração criam e atualizam registros. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:20`, `:165`, `:170`)

## 30/10 18:14 · sessao d4daaabd58b0 · - · - · origem=Ronald (tmux)

Contraprova confirmada por execução das funções reais extraídas por AST, em memória, sem Django/DB/escrita:

```text
_classificar(trabalho=True, sem_batida=True,
             futuro=False, veredito='concorde') -> falta
_classificar(..., veredito='coberta')            -> falta
_classificar(..., futuro=True, veredito=None)   -> previsto
```

Qualquer `_vv` cancela a proteção temporal (`app/colaboradores/services/calendario.py:822`); o ramo sem batida escolhe falta (`:148`), e o mapa `concorde -> ok` é deliberadamente apagado por ausência de batida (`:176`). O teste de veredito existente cobre somente `nunca_bateu`, portanto não pega esse caso (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:65`).

O estado `concorde` com turno não encerrado e sem batida nasce do próprio escritor: executei também `supra_juiz.classificar_dia` real para dia 14/10, hoje 15/10, adesão anterior, previsto 720, realizado 0, batidas 0 e `turno_encerrado=False`; resultado `[]`. `derivar_veredito([])` real devolveu `concorde`. Referências: `app/ponto/supra_juiz.py:279`, `app/ponto/supra_juiz.py:349`, `app/escala/models.py:1592`. O cartório normal repassa essa proteção temporal e lavra os códigos (`app/ponto/services/cartorio.py:1080`, `:1117`, `:1409`). Assim, **o cenário mais forte é ontem com turno noturno ainda em curso**; o calendário pode acusar falta quando a juíza ainda nega encerramento. Hoje antes do primeiro marco tem guarda adicional do cartório, por isso não alego nascimento medido desse estado em produção (`app/ponto/services/cartorio.py:1048`).

Também fiz mutação AST que remove apenas `retido_folha` do classificador. O caso cego muda de `sem_certidao` para `falta`; portanto a asserção nova discrimina esse ramo (`app/colaboradores/services/calendario.py:141`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:99`). Régua suspeita, batida presente, A2 e isento permanecem iguais com essa mutação, por precedência (`app/colaboradores/services/calendario.py:124`, `:137`, `:139`, `:159`).

Suite Django, RED contra o hash base e mutação da renderização: [não verificado]. [nao verificado]

## 30/10 18:13 · sessao 0564966d58eb · - · - · origem=Ronald (tmux)

Dois pontos para consolidar, sem execução de Django/banco:

1. **Cumprimento literal incompleto do rótulo compartilhado.** O calendário consulta o juiz da folha, mas ele retorna `data/motivo/celula_id/veredito`, sem rótulo (`app/folha/export.py:1203`). A frase “Dia sem julgamento…” nasce numa constante nova do calendário (`app/colaboradores/services/calendario.py:64`) e é composta localmente (`app/colaboradores/services/calendario.py:899`). Portanto há SSOT do **motivo**, mas não um rótulo produzido pelo juiz da folha. O aval exige a leitura desse rótulo (`app/docs/PROMPTS.md:1681`). Não é correto chamar isso de novo juiz de retenção: a decisão vem de `motivos_retencao_celula` (`app/colaboradores/services/calendario.py:726`).

2. **O motivo calculado pode ser descartado quando há batida.** `_sem_fim` e `_cego_folha` não restringem o universo a dias sem batida (`app/colaboradores/services/calendario.py:722`, `app/colaboradores/services/calendario.py:727`), mas `retido_folha` só participa do ramo sem batida e de trabalho (`app/colaboradores/services/calendario.py:133`, `app/colaboradores/services/calendario.py:141`). Para um passado com par fechado, DNA sem `hf` e nenhum veredito, o juiz pode retornar `sem_previsto` (`app/ponto/turnos.py:2097`, `app/ponto/turnos.py:2103`), enquanto a folha retém `sem_celula` (`app/folha/export.py:1237`). A classificação com batidas segue `aberto/irregular/ok` (`app/colaboradores/services/calendario.py:159`), e o motivo só aparece no rótulo se o status final for `sem_certidao` (`app/colaboradores/services/calendario.py:899`). O teste novo usa dias sem batida (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:82`); cenário completo com batidas **[não verificado]**. Avaliado em memória pelo corpo real de `_classificar`: `retido_folha='sem_celula'` com `tem_batida=True, celula='ok'` retorna `ok`; com `celula='irregular'`, retorna `irregular`.

Não encontrei regressão demonstrável na trilha `palavra_dia/veredito_dia/cor_dia`: ela permanece ligada ao derivador anterior (`app/colaboradores/services/calendario.py:887`), cujo ramo normal devolve `trabalhou` sem palavra (`app/ponto/services/dia_decidido.py:396`, `app/ponto/services/dia_decidido.py:426`). Essa divergência conceitual é herdada, não introduzida pelo diff.

## 30/10 18:13 · sessao 20d699d43c5a · - · - · origem=Ronald (tmux)

O núcleo novo mantém a autoridade existente: `dias_encerrados` chama `_dia_encerrado_motivo_puro`, e a condição temporal continua `agora >= fim` depois de verificar turno aberto; não identifiquei juiz novo nesse trecho. `app/ponto/turnos.py:2088`, `app/ponto/turnos.py:2105`, `app/ponto/turnos.py:2137`.

Há dois relógios herdados: encerramento pelo DNA congelado (`fim_turno` → `ec.marcos_do_dia`) e expiração da aberta pelo template, com margem padrão de quatro horas e comparação estrita `agora > expiração`. O diff alimenta o DNA, mas não muda a segunda regra. `app/escala/servico_jornada.py:68`, `app/escala/servico_jornada.py:89`, `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:2007`.

A montagem captura um único `now`, deriva dele `hoje`, passa o instante ao juiz e a data à folha; essa borda está coerente. `app/colaboradores/services/calendario.py:258`, `app/colaboradores/services/calendario.py:712`, `app/colaboradores/services/calendario.py:726`.

Custo incremental estático: um SELECT de todos os vínculos com `tipo_escala` por montagem; turnos e células já carregados são reaproveitados. No ramo cego, uma chamada à retenção acrescenta os SELECTs de células e vínculos por envelope, não por dia. Não medi tempo nem consultas em execução. `app/ponto/turnos.py:2130`, `app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2138`, `app/folha/export.py:1188`, `app/folha/export.py:1194`, `app/escala/alimentacao.py:116`.

Escritores relevantes:

- `trabalha`, `dna`, `escala_geradora`: geração e regeneração na porta da célula. `app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:593`, `app/ponto/portas/celula.py:612`.
- `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata`: `lavrar_veredito`; o método do modelo delega. `app/ponto/portas/celula.py:689`, `app/ponto/portas/celula.py:695`, `app/escala/models.py:1775`.
- `ativa`, `data_fim`: encerramento pela porta do vínculo. `app/colaboradores/services/vinculo.py:780`, `app/colaboradores/services/vinculo.py:797`.

Leitores relevantes:

- DNA: `EscalaColaborador.marcos_do_dia`; fechamento temporal: `fim_turno` e o juiz. `app/escala/models.py:1094`, `app/escala/servico_jornada.py:92`, `app/ponto/turnos.py:2102`.
- Veredito: calendário monta `_ver_map`; folha lê diretamente `CelulaDia.veredito` e `trabalha`. `app/colaboradores/services/calendario.py:525`, `app/folha/export.py:1215`, `app/folha/export.py:1237`.
- Motivo da folha: calendário filtra `sem_celula`, alimenta `retido_folha` e compõe o rótulo. `app/colaboradores/services/calendario.py:727`, `app/colaboradores/services/calendario.py:834`, `app/colaboradores/services/calendario.py:899`.

Dívida herdada, distinta do diff: o carregador aceita células com chave `(colaborador_id, data)` e as passa diretamente ao `turnos_do_colab` quando não recebe `turnos`; este exige chave `data` e a embrulha novamente. O cartório usa exatamente esse caminho. Isso pode fazer o vínculo do pareamento ignorar a geradora da célula. O teste novo com células alimentadas não contém batidas para exercitar essa diferença geométrica. `app/ponto/turnos.py:2129`, `app/ponto/turnos.py:2132`, `app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1928`, `app/ponto/services/cartorio.py:853`, `app/ponto/tests/test_c1_dias_encerrados.py:62`.

O contraexemplo enviado pelo pai — vínculo encerrado válido no dia, sem vínculo ativo, célula de trabalho sem fim e sem veredito — é compatível com a autoridade canônica, que aceita vínculo encerrado dentro da vigência; a seleção ativa do calendário perde esse vínculo. `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:250`, `app/escala/alimentacao.py:253`, `app/colaboradores/services/calendario.py:351`.

_691 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
