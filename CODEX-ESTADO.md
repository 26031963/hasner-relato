# CODEX-ESTADO

> Ultima resposta processada: **20/11/2026 02:02** · publicado em **20/11/2026 02:06** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 20/11 02:02 · sessao 8485b295327f · CADEIA-20-PASSO-1 · leitura · origem=Code

aval Ronald: CADEIA-20-IMPRESSAO-V3 (rascunho Codex 20/11) -- unidade: célula-dia, impressão da ata; lei: L-124, desenho célula soberana (`app/docs/LEIS.md:145`, `app/docs/GLOSSARIO.md:11`)
PAREI — (i)+(ii) não demonstram 9/9. A 129134 ainda muda se P3 atravessar o fim do turno. As 128355, 129943 e 138359 perdem o último fio vivo e podem sobrar em P2 se a mudança cair fora das passadas internas. A etapa escritora dessas três e o resultado a relógio fixo [nao verificado]. `app/ponto/services/cartorio.py:1152–1164,1179–1181,1200–1231,1540–1558`

## RESPOSTAS Q1–Q4

**Q1 — Granularidade.** Para escolher códigos, `classificar_dia` lê zero/não zero de `chamados_vivos`, `chamados_total` e `cobrancas_vivas`. Usa a contagem exata de `chamados_vivos` nos textos de três diagnósticos. O cartório descarta esses textos. O comando `supra_juiz` os publica e usa o veredito já lavrado para decidir a retratação. `emitir_furo_retroativo` consome os códigos. `app/ponto/supra_juiz.py:326–329,373–380,467–484,507–512,543–549` `app/ponto/services/cartorio.py:1614–1627` `app/ponto/management/commands/supra_juiz.py:211–245` `app/ponto/management/commands/emitir_furo_retroativo.py:94–112`
- No cartório, `chamados_total` é presença de qualquer chamado de batida no dia, inclusive morto. `chamados_vivos` é presença no subconjunto que protesta (`VIVOS`, dia de trabalho e lastro que não absolve). `cobrancas_vivas` é presença de um vivo com fio C6. A escolha direta de `cobrado` usa ainda a **interseção** fio vivo × protesta quando não há códigos. Dois dias com as mesmas contagens podem divergir nessa interseção. A emissão lê também a presença de qualquer vivo. O ramo `nunca_bateu` passa os três zeros. `app/ponto/services/cartorio.py:141–157,1399–1415,1527–1545,1605–1653,1889–1897` `app/ponto/services/lastro.py:153–181`
- `status_local` entra literal no hash atual como `pk:status_local`. Para o veredito, sua leitura direta é pertinência a `VIVOS`. C6 lê também a pertinência do destino a `ENCERRADOS` no fallback sem slot. Os gates de emissão e fecho ainda operam sobre os chamados individuais. `aberto` e `em_analise` pertencem aos mesmos conjuntos nesses leitores da palavra. `app/chamados/catalogo/motor.py:24–32,98–113` `app/chamados/juizes.py:769–789,1482–1491` `app/ponto/services/cartorio.py:502–507,1527–1532,1691–1738,1804–1835`
- As referências da ficha ao main correspondem, nesta cópia, a `app/ponto/services/cartorio.py:530–547` para fio no hash, `app/ponto/services/cartorio.py:1200–1209,1220–1223` para o gatilho da releitura e `app/chamados/signals.py:189–218` para o sinal. A fila citada na ficha como `1045–1049` é consumida aqui em `app/ponto/services/cartorio.py:1147–1164`.

**Q2 — Escritores e cobertura.**
- **Enfileiram.** Fechos por marco aceso, inexistente ou cobrança indevida, dedup e ressuscitação bem-sucedida escrevem depois de calcular a impressão e antes de gravar o carimbo físico. Depois do primeiro carimbo, o fechamento adiado enfileira a célula se `reconciliar()` fechar. A segunda passada força só as enfileiradas, com a janela original. O que for enfileirado nessa segunda passada ou na releitura vai ao contador de descarte. `app/ponto/services/cartorio.py:1140–1164,1229–1231,1541–1547,1691–1738,1812–1861,2035–2037`
- **Releitura condicionada.** Abertura ou reabertura pelo emissor que sobe `emitidos` ativa a releitura após a fábrica. `_espelhar_marcos` escreve o contexto que o lastro lê, sem fila própria. Se mover a granularidade, só a releitura acionada por emissão pode cobri-lo hoje. O carimbo também muda a ata e o veredito que C6 lê. `app/ponto/services/cartorio.py:1193–1223,1952–1955,1990–2037,2136–2146` `app/ponto/management/commands/detectar_ausencias.py:544–569` `app/ponto/services/lastro.py:153–181` `app/chamados/juizes.py:1418–1420,1450–1469`
- **Nenhuma cobertura garantida.** E2 carimba perguntas do colaborador e pode fechar disputa e chamado-pai. Só o fecho bem-sucedido da lista adiada enfileira a célula. A fábrica, depois de consumida a fila, pode descarimbar, criar perguntas, chamar `analisar()` e fechar disputa com cascata. Essas escritas não sobem `emitidos` por si. O cluster abre ou fecha chamado de `cluster_batida_espuria`, fora dos quatro módulos da lista do cartório. Efeito indireto do cluster nas nove células [nao verificado]. `app/ponto/services/cartorio.py:1126–1184,1209–1223` `app/chamados/reconciliador.py:634–665` `app/chamados/services/fabrica_da_celula.py:160–174,206–219` `app/chamados/services/disputa_emissao.py:1180–1192,1255–1302` `app/chamados/signals.py:19–46,149–165` `app/ponto/services/cluster_espurio.py:11,176–216` `app/chamados/catalogo/furo_modulos.py:15–16`
- **Depois do cartório, nenhuma das duas passadas internas.** `apurar_furos_diarios` agrupa e reconcilia filhos de adesão e limbo, inclusive reapontando perguntas no limbo. `supra_juiz --executar` pode reconciliar e resolver perguntas. `flip_automatico --apply` grava tipo de batida, aciona o sinal da batida e recompõe turnos. O consumidor contínuo de `EventoCelula`, que roda em paralelo à corrente, carimba, reconcilia e roda a fábrica. Para fechos com mudança de estado vale Q4. `app/config/crons.py:394,599,707,1527–1545` `app/escala/management/commands/apurar_furos_diarios.py:13–21` `app/chamados/services/adesao.py:103–120,143–154` `app/chamados/services/limbo_folgas.py:267–289` `app/ponto/management/commands/supra_juiz.py:257–292` `app/chamados/reconciliador.py:572–590,1414–1438` `app/ponto/management/commands/flip_automatico.py:51–79` `app/ponto/services/flip_batida.py:28–39` `app/ponto/registro_batida.py:255–274` `app/ponto/signals.py:85–93,131–135`
- `disparar_perguntas_competencia`, `reconciliar_perguntas_orfas`, `fechar_becos_disputa`, `detectar_intervalo_ausente` e `silenciar_chamados_isentos`, com os argumentos agendados, são vigias desses insumos. Para `reeavaliar_ausencias_feriado`, conferi o agendamento `--alarme`; corpo [nao verificado]. Os elos seguintes de lavra e alerta estão declarados no cron; censo transitivo de seus efeitos sobre fio [nao verificado]. `app/config/crons.py:491,608–609,661–663,1539–1811` `app/chamados/management/commands/disparar_perguntas_competencia.py:130–145` `app/ponto/management/commands/reconciliar_perguntas_orfas.py:22–34` `app/chamados/management/commands/fechar_becos_disputa.py:23–30` `app/ponto/management/commands/detectar_intervalo_ausente.py:98–108,138–139` `app/chamados/management/commands/silenciar_chamados_isentos.py:23–44`

**Q3 — Nove células, baseline medida e inferência sobre (i)+(ii).**
- **128355, 129943, 138359.** `tres.out`, registros `P2 pk=` correspondentes, mostra `em_analise → resolvido` no último vivo, com `fio:1` sumindo e ata intacta. A granularidade muda de presente para ausente em vivos e fio. (i) preserva essa diferença. (ii) a traz para P1 se o escritor e a célula estiverem cobertos no mesmo passe. Se a mudança vier depois da última leitura coberta, sobra para P2. Escritor efetivo e passe residual após a mudança [nao verificado]. `app/ponto/services/cartorio.py:1126–1164,1200–1231,1527–1545`
- **129476, 129490.** `insumo.out`, registros `P2 pk=129476` e `P2 pk=129490`, mostra `fio:1 → 2` nas duas e `aberto → em_analise` na segunda. (i) elimina esses deltas de contagem e rótulo se a presença de fio que protesta ficar igual. Quais chamados tinham fio e protestavam antes/depois, e eventual residual em P2/P3 [nao verificado]. `app/ponto/services/cartorio.py:1527–1545,1605–1653` `app/chamados/catalogo/motor.py:24–32`
- **138541, 142995, 143945.** Os IDs estão no V2 e na ficha. O corte exibido de `insumo.out` termina durante `P2 pk=131231`, antes das linhas de 138359, 138541, 142995 e 143945. `tres.out` recupera 138359. Faltam os deltas de três das nove no material combinado. Granularidade, escritor e residual destas três [nao verificado].
- **129134.** `insumo.out`, `P2 pk=129134`, mostra primeira lavra `concorde`, sem chamado e com `enc:0`. `tres.out`, `P3 pk=129134`, mostra `concorde → cobrado`, sai `enc:0` e entram chamado 43141 `em_analise` e `fio:1`. Se P3 cruza o fim do turno, a mudança continua exigida mesmo com (i)+(ii). A relógio fixo, a emissão seguida de releitura é um caminho existente no braço encerrado. Resultado nos dois braços e instante efetivo da sonda [nao verificado]. `app/ponto/turnos.py:2124–2134` `app/ponto/services/cartorio.py:523–532,1193–1223,1293–1297,1540–1545,1624,1980–1993`

**Q4 — Fora do passe.** Criação ou mudança de `status_local` por `save`, com dia do fato identificável e fora de `em_julgamento`, tenta `julgar_celula` no ato, seja porta humana ou cron. A chamada usa `forcar=False`. A porta avulsa pode recusar por data ou turno. Se a célula do ramo normal já tem carimbo, impressão igual e veredito preenchido diferente de `nunca_bateu`, o corpo pula. A palavra fica velha se a impressão igual encobrir uma mudança relevante, por exemplo fio que deixa de protestar enquanto a presença global de fio e a de chamados que protestam permanecem. Save de pergunta aciona outbox/beco, sem chamada direta ao cartório. Se não houver fecho de chamado ou outro fato que o acione, a atualização da palavra fica para o próximo julgamento que veja a impressão diferente. `app/chamados/signals.py:180–218,229–259` `app/chamados/models.py:3416–3445` `app/ponto/services/cartorio.py:1552–1558,1605–1653,2174–2205`

## FONTE

- **Base e autoridade.** CADEIA-20-PASSO-1, volta 3, caminho da cópia registrado em `app/docs/RELATO.md:100`. Hash informado `a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6`. A árvore recebida veio sem metadados Git. Identidade do HEAD [nao verificado]. Decisões e precedência dos anexos 1815/2325 e da Ficha 1 vêm dos literais fornecidos; aprovação do V2 [nao verificado na árvore].
- **Impressão e lavra.** SHA-256 de batidas (`pk`, tipo, instante), status da ausência, chamados de batida (`pk`, estado), DNA (versão e marcos) e condicionais `veto`, `teto`, `regra`, `enc`, `fio`, `cobparcial`, `isento`, `sem_adesao`. A parcial incorpora período e índices dispensados quando pertinentes. A porta da célula persiste impressão e lavra. Q1–Q4 localizam os leitores e escritores da mudança. `app/ponto/services/cartorio.py:477–578,1527–1558,2012–2037` `app/ponto/portas/celula.py:1189–1201`
- **Horizonte e relógio.** `versao_para` toma a maior versão aplicável à competência pelo corte da empresa. V1 começa em 2026-09 e v2–v5 em 2026-10. O lote captura `agora` e o repassa ao corpo. C6 já aceita esse instante. `app/docs/LEIS.md:145` `app/ponto/janelas.py:36–63` `app/ponto/services/cartorio.py:161–166,280–299,1110–1123,1333–1337,1399–1401,1541–1547` `app/ponto/management/commands/processar_cartorio.py:40–41,63–69` `app/chamados/juizes.py:866–898`
- **Entrega e dinheiro.** `_K6` pergunta por TXT vigente e não invalidado na tripla empresa × mês × ano. A grade da ata alimenta previsto, abono, realizado, retenção e `DiaPago`. DINHEIRO item 1 exige reversão anterior, exportada intacta e prova de frota no mesmo marco quando há apply admitido. `app/core/juizes.py:334–342` `app/folha/export.py:329–338,556–592` `app/ponto/services/fechamento.py:1046–1068,1181–1202` `CLAUDE.md:401–417`
- **Sondas.** `tres.out`, bloco de diffs P2/P3 até `FIM_SONDA_CELULA`, mostra zero `null → []` nesses passes. Em `P1_0808`, 12711/08 é de 21/07/2026, segue `furo` e muda `razoes_orfas` de `null` para `[]`, junto de `33432:aberto → em_analise`. Os hashes abreviados de FM e ED coincidem nesse anexo. Filtro, colunas, resposta de `_K6`, identidade das 19 e causalidade por célula [nao verificado].

## MUDA

1. **Granularidade e passadas.** (i) Desde o horizonte de MUDA 3, codificar nos trechos chamado/fio a presença de chamado total, de vivo, de vivo que protesta, de vivo com fio C6 e de vivo com fio C6 que protesta, pelas mesmas leituras do julgamento. Preservar a distinção entre ausência de vivo e vivo sem fio. A impressão não ganha assinatura de ato humano. (ii) Toda escrita da cadeia que altere essa projeção depois de lida para carimbar, inclusive após o carimbo, deve levar a célula à segunda passada existente por `out['rejulgar']` ou ficar coberta pela releitura existente. Antes do código, publicar escritor, projeção antes/depois, ordem e cobertura por célula. Se alguma escrita ficar sem cobertura nessas duas vias, PAREI nesse ponto, com a célula e a etapa identificadas. `app/ponto/services/cartorio.py:502–507,533–547,1123–1231,1527–1558,1605–1653,1889–1897,2035–2037`
2. **Relógio.** Passar o `agora` do cartório a `fio_vivo` e deste a `pergunta_viva`. Preservar a chamada existente dos outros leitores quando omitem o argumento. `app/ponto/services/cartorio.py:1110–1123,1532,2202–2205` `app/chamados/juizes.py:866–898,1382–1404,1510–1519`
3. **Vigência.** Usar `versao_para` e o horizonte default **2026-10** da volta 3. Até 2026-09, conservar byte a byte a impressão anterior para os mesmos insumos e a foto gravada sem fato novo. Em fato novo, vale o caminho anterior à mudança. Manter a leitura e a lavra da razão da sobra pela v5. `app/docs/LEIS.md:145` `app/ponto/services/cartorio.py:161–166,280–299,1038–1056,1333–1337,1552–1558` `app/escala/services/leitor_celula.py:194–222`
4. **Entregue.** Consultar `_K6` pela competência do dia antes de qualquer julgamento ou escrita desta obra, inclusive efeitos da releitura e da fábrica. Preservar a janela de leitura dos turnos que atravessam a borda. Provar zero escrita em célula entregue, além do hash de FM e ED. `app/folha/export.py:556–590` `app/ponto/janelas.py:60–63` `app/ponto/services/cartorio.py:1075–1079,1152–1181,1199–1222` `app/ponto/portas/celula.py:1189–1201`
5. **FM/ED, literal proposto.** Empresas **2, 3, 4** × competências **2026-08, 2026-09, 2026-10, 2026-11**. Agosto e novembro aparecem nas células da sonda. Setembro e outubro aparecem na declaração de versões e nos anexos 1815/2325. O filtro dos hashes antigos [nao verificado]. Fotografar imediatamente antes de P1 e após P3, no mesmo ensaio, antes de recálculo financeiro. FM por empresa/ano/mês/colab/PK e ED por empresa/ano/mês/PK, com **todas as colunas persistidas**, inclusive carimbos. Em ED incluir conteúdo, `hash_sha256`, `invalidada_em`, `invalidada_motivo` e `substituida_por_id`, com todas as versões. Ordenar pelas chaves, usar a mesma serialização canônica e publicar contagem e SHA-256 por modelo × empresa × competência, inclusive recortes vazios. `app/ponto/models.py:398–485` `app/folha/models.py:75–104,143–186`
6. **Aberta.** Antes do pouso, medir main × cópia a partir do mesmo dump e relógio, por empresa × colab × competência aberta alcançada. Usar o mesmo recálculo em ambos. Comparar presença da linha, rubricas, saldo, turnos, previsto/abonado/realizado, retenção (`motivos_espelho` e classificação de exportação) e linhas `DiaPago` por data/tipo/versão, inclusive ajuste. Movimento = `PAREI` com empresa, colab, competência, campo, antes/depois e dias afetados. Em 10, manter a Pauta DP de 2325 para 191, 203, 922 e 87 e o acerto a cargo do DP, conforme anexo 2325 [nao verificado na árvore]. `CLAUDE.md:401–417` `app/ponto/models.py:315–351,412–468` `app/ponto/services/fechamento.py:1046–1068,1191–1202` `app/folha/export.py:765,1301`
7. **(A) `null → []`.** Registrar as 19 do levantamento anterior e o zero observado no P2/P3 do dump novo. Sem varredura de normalização. Manter 12711/08 como testemunha do P1 e informar sua resposta de `_K6` no ensaio. Identidade das 19 e resposta de `_K6` para 12711 [nao verificado]. `app/ponto/services/cartorio.py:962–971` `app/docs/LEIS.md:145`

## RED

- **R1 — Oito células.** `128355`, `129476`, `129490`, `129943`, `138359`, `138541`, `142995`, `143945`, todas no recorte informado de 11/2026. Entradas observadas incluem `em_analise → resolvido` com `fio:1` sumindo (128355/129943/138359), só `fio:1 → 2` (129476) e `aberto → em_analise` com `fio:1 → 2` (129490). Detalhe das outras entradas [nao verificado]. Sem ato humano nem fato externo novo, ler `CelulaDia.impressao` e `veredito` por PK após P1, P2 e P3. Esperado **8/8 estáveis em P1=P2 e P2=P3**. As diferenças da base são as do anexo `insumo.out` [nao verificado por reexecução]. `app/ponto/services/cartorio.py:1527–1558` `app/ponto/portas/celula.py:1194–1201`
- **R2 — 129134, col474, 19/11, competência 11.** Entrada do anexo `tres.out` é `enc:0`, sem chamado, depois `fio:1` e chamado 43141 com `concorde → cobrado`. Causa exata [nao verificado]. Rodar três passes com o mesmo instante em cada braço, um em curso e outro já encerrado pelo juiz do turno. Em curso, a regra conserva `concorde` sem cobrança prematura. Encerrado, com cobrança devida e fio vivo, a primeira rodada completa deve sair `cobrado`, com uma só cobrança. Esperado **impressão e veredito iguais em P1=P2=P3** em cada braço. Atravessar o encerramento num controle separado deve permitir o rejulgamento exigido pelo juiz vigente. `app/ponto/services/cartorio.py:523–532,1193–1223,1293–1297,1540–1545,1614–1624,1975–1993` `app/ponto/turnos.py:2124–2134` `app/ponto/supra_juiz.py:317–329,450–463`
- **R3 — Ato entre passes e granularidade.** H1: célula passada, aberta e carimbada no horizonte, um único chamado de batida `em_analise`, premissa regularizada e sem pergunta viva que barre o fecho. Após P1, gestor faz POST real em `detalhe_local` com `novo_status=resolvido`. Esperado: o sinal chama o cartório com `forcar=False`, a projeção perde o último vivo, a impressão muda antes de o POST retornar e P2 já encontra o carimbo em dia. H2: na fábrica, `analisar()` muda `aberto → em_analise`, com as cinco presenças da projeção iguais. Esperado: delta de impressão zero e zero reentrada pelo sinal. H3: validação humana da única pergunta viva, com chamado que permanece vivo. Esperado: evento da pergunta, zero chamada direta do seu sinal ao cartório, e o próximo passe sem `forcar` vê fio 1→0 e atualiza a palavra conforme os códigos. H4: dois vivos, só um protesta, um único fio vivo muda do que protesta para o que não protesta, com os demais insumos e um dia sem códigos fixos. Esperado: impressão diferente e `cobrado → concorde` no julgamento. `app/chamados/urls.py:14,33` `app/chamados/services/acoes_chamado.py:292–297,322–331` `app/chamados/models.py:149–205,859–866` `app/chamados/services/disputa_emissao.py:1180–1192` `app/chamados/services/validacao.py:161–175` `app/chamados/signals.py:189–218,252–259` `app/ponto/services/cartorio.py:1552–1558,1605–1653,2174–2205` `app/escala/models.py:1624–1632`
- **R4 — Horizonte e entregue.** Fixtures `H09` e `H10` abertas e já carimbadas, com a mesma mudança exclusivamente derivada após P1. H09 mantém impressão, ata, veredito e `julgada_em` da base na ausência de fato novo. H10 usa a regra nova e passa R1. Fixture `E10` com TXT vigente e insumo suficiente para relavrar fica intacta em P1–P3 e tem **zero chamadas às portas de escrita**. Controle `I10`, mesma competência com último TXT invalidado sem substituta, entra no caminho de aberta. Para 12711, se `_K6` responder entregue, o esperado é zero toque inclusive em P1. `app/docs/LEIS.md:145` `app/folha/export.py:556–590` `app/escala/tests/test_cadeia20_volta3.py:142–158` `app/ponto/portas/celula.py:1189–1201`
- **R5 — Relógio injetado.** Fixture T1 com pergunta viva às 10:00 e validação datada às 10:01. Fixar relógio ambiente às 10:02. `fio_vivo` deve responder vivo com `agora=10:00` e morto com `agora=10:02`. As duas consultas usam a mesma pergunta. `app/chamados/juizes.py:886–898,1382–1406,1510–1519`

## SELO

- Django na pista da cópia e prova de frota na sombra. Para R1/R2, três rodadas de `processar_cartorio --apply` sem `--forcar` externo, empresas 2, 3, 4 nessa ordem em cada rodada. Usar o mesmo dump por braço, relógio local congelado em `2026-11-20T06:27:00-03:00` como literal proposto para o braço encerrado, e `2026-11-20T00:30:00-03:00` para o controle em curso de 129134. Confirmar os dois lados pelo juiz do turno. Tirar cada foto depois de E2, fechamento adiado, fábrica e releitura internos. Os instantes originais exatos das sondas [nao verificado]. `CLAUDE.md:84–

[truncado em 20000 de 27725 caracteres]

## 20/11 02:01 · sessao 00632872258d · TRAVAS-SEM-MEMORIA · leitura · origem=Code

aval Ronald: TRAVAS-SEM-MEMORIA-V4 (rascunho Codex 20/11) -- unidade: hook e ESTADO; lei: nenhuma nova

## FONTE

- F0. FICHA 2, alíneas (a)–(c), rege este rascunho sobre o V3, que não foi aprovado. Base informada `eb1e49c1ec1055e6bf43d7f67df63b17054b474e`. A cópia entregue veio por `git archive`, sem `.git`. `HEAD = main = hash`, carga dos hooks na VM, executor da pista e resultados de smoke [nao verificado]. `fichas_v3_para_rascunho_2011_0205.txt:1,24–28`, `CLAUDE.md:344–348`.
- F1. `suite.sh` resolve `--volta` e julga a seleção efetiva. Seleção vazia ou que cobre a descoberta toda é inteira. Inteira sem motivo canônico sai `rc 2`. A volta intermediária junta ruff, selo do disjuntor e recorte com vizinhos e selos globais. `bin/suite.sh:156–180,215–263`, `bin/volta_intermediaria.sh:18–46`.
- F2. Régua tem runner e carimbo próprios. O Git entrega argumentos e stdin ao wrapper do pre-push, que decide se reaproveita a régua e roda `tenants` em runner separado. O pouso usa `suite.sh` pela trava da VM. `bin/regua.sh:46–51,84–101,165–169`, `bin/hooks.sh:58–65`, `bin/pre-push.sh:6–8,27–74,159–200`, `CLAUDE.md:95–106`.
- F3. A opção A registra volta e pre-opus de lateral na pista, espinha e pouso na VM. A interface da pista recebe commit em `refs/pista/<sha>`, `pede <sha> [--only <labels>] [--parallel N]` e `espera <id>`. Há registros de `rc 0`, `rc 1` e `rc 71` sem a ref. `app/docs/RELATO.md:65,201,1497,1529`, `app/docs/BACKLOG.md:2191–2195`.
- F4. `PreToolUse` recebe o comando externo em `tool_input.command`. O disjuntor atual pode sair cedo para Bash da sessão principal. `UserPromptSubmit` está ausente do settings entregue. Stop lê `transcript_path` e cobra a fila pelas portas vigentes. A abertura em `app/` já deixou hooks da raiz inertes. `.claude/settings.json:2–73`, `bin/disjuntor.py:1837–1843,1864–1867,1912–1933`, `bin/hook_stop_fila1.py:1339–1345,1381–1404`, `app/docs/RELATO.md:3341`, `app/docs/PROMPTS.md:1877`.
- F5. `raias_estado.py` cruza worktrees com declarações do escritor `raia_agente.sh`. A união inclui declaradas, à frente e só sujas. Hoje as só sujas saem por nome, há declaração sem caminho e o HEAD por cópia não sai. `gerar_estado.py` lê o medidor e `relato.sh` gera, raspa e publica o ESTADO. `bin/raias_estado.py:42–135,138–174`, `bin/raia_agente.sh:40–59`, `bin/gerar_estado.py:211–229,329–331`, `bin/relato.sh:30–38,114–119,133–147`.
- F6. O construtor Codex escreve só em `<wt>/app`, com uma vaga por repo. `.claude/` consta no ignore e há precedente de instalação local negada pelo classificador. O arquivo do RELATO guarda marcos antigos. `CLAUDE.md:349–356,394,436–442`, `bin/codex_pergunta.sh:121–135,248–259`, `.gitignore:66`, `app/docs/RELATO.md:4356,4411`.
- F7. Janela de 24 h encerrada em 20/11 às 01:54 −03. F5 tem **58 entradas** em 18 horas com commits, pico de 11 às 08h. CADEIA tem **49** em 11 horas, pico de 17 às 16h. `main` tem **33**. São 107 entradas somadas nas duas cópias, 41 fora da lista do main (25 F5 + 16 CADEIA), com histórico alcançável contado pelo `git log`. Mudanças efetivas de HEAD e autoria por volta [nao verificado]. `medida_t2.txt:1–178`. [nao verificado]
- F8. Contagem V3 em 19/11. Contar uma devolução por evento recebido com token literal `CONSTRUIR` na primeira linha e pelo menos um alvo existente cujo prefixo colado difira do HEAD naquele recebimento. Tratar ausência de token de cópia, `copia=nova` e duplicata conforme suas portas. A lista de **seis candidatos de construção pelo título ou resumo** é `E4C-F5-CONSTROI-1911-0815`, `CADEIA-20-PASSO-1-CONSTROI-1911-1010`, `DST-L2-JZ1-C7-CONSTROI-1911-1200`, `E4C-F1-B2B-F0-CONSTROI-1911-1315`, `FERIADO-CONSTROI-E-C8-MEDE-1911-1400` e `E4C-F5-CONSTROI-A-1911-1710`. `medida_t2.txt:194,202,216,222,225,242`, `app/docs/PROMPTS.md:2608,2616,2630,2636,2639`, `CLAUDE.md:436–438`.
- F9. **0 devoluções por divergência demonstráveis, lista confirmada vazia, total real [nao verificado].** Os resumos também registram complementos que liberam ou retomam construção, inclusive F5 1735/1830 e CADEIA 1815/1836. O título com `CONSTROI` e o resumo com `CONSTRUIR` não identificam o token na primeira linha colada. Falta, por evento, o texto recebido com alvos/prefixos, o instante de recebimento e a fotografia de worktrees/HEADs nesse instante, inclusive das obras fora das duas listas. Exemplo medido de exposição: o merge F5 `3f1d63863` é das 17:07 e o 1710 foi recebido às 17:2x, mas o anexo não traz o prefixo colado, nem prova o HEAD naquele evento. `medida_t2.txt:37,181–254`, `app/docs/RELATO.md:60,111,119,158`.

## MUDA

- M1. A sessão constrói em cópia e instala os dois registros locais da raiz, somente para este instrumento. Codex verificador lê o diff depois da construção. Antes do código, publicar as cinco linhas abaixo no RELATO e mandá-las uma vez ao Codex. A execução depende de `aval Ronald: APROVA RASCUNHO TRAVAS-SEM-MEMORIA-V4 <sha256 12>`. O rascunho integral com SHA vai ao RELATO, e F5 C2 segue na frente. `fichas_v3_para_rascunho_2011_0205.txt:1`, `aval_rascunhado_pelo_codex_1911_1838.txt:5–7`, `CLAUDE.md:309,349–356,372–394`.
- M2. T1 usa a rota declarada pela sessão no despacho. Para a inteira pre-opus de **lateral ou espinha**, o pedido local carrega `PISTA_LATERAL=<OBRA>` junto de `bin/suite.sh --dir <cópia> --motivo "pre-opus <OBRA>"`. `PISTA_LATERAL` conserva o nome da chave e carrega o ID nas duas origens. Inteira pre-opus com chave ausente ou de outra obra é recusada antes de SSH e de runner VM. A exceção NOMINAL ao contrato PISTA-2 item 3 vale só para **“nem a suite que o opus recebe”**. O pouso segue pela trava da VM e certifica. Divergência VM × pista no mesmo commit = relatar e suspender o uso da pista até Ronald ver. `fichas_v3_para_rascunho_2011_0205.txt:25`, `app/docs/BACKLOG.md:2191,2194–2195`, `app/docs/RELATO.md:65,81`.
- M2, volta. Para volta de lateral, `PISTA_LATERAL=<OBRA>` acompanha `bin/volta_intermediaria.sh <cópia> <base> <saída>`. O script da volta conserva ruff e selo e passa o recorte efetivo ao ramo remoto de `suite.sh`. Volta cujo recorte cobre tudo continua sujeita ao `rc 2` da guarda atual. Régua, pre-push, pouso e teste focado mantêm seus comandos. `bin/suite.sh:129–180,215–263`, `bin/volta_intermediaria.sh:18–46`, `CLAUDE.md:95–106`, `app/docs/RELATO.md:65,81`.
- M3. Registrar um `PreToolUse` de Bash para a porta canônica. Ele lê `PISTA_LATERAL`, a obra do motivo e o SHA completo da cópia. Requer o mesmo ID nos dois campos e, para a inteira pre-opus, uma linha literal no início da linha `PRONTO <OBRA> item a item: sha=<SHA completo>` em `app/docs/RELATO.md` ou `app/docs/RELATO-ARQUIVO.md`. Um registro arquivado vale para o mesmo SHA. Cabeçalho de outro SHA, de outra obra, indentado ou citado vale como ausente. Resposta `SEM PRONTO ITEM A ITEM`. O mesmo verificador de instrumento roda no executor imediatamente antes de criar a ref e pedir o job. O SSH recebe o SHA conferido e as opções documentadas. `bin/disjuntor.py:1796–1798,1837–1843`, `bin/suite.sh:219–263`, `CLAUDE.md:439–442`, `app/docs/BACKLOG.md:2191`.
- M4. A sessão publica o PRONTO com cada item, medida e pendência. O hook confere identidade da obra e SHA. Após mais de três voltas Codex, pendências registradas permitem a inteira e a subida ao opus pela regra vigente. Uma recusa de PRONTO fecha o pedido antes do SSH. Falha de infraestrutura segue a contingência registrada da rota, com `PISTA: VM porque <motivo>` quando couber. Vermelho de teste retorna o rc do job. `CLAUDE.md:372–381`, `app/docs/RELATO.md:81,1010,1209`, `app/docs/BACKLOG.md:2191`.
- M5. T2 faz `raias_estado.py` expor o nome e o HEAD completo observado por `git rev-parse HEAD` em cada worktree da união, incluindo as só sujas. O ESTADO publica um `copia=<nome>@<HEAD12>` por cópia resolvida. Raia declarada sem caminho ou sem HEAD resolvível publica `raia=<nome> SEM WORKTREE`. O hook lê o mesmo medidor na hora do envio. O ESTADO é a fotografia publicada. `bin/raias_estado.py:42–48,73–135,138–174`, `bin/gerar_estado.py:211–224`, `bin/relato.sh:37–38,114–119`.
- M6. `UserPromptSubmit` examina aval colado com o token literal `CONSTRUIR` na primeira linha. Na linha `alvos=<nome>,<nome>`, listar todas as cópias tocadas e dar, na mesma ordem, um `copia=<nome>@<prefixo>` por alvo existente ou `copia=nova` na posição de cada alvo novo. Prefixo hexadecimal de 12 caracteres até o SHA completo. Nome é o basename da worktree, com escape percentual para delimitadores. `fora=<nome>,...` declara cópias vinculadas à obra que ficam fora do aval. Toda raia declarada com a mesma `OBRA` consta em `alvos` ou `fora`. A sessão explicita em `alvos` cópias tocadas sem declaração, inclusive descendentes. `bin/raias_estado.py:73–102,125–135`, `bin/raia_agente.sh:40–59`, `app/docs/RELATO.md:1513`, `travas_sem_memoria_v2_2011_0110.txt:5` [nao verificado].
- M6a. **Recomendada.** Por alvo existente, o prefixo resolve univocamente para o SHA completo lido. Antes da colagem, a sessão registra no RELATO `LEITURA-COPIA <ID-do-aval> <OBRA> <cópia> <SHA-lido> <fim-com-fuso> <referência-da-resposta>`, após leitura Codex pelo wrapper, `rc 0` e amostra conferida de arquivo:linha. Janela proposta de **60 min** entre o fim da leitura e a colagem. O hook aceita HEAD igual ou descendente quando **todos** os commits de `SHA-lido..HEAD` pertencem à mesma volta já autorizada e em curso nessa cópia. A sessão que faz o commit grava nele `Volta-Autorizada: <ID-do-aval-que-abriu-a-volta>/<OBRA>/<cópia>/<volta>`, conferido contra o despacho vigente no RELATO. Commit importado entra no exame do delta, inclusive por merge. O hook guarda o HEAD validado e o despacho repete a checagem se ele andar. `bin/codex_pergunta.sh:266–273,304–319,341–347`, `CLAUDE.md:357–370,439–442`, `bin/raias_estado.py:89–102`.
- M6b. **Custo em recolagens por 24 h.** No cenário de uma colagem com prefixo anterior por cada uma das 41 entradas fora do main, tratando cada entrada como avanço da própria volta com prova recente, V3 = **41**, M6a = **0**. Na medida real, M6a = **0 por avanço próprio coberto**. Recolagens por leitura vencida, prova ausente ou delta alheio [nao verificado]. Para calcular o total, cruzar F9 com hora/SHA de cada leitura e atribuição dos commits por volta. `medida_t2.txt:1–178`, M6a–M7. [nao verificado]
- M7. ALVOS com nome repetido, ambíguo, omitido, sem HEAD ou com prefixo divergente segundo M6a devolvem `AVAL SEM LEITURA DA COPIA (HEAD atual <hash>)` por alvo, com `indisponível` onde nenhum HEAD puder ser lido. `copia=nova` vale para um alvo sem worktree elegível nem declaração, inclusive quando há outros alvos existentes no mesmo aval. Uma declaração sem worktree exige saneamento para aquele alvo. A sessão registra `AVAL-DEVOLVIDO <ID>` no topo do RELATO pelo escritor existente, como recado que preserva a janela do Stop, e segue os itens andáveis. Ordens sem o token no cabeçalho conservam a passagem atual pelo detector e pelas guardas próprias. `bin/raias_estado.py:73–102`, `bin/hook_stop_fila1.py:413–524,1381–1404`, `app/core/esteira_vigia.py:846–892`, `CLAUDE.md:262–275,436–438`, `bin/codex_pergunta.sh:252–259`.
- M8. Fronteira T2 = primeiro evento de controle comprovado com os registros carregados na sessão de trabalho. Registrar data/hora com fuso, sessão, SHA do instrumento e digest do settings. Aval recebido antes, provado em PROMPTS/BACKLOG, segue sem releitura. Toda recolagem após o marco é novo evento de validação. Recolagem válida de título já recebido segue a resposta de duplicata da casa. `CLAUDE.md:436–438`, `.claude/settings.json:2–73`, `app/docs/RELATO.md:3341,4356`.
- Alternativa T1 em uma linha. Um `bin/pista_pede.sh pre-opus <OBRA> <cópia>` pode carregar obra e cópia no argv, usando o mesmo leitor de PRONTO e a mesma interface SSH. `app/docs/BACKLOG.md:2191,2195`.
- Alternativa T2 em uma linha. Segurar o despacho até leitura Codex do HEAD e do delta no recebimento, reaproveitando a colagem se o delta for só da volta autorizada. **0/41 recolagens** no mesmo cenário, uma releitura por evento com delta, total por delta alheio [nao verificado]. `medida_t2.txt:1–178`, `CLAUDE.md:357–370`, M6a–M6b.

### CONTRATO-DE-INSTRUMENTO

1. Quem chama — sessão/raia pelo Bash da raiz para T1, Code ao receber o aval para T2, `relato.sh` ao publicar o ESTADO. `.claude/settings.json:62–72`, `bin/relato.sh:37–38`, M2–M7.
2. Quem espera — sessão em shell de fundo, por `ssh pista espera <id>` e veredito com `Ran` e rc. T2 responde no evento de entrada. `app/docs/BACKLOG.md:2191`, `CLAUDE.md:316–319`, M7.
3. Falha/limite — recusa só do pedido ou aval afetado, antes de enfileirar ou usar o aval. Pista recebe árvore commitada. Divergência VM/pista fica registrada e suspende o uso até Ronald ver. `app/docs/BACKLOG.md:2191`, `CLAUDE.md:262–275`, M3–M8.
4. Medida — por caso, guardar porta, obra, SHA, rc/veredito, chamadas SSH/VM e linha de recusa ou publicação. Em job aceito, guardar id e tempo. `app/docs/RELATO.md:81,1497,1529`, M3–M8.
5. Quem desliga — Ronald, restaurando os dois registros anteriores e retirando a seleção da nova rota, com linha no RELATO. `app/docs/BACKLOG.md:2191`, `app/docs/RELATO.md:81,4356`, M1–M3.

## RED

Entradas de teste: `O=LATERAL-TESTE`, `C=wt-a`, `D=wt-b`, `H/J` = HEADs reais da fixture, `H0` = SHA anterior de C, `B` = base do recorte. `P` é `PISTA_LATERAL=LATERAL-TESTE bash bin/suite.sh --dir "$C" --motivo "pre-opus LATERAL-TESTE"`. `U` é colar `aval Ronald: LATERAL-TESTE CONSTRUIR` com o corpo do caso. Antes da cura, testar as mesmas entradas e registrar qual expectativa falhou. Resultados [nao verificado]. `CLAUDE.md:28–29`, `bin/tests/test_suite_motivo.sh:14–42`.

- **Pre-opus lateral** — P1 `P`, C commitada/ref apta, sem PRONTO → `SEM PRONTO ITEM A ITEM`, zero SSH e zero runner. P2 `P`, só há `PRONTO` de `O@H0` e de outra obra em H → mesma recusa. P3 `P`, marcador em `RELATO.md` para `O@H` → um `pede H` sem `--only`, id e `espera` com `Ran/OK/rc 0`. P4 `P`, mesmo marcador só no ARQUIVO, quatro voltas Codex e pendência escrita → mesmo pedido e subida ao opus após o verde. P5 `P`, PRONTO válido e job vermelho → `rc 1`, zero runner VM. `bin/suite.sh:236–263`, `app/docs/BACKLOG.md:2191`, `CLAUDE.md:372–381,439–442`, M3–M4.
- **Volta de lateral** — V1 `PISTA_LATERAL=LATERAL-TESTE bash bin/volta_intermediaria.sh "$C" "$B" "$OUT"`, C commitada e recorte próprio, sem PRONTO → ruff/selo e um `pede H --only <labels efetivas>`, zero trava VM. V2 mesmo comando, seleção cobre todos os módulos → guarda atual `rc 2` da suite, volta vermelha e zero SSH. V3 mesmo V1, pista indisponível e contingência vigente → recorte na VM e uma linha de causa. `bin/volta_intermediaria.sh:18–47`, `bin/suite.sh:162–180,215–263`, `app/docs/RELATO.md:81,1010`, M2–M4.
- **Pre-opus da espinha** — L1a `PISTA_LATERAL=ESPINHA-TESTE bash bin/suite.sh --dir "$ESP" --motivo "pre-opus ESPINHA-TESTE"`, cópia commitada, sem PRONTO → mesma recusa de P1, zero SSH e zero runner. L1b mesmo comando, PRONTO da obra e do SHA → um job inteiro na pista, `Ran/OK/rc 0`, zero runner VM. L1c mesmo pedido, pista e VM com vereditos divergentes medidos no mesmo SHA → relatar os dois e suspender novos pedidos à pista até Ronald ver. L1d mesmo comando sem `PISTA_LATERAL` → recusa antes de SSH ou VM. `bin/suite.sh:215–263,316–325`, `app/docs/BACKLOG.md:2191`, M2–M4.
- **VM e focado** — L2 `bash bin/suite.sh --dir "$C" --motivo "pouso LATERAL-TESTE"`, cópia sem PRONTO → inteira pela VM. L3 `bash bin/suite.sh --dir "$C" --only "$MODULO"`, módulo próprio e sem PRONTO → recorte roda. L4 `bash bin/suite.sh --dir "$C" --motivo "pre-opus"`, inteira → `rc 2` anterior. Zero SSH em L2–L4. `bin/suite.sh:215–263,316–325`, `app/docs/RELATO.md:65`, M2.
- **Régua e Git** — R1 `bash bin/regua.sh --force`, mesmo com carimbo verde e trava livre → runner integral executa, sem `--motivo`. G1 `git push remoto-teste HEAD:refs/heads/smoke`, hook instalado, commit novo e sem carimbo compatível → arsenal CI e `tenants` executam sobre o commit, push liberado se verdes. G2 mesmo push em fixture com carimbo compatível → reaproveita arsenal e executa `tenants`. Zero SSH. `bin/regua.sh:84–101,134–169`, `bin/hooks.sh:58–65`, `bin/pre-push.sh:27–74,159–200`, M2.
- **Aval** — A1 `U` com `alvos=wt-a`, sem `copia` → devolve com H. A2 `U` com `alvos=wt-a copia=wt-a@H0`, HEAD H, sem leitura registrada de H0 → devolve com H. A3 `U` com `alvos=wt-a,wt-b copia=wt-a@H12 copia=wt-b@J`, ambas da obra com leituras recentes → entra. A4 `U` com só `wt-a`, estando `wt-b` declarada para O → devolve; com `fora=wt-b` explícito → entra para C. A5 `U` com `alvos=wt-nova copia=nova`, nome livre → entra; mesmo envio com nome já declarado sem worktree → devolve. A6 `U` com `alvos=wt-sem copia=wt-sem@H12`, declaração sem worktree → devolve com HEAD indisponível. A7 `U` com C válida e `wt-nova` livre → entra com um token por alvo. Nos negativos, uma linha de devolução por evento e o próximo item andável segue. `bin/raias_estado.py:73–135`, `CLAUDE.md:262–275`, M5–M8.
- **Corte e outras entradas** — A8 `U` inválido recepcionado antes do marco, continuar o item após ativação → continuidade; recolagem do mesmo texto → recusa do novo evento. A9 colar `aval Ronald: LEITURA-TESTE` com `CONSTRUIR` só no corpo → T2 deixa passar. A10 enviar leitura, sonda e despacho pelos seus comandos vigentes de `bin/codex_pergunta.sh`, com entradas válidas e #G ativa → chegam às guardas atuais, zero recusa T1/T2. `CLAUDE.md:344–358,436–438`, `bin/codex_pergunta.sh:241–259`, M7–M8.
- **Volta em curso** — A11 `U` com H0 testemunhado há 10 min, HEAD H dois commits adiante, ambos da volta autorizada → entra com a colagem original e prova do delta. A12 mesma entrada com um commit de fora da volta, inclusive como segundo pai de merge → devolve com H. A13 HEAD H0 e leitura de H0 há 61 min → devolve com H0. A14 HEAD avança entre entrada e despacho com mais um commit da própria volta → despacho repete a checagem e segue com a mesma colagem. `bin/codex_pergunta.sh:266–273`, `bin/raias_estado.py:89–102`, M6a–M8.
- **ESTADO** — E1 `python3 bin/gerar_estado.py "$RAIZ" "$OUT"` e `DEST_RELATO="$DEST_TESTE" bash bin/relato.sh` com fixture raspada, C e D declaradas, uma só suja e uma sem worktree → três `copia=<nome>@<HEAD12>` e uma `SEM WORKTREE`, no ESTADO gerado e publicado. E2 repetir após avançar só H → muda só o token de C entre as cópias. `bin/raias_estado.py:125–174`, `bin/gerar_estado.py:211–224,398–407`, `bin/relato.sh:23,37–38,114–147`, M5.

## SELO

- `bin/tests/test_travas_sem_memoria.sh`, no host, chamado pelo glob da régua antes do Django. Adaptar `test_suite_motivo.sh` para seleção efetiva, `test_raia_declarada_no_estado.sh` para HEAD/publicação e usar o molde de fixtures e mutantes de `test_disjuntor.sh`. SSH falso registra argv e enfileiramentos, Git real temporário dá os HEADs e runners de teste registram chegada. `CLAUDE.md:127–130`, `bin/regua.sh:134–145`, `bin/tests/test_suite_motivo.sh:14–42`, `bin/tests/test_raia_declarada_no_estado.sh:43–84`, `bin/tests/test_disjuntor.sh:82–101,1532–1547`.
- Despachar payloads pelos registros lidos do settings candidato, por evento e matcher. Controle íntegro verde. Retirar só o registro T1 derruba P1 no teste do evento; retirar só o T2 derruba A2. Aceitar PRONTO de qualquer SHA derruba P2, ignorar segunda cópia derruba A4 e omitir HEAD na publicação derruba E1. A chamada direta ao script complementa a prova do executor. Aceitar qualquer descendente derruba A12 e ignorar idade da leitura derruba A13. `.claude/settings.json:62–72`, `bin/tests/test_disjuntor.sh:82–88,1532–1547`, M3–M8.
- **Smoke pré-pouso.** Ronald abre em terminal avulso, sem tmux novo de trabalho, com `cd -- "$COPIA_SMOKE" && timeout --foreground --kill-after=5s 10m claude`. `$COPIA_SMOKE` é o caminho absoluto da raiz da cópia candidata. Settings locais e comandos apontam para essa cópia. SSH, runners e destino de publicação são da fixture. Provar carga real dos dois eventos, P1/P3 e A1/A2/A3/A8, recorte V1, publicação E1 e continuidade com 

[truncado em 20000 de 26763 caracteres]

## 20/11 01:57 · sessao 2ab758cbcfcd · E4C-F5-CONSTROI · construtor · origem=Code

### 1. Por que o filtro rodava duas vezes

O GET renderiza o modal uma vez. A fixture tem dois cartões distintos, o chamado pai da disputa e o chamado destino das perguntas. O pai recebe a disputa pelo mapa de chamados e o destino recebe **a mesma instância Python** pelo mapa de perguntas. Os dois mapas vêm dos mesmos objetos de `todas_disputas`. O template percorre os cartões e pede a contagem uma vez para cada botão. `chamados/views.py:927`, `chamados/views.py:956`, `core/tests/test_selo_performance.py:306`, `chamados/services/fio.py:222`, `chamados/services/fio.py:255`, `chamados/services/fio.py:280`, `chamados/services/fio.py:287`, `templates/chamados/partials/modal_fio.html:68`, `templates/chamados/partials/modal_fio.html:396`.

### 2. Quem produz e quem consome o conjunto

| Sítio | Uso |
|---|---|
| `chamados/juizes.py:1762` | Seleciona as respostas com texto da disputa, carrega as decisões em lote, julga as candidatas e devolve um queryset por PK. A lista de PKs é interna ao juiz. |
| `chamados/views.py:1415` | A view do botão testa `.exists()`, passa o queryset ao escritor e usa o número atualizado no retorno da ação. `chamados/views.py:1454`. |
| `chamados/services/acoes_disputa.py:92` | A ação `reabrir_disputa` do POST do fio testa `.exists()` e passa o queryset ao mesmo escritor. `chamados/services/fio.py:690`, `chamados/services/acoes_disputa.py:99`. |
| `chamados/models.py:2404` | `reabrir_por_colab` testa `.exists()` e passa o queryset ao escritor dentro da transação que incrementa o contador. As entradas HTML e REST delegam a esse método. `chamados/models.py:2414`, `chamados/views.py:1329`, `api/views.py:2712`. |
| `chamados/services/disputa_emissao.py:1617` | `reabrir_para_correcao` lê o queryset para guardar a foto das respostas na trilha e usa `descarimbar_em_lote` para escrever. Devolve a quantidade atualizada. `chamados/services/disputa_emissao.py:1633`, `chamados/services/disputa_emissao.py:1656`. |
| `chamados/templatetags/chamados_tags.py:208` | A tag usa `.count()`. Modal e `detalhe_local.html` usam esse número no botão e na confirmação. A lista `perguntas_disputa` do detalhe vem de `pergunta_viva`, em uma leitura separada. `templates/chamados/partials/modal_fio.html:396`, `templates/chamados/detalhe_local.html:21`, `chamados/services/acoes_chamado.py:345`. |

A reabertura administrativa de disputa fechada chama `reabrir_admin` e abre uma pergunta nova pela porta de conversa. `chamados/services/acoes_disputa.py:239`, `chamados/services/acoes_disputa.py:259`.

### 3. Universo carregado pelo fio

Para a disputa **aberta da fixture**, o fio já carrega todas as perguntas da disputa com `decisoes`, antes de separá-las por cartão. Para uma **fechada**, aplica `q_vivas()` no SQL e depois `pergunta_viva` em Python. Uma resposta com texto, validada e ainda sem carimbo de materialização, exemplifica a diferença. A validação a exclui da carga da fechada, enquanto `pergunta_sem_lastro_ou_viva` a admite pelo carimbo ausente. `chamados/services/fio.py:251`, `chamados/services/fio.py:260`, `chamados/services/fio.py:270`, `chamados/models.py:3636`, `chamados/juizes.py:1410`, `chamados/juizes.py:1503`, `chamados/juizes.py:1762`.

### Cura e arquivos tocados

- **`chamados/templatetags/chamados_tags.py:231`** guarda só o inteiro devolvido pela leitura existente, por PK da disputa, no `RenderContext`. A primeira leitura de cada disputa chama o juiz com o prefetch intacto. `chamados/templatetags/chamados_tags.py:222`, `chamados/juizes.py:1765`.
- **`templates/chamados/partials/modal_fio.html:396`** usa essa tag para o botão e a confirmação, tanto no GET quanto na resposta renderizada após a ação. `chamados/views.py:927`, `chamados/views.py:1393`.
- **`chamados/tests/test_e4c_f5_voltaC3_reabrir.py:89`** cobra uma chamada e dois botões com N=3. Os outros casos associam N=1 e N=2 aos respectivos cartões e verificam N=0 em um novo GET após o POST real. `chamados/tests/test_e4c_f5_voltaC3_reabrir.py:95`, `chamados/tests/test_e4c_f5_voltaC3_reabrir.py:103`.

Ficou de fora alimentar o juiz com a carga do fio, pois isso ampliaria seu contrato e exigiria tratar o universo parcial das fechadas. `chamados/services/fio.py:251`, `chamados/juizes.py:1762`.

**Selo esperado no segundo GET: 92 → 89 consultas [nao verificado].** A previsão subtrai os três SQLs da segunda leitura do mesmo conjunto no anexo `medida_f3.txt`. O teto segue em 90. `core/tests/test_selo_performance.py:143`, `core/tests/test_selo_performance.py:377`.

### Comandos para o Code

Os dois arquivos Python e as quatro mutações compilaram em memória. Banco e suíte ficaram para a certificação. A compilação do template ficou bloqueada pela ausência de `django.template` no interpretador local, e o Ruff estava indisponível. Execução do RED, dos mutantes e do selo [nao verificado]. `chamados/tests/test_e4c_f5_voltaC3_reabrir.py:20`, `templates/chamados/partials/modal_fio.html:396`.

```bash
C3=chamados.tests.test_e4c_f5_voltaC3_reabrir.ReabrirNoModalTest
rodar() { PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci --noinput "$@"; }

rodar "$C3" \
  core.tests.test_selo_performance.SeloPerformanceTest.test_chamados_modal \
  chamados.tests.test_e4c_f5_voltaB_lotes.CarregadoresVoltaBTest.test_RED_k29_respostas_corrigiveis \
  chamados.tests.test_ui_resposta_diz_o_que_e.AsDuasCascasDeCartaoTest

F5_VOLTA_C3_MUTANTE=sem_memo rodar "$C3.test_RED_dois_cartoes_consultam_o_juiz_uma_vez"
F5_VOLTA_C3_MUTANTE=chave_unica rodar "$C3.test_MORDE_cada_disputa_mostra_sua_contagem"
F5_VOLTA_C3_MUTANTE=entre_renders rodar "$C3.test_MORDE_novo_render_depois_de_reabrir"
F5_VOLTA_C3_MUTANTE=zero rodar "$C3.test_RED_dois_cartoes_consultam_o_juiz_uma_vez"

ruff check --no-cache chamados/templatetags/chamados_tags.py \
  chamados/tests/test_e4c_f5_voltaC3_reabrir.py
```

**LEI-AKITA** origem=`chamados/juizes.py:1762`, testemunha=`chamados/templatetags/chamados_tags.py:231` e `templates/chamados/partials/modal_fio.html:396`, RED=`chamados/tests/test_e4c_f5_voltaC3_reabrir.py:89`, quem-mais-le=`chamados/views.py:1415`, `chamados/services/acoes_disputa.py:99`, `chamados/models.py:2414` e `templates/chamados/detalhe_local.html:21`, juizes novos=0, padrao=Identity Map (`docs/GLOSSARIO.md:30`).

## 20/11 01:40 · sessao fab078a7cd14 · E4C-F5-CONSTROI · construtor · origem=Code

**C2 entregue para E1, E2, F1 e F2.** O censo estático bate com o registro atualizado, e a âncora de F1 rejeita o mutante que deixa a chamada apenas em comentário. Banco e testes ficaram para o Code. [Registro F2](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/core/tests/test_medida_turno_do_par.py:271) · [Âncora F1](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_contract_dedup_status.py:142) [nao verificado]

### Arquivos tocados e usos dos avais

A última coluna conta instruções `assert` e chamadas `self.assert*`, nessa ordem, por AST contra `9d386d1b1`.

| Arquivo | Classe | Antes → depois | Asserções |
|---|---|---|---|
| [chamados/tests/e3_v8_apoio.py:77](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e3_v8_apoio.py:77) | ii, E1 | O módulo falso de `leitores()` passa a exportar `DecisaoPergunta` do helper único `chamados.tests.duble_pergunta`, já usado em B3 por `e3_volta7_apoio.py:111`. | `0/0 → 0/0` |
| [chamados/tests/e3_volta7_apoio.py:33](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e3_volta7_apoio.py:33) | v, E2 | `Query` ganha `prefetch_related` como identidade. A subclasse V8 passa a herdar a mesma implementação em `e3_v8_apoio.py:26`. | `0/0 → 0/0` |
| [chamados/tests/test_contract_dedup_status.py:142](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_contract_dedup_status.py:142) | i, F1 | A âncora textual da chamada sem argumentos passa a exigir a atribuição `_pend = self.respostas_sem_veredito(perguntas=perguntas)` no AST do corpo de `fechar`. | `0/12 → 0/12` |
| [core/tests/test_medida_turno_do_par.py:271](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/core/tests/test_medida_turno_do_par.py:271) | i, F2 | `434 → 437`, mesma linha que filtra a célula por `loc.date()`. `chamados/services/materializacao.py:437`. | `0/86 → 0/86` |
| [core/tests/test_medida_turno_do_par.py:271](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/core/tests/test_medida_turno_do_par.py:271) | i, F2 | `455 → 458`, mesma linha que calcula `(loc.date() - cel.data).days`. `chamados/services/materializacao.py:458`. | Mesmo módulo |

As duas ocorrências de F2 têm conteúdo idêntico ao de `d9319a215`. São as únicas diferenças encontradas pelo censo de linhas congeladas em `core/tests/test_medida_turno_do_par.py:558`.

### F3 · atribuição e parada

A atribuição estática aponta **B, `d0210a400`**. Os dois `SELECT` candidatos são sobre `chamados_decisaopergunta`, ambos no `prefetch_related('decisoes')` de `perguntas_corrigiveis`. A fixture abre o modal de um colaborador com dois cartões, pai e destino, ligados à mesma disputa. Cada cartão chama o filtro de Reabrir, que monta um queryset novo para a mesma disputa. Isso dá um `SELECT` de decisões por cartão. `core/tests/test_selo_performance.py:298`, `:377` · `chamados/services/fio.py:275`, `:287` · `templates/chamados/partials/modal_fio.html:396` · `chamados/templatetags/chamados_tags.py:222` · `chamados/juizes.py:1765`.

Os leitores que C acrescentou ao estado de aplicação do cartão recebem as instâncias do lote de decisões do fio. D alterou `fechar`, enquanto o GET medido renderiza o modal. `chamados/services/materializacao.py:193`, `:889`, `:927` · `chamados/services/fio.py:260`, `:378` · `chamados/models.py:2071` · `chamados/views.py:951`.

**PAREI em F3.** O candidato é um prefetch em lote repetido pelo render. A condição do aval era uma leitura avulsa a incorporar ao lote. O anexo informa **92 / teto 90**. A sonda abaixo deve isolar o delta esperado **92 → 90** ao restaurar em memória somente o corpo anterior a B. SQL efetivamente emitido e total da sonda **[nao verificado]**. `chamados/juizes.py:1765` · `core/tests/test_selo_performance.py:143`, `:333`.

### Varredura de C e D

| Família | Resultado da inspeção |
|---|---|
| Vigia | Os dois dublês faltantes estão em E1/E2. Os leitores de vivas em fechadas e carimbos por dono têm cobertura ORM em `chamados/tests/test_e4c_f5_voltaC_lotes.py:154`, `:167`. |
| Materialização | O contador já usa `_Recorte(Linhas)`, com prefetch identidade. A elegibilidade e a anuência já recebem o compositor do helper. A fixture do estado de aplicação já fornece o que o compositor real lê. `chamados/tests/test_bug_par_c2_sem_banco.py:32`, `:78` · `chamados/tests/test_e4b_classe_e_memoria.py:193` · `chamados/tests/test_bug_par_sem_banco.py:182` · `chamados/tests/test_estado_aplicacao.py:38`. |
| Auditoria, dedup e universo do fato | O dublê do piso I6 substitui `_i6_pares` inteiro. Os corpos alterados têm cobertura ORM nos selos de C. O harness sem banco do backfill de decisões chama o contador de validação sem linha. `chamados/tests/test_selo_lapide_legado.py:85` · `chamados/tests/test_e4c_f5_voltaC_lotes.py:89`, `:119`, `:141`, `:230` · `chamados/tests/test_e4c_f5_voltaC_sitios.py:65` · `chamados/tests/test_e4c_f6f7_c7_sem_banco.py:50`. |
| Backfill do fio, disparo, estoque e retroativo | `Linhas` já fornece `filter`, `select_related` e `prefetch_related` para o backfill. A matriz do disparo já fornece `prefetch_related` pelo `Mock`. Estoque e emissão retroativa exercitam os caminhos alterados por ORM. `chamados/tests/test_e4b_classe_e_memoria.py:165`, `:650` · `chamados/tests/test_e4a2_matriz_memoria.py:124` · `chamados/tests/test_f1nova_b2_leitores.py:249` · `chamados/tests/test_e4c_f5_voltaC_lotes.py:198`. |
| Arquivamento, becos e porta D | Os caminhos de execução encontrados usam ORM. O consumidor estrutural de D com âncora deslocada foi F1. `chamados/tests/test_c7_red_minimos.py:283` · `chamados/tests/test_e4c_f5_voltaC_lotes.py:218` · `chamados/tests/test_e4c_f5_opus_models.py:149` · `chamados/tests/test_contract_dedup_status.py:135`. |

### Comandos para o Code

Os quatro labels cobrem E1, E2, F1 e F2. A segunda chamada inclui os dois selos do modal, com `test_selo_teto_modal` em `chamados.tests`. Execução **[nao verificado]**. `chamados/tests/test_e3_portas_tarefa.py:505` · `chamados/tests/test_e3_degrau_dono_e_porta.py:388` · `chamados/tests/test_contract_dedup_status.py:135` · `core/tests/test_medida_turno_do_par.py:558` · `core/tests/test_selo_performance.py:377` · `chamados/tests/test_selo_teto_modal.py:37`.

```bash
e1=chamados.tests.test_e3_portas_tarefa.MatrizV8Test
e2=chamados.tests.test_e3_degrau_dono_e_porta.LeitoresPortaApp
f1=chamados.tests.test_contract_dedup_status.ContratoDedupDistingueStatusTest.test_porta_humana_de_disputa_presta_contas
f2=core.tests.test_medida_turno_do_par.CatracaTurnoDoParTest.test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit
f3=core.tests.test_selo_performance.SeloPerformanceTest.test_chamados_modal

PYTHONDONTWRITEBYTECODE=1 python -B manage.py test --settings=config.settings.ci "$e1" "$e2" "$f1" "$f2"
PYTHONDONTWRITEBYTECODE=1 python -B manage.py test --settings=config.settings.ci "$f3" chamados.tests.test_selo_teto_modal
```

Os mutantes abaixo são em memória. E1 deve reproduzir o `ImportError`, E2 o `AttributeError`, F1 deve falhar com a chamada só em comentário, e F2 deve falhar ao restaurar cada coordenada antiga isoladamente. Resultados dos mutantes **[nao verificado]**. `chamados/services/vigia_fio.py:504`, `:571` · `chamados/tests/test_contract_dedup_status.py:142` · `core/tests/test_medida_turno_do_par.py:558`.

```bash
c2() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci \
    python -B - "$@" <<'PY'
import inspect
import subprocess
import sys
import traceback
from contextlib import ExitStack
from types import SimpleNamespace
from unittest.mock import patch

import django
django.setup()

from django.conf import settings
from django.test.utils import get_runner

modo, label = sys.argv[1:]
with ExitStack() as escopo:
    if modo == 'e1':
        from chamados.tests import e3_v8_apoio as apoio
        modulo = apoio.modulo

        def sem_compositor(nome, **campos):
            if nome == 'chamados.models' and 'ChamadoColaborador' in campos:
                campos.pop('DecisaoPergunta')
            return modulo(nome, **campos)

        escopo.enter_context(patch.object(apoio, 'modulo', sem_compositor))
    elif modo == 'e2':
        from chamados.tests.e3_volta7_apoio import Query
        escopo.callback(setattr, Query, 'prefetch_related', Query.prefetch_related)
        del Query.prefetch_related
    elif modo == 'f1':
        from chamados.models import DisputaSupervisao
        from chamados.tests import test_contract_dedup_status as contrato

        def fonte_com_comentario(fn):
            fonte = inspect.getsource(fn)
            if fn is DisputaSupervisao.fechar:
                chamada = '_pend = self.respostas_sem_veredito(perguntas=perguntas)'
                return fonte.replace(chamada, '# ' + chamada + '\n        _pend = []', 1)
            return fonte

        escopo.enter_context(patch.object(
            contrato, 'inspect', SimpleNamespace(getsource=fonte_com_comentario)))
    elif modo in ('f2-437', 'f2-458'):
        from core.tests import test_medida_turno_do_par as catraca
        chave = ('chamados/services/materializacao.py::_plantio_preenche_furo_na_data', 'B1')
        linhas = [434, 458] if modo == 'f2-437' else [437, 455]
        escopo.enter_context(patch.dict(
            catraca.LINHAS_REGISTRADAS['derivacoes_de_dia'], {chave: linhas}))
    elif modo in ('f3', 'f3-sem-b'):
        from chamados import juizes
        from chamados.tests.e4_apoio import funcao
        from core.tests.test_selo_performance import SeloPerformanceTest
        from django.db import connection

        if modo == 'f3-sem-b':
            fonte = subprocess.check_output(
                ['git', 'show', 'd0210a400^:app/chamados/juizes.py'], text=True)
            anterior = funcao('chamados/juizes.py', 'perguntas_corrigiveis',
                              vars(juizes).copy(), fonte=fonte)
            escopo.enter_context(patch.object(juizes, 'perguntas_corrigiveis', anterior))

        medir = SeloPerformanceTest._medir_rota

        def medir_com_sql(self, nome, url):
            consultas = []

            def registrar(execute, sql, params, many, context):
                pilha = ()
                if sql.lstrip().upper().startswith('SELECT') and 'chamados_decisaopergunta' in sql:
                    pilha = tuple(f for f in traceback.extract_stack() if 'chamados/' in f.filename)
                consultas.append((sql, params, pilha))
                return execute(sql, params, many, context)

            with connection.execute_wrapper(registrar):
                total, ms = medir(self, nome, url)
            print(f'{nome} · {total} consultas')
            for sql, params, pilha in consultas[-total:]:
                if pilha:
                    print(sql, params)
                    print(' <- '.join(f'{f.filename}:{f.lineno}' for f in reversed(pilha[-5:])))
            return total, ms

        escopo.enter_context(patch.object(SeloPerformanceTest, '_medir_rota', medir_com_sql))
    else:
        raise SystemExit(f'Modo desconhecido: {modo}')

    raise SystemExit(bool(get_runner(settings)(verbosity=2, parallel=1).run_tests([label])))
PY
}

c2 e1 "$e1.test_v8_R1_colab_alcancavel_e_leitores"
c2 e2 "$e2.test_vigia_conta_app_sem_KeyError_e_publica_no_log"
c2 f1 "$f1"
c2 f2-437 "$f2"
c2 f2-458 "$f2"
c2 f3 "$f3"
c2 f3-sem-b "$f3"
```

**LEI-AKITA** — origem em `vigia_fio.py:504`, `:571`, `models.py:2072` e `materializacao.py:437`, `:458` · testemunhas no dublê único `duble_pergunta.py:12`, na chamada viva de `fechar` e no censo de `core/tests/test_medida_turno_do_par.py:558` · RED e mutantes entregues para o Code **[nao verificado]** · F3 parado pelo escopo literal `docs/LEIS.md:62` · juízes novos **0**, conforme os quatro arquivos da tabela.

## 20/11 01:35 · sessao fbf1984f8eb2 · CADEIA-20-PASSO-1 · leitura · origem=Code

aval Ronald: CADEIA-20-IMPRESSAO-V2 (rascunho Codex 20/11) -- unidade: célula-dia, impressão da ata; lei: L-124, desenho célula soberana

## FONTE

- **F1 — Base e alcance.** Volta 3 da CADEIA-20, cópia `wt-codex-cadeia-20-passo-1-112113`, hash informado `a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6`. Caminho da cópia em `app/docs/RELATO.md:100`. A árvore local tem o nome do hash e veio sem metadados Git. Identidade do HEAD [nao verificado]. L-124 conserva a foto anterior ao horizonte até fato novo. `app/docs/LEIS.md:145`
- **F2 — Impressão atual.** SHA-256 de batidas (`pk`, tipo, instante), status da ausência que cobre o dia, chamados de batida (`pk`, `status_local`), DNA (`dna_versao`, marcos) e condicionais `veto`, `teto`, `regra`, `enc`, `fio`, `cobparcial`, `isento`, `sem_adesao`. A parcial pode carregar período e índices dos marcos dispensados. A porta da célula grava `impressao` e os campos da lavra. `app/ponto/services/cartorio.py:477–490,493–578,1496–1501` `app/ponto/portas/celula.py:1189–1201`
- **F3 — Portas humanas de `status_local`.** Detalhe do chamado e resposta rápida do DP no fio aceitam `novo_status`. Aprovar exceção, responder agenda ou escala e autorizar ou recusar geofence também chamam a transição. A decisão de benefício chama a mesma porta. O cartório só carrega os quatro `FURO_MODULES` declarados e agrupa os chamados pelo dia lido do catálogo. `app/chamados/urls.py:14,21` `app/chamados/views.py:546–547` `app/chamados/services/acoes_chamado.py:70–77,109–122,154–167,228–241,285–287,322–329` `app/chamados/services/fio.py:756–772` `app/colaboradores/views.py:2107–2113` `app/chamados/catalogo/furo_modulos.py:15–16` `app/ponto/services/cartorio.py:141–157`
- **F4 — Portas humanas que chegam a gates compartilhados.** Reabrir disputa administrativa, desarquivar pela tranca e reabrir competência podem chamar `reabrir(humano=True)`. Abrir o registro de ausência para validar chama `analisar()`. Resolver dia e fechar disputa podem levar a `reconciliar()` no chamado. A confirmação da regularização externa pode levar a `encerrar()`, inclusive por retomada automática. `app/chamados/services/acoes_disputa.py:259–278,301–302` `app/chamados/views_arquivar.py:63–86` `app/chamados/services/competencia_trancada.py:260–296` `app/chamados/views.py:1489–1515` `app/ponto/services/veredito_celula.py:184–207` `app/chamados/signals.py:21–46,122–139` `app/chamados/services/regularizacao_ext.py:199–206,348–356`
- **F5 — Identidade e instante no chamado.** A transição direta passa `user` e via `admin` a `_gravar_estado`. A trilha `estado_chamado` guarda `LogAuditoria.usuario_id`, `criado_em`, objeto, antes/depois e via. `respondido_por_id` guarda o usuário quando o gate o passa. `resolvido_em` marca a saída de vivo, pode ser preservado em `resolvido → fechado` e é limpo na volta a vivo. `atualizado_em` também muda em outras escritas. As portas de reabertura citadas em F4 têm trilhas próprias, mas `reabrir(humano=True)` chega ao escritor sem passar autor ou origem do ato. A auditoria é best-effort e pode atribuir o usuário do request a um efeito automático dentro dele. **Não há um par persistido no chamado que identifique universalmente a decisão humana de origem.** `app/chamados/catalogo/motor.py:88–113` `app/chamados/models.py:58–74,234–255,298–302,459–462,509–511,859–866,1421–1425,1498–1501` `app/core/models.py:295–307` `app/core/observ.py:94–138`
- **F6 — O que alimenta `fio`.** Conta chamados de batida do dia em `VIVOS` com pelo menos uma `perguntas_que_resolvem` viva pelo C6. `fio:0` significa que há chamado vivo sem fio vivo, e a parte fica ausente quando não há chamado vivo. C6 lê validação, resposta e texto, `materializada_em` e `via_resolucao`, além de tranca, veredito/via da célula, lâmpada e lastro do marco. O fechamento adiado e o E2 mudam esses insumos. A fábrica pode criar perguntas ou desfazer carimbos após a lavra. O juiz do dia recebe vivos e fio, e a releitura interna após emissão usa a impressão para decidir se volta a julgar. `app/ponto/services/cartorio.py:543–547,1123–1141,1165–1184,1193–1223,1527–1545,1614–1618` `app/chamados/juizes.py:1382–1489,1510–1519` `app/chamados/reconciliador.py:634–645` `app/chamados/services/fabrica_da_celula.py:160–174,206–219`
- **F7 — Atos humanos no fio.** Resposta do colab pela web/API e correção de resposta pelo admin passam pelo escritor que grava `DecisaoPergunta` com `ato_por_id` e `ato_em`. Validação, devolução e recusa também têm trilha de decisão. Resolver dia e encerrar silêncio da disputa gravam `SILENCIO` com autor e instante do carimbo. A projeção `materializada_por_id`/`materializada_em`/`via_resolucao` é compartilhada. Na reabertura de questionário pela supervisão, a linha por pergunta guarda o instante com `ato_por` nulo, e o autor fica na auditoria da disputa. **Falta nessa porta a assinatura por pergunta.** `app/chamados/views.py:1276–1278` `app/api/views.py:2646–2647` `app/chamados/services/acoes_disputa.py:92–117,374–379` `app/chamados/services/disputa_emissao.py:1527–1534,1584,1617–1645` `app/chamados/services/validacao.py:161–175,237–280,915–939` `app/chamados/models.py:2080–2113,3246–3260,3293–3297,3378–3386,4166–4169` `app/ponto/services/veredito_celula.py:189–197,346–358`
- **F8 — Horizonte e relógio.** `versao_para` usa mês/ano do fim de `janela_atual(data, empresa)`, pelo corte da empresa, e devolve a maior versão com horizonte `<=` à competência, ou `None` se nenhuma alcançar. A tabela tem v1 em `2026-09` e v2–v5 em `2026-10`. `_regra_do_dia` alimenta os dois ramos de impressão. A v5 também é lida pelo contador e pelo leitor de razão ausente. O comando captura `agora`, repassa ao cartório, e C6 já aceita esse instante. `app/ponto/janelas.py:36–63` `app/ponto/services/cartorio.py:161–166,280–299,521–522,1038–1056,1110–1123,1329–1337,1399–1401,1541–1547,2202–2205` `app/escala/services/leitor_celula.py:194–222` `app/ponto/management/commands/processar_cartorio.py:40–41,63–69` `app/chamados/juizes.py:866–898`
- **F9 — Entrega e dinheiro.** O juiz registrado é `_K6`, `app/folha/export.py::competencia_entregue`, por empresa × mês × ano. A resposta é TXT vigente e não invalidado. A grade da ata alimenta previsto, abonado, realizado, retenção e `DiaPago`. DINHEIRO item 1 exige reversão anterior, exportada intacta e prova da frota no mesmo marco quando há apply admitido. `app/docs/JUIZES.md:40–42` `app/core/juizes.py:334–342` `app/folha/export.py:329–338,556–590` `app/ponto/services/fechamento.py:1046–1068,1181–1202` `CLAUDE.md:401–417`
- **F10 — Medidas fornecidas.** `insumo.out` registra oito diferenças só de impressão no P2, enumeradas no RED. `tres.out` registra três no P2 (128355, 129943, 138359) e uma no P3 (129134, `concorde → cobrado`, chamado 43141). (A) é zero no P2/P3 do dump novo. No P1, 12711, competência 08, mudou `razoes_orfas` de `null` para `[]` e manteve `furo`. Os anexos mostram diferenças nos insumos. Autor, causa por célula, relógio exato injetado e filtro/colunas dos hashes impressos [nao verificado].

## MUDA

1. **Ato humano.** Antes do código, publicar a matriz insumo × porta escritora × leitor. No horizonte, substituir o `status_local` derivado e a contagem `fio` da impressão pela identidade e pelo instante dos atos humanos de origem que afetam aquele dia. Retirar da impressão os insumos cujo único escritor é a cadeia. Conservar o gatilho de releitura quando o juiz precisa ler efeito novo da própria emissão. Onde a trilha não identifica o ato humano, vale o **PAREI de 1b**. Se a costura exigir lei ou juiz novo, PAREI no ponto e levo a pergunta ao Ronald. `app/ponto/services/cartorio.py:493–547,1193–1223,1527–1558,1614–1618` `app/chamados/models.py:298–302,4166–4169`
2. **Relógio.** Passar o `agora` do cartório a `fio_vivo` e deste a `pergunta_viva`. Preservar a chamada existente dos outros leitores quando omitem o argumento. `app/ponto/services/cartorio.py:1110–1123,1532,2202–2205` `app/chamados/juizes.py:866–898,1382–1404,1510–1519`
3. **Vigência.** Usar `versao_para` e o horizonte default **2026-10** da volta 3. Até 2026-09, conservar byte a byte a impressão anterior para os mesmos insumos e a foto gravada sem fato novo. Em fato novo, vale o caminho anterior à mudança. Manter a leitura e a lavra da razão da sobra pela v5. `app/docs/LEIS.md:145` `app/ponto/services/cartorio.py:161–166,280–299,1038–1056,1333–1337,1552–1558` `app/escala/services/leitor_celula.py:194–222`
4. **Entregue.** Consultar `_K6` pela competência do dia antes de qualquer julgamento ou escrita desta obra, inclusive efeitos da releitura e da fábrica. Preservar a janela de leitura dos turnos que atravessam a borda. Provar zero escrita em célula entregue, além do hash de FM e ED. `app/folha/export.py:556–590` `app/ponto/janelas.py:60–63` `app/ponto/services/cartorio.py:1075–1079,1152–1181,1199–1222` `app/ponto/portas/celula.py:1189–1201`
5. **FM/ED, literal proposto.** Empresas **2, 3, 4** × competências **2026-08, 2026-09, 2026-10, 2026-11**. Agosto e novembro aparecem nas células da sonda. Setembro e outubro aparecem na declaração de versões e nos anexos 1815/2325. O filtro dos hashes antigos [nao verificado]. Fotografar imediatamente antes de P1 e após P3, no mesmo ensaio, antes de recálculo financeiro. FM por empresa/ano/mês/colab/PK e ED por empresa/ano/mês/PK, com **todas as colunas persistidas**, inclusive carimbos. Em ED incluir conteúdo, `hash_sha256`, `invalidada_em`, `invalidada_motivo` e `substituida_por_id`, com todas as versões. Ordenar pelas chaves, usar a mesma serialização canônica e publicar contagem e SHA-256 por modelo × empresa × competência, inclusive recortes vazios. `app/ponto/models.py:398–485` `app/folha/models.py:75–104,143–186`
6. **Aberta.** Antes do pouso, medir main × cópia a partir do mesmo dump e relógio, por empresa × colab × competência aberta alcançada. Usar o mesmo recálculo em ambos. Comparar presença da linha, rubricas, saldo, turnos, previsto/abonado/realizado, retenção (`motivos_espelho` e classificação de exportação) e linhas `DiaPago` por data/tipo/versão, inclusive ajuste. Movimento = `PAREI` com empresa, colab, competência, campo, antes/depois e dias afetados. Em 10, manter a Pauta DP de 2325 para 191, 203, 922 e 87 e o acerto a cargo do DP, conforme anexo 2325 [nao verificado na árvore]. `CLAUDE.md:401–417` `app/ponto/models.py:315–351,412–468` `app/ponto/services/fechamento.py:1046–1068,1191–1202` `app/folha/export.py:765,1301`
7. **(A) `null → []`.** Registrar as 19 do levantamento anterior e o zero observado no P2/P3 do dump novo. Sem varredura de normalização. Manter 12711/08 como testemunha do P1 e informar sua resposta de `_K6` no ensaio. Identidade das 19 e resposta de `_K6` para 12711 [nao verificado]. `app/ponto/services/cartorio.py:962–971` `app/docs/LEIS.md:145`

## RED

- **R1 — Oito células.** `128355`, `129476`, `129490`, `129943`, `138359`, `138541`, `142995`, `143945`, todas no recorte informado de 11/2026. Entradas observadas incluem `em_analise → resolvido` com `fio:1` sumindo (128355/129943/138359), só `fio:1 → 2` (129476) e `aberto → em_analise` com `fio:1 → 2` (129490). Detalhe das outras entradas [nao verificado]. Sem ato humano nem fato externo novo, ler `CelulaDia.impressao` e `veredito` por PK após P1, P2 e P3. Esperado **8/8 estáveis em P1=P2 e P2=P3**. As diferenças da base são as do anexo `insumo.out` [nao verificado por reexecução]. `app/ponto/services/cartorio.py:1527–1558` `app/ponto/portas/celula.py:1194–1201`
- **R2 — 129134, col474, 19/11, competência 11.** Entrada do anexo `tres.out` é `enc:0`, sem chamado, depois `fio:1` e chamado 43141 com `concorde → cobrado`. Causa exata [nao verificado]. Rodar três passes com o mesmo instante em cada braço, um em curso e outro já encerrado pelo juiz do turno. Em curso, a regra conserva `concorde` sem cobrança prematura. Encerrado, com cobrança devida e fio vivo, a primeira rodada completa deve sair `cobrado`, com uma só cobrança. Esperado **impressão e veredito iguais em P1=P2=P3** em cada braço. Atravessar o encerramento num controle separado deve permitir o rejulgamento exigido pelo juiz vigente. `app/ponto/services/cartorio.py:523–532,1193–1223,1293–1297,1540–1545,1614–1624,1975–1993` `app/ponto/turnos.py:2124–2134` `app/ponto/supra_juiz.py:317–329,450–463`
- **R3 — Humano entre passes.** Fixture H1, célula passada carimbada no horizonte, chamado de batida `em_analise`, dia com premissa regularizada e sem pergunta viva que barre o fecho. Após P1, gestor autenticado faz POST na porta real `detalhe_local`, com `novo_status=resolvido`. Ler antes do POST, ao retornar e após P2. Esperado **ato assinado, impressão nova e rejulgamento no ato**, podendo P2 já encontrar o carimbo em dia. Controle H2 produz a mesma transição por `reconciliar()` sem ato humano, e o trecho da impressão reservado à origem humana fica igual. Para o fio, fixture H3 valida pela porta humana uma pergunta viva entre passes e mede a assinatura e o rejulgamento. `app/chamados/urls.py:14` `app/chamados/services/acoes_chamado.py:292–297,322–331` `app/chamados/models.py:179–205,298–302,654–668` `app/chamados/services/validacao.py:161–175` `app/chamados/signals.py:189–218`
- **R4 — Horizonte e entregue.** Fixtures `H09` e `H10` abertas e já carimbadas, com a mesma mudança exclusivamente derivada após P1. H09 mantém impressão, ata, veredito e `julgada_em` da base na ausência de fato novo. H10 usa a regra nova e passa R1. Fixture `E10` com TXT vigente e insumo suficiente para relavrar fica intacta em P1–P3 e tem **zero chamadas às portas de escrita**. Controle `I10`, mesma competência com último TXT invalidado sem substituta, entra no caminho de aberta. Para 12711, se `_K6` responder entregue, o esperado é zero toque inclusive em P1. `app/docs/LEIS.md:145` `app/folha/export.py:556–590` `app/escala/tests/test_cadeia20_volta3.py:142–158` `app/ponto/portas/celula.py:1189–1201`
- **R5 — Relógio injetado.** Fixture T1 com pergunta viva às 10:00 e validação datada às 10:01. Fixar relógio ambiente às 10:02. `fio_vivo` deve responder vivo com `agora=10:00` e morto com `agora=10:02`. As duas consultas usam a mesma pergunta. `app/chamados/juizes.py:886–898,1382–1406,1510–1519`

## SELO

- Django na pista da cópia e prova de frota na sombra. Para R1/R2, três rodadas de `processar_cartorio --apply` sem `--forcar` externo, empresas 2, 3, 4 nessa ordem em cada rodada. Usar o mesmo dump por braço, relógio local congelado em `2026-11-20T06:27:00-03:00` como literal proposto para o braço encerrado, e `2026-11-20T00:30:00-03:00` para o controle em curso de 129134. Confirmar os dois lados pelo juiz do turno. Tirar cada foto depois de E2, fechamento adiado, fábrica e releitura internos. Os instantes originais exatos das sondas [nao verificado]. `CLAUDE.md:84–102` `app/config/crons.py:616` `app/ponto/management/commands/processar_cartorio.py:27–69` `app/ponto/services/cartorio.py:1123–1223`
- Adaptar `test_cadeia20_volta3.py` para a foto anterior ao horizonte e para a invariância da lavra. Usar os testes de fio vivo/morto como base dos casos humano e automático, e as fixtures `_K6` para entrega e revogação. Mutantes a matar: recolocar `status_local` ou `fio` derivado no hash, omitir o ato humano, aplicar o formato abaixo do horizonte, furar `_K6`, suprimir a releitura de cobrança devida ou ignorar o `agora` injetado. Cada mutante cai no RED correspondente. `app/escala/tests/test_cadeia20_volta3.py:109–158` `app/ponto/tests/test_o261_cobrado_le_o_fio.py:132–164,315–354` `app/folha/tests/test_k6_competencia_entregue.py:58–85`
- **R5 de frota da casa fica como está.** Empresas 2/3/4, 10/2026, cartório e recálculo em duas rodadas, compara foto pós-rodada 1 com foto pós-rodada 2. Sua foto omite `impressao` e `julgada_em`. Os RED acima são prova adicional. `app/core/placar_estrutural.py:416–431` `bin/r5_idempotencia_frota.py:26–32,52–57,104–130`

## PROIBIDO

- Carimbo no fim do passe, apagar ato humano, criar lei ou juiz, backfill ou normalização em massa, escrita em entregue e pouso. Impacto financeiro ou toque em exportada = `PAREI` com a lista. Qualquer exceção à exportada exige o `!` aplicável fora deste rascunho. `app/docs/LEIS.md:67,109,112,145,155` `CLAUDE.md:401–417` Ficha 1 [nao verificado na árvore].
- **Colisão 1815.** Preservar o encaminhamento da razão da sobra por horizonte, o estado nomeado no leitor da ata antiga, os selos dos leitores e a revisão do escopo inteiro. **Colisão 2325.** Preservar causa por célula, identificação do dia de 08, Pauta DP de 10 e verificador antes da linha de PRONTO e do opus. A permissão de pouso do 2325 tem o escopo daquele aval. `app/docs/CORTES.md:563` `app/escala/services/leitor_celula.py:194–222` Anexos 1815 e 2325 [nao verificado na árvore quanto ao cumprimento].

## PRONTO

1. Matriz publicada com porta, campo de identidade/instante e leitor por insumo. **1b resolvido pelo Ronald** antes do código. `app/chamados/models.py:234–255,298–302,4166–4169` `app/ponto/services/cartorio.py:493–578`
2. Oito células com **0 mudança de impressão e 0 de veredito** no P2 e no P3. 129134 com os mesmos zeros por braço a relógio fixo. P1 registra total de rejulgadas e duração. Causa por célula publicada. `app/ponto/services/cartorio.py:1552–1558` `app/ponto/management/commands/processar_cartorio.py:69–74`
3. Ato humano provoca rejulgamento verificado imediatamente após o POST. Controle automático preserva a assinatura humana. Selo do relógio e todos os mutantes mortos. `app/chamados/signals.py:189–218` `app/chamados/juizes.py:886–898,1382–1406`
4. Foto anterior ao horizonte byte a byte no controle sem fato novo. **0 escrita e 0 diferença** na foto integral das células entregues. Contagem e SHA-256 de FM e ED iguais em cada recorte de MUDA 5. `app/docs/LEIS.md:145` `app/ponto/portas/celula.py:1189–1201` `app/folha/models.py:75–104`
5. Dinheiro e retenção main × cópia com **0 colab × competência divergente**, incluindo `DiaPago`. Qualquer movimento produz a lista de MUDA 6 e mantém `PAREI`. `CLAUDE.md:401–417` `app/ponto/services/fechamento.py:1046–1068,1191–1202`
6. (A) contado no P2/P3 e 12711/08 explicado em separado. Contador de atas no horizonte com sobra e sem lavra completa = 0. R5 da casa = 0. Verificador responde item a item e a sessão publica a linha de PRONTO antes do pre-opus. `app/ponto/services/cartorio.py:1038–1056` `app/core/placar_estrutural.py:416–431` `CLAUDE.md:95–103` Anexos 1815 e 2325 [nao verificado quanto à execução].

LEI-AKITA: origem=versao_para e atos das portas humanas, testemunha=CelulaDia.impressao e veredito lidos após cada passe, RED=R1–R5 deste aval, quem-mais-le=classificar_dia e releitura do cartório, grade/espelho/PDF pela ata, juizes novos=0, padrao=celula soberana  
`app/docs/GLOSSARIO.md:11` `app/ponto/services/cartorio.py:280–299,1199–1222,1614–1618` `app/escala/services/leitor_celula.py:194–222` `app/ponto/services/espelho.py:709,862` `app/relatorios/pdf_espelho.py:870–875`

## CONTRADIÇÕES DA ÁRVORE COM A FICHA

- CONTRADIZ a ficha (a): app/ponto/services/cartorio.py:502–507,1541–1547. Hoje entram o estado e a contagem derivados. Nas portas sem assinatura individual, a árvore não fornece o par completo exigido pela ficha. `app/chamados/models.py:1498–1501` `app/chamados/services/acoes_disputa.py:105–117` `app/chamados/services/disputa_emissao.py:1639–1645`
- CONTRADIZ a ficha (c): app/ponto/management/commands/processar_cartorio.py:47–69. O lote atual inclui células passadas sem `_K6` nessa seleção, e a lavra pode persistir impressão e ata. Toque efetivo em entregue nas sondas [nao verificado]. `app/ponto/services/cartorio.py:2013–2037` `app/ponto/portas/celula.py:1189–1201`
- CONTRADIZ a ficha (f): app/ponto/services/cartorio.py:1532 e app/chamados/juizes.py:1510–1519. Hoje a chamada e o juiz omitem o `agora` do cartório.

## BLOQUEIOS RESPONDIDOS

- **1b** — **SEGUE ABERTO: Ronald, qual registro de origem vale para reaberturas e outros gates compartilhados quando a trilha por chamado/pergunta não traz autor e instante inequívocos, 

[truncado em 20000 de 21555 caracteres]

## 20/11 01:33 · sessao 9097ab63bc09 · TRAVAS-SEM-MEMORIA · leitura · origem=Code

aval Ronald: TRAVAS-SEM-MEMORIA-V3 (rascunho Codex 20/11) -- unidade: hook e ESTADO; lei: nenhuma nova

## FONTE

- F0. FICHA 2, alíneas (a)–(e), rege este rascunho. Base informada `eb1e49c1ec1055e6bf43d7f67df63b17054b474e`; o diretório entregue não tem `.git`, então a identidade `HEAD = main = hash` fica [nao verificado]. Foram conferidos os arquivos citados. Carga dos hooks na VM, executor da pista e resultados de smoke [nao verificado]. `fichas_para_rascunho_2011_0130.txt:3`, `CLAUDE.md:344–348`.
- F1. `suite.sh` resolve `--volta` e julga a seleção efetiva. Seleção vazia ou que cobre a descoberta toda é inteira. Inteira sem motivo canônico sai `rc 2`. A volta intermediária junta ruff, selo do disjuntor e recorte com vizinhos e selos globais. `bin/suite.sh:156–180,215–263`, `bin/volta_intermediaria.sh:18–46`.
- F2. Régua tem runner e carimbo próprios. O Git entrega argumentos e stdin ao wrapper do pre-push, que decide se reaproveita a régua e roda `tenants` em runner separado. O pouso usa `suite.sh` pela trava da VM. `bin/regua.sh:46–51,84–101,165–169`, `bin/hooks.sh:58–65`, `bin/pre-push.sh:6–8,27–74,159–200`, `CLAUDE.md:95–106`.
- F3. A opção A registra volta e pre-opus de lateral na pista, espinha e pouso na VM. A interface da pista recebe commit em `refs/pista/<sha>`, `pede <sha> [--only <labels>] [--parallel N]` e `espera <id>`. Há registros de `rc 0`, `rc 1` e `rc 71` sem a ref. `app/docs/RELATO.md:65,201,1497,1529`, `app/docs/BACKLOG.md:2191–2195`.
- F4. `PreToolUse` recebe o comando externo em `tool_input.command`. O disjuntor atual pode sair cedo para Bash da sessão principal. `UserPromptSubmit` está ausente do settings entregue. Stop lê `transcript_path` e cobra a fila pelas portas vigentes. A abertura em `app/` já deixou hooks da raiz inertes. `.claude/settings.json:2–73`, `bin/disjuntor.py:1837–1843,1864–1867,1912–1933`, `bin/hook_stop_fila1.py:1339–1345,1381–1404`, `app/docs/RELATO.md:3341`, `app/docs/PROMPTS.md:1877`.
- F5. `raias_estado.py` cruza worktrees com declarações do escritor `raia_agente.sh`. A união inclui declaradas, à frente e só sujas. Hoje as só sujas saem por nome, há declaração sem caminho e o HEAD por cópia não sai. `gerar_estado.py` lê o medidor e `relato.sh` gera, raspa e publica o ESTADO. `bin/raias_estado.py:42–135,138–174`, `bin/raia_agente.sh:40–59`, `bin/gerar_estado.py:211–229,329–331`, `bin/relato.sh:30–38,114–119,133–147`.
- F6. O construtor Codex escreve só em `<wt>/app`, com uma vaga por repo. `.claude/` consta no ignore e há precedente de instalação local negada pelo classificador. O arquivo do RELATO guarda marcos antigos. `CLAUDE.md:349–356,394,436–442`, `bin/codex_pergunta.sh:121–135,248–259`, `.gitignore:66`, `app/docs/RELATO.md:4356,4411`.

## MUDA

- M1. A sessão constrói em cópia e instala os dois registros locais da raiz, somente para este instrumento. Codex verificador lê o diff depois da construção. Antes do código, publicar as cinco linhas abaixo no RELATO e mandá-las uma vez ao Codex. A execução depende de `aval Ronald: APROVA RASCUNHO TRAVAS-SEM-MEMORIA-V3 <sha256 12>`. `fichas_para_rascunho_2011_0130.txt:1,3`, `aval_rascunhado_pelo_codex_1911_1838.txt:5–7`, `CLAUDE.md:309,349–356,372–394`.
- M2. T1 usa a rota declarada pela sessão no despacho. Para lateral, o pedido local carrega `PISTA_LATERAL=<OBRA>` junto de `bin/suite.sh --dir <cópia> --motivo "pre-opus <OBRA>"` ou `bin/volta_intermediaria.sh <cópia> <base> <saída>`. O script da volta conserva ruff e selo e passa o recorte efetivo ao ramo remoto de `suite.sh`. Volta cujo recorte cobre tudo continua sujeita ao `rc 2` da guarda atual. Régua, pre-push, pouso, pre-opus da espinha e teste focado mantêm seus comandos. `bin/suite.sh:129–180,215–263`, `bin/volta_intermediaria.sh:18–46`, `CLAUDE.md:95–106`, `app/docs/RELATO.md:65,81`.
- M3. Registrar um `PreToolUse` de Bash para a porta canônica. Ele lê `PISTA_LATERAL`, a obra do motivo e o SHA completo da cópia. Requer o mesmo ID nos dois campos e, para a inteira pre-opus, uma linha literal no início da linha `PRONTO <OBRA> item a item: sha=<SHA completo>` em `app/docs/RELATO.md` ou `app/docs/RELATO-ARQUIVO.md`. Um registro arquivado vale para o mesmo SHA. Cabeçalho de outro SHA, de outra obra, indentado ou citado vale como ausente. Resposta `SEM PRONTO ITEM A ITEM`. O mesmo verificador de instrumento roda no executor imediatamente antes de criar a ref e pedir o job. O SSH recebe o SHA conferido e as opções documentadas. `bin/disjuntor.py:1796–1798,1837–1843`, `bin/suite.sh:219–263`, `CLAUDE.md:439–442`, `app/docs/BACKLOG.md:2191`.
- M4. A sessão publica o PRONTO com cada item, medida e pendência. O hook confere identidade da obra e SHA. Após mais de três voltas Codex, pendências registradas permitem a inteira e a subida ao opus pela regra vigente. Uma recusa de PRONTO fecha o pedido antes do SSH. Falha de infraestrutura segue a contingência registrada da rota, com `PISTA: VM porque <motivo>` quando couber. Vermelho de teste retorna o rc do job. `CLAUDE.md:372–381`, `app/docs/RELATO.md:81,1010,1209`, `app/docs/BACKLOG.md:2191`.
- M5. T2 faz `raias_estado.py` expor o nome e o HEAD completo observado por `git rev-parse HEAD` em cada worktree da união, incluindo as só sujas. O ESTADO publica um `copia=<nome>@<HEAD12>` por cópia resolvida. Raia declarada sem caminho ou sem HEAD resolvível publica `raia=<nome> SEM WORKTREE`. O hook lê o mesmo medidor na hora do envio. O ESTADO é a fotografia publicada. `bin/raias_estado.py:42–48,73–135,138–174`, `bin/gerar_estado.py:211–224`, `bin/relato.sh:37–38,114–119`.
- M6. `UserPromptSubmit` examina aval colado com o token literal `CONSTRUIR` na primeira linha. Na linha `alvos=<nome>,<nome>`, listar todas as cópias tocadas e dar, na mesma ordem, um `copia=<nome>@<prefixo>` por alvo existente ou `copia=nova` na posição de cada alvo novo. Prefixo hexadecimal de 12 caracteres até o SHA completo. Nome é o basename da worktree, com escape percentual para delimitadores. `fora=<nome>,...` declara cópias vinculadas à obra que ficam fora do aval. Toda raia declarada com a mesma `OBRA` consta em `alvos` ou `fora`. A sessão explicita em `alvos` cópias tocadas sem declaração, inclusive descendentes. `bin/raias_estado.py:73–102,125–135`, `bin/raia_agente.sh:40–59`, `app/docs/RELATO.md:1513`, `travas_sem_memoria_v2_2011_0110.txt:5`.
- M7. ALVOS com nome repetido, ambíguo, omitido, sem HEAD ou com prefixo divergente devolvem `AVAL SEM LEITURA DA COPIA (HEAD atual <hash>)` por alvo, com `indisponível` onde nenhum HEAD puder ser lido. `copia=nova` vale para um alvo sem worktree elegível nem declaração, inclusive quando há outros alvos existentes no mesmo aval. Uma declaração sem worktree exige saneamento para aquele alvo. A sessão registra `AVAL-DEVOLVIDO <ID>` no topo do RELATO pelo escritor existente, como recado que preserva a janela do Stop, e segue os itens andáveis. Ordens sem o token no cabeçalho conservam a passagem atual pelo detector e pelas guardas próprias. `bin/raias_estado.py:73–102`, `bin/hook_stop_fila1.py:413–524,1381–1404`, `app/core/esteira_vigia.py:846–892`, `CLAUDE.md:262–275,436–438`, `bin/codex_pergunta.sh:252–259`.
- M8. Fronteira T2 = primeiro evento de controle comprovado com os registros carregados na sessão de trabalho. Registrar data/hora com fuso, sessão, SHA do instrumento e digest do settings. Aval recebido antes, provado em PROMPTS/BACKLOG, segue sem releitura. Toda recolagem após o marco é novo evento de validação. Recolagem válida de título já recebido segue a resposta de duplicata da casa. `CLAUDE.md:436–438`, `.claude/settings.json:2–73`, `app/docs/RELATO.md:3341,4356`.
- Alternativa T1 em uma linha. Um `bin/pista_pede.sh pre-opus-lateral <OBRA> <cópia>` pode carregar obra e cópia no argv, usando o mesmo leitor de PRONTO e a mesma interface SSH. `app/docs/BACKLOG.md:2191,2195`.
- Alternativa T2 em uma linha. Um bloco `alvo=<nome> copia=<nome>@<hash>` por alvo explicita a mesma associação da lista ordenada, inclusive `alvo=<nome> copia=nova`. `bin/raias_estado.py:89–135`, `travas_sem_memoria_v2_2011_0110.txt:5`.

### CONTRATO-DE-INSTRUMENTO

1. Quem chama — sessão/raia pelo Bash da raiz para T1, Code ao receber o aval para T2, `relato.sh` ao publicar o ESTADO. `.claude/settings.json:62–72`, `bin/relato.sh:37–38`, M2–M7.
2. Quem espera — sessão em shell de fundo, por `ssh pista espera <id>` e veredito com `Ran` e rc. T2 responde no evento de entrada. `app/docs/BACKLOG.md:2191`, `CLAUDE.md:316–319`, M7.
3. Falha/limite — recusa só do pedido ou aval afetado, antes de enfileirar ou usar o aval. Pista recebe árvore commitada. Divergência VM/pista fica registrada e suspende o uso até Ronald ver. `app/docs/BACKLOG.md:2191`, `CLAUDE.md:262–275`, M3–M8.
4. Medida — por caso, guardar porta, obra, SHA, rc/veredito, chamadas SSH/VM e linha de recusa ou publicação. Em job aceito, guardar id e tempo. `app/docs/RELATO.md:81,1497,1529`, M3–M8.
5. Quem desliga — Ronald, restaurando os dois registros anteriores e retirando a seleção da nova rota, com linha no RELATO. `app/docs/BACKLOG.md:2191`, `app/docs/RELATO.md:81,4356`, M1–M3.

## RED

Entradas de teste: `O=LATERAL-TESTE`, `C=wt-a`, `D=wt-b`, `H/J` = HEADs reais da fixture, `H0` = SHA anterior de C, `B` = base do recorte. `P` é `PISTA_LATERAL=LATERAL-TESTE bash bin/suite.sh --dir "$C" --motivo "pre-opus LATERAL-TESTE"`. `U` é colar `aval Ronald: LATERAL-TESTE CONSTRUIR` com o corpo do caso. Antes da cura, testar as mesmas entradas e registrar qual expectativa falhou. Resultados [nao verificado]. `CLAUDE.md:28–29`, `bin/tests/test_suite_motivo.sh:14–42`.

- **Pre-opus lateral** — P1 `P`, C commitada/ref apta, sem PRONTO → `SEM PRONTO ITEM A ITEM`, zero SSH e zero runner. P2 `P`, só há `PRONTO` de `O@H0` e de outra obra em H → mesma recusa. P3 `P`, marcador em `RELATO.md` para `O@H` → um `pede H` sem `--only`, id e `espera` com `Ran/OK/rc 0`. P4 `P`, mesmo marcador só no ARQUIVO, quatro voltas Codex e pendência escrita → mesmo pedido e subida ao opus após o verde. P5 `P`, PRONTO válido e job vermelho → `rc 1`, zero runner VM. `bin/suite.sh:236–263`, `app/docs/BACKLOG.md:2191`, `CLAUDE.md:372–381,439–442`, M3–M4.
- **Volta de lateral** — V1 `PISTA_LATERAL=LATERAL-TESTE bash bin/volta_intermediaria.sh "$C" "$B" "$OUT"`, C commitada e recorte próprio, sem PRONTO → ruff/selo e um `pede H --only <labels efetivas>`, zero trava VM. V2 mesmo comando, seleção cobre todos os módulos → guarda atual `rc 2` da suite, volta vermelha e zero SSH. V3 mesmo V1, pista indisponível e contingência vigente → recorte na VM e uma linha de causa. `bin/volta_intermediaria.sh:18–47`, `bin/suite.sh:162–180,215–263`, `app/docs/RELATO.md:81,1010`, M2–M4.
- **VM e focado** — L1 `bash bin/suite.sh --dir "$ESP" --motivo "pre-opus ESPINHA-TESTE"`, cópia sem PRONTO → inteira pela VM. L2 `bash bin/suite.sh --dir "$C" --motivo "pouso LATERAL-TESTE"`, cópia sem PRONTO → inteira pela VM. L3 `bash bin/suite.sh --dir "$C" --only "$MODULO"`, módulo próprio e sem PRONTO → recorte roda. L4 `bash bin/suite.sh --dir "$C" --motivo "pre-opus"`, inteira → `rc 2` anterior. Zero SSH nos quatro casos. `bin/suite.sh:215–263,316–325`, `app/docs/RELATO.md:65`, M2.
- **Régua e Git** — R1 `bash bin/regua.sh --force`, mesmo com carimbo verde e trava livre → runner integral executa, sem `--motivo`. G1 `git push remoto-teste HEAD:refs/heads/smoke`, hook instalado, commit novo e sem carimbo compatível → arsenal CI e `tenants` executam sobre o commit, push liberado se verdes. G2 mesmo push em fixture com carimbo compatível → reaproveita arsenal e executa `tenants`. Zero SSH. `bin/regua.sh:84–101,134–169`, `bin/hooks.sh:58–65`, `bin/pre-push.sh:27–74,159–200`, M2.
- **Aval** — A1 `U` com `alvos=wt-a`, sem `copia` → devolve com H. A2 `U` com `alvos=wt-a copia=wt-a@H0`, HEAD H → devolve com H. A3 `U` com `alvos=wt-a,wt-b copia=wt-a@H12 copia=wt-b@J`, ambas da obra → entra. A4 `U` com só `wt-a`, estando `wt-b` declarada para O → devolve; com `fora=wt-b` explícito → entra para C. A5 `U` com `alvos=wt-nova copia=nova`, nome livre → entra; mesmo envio com nome já declarado sem worktree → devolve. A6 `U` com `alvos=wt-sem copia=wt-sem@H12`, declaração sem worktree → devolve com HEAD indisponível. A7 `U` com C válida e `wt-nova` livre → entra com um token por alvo. Nos negativos, uma linha de devolução por evento e o próximo item andável segue. `bin/raias_estado.py:73–135`, `CLAUDE.md:262–275`, M5–M8.
- **Corte e outras entradas** — A8 `U` inválido recepcionado antes do marco, continuar o item após ativação → continuidade; recolagem do mesmo texto → recusa do novo evento. A9 colar `aval Ronald: LEITURA-TESTE` com `CONSTRUIR` só no corpo → T2 deixa passar. A10 enviar leitura, sonda e despacho pelos seus comandos vigentes de `bin/codex_pergunta.sh`, com entradas válidas e #G ativa → chegam às guardas atuais, zero recusa T1/T2. `CLAUDE.md:344–358,436–438`, `bin/codex_pergunta.sh:241–259`, M7–M8.
- **ESTADO** — E1 `python3 bin/gerar_estado.py "$RAIZ" "$OUT"` e `DEST_RELATO="$DEST_TESTE" bash bin/relato.sh` com fixture raspada, C e D declaradas, uma só suja e uma sem worktree → três `copia=<nome>@<HEAD12>` e uma `SEM WORKTREE`, no ESTADO gerado e publicado. E2 repetir após avançar só H → muda só o token de C entre as cópias. `bin/raias_estado.py:125–174`, `bin/gerar_estado.py:211–224,398–407`, `bin/relato.sh:23,37–38,114–147`, M5.

## SELO

- `bin/tests/test_travas_sem_memoria.sh`, no host, chamado pelo glob da régua antes do Django. Adaptar `test_suite_motivo.sh` para seleção efetiva, `test_raia_declarada_no_estado.sh` para HEAD/publicação e usar o molde de fixtures e mutantes de `test_disjuntor.sh`. SSH falso registra argv e enfileiramentos, Git real temporário dá os HEADs e runners de teste registram chegada. `CLAUDE.md:127–130`, `bin/regua.sh:134–145`, `bin/tests/test_suite_motivo.sh:14–42`, `bin/tests/test_raia_declarada_no_estado.sh:43–84`, `bin/tests/test_disjuntor.sh:82–101,1532–1547`.
- Despachar payloads pelos registros lidos do settings candidato, por evento e matcher. Controle íntegro verde. Retirar só o registro T1 derruba P1 no teste do evento; retirar só o T2 derruba A2. Aceitar PRONTO de qualquer SHA derruba P2, ignorar segunda cópia derruba A4 e omitir HEAD na publicação derruba E1. A chamada direta ao script complementa a prova do executor. `.claude/settings.json:62–72`, `bin/tests/test_disjuntor.sh:82–88,1532–1547`, M3–M8.
- Smoke pré-pouso em sessão Code descartável aberta na raiz da cópia candidata, com settings locais e comandos apontando para essa cópia. Provar carga real dos dois eventos, P1/P3 e A1/A2/A3/A8, recorte V1, publicação E1 e continuidade com próximo item andável. Se a instalação for negada, registrar a negativa e levar o evento ao Ronald. Carga, instalação e smoke [nao verificado]. `app/docs/PROMPTS.md:1877`, `app/docs/RELATO.md:3341,4356,4411`, M1–M8.

## PROIBIDO

- Barrar teste focado, leitura, sonda ou despacho; mexer nas #G, exigir `--motivo` de régua ou pre-push, mover espinha/pouso para a pista, transmitir dado de produção, tocar regra de negócio ou criar juiz. Pouso próprio só com o smoke e a verificação do instrumento. `fichas_para_rascunho_2011_0130.txt:3`, `bin/codex_pergunta.sh:241–259`, `app/docs/BACKLOG.md:2191`, `app/docs/RELATO.md:65`, `CLAUDE.md:265–275,325–328,372–381`.

## PRONTO

1. M1 — contrato de cinco linhas publicado e um envio ao Codex; diff de `bin/` e configuração local conferidos pela sessão e pelo Codex verificador. `CLAUDE.md:369–394`, `.gitignore:66`. [nao verificado]
2. M2–M4 — P1–P5, V1–V3, L1–L4, R1 e G1–G2 com contagens por porta; um job real aceito com id, SHA e rc, e zero jobs para pedido sem PRONTO. `app/docs/BACKLOG.md:2191`, `bin/regua.sh:134–145`.
3. M5–M8 — A1–A10 e E1–E2 com HEADs de referência, topo do RELATO, continuidade de fila e marco de ativação registrados; ESTADO publicado bate com o medidor da mesma geração. `bin/gerar_estado.py:211–224`, `bin/relato.sh:114–147`, `CLAUDE.md:262–275,436–442`.
4. SELO — RED na base pelo motivo da regra, controle íntegro verde, os cinco mutantes vermelhos e smoke da configuração candidata provado antes do pouso próprio. `CLAUDE.md:28–29,325–328`, `bin/tests/test_disjuntor.sh:1532–1547`.

LEI-AKITA: origem=bin/suite.sh:215–263 + .claude/settings.json:62–72, testemunha=app/docs/RELATO.md:65 + app/docs/BACKLOG.md:2191 + bin/raias_estado.py:89–135, RED=P1–E2, quem-mais-le=bin/gerar_estado.py:211–224 + bin/raia_agente.sh:80–81 + bin/hook_stop_fila1.py:413–524, juizes novos=0, padrao=fonte unica (`app/docs/GLOSSARIO.md:11`)

## CONTRADIÇÕES COM A FICHA

- CONTRADIZ a ficha (b): `app/docs/BACKLOG.md:2191` conserva o literal da suite para opus na VM; `app/docs/BACKLOG.md:2195` e `app/docs/RELATO.md:65` registram a opção A para lateral. `app/docs/PROMPTS.md:2459` registra também uma ampliação condicional para espinha. Vigência dessa ampliação [nao verificado].
- Nos anexos, `abre_as_laterais_e_despacha_2011_0110.txt:2,7` põe Codex como construtor e `travas_na_frente_2011_0112.txt:3` generaliza a rota interina. Os literais na VM ficam [nao verificado]. M1–M4 seguem a FICHA 2. `fichas_para_rascunho_2011_0130.txt:3`, `bin/codex_pergunta.sh:121–135,248–259`.

## BLOQUEIOS RESPONDIDOS

- **1 — Premissas.** 1(a) F3 confirma a interface documentada; 1(b) F5/M5 fixam a fonte; 1(c) F5/M5 fixam o publicador; 1(d) F4/SELO exigem carga real; **1(e)** M2 e V1–V2 julgam seleção efetiva, com o recorte da volta e o `rc 2` da inteira coberta. `app/docs/BACKLOG.md:2191`, `bin/suite.sh:162–180,215–263`.
- **2 — Colisão.** **2(a)** M2, L2 e R1/G1–G2 preservam as três portas; 2(b) M7/A1–A2 provam recado e fila; **2(c)** M2–M4 e P4/L1–L2 preservam a subida após três voltas e a VM para espinha/pouso; 2(d) PROIBIDO, A10 e M1 mantêm #G e contrato prévio; **2(e)** M1 nomeia a sessão para construir e instalar, com verificação posterior. `CLAUDE.md:95–106,262–275,349–356,372–394`, `app/docs/RELATO.md:65,4411`.
- **3(a)** M2–M4 ligam obra e SHA ao pedido antes de enfileirar; P2–P4 distinguem obra, SHA e arquivo. `app/docs/BACKLOG.md:2191`, `CLAUDE.md:439–442`.
- **3(b)** RED enumera as portas e os sinais, inclusive volta e ESTADO; SELO fixa a sessão de smoke pré-pouso. `bin/volta_intermediaria.sh:18–46`, `bin/relato.sh:114–147`, `app/docs/RELATO.md:3341`.
- **3(c)** SELO nomeia o teste de host e exige mutação independente de cada registro. `bin/regua.sh:134–145`, `bin/tests/test_disjuntor.sh:1532–1547`.
- **3(d)** M5–M7 e A4–A7 fecham alvos, nova por alvo, raia sem worktree e alcance do token. `bin/raias_estado.py:73–135`, `bin/codex_pergunta.sh:252–259`.
- **3(e)** M8 e A8 fecham ativação, continuidade e recolagem. `CLAUDE.md:436–438`, `app/docs/RELATO.md:3341,4356`.
- **4 — Sem bloqueio na revisão.** PROIBIDO delimita instrumento e metadados. `CLAUDE.md:265–268`, `bin/relato.sh:114–119`.

## 20/11 01:21 · sessao 9305ea6475f1 · E4C-F5-CONSTROI · leitura · origem=Code

TRIAGEM C | vermelho | o comando entregue usa `ponto.tests.test_selo_teto_modal`, mas o selo está em `chamados/tests/test_selo_teto_modal.py:37` | brief, entrega (d) | PROXIMO corrigir o label para `chamados.tests.test_selo_teto_modal` e medir a C no hash `57582ee14`

- Catraca AST conferida, 10 → 0 fora da permissão nominal. Amostras em `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `chamados/management/commands/auditar_invariantes_chamados.py:179` e `chamados/services/materializacao.py:193`.
- Entraram 24 dos 25 K e a sobra de `dedup_perguntas_fato.py:75`. K40 ficou avulso, classe R. Amostras em `chamados/management/commands/backfill_decisoes_pergunta.py:534`, `chamados/services/vigia_fio.py:714`, `chamados/services/vigia_fio.py:719` e `chamados/management/commands/migrar_data_evento_dia_do_turno.py:466`.
- Os outros cinco R são K3, K19, K20, K22 e K58. `chamados/services/relavra_via.py:236`, `chamados/services/materializacao.py:2122`, `chamados/services/validacao.py:677`, `chamados/services/materializacao.py:286`, `chamados/services/validacao.py:431`.
- G segue avulso. K13 recarrega o restante após cada reabertura bem-sucedida, com custo SQL [nao verificado]. `chamados/reconciliador.py:367`, `chamados/services/disputa_emissao.py:473`, `chamados/management/commands/backfill_fio_morto_e4b.py:140`.
- O construtor preservou todos os `models.py`. O item D entrou no commit posterior `9d386d1b1`, em `chamados/models.py:2068`.
- As adaptações i, ii e v preservaram os asserts e seus operandos. A fixture da catraca ainda acusa a chamada crua de ordem 2. `chamados/tests/test_e4c_f5_selo_chamada.py:199`, `chamados/tests/test_bug_par_sem_banco.py:197`, `chamados/tests/test_bug_par_c2_sem_banco.py:32`, `chamados/tests/test_bug_par_c2_sem_banco.py:86`, `chamados/tests/test_estado_aplicacao.py:55`.
- A lista `RESTAM` passou a vazia conforme o brief. Os valores esperados e os tetos do modal e dos quatro cenários de PASSAGEM foram preservados. RED/verde, mordida ORM e contagens SQL da C [nao verificado]. `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `chamados/tests/test_selo_teto_modal.py:184`, `ponto/tests/test_selo_chokepoint_escrita.py:196`, `ponto/tests/test_selo_chokepoint_escrita.py:210`, `ponto/tests/test_selo_chokepoint_escrita.py:228`, `ponto/tests/test_selo_chokepoint_escrita.py:235`.

## 20/11 01:12 · sessao d4e67d9bba0e · CADEIA-20-PASSO-1 · verificador · origem=Code

# AVAL-DEVOLVIDO — CADEIA-20-IMPRESSAO-SEM-REALIMENTACAO-2011-0110

O `PAREI` do item 2 do aval já se aplica. `status_local` recebe a mesma transição pela porta humana e pela cadeia. A contagem `fio` também pode mudar por atos dos dois lados. `app/chamados/catalogo/motor.py:98–113`, `app/chamados/models.py:459–462,654–668`, `app/ponto/services/cartorio.py:1123–1141,1527–1545`, `app/ponto/services/veredito_celula.py:184–197`.

Identidade Git da cópia com `a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6` [nao verificado].

## 1. Premissas

- **1a — NAO PROCEDE / NAO BLOQUEIA.** `502–507` monta a base da impressão. `543–547` explica e acrescenta `fio`. A lista completa é **batidas** (`pk`, `tipo`, `timestamp`); **status da ausência que cobre o dia**; **chamados de batida** (`pk`, `status_local`); **DNA** (`dna_versao`, `marcos`); e os condicionais **`veto`, `teto`, `regra`, `enc`, `fio`, `cobparcial`, `isento`, `sem_adesao`**. `cobparcial` pode incluir período e índices de marcos dispensados. O resultado é SHA-256, gravado em `CelulaDia.impressao`. `app/ponto/services/cartorio.py:141–157,477–490,493–578,1496–1501`, `app/ponto/portas/celula.py:1189–1201`.
- **1b — PROCEDE / BLOQUEIA.** O código permite rastrear as portas e suas dependências. A divisão binária do aval falha em dois insumos. **`ChamadoColaborador.status_local`** é misto até na transição `em_analise → resolvido`. A porta humana e `reconciliar()` chegam ao mesmo escritor, e o fechamento adiado chama `reconciliar()`. **`fio`** é uma derivação mista. Conta chamados vivos com pelo menos uma pergunta viva. `fio:0` indica chamados vivos sem fio e a parte é omitida quando não há chamados vivos. Entre os insumos do fio, **`PerguntaDisputa.materializada_em` e `via_resolucao`** mudam tanto no fechamento pelo admin quanto no E2. A fábrica também pode desfazer carimbos ou criar perguntas após a lavra. Qual ato causou cada uma das oito diferenças [nao verificado]. `app/chamados/services/acoes_chamado.py:322–325`, `app/chamados/catalogo/motor.py:98–113`, `app/chamados/models.py:234–235,459–462,654–668,3246–3260,3320–3325,3378–3386`, `app/chamados/reconciliador.py:634–645`, `app/chamados/services/fabrica_da_celula.py:160–174,206–219`, `app/ponto/services/cartorio.py:543–547,1123–1141,1179–1181,1527–1545`.
- **1c — NAO PROCEDE / NAO BLOQUEIA.** A chamada omite `agora` e `fio_vivo` hoje só aceita `perguntas`. O lote captura `agora` na entrada e o passa a `julgar_colab`. O corpo recebe o mesmo valor, também disponível em `agora_deste_julgamento()`. O avulso aceita `agora` ou o captura na entrada. `pergunta_viva` já aceita `agora`, deriva `hoje` dele e projeta os carimbos naquele instante. `app/ponto/management/commands/processar_cartorio.py:40–41,63–69`, `app/ponto/services/cartorio.py:101–106,1110–1123,1264–1265,1532,2202–2204`, `app/chamados/juizes.py:866–898,1382–1404,1510–1519`.
- **1d — PROCEDE / BLOQUEIA.** R5 compara a foto após a primeira rodada com a foto após a segunda. O instrumento roda cartório sem `--forcar` e recálculo na competência 10/2026, empresas 2, 3 e 4. A foto da célula inclui ata e veredito, mas omite `impressao` e `julgada_em`. Uma troca só de carimbo pode dar R5 zero. O RED precisa comparar `CelulaDia.impressao` diretamente. `app/core/placar_estrutural.py:416–431`, `bin/r5_idempotencia_frota.py:26–32,52–57,104–130`, `app/ponto/portas/celula.py:1189–1201`.

## 2. Colisão com o aval vigente

- **2a — NAO PROCEDE / NAO BLOQUEIA.** Pela ordem informada no pedido, o RED é compatível com o aval vigente se entrar como prova adicional. A causa por célula e a revisão do verificador continuam antes de `PRONTO`, seguido de opus e pouso. O literal integral do aval 2325 e o cumprimento dessa ordem [nao verificado]. [nao verificado]
- **2b — PROCEDE / BLOQUEIA.** Falta delimitar a vigência da mudança. A retirada incondicional de uma parte muda o hash de células já carimbadas e pode provocar relavra pelo simples pouso. L-124 preserva a foto anterior ao horizonte até fato novo. `versao_para` seleciona a regra por competência e a proteção depende de preservar os bytes da impressão antiga fora do horizonte. A soberania do DNA segue pela porta da célula. `app/docs/LEIS.md:71,145`, `app/docs/PLANO-PADROES.md:70–76`, `app/ponto/services/cartorio.py:280–299,502–507,543–547,1541–1558`.
- **2c — NAO PROCEDE / NAO BLOQUEIA.** O desenho pode manter o mesmo juiz e a mesma porta. O cartório consulta `classificar_dia` e lavra pela porta da célula. `pergunta_viva` e `fio_vivo` continuam como autoridades registradas. A retirada de um insumo do hash ainda precisa preservar o gatilho de releitura, porque o juiz segue recebendo vivos e fio, e a releitura interna sem `forcar` depende da impressão. `app/docs/JUIZES.md:10–15,123–134`, `app/core/juizes.py:289–299`, `app/ponto/services/cartorio.py:1–11,1202–1223,1614–1618,2013–2037`.

## 3. Literal

- **3a — NAO PROCEDE / NAO BLOQUEIA.** O caso humano é montável pela rota real. Use célula passada já carimbada no ramo normal e chamado de batida `em_analise`, com dia identificável e sem pergunta viva que impeça a resolução. Entre os passes, o gestor envia `novo_status=resolvido`. O `post_save` tenta julgar na hora. O teste deve capturar a impressão antes e logo após o ato humano, pois o segundo lote já pode encontrar o carimbo em dia. Execução do teste [nao verificado]. `app/chamados/urls.py:14`, `app/chamados/views.py:546–547`, `app/chamados/services/acoes_chamado.py:292–297,322–331`, `app/chamados/models.py:134–177`, `app/chamados/signals.py:189–218`, `app/ponto/services/cartorio.py:1552–1558`.
- **3b — PROCEDE / BLOQUEIA.** O literal de `8 → 0` aponta para as oito de (B). O anexo nomeia seis e deixa `+2` [nao verificado]. Para fechar o RED, faltam os oito IDs e a comparação da impressão após as rodadas 1 e 2, com o relógio e a sequência de etapas definidos. O zero de frota do R5 tem universo próprio. O resultado do terceiro passe das 19 de (A) [nao verificado]. `app/core/placar_estrutural.py:416–431`, `bin/r5_idempotencia_frota.py:26–27,104–130`, `app/ponto/management/commands/processar_cartorio.py:40–41`.
- **3c — PROCEDE / BLOQUEIA.** **FM** é `ponto.FechamentoMensal` e **ED** é `folha.ExportacaoDominio`. A sigla é identificável. O literal de aceite ainda precisa fixar empresas, competências, linhas, campos e o instante das duas fotos. Há um hash de FM por colaborador e competência na porta de regeneração. Em ED, `hash_sha256` cobre o TXT e pode ficar igual mesmo quando a linha tem sua vigência alterada. `app/ponto/portas/regen_exportada.py:56–74`, `app/ponto/management/commands/lavrar_status_competencia.py:10–14`, `app/folha/models.py:75–96,143–169,174–186`.

## 4. Exportada, dinheiro e `!`

- **4a — PROCEDE / BLOQUEIA.** Em competência **aberta**, mudar quando a célula é pulada ou relavrada pode alterar a ata que o próximo recálculo lê. O fechamento usa a grade da ata para previsto, abono, realizado e retenção, e a mesma derivação alimenta `DiaPago`. Isso abre um caminho indireto para mexer no dinheiro. O delta efetivo [nao verificado]. `app/ponto/services/cartorio.py:1552–1558,2013–2037`, `app/folha/export.py:329–338`, `app/escala/services/leitor_celula.py:409–432`, `app/ponto/services/fechamento.py:1013–1014,1046–1068,1191–1200`.
- **4b — PROCEDE / BLOQUEIA.** Em competência **exportada**, o lote seleciona células passadas sem filtro de entrega, e a porta de carimbo pode persistir impressão e ata. Portanto, FM e ED iguais ainda permitem toque em célula exportada. Um recálculo posterior pode escrever no gravado pelos caminhos admitidos pela L-113. O aval precisa delimitar a exclusão da exportada pelo juiz de entrega e provar a invariância; se houver movimento de dinheiro, vale o regime de `!` aplicável. Toque efetivo ou delta [nao verificado]. `app/ponto/management/commands/processar_cartorio.py:47–69`, `app/ponto/portas/celula.py:1189–1201`, `app/folha/export.py:556–574`, `app/ponto/services/fechamento.py:525–565`, `app/docs/LEIS.md:67,109,112,155`.

**O que falta no aval por bloqueio**

- **1b** — Decidir o tratamento de `status_local` e `fio`, ambos mistos. `app/chamados/catalogo/motor.py:98–113`, `app/ponto/services/cartorio.py:1123–1141,1527–1545`.
- **1d** — Exigir a comparação direta de `CelulaDia.impressao`. `bin/r5_idempotencia_frota.py:52–57`.
- **2b** — Fixar o horizonte e preservar a impressão anterior fora dele. `app/docs/LEIS.md:145`, `app/ponto/services/cartorio.py:280–299`.
- **3b** — Nomear as oito células e fixar relógio, etapas e universo de cada prova. `app/core/placar_estrutural.py:416–431`, `app/ponto/management/commands/processar_cartorio.py:40–41`. Dois IDs [nao verificado].
- **3c** — Fixar o escopo e os campos das fotos FM e ED. `app/ponto/portas/regen_exportada.py:56–74`, `app/folha/models.py:75–96,143–169`.
- **4a** — Exigir diff de dinheiro e retenção na aberta, incluindo `DiaPago`. `app/ponto/services/fechamento.py:1046–1068,1191–1200`.
- **4b** — Exigir prova de que a exportada fica intacta e explicitar o `!` se houver impacto financeiro. `app/ponto/management/commands/processar_cartorio.py:47–69`, `app/docs/LEIS.md:67,109,112,155`.

`VEREDITO: BLOQUEIA=7 NAO_BLOQUEIA=5`

## 20/11 01:12 · sessao 98f2e59db7cb · TRAVAS-SEM-MEMORIA · verificador · origem=Code

# AVAL-DEVOLVIDO — TRAVAS-SEM-MEMORIA-V2-2011-0110

Revisão só de leitura. As referências aos anexos 1 e 4 contam as linhas a partir de `aval Ronald`. O hash da cópia, a configuração carregada na VM e os resultados de smoke no ar ficam **[nao verificado]**.

## 1. Premissas — PROCEDE · BLOQUEIA

- **1(a) Pista remota — NAO PROCEDE · NAO BLOQUEIA.** A **PISTA-2** está documentada no servidor do GLPI, com fila própria. A sessão registra o commit em `refs/pista/<sha>` e pede `ssh pista pede <sha> [--only <labels>] [--parallel N]`. Sem `--only`, o registro mostra uma inteira do commit `a0bf6e78e`, `Ran 15875`, `OK`, `rc 0`. Quem devolve o rc da suíte é `ssh pista espera <id>`. Há registro de `rc 1` em teste vermelho e de `rc 71` numa tentativa sem a ref. O código do executor e sua disponibilidade atual ficam **[nao verificado]**. `app/docs/BACKLOG.md:2191–2192`, `app/docs/RELATO.md:201,1497,1529`.
- **1(b) Fonte das cópias — NAO PROCEDE · NAO BLOQUEIA.** `bin/raias_estado.py` cruza `git worktree list --porcelain` com `logs/raias/<raia>.agente`, escrito por `bin/raia_agente.sh`. Inclui as declaradas, as à frente de `main` e as sujas. Exclui a árvore principal e as worktrees do harness. As só sujas saem numa lista de nomes. Uma declaração pode sair sem caminho no disco. A lista efetiva da VM fica **[nao verificado]**. `bin/raias_estado.py:51–69,73–135`, `bin/raia_agente.sh:40–59`.
- **1(c) Encaixe no ESTADO — NAO PROCEDE · NAO BLOQUEIA.** `bin/relato.sh` gera, raspa e publica `ESTADO.md`. A seção **Raias** já usa `ler()` e `linha()` de `bin/raias_estado.py`, onde cabe acrescentar o `copia=<nome>@<HEAD12>`. O retorno atual traz caminho e branch, mas ainda não publica o HEAD por cópia. `bin/relato.sh:37–38,114–119,133–147`, `bin/gerar_estado.py:211–229,329–331,378–380`, `bin/raias_estado.py:125–160`.
- **1(d) Eventos — NAO PROCEDE · NAO BLOQUEIA.** T1 tem o encaixe de `PreToolUse` em `.claude/settings.json`, que chama `disjuntor.py pretool`. O script lê JSON de stdin e usa `tool_input.command` para `Bash`. O `tool_input.prompt` ali é da ferramenta `Agent`. Para T2, o encaixe previsto é `hooks.UserPromptSubmit` no settings do Code, ausente no arquivo da cópia. O Stop usa `transcript_path` do payload. O RELATO registra um caso em que abrir em `app/` deixou os hooks da raiz inertes e a resposta mandou abrir na raiz no próximo restart. Suporte efetivo de T2 e carga atual dos hooks ficam **[nao verificado]**. `.claude/settings.json:2–73`, `bin/disjuntor.py:1837–1843,1864–1867,1912–1923`, `bin/hook_stop_fila1.py:1339–1345`, `app/docs/RELATO.md:3341`, `app/docs/PROMPTS.md:1877`.
- **1(e) Inteira versus recorte — PROCEDE · BLOQUEIA.** A guarda atual julga a **seleção efetiva**. Uma seleção vazia ou que cubra todos os módulos reconhecidos é inteira, mesmo com módulos em `--only`. `--volta <base>` calcula a seleção depois de entrar no script. A volta intermediária roda ruff, o selo do disjuntor e o recorte de obra, vizinhos e selos globais. Uma leitura superficial de “sem módulos nomeados” barraria `bin/volta_intermediaria.sh` ou `suite.sh --volta` mesmo quando o recorte é efetivo. Já `suite.sh --dir <cópia> --motivo "pre-opus <OBRA>"` e a variante `pouso <OBRA>` são portas válidas hoje e ficam barradas pelo literal da V2 quando rodam localmente. Com `--motivo "pre-opus"` sem obra, a inteira **já sai `rc 2`**. A régua usa `${ONLY:-$LABELS}` e o pre-push usa as `LABELS` no arsenal e `tenants` em runner próprio. `travas_sem_memoria_v2_2011_0110.txt:3–4,8`, `bin/suite.sh:162–180,215–263,316–325`, `bin/volta_intermediaria.sh:18–33`, `bin/regua.sh:47–51,165–169`, `bin/pre-push.sh:184–199`.

## 2. Colisão com aval ou regra vigente — PROCEDE · BLOQUEIA

- **2(a) §3, INTEIRA SO COM MOTIVO — PROCEDE · BLOQUEIA.** A V2 nomeia régua e pre-push, mas exige também `--motivo` para a inteira local. A régua recebe `--force` ou `--only` no fluxo atual. O pre-push é chamado pelo Git com os argumentos e o stdin do push. Nenhum dos dois exige `--motivo` hoje. O §3 também admite a inteira canônica de **pouso** por `suite.sh`, que ficou fora da exceção da V2. `travas_sem_memoria_v2_2011_0110.txt:3,6`, `CLAUDE.md:95–106`, `bin/regua.sh:46–51,84–94`, `bin/hooks.sh:58–65`, `bin/pre-push.sh:6–8,27–39`, `app/docs/RELATO.md:205`.
- **2(b) §7b, PARADA 1 — NAO PROCEDE · NAO BLOQUEIA.** A V2 manda devolver só o aval inválido, registrar `AVAL-DEVOLVIDO` e seguir a fila andável. Isso é compatível com PARADA 1. O Stop cobra a fila e só trata `PAREI` como saída ordinária quando há `!` no item em curso e nenhum outro item andável. Há também a saída de `ESPERANDO` provado. A entrega do recado de T2 à sessão precisa ser provada no smoke. `travas_sem_memoria_v2_2011_0110.txt:5,10`, `CLAUDE.md:262–275,296–298`, `bin/hook_stop_fila1.py:1232–1245,1260–1276,1381–1404`.
- **2(c) CODEX 5(c), SUITE-ANTES-DO-OPUS — PROCEDE · BLOQUEIA.** A regra exige a inteira verde da árvore da obra antes do opus. A rota aprovada manda à PISTA-2 a volta e a pre-opus **de lateral**, mantendo **pre-opus da espinha e pouso na VM**. O contrato da pista é de pré-filtro e há registro separado da certificação ainda pendente. A V2 generaliza a rota remota e pode fechar também a subida ao opus após mais de três voltas Codex se o `PRONTO` depender de aprovação que essa subida dispensa. O RELATO registra a retirada de uma trava anterior por esse último motivo. `travas_sem_memoria_v2_2011_0110.txt:3–4`, `CLAUDE.md:372–381`, `app/docs/BACKLOG.md:857–858,2191,2194–2195`, `app/docs/RELATO.md:65,1209`.
- **2(d) Travas `#G` — NAO PROCEDE · NAO BLOQUEIA.** A V2 proíbe mexer nelas e preserva leitura, sonda e despacho. O wrapper continua sendo a porta do censo, da vaga e da validação da worktree. A exigência de contrato de cinco linhas no RELATO antes do código e envio único ao Codex continua aplicável. `travas_sem_memoria_v2_2011_0110.txt:2,8`, `CLAUDE.md:344–358,394`, `bin/codex_pergunta.sh:241–259`.
- **2(e) ABRE-AS-LATERAIS-E-DESPACHA — PROCEDE · BLOQUEIA.** A posição depois de LASTRO-REDESENHO e COMP-PAGA é compatível com a ordem declarada. O conflito é de executor. O anexo 4 diz `Construtor = Codex` e reserva opus novo para o final do pouso. O construtor Codex só escreve em `<wt>/app` e o wrapper proíbe `bin/` e `.claude/`, justamente os locais do instrumento. O teto de quatro também não abre uma segunda vaga de construtor Codex no mesmo repo, pois o wrapper mantém um `flock` não bloqueante. A ocupação e o cruzamento real de arquivos ficam **[nao verificado]**. `abre_as_laterais_e_despacha_2011_0110.txt:1–2,6–7`, `travas_sem_memoria_v2_2011_0110.txt:1–5,8`, `CLAUDE.md:349–356`, `bin/codex_pergunta.sh:121–135,248–254`.

## 3. Literal — PROCEDE · BLOQUEIA

- **3(a) T1, ponto e alcance da guarda — PROCEDE · BLOQUEIA.** `PreToolUse` vê a chamada externa enviada à ferramenta. Um `git push` chega como `git push`, e o pre-push roda como filho do Git. Em `ssh pista pede <sha>`, a interface documentada recebe SHA e opções de teste, sem obra nem motivo. O marcador `PRONTO <OBRA> item a item:` agora tem forma canônica e pode ser lido no `RELATO-ARQUIVO.md`. Ainda falta dizer onde a guarda vai relacionar a obra ao pedido remoto e se um `PRONTO` arquivado de volta anterior vale para o SHA atual. `travas_sem_memoria_v2_2011_0110.txt:3–4`, `bin/disjuntor.py:1837–1843`, `bin/hooks.sh:58–65`, `app/docs/BACKLOG.md:2191`, `CLAUDE.md:439–442`.
- **3(b) Oito casos e RED — PROCEDE · BLOQUEIA.** **(a)** precisa de chamada local com motivo válido para distinguir a trava nova do `rc 2` existente. **(b)** precisa exercitar as duas portas, incluindo o arsenal do pre-push e uma régua que realmente execute, pois ambos têm caminhos de reaproveitamento. **(c–d)** precisam do mesmo pedido remoto válido, variando o `PRONTO`, com prova de recusa antes de enfileirar e de job aceito no positivo. **(e–h)** descrevem quatro resultados testáveis em sessão com T2 ativo, incluindo a continuidade da fila após **(e–f)**. Os oito casos deixam sem prova direta a volta intermediária livre e a publicação de `copia=…` no ESTADO. A V2 também deixa em aberto como ativar os hooks para o smoke exigido **antes** de autorizar o pouso. Os resultados pré-cura e no ar ficam **[nao verificado]**. `travas_sem_memoria_v2_2011_0110.txt:5–8`, `bin/suite.sh:255–263`, `bin/regua.sh:84–94`, `bin/pre-push.sh:43–74,159–188`, `app/docs/BACKLOG.md:2191`, `app/docs/RELATO.md:3341,4356,4411`.
- **3(c) Selo e mutantes — PROCEDE · BLOQUEIA.** Cabe como **selo de host da régua**. Já há teste que injeta payloads no hook e mata mutantes numa cópia temporária com controle sem mutação. A `suite.sh` roda o runner Django, cujo container não monta `bin/` nem `.git`. O literal precisa dizer onde o selo roda e provar que remover o registro de T1 ou T2 faz falhar a recusa correspondente. Invocar o script diretamente pode passar com o registro ausente no settings. `travas_sem_memoria_v2_2011_0110.txt:7`, `CLAUDE.md:127–130`, `bin/regua.sh:134–145`, `bin/suite.sh:316–325`, `bin/tests/test_disjuntor.sh:80–88,1532–1547`.
- **3(d) T2, identidade da cópia — PROCEDE · BLOQUEIA.** Comparar por prefixo resolve a diferença entre HEAD12 e SHA completo. Falta definir como o texto identifica **todas** as cópias tocadas para detectar uma omitida, o que fazer com uma raia sem worktree resolvível e quando `copia=nova` vale por alvo. Uma obra já aparece no RELATO com duas cópias descendentes. O próprio anexo 4 manda abrir vagas sem `CONSTRUIR` na primeira linha, portanto uma ordem futura nesse formato ficaria fora do detector literal. `travas_sem_memoria_v2_2011_0110.txt:5–6`, `abre_as_laterais_e_despacha_2011_0110.txt:1–2`, `bin/raias_estado.py:89–102,125–135`, `bin/codex_pergunta.sh:252–259`, `app/docs/RELATO.md:1513`.
- **3(e) T2, corte temporal — PROCEDE · BLOQUEIA.** É mensurável se o aval fixar a fronteira observável. Aplicar só aos novos eventos após ativação permite provar um aval recebido antes, sua continuidade após ativar e a recusa de um novo envio inválido. Se **recolar** o mesmo aval também preservar a dispensa, é preciso identificar o recebimento anterior. A regra atual registra recebimento e título repetido, mas o anexo não amarra essa trilha ao pouso de T2. `travas_sem_memoria_v2_2011_0110.txt:5–7`, `CLAUDE.md:436–438`, `.claude/settings.json:2–73`.

## 4. Exportada, dinheiro ou `!` fora do declarado — NAO PROCEDE · NAO BLOQUEIA

O literal autoriza hooks e publicação de metadados no ESTADO. Não pede escrita em competência exportada, apply de dinheiro, alteração do motor v2 ou atalho de teste. O publicador atual consulta a lista de nomes para a raspagem. Instalar a configuração do Code faz parte do escopo declarado e precisa do executor resolvido em 2(e). A permissão efetiva de instalação na VM fica **[nao verificado]**. `travas_sem_memoria_v2_2011_0110.txt:1–8`, `CLAUDE.md:141–142,265–268,344–355`, `bin/relato.sh:30–38,114–119`, `bin/relato_nomes.sh:8–19`, `app/docs/RELATO.md:4356,4411`.

**Falta no aval, por item bloqueante**

- **1(e)** Adotar a seleção efetiva da guarda atual e declarar a passagem da volta intermediária quando ela é recorte. `bin/suite.sh:162–180,222–263`.
- **2(a)** Preservar as entradas atuais da régua e do pre-push e explicitar a porta local do pouso. `CLAUDE.md:95–106`, `bin/hooks.sh:58–65`.
- **2(c)** Manter a certificação da espinha e do pouso na VM e preservar a subida após três voltas Codex. `app/docs/RELATO.md:65,1209`, `CLAUDE.md:372–381`.
- **2(e)** Nomear quem constrói e instala o instrumento sob o anexo 4 com as `#G` vigentes. `abre_as_laterais_e_despacha_2011_0110.txt:2,7`, `bin/codex_pergunta.sh:121–135,248–254`.
- **3(a)** Fixar a porta que associa o pedido remoto à obra e a validade do `PRONTO` arquivado. `app/docs/BACKLOG.md:2191`, `CLAUDE.md:439–442`.
- **3(b)** Fixar a ativação pré-pouso e os sinais observáveis de RED, smoke, volta livre e ESTADO publicado. `travas_sem_memoria_v2_2011_0110.txt:5–8`, `app/docs/RELATO.md:3341,4356`.
- **3(c)** Nomear o selo de host e exigir falha ao retirar cada registro de hook. `bin/regua.sh:134–145`, `bin/tests/test_disjuntor.sh:80–88,1532–1547`.
- **3(d)** Definir a associação obra–cópias, o alcance de `copia=nova` e as ordens de construção sem o token no cabeçalho. `travas_sem_memoria_v2_2011_0110.txt:5`, `abre_as_laterais_e_despacha_2011_0110.txt:1–2`, `bin/raias_estado.py:89–135`.
- **3(e)** Fixar o marco observável de ativação e dizer se recolagem conta como novo envio. `travas_sem_memoria_v2_2011_0110.txt:5`, `CLAUDE.md:436–438`.

VEREDITO: BLOQUEIA=3 NAO_BLOQUEIA=1

## 20/11 01:11 · sessao 5b1249fc8197 · ABRE-AS-LATERAIS · leitura · origem=Code

QUARENTENA | andável NÃO | perguntas de lei abertas: 1 identificada, como semear o início da isenção sem tirar #3057 do corte dos 8, com 15 ou 16 janelas. Um consolidado registrou 3, mas o enunciado e o estado das outras duas ficam [nao verificado]. [RELATO.md:2085](/home/ronald/saas-hasner/app/docs/RELATO.md:2085) [RELATO.md:1801](/home/ronald/saas-hasner/app/docs/RELATO.md:1801) | aval `QUARENTENA-REABRE-COM-CENSO` já veio, `E3-NA-VAGA` suspendeu a execução e o corte sobre as janelas segue pendente. [PROMPTS.md:2168](/home/ronald/saas-hasner/app/docs/PROMPTS.md:2168) [RELATO.md:2084](/home/ronald/saas-hasner/app/docs/RELATO.md:2084) [RELATO.md:2085](/home/ronald/saas-hasner/app/docs/RELATO.md:2085) | após o corte, abrir cópia nova do main, rodar o RED preservado em `c9a2a3846`, reaplicar a especificação de Quarentena+SVV pela matriz da porta e provar o DIFF de frota. [BACKLOG.md:2781](/home/ronald/saas-hasner/app/docs/BACKLOG.md:2781) [RELATO.md:2139](/home/ronald/saas-hasner/app/docs/RELATO.md:2139) | o WIP toca só [`test_silencio_vencido_volta.py:49`](/home/ronald/wt-quarentena-svv-151703/app/chamados/tests/test_silencio_vencido_volta.py:49). Lista final da reaplicação [nao verificado].

FERIADO-POR-POSTO | andável SIM | perguntas de lei abertas: 0 registradas. Fonte única, R7, C2 e A3 têm resposta. A5 segue como proposta de tela com três perguntas, cujos enunciados ficam [nao verificado]. [RELATO.md:149](/home/ronald/saas-hasner/app/docs/RELATO.md:149) [RELATO.md:76](/home/ronald/saas-hasner/app/docs/RELATO.md:76) [RELATO.md:77](/home/ronald/saas-hasner/app/docs/RELATO.md:77) [RELATO.md:78](/home/ronald/saas-hasner/app/docs/RELATO.md:78) [RELATO.md:44](/home/ronald/saas-hasner/app/docs/RELATO.md:44) | `FERIADO-RESPOSTAS-1911-1530` já veio. Falta resposta à `FERIADO-A5-TELA`. [PROMPTS.md:2654](/home/ronald/saas-hasner/app/docs/PROMPTS.md:2654) [RELATO.md:44](/home/ronald/saas-hasner/app/docs/RELATO.md:44) | retomar de `c853e8f2b`, fechar a prévia A5 nos dois sentidos após a resposta e comprovar `sombra_c2b=0` antes de certificar o pouso. [RELATO.md:39](/home/ronald/saas-hasner/app/docs/RELATO.md:39) [RELATO.md:57](/home/ronald/saas-hasner/app/docs/RELATO.md:57) [teste do item 4:519](/home/ronald/wt-feriado-posto-135549/app/colaboradores/tests/test_feriado_por_posto.py:519) | a prévia referencia a porta em [`feriado_do_posto.py:268`](/home/ronald/wt-feriado-posto-135549/app/colaboradores/services/feriado_do_posto.py:268). Arquivos a editar para A5 [nao verificado].

TRIAGEM-OFFLINE-RED-HEADLESS | andável SIM | perguntas de lei abertas: 0 registradas. A classe C1 já está identificada e o RED reproduziu clique e reload offline nas duas cascas. [BACKLOG.md:2806](/home/ronald/saas-hasner/app/docs/BACKLOG.md:2806) [RELATO.md:2207](/home/ronald/saas-hasner/app/docs/RELATO.md:2207) | `TRIAGEM-REVERTE-JA` e `TRIAGEM-REVERTE-JA-COMPLEMENTO` já vieram. `RESCALDO-CORTE-1511` deixou a cura na fila de construção quando abrir vaga. [PROMPTS.md:2146](/home/ronald/saas-hasner/app/docs/PROMPTS.md:2146) [PROMPTS.md:2148](/home/ronald/saas-hasner/app/docs/PROMPTS.md:2148) [RELATO.md:4102](/home/ronald/saas-hasner/app/docs/RELATO.md:4102) | partir de `d680518b0`, curar a navegação offline, passar o teste headless e colher o smoke do Ronald nas duas cascas. [RELATO.md:2207](/home/ronald/saas-hasner/app/docs/RELATO.md:2207) | cura em [`app/static/sw.js:46`](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/static/sw.js:46), com prova em [`test_sw_offline_headless_red.py:459`](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:459).

**Ordem no recorte das três candidatas do aval 01:10** — vaga 3 FERIADO-POR-POSTO, vaga 4 TRIAGEM-OFFLINE-RED-HEADLESS. O aval posterior `TRAVAS-NA-FRENTE-2011-0112` destina a vaga 3 a TRAVAS-V2 após revisão sem bloqueio e reserva a vaga 4 ao LASTRO-REDESENHO até liberar os arquivos. [RELATO.md:8](/home/ronald/saas-hasner/app/docs/RELATO.md:8) [PROMPTS.md:2671](/home/ronald/saas-hasner/app/docs/PROMPTS.md:2671) [RELATO.md:2](/home/ronald/saas-hasner/app/docs/RELATO.md:2)

## 20/11 01:08 · sessao c5737160597a · E4C-F5-CONSTROI · construtor · origem=Code

## E4C-F5-CONSTROI · Volta C

A catraca AST mediu **10 → 0** chamadas cruas fora da permissão nominal. Foram convertidos os dez chamadores e carregados em lote **24 K + a sobra do dedup**. Restam **6 K por item**, todos classe R. A checagem local compilou os 19 arquivos em memória, conferiu as âncoras dos mutantes e passou em `git diff --check`. RED na base, verde na C, contagens SQL e suíte estão [nao verificado]. A certificação fica com o Code e o Opus. `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `:183`, `chamados/tests/test_e4c_f5_voltaC_lotes.py:74`, `chamados/tests/e4c_f5_voltaC_mutantes.py:12`.

## Chamadores diretos

Todos passam de ORM cru para `DecisaoPergunta.composta(...)`. O instante vigente vem da sequência com o passivo, e o texto vem de `p.resposta_colab`. A tabela lista as dez identidades que saíram da catraca. A lista final de `RESTAM` está vazia. A única permissão é `creditos_perguntas`, que entrega a tupla do `values_list`. `chamados/models.py:4317`, `:4351`, `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `:23`, `chamados/services/raiox.py:135`, `:147`.

| Sítio | Base → C | Carga |
|---|---|---|
| Auditoria `_contraprova_i21a` | `chamados/management/commands/auditar_invariantes_chamados.py:178 → :179` | K5 |
| Auditoria I2 | `chamados/management/commands/auditar_invariantes_chamados.py:597 → :598` | K6 |
| Auditoria F1 | `chamados/management/commands/auditar_invariantes_chamados.py:1671 → :1672` | K7 |
| Dedup `Command.handle` | `chamados/management/commands/dedup_perguntas_fato.py:76 → :76` | Sobra |
| `par_do_intervalo` | `chamados/services/materializacao.py:192 → :193` | K21 no contador, K19 no plantio sob trava, K20 na validação. O fio leva a carga de `chamados/services/fio.py:262` |
| `paridade_absolvida_bug_par_103` | `chamados/services/materializacao.py:259 → :261` | K21 no contador. A devolução relê uma pergunta sob trava em `chamados/services/validacao.py:333` |
| `_situacao_da_irma` | `chamados/services/materializacao.py:290 → :293` | K22, `q` carregada avulsa em `:286` |
| `estado_aplicacao_pergunta` | `chamados/services/materializacao.py:885 → :889` | K21, K61 e carga do fio em `chamados/services/fio.py:262` |
| `_recusa_sem_anuencia` | `chamados/services/materializacao.py:1300 → :1305` | K19 no plantio sob trava. A porta de validação também entrega a instância por ID de `chamados/services/validacao.py:677` |
| Vigia `censo_alcance` | `chamados/services/vigia_fio.py:636 → :637` | K2 para `ps`, em `chamados/services/vigia_fio.py:560`, `:566`. K60 abastece `dono_fora_do_fio` em `:616` |

## Carregadores e arquivos tocados

As referências usam `app/` como raiz. Os 12 arquivos de produção são os desta tabela. Os sete arquivos de teste são o selo, os três módulos novos de lotes, sítios e mutantes, e os três harnesses listados nas adaptações. `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `chamados/tests/test_e4c_f5_voltaC_lotes.py:73`, `chamados/tests/test_e4c_f5_voltaC_sitios.py:37`, `chamados/tests/e4c_f5_voltaC_mutantes.py:11`.

| K | Arquivo e linha base → C | Antes → depois |
|---:|---|---|
| 5 | `chamados/management/commands/auditar_invariantes_chamados.py:1586 → :1592` | Universo I21a → `decisoes` no queryset |
| 6 | `chamados/management/commands/auditar_invariantes_chamados.py:590 → :595` | Pendentes I2 → `decisoes` no queryset |
| 7 | `chamados/management/commands/auditar_invariantes_chamados.py:1642 → :1646` | Perguntas do destino → `perguntas_que_resolvem__decisoes` |
| 8 | `chamados/services/fio.py:2078 → :2081` | Sementes do grupo → `decisoes` no queryset |
| 12 | `chamados/management/commands/backfill_fio_morto_e4b.py:89 → :90` | Perguntas do plano do dia → `decisoes` no queryset |
| 13 | `chamados/management/commands/backfill_fio_morto_e4b.py:134 → :135` | Alvos da reabertura → `decisoes` no queryset e releitura em lote dos restantes após `reaberto` |
| 14 | `chamados/management/commands/backfill_fio_morto_e4b.py:166 → :178` | Varredura das exportadas → `decisoes` no queryset |
| 21 | `chamados/services/materializacao.py:324 → :328` | Universo U163 → `decisoes` no queryset |
| 28 | `chamados/management/commands/backfill_decisoes_pergunta.py:531 → :534` | `only()` do DRY → `decisoes` mais escalares do compositor, do passivo e da chave usada pelo plano |
| 33 | `chamados/management/commands/auditar_invariantes_chamados.py:1723 → :1726` | Perguntas F1t → `decisoes` no queryset reutilizado pela contraprova |
| 34 | `chamados/management/commands/auditar_invariantes_chamados.py:537 → :540` | Pares I6 → `decisoes` no queryset |
| 35 | `chamados/management/commands/auditar_invariantes_chamados.py:860 → :864` | Candidatas I8 → `decisoes` no queryset |
| 36 | `chamados/management/commands/auditar_invariantes_chamados.py:896 → :899` | Candidatas I8b → `decisoes` no queryset |
| 37 | `chamados/management/commands/auditar_invariantes_chamados.py:1781 → :1786` | Universo F2 → `decisoes` no queryset |
| 38 | `chamados/management/commands/auditar_invariantes_chamados.py:1794 → :1798` | Universo F3 → `decisoes` no queryset |
| 39 | `chamados/management/commands/disparar_perguntas_competencia.py:107 → :109` | Perguntas do contador de fio mudo → `perguntas_que_resolvem__decisoes` |
| 51 | `chamados/services/vigia_fio.py:711 → :714` | Vivas em fechadas → `decisoes` por bloco, `iterator(chunk_size=500)` em `:719` |
| 52 | `chamados/services/vigia_fio.py:770 → :774` | Carimbos sem lastro → `decisoes` por bloco, `iterator(chunk_size=500)` em `:779` |
| 56 | `chamados/management/commands/estoque_l130_instante_sem_texto.py:37 → :38` | Foto do estoque → `decisoes` no queryset |
| 57 | `chamados/management/commands/estoque_l130_instante_sem_texto.py:97 → :98` | Releitura sob trava → `decisoes` no queryset |
| 59 | `chamados/management/commands/arquivar_competencia_encerrada.py:158 → :158` | `d.perguntas.all()` → `d.perguntas.prefetch_related('decisoes')` |
| 60 | `chamados/services/vigia_fio.py:570 → :571` | Todas as perguntas das células → `decisoes` no queryset |
| 66 | `ponto/management/commands/emitir_furo_retroativo.py:83 → :83` | Perguntas dos chamados do colaborador → `perguntas_que_resolvem__decisoes` |
| 69 | `chamados/management/commands/fechar_becos_disputa.py:32 → :33` | Perguntas das disputas abertas → `perguntas__decisoes` |
| Sobra | `chamados/management/commands/dedup_perguntas_fato.py:75 → :75` | Perguntas do grupo → `decisoes` no queryset |

K28 carrega `validada_por` junto de `validada_em`, pois uma validação projetada sem ato correspondente lê o ID do autor. O RED confere os seis escalares da leitura antes de chamar a canônica. K8 carrega as sementes e continua usando o percurso de vizinhança de K18. O ganho no percurso integral de descoberta dos grupos está [nao verificado]. `chamados/models.py:4274`, `:4286`, `chamados/management/commands/backfill_decisoes_pergunta.py:531`, `chamados/tests/test_e4c_f5_voltaC_lotes.py:119`, `chamados/services/fio.py:1597`, `:2081`, `:2146`.

**R restantes, 6.** K3 relê a candidata sob trava, `chamados/services/relavra_via.py:230`, `:236`. K19 relê cada item do plantio, `chamados/services/materializacao.py:2121`, `:2122`. K20 e K58 entram por ID, `chamados/services/validacao.py:677`, `:431`. K22 procura a irmã de uma pergunta, `chamados/services/materializacao.py:286`. K40 faz `.get(pk=pk)` dentro do laço, `chamados/management/commands/migrar_data_evento_dia_do_turno.py:464`, `:466`.

## Chokepoint G

- A emissão abre uma pergunta e tenta reatá-la imediatamente na mesma iteração e savepoint. Há também uma entrada que chama a emissão com um único motivo. `chamados/services/disputa_emissao.py:294`, `:450`, `:468`, `:473`.
- A abertura retorna pergunta e indicador de criação. As linhas que a porta interna registra ficam fora do retorno. O outro caminho de reate já carrega K9 em lote. `chamados/models.py:2983`, `:3231`, `:3243`, `chamados/reconciliador.py:1575`.
- Deixei G na leitura avulsa. Nessa entrada, o lote exigiria adiar o reate para depois de outros nascimentos, alterando a ordem dos atos. O compositor continua sendo chamado pelo juiz. `chamados/services/disputa_emissao.py:320`, `:468`, `:473`, `chamados/reconciliador.py:367`.

| Cenário | Base do anexo / teto | Previsão C |
|---|---:|---|
| 1 fecha | 236 / 230 | Δ 0 [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:435` |
| 1 FICA | 146 / 145 | Δ 0 [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:470` |
| 3 fecham | 300 / 294 | Δ 0 [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:440` |
| 3 FICAM | 221 / 219 | Δ 0 [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:475` |

A previsão considera o caminho síncrono de fecho, que chega a `materializar_pendentes_aprovadas` pelo S144. Essa função usa a guarda do lastro e `carimbar`. O item D fica em `chamados/models.py:2077` para o Opus. `chamados/signals.py:150`, `:164`, `chamados/services/materializacao.py:2173`, `:2185`.

## REDs, guardas e comandos para o Code

O RED de lote confere os PKs que chegaram ao compositor e compara somente os SELECTs cujo primeiro `FROM` é `chamados_decisaopergunta`. Dedup e K28 injetam um grupo completo na fronteira do DRY e isolam a busca K18 de vizinhos. A emissão retroativa usa o `handle` real e o juiz real do fio, com grade de um dia e saída do classificador de dia controladas. K13 tem um caso próprio de frescor, com a porta real de reabertura gravando por PK dentro de savepoint desfeito. `chamados/tests/test_e4c_f5_opus_models.py:25`, `chamados/tests/test_e4c_f5_voltaC_lotes.py:32`, `:49`, `:74`, `:119`, `:198`, `:239`.

| Sítio | Caso em `chamados.tests.test_e4c_f5_voltaC_sitios.SitiosVoltaCTest` |
|---|---|
| I21a | `test_RED_i21a_reabertura_desmente_a_projecao`, `:65` |
| I2 | `test_RED_i2_resposta_do_ato_entra_no_piso`, `:72` |
| F1 | `test_RED_f1_resposta_do_ato_retira_o_chamado_do_piso_sem_dia`, `:78` |
| Dedup | `test_RED_dedup_nomeia_resposta_do_ato`, `:87` |
| Par | `test_RED_par_do_intervalo_reconhece_resposta_do_ato`, `:93` |
| BUG-PAR-103 | `test_RED_bug_par_103_reconhece_resposta_do_ato`, `:99` |
| Irmã | `test_RED_situacao_da_irma_le_instante_do_ato`, `:110` |
| Aplicação | `test_RED_estado_aplicacao_le_instante_do_ato`, `:117` |
| Anuência | `test_RED_anuencia_recusa_par_antigo_depois_de_reabrir`, `:125` |
| Vigia | `test_RED_vigia_alcance_resposta_do_ato_sob_terminal`, `:135` |
| **GUARDA** | `test_GUARDA_ato_com_texto_e_coluna_vazia_nao_da_anuencia_ao_par`, `:145`. A base já recusava o texto vazio da projeção, `chamados/services/materializacao.py:192` na base e `chamados/juizes.py:1556` |

A partir de `app/`, o Code pode usar os mesmos labels dos REDs na cópia de medição da base e na C. Os módulos de teste estão em `chamados/tests/test_e4c_f5_voltaC_lotes.py:73`, `chamados/tests/test_e4c_f5_voltaC_sitios.py:24`, `:37`.

```bash
lotes=chamados.tests.test_e4c_f5_voltaC_lotes.CarregadoresVoltaCTest
sitios=chamados.tests.test_e4c_f5_voltaC_sitios
choke=ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  "$lotes" "$sitios.SitiosVoltaCTest" \
  chamados.tests.test_e4c_f5_selo_chamada \
  chamados.tests.test_bug_par_c2_sem_banco \
  chamados.tests.test_bug_par_sem_banco \
  chamados.tests.test_estado_aplicacao

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  "$choke.test_registrar_batida_com_1_furo_vivo_que_ela_fecha" \
  "$choke.test_registrar_batida_com_1_furo_vivo_que_fica" \
  "$choke.test_registrar_batida_com_3_furos_vivos_que_ela_fecha" \
  "$choke.test_registrar_batida_com_3_furos_vivos_que_ficam" \
  chamados.tests.test_selo_teto_modal
```

O seletor tem **exatamente duas** ocorrências de `os.environ.get(SELETOR)`. O efeito previsto para cada mutante de lote é `Q(6) > Q(3)` na igualdade. A mordida ORM de cada um está [nao verificado]. A transformação de `par_p_cru` foi conferida por AST e acusa somente `par_do_intervalo` em `chamados/services/materializacao.py:193`. `chamados/tests/e4c_f5_voltaC_mutantes.py:27`, `:63`, `chamados/tests/test_e4c_f5_voltaC_lotes.py:85`.

```bash
mut_lote() {
  F5_VOLTA_C_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 \
    python3 manage.py test "$lotes.$2"
}
```

| Mutante → caso que deve cair | Comando |
|---|---|
| K8, universo `:89` | `mut_lote k8 test_RED_k8_universo_do_grupo` |
| K21, contador `:100` | `mut_lote k21 test_RED_k21_contador_bug_par` |
| K28, DRY `:119` | `mut_lote k28 test_RED_k28_dry_backfill_decisoes` |
| K34, auditor I6 `:141` | `mut_lote k34 test_RED_k34_auditor_i6` |
| K51, vigia de fechadas `:154` | `mut_lote k51 test_RED_k51_vivas_em_fechada` |
| K52, vigia de carimbos `:167` | `mut_lote k52 test_RED_k52_carimbos_por_dono` |
| K60, censo `:181` | `mut_lote k60 test_RED_k60_vigia_alcance` |
| K66, emissão retroativa `:198` | `mut_lote k66 test_RED_k66_emissao_retroativa` |
| K69, becos `:218` | `mut_lote k69 test_RED_k69_fechar_becos` |
| Sobra, dedup `:230` | `mut_lote sobra test_RED_sobra_dedup_por_grupo` |
| `par_p_cru`, catraca em `chamados/tests/test_e4c_f5_voltaC_sitios.py:25` | `F5_VOLTA_C_MUTANTE=par_p_cru PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$sitios.SeloMutanteVoltaCTest.test_MORDE_par_do_intervalo_com_pergunta_crua"` |

As linhas de K na tabela de mutantes referem-se a `chamados/tests/test_e4c_f5_voltaC_lotes.py`.

## Adaptações e tetos

| Arquivo | Classe | Antes → depois | Asserts |
|---|---|---|---:|
| `chamados/tests/test_bug_par_sem_banco.py:170`, `:193` | ii | `NS` de pergunta e módulo sem compositor → `pergunta` e `DecisaoPergunta` do helper existente | 16 → 16 |
| `chamados/tests/test_bug_par_c2_sem_banco.py:80`, `:82` | ii | `NS` de pergunta e módulo sem compositor → mesmos helpers | 15 → 15 |
| `chamados/tests/test_bug_par_c2_sem_banco.py:32` | v | `_Recorte(list)` com métodos próprios → `_Recorte(Linhas)` com `prefetch_related` identidade e o mesmo filtro de disputa | incluídos nos 15 → 15 |
| `chamados/tests/test_estado_aplicacao.py:41` | ii | `NS` sem os insumos de `leitura` → `pergunta` do helper existente, com os mesmos valores do caso | 5 → 5 |
| `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `:189` | Catraca solicitada e i na âncora sintética | Dez identidades → conjunto vazio. A chamada de ordem 1 da fixture usa a composição real da C, preservando a acusação da ordem 2 | 12 → 12 |

O import de `DIA` referido no anexo já existe em `d9319a215`. `chamados/tests/test_e4c_f5_voltaB_sitios.py:17`.

**Fora das classes**, a previsão é persistência dos quatro FAILs de PASSAGEM pelos excessos acima. Outras quebras estão [nao verificado]. **Tetos que prevejo cair:** nenhum dos existentes foi identificado. No modal, os sítios convertidos recebem perguntas com `decisoes` já carregadas. O valor medido na C está [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:435`, `:440`, `:470`, `:475`, `chamados/services/fio.py:262`, `:268`, `:360`, `:381`, `chamados/tests/test_selo_teto_modal.py:184`.

## Campos, escritores e releituras

| Campo | Escrita e significado para os leitores da C |
|---|---|
| `validada_em` | A validação e as devoluções gravam a projeção e o ato. Nova resposta e reabertura podem limpar a validação. O compositor recebe validação vigente da sequência com passivo. As guardas escalares da irmã e do plantio continuam usando o carimbo projetado, que pode divergir da sequência. `chamados/services/validacao.py:168`, `:173`, `:273`, `:279`, `:349`, `:354`, `chamados/services/disputa_emissao.py:1494`, `:1615`, `chamados/models.py:4335`, `:4358`, `chamados/services/materializacao.py:200`, `:2128` |
| `respondida_em` | Resposta humana, automática e pela trilha gravam instante e ato. Reabertura limpa o par. Carimbo pode completar o instante projetado. Nos dez chamadores, o juiz recebe o instante vigente do ato. As faces SQL da auditoria e a guarda de integridade do vigia ainda leem a coluna projetada, que pode divergir. `chamados/services/disputa_emissao.py:1482`, `:1532`, `:1615`, `:1683`, `:1686`, `:1705`, `:1708`, `chamados/models.py:3349`, `:4358`, `chamados/juizes.py:1538`, `:1583`, `chamados/management/commands/auditar_invariantes_chamados.py:1587`, `chamados/services/vigia_fio.py:635` |
| `resposta_colab` | Resposta humana e automática gravam o texto atual. Reabertura e ressuscitação o limpam. O compositor, a derivação da hora e as guardas de projeção usam esse mesmo texto corrente. `chamados/services/disputa_emissao.py:1481`, `:1615`, `:1682`, `chamados/models.py:1722`, `:4358`, `chamados/services/materializacao.py:640`, `chamados/juizes.py:1538` |
| `materializada_em` | `_escrever_carimbo` abastece as portas de carimbo e identidade. A porta de lote limpa os campos do carimbo. Os leitores usam a mesma coluna como carimbo gravado. O C6 ainda exige os demais insumos para julgar vitalidade. `chamados/models.py:3314`, `:3346`, `:3401`, `:3508`, `:4359`, `chamados/services/materializacao.py:262`, `:882`, `chamados/juizes.py:1446` |
| `via_resolucao` | As mesmas portas gravam a via junto do carimbo. O compositor lê a coluna, e o BUG-PAR e a testemunha de aplicação leem o mesmo motivo gravado. O temporal recorta carimbos futuros de acordo com a via. `chamados/models.py:3318`, `:3346`, `:3401`, `:3508`, `:4359`, `chamados/services/materializacao.py:263`, `:882`, `chamados/juizes.py:1398` |
| `decisoes` | `registrar` insere pela FK. Resposta, validação, reabertura, absorção, ressuscitação e backfill chamam essa porta. `leitura` consome a relação e incorpora o passivo, e `composta` fornece a entrada dos juízes. O rótulo BUG-PAR lê a trilha de devolução para exibição. `chamados/models.py:4188`, `:1713`, `:4329`, `:4362`, `chamados/services/disputa_emissao.py:1532`, `:1639`, `chamados/services/validacao.py:173`, `chamados/services/materializacao.py:277`, `:1065`, `chamados/management/commands/backfill_decisoes_pergunta.py:351` |

**Materialização no mesmo fluxo**

- **Par.** O juízo lê antes de plantar a irmã. Na validação, o ato anterior entra por `pergunta=perg`, invalidando o cache dessa instância. Depois de sucesso ou rollback do par a porta faz `refresh_from_db()` integral. No plantio, a releitura imediata depois da irmã é da vizinhança de batidas, e a próxima metade é buscada por PK sob trava. `chamados/services/validacao.py:173`, `:632`, `:646`, `:655`, `:743`, `:771`, `chamados/models.py:4376`, `chamados/services/materializacao.py:1409`, `:1416`, `:1737`, `:2122`.
- **BUG-PAR-103.** O contador é leitura. A devolução busca `perg` sob trava, julga o estado anterior, grava descarimbo e devolução e monta a trilha a partir da foto anterior e dos campos atualizados. O registro recebe `pergunta=perg` e invalida o prefetch dessa instância. `chamados/services/materializacao.py:328`, `:332`, `chamados/services/validacao.py:332`, `:333`, `:334`, `:345`, `:354`, `:362`, `chamados/models.py:4376`.
- **Irmã e aplicação.** `_situacao_da_irma` consulta `q` avulsa. `estado_aplicacao_pergunta` lê a testemunha, e a via de recusa tem precedência. A porta de escrita chama o derivador antes do plantio. Esses caminhos não servem cache de decisão anterior a uma escrita do próprio percurso. `chamados/services/materializacao.py:286`, `:293`, `:882`, `:893`, `:896`, `:1930`.
- **Anuência.** Os três ramos consultam antes de escrever. Na recusa, a releitura em `refresh_from_db(fields=...)` é dos escalares do carimbo e da recusa. O ramo retorna sem recompor a pergunta. No caminho que aplica, o carimbo e a cauda do fecho vêm depois, e as vivas são buscadas novamente. `chamados/services/materializacao.py:1305`, `:1360`, `:1595`, `:1679`, `:1694`, `:1986`, `:2029`, `chamados/juizes.py:1512`.

**Outras releituras com lote.** Em K13, a ressuscitação pode registrar por PK. Depois de `reaberto`, o comando refaz a carga dos restantes e conserva a ordem inicial. K57 carrega novamente sob trava, e a porta K58 ainda relê por ID. Em K69, o fecho atualiza a disputa com `refresh_from_db()` antes de perguntar se pode fechar. `chamados/models.py:1706`, `:1713`, `chamados/management/commands/backfill_fio_mort

[truncado em 20000 de 20416 caracteres]

## 20/11 00:32 · sessao b9ffd9798ead · CADEIA-20-PASSO-1 · leitura · origem=Code

**1. Quem escolhe a grade**

A escolha é a mesma para todos os chamadores. `_julgar_colab_corpo` chama `montar_grade_prevista_periodo`, que usa **POR TURNO** se `settings.GRADE_POR_TURNO` for verdadeiro e **LEGADA** se for falso ou ausente. A configuração base vem do ambiente e tem padrão `True`. A POR TURNO devolve `insumos_razao_sobras` com turnos, avulsas e instantes. A LEGADA devolve `marcos` e `dias`. `app/ponto/services/cartorio.py:1317`, `app/escala/utils.py:2131`, `app/config/settings/base.py:165`, `app/escala/utils.py:2117`, `app/escala/utils.py:1065`.

- **Dois chamadores diretos de `_lavrar_razoes_sobras`**. O ramo `nunca_bateu` entra quando `adesao is None` e grava com `apply_`. O ramo normal atende quem tem adesão e também grava com `apply_`. Ambos passam `g.get('insumos_razao_sobras')` imediatamente antes de `lavrar_veredito(ata=...)`. A data, o teto do dia e a versão da regra não participam do seletor da grade. `app/ponto/services/cartorio.py:1353`, `app/ponto/services/cartorio.py:1410`, `app/ponto/services/cartorio.py:1434`, `app/ponto/services/cartorio.py:1461`, `app/ponto/services/cartorio.py:2012`, `app/escala/utils.py:2131`.
- **Três chamadas ao corpo**. `julgar_colab` chama o corpo na entrada, repete com `forcar=True` se houver células na fila `rejulgar` e faz a releitura sem forçar se houve emissão nova. As três passam pelo mesmo seletor. `app/ponto/services/cartorio.py:1123`, `app/ponto/services/cartorio.py:1152`, `app/ponto/services/cartorio.py:1209`, `app/ponto/services/cartorio.py:1222`, `app/ponto/services/cartorio.py:1317`.
- **Entradas de `julgar_colab`**. `processar_cartorio`, `julgar_celula`, `rejulgar_furos_do_vinculo`, `celula_veredito_velho` e o script `rejulgar_o118_col221`. `lavrar_status_competencia` também entra, com `completar_status=True`, e escreve só status. `app/ponto/management/commands/processar_cartorio.py:69`, `app/ponto/services/cartorio.py:2203`, `app/escala/regua_defesa.py:695`, `app/ponto/management/commands/celula_veredito_velho.py:144`, `bin/rejulgar_o118_col221.py:24`, `app/ponto/management/commands/lavrar_status_competencia.py:56`, `app/ponto/services/cartorio.py:942`.
- **Entradas por `julgar_celula`**. Vigia de hora, sinais de batida, ausência, justificativa e chamado, reconciliador de chamados, reescrita da célula, pós-vínculo, `cobrar_dia`, `furos_sem_cobranca`, `rejulgar_ausencia_em_folga` e `curar_residuo_bug115`. Todos chegam a `julgar_colab` pela mesma chamada. `app/ponto/services/vigia_de_hora.py:134`, `app/ponto/signals.py:93`, `app/ponto/signals.py:131`, `app/ponto/signals.py:161`, `app/chamados/signals.py:218`, `app/chamados/reconciliador.py:752`, `app/ponto/portas/celula.py:472`, `app/colaboradores/services/pos_vinculo.py:73`, `app/chamados/services/cobrar_dia.py:41`, `app/ponto/services/furos_sem_cobranca.py:128`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:80`, `app/ponto/management/commands/curar_residuo_bug115.py:146`, `app/ponto/services/cartorio.py:2203`.

**2. Célula que nasce no próprio dia**

`gerar_celulas` cria a célula com DNA e `ata` nula. Ainda no próprio dia, o vigia de hora pode lavrá-la quando ela passa pelos filtros e tem marco vencido, usando `teto_de_hoje=True`. Uma chamada avulsa também pode lavrá-la no dia se `trabalha is False` ou se já passou o fim do turno. O lote `processar_cartorio` só a alcança quando `data < hoje`. Nessa rodada, a impressão em dia pode fazê-lo pular a célula, salvo `--forcar`. `app/ponto/management/commands/gerar_celulas.py:349`, `app/ponto/portas/celula.py:913`, `app/escala/models.py:1778`, `app/ponto/services/vigia_de_hora.py:117`, `app/ponto/services/cartorio.py:2196`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1402`, `app/ponto/services/cartorio.py:1552`.

O chamador efetivo de cada um dos dois passes das 19 células e o valor da flag em cada passe ficam **[nao verificado]**. A troca de passada interna, por si só, usa o mesmo seletor. `app/ponto/services/cartorio.py:1123`, `app/ponto/services/cartorio.py:1156`, `app/ponto/services/cartorio.py:1222`, `app/ponto/services/cartorio.py:1317`.

**3. Quem grava a ata e quem lê a ausência**

- `ata_do_dia` monta a ata-base com `orfas` e `n_orfas`. `lavrar_veredito` grava o dicionário recebido, preservando da ata anterior apenas a chave `precedencia` quando ela falta na nova. Assim, uma entrada sem `razoes_orfas` pode substituir uma ata que tinha a chave, mesmo com veredito igual. `carimbar_precedencia`, chamado depois do julgamento avulso, copia a ata atual e acrescenta `precedencia`. Se a ata era nula, pode gravar um dicionário contendo só essa chave. `app/ponto/services/cartorio.py:752`, `app/ponto/portas/celula.py:106`, `app/ponto/portas/celula.py:1049`, `app/ponto/portas/celula.py:1076`, `app/ponto/portas/celula.py:1090`, `app/ponto/services/cartorio.py:2209`, `app/ponto/portas/celula.py:1205`.
- `sobras_da_ata`, usado pela grade lavrada e pelo espelho, devolve `[]` quando não há lista de `orfas` a projetar. Se há órfãs, chave ausente e lista de razões vazia resultam em **“razao nao lavrada”** por ocorrência, com o sufixo **“anterior ao horizonte”** abaixo da v5. A contagem de órfãs vem de `orfas` ou de `n_orfas`. `app/escala/services/leitor_celula.py:180`, `app/escala/services/leitor_celula.py:193`, `app/escala/services/leitor_celula.py:454`, `app/ponto/services/espelho.py:708`.
- `medir_razoes_sobras` **distingue as formas quando há órfãs**. Chave ausente conta em `orfas_sem_chave`. Chave presente com `[]` e órfãs na ata conta como desalinhamento. `atas_no_horizonte_sem_razao` pula atas sem órfãs pela lista e pela contagem, e acusa ambas as formas quando há órfãs e a versão é pelo menos 5. `app/ponto/services/cartorio.py:1004`, `app/ponto/services/cartorio.py:1038`.

**4. Escritor único da chave**

O único ponto que atribui `razoes_orfas` é **`_lavrar_razoes_sobras`**. Ali estão os três resultados atuais: omissão quando faltam insumos, `[]` quando há insumos e zero órfãs, e uma lista de respostas quando há órfãs a classificar. Para uma ata com `orfas=[]` e `n_orfas=0`, os leitores acima produzem o mesmo resultado com a chave ausente ou com `[]`. Com órfãs na lista, a troca muda o significado para `medir_razoes_sobras`. `app/ponto/services/cartorio.py:962`, `app/ponto/services/cartorio.py:1001`, `app/ponto/services/cartorio.py:1010`, `app/ponto/services/cartorio.py:1042`, `app/escala/services/leitor_celula.py:198`.

## 20/11 00:29 · sessao e8d689a8df3e · CADEIA-20-PASSO-1 · verificador · origem=Code

**CADEIA-20-PASSO-1, item (3) — BLOQUEIA.** O raspado informa mudança de ata na segunda passada com o insumo capturado igual em (A). Em (B), a mudança de hash acompanha mudanças nos insumos que o código manda hashear. O bloqueio de (B) é de ordem da cadeia: o cartório pode carimbar antes de a própria rodada terminar de alterar esses insumos. A meta de R5 é diferença zero na segunda rodada. `app/core/placar_estrutural.py:416–420`, `app/ponto/services/cartorio.py:502–507`, `app/ponto/services/cartorio.py:543–547`, `app/ponto/services/cartorio.py:1133–1157`, `app/ponto/services/cartorio.py:1179–1181`.

Identidade Git da cópia com `a4a9b6bfe5f7` [nao verificado]. `git rev-parse HEAD` retornou que o diretório fornecido não é um repositório Git.

### 1. (A) — BLOQUEIA

A primeira lavra e a relavra passam pelo mesmo corpo do cartório. Tanto o ramo normal quanto `nunca_bateu` montam a ata e chamam `_lavrar_razoes_sobras`. Com ata, zero órfãs e insumos da grade presentes, o escritor põe `[]`. Com insumos `None`, retorna sem incluir a chave na ata recém-montada. A grade por turno entrega os insumos, e a grade legada omite essa chave. O escritor do campo nunca atribui `None` a `razoes_orfas`. `app/ponto/services/cartorio.py:752–794`, `app/ponto/services/cartorio.py:962–971`, `app/ponto/services/cartorio.py:994–1001`, `app/ponto/services/cartorio.py:1123`, `app/ponto/services/cartorio.py:1156–1157`, `app/ponto/services/cartorio.py:1393`, `app/ponto/services/cartorio.py:1434–1459`, `app/ponto/services/cartorio.py:2013–2037`, `app/escala/utils.py:1065`, `app/escala/utils.py:2117–2137`.

A porta `lavrar_veredito` compara a ata inteira e atribui `cel.ata` quando ela difere. `carimbar` persiste os campos devolvidos pela lavra. A entrada avulsa `julgar_celula` ainda chama `carimbar_precedencia`, que copia a ata e altera a chave `precedencia`. A limpeza de julgamento de célula futura zera a ata inteira pela própria porta. O caminho efetivo da primeira gravação nas 19 células [nao verificado]. `app/ponto/portas/celula.py:1049–1067`, `app/ponto/portas/celula.py:1090–1098`, `app/ponto/portas/celula.py:1146–1167`, `app/ponto/portas/celula.py:1189–1216`, `app/ponto/services/cartorio.py:2202–2211`.

| Leitor direto | Ausente, `None` e `[]` |
|---|---|
| `sobras_da_ata` | Sem órfãs, devolve `[]` antes de ler a chave. Com órfãs, normaliza ausência e `None` para `[]`. É a projeção usada pela grade e pelo espelho. `app/escala/services/leitor_celula.py:193–223`, `app/escala/services/leitor_celula.py:450–454`, `app/ponto/services/espelho.py:708–710` |
| `medir_razoes_sobras` | Chave ausente tem ramo próprio. `[]` é uma lista válida para `len`; `None` explícito cai em `len(None)` e levanta `TypeError`. Com zero órfãs, chave ausente e `[]` dão a mesma contagem. `app/ponto/services/cartorio.py:1004–1035` |
| `atas_no_horizonte_sem_razao` | Sem órfãs nem contagem positiva, pula a ata antes de ler a chave. Com órfãs no horizonte, ausência e `None` falham no teste de tipo, e `[]` falha no alinhamento. `app/ponto/services/cartorio.py:1038–1056` |

A comparação da ata inteira também distingue os três estados na escrita. `app/ponto/portas/celula.py:1049–1059`.

### 2. (B) — BLOQUEIA pela ordem

**Sim, a impressão inclui insumo que a própria cadeia pode alterar adiante.** Ela inclui `pk:status_local` dos chamados do dia. `fio` conta **chamados vivos com pelo menos uma pergunta viva**. Com chamados vivos e nenhum com fio, entra `fio:0`. Sem chamados vivos, a parte `fio` é omitida. O juiz da pergunta também lê a célula, portanto a própria lavra pode mudar o resultado da próxima leitura do fio. `app/ponto/services/cartorio.py:146–155`, `app/ponto/services/cartorio.py:502–507`, `app/ponto/services/cartorio.py:543–547`, `app/ponto/services/cartorio.py:1527–1547`, `app/chamados/juizes.py:1450–1469`, `app/chamados/juizes.py:1510–1519`.

1. **Dentro do cartório**, o corpo calcula `imp` e depois o carimba. Há fechamentos e ressuscitações entre esses dois pontos. Depois do primeiro carimbo, o E2 carimba perguntas absolvidas e o cartório tenta fechar os chamados adiados. Os casos enfileirados voltam ao mesmo corpo. Depois, a fábrica pode criar perguntas ou desfazer carimbos. A releitura seguinte exige aumento de `emitidos`, e as filas produzidas nas releituras têm caminhos de descarte. `app/ponto/services/cartorio.py:1541–1547`, `app/ponto/services/cartorio.py:1691–1704`, `app/ponto/services/cartorio.py:1835–1861`, `app/ponto/services/cartorio.py:2035–2037`, `app/ponto/services/cartorio.py:1123–1164`, `app/ponto/services/cartorio.py:1179–1181`, `app/ponto/services/cartorio.py:1209–1231`, `app/chamados/reconciliador.py:634–648`, `app/chamados/services/fabrica_da_celula.py:160–174`, `app/chamados/services/fabrica_da_celula.py:206–219`.
2. **Na ordem configurada**, `reconciliar_grade` precede `processar_cartorio --apply`. Depois do cartório, um ramo segue por `apurar_furos_diarios` → `disparar_perguntas_competencia --alarme` → `reconciliar_perguntas_orfas --alarme`. `apurar_furos_diarios` chama o agrupamento sob limbo, que pode reatar perguntas e reconciliar o furo. `supra_juiz --executar` também depende do cartório e pode chamar o retratador. A transição `em_analise → resolvido` vem de `RECONCILIAR` e é persistida por `_gravar_estado`. `app/config/crons.py:462`, `app/config/crons.py:599`, `app/config/crons.py:616`, `app/config/crons.py:1527–1545`, `app/escala/management/commands/apurar_furos_diarios.py:13–19`, `app/chamados/services/limbo_folgas.py:279–287`, `app/ponto/management/commands/supra_juiz.py:257–287`, `app/chamados/reconciliador.py:572–586`, `app/chamados/catalogo/motor.py:109–113`, `app/chamados/models.py:234–235`, `app/chamados/models.py:302`, `app/chamados/models.py:654–668`.
3. **Pelo evento da célula**, a porta salva e publica no mesmo ato. O consumidor tem agenda própria e pode carimbar perguntas, retratar chamados e rodar a fábrica. O `post_save` do chamado tenta rejulgar quando está fora de um julgamento ativo e tem dia identificável. O comando, evento ou signal que produziu cada uma das oito transições, e a ordem efetiva da sonda, ficam [nao verificado]. `app/ponto/portas/celula.py:942–965`, `app/config/crons.py:393–394`, `app/chamados/reconciliador.py:1414–1438`, `app/chamados/signals.py:189–218`.

### 3. Terceiro passe — BLOQUEIA o aceite de idempotência

- **(A)** Com zero órfãs e a mesma grade por turno, uma nova lavra volta a produzir `[]`. Com a mesma impressão, o caminho comum pula a célula. Logo, o próprio caso `ausente/None → []` não se repete sob essas condições. O resultado efetivo do terceiro passe [nao verificado]. `app/ponto/services/cartorio.py:964–971`, `app/ponto/services/cartorio.py:1402–1408`, `app/ponto/services/cartorio.py:1552–1558`.
- **(B)** Se o status e o fio lidos no terceiro passe já estiverem refletidos no hash do segundo e os demais insumos se mantiverem, o caminho comum pula. Se o segundo passe tiver deixado uma mudança posterior ao último carimbo, o terceiro pode gravar outro hash com a mesma ata. O código não garante convergência em três passes, e o raspado só vai até o segundo [nao verificado]. `app/ponto/services/cartorio.py:1209–1231`, `app/ponto/services/cartorio.py:1541–1558`, `app/ponto/portas/celula.py:1189–1201`.

### 4. Classes de reprovação

| Classe | Veredito no caminho lido |
|---|---|
| Segundo juiz registrado | **NAO BLOQUEIA.** O registro aponta `razao_da_sobra` e `fio_vivo`, e o cartório chama essas autoridades. `app/core/juizes.py:290–299`, `app/core/juizes.py:1332–1333`, `app/ponto/services/cartorio.py:994–996`, `app/ponto/services/cartorio.py:1466`, `app/ponto/services/cartorio.py:1532` |
| Segundo escritor fora da porta | **NAO BLOQUEIA.** As duas lavras de razões convergem para o mesmo escritor e persistem pela porta da célula. `app/ponto/services/cartorio.py:1434–1436`, `app/ponto/services/cartorio.py:2013–2016`, `app/escala/models.py:1826–1828`, `app/ponto/portas/celula.py:1090–1098` |
| Relógio fora da borda | **BLOQUEIA.** O cartório tem `agora`, mas a leitura de `fio` chama `fio_vivo` sem esse instante. O juiz pode cair em `date.today()` e em `classificar_falta` com fallback para `_tz.now()`. Efeito nas oito células [nao verificado]. `app/ponto/services/cartorio.py:1122`, `app/ponto/services/cartorio.py:1532`, `app/chamados/juizes.py:588–596`, `app/chamados/juizes.py:1470–1477`, `app/chamados/juizes.py:1519`, `app/escala/servico_jornada.py:385–389` |
| Derivação paralela | **BLOQUEIA na contagem do fio.** O cartório compartilha `_com_fio` entre hash e julgamento, mas o comando `supra_juiz` recalcula a agregação para o placar. Ambos consultam o mesmo predicado por chamado. Divergência de contagem no raspado [nao verificado]. `CLAUDE.md:20–22`, `app/ponto/services/cartorio.py:1527–1545`, `app/ponto/services/cartorio.py:1614–1617`, `app/ponto/management/commands/supra_juiz.py:211–225` |
| Campo com dois significados | **BLOQUEIA em (A).** A ausência da chave e a lista vazia são distinguíveis na persistência, e o contador ainda trata `None` explícito de forma diferente. `app/ponto/services/cartorio.py:964–971`, `app/ponto/services/cartorio.py:1013–1022`, `app/ponto/portas/celula.py:1049–1059` |
| Ordem de etapa | **BLOQUEIA em (B).** Há escrita posterior ao carimbo e a releitura é condicional. `app/ponto/services/cartorio.py:1123–1157`, `app/ponto/services/cartorio.py:1179–1181`, `app/ponto/services/cartorio.py:1209–1231` |

## 20/11 00:27 · sessao 670009206e83 · E4C-F5-CONSTROI · construtor · origem=Code

Apliquei as duas adaptações pedidas e atualizei quatro âncoras de mutação deslocadas pela B/B2. A B2 deixa uma pendência no beco: o módulo falso de `chamados.juizes` ainda precisa expor `pergunta_respondida`. Esse import vem antes de `DecisaoPergunta` e está fora das classes aprovadas para esta volta. `chamados/tests/test_cef_c1_sem_banco.py:88`, `chamados/tests/e3_volta7_apoio.py:106`, `escala/services/cadastro_realidade.py:439`.

### Arquivos tocados e usos dos avais

| Arquivo e linha | Classe | Método ou alvo | Antes → depois |
|---|---|---|---|
| `chamados/tests/test_cef_c1_sem_banco.py:88` | v | `Consulta.prefetch_related` | Ausente → devolve `self`, sem ramo. |
| `chamados/tests/test_cef_c1_sem_banco.py:309` | i | `upload_arranca_motivo` | Âncora do `fio_vivo` sobre `filter(...)` → âncora sobre `filter(...).prefetch_related('decisoes')`. A mutação continua trocando a condição por `if True:`. |
| `chamados/tests/e3_volta7_apoio.py:106` | ii | `cenario`, módulo falso `chamados.models` | `DecisaoPergunta` ausente → exporta a classe do helper único `chamados/tests/duble_pergunta.py:12`, já usada em `chamados/tests/test_e3_r3_c5_sem_banco.py:180` e `chamados/tests/test_e3_r3_c6_sem_banco.py:299`. |
| `chamados/tests/e3_r3_c6_mutantes.py:48` | i | `K6_m1_sem_respondida` | Âncora no teste direto do instante → `a is None and respondida_no_horizonte(p)`. A mutação continua removendo o requisito de resposta. |
| `chamados/tests/e3_r3_c6_mutantes.py:50` | i | `K6_m1_ignora_ancora` | Âncora no teste direto do instante → `a is None and respondida_no_horizonte(p)`. A mutação continua removendo o requisito de ausência de âncora. |
| `chamados/tests/test_e4c_f6f7_c7_sem_banco.py:113` | i | `test_reflexao_e_mutacao_de_perguntas_MORDEM` | `_item(p)` → `_item(p, DecisaoPergunta.composta(p))` na âncora. A mutação continua substituindo a lista publicada por `[]`. |

### Varredura por família

| Família | Resultado da leitura estática |
|---|---|
| **Foto e emissão** | O upload CEF C1 precisava de `prefetch_related`. Os casos CEF C3 reutilizam o mesmo dublê pelo `setUp` do C1. A resolução S128 em memória já usa `Linhas`, que tem o método. `api/foto_recebida.py:81`, `chamados/tests/test_cef_c3_sem_banco.py:143`, `chamados/tests/test_cef_c3_sem_banco.py:171`, `chamados/tests/test_e4b_classe_e_memoria.py:193`, `chamados/tests/test_e4b_classe_e_memoria.py:374`. |
| **Beco C11 e M1** | C5 e C6 já fornecem `DecisaoPergunta.composta` e a projeção temporal. O retorno do helper é um `namedtuple` com `_replace`. A volta 7 já fornecia a projeção e agora exporta o compositor. A lista de imports pendentes está abaixo. `chamados/tests/duble_pergunta.py:7`, `chamados/tests/duble_pergunta.py:23`, `chamados/tests/test_e3_r3_c5_sem_banco.py:161`, `chamados/tests/test_e3_r3_c6_sem_banco.py:290`, `chamados/tests/e3_volta7_apoio.py:74`, `escala/services/cadastro_realidade.py:497`. |
| **Juízes e calendário** | S127 já recebe `Linhas` em `.all()`. No calendário inteiro de LU3, `Consulta.__getattr__` já devolve a própria consulta para `prefetch_related`. O C11 de navegação executa o trecho anterior à consulta. `chamados/juizes.py:2490`, `chamados/tests/test_disputa_s127.py:22`, `chamados/tests/e3_lu3_apoio.py:17`, `chamados/tests/e3_lu3_apoio.py:93`, `chamados/tests/test_e3_r3_c11_sem_banco.py:64`, `colaboradores/services/calendario.py:505`. |
| **API, views e serviços de chamados** | O selo sem banco da API tinha a âncora antiga. Nos demais sítios localizados, os selos leem fonte, dublam outra função ou chegam ao sítio alterado com ORM real. Isso inclui validação e recusa, declaração, detalhe e cobrança, classificação de becos, competência, explicador, migração do fio, limbo e pergunta no app. `api/views.py:2601`, `chamados/tests/test_bug_par_sem_banco.py:116`, `chamados/tests/test_bug_par_c2_sem_banco.py:97`, `chamados/tests/test_e1_volta2.py:141`, `chamados/tests/test_selo_resolucao_um_escritor.py:184`, `chamados/tests/test_e4b_c4v7_memoria.py:204`, `chamados/tests/test_tranca_vivas_08.py:123`, `chamados/tests/test_explicador_disputa.py:46`, `chamados/tests/test_e1_v7_catracas.py:467`, `chamados/tests/test_e1_v10.py:259`, `chamados/tests/test_e1_v6_horizonte.py:479`. |
| **Ficha, folha, dia do colaborador e comandos** | Os casos localizados para as consultas alteradas usam ORM real ou verificam fonte. O dublê do executor supra exerce `pode_executar_csf`; o `prefetch_related` alterado fica em `Command.handle`. `colaboradores/tests/test_ficha_praca_uf.py:8`, `folha/tests/test_previa_proposta.py:33`, `escala/tests/test_leitores_segundo_intervalo.py:246`, `chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:116`, `chamados/tests/test_porta_chamado_e2.py:58`, `ponto/tests/test_supra_juiz_executor.py:67`, `ponto/management/commands/supra_juiz.py:192`. |

**Fora das classes aprovadas.** Pela leitura estática, `pergunta_respondida` falta no juiz falso da volta 7, de C5, de C6 e do caso de porta só no app. O import falha antes de qualquer ramo do beco. Alterar o módulo falso do juiz exigiria ligar uma função de julgamento, além das adaptações de compositor e de ORM autorizadas. `escala/services/cadastro_realidade.py:439`, `chamados/tests/e3_volta7_apoio.py:66`, `chamados/tests/test_e3_r3_c5_sem_banco.py:174`, `chamados/tests/test_e3_r3_c6_sem_banco.py:294`, `chamados/tests/test_e3_degrau_dono_e_porta.py:507`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:79`.

### Selo de asserções

Comparei por AST com `a81f888dc`. A sequência e o conteúdo das asserções e os nomes dos testes são idênticos nos quatro módulos. `git diff --check` passou. Execução de testes e resultado dos mutantes **[nao verificado]**.

| Módulo | `assert` base → final | `self.assert*` base → final | Testes base → final | [nao verificado]
|---|---:|---:|---:|
| `chamados/tests/test_cef_c1_sem_banco.py:1` | 0 → 0 | 25 → 25 | 10 → 10 |
| `chamados/tests/e3_volta7_apoio.py:1` | 0 → 0 | 0 → 0 | 0 → 0 |
| `chamados/tests/e3_r3_c6_mutantes.py:1` | 0 → 0 | 0 → 0 | 0 → 0 |
| `chamados/tests/test_e4c_f6f7_c7_sem_banco.py:1` | 0 → 0 | 14 → 14 | 6 → 6 |

### Comandos para o Code

Executar a partir de `app/`. Os comandos abaixo incluem os consumidores dos módulos de apoio e os harnesses que chegam aos sítios alterados. Em B2, os casos do beco têm a pendência de import descrita acima. `chamados/tests/test_e3_volta7_sem_banco.py:52`, `chamados/tests/test_e3_r3_c5_sem_banco.py:194`, `chamados/tests/test_e3_r3_c6_sem_banco.py:313`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v \
  chamados.tests.test_cef_c1_sem_banco \
  chamados.tests.test_cef_c3_sem_banco \
  chamados.tests.test_e4c_f6f7_c7_sem_banco \
  chamados.tests.test_e3_volta7_sem_banco \
  chamados.tests.test_e3_r3_c5_sem_banco \
  chamados.tests.test_e3_r3_c6_sem_banco \
  chamados.tests.test_e3_r3_c11_sem_banco \
  chamados.tests.test_e3_degrau_dono_e_porta \
  chamados.tests.test_e3_portas_v6_sem_banco \
  chamados.tests.test_e4b_classe_e_memoria

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_disputa_s127 \
  chamados.tests.test_e4b_c4v14_memoria \
  core.tests.test_medida_turno_do_par \
  ponto.tests.test_supra_juiz_executor
```

**Foto.** O primeiro comando remove o método em memória e reproduz o RED de protocolo esperado. O segundo usa o mutante comportamental: retirar o motivo enquanto a irmã continua viva deve falhar no `assertIn`. `api/foto_recebida.py:81`, `chamados/tests/test_cef_c1_sem_banco.py:236`, `chamados/tests/test_cef_c1_sem_banco.py:242`, `chamados/tests/test_cef_c1_sem_banco.py:309`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -c '
import unittest
from chamados.tests import test_cef_c1_sem_banco as m
del m.Consulta.prefetch_related
unittest.main(module=m)
' UploadTest.test_T3_foto_tardia_da_primeira_so_extingue_primeira

CEF_C1_MUTANTE=upload_arranca_motivo PYTHONDONTWRITEBYTECODE=1 \
  python3 -m unittest -v \
  chamados.tests.test_cef_c1_sem_banco.UploadTest.test_T3_foto_tardia_da_primeira_so_extingue_primeira
```

**Beco.** Esta sonda usa em memória o corpo da B intermediária para isolar o RED anexado. Roda `test_beco` com o modelo falso atual e depois sem o export de `DecisaoPergunta`. O segundo caso deve dar `ImportError` nesse nome. A asserção de negócio continua no teste de origem. `chamados/tests/e3_volta7_apoio.py:106`, `chamados/tests/test_e3_volta7_sem_banco.py:52`, `escala/tests/test_f3_beco1_folga_na_lista.py:248`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import subprocess
import sys
import unittest
from contextlib import contextmanager
from unittest.mock import patch

from chamados.tests import test_e3_volta7_sem_banco as teste [nao verificado]
from chamados.tests.e4_catracas import FONTES_MUTADAS

arquivo = 'escala/services/cadastro_realidade.py'
fonte_b = subprocess.check_output(
    ['git', 'show', 'd0210a400:app/' + arquivo], text=True)
cenario_real = teste.cenario [nao verificado]

@contextmanager
def sem_compositor(caso):
    with cenario_real(caso) as contexto:
        del sys.modules['chamados.models'].DecisaoPergunta
        yield contexto

def rodar():
    caso = teste.SelosAnexadosTest('test_beco') [nao verificado]
    return unittest.TextTestRunner(verbosity=2).run(caso)

with patch.dict(FONTES_MUTADAS, {arquivo: fonte_b}):
    controle = rodar()
    with patch.object(teste, 'cenario', sem_compositor): [nao verificado]
        red = rodar()

raise SystemExit(0 if controle.wasSuccessful() and len(red.errors) == 1
                 and 'ImportError: cannot import name' in red.errors[0][1]
                 and 'DecisaoPergunta' in red.errors[0][1] else 1)
PY
```

**Âncoras C6 e API.** Os dois comandos C6 ficam bloqueados pelo import de `pergunta_respondida` no controle da B2; o próprio runner exige controle verde antes de aplicar cada mutante. O último comando executa o mutante interno da lista vazia da API. `chamados/tests/e3_r3_c6_mutantes.py:48`, `chamados/tests/e3_r3_c5_mutantes.py:76`, `chamados/tests/test_e4c_f6f7_c7_sem_banco.py:125`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_r3_c6_mutantes K6_m1_sem_respondida
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_r3_c6_mutantes K6_m1_ignora_ancora
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v \
  chamados.tests.test_e4c_f6f7_c7_sem_banco.CosturasSemDjango.test_reflexao_e_mutacao_de_perguntas_MORDEM
```

`LEI-AKITA | padrao=n/a (instrumento) | classes=i,ii,v | execução e certificação [nao verificado]`

## 20/11 00:22 · sessao 8aca83b0e978 · TRAVAS-SEM-MEMORIA · verificador · origem=Code

# AVAL-DEVOLVIDO — TRAVAS-SEM-MEMORIA-2011-0020

## 1. PREMISSAS — PROCEDE

- **(a) Hooks do Code.** Na cópia, `.claude/settings.json` da raiz aponta para `/home/ronald/saas-hasner/bin/`. `Stop` chama `hook_stop_fila1.py`. `PreToolUse` chama `disjuntor.py pretool`, que recebe o JSON no stdin e lê o comando Bash em `tool_input.command`. O `tool_input.prompt` que ele lê pertence à ferramenta `Agent`. O evento previsto por T2 para o texto colado pelo usuário é `UserPromptSubmit`, sem registro nessa configuração. `PreCompact` manual/auto chama `handoff_sessao.sh`. `SessionStart` compact/clear imprime o handoff e chama `marco_de_clear.py`. A `statusLine` chama `statusline_cota.py`. O RELATO registra uma sessão anterior em `app/` em que a configuração da raiz ficou inerte e só disparou o Stop de `~/.claude/settings.json`. Configuração efetivamente carregada hoje na VM **[nao verificado]**. `.claude/settings.json:2–77`, `bin/disjuntor.py:1837–1843,1864–1867,1912–1923`, `bin/hook_stop_fila1.py:1339–1345`, `app/docs/RELATO.md:3341`.
- **(b) “`bin/suite.sh` (pista 2)” não fecha.** O script da cópia roda Docker local sob `bin/trava_teste.sh`. A **PISTA-2** documentada é o executor remoto no servidor do GLPI, com fila própria e entrada `ssh pista pede` para uma árvore commitada. A escolha de rota dentro de `bin/suite.sh` aparece como obra separada, com opção A registrada para `--volta` e pre-opus de lateral. Estado atual do executor remoto **[nao verificado]**. `bin/suite.sh:316–325`, `app/docs/BACKLOG.md:2191,2193–2195`, `app/docs/RELATO.md:65`.
- **(c) “Por cópia em obra” precisa de universo definido.** `bin/relato.sh` já gera `ESTADO.md` por `bin/gerar_estado.py`, passa pela raspagem e publica. O encaixe de `copia=<nome>@<HEAD12>` é a seção **Raias**, que já usa `bin/raias_estado.py`. Esse medidor cruza `git worktree list --porcelain` e declarações `.agente`, inclui as declaradas, as à frente de `main` e as sujas, exclui a principal e as worktrees do harness, e agrupa as só sujas. Uma declaração pode aparecer sem caminho no disco. `wt-codex-*` é a convenção do construtor Codex. O BACKLOG registra a ordem da fila. O medidor é a fonte existente para as raias do ESTADO, mas a equivalência entre esse universo e **todas** as cópias em obra fica **[nao verificado]**. `bin/relato.sh:34–38,114–117,145–147`, `bin/gerar_estado.py:211–229,329,378–380`, `bin/raias_estado.py:13–27,73–135`, `bin/codex_pergunta.sh:217–232`, `bin/hook_stop_fila1.py:1394–1403`.
- **(d) “Suite completa rodada na VM (fora do `bin/suite.sh`)” abrange chamadas legítimas.** A régua inteira e o pre-push rodam fora de `suite.sh` por exceção expressa do §3 e pegam a trava diretamente. Ambos entram no alcance literal de T1, embora o hook de ferramenta só veja a chamada externa e a negativa efetiva dependa da implementação. A volta intermediária chama `suite.sh --volta`, com ruff e selo do disjuntor. Ela fica livre enquanto a seleção for um recorte efetivo. A guarda atual considera inteira a seleção vazia ou a que cobre todos os módulos de teste reconhecidos, mesmo quando os módulos são nomeados em `--only`. Uma inteira com `--dir … --motivo "pre-opus OBRA"` é válida hoje e cairia na segunda recusa de T1 se faltasse `PRONTO`. Se o argumento for literalmente `--motivo "pre-opus"`, já sai `rc 2` por faltar a obra. `CLAUDE.md:95–106`, `bin/regua.sh:47–51,165–169`, `bin/pre-push.sh:142–145,184–198`, `bin/volta_intermediaria.sh:18–33`, `bin/suite.sh:215–263`.
- **(e) “A linha ‘PRONTO item a item’ dessa obra” não tem formato demonstrado.** O RELATO traz prosa com formas como `CADEIA-20 PASSO 1 PRONTO (…)`, `PRONTO lista de repasse (…)` e `PRONTO (a) E3-NA-PROXIMA-VAGA …`. Alguns exemplos identificam a obra, mas o ID ocupa posições diferentes. Um formato canônico que permita ao hook casar a obra inteira, a volta e a prova exigida está **[nao verificado]**. `app/docs/RELATO.md:254,775,1753`, `CLAUDE.md:439–442`.

## 2. COLISÃO COM AVAL OU REGRA VIGENTE — PROCEDE

- **T1 — “SUITE CHEIA SO PELA PISTA 2” e “só lança se o RELATO tem […] PRONTO”.** A primeira frase conflita com as portas próprias da régua e do pre-push e, se “pista 2” for o executor remoto, com a destinação vigente de pouso e pre-opus da espinha à VM. A regra `TRAVA-MENOS-FILA-COM-SELO` já exige `pre-opus <OBRA>` ou `pouso <OBRA>` para a inteira em `suite.sh`, com `rc 2` na recusa. O novo `PRONTO` também precisa preservar a saída após mais de três voltas Codex, pois a regra manda subir ao opus e exige a inteira verde antes dele. O RELATO registra a retirada de uma guarda anterior que fechava essa saída. `CLAUDE.md:95–106,372–381`, `bin/suite.sh:236–263`, `app/docs/RELATO.md:65,1010,1209`, `app/docs/BACKLOG.md:2089`.
- **T2 — “devolve […] e não executa”.** Recusar apenas o aval inválido é compatível com seguir a fila. Se “devolve” encerrar o turno enquanto houver outro item que possa andar, colide com PARADA. O `PAREI` do Stop exige `!` no item em curso e considera os demais itens andáveis. A regra também prevê `ESPERANDO` com prova. A semântica da recusa de T2 no cliente fica **[nao verificado]**. `CLAUDE.md:262–275,296–298`, `bin/hook_stop_fila1.py:1232–1245,1260–1276,1381–1403`.
- **CODEX, contrato e pouso.** Preservar o wrapper, o despacho e as `#G` é compatível com o aval. A configuração e o código do instrumento cabem à sessão, com verificação pelo Codex. “REGISTRAR […] + CONSTRUIR” continua sujeito às cinco linhas no RELATO **antes** do código — quem chama, quem espera, falha/limite, medida e quem desliga — e ao envio único ao Codex. O contrato desta obra está **[nao verificado]**. O instrumento tem pouso próprio, conforme RAIAS item 5. `CLAUDE.md:325–329,344–358,394`, `app/docs/PISTA.md:169–176`, `bin/codex_pergunta.sh:122–135,241–254`.
- **Retenção.** A regra manda arquivar conteúdo com mais de três dias no mesmo commit. Uma linha `PRONTO` arquivada pode desaparecer do RELATO vivo antes da suite. T1 não define como a prova continua válida depois dessa transferência. `CLAUDE.md:439–442`, `bin/relato.sh:114–131`.

## 3. CONDIÇÃO QUE NÃO FECHA NO LITERAL — PROCEDE

- **“Suite completa rodada na VM (fora do `bin/suite.sh`)”.** T1, como hook do Code, alcançaria o lançamento avulso que o Code envia à ferramenta Bash e que o hook consiga classificar como inteiro. O evento vê o comando externo. Em `git push`, por exemplo, o pre-push roda como hook filho do Git. Na pista remota, a entrada documentada recebe SHA e opções, sem `--motivo` ou ID explícito da obra para casar com o `PRONTO`. Cobertura de toda a VM ou da pista pelo hook local **[nao verificado]**. `bin/disjuntor.py:1794–1798,1837–1843`, `bin/hooks.sh:10–11,61–65`, `bin/trava_teste.sh:35–42`, `app/docs/BACKLOG.md:2191`.
- **“Todo aval colado que diga CONSTRUIR” e “ou `copia=nova`”.** O anexo não define como reconhecer uma ordem efetiva de construção em meio a citações, proibições ou itens só de registro. Também falta a regra que associa cada item à sua cópia quando há várias, que distingue uma abertura real de um `copia=nova` escrito no texto, e que fixa o marco para reconhecer avais anteriores ao pouso. O RELATO já registra uma obra com duas cópias descendentes. Para instrumento sem worktree nomeada, falta definir que cópia e HEAD o aval deve citar. A regra vigente já exige preparar a mudança em cópia do HEAD e aplicar no commit. `app/docs/RELATO.md:1513`, `CLAUDE.md:37–40,309–313,436–438`, `.claude/settings.json:2–73`.
- **“`<HEAD 12>`” comparado com “`git rev-parse` da cópia”.** Igualdade textual entre 12 caracteres e o hash completo recusa um HEAD atual. O aval precisa fixar a normalização e a identidade da cópia. O wrapper já distingue o hash completo usado na guarda do prefixo de 12 usado na trilha, e confere a limpeza da cópia separadamente. Um HEAD igual, por si, deixa alterações locais fora da conferência. `bin/codex_pergunta.sh:79,252–259,304–308,409–415`, `bin/raias_estado.py:100–102`.
- **“Smoke dos 6 casos no ar” e “teste das duas travas na suite”.** O anexo enumera três resultados de T1 e quatro de T2, além da publicação no ESTADO, mas não identifica quais resultados compõem os seis casos nem onde os hooks devem estar ativos para a prova. Se o `PRONTO` exigido para o pre-opus da própria obra já incluir o smoke da configuração ativa, “pouso sem smoke” deixa a ordem de ativação e certificação indefinida. Hoje os selos de host são chamados pela régua, enquanto `suite.sh` executa o runner Django. O RELATO registra um precedente de hook validado com selos de host e configuração ativa, sem deploy do produto. Resultados dos quatro casos pré-cura e smoke na VM **[nao verificado]**. `bin/regua.sh:134–145`, `bin/suite.sh:316–325`, `app/docs/RELATO.md:3341,4356`.

## 4. EXPORTADA, DINHEIRO, MOTOR V2 OU `!` FORA DO DECLARADO — NAO PROCEDE

O escopo declarado é hook do Code e publicação do ESTADO. O anexo não pede alteração do gravado de exportada, apply de dinheiro, mudança em `ponto/motor_calculo_v2.py`, atalho ou outro ato da PARADA item 2. Configuração do Code **é tocada** para registrar o hook de prompt. A vedação de CODEX se aplica ao Codex, inclusive no papel construtor. Há precedente de configuração feita como instrumento, e também de instalação negada à sessão pelo classificador. O `PreToolUse` atual protege contra remover o próprio disjuntor. Permissão efetiva e carregamento da configuração nesta VM **[nao verificado]**. `CLAUDE.md:141–142,265–268,344–355,406–417`, `.claude/settings.json:62–73`, `bin/codex_pergunta.sh:122–135`, `bin/disjuntor.py:218–245`, `app/docs/RELATO.md:4356,4411`.

A posição nas vagas do `ABRE-AS-LATERAIS-1911-2340` e a lista atual de cópias na VM ficam **[nao verificado]**.

**BLOQUEIA=3 NAO_BLOQUEIA=1**

## 20/11 00:17 · sessao ce1b950b7668 · E4C-F5-CONSTROI · construtor · origem=Code

Implementei a volta B2 em seis arquivos. Conferi compilação em memória, `git diff --check`, os asserts preexistentes e as âncoras dos mutantes. Executei também o predicado isolado do M1 com os juízes reais em memória. Resultados com ORM, suíte e certificação pelo Code e pelo Opus final [nao verificado]. `escala/services/cadastro_realidade.py:497`, `chamados/tests/e4c_f5_voltaB_mutantes.py:28`.

## Arquivos tocados

- Produção — `api/views.py:2566`, `chamados/services/validacao.py:914`, `escala/services/cadastro_realidade.py:497`.
- REDs e mutantes — `chamados/tests/test_e4c_f5_voltaB_sitios.py:66`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:182`, `chamados/tests/e4c_f5_voltaB_mutantes.py:28`.

## Antes e depois

| Item | Base `d0210a400` | B2 |
|---|---|---|
| **1 · GET** | O juiz de `respondidas` recebia a composta e `_item` publicava o instante da coluna. `api/views.py:2581`, `:2597` na base. | **Sim, `perguntas` e `historico` usam o mesmo `_item`.** A troca é coerente para os três. `_item` recebe a composta e publica seu par, com `respondida_em` em `isoformat()` ou `None`. O juiz de `respondidas` recebe a mesma instância de leitura usada para montar o item reutilizado do histórico. `api/views.py:2566`, `:2579`, `:2581`, `:2586`, `:2601`. |
| **2 · Recusa** | A foto do evento usava o instante composto e a linha imutável fotografava a coluna. `chamados/services/validacao.py:914` na base, `chamados/services/disputa_emissao.py:1640`, `:1643`. | A composta alimenta só o juiz. A foto do evento lê as colunas antes da reabertura e serializa o instante em `isoformat()` quando presente, preservando `None`. `chamados/services/validacao.py:902`, `:913`, `:926`, `:935`. |
| **3 · M1** | Testava só o instante recortado, pelo índice `[1]`. `escala/services/cadastro_realidade.py:502` na base. | Usa `projecao_temporal_da_pergunta(p, agora=None)` com o `agora` do M1 e entrega o par recortado a `pergunta_respondida(p)` pelo `_replace` da composta. O corte limpa instante e texto quando `respondida_em > agora` e conserva a igualdade. O juiz aplica instante **e** texto não vazio, conforme a L-130. `escala/services/cadastro_realidade.py:497`, `:508`, `chamados/juizes.py:1383`, `:1397`, `:1551`, `:1560`, `docs/LEIS.md:151`. |
| **4 · REDs e K46** | O GET conferia ID e texto, a recusa conferia o estado posterior, e a medição do K46 compartilhava PKs com o K45. `chamados/tests/test_e4c_f5_voltaB_sitios.py:73`, `:118`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:165` na base. | O GET exige o instante vigente do ato. A recusa compara o evento persistido com a linha `RECUSA` para coluna nula e para coluna com instante divergente. O M1 cobre texto vazio, texto presente no limite e instante futuro. O novo K46 compara 3 × 6 perguntas sem validação, fora da janela do K45 e ligadas ao chamado só pelo destino. O conjunto de PKs é próprio do K46. Com `ficam = []`, `fica_pergunta_viva_fora` perde o chamado e a asserção deve falhar. `chamados/tests/test_e4c_f5_voltaB_sitios.py:78`, `:132`, `:276`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:40`, `:182`, `chamados/services/competencia_trancada.py:119`, `:164`, `:175`. |
| **5 · Asserts e tetos** | Contagem AST de `assert` e `self.assert*`. | Sítios **37 → 44**, lotes **24 → 27**, mutantes **2 → 3**. Todos os anteriores têm a mesma expressão AST. A igualdade de SELECTs 3 × 6 continua com o mesmo limite. `chamados/tests/test_e4c_f5_voltaB_sitios.py:66`, `:116`, `:276`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:40`, `:182`, `chamados/tests/e4c_f5_voltaB_mutantes.py:75`. |

## Quem escreve e quem lê

| Campo | Escrita e significado nos sítios tocados |
|---|---|
| `respondida_em` | As portas humana e automática gravam a coluna e registram o instante na linha `RESPOSTA`. A reabertura limpa a coluna pela porta de lote. GET, juiz da recusa e M1 leem o instante **vigente** da composta, com corte adicional no M1. A foto da recusa e a linha imutável leem o instante **gravado na coluna antes do ato**. Esses dois significados podem divergir. `chamados/services/disputa_emissao.py:1528`, `:1532`, `:1683`, `:1686`, `:1617`, `:1656`, `chamados/models.py:4339`, `:4358`, `api/views.py:2581`, `chamados/services/validacao.py:902`, `:914`, `escala/services/cadastro_realidade.py:498`. |
| `resposta_colab` | As portas humana e automática escrevem o texto da projeção e a reabertura o limpa. GET, juiz da recusa, M1 e foto da recusa partem da **mesma coluna**. No M1, o corte pode limpar o par entregue ao juiz. `chamados/services/disputa_emissao.py:1481`, `:1682`, `:1617`, `:1656`, `chamados/models.py:4358`, `api/views.py:2579`, `chamados/services/validacao.py:902`, `:913`, `escala/services/cadastro_realidade.py:498`, `chamados/juizes.py:1397`. |
| `respondida_original_em` | `reabrir_para_correcao` grava na linha `RECUSA` a coluna anterior ao ato. O RED lê a linha persistida e compara seu instante com `LogAuditoria.valor_antes['respondida_em']`. Ambos representam a mesma foto, em `datetime` na linha e ISO no evento, ou `None` nos dois. `chamados/services/disputa_emissao.py:1639`, `:1643`, `chamados/services/validacao.py:935`, `core/observ.py:133`, `chamados/tests/test_e4c_f5_voltaB_sitios.py:132`. |

## Comandos para o Code

A partir de `app/`. Com os REDs da B2 sobre `d0210a400`, as falhas esperadas são `get` pelo instante nulo, `recusa` pela divergência da foto e `m1` pelo subcaso de texto vazio. Os casos K46 verificam o lote já presente na base e devem cair com os respectivos mutantes. Execução [nao verificado]. `chamados/tests/test_e4c_f5_voltaB_sitios.py:78`, `:138`, `:296`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:179`, `:182`.

```bash
s=chamados.tests.test_e4c_f5_voltaB_sitios
l=chamados.tests.test_e4c_f5_voltaB_lotes.CarregadoresVoltaBTest

get="$s.SitiosVoltaBTest.test_RED_get_do_app_reconhece_instante_do_ato"
recusa="$s.SitiosVoltaBTest.test_RED_recusar_usa_instante_do_ato_e_reabre_sem_reusar_prefetch"
m1="$s.BecoTemporalVoltaBTest.test_RED_m1_exige_texto_e_instante_no_horizonte"
k46="$l.test_RED_k46_julga_cada_pergunta_fora_da_competencia"
k46_lote="$l.test_RED_k46_perguntas_do_chamado_na_competencia"
selo="$s.SeloMutanteVoltaBTest.test_MORDE_get_do_app_com_pergunta_crua"

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  "$get" "$recusa" "$m1" "$k46" "$k46_lote" "$selo"

mutar() {
  local seletor="$1"
  shift
  F5_VOLTA_B_MUTANTE="$seletor" PYTHONDONTWRITEBYTECODE=1 \
    python3 manage.py test "$@"
}

mutar get_instante_da_coluna "$get"
mutar recusa_foto_do_ato "$recusa"
mutar m1_so_instante "$m1"
mutar m1_sem_corte "$m1"
mutar k46_sem_julgamento "$k46"
mutar k46 "$k46_lote" "$k46"
mutar api_p_cru "$selo"
```

As mordidas esperadas são `None` no GET, divergência entre as fotos na recusa, inclusão indevida com texto vazio ou instante futuro no M1, perda do chamado em `fica_pergunta_viva_fora` com `ficam = []`, crescimento dos SELECTs sem o lote K46 e acusação da chamada crua no selo. A acusação do `api_p_cru` foi observada na análise AST em `api/views.py:2590`. `chamados/tests/e4c_f5_voltaB_mutantes.py:21`, `:28`, `:118`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:51`, `:191`.

LEI-AKITA — L-130, padrao=juiz unico puro + fonte unica (compositor). `docs/LEIS.md:151`, `chamados/juizes.py:1551`, `chamados/models.py:4351`.

## 20/11 00:09 · sessao 1ac528925785 · E4C-F5-CONSTROI · verificador · origem=Code

## 1. Escopo — BLOQUEIA

Os **14 chamadores** e os **27 carregadores** declarados aparecem no diff. Os 14 passam uma `LeituraComposta` ao juiz indicado, e os 27 acrescentam o prefetch à consulta que produz as instâncias. Os blobs dos 31 arquivos do diff conferem com a cópia. [nao verificado] para a identidade Git independente do arquivo exportado e para a numeração original da `matriz.md` e do `predesenho.md`, que não vieram como anexos separados.

| Chamador | Sítio conferido |
|---|---|
| GET e POST do app | `app/api/views.py:2597`, `app/api/views.py:2641` |
| `acao_regularizar_disputa` e `classificar` o beco | `app/chamados/services/acoes_disputa.py:134`, `app/chamados/services/becos.py:32` |
| `declarar_hora_colab` e `aplicar_declaracao` | `app/chamados/services/declaracao_texto.py:418`, `app/chamados/services/declaracao_texto.py:688` |
| `validar_pergunta` e `recusar_pergunta` | `app/chamados/services/validacao.py:682`, `app/chamados/services/validacao.py:902` e `:903` |
| As duas listas de `validar_inline` | `app/chamados/views_cobrar.py:170`, `app/chamados/views_cobrar.py:178` |
| Resposta web e ficha do colaborador | `app/chamados/views.py:1275`, `app/colaboradores/services/ficha.py:173` |
| M1 e material da prévia | `app/escala/services/cadastro_realidade.py:502`, `app/folha/previa.py:143` |

| K | Carga conferida |
|---|---|
| 4, 29, 30, 31, 32 | Vivas da disputa, corrigíveis, fila a validar, dias das vivas e disputa do chamado. `app/chamados/juizes.py:1512`, `:1765`, `:1773`, `:2437`, `:2490` |
| 11, 23 | Perguntas do evento do beco por `perguntas__decisoes` e perguntas da janela da prévia. `app/chamados/services/becos.py:87`, `app/folha/previa.py:66` |
| 25, 26, 27 | Fato do marco avulso, fatos da regularização e fato memoizado das propostas. `app/chamados/services/declaracao_texto.py:475`, `:588`, `:635` |
| 44, 45, 46 | Detalhe do chamado, janela da competência e perguntas de cada chamado da medição. `app/chamados/services/acoes_chamado.py:352`, `app/chamados/services/competencia_trancada.py:121`, `:166` |
| 47, 48, 49 | Disputa candidata sem credor, destino da migração e origem da migração. `app/chamados/services/disputa_emissao.py:2080`, `app/chamados/services/fio.py:2296`, `:2300` |
| 50, 53, 54, 55 | Fatos do app, pendentes da reabertura, janela do calendário e dia do colaborador. `app/chamados/services/pergunta_no_app.py:186`, `app/chamados/views.py:1412`, `app/colaboradores/services/calendario.py:505`, `app/ponto/services/dia_do_colab.py:65` |
| 61, 62, 63, 64 | Fallback do explicador, foto após validação, disputa após carimbos e perguntas do dia do filho. `app/chamados/services/explicador.py:161`, `app/api/foto_recebida.py:81`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:84`, `app/chamados/services/limbo_folgas.py:205` |
| 65, 67, 68 | Relação externa de todos os chamados do queryset do supra-juiz, guarda do comando de relavra e dedup do emissor. `app/ponto/management/commands/supra_juiz.py:192` e `:215`, `app/ponto/management/commands/reconciliar_grade.py:461`, `app/chamados/services/disputa_emissao.py:1436` |

K41 e K42 já aparecem carregados em `app/chamados/models.py:1555` e `:2077`. O K40 mantém `.get(pk=pk)` dentro do laço. O saldo **24 C + 6 R** é compatível com a reclassificação descrita no relato. O censo nominal completo das 30 sobras fica [nao verificado]. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:464` e `:466`.

Nenhum sítio de produção nomeado no brief ficou de fora. Os quatro arquivos reservados à C estão fora do diff e suas dez identidades continuam no `RESTAM`. O selo AST passou na cópia. `app/chamados/tests/test_e4c_f5_selo_chamada.py:21` e `:194`.

O trecho fora da lista literal é **`'respondida_em': antes_do_ato.respondida_em.isoformat()`**. Ele muda a fonte da foto de auditoria publicada por `recusar_pergunta`, além de converter a chamada do juiz. A mudança evita o `.isoformat()` sobre `None` no passivo do RED, mas deixa a foto da própria linha `RECUSA` lendo outra fonte. `CLAUDE.md:37`, `app/chamados/services/validacao.py:914` e `:935`, `app/chamados/services/disputa_emissao.py:1640` e `:1643`, `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:110`.

## 2. Cache depois da escrita — NAO BLOQUEIA

Nas oito portas, a leitura composta verifica o estado anterior à escrita de resposta, validação ou recusa. As releituras encontradas usam instância nova, refresh integral, invalidação pela porta de decisões ou leitura exclusivamente escalar. `app/chamados/models.py:4330` e `:4373`.

| Porta | Releitura depois da escrita |
|---|---|
| `validar_pergunta` | `_validar_o_par` faz refresh integral no rollback e no sucesso. A devolução relê escalares, e o refresh final lê só `validada_em` e `materializada_em`. No modal, a instância externa recebe refresh integral antes de `.estado`. `app/chamados/services/validacao.py:646`, `:655`, `:262`, `:576`, `:789`, `app/chamados/services/fio.py:662` e `:663` |
| `recusar_pergunta` | Depois da escrita em lote, a nota e o evento usam a foto anterior já guardada. A resposta do modal monta uma carga nova. O refresh do teste é de outra instância. `app/chamados/services/validacao.py:926`, `:931`, `:935`, `app/chamados/services/fio.py:486` e `:495`, `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:112` e `:118` |
| `validar_inline` | As duas listas são montadas antes de escrever. Cada iteração passa apenas o PK ao escritor, cujo `get()` obtém outra instância. O JSON usa o resultado da porta e a contagem. `app/chamados/views_cobrar.py:170`, `:178`, `:209`, `:215`, `app/chamados/services/validacao.py:677` |
| `responder_pergunta_disputa` | O núcleo grava a decisão com `pergunta=p`, invalidando o cache dessa instância antes de `p.resposta_display`. A view devolve a confirmação. `app/chamados/services/disputa_emissao.py:1532` e `:1568`, `app/chamados/models.py:4376`, `app/chamados/views.py:1278` e `:1282` |
| `api_disputa_responder` | Usa o mesmo núcleo. O JSON lê o instante escalar atualizado em memória. `app/api/views.py:2648` e `:2658`, `app/chamados/services/disputa_emissao.py:1528` e `:1568` |
| `acao_regularizar_disputa` | O materializador relê por PK sob trava e, ao final, recarrega as vivas por um queryset novo. A ação usa o resultado devolvido. `app/chamados/services/acoes_disputa.py:134` e `:143`, `app/chamados/services/materializacao.py:2117`, `:2127`, `:2024`, `app/chamados/juizes.py:1512` |
| `aplicar_declaracao` | Depois da resposta, a validação recebe o PK e faz `get()` novo. `app/chamados/services/declaracao_texto.py:694` e `:697`, `app/chamados/services/validacao.py:677` |
| `declarar_hora_colab` | Há refresh integral depois da costura, antes da guarda de resposta. Depois da resposta, a releitura de exibição é a do núcleo já invalidado. A declaração anexa a nota e devolve o PK. `app/chamados/services/declaracao_texto.py:371`, `:373`, `:418`, `:420`, `:424`, `app/chamados/services/disputa_emissao.py:1568` |

O K11 também relê `d` integralmente depois dos resolvers e antes de `d.pode_fechar()`. No K47, o fecho final faz refresh e consulta as perguntas de novo. `app/chamados/services/becos.py:58` a `:61`, `app/chamados/services/disputa_emissao.py:2127` e `:2134`.

## 3. Escritores, leitores e significado — BLOQUEIA

| Campo ou retorno | Escrita e leitura no percurso |
|---|---|
| `validada_em` | A validação grava, as devoluções limpam, a resposta humana e a reabertura limpam, `carimbar` pode preencher e a ressuscitação limpa em lote. `app/chamados/services/validacao.py:168`, `:273`, `:349`, `app/chamados/services/disputa_emissao.py:1494` e `:1617`, `app/chamados/models.py:3352` e `:1721`. C6, `estado` e respostas sem veredito leem a validação vigente do ato. As guardas de validar, recusar, declarar/aplicar e a seleção S128 ainda leem o carimbo da coluna. `app/chamados/juizes.py:1408`, `app/chamados/models.py:3256` e `:2025`, `app/chamados/services/validacao.py:685` e `:898`, `app/chamados/services/declaracao_texto.py:228`, `app/chamados/services/disputa_emissao.py:2085`. |
| `respondida_em` | Resposta humana, automática e reconstituição pela trilha gravam o instante da operação respectiva. A porta de carimbo pode completar o instante legado. Reabertura, ressuscitação e devolução L-130 zeram o par. `app/chamados/services/disputa_emissao.py:1528`, `:1683`, `:1705`, `:1617` e `:1656`, `app/chamados/models.py:3349` e `:1722`, `app/chamados/services/validacao.py:440`. Os chamadores da B recebem o instante vigente composto. A página do app, faces SQL, ordenação da fila e foto imutável da recusa ainda leem a coluna. `app/api/views.py:2581`, `app/chamados/juizes.py:1588`, `:1599`, `:1774`, `app/chamados/services/disputa_emissao.py:1640` e `:1643`. |
| `resposta_colab` | As portas humana e automática escrevem o texto. As portas que limpam o par zeram o texto junto. O compositor, o texto do GET, o dia do colaborador, o explicador e a igualdade da declaração usam a projeção. A exibição formatada e a tarefa do dia têm leitores preexistentes do texto do ato. `app/chamados/services/disputa_emissao.py:1481`, `:1682`, `:1617`, `app/chamados/models.py:1722`, `:4358`, `:3837`, `app/api/views.py:2579` e `:2580`, `app/ponto/services/dia_do_colab.py:70`, `app/chamados/services/explicador.py:102`, `app/chamados/services/declaracao_texto.py:418`, `app/chamados/services/tarefa_do_dia.py:165`. |
| `materializada_em` | `_escrever_carimbo`, suas portas e o descarimbo em lote mantêm o instante de encerramento registrado. C6, o compositor, as guardas e o materializador leem a coluna. Lastro é um julgamento à parte. `app/chamados/models.py:3316`, `:3346`, `:3400`, `:3425`, `:3508`, `:4359`, `app/chamados/juizes.py:1399` e `:1455`, `app/chamados/services/disputa_emissao.py:2085`, `app/chamados/services/materializacao.py:2107`. |
| `via_resolucao` | As portas de carimbo e descarimbo escrevem a via. A confirmação pelo espelho e `relavrar` também podem trocar só a via. Leem-na o compositor, C6, a identidade do fato, S128, o comando de encerramento e o explicador. `app/chamados/models.py:3318`, `:3508`, `:3517`, `:3418`, `:4359`, `app/chamados/services/relavra_via.py:125`, `app/chamados/juizes.py:1401`, `:1421`, `:1449`, `app/chamados/services/disputa_emissao.py:2095`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:34`, `app/chamados/services/explicador.py:118`. |
| `decisoes`, `leitura()` e `composta()` | A FK é `decisoes`, ordenada por `ato_em, pk`. `registrar()` insere as linhas das portas e invalida o cache da instância passada. `leitura()` incorpora o passivo sem linha correspondente e devolve validação, instante e texto vigentes do ato. `composta()` usa os dois instantes dessa leitura, o texto da projeção e as colunas de materialização e via. C6 e os 14 chamadores acima usam a composição. Histórico da tela, tarefa, auditoria e backfill também leem a sequência para seus próprios fins. `app/chamados/models.py:4188`, `:4222`, `:4273`, `:4330`, `:4348`, `:4358`, `:4372`, `:3823`, `app/chamados/juizes.py:1408`, `app/chamados/services/historico.py:98`, `app/chamados/services/tarefa_do_dia.py:429` e `:545`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:342`. |

Os carimbos são `DateTimeField`, com `USE_TZ=True` e fuso configurado `America/Sao_Paulo`. `data_evento` é uma data, com dia do turno para pergunta de fato e dia civil para conversa. O corte temporal mantém a igualdade com `agora`, pois descarta `> agora`. `leitura()` roda a sequência inteira antes desse corte. A tarefa do dia, quando pede histórico no horizonte, limita as linhas a `ato_em <= agora` antes da leitura. `app/chamados/models.py:2630`, `:2654`, `:3668`, `:4327`, `app/config/settings/base.py:93` e `:95`, `app/chamados/juizes.py:337` e `:1394`, `app/chamados/services/tarefa_do_dia.py:400` e `:429`.

**Divergências que bloqueiam**:

- **GET do app:** `if _pergunta_respondida(DecisaoPergunta.composta(p))` põe na lista `respondidas` a pergunta com instante vigente no ato e texto na projeção. O mesmo item sai com **`'respondida_em': p.respondida_em.isoformat() if p.respondida_em else None`**. O caso do RED devolve `null` nessa chave. O efeito no consumidor APK e a incidência em produção ficam [nao verificado]. `app/api/views.py:2581` e `:2597`, `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:39` e `:64`.
- **Validação e aplicação:** `if not _pergunta_respondida(DecisaoPergunta.composta(perg))` deixa passar o par com instante no ato e texto na projeção. Na mesma operação, `_recusa_sem_anuencia` usa **`if pergunta_respondida(p):`** sobre a instância crua. Para uma resposta de hora com coluna de instante vazia, a validação pode carimbar e encaminhar uma resposta que o materializador recusa como “sem resposta do colaborador”. `app/chamados/services/validacao.py:682`, `:743`, `:775`, `app/chamados/services/declaracao_texto.py:688` e `:697`, `app/chamados/services/materializacao.py:1300` e `:1355`.
- **Recusa:** o evento guarda **`antes_do_ato.respondida_em.isoformat()`**, enquanto a linha imutável guarda **`respondida_original_em=_resp`** obtido da coluna. No passivo montado pelo RED, a foto do evento tem o instante e a foto da linha tem `None`, para a mesma recusa. `app/chamados/services/validacao.py:914` e `:935`, `app/chamados/services/disputa_emissao.py:1640` e `:1643`, `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:110`.

## 4. Classes que já reprovaram obra — BLOQUEIA

| Classe | Veredito |
|---|---|
| **a. Segundo juiz** | Os chamadores delegam aos juízes existentes. O M1 mantém um predicado local divergente para “respondida”, descrito em **d**. `app/docs/JUIZES.md:38`, `:102`, `app/chamados/juizes.py:1551`, `app/escala/services/cadastro_realidade.py:502`. |
| **b. Segundo escritor** | A B troca insumos de guardas e da foto de auditoria. A gravação segue pelas portas existentes, inclusive na recusa. `app/chamados/services/validacao.py:923` e `:926`, `app/chamados/services/declaracao_texto.py:694`, `app/chamados/models.py:4362`. |
| **c. Relógio fora da borda** | Nenhuma leitura nova de relógio no diff. O M1 repassa `agora` e o GET conserva o relógio da borda. O fallback avulso `date.today()` já existe no C6. `app/escala/services/cadastro_realidade.py:449` e `:502`, `app/api/views.py:2552`, `app/chamados/juizes.py:597`. |
| **d. Derivação paralela** | **BLOQUEIA.** No M1, **`projecao_temporal_da_pergunta(DecisaoPergunta.composta(p), agora)[1] is not None`** decide “respondida” só pelo instante. O compositor fornece o texto, mas o predicado o descarta. A divergência passa a valer também para o instante recuperado do ato. `app/escala/services/cadastro_realidade.py:500` e `:502`, `app/chamados/juizes.py:1560`. |
| **e. RED ou selo sem mordida** | **BLOQUEIA nos pontos abaixo.** O filtro conta apenas SELECT cujo primeiro `FROM` é `chamados_decisaopergunta`. A igualdade dos conjuntos confirma que os 3 e 6 PKs chegaram ao compositor em cada percurso. Ela não atribui as chamadas a um carregador específico. `app/chamados/tests/test_e4c_f5_opus_models.py:25`, `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:40` a `:52`. |
| **f. Ordem de trava** | K48/K49 leem depois da trava global, K47 depois da trava do resolver. Os prefetches são SELECTs comuns. O K11 já carregava `perguntas` antes do ato e agora carrega também as decisões. `app/chamados/services/fio.py:2252`, `:2264`, `:2277`, `:2296`, `:2300`, `app/chamados/services/disputa_emissao.py:1914` e `:2080`, `app/chamados/services/becos.py:43`, `:53`, `:87`. |
| **g. Campo com dois significados** | A B conserva os tipos dos campos. O GET e a recusa passam a publicar versões diferentes de um mesmo instante lógico, conforme o item 3. `app/api/views.py:2581` e `:2597`, `app/chamados/services/validacao.py:914`, `app/chamados/services/disputa_emissao.py:1640`. |
| **h. Manager ou SQL proibido** | Os acréscimos estão nos querysets locais. A subconsulta de validação já entra por `q_vivas()` na fila e por `q_nao_validada_pelo_ato()` no comando existente. A B acrescenta prefetch a essas cargas. `app/chamados/juizes.py:1772` e `:1773`, `app/chamados/models.py:3560` e `:3634`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:33` e `:84`. |

Os dois pontos exatos de **4e**:

- O RED do GET compara apenas **`(p['id'], p['resposta_colab'])`** e fica verde com `respondida_em: null`. O RED da recusa verifica **`(depois.respondida_em, depois.resposta_colab)`** numa instância que o próprio teste refresca, sem conferir `respondida_original_em` na linha gravada. `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:73`, `:118`, `:120`, `app/chamados/services/disputa_emissao.py:1640`.
- No K46, **`{c.args[0].pk for c in composta.call_args_list}`** aceita os mesmos PKs julgados pelo K45 e pela guarda de fecho. Com a fixture toda validada, substituir o julgamento K46 por `ficam = []` conserva também `resultado['chamados'] == [chamado.pk]`. O RED de remoção do prefetch pode morder sem que o teste prove, isoladamente, a passagem de cada PK pelo K46. `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:49`, `:165` a `:176`, `app/chamados/services/competencia_trancada.py:144`, `:164` a `:178`.

As 14 âncoras dos mutantes de lote apontam para os prefetches reais. K45/K46 escolhem ordinais distintos de `medir`, K47 escolhe `:2080` e preserva `:1955`, e K11 volta a `'perguntas'`. O mutante `api_p_cru` acusou `app/api/views.py:2597` no selo executado em memória. As falhas dos mutantes ORM na asserção de custo ficam [nao verificado]. O 28/30 na base e o 30/30 na B são os dados agregados fornecidos pela sessão. `app/chamados/tests/e4c_f5_voltaB_mutantes.py:12`, `:31`, `:46`, `:59`, `:63`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:194`.

## 5. Adaptações de teste — NAO BLOQUEIA [nao verificado]

| Arquivo preexistente | Classe e contagem `assert` / `self.assert*` |
|---|---|
| `app/chamados/tests/test_e3_r3_c5_sem_banco.py:180` | **ii**, compositor do helper existente no stub. **34 → 34**. `app/chamados/tests/duble_pergunta.py:12` |
| `app/chamados/tests/test_e3_r3_c6_sem_banco.py:299` | **ii**, mesmo helper. **52 → 52**. `app/chamados/tests/duble_pergunta.py:12` |
| `app/chamados/tests/test_disputa_s127.py:23` | **v**, `Linhas` preserva a lista com `prefetch_related` identidade. **8 → 8**. `app/chamados/tests/test_e4b_classe_e_memoria.py:165` e `:193` |
| `app/chamados/tests/test_e4c_f5_selo_chamada.py:21` | Alteração nominal pedida no brief, de **24 → 10** identidades em `RESTAM`. **12 → 12**. A asserção da catraca conserva o mesmo corpo. `app/chamados/tests/test_e4c_f5_selo_chamada.py:194` |

Os hunks dos quatro arquivos preservam os asserts e os valores esperados de negócio. Nenhum teto foi editado no delta, inclusive o limite O261 de 9 por candidata e os quatro tetos da batida. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:207`, `app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`, `:228`, `:235`.

## 6. Leis — BLOQUEIA

- **L-130:** o trecho **`projecao_temporal_da_pergunta(DecisaoPergunta.composta(p), agora)[1] is not None`** mantém a segunda definição de “respondida” no M1. A lei exige instante **e** texto após trim. No fluxo da validação, a guarda composta da B e a guarda crua da materialização dão respostas diferentes ao mesmo par divergente, conforme o item 3. `app/docs/LEIS.md:151`, `app/escala/services/cadastro_realidade.py:502`, `app/chamados/services/validacao.py:682`, `app/chamados/services/materializacao.py:1300`.
- Os demais cortes examinados conservam seus pontos de decisão. A L-051 continua tratando tranca por leitura, a L-050 continua sem fronteira no dedup, e o recorte do fio continua delegado a `pergunta_viva`. O prefetch do calendário fica na guarda de perguntas, e a foto continua validando pela porta do dono. `app/docs/LEIS.md:91` e `:92`, `app/docs/CORTES.md:64`, `:82`, `:94`, `app/chamados/services/competencia_trancada.py:131` e `:203`, `app/chamados/services/disputa_emissao.py:1429` e `:1436`, `app/chamados/juizes.py:1526`, `app/colaboradores/services/calendario.py

[truncado em 20000 de 23005 caracteres]

## 19/11 23:42 · sessao 9535a0a5c084 · E4C-F5-CONSTROI · construtor · origem=Code

## E4C-F5-CONSTROI · Volta B

Implementei os **14 chamadores** e os **27 carregadores** da B. K41 e K42 já estavam em lote na base. A varredura AST mediu a catraca de chamadas em **24 → 10**, com zero chamadas cruas fora da lista restante e da permissão nominal do placar. O mutante do GET do app voltou a acusar `api/views.py:2597`. `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `:35`, `:194`, `chamados/models.py:1555`, `:2077`.

A verificação local ficou em compilação em memória, `git diff --check` e análise AST. **RED na base, verde na B, crescimento dos mutantes de lote, paridade da prévia de novembro e os quatro números do chokepoint estão [nao verificado].** `chamados/tests/test_e4c_f5_voltaB_lotes.py:40`, `chamados/tests/test_e4c_f5_voltaB_sitios.py:64`, `ponto/tests/test_selo_chokepoint_escrita.py:435`.

## Chamadores diretos

As linhas indicam **base → B**. Em cada linha, o juiz passa de uma instância ORM para `DecisaoPergunta.composta(...)`. A projeção temporal de M1 conserva o argumento `agora`. Na recusa, a leitura recebe um nome e também fornece o instante à foto de auditoria, de modo que a foto funcione quando a coluna está vazia. `chamados/services/validacao.py:902`, `:913`, `escala/services/cadastro_realidade.py:502`.

| Sítio | Base → B | Carga |
|---|---|---|
| `api_disputa_perguntas` | `api/views.py:2596 → :2597` | App com lote já na base, `chamados/services/pergunta_no_app.py:57` |
| `api_disputa_responder` | `api/views.py:2640 → :2641` | Avulsa, `api/views.py:2632` |
| `acao_regularizar_disputa` | `chamados/services/acoes_disputa.py:134 → :134` | K4 |
| `classificar` | `chamados/services/becos.py:31 → :32` | K11 no evento, K69 no comando da C |
| `declarar_hora_colab` | `chamados/services/declaracao_texto.py:418 → :418` | Reuso via K17 ou abertura, `:251`, `:377` |
| `aplicar_declaracao` | `chamados/services/declaracao_texto.py:688 → :688` | Reuso via K17 ou abertura, `:663`, `:667` |
| `validar_pergunta` | `chamados/services/validacao.py:682 → :682` | K20, avulsa |
| `recusar_pergunta` | `chamados/services/validacao.py:902 → :903` | Avulsa, `:894` |
| `validar_inline`, pendentes | `chamados/views_cobrar.py:170 → :170` | K4 |
| `validar_inline`, aguardando | `chamados/views_cobrar.py:178 → :178` | K4 |
| `responder_pergunta_disputa` | `chamados/views.py:1275 → :1275` | Avulsa, `:1239` |
| `ficha_do_colab` | `colaboradores/services/ficha.py:172 → :173` | App com lote já na base, `chamados/services/pergunta_no_app.py:57` |
| `beco_em_lote` | `escala/services/cadastro_realidade.py:502 → :502` | K2, coberto na A, `chamados/juizes.py:1665` |
| `_material` | `folha/previa.py:142 → :143` | K23. O import local entrou junto da expressão autorizada em `:142` |

## Carregadores e arquivos tocados

Cada linha abaixo troca a carga anterior pela mesma consulta com `prefetch_related('decisoes')`, salvo as duas relações externas indicadas e os dois sítios já cobertos pelo Opus. Em `fio_vivo`, a redução `any` continua recebendo a coleção do chamador. A B adiantou as decisões nas cinco origens autorizadas K32, K62, K63, K64 e K67, mais K68, e documentou a responsabilidade da origem. `chamados/juizes.py:1516`, `:1524`, `:1526`.

| K | Sítio da carga na B | O que passou a vir em lote | [nao verificado]
|---:|---|---|
| 4 | `chamados/juizes.py:1512` | Vivas da disputa, na ordem de PK |
| 11 | `chamados/services/becos.py:87` | `perguntas` → `perguntas__decisoes` no evento |
| 23 | `folha/previa.py:64`, `:66` | Perguntas da prévia agrupadas por colaborador e dia |
| 25 | `chamados/services/declaracao_texto.py:475` | `do_fato` da projeção avulsa por marco |
| 26 | `chamados/services/declaracao_texto.py:588`, `:589` | `linhas_dos_fatos` do lote |
| 27 | `chamados/services/declaracao_texto.py:635` | `do_fato` das propostas, com o memo por fato |
| 29 | `chamados/juizes.py:1765` | Seleção de corrigíveis antes de formar os IDs |
| 30 | `chamados/juizes.py:1773` | Fila de respondidas a validar |
| 31 | `chamados/juizes.py:2435`, `:2437` | Perguntas usadas para extrair os dias vivos |
| 32 | `chamados/juizes.py:2490` | Perguntas da disputa aberta que pode travar o chamado |
| 41 | `chamados/models.py:1555` | Ressuscitação, lote já presente na base |
| 42 | `chamados/models.py:2077` | Segunda carga de `DisputaSupervisao.fechar`, lote já presente na base |
| 44 | `chamados/services/acoes_chamado.py:352` | Perguntas do detalhe do chamado |
| 45 | `chamados/services/competencia_trancada.py:119`, `:121` | Perguntas da competência |
| 46 | `chamados/services/competencia_trancada.py:165`, `:166` | Perguntas do chamado percorrido na medição |
| 47 | `chamados/services/disputa_emissao.py:2080` | Perguntas da disputa candidata no ramo sem credor |
| 48 | `chamados/services/fio.py:2296` | Perguntas do destino sob a trava |
| 49 | `chamados/services/fio.py:2300` | Perguntas da origem sob a trava, na ordem de PK |
| 50 | `chamados/services/pergunta_no_app.py:186` | Fatos de `_marco_ja_perguntado` |
| 53 | `chamados/views.py:1412` | Perguntas sem resposta da reabertura |
| 54 | `colaboradores/services/calendario.py:504`, `:505` | Respondidas na janela do calendário |
| 55 | `ponto/services/dia_do_colab.py:64`, `:65` | Perguntas do dia do colaborador |
| 61 | `chamados/services/explicador.py:160`, `:161` | Perguntas da disputa quando a coleção opcional está ausente |
| 62 | `api/foto_recebida.py:81` | Perguntas de foto, depois da validação |
| 63 | `chamados/management/commands/encerrar_perguntas_com_fato.py:84` | Perguntas da disputa, depois dos carimbos |
| 64 | `chamados/services/limbo_folgas.py:203`, `:205` | Perguntas do dia usadas pela guarda do filho |
| 65 | `ponto/management/commands/supra_juiz.py:192`, `:215` | `perguntas_que_resolvem` → `perguntas_que_resolvem__decisoes`, para a coleção de todos os chamados daquela consulta |
| 67 | `ponto/management/commands/reconciliar_grade.py:461` | Respondidas usadas na guarda de relavra |
| 68 | `chamados/services/disputa_emissao.py:1429`, `:1436` | Perguntas do motivo e dia entregues ao dedup |

**Contagem por item.** Restam **30 K**. Destes, **24** são cargas da C e **6** são classe R. No predesenho, a decomposição dos mesmos 30 era **25 + 5**. O aval 1830, item 6b-1, reclassificou K40, que relê `.get(pk=pk)` dentro do laço. As outras R são K3, releitura sob trava da candidata, K19, execução por pergunta, K20 e K58, portas por ID, e K22, busca da irmã de uma pergunta. A sobra do dedup fica fora dos 69. `chamados/management/commands/migrar_data_evento_dia_do_turno.py:464`, `:466`, `chamados/services/relavra_via.py:230`, `:236`, `chamados/services/materializacao.py:284`, `:2117`, `chamados/services/validacao.py:431`, `:677`, `chamados/management/commands/dedup_perguntas_fato.py:75`.

Os **24 arquivos de produção tocados** são os das duas tabelas, exceto `chamados/models.py`. Os **sete arquivos de teste tocados** são os três novos abaixo, o selo de chamada e os três harnesses da seção de adaptações. `chamados/tests/test_e4c_f5_selo_chamada.py:21`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:24`, `chamados/tests/test_e4c_f5_voltaB_sitios.py:25`, `chamados/tests/e4c_f5_voltaB_mutantes.py:11`.

## Catraca da C

| Arquivo | Chamadas restantes |
|---|---|
| `chamados/management/commands/auditar_invariantes_chamados.py` | `:178` `_contraprova_i21a`, `:597` I2, `:1671` F1 |
| `chamados/management/commands/dedup_perguntas_fato.py` | `:76` `Command.handle` |
| `chamados/services/materializacao.py` | `:192` `par_do_intervalo`, `:259` `paridade_absolvida_bug_par_103`, `:290` `_situacao_da_irma`, `:885` `estado_aplicacao_pergunta`, `:1300` `_recusa_sem_anuencia` |
| `chamados/services/vigia_fio.py` | `:636` `censo_alcance` |

A permissão nominal do placar permanece em `chamados/services/raiox.py:147`, que recebe a tupla montada pelo `values_list` de `:135`. `chamados/tests/test_e4c_f5_selo_chamada.py:35`.

## REDs e mutantes

O RED 3 × 6 compara apenas SELECTs cujo primeiro `FROM` é `chamados_decisaopergunta`. O teste exige os PKs de todas as perguntas julgadas em ambas as medidas. No K47, a medição inclui o carimbo e o fecho reais e desfaz a savepoint para manter as três primeiras perguntas na rodada de seis. `chamados/tests/test_e4c_f5_opus_models.py:25`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:40`, `:178`.

Os REDs de divergência cobrem os 14 sítios da primeira tabela. O GET usa o instante do ato para compor `respondidas`. O POST do app e o PWA verificam o estado anterior entregue ao escritor. A validação vê a reabertura do ato, a recusa cobre o carimbo ausente na projeção, e o M1 exerce a projeção temporal. A **GUARDA** das portas usa texto vazio na projeção. Esse caso já era recusado na base porque o juiz puro lia o texto vazio do ORM. `chamados/tests/test_e4c_f5_voltaB_sitios.py:64`, `:76`, `:92`, `:104`, `:110`, `:123`, `:135`, `:147`, `:157`, `:169`, `:196`, `:205`, `:219`, `:229`, `:243`, `chamados/juizes.py:1556`.

Comandos para o Code, a partir de `app/`. As classes e métodos estão nos dois arquivos novos e no selo. `chamados/tests/test_e4c_f5_voltaB_lotes.py:24`, `chamados/tests/test_e4c_f5_voltaB_sitios.py:25`, `:38`, `:234`, `chamados/tests/test_e4c_f5_selo_chamada.py:194`.

```bash
lotes=chamados.tests.test_e4c_f5_voltaB_lotes.CarregadoresVoltaBTest
sitios=chamados.tests.test_e4c_f5_voltaB_sitios

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$lotes" "$sitios" \
  chamados.tests.test_e4c_f5_selo_chamada \
  chamados.tests.test_disputa_s127 \
  chamados.tests.test_e3_r3_c5_sem_banco \
  chamados.tests.test_e3_r3_c6_sem_banco
```

Cada comando abaixo ativa um único mutante. Para os 14 carregadores, a mordida esperada é `Q(6) > Q(3)` na asserção de igualdade. Para `api_p_cru`, a mordida é `api/views.py:2597` na catraca. A transformação AST da chamada crua foi conferida estaticamente. O resultado dos casos ORM está [nao verificado]. `chamados/tests/e4c_f5_voltaB_mutantes.py:11`, `:31`, `:63`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:51`, `chamados/tests/test_e4c_f5_voltaB_sitios.py:26`.

```bash
mut_lote() {
  F5_VOLTA_B_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 \
    python3 manage.py test "$lotes.$2"
}
```

| Mutante | Caso que deve cair e comando |
|---|---|
| K4 | `mut_lote k4 test_RED_k4_vivas_da_disputa` |
| K11 | `mut_lote k11 test_RED_k11_beco_do_evento` |
| K23 | `mut_lote k23 test_RED_k23_previa` |
| K26 | `mut_lote k26 test_RED_k26_declaracao_por_fato_em_lote` |
| K29 | `mut_lote k29 test_RED_k29_respostas_corrigiveis` |
| K30 | `mut_lote k30 test_RED_k30_fila_a_validar` |
| K31 | `mut_lote k31 test_RED_k31_dias_das_perguntas_vivas` |
| K32 | `mut_lote k32 test_RED_k32_disputa_viva_do_chamado` |
| K45 | `mut_lote k45 test_RED_k45_perguntas_da_competencia` |
| K46 | `mut_lote k46 test_RED_k46_perguntas_do_chamado_na_competencia` |
| K47 | `mut_lote k47 test_RED_k47_escolhas_sem_credor` |
| K54 | `mut_lote k54 test_RED_k54_calendario` |
| K55 | `mut_lote k55 test_RED_k55_dia_do_colab` |
| K68 | `mut_lote k68 test_RED_k68_dedup_da_emissao` |
| `api_p_cru` | `F5_VOLTA_B_MUTANTE=api_p_cru PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$sitios.SeloMutanteVoltaBTest.test_MORDE_get_do_app_com_pergunta_crua"` |

## Adaptações e tetos

| Arquivo | Classe | Antes → depois | Asserts |
|---|---|---|---:|
| `chamados/tests/test_e3_r3_c5_sem_banco.py:180` | ii | Stub sem `DecisaoPergunta` → compositor do helper existente, `chamados/tests/duble_pergunta.py:12` | 34 → 34 |
| `chamados/tests/test_e3_r3_c6_sem_banco.py:299` | ii | Stub sem `DecisaoPergunta` → mesmo helper | 52 → 52 |
| `chamados/tests/test_disputa_s127.py:23` | v | `.all()` devolvia `list` → `Linhas`, subclasse de lista com `prefetch_related` identidade, `chamados/tests/test_e4b_classe_e_memoria.py:165`, `:193` | 8 → 8 |
| `chamados/tests/test_e4c_f5_selo_chamada.py:21` | Catraca solicitada | 24 identidades → 10 | 12 → 12 |

**Chokepoint.** Nenhum sítio da B altera uma chamada a `registrar_batida`. Há alcance indireto pelo fecho S143 até K47, pela eventual reabertura do credor até K48/K49 e pelo repasse do emissor até K68. K11 entra pelo `on_commit` da pergunta. Em K47, uma disputa pode carregar o lote e depois sair pela guarda de destino vivo ou de lista sem abertas. O efeito nos quatro cenários é [nao verificado]. `ponto/registro_batida.py:170`, `ponto/signals.py:215`, `chamados/signals.py:117`, `:240`, `chamados/services/disputa_emissao.py:1878`, `:2080`, `:2086`, `chamados/models.py:1481`, `ponto/services/cartorio.py:1877`, `ponto/management/commands/detectar_ausencias.py:803`, `chamados/services/disputa_emissao.py:846`.

**Quebra fora das classes.** Os quatro casos do chokepoint já têm o excesso informado no aval de passagem, **236/146/300/221** contra **230/145/294/219**. A contribuição da B e outras quebras estão [nao verificado]. `ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`, `:228`, `:235`, `:435`, `:440`, `:470`, `:475`.

**Tetos a remedir para eventual queda.** O modal prod-like tem teto **2837** e alcança K27 nas propostas e K61 no fallback do explicador. A queda depende de quantas perguntas passam por essas cargas na fixture. O teto pareado S143/S128 é recalculado no próprio teste. A direção líquida e os valores estão [nao verificado]. Nenhum teto foi editado. `chamados/tests/test_selo_teto_modal.py:88`, `:184`, `chamados/services/fio.py:330`, `:351`, `chamados/tests/test_e4b_c4_orm.py:499`, `:507`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_1_furo_vivo_que_ela_fecha \
  ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_3_furos_vivos_que_ela_fecha \
  ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_1_furo_vivo_que_fica \
  ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_3_furos_vivos_que_ficam \
  chamados.tests.test_selo_teto_modal \
  chamados.tests.test_e4b_c4_orm \
  api.tests.test_f2_teto_consultas_app \
  chamados.tests.test_e2_tarefa_do_dia \
  escala.tests.test_f3_beco1_folga_na_lista
```

## Campos e releituras

| Campo | Quem escreve | Quem lê e qual significado |
|---|---|---|
| `validada_em` | Validação e devolução em `chamados/services/validacao.py:168`, `:173`, `:273`, `:279`, `:349`, `:354`. Nova resposta limpa a assinatura em `chamados/services/disputa_emissao.py:1494`. Carimbo e ressuscitação atualizam a projeção em `chamados/models.py:3352`, `:1721`. | C6 e temporal recebem validação vigente da sequência, com passivo incorporado por `leitura`, `chamados/models.py:4317`, `:4358`, `chamados/juizes.py:1408`, `escala/services/cadastro_realidade.py:502`. Guardas de projeção continuam lendo o carimbo gravado em `chamados/services/validacao.py:685`, `:897`, `chamados/services/declaracao_texto.py:228`, `chamados/services/disputa_emissao.py:2084`. Podem divergir. |
| `respondida_em` | Resposta humana, automática e trilha em `chamados/services/disputa_emissao.py:1528`, `:1532`, `:1683`, `:1686`, `:1705`, `:1708`. Reabertura e recusa limpam o par em `:1615`, `:1640`, `:1656`. Carimbo e ressuscitação também atualizam a projeção em `chamados/models.py:3349`, `:1722`. | Juízes da B usam o instante vigente do ato. A recusa usa esse instante na foto de auditoria. O payload do GET, a face SQL e a foto gravada pelo escritor de reabertura continuam lendo a coluna projetada. `chamados/models.py:4339`, `:4358`, `chamados/services/validacao.py:914`, `api/views.py:2583`, `chamados/juizes.py:1587`, `chamados/services/disputa_emissao.py:1640`. |
| `resposta_colab` | Escritores humano e automático em `chamados/services/disputa_emissao.py:1481`, `:1682`. Reabertura e ressuscitação limpam a projeção em `:1615`, `:1656`, `chamados/models.py:1722`. | Toda chamada convertida usa o texto corrente da projeção. `resposta_display` conserva sua leitura própria de exibição. `chamados/models.py:4359`, `:3837`, `api/views.py:2581`, `chamados/services/declaracao_texto.py:418`. |
| `materializada_em` | `_escrever_carimbo` abastece as portas de carimbo, identidade e descarimbo. A porta de lote limpa a projeção. `chamados/models.py:3314`, `:3346`, `:3401`, `:3424`, `:3508`. | Compositor e guardas escalares leem o carimbo projetado. O juiz temporal aplica o corte de `agora`. O carimbo é o estado registrado, e C6 julga a vitalidade pelo conjunto dos insumos. `chamados/models.py:4359`, `chamados/juizes.py:1394`, `:1400`, `:1474`, `chamados/services/disputa_emissao.py:2084`. |
| `via_resolucao` | Mesmas portas do carimbo e a troca pela via de fato no espelho em `chamados/models.py:3318`, `:3508`, `:3517`. | A origem é a mesma coluna projetada. No corte temporal, vias fora do fato conservam a identidade. `chamados/models.py:4359`, `chamados/juizes.py:1400`, `chamados/services/disputa_emissao.py:2095`. |
| `decisoes` | `DecisaoPergunta.registrar` insere pela FK e invalida `decisoes` na instância recebida por `pergunta=`. Escrita por `pergunta_id=` exige instância fresca para uma leitura posterior. `chamados/models.py:4188`, `:4362`, `:4373`, `chamados/services/disputa_emissao.py:1639`. | Os K adiantam a relação. `DecisaoPergunta.leitura` consome a sequência e incorpora o passivo antes de `composta` entregar os campos aos juízes. `chamados/models.py:4317`, `:4329`, `:4351`. |

**Depois de escrever, no mesmo request**

- **Validar.** A guarda usa a leitura anterior. A validação registra com `pergunta=perg`. Há releitura integral depois da unidade do par e releitura escalar de `validada_em` e `materializada_em` após materializar a disputa. A releitura escalar dispensa prefetch novo. `chamados/services/validacao.py:173`, `:646`, `:655`, `:682`, `:743`, `:786`, `:789`.
- **Recusar.** A foto usa a leitura anterior. A reabertura grava por queryset, e a porta usa a foto e os campos da trilha depois do ato. O RED relê a instância inteira antes de compor novamente, pois a instância do teste havia carregado a sequência anterior. `chamados/services/validacao.py:902`, `:913`, `:925`, `:933`, `chamados/tests/test_e4c_f5_voltaB_sitios.py:110`.
- **Responder via API e PWA.** As duas guardas são anteriores. O escritor grava na mesma instância e invalida o cache dessa instância. A releitura de exibição do escritor, quando há chamado, acessa a sequência atual. O retorno da API usa o instante atualizado em memória e o PWA retorna a confirmação. `api/views.py:2641`, `:2648`, `:2657`, `chamados/views.py:1275`, `:1278`, `:1282`, `chamados/services/disputa_emissao.py:1528`, `:1532`, `:1569`, `chamados/models.py:4373`.
- **Declarar e aplicar.** `declarar_hora_colab` pode reler integralmente depois da costura, antes de julgar a resposta. Depois da resposta, anexa a nota e devolve o PK. `aplicar_declaracao` passa o PK à validação, que faz um `get()` novo. Assim, a validação posterior lê a sequência atual pela própria instância. `chamados/services/declaracao_texto.py:373`, `:418`, `:420`, `:424`, `:688`, `:694`, `:697`, `chamados/services/validacao.py:677`.
- **Validar inline e regularizar.** As listas de `validar_inline` classificam antes das escritas e o laço passa cada PK à porta. A regularização delega ao materializador e usa o retorno dele. A releitura do beco após o resolver passa por `refresh_from_db()` antes do juiz de fecho, descartando a carga reversa anterior. `chamados/views_cobrar.py:170`, `:178`, `:200`, `:209`, `chamados/services/acoes_disputa.py:134`, `:142`, `chamados/services/becos.py:58`, `:59`.

LEI-AKITA — padrao=juiz unico puro + fonte unica (compositor) + fitness function. `chamados/juizes.py:1387`, `:1556`, `chamados/models.py:4351`, `chamados/tests/test_e4c_f5_selo_chamada.py:194`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:40`.

## 19/11 23:39 · sessao 7f9b70778ff8 · CENSO-DAS-COPIAS-1911-2330 · leitura · origem=Code

# CENSO-DAS-COPIAS-1911-2330

**57 cópias no `fatos.json` · 9 POUSOU · 22 PELA-METADE · 26 MORTA.** A soma inclui duas classificações POUSOU com prova fraca, detalhadas no fim. O universo é o dos 57 registros raspados. A conferência direta da lista da VM e da amostra de cinco contra o Git ficou [nao verificado]. `bin/raias_estado.py:73–89,96–109,125–133`

As linhas da tabela seguem a ordem dos objetos do `fatos.json`. Cópia, branch, ahead e já no main vêm do objeto correspondente. **Já no main** reproduz `cherry_ja_no_main`, inclusive `0` quando a sonda não trouxe commits ou quando a cópia contém merges. As referências **vivo** usam a numeração resultante do `docs_vivos_delta.diff`. **Dono** indica quem responde pela trava ou pelo próximo ato registrado, com **Code** para a sessão técnica. `bin/raias_estado.py:101–106,125–130`

| copia | obra | branch | ahead | ja no main | ultima linha da obra no RELATO/RELATO-ARQUIVO | trava | dono | classe | motivo da classe |
|---|---|---|---:|---:|---|---|---|---|---|
| `wt-b2-r4b-002812` | `E4C-F1-B2a` | `raia/wt-b2-r4b-002812` | 1 | 1 | 18/11 07:22 — B2a entrou no ar em `4eb10f99f`. `RELATO.md:571` | ninguém | Code | POUSOU | Patch-id 1/1 e teste R4b de recusa presente na cópia local. `TICKETS.md:1326` `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:313` |
| `wt-bug-enc-pouso-223206` | `BUG-ENC-CELULAS-FORMA` | `raia/bug-enc-pouso-1115` | 1 | 0 | 15/11 23:0x — BUG-ENC entrou no ar em `7825fec5b`. `RELATO.md:1164` | ninguém | Code | POUSOU | O RELATO identifica `2b80dcdca` como a árvore certificada do pouso, com diferenças só em LEIS/TICKETS. `RELATO.md:1164` `TICKETS.md:1325` |
| `wt-catraca-dec-151606` | `DATA-EVENTO-CRU` | `raia-catraca-dec-151606` | 4 | 0 | 11/11 21:04 — Opus reprovou a catraca v4 com três bloqueios. `RELATO.md:2248` | aval | Ronald | PELA-METADE | Falta rearme e correção da cobertura de templates, filtros por `**kw` e retornos de lookup. `RELATO.md:2248` `BACKLOG.md:2214` |
| `wt-cirineu-c4-contador-085632` | `[nome]-NAO-DESFAZ-O-PAPEL` — c4 contador | `raia/cirineu-c4-contador-085632` | 9 | 0 | 18/11 10:3x — A revisão devolveu a unidade do contador P1. `RELATO.md:503` | aval | Ronald | MORTA | A rodada de `4b68ae82b` foi cancelada como superada, e os complementos mudaram o universo do contador. `RELATO.md:503` `RELATO.md:511–513` |
| `wt-codex-bo-disputa-sem-pergunta-183316` | `BO-DISPUTA-SEM-PERGUNTA` | `bodisp-c25-183316` | 30 | 0 | 14/11 19:4x — c25 foi guardada como insumo da F1 da RAIZ. `RELATO.md:1555–1556` | espera `RAIZ-DA-DISPUTA` | Ronald | MORTA | `BO-DISPUTA-DESENHO-RAIZ` encerrou as voltas nessa linhagem e destinou c25, matriz e laudo à RAIZ. `BACKLOG.md:774` `RELATO.md:1556` |
| `wt-codex-bo-disputa-sem-pergunta-221357` | `BO-DISPUTA-SEM-PERGUNTA` | `raia/bo-disputa-221357` | 24 | 0 | 14/11 19:4x — O aval guardou c25 e passou o desenho à RAIZ. `RELATO.md:1556` | espera `RAIZ-DA-DISPUTA` | Ronald | MORTA | Os commits listados até c21 reaparecem em c25, que virou insumo da RAIZ. O conteúdo dos 13 sujos ficou [nao verificado]. `BACKLOG.md:774` `RELATO.md:1560` | [nao verificado]
| `wt-codex-cadeia-20-passo-1-112113` | `CADEIA-20-PASSO-1` | `(detached)` | 16 | 0 | 19/11 23:25 — Ronald autorizou pouso após PRONTO e Opus, com Pauta DP para os quatro casos. `RELATO.md:2` (vivo) | ninguém | Code | PELA-METADE | Faltam a causa das mudanças na segunda passada e da célula de agosto, a verificação e o PRONTO. `RELATO.md:1–4,24` (vivo) | [nao verificado]
| `wt-codex-cirineu-nao-desfaz-o-papel-181941` | `[nome]-NAO-DESFAZ-O-PAPEL` | `(detached)` | 1 | 0 | 18/11 à noite — A pergunta D1 registra a cura no ar e sete casos residuais. `RELATO.md:329` | ninguém [nao verificado] | [nao verificado] | POUSOU | **Prova fraca** para a cópia. O nome aponta para a cura pousada em `94bb5c8f4`, e a guarda consta no código local. O patch da cópia ficou [nao verificado]. `RELATO.md:4000–4007` `app/escala/utils.py:502` |
| `wt-codex-e3-resto-r3-221106` | `E3-RESTO-R3` | `e3-resto-r3-c17-221106` | 1 | 1 | 16/11 01:36 — R3 c17 entrou no ar em `f8ee770a5`. `RELATO.md:1151` | ninguém | Code | POUSOU | Patch-id 1/1 do ajuste c18 nessa cópia. `RELATO.md:1166` `BACKLOG.md:770` |
| `wt-codex-e4c-f5-constroi-090617` | `E4C-F5-CONSTROI` | `(detached)` | 6 | 0 | 19/11 10:0x — A volta 2 mudou a fonte do texto e acrescentou o contador. `RELATO.md:141` | ninguém | Code | MORTA | A montagem `9fa5ad533` sucedeu a cópia das primeiras voltas e foi incorporada à construção da volta A. `RELATO.md:149,80` `RELATO.md:5` (vivo) |
| `wt-codex-e4c-f5-constroi-184647` | `E4C-F5-CONSTROI` | `e4c-f5-constroi-a-170736` | 19 | 0 | 19/11 23:25 — A foi aceita como passagem, B abriu e o teto ficou como condição de certificação. `RELATO.md:5` (vivo) | ninguém | Code | PELA-METADE | Faltam as voltas B e C e a certificação final dentro do teto. `RELATO.md:5,8` (vivo) |
| `wt-codex-justificativa-paga-o-periodo-220358` | `JUSTIFICATIVA-PAGA-O-PERIODO` | `(detached)` | 23 | 0 | 16/11 12:2x — A obra reabriu como lateral pelo censo. `RELATO.md:1079` | espera `COMPETENCIA-PAGA-UM-JUIZ` | Code | PELA-METADE | Falta atualizar a cura pelo censo e certificar o DIFF com a guarda da competência paga. `BACKLOG.md:2082–2083` `RELATO.md:1391,1399` |
| `wt-codex-lastro-faixa-sem-default-215253` | `LASTRO-FAIXA-SEM-DEFAULT` | `(detached)` | 16 | 0 | 12/11 16:4x — O aval encerrou c20 e preservou a cópia como especificação. `RELATO.md:2009` | ninguém | Code | MORTA | `LASTRO-REDESENHA-NA-FONTE` substituiu a série pela base `f3ebd821b`. Os dois sujos ficaram [nao verificado]. `RELATO.md:2009–2012` |
| `wt-codex-lastro-redesenho-164050` | `LASTRO-FAIXA-SEM-DEFAULT` | `raia/lastro-redesenho-164050` | 9 | 0 | 19/11 13:35 — O passo 3 ficou atrás do passo 1 no ar. `RELATO.md:67` | espera `CADEIA-20-PASSO-1` | Code | PELA-METADE | Falta refazer a base `f3ebd821b` após o passo 1 e provar o DIFF de frota. `RELATO.md:2012` `BACKLOG.md:1960,1967` |
| `wt-codex-pauta-sem-turno-prefixo-reservado-030404` | `PAUTA-SEM-TURNO-PREFIXO-RESERVADO` | `(detached)` | 5 | 0 | 10/11 09:37 — `TRES-ITENS` congelou a v5 e registrou os quatro furos de identidade. `RELATO.md:2589` | aval | Ronald | PELA-METADE | **Prova fraca** do vínculo da cópia com `78528a3ec`; falta corte para retomar o desenho e tratar os quatro furos. `BACKLOG.md:2266` `RELATO.md:2595` |
| `wt-codex-tipo-na-fonte-184856` | `TIPO-NA-FONTE` — item 3 | `raia/tipo-item3-v2` | 13 | 0 | 18/11 — A retomada confirmou zero chamadas do acessor no HEAD e marcou a v5c como consulta. `RELATO.md:356` | ninguém | Code | MORTA | `TIPO-ITEM3-DESENHO-1811-1820` cortou o tipo fechado e mandou nascer a implementação em cópia nova do HEAD. `BACKLOG.md:1959–1960` `RELATO.md:355–356` |
| `wt-codex-triagem-offline-red-headless-101137` | `TRIAGEM-OFFLINE-RED-HEADLESS` | `(detached)` | 1 | 0 | 12/11 10:39 — RED de offline e reload reproduzido nas duas cascas em `d680518b0`. `RELATO.md:2151` | aval | Code | PELA-METADE | Falta abrir a volta de cura do `sw.js` e provar verde e smoke nas duas cascas. `RELATO.md:2151` `BACKLOG.md:2800` |
| `wt-codex2raias-090624` | `CODEX-2-RAIAS-PORTAO-CPU` | `codex2raias-090624` | 2 | 0 | 09/11 16:0x — `SERIE-E-SO` congelou a cópia `e381017bf` com a matriz pronta. `RELATO.md:2676–2677` | aval | Ronald | PELA-METADE | Falta liberar e executar a volta 3 pelo contrato E1–E25, com selos, suite e Opus. `RELATO.md:2677,2687` `BACKLOG.md:2281` |
| `wt-comboio-1811-121757` | `POUSOS-COMBOIO-1811-1332` — montagem inicial | `raia/comboio-1811-121757` | 4 | 0 | 18/11 13:10 — A certificação passou às montagens A e B. `RELATO.md:441,449` | ninguém | Code | MORTA | `COMBOIO-DAS-SUITES-1811` substituiu a montagem conjunta por `81f678c7e` e `fa49abfb6`, com pousos por fatia. `RELATO.md:441,449–450` `BACKLOG.md:2386` |
| `wt-comboio-1911-004256` | `QUEIMA-CODEX-E-PISTA-1911-0045` — item A | `(detached)` | 3 | 0 | 19/11 00:4x — A montagem `477a78ea5` juntou F6F7, ESPURIA e F9. `RELATO.md:235` | ninguém | Code | MORTA | O trem usou o comboio B para F6F7/F9 e levou ESPURIA c11 pelo comboio F. `RELATO.md:145,162,168–169` `BACKLOG.md:2050` |
| `wt-comboio-1911-b-011045` | `TREM-DE-POUSO-1911` — vagões 1 e 3 | `(detached)` | 2 | 0 | 19/11 08:25 — F9 entrou no ar pela árvore do comboio B. `RELATO.md:168` | ninguém | Code | POUSOU | `154d30455` igual ao primeiro passo do B e `3308f7599` igual à árvore do passo F9. `RELATO.md:168–169` |
| `wt-comboio-b-1811-123606` | `POUSOS-COMBOIO-1811-1332` — comboio B | `raia/comboio-b-1811-123606` | 3 | 0 | 18/11 13:32 — O aval separou o pouso de Cirineu da volta de ESCALA. `RELATO.md:437,449` | ninguém | Code | MORTA | `fa49abfb6` tinha ESCALA c5 reprovada. Cirineu pousou em `94bb5c8f4` e ESCALA seguiu até o comboio E. `RELATO.md:437,160,4000–4007` |
| `wt-comboio-d-080440` | `COMBOIO-1911-D` | `raia/comboio-d-1911-080440` | 4 | 0 | 19/11 08:3x — O job de `2e027a794` foi cancelado como superado. `RELATO.md:164` | ninguém | Code | MORTA | `COMBOIO-1911-E` substituiu D para retirar a ancestralidade da ESPURIA reprovada. `RELATO.md:164,167` |
| `wt-comp-paga-101739` | `COMPETENCIA-PAGA-UM-JUIZ` | `(detached)` | 7 | 0 | 16/11 15:4x — O cadastro passou a cobrir também as competências anteriores a 09/2026. `RELATO.md:1012` | espera a espinha em `models.py` e `fio.py` | Code | PELA-METADE | Falta completar a migração dos leitores, incluir o cadastro cortado e medir a guarda na sombra. `RELATO.md:1286,1303` `BACKLOG.md:2082` | [nao verificado]
| `wt-contador-codex-090249` | `CONTADOR-CODEX-OCIOSO` | `contador-codex-1711` | 3 | 0 | 17/11 11:2x — Ronald estacionou o contador e manteve a fila de reserva. `RELATO.md:793` | aval | Ronald | PELA-METADE | Falta rearme do B3 e correção dos quatro achados de implementação da v3 guardada. `RELATO.md:793,811` |
| `wt-deploy-exclusao-061631` | `DEPLOY-EXCLUSAO-ENVELOPE` | `deploy-exclusao-061631` | 2 | 0 | 09/11 08:0x — A triagem v2 confirmou cinco procedentes e deixou a volta 3 à espera de vaga. `RELATO.md:2781` | aval | Code | PELA-METADE | Falta a volta 3 para corrigir trava, marcador, métricas e selos, seguida de verificação. `RELATO.md:2776,2781` `BACKLOG.md:2110` | [nao verificado]
| `wt-e3-mesclada-121238` | `E3-FORMA-SQL-DO-JUIZ` — árvore de suite da E3 | `(detached)` | 1 | 0 | 14/11 23:4x — O push da E3 certificada terminou em `4b75cba10`. `RELATO.md:1503` | ninguém | Code | MORTA | A árvore de teste `6c465cb80` era da volta 4. `E3-POUSA-O-VERDE` escolheu `1ee368261`, pousada em `90f3fa66b`. `RELATO.md:1915,1522` `PROMPTS.md:2224` |
| `wt-e4c-f2-103933` | `E4C-F2-RESSUSCITAR-ATO` — protótipo F1/F2 | `e4c-f2-103933` | 4 | 0 | 18/11 17:5x — A revisão retroativa aprovou F2 P2 `8ae23ad62`. `RELATO.md:360` | ninguém | Code | MORTA | A série inicial `8c6577245` foi substituída pela F2 com o corte da reabertura compensadora e o P2 certificado. `RELATO.md:1278,4032–4036` `BACKLOG.md:2052` |
| `wt-escala-c7-021338` | `ESCALA-NAO-DESCREVE` | `raia/escala-c7-021338` | 3 | 0 | 19/11 09:10 — ESCALA c7 entrou no ar pelo comboio E em `433e2e57e`. `RELATO.md:152,160` | ninguém | Code | MORTA | A ponta `2fd98211a` foi rejeitada por descender de `d223c17d5` e substituída pela montagem E. `RELATO.md:164` |
| `wt-eval-contador-112226` | `EVAL-CONTADOR-CASOS` | `raia/eval-contador-112226` | 3 | 0 | 18/11 13:12 — O novo aval respondeu a revisão do contador em `13f5c2913`. `RELATO.md:442` | aval | Code | PELA-METADE | Falta executar os casos a–c e provar a mordida do mutante de data futura na vaga de instrumento. `BACKLOG.md:2829` `RELATO.md:470` |
| `wt-f2-073854` | `E4C-F2-RESSUSCITAR-ATO` | `f2-construtor-073854` | 10 | 0 | 18/11 17:5x — A revisão retroativa aprovou o P2 pousado. `RELATO.md:360` | ninguém | Code | POUSOU | A entrega da reabertura compensadora foi incorporada pelo P2 `8ae23ad62`, com migração 0056 e lógica presente na cópia local. `RELATO.md:4032–4036` `app/chamados/models.py:1669–1681` `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:1–8` |
| `wt-f2-p2-152531` | `E4C-F2-RESSUSCITAR-ATO` — P2 | `f2-p2-contract-152531` | 12 | 0 | 18/11 17:5x — A revisão retroativa aprovou o P2 de `8ae23ad62`. `RELATO.md:360` | ninguém | Code | MORTA | A árvore `73dd28073` levava B2 sem cerca e foi substituída pelo P2 refeito sobre B2a. `RELATO.md:533,556,668,4032–4036` |
| `wt-f5m-130211` | `E4C-F5-CONSTROI` | `raia/f5-merge-130211` | 9 | 0 | 19/11 23:25 — O aval da passagem A identifica `3f1d63863` como a base já atualizada. `RELATO.md:5` (vivo) | ninguém | Code | MORTA | `9fa5ad533`, antes candidata a pouso, foi incorporada à construção sobre `eb1e49c1e`, com o desenho A/B/C. `RELATO.md:58,80` `RELATO.md:5,22` (vivo) |
| `wt-f5v4-112404` | `E4C-F5-CONSTROI` | `(detached)` | 8 | 0 | 19/11 13:20 — A suite de `6978b3234` ficou verde como pré-filtro. `RELATO.md:83` | ninguém | Code | MORTA | A certificação dessa árvore foi substituída pelo merge `9fa5ad533`, incorporado à construção A. `RELATO.md:80,83` `RELATO.md:5` (vivo) |
| `wt-f6f7-sobre-main-173528` | `F6F7-LEI3-PELA-LEITURA` | `(detached)` | 1 | 0 | 19/11 08:20 — F6F7 entrou no ar em `154d30455`. `RELATO.md:169` | ninguém | Code | MORTA | A cópia `0726b5c98` era da medida. A série de medidas usou pontas posteriores, e o pouso certificou `67b63e0de`. `RELATO.md:366,271,169` |
| `wt-f6f7c14-sobre-main-202317` | `F6F7-LEI3-PELA-LEITURA` | `(detached)` | 1 | 0 | 18/11 à noite — A verificação bloqueou c14 e cancelou sua rodada de DIFF. `RELATO.md:283` | ninguém | Code | MORTA | **Prova fraca** do vínculo da cópia destacada com a c14; a c15 substituiu a rodada c14. `RELATO.md:280–283` |
| `wt-f6f7c15-sobre-main-203238` | `F6F7-LEI3-PELA-LEITURA` | `(detached)` | 1 | 0 | 18/11 à noite — O selo do escritor bloqueou c15 e abriu c16. `RELATO.md:280` | ninguém | Code | MORTA | **Prova fraca** do vínculo com a montagem `5baeb46a5`; o DIFF da ponta c15 foi cancelado e refeito em c16. `RELATO.md:279–281` |
| `wt-f6f7c16-sobre-main-213517` | `F6F7-LEI3-PELA-LEITURA` | `(detached)` | 1 | 0 | 18/11 22:0x — c17/c18 corrigiram o lint da c16. `RELATO.md:270–271` | ninguém | Code | MORTA | **Prova fraca** do vínculo com a árvore da medida v5; `a555d027b` sucedeu a volta c16. `RELATO.md:256,270–271` |
| `wt-feriado-092818` | `FERIADO-ESCALA` — `HJ-074` | `raia/feriado-escala-092818` | 1 | 0 | 13/11 11:0x — O aval apontou nominalmente esta cópia como superada. `RELATO.md:1928` | ninguém | Code | MORTA | `FERIADO-USA-A-LEI-DE-1609` tomou o lugar de `4289eadaf`; o conteúdo dos nove sujos ficou [nao verificado]. `BACKLOG.md:729` `RELATO.md:1928–1931` |
| `wt-feriado-posto-135549` | `FERIADO-POR-POSTO` | `raia/feriado-posto-135549` | 6 | 0 | 19/11 17:2x — Item 4 v2t ficou verde; A5 e `sombra_c2b=0` seguem pendentes. `RELATO.md:30` (vivo) | aval | Ronald | PELA-METADE | Faltam a resposta da prévia A5, o zero de C2 na sombra e a certificação final. `RELATO.md:30,35,48` (vivo) | [nao verificado]
| `wt-feriado1609-110424` | `FERIADO-USA-A-LEI-DE-1609` | `raia/wt-feriado1609-110424` | 4 | 0 | 15/11 à noite — A medida G parou no item 3(ii) por alcance em dias entregues. `RELATO.md:1196–1198` | ! | Ronald | PELA-METADE | Falta o corte sobre os dias entregues e a prova de G com referência e lote definidos. `RELATO.md:1196–1199` `BACKLOG.md:801` `PROMPTS.md:2216` |
| `wt-frac5-cura-100047` | `RC1-3-FRACAO-14` — FRAC5 | `(detached)` | 4 | 0 | 14/11 — FRAC5-TEXTO entrou no ar em `14b61c124`. `RELATO.md:1608` | ninguém | Code | MORTA | O aval `FRAC5-SO-O-QUE-O-CORTE-MANDOU` reduziu o pouso ao texto e guardou o mínimo de cinco dias em outra ref. `RELATO.md:1626,1657` `BACKLOG.md:760` |
| `wt-instr-travas-002004` | `CORTE-DAS-TRAVAS` | `(detached)` | 22 | 0 | 14/11 08:12 — O rearme foi consumido e o gate por classe saiu do escopo. `RELATO.md:1686` | ninguém | Code | PELA-METADE | Falta verificar e certificar os itens 1 e 5 no escopo reduzido, com o gate retirado. `RELATO.md:1688–1694,1715` `PROMPTS.md:2259` |
| `wt-lote7-ausencia-100250` | `POUSOS-CONDICIONAIS-1611` — LOTE7 | `(detached)` | 5 | 0 | 16/11 13:17 — O smoke fechou a condição do LOTE7 c7 no ar. `RELATO.md:1070` | ninguém | Code | POUSOU | A entrega `6cc3d6f22` é identificada na cópia, e o acordador está no código local após o pouso `42fd467a0`. `RELATO.md:1368,1072` `app/ponto/services/ausencia.py:154` |
| `wt-o10d-023001` | `O10-D-PERMUTA-RECIPROCA` | `o10d-023001` | 5 | 0 | 17/11 08:2x — Opus aprovou `bdbff7c4a`, com corte pendente para o pouso. `RELATO.md:886` | ! | Ronald | PELA-METADE | Falta resolver as condições de carta e tipo, `d1=d2`, recusas e o `!` de escala. `RELATO.md:886` `BACKLOG.md:2294` |
| `wt-par-rebase-060915` | `PAR-NAO-ADJACENTE` | `par-rebase-060915` | 5 | 0 | 09/11 07:3x — A v4 contrariava a regra do par externo. `RELATO.md:2790` | ninguém | Code | MORTA | `LEI-PAR-ANINHADO` superou `84a400e1` e exigiu v5 com o par externo preservado. `RELATO.md:2790` `BACKLOG.md:1313` |
| `wt-porta-pouso-185744` | ? | `(detached)` | 1 | 0 | 17/11 19:46 — A candidata PORTA-EXPORT v5 entrou no ar em `ba4cb6c3c`. `RELATO.md:476,651` | [nao verificado] | [nao verificado] | POUSOU | **Prova fraca**. Nome e horário são compatíveis com a certificação da v5. O vínculo com a árvore `91816030` ficou [nao verificado]. `RELATO.md:651` |
| `wt-pouso-ausencia-215637` | `AUSENCIA-EFEITO-PELO-ESTADO` | `(detached)` | 1 | 0 | 17/11 07:4x — O smoke fechou a obra. `RELATO.md:876` | ninguém | Code | MORTA | A primeira certificação teve dois vermelhos e foi substituída pela cópia `230733` após `44e0be955`. `RELATO.md:928,932,939` |
| `wt-pouso-ausencia-230733` | `AUSENCIA-EFEITO-PELO-ESTADO` | `(detached)` | 1 | 0 | 17/11 07:4x — O smoke fechou a obra. `RELATO.md:876` | ninguém | Code | POUSOU | A cópia nova foi certificada, e o pouso `6ce96a55e` bateu com os cinco arquivos da árvore de pouso. `RELATO.md:921–923,928` `BACKLOG.md:835` |
| `wt-quarentena-svv-151703` | `QUARENTENA-REABRE-COM-CENSO` | `raia/quarentena-svv-151703` | 1 | 0 | 12/11 15:3x — `E3-NA-VAGA` interrompeu o WIP com o RED ainda por rodar. `RELATO.md:2028` | aval | Code | PELA-METADE | Falta retomar com matriz da porta, executar o RED e provar o DIFF de frota. `BACKLOG.md:2775` `RELATO.md:2028` |
| `wt-relavra11-113556` | `RELAVRA-11-DOS-50` | `(detached)` | 9 | 0 | 15/11 12:4x — A resposta definiu que a reversão repõe os dados e os carimbos acompanham a escrita. `RELATO.md:1249–1250` | espera `COMPETENCIA-PAGA-UM-JUIZ` | Code | PELA-METADE | Falta empilhar sobre COMP-PAGA e provar foto, reversão e DIFF dos 47 na sombra. `RELATO.md:1324` `BACKLOG.md:2019` |
| `wt-saida-noturna-135512` | `SAIDA-NOTURNA-NO-DIA-CERTO` | `raia/saida-noturna-135512` | 1 | 0 | 12/11 15:3x — O aval deixou D2 seguir e isolou as decisões de D1/D3. `RELATO.md:2027` | aval | Ronald | PELA-METADE | Falta fechar a fonte e o custo do texto em D1 e a via de desfazer o carimbo em D3. `RELATO.md:2027,2082` `BACKLOG.md:2151` |
| `wt-selo-espera-084620` | `SELO-ESPERA` | `selo-espera-cego-fixture` | 1 | 0 | 11/11 09:33 — O aval estacionou a v1 sem abrir v2. `RELATO.md:2395` | aval | Ronald | PELA-METADE | Falta aval para a v2 e correção da leitura da raiz e da falha silenciosa do censo. `BACKLOG.md:2764,2766` |
| `wt-selos-canon-i3-113049` | `SELOS-CANON-I3` | `(detached)` | 3 | 0 | 09/11 16:0x — A v3 `4561d76e6` ficou congelada antes da matriz. `RELATO.md:2676,2678` | aval | Ronald | PELA-METADE | Faltam matriz, volta 4, suite, preverificação e Opus final. `RELATO.md:2678,2696` `BACKLOG.md:804` |
| `wt-silencio-2810-0915` | `QUARENTENA-DO-COLAB` / `SILENCIO-VENCIDO-VOLTA` | `silencio-vencido-2810` | 11 | 0 | 12/11 14:03 — `QUARENTENA-REABRE` usou o diff antigo como especificação após a reprovação. `RELATO.md:2089` | ninguém | Code | MORTA | O aval `QUARENTENA-REABRE-COM-CENSO` substituiu a cópia antiga por nova base e pelo WIP `c9a2a3846`. `BACKLOG.md:2775` `RELATO.md:2083,4687` |
| `wt-sonda-sem-grant-075803` | `SONDA-SEM-GRANT` | `(detached)` | 8 | 0 | 09/11 16:0x — A cópia `c5e7df309` ficou congelada c

[truncado em 20000 de 24756 caracteres]

## 19/11 23:30 · sessao 6a3bbfd9c659 · E4C-F5-CONSTROI · leitura · origem=Code

**A sonda atribui +6, +1, +6 e +2 SELECTs a sete sítios de leitura. Nenhuma das emissões listadas é ESCRITOR NOVO.** O `blame` atribui os prefetches à F5 volta A/A2 e a chamada avulsa à F5 A1. A volta 2 introduziu a leitura dos instantes pela sequência, que entrou na cópia sobre `main` pelo merge `3f1d63863`. O portão vi1 alterou testes. `atribuicao.txt:1`, `atribuicao.txt:12`, `atribuicao.txt:15`, `atribuicao.txt:26`, `commits.txt:1`, `commits.txt:2`, `commits.txt:7`, `commits.txt:8`, `commits.txt:9`, `commits.txt:10`, `app/chamados/models.py:4330`, `app/chamados/models.py:4357`.

Na tabela, as linhas de código são de `ec7134368`, salvo onde aparece `main`. A numeração de `atribuicao.txt` começa no primeiro cabeçalho de cenário. A sonda traz o prefixo do SQL e a pilha. A igualdade dos IDs entre consultas diferentes fica **[nao verificado]**.

| Cenário e linha da sonda | Emissão e chamadores em `ec7134368` | Commit da emissão | Classe e unidade de custo | O que o `main` lia e onde o lote pode ser reaproveitado |
|---|---|---|---|---|
| **1 fecha, +1** · `atribuicao.txt:2` | **A.** `prefetch_related('decisoes')` em [app/chamados/reconciliador.py:703](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/reconciliador.py:703), avaliado no `for` de `um_colaborador_por_identidade` (`app/chamados/juizes.py:1188`). Chamadores em `app/chamados/reconciliador.py:706` e `:635`. | `eaf69fe33`, **F5 A2**, lote K43. A1 liga o C6 ao compositor em `app/chamados/juizes.py:1408`. `commits.txt:2`, `commits.txt:7` | **LEITURA**, um lote de perguntas absolvíveis por chamada, +1 com 1 ou 3 fechamentos. `app/chamados/reconciliador.py:678`, `:702`, `atribuicao.txt:16` | **Substitui colunas** de instante e validação no C6. O cartório já tem decisões das perguntas com destino explícito em chamados de batida. O lote do E2 também pode conter órfãs e perguntas fora desses módulos. Reuso possível para a interseção com dados ainda atuais. Cobertura integral da emissão **[nao verificado]**. `main app/chamados/juizes.py:1389`, `app/chamados/reconciliador.py:678`, `:713`, `app/ponto/services/cartorio.py:145`, `:1026`, `app/chamados/models.py:3660` |
| **1 fecha, +1** · `atribuicao.txt:4` | **B.** `list(...prefetch_related('decisoes'))` de `_ancoradas` em [app/chamados/models.py:366](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/models.py:366). Chamadores `_plano_pergunta_viva` em `:400` e `_gate_pergunta_viva` em `:438`. | `18bf86f01`, **F5 volta A**, prefetch de `_ancoradas`. `commits.txt:8`, `app/chamados/models.py:366` | **LEITURA**, um lote por chamada do gate com ancoradas, +1 nos dois cenários de fecho. Pode crescer com o número de chamados que chegam ao gate com lote preenchido. `app/chamados/models.py:364`, `:400`, `:438`, `atribuicao.txt:18` | **Substitui colunas no C6**. O filtro de validação no `main` já consultava a sequência via SQL. O prefetch do cartório cobre destinos explícitos e o do E2 cobre as absolvíveis. Nenhum garante toda a âncora, que inclui órfãs pelo pai. O lote do próprio gate já chega a `_gravar_estado`. Eliminação integral deste SELECT por reuso **[nao verificado]**. `main app/chamados/models.py:351`, `:357`, `main app/chamados/juizes.py:1389`, `app/chamados/models.py:228`, `:361`, `:387`, `:401`, `app/chamados/reconciliador.py:703`, `app/ponto/services/cartorio.py:146` |
| **1 fecha, +1** · `atribuicao.txt:6` | **C.** Mesmo prefetch de `_ancoradas`, `app/chamados/models.py:366`. Chamador S143 em [app/chamados/services/disputa_emissao.py:1985](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/services/disputa_emissao.py:1985), sob o `lambda` de `:1920`. | `18bf86f01`, **F5 volta A**, prefetch compartilhado com B. A chamada S143 já estava na base. `commits.txt:8`, `main app/chamados/services/disputa_emissao.py:1983` | **LEITURA**, um lote de âncoras por chamada do resolver com resultado, +1 nos dois fechos. Pode crescer com o número de chamadas do S143 que encontram âncoras. `app/chamados/services/disputa_emissao.py:1985`, `atribuicao.txt:20` | **Substitui colunas no C6**, com sobreposição possível com B. O gate carrega em `app/chamados/models.py:400` e o cache é consumido em `:228`. Entre gate e S143 pode haver reaponte e gravação. O lote S128 de `app/chamados/services/disputa_emissao.py:1951` tem outro recorte e é avaliado em `:1986`, depois das âncoras. Reuso exige confirmar cobertura e frescor. Duplicação integral **[nao verificado]**. `main app/chamados/juizes.py:1389`, `app/chamados/models.py:456`, `:279`, `app/chamados/signals.py:117`, `app/chamados/services/disputa_emissao.py:1973` |
| **1 fecha, +1** · `atribuicao.txt:8` | **D.** `list(self.perguntas.all().prefetch_related('decisoes'))` de `DisputaSupervisao.fechar` em [app/chamados/models.py:2077](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/models.py:2077). Chamadores `app/chamados/reconciliador.py:663` e `app/ponto/services/cartorio.py:1026`. | `18bf86f01`, **F5 volta A**. O primeiro prefetch da sequência já existia na **F2**, `41620774bb`. `commits.txt:8`, `app/chamados/models.py:2077`, `main app/chamados/models.py:2001` | **LEITURA**, um lote por disputa que chega a esse trecho de `fechar`, +1 nos dois fechos. `app/chamados/models.py:2077`, `atribuicao.txt:22` | **Duplicada e evitável.** O `main` já carregava decisões para `respostas_sem_veredito()` e lia colunas no C6. Na cópia, `fechar()` chama `respostas_sem_veredito()` em `:2068`, que carrega a mesma coleção em `:1998` e `:2020`. As instâncias desse lote ficam no corpo da primeira leitura. Entregá-las ao C6 evita o segundo prefetch e mantém o compositor. `main app/chamados/models.py:2001`, `:2048`, `:2057`, `main app/chamados/juizes.py:1389`, `app/chamados/models.py:2018`, `:2023`, `:2077`, `:2078` |
| **1 fecha, +2** · `atribuicao.txt:10` | **E.** Prefetch aninhado `perguntas_que_resolvem__decisoes` em [app/ponto/services/cartorio.py:146](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/ponto/services/cartorio.py:146). Chamador `_julgar_colab_corpo` em `:1347`. `julgar_colab` chama o corpo em `:1015`, `:1048` e, condicionalmente, `:1113`. O par exato da sonda **[nao verificado]**. | `eaf69fe33`, **F5 A2**, lote K24. `commits.txt:2`, `app/ponto/services/cartorio.py:146` | **LEITURA**, um lote por execução do corpo que chega à carga e tem perguntas, duas emissões nos dois cenários de fecho. `app/ponto/services/cartorio.py:145`, `:1347`, `atribuicao.txt:24` | **Substitui colunas**. O `main` prefetchava as perguntas e o C6 lia seus campos. Na cópia, o primeiro lote serve aos leitores do corpo. O segundo usa o mesmo recorte por colaborador e módulos, mas a carga é local a cada passada e há carimbo/fecho entre passadas. Reuso do primeiro lote só para membros e decisões ainda atuais. Duplicação integral **[nao verificado]**. `main app/ponto/services/cartorio.py:147`, `main app/chamados/juizes.py:1389`, `app/ponto/services/cartorio.py:1026`, `:1033`, `:1048`, `:1347`, `:1417`, `:1689` |
| **1 FICA, +1** · `atribuicao.txt:13` | **F.** `list(do_fato(...).select_related(...).prefetch_related('decisoes'))` em [app/chamados/services/fio.py:1964](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/services/fio.py:1964). Chamadores `_nascer` em `app/chamados/models.py:3119` e `_sob_contrato` em `:3027`. | `eaf69fe33`, **F5 A2**, lote K17. A1 converte a eleição da canônica em `app/chamados/services/fio.py:1288`. `commits.txt:2`, `commits.txt:7` | **LEITURA**, um lote por chamada de `universo_do_fato` com candidatas, +1 com 1 ou 3 chamados que ficam. `app/chamados/services/fio.py:1963`, `atribuicao.txt:29` | **Substitui a coluna `respondida_em`** na eleição da canônica. O primeiro lote para esse recorte é carregado aqui e semeia `_viz`. A ponte anterior pode ter carregado vizinhas em `:1595` e guardado em `:1848`. Reuso sem nova derivação depende da mesma chave do `do_fato`. Lote anterior equivalente nesta execução **[nao verificado]**. `main app/chamados/services/fio.py:1283`, `:1958`, `app/chamados/services/fio.py:1288`, `:1811`, `:1968`, `:2001`, `app/chamados/models.py:3087` |
| **3 fecham, +1** · `atribuicao.txt:16` | **A**, mesmos sítio e chamadores. | `eaf69fe33`, **F5 A2**, como A. | **LEITURA**, +1 por lote, como A. | Mesma substituição de colunas e mesmo limite de cobertura de A. |
| **3 fecham, +1** · `atribuicao.txt:18` | **B**, mesmos sítio e chamadores. | `18bf86f01`, **F5 A**, como B. | **LEITURA**, +1 por lote do gate, como B. | Mesma substituição de colunas e mesmo limite de cobertura de B. |
| **3 fecham, +1** · `atribuicao.txt:20` | **C**, mesmos sítio e chamadores. | `18bf86f01`, **F5 A**, como C. | **LEITURA**, +1 por lote do resolver, como C. | Mesma substituição de colunas e mesma necessidade de confirmar frescor de C. |
| **3 fecham, +1** · `atribuicao.txt:22` | **D**, mesmos sítio e chamadores. | `18bf86f01`, **F5 A**, como D. | **LEITURA**, +1 por lote da disputa, como D. | Mesma duplicação evitável de D. `app/chamados/models.py:2020`, `:2068`, `:2077` |
| **3 fecham, +2** · `atribuicao.txt:24` | **E**, mesmos sítio e chamadores. | `eaf69fe33`, **F5 A2**, como E. | **LEITURA**, +2 por passadas do corpo, como E. | Mesma substituição de colunas e mesma condição de frescor de E. |
| **3 FICAM, +1** · `atribuicao.txt:27` | **G.** Avaliação de `list(p.decisoes.all())` em [app/chamados/models.py:4330](/home/ronald/codex_ws/ec71343686835c2e7460b5e25d4a7f3f886e0432/app/chamados/models.py:4330). Chamadores `composta` em `:4357` e `lavrar_sem_turno_aberto` em `app/chamados/reconciliador.py:367`. | `0db6afc73`, **F5 A1**, chamada corrente de `lavrar_sem_turno_aberto`. `e673e0efe`, **F5 volta 2**, introduziu a leitura no juiz, incorporada pelo merge `3f1d63863`. `commits.txt:7`, `:9`, `:10`, `app/chamados/reconciliador.py:367` | **LEITURA avulsa**, uma por pergunta persistida que chega sem cache. Nesta pilha, zero com 1 chamado que fica e +1 com 3. O código permite crescer por pergunta sem cache. `app/chamados/models.py:4326`, `:4330`, `atribuicao.txt:12`, `:27` | **Substitui a coluna `respondida_em`**. A varredura de órfãs já entrega o prefetch em `app/chamados/reconciliador.py:1575`, `:1580`. A emissão também pode chamar a porta com a pergunta recém-criada, em `app/chamados/services/disputa_emissao.py:468`, `:473`. Nesse ramo, as linhas reais retornadas por `registrar` já estão disponíveis no nascimento e o retorno é descartado. Repassá-las evitaria a leitura sem derivação paralela. O anexo termina em `lavrar_sem_turno_aberto`, então o ramo exato **[nao verificado]**. `main app/chamados/reconciliador.py:366`, `main app/chamados/juizes.py:1552`, `app/chamados/reconciliador.py:1673`, `app/chamados/models.py:3231`, `:4366`, `:4381` |
| **3 FICAM, +1** · `atribuicao.txt:29` | **F**, mesmos sítio e chamadores. | `eaf69fe33`, **F5 A2**, como F. | **LEITURA**, +1 por lote de candidatas, como F. | Mesma substituição de coluna e mesma condição de chave de F. |

| Cenário | Soma das linhas | Medição conferida |
|---|---:|---:|
| 1 chamado fecha | `1 + 1 + 1 + 1 + 2 = 6` | `236 − 230 = 6` · `atribuicao.txt:1` |
| 1 chamado FICA | `1 = 1` | `146 − 145 = 1` · `atribuicao.txt:12` |
| 3 chamados fecham | `1 + 1 + 1 + 1 + 2 = 6` | `300 − 294 = 6` · `atribuicao.txt:15` |
| 3 chamados FICAM | `1 + 1 = 2` | `221 − 219 = 2` · `atribuicao.txt:26` |

O `+7/+2/+7/+3` do item 2 do aval excede em uma consulta por cenário tanto a soma das linhas da sonda quanto a diferença medida. A origem desse descompasso **[nao verificado]**. `f5_chokepoint_passagem_1911_2325.txt:3`, `atribuicao.txt:1`, `:12`, `:15`, `:26`.

_7443 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
