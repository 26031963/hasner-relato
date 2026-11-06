# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 09:28** · publicado em **06/11/2026 09:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 09:28 · sessao 61eb5622041b · E-SEM-VIVA-CACA · sonda · origem=Code

**O dado permite explicar os bloqueios atuais do col574 e apontar uma diferença de universo para o col950; não permite provar uma causa histórica comum aos 21 novos.** Os valores de produção abaixo vêm exclusivamente do texto fornecido, sem conferência independente: `[nao verificado]`.

**1. A sonda usa as chamadas reais?**

**Não é possível certificar os argumentos da sonda:** o texto contém a saída, mas não o conteúdo de `sonda_esemviva.py`; esse script também não foi localizado na cópia examinada. Portanto, divergências concretas **script × código** permanecem `[nao verificado]`.

Este é o contrato real contra o qual conferir o script:

| Função | Assinatura e chamada do sistema |
|---|---|
| `censo_alcance` | Assinatura `(empresas=None, hoje=None, limite=50, lista=None)`. O comando chama `censo_alcance(empresas, hoje=hoje, lista=lista)`, dentro de `tranca_em_lote()`. `app/chamados/services/vigia_fio.py:542`; `app/chamados/management/commands/vigiar_fio_morto.py:171-173`. |
| `ListaDoAdmin` | Recebe `(hoje, empresas=None, *, agora=None)`. O comando fornece o mesmo instante da rodada: `ListaDoAdmin(hoje, empresas, agora=agora)`. `app/chamados/services/vigia_fio.py:199`; `app/chamados/management/commands/vigiar_fio_morto.py:163-167`. |
| `motivos_retencao_celula` | Recebe `(colab, ini, fim, hoje=None, com_dias=False)`. O leitor de dias retidos usa `com_dias=True`, sem fornecer `hoje` nesse chamador. `app/folha/export.py:1168`; `app/folha/fora_da_folha.py:95-99`. |
| `InsumosDoDono` | Recebe `(colaborador, ini, fim, hoje, espelho=None, *, agora, dias_de_fase=None, cadastro=None)`. O comando E6 fornece `espelho=esp, agora=_agora_do_ato`. `app/ponto/management/commands/e6_oraculo.py:128`; `app/ponto/management/commands/e6_oraculo.py:316`. |
| `pergunta_viva` | Recebe o **objeto da pergunta**, cujo estado e relações são consultados pelo cálculo. A alimentação das portas chama esse juiz para as perguntas da célula. `app/chamados/juizes.py:822-848`; `app/chamados/juizes.py:1300-1303`. |

Passar `limite` maior explica uma listagem mais extensa, mas não muda os contadores nem omite dias sem porta: esses dias entram sempre. Isso, isoladamente, não seria divergência de classificação. `app/chamados/services/vigia_fio.py:551-553`; `app/chamados/services/vigia_fio.py:628-631`.

**2. Quem poderia emitir e por que não emite?**

A distinção decisiva é: **col574 tem julgamento `nunca_bateu`; col950 tem célula física sem julgamento**, conforme a saída fornecida. `[nao verificado]`

O motivo `sem_celula` da folha também designa **célula existente sem veredito**, quando `trabalha` está preenchido; portanto ele não prova ausência física de `CelulaDia`. `app/folha/export.py:1255-1256`.

| Candidato existente | Bloqueio demonstrável ou limite da conclusão |
|---|---|
| **Cartório — col574** | A primeira batida apurável define adesão. Sem nenhuma, entra no ramo explicitamente descrito como **“carimba e não emite”**, lavra `nunca_bateu` sem cobertura integral e retorna antes da emissão normal. Os campos fornecidos — zero batidas da vida, `nunca_bateu`, protestos vazios — correspondem a esse mecanismo. `app/ponto/services/cartorio.py:1154-1162`; `app/ponto/services/cartorio.py:1232-1242`; `app/ponto/services/cartorio.py:1268`. |
| **Cobrança normal do cartório — col574** | Além do retorno anterior, o caminho normal exclui desligados. O estado atual fornecido basta para barrá-lo; não prova qual era a situação na data de cada julgamento. `app/ponto/services/cartorio.py:1696-1697`; `app/ponto/services/cartorio.py:1762`. Histórico: `[nao verificado]`. |
| **Cartório — col950** | É o responsável existente por julgar células passadas: a fila contém todas elas, sem filtro de situação. Entretanto, o cron declarado só expande empresas **2, 3 e 4**; a empresa 1 fornecida está fora dessa expansão. Isso prova o não alcance pela configuração declarada, não qual execução histórica ocorreu. `app/ponto/management/commands/processar_cartorio.py:45-65`; `app/config/crons.py:17`; `app/config/crons.py:616`; `app/config/crons.py:1835`. |
| **Fábrica de perguntas pela célula** | Exige colaborador ativo e dia que o juiz não considere sem trabalho: col574 desligado é barrado; suas folgas também. Para col950, a fábrica depende dos motivos extraídos da ata; sem ata, o questionário se abstém quando existe vínculo. O retorno efetivo do vínculo e de `_marcos_faltantes` não foi impresso. `app/chamados/services/fabrica_da_celula.py:89-104`; `app/chamados/services/fabrica_da_celula.py:45-57`; `app/chamados/services/disputa_emissao.py:818-843`. Aplicação completa ao col950: `[nao verificado]`. |
| **Pergunta por chamados órfãos** | Recebe chamados, não dias retidos. Retorna se a entrada estiver vazia ou sem chamados regularizáveis. A contagem de chamados sem célula criados perto do dia não informa módulo, data canônica, status ou destino suficientes para provar que alcançariam aquele dia. `app/chamados/services/disputa_emissao.py:876-896`; `app/chamados/services/disputa_emissao.py:925-933`. Conteúdo desses chamados: `[nao verificado]`. |
| **Detector de ausência/batida** | Seleciona colaboradores em operação, exigindo ativo, empresa ativa, identidade operacional, rollout e vínculo vigente hoje. Col574 é excluído pela situação atual. Para col950, apenas estar ativo não comprova todas essas guardas. O detector percorre hoje/ontem e, depois do encerramento do dia, atribui a cobrança ao cartório. `app/ponto/management/commands/detectar_ausencias.py:98-99`; `app/colaboradores/queries.py:25-29`; `app/colaboradores/queries.py:65`; `app/ponto/management/commands/detectar_ausencias.py:136-138`; `app/ponto/management/commands/detectar_ausencias.py:460-473`. |
| **Funil de cobrança de furo** | O veto da célula cala `trabalha=False` e `veredito=nunca_bateu`. Isso cobre as folgas e os dias de trabalho do col574 descritos no dado, mesmo numa chamada direta ao funil. `app/ponto/marcos.py:119`; `app/ponto/marcos.py:155-162`; `app/ponto/management/commands/detectar_ausencias.py:630-633`. |
| **Adesão** | O universo exige **ativo + `nunca_bateu`**; também exclui quem já possui batida apurável. Col574 é desligado; col950 já bateu e não apresenta esse veredito. Há ainda a regra expressa “desligado não adere a nada”. `app/chamados/services/adesao.py:32-34`; `app/chamados/services/adesao.py:60-67`. |
| **Lista DP/Cadastro x Realidade — C5** | É o encaminhamento escrito para quem nunca bateu, mas o carregador exclui desligados. Além disso, C5 é uma assinatura do colaborador, sem campo `data`; não insere seus dias em `dias_na_lista`. `app/escala/services/cadastro_realidade.py:72-79`; `app/escala/services/cadastro_realidade.py:151-164`; `app/escala/services/cadastro_realidade.py:955-970`. |
| **Turno aberto, corrente ou retroativo** | Exige turno materializado aberto e entrada; o retroativo exige também timestamp da entrada. Zero batidas na vida do col574 não fornece esse fato. Os turnos materializados do col950 não foram impressos; a batida da vizinhança de 09/10 não prova sua atribuição ao turno. `app/ponto/management/commands/processar_alertas_turno.py:76-93`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88-103`. Turnos efetivos: `[nao verificado]`. |
| **Chamado de ausência** | Recebe uma `Ausencia` já existente; não converte automaticamente dia sem batida em ausência. `ausencia_id=None` na célula não prova inexistência de registro bruto ou submissão. `app/ponto/services/ausencia.py:621-646`. Ausências não lavradas: `[nao verificado]`. |
| **Consumidor de `EventoCelula`** | Rejunta, retrata, encerra órfãs e fecha cobrança por lastro. O processamento do evento não significa que deveria criar uma porta para um dia vazio. `app/chamados/reconciliador.py:1346-1371`. |
| **`sem_batidas_mes` / `reconciliar_grade`** | O primeiro emissor está aposentado por escrito; o segundo é medidor na rotina, com escrita reservada à bancada dirigida. Encontrá-los no grep não os torna responsáveis atuais pelos dias. `app/ponto/services/fechamento.py:1089-1093`; `app/ponto/management/commands/reconciliar_grade.py:1-11`. |

Para o **Resolver dia**, a competência corrente não recebe `pode_ultimo_recurso`; a função também exige passado e status `falta`, `irregular`, `aberto` ou `sem_certidao`. Isso explica o bloqueio dos dias desde 21/10 e, pelo status de folga, das quatro folgas antigas do col574, contra os campos fornecidos. `app/colaboradores/services/calendario.py:94-104`; `app/colaboradores/services/calendario.py:738-739`.

Para os dias antigos do col950, `status_dia=None` bruto não basta para explicar o `False`: falta o item efetivo do calendário, incluindo presença do dia, `status` e `pode_ultimo_recurso`. A porta também responde `False` quando o calendário não devolve o dia. `app/colaboradores/services/calendario.py:97-104`; `app/colaboradores/services/calendario.py:132-136`.

**3. Quem escreve e quem lê os campos decisivos?**

“Pergunta viva”, “dono fora do fio” e “motivo de retenção” são resultados produzidos por funções; não são campos independentes que algum emissor simplesmente preenche. `app/chamados/juizes.py:822-848`; `app/chamados/services/vigia_fio.py:274-315`; `app/folha/export.py:1218-1222`.

| Campo ou resultado | Escritor/produtor | Leitor que decide a classe |
|---|---|---|
| Existência da pergunta, `data_evento`, `celula`, destino | `PerguntaDisputa.abrir` localiza/reaponta célula e destino e cria a pergunta. `app/chamados/models.py:2428-2477`. | `alimentar_portas` agrupa perguntas da célula e testa vitalidade; o censo recebe esse mapa. `app/chamados/juizes.py:1300-1303`; `app/chamados/services/vigia_fio.py:610-616`. |
| `resposta_colab`, `respondida_em` | Porta de resposta. `app/chamados/services/disputa_emissao.py:1282-1301`. | Vitalidade exige carimbo e texto para o ramo de resposta. `app/chamados/juizes.py:1150-1162`. |
| `validada_em` | Validação escreve; nova resposta desfaz validação anterior. `app/chamados/services/validacao.py:127-133`; `app/chamados/services/disputa_emissao.py:1285-1297`. | Primeiro degrau da vitalidade. `app/chamados/juizes.py:1135-1137`. |
| `materializada_em`, `via_resolucao` | Portas de carimbo, identidade e descarimbo. `app/chamados/models.py:2532-2543`; `app/chamados/models.py:2625-2644`. | Vitalidade lê via, carimbo e testemunho do marco/dia. `app/chamados/juizes.py:1145-1146`; `app/chamados/juizes.py:1170-1214`. |
| “Pergunta viva” | `pergunta_viva` delega ao cálculo; a property `viva` delega ao mesmo juiz. `app/chamados/juizes.py:822-848`; `app/chamados/models.py:2763-2773`. | Porta alcançável e mapa usado pelo censo. `app/chamados/juizes.py:1373-1383`; `app/chamados/services/vigia_fio.py:610`. |
| Colaborador e dia na lista do admin | `cadastro_realidade.lista` monta linhas/assinaturas; `dias_na_lista` exige assinatura com `data` e resolve seu dono. `app/escala/services/cadastro_realidade.py:792-857`; `app/escala/services/cadastro_realidade.py:950-970`. | `ListaDoAdmin` guarda separadamente conjunto de colaboradores e mapa de dias; a classificação consulta ambos. `app/chamados/services/vigia_fio.py:216-218`; `app/chamados/services/vigia_fio.py:222-251`; `app/chamados/services/vigia_fio.py:661-686`. |
| “Resolver dia” | Calendário produz último recurso; juiz exige passado/status permitido. `app/colaboradores/services/calendario.py:738-739`; `app/colaboradores/services/calendario.py:97-104`. | Porta plural e `dono_fora_do_fio`. `app/colaboradores/services/calendario.py:132-136`; `app/chamados/services/vigia_fio.py:306-307`. |
| Dono fora do fio | Ordem: Resolver oferecido → ausência cobrindo em `ABERTAS` → aceite de folga validado → dia na lista do admin. `app/chamados/services/vigia_fio.py:306-315`. | `censo_alcance` consulta esse resultado antes de atribuir causa. `app/chamados/services/vigia_fio.py:654-659`. |
| Dono E6 mostrado na sonda | `InsumosDoDono.dono` monta insumos e chama `dono_da_divergencia`. `app/ponto/management/commands/e6_oraculo.py:172-183`. | **Não entra diretamente na classe:** o vigia lê o dono presente na lista. `app/chamados/services/vigia_fio.py:313-314`. |
| Veredito, protestos, ata e status da célula | Porta de lavra grava esses dados. `app/ponto/portas/celula.py:809-817`. | Retenção lê veredito/protestos; vitalidade também lê veredito e marco da ata. `app/folha/export.py:1233-1256`; `app/chamados/juizes.py:1177-1196`. |
| Motivo e dias de retenção | `motivos_retencao_celula` produz ambos na mesma decisão. `app/folha/export.py:1218-1256`. | `dias_que_seguram` fornece o universo ao censo. `app/folha/fora_da_folha.py:95-99`; `app/chamados/services/vigia_fio.py:582-587`. |

**`ESTRUTURA` não identifica um emissor faltante:** no E6 é o resíduo das outras condições, e zero batidas é expressamente contagem par. `app/ponto/management/commands/e6_oraculo.py:69-75`; `app/ponto/management/commands/e6_oraculo.py:98-110`.

**4. Existe causa comum aos 21 novos?**

**Não foi demonstrada.** O dado divide os novos em **20 do col574 e 1 do col950**, mas não fornece o estado anterior de cada porta nem o evento responsável pela mudança. `[nao verificado]`

Os mecanismos atuais são distintos:

- **col574:** retenção por veredito acusador com protestos vazios, combinada com silêncio de cobrança/adesão e exclusão da C5 por desligamento. `app/folha/export.py:1233-1254`; `app/ponto/services/cartorio.py:1157-1162`; `app/escala/services/cadastro_realidade.py:151-154`.
- **col950:** retenção por célula sem veredito; seu novo dia 05/11 também passou a ser passado em 06/11, condição que o retentor e o vigia usam. Isso torna o avanço de data compatível com esse único novo, sem provar o histórico da execução. `app/folha/export.py:1255-1256`; `app/chamados/services/vigia_fio.py:648-650`. Causa histórica: `[nao verificado]`.

Não se pode afirmar que col574 foi desligado entre as duas sondas, nem que o cancelamento de `sem_batidas_mes` provocou a entrada dos vinte: faltam situação anterior, trilha e conteúdo do chamado. `[nao verificado]`

**5. Classificação**

| Colaborador | Classificação sustentada |
|---|---|
| **col574 — 20 dias** | **LEI FALTANDO quanto à atribuição específica de encaminhamento do desligado que nunca bateu.** As regras existentes mandam calar cobrança, excluem desligado da adesão e da C5, enquanto a retenção continua lendo seus dias até a demissão. Não encontrei regra específica que determine qual emissor deve abrir uma porta substituta nessa combinação. `app/ponto/services/cartorio.py:1157-1162`; `app/chamados/services/adesao.py:66-67`; `app/escala/services/cadastro_realidade.py:134-154`; `app/folha/export.py:1198-1201`. Ausência de outra atribuição escrita: `[nao verificado]`. |
| **col950 — 19 dias** | **INCONCLUSIVO sobre o mecanismo histórico.** Há responsável escrito: cartório para células passadas. Há diferença comprovada de universos: cron somente 2/3/4 e censo por empresas visíveis. Falta estabelecer a obrigação operacional da empresa 1 e se houve chamada efetiva do cartório para ela; se houve, falta saber se a grade devolveu esses dias ou ocorreu falha. `app/ponto/management/commands/processar_cartorio.py:45-65`; `app/config/crons.py:17`; `app/config/crons.py:1835`; `app/chamados/services/vigia_fio.py:567-585`; `app/ponto/services/cartorio.py:1365-1367`. |

Essa classificação **não significa ausência de regra geral de alcance**: há corte escrito exigindo fio para lâmpada acusadora, encaminhando dia sem fio ao Admin e qualificando o contador positivo como bug. A falha de alcance está registrada no dado fornecido; o que não ficou provado, pelo critério mais específico da pergunta, é **um emissor individual desobedecendo sua própria obrigação neste caso**. `app/docs/CORTES.md:263`; `app/docs/CORTES.md:265`; dado atual `[nao verificado]`.

Para fechar o diagnóstico faltam: **o script da sonda; os argumentos/estado da rodada de 05/11; a execução efetiva do cartório para empresa 1; e, por dia do col950, a saída da grade e do calendário usados pelos juízes.** Os pontos que consomem essas informações estão em `app/chamados/management/commands/vigiar_fio_morto.py:163-173`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1365-1367` e `app/colaboradores/services/calendario.py:132-136`. Disponibilidade dessas medições: `[nao verificado]`.

## 06/11 09:26 · sessao 984f6712ccf6 · G1-PORTA · sonda · origem=Code

**Os dados não provam que seguir as portas libera os cinco casos.** O juiz da porta responde por onde alcançar a célula; o export decide a situação do **colaborador na competência**, e não de cada dia isoladamente. ([folha/export.py:757](app/folha/export.py:757), [chamados/juizes.py:1341](app/chamados/juizes.py:1341))

**1. A sonda reproduz as chamadas reais?**

Não é possível certificar: o corpo de `sonda_g1porta.py` não consta do material recebido/localizado; os argumentos efetivamente usados permanecem **[não verificado]**.

Os pontos concretos de comparação são:

| Função | Chamada/contrato do sistema |
|---|---|
| `motivos_retencao_celula` | `_dias_que_retem` passa colaborador, início e fim, com `com_dias=True`, **sem `hoje` explícito**; a janela vem de `janela_fechamento`. ([fora_da_folha.py:95](app/folha/fora_da_folha.py:95), [fora_da_folha.py:118](app/folha/fora_da_folha.py:118)) |
| Alimentação e porta | Alimenta IDs de células e colaboradores; calcula verbos sobre **todos os `qs_vivos()`**, não somente sobre os casos selecionados; consulta a porta com `alimentacao=alim`. Recortar previamente os chamados pode mudar o crédito das perguntas. ([chamados/juizes.py:1267](app/chamados/juizes.py:1267), [chamados/juizes.py:1287](app/chamados/juizes.py:1287), [vigia_fio.py:635](app/chamados/services/vigia_fio.py:635)) |
| `InsumosDoDono` | E6 passa colaborador, janela, `HOJE`, `espelho=esp` e `agora` comum. A classe também aceita carregar o espelho sob demanda. ([e6_oraculo.py:128](app/ponto/management/commands/e6_oraculo.py:128), [e6_oraculo.py:154](app/ponto/management/commands/e6_oraculo.py:154), [e6_oraculo.py:316](app/ponto/management/commands/e6_oraculo.py:316)) |
| `censo_alcance` | O comando compartilha `ListaDoAdmin(hoje, empresas, agora=agora)` e chama o censo dentro de `tranca_em_lote()`. ([vigiar_fio_morto.py:163](app/chamados/management/commands/vigiar_fio_morto.py:163), [vigiar_fio_morto.py:167](app/chamados/management/commands/vigiar_fio_morto.py:167), [vigiar_fio_morto.py:171](app/chamados/management/commands/vigiar_fio_morto.py:171)) |

Há uma diferença de apresentação a conferir: o censo tem `limite=50`; a saída recebida informa lista integral. Ampliar esse limite altera a listagem, não o universo nem o contador. O argumento usado pela sonda é **[não verificado]**. ([vigia_fio.py:542](app/chamados/services/vigia_fio.py:542), [vigia_fio.py:625](app/chamados/services/vigia_fio.py:625))

A diferença semântica importante é o uso do dono: **o E6 real só consulta `.dono(data)` depois de identificar divergência**, passando pelos descartes anteriores. A sonda recebida não apresenta essa comparação; equivalência com esse fluxo é **[não verificado]**. ([e6_oraculo.py:330](app/ponto/management/commands/e6_oraculo.py:330), [e6_oraculo.py:353](app/ponto/management/commands/e6_oraculo.py:353), [e6_oraculo.py:366](app/ponto/management/commands/e6_oraculo.py:366))

**2. Quem escreve e quem lê?**

| Objeto | Quem produz/escreve | Quem lê |
|---|---|---|
| **Datas retentoras** | `motivos_retencao_celula`, pelo `_retem`, acrescenta data, motivo, célula e veredito ao retorno. Seu insumo persistido é a célula; o escritor único de veredito/protestos é `lavrar_veredito`. ([export.py:1218](app/folha/export.py:1218), [escala/models.py:1819](app/escala/models.py:1819)) | `_dias_que_retem` → `dias_que_seguram`; o censo enumera essas datas. `fora_da_folha` também as entrega ao retrato. ([fora_da_folha.py:98](app/folha/fora_da_folha.py:98), [vigia_fio.py:585](app/chamados/services/vigia_fio.py:585), [fora_da_folha.py:192](app/folha/fora_da_folha.py:192)) |
| **Porta de cada data** | `alimentar_portas` carrega fila, verbos e perguntas; `porta_alcancavel_do_dia` produz em memória `rota`, `fila`, `verbos` e `fio`. ([chamados/juizes.py:1282](app/chamados/juizes.py:1282), [chamados/juizes.py:1386](app/chamados/juizes.py:1386)) | `_fio_do_dia` da folha e `censo_alcance`. ([fora_da_folha.py:77](app/folha/fora_da_folha.py:77), [vigia_fio.py:635](app/chamados/services/vigia_fio.py:635)) |
| **Dono E6** | `InsumosDoDono.dono` monta os insumos e delega a `dono_da_divergencia`; o juiz devolve CADASTRO/BATIDA/ESTRUTURA. ([e6_oraculo.py:172](app/ponto/management/commands/e6_oraculo.py:172), [e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py:98)) | E6 contabiliza o retorno; pautas e teto também o consomem. ([e6_oraculo.py:366](app/ponto/management/commands/e6_oraculo.py:366), [pautas_discordantes.py:109](app/ponto/management/commands/pautas_discordantes.py:109), [teto_na_lista.py:86](app/ponto/services/teto_na_lista.py:86)) |

**O `dono` do retrato é outro conceito:** guarda o **fio agregado** da pessoa, não o dono E6. As datas e esse fio são persistidos pelo lavrador do retrato. ([retrato_folha.py:1183](app/folha/retrato_folha.py:1183), [lavrar_retrato_folha.py:74](app/folha/management/commands/lavrar_retrato_folha.py:74))

**3. Por caso, o admin consegue liberar?**

`cobrar` significa aguardar resposta do colaborador e depois validação; `validar` oferece ato do admin; `decidir` exige julgamento humano. Nenhum desses verbos equivale a liberação garantida. ([fora_da_folha.py:40](app/folha/fora_da_folha.py:40), [verbos.py:80](app/chamados/services/verbos.py:80))

A tabela aplica esses significados aos dados recebidos; sucesso dos atos e resultado posterior são **[não verificado]**.

| Caso / dia indicado | Portas das retentoras informadas | O que se pode concluir |
|---|---|---|
| **col504 — 21/10** | 29/10: fila, **cobrar** | Depende de resposta do colaborador; o admin sozinho não conclui o fluxo indicado. **[não verificado]** |
| **col709 — 22/10** | 30/10: fila, **validar**; 02, 03 e 04/11: fila, **cobrar** | Há uma validação e três dependências do colaborador. O dono ESTRUTURA de 02/11 não transforma seu verbo em ação de liberação. **[não verificado]** |
| **col204 — 21/09** | 29/09: fila, **validar** | Há ato disponível ao admin; aceitar a validação e eliminar a retenção não foram demonstrados. **[não verificado]** |
| **col43 — 26/10** | 01/11: pergunta, **cobrar** | Depende de resposta do colaborador e posterior validação. **[não verificado]** |
| **col502 — 09/10** | 28/09: pergunta, **cobrar**; 07/10: fila, **cobrar**; 10 e 14/10: fila, **decidir**; 18/10: fila, **cobrar** | Três cobranças e dois julgamentos; seguir uma porta não resolve as demais. **[não verificado]** |

Além disso, a porta não recebe usuário e não certifica autorização individual; a validação real pode recusar a operação. ([chamados/juizes.py:1341](app/chamados/juizes.py:1341), [views_cobrar.py:24](app/chamados/views_cobrar.py:24), [validacao.py:363](app/chamados/services/validacao.py:363))

Mesmo removidas todas as retenções de espelho, o export ainda verifica homologação; `turno_aberto` pertence à pergunta separada de aprovação/tranca. ([export.py:788](app/folha/export.py:788), [export.py:825](app/folha/export.py:825), [export.py:966](app/folha/export.py:966))

**Tela/leitor existente:** o drawer do colaborador, aberto com competência, lê a linha gravada do retrato e mostra **“Datas (ISO)”**. Portanto, existe apresentação **colaborador/competência → datas retentoras**. ([colaboradores/views.py:975](app/colaboradores/views.py:975), [colaboradores/views.py:982](app/colaboradores/views.py:982), [_retrato_o_que_fazer.html:43](app/templates/folha/_retrato_o_que_fazer.html:43))

Esse bloco não apresenta uma relação explícita **dia afetado X → datas Y/Z**; seus links recebem o colaborador, não cada retentora. A presença e atualização dos cinco casos nesse retrato são **[não verificado]**. ([retrato_folha.py:1177](app/folha/retrato_folha.py:1177), [retrato_folha.py:1020](app/folha/retrato_folha.py:1020))

A ferramenta `o_que_segura` fornece cada retentora, fio, ação e chamados, também por colaborador/competência. Uma negativa sobre **todas** as telas mostrarem a relação específica entre dias permanece **[não verificado]**. ([fora_da_folha.py:203](app/folha/fora_da_folha.py:203), [fora_da_folha.py:227](app/folha/fora_da_folha.py:227))

**4. Por que fora do censo e ESTRUTURA? Qual classificação?**

- **Fora do censo:** ele enumera exclusivamente as datas que **causam** retenção, retornadas por `dias_que_seguram`; não expande o bloqueio mensal para todos os dias da janela. Isso corresponde à regra escrita “dias que seguram a folha”. ([vigia_fio.py:585](app/chamados/services/vigia_fio.py:585), [CORTES.md:379](app/docs/CORTES.md:379))
- **ESTRUTURA:** é o retorno residual do juiz após excluir os ramos de cadastro/batida/fase. A função não exige previamente uma diferença de valores; essa exigência pertence ao chamador E6. Logo, obter ESTRUTURA numa data não comprova defeito nela. ([e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py:98), [e6_oraculo.py:172](app/ponto/management/commands/e6_oraculo.py:172), [e6_oraculo.py:353](app/ponto/management/commands/e6_oraculo.py:353))

**Classificação: INCONCLUSIVO quanto à liberação; BUG não provado por essa combinação.** A exclusão do dia afetado é compatível com o universo escrito; o dono isolado não demonstra incoerência contra a L-099. ([CORTES.md:379](app/docs/CORTES.md:379), [CLAUDE.md:176](CLAUDE.md:176), [e6_oraculo.py:110](app/ponto/management/commands/e6_oraculo.py:110))

Para concluir faltam: script integral da sonda; resultado dos atos nas retentoras; nova leitura das retentoras/export; e, para acusar divergência ESTRUTURA, comparação dos valores do mesmo dia. Tudo isso está **[não verificado]**.

Não há **LEI FALTANDO demonstrada**: o corte existente define alcance das causas. Uma obrigação adicional de exibir cada dia afetado ligado às suas retentoras precisaria de regra específica, cuja existência permanece **[não verificado]**. ([CORTES.md:379](app/docs/CORTES.md:379), [CORTES.md:401](app/docs/CORTES.md:401))

## 06/11 09:25 · sessao 892efc794fa0 · - · - · origem=Ronald (tmux)

Achados consolidados, todos somente leitura:

| Candidato | Condição no código e aplicação aos casos |
|---|---|
| Cartório, ramo sem adesão | A primeira batida apurável define `adesao`; nenhuma batida entra no ramo cuja regra escrita é “carimba e nao emite”. Esse ramo lavra `nunca_bateu` sem cobertura integral e retorna antes da emissão normal. Aplica-se ao `col574` conforme o dado fornecido. `app/ponto/services/cartorio.py:1154-1162`, `:1232-1242`, `:1268`. |
| Cobrança normal do cartório | Exclui expressamente `situacao == 'desligado'`, tanto para furo inteiro quanto parcial. Também exige código de furo, ausência de chamado vivo/piso e vínculo/dia de trabalho/marco inicial. O desligamento basta para impedir `col574`; os demais gates não devem ser inventados. `app/ponto/services/cartorio.py:1684`, `:1696-1697`, `:1714-1742`, `:1762`. |
| Adesão | `universo_adesao` exige situação `ativo` e célula com `veredito='nunca_bateu'`. O sincronizador reforça “desligado nao adere a nada”. Assim, `col574` é excluído pela situação; `col950` não tem o veredito exigido e já bateu. `app/chamados/services/adesao.py:25-40`, `:58-67`, `:74-86`. |
| Detector de ausência/batida | Universo passa por `colaboradores_em_operacao`, que exige ativo, empresa ativa, username operacional, rollout e vínculo vigente hoje. Examina hoje/ontem e só cobra antes de o dia encerrar; depois a atribuição escrita é do cartório. `col574` é excluído por desligamento; para `col950`, rollout/identidade/vínculo hoje não foram impressos. `app/colaboradores/queries.py:25-28`, `:50-65`; `app/ponto/management/commands/detectar_ausencias.py:98-99`, `:136-138`, `:305-306`, `:460-473`. |
| Funil de cobrança | A célula veta cobrança por `trabalha=False` ou `veredito='nunca_bateu'`; o funil real lê esse juiz. Isso impede cobrar as folgas e os nunca-bateu do `col574`, mesmo se chamado diretamente. `app/ponto/marcos.py:119`, `:155-162`; `app/ponto/management/commands/detectar_ausencias.py:630-633`. |
| Fábrica de perguntas por chamados | Não percorre dias retidos autonomamente: recebe chamados órfãos, retorna se lista vazia ou sem regularizáveis. Mesmo havendo chamado, sem ata e com vínculo existente abstém-se de perguntar. Para os chamados sem célula que a sonda apenas conta, faltam módulo, contexto/data canônica, status e destino para determinar se alcançariam os dias. `app/chamados/services/disputa_emissao.py:876-896`, `:925-933`, `:818-843`. |
| Turno aberto | Emissor corrente exige turno materializado aberto com entrada; retroativo exige turno aberto e timestamp da entrada. Dias sem batida não criam tal fato. Para `col950` em 09/10 a única batida da vizinhança é de 08/10: atribuição ao turno exige medir a autoridade, não usar a vizinhança como prova. `app/ponto/management/commands/processar_alertas_turno.py:76-98`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88-103`. |
| Ausência do colaborador | O emissor recebe uma `Ausencia` real já criada; não transforma automaticamente silêncio de batida em documento/ausência. API e formulário o chamam após criar registro. `ausencia_id=None` na célula não prova inexistência de ausência crua ou submissão. `app/ponto/services/ausencia.py:621-628`, `:643-646`; `app/api/views.py:1848-1857`; `app/ponto/views.py:2535-2547`. |
| `sem_batidas_mes` | Emissor aposentado por escrito desde 02/07, por duplicata sem dono; painel de folha registra inconsistências. O único cancelado de `col574` não pode ser identificado como causa histórica dos 20 dias. `app/ponto/services/fechamento.py:1089-1093`. |
| `reconciliar_grade` | O grep encontra um escritor de bancada, mas módulo declara comando como medidor sem escritor rotineiro; `--apply` ficou para sombra dirigida. Ainda exigiria ativo e leria o veto da célula. `app/ponto/management/commands/reconciliar_grade.py:1-11`, `:106`, `:501-504`. |
| Consumidor `EventoCelula` | Rejunta chamados, retrata perguntas/chamados/premissas, encerra órfãs e fecha por lastro. Não constitui emissor autônomo de porta para dia vazio. Evento processado não prova que havia obrigação de criar chamado/pergunta. `app/chamados/reconciliador.py:1346-1371`. |

Ponto decisivo para `col950`: o cartório declarado no cron é `por_empresa=True`, mas o universo estático é somente `(2,3,4)`. Portanto a configuração da cópia não agenda o cartório para a empresa 1. A expansão usa essa tupla. Isso explica um caminho estrutural de não alcance; não prova qual execução histórica ocorreu. O próprio contrato nomeia esse universo como empresas em operação, então a mera existência de `col950` na empresa 1 não basta para provar violação de uma regra que a inclua. `app/config/crons.py:10-17`, `:616`, `:1835`.

Se o cartório for chamado para empresa 1, sua fila é toda célula passada, sem filtro de situação. Célula sem julgamento não é pulada pela idempotência; ainda pode ser pulada se o dia não aparecer na grade carregada. Para fechar o mecanismo de `col950`, faltam o resultado dessa grade, vínculo alimentado e execução real do comando. `app/ponto/management/commands/processar_cartorio.py:45-65`; `app/ponto/services/cartorio.py:1358-1367`.

Para classificação: em `col574`, há regra explícita para calar cobrança e adesão; não encontrei regra específica atribuindo a algum emissor a dívida de pessoa desligada que nunca bateu, embora o dia permaneça retido. Isso sustenta **LEI FALTANDO quanto à atribuição de emissão**, sem converter o “esperado 0” em emissor inventado. `app/ponto/services/cartorio.py:1157-1162`, `:1268`; `app/chamados/services/adesao.py:66-67`; `app/chamados/services/vigia_fio.py:532-534`.

Em `col950`, há atribuição existente ao cartório para dia encerrado, mas seu cron declarado exclui empresa 1; falta regra incluindo a empresa 1 no universo operacional, além de execução/grade para estabelecer a causa concreta. **INCONCLUSIVO**, com divergência de universos comprovada na configuração. `app/ponto/management/commands/detectar_ausencias.py:460-464`; `app/ponto/management/commands/processar_cartorio.py:45-65`; `app/config/crons.py:17`, `:616`, `:1835`.

## 06/11 09:25 · sessao dd1bfbb8a56f · - · - · origem=Ronald (tmux)

**1. Paridade da sonda**

O arquivo `sonda_esemviva.py` não foi encontrado na cópia pelo levantamento de nomes; o prompt contém a saída, mas não o script. Portanto, argumentos efetivamente usados pela sonda e qualquer divergência script × código são **[nao verificado]**.

O contrato real a conferir no script é:

- `censo_alcance(empresas=None, hoje=None, limite=50, lista=None)` em `app/chamados/services/vigia_fio.py:542`.
- O command captura um instante, deriva `hoje`, cria `ListaDoAdmin(hoje, empresas, agora=agora)` e chama `censo_alcance(empresas, hoje=hoje, lista=lista)` dentro de `tranca_em_lote()`: `app/chamados/management/commands/vigiar_fio_morto.py:163-173`.
- `ListaDoAdmin.__init__(hoje, empresas=None, *, agora=None)`: `app/chamados/services/vigia_fio.py:199-203`. Carrega visibilidade e passa `lista(hoje=self.hoje, empresas=vis, agora=self.agora)`: `app/chamados/services/vigia_fio.py:211-218`.
- `motivos_retencao_celula(colab, ini, fim, hoje=None, com_dias=False)`: `app/folha/export.py:1168`; produção usa `com_dias=True`, sem passar `hoje`, em `app/folha/fora_da_folha.py:95-99`. Universo de retenção vem de janela do juiz e `classificar_export`: `app/folha/fora_da_folha.py:118-137`. Assim, passar `hoje` explicitamente na sonda pode ser legítimo para congelar medição, mas não é a mesma forma literal desse chamador.
- `InsumosDoDono(colaborador, ini, fim, hoje, espelho=None, *, agora, dias_de_fase=None, cadastro=None)`: `app/ponto/management/commands/e6_oraculo.py:128-146`; `dono(data)` carrega abertura, fase e cadastro e pergunta ao juiz: `:172-183`.
- `pergunta_viva(p)` recebe **objeto PerguntaDisputa**, não ID/estado textual: `app/chamados/juizes.py:822-848`. `alimentar_portas` usa todas as perguntas da célula e chama esse juiz: `:1300-1303`.

**2. Por que a classe é exatamente `e/sem_pergunta_viva`**

A porta exige chamado **da célula** na fila, ou pergunta viva daquela célula, no app e ancorada na fila: `app/chamados/juizes.py:1341-1349`, implementação `:1373-1383`. Chamados sem célula contados por proximidade de criação não satisfazem essa regra por esse único fato: `:1344-1348`.

Depois de ausência de porta e dia passado, o vigia consulta os donos externos: `app/chamados/services/vigia_fio.py:635-659`. Na ausência deles, não havendo pergunta viva, nem dia na lista, nem colaborador na lista, ele termina em `e/sem_pergunta_viva`: `:661-686`. Não há regra aqui que deduza qual emissor deixou de agir; esse vigia apenas classifica: `:542-553`, `:695-699`.

**3. Escritores e leitores dos campos decisivos**

| Campo/resultado | Quem escreve/produz | Quem lê |
|---|---|---|
| Pergunta existe; `celula`, `data_evento`, destino | Gate `PerguntaDisputa.abrir`: procura célula e identidade, reaponta e cria: `app/chamados/models.py:2398-2477` | `alimentar_portas` agrupa por célula e chama `pergunta_viva`: `app/chamados/juizes.py:1300-1303`; `censo_alcance` usa esse mapa em `app/chamados/services/vigia_fio.py:610-616` |
| `resposta_colab` + `respondida_em` | `responder_pergunta_disputa_core`: `app/chamados/services/disputa_emissao.py:1282-1301` | `app/chamados/juizes.py:1150-1162` |
| `validada_em` | Validação humana: `app/chamados/services/validacao.py:127-133`; resposta nova desfaz: `app/chamados/services/disputa_emissao.py:1285-1297`; auto-validação é porta do carimbo conforme `app/core/tests/test_contract_escritores_estado.py:133-147` | Primeiro degrau da vitalidade: `app/chamados/juizes.py:1135-1137` |
| `materializada_em` + `via_resolucao` | Portas `carimbar`, `carimbar_identidade`, `descarimbar`; contrato e propósito: `app/chamados/models.py:2532-2543`; identidade escreve e salva em `:2625-2644` | `app/chamados/juizes.py:1145-1146`, `:1170-1179`, `:1207-1214` |
| “pergunta viva” | É resultado de leitura, não campo gravado: memo delega ao cálculo em `app/chamados/juizes.py:822-848`; regras `:1135-1214` | Property `viva` delega ao mesmo juiz: `app/chamados/models.py:2763-2773`; `estado` traduz a leitura em `:2492-2506` |
| Dia e dono na lista do admin | `cadastro_realidade.lista` junta assinaturas e monta as linhas: `app/escala/services/cadastro_realidade.py:792-857`; harmoniza dono via juiz em `:897-935`; `dias_na_lista` só aceita assinatura com **data** e resolve dono: `:950-970` | `ListaDoAdmin._carregar` lê mapas e conjunto de colabs: `app/chamados/services/vigia_fio.py:216-218`; dia/admin em `:222-236`; colab em `:248-251`; classe lê ambos em `:661-686` |
| Dono L-099 exibido pela sonda | `InsumosDoDono.dono` compõe insumos: `app/ponto/management/commands/e6_oraculo.py:172-183`; juiz tem ordem cadastro explícito → turno aberto real → ímpar → fase → CxR → estrutura: `:98-110` | Esse `dono=ESTRUTURA` não é argumento direto de `censo_alcance`; o vigia lê dono **da lista**: `app/chamados/services/vigia_fio.py:313-314`, `:654-656` |
| Dono fora do fio | Composição ordenada de Resolver dia, ausência aberta, folga validada e lista: `app/chamados/services/vigia_fio.py:274-315` | `censo_alcance`: `:654-659` |
| “Resolver dia” | Campo montado pelo calendário: último recurso se competência encerrada e sem pergunta viva respondida: `app/colaboradores/services/calendario.py:455-475`, `:738-739`; juiz exige passado e status permitido: `:94-104` | Porta plural lê o mesmo campo ou devolve False se dia não veio no calendário: `:117-136`; vigia usa em `app/chamados/services/vigia_fio.py:258-271`, `:306-307` |
| Retenção | Função produz motivo e dia: `app/folha/export.py:1218-1222`; decide por veredito/protestos e cegueira em `:1230-1256` | `dias_que_seguram`: `app/folha/fora_da_folha.py:95-99`, `:118-137`; vigia toma esse universo: `app/chamados/services/vigia_fio.py:582-587` |
| `veredito`, protestos, ata, status | `CelulaDia.lavrar_veredito` delega: `app/escala/models.py:1819-1828`; porta escreve protestos/veredito/via/em e ata/status: `app/ponto/portas/celula.py:809-817`; status aplica em `:743-748` | Retenção: `app/folha/export.py:1233-1256`; vitalidade: `app/chamados/juizes.py:1180-1196` |

**4. Achados particulares relevantes**

**col574:** há lei explícita para nunca bateu: encaminhar à pauta/lista do DP por adesão/cadastro, pois perguntar a quem nunca usou o app não chega a ninguém: `app/escala/services/cadastro_realidade.py:72-79`. Porém o carregador C5 exclui expressamente desligados: `:151-154`, e sua docstring afirma isso como regra do portão: `:134-136`. A retenção não exclui por situação; corta apenas por admissão/demissão: `app/folha/export.py:1198-1205`, e `nunca_bateu` sem protestos continua retendo em `:1233-1254`.

Mesmo para um ativo, a C5 que esse código fabrica é assinatura de **colaborador**, sem `data` e `dono_do_dia`: `app/escala/services/cadastro_realidade.py:159-164`. Ela pode pôr o colaborador na lista, mas não põe cada dia no mapa `dias_na_lista`, porque esse mapa exige `data`: `:955-969`. Não confundir o dono adesão da C5 com o dono L-099 ESTRUTURA; são respostas de funções distintas: `:72-79` e `app/ponto/management/commands/e6_oraculo.py:69-75`.

Os dias de novembro de ambos não oferecem Resolver dia por serem da competência corrente (o calendário escreve que aí o caminho é chamado → pergunta → colaborador): `app/colaboradores/services/calendario.py:455-458`, `:738-739`. As folgas de outubro de col574 também não têm status em `STATUS_QUE_PEDEM_DECISAO`: `:94-104`.

**col950:** `sem_celula` na retenção não significa necessariamente ausência física de CelulaDia; uma célula existente sem veredito e `trabalha` preenchido também recebe esse motivo: `app/folha/export.py:1255-1256`. Isso corresponde aos campos apresentados pelo usuário **[nao verificado: saída fornecida sem arquivo:linha]**.

Para seus dias antigos de outubro, não se deve explicar `resolver_dia_oferecido=False` apenas por `status_dia=None`: esse é campo bruto; o calendário usa status calculado/lavrado do D4 e último recurso, não só aquele campo: `app/colaboradores/services/calendario.py:97-104`, `:122-136`. Faltaria medir, por dia, o item de `contexto_calendario`: `status`, `pode_ultimo_recurso`, presença do dia, `_ini_comp_atual` e perguntas respondidas que o calendário considera: `:132-136`, `:455-475`, `:738-739`.

**5. Limite da causa comum aos novos**

A atual saída distingue dois mecanismos: col574 é retido por veredito acusador sem protestos (`app/folha/export.py:1233-1254`), col950 por ausência de julgamento (`:1255-1256`). A ausência de porta é efeito comum (`app/chamados/services/vigia_fio.py:635-686`), mas uma causa histórica comum da entrada dos 21 entre 05 e 06/11 não é demonstrável sem o estado/argumentos da medição anterior e histórico das mudanças **[nao verificado]**. Em particular, situação atual “desligado” não prova que col574 mudou de situação nesse intervalo **[nao verificado]**.

## 06/11 09:25 · sessao d93523c2419c · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **Dono L-099 não é atribuição de emissor.** `dono_da_divergencia` retorna ESTRUTURA como resíduo; zero batidas é explicitamente contagem par e pode ser ESTRUTURA. Portanto `dono=ESTRUTURA` não prova que faltou cobrança. `app/ponto/management/commands/e6_oraculo.py:69-75`, `:98-110`.
- **Contrato de `InsumosDoDono`:** `(colaborador, ini, fim, hoje, espelho=None, *, agora, dias_de_fase=None, cadastro=None)`; a chamada real do comando passa `espelho=esp, agora=_agora_do_ato`. Carrega batidas `[ini−12h, fim+1d+14h]`, turnos `[ini−1d,fim+1d]`, cadastro, fase e CxR; `dono(data)` chama o juiz. `app/ponto/management/commands/e6_oraculo.py:128-183`, `:316`. Sem script da sonda, equivalência de seus argumentos é `[nao verificado]`.

**Resolver dia / dono fora do fio**

- A regra escrita proíbe “Resolver dia” na competência corrente; seu caminho declarado é chamado → pergunta → colaborador. O calendário escreve `pode_ultimo_recurso` somente quando `data < início_competência_atual` e não existe pergunta viva respondida. O juiz exige também passado e status `falta|irregular|aberto|sem_certidao`. `app/colaboradores/services/calendario.py:455-467`, `:738-739`, `:94-104`.
- Isso explica, contra os campos fornecidos, as quatro folgas antigas col574: `status_dia=folga` não satisfaz o juiz; seus dezesseis dias 21/10–05/11 e o novo 05/11 col950 estão na competência corrente fornecida, bloqueando último recurso. A identidade entre `status_dia` da célula e status efetivo do calendário deve ser validada pelo resultado real já impresso `resolver_dia_oferecido=False`; os dois campos não devem ser simplesmente equiparados. `app/colaboradores/services/calendario.py:103-104`, `:738-739`; campos medidos sem arquivo da sonda `[nao verificado]`.
- `dono_fora_do_fio` lê, nesta ordem: Resolver oferecido; ausência cobrindo em ABERTAS; pergunta VALIDADA de aceite de folga; dia presente na lista do admin. Sem nenhum, devolve None. Nunca consulta diretamente o dono E6; o dono vem da lista. `app/chamados/services/vigia_fio.py:274-315`.

**Emissores candidatos**

| Candidato | Guarda / aplicação |
|---|---|
| `detectar_ausencias` | Seleciona `colaboradores_em_operacao`; esta consulta exige ativo, empresa ativa, usuário CPF, empresa em rollout e vínculo cobrindo hoje. Col574 desligado é excluído hoje; col950 ativo só comprova uma das condições, demais `[nao verificado]`. `app/ponto/management/commands/detectar_ausencias.py:98-99`; `app/colaboradores/queries.py:25-29`, `:50-65`. |
| Mesmo detector | Só percorre hoje e ontem; folga é pulada; não cobra após `dia_encerrado`, pois dali em diante atribui ao cartório. Portanto não repara retrospectivamente os dias antigos. `app/ponto/management/commands/detectar_ausencias.py:136-141`, `:305-306`, `:460-473`. |
| Funil `_abrir_chamado` | Celula veto/nunca_bateu é perguntado em `celula_veta_cobranca`; sem célula física tem outro aviso `dia_sem_grade`. Col950 **tem** CelulaDia, então motivo folha `sem_celula` não autoriza afirmar que este ramo de aviso dispararia. `app/ponto/management/commands/detectar_ausencias.py:630-668`. |
| Adesão | Seu universo requer ativo + célula `nunca_bateu`; exclui quem já bateu/isento e tem guarda expressa desligado. Col574 é desligado; col950 não tem veredito nunca_bateu e tem batidas. `app/chamados/services/adesao.py:25-40`, `:60-70`. |
| Turno aberto corrente | Lê turnos materializados de até 36h, exige aberto e entrada; col574 zero batidas não pode produzir entrada real. A presença/ausência de turno materializado do col950 não foi impressa `[nao verificado]`. `app/ponto/management/commands/processar_alertas_turno.py:46-79`, `:88-93`. |
| Turno aberto avançado | Só ontem, exige em_uso, vínculo e horas de turno aberto; não é detector de dia inteiro sem nenhuma batida. `app/ponto/management/commands/processar_alertas_avancados.py:20-38`, `:49-65`. |
| Turno aberto retroativo | Exige turno aberto e entrada com timestamp, depois ≥24h. Nada disso foi medido como turno no anexo; inexistência de batidas no recorte não deve substituir o juiz do turno. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88-103`. |
| Furo retroativo | Filtra ativo e exige alguma batida na janela; col574 excluído pelas duas condições. No col950 exige que `classificar_dia` devolva FURO_SEM_COBRANCA; sem executar sua classificação, emissão concreta `[nao verificado]`. Também exige dia trabalho sabido e marco inicial. `app/ponto/management/commands/emitir_furo_retroativo.py:52-56`, `:101-112`, `:130-145`. |
| Chamado de ausência | Recebe registro de ausência concreto; é aviso persistente de ausência criada pelo colaborador, não criador automático de falta por ausência de batida. Célula `ausencia_id=None` não é prova de inexistência de toda ausência cobrindo o período `[nao verificado]`. `app/ponto/services/ausencia.py:621-646`. |
| Disparo de perguntas, fio mudo | Universo é chamado VIVO ligado à célula; com zero chamados da célula não entra. Não-ativo também é barrado expressamente. `app/chamados/management/commands/disparar_perguntas_competencia.py:141-150`, `:169-171`. |

**Mandatos a enfrentar na classificação**

- O cartório declara julgar toda célula passada sem carimbo, sem filtro de situação; o lote seleciona todos colaboradores com células passadas e passa suas células a `julgar_colab`. Logo há regra escrita responsável por julgar col950; não é defensável dizer apenas “nenhuma lei manda julgar”. Falta saber execução, limite, retornos e exceções para provar por que ele permaneceu com tudo NULL. `app/ponto/management/commands/processar_cartorio.py:1-13`, `:45-65`.
- Há corte explícito “lâmpada acesa no marco = fio vivo obrigatório”, que manda censar o emissor que pulou e curar nele; há também corte incluindo dias sem fio no Admin e declarando o alarme >0 bug provado. Entretanto o caso desligado/nunca_bateu possui guardas expressas de silêncio e RH, então é necessário confrontar escopo antes de escolher “lei faltando”. `app/docs/CORTES.md:263`, `:265`; `app/chamados/services/adesao.py:66-67`; `app/chamados/reconciliador.py:804-811`.
- **Causa histórica comum aos 21:** o retrato atual permite identificar que 17 são da competência corrente e quatro são folgas antigas, com impedimentos distintos de Resolver dia; não demonstra o evento que fez 20 col574 entrarem desde 05/11. Estado anterior, data efetiva do desligamento e execução do emissor anterior não foram fornecidos `[nao verificado]`. As guardas atuais estão em `app/colaboradores/services/calendario.py:103-104`, `:738-739`.

## 06/11 09:25 · sessao b33ecbc3dc1f · - · - · origem=Ronald (tmux)

- **O censo enumera causas de retenção, não todo dia afetado pelo bloqueio da competência.** Ele chama `dias_que_seguram`, insere exclusivamente as datas retornadas e só consulta portas para essas chaves; por isso um dia afetado, mas ausente das retentoras, não aparece no universo nem entra no contador sem porta. (`app/chamados/services/vigia_fio.py:577`, `:585`, `:587`, `:635`, `:638`)

- **Isso corresponde ao contrato explícito local:** o comentário define a pergunta como “o dia que SEGURA a folha tem uma porta”; a autoridade das datas é `dias_que_seguram`, e a soma das rotas fecha esse universo. A lei escrita da porta é chamado da célula na fila OU pergunta viva no app com âncora na fila. (`app/chamados/services/vigia_fio.py:493`, `:502`, `:517`, `:531`; `app/docs/CORTES.md:401`)

- **Quem produz e lê a porta:** `alimentar_portas` lê `qs_na_fila`, particiona verbos sobre **todos** `qs_vivos`, carrega perguntas da célula e aplica `pergunta_viva`; a leitura do app é sob demanda. `porta_alcancavel_do_dia` monta uma resposta em memória com rota, fila visível, verbos e fio; seus leitores incluem `fora_da_folha._fio_do_dia` e o censo. (`app/chamados/juizes.py:1282`, `:1287`, `:1290`, `:1300`, `:1302`, `:1319`, `:1375`, `:1380`, `:1386`; `app/folha/fora_da_folha.py:77`; `app/chamados/services/vigia_fio.py:635`)

- **A alimentação deve preservar o universo global dos verbos.** Particionar somente os chamados/células dos casos pode mudar o crédito da pergunta; o comentário explica que destino vivo fora da fila pode vencer o pai e que a autoridade é a lista padrão sem filtros. Não há código da sonda disponível aqui para conferir se ela respeitou isso. (`app/chamados/juizes.py:1267`, `:1271`, `:1287`; disponibilidade integral da sonda: **[não verificado]**)

- **Produção chama o censo com lista e contexto compartilhados:** o command calcula `agora/hoje`, instancia `ListaDoAdmin(hoje, empresas, agora=agora)` e executa `censo_alcance(empresas, hoje=hoje, lista=lista)` dentro de `tranca_em_lote()`. O padrão do censo é `limite=50`, que só limita apresentação de dias com porta/balde legal, sem limitar o universo ou dias sem porta. A saída fornecida com 2.127 linhas/zero omitidas exige limite ampliado ou outra apresentação; a chamada exata da sonda permanece **[não verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:159`, `:163`, `:167`, `:171`, `:173`; `app/chamados/services/vigia_fio.py:542`, `:551`, `:625`, `:628`)

- **Há uma sutileza de relógio:** embora `censo_alcance` receba `hoje`, chama `dias_que_seguram` sem esse argumento; `_dias_que_retem` também chama `motivos_retencao_celula` sem `hoje`. Não concluir divergência da sonda sem seu script; comparar se usou a mesma janela e data efetiva. (`app/chamados/services/vigia_fio.py:585`; `app/folha/fora_da_folha.py:98`, `:118`, `:137`)

- **`porta=s` não prova liberação pelo admin.** A função não recebe usuário e não aplica autorização individual; os endpoints exigem `gerir_chamados` e `recusa_de_area`. Cobrar envia push e registra nota, aguardando resposta do colaborador; Validar pode recusar por implausibilidade ou falta de resposta, e depois materializa/recalcula pela data da pergunta. (`app/chamados/juizes.py:1341`, `:1375`, `:1386`; `app/chamados/views_cobrar.py:24`, `:25`, `:31`, `:44`, `:55`, `:145`, `:149`, `:157`; `app/chamados/services/validacao.py:363`, `:374`, `:396`, `:407`, `:430`)

- **Leitura dos cinco casos:** cobrar significa depender do colaborador e depois validar; validar indica uma oportunidade de ato do admin; decidir exige julgamento humano. Assim col504 e col43 dependem de resposta; col709 mistura validar com três cobrar; col204 tem validar; col502 mistura três cobrar com dois decidir. A conclusão sobre os atos necessários segue os verbos fornecidos, mas sucesso após execução, autorização do admin e reclassificação final são **[não verificado]**. (`app/chamados/services/verbos.py:20`, `:22`, `:80`; `app/folha/fora_da_folha.py:40`)

- **Existe UI mostrando retentoras por colaborador/competência:** o drawer com `mes/ano` busca `RetratoFolha` gravado, localiza a linha e renderiza “Datas (ISO)”. Mostra as datas retentoras da **pessoa**, sem estabelecer nesse bloco uma aresta de um dia afetado específico para suas dependências. Os links são genéricos por fio/tipo, apontando ao fio, vínculo ou cadastro. Se o retrato gravado contém as datas da medição atual, permanece **[não verificado]**. (`app/colaboradores/views.py:973`, `:975`, `:980`, `:982`, `:984`; `app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/templates/folha/_retrato_o_que_fazer.html:43`, `:46`; `app/folha/retrato_folha.py:974`, `:986`, `:1015`, `:1020`)

- **Não afirmar “nenhum leitor mostra datas”.** `o_que_segura` é ferramenta existente que traz as retentoras e o que destrava; o drawer mostra datas gravadas. A afirmação mais estreita é: não identifiquei nesses leitores uma apresentação explícita “dia retido X depende de Y/Z”; uma negativa sobre **todas** as telas do sistema permanece **[não verificado]**. (`app/folha/fora_da_folha.py:203`, `:231`; `app/templates/folha/_retrato_o_que_fazer.html:43`)

- **Classificação sugerida:** exclusão do dia afetado do censo, isoladamente, não prova bug contra o contrato existente, pois o universo escrito é das retentoras. Se a exigência for listar também cada dia afetado e sua relação com as retentoras, essa obrigação precisa ser localizada em regra escrita; não está no corte que define a porta. A garantia de liberação dos cinco casos permanece inconclusiva sem resultado dos atos e reavaliação do export. (`app/chamados/services/vigia_fio.py:493`, `:502`, `:585`; `app/docs/CORTES.md:401`; sucesso final: **[não verificado]**)

## 06/11 09:25 · sessao 651fd7a6e630 · - · - · origem=Ronald (tmux)

O leitor existente mostra **colaborador + competência → datas retentoras**, não uma relação explícita **dia “retido” → outras datas**: a linha gravada contém `colab`, `status`, `motivo`, `dias`, `datas` e `dono`, sem chave para o dia afetado; `montar_linhas` obtém essas datas de `fora_da_folha`. (`app/folha/retrato_folha.py:1156`, `app/folha/retrato_folha.py:1171`, `app/folha/retrato_folha.py:1177`)

A cadeia efetiva da tela é:

- A lista do retrato abre o drawer com colaborador e `mes`/`ano`. (`app/templates/folha/_retrato_lista.html:60`)
- A view exige competência, acesso `ver_folha` e retrato acessível; busca a linha gravada, monta ações e portas. (`app/colaboradores/views.py:975`, `app/colaboradores/views.py:980`, `app/colaboradores/views.py:982`)
- O drawer inclui o bloco “O que fazer” quando existe `retrato_linha`. (`app/templates/colaboradores/partials/painel_colaborador.html:26`)
- O bloco mostra **“Datas (ISO)”** e percorre `retrato_linha.datas`; sem lista, informa que a fonte não deu lista de datas. (`app/templates/folha/_retrato_o_que_fazer.html:42`, `app/templates/folha/_retrato_o_que_fazer.html:46`, `app/templates/folha/_retrato_o_que_fazer.html:49`)
- “O que destrava” vem de `DESTRAVA`, pelo fio agregado da linha. Os links também são por colaborador, escolhidos pelo fio e tipo; não recebem cada data retentora. (`app/folha/retrato_folha.py:1057`, `app/folha/retrato_folha.py:1068`, `app/folha/retrato_folha.py:1015`, `app/folha/retrato_folha.py:1020`)

Logo, seria incorreto dizer **“nenhuma tela mostra aquelas datas”**: o drawer tem leitor para elas. A presença e atualização desses cinco casos no retrato gravado não foram dadas pela sonda. **[não verificado]**

A ferramenta `o_que_segura(colab, mes, ano)` fornece dados ainda mais específicos: data, veredito, motivo, fio, ação e chamados da fila, mas continua sendo uma pergunta por colaborador/competência. (`app/folha/fora_da_folha.py:203`, `app/folha/fora_da_folha.py:217`, `app/folha/fora_da_folha.py:225`, `app/folha/fora_da_folha.py:227`, `app/folha/fora_da_folha.py:229`) Não encontrei uma view chamando essa ferramenta; a documentação interna afirma que o drawer a lê, porém a view efetiva lê o retrato gravado. (`app/folha/retrato_folha.py:1218`, `app/colaboradores/views.py:978`, `app/colaboradores/views.py:982`)

Outros leitores relevantes:

- O calendário lê o status diário lavrado pelo D4; o código declara que retenção da folha não decide a palavra da tela. (`app/colaboradores/services/calendario.py:597`, `app/colaboradores/services/calendario.py:602`, `app/colaboradores/services/calendario.py:606`)
- O Raio-X seleciona pessoas pelo export mensal, mas obtém suas datas de `furos_por_dia`, não de `motivos_retencao_celula(com_dias=True)`; o clique abre chamados por colaborador. (`app/folha/views.py:589`, `app/folha/views.py:591`, `app/folha/views.py:616`, `app/folha/views.py:667`)
- A prévia verifica se as datas propostas cobrem **todas** as retentoras e mostra “passaria a entrar” ou “continua fora”; também é efeito por colaborador, não aresta entre dias. (`app/folha/previa.py:119`, `app/folha/previa.py:169`, `app/folha/previa.py:171`, `app/folha/previa.py:173`)

As regras escritas relevantes exigem mesma autoridade para leitores e ação, e coerência com o cadastro existente; não encontrei nelas exigência expressa de apresentar a aresta dia afetado → datas retentoras. (`app/docs/LEIS.md:60`, `app/docs/LEIS-UI.md:23`, `CLAUDE.md:176`) Declarar essa ausência específica como violação provada dessas regras seria **[não verificado]**.

## 06/11 09:25 · sessao cbf261424aa8 · BO-0511-G2-TRIAGEM · censo · origem=Code

**Padrão: n/a (medida).** Para citar o dado fornecido, uso `anexo_dados_g2.txt:N`, contando `== A) relato raspado...` como linha 1, inclusive linhas vazias. É numeração da transcrição colada.

Os status abaixo têm estes significados: `irregular` indica batida faltando, fora do horário ou em ordem inesperada; `furo` integra os vereditos que acusam essa irregularidade. `escala_suspeita`, associada a `indefinida`, expressa dúvida cadastral sobre o dia de trabalho, sem afirmar falta do colaborador. A fonte `lavrado` lê o status persistido na célula. (`app/ponto/status_dia.py:32`, `app/ponto/status_dia.py:38`, `app/ponto/status_dia.py:68`, `app/ponto/status_dia.py:129`, `app/ponto/status_dia.py:143`, `app/ponto/status_dia.py:287`.)

A retenção é outra pergunta: `dias_que_seguram` seleciona a classificação `furo_espelho` e consulta os motivos por dia, independentemente da porta. O retentor registra `espelho_` + veredito acusador quando não incidem as exceções de protestos não cobráveis ou furo somente da pausa. C.d demonstra esse resultado nos cinco dias; protestos e ata individuais não foram fornecidos [nao verificado]. (`app/folha/fora_da_folha.py:95`, `app/folha/fora_da_folha.py:102`, `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`; `app/folha/export.py:1233`, `app/folha/export.py:1253`, `app/folha/export.py:1254`.)

O dono é respondido por **`dono_da_divergencia`**, com insumos montados por **`InsumosDoDono.dono`**. C.b informa o retorno, mas não os argumentos que distinguem os ramos. BATIDA pode resultar de `batida_fora_de_turno` ou contagem ímpar das horas atribuídas pelo oráculo; CADASTRO pode resultar da prioridade folga/vínculo, fase deslocada ou Cadastro×Realidade. O ramo individual exato permanece [nao verificado]. Uma trava `turno_aberto` não demonstra o insumo `batida_fora_de_turno`, que exige também órfã real atribuída ao dia. (`app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:104`, `app/ponto/management/commands/e6_oraculo.py:106`, `app/ponto/management/commands/e6_oraculo.py:108`, `app/ponto/management/commands/e6_oraculo.py:110`, `app/ponto/management/commands/e6_oraculo.py:172`, `app/ponto/management/commands/e6_oraculo.py:196`.)

| Caso | Col | Dia | Classe: vigia / status / motivo do segura | Dono de C e ramo | Obra/lei existente: ID, estado e alcance | Arquivo:linha |
|---|---|---|---|---|---|---|
| **G2-01** | col407 | 21/10/2026 | **B, 05/11:** **a — `chamado_vivo_com_veu`**, detalhe `registrado`. **C, 06/11:** porta nula e #33290 `registrado` satisfazem o véu **se o dia chegar à cadeia a–e**; os baldes anteriores não foram expostos, portanto classificação completa atual [nao verificado]. **Lavra:** `furo` / `irregular`. **Segura:** `espelho_furo`, pela regra de retenção acima. | **BATIDA**; ramo `batida_fora_de_turno` (:104) ou paridade ímpar (:106); qual ocorreu [nao verificado]. | **O261-FIO-MORTO-PELO-RETRATO — ABSORVIDA; “curas no ar”**: escopo expresso de dia retido sem porta, incluindo causa a; alcança a medição B. **L-099 — PELA-METADE**: enquadra o dono informado, BATIDA na lista do admin. | Dado: `anexo_dados_g2.txt:18`, `anexo_dados_g2.txt:31`. Classe: `app/chamados/services/vigia_fio.py:654`, `app/chamados/services/vigia_fio.py:664`, `app/chamados/services/vigia_fio.py:669`. Escopo: `app/docs/BACKLOG.md:963`; `app/docs/LEIS.md:122`. |
| **G2-02** | col836 | 13/10/2026 | **B:** **e — `pergunta_viva_sob_chamado_terminal`**. **C:** porta nula e chamado terminal não informam vitalidade, resposta, texto ou âncora; permanência dessa classe em 06/11 [nao verificado]. **Lavra:** `indefinida` / `escala_suspeita`. **Segura:** `espelho_indefinida`. | **CADASTRO**; prioridade cadastral (:98), fase (:108) ou Cadastro×Realidade (:110); ramo exato [nao verificado]. | **O261-E-CELULA-20-UM-DONO — F0 FECHADA; F1 NO AR; F2 NO AR 01/11; F3 PARADA 02/11**: escopo explícito de pergunta viva sob chamado terminal, demonstrado por B. **L-099 — PELA-METADE**: CADASTRO na lista do admin. | Dado: `anexo_dados_g2.txt:21`, `anexo_dados_g2.txt:32`. Classe: `app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:687`. Escopo: `app/docs/BACKLOG.md:859`; `app/docs/LEIS.md:122`. |
| **G2-03** | col836 | 14/10/2026 | **B:** **e — `pergunta_viva_sob_chamado_terminal`**, medido separadamente do dia 13. **C:** classe atual [nao verificado], pelos mesmos campos ausentes. **Lavra:** `indefinida` / `escala_suspeita`. **Segura:** `espelho_indefinida`. | **CADASTRO**; ramos possíveis :98, :108 ou :110; ramo exato [nao verificado]. | **O261-E-CELULA-20-UM-DONO**, com os mesmos estados escritos acima: B demonstra o escopo também neste dia. **L-099 — PELA-METADE**. | Dado: `anexo_dados_g2.txt:22`, `anexo_dados_g2.txt:33`. Classe: `app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:687`. Escopo: `app/docs/BACKLOG.md:859`; `app/docs/LEIS.md:122`. |
| **G2-04** | col909 | 16/10/2026 | **B:** colaborador ausente da lista de dias sem porta; rota daquele instante [nao verificado]. **C:** **`porta_fila`**, #31891, verbo **`validar`**, fio **`admin`**; não entra nos ramos a–e, pois a porta antecede essa classificação. **Lavra:** `furo` / `irregular`. **Segura:** `espelho_furo`, mesmo havendo porta. | **BATIDA**; ramo :104 ou :106; qual ocorreu [nao verificado]. | **L-099 — PELA-METADE** alcança o dono medido. **Nenhuma obra específica encontrada com enquadramento demonstrado.** **C20-JUIZ-3 — FECHADA / NO AR** trata de evento em `criar()/recompute_turnos`; a queixa e C não demonstram esse evento causal [nao verificado]. | Dado: `anexo_dados_g2.txt:28`, `anexo_dados_g2.txt:34`. Porta: `app/chamados/services/vigia_fio.py:642`; `app/chamados/juizes.py:1327`, `app/chamados/juizes.py:1344`. Escopo: `app/docs/LEIS.md:122`; `app/docs/BACKLOG.md:1768`. |
| **G2-05** | col292 | 21/09/2026 | **B:** não contém este dia; as causas **d** de 22/10 e 28/10 não se transferem para 21/09. **C:** célula presente, porta nula e #24460 `fechado`; faltam perguntas, âncoras e lista para determinar a–e [nao verificado]. **Lavra:** `furo` / `irregular`. **Segura:** `espelho_furo`. | **BATIDA**; ramo :104 ou :106; qual ocorreu [nao verificado]. | **O261-COMPLEMENTO-[nome]**, estado escrito **“(C) = cura (2) PAI-TERMINAL em copia”; “(B) materializada sem resposta 2.970 a construir”**. A linha cita expressamente **col292 21/09** e define alcance da porta. Vincular as perguntas citadas nessa documentação ao conteúdo atual de C [nao verificado]. **L-099 — PELA-METADE**. | Dado: `anexo_dados_g2.txt:16`, `anexo_dados_g2.txt:17`, `anexo_dados_g2.txt:35`. Classe d: `app/chamados/services/vigia_fio.py:677`; demais condições: `app/chamados/services/vigia_fio.py:673`. Escopo: `app/docs/BACKLOG.md:782`; `app/docs/LEIS.md:122`. |
| **G2-06** | col207 | **sem dia** | A relata outubro retido, novembro apta e dez chamados vivos, incluindo disputa #36736 sem célula. Classe do vigia, lavra e motivo diário do segura [nao verificado]. | C não informa dono; dono e ramo [nao verificado]. | **Nenhuma encontrada com enquadramento individual demonstrado.** A queixa não demonstra ausência de pergunta viva nem evento abrangido por **C20-FIO-MUDO — ABERTA / “construtor aberto”**; aplicação [nao verificado]. | `anexo_dados_g2.txt:10`, `anexo_dados_g2.txt:36`; escopo candidato: `app/docs/BACKLOG.md:1769`. |
| **G2-07** | col332 | **sem dia** | A relata retenção 10/11 e treze chamados vivos de batida ausente/turno aberto. Classe, lavra e motivo diário do segura [nao verificado]. | C não informa dono; dono e ramo [nao verificado]. | **Nenhuma encontrada com enquadramento individual demonstrado.** **C20-FIO-MUDO — ABERTA / “construtor aberto”** tem escopo por evento; chamados vivos, sozinhos, não demonstram sua ocorrência [nao verificado]. | `anexo_dados_g2.txt:11`, `anexo_dados_g2.txt:37`; escopo candidato: `app/docs/BACKLOG.md:1769`. |
| **G2-08** | col446 | **sem dia** | A relata retenção 10/11 e seis chamados vivos de batidas ausentes, incluindo #36459–#36462. Classe, lavra e motivo diário do segura [nao verificado]. | C não informa dono; dono e ramo [nao verificado]. | **Nenhuma encontrada com enquadramento individual demonstrado.** Aplicação de **C20-FIO-MUDO — ABERTA** [nao verificado]. **O261-FIO-MORTO-PELO-RETRATO — ABSORVIDA** cita col446/#11582, mas a ligação com esta queixa sem dia [nao verificado]. | `anexo_dados_g2.txt:12`, `anexo_dados_g2.txt:38`; `app/docs/BACKLOG.md:1769`, `app/docs/BACKLOG.md:963`. |

**F3-BECO-1 não fica confirmado para os dois dias de col836:** a linha existente **F3-BECO-1-VOLTA-3 — NO AR 05/11 17:5x** exige a transição do defeito de folga para `False` e trata do reatamento a pai na fila. Esses insumos não constam de C [nao verificado]. (`app/docs/BACKLOG.md:1775`; `anexo_dados_g2.txt:32`, `anexo_dados_g2.txt:33`.)

**Contagem por dono:** BATIDA **3**; CADASTRO **2**; ESTRUTURA **0**; sem dono informado **3**. **Soma = 8.** (`anexo_dados_g2.txt:31` a `anexo_dados_g2.txt:38`.)

**Contagem “obra existente × nenhuma”, com enquadramento demonstrado:** obra específica **4**; nenhuma obra específica **4** — G2-04 possui apenas o enquadramento pela L-099. **Soma = 8.** Incluindo **obra ou lei**, como na coluna solicitada: existente **5**; nenhuma encontrada **3**. **Soma = 8.** (`app/docs/BACKLOG.md:782`, `app/docs/BACKLOG.md:859`, `app/docs/BACKLOG.md:963`, `app/docs/LEIS.md:122`; dados individuais citados na tabela.)

## 06/11 09:25 · sessao 1a1cffb2681a · - · - · origem=Ronald (tmux)

**Dono não é prova de defeito:** `dono_da_divergencia` devolve `ESTRUTURA` como resíduo, após excluir cadastro, batida e fase; aceita inclusive ausência de horas. [app/ponto/management/commands/e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py:98), [app/ponto/tests/test_e6_janela_declarada_nao_e_divergencia.py:143](app/ponto/tests/test_e6_janela_declarada_nao_e_divergencia.py:143)

O comando E6 real primeiro comprova divergência: lê minutos, descarta ausência de lavra, ambos zero, diferença explicada por intervalo e diferença ≤10 minutos; somente então chama `.dono(data)`. Portanto, chamar `.dono` para um dia meramente retido pelo export responde uma classificação residual; não comprova que esse dia esteja divergente. [app/ponto/management/commands/e6_oraculo.py:330](app/ponto/management/commands/e6_oraculo.py:330), [app/ponto/management/commands/e6_oraculo.py:366](app/ponto/management/commands/e6_oraculo.py:366)

**Argumentos e fontes:** assinatura real `InsumosDoDono(colaborador, ini, fim, hoje, espelho=None, *, agora, dias_de_fase=None, cadastro=None)`; carrega batidas apuráveis de `[ini−12h, fim+1d+14h]`, turnos de `[ini−1d, fim+1d]`, cadastro do dia, fase lavrada e CxR do espelho/motor. A resposta `.dono(data)` não consulta retenção nem porta. [app/ponto/management/commands/e6_oraculo.py:128](app/ponto/management/commands/e6_oraculo.py:128), [app/ponto/management/commands/e6_oraculo.py:142](app/ponto/management/commands/e6_oraculo.py:142), [app/ponto/management/commands/e6_oraculo.py:172](app/ponto/management/commands/e6_oraculo.py:172)

O E6 real passa janela de fechamento, hoje, `espelho=esp` já lido e um instante comum `agora`; pautas usa a mesma classe sem espelho antecipado. [app/ponto/management/commands/e6_oraculo.py:292](app/ponto/management/commands/e6_oraculo.py:292), [app/ponto/management/commands/e6_oraculo.py:304](app/ponto/management/commands/e6_oraculo.py:304), [app/ponto/management/commands/e6_oraculo.py:316](app/ponto/management/commands/e6_oraculo.py:316), [app/ponto/management/commands/pautas_discordantes.py:81](app/ponto/management/commands/pautas_discordantes.py:81)

**Quem produz/quem lê dono:** o juiz produz o retorno; `InsumosDoDono.dono` monta os argumentos e delega; E6 conta o retorno; pautas grava no item; teto inclui no resultado; Cadastro x Realidade inclui como `dono_do_dia` e usa o juiz para resolver conflitos. [app/ponto/management/commands/e6_oraculo.py:110](app/ponto/management/commands/e6_oraculo.py:110), [app/ponto/management/commands/e6_oraculo.py:183](app/ponto/management/commands/e6_oraculo.py:183), [app/ponto/management/commands/e6_oraculo.py:366](app/ponto/management/commands/e6_oraculo.py:366), [app/ponto/management/commands/pautas_discordantes.py:109](app/ponto/management/commands/pautas_discordantes.py:109), [app/ponto/services/teto_na_lista.py:86](app/ponto/services/teto_na_lista.py:86), [app/escala/services/cadastro_realidade.py:459](app/escala/services/cadastro_realidade.py:459), [app/escala/services/cadastro_realidade.py:932](app/escala/services/cadastro_realidade.py:932)

**Universo causal do censo:** regra escrita exige medir “dias que seguram a folha (`o_que_segura`) sem porta”; implementação enumera precisamente as datas retornadas por `dias_que_seguram`, sem expandir para cada dia exportado como fora. Retido e retentor são perguntas distintas nesse contrato. [app/docs/CORTES.md:379](app/docs/CORTES.md:379), [app/chamados/services/vigia_fio.py:493](app/chamados/services/vigia_fio.py:493), [app/chamados/services/vigia_fio.py:585](app/chamados/services/vigia_fio.py:585)

**Classificação recomendada:** exclusão do dia retido do censo + `ESTRUTURA` isolado não constitui bug provado nem demonstra lei faltando. Para concluir sobre liberação faltam execução do ato, efeitos sobre as datas causais, nova avaliação da retenção e demais requisitos do export; a sonda apresentada não contém esse antes/depois. [não verificado]

A L-099 exige coerência dos leitores, não porta própria em cada dia afetado por bloqueio mensal; a L-120 proíbe trava sem saída por bug, mas a existência de porta causal não demonstra conclusão do fluxo. [CLAUDE.md:176](CLAUDE.md:176), [app/docs/LEIS.md:141](app/docs/LEIS.md:141)

Existe regra “a tela NUNCA difere da folha”, porém o ADR trata status/valor do dia e a sonda não mede tela; não basta citar essa frase para provar violação nos cinco casos. [app/docs/ADR/0002-celula-e-o-dia.md:8](app/docs/ADR/0002-celula-e-o-dia.md:8), [app/docs/ADR/0002-celula-e-o-dia.md:29](app/docs/ADR/0002-celula-e-o-dia.md:29), [não verificado]

## 06/11 09:23 · sessao cd8554eb9adc · BO-0511-G2-TRIAGEM · sonda · origem=Code

**Parte A — conferência na cópia**

A execução, o dump, a vinculação ao hash informado e a equivalência com as sondas HJ/G3G4 ficam [nao verificado]. Abaixo, as referências apontam para as funções existentes na cópia; os detalhes do script anexado ficam [nao verificado] quanto à execução.

| Campo | Chamada do anexo e conferência |
|---|---|
| **a** | `status_do_dia_lavrado(c, dia, agora=agora)` corresponde à assinatura `status_do_dia_lavrado(colaborador, data, *, agora=None)`. A função delega ao corpo em lote. `app/ponto/status_dia.py:201–218`. |
| **b** | `espelho_do_colab(c, hoje, ini, fim, agora=agora)` corresponde à assinatura com quatro argumentos posicionais e `agora` obrigatório por palavra-chave. `app/ponto/services/espelho.py:1194`. `InsumosDoDono(c, ini, fim, hoje, espelho=esp, agora=agora)` e `.dono(dia)` também correspondem às assinaturas. `app/ponto/management/commands/e6_oraculo.py:128`, `:172`. É a sequência usada pelo E6. `app/ponto/management/commands/e6_oraculo.py:304`, `:316`, `:366`. |
| **c** | A assinatura real é `classificar_export(empresa, mes, ano, fechamentos=None)`; a chamada com três argumentos usa os fechamentos gravados. `app/folha/export.py:757–780`. No anexo, o nome público passa pelo proxy de memória, que chama a referência real previamente guardada [nao verificado]. |
| **d** | `dias_que_seguram(c.empresa, mes, ano)` corresponde à assinatura real. Ela devolve `itens`, `furo` e `dias` por colaborador. `app/folha/fora_da_folha.py:102–139`. Sua importação interna de `classificar_export` ocorre em `app/folha/fora_da_folha.py:115–119`; sob o patch do anexo, passa pelo mesmo proxy [nao verificado]. |
| **e** | `porta_alcancavel_do_dia(cel.pk)` corresponde à assinatura `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)`. `app/chamados/juizes.py:1341`. A função admite retorno `None`. `app/chamados/juizes.py:1382–1387`. |
| **f** | `data_do_chamado(ch.modulo_origem, ch.contexto_json)` corresponde exatamente à assinatura real. A função consulta a chave de data declarada no catálogo. `app/chamados/catalogo/modulos.py:219–225`. |

**Lógica construída no anexo:** em **d**, `segura` é calculado localmente pelo filtro `x["data"] == dia` e por `bool(encontrados)`, com ramos próprios para ausência no universo; em **f**, a consulta dos chamados, a conversão `date.fromisoformat(str(raw)[:10])`, a comparação com o dia e a montagem da lista são locais. Portanto, ambos chamam as funções reais, mas esses resultados agregados são construídos pela sonda [nao verificado]. As fontes reais fornecem, respectivamente, os dias por colaborador e a data catalogada. `app/folha/fora_da_folha.py:138–139`; `app/chamados/catalogo/modulos.py:219–225`.

A competência usa as funções reais `corte_da_empresa(empresa)` e `janela_atual(data, empresa=None)`, com assinaturas compatíveis. `app/ponto/janelas.py:36`, `:60–63`. A exigência adicional `corte == 21` pertence ao anexo [nao verificado].

**Parte B — oito unidades**

Cada marca `[nao verificado]` na primeira coluna abrange a transcrição da respectiva linha do dado fornecido. `null` em porta é mantido como retorno explícito; a função pode retornar `None`. `app/chamados/juizes.py:1382–1383`.

| uid | col | dia | segura a folha (d) | dono (b) | status lavrado (a) | porta (e) | chamado(s) do dia: id, tipo, status_local (f) | erro |
|---|---:|---|---|---|---|---|---|---|
| G2-01 [nao verificado] | 407 | 2026-10-21 | s | BATIDA | `irregular`; fonte `lavrado` | `null` | 33284, `batida_ausente`, `"fechado"`; 33290, `batida_fora_escala_leve`, `"registrado"`; 33491, `turno_aberto`, `"resolvido"` | Nenhum `ERRO` informado |
| G2-02 [nao verificado] | 836 | 2026-10-13 | s | CADASTRO | `escala_suspeita`; fonte `lavrado` | `null` | 30870, `batida_ausente`, `"resolvido"` | Nenhum `ERRO` informado |
| G2-03 [nao verificado] | 836 | 2026-10-14 | s | CADASTRO | `escala_suspeita`; fonte `lavrado` | `null` | 31151, `batida_ausente`, `"resolvido"` | Nenhum `ERRO` informado |
| G2-04 [nao verificado] | 909 | 2026-10-16 | s | BATIDA | `irregular`; fonte `lavrado` | `rota=porta_fila`; `fila=[31891]`; `verbos={"31891":"validar"}`; `fio=admin` | 31889, `turno_aberto`, `"resolvido"`; 31891, `batida_ausente`, `"em_analise"`; 32064, `turno_aberto_24h`, `"fechado"` | Nenhum `ERRO` informado |
| G2-05 [nao verificado] | 292 | 2026-09-21 | s | BATIDA | `irregular`; fonte `lavrado` | `null` | 24460, `batida_ausente`, `"fechado"` | Nenhum `ERRO` informado |
| G2-06 [nao verificado] | 207 | indeterminado | indeterminado | indeterminado | indeterminado | indeterminado | indeterminado | indeterminado; a–f ausentes, motivo `"sem dia"` |
| G2-07 [nao verificado] | 332 | indeterminado | indeterminado | indeterminado | indeterminado | indeterminado | indeterminado | indeterminado; a–f ausentes, motivo `"sem dia"` |
| G2-08 [nao verificado] | 446 | indeterminado | indeterminado | indeterminado | indeterminado | indeterminado | indeterminado | indeterminado; a–f ausentes, motivo `"sem dia"` |

Contagem por **dono**: ESTRUTURA **0**, CADASTRO **2**, BATIDA **3**, indeterminado **3**. **Soma = 8**. Os zeros contam apenas valores identificados no dado; as três unidades sem medida permanecem indeterminadas. [nao verificado]

Contagem por **segura**: s **5**, n **0**, indeterminado **3**. **Soma = 8**. [nao verificado]

## 06/11 09:23 · sessao 10f9eac90e5b · - · - · origem=Ronald (tmux)

Conclusões pelo código, distinguindo a medição B da sonda C:

| Caso | 05/11 — B | 06/11 — C | Retenção C.d |
|---|---|---|---|
| G2-01 col407 21/10 | **a — `chamado_vivo_com_veu`**, detalhe `registrado`, conforme linha B fornecida e vocabulário em `app/chamados/services/vigia_fio.py:538`; predicado em `app/chamados/services/vigia_fio.py:669`. | `porta=null` e chamado `registrado` satisfazem o predicado do véu **se o dia alcançar a cadeia a..e**; os quatro baldes anteriores não foram sondados, portanto classe atual completa [nao verificado]. Precedência: `app/chamados/services/vigia_fio.py:654`; véu: `app/chamados/services/vigia_fio.py:664`. | `espelho_furo`, veredito `furo`, em C.d; regra `espelho_` + veredito em `app/folha/export.py:1254`. |
| G2-02 col836 13/10 | **e — `pergunta_viva_sob_chamado_terminal`**, conforme B; ramo em `app/chamados/services/vigia_fio.py:687`. | `porta=null` + chamado terminal não mostra pergunta viva, resposta, texto, âncora nem baldes anteriores: classe atual [nao verificado]. Terminais sozinhos não distinguem c de e: `app/chamados/services/vigia_fio.py:675`; `app/chamados/services/vigia_fio.py:687`. | `espelho_indefinida`, veredito `indefinida`, em C.d; `app/folha/export.py:1254`. |
| G2-03 col836 14/10 | **e — `pergunta_viva_sob_chamado_terminal`**, conforme B; `app/chamados/services/vigia_fio.py:687`. | Mesma insuficiência de campos, julgada separadamente: classe atual [nao verificado]; `app/chamados/services/vigia_fio.py:675`; `app/chamados/services/vigia_fio.py:687`. | `espelho_indefinida`, veredito `indefinida`, em C.d; `app/folha/export.py:1254`. |
| G2-04 col909 16/10 | Sem linha B: pelo significado explicitamente fornecido no anexo, não tinha dia SEM porta na medição de 05/11; rota daquele instante [nao verificado]. | **`porta_fila`**, fila `[31891]`, verbo `validar`, fio `admin`, conforme C.e; **não entra em a..e**, pois porta provoca `continue`: `app/chamados/services/vigia_fio.py:642`. Definição da porta/verbo: `app/chamados/juizes.py:1344`; `app/chamados/juizes.py:1357`. | `espelho_furo`, veredito `furo`, em C.d; `app/folha/export.py:1254`. |
| G2-05 col292 21/09 | **Não transferir a classe d de 22/10 ou 28/10 para 21/09**: B não possui linha deste dia. O ramo d exige ausência de perguntas vivas e dia fora da lista com colaborador na lista: `app/chamados/services/vigia_fio.py:677`. | C mostra célula, nenhuma porta e chamado `fechado`; não mostra perguntas/âncoras/lista/baldes, portanto classe a..e [nao verificado]. Terminais não determinam ramo: `app/chamados/services/vigia_fio.py:675`; `app/chamados/services/vigia_fio.py:679`. | `espelho_furo`, veredito `furo`, em C.d; `app/folha/export.py:1254`. |
| G2-06/07/08 sem dia | B não fornece um dia dos casos; enquadramento individual [nao verificado]. | C declara `indeterminado`, `sem dia`; classe, status e motivo individual [nao verificado]. O censo opera por chave colaborador+data: `app/chamados/services/vigia_fio.py:581`; `app/chamados/services/vigia_fio.py:638`. | [nao verificado]. |

A retenção não deriva do total de horas nem do estado terminal dos chamados: `dias_que_seguram` seleciona colaboradores classificados por `furo_espelho`, depois pede os dias ao juiz da retenção, sem consultar porta nessa etapa. `app/folha/fora_da_folha.py:102`; `app/folha/fora_da_folha.py:118`; `app/folha/fora_da_folha.py:128`; `app/folha/fora_da_folha.py:137`; delegação em `app/folha/fora_da_folha.py:95`.

O juiz retém veredito acusador quando não incide a isenção de todos os protestos não cobráveis nem a isenção de furo só da pausa, registrando `espelho_` + veredito. `app/folha/export.py:1233`; `app/folha/export.py:1245`; `app/folha/export.py:1253`. C.d demonstra o resultado dessa regra; os protestos e a ata que explicariam individualmente qual isenção falhou permanecem [nao verificado].

Detalhe útil: `registrado` é vivo, mas não cobra ação; está fora de `COBRANCA`. `app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:26`; `app/chamados/catalogo/motor.py:29`.

## 06/11 09:23 · sessao d29c534df35b · - · - · origem=Ronald (tmux)

Escopos conferidos, sem escrita:

| Obra/lei | Estado literal na linha | Alcance e limite |
|---|---|---|
| **O261-FIO-MORTO-PELO-RETRATO** | **ABSORVIDA**; “curas no ar” | Mede dia que segura folha sem porta viva, causas a–e. Enquadra documentalmente o problema de alcance de col407; não demonstra causa atual de cada ausência de porta. Cita col446/#11582, cuja identidade com G2-08 é **[nao verificado]**. `app/docs/BACKLOG.md:963` |
| **O261-E-CELULA-20-UM-DONO** | “F0 FECHADA”; “F1 NO AR”; “F2 NO AR 01/11”; “F3 PARADA 02/11” | Escopo expresso: pergunta viva sob chamado terminal. Alcança a classe registrada em B para col836 13/10 e 14/10; permanência dessa subclasse em 06/11 **[nao verificado]**. `app/docs/BACKLOG.md:859` |
| **O261-COMPLEMENTO-[nome]** | “(C) = cura (2) PAI-TERMINAL em copia”; “(B) materializada sem resposta 2.970 a construir” | Cita **col292 21/09** nominalmente; define alcance da porta, materializada sem resposta e nascimento sob pai terminal. Atribuir os IDs de perguntas dessa documentação ao conteúdo de C permanece **[nao verificado]**. `app/docs/BACKLOG.md:782` |
| **O261-E-F3-DORMENTE** | “fila 1”; “medida despachada 00:3x” | Matriz do estoque por classe/cutucada; beco 1 mede dono pelo oráculo e prevê CADASTRO para lista do admin. Não prova, sozinho, que os dois dias de col836 pertencem ao estoque específico. `app/docs/BACKLOG.md:1799` |
| **F3-BECO-1-VOLTA-3** | **NO AR 05/11 17:5x** | Rejulga mês do vínculo quando defeito de folga vira False; reata somente a pai na fila. C não contém a transição de folga nem o pai na fila, portanto enquadramento específico dos G2-02/03 **[nao verificado]**. `app/docs/BACKLOG.md:1775` |
| **C20-JUIZ-3** | **FECHADA**, **NO AR** | Evento em criar()/recompute_turnos, consumidor O-OUTBOX. A queixa “resposta sem reflexo” e `turno_aberto` terminal não demonstram a premissa individual desse evento em G2-04. `app/docs/BACKLOG.md:1768` |
| **C20-FIO-MUDO** | **ABERTA**, “construtor aberto” | Evento na abertura de `batida_ausente` e cinco atos de perda. Ter chamados vivos de batida ausente, sem dia, não demonstra fio mudo nem ato de perda: G2-06/07/08 **[nao verificado]**. `app/docs/BACKLOG.md:1769` |
| **L-099** | **PELA-METADE** | Separa ESTRUTURA/CADASTRO/BATIDA; somente ESTRUTURA é fatia; CADASTRO/BATIDA são lista do admin. Serve ao enquadramento dos donos medidos, sem comprovar uma obra técnica para cada caso. `app/docs/LEIS.md:122` |

O261 original consta **FECHADA 24/10 09:5x**, mas seus complementos têm estados próprios; não substituir o estado de cada complemento pelo estado da original. `app/docs/BACKLOG.md:1450`

Para os três sem dia, **nenhuma encontrada com enquadramento individual demonstrado pela queixa A**; candidatos genéricos acima permanecem **[nao verificado]**. A referência de col207 a **02/10** em documentação não pode preencher o dia ausente de G2-06: ela trata de ponta de 127 minutos/raio de 90, com estado “aberta, nada construido — linha de fila, nao obra”. `app/docs/BACKLOG.md:1449`

Cortes adicionais que explicitam escopo: **O261-FIO-MORTO-PELO-RETRATO**, “medida em curso”, `app/docs/CORTES.md:379`; **O261-COMPLEMENTO-[nome]**, “medida em curso”, `app/docs/CORTES.md:382`; **LAUDO-3010-JUIZ-DA-ACAO-DO-ADMIN**, “registrado”, porta comum lida por folha/raio-X/vigia, `app/docs/CORTES.md:401`.

## 06/11 09:23 · sessao b22df4f1edd5 · - · - · origem=Ronald (tmux)

Leitura do código:

- `status_do_dia_lavrado` delega ao leitor em lote; quando o dia não está em curso e a célula tem `status_dia`, o leitor devolve a coluna e `fonte='lavrado'`, sem rederivar o status. `app/ponto/status_dia.py:217`, `app/ponto/status_dia.py:287`.
- `furo` pertence aos vereditos que acusam irregularidade; o escritor retorna `irregular` nas passagens pertinentes, tanto sem batida quanto com batida. O rótulo significa “Batida faltando, fora do horario previsto ou em ordem inesperada”. `app/ponto/status_dia.py:32`, `app/ponto/status_dia.py:68`, `app/ponto/status_dia.py:154`, `app/ponto/status_dia.py:159`.
- `indefinida` expressa dúvida sobre se o dia era de trabalho; a regra retorna `escala_suspeita`, salvo precedências humanas descritas e tratadas antes. O rótulo é “Sem batida, mas a ESCALA esta em duvida no cadastro — nao e falta do colaborador”. `app/ponto/status_dia.py:38`, `app/ponto/status_dia.py:128`, `app/ponto/status_dia.py:139`, `app/ponto/status_dia.py:143`.
- Status não é o valor pago do dia: o valor fica no `DiaPago`. `app/ponto/status_dia.py:12`.

Aplicação aos anexos C:

| Casos | Status/veredito informado e significado no código | Dono informado e ramo demonstrável |
|---|---|---|
| G2-01 col407 21/10 | `irregular`, fonte `lavrado`; veredito `furo`: irregularidade de batida, conforme rótulo e conjunto `_IRREGULAR`. `app/ponto/status_dia.py:32`, `app/ponto/status_dia.py:68`, `app/ponto/status_dia.py:287`. A ocorrência individual está no anexo C fornecido, sem linha de arquivo local [nao verificado]. | `BATIDA`, respondido pelo juiz `dono_da_divergencia`, alimentado por `InsumosDoDono.dono`; alternativas: ramo `batida_fora_de_turno` ou contagem ímpar das horas atribuídas pelo oráculo. `app/ponto/management/commands/e6_oraculo.py:104`, `app/ponto/management/commands/e6_oraculo.py:106`, `app/ponto/management/commands/e6_oraculo.py:172`. Qual desses dois ramos ocorreu [nao verificado]. |
| G2-02 col836 13/10 | `escala_suspeita`, fonte `lavrado`; veredito `indefinida`: o juiz não sabe se era dia de trabalho, status com dúvida de cadastro. `app/ponto/status_dia.py:38`, `app/ponto/status_dia.py:129`, `app/ponto/status_dia.py:143`, `app/ponto/status_dia.py:287`. A ocorrência individual está no anexo C, sem linha local [nao verificado]. | `CADASTRO`: pode sair da prioridade `folga_nao_definida / sem_vinculo_no_dia / sem_vinculo_em_dia_de_trabalho`, de `fase_deslocada`, ou de `em_cadastro_x_realidade`. `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:108`, `app/ponto/management/commands/e6_oraculo.py:110`. Qual ramo ocorreu [nao verificado]. |
| G2-03 col836 14/10 | Mesmo enquadramento de status/veredito, aplicado separadamente ao dia 14. `app/ponto/status_dia.py:38`, `app/ponto/status_dia.py:129`, `app/ponto/status_dia.py:143`. Ocorrência individual sem linha local [nao verificado]. | Mesmo conjunto de alternativas CADASTRO, sem ramo individual demonstrado. `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:108`, `app/ponto/management/commands/e6_oraculo.py:110`; ramo [nao verificado]. |
| G2-04 col909 16/10 | `irregular` / `furo`, fonte `lavrado`, com o mesmo significado de irregularidade acima. `app/ponto/status_dia.py:32`, `app/ponto/status_dia.py:68`, `app/ponto/status_dia.py:287`. Ocorrência individual sem linha local [nao verificado]. | `BATIDA`: ramo 104 ou 106, sem escolha possível pelo dump. `app/ponto/management/commands/e6_oraculo.py:104`, `app/ponto/management/commands/e6_oraculo.py:106`; ramo exato [nao verificado]. |
| G2-05 col292 21/09 | `irregular` / `furo`, fonte `lavrado`, mesmo significado. `app/ponto/status_dia.py:32`, `app/ponto/status_dia.py:68`, `app/ponto/status_dia.py:287`. Ocorrência individual sem linha local [nao verificado]. | `BATIDA`: ramo 104 ou 106, sem escolha possível pelo dump. `app/ponto/management/commands/e6_oraculo.py:104`, `app/ponto/management/commands/e6_oraculo.py:106`; ramo exato [nao verificado]. |
| G2-06/07/08 | Sem dia: status/veredito individual [nao verificado]. | Anexo C não tem `b.dono`; dono e ramo [nao verificado]. |

Limites importantes para a consolidação:

- A ordem efetiva do juiz é prioridade cadastral → batida fora do turno → contagem ímpar → fase deslocada → Cadastro×Realidade/resíduo ESTRUTURA. `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:104`, `app/ponto/management/commands/e6_oraculo.py:106`, `app/ponto/management/commands/e6_oraculo.py:108`, `app/ponto/management/commands/e6_oraculo.py:110`.
- `travas_aprovacao=['turno_aberto']` e um chamado de tipo `turno_aberto` não constituem o insumo `batida_fora_de_turno`: essa função exige realizado aberto, entrada sem saída atribuída ao dia e existência da entrada no universo de batidas apuráveis. `app/ponto/management/commands/e6_oraculo.py:186`, `app/ponto/management/commands/e6_oraculo.py:196`, `app/ponto/management/commands/e6_oraculo.py:198`.
- A quantidade usada pelo ramo de paridade vem das horas do oráculo, com carga ampliada de batidas apuráveis; não é contagem de chamados nem contagem física numa data. `app/ponto/management/commands/e6_oraculo.py:120`, `app/ponto/management/commands/e6_oraculo.py:142`, `app/ponto/management/commands/e6_oraculo.py:144`, `app/ponto/management/commands/e6_oraculo.py:180`.
- `InsumosDoDono.dono` monta `batida_fora_de_turno`, fase e cadastro; consulta Cadastro×Realidade quando necessário. O anexo C não expõe esses argumentos, portanto o retorno permite identificar os ramos possíveis, mas não escolher um ramo exato. `app/ponto/management/commands/e6_oraculo.py:175`, `app/ponto/management/commands/e6_oraculo.py:179`, `app/ponto/management/commands/e6_oraculo.py:183`; argumentos individuais [nao verificado].

## 06/11 09:19 · sessao dd224cc5cb59 · O10-TROCA-DE-PLANTAO · censo · origem=Code

HASH LIDO = 185c878317e21679038d2274c24f4d017998ee4b

Identificador da cópia recebida; correspondência com commit por metadados Git: [nao verificado]. Resultados abaixo são censo estático; execução dos comportamentos e valores financeiros: [nao verificado]. [nao verificado]

## Q1. Escritores de `CelulaDia` editada

**Não encontrei escritor humano de produção para `origem='editada'`.** A cópia declara expressamente `editada_em` e `editada_por` como campos **sem escritor**, com o comentário “a edição humana não tem porta”; outro serviço registra “escritor de produção: nenhum declarado hoje”. Isso diverge da premissa “escritor humano existente” do anexo (`app/escala/tests/test_d3_guarda_da_linha_celula.py:50`; `app/escala/tests/test_d3_guarda_da_linha_celula.py:179`; `app/escala/services/fase_12x36.py:161`).

| Caminho existente | Campos e trilha | Atomic e chamador |
|---|---|---|
| `gerar_celulas_periodo` | Cria `colaborador,data,trabalha,origem='gerada',escala_geradora,dna`; pula editadas. Não é editor humano (`app/ponto/portas/celula.py:633`; `app/ponto/portas/celula.py:638`). | A função não abre atomic próprio; o command a envolve em atomic (`app/ponto/portas/celula.py:595`; `app/ponto/management/commands/gerar_celulas.py:310`). |
| `regenerar_celulas_vinculo` | Cria gerada; reescreve somente geradas. Guarda `dna_anterior,regenerada_em,regeneracoes`; evento leva usuário, motivo e resumo posterior, sem constituir trilha de edição humana (`app/ponto/portas/celula.py:253`; `app/ponto/portas/celula.py:257`; `app/ponto/portas/celula.py:265`; `app/ponto/portas/celula.py:330`). | Atomic em `:212`; chamado por vínculo, edição de template e signals (`app/ponto/portas/celula.py:212`; `app/colaboradores/services/vinculo.py:857`; `app/escala/services/cadastro_tipo.py:348`; `app/escala/signals.py:71`). |
| Resolver: `veredito_celula` → `lavrar_veredito` | Escreve **julgamento**, não `origem/trabalha/dna`: veredito, via, horário, protestos e status; ata quando fornecida. Evento registra ator e antes/depois; despachante registra ações/erros, sem argumento de motivo livre (`app/ponto/services/veredito_celula.py:191`; `app/ponto/services/veredito_celula.py:330`; `app/ponto/services/veredito_celula.py:334`; `app/ponto/portas/celula.py:815`; `app/ponto/portas/celula.py:819`). | Atomic no despachante; save/outbox também atomic. Entrada HTTP em `veredito_celula_view` (`app/ponto/services/veredito_celula.py:204`; `app/ponto/portas/celula.py:681`; `app/ponto/views.py:3312`). |

O modelo apenas oferece as choices e os campos de auditoria; a migration apenas cria esse schema, sem implementar o ato humano (`app/escala/models.py:1690`; `app/escala/models.py:1732`; `app/escala/migrations/0030_celuladia.py:23`).

**Duas células no mesmo atomic:** o modelo permite duas linhas de colaboradores/datas distintos: sua unicidade é `(colaborador,data)`. O escritor canônico é definido por **módulo**, `ponto/portas/celula.py`, sem limite de uma linha por transação (`app/escala/models.py:1833`; `app/core/portas.py:107`).

**Mas não existe hoje a operação humana de edição para chamar duas vezes.** O Resolver aceita somente três vereditos, e sua view processa vários dias de **um colaborador**, chamando um atomic por dia; portanto esse lote não realiza o par indivisível solicitado (`app/ponto/services/veredito_celula.py:18`; `app/ponto/services/veredito_celula.py:204`; `app/ponto/views.py:3318`; `app/ponto/views.py:3339`).

A porta existente poderia comportar mais de uma linha sem virar outro **módulo escritor**; porém sua lista atual só declara gerar/lavrar/carimbar. Os selos cobram tanto ausência de escrita fora da porta quanto pertencimento a uma operação declarada. Logo, “reusar duas edições existentes, sem acrescentar capacidade” não corresponde ao código disponível (`app/ponto/portas/celula.py:66`; `app/ponto/tests/test_chokepoint_celula_gate.py:49`; `app/ponto/tests/test_chokepoint_celula_gate.py:69`).

**Escritas `editada` encontradas em testes**, sem constituir porta humana de produção: [nao verificado]

| Forma | Sítios |
|---|---|
| Atribuição e `save` | `app/escala/tests/test_celula_dia.py:73`, `:98`, `:358`; `app/colaboradores/tests/test_vinculo_regenera_celulas.py:39`; `app/ponto/tests/test_porta_celula_dia.py:247`. |
| Criação/update direto | `app/escala/tests/test_selo_foto_folgas_relavra.py:104`; `app/escala/tests/test_sem_dia_pelo_juiz.py:46`; `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:97`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:134`, `:180`, `:225`; `app/ponto/tests/test_d3_impressao_cobre_insumos.py:242`. |
| Helper que recebe a origem | `app/escala/tests/test_selo_celula_alem_do_vinculo.py:58`, chamado com editada em `:103`; `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:116`, chamado em `:128`. |

## Q2. Leitores e sobrevivência à regeneração

| Leitor | O que efetivamente lê |
|---|---|
| `EC.eh_dia_trabalho` | Retorna `cel.trabalha`, sem filtrar origem; somente sem célula usa a aritmética do vínculo (`app/escala/models.py:1087`; `app/escala/models.py:1094`). |
| `EC.marcos_do_dia` | Lê `dna.marcos`, sem filtrar origem. **Marcos vazios/nulos fazem cair no template**, mesmo existindo célula (`app/escala/models.py:1124`; `app/escala/models.py:1133`). |
| `EC.pausas_do_dia` / `intervalo_do_dia` | Leem as pausas congeladas; célula existente sem marcos devolve nenhuma pausa, sem aquele fallback (`app/escala/models.py:1163`; `app/escala/models.py:1189`; `app/escala/models.py:1202`). |
| `gerar_celulas_periodo` | Editada é pulada: **sobrevive** (`app/ponto/portas/celula.py:638`). |
| `regenerar_celulas_vinculo` / reversão | Reescreve somente gerada; até o desligamento fora da vigência exige gerada. Reversão também recusa editada: **sobrevive** (`app/ponto/portas/celula.py:232`; `app/ponto/portas/celula.py:257`; `app/ponto/portas/celula.py:558`). |
| Fechamento/motor | Previsto vem de `EC.eh_dia_trabalho`; células alimentam o motor; separação trabalho/folga consulta o vínculo (`app/ponto/services/fechamento.py:722`; `app/ponto/services/fechamento.py:741`; `app/ponto/motor_calculo_v2.py:2448`). |
| Export | Grade delega a `grade_da_celula`, que lê todas as origens, mas prefere `ata.tipo_dia` e `ata.minutos_previstos` já lavrados. Rubricas do TXT vêm do fechamento gravado (`app/folha/export.py:335`; `app/escala/services/leitor_celula.py:350`; `app/escala/services/leitor_celula.py:380`; `app/folha/export.py:439`). |

**Sobreviver à regeneração não garante ata e dinheiro atualizados.** O export pode continuar lendo a ata anterior; a impressão do cartório não inclui `trabalha`, e impressão igual pode fazer pular o julgamento (`app/escala/services/leitor_celula.py:380`; `app/ponto/services/cartorio.py:456`; `app/ponto/services/cartorio.py:1358`).

## Q3. Rubricas do dia cedido e assumido

As conclusões seguintes pressupõem o cenário perguntado: cedido sem trabalho real; assumido com trabalho real e célula/marcos coerentes. Batidas, pausas, feriados, vínculos e valores de um par concreto: [nao verificado].

| Rubrica | CEDIDO: célula folga | ASSUMIDO: célula trabalho | Quem decide / fonte |
|---|---|---|---|
| **Adicional noturno** | Sem período trabalhado, não nasce AN. | Apura AN dos períodos noturnos; a passagem folga→trabalho não cria, por si, o direito, pois folga trabalhada já participa da soma. | Períodos reais e política noturna; soma `periodos + periodos_ft`; fechamento grava resultado (`app/ponto/motor_calculo_v2.py:983`; `app/ponto/motor_calculo_v2.py:321`; `app/ponto/services/fechamento.py:909`). |
| **Intrajornada** | Sem período real, não nasce indenização. | Apura supressão conforme duração e pausa; pode ser zero. | Intervalos reais, previsto e cadastro/perfil. Regras 12x36/comercial em `app/ponto/motor_calculo_v2.py:2766` e `:3226`; mínimo/cadastro em `:1824` e `:831`. Previsto consulta célula e marcos (`app/escala/utils.py:2287`; `app/escala/utils.py:2290`). |
| **HE100 por folga/DSR trabalhado** | Sem trabalho, não há período a pagar. | Célula `True` retira o período de `periodos_ft`; passa a normal e deixa de alimentar `horas_folga_trabalhada`. | **Lê célula por EC** em `app/ponto/motor_calculo_v2.py:2448`; partição em `:2468` e `:2492`. Fechamento paga a rubrica de folga a partir de FT e escala certa (`app/ponto/services/fechamento.py:925`; `app/ponto/services/fechamento.py:949`). |
| **DSR: semana/reflexo** | Sai dos dias obrigatórios; não gera falta injustificada por não trabalhar nessa data. | Entra no previsto; trabalho normal conta para a semana. Resultado depende dos demais dias/extras. | Previsto lê célula no fechamento (`app/ponto/services/fechamento.py:722`). Motor combina previsto, períodos e datas justificadas (`app/ponto/motor_calculo_v2.py:2595`; `:2600`; `:2636`); comercial refaz o DSR em `:3452`. |
| **Furo apurado** | Sai dos obrigatórios e deixa de gerar furo por essa ausência. | Com trabalho reconhecido, não é furo; sem trabalho/abono, pode ser. | Previsto + períodos + justificativas (`app/ponto/motor_calculo_v2.py:2600`; `:2610`). O fechamento aplica teto temporal antes de lavrar (`app/ponto/services/fechamento.py:1099`). |
| **Falta/desconto decidido** | **Célula não cancela Ausencia DESCONTA aprovada preexistente.** | Também não cancela essa decisão. | Outra fonte: Ausencia aprovada + catálogo. Fechamento lê minutos decididos; TXT lê datas decididas (`app/ponto/services/fechamento.py:864`; `app/folha/export.py:87`; `:138`; `:179`; `:732`). |
| **Atraso/antecipada** | Previsto zero limita a cobrança a zero. | Novo dia normal com marcos pode sofrer pontualidade pelas regras já vigentes. | Teto pelo previsto em `app/ponto/motor_calculo_v2.py:1040`; marcos do vínculo em `:2048`; comercial chama o julgamento em `:3293`; 12x36 com escala em `:3533`. |

**“Sem HE100” precisa distinguir a causa:** editar para trabalho retira o adicional **por folga**, mas não elimina automaticamente HE100 por feriado ou excedente normal. Essas decisões usam calendário/política de feriado e excedente, respectivamente (`app/ponto/motor_calculo_v2.py:934`; `:2847`; `:3392`).

**Não identifiquei rubrica-alvo que exija alterar `ponto/motor_calculo_v2.py` para consumir esse override.** Os sítios necessários já consultam célula: classificação trabalho/folga em `:2448`, partição em `:2468`, marcos em `:2290`. Cancelar desconto já decidido pertence ao fluxo de Ausencia, cuja leitura financeira é separada (`app/folha/export.py:138`; `app/folha/export.py:179`). Prova executada de que duas edições satisfazem todo o contrato O10: [nao verificado].

**S5b histórico não descreve integralmente esta cópia:** O271 posteriormente ligou pontualidade comercial; o código efetivamente chama `_julgar_pontualidade`. Portanto não cabe afirmar que o comercial atual continua isento/`NAO_DECIDE` (`app/ponto/calculador/regras.py:455`; `app/ponto/calculador/regras.py:466`; `app/ponto/motor_calculo_v2.py:3293`).

**DSR no TXT:** o export não emite evento DSR; registra que o Domínio calcula o reflexo a partir de HE/AN. Valor efetivamente pago no Domínio: [nao verificado] (`app/folha/export.py:453`).

**Motor versus calculador:** o fechamento mensal recebe rubricas do motor; desde competência 10/2026 há também `DiaPago` versão `oraculo`, alimentado pelo calculador, sem substituir esse fechamento. O calculador igualmente consulta célula e marcos (`app/ponto/services/fechamento.py:909`; `:1085`; `:1249`; `:1278`; `app/ponto/calculador/alimentacao.py:201`; `:434`).

## Q4. Resolver dia: oferta, opções e entrada da quarta

| Camada | Comportamento atual |
|---|---|
| Juiz da oferta | `pode_ultimo_recurso` + data passada + status entre `falta,irregular,aberto,sem_certidao`; permissão fica fora do predicado (`app/colaboradores/services/calendario.py:94`; `:97`; `:103`). |
| Último recurso | Exige data anterior ao início da competência atual e ausência de pergunta viva respondida naquele dia (`app/colaboradores/services/calendario.py:472`; `:738`). |
| Chamadores | Calendário carimba o resultado por dia. Porta plural monta calendários e lê esse mesmo campo; o vigia do fio usa a plural (`app/colaboradores/services/calendario.py:132`; `:135`; `:769`; `app/chamados/services/vigia_fio.py:265`). |
| Telas | Perfil e drawer fornecem permissão `regularizar_batida` e tipos do cadastro; grade inclui o popover quando permitido/oferecido (`app/colaboradores/views.py:1948`; `:1957`; `app/colaboradores/services/drawer.py:130`; `:154`; `app/templates/colaboradores/partials/_calendario_grade.html:125`). |
| POST | `/celula/veredito/` → `veredito_celula_view`; valida permissão/veredito, recebe um colaborador e uma ou várias datas (`app/ponto/urls.py:58`; `app/ponto/views.py:3315`; `:3318`; `:3328`). |

| Opção apresentada | Quem executa |
|---|---|
| **Trabalhou no horário da escala** | Simula correções, faz flips e materializa batidas faltantes; recusa se a grade não prevê trabalho. Não transforma folga em dia normal (`app/templates/ponto/partials/_veredito_dia.html:21`; `app/ponto/services/veredito_celula.py:221`; `:295`; `:301`). |
| **Não trabalhou — falta** | Cria Ausencia `falta`, aprovada (`app/templates/ponto/partials/_veredito_dia.html:22`; `app/ponto/services/veredito_celula.py:311`; `:315`). |
| **Justificado** | Cria Ausencia aprovada do tipo escolhido (`app/templates/ponto/partials/_veredito_dia.html:23`; `app/ponto/services/veredito_celula.py:315`). |
| **Cobrar este dia** | Botão adicional, com outra porta; manda julgar/emite cobrança e pode criar pergunta no app. Não é quarto veredito do despachante (`app/templates/ponto/partials/_veredito_dia.html:25`; `app/chamados/services/cobrar_dia.py:39`; `:43`; `:47`). |

**Entrada natural da quarta opção de negócio:** mesmo partial → mesma view → despachante/lista `VEREDITOS`; o contrato nomeia justamente “Trocou de plantão” nessa porta. Porém hoje faltam os dados do par/anexos na requisição, e o serviço não possui a operação de edição identificada em Q1 (`app/docs/CORTES.md:187`; `app/templates/ponto/partials/_veredito_dia.html:17`; `app/ponto/views.py:3318`; `app/ponto/services/veredito_celula.py:18`).

Há também uma questão de alcance: **`folga_trabalhada` não está entre os status que oferecem o Resolver**, e competência corrente fica fora do último recurso. A oferta atual, sozinha, não alcança todos os cenários descritos no corte (`app/colaboradores/services/calendario.py:94`; `:738`; `app/docs/CORTES.md:187`). Inclusão atual desse mesmo Resolver diretamente no chamado de batida em folga: [nao verificado].

## Q5. `troca_turno`, `troca_de_plantao` e `efeito='abona'`

### `troca_turno`

| Aspecto | Escritor/leitor e efeito |
|---|---|
| Natureza | Choice de `Justificativa.tipo`; migration `0019` altera schema/choices. API oferece e traduz o valor; template web também oferece (`app/ponto/models.py:126`; `app/ponto/migrations/0019_tipos_justificativa.py:16`; `app/api/views.py:2070`; `:2099`; `app/templates/ponto/justificativa_form.html:43`). |
| Escritores do tipo | API atualiza/cria justificativa pendente; formulário web faz equivalente (`app/api/views.py:2001`; `:2007`; `app/ponto/views.py:1625`; `:1635`). Django admin também é editável (`app/ponto/admin.py:18`). |
| Escritores da decisão | Individual no serviço; lote na view; materialização retroativa também aprova. Registram status, usuário e horário (`app/ponto/services/justificativa.py:190`; `app/ponto/views.py:1796`; `app/ponto/services/justificativa.py:368`). |
| Leitor específico do nome | SLA de **3 dias**; fila e cron consultam essa autoridade. É prazo do DP, não regra monetária (`app/ponto/catalogo/justificativas.py:24`; `:29`; `app/ponto/views.py:1726`; `app/ponto/management/commands/alertar_justificativas_sla.py:19`). |
| Efeito financeiro genérico | Justificativa não tem campo `efeito`. Qualquer aprovada entra em `datas_justificadas`, sem filtro de tipo; isso interfere em falta injustificada/abono/DSR (`app/ponto/models.py:146`; `:159`; `app/ponto/services/fechamento.py:651`; `app/ponto/motor_calculo_v2.py:2600`; `:2606`). |
| Batida e célula | Aprovação pode criar saída manual se houver entrada + saída informada; não escreve override do par. A API anuncia `requer_saida=False` (`app/ponto/services/justificativa.py:137`; `:172`; `:190`; `app/api/views.py:2070`). |

### `troca_de_plantao` / Ausencia

**Não localizei ramo runtime nem migration de dados com esse literal.** A existência e `efeito='abona'` vêm do dado raspado fornecido; quem criou esse cadastro e quando: [nao verificado]. O código aceita tipos cadastrados e decide genericamente pelo efeito (`app/ponto/services/ausencia.py:193`; `app/ponto/catalogo/ausencias.py:249`; `:293`).

| Entidade/caminho | Escritor | Leitor/efeito atual |
|---|---|---|
| `TipoAusencia` | UI entrega nome, efeito e flags à porta `gravar_tipo_ausencia`; código permanece estável (`app/ponto/views_tipos_ausencia.py:67`; `:84`; `:94`; `app/ponto/services/ausencia.py:1285`). | `efeito` pertence a esse cadastro. `Ausencia.efeito` consulta o catálogo vivo (`app/ponto/models.py:581`; `:840`; `app/ponto/catalogo/ausencias.py:293`). |
| Ausencia do tipo cadastrado | `criar_ausencia` grava tipo, datas, minutos, documento, status e autores; entradas API/admin/PWA. Edição e decisão também passam pelo serviço (`app/ponto/services/ausencia.py:341`; `:497`; `:895`; `app/api/views.py:1820`; `app/ponto/views.py:2415`; `:2536`). | Para folha só aprovada; ABONA entra em `datas_justificadas`; ausência inteira abonada pode zerar pontualidade (`app/ponto/turnos.py:2514`; `app/ponto/services/fechamento.py:684`; `app/ponto/turnos.py:2689`). |
| Pedido ainda não aprovado | Estados abertos são aguardando documento/decisão (`app/ponto/services/ausencia.py:749`). | Não cobre folha, mas ausência não rejeitada já pode suspender cobrança. Logo “virar pedido” também envolve esse leitor (`app/ponto/turnos.py:2514`; `:2516`). |
| Migrations | Seed cria tipos por `get_or_create`; lista não contém `troca_de_plantao`. Backfill posterior ajusta pendentes conforme documento (`app/ponto/migrations/0052_seed_tipoausencia.py:9`; `:33`; `app/ponto/migrations/0057_a4_backfill_pendente.py:30`). | Aplicação dessas migrations e origem específica do cadastro em produção: [nao verificado]. |

Existe auxiliar que seleciona tipos abonados para “8932”, mas **não encontrei chamador de produção de `datas_abonadas`**. Não concluo que a troca emita 8932 hoje: o montador efetivo percorre eventos do fechamento e acrescenta faltas; sua lista contém HE, folga, AN, intra e pontualidade (`app/folha/export.py:193`; `:201`; `:439`; `:722`; `:732`).

Os números **16 lançamentos, 12 aprovados, 2 rejeitados, 2 aguardando, todos SOLO/abona** são exclusivamente o anexo do pedido; conferência por arquivo de dados da cópia ou banco: [nao verificado].

## Q6. Cartas e exigência de dois anexos

| Ponto | Situação atual |
|---|---|
| Armazenamento | `Ausencia.documento`: **um `FileField`**, em `ausencias/documentos/`, opcional. `documento_referencia` aponta ao primeiro registro do intervalo; não é segunda carta (`app/ponto/models.py:777`; `:791`). |
| Entrada inicial | API recebe `FILES['documento']`; admin/PWA repassam um documento a `criar_ausencia` (`app/api/views.py:1775`; `:1822`; `app/ponto/views.py:2418`; `:2538`). |
| Entrada posterior | API e view administrativa chamam `anexar_documento`; esta **substitui** o campo único, registra ator/antes/depois e pode passar para aguardando decisão (`app/api/views.py:3130`; `app/ponto/views.py:2150`; `app/ponto/services/ausencia.py:840`; `:854`; `:857`). |
| Leitura | Download resolve `documento_referencia or a` e abre o arquivo da raiz (`app/ponto/views.py:2115`; `:2122`). |
| Exigência existente | Flag booleana `exige_documento`; estado inicial recebe `tem_documento`, também booleano. Não conta cartas nem identifica cedente/substituto (`app/ponto/models.py:583`; `app/ponto/services/ausencia.py:763`; `:775`). |
| Garantia de dois documentos | **Não existe nesses caminhos.** Há um arquivo por registro e nenhum gate do par; outra implemen

[truncado em 20000 de 24211 caracteres]

## 06/11 09:17 · sessao 501a50827797 · RE-O10-TROCA-DE-PLANTAO · censo · origem=Code

HASH LIDO = 185c878317e21679038d2274c24f4d017998ee4b [nao verificado]

| id | regra de origem (L-NNN, corte literal do aval ou arquivo:linha da regra) | entrada (model e campos, cada campo com arquivo:linha da definicao) | esperado OBSERVAVEL (o que um teste ve: retorno, linha gravada, status, texto) | mutante que tem de morder (a alteracao de codigo que este caso pega) | tipo golden/borda/adversarial | [nao verificado]
|---|---|---|---|---|---|
| G01 — cedente 12x36 | Aval A2: “cedente 12x36 e 6x1, dia cedido = folga sem desconto”; `app/docs/CORTES.md:187` | Confirmação pelo admin com par completo e duas cartas. `TipoEscala.tipo_ciclo='12x36'` (`app/escala/models.py:117`). `CelulaDia.colaborador=cedente` (`app/escala/models.py:1681`), `data=2026-11-03` (`app/escala/models.py:1685`), `trabalha=True` (`app/escala/models.py:1686`). Cartas em `Ausencia.documento` (`app/ponto/models.py:777`); ligação ao par: [nao verificado]. | Dia cedido passa a folga: `trabalha=False`; a troca não acrescenta desconto por esse dia. | Manter o dia cedido como trabalho e gerar falta pela ausência de batidas. | golden |
| G02 — cedente 6x1 | Aval A2: “cedente 12x36 e 6x1, dia cedido = folga sem desconto”; `app/docs/CORTES.md:187` | Confirmação pelo admin com par completo e duas cartas. `TipoEscala.tipo_ciclo='6x1'` (`app/escala/models.py:117`). `CelulaDia.colaborador=cedente` (`app/escala/models.py:1681`), `data=2026-11-04` (`app/escala/models.py:1685`), `trabalha=True` (`app/escala/models.py:1686`). Cartas em `Ausencia.documento` (`app/ponto/models.py:777`); ligação ao par: [nao verificado]. | Dia cedido passa a folga sem desconto, também no 6x1. | Aplicar a dispensa de desconto somente ao ciclo 12x36. | golden |
| G03 — substituto em folga | Aval A2: “substituto em folga, dia assumido = normal com AN/intra e SEM HE100”; `app/docs/CORTES.md:187` | Par completo, duas cartas e confirmação administrativa. `CelulaDia.colaborador=substituto` (`app/escala/models.py:1681`), `data=2026-11-03` (`app/escala/models.py:1685`), `trabalha=False` (`app/escala/models.py:1686`). Turno com período noturno e intervalo: `TipoEscala.hora_inicio=19:00` (`app/escala/models.py:50`), `hora_fim=07:00` (`app/escala/models.py:51`), `hora_inicio_intervalo=23:00` (`app/escala/models.py:52`), `hora_fim_intervalo=23:30` (`app/escala/models.py:53`). | Dia assumido passa a normal: `trabalha=True`; AN e intra seguem o tratamento de dia normal equivalente; nenhum HE100 decorre de a data ser originalmente folga. Quantias exatas: regra nao decide. | Preservar a classificação de folga trabalhada e pagar HE100; ou suprimir AN/intra por ser troca. | golden |
| G04 — pedido pelo substituto | `app/docs/CORTES.md:187`: “pode nascer de qualquer dos dois lados” | `Ausencia.colaborador=substituto` (`app/ponto/models.py:743`), `data=2026-11-03` (`app/ponto/models.py:750`), `documento=carta do substituto` (`app/ponto/models.py:777`). Identificação do cedente/dia cedido no pedido: [nao verificado]. Admin confirma com a carta do cedente e par completo. | A origem pelo substituto permite o mesmo ato confirmado: cedido folga sem desconto; assumido normal com AN/intra, sem HE100. | Aceitar somente pedidos iniciados pelo cedente. | golden |
| B01 — nenhum anexo | Aval A2: “troca confirmada sem os 2 anexos = recusa” | Tentativa de confirmação de par completo; `Ausencia.documento` vazio nos dois pedidos (`app/ponto/models.py:777`). | Confirmação recusada; nenhum par confirmado válido. Código de retorno e texto exato: regra nao decide. | Retirar a exigência documental da confirmação. | borda |
| B02 — falta carta do substituto | Aval A2: “troca confirmada sem os 2 anexos = recusa”; `app/docs/CORTES.md:187` | Par completo; `Ausencia.documento` preenchido para cedente e vazio para substituto (`app/ponto/models.py:777`); tentativa de confirmação administrativa. | Confirmação recusada. Uma carta não satisfaz as duas cartas exigidas. | Validar apenas a existência de algum documento. | borda |
| B03 — falta carta do cedente | Aval A2: “troca confirmada sem os 2 anexos = recusa”; `app/docs/CORTES.md:187` | Par completo; `Ausencia.documento` vazio para cedente e preenchido para substituto (`app/ponto/models.py:777`); tentativa de confirmação administrativa. | Confirmação recusada, independentemente do lado que iniciou o pedido. | Exigir apenas a carta de quem iniciou o pedido. | borda |
| B04 — somente um lado | Aval A2: “par incompleto (so um lado) = pedido, nao vale” | Um único lado de edição: `CelulaDia.colaborador=cedente` (`app/escala/models.py:1681`), `data=2026-11-03` (`app/escala/models.py:1685`), `trabalha=True` (`app/escala/models.py:1686`). Outro lado ausente, mesmo com cartas disponíveis em `Ausencia.documento` (`app/ponto/models.py:777`). | Continua sendo pedido; não produz os efeitos de troca confirmada. Nome do estado persistido: regra nao decide. | Considerar um lançamento SOLO suficiente para efetivar a troca. | borda |
| B05 — documentos sem confirmação | `app/docs/CORTES.md:187`: “o par so vale quando o admin confirma no Resolver dia com os dois documentos presentes” | Pedidos com `Ausencia.documento` preenchido (`app/ponto/models.py:777`); confirmação administrativa ainda não realizada. Campo próprio de confirmação do par: [nao verificado]. | Presença das cartas não efetiva a troca; permanece pedido sem os efeitos do ato confirmado. | Confirmar automaticamente quando o segundo documento chega. | borda |
| B06 — segunda confirmação | Aval A2: “segunda confirmacao do mesmo par = idempotente”; `CLAUDE.md:175` | Repetir duas vezes a confirmação das mesmas células: `CelulaDia.colaborador` (`app/escala/models.py:1681`) e `data` (`app/escala/models.py:1685`) idênticos nas duas chamadas. Chave formal do par: [nao verificado]. | Segunda chamada mantém o estado da primeira, sem nova aplicação financeira nem duplicação da trilha do mesmo ato. Resposta da segunda chamada: regra nao decide. | Executar novamente as edições, os efeitos financeiros ou a trilha a cada confirmação. | borda |
| B07 — cruza meia-noite | Aval A2: “troca cruzando meia-noite”; `app/docs/CORTES.md:187`; contrato dos marcos em `app/escala/servico_jornada.py:62` | Troca completa confirmada. `TipoEscala.hora_inicio=19:00` (`app/escala/models.py:50`), `hora_fim=07:00` (`app/escala/models.py:51`). `Batida.colaborador=substituto` (`app/ponto/models.py:22`), `timestamp=2026-11-03 19:00 / 2026-11-04 07:00` (`app/ponto/models.py:23`), `tipo=E/S` (`app/ponto/models.py:24`). | Marcos permanecem encadeados no turno iniciado em 03/11; dia assumido recebe tratamento normal com AN/intra e sem HE100 pela troca. | Cortar o turno na meia-noite e tratar a saída como outro dia de folga trabalhada. | borda |
| B08 — fronteira 20/21 | Aval A2: “troca […] cruzando competencia”; `app/docs/CORTES.md:216`; contrato da janela em `app/ponto/janelas.py:11` | Par confirmado com duas cartas. `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`). Datas dos lados em `CelulaDia.data=2026-10-20 / 2026-10-21` (`app/escala/models.py:1685`), pessoas em `colaborador` (`app/escala/models.py:1681`). | 20/10 pertence à competência 10, janela 21/09–20/10; 21/10 pertence à competência 11, janela 21/10–20/11. Cada lado conserva o efeito contratado, sem virar SOLO pela separação das competências. | Atribuir os dois lados à competência do mês civil ou filtrar o par somente pela competência da primeira data. | borda |
| B09 — meia-noite no corte | Aval A2: “troca cruzando meia-noite e cruzando competencia”; `app/docs/CORTES.md:187`; `app/escala/servico_jornada.py:62` | `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`). `Batida.colaborador=substituto` (`app/ponto/models.py:22`), `timestamp=2026-10-20 19:00 / 2026-10-21 07:00` (`app/ponto/models.py:23`), `tipo=E/S` (`app/ponto/models.py:24`). Par completo confirmado com duas cartas. | Continua sendo turno cruzando meia-noite e dia assumido normal, sem HE100 pela troca. Rateio de AN/intra entre competências: regra nao decide. | Reclassificar como HE100 a parcela após 00:00 por ter mudado a competência. | borda |
| B10 — virada do ano | `app/ponto/janelas.py:11`; `app/docs/CORTES.md:216`; `app/docs/CORTES.md:187` | Par confirmado. `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`); `CelulaDia.data=2026-12-20 / 2026-12-21` (`app/escala/models.py:1685`), `colaborador` dos respectivos lados (`app/escala/models.py:1681`). | 20/12 pertence à competência 12/2026; 21/12 pertence à competência 01/2027. Mantêm-se os efeitos do par. | Incrementar o mês sem avançar o ano ou usar dezembro para ambas as datas. | borda |
| B11 — vínculo trocado | Regra do vínculo do dia em `app/escala/alimentacao.py:231`; limite da vigência em `app/escala/alimentacao.py:272`; O10 em `app/docs/CORTES.md:187` | `CelulaDia.data=2026-10-20 / 2026-10-21` (`app/escala/models.py:1685`), `escala_geradora=vínculo antigo / novo` (`app/escala/models.py:1692`). Vínculo antigo: `data_fim=2026-10-20` (`app/escala/models.py:876`), `ativa=False` (`app/escala/models.py:877`); novo: `data_inicio=2026-10-21` (`app/escala/models.py:875`). | Consulta do vínculo de 20/10 retorna o antigo; de 21/10, o novo, conforme geradoras e vigências. Aceitar a troca entre vínculos diferentes: regra nao decide. | Resolver ambos os dias pelo vínculo atualmente ativo. | borda |
| B12 — vazio | Aval A2: “par incompleto (so um lado) = pedido, nao vale”; `app/docs/CORTES.md:187`: “ATO com par” | Conjunto de lados vazio: nenhum `CelulaDia` selecionado (modelo em `app/escala/models.py:1635`). Campos do formulário de dia cedido/colega: [nao verificado]. | Não existe troca confirmada válida sem par. Retorno, mensagem e eventual criação de pedido vazio: regra nao decide. | Tratar uma seleção vazia como confirmação bem-sucedida de troca. | borda |
| A01 — duas confirmações concorrentes | Aval A2: “segunda confirmacao do mesmo par = idempotente”; aval FATIA B: “em atomic”; `CLAUDE.md:175` | Duas confirmações simultâneas do mesmo par completo, com duas cartas. Mesmos `CelulaDia.colaborador` (`app/escala/models.py:1681`) e `data` (`app/escala/models.py:1685`). Identidade persistida do par: [nao verificado]. | Estado final equivale a uma confirmação; sem duplicar efeitos ou trilha do mesmo ato; nenhum meio par confirmado. Qual chamada vence e quais respostas retornam: regra nao decide. | Deixar ambas as requisições aplicarem o ato integralmente após lerem o mesmo estado inicial. | adversarial |
| A02 — falha entre os lados | Aval FATIA B: “escrevendo o PAR pelas duas edicoes de celula do escritor existente, em atomic, com trilha (usuario, antes/depois, motivo)” | Par com `CelulaDia.colaborador` (`app/escala/models.py:1681`), `data` (`app/escala/models.py:1685`) e `trabalha` (`app/escala/models.py:1686`); falha provocada depois da primeira edição e antes da segunda. Trilha: `LogAuditoria.usuario` (`app/core/models.py:295`), `valor_antes` (`app/core/models.py:300`), `valor_depois` (`app/core/models.py:301`), `descricao` (`app/core/models.py:299`). | Após a falha, as duas células mantêm os valores anteriores; não existe trilha afirmando um par concluído. Resposta e registro da tentativa falha: regra nao decide. | Confirmar a primeira edição antes de executar a segunda, deixando estado parcial persistido. | adversarial |
| A03 — legado aprovado SOLO | `app/docs/BACKLOG.md:1224`: censo corrigido de 16 SOLO e conversão por “DRY -> aval”; aval A2: “par incompleto […] nao vale” | Fixture derivada do censo fornecido: 16 `Ausencia`, com `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data_fim=None` (`app/ponto/models.py:751`), `status` distribuído em 12 aprovadas, 2 rejeitadas e 2 aguardando (`app/ponto/models.py:799`). `TipoAusencia.efeito='abona'` (`app/ponto/models.py:581`). Sem referência ao outro lado: [nao verificado]. | As 12 aprovações legadas e o efeito `abona` não comprovam pares confirmados. DRY não grava conversão; conversão depende do aval previsto. Destino individual das 16 linhas: regra nao decide. | Promover automaticamente todo SOLO aprovado a troca confirmada, sem par, cartas ou aval de conversão. | adversarial |
| A04 — colega sem vínculo no dia | `app/escala/alimentacao.py:209`; `app/docs/CORTES.md:187` | Colega sem célula geradora na data. `EscalaColaborador.colaborador=colega` (`app/escala/models.py:872`), `data_inicio=2026-10-01` (`app/escala/models.py:875`), `data_fim=None` (`app/escala/models.py:876`), `ativa=False` (`app/escala/models.py:877`). Data consultada: 03/11. | `vinculo_cobre_a_data` retorna falso: vínculo desativado sem fim declarado não cobre a data. Confirmar ou recusar a troca nessa situação: regra nao decide. | Considerar suficiente `data_inicio` anterior ao dia e ignorar a ausência de vigência declarada. | adversarial |
| A05 — dia cedido já trabalhado | `app/docs/CORTES.md:187`: “folga trocada sem desconto”; `app/docs/BACKLOG.md:1248`: “a troca NAO cria desconto novo” | `CelulaDia.colaborador=cedente` (`app/escala/models.py:1681`), `data=2026-11-03` (`app/escala/models.py:1685`). Batidas existentes: `Batida.colaborador=cedente` (`app/ponto/models.py:22`), `timestamp=03/11 07:00 / 19:00` (`app/ponto/models.py:23`), `tipo=E/S` (`app/ponto/models.py:24`). Tentativa de troca completa. | Aceitar ou recusar diante do trabalho já realizado: regra nao decide. A troca não pode introduzir desconto novo para resolver essa colisão. | Gerar falta ou outro desconto novo ao encontrar batidas no dia cedido. | adversarial |
| A06 — célula já editada | Aval FATIA B: “com trilha (usuario, antes/depois, motivo)”; `app/docs/CORTES.md:187` | `CelulaDia.origem='editada'` (`app/escala/models.py:1691`), `trabalha=True` (`app/escala/models.py:1686`), `dna` previamente alterado (`app/escala/models.py:1697`), `data=2026-11-03` (`app/escala/models.py:1685`). Tentativa de confirmar par completo. | Sobrescrever ou recusar a edição humana anterior: regra nao decide. Se a troca for efetivada, a trilha identifica usuário, estado anterior real, estado posterior e motivo. | Registrar como “antes” o template gerado, apagando da trilha o estado humano efetivamente substituído. | adversarial |
| A07 — competência exportada | Aval PROIBIDO: “tocar exportada sem !” | `FechamentoMensal.colaborador` (`app/ponto/models.py:405`), `mes=9` (`app/ponto/models.py:408`), `ano=2026` (`app/ponto/models.py:409`), `status='exportado'` (`app/ponto/models.py:410`). Tentativa de efetivar troca nessa competência sem `!`. | No escopo deste aval, nenhuma alteração da competência exportada é autorizada. Recusa automática, código HTTP e mensagem: regra nao decide. | Aplicar a troca em competência exportada usando a autorização comum da obra. | adversarial |
| A08 — S5b por colaborador | `app/docs/BACKLOG.md:1248`: “a troca NAO cria desconto novo” e “todo outro campo de todo colaborador da ZERO”; `app/docs/PROMPTS.md:341` | Dois colaboradores sintéticos, um com troca e outro de controle. Linhas `DiaPago.colaborador` (`app/ponto/models.py:315`), `data` (`app/ponto/models.py:319`), `horas_atraso` (`app/ponto/models.py:334`), `horas_saida_antecipada` (`app/ponto/models.py:335`), `horas_falta` (`app/ponto/models.py:336`), comparadas antes/depois, mantendo os demais fatos. | Nenhum colaborador recebe desconto novo atribuível à troca. Redução em outro colaborador não compensa aumento indevido no primeiro. | Validar apenas o total da frota, permitindo descontos positivos individuais cancelados por diferenças negativas. | adversarial |
| A09 — selo de confirmação inválida | Aval FATIA B: “selo: troca confirmada sem 2 anexos = VERMELHO”; `app/docs/CORTES.md:187` | Estado inconsistente apresentado ao selo: confirmação do par [nao verificado]; `Ausencia.documento` vazio em pelo menos um dos lados (`app/ponto/models.py:777`). | Selo VERMELHO, mesmo que o estado apresentado se declare confirmado. Forma exata do retorno do selo: regra nao decide. | Pintar verde apenas pelo indicador de confirmação, sem conferir as duas cartas. | adversarial |

- **regras que voce NAO encontrou escritas:** identidade formal do par, campos de ligação entre os lados e representação persistida de “pedido”/“confirmada”; procurados em `app/escala/models.py:1635`, `app/ponto/models.py:660` e `app/docs/CORTES.md:187`; definição específica: [nao verificado].
- **regras que voce NAO encontrou escritas:** aceitação ou recusa de troca com dia já trabalhado, edição humana anterior, colega sem vínculo ou vínculos diferentes; procuradas no contrato `app/docs/CORTES.md:187` e nas regras de vínculo `app/escala/alimentacao.py:231`; política específica O10: [nao verificado].
- **regras que voce NAO encontrou escritas:** vencedor, respostas e tratamento de conflito entre confirmações concorrentes de pares diferentes; procurados no aval FATIA B “em atomic” e em `CLAUDE.md:175`; política específica: [nao verificado].
- **regras que voce NAO encontrou escritas:** datas iguais, invertidas, inválidas, futuras, distância máxima entre os dias e mensagem para entrada vazia; procuradas em `app/docs/CORTES.md:187`; critérios específicos: [nao verificado].
- **regras que voce NAO encontrou escritas:** quantias exatas de AN/intra, rateio do turno entre competências e demais efeitos financeiros de um dia cedido já trabalhado; procurados em `app/docs/CORTES.md:187` e `app/docs/BACKLOG.md:1248`; definição específica: [nao verificado].
- **regras que voce NAO encontrou escritas:** códigos HTTP, textos de recusa, validação de autenticidade/unicidade das cartas e destino individual das 16 linhas legadas; procurados em `app/docs/BACKLOG.md:1224` e `app/docs/CORTES.md:187`; definição específica: [nao verificado].

## 06/11 09:17 · sessao 07958c9b883b · LEI-FIO-MUDO-B · verificador · origem=Code

**Parecer: a remoção resolve o callback específico, mas a obra está incompleta para garantir a lei em todo o fluxo de abertura.** Permanecem reabertura automática alcançável pelo renascimento da cobrança, alarme F2 incompatível com a sobra agora permitida e lacunas relevantes no R08f. [app/chamados/models.py:1495; app/chamados/models.py:1625; app/chamados/management/commands/auditar_invariantes_chamados.py:1761; app/chamados/tests/test_c20_fio_mudo.py:247]

Execução dos testes, correspondência da cópia com o HASH/HEAD informado, quantidade afetada em produção e ocorrência efetiva de push: **[nao verificado]**. [nao verificado]

**1. O que está errado ou incompleto**

- **A proteção ficou na adoção, mas não cobre todo `abrir`.** O detector procura cobrança por módulo/data sem filtrar status; `abrir` pode encontrar encerrado e fazê-lo renascer; `reabrir` chama `ressuscitar_perguntas` **antes** da adoção. Essa ressuscitação ainda reabre disputas FECHADAS. É caminho condicional aos guardas existentes, não efeito obrigatório de qualquer abertura. [app/ponto/management/commands/detectar_ausencias.py:693; app/chamados/models.py:974; app/chamados/models.py:1073; app/chamados/models.py:1495; app/chamados/models.py:1625]
- **O F2 ainda alarma o estado que a lei aceita.** Seleciona pergunta viva, não validada/materializada, em disputa fechada de colaborador ativo, e termina com `ALARME`/exit 2. Continua agendado; o teste antigo exige a violação. Isso contradiz o contador escolhido pelo delta, cujo dono é ADMIN e que expressamente não alarma. [app/chamados/management/commands/auditar_invariantes_chamados.py:1761; app/chamados/management/commands/auditar_invariantes_chamados.py:1853; app/config/crons.py:542; app/chamados/tests/test_hx_inv_fio.py:75; app/chamados/services/vigia_fio.py:740]
- **“Só Validar” não está demonstrado para todo o modal.** O template ainda oferece “Reabertura administrativa” ao staff/admin_ti, além das ações da pergunta; o backend dessa ação reabre e envia FCM. No chamado novo que apenas herda a disputa, o backend procura uma disputa cujo próprio container seja aquele chamado; pelo container antigo, a ação continua disponível. É comportamento anterior deixado pelo delta, não reabertura automática da adoção. [app/templates/chamados/partials/modal_fio.html:377; app/templates/chamados/partials/modal_fio.html:385; app/chamados/services/acoes_disputa.py:252; app/chamados/services/acoes_disputa.py:256; app/chamados/services/acoes_disputa.py:283]
- **A pergunta sem resposta não ganha uma saída “Validar”.** O modal mostra “Aguardando resposta do colaborador”; a validação recusa `respondida_em` ausente. O R08f evita esse caso preenchendo a resposta. O destino operacional dessa classe sob a lei literal está **[nao verificado]**. [app/templates/chamados/partials/modal_fio.html:314; app/chamados/services/validacao.py:363; app/chamados/tests/test_c20_fio_mudo.py:247]
- **A docstring dos mocks promete cobertura maior que a existente**, e o topo do RELATO ainda apresenta a decisão como pendente e o callback removido como existente. [app/chamados/tests/test_c20_fio_mudo.py:218; app/ponto/management/commands/processar_alertas_turno.py:251; app/docs/RELATO.md:13]

**2. Quem escreve e quem lê**

Abaixo distingo persistência, leitura e alteração apenas em memória; a FK reversa `perguntas_que_resolvem` também lê a relação `chamado_destino`. [app/chamados/models.py:2882]

| Alvo | Escritores/definição | Leitores/chamadores principais |
|---|---|---|
| `_adotar_vivas_na_abertura` | Definida em `app/chamados/models.py:434`; costura por `fio.reapontar_resolvedora` em `app/chamados/models.py:502`. | `abrir`: `app/chamados/models.py:1027`; `reabrir`: `app/chamados/models.py:1496`; chamadas diretas nos testes: `app/chamados/tests/test_c20_fio_mudo.py:188,316,333,460`. |
| Função removida `reabrir_continentes_das_reatadas` | A lápide registra sua remoção e a do chamador. | Restam referências nas lápides e no RELATO desatualizado: `app/chamados/models.py:517`; `app/chamados/services/fio.py:899`; `app/docs/RELATO.md:13`. |
| `DisputaSupervisao.fechada_em` | `fechar`: `app/chamados/models.py:1955`; `reabrir_admin`: `app/chamados/models.py:2157`; `reabrir_sistema`: `app/chamados/models.py:2181`. | Censo abaixo; a property `aberta` lê em `app/chamados/models.py:1859`. |
| `DisputaSupervisao.resolucao` | `fechar`: `app/chamados/models.py:1957`; `reabrir_admin`: `app/chamados/models.py:2159`; `reabrir_sistema`: `app/chamados/models.py:2183`; `limpar_resolucao_residual`: `app/chamados/models.py:2267`. | Idempotência/limpeza: `app/chamados/models.py:1915,2264,2266`; ocorrência no fio: `app/chamados/signals.py:45`; fallback do materializador: `app/chamados/services/materializacao.py:1253`; snapshot: `app/chamados/management/commands/reabrir_orfas_enterradas.py:132`. |
| `PerguntaDisputa.chamado_destino` | Nascimento: `app/chamados/models.py:2571`; reaponte de existente no gate de identidade: `app/chamados/models.py:2544`, salvo em `:2568`; `reapontar_resolvedora`: `app/chamados/services/fio.py:845`, salvo em `:847`. | Censo abaixo; autoridade da âncora: `app/chamados/services/fio.py:117`. |

**Há dois lugares que reapontam pergunta existente**, embora a docstring de `reapontar_resolvedora` reivindique exclusividade. É anterior ao delta: o gate `PerguntaDisputa.abrir` também escreve e salva o destino de uma identidade encontrada. [app/chamados/services/fio.py:787; app/chamados/services/fio.py:845; app/chamados/models.py:2544; app/chamados/models.py:2568]

As atribuições em `models.py:350` e `fio.py:773,777` são atualização/simulação em memória; não são persistência adicional desse campo. [app/chamados/models.py:349; app/chamados/services/fio.py:772]

Censo dos acessos executáveis a **`fechada_em`**, incluindo filtros, ordenação e carregamento; referências agrupadas por arquivo:

```text
app/chamados/models.py:184,1625,1831,1834,1841,1859,1914,1918,2041,2146,2175,2232,2264
app/chamados/signals.py:28,106,130
app/chamados/reconciliador.py:411,589,600,601,1038,1149,1150,1487
app/chamados/views.py:471,495,512,530,911,1329
app/chamados/views_cobrar.py:161,164
app/chamados/relatorios_disputa.py:41
app/chamados/juizes.py:2080
app/chamados/services/fio.py:131,135,184,596,629
app/chamados/services/relavra_via.py:181,244
app/chamados/services/acoes_disputa.py:252,343
app/chamados/services/becos.py:27,51,54
app/chamados/services/competencia_trancada.py:151,196
app/chamados/services/disputa_emissao.py:38,468,936,1157,1311,1575,1621,1645,1647,1695
app/chamados/services/materializacao.py:567,1241
app/chamados/services/validacao.py:140,196,199,201
app/chamados/services/acoes_chamado.py:336
app/chamados/services/fila_por_causa.py:225,259,262
app/chamados/services/vigia_fio.py:748
app/chamados/services/cobranca.py:230
app/chamados/services/regularizacao_ext.py:59
app/chamados/management/commands/fechar_becos_disputa.py:32
app/chamados/management/commands/auditar_chamados_orfaos.py:40
app/chamados/management/commands/encerrar_perguntas_com_fato.py:82
app/chamados/management/commands/auditar_invariantes_chamados.py:536,592,806,1762
app/chamados/management/commands/reabrir_orfas_enterradas.py:96,131,154,155
app/chamados/management/commands/auditar_disputas.py:72,87,136,152,167
app/api/foto_recebida.py:62
app/api/views.py:1008,1030,2694
app/colaboradores/painel_cell.py:32
app/colaboradores/signals.py:73
app/colaboradores/services/situacional.py:117
app/colaboradores/services/drawer.py:70
app/colaboradores/services/desligamento.py:139
app/escala/signals.py:157
app/ponto/services/entrada_sincronizada.py:56
app/ponto/management/commands/escalonar_chamados_supervisao.py:58,149
app/ponto/management/commands/reconciliar_fantasmas.py:214
app/ponto/management/commands/alertar_disputas_pendentes.py:34
app/templates/chamados/partials/modal_fio.html:374
bin/censo_folha.py:28
```

Leituras indiretas por **`aberta`**, cuja fonte é `fechada_em`: montagem/flags do fio, representação do modelo e escolha do ramo no modal. [app/chamados/services/fio.py:191,193,197,213,231,338; app/chamados/models.py:2279; app/templates/chamados/partials/modal_fio.html:321]

Censo dos acessos executáveis diretos a **`chamado_destino`**:

```text
app/chamados/models.py:287,293,294,328,344,345,349,419,501,2532
app/chamados/juizes.py:743,803,817,1579,1706,1750,1752,2030
app/api/views.py:1010,1045,1046,2574,2590,2592
app/chamados/views_cobrar.py:165
app/chamados/reconciliador.py:641,1040,1485
app/colaboradores/services/calendario.py:474
app/ponto/management/commands/reconciliar_grade.py:460
app/ponto/management/commands/reconciliar_fantasmas.py:212
app/chamados/services/vigia_carimbo.py:103
app/chamados/services/materializacao.py:339,526,563,564,686,764,964,1046,1075,1077,1167,1188,1190
app/chamados/services/validacao.py:795
app/chamados/services/pergunta_no_app.py:140,163,166
app/chamados/services/limbo_folgas.py:203,238,239
app/chamados/services/declaracao_texto.py:314
app/chamados/services/acoes_chamado.py:357
app/chamados/services/disputa_emissao.py:210,404,909,929,1244,1573,1653
app/chamados/services/explicador.py:122
app/chamados/services/competencia_trancada.py:108,118,140
app/chamados/services/raiox.py:55,60,135
app/chamados/services/becos.py:30,46,47
app/chamados/services/janela_colab.py:64,65
app/chamados/services/acoes_disputa.py:350
app/chamados/services/fio.py:80,81,117,135,223,224,327,392,393,772
app/chamados/services/relavra_via.py:99,101,154
app/chamados/services/historico.py:96
app/chamados/management/commands/auditar_invariantes_chamados.py:526,532,537,588,589,595,856,892,893,896,1159,1573
app/chamados/management/commands/reabrir_orfas_enterradas.py:69
app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:87,117,119
app/chamados/management/commands/relavrar_via_i6.py:56
app/chamados/management/commands/auditar_disputas.py:151
app/chamados/management/commands/lavrar_perguntas_stale.py:57,86
app/chamados/management/commands/backfill_veredito_lavrado.py:101
```

Leituras pela **FK reversa**, relevantes para não perder consumidores sem o nome literal do campo:

```text
app/chamados/models.py:1528,1593
app/chamados/services/fio.py:938,940
app/chamados/services/disputa_emissao.py:695
app/chamados/management/commands/auditar_invariantes_chamados.py:1042,1043,1627,1649
app/chamados/management/commands/disparar_perguntas_competencia.py:147,148
app/ponto/supra_juiz.py:547
app/ponto/management/commands/supra_juiz.py:192,215
app/ponto/management/commands/emitir_furo_retroativo.py:83,98
app/ponto/services/cartorio.py:118,1339,1594,1614
```

Fornecem o destino à porta de nascimento os serviços de declaração e emissão; não são escritores independentes da coluna. [app/chamados/services/declaracao_texto.py:326,615; app/chamados/services/disputa_emissao.py:395,776,1201; app/chamados/models.py:2571]

As migrações declaram esquema, não atualização de dados: `fechada_em/resolucao` em `0013`, destino em `0016`. [app/chamados/migrations/0013_disputasupervisao.py:24,25; app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:18]

Há escritores crus de **fixture**, incluindo o novo `update` e os testes de signals/ressuscitação; os selos inspecionam escritores e estado. Isso não constitui segundo escritor de produção. [app/chamados/tests/test_c20_fio_mudo.py:245; app/chamados/tests/test_signals.py:62,73,94,107,121,141,153,165,178,193,213,230,250,268,290; app/chamados/tests/test_selo_i8_fio_solto.py:201; app/ponto/tests/test_hx_cartorio_ressuscita.py:87; app/chamados/tests/test_selo_resolucao_um_escritor.py:44; app/core/tests/test_contract_escritores_estado.py:61]

A classificação semântica exaustiva de **todos os acessos dentro de todos os testes**, além dos identificados acima, permanece **[nao verificado]**; não equiparo busca lexical a censo de escritores/leitores. [nao verificado]

**3. Caminhos de reabertura e push; alcance pela abertura** [nao verificado]

| Caminho | Efeito | Alcançado pelo fluxo perguntado? |
|---|---|---|
| `reapontar_resolvedora` | Salva destino/lavra; o signal agenda fechamento de beco, que exclui FECHADA. [app/chamados/services/fio.py:845; app/chamados/signals.py:191; app/chamados/services/becos.py:27] | **Sim**, diretamente pela adoção; não reabre nem envia push nesse caminho. [app/chamados/models.py:502] |
| `migrar_resolvedoras_vivas` | Reaponta/retrata; sem reabertura direta da disputa. [app/chamados/services/fio.py:944,954] | Pode ser alcançado pelo ramo “irmão vivo” de `reabrir`, que retorna antes da adoção. [app/chamados/models.py:1465,1480] |
| `ressuscitar_perguntas` | Descarimba em lote e reabre FECHADAS por `reabrir_sistema`; exige materializada, sem texto, marco/C6 permitindo. [app/chamados/models.py:1528,1531,1546,1552,1598,1625] | **Sim**, no renascimento via `abrir → reabrir`, antes da adoção. [app/chamados/models.py:974,1073,1495] |
| Ressuscitação pelo cartório | Cobrança viva sem fio e sem resposta pode entrar na mesma porta. [app/ponto/services/cartorio.py:1591,1595,1597,1622] | **Sim pela vigia**, condicional à seleção; não é consequência obrigatória do reapontamento. [app/ponto/services/vigia_de_hora.py:131] |
| Fábrica de disputa após abertura | Usa `notificar=True`; pode enviar push se a disputa permanecer aberta. Disputa vazia pode fechar e retornar antes do envio. [app/chamados/services/disputa_emissao.py:876,1112,1116,1123] | **Sim**: detector chama a fábrica após coletar órfãos. O filtro para marcos da ata permite passagem mesmo existindo pergunta não materializada. [app/ponto/management/commands/detectar_ausencias.py:790; app/chamados/services/disputa_emissao.py:925,928] |
| `desfazer_carimbo_sem_lastro` | Default permite reabrir FECHADA; chamada do julgamento passa `reabrir_fechada=False`. [app/chamados/services/relavra_via.py:185,244; app/chamados/services/fabrica_da_celula.py:166] | Não pela adoção diretamente; o uso protegido do julgamento não reabre FECHADA. [app/chamados/models.py:502; app/chamados/services/fabrica_da_celula.py:166] |
| Validação recusada | Default reabre; fio do admin passa `sem_reabrir` quando fechada. [app/chamados/services/validacao.py:199; app/chamados/services/fio.py:596,605] | Não pela adoção; depende de validação posterior. [app/chamados/services/validacao.py:425] |
| Ausência rejeitada | Devolve perguntas absorvidas e reabre suas disputas. [app/ponto/signals.py:261; app/chamados/services/materializacao.py:658,661] | Ato próprio de ausência; não chamado pela adoção. [app/chamados/models.py:502; app/chamados/services/materializacao.py:638] |
| `reabrir_orfas_enterradas --apply` | Reabre fantasma e disputa fechada. É comando de uma vez. [app/chamados/management/commands/reabrir_orfas_enterradas.py:147,155; app/config/crons.py:785] | Não chamado pela adoção. [app/chamados/models.py:502] |
| Reabertura administrativa | Reabre disputa, cria pergunta e envia FCM. [app/chamados/services/acoes_disputa.py:256,259,283] | Ação humana posterior, não automática na adoção. [app/chamados/services/acoes_disputa.py:242,245,250] |
| Desarquivar/reabrir competência | Chamam `Chamado.reabrir`; desarquivar pela tranca também ressuscita depois de levantar o véu. [app/chamados/views_arquivar.py:70,86; app/chamados/services/competencia_trancada.py:250,265] | Outros atos; podem alcançar a ressuscitação sobrevivente. [app/chamados/models.py:1495] |
| `reabrir_por_colab` | **Recusa FECHADA**; reabre apenas questionário de disputa aberta. [app/chamados/models.py:2232,2240] | Não contorna o fechamento nesse caso. [app/chamados/models.py:2233] |

**Existe ainda o push posterior do cron diário:** ele seleciona disputas abertas e envia `push_colaborador`, sem exigir pergunta pendente/C6 nesse laço. Portanto uma reabertura sobrevivente habilita cobrança posterior que o R08f não executa; o agendamento é 08h. [app/ponto/management/commands/alertar_disputas_pendentes.py:33,48,59; app/config/crons.py:745]

Os signals de `DisputaSupervisao` tratam fechamento/materialização/retomada; o de pergunta agenda beco; o de chamado pode entrar no cartório. Não há neles uma nova reabertura direta inserida pelo delta. [app/chamados/signals.py:18,97,124,164,191; app/chamados/models.py:516]

**4. Órfãos e comportamento antigo**

- **Referência operacional órfã:** topo de `RELATO.md` ainda cita a função removida, callback e espera pela escolha A/B. [app/docs/RELATO.md:13]
- **Lápides intencionais:** as referências em `models.py` e `fio.py` declaram explicitamente a remoção; não são imports quebrados. [app/chamados/models.py:516; app/chamados/services/fio.py:899]
- **Regra antiga ampla:** MAPA e docstring de `relavra_via` dizem que ressuscitar em FECHADA religa a disputa. Ainda descrevem código sobrevivente, mas não explicitam a exceção da adoção muda. [app/docs/MAPA/chamado.md:89; app/chamados/services/relavra_via.py:69; app/chamados/services/relavra_via.py:244]
- **Outro texto impreciso:** `devolver_validacao_recusada` descreve reabertura da fechada, embora possua a exceção `reabrir=False`. [app/chamados/services/validacao.py:171,197]
- **Teste/contador incompatível:** F2 e seu teste continuam exigindo alarme da viva em fechada; não são apenas documentação histórica. [app/chamados/management/commands/auditar_invariantes_chamados.py:1761; app/chamados/tests/test_hx_inv_fio.py:75]
- **Teste do comportamento sobrevivente:** o selo I8 exige reabertura após desfazer carimbo; não ficou importando função apagada. [app/chamados/tests/test_selo_i8_fio_solto.py:192,205] [nao verificado]
- **Contador escolhido não ficou órfão:** conta pelo C6, tem dono ADMIN, não alarma e é consumido por `vigiar_fio_morto`. O GLOSSARIO conserva referência a ele. [app/chamados/services/vigia_fio.py:737,748,755; app/chamados/management/commands/vigiar_fio_morto.py:174,225; app/docs/GLOSSARIO.md:28]

**5. O R08f morde?**

**Morde a volta do callback original nesse cenário:** exige reapontamento, preservação conjunta de fechamento/resolução e executa callbacks antes das assertivas. [app/chamados/tests/test_c20_fio_mudo.py:253,258,260]

Não certifica os seguintes mutantes plausíveis:

| Mutante | Por que escaparia deste teste | [nao verificado]
|---|---|
| Reabrir somente pergunta **sem resposta** | O teste força `resposta_colab/respondida_em`. [app/chamados/tests/test_c20_fio_mudo.py:247] | [nao verificado]
| Reabrir em renascimento, ressuscitação ou cron posterior | R08f exercita nascimento pelo vigia; R08r testa `reabrir`, mas sem fechamento/push como invariantes. [app/chamados/tests/test_c20_fio_mudo.py:150,168,253] | [nao verificado]
| Mandar push por wrapper padrão | A fixture não configura usuário/token; wrappers podem retornar antes dos mocks inferiores. [app/chamados/tests/test_pergunta_do_dia_em_curso.py:34; app/chamados/fcm_utils.py:104; app/chamados/push_utils.py:116] |
| Colocar a pergunta no **card antigo** do admin | O teste achata perguntas de todos os cards e verifica presença global no HTML; não exige que esteja sob `ch.pk`. [app/chamados/tests/test_c20_fio_mudo.py:266,276; app/chamados/services/fio.py:236] |
| Exibir `corrigir_resposta` indevidamente | Fixture é `escolha`; Corrigir pertence ao ramo `hora`; teste só proíbe `recusar_pergunta`. [app/chamados/tests/test_c20_fio_mudo.py:65,278; app/templates/chamados/partials/modal_fio.html:295] |
| Reabrir/enviar push no **POST Validar** | Só faz GET; mocks e verificação do fechamento já terminaram. Remover `sem_reabrir` da delegação não seria exercitado. [app/chamados/tests/test_c20_fio_mudo.py:253,260,275; app/chamados/services/fio.py:605] |

**Os mocks não cobrem todos os canos da árvore.** Há `pywebpush.webpush` direto ao colaborador em comandos de alertas, fora dos três alvos. A chamada desses comandos pela adoção não foi demonstrada; sua execução nesse fluxo é **[nao verificado]**. [app/chamados/tests/test_c20_fio_mudo.py:224; app/ponto/management/commands/processar_alertas_turno.py:247,251,288; app/ponto/management/commands/processar_alertas_avancados.py:195,212]

Imports de wrappers por outros módulos não furam, por si, 

[truncado em 20000 de 21456 caracteres]

## 06/11 09:16 · sessao 5a281d793e06 · - · - · origem=Ronald (tmux)

Q5 — `troca_turno` e `troca_de_plantao`

| Item | Escritores / leitores / efeito |
|---|---|
| `troca_turno` pertence a `Justificativa.tipo` | Choice explícito em `app/ponto/models.py:126`; migration de schema `app/ponto/migrations/0019_tipos_justificativa.py:16`; opção web `app/templates/ponto/justificativa_form.html:43`; opção API `app/api/views.py:2070`; rótulo no histórico API `app/api/views.py:2099`, consumido em `app/api/views.py:2111`. |
| Quem grava `Justificativa.tipo` informado pelo humano | API lê `tipo` e atualiza/cria a pendente em `app/api/views.py:1955`, `app/api/views.py:2001`, `app/api/views.py:2007`; web faz equivalente em `app/ponto/views.py:1591`, `app/ponto/views.py:1625`, `app/ponto/views.py:1635`. Esses caminhos recebem o valor enviado; não há tratamento específico de troca nesses trechos. |
| Outros escritores capazes de editar a justificativa | Django admin registra `JustificativaAdmin(admin.ModelAdmin)` em `app/ponto/admin.py:18`, com tipo e status na listagem em `app/ponto/admin.py:20`; não herda `SoLeitura`, diferentemente de Ausencia em `app/ponto/admin.py:37`. |
| Quem decide a aprovação | Individual: view delega em `app/ponto/views.py:1915`; serviço abre atomic/lock em `app/ponto/services/justificativa.py:125`, grava status/usuário/data em `app/ponto/services/justificativa.py:190`, invalida fechamento em `app/ponto/services/justificativa.py:199`. Lote: atomic por item em `app/ponto/views.py:1792`, status/carimbos em `app/ponto/views.py:1796`, invalidação em `app/ponto/views.py:1804`. Materialização retroativa também aprova em `app/ponto/services/justificativa.py:368`. |
| Pode criar batida? | A aprovação individual cria S manual se há `saida_informada` **e** entrada, sem condicionar ao tipo, em `app/ponto/services/justificativa.py:137`, `app/ponto/services/justificativa.py:172`. Lote exige saída informada e ausência de batida gerada em `app/ponto/views.py:1808`. API que anuncia `troca_turno` marca `requer_saida=False` em `app/api/views.py:2070`; seu POST não atribui saída em `app/api/views.py:2007`. |
| Efeito financeiro de justificativa aprovada | **Não possui campo `efeito='abona'`.** Fechamento lê qualquer justificativa aprovada por data, sem filtrar tipo, para `datas_justificadas`: `app/ponto/services/fechamento.py:651`. Serviço do espelho faz equivalente em `app/ponto/services/efeito_ausencia.py:36`. Motor exclui dessas datas as faltas injustificadas e conta abono se não houve trabalho em `app/ponto/motor_calculo_v2.py:2600`, `app/ponto/motor_calculo_v2.py:2606`; `dsr_ok = not faltas_injustificadas` em `app/ponto/motor_calculo_v2.py:2605`. Não escreve o par de células nesses caminhos: aprovação grava Justificativa/batida em `app/ponto/services/justificativa.py:172`, `app/ponto/services/justificativa.py:190`. |
| SLA de `troca_turno` | Prazo 3 dias em `app/ponto/catalogo/justificativas.py:24`, função em `app/ponto/catalogo/justificativas.py:29`; leitura pela fila em `app/ponto/views.py:1726` e cron em `app/ponto/management/commands/alertar_justificativas_sla.py:19`. É prazo do DP, não cálculo monetário, conforme contrato em `app/ponto/catalogo/justificativas.py:9`. |
| `troca_de_plantao` | O literal só apareceu em documentação na busca da cópia, não em implementação runtime nem migrations; origem histórica exata do cadastro fornecido pelo usuário = **[nao verificado]**. O mecanismo atual aceita códigos cadastrados: valida `codigos_validos()` em `app/ponto/services/ausencia.py:193`, exclui `tipo` do full_clean por sua autoridade ser o catálogo em `app/ponto/services/ausencia.py:361`. |
| Escritor de `TipoAusencia` | View aceita nome, efeito e flags em `app/ponto/views_tipos_ausencia.py:61`, `app/ponto/views_tipos_ausencia.py:67`; chama `gravar_tipo_ausencia` em `app/ponto/views_tipos_ausencia.py:86`, `app/ponto/views_tipos_ausencia.py:94`. Porta grava existente ou cria em `app/ponto/services/ausencia.py:1285`. Código imutável nessa edição em `app/ponto/views_tipos_ausencia.py:84`. |
| Escritor de Ausencia desse tipo cadastrado | Criar recebe código validado e monta tipo, datas, minutos, documento, status e autor em `app/ponto/services/ausencia.py:341`; API chama em `app/api/views.py:1820`, admin em `app/ponto/views.py:2415`, PWA em `app/ponto/views.py:2536`. Edição genérica grava tipo/datas/minutos em `app/ponto/services/ausencia.py:497`; decisão passa por `mudar_estado` em `app/ponto/services/ausencia.py:895`. |
| De quem é `efeito='abona'` e quem o lê | É de **TipoAusencia**, campo em `app/ponto/models.py:581`; `Ausencia.efeito` consulta efeito vivo em `app/ponto/models.py:840`. Catálogo lê cadastro ativo em `app/ponto/catalogo/ausencias.py:249`, `efeito_vigente` devolve a primeira posição em `app/ponto/catalogo/ausencias.py:293`. Portanto o nome customizado não precisa de ramo específico. |
| Efeito financeiro de Ausencia aprovada com efeito abona | Cobertura para folha filtra só aprovada em `app/ponto/turnos.py:2514`. Fechamento lê `a.efeito` e inclui dia em `datas_justificadas` em `app/ponto/services/fechamento.py:684`; pagas parciais entram como crédito de minutos em `app/ponto/services/fechamento.py:664`. Ausencia inteira abonada alimenta teto que zera pontualidade em `app/ponto/turnos.py:2681`, selecionada por efeito em `app/ponto/turnos.py:2689`. Export inclui tipos abonados, menos exceções, para rubrica 8932 em `app/folha/export.py:193`, `app/folha/export.py:201`; exceções em `app/ponto/catalogo/ausencias.py:290`. |
| Migrations de dados | Seed `TipoAusencia` contém os 17 códigos em `app/ponto/migrations/0052_seed_tipoausencia.py:9`, grava por `get_or_create` em `app/ponto/migrations/0052_seed_tipoausencia.py:33`; não contém `troca_de_plantao`. Backfill genérico converte antigas pendentes segundo presença de **um documento**/tipos exigentes em `app/ponto/migrations/0057_a4_backfill_pendente.py:30`, `app/ponto/migrations/0057_a4_backfill_pendente.py:35`, com trilha em `app/ponto/migrations/0057_a4_backfill_pendente.py:44`. Aplicação em prod dessas migrations = **[nao verificado]**. |

Não inferir do código quantas trocas foram executadas, quem as cadastrou ou anexos existentes: **[nao verificado]**; os 16/12/2/2 e todos SOLO/abona são somente o anexo raspado do usuário **[nao verificado por arquivo da cópia]**.

Q6 — cartas e dois anexos

| Item | Evidência |
|---|---|
| Onde chega o arquivo | `Ausencia.documento` é **um** `FileField`, `upload_to="ausencias/documentos/"`, opcional: `app/ponto/models.py:777`. `documento_referencia` aponta para primeira Ausencia de intervalo, não é segundo anexo: `app/ponto/models.py:791`. |
| Portas de envio inicial | API `request.FILES.get('documento')` em `app/api/views.py:1775`, repassado em `app/api/views.py:1822`; admin em `app/ponto/views.py:2393`, `app/ponto/views.py:2418`; PWA em `app/ponto/views.py:2480`, `app/ponto/views.py:2538`. Inputs únicos em `app/templates/ponto/lancar_ausencia.html:85`, `app/templates/ponto/submeter_ausencia.html:52`. |
| Envio posterior | API própria verifica colaborador e estado aberto em `app/api/views.py:3092`, `app/api/views.py:3102`, recebe um arquivo em `app/api/views.py:3106`, chama porta em `app/api/views.py:3130`. Admin aceita anexo/substituição em qualquer status em `app/ponto/views.py:2133`, chama mesma porta em `app/ponto/views.py:2150`. |
| Escritor do anexo posterior | `anexar_documento` **substitui** `ausencia.documento`, salva esse campo e registra antes/depois/ator em `app/ponto/services/ausencia.py:840`, `app/ponto/services/ausencia.py:854`. Se aguardava documento, muda para aguardando decisão em `app/ponto/services/ausencia.py:857`. |
| Leitor para download | Resolve `documento_referencia or a`, abre o arquivo da raiz e devolve `FileResponse` em `app/ponto/views.py:2115`, `app/ponto/views.py:2122`, `app/ponto/views.py:2127`. |
| Exigência existente | Cadastro tem booleano `exige_documento` em `app/ponto/models.py:583`; `estado_inicial(tipo, tem_documento)` decide por booleano e não cardinalidade em `app/ponto/services/ausencia.py:763`, `app/ponto/services/ausencia.py:775`. |
| Exige 2 documentos? | As portas verificadas recebem um arquivo, o modelo possui um campo e o gate só testa presença. Não há validação de “carta do cedente + carta do substituto” nesses mecanismos: `app/ponto/models.py:777`, `app/ponto/services/ausencia.py:775`, `app/ponto/services/ausencia.py:840`. Outra implementação de exigência de 2 anexos: **[nao verificado]**. |
| Aprovação sem documento | Gate de UI permite confirmação explícita sem lastro em `app/ponto/services/ausencia.py:612`; máquina não permite ir diretamente de aguardando documento para aprovada: `app/ponto/services/ausencia.py:752`. Admin pode criar já aprovada em `app/ponto/views.py:2420`. Não confundir esses caminhos com garantia obrigatória de dois documentos. |

Q7 — conexos relevantes

- **Falta já aprovada/minutos persistidos:** `Ausencia.save` só deriva minutos se tipo falta, aprovada e minutos vazio; soma `minutos_previstos_do_dia` em `app/ponto/models.py:877`, `app/ponto/models.py:885`. Trocar célula posteriormente não rederiva automaticamente um valor positivo nesse método; export de faltas continua consultando Ausencias cujo catálogo diz desconta em `app/folha/export.py:179`. Isso é fonte separada a conferir na conversão do par.
- **Justificativa apaga cobrança visual sem alterar tipo de dia:** grade consulta todas aprovadas em `app/escala/utils.py:1218`; contrato comenta limpeza de furo sem mudar tipo em `app/escala/utils.py:1213`. Post-save rejulga o dia em `app/ponto/signals.py:161`.
- **Relavra por fato novo:** cartório observa criado/atualizado/aprovado de Ausencia e Justificativa e `CelulaDia.editada_em`/`regenerada_em` em `app/ponto/services/cartorio.py:355`, `app/ponto/services/cartorio.py:359`, `app/ponto/services/cartorio.py:365`.
- **Status de Ausencia tem outro efeito além da folha:** folha só aprovada; cobrança aceita qualquer não rejeitada em `app/ponto/turnos.py:2514`. Converter tipo para “pedido” mexe também nesse consumidor, se continuar modelado como Ausencia.
- **Cache do cadastro:** runtime tem TTL de 60s em `app/ponto/catalogo/ausencias.py:258`, fonte viva/fallback em `app/ponto/catalogo/ausencias.py:278`; save/delete de TipoAusencia invalida cache do processo em `app/ponto/models.py:1235`. Não atribuir automaticamente efeito novo a todo histórico sem conferir leitores que consultam o cadastro atual.
- **Escritores paralelos do status de Justificativa:** individual em `app/ponto/services/justificativa.py:190`; lote em `app/ponto/views.py:1796`; retroativa em `app/ponto/services/justificativa.py:368`; rejeição individual em `app/ponto/services/justificativa.py:268`; rejeição lote em `app/ponto/views.py:1875`; admin editável em `app/ponto/admin.py:19`. Regulação retorno também pode pôr pendente a linha encontrada, sem trocar seu tipo, em `app/ponto/services/regularizacao.py:102`.
- **OrdemSubstituicao/AgendaDia são entidades existentes:** escritores declarados em `app/core/portas.py:475`, `app/core/portas.py:478`; cobertura cria ordem em `app/ponto/services/cobertura.py:100`; agenda cria dia em `app/escala/services/agenda.py:17`. Não examinei integralmente seu efeito em folha: **[nao verificado]**.

## 06/11 09:16 · sessao 1101f91bfa73 · - · - · origem=Ronald (tmux)

Q1 — A premissa de “escritor humano existente de `origem='editada'`” não corresponde à cópia: os campos existem, mas a catraca declara **sem escritor de produção** `editada_em` e `editada_por`; o teste varre produção e exige que continuem sem escritor até a obra própria. Evidências: `app/escala/models.py:1690`, `app/escala/models.py:1732`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:20`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:72`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:179`. O serviço de fase também declara literalmente “escritor de producao: nenhum declarado hoje”: `app/escala/services/fase_12x36.py:161`.

| Escritor vivo, distinto de edição humana da previsão | O que grava / trilha / atomic / chamada |
|---|---|
| `gerar_celulas_periodo` | Cria `colaborador,data,trabalha,origem='gerada',escala_geradora,dna`; atualiza previsão gerada e pula editada; não possui `atomic` próprio no corpo. `app/ponto/portas/celula.py:595`, `app/ponto/portas/celula.py:633`, `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:650`. Command reexporta: `app/ponto/management/commands/gerar_celulas.py:13`. |
| `regenerar_celulas_vinculo` | Mesmo nascimento `gerada`; reescrita só gerada; guarda `dna_anterior,regenerada_em,regeneracoes`; `atomic` em :212. `app/ponto/portas/celula.py:212`, `app/ponto/portas/celula.py:253`, `app/ponto/portas/celula.py:257`, `app/ponto/portas/celula.py:265`. |
| `veredito_celula` / `CelulaDia.lavrar_veredito` | Ato humano existente do Resolver, mas **veredito**, não edição de `trabalha/dna/origem`. Serviço tem `atomic` :204; chama a lavra :330; modelo delega à porta. `app/ponto/services/veredito_celula.py:191`, `app/ponto/services/veredito_celula.py:204`, `app/ponto/services/veredito_celula.py:330`, `app/escala/models.py:1819`. A porta grava `veredito,veredito_via,veredito_em,protestos`, ata/status quando aplicável; trilha `ator,antes,depois` em evento. `app/ponto/portas/celula.py:764`, `app/ponto/portas/celula.py:811`, `app/ponto/portas/celula.py:815`, `app/ponto/portas/celula.py:819`. Save+outbox em atomic próprio :681; serviço também loga ações/erros com autor. `app/ponto/portas/celula.py:676`, `app/ponto/services/veredito_celula.py:334`. View: `app/ponto/views.py:3312`. |

**Duas células no mesmo atomic:** a identidade é apenas `(colaborador,data)`; não há unicidade que impeça dois colaboradores e datas distintas. A autoridade de escrita é o **módulo** `ponto/portas/celula.py`, não “uma célula por transação”: `app/escala/models.py:1833`, `app/core/portas.py:107`. Entretanto, **não há API humana de edição existente para invocar duas vezes**; chamar `veredito_celula` duas vezes não escreve os overrides pretendidos, pois as opções são os três vereditos e o serviço somente os lavra. `app/ponto/services/veredito_celula.py:18`, `app/ponto/services/veredito_celula.py:201`, `app/ponto/services/veredito_celula.py:330`. O gate exige que toda escrita permaneça numa operação declarada da porta; hoje são gerar/lavrar/carimbar. `app/ponto/portas/celula.py:66`, `app/ponto/tests/test_chokepoint_celula_gate.py:62`, `app/ponto/tests/test_chokepoint_celula_gate.py:69`.

**Admin Django:** `app/escala/admin.py:4` importa e registra só TipoEscala e EC; EC é `SoLeitura` em :13. `app/ponto/admin.py:2`–:48 registra Batida/Justificativa/Importacao/Alerta/Ausencia. Nenhum registro de CelulaDia encontrado na busca em admin/forms/serializers; existência de algum mecanismo dinâmico externo à cópia é [nao verificado].

**Escritores de fixtures/testes `editada`**, separados de runtime: [nao verificado]

- Atribuição+save: `app/escala/tests/test_celula_dia.py:73`, `app/escala/tests/test_celula_dia.py:98`, `app/escala/tests/test_celula_dia.py:358`; `app/colaboradores/tests/test_vinculo_regenera_celulas.py:39`; `app/ponto/tests/test_porta_celula_dia.py:247`.
- `create/update_or_create/update`: `app/escala/tests/test_selo_foto_folgas_relavra.py:104`, `app/escala/tests/test_sem_dia_pelo_juiz.py:46`, `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:97`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:134`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:180`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:225`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:242`.
- Helpers recebem origem editada e fazem create: `app/escala/tests/test_selo_celula_alem_do_vinculo.py:58` chamado em :103; `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:116` chamado em :128.
- Migration apenas cria schema/choices, sem escritor de dado editado: `app/escala/migrations/0030_celuladia.py:23`, :25, :27.

Q2 — Leitores e sobrevivência:

| Leitor | Comportamento |
|---|---|
| EC.eh_dia_trabalho | Consulta célula sem filtrar origem e retorna `cel.trabalha`; só sem célula cai na aritmética do vínculo. `app/escala/models.py:1087`, `app/escala/models.py:1094`. |
| EC.marcos_do_dia | Lê `dna.marcos` da célula, sem filtrar origem; se `marcos` vazio/nulo cai no template vivo, **inclusive célula de folga**. `app/escala/models.py:1117`, `app/escala/models.py:1124`, `app/escala/models.py:1133`. |
| EC.pausas_do_dia / intervalo_do_dia | Leem DNA; diferente do anterior, célula existente sem marcos devolve nenhuma pausa. `app/escala/models.py:1163`, `app/escala/models.py:1182`, `app/escala/models.py:1202`. |
| gerar_celulas_periodo | Pula origem editada antes de qualquer atualização. **Sobrevive.** `app/ponto/portas/celula.py:638`. |
| regenerar_celulas_vinculo | Atualiza existentes somente `origem=='gerada'`, inclusive ao desligar previsão fora da vigência. **Sobrevive.** `app/ponto/portas/celula.py:232`, `app/ponto/portas/celula.py:257`. |
| reverter_regeneracao_dia | Recusa célula não gerada. `app/ponto/portas/celula.py:558`. |
| Fechamento | Carrega todas células do envelope e alimenta motor; previsão passa por EC.eh_dia_trabalho; identity map cobre marcos do motor. `app/ponto/services/fechamento.py:595`, `app/ponto/services/fechamento.py:722`, `app/ponto/services/fechamento.py:741`, `app/ponto/services/fechamento.py:269`. |
| Export | Grade delega a grade_da_celula; esta lê célula+ata, com preferência ao `ata.tipo_dia` e `ata.minutos_previstos` já lavrados. `app/folha/export.py:335`, `app/escala/services/leitor_celula.py:350`, `app/escala/services/leitor_celula.py:380`. Retenção lê `veredito/protestos`, sem filtrar origem. `app/folha/export.py:1206`, `app/folha/export.py:1233`, `app/folha/export.py:1245`. |

**Portanto sobreviver ao gerador não equivale a re-lavrar automaticamente ata/dinheiro.** Export prefere ata e a impressão hoje não vê trabalha; evidências abaixo. `app/escala/services/leitor_celula.py:380`, `app/ponto/services/cartorio.py:443`, `app/ponto/services/cartorio.py:1358`.

Q7 — Conexos importantes:

| Campo/família | Escritor/leitor e consequência |
|---|---|
| `trabalha` fora da impressão | Dívida declarada no catálogo: `app/escala/tests/test_d3_guarda_da_linha_celula.py:43`, :68-72. Hash aceita DNA, não trabalha, e codifica somente `dna_versao` + `marcos`: `app/ponto/services/cartorio.py:443`, :456. Chamada passa `cel.dna`; impressão igual mantém `_em_dia` e pode pular julgamento: `app/ponto/services/cartorio.py:1348`, :1358. Editar apenas trabalha não garante despertar. |
| `editada_em` como freshness | Leitura em `fato_do_dia_mudou`: `app/ponto/services/cartorio.py:365`; lote de freshness das perguntas inclui `editada_em` junto de outros quatro relógios: `app/chamados/veredito_lavrado.py:151`; reabertura das janelas de fase 12x36 agrega Max editada_em e compara lavra: `app/escala/services/fase_12x36.py:188`, :192. Sem escritor humano atual, relógio é preparação já existente, não prova de edição em produção. `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`. |
| outbox da célula | Só veredito/ata acordam `salvar_e_emitir`; `trabalha/dna/origem/editada_*` não estão em CAMPOS_QUE_ACORDAM. `app/ponto/portas/celula.py:663`, :680. |
| ata/veredito/status | Campos de saída lavrados pela mesma porta e usados por export/status; edição de previsão não deve ser confundida com decisão admin. `app/escala/models.py:1749`, :1774, :1810; `app/ponto/portas/celula.py:815`. |
| guardas de folha | Regeneração por vínculo verifica competências lavradas salvo motivo `apesar_da_lavra`; guarda nasce antes do atomic. `app/ponto/portas/celula.py:202`, :244. Não há como assegurar guarda equivalente para edição humana ausente [nao verificado]. |
| vigias | Stale só lê `origem='gerada'`: `app/ponto/management/commands/tripwire_celulas.py:21`. Contra template só geradas: `app/escala/management/commands/celulas_contra_o_template.py:29`. Já folga com marcos conta qualquer origem: `app/escala/management/commands/celulas_contra_o_template.py:43`. |
| DNA completo | Vocabulário de marcos/segundo intervalo é da entidade: `app/escala/models.py:1652`, :1659; `montar_lampadas` encadeia meia-noite e duas pausas: `app/ponto/management/commands/gerar_celulas.py:36`, :55, :60. Um override com marcos incompletos pode deixar consumidores lendo template/sem pausa conforme os contratos distintos acima. |

Nenhum arquivo foi alterado; testes e banco não foram executados. As conclusões são leitura da cópia e dos selos existentes, não resultado de execução [nao verificado]. [nao verificado]

## 06/11 09:16 · sessao 65f096009142 · - · - · origem=Ronald (tmux)

Q3 — conclusões condicionais, pelo código desta cópia. Não há batidas, pares concretos ou valores de folha no dado fornecido; valores financeiros do par são [nao verificado].

| Rubrica | Dia CEDIDO (`trabalha=False`, sem trabalho) | Dia ASSUMIDO (`trabalha=True`, com marcos e trabalho real) | Fonte e decisão |
|---|---|---|---|
| Adicional noturno | Sem período real, não nasce AN; a edição não fabrica trabalho. | Período noturno continua pagando AN normal; mudar de folga para trabalho não é requisito para ter AN, pois a soma já inclui `periodos + periodos_ft`. | `app/ponto/motor_calculo_v2.py:321`; cálculo efetivo por par/intervalos em `:983-987`, escrita única `:994-1005`; entrada dos marcos do vínculo em `:2290-2291` e janela em `:1532-1540`. Fechamento grava `resultado.horas_noturnas` em `app/ponto/services/fechamento.py:909`. |
| Intrajornada | Sem período real, não nasce indenização. | Continua apurando intervalo real versus mínimo; edição não garante valor positivo: depende da duração/pausa. | 12x36: `app/ponto/motor_calculo_v2.py:2766-2784`; comercial: `:3226-3242`; mínimo lê previsto do dia e cadastro/perfil em `:1824-1831`; cadastro inicial vem do template/perfil em `:831-864`. Previsto lê célula/marcos: `app/escala/utils.py:2287-2290`. Fechamento `app/ponto/services/fechamento.py:988`. |
| HE100 por folga/DSR trabalhado | Sem trabalho, não há período para pagar. | `trabalha=True` retira a classificação de folga: vai a `periodos`, não `periodos_ft`; assim deixa de alimentar `horas_folga_trabalhada`. | Juízo lê `EC.eh_dia_trabalho`: `app/ponto/motor_calculo_v2.py:2447-2448`; partição em `:2468-2494`. Fechamento paga folga somente dos `periodos_ft` com escala certa: `app/ponto/services/fechamento.py:924-950`. Template semanal só é fallback sem vínculo que responda: motor `:2449-2453`; previsão injetada prevalece no domingo em `:950-954`. |
| DSR: semanas e reflexo | Dia sai dos obrigatórios porque o cursor do fechamento só inclui `eh_dia_trabalho=True`; logo a ausência nesse dia não vira falta injustificada para DSR. | Entra no previsto e, havendo período normal, entra nas datas trabalhadas; semana e reflexo dependem dos demais dias e extras. | Previsto vindo da célula: `app/ponto/services/fechamento.py:720-731`; `EC` devolve `cel.trabalha` em `app/escala/models.py:1094-1096`. DSR base lê `datas_previstas_trabalho`, períodos e justificativas: `app/ponto/motor_calculo_v2.py:2576-2611`; reflexo `:2636-2653`. Comercial refaz semanas/reflexo em `:3441-3479`. Fechamento grava `app/ponto/services/fechamento.py:1085-1087`. |
| Furo/falta apurada | Dia fora dos obrigatórios deixa de ser `resultado.datas_falta`; isso não é desconto decidido. | Com período normal não é furo; sem trabalho e sem abono, sendo previsto, pode virar furo. | `app/ponto/motor_calculo_v2.py:2595-2619`; teto temporal antes da lavra em `app/ponto/services/fechamento.py:1099-1111`; `DiaPago.furo_apurado` em `app/ponto/services/dia_pago.py:343-345`. O campo não é rubrica/dinheiro: `:150-158`. |
| Falta/desconto já decidido | **Célula sozinha não apaga uma Ausencia DESCONTA aprovada preexistente.** | A edição tampouco cancela esse registro. | `horas_falta` é `minutos_de_falta_por_dia` em `app/ponto/services/fechamento.py:864-866`, gravado em `:987`; essa autoridade lê Ausencia aprovada e catálogo, não célula: `app/folha/export.py:87-92`, `:138-143`. TXT usa `datas_falta` em `:732-736`, cuja fonte é o catálogo DESCONTA em `:179-181`. |
| Atraso/antecipada | Previsto zero zera teto da pontualidade; nenhum período normal também impede cobrar nesse dia. | **A cópia já julga comercial e 12x36 com escala pelos marcos da célula**, portanto atrasos reais em relação ao novo marco podem ser cobrados conforme as regras vigentes. | Teto zero para previsto zero: `app/ponto/motor_calculo_v2.py:1040-1046`; só percorre `resultado.periodos`: `:1078-1084`. Leitor dos marcos do vínculo `:2048-2055`; comercial chama `:3293`; 12x36 com escala chama `:3533`. |

**Limite da conclusão “sem HE100”:** a célula `True` basta para retirar **HE100 por folga**, mas não suprime HE100 de feriado ou de excedente normal acima de duas horas: feriado é outra fonte (`feriado_em_dobra` + calendário), em `app/ponto/motor_calculo_v2.py:934`; excedente 12x36 em `:2847-2854`, comercial em `:3392-3398`. Portanto não afirmar “HE100 global sempre zero” para todo turno só por editar a célula. Esses caminhos existem também no calculador, em `app/ponto/calculador/regras.py:779-815`.

**Nenhuma das rubricas-alvo exige necessariamente modificar `motor_calculo_v2.py` para consumir o override do par:** o ponto exato já lê o vínculo/célula em `app/ponto/motor_calculo_v2.py:2448` e separa folga/trabalho em `:2468-2494`; os marcos vêm do vínculo em `:2290-2291`. AN/intra já usam os períodos reais com esses marcos/janelas (`:2759-2792`, `:3211-3247`). Remover uma falta DESCONTA já decidida exige tratar a decisão em Ausencia, cuja leitura financeira não passa pela célula (`app/folha/export.py:138-143`, `:179-181`), e não é motivo demonstrado para alterar motor. Prova executada do par concreto: [nao verificado].

**S5b do anexo versus estado da cópia:** a cópia registra obra posterior O271 que ligou pontualidade comercial, e o código efetivamente a chama; a regra “calculador não cobra o que o sistema não cobra” permanece, porém comercial saiu dos isentos (`app/ponto/calculador/regras.py:455-472`, `:491-502`; `app/ponto/motor_calculo_v2.py:3293`). Não usar o número histórico do anexo para afirmar que o comercial atual continua `NAO_DECIDE`.

**DSR dinheiro no TXT:** o export não emite evento DSR; documenta que o Domínio calcula reflexo a partir das HE e AN, e a lista devolvida contém essas rubricas sem DSR (`app/folha/export.py:439-460`). Efeito efetivamente pago pelo Domínio no caso: [nao verificado].

**Motor, calculador e lavratura — não confundir as versões:**

- Fechamento mensal recebe AN/intra/HE do motor (`app/ponto/services/fechamento.py:909-915`, `:988`) e DSR do motor (`:1085-1087`), sendo salvo em `:1127-1128`.
- `DiaPago versao='motor'` recebe os mesmos períodos, partições de folga, faltas decididas e reflexo em `app/ponto/services/fechamento.py:1185-1211`; a distribuição soma AN/intra/HE dos períodos em `app/ponto/services/dia_pago.py:173-186`, falta em `:272-275`, DSR numa linha de ajuste em `:346-356`.
- A partir de 10/2026 há uma segunda **versão** `oraculo`, pela mesma função `lavrar`, com valores vindos do calculador (`app/ponto/services/fechamento.py:1249-1278`). Só nessa chamada `rubricas=` sobrepõe campos decididos em `app/ponto/services/dia_pago.py:310-328`; não reescreve o FechamentoMensal nem a versão `motor` (delete é filtrado por `versao`, `:357-358`).
- Calculador também consulta `EC.eh_dia_trabalho` (`app/ponto/calculador/alimentacao.py:434-445`), recebe previsto/célula, intervalo cadastrado, calendário e escala certa em `:514-532`; folga sai pelo ramo `app/ponto/calculador/regras.py:650-704`; AN normal por `:844-849`. Os leitores de export filtram `versao='motor'` em `app/folha/porta_export.py:284`, `:589`, `:651`.

**Q7 conexões adicionais vistas:**

- `datas_justificadas` contém todas as Justificativas aprovadas com `data_turno`, sem filtro de motivo, e também Ausencias cujo efeito é ABONA; SUPRIME remove o dia do previsto (`app/ponto/services/fechamento.py:651-655`, `:684-692`). Manter um pedido `troca_de_plantao` como abono aprovado pode continuar interferindo por esse caminho enquanto não se mudar a semântica do pedido (`:688-690`).
- Ausencia aprovada que explica dia inteiro zera atraso/antecipada na última camada do teto, sem apagar trabalho; essa alimentação vem do fechamento (`app/ponto/services/fechamento.py:682-683`, `:742`; `app/ponto/motor_calculo_v2.py:1085-1105`).
- A edição de marcos não altera a classe do motor por si: `get_motor` escolhe por `tipo_base`/ciclo/geometria do TipoEscala (`app/ponto/motor_calculo_v2.py:3623-3668`), e a política de feriado pelo ciclo em `:3669-3670`.
- Marcação noturna pós-5h/hora reduzida é política de ciclo/CCT, não campo `trabalha` (`app/ponto/motor_calculo_v2.py:758-769`); o mínimo intra também lê cadastro/perfil (`:831-864`, `:1824-1831`). O override é soberano para previsto/marcos, não substitui todas as fontes de dinheiro.

## 06/11 09:15 · sessao 3a25d6a67f22 · - · - · origem=Ronald (tmux)

Referências para os campos das entradas:

| Model | Campos e definições |
|---|---|
| `TipoEscala` | `tipo_ciclo` app/escala/models.py:117; valores `12x36` app/escala/models.py:26 e `6x1` app/escala/models.py:30; `hora_inicio` app/escala/models.py:50; `hora_fim` app/escala/models.py:51; `hora_inicio_intervalo` app/escala/models.py:52; `hora_fim_intervalo` app/escala/models.py:53 |
| `EscalaColaborador` | `colaborador` app/escala/models.py:872; `posto` app/escala/models.py:873; `tipo_escala` app/escala/models.py:874; `data_inicio` app/escala/models.py:875; `data_fim` app/escala/models.py:876; `ativa` app/escala/models.py:877 |
| `CelulaDia` | `colaborador` app/escala/models.py:1681; `data` app/escala/models.py:1685; `trabalha` app/escala/models.py:1686 (False significa folga em app/escala/models.py:1688); `origem` app/escala/models.py:1691; `escala_geradora` app/escala/models.py:1692; `dna` app/escala/models.py:1697; `editada_em` app/escala/models.py:1732; `editada_por` app/escala/models.py:1733; unicidade colaborador/data app/escala/models.py:1833 |
| `TipoAusencia` | `codigo` app/ponto/models.py:577; `efeito` app/ponto/models.py:581; `exige_documento` app/ponto/models.py:583 |
| `Ausencia` | `colaborador` app/ponto/models.py:743; `tipo` app/ponto/models.py:749; `data` app/ponto/models.py:750; `data_fim` app/ponto/models.py:751; `minutos` app/ponto/models.py:755; `hora_ini` app/ponto/models.py:768; `hora_fim` app/ponto/models.py:770; `hora_extra_100` app/ponto/models.py:772; `documento` app/ponto/models.py:777; `documento_referencia` app/ponto/models.py:791; `status` app/ponto/models.py:799; `registrado_por` app/ponto/models.py:801; `aprovado_por` app/ponto/models.py:805; `aprovada_em` app/ponto/models.py:810 |
| `Batida` | `colaborador` app/ponto/models.py:22; `timestamp` app/ponto/models.py:23; `tipo` app/ponto/models.py:24; `is_intervalo` app/ponto/models.py:26; `escala` app/ponto/models.py:29 |
| `LogAuditoria` | `usuario` app/core/models.py:295; `acao` app/core/models.py:296; `modelo` app/core/models.py:297; `objeto_id` app/core/models.py:298; `descricao` app/core/models.py:299; `valor_antes` app/core/models.py:300; `valor_depois` app/core/models.py:301 |
| `ExportacaoDominio` | `empresa` app/folha/models.py:77; `mes` app/folha/models.py:78; `ano` app/folha/models.py:79; `gerado_em` app/folha/models.py:80; `conteudo` app/folha/models.py:86; `invalidada_em` app/folha/models.py:96 |
| `PeriodoFechado` | `empresa` app/ponto/models.py:916; `mes` app/ponto/models.py:920; `ano` app/ponto/models.py:921; `ativo` app/ponto/models.py:933 |

Regras pertinentes ao vínculo:

- Autoridade do vínculo do dia é `CelulaDia.escala_geradora`, não o vínculo ativo ou mais recente: app/escala/alimentacao.py:231.
- Corte E1 literal: geradora só vale dentro de `data_inicio..data_fim`; fora, retorna `None` e o dia é de cadastro; vínculo hoje inativo continua valendo se cobria o dia: app/escala/alimentacao.py:272.
- Sem geradora, o vínculo desativado e sem `data_fim` não cobre dia algum: app/escala/alimentacao.py:209.
- Célula editada é imutável **pelo gerador**, sem regra nesse trecho sobre sobrescrita por outro ato humano: app/escala/models.py:1640.

Resolver dia:

- Função `resolver_dia_oferecido(dia, hoje)` app/colaboradores/services/calendario.py:97.
- Entrada `dia` é um item de `contexto_calendario()['dias']`, conforme docstring app/colaboradores/services/calendario.py:98.
- Predicado documentado: último recurso + passado + status que pede decisão; permissão não integra o predicado: app/colaboradores/services/calendario.py:100.
- Campos do dict usados: `pode_ultimo_recurso`, `data`, `status` app/colaboradores/services/calendario.py:103. Status previstos `falta`, `irregular`, `aberto`, `sem_certidao`: app/colaboradores/services/calendario.py:94.

Campos específicos de par/cedente/substituto/identidade da troca/segundo anexo/estado `confirmada`: [nao verificado]. O modelo existente documentado possui `Ausencia.documento` e `documento_referencia`, com a última referência destinada ao primeiro registro de um intervalo, não a um par de troca: app/ponto/models.py:777 e app/ponto/models.py:791.

A catraca atual declara `editada_em` e `editada_por` sem escritor de produção: app/escala/tests/test_d3_guarda_da_linha_celula.py:50. Isso é evidência da cópia, não regra para esperado do O10.

_2172 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
