# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 16:38** · publicado em **31/10/2026 16:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 16:38 · sessao eb6a9f7f9f33 · F1C4-3-EMISSORES · verificador · origem=Code

A delegação do ramo do supra está implementada, e os dois ramos do cartório entregam atas montadas no julgamento corrente. A exceção `_protesta` continua real: ela impede que a primeira cláusula do juiz seja alcançada pelo cartório quando `trabalha=False`. (`app/ponto/supra_juiz.py:65-71`; `app/ponto/services/cartorio.py:1012-1029`, `:1184-1214`)

Há também um defeito adicional no delta: a nova deduplicação de Pauta permite duplicatas sob concorrência, por intercalação entre a consulta e a criação. Ocorrência efetiva em produção: **[não verificado]**. (`app/chamados/services/fabrica_da_celula.py:180-185`; `app/pautas/services.py:152-185`; `app/pautas/models.py:113-133`)

**1. Escritores, leitores e autoridades**

O predicado efetivo do juiz é:

```python
cel.trabalha is False or bool(
    ata.get('tipo_dia') and ata.get('tipo_dia') != 'trabalho'
)
```

Portanto, a segunda cláusula exige um valor verdadeiro: `None` e `''` não bastam; `trabalha=None` pode ser calado por uma ata preenchida com tipo diferente de trabalho. (`app/chamados/juizes.py:407-408`)

| Campo/valor | Quem escreve | Quem lê e como decide |
|---|---|---|
| `CelulaDia.trabalha` | Geração/regeneração pela porta, a partir do calendário calculado; há também limpeza para `False` e reversão pelo DNA anterior. (`app/ponto/portas/celula.py:231-260`, `:532`, `:590-615`) | O juiz lê `is False`; fábrica, materialização, cluster e intervalo passaram a consultá-lo. (`app/chamados/juizes.py:408`; `app/chamados/services/fabrica_da_celula.py:80`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`) |
| `ata.tipo_dia` | `ata_do_dia` copia o tipo da grade; a persistência passa por `lavrar_veredito`. (`app/ponto/services/cartorio.py:568`; `app/escala/models.py:1787-1796`; `app/ponto/portas/celula.py:726-768`) | O juiz aplica a segunda cláusula. `grade_da_celula` mantém projeção própria de tipo; declaração mantém veto próprio por tipo. (`app/chamados/juizes.py:408`; `app/escala/services/leitor_celula.py:345-346`; `app/chamados/services/declaracao_texto.py:95`) |
| `ata.minutos_previstos` | Cartório grava `int(dia.get('minutos_previstos') or 0)` pela mesma porta. (`app/ponto/services/cartorio.py:569`; `app/ponto/portas/celula.py:726-768`) | O leitor usa o número da ata; se ausente, consulta `minutos_previstos_do_dia` somente quando `cel.trabalha` é verdadeiro, senão usa zero. O supra lê a projeção em `prev`. Esses passos não consultam `dia_sem_trabalho`. (`app/escala/services/leitor_celula.py:347-354`; `app/ponto/supra_juiz.py:223`) |
| `ata.lampadas` | `lampadas_do_dia` monta; `ata_do_dia` incorpora; dispensa por ausência/justificativa e teto podem alterar as luzes antes da lavra. (`app/ponto/services/cartorio.py:541-569`, `:611-624`, `:707-717`) | Materialização lê slots/luzes; cluster conta apagadas; intervalo indexa lâmpadas; leitor projeta células; `lastro` procura os marcos cobrados. São perguntas sobre os marcos, adicionais ao gate de dia sem trabalho. (`app/chamados/services/materializacao.py:158-164`; `app/ponto/services/cluster_espurio.py:29-31`; `app/ponto/management/commands/detectar_intervalo_ausente.py:64`; `app/escala/services/leitor_celula.py:92-121`; `app/ponto/services/lastro.py:82-95`) |
| `INSUMO_DIA_SEM_TRABALHO` | Constante do supra; escrita pelo adaptador e por `grade_da_celula`. Cartório usa o adaptador nos dois ramos. (`app/ponto/supra_juiz.py:57-62`; `app/escala/services/leitor_celula.py:370`; `app/ponto/services/cartorio.py:1029`, `:1213`) | `_dia_sem_trabalho_pelo_juiz` exige a chave e entrega `trabalha` e `ata` ao juiz, sem predicado substituto. (`app/ponto/supra_juiz.py:65-71`) |
| `chamados_vivos` | Commands passam `len(vivos)`, selecionados por `VIVOS`; cartório normal passa `len(_protestam)`; `nunca_bateu` passa zero. (`app/chamados/catalogo/motor.py:24`; `app/ponto/management/commands/supra_juiz.py:194-208`; `app/ponto/management/commands/emitir_furo_retroativo.py:92-101`; `app/ponto/services/cartorio.py:1029`, `:1206-1214`) | O supra exige contagem verdadeira nesse ramo. A contagem dos commands não consulta o juiz de dia; a do cartório passa pelo predicado próprio `_protesta`. (`app/ponto/supra_juiz.py:470-472`; `app/ponto/services/cartorio.py:1206-1208`) |
| `cobertura_status` | Vem de `ausencia_cobre(..., para='cobranca')`, por `getattr(cob, 'status', None)`, nos commands e no cartório. (`app/ponto/management/commands/supra_juiz.py:199-209`; `app/ponto/management/commands/emitir_furo_retroativo.py:97-102`; `app/ponto/services/cartorio.py:982`, `:1030`, `:1108`, `:1216`) | O ramo exige **exatamente `None`**, não apenas “não aprovada”. É guarda de cobertura, não outro juiz de dia sem trabalho. (`app/ponto/supra_juiz.py:471`) |
| Novos `Pauta.texto`/`ancora_id` | Fábrica constrói `colaborador_id:AAAA-MM-DD`, marcador e tags; chama `escrever`, que normaliza, acrescenta contexto e persiste. Não há criação direta de Pauta pela fábrica. (`app/chamados/services/fabrica_da_celula.py:175-190`; `app/pautas/services.py:163-185`) | A fábrica relê âncora/texto para deduplicar; API expõe ambos; painel mostra texto e tenta navegar pela âncora; chip consulta a âncora. Não são decisões de cobrança. (`app/chamados/services/fabrica_da_celula.py:180-183`; `app/pautas/views.py:40-41`; `app/templates/core/_copiloto.html:405`, `:564-573`; `app/pautas/services.py:334-338`) |

A fábrica consulta o juiz na entrada, mas depois passa por `motivos_pela_ata → _ata_do_dia → _marcos_faltantes`, onde sobrevive o veto próprio `tipo_dia != 'trabalho'`. Portanto, delegar o primeiro gate não eliminou todos os predicados do caminho. (`app/chamados/services/fabrica_da_celula.py:49`, `:80`, `:111`; `app/chamados/services/declaracao_texto.py:79-96`)

Os leitores anteriores `_celula_furada`, `ChamadoColaborador` e `reconciliar_grade` também consultam o juiz; possuem seus próprios universos e guardas ao redor da consulta. (`app/chamados/juizes.py:411-443`; `app/chamados/models.py:1227-1232`; `app/ponto/management/commands/reconciliar_grade.py:274-282`)

Nos testes, há escrita direta de `trabalha/ata` para construir as situações adversariais; isso não representa um novo escritor de produção. (`app/chamados/tests/test_f1c4_3_emissores.py:65-66`; `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:43`, `:100-101`)

**2. Produtores do dict, ausência de insumo e idade da ata**

| Caminho | Produção do dict e ata entregue |
|---|---|
| Cron/command `supra_juiz` | Usa `grade_da_celula`: insumo com `cel.trabalha` e **ata já gravada**. (`app/ponto/management/commands/supra_juiz.py:116`, `:206-209`; `app/escala/services/leitor_celula.py:332`, `:370`) |
| `emitir_furo_retroativo` | Mesmo leitor e **ata já gravada**. (`app/ponto/management/commands/emitir_furo_retroativo.py:59`, `:99-102`; `app/escala/services/leitor_celula.py:332`, `:370`) |
| Cartório, `nunca_bateu` | Monta `_ata_nb` neste julgamento, entrega-a ao helper e depois à lavra. (`app/ponto/services/cartorio.py:1012`, `:1029`, `:1044-1045`) |
| Cartório, normal | Monta `_ata_agora.ata`, publica em `_tl.atas` e entrega esse objeto ao helper antes da lavra final. (`app/ponto/services/cartorio.py:1184-1188`, `:1213`, `:1563-1565`) |
| Evento `julgar_celula` | Carrega a célula e chama `julgar_colab`; desemboca nos mesmos dois ramos, não cria outro adaptador de dict. (`app/ponto/services/cartorio.py:1708-1723`, `:777`) |

O leitor produz `fora_vinculo` e `sem_celula` **sem insumo**, mas ambos são barrados por `SEM_JURISDICAO` antes do acesso obrigatório. Com célula no ramo normal do leitor, a chave é acrescentada. (`app/escala/services/leitor_celula.py:340-370`; `app/ponto/supra_juiz.py:48`, `:470-472`)

Nos caminhos de produção identificados, não encontrei um dict sem insumo que alcance esse acesso: são os dois commands e as duas chamadas do cartório. A catraca declara esses mesmos arquivos. Ausência efetiva de `KeyError` em produção: **[não verificado]**. (`app/ponto/management/commands/supra_juiz.py:206`; `app/ponto/management/commands/emitir_furo_retroativo.py:99`; `app/ponto/services/cartorio.py:1028`, `:1212`; `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:161-165`)

Nos testes existem estes modos de produção do dict: [nao verificado]

- Pelo leitor real, após alteração da célula: recebe ata gravada na fixture e insumo do leitor. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:39-45`)
- Pelo cartório real, com espião no adaptador: recebe ata corrente. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:89-104`)
- Pelo adaptador em fixtures de folga/indefinido. (`app/ponto/tests/test_f1c4_3_supra_indefinido_nao_retrata.py:17-24`; `app/ponto/tests/test_supra_juiz.py:231-235`; `app/ponto/tests/test_supra_juiz_cobranca.py:102-105`)
- Por dict manual com insumo explícito, inclusive tipo externo diferente do tipo na ata. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:147-155`)
- Por dict manual sem insumo: deliberadamente para exigir `KeyError`, testar `SEM_JURISDICAO` ou exercitar caminhos barrados por previsto/chamados/cobertura. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:135-145`; `app/ponto/tests/test_supra_juiz_cobranca.py:107-119`; `app/ponto/tests/test_supra_juiz_indefinido.py:21-48`)

**`tipo_dia='trabalho'` não protege mais um dict sem insumo**: previsto zero, chamados vivos e cobertura `None` fazem esse dict chegar ao acesso obrigatório. A proteção por tipo agora cobre somente `SEM_JURISDICAO`. (`app/ponto/supra_juiz.py:470-472`)

**3. Sítios que conservam outra decisão**

| Sítio | Predicado e universo | Equivalência ao juiz |
|---|---|---|
| Cartório `_protesta` | `trabalha is not False and marcos_acesos(...) is not True`, sobre chamados vivos; também participa de `cobrado`, adiamento e reconciliação. (`app/ponto/services/cartorio.py:1206-1214`, `:1250`, `:1281-1287`) | **Não**: ignora `ata.tipo_dia` e considera satisfação dos marcos. (`app/ponto/services/cartorio.py:1207`; `app/chamados/juizes.py:408`) |
| Cartório, emissão total | Exige `ec.eh_dia_trabalho(data) is True`, depois dos gates de furo, situação, piso e afastamento. (`app/ponto/services/cartorio.py:1492-1507`) | **Não**: com célula, essa autoridade lê `trabalha`, sem a segunda cláusula da ata. (`app/escala/models.py:1066-1073`) |
| Cartório `_marcos_do_furo_parcial` | Recusa vínculo ausente ou `eh_dia_trabalho is not True`. (`app/ponto/services/cartorio.py:1604-1609`) | **Não**, pela mesma distinção entre previsão e ata. (`app/escala/models.py:1071-1073`) |
| Declaração `_marcos_faltantes` | Só aceita tipo projetado `trabalho`; lê dia e véspera, desta aproveitando somente saída. (`app/chamados/services/declaracao_texto.py:79-96`, `:116-123`) | **Não**: não aplica a cláusula soberana quando a ata diz trabalho; universo inclui véspera. (`app/chamados/services/declaracao_texto.py:88-96`; `app/chamados/juizes.py:408`) |
| Disputa, identificação do dia | Só aceita `tipo_dia == 'trabalho'` na grade da data da entrada aberta. (`app/chamados/services/disputa_emissao.py:56-70`) | **Não**: decide pelo tipo projetado, sem consultar o juiz. (`app/chamados/services/disputa_emissao.py:65-68`) |
| Disputa `_tem_base` | Aceita batida primeiro; senão previsão `is not False`. (`app/chamados/services/disputa_emissao.py:749-753`) | **Não**: aceita `None` e não consulta o tipo da ata nesse gate. (`app/chamados/services/disputa_emissao.py:750-753`) |
| Lote de perguntas | Pré-seleciona somente `trabalha=True` na janela. (`app/chamados/management/commands/disparar_perguntas_competencia.py:74-76`) | **Não**: `None/trabalho` fica fora antes dos demais filtros. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`; `app/chamados/juizes.py:408`) |
| Fila por causa | Dias previstos são células `trabalha=True` dos colaboradores da fila. (`app/chamados/services/fila_por_causa.py:191-200`) | **Não**: considera somente o campo, sem ata. (`app/chamados/services/fila_por_causa.py:192-194`) |
| Vigia de hora | Candidatas `trabalha=True`, empresa, hoje/ontem e janela do turno. (`app/ponto/services/vigia_de_hora.py:15-26`) | **Não**: `None` não chega ao julgamento por essa seleção. (`app/ponto/services/vigia_de_hora.py:21-23`) |
| Vigia de entrada | Somente `trabalha=True` e ata com lâmpadas, na janela ou data consultada. (`app/ponto/services/vigia_entrada.py:73-85`, `:120-125`) | **Não**: mesmo recorte por `True`; é instrumento de leitura. (`app/ponto/services/vigia_entrada.py:79`, `:120`) |
| `dia_do_colab` | `not cel.trabalha` produz “Dia sem trabalho previsto”. (`app/ponto/services/dia_do_colab.py:121-123`) | **Não**: inclui `None` nessa descrição. (`app/ponto/services/dia_do_colab.py:121`; `app/chamados/juizes.py:408`) |
| `passivo_s84` | Célula truthy vira TRABALHO; `False/None` viram FOLGA, sobre batidas atribuídas ao dia-turno. (`app/ponto/services/passivo_s84.py:88-96`) | **Não**: colapsa o indeterminado em folga; a classificação participa da seleção posterior. (`app/ponto/services/passivo_s84.py:96`, `:108-112`) |
| Veto em `ponto.marcos` | `trabalha is False` dá FOLGA; depois considera vereditos vetados. (`app/ponto/marcos.py:155-163`) | Só coincide com a primeira cláusula; responde ao veto, com outro conjunto de condições. (`app/ponto/marcos.py:159-162`) |
| `flip_auto` | Célula `False` recusa dia diurno; no noturno exige véspera `True` para recuar. (`app/ponto/services/flip_auto.py:201-215`) | **Não**: decisão geométrica entre dia/véspera, sem tipo da ata. (`app/ponto/services/flip_auto.py:208-215`) |
| Worklist | Tipo projetado diferente de trabalho retorna gap indeterminado antes da leitura das lâmpadas. (`app/ponto/worklist.py:18-26`) | **Não**: não aplica a primeira cláusula soberana. (`app/ponto/worklist.py:19-22`) |
| Bordas do realizado | Só tipo projetado `trabalho`, entre vínculos ativos e dentro da janela. (`app/ponto/services/bordas_realizado.py:62-75`) | **Não**: universo de auditoria do realizado. (`app/ponto/services/bordas_realizado.py:76-87`) |
| Outros ramos do supra | Conservam gates por tipo; `dia_de_cobranca` admite ausência parcial. (`app/ponto/supra_juiz.py:281`, `:304-308`, `:402-420`, `:443`, `:476-491`) | **Não**: não foram todos migrados; ausência parcial pode ser cobrável ali enquanto o juiz cala ata `ausencia`. (`app/ponto/supra_juiz.py:299-305`; `app/chamados/juizes.py:408`) |

Há ainda recortes próprios nos instrumentos: auditor de chamados (`not trabalha`/`is not True`), esmeril (`is False`/truthy), tripwire (`True`, ou `False/None` mais ata folga), `dna_x_batida_real` e `plano_b_no_dinheiro` (`True`). Seus universos são invariantes e diagnósticos, não a mesma pergunta de cobrança do juiz. (`app/chamados/management/commands/auditar_invariantes_chamados.py:376`, `:1443`, `:1542`; `app/ponto/services/esmeril_espelho.py:71`, `:121-124`; `app/ponto/management/commands/tripwire_celulas.py:98`, `:257`, `:338-344`; `app/ponto/management/commands/dna_x_batida_real.py:58-65`; `app/ponto/management/commands/plano_b_no_dinheiro.py:142-144`)

Os leitores de **previsão** também precisam permanecer distinguidos: alerta avançado usa resultado falsey; retroativo exige `True`; detector de anomalias trata `None` como indeterminação e `False` como folga; faltas de hoje aceita `True` ou defeito cadastral; plantio overnight verifica `False`. Eles consultam a autoridade prevista, que, havendo célula, retorna `cel.trabalha`. (`app/ponto/management/commands/processar_alertas_avancados.py:34-50`; `app/ponto/management/commands/emitir_furo_retroativo.py:117-129`; `app/chamados/detector_anomalias.py:164-170`; `app/ponto/services/faltas_de_hoje.py:32-42`; `app/ponto/management/commands/vigia_plantio_overnight.py:38-50`; `app/escala/models.py:1071-1073`)

`detectar_ausencias` conserva tratamento específico do limbo previsto; propositor exige tipo trabalho em dicts; simulação e `veredito_celula` julgam grade remontada. Não são consultas equivalentes ao juiz da cobrança lavrada. (`app/ponto/management/commands/detectar_ausencias.py:136-159`; `app/ponto/propositor.py:34-37`, `:201-211`; `app/ponto/services/simular_dia.py:5-12`; `app/ponto/services/veredito_celula.py:216-222`)

A documentação reconhece parte dessa dívida em `F1C4-3-NONE-LEITORES` e `F1C4-3-EMISSORES-FILA`; não sustenta que todos esses leitores tenham migrado. (`app/docs/BACKLOG.md:805`, `:823`)

**4. Os dois testes novos** [nao verificado]

| Teste | Mutação mínima no cartório que deve deixá-lo vermelho, por análise estática | [nao verificado]
|---|---|
| `test_MORDE_ramo_nunca_bateu_entrega_a_ata_deste_julgamento` | Trocar `ata=_ata_nb` por `ata=cel.ata` em `app/ponto/services/cartorio.py:1029`: entrega a sentinela e tipo `folga`, contrariando as asserções. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100-111`, `:116-119`) |
| `test_MORDE_ramo_normal_entrega_a_ata_deste_julgamento` | Trocar `ata=_ata_agora.ata` por `ata=cel.ata` em `app/ponto/services/cartorio.py:1213`: falha pelas mesmas verificações e pela identidade com a ata em curso. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100-111`, `:121-132`) |

**Nenhum passa se o espião nunca for chamado**: `assertTrue(vistos)` antecede o laço. **Nenhum passa recebendo a ata anterior intacta**: ambos rejeitam a sentinela e exigem tipo `trabalho`. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100-114`)

Não passam por fixture vazia quanto ao **transporte observado**: exigem chamada, conteúdo e substituição da ata gravada; o normal também exige identidade com `_tl.atas`. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:107-114`, `:132`)

Entretanto, não demonstram a execução do ramo de cobrança nem a chamada efetiva ao juiz: `nunca_bateu` passa zero vivos; a fixture normal começa com célula de trabalho e jornada prevista. (`app/ponto/services/cartorio.py:1029`; `app/ponto/supra_juiz.py:471-472`; `app/ponto/tests/test_bo_inss_nunca_bateu_le_a_precedencia.py:72-90`)

O teste `nunca_bateu` é mais fraco quanto à integridade da ata: trocar seu argumento por `{'tipo_dia': 'trabalho'}` satisfaz as verificações de conteúdo, mesmo omitindo minutos e lâmpadas. O normal exige identidade com a ata em curso, mas nenhum dos dois compara integralmente o insumo com a ata persistida. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:106-114`, `:132`)

Execução dos testes Django e dos mutantes completos: **[não verificado]**. [nao verificado]

**5. Correspondência entre documentos e código**

- **Correto:** migração do ramo no mesmo pouso, guarda `SEM_JURISDICAO` antes do juiz e transporte pelo insumo. (`app/docs/CORTES.md:415`; `app/docs/JUIZES.md:58-59`; `app/ponto/supra_juiz.py:470-472`)
- **Correto:** ata deste julgamento nos dois ramos do cartório. (`app/docs/CORTES.md:415`; `app/ponto/services/cartorio.py:1012-1029`, `:1184-1213`)
- **Correto com qualificação:** `_protesta` conserva somente a primeira cláusula **sobre a natureza do dia**; a função inteira também pergunta por marcos acesos. “Zera chamados vivos” vale para `trabalha=False`, não incondicionalmente. (`app/docs/BACKLOG.md:807`; `app/docs/JUIZES.md:60-61`; `app/core/juizes.py:144-146`; `app/ponto/services/cartorio.py:1206-1214`)
- **Imprecisão contradita pelos casos vazios:** a fórmula documental `ata.tipo_dia != trabalho` omite a guarda de preenchimento. Para `True/None` com tipo `None/''`, o juiz não cala. (`app/docs/CORTES.md:415`; `app/docs/JUIZES.md:56`; `app/core/juizes.py:138`; `app/chamados/juizes.py:408`)
- **Imprecisão se lida literalmente:** “`trabalha=None` COBRA” não vale quando a segunda cláusula é verdadeira. `None` com ata `folga`, `ausencia`, `falta` ou `indefinido` é dia sem trabalho para o juiz. (`app/docs/JUIZES.md:57`; `app/core/juizes.py:139`; `app/chamados/juizes.py:408`)
- **Ressalva temporal ausente em `_C18`:** “célula JÁ LAVRADA” não descreve esse leitor do cartório, que consulta a ata corrente antes da persistência final; `CORTES` descreve essa exceção corretamente. (`app/core/juiz

[truncado em 20000 de 24186 caracteres]

## 31/10 16:38 · sessao 1d6ef70ea0d8 · - · - · origem=Ronald (tmux)

1. **Situação do instrumento.** O código atual é B3 por **obra**, alimentado por veredictos de transcripts e triagens Codex; o disparo usa `n>=1` reprovação final ou `nc>=3` voltas procedentes, sem entidades ou janela de desenhos (`bin/disjuntor.py:736`, `bin/disjuntor.py:862`, `bin/disjuntor.py:896`, `bin/disjuntor.py:912`). Não existem nessa transição os campos `visto_em`, entidade, eventos nomeados ou consumo de `PADRAO-DE-CIMA`; o estado manipulado é `b3`, `b3_sem_obra`, reprovações, Codex e liberações por obra (`bin/disjuntor.py:891`, `bin/disjuntor.py:936`). Resultado executado do instrumento novo: [não verificado].

2. **Quem chama / quem espera.** Há pontos de integração: PreToolUse chama `main_pretool` pela configuração; Stop chama `julga_b3` para acrescentar recados; wrapper pergunta diretamente `--b3-parada` depois de obter slot e trava da resposta (`.claude/settings.json:68`, `bin/hook_stop_fila1.py:1048`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:165`). Hoje o Agent alcançado por B3 é somente `subagent_type=verificador` cujo prompt passa `_eh_final`; construtor e verificador de desenho não são alcançados por esse ramo (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:499`). Portanto reutilizar o ramo sem ampliá-lo deixa passar os Agents que o contrato novo pretende bloquear (`bin/disjuntor.py:1454`).

3. **Evento / tempo.** `_topo_consistente` já lê o RELATO inteiro duas vezes e verifica bytes/stat, mas devolve somente as duas primeiras linhas úteis recortadas por `_bloco_do_topo`; serve à liberação atual, não à varredura de todos os eventos vivos (`bin/disjuntor.py:662`, `bin/disjuntor.py:679`, `bin/disjuntor.py:686`, `bin/hook_stop_fila1.py:415`, `bin/hook_stop_fila1.py:439`). `_sha_b3` fornece SHA-256 truncado a 16 hex após `strip().lstrip('*_`> ')`; é normalização específica do topo, não definição pronta para identidade imutável de evento (`bin/disjuntor.py:571`). A janela de 24h atual é apenas painel/trilha, baseada no timestamp da trilha, não `visto_em` de desenho (`bin/disjuntor.py:1548`, `bin/disjuntor.py:1563`). Contraexemplo hipotético: dois eventos válidos na terceira e quarta linhas do RELATO seriam invisíveis se o instrumento usar diretamente a saída de `_topo_consistente` (`bin/hook_stop_fila1.py:439`). Contraexemplo hipotético de identidade: `DESENHO OBRA-X: ...` e `> DESENHO OBRA-X: ...` têm o mesmo hash nessa função; se `>` deveria ser semanticamente diferente, a normalização precisa ser especificada (`bin/disjuntor.py:573`).

4. **Disparo / liberação.** Existem a trava única, gravação por temporário+`os.replace`, anúncio após gravação e reversão da liberação se não gravou; são infraestrutura reutilizável (`bin/disjuntor.py:237`, `bin/disjuntor.py:271`, `bin/disjuntor.py:274`, `bin/disjuntor.py:948`, `bin/disjuntor.py:958`). O reconhecedor existente só aceita `DESENHO <obra>: lei=... pergunta=... <arquivo>:<linha>` ou `ERRO-BOBO`; não valida classe/padrão/armadilha/obra nem consulta GLOSSÁRIO/PLANO (`bin/disjuntor.py:626`, `bin/disjuntor.py:639`, `bin/disjuntor.py:650`). O consumo atual é por **obra**: `b3[obra].consumidas`; não é consumo global do SHA de um episódio por entidade (`bin/disjuntor.py:909`, `bin/disjuntor.py:932`, `bin/disjuntor.py:939`). Contraexemplo hipotético: se uma mesma linha de liberação nomear várias obras, o consumidor atual pode consumi-la uma vez em cada `b3[obra]`, porque o histórico consumido pertence a cada obra (`bin/disjuntor.py:637`, `bin/disjuntor.py:939`).

5. **Falha/NAO_SEI.** O contrato está desatualizado ao afirmar que a exceção do envelope hoje libera todo ramo: o B3 já fecha a exceção com negativa por `_ramo_b3`; outros ramos ainda falham abertos (`bin/disjuntor.py:1496`, `bin/disjuntor.py:1498`, `bin/disjuntor.py:1506`). O comportamento B3 atual excede o escopo proposto: `julga_b3` retorna `{}`+motivo em qualquer exceção e `estado_b3` devolve `NAO_SEI` para **qualquer obra consultada** se há motivo; não restringe a obras do último estado legível (`bin/disjuntor.py:977`, `bin/disjuntor.py:982`, `bin/disjuntor.py:989`, `bin/disjuntor.py:992`). `ler_estado` não mantém cópia anterior recuperável e `_com_estado` preserva o JSON ilegível, levantando erro (`bin/disjuntor.py:259`, `bin/disjuntor.py:265`, `bin/disjuntor.py:291`). Contraexemplo hipotético: último estado legível disparou apenas `OBRA-X`; JSON vira `{lixo`; consulta verificador de `OBRA-Y` também recebe NAO_SEI/recusa pelo B3 atual (`bin/disjuntor.py:982`, `bin/disjuntor.py:993`, `bin/disjuntor.py:1046`). Para implementar o limite novo falta uma fonte durável recuperável de `obras_em_entidades_disparadas`; seu escritor/leitor ainda não existe nas funções de estado examinadas (`bin/disjuntor.py:237`, `bin/disjuntor.py:291`).

6. **Medida.** Há `dispara` para trilha padronizada, `ler` para agregações 24h e `linha` para painel; ESTADO importa ambos diretamente (`bin/disjuntor.py:139`, `bin/disjuntor.py:1546`, `bin/disjuntor.py:1677`, `bin/gerar_estado.py:280`). O painel enumera IDs fixos B1/B2/B3/B4/B4-CODEX/ANEXO-ENXUTO; adicionar só um novo evento à trilha não o faz aparecer nessa enumeração, e não há desenhos por entidade ou eventos None (`bin/disjuntor.py:1679`, `bin/disjuntor.py:1615`). Escritores atuais de métricas: `trilha`/`dispara` e anúncio da decisão; leitores: `ler`/`linha` e gerador de ESTADO (`bin/disjuntor.py:128`, `bin/disjuntor.py:139`, `bin/disjuntor.py:948`, `bin/disjuntor.py:1554`, `bin/gerar_estado.py:281`).

7. **Desliga — falha concreta na porta atual.** `desligado` já usa `lexists` para a chave global; PreToolUse e Stop respeitam-na (`bin/disjuntor.py:144`, `bin/disjuntor.py:1487`, `bin/hook_stop_fila1.py:1039`). Porém o wrapper sempre consulta B3 para verificador, e o caminho CLI → `b3_parada` → `estado_b3` → `decide_b3` não consulta a chave (`bin/codex_pergunta.sh:164`, `bin/disjuntor.py:1709`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:989`, `bin/disjuntor.py:877`). Contraexemplo hipotético: chave existe, `b3["OBRA-X"]={"reprovados":["a1"],"disparado":true}`, fontes legíveis; o hook fica desligado, mas wrapper recusa OBRA-X com rc3 (`bin/disjuntor.py:1487`, `bin/disjuntor.py:1043`, `bin/codex_pergunta.sh:165`). A proteção da chave é textual e muito ampla: até Bash meramente contendo `DISJUNTOR_DESLIG` é negado; não autentica autoria Ronald por credencial (`bin/disjuntor.py:160`). Não prometer “só Ronald” como propriedade de segurança forte (`bin/disjuntor.py:152`).

8. **Mapa de estado existente e proposta explicitamente ausente.**
   - Estado físico: leitura/escrita exclusivamente pela infraestrutura `_com_estado` e leitura independente `ler_estado`; trava `.lock`, temporário e substituição atômica (`bin/disjuntor.py:247`, `bin/disjuntor.py:249`, `bin/disjuntor.py:260`, `bin/disjuntor.py:271`, `bin/disjuntor.py:291`).
   - `b3_limiar`: `_migra_limiar` lê e escreve (`bin/disjuntor.py:699`, `bin/disjuntor.py:710`).
   - `b3_sem_obra`: `decide_b3` acumula; `_b3_sem_obra` lê para painel (`bin/disjuntor.py:892`, `bin/disjuntor.py:895`, `bin/disjuntor.py:1624`).
   - `b3[obra].reprovados`: `decide_b3` acumula, mede e zera ao liberar; rearme também zera (`bin/disjuntor.py:900`, `bin/disjuntor.py:910`, `bin/disjuntor.py:938`, `bin/disjuntor.py:1020`).
   - `codex`: decisão recalcula da triagem, mede e zera; `_triagens` lê os IDs existentes para impedir redução da fonte (`bin/disjuntor.py:832`, `bin/disjuntor.py:907`, `bin/disjuntor.py:911`, `bin/disjuntor.py:938`).
   - `disparado`: decisão e rearme escrevem; decisão lê (`bin/disjuntor.py:913`, `bin/disjuntor.py:922`, `bin/disjuntor.py:938`, `bin/disjuntor.py:1020`).
   - `disparado_em`, `topo_no_disparo`: `_fotografa` escreve; decisão lê (`bin/disjuntor.py:727`, `bin/disjuntor.py:730`, `bin/disjuntor.py:927`, `bin/disjuntor.py:934`).
   - `consumidas`: decisão lê e acrescenta SHA por obra (`bin/disjuntor.py:932`, `bin/disjuntor.py:939`).
   - `liberado`, `rearmado`: decisão/liberação, migração e rearme escrevem; acumulação e recálculo leem para excluir IDs antigos (`bin/disjuntor.py:708`, `bin/disjuntor.py:898`, `bin/disjuntor.py:905`, `bin/disjuntor.py:936`, `bin/disjuntor.py:1018`).
   - `b3_triagem_linhas`: escritor de triagem incrementa, `_triagens` lê e atualiza (`bin/disjuntor.py:815`, `bin/disjuntor.py:829`, `bin/disjuntor.py:853`).
   - Campos **novos propostos**, ainda sem escritor/leitor: registro por SHA com `visto_em/obra/entidades/nomeado_em_episodio`; episódios por entidade com obras alcançadas; consumo global de SHA de nomeação; contadores por entidade/None; cópia recuperável de último escopo legível. Nenhuma dessas estruturas pertence ao schema B3 manipulado na decisão atual; funções futuras/nomes exatos: [não verificado] (`bin/disjuntor.py:881`, `bin/disjuntor.py:891`, `bin/disjuntor.py:936`).

9. **MORDE.** A suíte atual declara casos B3/Codex e mutantes de reconhecimento, atomicidade, NAO_SEI e porta, mas sua lista de execução não inclui casos PADRAO-DE-CIMA por entidade (`bin/tests/test_disjuntor.sh:1468`, `bin/tests/test_disjuntor.sh:1626`, `bin/tests/test_disjuntor.sh:1757`). Os testes criam diretórios/arquivos e mutantes: não executá-los sob a exigência de somente leitura (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:1478`, `bin/tests/test_disjuntor.sh:1487`). Resultado de teste e mutante global do instrumento novo: [não verificado]. Os casos necessários do contrato devem fixar estados explícitos: entidades E1/E2, mesmo programa P1, SHA distintos, dois `visto_em` dentro de 86400s, pelo menos um `nomeado=false`; com E1/E2 não dispara, com E1/E1 dispara e mutação que remove chave entidade deve tornar primeiro caso vermelho. Implementação desses testes hoje: [não verificado].

10. **Observação adicional para consolidação.** O parser já extrai `CODEX_OBRA` da atribuição imediata em `_codex_cmds`, contrariando o anexo; o docstring da função ficou desatualizado ao listar retorno sem `obra` (`bin/disjuntor.py:1325`, `bin/disjuntor.py:1326`, `bin/disjuntor.py:1337`). Não há AGENTS.md na árvore auditada pelo comando de localização; aplicabilidade de instruções externas: [não verificado].

## 31/10 16:38 · sessao 657183ad6699 · - · - · origem=Ronald (tmux)

1. **Entidade: material reutilizável existe, resolvedor do contrato ainda precisa ser construído.** `varrer_arvore()` entrega `{arquivo, linha, entidade, op, expr}`; caminhos são relativos a `app/`. Não entrega nome da função, intervalo da função, `end_lineno` ou atributo `DIRETA`; `_anotar()` guarda apenas linha, entidade, operação e expressão. A resolução de “arquivo:linha → entidade direta naquela linha/função” exige adaptador e, para referência dentro de função fora da linha de escrita, outro mapeamento AST. (`app/core/censo_escritas.py:361`, `app/core/censo_escritas.py:469`, `app/core/censo_escritas.py:489`)

2. **Separar atribuição direta de inferência é obrigatório.** Gerente explícito `Modelo.objects...` gera entidade diretamente; amarração a modelo e `self` também podem gerar expressão sem marcador. Inferências aparecem em `expr` como `[campo]`, `[relacionado]`, `[update_fields]`, `[nome]`; não existe flag estruturada de proveniência. Tratar todo `entidade != None` como DIRETA viola esse corte do contrato. (`app/core/censo_escritas.py:377`, `app/core/censo_escritas.py:406`, `app/core/censo_escritas.py:413`, `app/core/censo_escritas.py:420`, `app/core/censo_escritas.py:424`)

3. **Contraexemplo sintético de agrupamento direto correto.** Com `visto_em=agora−60/−30`, ambos ainda não nomeados:
   ```
   DESENHO OBRA-A: pergunta=x app/ponto/registro_batida.py:152
   DESENHO OBRA-B: pergunta=y app/ponto/registro_batida.py:157
   ```
   As duas escritas são `Batida.objects.create(...)`; a entidade correta é Batida para ambas e o contrato exige disparo. Trocar a segunda referência por `app/ponto/portas/celula.py:247` dá CelulaDia e exige ausência de disparo; um programa comum hipotético não muda isso. As escritas e a atribuição direta estão em `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/portas/celula.py:247`, `app/core/censo_escritas.py:377`. Estado e linhas de RELATO acima são sintéticos, **[não verificado]** em produção.

4. **Contraexemplo sintético para DIRETA.** `DESENHO OBRA-A: pergunta=x app/ponto/registro_batida.py:254` referencia `batida.save(update_fields=list(campos))`; o censo a atribui por nome da variável (`[nome]`), pois `update_fields` não é literal. Usar essa entidade como DIRETA pode agrupar indevidamente com outro evento Batida e impedir o fallback para programa; o filtro correto precisa descartar essa proveniência. (`app/ponto/registro_batida.py:254`, `app/core/censo_escritas.py:433`, `app/core/censo_escritas.py:424`) Ocorrência em RELATO/produção **[não verificado]**.

5. **`modelos()` satisfaz literalmente o vocabulário de classes do contrato, com uma limitação já demonstrada pelos testes.** Aceita classe de topo com alguma base em `*/models.py` ou `*/models/*.py`, excluindo nomes terminados em `QuerySet`/`Manager`; não verifica registro Django. Assim inclui `ResolvedoraCruzada(ValueError)`. Duas linhas sintéticas que a citem podem disparar uma entidade que é exceção, não modelo Django; isso decorre do próprio critério literal `modelos()` escolhido no contrato. (`app/core/censo_escritas.py:89`, `app/core/censo_escritas.py:95`, `app/core/censo_escritas.py:108`, `app/chamados/models.py:2179`, `app/core/tests/test_censo_escritas.py:264`, `app/core/tests/test_censo_escritas.py:289`)

6. **Fallback obra → PROGRAMA carece de resolvedor e gramática única.** O BACKLOG contém relações em prosa diferentes: “complemento”, “linha da E4 do PROGRAMA…” e itens com nome do programa. “CALENDARIO-HOJE-MINIMO” diz apenas “1o do programa”; extrair associação só por busca de token `PROGRAMA-...` perde essa ligação. A obra PADRAO-DE-CIMA está registrada como construção futura sobre B3. (`app/docs/BACKLOG.md:771`, `app/docs/BACKLOG.md:772`, `app/docs/BACKLOG.md:773`, `app/docs/BACKLOG.md:774`, `app/docs/BACKLOG.md:787`) Associação em estado de produção **[não verificado]**.

7. **Há validador de vocabulário aproveitável, mas dentro de selo shell, sem API runtime do instrumento.** `vocabulario()` está no heredoc Python de `bin/tests/test_lei_akita.sh`; lê as duas primeiras colunas do GLOSSARIO, negritos do NORTE e nomes em itálico dos ADRs do PLANO. Normaliza caixa, espaços e Markdown, compara termos inteiros, separa alternativas e alguns nomes em parênteses. Sua leitura usa `git show` do commit julgado, não o arquivo vivo; reutilizá-lo sem decidir essa diferença pode rejeitar padrão já editado no arquivo e ainda não commitado. (`bin/tests/test_lei_akita.sh:65`, `bin/tests/test_lei_akita.sh:74`, `bin/tests/test_lei_akita.sh:75`, `bin/tests/test_lei_akita.sh:80`, `bin/tests/test_lei_akita.sh:89`, `bin/tests/test_lei_akita.sh:102`, `bin/tests/test_lei_akita.sh:112`, `bin/tests/test_lei_akita.sh:138`)

8. **“Nome clássico declarado” requer definição adicional no instrumento.** O validador existente só aceita `VOCAB` ou exceções `n/a`; não tem regra independente para declaração de nome clássico. O texto documental atual também restringe padrão novo a corte do Ronald; o contrato anexo introduz uma alternativa cuja forma de declaração não foi fornecida **[não verificado]**. (`bin/tests/test_lei_akita.sh:120`, `bin/tests/test_lei_akita.sh:124`, `bin/tests/test_lei_akita.sh:138`, `app/docs/PLANO-PADROES.md:46`, `app/docs/PLANO-PADROES.md:53`)

9. **MORDE do censo não prova MORDE do novo instrumento.** Os testes existentes verificam distinção entre escrita direta, inferência e entidade desconhecida; são úteis como suporte do resolvedor. O teste “duas entidades distintas no mesmo programa não dispara” ainda pertence ao novo instrumento, cujo registro está como obra futura. (`app/core/tests/test_censo_escritas.py:25`, `app/core/tests/test_censo_escritas.py:80`, `app/core/tests/test_censo_escritas.py:92`, `app/core/tests/test_censo_escritas.py:110`, `app/docs/BACKLOG.md:787`) Nenhum teste foi executado porque os selos existentes podem criar arquivos; fiz somente leitura e uma consulta Python `-B` em memória **[não verificado]** quanto a execução dos selos.

## 31/10 16:38 · sessao 11a4f092d15a · - · - · origem=Ronald (tmux)

1. **Premissa desatualizada no contrato:** a cópia **já extrai `CODEX_OBRA`**. `_codex_cmds()` lê as atribuições anteriores à invocação por `_atribuicoes()` e retorna `obra`; o PreToolUse usa esse valor para negar a volta do verificador pelo B3. `bin/disjuntor.py:1309`, `bin/disjuntor.py:1325`, `bin/disjuntor.py:1335`, `bin/disjuntor.py:1337`, `bin/disjuntor.py:1440`.

2. **Há dois parsers com contratos distintos.**

   - **Comando Bash/CODEX_OBRA:** aceita qualquer valor textual em atribuição reconhecida, remove aspas por `shlex.split()`, mantém a última atribuição repetida e não aplica regex de nome de obra. Ex.: `CODEX_OBRA="O9 (C)"`, `CODEX_OBRA="qualquer nome / ?"` são extraídos literalmente. `bin/disjuntor.py:1309`, `bin/disjuntor.py:1314`, `bin/disjuntor.py:1318`, `bin/disjuntor.py:1337`.
   - **Ambiente efetivo do wrapper:** `OBRA="${CODEX_OBRA:-}"`, recusando apenas valor vazio; papel deve ser leitura/censo/verificador/sonda. Portanto o wrapper aceita nomes que `_obra_de()` não reconhece. `bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:45`, `bin/codex_pergunta.sh:48`.
   - **Prompt de Agent:** `_obra_de()` reconhece `obra X`, `Obra: X`, markdown em torno do nome, ou `leitura ... da X` com até 80 caracteres antes de `da`, escolhendo a primeira ocorrência válida. `bin/disjuntor.py:104`, `bin/disjuntor.py:105`, `bin/disjuntor.py:504`, `bin/disjuntor.py:514`.
   - O ID deve ser `[A-Z][A-Z0-9]*(?:-[A-Za-z0-9]+)+` ou `[A-Z]+\d+[A-Z0-9]*`. Assim aceita `PADRAO-DE-CIMA`, `OBRA-x`, `O9`, `A1B`, mas não `Obra-X`, `ABC`, `O9a`, `A1b`. Busca em `re.I` é seguida por validação sensível à caixa. `bin/disjuntor.py:97`, `bin/disjuntor.py:104`, `bin/disjuntor.py:518`, `bin/disjuntor.py:521`.
   - ID sem hífen preserva cauda `(letra)` ou outro ID com hífen, normalizada para maiúsculas: `O9 (c)` → `O9 (C)`; `O9 NOTURNO-FOO` é identidade completa. ID com hífen ignora essa cauda: `OBRA-X (C)` → `OBRA-X`. `bin/disjuntor.py:99`, `bin/disjuntor.py:518`, `bin/disjuntor.py:526`, `bin/disjuntor.py:529`.
   - Não há borda direita no token: `obra O9_A` vira `O9`, e `obra OBRA-X_Y` vira `OBRA-X`. É risco de colisão se o instrumento receber esses nomes. `bin/disjuntor.py:97`, `bin/disjuntor.py:103`, `bin/disjuntor.py:104`, `bin/disjuntor.py:515`.

3. **Limites concretos do parser de comando.**

   - `env CODEX_OBRA=OBRA-X CODEX_PAPEL=verificador bin/codex_pergunta.sh h q r` não é reconhecido: a regex permite atribuições **antes** dos prefixos; depois de `env` espera o caminho do wrapper. `bin/disjuntor.py:1243`, `bin/disjuntor.py:1244`, `bin/disjuntor.py:1245`.
   - `export CODEX_OBRA=OBRA-X; CODEX_PAPEL=verificador bin/codex_pergunta.sh h q r` é reconhecido com `obra=''`: o parser usa só atribuições do mesmo trecho da invocação, sem carregar ambiente anterior. `bin/disjuntor.py:1248`, `bin/disjuntor.py:1292`, `bin/disjuntor.py:1327`, `bin/disjuntor.py:1335`, `bin/disjuntor.py:1337`.
   - `CODEX_OBRA="$OBRA"` retorna o literal `$OBRA`, sem expansão de variável; o wrapper recebe o valor expandido pelo shell. Isso permite divergência entre a comparação do PreToolUse e a obra efetiva. `bin/disjuntor.py:1314`, `bin/disjuntor.py:1321`, `bin/codex_pergunta.sh:44`.
   - `bash -c "..."`, `$(...)`, script posterior e `codex exec` direto são resíduos explicitamente declarados pelo próprio código. `bin/disjuntor.py:1240`, `bin/disjuntor.py:1241`.
   - Hoje esses desvios não contornam **B3 pelo wrapper**, pois todo `PAPEL=verificador` consulta `--b3-parada "$OBRA"` depois de aguardar slot e trava da resposta. O instrumento novo precisa entrar também nessa porta efetiva, ou poderá ser contornado por ambiente/export/env. `bin/codex_pergunta.sh:137`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:164`, `bin/codex_pergunta.sh:165`.

4. **Contraexemplos para ação/escopo do contrato.**

   - Estado hipotético PADRAO: entidade `E` disparada, obras dos eventos `{OBRA-X}`; `Agent` construtor com `prompt="construa a obra OBRA-X"` passa hoje pelo ramo B3, que exige `subagent_type=verificador` e leitura final. Falta decisão PADRAO sobre construtor. `bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1457`.
   - Mesmo estado; `Agent` verificador de leitura curta passa pelo B3 porque `_eh_final()` exclui leitura de desenho; o contrato PADRAO não restringe verificador à final. `bin/disjuntor.py:114`, `bin/disjuntor.py:499`, `bin/disjuntor.py:1454`.
   - Mesmo estado; `export CODEX_OBRA=OBRA-X; CODEX_PAPEL=verificador ...` não oferece obra ao gate do PreToolUse: `_nega_b3()` só é chamado se `c.get('obra')`. Se PADRAO for implementado apenas nesse ponto, deixará passar a obra disparada. `bin/disjuntor.py:1337`, `bin/disjuntor.py:1442`, `bin/disjuntor.py:1443`.
   - `prompt="Compare obra OBRA-Y antes de construir obra OBRA-X"` resolve `OBRA-Y`; com `{OBRA-X}` disparada deixa passar a execução pretendida de X, com `{OBRA-Y}` disparada pode bloquear por mera referência. A primeira ocorrência manda. `bin/disjuntor.py:504`, `bin/disjuntor.py:510`, `bin/disjuntor.py:514`.

5. **O que quebra ao mudar o parser de nomes.**

   - `_obra_de()` alimenta a identificação dos verificadores finais nos transcripts, o gate de Agent e a classificação do ramo de exceção B3; uma mudança pode alterar tanto contagem quanto bloqueio e NAO_SEI. `bin/disjuntor.py:546`, `bin/disjuntor.py:557`, `bin/disjuntor.py:736`, `bin/disjuntor.py:744`, `bin/disjuntor.py:1415`, `bin/disjuntor.py:1416`, `bin/disjuntor.py:1455`.
   - `_OBRA_TOK`, `_obra_lida()` e `_identidade()` são compartilhados com `obras_da_linha()`, que valida a identidade no cabeçalho das linhas de liberação B3. Modificar só uma ponta pode contar uma identidade e liberar outra. `bin/disjuntor.py:100`, `bin/disjuntor.py:103`, `bin/disjuntor.py:518`, `bin/disjuntor.py:526`, `bin/disjuntor.py:533`, `bin/disjuntor.py:637`.
   - Estado e triagem usam a obra como chave literal; trocar normalização pode separar históricos antigos da identidade nova. O Codex mantém a identidade literal de `CODEX_OBRA`, e o Agent aplica `_identidade()`. `bin/disjuntor.py:808`, `bin/disjuntor.py:813`, `bin/disjuntor.py:850`, `bin/disjuntor.py:1013`, `bin/codex_pergunta.sh:199`.
   - Existem testes dessas diferenças: `O9` versus `O9 (C)`, markdown, caixa da cauda, exclusão de `O9a`, hífen sem cauda, concordância contador/topo, última atribuição e atribuição dentro de outro valor. Não foram executados porque os scripts criam arquivos; só exercitei funções puras com `python3 -B`. `bin/tests/test_disjuntor.sh:686`, `bin/tests/test_disjuntor.sh:688`, `bin/tests/test_disjuntor.sh:696`, `bin/tests/test_disjuntor.sh:697`, `bin/tests/test_disjuntor.sh:698`, `bin/tests/test_disjuntor.sh:699`, `bin/tests/test_disjuntor.sh:743`, `bin/tests/test_disjuntor.sh:1254`, `bin/tests/test_disjuntor.sh:1260`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1262`, `bin/tests/test_disjuntor.sh:1265`.

6. **Escritores/leitores relacionados ao wrapper e seus campos.**

   | Estado/artefato | Escritor | Leitor |
   |---|---|---|
   | `chamadas.log`: hora, obra, papel, hash12, pid, ini, pergunta raspada | `bin/codex_pergunta.sh:88` | `_linhas_chamadas`: `bin/codex_limite_estado.py:49`, `bin/codex_limite_estado.py:55`, `bin/codex_limite_estado.py:63`, `bin/codex_limite_estado.py:65` |
   | `chamadas.log`: fim, thread_id, rc, duração | `_saida`: `bin/codex_pergunta.sh:56`, `bin/codex_pergunta.sh:59` | `bin/codex_limite_estado.py:66`, `bin/codex_limite_estado.py:67`; últimas chamadas `bin/codex_limite_estado.py:82`; origem Code `bin/codex_limite_estado.py:92` |
   | `limite.log`: hora, obra, mensagem | `bin/codex_pergunta.sh:188` | `bin/codex_limite_estado.py:33`, `bin/codex_limite_estado.py:34` |
   | `<resposta>.id`: obra, papel, hash, pid, fim, sha | `bin/codex_pergunta.sh:198`, `bin/codex_pergunta.sh:199`, `bin/codex_pergunta.sh:201` | `_le_id`: `bin/disjuntor.py:753`, `bin/disjuntor.py:756`, `bin/disjuntor.py:760`; `_triagem_sob_trava`: `bin/disjuntor.py:795`, `bin/disjuntor.py:798`, `bin/disjuntor.py:800`, `bin/disjuntor.py:802` |
   | Trilha de triagem: hora, obra, volta, veredito | `_triagem_sob_trava`: `bin/disjuntor.py:810`, `bin/disjuntor.py:812`, `bin/disjuntor.py:813` | `_triagens`: `bin/disjuntor.py:823`, `bin/disjuntor.py:834`, `bin/disjuntor.py:846`, `bin/disjuntor.py:850` |
   | `disjuntor_estado.json:b3_triagem_linhas` | `_triagem_sob_trava`: `bin/disjuntor.py:815`; `_triagens`: `bin/disjuntor.py:853` | `_triagens`: `bin/disjuntor.py:829`, `bin/disjuntor.py:843` |
   | Campos novos PADRAO | Nenhum escritor apresentado nesses caminhos; definição/integração futura [não verificado] | Nenhum leitor apresentado nesses caminhos; definição/integração futura [não verificado] |

   O ESTADO agrega chamadas por `disjuntor.ler()` → `codex_limite_estado.ler_chamadas()` e publica o texto pelo gerador. `bin/disjuntor.py:1611`, `bin/disjuntor.py:1615`, `bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`.

## 31/10 16:35 · sessao f0bf8853260b · - · - · origem=Ronald (tmux)

Base para as questões 3 e 5, somente por leitura estática.

O juiz efetivo é `cel.trabalha is False or bool(ata.get('tipo_dia') and ata.get('tipo_dia') != 'trabalho')`: `None` e `''` na ata não bastam para calar; `trabalha=None` com ata não vazia diferente de trabalho cala. (`app/chamados/juizes.py:398-408`)

| Sítio | Predicado próprio e universo | Igual ao juiz? |
|---|---|---|
| `cartorio._protesta` | `trabalha is not False AND marcos_acesos(chamado, ata_atual) is not True`; aplicado a `vivos`, `_com_fio`, adiamento e reconciliação. (`app/ponto/services/cartorio.py:1206-1214`, `:1250`, `:1281-1287`) | Não: ignora a segunda cláusula e considera satisfação dos marcos. Para `trabalha=False`, todos os chamados deixam de protestar e `chamados_vivos=0`. (`app/ponto/services/cartorio.py:1207-1214`) |
| `cartorio` emissão total | `ec.eh_dia_trabalho(data) is True`, após código de furo/morta/limbo atual, situação, piso e afastamento. (`app/ponto/services/cartorio.py:1492-1507`) | Não: `eh_dia_trabalho` devolve `cel.trabalha` sem ler ata quando existe célula; sem célula calcula previsão. (`app/escala/models.py:1066-1073`) |
| `cartorio._marcos_do_furo_parcial` | Ausência de vínculo ou `eh_dia_trabalho is not True` impede nomear marcos apagados. (`app/ponto/services/cartorio.py:1604-1625`) | Não, pela mesma distinção entre previsão e ata. (`app/escala/models.py:1071-1073`) |
| `declaracao_texto._marcos_faltantes` | Grade lavrada do dia e véspera; `tipo_dia != 'trabalho'` pula o dia, inclusive indefinido; véspera contribui só saída. (`app/chamados/services/declaracao_texto.py:79-96`, `:116-123`) | Não: não vê `trabalha=False` quando ata diz trabalho; indefinido explícito fica calado; universo inclui véspera. (`app/chamados/services/declaracao_texto.py:88-96`, `app/chamados/juizes.py:408`) |
| `disputa_emissao` identificação do dia | Grade da data da entrada aberta, aceita só `tipo_dia == 'trabalho'`. (`app/chamados/services/disputa_emissao.py:56-70`) | Não: decide pelo tipo projetado da ata, sem primeira cláusula soberana. (`app/chamados/services/disputa_emissao.py:65-68`, `app/escala/services/leitor_celula.py:345-346`) |
| `disputa_emissao._tem_base` | Primeiro aceita batidas do turno; senão vínculo cujo `eh_dia_trabalho is not False`. (`app/chamados/services/disputa_emissao.py:749-753`) | Não: batida vence e previsão sem ata aceita None. (`app/chamados/services/disputa_emissao.py:750-753`, `app/escala/models.py:1071-1073`) |
| `disparar_perguntas_competencia` | Consulta só `trabalha=True` na janela; depois exige ativo e aplica outros filtros de emissão. (`app/chamados/management/commands/disparar_perguntas_competencia.py:74-76`, `:104-110`) | Não: exclui None/trabalho e consulta aceita True/ausência antes dos demais filtros. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `app/chamados/juizes.py:408`) |
| `fila_por_causa` | Dias previstos são células `trabalha=True`, desde `ini`, para colaboradores da fila. (`app/chamados/services/fila_por_causa.py:191-200`) | Não: fonte só trabalha, sem ata. (`app/chamados/services/fila_por_causa.py:192-194`) |
| `vigia_de_hora._candidatas` | `trabalha=True`, empresa, hoje e ontem; ontem ainda passa pela janela do turno. (`app/ponto/services/vigia_de_hora.py:15-26`) | Não: None não chega ao cartório por esta seleção. (`app/ponto/services/vigia_de_hora.py:21-23`) |
| `vigia_entrada` | Só `trabalha=True` e ata com lâmpadas, em janela passada ou data pedida. (`app/ponto/services/vigia_entrada.py:73-85`, `:120-125`) | Não: mesma divergência do filtro True, embora seja instrumento de leitura. (`app/ponto/services/vigia_entrada.py:79`, `:120`) |
| `dia_do_colab._veredito_do_dia` | `not cel.trabalha` produz “Dia sem trabalho previsto”; também compara faixa de precedência com trabalha truthy. (`app/ponto/services/dia_do_colab.py:117-136`) | Não: None é descrito como sem trabalho e ata não decide esse ramo. (`app/ponto/services/dia_do_colab.py:121-123`) |
| `passivo_s84` | Batidas vivas atribuídas ao dia-turno; célula truthy vira TRABALHO, False/None viram FOLGA. (`app/ponto/services/passivo_s84.py:70-96`) | Não: colapsa None em folga; essa classe alimenta seleção de retratação. (`app/ponto/services/passivo_s84.py:96`, `:108-112`) |
| `ponto.marcos` veto | `trabalha is False` dá FOLGA; em seguida considera vereditos vetados. (`app/ponto/marcos.py:155-163`) | Só a primeira cláusula; não ata. (`app/ponto/marcos.py:159-162`) |
| `flip_auto` | Para célula False, recusa folga diurna; no noturno pede véspera True e recua ao dia anterior. (`app/ponto/services/flip_auto.py:201-215`) | Não: problema geométrico com célula/véspera, sem ata tipo. (`app/ponto/services/flip_auto.py:208-215`) |
| `worklist.linha_da_ata` | Grade lavrada de um dia: `tipo_dia != trabalho` devolve gap indeterminado; só então lê lâmpadas. (`app/ponto/worklist.py:18-26`) | Não: tipo da ata sem cláusula trabalha. (`app/ponto/worklist.py:19-22`) |
| `bordas_realizado` | Grade lavrada dos vínculos ativos, só `tipo_dia == trabalho` em intervalo. (`app/ponto/services/bordas_realizado.py:62-75`) | Não; universo de auditoria do realizado, não emissão de cobrança. (`app/ponto/services/bordas_realizado.py:76-87`) |
| `supra_juiz` outros ramos | Mantém tipo trabalho nos ramos `:281`, `:402`, `:419`, `:443`, `:476`, `:491`; dia de cobrança inclui ausência parcial em `:304`. (`app/ponto/supra_juiz.py:281`, `:304-308`, `:402-420`, `:443`, `:476-491`) | Não são todos a pergunta migrada; ausência parcial é explicitamente cobrável ali, enquanto juiz cala ata ausência. (`app/ponto/supra_juiz.py:299-305`, `app/chamados/juizes.py:408`) |

Outros instrumentos sobre células também conservam recortes próprios: auditor de chamados `not trabalha`/`is not True` para invariantes, esmeril `is False` para A8/A2 e truthy para A4, tripwire filtro True para DNA/ata e filtro False/None mais ata folga para “folga mentindo”, sondas `dna_x_batida_real` e `plano_b_no_dinheiro` filtro True. São universos de diagnóstico, não equivalentes ao juiz de cobrança. (`app/chamados/management/commands/auditar_invariantes_chamados.py:376`, `:1443`, `:1542`; `app/ponto/services/esmeril_espelho.py:71-75`, `:121-124`; `app/ponto/management/commands/tripwire_celulas.py:96-98`, `:256-264`, `:338-344`; `app/ponto/management/commands/dna_x_batida_real.py:58-65`; `app/ponto/management/commands/plano_b_no_dinheiro.py:142-144`)

Há leitores da autoridade **prevista** que não devem ser apresentados como segundo juiz da ata: alerta avançado pula previsão falsey de ontem; retroativo exige previsão True; detector de anomalias devolve ok para previsão None e alerta folga para False; faltas de hoje aceita previsão True ou defeito de folga; plantio overnight acusa previsão False. Eles discordam do juiz de cobrança em células com ata contrária ou em None, mas o método chamado é a autoridade prevista registrada. (`app/ponto/management/commands/processar_alertas_avancados.py:34-50`; `app/ponto/management/commands/emitir_furo_retroativo.py:117-129`; `app/chamados/detector_anomalias.py:164-170`; `app/ponto/services/faltas_de_hoje.py:32-42`; `app/ponto/management/commands/vigia_plantio_overnight.py:38-50`; `app/escala/models.py:1048-1051`, `:1071-1073`)

`detectar_ausencias` também mantém tratamento próprio do limbo de previsão e defeito cadastral nas passadas de hoje/ontem; não resume sua regra a dia sem trabalho lavrado. (`app/ponto/management/commands/detectar_ausencias.py:136-159`)

`propositor.dia_degenerado` e `template_suspeito` exigem tipo trabalho, mas recebem dicts de diferentes montadores; `simular_dia.dia_perfeito` e o gate de `veredito_celula` leem uma grade simulada remontada, portanto não são leitura exclusiva da ata persistida. (`app/ponto/propositor.py:34-37`, `:201-211`; `app/ponto/services/simular_dia.py:5-12`; `app/ponto/services/veredito_celula.py:216-222`)

Conferência documental:

- A exceção `_protesta` está declarada e corresponde à omissão da segunda cláusula; “zera chamados_vivos” vale quando trabalha é False, não incondicionalmente. Há também o veto independente `marcos_acesos`. (`app/docs/BACKLOG.md:807`; `app/core/juizes.py:144-146`; `app/docs/JUIZES.md:60-61`; `app/ponto/services/cartorio.py:1206-1214`)
- A primeira cláusula do juiz realmente não é alcançada pelo cartório normal com trabalha False, porque `chamados_vivos=0`; nunca bateu já passa zero explicitamente. (`app/ponto/services/cartorio.py:1029`, `:1207-1214`; `app/ponto/supra_juiz.py:470-472`)
- “Ata DESTE julgamento” está correto nos dois ramos: `_ata_nb` recém-montada e `_ata_agora.ata` recém-montada; a segunda fica em `_tl.atas` antes da chamada. (`app/docs/CORTES.md:415`; `app/ponto/services/cartorio.py:1012`, `:1029`, `:1184-1188`, `:1213`)
- “Célula JA LAVRADA” de `_C18` exige ressalva para o cartório: esse leitor usa lavra em curso antes da persistência final. (`app/core/juizes.py:137`; `app/ponto/services/cartorio.py:1184-1188`, `:1213`, `:1563-1565`)
- A fórmula textual `ata.tipo_dia != trabalho` omite a guarda de valor verdadeiro: None/'' não calam para trabalha True/None. (`app/docs/CORTES.md:415`; `app/docs/JUIZES.md:56`; `app/core/juizes.py:138`; `app/chamados/juizes.py:408`)
- “trabalha=None COBRA” é condicional: ata folga/ausência/falta/indefinido cala mesmo com None. (`app/docs/JUIZES.md:57`; `app/core/juizes.py:139`; `app/chamados/juizes.py:408`)
- O fechamento SUPRA-DELEGA descreve corretamente a chamada ao juiz nesse ramo, mantendo a exceção SEM_JURISDICAO. A divergência para ata explícita indefinido continua existindo pela guarda autorizada: o juiz isolado diria sem trabalho, mas supra nem o chama. (`app/docs/BACKLOG.md:806`; `app/docs/CORTES.md:415`; `app/ponto/supra_juiz.py:48`, `:470-472`; `app/chamados/juizes.py:408`)
- Os números de produção citados em BACKLOG permanecem declarações documentais; sem banco/log não os confirmei. [não verificado]
- BACKLOG NONE-LEITORES conserva endereços antigos do cartório; os gates equivalentes atuais estão em `1504` e `1608`, não nos quatro números registrados. (`app/docs/BACKLOG.md:805`; `app/ponto/services/cartorio.py:1504`, `:1608`)
- A fila já admite outros predicados próprios e ausência parcial divergente, portanto a documentação não sustenta “todos os leitores de chamados/ponto migraram”. (`app/docs/BACKLOG.md:823`)

Não identifiquei defeito adicional comprovado no delta além dessas limitações/documentação; os outros predicados acima são majoritariamente preexistentes e têm universos diferentes. [não verificado]

## 31/10 16:34 · sessao cdc88f1201ca · - · - · origem=Ronald (tmux)

Escritores e leitores confirmados, sem acesso a banco/log nem alterações:

| Valor | Escritor / porta | Leitores e autoridade |
|---|---|---|
| `CelulaDia.trabalha` | Geração calcula `ec.eh_dia_trabalho_calculado(d)` e cria/atualiza pela porta; regeneração também, inclusive limpa para `False` fora de vínculo; reversão deriva do DNA anterior. `app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:260`, `app/ponto/portas/celula.py:532`, `app/ponto/portas/celula.py:590`, `app/ponto/portas/celula.py:612`. | Juiz lê `is False`, mais tipo verdadeiro diferente de trabalho na ata: `app/chamados/juizes.py:407`. Fábrica delega em `app/chamados/services/fabrica_da_celula.py:80`; materialização em `app/chamados/services/materializacao.py:160`; cluster em `app/ponto/services/cluster_espurio.py:28`; intervalo em `app/ponto/management/commands/detectar_intervalo_ausente.py:51`. |
| `ata.tipo_dia` | `ata_do_dia` copia do dict da grade em `app/ponto/services/cartorio.py:568`; `CelulaDia.lavrar_veredito` apenas delega em `app/escala/models.py:1794`; persistência na porta em `app/ponto/portas/celula.py:726`, `app/ponto/portas/celula.py:768`. | Juiz em `app/chamados/juizes.py:408`; grade lê e tem fallback próprio em `app/escala/services/leitor_celula.py:345`; declaração tem veto próprio `tipo_dia != 'trabalho'` em `app/chamados/services/declaracao_texto.py:95`. |
| `ata.minutos_previstos` | Cartório converte `int(dia.get(...) or 0)` em `app/ponto/services/cartorio.py:569`; mesma porta de persistência acima. | Grade lê em `app/escala/services/leitor_celula.py:347`; sem número, consulta minutos do dia apenas se `cel.trabalha` for verdadeiro, senão põe zero em `app/escala/services/leitor_celula.py:354`; supra usa o valor projetado em `app/ponto/supra_juiz.py:223`. Nenhum desses cálculos consulta `dia_sem_trabalho`. |
| `ata.lampadas` | `lampadas_do_dia` monta os itens em `app/ponto/services/cartorio.py:540`, `app/ponto/services/cartorio.py:552`; `ata_do_dia` inclui em `app/ponto/services/cartorio.py:566`; ausência/justificativa dispensa luzes em `app/ponto/services/cartorio.py:611`; teto transforma apagada em `None` em `app/ponto/services/cartorio.py:707`. | Materialização lê slots/luzes em `app/chamados/services/materializacao.py:158`, `app/chamados/services/materializacao.py:164`; cluster conta apagadas em `app/ponto/services/cluster_espurio.py:31`; intervalo indexa em `app/ponto/management/commands/detectar_intervalo_ausente.py:64`; grade projeta células em `app/escala/services/leitor_celula.py:92`; Pauta informa apagadas em `app/pautas/services.py:422`. São leituras da lâmpada para outras perguntas; os quatro emissores já delegaram o gate de dia sem trabalho nos locais acima. |
| `INSUMO_DIA_SEM_TRABALHO` | Constante em `app/ponto/supra_juiz.py:57`; adaptador escreve `{'trabalha':..., 'ata':...}` em `app/ponto/supra_juiz.py:62`; grade escreve em `app/escala/services/leitor_celula.py:370`; cartório injeta ata atual em `app/ponto/services/cartorio.py:1029`, `app/ponto/services/cartorio.py:1213`. | Único leitor funcional é `_dia_sem_trabalho_pelo_juiz`: acesso obrigatório e delegação, sem predicado paralelo, `app/ponto/supra_juiz.py:65`. |
| `Pauta.texto/ancora_id` novos | Fábrica constrói âncora `colab:data`, marcador e tags em `app/chamados/services/fabrica_da_celula.py:175`, `app/chamados/services/fabrica_da_celula.py:189`; escreve pela porta em `app/chamados/services/fabrica_da_celula.py:185`; porta normaliza/corta texto e grava em `app/pautas/services.py:163`, `app/pautas/services.py:180`. | Dedup relê ambas com filtro por cabeça/DP/marcador e procura tags em `app/chamados/services/fabrica_da_celula.py:180`; API expõe em `app/pautas/views.py:40`; painel mostra texto em `app/templates/core/_copiloto.html:405`; chip consulta âncora em `app/pautas/services.py:334`; contexto parseia âncora em `app/pautas/services.py:381`. Não respondem “dia sem trabalho”; contexto usa `trabalha is False` para rótulo `folga`, sem juiz, em `app/pautas/services.py:418`. |

Exceções a “um escritor da ata”: `carimbar_precedencia` escreve outra chave da mesma ata pela mesma porta em `app/ponto/portas/celula.py:811`; projeções em memória copiam ata sem salvar em `app/ponto/portas/celula.py:667` e `app/escala/utils.py:1826`. Os testes novos escrevem `trabalha/ata` diretamente para formar fixture em `app/chamados/tests/test_f1c4_3_emissores.py:65`.

A fábrica delega primeiro ao juiz, mas seu caminho efetivo ainda passa pelo veto próprio da declaração: `motivos_pela_ata` chama `_ata_do_dia` em `app/chamados/services/fabrica_da_celula.py:49`; a leitura da declaração rejeita qualquer tipo projetado diferente de `trabalho` em `app/chamados/services/declaracao_texto.py:95`. A grade também mantém projeção própria de tipo e de minutos em `app/escala/services/leitor_celula.py:345`.

Defeito adicional inferido: dedup nova de Pauta não é segura sob concorrência. O SELECT da fábrica acontece antes da transação de `escrever`; dois julgamentos podem obter `ja` vazio e ambos criar Pauta: `app/chamados/services/fabrica_da_celula.py:180`, `app/chamados/services/fabrica_da_celula.py:185`, `app/pautas/services.py:152`, `app/pautas/services.py:180`. Não há unicidade por dia/motivo no modelo, apenas índice de âncora e check de destinatário: `app/pautas/models.py:113`, `app/pautas/models.py:123`. Os caminhos lidos não serializam esse trecho: batch chama diretamente em `app/ponto/management/commands/processar_cartorio.py:59`; evento lê célula sem trava em `app/ponto/services/cartorio.py:1708` e chama o mesmo julgamento em `app/ponto/services/cartorio.py:1721`; fábrica roda após o corpo em `app/ponto/services/cartorio.py:824`. `em_julgamento` é thread-local, não trava entre processos: `app/ponto/services/cartorio.py:34`, `app/ponto/services/cartorio.py:770`. Duplicação efetivamente ocorrida em produção e eventual serialização externa: [não verificado].

Limitação já declarada do produto: âncora `dia` não abre objeto no painel; ROTA contém só colab/chamado/posto e dá toast para dia em `app/templates/core/_copiloto.html:564`. O link do calendário está no fim do contexto em `app/pautas/services.py:426`, podendo cair com o corte novo de contexto em `app/pautas/services.py:177`.

## 31/10 16:34 · sessao e08f301f9bb6 · - · - · origem=Ronald (tmux)

**Q2 — produtores e risco de `KeyError`.**

| Caminho | Dict e ata entregues | Insumo ausente? |
|---|---|---|
| Command `supra_juiz` | Usa `grade_da_celula`; esta lê `cel.ata` já gravada e põe `cel.trabalha` + essa ata no insumo. Referências: `app/ponto/management/commands/supra_juiz.py:116`, `:206`; `app/escala/services/leitor_celula.py:332`, `:370`. | Dias `fora_vinculo` e `sem_celula` são produzidos sem insumo, mas ficam barrados por `SEM_JURISDICAO` antes da chamada ao juiz. Referências: `app/escala/services/leitor_celula.py:340`, `:343`; `app/ponto/supra_juiz.py:470`. |
| Command `emitir_furo_retroativo` | Mesmo produtor e mesma ata já gravada. Referências: `app/ponto/management/commands/emitir_furo_retroativo.py:59`, `:99`; `app/escala/services/leitor_celula.py:332`, `:370`. | Mesma proteção para os dois dias sem insumo. Referências: `app/escala/services/leitor_celula.py:340`, `:343`; `app/ponto/supra_juiz.py:470`. |
| Cartório, `nunca_bateu` | Monta `_ata_nb` neste julgamento e entrega exatamente essa variável ao helper, antes da lavra. Referências: `app/ponto/services/cartorio.py:1012`, `:1029`, `:1045`. | Helper sempre acrescenta a chave; neste ramo `chamados_vivos=0` também impede alcançar a leitura do insumo. Referências: `app/ponto/supra_juiz.py:62`, `:471`; `app/ponto/services/cartorio.py:1029`. |
| Cartório, normal | Monta `_ata_agora.ata`, publica-a em `_tl.atas` e passa essa ata ao helper. Referências: `app/ponto/services/cartorio.py:1184`, `:1188`, `:1213`. | Helper acrescenta a chave antes da classificação. Referências: `app/ponto/supra_juiz.py:62`; `app/ponto/services/cartorio.py:1213`. |
| Evento `julgar_celula` | Não monta um quinto produtor: carrega a célula e chama `julgar_colab`, chegando aos mesmos dois ramos. Referências: `app/ponto/services/cartorio.py:1708`, `:1721`, `:777`. | Mesmas garantias dos dois ramos; célula inexistente retorna antes. Referências: `app/ponto/services/cartorio.py:1709`, `:1721`. |

Na varredura dos chamadores de produção, os únicos sítios de chamada ao classificador do supra são os dois commands e os dois ramos do cartório; os usos em `relatorios/furos.py` e `inteligencia/calculadores.py` pertencem a outra `classificar_dia`. Referências: `app/ponto/management/commands/supra_juiz.py:206`; `app/ponto/management/commands/emitir_furo_retroativo.py:99`; `app/ponto/services/cartorio.py:1028`, `:1212`; `app/relatorios/furos.py:55`; `app/inteligencia/calculadores.py:26`.

Não encontrei nesses caminhos um dict sem insumo que alcance a leitura obrigatória; afirmar ocorrência ou ausência real de `KeyError` em produção permanece **[não verificado]**. A leitura obrigatória lança se a chave faltar e só acontece depois de `tipo_dia not in SEM_JURISDICAO`, previsto falso, chamados vivos e cobertura exatamente `None`. Referências: `app/ponto/supra_juiz.py:70`, `:470`.

Os testes também produzem dicts sem insumo deliberadamente: um exige `KeyError`, outro verifica a guarda `SEM_JURISDICAO`; testes antigos sem chamado ou com cobertura aprovada continuam barrados antes da leitura. Referências: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:136`, `:141`; `app/ponto/tests/test_supra_juiz_cobranca.py:107`, `:111`; `app/ponto/supra_juiz.py:471`.

**Q4 — os dois testes novos.** [nao verificado]

| Teste | Mutação mínima no cartório que o torna vermelho, por leitura estática | [nao verificado]
|---|---|
| `test_MORDE_ramo_nunca_bateu_entrega_a_ata_deste_julgamento` | Em `app/ponto/services/cartorio.py:1029`, substituir `ata=_ata_nb` por `ata=cel.ata`: o espião recebe a ata semeada com a sentinela e `tipo_dia='folga'`, contrariando as asserções. Referências: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100`, `:109`, `:111`, `:116`. |
| `test_MORDE_ramo_normal_entrega_a_ata_deste_julgamento` | Em `app/ponto/services/cartorio.py:1213`, substituir `ata=_ata_agora.ata` por `ata=cel.ata`: falha pelas mesmas asserções e também pela identidade com `_tl.atas`. Referências: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100`, `:109`, `:111`, `:132`. |

Nenhum passa se o espião nunca for chamado: `_conferir` começa exigindo `vistos` não vazio. Referência: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:107`.

Nenhum passa recebendo a ata anterior intacta, pois ela contém sentinela e `tipo_dia='folga'`; ambos exigem ausência da sentinela e tipo `'trabalho'`. Referências: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100`, `:109`, `:111`.

Não são fixtures vazias quanto ao **transporte da ata**: exigem chamada observada, conteúdo esperado e que a lavra substitua a ata anterior; o teste normal exige ainda identidade do objeto com a ata em curso. Referências: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:107`, `:111`, `:112`, `:132`.

Entretanto, **não provam execução do ramo `COBRANCA_EM_DIA_SEM_TRABALHO` nem chamada efetiva a `dia_sem_trabalho`**: o ramo `nunca_bateu` passa `chamados_vivos=0`; a fixture normal começa com célula de trabalho e jornada prevista, sem criar chamado no cadastro. Referências: `app/ponto/services/cartorio.py:1029`; `app/ponto/supra_juiz.py:471`; `app/ponto/tests/test_bo_inss_nunca_bateu_le_a_precedencia.py:72`, `:80`, `:89`.

Há uma diferença de força entre os testes: o normal exige identidade com a ata em curso; o `nunca_bateu` só compara sentinela e `tipo_dia`, então uma ata sintética `{'tipo_dia': 'trabalho'}` pode satisfazer suas asserções mesmo perdendo outros campos. Referências: `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:106`, `:113`, `:132`.

Execução dos testes e dos mutantes completos: **[não verificado]**; as conclusões acima são estáticas, sem banco e sem escrita. [nao verificado]

## 31/10 16:28 · sessao ffcc8c5e890a · E4-COMBOIO · verificador · origem=Code

Há achados procedentes no entorno e provas mais estreitas que as promessas dos selos: atividade crua sobrevivente no painel, rótulo `previsto` invisível no HTML e escritor de `Batida` fora da porta. — `app/core/services/painel_op.py:239`; `app/templates/escala/minha_escala.html:76`; `app/chamados/services/regularizacao_ext.py:153`.

Execução dos TestCase Django, RED/GREEN histórico, correspondência com a base `5693e462` e impacto em produção: **[nao verificado]**. Os números “11” e “4732” aparecem nas docstrings; sua confirmação por registros de produção permanece **[nao verificado]**. — `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:5`; `app/escala/tests/test_e4_6_minha_escala_e_previsao.py:4`.

**1. Escritores e leitores dos campos/valores**

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `empresa_id`, `filtro_status` do painel | GET das views, passado ao construtor. — `app/core/views.py:607`; `app/core/views.py:626` | Recorte de colaboradores/postos e filtro das linhas. — `app/core/services/painel_op.py:41`; `app/core/services/painel_op.py:179`; `app/core/services/painel_op.py:246` |
| `agora`, `hoje`, `_14h_atras` | `timezone.now()`, conversão local e subtração de **24h**. — `app/core/services/painel_op.py:32`; `app/core/services/painel_op.py:46` | Consulta de entradas, candidatos, juiz de turno, previsão e duração. — `app/core/services/painel_op.py:53`; `app/core/services/painel_op.py:60`; `app/core/services/painel_op.py:65`; `app/core/services/painel_op.py:125`; `app/core/services/painel_op.py:152` |
| `Batida.colaborador`, `timestamp`, `tipo`, coordenadas | Criação pela porta `registrar_batida`, com sanitização geo; **há também correção direta de `tipo/timestamp` na regularização externa**. — `app/ponto/registro_batida.py:105`; `app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:157`; `app/chamados/services/regularizacao_ext.py:153` | Helper recorta datas/retratação; painel seleciona E, chaveia pelo colaborador e publica hora/coordenadas. — `app/ponto/turnos.py:2845`; `app/core/services/painel_op.py:51`; `app/core/services/painel_op.py:188`; `app/core/services/painel_op.py:195` |
| `Batida.retratada_em` | `retratar_batida → anotar`; desfazer humano escreve `None`; fixture E4-7 usa `update` direto. — `app/ponto/registro_batida.py:252`; `app/ponto/registro_batida.py:274`; `app/ponto/services/retratar_humano.py:207`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:57` | `batidas_apuraveis`, candidatos do painel e carregamento do juiz. — `app/ponto/turnos.py:2845`; `app/core/services/painel_op.py:61`; `app/ponto/turnos.py:2192` |
| `entradas_hoje` | Dict local: última E, na ordem de timestamp, por colaborador. — `app/core/services/painel_op.py:51`; `app/ponto/turnos.py:2852` | Lista de IDs para pings e seleção de `entrada` por linha. — `app/core/services/painel_op.py:102`; `app/core/services/painel_op.py:143` |
| `entrada → status` | Sem turno vivo, entrada existente produz `encerrado`; trabalho sem entrada produz `ausente`; restante produz `folga`. — `app/core/services/painel_op.py:169` | Filtro, contadores, quantidade de ausentes e ordenação dos colaboradores. — `app/core/services/painel_op.py:179`; `app/core/services/painel_op.py:202`; `app/core/services/painel_op.py:254`; `app/core/services/painel_op.py:263` |
| `entrada`, `entrada_id` | Timestamp local e PK da entrada selecionada. — `app/core/services/painel_op.py:188` | Card mostra a hora; ID abre detalhe geo por HTMX; view do detalhe reabre a Batida por PK. — `app/templates/core/_painel_card.html:64`; `app/templates/core/_painel_card.html:68`; `app/ponto/views.py:3236` |
| `presenca_geo` | `classificar_presenca_turno(entrada, pings)`; pings carregados para as entradas selecionadas. — `app/core/services/painel_op.py:102`; `app/core/services/painel_op.py:183`; `app/ponto/presenca.py:50` | Classe e título da bolinha geo. — `app/templates/core/_painel_card.html:68` |
| `data_turno`, `lat`, `lng` | Derivados do turno vivo/entrada e das coordenadas da entrada. — `app/core/services/painel_op.py:190`; `app/core/services/painel_op.py:195` | `data_turno` é conferido por testes antigos; consumidor produtivo desses três campos do retorno: **[nao verificado]**. — `app/core/tests/test_painel_op_turno_aberto_ancora.py:50`; `app/core/tests/test_painel_op_turno_pelo_juiz.py:80` |
| `bateu_hoje_ids → _teve_ativ → cobertura` | **Consulta independente**, crua, de qualquer Batida na data local; não deriva de `entradas_hoje`. — `app/core/services/painel_op.py:239`; `app/core/services/painel_op.py:258` | `classificar_cobertura`, mapa do painel, agregação e ordenação dos postos. — `app/core/services/painel_op.py:259`; `app/core/services/painel_op.py:273` |
| Linhas, contadores e postos do painel | Construtor. — `app/core/services/painel_op.py:185`; `app/core/services/painel_op.py:202`; `app/core/services/painel_op.py:261` | Página, fragmento e lote; card/stats; JS lê atributos de cobertura/geofence/aberto/ausente. — `app/core/views.py:607`; `app/core/views.py:632`; `app/core/views.py:679`; `app/templates/core/_painel_card.html:2`; `app/templates/core/_painel_stats.html:32`; `app/templates/core/painel_operacional.html:177` |

Para `minha_escala` e o medidor:

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| Vínculo selecionado, `tipo_ciclo`, âncora | View escolhe o primeiro vínculo ativo e lê seu cadastro. — `app/escala/views.py:17`; `app/escala/views.py:25`; `app/escala/views.py:32` | Cálculo de `ancora_ausente`, previsão e cabeçalho da tela. — `app/escala/views.py:32`; `app/escala/views.py:40`; `app/templates/escala/minha_escala.html:24`; `app/templates/escala/minha_escala.html:49` |
| `CelulaDia.trabalha`, `dna.marcos` | Porta gera/regera a célula a partir do vínculo e DNA. — `app/ponto/portas/celula.py:243`; `app/ponto/portas/celula.py:260` | `EC.eh_dia_trabalho` lê `trabalha`, com fallback calculado; `EC.marcos_do_dia` lê DNA, com fallback no template. — `app/escala/models.py:1069`; `app/escala/models.py:1072`; `app/escala/models.py:1099`; `app/escala/models.py:1108` |
| `hoje`, `dia`, `trabalho`, horários | View lê `date.today()`, gera 14 datas, consulta os dois métodos do vínculo. — `app/escala/views.py:26`; `app/escala/views.py:38`; `app/escala/views.py:40`; `app/escala/views.py:43` | Ternário `_st`, contexto e template. — `app/escala/views.py:54`; `app/escala/views.py:55`; `app/templates/escala/minha_escala.html:63`; `app/templates/escala/minha_escala.html:78` |
| `_st`: `folga/escala_suspeita/previsto` | Ternário local; não grava estado persistido. — `app/escala/views.py:54`; `app/escala/views.py:55` | Indexação de `ROTULO_STATUS`; detector AST examina esses literais. — `app/escala/views.py:60`; `app/core/medida_leitor_do_dia.py:340` |
| `ROTULO_STATUS` | Dicionário canônico em `ponto/status_dia.py`, reexportado pelo calendário. — `app/ponto/status_dia.py:28`; `app/colaboradores/services/calendario.py:45` | View, testes e leitura AST do vocabulário. — `app/escala/views.py:36`; `app/escala/views.py:60`; `app/escala/tests/test_e4_6_minha_escala_e_previsao.py:56`; `app/core/medida_leitor_do_dia.py:159` |
| `dias[].status_rotulo` | View publica **a frase**, sem publicar a chave `_st`. — `app/escala/views.py:55`; `app/escala/views.py:60` | Template somente nos ramos dúvida/folga; testes leem também o contexto de trabalho. — `app/templates/escala/minha_escala.html:75`; `app/templates/escala/minha_escala.html:84`; `app/escala/tests/test_e4_6_minha_escala_e_previsao.py:53` |
| `CATEGORIAS`, `PALAVRAS_DA_PREVISAO`, `NAO_E_LEITOR`, teto, `DECLARADOS` | Constantes do medidor; previsão tem as três palavras e uma entrada nominal para `minha_escala`. — `app/core/medida_leitor_do_dia.py:259`; `app/core/medida_leitor_do_dia.py:262`; `app/core/medida_leitor_do_dia.py:285`; `app/core/medida_leitor_do_dia.py:289`; `app/core/medida_leitor_do_dia.py:296` | `censo` usa exclusões, categoria, palavras e declarados; **categorias válidas/teto são cobrados pelos testes**, não por um guard de execução do censo. — `app/core/medida_leitor_do_dia.py:348`; `app/core/medida_leitor_do_dia.py:358`; `app/core/tests/test_medida_leitor_do_dia.py:73` |
| `chave/metade/linhas/palavras/sitios/categoria/motivo` | Detector e montagem dos registros do censo. — `app/core/medida_leitor_do_dia.py:221`; `app/core/medida_leitor_do_dia.py:346`; `app/core/medida_leitor_do_dia.py:351` | Contagem, novos/sumidos/exclusões mortas, testes e publicação no ESTADO. — `app/core/medida_leitor_do_dia.py:368`; `app/core/tests/test_medida_leitor_do_dia.py:39`; `bin/leitor_dia_estado.py:46`; `bin/leitor_dia_estado.py:51` |

`bin/leitor_dia_estado.py` carrega o mesmo módulo e chama `censo`; `gerar_estado` usa esse leitor. O selo S3 importa o detector de **valor**, sem duplicá-lo. — `bin/leitor_dia_estado.py:32`; `bin/leitor_dia_estado.py:35`; `bin/gerar_estado.py:290`; `app/ponto/tests/test_s3_placar_exercicio.py:38`.

Para o E4-1:

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `CelulaDia.status_dia/status_rotulo` | Produção: aplicação do status dentro da porta de lavratura; fixture injeta ambos por `update`. — `app/ponto/portas/celula.py:701`; `app/ponto/portas/celula.py:709`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:52`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:67` | Leitor D4, controle de preenchimento do cartório e consulta/assert do teste; o service atual decide pelos fatos. — `app/ponto/status_dia.py:171`; `app/ponto/status_dia.py:181`; `app/ponto/services/cartorio.py:726`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:80`; `app/chamados/services/feriado_reavaliar.py:33` |
| `FolgaDia.escala_colaborador/data/calendario_origem` | Produção materializa calendário/cria e reparenta folga manual; fixture cria calendário no velho ou manual no novo. — `app/escala/folgas.py:173`; `app/escala/folgas.py:259`; `app/escala/folgas.py:405`; `app/escala/folgas.py:435`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:88`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:104` | Precedência lê por pessoa/data e descarta calendário de outro vínculo; overlay da grade lê por vínculo. — `app/ponto/precedencia.py:236`; `app/ponto/precedencia.py:313`; `app/escala/utils.py:1109` |
| `faixa`, `era_dia_previsto`, `fatos.folga_lancada` | `fatos_do_dia → fatos_do_periodo`. — `app/ponto/precedencia.py:209`; `app/ponto/precedencia.py:355` | Seleção de espúrios e diagnóstico/asserts da fixture. — `app/chamados/services/feriado_reavaliar.py:33`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:73`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:94` |
| `modulo_origem`, `subcategoria`, `contexto_json.data_turno` | Fixture cria `batida_ausente/atraso/data_turno`; catálogo declara a chave do dia. — `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:59`; `app/chamados/catalogo/modulos.py:105` | Universo, juiz do dia do fato e premissa. — `app/chamados/services/feriado_reavaliar.py:17`; `app/chamados/services/feriado_reavaliar.py:30`; `app/chamados/services/feriado_reavaliar.py:38`; `app/chamados/juizes.py:104` |
| `FATO_FALTA_DESCOBERTA=False` | `espurios` fornece ao catálogo após consultar o juiz. — `app/chamados/services/feriado_reavaliar.py:36` | `premissa_morta → avaliar`. — `app/chamados/catalogo/premissa.py:144` |
| `Chamado.status_local` | Fixture nasce aberto; fechamento produtivo usa `reconciliar → _gravar_estado`. — `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:58`; `app/chamados/models.py:615`; `app/chamados/models.py:618` | Universo VIVOS, motor/gates e asserts aberto/resolvido. — `app/chamados/services/feriado_reavaliar.py:17`; `app/chamados/models.py:637`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:97`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:110` |

Os cadastros auxiliares das fixtures — Empresa/Praca/Posto/TipoEscala/Colaborador/vínculos/usuário — são escritos nos respectivos `setUp`/`setUpTestData`; sustentam os seletores, ciclos e relações acima. Consumidores adicionais de cada identificador auxiliar, como CNPJ/nome de fixture: **[nao verificado]**. — `app/core/tests/test_e4_7_painel_entrada_retratada.py:35`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:43`; `app/escala/tests/test_e4_6_minha_escala_e_previsao.py:23`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:32`.

**2. Classes que já reprovaram obra**

| Classe | Resultado |
|---|---|
| Segundo juiz | **Procedente no entorno:** a grade ainda transforma FolgaDia em folga sem verificar a titularidade pelo vínculo do dia, enquanto a precedência descarta calendário de outro vínculo. O próprio RELATO nomeia essa divergência. — `app/escala/utils.py:1109`; `app/escala/utils.py:1118`; `app/ponto/precedencia.py:307`; `app/ponto/precedencia.py:313`; `app/docs/RELATO.md:63` |
| Segundo escritor fora da porta | **Procedente, anterior ao diff:** `alvo` é FK de Batida; regularização externa grava `tipo/timestamp` por `save` direto e remove por `delete`. O caminho é alcançável pela confirmação web e APIs. — `app/chamados/models.py:3098`; `app/chamados/services/regularizacao_ext.py:127`; `app/chamados/services/regularizacao_ext.py:136`; `app/chamados/services/regularizacao_ext.py:153`; `app/chamados/services/regularizacao_ext.py:356`; `app/chamados/views_regularizacao.py:216`; `app/api/views.py:3061`; `app/api/views_core.py:1327` |
| Selo cego ao escritor | **Procedente:** o censo puro classificou essas escritas por `alvo` sem entidade resolvida; o gate ignora `entidade=None`. Portanto seu verde não prova escritor único nesse caminho. — `app/core/censo_escritas.py:43`; `app/core/censo_escritas.py:469`; `app/ponto/tests/test_chokepoint_batida_gate.py:29` |
| Relógio fora da borda | E4-7 não acrescenta relógio: helper recebe a janela e painel passa `agora` ao juiz. A view mantém `date.today()`; congelar testes não altera essa leitura real. **Dúvida concreta no selo E4-1:** setup alcança cartório com relógio real, sem freeze, e o cartório ignora datas futuras. — `app/core/services/painel_op.py:32`; `app/core/services/painel_op.py:65`; `app/ponto/turnos.py:2844`; `app/escala/views.py:26`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:26`; `app/chamados/signals.py:180`; `app/ponto/services/cartorio.py:1691`; `app/ponto/services/cartorio.py:1700` |
| Derivação paralela | **Procedente, dívida mantida:** painel ainda escolhe status próprio e permanece declarado no medidor. E4-7 corrige sua entrada, sem concluir essa migração. — `app/core/services/painel_op.py:148`; `app/core/services/painel_op.py:169`; `app/core/medida_leitor_do_dia.py:312` |
| Exclusão sem predicado | A exclusão nova **tem predicado lexical**, mas não comprova futuro nem consulta à grade: testa apenas o subconjunto de palavras detectadas. — `app/core/medida_leitor_do_dia.py:348` |
| Pouso antes do pré-requisito | **Dúvida documental:** RELATO manda E4-1 esperar cura da grade e sombra com 0044; cumprimento: **[nao verificado]**. A saída ii fornecida é caracterização, e o cron permanece na lista de juízes; o diff não demonstra rebaixamento prematuro. — `app/docs/RELATO.md:63`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:1`; `app/config/crons.py:1983` |

**Mordida dos selos:**

- **E4-7, estática procedente:** restaurar a consulta crua faz A recuperar sua única entrada retratada e C escolher 07:30; isso contradiz os asserts. Zerar todas as entradas contradiz B. O teste não afirma atividade/cobertura, apesar da promessa da docstring. Execução: **[nao verificado]**. — `app/core/tests/test_e4_7_painel_entrada_retratada.py:3`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:64`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:69`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:78`.
- **E4-6:** o teste de contexto rejeita `previsto → ok`; o teste HTML das duas cascas não rejeita essa troca por si só, pois o ramo trabalho não imprime o rótulo. — `app/escala/tests/test_e4_6_minha_escala_e_previsao.py:53`; `app/escala/tests/test_e4_6_minha_escala_e_previsao.py:66`; `app/templates/escala/minha_escala.html:76`.
- **Medidor:** censo executado em memória contou **6 status, 0 valor**, sem novos/sumidos/sítios alterados/exclusões mortas/ilegíveis; a mutação literal para `ok` produziu **7** e recolocou `minha_escala` em `novos`. Isso verifica a mordida lexical prevista pelo código. — `app/core/medida_leitor_do_dia.py:348`; `app/core/medida_leitor_do_dia.py:370`; `app/core/tests/test_medida_leitor_do_dia.py:110`.
- **E4-1, dúvida forte na mordida funcional:** o mutante literal é pego pelo AST, mas “selecionar erroneamente” não garante “fechar”. O nascimento do chamado pode lavrar ATA com lâmpadas apagadas; `_lampada_folga` preserva essa ATA. Então `_celula_furada=True` faz `premissa_regularizada=False` antes do fallback para `espurios`, e o gate posterior recusa o fechamento. Nesse caminho, os asserts aberto/folga sobrevivem ao mutante. Execução efetiva: **[nao verificado]**. — `app/chamados/signals.py:164`; `app/ponto/services/cartorio.py:1011`; `app/ponto/services/cartorio.py:1042`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:67`; `app/chamados/juizes.py:454`; `app/chamados/models.py:1247`; `app/chamados/models.py:646`; `app/chamados/models.py:709`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:97`.

Para prender a seleção, falta observar diretamente que A não pertence a `espurios`, além de observar o estado final; hoje o teste verifica apenas faixa inicial, fechamento e coluna. — `app/chamados/services/feriado_reavaliar.py:23`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:93`.

**3. `painel_op`: mudança de universo**

No trecho substituído, a condição adicional é **`retratada_em IS NULL`**. Permanecem o limite inferior `agora−24h`, ausência de limite superior, todos os colaboradores na pré-carga, filtro `tipo='E'`, `select_related` e ordenação por timestamp; o dict continua escolhendo a última E por colaborador. Não surge filtro por `aprovada`. — `app/core/services/painel_op.py:46`; `app/core/services/painel_op.py:51`; `app/ponto/turnos.py:2845`.

O comentário/nome “14h” continua divergente das **24h** efetivas; isso antecede a troca. “Mesmo universo do juiz” vale para retratação, sem igualdade das janelas: o juiz carrega três datas locais, `hoje−2..hoje`. — `app/core/services/painel_op.py:45`; `app/ponto/turnos.py:2186`; `app/ponto/turnos.py:2190`.

Continuam dois acessos diretos a Batida no arquivo:

- `_cids_pn` contém explicitamente o filtro de retratação. — `app/core/services/painel_op.py:60`.
- `bateu_hoje_ids` não contém esse filtro e admite qualquer tipo na data local. **Procedente:** a afirmação de que ele foi corrigido junto de `entradas_hoje` excede o patch. — `app/core/services/painel_op.py:239`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:3`.

Não há base para afirmar mudança visual de cobertura causada somente por esse segundo cru: atividade distingue `encerrado/inativo` quando previsto/presente são zero, mas o painel converte ambos em `encerrado`. — `app/ponto/selecao_periodo.py:237`; `app/core/services/painel_op.py:242`.

**4. `minha_escala`: `previsto` nas duas cascas**

**Procedente: o contexto muda; a apresentação do trabalho não consome a mudança.** A view publica `ROTULO_STATUS['previsto']`, porém o ramo trabalho mostra somente início/fim. Cor depende de hoje e `trabalho`; não há decisão por `previsto`, título de previsão ou ícone específico desse status. Portanto não cai em default de status desconhecido: a string não participa da renderização desse ramo. — `app/escala/views.py:54`; `app/escala/views.py:60`; `app/templates/escala/minha_escala.html:63`; `app/templates/escala/minha_escala.html:76`.

As duas cascas compartilham esse conteúdo: template estende `base_tpl`, e `app_shell` escolhe `base.html/base_app.html`. Renderização efetiva no navegador: **[nao verificado]**. — `app/templates/escala/minha_escala.html:1`; `app

[truncado em 20000 de 23911 caracteres]

## 31/10 16:27 · sessao e6629a237889 · V3-RETOMA · verificador · origem=Code

**BLOQUEIA — permanecem pausas perdidas no split e a fusão das pontas altera a prorrogação noturna.** A mistura entre os totais de HE de CCTs diferentes foi corrigida, mas esses dois problemas impedem aprovação (`app/ponto/calculador/alimentacao.py:539`; `app/ponto/calculador/regras.py:840`; `app/ponto/calculador/regras.py:852`).

**BLOQUEIA — os intervalos internos do período continuam fora do split.** A porta conserva o envelope inteiro em `he_pontas`, desconta os intervalos dos segmentos trabalhados, mas chama `do_dia` com `intervalos=()`. O novo split acrescenta somente lacunas **entre** períodos; não recupera as pausas internas (`app/ponto/calculador/alimentacao.py:99`; `app/ponto/calculador/alimentacao.py:118`; `app/ponto/calculador/alimentacao.py:124`; `app/ponto/calculador/alimentacao.py:539`; `app/ponto/calculador/regras.py:851`).

Contraprova: feriado **21h–05h**, pausa **23h–00h**, trabalhados=420, previstos=360, intervalo declarado=60, dobra ativa e prorrogação desativada. Há excedente positivo de 60 minutos, substituído pela dobra: HE50=0 e HE100=7h. O split atual recua sete horas sem pausa, toma **22h–05h** e afirma **7h noturnas**. Com o intervalo julgado, o trabalho noturno é **22h–23h + 00h–05h = 6h**. A função compartilhada descontaria essa pausa se a recebesse; o motor a entrega, a porta diária não (`app/ponto/calculador/regras.py:767`; `app/ponto/calculador/regras.py:779`; `app/ponto/calculador/regras.py:841`; `app/ponto/motor_calculo_v2.py:390`; `app/ponto/motor_calculo_v2.py:392`; `app/ponto/motor_calculo_v2.py:956`). Ocorrência em produção: **[nao verificado]**.

**BLOQUEIA — fundir as pontas muda a janela de prorrogação.** A chamada única usa a primeira entrada e a última saída; `minutos_noturnos` decide a extensão depois das 05h pelo início/fim dessa chamada. Portanto, descontar as lacunas depois não equivale a julgar cada período separadamente (`app/ponto/calculador/regras.py:846`; `app/ponto/calculador/regras.py:852`; `app/ponto/motor_calculo_v2.py:109`; `app/ponto/motor_calculo_v2.py:145`).

Contraprova: mesmo dia, períodos **00h–01h e 06h–08h**, feriado em dobra, trabalhados=previstos=180 e prorrogação ativa. A chamada fundida conta 00h–08h como noturno prorrogado e desconta a lacuna 01h–06h: **HE100 noturna=3h**. Separadamente, o primeiro período dá 1h e o segundo zero. O próprio adicional noturno diário continua calculado por pares e reconhece somente 1h de relógio (`app/ponto/calculador/regras.py:82`; `app/ponto/calculador/regras.py:85`; `app/ponto/calculador/regras.py:841`; `app/ponto/calculador/regras.py:851`; `app/ponto/motor_calculo_v2.py:110`). Ocorrência em produção: **[nao verificado]**.

Além disso, a regra escrita exige ancoramento no fim de **cada período** e mantém “último par do dia” como lei aberta; o novo código assume o último fim global. Autorização específica para essa mudança: **[nao verificado]** (`app/docs/PROMPTS.md:344`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:255`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:261`; `app/ponto/calculador/regras.py:825`).

1. **Destino dos achados anteriores**

| Achado anterior | Resultado no diff atual | Prova |
|---|---|---|
| **BLOQUEIA — HE principal e split com CCTs diferentes** | **Resolve o defeito específico.** O split usa `_dobra/extra` do dia; extras do motor ficam apenas no detalhe. A porta também entrega períodos sem extras. | `app/ponto/calculador/regras.py:840`; `app/ponto/calculador/regras.py:862`; `app/ponto/calculador/alimentacao.py:99` |
| **NAO-BLOQUEIA — consultas por dia sem medida/cache equivalente** | **Resolve em parte.** Acrescenta instrumento e teto, mas mantém a fábrica por dia; não há resultado medido fornecido. | `app/ponto/calculador/alimentacao.py:403`; `app/ponto/calculador/alimentacao.py:517`; `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`; **[nao verificado: contagem real]** |
| **NAO-BLOQUEIA — testes apresentados como RED que passariam antes** | **Resolve em parte.** Nacional e vigência interna viram `GUARDA`; a fixture de praça passa a cobrir 21/09; surge vigência começando no meio. A dobra básica continua sendo guarda. | `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:45`; `:217`; `:270`; `:313`; `:337` |
| **NAO-BLOQUEIA — cobertura incompleta** | **Resolve em parte.** Acrescenta excedente positivo com dobra e HE50 noturna. Os casos novos continuam sem assert de saída antecipada e sem mudança real dos parâmetros noturnos por praça/vigência. | `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:48`; `:125`; `:258`; `:289`; `:320` |

Os testes existentes dos outros parâmetros verificam troca de **template**, usando fábrica simulada; isso não demonstra mudança dos parâmetros noturnos pela vigência ou praça da CCT real (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:158`; `:176`; `:187`; `:193`).

2. **Quem escreve e quem lê**

| Campo/valor | Escritor/origem | Leitor/destino |
|---|---|---|
| Entrada/saída efetivas e intervalos julgados | O motor carimba `efetivo_entrada`, `efetivo_saida` e `intervalos_do_motor`. | A porta lê as pontas, monta segmentos e guarda o envelope em `he_pontas`. **O intervalo não acompanha esse envelope até o split.** (`app/ponto/motor_calculo_v2.py:1778`; `app/ponto/calculador/alimentacao.py:75`; `:99`; `:124`; `:539`) |
| Dia da jornada | O motor publica o mapa por identidade do período. | A porta chaveia pontas/segmentos pelo mapa; o núcleo soma pelo terceiro elemento do par; o laço consulta a CCT pelo mesmo `k`. (`app/ponto/motor_calculo_v2.py:2398`; `app/ponto/calculador/alimentacao.py:70`; `:99`; `app/ponto/calculador/nucleo.py:134`; `:141`; `app/ponto/calculador/alimentacao.py:517`) |
| Extras antigas dos períodos | O motor grava dobra/excedente nos períodos; há também distribuição diária nos ramos partido/comercial. | A porta copia para a tupla; agora `do_dia` só as soma em `he_motor_*_min`. (`app/ponto/motor_calculo_v2.py:2749`; `:2789`; `:3032`; `:3080`; `:3278`; `:3332`; `app/ponto/calculador/alimentacao.py:94`; `app/ponto/calculador/regras.py:862`) |
| HE50/HE100 principal do dia | `do_dia` calcula `extra`, pergunta a dobra e escreve os totais. | O split **repete os ramos/faixas** para produzir `_x50/_x100`; não lê diretamente `val['horas_extras_*']`. (`app/ponto/calculador/regras.py:769`; `:776`; `:780`; `:795`; `:810`; `:840`) |
| HE50/HE100 noturna | `he_noturna` devolve os minutos; `do_dia` grava horas em `val`. | Fechamento e comparador recebem as rubricas pela mesma porta. (`app/ponto/motor_calculo_v2.py:359`; `app/ponto/calculador/regras.py:852`; `:856`; `app/ponto/services/fechamento.py:1239`; `app/ponto/management/commands/diff_calculador.py:417`) |
| Persistência das rubricas | Fechamento entrega `rubricas` ao serviço existente, com versão `oraculo`; `lavrar` substitui os campos e grava `DiaPago`. | A soma da competência lê os campos persistidos, com versão padrão `motor`. (`app/ponto/services/fechamento.py:1245`; `:1256`; `:1262`; `app/ponto/services/dia_pago.py:328`; `:358`; `:548`; `:557`) |
| `he_calc_*`, `he_motor_*`, quantidade de pontas | `do_dia` escreve em `Rubricas.detalhe`. | Teste lê `he_motor_*`; comparador imprime o detalhe com `--colab`. A lavratura não copia esse detalhe: copia valores e causas de `nao_decide`. (`app/ponto/calculador/regras.py:859`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:322`; `app/ponto/management/commands/diff_calculador.py:545`; `app/ponto/services/dia_pago.py:322`; `:329`) |
| Motor CCT do dia `k` | `_cct_de` chama a fábrica com template diário e `k`; a fábrica resolve praça/vigência e prepara os parâmetros. | `cct_do_motor` transcreve os atributos para `CCT`; a porta entrega dobra, calendário, prorrogação e hora reduzida a `do_dia`. (`app/ponto/calculador/alimentacao.py:403`; `:517`; `app/core/regua_cct.py:329`; `:366`; `:466`; `:470`; `:477`; `app/ponto/calculador/nucleo.py:347`; `app/ponto/calculador/alimentacao.py:518`; `:534`; `:540`) |

A exportação examinada emite HE principal e adicional noturno; os splits não viram eventos separados no TXT. Assim, o defeito demonstrado no split não prova alteração do TXT atual (`app/folha/export.py:413`; `:430`; `:436`). Alteração efetiva exportada: **[nao verificado]**.

3. **Classes de reprovação e mordida dos testes** [nao verificado]

| Classe procurada | Resultado |
|---|---|
| Segundo juiz duplicando juiz registrado | Não identificado na resolução de vínculo/aplicação: a fábrica chega à mesma `vinculo_do_dia`, e praça/vigência continuam em `_aplicacao_vigente`. A composição global das pontas, porém, altera a pergunta temporal conforme o segundo bloqueio acima. (`app/escala/alimentacao.py:298`; `:301`; `app/core/regua_cct.py:253`; `:261`; `app/ponto/calculador/regras.py:852`) |
| Segundo escritor fora da porta | Nenhum novo identificado neste fluxo: a versão `oraculo` continua entrando pelo mesmo `lavrar`. (`app/ponto/services/fechamento.py:1245`; `:1262`; `app/ponto/services/dia_pago.py:328`; `:358`) |
| Relógio fora da borda | Nenhuma nova leitura do relógio identificada no diff; a decisão diária recebe `k`. O fechamento captura seu instante na borda da passada. (`app/ponto/calculador/alimentacao.py:481`; `:517`; `app/ponto/services/fechamento.py:595`) |
| Derivação paralela de fonte única | Há repetição interna da escolha dobra/excedente e das faixas no split. Divergência numérica causada somente por essa repetição: **[nao verificado]**. A divergência temporal da fusão está demonstrada acima. (`app/ponto/calculador/regras.py:779`; `:794`; `:840`; `:843`; `:852`) |
| Selo sem mordida | O `>0` do contador não comprova consultas diárias da CCT: outras consultas diárias continuam dentro da captura. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`; `:370`; `app/ponto/calculador/alimentacao.py:528`; `app/escala/utils.py:2120`) |
| Pouso antes do pré-requisito | Não demonstrado. O relato declara ausência de pouso e exige suite, Codex, opus e condições financeiras; o backlog mantém troca dependente de `!`. Cumprimento efetivo para este hash: **[nao verificado]**. (`app/docs/RELATO.md:14`; `app/docs/BACKLOG.md:816`) |

Simulação mental dos testes novos/alterados contra **7e0fc9bc**, usando o diff fornecido; execução da suíte nessa base: **[nao verificado]**.

| Teste/caso | Passaria na base? | [nao verificado]
|---|---|
| S5b invertido para HE diária | **Não**: a base usaria extras das pontas e não produziria os novos detalhes. (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:292`; `:315`; `:322`) |
| Dobra com excedente positivo | **Sim**: o ramo dobra/`elif` já existia. É guarda contra `elif` virar `if`. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:48`; `app/ponto/calculador/regras.py:779`; `:794`) |
| CCT diária sem dobra × motor com dobra | **Não**: exige eliminar o split antigo positivo. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:111`) |
| CCT diária com dobra × motor zerado | **Não**: exige criar o split diário de 6h. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:118`) |
| Excedente diário × dobra do motor | **Não**: exige HE50 noturna=1h e HE100 noturna=0. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:125`) |
| Sem calendário | **Não**: exige declarar as duas parcelas, sem valores afirmados. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:134`; `app/ponto/calculador/regras.py:830`) |
| Ponta sem extras entra como geometria | **Não**: o filtro removido descartava essa ponta. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:150`; `app/ponto/calculador/alimentacao.py:99`) |
| Nacional e vigência interna renomeados | **Sim**, corretamente descritos como guardas. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:217`; `:313`) |
| Praça externa com fixture corrigida; vigência começando no meio | **Sim contra 7e0**, que já tinha resolução diária segundo o anexo. Distinguem a implementação anterior por `ini`, não esta troca do split. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:270`; `:284`; `:337`; `app/ponto/calculador/alimentacao.py:517`) |
| Contador | O diff não altera o caminho ORM capturado; não é RED da mudança do split. Resultado numérico: **[nao verificado]**. (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`; `:368`) |

4. **Contraexemplos solicitados**

- **HE positiva + noturno + dobra:** o caso 21h–05h com pausa 23h–00h demonstra o primeiro bloqueio. Sem pausa, o novo teste protege a substituição do excedente, mas não afirma o split desse mesmo caso (`app/ponto/calculador/alimentacao.py:539`; `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:54`; `:61`).

- **CCT muda entre a fatia e `k`:** o motor antigo continua nascendo em `fatia_ini`; o calculador consulta `k`. Agora, ponta com 360 minutos de HE100 e CCT diária sem dobra produz total/split zero; no espelho, CCT diária com dobra e ponta zerada produz 6h/6h. Esse defeito específico está corrigido (`app/ponto/services/fechamento.py:711`; `app/ponto/calculador/alimentacao.py:517`; `app/ponto/calculador/regras.py:840`; `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:111`; `:118`).

- **Turno dividido atravessando 20→21:** se o juiz atribuir ambas as partes ao dia 20, pontas, minutos e CCT permanecem no dia 20. A busca inclui a saída do dia seguinte; o motor filtra pelo dia da jornada, e o núcleo conserva esse dia nos segmentos. Teste novo específico combinando split e virada: **[nao verificado]** (`app/ponto/services/fechamento.py:606`; `:610`; `app/ponto/motor_calculo_v2.py:2367`; `:2398`; `app/ponto/calculador/nucleo.py:134`; `:141`; `app/ponto/calculador/alimentacao.py:517`).

- **Intermitente em feriado municipal:** o motor esvazia o calendário injetado; esse conjunto chega à regra diária. A consulta de data ausente usa o calendário sem município, preservando somente o nacional nesse caminho. O teste municipal exige zero e o nacional é guarda (`app/ponto/motor_calculo_v2.py:3495`; `app/ponto/calculador/nucleo.py:350`; `app/ponto/calculador/alimentacao.py:534`; `app/ponto/motor_calculo_v2.py:901`; `app/core/feriados.py:91`; `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:211`; `:217`).

5. **NAO-BLOQUEIA — contador e teto 99 não constituem a medida prometida da CCT.**

`q2/q4` contam **todas as consultas de `rubricas_por_dia`**; `(q4-q2)/2` mede o custo marginal da porta nessa fixture. Previsto e intervalo também consultam vínculos por dia, portanto o `>0` pode continuar passando mesmo com cache da CCT (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`; `:368`; `:370`; `app/ponto/calculador/alimentacao.py:528`; `:529`; `app/escala/utils.py:2152`; `:2200`; `:2120`).

**99 é trava provisória sobre esse custo marginal, não valor medido.** O literal é escrito na constante e lido pelo `assertLessEqual` e sua mensagem; `q2`, `q4` e `por_dia` são locais, impressos e consumidos pelos asserts. Nenhum leitor produtivo desse contador foi identificado (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:366`; `:369`; `:370`; `:372`; `:377`; `:379`). Contagem efetiva, latência e impacto em produção: **[nao verificado]**.

## 31/10 16:26 · sessao 7c8017290e23 · - · - · origem=Ronald (tmux)

**BLOQUEIA — a fusão das pontas muda a prorrogação noturna.** O novo split chama `he_noturna` uma vez para o envelope inteiro; a prorrogação depende do início/fim dessa chamada (`app/ponto/calculador/regras.py:846`; `app/ponto/calculador/regras.py:852`; `app/ponto/motor_calculo_v2.py:109`).

Contraexemplo: mesmo dia, períodos 00–01 e 06–08, feriado em dobra, trabalhados=previstos=180, prorrogação ativa. A versão atual produz HE100 noturna=3h; a soma por período produz 1h. O próprio adicional noturno continua julgando os pares separadamente, produzindo 1h (`app/ponto/calculador/regras.py:82`; `app/ponto/calculador/regras.py:85`; `app/ponto/calculador/regras.py:840`). Executei as funções reais extraídas por AST em memória, sem arquivos/banco, com fuso fixo UTC−3: fundidas `(0,180)`; separadas `(0,60)+(0,0)` (`app/ponto/motor_calculo_v2.py:359`; `app/ponto/motor_calculo_v2.py:390`). Ocorrência em produção: [nao verificado].

**BLOQUEIA — mudança de ancoramento sem corte demonstrado.** A regra escrita exige fim de **cada período**, deixando explicitamente o último par como lei aberta; o novo código ancora no último fim global (`app/docs/PROMPTS.md:344`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:255`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:261`; `app/ponto/calculador/regras.py:825`). Contraexemplo sem divergência de CCT: p1=08–13 com HE50=90; p2=18–23:30 com HE50=30; trabalhados630, previstos510, pausa300, intervalo declarado0. HE50 total=120; antes HE50 noturna=30 minutos, agora90 (`app/ponto/calculador/regras.py:769`; `app/ponto/calculador/regras.py:795`; `app/ponto/calculador/regras.py:843`; `app/ponto/calculador/regras.py:852`). Aval específico para alterar essa distribuição: [nao verificado]. O registro de retomada permite construção em cópia e mantém a troca dependente de `!` (`app/docs/BACKLOG.md:816`).

**BLOQUEIA — a porta continua descartando os intervalos do split.** A ponta conserva o envelope inteiro, os pares descontam o intervalo, mas `rubricas_por_dia` passa `intervalos=()` (`app/ponto/calculador/alimentacao.py:99`; `app/ponto/calculador/alimentacao.py:118`; `app/ponto/calculador/alimentacao.py:124`; `app/ponto/calculador/alimentacao.py:539`). Contraexemplo: feriado21–05, pausa23–00, trabalhados420, previstos360, intervalo declarado60. HE100=7h; split sem intervalo=7h noturnas; com o intervalo julgado=6h (`app/ponto/calculador/regras.py:780`; `app/ponto/calculador/regras.py:841`; `app/ponto/motor_calculo_v2.py:390`; `app/ponto/motor_calculo_v2.py:392`). A execução isolada confirmou420 contra360 minutos. O defeito já existia no caminho anterior; a nova distribuição também o recebe (`app/ponto/calculador/alimentacao.py:539`).

O achado anterior de **mistura entre HE diária e extras da CCT da fatia foi resolvido no split**, pois os novos minutos vêm de `_dobra/extra` do dia; as extras antigas ficam apenas no detalhe (`app/ponto/calculador/regras.py:840`; `app/ponto/calculador/regras.py:862`). A ponta sem extras também entra agora (`app/ponto/calculador/alimentacao.py:93`; `app/ponto/calculador/alimentacao.py:99`).

Escrita/leitura dos valores:

- HE diária: `extra` e `_dobra` são julgados em `regras`; totais são escritos em `val`, relidos para montar `_x50/_x100`, e o juiz `he_noturna` devolve os splits (`app/ponto/calculador/regras.py:769`; `app/ponto/calculador/regras.py:776`; `app/ponto/calculador/regras.py:780`; `app/ponto/calculador/regras.py:795`; `app/ponto/calculador/regras.py:840`; `app/ponto/calculador/regras.py:856`).
- Geometria/extras antigas: porta lê os campos do período, escreve `he_pontas_por_dia`, depois entrega `he_pontas` ao dia (`app/ponto/calculador/alimentacao.py:94`; `app/ponto/calculador/alimentacao.py:99`; `app/ponto/calculador/alimentacao.py:465`; `app/ponto/calculador/alimentacao.py:538`).
- Persistência: fechamento chama a mesma porta e `lavrar`, com versão `oraculo`; `lavrar` substitui os campos e grava `DiaPago` (`app/ponto/services/fechamento.py:1239`; `app/ponto/services/fechamento.py:1245`; `app/ponto/services/fechamento.py:1262`; `app/ponto/services/dia_pago.py:320`; `app/ponto/services/dia_pago.py:328`; `app/ponto/services/dia_pago.py:358`).
- Detalhes novos: escritos em `regras`; leitor genérico é a impressão `--colab` do comparador. `lavrar` copia valores e `nao_decide`, mas não copia `Rubricas.detalhe` (`app/ponto/calculador/regras.py:859`; `app/ponto/management/commands/diff_calculador.py:545`; `app/ponto/services/dia_pago.py:322`; `app/ponto/services/dia_pago.py:329`).
- Exportação: não emite os splits como eventos próprios; emite HE principal e adicional noturno. A porta consulta `soma_da_competencia`, cujo padrão é versão `motor` (`app/folha/export.py:413`; `app/folha/export.py:430`; `app/folha/export.py:436`; `app/folha/porta_export.py:267`; `app/ponto/services/dia_pago.py:548`; `app/ponto/services/dia_pago.py:557`).

Na virada20→21, a alimentação conserva o dia do mapa do motor e o núcleo soma pelo terceiro elemento; o filtro de competência e a consulta CCT usam esse mesmo `k` (`app/ponto/calculador/alimentacao.py:70`; `app/ponto/calculador/alimentacao.py:99`; `app/ponto/calculador/nucleo.py:134`; `app/ponto/calculador/nucleo.py:141`; `app/ponto/calculador/alimentacao.py:481`; `app/ponto/calculador/alimentacao.py:517`). A busca inclui a saída do dia21 e o motor filtra pelo dia da jornada (`app/ponto/services/fechamento.py:606`; `app/ponto/services/fechamento.py:610`; `app/ponto/motor_calculo_v2.py:2367`). Regressão adicional nessa classe: [nao verificado].

## 31/10 16:26 · sessao e69e5490d2c0 · - · - · origem=Ronald (tmux)

Complemento concluído.

- **Procedente — escritor produtivo de `Batida.tipo`/`timestamp` fora da porta, anterior ao diff:** `materializar_regularizacao_externa` obtém `alvo = item.batida_alvo` e grava `alvo.tipo`, `alvo.timestamp`, `alvo.save(update_fields=['tipo','timestamp'])`; o FK do alvo aponta expressamente para `ponto.Batida`. Também remove por `alvo.delete()`. — `app/chamados/services/regularizacao_ext.py:127`, `app/chamados/services/regularizacao_ext.py:136`, `app/chamados/services/regularizacao_ext.py:153`, `app/chamados/models.py:3098`.
- **Caminho alcançável pelas três cascas:** confirmação web chama `confirmar_e_materializar`, assim como as duas APIs; o helper chama o materializador. Não é fixture nem legado desabilitado; ocorrência efetiva em produção **[nao verificado]**. — `app/chamados/views_regularizacao.py:216`, `app/api/views.py:3061`, `app/api/views_core.py:1327`, `app/chamados/services/regularizacao_ext.py:347`; rotas em `app/chamados/urls.py:54`, `app/api/urls.py:91`, `app/api/urls_core.py:16`.
- **Procedente — gate cego a esse escritor:** rodei somente o censo AST puro, com `python3 -B`, sem carregar Django/banco e sem escrever arquivos. Ele classifica `alvo.delete()` e `alvo.save()` como `entidade=None`; o gate pula entidades não atribuídas. Assim, “nenhum `Batida` atribuído fora da porta” não significa escritor único real. — implementação do censo em `app/core/censo_escritas.py:469`; limitação declarada em `app/core/censo_escritas.py:43`; descarte no gate em `app/ponto/tests/test_chokepoint_batida_gate.py:29`; escritas em `app/chamados/services/regularizacao_ext.py:136`, `app/chamados/services/regularizacao_ext.py:155`.
- **Outros candidatos revisados não são escritas persistentes:** normalização de timestamp em cópia no pareador e normalização em memória no motor, sem `save` nesses trechos. — `app/ponto/turnos.py:1524`, `app/ponto/motor_calculo_v2.py:409`.
- **Para `colaborador_id`, latitude, longitude e `retratada_em`:** o rg curto não encontrou outro escritor produtivo persistente fora da porta; exaustividade por alias/indireção **[nao verificado]**. Criação comum permanece `registrar_batida`; `retratada_em` passa por `anotar`/`retratar_batida` e desfazer humano. — `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:252`, `app/ponto/registro_batida.py:274`, `app/ponto/services/retratar_humano.py:207`.

**Leituras do retorno:** não apareceu consumidor de `lat`, `lng` ou `data_turno` do retorno do painel nos templates/cards/JS ou views que usam o construtor; eles são publicados no dicionário. `data_turno` tem consumidores claros nos dois testes antigos; `lat/lng` ficam sem consumidor encontrado, não certificar “inexistente” universalmente. — publicação em `app/core/services/painel_op.py:190`, `app/core/services/painel_op.py:195`; entrega pelas views em `app/core/views.py:610`, `app/core/views.py:635`; consumidores de teste em `app/core/tests/test_painel_op_turno_aberto_ancora.py:50`, `app/core/tests/test_painel_op_turno_pelo_juiz.py:80`; card usa entrada/ID/geo/status em `app/templates/core/_painel_card.html:64`.

## 31/10 16:25 · sessao db1652df3a91 · - · - · origem=Ronald (tmux)

Revisão estática dos testes; execução contra 7e0fc9bc: **[nao verificado]**.

| Teste novo/alterado | Resultado mental contra 7e0fc9bc |
|---|---|
| `test_MORDE_com_as_PONTAS_o_split_SAI_e_reparte_a_HE_DO_DIA` | **Falharia**: a base usava extras das pontas; o caso exige HE100 noturna zero e HE50 noturna 1,5, além dos novos detalhes (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:292`, `:315`, `:322`). |
| `test_MORDE_dobra_com_EXCEDENTE_nao_soma_HE50` | **Passaria**: dobra/`elif` já existiam e o diff não os altera. É guarda contra mutação `elif`→`if`, não RED desta alteração (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:48`; `app/ponto/calculador/regras.py:779`, `:794`). |
| CCT diária sem dobra × ponta com dobra | **Falharia**: base produziria parcela noturna positiva; o teste exige zero (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:111`). |
| CCT diária com dobra × ponta zerada | **Falharia**: base conservaria split zero; teste exige seis horas (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:118`). |
| Excedente diário × dobra na ponta | **Falharia**: base usaria HE100 da ponta; teste exige HE50 noturna=1 e HE100 noturna=0 (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:125`). |
| Sem calendário | **Falharia**: teste exige declaração das duas parcelas, ausência nos valores e causa; esse ramo é novo (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:134`; `app/ponto/calculador/regras.py:830`). |
| Ponta sem extra entra como geometria | **Falharia**: filtro anterior descartava a ponta zerada; teste exige sua presença (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:150`). |
| Nacional e vigência interna renomeados `GUARDA` | **Passariam**, agora corretamente descritos como guardas (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:217`, `:313`). |
| Praça externa com vínculo começando em setembro | **Passaria em 7e0fc9bc**, que já resolve CCT diariamente; a fixture corrigida distingue a implementação anterior que consultava `ini` (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:269`, `:284`; `app/ponto/calculador/alimentacao.py:517`). |
| Vigência começando no meio da janela | **Passaria em 7e0fc9bc** pelo mesmo motivo; é adversarial válido contra a implementação anterior por `ini` (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:320`, `:337`; `app/ponto/calculador/alimentacao.py:517`). |
| Contador de consultas | O diff não modifica o caminho ORM capturado; não constitui RED da troca do split. Resultado numérico efetivo: **[nao verificado]** (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`, `:368`). |

**NAO-BLOQUEIA — o contador mede o custo marginal da porta inteira e não comprova consultas diárias da CCT.** O contexto envolve toda `rubricas_por_dia`; seu `>0` pode continuar verdadeiro após introduzir cache na CCT, pois previsto e intervalo também consultam vínculos por dia sem receber `escalas` (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`, `:370`; `app/ponto/calculador/alimentacao.py:528`, `:529`; `app/escala/utils.py:2152`, `:2200`, `:2119`).

O **99 é teto provisório**, explicitamente declarado assim, lido pelo assert superior e pela mensagem. Não é valor medido fornecido; os valores `q2`, `q4` e `por_dia` são locais e impressos pelo teste (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:363`, `:368`, `:369`, `:372`, `:377`, `:379`).

O achado anterior de cobertura foi **resolvido em parte**: há excedente positivo em dobra e HE50 noturna explícitos; continuam ausentes nesses casos novos saída antecipada e mudança dos parâmetros noturnos por praça/vigência (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:48`, `:125`, `:258`, `:289`, `:320`).

**Pouso prematuro não demonstrado.** A documentação disponível diz “nada pousou, v2 intocada” e registra suite, Codex, opus e condições financeiras como pré-requisitos; o backlog mantém troca somente com `!` (`app/docs/RELATO.md:14`; `app/docs/BACKLOG.md:816`; `CLAUDE.md:710`). Cumprimento efetivo para 949baf5c: **[nao verificado]**.

## 31/10 16:25 · sessao da5f3e67fd21 · - · - · origem=Ronald (tmux)

- **NAO-BLOQUEIA — custo segue sem medida fornecida e o teste captura mais que CCT.** `CaptureQueriesContext` envolve toda `rubricas_por_dia`, contando todas as consultas dessa porta (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:360`). A porta também pede previsto e intervalo por dia sem alimentar `escalas=` (`app/ponto/calculador/alimentacao.py:528`), acionando novas consultas de vínculo (`app/escala/utils.py:2152`, `app/escala/utils.py:2200`, `app/escala/utils.py:2120`). Portanto `(q4-q2)/2` é custo marginal da porta nessa fixture; atribuí-lo exclusivamente à CCT excede o que o instrumento separa (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:368`).

- **99 é trava provisória, não medida.** Escritor: constante literal `CUSTO_POR_DIA_MEDIDO = 99`; leitor: `assertLessEqual` e sua mensagem. `q2`, `q4` e `por_dia` são locais do teste, impressos e consumidos pelos dois asserts; não há consumidor produtivo encontrado (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:366`, `:369`, `:370`, `:372`, `:379`). Valor efetivamente medido e latência: **[nao verificado]**.

- **CCT diária: fluxo sem segundo resolvedor identificado.** `_cct_de` chama `get_motor_cct(c,k,...)`; o laço entrega o template do vínculo diário e a data `k` (`app/ponto/calculador/alimentacao.py:403`, `:500`, `:517`). A fábrica lê switch e chama `regua_para`, que usa `vinculo_do_dia_carregado`; este alimenta a mesma juíza `vinculo_do_dia` (`app/core/regua_cct.py:444`, `:458`, `:329`; `app/escala/alimentacao.py:298`, `:301`). Aplicação e vigência são julgadas em `_aplicacao_vigente`, sem derivação disso na porta (`app/core/regua_cct.py:253`, `:261`, `:265`).

- **Escritores/leitores dos parâmetros.** `get_motor_cct` prepara prorrogação, afastamento da hora reduzida e exceção de dobra (`app/core/regua_cct.py:466`, `:470`, `:477`). `get_motor` resolve a dobra padrão pelo ciclo e entrega os argumentos ao construtor (`app/ponto/motor_calculo_v2.py:3607`). `MotorBase` grava atributos e calendário; `MotorIntermitente` substitui seu calendário por vazio (`app/ponto/motor_calculo_v2.py:712`, `:719`, `:737`, `:740`, `:3495`). `cct_do_motor` os transcreve para a dataclass congelada; a porta extrai e entrega a `do_dia` (`app/ponto/calculador/nucleo.py:281`, `:344`, `:347`; `app/ponto/calculador/alimentacao.py:518`, `:534`, `:540`).

- **Intermitente municipal: não dobra pelo caminho examinado.** O dispatch escolhe `MotorIntermitente`, que esvazia o calendário; esse conjunto vazio chega à regra diária (`app/ponto/motor_calculo_v2.py:3563`, `:3495`; `app/ponto/calculador/nucleo.py:350`; `app/ponto/calculador/alimentacao.py:534`). A regra empresta os métodos de `MotorBase`; ausência no conjunto consulta `is_feriado(data)`, sem município, alcançando somente nacional (`app/ponto/calculador/regras.py:290`, `:344`, `:347`; `app/ponto/motor_calculo_v2.py:901`; `app/core/feriados.py:91`). O comportamento histórico do intermitente já está registrado como pendência, não nasce neste diff (`app/core/juizes.py:437`). O teste municipal exige zero; o nacional é explicitamente guarda (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:211`, `:217`).

- **Relógio e escrita fora da porta:** nenhum `now`, `today` ou `localdate` encontrado nos módulos diretamente rastreados de CCT/alimentação/regras/núcleo/motor; a data é argumento explícito (`app/ponto/calculador/alimentacao.py:481`, `:517`; `app/core/regua_cct.py:317`). Nenhum segundo escritor persistente novo identificado no trecho examinado; a lavratura continua pelo serviço existente (`app/ponto/services/fechamento.py:1245`, `:1256`, `:1262`). Censo integral executado dos contratos: **[nao verificado]**.

- **RED contra 7e0:** o contador e o novo teste de vigência começando no meio não são RED do split desta volta: 7e0 já possuía resolução CCT diária segundo o anexo anterior, e o diff949 não a altera; passariam por inspeção, execução **[nao verificado]**. São guardas da resolução diária (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:337`, `:343`; anexo `diff_949b.patch` **[nao verificado: sem numeração local do anexo]**).

- **Pouso:** não há prova fornecida de deploy irregular. O relato da cópia declara a cura anterior sem pouso e sujeita a suite, Codex, opus e gates de dinheiro (`app/docs/RELATO.md:14`); cumprimento real desses requisitos: **[nao verificado]**.

## 31/10 16:24 · sessao 53afcb528e7e · - · - · origem=Ronald (tmux)

**E4-1 — achados**

- **Procedente: o selo AST não prova o “caminho do cron” inteiro.** Varre somente os dois arquivos literais de `SITIOS`, inteiros, sem seguir imports/chamadas; coleta atributos, nomes, aliases, keywords e strings, depois intersecta quatro nomes proibidos. A presença de `fatos_do_dia` é apenas nominal, sem exigir chamada ou uso do retorno. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:116`, `:120`, `:136`)
- **Procedente: há falsos negativos e positivos estruturais.** Não vê helper transitivo, nome montado por concatenação ou `filter(celula__status_dia=...)`/`values('celula__status_dia')`, pois guarda somente o prefixo anterior a `__`; pode reprovar string/docstring inocente exatamente igual a nome proibido. Comentários não entram no AST; código morto entra. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:122`, `:130`, `:132`)
- **O mutante literal descrito é morto pelo AST:** `celula.status_dia` adiciona `status_dia` ao conjunto proibido; os exemplos MORDE verificam detecção, não aplicam mutação ao service. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:123`, `:140`, `:144`)
- **Dúvida forte: o funcional pode sobreviver ao mutante descrito.** A docstring afirma que trocar o teste de faixa pela coluna fecha o primeiro caso, mas `encerrar` ainda atravessa `reconciliar` e o gate da premissa. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:15`; `app/chamados/services/feriado_reavaliar.py:47`; `app/chamados/models.py:615`, `:646`, `:699`)
- O nascimento do chamado dispara o cartório, que lavra ATA; `_lampada_folga` altera somente `status_dia/status_rotulo`, preservando ATA. Com ATA de trabalho e lâmpadas apagadas, `_celula_furada` responde `True`; `premissa_regularizada` retorna `False` **antes** do fallback para `espurios`, e o gate recusa o fechamento do único chamado. Nesse caminho, as asserções de permanecer aberto/folga passam mesmo com a seleção errada do mutante. Execução efetiva do TestCase/mutante: **[nao verificado]**. (`app/chamados/signals.py:164`; `app/ponto/services/cartorio.py:1011`, `:1042`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:67`, `:97`; `app/chamados/juizes.py:454`; `app/chamados/models.py:1247`, `:1252`, `:709`)
- A fixture A escolhe o vínculo novo para 15/10; o antigo encerra em 30/09, e o overlay da grade só percorre vínculos selecionados na janela. Isso sustenta a ATA de trabalho no julgamento unitário que precede `_lampada_folga`; falta asserção explícita da ATA/lâmpadas e da exclusão do chamado em `espurios`. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:44`, `:49`, `:88`; `app/escala/utils.py:1254`, `:1261`, `:1548`, `:1785`)
- **Dúvida de relógio no teste:** E4-1 não congela o tempo; criar o chamado alcança `julgar_celula`, que lê `localdate()` e abandona dias futuros. Antes de 15/10/2026, a preparação da ATA segue outro caminho. As condições temporais de execução do selo fornecidas: **[nao verificado]**. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:18`, `:26`, `:30`; `app/chamados/signals.py:180`; `app/ponto/services/cartorio.py:1691`, `:1700`)
- **Dúvida de pré-requisito:** o RELATO manda E4-1 esperar a cura da grade e a sombra com 0044; a cópia mantém `_aplicar_folgas_foto` sem o descarte por vínculo do dia. A realização da cura/sombra exigida não consta do dado fornecido **[nao verificado]**. O cron continua JUIZ, portanto o diff não o rebaixa prematuramente. (`app/docs/RELATO.md:61`, `:63`; `app/escala/utils.py:1109`, `:1118`; `app/config/crons.py:1983`)

**Escritores/leitores relevantes**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.status_dia/status_rotulo` | Produção: `_aplicar_status`, chamada por `lavrar_veredito`; fixture injeta com `update`, deliberadamente fora da porta para fabricar divergência. (`app/ponto/portas/celula.py:701`, `:709`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:52`, `:67`) | Leitor canônico D4; cartório verifica ausência de lavra; fixture consulta `status_dia`. O cron/service atuais não leem essas colunas. (`app/ponto/status_dia.py:171`, `:181`; `app/ponto/services/cartorio.py:726`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:80`; `app/chamados/services/feriado_reavaliar.py:33`) |
| `faixa`, `era_dia_previsto`, `fatos.folga_lancada` | `fatos_do_dia` delega ao período, que produz os dicionários. (`app/ponto/precedencia.py:195`, `:209`, `:355`) | `espurios` exige previsão conhecida e faixa feriado/folga; teste verifica faixa trabalho e usa demais campos no diagnóstico. (`app/chamados/services/feriado_reavaliar.py:33`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:73`, `:94`) |
| `FolgaDia.escala_colaborador/data/calendario_origem` | Fixture cria calendário antigo ou folga manual no novo; produção materializa calendário, cria manual e reparenta só manual. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:88`, `:104`; `app/escala/folgas.py:173`, `:259`, `:405`, `:435`) | Precedência lê por pessoa/data e descarta calendário de vínculo diferente do escolhido; grade lê por vínculo e aplica overlay. (`app/ponto/precedencia.py:236`, `:307`, `:313`; `app/escala/utils.py:1109`, `:1118`) |
| `Chamado.status_local` | Fixture nasce `aberto`; fechamento real passa por `_gravar_estado`, com save e trilha. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:58`; `app/chamados/models.py:618`, `:222`, `:274`) | Universo seleciona `VIVOS`; motor/gates consultam; testes recarregam e exigem aberto/resolvido. (`app/chamados/services/feriado_reavaliar.py:17`; `app/chamados/models.py:637`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:96`, `:109`) |
| `modulo_origem/subcategoria/contexto_json.data_turno` | Fixture cria `batida_ausente/atraso/data_turno`; catálogo declara a chave de data desse módulo. (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:59`; `app/chamados/catalogo/modulos.py:105`) | Universo/`premissa_morta`; dia vem de `dia_do_fato`, que consulta catálogo e fallback `data_turno`. (`app/chamados/services/feriado_reavaliar.py:17`, `:30`, `:38`; `app/chamados/juizes.py:89`, `:104`, `:109`) |
| `FATO_FALTA_DESCOBERTA=False` | Coletor `espurios` fornece o fato ao catálogo depois de ler o juiz. (`app/chamados/services/feriado_reavaliar.py:36`, `:38`) | `premissa_morta` delega a `avaliar`; catálogo responde escopo e vida da premissa. (`app/chamados/catalogo/premissa.py:115`, `:127`, `:144`) |

O juiz de precedência já está registrado; o service delega a ele, e o diff E4-1 apenas acrescenta testes. O fechamento usa a porta existente, sem escritor novo de produção nesse trecho. (`app/core/juizes.py:160`; `app/chamados/services/feriado_reavaliar.py:27`, `:47`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:1`)

## 31/10 16:24 · sessao d8fb872ec851 · - · - · origem=Ronald (tmux)

E4-6 — conclusões por leitura estática:

- **Procedente: o novo rótulo de trabalho não chega ao HTML.** `minha_escala` produz `_st='previsto'` e publica somente a frase em `dias[].status_rotulo`, sem publicar `_st`/`status` (`app/escala/views.py:54`, `:60`). No template, `status_rotulo` só é lido nos ramos `trabalho is None` e folga; o ramo trabalho imprime exclusivamente horários (`app/templates/escala/minha_escala.html:74`, `:76`, `:78`, `:81`, `:84`). Assim, a troca corrige o contexto Python, mas não altera a apresentação de um dia de trabalho (`app/escala/views.py:54`; `app/templates/escala/minha_escala.html:76`).

- **Procedente: não existe tratamento de `previsto` por palavra, cor ou ícone nessa tela.** As cores usam `dia.data == hoje` e o booleano `dia.trabalho`; o dia de trabalho mostra horários, sem ícone específico nem `title` de previsto (`app/templates/escala/minha_escala.html:63`, `:65`, `:66`, `:76`). Isso não cai em um default de status desconhecido: o status nem participa da decisão visual (`app/templates/escala/minha_escala.html:63`, `:74`). Quebra observada em navegador: **[nao verificado]**.

- **Duas cascas, mesmo comportamento.** O template único estende `base_tpl`, cuja fonte é `app_shell`; esta usa `modo_app` para selecionar `base_app.html`/`base.html` (`app/templates/escala/minha_escala.html:1`; `app/core/context_processors.py:194`, `:205`, `:210`). O processador está instalado em `app/config/settings/base.py:82`. O teste injeta `via_app` e `plataforma_sessao`; `carimbo_app` lê esses campos e `modo_app` o chama (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:42`; `app/core/utils.py:52`, `:54`, `:67`).

- **Procedente: a prova HTML das cascas não morde o mutante descrito.** O teste das cascas verifica presença do template base, ausência da frase de `ok` e presença do `title` de folga; não exige a frase de previsto nem sua aparência (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:66`, `:70`, `:72`, `:73`). A frase de `ok` também ficaria invisível no ramo trabalho original, porque esse ramo não lê `status_rotulo` (`app/templates/escala/minha_escala.html:76`). O teste de **contexto**, sim, rejeita `previsto -> ok`, ao exigir o rótulo de previsto nos futuros de trabalho (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:50`, `:53`, `:56`, `:57`). Execução Django/RED histórico: **[nao verificado]**.

- **Dúvida de enquadramento: a categoria diz futuro, a janela inclui hoje.** A view lê `datetime.date.today()`, percorre `range(14)` a partir de zero e dá `previsto` a todo dia de trabalho, inclusive hoje, independentemente de batidas/turno (`app/escala/views.py:26`, `:38`, `:39`, `:54`). O rótulo reutilizado diz literalmente “Dia futuro, ainda sem batida” (`app/ponto/status_dia.py:39`), enquanto a própria autoridade anterior distingue “Dia ainda nao encerrado, sem batida” (`app/colaboradores/services/calendario.py:46`). O selo testa futuros com `data > HOJE`, deixando hoje fora da exigência de rótulo (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:52`, `:53`). Como o rótulo de trabalho está invisível no template, impacto humano atual desse erro de contexto: **[nao verificado]**.

- **Relógio: existente na view, congelado apenas nos testes.** A leitura real continua `datetime.date.today()`; o diff não a introduziu nem a levou a um parâmetro (`app/escala/views.py:26`). O teste usa `freeze_time` (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:20`). A API usa data local de Django, diferente da construção da view; divergência em produção depende do fuso do processo, ausente do dado **[nao verificado]** (`app/api/views.py:926`; `app/config/settings/base.py:93`).

Fontes e consumidores:

| Campo/valor | Quem escreve / fonte | Quem lê |
|---|---|---|
| `trabalho` | Retorno de `ec.eh_dia_trabalho(dia)` (`app/escala/views.py:40`); juiz lê `CelulaDia.trabalha` ou cálculo do vínculo (`app/escala/models.py:1069`, `:1072`, `:1073`) | Escolha de marcos e `_st`, contexto (`app/escala/views.py:42`, `:54`, `:57`); cor e conteúdo (`app/templates/escala/minha_escala.html:65`, `:66`, `:74`, `:76`) |
| `CelulaDia.trabalha` e `dna` | Porta gera/regera a partir de `eh_dia_trabalho_calculado` e `montar_dna` (`app/ponto/portas/celula.py:243`, `:247`, `:260`, `:264`); também restauração/geração diária (`:532`, `:595`, `:612`) | Juízes do vínculo (`app/escala/models.py:1072`, `:1099`) |
| Horários do dia | `ec.marcos_do_dia` (`app/escala/views.py:43`), que lê DNA e só sem marcos usa template (`app/escala/models.py:1097`, `:1099`, `:1106`, `:1108`) | Contexto (`app/escala/views.py:58`, `:59`), template (`app/templates/escala/minha_escala.html:78`, `:81`) |
| `_st` | Ternário da view (`app/escala/views.py:54`) | Indexação de `ROTULO_STATUS` (`app/escala/views.py:60`); também medidor AST (`app/core/medida_leitor_do_dia.py:340`) |
| `ROTULO_STATUS` | Dicionário canônico (`app/ponto/status_dia.py:28`) | Calendário reexporta (`app/colaboradores/services/calendario.py:45`); view importa e indexa (`app/escala/views.py:36`, `:60`); medidor lê AST da autoridade (`app/core/medida_leitor_do_dia.py:124`, `:159`, `:164`); testes comparam (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:15`, `:56`) |
| `dias[].status_rotulo` | Somente contexto local da view; não grava `CelulaDia.status_rotulo` (`app/escala/views.py:55`, `:60`, `:64`) | Template, ramos dúvida/folga (`app/templates/escala/minha_escala.html:75`, `:84`); testes de contexto (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:56`, `:57`, `:64`) |
| `ancora_ausente` | `tipo_ciclo` e `data_ancora_colaborador` do vínculo (`app/escala/views.py:32`) | Contexto e aviso no template (`app/escala/views.py:68`; `app/templates/escala/minha_escala.html:49`) |

A rota HTML é `config/urls.py:27` + `escala/urls.py:11`, com retorno `render` em `escala/views.py:64`. Não há JS de página nem serialização desse contexto no template (`app/templates/escala/minha_escala.html:1`, `:100`). A API **não é consumidora** da string da view: `/api/escala/v2/` tem implementação independente, retorna `dias` sem `status`/`status_rotulo` (`app/api/urls.py:72`; `app/api/views.py:908`, `:945`, `:956`). A API v1 retorna 410 (`app/api/views.py:878`, `:883`). API v2 chama o mesmo juiz de trabalho, mas usa marcos do **template**, ao contrário da view HTML (`app/api/views.py:934`, `:939`; `app/escala/views.py:43`); autoridade registrada dos marcos do dia é `EscalaColaborador.marcos_do_dia` (`app/core/juizes.py:182`). Divergência concreta em dado de produção: **[nao verificado]**.

Medidor:

- `CATEGORIAS`, `PALAVRAS_DA_PREVISAO`, `NAO_E_LEITOR`, teto e `DECLARADOS` são escritos como constantes no próprio módulo (`app/core/medida_leitor_do_dia.py:259`, `:262`, `:263`, `:285`, `:289`, `:296`). O leitor de produção das exclusões é `censo`, que lê categoria/vocabulário permitido e distribui registros entre `excluidos` e `status` (`app/core/medida_leitor_do_dia.py:348`, `:350`, `:351`, `:353`). Os testes leem categorias/teto/motivos/disjunção (`app/core/tests/test_medida_leitor_do_dia.py:73`) e especificamente a exceção de previsão (`:85`).

- **Procedente: previsão tem predicado lexical; não é exclusão incondicional.** `pal ⊆ PALAVRAS_DA_PREVISAO` decide exclusão; `ok` devolve o item ao status, e `novos` usa diferença contra `DECLARADOS` (`app/core/medida_leitor_do_dia.py:348`, `:349`, `:353`, `:377`). Fiz apenas sondagem Python `-B`, em memória: detector retornou base `['escala_suspeita','folga','previsto']`, mutante `['escala_suspeita','folga','ok']`, três sítios em ambos; o predicado aceita base e rejeita mutante, conforme `app/core/medida_leitor_do_dia.py:229`, `:349`.

- **Limite procedente: o predicado não prova “futuro” nem “pergunta à grade”.** Inspeciona apenas os literais já reconhecidos; não valida data, ausência de batidas ou autoridade consultada (`app/core/medida_leitor_do_dia.py:340`, `:348`). Indireção sem literal, booleanos/relógio e HTML/JS estão explicitamente fora da visão (`app/core/medida_leitor_do_dia.py:317`). Uma exclusão que desapareça vira `exclusoes_mortas`, cobrada pela suíte (`app/core/medida_leitor_do_dia.py:384`; `app/core/tests/test_medida_leitor_do_dia.py:74`).

- Os consumidores externos do resultado são a suíte (`app/core/tests/test_medida_leitor_do_dia.py:29`) e `bin/leitor_dia_estado.py`, que carrega o módulo e chama `censo` (`bin/leitor_dia_estado.py:25`, `:32`, `:35`); `gerar_estado` importa esse leitor (`bin/gerar_estado.py:290`). O selo S3 importa daqui o detector de **valor**, não o predicado de previsão (`app/ponto/tests/test_s3_placar_exercicio.py:38`).

As fixtures E4-6 são escritas por `setUpTestData` e pela sessão do cliente (`app/escala/tests/test_e4_6_minha_escala_e_previsao.py:23`, `:26`, `:27`, `:28`, `:29`, `:30`, `:32`, `:33`, `:36`, `:43`); os leitores verificadores estão nos testes de contexto e render (`:50`, `:59`, `:66`). Não executar esses testes preservou a exigência de nenhum arquivo/banco criado; RED de execução continua **[nao verificado]**.

## 31/10 16:23 · sessao e69e5490d2c0 · - · - · origem=Ronald (tmux)

**E4-7: troca de universo correta no recorte alterado, com uma promessa documental maior que a cura.** `entradas_hoje` passa pela autoridade `batidas_apuraveis`; `bateu_hoje_ids` continua consulta independente e crua, embora a docstring nova os associe. — `app/core/services/painel_op.py:51`, `app/core/services/painel_op.py:239`, `app/core/tests/test_e4_7_painel_entrada_retratada.py:3`.

**Achados**

- **Procedente — atividade crua sobrevive.** `_cids_pn` também consulta `Batida` diretamente, mas contém `retratada_em__isnull=True`; `bateu_hoje_ids` não contém esse filtro, admite qualquer tipo e alimenta `_teve_ativ`/`classificar_cobertura`. O selo novo só afirma `entrada`, `entrada_id` e status de linha; não verifica essa atividade. — `app/core/services/painel_op.py:60`, `app/core/services/painel_op.py:239`, `app/core/services/painel_op.py:258`, `app/core/tests/test_e4_7_painel_entrada_retratada.py:78`.
- **Limite desse achado:** não concluir que a atividade retratada muda a cobertura visível atual. O juiz usa atividade somente para distinguir `encerrado` de `inativo` quando previsto/presente são zero; o painel converte os dois em `encerrado`. — `app/ponto/selecao_periodo.py:237`, `app/core/services/painel_op.py:242`, `app/core/services/painel_op.py:259`.
- **Procedente — “mesmo universo do juiz” exige precisão.** A marca de retratação coincide, mas as janelas não: entrada do painel usa 24h móveis sem teto; o juiz de turno carrega as três datas locais `hoje-2..hoje`. — `app/core/services/painel_op.py:46`, `app/core/services/painel_op.py:53`, `app/ponto/turnos.py:2186`, `app/ponto/turnos.py:2190`.
- **Não aparece alteração adicional do universo no trecho substituído:** `ini` continua `timestamp >= agora−24h`; `colab=None` não restringe colaboradores; `fim=None` não acrescenta teto; o chamador mantém `tipo='E'`, `select_related('colaborador')`; a ordenação por `timestamp` vem do helper e o dict mantém a última entrada por colaborador. A única condição acrescentada é `retratada_em IS NULL`. — `app/core/services/painel_op.py:46`, `app/core/services/painel_op.py:51`, `app/ponto/turnos.py:2844`.
- **Procedente, anterior ao diff — nome/comentário mentem sobre a duração:** `_14h_atras` e comentário “janela 14h” usam efetivamente `timedelta(hours=24)`. — `app/core/services/painel_op.py:45`.
- **Derivação paralela existente continua:** o painel escolhe `normal/geofence/turno_aberto/encerrado/ausente/folga`; o medidor mantém explicitamente esse sítio nos declarados, com quatro palavras medidas. A alteração E4-7 não remove essa dívida nem cria nova regra de classificação. — `app/core/services/painel_op.py:148`, `app/core/services/painel_op.py:169`, `app/core/medida_leitor_do_dia.py:312`.
- **Relógio:** o construtor lê `timezone.now()` uma vez e passa `agora` ao juiz; o helper novo não lê relógio. O `timezone.now()` do teste está sob `freeze_time`. Não há nova leitura solta introduzida por E4-7. — `app/core/services/painel_op.py:32`, `app/core/services/painel_op.py:65`, `app/ponto/turnos.py:2844`, `app/core/tests/test_e4_7_painel_entrada_retratada.py:32`, `app/core/tests/test_e4_7_painel_entrada_retratada.py:56`.

**Escritores → leitores relevantes**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `Batida.colaborador/timestamp/tipo` e coordenadas | criação pela porta `registrar_batida`; geo sanitizado antes do `create`; fábrica por `semear`, guardada como ensaio | helper filtra retratação/data; painel seleciona E e chaveia por colaborador |
| | `app/ponto/registro_batida.py:94`, `app/ponto/registro_batida.py:105`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:331` | `app/ponto/turnos.py:2844`, `app/core/services/painel_op.py:51` |
| `tipo` de batida existente | `flip_tipo → anotar`; porta permite esse campo | `.filter(tipo='E')` decide entrada candidata |
| | `app/ponto/services/flip_batida.py:25`, `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:252` | `app/core/services/painel_op.py:53` |
| `retratada_em` | `retratar_batida → anotar`; desfazer escreve `None` pela mesma porta; fixture nova escreve cru por `update` | `batidas_apuraveis`, candidatos e carregador do juiz |
| | `app/ponto/registro_batida.py:267`, `app/ponto/services/retratar_humano.py:207`, `app/core/tests/test_e4_7_painel_entrada_retratada.py:56` | `app/ponto/turnos.py:2845`, `app/core/services/painel_op.py:61`, `app/ponto/turnos.py:2192` |
| `entradas_hoje` | dict local da última E apurável por colaborador | carga de pings e variável `entrada` por linha |
| | `app/core/services/painel_op.py:51` | `app/core/services/painel_op.py:102`, `app/core/services/painel_op.py:143` |
| `entrada` → status | entrada presente sem turno vivo produz `encerrado`; ausência de entrada com trabalho produz `ausente` | filtro de status, contadores, agrupamento por posto, ausentes e ordenação dos colaboradores |
| | `app/core/services/painel_op.py:169` | `app/core/services/painel_op.py:179`, `app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:232`, `app/core/services/painel_op.py:254`, `app/core/services/painel_op.py:263` |
| `entrada`, `entrada_id`, `data_turno`, `lat/lng` | valores da entrada selecionada; `data_turno` prefere turno vivo | entrada/hora e ID geo consumidos pelo card; consumidores de `data_turno` encontrados nos testes; consumidor de `lat/lng` desse retorno **[nao verificado]** | [nao verificado]
| | `app/core/services/painel_op.py:188` | `app/templates/core/_painel_card.html:64`, `app/templates/core/_painel_card.html:68`, `app/core/tests/test_painel_op_turno_aberto_ancora.py:50`, `app/core/tests/test_painel_op_turno_pelo_juiz.py:80` |
| `presenca_geo` | `classificar_presenca_turno(entrada, pings)`; pings carregados apenas para entradas selecionadas | classe/caption da bolinha geo |
| | `app/core/services/painel_op.py:102`, `app/core/services/painel_op.py:183`, `app/ponto/presenca.py:50` | `app/templates/core/_painel_card.html:68` |
| status/agregados/postos | construtor | view inicial, fragmento e lote; card pinta estado, botão de substituição e cobertura; stats pinta contadores; JS filtra atributos do card |
| | `app/core/services/painel_op.py:185`, `app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:261` | `app/core/views.py:607`, `app/core/views.py:632`, `app/core/views.py:679`, `app/templates/core/_painel_card.html:2`, `app/templates/core/_painel_card.html:48`, `app/templates/core/_painel_card.html:72`, `app/templates/core/_painel_stats.html:32`, `app/templates/core/painel_operacional.html:177` |

O ID da entrada selecionada também vira URL HTMX de detalhe geo; a view reabre `Batida` por PK e tipo E e chama `resumo_presenca_turno`. — `app/templates/core/_painel_card.html:69`, `app/ponto/urls.py:79`, `app/ponto/views.py:3236`, `app/ponto/presenca.py:108`.

Página e lotes usam o mesmo card: página inclui `_painel_grid`, grid inclui `_painel_card`; lote inclui `_painel_card`; fragmento inclui grid e stats. — `app/templates/core/painel_operacional.html:152`, `app/templates/core/_painel_grid.html:1`, `app/templates/core/_painel_lote.html:7`, `app/templates/core/_painel_fragment.html:1`.

**Mordida estática E4-7**

Repor a consulta crua faz A selecionar sua única entrada retratada e viola `entrada is None`/`entrada_id is None`; C seleciona a retratada mais recente (07:30) e viola a comparação com 07:00. Uma “cura” que zere todas as entradas viola B, cujo status deve continuar `encerrado` e cuja entrada deve continuar 07:00. Essa é conclusão do fluxo do código; execução/RED observado **[nao verificado]**. — `app/core/tests/test_e4_7_painel_entrada_retratada.py:64`, `app/core/tests/test_e4_7_painel_entrada_retratada.py:66`, `app/core/tests/test_e4_7_painel_entrada_retratada.py:69`, `app/core/tests/test_e4_7_painel_entrada_retratada.py:78`, `app/core/services/painel_op.py:169`.

O diff de produção deste sítio só muda leitura; as escritas cruas novas são montagem de fixture e retratação de teste, não uma nova porta produtiva. — `app/core/services/painel_op.py:50`, `app/core/tests/test_e4_7_painel_entrada_retratada.py:35`, `app/core/tests/test_e4_7_painel_entrada_retratada.py:56`.

## 31/10 16:22 · sessao 2ca4f78aa1d8 · F1C4-3-EMISSORES · censo · origem=Code

**Há seis classificações sustentáveis e dois achados cuja classe fica [nao verificado].** O material não permite concluir “zero falhas do Codex”: falta estabelecer quando os defeitos específicos dos itens 3 e 4 entraram nos hashes examinados. (`opus_v11.txt:4–5`; `v8.q:1–2`.)

Nos anexos colados, conto linhas desde a primeira linha após cada identificador, incluindo linhas vazias. Não atribuo descarte a `triagens.txt`, cujo bloco veio vazio; os registros locais de fila não individualizam os defeitos finais dos itens 3 e 4. (`app/docs/RELATO.md:29`; `app/docs/BACKLOG.md:822`; descarte específico: **[nao verificado]**.)

1. **Corte/backlog divergentes da migração — (c), alteração posterior à última chamada guardada.** (`opus_v11.txt:2`; `v8.q:1`.)

   **Pergunta:** “Ha sitio de producao que ainda responde […] por conta propria […] sem perguntar a chamados/juizes.py::dia_sem_trabalho?” (`v4.q:15–16`.)

   **Resposta:** “a migração está declarada no corte, mas não realizada nesse sítio.” (`v4.r:42`.)

   Codex viu o antecedente — supra com predicado próprio —, mas o achado final é outro: **migrar em `b3bef403` e deixar a documentação anterior**. Não cabe transformar o descarte do antecedente em descarte desse defeito novo. (`v1.r:88`; `opus_v11.txt:2`.)

   **Entrada:** tema supra já na **v1**; corte discutido na **v4**; contradição causada pela migração documentada na **v11**, após a chamada v8. (`v1.r:88`; `v4.r:42`; `opus_v11.txt:1–2`; `v8.q:1`.)

2. **Número atribuído ao universo errado na mensagem — (c), dado de produção/mensagem final.** (`opus_v11.txt:3`; `CLAUDE.md:557`.)

   **Pergunta:** “antes, sombra com novos CHAMADOS por emissor; ate 50 pousa”. (`v2.q:2`.)

   **Resposta:** “A sombra com novos CHAMADOS por emissor, e o atendimento ao teto de 50, são **[não verificado]** no material fornecido.” (`v2.r:1`.)

   A fonte local associa **0/127.784** a ata explícita **`indefinido`**, confirmando a diferença de universo apontada pelo opus. O valor **158/143.452** está sustentado apenas pelo dado raspado fornecido. (`app/docs/BACKLOG.md:806`; `opus_v11.txt:3`.)

   **Entrada:** o número anterior já constava da obra precedente; a frase acusada está documentada na **v11**. Primeira volta em que essa frase foi escrita: **[nao verificado]**. (`app/docs/BACKLOG.md:806`; `opus_v11.txt:1–3`.)

3. **Catraca aceita o nome, mas deixa passar `ata=cel.ata` — classe [nao verificado].**

   **Pergunta:** “Os testes novos mordem a reversao de cada um dos 4 sitios individualmente?”; checklist: “selo/RED sem mordida”. (`v1.q:6–7`.) [nao verificado]

   **Resposta:** “também não exigem estruturalmente delegação ao juiz — um clone equivalente passa nos asserts de retorno.” (`v1.r:105`.)

   Isso comprova que Codex viu **uma fragilidade de delegação nos testes antigos**; não comprova que viu a catraca específica de `com_insumo_dia_sem_trabalho` nem o mutante descrito pelo opus. (`v1.r:105`; `opus_v11.txt:4`.) [nao verificado]

   **Entrada:** tema de proteção estrutural presente na **v1**; primeira volta da catraca específica: **[nao verificado]**. Se já estava num hash examinado, pode caber **(a)**; se nasceu depois da v8, pode caber **(c)**. Não há evidência para escolher nem para atribuir **(b)**. **[nao verificado]**

4. **`_protesta` antecipa o juiz; cláusula inatingível; omissão em `_C18` — classe integral [nao verificado].**

   **Pergunta:** “A pergunta registrada em `_C18` e respondida por outra funcao […] com predicado igual ou diferente?” (`v4.q:14–16`.)

   **Resposta anterior:** “Protesto e espera pelo fim do turno usam `trabalha is not False`.” (`v1.r:87`.)

   Portanto, **o predicado do cartório foi visto**. A cópia também o contém literalmente: `return _cel.trabalha is not False and _acesos(x, _ata) is not True`. (`v1.r:87`; `app/ponto/services/cartorio.py:1204–1205`.)

   A consequência final — cláusula do juiz inatingível após a delegação — não é o mesmo achado que simplesmente listar esse predicado. Quando surgiu essa delegação, e se a omissão específica de `_C18` já estava nos hashes examinados: **[nao verificado]**. (`opus_v11.txt:5`.)

   **Entrada:** sítio/predicado na **v1**; `_C18` entra explicitamente na pergunta da **v4**; primeira volta da combinação final: **[nao verificado]**. (`v1.r:87`; `v4.q:11–16`.)

5. **Sombra só do supra; cartório não medido; efeito com tipo `None` — (c), quanto à medição final.** (`opus_v11.txt:6`; `CLAUDE.md:557`.)

   **Pergunta:** “antes, sombra com novos CHAMADOS por emissor”. (`v2.q:2`.)

   **Resposta:** a sombra e o teto permaneciam “**[não verificado]** no material fornecido”. (`v2.r:1`.)

   A semântica geral de `None` já fora percebida: “tipo ausente, `None` ou vazio não satisfaz `A`”. Isso não demonstra exame da migração final nem cobertura do caminho final do cartório. (`v1.r:7`; `opus_v11.txt:6`.)

   **Entrada:** semântica de tipo ausente/`None` na **v1**; sombra explicitamente exigida na **v2**; medição específica acusada documentada na **v11**. Sua primeira execução e a volta da mudança comportamental: **[nao verificado]**. (`v1.r:7`; `v2.q:2`; `opus_v11.txt:6`.)

6. **`v11_green.log` vazio; suíte sem hash — (c), suíte/log.** (`opus_v11.txt:7`; `CLAUDE.md:557`.) [nao verificado]

   **Pergunta relacionada:** checklist “selo/RED sem mordida”; não há pedido específico de conferência desse log final. (`v1.q:7`; conteúdo anterior de `v11_green.log`: **[nao verificado]**.)

   **Resposta:** “Execução de testes/mutantes, respeito aos tetos após a mudança e incidência por colaborador em produção: **[não verificado]**.” (`v1.r:3`.) [nao verificado]

   **Entrada:** arquivo vazio documentado na **v11**. Quando a suíte passou a não imprimir hash: **[nao verificado]**. (`opus_v11.txt:7`.) [nao verificado]

7. **`c5c43360` sem linha LEI-AKITA — (e), NÃO PERGUNTADO sobre a mensagem.** (`v3.q:1–30`; `opus_v11.txt:8`.)

   **Pergunta mais próxima:** “Aponte tambem qualquer outro defeito no delta que estas perguntas nao cobrem.” Ela não solicita auditoria da mensagem nem da linha LEI-AKITA. (`v3.q:30`.)

   **Resposta:** abre com “Reprovo a volta 3 pelo aval literal”, discutindo a fronteira de competência. Não há trecho específico apontando a ausência da linha na mensagem nos registros fornecidos. (`v3.r:1`; trecho específico: **[nao verificado]**.)

   **Entrada:** fábrica presente desde a **v1**; commit acusado já identificado na **v3**. Portanto, não cabe justificar este item como “commit ainda não escrito”. A ausência efetiva da linha tem como evidência fornecida o relato do opus. (`v1.r:1`; `v3.q:1`; `opus_v11.txt:8`.) [nao verificado]

8. **Mensagem afirma ausência de arquivos em PROTEGE — (e), NÃO PERGUNTADO, com divergência material na evidência.** (`v2.q:1–20`; `v3.q:1–30`; `v4.q:1–21`; `v8.q:1–12`; `opus_v11.txt:9`.)

   **Pergunta mais próxima:** “Leis citadas pelos sitios (grep LEIS/CORTES) e app/docs/JUIZES.md vao anexos.” (`v2.q:20`.)

   **Resposta mais próxima:** “A L-061 exige leitura da célula/ata; ela não demonstra equivalência dos predicados sobreviventes.” Isso discute a lei, sem conferir a afirmação da mensagem contra PROTEGE. (`v2.r:139`.)

   **Ressalva:** nesta cópia, **PROTEGE está vazio nas três leis citadas pelo opus**. Os arquivos aparecem em outra coluna; a documentação distingue expressamente DONO de PROTEGE. Assim, a cópia não confirma essa parte do achado final. (`app/docs/LEIS.md:43–48`; `app/docs/LEIS.md:57`; `app/docs/LEIS.md:81`; `app/docs/LEIS.md:91`; `app/docs/LEIS.md:95`.)

   **Entrada:** primeira volta da afirmação na mensagem: **[nao verificado]**. O resumo não identifica seu commit; não há base para declará-la posterior à v8. (`opus_v11.txt:9`.) [nao verificado]

**Contagem das classificações sustentadas acima** — zeros significam ausência de atribuição comprovada neste censo, não prova de inexistência de falhas. (`CLAUDE.md:554–558`; pendências dos itens 3 e 4: **[nao verificado]**.)

| Classe | Quantidade | Achados |
|---|---:|---|
| (a) Não viu com tema presente | 0 comprovados | — |
| (b) Viu e triagem descartou | 0 comprovados | — |
| (c) Fora do alcance | 4 | 1, 2, 5, 6 |
| (e) Não perguntado | 2 | 7, 8 |
| (d) Nenhum viu | 0 comprovados | — |
| **Classe [nao verificado]** | **2** | **3, 4** |
| **Total** | **8** | |

## 31/10 16:20 · sessao 0d61e8d5537d · PADRAO-DE-CIMA · leitura · origem=Code

O ponto decisivo é separar **o B3 desta cópia** do mecanismo futuro: hoje ele dispara com três reprovações, rearma por CLI e não consome linha liberadora; a pergunta no topo apenas cala a cobrança do Stop. O contrato documentado já descreve hash e consumo, mas isso ainda não está nesses caminhos executáveis. A implementação triestado da volta 4 é [nao verificado]. ([bin/disjuntor.py:68](/home/ronald/codex_ws/5693e4622c2055d69d24d99ea57d816f98b001c8/bin/disjuntor.py:68), `bin/disjuntor.py:585`, `bin/disjuntor.py:947`; `bin/hook_stop_fila1.py:1049`; `app/docs/RELATO.md:11–13`.)

**1) As formas existentes não constituem hoje um registro uniforme de eventos.** Os exemplos abaixo têm diferenças relevantes para contar ocorrências e delimitar 24 horas. (`app/docs/RELATO.md:4`, `app/docs/RELATO.md:12`, `app/docs/RELATO.md:141`; `app/docs/RELATO-ARQUIVO.md:983`.)

| Forma observada | Exemplo e localização | O que permite extrair |
|---|---|---|
| Modelo literal de linha | `DESENHO <obra>: lei=... pergunta=... <arquivo>:<linha>` — `app/docs/RELATO.md:12` | É **exemplo dentro do contrato**, com placeholders; contá-lo produziria evento fictício. |
| Reprovação/desenho no topo | `REPROVA-E-DESENHO V3-RETOMA (12:4x, ...)` — `app/docs/RELATO.md:4` | Obra explícita; hora aproximada, sem data própria. Cita motor, alimentação, regras e núcleo, sem entidade única declarada. |
| Pergunta B3 respondida | `[RESPONDIDA 23:5x...] PERGUNTA DE DESENHO O261-E-F5 (...), 30/10 23:4x` — [RELATO.md:141](/home/ronald/codex_ws/5693e4622c2055d69d24d99ea57d816f98b001c8/app/docs/RELATO.md:141) | Obra e data explícitas; horas aproximadas da resposta e da pergunta são diferentes. Há referências a chamado, célula, folha e funções distintas. |
| Pergunta histórica com texto intermediário | `PERGUNTA DE DESENHO (nao devolve turno) -- **GEO-SILENCIO-DE-PING...` — `app/docs/RELATO-ARQUIVO.md:983` | Não segue imediatamente `DESENHO <obra>`; não tem hora nem arquivo:linha próprios. Referências aparecem nos itens seguintes, em `:985` e `:987`. |
| Pergunta respondida e riscada | `~~(pergunta de DESENHO -- ...` — `app/docs/RELATO.md:947` | A data disponível é da resposta. Hora de abertura e identidade normalizada do evento: [nao verificado]. |
| Classificação de achado/custo | `(DESENHO)`, `= DESENHO`, `DESENHO, NAO DEFEITO` — `app/docs/RELATO.md:1302`, `:1333`, `:4058`; `app/docs/RELATO-ARQUIVO.md:6876`, `:7130` | São menções/classificações em prosa; buscar somente a palavra contaria coisas diferentes de uma linha de evento. |
| Título de arquitetura | `### O DESENHO DA LINHA (ponto.OcorrenciaHE)` — `app/docs/RELATO-ARQUIVO.md:22802` | Explicita um modelo, mas não uma ocorrência no formato do aval; o título anterior tem data aproximada em `:22783`. |

`23:4x` fornece uma dezena de minutos, não um instante para decidir uma borda estrita de 24 horas. O selo existente transforma `HH:Mx` em `HH:M0`, usa São Paulo e busca ano no `git blame`, mas verifica **títulos de seção**, não timestamps de cada pergunta. (`app/docs/RELATO.md:141`; `bin/tests/test_hora_da_secao_nao_e_futura.sh:21`, `:25`, `:41–49`.)

Também não é seguro herdar sempre a data da seção anterior: o preâmbulo do RELATO contém entradas de 31/10 antes da primeira seção, datada de 30/10; o histórico recebeu blocos no fim e títulos que perderam o cabeçalho original. O mtime é alterado por escritores de máquina e não demonstra a hora individual do DESENHO. (`app/docs/RELATO.md:41`, `:185`; `app/docs/RELATO-ARQUIVO.md:23–30`; `app/core/esteira_vigia.py:879–884`.)

**2) O repo já responde bem sobre sítios de escrita; não oferece, nas fontes examinadas, um resolvedor geral de toda leitura ou escrita para uma entidade única.** O principal componente reaproveitável é [censo_escritas.py:469](/home/ronald/codex_ws/5693e4622c2055d69d24d99ea57d816f98b001c8/app/core/censo_escritas.py:469), cuja saída é `{arquivo, linha, entidade, op, expr}`; as demais fontes têm escopos diferentes. (`app/core/portas.py:9`; `app/core/juizes.py:3`; `app/core/mapa/contrato.py:13`.)

| Fonte existente | Resposta que realmente fornece |
|---|---|
| `core/censo_escritas.py` | Varredura AST, sem Django, de construtores e operações ORM; identifica variáveis amarradas ao modelo e `self` na classe. Marca inferências por campo, `update_fields`, relação e nome. (`app/core/censo_escritas.py:3`, `:6–45`, `:401`.) |
| `core/portas.py::PORTAS` | **Entidade → módulos autorizados a escrever**; não afirma que toda linha do módulo pertence à entidade. (`app/core/portas.py:9`.) |
| `core/portas.py::FAMILIA_DE` | Entidade → família. `CelulaDia` e `FuroDiario`, por exemplo, compartilham família; isso não os torna uma entidade só. (`app/core/portas.py:40–44`.) |
| `core/juizes.py::JUIZES` | Pergunta/família → autoridade `arquivo::função`; não é catálogo de modelos lidos. (`app/core/juizes.py:3–14`.) |
| `core/mapa/contrato.py` | Inscrição de módulo: `@bolha`, entradas, saídas e chamadas; `@bolha` designa família, sem campo estruturado de modelo lido/escrito. (`app/core/mapa/contrato.py:13–25`.) |
| Censos especializados | Há leitura/escrita por modelo em universos específicos, como Batida; não é cobertura geral de entidades. (`bin/esmeril_batida_censo.py:184–200`, `:234–262`.) |
| `esmeril_chamado_censo.py::_onde` | Já associa intervalos de linhas a classe/função por AST; resolve o escopo da citação, não a entidade. (`bin/esmeril_chamado_censo.py:76–89`.) |

Um caminho determinístico **proposto, ainda não implementado [nao verificado]**, seria normalizar o caminho relativamente a `app/`, localizar a expressão e sua função por AST, resolver modelos explícitos e consultar os sítios do censo; aceitar entidade única com evidência, preservando a distinção entre atribuição direta e inferência. As bases existentes são `app/core/censo_escritas.py:361`, `:451`, `:469` e `bin/esmeril_chamado_censo.py:76`.

Os casos que impedem escolher automaticamente uma entidade são concretos:

- **Arquivo com várias entidades:** `ponto/portas/celula.py` é porta de `CelulaDia` e `FuroDiario`; fechamento é porta de `FechamentoMensal` e `PeriodoFechado`. Inverter o mapa e escolher a primeira entidade perderia informação. (`app/core/portas.py:111–112`, `:495–496`.)
- **Origem desconhecida:** parâmetros, `form.save()` e `apps.get_model` podem ficar no balde `entidade=None`; inferência por nome é declarada fraca. (`app/core/censo_escritas.py:38–45`, `:429`.)
- **Falha confundida com vazio:** erro sintático retorna `[]`; erro de leitura é ignorado na varredura. Esses retornos, isoladamente, não sustentam `LIVRE` num juiz triestado. (`app/core/censo_escritas.py:451–456`, `:485–488`.)
- **Cadastro AST imperfeito:** `modelos()` aceita classes com base em `models.py`, inclusive a exceção `ResolvedoraCruzada`; também chaveia pelo nome simples da classe. (`app/core/censo_escritas.py:89–111`; `app/core/portas.py:60–65`.)
- **Nome coloquial versus identidade:** a classe principal é `ChamadoColaborador`; um alias normativo `Chamado → ChamadoColaborador` para este instrumento é [nao verificado]. (`app/chamados/models.py:21`; `app/core/portas.py:58`.)
- **Referência histórica versus hash atual:** o próprio histórico corrige caminho inexistente e número de linha deslocado; interpretar uma referência antiga diretamente na árvore atual pode apontar outra expressão. (`app/docs/RELATO-ARQUIVO.md:990`.)

**3) `_obra_de` reconhece identidade textual da obra, sem consultar entidade ou programa.** Ele procura âncoras como `obra X`, `Obra: X`, markdown e `leitura ... da X`, valida os tokens e escolhe a primeira ocorrência válida entre essas âncoras. Aceita IDs com hífen ou letras+algarismos; IDs sem hífen podem carregar cauda distintiva, como `O261 (C)`. (`bin/disjuntor.py:78–93`; [bin/disjuntor.py:478](/home/ronald/codex_ws/5693e4622c2055d69d24d99ea57d816f98b001c8/bin/disjuntor.py:478), `:492–503`.)

No **Agent**, o B3 atual chama `_obra_de` somente sobre `tool_input.prompt`, para `subagent_type == 'verificador'` e leitura final; a descrição não é usada nesse reconhecimento. Leitura de desenho é excluída por `_eh_final`. Construtores e verificações sem “final” não entram nessa trava B3. (`bin/disjuntor.py:94–98`, `:473–475`, `:944–954`.)

No **Codex**, o wrapper exige `CODEX_OBRA` e `CODEX_PAPEL` e escreve `OBRA:` no prompt. Porém o parser do PreToolUse extrai apenas papel, volta e anexos: **não extrai `CODEX_OBRA` nem chama `_obra_de` para obter a obra da consulta**. Isso precisará de tratamento próprio para cumprir o anexo. (`bin/codex_pergunta.sh:43–48`, `:118–131`; `bin/disjuntor.py:849–862`, `:938–954`.)

Há vínculos documentais obra→programa: O280 aparece como E3, V3-BATERIA como E1 e O261-E-CELULA-20-UM-DONO como E2 do PROGRAMA-CELULA-E-O-DIA. As formas variam entre `PROGRAMA:`, “esta obra é a E2” e “linha da E4”; parser único dessas relações e herança entre aliases/subobras são [nao verificado]. (`app/docs/BACKLOG.md:1398`, `:1399`, `:782`, `:774`.)

Programa tampouco equivale a uma tabela: o ADR reúne status em `CelulaDia` e valor em `DiaPago`. A prioridade entre agrupar por entidade e por programa precisa ser declarada, senão obras de tabelas diferentes podem acabar no mesmo grupo por programa. (`app/docs/ADR/0002-celula-e-o-dia.md:4–8`; política de prioridade: [nao verificado].)

**4) O anexo deixa decisões necessárias em aberto.** As observações sobre suas omissões abaixo se referem ao texto fornecido no prompt, sem arquivo local com numeração verificável: [nao verificado].

- **Disparo e ação incompletos:** “segunda ocorrência na mesma entidade/programa em 24 h” aparece em MORDE, mas falta como regra operacional explícita. Falta integralmente a linha do aval `PADRAO-DE-CIMA <entidade>: classe=... padrao=... armadilha=... obra=...`, seus campos obrigatórios, validação e autor responsável. [nao verificado]
- **Universo de eventos:** acrescenta `PERGUNTA DE DESENHO` sem dizer se é alias do marcador ou ocorrência independente; não define exclusão de exemplos, perguntas respondidas, repetições e movimentação para arquivo. Esses formatos existem nos documentos. (`app/docs/RELATO.md:12`, `:141`; `app/docs/RELATO-ARQUIVO.md:23–40`; política: [nao verificado].)
- **Tempo e agrupamento:** faltam data/ano/fuso, tratamento de `HH:Mx`, timestamp ausente/futuro, vários modelos, aliases, várias referências e prioridade entidade/programa. [nao verificado]
- **`NAO_SEI = nega`:** não define quais chamadas negar quando a entidade é desconhecida. Negar indiscriminadamente ultrapassaria o escopo “obras daquela entidade” do aval. Além disso, o envelope atual captura exceção e libera; a política nova precisa chegar até ele. (`bin/disjuntor.py:989–990`; escopo novo: [nao verificado].)
- **Consumo e rearme:** “mesma mecânica de `linha_libera_b3`” remete a código futuro. O texto B3 disponível exige frescor, SHA consumido e transferência para `liberado`; o código atual só transfere para `rearmado` por CLI. Falta definir se PADRAO libera uma chamada ou um episódio, como trata a terceira ocorrência e como evita redisparar imediatamente pelas mesmas duas linhas ainda dentro das 24 h. (`app/docs/RELATO.md:12`; `bin/disjuntor.py:585–595`; política nova: [nao verificado].)
- **Falha e limite:** não cobre leitura parcial, fonte sintaticamente inválida, JSON corrompido, trava ocupada, prazo/tamanho de varredura e falha ao persistir consumo. O estado atual serializa por flock, mas mantém a decisão mesmo quando a gravação falha; isso exige regra específica para garantir consumo 1x. (`bin/disjuntor.py:219–259`; política nova: [nao verificado].)
- **Medida:** o painel atual conta disparos pelo primeiro token da regra e negativas globais, sem liberações por entidade; a lista exibida também é fixa. Faltam formato dos eventos novos, denominadores e apresentação de medida ilegível. (`bin/disjuntor.py:1030–1055`, `:1147–1160`; medida nova: [nao verificado].)
- **Quem desliga:** `pausar.sh` é escritor genérico de arquivo de pausa; não é, por si, integração com o disjuntor. A chave lida hoje é `DISJUNTOR_DESLIGADO`, global e reservada ao Ronald; comandos do Code que a nomeiem são negados. Falta escolher chave global ou específica, cobertura, condição de retorno e autorização de uso de `pausar.sh`. (`bin/pausar.sh:31–47`; `bin/disjuntor.py:128–157`; decisão nova: [nao verificado].)

O MORDE do anexo também precisa distinguir “duas entidades diferentes” de “duas entidades diferentes **dentro do mesmo programa**”: o aval permite agrupamento por programa, mas não estabelece a precedência entre os dois critérios. Resultado esperado desse caso: [nao verificado].

**5) Estes são os escritores e leitores comprovados que delimitam os arquivos candidatos; o estado específico de PADRAO-DE-CIMA ainda é [nao verificado].**

| Arquivo/estado | Quem escreve | Quem lê |
|---|---|---|
| `app/docs/RELATO.md`, topo humano | O código reconhece escrita manual e preserva o topo; autoria concreta das linhas: [nao verificado]. (`app/core/esteira_vigia.py:829–840`.) | Stop lê duas primeiras linhas úteis; B3 procura DESENHO e identidade da obra nelas. (`bin/hook_stop_fila1.py:411–444`.) |
| `RELATO.md`, linhas de máquina | Disjuntor chama `relato_negacao → esteira_vigia.no_relato`, com flock do diretório e temporário único. Outros escritores declarados são Edit manual, gerador de cortes e deploy agendado, fora dessa trava. (`bin/disjuntor.py:209–211`, `:988`; `app/core/esteira_vigia.py:834–840`.) | Stop verifica afirmações sem prova; publicação lê e raspa o documento. (`bin/hook_stop_fila1.py:1171–1183`; `bin/relato.sh:54–56`, `:98–104`.) |
| `app/docs/RELATO-ARQUIVO.md` | Conduta manda mover material antigo; cabeçalho registra movimentos e append. Escritor automático de rotação: [nao verificado]. (`CLAUDE.md:610–613`; `app/docs/RELATO-ARQUIVO.md:23–40`.) | B3 atual aponta somente para RELATO vivo. Leitura do arquivo por PADRAO-DE-CIMA: [nao verificado]. (`bin/disjuntor.py:57`; `bin/hook_stop_fila1.py:34`, `:430`.) |
| Fontes de modelos, portas e juízes | São declarações no código; autoria efetiva de cada alteração: [nao verificado]. (`app/core/portas.py:98`; `app/core/juizes.py:156`.) | Censo lê modelos/fontes por AST; mapa lê os registros de portas e juízes. (`app/core/censo_escritas.py:89`, `:469`; `app/core/mapa/contrato.py:157–177`.) |
| `BACKLOG.md` e ADR 0002 | Registram os vínculos de programa; escritor único dessas relações: [nao verificado]. (`app/docs/BACKLOG.md:782`, `:1398`; `app/docs/ADR/0002-celula-e-o-dia.md:4`.) | Stop lê BACKLOG para fila; resolvedor automático obra→programa: [nao verificado]. (`bin/hook_stop_fila1.py:33`, `:170`.) |
| `logs/sessao/disjuntor_estado.json` e `.lock` | `_com_estado` lê, trava e grava; varredura B3 e rearme o modificam. **PreToolUse e Stop também escrevem ao varrer B3.** (`bin/disjuntor.py:219–253`, `:559–595`, `:950`; `bin/hook_stop_fila1.py:1050`.) | `ler_estado`, consultas B3 e painel. Consumo por entidade/SHA de PADRAO: [nao verificado]. (`bin/disjuntor.py:265`, `:598`, `:1094`.) |
| `logs/sessao/disjuntor.log` | `trilha`, disparos, rearme e negativas; falha do append é silenciada. (`bin/disjuntor.py:112–125`, `:590`, `:985`.) | `disjuntor.ler` calcula janela; `linha` formata; gerador do ESTADO chama ambos. (`bin/disjuntor.py:1030`, `:1147`; `bin/gerar_estado.py:278–280`.) |
| `DISJUNTOR_DESLIGADO` | Ronald, fora do Code, segundo contrato e guarda. (`bin/disjuntor.py:28–31`, `:136–157`.) | PreToolUse e Stop verificam existência antes de agir. (`bin/disjuntor.py:128`, `:980`; `bin/hook_stop_fila1.py:1040`.) |
| Arquivo escolhido para pausa e `logs/pausas.log` | `pausar.sh` grava dono/motivo/saída, faz rename e registra criação/remoção. (`bin/pausar.sh:24–47`.) | `pausar.sh --ler`; vigia inspeciona nomes específicos de pausa, sem integração automática com um nome novo. (`bin/pausar.sh:19–23`; `app/core/esteira_vigia.py:472–481`.) |
| `.claude/settings.json` | Autoria efetiva: [nao verificado]; guarda impede retirar o hook pelos caminhos reconhecidos. (`bin/disjuntor.py:148–157`.) | Configura PreToolUse com caminho absoluto do host; ativação efetiva em produção: [nao verificado]. (`.claude/settings.json:62–68`.) |
| Pergunta/resposta Codex | Escritor da pergunta: [nao verificado]; wrapper monta o prompt e dirige a resposta para `codex exec -o`. (`bin/codex_pergunta.sh:118–135`, `:157`.) | Wrapper lê pergunta/anexos; PreToolUse atual analisa o comando, sem ler a pergunta para resolver obra/entidade. (`bin/codex_pergunta.sh:131–133`; `bin/disjuntor.py:849–862`.) |

## 31/10 16:19 · sessao 4a312e2f06f8 · - · - · origem=Ronald (tmux)

**Achado 7 — recomendo (e), NAO PERGUNTADO.** O commit já estava escrito na volta 3: abertura de `v3.q`, literal, “Obra F1C4-3-EMISSORES volta 3, commit c5c43360”. Portanto, não sustenta a justificativa “commit ainda não escrito”. Nenhuma pergunta fornecida pede auditoria da mensagem ou presença da linha `LEI-AKITA`; nenhuma resposta fornecida acusa sua ausência. A frase genérica “Aponte tambem qualquer outro defeito no delta que estas perguntas nao cobrem” (`v3.q`, fechamento; numeração original [nao verificado]) não explicita esse tema.

O trecho mais próximo de resposta é `v3.r`: “Execução dos testes, execução das mutações e efeitos reais sobre os números de produção: **[nao verificado]**.” Ele não trata da mensagem. **Trecho de resposta sobre a falta da linha: inexistente nos anexos fornecidos** [nao verificado quanto à numeração]. O sítio `fabrica_da_celula.py` participa desde v1; o commit acusado entra documentalmente em v3.q. A regra exige a linha em `CLAUDE.md:62–64`; o selo distingue ausência de linha de validação de seu conteúdo em `bin/tests/test_lei_akita.sh:142–143`. A ausência efetiva no commit só está atestada por `opus_v11.txt`, item 7; mensagem integral e histórico não disponíveis [nao verificado].

**Achado 8 — recomendo (e), com ressalva material.** Nenhuma pergunta fornecida pede conferir a afirmação da mensagem contra a coluna PROTEGE. O mais próximo é `v2.q`, literal: “Leis citadas pelos sitios (grep LEIS/CORTES) e app/docs/JUIZES.md vao anexos.” `v2.r` responde: “A L-061 exige leitura da célula/ata; ela não demonstra equivalência dos predicados sobreviventes.” Isso discute a lei, sem auditar a coluna ou a mensagem. **Trecho de resposta sobre a afirmação “nenhuma lei lista os arquivos em PROTEGE”: inexistente nos anexos fornecidos** [nao verificado quanto à numeração].

**A cópia não confirma a alegação “L-061, L-033, L-050 listam em PROTEGE”.** O cabeçalho põe PROTEGE na quinta coluna (`app/docs/LEIS.md:57`); essa célula está vazia nas três linhas (`app/docs/LEIS.md:81`, `:91`, `:95`). Os arquivos mencionados nessas linhas aparecem na coluna **DONO**, anterior à coluna PROTEGE. Assim, há divergência entre a cópia e o resumo Opus, que deve ser preservada no laudo. O selo efetivamente lê a coluna PROTEGE por nome (`bin/tests/test_lei_protege_sitio.sh:58`), não a coluna DONO.

A volta de entrada da **afirmação na mensagem** do item 8 é **[nao verificado]**: `opus_v11.txt` não identifica seu commit; não há como dizer que surgiu depois de v8. O tema geral L-061 já aparece em `v2.r`, mas isso não data a afirmação acusada. Sem mensagem integral, não transformar essa lacuna em prova de classe (c) por commit posterior. `triagens.txt` vazio não permite classe (b) para nenhum dos dois [nao verificado quanto à numeração dos anexos]. [nao verificado]

## 31/10 16:19 · sessao 664f4c3bfd34 · - · - · origem=Ronald (tmux)

**Achado 2 — (c), dado de produção e mensagem posterior à última chamada.** A fonte local diz literalmente: “ata explicita 'indefinido' divergiria […] Prod 31/10: 0 de 127.784 atas” (`app/docs/BACKLOG.md:806`). Isso sustenta a distinção de universo apontada pelo opus; não prova autonomamente o novo número 158/143.452, que consta somente do dado raspado (`opus_v11.txt`, item 2; linha original [nao verificado]).

- Pergunta pertinente: `v2.q`, aval: “antes, sombra com novos CHAMADOS por emissor; ate 50 pousa” — linha original [nao verificado].
- Resposta: `v2.r`, item 1: “A sombra com novos CHAMADOS por emissor, e o atendimento ao teto de 50, são [não verificado] no material fornecido.” — linha original [nao verificado].
- Entrada: o número 0/127.784 vem da obra anterior F1C4-3, segundo a própria identificação “Codex verif4 F1C4-3” (`app/docs/BACKLOG.md:806`). Sua atribuição errada no commit b3bef403 é documentada em v11, depois da última chamada v8; volta anterior exata de introdução da frase [nao verificado].

**Achado 5 — (c), medição não disponível e mudança final não examinada.** A cópia ainda contém o predicado antigo do supra (`app/ponto/supra_juiz.py:443`) e a chamada do cartório sem o insumo novo (`app/ponto/services/cartorio.py:1207`). Portanto não permite reconstituir a sombra final nem a implementação migrada descrita no opus (`opus_v11.txt`, item 5; linha original [nao verificado]).

- Pergunta: `v2.q`, mesmo aval literal de sombra citado acima — linha original [nao verificado].
- Resposta sobre limite: `v2.r`, item 1, mesma ressalva de medição acima — linha original [nao verificado].
- Resposta que já percebeu a semântica: `v1.r`, item 1: “tipo ausente, `None` ou vazio não satisfaz `A`” — linha original [nao verificado]. Isso mostra conhecimento da semântica geral, **não descoberta do defeito da migração final**.
- Entrada: tema sombra presente em v2; semântica de tipo `None` presente em v1; migração e sombra finais documentadas em v11. Volta exata entre esses marcos [nao verificado].

**Achado 6 — (c), suíte/log final.** A regra local exclui expressamente “RED, suite, dado de prod” da falha do Codex (`CLAUDE.md:557`). [nao verificado]

- Pergunta relacionada: `v1.q`, item 5: “selo/RED sem mordida” — linha original [nao verificado].
- Resposta: `v1.r`, abertura: “Execução de testes/mutantes […] [não verificado]” — linha original [nao verificado]. [nao verificado]
- Última resposta: `v8.r`, item 3: “execução da suíte Django com as mutações: [não verificado]” — linha original [nao verificado]. [nao verificado]
- Entrada: o arquivo `v11_green.log` e seus zero bytes aparecem no achado final v11; ocorrência nas voltas anteriores [nao verificado]. A idade do comportamento “suite nao imprime hash” também fica [nao verificado]. [nao verificado]

Não há linha fornecida em `triagens.txt`; portanto nenhum destes três pode receber classe (b) com descarte comprovado — conteúdo da triagem [nao verificado].

_1147 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
