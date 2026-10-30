# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 08:40** · publicado em **30/10/2026 08:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 08:40 · sessao e3bb5ee2fac5 · - · - · origem=Ronald (tmux)

A triagem “sítio não tocado” não procede: o registro da dívida permanece em `core/juizes.py`, mas o mecanismo executável está em `fora_de_fase`, justamente a função alterada para expor `ec_id` e datas. O escopo exige examinar todo sítio tocado, não apenas as linhas acrescentadas. `CLAUDE.md:600`; `app/core/juizes.py:905`; `app/escala/services/fase_12x36.py:192`, `:207`.

A dívida é anterior, portanto não deve ser descrita como “segundo juiz criado neste diff”; contudo, segue dentro de função tocada e passa a alimentar a C10: batidas cruas entram no pareador, enquanto o registro aponta `turnos_do_colab` como autoridade. `app/escala/services/fase_12x36.py:128`, `:146`, `:175`; `app/core/juizes.py:667`, `:905`; `app/escala/services/cadastro_realidade.py:335`.

O aval de reutilizar o leitor é compatível com corrigir esse leitor existente pela autoridade da casa; não autoriza preservar a dívida [não verificado: interpretação do aval]. Uma discordância concreta entre pareamento cru e ata ainda precisa ser demonstrada para fundamentar o impacto funcional; o registro comprova a dívida declarada, não uma reprodução executada. `app/core/juizes.py:905`; [não verificado: reprodução dinâmica].

## 30/10 08:40 · sessao a11bf3d62b06 · - · - · origem=Ronald (tmux)

Achei uma falha nova de rótulo: **C10 chama qualquer achado de “12x36”, mas o leitor inclui também 24x48**. O universo admite os dois ciclos (`app/escala/services/fase_12x36.py:29`, `:106`); uma 24x48 deslocada um dia pode atingir os dois pisos, pois sua regra é `% 3` (`app/escala/models.py:1264`) e o detector exige três dias de cada lado (`app/escala/services/fase_12x36.py:187`). `_fase_deslocada` transporta todos os achados (`app/escala/services/cadastro_realidade.py:337`), mas a assinatura os descreve obrigatoriamente como “12x36” (`:112`). A correção pode ser apenas de apresentação, sem outro juiz.

As triagens conferidas:

- **Identidade do vínculo tratado:** leitor expõe `ec_id` (`app/escala/services/fase_12x36.py:192`); adaptador guarda `ec` e inclui esse identificador no detalhe (`app/escala/services/cadastro_realidade.py:340`, `:344`); tela e PDF apresentam o detalhe (`app/templates/escala/cadastro_x_realidade.html:28`; `app/escala/services/cadastro_realidade.py:732`).
- **Falha da leitura continua sem aviso visual:** C10 registra exceção e segue (`app/escala/services/cadastro_realidade.py:601`). Isso realmente repete C2 (`:550`), C4 (`:567`), C6 (`:574`), C7 (`:581`), C8 (`:588`), C9 (`:595`) e C5 (`:609`). O teste confirma ausência de C10 preservando A4 (`app/escala/tests/test_o261v_fase_na_lista.py:158`).
- **Datas, PDF e MORDE-zero ganharam mordida no código do teste:** datas literais comparadas com saída real (`app/escala/tests/test_o261v_fase_na_lista.py:106`, `:121`); PDF extraído com `pypdf` e conteúdo exigido (`:144`); MORDE-zero exige primeiro o positivo real (`:190`). Execução dos testes: **[não verificado]**.
- **Filtro de empresa permanece efetivo:** é aplicado ao construir a linha, depois da coleta C10 (`app/escala/services/cadastro_realidade.py:626`).
- **Uma lista para tela/PDF:** a view chama `lista` e passa os mesmos dados a `pdf_bytes` (`app/escala/views.py:490`); PDF percorre `dados['postos']` e assinaturas (`app/escala/services/cadastro_realidade.py:725`).
- **Porta existente preservada:** C10 retorna apenas o link de cadastro (`app/escala/services/cadastro_realidade.py:490`); nenhuma escrita foi acrescentada nesse ramo (`:492`).
- **Dívida de batida crua permanece anterior:** o leitor chama `parear_turnos` diretamente (`app/escala/services/fase_12x36.py:146`, `:175`), e o registro aponta que a API deveria ler `turnos_do_colab` (`app/core/juizes.py:905`).

## 30/10 08:39 · sessao cdebc79f1e64 · - · - · origem=Ronald (tmux)

Achei um candidato novo de rótulo/universo: C10 diz “12x36”, mas importa todos os achados de `fora_de_fase`, inclusive `24x48`. Isso não exige juiz novo para corrigir, mas pede alinhar o rótulo ao universo do leitor. Fontes: `app/escala/services/cadastro_realidade.py:112`, `app/escala/services/cadastro_realidade.py:337`, `app/escala/services/fase_12x36.py:29`, `app/escala/services/fase_12x36.py:106`; lei: `CLAUDE.md:35-36`.

O caso é possível pelo código: o ciclo `24x48` trabalha quando `delta % 3 == 0`; se os turnos vierem um dia deslocados, haverá pelo menos três dias de trabalho sem turno e três folgas com turno dentro de 14 dias, satisfazendo o leitor. Fontes: `app/escala/models.py:1264-1266`, `app/escala/services/fase_12x36.py:179-187`. Não executei reprodução no banco. [não verificado]

As triagens dos testes procedem por inspeção: [nao verificado]

- Datas agora têm expectativas literais independentes e comparação tanto do leitor quanto da C10; todas as datas precisam estar no detalhe. `app/escala/tests/test_o261v_fase_na_lista.py:29-30`, `:106-112`, `:121-129`.
- PDF agora é extraído com `pypdf` e precisa conter C10 e datas dos dois lados. `app/escala/tests/test_o261v_fase_na_lista.py:144-148`. A dependência já existe. `app/requirements.txt:23`.
- MORDE-zero exige positivo real antes do mock vazio; MORDE-um substitui o achado real por colaborador em fase. `app/escala/tests/test_o261v_fase_na_lista.py:177-193`.
- Isso comprova mordida potencial por leitura; execução verde e RED histórico não foram verificados. [não verificado] [nao verificado]

A dívida da ata permanece: o leitor chama `parear_turnos` diretamente sem `papel_por_minuto`, tanto na janela ampla quanto na detecção. `app/escala/services/fase_12x36.py:146`, `:175`. O pareador documenta que sem alimentação o papel sai do `tipo`; o caminho canônico carrega o papel da ata e o repassa. `app/ponto/turnos.py:1212-1213`, `:1911`, `:1973-1975`. Ela está registrada em `app/core/juizes.py:905-906`; o diff apresentado só expõe ID e datas já contadas nesse leitor. `app/escala/services/fase_12x36.py:192`, `:207-208`.

A cópia não disponibilizou metadados Git; comparação real com `f0d319ba` e ordem temporal de teste/código ficaram não verificadas. [não verificado]

## 30/10 08:39 · sessao e3bb5ee2fac5 · - · - · origem=Ronald (tmux)

Achei uma falha nova de rótulo: C10 declara sempre “12x36”, mas consome integralmente um leitor cujo universo inclui `24x48`; a regra desse ciclo pode alcançar os dois pisos da detecção, portanto um achado `24x48` será apresentado como `12x36` na lista e no PDF. Isso contraria “rótulo diz o que a conta faz”; não exige juiz novo para corrigir a apresentação. Evidências: `app/escala/services/cadastro_realidade.py:112`, `:337`, `:643`, `:732`; `app/escala/services/fase_12x36.py:29`, `:106`, `:179`, `:187`; `app/escala/models.py:1264`; `CLAUDE.md:35`.

As triagens:

- Identificação do vínculo: procede como correção de apresentação; o leitor entrega `ec_id` e a C10 o preserva em `ec` e no detalhe. `app/escala/services/fase_12x36.py:192`; `app/escala/services/cadastro_realidade.py:340`, `:344`.
- Falha do leitor: permanece silenciosa para o consumidor da lista; a igualdade com o comportamento dos vizinhos é factual. O aval literal fornecido não contém exigência explícita de “mesma guarda” [não verificado]. Não encontrei esse complemento nos documentos pesquisados [não verificado]. `app/escala/services/cadastro_realidade.py:550`, `:567`, `:574`, `:581`, `:588`, `:595`, `:602`, `:609`.
- Selos: as datas esperadas são literais; há extração textual do PDF; MORDE-zero exige antes a presença sem mock. Não executei testes nesta inspeção [não verificado]. `app/escala/tests/test_o261v_fase_na_lista.py:29`, `:106`, `:145`, `:189`.
- Pareamento cru: a dívida está documentada e o leitor continua chamando `parear_turnos`; esse mecanismo não foi introduzido pelo diff apresentado. `app/core/juizes.py:905`; `app/escala/services/fase_12x36.py:146`, `:175`.

Não identifiquei novo escritor, novo relógio ou derivação de fase no adaptador: ele somente projeta a resposta do leitor existente. `app/escala/services/cadastro_realidade.py:330`.

## 30/10 08:38 · sessao 302bc57ad3d7 · O261-NOTURNO · verificador · origem=Code

**O achado principal é a contradição entre o novo texto e o dia destacado nas telas.** Há também custo adicional antes da deduplicação e lacunas no selo. (`app/templates/chamados/partials/_card_atendimento.html:90`; `app/chamados/catalogo/query.py:100`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:128`)

1. **A tela pode continuar induzindo a resposta pelo dia D.** O card destaca “Estamos falando do dia” usando `data_evento`, imediatamente antes de imprimir a pergunta. Para a saída noturna, passa a apresentar cabeçalho **21/10** e texto **22/10**. O modal também imprime “Refere-se ao dia” pela identidade; seu aviso de divergência compara identidade com dia do chamado, sem comparar o dia textual com o instante do marco. (`app/templates/chamados/partials/_card_atendimento.html:87`; `app/templates/chamados/partials/_card_atendimento.html:93`; `app/templates/chamados/partials/modal_fio.html:157`; `app/templates/chamados/partials/modal_fio.html:180`; `app/chamados/services/fio.py:85`)

   A API igualmente entrega `data_evento_label` junto do texto gravado, preservando essa combinação D/D+1; como o cliente nativo a apresenta é **[não verificado]**. (`app/api/views.py:1168`; `app/api/views.py:1170`)

2. **Há mudança adicional no tratamento de data com hora.** O helper agora corta qualquer entrada nos primeiros dez caracteres: `2026-10-21T19:00:00` vira `21/10/2026`, inclusive sem colaborador. Isso amplia a normalização além da escolha do dia da lâmpada e altera a preservação do valor bruto descrita no diff anterior. (`app/chamados/catalogo/query.py:95`; `app/chamados/catalogo/query.py:97`)

   **Não encontrei divergência textual entre os dois catálogos:** a comparação estática dos 16 motivos deu textos iguais. Motivos sem `{data}` retornam o texto diretamente; motivo desconhecido sem `padrao` retorna `None`; `padrao=''` permanece vazio; um `padrao` fornecido não substitui motivo conhecido. Portanto, a troca de catálogo nos seis sítios não demonstra outra mudança de redação. (`app/chamados/models.py:2355`; `app/chamados/catalogo/perguntas.py:34`; `app/chamados/catalogo/query.py:83`)

3. **O disparo pode pagar duas consultas novas por alvo, inclusive na tentativa deduplicada.** Cada formatação com colaborador e data válida executa um `SELECT CelulaDia`. A fábrica chama `criar_ou_atualizar_disputa`, que já gera perguntas, e depois chama o gerador novamente. Para motivos datados, a formatação acontece antes do dedup; a segunda tentativa ainda consulta a célula. (`app/chamados/catalogo/query.py:98`; `app/chamados/services/fabrica_da_celula.py:122`; `app/chamados/services/fabrica_da_celula.py:124`; `app/chamados/services/disputa_emissao.py:606`; `app/chamados/services/disputa_emissao.py:618`; `app/chamados/services/disputa_emissao.py:317`; `app/chamados/services/disputa_emissao.py:370`; `app/chamados/services/disputa_emissao.py:386`)

   Isso está dentro do laço de frota, embora a célula já tenha sido carregada na seleção dos alvos. Volume e latência efetivos são **[não verificado]**. Não demonstrei essa mesma multiplicação em toda passagem do reconciliador: os caminhos rastreados reatam perguntas existentes e reemitem chamados. (`app/chamados/management/commands/disparar_perguntas_competencia.py:104`; `app/chamados/management/commands/disparar_perguntas_competencia.py:123`; `app/chamados/management/commands/disparar_perguntas_competencia.py:279`; `app/chamados/reconciliador.py:862`; `app/chamados/reconciliador.py:902`; `app/chamados/reconciliador.py:1347`)

4. **O plantio em leitura não introduz escrita de banco no derivador rastreado, mas acrescenta consultas.** A persistência da pendência está no materializador, depois da derivação; o lote passa apenas uma lista descartável de erros. Exceções engolidas e divergências podem, contudo, incrementar contadores e emitir log. (`app/chamados/services/validacao.py:830`; `app/chamados/services/materializacao.py:379`; `app/chamados/services/materializacao.py:786`; `app/chamados/services/materializacao.py:790`; `app/core/observ.py:73`)

   Para `07:00`/`00:00`, a derivação pode consultar a grade no crivo e novamente na escolha do tipo, além de carregar vínculo e marcos no ajuste overnight. Em diurno, quando os dois instantes são iguais e não há conflito, ainda executa dois `exists()` iguais. (`app/chamados/services/materializacao.py:355`; `app/chamados/services/materializacao.py:266`; `app/chamados/services/materializacao.py:404`; `app/chamados/juizes.py:301`; `app/chamados/services/validacao.py:831`; `app/chamados/services/validacao.py:834`)

5. **Ausência de destino/contexto não quebra por si; existem outros limites de exceção.** Havendo `data_evento`, contexto vazio é aceito, e os leitores de marco toleram chamado `None`. O teste novo monta precisamente essa situação. (`app/chamados/services/materializacao.py:323`; `app/chamados/services/materializacao.py:327`; `app/chamados/juizes.py:461`; `app/chamados/juizes.py:475`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:86`)

   Entretanto, o ajuste overnight fica fora do `try` da hora e alcança o parse desprotegido de `dna.marcos`; uma hora inválida nesse DNA pode propagar exceção até a classificação da fila. Ocorrência desse dado em produção é **[não verificado]**. (`app/chamados/services/materializacao.py:396`; `app/chamados/services/materializacao.py:404`; `app/chamados/juizes.py:303`; `app/escala/models.py:1104`; `app/chamados/services/validacao.py:881`)

6. **O selo morde as duas formas antigas, mas não garante “um produtor”.** Ele procura somente `.format(data=...)` e `.replace('{data}', ...)`, em arquivos Python sob `chamados/`. Executei o detector extraído em memória: f-string, `%`, formato posicional, `str.format(t, data)` e `format_map` escapam; as formas cobertas tiveram zero ocorrências atuais. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:120`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:123`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:140`)

   Existe produtor administrativo com `%`, ignorado pelo selo, embora não formate data. Outro interpolador atual de `{data}` fora de `chamados/` não foi localizado; inexistência absoluta é **[não verificado]**. (`app/chamados/services/acoes_disputa.py:259`)

   Os testes comportamentais têm controles úteis — conflito em D+1, conflito em D e ausência de conflito —, mas não exercitam os seis emissores, a identidade persistida, os intervalos, `padrao` ou data com hora. O DNA de teste deixa os intervalos vazios. Suíte Django e RED histórico são **[não verificado]**. (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:36`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:60`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:96`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:110`)

7. **Não vejo um segundo juiz novo nesses trechos; há fontes paralelas preexistentes.** O novo leitor usa o mapa `MARCO_DA_PERGUNTA` e lê o instante gravado; o lote chama o derivador existente. Porém, seu envelope continua dependendo de um fallback que escolhe escala ativa e lê o template, enquanto o plantio usa vínculo do dia e DNA congelado. Essa divergência permanece possível. Nos trechos novos, não há leitura de “agora”. (`app/chamados/catalogo/query.py:59`; `app/chamados/catalogo/query.py:67`; `app/chamados/services/validacao.py:812`; `app/chamados/services/validacao.py:830`; `app/chamados/juizes.py:1251`; `app/chamados/juizes.py:1256`; `app/chamados/juizes.py:301`; `app/escala/models.py:1099`)

8. **Os consumidores existentes não fiscalizam o novo contrato textual.** INV4 só pergunta se o helper retorna algo, sem colaborador; a auditoria de cegas aceita qualquer ocorrência `dd/mm/aaaa`. Nenhum desses controles confere o dia do DNA. O teste de correção cega também chama sem colaborador e simula a aplicação com atribuição/save direto; não prova a ação real nem o noturno. O handler funcional dessa ação não foi localizado: **[não verificado]**. (`app/chamados/management/commands/auditar_disputas.py:52`; `app/chamados/management/commands/auditar_disputas.py:74`; `app/chamados/management/commands/auditar_disputas.py:191`; `app/chamados/tests/test_corrigir_pergunta_cega.py:54`; `app/templates/chamados/partials/modal_fio.html:187`)

   Há dependência do texto integral: a cura de limbo procura a pergunta inteira no followup, e a nota de resposta inclui esse texto no hash de deduplicação. Uma correção retrospectiva pode afetar esse casamento. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:31`; `app/chamados/services/disputa_emissao.py:1314`)

As contagens de produção presentes na docstring não foram confirmadas por registros fornecidos; alcance real, comparação histórica dos commits e execução RED/VERDE permanecem **[não verificado]**. (`app/chamados/catalogo/query.py:51`)

## 30/10 08:36 · sessao 2ca5c49cf01b · O261-V-FASE · verificador · origem=Code

Encontrei uma incompatibilidade de vínculo e duas falhas de proteção nos testes. A integração reutiliza o leitor existente, mas isso não garante que a linha mostre o vínculo acusado. (`app/escala/services/cadastro_realidade.py:336`, `:338`; `app/escala/services/fase_12x36.py:114`.)

1. **C10 pode acusar um vínculo sob o cabeçalho de outro. Classe: derivação paralela de fonte única.**  
   O detector colapsa os vínculos por colaborador; com a ordenação `-data_inicio`, o último sobrescreve os anteriores, conservando o mais antigo. A lista escolhe separadamente o vínculo vigente pela juíza. O adaptador C10 não transporta a identidade do vínculo acusado. Resultado possível: template/âncora antigos no detalhe, cabeçalho do vínculo novo e ação sem identificar qual corrigir. Reproduzi as duas seleções com objetos sintéticos em memória: detector escolheu vínculo 1; juíza, vínculo 2. O estado com dois ativos é admitido pelo modelo atual. (`app/escala/services/fase_12x36.py:114`; `app/escala/models.py:1003`, `:1019`; `app/escala/services/cadastro_realidade.py:338`, `:488`, `:521`; `app/escala/alimentacao.py:254`.) Isso compromete o agrupamento **posto → vínculo → assinatura → ação** exigido pela L-040. (`app/docs/LEIS.md:88`.)

2. **Falha do leitor desaparece como ausência de C10. Classe: ausência de sinal lida como sinal bom; selo que protege essa omissão.**  
   A exceção apenas gera log; o retorno não informa que a fase ficou sem leitura. Assim, para tela/PDF, “leitor falhou” e “nenhum achado de fase” ficam indistinguíveis. C3 já oferece o precedente correto: `sem_medida` explícito, apresentado na tela. O teste novo exige justamente C10 vazia após a exceção, sem exigir aviso de leitura incompleta. (`app/escala/services/cadastro_realidade.py:599`, `:684`; `:556`; `app/templates/escala/cadastro_x_realidade.html:17`; `app/escala/tests/test_o261v_fase_na_lista.py:139`.) A própria lista identifica essa classe de erro como “ausência de sinal lida como sinal bom”. (`app/escala/services/cadastro_realidade.py:540`.)

3. **Os selos não protegem integralmente os dias nem a igualdade do PDF. Classe: selo/RED sem mordida.**
   - As datas da lista são comparadas ao próprio leitor; a verificação do leitor exige apenas comprimento, strings e ISO. Executei esse método isoladamente com datas repetidas de `1900-01-01`: passou. Portanto, uma alteração que preserve comprimentos pode publicar dias errados sem essa proteção detectar. (`app/escala/tests/test_o261v_fase_na_lista.py:94`, `:100`, `:111`.)
   - O teste do PDF exige somente o prefixo `%PDF`; um PDF que omitisse C10 e suas datas satisfaria essa asserção, contrariando o contrato protegido pela L-040. O código atual imprime `detalhe`, mas o teste não prende esse conteúdo. (`app/escala/tests/test_o261v_fase_na_lista.py:128`; `app/escala/services/cadastro_realidade.py:727`; `app/docs/LEIS.md:88`.)

Também conferi os pontos pedidos:

- **Remover a integração quebra testes positivos.** O caso real exige C10 e o mock positivo exige exclusivamente o colaborador devolvido pelo leitor. Já o mock vazio continuaria verde sem a integração; a frase “os dois ficam vermelhos” está exagerada. (`app/escala/tests/test_o261v_fase_na_lista.py:97`, `:158`, `:170`, `:12`.)
- **`empresas` filtra corretamente linhas e contadores C10**, porém só depois da varredura global. Cada HTML/PDF e os commands C7/C8 passam a pagar a leitura de toda a frota, incluindo duas janelas de batidas e dois pareamentos por colaborador com batidas. Não reproduzi a medição de “1,0 s”; latência atual **[não verificado]**. (`app/escala/services/cadastro_realidade.py:336`, `:601`, `:626`, `:647`; `app/escala/services/fase_12x36.py:128`, `:140`, `:146`, `:175`; `app/escala/views.py:490`; `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`.)
- **As chaves novas são aditivas para os consumidores encontrados:** API encaminha os vínculos; fila e propositor extraem IDs; pauta usa campos nomeados. Os commands continuam exigindo especificamente C7/C8 para fechar. (`app/api/views_mensageria.py:1675`; `app/chamados/services/fila_por_causa.py:173`; `app/escala/services/propositor.py:455`; `app/escala/services/fase_12x36.py:225`; `app/escala/management/commands/fechar_avisos_regua_na_lista.py:32`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:40`.)
- **Há dívida anterior no leitor reutilizado:** ele pareia batidas cruas sem o papel da ata; esse caminho já está registrado como classe C, devendo ler `turnos_do_colab`. C10 amplia o consumo dessa dívida, embora não introduza regra própria de fase. (`app/escala/services/fase_12x36.py:146`, `:175`; `app/ponto/turnos.py:1199`, `:1212`, `:1911`; `app/core/juizes.py:905`.)

Não identifiquei no trecho novo segundo escritor, nova leitura de relógio ou alteração da ordem de travas. (`app/escala/services/cadastro_realidade.py:330`, `:488`, `:599`; `app/escala/services/fase_12x36.py:204`.) Suíte Django completa, presença nominal dos quatro dias do col451 e identidade Git da cópia: **[não verificado]**.

## 30/10 08:35 · sessao 5216099b546d · - · - · origem=Ronald (tmux)

Achados concretos:

- **Dia contraditório na tela:** o texto novo pode nomear D+1, mas o modal continua mostrando “Refere-se ao dia D”; a badge deriva de `data_evento`, e o aviso de divergência compara somente `data_evento` com o dia do chamado. O conflito novo entre texto/DNA e badge passa sem aviso. `app/templates/chamados/partials/modal_fio.html:157`, `:159`, `:180`; `app/chamados/services/fio.py:85-88`. O preview do lote e o explicador também mostram identidade junto do texto: `app/templates/chamados/partials/_lote_preview.html:24`; `app/chamados/services/explicador.py:70-71`.

- **Mesmo par divergente exposto pela API:** `data_evento_label` preserva D ao lado do texto D+1; a fila de mensageria expõe a mesma combinação. Impacto na renderização do cliente nativo [não verificado]. `app/api/views.py:1168-1170`; `app/api/views_mensageria.py:564-568`.

- **Selo tem alcance menor que a promessa:** a raiz é só `chamados/`, exclui templates e só lê `.py`; reconhece exclusivamente chamadas atributo `.format(data=...)` e `.replace('{data}', ...)`. F-string, `%`, formato posicional, `str.format(t, data)` e `format_map` passam. `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:120-131`, `:140-149`. Extraí e executei somente `_copias` em memória: confirmou as duas formas reconhecidas e todas essas fugas; o scanner atual devolveu zero ocorrências nas formas cobertas. A cobertura limitada é demonstrável pelo corpo citado; suíte Django/RED histórico [não verificado].

- **Teste não cobre contratos importantes:** chama diretamente o helper, não os seis emissores, e `_dna` deixa os intervalos vazios; não comprova manutenção de `data_evento`, texto integral, motivos externos aos quatro, `padrao`, hora no contexto, DNA ausente/corrompido ou turno 24h com horas coincidentes. `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:36-38`, `:60-82`, `:85-113`.

- **INV4 não verifica o dia mostrado:** chama helper sem colaborador e só compara retorno com `None`; a auditoria de cegas procura qualquer data por regex. Portanto o dia errado no texto permanece fora dessas detecções. `app/chamados/management/commands/auditar_disputas.py:69-81`, `:188-194`.

- **Correção cega sem prova de integração:** o teste simula a aplicação atribuindo/salvando `pergunta`; não exercita handler da ação que o template envia. `app/chamados/tests/test_corrigir_pergunta_cega.py:54-63`; `app/templates/chamados/partials/modal_fio.html:185-190`. Handler funcional da ação nesta árvore [não verificado].

- **Leitor depende do texto exato gravado:** `nota_da_resposta` localiza followup contendo a pergunta inteira; as notas de resposta gravam o texto da pergunta e o incluem no hash de deduplicação. Alterar retrospectivamente esse texto pode afetar a localização; alteração retrospectiva pelo diff [não verificado]. `app/chamados/management/commands/curar_perguntas_em_limbo.py:26-34`; `app/chamados/services/disputa_emissao.py:1314-1321`.

Rastreio adicional:

- Há outro produtor de pergunta com `%`, ignorado pelo selo, na reabertura administrativa; ele não interpola data e é preexistente ao diff fornecido. `app/chamados/services/acoes_disputa.py:259-263`. Outro interpolador atual de `{data}` fora de `chamados/` não foi localizado; inexistência absoluta [não verificado].

- Caso de duas pausas merece ressalva: ambos os pares usam os mesmos motivos, e o juiz de fato lê todos os pares; o novo helper toma somente `hii`/`hfi`. Se os pares estiverem em dias distintos, escolhe o dia do primeiro par. Existência desse caso no dado fornecido [não verificado]. `app/ponto/marcos.py:19-28`, `:35-40`; `app/chamados/juizes.py:823-827`, `:850-869`; `app/chamados/catalogo/query.py:60-67`.

## 30/10 08:35 · sessao 97e8068ed3f7 · - · - · origem=Ronald (tmux)

- **Dia contraditório na tela do colaborador.** O cartão exibe “Estamos falando do dia” usando `p.data_evento`, imediatamente antes do texto gravado; logo uma pergunta noturna nova poderá mostrar cabeçalho D e texto D+1. O cartão é incluído no atendimento do colaborador. (`app/templates/chamados/partials/_card_atendimento.html:87-93`; `app/templates/chamados/meu_atendimento.html:127`, `:144`; `app/chamados/catalogo/query.py:98-101`)

- **Outro leitor do dia mostrado permanece no modal.** O modal imprime `perg.pergunta`, depois “Refere-se ao dia” pelo filtro `data_do_fato`; esse filtro lê `fio.data_do_fato_de`, que devolve `p.data_evento`, sem consultar o instante do marco. Se D coincide com o dia do chamado, o aviso de divergência também não aparece, embora o texto nomeie D+1. (`app/templates/chamados/partials/modal_fio.html:157-180`; `app/chamados/templatetags/chamados_extras.py:29-31`; `app/chamados/services/fio.py:84-88`)

- **O custo no disparo pode ser duas queries novas por alvo, inclusive tentativa deduplicada.** Cada interpolação com colaborador e data válida faz `CelulaDia.objects.filter(...).first()`. O lote chama `perguntar` por alvo; `perguntar` chama `criar_ou_atualizar_disputa`, que já gera perguntas, e chama `gerar_perguntas_disputa` novamente. Para os quatro motivos datados, a formatação ocorre antes do dedup. Portanto, nos caminhos normais dessas duas chamadas, a segunda ainda paga o SELECT antes de descobrir a pergunta existente. (`app/chamados/catalogo/query.py:87-100`; `app/chamados/management/commands/disparar_perguntas_competencia.py:279-289`; `app/chamados/services/fabrica_da_celula.py:118-124`; `app/chamados/services/disputa_emissao.py:605-606`, `:618`, `:317`, `:369-387`) Volume e latência efetivos em produção: **[não verificado]**.

- **O objeto célula já disponível no lote não evita a releitura.** O disparador percorre as células com colaborador carregado, mas guarda somente `(colaborador, data, motivo)`; o helper depois consulta novamente a célula e não possui cache. O caminho do julgamento também recebe `_cel` e termina na mesma fábrica. (`app/chamados/management/commands/disparar_perguntas_competencia.py:104-123`; `app/chamados/catalogo/query.py:98-100`; `app/ponto/services/cartorio.py:768-770`; `app/chamados/services/fabrica_da_celula.py:144-149`)

- **INV4 não verifica correspondência do dia textual.** Usa o helper sem colaborador somente para decidir se uma pergunta deveria existir. O detector de pergunta cega verifica apenas presença de alguma sequência `dd/mm/aaaa`; não compara texto, identidade e instante. Portanto não detectará as contradições acima. (`app/chamados/management/commands/auditar_disputas.py:49-52`, `:69-82`, `:188-195`)

- **`corrigir_pergunta_cega` não tem teste da rota nem do noturno.** O teste chama o helper sem colaborador, comenta “simula aplicar” e grava diretamente `pergunta`; assim não comprova que a ação real passará colaborador ou corrigirá para D+1. O formulário existente envia `confirmar_correcao_pergunta`; a implementação do handler não foi localizada na cópia **[não verificado]**. (`app/chamados/tests/test_corrigir_pergunta_cega.py:54-63`; `app/templates/chamados/partials/modal_fio.html:182-190`)

- **Outros consumidores do texto gravado:** API retorna `p.pergunta` diretamente; o explicador apresenta a identidade `data_evento` como dia separado e o texto gravado; a cura de limbo procura a pergunta textual exata dentro do followup. Alterar texto já gravado poderia afetar esse último casamento; o diff apresentado não faz migração de textos existentes. (`app/api/views.py:2620-2634`; `app/chamados/services/explicador.py:68-71`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:26-34`; `app/chamados/services/disputa_emissao.py:389`)

- **Não atribuir automaticamente esse N+1 ao reconciliador.** O caminho rastreado reata perguntas existentes e reemite chamados; `reemitir_chamado_do_dia` usa `ChamadoColaborador.abrir` e `reapontar_resolvedora`, sem formatar a pergunta. O consumidor de eventos chama retratação e encerramento de órfãs. Uma query nova por pergunta de toda passagem do reconciliador não está demonstrada nesses caminhos. (`app/chamados/reconciliador.py:862-874`, `:896-905`, `:1293-1303`, `:1347-1348`)

## 30/10 08:35 · sessao ff61757ac193 · - · - · origem=Ronald (tmux)

Achados no caminho do lote/plantio, por inspeção estática:

- **Custo adicional real por pergunta classificada.** `fila_validar_classificada` percorre perguntas e chama `classe_do_lote` individualmente; agora cada candidata com `data_evento` deriva o plantio antes de consultar conflito. Em diurno, os dois instantes podem ser iguais e o código executa dois `exists()` iguais quando o primeiro retorna falso, pois não elimina repetição. (`app/chamados/services/validacao.py:817`, `:830`, `:831`, `:834`, `:880`.)

- **A derivação faz mais que aritmética.** Para resposta `07:00`/`00:00`, consulta `grade_da_celula` no crivo do relógio; depois `_tipo_pelo_marco` consulta novamente a grade, e o ajuste overnight carrega vínculo/célula e os marcos. Essas leituras não recebem dados previamente carregados. (`app/chamados/services/materializacao.py:354`, `:355`, `:370`, `:263`, `:266`, `:404`; `app/chamados/juizes.py:300`; `app/escala/servico_jornada.py:41`, `:47`, `:58`, `:69`; `app/escala/models.py:1097`.)
  A grade consulta `CelulaDia`, e célula sem `ata.minutos_previstos` pode ainda chamar `minutos_previstos_do_dia`, que carrega célula/vínculo. O custo preciso em produção é [não verificado]. (`app/escala/services/leitor_celula.py:325`, `:346`, `:353`; `app/escala/utils.py:2198`.)

- **Sem chamado-destino e contexto vazio não quebram por si.** Havendo `data_evento`, o contexto pode ser `{}`/`None`; `entrada_de_marco(None)` e `_marco_faltante(None)` devolvem `None`. O teste novo cria precisamente pergunta sem chamado-destino e disputa com contexto `{}`. (`app/chamados/services/materializacao.py:323`, `:327`, `:328`; `app/chamados/juizes.py:461`, `:475`; `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:87`.)

- **Não encontrei escrita de banco no derivador.** `_ts_tipo_do_plantio` só preenche listas de retorno; a persistência de `pendencia_admin` fica no materializador, que passa uma lista própria e chama `save()` depois. O lote passa `[]` para erros e omite `pendencia`, portanto não entra nessa escrita. Há efeito de observabilidade: exceções engolidas/divergência incrementam contador de processo e registram log. (`app/chamados/services/materializacao.py:379`, `:404`, `:786`, `:787`, `:790`, `:791`; `app/chamados/services/validacao.py:830`; `app/core/observ.py:73`.)

- **Não é função universalmente protegida contra exceções.** O parse da hora está protegido, mas o ajuste overnight fica fora do `try`; este chama `ec.marcos_do_dia`, cujo parse de `dna.marcos` faz `split(':')`/`int()` sem proteção. DNA com hora inválida pode agora derrubar a classificação da fila; ocorrência no dado fornecido é [não verificado]. Falta de chamado/contexto, especificamente, não demonstra esse problema. (`app/chamados/services/materializacao.py:396`, `:404`; `app/chamados/juizes.py:301`, `:303`; `app/escala/servico_jornada.py:69`; `app/escala/models.py:1101`, `:1104`, `:1105`; `app/chamados/services/validacao.py:881`.)

- **Outro juiz já existente continua divergente da célula.** O envelope do lote vem de `delta_da_resposta`; sem marco no chamado, `_marco_esperado_da_grade` escolhe vínculo `ativa=True`, lê `tipo_escala.marcos_do_dia(d)` e ignora o DNA congelado. O plantio agora consultado usa `escala_vigente` e `ec.marcos_do_dia`, que lê DNA. É divergência preexistente, não criada pelo diff, mas o lote continua dependendo dos dois caminhos. (`app/chamados/services/validacao.py:812`, `:815`; `app/chamados/juizes.py:1303`, `:1305`, `:1251`, `:1256`; `app/chamados/juizes.py:301`, `:303`; `app/escala/models.py:1099`.)

- **`data_hora` não ganha a checagem efetiva do plantio própria desse tipo.** A classe aceita `data_hora`, mas chama diretamente `_ts_tipo_do_plantio`, que espera a resposta inteira em `HH:MM`; resposta `2026-10-22T07:00` cai no erro capturado e retorna timestamp `None`. O juiz existente `_slot_esperado` tem ramo específico que interpreta data e hora. Logo o novo segundo instante não cobre esse tipo; impacto em perguntas reais é [não verificado]. (`app/chamados/services/validacao.py:810`, `:830`, `:832`; `app/chamados/services/materializacao.py:391`, `:397`, `:399`; `app/chamados/juizes.py:331`.)

Não executei testes nem consultei banco/log; execução verde/RED permanece [não verificado]. [nao verificado]

## 30/10 08:35 · sessao 8afa26187ff4 · O261-VETO-NA-PORTA · sonda · origem=Code

Transcrição do dado fornecido, sem validação independente. [nao verificado]

| Módulo | sem_dia | ec_none | veto=None | veto=VETO_* | Total das linhas | Fonte |
|---|---:|---:|---:|---:|---:|---|
| batida_ausente | — | — | 2180 | — | 2180 | [nao verificado] |
| turno_aberto_24h | 3 | 2 | 9 | — | 14 | [nao verificado] |
| turno_aberto | — | — | 88 | — | 88 | [nao verificado] |
| **Total** | **3** | **2** | **2277** | **—** | **2282** | [nao verificado] |

“—” significa nenhuma linha correspondente no dado fornecido; não certifica zero. **ERROS=0**, conforme o anexo. [nao verificado]

Pela leitura do código:

- `dia_do_fato(chamado, fortes=False)` recebe o objeto chamado e retorna uma data ou `None`; `dia_do_fato(ch)` usa o padrão `fortes=False`. [app/chamados/juizes.py:78](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:78)
- `vinculo_do_emissor(colab, data)` recebe colaborador e data, nessa ordem, e delega a `escala_vigente(colab, data)`. O consumidor no mesmo arquivo passa `cel.colaborador, cel.data`. [app/ponto/services/furos_vetados.py:10](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/furos_vetados.py:10) [app/ponto/services/furos_vetados.py:52](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/furos_vetados.py:52)

**Não é possível confirmar as chamadas da sonda:** o anexo contém sua saída, sem imports ou chamadas, e não localizei `sonda_veto_ec_none.py` na cópia. Funções efetivamente chamadas e argumentos efetivamente passados: **[nao verificado]**.

## 30/10 08:34 · sessao fe33574f015e · - · - · origem=Ronald (tmux)

Não achei quebra direta dos consumidores pelas novas chaves ou pela inclusão de C10:

- API devolve `vinculos` diretamente; datas ISO são serializáveis. Propositor e fila extraem somente IDs; pauta acessa campos nomeados. (`app/api/views_mensageria.py:1670`, `app/escala/services/propositor.py:455`, `app/chamados/services/fila_por_causa.py:173`, `app/escala/services/fase_12x36.py:220`)
- Filtro de empresas vale antes de montar linhas e contadores C10; não há vazamento C10 demonstrado. (`app/escala/services/cadastro_realidade.py:626`, `app/escala/services/cadastro_realidade.py:647`, `app/escala/services/cadastro_realidade.py:674`)
- Commands continuam fechando exclusivamente assinaturas C7/C8; C10 não satisfaz esses predicados. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:32`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:40`)
- Tela e PDF consomem o mesmo `dados`; ambos exibem `detalhe`, que contém as datas C10. (`app/escala/views.py:490`, `app/escala/services/cadastro_realidade.py:340`, `app/escala/services/cadastro_realidade.py:730`, `app/templates/escala/cadastro_x_realidade.html:28`)

Dois pontos merecem consolidação:

1. **Custo global por leitura, inclusive empresa vazia e commands interessados só em C7/C8.** `_fase_deslocada()` ignora `empresas` e chama o detector sem estreitar IDs; a filtragem ocorre depois. O caminho carrega EC, duas janelas de batidas, células e folgas, pareia duas vezes por colaborador e visita 14 dias por candidato. Isso agora acontece em cada request HTML/PDF e nos dois commands. A medição atual de latência não foi reproduzida; números antigos de 6,5 s/5.070 queries no docstring precedem as otimizações existentes. (`app/escala/services/cadastro_realidade.py:333`, `app/escala/services/cadastro_realidade.py:336`, `app/escala/services/fase_12x36.py:106`, `app/escala/services/fase_12x36.py:128`, `app/escala/services/fase_12x36.py:140`, `app/escala/services/fase_12x36.py:146`, `app/escala/services/fase_12x36.py:160`, `app/escala/services/fase_12x36.py:170`, `app/escala/services/fase_12x36.py:175`, `app/escala/services/fase_12x36.py:179`, `app/escala/views.py:490`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`)

2. **Risco herdado de identidade do vínculo, exposto pela nova integração.** Detector colapsa vínculos ativos por colaborador, com ordenação padrão decrescente de início: havendo dois ativos, sobra o mais antigo. A lista, separadamente, escolhe o vínculo vigente pelo juiz; C10 leva apenas colaborador, sem identidade do vínculo. Pode aparecer detalhe da âncora antiga sob cabeçalho do vínculo novo. O banco permite esse estado; o comentário do modelo declara explicitamente que testes existentes dependem dele. Classe possível: **derivação paralela da fonte única / contador e universo**; não atribuir a C10 a criação desse defeito herdado. (`app/escala/services/fase_12x36.py:114`, `app/escala/models.py:1003`, `app/escala/models.py:1019`, `app/escala/models.py:1037`, `app/escala/services/cadastro_realidade.py:338`, `app/escala/services/cadastro_realidade.py:521`, `app/escala/services/cadastro_realidade.py:525`)

Também há dívida registrada no próprio caminho reutilizado: detector pareia batidas cruas sem alimentar marcos ou papel da ata; o registro de juízes já classifica esse caminho como classe C e manda ler `turnos_do_colab`. A nova C10 reutiliza o leitor autorizado; isso não prova juiz novo, mas propaga a dívida existente. (`app/escala/services/fase_12x36.py:146`, `app/escala/services/fase_12x36.py:175`, `app/core/juizes.py:905`, `app/ponto/turnos.py:1199`, `app/ponto/turnos.py:1212`, `app/ponto/turnos.py:1911`)

## 30/10 08:33 · sessao 56381f3a89d2 · - · - · origem=Ronald (tmux)

A integração tem testes que mordem a remoção: o caso real exige C10 com dias, dono e porta; o mock positivo exige exclusivamente o colaborador que o leitor devolveu, mesmo estando em fase no cadastro. Remover `_fase_deslocada` ou substituí-la por um cálculo independente quebra essas expectativas. `app/escala/tests/test_o261v_fase_na_lista.py:93`, `app/escala/tests/test_o261v_fase_na_lista.py:158`.

Encontrei duas limitações concretas de cobertura, classe **selo/RED sem mordida**:

- **PDF sem prova de conteúdo:** o teste novo aceita qualquer resposta começando por `%PDF`; um PDF que omita todas as C10 e suas datas continuaria passando. A lei exige PDF igual à lista; o código atual percorre as assinaturas e imprime `detalhe`, mas esse contrato não está protegido pelo teste novo. `app/escala/tests/test_o261v_fase_na_lista.py:128`, `app/docs/LEIS.md:88`, `app/escala/services/cadastro_realidade.py:727`.
- **Datas sem resultado independente:** as datas da lista são comparadas às datas produzidas pelo próprio leitor; no leitor, verificam-se apenas comprimento, tipo e ISO. Alterar as duas listas de datas do leitor para datas erradas ou repetidas, preservando seus comprimentos, passa nessas verificações; o mock positivo tampouco executa o leitor real. Isso limita a prova de que são precisamente os dias contados. `app/escala/tests/test_o261v_fase_na_lista.py:94`, `app/escala/tests/test_o261v_fase_na_lista.py:100`, `app/escala/tests/test_o261v_fase_na_lista.py:111`, `app/escala/tests/test_o261v_fase_na_lista.py:165`.

O comentário “os dois ficam vermelhos” exagera: o mock vazio continua verde se a integração C10 for removida, embora o mock positivo e outros casos falhem. `app/escala/tests/test_o261v_fase_na_lista.py:12`, `app/escala/tests/test_o261v_fase_na_lista.py:170`.

Execução da suíte e mutações reais: **[não verificado]**; a avaliação acima é estática e não houve alteração de arquivos. [nao verificado]

## 30/10 08:27 · sessao d5dea9825fc6 · O261-VETO-NA-PORTA · leitura · origem=Code

A cópia **ainda não reúne nascimento e reabertura numa única função**: `abrir` insere por `get_or_create`, `criar` por `create`, e as transições gravam por `_gravar_estado`. O veto compartilhado dos nascimentos é celular, limitado a `MODULOS_DE_TURNO`; não consulta o veto de régua pedido. (`app/chamados/models.py:730`, `app/chamados/models.py:834`, `app/chamados/models.py:231`, `app/chamados/models.py:839`, `app/chamados/models.py:849`)

Abaixo, “desenho” identifica proposta, sem implementação; comportamento futuro permanece **[nao verificado]**.

**1. Contrato de entrada do juiz único**

**Fonte.** As funções existentes precisam representar uma decisão só, distinguindo consulta e execução de consequências:

| Função existente | Responsabilidade atual |
|---|---|
| `motivo_do_veto(colab, ec, data)` | Leitura: degeneração primeiro, suspeita depois; retorna `VETO_DEGENERADA`, `VETO_REGUA` ou `None`. (`app/ponto/services/furos_vetados.py:19`, `app/ponto/services/furos_vetados.py:25`) |
| `regua_degenerada(tipo_escala, data)` | Predicado estrutural: template ausente, exceção ao obter marcos, início/fim ausentes ou iguais. Pergunta ao template, não aos marcos congelados da célula. (`app/escala/regua_defesa.py:16`, `app/escala/regua_defesa.py:23`, `app/escala/regua_defesa.py:26`, `app/escala/regua_defesa.py:31`) |
| `vetar_cobranca_se_degenerada(colab, ec, data)` | Usa aquele predicado, mas também abre/deduplica chamado de cadastro `regua_degenerada`. (`app/escala/regua_defesa.py:45`, `app/escala/regua_defesa.py:51`, `app/escala/regua_defesa.py:80`) |
| `vetar_cobranca_se_regua_suspeita(colab, ec)` | Sem vínculo ou avaliação, não veta; caso contrário delega a `deve_vetar_regua`. (`app/escala/regua_defesa.py:554`, `app/escala/regua_defesa.py:560`) |
| `deve_vetar_regua(ec)` | Exige faixa suspeita e pelo menos duas avaliações baixas. Corroboração administrativa alimenta esse mesmo predicado com confiança zero e dois dias baixos. (`app/escala/regua_defesa.py:641`, `app/escala/regua_defesa.py:644`, `app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:666`) |

**Unidade:** colaborador + dia do fato + vínculo responsável por aquele dia. O resolvedor existente é `vinculo_do_emissor → escala_vigente`; a célula/geradora do dia precede a linha do tempo. Isso difere da chave de dedup dos dois emissores, que é colaborador + módulo + `data_turno`, sem vínculo. (`app/ponto/services/furos_vetados.py:10`, `app/ponto/services/furos_vetados.py:15`, `app/escala/servico_jornada.py:29`, `app/escala/servico_jornada.py:55`, `app/ponto/management/commands/detectar_ausencias.py:688`, `app/ponto/management/commands/reconciliar_grade.py:499`)

Para a porta, o dia já tem autoridade: `dia_do_fato(..., fortes=True)` lê célula, chave do catálogo e `data_turno`; sem esses elementos, retorna `None`. Não autoriza substituir dia desconhecido por hoje ou por criação do chamado. (`app/chamados/juizes.py:78`, `app/chamados/juizes.py:89`, `app/chamados/juizes.py:104`, `app/chamados/juizes.py:118`)

**Universo: há três conjuntos diferentes.**

| Constante | Módulos |
|---|---|
| `FURO_MODULES` | `batida_ausente`, `turno_aberto`, `intervalo_pendente`, `entrada_bloqueada_turno_aberto`. (`app/chamados/catalogo/furo_modulos.py:15`) |
| `MODULOS_DE_TURNO` | `turno_aberto`, `turno_aberto_24h`, `entrada_bloqueada_turno_aberto`. (`app/chamados/reconciliador.py:336`) |
| `CALADOS_POR_ISENCAO` | Inclui `sem_batidas_mes`; não inclui `entrada_bloqueada_turno_aberto`. Não é sinônimo dos anteriores. (`app/chamados/catalogo/modulos.py:235`) |

**Desenho:** para alcançar os emissores de furo/turno enumerados no censo, declararia o universo como **união de `FURO_MODULES` e `MODULOS_DE_TURNO`**, cinco módulos. Usar apenas `MODULOS_DE_TURNO` deixa detector/grade de `batida_ausente` descobertos; usar apenas `FURO_MODULES` deixa `turno_aberto_24h` descoberto. Essa união é proposta, não constante canônica já existente. (`app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/reconciliador.py:336`, `app/ponto/management/commands/reconciliar_grade.py:500`, `app/ponto/management/commands/processar_alertas_avancados.py:121`) **[nao verificado — desenho]**

**Exclusões do contrato proposto:** containers `disputa_supervisao*`, avisos de cadastro — inclusive o próprio `regua_degenerada` — e demais módulos fora desse universo. O cadastro precisa atravessar a porta sem sofrer o veto cuja consequência ele registra; o container não deve herdar o veto de um dos dias das perguntas. (`app/chamados/catalogo/modulos.py:133`, `app/chamados/catalogo/modulos.py:144`, `app/chamados/catalogo/modulos.py:146`, `app/escala/regua_defesa.py:53`) **[nao verificado — desenho]**

O predicado de régua não contém filtros de módulo, isenção, situação, afastamento, feriado, posto ou célula. A exclusão de desligado está no leitor `furos_vetados_por_regua`, que mantém afastados deliberadamente; não pertence a `motivo_do_veto`. (`app/ponto/services/furos_vetados.py:19`, `app/ponto/services/furos_vetados.py:25`, `app/ponto/services/furos_vetados.py:39`, `app/ponto/services/furos_vetados.py:49`)

**Diferenças atuais, sem eleger um caminho como correto:**

| Diferença | `motivo_do_veto` | Detector `_abrir_chamado` | Grade `_emitir_furo` |
|---|---|---|---|
| **`ec=None`** | Retorna `None` imediatamente. (`app/ponto/services/furos_vetados.py:26`) | Trata como template ausente: abre cadastro e retorna `VETO_DEGENERADA`. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/escala/regua_defesa.py:23`, `app/escala/regua_defesa.py:45`, `app/escala/regua_defesa.py:51`) | Igual ao detector; a assinatura inclusive aceita `ec=None`. (`app/ponto/management/commands/reconciliar_grade.py:480`, `app/ponto/management/commands/reconciliar_grade.py:487`) |
| **Degeneração com vínculo** | Consulta `regua_degenerada`, sem escrita. (`app/ponto/services/furos_vetados.py:28`) | Consulta a versão com efeito de cadastro; mesmo predicado estrutural. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/escala/regua_defesa.py:46`) | Igual ao detector. (`app/ponto/management/commands/reconciliar_grade.py:487`) |
| **Cadastro decorrente** | Não abre. (`app/ponto/services/furos_vetados.py:23`) | Abre/deduplica por colaborador + template, não por dia; guarda `data_exemplo`. (`app/escala/regua_defesa.py:51`, `app/escala/regua_defesa.py:54`, `app/escala/regua_defesa.py:76`) | Mesmo efeito. (`app/ponto/management/commands/reconciliar_grade.py:487`) |
| **Suspeita e precedência** | Depois da degeneração, consulta `vetar_cobranca_se_regua_suspeita`. (`app/ponto/services/furos_vetados.py:28`, `app/ponto/services/furos_vetados.py:30`) | Mesma função e precedência. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:604`) | Mesma função e precedência. (`app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:491`) |
| **Feriado do dia** | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | Faixa `feriado` de `fatos_do_dia` retorna `VETO_FERIADO`. (`app/ponto/management/commands/detectar_ausencias.py:615`) | Não há essa consulta dentro de `_emitir_furo`. (`app/ponto/management/commands/reconciliar_grade.py:480`) |
| **Posto fechado no feriado** | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | `not cobra_no_dia` retorna `VETO_FERIADO_DO_POSTO`. (`app/ponto/management/commands/detectar_ausencias.py:622`) | Não há essa consulta dentro de `_emitir_furo`. (`app/ponto/management/commands/reconciliar_grade.py:480`) |
| **Veto celular** | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | Consulta `celula_veta_cobranca`; retorna `VETO_CELULA_<motivo>`. (`app/ponto/management/commands/detectar_ausencias.py:625`) | Mesma consulta e retorno. (`app/ponto/management/commands/reconciliar_grade.py:494`) |
| **Célula ausente** | Não consulta existência. (`app/ponto/services/furos_vetados.py:25`) | Fora de limbo, abre aviso de dia sem grade e retorna `VETO_SEM_CELULA`; limbo atravessa essa guarda. (`app/ponto/management/commands/detectar_ausencias.py:651`, `app/ponto/management/commands/detectar_ausencias.py:654`, `app/ponto/management/commands/detectar_ausencias.py:662`) | `celula_veta_cobranca` aceita célula ausente; não há exigência adicional no método. (`app/ponto/marcos.py:156`, `app/ponto/management/commands/reconciliar_grade.py:494`, `app/ponto/management/commands/reconciliar_grade.py:499`) |
| **Encerrado que não renasceu** | Não trata estado de chamado. (`app/ponto/services/furos_vetados.py:19`) | Retorna `VETO_ENCERRADO_NAO_RENASCEU`, antes de push/fio. (`app/ponto/management/commands/detectar_ausencias.py:729`) | Tenta `reabrir()` novamente, ignora seu resultado e retorna `None`. (`app/ponto/management/commands/reconciliar_grade.py:519`, `app/ponto/management/commands/reconciliar_grade.py:521`) |
| **Limbo após nascimento** | Não trata. (`app/ponto/services/furos_vetados.py:19`) | Pode agrupar sob pai limbo ou registrar ressalva, conforme `pode_calar`. (`app/ponto/management/commands/detectar_ausencias.py:737`, `app/ponto/management/commands/detectar_ausencias.py:748`, `app/ponto/management/commands/detectar_ausencias.py:751`, `app/ponto/management/commands/detectar_ausencias.py:755`) | Não tem esse tratamento no método. (`app/ponto/management/commands/reconciliar_grade.py:499`, `app/ponto/management/commands/reconciliar_grade.py:521`) |

O veto celular também não é sinônimo da guarda compartilhada dos nascimentos: `celula_veta_cobranca` testa `trabalha is False` ou cinco vereditos explícitos; `_celula_que_absolve` alcança somente três módulos com célula vinculada e delega a veredito não vazio fora de `VEREDITOS_ACUSAM`, sem marcos pendentes. (`app/ponto/marcos.py:119`, `app/ponto/marcos.py:159`, `app/ponto/marcos.py:161`, `app/chamados/models.py:849`, `app/chamados/reconciliador.py:975`, `app/chamados/reconciliador.py:977`)

As guardas anteriores do `handle` da grade continuam relevantes: cobertura, isenção e `classificar_falta(...)[1] is None` podem impedir chegar a `_emitir_furo`. Portanto, ausência de teste de feriado dentro do método não prova emissão pelo comando completo. (`app/ponto/management/commands/reconciliar_grade.py:216`, `app/ponto/management/commands/reconciliar_grade.py:230`, `app/ponto/management/commands/reconciliar_grade.py:243`, `app/ponto/management/commands/reconciliar_grade.py:254`)

**Pendência de contrato:** o resultado único para vínculo ausente não está resolvido pelos comportamentos existentes. Substituir os dois emissores por `motivo_do_veto` escolheria silenciosamente “não veta” e retiraria a consequência de cadastro. A Lei fornecida resolve automático versus humano, mas não arbitra essa divergência. (`app/ponto/services/furos_vetados.py:26`, `app/escala/regua_defesa.py:45`, `app/escala/regua_defesa.py:51`) **[nao verificado — decisão para `ec=None`]**

**2. Porta única na aggregate root**

**Não existe função atual pronta para esse papel.** O núcleo que eu promoveria a escritor único de nascimento **e** mudança de vitalidade é `ChamadoColaborador._gravar_estado`: hoje ele já concentra a atribuição, a trilha de estado e o `save`, mas não recebe os nascimentos. Apenas acrescentar veto nele deixaria `abrir/criar` descobertos. (`app/chamados/models.py:185`, `app/chamados/models.py:192`, `app/chamados/models.py:227`, `app/chamados/models.py:231`, `app/chamados/models.py:730`, `app/chamados/models.py:834`) **[nao verificado — desenho]**

Nesse desenho:

- `abrir` conserva dedup/trava; `criar` conserva sua semântica sem dedup; ambos entregam o nascimento ao núcleo antes de inserir. A trava por colaborador já existente em `abrir` deve continuar envolvendo decisão e escrita. (`app/chamados/models.py:599`, `app/chamados/models.py:666`, `app/chamados/models.py:669`, `app/chamados/models.py:805`) **[nao verificado — desenho]**
- `reabrir`, `_renascer_por_premissa_viva` e `transicionar` convergem ao mesmo núcleo; o veto atua em nascimento ou passagem de não vivo para `VIVOS`, antes da mutação. Dedup de vivo e costura em vivo não são nova admissão. (`app/chamados/models.py:183`, `app/chamados/models.py:260`, `app/chamados/models.py:685`, `app/chamados/models.py:770`, `app/chamados/models.py:1089`, `app/chamados/reconciliador.py:805`, `app/chamados/catalogo/motor.py:24`) **[nao verificado — desenho]**
- O núcleo orquestra as funções existentes do veto; não ganha um terceiro predicado. A leitura pura serve aos leitores; a consequência de cadastro fica na execução da decisão, sem transformar consulta informativa em emissão. (`app/ponto/services/furos_vetados.py:23`, `app/ponto/services/furos_vetados.py:25`, `app/escala/regua_defesa.py:46`, `app/escala/regua_defesa.py:51`) **[nao verificado — desenho]**

**Como distingue automático de humano hoje:**

| Porta | Informação disponível hoje |
|---|---|
| `abrir` | Não tem parâmetro próprio de ato, autor ou via. `**lookup` é filtro ORM. (`app/chamados/models.py:599`, `app/chamados/models.py:670`) |
| `criar` | Não tem discriminador próprio; usa `**campos`. O comentário “user-initiated” não garante isso: alerta automático também chama `criar`. (`app/chamados/models.py:805`, `app/ponto/management/commands/processar_alertas_avancados.py:121`) |
| `_renascer_por_premissa_viva` | Recebe somente marcos; delega a `reabrir`. (`app/chamados/models.py:742`, `app/chamados/models.py:770`) |
| `reabrir` | Recebe somente `marcos`, embora documente admin/sistema. (`app/chamados/models.py:1023`) |
| `transicionar` | Recebe `user`, calcula perfil e usa evento `TRANSICAO_HUMANA`. (`app/chamados/models.py:122`, `app/chamados/models.py:125`, `app/chamados/models.py:260`) |
| `_gravar_estado` | Recebe `via`, `user`, `set_user`; a reabertura atual chama sem esses dados. Ao voltar a vivo, limpa `via_encerramento`. (`app/chamados/models.py:185`, `app/chamados/models.py:212`, `app/chamados/models.py:1089`) |

**Desenho:** ato administrativo explícito, autor e motivo devem atravessar toda a cadeia. Usuário presente, HTTP, nome `criar`, execução de command ou `forcar=True` não bastam para identificar override humano. O cartório já distingue `toque` de `forcar`, e há usuários de sistema nos serviços. (`app/ponto/services/cartorio.py:1312`, `app/ponto/services/cartorio.py:1316`, `app/chamados/services/competencia_trancada.py:239`) **[nao verificado — desenho]**

A trilha pode recuperar usuário da request implicitamente, mas isso é atribuição de auditoria, não classificação do ato: a batida do colaborador também ocorre em request. (`app/core/observ.py:100`, `app/core/observ.py:110`, `app/ponto/views.py:275`)

**Trilha existente reutilizável:** `_trilha_da_recusa` grava `nascimento_recusado` sobre `CelulaDia`, deduplicado por célula + módulo + veredito. Hoje exige `(dia, célula)` e não aceita diretamente motivo de régua, vínculo ausente ou operação de reabertura. (`app/chamados/models.py:862`, `app/chamados/models.py:866`, `app/chamados/models.py:870`, `app/chamados/models.py:873`)

O desenho precisa generalizar esse suporte: recusa automática registra operação/unidade/motivo; ato humano admitido registra autor, motivo do ato e veto apresentado, sem descrevê-lo como nascimento recusado. A resposta da porta precisa carregar o veto até a tela. A infraestrutura atual de `evento` é best-effort; garantia obrigatória de persistência da trilha ainda não existe ali. (`app/chamados/models.py:862`, `app/core/observ.py:94`, `app/core/observ.py:98`) **[nao verificado — desenho]**

As recusas já existentes de reabertura — fechamento administrativo sem fato novo, premissa regularizada, agrupamento, irmão vivo e motor — são outras guardas. Não devem ser confundidas nem removidas silenciosamente pela centralização do veto de régua. (`app/chamados/models.py:1003`, `app/chamados/models.py:1009`, `app/chamados/models.py:1012`, `app/chamados/models.py:1015`, `app/chamados/models.py:1019`)

**3. Destino de cada caminho do censo**

Nesta classificação, **A** = automático: nascimento/reabertura de módulo do universo sofre veto na porta; **H** = ato administrativo explícito: atravessa esse veto, apresenta motivo e registra trilha; **F** = fora do universo. São destinos propostos, **[nao verificado — desenho]**.

| Caminho | Mudança proposta e evidência atual |
|---|---|
| `abrir` — nascimento | **A/H conforme ato declarado**; decisão antes do INSERT. (`app/chamados/models.py:726`, `app/chamados/models.py:730`) |
| `criar` — nascimento | **A/H conforme ato declarado**; decisão antes do INSERT, sem presumir manual pelo nome. (`app/chamados/models.py:805`, `app/chamados/models.py:834`) |
| `abrir` — dedup encerrado | **A/H**; precisa atingir a decisão comum. Hoje retorna desse ramo antes do veto celular de nascimento. (`app/chamados/models.py:685`, `app/chamados/models.py:688`, `app/chamados/models.py:726`) |
| `_renascer_por_premissa_viva` | **A** nos chamadores de renascimento do censo; veto antes de reabrir. (`app/chamados/models.py:742`, `app/chamados/models.py:770`) |
| `reabrir` | **A/H**; acrescentar propagação explícita do ato hoje ausente. (`app/chamados/models.py:1023`) |
| `transicionar` para vivo | **H** nos formulários de gestor/DP do censo; mesma decisão, com aviso e trilha. Hoje passa por `_aplicar`, sem passar por `reabrir`. (`app/chamados/models.py:183`, `app/chamados/models.py:260`) |
| Detector `_abrir_chamado` | **A**; deixa de decidir régua por fora — L-111. Demais guardas permanecem identificadas como outras perguntas. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:604`, `app/ponto/management/commands/detectar_ausencias.py:688`) |
| `emitir_furo_retroativo` | **A**; herda a centralização do detector. (`app/ponto/management/commands/emitir_furo_retroativo.py:38`, `app/ponto/management/commands/emitir_furo_retroativo.py:142`) |
| Grade `_emitir_furo` | **A**; retira decisões externas de régua; a tentativa adicional de `reabrir` também fica coberta. (`app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:491`, `app/ponto/management/commands/reconciliar_grade.py:499`, `app/ponto/management/commands/reconciliar_grade.py:520`) |
| Cartório — integral, cobrança morta e limbo de hoje | **A**, salvo ato admin explicitamente propagado; emissão delega ao detector. A leitura de veto para impressão continua sendo leitura. (`app/ponto/services/cartorio.py:1083`, `app/ponto/services/cartorio.py:1358`) |
| Cartório — furo parcial | Mesmo destino; delega ao detector. (`app/ponto/services/cartorio.py:1394`) |
| C2 `reemitir_chamado_do_dia` | **A**; elimina o retorno antecipado por régua — L-111. Costura em chamado já vivo continua fora do evento de nascimento/reabertura. (`app/chamados/reconciliador.py:805`, `app/chamados/reconciliador.py:852`, `app/chamados/reconciliador.py:855`, `app/chamados/reconciliador.py:862`) |
| Alerta crítico de turno | **A**; passa a encontrar o veto na porta. (`app/ponto/management/commands/processar_alertas_turno.py:316`, `app/ponto/management/commands/processar_alertas_turno.py:335`) |
| Alerta avançado `turno_aberto_24h` | **A**, embora use `criar`; coberto pela união de módulos proposta. (`app/ponto/management/commands/processar_alertas_avancados.py:121`) |
| Retroativo `turno_aberto_24h` | **A**; dry pode consultar informativamente, escrita decide na porta. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:134`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:146`) |
| `bater_ponto` → entrada bloqueada | **A**: reação automática à batida do colaborador, não override admin. Falta resolver a unidade: contexto contém resumo de vários pendentes e total, sem dia estruturado. (`app/ponto/views.py:275`, `app/ponto/views.py:280`, `app/ponto/views.py:291`, `app/ponto/views.py:301`) |
| View de abertura → `abrir_chamado_local` | Abertura humana existe, mas override **admin** não está demonstrado: serviço não 

[truncado em 20000 de 38206 caracteres]

## 30/10 08:24 · sessao d13ad87f9021 · - · - · origem=Ronald (tmux)

Censo confirmado na cópia; classificação abaixo é **desenho proposto**, baseada no chamador e na Lei fornecida, não comportamento já implementado. A distinção de canal ainda não chega a `abrir`, `criar`, `reabrir` ou `_renascer_por_premissa_viva`: assinaturas não recebem ato/autor; `transicionar` recebe `user`. (`app/chamados/models.py:599`, `app/chamados/models.py:805`, `app/chamados/models.py:1023`, `app/chamados/models.py:742`, `app/chamados/models.py:122`)

| Caminho do censo | Efeito pelo desenho |
|---|---|
| `abrir` nascimento | Porta deve barrar emissão automática vetada antes de `get_or_create`; hoje há escrita em `models.py:730`. (`app/chamados/models.py:726`, `app/chamados/models.py:730`) |
| `criar` nascimento | Mesma política; nome `criar` não prova ato humano, pois alerta avançado também chama essa função. (`app/chamados/models.py:805`, `app/chamados/models.py:834`, `app/ponto/management/commands/processar_alertas_avancados.py:121`) |
| `abrir` dedup encerrado | Deve passar pela mesma decisão de renascimento automático; ramo retorna antes do veto celular atual. (`app/chamados/models.py:685`, `app/chamados/models.py:686`, `app/chamados/models.py:688`, `app/chamados/models.py:726`) |
| `_renascer_por_premissa_viva` | Automático; aplicar veto na porta compartilhada antes da reabertura. (`app/chamados/models.py:742`, `app/chamados/models.py:770`) |
| `reabrir` | Mistura admin/sistema hoje; precisa canal explícito, pois a assinatura só tem `marcos`. (`app/chamados/models.py:1023`, `app/chamados/models.py:1024`) |
| `transicionar` para vivo | Precisa mesma política com canal humano comprovado; hoje segue direto `_aplicar → _gravar_estado`, sem guardas de `reabrir`. (`app/chamados/models.py:122`, `app/chamados/models.py:183`, `app/chamados/models.py:260`) |
| Detector `_abrir_chamado` | Automático: passa a barrar na porta, deixa de executar veto de régua por fora; preservar demais filtros de emissão. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:604`, `app/ponto/management/commands/detectar_ausencias.py:688`) |
| `emitir_furo_retroativo` | Herda mudança do detector; executar command não caracteriza override administrativo por chamado. (`app/ponto/management/commands/emitir_furo_retroativo.py:38`, `app/ponto/management/commands/emitir_furo_retroativo.py:142`) |
| Grade `_emitir_furo` | Automático: retirar perguntas de régua externas e cobrir tanto `abrir` quanto `ch.reabrir` posterior. (`app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:491`, `app/ponto/management/commands/reconciliar_grade.py:499`, `app/ponto/management/commands/reconciliar_grade.py:520`) |
| Cartório integral/cobrança morta/limbo | Emissão delegada automática passa à política da porta; consulta para impressão pode continuar como leitura, sem decidir recusa por fora. (`app/ponto/services/cartorio.py:1083`, `app/ponto/services/cartorio.py:1358`) |
| Cartório parcial | Idem; mesma chamada do detector. (`app/ponto/services/cartorio.py:1394`) |
| C2 `reemitir_chamado_do_dia` | Retirar retorno antecipado pelo veto; nascimento/renascimento automático fica na porta. Costurar num vivo não é nascimento. (`app/chamados/reconciliador.py:850`, `app/chamados/reconciliador.py:852`, `app/chamados/reconciliador.py:854`, `app/chamados/reconciliador.py:855`, `app/chamados/reconciliador.py:862`) |
| Alerta crítico de turno | Automático, passa a ser barrado na porta. (`app/ponto/management/commands/processar_alertas_turno.py:316`, `app/ponto/management/commands/processar_alertas_turno.py:335`) |
| Alerta avançado 24h | Automático apesar de usar `criar`; só coberto se 24h integrar universo decidido. (`app/ponto/management/commands/processar_alertas_avancados.py:121`, `app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/catalogo/modulos.py:109`) |
| Retroativo turno 24h | Mesma ressalva de universo; consulta dry pode continuar informativa, decisão real na porta. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:134`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:146`) |
| `bater_ponto` entrada bloqueada | **Emissão automática em resposta à batida do colaborador**, não ato admin: barrar na porta se vetado. Contexto não identifica dia, apenas `pendentes`/`total`; contrato deve resolver isso. (`app/ponto/views.py:275`, `app/ponto/views.py:280`, `app/ponto/views.py:291`, `app/ponto/views.py:301`) |
| Abertura local genérica | **Não presumir admin**: serviço não recebe autor e view usa colaborador vinculado; aceita qualquer módulo em `MOTIVOS`, inclusive furo. Override admin aqui é [nao verificado]. (`app/chamados/services/abertura.py:19`, `app/chamados/views.py:170`, `app/chamados/views.py:174`, `app/chamados/views.py:192`, `app/chamados/views.py:197`) |
| Resposta tardia | Resposta de **colaborador**; renascimento é reação automática condicionada à célula, portanto não override admin. (`app/chamados/views.py:1041`, `app/chamados/services/resposta_tardia.py:34`, `app/chamados/services/resposta_tardia.py:45`) |
| Resposta livre, novo status | Ato gestor explícito: veto informa+trilha, passa; já leva `request.user` ao modelo. (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/acoes_chamado.py:315`) |
| Fio, novo status | Ato DP com autorização por chamado: informar+trilhar, passar; já leva `request.user`. (`app/chamados/services/fio.py:702`, `app/chamados/services/fio.py:708`, `app/chamados/services/fio.py:715`) |
| Desarquivar | Ato humano autorizado: informar+trilhar, passar. Hoje perde usuário em `ch.reabrir()`, embora usuário exista na trilha de arquivo/nota. (`app/chamados/views_arquivar.py:54`, `app/chamados/views_arquivar.py:60`, `app/chamados/views_arquivar.py:68`, `app/chamados/views_arquivar.py:75`, `app/chamados/views_arquivar.py:79`) |
| Reabrir competência | Wrapper recebe `por`+`motivo`; serviço recebe `autor`, mas perde-o no `ch.reabrir()`. Definir explicitamente se efeito em lote é ato humano delegado ou emissão automática; não inferir apenas por `autor` opcional, que cai em usuário sistema. (`app/ponto/services/fechamento.py:1400`, `app/ponto/services/fechamento.py:1416`, `app/chamados/services/competencia_trancada.py:228`, `app/chamados/services/competencia_trancada.py:239`, `app/chamados/services/competencia_trancada.py:258`) |
| Reabrir disputa admin | É ato admin explícito, exige motivo/justificativa; pai é usualmente container, fora universo; se chamado selecionado for turno, informar+trilhar e passar pelo canal admin. (`app/chamados/services/acoes_disputa.py:242`, `app/chamados/services/acoes_disputa.py:245`, `app/chamados/services/acoes_disputa.py:250`, `app/chamados/services/acoes_disputa.py:266`, `app/chamados/services/acoes_disputa.py:270`) |
| Resolver perguntas → pai | Reação automática; pais `disputa_supervisao*` ficam fora do veto de turno, não bloquear container por veto de um dia. (`app/chamados/services/disputa_emissao.py:1599`, `app/chamados/services/disputa_emissao.py:1606`, `app/chamados/catalogo/modulos.py:144`, `app/chamados/catalogo/modulos.py:146`) |
| Command órfãs enterradas | Reabertura sistema do pai; recusa antecipada é `recusa_da_reabertura`, não pergunta externa ao juiz da régua. Pai fora universo usual; se turno, canal automático. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:154`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`) |
| Migration 0038 reverso | Bypass histórico de escritor; é vivo→vivo (`registrado→aberto`), não nascimento/reabertura encerrado→vivo. Não mudar migration pelo desenho. (`app/chamados/migrations/0038_a3_registrado_backfill.py:15`, `app/chamados/catalogo/motor.py:24`) |
| Núcleo disputa / agrupador órfãos / âncora | Pais fora universo do veto; não transferir veto de furo ao pai. Se gerar c2 depois, somente nascimento/renascimento do filho é barrado. (`app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:507`, `app/chamados/services/disputa_emissao.py:908`, `app/chamados/services/disputa_emissao.py:911`, `app/chamados/services/disputa_emissao.py:1128`, `app/chamados/services/disputa_emissao.py:1131`, `app/chamados/services/disputa_emissao.py:409`) |
| Abertura DP para colaborador | Humano explícito com autor; catálogo atual não declara módulos estritos como abríveis por esse formulário, mas permite container manual. Portanto não inventar que já há abertura admin de turno por essa porta. (`app/chamados/services/abertura.py:122`, `app/chamados/services/abertura.py:149`, `app/chamados/services/abertura.py:172`, `app/chamados/services/abertura.py:188`, `app/chamados/catalogo/modulos.py:105`, `app/chamados/catalogo/modulos.py:146`) |

**Wrappers do censo:** herdam o caminho final; presença de HTTP, signal ou command não muda por si só canal para admin. Exceção comprovada é botão `Cobrar este dia`: serviço se define manual, recebe autor/request, chama cartório `toque=True`, grava trilha manual e hoje retorna recusa quando `out.vetados`. Pela Lei nova, precisa propagar canal admin até a porta; apenas mover veto para o modelo mantendo esse wrapper sem canal ainda barraria o clique humano. (`app/chamados/services/cobrar_dia.py:1`, `app/chamados/services/cobrar_dia.py:16`, `app/chamados/services/cobrar_dia.py:29`, `app/chamados/services/cobrar_dia.py:37`, `app/chamados/services/cobrar_dia.py:54`, `app/chamados/views_cobrar.py:325`)

- Cartório command, célula, vigia de hora, passivo, signals insumos/chamado, regeneração de vínculo, pós-vínculo e régua corrigida: mesma porta automática por detector; signal do chamado acontece após escrita. (`app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1560`, `app/ponto/services/vigia_de_hora.py:130`, `app/ponto/services/furos_sem_cobranca.py:128`, `app/ponto/signals.py:93`, `app/chamados/signals.py:180`, `app/ponto/portas/celula.py:372`, `app/colaboradores/services/pos_vinculo.py:72`, `app/escala/regua_defesa.py:692`)
- Commands de rejulgamento e scripts: mesma decisão automática do cartório, sem override admin explícito por chamado. (`app/ponto/management/commands/celula_veredito_velho.py:144`, `app/ponto/management/commands/curar_residuo_bug115.py:143`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:71`, `bin/rejulgar_o118_col221.py:24`, `bin/r5_idempotencia_frota.py:109`)
- Contexto c2, reconciliação command/signal, fábrica no julgamento e disparo competência: nascimentos do filho por c2 seguem porta automática; pai continua fora universo. (`app/chamados/reconciliador.py:671`, `app/chamados/reconciliador.py:1518`, `app/chamados/services/disputa_emissao.py:409`, `app/ponto/services/cartorio.py:770`, `app/chamados/services/fabrica_da_celula.py:122`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`)
- API ambos kernels, triagem saída sem entrada e foto retroativa: primeiro container fora universo; emissão c2 posterior segue política automática. (`app/api/views.py:778`, `app/api/views_core.py:778`, `app/ponto/services/triagem_batida.py:651`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`, `app/chamados/services/disputa_emissao.py:598`, `app/chamados/services/disputa_emissao.py:606`)
- Declaração de horário: chamada do núcleo gera pai fora universo; não presumir override admin para todo efeito derivado da declaração. (`app/chamados/services/declaracao_texto.py:307`, `app/chamados/services/declaracao_texto.py:605`, `app/chamados/services/disputa_emissao.py:563`, `app/chamados/services/disputa_emissao.py:504`)
- Outros chamadores agrupador/núcleo: mesmo pai fora universo; nenhum motivo para repetir veto por dia no container. (`app/chamados/services/acoes_disputa.py:221`, `app/chamados/services/pergunta_no_app.py:159`, `app/chamados/services/cobrar_dia.py:44`, `app/ponto/management/commands/processar_alertas_turno.py:384`, `app/ponto/management/commands/processar_alertas_avancados.py:158`, `app/ponto/management/commands/detectar_ausencias.py:791`, `app/ponto/management/commands/reconciliar_grade.py:416`, `app/chamados/management/commands/smoke_chamados.py:99`)

Alerta de desenho: não usar `criar` como sinônimo de manual, `abrir` como sinônimo de automático ou usuário presente como sinônimo de override; os exemplos acima contradizem essas três heurísticas. (`app/ponto/management/commands/processar_alertas_avancados.py:121`, `app/chamados/services/abertura.py:47`, `app/chamados/services/competencia_trancada.py:239`)

## 30/10 08:23 · sessao cfff94ef1c2e · - · - · origem=Ronald (tmux)

Referências abaixo relativas à raiz da cópia; nenhuma consulta a banco/log foi feita e nenhum teste foi executado. [nao verificado] [nao verificado]

**Contrato e unidade**

- A leitura existente é `motivo_do_veto(colab, ec, data)`: retorna `VETO_DEGENERADA`, `VETO_REGUA` ou `None`; usa `regua_degenerada(ec.tipo_escala, data)` primeiro e `vetar_cobranca_se_regua_suspeita(colab, ec)` depois. (`app/ponto/services/furos_vetados.py:19`, `app/ponto/services/furos_vetados.py:25`, `app/ponto/services/furos_vetados.py:28`, `app/ponto/services/furos_vetados.py:30`)
- A versão emissora para degeneração é `vetar_cobranca_se_degenerada(colab, ec, data)`: usa o mesmo `regua_degenerada`, mas transforma vínculo ausente em template ausente e abre um chamado de cadastro `regua_degenerada`. (`app/escala/regua_defesa.py:36`, `app/escala/regua_defesa.py:45`, `app/escala/regua_defesa.py:51`, `app/escala/regua_defesa.py:53`)
- A unidade do juízo é **colaborador + dia + vínculo que responde por esse dia**; o módulo não entra nesses predicados. `vinculo_do_emissor` delega a `escala_vigente`, que carrega e pergunta ao `escala.alimentacao.vinculo_do_dia`; célula/geradora do dia tem precedência sobre a linha do tempo. (`app/ponto/services/furos_vetados.py:10`, `app/ponto/services/furos_vetados.py:15`, `app/escala/servico_jornada.py:15`, `app/escala/servico_jornada.py:29`, `app/escala/servico_jornada.py:38`, `app/escala/servico_jornada.py:55`)
- Não confundir unidade do juízo com dedup: detector e grade deduplicam **colaborador + módulo + data_turno**, sem vínculo na chave. (`app/ponto/management/commands/detectar_ausencias.py:688`, `app/ponto/management/commands/detectar_ausencias.py:691`, `app/ponto/management/commands/reconciliar_grade.py:499`, `app/ponto/management/commands/reconciliar_grade.py:501`)
- A constante estrita `FURO_MODULES` contém `batida_ausente`, `turno_aberto`, `intervalo_pendente`, `entrada_bloqueada_turno_aberto`; `MODULOS_DE_TURNO` contém `turno_aberto`, **`turno_aberto_24h`**, `entrada_bloqueada_turno_aberto`. São universos diferentes; o contrato da porta precisa explicitar se usa a união, evitando perder `24h` ou perder `batida_ausente`/`intervalo_pendente`. (`app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/reconciliador.py:336`)
- `motivo_do_veto` não filtra módulo, situação, isenção, afastamento, célula, feriado nem posto: sua função contém somente as duas provas de régua. Esses filtros não podem ser atribuídos ao juiz de régua por inferência. (`app/ponto/services/furos_vetados.py:19`, `app/ponto/services/furos_vetados.py:25`)
- A exclusão de desligado existente em `furos_vetados_por_regua` é do leitor, não do predicado; o leitor mantém afastados deliberadamente. (`app/ponto/services/furos_vetados.py:39`, `app/ponto/services/furos_vetados.py:49`)

**Diferenças de predicado/comportamento nas funções pedidas**

| Diferença | `motivo_do_veto` hoje | Detector `_abrir_chamado` hoje | Grade `_emitir_furo` hoje |
|---|---|---|---|
| `ec=None` | Retorna `None` imediatamente. (`app/ponto/services/furos_vetados.py:26`) | Chama degenerada; `te=None` é degenerada, abre cadastro e retorna `VETO_DEGENERADA`. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/escala/regua_defesa.py:23`, `app/escala/regua_defesa.py:45`, `app/escala/regua_defesa.py:51`) | Mesmo comportamento do detector; `ec` inclusive tem default `None`. (`app/ponto/management/commands/reconciliar_grade.py:480`, `app/ponto/management/commands/reconciliar_grade.py:487`) |
| Degeneração com `ec` presente | Consulta pura `regua_degenerada`. (`app/ponto/services/furos_vetados.py:28`) | Consulta versão com efeito de cadastro; mesmo predicado estrutural. (`app/ponto/management/commands/detectar_ausencias.py:599`, `app/escala/regua_defesa.py:46`) | Igual detector. (`app/ponto/management/commands/reconciliar_grade.py:486`) |
| Efeito lateral | Não abre cadastro. (`app/ponto/services/furos_vetados.py:23`) | Degenerada abre/deduplica cadastro por colaborador + template, independente do dia; contexto guarda apenas `data_exemplo`. (`app/escala/regua_defesa.py:41`, `app/escala/regua_defesa.py:51`, `app/escala/regua_defesa.py:54`, `app/escala/regua_defesa.py:76`) | Igual detector. (`app/ponto/management/commands/reconciliar_grade.py:487`) |
| Régua suspeita | Mesmo `vetar_cobranca_se_regua_suspeita`, após degeneração. (`app/ponto/services/furos_vetados.py:30`) | Mesmo predicado e precedência. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:604`) | Mesmo predicado e precedência. (`app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:491`) |
| Feriado do dia | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | `fatos_do_dia(...).faixa == 'feriado'` retorna `VETO_FERIADO`. (`app/ponto/management/commands/detectar_ausencias.py:615`) | Não consulta dentro de `_emitir_furo`. (`app/ponto/management/commands/reconciliar_grade.py:480`) |
| Fechamento do posto em feriado | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | `not cobra_no_dia(...)` retorna `VETO_FERIADO_DO_POSTO`. (`app/ponto/management/commands/detectar_ausencias.py:622`) | Não consulta dentro de `_emitir_furo`. (`app/ponto/management/commands/reconciliar_grade.py:480`) |
| Veto da célula | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | Consulta `celula_veta_cobranca`, retorna `VETO_CELULA_<motivo>`. (`app/ponto/management/commands/detectar_ausencias.py:625`) | Igual detector. (`app/ponto/management/commands/reconciliar_grade.py:494`) |
| Ausência de célula | Não consulta. (`app/ponto/services/furos_vetados.py:25`) | Sem célula e fora de limbo: abre aviso interno `_chamado_dia_sem_grade` e retorna `VETO_SEM_CELULA`. Limbo não cai nessa guarda. (`app/ponto/management/commands/detectar_ausencias.py:651`, `app/ponto/management/commands/detectar_ausencias.py:654`, `app/ponto/management/commands/detectar_ausencias.py:662`) | `celula_veta_cobranca` não veta célula ausente; não há guarda adicional. (`app/ponto/marcos.py:156`, `app/ponto/management/commands/reconciliar_grade.py:494`, `app/ponto/management/commands/reconciliar_grade.py:499`) |
| Recusa de renascimento | Não trata estado de chamado. (`app/ponto/services/furos_vetados.py:19`) | Após `abrir`, existente que continua fora de `VIVOS` retorna `VETO_ENCERRADO_NAO_RENASCEU`; não segue para push/fio. (`app/ponto/management/commands/detectar_ausencias.py:729`) | Após `abrir`, se `ENCERRADOS`, chama `reabrir()` outra vez, ignora resultado e retorna `None`. (`app/ponto/management/commands/reconciliar_grade.py:519`) |
| Limbo depois de nascer | Não trata. (`app/ponto/services/furos_vetados.py:19`) | Pode agrupar furo sob pai limbo e retornar `AGRUPADO_LIMBO_FOLGAS`; ou gravar ressalva conforme `pode_calar`. (`app/ponto/management/commands/detectar_ausencias.py:737`, `app/ponto/management/commands/detectar_ausencias.py:748`, `app/ponto/management/commands/detectar_ausencias.py:751`, `app/ponto/management/commands/detectar_ausencias.py:755`) | Não tem esse tratamento em `_emitir_furo`. (`app/ponto/management/commands/reconciliar_grade.py:499`, `app/ponto/management/commands/reconciliar_grade.py:521`) |

Detalhes dos predicados já existentes:

- `regua_degenerada` considera template ausente, exceção ao obter marcos, início/fim ausentes ou início igual ao fim. Não usa os marcos congelados da célula: pergunta diretamente `tipo_escala.marcos_do_dia(data)`. (`app/escala/regua_defesa.py:23`, `app/escala/regua_defesa.py:26`, `app/escala/regua_defesa.py:27`, `app/escala/regua_defesa.py:29`, `app/escala/regua_defesa.py:31`)
- Suspeita não equivale simplesmente a classe suspeita nem a cadastro vivo: vínculo ausente/nunca avaliado não veta; precisa faixa `<40` e `regua_dias_baixo >=2`. A função recebe `colab`, mas seu corpo decide só pelo vínculo. (`app/escala/regua_defesa.py:554`, `app/escala/regua_defesa.py:560`, `app/escala/regua_defesa.py:599`, `app/escala/regua_defesa.py:644`)
- Corroboração humana existente grava escore zero, dois dias baixos e avaliação corrente, ligando aquele mesmo predicado sem juiz adicional. (`app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:666`)
- `celula_veta_cobranca` veta `trabalha is False` ou veredito em `nunca_bateu`, `trabalhou`, `justificado`, `coberta`, `cobranca_indevida`; sem célula não veta e `trabalha=None` não é folga. (`app/ponto/marcos.py:119`, `app/ponto/marcos.py:151`, `app/ponto/marcos.py:156`, `app/ponto/marcos.py:159`, `app/ponto/marcos.py:161`)

**Limites importantes para desenho L-111**

- Retirar consultas antecipadas de **régua** do detector/grade exige preservar o efeito de cadastro dentro da execução automática do juiz composto, ou declarar explicitamente sua supressão; trocar tudo simplesmente por `motivo_do_veto` altera `ec=None` e elimina o efeito de cadastro. (`app/ponto/services/furos_vetados.py:26`, `app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/reconciliar_grade.py:487`, `app/escala/regua_defesa.py:51`)
- Não há base no código para transportar automaticamente feriado, posto, célula ausente e limbo para o juiz de régua: são guardas diferentes, em funções/fontes diferentes. (`app/ponto/management/commands/detectar_ausencias.py:615`, `app/ponto/management/commands/detectar_ausencias.py:622`, `app/ponto/management/commands/detectar_ausencias.py:651`)
- A grade tem ainda guardas anteriores, no `handle`, de cobertura, isenção e `classificar_falta(...)[1] is None`; não pertencem ao corpo de `_emitir_furo`. A seleção do vínculo do dia também já acontece no chamador via `vinculo_do_dia`. (`app/ponto/management/commands/reconciliar_grade.py:196`, `app/ponto/management/commands/reconciliar_grade.py:216`, `app/ponto/management/commands/reconciliar_grade.py:230`, `app/ponto/management/commands/reconciliar_grade.py:243`, `app/ponto/management/commands/reconciliar_grade.py:254`)
- Os dois emissores são de `batida_ausente`; não recebem o veto compartilhado `_celula_que_absolve`, cujo filtro é só `MODULOS_DE_TURNO`. (`app/ponto/management/commands/reconciliar_grade.py:54`, `app/ponto/management/commands/detectar_ausencias.py:690`, `app/chamados/models.py:849`, `app/chamados/reconciliador.py:336`)
- A régua degenerada abre módulo de **cadastro**, fora tanto de `FURO_MODULES` quanto de `MODULOS_DE_TURNO`; o filtro de universo precisa excluí-lo para o veto não barrar sua própria consequência/reentrar. (`app/escala/regua_defesa.py:53`, `app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/reconciliador.py:336`)

## 30/10 08:23 · sessao 5dacd1f8e7da · - · - · origem=Ronald (tmux)

Selos atuais:

- `test_contract_emissao_gate.py` usa AST, mas detecta apenas `*.objects.create(..., status_local=...)`; isenta todo `chamados/models.py` e pula migrations. Não cobre criação com estado padrão, `get_or_create`, lotes nem nascimento por instância+`save`. (`app/chamados/tests/test_contract_emissao_gate.py:14`, `:21`, `:40`, `:44`)
- O selo mais forte é `test_chokepoint_chamado_gate`: usa o censo de escrita e exige **zero fora das portas**, teto vazio e allowlist vazia. Porém sua unidade de autorização é o **arquivo**, não a função: qualquer escrita em módulo listado como porta é aceita. (`app/chamados/tests/test_chokepoint_chamado_gate.py:323`, `:350`, `:371`, `:376`)
- `ChamadoColaborador` autoriza vários módulos, incluindo todo `chamados/models.py`; portanto o selo atual não prova que nascimento/reabertura passou pela mesma guarda. (`app/core/portas.py:195`, `:216`, `:233`)
- O censo reconhece construção, `create/get_or_create/update_or_create/bulk_create`, `update/bulk_update`, `save` e gerentes relacionados; resolve imports, mas pula migrations. (`app/core/censo_escritas.py:8`, `:15`, `:20`, `:61`, `:66`, `:458`)
- `test_contract_status_nucleo` é regex, não AST: permite a linha `self.status_local = novo`, detecta atribuição e `update(status_local=...)`, pula migrations. (`app/chamados/tests/test_contract_status_nucleo.py:7`, `:9`, `:16`, `:23`)
- O contrato de dedup exige textualmente que `abrir` distinga `ENCERRADOS` e chame `_renascer_por_premissa_viva`; exige também que renascimento delegue a `reabrir`. Não verifica o veto de régua. (`app/chamados/tests/test_contract_dedup_status.py:28`, `:37`, `:41`, `:54`)

Para morder emissão por fora, o desenho precisa restringir **funções/atos dentro dos módulos autorizados**, além do selo atual de arquivo; provar que todo nascimento e `ENCERRADOS → VIVOS` passa pela guarda antes da escrita; e conter casos que violem deliberadamente por `create` sem `status_local`, `get_or_create`, instância+`save`, `update/bulk_update` e escrita dentro de `models.py` fora da função autorizada. Isso é requisito proposto, ainda inexistente. [nao verificado]

Contadores:

| Contador | O que responde hoje |
|---|---|
| `furos_sem_cobranca.medir` | **Não responde vivo em dia vetado**: monta conjunto de vivos de qualquer módulo e exclui o dia antes de consultar o veto; `veto` fica fora do total. (`app/ponto/services/furos_sem_cobranca.py:68`, `:74`, `:83`, `:96`, `:109`) |
| `furos_vetados_por_regua` | Lista células `veredito='furo'`, até hoje, exceto desligados, cujo juiz veta; não cruza com chamado vivo. (`app/ponto/services/furos_vetados.py:49`, `:52`, `:56`) |
| `vigia_fio.censo` | Parte de chamados vivos **com célula**, recorta `FURO_MODULES`, lê células acusadoras e mede fio/pergunta/decisão pendente; não consulta veto de régua. (`app/chamados/services/vigia_fio.py:186`, `:201`, `:228`, `:260`, `:268`, `:288`) |
| `vigia_carimbo` | Perguntas de hora já validadas sem materialização de fato; não conta chamado vivo em dia vetado. (`app/chamados/services/vigia_carimbo.py:97`, `:105`, `:119`) |
| `vigia_loop` | Conta pares fecha/reabre sem fato novo, escritas por cron e divergência da última trilha contra banco; não cruza com veto. (`app/chamados/services/vigia_loop.py:28`, `:45`, `:50`, `:86`, `:114`) |

Nenhum dos contadores examinados responde exatamente “chamado de turno vivo em dia vetado”. As exclusões e predicados acima demonstram os limites desses candidatos; ausência de qualquer outro contador na árvore inteira permanece [nao verificado].

## 30/10 08:18 · sessao 9bf5c72ae18e · O261-V · sonda · origem=Code

Pela sonda fornecida, sem verificação independente:

- **451 aparece em `fora_de_fase`**: template `179`; âncora `2026-10-29`, origem `declarada`; `trabalho_sem_batida=6`, `folga_com_turno=6`. **Sem âncora sugerida**: `{'ancora': None, 'motivo': 'ganho de 0 pp e ruido (minimo 40)', 'batidas': 27, 'casa_hoje': 52}`. [nao verificado]
- **Contador da frota**: `{'fonte': 'fase_12x36', 'n': 11, 'janela_dias': 14, 'piso': 3, 'universo': 341}`, com `n_vinculos=11`. [nao verificado]
- **451 consta em Cadastro x Realidade**: `CXR_COL451_paths=['/postos[116][1][1]/colab_id']`. A assinatura e o motivo dessa inclusão não aparecem no dado fornecido. [nao verificado]

Chamadores diretos de `fora_de_fase` no código de produção da cópia:

| Chamador | Local da chamada |
|---|---|
| `api_mensageria_fase_12x36` | `app/api/views_mensageria.py:1670` |
| `particionar` | `app/chamados/services/fila_por_causa.py:173` |
| `propostas` | `app/escala/services/propositor.py:456` |
| `linhas_para_a_pauta` | `app/escala/services/fase_12x36.py:215` |
| `Command.handle` de `vigiar_familia_escala` | `app/ponto/management/commands/vigiar_familia_escala.py:23` |

**Nenhum desses fornece a saída de `fora_de_fase` como entrada de Cadastro x Realidade.** `lista` reúne as entradas de cadastro em `cad`, lê a lavra do esmeril e monta os IDs pela união de `por_colab` com `cad` (`app/escala/services/cadastro_realidade.py:513`, `:518`, `:527`, `:535`, `:542`, `:549`, `:556`, `:563`, `:570`, `:575`, `:579`).

O destino sugerido dessa lista vem de `d['destino']` da lavra (`app/escala/services/cadastro_realidade.py:581`, `:590`), produzido por **`escala.detector_proposta.propor_escala`** (`app/ponto/services/esmeril_espelho.py:213`, `:223`); o chamador de `fora_de_fase` é **`escala.services.propositor.propostas`** (`app/escala/services/propositor.py:456`).

## 30/10 08:16 · sessao 9dc80579edf4 · O261-NOTURNO · sonda · origem=Code

Transcrição de `NOT|`, por motivo; “—” indica linha ausente no dado, sem assumir zero. [nao verificado]

| Motivo | instante_outro_dia | mesmo_dia | sem_instante | Soma das parcelas exibidas | Fonte |
|---|---:|---:|---:|---|---|
| intervalo_saida | 776 | 3443 | 3514 | 776 + 3443 + 3514 = **7733** | [nao verificado] |
| intervalo_volta | 1460 | 3261 | 3623 | 1460 + 3261 + 3623 = **8344** | [nao verificado] |
| orfao_14h | 2516 | 4653 | 6668 | 2516 + 4653 + 6668 = **13837** | [nao verificado] |
| saida_sem_entrada | — | 4983 | 4785 | 4983 + 4785 = **9768** | [nao verificado] |

Demais linhas `NOT|`, todas subordinadas a `instante_outro_dia`: [nao verificado]

| Campo | intervalo_saida | intervalo_volta | orfao_14h | Fonte |
|---|---:|---:|---:|---|
| data_evento==celula=True | 776 | 1460 | 2516 | [nao verificado] |
| plantio_no_dia_celula | 9 | 4 | 13 | [nao verificado] |
| plantio_no_dia_instante | 40 | 78 | 153 | [nao verificado] |
| texto_mostra_dia_da_celula=True | 776 | 1460 | 2516 | [nao verificado] |
| viva | 343 | 413 | 472 | [nao verificado] |

| Linha | Valores | Fonte |
|---|---|---|
| NOT\|plantios_amostrados | 271; com_batida_real_na_hora_no_dia_da_celula=39; colabs=7 | [nao verificado] |
| ERROS | 0 | [nao verificado] |

As somas calculadas estão acima. O dado não apresenta um total independente de cada motivo para confirmar se as parcelas fecham esse total. [nao verificado]

## 30/10 08:13 · sessao 7bacb9cf86c7 · O261-NOTURNO · sonda · origem=Code

**1. Nas quatro ATAs anexadas, E 19:00 está apagada.** Não há batida associada a esse marco: `luz=None`, `tipo_real=None`. O anexo não fornece instante de uma batida acendedora de E 19:00. [nao verificado]

| Célula | Dia | E 19:00 | Batida associada na ATA |
|---|---|---|---|
| 129745 | 21/10/2026 | Apagada | Nenhuma (`luz=None`) [nao verificado] |
| 129747 | 23/10/2026 | Apagada | Nenhuma (`luz=None`) [nao verificado] |
| 129749 | 25/10/2026 | Apagada | Nenhuma (`luz=None`) [nao verificado] |
| 129751 | 27/10/2026 | Apagada | Nenhuma (`luz=None`) [nao verificado] |

Isso concorda com a cobrança descrita de **“E 19:00 faltante”** e discorda de considerar esse mesmo marco cumprido para encerrar `saida_sem_entrada` como `fantasma`. Os registros desses chamados e perguntas, seus vínculos e horários de encerramento não estão na sonda; essa associação histórica individual permanece [nao verificado].

Na cópia, as leituras são estas:

- **Cobrança do furo:** `marcos_apagados` lê as lâmpadas apagadas da ATA; o cartório usa esse resultado para formar os marcos faltantes. (`app/ponto/services/lastro.py:118`; `app/ponto/services/cartorio.py:1429`)
- **Juiz atual da pergunta:** `saida_sem_entrada` cobra `('E', 'hi')`; procura a hora no DNA e lê `acesa` na ATA por `(hora, tipo)`. (`app/chamados/juizes.py:823`; `app/chamados/juizes.py:848`; `app/chamados/juizes.py:866`)
- **Chamador atual do encerramento:** exige que esse juiz devolva exatamente `True`; depois, só chama `encerrar_fantasma` quando o veredito é `ENCERRA`. Assim, uma lâmpada E19 apagada não passa por essa condição. (`app/chamados/reconciliador.py:1032`; `app/chamados/reconciliador.py:1079`; `app/chamados/reconciliador.py:1092`)
- **Escritor:** `encerrar_fantasma` grava via, instante do carimbo e autor. Seu corpo não consulta ATA nem batidas; `materializada_em=now()` é o instante do encerramento, não uma batida que acendeu o marco. (`app/chamados/models.py:2167`)

Portanto, **não é demonstrável que o chamador atual tenha produzido os encerramentos históricos mencionados**: faltam os respectivos registros e a ATA existente no instante de cada encerramento. [nao verificado]

**2. O predicado antigo preservado na lápide era morfológico:** turno recomputado completo **e** última saída a até 180 minutos do fim previsto, com `TOL_MORFOLOGIA_MIN=180`; aparecem os nomes removidos `_eh_fantasma`, `_bem_formado` e `_turnos_na_janela`. (`app/ponto/management/commands/reconciliar_perguntas_orfas.py:10`)

O comentário do reconciliador distingue expressamente os dois critérios: a regra posterior encerra pela lâmpada do próprio marco; a antiga julgava pela morfologia do turno. (`app/chamados/reconciliador.py:982`)

O universo identificado como “mesmo SQL do cron que carimbava” contém perguntas sem materialização, sem validação, em disputa aberta, dos motivos `orfao_14h`/`saida_sem_entrada`, com `data_evento` preenchida. (`app/chamados/reconciliador.py:992`)

**A lápide não preserva como o algoritmo antigo escolhia o dia da batida.** Não permite afirmar D, D+1 ou outro recorte. Tampouco a sonda contém as execuções de aproximadamente 06:35 em 22–28/10; atribuir-lhes o algoritmo completo ou uma batida específica seria [nao verificado].

**3. P26: o ramo efetivamente executado é [nao verificado] para cada uma das 26 linhas.** O carregamento de `_marcos` depende do vínculo e do tipo encontrados naquele dia; esses resultados não constam no anexo. (`app/chamados/juizes.py:298`)

Além disso, existem **dois retornos distintos que podem conservar `ts`**: `_marcos` preenchido, mas hora fora do envelope (`return ts`, linha 312), e caminho sem marcos, cujo retorno final está na linha 321. Logo, plantio em D não prova `_marcos` vazio. (`app/chamados/juizes.py:304`; `app/chamados/juizes.py:312`; `app/chamados/juizes.py:313`; `app/chamados/juizes.py:321`)

A aplicação matemática às **horas respondidas e marcos informados** está abaixo. O código encadeia os marcos atravessando a meia-noite, amplia o primeiro/último em 90 minutos e testa D antes de D+1. (`app/escala/servico_jornada.py:74`; `app/chamados/juizes.py:305`)

Os valores de produção transcritos e o ramo histórico de cada linha permanecem [nao verificado].

| P# | Resposta | Envelope calculado pelos marcos informados | Resultado pela hora respondida |
|---|---|---|---|
| 22818 | 07:00 | D 17:30 → D+1 08:30 | Envelope: D+1 |
| 22970 | 23:45 | D 17:30 → D+1 08:30 | Envelope: D |
| 22993 | 03:41 | D 22:00 → D+1 09:00 | Envelope: D+1 |
| 25281 | 03:01 | D 17:30 → D+1 08:30 | Envelope: D+1 |
| 25282 | 07:01 | D 17:30 → D+1 08:30 | Envelope: D+1 |
| 26108 | 23:30 | D 22:00 → D+1 09:00 | Envelope: D |
| 27227 | 18:55 | D 17:30 → D+1 08:30 | Envelope: D |
| 27290 | 12:00 | D 16:30 → D+1 07:30 | Fora: `return ts` |
| 27670 | 23:16 | D 22:00 → D+1 09:00 | Envelope: D |
| 27739 | 03:00 | D 17:30 → D+1 08:30 | Envelope: D+1 |
| 32357 | 01:00 | D 16:30 → D+1 07:30 | Envelope: D+1 |
| 33283 | 06:00 | D 20:30 → D+1 07:30 | Envelope: D+1 |
| 33629 | 23:14 | D 22:00 → D+1 09:00 | Envelope: D |
| 34397 | 09:15 | D 22:00 → D+1 09:00 | Fora: `return ts` |
| 35366 | 23:11 | D 22:00 → D+1 09:00 | Envelope: D |
| 37847 | 18:57 | D 17:30 → D+1 08:30 | Envelope: D |
| 38265 | 04:00 | D 17:30 → D+1 08:30 | Envelope: D+1 |
| 39186 | 23:57 | D 17:30 → D+1 08:30 | Envelope: D |
| 39761 | 23:59 | D 14:30 → D+1 01:30 | Envelope: D |
| 40675 | 23:56 | D 17:30 → D+1 08:30 | Envelope: D |
| 41549 | 23:50 | D 17:30 → D+1 08:30 | Envelope: D |
| 41707 | 19:00 | D 17:30 → D+1 08:30 | Envelope: D |
| 42368 | 22:41 | D 17:30 → D+1 08:30 | Envelope: D |
| 43845 | 11:59 | D 17:30 → D+1 08:30 | Fora: `return ts` |
| 46364 | 11:00 | D 17:30 → D+1 08:30 | Fora: `return ts` |
| 49873 | 10:11 | D 17:30 → D+1 08:30 | Fora: `return ts` |

Essa tabela não identifica o ramo histórico: nas perguntas de intervalo, o materializador pode substituir a resposta pela hora do marco faltante **antes** de montar `ts` e chamar o ajuste. O anexo não informa essa `_hora_fonte` efetiva. (`app/chamados/services/materializacao.py:388`; `app/chamados/services/materializacao.py:396`; `app/chamados/services/materializacao.py:404`)

Também não cabe deduzir ausência de marcos pela ausência de intervalo: havendo `hi` e `hf`, `marcos_turno` produz ao menos E inicial e S final. (`app/escala/servico_jornada.py:72`)

**4. O texto `pergunta` é formatado na emissão e gravado na criação.** O formatador substitui `{data}` por `contexto_disputa[motivo]['data']`, com `%d/%m/%Y`; a emissão passa `pergunta=texto` para `abrir`. (`app/chamados/catalogo/query.py:58`; `app/chamados/catalogo/query.py:64`; `app/chamados/services/disputa_emissao.py:316`; `app/chamados/services/disputa_emissao.py:389`)

`pergunta` é um `TextField`; `abrir` persiste os campos recebidos em `objects.create(..., **campos)`. Quando encontra pergunta existente, reaponta destino/célula e retorna sem reformatar seu texto. (`app/chamados/models.py:1855`; `app/chamados/models.py:2061`; `app/chamados/models.py:2085`)

As exibições citadas leem `p.pergunta`/`perg.pergunta`. Portanto, a data escrita na pergunta não comprova o dia da batida plantada. (`app/templates/chamados/partials/_card_atendimento.html:93`; `app/templates/chamados/partials/modal_fio.html:157`; `app/chamados/catalogo/query.py:64`)

## 30/10 08:12 · sessao d27e633afa7f · - · - · origem=Ronald (tmux)

Q3: **o ramo efetivamente executado é [não verificado] em todas as 26 linhas**. A sonda fornece DNA, resposta e plantio; não fornece `_ec`, `tipo_escala`, `_marcos`, `tipo`, `hora_tentativa` nem `_hora_fonte` efetivos. O carregamento de `_marcos` depende desses primeiros campos (`app/chamados/juizes.py:298-303`). A geradora pode ser rejeitada por vigência, devolvendo `None`, apesar de existir célula com DNA (`app/escala/alimentacao.py:237-251`).

Além disso, a dicotomia proposta é incompleta: existe `_marcos` **presente**, mas nenhuma data candidata dentro do envelope, seguido de `return ts` (`app/chamados/juizes.py:304-312`). Portanto “plantou em D” não demonstra `_marcos` vazio (`app/chamados/juizes.py:312,321`).

Os marcos são encadeados E-hi / S-hii / E-hfi / S-hf, avançando o dia quando o instante seguinte não é posterior ao anterior (`app/escala/servico_jornada.py:74-85`). O envelope vai do primeiro marco menos 90 min ao último mais 90 min; testa D antes de D+1 (`app/chamados/juizes.py:305-311`).

Cálculo **condicional**, usando os marcos DNA e a **hora da resposta** como entrada do ajuste; isto não identifica o ramo histórico real (`app/chamados/juizes.py:304-312`):

| P# | Resposta | Envelope pelo DNA | Resultado do cálculo |
|---|---|---|---|
|22818|07:00|D 17:30–D+1 08:30|D+1|
|22970|23:45|D 17:30–D+1 08:30|D|
|22993|03:41|D 22:00–D+1 09:00|D+1|
|25281|03:01|D 17:30–D+1 08:30|D+1|
|25282|07:01|D 17:30–D+1 08:30|D+1|
|26108|23:30|D 22:00–D+1 09:00|D|
|27227|18:55|D 17:30–D+1 08:30|D|
|27290|12:00|D 16:30–D+1 07:30|Fora; `return ts`|
|27670|23:16|D 22:00–D+1 09:00|D|
|27739|03:00|D 17:30–D+1 08:30|D+1|
|32357|01:00|D 16:30–D+1 07:30|D+1|
|33283|06:00|D 20:30–D+1 07:30|D+1|
|33629|23:14|D 22:00–D+1 09:00|D|
|34397|09:15|D 22:00–D+1 09:00|Fora; `return ts`|
|35366|23:11|D 22:00–D+1 09:00|D|
|37847|18:57|D 17:30–D+1 08:30|D|
|38265|04:00|D 17:30–D+1 08:30|D+1|
|39186|23:57|D 17:30–D+1 08:30|D|
|39761|23:59|D 14:30–D+1 01:30|D|
|40675|23:56|D 17:30–D+1 08:30|D|
|41549|23:50|D 17:30–D+1 08:30|D|
|41707|19:00|D 17:30–D+1 08:30|D|
|42368|22:41|D 17:30–D+1 08:30|D|
|43845|11:59|D 17:30–D+1 08:30|Fora; `return ts`|
|46364|11:00|D 17:30–D+1 08:30|Fora; `return ts`|
|49873|10:11|D 17:30–D+1 08:30|Fora; `return ts`|

**Atenção à hora de entrada:** `_ts_tipo_do_plantio` começa com a resposta, mas para `intervalo_saida`/`intervalo_volta` substitui-a pelo marco faltante, quando encontrado (`app/chamados/services/materializacao.py:388-395`); monta `ts` em `data_evento` e só depois chama o ajuste (`app/chamados/services/materializacao.py:396-404`). Logo não se pode transformar esta tabela condicional em atribuição histórica de ramo.

Usando **a hora do plantio** no mesmo cálculo, mudariam: P25281 19:00→D; P27739 19:00→D; P32357 19:00→D; P38265 19:00→D; P27670 03:00→D+1. O ajuste preserva a hora e decide apenas data (`app/chamados/juizes.py:307-311,318`).

A fonte do vínculo lê célula e geradora (`app/escala/servico_jornada.py:46-58`). Com vínculo e tipo existentes, `marcos_turno` usa `ec.marcos_do_dia` (`app/escala/servico_jornada.py:68-73`), que prefere DNA preenchido (`app/escala/models.py:1094-1108`). A ausência de `hii/hfi` não esvazia `_marcos` quando `hi/hf` existem; ainda há E-hi e S-hf (`app/escala/servico_jornada.py:72-77`).

_399 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
