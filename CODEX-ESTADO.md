# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 14:38** · publicado em **30/10/2026 14:38** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 14:38 · sessao e11b54d6c877 · - · - · origem=Ronald (tmux)

Nenhum defeito encontrado na guarda do item (2), por inspeção:

- A chamada passa `logger_nome`, `operacao`, `exc` e `pergunta_id` como contexto, compatíveis com `registrar_engolido(logger_nome, operacao, exc, **contexto)`. O registrador incrementa o contador e emite log. (`app/chamados/services/validacao.py:834`; `app/core/observ.py:73`; `app/core/observ.py:75`; `app/core/observ.py:79`)
- Exceção em `_ts_tipo_do_plantio` devolve `LOTE_B` com `sem_instante_do_plantio=True`; a fila armazena esse detalhe no grupo B, continua a iteração e inclui o item nas contagens. (`app/chamados/services/validacao.py:830`; `app/chamados/services/validacao.py:835`; `app/chamados/services/validacao.py:885`; `app/chamados/services/validacao.py:887`; `app/chamados/services/validacao.py:895`)
- A pergunta fica fora de `refs_lote`, formado exclusivamente por A; a execução em lote também recusa classes diferentes de A. (`app/chamados/services/validacao.py:897`; `app/chamados/services/validacao.py:681`)
- A guarda captura exceções, não o retorno contratual `(None, tipo)`; este continua pulado no laço. Isso corresponde ao comportamento explicitamente preservado no código, não demonstra regressão do delta. (`app/chamados/services/materializacao.py:320`; `app/chamados/services/validacao.py:825`; `app/chamados/services/validacao.py:837`)

Execução real de `fila_validar_classificada` e logs produzidos: [nao verificado].

## 30/10 14:37 · sessao e14a9ebf73d4 · BENCH-O261-V-FASE · verificador · origem=Code

**Resta um achado procedente, P2: o rótulo C10 promete falta onde o leitor apenas identifica ausência de turno completo.** A frase “cada dia de trabalho sem batida vira falta” excede o resultado consumido e viola LEI-AKITA 8, “rótulo diz o que a conta faz”. (`app/escala/services/cadastro_realidade.py:114`; `app/escala/services/fase_12x36.py:175`; `CLAUDE.md:35`)

Contraexemplo por inspeção: num vínculo que alcance os dois pisos, um dia previsto sem batidas, mas justificado, pode integrar `trabalho_mudo`; o leitor consulta a programação e a presença de turno completo, enquanto o motor exclui datas justificadas das faltas. Portanto, a lista pode anunciar falta que o cálculo não atribui. A cura cabe no texto — descrever “dias de trabalho sem turno completo” — preservando regras e contagens. (`app/escala/services/fase_12x36.py:181`; `app/escala/services/fase_12x36.py:187`; `app/escala/models.py:1071`; `app/ponto/motor_calculo_v2.py:2538`)

As duas triagens procedem:

- **24x48 apresentado como 12x36: tratado.** O rótulo agora nomeia ambos, e o teste exige os ciclos definidos pelo leitor. (`app/escala/services/cadastro_realidade.py:113`; `app/escala/services/fase_12x36.py:29`; `app/escala/tests/test_o261v_fase_na_lista.py:116`)
- **Dívida da ata: real, separável desta alteração aditiva.** O pareamento continua sem alimentação da ata; o destino `turnos_do_colab` está inventariado. As novas chaves apenas expõem vínculo e datas já contadas. (`app/escala/services/fase_12x36.py:175`; `app/ponto/turnos.py:1199`; `app/core/juizes.py:905`; `app/escala/services/fase_12x36.py:192`; `app/escala/services/fase_12x36.py:202`)

No restante, conferi reutilização do leitor, mesma guarda dos vizinhos, filtro empresarial antes da contagem, ação pela tela existente e compartilhamento da lista entre HTML/PDF. Não identifiquei outra falha nesses caminhos. (`app/escala/services/cadastro_realidade.py:338`; `app/escala/services/cadastro_realidade.py:595`; `app/escala/services/cadastro_realidade.py:629`; `app/escala/services/cadastro_realidade.py:491`; `app/escala/views.py:490`)

Os testes contêm casos adversariais com leitor retornando um achado e nenhum achado. Execução da suíte, histórico RED/verde e reprodução dos quatro dias do col451: **[nao verificado]**. (`app/escala/tests/test_o261v_fase_na_lista.py:180`; `app/escala/tests/test_o261v_fase_na_lista.py:192`)

## 30/10 14:37 · sessao a6da4c562bd0 · BENCH-O261-V-FASE · verificador · origem=Code

**Encontrei um achado novo procedente: a C10 rotula como 12x36 um universo que também contém 24x48.** O leitor aceita ambos os ciclos, o adaptador incorpora todos os resultados e a descrição fixa afirma “12x36 com a fase deslocada”. Essa descrição também alimenta o contador. Viola LEI-AKITA 8: rótulo deve corresponder ao universo contado. (`app/escala/services/fase_12x36.py:29`, `app/escala/services/fase_12x36.py:106`, `app/escala/services/cadastro_realidade.py:337`, `app/escala/services/cadastro_realidade.py:112`, `app/escala/services/cadastro_realidade.py:677`, `CLAUDE.md:35`)

**[P2] Contraexemplo estático:** 24x48 com âncora em 01/09, janela 28/08–10/09 e plantões completos deslocados um dia pode produzir cinco trabalhos sem turno e quatro folgas com turno, ultrapassando o piso três dos dois lados. Portanto, o ramo é alcançável; não é apenas uma constante sobrando. A correção é alinhar a descrição ao universo do leitor, preservando a autoridade existente. O teste novo só cria 12x36. (`app/escala/models.py:1264`, `app/escala/services/fase_12x36.py:31`, `app/escala/services/fase_12x36.py:179`, `app/escala/services/fase_12x36.py:187`, `app/escala/tests/test_o261v_fase_na_lista.py:70`)

Quanto às triagens anteriores:

- **Vínculo sob cabeçalho de outro: procede a correção de identificação.** `ec_id` vem do vínculo examinado e chega ao payload e ao detalhe exibido. O teste cobra ambos, embora não reproduza dois vínculos diferentes para o mesmo colaborador. (`app/escala/services/fase_12x36.py:192`, `app/escala/services/cadastro_realidade.py:340`, `app/escala/services/cadastro_realidade.py:344`, `app/templates/escala/cadastro_x_realidade.html:28`, `app/escala/tests/test_o261v_fase_na_lista.py:109`)
- **Falha do leitor: procede que permanece e é compartilhada.** C10 captura, registra e segue sem indicador de leitura incompleta; os vizinhos citados fazem o mesmo. Isso confirma a dívida transversal, não uma correção do silêncio. O próprio teste exige C10 ausente quando o leitor falha. A efetiva inscrição dessa dívida na fila está **[nao verificado]**. (`app/escala/services/cadastro_realidade.py:550`, `app/escala/services/cadastro_realidade.py:567`, `app/escala/services/cadastro_realidade.py:574`, `app/escala/services/cadastro_realidade.py:581`, `app/escala/services/cadastro_realidade.py:588`, `app/escala/services/cadastro_realidade.py:595`, `app/escala/services/cadastro_realidade.py:602`, `app/escala/services/cadastro_realidade.py:609`, `app/escala/tests/test_o261v_fase_na_lista.py:165`)
- **Datas, PDF e MORDE-zero: procede estaticamente.** Há expectativas literais, extração efetiva do PDF e positivo sem mock antes do leitor vazio. (`app/escala/tests/test_o261v_fase_na_lista.py:29`, `app/escala/tests/test_o261v_fase_na_lista.py:106`, `app/escala/tests/test_o261v_fase_na_lista.py:145`, `app/escala/tests/test_o261v_fase_na_lista.py:190`)
- **Pareamento cru: procede como dívida do leitor existente.** Ele consulta batidas e chama diretamente o pareador; o registro aponta que deveria ler `turnos_do_colab`. (`app/escala/services/fase_12x36.py:128`, `app/escala/services/fase_12x36.py:175`, `app/core/juizes.py:905`)

No encaixe novo, não identifiquei segundo juiz, escritor ou leitura adicional do relógio: o adaptador formata o leitor e recebe `hoje` da lista; tela e PDF usam a mesma montagem. (`app/escala/services/cadastro_realidade.py:330`, `app/escala/services/cadastro_realidade.py:541`, `app/escala/services/cadastro_realidade.py:603`, `app/escala/views.py:490`)

Execução verde, RED histórico, identidade Git da cópia e presença dos quatro dias reais do col451: **[nao verificado]**. [nao verificado]

## 30/10 14:37 · sessao 3e52780cf0d6 · - · - · origem=Ronald (tmux)

Achado procedente: **o novo rótulo C10 afirma “cada dia de trabalho sem batida vira falta”, mas o leitor só mede ausência de turno completo e não decide falta**. A frase está em `app/escala/services/cadastro_realidade.py:114`; a medição exclui turnos abertos e inclui como trabalho mudo todo dia de trabalho sem turno completo em `app/escala/services/fase_12x36.py:175-184`. Viola “rótulo diz o que a conta faz”, `CLAUDE.md:35-36`.

Contraexemplo por leitura do código: atestado integral num dia originalmente de trabalho pode integrar os dias da C10, porque `eh_dia_trabalho` lê `cel.trabalha` (`app/escala/models.py:1071-1073`), enquanto a grade transforma ausência aprovada não-falta em `ausencia` (`app/escala/utils.py:1173-1180`) e o juiz impede cobrança de furo quando a cobertura é integral (`app/ponto/supra_juiz.py:279-283`). A falta da folha vem de ausências cujo catálogo determina desconto (`app/folha/export.py:158-170`). A correção necessária é no texto, preservando regras e contagens do leitor (`app/escala/services/cadastro_realidade.py:114`).

Demais verificações:

- C10 reutiliza `fora_de_fase`, apenas formata datas e identificação do vínculo; não cria regra de fase (`app/escala/services/cadastro_realidade.py:331-350`).
- A guarda corresponde à dos vizinhos, incluindo registro da exceção (`app/escala/services/cadastro_realidade.py:595-608`).
- O filtro empresarial precede inclusão e contagem das assinaturas (`app/escala/services/cadastro_realidade.py:629-652`).
- A ação aponta à tela existente de vínculo; não acrescenta escritor (`app/escala/services/cadastro_realidade.py:491-493`).
- Tela e PDF imprimem o detalhe produzido pela mesma lista (`app/templates/escala/cadastro_x_realidade.html:28`; `app/escala/services/cadastro_realidade.py:733-735`; `app/escala/views.py:490-495`).
- A triagem dos ciclos procede e está tratada: o rótulo menciona ambos, e o teste consulta `CICLOS` (`app/escala/services/cadastro_realidade.py:113`; `app/escala/tests/test_o261v_fase_na_lista.py:116-118`).
- Os testes têm mordida de integração por respostas contraditórias do leitor: um colaborador normalmente em fase passa a aparecer; leitor vazio elimina os normalmente acusados (`app/escala/tests/test_o261v_fase_na_lista.py:180-196`). Execução e resultado dos testes: [não verificado].

## 30/10 14:37 · sessao f0581bb55c3c · BENCH-O261-V-FASE · verificador · origem=Code

Encontrei **um problema de integração e duas lacunas nos selos**. Não encontrei segundo juiz no complemento: ele chama o leitor existente e formata sua saída (`app/escala/services/cadastro_realidade.py:336`).

1. **Varredura global no request, contrariando a restrição do próprio serviço.**  
   `_fase_deslocada` recebe `empresas`, mas chama o detector sem restringir colaboradores. O detector consulta duas janelas de batidas, pareia turnos e percorre os dias de todos os vínculos elegíveis. Isso acontece antes do filtro de visibilidade, em cada abertura da tela/PDF (`app/escala/services/cadastro_realidade.py:330`; `app/escala/services/fase_12x36.py:128`, `:140`, `:173`; `app/escala/views.py:490`).  
   **Classe:** custo de varredura no caminho de leitura. A restrição está expressa em `app/escala/services/cadastro_realidade.py:48` e `:353`. Os commands também pagam esse custo, embora selecionem exclusivamente C7 e C8 (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`, `:32`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`, `:40`). O comentário declara “1,0 s”, mas latência e impacto real nesta cópia **[não verificado]** (`app/escala/services/cadastro_realidade.py:111`).

2. **O selo das datas não detecta a troca entre trabalho e folga.**  
   O fixture produz duas sequências alternadas de igual tamanho; a expectativa vem do próprio leitor, e a verificação adicional exige apenas comprimento e formato ISO. Trocar as listas nas duas chaves novas preservaria essas verificações; os testes com mocks substituem o leitor inteiro (`app/escala/tests/test_o261v_fase_na_lista.py:46`, `:100`, `:114`, `:165`).  
   **Classe: selo/RED sem mordida**, especificamente na associação **data → categoria**. A implementação atual associa corretamente as listas; o defeito está na proteção contra regressão (`app/escala/services/fase_12x36.py:204`). Contrafactual analisado estaticamente; execução **[não verificado]**.

3. **O selo do PDF não prova “PDF = mesma lista”.**  
   O teste aceita qualquer conteúdo iniciado por `%PDF`. Omitir C10 ou seus dias somente no PDF não reprovaria essa asserção (`app/escala/tests/test_o261v_fase_na_lista.py:128`).  
   **Classe: selo/RED sem mordida**, contra L-040; essa mesma insuficiência já está registrada em `app/docs/LEIS.md:163`. Não encontrei omissão atual: o PDF percorre as assinaturas e imprime `detalhe` (`app/escala/services/cadastro_realidade.py:727`, `:732`).

Também conferi:

- **Empresas e contadores:** o filtro final elimina colaboradores não visíveis antes da contagem C10; ignorar `empresas` na consulta amplia o processamento, mas não demonstra vazamento ou contador inflado (`app/escala/services/cadastro_realidade.py:626`, `:647`, `:674`).
- **Compatibilidade:** as novas chaves são aditivas e contêm strings ISO. API, pauta, fila, propositor e command consultados continuam usando campos preservados (`app/escala/services/fase_12x36.py:204`, `:220`; `app/api/views_mensageria.py:1675`; `app/chamados/services/fila_por_causa.py:173`; `app/escala/services/propositor.py:455`; `app/ponto/management/commands/vigiar_familia_escala.py:26`).
- **Juiz, escritor e relógio:** o complemento não recalcula fase nem grava cadastro; a ação encaminha à porta do vínculo. `hoje` é resolvido por `lista` e repassado ao detector (`app/escala/services/cadastro_realidade.py:336`, `:488`, `:539`, `:601`). O pré-requisito do command C8 continua antes de `lista`; não identifiquei nova inversão nessa ordem (`app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:32`).
- **Remover a cura seria detectado:** o caso positivo exige C10, e o mock exige exclusivamente o colaborador indicado pelo leitor; portanto, não procede chamar toda a suíte de vazia (`app/escala/tests/test_o261v_fase_na_lista.py:97`, `:167`).

Os testes usam setembro e colaboradores sintéticos; não demonstram a presença dos quatro dias reais de col451, documentados como **21, 23, 25 e 27/10** (`app/escala/tests/test_o261v_fase_na_lista.py:23`, `:40`; `app/docs/RELATO.md:217`). Essa confirmação, a execução dos testes e a identidade Git do hash ficaram **[não verificado]**.

## 30/10 14:37 · sessao 0cd989a7e12e · - · - · origem=Ronald (tmux)

Achado novo: **C10 rotula como “12x36” resultados que também incluem 24x48**, contrariando “rótulo diz o que a conta faz” da LEI-AKITA 8. O texto novo é fixo; o leitor admite ambos os ciclos e o adaptador incorpora todos os achados. (`app/escala/services/cadastro_realidade.py:112`, `app/escala/services/cadastro_realidade.py:337`, `app/escala/services/fase_12x36.py:29`, `app/escala/services/fase_12x36.py:106`, `CLAUDE.md:35`)

Contraexemplo estático: 24x48, âncora 01/09/2026, hoje 10/09, sem célula/foto e plantões deslocados +1 dia. O cadastro marca trabalho em 29/08, 01/09, 04/09, 07/09 e 10/09; plantões reais 07h→07h seguinte em 30/08, 02/09, 05/09 e 08/09 produzem **5 trabalhos sem turno e 4 folgas com turno**, superando o piso 3 nos dois lados. O cálculo canônico usa módulo 3; o leitor admite os resultados e o pareador fecha esses plantões de 24h pelo envelope de 26h. (`app/escala/models.py:1264`, `app/escala/services/fase_12x36.py:31`, `app/escala/services/fase_12x36.py:179`, `app/escala/services/fase_12x36.py:187`, `app/ponto/turnos.py:1428`, `app/ponto/turnos.py:1445`, `app/ponto/turnos.py:1490`)

A correção pode ser apenas tornar o rótulo fiel ao universo do leitor, preservando a autoridade única. O teste novo não alcança esse caso: cria somente 12x36. (`app/escala/tests/test_o261v_fase_na_lista.py:70`)

Demais verificações:

- **Identidade do vínculo tratada:** `ec_id` provém do vínculo examinado e segue em `ec` e no detalhe, independentemente do cabeçalho vigente. (`app/escala/services/fase_12x36.py:173`, `app/escala/services/fase_12x36.py:192`, `app/escala/services/cadastro_realidade.py:340`, `app/escala/services/cadastro_realidade.py:344`)
- **Datas sem derivação paralela:** a alteração serializa exatamente as listas usadas nas contagens anteriores. (`app/escala/services/fase_12x36.py:202`, `app/escala/services/fase_12x36.py:207`)
- **Sem nova leitura de relógio nesse caminho:** `lista` fixa `hoje`, passa ao adaptador e ele repassa ao leitor; o fallback existente do leitor não é acionado com esse valor. (`app/escala/services/cadastro_realidade.py:541`, `app/escala/services/cadastro_realidade.py:603`, `app/escala/services/cadastro_realidade.py:337`, `app/escala/services/fase_12x36.py:103`)
- **Dívida anterior confirmada:** o leitor chama `parear_turnos` com batidas cruas sem `papel_por_minuto`; o registro exige migrar para `turnos_do_colab`. Esses sítios não pertencem às linhas alteradas do diff fornecido. (`app/escala/services/fase_12x36.py:146`, `app/escala/services/fase_12x36.py:175`, `app/ponto/turnos.py:1199`, `app/core/juizes.py:905`)
- Os consumidores anteriores continuam lendo o mesmo resultado: pauta, mensageria e propositor. As adições `ec_id` e datas não mudam as chaves que esses consumidores usam. (`app/escala/services/fase_12x36.py:223`, `app/api/views_mensageria.py:1670`, `app/api/views_mensageria.py:1675`, `app/escala/services/propositor.py:455`)

## 30/10 14:36 · sessao 633de4db7000 · - · - · origem=Ronald (tmux)

Revisão dos consumidores concluída, sem execução de testes ou escrita de arquivos. [não verificado] [nao verificado]

- **Não há segundo juiz novo no wrapper.** `_fase_deslocada` chama a autoridade existente e apenas formata seus resultados; as datas novas vêm das listas que o próprio detector já contou. `app/escala/services/cadastro_realidade.py:336`, `app/escala/services/cadastro_realidade.py:338`, `app/escala/services/fase_12x36.py:199`, `app/escala/services/fase_12x36.py:204`. Acusar essa chamada de violar CLAUDE 4a contrariaria o aval literal de usar o leitor existente; pareamento e julgamento são anteriores ao diff. `app/escala/services/fase_12x36.py:175`, `app/escala/services/fase_12x36.py:181`.

- **Custo adicional comprovável, sem número de latência medido:** cada chamada não vazia executa cinco leituras ORM em lote: vínculos, duas janelas de batidas, células e folgas; pareia as batidas duas vezes e percorre cada colaborador × 14 dias. `app/escala/services/fase_12x36.py:114`, `app/escala/services/fase_12x36.py:128`, `app/escala/services/fase_12x36.py:140`, `app/escala/services/fase_12x36.py:160`, `app/escala/services/fase_12x36.py:170`, `app/escala/alimentacao.py:66`, `app/escala/services/fase_12x36.py:145`, `app/escala/services/fase_12x36.py:175`, `app/escala/services/fase_12x36.py:179`. C10 chama sem estreitar por IDs ou empresas, embora a autoridade aceite esse estreitamento; o filtro de empresas ocorre depois de toda a leitura. `app/escala/services/cadastro_realidade.py:336`, `app/escala/services/fase_12x36.py:108`, `app/escala/services/cadastro_realidade.py:626`. A tela HTML/PDF e ambos os commands C7/C8 passam a pagar esse custo, mesmo sem usar C10. `app/escala/views.py:490`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`. Há conflito com a lápide local que proíbe varrer frota no request; esta é a referência mais forte para um achado de custo, não “juiz duplicado”. `app/escala/services/cadastro_realidade.py:48`, `app/escala/services/cadastro_realidade.py:353`.

- **Não achei vazamento por `empresas` nem contador C10 inflado:** linhas externas são descartadas antes de computar `por_cod`, `colabs` e `ocorrencias`. `app/escala/services/cadastro_realidade.py:626`, `app/escala/services/cadastro_realidade.py:647`, `app/escala/services/cadastro_realidade.py:674`.

- **Novas chaves não quebram os consumidores localizados:** API serializa `vinculos` diretamente e recebe strings ISO; fila e propositor só extraem IDs; command e pauta selecionam campos existentes. `app/api/views_mensageria.py:1675`, `app/escala/services/fase_12x36.py:204`, `app/chamados/services/fila_por_causa.py:173`, `app/escala/services/propositor.py:455`, `app/ponto/management/commands/vigiar_familia_escala.py:26`, `app/escala/services/fase_12x36.py:220`.

- **Não há nova leitura de relógio fora da borda nesse fluxo:** `lista` resolve `hoje` e transmite o mesmo valor até `fora_de_fase`. `app/escala/services/cadastro_realidade.py:539`, `app/escala/services/cadastro_realidade.py:601`, `app/escala/services/cadastro_realidade.py:336`, `app/escala/services/fase_12x36.py:103`.

- **O detector também cobre 24x48, mas o rótulo 12x36 já era herdado** na pauta e API; não usaria isso como achado central do diff. `app/escala/services/fase_12x36.py:29`, `app/escala/services/fase_12x36.py:228`, `app/api/views_mensageria.py:1657`.

## 30/10 14:36 · sessao 1ba01fb3d609 · - · - · origem=Ronald (tmux)

Os testes **mordem a remoção da integração**, mas têm duas lacunas específicas de prova. Não executei testes, pois criariam banco/arquivos; a avaliação abaixo é estática. [não verificado: execução] [nao verificado]

- **Classe: selo/RED sem mordida — associação dos dias à categoria.** Trocar `trabalho_mudo` por `folga_trabalhada` nas duas novas chaves manteria os testes novos verdes: o fixture produz 14 dias alternados, com sete em cada lado; os testes comparam a lista com o próprio leitor e depois verificam apenas comprimento e ISO. Os mocks substituem integralmente o leitor, portanto não detectam essa troca. O código atual associa corretamente as listas; trata-se de lacuna do selo, não de erro comprovado na saída atual. Referências: `app/escala/services/fase_12x36.py:204`, `app/escala/services/fase_12x36.py:205`, `app/escala/tests/test_o261v_fase_na_lista.py:46`, `app/escala/tests/test_o261v_fase_na_lista.py:100`, `app/escala/tests/test_o261v_fase_na_lista.py:114`, `app/escala/tests/test_o261v_fase_na_lista.py:165`.

- **Classe: selo/RED sem mordida — PDF igual à lista.** O teste novo aceita qualquer resposta começando por `%PDF`; retirar C10 ou seus dias somente do PDF não o reprovaria. Repete precisamente a dívida já registrada em L-040. O renderizador atual percorre todas as assinaturas e inclui `detalhe`, portanto não encontrei omissão efetiva atual. Referências: `app/escala/tests/test_o261v_fase_na_lista.py:128`, `app/escala/tests/test_o261v_fase_na_lista.py:129`, `app/docs/LEIS.md:163`, `app/escala/services/cadastro_realidade.py:727`, `app/escala/services/cadastro_realidade.py:732`.

**O que funciona nos selos:** remover a leitura C10 reprovaria o caso positivo; recalcular independentemente ignorando o leitor reprovaria os mocks que fazem entrar exclusivamente o colaborador em fase e depois zeram o resultado. A exceção é provocada e exige que A4 sobreviva. O filtro testa exclusão e inclusão, e a contagem compara C10 com os dois achados reais. Referências: `app/escala/tests/test_o261v_fase_na_lista.py:93`, `app/escala/tests/test_o261v_fase_na_lista.py:135`, `app/escala/tests/test_o261v_fase_na_lista.py:139`, `app/escala/tests/test_o261v_fase_na_lista.py:150`, `app/escala/tests/test_o261v_fase_na_lista.py:158`, `app/escala/tests/test_o261v_fase_na_lista.py:170`.

O fixture novo não representa especificamente os quatro dias de col451: usa colaboradores sintéticos e HOJE=10/09/2026. Os quatro dias documentados de col451 são 21, 23, 25 e 27/10; não há prova, nesses testes, de sua presença na lista atual. Referências: `app/escala/tests/test_o261v_fase_na_lista.py:23`, `app/escala/tests/test_o261v_fase_na_lista.py:40`, `app/docs/RELATO.md:217`.

## 30/10 14:36 · sessao 23548e779ccc · - · - · origem=Ronald (tmux)

Não achei quebra procedente nos consumidores de `lista()` ou no filtro de empresas:

- **Estrutura/compatibilidade:** C10 entra no mesmo `cad` e recebe os mesmos campos comuns de assinatura; as chaves externas do retorno permanecem intactas (`app/escala/services/cadastro_realidade.py:601`, `:640`, `:684`). A tela imprime `detalhe` e ação genericamente (`app/templates/escala/cadastro_x_realidade.html:28`); o PDF recebe os mesmos dados e imprime o mesmo detalhe (`app/escala/views.py:490`, `app/escala/services/cadastro_realidade.py:723`).
- **Contador == universo visível:** o descarte por `empresas` acontece antes da montagem de assinaturas e da contagem; C10 recebe contadores derivados apenas das linhas incluídas (`app/escala/services/cadastro_realidade.py:626`, `:647`, `:674`). Portanto, ignorar `empresas` dentro de `_fase_deslocada()` não vaza linhas ou infla o contador C10; amplia somente o trabalho executado (`app/escala/services/cadastro_realidade.py:333`, `:336`).
- **Commands:** ambos continuam selecionando explicitamente C7/C8; C10 não faz fechar chamados adicionais (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`, `:32`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`, `:40`). O pré-requisito da lavra C8 continua precedendo `lista()` (`app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:32`).
- **Custo comprovável estaticamente:** cada chamada da tela/PDF e dos dois comandos agora executa o detector global, mesmo quando a tela tem empresas restritas ou o command usa somente C7/C8 (`app/escala/views.py:490`; `app/escala/services/cadastro_realidade.py:336`; comandos acima). O detector lê duas janelas de batidas e pareia turnos, além de carregar células/folgas e visitar cada dia por vínculo (`app/escala/services/fase_12x36.py:128`, `:140`, `:146`, `:160`, `:170`, `:173`). Tempo real e impacto impeditivo: **[não verificado]**. O comentário novo de 1 segundo/5 consultas não constitui medição reproduzida nesta revisão (`app/escala/services/cadastro_realidade.py:110`).

Ponto para avaliação do responsável pela consolidação: falha do leitor C10 produz apenas log, sem estado `sem_medida` no retorno, indistinguível visualmente de ausência de achados (`app/escala/services/cadastro_realidade.py:603`, `:684`). Isso contrasta com a explicitação da indisponibilidade de C3 (`app/escala/services/cadastro_realidade.py:556`; `app/templates/escala/cadastro_x_realidade.html:17`) e com o princípio escrito contra ausência de sinal tratada como sinal bom (`app/escala/services/cadastro_realidade.py:540`). **Não fechei como achado**, porque os demais leitores já seguem a mesma guarda e o teste novo exige continuidade da lista; seria necessário sustentar que o requisito desta obra exige expor indisponibilidade C10, além de preservar as outras assinaturas (`app/escala/services/cadastro_realidade.py:599`).

## 30/10 14:36 · sessao d21df08be375 · - · - · origem=Ronald (tmux)

Não achei regressão procedente nas chaves acrescentadas ao leitor: `ec_id` é o vínculo efetivamente iterado; as datas são serializações das mesmas listas usadas nas contagens, sem mudar predicado, piso ou `n`. `app/escala/services/fase_12x36.py:173`, `:187`, `:192`, `:202`, `:207`, `:216`.

As duas triagens procedem:

- O leitor cobre `12x36` e `24x48`; o teste novo consulta `CICLOS` e exige ambos no rótulo, cobrindo a correção declarada. `app/escala/services/fase_12x36.py:29`; `app/escala/tests/test_o261v_fase_na_lista.py:116`.
- A dívida da ata existe: o leitor chama `parear_turnos` sem alimentar `papel_por_minuto`; esse parâmetro é justamente o que faz a ata prevalecer sobre `tipo`, e `turnos_do_colab` o carrega. O registro já aponta o destino `turnos_do_colab`. `app/escala/services/fase_12x36.py:146`, `:175`; `app/ponto/turnos.py:1199`, `:1212`, `:1909`; `app/core/juizes.py:905`.
- Trocar o pareamento alcançaria pauta, fila por causa, propositor e mensageria; portanto preservar essa dívida nesta mudança aditiva é coerente com a instrução recebida de não alterar regra/contagem. `app/escala/services/fase_12x36.py:223`; `app/chamados/services/fila_por_causa.py:173`; `app/escala/services/propositor.py:456`; `app/api/views_mensageria.py:1670`.

Os consumidores existentes selecionam chaves específicas ou serializam a lista como JSON; as novas chaves contêm inteiro, strings ISO e listas, sem incompatibilidade identificada nessa leitura. `app/escala/services/fase_12x36.py:192`, `:207`, `:228`; `app/api/views_mensageria.py:1671`.

Execução dos testes: [não verificado]. [nao verificado]

## 30/10 14:36 · sessao 3457bbe982c8 · - · - · origem=Ronald (tmux)

Não encontrei falha nova procedente nos testes nem no caminho HTML/PDF revisado: a view chama `lista` uma vez e entrega os mesmos dados ao renderizador escolhido; ambos imprimem `detalhe`. (`app/escala/views.py:490`, `app/escala/views.py:492`, `app/escala/views.py:495`, `app/templates/escala/cadastro_x_realidade.html:28`, `app/escala/services/cadastro_realidade.py:732`)

A triagem dos selos procede:

- Datas esperadas estão escritas explicitamente pela regra e são cobradas tanto da C10 quanto do leitor. (`app/escala/tests/test_o261v_fase_na_lista.py:15`, `app/escala/tests/test_o261v_fase_na_lista.py:29`, `app/escala/tests/test_o261v_fase_na_lista.py:106`, `app/escala/tests/test_o261v_fase_na_lista.py:121`)
- PDF é efetivamente extraído com `pypdf`, e o texto extraído precisa conter C10 e datas dos dois lados. (`app/escala/tests/test_o261v_fase_na_lista.py:145`, `app/escala/tests/test_o261v_fase_na_lista.py:146`)
- MORDE-zero exige primeiro o invertido na lista sem mock; depois exige ausência de C10 com leitor vazio. MORDE-um exige precisamente o colaborador indicado pelo leitor falso. (`app/escala/tests/test_o261v_fase_na_lista.py:184`, `app/escala/tests/test_o261v_fase_na_lista.py:186`, `app/escala/tests/test_o261v_fase_na_lista.py:189`)
- A identidade do vínculo é cobrada no payload e no detalhe, mas o fixture contém somente um vínculo por colaborador; não reproduz o caso adversarial de cabeçalho diferente. Isso é limitação de cobertura, não demonstração de defeito atual. (`app/escala/tests/test_o261v_fase_na_lista.py:46`, `app/escala/tests/test_o261v_fase_na_lista.py:109`, `app/escala/tests/test_o261v_fase_na_lista.py:123`)

Execução verde e evidência histórica de RED: [nao verificado]. [nao verificado]

## 30/10 14:35 · sessao 9e28c6d21f16 · BENCH-O261-C · verificador · origem=Code

**Encontrei uma divergência no ponto 3: o reordenamento preserva o retorno no caminho identificado, mas acrescenta uma chamada ao logger.** A afirmação “sem efeito colateral” não corresponde ao código: `VETO_CELULA_*` é registrado antes da cerca. (`app/chamados/reconciliador.py:765`, `app/chamados/reconciliador.py:830`, `app/chamados/reconciliador.py:835`)

O caminho é alcançável pelo código: um chamado vivo já ocupado por pergunta do mesmo motivo é excluído pelo derivador; o c2 reencontra esse chamado e dispensa piso/protesto; uma célula vetada então aciona o logger. Em competência exportada, a cerca antecipada encerraria antes dessa chamada. Ambos retornam `False`; não há escrita de domínio nesse ramo. (`app/chamados/services/disputa_emissao.py:209`; `app/chamados/reconciliador.py:797`, `app/chamados/reconciliador.py:826`; `app/ponto/marcos.py:159`; `app/chamados/reconciliador.py:713`, `app/chamados/reconciliador.py:830`)

Ocorrência desse cenário em produção e persistência da mensagem pelos handlers: [nao verificado].

Nos seis pontos:

1. **LEI-AKITA no corpo de `22c08b8e`:** [nao verificado]. O corpo do commit não estava disponível para conferência.

2. **Pauta DP:** a prosa do código agora declara explicitamente dono DP, geração pelo script de passivo depois do pouso e ausência dessa responsabilidade na porta. Prosa do commit: [nao verificado]. (`app/chamados/reconciliador.py:691`)

3. **Custo e reordenamento:** a cerca está depois dos gates, imediatamente antes da emissão. Não encontrei dependência deles de algo garantido pela cerca; a ressalva é o logger acima. Os testes exigem zero consultas à cerca no veto por protesto e medem `registrar_batida` com teto 31. Contagens executadas: [nao verificado]. (`app/chamados/reconciliador.py:835`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:318`; `app/ponto/tests/test_selo_chokepoint_escrita.py:169`, `app/ponto/tests/test_selo_chokepoint_escrita.py:281`)

4. **Guarda removida/idempotência:** confirmado. Os dois chamadores exigem destino nulo; o teste passa duas vezes pelo cron e exige primeira passagem efetiva, segunda sem alteração de pergunta, chamados ou quantidade de logs. (`app/chamados/reconciliador.py:860`, `app/chamados/reconciliador.py:877`, `app/chamados/reconciliador.py:1376`; `app/chamados/services/disputa_emissao.py:404`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:297`)

5. **R1/R1b:** confirmado. R1 exige `(None, None)`, mesmo destino e chamado vivo; R1b exige `admin_sem_fato_novo`, retorno falso e nenhuma costura no morto. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:150`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:163`)

6. **Composição única/vigia:** confirmado; não encontrei mudança de comportamento do vigia. Mantém janela, chave do memo e classificação; K6 já retorna booleano. Sem data, a função devolve `None`, o vigia escolhe `aberta` e a cerca exclui antes de chamá-la. (`app/chamados/services/vigia_fio.py:413`, `app/chamados/services/vigia_fio.py:431`, `app/chamados/services/vigia_fio.py:435`; `app/folha/export.py:575`; `app/chamados/reconciliador.py:705`)

Por classe, limitado aos sítios criados ou tocados:

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando registrado | **Nada**: delegação a K6, C7, C6 e `motivos_atendidos`. (`app/chamados/reconciliador.py:713`, `:716`, `:727`, `:1476`) |
| Segundo escritor fora da porta | **Nada**: emissão e costura permanecem nas portas existentes. (`app/chamados/reconciliador.py:837`, `:877`, `:1513`) |
| Relógio fora da borda | **Nada novo identificado** nos trechos acrescentados. (`app/chamados/reconciliador.py:704`; `app/chamados/services/vigia_fio.py:431`) |
| Derivação paralela de fonte única | **Nada novo**: ambos leem a composição extraída. (`app/chamados/reconciliador.py:711`; `app/chamados/services/vigia_fio.py:413`) |
| Selo/RED sem mordida | **Nada demonstrado nas asserções**; execução e RED histórico: [nao verificado]. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:151`, `:324`) |
| Ordem de trava | **Nada novo identificado**: os gates antecipados são leitores; `abrir` mantém sua trava. (`app/chamados/reconciliador.py:797`, `:824`, `:835`; `app/chamados/models.py:666`) |
| Pouso antes do pré-requisito | **Nada demonstrado**: o RELATO declara B pousado antes de C; comprovação operacional e remedida: [nao verificado]. (`app/docs/RELATO.md:3`, `:18`) |

## 30/10 14:35 · sessao 2c6263d7ddd1 · BENCH-O261-C · verificador · origem=Code

**Há falhas no alcance do HIST novo: ele chama um S128 que modifica outras perguntas e cujo segundo laço não preserva as guardas do primeiro.** O defeito interno já existia; o diff acrescenta caminhos que chegam a ele. (`app/chamados/reconciliador.py:1450`, `app/chamados/reconciliador.py:1493`, `app/chamados/services/disputa_emissao.py:1621`)

Não autentiquei os commits e ancestrais: a cópia recebida não tem repositório Git acessível. A comparação histórica abaixo usa o diff fornecido. Não executei a suíte Django. [não verificado] [nao verificado]

1. **[P1] A restrição à competência não entregue não contém os efeitos da operação.**  
   Uma pergunta MORTA de intervalo, em competência não entregue, passa pelo HIST novo e chama S128. Esse serviço inclui **todas as disputas abertas do colaborador**, sem restringir dia ou competência. Uma segunda pergunta, de competência entregue, MORTA, não validada, não materializada e sem credor vivo recebe carimbo e via. Portanto, verificar somente a competência da pergunta de entrada não garante “na entregue, caminho anterior igual”. (`app/chamados/reconciliador.py:1436`, `app/chamados/reconciliador.py:1493`, `app/chamados/services/disputa_emissao.py:1622`, `app/chamados/services/disputa_emissao.py:1634`, `app/chamados/services/disputa_emissao.py:1644`)

   Antes, o intervalo não casava com o mapa de `batida_ausente` nem entrava no c1; o alargamento do HIST passa a disparar essa cascata. Isso contraria o limite de alcance e o gravado intocado da exportada. (`app/chamados/models.py:2469`, `app/chamados/reconciliador.py:332`, `app/chamados/reconciliador.py:1450`, `app/docs/CORTES.md:399`)

2. **[P1] MORTA com resposta pode ganhar carimbo sem testemunha, apesar da proteção explícita.**  
   Caso: intervalo respondido, `validada_em` e `materializada_em` nulos, sem batida testemunha, célula decidida pelo admin com veredito que não acusa, histórico encerrado único e disputa sem credor vivo. O C6 pode declarar essa pergunta MORTA pela decisão da célula. (`app/chamados/juizes.py:1018`, `app/chamados/juizes.py:1028`, `app/chamados/juizes.py:1054`)

   O HIST novo chama S128. O primeiro laço **pula** a resposta sem testemunha; o segundo seleciona novamente a mesma pergunta e carimba sem repetir a proteção. O materializador posteriormente exige `materializada_em IS NULL`, portanto perde essa resposta do seu universo. (`app/chamados/reconciliador.py:1493`, `app/chamados/services/disputa_emissao.py:1558`, `app/chamados/services/disputa_emissao.py:1634`, `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/materializacao.py:1286`)

3. **[P1] A guarda da tranca também é vencida pelo segundo laço.**  
   Caso: competência não entregue, porém trancada; intervalo sem carimbo; histórico resolvido por via diferente de `competencia_trancada`; disputa sem credor vivo. Tranca e entrega consultam fatos distintos, e a porta de trancar não exige exportação. (`app/folha/export.py:570`, `app/ponto/services/fechamento.py:1376`, `app/ponto/services/fechamento.py:1432`)

   C6 declara MORTA pela tranca; o HIST costura antes de S128. O primeiro laço respeita a tranca, mas o segundo aceita o mesmo resultado MORTA e grava `materializada_em`. A reversão por competência só desfaz perguntas com a via específica `competencia_trancada`, não esse carimbo `chamado_encerrado`. É quebra da proteção de silêncio reversível, exposta agora aos intervalos pelo HIST alargado. (`app/chamados/juizes.py:1008`, `app/chamados/reconciliador.py:1487`, `app/chamados/services/disputa_emissao.py:1551`, `app/chamados/services/disputa_emissao.py:1641`, `app/chamados/services/disputa_emissao.py:1647`, `app/chamados/models.py:2213`)

4. **[P2] c1 está intacto no corpo, mas perde casos por mudança no despacho.**  
   `saida_sem_entrada` MORTA/validada, sem resposta e sem turno aberto, com **dois históricos `batida_ausente`**: antes nenhum casava pelo mapa e c1 lavrava; agora ambos atendem pelo juiz, `len(hist)==2`, c1 não é chamado e a porta devolve `False`. O caso simétrico ocorre com `orfao_14h` diante de históricos `saida_sem_entrada`. (`app/chamados/models.py:2468`, `app/chamados/juizes.py:1597`, `app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:399`, `app/chamados/reconciliador.py:1457`, `app/chamados/reconciliador.py:1469`)

   Com histórico único, essas perguntas passam a HIST/S128, mudando destino e via. A troca pelo juiz foi solicitada; portanto essa mudança isolada não prova infração. A perda do caminho c1 quando surgem múltiplos candidatos precisa ser distinguida da afirmação “c1 intocado”. (`app/chamados/reconciliador.py:406`, `app/chamados/reconciliador.py:1487`, `app/chamados/reconciliador.py:1493`)

5. **[P2] Há consulta duplicada de competência e custo novo sem cobertura do cenário afetado.**  
   No intervalo vivo que chega ao c2, K6 é consultado em `reatar` e novamente pelo helper do c2. Cada chamada carrega os registros da competência; o memo de C6 não elimina essa duplicação. C6 também passou a anteceder as recusas do c1, cuja ordem anterior tinha justificativa explícita de custo na batida. (`app/chamados/reconciliador.py:1436`, `app/chamados/reconciliador.py:749`, `app/chamados/reconciliador.py:709`, `app/folha/export.py:570`, `app/chamados/reconciliador.py:394`)

   O selo existente com três perguntas já lhes atribui destino; o caminho alterado seleciona órfãs. Logo esse cenário não mede o custo novo. **Estouro efetivo do teto: [não verificado].** (`app/ponto/tests/test_selo_chokepoint_escrita.py:239`, `app/chamados/reconciliador.py:1355`)

6. **[P2] O selo novo não distingue algumas implementações proibidas.**
   
   - Todas as MORTAS da fixture são produzidas preenchendo `validada_em`. Por inspeção, trocar as novas consultas a C6 por `p.validada_em is None` preserva os resultados esperados dessa bateria, embora crie um segundo juiz. Mutante executado: **[não verificado]**. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:92`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:150`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:159`)
   - A bateria chama `reatar` e o cron, sem testar diretamente c2. Não sela que o alargamento esteja na porta exigida, em vez de num ramo alternativo do wrapper. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:95`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:209`)
   - O DNA usa `ii`/`fi`; o juiz procura `hii`/`hfi`. Os intervalos testados dependem do veredito genérico `furo`, sem comprovar a leitura da lâmpada específica. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:66`, `app/chamados/juizes.py:823`, `app/chamados/juizes.py:863`)
   - R4 exportada, R4b, R5 e R6 preservam comportamento anterior e passam sem a cura por inspeção; são controles, não RED isolado contra o ancestral. R1/R2/R3 e o controle positivo R4 possuem asserções da mudança. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:117`, `:133`, `:141`, `:148`, `:161`, `:169`, `:178`, `:182`)

Os efeitos dos itens 1–3 foram também reproduzidos executando **somente em memória** o corpo real do S128, com ORM e respostas dos juízes substituídos: o segundo laço carimbou a respondida, a trancada e a irmã de outro dia. Isso confirma o fluxo, não constitui teste integrado nem medição de produção. (`app/chamados/services/disputa_emissao.py:1514`, `app/chamados/services/disputa_emissao.py:1634`)

Quanto às classes restantes:

- **Segundo juiz efetivamente introduzido: nada.** Os helpers delegam a C6, K6 e `janela_atual`. (`app/chamados/reconciliador.py:700`, `app/chamados/reconciliador.py:711`)
- **Segundo escritor fora da porta: nada.** Nascimento e costura continuam em `abrir` e `reapontar_resolvedora`. (`app/chamados/reconciliador.py:816`, `app/chamados/reconciliador.py:856`, `app/chamados/reconciliador.py:1487`)
- **Relógio novo fora da borda: nada no diff.** Os helpers recebem o dia; o parse movido não consulta o relógio. (`app/chamados/reconciliador.py:697`, `app/chamados/reconciliador.py:1425`)
- **Derivação paralela nova de fonte única: nada.** O HIST aberto passa a ler `motivos_atendidos`. (`app/chamados/reconciliador.py:1450`)
- **Ordem de locks: nada novo identificado.** `abrir` conserva a trava do colaborador antes do dedup; a falha de guarda da tranca é o item 3. (`app/chamados/models.py:666`)
- **Pouso antes do pré-requisito: nada comprovado.** O relato registra B pousado e C em remedição; comprovação externa dos logs e cumprimento do teto: **[não verificado]**. (`app/docs/RELATO.md:3`, `app/docs/RELATO.md:4`, `app/docs/CORTES.md:399`)

O alcance operacional inclui cron `*/15`, emissão de disputa e signal da batida. Portanto, os efeitos acima não ficam restritos ao cron: o evento chama a mesma porta. (`app/config/crons.py:376`, `app/chamados/management/commands/reconciliar_chamados.py:36`, `app/chamados/services/disputa_emissao.py:409`, `app/ponto/signals.py:215`, `app/chamados/reconciliador.py:1302`)

## 30/10 14:34 · sessao 5b5f385a7ac8 · - · - · origem=Ronald (tmux)

Achado no c2: a alegação “conjunções sem efeito colateral” é falsa; `_log.info('reconciliador_c2: VETO_CELULA_%s ...')` executa antes da cerca. O retorno permanece `False`, mas há chamada adicional ao logger quando a cerca antiga recusaria primeiro. Não identifiquei escrita de domínio adicional nesse caminho. (`app/chamados/reconciliador.py:765`, `:828`, `:830`, `:835`.)

Caminho alcançável pelo código, sem exigir protesto contraditório:

- Pergunta órfã de intervalo entra pelo filtro real; c1 recusa intervalo. (`app/chamados/reconciliador.py:1375`, `:363`.)
- Chamado vivo do mesmo dia já possui outra pergunta do mesmo motivo sem carimbo: o derivador o exclui; sem HIST, alcança c2. (`app/chamados/services/disputa_emissao.py:209`, `app/chamados/reconciliador.py:1483`, `:1493`.)
- c2 encontra esse chamado vivo e dispensa piso/protesto; célula com `trabalha=False` ou `veredito='trabalhou'` chega ao veto. (`app/chamados/reconciliador.py:797`, `:799`, `:826`, `app/ponto/marcos.py:159`, `:161`.)
- Se competência exportada, a cerca antecipada recusaria sem esse logger; a nova ordem chega ao logger e retorna `False`. (`app/chamados/reconciliador.py:713`, `:724`, `:830`.)
- Existência desse cenário no banco real: [nao verificado]. Persistência da mensagem pelos handlers: [nao verificado].

Verificação complementar somente em memória: AST da função atual, imports substituídos por fakes, comparação cerca antecipada/final; ambos retornaram `False`, respectivamente zero/uma chamadas ao logger. Isso verifica a ordem local, não integração Django. [nao verificado]

Demais gates examinados são leitores: horizonte delega à janela; isenção lê flag/catálogo; ausência consulta queryset; célula consulta valores; motor retorna decisão. Não dependem de pré-condição estabelecida pela cerca. (`app/ponto/services/cartorio.py:244`, `app/core/constants.py:18`, `app/chamados/catalogo/modulos.py:277`, `app/ponto/turnos.py:2518`, `app/ponto/marcos.py:155`, `app/chamados/catalogo/motor.py:130`.)

Por esse recorte: segundo escritor **nada**, nova ordem de travas **nada**, relógio novo fora da borda **nada**; o trecho novo consulta juízes e o escritor `abrir`, incluindo avaliação de `_ctx_c2`, permanece depois da cerca. (`app/chamados/reconciliador.py:705`, `:713`, `:716`, `:724`, `:727`, `:835`, `:837`, `:847`.)

## 30/10 14:34 · sessao 797f487eabf6 · BENCH-O261-C · verificador · origem=Code

**Achei uma lacuna de paridade herdada pelo diff: vínculo ausente não é vetado pelo leitor escolhido.** (`app/ponto/services/furos_vetados.py:26`; `app/chamados/reconciliador.py:845`)

- **(a) Predicado:** `motivo_do_veto(..., None, ...)` retorna `None`; `_abrir_chamado` considera esse mesmo caso `VETO_DEGENERADA`, pois template ausente é degenerado. O cartório também impede emissão sem vínculo. Portanto, compartilhar o leitor da impressão **não garante paridade integral com a emissão**. (`app/ponto/services/furos_vetados.py:26`; `app/ponto/management/commands/detectar_ausencias.py:599`; `app/escala/regua_defesa.py:23`; `app/escala/regua_defesa.py:45`; `app/ponto/services/cartorio.py:1348`)

  Esse `None` está previsto inclusive com célula existente: geradora fora da vigência retorna `None`. Os gates anteriores do c2 verificam protestos e `trabalha/veredito`, sem exigir vínculo válido; o bloco acrescentado deixa essa ausência passar. É lacuna residual, não regressão criada neste diff; ocorrência em dados reais **[não verificado]**. (`app/escala/alimentacao.py:250`; `app/chamados/reconciliador.py:825`; `app/ponto/marcos.py:155`; `app/chamados/reconciliador.py:845`)

  **Segundo juiz, vínculo ou dia divergente: nada adicional.** Com vínculo presente, são os mesmos predicados; a impressão usa exatamente a mesma composição de leitores. (`app/ponto/services/furos_vetados.py:28`; `app/ponto/management/commands/detectar_ausencias.py:599`; `app/ponto/services/cartorio.py:1013`)

- **(b) Costura/renascer/abrir com encerrado: nada no fluxo sequencial.** Encerrado torna `_costura_no_vivo=False`, recebe o veto antes de `abrir` e, portanto, antes de `_renascer_por_premissa_viva`. Consulta e abertura usam a mesma chave. (`app/chamados/reconciliador.py:795`; `app/chamados/reconciliador.py:843`; `app/chamados/reconciliador.py:853`; `app/chamados/models.py:685`)

- **(c) C1: muda, sim.** `orfao_14h` e `saida_sem_entrada` passam a sofrer o veto em NASCER/RENASCER, inclusive fora da competência aberta quando chegam ao c2: a condição `_so_no_alcance_novo` não limita o bloco novo. Isso amplia a alteração de comportamento além dos intervalos e tensiona a promessa anterior de preservar os caminhos fora do alcance novo. O comentário novo declara explicitamente os quatro motivos; autorização específica dessa ampliação **[não verificado]**. (`app/chamados/reconciliador.py:332`; `app/chamados/reconciliador.py:770`; `app/chamados/reconciliador.py:843`; `app/chamados/reconciliador.py:851`; `app/chamados/reconciliador.py:688`; `app/chamados/reconciliador.py:840`)

- **(d) Selo sem mordida: nada demonstrado.** R2b estabelece veto real e exige retorno falso e ausência de chamado; R2 fornece o controle positivo sem veto. Entretanto, R2b cobre apenas régua suspeita, intervalo e nascimento sem chamado — não caracteriza C1, degenerada ou renascimento vetado. RED executado **[não verificado]**. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:174`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:186`)

- **Relógio fora da borda: nada. Escrita fora da porta: nada. Ordem de trava: nada introduzido.** O bloco usa o dia recebido, consulta leitores sem escrita/trava e retorna antes da porta existente. (`app/chamados/reconciliador.py:791`; `app/chamados/reconciliador.py:842`; `app/ponto/services/furos_vetados.py:19`; `app/escala/servico_jornada.py:38`; `app/chamados/models.py:666`)

Validação por execução da suíte: **[não verificado]**. [nao verificado]

## 30/10 14:34 · sessao 092cd5f1ccd9 · - · - · origem=Ronald (tmux)

- **(b) Nada no caminho sequencial com chamado já encerrado:** `_costura_no_vivo` só é verdadeiro para `VIVOS`; encerrado passa pelo novo veto antes de `abrir`, que depois poderia renascê-lo. A chave do teste e da abertura é a mesma (`app/chamados/reconciliador.py:795-799`, `:843-848`, `:853-854`; `app/chamados/models.py:670-688`).
- **Escrita fora da porta: nada introduzido.** O bloco acrescentado consulta vínculo/predicado e retorna; a busca do vínculo usa consultas de leitura, e o veto usa os predicados existentes (`app/chamados/reconciliador.py:842-848`; `app/ponto/services/furos_vetados.py:15-32`; `app/escala/servico_jornada.py:38-58`; `app/escala/regua_defesa.py:16-33`, `:554-562`).
- **Relógio fora da borda: nada introduzido.** A data entregue ao leitor é `_d_hz`, já calculada do argumento `dia`; não há consulta ao relógio no novo bloco nem nos leitores percorridos (`app/chamados/reconciliador.py:789-791`, `:845`; `app/ponto/services/furos_vetados.py:19-32`; `app/escala/regua_defesa.py:554-562`).
- **Ordem de trava: nada introduzido.** A leitura adicionada não adquire trava; a aquisição existente de `Colaborador` permanece dentro de `abrir` (`app/escala/servico_jornada.py:38-58`; `app/chamados/models.py:666-670`).

Limite relevante: `_costura_no_vivo` é um snapshot anterior à porta; `_ctx_c2` pode executar cartório antes da abertura, e o cartório pode encerrar chamados pelos marcos. Isso já existia; fuga efetiva do veto por esse encadeamento não foi demonstrada e não deve ser apresentada como achado confirmado (`app/chamados/reconciliador.py:798-799`, `:669-672`, `:863`; `app/ponto/services/cartorio.py:1163`, `:1174-1175`; fuga efetiva: [não verificado]).

## 30/10 14:34 · sessao 35744f769b6c · - · - · origem=Ronald (tmux)

- **(a) Divergência concreta: vínculo ausente.** `motivo_do_veto(..., ec=None, ...)` devolve `None`, enquanto `_abrir_chamado` chama `vetar_cobranca_se_degenerada`, que converte `ec=None` em template ausente e veta. A divergência já existe no leitor reutilizado; o bloco novo a herda. Referências: `app/ponto/services/furos_vetados.py:26`; `app/ponto/management/commands/detectar_ausencias.py:599`; `app/escala/regua_defesa.py:23`; `app/escala/regua_defesa.py:45`.

- **É um retorno previsto pelo código atual, inclusive havendo célula:** a juíza devolve `None` quando a data está fora da vigência da geradora. O c2 exige protesto cobravel e o veto da célula, mas este último só lê `trabalha`/`veredito`; o novo veto deixa passar `ec=None`. O cartório, em ambos os caminhos de emissão, recusa o vínculo ausente antes de chamar o funil. Referências: `app/escala/alimentacao.py:238`; `app/escala/alimentacao.py:250`; `app/chamados/reconciliador.py:825`; `app/chamados/reconciliador.py:845`; `app/ponto/marcos.py:155`; `app/ponto/services/cartorio.py:1347`; `app/ponto/services/cartorio.py:1447`. Ocorrência em dados reais: **[não verificado]**.

- **(a) Segundo juiz / vínculo / dia: nada adicional.** Para vínculo presente, o leitor chama os mesmos predicados do funil; a impressão usa exatamente `motivo_do_veto(c, vinculo_do_emissor(c,data),data)`. `vinculo_do_emissor` delega a `escala_vigente`, também usada nas duas emissões do cartório. Referências: `app/ponto/services/furos_vetados.py:15`; `app/ponto/services/furos_vetados.py:28`; `app/ponto/services/cartorio.py:35`; `app/ponto/services/cartorio.py:1013`; `app/ponto/services/cartorio.py:1347`; `app/ponto/services/cartorio.py:1446`.

- **(c) C1 muda efetivamente.** `orfao_14h` e `saida_sem_entrada` passam a ser vetados no nascimento/renascimento: `_so_no_alcance_novo` só controla a cerca posterior, não o bloco novo. Isso também alcança C1 fora da competência aberta quando chegar ao c2; não altera costura no chamado já vivo. Referências: `app/chamados/reconciliador.py:332`; `app/chamados/reconciliador.py:770`; `app/chamados/reconciliador.py:843`; `app/chamados/reconciliador.py:851`. O próprio comentário novo declara “vale para os 4 motivos”; concluir que esse alcance foi “não pedido” exige autorização externa **[não verificado]** (`app/chamados/reconciliador.py:840`).

- **Limite da comprovação:** revisão estática; testes não executados **[não verificado]**. [nao verificado]

## 30/10 14:34 · sessao a3c4ce7f8013 · - · - · origem=Ronald (tmux)

Nenhum defeito adicional encontrado nos pontos 4/5 ou nos novos testes, por inspeção estática: [nao verificado]

- **4 — Guarda removida e idempotência:** não há guarda `p.chamado_destino_id == ch_novo.pk` entre o retorno de `abrir` e a costura; permanecem a exigência de chamado vivo e a porta `reapontar_resolvedora` (`app/chamados/reconciliador.py:860`, `app/chamados/reconciliador.py:877`). Os dois chamadores reais entregam pergunta sem destino: filtro do cron (`app/chamados/reconciliador.py:1376`, `app/chamados/reconciliador.py:1398`) e condição do emissor (`app/chamados/services/disputa_emissao.py:404`, `app/chamados/services/disputa_emissao.py:409`). O teste exige primeira passada efetiva, destino vivo, segunda passada zero e identidade do estado da pergunta, chamados e número de logs (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:297`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:303`).
- **5 — R1 agora morde:** afirma pergunta viva e recusa `(None, None)`, exige mesmo chamado como destino e estado vivo; não aceita mais recusa como alternativa de sucesso (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:150`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:151`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:156`). R1b monta fechamento administrativo posterior ao fato, exige `admin_sem_fato_novo`, retorno falso e ausência de costura/materialização (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:123`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:163`).
- **3 — Testes de custo:** o caso unitário observa as funções reais e exige zero chamadas à cerca e à guarda externa; mover a cerca antes do protesto viola essas asserções (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:318`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:324`). Há teto de seis consultas para a porta e teto 31 no caminho real de `registrar_batida` (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:55`, `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:336`, `app/ponto/tests/test_selo_chokepoint_escrita.py:169`, `app/ponto/tests/test_selo_chokepoint_escrita.py:281`).
- **Selo/RED sem mordida:** nada demonstrado nos testes novos; execução, contagens empíricas e RED histórico não verificados. [nao verificado] [nao verificado]
- **1 — Corpo do commit:** não foi possível verificar `LEI-AKITA` em `22c08b8e`; o snapshot não disponibiliza `.git` nem artefato encontrado com esse corpo. [nao verificado]

Alerta já enviado ao agente c2: existe **log antes da cerca**, contrariando literalmente a afirmação de que todos os gates antecipados não têm efeito colateral: o veto de célula registra `_log.info` em `app/chamados/reconciliador.py:830`, enquanto a cerca só é consultada em `app/chamados/reconciliador.py:835`; a alegação está em `app/chamados/reconciliador.py:765`. A alteração de resultado persistido ainda depende da análise do agente c2. [nao verificado]

## 30/10 14:34 · sessao eea387bf13fd · - · - · origem=Ronald (tmux)

Não encontrei falha na extração do vigia nem duplicação de juiz/fonte na cerca e no HIST examinados. (`app/chamados/services/vigia_fio.py:419`; `app/chamados/reconciliador.py:711`; `app/chamados/reconciliador.py:1476`)

- **Vigia, dia presente:** continua compondo `janela_atual(dia, empresa)` com K6 e memo pela chave `(empresa.pk, fim.month, fim.year)`; o `bool()` acrescentado não altera o valor, pois K6 já retorna `vig is not None`. (`app/chamados/services/vigia_fio.py:435`; `app/chamados/services/vigia_fio.py:436`; `app/chamados/services/vigia_fio.py:440`; `app/folha/export.py:575`)
- **Vigia, dia ausente:** a função retorna `None`, que o chamador põe em `aberta`, como antes; não consulta janela/K6. (`app/chamados/services/vigia_fio.py:413`; `app/chamados/services/vigia_fio.py:431`)
- Avaliar `p.disputa.colaborador.empresa` também no caso sem data não introduz falha no universo normal: essa relação está carregada por `select_related` e a FK empresa é obrigatória. (`app/chamados/services/vigia_fio.py:405`; `app/colaboradores/models.py:321`)
- Universo, filtro por empresas, ordenação, teste de vitalidade e acumulação de PKs permanecem no chamador; a extração só decide o mesmo balde. (`app/chamados/services/vigia_fio.py:404`; `app/chamados/services/vigia_fio.py:406`; `app/chamados/services/vigia_fio.py:409`; `app/chamados/services/vigia_fio.py:415`)
- **Cerca:** sem `data_evento`/dia válido fica fora antes da composição; com data consulta a composição única e C7, preservando as autoridades existentes. (`app/chamados/reconciliador.py:705`; `app/chamados/reconciliador.py:713`; `app/chamados/reconciliador.py:716`)
- **HIST:** no alcance novo, pergunta viva elimina os candidatos encerrados; morta casa por `motivos_atendidos`; fora do alcance mantém o mapa anterior. (`app/chamados/reconciliador.py:1465`; `app/chamados/reconciliador.py:1466`; `app/chamados/reconciliador.py:1476`; `app/chamados/reconciliador.py:1479`)
- O desvio da viva do HIST para c1/c2 não a deixa ser carimbada pelo c1: este consulta `pergunta_viva` e retorna `False` antes da escrita. (`app/chamados/reconciliador.py:1491`; `app/chamados/reconciliador.py:398`; `app/chamados/reconciliador.py:402`)
- A prosa da cerca declara expressamente Pauta DP gerada pelo script de passivo **depois do pouso**, fora desta porta. (`app/chamados/reconciliador.py:692`; `app/chamados/reconciliador.py:693`)

Comparação estática com o diff fornecido; execução de testes não realizada. [nao verificado] [nao verificado]

## 30/10 14:33 · sessao 0456e283eb91 · - · - · origem=Ronald (tmux)

Achados por leitura estática; cenários não executados. [não verificado]

1. **O HIST novo pode alterar perguntas de competência entregue por efeito lateral do S128.** Uma `intervalo_saida` MORTA em competência aberta, com histórico único `batida_ausente`, passa a chamar S128; anteriormente não casava o mapa e c1/c2 recusavam intervalo. O S128 acrescenta **todas** as disputas abertas do colaborador, sem limitar dia/competência, e carimba perguntas MORTAS sem credor vivo — inclusive outra pergunta em competência entregue. Isso viola o alcance solicitado embora a entrada tenha consultado corretamente a competência. Fontes: `app/chamados/reconciliador.py:1436`, `:1450`, `:1487`, `:1493`; `app/chamados/services/disputa_emissao.py:1621`, `:1623`, `:1634`, `:1641`, `:1644`, `:1659`.

2. **MORTA respondida: a proteção do primeiro laço do S128 é desfeita pelo segundo.** Cenário: intervalo com texto/resposta carimbada, sem validação/materialização, sem testemunha de batida, célula decidida pelo admin com veredito que não acusa, disputa sem credor vivo e histórico encerrado único. O C6 devolve MORTA; o HIST novo chama S128. O primeiro laço pula pela proteção da resposta, mas o segundo seleciona a mesma pergunta e grava `materializada_em` sem repetir essa proteção. O próprio código documenta que esse carimbo impede o materializador real de consumir a resposta. Fontes: `app/chamados/juizes.py:1018`, `:1020`, `:1028`, `:1054`, `:1064`; `app/chamados/reconciliador.py:1450`, `:1493`; `app/chamados/services/disputa_emissao.py:1553`, `:1558`, `:1634`, `:1641`, `:1644`, `:1659`. **O defeito interno do S128 preexiste; a novidade é fazê-lo alcançar esse caso antes recusado.**

3. **A mesma cascata vence a proteção de competência trancada.** Com competência não entregue porém trancada, intervalo sem carimbo e histórico resolvido por via diferente de `competencia_trancada`, C6 devolve MORTA. HIST costura; S128 pula a pergunta no primeiro laço por tranca, mas o segundo carimba porque pergunta MORTA passa. Isso transforma silêncio reversível por leitura em carimbo persistido; além disso, a própria costura já ocorreu antes da guarda. Fontes: `app/chamados/juizes.py:879`, `:881`, `:1008`; `app/chamados/reconciliador.py:1487`, `:1493`; `app/chamados/services/fio.py:797`, `:799`; `app/chamados/services/disputa_emissao.py:1531`, `:1551`, `:1634`, `:1641`, `:1644`. Tranca e entrega usam fatos distintos: `app/ponto/services/fechamento.py:1432` e `app/folha/export.py:569`.

4. **c1 muda de alcance apesar de corpo intocado.** `saida_sem_entrada` MORTA/validada, sem resposta e sem turno aberto, com dois históricos `batida_ausente` do mesmo dia: antes nenhum casava e c1 lavrava; agora ambos casam, `len(hist)==2` impede c1 e retorna `False`. Com um histórico, muda a via para HIST/S128. A troca pelo juiz foi solicitada; portanto esse efeito deve ser reportado como mudança efetiva/caso descoberto, não automaticamente como violação independente. Fontes: `app/chamados/models.py:2468`, `:2469`; `app/chamados/juizes.py:1597`, `:1599`; `app/chamados/reconciliador.py:363`, `:365`, `:372`, `:399`, `:406`, `:1450`, `:1457`, `:1469`.

**Classes sem achado independente nesta leitura:**

- Segundo juiz/derivação paralela: **nada**; helpers consultam autoridades existentes — `app/chamados/reconciliador.py:700`, `:702`, `:703`, `:711`, `:712`; registros em `app/core/juizes.py:232`, `:275`, `:277`.
- Segundo escritor: **nada**; costuras continuam pela porta — `app/chamados/reconciliador.py:856`, `:1487`, `:1499`; exclusividade e escrita em `app/chamados/services/fio.py:743`, `:797`.
- Relógio novo fora da borda: **nada** nos helpers/diff — `app/chamados/reconciliador.py:697`, `:702`, `:1425`.
- Pouso antes do pré-requisito: **não comprovado**. B consta pousado; CORTES exige remedida e teto 343, mas o total 786 do teste inclui 461 exportadas e não permite sozinho concluir violação do teto. `app/docs/RELATO.md:3`, `:18`; `app/docs/CORTES.md:399`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:7`.

_527 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
