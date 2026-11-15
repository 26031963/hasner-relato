# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 05:52** · publicado em **15/11/2026 05:54** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 05:52 · sessao 59fa89197d9f · - · - · origem=Ronald (tmux)

1. **NULL não exclui a legada da tela do admin.** Fio e modal chamam `montar_chamados_fio`; no modo admin ele carrega todos os chamados do colaborador, sem janela, e todas as perguntas das disputas abertas, sem filtro de motivo/origem. A pergunta aparece sob `chamado_destino` ou, na falta dele, `disputa.chamado`. (`app/chamados/views.py:677`; `app/chamados/views.py:889`; `app/chamados/services/fio.py:190`; `app/chamados/services/fio.py:204`; `app/chamados/services/fio.py:251`; `app/chamados/services/fio.py:255`; `app/chamados/services/fio.py:117`; `app/chamados/services/fio.py:271`)

2. **Admin vê e cobra a muda; valida depois de resposta.** A legada `foto_ausente` é pergunta de escolha; o colaborador possui formulário de opções e porta de resposta que testa propriedade/vitalidade, sem exigir `batida_origem`. No modal admin, muda mostra “Aguardando resposta” e botão Cobrar; resposta não validada ganha Validar. A seleção da ação usa pergunta e a mesma âncora da listagem; a validação exige resposta, sem exigir origem. (`app/chamados/catalogo/perguntas.py:94`; `app/templates/chamados/partials/_card_atendimento.html:77`; `app/templates/chamados/partials/_card_atendimento.html:110`; `app/chamados/views.py:1239`; `app/chamados/views.py:1242`; `app/templates/chamados/partials/modal_fio.html:319`; `app/templates/chamados/partials/modal_fio.html:349`; `app/templates/chamados/partials/modal_fio.html:281`; `app/chamados/services/fio.py:633`; `app/chamados/services/fio.py:636`; `app/chamados/services/validacao.py:410`; `app/chamados/services/validacao.py:418`)

3. **Não fecha automaticamente enquanto só a legada continuar viva.** `pode_fechar` exige ausência de vivas e de resposta sem veredito; `fechar` recusa viva sem porta de exceção. Regularizar pelo modal também recusa viva sem resposta. Depois de resposta e validação, o ramo escolha resolve sem precisar de batida/origem; pode então permitir o fechamento normal. (`app/chamados/models.py:1917`; `app/chamados/models.py:1961`; `app/chamados/services/fio.py:697`; `app/chamados/services/fio.py:698`; `app/chamados/services/materializacao.py:1077`; `app/chamados/services/materializacao.py:1082`; `app/chamados/services/materializacao.py:1086`)

4. **`admin_encerra` é exceção existente, mas não demonstrada como botão/caminho humano de produção.** O método exige motivo e delega ao fechamento excepcional; o fechamento carimba as vivas antes de salvar a disputa. Busca textual encontrou definição e testes, sem chamador produtivo literal; chamada dinâmica: [nao verificado]. Não convém chamar essa existência isolada de porta de tela. (`app/chamados/models.py:2007`; `app/chamados/models.py:2011`; `app/chamados/models.py:2014`; `app/chamados/models.py:1982`; `app/chamados/models.py:1985`)

5. **C17 tem alcance condicional, não garantia universal.** `fio_vivo` reduz o mesmo C6, sem opinião sobre origem. C17 só aceita `celula_id`; carrega perguntas vivas dessa célula e reconhece chamado da célula na fila, ou pergunta viva no app com âncora na fila. O universo do app não filtra origem NULL. Sem célula, C17 retorna `None`. Silenciamento/status/âncora/célula das perguntas 40112 e 56615: [nao verificado]. (`app/chamados/juizes.py:1516`; `app/chamados/juizes.py:1624`; `app/chamados/juizes.py:1628`; `app/chamados/juizes.py:1699`; `app/chamados/juizes.py:1702`; `app/chamados/juizes.py:1706`; `app/chamados/services/pergunta_no_app.py:52`; `app/chamados/services/pergunta_no_app.py:61`)

6. **Nuance relevante do juiz publicado E3:** ele conserva chamados sem FK de célula pelo dia forte e âncoras na fila; porém pergunta de motivo fora dos marcos, como foto, entra apenas por `expor_admin`, que conserva ações de ADMIN. A muda sem decisões produz RESPONDER/COLAB e não vira instância administrativa. Se a única porta residual tiver verbo `cobrar`, não dá dono e a rota publicada pode ser `None`. Isso permite diferença entre **modal/app visíveis**, **C17 alcançável** e **`porta_da_tarefa` sem porta**, em dia cujo único trabalho seja foto muda. É mecanismo preexistente; ocorrência concreta no acervo fornecido: [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:145`; `app/chamados/services/tarefa_do_dia.py:256`; `app/chamados/services/tarefa_do_dia.py:268`; `app/chamados/services/tarefa_do_dia.py:331`; `app/chamados/services/tarefa_do_dia.py:338`; `app/chamados/services/tarefa_do_dia.py:599`; `app/chamados/services/tarefa_do_dia.py:601`; `app/chamados/services/tarefa_do_dia.py:618`; `app/chamados/services/tarefa_do_dia.py:624`; `app/chamados/services/tarefa_do_dia.py:629`)

7. **Nenhuma prova de fio morto invisível por ser NULL.** A condição NULL não participa dos caminhos de listagem, cobrança ou validação acima. A c4 deixa de quitar a muda pelo upload e mantém o fluxo normal de resposta e decisão; “fica com admin” precisa ser lido como **visível/cobrável e validável após resposta**, sem prometer encerramento unilateral da muda pela tela. (`app/api/foto_recebida.py:71`; `app/chamados/services/fio.py:255`; `app/templates/chamados/partials/modal_fio.html:319`; `app/templates/chamados/partials/modal_fio.html:349`; `app/chamados/services/validacao.py:418`)

## 15/11 05:51 · sessao f18502481a68 · - · - · origem=Ronald (tmux)

1. **Retorno 0:** ambas views continuam respondendo `status='ok'`; muda apenas `disputas_fechadas`, após a foto já ter sido anexada. Não há ramificação por esse contador. (`app/api/views.py:851`; `app/api/views.py:861`; `app/api/views.py:863`; `app/api/views_core.py:835`; `app/api/views_core.py:845`; `app/api/views_core.py:847`)

2. **Contador tem significado preexistente de “disputas tratadas”:** incrementa depois da seleção/validação, ignora quantas perguntas validaram e só depois examina fechamento; pode ser 1 com disputa aberta e legada viva. (`app/api/foto_recebida.py:74`; `app/api/foto_recebida.py:77`; `app/api/foto_recebida.py:78`; `app/api/foto_recebida.py:83`; `app/chamados/tests/test_cef_c3_sem_banco.py:175`)

3. **Log/SSE:** anexar foto continua registrando trilha independentemente do retorno; o publisher de batida retorna quando `created=False`. As escritas de pergunta anteriormente feitas no ramo NULL deixavam eventos de beco/outbox; retirando essa seleção, esses efeitos deixam de nascer desse ramo. (`app/ponto/registro_batida.py:309`; `app/ponto/registro_batida.py:256`; `app/ponto/registro_batida.py:265`; `app/ponto/signals.py:39`; `app/ponto/signals.py:42`; `app/chamados/signals.py:229`; `app/chamados/signals.py:253`)

   Consumidor JS/TS/HTML/Java/Kotlin/Dart desse campo não foi encontrado por busca textual; dependência no APK externo: **[nao verificado]**.

4. **Testes mordem a seleção:** origem exata misturada com NULL, contexto trocado A→B e motivos diferentes têm asserções específicas. Os cinco operadores têm alvo único e o runner exige falha de asserção prevista, sem `ERROR`. (`app/chamados/tests/test_cef_c3_sem_banco.py:175`; `app/chamados/tests/test_cef_c3_sem_banco.py:185`; `app/chamados/tests/test_cef_c3_sem_banco.py:199`; `app/chamados/tests/test_cef_c3_sem_banco.py:209`; `app/chamados/tests/test_cef_c3_sem_banco.py:35`; `app/chamados/tests/cef_c3_verificar.py:33`)

   Executei diretamente os módulos com `PYTHONDONTWRITEBYTECODE=1`: controle 29 testes verde; mutantes morreram com 2/4/8/9/1 falhas, sem erros. Corresponde aos registros fornecidos. (`app/chamados/tests/cef_c4_green.txt:32`; `app/chamados/tests/cef_c4_mutantes.txt:63`; `app/chamados/tests/cef_c4_mutantes.txt:140`; `app/chamados/tests/cef_c4_mutantes.txt:302`; `app/chamados/tests/cef_c4_mutantes.txt:484`; `app/chamados/tests/cef_c4_mutantes.txt:525`)

5. **Mutante plausível vivo:** retirei somente `fechada_em__isnull=True` da seleção, em `c1.[nome]`, mantendo tudo em memória: os 29 testes continuaram verdes. As fixtures só oferecem uma disputa aberta, portanto não demonstram exclusão de disputa fechada. Registro persistido desse ensaio: **[nao verificado]**. (`app/api/foto_recebida.py:66`; `app/chamados/tests/test_cef_c1_sem_banco.py:210`; `app/chamados/tests/test_cef_c1_sem_banco.py:226`; `app/chamados/tests/test_cef_c1_sem_banco.py:27`)

6. **Limite relevante:** `_validar_pelo_dono` é substituído por double que apenas marca `viva=False`; `fio_vivo` e `pode_fechar` também são doubles. A integração C3 prepara foto com `.update()`, chama diretamente o resolver e não exercita upload completo nem porta admin. Execução ORM: **[nao verificado]**. (`app/chamados/tests/test_cef_c1_sem_banco.py:215`; `app/chamados/tests/test_cef_c1_sem_banco.py:219`; `app/chamados/tests/test_cef_c1_sem_banco.py:227`; `app/chamados/tests/test_cef_c1_sem_banco.py:230`; `app/chamados/tests/test_cef_c3_integracao.py:46`; `app/chamados/tests/test_cef_c3_integracao.py:48`)

7. **Classes do delta:** ele conserva os donos de resposta/validação e os juízes de vitalidade/fechamento, removendo a derivação contexto→origem NULL. O relógio interno é preexistente. Não há piora demonstrada dos caminhos A1/`nota.py`/`renovar_contexto` pelo trecho produtivo alterado. (`app/api/foto_recebida.py:42`; `app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/api/foto_recebida.py:71`; `app/api/foto_recebida.py:81`; `app/api/foto_recebida.py:83`)

## 15/11 05:51 · sessao 82681902e4d0 · - · - · origem=Ronald (tmux)

1. **O resolver C4 não entrega NULL ao escritor de resposta/validação.** Seleciona exclusivamente `motivo='foto_ausente', batida_origem=batida`; sem candidata, sai antes das portas e do fechamento. (`app/api/foto_recebida.py:71`; `app/api/foto_recebida.py:72`; `app/api/foto_recebida.py:74`)

2. **Há efeitos genéricos do upload capazes de carimbar/fechar acervo NULL, mas não demonstram o erro A2.** `anexar_foto` chama `anotar`, cujo `save` dispara a reconciliação do colaborador; esta inclui perguntas de qualquer motivo/origem cuja célula absolve e cujo C6 já declara mortas. Carimba `materializada_em/via`, preservando resposta/validação, e fecha a disputa se `pode_fechar`. É alcance genérico pelo C6, não casamento foto↔NULL pelo contexto. (`app/ponto/registro_batida.py:309`; `app/ponto/registro_batida.py:256`; `app/ponto/signals.py:206`; `app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1433`; `app/chamados/reconciliador.py:628`; `app/chamados/reconciliador.py:662`; `app/chamados/reconciliador.py:596`; `app/chamados/reconciliador.py:612`)

   **Contraexemplo de legada viva antes do upload que morra apenas pela presença da foto: [nao verificado].** O C6 pode absolver pela célula; no tipo `escolha`, o lastro lê `materializada_em`, e sem carimbo o ramo final mantém viva. (`app/chamados/juizes.py:1450`; `app/chamados/juizes.py:1465`; `app/chamados/juizes.py:709`; `app/chamados/juizes.py:1479`)

3. **Outra cauda pode materializar irmã NULL previamente respondida e validada.** Validar uma pergunta exata chama o materializador da disputa inteira, que busca todas com ambos os carimbos e sem materialização; não responde/valida uma NULL pendente nesse caminho. (`app/chamados/services/validacao.py:494`; `app/chamados/services/materializacao.py:1375`; `app/chamados/services/materializacao.py:1381`)

4. **Escritores persistidos de `respondida_em`/`validada_em`:**
   
   - Resposta humana: escreve resposta/carimbo e desfaz validação anterior. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`)
   - Resposta automática: escreve resposta/carimbo; único chamador produtivo encontrado é o helper da foto. (`app/chamados/services/disputa_emissao.py:1673`; `app/api/foto_recebida.py:48`)
   - Recuperação pela trilha: preenche carimbo faltante, chamada pelo comando de cura de limbo. (`app/chamados/services/disputa_emissao.py:1691`; `app/chamados/services/disputa_emissao.py:1695`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`)
   - Validador: grava validação/autoria; devolução recusada pode zerá-las. (`app/chamados/services/validacao.py:159`; `app/chamados/services/validacao.py:224`)
   - Porta `carimbar`, com texto e opção `resposta`: preenche os carimbos ausentes; chamada pelo materializador e S128. (`app/chamados/models.py:3213`; `app/chamados/models.py:3218`; `app/chamados/services/materializacao.py:1283`; `app/chamados/services/disputa_emissao.py:2007`)
   - Reabertura e ressuscitação: zeram carimbos pela porta de lote. (`app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1648`; `app/chamados/models.py:1613`; `app/chamados/models.py:3363`)
   - Nascimento: recebe assinaturas em `**campos`, persiste e registra os atos. (`app/chamados/models.py:3097`; `app/chamados/models.py:3100`; `app/chamados/models.py:3102`)

   As atribuições em cópia de simulação e restauração da memória não constituem escritores persistidos independentes. (`app/chamados/models.py:1541`; `app/chamados/models.py:1554`; `app/chamados/models.py:3242`)

5. **Legada ainda viva impede fechamento normal.** `pode_fechar` exige ausência de resposta sem veredito e de fio vivo; `fechar` recusa vivas sem exceção nomeada. Admin pode encerrar com motivo, carimbando identidade `admin_decidiu`; esse ato não exige origem preenchida e não escreve resposta/validação. (`app/chamados/models.py:1917`; `app/chamados/models.py:1961`; `app/chamados/models.py:1982`; `app/chamados/models.py:2007`; `app/chamados/models.py:3147`)

**Nenhum novo caminho de quitação NULL por contexto da foto foi demonstrado; o caminho incorreto A2 foi retirado.** (`app/api/foto_recebida.py:71`)

## 15/11 05:48 · sessao aa3076601e72 · CONTRATO-DE-ENTRADA-DO-FIO · construtor · origem=Code

Cura aplicada: upload seleciona somente a origem exata; legadas ficam com o admin ([foto_recebida.py:60](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/api/foto_recebida.py:60)). [nao verificado]

[Entrega completa](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c4_entrega.txt:53): RED com três falhas, GREEN com 29 testes, cinco mutantes mortos, censo e comandos. [nao verificado]

Ruff indisponível; integração e certificação pelo Code/opus pendentes ([registro](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c4_entrega.txt:164)).

## 15/11 05:40 · sessao a8ee662a6825 · CONTRATO-DE-ENTRADA-DO-FIO · verificador · origem=Code

**REPROVA.** A1 corrige o apagamento sequencial apontado na v1; A2 pode validar pergunta legada de **outra batida** usando a última identidade do contexto agregado. (`app/chamados/services/disputa_emissao.py:569`; `app/api/foto_recebida.py:70`; `app/chamados/models.py:3599`)

1. **A1 — cura correta, com alcance limitado.**

   Mesmo `id` conhecido: `{**anterior, **novo}` preserva `recebida_em` omitida e deixa valor explicitamente enviado vencer. Outro `id`, identidade ausente ou bloco incompatível: substituição integral. Isso cura API→retro consecutivos da mesma batida; não transforma o contexto em histórico de episódios. (`app/chamados/services/disputa_emissao.py:569`; `app/chamados/services/disputa_emissao.py:573`; `app/chamados/services/disputa_emissao.py:575`)

   **Reuso por `nota.py` não repete esse apagamento:** escreve `Chamado.contexto_json`, delegando a `fundir_contexto`; dicionário já presente não é substituído. Entretanto, também não incorpora uma recepção nova se `ultima_batida` já existir sem ela, podendo conservar inclusive um episódio antigo. É comportamento distinto da fusão da disputa. (`app/chamados/services/disputa_emissao.py:600`; `app/chamados/services/nota.py:110`; `app/chamados/services/nota.py:123`; `app/chamados/services/contexto.py:155`)

   Os outros escritores examinados preservam o bloco sequencialmente: preenchimento de motivo/data, remoção da chave de um motivo e acréscimo de `revalidacoes`. O envelope atribuído no gerador pertence a uma cópia transitória. (`app/chamados/services/disputa_emissao.py:291`; `app/chamados/services/disputa_emissao.py:312`; `app/chamados/models.py:2028`; `app/chamados/models.py:2104`; `app/chamados/models.py:2128`)

   **Há possibilidade preexistente de perda por concorrência:** o cron carrega contexto antigo; a API salva a recepção; depois `renovar_contexto('ultimo_alerta_diario', ...)` salva o JSON inteiro da instância antiga. Essa porta não relê o contexto antes de gravar. Ocorrência concreta na sombra: **[nao verificado]**. (`app/ponto/management/commands/alertar_disputas_pendentes.py:33`; `app/ponto/management/commands/alertar_disputas_pendentes.py:48`; `app/chamados/models.py:2182`; `app/chamados/models.py:2187`)

2. **A2 — bloqueio: contexto de B pode quitar pergunta NULL de A.**

   Cenário: pergunta de foto da batida A permanece viva, com origem NULL; outra emissão B reutiliza a disputa e substitui `ultima_batida` por B; chega foto de B. O ramo novo inclui **todas** as perguntas `foto_ausente` NULL daquela disputa, inclusive a de A. Não compara dia nem vínculo individual. O próprio modelo documenta que esse contexto, depois do nascimento, pode apontar outra anomalia. (`app/chamados/services/disputa_emissao.py:540`; `app/chamados/services/disputa_emissao.py:575`; `app/api/foto_recebida.py:70`; `app/api/foto_recebida.py:72`; `app/chamados/models.py:3599`)

   As portas seguintes não corrigem a seleção: recebem pergunta/disputa, sem receber a batida do upload. O ramo `escolha` de `foto_ausente` pode materializar sem conferir foto ou episódio; a validação já retira a pergunta da vitalidade. Portanto, **usar o dono correto da escrita não garante que ele recebeu a pergunta correta**. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/chamados/catalogo/perguntas.py:94`; `app/chamados/services/materializacao.py:1082`; `app/chamados/services/materializacao.py:1238`; `app/chamados/juizes.py:1404`)

3. **Sombra — duas candidatas, quantidade com associação errada desconhecida.**

   O dado fornecido registra 15 perguntas vivas NULL e duas com disputa aberta/contexto com ID: pergunta **40112**, disputa **5158**, batida contextual **118315**; pergunta **56615**, disputa **8065**, batida contextual **155158**. Isso identifica candidatas, não comprova a origem histórica delas. (`sonda.out:3`, anexo fornecido; `app/chamados/tests/cef_c3_sonda.py:23`; `app/chamados/tests/cef_c3_sonda.py:31`)

   **Quantas dessas duas caem no cenário: [nao verificado].** A sonda não mede correspondência pergunta↔episódio, nem exige o motivo na lista atual da disputa, exigência existente no resolver. (`app/chamados/tests/cef_c3_sonda.py:23`; `app/chamados/tests/cef_c3_sonda.py:33`; `app/api/foto_recebida.py:64`)

   Para contar, faltam elegibilidade completa do resolver, existência/posse da batida contextual e lastro independente da batida que originou cada pergunta. Comparar dia do turno pode revelar incompatibilidade, mas **dia igual não prova episódio igual**: o acervo NULL tem somente chave de dia. Se o lastro disponível não distinguir episódios, o resultado deve continuar indeterminado. (`app/chamados/models.py:3610`; `app/chamados/models.py:3599`; `app/chamados/services/fio.py:1286`)

   Não reabro o bloqueio antigo da medição 140/0, descartado pela triagem fornecida. (`triagem_v1.txt:6`, anexo fornecido)

4. **O `|` não causa o erro de associação.**

   Os conjuntos são disjuntos: `batida_origem=batida` versus `batida_origem IS NULL`, ambos no mesmo relacionamento da disputa. Não há sobreposição entre os ramos que explique duplicação; a união inclui o conjunto legado inteiro quando o contexto casa. Quando não casa, as NULL ficam excluídas deliberadamente. O problema é esse critério de inclusão. (`app/api/foto_recebida.py:68`; `app/api/foto_recebida.py:70`; `app/api/foto_recebida.py:72`)

   Execução SQL real: **[nao verificado]**. O teste sem banco usa uma implementação própria de união, que não demonstra comportamento do ORM. (`app/chamados/tests/test_cef_c1_sem_banco.py:49`; `app/chamados/tests/test_cef_c3_sem_banco.py:3`)

5. **Escritores e leitores dos valores envolvidos.**

   | Valor | Quem escreve | Quem lê |
   |---|---|---|
   | `ultima_batida.id` | APIs e retro; fusão persiste na disputa. (`app/api/views.py:762`; `app/api/views_core.py:756`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:99`; `app/chamados/services/disputa_emissao.py:576`) | Comparação A1; associação legada A2; busca do dia/episódio no nascimento. (`app/chamados/services/disputa_emissao.py:571`; `app/api/foto_recebida.py:70`; `app/chamados/services/disputa_emissao.py:396`; `app/chamados/models.py:3606`) |
   | `tipo` | APIs e retro. (`app/api/views.py:763`; `app/api/views_core.py:757`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:100`) | Transporte genérico pelo merge; leitor semântico específico: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:573`) |
   | `timestamp` | APIs: instante local do fato; retro: timestamp local da batida. (`app/api/views.py:764`; `app/api/views_core.py:758`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`) | Juiz fraco extrai a data da string. (`app/chamados/juizes.py:162`; `app/chamados/juizes.py:169`) |
   | `recebida_em` | APIs; retro omite; A1 conserva a omissão no mesmo episódio. (`app/api/views.py:765`; `app/api/views_core.py:759`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/chamados/services/disputa_emissao.py:573`) | Merge genérico; consumidor semântico específico: **[nao verificado]**. O juiz fraco lê `timestamp`. (`app/chamados/juizes.py:163`) |
   | `Disputa.contexto` / `Chamado.contexto_json` | Emissão escreve ambos no nascimento; agrega contexto da disputa; reuso do chamado passa por nota/contexto. (`app/chamados/services/disputa_emissao.py:594`; `app/chamados/services/disputa_emissao.py:604`; `app/chamados/services/disputa_emissao.py:576`; `app/chamados/services/nota.py:123`) | Gerador usa envelope do ato; upload legado usa agregado; template exibe contexto do chamado. (`app/chamados/services/disputa_emissao.py:315`; `app/api/foto_recebida.py:70`; `app/templates/chamados/partials/painel_chamado_turno.html:35`) |
   | Pergunta: `motivo`, `batida_origem`, `data_evento` | Porta de nascimento recebe motivo, deriva origem e grava dia pela porta do fio. (`app/chamados/models.py:2903`; `app/chamados/models.py:2949`; `app/chamados/models.py:3097`; `app/chamados/models.py:3099`) | Dedup lê episódio/acervo; upload lê motivo e origem, sem usar dia no ramo legado. (`app/chamados/models.py:2798`; `app/api/foto_recebida.py:68`; `app/api/foto_recebida.py:72`) |
   | Resposta e validação | `responder_automaticamente` grava texto/carimbo; validação grava carimbo/autor. (`app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/validacao.py:159`) | Validador, materializador e juiz de vitalidade. (`app/chamados/services/validacao.py:418`; `app/chamados/services/materializacao.py:1375`; `app/chamados/juizes.py:1403`) |
   | Motivos e fechamento da disputa | Emissão agrega motivos; portas existentes aparam motivo e fecham disputa. (`app/chamados/services/disputa_emissao.py:550`; `app/chamados/models.py:2030`; `app/chamados/models.py:1996`) | Resolver seleciona abertas com `foto_ausente`; consulta vitalidade e possibilidade de fechar. (`app/api/foto_recebida.py:62`; `app/api/foto_recebida.py:82`; `app/api/foto_recebida.py:84`) |

6. **Testes mordem os cortes escolhidos, mas não o bloqueio encontrado.** [nao verificado]

   Os quatro operadores registrados têm asserções específicas: preservar sempre, substituir sempre, retirar legado e aceitar legado sem conferir ID. O runner distingue falha esperada de erro de execução. (`app/chamados/tests/cef_c3_verificar.py:12`; `app/chamados/tests/cef_c3_verificar.py:31`)

   A fixture sem banco assume uma NULL associada ao contexto atual; a integração apaga a origem justamente da última batida emitida. Nenhuma exercita **NULL de A → contexto atualizado para B → upload de B**, nem múltiplas NULL de episódios diferentes. (`app/chamados/tests/test_cef_c3_sem_banco.py:140`; `app/chamados/tests/test_cef_c3_integracao.py:33`; `app/chamados/tests/test_cef_c3_integracao.py:38`)

   **Mutante plausível vivo na C3 sem banco:** retirar `motivo='foto_ausente'` apenas do filtro legado, passando a selecionar NULL de outros motivos. O ensaio em memória manteve os dez testes verdes; as fixtures desse caminho só contêm `foto_ausente`. Registro persistido desse ensaio: **[nao verificado]**. (`app/api/foto_recebida.py:72`; `app/chamados/tests/test_cef_c3_sem_banco.py:140`; `app/chamados/tests/test_cef_c1_sem_banco.py:208`)

   ORM, persistência, concorrência e validação real continuam fora da prova sem banco; execução da integração nesta revisão: **[nao verificado]**. (`app/chamados/tests/test_cef_c3_sem_banco.py:3`; `app/chamados/tests/test_cef_c1_sem_banco.py:219`; `app/chamados/tests/test_cef_c3_integracao.py:1`)

7. **Classes de reprovação.**

   **Derivação paralela / campo com dois significados: presente em A2.** O último episódio do agregado passa a funcionar como origem presumida de cada pergunta NULL, contrariando a distinção documentada entre contexto corrente e episódio da pergunta. (`app/api/foto_recebida.py:70`; `app/chamados/models.py:3599`; `app/chamados/services/fio.py:1286`)

   **Segundo juiz de vitalidade ou segundo escritor de resposta/validação: nenhum novo demonstrado nesses trechos.** As portas e `fio_vivo` são reutilizados; o defeito está no universo entregue a elas. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/api/foto_recebida.py:82`)

   **Relógios internos são preexistentes**, para `ultima_em` e resposta automática; a convivência de timestamp legado de recepção com timestamp novo do fato também já está documentada. (`app/chamados/services/disputa_emissao.py:567`; `app/api/foto_recebida.py:42`; `app/chamados/juizes.py:152`; `app/chamados/juizes.py:155`)

   **O selo tem mordida parcial:** verifica estrutura do envelope, mas não comprova correspondência histórica entre pergunta NULL e contexto. Os quatro mutantes mortos não cobrem essa propriedade. (`app/chamados/tests/test_e1_v5_familia.py:133`; `app/chamados/tests/test_e1_v5_familia.py:170`; `app/chamados/tests/cef_c3_verificar.py:12`)

## 15/11 05:38 · sessao 862111608ebb · - · - · origem=Ronald (tmux)

**A2 continua inseguro: o ramo legado confunde contexto agregado com identidade da pergunta.** (`app/api/foto_recebida.py:70`; `app/chamados/models.py:3599`)

1. **Cenário demonstrável:** pergunta A `foto_ausente`, origem NULL, permanece viva; uma nova emissão B reutiliza a disputa e substitui `ultima_batida` por B; chega upload de B. O ramo legado seleciona A porque filtra apenas motivo/NULL na disputa, sem conferir episódio ou dia da pergunta. Isso pode ocorrer mesmo se B nasceu por outra anomalia. (`app/chamados/services/disputa_emissao.py:540`; `app/chamados/services/disputa_emissao.py:575`; `app/api/foto_recebida.py:70`; `app/api/foto_recebida.py:72`)

2. **As portas existentes não corrigem essa seleção.** O helper passa somente pergunta/disputa ao respondedor e validador; nenhuma batida é passada para verificar correspondência. O respondedor grava texto/carimbo; o validador busca por PK/disputa e carimba a validação. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/validacao.py:410`; `app/chamados/services/validacao.py:460`; `app/chamados/services/validacao.py:159`)

   `foto_ausente` tem resposta `escolha`; esse ramo reconcilia destino e retorna verdadeiro sem consultar foto/episódio. A exigência posterior de testemunha aplica-se a `hora`/`data_hora`; pergunta validada morre imediatamente pelo juiz. (`app/chamados/catalogo/perguntas.py:94`; `app/chamados/services/materializacao.py:1069`; `app/chamados/services/materializacao.py:1085`; `app/chamados/services/materializacao.py:1238`; `app/chamados/juizes.py:1404`)

3. **Das duas da sombra, quantidade afetada: [nao verificado].** A sonda só observa pergunta viva NULL, disputa aberta e ID presente; não verifica se o ID representa a pergunta, sequer exige `foto_ausente` nos motivos atuais da disputa como o resolver exige. (`app/chamados/tests/cef_c3_sonda.py:23`; `app/chamados/tests/cef_c3_sonda.py:27`; `app/chamados/tests/cef_c3_sonda.py:31`; `app/api/foto_recebida.py:64`)

   Medir para `(40112,5158,118315)` e `(56615,8065,155158)`: elegibilidade completa da disputa/upload, existência e dono da batida do contexto, dia do turno versus dia da pergunta e, principalmente, evidência independente da batida que originou a pergunta. Os pares vêm somente do anexo fornecido: [nao verificado]. Dias diferentes podem revelar incompatibilidade; dias iguais não demonstram igualdade de episódio, pois o acervo tem apenas chave de dia. (`app/chamados/services/fio.py:1286`; `app/chamados/models.py:3599`)

4. **O `|` não explica o defeito lógico.** Os predicados `origem=batida` e `origem IS NULL` são disjuntos e usam o mesmo manager da disputa; o problema é o universo indevido incluído pelo segundo predicado. (`app/api/foto_recebida.py:68`; `app/api/foto_recebida.py:71`)

   SQL real, plano e comportamento efetivo do Django: **[nao verificado]**. Django não está disponível neste ambiente; o double implementa sua própria união sem repetição e os testes declaram não demonstrar SQL. (`app/chamados/tests/test_cef_c1_sem_banco.py:49`; `app/chamados/tests/test_cef_c3_sem_banco.py:3`)

5. **Testes não cobrem deslocamento posterior do contexto.** A fixture sem banco só chama a NULL de “legada de 101”, sem identidade independente; a integração também deixa por último exatamente o episódio convertido em NULL. Não exercita legada A seguida por atualização do contexto para B e upload B. (`app/chamados/tests/test_cef_c3_sem_banco.py:140`; `app/chamados/tests/test_cef_c3_sem_banco.py:153`; `app/chamados/tests/test_cef_c3_integracao.py:33`; `app/chamados/tests/test_cef_c3_integracao.py:38`)

6. **Mutante plausível vivo confirmado em memória:** remover `motivo='foto_ausente'` somente do filtro legado, permitindo validar NULL de outros motivos. Os dez testes C3 passaram; registro persistido da execução: **[nao verificado]**. As fixtures de upload contêm somente `foto_ausente`, portanto não detectam a ampliação. (`app/api/foto_recebida.py:72`; `app/chamados/tests/test_cef_c3_sem_banco.py:140`; `app/chamados/tests/test_cef_c3_sem_banco.py:141`; `app/chamados/tests/test_cef_c1_sem_banco.py:208`)

## 15/11 05:37 · sessao ba44442ab92c · - · - · origem=Ronald (tmux)

1. **Os testes C3 mordem os quatro mutantes escolhidos**, exigindo falha de asserção identificada e recusando erro de execução. Há controle verde e falhas registradas para os operadores; isso prova esses cortes, não completude. (`app/chamados/tests/cef_c3_verificar.py:12`; `app/chamados/tests/cef_c3_verificar.py:31`; `app/chamados/tests/cef_c3_mutantes.txt:17`; `app/chamados/tests/cef_c3_mutantes.txt:33`)

2. **Mutante plausível vivo novo:** fazer a recepção antiga vencer quando a nova emissão trouxer `recebida_em` explícita. Substituí somente a fonte em memória de `ctx[k] = {**ctx[k], **v}` por:

   ```python
   ctx[k] = {
       **ctx[k], **v,
       **({'recebida_em': ctx[k]['recebida_em']}
          if 'recebida_em' in ctx[k] else {}),
   }
   ```

   O controle executado passou 10/10 e esse mutante também passou 10/10, com `PYTHONDONTWRITEBYTECODE=1`, sem arquivo de saída. O código correto permite valor novo vencer; o caso chamado “atualiza valor presente” só altera `timestamp`, deixando essa recepção explícita sem testemunha. A integração também apenas omite a recepção ou troca o episódio. (`app/chamados/services/disputa_emissao.py:573`; `app/chamados/tests/test_cef_c3_sem_banco.py:113`; `app/chamados/tests/test_cef_c3_integracao.py:18`)

3. **A2 tem uma lacuna mais grave que os mutantes cobertos:** cada teste sem banco cria uma única NULL e assume sua associação ao `ctx.id=101`. Na integração, a NULL é construída apagando a origem da própria batida que ficou por último no contexto. O caso negativo envia uma terceira batida cujo id diverge do contexto. Nenhum desses casos representa NULL antiga de A, contexto atualizado para B e upload de B; tampouco duas NULL de episódios distintos no mesmo agrupador. (`app/chamados/tests/test_cef_c3_sem_banco.py:140`; `app/chamados/tests/test_cef_c3_sem_banco.py:153`; `app/chamados/tests/test_cef_c3_integracao.py:32`; `app/chamados/tests/test_cef_c3_integracao.py:60`)

4. **Limites efetivos:** ORM, transação, `save`, validação e vitalidade são doubles no executor sem banco. `_validar_pelo_dono` é substituído por função que simplesmente marca as perguntas selecionadas como mortas. Portanto, o mutante antigo `_validar_pelo_dono -> return 0` permanece invisível nesse executor; a integração tem asserções de `validada_em` e vitalidade que o denunciariam, mas sua execução nesta revisão é **[nao verificado]**. (`app/chamados/tests/test_cef_c3_sem_banco.py:3`; `app/chamados/tests/test_cef_c1_sem_banco.py:219`; `app/chamados/tests/test_cef_c1_sem_banco.py:230`; `app/chamados/tests/test_cef_c3_integracao.py:53`)

5. **Classes de reprovação:**
   
   - **Derivação paralela / campo com dois significados: demonstrada em A2.** O leitor toma o último id do agregado como origem de todas as NULL selecionadas. O contrato da coluna documenta precisamente que, depois do nascimento, esse contexto pode apontar outra anomalia e que o acervo não recebe episódio por essa derivação; o acervo só tem chave de dia. (`app/api/foto_recebida.py:70`; `app/api/foto_recebida.py:72`; `app/chamados/models.py:3599`; `app/chamados/services/fio.py:1286`)
   - **Segundo juiz de vitalidade / segundo escritor de resposta ou validação:** nenhum novo demonstrado no delta; o resolver usa `fio_vivo` e a rotina delega às portas de resposta e validação existentes. O defeito está no universo entregue a essas portas. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/api/foto_recebida.py:82`)
   - **Relógio fora da borda:** há relógios internos preexistentes para `ultima_em` e resposta automática; C3 não acrescenta outro nesses trechos. (`app/chamados/services/disputa_emissao.py:567`; `app/api/foto_recebida.py:42`)
   - **Selo sem mordida:** a catraca AST verifica presença estrutural de envelope/id, sem provar valor em execução, domínio do ramo ou correspondência histórica entre pergunta e contexto. Sua mordida não cobre A2. (`app/chamados/tests/test_e1_v5_familia.py:133`; `app/chamados/tests/test_e1_v5_familia.py:170`; `app/chamados/tests/test_e1_v5_familia.py:180`)
   - **Timestamp com significados recepção antiga/fato novo:** convivência explicitamente preservada, anterior a C3. (`app/chamados/juizes.py:152`; `app/chamados/juizes.py:155`)

6. **Não executei `cef_c3_verificar`: ele escreve `cef_c3_mutantes.txt`**, apesar de não alterar fontes. O ensaio acima carregou e alterou apenas `c1.[nome]` em memória, mecanismo previsto no próprio executor. (`app/chamados/tests/cef_c3_verificar.py:36`; `app/chamados/tests/test_cef_c1_sem_banco.py:23`; `app/chamados/tests/test_cef_c1_sem_banco.py:26`)

## 15/11 05:37 · sessao b8538b8203fa · - · - · origem=Ronald (tmux)

A1 está correta para o defeito sequencial: somente dicionários com `id` conhecido igual fundem; chaves omitidas, inclusive `recebida_em`, sobrevivem; valor explícito novo vence e outro episódio substitui integralmente. (`app/chamados/services/disputa_emissao.py:569`; `app/chamados/services/disputa_emissao.py:573`; `app/chamados/services/disputa_emissao.py:575`)

O censo dos outros escritores dá estes limites:

- **Reuso via nota não apaga recepção já presente.** Escreve `ChamadoColaborador.contexto_json`, enquanto A1 escreve `DisputaSupervisao.contexto`; a nota delega à porta `fundir_contexto`, que trata dicionário como escalar e só o insere se a chave estiver ausente. Também **não acrescenta** recepção nova a `ultima_batida` já presente sem ela: permanece o snapshot antigo. Esse comportamento é preexistente e pode alimentar o fallback de dia pelo timestamp antigo. (`app/chamados/services/disputa_emissao.py:594`; `app/chamados/services/disputa_emissao.py:600`; `app/chamados/services/nota.py:110`; `app/chamados/services/nota.py:123`; `app/chamados/services/contexto.py:144`; `app/chamados/services/contexto.py:155`; `app/chamados/juizes.py:162`)
- **`contexto.py` não é outro escritor atual de `ultima_batida` da disputa.** Atua em `contexto_json` do chamado; `renovar_contexto` pode substituir qualquer chave recebida, mas os chamadores localizados enviam outras chaves. Exemplos: marcos, batida/data do turno, agrupamento, escape e sugestão. Acesso dinâmico fora do censo textual: **[nao verificado]**. (`app/chamados/services/contexto.py:97`; `app/chamados/services/contexto.py:100`; `app/chamados/services/contexto.py:102`; `app/chamados/management/commands/nomear_marco_por_lampada.py:109`; `app/chamados/management/commands/carimbar_por_batida_coincidente.py:98`; `app/chamados/services/adesao.py:113`; `app/chamados/services/declaracao_texto.py:208`; `app/chamados/services/limbo_folgas.py:68`)
- **Outras atribuições de `.contexto` preservam `ultima_batida` sequencialmente:** preenchimento de motivo/data parte de cópia; aparar remove a chave do motivo; remover acrescenta `revalidacoes`. A atribuição do envelope no gerador ocorre numa cópia transitória e não grava o agregado. (`app/chamados/services/disputa_emissao.py:291`; `app/chamados/services/disputa_emissao.py:296`; `app/chamados/services/disputa_emissao.py:312`; `app/chamados/models.py:2028`; `app/chamados/models.py:2032`; `app/chamados/models.py:2104`; `app/chamados/models.py:2109`; `app/chamados/models.py:2128`)
- **Portas genéricas da disputa substituem uma chave, mas os chamadores encontrados escrevem outras:** `supervisao_omissa_escalado`, `incidente_operacional`, `ultimo_alerta_diario`. Não demonstram outro caso sequencial do mesmo apagamento. (`app/chamados/models.py:2164`; `app/chamados/models.py:2182`; `app/chamados/views.py:1343`; `app/ponto/management/commands/escalonar_chamados_supervisao.py:159`; `app/ponto/management/commands/alertar_disputas_pendentes.py:48`)

**Não cabe prometer “nunca perde recepção” para concorrência:** o cron pode carregar a disputa antes da recepção; a emissão salva `recebida_em`; depois `renovar_contexto` grava o JSON inteiro da instância antiga, com outra chave, apagando a recepção recém-conhecida. A porta de renovação não relê nem trava nessa implementação. É risco preexistente, distinto do defeito sequencial curado; ocorrência na sombra **[nao verificado]**. (`app/ponto/management/commands/alertar_disputas_pendentes.py:33`; `app/ponto/management/commands/alertar_disputas_pendentes.py:42`; `app/ponto/management/commands/alertar_disputas_pendentes.py:48`; `app/chamados/models.py:2182`; `app/chamados/models.py:2187`; `app/chamados/services/disputa_emissao.py:539`; `app/chamados/services/disputa_emissao.py:579`)

Escritores dos envelopes: ambas APIs fornecem `id`, `tipo`, timestamp local do fato e recepção; retro fornece os três primeiros e omite recepção. O significado local de recepção não decide o dia no juiz fraco. (`app/api/views.py:761`; `app/api/views_core.py:755`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/chamados/juizes.py:151`; `app/chamados/juizes.py:169`)

## 15/11 05:34 · sessao 61f0f8265324 · CONTRATO-DE-ENTRADA-DO-FIO · construtor · origem=Code

Cura A1/A2 e entrega em [cef_c3_entrega.txt](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c3_entrega.txt:1).

- RED anterior à cura: [6 falhas](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c3_red.txt:87).
- Verificação sem banco: [37 testes passaram](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c3_verificacao.txt:45). [nao verificado]
- Quatro mutantes falharam por asserção, com controles verdes: [evidências](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c3_mutantes.txt:1).

Sonda e integração com banco não executadas; comandos e limites constam na [entrega](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c3_entrega.txt:234).

## 15/11 05:26 · sessao a9a6cd709e27 · JUSTIFICATIVA-PAGA-O-PERIODO · censo · origem=Code

**A v8 terminou REPROVADA**, com bloqueios de exportada e `None` explicitamente apontados; esses alertas não podem ser contabilizados como omissões do Codex (v8.r:63).

As referências aos anexos abaixo usam a numeração da transcrição fornecida, contando linhas em branco, a partir da primeira linha de cada anexo. A classificação limita-se à v8; o conteúdo das demais voltas é **[nao verificado]**. Aplico as classes de CLAUDE.md:382–393.

Desdobro o achado 6: a precedência da ausência inteira foi apontada, mas isso não comprova detecção das críticas específicas ao previsto antigo e ao contador de dias (v8.r:14; v8.r:21; app/ponto/services/dia_pago.py:730).

| achado | classe | trecho q | trecho r | trecho triagem |
|---|---|---|---|---|
| **1 — DIFF de frota, condições do aval e sonda vazia** | **(c) — não conta:** depende da medição de frota/prod/sombra. O dado fornecido relata a insuficiência; não permite refazer a medição (opus_final.txt:2; CLAUDE.md:386). | “pre-verificacao antes da suite INTEIRA e do opus final”; “Avais anexos.” Não há pedido literal para medir os 41 nessa pergunta (v8.q:1; v8.q:3). | “**FORA**, destino declarado: sessão mede 09/10.” Isso registra o destino da medição, sem certificar seu resultado (v8.r:7). | “PERGUNTA DE LEI (a)(b)(c) [...] pouso espera”. A triagem não apresenta DIFF nem números da frota (triagem_v8.txt:3). | [nao verificado]
| **2 — exportada sem guarda genérica** | **(b) — falha da triagem.** O Codex viu e marcou **bloqueia**. Motivo do descarte: remessa a pergunta de lei, sem cura por iniciativa, invocando L-117 e DINHEIRO item 3 (v8.r:7; triagem_v8.txt:3). | “Algum achado do v7 ficou sem cura e sem destino?” (v8.q:5–6). | “Continua existindo somente a guarda de 09/2026; exportação nas demais competências gera aviso e permite persistência”; gravidade: “**bloqueia**” (v8.r:7). | “itens 1 (exportada sem guarda generica) [...] -> PERGUNTA DE LEI (a)(b)(c) no topo do RELATO [...] nao curados por iniciativa (L-117)”; “nenhuma trava nem aviso de tela por exportada” (triagem_v8.txt:3). O opus contesta o encaminhamento: “o topo nao tinha essa pergunta” (opus_final.txt:3). |
| **3 — suíte inteira de árvore anterior; c13b posterior** | **(c) — não conta:** execução da suíte e certificação de alterações posteriores à v8. A pergunta identifica c12/c12b/c12c, enquanto o opus cobra c13b (v8.q:2; opus_final.txt:4). | “pre-verificacao antes da suite INTEIRA e do opus final” (v8.q:1). | “sua execução independente nesta volta e a suíte inteira são **[nao verificado]**” (v8.r:61). | “item 4 K1 escapes -> PROCEDE, curado na c13 (326a796aa).” Esse trecho registra a cura posterior, sem certificar a suíte da árvore final (triagem_v8.txt:2). |
| **5 — motor × ata em batida aberta; K2 ainda ‘espera !’** | **(a) — falha do Codex.** O tema estava presente; a resposta não identifica a divergência K2 nem o comentário de espera. A cópia contém expectativa divergente entre motor e grade e a frase de espera (v8.q:18; app/ponto/tests/test_justificativa_paga_o_periodo.py:653; app/ponto/tests/test_justificativa_paga_o_periodo.py:656). | “Regra fora do aval, derivacao paralela [...] selo sem mordida, campo com dois significados.” Também pergunta: “Algum esperado foi trocado para passar?” (v8.q:18; v8.q:17). | Trecho relacionado, **sem detectar K2**: “**NÃO PROCEDE fórmula ou juiz novo nos três hunks produtivos c12**”; “A fórmula continua na fonte de períodos” (v8.r:59). | Nenhum descarte específico de K2 registrado. “item 4 K1 escapes [...] curado na c13” refere-se a **K1**, não a K2 (triagem_v8.txt:2). |
| **6 — parte: override de `None` e precedência da ausência inteira** | **(b) — falha da triagem, nesta parte.** O Codex viu a divergência entre feed e grade. Motivo do descarte: “residuo pre-existente ou fixture” (v8.r:14; triagem_v8.txt:4). | “Quem ESCREVE motivo_justificativa='sem_ata'”; “Todos supoem o mesmo significado”; “campo com dois significados” (v8.q:7–9; v8.q:18). | “Precedência da ausência inteira diverge na grade”; “O feed remove o desconhecido, mas a grade financeira recebe os períodos crus” (v8.r:14). A entrega citada nessa resposta localiza a precedência em `leitor_celula.py:479` (app/ponto/tests/c12_entrega.txt:139). | “8 (grade crua x feed) [...] -> FILA, nao bloqueiam (motivo: residuo pre-existente ou fixture; opus decide se bloqueia)” (triagem_v8.txt:4). |
| **6 — demais partes: previsto da ata antiga e `dia_pago.py` ~730** | **(a) — falha do Codex.** O censo de escritores/leitores estava pedido, mas citar o builder não equivale a detectar o uso do previsto antigo; os alertas de conversão `None→0` também não identificam o contador em ~730 (v8.q:7–9; v8.r:21; v8.r:23; app/ponto/services/efeito_ausencia.py:159; app/ponto/services/dia_pago.py:730). | “Quem ESCREVE [...] e quem LE”; “2o escritor [...] campo com dois significados” (v8.q:7–9; v8.q:18). | Trecho mais próximo: “O censo de escritores da marca precisa incluir **também o builder**”; “Ambos recebem a indicação da mesma fonte de períodos”. Não aponta o problema do previsto antigo (v8.r:21). Para DiaPago, aponta outros locais: “lavratura e retrato de exportada fazem exatamente essa conversão” (v8.r:23). | “2 (lavratura None->0 em DiaPago), 6 (None com 2 significados) -> PERGUNTA DE LEI”. Esse descarte é correlato, sem demonstrar detecção dos subachados específicos do opus (triagem_v8.txt:3; opus_final.txt:6). |
| **7 — juiz com corte, sem registro em `core/juizes.py`** | **(a) — falha do Codex.** “Juiz novo” estava literalmente perguntado; a resposta restringiu sua conclusão aos três hunks produtivos c12 e não apontou a falta de registro relatada pelo opus (v8.q:18; v8.r:59; opus_final.txt:7). | “juiz novo” (v8.q:18). | “**NÃO PROCEDE fórmula ou juiz novo nos três hunks produtivos c12**” (v8.r:59). O corte literal existe na cópia (app/docs/CORTES.md:342). | Nenhum trecho específico sobre esse juiz; as destinações registradas tratam dos itens enumerados da resposta e do limite de c13 (triagem_v8.txt:3–5). |
| **8 — palavra `fallback` em docstring e commit A3** | **(e) — NÃO PERGUNTADO.** Não houve pedido literal de revisão lexical de docstrings ou da mensagem do commit A3; perguntar por derivação paralela não demonstra esse pedido específico (v8.q:1–19; opus_final.txt:8). | Trecho mais próximo: “Regra fora do aval, derivacao paralela” (v8.q:18). Não menciona `fallback`, `obra.diff:140` ou A3 (v8.q:1–19). | Sem apontamento sobre essa palavra ou A3. O trecho relacionado examina comportamento: “A fórmula continua na fonte de períodos” (v8.r:59). | Sem descarte desse achado registrado nas destinações da triagem (triagem_v8.txt:2–5). | [nao verificado]

## 15/11 05:21 · sessao 90735d134d0d · CONTRATO-DE-ENTRADA-DO-FIO · verificador · origem=Code

**REPROVA.** Há dois defeitos demonstráveis no código e falta comprovação do pré-requisito de sombra. (`app/chamados/services/disputa_emissao.py:570`; `app/api/foto_recebida.py:68`; `app/chamados/tests/cef_c2_entrega.txt:214`)

1. **BLOQUEIA — o retro apaga uma recepção já conhecida.**

   Cenário: APK envia batida sem foto, com `from_mock=true`. A API abre disputa por `gps_mock` e registra `ultima_batida.recebida_em`; depois do prazo, o retro emite `foto_ausente` para **a mesma batida**, enviando `ultima_batida` sem recepção. O merge substitui o dicionário inteiro. Resultado: `{id:101, timestamp:08:00, recebida_em:08:01}` → `{id:101, timestamp:08:00}`. (`app/api/views.py:447`; `app/api/views.py:457`; `app/api/views.py:765`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:73`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/chamados/services/disputa_emissao.py:557`; `app/chamados/services/disputa_emissao.py:570`)

   Portanto, omitir uma recepção desconhecida no envelope do retro acaba destruindo uma recepção conhecida no agregado. Isso contraria o “nada se perde” do aval fornecido; quantidade afetada em produção **[nao verificado]**.

2. **BLOQUEIA — o upload abandona perguntas sem `batida_origem`, inclusive após reemissão pelo retro novo.**

   Cenário: disputa aberta com `foto_ausente`, `ultima_batida.id=101` e pergunta viva com `batida_origem_id=None`. Chega a foto de 101: o filtro novo não encontra pergunta, não chama a validação e devolve zero. A foto é salva antes dessa tentativa de resolução; a pergunta permanece pendente. (`app/api/views.py:851`; `app/api/views.py:861`; `app/api/foto_recebida.py:68`; `app/api/foto_recebida.py:69`; `app/chamados/juizes.py:1479`)

   O envelope correto não elimina esse cenário: a porta inclui deliberadamente perguntas legadas sem episódio do mesmo dia no dedup; pode reutilizar essa pergunta e retornar sem preencher `batida_origem_id`. Assim, nova passagem do retro também pode terminar numa pergunta que o upload agora ignora. (`app/chamados/models.py:2793`; `app/chamados/models.py:2798`; `app/chamados/models.py:3004`; `app/chamados/models.py:3046`; `app/chamados/models.py:3091`)

   Esse acervo é uma possibilidade expressamente prevista pelo código; sua existência e quantidade em produção **[nao verificado]**. A mudança precisa tratar essa compatibilidade sem inferir silenciosamente um episódio pelo contexto acumulado, cuja insuficiência está documentada. (`app/chamados/services/fio.py:1286`; `app/chamados/models.py:3599`)

3. **BLOQUEIA a liberação — a medição obrigatória em sombra não está comprovada.**

   A entrega registra “quantas perguntas vivas mudam de dia, abertas × exportadas” como **[nao verificado]** e manda não presumir zero. A frase “140 vivas, zero exportadas” do commit anexado não vem acompanhada, no dado fornecido, de resultado rastreável da sonda. Não é possível certificar o cumprimento desse pré-requisito, nem afirmar `exportada > 0`. (`app/chamados/tests/cef_c2_entrega.txt:214`; `app/chamados/tests/cef_c2_entrega.txt:217`)

4. **Forward only: C2 não acrescenta um passe de reescrita, mas “nenhuma escrita no passado” não está garantido transitivamente.**

   Cenário admitido: batida noturna pertence a 13/11, mas sua pergunta com episódio está gravada em 14/11. O retro novo entrega o episódio; o gerador pergunta o dia ao juiz; a porta encontra a pergunta e chama `rechavear_no_reuso`. Com competências não exportadas e demais condições satisfeitas, essa porta pode mudar a pergunta antiga para 13/11. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:96`; `app/chamados/services/disputa_emissao.py:396`; `app/chamados/models.py:3023`; `app/chamados/models.py:3035`; `app/chamados/services/fio.py:1107`; `app/chamados/services/fio.py:1112`)

   **Não conto isso como mecanismo novo nem como bloqueio independente:** é comportamento preexistente alcançável pela emissão. Porém, ele impede certificar ausência de escrita histórica apenas olhando as linhas alteradas. Ocorrência concreta no dado de produção **[nao verificado]**. (`app/chamados/tests/cef_c1_entrega.txt:170`; `app/chamados/models.py:3010`)

5. **Censo dos contratos: episódio, instante, recepção e dia não são a mesma unidade.**

   | Campo ou valor | Escritores | Leitores e significado |
   |---|---|---|
   | `ultima_batida.id` | APIs e retro. (`app/api/views.py:762`; `app/api/views_core.py:756`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:99`) | Derivador confere posse; grade e gerador buscam a batida para perguntar o **dia do turno**. (`app/chamados/models.py:3606`; `app/chamados/models.py:3610`; `app/chamados/services/disputa_emissao.py:72`; `app/chamados/services/disputa_emissao.py:396`) |
   | `ultima_batida.tipo` | Tipo da batida nos três emissores. (`app/api/views.py:763`; `app/api/views_core.py:757`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:100`) | Nenhum leitor semântico específico localizado; leitura dinâmica **[nao verificado]**. |
   | `ultima_batida.timestamp` | APIs: `ts_efetivo` local; retro: `Batida.timestamp` local. (`app/api/views.py:764`; `app/api/views_core.py:758`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`) | Juiz fraco extrai **dia civil no offset da string**, não dia do turno. (`app/chamados/juizes.py:162`; `app/chamados/juizes.py:169`) |
   | `recebida_em` | Relógio da recepção das APIs; retro omite. (`app/api/views.py:765`; `app/api/views_core.py:759`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`) | Nenhum leitor semântico específico localizado; preservação falha no achado 1. (`app/chamados/services/disputa_emissao.py:570`) |
   | Contexto persistido e envelope da emissão | Nascimento grava chamado/disputa; recorrência substitui chaves do agregado; gerador recebe cópia com o envelope atual. (`app/chamados/services/disputa_emissao.py:589`; `app/chamados/services/disputa_emissao.py:599`; `app/chamados/services/disputa_emissao.py:570`; `app/chamados/services/disputa_emissao.py:312`) | Reuso do chamado funde contexto preservando chave já existente; template também exibe o contexto genericamente. (`app/chamados/services/nota.py:110`; `app/chamados/services/contexto.py:155`; `app/templates/chamados/partials/painel_chamado_turno.html:35`) |
   | `batida_origem` e `data_evento` | Origem deriva na porta; dia passa pelo juiz e pelo ponto de escrita do fio. (`app/chamados/models.py:2949`; `app/chamados/models.py:2958`; `app/chamados/models.py:3099`) | Dedup lê episódio ou acervo do dia; upload exige episódio exato. (`app/chamados/models.py:2795`; `app/api/foto_recebida.py:68`) |

   No upload, as duas cascas selecionam a batida do colaborador e chamam `anexar_foto`; essa porta escreve `Batida.foto` e `FotoBatida`. O resolver responde pela porta automática, valida pelo dono, consulta vitalidade e fecha pela porta existente. (`app/api/views.py:846`; `app/api/views_core.py:830`; `app/ponto/registro_batida.py:309`; `app/ponto/registro_batida.py:313`; `app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/api/foto_recebida.py:78`; `app/api/foto_recebida.py:80`)

6. **Ordenação e UTC/local: não encontrei comparação lexicográfica desse timestamp; há agregação indireta pelo dia.**

   O merge não escolhe o maior instante: substitui o bloco, podendo fazê-lo retroceder quando chega batida offline. O leitor direto apenas executa `fromisoformat(...).date()`; portanto, legado UTC conserva o dia UTC e envelope novo fornece o dia local. Essa convivência é explicitamente mantida pelo teste de legado. (`app/chamados/services/disputa_emissao.py:570`; `app/chamados/juizes.py:169`; `app/chamados/tests/test_cef_c2_sem_banco.py:104`)

   **Sim, existem consumidores cujo agrupamento pode mudar:** o canal agrupa respostas por colaborador/dia do juiz; a fila usa o mesmo dia e pode agrupar toques por semana. Uma mudança domingo→sábado permanece na mesma semana; domingo→segunda pode mudar essa chave semanal. Isso é consequência indireta do novo dia, não ordenação pelo ISO completo. (`app/chamados/canal.py:45`; `app/chamados/canal.py:49`; `app/chamados/services/fila_por_causa.py:298`; `app/chamados/services/fila_por_causa.py:319`; `app/chamados/services/fila_por_causa.py:324`)

   Completude para acesso dinâmico e impacto em produção **[nao verificado]**.

7. **Os testes mordem regras, mas deixam sobreviver transições relevantes.** [nao verificado]

   Há casos comportamentais de episódios fora de ordem, repetição, isolamento do upload, fato/recepção e fuso; não são apenas testes da forma textual. Entretanto, o núcleo de persistência é substituído por `Mock`, os uploads começam com origem preenchida e o teste do retro captura o envelope sem fundi-lo no agregado existente. Assim, os achados 1 e 2 ficam descobertos. (`app/chamados/tests/test_cef_c1_sem_banco.py:144`; `app/chamados/tests/test_cef_c1_sem_banco.py:177`; `app/chamados/tests/test_cef_c1_sem_banco.py:205`; `app/chamados/tests/test_cef_c2_sem_banco.py:74`; `app/chamados/tests/test_cef_c2_sem_banco.py:131`)

   Mutante plausível sobrevivente aos testes **sem banco**: `_validar_pelo_dono` virar `return 0`. O double usado pelo teste continua extinguindo perguntas independentemente dessa implementação. A integração tem asserções capazes de matá-lo; execução dessa integração nesta revisão **[nao verificado]**. (`app/api/foto_recebida.py:28`; `app/chamados/tests/test_cef_c1_sem_banco.py:216`; `app/chamados/tests/test_cef_c1_sem_banco.py:227`; `app/chamados/tests/test_cef_c1_episodios.py:101`)

   O selo AST também aceita `id` vindo de variável nula e atribuição de envelope em ramo não executado; alias por atribuição escapa do reconhecimento. **Não bloqueia isoladamente**, mas limita a promessa de catraca completa. (`app/chamados/tests/test_e1_v5_familia.py:133`; `app/chamados/tests/test_e1_v5_familia.py:170`; `app/chamados/tests/test_e1_v5_familia.py:180`)

8. **Arquitetura e sítios proibidos: nenhum novo desvio demonstrado no recorte.**

   O upload consulta os juízes existentes e delega resposta/validação às portas existentes; não identifiquei segundo juiz ou escritor direto novo nesses trechos. Há relógio interno preexistente no merge para `ultima_em`, que esta obra mantém. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/api/foto_recebida.py:78`; `app/chamados/models.py:1917`; `app/chamados/services/disputa_emissao.py:567`; `app/chamados/tests/cef_c1_entrega.txt:192`)

   Os inventários de C1/C2 não apontam alterações em motor, `ponto/portas/celula.py`, `core/juizes.py`, models/migrations, crons ou `bin/`. Conferência independente contra objetos Git do hash **[nao verificado]**; não atribuo a esta obra o merge de dedup indicado como externo. (`app/chamados/tests/cef_c1_verificar.py:16`; `app/chamados/tests/cef_c2_verificacao.txt:68`; `app/chamados/tests/cef_c2_verificacao.txt:79`)

## 15/11 05:19 · sessao 97571a15e9af · - · - · origem=Ronald (tmux)

1. **Regressão demonstrável: upload abandona perguntas legadas sem episódio. Bloqueia compatibilidade com esse acervo; ocorrência em produção [nao verificado].** Entrada: disputa aberta com `foto_ausente`, `contexto.ultima_batida.id=101` e pergunta viva com `batida_origem_id=None`; chega foto da batida 101. O filtro novo não encontra pergunta e retorna 0 sem responder/validar (`app/api/foto_recebida.py:62`, `:68`, `:69`, `:85`). Pergunta sem origem pode continuar viva: não existe descarte automático desse legado, e pergunta sem materialização retorna viva (`app/chamados/juizes.py:1402`, `:1479`). Reproduzi o corpo real por AST com doubles somente de ORM: retorno 0, validação não chamada [nao verificado por integração/banco].

   Isso alcança inclusive **nova execução do retro**: a porta deliberadamente reutiliza pergunta antiga sem origem do mesmo dia (`app/chamados/models.py:2793`, `:2798`, `:2800`); escolhe a canônica (`:2990`, `:3004`) e o ramo de reuso atualiza célula/destino/lavra, sem preencher a origem (`:3046`, `:3080`, `:3091`). Portanto, emitir agora envelope correto não garante pergunta com episódio que o upload exige (`app/ponto/management/commands/marcar_foto_ausente_retro.py:96`, `app/api/foto_recebida.py:68`). O legado sem episódio é previsto expressamente pelo código (`app/chamados/services/fio.py:1286`).

   Os casos de upload usam exclusivamente perguntas com origem conhecida (`app/chamados/tests/test_cef_c1_sem_banco.py:205`); a integração começa criando novas batidas/perguntas (`app/chamados/tests/test_cef_c1_episodios.py:36`, `:51`). Não cobre essa regressão.

2. **Forward only: caminho transitivo que pode reescrever passado existe, mas não é defeito novo inequívoco desta obra.** O retro novo pode reapresentar batida cujo episódio já possui pergunta gravada em dia errado (`app/ponto/management/commands/marcar_foto_ausente_retro.py:96`). O gerador resolve o dia correto (`app/chamados/services/disputa_emissao.py:396`), e o nascimento reutiliza a pergunta e chama `rechavear_no_reuso` quando o dia gravado difere (`app/chamados/models.py:3023`, `:3035`). A porta veta exportada/desconhecida, mas escreve a data antiga quando ambos os dias pertencem a competência não exportada (`app/chamados/services/fio.py:1107`, `:1112`). Esse mecanismo já existia e depende de estado anterior; incidência no dado fornecido [nao verificado]. **Não afirmar que legado sem origem em outro dia será necessariamente encontrado:** a busca desse legado é pela data gravada igual ao dia pedido (`app/chamados/models.py:2798`).

3. **Inventário c1, portas e fontes.** O retro escreve `ultima_batida.{id,tipo,timestamp}` da batida examinada e entrega ao escritor existente (`app/ponto/management/commands/marcar_foto_ausente_retro.py:96`). O agregado persiste contexto no nascimento da disputa/chamado e sobrescreve a chave de topo na recorrência (`app/chamados/services/disputa_emissao.py:570`, `:589`, `:599`); o gerador usa cópia transitória com o envelope atual (`:312`). Identidade do episódio sai do derivador existente, que verifica posse da batida (`app/chamados/models.py:3603`, `:3606`, `:3610`); dia passa pelo juiz existente do turno (`app/chamados/services/disputa_emissao.py:396`, `app/chamados/models.py:2958`). Upload lê a coluna de episódio, responde pela porta automática e valida pelo dono (`app/api/foto_recebida.py:48`, `:51`, `:68`); vitalidade/fecho consultam autoridades existentes (`:78`, `:80`, `app/chamados/models.py:1911`). Não identifiquei juiz novo ou escritor de validação fora da porta nesses trechos (`app/api/foto_recebida.py:51`, `app/chamados/models.py:1917`).

4. **Detalhe do relógio/merge preexistente:** a recorrência lê relógio dentro do núcleo para `contexto[motivo].ultima_em`, independentemente do instante recebido da API (`app/chamados/services/disputa_emissao.py:567`); o retro lê relógio na borda do comando (`app/ponto/management/commands/marcar_foto_ausente_retro.py:63`). A obra não introduz essa leitura interna (`app/chamados/tests/cef_c1_entrega.txt:196`).

## 15/11 05:19 · sessao 90f92ea505b7 · - · - · origem=Ronald (tmux)

1. Os testes sem banco mordem comportamentos: dois episódios fora de ordem, repetição, envelope vazio e isolamento do upload; C2 verifica fato, recepção, offset e prioridade da escada. Executei os 16 casos: OK. Evidência dos casos: `app/chamados/tests/test_cef_c1_sem_banco.py:177`, `:192`, `:230`, `:250`; `app/chamados/tests/test_cef_c2_sem_banco.py:74`, `:86`, `:95`, `:112`. Resultado desta execução: [nao verificado por artefato persistido; saída da ferramenta].

2. Mutante concreto sobrevivente aos 16: substituir `_validar_pelo_dono` por `return 0`. Entrada upload de B1 → nenhuma resposta nem validação real; o resolver chama o helper e ignora seu retorno (`app/api/foto_recebida.py:71`). Sobrevive porque o teste injeta outro helper que sempre extingue as perguntas (`app/chamados/tests/test_cef_c1_sem_banco.py:216`, `:227`). Demonstrei em memória: 16 OK. A integração existente deve matá-lo: exige `validada_em` e pergunta morta (`app/chamados/tests/test_cef_c1_episodios.py:98`–`:103`); execução dessa integração [nao verificado]. **Não é bug atual de produção, nem razão isolada para bloquear: delimita a prova sem banco.**

3. O selo T5 tem falsos negativos demonstrados: aceita ID nulo vindo de variável; aceita atribuição de envelope dentro de ramo não executado; ignora alias por atribuição (`porta = criar_ou_atualizar_disputa`). A análise considera qualquer expressão não constante suficiente para `id` (`app/chamados/tests/test_e1_v5_familia.py:180`), ignora fluxo ao selecionar atribuições (`:170`–`:179`) e reconhece apenas nome direto/import aliases (`:133`, `:149`). Nos três exemplos, retornou `[]` em memória. **Fragilidade da catraca; não prova defeito no emissor atual.**

4. Medida obrigatória anterior ao pouso permanece sem prova no material: a própria entrega registra sombra, abertas/exportadas e lista como `[nao verificado]`, proibindo presumir zero (`app/chamados/tests/cef_c2_entrega.txt:214`–`:217`). A alegação “140 / 0 exportadas” do commit fornecido não vem acompanhada de sonda/lista no dado [nao verificado]. **Bloqueia comprovar liberação pelo aval; não permite afirmar exportadas > 0.**

5. Não executar `cef_c1_verificar.py` nesta auditoria: ele escreve `cef_c1_mutantes.txt` (`app/chamados/tests/cef_c1_verificar.py:40`). Os testes unitários foram executados com `PYTHONDONTWRITEBYTECODE=1`; nenhum arquivo foi escrito pelos comandos realizados [nao verificado por censo posterior].

## 15/11 05:19 · sessao 8a596e22b27c · - · - · origem=Ronald (tmux)

1. **Achado bloqueante: o retro apaga uma recepção já conhecida.** APK sem foto, com `from_mock=true`, abre disputa por `gps_mock`; a foto do APK aguarda upload separado (`app/api/views.py:447`, `app/api/views.py:457`). A API grava `ultima_batida.recebida_em` (`app/api/views.py:765`). Depois do prazo, o retro envia a mesma batida, sem `recebida_em` (`app/ponto/management/commands/marcar_foto_ausente_retro.py:73`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`). O núcleo substitui o bloco inteiro e salva, apagando a recepção anteriormente registrada (`app/chamados/services/disputa_emissao.py:557`, `app/chamados/services/disputa_emissao.py:570`, `app/chamados/services/disputa_emissao.py:574`). Contraria “a recepção fica em chave própria, nada se perde”, do aval fornecido [nao verificado: anexo sem arquivo local identificado].

   Reprodução realizada somente em memória, compilando o AST real de `_abrir_ou_agregar_disputa`, com doubles de ORM e transação: `{id:101,tipo:E,timestamp:08:00,recebida_em:08:01}` → retro da mesma batida → um `save`, bloco final sem `recebida_em` (`app/chamados/services/disputa_emissao.py:483`, `app/chamados/services/disputa_emissao.py:570`).

2. **Os testes não cobrem essa perda.** O teste sem banco substitui o núcleo de persistência por `Mock`; seu teste de contexto acumulado observa esse double, sem executar a fusão real (`app/chamados/tests/test_cef_c1_sem_banco.py:144`, `app/chamados/tests/test_cef_c1_sem_banco.py:180`). O teste C2 captura somente o envelope emitido pelo retro e exige a ausência de recepção nele; não o funde numa disputa que já possui recepção (`app/chamados/tests/test_cef_c2_sem_banco.py:131`, `app/chamados/tests/test_cef_c2_sem_banco.py:150`).

3. **Inventário direto dos campos.**
   - Escritores do envelope: API DRF (`app/api/views.py:761`), API core (`app/api/views_core.py:755`), retro (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`). APIs escrevem fato local e recepção local; retro escreve fato local, sem recepção (`app/api/views.py:764`, `app/api/views_core.py:758`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`).
   - Persistência: contexto de nascimento do chamado e da disputa (`app/chamados/services/disputa_emissao.py:589`, `app/chamados/services/disputa_emissao.py:599`); atualização do agregado (`app/chamados/services/disputa_emissao.py:557`, `app/chamados/services/disputa_emissao.py:570`).
   - `id`: derivação da origem, com conferência de posse (`app/chamados/models.py:3606`, `app/chamados/models.py:3610`); referência de grade e dia do turno (`app/chamados/services/disputa_emissao.py:72`, `app/chamados/services/disputa_emissao.py:76`); data da pergunta pelo juiz do turno (`app/chamados/services/disputa_emissao.py:396`, `app/chamados/services/disputa_emissao.py:400`).
   - `timestamp`: leitor direto localizado é `_dia_do_contexto_fraco`, que faz `datetime.fromisoformat(...).date()` no offset presente (`app/chamados/juizes.py:162`, `app/chamados/juizes.py:169`).
   - `tipo` e `recebida_em`: nenhum leitor semântico específico de produção localizado; cobertura de leituras dinâmicas [nao verificado]. O template genérico imprime o contexto integral do chamado (`app/templates/chamados/partials/painel_chamado_turno.html:35`).

4. **Não encontrei ordenação, dedup ou comparação lexicográfica pelo timestamp desse envelope.** A fusão substitui sem comparar horários (`app/chamados/services/disputa_emissao.py:570`); o leitor de timestamp apenas extrai a data (`app/chamados/juizes.py:169`). A ordenação explícita do painel usa urgência/criação (`app/chamados/services/painel.py:115`, `app/chamados/services/painel.py:492`). Há agregações indiretas por dia que podem mudar quando o juiz alcança o carimbo fraco: respostas por colaborador/dia (`app/chamados/canal.py:45`, `app/chamados/canal.py:49`), contagem de toques (`app/chamados/services/fila_por_causa.py:319`, `app/chamados/services/fila_por_causa.py:324`) e seleção de evidências (`app/chamados/services/acoes_chamado.py:380`, `app/chamados/services/acoes_chamado.py:388`). Ocorrência real em produção [nao verificado].

5. **UTC legado e local novo convivem sem comparação entre strings no leitor direto localizado.** O legado conserva sua data UTC; o novo fornece a data local, pois o leitor não normaliza o offset (`app/chamados/juizes.py:169`). O comportamento é explicitamente testado (`app/chamados/tests/test_cef_c2_sem_banco.py:104`). A regressão cronológica do agregado por chegada offline decorre da substituição incondicional; sozinha, não prova backfill nem consumidor incorreto (`app/chamados/services/disputa_emissao.py:570`).

## 15/11 05:16 · sessao f9325d08478f · CONTRATO-DE-ENTRADA-DO-FIO · construtor · origem=Code

Entrega em [cef_c2_entrega.txt](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c2_entrega.txt:1): cura nas duas APIs, RED/GREEN, censo, comandos, 27 testes verdes, Ruff e nove mutantes mortos. [nao verificado]

Medição em sombra, suite e opus final: **[nao verificado]**, registrados nas [pendências](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c2_entrega.txt:210). [nao verificado]

## 15/11 04:21 · sessao 3453bc3dc40d · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Entrega em [c13_entrega.txt](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c13_entrega.txt:1), com RED/GREEN, comandos, censo e mutantes.

[26 testes OK](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c13_green.txt:30); [13/13 mutantes mortos](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c13_mutantes.txt:341). [nao verificado]

Pendências explícitas: [copy/replace sem origem local ainda escapa](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c13_entrega.txt:15) e [Ruff indisponível](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c13_checks.txt:6). Certificação suite/Opus: [nao verificado]. [nao verificado]

## 15/11 04:05 · sessao 0c663ade5d11 · - · - · origem=Ronald (tmux)

Censo somente por leitura; nenhuma escrita.

- `chamados/models.py:1541`: cópia de pergunta do queryset de `:1530`; `:1544`: cópia de `_state`. Falsos positivos para ResultadoMes.
- `chamados/services/carga_pergunta_viva.py:177`: cópia de batidas, com filtro sobre `timestamp` em `:178`; classe herda `CargaDaJanela` em `:83`. Falso positivo.
- `escala/utils.py:1929`: cópia de célula do mapa produzido por `celulas_do_periodo` em `:1351`, iterado em `:1924`. Falso positivo.
- `ponto/portas/celula.py:1001`: cópia de célula para sobrepor ata/veredito em `:1002-1005`. Falso positivo.
- `ponto/turnos.py:1543`, `:1999`, `:3121`: cópias de batidas; origem da carga em `:3101-3105`. Falsos positivos.
- `ponto/motor_calculo_v2.py:2268`: cópias das batidas de entrada, usadas em `:2273`. Falso positivo.
- `core/management/commands/describe.py:71`: membership em `alvo.__dict__` para listar métodos, sem transportar valores. Falso positivo.

Os demais `__dict__` encontrados fazem seleção de chave (`get`, `pop`, `setdefault`, subscrito), como `chamados/juizes.py:1112` e `escala/alimentacao.py:43`. Separar seleção de chave de abertura inteira é um estreitamento estrutural útil.

Não há prova de tipo em `def f(resultado): return {**vars(resultado)}`. Capturá-lo exige tratar parâmetro desconhecido conservadoramente; distinguir esse parâmetro de `cel` em `ponto/portas/celula.py:1001` exige proveniência por chamadas/tipos. Exclusão apenas por nomes ou por acesso a atributos estranhos é insegura: ResultadoMes admite atributos dinâmicos, e o motor o constrói em `ponto/motor_calculo_v2.py:2274`.

Tipos efetivos em execução e ocorrência em produção: [nao verificado].

## 15/11 03:58 · sessao 90dd9a6ed7b7 · JUSTIFICATIVA-PAGA-O-PERIODO · verificador · origem=Code

**Há bloqueio: K1 ainda deixa passar leitores novos, inclusive mutantes que sobrevivem aos 12 testes da própria catraca.** O rastreador não acompanha cadeias de aliases e descarta módulos sem os nomes literais dos campos (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:101`, `:147`).

1. **PROCEDE parcialmente — bloqueia/fila: os achados do v7 estão mencionados, mas nem todos estão curados ou têm destino verificável.** A seção 6 lista os resíduos; para alguns, apenas diz FORA (`app/ponto/tests/c12_entrega.txt:138`).

| Achado do v7 | Conferência nesta cópia | Gravidade restante |
|---|---|---|
| 1. Exportada sem proteção genérica | **FORA**, destino declarado: sessão mede 09/10. Continua existindo somente a guarda de 09/2026; exportação nas demais competências gera aviso e permite persistência (`app/ponto/tests/c12_entrega.txt:141`; `app/ponto/services/fechamento.py:36`, `:539`, `:1157`, `:1194`). | **bloqueia** |
| 2. Lavratura converte `None` em zero | **FORA**, sem destino inequívoco. A seção 6 remete a “pergunta de lei (c)”, mas a pergunta local 5(c) trata de K1. A conversão continua (`app/ponto/tests/c12_entrega.txt:138`, `:132`; `app/ponto/services/dia_pago.py:292`, `:490`). | **bloqueia** |
| 3. Template não lê o rótulo | **Fechado pelo hunk** de `espelho.html`, `@@ -92,6 +92,16 @@`: agora consulta motivo e dias desconhecidos (`app/templates/ponto/espelho.html:100`). | — |
| 4. Rótulo usa janela visual inteira | **Fechado nesse ponto pelo hunk** de `espelho.py`, `@@ -1260,8 +1260,11 @@`: conjunto recortado por competência. Permanecem as diferenças descritas no item 3 abaixo (`app/ponto/services/espelho.py:1266`). | **fila**, para os resíduos |
| 5. Escape literal de K1 | **Escape original fechado** pelo detector de módulo dinâmico e pelos casos acrescentados; o contrato completo continua furado, conforme item 4 (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:93`, `:289`). | **bloqueia** |
| 6. Mutante `any → all` | **Fechado pelo hunk novo de testes c12**: fixture exige mapa misto `{D: None, D2: 180}` e conservação dos demais campos. O mutante morre (`app/ponto/tests/test_justificativa_c12_sem_banco.py:70`, `:77`; `app/ponto/tests/c12_mutantes.txt:4`). | — |
| 7. Mutante `discard → clear` | **Fechado pelo hunk novo de testes c12**: ausência inteira em D conserva VIZ desconhecido. O mutante morre (`app/ponto/tests/test_justificativa_c12_sem_banco.py:92`; `app/ponto/tests/c12_mutantes.txt:8`). | — |
| 8. Precedência da ausência inteira diverge na grade | **FORA**, sem encaminhamento concreto na seção 6. O feed remove o desconhecido, mas a grade financeira recebe os períodos crus (`app/ponto/tests/c12_entrega.txt:139`; `app/ponto/services/efeito_ausencia.py:75`; `app/folha/export.py:339`). | **fila** |
| 9. Cartão sobrescreve parcialmente o rótulo | **FORA**, sem encaminhamento concreto. Horas podem ser numéricas com dias desconhecidos; motivo posterior `None` não limpa o anterior (`app/ponto/tests/c12_entrega.txt:139`; `app/relatorios/cartao_pela_celula.py:187`, `:208`, `:214`). | **fila** |
| 10. Causa falsa de zero | **Fechado para o caso marcado `sem_ata`**, pelo hunk de `causas.py`, `@@ -83,6 +83,15 @@`, e teste com classificador real (`app/folha/causas.py:91`; `app/ponto/tests/test_justificativa_c12_sem_banco.py:145`). | — |
| 11. Relógio fora da passada | **FORA**, sem encaminhamento concreto. A retenção continua chamada sem `hoje` e alcança `localdate()` (`app/ponto/tests/c12_entrega.txt:140`; `app/ponto/services/fechamento.py:1080`; `app/folha/export.py:1248`). | **fila** |

2. **NÃO PROCEDE a persistência de `realizado_zero` nos leitores da causa corrigida; PROCEDE a quebra mais ampla de `None` — bloqueia.** Para previsto positivo, soma conhecida zero e algum dia marcado `sem_ata`, o classificador retorna `indeterminado`. O fechamento grava essa resposta; retrato e lista leem o campo gravado. O retrato traduz `indeterminado` para “sem causa”, e o template da lista mostra “a apurar” (`app/folha/causas.py:92`; `app/ponto/services/fechamento.py:1093`; `app/folha/retrato_folha.py:249`, `:1115`; `app/ponto/services/fechamento_lista.py:122`; `app/templates/ponto/_fech_linha.html:18`).

   O censo de escritores da marca precisa incluir **também o builder**: `escala/utils.py` transporta `motivo='sem_ata'` para `motivo_justificativa`, além do overlay do leitor de célula. Ambos recebem a indicação da mesma fonte de períodos; isso, por si, não demonstra juiz novo (`app/escala/utils.py:1259`, `:1281`; `app/escala/services/leitor_celula.py:479`; `app/ponto/services/efeito_ausencia.py:165`).

   O contrato global ainda não é “`None` nunca vira zero”: lavratura e retrato de exportada fazem exatamente essa conversão (`app/ponto/services/dia_pago.py:292`, `:490`, `:495`). Há ainda consumidor que acusa zero: com `minutos_realizados=None`, previsto 480 e `n_ok=2`, `supra_juiz.classificar_dia` produz `REALIZADO_ZERO_COM_TURNO`, pois começa por `None or 0`. É contraprova do consumidor, distinta de `causa_espelho`; ocorrência desse formato em produção **[nao verificado]** (`app/ponto/supra_juiz.py:237`, `:294`). **Fila** para esse consumidor.

3. **NÃO PROCEDE a existência de autoridade nova para o rótulo; PROCEDE a identidade incompleta das janelas — fila.** Tela e PDF chamam `rotular_abono_sem_apuracao`; template e badge exigem dias `None` e motivo `sem_ata` (`app/ponto/services/espelho.py:1266`; `app/relatorios/pdf_espelho.py:483`, `:790`; `app/templates/ponto/espelho.html:100`).

   O recorte nominal por competência foi corrigido, mas os universos efetivos diferem: a tela pede autoridade até **hoje**, enquanto o PDF aplica teto em **ontem**. Um dia de hoje justificado sem ata pode entrar no conjunto da tela e ficar fora do PDF (`app/ponto/services/espelho.py:1236`, `:1267`; `app/relatorios/pdf_espelho.py:287`, `:333`).

   Além disso, somente o **rótulo** foi recortado: `dias_abono` continua copiado do resultado da janela visual. Quando o rótulo de competência anterior deixa de esconder esse número, ele pode aparecer no resumo atual; o mapeamento do fechamento não fornece `dias_abono` para substituí-lo. A própria entrega registra essa pendência (`app/ponto/services/espelho.py:1261`; `app/relatorios/cartao_pela_celula.py:93`; `app/ponto/tests/c12_entrega.txt:122`).

   **NÃO PROCEDE parâmetro novo com default neste delta:** a assinatura conserva os parâmetros existentes, com `agora` obrigatório. O hunk altera somente o argumento do helper (`app/ponto/services/espelho.py:1203`, `:1266`). Os 12 chamadores estão enumerados na entrega (`app/ponto/tests/c12_entrega.txt:112`).

4. **PROCEDE — bloqueia: K1 ainda tem escapes estáticos.** O exemplo literal do v7 agora é capturado, inclusive quando injetado na tela (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:289`, `:296`).

   Entretanto, esta variante passa: o detector reconhece `g = getattr`, mas não reconhece `h = g`; a chave do dicionário volta a ser descartada (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:101`, `:125`).

   ```python
   g = getattr
   h = g
   def _escape(resultado):
       nomes = {'dias_abono': 1}
       return [h(resultado, k) for k in nomes]
   ```

   Em reprodução somente em memória, essa fonte anexada a `folha/causas.py` devolveu `[1]`, manteve a allowlist sem diferenças e sobreviveu aos **12 testes K1**. A atribuição `g, h = getattr, getattr` também escapa pelo tratamento limitado dos destinos (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:101`, `:104`, `:179`).

   Outro escape é `return {**vars(resultado)}`, equivalente a `{**resultado.__dict__}`: transporta os dois campos sem nomeá-los. O filtro textual descarta o módulo antes de analisar esses acessos; a variante `vars` também sobreviveu aos 12 testes. `ResultadoMes` é dataclass com ambos os campos (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:147`; `app/ponto/motor_calculo_v2.py:268`, `:284`, `:292`).

   `vars(resultado)['dias_abono']` e `resultado.__dict__['dias_abono']` são detectados. Subscript direto é ignorado em módulo não dinâmico, mas seu funcionamento sobre o resultado real **[nao verificado]** (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:107`, `:127`).

5. **PROCEDE parcialmente — fila: nem toda ata nasce pela porta real.** As duas fixtures usam `montar_dna`, mas `_justificar` ainda calcula os números e grava `ata` por `QuerySet.update`. Ela própria declara que não prova a escrita do cartório (`app/ponto/tests/test_justificativa_paga_o_periodo.py:204`, `:210`, `:219`, `:224`). A aprovação posterior aciona o signal do cartório; isso não elimina a inicialização manual anterior (`app/ponto/tests/test_justificativa_paga_o_periodo.py:225`; `app/ponto/signals.py:165`, `:93`).

   A fixture do PDF chama explicitamente `julgar_celula`, recarrega e exige W=0/P>0 (`app/relatorios/test_espelho_falta.py:48`, `:49`, `:51`).

   **NÃO PROCEDE troca dos esperados positivos no delta fornecido:** continuam duas pausas de 85 minutos e os dois esperados **550**; continua **1 dia de abono** no PDF. O caso irmão sem ata exige `None/None/sem_ata` (`app/ponto/tests/test_justificativa_paga_o_periodo.py:282`, `:296`, `:297`; `app/relatorios/test_espelho_falta.py:97`, `:71`). Houve enfraquecimento declarado de outro teste, P3, de igualdade com 1 para `IsNotNone`; ele verifica ausência do rótulo, não a correção do total (`app/ponto/tests/c12_entrega.txt:67`; `app/ponto/tests/test_justificativa_c12_sem_banco.py:54`).

6. **PROCEDE — bloqueia/fila: permanecem selo sem mordida e campos com significados divergentes.** K1 não garante a exclusividade dos leitores; `None` continua significando desconhecido no fechamento e zero no `DiaPago`; o resumo pode conservar dias desconhecidos com horas numéricas ou motivo residual (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:101`, `:147`; `app/ponto/services/fechamento.py:1073`; `app/ponto/services/dia_pago.py:292`; `app/relatorios/cartao_pela_celula.py:187`, `:214`).

   **NÃO PROCEDE fórmula ou juiz novo nos três hunks produtivos c12:** eles acrescentam diagnóstico, recorte do conjunto e leitura do resumo. A fórmula continua na fonte de períodos (`app/folha/causas.py:91`; `app/ponto/services/espelho.py:1266`; `app/templates/ponto/espelho.html:100`; `app/ponto/services/efeito_ausencia.py:170`). O relógio fora da passada permanece como resíduo declarado (`app/ponto/services/fechamento.py:1080`; `app/folha/export.py:1248`).

O verde de 6477 testes do recorte foi informado no dado; sua execução independente nesta volta e a suíte inteira são **[nao verificado]**. [nao verificado]

**REPROVADO para liberar a suíte inteira nesta pré-verificação**, pelo escape sobrevivente de K1 e pelos bloqueios de exportada/`None` ainda abertos (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:101`, `:147`; `app/ponto/services/fechamento.py:36`; `app/ponto/services/dia_pago.py:292`).

## 15/11 03:56 · sessao 3e3b5a8340d3 · - · - · origem=Ronald (tmux)

Item 4 — **PROCEDE, bloqueia**: o escape literal do v7 está coberto pelos sete casos e pela injeção na tela, mas K1 ainda admite novos leitores estáticos. (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:217`, `:289`, `:296`)

Reprodução em memória, anexando cada fonte a `folha/causas.py` na árvore viva:

```python
g = getattr
h = g
def _escape(resultado):
    nomes = {'dias_abono': 1}
    return [h(resultado, k) for k in nomes]
```

Também passa com `g,h=getattr,getattr`. O rastreador reconhece somente atribuição cujo valor pertence diretamente a `DINAMICOS` e cujo destino é `ast.Name`; não acompanha cadeia nem desempacotamento. Ambos devolvem `[1]`, mantêm `fora_da_allowlist={}` e sobrevivem aos **12 testes completos da K1**. (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:101`, `:103`, `:105`, `:125`)

Outro escape:

```python
def _escape(resultado):
    return {**vars(resultado)}
```

O equivalente `{**resultado.__dict__}` também transporta os dois abonos. Sem literal `dias_abono`/`horas_abono` no módulo, o filtro nem analisa seu AST; a versão `vars` sobrevive aos **12 testes completos da K1** quando injetada em `folha/causas.py`. A classe real permite isso: `ResultadoMes` é dataclass e declara ambos os campos. (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:147`, `:148`; `app/ponto/motor_calculo_v2.py:268`, `:284`, `:292`)

`vars(resultado)['dias_abono']` e `resultado.__dict__['dias_abono']` são detectados; o subscript direto `resultado['dias_abono']` é ignorado, mas seu funcionamento no objeto real não foi demonstrado. (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:107`, `:118`, `:127`; funcionamento do subscript direto: [nao verificado])

As execuções usaram `PYTHONDONTWRITEBYTECODE=1`/`-B`, fontes e mutações somente em memória. [nao verificado]

## 15/11 03:54 · sessao 3ea9d6f3a750 · - · - · origem=Ronald (tmux)

**Item 5: PROCEDE parcialmente; gravidade fila.** A primeira fixture ainda fabrica a ata por `ata.update` e `CelulaDia.objects.filter(...).update(ata=ata)`, embora use `montar_dna` real. Ela própria declara que não prova a escrita do cartório (`app/ponto/tests/test_justificativa_paga_o_periodo.py:204-224`). O selo exclui diretórios `tests`, portanto não certifica esse caminho (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:171-174`).

Há uma nuance: depois da escrita manual, a fixture cria a justificativa aprovada (`app/ponto/tests/test_justificativa_paga_o_periodo.py:225-226`); seu `post_save` chama o cartório (`app/ponto/signals.py:161-165`, `:85-93`). A execução efetiva dessa relavratura nesta verificação é **[nao verificado]**.

**A segunda fixture usa a porta real:** cria a célula com `montar_dna`, chama `julgar_celula`, recarrega e exige realizado zero e previsto positivo (`app/relatorios/test_espelho_falta.py:43-52`). O cartório monta a ata e passa-a à lavra (`app/ponto/services/cartorio.py:1850-1874`); sem realizado, grava `int(... or 0)` (`:738-740`).

**Não houve troca dos esperados positivos no delta fornecido:** continuam duas pausas de 85 minutos e duas asserções de **550**, na fonte e na grade (`app/ponto/tests/test_justificativa_paga_o_periodo.py:282-297`); o PDF conserva **um dia de abono**, uma falta e sua data (`app/relatorios/test_espelho_falta.py:94-97`). O caso irmão acrescentado exige `None/None/'sem_ata'` (`:60-73`). O produtor real inclui o segundo par de pausas no DNA (`app/ponto/management/commands/gerar_celulas.py:90-103`).

A prova de execução ORM nesta verificação é **[nao verificado]**.

_5520 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
