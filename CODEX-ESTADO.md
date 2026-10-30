# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 14:53** · publicado em **30/10/2026 14:54** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 14:53 · sessao ddeaeb928a6b · - · - · origem=Ronald (tmux)

- Item 2: a cópia não contém a declaração nova de `motivo_do_veto` no registro, mas já contém o leitor `furos_vetados_por_regua` em `app/escala/management/commands/desfazer_corroboracao_regua.py:38`. A desatualização específica da linha LEI-AKITA nos hashes das voltas depende do anexo; anterioridade exata [nao verificado]. B/1c cita esse leitor, mas não denuncia a declaração desatualizada; isso sustenta, no máximo, EM_PARTE [nao verificado].
- Item 3: o cartório já consulta o juiz sem filtrar `ec=None` e inclui seu resultado na impressão (`app/ponto/services/cartorio.py:1013`, `app/ponto/services/cartorio.py:1083`). O juiz da cópia ainda devolve `None` para ausência de vínculo (`app/ponto/services/furos_vetados.py:26`). O teste anterior cria vínculo e só compara impressão com `veto=None`/`VETO_REGUA` (`app/ponto/tests/test_veto_que_cai_rejulga.py:42`, `app/ponto/tests/test_veto_que_cai_rejulga.py:115`). B/1c explica o efeito na impressão, sem apontar falta de teste próprio ou mutante: EM_PARTE [nao verificado quanto à classificação textual].
- Item 5: a cópia ainda não possui `recusado_veto`; o retorno `_ch is None` incrementa `recusado_celula` (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:169`). Portanto o defeito novo descrito no gabarito não pode ser retroprojetado somente desta cópia; a data de introdução por volta é [nao verificado].
- Item 7: a exigência de mostrar o veto ao humano e registrar motivo já está documentada em `app/docs/BACKLOG.md:785` e `app/docs/PROMPTS.md:1659`. As notas humanas antigas registram resgate, competência ou justificativa administrativa, sem o veto (`app/chamados/views_arquivar.py:79`, `app/chamados/services/competencia_trancada.py:259`, `app/chamados/services/acoes_disputa.py:270`). A e B/3b discutem autorização e notas/trilha, mas não apontam ausência de exibição/registro do veto ultrapassado: EM_PARTE é defensável pelo tema explícito [nao verificado quanto à classificação textual].
- Fonte documental útil para item 6: reversão antes e prova depois já constam em `app/docs/BACKLOG.md:785`; o estado-base já documenta impacto da impressão e de `furos_sem_cobranca.medir` em `app/docs/RELATO.md:9`.

## 30/10 14:53 · sessao 946742f8d813 · - · - · origem=Ronald (tmux)

A cópia disponível diverge do estado descrito para `61ae629c`: `motivo_do_veto(..., ec=None, ...)` ainda retorna `None` em `app/ponto/services/furos_vetados.py:26–27`; o arquivo termina em `:58` e ainda usa `ec.pk` diretamente em `:56`. A identificação histórica desta árvore como base anterior é [nao verificado].

- **s1:** A e B não identificam a quebra das fixtures em 45 módulos [nao verificado quanto à contagem, fornecida somente no gabarito]. Ambos censam leitores de produção; nenhum descreve fixtures sem vínculo recebendo `None`. Sugestão: **NAO_VIU / NAO_VIU**.
- **s2:** Os testes citados existem. `app/chamados/tests/test_resposta_tardia.py:24–29` cria colaborador sem vínculo; `:61–75` exige renascimento e declaração. `app/chamados/tests/test_c2_le_o_protesto_que_cobra.py:27–34` também não cria vínculo; `:68–72` exige emissão e destino. A/B discutem efeitos nos dois serviços, mas não identificam esses testes incompatíveis. Sugestão: **EM_PARTE / EM_PARTE** se “toca o tema” abranger os efeitos funcionais; nenhum merece VIU.
- **s3:** A/B não apontam ausência de inscrição `@bolha`. O selo exige ausência de novas bolhas sem contrato em `app/core/tests/test_selo_mapa_contrato.py:41–49`. O registro de `furos_vetados` não está no `core/juizes.py` desta cópia; existência no hash da volta vem somente do anexo [nao verificado no código disponível]. Sugestão: **NAO_VIU / NAO_VIU**.
- **m1:** B aponta precisamente impressão, rejulgamento e mudança do passivo/c2: “impressão passa a conter `veto:VETO_DEGENERADA` [...] provoca rejulgamento”; “passa de canal de cobrança para `veto`; sai do total”. O mecanismo preexistente é confirmado por `app/ponto/services/cartorio.py:329–330`, `:1013`, `:1083`, `:1092`; `app/ponto/services/furos_sem_cobranca.py:96–109`, `:125–126`; `app/chamados/reconciliador.py:854–857`. **B=VIU**, sem exigir que tivesse os números da medida. A lista efeitos de emissão/c2, mas não impressão/cartório/passivo: **A=EM_PARTE**, se contar esses efeitos como contato com o tema; caso critério estrito sobre a regressão compartilhada, NAO_VIU.

Os números `2.736`, `234 × 1.964`, `31 failures`, `163 errors`, `201 labels` e `46 módulos` não são conferíveis no código lido; só constam do dado fornecido [nao verificado].

## 30/10 14:53 · sessao 68fb51d5b847 · - · - · origem=Ronald (tmux)

O anexo A2 ausente impede confirmar quais candidatos abaixo são novos em relação ao gabarito completo; a correspondência com A2 itens 2, 3, 5 e 7 é `[nao verificado]`.

Candidatos fora de s1/s2/s3/m1, usando as referências citadas nas respostas:

| Revisor | Achado reportado | Referência citada |
|---|---|---|
| A/B | Grade reabre chamado após recusa de renascimento pela porta | `app/ponto/management/commands/reconciliar_grade.py:522` |
| A/B | Callback de resolução reabre pai automaticamente sem veto | `app/chamados/services/disputa_emissao.py:1606` |
| A/B | `entrada_bloqueada_turno_aberto` nasce sem dia e escapa do veto | `app/ponto/views.py:291` |
| A/B | Marco crítico cobra após renascimento recusado, porque só testa `None` | `app/ponto/management/commands/processar_alertas_turno.py:317` |
| A/B | Reabertura humana não apresenta nem registra motivo do veto | `app/chamados/views_arquivar.py:75` |
| B | Vigia inclui reabertura humana permitida entre violações | `app/ponto/services/furos_vetados.py:98` |
| B | Contador publica um vínculo para achados cujo vínculo é `None` | `app/ponto/management/commands/furos_vetados_por_regua.py:32` |
| B | AST não reconhece alias transitivo de classe | `app/chamados/tests/test_o261_veto_na_porta.py:100` |
| B | AST não reconhece import relativo de módulo | `app/chamados/tests/test_o261_veto_na_porta.py:92` |
| B | AST não reconhece construtor seguido de `save` | `app/chamados/tests/test_o261_veto_na_porta.py:170` |
| B | AST não reconhece `bulk_create` por alias de manager | `app/chamados/tests/test_o261_veto_na_porta.py:168` |
| B | AST não reconhece `update(**valores)` | `app/chamados/tests/test_o261_veto_na_porta.py:143` |
| B | AST não reconhece pedido à porta por alias local | `app/chamados/tests/test_o261_veto_na_porta.py:163` |
| B | AST não reconhece reabridores automáticos por instância | `app/chamados/tests/test_o261_veto_na_porta.py:170` |
| B | Selo complementar também deixa passar construtor/`save` e `update(**valores)` | `app/chamados/tests/test_contract_status_nucleo.py:7` |
| B | Testes não exercitam integração da porta com vínculo ausente | `app/chamados/tests/test_o261_veto_na_porta.py:266` |
| B | Testes não distinguem exclusão de `batida_ausente` e `intervalo_pendente` do universo | `app/chamados/tests/test_o261_veto_na_porta.py:269` |

A cópia atual confirma os sítios de grade, callback, nascimento sem dia, marco crítico e trilha humana, mas não contém a implementação nova do veto: `motivo_do_veto` ainda retorna `None` para vínculo ausente (`app/ponto/services/furos_vetados.py:26`). Os trechos do AST, do novo vigia e dos novos testes não puderam ser conferidos nesta cópia `[nao verificado]`.

Na cópia atual, os sítios correspondentes são: grade `app/ponto/management/commands/reconciliar_grade.py:519`; booleano ignorado `app/chamados/models.py:686`; callback `app/chamados/services/disputa_emissao.py:1606`; contexto sem dia `app/ponto/views.py:280`; marco crítico `app/ponto/management/commands/processar_alertas_turno.py:317`; trilha humana `app/chamados/views_arquivar.py:75`, `app/chamados/services/acoes_disputa.py:270` e `app/chamados/services/competencia_trancada.py:261`.

## 30/10 14:51 · sessao 388a9c07ec1a · BENCH-O261-VETO-A2 · verificador · origem=Code

**1. Achados 1–5 do Opus**

| Achado | Veredito e mudança | Mutante plausível que permanece |
|---|---|---|
| **1 — RED contra a base** | **Cura não comprovada.** O diff fornecido não traz nova execução contra a base; RED histórico continua **[nao verificado]**. Os testes de nascimento e reabertura existem, mas sua existência não prova aquela execução (`app/chamados/tests/test_o261_veto_na_porta.py:418`, `app/chamados/tests/test_o261_veto_na_porta.py:522`). | Não há teste novo dirigido a esse achado no diff fornecido **[nao verificado quanto ao histórico]**. |
| **2 — declaração LEI-AKITA** | **Cura não comprovada.** A declaração corrigida não foi fornecida **[nao verificado]**. Os elementos que ela deveria nomear existem: `recusa_da_reabertura`, chamada por `reabrir`, e o consumidor `desfazer_corroboracao_regua` (`app/chamados/models.py:1071`, `app/chamados/models.py:1143`, `app/escala/management/commands/desfazer_corroboracao_regua.py:38`). | Não há teste novo dirigido à completude dessa declaração **[nao verificado]**. |
| **3 — impressão com `ec=None`** | **Curado no escopo apontado.** O novo teste observa a chamada real, exige ausência de veto com vínculo e `VETO_DEGENERADA` depois de encerrá-lo (`app/ponto/tests/test_veto_que_cai_rejulga.py:124`, `app/ponto/tests/test_veto_que_cai_rejulga.py:129`, `app/ponto/tests/test_veto_que_cai_rejulga.py:131`). | Fazer o hash **ignorar especificamente `VETO_DEGENERADA`**: o spy ainda recebe o argumento esperado, pois o teste só inspeciona `kwargs`. O teste vizinho de hash cobre `VETO_REGUA` (`app/ponto/services/cartorio.py:329`, `app/ponto/tests/test_veto_que_cai_rejulga.py:126`, `app/ponto/tests/test_veto_que_cai_rejulga.py:138`). |
| **4 — helper 5x2** | **Curado para o calendário do helper.** Agora declara trabalho nos sete dias; o cálculo semanal respeita essa lista (`app/chamados/tests/_vinculo_do_dia.py:18`, `app/escala/models.py:830`). Isso não certifica toda a suíte contra dependência de calendário **[nao verificado]**. | Não foi acrescentado teste próprio percorrendo os sete dias no diff fornecido **[nao verificado quanto à suíte]**. Retirar domingo da lista é um mutante plausível cuja detecção dependeria dos dias exercitados; sobrevivência **[nao verificado]**. |
| **5 — veto por eliminação** | **Curado.** O contador de veto exige resposta positiva de `veto_no_nascimento`; ausência das duas respostas vai para `recusado_sem_juiz` (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:179`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:184`). | Inverter a prioridade quando **célula e veto recusam simultaneamente**: os testes cobrem motivos isolados. Também passa pelo novo MORDE retirar a mensagem `[RECUSADO sem juiz]`, preservando os contadores que ele verifica (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:113`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:124`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:142`). |

As sobrevivências acima são inferências das asserções citadas; execução dos mutantes e resultado da suíte completa: **[nao verificado]**. [nao verificado]

**2. Efeito do helper nas fixtures**

**Sim, muda comportamento de calendário.** Sábado e domingo deixam de cair no `weekday() < 5` do 5x2 e passam a trabalho no semanal 0–6. Criação ou mudança de chamado pode disparar julgamento do dia, portanto o efeito pode alcançar grade, ata e emissão; chamado novo ainda depende dos demais predicados do cartório (`app/escala/models.py:861`, `app/escala/models.py:830`, `app/chamados/signals.py:164`, `app/chamados/signals.py:180`, `app/ponto/services/cartorio.py:1339`).

Casos concretos que merecem distinção:

| Módulo | Dependência e efeito |
|---|---|
| `app/chamados/tests/test_agrupado_em.py:17` | Volta a **01/08/2026**, sábado. A fixture precisa de premissa viva para reabrir quando o pai fecha; há controle positivo e verificação de unicidade, portanto não ficou apenas num negativo vazio (`app/chamados/tests/test_agrupado_em.py:39`, `app/chamados/tests/test_agrupado_em.py:40`, `app/chamados/tests/test_agrupado_em.py:45`). |
| `app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:52` | Usa **05/09/2026**, sábado, e recebe o helper. Entretanto, `premissa_regularizada` está patchada para `False`: esse teste não prova a classificação real de folga/trabalho (`app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:42`, `app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:68`). |
| `app/ponto/tests/test_justificativa_encerra_origem.py:101` | O lote inclui **25/10/2026**, domingo, com `turno_aberto`; muda o calendário de apoio. A asserção exige resolução dos chamados após aprovação (`app/ponto/tests/test_justificativa_encerra_origem.py:93`, `app/ponto/tests/test_justificativa_encerra_origem.py:102`). |
| `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:196` | **Folga explícita preservada:** impõe `trabalha=False` e ata de folga; também cobre folga com ata velha e o oposto, trabalho com furo. A célula tem prioridade sobre o template (`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:199`, `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:214`, `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:219`, `app/escala/models.py:1071`). |

Módulos com **data do fato relativa**, sujeitos à troca do calendário de apoio quando ela cai no fim de semana:

| Caminho | Referências |
|---|---|
| `app/ponto/tests/` | `test_a2_marco_morto.py:31`; `test_bug115_lampada_do_marco.py:54`; `test_bug114_fechador_de_dois_dias.py:73` |
| `app/api/tests/` | `test_chamados_v2_destino.py:37` |
| `app/chamados/tests/` | `test_orfa_sem_candidato.py:37`; `test_3c_abrir_ausencia.py:36`; `test_e5b_cobrar_massa.py:24`; `test_f3_dedup_atraso.py:55`; `test_selo_b5h_followup_motivo.py:68`; `test_validar_classe_a.py:45`; `test_reconciliar_pergunta_orfa.py:27`; `test_fila_por_causa.py:111`; `test_b8_smoke_reabertura.py:36`; `test_selo_i6_ouve_a_lei.py:63`; `test_resposta_tardia.py:20`; `test_emissor_nomeia_o_marco.py:44`; `test_selo_f4_fatia4.py:65`; `test_selo_dedup_premissa_viva.py:90`; `test_bug125_cobranca_cega.py:56`; `test_selo_porta_recusa.py:63`; `test_regressao_etapa7.py:79` |

Nessa lista, alguns declaram trabalho diretamente na célula, reduzindo a dependência do template: `app/chamados/tests/test_3c_abrir_ausencia.py:43`, `app/chamados/tests/test_f3_dedup_atraso.py:44`, `app/chamados/tests/test_orfa_sem_candidato.py:41`, `app/chamados/tests/test_emissor_nomeia_o_marco.py:48`, `app/chamados/tests/test_resposta_tardia.py:34`.

O caso “sem escala” de `test_selo_f4_fatia4` também permanece separado: consulta diretamente o colaborador sem passar pelo método que aplica o helper (`app/chamados/tests/test_selo_f4_fatia4.py:67`, `app/chamados/tests/test_selo_f4_fatia4.py:131`, `app/chamados/tests/test_selo_f4_fatia4.py:139`).

**Sem achado demonstrado de folga intencional convertida em verde vazio.** Quantidades efetivamente criadas ou ocultadas de furos, chamados e previstos, antes/depois: **[nao verificado]**. A afirmação do anexo sobre “27 módulos com data relativa” não fica certificada por este inventário **[nao verificado]**. [nao verificado]

**3. O spy intercepta?**

**Sim; sem achado.** A chamada usa o nome global `impressao_insumos`, definido no próprio módulo. Esse nome é resolvido no dicionário de `cartorio`, justamente onde `patch.object` coloca o spy; não há uma referência local previamente capturada nessa chamada (`app/ponto/services/cartorio.py:314`, `app/ponto/services/cartorio.py:829`, `app/ponto/services/cartorio.py:1083`, `app/ponto/tests/test_veto_que_cai_rejulga.py:124`).

O import local de `julgar_colab` no teste não captura `impressao_insumos`: ele chama a função que despacha o corpo do julgamento (`app/ponto/tests/test_veto_que_cai_rejulga.py:83`, `app/ponto/services/cartorio.py:744`).

**4. Ordem e alcance do MORDE**

**Sem achado na ordem implementada:** no nascimento, `abrir` pergunta célula e depois veto; o command repete essa prioridade. `sem_juiz` é uma classificação posterior do command, não uma terceira guarda de `abrir` (`app/chamados/models.py:726`, `app/chamados/models.py:731`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:184`).

**O MORDE exercita o ramo certo do command.** A fixture tem vínculo e célula acusadora; somente `abrir` é substituído por `(None, False)`, permitindo alcançar “nenhum juiz assume” (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:50`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:73`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:140`).

Ele não exercita internamente `abrir`, nem demonstra a mudança concorrente do dia narrada na docstring; essa ocorrência real é **[nao verificado]** (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:136`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:140`).

**5. Consumidores da linha `fim:`**

**Sem achado de quebra nos consumidores identificados da cópia.** Os testes leem substrings e toleram o campo acrescentado (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:118`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:131`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:142`).

O command está declarado fora do pipeline, por ato, sem horário; o envelope de cron produz JSON próprio a partir de início/fim/exit, sem interpretar essa linha de stdout (`app/config/crons.py:743`, `app/config/crons.py:875`, `app/config/crons.py:884`, `bin/cron_run.sh:11`, `bin/cron_run.sh:13`). O placar lê esse JSON, e a regex de `placar_code.sh` procura `crons_quebrados=`, não `fim:` (`app/core/management/commands/pipeline_placar.py:65`, `app/core/services/crons_quebrados.py:35`, `bin/placar_code.sh:453`).

Cron instalado, scripts externos e regex fora da cópia: **[nao verificado]**.

## 30/10 14:50 · sessao 0628f7e0436d · - · - · origem=Ronald (tmux)

Q2 — mudança real na fixture, mas **sem achado de regressão concreta demonstrada**; execução da suíte e comparação de quantidades de furos/chamados/previstos: **[nao verificado]**. [nao verificado]

O helper declara SEMANAL 0..6 em `app/chamados/tests/_vinculo_do_dia.py:18`. Isso muda sábado/domingo de folga para trabalho no cálculo do template: SEMANAL consulta `weekday in dias` em `app/escala/models.py:830`, enquanto 5x2 sem configuração retorna `weekday < 5` em `app/escala/models.py:861`. Não é só “dar vínculo”: pode mudar a grade/ata quando há julgamento disparado pela criação/mudança de chamado (`app/chamados/signals.py:164`, `app/chamados/signals.py:180`). Não implica necessariamente furo/chamado novo: emissão do cartório tem outros predicados e exige dia de trabalho (`app/ponto/services/cartorio.py:1339`, `app/ponto/services/cartorio.py:1348`).

Casos fixos relevantes:

- `test_agrupado_em`: volta para 01/08/2026; exige que pai fechado libere reabertura, portanto quer premissa viva/trabalho e não folga. Controle não é só negativo: exige filho existente e único e reabertura positiva (`app/chamados/tests/test_agrupado_em.py:17`, `:39`, `:40`, `:45`).
- `test_selo_i4b_um_chamado_por_turno`: fixa 05/09/2026 e dá vínculo quando não há escala (`app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:52`, `:68`). Porém a premissa está patchada para `False`, então não prova classificação de folga/trabalho (`:42`). Seu teste do emissor usa vínculo próprio 12x36 e célula explícita, fora do helper (`:219`, `:224`).
- `test_justificativa_encerra_origem`: segundo caso do lote usa 25/10/2026 e cria chamado `turno_aberto`; muda o calendário de apoio, mas asserção principal exige ambos resolvidos após aprovar (`app/ponto/tests/test_justificativa_encerra_origem.py:101`, `:102`, `:93`).

**Folga explícita preservada:** `test_reconciliar_pergunta_a_premissa` impõe `trabalha=False`/ata folga e exige fechamento (`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:196`, `:199`, `:201`); testa também folga com ata velha (`:212`, `:214`, `:215`) e o oposto, trabalho com furo recusando (`:217`, `:219`). O leitor da escala prioriza `cel.trabalha` sobre template (`app/escala/models.py:1071`). Não identifiquei caso que esperasse folga apenas por cair no fim de semana e agora ficasse verde vacuamente **[nao verificado quanto à execução]**.

**Inventário dos 21 módulos com data do fato explicitamente relativa**, todos sujeitos à troca do calendário de apoio caso a data caia no fim de semana; isto não significa que cada um tenha resultado final alterado:

- `app/ponto/tests/test_a2_marco_morto.py:31`
- `app/ponto/tests/test_bug115_lampada_do_marco.py:54`
- `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:73`
- `app/api/tests/test_chamados_v2_destino.py:37`
- `app/chamados/tests/test_orfa_sem_candidato.py:37`
- `app/chamados/tests/test_3c_abrir_ausencia.py:36`
- `app/chamados/tests/test_e5b_cobrar_massa.py:24`
- `app/chamados/tests/test_f3_dedup_atraso.py:55`
- `app/chamados/tests/test_selo_b5h_followup_motivo.py:68`
- `app/chamados/tests/test_validar_classe_a.py:45`
- `app/chamados/tests/test_reconciliar_pergunta_orfa.py:27`
- `app/chamados/tests/test_fila_por_causa.py:111`
- `app/chamados/tests/test_b8_smoke_reabertura.py:36`
- `app/chamados/tests/test_selo_i6_ouve_a_lei.py:63`
- `app/chamados/tests/test_resposta_tardia.py:20`
- `app/chamados/tests/test_emissor_nomeia_o_marco.py:44` (também `:126`)
- `app/chamados/tests/test_selo_f4_fatia4.py:65`
- `app/chamados/tests/test_selo_dedup_premissa_viva.py:90`
- `app/chamados/tests/test_bug125_cobranca_cega.py:56`
- `app/chamados/tests/test_selo_porta_recusa.py:63`
- `app/chamados/tests/test_regressao_etapa7.py:79` (também `:107`).

Distinções que evitam chamar todo consumidor de regressão:

- Trabalho declarado por célula: `test_3c_abrir_ausencia.py:43`, `test_f3_dedup_atraso.py:44`, `test_orfa_sem_candidato.py:41`, `test_emissor_nomeia_o_marco.py:48`, `test_resposta_tardia.py:34` (todos sob `app/chamados/tests/`). Esses testes não dependem exclusivamente do template para declarar trabalho. [nao verificado]
- `test_a2_marco_morto` preserva indefinição explicitamente (`app/ponto/tests/test_a2_marco_morto.py:40`) e tem controle positivo de furo vivo (`:56`, `:68`).
- `test_selo_f4_fatia4` só aplica helper ao construir chamado e se não houver escala (`app/chamados/tests/test_selo_f4_fatia4.py:67`). O teste que declara “sem escala” e espera `procedencia_fina == ambiguo` não chama esse método: cria colaborador sem escala, registra batidas retratadas e consulta diretamente (`:131`, `:136`, `:139`). Portanto não foi ocultado por este helper.
- `test_selo_dedup_premissa_viva` exige chamada criada e estado vivo, não só ausência de erro (`app/chamados/tests/test_selo_dedup_premissa_viva.py:95`, `:98`, `:119`).
- `test_regressao_etapa7` exige exatamente um chamado e distintos para dias diferentes (`app/chamados/tests/test_regressao_etapa7.py:94`, `:115`).

Censo bruto por import encontrou 45 módulos; 27 contêm alguma referência a relógio (`timezone.now`, `localdate` ou `today`), mas só 21 acima têm data do fato relativa explicitamente identificada. Esse censo é resultado de busca, sem arquivo de relatório gravado: **[nao verificado por artefato citável]**. Evite repetir “27 módulos com data relativa a hoje” como comprovado pela cópia.

## 30/10 14:50 · sessao d8ac4c6e0eb6 · BENCH-O261-VETO-A2 · verificador · origem=Code

1. **A cobertura não é absoluta: resta um escape condicionado a instância desatualizada.** O callback exigido pelo aval agora passa pelo veto: chama `reabrir()` sem exceção humana, e a nova guarda retorna antes de gravar estado ou ressuscitar perguntas. Nesse caminho, **sem achado**. (`app/chamados/services/disputa_emissao.py:1606`; `app/chamados/models.py:1090`; `app/chamados/models.py:1143`; `app/chamados/models.py:1185`.)

   Porém há cadeia automática `detectar_ausencias → fábrica de disputa → analisar() → _gravar_estado()`. Se o chamado foi carregado como `aberto`, outro escritor o encerra e o dia está vetado quando `analisar()` prossegue, este decide pelo estado antigo da instância e grava `em_analise`, sem consultar o veto nem conferir o estado atual do banco. É um contraexemplo estrutural concorrente, não uma ocorrência demonstrada em produção. (`app/ponto/management/commands/detectar_ausencias.py:790`; `app/chamados/services/disputa_emissao.py:981`; `app/chamados/models.py:569`; `app/chamados/models.py:192`; `app/chamados/models.py:231`.)

   `transicionar()` também permite encerrado→vivo sem esse veto, mas os chamadores de reabertura encontrados recebem ação humana e `request.user`; não encontrei escape automático por esse caminho. Sem dia identificável, a própria guarda retorna `None`. (`app/chamados/catalogo/motor.py:102`; `app/chamados/services/acoes_chamado.py:313`; `app/chamados/services/fio.py:708`; `app/chamados/models.py:892`.)

2. **sem achado nos três `humano=True` pela cadeia atual da cópia.**

   - **Desarquivar:** login, POST, permissão `gerir_chamados` e verificação de área antecedem a chamada. (`app/chamados/views_arquivar.py:51`; `app/chamados/views_arquivar.py:54`; `app/chamados/views_arquivar.py:60`; `app/chamados/views_arquivar.py:68`.)
   - **Disputa administrativa:** o fio verifica `pode_agir_chamado`; a ação exige staff/admin_ti, motivo e justificativa, usando `request.user`. (`app/chamados/services/fio.py:560`; `app/chamados/services/fio.py:577`; `app/chamados/services/acoes_disputa.py:242`; `app/chamados/services/acoes_disputa.py:256`; `app/chamados/services/acoes_disputa.py:266`.)
   - **Competência:** login, POST e permissão `reabrir_periodo`; `request.user` segue como `por` e depois `autor`. (`app/ponto/views.py:1520`; `app/ponto/views.py:1525`; `app/ponto/views.py:1544`; `app/ponto/services/fechamento.py:1416`; `app/chamados/services/competencia_trancada.py:258`.)

   Não localizei chamada automática que alcance esses três pontos. Ressalva: o serviço de competência aceita `autor=None` e substitui por usuário sistema; a garantia humana está na cadeia encontrada, não no serviço isolado. (`app/chamados/services/competencia_trancada.py:228`; `app/chamados/services/competencia_trancada.py:239`.) Chamadas externas/dinâmicas em produção: **[nao verificado]**.

3. **sem achado nos leitores de `recusa_da_reabertura`.** `reabrir()` trata `'veto_do_dia'` antes do ramo que usa o segundo elemento como irmão. `reabrir_orfas_enterradas` apenas atribui `_irmao`; testa `motivo`, imprime a recusa e executa `continue`, sem acessar `.pk` do veto. A prioridade nova impede também chegar ao redirecionamento para irmão quando há veto. (`app/chamados/models.py:1143`; `app/chamados/models.py:1164`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:105`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:107`.)

4. **Há contraexemplos na deduplicação.**

   - **Duplicação concorrente:** duas chamadas de `criar()` ou `reabrir()` podem ler ausência da mesma linha e ambas inserir. A consulta não está protegida por trava/unicidade; o `atomic` de `evento()` cobre a inserção, não a decisão anterior. (`app/chamados/models.py:925`; `app/chamados/models.py:937`; `app/core/models.py:309`; `app/core/observ.py:133`.)
   - **Linha extra por outro dia:** recusa em D → nascimento permitido em D2, mesmo colaborador/módulo → nova recusa idêntica em D. `_passou` não filtra dia/célula, embora a chave da recusa os distinga. (`app/chamados/models.py:925`; `app/chamados/models.py:933`.)
   - **Volta de veto calada:** A→B→A, sem nascimento/reabertura intermediária. A última tentativa encontra a linha antiga de A e retorna sem registrar sua volta. Isso contraria “enquanto nada muda” se mudança de motivo contar como ciclo; pela definição restrita de “passou depois”, é uma limitação deliberada do algoritmo. (`app/chamados/models.py:909`; `app/chamados/models.py:925`; `app/chamados/models.py:935`; `app/ponto/services/furos_vetados.py:41`; `app/ponto/services/furos_vetados.py:43`.)

   **Custo da função:** primeira recusa, 1 SELECT + 1 INSERT; repetição suprimida, 2 SELECTs; nova linha após `_passou`, 2 SELECTs + 1 INSERT, além do controle transacional e eventual carregamento do usuário. Quantidade constante de consultas não garante custo constante: os índices declarados não cobrem a chave JSON e a ordenação. (`app/chamados/models.py:925`; `app/chamados/models.py:931`; `app/chamados/models.py:937`; `app/core/observ.py:110`; `app/core/observ.py:133`; `app/core/models.py:313`.) Latência/plano real: **[nao verificado]**.

5. **sem achado de novo chamado, nota ou pergunta introduzido pelo delta.** O ramo novo registra `LogAuditoria` e retorna `False`. As perguntas/notas dos atos humanos já aparecem como contexto inalterado no diff fornecido; nesses pontos, a alteração é passar `humano=True`. (`app/chamados/models.py:1144`; `app/core/observ.py:134`; `app/chamados/services/acoes_disputa.py:259`; `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/competencia_trancada.py:258`; `app/chamados/views_arquivar.py:68`.)

Ocorrência e frequência em produção dos contraexemplos, assim como resultados de execução de testes: **[nao verificado]**. [nao verificado]

## 30/10 14:49 · sessao ef47481153aa · - · - · origem=Ronald (tmux)

Cadeia automática concreta até o escritor que escapa de `reabrir()`:

1. Cron `detectar_ausencias`, a cada cinco minutos: `app/config/crons.py:345`.
2. O comando materializa `_orfaos` a partir de chamados em `COBRANCA`, sem `select_for_update`, e chama a fábrica: `app/ponto/management/commands/detectar_ausencias.py:790`, `app/ponto/management/commands/detectar_ausencias.py:796`.
3. A fábrica admite `batida_ausente` de dia passado com marcos nomeados: `app/chamados/services/disputa_emissao.py:666`, `app/chamados/services/disputa_emissao.py:680`, `app/chamados/services/disputa_emissao.py:693`. A guarda de encerramento lê a instância recebida, sem recarga: `app/chamados/services/disputa_emissao.py:691`.
4. O ramo `_por_ata` termina em `ch.analisar()`: `app/chamados/services/disputa_emissao.py:969`, `app/chamados/services/disputa_emissao.py:978`, `app/chamados/services/disputa_emissao.py:981`.
5. `analisar()` decide pelo `self.status_local` antigo e chama `_gravar_estado`; não relê o banco nem consulta veto: `app/chamados/models.py:566`, `app/chamados/models.py:569`, `app/chamados/models.py:572`.
6. O motor aceita `aberto→em_analise`, e o escritor grava esse estado na linha existente: `app/chamados/catalogo/motor.py:112`, `app/chamados/models.py:192`, `app/chamados/models.py:231`.

**Contraexemplo condicional demonstrável pela estrutura:** depois de a fábrica terminar suas consultas e antes de `analisar()`, outra execução encerra o chamado e o dia fica vetado. A primeira execução conserva `status_local='aberto'`, passa por `ANALISAR` e grava `em_analise` sobre o encerrado, sem `recusa_da_reabertura`. Não há recarga/guarda no trecho entre a chamada e a escrita: `app/chamados/services/disputa_emissao.py:981`, `app/chamados/models.py:566`, `app/chamados/models.py:185`. Ocorrência em produção: [nao verificado].

A ata não substitui o veto: `_ata_do_dia` só bloqueia ausência de célula com vínculo, e devolve os marcos faltantes: `app/chamados/services/disputa_emissao.py:825`, `app/chamados/services/disputa_emissao.py:833`, `app/chamados/services/disputa_emissao.py:844`. Seu leitor usa lâmpadas `missing`, sem perguntar `motivo_do_veto`: `app/chamados/services/declaracao_texto.py:79`, `app/chamados/services/declaracao_texto.py:107`; a grade lê a ata persistida e deriva `missing` da lâmpada: `app/escala/services/leitor_celula.py:325`, `app/escala/services/leitor_celula.py:354`, `app/escala/services/leitor_celula.py:97`.

## 30/10 14:49 · sessao 91766570d59a · BENCH-O261-VETO-A2 · verificador · origem=Code

1. **Não: o selo não cobre toda reabertura.** Ele reconhece apenas chamadas cujo alvo AST é um atributo chamado `reabrir`, dentro dos corpos examinados; não segue fluxo de dados nem chamadas indiretas. Além disso, registra funções, não chamadas individuais: acrescentar outra reabertura numa função já classificada não altera o inventário. (`app/chamados/tests/test_o261_veto_na_porta.py:251`, `app/chamados/tests/test_o261_veto_na_porta.py:275`)

   Mutante plausível: **acrescentar** uma função `automatico(ch)` com `retomar = ch.reabrir; retomar()`. Ela escapa porque a chamada é um `ast.Name`; `getattr(ch, "reabrir")()` também escapa. A execução isolada dos varredores, em memória, devolveu listas vazias para ambos. São mutantes propostos, não ocorrências de produção demonstradas. (`app/chamados/tests/test_o261_veto_na_porta.py:231`, `app/chamados/tests/test_o261_veto_na_porta.py:256`; ocorrência em produção: [não verificado])

   **Existe hoje um caminho concreto fora desse censo:** `acao_resposta_livre` recebe `novo_status` e chama `transicionar`; a matriz permite `resolvido → em_analise`. O caminho termina em `_aplicar → _gravar_estado`, sem chamar `.reabrir`. É reabertura humana não enumerada pelo selo. (`app/chamados/services/acoes_chamado.py:287`, `app/chamados/services/acoes_chamado.py:315`, `app/chamados/catalogo/motor.py:102`, `app/chamados/models.py:183`, `app/chamados/models.py:260`)

   Outro mutante que escapa é `ch._gravar_estado("em_analise")` numa função externa: o escritor aceita o estado e limpa os carimbos de encerramento; o contrato complementar procura atribuição/update de `status_local`, não chamadas ao escritor. (`app/chamados/models.py:185`, `app/chamados/models.py:192`, `app/chamados/models.py:212`, `app/chamados/tests/test_contract_status_nucleo.py:7`, `app/chamados/tests/test_contract_status_nucleo.py:23`)

2. **Classificação dos sítios literais: sem achado de classe incorreta nos caminhos examinados.** Isso não transforma a classificação em garantia de origem humana para futuros chamadores. (`app/chamados/tests/test_o261_veto_na_porta.py:62`, `app/chamados/tests/test_o261_veto_na_porta.py:310`)

   - **`competencia_trancada.reabrir`: humano no caminho atual.** A view exige POST, permissão e motivo; `reabrir_periodo` chama a reabertura da fila com o autor. O processamento em lote decorre desse gesto humano. O serviço aceitar `autor=None` e recorrer ao usuário sistema não demonstra automação atual. (`app/ponto/views.py:1520`, `app/ponto/views.py:1536`, `app/ponto/views.py:1544`, `app/ponto/services/fechamento.py:1415`, `app/chamados/services/competencia_trancada.py:239`)
   - **`acao_reabrir_disputa_admin`: humano, mas há ressalva funcional.** Valida usuário, motivo e justificativa; porém reabre a disputa e cria pergunta antes de chamar `ch.reabrir()`, ignorando seu retorno. Contraexemplo: chamado fechado por admin sem fato novo pode continuar encerrado enquanto a disputa é reaberta e a ação retorna sucesso. Isso não é erro de classificação, mas impede equiparar “ação humana executada” a “ChamadoColaborador reaberto”. (`app/chamados/services/acoes_disputa.py:242`, `app/chamados/services/acoes_disputa.py:256`, `app/chamados/services/acoes_disputa.py:259`, `app/chamados/services/acoes_disputa.py:266`, `app/chamados/services/acoes_disputa.py:295`, `app/chamados/models.py:1122`)
   - **Porta, desarquivar e outro modelo: sem achado.** O renascer consulta o veto; desarquivar é ação autorizada da tela; `reabrir_colaborador` opera sobre fechamento mensal. (`app/chamados/models.py:780`, `app/chamados/views_arquivar.py:54`, `app/ponto/views.py:1442`, `app/ponto/views.py:1453`)
   - **As seis exceções continuam reabrindo diretamente.** Nomeá-las documenta a dívida; não elimina o desvio do veto. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:161`, `app/chamados/services/adesao.py:92`, `app/chamados/services/disputa_emissao.py:1606`, `app/ponto/management/commands/detectar_ausencias.py:301`, `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`, `app/ponto/management/commands/reconciliar_grade.py:522`)

3. **Nova chave da trilha: sem achado de quebra nos leitores/testes examinados.** `via` integra agora a deduplicação; `abrir` e `criar` podem produzir linhas distintas para o mesmo objeto/dia/veto. O teste que exige uma linha repete somente `abrir`, mantendo a mesma chave. A tela de auditoria conta os registros encontrados, sem impor unicidade por dia. (`app/chamados/models.py:923`, `app/chamados/models.py:930`, `app/chamados/tests/test_o261_veto_na_porta.py:430`, `app/chamados/tests/test_o261_veto_na_porta.py:437`, `app/core/views.py:727`)

   Execução da suíte Django e leitores externos à cópia: [não verificado]. [nao verificado]

4. **Não demonstraria bloqueio obrigatório do pouso A apenas pelos itens FILA; bloquearia a alegação de cobertura total pelo achado 1.** O código declara exceções para B, e o inventário atual não garante que novas reaberturas sejam detectadas. (`app/chamados/tests/test_o261_veto_na_porta.py:69`, `app/chamados/tests/test_o261_veto_na_porta.py:256`)

   - **Item 6 bloqueia usar o placar como prova desse vigia.** Contraexemplo: chamado `turno_aberto_24h` vivo em dia vetado, sem célula com veredito `furo`. O contador novo acusa; o antigo pode dar zero; o placar extrai apenas o antigo. Exigência de integrar esse contador ao placar já em A: [não verificado]. (`app/ponto/services/furos_vetados.py:61`, `app/ponto/services/furos_vetados.py:95`, `app/ponto/services/furos_vetados.py:106`, `bin/placar_code.sh:209`)
   - **Item 5 permanece defeito de trilha.** Veto V → correção → renascimento → encerramento → retorno de V: a busca histórica encontra a primeira recusa e não registra o novo ciclo. Entretanto, a reabertura continua recusada; esse contraexemplo não demonstra falha do bloqueio. (`app/chamados/models.py:923`, `app/chamados/models.py:780`)
   - **Item 7 exige corrigir a generalização da triagem.** O detector pula `ec=None`, mas `_emitir_furo` da grade aceita `ec=None` e chama a defesa de cadastro. Não está demonstrado que ambos sempre pulem antes dela, nem que A tenha causado essa lacuna. (`app/ponto/management/commands/detectar_ausencias.py:139`, `app/ponto/management/commands/reconciliar_grade.py:480`, `app/ponto/management/commands/reconciliar_grade.py:487`; regressão: [não verificado])
   - **Itens 4 e 8: sem achado bloqueante adicional.** O ramo `nunca_bateu` retorna antes da emissão; os dois testes citados verificam contexto, portanto não sustentam prova de nascimento. (`app/ponto/services/cartorio.py:1003`, `app/chamados/tests/test_emissor_nomeia_o_marco.py:88`, `app/chamados/tests/test_bug125_cobranca_cega.py:176`)

   **O item 9 continua sem resultado da medição exigida no anexo.** O comando pode rejulgar células passadas quando a impressão muda, mas quantidade atingida e efeito em produção não foram fornecidos. (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1083`; medição e impacto: [não verificado])

## 30/10 14:49 · sessao 0ce7e05a0463 · - · - · origem=Ronald (tmux)

4. **Sem achado na ordem implementada.** No nascimento de `abrir`, célula precede veto: `app/chamados/models.py:726`, `app/chamados/models.py:731`; os dois devolvem `(None, False)` (`:729`, `:734`). O command consulta os mesmos predicados públicos, célula → veto → fallback sem juiz (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174`, `:179`, `:184`); esses wrappers delegam aos predicados da porta (`app/chamados/models.py:874`, `:904`). `sem_juiz` é classificação posterior do command, não terceira causa de recusa implementada em `abrir` (`app/chamados/models.py:726`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:184`).

O MORDE exercita o fallback certo do command: a fixture lavra célula acusadora (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:73`), possui vínculo (`:50`), e substitui somente `abrir` por `(None, False)` (`:140`); a saída exigida é veto zero e sem juiz um (`:142`). Não testa a execução interna da porta nem demonstra a corrida narrada na docstring; o retorno é imposto pelo mock (`:136`, `:140`).

**Achado 5 curado no código, com lacuna de teste:** o veto deixou de ser inferido por eliminação e exige resposta positiva do juiz (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:179`). Mutante plausível sobrevivente aos novos testes: inverter a prioridade célula/veto **quando ambos recusam**. Os casos novos cobrem veto isolado e nenhum juiz (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:124`, `:135`); o caso anterior de célula usa a régua sadia da fixture (`:41`, `:113`). Outro mutante simples: retirar a linha `[RECUSADO sem juiz]` mantendo o contador; o novo MORDE verifica apenas os dois contadores (`:142`), embora o código imprima a linha (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:188`). Sobrevivência efetiva em execução: [nao verificado].

5. **Sem achado de quebra nos consumidores identificados da cópia.** A extensão é apensa ao fim da linha (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:192`). Os consumidores diretos encontrados são asserts por substring, tolerantes ao campo extra (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:118`, `:131`, `:142`, `:172`, `:182`).

O command consta em `FORA_DE_PIPELINE` (`app/config/crons.py:743`, `:875`), declarado por ato, nunca por horário (`:884`). O envelope cron executa o comando, captura seu exit e gera JSON próprio, sem interpretar stdout (`bin/cron_run.sh:11`). O placar pipeline lê esse JSON e usa `ini`, `nome`, `emp`, `exit`, `dur` (`app/core/management/commands/pipeline_placar.py:65`). O vigia também lê JSON e exit (`app/core/services/crons_quebrados.py:30`, `:46`); a regex do placar Code lê `crons_quebrados=`, não `fim:` (`bin/placar_code.sh:453`). Consumidor externo/cron instalado/regex fora da cópia: [nao verificado].

## 30/10 14:49 · sessao 3512f8b1456e · - · - · origem=Ronald (tmux)

Achado 3: **curado no escopo apontado; sem achado no caminho do spy**. O teste observa chamadas reais, exige `{None}` no controle, encerra o vínculo antes do dia e exige `{'VETO_DEGENERADA'}` depois; lista vazia também falha nessa última igualdade (`app/ponto/tests/test_veto_que_cai_rejulga.py:123–131`). O código pergunta ao juiz mesmo com vínculo ausente (`app/ponto/services/cartorio.py:1013`), e o juiz transforma template ausente em `VETO_DEGENERADA` (`app/ponto/services/furos_vetados.py:40–41`; `app/escala/regua_defesa.py:23–24`). O mutante citado pelo Opus — condicionar a pergunta a `ec is not None` — contradiz a asserção final (`app/ponto/tests/test_veto_que_cai_rejulga.py:131`); confirmação executada do vermelho: [nao verificado].

Pergunta 3: **o spy intercepta**. `impressao_insumos` é função global do módulo (`app/ponto/services/cartorio.py:314`), chamada pelo nome global dentro de `_julgar_colab_corpo` (`app/ponto/services/cartorio.py:1083`), sem parâmetro homônimo na assinatura (`app/ponto/services/cartorio.py:829`). Esse nome é resolvido no dicionário do módulo que `patch.object(cartorio, ...)` substitui (`app/ponto/tests/test_veto_que_cai_rejulga.py:124`). O import local de `julgar_colab` no teste não captura `impressao_insumos`; chama a função que despacha `_julgar_colab_corpo` (`app/ponto/tests/test_veto_que_cai_rejulga.py:83–86`; `app/ponto/services/cartorio.py:744`).

**Mutante plausível sobrevivente ao novo teste:** alterar a composição do hash para ignorar especificamente `VETO_DEGENERADA`, por exemplo trocando `if veto:` por `if veto and veto != 'VETO_DEGENERADA':`. O spy continuaria recebendo o argumento esperado, pois o teste verifica somente `kwargs['veto']`, não o hash ou carimbo (`app/ponto/services/cartorio.py:329–330`; `app/ponto/tests/test_veto_que_cai_rejulga.py:126–131`). O teste de hash vizinho verifica diferença apenas para `VETO_REGUA` (`app/ponto/tests/test_veto_que_cai_rejulga.py:135–139`). Sobrevivência executada do mutante ou na suíte completa: [nao verificado].

## 30/10 14:48 · sessao 26974b772216 · - · - · origem=Ronald (tmux)

3. **Sem achado nos leitores.** `reabrir()` trata `'veto_do_dia'` antes de usar o segundo elemento como irmão; `reabrir_orfas_enterradas` apenas atribui `_irmao`, testa `motivo`, imprime a recusa e pula a pergunta. Não há acesso a `.pk` do veto nesse comando. (`app/chamados/models.py:1143`, `app/chamados/models.py:1164`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `:107`, `:111`.)

4. **Contraexemplos de deduplicação:**

- **Linha extra por nascimento de outro dia:** recusa em D → nasce chamado do mesmo colaborador/módulo em D2 → nova tentativa idêntica em D produz outra linha. `_passou` não filtra dia nem célula, embora a chave da recusa contenha dia/objeto. (`app/chamados/models.py:925`, `:933`, `:937`.)
- **Volta de veto calada:** A → B → A, sem nascimento/reabertura intermediária. Na última tentativa, a busca encontra a linha antiga de A, ignorando B; `_passou=False` encerra sem registrar a volta de A. Isso contraria uma leitura literal de “enquanto nada muda/ciclo novo de veto”; se o contrato exigir exclusivamente passagem intermediária, é limitação da definição adotada. Existem dois motivos concretos no juiz: `VETO_DEGENERADA` e `VETO_REGUA`. (`app/chamados/models.py:909`, `:925`, `:935`; `app/ponto/services/furos_vetados.py:41`, `:43`.)
- **Concorrência:** duas chamadas de `criar()` ou `reabrir()` podem consultar a mesma ausência de linha e inserir ambas; a deduplicação não trava nem tem restrição única no log. O `atomic` de `evento()` envolve somente a inserção. (`app/chamados/models.py:818`, `:925`, `:937`, `:1115`; `app/core/models.py:309`; `app/core/observ.py:133`.)
- **Indicador excessivamente amplo em reabertura:** atualizar SLA altera `atualizado_em` sem reabrir o chamado; a próxima recusa registra outra linha. É compatível com “objeto mudou”, mas não prova que a porta “deixou passar”. (`app/chamados/models.py:931`, `:383`.)

**Custo específico de `_trilha_do_veto`:** primeira ocorrência: 1 SELECT + 1 INSERT; repetição suprimida: 2 SELECTs; repetição após `_passou`: 2 SELECTs + 1 INSERT, além do controle transacional e possível carregamento do usuário da requisição. A consulta ordena por tempo/PK e filtra campos JSON; os índices declarados do log não cobrem essa chave/ordenação. Plano real e latência: **[nao verificado]**. (`app/chamados/models.py:925`, `:931`, `:933`, `:937`; `app/core/observ.py:110`, `:133`; `app/core/models.py:313`.)

Detalhe menor: `via='reabrir'` ganha ação correta `reabertura_recusada`, mas descrição textual “**nascimento recusado (reabrir)**”, pois somente `'renascer'` seleciona o outro texto. (`app/chamados/models.py:919`, `:938`.)

Execução em produção e incidência desses cenários: **[nao verificado]**.

## 30/10 14:48 · sessao ef47481153aa · - · - · origem=Ronald (tmux)

Q1 — **sem achado de escape automático efetivo nos caminhos encontrados**, considerando chamado com dia identificável pelo juiz. O veto fica na primeira guarda de `recusa_da_reabertura`, e `reabrir` devolve `False` antes de gravar estado/perguntas: `app/chamados/models.py:1090`, `app/chamados/models.py:1143`, `app/chamados/models.py:1185`.

- O callback automático exigido pelo aval continua passando por `reabrir()` sem `humano=True`; sua nota depende do retorno verdadeiro: `app/chamados/services/disputa_emissao.py:1598`, `app/chamados/services/disputa_emissao.py:1606`, `app/chamados/services/disputa_emissao.py:1608`. A cadeia automática parte do receiver `post_save`, com usuário sistema: `app/chamados/signals.py:75`, `app/chamados/signals.py:81`, `app/chamados/signals.py:92`.
- Outros chamadores diretos também usam o default automático: adesão (`app/chamados/services/adesao.py:92`), grade (`app/ponto/management/commands/reconciliar_grade.py:522`), detector (`app/ponto/management/commands/detectar_ausencias.py:301`), ausência aberta (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`), órfãs (`app/chamados/management/commands/reabrir_orfas_enterradas.py:161`). Dedup chama `_renascer_por_premissa_viva` e esta já pergunta o veto antes de `reabrir`: `app/chamados/models.py:685`, `app/chamados/models.py:779`.
- `transicionar` **permite** encerrado→vivo sem o veto: matriz `resolvido→em_analise` e exceção `admin_ti`, seguida de `_aplicar→_gravar_estado`: `app/chamados/catalogo/motor.py:102`, `app/chamados/catalogo/motor.py:150`, `app/chamados/models.py:183`, `app/chamados/models.py:260`. Os chamadores produtivos encontrados que aceitam destino vivo vêm de POST humano e levam `request.user`: `app/chamados/services/acoes_chamado.py:286`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:498`, `app/chamados/services/fio.py:708`, `app/chamados/services/fio.py:715`.
- O comando smoke também transiciona automaticamente, mas recusa banco sem `"test"` e usa módulo `"smoke"`, fora do universo: `app/chamados/management/commands/smoke_chamados.py:31`, `app/chamados/management/commands/smoke_chamados.py:126`, `app/chamados/management/commands/smoke_chamados.py:147`.
- `_gravar_estado` em si não contém veto (`app/chamados/models.py:185`); os outros gates automáticos de estado produzem encerramento/cancelamento/superação, ou `ANALISAR` só aceita origens `aberto`/`registrado`: `app/chamados/catalogo/motor.py:111`. Não encontrei update produtivo direto que reabra; a atribuição de estado fica em `app/chamados/models.py:192`, e o update auxiliar só grava urgência/carimbo: `app/chamados/models.py:424`.

Limites que não devem virar garantia absoluta:

- Sem dia forte, `_veto_da_regua` retorna `None`; logo não prova cobertura de um chamado cujo dia real esteja vetado, mas não seja identificável pelo código: `app/chamados/models.py:892`. Existência desse caso em produção: [nao verificado].
- `analisar()` decide pela instância sem recarregar o estado do banco e chama o escritor sem veto: `app/chamados/models.py:566`. Há callers automáticos (`app/chamados/services/disputa_emissao.py:981`, `app/ponto/services/regularizacao.py:137`), então uma instância antiga ainda `aberto` pode, por interleaving com encerramento, escrever `em_analise` sobre o banco encerrado: `app/chamados/models.py:192`, `app/chamados/models.py:231`. É contraexemplo estático sob concorrência/instância desatualizada; ocorrência em produção e alteração causada pelo delta: [nao verificado].

## 30/10 14:48 · sessao c73a7789541d · BENCH-O261-VETO-A · verificador · origem=Code

**Reprovo o pouso A:** há reabertura automática fora do veto, nascimento que dispensa o veto por falta de dia e mutantes que o selo não detecta. (`app/ponto/management/commands/reconciliar_grade.py:522`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/models.py:893`; `app/chamados/tests/test_o261_veto_na_porta.py:168`)

Os contraexemplos abaixo são entradas construídas a partir do código. Ocorrência e quantidade em produção, DIFF de frota, execução Django e RED anterior: **[não verificado]**.

**1. A distinção “`reabrir` direto = humano” é falsa; há automáticos que escapam.**

- **Grade desfaz a recusa da própria porta.** `abrir` devolve o existente encerrado mesmo quando `_renascer_por_premissa_viva` recusa; `_emitir_furo` verifica apenas `None` e depois chama `ch.reabrir()`. **Entrada → comportamento:** chamado `batida_ausente` resolvido, premissa viva, vínculo recebido pelo emissor saudável, mas vínculo consultado pela porta vetado → renascimento recusado e registrado; em seguida a grade reabre diretamente. O novo tratamento de `None` não cobre esse retorno. (`app/chamados/models.py:685`; `app/chamados/models.py:778`; `app/ponto/management/commands/reconciliar_grade.py:487`; `app/ponto/management/commands/reconciliar_grade.py:519`)

- **O signal também reabre automaticamente sem veto.** Ao salvar um destino encerrado, o receiver chama a resolução das perguntas; ela reabre o pai quando há resposta aguardando consumo, sem restringir módulo nem consultar o veto. **Entrada → comportamento:** disputa aberta, pai `entrada_bloqueada_turno_aberto` resolvido com dia vetado, resposta não validada/materializada creditada ao pai, sem outras recusas de reabertura → salvar o destino encerrado torna o pai vivo. (`app/chamados/signals.py:75`; `app/chamados/signals.py:92`; `app/chamados/juizes.py:1471`; `app/chamados/services/disputa_emissao.py:1598`; `app/chamados/services/disputa_emissao.py:1606`)

Esses caminhos contradizem a justificativa introduzida no diff de que chamadas diretas representam atos humanos. (`app/chamados/models.py:775`)

**2. Um nascimento automático do universo passa sem consultar o juiz.**

`bater_ponto` cria `entrada_bloqueada_turno_aberto` com somente `pendentes` e `total`; a porta retorna “sem veto” quando não consegue obter dia forte. **Entrada → comportamento:** tentativa de entrada com turno anterior pendente, nenhum chamado vivo correspondente e régua vetada → nasce chamado e segue push, porque o contexto não informa o dia. Ele passa nominalmente por `criar`, mas escapa da invariante; o contador também o separa em `sem_dia`, sem acusá-lo. (`app/ponto/views.py:275`; `app/ponto/views.py:280`; `app/ponto/views.py:291`; `app/ponto/views.py:305`; `app/chamados/models.py:892`; `app/ponto/services/furos_vetados.py:93`)

O teste transforma essa lacuna em comportamento esperado, embora o universo inclua esse módulo. (`app/chamados/tests/test_o261_veto_na_porta.py:313`; `app/chamados/catalogo/furo_modulos.py:15`)

**3. Recusar renascimento ainda pode produzir cobrança ligada ao chamado morto.**

`_marco_critico` testa somente se recebeu `None`; `_abrir_chamado_critico` devolve o objeto encerrado retornado pelo dedup. **Entrada → comportamento:** `turno_aberto` resolvido, premissa viva, régua suspeita e nenhum alerta `pos_20` → a porta recusa renascimento, mas o chamador prossegue com alerta e justificativa automática apontando o encerrado. Antes, satisfeitas as demais guardas, esse objeto renascia. (`app/chamados/models.py:685`; `app/ponto/management/commands/processar_alertas_turno.py:316`; `app/ponto/management/commands/processar_alertas_turno.py:387`; `app/ponto/management/commands/processar_alertas_turno.py:391`)

**4. A exceção humana não cumpre aviso/trilha do veto, e o vigia a acusa como violação.**

Desarquivar pode reabrir diretamente e registra apenas o resgate genérico; não consulta nem devolve o motivo do veto. **Entrada → comportamento:** chamado encerrado pela tranca, dia suspeito, demais guardas satisfeitas → admin desarquiva, recebe `ok`, chamado fica vivo, sem aviso/trilha específica do veto; o novo contador acusa um achado contra esperado zero. (`app/chamados/views_arquivar.py:64`; `app/chamados/views_arquivar.py:75`; `app/chamados/views_arquivar.py:80`; `app/ponto/services/furos_vetados.py:98`)

A própria implementação admite adiar essa trilha para B e contar atos humanos enquanto isso. Logo, o contador não certifica isoladamente a invariante **automática** aprovada. (`app/ponto/services/furos_vetados.py:82`; `app/docs/CORTES.md:401`)

Não há achado demonstrado de bloqueio pelo formulário manual atual: o catálogo não oferece os módulos desse universo nessa abertura. A API genérica `criar`, porém, não possui distinção humano/automático. (`app/chamados/catalogo/modulos.py:105`; `app/chamados/catalogo/modulos.py:182`; `app/chamados/catalogo/modulos.py:283`; `app/chamados/services/abertura.py:149`; `app/chamados/models.py:838`)

**5. “DIFF zero por construção” não se sustenta, inclusive com vínculo presente.**

**Entrada → comportamento:** vínculo válido, horários utilizáveis, régua suspeita, turno N5 aberto e nenhuma célula absolvendo → `processar_alertas_avancados` antes alcançava o nascimento de `turno_aberto_24h`; agora `criar` retorna `None` e o emissor para. As guardas anteriores de vínculo ausente não demonstram equivalência para régua suspeita. (`app/ponto/management/commands/processar_alertas_avancados.py:37`; `app/ponto/management/commands/processar_alertas_avancados.py:94`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/ponto/management/commands/processar_alertas_avancados.py:141`; `app/chamados/models.py:838`)

Além disso, alterar `ec=None` muda leitores antigos fora da porta; o veto foi autorizado, mas esses efeitos precisam entrar no censo e na avaliação de equivalência. (`app/ponto/services/furos_vetados.py:32`; `app/docs/CORTES.md:401`)

**(a) Censo dos leitores afetados de `motivo_do_veto`:**

| Leitor | Entrada → mudança |
|---|---|
| `_veto_da_regua`, novo | Módulo do universo, dia forte e veto → recusa nascimento/renascimento. `app/chamados/models.py:896` |
| `reemitir_chamado_do_dia` | `ec=None`, demais guardas satisfeitas e sem vivo para costurar → agora retorna `False` antes de abrir. `app/chamados/reconciliador.py:852` |
| `furos_vetados_por_regua` | Furo sem vínculo → antes omitido; agora achado com `vinculo=None`. `app/ponto/services/furos_vetados.py:57`; `app/ponto/services/furos_vetados.py:61` |
| `_julgar_colab_corpo._veto_do_dia` | Célula já julgada, sem vínculo, demais insumos iguais → acrescenta `veto:VETO_DEGENERADA` à impressão e deixa de satisfazer o salto por impressão igual. `app/ponto/services/cartorio.py:1013`; `app/ponto/services/cartorio.py:329`; `app/ponto/services/cartorio.py:1092` |
| `furos_sem_cobranca.medir` | Furo cobrável, sem vínculo, sem vivo e demais exclusões falsas → passa de porta de cobrança para `veto`, sai do total e é pulado por `cobrar`. `app/ponto/services/furos_sem_cobranca.py:96`; `app/ponto/services/furos_sem_cobranca.py:109`; `app/ponto/services/furos_sem_cobranca.py:125` |
| `chamados_vivos_em_dia_vetado`, novo | Vivo com dia e sem vínculo → entra nos achados. `app/ponto/services/furos_vetados.py:98` |

Os efeitos transitivos alcançam os comandos de medir/cobrar passivo e os pontos de entrada do cartório. (`app/ponto/management/commands/furos_sem_cobranca_viva.py:14`; `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:19`; `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:30`; `app/ponto/services/cartorio.py:709`; `app/ponto/services/cartorio.py:1560`)

Há ainda um erro concreto no consumidor dos achados: **dois furos sem nenhum vínculo real → `{None}` → `furos_vetados_por_regua_vinculos=1`**. O ajuste para aceitar ausência de vínculo não ajustou sua contagem. (`app/ponto/services/furos_vetados.py:61`; `app/ponto/management/commands/furos_vetados_por_regua.py:32`)

**Censo dos chamadores de `abrir`/`criar`/`_renascer_por_premissa_viva` cujo resultado pode mudar:**

| Chamador | Efeito do novo veto |
|---|---|
| `detectar_ausencias._abrir_chamado` | Se o vínculo usado pela porta divergir do recebido pelo emissor: nascimento retorna `VETO_NA_PORTA`; dedup encerrado pode retornar `VETO_ENCERRADO_NAO_RENASCEU`. `app/ponto/management/commands/detectar_ausencias.py:688`; `app/ponto/management/commands/detectar_ausencias.py:729` |
| `reconciliar_grade._emitir_furo` | Nascimento pode retornar `VETO_NA_PORTA`; renascimento recusado pode ser desfeito pelo `reabrir` direto. `app/ponto/management/commands/reconciliar_grade.py:499`; `app/ponto/management/commands/reconciliar_grade.py:519` |
| `reemitir_chamado_do_dia` | Recusa sem costurar a pergunta; também recebe a mudança direta de `motivo_do_veto`. `app/chamados/reconciliador.py:854`; `app/chamados/reconciliador.py:862`; `app/chamados/reconciliador.py:885` |
| `processar_alertas_turno._abrir_chamado_critico` | Novo nascimento pode devolver `None`; dedup pode devolver encerrado, com o efeito colateral descrito acima. `app/ponto/management/commands/processar_alertas_turno.py:335`; `app/ponto/management/commands/processar_alertas_turno.py:316` |
| `processar_alertas_avancados.handle` | Recusa novo `turno_aberto_24h` e pula os efeitos seguintes. `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/ponto/management/commands/processar_alertas_avancados.py:141` |
| `emitir_turno_aberto_retroativo.handle` | Dry passa a anunciar veto; apply pode deixar de emitir e contar recusa. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174` |
| `ChamadoColaborador.abrir`, dedup | Continua devolvendo existente/`False`, mas o existente pode permanecer encerrado devido ao veto novo. `app/chamados/models.py:685`; `app/chamados/models.py:778` |
| `resposta_tardia.receber` | Célula ainda acusa, mas veto impede renascimento → resposta vira nota tardia e, havendo texto livre, pauta DP. `app/chamados/services/resposta_tardia.py:45`; `app/chamados/services/resposta_tardia.py:52`; `app/chamados/services/resposta_tardia.py:58` |

`bater_ponto` pertence ao universo, mas permanece sem mudança justamente pela fuga sem dia descrita no achado 2. (`app/ponto/views.py:280`; `app/chamados/models.py:893`)

**(b) Os escapes automáticos são os dois `reabrir` diretos do achado 1 e o nascimento sem dia do achado 2.** O reverso da migration 0038 está explicitamente autorizado na allowlist; sua permanência, isoladamente, não reprova A. (`app/ponto/management/commands/reconciliar_grade.py:522`; `app/chamados/services/disputa_emissao.py:1606`; `app/ponto/views.py:291`; `app/chamados/tests/test_o261_veto_na_porta.py:40`)

**(c) O selo AST deixa passar mutantes plausíveis.**

| Forma dentro de uma função | Resultado |
|---|---|
| `from chamados.models import ChamadoColaborador as Ch; Ch.objects.create(...)` | Detecta: **sem achado nessa forma**. `app/chamados/tests/test_o261_veto_na_porta.py:92` |
| Import anterior, seguido de `C = Ch; C.objects.create(...)` | Escapa: não propaga esse alias. `app/chamados/tests/test_o261_veto_na_porta.py:97` |
| `from .models import ChamadoColaborador as Ch` e criação direta | Detecta: **sem achado nessa forma relativa**. `app/chamados/tests/test_o261_veto_na_porta.py:92` |
| `from . import models as m; m.ChamadoColaborador.objects.create(...)` | Escapa: raiz `m` não reconhecida como modelo. `app/chamados/tests/test_o261_veto_na_porta.py:168` |
| `ch = ChamadoColaborador(...); ch.save()` | Escapa: construtor e `save` não são tratados como nascimento. `app/chamados/tests/test_o261_veto_na_porta.py:28`; `app/chamados/tests/test_o261_veto_na_porta.py:170` |
| `ChamadoColaborador.objects.bulk_create([...])` | Detecta: **sem achado na chamada direta**. `app/chamados/tests/test_o261_veto_na_porta.py:28` |
| `manager = ChamadoColaborador.objects; manager.bulk_create([...])` | Escapa pelo alias do manager. `app/chamados/tests/test_o261_veto_na_porta.py:97`; `app/chamados/tests/test_o261_veto_na_porta.py:168` |
| `campos = {'status_local': 'aberto'}; ...update(**campos)` | Escapa: procura argumentos/literais na chamada, não o conteúdo da variável. `app/chamados/tests/test_o261_veto_na_porta.py:137` |

**Contraexemplo comum:** dia vetado → construtor seguido de `save()` cria chamado com status padrão `aberto`, sem consultar a porta, enquanto o censo permanece igual. (`app/chamados/models.py:46`; `app/chamados/tests/test_o261_veto_na_porta.py:170`)

A catraca também não garante “só desce”: retirar um nome autorizado e acrescentar outro mantém tamanho ≤5; acrescentar outra escrita dentro de função autorizada desaparece no `set`. (`app/chamados/tests/test_o261_veto_na_porta.py:194`; `app/chamados/tests/test_o261_veto_na_porta.py:198`; `app/chamados/tests/test_o261_veto_na_porta.py:209`)

**(d) Há lacunas de mordida nos testes da porta, mas não em todos.** [nao verificado]

- **Sem vínculo não testa integração com a porta.** A fixture mantém vínculo válido; o teste chama somente o juiz. **Mutante → resultado:** `_veto_da_regua` retorna `None` quando o vínculo está ausente, mantendo `motivo_do_veto` correto → os nascimentos sem vínculo escapam, mas esse teste passa. (`app/chamados/tests/test_o261_veto_na_porta.py:266`; `app/chamados/tests/test_o261_veto_na_porta.py:302`; `app/chamados/models.py:896`)
- **`criar` não tem controle positivo correspondente.** **Mutante → resultado:** retornar sempre `None` para `turno_aberto_24h` → satisfaz o teste mesmo recusando também régua saudável. O outro caso de `criar` usa outro módulo. (`app/chamados/tests/test_o261_veto_na_porta.py:295`; `app/chamados/tests/test_o261_veto_na_porta.py:313`)
- **Renascimento: sem achado de vacuidade por premissa.** Existe controle positivo correspondente, que exige renascer sem veto. (`app/chamados/tests/test_o261_veto_na_porta.py:320`; `app/chamados/tests/test_o261_veto_na_porta.py:338`)
- **Contador: sem achado de lista sempre vazia.** O teste exige denominador e achado concreto; falta distinguir a exceção humana. (`app/chamados/tests/test_o261_veto_na_porta.py:345`; `app/ponto/services/furos_vetados.py:82`)

Quanto às demais classes: **sem achado adicional de juiz novo não autorizado**, pois o corte declara `motivo_do_veto` e A permite temporariamente as três cópias; **sem achado demonstrado de nova leitura de relógio ou inversão de trava** na cadeia adicionada. Há consultas adicionais de vínculo, mas custo excessivo no request e latência são **[não verificado]**. (`app/docs/CORTES.md:401`; `app/chamados/tests/test_o261_veto_na_porta.py:46`; `app/chamados/models.py:669`; `app/chamados/models.py:896`; `app/escala/servico_jornada.py:46`; `app/escala/servico_jornada.py:58`)

## 30/10 14:47 · sessao efc982d187ae · - · - · origem=Ronald (tmux)

A classificação dos sítios literais está coerente com os caminhos atuais; “humano” descreve a origem da ação, não uma garantia imposta pela assinatura do serviço. (`app/chamados/tests/test_o261_veto_na_porta.py:60`, `app/chamados/tests/test_o261_veto_na_porta.py:62`, `app/chamados/tests/test_o261_veto_na_porta.py:71`)

- **`competencia_trancada.reabrir`: sem achado de classificação.** A view exige POST, permissão `reabrir_periodo` e motivo; chama o serviço de fechamento, que chama `_reabrir_fila(..., autor=por)`. A reabertura em lote decorre desse ato humano. O serviço aceita `autor=None` e usa usuário sistema, portanto a função isolada não prova origem humana. (`app/ponto/views.py:1520`, `app/ponto/views.py:1523`, `app/ponto/views.py:1525`, `app/ponto/views.py:1536`, `app/ponto/views.py:1544`, `app/ponto/services/fechamento.py:1415`, `app/chamados/services/competencia_trancada.py:239`, `app/chamados/services/competencia_trancada.py:258`)

- **`acao_reabrir_disputa_admin`: sem achado de classificação humana**, com ressalva funcional: verifica staff/grupo, motivo e justificativa, mas reabre a disputa e cria pergunta antes de chamar `ch.reabrir()`, cujo retorno ignora. Um chamado encerrado por admin sem fato novo pode permanecer encerrado enquanto a ação cria pergunta, envia notificações e responde sucesso. Esse defeito já existe no código, mas não converte o sítio em automático. (`app/chamados/services/fio.py:575`, `app/chamados/services/acoes_disputa.py:242`, `app/chamados/services/acoes_disputa.py:245`, `app/chamados/services/acoes_disputa.py:256`, `app/chamados/services/acoes_disputa.py:259`, `app/chamados/services/acoes_disputa.py:266`, `app/chamados/models.py:1122`, `app/chamados/models.py:1128`, `app/chamados/services/acoes_disputa.py:273`, `app/chamados/services/acoes_disputa.py:295`)

- **Porta / desarquivar / outro modelo: sem achado.** O renascer consulta veto antes de `self.reabrir`; desarquivar exige POST e permissão; `ponto.views.reabrir_colaborador` passa um `FechamentoMensal` ao serviço que altera seus campos de aprovação. (`app/chamados/models.py:780`, `app/chamados/models.py:784`, `app/chamados/views_arquivar.py:54`, `app/chamados/views_arquivar.py:56`, `app/chamados/views_arquivar.py:68`, `app/ponto/views.py:1442`, `app/ponto/views.py:1453`, `app/ponto/services/fechamento.py:1317`)

- **Seis sítios da allowlist: sem achado de classificação como decisões programáticas sem consulta direta ao veto.** São reabertura de pai por pergunta fantasma, adesão, resposta aguardando, detector, ausência ainda aberta e grade persistente. Os comandos corretivos possuem `--apply`; sua execução real em produção não foi demonstrada pelo dado fornecido. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`, `app/chamados/services/adesao.py:89`, `app/chamados/services/adesao.py:92`, `app/chamados/services/disputa_emissao.py:1604`, `app/chamados/services/disputa_emissao.py:1606`, `app/ponto/management/commands/detectar_ausencias.py:300`, `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:36`, `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:44`, `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`, `app/ponto/management/commands/reconciliar_grade.py:519`; execução em produção: [não verificado])

**Achado adicional para a pergunta 1:** há reabertura real por outra porta que o censo `.reabrir` não representa: `acao_resposta_livre` aceita `novo_status` do POST e chama `transicionar`; `resolvido → em_analise` é permitido pela matriz, e admin pode reabrir fechado. `transicionar` termina em `_aplicar → _gravar_estado`, que grava estado vivo e limpa os carimbos. Portanto não é preciso imaginar apelido/getattr para demonstrar que o selo não enumera toda reabertura. (`app/chamados/services/acoes_chamado.py:287`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/acoes_chamado.py:315`, `app/chamados/catalogo/motor.py:102`, `app/chamados/catalogo/motor.py:150`, `app/chamados/catalogo/motor.py:156`, `app/chamados/models.py:183`, `app/chamados/models.py:260`, `app/chamados/models.py:192`, `app/chamados/models.py:212`)

Também existe esse caminho no POST do fio, condicionado à permissão por área. (`app/chamados/services/fio.py:703`, `app/chamados/services/fio.py:708`, `app/chamados/services/fio.py:713`, `app/chamados/services/fio.py:715`)

## 30/10 14:47 · sessao e72c4331ad49 · - · - · origem=Ronald (tmux)

- **Item 6: recomendo bloquear a alegação de vigia operacional do A.** O contador foi incluído no escopo do selo (`app/chamados/tests/test_o261_veto_na_porta.py:15`) e o comando imprime seu resultado (`app/ponto/management/commands/furos_vetados_por_regua.py:38`, `:52`), mas o placar extrai exclusivamente `furos_vetados_por_regua` (`bin/placar_code.sh:209`). Contraexemplo: chamado vivo `turno_aberto_24h`, dia vetado, sem célula com veredito `furo`: o contador novo acusa (`app/ponto/services/furos_vetados.py:95`, `:105`), o antigo não inclui (`:61`), e o placar pode mostrar zero. Obrigatoriedade de ligação ao placar **neste pouso**, contra a decisão de fila L-105 do anexo: [não verificado].

- **Item 4: defeito real, mas não demonstrei bloqueio do veto na porta.** O ramo sem adesão calcula impressão sem `veto` e pode pular (`app/ponto/services/cartorio.py:907`, `:941`, `:943`), mas termina antes da emissão (`:1003`); quando surge adesão, o ramo normal força rejulgamento de `nunca_bateu` (`:1089`, `:1092`). A simples transição “veto caiu” não fornece aqui contraexemplo de nascimento automático indevido. **Sem achado bloqueante para A**, nesses termos (`app/chamados/tests/test_o261_veto_na_porta.py:5`).

- **Item 7: a garantia “segue abrindo cadastro” não está provada.** Detector descarta `ec=None` antes do emissor (`app/ponto/management/commands/detectar_ausencias.py:139`), enquanto a criação de cadastro reside em `vetar_cobranca_se_degenerada` (`app/escala/regua_defesa.py:36`, `:51`) e o novo juiz é somente leitura (`app/ponto/services/furos_vetados.py:34`). Contraexemplo: ativo sem vínculo nas duas datas da passada é descartado e não chega à criação pelo detector (`app/ponto/management/commands/detectar_ausencias.py:136`). Isso sustenta a fila; regressão causada pelo delta, presença em produção ou obrigação de curá-lo no A: [não verificado]. **A triagem generaliza demais ao dizer “grade pula ec=None”:** `_coberto` devolve `False` nesse caso (`app/ponto/management/commands/reconciliar_grade.py:383`), e `_emitir_furo` recebe `ec=None` e chama a defesa (`:480`, `:487`). A grade pode excluir antes por outros critérios, mas não encontrei um `continue ec=None` equivalente ao detector.

- **Item 8: testes realmente não provam nascimento.** Ambos chamam somente `_ctx_c2` e verificam dicionário (`app/chamados/tests/test_emissor_nomeia_o_marco.py:88`, `:91`; `app/chamados/tests/test_bug125_cobranca_cega.py:176`, `:179`). Contraexemplo: fazer a porta recusar tudo não muda essas asserções. Isso bloqueia usar esses testes como evidência de nascimento; por si só não demonstra bug adicional que impeça A. (`app/chamados/tests/test_emissor_nomeia_o_marco.py:92`; `app/chamados/tests/test_bug125_cobranca_cega.py:182`.)

- **Item 9: risco real de relavratura, sem quantidade comprovada.** O comando seleciona todas as células passadas e passa `apply` ao cartório (`app/ponto/management/commands/processar_cartorio.py:49`, `:61`, `:65`); impressão alterada passa pelo teste de idempotência (`app/ponto/services/cartorio.py:1083`, `:1092`) e é gravada (`:1424`). Contraexemplo condicional: célula exportada, com adesão, cuja impressão antiga não contém o novo `VETO_DEGENERADA` de `ec=None`; o juiz atual acrescenta esse motivo (`app/ponto/services/furos_vetados.py:40`; `app/ponto/services/cartorio.py:329`) e a próxima passagem pode relavrar. Não assumir que exportada proíbe correção: o código registra explicitamente a lei contrária (`app/ponto/services/cartorio.py:1252`). Medida da sombra exigida pelo próprio anexo, resultado e impacto financeiro: [não verificado].

## 30/10 14:47 · sessao cf5bdee8d8a6 · - · - · origem=Ronald (tmux)

2. **Sem achado na cadeia atual:** os três `humano=True` são alcançados por atos HTTP autenticados e autorizados:

- **Desarquivar:** `login_required`, permissão `gerir_chamados`, POST e verificação de área antes de `humano=True`. (`app/chamados/views_arquivar.py:51`, `:54`, `:56`, `:60`, `:68`)
- **Reabrir disputa administrativa:** fio/modal exigem login e gestor; o processamento verifica `pode_agir_chamado`; a ação exige staff/admin_ti, motivo válido e justificativa, e passa `request.user` à reabertura administrativa. (`app/chamados/views.py:658`, `:662`, `:667`, `:868`, `:872`, `:951`; `app/chamados/services/fio.py:560`, `:575`; `app/chamados/services/acoes_disputa.py:242`, `:245`, `:250`, `:256`, `:266`)
- **Reabrir competência:** view exige login, POST, permissão `reabrir_periodo` e motivo; propaga `request.user` como `por`, depois `autor`, até o serviço que usa `humano=True`. (`app/ponto/views.py:1520`, `:1523`, `:1525`, `:1536`, `:1544`; `app/ponto/services/fechamento.py:1415`; `app/chamados/services/competencia_trancada.py:258`)

Não localizei chamada automática a esses três pontos na cópia; os chamadores produtivos identificados são a rota de desarquivar, o processamento do fio e o serviço de reabertura do período. (`app/chamados/urls.py:27`; `app/chamados/services/fio.py:577`; `app/ponto/services/fechamento.py:1416`) Ressalva estrutural: o serviço da competência aceita `autor=None` e usa usuário sistema; portanto a autorização está na cadeia atual, não dentro dele. (`app/chamados/services/competencia_trancada.py:228`, `:239`) Uso externo/dinâmico em produção: [nao verificado].

5. **Sem achado no delta fornecido:** o ramo novo apenas registra a recusa e retorna `False`; a trilha escreve `LogAuditoria`. (`app/chamados/models.py:1143`, `:1145`, `:1146`, `:937`; `app/core/observ.py:134`) A pergunta e a nota da reabertura administrativa, a nota da competência e a nota do desarquivamento já constam como contexto inalterado no diff fornecido; nos três locais a alteração é adicionar `humano=True`. (`app/chamados/services/acoes_disputa.py:259`, `:266`, `:270`; `app/chamados/services/competencia_trancada.py:258`, `:259`; `app/chamados/views_arquivar.py:68`, `:79`)

## 30/10 14:47 · sessao 3e890c58ac47 · - · - · origem=Ronald (tmux)

Q3: **sem achado** de quebra de leitor/teste pela nova chave. O `via` já era gravado; agora também participa da deduplicação (`app/chamados/models.py:923`, `app/chamados/models.py:930`).

- O teste que espera uma linha repete somente `abrir`, portanto continua usando a mesma chave (`app/chamados/tests/test_o261_veto_na_porta.py:430`, `app/chamados/tests/test_o261_veto_na_porta.py:437`).
- Os testes do outbox contam recusas por célula absolvida; esse ramo antecede o veto e usa outra deduplicação, inalterada (`app/ponto/tests/test_o_outbox_celula.py:351`, `app/ponto/tests/test_o_outbox_celula.py:550`, `app/chamados/models.py:726`, `app/chamados/models.py:942`).
- A tela de auditoria conta registros reais; duas operações distintas aparecerem como duas linhas não viola seu código (`app/core/views.py:705`, `app/core/views.py:727`).
- Execução dos testes e leitores externos: [não verificado]. [nao verificado]

**VETO-TRILHA-CICLO permanece um defeito**, mas não encontrei fundamento técnico suficiente para elevá-lo sozinho a bloqueador do veto: a recusa continua retornando `False`, mesmo quando a trilha é deduplicada (`app/chamados/models.py:780`).

Contraexemplo derivado: chamado resolvido recebe veto V e gera trilha; régua corrigida permite renascimento; chamado encerra novamente; V retorna; nova tentativa é recusada, porém a consulta encontra a primeira linha histórica e não registra o novo ciclo. A chave não contém ciclo nem considera transições posteriores (`app/chamados/models.py:923`); o renascimento sem veto está caracterizado em `app/chamados/tests/test_o261_veto_na_porta.py:511`. Ocorrência em produção: [não verificado].

Isso contradiz “enquanto nada muda” (`app/chamados/models.py:910`). Há precedente na própria classe para considerar mudança de estado desde a recusa anterior (`app/chamados/models.py:517`). Se a exigência do pouso A incluir trilha de cada ciclo, há falha contratual; essa exigência adicional não foi demonstrada pelo anexo. [não verificado]

## 30/10 14:46 · sessao 9bd14f4b5474 · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

1. **A grade pode desfazer a recusa da porta.** `abrir` descarta o resultado de `_renascer_por_premissa_viva` e devolve o chamado encerrado (`app/chamados/models.py:686`–688); `_emitir_furo` reabre esse mesmo objeto diretamente (`app/ponto/management/commands/reconciliar_grade.py:521`–522). Contraexemplo: `_emitir_furo(c,D,marks,ec_saudavel)` passa as guardas externas, mas o vínculo consultado pela porta está suspeito; o chamado existente está resolvido e sua premissa permanece viva → a porta registra recusa, devolve o encerrado, a grade chama `reabrir()` e o deixa vivo. O próprio diff reconhece a possibilidade de divergência dos vínculos no detector (`app/ponto/management/commands/detectar_ausencias.py:729`); `reabrir` não pergunta o veto (`app/chamados/models.py:1119`, `app/chamados/models.py:1158`). Ocorrência desse estado em produção: [não verificado].

2. **Há reabertura automática direta por signal.** O signal de chamado encerrado chama `resolver_perguntas_por_chamado_resolvido` (`app/chamados/signals.py:75`, `app/chamados/signals.py:89`–92), cujo ramo de resposta aguardando validação faz `_pai.reabrir()` sem recorte de módulo nem veto (`app/chamados/services/disputa_emissao.py:1598`–1608). Contraexemplo: disputa aberta com pai `entrada_bloqueada_turno_aberto`, resolvido, dia D vetado, sem irmão vivo/agrupamento/fecho admin, e pergunta respondida não validada nem materializada cujo destino está encerrado → salvar destino encerrado encontra a resposta pelo juiz (`app/chamados/juizes.py:1471`–1477) e reabre o pai vetado. Existência desse formato em produção: [não verificado].

3. **Nasce automaticamente chamado do universo sem perguntar o veto.** `bater_ponto` monta apenas `pendentes` e `total`, criando `entrada_bloqueada_turno_aberto` (`app/ponto/views.py:275`–301); sem dia, `_veto_da_regua` retorna `None` (`app/chamados/models.py:891`–895). Contraexemplo: POST de entrada com turno antigo aberto, vínculo suspeito, sem chamado vivo desse módulo → nasce chamado e envia push (`app/ponto/views.py:303`–310). A chamada atravessa `criar`, mas escapa da invariante; o próprio teste sanciona o escape (`app/chamados/tests/test_o261_veto_na_porta.py:310`–315).

4. **Leitor de `abrir` mantém cobrança após renascimento recusado.** `_marco_critico` só testa se o resultado é `None` (`app/ponto/management/commands/processar_alertas_turno.py:316`–321); `_abrir_chamado_critico` devolve o objeto encerrado (`app/ponto/management/commands/processar_alertas_turno.py:387`). Contraexemplo: `turno_aberto` resolvido, premissa viva, régua suspeita, sem alerta `pos_20` → `_renascer` recusa, mas `abrir` devolve objeto; o cron envia alerta e cria justificativa apontando chamado morto (`app/ponto/management/commands/processar_alertas_turno.py:389`–400). Antes do diff, com demais guardas satisfeitas, esse objeto renascia (`app/chamados/models.py:778`–782).

5. **Exceção humana passa sem a informação exigida.** Desarquivar chama `reabrir`, registra apenas resgate e responde `{'ok':True}`; não consulta nem inclui motivo do veto (`app/chamados/views_arquivar.py:68`–80). Reabrir disputa admin grava motivo de reabertura/justificativa, mas não veto (`app/chamados/services/acoes_disputa.py:245`–270); reabrir competência também registra apenas competência/estado (`app/chamados/services/competencia_trancada.py:258`–264). Contraexemplo: admin desarquiva chamado de dia suspeito, com demais guardas satisfeitas → volta à fila, mas resposta e trilha não dizem que admitiu dia vetado.

**Censo dos leitores de portas cujo comportamento pode mudar:**

| Leitor | Efeito |
|---|---|
| `app/ponto/management/commands/detectar_ausencias.py:688` (`MODULO_ORIGEM='batida_ausente'`, :29) | Agora pode retornar `VETO_NA_PORTA`; existente vetado fica encerrado (:731–734). |
| `app/ponto/management/commands/reconciliar_grade.py:499` (`CANON`, :54) | Nascimento pode ser recusado; reabertura pode contornar veto (:519–522). |
| `app/chamados/reconciliador.py:862` (chave `batida_ausente`, :804) | Nova recusa retorna `False`, não costura pergunta (:885–892). |
| `app/ponto/management/commands/processar_alertas_turno.py:335` | Novo nascimento vetado não gera marco (:317); existente vetado ainda permite cobrança, achado 4. |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | `turno_aberto_24h` pode ser recusado por suspeita/degeneração, pulando push/fio (:141–145). |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` | Nova recusa por veto; dry também muda (:138). |
| `app/chamados/models.py:686` | Dedup mantém encerrado quando renascimento recebe veto. |
| `app/chamados/services/resposta_tardia.py:45` | Celula acusando + dia vetado deixa de reabrir; passa ao ramo de nota/log e eventual pauta (:48–67). |

Os aliases de importação foram incluídos no censo estático; os outros emissores encontrados usam módulos fora do universo, e o emissor `app/ponto/views.py:291` não muda porque não leva dia. O universo vem de `app/chamados/catalogo/furo_modulos.py:15` e `app/chamados/reconciliador.py:336`.

**Correção importante:** não há caminho atual comprovado para o alegado `None.pk` da abertura manual no formulário. `tipos_para_colaborador` só lista entradas com `_abre` (`app/chamados/catalogo/modulos.py:281`–309); os cinco módulos do universo não o possuem (:105–112, :182), e o serviço rejeita tipo fora da lista (`app/chamados/services/abertura.py:149`–151). Portanto, não reportar esse 500 como achado.

`reabrir_orfas_enterradas.py:161` também chama `reabrir` diretamente, mas é comando de uma vez com `--apply`, explicitamente fora do pipeline (`app/chamados/management/commands/reabrir_orfas_enterradas.py:1`, :21–22; `app/config/crons.py:743`–750); não classificá-lo como cron automático.

## 30/10 14:46 · sessao 4155f4ff22ff · BENCH-FIO-MORTO-DONO-NA-LISTA · verificador · origem=Code

**REPROVADO como verificação final:** a leitura do dono lavrado está implementada, mas restam falhas de observabilidade, relógios internos e cobertura dos selos. O vigia consulta a lista e compara seu valor, sem recalcular o dono (`app/chamados/services/vigia_fio.py:259`, `app/chamados/services/vigia_fio.py:266`).

Identidade Git da cópia, completude de `f0d319ba..HEAD`, RED executado e resultados atuais de produção: **[nao verificado]**. As conclusões abaixo são de leitura do código; comentários com medições históricas não foram tratados como medição atual.

**Premissas pelo código**

Para **dono da divergência L-099**, o percurso usa um decisor: `dono_da_divergencia`. `InsumosDoDono.dono()` monta os insumos e delega; o medidor C9 escreve o resultado no item, e `lavrar()` persiste esse item em `MetricaSnapshot.detalhe`. O vigia recebe depois `dono_do_dia` da assinatura. Isso não significa que toda pergunta chamada “dono” tenha esse juiz: o responsável pelo fio — `admin`, `colab`, `chamado`, `nada` — é outra classificação (`app/ponto/management/commands/e6_oraculo.py:90`, `app/ponto/management/commands/e6_oraculo.py:146`, `app/ponto/services/teto_na_lista.py:86`, `app/ponto/services/teto_na_lista.py:113`, `app/escala/services/cadastro_realidade.py:313`, `app/folha/fora_da_folha.py:59`).

“Está na lista do admin” tem **uma função compartilhada entre tela e vigia, mas várias fontes internas**: `lista()` agrega cadastro vivo e lavras. Além disso, `resultado.dias_cadastro_x_realidade`, produzido pelo motor e lido pelo juiz do dono, **não é o objeto retornado pela lista da tela** (`app/escala/views.py:490`, `app/chamados/services/vigia_fio.py:259`, `app/escala/services/cadastro_realidade.py:513`, `app/escala/services/cadastro_realidade.py:575`, `app/ponto/motor_calculo_v2.py:2657`, `app/ponto/management/commands/e6_oraculo.py:142`).

No agregador atual, **C9 é a assinatura que publica `data` estruturada**. C4 e C5 mencionam datas apenas no texto `detalhe`; portanto `dias_na_lista()` não representa todas as datas visualmente mencionadas pela tela: representa as assinaturas com chave `data`, atualmente C9 (`app/escala/services/cadastro_realidade.py:312`, `app/escala/services/cadastro_realidade.py:374`, `app/escala/services/cadastro_realidade.py:142`, `app/escala/services/cadastro_realidade.py:669`).

| Campo/valor | Quem escreve ou calcula | Quem lê, inclusive fora do diff |
|---|---|---|
| `fio_do_admin` | Inicialização e incremento no censo: `app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:320`. | Impressão: `app/chamados/services/vigia_fio.py:516`; carimbo: `app/chamados/management/commands/vigiar_fio_morto.py:205`; ensaio de reparo, por seleção genérica de números: `app/chamados/management/commands/materializar_passivo_validado.py:208`; testes: `app/chamados/tests/test_o261_vigia_fio.py:566`, `app/chamados/tests/test_o261_vigia_fio.py:666`. |
| `celulas_fio_do_admin` | Lista inicial e inclusão limitada por `limite`: `app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:321`. | Impressão e carimbo: `app/chamados/services/vigia_fio.py:516`, `app/chamados/management/commands/vigiar_fio_morto.py:205`; teste: `app/chamados/tests/test_o261_vigia_fio.py:565`. O ensaio exclui listas: `app/chamados/management/commands/materializar_passivo_validado.py:208`. |
| `fio_do_admin_sem_resposta` | Inicialização; incremento **por célula candidata**, quando a chamada única à lista falhou: `app/chamados/services/vigia_fio.py:217`, `app/chamados/services/vigia_fio.py:250`, `app/chamados/services/vigia_fio.py:264`. | Impressão, carimbo e ensaio: `app/chamados/services/vigia_fio.py:516`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `app/chamados/management/commands/materializar_passivo_validado.py:208`; testes de falha: `app/chamados/tests/test_o261_vigia_fio.py:579`, `app/chamados/tests/test_o261_vigia_fio.py:588`. |
| `lista()` | Compõe fontes, filtra empresas e produz `postos`: `app/escala/services/cadastro_realidade.py:513`, `app/escala/services/cadastro_realidade.py:588`, `app/escala/services/cadastro_realidade.py:652`. | Tela/PDF: `app/escala/views.py:490`, `app/escala/views.py:492`, `app/escala/views.py:495`; vigia: `app/chamados/services/vigia_fio.py:259`; encerradores C7/C8: `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`. |
| Assinaturas `data` / `dono_do_dia` | Medidor escreve `data`/`dono`; lavra persiste; `_teto_do_dia()` transforma em assinatura C9: `app/ponto/services/teto_na_lista.py:79`, `app/ponto/services/teto_na_lista.py:86`, `app/ponto/services/teto_na_lista.py:113`, `app/escala/services/cadastro_realidade.py:312`. | `lista()` preserva assinatura, deriva apresentação/ação e conta por dono: `app/escala/services/cadastro_realidade.py:598`, `app/escala/services/cadastro_realidade.py:602`, `app/escala/services/cadastro_realidade.py:607`; HTML/PDF mostram `detalhe` e `dono`: `app/templates/escala/cadastro_x_realidade.html:28`, `app/escala/services/cadastro_realidade.py:708`; projeção lê os campos estruturados: `app/escala/services/cadastro_realidade.py:670`. |
| `dias_na_lista()` | Projeta a saída recebida em mapa `(colab_id, data) → dono_do_dia`: `app/escala/services/cadastro_realidade.py:665`, `app/escala/services/cadastro_realidade.py:670`. | Vigia guarda e consulta o mapa: `app/chamados/services/vigia_fio.py:259`, `app/chamados/services/vigia_fio.py:266`; teste direto: `app/chamados/tests/test_o261_vigia_fio.py:615`. |
| `InsumosDoDono.dono()` | Calcula o retorno por delegação ao juiz; os insumos ficam na instância: `app/ponto/management/commands/e6_oraculo.py:121`, `app/ponto/management/commands/e6_oraculo.py:149`, `app/ponto/management/commands/e6_oraculo.py:152`, `app/ponto/management/commands/e6_oraculo.py:155`. | E6: `app/ponto/management/commands/e6_oraculo.py:338`; pautas: `app/ponto/management/commands/pautas_discordantes.py:104`; produtor C9: `app/ponto/services/teto_na_lista.py:86`. O vigia atual lê o resultado lavrado: `app/chamados/services/vigia_fio.py:266`. |
| `agora` / `hoje` do comando | `timezone.now()` e `localdate(agora)`: `app/chamados/management/commands/vigiar_fio_morto.py:158`, `app/chamados/management/commands/vigiar_fio_morto.py:159`. | Comando passa `hoje` ao censo e serializa ambos: `app/chamados/management/commands/vigiar_fio_morto.py:160`, `app/chamados/management/commands/vigiar_fio_morto.py:196`, `app/chamados/management/commands/vigiar_fio_morto.py:197`. Censo lê nos cortes temporais e na lista: `app/chamados/services/vigia_fio.py:278`, `app/chamados/services/vigia_fio.py:289`, `app/chamados/services/vigia_fio.py:259`. Lista repassa a C4, C5, vínculos e ações: `app/escala/services/cadastro_realidade.py:535`, `app/escala/services/cadastro_realidade.py:570`, `app/escala/services/cadastro_realidade.py:580`, `app/escala/services/cadastro_realidade.py:599`. |
| `medido_em` do carimbo | Derivado de `agora`; JSON gravado por `_carimbar()` com substituição do arquivo: `app/chamados/management/commands/vigiar_fio_morto.py:196`, `app/chamados/management/commands/vigiar_fio_morto.py:128`, `app/chamados/management/commands/vigiar_fio_morto.py:129`. | Leitor calcula idade e imprime: `bin/fio_estado.py:74`, `bin/fio_estado.py:79`, `bin/fio_estado.py:113`; ESTADO usa esse compositor: `bin/gerar_estado.py:261`; teste verifica formato/data: `app/chamados/tests/test_o261_vigia_fio.py:793`, `app/chamados/tests/test_o261_vigia_fio.py:795`. |

Os escritores dos demais contadores ficam no próprio censo; o comando acrescenta medições auxiliares ao carimbo (`app/chamados/services/vigia_fio.py:207`, `app/chamados/management/commands/vigiar_fio_morto.py:205`):

| Grupo | Escrita/cálculo | Leitura específica |
|---|---|---|
| Universo, lâmpadas e `cego` | `app/chamados/services/vigia_fio.py:207`, `app/chamados/services/vigia_fio.py:231`, `app/chamados/services/vigia_fio.py:363`. | Impressão: `app/chamados/services/vigia_fio.py:489`; ESTADO: `bin/fio_estado.py:90`. |
| Vocabulário, fio vivo, futuro/hoje | `app/chamados/services/vigia_fio.py:278`, `app/chamados/services/vigia_fio.py:282`, `app/chamados/services/vigia_fio.py:287`, `app/chamados/services/vigia_fio.py:292`. | Impressão: `app/chamados/services/vigia_fio.py:491`, `app/chamados/services/vigia_fio.py:496`, `app/chamados/services/vigia_fio.py:517`; seleção no ESTADO: `bin/fio_estado.py:88`, `bin/fio_estado.py:91`. |
| Decisão pendente, ausência, escala não descreve | `app/chamados/services/vigia_fio.py:298`, `app/chamados/services/vigia_fio.py:305`, `app/chamados/services/vigia_fio.py:310`; pendente com alvo: `app/chamados/services/vigia_fio.py:362`. | Impressão: `app/chamados/services/vigia_fio.py:506`, `app/chamados/services/vigia_fio.py:509`, `app/chamados/services/vigia_fio.py:511`. |
| `fio_morto`, cobrando, sem pergunta/pergunta morreu, células | `app/chamados/services/vigia_fio.py:324`, `app/chamados/services/vigia_fio.py:325`, `app/chamados/services/vigia_fio.py:327`, `app/chamados/services/vigia_fio.py:330`. | Impressão: `app/chamados/services/vigia_fio.py:494`; células opcionais: `app/chamados/management/commands/vigiar_fio_morto.py:181`; ESTADO: `bin/fio_estado.py:86`, `bin/fio_estado.py:101`. |
| Classes da porta, com/sem alvo | `app/chamados/services/vigia_fio.py:223`, `app/chamados/services/vigia_fio.py:338`, `app/chamados/services/vigia_fio.py:345`, `app/chamados/services/vigia_fio.py:355`. | Impressão: `app/chamados/services/vigia_fio.py:499`, `app/chamados/services/vigia_fio.py:503`. |

Todos esses campos vão ao carimbo inteiro; todos os números/booleanos também entram no ensaio de reparo, que compara antes/depois e imprime diferenças (`app/chamados/management/commands/vigiar_fio_morto.py:205`, `app/chamados/management/commands/materializar_passivo_validado.py:97`, `app/chamados/management/commands/materializar_passivo_validado.py:105`, `app/chamados/management/commands/materializar_passivo_validado.py:208`, `app/chamados/management/commands/materializar_passivo_validado.py:232`).

As medições auxiliares são calculadas por `viva_em_disputa_fechada`, `respondida_sem_resposta` e `materializada_viva`; o comando imprime e carimba seus resultados. O ESTADO compara genericamente as chaves presentes em `esperado`, incluindo as duas últimas quando declaradas ali (`app/chamados/services/vigia_fio.py:388`, `app/chamados/services/vigia_fio.py:416`, `app/chamados/services/vigia_fio.py:480`, `app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/management/commands/vigiar_fio_morto.py:174`, `app/chamados/management/commands/vigiar_fio_morto.py:178`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `bin/fio_estado.py:72`).

Alarmes, pautas e placar têm percursos distintos:

- **Alarme:** lê os contadores declarados em `ALARMA_SEMPRE`/`ALARMA_EM_0`, monta `gritou` e sai com código 2. O cron atual habilita **também `--alarme-passivo`**; os três campos novos não estão nas listas de alarme (`app/chamados/management/commands/vigiar_fio_morto.py:80`, `app/chamados/management/commands/vigiar_fio_morto.py:161`, `app/chamados/management/commands/vigiar_fio_morto.py:214`, `app/config/crons.py:1654`).
- **Pautas discordantes:** consultam células e `_fio_do_dia`, depois recebem o dono calculado por `InsumosDoDono`; não consomem o censo alterado (`app/ponto/services/pautas_dos_discordantes.py:59`, `app/ponto/services/pautas_dos_discordantes.py:75`, `app/ponto/management/commands/pautas_discordantes.py:104`).
- **Placar de cron:** recebe tempos, duração e código de saída; não recebe contadores VFM. Código 2 vira alarme; pauta de cron quebrado exige saída diferente de 0 e 2 (`bin/cron_run.sh:13`, `app/core/management/commands/pipeline_placar.py:76`, `bin/cron_run.sh:32`).

**Checklist de classes — achados**

1. **Segundo juiz / derivação paralela:** não há recálculo novo do dono no vigia; há leitura do valor lavrado. Entretanto, o caminho incorporado da lista mantém uma escolha própria de competência: C2 usa corte literal `21`, enquanto a autoridade indicada usa corte da empresa. É desvio preexistente agora alcançado pelo censo, não duplicação nova do juiz L-099 (`app/chamados/services/vigia_fio.py:266`, `app/escala/services/cadastro_realidade.py:187`, `app/docs/JUIZES.md:57`, `app/ponto/janelas.py:60`).

2. **Segundo escritor fora da porta:** não identifiquei escritor concorrente dos campos auditados no diff. C9 continua persistida por `lavrar`; a nova classificação escreve o dicionário do censo, e o comando grava sua fotografia. O tratamento externo de erro incrementa observabilidade em memória e emite log (`app/ponto/services/teto_na_lista.py:113`, `app/chamados/services/vigia_fio.py:320`, `app/chamados/management/commands/vigiar_fio_morto.py:128`, `app/core/observ.py:75`, `app/core/observ.py:79`).

3. **Relógio fora da borda — confirmado.** O comando unificou seu instante, mas C2 ainda chama `date.today()`; C4 chama `afastados_hoje` sem passar o dia, acionando outro `localdate()`. Antes mesmo do ramo admin, o calendário também lê seu próprio relógio. Logo “uma leitura de relógio na rodada” não vale transitivamente (`app/chamados/management/commands/vigiar_fio_morto.py:158`, `app/escala/services/cadastro_realidade.py:518`, `app/escala/services/cadastro_realidade.py:186`, `app/escala/services/cadastro_realidade.py:350`, `app/colaboradores/services/situacao_exibida.py:45`, `app/chamados/services/vigia_fio.py:294`, `app/colaboradores/services/calendario.py:249`).

4. **Falha da fonte sem sinal no contador — confirmado.** `lista()` captura a exceção da C9 e prossegue; ausência de snapshot retorna `sem_medida=True`, também descartado pelo formatador. O vigia só incrementa `fio_do_admin_sem_resposta` quando a chamada externa lança exceção. Assim a fonte C9 pode falhar ou nunca ter sido medida e o contador continuar zero. O dia permanece no fio morto: o defeito é de observabilidade, não uma retirada indevida do alarme (`app/escala/services/cadastro_realidade.py:565`, `app/ponto/services/teto_na_lista.py:125`, `app/escala/services/cadastro_realidade.py:310`, `app/chamados/services/vigia_fio.py:260`, `app/chamados/services/vigia_fio.py:264`, `app/chamados/services/vigia_fio.py:324`).

5. **Selo sem mordida suficiente — confirmado no cenário da soma.** O teste acrescentou `+ fio_do_admin`, mas sua mistura não cria C9 nem usa `_admin`; a empresa de diagnóstico fica fora das visíveis. O termo novo vale zero nesse cenário: removê-lo da igualdade não é detectado por esse teste. Há teste separado com dois dias admin, mas ele não verifica a partição completa. O teste de carimbo também não força travessia de meia-noite; RED real permanece **[nao verificado]** (`app/chamados/tests/test_o261_vigia_fio.py:34`, `app/colaboradores/queries.py:85`, `app/chamados/tests/test_o261_vigia_fio.py:649`, `app/chamados/tests/test_o261_vigia_fio.py:666`, `app/chamados/tests/test_o261_vigia_fio.py:565`, `app/chamados/tests/test_o261_vigia_fio.py:795`).

6. **Custo — ampliação confirmada, impacto não medido.** Uma célula candidata dispara a lista completa. O cache limita chamadas, não consultas: C2 percorre vínculos ativos e células da competência, chamando `periodos_do_dia` por célula; C4 consulta por afastado e C5 por empresa. Portanto o custo não se limita aos dias mortos. Tempo, volume e eventual estouro em produção: **[nao verificado]** (`app/chamados/services/vigia_fio.py:250`, `app/chamados/services/vigia_fio.py:259`, `app/escala/services/cadastro_realidade.py:190`, `app/escala/services/cadastro_realidade.py:195`, `app/escala/services/cadastro_realidade.py:200`, `app/escala/services/cadastro_realidade.py:359`, `app/escala/services/cadastro_realidade.py:128`).

7. **Censo “quem mais lê” incompleto — confirmado no ESTADO.** A transferência reduz `fio_morto`, e o carimbo preserva os novos campos; porém o leitor do ESTADO descarta os três e continua exibindo morto/cobrando/hoje. A queda aparece sem mostrar o destino admin nem sua falha de consulta. O ensaio de reparo é outro leitor fora do diff, mas seus números entram automaticamente pela seleção genérica (`app/chamados/services/vigia_fio.py:320`, `app/chamados/services/vigia_fio.py:323`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `bin/fio_estado.py:82`, `bin/fio_estado.py:101`, `bin/gerar_estado.py:261`, `app/chamados/management/commands/materializar_passivo_validado.py:208`).

## 30/10 14:46 · sessao eae7429fd562 · BENCH-FIO-MORTO-DONO-NA-LISTA · verificador · origem=Code

**Não daria a verificação como fechada:** há perda de visibilidade no ESTADO, cobertura parcial da expressão “dia na lista”, falhas internas não contadas em `sem_resposta` e selos que não exercitam a nova classe integralmente. As evidências estão em `bin/fio_estado.py:82`, `app/escala/services/cadastro_realidade.py:667`, `app/escala/services/cadastro_realidade.py:565` e `app/chamados/tests/test_o261_vigia_fio.py:655`.

Esta é uma revisão estática. Execução dos testes, RED observado, quantidades afetadas e desempenho em produção: **[nao verificado]**. A correspondência histórica `f0d319ba..HEAD` também é **[nao verificado]**: a cópia acessível não contém repositório Git. O texto específico de `CODEX-TRIAGEM-INDEPENDENTE item 3` não foi localizado: **[nao verificado]**.

Pelo código, as premissas ficam assim:

1. **O dono L-099 tem um calculador central; “dono” sem qualificador não é uma pergunta única.** `dono_da_divergencia` decide CADASTRO por defeito de cadastro, BATIDA por sobra/falta ou paridade ímpar, e CADASTRO/ESTRUTURA pelo pertencimento ao conjunto do motor. `InsumosDoDono.dono` alimenta e chama essa função. Outros domínios têm perguntas distintas: `dono_da_linha` da folha e `dono(causa)` da pergunta no app. (`app/ponto/management/commands/e6_oraculo.py:90`, `app/ponto/management/commands/e6_oraculo.py:96`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:100`, `app/ponto/management/commands/e6_oraculo.py:146`, `app/folha/retrato_folha.py:458`, `app/chamados/services/pergunta_no_app.py:94`.)

2. **A lista tem um montador compartilhado, mas várias fontes.** A tela chama `lista(empresas={visíveis})`; o censo chama o mesmo montador, cruzando visíveis com `--empresa`. Dentro dele há consultas atuais de cadastro e diversas lavras: C1–C9 e assinaturas do esmeril. Portanto, “mesma função da tela” é verdadeiro; “uma única lavra/tabela como fonte” não é. (`app/escala/views.py:490`, `app/chamados/services/vigia_fio.py:256`, `app/chamados/services/vigia_fio.py:259`, `app/escala/services/cadastro_realidade.py:513`, `app/escala/services/cadastro_realidade.py:527`, `app/escala/services/cadastro_realidade.py:549`, `app/escala/services/cadastro_realidade.py:563`, `app/escala/services/cadastro_realidade.py:575`.)

3. **“Dia na lista”, no código novo, significa assinatura com `data` estruturada.** C9 escreve esse atributo; C4 e C5 mostram dias no texto `detalhe`, sem esse atributo. A tela imprime esses detalhes, mas `dias_na_lista` não os reconhece. Assim, o predicado implementado não abrange todo dia mencionado visualmente na lista. (`app/escala/services/cadastro_realidade.py:312`, `app/escala/services/cadastro_realidade.py:142`, `app/escala/services/cadastro_realidade.py:372`, `app/escala/services/cadastro_realidade.py:667`, `app/templates/escala/cadastro_x_realidade.html:28`.)

4. **O dono mostrado na C9 e o dono consultado pelo vigia são medições distintas.** A lista usa `dono` lavrado; o vigia recalcula pelo carregador atual. Uma C9 sem dono ou com ESTRUTURA oferece nenhuma porta humana, enquanto o censo só exige presença da data e dono atual BATIDA/CADASTRO. Essa combinação é admitida pelo código; sua ocorrência em produção é **[nao verificado]**. (`app/ponto/services/teto_na_lista.py:86`, `app/escala/services/cadastro_realidade.py:313`, `app/escala/services/cadastro_realidade.py:599`, `app/escala/services/cadastro_realidade.py:457`, `app/chamados/services/vigia_fio.py:267`, `app/chamados/services/vigia_fio.py:275`.)

O mapa dos campos pedidos, incluindo consumidores fora do diff:

| Campo/valor | Quem escreve/calcula | Quem lê |
|---|---|---|
| `fio_do_admin` | Inicialização e incremento em `app/chamados/services/vigia_fio.py:217` e `app/chamados/services/vigia_fio.py:333`. | Impressão em `app/chamados/services/vigia_fio.py:529`; cópia integral para carimbo em `app/chamados/management/commands/vigiar_fio_morto.py:205`; projeção numérica do reparador em `app/chamados/management/commands/materializar_passivo_validado.py:208`; testes em `app/chamados/tests/test_o261_vigia_fio.py:577` e `app/chamados/tests/test_o261_vigia_fio.py:674`. |
| `celulas_fio_do_admin` | Inicialização e inclusão limitada por `limite` em `app/chamados/services/vigia_fio.py:217` e `app/chamados/services/vigia_fio.py:334`. | Impressão em `app/chamados/services/vigia_fio.py:529`; carimbo em `app/chamados/management/commands/vigiar_fio_morto.py:205`; teste em `app/chamados/tests/test_o261_vigia_fio.py:576`. O reparador exclui listas pela seleção `int/bool`, em `app/chamados/management/commands/materializar_passivo_validado.py:208`. |
| `fio_do_admin_sem_resposta` | Nasce somente nas exceções, contando candidatos afetados, em `app/chamados/services/vigia_fio.py:264` e `app/chamados/services/vigia_fio.py:278`. | Impressão com zero por ausência em `app/chamados/services/vigia_fio.py:529`; carimbo genérico em `app/chamados/management/commands/vigiar_fio_morto.py:205`; reparador genérico em `app/chamados/management/commands/materializar_passivo_validado.py:208`; testes em `app/chamados/tests/test_o261_vigia_fio.py:588` e `app/chamados/tests/test_o261_vigia_fio.py:597`. |
| `lista()` — `postos` e `assinaturas` | `lista` monta linhas, preserva atributos C e sintetiza assinaturas A em `app/escala/services/cadastro_realidade.py:590`, `app/escala/services/cadastro_realidade.py:602`, `app/escala/services/cadastro_realidade.py:617` e `app/escala/services/cadastro_realidade.py:652`. | Tela/PDF em `app/escala/views.py:490`, `app/templates/escala/cadastro_x_realidade.html:28` e `app/escala/services/cadastro_realidade.py:706`; censo em `app/chamados/services/vigia_fio.py:259`; fechamento C7 em `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`; fechamento C8 em `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`. |
| `assinaturas[].data` | A medição C9 produz a data do turno em `app/ponto/services/teto_na_lista.py:69` e `app/ponto/services/teto_na_lista.py:79`; `lavrar` persiste em `MetricaSnapshot.detalhe` em `app/ponto/services/teto_na_lista.py:113`; `_teto_do_dia` lê e copia para a assinatura em `app/escala/services/cadastro_realidade.py:310` e `app/escala/services/cadastro_realidade.py:312`; `lista` preserva em `app/escala/services/cadastro_realidade.py:602`. | `dias_na_lista` lê diretamente em `app/escala/services/cadastro_realidade.py:667`. Tela/PDF exibem a data pelo `detalhe` formatado em `app/escala/services/cadastro_realidade.py:314`, consumido em `app/templates/escala/cadastro_x_realidade.html:28` e `app/escala/services/cadastro_realidade.py:711`. |
| `dias_na_lista` | Projeta conjunto `(colab_id, str(data)[:10])` em `app/escala/services/cadastro_realidade.py:663` e `app/escala/services/cadastro_realidade.py:668`. | O censo guarda e consulta em `app/chamados/services/vigia_fio.py:259` e `app/chamados/services/vigia_fio.py:267`; teste direto em `app/chamados/tests/test_o261_vigia_fio.py:623`. |
| `InsumosDoDono.dono` | Carrega batidas, turnos, cadastro e eventualmente espelho; chama `dono_da_divergencia` em `app/ponto/management/commands/e6_oraculo.py:126`, `app/ponto/management/commands/e6_oraculo.py:133`, `app/ponto/management/commands/e6_oraculo.py:141`, `app/ponto/management/commands/e6_oraculo.py:149` e `app/ponto/management/commands/e6_oraculo.py:155`. | Vigia em `app/chamados/services/vigia_fio.py:275`; e6, contadores e linhas exportáveis em `app/ponto/management/commands/e6_oraculo.py:338`, `app/ponto/management/commands/e6_oraculo.py:339` e `app/ponto/management/commands/e6_oraculo.py:361`; pautas em `app/ponto/management/commands/pautas_discordantes.py:104`; medição C9 em `app/ponto/services/teto_na_lista.py:86`. |
| `agora` / `hoje` do comando | Captura única e derivação em `app/chamados/management/commands/vigiar_fio_morto.py:158` e `app/chamados/management/commands/vigiar_fio_morto.py:159`. O censo tem defaults independentes em `app/chamados/services/vigia_fio.py:181` e `app/chamados/services/vigia_fio.py:182`. | Censo em `app/chamados/management/commands/vigiar_fio_morto.py:160`; teto futuro/passado em `app/chamados/services/vigia_fio.py:291` e `app/chamados/services/vigia_fio.py:302`; lista em `app/chamados/services/vigia_fio.py:259`; carregador em `app/chamados/services/vigia_fio.py:274`; espelho em `app/ponto/management/commands/e6_oraculo.py:141`; carimbo em `app/chamados/management/commands/vigiar_fio_morto.py:196`. |
| `medido_em` do carimbo | Composto do instante capturado em `app/chamados/management/commands/vigiar_fio_morto.py:196`; escrita física JSON e substituição em `app/chamados/management/commands/vigiar_fio_morto.py:127` e `app/chamados/management/commands/vigiar_fio_morto.py:129`. | Parse, idade e apresentação em `bin/fio_estado.py:74`, `bin/fio_estado.py:79` e `bin/fio_estado.py:112`; integração no ESTADO em `bin/gerar_estado.py:260`; teste em `app/chamados/tests/test_o261_vigia_fio.py:801`. |

Para `fio_morto` e os demais contadores, a autoria e a circulação são:

- **Escritor do censo:** universos e zeros em `app/chamados/services/vigia_fio.py:208`; lâmpadas em `app/chamados/services/vigia_fio.py:231`; categorias temporal/vocabulário/vivo em `app/chamados/services/vigia_fio.py:291`; decisão, ausência e escala em `app/chamados/services/vigia_fio.py:307`; morto e suas subdivisões em `app/chamados/services/vigia_fio.py:337`; classes da porta e alvo em `app/chamados/services/vigia_fio.py:351`; pendente com alvo em `app/chamados/services/vigia_fio.py:375`; cegueira em `app/chamados/services/vigia_fio.py:376`.

- **Impressão e carimbo:** `linhas` lê os contadores; o comando imprime e persiste o dicionário inteiro. Os números adicionais de disputa fechada, resposta vazia e materializada viva são calculados separadamente e anexados pelo comando. (`app/chamados/services/vigia_fio.py:499`, `app/chamados/management/commands/vigiar_fio_morto.py:164`, `app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/management/commands/vigiar_fio_morto.py:174`, `app/chamados/management/commands/vigiar_fio_morto.py:178`, `app/chamados/management/commands/vigiar_fio_morto.py:205`.)

- **ESTADO:** compara genericamente `censo × esperado`, mas publica uma seleção fixa dos campos. O esperado não inclui os novos contadores de admin. (`bin/fio_estado.py:69`, `bin/fio_estado.py:72`, `bin/fio_estado.py:82`, `bin/fio_estado.py:101`, `app/chamados/management/commands/vigiar_fio_morto.py:86`, `app/chamados/management/commands/vigiar_fio_morto.py:91`.)

- **Alarmes:** leem futuro/cego e, com flag passiva, morto/cobrando/com-alvo; o cron da cópia liga ambas as flags. Exceção externa da lista/juiz conserva o dia no morto, que continua elegível ao alarme. (`app/chamados/management/commands/vigiar_fio_morto.py:80`, `app/chamados/management/commands/vigiar_fio_morto.py:161`, `app/chamados/management/commands/vigiar_fio_morto.py:214`, `app/config/crons.py:1654`, `app/chamados/services/vigia_fio.py:263`, `app/chamados/services/vigia_fio.py:337`.)

- **Reparador:** também lê o censo, antes/depois, sem fornecer `hoje/agora`; copia todos os números e imprime diferenças. Isso inclui `fio_do_admin` e `sem_resposta` quando presente. (`app/chamados/management/commands/materializar_passivo_validado.py:97`, `app/chamados/management/commands/materializar_passivo_validado.py:105`, `app/chamados/management/commands/materializar_passivo_validado.py:203`, `app/chamados/management/commands/materializar_passivo_validado.py:208`, `app/chamados/management/commands/materializar_passivo_validado.py:232`.)

- **Placar de cron e pauta TI:** o envelope escreve duração/exit, não contadores do censo; o placar lê esses dados. Pauta TI nasce por falha de execução, excluindo exit 0/2, e fecha na recuperação. (`bin/cron_run.sh:13`, `app/core/management/commands/pipeline_placar.py:73`, `bin/cron_run.sh:32`, `app/core/services/crons_quebrados.py:71`, `app/core/services/crons_quebrados.py:80`.)

- **Pautas dos discordantes:** não recebem este censo; obtêm dias por `_alimentar/_fio_do_dia`, calculam dono por `InsumosDoDono` e escrevem/fecham pela porta de pautas. Logo não são leitor automático do novo `fio_do_admin`. (`app/ponto/services/pautas_dos_discordantes.py:59`, `app/ponto/services/pautas_dos_discordantes.py:73`, `app/ponto/management/commands/pautas_discordantes.py:104`, `app/ponto/services/pautas_dos_discordantes.py:172`, `app/ponto/services/pautas_dos_discordantes.py:178`.)

O checklist de classes resulta nestes achados:

1. **Segundo juiz de pergunta registrada — encontrado na dependência C2, preexistente.** A nova chamada de `lista` alcança `_jornada_que_mente`, que calcula competência com `periodo_apuracao(..., 21)`. A autoridade registrada é `janela_atual`, e JUIZES explicita “nunca 21 cravado”. Não é um novo clone do dono L-099: o trecho novo chama o juiz existente. (`app/escala/services/cadastro_realidade.py:518`, `app/escala/services/cadastro_realidade.py:187`, `app/core/juizes.py:246`, `app/docs/JUIZES.md:57`, `app/chamados/services/vigia_fio.py:275`.)

2. **Segundo escritor fora da porta — sem achado comprovado nas alterações.** As novas escritas são no dicionário/conjunto em memória; o carimbo continua usando `_carimbar`; a assinatura C9 vem da lavra existente. Isso não equivale a prova dinâmica de ausência de efeitos em toda a cadeia. (`app/chamados/services/vigia_fio.py:264`, `app/chamados/services/vigia_fio.py:333`, `app/escala/services/cadastro_realidade.py:668`, `app/chamados/management/commands/vigiar_fio_morto.py:195`, `app/ponto/services/teto_na_lista.py:113`.)

3. **Relógio fora da borda — encontrado.** O comando alinha seu carimbo, mas o censo ainda possui defaults independentes; C2 lê `date.today()`; C4 chama `afastados_hoje` sem transmitir o dia. O caminho preexistente do calendário também captura outro instante. Portanto a rodada inteira não está congelada pelo novo `agora`. C2/C4 não produzem `data` estruturada: esses relógios, isoladamente, não demonstram alteração do novo contador. (`app/chamados/services/vigia_fio.py:181`, `app/escala/services/cadastro_realidade.py:186`, `app/escala/services/cadastro_realidade.py:350`, `app/colaboradores/services/situacao_exibida.py:45`, `app/chamados/services/vigia_fio.py:307`, `app/colaboradores/services/calendario.py:249`, `app/escala/services/cadastro_realidade.py:215`, `app/escala/services/cadastro_realidade.py:372`.)

4. **Derivação paralela/fonte única — a presença é projetada da mesma lista, mas o contrato é mais estreito que a apresentação.** `dias_na_lista` não recalcula a lista; porém só considera `data`, omitindo os dias textuais de C4/C5. Além disso, `dias_cadastro_x_realidade` do motor, usado pelo dono, não é o conjunto produzido por `dias_na_lista`. São entradas distintas da conjunção, não evidências intercambiáveis. (`app/escala/services/cadastro_realidade.py:664`, `app/escala/services/cadastro_realidade.py:667`, `app/escala/services/cadastro_realidade.py:142`, `app/escala/services/cadastro_realidade.py:374`, `app/ponto/management/commands/e6_oraculo.py:142`, `app/chamados/services/vigia_fio.py:267`, `app/chamados/services/vigia_fio.py:275`.)

5. **Falha de fonte não inteiramente observada.** Se `_teto_do_dia` falhar, `lista` captura a exceção e retorna resultado parcial; a exceção não chega ao contador externo. Sem snapshot, o leitor também devolve itens vazios. O dia permanece morto, mas `fio_do_admin_sem_resposta` não denuncia essas perdas da fonte C9. (`app/escala/services/cadastro_realidade.py:563`, `app/escala/services/cadastro_realidade.py:565`, `app/ponto/services/teto_na_lista.py:125`, `app/chamados/services/vigia_fio.py:260`, `app/chamados/services/vigia_fio.py:267`.)

6. **Selo/RED sem mordida suficiente — encontrado na partição e no tempo.** A soma inclui `fio_do_admin`, mas a fixture não produz essa classe: usa empresa de diagnóstico excluída dos visíveis. O teste positivo substitui lista e carregador por mocks, e não cobra a conservação. O teste temporal só verifica formato/data, reexecutando censo sem reutilizar `agora`; não fabrica travessia da meia-noite. (`app/chamados/tests/test_o261_vigia_fio.py:34`, `app/colaboradores/queries.py:85`, `app/chamados/tests/test_o261_vigia_fio.py:655`, `app/chamados/tests/test_o261_vigia_fio.py:674`, `app/chamados/tests/test_o261_vigia_fio.py:560`, `app/chamados/tests/test_o261_vigia_fio.py:576`, `app/chamados/tests/test_o261_vigia_fio.py:787`, `app/chamados/tests/test_o261_vigia_fio.py:801`.)

7. **Custo — ampliação demonstrada, duração não medida.** Um candidato já dispara a lista inteira, incluindo passagem pelas células da janela C2. Há cache de lista e de carregador por colaborador/competência, mas cada carregador pode consultar batidas, turnos e espelho; o espelho apura de `min(piso_visual, apur_ini)` até hoje, permitindo trabalho sobreposto entre competências. (`app/chamados/services/vigia_fio.py:250`, `app/chamados/services/vigia_fio.py:274`, `app/escala/services/cadastro_realidade.py:195`, `app/escala/services/cadastro_realidade.py:200`, `app/ponto/management/commands/e6_oraculo.py:126`, `app/ponto/management/commands/e6_oraculo.py:141`, `app/ponto/services/espelho.py:1229`, `app/ponto/services/espelho.py:1266`.) Latência e consultas reais: **[nao verificado]**.

8. **Censo “quem mais lê” incompleto — consequência concreta no ESTADO.** A transferência reduz `fio_morto`, mas o destino `fio_do_admin` fica fora da projeção e da frase do painel. O carimbo e stdout preservam o número; o ESTADO não o apresenta. O reparador é outro leitor que também precisa constar dessa análise. (`app/chamados/services/vigia_fio.py:333`, `app/chamados/services/vigia_fio.py:336`, `app/chamados/services/vigia_fio.py:529`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `bin/fio_estado.py:82`, `bin/fio_estado.py:101`, `app/chamados/management/commands/materializar_passivo_validado.py:208`.)

_578 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
