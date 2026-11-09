# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 15:44** · publicado em **09/11/2026 15:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 15:44 · sessao c2a5482fbf2c · - · - · origem=Ronald (tmux)

- **P3 — NAO PROCEDE no instrumento:** o comando lê o usuário e fornece `sistema_pk` antes das dependências; o contexto restaura o memo ao sair. Assim, o fallback escritor fica inacessível pelo `handle` normal (`app/chamados/management/commands/diff_e3_sombra.py:94`, `app/chamados/management/commands/diff_e3_sombra.py:98`, `app/chamados/juizes.py:1111`, `app/chamados/juizes.py:1173`). Os testes de usuário ausente e de tranca real falhariam sem as respectivas curas (`app/chamados/tests/test_e3_diff_sombra.py:26`, `app/chamados/tests/test_e3_diff_sombra.py:41`). A guarda intercepta todos os aliases e o teste injeta escritor transitivo (`app/chamados/management/commands/diff_e3_sombra.py:92`, `app/chamados/tests/test_e3_diff_sombra.py:71`).

- **PROCEDENTE fora do instrumento — leitura nova pode escrever:** `CargaDaTarefa` chama `dias_trancados`; com tranca e memo sem usuário, chega a `get_user_sistema().pk`, cujo corpo executa `get_or_create`. Portanto, usuário ausente permite INSERT durante `tarefa_do_dia` (`app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/juizes.py:1036`, `app/chamados/juizes.py:1173`, `app/core/utils.py:18`). Cura: obter o usuário por leitura estrita ou exigir sua identidade na borda; acrescentar teste de tranca com usuário ausente, bloqueando qualquer tentativa de criação. O teste atual de zero escritas não cria tranca (`app/chamados/tests/test_e2_tarefa_do_dia.py:878`).

- **P4 — NAO PROCEDE para os defeitos nomeados:** atribuição simples e `getattr` literal são resolvidos; models e registro entram no universo (`app/chamados/tests/test_e3_pendentes_app.py:46`, `app/chamados/tests/test_e3_pendentes_app.py:69`, `app/chamados/tests/test_e3_pendentes_app.py:75`, `app/chamados/tests/test_e3_pendentes_app.py:96`). Os testes de inclusão e resolução falhariam sem cura; o contador isolado poderia passar perdendo o quarto chamador (`app/chamados/tests/test_e3_pendentes_app.py:119`, `app/chamados/tests/test_e3_pendentes_app.py:124`, `app/chamados/tests/test_e3_pendentes_app.py:144`).

- **FILA — alcance residual do selo:** imports relativos ignoram `ImportFrom.level`; atribuição por desempacotamento não registra aliases. Ambos podem esconder um quarto chamador; uso real nesta árvore não demonstrado (`app/chamados/tests/test_e3_pendentes_app.py:46`, `app/chamados/tests/test_e3_pendentes_app.py:78`). Cura sugerida: resolver esses dois formatos e acrescentar respectivos REDs.

- **FILA — guarda SQL:** aceita `SELECT … FOR UPDATE`; nenhum caminho concreto com essa trava foi demonstrado. Não constitui escritor P3 comprovado, mas impede chamar a guarda de bloqueio universal de travas (`app/chamados/management/commands/diff_e3_sombra.py:35`).

## 09/11 15:44 · sessao 76b09c74f4a9 · - · - · origem=Ronald (tmux)

- **NAO PROCEDE — segundo juiz/escritor no recorte auditado:** tarefa lê a decisão ordenada por `ato_em,pk`, passivo pela autoridade existente, lavra por `esta_stale`, tranca por `dias_trancados`, fila/verbos por `alimentar_fila`; publica apenas portas das ações já julgadas. `app/chamados/services/tarefa_do_dia.py:337`, `:346`, `:350`, `:367`, `:407`, `:425`. O helper de dia sem trabalho delega ao juiz existente. `app/ponto/supra_juiz.py:65`.
- **NAO PROCEDE — pouso antecipado da fachada C17:** os únicos consumidores produtivos encontrados de tarefa/carga estão no instrumento da sombra. `app/chamados/management/commands/diff_e3_sombra.py:115`, `:125`. A tarefa usa `alimentar_fila`, sem chamar C17. `app/chamados/services/tarefa_do_dia.py:407`.
- **FILA — identidade de segunda pausa:** a saída diferencia `int_ida:2`; a pergunta persistente ainda exige a posição derivada do motivo, recusando outro valor. A tarefa declara desconhecimento quando existe pergunta da posição; sem pergunta, solicita emissão sem identidade persistente específica para a repetição. Isso impede afirmar resolução completa de pausas repetidas; obrigação nesta etapa depende da lei literal ausente. `app/chamados/services/tarefa_do_dia.py:67`, `:268`; `app/chamados/models.py:3425`. Cura futura: identidade persistente por ocorrência, emissão e leitor migrados juntos. [nao verificado]
- **NAO PROCEDE — ambiguidade temporal introduzida no julgamento:** turno selecionado por `data_evento` com `conversa=False`, intervalo inclusivo; decisão e efeito incluem igualdade com `agora`; teto vence na igualdade; lavra futura é rejeitada. `app/chamados/services/tarefa_do_dia.py:324`, `:339`, `:358`; `app/ponto/services/cartorio.py:858`; `app/chamados/veredito_lavrado.py:224`. A fila exclui silêncio estritamente futuro. `app/chamados/services/painel.py:188`.

Mapa de escrita → leitura:

| Dado | Escritor/autoridade | Leitor novo |
|---|---|---|
| Decisão | `app/chamados/models.py:3932` (`bulk_create`) | tarefa `:337` |
| Passivo | `app/chamados/models.py:3903` | tarefa `:346` |
| Dia do fato | `app/chamados/services/fio.py:800`; rechaveamento salva `:1034` | tarefa `:324`, `:233` |
| Identidade/posição | `app/chamados/models.py:3414`; posição comum `app/chamados/services/declaracao_texto.py:79` | tarefa `:65`, `:108`; declaração `:134` |
| Ata/lâmpada | `app/ponto/services/cartorio.py:638`, `:1811` → `app/ponto/portas/celula.py:969` | `app/escala/services/leitor_celula.py:130` → tarefa `:84` |
| Lavra | `app/chamados/veredito_lavrado.py:250`, `:270` | tarefa `:350`; validade `app/chamados/veredito_lavrado.py:224` |
| Batida apurável | universo `app/ponto/turnos.py:2875`; criação datada `app/ponto/models.py:79` | tarefa `:358` |
| Status do chamado | `app/chamados/models.py:228`, `:296` | fila `app/chamados/services/painel.py:188` |
| Silêncio | `app/chamados/services/arquivo.py:166`, `:202` | painel `:190`; verbos `app/chamados/services/verbos.py:51` |
| Âncora | leitura comum `app/chamados/services/fio.py:116` | tarefa `:125`, `:362` |
| Rota/fila/verbos/fio | produção exclusivamente em tarefa `:425` | projeção da sombra `app/chamados/management/commands/diff_e3_sombra.py:43`, `:47` |

**FILA — merge futuro:** pontos de acoplamento concretos são as três consultas da API (`app/api/views.py:325`, `:1023`, `:2561`), a identidade do modelo (`app/chamados/models.py:3414`) e a posição compartilhada (`app/chamados/services/declaracao_texto.py:79`). Ressuscitar método antigo, mudar identidade ou substituir a regra de posição pode alterar o comportamento; conflito efetivo com main não é demonstrável sem o outro lado. [nao verificado]

Anexos literais e triagem não foram localizados; essa ausência limita a classificação da fila de pausas e a conferência literal dos pré-requisitos. [nao verificado]

## 09/11 15:43 · sessao 97912f6ce043 · - · - · origem=Ronald (tmux)

- **P1 — NAO PROCEDE, curado na causa:** `perguntas_pendentes_do_colab` delega a `consulta_do_app()['pendentes']`; `_pendentes` faz o mesmo. O teste contém fechada viva e aberta, exige apenas aberta nos dois leitores e nos kernels; só fechada deve produzir nenhum aviso. Repor o filtro antigo incluiria a fechada e falharia nessas asserções. Execução não realizada. `app/chamados/services/aviso_pendencia.py:7`, `app/chamados/services/cobranca.py:44`, `app/chamados/tests/test_e3_v4_orm.py:20`, `app/chamados/tests/test_e3_v4_orm.py:37`.

- **P2 — NAO PROCEDE, curado na causa:** API lista `pendentes + historico`, excluindo disputa fechada; não reconstrói vitalidade com SELECT próprio. O teste constrói pergunta morta por tranca que **continua no q_vivas**, confirma exclusão central e exige viva + respondida na lista nativa. Sem a cura, o SELECT antigo devolveria também a morta; usar só pendentes perderia a respondida. `app/api/views.py:1058`, `app/chamados/tests/test_e3_v4_orm.py:42`, `app/chamados/tests/test_e3_v4_orm.py:59`, `app/chamados/tests/test_e3_v4_orm.py:66`.

- **UNIDADE literal não chegou a zero:** relatório ainda calcula C6 + sem resposta para `AGUARDA_COLAB`; pauta semanal ainda calcula `q_vivas + respondida_em NULL + disputa aberta`; painel/placar ainda separa respondida/não respondida em `creditos_perguntas`. São leitores externos à consulta central. A classificação como F1/F2 preexistentes ou violação nova depende da triagem/leis não disponibilizadas na cópia [nao verificado]. `app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:21`, `app/chamados/services/cobranca.py:228`, `app/chamados/services/cobranca.py:257`, `app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:139`, `app/chamados/services/verbos.py:34`.

- **UI web conserva cálculo próprio:** `fio` carrega perguntas da disputa aberta sem C6 naquele ramo; template chama `p.viva` e lê `respondida_em` para mostrar/reenvio; `todas_respondidas` também é derivado localmente. Essa trilha não consulta `consulta_do_app`. `app/chamados/services/fio.py:221`, `app/chamados/services/fio.py:235`, `app/chamados/services/fio.py:340`, `app/templates/chamados/partials/_card_atendimento.html:77`, `app/templates/chamados/partials/_card_atendimento.html:94`, `app/templates/chamados/partials/_card_atendimento.html:155`.

- **FILA adicional, se a lei pretende abranger toda indicação de pendência:** respostas administrativas ainda contam sem resposta diretamente ao reabrir questionário e no fallback de validar inline, podendo anunciar “aguardando colaborador” para pergunta morta; o segundo nem sequer aplica q_vivas. Nenhum desses sítios aparece no diff total fornecido; autoria nova não demonstrada. `app/chamados/services/acoes_disputa.py:95`, `app/chamados/services/acoes_disputa.py:99`, `app/chamados/views_cobrar.py:175`, `app/chamados/views_cobrar.py:192`.

- **Nomes de fila F1/F2 e leis literais:** arquivos `triagem_preverif_v3b.txt`, `aval_lei_e3_unidade_c17.txt`, `aval_lei_c17_um_juiz.txt` e `leis_sitios_v6.txt` não localizados pela busca na cópia; conteúdo e correspondência exata dos rótulos [nao verificado].

## 09/11 15:39 · sessao ef93569ce1ce · - · - · origem=Ronald (tmux)

Escolha técnica recomendada para P3: **“run na linha diária” = intervalo reconstituído de um `ini` cuja contribuição de tempo ao dia consultado é estritamente positiva**. A aquisição existe mesmo com 0 segundos, mas não aumenta este contador de ocupação. Esta definição preserva o código e o selo atuais; a docstring precisa deixar de prometer inclusão de todo órfão. (`bin/codex_vagas_medida.py:125`, `bin/codex_vagas_medida.py:128`, `bin/tests/test_trava_hist_unico.sh:57`, `bin/tests/test_trava_hist_unico.sh:68`; escolha técnica: `CLAUDE.md:273`)

**C — matriz do histórico**

Use `d = max(min(fim, fim_dia) − max(ini, ini_dia), 0)`; números abaixo são dedução do juiz atual, sem execução do selo. (`bin/codex_vagas_medida.py:125`)

| Célula | Escritor → leitor | Número esperado pela definição acima / atual | Rótulo e prova |
|---|---|---|---|
| C1 `ini A` + `fim A` | Após adquirir flock escreve `ini`; trap escreve `fim` e rc. Leitor fecha somente se `quem` coincide. (`bin/trava_teste.sh:48`, `bin/trava_teste.sh:53`, `bin/trava_teste.sh:54`, `bin/codex_vagas_medida.py:144`) | `d>0`: `(d,1,0)`; `d=0`: `(0,0,0)`, esperado=atual. (`bin/codex_vagas_medida.py:125`, `bin/codex_vagas_medida.py:128`) | “trava de suite presa … em 1 run(s) (0 ini sem fim)”; selo testa 2 cópias/2 runs e preservação de rc7/ini/fim. (`bin/codex_vagas_medida.py:203`, `bin/tests/test_trava_hist_unico.sh:40`, `bin/tests/test_trava_hist_unico.sh:44`, `bin/tests/test_trava_teste.sh:146`, `bin/tests/test_trava_teste.sh:148`) |
| C2 `ini A` sem fim seguido de `ini B` | Próximo `ini` fecha o intervalo estimado de A, incrementa órfãos e abre B. (`bin/codex_vagas_medida.py:139`, `bin/codex_vagas_medida.py:142`, `bin/codex_vagas_medida.py:143`) | A contribui `d(A,B)` e 1 run somente se positivo; órfãos +1. Selo A01:00/B02:00/fimB02:30/C02:46:40, trava solta: `(5400,2,2)`. (`bin/codex_vagas_medida.py:125`, `bin/tests/test_trava_hist_unico.sh:54`, `bin/tests/test_trava_hist_unico.sh:57`) | Estimativa pode incluir intervalo ocioso A→B: o código não sabe quando A soltou. Não chamar esse total de duração factual exata. (`bin/codex_vagas_medida.py:111`, `bin/codex_vagas_medida.py:142`) Mutante existente remove soma do órfão e exige falha 0h30/1 run. (`bin/tests/test_trava_hist_unico.sh:82`) |
| C3 último `ini` sem fim, trava presa | Leitor consulta `--presa` após terminar leitura; `--presa` tenta o próprio flock e devolve rc0/presa ou rc1/livre. (`bin/codex_vagas_medida.py:149`, `bin/trava_teste.sh:39`, `bin/trava_teste.sh:40`) | Se presa: soma até agora recortado ao dia, n+1 se d>0, órfãos+1. Selo: `(81800,3,2)`, mostrado 22h43m. (`bin/codex_vagas_medida.py:147`, `bin/codex_vagas_medida.py:153`, `bin/tests/test_trava_hist_unico.sh:68`) | Vida significa “trava presa agora”, não identificação do antigo `quem`; corrida nova aquisição/novo `ini` permanece possível. (`bin/codex_vagas_medida.py:130`, `bin/codex_vagas_medida.py:149`, `bin/trava_teste.sh:48`, `bin/trava_teste.sh:53`) F7 permanece fila fornecida, ocorrência real `[não verificado]`. |
| C4 último `ini` sem fim, trava solta | Não chama `soma`; só órfãos++. (`bin/codex_vagas_medida.py:147`, `bin/codex_vagas_medida.py:152`) | Isolado: `(0,0,1)`; esperado=atual segundo definição escolhida. **DIVERGE P3 na declaração** “todo ini sem fim entra no n de runs”. (`bin/codex_vagas_medida.py:114`, `bin/codex_vagas_medida.py:128`, `bin/tests/test_trava_hist_unico.sh:57`) | Selo já exige exclusão do último solto; mutante existente ignora estado do lock e deve falhar com 22h43/3. (`bin/tests/test_trava_hist_unico.sh:57`, `bin/tests/test_trava_hist_unico.sh:83`) |
| C5 histórico ausente | `--hist` devolve o caminho, sem verificar existência; leitor abre e lança exceção; `_parte` converte em “nao sei”. (`bin/trava_teste.sh:38`, `bin/codex_vagas_medida.py:115`, `bin/codex_vagas_medida.py:130`, `bin/codex_vagas_medida.py:193`) | Esperado/atual: desconhecido, não `(0,0,0)`; linha diária segue rc0, com componente desconhecido. (`app/docs/LEIS.md:61`, `bin/codex_vagas_medida.py:213`, `bin/codex_vagas_medida.py:233`) | Mutante `--hist`→`--quem` já exige “nao sei”, mas não há caso específico de histórico ausente na lista atual. (`bin/tests/test_trava_hist_unico.sh:84`, `bin/tests/test_trava_hist_unico.sh:39`, `bin/tests/test_trava_hist_unico.sh:51`) |
| C6 relógio regressivo | Escritor usa epoch civil `date +%s`; leitor não rejeita timestamp menor que anterior nem une intervalos. (`bin/trava_teste.sh:32`, `bin/codex_vagas_medida.py:136`, `bin/codex_vagas_medida.py:139`, `bin/codex_vagas_medida.py:145`) | Sequência `ini A 0/fim A 86400/ini B 0/fim B 86400` produz `(172800,2,0)` para dia de 86400s. (`bin/codex_vagas_medida.py:125`, `bin/codex_vagas_medida.py:127`) | Rótulo atual pode imprimir 48h; tratamento restritivo de regressão é F6 da triagem, não P novo. Não há teste/mutante correspondente no selo atual. (`bin/codex_vagas_medida.py:203`, `bin/tests/test_trava_hist_unico.sh:81`, `bin/tests/test_trava_hist_unico.sh:84`) Ocorrência real `[não verificado]`. |
| C7 cruza meia-noite | Mesmos eventos; dia inicia à meia-noite local, fim é ini+86400; soma recorta intervalo. (`bin/codex_vagas_medida.py:119`, `bin/codex_vagas_medida.py:120`, `bin/codex_vagas_medida.py:125`) | `23:50→00:10`: `(600,1,0)` em cada dia regular de 86400s; n diário não é contador global de execuções únicas. (`bin/codex_vagas_medida.py:125`, `bin/codex_vagas_medida.py:128`) | Caso direto/mutante de retirar recorte ainda exigidos `[não verificado]`; selo atual só usa eventos no mesmo dia. (`bin/tests/test_trava_hist_unico.sh:54`) |

**Precisões do contrato C:** `orfaos` conta todos os órfãos do histórico lido, independentemente do dia consultado, pois incrementa fora do recorte diário; o rótulo atual apenas diz “N ini sem fim”. Não declarar esse número como “órfãos iniciados no dia”. (`bin/codex_vagas_medida.py:141`, `bin/codex_vagas_medida.py:148`, `bin/codex_vagas_medida.py:203`)

`--presa` toma exclusividade brevemente quando consegue o flock; não prova FIFO ou impossibilidade de disputar com chamador real. O real espera `-w`, a sonda tenta `-n`; impacto real `[não verificado]`. (`bin/trava_teste.sh:40`, `bin/trava_teste.sh:48`)

**D — classificador**

| Célula | Esperado | Atual | Prova |
|---|---|---|---|
| D1 morte pela mensagem declarada | MORTO quando todas as falhas declaradas aparecem como asserções do caso. (`bin/tests/test_codex_pergunta.sh:640`, `bin/tests/test_codex_pergunta.sh:641`) | MORTO se cada substring `"FALHA: $e"` aparece em algum lugar da saída. (`bin/tests/test_codex_pergunta.sh:747`, `bin/tests/test_codex_pergunta.sh:748`) | Autoteste positivo atual. (`bin/tests/test_codex_pergunta.sh:752`) |
| D2 eco em diagnóstico | Só diagnóstico, sem linha real de asserção: VIVO; com falha real diferente: ERRADO. É exigência de endurecimento F5, não reprovação P nova. (`bin/tests/test_codex_pergunta.sh:641`, `bin/tests/test_codex_pergunta.sh:642`; definição da linha real a fixar `[não verificado]`) | **DIVERGE F5:** `diagnostico: FALHA: <esperado>` conta MORTO, porque buscas não ancoram linha nem distinguem produtor. (`bin/tests/test_codex_pergunta.sh:744`, `bin/tests/test_codex_pergunta.sh:747`) | Autotestes atuais não incluem eco. Mutante atual que produza efetivamente esse eco `[não verificado]`. (`bin/tests/test_codex_pergunta.sh:750`, `bin/tests/test_codex_pergunta.sh:755`) |
| D3 mais de uma mensagem | Todos os esperados exigidos; esperado+outras falhas ainda MORTO; faltou algum=ERRADO; nenhum evento de falha=VIVO. (`bin/tests/test_codex_pergunta.sh:641`, `bin/tests/test_codex_pergunta.sh:642`) | Igual, ressalva D2: cada esperado é substring; quantidade de falhas extras não impede MORTO. (`bin/tests/test_codex_pergunta.sh:744`, `bin/tests/test_codex_pergunta.sh:747`) | Autoteste “dois esperados/bastou um” exige ERRADO; vazio exige ERRADO; arquivo vazio exige VIVO. Falta positivo com ambos em linhas distintas. (`bin/tests/test_codex_pergunta.sh:753`, `bin/tests/test_codex_pergunta.sh:754`, `bin/tests/test_codex_pergunta.sh:755`) |
| D4 mensagem em outra guarda | Outra asserção sem a declarada=ERRADO; mensagem deve identificar a guarda pretendida. (`bin/tests/test_codex_pergunta.sh:641`, `bin/tests/test_codex_pergunta.sh:751`) | Outra asserção distinta=ERRADO; mensagem textual idêntica não tem identidade adicional no classificador. (`bin/tests/test_codex_pergunta.sh:747`) O acidente concreto `saida_symlink` foi desambiguado no caso. (`bin/tests/test_codex_pergunta.sh:201`) | Guarda de saída escreve `saida … e symlink`; a seguinte escreve `codex: trava … e symlink`. (`bin/codex_pergunta.sh:338`, `bin/codex_pergunta.sh:511`) Mutante `saida_symlink` exige falha de motivo começando `'saida`. (`bin/tests/test_codex_pergunta.sh:666`) |

ERRADO e VIVO aumentam `falhas`; não contam como sucesso do selo. (`bin/tests/test_codex_pergunta.sh:777`, `bin/tests/test_codex_pergunta.sh:779`, `bin/tests/test_codex_pergunta.sh:787`)

**P4 é divergência de prova em B/load e D/declaração:** cenário usa `NPL=nproc−2` e mensagem dinâmica; duas declarações de mutantes exigem literalmente `load5 6.00`. Se nproc≠8, a falha pretendida produz outro número e recebe ERRADO. (`bin/tests/test_codex_pergunta.sh:100`, `bin/tests/test_codex_pergunta.sh:102`, `bin/tests/test_codex_pergunta.sh:520`, `bin/tests/test_codex_pergunta.sh:720`, `bin/tests/test_codex_pergunta.sh:722`, `bin/tests/test_codex_pergunta.sh:747`) nproc ou falso vermelho ocorrido na produção `[não verificado]`.

**E — testes/mutantes necessários deste recorte** [nao verificado]

1. `hist_unico_duas_copias` — `bin/tests/test_trava_hist_unico.sh:39`; C1/fonte única; mutantes já existentes “HIST relativo à cópia” e `--hist→--quem`. (`bin/tests/test_trava_hist_unico.sh:81`, `bin/tests/test_trava_hist_unico.sh:84`)
2. `hist_orfao_fecha_no_proximo_ini` — `bin/tests/test_trava_hist_unico.sh:54`; C2; mutante já existente remove soma do órfão. (`bin/tests/test_trava_hist_unico.sh:82`)
3. `hist_ultimo_orfao_preso_solto` — `bin/tests/test_trava_hist_unico.sh:57`, `bin/tests/test_trava_hist_unico.sh:68`; C3/C4; mutante já existente conta até agora mesmo solto. (`bin/tests/test_trava_hist_unico.sh:83`)
4. `hist_run_contribuinte_contrato_unico` — mesmo selo; C4/P3: exigir declaração do significado escolhido e tuplas `(0,0,1)` para órfão solto, `(0,0,0)` para ini/fim no mesmo instante, positivo=1. Mutante exigido: contar run sem contribuição positiva. Teste/mutante novos `[não verificado]`; sítios atuais da contradição `bin/codex_vagas_medida.py:114`, `bin/codex_vagas_medida.py:128`, `bin/tests/test_trava_hist_unico.sh:57`.
5. `hist_ausente_nao_sei` — mesmo selo; C5; mutante exigido troca exceção por `(0,0,0)`. Novo `[não verificado]`; juiz atual `bin/codex_vagas_medida.py:130`, `bin/codex_vagas_medida.py:193`.
6. `hist_recorte_meia_noite` — mesmo selo; C7; mutante exigido retira recorte diário. Novo `[não verificado]`; juiz atual `bin/codex_vagas_medida.py:125`.
7. `classifica_eco_diagnostico` — `bin/tests/test_codex_pergunta.sh`, D2/F5; diagnóstico ecoando esperado sozinho deve ser VIVO; diagnóstico+asserção alheia deve ser ERRADO. Mutante exigido restaura busca sem âncora. Novo `[não verificado]`; sítio atual `bin/tests/test_codex_pergunta.sh:747`.
8. `classifica_todos_e_somente_morte_declarada` — mesmo selo; D1/D3/D4; conservar autotestes atuais e acrescentar dois esperados presentes e esperado+falha extra. Mutante `echo MORTO`, mutante que aceita só um esperado, mutante que aceita qualquer FALHA. Cobertura atual parcial `bin/tests/test_codex_pergunta.sh:751`, `bin/tests/test_codex_pergunta.sh:755`; novos cenários/mutantes `[não verificado]`.
9. `v2_carga_portavel` — mesmo selo; B/load/P4; cenário e esperado precisam falar do mesmo `NPL`, exercitado com nproc≠8. Mutantes atuais `portao` e `carga` devem continuar morrer pela falha dinâmica, e mutante exigido recrava 6.00 para morder portabilidade. Atual `bin/tests/test_codex_pergunta.sh:520`, `bin/tests/test_codex_pergunta.sh:720`, `bin/tests/test_codex_pergunta.sh:722`; novo teste de portabilidade `[não verificado]`.
10. `trava_hist_preserva_rc` — `bin/tests/test_trava_teste.sh:146`; C1/escritor; comando exit7 deve continuar rc7 e fim rc7. Mutantes exigidos remover `ini`, remover `fim`, engolir rc. Teste atual `bin/tests/test_trava_teste.sh:147`, `bin/tests/test_trava_teste.sh:148`; mutantes `[não verificado]`.

F6 relógio regressivo e F7 identidade/race são filas do dado; não alegar que os mutantes atuais os cobrem. A lista atual tem só os quatro mutantes acima. (`bin/tests/test_trava_hist_unico.sh:81`, `bin/tests/test_trava_hist_unico.sh:84`)

**F — escritores/leitores deste recorte**

| Campo/arquivo | Escritor → leitor |
|---|---|
| `TRAVA` | Definido em `bin/trava_teste.sh:23`; aquisição de execução fd9 em `bin/trava_teste.sh:47`, `bin/trava_teste.sh:48`; sonda fd8 em `bin/trava_teste.sh:39`. |
| `HIST`/caminho | Derivado uma vez em `bin/trava_teste.sh:31`; publicado por `--hist` em `bin/trava_teste.sh:38`; medida pergunta em `bin/codex_vagas_medida.py:115`; selo pergunta em `bin/tests/test_trava_hist_unico.sh:47`; selo antigo consulta antes de leitura direta em `bin/tests/test_trava_teste.sh:112`, `bin/tests/test_trava_teste.sh:148`. |
| Histórico `epoch,evento,quem,rc` | `_hist` escreve best effort em `bin/trava_teste.sh:32`; ini/fim chamados em `bin/trava_teste.sh:53`, `bin/trava_teste.sh:54`; medida lê epoch/evento/quem em `bin/codex_vagas_medida.py:132`, `bin/codex_vagas_medida.py:136`, `bin/codex_vagas_medida.py:139`, `bin/codex_vagas_medida.py:144`; rc não entra na soma. |
| `TRAVA.quem` | Escritor antes da execução e trap em `bin/trava_teste.sh:52`, `bin/trava_teste.sh:54`; leitores `--quem` e diagnóstico timeout em `bin/trava_teste.sh:37`, `bin/trava_teste.sh:49`. |
| `segundos,n,orfaos` | `_ocupacao_trava` produz em `bin/codex_vagas_medida.py:154`; linha diária lê e rotula em `bin/codex_vagas_medida.py:202`, `bin/codex_vagas_medida.py:203`; selo lê texto em `bin/tests/test_trava_hist_unico.sh:22`. |
| `chamadas.log`/vaga2 | Medida filtra data, evento ini e último campo `vaga=2` em `bin/codex_vagas_medida.py:160`, `bin/codex_vagas_medida.py:164`; contagem de recusas lê `vaga2.log` por prefixo data em `bin/codex_vagas_medida.py:166`, `bin/codex_vagas_medida.py:168`. |
| SAR média/máximo/n | `sar -q` ou arquivo em `bin/codex_vagas_medida.py:38`, `bin/codex_vagas_medida.py:43`; parser do cabeçalho em `bin/codex_vagas_medida.py:57`; consumidores desligador e linha em `bin/codex_vagas_medida.py:88`, `bin/codex_vagas_medida.py:198`. |
| Sessões `timestamp,primary.used_percent,window_minutes,resets_at` | Produtor CLI declarado em `bin/codex_vagas_medida.py:15`; escrita efetiva CLI `[não verificado]`; leitor canônico em `bin/codex_limite_estado.py:154`, `bin/codex_limite_estado.py:155`, `bin/codex_limite_estado.py:159`, `bin/codex_limite_estado.py:161`; medida reutiliza em `bin/codex_vagas_medida.py:78`; desligador consome em `bin/codex_vagas_medida.py:89`, `bin/codex_vagas_medida.py:95`. |
| `cota.json`: `seven_day_pct,seven_day_resets_at,hora` | Statusline escreve por replace em `bin/statusline_cota.py:51`, `bin/statusline_cota.py:54`; medida lê atual/anterior e compara reset/hora em `bin/codex_vagas_medida.py:174`, `bin/codex_vagas_medida.py:178`, `bin/codex_vagas_medida.py:180`, `bin/codex_vagas_medida.py:182`; disjuntor lê pct/reset em `bin/disjuntor.py:1167`. Produtor da foto anterior `[não verificado]`. |
| Mutantes/esperados/saída/tabela | Declaração em `bin/tests/test_codex_pergunta.sh:643`; fábrica em `bin/tests/test_codex_pergunta.sh:769`; caso produz saída em `bin/tests/test_codex_pergunta.sh:773`; classificador lê em `bin/tests/test_codex_pergunta.sh:747`; tabela escrita em `bin/tests/test_codex_pergunta.sh:776`; selo histórico usa sua própria busca substring em `bin/tests/test_trava_hist_unico.sh:97` (mesma fragilidade de eco). |

Não executei nenhum selo nem consulta real de `--presa`; os próprios selos criam arquivos e `--presa` abre o lock com append. (`bin/tests/test_trava_hist_unico.sh:18`, `bin/tests/test_codex_pergunta.sh:21`, `bin/trava_teste.sh:39`)

## 09/11 15:38 · sessao 3e8b758c395a · - · - · origem=Ronald (tmux)

Matriz B — “E=A” significa esperado igual ao atual; admissão pressupõe validações anteriores verdes. O critério de simultaneidade é arquivo comum, não app, e o teto é dois construtores. (`CLAUDE.md:306`, `CLAUDE.md:288`; `bin/codex_pergunta.sh:375`)

| Linha | Juiz → admite/recusa | Persistido | Retorno | Prova atual / lacuna |
|---|---|---|---|---|
| 1ª chamada, nenhuma outra viva | E=A: sob meta-trava, tenta vaga 1; outra livre dispensa portões; publica estado. (`bin/codex_pergunta.sh:380`, `:383`, `:390`, `:392`) | Estado publicado por rename; depois `ini` com vaga; no EXIT `fim`, `.rc`, remoção de estado. (`bin/codex_pergunta.sh:310`, `:311`, `:428`, `:229`, `:232`, `:236`) | Admissão não determina rc final; controle positivo termina 0. (`bin/codex_pergunta.sh:609`; `bin/tests/test_codex_pergunta.sh:315`) | `c_positivo`, `c_v2_admite`; mutantes de guardas operacionais, porém controle explícito “portões vermelhos com primeira vaga livre ainda admite” inexistente na lista atual. (`bin/tests/test_codex_pergunta.sh:315`, `:506`, `:611`) |
| 2ª chamada, primeira viva | E=A: ocupa vaga 2 e passa todos os portões, depois publica estado. (`bin/codex_pergunta.sh:385`, `:390`, `:391`, `:392`) | `.estado`, `chamadas.log ini/fim vaga=2`; estado sai no EXIT. (`bin/codex_pergunta.sh:311`, `:428`, `:229`, `:236`) | 0 se execução/inventário verdes; caso recusa portão: 2, `VAGA2:`. (`bin/codex_pergunta.sh:250`, `:587`, `:609`) | `c_v2_admite`; mutantes `portao`, `carga`, `janela`, `desligador`, `toca`, `estado_outra`, `pausa_segunda`. (`bin/tests/test_codex_pergunta.sh:506`, `:720`) |
| 3ª, duas vivas | E=A: falham os dois flocks, recusa teto dois. (`bin/codex_pergunta.sh:383`, `:386`) | Uma tentativa de append `vaga2.log`; não publica estado nem alcança `chamadas.log ini`. (`bin/codex_pergunta.sh:249`, `:386`, `:392`, `:428`) | 2, `VAGA2: vaga ... ocupada nas 2 vagas`. (`bin/codex_pergunta.sh:250`, `:386`) | `c_vaga`, inclusive outro LOGD; mutante `vaga`. (`bin/tests/test_codex_pergunta.sh:239`, `:679`) |
| load5 abaixo de nproc−2 | E=A: leitura segunda coluna e comparação estrita; aceita este portão se nproc>2. (`bin/codex_pergunta.sh:297`, `:298`) | Admitida segue estado/chamadas; recusas posteriores registram load lido. (`bin/codex_pergunta.sh:249`, `:311`, `:428`) | Não fixa rc final. (`bin/codex_pergunta.sh:609`) | Fixture `load_1`; caso de admissão. Não há matriz explícita abaixo/igual/acima por nproc simulado. (`bin/tests/test_codex_pergunta.sh:100`, `:101`, `:506`) |
| load5 igual | E=A no wrapper: recusa igualdade. **DIVERGE no selo P4**: esperado de mutantes crava 6.00, cenário usa nproc−2. (`bin/codex_pergunta.sh:298`; `bin/tests/test_codex_pergunta.sh:100`, `:520`, `:720`, `:722`) | `vaga2.log` com motivo, `load5=NPL.00`, fonte teste. (`bin/codex_pergunta.sh:249`; `bin/tests/test_codex_pergunta.sh:521`) | Wrapper 2; classificador pode ERRADO quando NPL≠6. (`bin/codex_pergunta.sh:250`; `bin/tests/test_codex_pergunta.sh:747`, `:750`, `:720`) | `c_v2_carga`; mutantes `portao`, `carga`, `log_segunda`. (`bin/tests/test_codex_pergunta.sh:519`, `:720`, `:722`, `:728`) |
| load5 acima | E=A: recusa. (`bin/codex_pergunta.sh:298`) | `vaga2.log`; sem estado desta chamada/ini. (`bin/codex_pergunta.sh:249`, `:306`, `:392`, `:428`) | 2, mensagem load ≥ limiar ou ilegível. (`bin/codex_pergunta.sh:299`, `:250`) | Caso específico acima não consta; atual `c_v2_carga` cobre igualdade. (`bin/tests/test_codex_pergunta.sh:519`) |
| load real / cravado | E=A: default `/proc/loadavg`; teste pode substituir arquivo sob guarda topológica; fonte `real/teste`. (`bin/codex_pergunta.sh:111`, `:112`, `:114`, `:116`, `:118`) | Fonte vai ao `vaga2.log`; load de admissão não é relido durante execução. (`bin/codex_pergunta.sh:249`, `:297`, `:565`) | Env teste em topologia proibida: 2 antes da admissão. (`bin/codex_pergunta.sh:114`) | `c_teste_guarda`, mutante `teste_guarda`; fixtures falsas em todas chamadas `_roda`. (`bin/tests/test_codex_pergunta.sh:153`, `:598`, `:729`) |
| 06:00 | E=A: recusa. (`bin/codex_pergunta.sh:301`) | `vaga2.log`, motivo janela. (`bin/codex_pergunta.sh:249`, `:302`) | 2, `VAGA2: janela 06:00-07:29...`. (`bin/codex_pergunta.sh:250`, `:302`) | `c_v2_janela/0600`; mutante janela. (`bin/tests/test_codex_pergunta.sh:524`, `:723`) |
| 07:29 | E=A: recusa. (`bin/codex_pergunta.sh:301`) | Igual linha anterior. (`bin/codex_pergunta.sh:249`) | 2. (`bin/codex_pergunta.sh:250`) | `c_v2_janela/0729`. (`bin/tests/test_codex_pergunta.sh:525`) |
| 07:30 | E=A: passa janela. (`bin/codex_pergunta.sh:301`) | Estado/chamadas se demais verdes. (`bin/codex_pergunta.sh:392`, `:428`) | 0 no controle. (`bin/tests/test_codex_pergunta.sh:526`, `:493`) | `c_v2_janela/0730`. (`bin/tests/test_codex_pergunta.sh:526`) |
| Desligador ilegível | E=A: medida retorna “não sei” rc2; wrapper converte todo não zero em recusa. (`bin/codex_vagas_medida.py:90`, `:91`; `bin/codex_pergunta.sh:303`, `:304`) | `vaga2.log` motivo desligador, última linha limitada a 200 caracteres. (`bin/codex_pergunta.sh:249`, `:304`) | Wrapper 2 com `VAGA2: desligador: nao sei...`. (`bin/codex_pergunta.sh:250`, `:304`) | `c_v2_desligador/sem_leitura` e `/sem_sar`; mutante desligador hoje exige falha de cenário sar alto, não individualmente das duas fontes ausentes. (`bin/tests/test_codex_pergunta.sh:535`, `:536`, `:724`) |
| Vaga órfã após SIGKILL | E=A quanto ao juiz: estado/pid não provam vida, `_viva` tenta flock; lock livre permite reutilizar e substituir estado; lock preso com estado ilegível recusa. (`bin/codex_pergunta.sh:253`, `:255`, `:291`, `:296`, `:311`, `:383`) | SIGKILL não possui trap próprio; único cleanup é EXIT, logo não há promessa demonstrada de estado/fim após kill9. (`bin/codex_pergunta.sh:226`, `:236`, `:240`; execução SIGKILL [não verificado]) | Nova tentativa depende do flock real, não existência do estado. (`bin/codex_pergunta.sh:390`, `:391`) | Sem caso explícito SIGKILL na lista de casos. (`bin/tests/test_codex_pergunta.sh:611`) |
| Meta ocupada na admissão | E=A: espera default10 s, teste1 s; falhando recusa. (`bin/codex_pergunta.sh:119`, `:381`; `bin/tests/test_codex_pergunta.sh:548`) | `vaga2.log`, `load5=-` pois carga ainda não lida; sem estado/ini. (`bin/codex_pergunta.sh:247`, `:249`, `:381`, `:392`, `:428`) | 2, `VAGA2: meta-trava...`. (`bin/codex_pergunta.sh:250`, `:381`) | `c_meta_trava`; mutante `meta_trava`. (`bin/tests/test_codex_pergunta.sh:546`, `:719`) |
| Meta ocupada no fim | E=A restritivo: inventário ganha `FALHA vagas meta-trava ... nao presa no fim`; `_IRC=3`. (`bin/codex_pergunta.sh:565`, `:572`) | `.inventario`, `.recusa`, `.rc rc3`, `fim`; estado removido no EXIT. (`bin/codex_pergunta.sh:572`, `:579`, `:232`, `:229`, `:236`) | 3 prevalece. (`bin/codex_pergunta.sh:587`) | Não há caso próprio nem #G próprio na linha de falha do fim; mutante futuro deve neutralizar essa recusa e morrer pela falta da FALHA declarada. (`bin/codex_pergunta.sh:572`; `bin/tests/test_codex_pergunta.sh:611`, `:719`) |
| rc3 na saída | E=A: inventário proibido/falha vence qualquer RC; grava recusa e sai3; `.id` não nasce. (`bin/codex_pergunta.sh:557`, `:576`, `:579`, `:587`, `:603`) | `.inventario`, `.recusa`, `.rc3`, chamadas fim, limpa estado. (`bin/codex_pergunta.sh:560`, `:579`, `:232`, `:229`, `:236`) | 3 + `RECUSA rc 3... wt DESCARTADA`. (`bin/codex_pergunta.sh:578`, `:587`) | `_inv_recusa`, casos de inventário, mutante `inventario`. (`bin/tests/test_codex_pergunta.sh:169`, `:374`, `:693`) |
| Timeout do Codex | E=A pelo fluxo: timeout com kill adicional após30; captura RC e sempre inventaria quando retorna controle; inventário ruim vence, senão RC retornado;124 não vira LIMITE. (`bin/codex_pergunta.sh:541`, `:551`, `:560`, `:587`, `:588`, `:609`) | `.inventario`; `.recusa` só inventário ruim; `.rc/fim` no EXIT, sem `.id` se RC≠0. (`bin/codex_pergunta.sh:560`, `:576`, `:603`, `:232`, `:229`) | 124 quando timeout retorna124 e inventário verde;137 possível depende do timeout/processos [não verificado]; nunca afirmar timeout=3 sempre. (`bin/codex_pergunta.sh:551`, `:587`, `:588`, `:609`) | Não há cenário timeout próprio na lista. (`bin/tests/test_codex_pergunta.sh:611`) |
| kill9 do wrapper | Expectativa revisável: sem conclusão verde nem lastro novo completo [não verificado]. Atual: só há EXIT trap; sem caminho de inventário independente do wrapper. (`bin/codex_pergunta.sh:240`, `:560`, `:603`) | Estado e `ini` podem sobrar; `.inventario/.rc/fim` não são garantidos pelo código nessa morte. (`bin/codex_pergunta.sh:236`, `:428`, `:560`, `:232`; ocorrência [não verificado]) | Wrapper morto não emite `RECUSA` nem `sessao=...`; status observado pelo pai [não verificado]. (`bin/codex_pergunta.sh:578`, `:608`) | Necessário caso isolado de kill9 para provar lastro ausente e decisão pelo flock [não verificado]. |

**Cuidado com herança do lock:** fd6 é aberto e adquirido em `:382/:386` e continua aberto ao lançar `timeout ... codex` em `:541`; não há fechamento explícito pré-filho. Portanto não afirmar “matou o wrapper, a vaga soltou imediatamente”: filhos podem manter descritor; a prova útil permanece `_viva`. (`bin/codex_pergunta.sh:382`, `:386`, `:541`, `:253`; retenção efetiva por Codex [não verificado])

F — escritores/leitores de B:

| Campo/arquivo | Escritor → leitor |
|---|---|
| Caminhos vaga1/vaga2/meta/estados | Git common-dir → wrapper deriva nomes e reserva respostas; aquisição e consulta usam esses nomes. (`bin/codex_pergunta.sh:81`, `:85`, `:93`, `:97`, `:380`, `:389`) |
| Flock da vaga | Wrapper fd6 → `_viva` abre outro fd, tenta não bloquear; falha de abertura conta viva. (`bin/codex_pergunta.sh:382`, `:383`, `:386`, `:254`, `:255`) |
| Meta-flock | Wrapper fd3 → admissão e snapshot do fim; fecha antes do Codex e após leitura final. (`bin/codex_pergunta.sh:380`, `:381`, `:393`, `:565`, `:570`) |
| `.estado` `vaga,pid,obra,wt,hash,hora,toca` | `_publica_estado` grava temporário e renomeia; `_toca_de` lê apenas linhas `toca=`; `_saida` remove. Não há leitor desses outros campos no juiz de admissão/fim. (`bin/codex_pergunta.sh:310`, `:311`, `:291`, `:295`, `:567`, `:236`) |
| Load5 | Kernel `/proc/loadavg` ou fixture → awk segunda coluna, portão compara com `nproc`; recusa escreve valor. (`bin/codex_pergunta.sh:118`, `:297`, `:298`, `:249`) |
| Hora | `TZ=America/Sao_Paulo date +%H%M` ou fixture → juiz janela. (`bin/codex_pergunta.sh:300`, `:301`) |
| Pausas | Produtor externo não examinado [não verificado] → wrapper consulta `codex-construtor` e `codex-vaga2`. (`bin/codex_pergunta.sh:328`, `:294`) |
| Sar | `sar -q` real ou arquivo teste → parser média/máximo/amostras → desligador média>7. (`bin/codex_vagas_medida.py:38`, `:43`, `:49`, `:93`) |
| Codex used/reset | Leitor semanal canônico → desligador, >85 com reset futuro/ausente desliga. Escrita real do CLI [não verificado]. (`bin/codex_vagas_medida.py:75`, `:78`, `:95`, `:96`) |
| `vaga2.log` hora/obra/motivo/load/fonte | `_recusa_v2` append → contador diário e selo; append é best effort, falha não altera rc2. (`bin/codex_pergunta.sh:249`, `:250`; `bin/codex_vagas_medida.py:166`, `:168`; `bin/tests/test_codex_pergunta.sh:503`) |
| `chamadas.log` hora/obra/papel/hash12/pid/evento/pergunta ou sessão/rc/duração/vaga | Wrapper ini/fim → leitor posicional canônico; medida conta `ini` cujo último campo é vaga2; disjuntor reutiliza leitor. (`bin/codex_pergunta.sh:428`, `:229`; `bin/codex_limite_estado.py:49`, `:63`, `:67`; `bin/codex_vagas_medida.py:164`; `bin/disjuntor.py:2121`) |
| `.inventario` | Inventário final + validação gitfile + snapshot e TOCA → decisão `_IRC`, recusa. (`bin/codex_pergunta.sh:560`, `:561`, `:568`, `:574`, `:576`) |
| `.recusa` | Wrapper filtra linhas não TOCADO → stderr e selo; instrução de descarte é mensagem, não remoção automática da WT. (`bin/codex_pergunta.sh:578`, `:579`, `:580`; `bin/tests/test_codex_pergunta.sh:171`) |
| `.rc` | EXIT publica por rename com id da chamada, rc, fim → selo; não é prova de vida. (`bin/codex_pergunta.sh:231`, `:232`, `:233`; `bin/tests/test_codex_pergunta.sh:174`, `:330`) |
| Resposta/JSONL/err e `.id` | Codex cria saída temporária/JSONL/stderr; wrapper move resposta e publica `.id` apenas RC0+resposta desta chamada. (`bin/codex_pergunta.sh:545`, `:553`, `:603`, `:604`) |

P4 deve entrar no contrato E como teste de portabilidade dos dois registros mutantes `portao/carga`: cenário e esperado precisam variar juntos com NPL, incluindo host simulado distinto de8; o mutante que retorna “6.00” fixo deve morrer pela asserção de esperado incorreto, não por outra falha. O defeito atual está nos literais; execução desse futuro teste [não verificado]. (`bin/tests/test_codex_pergunta.sh:100`, `:520`, `:720`, `:722`)

## 09/11 15:38 · sessao 11601509dba7 · - · - · origem=Ronald (tmux)

A matriz A pode usar estes sítios comuns: argumento `bin/codex_pergunta.sh:71`, `:321`, `:323`; admissão `:270`, `:305`; inventário `:196`, `:217`; leitura `:278`, `:279`, `:282`; juiz final `:284`, `:287`; estado `:310`, `:311`, leitura `:291`; persistência e retorno `:560`, `:574`, `:579`, `:587`, `:603`, `:609`. A lei de arquivo comum está em `CLAUDE.md:306`, `:307`; cura restritiva em `CLAUDE.md:278`.

| Linha A | Esperado × atual e localização |
|---|---|
| Arquivo exato | Concordam: argumento sem `/` cobre só igualdade (`bin/codex_pergunta.sh:264`); outro arquivo é admitido, mesmo arquivo recusa (`:266`, `:273`); TOCADO fora recusa rc 3 (`:284`, `:587`). Caso `c_toca_exato` em `bin/tests/test_codex_pergunta.sh:563`, `:565`; mutante `toca_exato` em `:733`, `toca_fim` em `:732`. |
| Pasta `/` | Concordam: prefixo inclui `/`, logo pasta cobre descendentes por componente (`bin/codex_pergunta.sh:264`). Casos `controle_pasta` e `folha_nao_cobre_folhas` em `bin/tests/test_codex_pergunta.sh:564`, `:567`; mutante `toca_exato` em `:733`. |
| Pasta pai criada para arquivo nomeado | Sem outra raia, concordam: pasta nova vira TOCADO (`bin/codex_pergunta.sh:196`), pai necessário é aceito (`:284`); caso existente em `bin/tests/test_codex_pergunta.sh:569`. Com meu `app/novo/a.py` e outro `app/novo/b.py`, **DIVERGE P2**: admissão disjunta (`bin/codex_pergunta.sh:266`), fim considera `app/novo/` comum com `b.py` (`:287`). Lei exige arquivo comum (`CLAUDE.md:306`). Falta caso concorrente; futuro `[não verificado]`. |
| Pasta irmã | Concordam: `app/folhas/` não cruza `app/folha/` (`bin/codex_pergunta.sh:264`, `:266`); caso admissão em `bin/tests/test_codex_pergunta.sh:530`; caso fim `:564`. |
| Mesmo nome com e sem `/` | Concordam na recusa restritiva da admissão: igualdade após remover `/` (`bin/codex_pergunta.sh:266`, `CLAUDE.md:278`); caso `app/folha` contra `app/folha/` em `bin/tests/test_codex_pergunta.sh:529`. Sem `/` continua arquivo exato no fim (`bin/codex_pergunta.sh:264`), não permissão para descendentes; caso `bin/tests/test_codex_pergunta.sh:563`. |
| TAB | Argumento recusa (`bin/codex_pergunta.sh:323`). Nome encontrado no FS não recebe validação equivalente: **DIVERGE P1**. Inventário imprime cru (`:217`); leitor separa TAB e usa só segundo campo (`:279`, `:282`). `app/folha/views.py<TAB>fora` passa por declaração exata `app/folha/views.py`. Falta caso/mutante próprio no CASOS/lista atuais (`bin/tests/test_codex_pergunta.sh:617`, `:618`, `:730`, `:737`). |
| Newline | Argumento recusa (`bin/codex_pergunta.sh:323`). **DIVERGE P1**: impressão crua cria outra linha (`:217`); leitura linha a linha aceita primeiro fragmento como nome exato (`:278`, `:282`), demais fragmentos sem classe TOCADO são ignorados (`:280`). Falta caso/mutante próprio (`bin/tests/test_codex_pergunta.sh:617`, `:618`). |
| CR | Argumento recusa (`bin/codex_pergunta.sh:323`). **DIVERGE P1**: `open` em modo texto usa leitura universal de quebras; CR do nome vira quebra antes da comparação (`:278`, `:282`); impressão é crua (`:217`). Falta caso/mutante próprio (`bin/tests/test_codex_pergunta.sh:617`, `:618`). |
| Espaço | Argumento recusa (`bin/codex_pergunta.sh:323`). No FS, TSV conserva o espaço; nome com espaço sob pasta declarada pode ser TOCADO permitido, enquanto `views.py espaço` está fora de declaração exata `views.py` (`:217`, `:279`, `:284`). Não confundir a gramática restritiva do argumento com proibição de espaço em todo arquivo descoberto. Caso específico atual `[não verificado]`. |
| Symlink tocado | Nome sintaticamente válido pode ser declarado (`bin/codex_pergunta.sh:323`); inventário usa `lstat` (`:194`), marca symlink PROIBIDO (`:215`), rc 3 (`:219`, `:587`). Caso `c_p_link`/mutante `p_link`: `bin/tests/test_codex_pergunta.sh:366`, `:663`. Symlink transitório desfeito antes do fim `[não verificado]`. |
| Hardlink | Preexistente entra em TOCADO mesmo com conteúdo igual (`bin/codex_pergunta.sh:201`), causando pré-check não vazio rc 2 (`:397`, `:399`); criado no trabalho vira PROIBIDO (`:216`) e rc 3 (`:587`). Casos `p_hardlink`, `p_hardlink_ident`, `revalida_hardlink`: `bin/tests/test_codex_pergunta.sh:367`, `:403`, `:406`; mutantes `inv_nlink`/`p_hardlink`: `:656`, `:657`, `:658`, `:664`. |
| `..`, `//`, absoluto | Argumentos recusados antes da admissão (`bin/codex_pergunta.sh:323`, `:324`). Casos atuais `bin/tests/test_codex_pergunta.sh:554`; mutante `toca_forma` `:731`. Inventário produz caminhos relativos da WT (`bin/codex_pergunta.sh:189`, `:193`), não a grafia original de uma operação. Escape transitório ou escrita fora da WT `[não verificado]`. |
| Fora de `app/` e `bin/` | Argumento recusa (`bin/codex_pergunta.sh:323`); arquivo encontrado dentro da WT mas fora dessas raízes é PROIBIDO (`:151`). Caso `p_fora` `bin/tests/test_codex_pergunta.sh:352`, mutante `:689`. |
| `bin/` comum | Permitido quando coberto pelo próprio TOCA e sem cruzamento (`bin/codex_pergunta.sh:151`, `:284`, `:287`); writable root extra é ativado por declaração `bin/...` (`:539`, `:543`). Caso `bin_ok` `bin/tests/test_codex_pergunta.sh:582`. Mutante específico para concessão do writable root `[não verificado]`. |
| `bin/` protegido | Argumento pode passar e admissão também; inventário proíbe independentemente do TOCA (`bin/codex_pergunta.sh:157`, `:162`, `:210`). Casos todos protegidos `bin/tests/test_codex_pergunta.sh:590`, `:592`; mutantes `lista_protegida` e `p_protegido` `:736`, `:737`. |
| `.claude/` | Raiz `.claude/` é argumento inválido; `app/.claude/...` ou `bin/.claude/...` são argumentos sintaticamente válidos (`bin/codex_pergunta.sh:323`), porém inventário sempre proíbe componente `.claude` (`:147`). Caso `p_claude` `bin/tests/test_codex_pergunta.sh:348`; mutante `:685`. |

Confirmei em memória, sem criar arquivos, o trecho real `_TOCA_PY`: `views.py<TAB>fora`, `views.py<NL>fora` e `views.py<CR>fora` retornam 0 contra declaração exata `views.py`; `views.py espaço` retorna 3. Os sítios que determinam os resultados são `bin/codex_pergunta.sh:217`, `:278`, `:279`, `:282`, `:284`.

Para P2, o teste novo deve provar **admissão e fim rc 0** para irmãos novos `a.py`/`b.py`, e também **fim rc 3** quando a outra declaração passa a cobrir `app/novo/`; os controles distinguem arquivo comum de pai necessário (`CLAUDE.md:306`, `bin/codex_pergunta.sh:284`, `:287`). O mutante exigido deve restaurar a colisão simétrica sobre a pasta pai e morrer no controle positivo; outro deve eliminar a recusa quando a outra raia cobre a pasta e morrer no controle negativo. Esses casos/mutantes futuros estão `[não verificado]`.

Persistência comum: argumento válido admitido é publicado como linhas `toca=` cruas e rename (`bin/codex_pergunta.sh:310`, `:311`), relido por `sed` (`:291`); nomes do inventário são TSV cru (`:217`), acrescidos de PROIBIDO pelo juiz final (`:575`), e `.recusa` retém linhas que não começam com TOCADO (`:579`). Em saída normal, estado é removido (`:236`), chamadas.log ganha `fim` (`:229`), `.rc` registra o retorno (`:232`); inventário proibido força 3 (`:587`), sucesso com resposta nova produz `.id` (`:603`). Como o filtro é por linha crua, fragmentos de nome com newline podem sobreviver em `.recusa` sem estrutura de registro (`:217`, `:579`).

A célula precisa de ESMERIL-SEMÂNTICA é **pai novo × outra declaração de arquivo irmão no juiz final**: admissão diz disjunto (`bin/codex_pergunta.sh:266`, `:273`), fim diz arquivo comum por causa da pasta TOCADA (`:196`, `:287`). P1 acrescenta divergência de representação: argumento validado não contém controles (`:323`), mas inventário e leitor não preservam o nome encontrado (`:217`, `:278`, `:282`).

## 09/11 15:34 · sessao 6b3547444afa · - · - · origem=Ronald (tmux)

Censo textual runtime completo de chamadas diretas nesta cópia:

| Chamador | Dependência da forma atual | Adaptação necessária |
|---|---|---|
| `chamados/services/materializacao.py:129` — `reabrir_selecao` | Todos os elementos têm PK de pergunta | Seleção, trava administrativa e reabertura abrangem só perguntas; testemunha fica factual |
| `chamados/services/materializacao.py:524` — projeção | `metade(q)` lê campos/decisões de pergunta | Projetar testemunha como A, identificar sua origem e manter `par_ids` exclusivo das perguntas |
| `chamados/services/materializacao.py:1433` — ato | Validação, novas, replay, recusa e correção percorrem perguntas | Escrever apenas perguntas; não chamar decisão, carimbo ou resposta sobre testemunha |
| `chamados/services/materializacao.py:1539` — `_materializar_uma` | `{q.pk for q in par}` vira universo/processadas | Testemunha não é pergunta selecionável nem escrita nova |
| `chamados/services/validacao.py:721` | Unidade intersecta seleção elegível | Extrair IDs só das perguntas |
| `chamados/services/passivo_validado.py:385` | DRY verifica universo e conta unidade | Considerar uma pergunta autorizada + testemunha factual, sem acrescentar ID de batida a `autorizadas` |
| `chamados/management/commands/validar_classe_a.py:90` | DRY compara IDs com seleção de perguntas | Mesma extração exclusiva de perguntas |

As chamadas de teste atuais ficam em `chamados/tests/test_bug_par_unidade.py:386`, `:475`, `:586`; há substituição do identificador por tupla artificial em `:502`. Âncoras de mutantes referem o identificador em `chamados/tests/bug_par_mutantes.py:27`, `:55`, `:58`, `:79`, `:103`.

Riscos concretos:

- `chamados/services/acoes_disputa.py:23` exige exatamente duas perguntas; precisa aceitar um ou dois IDs e deixar o ato canônico conferir a identidade completa.
- `chamados/services/materializacao.py:1467` bloqueia recusa se qualquer metade for A. Deve considerar A de pergunta separadamente da testemunha A, para recusar a pergunta e preservar o chão.
- `templates/chamados/partials/_par_intervalo.html:37` gera um campo obrigatório para cada metade. A testemunha deve ser exibida e excluída dos campos de correção.
- `chamados/services/materializacao.py:1499` usa `par[0]` como pergunta; com saída batida, primeiro item pode ser testemunha.
- `chamados/services/materializacao.py:1519` passa `n_mat=2`; precisa contar perguntas efetivamente aplicadas.

Leitores indiretos:

| Leitor | Efeito esperado da projeção canônica |
|---|---|
| Vigia carimbo, `chamados/services/vigia_carimbo.py:107` | Continua julgando a pergunta pelo seu `codigo`; testemunha A não absolve pergunta V/R |
| Vigia fio, `chamados/services/vigia_fio.py:168` | Continua consumindo obrigação conjunta |
| Explicador, `chamados/services/explicador.py:73` | Transporta estado/capacidades novos; pergunta + batida pode estar pronta |
| Auditoria I6/I18, `chamados/management/commands/auditar_invariantes_chamados.py:537`, `:557` | Obrigação só desaparece quando pergunta e testemunha completam aplicação |
| O330, `chamados/services/passivo_validado.py:325` | Inventário autorizado contém perguntas; testemunha-batida não entra em reversão, seleção nem contador de perguntas |
| O330 contagem, `chamados/services/passivo_validado.py:565` | Já conta apenas perguntas presentes no grupo, se `processadas/aplicadas` mantiverem IDs de perguntas |
| Projeção textual, `chamados/services/declaracao_texto.py:477` | `pode_decidir_par` pode ser verdadeiro com uma pergunta respondida e contraparte batida |
| API, `api/views.py:1126` | Transporta esse resultado por `estado_par`, `estado_metade`, `aplicada` e capacidades |
| Tag, `chamados/templatetags/chamados_tags.py:18` | Já consulta a autoridade sem fórmula paralela |
| Cartões/modal, `templates/chamados/partials/_card_atendimento.html:77`, `_pergunta_card.html:20`, `modal_fio.html:160` | Todos usam o include único `_par_intervalo.html` |
| Fechamento explicado, `chamados/services/explicador.py:142` | Continua lendo C7; nenhuma nova fórmula local |

Somente leitura realizada; nenhum runtime ou banco executado. Resultados comportamentais [nao verificado].

## 09/11 15:31 · sessao 08d8d268322f · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

A construção está escrita. A [entrega completa](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:1) contém:

| Entrega | Endereço |
|---|---|
| Tabela arquivo:linha de cada hunk | [relatório:41](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:41) |
| 47 testes e o que cada um morde | [relatório:173](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:173) | [nao verificado]
| Comando individual de cada teste | [relatório:225](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:225) | [nao verificado]
| 40 mutantes, âncoras e comandos | [relatório:283](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:283) |

O juiz do par permanece na ampliação de [estado_aplicacao_pergunta](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/chamados/services/materializacao.py:512). A porta conjunta está em [operar_par_intervalo](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/chamados/services/materializacao.py:1406); a obrigação pronta para C7 consumir está em [obrigacoes_par_intervalo](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/chamados/services/materializacao.py:112). É a autoridade existente de aplicação ampliada para a unidade PAR; a justificativa está no [relatório:7](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:7).

`models.py` permanece para a sessão. Os trechos exatos para C7, captura de `pelo_juiz` antes da linha 1905 e reconsulta após a trava terminada na linha 1920 estão no [relatório:12](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:12). Até esses hunks, ficam previstos RED: V/T técnico, V/V, `fechar`, ambas as transições por S127 e releitura sob trava. **Execução: [nao verificado].**

Foram conferidas sintaxe, âncoras e catracas estáticas; **Django e mutantes não foram executados**. RED/GREEN, mortes, concorrência e efeitos em produção permanecem **[nao verificado]**; não há certificação nesta entrega. ([registro das verificações](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA.r:375))

## 09/11 15:14 · sessao f5a6761ef894 · CODEX-2-RAIAS-PORTAO-CPU · verificador · origem=Code

**REPROVA.** Procedentes: **P1 — escape do `--toca`; P2 — divergência entre admissão e fim; P3 — contador de runs contradiz o contrato declarado; P4 — falso vermelho condicional no selo.** Evidências abaixo. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:284`, `bin/codex_vagas_medida.py:114`, `bin/tests/test_codex_pergunta.sh:720`)

1. **T1 — semântica e escapes**

   **P1: nome com TAB escapa do arquivo exato.** O inventário imprime o nome sem codificação; o leitor separa por TAB e considera somente `c[1]`. Assim, o arquivo real `app/folha/views.py<TAB>fora` é interpretado como `app/folha/views.py` e passa por `--toca app/folha/views.py`. Reproduzi rc 0 executando a função real somente em memória. A validação dos caracteres do argumento não restringe os nomes encontrados no filesystem. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:278`, `bin/codex_pergunta.sh:282`, `bin/codex_pergunta.sh:323`)

   **P2: arquivos novos irmãos admitem e depois recusam.** `--toca app/novo/a.py` e `app/novo/b.py` são disjuntos na admissão. Porém, criar `a.py` também inventaria `app/novo/`: o fim aceita essa pasta como pai necessário ao meu arquivo e simultaneamente a considera cruzada com o arquivo da outra raia. Reproduzi admissão rc 0 e fim rc 3 em memória. Isso contraria o critério escrito de **arquivo em comum**, sem bloquear por família/app. (`bin/codex_pergunta.sh:263`, `bin/codex_pergunta.sh:270`, `bin/codex_pergunta.sh:196`, `bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:287`, `CLAUDE.md:306`)

   A primitiva `cobre/cruza` é compartilhada: arquivo usa igualdade; pasta usa prefixo terminado em `/`. A divergência P2 surge do tratamento adicional das pastas inventariadas. (`bin/codex_pergunta.sh:263`, `bin/codex_pergunta.sh:266`, `bin/codex_pergunta.sh:284`)

   `..`, `/./`, barra dupla, absoluto e raiz diferente de `app/` ou `bin/` são recusados no argumento. Symlink tocado é proibido; a varredura usa `lstat` e não segue diretórios symlink. Ausência de escape transitório durante a execução: **[não verificado]**. (`bin/codex_pergunta.sh:323`, `bin/codex_pergunta.sh:188`, `bin/codex_pergunta.sh:194`, `bin/codex_pergunta.sh:215`)

   O chamador fornece `--toca`; o parser preenche `TOCA[]`; `_publica_estado` grava `toca=`; `_toca_de` lê; `_portoes` compara na admissão e o fim relê a outra vaga. (`bin/codex_pergunta.sh:71`, `bin/codex_pergunta.sh:311`, `bin/codex_pergunta.sh:291`, `bin/codex_pergunta.sh:305`, `bin/codex_pergunta.sh:567`)

2. **T2 — MORTO/ERRADO/VIVO**

   Não encontrei morte acidental concreta nos casos atuais. O acidente de `saida_symlink` foi corrigido: o caso exige `saida … e symlink`, enquanto a guarda seguinte escreve `codex: trava … e symlink`. (`bin/tests/test_codex_pergunta.sh:201`, `bin/codex_pergunta.sh:338`, `bin/codex_pergunta.sh:511`)

   O controlador exige todos os esperados; ERRADO e VIVO aumentam as falhas. O autoteste morde: trocar a decisão por `echo MORTO` reprova outra asserção, dois esperados e esperado vazio. Isso foi exercitado em memória. (`bin/tests/test_codex_pergunta.sh:747`, `bin/tests/test_codex_pergunta.sh:751`, `bin/tests/test_codex_pergunta.sh:753`, `bin/tests/test_codex_pergunta.sh:754`, `bin/tests/test_codex_pergunta.sh:775`)

   Existe uma fragilidade: a busca não ancora a linha; um diagnóstico contendo `FALHA: <esperado>` também pode contar MORTO. Os autotestes não cobrem esse eco; um mutante da lista que efetivamente o produza permanece **[não verificado]**. (`bin/tests/test_codex_pergunta.sh:747`, `bin/tests/test_codex_pergunta.sh:750`)

   **P4: regressão condicional de portabilidade.** O cenário calcula `NPL=nproc-2`, mas dois esperados cravam `load5 6.00`. Com `nproc != 8`, classificam ERRADO uma morte pela asserção pretendida. É falso vermelho; ocorrência no host de produção: **[não verificado]**. (`bin/tests/test_codex_pergunta.sh:100`, `bin/tests/test_codex_pergunta.sh:520`, `bin/tests/test_codex_pergunta.sh:720`, `bin/tests/test_codex_pergunta.sh:722`)

3. **T3 — histórico e trava**

   A derivação paralela foi corrigida: `HIST="$TRAVA.hist"` tem um sítio; a medida pergunta `--hist`. Os eventos produtivos encontrados são escritos por `_hist`, com `ini` após aquisição e `fim` no trap. Os selos também escrevem históricos, em cenários isolados. (`bin/trava_teste.sh:31`, `bin/trava_teste.sh:32`, `bin/trava_teste.sh:53`, `bin/trava_teste.sh:54`, `bin/codex_vagas_medida.py:115`, `bin/tests/test_trava_hist_unico.sh:54`)

   Portanto, **nem todo leitor literalmente pergunta**: o selo antigo usa diretamente `iso.lock.hist`, mas antes confere a resposta de `--hist`; o leitor produtivo pergunta. (`bin/tests/test_trava_teste.sh:112`, `bin/tests/test_trava_teste.sh:145`, `bin/codex_vagas_medida.py:115`)

   **`--presa` pode atrasar um chamador real.** Ele adquire exclusividade brevemente com `flock -n`; o chamador real disputa o mesmo lock com espera finita. Usar `-w` não prova ausência de colisão nem impossibilidade de rc 75 no limite da espera. Ocorrência e impacto reais: **[não verificado]**. L-074 exige exclusividade e rc correto, sem garantir FIFO; trataria a interferência como decisão técnica/fila, corrigindo a alegação absoluta da entrega. (`bin/trava_teste.sh:39`, `bin/trava_teste.sh:40`, `bin/trava_teste.sh:48`, `bin/trava_teste.sh:50`, `app/docs/LEIS.md:101`)

   A regra declarada pode **inflar** ocupação incluindo o intervalo ocioso entre um órfão e o próximo `ini`, ou **zerar** ocupação efetiva do último órfão quando a trava está livre. Isso é consequência da regra explicitada, não motivo novo para reprovar por preferência. (`bin/codex_vagas_medida.py:110`, `bin/codex_vagas_medida.py:142`, `bin/codex_vagas_medida.py:152`)

   A prova de vida não identifica o órfão: leitura do histórico e sondagem são separadas; outro chamador pode adquirir a trava antes de publicar seu `ini`. Nesse intervalo, o órfão antigo recebe a vida da chamada nova. (`bin/codex_vagas_medida.py:130`, `bin/codex_vagas_medida.py:149`, `bin/trava_teste.sh:48`, `bin/trava_teste.sh:53`)

   **Mais de 24h ainda é possível com relógio regressivo.** A sequência temporal `[ini A 00h, fim A 24h, ini B 00h, fim B 24h]` retorna 48h: o escritor usa relógio civil e o leitor soma sem validar regressão ou unir sobreposições. Com timestamps não decrescentes, a máquina de um único aberto elimina a sobreposição antiga dos órfãos. Recuo real do relógio: **[não verificado]**. (`bin/trava_teste.sh:32`, `bin/codex_vagas_medida.py:123`, `bin/codex_vagas_medida.py:139`, `bin/codex_vagas_medida.py:145`)

   **P3: o contador contradiz a declaração.** A docstring promete que todo órfão entra em `n de runs`; o código só incrementa `n` com duração positiva. Um órfão solto retorna `(0 segundos, 0 runs, 1 órfão)`. O selo exige essa exclusão. (`bin/codex_vagas_medida.py:114`, `bin/codex_vagas_medida.py:126`, `bin/codex_vagas_medida.py:128`, `bin/tests/test_trava_hist_unico.sh:57`)

4. **DECISÃO-ABERTA 1–7**

   | Nº | Resposta pela lei ou encaminhamento |
   |---|---|
   | 1 | Retenção/caminho é decisão técnica do Code; não achei exigência de persistência histórica. A limpeza efetiva de `/tmp` no reboot é **[não verificado]**, embora afirmada no comentário. (`CLAUDE.md:273`, `bin/trava_teste.sh:30`) |
   | 2 | **L-003 responde:** fonte ausente não autoriza inventar zero. A exceção vira “não sei”. (`app/docs/LEIS.md:61`, `bin/codex_vagas_medida.py:130`, `bin/codex_vagas_medida.py:193`) |
   | 3 | A opção restritiva já cabe na regra de cura mais restritiva; não pede lei nova. O mesmo caminho com/sem `/` cruza expressamente. (`CLAUDE.md:278`, `bin/codex_pergunta.sh:266`) |
   | 4 | Política técnica de estimativa/retenção; a implementação fecha pelo próximo `ini` sem consultar boot. Não há resposta legal específica identificada. (`CLAUDE.md:273`, `bin/codex_vagas_medida.py:108`, `bin/codex_vagas_medida.py:142`) |
   | 5 | Questão técnica/fila; “nenhum chamador perde a vez” não está demonstrado pelo uso de espera finita. (`CLAUDE.md:273`, `bin/trava_teste.sh:40`, `bin/trava_teste.sh:48`) |
   | 6 | Conferência de ambiente, não pergunta de lei. Ruff do container e equivalência com o resultado entregue: **[não verificado]**. |
   | 7 | O leitor produtivo agora usa `--hist`; existência/conteúdo dos históricos antigos e necessidade real de migração: **[não verificado]**. Não sustento “nada a migrar” como fato de produção. (`bin/codex_vagas_medida.py:115`) |

5. **Regressão no delta**

   Vagas, portões de CPU e lista protegida de `bin/` mantêm os sítios operacionais apresentados na volta 1; as mudanças de comportamento encontradas no delta são P2, P4 e a nova contagem/sondagem do histórico. (`bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:293`, `bin/codex_pergunta.sh:380`, `bin/codex_pergunta.sh:284`, `bin/tests/test_codex_pergunta.sh:720`, `bin/codex_vagas_medida.py:139`)

   `vaga=N` continua no último campo. O leitor posicional conserva os índices anteriores: sessão em `p[6]`, rc em `p[7]`, duração em `p[8]`; não exige tamanho exato. Disjuntor e gerador de estado usam esse leitor. Não encontrei quebra posicional nesses consumidores. (`bin/codex_pergunta.sh:229`, `bin/codex_pergunta.sh:428`, `bin/codex_limite_estado.py:66`, `bin/codex_limite_estado.py:67`, `bin/disjuntor.py:2121`, `bin/gerar_codex_estado.py:176`)

   A leitura semanal reutiliza `leituras_semanais`, sem novo juiz para esse insumo. O relógio da medida entra em `main` e é passado às funções; a borda de horário do wrapper permanece na admissão. (`bin/codex_vagas_medida.py:78`, `bin/codex_vagas_medida.py:226`, `bin/codex_pergunta.sh:300`)

6. **Quem escreve / quem lê**

   | Campo ou arquivo | Escritor → leitor |
   |---|---|
   | `--toca`, `TOCA[]` | Chamador → parser, validação, prompt, admissão e fim. (`bin/codex_pergunta.sh:71`, `:323`, `:465`, `:305`, `:574`) |
   | Caminhos das vagas/meta/estados | Wrapper deriva do common-dir → reserva nomes, abre locks e consulta estados. (`bin/codex_pergunta.sh:85`, `:93`, `:97`, `:380`, `:389`) |
   | Flock das vagas | Wrapper adquire → `_viva` testa o próprio flock. (`bin/codex_pergunta.sh:383`, `:386`, `:253`) |
   | `.estado`: `vaga,pid,obra,wt,hash,hora,toca` | `_publica_estado` grava por rename; `_saida` remove. Decisão lê `toca=`; os demais campos não são lidos por essa decisão. (`bin/codex_pergunta.sh:310`, `:311`, `:236`, `:291`) |
   | Load5, hora, pausas | `/proc/loadavg`, relógio e arquivos de pausa → portões; entradas falsas passam pela guarda de teste. (`bin/codex_pergunta.sh:118`, `:294`, `:297`, `:300`, `:328`, `:112`) |
   | `chamadas.log`: hora, obra, papel, hash, pid, evento, pergunta/sessão, rc, duração, vaga | Wrapper → leitor posicional e contador de vaga 2. (`bin/codex_pergunta.sh:229`, `:428`, `bin/codex_limite_estado.py:49`, `bin/codex_vagas_medida.py:160`) |
   | `vaga2.log`: hora, obra, motivo, load5, fonte | `_recusa_v2` → contagem diária e selo. (`bin/codex_pergunta.sh:249`, `bin/codex_vagas_medida.py:166`, `bin/tests/test_codex_pergunta.sh:521`) |
   | Arquivos da WT, HEAD, gitfile, blobs, modos, links e ignorados | Construtor/git → mesmo inventário no pré-check, revalidação e fim. (`bin/codex_pergunta.sh:174`, `:181`, `:194`, `:397`, `:530`, `:560`) |
   | `.inventario`: classe, caminho, motivo | Inventário e verificações finais → `_TOCA_PY`, decisão rc 3 e `.recusa`. (`bin/codex_pergunta.sh:217`, `:278`, `:560`, `:575`, `:579`) |
   | Resposta/JSONL/stderr; `.id`, `.rc` | Codex escreve saída; wrapper publica resposta e sidecars. `.id` é lido pela triagem; `.rc` é cobrado no selo. (`bin/codex_pergunta.sh:545`, `:553`, `:232`, `:604`, `bin/disjuntor.py:854`, `bin/tests/test_codex_pergunta.sh:174`) |
   | `TRAVA`, `HIST`, eventos `epoch,ini/fim,quem,rc` | `trava_teste.sh` define/publica e escreve eventos → medida pede caminho e lê eventos; `rc` não participa da soma. (`bin/trava_teste.sh:23`, `:31`, `:32`, `:38`, `bin/codex_vagas_medida.py:115`, `:132`, `:144`) |
   | `TRAVA.quem` | Trava escreve → `--quem` e diagnóstico de timeout. (`bin/trava_teste.sh:52`, `:54`, `:37`, `:49`) |
   | Sar: `ldavg-5`, Average e amostras | `sar -q` → parser, desligador e linha diária. (`bin/codex_vagas_medida.py:43`, `:57`, `:66`, `:88`, `:198`) |
   | Sessões: timestamp, janela, used_percent, resets_at | Codex CLI é o produtor declarado; leitor canônico → desligador. Escrita efetiva do CLI: **[não verificado]**. (`bin/codex_vagas_medida.py:15`, `bin/codex_limite_estado.py:154`, `:155`, `:159`, `bin/codex_vagas_medida.py:78`) |
   | Cota: `seven_day_pct,resets_at,hora`; foto anterior | Statusline grava cota → medida calcula diferença entre fotos. Produtor da foto anterior: **[não verificado]**. (`bin/statusline_cota.py:51`, `:54`, `bin/codex_vagas_medida.py:178`, `:180`, `:182`) |
   | Mutantes, esperados, saídas e tabela | Selos fabricam cenários/mutantes → classificadores e resultado final. (`bin/tests/test_codex_pergunta.sh:643`, `:769`, `:775`, `bin/tests/test_trava_hist_unico.sh:80`, `:95`, `:97`) |

**Veredito final: REPROVA — P1 e P2 exigem cura do controle de escopo; P3 exige alinhar contagem e contrato; P4 exige alinhar o esperado ao cenário dinâmico.** GREEN integral da volta 2 não foi reproduzido nesta verificação somente leitura. (`bin/codex_pergunta.sh:217`, `:284`, `bin/codex_vagas_medida.py:114`, `:128`, `bin/tests/test_codex_pergunta.sh:100`, `:720`; GREEN: **[não verificado]**)

## 09/11 15:12 · sessao aafe6042da94 · - · - · origem=Ronald (tmux)

T3 ficou com fonte única de caminho: `TRAVA` em `bin/trava_teste.sh:23`, `HIST="$TRAVA.hist"` em `:31`, publicação por `--hist` em `:38`; o leitor pergunta por `_trava('--hist')` em `bin/codex_vagas_medida.py:103-118`.

- **Escritores/leitores encontrados:** produção escreve eventos exclusivamente por `_hist` (`bin/trava_teste.sh:32`), chamado com `ini` após adquirir a trava (`:47-53`) e `fim` no trap (`:54`). A medida lê os quatro campos, mas ignora `rc` (`bin/codex_vagas_medida.py:130-146`). Os testes escrevem históricos sintéticos isolados (`bin/tests/test_trava_hist_unico.sh:30-34,54`; `bin/tests/test_trava_teste.sh:105-112,145-149`). O teste antigo também conhece `iso.lock.hist` diretamente, porém confere que `--hist` devolve esse caminho (`bin/tests/test_trava_teste.sh:112,145-149`).
- **`--presa` interfere na trava:** abre o lock e tenta adquirir exclusividade com `flock -n`, soltando depois (`bin/trava_teste.sh:39-40`); o chamador real disputa o mesmo lock com espera finita (`:47-50`). Assim, pode ocupar um intervalo livre antes do chamador real e atrasá-lo. A conclusão da entrega “nenhum chamador vira rc75 pela colisão” não decorre do código: o timeout pode vencer durante essa ocupação. Ocorrência real, duração e frequência: **[não verificado]**. L-074 exige exclusividade e rc correto, sem promessa FIFO nem proibição expressa de sondagem (`app/docs/LEIS.md:101`); classificaria a interferência como decisão técnica/fila, sem inventar violação de lei.
- **Regra dos órfãos:** próximo `ini` fecha o anterior (`bin/codex_vagas_medida.py:139-143`); último órfão solto soma zero, preso soma até `agora` (`:147-153`). Pode incluir lacuna ociosa ou zerar ocupação efetiva, mas isso foi explicitamente permitido pela triagem v1 e declarado (`:110-114`); não é motivo novo de reprovação por preferência.
- **Identidade não é provada:** leitura do histórico e `--presa` são operações distintas (`bin/codex_vagas_medida.py:130-153`). Se após EOF um novo chamador prende o lock antes de publicar seu `ini`, o `--presa` pode atribuir vida a um órfão antigo; essa janela existe entre adquirir lock e `_hist ini` (`bin/trava_teste.sh:48-53`). O teste “presa” segura apenas o flock, sem identidade nem evento novo (`bin/tests/test_trava_hist_unico.sh:60-68`), portanto não distingue esse caso.
- **Mais de 24h ainda possível com relógio regressivo:** executei somente em memória a função real extraída por AST, substituindo `open` por `StringIO` e `_trava` por retorno sintético. Eventos `[ini A 00h, fim A 24h, ini B 00h, fim B 24h]` retornam `(172800.0, 2, 0)`, ou 48h. O escritor usa wallclock (`bin/trava_teste.sh:32`); o leitor soma intervalos sem validar sequência temporal nem unir sobreposições (`bin/codex_vagas_medida.py:123-145`). Com eventos temporalmente não decrescentes, a máquina de um único aberto elimina a inflação antiga por sobreposição de órfãos (`:139-146`). Recuo real do relógio: **[não verificado]**.
- **Inconsistência documental concreta:** a docstring promete que todo órfão entra em `n de runs` (`bin/codex_vagas_medida.py:113-114`), mas `n` só aumenta com duração positiva (`:123-128`). Em memória, um único órfão solto retorna `(0.0, 0, 1)`. O selo cristaliza essa exclusão do último órfão solto (`bin/tests/test_trava_hist_unico.sh:54-58`).

Decisões abertas:

1. Retenção em `/tmp`: caminho/encaixe técnico do Code, não pergunta de lei (`CLAUDE.md:273-278`); limpeza efetiva no reboot do host **[não verificado]**, apenas afirmada no comentário (`bin/trava_teste.sh:30`).
2. Ausência → “não sei”: L-003 responde contra inventar número sem fonte (`app/docs/LEIS.md:61`); implementação por exceção de `open` convertida em “não sei” (`bin/codex_vagas_medida.py:130,189-193`).
4. Órfãos anteriores ao boot: mesma regra declarada, sem consulta ao uptime (`bin/codex_vagas_medida.py:108-153`); política de retenção/estimativa é técnica (`CLAUDE.md:273-278`).
5. Probe concorrente: decisão técnica; corrigir a alegação absoluta de inocuidade da entrega (`bin/trava_teste.sh:39-40,47-50`).
7. Migração: não há leitor produtivo remanescente do antigo caminho encontrado no código; único leitor produtivo agora pede `--hist` (`bin/codex_vagas_medida.py:115-130`). Existência/conteúdo real dos históricos antigos: **[não verificado]**; não afirmar “nada a migrar” como fato de produção.

## 09/11 15:12 · sessao 8b64751089c9 · - · - · origem=Ronald (tmux)

T2: **não encontrei morte acidental concreta entre os mutantes declarados**; o classificador agora exige todos os fragmentos esperados, e ERRADO/VIVO reprovam o selo. `bin/tests/test_codex_pergunta.sh:742`, `bin/tests/test_codex_pergunta.sh:775`.

- O acidente anterior de `saida_symlink` foi corrigido: o caso exige a mensagem específica `saida <...>.lock e symlink`; a guarda seguinte escreve `codex: trava <...>.lock e symlink`. `bin/tests/test_codex_pergunta.sh:201`, `bin/codex_pergunta.sh:338`, `bin/codex_pergunta.sh:511`.
- `_recusa` cobra motivo, rc e ausência de chamada; `_inv_recusa` procura PROIBIDO e motivo no terceiro campo. Assim, os esperados “motivo ausente” e “sem PROIBIDO” identificam asserções determinadas, mesmo quando outra guarda ainda recusa a chamada. `bin/tests/test_codex_pergunta.sh:163`, `bin/tests/test_codex_pergunta.sh:169`, `bin/tests/test_codex_pergunta.sh:644`.
- O autoteste morde o controlador convertido em `echo MORTO`: os testes de outra asserção, dois esperados e esperado vazio falham. Reproduzi a função em memória, com leitura simulada pelo `grep`, sem escrever arquivos: original `ERRADO|MORTO|ERRADO|ERRADO|VIVO`; mutado `MORTO|MORTO|MORTO|MORTO|VIVO`. `bin/tests/test_codex_pergunta.sh:750`.
- Limite: o classificador usa substring sem ancoragem; uma linha `diagnostico: FALHA: <esperado>` também conta MORTO. Os autotestes não exercitam esse eco. Não localizei um caso atual que produza essa situação; sua ocorrência na lista permanece **[não verificado]**. `bin/tests/test_codex_pergunta.sh:747`, `bin/tests/test_codex_pergunta.sh:750`.
- **Regressão objetiva de portabilidade do selo:** `NPL=nproc-2` e o motivo do caso são dinâmicos, mas dois esperados cravam `load5 6.00`. Com `nproc!=8`, esses mutantes dão ERRADO mesmo morrendo pela asserção pretendida. É falso vermelho, não falso verde; a VM da autorização tem 8 CPUs segundo o dado recebido **[não verificado na cópia]**. `bin/tests/test_codex_pergunta.sh:100`, `bin/tests/test_codex_pergunta.sh:520`, `bin/tests/test_codex_pergunta.sh:720`, `bin/tests/test_codex_pergunta.sh:722`.

Não executei os selos completos, pois criam e removem arquivos; os resultados GREEN da entrega não foram reproduzidos. `bin/tests/test_codex_pergunta.sh:22`, `bin/tests/test_codex_pergunta.sh:23`, `bin/tests/test_codex_pergunta.sh:769`.

## 09/11 15:11 · sessao 22223ccca92c · - · - · origem=Ronald (tmux)

Dois procedentes em T1:

1. **Escape do `--toca` por TAB ou newline no nome real.** O inventário emite `t` sem escape em TSV (`bin/codex_pergunta.sh:217`), enquanto o consumidor separa por linha/TAB e usa somente `c[1]` (`bin/codex_pergunta.sh:278-282`). Assim, o arquivo real `app/folha/views.py<TAB>fora` é julgado como `app/folha/views.py` e passa por `--toca app/folha/views.py`, embora seja outro arquivo; reproduzi o rc 0 em memória executando `_TOCA_PY` real com `StringIO`, sem escrever arquivo. A lista de caracteres do `--toca` não protege nomes de arquivos encontrados pelo inventário (`bin/codex_pergunta.sh:323`, `bin/codex_pergunta.sh:193`, `bin/codex_pergunta.sh:217`).

2. **Admissão e fim divergem para arquivos novos irmãos sob pai novo.** `app/novo/a.py` versus `app/novo/b.py` não cruzam na admissão (`bin/codex_pergunta.sh:263-275`). Criar `a.py` produz também `TOCADO app/novo/` (`bin/codex_pergunta.sh:195-196`); o fim aceita esse pai como necessário ao meu arquivo, mas o considera cruzado com o arquivo da outra raia (`bin/codex_pergunta.sh:283-288`). Reproduzi em memória admissão rc 0 e fim rc 3. É falsa recusa por pasta comum, contrária ao critério de arquivo comum (`CLAUDE.md:306-308`).

Demais verificações:

- A primitiva de interpretação é compartilhada: sem `/` final é igualdade exata; com `/` é prefixo por componente; admissão e fim chamam a mesma definição (`bin/codex_pergunta.sh:263-287`).
- `..`, `/./`, `//`, caminho absoluto e raiz diversa de `app/` ou `bin/` são recusados no argumento (`bin/codex_pergunta.sh:323-324`). `app/.` terminal passa na forma, mas não cobre descendentes; não identifiquei escape por isso (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:323`).
- Symlink tocado é `PROIBIDO`; o walk não segue symlink e usa `lstat` (`bin/codex_pergunta.sh:188-194`, `bin/codex_pergunta.sh:215`). Ausência de escape transitório durante execução: [não verificado].
- O chamador fornece `--toca`; o parser escreve `TOCA[]`, a validação restringe o formato, `_publica_estado` grava `toca=`, `_toca_de` lê esse campo, `_portoes` julga a admissão e o fim relê o estado da outra raia (`bin/codex_pergunta.sh:71`, `bin/codex_pergunta.sh:321-324`, `bin/codex_pergunta.sh:308-312`, `bin/codex_pergunta.sh:291`, `bin/codex_pergunta.sh:295-306`, `bin/codex_pergunta.sh:565-574`).
- Os controles novos cobrem arquivo exato, pasta por componente e pai novo com uma raia; não cobrem TAB/newline nem dois arquivos irmãos sob pai novo (`bin/tests/test_codex_pergunta.sh:562-570`).

## 09/11 14:51 · sessao 8edd85065351 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · sonda · origem=Code

**O anexo permite contar pares candidatos, mas não quantificar disputas distintas que passariam a ficar retidas.** São **112 pares em disputas abertas e 18 em fechadas**, nas classes citadas. [nao verificado — cálculo sobre o anexo]

Não localizei `sonda_par.py` nesta cópia; portanto, não conferi seu conteúdo nem o hash anunciado. Uso `[nao verificado]` nas contagens do anexo e nas somas derivadas delas, sem confirmação independente de PROD. [nao verificado]

1. **Censo por combinação**

“Fechada” abaixo é total menos aberta, condicionado a ambos os dicionários cobrirem o mesmo recorte. [nao verificado]

| Classe rotulada | Pares totais | Em abertas | Em fechadas | Evidência |
|---|---:|---:|---:|---|
| A/A | 3.793 | 1.494 | 2.299 | [nao verificado] |
| A/R | 49 | 43 | 6 | [nao verificado] |
| A/S | 53 | 52 | 1 | [nao verificado] |
| A/T | 1 | 0 | 1 | [nao verificado] |
| A/V | 5 | 2 | 3 | [nao verificado] |
| R/R | 172 | 168 | 4 | [nao verificado] |
| R/S | 18 | 17 | 1 | [nao verificado] |
| S/S | 2.514 | 2.482 | 32 | [nao verificado] |
| S/V | 4 | 4 | 0 | [nao verificado] |
| T/T | 196 | 0 | 196 | [nao verificado] |
| V/V | 18 | 11 | 7 | [nao verificado] |
| **Total** | **6.823** | **4.273** | **2.550** | [nao verificado] |

R/T, R/V, S/T e T/V não aparecem nos dicionários: contribuição zero **na saída apresentada**, sem comprovar zero na projeção correta da matriz. [nao verificado]

Os 6.823 pares correspondem a **13.646 metades**, distribuídas pelos rótulos: **S=5.103, R=411, V=45, A=7.694, T=393**. Das 18.424 metades declaradas, **4.778 ficam fora dessa tabela**; sua distribuição por estado não foi fornecida. A conta dos grupos fecha: `9.900 − 2.756 sem_par − 321 ambíguos = 6.823`. [nao verificado]

**N não é zero conhecido.** A recusa humana registra `ato='recusa'` na trilha e limpa resposta e validação da projeção; sem E1, uma metade recusada pode parecer S. Isso esconde, por exemplo, S/N dentro de S/S e A/N dentro de A/S. (`app/chamados/services/validacao.py:591`; `app/chamados/services/disputa_emissao.py:1590`; exemplos de redistribuição [nao verificado])

2. **Retenção pelo C7 ampliado e legado**

| Classe candidata | Pares em abertas | Pares em fechadas | Evidência |
|---|---:|---:|---|
| A/R | 43 | 6 | [nao verificado] |
| A/S | 52 | 1 | [nao verificado] |
| A/V | 2 | 3 | [nao verificado] |
| A/T | 0 | 1 | [nao verificado] |
| S/V | 4 | 0 | [nao verificado] |
| V/V | 11 | 7 | [nao verificado] |
| **Total** | **112** | **18** | [nao verificado] |

V/R e V/T não acrescentam ocorrências nos dicionários apresentados. O total separado `V_sem_A=47` não deve ser somado aos pares: mistura unidades e pode sobrepor essas ocorrências. [nao verificado]

**Não dá para chamar esses números de 112 disputas novas retidas e 18 disputas legadas.** Os exemplos repetem `disputa_pk`, inclusive entre combinações, e não fornecem a relação completa para deduplicação. [nao verificado — anexo `EXEMPLOS`]

Também falta demonstrar **“que hoje fecham”**: C7 consulta todas as perguntas da disputa, exigindo ausência de resposta sem veredito e de fio vivo; outra pergunta pode já impedir seu fechamento. (`app/chamados/models.py:1850`; `app/chamados/models.py:1861`)

A brecha para V está comprovada no código: C6 considera qualquer pergunta validada morta imediatamente, e `respostas_sem_veredito()` exclui as validadas, mesmo sem aplicação. Isso demonstra a possibilidade de escape, sem quantificar a população afetada. (`app/chamados/juizes.py:1153`; `app/chamados/models.py:1851`)

Os 18 pares fechados são ocorrências candidatas de legado, sem autorização para correção automática do dado pela L-099; a porta `fechar()` também não reabre disputa já fechada. (`CLAUDE.md:176`; `app/chamados/models.py:1887`; quantidade [nao verificado])

3. **`ESPERA_PAR_INTERVALO`**

O anexo informa **`espera_gravada=0`**. A expressão também não apareceu na busca textual desta cópia. A afirmação histórica “v5 nunca pousou” permanece sustentada apenas pelo anexo. [nao verificado] [nao verificado]

Zero gravado não significa zero obrigação pendente: o próprio anexo declara 47 ocorrências de `V_sem_A`. [nao verificado]

4. **Rótulo versus predicado — LEI-AKITA 8**

Sem o script, **não é possível certificar seu predicado nem atribuir-lhe uma inversão específica de precedência**. A lei exige medir pela função real, com rótulo correspondente ao universo contado. (`CLAUDE.md:35`; implementação da sonda [nao verificado])

Os pontos verificáveis para confrontá-lo com `T > A factual > N atual > V > R/S` são:

- **T antes de A:** linhas fora do fato são identificadas pelas vias `duplicata` e `motivo_retratado`; `fora_do_fato()` não depende de carimbo. Classificar pelo timestamp antes dessa identidade pode encobrir T. (`app/chamados/models.py:2401`; `app/chamados/models.py:3193`)
- **A factual:** `materializada_em` sozinho não prova aplicação. `estado_aplicacao_pergunta()` reconhece explicitamente carimbo de recusa sem batida; o juiz de lastro dispõe de verificações de testemunha factual. (`app/chamados/services/materializacao.py:434`; `app/chamados/juizes.py:645`; `app/chamados/juizes.py:789`)
- **N atual:** `recusa_motivo` também guarda recusa técnica; recusa humana histórica não basta para identificar a decisão vigente. A sequência contém respostas e validações e ordena por `ato_em, pk`. (`app/chamados/models.py:2567`; `app/chamados/models.py:3857`; `app/chamados/models.py:3889`)
- **A função atual não entrega a matriz:** exige pergunta tipo hora com texto e retorna estados de aplicação, sem implementar aquela sequência S/R/V/A/N/T. (`app/chamados/services/materializacao.py:430`; `app/chamados/services/materializacao.py:432`)

5. **O que exige outra leitura**

Permanecem indeterminados: classificação correta das 18.424 metades; quantidade de N; identidade dos pares nos grupos ambíguos; classes das metades fora dos pares únicos; sobreposição dos 28 `pendencia_admin`; e correspondência entre os 47 `V_sem_A` e os 45 rótulos V dos pares únicos. [nao verificado]

Para medir **disputas adicionais retidas**, falta uma leitura que associe cada par à disputa, deduplique os IDs, projete os estados com E1 e testemunha factual e compare, na mesma disputa, C7 atual com a obrigação ampliada. O agregado fornecido não contém esse cruzamento. (`app/chamados/models.py:1861`; `app/chamados/models.py:3839`; `app/chamados/juizes.py:645`; disponibilidade desse cruzamento [nao verificado])

## 09/11 14:50 · sessao 35b635788e2f · - · - · origem=Ronald (tmux)

- `estado_aplicacao_pergunta` ainda não implementa a projeção S/R/V/A/N/T: exige `tipo_resposta='hora'` e texto; trata `materializada_em` como aplicada, exceto `paridade_recusada`; pode responder `ja_satisfeita` por batida aprovada ou `bloqueada_paridade`, sem exigir validação. (`app/chamados/services/materializacao.py:432`, `:434`, `:438`, `:447`, `:452`)
- A factual não se prova apenas por `materializada_em`: o juiz de lastro pede testemunha externa, com caminhos para batida ligada, slot esperado e batida não retratada. (`app/chamados/juizes.py:645`, `:732`, `:734`, `:789`)
- T deve prevalecer: as vias fora do fato são `duplicata` e `motivo_retratado`; ambas podem ter carimbo, tornando inadequado classificar A primeiro apenas pelo timestamp. (`app/chamados/models.py:2401`, `:2518`)
- N fica escondido sobretudo em S quando não se lê E1: a recusa grava motivo e chama reabertura com `ato='recusa'`; a reabertura zera resposta, `respondida_em` e `validada_em`, preservando a foto na sequência. (`app/chamados/services/validacao.py:584`, `:591`; `app/chamados/services/disputa_emissao.py:1590`, `:1612`)
- Recusa histórica não basta para N atual: a sequência inclui respostas e validações posteriores e ordena por `ato_em`, `pk`. O campo `recusa_motivo` também recebe recusa técnica do materializador; portanto não identifica sozinho recusa humana vigente. (`app/chamados/models.py:3857`, `:3889`, `:2567`)
- O script `sonda_par.py` e a matriz `matriz_par` não foram encontrados pelo censo de nomes desta cópia; sua lógica exata, hash e correspondência dos rótulos permanecem **[não verificado]**.

## 09/11 14:50 · sessao e86b8c634a3e · - · - · origem=Ronald (tmux)

- C7 atual aceita somente disputa sem respostas sem veredito e sem fio vivo entre **todas** as perguntas; não consulta obrigação do par. [app/chamados/models.py:1856](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:1856)
- “Resposta sem veredito” exige `validada_em is None`, `materializada_em is None`, resposta por timestamp **ou** texto não vazio, e dia não trancado. [app/chamados/models.py:1850](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:1850)
- C6 mata imediatamente toda pergunta validada; assim, a validação sem aplicação pode desaparecer do fio atual. [app/chamados/juizes.py:1152](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/juizes.py:1152)
- `fio_vivo` usa `any(pergunta_viva(p))`; basta outra pergunta viva na mesma disputa para ela já estar retida hoje. Logo somar pares de classes candidatas não mede disputas distintas nem incremento de retenção. [app/chamados/juizes.py:1262](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/juizes.py:1262), [app/chamados/models.py:1861](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:1861)
- S127 está em `chamados/juizes.py`, não `core/juizes.py`: disputa aberta barra o chamado se vazia ou com fio vivo; não chama C7 inteiro. [app/chamados/juizes.py:2126](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/juizes.py:2126)
- `fechar` verifica resposta pendente e perguntas vivas **antes** do `atomic`/travas; não reconsulta C7 sob trava nesta cópia. [app/chamados/models.py:1894](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:1894), [app/chamados/models.py:1913](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:1913)
- Disputa já fechada: repetição idêntica devolve `False`; outra entrada gera erro, sem reabertura nessa porta. [app/chamados/models.py:1887](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:1887)
- `ESPERA_PAR_INTERVALO` não consta da lista de vias do modelo; busca textual em `app`, `bin` e extensões textuais da cópia retornou zero ocorrências. A ausência global não tem linha positiva para citar. [app/chamados/models.py:2533](/home/ronald/codex_ws/3803e66235912bd9ba1a7aa5f20ee94d1dc307f3/app/chamados/models.py:2533); busca global [não verificado].

## 09/11 14:49 · sessao 4353177d1904 · - · - · origem=Ronald (tmux)

Inspeção somente leitura; nenhuma execução nem edição.

| Sítio | Adaptação necessária |
|---|---|
| `chamados/services/fio.py:225`, `:238` | Incluir intervalo no universo das disputas fechadas e consumir a obrigação da projeção canônica; `q_vivas` seguido de C6 elimina V legado. |
| `chamados/services/fio.py:335` | Já chama `estado_aplicacao_pergunta` por reexport do modelo; pode consumir os novos campos diretamente. |
| `chamados/services/acoes_chamado.py:348` | Manter a pergunta cuja projeção indique obrigação do par, além das demais perguntas vivas por C6. |
| `chamados/services/explicador.py:72` | Ramificar intervalo pela projeção antes de consultar `p.estado`. |
| `chamados/services/explicador.py:85` | A precedência da pendência sobre lastro produz W#5: anuncia ausência de plantio antes de perguntar pelo fato. |
| `chamados/services/explicador.py:137` | Substituir recomposição por `disputa.pode_fechar()`. |
| `chamados/services/declaracao_texto.py:221` | Guarda de declaração deve consumir capacidade canônica para intervalo; não importar bloqueio de espera da v5. |
| `chamados/services/declaracao_texto.py:471` | Acrescentar estado/capacidades à projeção, preservando o significado de `aceita`. |
| `chamados/services/declaracao_texto.py:687` | Aplicação textual singular precisa impedimento explícito antes de escrever resposta/validar; proposta de uma hora não autoriza ato de par. |
| `chamados/services/vigia_carimbo.py:106` | Para intervalo, classificar pelo resultado canônico. V permanece dívida; não importar classe de espera que o retire do alarme. |
| `chamados/services/vigia_fio.py:165` | A redução atual é só C6; consumir obrigação canônica para que V legado continue aparecendo no fio. |
| `chamados/juizes.py:1322` | Alimentar a porta do dia com obrigação do par, além de C6. |
| `chamados/juizes.py:1396` | Só acrescentar V em `vivas` é insuficiente: exige presença em `_no_app`, que pode excluir V. Transportar a capacidade/dono canônicos para permitir a obrigação administrativa sem inventar resposta. |
| `chamados/services/raiox.py:68` | Crédito do painel prefiltra `q_vivas`; obrigação V pode desaparecer antes de `verbos`. Migrar esse alimentador também, mantendo uma única projeção. |
| `chamados/services/passivo_validado.py:516` | Encaminhar seleção completa, processadas/aplicadas e operar uma vez por par. |
| `chamados/services/passivo_validado.py:427` | Inventariar/travar disputas, inclusive fechadas, antes do primeiro dia. |
| `chamados/services/passivo_validado.py:545` | Falha geométrica atualmente carimba recusa individual; para intervalo devolver relato conjunto sem fabricar meia decisão. |
| `chamados/services/passivo_validado.py:363` | DRY já evita chamar a unidade; preservar esse contrato. |
| `chamados/management/commands/auditar_invariantes_chamados.py:536`, `:1075` | I6/I18 têm filtros locais que podem esconder dívida por motivo preenchido; intervalos devem consumir classificação canônica onde a pergunta for aplicação/obrigação. |
| `chamados/management/commands/auditar_invariantes_chamados.py:1112` | I19 permanece técnico, sem conversão em estado do par. |
| `templates/chamados/partials/_card_atendimento.html:183` | Frase atual “conferir antes de lançar” deriva ausência de batida da pendência; exibir fato e conferência separadamente. |
| `templates/chamados/partials/modal_fio.html:221` | Badges atuais priorizam pendência/validação individual; ramificar pela projeção do intervalo. |

Atenção ao OL no O330: a irmã A é retirada do grupo em `passivo_validado.py:326`; exigir ambas no grupo de alvos inviabiliza reparação parcial. O inventário deve distinguir irmã A preservada como testemunha de irmã excluída por `FORA_DO_ESCOPO`; esta última deve impedir o conjunto (`passivo_validado.py:329`).

A catraca antiga só detecta comparações com `None`, filtros `isnull` e `if` direto de atributo, portanto não satisfaz resolução de aliases/auxiliares da catraca do par (`chamados/tests/test_contract_leitores_vitalidade.py:294`). Execução, concorrência e efeitos em produção: [nao verificado].

## 09/11 14:49 · sessao edf66e4dad99 · - · - · origem=Ronald (tmux)

Exploração concluída, sem edições nem execução de Python/Django.

| Ponto | Evidência e consequência |
|---|---|
| C7 atual | `chamados/models.py:1856`: apenas respostas sem veredito e `fio_vivo`; falta consumir obrigação do par. |
| `fechar` | Capturar `pelo_juiz = motivo is None` antes de `chamados/models.py:1905`; reler C7 após `trava_na_ordem`, em `chamados/models.py:1918`, preservando exceções nomeadas. |
| Fotos append-only | `reabrir_para_correcao`, `chamados/services/disputa_emissao.py:1571`, registra RECUSA/REABERTURA com foto antes de limpar resposta e validação; não escreve pendência/motivo. |
| Sequência | `DecisaoPergunta`, `chamados/models.py:3857`; somente RECUSA/REABERTURA aceitam foto em `chamados/models.py:3867`; porta de criação em `chamados/models.py:3943`. |
| Fixture | A fixture anexada usa campos existentes; manter `pk=960`, `nome='col960'`, relógio congelado, CNPJ sintético. CPF `[cpf]` normaliza para vazio: `core/identifiers.py:25` e `colaboradores/models.py:417`. |
| Ambiguidade | Duplicatas de mesmo marco só cabem no acervo anterior ao horizonte do índice: `chamados/models.py:3483`. Não inventar `instancia_marco`: `chamados/models.py:3439`. |
| S127 antigo | Dublês e expectativa precisam migrar para C7 em `chamados/tests/test_disputa_s127.py:17` e `chamados/tests/test_pergunta_viva_no_guarda.py:24`. |
| Teste legado afetado | `chamados/tests/test_fio_intervalo.py:115` espera materializar duas aceitas sem fornecer escala/DNA. Preservar o caso e explicitar abstinência ou fornecer identidade comprovável conforme a lei. |
| Selo de escritores | `chamados/tests/test_porta_validacao_pergunta.py:79` enumera atos com escrita exatos; novo `save` em helper diferente pode exigir ajuste fundamentado do selo. |
| Concorrência | Molde real com duas conexões em `chamados/tests/test_e1_identidade_corrida.py:22`; exige `TransactionTestCase`, porque `TestCase` mantém fixture em transação não visível às threads. |
| Molde de mutantes | Arquivo `e3_mutantes` sob `app/`: [nao verificado — busca não encontrou]. Nenhum caminho externo foi consultado. |

Primeiro aceite singular preservado em outros testes existentes: [nao verificado — busca não encontrou]. Execução, compatibilidade das fixtures e mortes de mutantes: [nao verificado]. [nao verificado]

## 09/11 14:49 · sessao 3e47de222653 · - · - · origem=Ronald (tmux)

Leitura concluída; nenhuma edição nem execução de Python/manage.py.

| Entrada | Adaptação necessária |
|---|---|
| Validar detalhe/modal | Casca `acao_validar_pergunta` já traduz `ok=False`; pode encaminhar pedido explicitamente conjunto à porta do par, mantendo singular recusado. `chamados/services/acoes_disputa.py:19`, `:46`. |
| Recusar | Acrescentar guarda `not r['ok']`: hoje estados novos acabam em toast de sucesso. `chamados/services/acoes_disputa.py:82`, `:87`. |
| Corrigir | Desviar intervalo antes de qualquer nota/resposta: escreve nota em `:359`, responde em `:371` e só depois valida em `:379`. Singular deve sair antes de `:352`. `chamados/services/acoes_disputa.py:345`. |
| Reabrir questionário | `_corrig(d)` pode selecionar uma irmã só; formar e validar universo de pares antes de `_reabrir(qs)`. `chamados/services/acoes_disputa.py:96`, `:103`. |
| Inline | Universo atual vem de C6, portanto pode esconder V legado; laço chama porta singular. Precisa agrupar uma vez por identidade, mantendo universo autorizado. `chamados/views_cobrar.py:169`, `:208`. |
| Lote JSON/tela/chat | Convergem em `validar_lote_classe_a`; correção central alcança JSON. `chamados/views_cobrar.py:247`, `:261`. |
| Comando classe A | `--limite` corta perguntas, podendo separar irmãs; APPLY chama singular. Selection incompleta precisa impedir conjunto. `chamados/management/commands/validar_classe_a.py:69`, `:100`. |
| Fio | Novas ações precisam entrar na porteira contextual e preservar política de disputa fechada. `chamados/services/fio.py:558`, `:573`, `:600`. |
| Detalhe | Gates usam `chamado_que_lista` da pergunta âncora; serviço precisa conferir ambas no escopo autorizado. `chamados/views.py:501`, `:518`, `:536`. |

Templates e resposta:

- `_pergunta_card.html` tem formas singulares em `:76`, `:102`, `:125`; modal em `:264`, `:280`, `:291`, `:302`. Um ramo exclusivo de intervalo com include conjunto evita oferecer meia validação.
- O fragmento htmx atual recarrega apenas a âncora com `{perg, pode_agir}`; precisa renderizar conjunto ou atualizar irmã por OOB. `chamados/views.py:399`, `:404`.
- O cartão do colaborador afirma “antes de lançar” só pela pendência, sem perguntar pelo lastro. `templates/chamados/partials/_card_atendimento.html:183`.
- O modal dá prioridade à pendência sobre aplicação. `templates/chamados/partials/modal_fio.html:221`, `:226`.

Casos antigos a inspecionar para reescrever expectativas, sem apagar: `chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:137`, `:151`, `:159`, `:176`, `:185`; `chamados/tests/test_s139.py:26`; `chamados/tests/test_marco_por_motivo.py:59`.

Comportamento executado, cobertura completa de portas e efeitos concorrentes: [nao verificado].

## 09/11 14:46 · sessao 5a6bf4616dff · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · leitura · origem=Code

**Matriz contra o MAIN informado.** `FALTA` significa comportamento a construir; não significa comportamento já comprovado. Os testes propostos abaixo não foram executados. **[nao verificado: implementação e execução futuras]** [nao verificado]

Há uma diferença de base que precisa ficar explícita: no MAIN, `materializacao.py:1433` é um retorno, e `declaracao_texto.py:485` seleciona marcos aceitos. A escrita de `ESPERA_PAR_INTERVALO` está no **diff v5 fornecido**, não nesses pontos do MAIN. O materializador atual opera uma pergunta, e a projeção atual publica apenas `aceita`. (`app/chamados/services/materializacao.py:1158`, `app/chamados/services/materializacao.py:1433`, `app/chamados/services/declaracao_texto.py:471`, `app/chamados/services/declaracao_texto.py:485`; ausência do símbolo no censo: **[nao verificado: ausência sem endereço próprio]**)

**1. Estados e ESMERIL-SEMÂNTICA**

Os seis nomes solicitados precisam ser uma **projeção derivada**: a propriedade existente só distingue pendente, respondida, validada e resolvida. Não existe hoje uma enumeração exclusiva com os seis estados. (`app/chamados/models.py:3041`)

| Código | Estado da metade | Fonte existente no MAIN | Contrato da leitura nova |
|---|---|---|---|
| S | Sem resposta | `resposta_colab`, `respondida_em`; reabertura limpa ambos. (`app/chamados/models.py:2455`, `app/chamados/services/disputa_emissao.py:1590`) | Ausência de declaração atual; não confundir com recusa humana histórica. **FALTA** |
| R | Respondida | A porta grava resposta e instante. (`app/chamados/services/disputa_emissao.py:1464`) | Declaração presente, sem aceite/aplicação atual. **FALTA** |
| V | Validada | `validada_em/por`, escritos por `_carimbar_validacao`, com decisão E1. (`app/chamados/services/validacao.py:127`, `app/chamados/services/validacao.py:139`) | Aceite sem aplicação comprovada: dívida legada, nunca “aguardando” persistido. **FALTA** |
| A | Aplicada | Carimbo `materializada_em/por/via` pela porta; testemunha julgada por `_pergunta_materializada_de_fato`. (`app/chamados/models.py:3103`, `app/chamados/juizes.py:645`) | Para intervalo, exige resolução factual da metade; carimbo de identidade/retratação não prova batida. **FALTA** |
| N | Recusada | Recusa humana escreve motivo e chama reabertura com `ato='recusa'`; a projeção perde resposta/validação. (`app/chamados/services/validacao.py:584`, `app/chamados/services/validacao.py:593`, `app/chamados/services/disputa_emissao.py:1590`) | Ler a sequência de decisões e respostas posteriores; `recusa_motivo != ''` não identifica recusa atual. **FALTA** |
| T | Retratada/fora do fato | `via_resolucao`, `fora_do_fato()`; inclui duplicata e motivo retratado. (`app/chamados/models.py:2401`, `app/chamados/models.py:3201`) | Terminalidade de identidade, separada de aplicação. Não ressuscitar a linha. (`app/chamados/models.py:3180`, `app/chamados/models.py:3206`) |

**Ordem da projeção proposta:** primeiro identidade fora do fato; depois aplicação factual; depois decisão atual de recusa; depois aceite; depois resposta/ausência. Preservar os indicadores originais e a trilha como evidência, sem apagá-los para fabricar exclusividade. **FALTA — [nao verificado: desenho]**

| Campo | Mais de um significado hoje | Consequência obrigatória |
|---|---|---|
| `validada_em` | Registra aceite e também mata imediatamente a pergunta no C6, mesmo sem aplicação. (`app/chamados/services/validacao.py:134`, `app/chamados/juizes.py:1153`) | Não usar morte no C6 como prova de aplicação do par. **FALTA** |
| `materializada_em` | Carimbo de aplicação e de resolução por identidade, inclusive retratação. (`app/chamados/models.py:3103`, `app/chamados/models.py:3185`) | Não apresentar toda materializada como batida aplicada. **FALTA** |
| `recusa_motivo` | Recusa materializadora, recusa humana, escape e descrição de identidade. (`app/chamados/services/materializacao.py:1102`, `app/chamados/services/validacao.py:584`, `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/models.py:3188`) | Não usá-lo como enumeração do par. Motivo do ato conjunto vai à decisão/trilha. **FALTA** |
| `pendencia_admin` | O comentário diz “recusou plantar”; o rótulo diz “plantada pelo motivo”. O escritor pode gravar pendência e continuar até `registrar_batida`. (`app/chamados/models.py:2555`, `app/chamados/models.py:2562`, `app/chamados/services/materializacao.py:850`, `app/chamados/services/materializacao.py:927`) | Conferência técnica e existência de batida devem ser apresentadas separadamente. **FALTA** |
| `respondida_em` / resposta | Timestamp sozinho não garante conteúdo; a aplicação exige texto não vazio. (`app/chamados/services/materializacao.py:829`) | Incoerência entre texto e instante deve gerar insumo inconsistente, sem aceite/aplicação automática. **FALTA** |
| Fora do fato | Também recebe carimbo e pode ser reconhecida pelo juiz como resolvida sem testemunha de batida. (`app/chamados/models.py:3185`, `app/chamados/juizes.py:670`) | Preservar resolução de identidade sem chamá-la de intervalo aplicado. **FALTA** |

O W semântico já tem base no MAIN: o explicador afirma “NAO plantou” diante da pendência antes de perguntar pelo lastro, embora o ramo possa ter plantado. (`app/chamados/services/explicador.py:85`, `app/chamados/services/explicador.py:87`, `app/chamados/services/explicador.py:93`, `app/chamados/services/materializacao.py:850`, `app/chamados/services/materializacao.py:927`)

**2. Autoridades da matriz**

| Pergunta | Autoridade existente | Por que não resolve sozinha o par / destino |
|---|---|---|
| Quais são as duas perguntas da mesma pausa? | Motivos → S/hii e E/hfi; pausas e pares canônicos do DNA. (`app/chamados/juizes.py:885`, `app/escala/models.py:1166`, `app/escala/models.py:1197`) | Adaptador `identificar_par_intervalo`: **FALTA no MAIN**. Os motivos não carregam índice da pausa. (`app/chamados/services/declaracao_texto.py:48`) |
| Qual o estado de aplicação? | `estado_aplicacao_pergunta`. (`app/chamados/services/materializacao.py:425`) | Hoje avalia uma pergunta e sua vizinha. Deve delegar intervalo à avaliação única do conjunto. Extração do juiz derivado do par: **FALTA** |
| A testemunha factual existe? | `_pergunta_materializada_de_fato`. (`app/chamados/juizes.py:645`) | Reutilizar sua autoridade de lastro, distinguindo as vias de identidade; não duplicar procura de batida nos leitores. (`app/chamados/juizes.py:670`, `app/chamados/juizes.py:732`) |
| A pergunta está viva? | C6, `pergunta_viva`. (`app/core/juizes.py:266`) | Não responde aplicação do par: `validada_em` já devolve falso. (`app/chamados/juizes.py:1153`) |
| A disputa pode fechar? | C7, `DisputaSupervisao.pode_fechar`. (`app/core/juizes.py:267`, `app/chamados/models.py:1856`) | Ampliar a autoridade existente para consumir obrigações derivadas do par. **FALTA** |
| O chamado pode encerrar com essa disputa? | S127, `disputa_viva_do_chamado`. (`app/chamados/juizes.py:2126`) | Hoje recompõe pelo `fio_vivo`; deve consumir C7 inteiro, conservando a exceção de disputa vazia. (`app/chamados/juizes.py:2137`) |
| O vão entre batidas foi intervalo? | `intervalos_do_turno`. (`app/core/juizes.py:202`) | Julga batidas, não identifica duas declarações nem decide aceite administrativo. Não serve como substituto da identidade das perguntas. (`app/core/juizes.py:202`, `app/chamados/services/declaracao_texto.py:48`) |

O juiz derivado proposto é a ampliação da autoridade de aplicação existente, com identidade alimentada pelas fontes acima; telas, C7 e vigias consomem sua resposta. Não haverá um juiz de espera em cada consumidor. **FALTA — [nao verificado: desenho]**

| Identidade | Contrato |
|---|---|
| Par único | Mesma disputa e dia do turno, duas metades inequívocas, marcos pertencentes à mesma pausa declarada. **FALTA**; fontes: `app/chamados/models.py:2424`, `app/chamados/models.py:2431`, `app/escala/models.py:1197` |
| Ambíguo | Multiplicidade de candidatas ou duas pausas sem índice que prove a associação: abstenção explícita. Não escolher primeiro PK nem aplicar avulsa. **FALTA**; lacuna existente: `app/chamados/services/declaracao_texto.py:48` |
| Sem par | Pergunta de intervalo sem contraparte identificável: informar a falta da identidade; nenhuma aplicação individual. **FALTA**; unidade atual individual: `app/chamados/services/materializacao.py:1158` |
| Contraparte retratada | A identidade ativa pode deixar de formar par, mas o aceite residual da metade ainda no fato continua sendo obrigação de fechamento. **FALTA**; risco atual: `app/chamados/juizes.py:1153`, `app/chamados/models.py:1851` |
| Índice inventado | Não preencher `instancia_marco` arbitrariamente para resolver ambiguidade: o modelo exige a derivação do motivo. (`app/chamados/models.py:3436`, `app/chamados/models.py:3441`) |

**3. Contratos de operação, persistência e fechamento usados na matriz**

Todas as operações seguintes são **FALTA como unidade PAR**. As referências identificam as portas existentes a reutilizar.

| Código | Operação e persistência | Tela e prova |
|---|---|---|
| O0 | **Nenhuma operação administrativa do par.** Zero alteração de validação, materialização, pendência ou motivo para representar espera. A resposta continua pela porta existente. (`app/chamados/services/disputa_emissao.py:1464`) | “Aguardando a outra metade”, derivado das declarações; indicar qual falta. Teste `test_espera_nao_escreve`: fotografar campos, decisões, notas e eventos antes/depois. **FALTA** |
| OV | **Validar par.** Duas declarações presentes; travar/reler ambas antes do primeiro carimbo. Validar, registrar/reutilizar as duas batidas pela porta, julgar conjunto e lastro, carimbar ambas e só então reconciliar. Qualquer falha desfaz o ato inteiro. Portas: `app/chamados/services/validacao.py:127`, `app/ponto/registro_batida.py:94`, `app/chamados/models.py:3103` | “Intervalo aplicado” somente após as duas testemunhas. `test_validar_par_atomico`: nenhuma observação intermediária contém só uma aplicação. **FALTA** |
| ON | **Recusar par.** Motivo obrigatório, foto anterior e decisões `RECUSA` das duas metades no mesmo ato; reabertura pela porta. Motivo conjunto em `ato_motivo`/trilha, sem transformá-lo em estado de `recusa_motivo`. (`app/chamados/services/disputa_emissao.py:1590`, `app/chamados/services/disputa_emissao.py:1612`, `app/chamados/models.py:3876`) | “Par recusado; corrigir as declarações”. Não anuncia aplicação nem quitação automática. `test_recusar_par_preserva_fotos`: duas projeções devolvidas, histórico intacto. **FALTA** |
| OC | **Corrigir par.** Ato explícito sobre o conjunto; preserva decisões anteriores e usa as portas de resposta, reabertura e aplicação pertinentes. Não preenche hora ausente por inferência. (`app/chamados/services/disputa_emissao.py:1464`, `app/chamados/services/disputa_emissao.py:1590`, `app/chamados/models.py:3914`) | Mostra as duas metades e o impedimento concreto. `test_corrigir_par_sem_inferencia`: nenhum horário sintético e nenhuma alteração avulsa. **FALTA** |
| OL | **Corrigir aplicação parcial legada.** Rejulgar o conjunto sob a mesma porta; preservar/reutilizar testemunha válida, sem duplicar a metade existente. Dedup e reuso já existem. (`app/ponto/registro_batida.py:113`, `app/chamados/services/materializacao.py:859`) | “Aplicação parcial; corrigir o par”. Não oferecer “validar a que falta”. `test_legado_parcial_reusa_testemunha`: testemunha anterior intacta, resultado conjunto comprovado. **FALTA** |
| OT | **Corrigir obrigação cuja irmã saiu do fato.** Não ressuscitar T. Se o fato continuar cobrado, a identidade deve ser recomposta pelas portas existentes; se deixou de existir, sua resolução deve alcançar a obrigação residual com trilha. (`app/chamados/models.py:3193`, `app/chamados/models.py:3206`) | “Contraparte fora do fato; revisar o par”. `test_irma_retratada_nao_apaga_divida`: aceite residual continua retendo até resolução explícita. **FALTA** |
| OI | **Nenhuma no replay de par aplicado.** Mesmo estado, nenhuma batida ou decisão adicional. Uma revisão com conteúdo diferente é outro ato explícito. Imutabilidade: `app/chamados/models.py:3903` | “Intervalo aplicado”. `test_replay_par_aplicado`: fotografias iguais e zero duplicata. **FALTA** |

| Fechamento | MAIN | Contrato da construção |
|---|---|---|
| C7 | Exige ausência de respostas sem veredito e de perguntas vivas. (`app/chamados/models.py:1861`) | **FALTA:** acrescentar a retenção das obrigações do par, inclusive aceite sem aplicação, independentemente de `pendencia_admin` e `recusa_motivo`. |
| `Disputa.fechar` | Examina respostas/vivas antes do `atomic`; trava depois. (`app/chamados/models.py:1894`, `app/chamados/models.py:1903`, `app/chamados/models.py:1918`) | **FALTA:** no percurso normal, reler C7 sob a trava antes da escrita de fechamento. Preservar as portas nomeadas de exceção e seus contratos. (`app/chamados/models.py:1884`) |
| S127 | Retém disputa vazia ou `fio_vivo`. (`app/chamados/juizes.py:2138`) | **FALTA:** disputa vazia ou C7 retendo. Nenhuma recomposição local do estado do par. |
| `transicionar('fechado'/'resolvido')` | Ambos passam por S127; a guarda adicional de FURO é exclusiva de `resolvido`. (`app/chamados/models.py:148`, `app/chamados/models.py:181`) | **FALTA:** ambos precisam parar pela retenção S127/C7 do par, antes de `_aplicar`. (`app/chamados/models.py:199`) |
| Liberação | C7 continua autoridade da disputa inteira. (`app/chamados/models.py:1856`) | “Liberar” abaixo significa apenas que **este par** não retém; outras perguntas, competência e guardas existentes continuam sendo julgadas. **FALTA** |

**4. Matriz 6×6 reduzida**

A tabela contém as **21 classes por simetria de estados**, cobrindo as 36 posições. A simetria não troca os papéis temporais: saída continua S/hii e volta E/hfi. (`app/chamados/juizes.py:885`)

Não há escritor do par no MAIN que permita declarar combinações entre metades impossíveis: validar, recusar e retratar operam individualmente. A existência dessas portas não prova a ocorrência de cada combinação em produção. (`app/chamados/services/validacao.py:435`, `app/chamados/services/validacao.py:593`, `app/chamados/models.py:2062`; ocorrência populacional: **[nao verificado]**)

**Todas as células de operação/tela/fechamento/prova abaixo são FALTA para o PAR**, com os contratos O acima. O juiz é a ampliação de `estado_aplicacao_pergunta` descrita na seção 2. (`app/chamados/services/materializacao.py:425`)

| Combinação, inclusive transposta | Evidência de leitura/independência no MAIN | Operação → persistência | Tela derivada | Fechamento do par | Teste sugerido + asserção | [nao verificado]
|---|---|---|---|---|---|
| S/S | Respostas e instantes. `disputa_emissao.py:1590` | O0 | Aguardando as duas respostas | Reter enquanto cobrado | `test_SS`: zero aceite, batida ou espera gravada |
| S/R | Resposta individual. `disputa_emissao.py:1464` | O0 | Aguardando a metade S | Reter | `test_SR`: declarar uma não valida nenhuma |
| S/V | Aceite individual. `validacao.py:435` | OC para devolver/corrigir legado; sem validação parcial | Aceite legado sem par pronto | Reter, mesmo com motivo técnico | `test_SV`: C7 falso e zero aplicação |
| S/A | Aplicação individual. `materializacao.py:1220` | OL | Aplicação parcial legada | Reter até resolver conjunto | `test_SA`: não completar silenciosamente |
| S/N | Recusa limpa resposta. `validacao.py:593` | O0; nova declaração pela porta | Aguardando respostas/correção | Reter enquanto cobrado | `test_SN`: recusa histórica não vira resposta |
| S/T | Fora do fato é terminal. `models.py:3201` | Nenhuma aplicação; identidade incompleta | Contraparte fora do fato | Reter cobrança remanescente | `test_ST`: sem fallback avulso |
| R/R | Validação atual por pergunta. `validacao.py:435` | OV / ON / OC | Par pronto para decisão | Reter até ato completo | `test_RR`: um comando decide ambas |
| R/V | Aceite mata só a validada. `juizes.py:1153` | OV / ON / OC sobre ambas | Par com aceite legado | Reter até aplicação/decisão conjunta | `test_RV`: não preservar meia validação nova |
| R/A | Aplicação individual. `materializacao.py:1220` | OL | Aplicação parcial; revisar conjunto | Reter | `test_RA`: reuso não duplica primeira |
| R/N | Recusa e resposta são atos distintos. `models.py:3857` | OC; OV só após duas declarações atuais | Corrigir as declarações | Reter | `test_RN`: não recuperar hora antiga como atual |
| R/T | Identidade terminal. `models.py:3180` | Nenhuma aplicação; recompor identidade primeiro | Sem par ativo completo | Reter resposta/cobrança remanescente | `test_RT`: R não aplica avulsa |
| V/V | C6 mata ambas pelo aceite. `juizes.py:1153` | OV / ON / OC para legado | Aceites sem aplicação | **Reter**; não são quitação | `test_VV`: C7 falso sem duas aplicações |
| V/A | Aceite e aplicação são independentes. `models.py:3043` | OL | Aplicação parcial legada | Reter | `test_VA`: metade aplicada não quita outra | [nao verificado]
| V/N | Recusa da outra não desfaz aceite. `validacao.py:593` | OC sobre conjunto | Aceite residual e declaração recusada | Reter | `test_VN`: motivo de recusa não oculta V |
| V/T | V mata no C6; T também pode morrer por identidade. `juizes.py:1153`, `juizes.py:1164` | OT | Aceite sem aplicação; irmã fora do fato | **Reter — RED W#3** | `test_VT_pendencia`: C7/S127/fechar retêm com motivo não-ESPERA |
| A/A | Carimbo e testemunha. `models.py:3103`, `juizes.py:645` | OI; OC somente para outro ato explícito | Intervalo aplicado | Não retém por aplicação | `test_AA`: replay idêntico, zero nova linha/batida | [nao verificado]
| A/N | Recusa não é reversão de batida. `validacao.py:573` | OL | Aplicação parcial e recusa | Reter até revisão conjunta | `test_AN`: recusar não apaga chão |
| A/T | Retratação não prova aplicação da irmã. `models.py:3185` | OT / OL | Conjunto sem resolução factual integral | Reter obrigação residual | `test_AT`: sem ressurreição de T nem quitação presumida |
| N/N | Recusa devolve perguntas. `disputa_emissao.py:1590` | Nenhuma no replay; aguardar novas declarações | Par recusado, aguardando correção | Recusa não quita cobrança | `test_NN`: repetir não duplica RECUSA |
| N/T | Decisão e identidade distintas. `models.py:3857`, `models.py:3201` | Nenhuma aplicação; revisar identidade/cobrança | Recusa e contraparte fora do fato | Reter cobrança remanescente | `test_NT`: nenhuma hora histórica aplicada | [nao verificado]
| T/T | Ambas fora do fato. `models.py:3201` | Nenhuma | Fato retirado; histórico preservado | Não retém por essas identidades | `test_TT`: C7 conserva regras das demais perguntas |

Nas referências abreviadas da matriz, `models.py`, `juizes.py` e os serviços pertencem a `app/chamados/`.

| Situação adicional | Juiz/operação | Persistência, tela, fechamento e prova |
|---|---|---|
| **AMBÍGUO** | Identificador e avaliação do par: **FALTA**. Não selecionar candidatas arbitrariamente. Lacuna documentada em `app/chamados/services/declaracao_texto.py:48` | Zero aplicação/aceite do pedido singular; tela explica ambiguidade; obrigações existentes continuam retidas. `test_ambiguo_nao_escolhe_pk`: quatro candidatas intactas. **FALTA** |
| **Intervalo SEM PAR** | Identificador: **FALTA**. A aplicação avulsa atual não serve. (`app/chamados/services/materializacao.py:1158`) | Recusa falante: falta contraparte identificada; não dizer “colada”. `test_sem_par_nao_aplica_avulsa`: zero escrita e impedimento explícito. **FALTA** |
| **Resposta/instante incoerentes ou carimbo sem lastro** | Fontes de resposta e lastro existentes. (`app/chamados/services/materializacao.py:829`, `app/chamados/juizes.py:645`) | Não forçar enquadramento saudável nos seis rótulos; devolver impedimento e dívida real. `test_insumo_incoerente`: não aplicar/fechar por ignorância. **FALTA** |

| “Impossível” | Escritor que impede |
|---|---|
| V persistida por **novo** aceite sem aplicação integral | **FALTA:** porta do par deve manter validação e aplicação no mesmo `atomic`; falha reverte ambas. A porta atual 

[truncado em 20000 de 51628 caracteres]

## 09/11 14:40 · sessao 685913bd30a3 · - · - · origem=Ronald (tmux)

Censo realizado por AST em memória, sem importar Django: resolução de `ImportFrom.asname`, `Import.asname`, atributos e atribuições simples; 25 chamadas executáveis de validar/recusar/corrigir/materializar/reparar em runtime. Reflexão, nomes montados dinamicamente e chamadas externas à cópia: **[nao verificado]**.

| Entrada MAIN | Comportamento hoje | Destino no desenho do par |
|---|---|---|
| Detalhe: `app/chamados/views.py:501`, `:518`, `:536` | Despacha validar, recusar e corrigir uma pergunta para as cascas. | Operar par inteiro com identidade comprovada; pedido singular recebe recusa falante antes de escrita. **FALTA**. |
| Fio/modal: `app/chamados/services/fio.py:609`, `:638`, `:639` | Mesmas três cascas; `:609` passa `sem_reabrir` quando disputa estava fechada. | Mesma porta do par e mesmo tratamento de disputa fechada; preservar política de reabertura. **FALTA**. |
| Cascas: `app/chamados/services/acoes_disputa.py:19`, `:80`, `:379` | Chamam `validar_pergunta`, `recusar_pergunta`; correção responde uma pergunta e depois valida. | Formulário coleta ambas as horas e motivo; serviço decide o conjunto. Correção atual não serve como atomic do par: primeiro responde em `:371`, depois valida em `:379`. |
| Aplicar declaração de texto: `app/chamados/views.py:480` → `app/chamados/services/declaracao_texto.py:690` | Abre/acha pergunta, eventualmente grava resposta em `:687`, valida uma em `:690`. | Uma proposta de uma hora não autoriza validar meia pausa: apresentar par e exigir dados completos ou recusa falante. **FALTA**. |
| Validar inline: `app/chamados/views_cobrar.py:208` | Laço de perguntas vivas respondidas, selecionadas em `:169`; uma chamada por item. | Agrupar por identidade; processar cada par uma vez; nunca completar seleção com irmã fora do universo autorizado. **FALTA**. |
| Lote JSON: `app/chamados/views_cobrar.py:261` | Chama `validar_lote_classe_a`. | Mesmo agrupamento por par. **FALTA**. |
| Lote da tela: `app/chamados/views_cobrar.py:381` | Chama o mesmo lote com refs do POST. | Contagem de pares e perguntas separada; seleção incompleta recebe motivo. **FALTA**. |
| Lote do chat/IA: `app/core/views_ia.py:169` | Chama o mesmo lote após gate de permissão em `:165`. | Mesma porta do par; proposta precisa identificar o conjunto. **FALTA**. |
| Escritor do lote: `app/chamados/services/validacao.py:746` | Reclassifica cada pergunta em `:735`; altera `request.POST['pergunta_id']` em `:745`; conta pela validação em `:753`. | Elegibilidade e decisão por conjunto; uma irmã fora da classe/seleção impede aplicar ambas. **FALTA**. |
| Comando assinado: `app/chamados/management/commands/validar_classe_a.py:100` | Chama validação por pergunta; exige usuário em `:90`, DRY retorna em `:85`. | DRY lista pares/ambíguos/sem par; APPLY exige par completo e assinado. **FALTA**. |
| Regularizar detalhe: `app/chamados/services/acoes_disputa.py:139` | Chama materialização granular da disputa. | Processar unidades par sem validar respostas novas implicitamente; legado validado segue matriz de correção. **FALTA**. |
| Regularizar fio: `app/chamados/services/fio.py:670` | Mesmo materializador, importado por reexport de `chamados.models` em `:663`. | Mesmo destino. **FALTA**. |
| Validação individual: `app/chamados/services/validacao.py:463`, `:469` | Primeiro materializa a pergunta atual; depois varre a disputa. | Ramificação de intervalo deve impedir ação singular antes de `_carimbar_validacao`, que hoje ocorre em `:435`; ato conjunto substitui ambos caminhos para intervalo. **FALTA**. |
| Varredura: `app/chamados/services/materializacao.py:1376` | Seleciona respondidas+validadas+sem materialização em `:1369`, aplica individualmente. | Identificar, travar e processar par uma vez; nunca cair em aplicação avulsa por identidade ausente/ambígua. **FALTA**. |
| O330: `app/chamados/services/passivo_validado.py:516` | Dentro do atomic do dia (`:511`), chama unidade individual; mede geometria depois em `:536`. | Par completo dentro do universo do reparador, mantendo atomic do dia/competência, lastro e geometria. Não puxar irmã excluída. **FALTA**. |
| Comando O317/O330: `app/chamados/management/commands/materializar_passivo_validado.py:101`, `:110` | Chama `reparar`; DRY não chama unidade, conforme `app/chamados/services/passivo_validado.py:359`. | Preservar DRY sem escrita e escopo do reparador; relatório passa a contar unidades par. **FALTA**. |
| API de foto: `app/api/foto_recebida.py:51`; chamadas de fachada em `app/api/views.py:886`, `app/api/views_core.py:876` | Universo nominal é `foto_ausente` (`foto_recebida.py:61`, `:73`); ramo `:79` usa todas as perguntas quando `d.motivos` fica sem outros motivos (`:70`). | Não é endpoint nominal de intervalo. Porta central deve recusar meia pausa caso pergunta de intervalo alcance esse universo; foto não pode fornecer horas sintéticas. Entrada efetiva de intervalo por essa inconsistência: **[nao verificado]**. |
| Cron de validação do admin | `validar_classe_a` está fora do pipeline: `app/config/crons.py:1335`; texto diz ato assinado, nunca rotina em `:1336`. | Nenhum cron novo que valide par; rotina só observa estados derivados. |
| Cron do reparador | `materializar_passivo_validado` também fora do pipeline: `app/config/crons.py:825`; ato sob aval em `:826`. | Mantém-se ato, sem automatizar validação. |
| Observadores/fechamento | `auditar_invariantes_chamados`: `app/config/crons.py:542`; `fechar_becos_disputa --alarme`: `:663`; vigia fio: `:1713`; vigia carimbo: `:1751`. | Leem juiz derivado; beco já consulta C7 em `app/chamados/services/becos.py:33`, `:56`. |

**Atomic e portas existentes reaproveitáveis**

| Propriedade | MAIN |
|---|---|
| Validação e tentativa de aplicação no mesmo atomic | `app/chamados/services/validacao.py:425`; trava em `:431`; carimbo em `:435`; aplicação em `:463`. |
| Atomic da aplicação individual e proteção de reentrada | `app/chamados/services/materializacao.py:1220`. Para o par precisa cobrir as duas perguntas antes da primeira escrita: **FALTA**. |
| Lastro após plantio | `app/chamados/services/materializacao.py:1233`; reprovação levanta dentro do atomic em `:1239`. |
| Carimbo pela porta C6 | `app/chamados/services/materializacao.py:1277`. |
| Batida pela porta única | `app/chamados/services/materializacao.py:927`; flags `autoridade_humana=False` em `:929`, `fato_de_chao=False` em `:930`. |
| Vínculo da batida à pergunta | `app/chamados/services/materializacao.py:948`, por `anotar`. |
| Reconciliação hoje acontece após uma metade | `app/chamados/services/materializacao.py:974`; deve passar para depois das duas escritas e carimbos. **FALTA**. |
| Dedup existente | `app/ponto/registro_batida.py:106`, `:113`: mesmo tipo ±60s inclusivos; retorna existente em `:115`. |
| Guardas de competência e colisão continuam na porta | `app/ponto/registro_batida.py:132`, `:145`, `:150`. |
| O330 já julga geometria do conjunto do dia | `app/chamados/services/passivo_validado.py:536`, `:537`; não resolve a recusa individual anterior de cada metade em `:516`. |

**Trilha: detalhe necessário para não prometer uma linha física impossível pelo modelo existente**

| Contrato | Fonte |
|---|---|
| Decisão existente pertence a uma pergunta, não a um par | FK obrigatória em `app/chamados/models.py:3871`; atos em `:3857`. Uma entidade/linha própria do par: **FALTA**. |
| Validação grava uma linha E1 por pergunta | `app/chamados/services/validacao.py:139`. Validar duas metades preservando esse contrato gera duas linhas E1, uma por metade. |
| Recusa grava uma linha por pergunta, com foto anterior | `app/chamados/services/disputa_emissao.py:1612`; comentário explícito “UMA linha por ato, recusa, nunca reabertura + outra” em `:1595`. |
| Recusa hoje limpa resposta e validação | `app/chamados/services/disputa_emissao.py:1590`, update em `:1629`. Portanto “recusada” atual não é resposta mantida mais flag; a foto mora na decisão. |
| Imutabilidade | `app/chamados/models.py:3903`, `:3908`; porta de append `:3943`. |
| Preservar legado antes de substituir projeção | `app/chamados/models.py:3914`; validação chama em `app/chamados/services/validacao.py:133`. |
| Idempotência | Prova deve exigir repetição com zero linhas E1 adicionais, uma linha de cada ato por metade; se requisito exigir “uma linha do par”, uma ocorrência consolidada contendo ambos IDs pode ser desenhada, mas vínculo/porta do ato do par são **FALTA**. Existência desse recurso: **[nao verificado]**. |

A API pública de validar/recusar **intervalo** dedicada não apareceu nesse censo; `api/foto_recebida.py:51` é o único chamador direto de `validar_pergunta` sob `app/api` encontrado por AST. Portas externas, reflexão e execução real permanecem **[nao verificado]**.

_3190 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
