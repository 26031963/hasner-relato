# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 14:13** · publicado em **01/11/2026 14:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 14:13 · sessao 5d0afc1570dd · CONTENCAO-QUE-MORDE-V2 · censo · origem=Code

Uso `raiz/` para as três primeiras voltas e `v4/` para as três seguintes. São identificadores dos anexos colados: linha 1 é a primeira após `== anexo ==`, contando linhas vazias.

**Os itens 1 e 7 exigem ressalva:** no primeiro, houve detecção seguida de cura declarada; no segundo, falta descarte explícito do risco na triagem. As classes atribuídas abaixo ficam condicionadas a essas limitações (`raiz/codex_v1.r:38`; `raiz/codex_v1_triagem.txt:2`; `v4/codex_v1.r:80`; `v4/codex_v3_triagem.md:5`).

1. **Certificado fabricável pelo Code — (a), na reconferência da cura; enquadramento global [nao verificado].**

   Pergunta: “a cura no delta fecha o caminho que o achado descrevia?” (`raiz/codex_v2.q:11`). Resposta: “Fecha o certificado reduzido a árvore/rc: leitor exige também Ran e OK” (`raiz/codex_v2.r:35`).

   **O Codex havia visto exatamente a fabricação:** “um log sintético […] emite certificado” e “A guarda não protege essa emissão nem Write direto” (`raiz/codex_v1.r:38`). A triagem declarou **#13 curado**, não descartado (`raiz/codex_v1_triagem.txt:2`). Portanto, (a) descreve somente o residual perdido na reconferência; dizer que o Codex nunca viu seria incorreto. O caminho permanece na cópia (`bin/disjuntor.py:1294`; `bin/disjuntor.py:2496`).

2. **Prova ponta a ponta com suite real — (c).** [nao verificado]

   Pergunta: “cada mutante tira de fato a costura […]? […] caso que passaria com a cláusula removida?” (`raiz/codex_v1.q:16–19`). Resposta: “Testa emissão pela CLI com log sintético; retirar a chamada de certificação de suite.sh não é mordido” (`raiz/codex_v1.r:83`).

   A parcela estática **foi vista**; classifico em (c) pela exigência de execução real e prova ponta a ponta. A resposta declarou a execução integral **[nao verificado]** (`raiz/codex_v2.r:3`). O grep e o `|| true` são legíveis na cópia, portanto essas parcelas isoladas não ficam fora do alcance da leitura (`bin/tests/test_disjuntor.sh:1715`; `bin/suite.sh:112`).

3. **Regex do veredito derivada em paralelo — (a).**

   Pergunta: checklist contendo “derivação paralela de fonte única” (`raiz/codex_v1.q:20–22`). **Resposta específica sobre a duplicação entre `cert_suite` e `_cert_ok`: ausente.**

   A duplicação está nos reconhecedores do emissor e do leitor (`bin/disjuntor.py:1307`; `bin/disjuntor.py:1309`; `bin/disjuntor.py:1337`). A lei citada pelo opus trata do veredito pelo rc do processo (`app/docs/LEIS.md:101`). O tema estava perguntado; não há identificação específica desse achado nas respostas fornecidas.

4. **H0 antes da espera; A→B→A — (b).**

   Pergunta: caminhos indevidos envolvendo corrida e checklist de ordem de trava (`raiz/codex_v1.q:9–11`; `raiz/codex_v1.q:20–22`). Resposta: “H0 é calculado antes da trava […] A→B→A pode certificar A após testar B” (`raiz/codex_v1.r:42`).

   Triagem: “#14b A->B->A durante a suite: fila” (`raiz/codex_v1_triagem.txt:8`). **Motivo registrado: somente “fila”; justificativa adicional ausente nessa linha.**

5. **Harness aceita qualquer rc não zero — (b).**

   Pergunta: verificar se os mutantes retiram a costura anunciada (`raiz/codex_v1.q:16–19`). Resposta: “O harness aceita qualquer falha do caso como morte do mutante” (`raiz/codex_v1.r:89`).

   Triagem inicial: “harness que aceita qualquer rc nao zero: fila” (`raiz/codex_v2_triagem.txt:8`). Motivo posteriormente explícito: “já existia, não é desta volta” (`v4/codex_v2_triagem.md:8`). O comportamento permanece na cópia (`bin/tests/test_disjuntor.sh:2282`).

6. **Contrato sem relação com o diff; `--no-verify` — (e).**

   **Pergunta específica sobre pertinência semântica obra↔diff ou sobre `--no-verify`: ausente. Resposta específica: ausente.**

   A pergunta próxima cobra ordem do portão perante `SKIP_TESTS` e saída antecipada **do pre-push**; não pergunta por essas duas garantias (`raiz/codex_v1.q:23`). A implementação verifica identificação da obra e existência do contrato, sem julgamento de sua relação com o diff (`bin/contrato_do_push.py:76–83`). O bypass por `--no-verify` consta do achado fornecido (`opus_final.md:7`).

7. **SessionStart remove reservas de outra sessão viva — (b) provisório: [nao verificado].**

   Pergunta: “A poda […] que caso concreto isso erra? Qual chamador grava a reserva e qual a apaga […]?” (`v4/codex_v1.q:14`). Resposta: “Com sessão preenchida, também se remove toda outra sessão sem verificar se continua viva” (`v4/codex_v1.r:80`).

   **Linha de triagem descartando especificamente esse risco: ausente.** A triagem encaminha a cura de `poda_seq` “junto com o aval CONTENCAO-SESSOES”, sem explicitar o destino daquele risco (`v4/codex_v3_triagem.md:5`). O código assume “UMA sessão […] viva por desenho” e elimina reservas das demais (`bin/disjuntor.py:1691`; `bin/disjuntor.py:1706`). Isso sustenta aceitação do desenho, mas não comprova o descarte exigido literalmente por (b).

8. **Idempotência limitada às últimas 500 autorizações — (b).**

   Pergunta: escritores e leitores de `est['autorizados']` (`raiz/codex_v2.q:16–18`). Resposta: “idempotência termina após 500 autorizações”; entradas antigas deixam de ser reconhecidas (`raiz/codex_v2.r:17–18`).

   Triagem: “idempotência limitada a 500 autorizações: fila”, por “pergunta de desenho: TTL/teto de autorizados” (`raiz/codex_v2_triagem.txt:6`). O corte permanece na cópia (`bin/disjuntor.py:1658`).

| Achado | Classe |
|---|---|
| 1 | (a)\* |
| 2 | (c) |
| 3 | (a) |
| 4 | (b) |
| 5 | (b) |
| 6 | (e) |
| 7 | (b)\* |
| 8 | (b) |

| Classe | Contagem |
|---|---:|
| (a) | 2 — inclui o item 1 condicional |
| (b) | 4 — inclui o item 7 provisório |
| (c) | 1 |
| (d) | 0 |
| (e) | 1 |
| **Total** | **8** |

\* **Não são dois enquadramentos estritos comprovados:** o item 1 foi visto e declarado curado; no item 7, o descarte específico não está documentado (`raiz/codex_v1.r:38`; `raiz/codex_v1_triagem.txt:2`; `v4/codex_v3_triagem.md:5`).

## 01/11 14:11 · sessao 7330d4c01221 · - · - · origem=Ronald (tmux)

- `v4/codex_v1.q:14`: pergunta 5.
- `v4/codex_v1.r:80`: bullet de `clear`, incluindo “Com sessão preenchida, também se remove toda outra sessão sem verificar se continua viva”.

Contagem virtual literal: primeira linha do anexo = 1, linhas vazias incluídas, quebras visuais ignoradas.

## 01/11 14:10 · sessao ee5fc7f037ed · BUG-ENC-CELULAS-FORMA · censo · origem=Code

**Há incompatibilidade real, mas a descrição da fila mistura dois efeitos:** o mapa composto perde a **geradora na escolha do vínculo dos segmentos**; ele **não esconde, por si só, as células do leitor do papel da ata**. O cartório alcança esse repasse; a carga em lote converte as chaves corretamente. (`app/ponto/turnos.py:2148`, `app/ponto/turnos.py:1954`, `app/escala/alimentacao.py:237`, `app/ponto/turnos.py:1629`, `app/ponto/services/cartorio.py:942`, `app/ponto/turnos.py:3005`)

**1. Formatos, acessos e efeito**

| Etapa/consumidor | Formato e acesso efetivo |
|---|---|
| `celulas_do_periodo` | Produz `{(colaborador_id, data): CelulaDia}`, filtrado pelo intervalo inclusivo. (`app/escala/alimentacao.py:41`) |
| `dias_encerrados` | Recebe esse mapa composto; quando omitido, deriva-o pelo carregador. Não devolve células: devolve `{data: bool}` ou `{data: motivo\|None}`. (`app/ponto/turnos.py:2130`, `app/ponto/turnos.py:2145`, `app/ponto/turnos.py:2166`) |
| `_dias_encerrados_puro` | Repassa o mapa composto, corretamente, tanto a `vinculo_do_dia` quanto ao julgamento do fim previsto. (`app/ponto/turnos.py:2164`, `app/ponto/turnos.py:2165`) |
| `turnos_do_colab` | Espera `{data: CelulaDia}`. Se precisa carregar, converte explicitamente o composto para esse formato. (`app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1884`) |
| Candidatos, `mais=` | Lê `celulas.values()` e extrai `escala_geradora_id`; a forma da chave não prejudica esse consumidor. (`app/ponto/turnos.py:1886`) |
| Papel da ata | Percorre `celulas.items()` e entrega **o objeto** a `periodos_do_dia`. A chave apenas identifica um balde interno. (`app/ponto/turnos.py:1629`, `app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1702`, `app/ponto/turnos.py:1729`) |
| Escolha do vínculo dos segmentos | Reconstrói `{(_cid, _d): _c}` a partir das chaves recebidas. Depois chama `vinculo_do_dia`. (`app/ponto/turnos.py:1954`, `app/ponto/turnos.py:1958`) |
| `vinculo_do_dia` | Busca `(celulas or {}).get((colaborador_id, data))`; lê a geradora e procura seu vínculo. Sem célula encontrada, escolhe entre os vínculos que cobrem a data. (`app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:238`, `app/escala/alimentacao.py:247`, `app/escala/alimentacao.py:253`) |
| Fim previsto do encerramento | `_dia_encerrado_motivo_puro` → `fim_turno` → `marcos_turno` → `ec.marcos_do_dia`, que faz `celulas.get((self.colaborador_id, data))`. Nesse ramo, o mapa composto está correto. (`app/ponto/turnos.py:2119`, `app/escala/servico_jornada.py:92`, `app/escala/servico_jornada.py:69`, `app/escala/models.py:1095`) |
| DNA por período, casca | Consulta `marcos_dna_periodo` **sem** alimentar `celulas`; portanto relê o DNA pela consulta própria. (`app/ponto/turnos.py:1910`, `app/escala/utils.py:100`) |
| DNA por período, lote | Alimenta `marcos_dna_periodo` com mapa composto, desestruturado por `(cid, d)` em `.items()`. (`app/ponto/turnos.py:3001`, `app/escala/utils.py:97`) |

A transformação defeituosa é esta: uma chave recebida como `(cid, D)` vira `(cid, (cid, D))`; a busca por `(cid, D)` não encontra a célula. O dado perdido nesse ponto é `escala_geradora_id`, e a decisão afetada é **qual vínculo fornece o template e delimita os segmentos do pareamento**. O vínculo escolhido fornece marcos de borda/intervalo e também condiciona o uso do mapa de DNA quando há segundo intervalo. (`app/ponto/turnos.py:1954`, `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:253`, `app/ponto/turnos.py:1930`, `app/ponto/turnos.py:1982`, `app/ponto/turnos.py:1985`)

O papel da ata continua lendo `celula.ata`, `celula.dna` e **`celula.data`**; não busca `celulas.get(data)`. Produz um mapa por minuto, lido depois por `papel_por_minuto.get(_m)`. Assim, a alegação “o formato composto torna o papel cego às células recebidas” não se sustenta nesse código. (`app/ponto/juiz_batida.py:157`, `app/ponto/juiz_batida.py:158`, `app/ponto/juiz_batida.py:175`, `app/ponto/turnos.py:1804`, `app/ponto/turnos.py:903`)

Há, porém, uma **segunda diferença de alimentação**: o cartório passa suas células de `d0..d1`, enquanto o pareador lê batidas e papel na janela com pad, `d0−2..d1+1`. Como qualquer mapa fornecido impede a carga automática, as células externas a `d0..d1` não são acrescentadas. Isso retira a ata das bordas, independentemente do defeito das chaves. (`app/ponto/services/cartorio.py:919`, `app/ponto/services/cartorio.py:942`, `app/ponto/turnos.py:1845`, `app/ponto/turnos.py:1882`, `app/ponto/turnos.py:1937`)

O cadastro direto do cartório continua recebendo todos os vínculos e o mapa composto correto; não se deve confundir essa consulta com a segmentação interna do pareador. (`app/ponto/services/cartorio.py:931`, `app/ponto/services/cartorio.py:933`, `app/ponto/supra_juiz.py:161`, `app/ponto/supra_juiz.py:162`)

**2. Chamadores e formatos**

Na tabela, “omitidos” significa `celulas=None`/`turnos=None`, conforme a assinatura. (`app/ponto/turnos.py:2125`)

| Chamador de `dias_encerrados` | Alimentação |
|---|---|
| `app/ponto/services/cartorio.py:942` | Mapa composto construído de `cels`; `turnos` omitido. Alcança o repasse incompatível. |
| `app/colaboradores/services/calendario.py:699` | Mapa composto `_cels_mes`; lista de turnos já calculados, passada em `app/colaboradores/services/calendario.py:700`. Não chama internamente o pareador. |
| `app/ponto/turnos.py:2177` | Ambos omitidos; composição de `par_completo_da_janela`. |
| `app/ponto/management/commands/supra_juiz.py:121` | Ambos omitidos. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:62` | Ambos omitidos. |
| `app/ponto/tests/test_c1_dias_encerrados.py:28` | Ambos omitidos. |
| `app/ponto/tests/test_c1_dias_encerrados.py:51` | Ambos omitidos. |
| `app/ponto/tests/test_c1_dias_encerrados.py:66` | Mapa composto de `celulas_do_periodo`; `turnos` omitido. |
| `app/ponto/tests/test_c1_dias_encerrados.py:77` | Mesmo mapa composto; `turnos` omitido. |
| `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:73` | Ambos omitidos. |

O calendário calcula os turnos sem células fornecidas e carrega separadamente `_cels_mes`; depois entrega ambos ao encerramento. (`app/colaboradores/services/calendario.py:301`, `app/colaboradores/services/calendario.py:395`, `app/colaboradores/services/calendario.py:699`)

| Chamador de `_dias_encerrados_puro` | Alimentação |
|---|---|
| `app/ponto/turnos.py:2147` | Mapa composto; todos os vínculos; turnos fornecidos ou produzidos pela casca. (`app/ponto/turnos.py:2145`, `app/ponto/turnos.py:2146`, `app/ponto/turnos.py:2148`) |
| `app/ponto/turnos.py:3012` | Mapa composto recortado para `[ini,fim]`; todos os vínculos do colaborador; `self.turnos(colab)` memoizado. (`app/ponto/turnos.py:3013`) |

| Instância de `CargaDaJanela` | Janela e uso |
|---|---|
| `app/ponto/status_dia.py:189` | `[data,data]`, agrupando colaboradores por data; lê turnos e motivos do encerramento. (`app/ponto/status_dia.py:195`, `app/ponto/status_dia.py:196`) |
| `app/core/services/painel_op.py:327` | `[hoje−2,hoje]`; encontra o dia da entrada e consulta encerramento antes de pedir o status D4. (`app/core/services/painel_op.py:330`, `app/core/services/painel_op.py:334`, `app/core/services/painel_op.py:336`) |

Os chamadores de `turnos_do_colab` com células **explícitas** são:

| Chamador | Formato |
|---|---|
| `app/ponto/turnos.py:2096` | Repasse da porta singular; contrato por `data`. |
| `app/ponto/turnos.py:2148` | Repasse da porta plural; mapa composto, incompatível. |
| `app/escala/utils.py:1532` | Por `data`. |
| `app/escala/utils.py:1847` | Por `data`, com cópias contendo ata construída em memória. (`app/escala/utils.py:1835`, `app/escala/utils.py:1836`) |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88` | Por `data`, construído nas linhas anteriores. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`) |
| `app/ponto/tests/test_o142_turno_invariante_na_janela.py:169`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:172`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:176` | Por `data`. |
| `app/ponto/tests/test_o142_turno_invariante_na_janela.py:203`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:204`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:214` | `{}` explícito. |
| `app/escala/tests/test_saida_valida_vira_orfa.py:158` | `{DIA: cel}`, com ata em memória. (`app/escala/tests/test_saida_valida_vira_orfa.py:152`) |

Os demais chamadores abaixo **omitem `celulas`**, acionando a carga e conversão próprias; `turnos_do_colab` não recebe argumento `turnos`. (`app/ponto/turnos.py:1848`, `app/ponto/turnos.py:1882`, `app/ponto/turnos.py:1884`)

| Grupo | Chamadas |
|---|---|
| Núcleo e portas de leitura | `app/ponto/turnos.py:778`; `app/ponto/turnos.py:2179`; `app/ponto/turnos.py:2909`; `app/ponto/turnos.py:3041`; `app/ponto/turno_leitura.py:89`; `app/ponto/turno_leitura.py:95`; `app/ponto/nucleo.py:47` |
| Telas/API/relatórios | `app/ponto/views.py:377`; `app/ponto/views.py:1586`; `app/ponto/views.py:3167`; `app/api/views.py:1449`; `app/relatorios/pdf_espelho.py:213`; `app/relatorios/pdf_espelho.py:447`; `app/colaboradores/services/calendario.py:301`; `app/colaboradores/services/drawer.py:45` |
| Escala | `app/escala/regua_defesa.py:358`; `app/escala/servico_jornada.py:250`; `app/escala/servico_jornada.py:392`; `app/escala/services/escala_auto.py:87`; `app/escala/services/impacto_troca.py:18` |
| Chamados | `app/chamados/models.py:1260`; `app/chamados/juizes.py:369`; `app/chamados/juizes.py:1727`; `app/chamados/detector_anomalias.py:155`; `app/chamados/services/materializacao.py:109`; `app/chamados/services/materializacao.py:206`; `app/chamados/services/disputa_emissao.py:644` |
| Serviços de ponto | `app/ponto/services/espelho.py:625`; `app/ponto/services/bordas_realizado.py:66`; `app/ponto/services/teto_na_lista.py:68`; `app/ponto/services/vigia_pareador_celula.py:97`; `app/ponto/services/esmeril_espelho.py:98`; `app/ponto/services/triagem_batida.py:552`; `app/ponto/services/regularizacao.py:61`; `app/ponto/services/passivo_s84.py:82` |
| Comandos | `app/ponto/management/commands/detectar_par_relampago.py:101`; `app/ponto/management/commands/dna_x_batida_real.py:69`; `app/ponto/management/commands/marcar_abandono_provisorio.py:46`; `app/ponto/management/commands/detectar_ausencias.py:336`; `app/ponto/management/commands/vigia_plantio_overnight.py:38`; `app/ponto/management/commands/verificar_nucleo.py:29`; `app/ponto/management/commands/e6_oraculo.py:147`; `app/ponto/management/commands/censo_fase_12x36.py:180`; `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124` |
| Testes de ponto | `app/ponto/tests/test_retratada_geometria.py:39`; `app/ponto/tests/test_turnos_borda_direita.py:37`; `app/ponto/tests/test_pareador_dna_multivig.py:55`; `app/ponto/tests/test_bateria_lavratura.py:1406`; `app/ponto/tests/test_vinculo_do_dia_pela_celula.py:144`; `app/ponto/tests/test_vinculo_do_dia_pela_celula.py:156`; `app/ponto/tests/test_pareador_dna.py:50`; `app/ponto/tests/test_o280_bateria_da_lei.py:3010`; `app/ponto/tests/test_f9d_n1_caracterizacao_eco_flush.py:96`; `app/ponto/tests/test_turnos_do_colab_troca_escala.py:53` |
| Outros testes | `app/relatorios/tests/test_pdf_le_o_juiz_do_dia.py:61`; `app/relatorios/tests/test_pdf_sem_regra_propria.py:94`; `app/escala/tests/test_ata_x_turno_entrada.py:82`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:96`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:199`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:76`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:90`; `app/escala/tests/test_saida_valida_vira_orfa.py:92`; `app/escala/tests/test_saida_valida_vira_orfa.py:196`; `app/api/tests/test_espelho_app_fonte_e_geometria.py:322`; `app/chamados/tests/test_orfas_pelo_evento.py:391` |

O caminho que chega ao **veredito gravado** é o do cartório: `_turno_encerrado` alimenta o classificador e a impressão de dia em curso; a lavra recebe os códigos resultantes. Calendário e painel são leitores/apresentação nesses caminhos. (`app/ponto/services/cartorio.py:942`, `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:1253`, `app/ponto/services/cartorio.py:1596`, `app/colaboradores/services/calendario.py:701`, `app/core/services/painel_op.py:338`)

**3. Quem escreve, quem lê e qual juiz responde**

| Dado/resultado | Escritor/produtor | Leitor no fluxo auditado |
|---|---|---|
| `trabalha`, geradora e DNA | Geração/regeneração pela porta; `montar_dna` constrói o blob e seus marcos. (`app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:260`, `app/ponto/portas/celula.py:593`, `app/ponto/portas/celula.py:612`, `app/ponto/management/commands/gerar_celulas.py:73`) | Geradora: candidatura e vínculo diário; DNA: marcos e papel; `trabalha`: previsão de trabalho. (`app/ponto/turnos.py:1886`, `app/escala/alimentacao.py:238`, `app/escala/models.py:1099`, `app/ponto/juiz_batida.py:158`, `app/escala/models.py:1072`) |
| Ata do julgamento | `ata_do_dia` monta natureza, lâmpadas, minutos, órfãs e contagens; cartório entrega à lavra. (`app/ponto/services/cartorio.py:599`, `app/ponto/services/cartorio.py:607`, `app/ponto/services/cartorio.py:623`, `app/ponto/services/cartorio.py:1594`) | Papel pelo juiz de batida; projeção de grade pela célula. (`app/ponto/juiz_batida.py:157`, `app/escala/services/leitor_celula.py:332`) |
| Ata persistida | `lavrar_veredito` escreve `cel.ata`; `carimbar` persiste junto dos campos lavrados. Há ainda a operação específica que escreve `ata['precedencia']`. (`app/ponto/portas/celula.py:741`, `app/ponto/portas/celula.py:768`, `app/ponto/portas/celula.py:807`, `app/ponto/portas/celula.py:819`) | Folha, relatório e cobrança também leem a ata. (`app/folha/previa.py:83`, `app/folha/validacao_pdf.py:47`, `app/relatorios/cartao_pela_celula.py:287`, `app/chamados/juizes.py:407`) |
| Papel por minuto | `papel_por_minuto_da_ata` produz o mapa; o pareador escreve `_papel_ata` nas instâncias em memória. (`app/ponto/turnos.py:1804`, `app/ponto/turnos.py:1828`, `app/ponto/turnos.py:1313`) | `_papel_da_ata` consulta o mapa; pareador decide borda/intervalo; outros passos leem o carimbo. (`app/ponto/turnos.py:903`, `app/ponto/turnos.py:1314`, `app/ponto/turnos.py:1380`, `app/ponto/turnos.py:875`, `app/ponto/turnos.py:996`) |
| Resultado de `vinculo_do_dia` | Retorno calculado, sem gravação nessa função. (`app/escala/alimentacao.py:247`, `app/escala/alimentacao.py:252`, `app/escala/alimentacao.py:254`) | Segmentação, encerramento e cadastro do classificador. (`app/ponto/turnos.py:1958`, `app/ponto/turnos.py:2164`, `app/ponto/supra_juiz.py:161`) |
| Veredito, protestos, via e instante | Porta de lavra; o modelo delega a ela. (`app/ponto/portas/celula.py:769`, `app/ponto/portas/celula.py:770`, `app/ponto/portas/celula.py:771`, `app/ponto/portas/celula.py:772`, `app/escala/models.py:1787`) | Projeção da célula e regra do status da mesma lavra. (`app/escala/services/leitor_celula.py:366`, `app/ponto/portas/celula.py:752`) |
| `status_dia`, `status_rotulo` | Regra `status_da_lavra`, aplicada pela porta junto do veredito; decisão administrativa tem tratamento próprio. (`app/ponto/portas/celula.py:693`, `app/ponto/portas/celula.py:695`, `app/ponto/portas/celula.py:698`, `app/ponto/portas/celula.py:705`) | D4 lê as colunas após consultar o encerramento; painel recebe o resultado em `linha['dia']`. (`app/ponto/status_dia.py:196`, `app/ponto/status_dia.py:203`, `app/ponto/status_dia.py:205`, `app/core/services/painel_op.py:338`) |

As chaves da ata têm leitores específicos: `lampadas`/`orfas` alimentam o juiz de batida; contagens, minutos e natureza alimentam `grade_da_celula`; o cartório também extrai entrada/saída da ata para carimbar a célula. (`app/ponto/juiz_batida.py:160`, `app/ponto/juiz_batida.py:161`, `app/escala/services/leitor_celula.py:332`, `app/escala/services/leitor_celula.py:345`, `app/escala/services/leitor_celula.py:355`, `app/ponto/services/cartorio.py:1615`)

Outros leitores diretos da ata aparecem em pautas, feriados, worklist, materialização, detectores e vigias: `app/pautas/services.py:422`; `app/colaboradores/services/feriado_do_posto.py:96`; `app/ponto/worklist.py:22`; `app/chamados/services/materializacao.py:157`; `app/ponto/management/commands/detectar_intervalo_ausente.py:47`; `app/ponto/services/cluster_espurio.py:24`; `app/ponto/services/vigia_entrada.py:80`; `app/ponto/services/dia_do_colab.py:29`.

O cruzamento com o anexo deve usar o **registro vivo**, como o próprio documento determina. (`app/docs/JUIZES.md:16`)

| Pergunta tocada | Juiz registrado |
|---|---|
| A qual turno pertence a batida? | `turnos_do_colab`. (`app/core/juizes.py:196`) |
| Há turno aberto vivo? | `turno_aberto_de`. (`app/core/juizes.py:199`) |
| Quais são os marcos do dia? | `EscalaColaborador.marcos_do_dia`. (`app/core/juizes.py:201`) |
| Cruza meia-noite? | `turno_cruza_meia_noite`. (`app/core/juizes.py:204`) |
| O dia está encerrado? | `dia_encerrado`; `dias_encerrados` é carregador/composição. (`app/core/juizes.py:223`, `app/ponto/turnos.py:2134`) |
| Qual vínculo vale e se cobre a data? | `vinculo_do_dia` e `vinculo_cobre_a_data`. (`app/core/juizes.py:1275`, `app/core/juizes.py:1276`) |
| Dia de trabalho, previsto e precedência? | `eh_dia_trabalho`, `minutos_previstos_do_dia`, `fatos_do_dia`. (`app/core/juizes.py:172`, `app/core/juizes.py:173`, `app/core/juizes.py:175`) |
| Qual status do dia? | `status_do_dia_lavrado`. (`app/core/juizes.py:187`) |
| Cobrança deve calar por dia sem trabalho? | `chamados/juizes.py::dia_sem_trabalho`. (`app/core/juizes.py:278`) |
| Folga lançada vale no dia? | `folga_lancada_do_dia`, alimentada pelo vínculo diário. (`app/core/juizes.py:192`, `app/docs/JUIZES.md:84`) |

A lista resumida do anexo não inclui `dia_encerrado` no bloco de geometria, mas o registro vivo o registra expressamente; “lavra/fechamento: SEM juiz” é outra pergunta e não transforma `CargaDaJanela` ou a porta escritora em autoridade de encerramento. (`app/docs/JUIZES.md:20`, `app/core/juizes.py:223`, `app/docs/JUIZES.md:71`, `app/ponto/turnos.py:2934`)

**4. Teste existente e caso mínimo discriminante** [nao verificado]

**Existe teste com células fornecidas e template noturno.** Ele cria DNA `10–14` sobre template `19–07`, passa o mapa composto e compara os motivos com a porta singular. Porém, não cria batidas nesse teste, não nomeia uma geradora na célula e herda uma pessoa com somente um vínculo. Portanto, verifica o fim previsto por DNA, mas não distingue a perda da geradora no pareamento. (`app/ponto/tests/test_c1_dias_encerrados.py:53`, `app/ponto/tests/test_c1_dias_encerrados.py:62`, `app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/ponto/tests/test_c1_dias_encerrados.py:70`, `app/ponto/tests/test_juiz_dia_encerrado.py:69`)

Não foi localizado teste que reúna **`dias_encerrados` com células externas + múltiplos vínculos + batidas noturnas**. Ausência de outro teste fora do inventário apresentado: `[nao verificado]`. [nao verificado]

Um caso mínimo proposto, ainda não executado, é:

- Pessoa com vínculo ativo A `08–17`, pausa `07–08`; vínculo B inativo, sem `data_fim`, início anterior a D, `19–07`, sem pausa. A célula de D nomeia B. A geradora nomeada pode vencer mesmo inativa; sem enxergá-la, o fallback escolhe A. (`app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:250`, `app/escala/alimentacao.py:253`, `app/escala/alimentacao.py:254`)
- Apenas célula D, DNA `19–07`, ata vazia; batidas `D 19:00 E` e `D+1 07:00 S`, sem batida posterior; observar em `D+1 08:00`. Sem DNA em D+1, as distâncias usam os marcos do template escolhido. (`app/ponto/turnos.py:1227`, `app/ponto/turnos.py:1235`)
- Comparar `dias_encerrados(..., celulas=composto)` com a mesma chamada acrescida de `turnos=turnos_do_colab(..., celulas={D: cel})`, mantendo `[D,D]`. Essa alimentação evita o repasse incompatível. (`app/ponto/turnos.py:2148`)

| Caminho | Saída previst

[truncado em 20000 de 24647 caracteres]

## 01/11 14:08 · sessao 7330d4c01221 · - · - · origem=Ronald (tmux)

Proposta pela **falha residual na cópia final**: **1=a, 2=c, 3=a, 4=b, 5=b, 6=e, 7=b condicionado, 8=b**.

- **1:** a só com ressalva: o Codex viu exatamente a fabricação por CLI/log sintético/Write no `raiz/codex_v1.r` #13. A triagem declarou #13 **curado**, sem descarte. A revisão v2, perguntada sobre curas, confirmou apenas Ran/OK e perdeu o residual. Não usar descarte da guarda lexical #8 como substituto.
- **2:** c para execução real/prova ponta a ponta. A insuficiência estática do grep e do log sintético já foi vista em v1 e v2; não há descarte explícito dessa parte.
- **3:** a: derivação paralela estava no checklist; nenhuma resposta apresentada identifica especificamente as duas regex como duplicação. “NENHUM viu” além dos registros fornecidos é [nao verificado].
- **4:** b inequívoco: v1.r #14b viu ABA; v1_triagem mantém em fila.
- **5:** b inequívoco: v1.r, v2.r, v3.r e v4/v2.r viram; triagens raiz/v2 e v4/v2 dizem fila.
- **6:** e para pertinência semântica contrato↔diff e `--no-verify`: perguntas sobre existência/escritores/ordem do gate não explicitam essas garantias adicionais.
- **7:** v4/v1.r #5 viu literalmente remoção de outra sessão viva. **Falta descarte explícito nas triagens fornecidas.** v4/v3_triagem c3 só corrige `poda_seq` e remete ao aval CONTENCAO-SESSOES. Se exigir rigor literal, nenhuma classe atende integralmente: viu, mas não há linha de descarte. “Coberto pelo aval” consta no opus, não comprova descarte na triagem.
- **8:** b inequívoco: raiz/v2.r #5 e v3.r #3; raiz/v2_triagem descarta para fila por desenho TTL/teto.

A taxonomia tem duas limitações documentais: não prevê “viu e triagem declarou curado incorretamente” (#1) nem “viu, sem destino explícito desse risco na triagem” (#7). Convém declarar isso para não fabricar evidência.

## 01/11 14:07 · sessao 509803024587 · - · - · origem=Ronald (tmux)

Consolidado enviado com os chamadores e formatos. O ponto decisivo é:

- `dias_encerrados` repassa o mapa composto a `turnos_do_colab`, que reembrulha as chaves e impede a leitura da geradora na segmentação (`app/ponto/turnos.py:2148`, `app/ponto/turnos.py:1954`, `app/ponto/turnos.py:1958`).
- O papel da ata continua lendo os objetos por `.items()`; a chave composta, sozinha, não o torna invisível (`app/ponto/turnos.py:1629`, `app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1702`, `app/ponto/turnos.py:1729`).
- Existe teste com células fornecidas e noturno, mas ele não cria batidas e herda um único vínculo; portanto não distingue o defeito de segmentação (`app/ponto/tests/test_c1_dias_encerrados.py:53`, `app/ponto/tests/test_c1_dias_encerrados.py:62`, `app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/ponto/tests/test_juiz_dia_encerrado.py:69`).
- O caso mínimo proposto pelo coordenador distingue estaticamente `False / turno_aberto_relogio` de `True / None`; execução `[nao verificado]` (`app/ponto/turnos.py:1354`, `app/ponto/turnos.py:1169`, `app/ponto/turnos.py:2114`, `app/ponto/turnos.py:2119`).

## 01/11 14:06 · sessao effa75d5be39 · - · - · origem=Ronald (tmux)

Rastreio de escritores/leitores e registro de juízes:

- **Ata produzida:** `app/ponto/services/cartorio.py:591` monta `tipo_dia`, `lampadas`, `minutos_previstos`, `minutos_realizados`, `sem_ancora`, `orfas`, `n_orfas`, `n_missing`, `n_celulas` (`:596-623`). Ramo principal passa ata e fatos para lavra em `:1594-1601`; ramo sem batida em `:1075-1080`.
- **Ata/veredito/status gravados:** modelo apenas delega (`app/escala/models.py:1787-1796`). Porta `lavrar_veredito` copia/compara ata (`app/ponto/portas/celula.py:726-736`), escreve ata (`:741`, `:768`), protestos/veredito/via/instante (`:769-773`), aplica status (`:686-705`). Cartório usa `save=False` e depois `carimbar` (`app/ponto/services/cartorio.py:1615-1617`); `carimbar` acrescenta `julgada_em`, impressão, ausência, `tem_entrada/tem_saida`, `insumos_em` e persiste todos os campos lavrados (`app/ponto/portas/celula.py:800-807`), com `save(update_fields=...)` em `:640`.
- **Outro escritor legítimo da ata:** `carimbar_precedencia` escreve exclusivamente `ata['precedencia']` e salva pela mesma porta (`app/ponto/portas/celula.py:811-822`). Portanto, “lavrar_veredito é o único escritor absoluto de qualquer chave de ata” seria impreciso; é a operação escritora do julgamento (`:709-713`).

**Papel da ata: a alegação da fila precisa ser corrigida.** `papel_por_minuto_da_ata` itera `celulas.items()` (`app/ponto/turnos.py:1629`), chama `periodos_do_dia(cel, te)` (`:1631`), que lê `celula.ata/dna` (`app/ponto/juiz_batida.py:157-160`) e usa `celula.data`, não a chave do mapa, ao reconstruir instantes (`:174-176`). A chave `_d` só armazena/intera baldes (`app/ponto/turnos.py:1702`, `:1729`); os papéis saem indexados por minuto (`:1802-1804`, `:1828`). Logo, mapa com chaves `(cid,data)` viola o contrato declarado `{data:CelulaDia}` (`:1921-1924`), mas **isso isoladamente não torna essa leitura de ata cega**. O pareador consulta o papel em `_papel_da_ata` (`:888-907`, especialmente `.get(_m)` em `:903`), recebe-o em `:1308` e carimba `_papel_ata` em `:1313`.

**Incompatibilidade efetiva com vínculo:** `_turnos_do_colab_puro` transforma o mapa recebido com `{(_cid,_d):_c for _d,_c in celulas.items()}` (`app/ponto/turnos.py:1954`); se `_d` já é `(cid,data)`, forma `(cid,(cid,data))`. `vinculo_do_dia` consulta `(cid,data)` (`app/escala/alimentacao.py:237`), não vê a geradora (`:238`), e usa candidatura por vigência/desempate (`:253-254`) em lugar da geradora da célula (`:247-252`).

**Cartório não está inteiro cego à geradora:** o carregador próprio de cadastro passa todos os vínculos e mapa correto em `app/ponto/services/cartorio.py:931-933`; `cadastro_do_dia` consulta o juiz e `celulas.get((cid,data))` em `app/ponto/supra_juiz.py:160-162`. O caminho afetado pelo repasse a `dias_encerrados` é `_turno_encerrado` (`app/ponto/services/cartorio.py:939-943`), usado em classificação (`:1063`, `:1253`) e em `_em_curso` (`:1180`). Dano concreto ou direção da mudança em produção: `[nao verificado]`.

**Leitores da ata e campos:**

- `periodos_do_dia`: ata, DNA, lâmpadas e órfãs (`app/ponto/juiz_batida.py:157-161`), alimentando papel e pareador pelos pontos acima.
- `grade_da_celula`: `n_celulas/n_missing/minutos_realizados` (`app/escala/services/leitor_celula.py:332-334`), `tipo_dia/minutos_previstos` (`:345-347`), lâmpadas/marcos/órfãs e contagens (`:355-365`), veredito/id (`:366`) e insumo do juiz `dia_sem_trabalho` (`:367-370`).
- Folha: prévia (`app/folha/previa.py:83`), validação PDF (`app/folha/validacao_pdf.py:47`), lâmpadas apagadas (`app/folha/services/sem_furo.py:20-22`); cartão relatório (`app/relatorios/cartao_pela_celula.py:286-287`).
- Cobrança: leitores diretos da ata em `app/chamados/juizes.py:407`, `:442`, `:1008`; materialização em `app/chamados/services/materializacao.py:157`; cartório publica ata do julgamento corrente em `app/ponto/services/cartorio.py:1219`, exposta em `:48-57`, e o leitor C6 projeta essa ata corrente em `app/chamados/juizes.py:392-394`.
- Status gravado: único leitor funcional localizado é `app/ponto/status_dia.py:203-205`; antes dele, a decisão usa `carga.turnos` e `carga.dias_encerrados` (`:195-201`). Sem célula produz `sem_dados` (`:191-194`); sem status lavrado usa previsto/sem_certidão (`:207-208`).
- Painel: `_d4_em_lote` cria carga em janela `[hoje−2,hoje]`, escreve `linha['data_turno']`, só pede D4 para dias encerrados e escreve `linha['dia']` (`app/core/services/painel_op.py:327-338`). D4 cria outra carga por data distinta (`app/ponto/status_dia.py:188-189`).

**Juízes vivos cruzados com anexo:**

| Pergunta | Registro vivo | Autoridade |
|---|---|---|
| Dia é de trabalho | `app/core/juizes.py:172` | `EscalaColaborador.eh_dia_trabalho` |
| Minutos previstos | `app/core/juizes.py:173` | `escala/utils.py::minutos_previstos_do_dia` |
| Precedência | `app/core/juizes.py:174-175` | `ponto/precedencia.py::fatos_do_dia` |
| Status do dia | `app/core/juizes.py:187` | `status_do_dia_lavrado` |
| Folga lançada válida | `app/core/juizes.py:192` | `folga_lancada_do_dia` |
| Turno da batida | `app/core/juizes.py:196` | `turnos_do_colab` |
| Turno aberto | `app/core/juizes.py:199` | `turno_aberto_de` |
| Marcos previstos | `app/core/juizes.py:201` | `EC.marcos_do_dia` |
| Cruza meia-noite | `app/core/juizes.py:204` | `turno_cruza_meia_noite` |
| Dia encerrado | `app/core/juizes.py:223` | `dia_encerrado` |
| Par completo | `app/core/juizes.py:228` | `par_completo` |
| Cobrança cala em dia sem trabalho | `app/core/juizes.py:278` | `chamados/juizes.py::dia_sem_trabalho` |
| Vínculo do dia/cobre data | `app/core/juizes.py:1275-1276` | `vinculo_do_dia`/`vinculo_cobre_a_data` |

O anexo local confirma geometria (`app/docs/JUIZES.md:20`), dia sem trabalho (`:55`), status (`:82-83`), vínculo/vigência (`:92-95`). O registro vivo acrescenta expressamente `dia_encerrado` (`app/core/juizes.py:223`), ausente da lista resumida de geometria do anexo. “Lavra/fechamento: SEM juiz” no anexo é outro domínio (`app/docs/JUIZES.md:68-71`); a porta escritora da célula não deve ser apresentada como juiz novo de encerramento ou de formato.

## 01/11 14:06 · sessao cf00d4a9f629 · - · - · origem=Ronald (tmux)

Auditoria estática, somente leitura.

| Carga | Casca singular | `CargaDaJanela` | Conclusão |
|---|---|---|---|
| Janela | `janela_com_pad`: `ini−2..fim+1` (`app/ponto/turnos.py:1845`); usada em `:1860` | Mesma função em `:2948` | Mesma janela de pad no código (`:1860`, `:2948`). |
| Células | Sem alimentação, carrega pad e converte tupla→data (`:1882–1884`) | Carrega pad uma vez, agrupa por colaborador/data (`:2951–2955`), converte ao chamar núcleo (`:2996–3005`) | Contrato padrão equivalente; lote faz a conversão necessária (`:3005`). |
| Candidatos dos turnos | `escalas_do_periodo(colab,pad_ini,fim,mais=geradoras)` (`:1885–1886`) | `escalas_do_periodo_por_colab(ids,pad_ini,fim,mais=geradoras)` (`:2957–2958`) | Mesma porta: singular delega à plural (`app/escala/alimentacao.py:153–154`). |
| Todos os vínculos para encerramento | Sem filtro de atividade/data, com `tipo_escala` (`app/ponto/turnos.py:2146`) | Mesmo universo em lote (`:2961–2962`) | Sem diferença estática identificada nesse universo (`:2146`, `:2961`). |
| Batidas | Vivas, `timestamp__date` no pad (`:1861–1864`) | `batidas_apuraveis(None)`, mesmos cortes, agrupadas/ordenadas (`:2990–2994`) | `batidas_apuraveis` impõe `retratada_em IS NULL` (`:2872`); universo equivalente nos cortes mostrados. |
| Ata | Núcleo recebe `escalas_ata=None` (`:1913`); papel chama `_escalas_da_ata` (`:1591`) | Pré-carrega `_escalas_da_ata(ids)` (`:2963`) e fornece ao núcleo (`:3005`) | Mesma seleção compartilhada (`:1536–1560`). |
| DNA | Provedor consulta `marcos_dna_periodo` sem células (`:1907–1910`) | Fornece células já carregadas (`:2999–3002`) | Singular pode reler células/DNA; lote reutiliza snapshot (`app/escala/utils.py:96–102`). |
| Turnos | Cada chamada à casca chama núcleo (`:1913`) | Cache por colaborador (`:2995–3007`) | Lote reutiliza turnos entre encerramento e dia das batidas (`:3012–3018`). |

A candidatura admite vínculos ativos ou encerrados com `data_fim`, sobrepostos à janela (`app/escala/alimentacao.py:116–122`). `mais=` busca os IDs ausentes sem o filtro de vigência/atividade e os coloca no balde do respectivo dono (`:123–127`). Portanto, geradora inativa chega por ambas as cargas; a escolha diária ainda aplica a lei da geradora e sua vigência (`:237–254`). Não identifiquei divergência de universo padrão entre lote e casca nesses pontos (`app/ponto/turnos.py:1885–1886`, `:2957–2958`).

Há duplicação da **implementação da carga**: casca em `app/ponto/turnos.py:1859–1913`, lote em `:2944–3006`; o julgamento é compartilhado em `_turnos_do_colab_puro` (`:1916`) e `_dias_encerrados_puro` (`:2152`). Além disso, sem argumentos alimentados, `dias_encerrados` carrega células de `[ini,fim]` e todos os vínculos (`:2145–2146`), chama a casca com `celulas=None` (`:2148`), e esta carrega novamente células com pad e candidatos (`:1882–1886`); o DNA pode custar outra leitura (`:1910`; `app/escala/utils.py:100–102`).

O painel monta carga de `[hoje−2,hoje]` para descobrir dia da entrada e encerramento (`app/core/services/painel_op.py:327–335`), depois chama D4 (`:336`), que monta outra carga para cada data distinta, agora `[data,data]` (`app/ponto/status_dia.py:188–196`). São janelas intencionalmente distintas no código; equivalência dos turnos entre essas janelas não deve ser presumida (`app/ponto/status_dia.py:176–179`).

**Selo existente e lacuna:** o teste D4 contém noturno e várias pessoas/datas (`app/core/tests/test_e4_7iii_lote_d4.py:67–109`), mas cada pessoa nasce com um vínculo ativo (`app/core/tests/test_e4_7iii_painel_e_presenca.py:54–59`; noturno em `app/core/tests/test_e4_7iii_lote_d4.py:101–108`). Não exercita pessoa com múltiplos vínculos nem geradora inativa nessa fixture (`:67–109`). A “paridade lote/singular” compara `status_dos_dias_lavrados` com `status_do_dia_lavrado` (`:136–147`), e este último já delega ao lote com um par (`app/ponto/status_dia.py:167–170`): não é selo independente de `CargaDaJanela` contra a casca `turnos_do_colab`.

Existe teste alimentando `dias_encerrados` com células de `celulas_do_periodo`, template noturno e DNA divergente (`app/ponto/tests/test_c1_dias_encerrados.py:53–77`); ele não cria batidas nesse teste (`:62–77`) e a pessoa tem somente um vínculo ativo (`app/ponto/tests/test_juiz_dia_encerrado.py:48–70`). Assim, não distingue a perda da geradora durante a montagem dos turnos.

**Correção importante da descrição da fila:** o papel da ata itera os **valores** de `celulas` (`app/ponto/turnos.py:1629–1631`); a chave `_d` apenas indexa `_dias` (`:1702`) e é iterada sem busca por data (`:1729`). Nesse consumidor, trocar chave data por tupla não basta para esconder a célula. A perda demonstrável ocorre ao reencapsular uma chave já composta como `(_cid, (cid,data))` (`:1954`), que não será encontrada por `vinculo_do_dia`, cujo acesso é `.get((colaborador_id,data))` (`app/escala/alimentacao.py:237`). Com mapa errado, fallback passa a escolher por vigência/atividade (`:253–254`) em vez da geradora (`:237–252`).

Execução de testes e consequências em dados de produção: `[nao verificado]`. [nao verificado]

## 01/11 13:46 · sessao 8911ef9e5156 · O261-E-F2 · censo · origem=Code

A F1 está **parcialmente realizada**: há gate, pós-estado e tripwire, mas a absorção prevista da S127 não ocorreu integralmente. A F2 ainda tem vários escritores diretos, sem função comum de escrita. (`app/chamados/models.py:147`, `app/chamados/models.py:195`, `app/chamados/models.py:391`, `app/core/tests/test_contract_escritores_estado.py:83`)

O inventário abaixo separa código operacional, atribuições somente em memória e definições de schema. Execução dos testes, sondas de produção e confirmação Git do hash informado: **[não verificado]**. [nao verificado]

**1. Passo-a-passo da F1**

| Passo do anexo | Situação encontrada | Sítios |
|---|---|---|
| **1 — Medir desligamento e RECONCILIAR** | **Documentado; execução primária [não verificado].** O relato registra 43 vivas sob 25 chamados de desligados; 106 fechamentos possíveis, dos quais 18 reatariam e 88 recusariam. A ordem do desligamento foi efetivamente invertida: disputas antes dos chamados. | Registro: `app/docs/RELATO.md:270`. Ordem atual: `app/colaboradores/services/desligamento.py:139`, `app/colaboradores/services/desligamento.py:150`. |
| **2 — RED: viva na âncora, pós-estado e costura em terminal** | **Testes presentes.** Execução histórica em RED contra a base: **[não verificado]**. | Âncora destino: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`; container: `:94`; pós-estado/slot não derivável: `:172`; célula acusadora: `:179`; costura terminal: `:252`; dedup de `abrir`: `:275`. |
| **3 — Gate reata/recusa; humano levanta; absorver `fio_vivo` da S127** | **Parcial.** Sistema compõe premissa e guarda da pergunta; reata por pergunta ou registra recusa. Humano levanta. Entretanto, a S127 continua consultando `disputa_viva_do_chamado`, cujo corpo ainda usa `fio_vivo`. | Composição: `app/chamados/models.py:627`, `:646`, `:649`; plano por pergunta: `:355`, `:371`, `:373`; reaponte: `:418`; recusa: `:378`, `:403`; humano: `:159`. S127 remanescente: `app/chamados/models.py:147`, `app/chamados/juizes.py:1967`. |
| **4 — Tripwire vivo→TERMINAIS com pós-estado** | **Mecanismo realizado.** Detecta também instância velha, consulta as perguntas no pós-estado e levanta antes da escrita. **Ressalva ao “mesmo atomic”:** a checagem antecede o `atomic` interno de `_gravar_estado`; os gates de sistema envolvem decisão e gravação em transação, mas `transicionar`→`_aplicar` não abre essa transação externa no próprio modelo. | Tripwire: `app/chamados/models.py:213`, `:215`, `:218`, `:220`; pós-estado e restauração: `:332`, `:339`, `:348`, `:349`, `:352`; `atomic` interno: `:274`; sistema: `:614`; humano: `:122`, `:428`. |
| **5 — Costura em terminal, ou declaração para F2** | **Realizado para reaponte de perguntas existentes; nascimento declarado para F2.** A guarda ficou separada de `resolvedora_valida`. O ramo de pergunta nova ainda cria diretamente. | `_costura_nao_mata_o_fio`: `app/chamados/services/fio.py:742`; declaração do nascimento para F2: `:750`; aplicação no reaponte: `:840`; dedup existente: `app/chamados/models.py:2424`; nascimento: `:2453`. |
| **6 — Selo AST com mutante** | **Presente em fonte.** Execução verde e morte efetiva dos mutantes nesta cópia: **[não verificado]**. | Selo do tripwire: `app/chamados/tests/test_o261e_f1_selo_encerramento.py:24`, `:55`; mutantes: `:64`, `:68`, `:73`, `:78`, `:83`, `:87`. Selo amplo de status: `app/chamados/tests/test_selo_status_so_pelas_portas.py:340`; mutante de defaults: `:421`. |
| **7 — `superar` sai por L-111** | **Realizado como remoção do método.** Há lápide; o estado histórico `superado` permanece no catálogo. | `app/chamados/models.py:755`, `app/chamados/models.py:759`. |
| **8 — DIFF de quem entra no TXT, aberta antes/depois, exportada por hash** | **Documentado; prova primária [não verificado].** O relato registra base 116×cura 29, diferença nominal de entrada e 26 exportadas com hash igual; uma rodada posterior registra ENTRA 347→347. | `app/docs/RELATO.md:270`, `app/docs/RELATO.md:252`, `app/docs/RELATO.md:253`. |

A decisão de RECONCILIAR continua compartilhada com o dry-run: `reconciliaria()` consulta `_decisao_do_fechamento(registrar=False)`. A guarda nova está em `_gate_pergunta_viva`, composta com `_gate_premissa_do_fechamento`, em vez de morar integralmente nesta última. (`app/chamados/models.py:621`, `app/chamados/models.py:627`, `app/chamados/models.py:646`, `app/chamados/models.py:649`)

A linha da obra ainda diz “F1 em construtor”, portanto esse rótulo documental não certifica conclusão dos oito passos. (`app/docs/BACKLOG.md:820`)

**2. Escritores e leitores de `materializada_em`**

Há dois campos homônimos: o de `PerguntaDisputa` e o de `RegularizacaoExterna`. (`app/chamados/models.py:2659`, `app/chamados/models.py:3038`)

Todos os escritores operacionais explícitos localizados estão abaixo. “C6 antes” significa consulta local antes da atribuição; isso **não equivale** à porta F2 comum que carimba em memória e consulta C6 sobre o resultado, pois esses corpos persistem separadamente. (`app/chamados/reconciliador.py:399`, `app/chamados/reconciliador.py:402`, `app/chamados/services/disputa_emissao.py:1547`, `app/chamados/services/disputa_emissao.py:1562`)

| Função | Atribuição e persistência | Porta/guarda existente |
|---|---|---|
| `PerguntaDisputa.fechar_por_admin` | `app/chamados/models.py:2500`; `save(update_fields=...)` em `:2502` | Gate Q1; idempotência em `app/chamados/models.py:2490`; sem chamada C6 no corpo. |
| `PerguntaDisputa.fechar_por_competencia_trancada` | `app/chamados/models.py:2514`; save em `:2516` | Gate Q1; condição própria em `app/chamados/models.py:2511`; sem C6 no corpo. |
| `PerguntaDisputa.encerrar_fantasma` | `app/chamados/models.py:2539`; save em `:2541` | Gate próprio; idempotência em `app/chamados/models.py:2536`; sem C6 no corpo. |
| `PerguntaDisputa.reabrir_fantasma` | Limpa em `app/chamados/models.py:2554`; save em `:2556` | Inversa da via, condicionada em `app/chamados/models.py:2551`. |
| `PerguntaDisputa.reabrir_por_resposta` | Limpa em `app/chamados/models.py:2574`; devolve campos em `:2576` | Quem persiste é `responder_pergunta_disputa_core`, somando os campos e salvando: `app/chamados/services/disputa_emissao.py:1265`, `:1266`. |
| `PerguntaDisputa.reabrir_por_competencia` | Limpa em `app/chamados/models.py:2584`; save em `:2586` | Inversa da via, condicionada em `app/chamados/models.py:2581`. |
| `ChamadoColaborador.ressuscitar_perguntas` | `update(materializada_em=None, ...)` em `app/chamados/models.py:1488` | Seleção pelo universo comum em `:1479`; C6 julga uma cópia em memória em `:1445`. |
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:402`; save em `:407` | C6 antes da atribuição: `app/chamados/reconciliador.py:399`. |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:621`; save em `:624` | C6 antes, mais exclusão de dia trancado: `app/chamados/reconciliador.py:611`, `:617`. |
| `_materializar_ausencia_de_resposta` | `app/chamados/services/materializacao.py:511`; save em `:530` | Escrita própria após criar ausência em `app/chamados/services/materializacao.py:501`; sem guarda C6 comum. |
| `_materializar_resposta_hora` | `app/chamados/services/materializacao.py:860`; save em `:870` | Ramo de identidade `paridade_recusada`; condição da lâmpada em `app/chamados/services/materializacao.py:843`. |
| `_carimbar_recusa` | `app/chamados/services/materializacao.py:1045`; campos em `:1053`, save em `:1055` | Só carimba `COLISAO` com campo nulo; usa `fato_ocupado`: `app/chamados/services/materializacao.py:1044`, `:1046`. |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1151`; campos em `:1186`, save em `:1200` | Unidade compartilhada por duas filas; atomic e juiz do lastro, sem guarda C6 comum pós-carimbo: `app/chamados/services/materializacao.py:1126`, `:1139`. |
| `materializar_pendentes_aprovadas` | `app/chamados/services/materializacao.py:1341`; save em `:1344` | Consulta `_pergunta_materializada_de_fato` antes: `app/chamados/services/materializacao.py:1339`. |
| `resolver_perguntas_por_chamado_resolvido`, primeiro ramo S128 | `app/chamados/services/disputa_emissao.py:1562`; save em `:1583` | C6 antes e exclusão da tranca: `app/chamados/services/disputa_emissao.py:1547`, `:1551`. |
| Mesma função, ramo sem credor | `app/chamados/services/disputa_emissao.py:1644`; save em `:1659` | C6 antes: `app/chamados/services/disputa_emissao.py:1641`. |
| `reabrir_para_correcao` | Limpeza montada em dicionário: `app/chamados/services/disputa_emissao.py:1360`; `qs.update(**campos)` em `:1377` | Escrita dinâmica da porta de reabertura. |
| `retratar_duplicata` | `app/chamados/services/fio.py:894`; save em `:896` | Identidade; só preenche carimbo nulo: `app/chamados/services/fio.py:893`. |
| `desfazer_carimbo_sem_lastro` | Limpa em `app/chamados/services/relavra_via.py:231`; save em `:234` | Relê travada e exige C6 viva: `app/chamados/services/relavra_via.py:221`, `:227`. |
| **Outro modelo:** `materializar_regularizacao_externa` | `app/chamados/services/regularizacao_ext.py:198`; save em `:200` | Escreve `RegularizacaoExterna`, não a pergunta: `app/chamados/models.py:3038`. |

Há ainda uma atribuição **somente em memória**: `_universo_da_ressuscitacao` faz `copy.copy(p)`, limpa o campo na cópia e consulta C6. Essa atribuição não é salva; a persistência posterior é o `update` de `ressuscitar_perguntas`. (`app/chamados/models.py:1427`, `app/chamados/models.py:1440`, `app/chamados/models.py:1445`, `app/chamados/models.py:1488`)

`PerguntaDisputa.abrir(**campos)` também possui capacidade genérica de criação com campos extras: repassa `**campos` a `objects.create`. Chamador operacional passando `materializada_em` por essa capacidade: **[não verificado]**. (`app/chamados/models.py:2453`)

Não foi localizado `bulk_update` ou SQL cru explicitamente escrevendo esse campo; cobertura de SQL/nomes de campo inteiramente construídos em execução: **[não verificado]**. O `bulk_update` da lavra encontrado escreve outro conjunto de campos. (`app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:207`)

As migrações localizadas introduzem os campos no schema, sem carimbo de dados nesses sítios. (`app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:23`, `app/chamados/migrations/0021_regularizacaoexterna_regularizacaoitembatida_and_more.py:26`)

O compartilhamento existente é **parcial**: `_materializar_uma` atende a unidade de trabalho do admin e do passivo; os demais escritores da tabela não passam por ela. (`app/chamados/services/materializacao.py:1065`, `app/core/portas.py:284`, `app/chamados/tests/test_porta_desfecho_pergunta.py:14`)

Quanto ao dinheiro mencionado no F0: `_materializar_uma` materializa antes do carimbo, consulta o juiz do lastro e levanta `_Reprovada` dentro do atomic para provocar rollback. Já o corpo atual de `materializar_pendentes_aprovadas` apenas julga e carimba; ele declara expressamente que não fabrica batida. (`app/chamados/services/materializacao.py:1126`, `:1128`, `:1139`, `:1145`, `app/chamados/services/materializacao.py:1327`, `:1337`)

Os **leitores operacionais diretos**, incluindo consultas ORM, histórico e snapshots, são:

| Arquivo | Funções e sítios de leitura |
|---|---|
| `app/chamados/juizes.py` | `_retratada_com_trilha`: `app/chamados/juizes.py:275`; `_pergunta_materializada_de_fato`: `:624`, `:650`; `_chave_memo_pv`: `:791`; `_pergunta_viva_calc`: `:1042`, `:1071`, `:1077`, `:1107`; `pergunta_sem_lastro_ou_viva`: `:1125`; `irmas_respondidas_sem_consumo`: `:1633`. |
| `app/chamados/models.py` | `_universo_da_ressuscitacao`: `app/chamados/models.py:1421`; `respostas_sem_veredito`: `:1769`; `estado`: `:2478`, `:2481`; guardas de `fechar_por_admin`, `encerrar_fantasma`, `reabrir_fantasma`, `reabrir_por_resposta`: `:2491`, `:2536`, `:2551`, `:2571`; `q_vivas`: `:2616`, `:2618`. |
| `app/chamados/reconciliador.py` | `lavrar_sem_turno_aberto`: `app/chamados/reconciliador.py:367`; `retratar_perguntas_absolvidas_por_celula`: `:586`; `_orfas_abertas`: `:1004`; trilha de `encerrar_orfas_pela_lampada`: `:1096`, `:1106`; `perguntas_orfas_reatavies`: `:1411`. |
| `app/chamados/services/disputa_emissao.py` | `_derivar_chamado_destino`: `app/chamados/services/disputa_emissao.py:211`; `motivo_fora_da_fabrica`: `:695`; `criar_disputa_a_partir_de_chamados_orfaos`: `:874`, `:894`; `criar_disputa_ancora_de_chamado`: `:1117`; `resolver_perguntas_por_chamado_resolvido`: `:1538`, `:1635`, `:1666`. |
| `app/chamados/services/materializacao.py` | `estado_aplicacao_pergunta`: `app/chamados/services/materializacao.py:419`; `_materializar_ausencia_de_resposta`: `:510`; `perguntas_absorvidas_por`: `:579`, `:581`, `:586`; snapshot de `devolver_absorvidas_da_ausencia_rejeitada`: `:625`; `_carimbar_recusa`: `:1037`, `:1044`; `materializar_perguntas_validadas_da_disputa`: `:1291`; `materializar_pendentes_aprovadas`: `:1338`. |
| `app/chamados/services/validacao.py` | `devolver_validacao_recusada`: `app/chamados/services/validacao.py:180`, `:182`; `validar_pergunta`: `:422`, `:433`, `:451`. |
| `app/chamados/services/raiox.py` | `creditos_perguntas`: `app/chamados/services/raiox.py:133`; `pergunta_sem_acao_possivel`: `:173`. |
| `app/chamados/services/relavra_via.py` | `q_carimbo_desfazivel`: `app/chamados/services/relavra_via.py:179`; snapshot de `desfazer_carimbo_sem_lastro`: `:229`. |
| `app/chamados/services/fio.py` | `retratar_duplicata`: `app/chamados/services/fio.py:893`. |
| `app/chamados/services/fabrica_da_celula.py` | `perguntar`: `app/chamados/services/fabrica_da_celula.py:123`. |
| `app/chamados/services/painel.py` | Agregação de `marcador_do_placar`: `app/chamados/services/painel.py:67`. |
| `app/chamados/services/historico.py` | `_eventos_dos_chamados`, leitura dinâmica pelo nome do campo: `app/chamados/services/historico.py:103`, `:105`. |
| `app/chamados/services/passivo_validado.py` | `_reversao_de`, leitura dinâmica por `CAMPOS_REVERSAO`: `app/chamados/services/passivo_validado.py:64`, `:83`. |
| `app/chamados/veredito_lavrado.py` | Agregação de `marcos_do_lote`: `app/chamados/veredito_lavrado.py:163`, `:164`. |
| `app/chamados/views.py` | `responder_pergunta_disputa`: `app/chamados/views.py:1257`. |
| `app/ponto/services/procedencia_batida.py` | `procedencia`: `app/ponto/services/procedencia_batida.py:74`. |
| `app/chamados/services/regularizacao_ext.py` | `retomar_regularizacoes_confirmadas`, **outro modelo**: `app/chamados/services/regularizacao_ext.py:219`. |

Há também leitura **indireta pela expressão comum** `q_carimbo_desfazivel`: a fábrica a usa, e o vigia seleciona os carimbos e depois pergunta ao C6; `materializada_viva` conta esse resultado. (`app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/vigia_fio.py:760`, `app/chamados/services/vigia_fio.py:766`, `app/chamados/services/vigia_fio.py:800`)

Leitores em comandos:

- `auditar_disputas.handle`: `app/chamados/management/commands/auditar_disputas.py:71`, `:150`.
- `auditar_invariantes_chamados`: `universo_i6` em `app/chamados/management/commands/auditar_invariantes_chamados.py:469`; `_i6_pares` em `:537`; demais consultas/contagens em `:587`, `:904`, `:954`, `:1072`, `:1153`, `:1570`, `:1598`, `:1763`.
- `encerrar_perguntas_com_fato.handle`: `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`.
- `reabrir_orfas_enterradas.handle`: `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`, `:87`, `:90`, `:128`, `:141`.
- `reconciliar_fantasmas.handle`: `app/ponto/management/commands/reconciliar_fantasmas.py:212`.

**3. Nomes do F0 e ponto natural de escrita**

| Nome citado | Existe hoje? |
|---|---|
| `relavra_via.VIAS_DE_IDENTIDADE` | **Sim.** Contém quatro vias: `admin_decidiu`, `duplicata`, `fato_ocupado`, `paridade_recusada`. `competencia_trancada` não integra essa constante. (`app/chamados/services/relavra_via.py:166`) |
| `competencia_trancada` como via | **Sim.** É constante da pergunta; C6 a trata em degrau próprio e também no conjunto de cinco vias que encerram por identidade. (`app/chamados/models.py:2263`, `app/chamados/juizes.py:1042`, `app/chamados/juizes.py:1066`) |
| `PerguntaDisputa.fechar_por_competencia_trancada` | **Sim, ainda existe e escreve.** Continua também na lista do selo Q1; portanto a retirada prevista no F0 não está realizada. (`app/chamados/models.py:2505`, `app/chamados/models.py:2514`, `app/chamados/tests/test_porta_pergunta_identidade.py:65`) |
| `core/portas.py`, Q1 | **Sim.** O comentário histórico ainda enumera cinco atos, incluindo o método anterior. O selo atual enumera nove atos. (`app/core/portas.py:241`, `app/core/portas.py:242`, `app/chamados/tests/test_porta_pergunta_identidade.py:60`) |
| `competencia_trancada` como autoridade de fechamento | **Sim.** Há função com esse nome em fechamento e registro K4 apontando para ela. (`app/ponto/services/fechamento.py:1441`, `app/core/juizes.py:301`) |
| Serviço da competência trancada | **Sim.** O módulo declara a via e chama o encerramento do chamado. (`app/chamados/services/competencia_trancada.py:41`, `app/chamados/services/competencia_trancada.py:177`) |

**Não há um ponto único já existente que receba todas as escritas de `materializada_em`.** Os gates da pergunta e os serviços atribuem e salvam diretamente. (`app/chamados/models.py:2500`, `app/chamados/models.py:2502`, `app/chamados/reconciliador.py:621`, `app/chamados/services/materializacao.py:1151`, `app/chamados/services/materializacao.py:1200`)

Como **inferência de localização pelo desenho atual**, o locus natural seria a própria entidade `PerguntaDisputa`, onde já estão seus gates, por analogia com o núcleo de estado do chamado. Isso identifica um lugar arquitetural; não identifica uma função implementada da F2. (`app/chamados/models.py:195`, `app/chamados/models.py:2485`, `app/chamados/models.py:2505`, `app/chamados/models.py:2530`)

`_materializar_uma` é o ponto compartilhado existente para duas filas específicas. `veredito_lavrado.lavrar` também não é essa porta: ele julga pelo lastro e grava os campos da lavra, sem escrever `materializada_em`. (`app/chamados/services/materializacao.py:1065`, `app/core/portas.py:284`, `app/chamados/veredito_lavrado.py:211`, `app/chamados/veredito_lavrado.py:227`)

**4. Selo de escritores e allowlist**

**Existe o P13**, `ContratoEscritoresEstadoTest`. Ele cobra ausência de arquivo novo e ausência de arquivo sancionado que deixou de escrever. (`app/core/tests/test_contract_escritores_estado.py:250`, `app/core/tests/test_contract_escritores_estado.py:251`, `app/core/tests/test_contract_escritores_estado.py:260`)

A allowlist de `materializada_em` tem estes **sete arquivos**:

| Arquivo permitido | Declaração |
|---|---|
| `chamados/services/regularizacao_ext.py` | `app/core/tests/test_contract_escritores_estado.py:85` |
| `chamados/services/disputa_emissao.py` | `app/core/tests/test_contract_escritores_estado.py:87` |
| `chamados/services/materializacao.py` | `app/core/tests/test_contract_escritores_estado.py:89` |
| `chamados/models.py` | `app/core/tests/test_contract_escritores_estado.py:90` |
| `chamados/reconciliador.py` | `app/core/tests/test_contract_escritores_estado.py:95` |
| `chamados/services/fio.py` | `app/core/tests/test_contract_escritores_estado.py:112` |
| `chamados/services/relavra_via.py` | `app/core/tests/test_contract_escritores_estado.py:117` |

Os limites do selo são concretos:

- Reconhece por regex `.campo = ...` e `update(...campo=...)`; não é varredor semântico de `save(update_fields)`, `bulk_update`, SQL ou kwargs dinâmicos. (`app/core/tests/test_contract_escritores_estado.py:227`)
- Usa `RAIZ='/app'` e seis apps: chamados, ponto, escala, colaboradores, férias e folha. (`app/core/tests/test_contract_escritores_estado.py:12`)
- Exclui diretórios de testes/migrações e arquivos `test_`. (`app/core/tests/test_contract_escritores_estado.py:239`, `app/core/tests/test_contract_escritores_estado.py:242`)
- Trabalha por nome do campo e arquivo; por isso a mes

[truncado em 20000 de 26774 caracteres]

## 01/11 13:43 · sessao 982955704424 · - · - · origem=Ronald (tmux)

Auditoria de leitores concluída, com inventários enviados por mensagem. Pontos centrais:

- `VIAS_DE_IDENTIDADE` existe e contém quatro vias; `competencia_trancada` fica fora dessa constante, mas integra as cinco vias de encerramento por identidade do C6. `app/chamados/services/relavra_via.py:166`; `app/chamados/juizes.py:1066`.
- `PerguntaDisputa.fechar_por_competencia_trancada` continua presente; a busca estática encontrou somente sua definição na produção. Continua nomeado pelo selo de identidade. `app/chamados/models.py:2505`; `app/chamados/tests/test_porta_pergunta_identidade.py:65`.
- O comentário Q1 descreve cinco atos, enquanto a allowlist real do selo tem nove atos. `app/core/portas.py:242`; `app/chamados/tests/test_porta_pergunta_identidade.py:60`.
- Não há núcleo comum existente para todos os carimbos: os gates do modelo gravam diretamente, assim como materializador, reconciliador e serviços. Inferência sobre “ponto natural”: núcleo na entidade `PerguntaDisputa`, por analogia com os gates existentes; isso é inferência, não porta implementada. `app/chamados/models.py:2485`; `app/chamados/models.py:2502`; `app/chamados/services/materializacao.py:1186`; `app/chamados/reconciliador.py:624`.
- `materializada_por` serve à procedência e à trilha; não entra no julgamento C6, que lê `materializada_em` e `via_resolucao`. `app/ponto/services/procedencia_batida.py:72`; `app/chamados/juizes.py:1042`; `app/chamados/juizes.py:1071`; `app/chamados/juizes.py:1107`.
- Dois leitores dinâmicos precisam constar: histórico lê o campo por `getattr`; snapshot do passivo lê `materializada_em`, `materializada_por_id` e `via_resolucao` por lista de campos. `app/chamados/services/historico.py:103`; `app/chamados/services/historico.py:105`; `app/chamados/services/passivo_validado.py:64`; `app/chamados/services/passivo_validado.py:83`.
- O vigia lê a elegibilidade comum por `q_carimbo_desfazivel`, depois C6, dentro de `carimbos_sem_lastro_por_dono`; `materializada_viva` apenas conta. `app/chamados/services/vigia_fio.py:760`; `app/chamados/services/vigia_fio.py:766`; `app/chamados/services/vigia_fio.py:800`.
- `JUIZES.md` é lookup subordinado ao vivo e não declara escritores dos três campos; seus “SEM juiz” para fechamento de disputa e significado de fechado divergem do registro atual C7/K4. `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:14`; `app/docs/JUIZES.md:54`; `app/docs/JUIZES.md:71`; `app/core/juizes.py:267`; `app/core/juizes.py:301`.
- A frase “escritor único de validada_em” do lookup não é um inventário literal atual do campo: o próprio P13 admite quatro arquivos. `app/docs/JUIZES.md:38`; `app/core/tests/test_contract_escritores_estado.py:183`.

## 01/11 13:42 · sessao 180955daa069 · - · - · origem=Ronald (tmux)

Inventário operacional de `materializada_em`:

| Modelo/função | Atribuição | Persistência | Guarda/porta |
|---|---|---|---|
| `PerguntaDisputa.fechar_por_admin` | `app/chamados/models.py:2500` | `:2502`, `save(update_fields=...)` | Q1; idempotência em `:2490`; sem guarda C6 comum |
| `PerguntaDisputa.fechar_por_competencia_trancada` | `app/chamados/models.py:2514` | `:2516` | Q1; guarda de validada/via em `:2511`; sem C6 |
| `PerguntaDisputa.encerrar_fantasma` | `app/chamados/models.py:2539` | `:2541` | Q1; guarda idempotente em `:2536`; sem C6 na função |
| `PerguntaDisputa.reabrir_fantasma` | `app/chamados/models.py:2554`, limpa | `:2556` | Q1, inversa de via |
| `PerguntaDisputa.reabrir_por_resposta` | `app/chamados/models.py:2574`, limpa | Retorna campos em `:2576`; chamador `responder_pergunta_disputa_core` soma campos em `app/chamados/services/disputa_emissao.py:1265` e salva em `:1266` | Modelo é escritor, service persiste |
| `PerguntaDisputa.reabrir_por_competencia` | `app/chamados/models.py:2584`, limpa | `:2586` | Q1, inversa de via |
| `ChamadoColaborador.ressuscitar_perguntas` | `app/chamados/models.py:1488`, limpa | `alvo.update(materializada_em=None,...)` no mesmo sítio | Usa `_universo_da_ressuscitacao` em `:1479`; C6 da cópia em `:1445` |
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:402` | `:407` | Pergunta ao C6 antes do carimbo em `:399`; escrita direta própria |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:621` | `:624` | C6 em `:611`; exclui dia trancado em `:617`; escrita própria |
| `_materializar_ausencia_de_resposta` | `app/chamados/services/materializacao.py:511` | `:530` | Escreve diretamente após criar ausência em `:501`; sem C6 comum |
| `_materializar_resposta_hora` | `app/chamados/services/materializacao.py:860` | `:870` | Identidade `paridade_recusada`; exige lâmpada True em `:843`; sem C6 comum |
| `_carimbar_recusa` | `app/chamados/services/materializacao.py:1045` | Campos adicionados `:1053`, `save` em `:1055` | Só `codigo == 'COLISAO'` e campo nulo (`:1044`); identidade `fato_ocupado` em `:1046` |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1151` | Campos em `:1186`, `save` em `:1200` | Atomic em `:1126`; materializa antes (`:1128-1132`), juiz do lastro em `:1139`, rollback `_Reprovada` em `:1145`; sem C6 comum pós-carimbo |
| `materializar_pendentes_aprovadas` | `app/chamados/services/materializacao.py:1341` | `:1344` | Juiz do lastro `_pergunta_materializada_de_fato` em `:1339`; sem C6 comum |
| `resolver_perguntas_por_chamado_resolvido`, S128 primeiro ramo | `app/chamados/services/disputa_emissao.py:1562` | Campos `:1565`, `save` em `:1583` | C6 antes em `:1547`; exclui trancado `:1551` |
| Mesma função, S128 sem credor | `app/chamados/services/disputa_emissao.py:1644` | Campos `:1648`, `save` em `:1659` | C6 antes em `:1641`; escrita própria |
| `reabrir_para_correcao` | `app/chamados/services/disputa_emissao.py:1360`, limpa em dict se `desfazer_absorcao` | `qs.update(**campos)` em `:1377` | Porta de reabertura; campo dinâmico, importante não omitir |
| `retratar_duplicata` | `app/chamados/services/fio.py:894` | `:896-897` | Identidade; só preenche carimbo nulo (`:893`); sem C6 comum |
| `desfazer_carimbo_sem_lastro` | `app/chamados/services/relavra_via.py:231`, limpa | `:234` | Relê travada `:221-224`, C6 viva `:227`; elegibilidade `q_carimbo_desfazivel` em `:223` |
| **Outro modelo:** `materializar_regularizacao_externa` | `app/chamados/services/regularizacao_ext.py:198` | `:200` | `RegularizacaoExterna`, campo declarado em `app/chamados/models.py:3038`; não é PerguntaDisputa (`:2659`) |

Atribuição **só em memória**, não escritor persistente: `ChamadoColaborador._universo_da_ressuscitacao`, `app/chamados/models.py:1440`; opera em `copy.copy(p)` (`:1427`) e consulta C6 em `:1445`. Atribui `materializada_em=None`, `materializada_por=None`, `validada_em=None`, `respondida_em=None`, `via_resolucao=''` (`:1440-1444`).

Capacidade genérica adicional: `PerguntaDisputa.abrir(**campos)` aceita extras e repassa a `objects.create(..., **campos)` em `app/chamados/models.py:2453-2454`. Não localizei caller operacional fornecendo `materializada_em`; fixtures fornecem. Não trataria essa capacidade como carimbo operacional explícito, mas convém declará-la como limite da análise de kwargs.

Não existe uma função comum que todos esses carimbos chamem: os sítios acima atribuem e persistem diretamente. O compartilhamento existente é `_materializar_uma` entre duas filas, documentado e testado por `app/chamados/tests/test_porta_desfecho_pergunta.py:14-21`, registrado em `app/core/portas.py:284-290`. Não confundir “porta é módulo”, declarada em `app/chamados/tests/test_porta_pergunta_identidade.py:23-25`, com uma função comum de escrita/C6.

Selo P13:

- `app/core/tests/test_contract_escritores_estado.py:83-118` permite sete arquivos: `regularizacao_ext.py` (`:85`), `disputa_emissao.py` (`:87`), `materializacao.py` (`:89`), `chamados/models.py` (`:90`), `reconciliador.py` (`:95`), `fio.py` (`:112`), `relavra_via.py` (`:117`).
- `test_nenhum_escritor_fora_do_censo` (`:251`) barra novos arquivos; `test_divida_nao_e_fantasma` (`:260`) barra allowlist sem escritor.
- `_escreve` (`:227-231`) reconhece regex `.campo =` e `update(...campo=...)`. Não cobre semanticamente SQL cru, `bulk_update`, `save(update_fields=...)` isolado ou kwargs dinâmicos.
- `RAIZ='/app'`, seis `APPS` (`:12-13`): chamados, ponto, escala, colaboradores, ferias, folha; deixa api, relatorios e core fora. Exclui testes/migrações (`:239`) e arquivos `test_` (`:242`). Não executado aqui. [nao verificado]
- Complementos: `app/chamados/tests/test_contract_via_muda.py:11-37` cobra `via_resolucao` no bloco do carimbo; `app/chamados/tests/test_contract_autor_muda.py:26-55` cobra `materializada_por`. Ambos varrem atribuição não-None por regex, pulam `reg` (outro modelo, respectivamente `:28`, `:43`), testes e migrações; não são guarda C6 comum. Anti-vacuidade de autor em `:57-69`.

Escopo global: busca por referência ao campo fora de app encontrou só comentário `bin/tests/test_artefato_de_estado.sh:5`; nenhum escritor operacional fora de app foi localizado. Nenhum SQL cru ou `bulk_update` com esse campo foi localizado na cópia; ausência de execução em produção é [não verificado].

Migrações do campo são definição de schema, sem carimbo de dado encontrado: `app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:23` e `app/chamados/migrations/0021_regularizacaoexterna_regularizacaoitembatida_and_more.py:26`. Fixtures de teste são separadas dos escritores operacionais e excluídas pelo P13 (`app/core/tests/test_contract_escritores_estado.py:239`).

`passivo_validado.CAMPOS_REVERSAO` é **leitor**: enumera `materializada_em` em `app/chamados/services/passivo_validado.py:64`, lido por `getattr` em `_reversao_de`, `:83`; não há setattr/update/save desses campos nesse arquivo.

## 01/11 13:41 · sessao 2ebc7e13e9cf · - · - · origem=Ronald (tmux)

1. **Medir desligamento/RECONCILIAR: documentado, execução não verificável.** `app/docs/RELATO.md:270` registra a medida: 43 vivas em 25 chamados de desligado; 1.281 chamados vivos com viva ancorada, dos quais 106 fechariam, 18 reatariam e 88 recusariam. Não li a sonda/log citado (`logs/o261e/f1_medida_p1.resp`): resultado primário e correspondência com o hash atual **[não verificado]**. A inversão efetiva da ordem está no código: disputas primeiro em `app/colaboradores/services/desligamento.py:139`, chamados depois em `:150`.

2. **REDs: testes presentes; execução RED histórica [não verificado].** Fechamento com viva pela âncora destino: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`; pelo container: `:94`; slot não derivável/pós estado: `:172`, com contraexemplo da célula acusadora em `:179`; costura em terminal: `:252`; dedup de `abrir` reapontando existente: `:275`. A alegação de execução RED está em `app/docs/RELATO.md:270`.

3. **Gate: realizado parcialmente.** Sistema decide uma vez em `_decisao_do_fechamento`, compartilhada por `reconciliaria()` e `reconciliar()`: `app/chamados/models.py:605`, `:621`, `:627`. A guarda nova fica em `_gate_pergunta_viva`, **separada** de `_gate_premissa_do_fechamento`: `:391`, `:678`; a composição chama premissa em `:646` e pergunta viva em `:649`. Reata por pergunta via C8/E4-c em `:355`, `:371`, `:373`, e `fio.reapontar_resolvedora` em `:418`; recusa idempotente em `:378`, `:403`. Humano levanta em `transicionar`: `:159`. `cancelar`, `encerrar` e competência trancada usam guarda em `:747`, `:1532`, `:1552`. **Não ocorreu a absorção da parte `fio_vivo` da S127**: `transicionar` ainda consulta `disputa_viva_do_chamado` antes da nova guarda (`:147`), e esse juiz ainda retorna disputa por `(not perguntas) or fio_vivo(perguntas)` (`app/chamados/juizes.py:1955`, `:1967`).

4. **Tripwire/pós estado: realizado.** `_gravar_estado` detecta travessia vivo→TERMINAIS inclusive por instância velha, chama `_vivas_entre` e levanta antes da escrita em `app/chamados/models.py:213`, `:215`, `:218`, `:220`, `:222`. `_vivas_entre` coloca `status_local=novo`, aponta `p.chamado_destino=self`, consulta C6 e restaura no `finally`: `:332`, `:339`, `:348`, `:349`, `:351`. O gate de sistema reúne decisão/reatamento/save no atomic em `:614`. A relavra S2 é posterior ao save no mesmo atomic: `:274`, `:279`, `:288`; há testes de ordem e falha em `app/chamados/tests/test_o261e_f1_codex_v5.py:77`, `:93`.

5. **Costura: realizada para perguntas existentes; nascimento declarado fora.** A guarda é `_costura_nao_mata_o_fio` (`app/chamados/services/fio.py:742`), consultada pelo reaponte (`:840`) e pelo dedup de `PerguntaDisputa.abrir` (`app/chamados/models.py:2424`). O E4-c `resolvedora_valida` ficou separado, conforme `fio.py:749`. A docstring declara nascimento com destino/pai terminal para F2 em `fio.py:750`; o nascimento continua em `models.py:2453`, sem a guarda usada no ramo de existente em `:2425`.

6. **Selo AST/mutantes: realizado em fonte; execução [não verificado].** Composição dos selos anteriores explicitada em `app/chamados/tests/test_o261e_f1_selo_encerramento.py:5`; tripwire AST em `:24`, teste em `:55`, mutantes em `:64`, `:68`, `:73`, `:78`, `:83`, `:87`. Selo de status cobre atribuição, verbos, defaults, splats, update_fields, bulk_update, setattr e SQL em `app/chamados/tests/test_selo_status_so_pelas_portas.py:10`; varredura em `:340`; mutante de defaults em `:421`. Exceção declarada da troca em memória em `:40` e `app/chamados/tests/test_contract_status_nucleo.py:9`.

7. **Remoção de `superar`: realizada como método.** Lápide em `app/chamados/models.py:755` documenta retirada por zero chamador e L-111; `:759` preserva estado `superado` e indica `cancelar()` para duplicata nova. O catálogo ainda conservar evento SUPERAR é questão distinta: `app/docs/RELATO.md:204` o registra em fila.

8. **DIFF de TXT/aberta/exportada: documentado; prova primária no hash [não verificado].** `app/docs/RELATO.md:270` registra base116×cura29, mudança nominal col750/col911 só na base e 26 exportadas com hashes iguais; `:252` registra rodada posterior com ENTRA347→347 e hash26 igual; `:253` aponta comparador/logs. Não há execução dessas sondas lida nesta auditoria. A própria linha da obra ainda descreve F1 como em construtor no `app/docs/BACKLOG.md:820`.

## 01/11 13:03 · sessao 8e5e644a35b0 · COBERTURA-PARCIAL-UM-JUIZ · censo · origem=Code

**Classificação: 1 → (c), com alerta estático já feito; 2 → (a); 3 → (a), com existência histórica dos sítios parcialmente não verificada; 4 → (c); 5 → (c); 6 → (e), ressalvada L-092.** As distinções e evidências estão abaixo. (`triagem.md:5730`, `:5741`; r3.r → `app/ponto/services/cartorio.py:208`, `app/ponto/turnos.py:2632`; r1.q:10; r3.q:28)

Numerei as linhas das perguntas a partir da primeira linha de cada anexo. As referências apresentadas como “r1.r → arquivo:linha” são as referências históricas da resposta, não posições da cópia final.

**1. Bump isolado, alcance financeiro e emissão em lote — (c) para a medição final; o risco estático já havia sido visto.**

Pergunta literal:

> “pouso antes do pre-requisito (REGRA_VERSOES (3,'2026-09') rejulga a competencia 09, que esta EXPORTADA).” (`r3.q:31`) [nao verificado]

Resposta literal:

> “O horizonte alcança **toda competência ≥09**, não apenas cinco dias, e invalida impressões inclusive sem mudança de marcos dispensados. Não seleciona uma implementação histórica isolada.”  
> “**Bloqueia certificar o pouso:** os hashes, o impacto financeiro, a eventual Pauta DP e a reconstrução exigida das projeções não exportadas não estão demonstrados no material fornecido; cumprimento **[nao verificado]**.” (r3.r → `app/ponto/services/cartorio.py:208`, `:380`, `:1192`, `:169`; `app/docs/LEIS.md:142`)

Portanto, **não cabe dizer que o Codex deixou passar o alcance geral do bump**. A falha específica da comparação posterior com `forcar=True`, os 16 colaboradores e os deltas produtivos pertencem à medição posterior às três leituras. A triagem exigiu a comparação e depois registrou a hipótese ainda não provada, mantendo a obra parada. (`triagem.md:11`, `:5730`, `:5741`, `:5744`)

Existência nas árvores lidas:

| Volta | Evidência histórica |
|---|---|
| r1 | Impressão e desvio por força já existiam: “`forcar=True` ignora o salto por impressão igual”. A versão 3 ainda nessa árvore: **[nao verificado]**. (r1.r → `app/ponto/services/cartorio.py:1177`) |
| r2 | Impressão e pulo já existiam; a resposta observou que “a declaração de versões atual não contém uma versão adicional para esta obra”. (r2.r → `app/ponto/services/cartorio.py:1189`, `:111`) |
| r3 | Versão, horizonte geral e invalidação já estavam presentes e foram apontados expressamente. (r3.r → `app/ponto/services/cartorio.py:111`, `:208`, `:380`, `:1192`) |

A cópia final permite confirmar estaticamente que a versão alimenta a impressão e que `forcar` atravessa os dois testes de igualdade; isso não reproduz a medição. (`app/ponto/services/cartorio.py:1003`, `:1055`, `:1060`, `:1208`, `:1218`)

O próprio achado final limita o contraexemplo sem força à árvore anterior: “Não verifiquei na 113fd774: o v4 rodou na 129bfeaa, antes do c2”. Logo, repetir os mesmos deltas na árvore final permanece **[nao verificado]**. (anexo `opus_final_achados.md`, item 1; `triagem.md:5741`, `:5758`)

**2. Materialização cala parcial sem período; `_celula_furada` não — (a).**

Pergunta literal:

> “compare o predicado novo com os outros sitios que decidem ‘ausencia cobre o dia’”  
> “Algum responde diferente para o mesmo (ausencia, dia)?” (`r3.q:21`, `:23`)

Resposta literal:

> “O argumento vazio **não é o defeito**: com os dois horários preenchidos, `periodo_da_parcial_no_dia(a, ())` retorna `fora`; faltando um horário, retorna `sem_periodo`.” (r3.r → `app/ponto/turnos.py:2632`, `:2638`, `:2641`)

A resposta identificou o retorno `sem_periodo`, mas apontou como divergência o desempate entre duas ausências; não apontou a cláusula adicional que cala a parcial sem período. Essa diferença aparece na cópia entre o `or (... == 'sem_periodo')` da materialização e a consulta exclusiva à integralidade em `_celula_furada`. (`app/chamados/services/materializacao.py:81`, `app/chamados/juizes.py:447`; r3.r → `app/chamados/services/materializacao.py:74`)

Existência nas árvores lidas:

| Volta | Evidência histórica |
|---|---|
| r1 | Ambos os leitores existiam; a materialização ainda foi descrita como cobertura ampla por qualquer ausência não-falta. A cláusula específica `sem_periodo` nessa volta: **[nao verificado]**. (r1.r → `app/chamados/services/materializacao.py:65`, `app/chamados/juizes.py:433`) |
| r2 | A cobertura ampla foi novamente denunciada. A cláusula específica `sem_periodo` nessa volta: **[nao verificado]**. (r2.r → `app/chamados/services/materializacao.py:55`) |
| r3 | A lógica de `sem_periodo` já integrava o predicado examinado, e os dois leitores foram citados. A seleção por `any()` ainda seria corrigida depois. (r3.r → `app/ponto/turnos.py:2632`, `app/chamados/services/materializacao.py:74`, `app/chamados/juizes.py:446`; cópia: `app/chamados/services/materializacao.py:66`, `:73`) |

A r2 escreveu literalmente “**Procedem de fato três descartes:**” e incluiu `_coberto_por_ausencia`. Isso comprova que houve contestação de descarte sobre o **antecedente mais amplo**, mas não fornece a linha individualizada da triagem nem identifica o achado final de `sem_periodo` como descartado. Classificação **(b)** para este achado: **[nao verificado]**. (r2.r → `app/chamados/services/materializacao.py:55`; `triagem.md:5730`)

**3. `furos_diarios` e `calculadores` ainda pulam parcial por `tipo_dia` — (a) quanto ao tema perguntado.**

Pergunta literal:

> “Ha algum leitor [...] que segue derivando o dia parcial por regra propria (ex. por Ausencia.minutos, por tipo_dia, por cobre_parte_do_dia) e agora contradiz a grade?” (`r1.q:10`)

Trecho literal relacionado da resposta:

> “Conta não-missing, mas filtra dia `trabalho` em `:141`; parcial rotulada ausência não entra.” (r2.r → `app/inteligencia/calculadores.py:153`, `:141`)

A r2 percebeu um filtro por tipo em `calculadores`, mas o tratou como proteção daquela **contagem de batidas**; não formulou o achado final sobre os leitores que excluem o dia parcial do universo de furos. Os dois filtros finais estão presentes na cópia. (`app/escala/services/furos_diarios.py:39`, `app/inteligencia/calculadores.py:46`; r2.r → `app/inteligencia/calculadores.py:141`, `:153`)

Existência nas árvores lidas:

| Volta | Evidência histórica |
|---|---|
| r1 | Os dois sítios exatos do achado final: **[nao verificado]**. |
| r2 | O arquivo `calculadores.py` e outro filtro por `trabalho` estão comprovados pela resposta; o bloco atualmente em `:46` e o filtro de `furos_diarios.py:39` naquela árvore: **[nao verificado]**. (r2.r → `app/inteligencia/calculadores.py:141`, `:153`) |
| r3 | A resposta encontrou exclusão semelhante em relatórios, mas a existência histórica dos dois sítios exatos finais continua **[nao verificado]**. (r3.r → `app/relatorios/furos.py:151`, `:218`, `:237`) |

Assim, o tema estava explicitamente perguntado; **atribuir a omissão dos dois blocos exatos a cada uma das três árvores exige evidência histórica que o material não fornece**. (`r1.q:10`; existência desses blocos nas três árvores: **[nao verificado]**)

**4. Falta aprovada deixa de suprimir c2 e permite costura — (c), pela mudança posterior à r3.**

Pergunta literal relacionada:

> “Algum responde diferente para o mesmo (ausencia, dia)?”  
> “(c) parcial do tipo 'falta'” (`r3.q:23`, `:26`)

Resposta literal:

> “Parcial com período e marco externo apagado: qualquer ausência vencedora veta criação, reabertura e até costura no chamado vivo.” (r3.r → `app/chamados/reconciliador.py:811`, `:825`)

A r3 viu o **veto antigo excessivo**. A v4 trocou esse veto pela composição `_coberto_por_ausencia`; nessa composição, falta retorna falso. O achado final examina o comportamento resultante dessa troca, especialmente no chamado já vivo. (`app/chamados/reconciliador.py:812`, `:825`, `:828`; `app/chamados/services/materializacao.py:81`; `app/ponto/turnos.py:2674`)

Existência nas árvores lidas:

| Volta | Evidência histórica |
|---|---|
| r1 | Sítio c2 específico: **[nao verificado]**. |
| r2 | c2 já existia e “também para ao encontrar ausência”. (r2.r → `app/chamados/reconciliador.py:825`) |
| r3 | Veto e costura já existiam, mas com o comportamento antigo expressamente denunciado. (r3.r → `app/chamados/reconciliador.py:811`, `:825`) |

A triagem mandou curar os procedentes da r3 e registrou o RED posterior “dos 4 procedentes Codex r3 + c2”; isso não demonstra cobertura do mutante específico de falta aprovada citado pelo Opus. (`triagem.md:5730`, `:5744`; teste desse mutante: **[nao verificado]**) [nao verificado]

**5. Fuso de `deslocamentos_na_cadeia` e descarte de marco sem instante — (c) para os mecanismos finais.**

Pergunta literal:

> “os testes novos em ponto/tests/test_cobparc_marco_no_periodo.py mordem a costura? algum passa com a cura revertida?” (`r3.q:29`)

Resposta literal:

> “A lacuna bloqueante é concreta: teste noturno pula a construção da régua; teste de âncora é diurno e verifica somente `.time()`, não a data.” (r3.r → `app/ponto/tests/test_cobparc_marco_no_periodo.py:43`, `:423`)

O Codex viu lacunas dos testes da **implementação anterior**. A composição `deslocamentos_na_cadeia` e a consulta ao mapa de instantes que agora descarta marcos foram apresentadas como cura da volta 4, posterior à leitura final do Codex. (`app/escala/utils.py:356`, `:366`, `:372`; `app/ponto/services/cartorio.py:1721`, `:1733`; `triagem.md:5730`)

Existência nas árvores lidas:

| Volta | Evidência histórica |
|---|---|
| r1 | Existia o antecessor `deslocamentos_dos_marcos`, exercitado indiretamente pelos testes noturnos. (r1.r → `app/ponto/turnos.py:2661`; `app/ponto/tests/test_cobparc_marco_no_periodo.py:64`, `:71`) |
| r2 | O antecessor continuava sendo usado e foi denunciado pela ordem noturna. A cadeia de nomeação também foi citada. (r2.r → `app/ponto/turnos.py:2661`, `app/ponto/services/cartorio.py:1674`) |
| r3 | `_marcos_do_furo_parcial` já existia, mas ordenava horas e combinava com D; a nova filtragem por mapa de instantes pertence à cura posterior. (r3.r → `app/ponto/services/cartorio.py:1717`, `:1719`; cópia: `app/ponto/services/cartorio.py:1721`, `:1733`) |

O caso antigo de “marco fora da régua no ramo DNA” foi perguntado e apontado; ele não comprova que o Codex tenha lido o **novo descarte por ausência de instante**. (`r1.q:13`; r1.r → `app/ponto/services/cartorio.py:491`, `:513`; cópia: `app/ponto/services/cartorio.py:1733`)

**6. Auditoria das leis que protegem os sítios — (e) para conferência artigo por artigo; L-092 retorna ao item 1.**

Pergunta literal:

> “Checklist das classes que ja reprovaram obra: 2o juiz [...] 2o escritor [...] relogio [...] derivacao paralela [...] selo/RED [...] ordem de trava; pouso antes do pre-requisito [...]” (`r3.q:28`) [nao verificado]

Resposta literal:

> “A 09 exportada, por si só, não impede rejulgar ata: o aval escrito autoriza isso e exige fechamento intacto com hashes antes/depois e destino da diferença financeira.” (r3.r → `app/ponto/services/cartorio.py:168`, `app/ponto/services/fechamento.py:36`, `:544`)

A r3 recebeu as linhas de leis como anexo, mas o pedido não enumerava a conferência artigo por artigo dessas oito leis. Pediu classes estruturais e explicitamente o problema da competência exportada; este último foi respondido com bloqueio de certificação, conforme item 1. (`r3.q:2`, `:28`, `:31`; r3.r → `app/docs/LEIS.md:142`)

Os cinco arquivos envolvidos já aparecem nas três respostas: cartório, leitor da célula, supra, utils e turnos. Isso comprova os **arquivos/sítios antecedentes**, não a identidade de todos os trechos finais nem sua conformidade integral com cada artigo. (r1.r → `app/ponto/services/cartorio.py:430`, `app/escala/services/leitor_celula.py:120`, `app/ponto/supra_juiz.py:311`, `app/escala/utils.py:1191`, `app/ponto/turnos.py:2661`; r2.r → respectivos `:1204`, `:83`, `:313`, `:1207`, `:2661`; r3.r → respectivos `:1217`, `:91`, `:530`, `:1206`, `:2652`)

O item 6 do Opus tampouco demonstra violações adicionais: ele próprio diz “Nas outras não verifiquei artigo por artigo”. Classificá-las como falhas que “nenhum viu” seria afirmar infrações ainda **[nao verificado]**.

**Não há classe (b) demonstrável para os seis achados exatos com as linhas de triagem fornecidas.** Há relato de descartes na r2, mas falta a linha individualizada exigida para atribuir essa classe; para os procedentes da r3, a triagem determina cura. (r2.r → `app/chamados/services/materializacao.py:55`; `triagem.md:11`, `:5730`)

Uma diferença documental impede transportar a referência final do Opus diretamente para esta cópia: aqui a obra está em `app/docs/BACKLOG.md:755`, ainda com “PAROU”; `:756` é outra obra. A frase posterior sobre “artefato da ata velha” nessa cópia está **[nao verificado]**. (`app/docs/BACKLOG.md:755`, `:756`)

## 01/11 12:47 · sessao e228828c2fac · GRADE-FOLGA-JUIZ · censo · origem=Code

**B1 → (c); B2 → (c); B3 → (e)**, considerando o achado específico de cada item. As justificativas estão nos trechos abaixo. (`v1.r:3,108,177`; `v1.q:22-24`)

As referências aos anexos contam linhas desde a primeira linha de cada conteúdo, incluindo linhas vazias.

1. **B1 — (c): dependia do RED executado.**

   **Pergunta literal:** “(e) selo ou teste RED que nao morde (passaria tambem sem a cura) -- avalie cada teste novo do diff;” (`v1.q:19`) [nao verificado]

   **Resposta literal:** “A avaliação dos testes abaixo é **estática**; execução e RED observado: **[não verificado]**.” (`v1.r:108`) [nao verificado]

   O Codex avaliou a mordida pelo código, inclusive classificando o teste de calendário `:96` como “**Morde:** calendário do ativo indevido no dia da geradora velha”. Isso não comprova um FAIL executado desse teste nem verifica se o log citado como RED era verde — objetos específicos de B1. (`v1.r:114`; `opus_final_achados.md:2`) [nao verificado]

   **Relação com a triagem:** ela descartou “3 MORDE de toda competencia passam no main” e afirmou que “o RED desta cura contra o main esta em calendario :54/:96”. Portanto, houve descarte de uma crítica vizinha, mas não registro e descarte da divergência de logs apontada em B1. (`v1_triagem.md:4`; `opus_final_achados.md:2`)

2. **B2 — (c): dependia das medições da sombra/produção.**

   **Pergunta literal:** “(f) ordem de trava / pouso antes do pre-requisito;” A pergunta também cobrava: “`status` do dia: em que casos ele difere do main, e por qual linha do diff?” (`v1.q:20`; `v1.q:13`) [nao verificado]

   **Resposta literal:** “A identidade Git da cópia, execução de RED/GREEN, medidas de produção, reversão e ensaio desta árvore permanecem **[não verificado]**.” (`v1.r:3`)

   A divergência entre a declaração documental de zero e as contagens **32/40/8/4** é o achado baseado no ensaio fornecido. A cópia contém a declaração “main x cura = 0 `tipo_dia` diferente e 0 FechamentoMensal da 09 diferente”. (`opus_final_achados.md:3`; `app/ponto/services/cartorio.py:182-183`)

   **Relação com a triagem:** “NAO E ACHADO 2 (status pode mudar falta<->folga): e o efeito pedido [...] vai medido no ensaio DINHEIRO na sombra antes do opus.” Ela descartou a mudança de **status como defeito**, deixando a medição para depois; não descartou as contradições numéricas de B2. As transições fornecidas também não demonstram, por si, alteração de FechamentoMensal. (`v1_triagem.md:6`; `opus_final_achados.md:3`)

3. **B3 — (e): a existência da fila no BACKLOG não foi perguntada.**

   **Pergunta literal:** “Liste todos com arquivo:linha e diga se estao declarados na ALLOWLIST da catraca ponto/tests/test_catraca_folga_lancada_um_juiz.py.” (`v1.q:23-24`)

   **Resposta literal**, na linha do gerador: “**Sim**, pendência GRADE-FOLGA-GERADOR: `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`.” (`v1.r:177`)

   O Codex viu o gerador, a ALLOWLIST e o nome da pendência. O achado específico — a fila citada não existir no BACKLOG — exige uma conferência diferente da presença na ALLOWLIST solicitada pela pergunta. A referência permanece na cópia em JUIZES e na catraca. (`v1.q:22-24`; `v1.r:177`; `opus_final_achados.md:4`; `app/docs/JUIZES.md:89`; `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160-161`)

   **Relação com a triagem:** o descarte referente a fila/ALLOWLIST foi o de **C5 / GRADE-FOLGA-C5**, não o de GRADE-FOLGA-GERADOR. (`v1_triagem.md:3`)

A **volta 2** estava limitada ao delta de remoção do parâmetro: “pre-verificacao, volta 2, so o delta”. Sua conclusão — “não muda o retorno dos chamadores encontrados” — refere-se a esse delta, sem verificar RED, impacto de produção ou existência da fila de B3. (`v2.q:1-2,7`; `v2.r:1`)

## 01/11 12:46 · sessao d59d370773e9 · - · - · origem=Ronald (tmux)

Recomendação: **B1 (c), B2 (c), B3 (e)**, classificando os achados específicos, não temas vizinhos.

- **B1 — (c):** a pergunta pede “selo ou teste RED que nao morde […] avalie cada teste novo do diff” (`v1.q:19`). A resposta limita sua conclusão: “A avaliação dos testes abaixo é **estática**; execução e RED observado: **[não verificado]**.” (`v1.r:108`). A triagem rejeitou a crítica aos três testes de toda competência, não a divergência entre o log citado e o RED executado (`v1_triagem.md:4`; `opus_final_achados.md:2`). A análise estática inclusive considerou que o segundo teste de calendário mordia (`v1.r:114`). [nao verificado]

- **B2 — (c):** a resposta declara “execução de RED/GREEN, medidas de produção, reversão e ensaio desta árvore permanecem **[não verificado]**.” (`v1.r:3`). A pergunta sobre diferenças de `status` era estática (`v1.q:12-13`); a triagem disse literalmente “vai medido no ensaio DINHEIRO na sombra antes do opus” (`v1_triagem.md:6`). Isso não descarta previamente as contagens posteriores nem verifica a declaração documental de zero (`opus_final_achados.md:3`).

- **B3 — (e), com granularidade explícita:** a pergunta foi “Liste todos […] e diga se estao declarados na ALLOWLIST” (`v1.q:22-24`), não se cada fila citada existia no BACKLOG. A resposta viu o gerador e afirmou “**Sim**, pendência GRADE-FOLGA-GERADOR” (`v1.r:177`). Portanto o tema gerador/ALLOWLIST foi visto, mas a existência da fila não foi perguntada; não cabe dizer que ninguém viu o tema nem que a triagem descartou esse achado (`v1.r:177`; `v1_triagem.md:1-8`). **(a)** só seria defensável mediante interpretação ampliada de “tema”, que convém declarar como inferência.

Dupla contagem manual do anexo `v1.r`: **198 linhas físicas**, incluindo vazias; âncoras: ressalvas `:3`, segundo juiz `:98`, pré-requisitos `:103`, avaliação estática `:108`, calendário `:96` na tabela em `:114`, gerador em `:177`, parágrafo final em `:198`.

## 01/11 12:36 · sessao c9220c28be1f · CONTENCAO-QUE-MORDE-V2 · verificador · origem=Code

**O diff corrige partes do problema, mas o selo ainda dá uma prova falsa para B1 e não testa a ordenação que chama de “seq lido depois”. Além disso, existe outro escritor que remove reservas sem atualizar `poda_seq`.** (`bin/tests/test_disjuntor.sh:2463`, `bin/tests/test_disjuntor.sh:2476`, `bin/disjuntor.py:1667`.)

A ocorrência dessas corridas em produção é **[nao verificado]**. Não executei os selos completos: eles criam arquivos, cópias e commits, contrariando a restrição de somente leitura; a análise abaixo usa o código e os anexos fornecidos. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:2104`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:17`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:86`.)

**1. Os problemas e os contraexemplos concretos**

1. **O mutante `v4-b1-mede-sob-trava` morre antes de medir.** Ele insere `med = medir_b1(...)` dentro de `_fn`, imediatamente antes da atribuição de `b1[agent_id]`. Isso torna `med` local ao callback inteiro, mas a nova guarda já acessa `med['disparo']` antes dessa atribuição: ocorre `UnboundLocalError`. O evento da medição nunca é marcado; `_lento` reprova por ausência desse evento, e `_mutante2` aceita qualquer retorno diferente de zero. Portanto, esse vermelho **não prova contenção da trava**. (`bin/tests/test_disjuntor.sh:2476`, `bin/disjuntor.py:583`, `bin/tests/test_disjuntor.sh:1870`, `bin/tests/test_disjuntor.sh:1886`, `bin/tests/test_disjuntor.sh:2130`.)

   O mutante representativo precisaria executar a medição dentro de `_fn` **antes de qualquer uso de `med`**; só então o teste poderia demonstrar que a concorrente estoura a trava. (`bin/disjuntor.py:578`, `bin/disjuntor.py:583`.)

2. **`v6-seq-lido-depois` não implementa o defeito que nomeia.** Ele substitui a leitura por `seq = 10 ** 9`; não move a leitura para depois dos bytes. Os casos terminam de produzir `foto_a` antes de executar a poda concorrente, portanto uma implementação que realmente lesse a sequência depois dos bytes continuaria passando esses casos. (`bin/tests/test_disjuntor.sh:2463`, `bin/tests/test_disjuntor.sh:1934`, `bin/tests/test_disjuntor.sh:1937`, `bin/tests/test_disjuntor.sh:1988`, `bin/tests/test_disjuntor.sh:1996`.)

   Contraexemplo necessário: A lê os bytes com cinco vivos; B entrega o sexto; C poda sua reserva e grava `poda_seq=1`; A só então lê a sequência e monta `(1, cinco_vivos_antigos, resultados_antigos)`. A comparação aceita a igualdade, conta cinco e permite o sétimo. É precisamente a intercalação **durante `_foto_sessao`** que os casos não fabricam. (`bin/disjuntor.py:1641`, `bin/disjuntor.py:1644`, `bin/disjuntor.py:1600`, `bin/disjuntor.py:1607`, `bin/disjuntor.py:1617`.)

3. **A correção de `poda_seq` não cobre todos os escritores de `reservas`.** A porta incrementa a sequência quando elimina reservas por resultado, mas `poda_reservas`, chamado pelo SessionStart, também elimina reservas e não incrementa a sequência. (`bin/disjuntor.py:1602`, `bin/disjuntor.py:1606`, `bin/disjuntor.py:1661`, `bin/disjuntor.py:1667`, `bin/marco_de_clear.py:31`, `bin/marco_de_clear.py:81`.)

   Contraexemplo: S1 tem cinco vivos; B reserva o sexto; A fotografa cinco vivos com sequência zero; chega o resultado de B; SessionStart S2 remove a reserva de B, mantendo sequência zero; A, ainda em voo por S1, aceita sua fotografia antiga e reserva mais um. A conta observada é cinco, embora existam seis vivos no transcript e a nova reserva. O código permite essa sequência; a sobrevivência efetiva dessa chamada após `/clear` em produção é **[nao verificado]**. (`bin/disjuntor.py:1617`, `bin/disjuntor.py:1661`, `bin/disjuntor.py:1667`, `bin/disjuntor.py:1600`, `bin/disjuntor.py:1607`.)

4. **O handshake ainda tem prazo e não garante o “sempre” do comentário.** As leituras fabricadas terminam após 15 segundos mesmo sem `tentou`. Se a thread principal ficar sem execução por mais que isso antes de tentar a trava, uma implementação que faça a leitura sob trava pode terminá-la primeiro e passar. Isso é uma limitação distinta do mutante B1 inválido. (`bin/tests/test_disjuntor.sh:1855`, `bin/tests/test_disjuntor.sh:1868`, `bin/tests/test_disjuntor.sh:1872`, `bin/tests/test_disjuntor.sh:1888`.)

5. **B1 ainda anuncia disparo sem exigir persistência — problema fora da alteração, mas relevante ao registro alterado.** `dispara` executa antes da tentativa de gravação, e `b1_do_agente` não consulta `gravou`. Se `os.replace` falhar, aquela chamada pode retornar disparada e negar a ferramenta, enquanto o Stop lê o estado anterior e não encontra o agente disparado. A nova proteção de `reabre` não foi aplicada a B1. (`bin/disjuntor.py:590`, `bin/disjuntor.py:593`, `bin/disjuntor.py:317`, `bin/disjuntor.py:324`, `bin/disjuntor.py:597`, `bin/hook_stop_fila1.py:1055`.)

**2. Quem escreve e quem lê os campos**

A persistência comum é `_com_estado`: lê sob `flock`, executa o callback, serializa e substitui o arquivo. Falha de escrita conserva o retorno da decisão; `gravou`, quando solicitado, recebe o sucesso ou fracasso. (`bin/disjuntor.py:296`, `bin/disjuntor.py:303`, `bin/disjuntor.py:311`, `bin/disjuntor.py:316`, `bin/disjuntor.py:317`, `bin/disjuntor.py:325`, `bin/disjuntor.py:329`.)

| Campo/valor | Quem escreve | Quem lê e para quê |
|---|---|---|
| `poda_seq` | A porta incrementa quando poda reservas por resultado. `bin/disjuntor.py:1606`. | `_foto_sessao` captura antes dos bytes; a porta compara sob trava; o incremento lê o valor anterior. `bin/disjuntor.py:1641`, `bin/disjuntor.py:1600`, `bin/disjuntor.py:1606`. |
| `podado_ate` | O código atual apenas remove a marca antiga. `bin/disjuntor.py:1599`. | O consumidor substituto usa `poda_seq`; não há leitura de `podado_ate` nesse caminho. `bin/disjuntor.py:1599`, `bin/disjuntor.py:1600`. |
| `foto=(seq_foto, vivos_tx, feitos)` | `_foto_sessao` monta os três componentes a partir da sequência e de uma leitura dos bytes. `bin/disjuntor.py:1641`, `bin/disjuntor.py:1644`, `bin/disjuntor.py:1648`. | A porta desempacota, valida sequência, poda por `feitos` e soma vivos com reservas. `bin/disjuntor.py:1592`, `bin/disjuntor.py:1600`, `bin/disjuntor.py:1602`, `bin/disjuntor.py:1607`. |
| `vivos_tx` | `_sem_entrega` extrai lançamentos e exclui IDs com notificação de fim. `bin/disjuntor.py:425`, `bin/disjuntor.py:426`, `bin/disjuntor.py:429`. | Contagem da porta; também há o leitor independente `agentes_sem_entrega`, usado pelo Stop. `bin/disjuntor.py:1607`, `bin/disjuntor.py:420`, `bin/hook_stop_fila1.py:1053`. |
| `feitos` / `tool_use_id` dos resultados | `_resultados` extrai IDs dos objetos `tool_result`; os testes fabricam esses objetos. Escritor real do harness: **[nao verificado]**. `bin/disjuntor.py:1684`, `bin/disjuntor.py:1687`, `bin/tests/test_disjuntor.sh:1928`. | A porta elimina reservas cujas chaves aparecem nesse conjunto. `bin/disjuntor.py:1602`. |
| `reservas[tuid]` | A porta cria; a própria porta remove por resultado; SessionStart remove por dono. `bin/disjuntor.py:1617`, `bin/disjuntor.py:1604`, `bin/disjuntor.py:1667`. | Idempotência, poda por resultado e contagem global. `bin/disjuntor.py:1578`, `bin/disjuntor.py:1602`, `bin/disjuntor.py:1607`. |
| Reserva: `t`, `obra`, `sessao`, `agente` | Todos são gravados na autorização. `bin/disjuntor.py:1617`. | A poda SessionStart lê `sessao`; a contagem usa a quantidade de reservas, sem ler `t`, `obra` ou `agente`. O teste verifica a presença de `agente`. `bin/disjuntor.py:1661`, `bin/disjuntor.py:1607`, `bin/tests/test_disjuntor.sh:1814`. |
| `autorizados` e limite 500 | A porta acrescenta o ID e conserva os últimos 500. `bin/disjuntor.py:1618`, `bin/disjuntor.py:1620`. | Atalho de idempotência, inclusive após desaparecer a reserva. `bin/disjuntor.py:1578`. |
| `_FOTO_VELHA`, `FOTO_RELEITURAS=3` | Constantes; o callback devolve a sentinela. `bin/disjuntor.py:119`, `bin/disjuntor.py:120`, `bin/disjuntor.py:1601`. | O laço compara por identidade, permite quatro fotografias e depois nega com “não sei”. `bin/disjuntor.py:1623`, `bin/disjuntor.py:1627`, `bin/disjuntor.py:1630`. |
| `gravou` | `_com_estado` acrescenta `ok`. `bin/disjuntor.py:325`. | Porta exige gravação para autorizar; reserva Codex também; `reabre` agora exige gravação antes de anunciar sucesso. `bin/disjuntor.py:1632`, `bin/disjuntor.py:1506`, `bin/disjuntor.py:1701`. |
| `med['disparo']`, `med['motivo']` | `medir_b1`, com saídas positivas, negativas e “não sei”. `bin/disjuntor.py:481`, `bin/disjuntor.py:489`, `bin/disjuntor.py:529`, `bin/disjuntor.py:533`. | Guarda nova, registro, anúncio e retorno de B1. `bin/disjuntor.py:583`, `bin/disjuntor.py:587`, `bin/disjuntor.py:590`, `bin/disjuntor.py:592`. |
| `b1[agent].checado` | B1 grava `max(agora, checado_anterior)`. O teste injeta `agora+5`. `bin/disjuntor.py:587`, `bin/tests/test_disjuntor.sh:2036`. | Cache de 60 segundos e precedência da medida sem disparo. `bin/disjuntor.py:70`, `bin/disjuntor.py:574`, `bin/disjuntor.py:583`. |
| `b1[agent].disparado` | Mesmo registro de B1. `bin/disjuntor.py:587`. | Atalho externo, preservação sob trava e lista de disparados para o Stop. `bin/disjuntor.py:572`, `bin/disjuntor.py:581`, `bin/disjuntor.py:597`, `bin/hook_stop_fila1.py:1055`. |
| `b1[agent].motivo` | Mesmo registro de B1. `bin/disjuntor.py:588`. | Retornos dos atalhos/cache/precedência; o PreToolUse incorpora o motivo à negativa. O Stop lê somente os IDs disparados. `bin/disjuntor.py:573`, `bin/disjuntor.py:575`, `bin/disjuntor.py:582`, `bin/disjuntor.py:584`, `bin/disjuntor.py:2152`, `bin/disjuntor.py:597`. |
| `b3[obra].voltas` | `_voltas` instala uma cópia da semente quando falta o objeto. `bin/disjuntor.py:1464`, `bin/disjuntor.py:1465`. | Reserva Codex, porta Agent, `reabre` e painel. `bin/disjuntor.py:1493`, `bin/disjuntor.py:1612`, `bin/disjuntor.py:1696`, `bin/disjuntor.py:1756`. |
| `voltas.agent` | Semente conta transcripts; complementação ajusta a semente sem transcript; porta incrementa. `bin/disjuntor.py:1455`, `bin/disjuntor.py:1469`, `bin/disjuntor.py:1615`. | Teto Agent, mensagem e painel. `bin/disjuntor.py:1613`, `bin/disjuntor.py:1478`, `bin/disjuntor.py:1757`. |
| `voltas.codex` | Semente conta sidecars da obra; reserva Codex incrementa. `bin/disjuntor.py:1440`, `bin/disjuntor.py:1441`, `bin/disjuntor.py:1498`. | Teto Codex, mensagem e painel. `bin/disjuntor.py:1496`, `bin/disjuntor.py:1478`, `bin/disjuntor.py:1757`. |
| `voltas.extra`, `n`, retorno `ex` | Semente inicia `extra=0`; `reabre` soma `n`; CLI converte o argumento para inteiro. `bin/disjuntor.py:1433`, `bin/disjuntor.py:1697`, `bin/disjuntor.py:2463`. | **Ambos os tetos**, mensagem, painel, trilha REABRE e stdout `extra=N`. `bin/disjuntor.py:1496`, `bin/disjuntor.py:1613`, `bin/disjuntor.py:1478`, `bin/disjuntor.py:1758`, `bin/disjuntor.py:1703`, `bin/disjuntor.py:2467`. |
| `voltas.chaves` | Semente vazia; Codex e Agent acrescentam suas chaves. `bin/disjuntor.py:1433`, `bin/disjuntor.py:1499`, `bin/disjuntor.py:1616`. | Idempotência Codex; a idempotência Agent usa `reservas/autorizados`. `bin/disjuntor.py:1494`, `bin/disjuntor.py:1578`. |
| `sem_transcript`, `semente_parcial` | Semente marca ausência/falhas; complementação atualiza a parcialidade e remove `sem_transcript`. `bin/disjuntor.py:1438`, `bin/disjuntor.py:1446`, `bin/disjuntor.py:1448`, `bin/disjuntor.py:1470`, `bin/disjuntor.py:1471`. | Complementação única da contagem Agent e indicação de parcialidade no painel. `bin/disjuntor.py:1466`, `bin/disjuntor.py:1759`. |
| Recado e trilha `FALHA stop b1_disparados` | Catch específico do hook. `bin/hook_stop_fila1.py:1058`, `bin/hook_stop_fila1.py:1060`. | O caso exige recados B3/B1 e a linha específica no log; o leitor geral percorre eventos FALHA, mas só acrescenta à contagem `sem_relato` quando o detalhe é `relato_negacao`. `bin/tests/test_disjuntor.sh:2088`, `bin/tests/test_disjuntor.sh:2091`, `bin/disjuntor.py:2286`, `bin/disjuntor.py:2305`. |

`extra` amplia separadamente os limites Agent e Codex: com ambos em seis e `extra=1`, cabe mais um de cada. Não é um saldo único consumido pelos dois. (`bin/disjuntor.py:1496`, `bin/disjuntor.py:1613`.)

**3. O que cada caso/mutante novo ou alterado realmente prova**

“Passa” e “cai” nesta tabela descrevem os critérios escritos no selo; a execução integral nesta cópia é **[nao verificado]**.

| Caso / mutantes | Avaliação |
|---|---|
| `v4_ausente`; `v4-ausente-vazio` | **Morde a guarda interna.** O acréscimo chama `_com_estado` diretamente e exige AUSENTE sem recriação, evitando o sombreamento pela foto externa. `bin/tests/test_disjuntor.sh:1841`, `bin/tests/test_disjuntor.sh:1845`, `bin/tests/test_disjuntor.sh:1847`, `bin/tests/test_disjuntor.sh:2455`. |
| `v4-ausente-vazio-leitura` | **Morde o leitor externo**, inclusive por B1/painel. Se `ler_estado` voltar `{}`, esses caminhos deixam de produzir o “não sei” exigido. `bin/tests/test_disjuntor.sh:1834`, `bin/tests/test_disjuntor.sh:1838`, `bin/tests/test_disjuntor.sh:2456`, `bin/disjuntor.py:571`. |
| `v4_trava_curta`; `v4-porta-le-sob-trava` | **Tem sinal positivo:** leitura iniciada, transação concorrente concluída e reserva efetivamente gravada. A mutação move a foto para o callback sob trava. Ressalva dos 15 segundos descrita acima. `bin/tests/test_disjuntor.sh:1865`, `bin/tests/test_disjuntor.sh:1890`, `bin/tests/test_disjuntor.sh:1900`, `bin/tests/test_disjuntor.sh:2457`. |
| `v4-b1-mede-sob-trava` | **Mordida falsa para o defeito nomeado:** `UnboundLocalError` antes da medição; reprovação por ausência de evento. `bin/disjuntor.py:583`, `bin/tests/test_disjuntor.sh:1886`, `bin/tests/test_disjuntor.sh:2476`. |
| `v5-porta-nega-cedo`, `v5-b1-nao-mede` | **Mordem a ausência de operação útil**, porque exigem entrada no trecho observado e resultado válido. Não demonstram duração de trava. `bin/tests/test_disjuntor.sh:1886`, `bin/tests/test_disjuntor.sh:1901`, `bin/tests/test_disjuntor.sh:1903`, `bin/tests/test_disjuntor.sh:2478`, `bin/tests/test_disjuntor.sh:2479`. |
| `v5_foto_velha`; `v5-foto-velha-passa` | **Morde o sétimo autorizado pela foto antiga.** Exige negativa com seis vivos e exatamente duas fotos quando a segunda resolve. `bin/tests/test_disjuntor.sh:1949`, `bin/tests/test_disjuntor.sh:1951`, `bin/tests/test_disjuntor.sh:2460`. |
| `v6_poda_seq`; `v6-seq-cego-reescrita`, `v6-poda-sem-seq` | **Mordem retirada da comparação/incremento** nos cenários fabricados de reescrita e alias. O caso exige encolhimento e duas fotos até a negativa correta; não intercala SessionStart. `bin/tests/test_disjuntor.sh:1968`, `bin/tests/test_disjuntor.sh:1995`, `bin/tests/test_disjuntor.sh:2008`, `bin/tests/test_disjuntor.sh:2461`, `bin/tests/test_disjuntor.sh:2462`. |
| `v6-seq-lido-depois` | **Morde uma sequência artificialmente enorme**, não a leitura feita depois dos bytes. Falta a intercalação durante a foto. `bin/tests/test_disjuntor.sh:2463`, `bin/tests/test_disjuntor.sh:1934`, `bin/tests/test_disjuntor.sh:1937`. |
| `v6-foto-duas-tentativas` | **Morde exatamente a redução de quatro para duas fotos:** agora há contador explícito, além da frase “não sei”. `bin/tests/test_disjuntor.sh:1953`, `bin/tests/test_disjuntor.sh:1961`, `bin/tests/test_disjuntor.sh:2470`. |
| `v5_b1_checado`, modo `cede`; `v5-b1-velha-sobrescreve` | **Morde:** mede sem disparo, injeta registro concorrente com hora maior e exige preservação de hora, motivo e ausência de disparo. `bin/tests/test_disjuntor.sh:2034`, `bin/tests/test_disjuntor.sh:2036`, `bin/tests/test_disjuntor.sh:2043`, `bin/tests/test_disjuntor.sh:2464`. |
| Mesmo caso, modo `dispara`; `v6-b1-disparo-cede`, `v6-b1-checado-regride` | **Mordem:** exigem disparo verdadeiro, registro disparado e conservação de `checado=agora+5`. `bin/tests/test_disjuntor.sh:2034`, `bin/tests/test_disjuntor.sh:2044`, `bin/tests/test_disjuntor.sh:2465`, `bin/tests/test_disjuntor.sh:2466`. |
| `v5_reabre_ausente`; `v5-reabre-traceback` | **Morde:** exige rc 4, mensagem AUSENTE, ausência de traceback e arquivo ainda inexistente. `bin/tests/test_disjuntor.sh:2053`, `bin/tests/test_disjuntor.sh:2057`, `bin/tests/test_disjuntor.sh:2467`. |
| Novo ENOSPC; `v6-reabre-sem-gravou` | **Morde:** falha fabricada em `os.replace`, rc 4, nenhum sucesso no stdout, nenhuma trilha `extra=4` e conservação do valor anterior. `bin/tests/test_disjuntor.sh:2067`, `bin/tests/test_disjuntor.sh:2077`, `bin/tests/test_disjuntor.sh:2078`, `bin/tests/test_disjuntor.sh:2472`. |
| `v5_stop_b1_naosei`; `v6-stop-b1-sem-trilha`, `v5-stop-b1-engole` | **Mordem os sinais específicos:** os dois recados e a trilha B1. Não dependem apenas de saída vazia ou retorno genérico. `bin/tests/test_disjuntor.sh:2088`, `bin/tests/test_disjuntor.sh:2091`, `bin/tests/test_disjuntor.sh:2471`, `bin/tests/test_disjuntor.sh:2473`. |
| Controles sem mutação | Confirmam que a cópia usada pelo harness passa o caso original; **não corrigem** a aceitação de qualquer erro no mutante. `bin/tests/test_disjuntor.sh:2107`, `bin/tests/test_disjuntor.sh:2115`, `bin/tests/test_disjuntor.sh:2130`, `bin/tests/test_disjuntor.sh:2133`. |

Há outras duas lacunas concretas nas asserções novas:

- **Motivo persistido de B1:** trocar somente a escrita por `motivo=''`, preservando o retorno correto, passa os dois modos novos. `dispara` verifica o motivo retornado, mas não `g['motivo']`; `cede` conserva o registro injetado. A chamada seguinte, porém, leria o motivo vazio do estado. (`bin/disjuntor.py:588`, `bin/disjuntor.py:592`, `bin/disjuntor.py:573`, `bin/tests/test_disjuntor.sh:2043`, `bin/tests/test_disjuntor.sh:2044`.)
- **Estado conservado no ENOSPC:** contar uma ocorrência de `"extra": 2` não identifica `OBRA-RA` nem comprova conservação integral do objeto. Um estado com esse valor transferido para outra obra satisfaz esse predicado. A integração entre `--reabre` e abertura efetiva de novas voltas existe no caso anterior `i1_voltas`. (`bin/tests/test_disjuntor.sh:2078`, `bin/tests/test_disjuntor.sh:1593`, `bin/tests/test_disjuntor.sh:1594`, `bin/tests/test_disjuntor.sh:1601`, `bin/tests/test_disjuntor.sh:1602`.)

**4. A regex de hora: escritores, leitores e mordidas**

| Valor | Escritor | Leitor |
|---|---|---|
| Código do juiz / `LIN` | Heredoc do selo; definição da regex. `bin/tests/test_hora_da_secao_nao_e_futura.sh:19`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:39`. | `LIN.match(txt)`. `bin/tests/test_hora_da_secao_nao_e_futura.sh:61`. |
| `txt`, `quando` | Saída de `git blame`; texto da linha e `author-time`. `bin/tests/test_hora_da_secao_nao_e_futura.sh:25`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:43`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:46`. | Reconhecedores de título/lista e cálculo de `ref=min(quando, agora)`. `bin/tests/test_hora_da_secao_nao_e_futura.sh:47`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:61`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:66`. |
| `hh`, `d10`, `un`, `mi` | Grupos de `LIN`; `x` vira unidade zero. `bin/tests/test_hora_da_secao_nao_e_futura.sh:64`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:65`. | Validação de faixa/corte, dia mais próximo e comparação temporal. `bin/tests/test_hora_da_secao_nao_e_futura.sh:67`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:69`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:70`. |
| Linhas fabricadas | `printf` e commits dos cenários positivos e negativos. `bin/tests/test_hora_da_secao_nao_e_futura.sh:100`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:106`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:127`. | `julga`, executando o juiz selecionado. `bin/tests/test_hora_da_secao_nao_e_futura.sh:78`. |
| Regex desligada / lookahead antigo | Substituições no juiz copiado. `bin/tests/test_hora_da_secao_nao_e_futura.sh:119`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:139`. | Exigências de rc **exatamente 0** e **exatamente 1**, respectivamente. `bin/tests/test_hora_da_secao_nao_e_futura.sh:123`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:144`. |

**A alteração para `[\w/]` resolve os sufixos apresentados.** Os negativos `h`, `ms`, `abc`, `H` e `xyz` passa

[truncado em 20000 de 22258 caracteres]

## 01/11 12:34 · sessao a1ab4b92cafc · - · - · origem=Ronald (tmux)

Achado principal: `v4-b1-mede-sob-trava` morre pelo motivo errado. O mutante move `med = medir_b1(...)` para dentro de `_fn`, imediatamente antes da atribuição `b1[agent_id]`, mas a guarda nova já lê `med['disparo']` antes disso; a atribuição interna também torna `med` variável local de `_fn`. Resultado confirmado executando somente AST em memória: `UnboundLocalError`, sem chamar `medir_b1`. (`bin/tests/test_disjuntor.sh:2476`, `bin/disjuntor.py:583`, `bin/disjuntor.py:587`.)

O evento `no_meio` só seria marcado ao entrar em `medir_b1`; sem medida, `_lento` sai por ausência desse evento, com código 2, que `_mutante2` aceita como queda do mutante. Portanto o selo não demonstra esse defeito de trava no B1. (`bin/tests/test_disjuntor.sh:1870`, `bin/tests/test_disjuntor.sh:1886`, `bin/tests/test_disjuntor.sh:2130`.)

Contraprova concreta: mover a medida para o início de `_fn`, antes de qualquer uso de `med`, representaria uma implementação funcional que mede sob trava; o mutante atual não alcança essa situação. (`bin/disjuntor.py:578`, `bin/disjuntor.py:583`, `bin/tests/test_disjuntor.sh:2476`.)

Quanto ao handshake:

- O caso da porta efetivamente observa leitura de transcript em andamento e verifica que a concorrente termina com trava curta, além de exigir autorização e reserva persistida. O mutante da porta põe a foto sob trava e alcança o evento; a concorrente encontra a trava ocupada. (`bin/tests/test_disjuntor.sh:1865`, `bin/tests/test_disjuntor.sh:1890`, `bin/tests/test_disjuntor.sh:1900`, `bin/tests/test_disjuntor.sh:2457`; `bin/disjuntor.py:294`.)
- “Sem prazo de sono”/“sempre” exagera o determinismo: as duas esperas têm prazo de 15 segundos. Se a thread principal ficar sem execução por mais de 15 segundos entre observar `no_meio` e tentar a trava, a leitura lenta termina e uma implementação que lê sob trava pode passar. É limite de escalonamento do teste, distinto do falso kill do B1. (`bin/tests/test_disjuntor.sh:1855`, `bin/tests/test_disjuntor.sh:1868`, `bin/tests/test_disjuntor.sh:1872`, `bin/tests/test_disjuntor.sh:1888`.)

Mapa da hora/regex:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| Código do juiz e `LIN` | Heredoc do shell; regex definida na linha 39 | `LIN.match(txt)` na linha 61 |
| `txt` | Saída de `git blame`, retirando `\t` | `TIT.match` e `LIN.match` |
| `quando` | `author-time` do blame | `ref=min(quando, agora)`; depois é limpo por linha |
| `hh`, `d10`, `un`, `mi` | Grupos da regex; `x` vira unidade zero | Faixa válida, escolha de dia mais próximo e comparação com `ref` |
| Fixtures com hora | `printf` e commits no repositório fabricado | `julga()` executa o juiz | [nao verificado]
| Regex mutante desligada | Substituição por `(?!)` | Exige retorno exatamente 0 |
| Lookahead mutante velho | Substituição de `[\w/]` por `[\d/h]` | Exige retorno exatamente 1 para `ms` |

Referências do mapa: `bin/tests/test_hora_da_secao_nao_e_futura.sh:19`, `:25`, `:39`, `:43`, `:46`, `:61`, `:64`, `:65`, `:66`, `:67`, `:69`, `:70`, `:72`, `:78`, `:100`, `:106`, `:119`, `:123`, `:127`, `:139`, `:141`, `:144`.

Fora do diff, a régua executa todos os `test_*.sh` e bloqueia na primeira falha; essa é a ligação do selo ao fluxo. (`bin/regua.sh:138`, `bin/regua.sh:140`.)

O escritor de linhas de máquina monta `**DD/MM HH:MM:SS ...` e passa pela porta de escrita do vigia; isso não corresponde ao formato `- HH:MM` desta regex. O formato com data tem controle negativo explícito no teste. (`bin/disjuntor.py:254`, `bin/disjuntor.py:255`, `app/core/esteira_vigia.py:815`, `app/core/esteira_vigia.py:882`, `app/core/esteira_vigia.py:884`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:100`.)

Avaliação dos casos de hora:

- Os positivos minuto exato, dezena e negrito exigem acusação, e o mutante de ramo desligado exige verde exato: há evidência de funcionamento do ramo, não morte por erro de execução. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:105`, `:109`, `:119`, `:123`.)
- Os negativos `h`, `ms`, `abc`, `H` e `xyz` passam por ausência de correspondência da regex; isso é o comportamento intencional de excluir tokens colados. O mutante velho acusando `ms` comprova concretamente a mudança para essa classe, com código 1 exato. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:126`, `:130`, `:139`, `:144`.)
- Não há mutante individual para cada sufixo. Um lookahead enumerado `(?![hmsabcHyz/])` passaria as cinco fixtures mas acusaria falsamente `- 13:00q`, `- 13:00_` ou `- 13:00é`; portanto as fixtures não provam universalmente “qualquer token”. A regex real com `\w` cobre esses três exemplos. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:39`, `:126`.)
- “Token inteiro” também precisa de definição: `- **13:00**ms`, `- 13:00́abc` (acento combinante) e `- 13:00:59` ainda correspondem à regex atual, que só examina o próximo caractere imediato. Verificado em memória com a regex literal; a intenção semântica desses formatos é [nao verificado]. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:39`.)
- Limite declarado, não defeito novo: às 12:00, `- 01:00` é interpretado como 01:00 passado; se a intenção fosse 01:00 do dia seguinte, a antecipação escaparia. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:37`, `:69`.)

Não executei os scripts de teste, pois criam arquivos e commits; as verificações executadas foram somente leitura e simulações em memória. [nao verificado] [nao verificado]

## 01/11 12:34 · sessao 75b05c593a38 · - · - · origem=Ronald (tmux)

Achados centrais:

1. **A poda de SessionStart não participa de `poda_seq`.** `porta_agent` compara a sequência em `bin/disjuntor.py:1600` e a incrementa apenas quando poda por resultado (`:1602–1606`), mas `poda_reservas` também elimina reservas (`:1661–1667`) sem incrementá-la. O chamador externo existe em `bin/marco_de_clear.py:31`, acionado por `:81`. Contraexemplo reproduzido somente em memória: S1 tem cinco vivos; B reserva sexto; A fotografa cinco/seq=0; chega resultado de B; SessionStart S2 remove a reserva B, mantém seq=0; A atrasado de S1 autoriza, ficando seis vivos no transcript mais uma reserva. Aceitação vem de `:1600`, `:1607`, `:1617`. Sobrevivência efetiva de chamadas S1 após clear em produção: [nao verificado].

2. **`v6-seq-lido-depois` não testa o defeito nomeado.** O mutante substitui a sequência por `10 ** 9`, sem mover leitura alguma (`bin/tests/test_disjuntor.sh:2463`). Os testes terminam `foto_a` antes da poda C (`:1934–1939`, `:1988–1996`), portanto uma implementação realmente errada que leia sequência depois dos bytes passa. Reproduzi em memória os critérios de `v5_foto_velha` com essa mutação real: A relê duas vezes; D nega após quatro. O contraexemplo necessário é poda entre leitura dos bytes antigos e leitura da sequência: foto fica `(seq_nova, cinco_vivos_antigos)` e passa pela igualdade (`bin/disjuntor.py:1600`).

3. **`v4-b1-mede-sob-trava` morre pelo motivo errado.** A inserção de `med = medir_b1(...)` em `_fn` faz `med` local desse callback (`bin/tests/test_disjuntor.sh:2476–2477`); a nova guarda já usa `med` antes da atribuição (`bin/disjuntor.py:583`). Resultado reproduzido em memória: `UnboundLocalError`, sem medir. `_lento` captura a exceção (`bin/tests/test_disjuntor.sh:1883–1884`) e falha por não receber o evento (`:1886–1887`); o harness aceita qualquer não zero (`:2130`). Não prova contenção da trava.

Mapa completo do trecho:

| Campo/valor | Escritores | Leitores |
|---|---|---|
| `poda_seq` | Único incremento em `porta_agent`, `bin/disjuntor.py:1606`; persistência genérica por `_com_estado`, `:316–317` | `_foto_sessao` antes dos bytes, `:1641`; comparação sob trava, `:1600`; incremento lê valor anterior, `:1606` |
| `podado_ate` | Diff apenas remove, `bin/disjuntor.py:1599` | Nenhum leitor restante no código pesquisado; marca antiga recebida pelo estado desaparece quando passa por esse ponto, `:1599` |
| `foto=(seq,vivos,feitos)` | `_foto_sessao`, `bin/disjuntor.py:1641–1648`; deriva vivos de `_sem_entrega`, `:423–431`, e resultados de `_resultados`, `:1674–1688` | `porta_agent`, `:1589–1607`; fotografia nova a cada tentativa, `:1623–1626` |
| `reservas[tuid]` | Inicialização `bin/disjuntor.py:1577`; autorização grava `{t,obra,sessao,agente}`, `:1617`; poda por resultado, `:1602–1604`; poda SessionStart, `:1657–1667` | Idempotência `:1578`; poda por resultado `:1602`; contagem `:1607`; poda SessionStart usa apenas `sessao`, `:1661` |
| `reservas[*].t/obra/agente` | `bin/disjuntor.py:1617` | Nenhum leitor decisório encontrado; teste verifica existência de `agente`, `bin/tests/test_disjuntor.sh:1814` |
| `autorizados` | Inicialização/append/corte de 500, `bin/disjuntor.py:1618–1620` | Idempotência `:1578` |
| `FOTO_RELEITURAS=3` | Constante `bin/disjuntor.py:119` | Laço de quatro fotos `:1623`, negativa final `:1630–1631`; teste conta fotos `bin/tests/test_disjuntor.sh:1961` |
| `_FOTO_VELHA` | Sentinela `bin/disjuntor.py:120`, retornada em `:1601` | Comparação por identidade `:1627` |
| `gravou` da porta | `_com_estado` acrescenta booleano, `bin/disjuntor.py:325–326` | Autorização exige `[True]`, `:1632–1633` |
| `payload.session_id` | Fornecido pelo chamador; produção externa [nao verificado] | Exigido em `bin/disjuntor.py:1566–1568`; gravado como dono em `:1617`; SessionStart recebe no stdin, `bin/marco_de_clear.py:44–48`, `:81` |
| `payload.transcript_path` | Fornecido pelo chamador; produção externa [nao verificado] | Porta lê em `bin/disjuntor.py:1569`; `_foto_sessao` abre em `:1643–1644` |
| `tool_use_id`/`feitos` | Resultados sintéticos do selo em `bin/tests/test_disjuntor.sh:1926–1929`, `:1977–1981`; escritor real do harness [nao verificado] | Extração `bin/disjuntor.py:1684–1687`; poda `:1602` |

Classificação dos casos/mutantes associados:

- `v5_foto_velha`: morde perda do vivo após poda concorrente; exige negativa com seis vivos e segunda foto (`bin/tests/test_disjuntor.sh:1949–1952`), depois negativa restritiva com quatro fotos (`:1953–1961`).
- `v6_poda_seq`: morde os cenários fabricados de compactação textual e alias, com pré-condição explícita de encolhimento (`:1991–1995`) e negativa/sequência de duas fotos (`:2004–2008`). Não cobre eliminação de registros vivos nem poda SessionStart.
- `v5-foto-velha-passa` e `v6-seq-cego-reescrita`: retiram realmente a comparação (`:2460–2461`); os casos detectam autorização indevida pelo retorno `None` (`:1949–1950`, `:2006–2007`).
- `v6-poda-sem-seq`: retira realmente o incremento (`:2462`), deixando foto velha indistinguível; mesmo critério positivo acima.
- `v6-seq-lido-depois`: mata um bypass com número enorme; não prova ordem da observação (`:2463`).
- `v6-foto-duas-tentativas`: morde exatamente a redução de quatro para duas; contador explícito em `:1961`, mutação em `:2470`.
- `v4-porta-le-sob-trava`: move efetivamente a foto para callback (`:2457–2459`); handshake da leitura em `:1865–1869`, concorrente em `:1890`, liberação após tentativa em `:1893–1894`. O teste também exige reserva persistida (`:1900–1901`). [nao verificado]
- `v4-b1-mede-sob-trava`: falso positivo por `UnboundLocalError`, explicado acima.
- `v5-porta-nega-cedo`/`v5-b1-nao-mede`: detectam ausência de operação útil pelo evento e/ou resultado exigido (`:1886–1887`, `:1900–1903`), mas não são evidência de tempo de trava (`:2478–2479`).
- `v4_ausente`: o acréscimo testa `_com_estado` diretamente e exige erro AUSENTE e arquivo ainda inexistente (`:1845–1848`); cobre a guarda sob trava antes sombreada pela nova leitura externa (`:1839–1840`).

Limites adicionais: a sequência global detecta poda posterior à leitura da sequência; não autentica que o transcript ainda contém todos os agentes cuja reserva já foi podada (`bin/disjuntor.py:1641–1648`, `:1602–1607`). Contraexemplo de código: após poda deixar seis agentes apenas no transcript, reescrita que retire esses seis registros produz vivos vazios e permite nova autorização sem mudar `poda_seq` (`:423–431`, `:1648`, `:1607–1617`); ocorrência real dessa reescrita: [nao verificado].

## 01/11 12:33 · sessao 94b5515c8b9d · - · - · origem=Ronald (tmux)

Achado principal: **o mutante `v4-b1-mede-sob-trava` morre antes de medir**, portanto seu vermelho não prova o defeito que nomeia. Ele retira a medida externa e insere `med = medir_b1(...)` dentro de `_fn`, depois do acesso a `med['disparo']`; a atribuição torna `med` local em toda `_fn`, produzindo `UnboundLocalError` em `bin/disjuntor.py:583` antes da medida. A transformação está em `bin/tests/test_disjuntor.sh:2476`; a espera por `no_meio` falha em `bin/tests/test_disjuntor.sh:1886`; o harness aceita qualquer retorno diferente de zero em `bin/tests/test_disjuntor.sh:2130`. Reproduzi a função mutada por AST somente em memória: `UnboundLocalError`, zero chamadas a `medir_b1`. A reprodução não gera arquivo; a localização causal é `bin/disjuntor.py:583`.

Mapa de B1:

| Campo/valor | Escritor | Leitor/efeito |
|---|---|---|
| `med['disparo']`, `med['motivo']` | `medir_b1`, inclusive resultados “não sei”, isenção e disparo: `bin/disjuntor.py:472`, `bin/disjuntor.py:480`, `bin/disjuntor.py:488`, `bin/disjuntor.py:520`, `bin/disjuntor.py:533` | Guarda de precedência: `bin/disjuntor.py:583`; estado: `bin/disjuntor.py:587`; anúncio: `bin/disjuntor.py:589`; retorno: `bin/disjuntor.py:592` |
| `b1[agent].checado` | `b1_do_agente`, máximo do instante de entrada e valor já gravado: `bin/disjuntor.py:587` | Cache de um minuto: `bin/disjuntor.py:574`; precedência de medida sem disparo: `bin/disjuntor.py:583`; cálculo do máximo: `bin/disjuntor.py:587` |
| `b1[agent].disparado` | Mesmo registro: `bin/disjuntor.py:587` | Atalho sem trava: `bin/disjuntor.py:572`; preservação sob trava: `bin/disjuntor.py:581`; lista para Stop: `bin/disjuntor.py:597` |
| `b1[agent].motivo` | Mesmo registro: `bin/disjuntor.py:588` | Retorno do cache e das duas guardas: `bin/disjuntor.py:573`, `bin/disjuntor.py:575`, `bin/disjuntor.py:582`, `bin/disjuntor.py:584`; PreToolUse usa retorno em `bin/disjuntor.py:2148` e `bin/disjuntor.py:2152` |
| Resultado B1 para Stop | `b1_disparados` filtra apenas `disparado`: `bin/disjuntor.py:597` | Hook cruza com vivos: `bin/hook_stop_fila1.py:1053`; monta TaskStop: `bin/hook_stop_fila1.py:1061` |
| Falha B1 no Stop | Hook escreve recado e trilha: `bin/hook_stop_fila1.py:1058`, `bin/hook_stop_fila1.py:1060` | Caso exige recados B3 e B1 e trilha: `bin/tests/test_disjuntor.sh:2088` |

A persistência material de todos esses campos passa por `json.dump` e `os.replace`, em `bin/disjuntor.py:316` e `bin/disjuntor.py:317`. Falha de escrita mantém o retorno da decisão em `bin/disjuntor.py:324` e `bin/disjuntor.py:329`.

Consequência fora do diff: **B1 ainda pode anunciar e devolver disparo sem persistir o registro**. `dispara` roda dentro do callback, antes da tentativa de escrita, em `bin/disjuntor.py:590`; `b1_do_agente` não pede o indicador `gravou` em `bin/disjuntor.py:593`. Contraexemplo: estado anterior sem disparo, medida verdadeira, `os.replace` levanta; PreToolUse nega aquela chamada, mas o Stop lê o estado antigo e não encontra esse agente disparado. O encadeamento está em `bin/disjuntor.py:317`, `bin/disjuntor.py:324`, `bin/disjuntor.py:597` e `bin/hook_stop_fila1.py:1055`. Ocorrência em produção: [nao verificado].

Mapa de `voltas`/`extra`:

| Campo/valor | Escritor | Leitor/efeito |
|---|---|---|
| `b3[obra].voltas` | `_voltas` instala cópia da semente: `bin/disjuntor.py:1464`; semente começa com `agent=0`, `codex=0`, `extra=0`, `chaves=[]`: `bin/disjuntor.py:1433` | `reserva_volta_codex`: `bin/disjuntor.py:1493`; `porta_agent`: `bin/disjuntor.py:1612`; `reabre`: `bin/disjuntor.py:1696`; painel: `bin/disjuntor.py:1756` |
| `agent` | Semente conta primeiros prompts: `bin/disjuntor.py:1454`; complementação sem transcript: `bin/disjuntor.py:1469`; porta incrementa: `bin/disjuntor.py:1615` | Teto Agent: `bin/disjuntor.py:1613`; recado: `bin/disjuntor.py:1478`; painel: `bin/disjuntor.py:1757` |
| `codex` | Semente conta sidecars verificador da obra: `bin/disjuntor.py:1440`; reserva incrementa: `bin/disjuntor.py:1498` | Teto Codex: `bin/disjuntor.py:1496`; recado: `bin/disjuntor.py:1478`; painel: `bin/disjuntor.py:1757` |
| `extra` | Inicialmente zero: `bin/disjuntor.py:1433`; `reabre` soma `n`: `bin/disjuntor.py:1697` | Ambos os tetos: `bin/disjuntor.py:1496`, `bin/disjuntor.py:1613`; recado: `bin/disjuntor.py:1478`; painel: `bin/disjuntor.py:1758`; trilha REABRE: `bin/disjuntor.py:1703`; stdout CLI: `bin/disjuntor.py:2467` |
| `chaves` | Semente vazia: `bin/disjuntor.py:1433`; Codex append: `bin/disjuntor.py:1499`; Agent append: `bin/disjuntor.py:1616` | Idempotência Codex: `bin/disjuntor.py:1494` |
| `sem_transcript`, `semente_parcial` | Semente: `bin/disjuntor.py:1438`, `bin/disjuntor.py:1446`; complementação: `bin/disjuntor.py:1470` | Complementação única: `bin/disjuntor.py:1466`; painel declara parcial: `bin/disjuntor.py:1759` |
| `gravou` | `_com_estado` faz append de `ok`: `bin/disjuntor.py:325` | Novo `reabre` recusa anúncio se não houve escrita: `bin/disjuntor.py:1701`; CLI converte exceção em rc 4: `bin/disjuntor.py:2464` |

Os dois tetos usam separadamente o mesmo `extra`: com `agent=6`, `codex=6`, `extra=1`, ainda cabe um Agent **e** um Codex. Essa é a expressão efetiva dos leitores, não uma cota única compartilhada. `bin/disjuntor.py:1496`, `bin/disjuntor.py:1613`.

Avaliação dos casos/mutantes desta parte:

- `v4_ausente`, complemento novo: **morde a guarda interna**. Chama `_com_estado` diretamente com arquivo ausente e exige `RuntimeError` contendo AUSENTE sem recriação; evita o sombreamento pelo leitor externo da foto. `bin/tests/test_disjuntor.sh:1841`, `bin/tests/test_disjuntor.sh:1845`, `bin/tests/test_disjuntor.sh:1847`. O mutante da leitura externa continua sendo acusado pelo B1/painel, que usam `ler_estado`. `bin/tests/test_disjuntor.sh:1834`, `bin/tests/test_disjuntor.sh:1838`, `bin/disjuntor.py:571`.

- `_lento`, porta: o handshake exige leitura iniciada, tenta uma transação concorrente e depois exige reserva persistida; há sinal positivo de operação. `bin/tests/test_disjuntor.sh:1865`, `bin/tests/test_disjuntor.sh:1890`, `bin/tests/test_disjuntor.sh:1900`. O mutante da porta efetivamente move a foto para dentro da trava. `bin/tests/test_disjuntor.sh:2457`.

- `_lento`, B1: a base exige entrada real no wrapper de medida e retorno com formato de tupla; contudo o mutante atualizado é inválido por variável local não inicializada, como explicado acima. `bin/tests/test_disjuntor.sh:1870`, `bin/tests/test_disjuntor.sh:1903`, `bin/tests/test_disjuntor.sh:2476`.

- `v5_b1_checado`, `dispara`: **morde** `v6-b1-disparo-cede` e `v6-b1-checado-regride`: mede disparo verdadeiro, injeta registro sem disparo com `checado=agora+5`, exige retorno verdadeiro, registro disparado e hora preservada. `bin/tests/test_disjuntor.sh:2034`, `bin/tests/test_disjuntor.sh:2036`, `bin/tests/test_disjuntor.sh:2044`, `bin/tests/test_disjuntor.sh:2465`, `bin/tests/test_disjuntor.sh:2466`.

- `v5_b1_checado`, `cede`: **morde** `v5-b1-velha-sobrescreve`; exige motivo e hora do registro concorrente e ausência de disparo. `bin/tests/test_disjuntor.sh:2043`, `bin/tests/test_disjuntor.sh:2464`. O modo `dispara` isolado passaria ao remover a guarda; é o modo `cede` que distingue esse mutante. `bin/disjuntor.py:583`, `bin/tests/test_disjuntor.sh:2043`.

- Lacuna concreta de `v5_b1_checado`: trocar somente a escrita de `motivo` por `''` em `bin/disjuntor.py:588`, mantendo o retorno original em `bin/disjuntor.py:592`, passa as novas asserções do modo `dispara`, pois elas não conferem `g['motivo']`; o modo `cede` preserva o registro injetado e também passa. `bin/tests/test_disjuntor.sh:2043`, `bin/tests/test_disjuntor.sh:2044`. Isso permite negar corretamente agora e perder o motivo nas chamadas seguintes, cujos leitores estão em `bin/disjuntor.py:573`.

- `v5_reabre_ausente`, novo ENOSPC: **morde** `v6-reabre-sem-gravou`. Injeta falha em `os.replace`, exige rc 4, stdout sem `extra=`, stderr contendo “nao gravado”, nenhuma trilha `extra=4` e conservação de uma ocorrência `extra:2`. `bin/tests/test_disjuntor.sh:2067`, `bin/tests/test_disjuntor.sh:2077`, `bin/tests/test_disjuntor.sh:2472`. O controle de sucesso anterior exige exatamente `extra=2`. `bin/tests/test_disjuntor.sh:2059`.

- Lacuna concreta desse caso: contar `json.dumps(est).count('"extra": 2')` não prova que o valor pertence a `OBRA-RA`, nem que o restante do estado foi conservado. Um estado que movesse o único `extra=2` a outra obra passaria esse predicado. `bin/tests/test_disjuntor.sh:2078`. Também não verifica a abertura efetiva de uma volta após o incremento; essa integração aparece no caso anterior `i1_voltas`, em `bin/tests/test_disjuntor.sh:1585`.

- `v5_stop_b1_naosei`: **morde** o novo `v6-stop-b1-sem-trilha`, pois começa com estado ausente e exige a linha específica de falha além dos dois recados. `bin/tests/test_disjuntor.sh:2086`, `bin/tests/test_disjuntor.sh:2091`, `bin/tests/test_disjuntor.sh:2471`. O mutante que troca a captura de exceção por `ZeroDivisionError` também perde os recados pelo catch externo e cai. `bin/tests/test_disjuntor.sh:2473`, `bin/hook_stop_fila1.py:1065`.

Há ainda uma limitação forense em `reabre`: o anúncio REABRE acontece depois de liberar a trava, permitindo ordem de trilha invertida entre duas reaberturas concorrentes (`extra=4` seguido de `extra=2`). A escrita é serializada em `bin/disjuntor.py:296`; a trava fecha em `bin/disjuntor.py:330`; o anúncio vem depois, em `bin/disjuntor.py:1703`. O mecanismo `anuncia`, que executaria sob a trava, já existe em `bin/disjuntor.py:327`. O novo caso testa persistência falha, não essa concorrência. `bin/tests/test_disjuntor.sh:2061`.

## 01/11 12:11 · sessao eb9b6523731d · CONTENCAO-QUE-MORDE-V2 · verificador · origem=Code

**Reprovaria a garantia de ordenação de (b) e consideraria (a) incompleto:** `checado` ordena o início das chamadas, não as observações; bytes e caminho só ordenam fotos se o transcript mantiver identidade e crescer. Há também lacunas nos testes de trava, releituras e trilha. (`bin/disjuntor.py:583`, `bin/disjuntor.py:1595`, `bin/disjuntor.py:1620`, `bin/tests/test_disjuntor.sh:1853`, `bin/tests/test_disjuntor.sh:1936`, `bin/tests/test_disjuntor.sh:1983`)

1. **Escritores e leitores**

| Campo/saída | Quem escreve | Quem lê |
|---|---|---|
| `logs/sessao/disjuntor_estado.json` | `_com_estado`: lê o objeto inteiro e grava por temporário + `replace`. (`bin/disjuntor.py:303`, `bin/disjuntor.py:316`, `bin/disjuntor.py:317`) | `_com_estado` e `ler_estado`. (`bin/disjuntor.py:303`, `bin/disjuntor.py:338`) |
| `podado_ate[chave]` | A porta: chave `abspath(tp)`; valor máximo de bytes; atualização e descarte sob `_com_estado`. (`bin/disjuntor.py:1601`, `bin/disjuntor.py:1602`, `bin/disjuntor.py:1620`, `bin/disjuntor.py:1625`) | A própria porta compara tamanho com a marca. (`bin/disjuntor.py:1594`, `bin/disjuntor.py:1595`) |
| `reservas[tuid]` | A porta grava `{t, obra, sessao, agente}`, remove reservas com resultado; `poda_reservas` remove por sessão. (`bin/disjuntor.py:1597`, `bin/disjuntor.py:1615`, `bin/disjuntor.py:1663`) | A porta lê chaves para idempotência, resultados e contagem; a poda lê `sessao`. Os outros valores não participam dessas decisões. (`bin/disjuntor.py:1575`, `bin/disjuntor.py:1597`, `bin/disjuntor.py:1605`, `bin/disjuntor.py:1658`) |
| `b1[agent]`: `checado`, `disparado`, `motivo` | `b1_do_agente`, sob `_com_estado`. (`bin/disjuntor.py:585`, `bin/disjuntor.py:590`) | Espiada inicial e decisão sob trava; `b1_disparados` lê somente `disparado`. (`bin/disjuntor.py:571`, `bin/disjuntor.py:580`, `bin/disjuntor.py:594`) |
| Tupla B1 | `b1_do_agente`. (`bin/disjuntor.py:582`, `bin/disjuntor.py:584`, `bin/disjuntor.py:589`) | `pretool` usa booleano/motivo para negar. (`bin/disjuntor.py:2142`, `bin/disjuntor.py:2145`) |
| Recados de `_disjuntor_contido` | B3 monta os primeiros; o novo handler acrescenta B1 “nao sei”; B1 disparado acrescenta TaskStop. (`bin/hook_stop_fila1.py:1047`, `bin/hook_stop_fila1.py:1058`, `bin/hook_stop_fila1.py:1062`) | O retorno entra no `reason` do JSON de bloqueio. (`bin/hook_stop_fila1.py:1064`, `bin/hook_stop_fila1.py:1306`, `bin/hook_stop_fila1.py:1310`) |
| Nova trilha `FALHA` | Handler chama `DJ.trilha`; o append real ocorre em `trilha`, que contém falhas de escrita. (`bin/hook_stop_fila1.py:1060`, `bin/disjuntor.py:143`, `bin/disjuntor.py:147`, `bin/disjuntor.py:150`) | `ler` percorre `FALHA`, mas só incrementa o contador para `relato_negacao`; esta falha B1 não ganha contador específico. (`bin/disjuntor.py:2280`, `bin/disjuntor.py:2298`) |
| Saída/rc `--reabre` | `main`: erro capturado → stderr/4; sucesso → `extra=N`/0. (`bin/disjuntor.py:2455`, `bin/disjuntor.py:2459`, `bin/disjuntor.py:2461`) | Chamadores encontrados são testes: anteriores exigem sucesso; o novo exige rc 4 e mensagem na ausência. (`bin/tests/test_disjuntor.sh:1593`, `bin/tests/test_disjuntor.sh:1601`, `bin/tests/test_disjuntor.sh:1968`, `bin/tests/test_disjuntor.sh:1974`) |

Os demais consumidores do estado são ancestria, texto de voltas, B4 e contador sem obra; eles selecionam campos sem reconstruir e gravar um estado reduzido. (`bin/disjuntor.py:1337`, `bin/disjuntor.py:1747`, `bin/disjuntor.py:2170`, `bin/disjuntor.py:2353`)

`_com_estado` preserva campos desconhecidos porque regrava o mesmo objeto carregado. **`poda_reservas` não conhece nem limpa `podado_ate`**; `marco_de_clear.py` apenas delega a poda. (`bin/disjuntor.py:304`, `bin/disjuntor.py:311`, `bin/disjuntor.py:316`, `bin/disjuntor.py:1653`, `bin/marco_de_clear.py:31`)

O Stop consulta também o inode do arquivo para rejeitá-lo como prova de espera; a publicação do ESTADO consome `disjuntor.ler/linha`. (`bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:590`, `bin/hook_stop_fila1.py:598`, `bin/gerar_estado.py:280`)

Há escrita fora de `_com_estado` **nas fixtures**, inclusive criação, corrupção, remoção e restauração direta do JSON. Outro escritor de execução em `bin/`, `app/` ou `.claude/`: não encontrado no censo [nao verificado]. (`bin/tests/test_disjuntor.sh:52`, `bin/tests/test_disjuntor.sh:494`, `bin/tests/test_disjuntor.sh:1476`, `bin/tests/test_disjuntor.sh:1967`, `bin/tests/test_disjuntor.sh:1973`)

2. **(a): caminho e tamanho não identificam a geração**

- **Mesmo caminho, arquivo menor que a marca:** quatro fotos continuam menores; a chamada nega. Chamadas seguintes repetem a negativa enquanto o tamanho permanecer abaixo da marca. Não existe expiração dessa marca; SessionStart também não a limpa. Portanto pode bloquear indefinidamente, mas não necessariamente “para sempre”: crescer até a marca ou sofrer descarte entre as 50 chaves muda o resultado. (`bin/disjuntor.py:1595`, `bin/disjuntor.py:1603`, `bin/disjuntor.py:1622`, `bin/disjuntor.py:1629`, `bin/disjuntor.py:1653`)

- **Reescrita pode inverter a ordem dos tamanhos:** suponha A com foto antiga de 2.000 bytes e cinco vivos; após reescrita, B vê seis vivos em 1.500 bytes e poda a reserva correspondente. Se a marca anterior permitia essa poda, fica `podado_ate=1500`; A passa porque `2000 < 1500` é falso e pode autorizar o sétimo. É um contraexemplo condicional ao algoritmo; ocorrência dessa reescrita em produção: [nao verificado]. (`bin/disjuntor.py:1595`, `bin/disjuntor.py:1597`, `bin/disjuntor.py:1602`, `bin/disjuntor.py:1605`)

- **Troca de caminho/alias:** uma poda por `/p1/t.jsonl` não protege uma foto tomada por `/p2/t.jsonl`, mesmo que sejam hardlinks ou aliases por diretório pai para o mesmo arquivo. A chave não inclui inode nem sessão. Symlink no componente final é recusado pelo leitor. Ocorrência desses aliases em produção: [nao verificado]. (`bin/disjuntor.py:1620`, `bin/disjuntor.py:1595`, `bin/orcamento_contexto.py:74`)

- **Compact/clear não provam truncamento:** a cópia registra ambos os hooks, mas isso não demonstra como o harness reescreve seus transcripts. Nos testes, `_compacta` acrescenta um `compact_boundary`. Truncamento, rotação ou troca real de caminho pelo harness: [nao verificado]. (`.claude/settings.json:44`, `.claude/settings.json:57`, `bin/tests/test_disjuntor.sh:183`)

- **A trava sai corretamente:** o sentinel retorna pelo callback; `_com_estado` termina e fecha o descritor no `finally`; a próxima foto ocorre depois. São uma leitura inicial e três releituras. (`bin/disjuntor.py:1596`, `bin/disjuntor.py:329`, `bin/disjuntor.py:330`, `bin/disjuntor.py:1622`, `bin/disjuntor.py:1623`)

- **As 50 chaves são as últimas por poda**, não as sessões ativas nem as fotos em voo. Cinquenta outras chaves atualizadas podem expulsar uma marca ainda necessária para uma chamada suspensa, fazendo a comparação voltar a zero. (`bin/disjuntor.py:1602`, `bin/disjuntor.py:1603`, `bin/disjuntor.py:1595`)

3. **(b): uma observação mais nova pode ser descartada**

A ordem problemática não exige relógios dessincronizados: A recebe `agora=1000` e pausa antes de medir; B recebe `1001`, mede sem disparo e grava; chegam novos dados; A retoma e observa algo que dispara. A guarda descarta a observação de A porque compara **hora de entrada**, embora A tenha observado os arquivos depois de B. (`bin/disjuntor.py:2197`, `bin/disjuntor.py:576`, `bin/disjuntor.py:478`, `bin/disjuntor.py:520`, `bin/disjuntor.py:583`)

**`disparado=True` já gravado não é perdido por essa guarda:** a verificação anterior o preserva. O que pode desaparecer é um disparo recém-medido, ainda não gravado. (`bin/disjuntor.py:581`, `bin/disjuntor.py:583`)

Igualdade de timestamps também descarta a segunda observação; retrocesso do relógio pode impedir até a medição pelo cache inicial. O teste novo injeta `agora+5` e aceita a premissa de ordenação, sem testar esses intercalamentos. (`bin/disjuntor.py:574`, `bin/disjuntor.py:583`, `bin/tests/test_disjuntor.sh:1954`, `bin/tests/test_disjuntor.sh:1961`)

4. **(c)/(d): exceções e rc**

`--reabre` captura também erros de programação derivados de `Exception`, transformando-os em rc 4 sem traceback; o handler não distingue esses erros dos `RuntimeError` esperados de ausência, ilegibilidade e trava. `KeyboardInterrupt` e `SystemExit` escapam de `except Exception`. (`bin/disjuntor.py:2458`, `bin/disjuntor.py:301`, `bin/disjuntor.py:306`, `bin/disjuntor.py:308`)

**Há uma limitação anterior relevante:** falha ao gravar é contida por `_com_estado`; `reabre` não pede confirmação por `gravou`. Assim ainda pode anunciar `REABRE`, imprimir `extra=N` e retornar 0 sem persistência. O novo `except` não resolve isso. (`bin/disjuntor.py:319`, `bin/disjuntor.py:325`, `bin/disjuntor.py:1696`, `bin/disjuntor.py:1697`, `bin/disjuntor.py:2461`)

No Stop, conter erros já era o contrato. O handler interno conserva os recados anteriores e informa classe/motivo do erro B1; não introduz mudança de rc, pois seu retorno continua compondo o JSON `decision=block`. (`bin/hook_stop_fila1.py:1035`, `bin/hook_stop_fila1.py:1056`, `bin/hook_stop_fila1.py:1064`, `bin/hook_stop_fila1.py:1306`, `bin/hook_stop_fila1.py:1310`)

5. **(e): regex**

| Entrada | Resultado |
|---|---|
| `- 14:30` | Casa. |
| `- 14:3x` | Casa. |
| `- **14:30**` | Casa. |
| `**14:30**` sem `- ` | Não casa, como antes. |
| `- 14:30 h` | Casa; o espaço impede que o lookahead veja `h`. |
| `- 14:30h` | Não casa: mudança pretendida. |
| `- 14:30ms`, `- 14:30abc`, `- 14:3xyz`, `- 14:30H` | Casam pelo prefixo. |

Todos esses resultados decorrem de `bin/tests/test_hora_da_secao_nao_e_futura.sh:39`.

Portanto não perdeu os formatos de lista pedidos, mas **continua sem delimitação completa do token**. Os falsos prefixos acima já eram aceitos; a alteração apenas exclui `h` minúsculo imediatamente depois do minuto. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:39`)

6. **Casos e mutantes**

- **`v4_trava_curta` ainda depende do escalonamento.** O evento anuncia o início e a thread dorme 2,5 segundos; não há handshake mantendo-a parada até a concorrente tentar a trava. Se a concorrente demorar aproximadamente mais de 1,5 segundo, o mutante sob trava pode liberar dentro das tentativas restantes. O mutante sobrevive e a suíte falha de forma intermitente. (`bin/tests/test_disjuntor.sh:1853`, `bin/tests/test_disjuntor.sh:1858`, `bin/tests/test_disjuntor.sh:1875`, `bin/disjuntor.py:294`, `bin/tests/test_disjuntor.sh:2350`, `bin/tests/test_disjuntor.sh:2362`)

- **Os controles positivos melhoraram:** a porta precisa retornar `None` e deixar reserva; B1 precisa iniciar a medição e devolver tupla com booleano. Os mutantes “nega cedo” e “não mede” deixam de produzir o evento esperado. (`bin/tests/test_disjuntor.sh:1857`, `bin/tests/test_disjuntor.sh:1872`, `bin/tests/test_disjuntor.sh:1883`, `bin/tests/test_disjuntor.sh:1886`, `bin/tests/test_disjuntor.sh:2364`, `bin/tests/test_disjuntor.sh:2365`)

- **`v5_foto_velha` tem sinais positivos**, incluindo autorização inicial, poda e recusa por seis vivos. Entretanto, no ramo sempre velho, não exige quatro tentativas; uma negativa antecipada com a frase esperada passa essa parte. (`bin/tests/test_disjuntor.sh:1915`, `bin/tests/test_disjuntor.sh:1921`, `bin/tests/test_disjuntor.sh:1931`, `bin/tests/test_disjuntor.sh:1933`, `bin/tests/test_disjuntor.sh:1936`)

- **`v5_b1_checado` testa a comparação, não sua validade como relógio da observação:** injeta a marca futura e exige descartar o disparo. (`bin/tests/test_disjuntor.sh:1953`, `bin/tests/test_disjuntor.sh:1954`, `bin/tests/test_disjuntor.sh:1961`)

- **`v5_reabre_ausente` não passa por silêncio:** exige rc, mensagem, ausência de traceback e de recriação, seguido de controle com estado presente. Não cobre ilegibilidade/trava nem confirma a persistência do controle. (`bin/tests/test_disjuntor.sh:1967`, `bin/tests/test_disjuntor.sh:1969`, `bin/tests/test_disjuntor.sh:1974`, `bin/tests/test_disjuntor.sh:1975`)

- **`v5_stop_b1_naosei` não testa a trilha prometida:** remover somente `DJ.trilha('FALHA', ...)` deixa suas duas verificações satisfeitas. (`bin/tests/test_disjuntor.sh:1983`, `bin/tests/test_disjuntor.sh:1984`, `bin/hook_stop_fila1.py:1060`)

- **Hora:** exigir rc exatamente 0 no mutante evita aceitar erro de execução como mordida; minuto exato e duração têm resultados explícitos. Ainda dependem do relógio real e da data de ativação do ramo. (`bin/tests/test_hora_da_secao_nao_e_futura.sh:123`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:105`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:128`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:38`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:67`)

- **O harness de mutantes do disjuntor aceita qualquer rc não zero**, incluindo erro de execução. Âncoras únicas e controles reduzem mortes acidentais, mas não as distinguem da assertiva pretendida. Mutante novo concretamente morto por causa alheia: [nao verificado]. (`bin/tests/test_disjuntor.sh:2004`, `bin/tests/test_disjuntor.sh:2008`, `bin/tests/test_disjuntor.sh:2023`, `bin/tests/test_disjuntor.sh:2026`)

7. **Classes da casa e leis**

| Classe | Avaliação |
|---|---|
| Segundo juiz duplicando registrado | Não identifiquei introdução: vivos/resultados agora derivam dos mesmos bytes, e `_sem_entrega` é compartilhado. O registro vivo de autoridades é `core/juizes.py`. (`bin/disjuntor.py:420`, `bin/disjuntor.py:1645`, `app/docs/JUIZES.md:10`) |
| Segundo escritor fora da porta | As novas mutações de estado passam por `_com_estado`; as escritas diretas encontradas pertencem às fixtures. (`bin/disjuntor.py:590`, `bin/disjuntor.py:1625`, `bin/tests/test_disjuntor.sh:52`) |
| Relógio fora da borda | `time.time()` continua na entrada; o defeito é usá-lo para ordenar observações posteriores fora da trava. (`bin/disjuntor.py:2197`, `bin/disjuntor.py:576`, `bin/disjuntor.py:583`) |
| Derivação paralela de fonte única | A extração de `_sem_entrega` reduz duplicação; tamanho/caminho introduzem uma premissa de frescor que a foto não comprova. (`bin/disjuntor.py:420`, `bin/disjuntor.py:1595`, `bin/disjuntor.py:1645`) |
| Selo/RED sem mordida | Aparecem lacunas na trilha FALHA e no limite de releituras; o teste de concorrência permanece temporal. (`bin/tests/test_disjuntor.sh:1983`, `bin/tests/test_disjuntor.sh:1936`, `bin/tests/test_disjuntor.sh:1854`) |
| Ordem de trava | O sentinel libera a trava antes da releitura; não identifiquei inversão nova nesse laço. (`bin/disjuntor.py:330`, `bin/disjuntor.py:1623`, `bin/disjuntor.py:1625`) |
| Pouso antes do pré-requisito | Pouso, ordem dos commits e execução dos pré-requisitos: [nao verificado]. O contrato exige revisão antes da construção e o instrumento tem pouso próprio. (`app/docs/CORTES.md:393`, `app/docs/LEIS.md:131`) |

As citações nominais encontradas para `hook_stop_fila1.py` são L-008, L-081, L-096 e o corte OBRA-INTERROMPIDA-VOLTA-SOZINHA. O corte DISJUNTOR-TRES-DEFINICOES trata também do mecanismo. (`app/docs/LEIS.md:66`, `app/docs/LEIS.md:108`, `app/docs/LEIS.md:114`, `app/docs/CORTES.md:79`, `app/docs/CORTES.md:407`)

Citação nominal adicional dos outros três arquivos, ou desses quatro em `DOSSIES.md`: não encontrada [nao verificado]. L-002/L-007 estabelecem autoridade e escritor únicos; L-116 proíbe reescrever seção antiga; L-105 separa pouso de instrumento. (`app/docs/LEIS.md:60`, `app/docs/LEIS.md:65`, `app/docs/LEIS.md:145`, `app/docs/LEIS.md:131`)

## 01/11 12:08 · sessao c5641cc3840c · - · - · origem=Ronald (tmux)

Achados de testes, sem rodar suítes nem escrever arquivos: [nao verificado]

1. **`Event` não elimina a corrida do teste de trava.** A thread sinaliza e dorme 2,5 s; não espera a outra chamada efetivamente tentar a trava (`bin/tests/test_disjuntor.sh:1853-1858`). Se a thread principal demorar mais de aproximadamente 1,5 s até `_com_estado`, o mutante que mantém a leitura sob trava pode liberar antes de vencer o prazo de cerca de 1 s (`bin/tests/test_disjuntor.sh:1872-1878`; `bin/disjuntor.py:294-301`). O caso passa, o mutante sobrevive e a suíte acusa falha dependente do escalonamento (`bin/tests/test_disjuntor.sh:2023`, `2350-2352`, `2362-2363`). Um segundo evento de liberação tornaria a interposição determinística.

2. **Cobertura incompleta da trilha no Stop.** `v5_stop_b1_naosei` exige os dois recados, mas não exige a linha `FALHA` que o diff promete (`bin/tests/test_disjuntor.sh:1982-1985`; `bin/hook_stop_fila1.py:1060`). Remover apenas essa chamada de trilha passa nesse caso.

3. **O limite de releituras não está coberto.** A recuperação exige exatamente duas fotos, mas o ramo de foto sempre velha exige somente a mensagem; negar imediatamente com a mensagem esperada passaria essa segunda parte (`bin/tests/test_disjuntor.sh:1931-1936`). O código promete quatro fotos no total (`bin/disjuntor.py:1622-1630`).

4. **O teste B1 compra a premissa do relógio.** Ele escreve artificialmente `checado=agora+5` e exige descartar a medida que dispararia; não exercita início das chamadas versus instante real da leitura, igualdade de timestamps ou retrocesso do relógio (`bin/tests/test_disjuntor.sh:1949-1961`). Logo não prova que timestamp maior significa foto mais nova.

5. **Regex: comportamento pedido e limites.** Casam `- 14:30`, `- 14:3x`, `- **14:30**` e `- 14:30 h`; `**14:30**` sem `- ` não casa, como já ocorria. `- 14:30h` deixa de casar. Também casam prefixos de `- 14:30ms`, `- 14:30abc`, `- 14:3xyz` e `- 14:30H`: a guarda não estabelece fim do token (`bin/tests/test_hora_da_secao_nao_e_futura.sh:39`). Esses exemplos foram conferidos com Python puro, sem arquivos.

6. **Os novos controles positivos melhoram a mordida.** A porta precisa retornar `None` e deixar reserva; B1 precisa produzir tupla com booleano, além de começar a medição sinalizada (`bin/tests/test_disjuntor.sh:1856-1859`, `1882-1886`). `v5_foto_velha` verifica explicitamente autorização inicial, poda e recusa por seis vivos; não passa só por silêncio (`bin/tests/test_disjuntor.sh:1915-1932`). `v5_reabre_ausente` exige rc 4, mensagem, ausência de traceback e de recriação, seguido de controle com estado presente (`bin/tests/test_disjuntor.sh:1967-1975`).

7. **O mutante de hora agora rejeita mortes acidentais.** Exigir exatamente rc 0 impede considerar erro de execução como prova de que o ramo foi desligado (`bin/tests/test_hora_da_secao_nao_e_futura.sh:121-123`). O minuto exato e a duração possuem resultados esperados explícitos (`bin/tests/test_hora_da_secao_nao_e_futura.sh:105-109`, `125-128`). Ainda há dependência de relógio e data de corte: antes de `DESDE_LINHA`, linhas futuras são ignoradas e esses controles falham (`bin/tests/test_hora_da_secao_nao_e_futura.sh:38`, `67`, `95-109`); execução nessa condição é [nao verificado].

8. **Limite geral dos mutantes do disjuntor.** O harness considera qualquer retorno não zero como morte, sem distinguir assertiva relevante de erro de execução (`bin/tests/test_disjuntor.sh:2008`, `2023`). Há controles das cópias sem mutação e validação de âncora única, que reduzem mortes por cópia incompleta ou mutação ausente (`bin/tests/test_disjuntor.sh:1997-2006`, `2014-2021`, `2026`), mas não eliminam essa limitação. Não encontrei morte necessariamente alheia à costura nos mutantes novos; execução concreta é [nao verificado].

_1399 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
