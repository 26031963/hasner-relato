# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 15:31** · publicado em **13/11/2026 15:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 15:31 · sessao b1ab3636f9b5 · - · - · origem=Ronald (tmux)

**Complemento: 19 pares novos**, separados dos 93 anteriores. Nenhuma quebra de chave obrigatória foi confirmada neste complemento; há formatos alternativos e omissões condicionais atendidas por defaults ou discriminadores. Selos: **9 localizados; 10 não localizados**, inexistência desses dez **[não verificado]**. Atribuição por estrutura: **17 escala; 2 ponto** — os dois últimos são as trilhas `veredito_celula`. Dono provável de eventual incompatibilidade estrutural: ESTRUTURA **[não verificado]**.

As linhas pendentes dos gravadores anteriores ficam verificadas:

- M02: `app/escala/management/commands/lavrar_previsto_cego.py:37`, `detalhe=d`.
- M03: `app/escala/management/commands/lavrar_vigencia_impossivel.py:39`, `detalhe=d`.
- M04: `app/escala/management/commands/lavrar_vigencia_sem_trilha.py:33`, `detalhe=d`.

**Schemas compartilhados**

`F12`: o command produz o dicionário em `app/ponto/management/commands/censo_fase_12x36.py:290`; a porta persiste integralmente em `app/escala/services/fase_12x36.py:91`. Chaves: `linhas_de_desvio,linhas_medidas,colabs_de_desvio,automaticos,propostas,linhas,itens,janelas,agora,itens_reabertos,criterio,esperado`. `linhas` contém `colab,empresa,competencia,ec,template,plantoes,casados,paridade,furos,folga_trabalhada` em `:295`; os itens recebem `data,ec_do_dia,template_do_dia,tipo,aplicavel` em `:235` e `colab,empresa,competencia,dono_do_dia` em `:299`; `janelas={empresa,ini,fim,reaberta}` em `:306`. O leitor `ultimo_lavrado` copia integralmente e acrescenta `data_ref,medido_em` em `fase_12x36.py:211`; sem formato atual, retorna apenas `colabs_de_desvio,linhas,itens,sem_medida` em `:210`.

`C7`: `app/escala/services/regua_na_lista.py:48` persiste `total,colabs,universo,esperado,por_colab`. Cada item é produzido em `:31` com `ec,colab,origem,classe,tipo_escala_id,tipo_escala_codigo,declarado,real,evidencia,data_exemplo,destino`. `ultimo_lavrado` acrescenta `medido_em` em `:65`; fallback com `total=None,por_colab={},esperado=0,sem_medida=True` em `:63`.

| ID | Estrutura / emissor | Consumidor / chaves lidas | Desencontro / selo |
|---|---|---|---|
| X01 | F12 — `censo_fase_12x36.py:290` | `app/escala/services/fase_12x36.py:93`, `:106`, `:110`, `:144`, `:179`, `:207`: `colabs_de_desvio`; `janelas,agora`; janela `empresa,ini,fim`; `itens,itens_reabertos`, item `data,colab,empresa`; cópia integral | Nenhuma quebra confirmada. **Sim**: `app/escala/tests/test_o261_v_fase_na_lista.py:94`; leitura de cobertura/as-of também em `app/escala/tests/test_vfase_dono_as_of.py:83` |
| X02 | F12 via `fase_12x36.py:211` | `app/escala/services/cadastro_realidade.py:421`: `itens` default `()`, item `colab,data,ec_do_dia,template_do_dia,dono_do_dia,aplicavel,competencia,tipo` | Nenhuma quebra confirmada. **Sim**: `app/escala/tests/test_o261_v_fase_na_lista.py:180` |
| X03 | mesmo | `app/api/views_mensageria.py:1685`: `colabs_de_desvio,linhas_medidas,criterio,data_ref,sem_medida,linhas`, todos `.get` | Variantes normal/sem medida: `sem_medida` só no fallback; demais chaves ausentes nele usam defaults. **Sim**: `app/escala/tests/test_o261_v_fase_na_lista.py:263` |
| X04 | mesmo | `app/chamados/services/fila_por_causa.py:184`: `colabs_de_desvio` default `()` | Nenhuma quebra confirmada. Selo não localizado **[não verificado]** |
| X05 | mesmo | `app/escala/services/propositor.py:493`: `itens` default `()`, item `colab,ec_do_dia` via `.get` | Nenhuma quebra confirmada. Selo específico desta dependência não localizado **[não verificado]**; `test_o261_v_fase_na_lista.py:280` testa o universo do propositor, mas não demonstrei que morde retirar a leitura da lavra |
| X06 | mesmo | `app/ponto/management/commands/vigiar_familia_escala.py:24`: `data_ref,linhas_medidas,colabs_de_desvio,criterio,linhas`; linha `colab,competencia,ec,paridade,plantoes,furos,folga_trabalhada` | Variantes sem medida atendidas por `.get`. Selo não localizado **[não verificado]** |
| X07 | C7 — `app/escala/services/regua_na_lista.py:48` | `app/escala/services/cadastro_realidade.py:347`: `por_colab` default `{}`; item `destino,ec,tipo_escala_codigo,classe,declarado,real`; destino `codigo` | Nenhuma quebra confirmada. **Sim**: `app/escala/tests/test_termometro_vira_lista.py:51`, `:61` |
| X08 | mesmo | `app/escala/views.py:469`: `por_colab` default `{}`, índice `str(colaborador_id)` default `[]`; item `ec,destino`; destino repassado à porta | Nenhuma quebra confirmada. Selo da realocação por esse payload não localizado **[não verificado]** |
| X09 | retorno C7 — `app/escala/services/regua_na_lista.py:54` | `app/escala/management/commands/termometro_regua.py:94`: `total,colabs,universo` | Nenhuma quebra confirmada. **Sim**, fluxo real executado: `app/escala/tests/test_termometro_vira_lista.py:136` |
| X10 | sugestão — `app/escala/services/fase_12x36.py:50`, `:61`, `:64`: `ancora,motivo,batidas`; `casa_hoje` em duas variantes, `casaria` só na sugestão | `app/escala/services/propositor.py:333`: `ancora` via `.get`, depois indexação sob a guarda | Consumidor lê a única chave presente em todas as variantes. Selo do produtor existe em `app/escala/tests/test_ancora_sugerida.py:43`; selo que prende o par inteiro não localizado **[não verificado]** |
| X11 | impacto — `app/escala/services/impacto_troca.py:47`: `de,ate,furos_antes,furos_depois,dias_furo_antes,dias_furo_depois,frase`, ou `None` em `:32` | `app/colaboradores/services/vinculo.py:142`, transporta integralmente em `impacto` no retorno em `:147` | Nenhuma quebra confirmada. **Sim**: `app/colaboradores/tests/test_troca_escala_porta.py:50`, `:58` |
| X12 | HPD JS — `app/templates/escala/wizard_tipo_escala.html:454`: weekday → `hi,hf`; `hii,hfi` condicionais em `:455`, serialização em `:459`, parâmetro em `:481` | `app/escala/views_wizard.py:53`: lê `horarios_por_dia`, `json.loads`, aceita mapa inteiro em `:57`, inválido → `{}` em `:59` | Omissão de pausa prevista. `app/escala/tests/test_wizard_intervalo.py:84` injeta JSON manual: **não prende emissão do JS**. Selo completo não localizado **[não verificado]** |
| X13 | trilha da lavra — `app/ponto/portas/celula.py:1087`, `:1102`: `veredito,via,protestos,status_dia` | `app/chamados/juizes.py:1295`: `valor_depois`; `.get('via') == VIA_HUMANA` ou presença de `acoes` em `:1298`; `VIA_HUMANA='admin'` em `app/ponto/services/veredito_celula.py:76` | Variante da assinatura atendida pelo OR. **Sim**, escritor real→leitor: `app/chamados/tests/test_tranca_vivas_08.py:56`, `:82`, `:95`; mutante em `:300` |
| X14 | trilha do despachante — `app/ponto/services/veredito_celula.py:356`: `veredito,acoes,erros` | mesmo consumidor `app/chamados/juizes.py:1298`: lê `via` com default `None`, ou presença de `acoes` | `via` ausente é variante deliberada: `acoes` discrimina. O ramo tem teste em `app/chamados/tests/test_tranca_vivas_08.py:101`, porém o teste grava payload manual por `registrar_log`, sem chamar o despachante inteiro. Selo completo escritor→leitor **[não verificado]** |
| X15 | envelope EventoCelula — `app/ponto/portas/celula.py:954`: `celula_id,colaborador_id,data`; defaults do modelo em `app/escala/models.py:1860`: `criado_em,processado_em=None,tentativas=0,erro=''` | `app/chamados/reconciliador.py:1250`, `:1262`, `:1344`, `:1351`, `:1394`: `processado_em,tentativas,criado_em,pk,colaborador_id`; `celula_id` também em `:1274`, `:1296`, `:1309`; lote repassado em `:1388` | Envelope sem blob é declarado em `app/escala/models.py:1848`. Nenhuma quebra confirmada. **Sim**: `app/ponto/tests/test_o_outbox_celula.py:56`, `:163` |
| X16 | mesmo envelope | `app/chamados/services/fabrica_da_celula.py:297`: `data`; relê célula atual por `(colab.pk,data)` em `:299` | `celula_id` antigo não decide a célula atual, explicitado em `:291`. Teste do leitor: `app/chamados/tests/test_e4_c20.py:42`; usa eventos fake. Selo completo produtor→leitor **[não verificado]** |
| X17 | mesmo envelope | `app/ponto/services/lastro.py:422`: `colaborador_id` dos eventos pendentes; filtro de estado definido em `reconciliador.py:1250` | Nenhuma quebra confirmada. Selo específico não localizado **[não verificado]** |
| X18 | mesmo envelope | `app/chamados/management/commands/reconciliar_chamados.py:62`: `colaborador_id` dos pendentes | Nenhuma quebra confirmada. **Sim**, evento real e mutação do estado: `app/chamados/tests/test_c20_juiz3_outbox.py:349` |
| X19 | mesmo envelope | `app/chamados/services/foto_e4b.py:270`: filtro `colaborador_id`, `data` para alcance e registro inteiro pelo `guardar` | Nenhuma quebra confirmada. Selo específico não localizado **[não verificado]** |

O envelope também tem **escritor de estado**: `app/chamados/reconciliador.py:1396` grava `tentativas,erro`; `:1402` grava `processado_em`; `:1409` aplica retenção. Esses campos participam dos filtros dos leitores X15/X17/X18; portanto não são chaves lidas sem escritor. Os demais gatilhos chamam o escritor único `emitir_evento_celula`, cujos chamadores estão declarados em `app/ponto/portas/celula.py:946` e `app/escala/models.py:1851`; não multipliquei a costura por cada gatilho que apenas chama essa porta.

**Correção de classificação para o subtotal anterior:** O01 é desencontro formal com default explícito de sucesso, não quebra obrigatória demonstrada; R01/R02 são excesso sem leitor localizado, cuja ausência global continua **[não verificado]**. Assim, para os **93 anteriores + estes 19 = 112 pares localizados**, não certifique “três quebras”: são **zero quebras obrigatórias confirmadas por mim**, um casamento de schemas com defaults e dois excessos sem consumo localizado. A soma permanece um recorte, sem certidão de completude.

## 13/11 15:27 · sessao b1ab3636f9b5 · - · - · origem=Ronald (tmux)

Consolidei **92 pares por arquivo emissor–arquivo leitor**, agrupando operações do mesmo arquivo sobre a mesma estrutura. É **subtotal verificado, não censo total**: ainda faltam fechar retornos auxiliares, aliases, templates e chamadas dinâmicas. As contagens de desencontro e de inexistência de selo para o universo inteiro permanecem **[não verificado]**.

**Convenções para consolidar**

- `D`: schema produzido em `app/ponto/management/commands/gerar_celulas.py:83` e `:104`; persistido pelas operações de `app/ponto/portas/celula.py:261`, `:278`, `:294`, `:306`, `:639`, `:642`, `:827`, `:917`, `:925`. Chaves: `dna_versao,vinculo_id,tipo_escala_id,tipo_ciclo,marcos,minutos_jornada,intervalo_livre,lampadas`; `marcos={hi,hf,hii,hfi,[hii2,hfi2]}`; `lampadas=[{chave,tipo,hora,instante}]`; `extra_declarada={antes,depois}` nasce condicionalmente em `gerar_celulas.py:130`.
- `A`: ATA produzida em `app/ponto/services/cartorio.py:738`, com `tipo_dia,lampadas,minutos_previstos,minutos_realizados,sem_ancora,n_orfas,orfas,n_missing,n_celulas`; condicionais `cobertura_parcial,periodo_parcial` em `:766`; lâmpada `{tipo,hora,acesa,luz,tipo_real}` em `:711`; `isencao` pelo `_carimbar_isencao` chamado em `:714`; `dispensada` em `:810`; `pendente` em `:915`. Persistência em `app/ponto/portas/celula.py:1064` e `:1091`; `precedencia` em `:1213`, preservada pelas lavras em `:1053`.
- **Omissão condicional prevista** precisa ser distinguida de incompatibilidade: `extra_declarada`, segunda pausa, `isencao`, `dispensada` e `pendente` não existem em todas as variantes, mas têm leitores `.get`; isso satisfaz a definição literal de ausência de chave, sem provar falha.
- Nas linhas abaixo, selo não indicado significa **não localizado nesta inspeção; inexistência [não verificado]**. Dono provável de incompatibilidade de forma: **ESTRUTURA [não verificado]**; não atribuí dado real a CADASTRO/BATIDA sem evidência.

### Persistidos: DNA — 21 pares

| ID | Estrutura/emissor | Consumidor | Chaves lidas |
|---|---|---|---|
| D01 | D | `app/escala/models.py:1124`, `:1189`, `:1235` | `marcos.hi,hf,hii,hfi`, todos os pares de pausa; `extra_declarada.antes,depois` com zero |
| D02 | D | `app/escala/utils.py:53`, `:104`, `:137`, `:153` | `marcos` e pares; `lampadas` default `[]`, `hora,instante` |
| D03 | D | `app/chamados/juizes.py:948`, `:1348` | `marcos`, chave do motivo, pares de pausa |
| D04 | D | `app/chamados/services/adesao.py:36` | `marcos`, repassado a `CelulaDia.marcos_presentes` |
| D05 | D | `app/ponto/juiz_batida.py:159` | `marcos.hi,hf` e todos os pares de pausa |
| D06 | D | `app/ponto/services/cartorio.py:484`, `:615` | `dna_versao,marcos`, chaves de turno |
| D07 | D | `app/ponto/services/dia_do_colab.py:46` | `marcos`, repassado a `marcos_presentes` |
| D08 | D | `app/ponto/services/diagnostico_escala.py:153` | `marcos`, default `{}` |
| D09 | D | `app/ponto/services/esmeril_espelho.py:116` | `marcos`, repassado a `marcos_presentes` |
| D10 | D | `app/ponto/services/faltas_de_hoje.py:43` | `marcos.hi`, fallback de template em `:44` |
| D11 | D | `app/ponto/services/veredito_celula.py:31` | `marcos.hi,hf,hii,hfi` e todos os pares |
| D12 | D | `app/ponto/services/vigia_cadastro_futuro.py:60`, `:79` | `vinculo_id`; comparação do DNA inteiro |
| D13 | D | `app/ponto/services/vigia_entrada.py:126` | `marcos.hi,hf`, `tipo_ciclo` |
| D14 | D | `app/ponto/management/commands/tripwire_celulas.py:30`, `:99`, `:259` | inteiro em comparação; `marcos` |
| D15 | D | `app/ponto/management/commands/aplicar_col900.py:76`, `:174` | inteiro em snapshot; `marcos.hi` |
| D16 | D | `app/ponto/management/commands/censo_col900.py:119` | `marcos`, comparação integral do subdict; `hi` |
| D17 | D | `app/ponto/management/commands/censo_fase_12x36.py:392` | inteiro em snapshot |
| D18 | D | `app/ponto/management/commands/detectar_intervalo_ausente.py:53` | `marcos.hi,hf` e todos os pares |
| D19 | D | `app/ponto/management/commands/backfill_b5_insumos.py:73` | inteiro para `insumos_da_ata` |
| D20 | D | `app/ponto/management/commands/vigia_b5_ausencia.py:79` | inteiro para `insumos_da_ata` |
| D21 | D | `app/pautas/services.py:424` | `marcos` |

O produto tem teste de chaves em `app/escala/tests/test_t2_lampadas.py:28` e de seis marcos em `:34`; isso **não demonstra selo de todos esses leitores**. A comparação do blob inteiro em D12/D14 impede afirmar que suas chaves são globalmente “ninguém lê” apenas por ausência de `.get` nominal.

### Persistidos: ATA — 30 pares

| ID | Estrutura/emissor | Consumidor | Chaves lidas |
|---|---|---|---|
| A01 | A | `app/escala/services/leitor_celula.py:68`, `:91`, `:115`, `:187`, `:379` | schema base; lâmpadas `acesa` default `luz is not None`, `luz,hora,tipo,tipo_real,isencao,dispensada`; `cobertura_parcial,periodo_parcial` |
| A02 | A | `app/chamados/juizes.py:450`, `:493`, `:966`, `:1354` | `tipo_dia,cobertura_parcial,lampadas,orfas,n_orfas`; lâmpadas `tipo,hora,acesa,luz` |
| A03 | A | `app/chamados/reconciliador.py:701`, `:879` | presença de `lampadas`; inteiro para `parcial_so_lista` |
| A04 | A | `app/chamados/services/disputa_emissao.py:940` | inteiro para `parcial_so_lista` |
| A05 | A | `app/chamados/services/fabrica_da_celula.py:123` | inteiro para `parcial_so_lista` |
| A06 | A | `app/chamados/services/materializacao.py:172` | `lampadas,orfas`; lâmpadas `tipo,hora,luz,acesa` |
| A07 | A | `app/ponto/juiz_batida.py:157` | `lampadas,orfas`; uso das lâmpadas começa em `:190` |
| A08 | A | `app/ponto/services/cartorio.py:451`, `:805`, `:834`, `:913` | `lampadas`, `pendente`; dispensa, insumos e teto |
| A09 | A | `app/ponto/services/cluster_espurio.py:27` | `orfas,lampadas`; `acesa` |
| A10 | A | `app/ponto/services/dia_do_colab.py:30` | `lampadas`; `hora,tipo,acesa,luz` |
| A11 | A | `app/ponto/services/diagnostico_escala.py:154` | `lampadas`; `hora,tipo,acesa,luz` |
| A12 | A | `app/ponto/services/esmeril_espelho.py:125` | `orfas,lampadas`; `luz,hora,acesa` |
| A13 | A | `app/ponto/services/lastro.py:89`, `:113`, `:128`, `:148` | `lampadas`; `hora,tipo,acesa,pendente` |
| A14 | A | `app/ponto/services/vigia_entrada.py:80`, `:123` | `lampadas`; `tipo,acesa` |
| A15 | A | `app/ponto/services/vigia_pareador_celula.py:73` | `orfas,n_orfas` default zero, `lampadas` |
| A16 | A | `app/ponto/worklist.py:22` | presença de `lampadas` |
| A17 | A | `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:61` | `lampadas.acesa` |
| A18 | A | `app/ponto/management/commands/backfill_b5_insumos.py:73` | inteiro para `insumos_da_ata` |
| A19 | A | `app/ponto/management/commands/celula_veredito_velho.py:108` | inteiro em snapshot |
| A20 | A | `app/ponto/management/commands/curar_residuo_bug115.py:45`, `:148` | presença de `lampadas` |
| A21 | A | `app/ponto/management/commands/detectar_intervalo_ausente.py:64` | `lampadas.tipo,hora,acesa,luz`; `acesa` default `luz is not None` em `:68` |
| A22 | A | `app/ponto/management/commands/tripwire_celulas.py:264`, `:342` | `lampadas,orfas,tipo_dia` |
| A23 | A | `app/ponto/management/commands/vigia_b5_ausencia.py:79` | inteiro para `insumos_da_ata` |
| A24 | A | `app/pautas/services.py:432` | `lampadas.hora,acesa` |
| A25 | A | `app/relatorios/dia_da_frota.py:21`, `:61` | `lampadas.tipo,hora,acesa` |
| A26 | A | `app/relatorios/cartao_pela_celula.py:287` | `orfas,lampadas.hora,isencao` |
| A27 | A | `app/folha/previa.py:83` | `lampadas.acesa,hora,tipo` |
| A28 | A | `app/folha/services/sem_furo.py:22` | `lampadas.acesa` |
| A29 | A | `app/folha/validacao_pdf.py:47` | `minutos_realizados`, default zero |
| A30 | A | `app/colaboradores/services/feriado_do_posto.py:97` | `lampadas`, processamento posterior no mesmo corpo |

**Outro emissor, outro alcance:** `app/escala/utils.py:1887` fabrica **ATA em memória** apenas com `lampadas,orfas`, passando-a em `celulas=` a `turnos_do_colab` em `:1898`. Seu leitor efetivo é `app/ponto/juiz_batida.py:157`, que lê essas duas chaves. Esse é **E01**, um par adicional; não deve ser cruzado artificialmente com todos os leitores da ATA persistida.

### Outros JSONFields — 16 pares

| ID | Estrutura / emissor e chaves | Consumidor e leitura |
|---|---|---|
| P01 | `protestos`: `app/ponto/portas/celula.py:1069`, `:1092`, lista de códigos | `app/escala/services/cadastro_realidade.py:478`, `:612`, membership de códigos |
| P02 | mesmo | `app/chamados/reconciliador.py:869`, lista |
| P03 | mesmo | `app/folha/export.py:1173`, `:1254`, conjunto de códigos |
| P04 | mesmo | `app/ponto/services/furos_sem_cobranca.py:57`, membership |
| P05 | mesmo | `app/ponto/services/cartorio.py:934`, `:1084`, lista e presença |
| P06 | mesmo | `app/ponto/management/commands/celula_veredito_velho.py:108`, inteiro |
| P07 | mesmo | `app/ponto/management/commands/supra_juiz.py:166`, inteiro |
| H01 | `horarios_por_dia`: `app/escala/services/cadastro_tipo.py:267`, mapa weekday → dict externo | `app/escala/models.py:231`, `:239`, `:338`, `:686`, `:864`: weekday string/int; `hi,hf,hii,hfi` |
| H02 | mesmo | `app/escala/utils.py:274`: `hi,hf,hii,hfi`; **selo sim**, `app/escala/tests/test_marcos_def_hpd.py:35` |
| H03 | mesmo | `app/escala/views_wizard.py:80`, serializa inteiro |
| H04 | mesmo | `app/ponto/selecao_periodo.py:223`: weekday string, `hi,hf` |
| H05 | mesmo | `app/colaboradores/services/drawer.py:99`, presença do mapa |
| F01 | `FolgaCalendario.celulas`: `app/escala/folgas.py:586`, `:591`, `:656`, lista `[semana,weekday]` | `app/escala/views.py:304`, `:371`, lista e pares |
| F02 | `FolgaCalendarioTamanho.dias`: `app/escala/folgas.py:595`, `:661`, lista de dias | `app/escala/views.py:293`, `:362`, dias |
| F03 | mesmo | `app/escala/models.py:1560`, dias → mapa tamanho do mês |
| S01 | `dna_anterior`: `app/ponto/portas/celula.py:262`, `:307`, cópia do DNA anterior; limpa em `:828` | `app/ponto/management/commands/aplicar_col900.py:175`, `marcos.hi` |

O `horarios_por_dia` tem emissor de conteúdo no JS: `app/templates/escala/wizard_tipo_escala.html:454` grava `hi,hf`, acrescenta `hii,hfi` condicionalmente em `:455`, e serializa em `:459`; a fronteira Python o decodifica em `app/escala/views_wizard.py:53`. Não contei essa costura JS→Python no subtotal de 92.

`DecisaoProposta.resumo` é escrito em `app/escala/services/propositor.py:720` e `:776`; `_resumo` grava `tipo,template,template_id,ancora,ancora_atual,template_atual,casa_hoje,casaria,sobra,aplicar_desde,dias_batidos` em `:724`. A documentação do próprio campo diz “foto … para a forense” em `app/escala/models.py:1927`. **Leitor de produção não localizado; inexistência global [não verificado]**. Como não há par localizado, não entrou nas 92 costuras.

### Portas e retornos — 25 pares

| ID | Estrutura / emissor | Consumidor / chaves |
|---|---|---|
| W01 | payload wizard, `app/escala/views_wizard.py:20`: `template_nome,apelido,hora_inicio,hora_fim,hora_inicio_intervalo,hora_fim_intervalo,hora_inicio_intervalo2,hora_fim_intervalo2,intervalo_duracao_min,noturno,permite_hora_extra,he_extra_antes_min,he_extra_depois_min,folga_calendario_padrao_id,folgas,dias_trabalho_semana`; criação acrescenta `tipo_ciclo,tipo_base,tipo_base_ref`; `horarios_por_dia` condicional em `:57` | `app/escala/services/cadastro_tipo.py:148`: mesmas chaves até `:267`, inclusive defaults; `descricao` aparece apenas em teste de presença em `:185` |
| W02 | patch `app/chamados/services/limbo_folgas.py:92`: `template_nome,folgas` | mesma porta `cadastro_tipo.py:148`; `hora_inicio,hora_fim` lidos com `.get` em `:205`; demais alterações condicionadas à presença |
| C01 | carimbo `app/escala/services/regeneracao.py:104`: `{data:{regenerada_em,editada_em}}` | `app/ponto/management/commands/gerar_celulas.py:257`: `**reescrita[dia]` para `cobranca_orfa`; assinatura receptora em `regeneracao.py:141` |
| C02 | mesmo | `app/ponto/management/commands/supra_juiz.py:170`, uso em chamada de `cobranca_orfa` no laço iniciado em `:260` |
| T01 | detector `app/escala/detector_proposta.py:123`, `:128`, `:153`: `padrao,proposta,confianca,porque`; proposta `{tipo_escala_id,codigo,descricao,ciclo,horario}`; padrão em `:63` e `:82` | `app/escala/proposta_pdf.py:75`, `:121`: outer completo, padrão `dias,marcos_moda,entrada,saida,duracao_h,dispersao_entrada_min,padrao_dias,dows`; proposta `codigo,horario,ciclo`; **selo sim**, `app/escala/tests/test_detector_proposta.py:141` |
| T02 | mesmo | `app/escala/management/commands/propor_escala_limbo.py:38`: outer completo, padrão completo exceto `marcos_freq`, proposta `codigo,horario,ciclo` |
| T03 | mesmo | `app/escala/services/escala_auto.py:121`: `proposta,confianca,padrao.dias,porque`; proposta `tipo_escala_id,codigo,descricao,horario` |
| T04 | mesmo | `app/ponto/services/esmeril_espelho.py:234`: `proposta,porque,confianca`; proposta `codigo,descricao,horario` |
| Q01 | propostas `app/escala/services/propositor.py:465`, `:539`, `:585` | `app/chamados/services/proposta_no_fio.py:67`: `n,propostas`; proposta `chamados_que_morreriam,vinculo_id,tipo,ancora,ancora_atual,template,template_atual,motivo` |
| Q02 | mesmo | `app/chamados/services/painel.py:522`: `propostas[].colaborador_id` |
| Q03 | mesmo | `app/colaboradores/services/ficha.py:176`: `propostas`; `tipo,template_atual,template,ancora,casa_hoje,casaria,dias_batidos,aplicar_desde,chamados_que_morreriam` |
| Q04 | mesmo | `app/escala/management/commands/propostas_de_escala.py:39`, `:74`: `propostas,universo,n`; por proposta `colaborador_id,vinculo_id,colaborador,tipo,template_atual,ancora_atual,casa_hoje,dias_batidos,motivo,template,ancora,casaria,sobra,aplicar_desde,chamados_que_morreriam` |
| Q05 | mesmo | `app/escala/management/commands/ritmo_discordante.py:20`: `ritmo_discordante_vinculos` default `[]`, itens `colaborador_id,ciclo,ritmo,proposta`; `ritmo_discordante` default zero |
| Q06 | mesmo | `app/api/views_mensageria.py:2034`: `propostas` default `[]`; `colaborador,posto,ancora,template_atual,template,casa_hoje,casaria,chamados_que_morreriam,aplicar_desde,tipo` |
| Q07 | mesmo | `app/core/views_ia.py:575`: `propostas` default `[]`, `colaborador_id`; assinatura e seleção delegadas |
| O01 | resultado aplicar `app/escala/services/propositor.py:788`: `decisao_id,saida` | `app/chamados/services/proposta_no_fio.py:152`: **lê `ok` com default `True`, `msg` com default `None`; emissor não grava** |
| O02 | mesmo | `app/escala/management/commands/propostas_de_escala.py:69`: `saida,decisao_id`, coincide |
| K01 | kwargs de correção `app/escala/services/propositor.py:765`: `colab,vinculo,template,desde,ate,ancora,novo_trecho,regenerar_desde,apesar_da_lavra,apply,schema` | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:63`: `colab,vinculo,template,desde,apesar_da_lavra,ancora,apply,ate,novo_trecho,regenerar_desde`; `schema` só pertence ao wrapper `handle` em `:50` |
| M01 | snapshot fase conflitante: schema `app/escala/services/fase_conflitante.py:92`; persistência `app/escala/management/commands/lavrar_fase_conflitante.py:35` | `app/escala/services/fase_conflitante.py:112`: cópia integral de `detalhe`, acrescenta `medido_em`; fallback `{total:None,esperado:0,sem_medida:True}` em `:111` |
| M02 | snapshot previsto cego: schema `app/escala/services/previsto_cego.py:193`; gravador command homônimo **linha da persistência [não verificado]** | `app/escala/services/previsto_cego.py:224`: integral + `medido_em`; fallback em `:223` |
| M03 | snapshot vigência impossível: schema `app/escala/services/vigencia_impossivel.py:48`; gravador command homônimo **linha da persistência [não verificado]** | `app/escala/services/vigencia_impossivel.py:72`: integral + `medido_em`; fallback em `:71`; **selo sim**, `app/escala/tests/test_contador_vigencia_impossivel.py:108` |
| M04 | snapshot vigência sem trilha: schema `app/escala/services/vigencia_sem_trilha.py:79`; gravador command homônimo **linha da persistência [não verificado]** | `app/escala/services/vigencia_sem_trilha.py:98`: integral + `medido_em`; fallback em `:97` |
| R01 | rótulo régua `app/escala/regua_defesa.py:632`: `classe,faixa,escore,texto` | `app/chamados/juizes.py:516`: só testa resultado `is None` |
| R02 | mesmo | `app/escala/services/furos_diarios.py:36`, `:305`: presença e `classe`, com default `''` em `:306`; não lê `faixa,escore,texto` |

Os campos de cada proposta Q vêm dos emissores internos de `app/escala/services/propositor.py:184`, `:191`, `:299`, `:303`, `:319`, `:339`, `:363`, `:371`, acrescidos pelo `p.update` em `:539` e `chamados_que_morreriam` em `:572`/`:575`. `motivo` e `sobra` têm variantes; o CLI só lê `sobra` no ramo que não é `grade_por_dia`, em `app/escala/management/commands/propostas_de_escala.py:87`.

**Três pares com desencontro de forma confirmado, além das omissões condicionais:**

1. **O01**: emissor retorna `decisao_id,saida`; consumidor consulta `ok,msg`. O default de sucesso está explícito; falha produtiva **[não verificado]**. O teste troca o retorno real por mock `{'ok': True}` em `app/chamados/tests/test_proposta_no_fio.py:112`, portanto não prende esse casamento de schemas. Dono provável ESTRUTURA; classe sim **[não verificado]**.
2. **R01 e R02**: o produtor entrega `faixa,escore,texto`, mas os únicos chamadores de produção localizados só usam presença/`classe`, conforme as linhas acima. Ausência de outros leitores por alias **[não verificado]**. O teste `app/escala/tests/test_termometro_regua.py:177` testa o produto diretamente, não obriga consumidores a lerem essas chaves. Dono provável ESTRUTURA; classe sim **[não verificado]**.

**Contagem do meu subconjunto:** 21 DNA + 30 ATA + 16 outros JSONFields + 25 portas/retornos + E01 = **93**, não 92. A diferença veio da inclusão explícita da ATA transitória E01 na consolidação. **93 costuras localizadas; 3 desencontros de forma confirmados; número adicional por omissões condicionais [não verificado]; 3 com selo de costura localizado, 90 sem selo localizado, inexistência dos 90 [não verificado].** Atribuição destas estruturas a `escala` segue o modelo proprietário, inclusive quando o escritor fica em `ponto`; se o relatório atribuir pelo arquivo emissor, precisa redistribuir.

**Residual já localizado, ainda fora dessas 93:** lavra de fase em `app/escala/services/fase_12x36.py:85`, produzida pelo command `app/ponto/management/commands/censo_fase_12x36.py:279`; consumidores `app/escala/services/cadastro_realidade.py:421`, `app/api/views_mensageria.py:1685`, além dos leitores internos da lavra em `fase_12x36.py:97`, `:117`, `:197`. Lavra C7 em `app/escala/services/regua_na_lista.py:48`, consumidores `app/escala/services/cadastro_realidade.py:347` e `app/escala/views.py:469`. Há ainda `ancora_sugerida → propositor` em `app/escala/services/fase_12x36.py:50`/`:64` e `app/escala/services/propositor.py:333`, e `impacto_da_troca → julgar_vinculo` em `app/escala/services/impacto_troca.py:47` e `app/colaboradores/services/vinculo.py:142`. Essas pendências impedem chamar o subtotal de censo total.

## 13/11 15:27 · sessao e6c5df440ac0 · - · - · origem=Ronald (tmux)

Ampliação dos dicionários transitórios: **13 pares adicionais enumerados, sem desencontro de chave obrigatório demonstrado neste recorte**. O produtor de `alimentar_portas` e o produtor da tarefa são estruturas diferentes; não há chamada de `tarefa_do_dia` no corpo atual de `censo_alcance`. (`app/chamados/juizes.py:1585`; `app/chamados/services/tarefa_do_dia.py:182`; `app/chamados/services/vigia_fio.py:604`, `:635`)

Todos os donos abaixo são **ESTRUTURA prováveis**, porque a unidade observada é o esquema entre funções/módulos, não a correção do cadastro ou da batida; classe **sim**, como contrato reutilizado. Atribuição provável, não resultado do oráculo L-099. (`CLAUDE.md:179`)

Os testes citados foram lidos, não executados; estado RED/GREEN nesta cópia [não verificado]. [nao verificado]

| # | Estrutura | Emissor; chaves escritas | Consumidor; chaves lidas/defaults | Desencontro | Selo que prende |
|---|---|---|---|---|---|
| T01 | partição dos verbos | `app/chamados/services/verbos.py:62`: `mapa`, `contagens`; mapa contém `chamado_id→verbo/None`; contagens contém `verbo→{n,atrasados,urgentes}` (`:37-39`) | `app/chamados/juizes.py:1540`: `['mapa']`, sem default | Não demonstrado; `contagens` não é consumida por este leitor, mas há outros leitores no próprio serviço (`verbos.py:201`) e fora deste recorte [não verificado] | Caminho real exercitado por `app/chamados/tests/test_e3_portas_tarefa.py:49-55` e `:113-127`; não é uma catraca de todo consumidor futuro |
| T02 | alimentação de fila | `app/chamados/juizes.py:1549`: `verbo`, `fila_ids`, `fila_cel` | `app/chamados/services/tarefa_do_dia.py:529-532`, `:546-549`: `fila_ids`, `verbo[pk]`, `fila_cel.get(celula_id,())`; nenhum default de chave superior | Não demonstrado | **Sim:** `app/chamados/tests/test_e3_portas_tarefa.py:113-127`, `test_RED_lote_le_painel_uma_vez_sem_rejulgar_C6`, usa `wraps=alimentar_fila`, cobra uma alimentação, relógio, zero query na releitura e verbo da tarefa; casos de forma em `:32-82` |
| T03 | alimentação completa C17 | `app/chamados/juizes.py:1585-1587`: anteriores + `vivas`, `colab`, `no_app={}`, `instancias`; `_no_app` acrescenta `no_app[colab_id]`/instância em `:1593-1601` | `app/chamados/services/vigia_fio.py:605`, `:610`, `:615`: `fila_ids`, `vivas`, `instancias`; passa a estrutura ao `_no_app` em `:621` e ao C17 em `:635` | Não demonstrado. Esse leitor usa os **insumos brutos**, além da resposta da porta | Integração do alcance lê produtor real em `app/chamados/tests/test_o261e_f5_alcance.py:310-315` e `:385-400`; selo independente de todas as chaves brutas [não verificado] |
| T04 | alimentação completa C17 | `app/chamados/juizes.py:1585-1587`: `verbo,fila_ids,fila_cel,vivas,colab,no_app,instancias` | `app/escala/services/cadastro_realidade.py:484-488`: `vivas.get(pk) or ()`; restante encaminhado como `alimentacao` ao C17 | Não demonstrado | **Sim, integração de comportamento:** `app/escala/tests/test_f3_beco1_folga_na_lista.py:163-175`, `:177-196`, dois contraexemplos com C17 e lista reais; não checa lista textual de todas as chaves |
| T05 | alimentação completa C17 | `app/chamados/juizes.py:1585-1587`: mesmas chaves | `app/chamados/management/commands/diff_e3_sombra.py:114`, `:62-64`: `fila_cel.get(cel_id,())`, `vivas.get(cel_id,())`, `fila_ids`; encaminha ao `_porta` em `:124` | Não demonstrado | **Sim:** `app/chamados/tests/test_e3_diff_sombra.py:150-163`, `test_AB_categorizado_e_diff_por_consumidor`, executa o command real e cobra casos A/B/diffs; `:165-174` cobre fila residual/isento |
| T06 | resposta de porta C17 | `app/chamados/juizes.py:1667-1668`: `rota,fila,verbos,fio`, ou `None` em `:1655`, `:1664` | `app/chamados/services/vigia_fio.py:635`, `:642-646`: se não None, `verbos.values()` e `rota`; sem defaults | Não demonstrado. `fila`/`fio` não são lidos por este consumidor | **Sim:** `app/chamados/tests/test_o261e_f5_alcance.py:317-324`, `test_MORDE_o_papel_SEGUE_o_juiz_e_nao_uma_tabela_do_vigia`, troca resposta do juiz e exige mudança no papel; produtor C17 tem bateria `app/chamados/tests/test_e3_pendentes_app.py:160-192` |
| T07 | resposta de porta C17 | `app/chamados/juizes.py:1667-1668`, ou None | `app/escala/services/cadastro_realidade.py:488`: só a distinção `is not None`, sem leitura de chave | Não demonstrado | **Sim:** dois contraexemplos reais `app/escala/tests/test_f3_beco1_folga_na_lista.py:163-196` |
| T08 | resposta de porta C17 | `app/chamados/juizes.py:1667-1668`, ou None | `app/folha/fora_da_folha.py:67`, `:78`: `fio`, None→`FIO_SEM_PORTA`; `:225`: `.get('fila') or ()`, depois `list` | Não demonstrado | **Sim:** `app/folha/tests/test_haiku_fora_da_folha.py:93-112`, juiz falso diz `xyz`/`abc` e a folha deve conservar; `:114-148` garante que número do chamado sai de `fila`, não das âncoras em `verbos` |
| T09 | resposta de porta C17 via casca `_porta` | `app/chamados/juizes.py:1667-1668`, casca devolve intacta em `app/folha/fora_da_folha.py:67` | `app/chamados/management/commands/diff_e3_sombra.py:124`, `:51-57`, `:139-141`: `fio,fila,rota,verbos`; None→`nada,(),None,[],False`, conforme projeção | Não demonstrado | **Sim:** `app/chamados/tests/test_e3_diff_sombra.py:138-144` fixa projeções; integração real em `:150-163` |
| T10 | tarefa do dia/CargaDaTarefa | `app/chamados/services/tarefa_do_dia.py:182-183`: `colaborador_id,dia_do_turno,agora,celula_id,nao_sei,razao,instancias`; itens em `:256-258`, ações em `:113`; `:553-554` acrescenta sempre `rota,fila,verbos,fio` antes de `CargaDaTarefa.tarefa` retornar `:533` | `app/chamados/management/commands/diff_e3_sombra.py:125`, `:44`: `rota,fila,verbos,fio`, só projeta se `rota` truthy; `:68`: `razao`; sem defaults de chaves | Não demonstrado. Demais dados da tarefa não são necessários à projeção desse leitor | **Sim:** `app/chamados/tests/test_e3_diff_sombra.py:131-136`, conserva exatamente quatro campos e None sem rota; integração `:150-174`; produtor real de tarefas selado por `app/chamados/tests/test_e3_portas_tarefa.py:32-82` |
| T11 | tarefa do dia para backfill | Mesmo produtor de T10; itens têm `instancia,tipo,hora,data_civil,lampada,vencido,motivo,pergunta_id,razao,acoes`; ação tem `acao,de,porta` (`app/chamados/services/tarefa_do_dia.py:113`, `:256-258`, `:316-318`) | `app/chamados/management/commands/backfill_fio_morto_e4b.py:87`, `:97-117`: `instancias`, `instancias[].{razao,pergunta_id,acoes}`, `acoes[].{de,acao,porta}`, `razao`, `nao_sei`; todos por acesso obrigatório, sem `.get` | Não demonstrado | Selo real produtor→backfill completo [não verificado]. `app/chamados/tests/test_e4b_classe_e_memoria.py:592-595` **substitui** tarefa por dict sintético e usa corpo real do leitor em `:638`; isso cobre leitor, não compatibilidade com produtor real |
| T12 | relatório censo_alcance | `app/chamados/services/vigia_fio.py:571-575`: `alcance_universo,alcance_competencias,alcance_porta_fila,alcance_porta_pergunta,alcance_de_hoje_pra_frente,fio_morto_alcance,alcance_porta_exige_o_admin,alcance_e_detalhe,alcance_por_rota_papel,alcance_dias,alcance_dias_omitidos`, `alcance_<causas/baldes>`; `:589` acrescenta `alcance_cego`; retorno `:700` | `app/chamados/management/commands/vigiar_fio_morto.py:176`, `:189`: encaminha inteiro a `linhas_alcance`; `:240`: `fio_morto_alcance`; `:243`: persiste inteiro dentro do carimbo `alcance` | Não demonstrado; leitura nominal e cópia integral | Selo específico desse par [não verificado]. Integridade dos campos/listas do produtor coberta por `app/chamados/tests/test_o261e_f5_alcance.py:385-400` |
| T13 | relatório/lista censo_alcance | Mesmo produtor de T12. Linha sempre `[celula_id,colab_id,data,competencia,rota,causa,detalhe,papel]` em `app/chamados/services/vigia_fio.py:629` | `app/chamados/management/commands/backfill_fio_morto_e4b.py:79`: `alcance_dias`, posição 4=`sem_porta`; `:239`: `alcance_universo,fio_morto_alcance`; `:242-254`: posições 3/1 e desempacota as oito colunas, parse ISO de `data` | Não demonstrado | Selo produtor→backfill real completo [não verificado]. Teste de backfill usa linha sintética de oito colunas em `app/chamados/tests/test_e4b_classe_e_memoria.py:620-626`; integridade do produtor isolado em `app/chamados/tests/test_o261e_f5_alcance.py:385-400` |

A estrutura de `alimentar_portas` não pode ser tomada como “mesma resposta” da tarefa apenas por ambos terem informação sobre porta. A primeira entrega mapas brutos `fila_ids,vivas,instancias` usados no vigia (`app/chamados/juizes.py:1585-1587`; `app/chamados/services/vigia_fio.py:605-615`); a segunda entrega ações por instância e resume `fio` pelos donos das ações (`app/chamados/services/tarefa_do_dia.py:545-554`). O DIFF extrai da tarefa somente `rota,fila,verbos,fio` (`app/chamados/management/commands/diff_e3_sombra.py:27`, `:44`) e mede diferenças conhecidas por consumidor (`app/chamados/tests/test_e3_diff_sombra.py:161-163`). Isso é diferença declarada, não desencontro acidental comprovado.

A memória e os números A1/A2/A3 da execução real não são inferíveis da leitura acima. [não verificado]

Adicional fora dessa contagem de 13:

| Estrutura | Emissor | Escritas | Consumidor | Leituras | Desencontro | Selo; dono; classe |
|---|---|---|---|---|---|---|
| push web de chamados | `app/chamados/push_utils.py:28-33` | `title,body,url,requireInteraction=True` | `app/static/sw.js:89-99`, `:102-107` | `title || 'Hasner Ponto'`, `body || 'Nova notificação'`, `urgente || false`; copia payload em notification.data | **Sim:** emissor escreve `requireInteraction`; consumidor procura `urgente`. `url` é transportada dentro de `data` (`:96`) mas o clique não a lê: foca primeira janela ou abre `'/'` (`:105-107`) | Selo específico [não verificado]; ESTRUTURA provável; classe sim |

Correção à minha contagem anterior: os dois pares `data_turno` das APIs leves devem ser classificados como **semânticas distintas observadas**, não automaticamente defeito, porque o catálogo distingue os módulos e a consequência alcançável não foi verificada. Fontes da diversidade continuam `app/api/views.py:714-716`, `app/api/views_core.py:708-710`, `app/ponto/management/commands/processar_alertas_turno.py:349`. Assim, dos 57 pares anteriores há **4 incompatibilidades de chave demonstradas** (retro→gerador, retro→models, retro→foto_recebida, bloqueio→catálogo), mais duas diferenças semânticas cuja classificação como defeito é [não verificado].

## 13/11 15:26 · sessao e5aa1e28ad97 · - · - · origem=Ronald (tmux)

Ampliação: **7 pares adicionais diretos/fronteiriços de retornos de portas**, mais **2 pares do retorno enriquecido de autorização por período**. Sem chave consumida ausente demonstrada; há campos retornados sem leitor de produção localizado. O inventário negativo desses campos continua limitado à busca estática. [não verificado]

| ID / estrutura | Emissor | Chaves gravadas | Consumidor | Chaves lidas/defaults | DESENCONTRO | Selo que prende | Dono provável | Classe |
|---|---|---|---|---|---|---|---|---|
| P41 retorno `decidir_he` | `app/ponto/portas/he.py:178`, `:203` | Ambos `{estado,minutos,criada,trocada,no_op}`; ramo idempotente `criada=False,trocada=False,no_op=True`, ramo gravado `no_op=False` | `app/ponto/views.py:529`, `:536` | Indexação de `no_op,estado`; no ramo alterado, `trocada,minutos`, `:540` | `criada` sem leitor de produção localizado; os consumidores internos de `he.py:242` e `:567` leem somente `no_op`. [não verificado] | **Sim para execução do par**, chamadas reais da view: `app/ponto/tests/test_tela_gestao_he.py:254`, `:264`; não prende a sobra `criada` [não verificado] | ESTRUTURA | sim para campo sem leitor [não verificado] |
| P42 retorno `recusar_em_lote` | `app/ponto/portas/he.py:234`, `:240`, `:243`, `:245`, retorno `:246` | `{recusados,no_op,erros}`; erros são tuplas `(colab_id,data,msg)` | `app/ponto/views.py:614` | `recusados,no_op,erros` por indexação; erros inteiros até cinco em `:622` | Nenhuma chave usada ausente; nenhum campo excedente localizado | **Sim**: caminho real tela→porta→view, `app/ponto/tests/test_tela_gestao_he_filtros.py:223`; idempotência do retorno da porta, `app/ponto/tests/test_tela_gestao_he.py:382` | ESTRUTURA | não |
| P43 retorno `autorizar_em_lote` | `app/ponto/portas/he.py:545`, `:565`, `:568`, `:570`, retorno `:571` | `{dias,autorizados,no_op,erros}` | `app/ponto/services/autorizacao_he_periodo.py:277` | `erros` por indexação `:282`; `autorizados` em `:299`; demais preservados para a saída; **reescreve `dias` como total dos itens recebidos**, incluindo os de fora da competência em `:281` | `dias` passa de quantidade dos itens entregues à porta (`he.py:545`) para quantidade de todos os itens do ato (`autorizacao_he_periodo.py:281`): mesmo nome com universos diferentes, transformação explícita | **Sim**: caso cinco dias e retorno, `app/ponto/tests/test_o214_item2_autorizar_periodo.py:177`; borda fora da competência, `:291` | ESTRUTURA | sim, duas definições explícitas |
| P44 retorno `recusar_ponta_pequena` | `app/ponto/portas/he.py:330`, `:335`, `:344` | Todos `{recusada,ja_decidido,minutos}`; ramo já decidido devolve estado em `ja_decidido`; demais `None` | `app/ponto/management/commands/recusar_ponta_pequena.py:170` | `recusada` em `:179`; `minutos` em `:181` | `ja_decidido` sem leitor de produção localizado [não verificado] | **Sim para campos usados**, comando real: `app/ponto/tests/test_o214_ponta_pequena.py:411`; não prende sobra `ja_decidido` [não verificado] | ESTRUTURA | sim para campo sem leitor [não verificado] |
| P45 retorno `revogar_recusa_do_sistema` | `app/ponto/portas/he.py:448`, `:451`, `:458`, `:464`, `:467`, `:471`, `:477` | Todos `{revogada,por_que_nao,minutos_na_decisao,minutos}`; sem decisão `minutos_na_decisao=None`; motivo textual nos cinco ramos falsos; motivo `None` nos dois verdadeiros | `app/ponto/management/commands/lavrar_he_pendente.py:59` | Somente `['revogada']` em `:60`; devolve bool ao callback de `apurar` | `por_que_nao,minutos_na_decisao,minutos` sem leitor do **retorno** em produção localizado; os campos homônimos do modelo têm outro fluxo de leitura. [não verificado] | **Sim para `revogada` e fiação**, comando fim a fim: `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:338`; não prende sobras [não verificado] | ESTRUTURA | sim para campos sem leitor [não verificado] |
| P46 retorno `regenerar_em_exportada` | `app/ponto/portas/regen_exportada.py:221` | `{celulas,vinculos,sem_cobertura,ambiguos,hash_antes,hash_depois,antes,depois,mudou,campos_que_mudaram,regua_antes,regua_depois}` | `app/chamados/services/passivo_validado.py:444` | `.get('mudou')` em `:448`; pauta lê `.get('antes')`, `depois or {}` em `:178`; `campos_que_mudaram or []` em `:180`; `regua_antes,regua_depois` em `:189`; `hash_antes,hash_depois,celulas` em `:207`; preserva inteiro no retorno em `:400` | `vinculos,sem_cobertura,ambiguos` sem interpretação pelos consumidores localizados; dict inteiro segue no relatório, portanto “ninguém lê” absoluto **não demonstrado** | **Sim para hash/pauta**, porta real: `app/chamados/tests/test_o317_passivo_pela_porta.py:223`; números pago/devido `:631` e `:650`; régua antiga `:695` | ESTRUTURA | não demonstrada |
| P47 mesmo retorno de regeneração, aninhado no relatório | `app/ponto/portas/regen_exportada.py:221`, repasse `app/chamados/services/passivo_validado.py:400` | Mesmo esquema de P46 | `app/chamados/management/commands/materializar_passivo_validado.py:267` | `(c['regen'] or {}).get('hash_depois','-')` em `:271`; `(c['regen'] or {}).get('mudou')` em `:287` | Nenhuma chave consumida ausente | **Não localizado**; os testes do serviço em `test_o317_passivo_pela_porta.py:223` não executam esse comando [não verificado] | ESTRUTURA | não |

A mudança explícita de significado de `dias` em P43 não demonstra resultado errado: o serviço inclui os itens recusados por estarem fora da competência no total do ato e os nomeia em `erros`. É o caso literal de “mesmo nome com dois significados”, documentado pela atribuição e pelo teste de borda. `app/ponto/services/autorizacao_he_periodo.py:268`, `:281`, `:282`; `app/ponto/tests/test_o214_item2_autorizar_periodo.py:291`.

| ID / estrutura adicional | Emissor | Chaves gravadas | Consumidor | Chaves lidas/defaults | Desencontro | Selo | Dono | Classe |
|---|---|---|---|---|---|---|---|---|
| P48 retorno enriquecido de `previa/confirmar` | `app/ponto/services/autorizacao_he_periodo.py:278`, `:281`, `:286`, `:300`, `:326`, `:356` | Base `{dias,autorizados,no_op,erros}`; acrescenta `{antes,depois,diff,exportada,periodo,previa}`; **`pauta_dp` só existe quando exportada e houve autorização**, `:299` | `app/ponto/views.py:709`, `:751` | Prévia lê `antes,depois,diff` em `:717`, `:720`; confirmação lê `autorizados,no_op,erros,exportada` em `:755`; repassa dict inteiro como `r` em `:714` | `pauta_dp` e `previa` sem leitura terminal de produção localizada [não verificado]; sua presença condicional não quebra os consumidores encontrados | **Sim para saída usada**, prévia HTML e confirmação com o formulário real: `app/ponto/tests/test_o214_item2_autorizar_periodo.py:579`, `:602`; não prende sobras [não verificado] | ESTRUTURA | sim para campos sem leitor [não verificado] |
| P49 mesmo retorno enriquecido | `app/ponto/services/autorizacao_he_periodo.py:286`, `:326`, via `app/ponto/views.py:714` | Esquema P48 | `app/templates/ponto/gestao_he_periodo_previa.html:23` | `periodo[0:2]`; `dias` em `:27`, `:94`, `:107`; `exportada` em `:31`; `erros`, tamanho e tuplas `[1],[2]` em `:65`, `:70` | Nenhuma chave consumida ausente; campos de P48 não usados aqui | **Sim para integração da prévia**, `app/ponto/tests/test_o214_item2_autorizar_periodo.py:579` | ESTRUTURA | não |

**Offline encerrado como estrutura sem leitor, não uma costura adicional:** os dois emissores estão em `app/ponto/management/commands/testar_pwa.py:240` e `:260`, com `{tipo,timestamp_dispositivo,timestamp_real,timestamp_gap_ms}`, primeiro também `latitude,longitude`. A URL é ligada diretamente à função aposentada por `app/ponto/urls.py:13`. O destino `app/ponto/views.py:419` não parseia o corpo nem lê chave alguma: devolve sempre **410** em `:426`. O instrumento espera **200/201** no primeiro cenário, `testar_pwa.py:254`, e **400** no segundo, `:271`. Não há par gravador→leitor do dict; há emissão para uma porta aposentada. `app/ponto/views.py:419`.

**Contagem separada desta ampliação:** 9 pares PONTO; **1 desencontro de significado comprovado** (P43), **4 pares com campos sem leitor terminal localizado** (P41/P44/P45/P48), **1 par sem selo de execução localizado** (P47). Nos quatro pares com sobras, nenhum selo localizado prende especificamente a ausência de leitor dessas sobras. Os números de ausência são relativos à inspeção realizada, não prova universal. [não verificado]

## 13/11 15:25 · sessao 6af76433e3e9 · - · - · origem=Ronald (tmux)

Mudanças necessárias, por leitura da cópia:

- `vigiar_fio_morto.py::ESPERADO_DO_CARIMBO`: incluir contador de férias com esperado `0` (`chamados/management/commands/vigiar_fio_morto.py:98`).
- `vigiar_fio_morto.py::Command.handle`: chamar `ferias_concedidos_x_soma()` e levar seu resultado ao `censo`, pelo caminho já usado pelos demais contadores (`chamados/management/commands/vigiar_fio_morto.py:175`, `chamados/management/commands/vigiar_fio_morto.py:239`).
- A função compartilhada retorna listas distintas `soma_maior` e `concedidos_maior`; o tripwire conta ambas por `len`, sem outra soma (`ferias/services.py:116`, `ferias/management/commands/tripwire_status_ferias.py:38`).
- O item 3 pede explicitamente `concedidos_maior`; o anexo enfatiza `soma_maior`. Sugiro publicar ambos, com esperado `0`, preservando os sentidos já existentes (`ferias/services.py:117`).
- Ajustar o teste que compara os dicionários completos: `VigiaDoFioMortoTest.test_MORDE_o_carimbo_guarda_os_CONTADORES_que_a_rodada_mediu`; incluir novos valores no `censo` e no `esperado` (`chamados/tests/test_o261_vigia_fio.py:815`, `chamados/tests/test_o261_vigia_fio.py:825`).
- Esse teste executa o comando, que lê o relógio; adicionar `freeze_time` com instante literal ao alterá-lo (`chamados/tests/test_o261_vigia_fio.py:806`, `chamados/management/commands/vigiar_fio_morto.py:166`).

Nenhuma escrita ou execução de teste realizada. Resultados de execução: [nao verificado]. [nao verificado]

## 13/11 15:24 · sessao e6c5df440ac0 · - · - · origem=Ronald (tmux)

Censo parcial de `chamados`: leitura de AST, `rg`, chamadas e corpos dos consumidores, sem importar Django nem executar testes. As declarações persistidas estão em `app/chamados/models.py:47` (`contexto_json`), `:1742` (`motivos`), `:1746` (`contexto`), `:2444` (`opcoes`) e `:3408` (`chamados_relacionados_ids`). A enumeração abaixo não comprova exaustividade de aliases dinâmicos, ORM indireto, templates/JavaScript e todos os pares possíveis. [não verificado]

**Achados de estrutura confirmados**

1. O exemplo fornecido confere: `app/ponto/management/commands/marcar_foto_ausente_retro.py:98-103` escreve `foto_ausente.{tipo_batida,batida_id,batida_em,_por}`. `app/chamados/services/disputa_emissao.py:372` busca `motivo.data` e `:387` busca `ultima_batida.id`; `app/chamados/models.py:3464` busca apenas `ultima_batida.id`. São dois pares gravador–leitor, ambos com desencontro de episódio. A presença das chaves no contexto fundido em produção é [não verificado].

2. Há um terceiro consumidor da mesma forma: `app/api/foto_recebida.py:67-69` só trata a disputa cuja `ultima_batida.id` coincide com a batida recebida. A escrita retroativa acima não entrega essa chave. Dono provável ESTRUTURA e classe sim: são formas de contrato incompatíveis, independentemente de CPF/colaborador. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/api/foto_recebida.py:67`)

3. `app/ponto/views.py:280` escreve `contexto_json={'pendentes': resumo,'total':len(...)}`, renovando em `:288` ou persistindo no nascimento em `:301`, para `entrada_bloqueada_turno_aberto` (`:283`, `:299`). O catálogo declara que esse módulo tem `chave_data='data_turno'` em `app/chamados/catalogo/modulos.py:182`, e seu leitor consulta a chave declarada em `:225`. O emissor não entrega `data_turno`. Dono provável ESTRUTURA; classe sim. Efeito/quantidade em produção: [não verificado].

4. Mesmo nome com dois significados: `contexto_json.data_turno` das APIs leves recebe `localdate(batida.timestamp)`, data civil (`app/api/views.py:714-716`; `app/api/views_core.py:708-710`), enquanto emissores de turno escrevem o dia de turno/grade (`app/ponto/management/commands/processar_alertas_turno.py:349`; `app/ponto/management/commands/reconciliar_grade.py:517`). O catálogo e o leitor consomem ambos pela mesma chave (`app/chamados/catalogo/modulos.py:168`, `:225`). Incidência noturna em produção: [não verificado]. Dono provável ESTRUTURA; classe sim.

5. `horario_previsto` possui duas formas: hora `HH:MM` no detector (`app/chamados/detector_anomalias.py:234`), levada ao `contexto_json` das APIs em `app/api/views.py:726` / `app/api/views_core.py:720`; datetime ISO no detector de ausência (`app/ponto/management/commands/detectar_ausencias.py:708`). `app/chamados/data_evento.py:39` só interpreta a forma que contém data, enquanto `app/chamados/management/commands/nomear_marco_por_lampada.py:65-68` fatia `[11:16]`. A segunda rotina é selecionada por suas consultas; não conferi integralmente se alcança módulos leves. Portanto diversidade de forma comprovada, impacto/desencontro alcançável [não verificado].

**Selos**

- `app/chamados/tests/test_e1_v6_horizonte.py:252-276`, `EmissorBatidaPorEpisodioTest.test_RED_segunda_batida_na_disputa_aberta_ganha_pergunta`, monta manualmente a forma `ultima_batida.{id,tipo,timestamp}` em `:260`, chama a porta em `:268-269` e cobra os episódios em `:272-276`. Prende o caminho feliz da porta; **não prende todos os emissores reais**, portanto não sela as três costuras do retro.
- `app/chamados/tests/test_porta_contexto.py:199-232` prende escrita de campos/idempotência; **não prende o vocabulário entregue pelos emissores**.
- Selo específico de chaves para cada par enumerado abaixo não localizado na leitura realizada; ausência absoluta de teste [não verificado]. Não executar testes: regra de só leitura e ausência de banco autorizado. [não verificado] [nao verificado]

**Catálogo de escritores**

Porta material de atualização de `contexto_json`: `app/chamados/services/contexto.py:62-78` (`anotar`, todas as kwargs; grava as sem valor), `:82-104` (`renovar`, todas as kwargs; `decide_por` controla guarda), `:108-127` (`retirar`, remove chaves nomeadas), `:131-163` (`fundir`, lista por união, escalar ausente). Os módulos abaixo são os emissores lógicos; a porta não conhece um esquema fixo de chaves. (`app/chamados/services/contexto.py:71`, `:98`, `:146`)

| Estrutura/subtipo | Escritores e chaves |
|---|---|
| Disputa, batida API | `app/api/views.py:758-765`, `app/api/views_core.py:752-759`: `ultima_batida.{id,tipo,timestamp}`, além de `contexto_anomalia` produzido pelo detector. |
| Disputa, retro foto | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98-103`: `foto_ausente.{tipo_batida,batida_id,batida_em,_por}`. |
| Disputa, falta pela célula | `app/chamados/services/fabrica_da_celula.py:172`: `motivo.data`. |
| Disputa, saída sem entrada PWA | `app/ponto/services/triagem_batida.py:738-743`: `saida_sem_entrada.{data,hora_tentativa}`. |
| Disputa, detector | `app/chamados/detector_anomalias.py:49-53`: `orfao_14h.{entradas_pendentes,data,hora_abertura}`; `:55` `_erro_orfao`; `:65-68` `colisao_escala.{escalas_ids,count}`; `:92-98` `ordem_substituicao_invalida.ordens[].{ordem_id,problemas}`; `:103-106` `posto_desativado.{posto_id,posto_nome}`; `:111` `sem_posto={}`; `:119` `em_ferias` com dict delegado ou `info`; `:121` `_erro_ferias`. |
| Disputa, agregação | `app/chamados/services/disputa_emissao.py:548-562`: passa todas as chaves de `merge_contexto`, acrescenta `motivo.{qtd,ultima_em}`, preserva/funde demais chaves do motivo. Nascimento em `:590`. |
| Disputa, origens especiais | `app/chamados/services/disputa_emissao.py:630`: `origem`; `:1126`: `origem='manual_orfaos'`, `chamados_orfaos_ids`; `:1361`: `origem='ancora_limbo'`, `chamado_limbo_id`; `app/ponto/management/commands/reconciliar_grade.py:427`: `origem='grade'`; `app/chamados/management/commands/smoke_chamados.py:97-102`: motivo.data quando pergunta contém `{data}`. |
| Disputa, anotações | `app/chamados/models.py:2042-2046`: `revalidacoes[].{evento,em,motivo_removido,**contexto_extra}`; `:2104` / `:2122`: chave dinâmica recebida das portas; emissores concretos `app/chamados/views.py:1343` (`supervisao_omissa_escalado`), `app/ponto/management/commands/escalonar_chamados_supervisao.py:159` (`incidente_operacional`), `app/ponto/management/commands/alertar_disputas_pendentes.py:48` (`ultimo_alerta_diario`). |
| Disputa, entrada pendente | `app/chamados/services/disputa_emissao.py:292-296`: `motivo.data` por dia derivado dos `entradas_pendentes[].pk`. |
| Chamado, turno aberto | `app/ponto/management/commands/processar_alertas_turno.py:348-352`: `data_turno,entrada_timestamp,posto`; `app/ponto/management/commands/processar_alertas_avancados.py:126-129`: `data_turno,entrada_timestamp`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:135-180`: contexto variável `_ctx`, conteúdo integral não conferido. |
| Chamado, batida ausente | `app/ponto/management/commands/detectar_ausencias.py:706-712`: `data_turno,horario_previsto,atraso_min,marcos_faltantes`; `:559`: renova `marcos_faltantes,_marco_por`; `:766`: `folga_nao_declarada,nao_calado_porque`. |
| Chamado, reemissão c2 | `app/chamados/reconciliador.py:691`: `data_turno,origem`; condicionalmente `:708-709`: `marcos_faltantes,_marco_por`; a estrutura vai à porta em `:921`. |
| Chamado, grade | `app/ponto/management/commands/reconciliar_grade.py:516-518`: `data_turno,origem,marcos_faltantes[].{tipo,hora,motivo}`; `:473`: renova `marcos_faltantes,orfa_regeneracao_curada`. |
| Chamado, regeneração | `app/ponto/management/commands/gerar_celulas.py:264`: `orfa_regeneracao.{em,marcos_na_emissao}`. |
| Chamado, reconstrução de marco | `app/chamados/management/commands/nomear_marco_por_lampada.py:109`: `marcos_faltantes,_marco_por`; `app/chamados/management/commands/carimbar_por_batida_coincidente.py:98`: `batida_id,data_turno,_dia_por`; `app/ponto/management/commands/curar_residuo_bug115.py:124`: `marcos_candidatos,_marco_por`. |
| Chamado, entrada bloqueada | `app/ponto/views.py:280-301`: `pendentes,total`. |
| Chamado, anuência | `app/ponto/services/triagem_batida.py:347`: `tipo_batida,hora_tentativa`; `:353-362`: `motivo_anuencia,tipo_batida,hora_tentativa` e condicionais `foto_b64,foto_nome`; `:470`: `tipo_batida,hora_tentativa,hora_prevista,diferenca_minutos,escala`. |
| Chamado, acesso bloqueado | `app/ponto/services/triagem_batida.py:117-121`: `motivo,situacao,posto`. |
| Chamado, leve | `app/chamados/detector_anomalias.py:231-237`: `tipo,tipo_batida,horario_previsto,horario_real,desvio_minutos,escala`; APIs acrescentam `batida_id,data_turno` (`app/api/views.py:714-716`; `app/api/views_core.py:708-710`) e persistem em `:726` / `:720`. |
| Chamado, geofence | `app/ponto/services/geofence.py:345-352`: ocorrência com `batida_id,alerta_geofence_id,distancia_metros,batida_lat,batida_lon,tipo_batida`; `:359`: agrega `ocorrencias`; `:375-394`: `ocorrencias`, campos da primeira ocorrência e `tipo_alerta,raio_metros,posto_nome,posto_endereco,posto_lat,posto_lon`. |
| Chamado, ausência | `app/ponto/services/ausencia.py:672-676`: `ausencia_id,data,tipo,tem_documento`; `app/api/views.py:3120`: renova `tem_documento`. |
| Chamado, agenda | `app/colaboradores/views.py:2376`: `agenda_dia_ids`. |
| Chamado, aceite escala | `app/colaboradores/services/vinculo.py:419-424`: `snapshot_ecs,ec_nova,template_id,template_nome,data_inicio`. |
| Chamado, desligamento | `app/colaboradores/services/desligamento.py:182`: `data_demissao,motivo_informado,executado_por_username,executado_por_id,executado_em`. |
| Chamado, férias | `app/ponto/utils.py:120`: `agendamento_pk,data_inicio,data_fim`. |
| Chamado, justificativa rejeitada | `app/ponto/services/justificativa.py:301`: `data_turno,tipo,motivo_rejeicao,rejeitado_por`; `app/api/views.py:1978`: `data_turno`. |
| Chamado, par relâmpago | `app/ponto/management/commands/detectar_par_relampago.py:383`: `data_turno,par,duracao_min`. |
| Chamado, cluster | `app/ponto/services/cluster_espurio.py:211`: `data_turno,orfas,n_missing`. |
| Chamado, cadastro | `app/ponto/management/commands/detectar_ausencias.py:224`: `escala_id,tipo_escala_id,tipo_escala_codigo,tipo_ciclo,colabs_mesmo_template,causa,data_inicio`; `:294`: `escala_id,tipo_ciclo,data_inicio`; `:532`: `data_inicio,causa,escala_id`; `:538`: `dias_sem_grade`. |
| Chamado, régua | `app/escala/regua_defesa.py:72-76`: `tipo_escala_id,tipo_escala_codigo,causa,data_exemplo`. |
| Chamado, adesão | `app/chamados/services/adesao.py:79-87`: `adesao,data_inicio,periodo`; `:113`: `agrupado_em`. |
| Chamado, limbo | `app/chamados/services/limbo_folgas.py:68-69`: `sugestao.{classe,dias,ndias,semanas}`; `:276-277`: `agrupado_em,defeito_cadastro`. |
| Chamado, escape | `app/chamados/services/declaracao_texto.py:208`: `escape_colab,escape_colab_em`. |
| Chamado, abertura humana | `app/chamados/services/abertura.py:58-64`: `sistema,nome,empresa,posto,pendente_glpi`; `:183-192`: acrescenta `aberto_por_user_id,aberto_por` e chave de dia calculada quando recebe `data_fato`; `:116`: `pendente_glpi=False`. |
| Chamado, agrupamento disputa | `app/chamados/services/disputa_emissao.py:1116`, `:1122`: `chamados_orfaos_ids`; `:1351`: `origem,chamado_limbo_id`; `:1357`: `chamados_limbo_ids`; `:580`, `:586`: cópia/fusão do contexto de nascimento da disputa. |
| Chamado, escalonamento | `app/ponto/management/commands/escalonar_chamados_supervisao.py:64`, `:138`, `:163`, `:179`, `:195`: `orfao_detectado,escalado_n2_1h,escalado_n3_4h,escalado_n4_comercial,escalado_n5_24h`. |
| Pergunta, opções | `app/chamados/services/disputa_emissao.py:330-342` e `:449`: lista do catálogo (`valor,rotulo` quando dict), posto `id,nome`, escala `id,tipo_escala__template_nome`; `:842`, `:882`, `:1155`, `:1215`, `:1247`, `:1376`: listas delegadas/escapes/vazias; `app/chamados/services/acoes_disputa.py:267`: lista textual; `app/chamados/services/declaracao_texto.py:376`, `:666`: vazia. |
| Pergunta, clones | `app/chamados/services/disputa_emissao.py:834-845`: `chamados_relacionados_ids` quando `relacionados_ids is not None`. |

**Catálogo dos leitores de chaves**

| Estrutura | Leitores verificados |
|---|---|
| Disputa.contexto | `app/chamados/services/disputa_emissao.py:59-73`: `entrada_sem_saida.{entrada_id,entrada_aberta_em}`, `ultima_batida.id`; `:191-196`, `:372`: `motivo.data`; `:278-280`: `motivo.entradas_pendentes[].pk`; `:387`: `ultima_batida.id`; `:551-552`: motivo.qtd com default 1; `app/chamados/models.py:3464`: `ultima_batida.id`; `app/chamados/catalogo/query.py:58-66`: `motivo.data`, sem ela retorna None quando texto exige data; `app/chamados/juizes.py:391`: `motivo.data`; `app/chamados/services/materializacao.py:346-352`, `:478-484`: `motivo.data` só como fallback a `p.data_evento`; `app/chamados/reconciliador.py:1561-1569`: `motivo.data`, fallback a `p.data_evento`; `app/chamados/management/commands/auditar_disputas.py:75-91`: `motivo`, `data`; `app/api/foto_recebida.py:67-69`: `ultima_batida.id`; `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:52-55`: `device_comprometido.{motivo,device_id_prefix}`. |
| Chamado.contexto_json, data | `app/chamados/catalogo/modulos.py:219-225`: chave dinâmica `MOTIVOS[modulo]['chave_data']`; `app/chamados/juizes.py:118`, `:129`, `:159-166`: `data_turno,batida_id,ultima_batida.timestamp,entrada_timestamp`; lote em `:187`, `:197`; `app/chamados/data_evento.py:27-39`: `data_turno,data_batida,entrada_timestamp,pendentes,horario_previsto`; todas por `.get` sem default explícito. |
| Chamado.contexto_json, turno | `app/chamados/reconciliador.py:146`: `entrada_timestamp`; `:198`: `par`; `:243`: `chamados_orfaos_ids`; `app/chamados/services/disputa_emissao.py:145`: `entrada_timestamp`; `:1143`: `marcos_faltantes`; `app/ponto/services/lastro.py:75-78`: `marcos_faltantes[].{hora,tipo}`; `app/escala/services/regeneracao.py:148-150`, `:196`: `marcos_faltantes`, itens.hora; `app/escala/utils.py:1062-1066`: `agrupado_em,marcos_faltantes`; `app/ponto/management/commands/reconciliar_grade.py:376-381`: `orfao_14h.{data,entradas_pendentes[].data}`, `pendentes`; `:445-465`: `orfa_regeneracao,marcos_faltantes`; `app/ponto/worklist.py:54-55` e `app/ponto/management/commands/reconciliar_fantasmas.py:183-184`: `pendentes`. |
| Chamado.contexto_json, agrupamento | `app/chamados/models.py:816`, `app/chamados/juizes.py:1723`, `:1747`, `app/chamados/services/adesao.py:111`, `app/chamados/services/limbo_folgas.py:253`: `agrupado_em`; `app/chamados/reconciliador.py:243`, `app/ponto/services/entrada_sincronizada.py:51-55`, `app/chamados/signals.py:361`: `chamados_orfaos_ids` inclusive consulta JSON ORM. |
| Chamado.contexto_json, ações | `app/chamados/services/acoes_chamado.py:30-61`: `tipo_batida` default `'E'`, `hora_tentativa` default `''`, `motivo_anuencia` default `''`, `foto_b64`, `foto_nome` default `'selfie.jpg'`; `:96`, `:403`: `agenda_dia_ids` default `[]`; `:144-146`: presença/valor de `snapshot_ecs`; `:185-210`: `distancia_metros,alerta_geofence_id,tipo_batida,posto_nome,raio_metros,posto_lat,posto_lon,batida_lat,batida_lon`; `:247`: `batida_id,ocorrencias[].batida_id`; `:267`, `:275-276`: `batida_id,distancia_metros,posto_nome,raio_metros`; `:371`: `hora_tentativa`. |
| Chamado.contexto_json, ausência | `app/chamados/views.py:1512`, `app/api/views.py:1045`, `app/ponto/services/ausencia.py:715`, `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:32`: `ausencia_id`; `app/api/views.py:3114`: consulta `contexto_json__ausencia_id`. |
| Chamado.contexto_json, cadastro | `app/chamados/views.py:441`, `app/chamados/services/limbo_folgas.py:64`, `:84`: `tipo_escala_id`; `:82-85`: `sugestao.{dias,classe}`; `app/escala/regua_defesa.py:328-334`: `destino.{codigo}`, `tipo_escala_codigo`; `app/escala/management/commands/fechar_avisos_regua_na_lista.py:42`: `classe`; `app/colaboradores/services/pos_vinculo.py:38`: `periodo`. |
| Chamado.contexto_json, reparadores/auditor | `app/chamados/management/commands/nomear_marco_por_lampada.py:50`, `:65`, `:70`: `marcos_faltantes,horario_previsto,origem`; `app/ponto/management/commands/curar_residuo_bug115.py:76-86`, `:121`: `marcos_faltantes,horario_previsto,origem,marcos_candidatos`; `app/ponto/management/commands/gerar_celulas.py:259`: `marcos_faltantes`; `app/chamados/management/commands/auditar_invariantes_chamados.py:93`, `:104`, `:617`, `:686`: `entrada_timestamp`; `:1016`: chave dinâmica do catálogo. |
| Opcoes | `app/chamados/models.py:3540-3543`: `valor`, `rotulo` com default `resposta_colab`; `app/chamados/services/disputa_emissao.py:1435-1458`: opções textuais, dict.valor e dict.id conforme tipo; `app/api/views.py:2538-2540`: lista inteira, filtra `valor='OUTRO_MOTIVO'`; `:1157`: transmite inteiro; `app/api/views_mensageria.py:566`: transmite inteiro ou `[]`; `app/templates/chamados/partials/_card_atendimento.html:105-108`: texto; `:148-149`: `id,nome,label,descricao,tipo_escala__template_nome`; `:166-167`: `valor,rotulo`. |
| Chamados_relacionados_ids | `app/chamados/services/materializacao.py:781-786`: lista ou `[]`, consulta clones; `:975`: reemite lista; `app/chamados/services/validacao.py:431-433`: lista ou `[]` na ordem de travas. |
| Leitura genérica | `app/templates/chamados/partials/painel_chamado_turno.html:33-35` percorre `contexto_json.items`; portanto não afirmar que campos de contexto_json são globalmente “ninguém lê” apenas porque não há `.get` nominal. |

**Pares prontos para a consolidação**

Para não inflar “sem selo”, todos abaixo têm **selo específico do par [não verificado]**; os dois selos parciais descritos acima não cobrem o vocabulário de todos os emissores. “Não demonstrado” significa que não encontrei incompatibilidade, não uma prova universal. [não verificado]

| # | Estrutura | Emissor → consumidor | Escritas → leituras | Desencontro | Dono provável; classe |
|---|---|---|---|---|---|
| 1 | disputa.contexto | retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:98` → `app/chamados/services/disputa_emissao.py:387` | foto_ausente.batida_id → ultima_batida.id | Sim | ESTRUTURA; sim |
| 2 | disputa.contexto | retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:98` → `app/chamados/models.py:3464` | idem | Sim | ESTRUTURA; sim |
| 3 | disputa.contexto | retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:98` → `app/api/foto_recebida.py:67` | idem | Sim | ESTRUTURA; sim |
| 4 | disputa.contexto | `app/api/views.py:758` → `app/chamados/models.py:3464` | ultima_batida.id → mesma | Não demonstrado | ESTRUTURA; sim |
| 5 | disputa.contexto | `app/api/views_core.py:752` → `app/chamados/models.py:3464` | idem | Não demonstrado | ESTRUTURA; sim |
| 6 | disputa.contexto | `app/api/views.py:758` → `app/api/foto_recebida.py:67` | idem | Não demonstrado | ESTRUTURA; sim |
| 7 | disputa.contexto | `app/api/views_core.py:752` → `app/api/foto_recebida.py:67` | idem | Não demonstrado | ESTRUTURA; sim |
| 8 | disputa.contexto | `app/chamados/services/fabrica_da_celula.py:172` → `app/chamados/services/disputa_emissao.py:372` | motivo.data → mesma | Não demonstrado | ESTRUTURA; sim |
| 9 | disputa.contexto | `app/ponto/services/triagem_batida.py:738` → `app/chamados/services/disputa_emissao.py:372` | saida_sem_entrada.data → mesma | Não demonstrado | ESTRUTURA; sim |
| 10 | disputa.contexto | `app/chamados/services/disputa_emissao.py:590` → `app/chamados/catalogo/query.py:58` | contexto_nascimento → motivo.data | Condicional; origem-only não grava data; texto pode dispensá-la `query.py:56` | ESTRUTURA; sim |
| 11 | disputa.contexto | `app/chamados/services/disputa_emissao.py:562` → `app/chamados/juizes.py:391` | motivo.data, demais → motivo.data | Não demonstrado | ESTRUTURA; sim |
| 12 | disputa.contexto | `app/chamados/services/disputa_emissao.py:562` → `app/chamados/services/materializacao.py:346` | motivo.data → mesma se data_evento ause

[truncado em 20000 de 28724 caracteres]

## 13/11 15:24 · sessao e5aa1e28ad97 · - · - · origem=Ronald (tmux)

Recorte PONTO: **40 pares identificados, 4 com desencontro confirmado, 35 sem selo do par localizado**. Contagem por `(estrutura, módulo gravador, módulo leitor)`; vários pontos no mesmo módulo aparecem juntos. Isso é um **mínimo enumerado**, não fecho de “TODO dict”. [não verificado]

Só li fontes. Usei `rg`, inspeção numerada e AST em memória para localizar chamadas de `ponto.registro_batida`, inclusive aliases importados. Não importei a aplicação, não rodei testes e não consultei banco ou logs. O método não fecha aliases dinâmicos, reflexão, consumidores externos, esquemas guardados apenas no banco nem todas as estruturas transitórias. [não verificado] [nao verificado]

Nos quadros, **E** = ESTRUTURA provável para eventual quebra do contrato; **—** = desencontro não demonstrado, não certificação de compatibilidade; **N** = selo do par não localizado [não verificado]. “Classe sim” aparece somente nos desencontros confirmados. A classificação estrutural segue `CLAUDE.md:179`.

**Catálogo dos esquemas, para evitar repetir chaves:**

- **RB**: porta `registrar_batida`: parâmetros `colaborador,timestamp,tipo,origem`, `autoridade_humana=False`, `fato_de_chao=None`; `**campos` passa inteiro ao modelo; sanitização só examina `latitude,longitude,gps_accuracy` presentes. A omissão de `fato_de_chao` deriva `not autoridade_humana`; a consulta de competência ocorre quando resulta falso. `app/ponto/registro_batida.py:5`, `:94`, `:105`, `:130`, `:152`, `:157`.
- **AN**: porta `anotar`: lê todas as chaves de `campos`, valida contra `CAMPOS_ANOTAVEIS`, compara com atributos atuais, grava e monta trilha; `extra` funde no `depois`. Permitidas: `tipo,aprovada,aprovada_por,aprovada_em,origem,retratada_em,foto,justificativa,pergunta_origem`. `app/ponto/registro_batida.py:203`, `:235`, `:241`, `:244`, `:261`.
- **CA**: `causa_espelho`: `{causa,dias_furo,dias_previstos,evidencia}`, ou `None` se classificação falhar. Produção do dict: `app/folha/causas.py:33`, `:36`, `:45`; persistência: `app/ponto/services/fechamento.py:1080`.
- **VI**: snapshot vínculo: `{total,achados,esperado,por_colab}`, item `{colab,veredito,posto_id,posto,motivo,dist_atual_m,sugerido}`. Leitura acrescenta `medido_em,data_ref`; sem snapshot retorna `total=None,por_colab={},esperado=0,sem_medida=True`. `app/ponto/services/vinculo_na_lista.py:105`, `:118`, `:132`, `:133`.
- **TE**: snapshot teto: `{total,colabs,universo,por_competencia,por_dono,janelas,competencias,erros,erros_amostra,itens,rotulo,onde_olha}`; item `{colab,empresa,data,competencia,entrada,saida,hi,hf,vao_min,teto_min,ec,template_id,dono}`. `app/ponto/services/teto_na_lista.py:78`, `:96`. Leitura acrescenta `medido_em,data_ref`; sem medida `{total:None,itens:[],sem_medida:True}`. `:125`.
- **CX**: snapshot CXR: campos de TE, mais `sem_dia,repetidos_do_motor,fora_da_competencia,por_causa,cobertos`; item-base `{colab,empresa,competencia,data,ec,template_id,dono}`, campos opcionais `causa,real,cadastrado,minutos_fora,entrada_real,entrada_cadastrada,saida_real,saida_cadastrada,delta_entrada_min,delta_saida_min`. `app/ponto/services/cxr_na_lista.py:30`, `:84`, `:125`.
- **HE**: retrato `{competencia,data_ini,data_ref,colabs,dias,pendentes,erros,recusas_revogadas}`; colaborador `{colab,nome,dias_trabalhados,dias,minutos,sem_decisao}`; dia `{data,estado,minutos,pontas}`; ponta `{hora,marco,minutos,sentido}`. `app/ponto/services/he_pendente_lavrado.py:148`, `:160`, `:169`.
- **ES**: esmeril `{lavrado_em,universo,colabs_com_anomalia_recorrente,por_assinatura,por_colab,destino}`; assinatura `{o_que_se_repete,dono,colabs,ocorrencias,top5}`. `app/ponto/services/esmeril_espelho.py:222`, `:227`, `:229`.
- **VF**: quatro snapshots do vigia futuro, cada um com a medida inteira `{contadores,selos,universo,sem_vinculo_do_dia,na_lista_do_admin_c6,em_dia,universo_julgada,julgada_via_admin,ini,fim,esperado,medido_antes_do_gerador}`, mais `total,rotulo`; leitor acrescenta `medido_em`; ausência devolve `total=None,esperado=0,sem_medida=True,rotulo`. `app/ponto/services/vigia_cadastro_futuro.py:83`, `:103`, `:119`, `:121`.
- **AB**: snapshot `{mudos,recusados,total,abriram,janela_horas}`; item `{colaborador_id,colaborador,posto}`, recusados incorporam dados de `erros`. `app/ponto/services/abriu_nao_bateu.py:117`, `:142`, `:145`, `:147`. Leitor acrescenta `medido_em`; ausência `{total:None,sem_medida:True}`. `:159`.

| ID / estrutura | Emissor | Chaves gravadas | Consumidor | Chaves lidas | Desencontro | Selo do par | Dono | Classe |
|---|---|---|---|---|---|---|---|---|
| P01 RB | `app/ponto/views.py:1815` | RB + `autoridade_humana=True,justificativa,aprovada,aprovada_por` | `app/ponto/registro_batida.py:94` | RB | — | N | E | não |
| P02 RB | `app/api/views.py:654`; mapa `:562` | RB + `autoridade_humana=False,canal,sequencia,latitude,longitude,gps_accuracy,timestamp_dispositivo`; condicionais `ordem_substituicao,posto_efetivo` | `app/ponto/registro_batida.py:94` | RB; geo presentes; encaminha demais | — | N | E | não |
| P03 RB | `app/api/views_core.py:645`; mapa `:548` | Mesmas chaves de P02 | `app/ponto/registro_batida.py:94` | RB | — | N | E | não |
| P04 RB | `app/ponto/services/justificativa.py:188`, `:361` | RB + `autoridade_humana=True,justificativa,aprovada,aprovada_por`; só `:361` passa `aprovada_em` | `app/ponto/registro_batida.py:94` | RB, encaminhamento integral | Presença desigual de `aprovada_em`; consumidor não a exige explicitamente | N | E | não |
| P05 RB | `app/ponto/services/veredito_celula.py:323` | RB + `autoridade_humana=True,aprovada_por,is_intervalo` | `app/ponto/registro_batida.py:94` | RB, encaminhamento integral | — | N | E | não |
| P06 RB | `app/ponto/services/triagem_batida.py:702`; mapa `:682` | RB + `autoridade_humana=False,canal,sequencia,latitude,longitude,gps_accuracy,justificativa,aprovada`; condicionais `ordem_substituicao,posto_efetivo` | `app/ponto/registro_batida.py:94` | RB | — | N | E | não |
| P07 RB | `app/chamados/services/regularizacao_ext.py:78` | RB + `autoridade_humana=False,aprovada,aprovada_por,aprovada_em,justificativa`; omite `fato_de_chao` | `app/ponto/registro_batida.py:124` | `autoridade_humana`; `fato_de_chao=None` deriva verdadeiro, não consulta competência | Mesmo default significa fato de chão para regularização institucional; a própria porta declara que regularização deve passar falso em `:126` | **Não**; teste adjacente chama a porta diretamente, sem esse emissor: `app/ponto/tests/test_ajuste_retroativo_folha_fechada.py:56` | E | **sim** |
| P08 RB | `app/chamados/services/materializacao.py:933`, `:1013` | Ambos `autoridade_humana=False,fato_de_chao=False,aprovada,aprovada_por,aprovada_em`; `:1013` também `sequencia,justificativa` | `app/ponto/registro_batida.py:94` | RB | — | N; teste `test_ajuste_retroativo_folha_fechada.py:56` sela porta direta, não todos os emissores | E | não |
| P09 RB | `app/chamados/services/acoes_chamado.py:44` | RB + `autoridade_humana=True,sequencia,justificativa,aprovada,aprovada_por` | `app/ponto/registro_batida.py:94` | RB | — | N | E | não |
| P10 RB | `app/ponto/management/commands/testar_pwa.py:184`, `:198`, `:219`, `:228`, `:229` | RB + `sequencia`; origem teste | `app/ponto/registro_batida.py:94` | RB | — | N | E | não |
| P11 AN | `app/ponto/views.py:472`, `:1835` | `aprovada,aprovada_por,aprovada_em,origem`; outro ramo `justificativa`; `:472` fornece `depois=_snap` | `app/ponto/registro_batida.py:235` | AN; trilha explícita quando fornecida | — | N | E | não |
| P12 AN | `app/ponto/services/justificativa.py:202`, `:377` | `justificativa` | `app/ponto/registro_batida.py:235` | AN | — | N | E | não |
| P13 AN | `app/ponto/services/flip_batida.py:30` | `campos.tipo`; trilha `antes.tipo`; `depois.tipo,causa` | `app/ponto/registro_batida.py:235` | AN; trilha explícita encaminhada | — | N | E | não |
| P14 AN | `app/ponto/services/retratar_humano.py:207` | `aprovada,retratada_em` | `app/ponto/registro_batida.py:235` | AN | — | N | E | não |
| P15 AN | `app/chamados/services/materializacao.py:954` | `pergunta_origem` | `app/ponto/registro_batida.py:235` | AN | — | N; `app/chamados/tests/test_selo_batida_com_origem.py:143` verifica campo permitido, não todos os contratos AN | E | não |
| P16 AN | `app/chamados/services/acoes_chamado.py:54` | `justificativa` | `app/ponto/registro_batida.py:235` | AN | — | N | E | não |
| P17 semeadura `list[dict]` | `app/ponto/management/commands/gerar_batidas_smoke_motor.py:182`; chamada `:107` | `colaborador,timestamp,tipo,sequencia,terminal,tipo_registro,escala,origem,aprovada` | `app/ponto/registro_batida.py:335` | Todos via `Batida(**d)` | — | N; gate de localização da escrita não sela o conteúdo: `app/ponto/tests/test_chokepoint_batida_gate.py:42` | E | não |
| P18 CA | `app/ponto/services/fechamento.py:1080` | CA | `app/ponto/services/fechamento_lista.py:122` | Dict inteiro repassado como `causa` | `dias_furo,dias_previstos` sem leitura terminal localizada [não verificado] | N | E | não |
| P19 CA | `app/ponto/services/fechamento.py:1080` | CA | `app/folha/retrato_folha.py:1113` | `causa` por `.get`, ou `None` se não dict | Igual P18 [não verificado] | N; `app/folha/tests/test_retrato_folha_quadros.py:1459` prende não reclassificar, não esquema integral | E | não |
| P20 CA | `app/ponto/services/fechamento.py:1080` | CA, passando pela linha de `fechamento_lista.py:122` | `app/templates/ponto/_fech_linha.html:16` | `causa` com default `zzz`; `evidencia` em `:18` | Igual P18 [não verificado] | N | E | não |
| P21 VI | `app/ponto/services/vinculo_na_lista.py:119` | VI | `app/escala/services/cadastro_realidade.py:363` | `data_ref or '?'`; itens `motivo or veredito,posto or '-',sugerido`; filtro usa `por_colab` e `posto_id` em `vinculo_na_lista.py:147` | — | **Sim**, cadeia lavra→lista: `app/ponto/tests/test_c2_vinculo_vira_lista.py:63` | E | não |
| P22 VI | `app/ponto/services/vinculo_na_lista.py:119` | VI | `app/ponto/services/geofence.py:77` | Existência da chave colaborador após `colabs_em_revisao`; filtro `posto_id` em `vinculo_na_lista.py:154` | — | **Sim**, mesma lavra e mudança de posto: `app/ponto/tests/test_c2_vinculo_vira_lista.py:73` | E | não |
| P23 VI | `app/ponto/services/vinculo_na_lista.py:119` | VI | `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:33` | `sem_medida`; restante chega pela lista C8 | — | **Sim**, recusa sem lavra e fechamento só dos listados: `app/ponto/tests/test_c2_vinculo_vira_lista.py:87` | E | não |
| P24 TE | `app/ponto/services/teto_na_lista.py:113` | TE | `app/escala/services/cadastro_realidade.py:376` | `itens or ()`; item `colab,data,ec,template_id,dono,competencia,entrada,saida,vao_min,hi or '-',hf or '-',teto_min` | — | **Sim**, lavra→lista e horários: `app/ponto/tests/test_col129_teto_vira_lista.py:71` | E | não |
| P25 CX | `app/ponto/services/cxr_na_lista.py:145` | CX | `app/escala/services/cadastro_realidade.py:393`, `:522`, `:1020` | `itens`; campos opcionais com `.get`; `colab,data,ec,template_id,dono,competencia`; dono também usa `cobertos` e datas via `cxr_lavrado`, `app/ponto/services/cxr_na_lista.py:176` | —; há duas formas opcionais declaradas em `cxr_na_lista.py:29` | **Sim**, motor→lavra→tela: `app/ponto/tests/test_cxr_tela_le_o_motor.py:84` | E | não |
| P26 HE | `app/ponto/services/he_pendente_lavrado.py:182` | HE | `app/ponto/views.py:801`, `:830`, `:906` | Dict repassado a enriquecer; PDF lê `competencia,data_ini,data_ref,colabs or []` | — | N; renderer/ausência têm testes, sem fecho de todas as chaves do produtor: `app/ponto/tests/test_tela_gestao_he.py:82`, `:99` | E | não |
| P27 HE | `app/ponto/services/he_pendente_lavrado.py:182` | HE | `app/ponto/services/gestao_he.py:236`, `:246`, `:278`, `:470` | `colabs or []`, `data_ini,data_ref`; colaborador `colab,dias or [],dias_trabalhados`; dia `data,minutos or 0,pontas or []`; ponta `sentido or 'antes'` | —; `estado` da foto é substituído pelo estado vivo em `:278`, uso diferente explicitado em `:271` | N; igualdade contador/tela tem selo parcial em `app/chamados/tests/test_atalho_he_na_central.py:134` | E | não |
| P28 HE | `app/ponto/services/he_pendente_lavrado.py:182` | HE | `app/ponto/management/commands/recusar_ponta_pequena.py:98`, `:113`, `:128` | `colabs or []`, colaborador `colab,dias`; dia `data,minutos`; idade vem de `calculado_em` em `:105` | — | N | E | não |
| P29 ES | `app/ponto/services/esmeril_espelho.py:324` | ES | `app/chamados/services/validacao.py:897` | `por_colab or {}`, chaves dos códigos por colaborador | — | N | E | não |
| P30 ES | `app/ponto/services/esmeril_espelho.py:324` | ES | `app/escala/services/escala_auto.py:169` | `por_colab or {}`, `.get(str(colaborador_id))` em `:181` | — | N | E | não |
| P31 ES | `app/ponto/services/esmeril_espelho.py:324` | ES | `app/escala/services/cadastro_realidade.py:886` | `por_colab or {}`, `destino or {}` em `:892`; códigos por colaborador | — | N; produtor e leitor têm testes separados, leitor recebe fixture: `app/escala/tests/test_cadastro_x_realidade.py:47`, `:53` | E | não |
| P32 ES | `app/ponto/services/esmeril_espelho.py:324` | ES | `app/api/views_mensageria.py:1793` | `lavrado_em,universo,colabs_com_anomalia_recorrente,por_assinatura` por indexação em `:1796` | — | N | E | não |
| P33 ES | `app/ponto/services/esmeril_espelho.py:324` | ES | `app/ponto/services/pautas_do_esmeril.py:30`, `:33`, `:79` | `por_colab` default `{}`, códigos; `lavrado_em` default `?` | — | N; testes usam dict fabricado, não lavra real: `app/ponto/tests/test_pautas_do_esmeril.py:22`, `:29` | E | não |
| P34 VF | `app/ponto/services/vigia_cadastro_futuro.py:104` | VF | `app/api/views_mensageria.py:648` | Repassa todas as chaves dos quatro contadores | — | N; writer/read tests separados e snapshot simulado: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:821`, `:842` | E | não |
| P35 AB | `app/ponto/management/commands/lavrar_abriu_nao_bateu.py:47` | AB | `app/ponto/services/abriu_nao_bateu.py:160` | Dict inteiro; acrescenta `medido_em` | — | N | E | não |
| P36 AB | `app/ponto/management/commands/lavrar_abriu_nao_bateu.py:47` | AB | `app/api/views_mensageria.py:139` | Dict inteiro via `ultimo_lavrado()`; exceção `{total:None,indisponivel:True}` em `:142` | — | N | E | não |
| P37 push | `app/ponto/webpush.py:44` | `title,body,url` | `app/static/sw.js:89` | `title || 'Hasner Ponto'`, `body || 'Nova notificação'`, `urgente || false`; guarda dict inteiro em notification.data | `urgente` ausente; `url` não é usado no click, que abre `/` em `sw.js:107` | N | E | **sim** |
| P38 push | `app/ponto/management/commands/processar_alertas_turno.py:279` | `title,body,url,requireInteraction` | `app/static/sw.js:89` | Igual P37 | `requireInteraction` escrito, leitor consulta `urgente`; `url` sem uso no click | N | E | **sim** |
| P39 realtime | `app/ponto/realtime.py:32` | `tipo='batida',colab_id,batida_id,ts` | `app/sse_server.py:92` | `n.payload` inteiro, retransmitido como SSE em `:93`; não interpreta chaves | —; consumidor browser não localizado [não verificado] | N | E | não |
| P40 push | `app/ponto/management/commands/processar_alertas_avancados.py:207`, `:240` | Ambos `title,body,url,requireInteraction` | `app/static/sw.js:89` | Igual P37 | Igual P38 | N | E | **sim** |

**JSON persistidos sem par externo de leitura demonstrado — não entram nas 40 costuras:**

| Estrutura | Todos os escritores localizados | Chaves | Leitura / selo |
|---|---|---|---|
| `DiaPago.detalhe` (`app/ponto/models.py:378`) | `app/ponto/services/dia_pago.py:195`, `:257`, `:266`, `:272`, `:277`, `:333`, `:353`, `:356`, `:474`, `:512` | `credito_parcial_min,folga_feriado_min,folga_sem_escala_min,o85_dia_sem_vinculo_min,falta_min,calculador_nao_decide,o_que,dias_sem_regua_no_mapa`; os dois retratos finais escrevem `o_que` | Leitor literal de `folga_feriado_min` está no **mesmo módulo**, `app/ponto/services/dia_pago.py:897`. Leitor externo das chaves não localizado [não verificado]. |
| `FechamentoMensal.motivos_espelho` (`app/ponto/models.py:451`) | `app/ponto/services/fechamento.py:1067`; clone com o mesmo valor em `:1186` | Lista retornada por `folha.export.motivos_retencao_espelho` | Leitor externo do **campo persistido** não localizado [não verificado]. `folha.export.py:972` escreve outro dict `item['motivos_espelho']`, não prova leitura desse JSONField. |
| `Advertencia.detalhe_json` (`app/ponto/models.py:1173`) | `app/chamados/services/acoes_chamado.py:203` | `distancia_metros,raio_metros,posto_nome,posto_lat,posto_lon,batida_lat,batida_lon` | Leitor de produção não localizado [não verificado]. O teste `app/ponto/tests/test_s127_advertencia.py:25` fabrica `distancia,raio,posto` e só verifica roundtrip, não o esquema do emissor. |

**Ampliação identificada, ainda fora da contagem acima:**

- `testar_pwa.py:240` e `:260` são payloads **de batida offline**, não push: `{tipo,timestamp_dispositivo,timestamp_real,timestamp_gap_ms}`; primeiro também `latitude,longitude`; endpoint `/ponto/batida-offline/` em `:249`. Consumidor despachado por `app/ponto/views.py:419`; rastreamento completo ao leitor efetivo pendente [não verificado].
- `app/ponto/portas/he.py:178`, `:203` emite `{estado,minutos,criada,trocada,no_op}`; `:330`, `:335`, `:344` emite `{recusada,ja_decidido,minutos}`; comando lê `recusada` em `app/ponto/management/commands/recusar_ponta_pequena.py:179`. `revogar_recusa_do_sistema` emite formas em `he.py:448`, `:451`, `:458`, `:464`, `:467`, `:471`, `:477`; `app/ponto/management/commands/lavrar_he_pendente.py:60` lê `revogada`. Leitores restantes/selos não fechados [não verificado].
- `app/ponto/portas/regen_exportada.py:221` emite `{celulas,vinculos,sem_cobertura,ambiguos,hash_antes,hash_depois,antes,depois,mudou,campos_que_mudaram,regua_antes,regua_depois}`; `app/chamados/services/passivo_validado.py:444` recebe em `local['regen']`. Leituras posteriores/selos não fechados [não verificado].
- Emissor push adicional de CHAMADOS: `app/chamados/push_utils.py:28` escreve `{title,body,url,requireInteraction:True}` para o mesmo consumidor `app/static/sw.js:89`, com o mesmo desencontro de P38. Deve entrar na contagem de CHAMADOS, não PONTO.
- `colabs_medidos` é lido com fallback por `app/ponto/services/esmeril_espelho.py:329`, enquanto o único produtor localizado desse dict grava `universo`, não `colabs_medidos`, em `:229`. É desencontro interno ao mesmo módulo; não é par entre módulos desta contagem. Não inferir efeito em produção. `app/ponto/services/esmeril_espelho.py:229`, `:329`.

## 13/11 15:21 · sessao 8d66b0d6f2f9 · HJ-090 · verificador · origem=Code

**Persistem quatro bloqueantes de código**, detalhados abaixo. Execução do RED na base, suíte e efeitos em produção: **[nao verificado]**. [nao verificado]

1. **BLOQUEANTE — triagem 5 meio-curada: commit ainda precede o registro efetivo de reversão.** O arquivo `.aplicado.jsonl` nasce antes do lote, mas cada entrada só é escrita após `realinhar_concedidos()` retornar; o `atomic` da porta já terminou nesse momento. ([command:89](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/app/ferias/management/commands/realinhar_concedidos_ferias.py:89); `app/ferias/management/commands/realinhar_concedidos_ferias.py:91–95`; `app/ferias/services.py:150–164`)

   **Contra-exemplo:** a porta grava concedidos `30→33`; `reg.write()`, `flush()` ou `fsync()` falha. O período fica realinhado, com trilha no banco, mas sem entrada íntegra no arquivo aceito por `--reverter`. O snapshot inicial não resolve: contém foto anterior à trava e o próprio comando recusa usá-lo para reversão. Uma última linha JSON truncada também impede carregar todas as entradas anteriores, pois o arquivo inteiro é decodificado antes de começar a reversão. (`app/ferias/management/commands/realinhar_concedidos_ferias.py:54–60`, `:80–95`; `app/ferias/services.py:155–162`)

   **Falha da trilha no meio do lote:** se PA1 terminou e PA2 recebeu `evento() == None`, PA1 permanece aplicado; PA2 desfaz sua alteração; os seguintes não são executados. A atomicidade está por período, não pelo lote. Isso distingue a cura efetiva da triagem 6 da janela ainda aberta na triagem 5. (`app/ferias/services.py:150–164`, `:404–411`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:90–96`)

2. **BLOQUEANTE — escritor de insumo do saldo ainda valida antes da trava.** A edição do direito compara o pedido com `dias_concedidos` da foto da request. A porta abre `atomic`, mas não relê o período sob trava nem recalcula seu status. ([view:844](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/app/ferias/views.py:844); `app/ferias/services.py:725–733`)

   **Contra-exemplo:** edição lê concedidos `0` e aceita direito `20`; outra request agenda `30` pela porta travada; a edição grava depois, deixando saldo `−10`. Mesmo sem concorrência, direito `30`, concedidos `20`, vendidos `10` aceita direito `20`, porque a validação ignora vendidos. É escritor de um insumo que o juiz lê, embora não escreva diretamente `dias_concedidos` ou `dias_vendidos`. (`app/ferias/views.py:834–849`; `app/ferias/services.py:493–511`, `:728–733`; `app/ferias/models.py:219–220`)

3. **BLOQUEANTE — triagem 7 meio-curada: o contador não chega ao ESTADO pelo caminho existente.** O tripwire agora retira `OK` nos dois sentidos e alarma `soma_maior`. Entretanto, só imprime o resultado; o produtor do carimbo consumido pelo ESTADO não inclui os contadores de férias em `esperado`/`censo`. ([tripwire:38](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/app/ferias/management/commands/tripwire_status_ferias.py:38); `app/chamados/management/commands/vigiar_fio_morto.py:98–99`, `:228–241`)

   **Contra-exemplo:** PA com concedidos `30`, soma `33`: o tripwire retorna alarme, mas a execução do produtor do carimbo não entrega esse contador ao ESTADO. O leitor genérico existe e mostraria chaves divergentes, desde que fossem produzidas. (`app/ferias/management/commands/tripwire_status_ferias.py:48–57`; `bin/fio_estado.py:125–126`, `:191–194`; `bin/gerar_estado.py:260–261`)

4. **BLOQUEANTE — triagem 12 meio-curada no caminho até o Haiku.** A API envia `saldo_negativo_dias`; a ferramenta reconstrói o resultado sem repassar esse campo. ([ferramenta:2109](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/mensageria/nucleo/ferramentas.py:2109); `app/api/views_mensageria.py:835–842`)

   **Contra-exemplo:** oito períodos antigos com saldo positivo e o nono com saldo `−3`. A situação ordena por início, a API limita os períodos a oito e a ferramenta descarta o agregado negativo. O Haiku recebe saldo positivo e nenhum valor explícito de saldo negativo. O aviso de consumo acima do direito ainda chega; portanto, não afirmo ocultação completa do problema. (`app/ferias/situacao.py:163–164`, `:179–190`, `:300–302`; `app/api/views_mensageria.py:41`, `:842`; `mensageria/nucleo/ferramentas.py:2113–2119`)

   Na tela, o número negativo individual permanece visível, sem corte. (`app/templates/ferias/saldo.html:41`; `app/templates/ferias/_secao_colab.html:50`)

5. **NAO-BLOQ — RED aceita erro de programação como recusa válida.** `_recusou()` captura qualquer `Exception`. **Contra-exemplo:** trocar o corpo da porta por um `TypeError` antes de escrever deixa verdes os dois testes diretos de recusa por saldo: houve exceção e o banco permaneceu intacto. ([RED:45](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/app/ferias/test_hj090_red_por_valor.py:45); `app/ferias/test_hj090_red_por_valor.py:77–79`, `:91–93`)

   Os testes históricos também não conferem a resposta nem o motivo da recusa: uma negativa antecipada de permissão satisfaria suas asserções de ausência de escrita. Isso enfraquece o selo; não prova que ele deixe de reprovar a implementação anterior por valor. A mordida **executada na base** é **[nao verificado]**. (`app/ferias/test_hj090_red_por_valor.py:40–42`, `:67–71`, `:82–87`, `:96–98`)

6. **NAO-BLOQ — POST convencional preserva o prefill, mas pode escolher um período incapaz de receber o pedido.** Escolhe o primeiro `p.pode_agendar`, cujo critério exige apenas adquirido e saldo positivo. ([view:221](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/app/ferias/views.py:221); `app/ferias/models.py:233–234`)

   **Contra-exemplo:** primeiro período adquirido com saldo `10`, segundo com saldo `30`, histórico futuro de `30` dias. Abre o primeiro formulário com `value=30`, `max=10`, apesar de existir outro período suficiente. Presença desse arranjo nos 32 casos fornecidos: **[nao verificado]**. (`app/ferias/views.py:221–228`; `app/templates/ferias/agendar.html:22`)

7. **NAO-BLOQ — exclusão introduz ordem inversa à documentada.** A ordem declarada é agendamento → período → ausência; a exclusão trava período, apaga ausências e depois apaga agendamento. ([services:744](/home/ronald/codex_ws/9eb4498a96d522ccab69373658b70151c6c88c23/app/ferias/services.py:744); `app/ferias/services.py:557–563`, `:748–749`; `app/ferias/models.py:456–460`; `app/ponto/services/ausencia.py:1267–1271`)

   **Contra-exemplo estrutural:** excluir um período só com agendamento cancelado percorre período → ausência → agendamento. Deadlock concreto entre portas oficiais: **[nao verificado]**, pois a exclusão recusa vivos e `_travar_agendamento` recusa cancelados antes de pedir a trava do período. (`app/ferias/services.py:561–563`, `:745–749`)

   O abono integrado pede a mesma trava do período três vezes, mas mantendo a ordem: entrada da porta, execução do agendamento e releitura para vender. Não há inversão nesses três pedidos. (`app/ferias/services.py:245`, `:493`, `:258`)

8. **NAO-BLOQ — permanecem relógios internos e comando sem `--apply`.** O histórico agora repassa `hoje`, mas o agendamento calcula aviso com outro relógio e a troca de janela também lê a data internamente. **Contra-exemplo:** atravessar a meia-noite entre o cálculo de aviso da view e o da porta pode produzir feedback de 30 dias e gravação de 29 dias de antecedência. (`app/ferias/services.py:451`, `:476`, `:491`, `:637`; `app/ferias/views.py:307–320`)

   Executar apenas `alertar_ferias_vencendo` pode modificar status: chama `sincronizar_status()` incondicionalmente. **Contra-exemplo:** período adquirido cujo limite venceu passa a vencido durante o comando sem flag de aplicação. (`app/ferias/management/commands/alertar_ferias_vencendo.py:14–32`; `app/ferias/models.py:152–158`; `app/ferias/services.py:327–344`)

A conferência dos itens PROCEDENTES da triagem fica assim; “fechou” abaixo significa fechamento pelo código, sem certificação de execução **[nao verificado]**.

| Item | Resultado da cura |
|---|---|
| **1 — NAO-BLOQ** | Os julgamentos específicos de agendamento, abono integrado e venda isolada passaram para depois da trava. Subsiste o escritor de direito do achado 2. (`app/ferias/services.py:245–246`, `:493–505`, `:795–805`) |
| **2 — NAO-BLOQ** | Sincronização e garantia de período existente agora julgam a linha relida sob trava. (`app/ferias/services.py:327–344`, `:374–378`) |
| **4 — NAO-BLOQ** | A contagem de agendamentos vivos passou para dentro da porta, sob trava; ressalva de ordem no achado 7. (`app/ferias/services.py:743–749`) |
| **5 — BLOQUEANTE** | Antes/depois vêm da linha travada; identidade inclui PK e dias dos vivos; arquivos usam abertura exclusiva. Continua a janela commit → registro, achado 1. (`app/ferias/services.py:137`, `:155–176`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:83–95`) |
| **6 — NAO-BLOQ** | Para realinhar e reverter, retorno `None` da trilha levanta dentro do atomic e desfaz aquela unidade. (`app/ferias/services.py:161–162`, `:182–183`, `:404–411`) |
| **7 — BLOQUEANTE** | Alarme corrigido; entrega ao ESTADO ausente, achado 3. (`app/ferias/management/commands/tripwire_status_ferias.py:38–57`; `app/chamados/management/commands/vigiar_fio_morto.py:233–241`) |
| **8 — NAO-BLOQ** | Universo agora é Ausencia férias × batidas apuráveis, sem filtro de status; status e véspera são impressos. Reprodução dos 40 dias: **[nao verificado]**. (`app/ferias/management/commands/listar_ferias_com_batida.py:40–69`) |
| **9 — NAO-BLOQ** | Prefill convencional implementado no redirect, GET e template; ressalva da escolha do período no achado 6. (`app/ferias/views.py:213–228`, `:331–339`; `app/templates/ferias/agendar.html:18–22`) |
| **10 — NAO-BLOQ** | A conta paralela foi removida: `status_de_periodo` chama o `fget` do juiz registrado, inclusive para objeto com os mesmos campos. (`app/ferias/models.py:95–98`, `:219–220`; `app/core/juizes.py:264`) |
| **11 — NAO-BLOQ** | Histórico repassa `hoje` aos dois status; resíduos descritos no achado 8. (`app/ferias/services.py:451`, `:476`) |
| **12 — BLOQUEANTE** | Situação inclui períodos negativos e agregado separado; caminho até Haiku permanece parcial, achado 4. (`app/ferias/situacao.py:300–302`; `mensageria/nucleo/ferramentas.py:2109–2119`) |

O censo dos campos encontra os seguintes escritores e leitores:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| **PA.dias_concedidos** | Realinhar, reverter, histórico, agendar, cancelar DP/admin, trocar janela e aprovar solicitação. (`app/ferias/services.py:156`, `:179`, `:474`, `:509`, `:577`, `:604`, `:635`, `:706`) | Juiz do saldo; contador; reversão; edição do direito; telas. (`app/ferias/models.py:220`; `app/ferias/services.py:125–126`, `:176`; `app/ferias/views.py:844`; `app/templates/ferias/saldo.html:37`) |
| **PA.dias_vendidos** | Abono integrado, histórico e venda isolada. (`app/ferias/services.py:259`, `:475`, `:806`) | Saldo, teto vendável e telas. (`app/ferias/models.py:220`, `:238`; `app/templates/ferias/_secao_colab.html:51`) |
| **PA.dias_saldo** | Não é coluna: deriva de direito − concedidos − vendidos; o escritor adicional de direito está no achado 2. (`app/ferias/models.py:218–220`; `app/ferias/services.py:732–733`) | Status, validação normal, abono, histórico, remarcação, aprovação, venda, situação e API. (`app/ferias/models.py:98`; `app/ferias/views.py:250`; `app/ferias/services.py:294–302`, `:470`, `:631`, `:690`, `:802`; `app/ferias/situacao.py:69`; `app/api/views_mensageria.py:841`) |
| **PA.status** | Portas de saldo por `aplicar_status`; sincronização; garantia/criação por defaults. (`app/ferias/models.py:215`; `app/ferias/services.py:344`, `:369–378`) | Permissões de agendar/vender, seleção na aprovação, tripwire, listas e telas. (`app/ferias/models.py:234`, `:242`; `app/ferias/services.py:92`, `:684–686`; `app/ferias/management/commands/listar_ferias_dp.py:67–75`; `app/templates/ferias/_secao_colab.html:42–45`) |
| **AG.dias/datas** | Histórico, agendamento normal e solicitação criam; `_trocar_janela` altera existentes. (`app/ferias/services.py:447–473`, `:506–508`, `:695–705`, `:636–649`) | Soma/identidade, fracionamento, cancelamento, remarcação, materialização, folha e API. (`app/ferias/services.py:105–137`, `:577`, `:604`, `:635`; `app/ferias/models.py:363–386`; `app/ponto/services/ausencia.py:1185–1207`; `app/folha/export.py:491–493`; `app/api/views.py:2244–2247`) |
| **AG.status** | Histórico fornece status; criações normais usam default; cancelamentos, troca de janela e cron atualizam. (`app/ferias/services.py:451`, `:506–508`, `:580–581`, `:612–613`, `:641–649`, `:695–705`, `:825–829`; `app/ferias/models.py:319`) | Portas de escrita, universo da soma, juiz de férias na janela e espelho Ausencia. (`app/ferias/services.py:53`, `:106`, `:137`, `:561`; `app/ponto/services/ausencia.py:1181`) |
| **Ausencia férias** | `AG.save` chama espelhamento; cria dias, remove cancelados, apaga janela e rejeita no cancelamento admin. Portas genéricas também editam e mudam estado. (`app/ferias/models.py:449–454`; `app/ponto/services/ausencia.py:1203–1208`, `:1189–1193`, `:1263–1284`, `:498–509`, `:825–836`) | Cobertura de ausência, materializador e lista com batidas. (`app/ponto/turnos.py:2554`, `:2753`, `:3158`; `app/ponto/services/ausencia.py:1182–1186`; `app/ferias/management/commands/listar_ferias_com_batida.py:40–63`) |

As portas genéricas de Ausencia continuam podendo modificar férias sem as travas de PA/AG; **contra-exemplo:** editar a data da ausência espelhada modifica a ausência sem mudar a janela do agendamento. Esse é o resíduo remetido a HJ-091 na triagem fornecida; implementação dessa obra: **[nao verificado]**. (`app/ponto/services/ausencia.py:465–509`)

Há também consumidores de saldo em relatórios e no intake IA, além dos leitores acima. (`app/relatorios/services.py:290`, `:316`; `app/relatorios/views.py:370`; `app/core/views_ia.py:261–269`, `:532–538`, `:684–688`)

Os contra-exemplos restantes pedidos têm estes resultados pelo código:

- **NAO-BLOQ — reversão consecutiva 2×:** para antes `30`, depois `33`, a primeira volta a `30`; a segunda é pulada porque `30 != 33`. Trocar o conjunto por outros agendamentos de mesma soma também é recusado pela identidade PK/dias. (`app/ferias/services.py:137`, `:174–185`)
- **NAO-BLOQ — cancelar 33 depois de realinhar:** concedidos passa de `30` para `33`; cancelar devolve `33`, deixando concedidos `0`. O corte antigo não permanece nesse caminho. (`app/ferias/services.py:152–158`, `:604–606`)
- **NAO-BLOQ — rótulo contraditório permanece:** saldo `−3` recebe status `concedido`, cujo tooltip diz “Nada pendente”. O número negativo aparece, mas o texto continua inadequado. (`app/ferias/models.py:98–100`; `app/templates/ferias/_secao_colab.html:44`, `:50`)
- **NAO-BLOQ — data cravada removida da lista com batidas:** `--desde` agora é obrigatório; erros do dono são impressos e contados. (`app/ferias/management/commands/listar_ferias_com_batida.py:25`, `:55–66`)
- A lista DP imprime os dois números e origem/status do período usado; a origem é inferida de `obs`. A simulação pergunta ao validador normal e força rollback, mas não executa a porta normal completa. Aceitação dos mesmos **32**, separação medida dos **52** e reprodução dos **40/quatro prioritários**: **[nao verificado]**. (`app/ferias/management/commands/listar_ferias_dp.py:26–32`, `:55–84`, `:98–111`)

## 13/11 15:17 · sessao 5addfb7c75bc · E3 · leitura · origem=Code

**A e B compartilham a alimentação da fila, mas não carregam o mesmo conjunto de perguntas nem oferecem os mesmos dados públicos. Há duas chamadas de `alimentar_fila` no caminho direto; a expansão pela `ListaDoAdmin` pode acrescentar outras.** (`app/chamados/juizes.py:1594`; `app/chamados/services/tarefa_do_dia.py:376–380`; `app/chamados/services/tarefa_do_dia.py:529–565`; `app/escala/services/cadastro_realidade.py:486–497`)

As contagens abaixo distinguem sítios de carregamento, objetos ORM e projeções `values_list`; quantidade de SQL efetivamente executado e quantidade de linhas em produção são **[nao verificado]**.

**1. Escritores e leitores das sete chaves de A**

A recebe as células dos pares retidos, os colaboradores previamente carregados e `agora`; não recebe simplesmente todas as células entre duas datas. (`app/chamados/services/vigia_fio.py:598–608`)

| Chave | Quem escreve e de onde vem | Quem lê no caminho de `censo_alcance` |
|---|---|---|
| `fila_ids` | `alimentar_fila`: percorre `qs_na_fila(agora=agora).values_list('pk','celula_id')`, descarta PK ausente de `verbo` e adiciona os demais ao conjunto **global**. (`app/chamados/juizes.py:1561–1564`) | O censo cria `fila_pk`; depois testa a âncora fora desse conjunto para formar o véu. (`app/chamados/services/vigia_fio.py:609`; `app/chamados/services/vigia_fio.py:673–674`) |
| `verbo` | `alimentar_fila`: `particionar_verbos(qs_vivos(), agora=agora)['mapa']`. Dentro da própria alimentação, sua presença filtra `fila_ids`. (`app/chamados/juizes.py:1558`; `app/chamados/juizes.py:1562`) | Não há leitura dessa chave de **A** no corpo atual do censo nem nos auxiliares aos quais ele passa A; a porta usada vem de B. (`app/chamados/services/vigia_fio.py:608–625`; `app/chamados/services/vigia_fio.py:640–708`) |
| `fila_cel` | `alimentar_fila`: usa a mesma leitura global da fila, mas só agrupa os chamados cujo `celula_id` pertence aos IDs recebidos. (`app/chamados/juizes.py:1560–1566`) | Não há leitura dessa chave de **A** no caminho atual do censo. (`app/chamados/services/vigia_fio.py:608–625`; `app/chamados/services/vigia_fio.py:640–708`) |
| `vivas` | `alimentar_portas`: `PerguntaDisputa.filter(celula_id__in=_ids).select_related('disputa','celula').order_by('pk')`; passa pelo mapa de identidade e chama `pergunta_viva(p, agora=agora)` para cada pergunta carregada; agrupa as aprovadas por célula. (`app/chamados/juizes.py:1599–1604`) | Alias `perg`; extração das âncoras; seleção de `ps` do dia; status das âncoras/véu; causas e detalhes do sem-porta. (`app/chamados/services/vigia_fio.py:614`; `app/chamados/services/vigia_fio.py:621`; `app/chamados/services/vigia_fio.py:646`; `app/chamados/services/vigia_fio.py:670–700`) |
| `colab` | `alimentar_portas`: mapa célula → colaborador, por `CelulaDia.filter(pk__in=_ids).values_list('pk','colaborador_id')`, sem `select_related`. (`app/chamados/juizes.py:1606`) | Não é lida pelo censo: seu wrapper `no_app(cid)` passa diretamente `cid` para `_no_app`. (`app/chamados/services/vigia_fio.py:624–625`; `app/chamados/services/vigia_fio.py:700`) |
| `no_app` | `alimentar_portas` inicia `{}`; `_no_app` preenche, sob demanda, com `{p.pk for p in perguntas_do_app(_c)}`. (`app/chamados/juizes.py:1606`; `app/chamados/juizes.py:1613–1620`) | `_no_app` testa o cache e devolve o conjunto; o censo testa se alguma pergunta viva pertence a ele, exclusivamente no detalhe `pergunta_viva_fora_do_app`. (`app/chamados/juizes.py:1613`; `app/chamados/juizes.py:1621`; `app/chamados/services/vigia_fio.py:700–701`) |
| `instancias` | `alimentar_portas` semeia `{c.pk:c for c in colabs}`. O mapa de identidade pode acrescentar um colaborador que o consumidor tenha carregado; `_no_app` também pode acrescentar o resultado de `Colaborador.filter(pk=colab_id).first()` quando falta a chave. (`app/chamados/juizes.py:1598`; `app/chamados/juizes.py:1179–1186`; `app/chamados/juizes.py:1616–1619`) | Mapa usado na alimentação das vivas, no segundo carregamento `perg_todas` e em `_no_app`. (`app/chamados/juizes.py:1600`; `app/chamados/services/vigia_fio.py:618–619`; `app/chamados/juizes.py:1616–1619`) |

As queries que sustentam as três chaves da fila são:

- `qs_vivos()` parte de `ChamadoColaborador`, declara `select_related('colaborador','colaborador__empresa','colaborador__posto','respondido_por')`, `prefetch_related('followups')`, anotações e ordenação; sem os filtros opcionais, aplica `status_local__in=VIVOS`. (`app/chamados/services/painel.py:96–115`; `app/chamados/services/painel.py:153–155`)
- `qs_na_fila(agora=...)` acrescenta `status_local__in=COBRANCA` e exclui `silenciado_ate__gt=agora`. (`app/chamados/services/painel.py:188–190`)
- A partição remove o prefetch e lê os campos do chamado por `values_list`; também consulta os créditos de perguntas. Portanto a declaração de `select_related` da base não significa que essa projeção materialize objetos de `Colaborador`. (`app/chamados/services/verbos.py:34`; `app/chamados/services/verbos.py:44–46`)
- Os créditos consultam `PerguntaDisputa` com destino em vivos **ou** chamado da disputa em vivos, aplicam `q_vivas()`, excluem a combinação materializada + `VL_SIM` e projetam cinco campos; não chamam C6 em Python nesse percurso. (`app/chamados/services/raiox.py:56–58`; `app/chamados/services/raiox.py:74`; `app/chamados/services/raiox.py:135–155`)

Os auxiliares citados na pergunta consomem os derivados assim:

| Auxiliar/derivado | Uso |
|---|---|
| `no_app` / `_no_app` | O wrapper encaminha A; `_no_app` usa `no_app` e `instancias`. `perguntas_do_app` consulta `PerguntaDisputa.filter(disputa__colaborador=colab).filter(q_vivas()).select_related('disputa','celula').order_by('id')` e aplica `pergunta_viva(p)` **sem** `agora`, `hoje` ou `carga`. (`app/chamados/services/vigia_fio.py:624–625`; `app/chamados/juizes.py:1613–1621`; `app/chamados/services/pergunta_no_app.py:49–58`) |
| `perg_todas` | É uma variável, não uma função. Recarrega todas as perguntas com `celula_id__in=_cels`, `select_related('disputa','celula')`, compartilhando `alim['instancias']`; não filtra vitalidade. (`app/chamados/services/vigia_fio.py:617–620`) |
| `dono_fora_do_fio` | Recebe `colabs[cid]`, as perguntas de `perg_todas`, a lista e `oferecido`; não recebe o dict A. Das perguntas, lê `p.estado == VALIDADA` e `_e_aceite_de_folga(p)`. (`app/chamados/services/vigia_fio.py:311–314`; `app/chamados/services/vigia_fio.py:662–664`) |
| `p.estado` / aceite de folga | `estado` retorna pelo carimbo de validação quando preenchido; caso contrário chama `pergunta_viva(self)` sem argumentos temporais. O aceite compara `resposta_colab.strip()` com o código `FOLGA_CONTESTA`. (`app/chamados/models.py:3067–3074`; `app/chamados/services/validacao.py:77`; `app/chamados/services/validacao.py:236–238`) |
| `chamado_que_lista` | Recebe perguntas de `vivas`; lê a disputa e devolve `chamado_destino_id` ou `disputa.chamado_id`. É usado para carregar status das âncoras e para as classificações posteriores. (`app/chamados/services/fio.py:116–117`; `app/chamados/services/vigia_fio.py:621–622`; `app/chamados/services/vigia_fio.py:670–698`) |
| `respondida_sem_texto` | Recebe perguntas de `vivas`; lê `respondida_em` e `resposta_colab`, usando o mesmo teste de texto vazio. (`app/chamados/services/vigia_fio.py:682`; `app/chamados/juizes.py:1510–1518`) |
| `ListaDoAdmin` | Não lê as sete chaves de A. Carrega sua própria `cadastro_realidade.lista`, produz `_dias`/`_colabs` e responde presença, dono e papel. O censo consulta `dia_na_lista` e `colab_na_lista`; `dono_fora_do_fio` consulta `dia_do_admin` e `papel_do_dia`. (`app/chamados/services/vigia_fio.py:205–251`; `app/chamados/services/vigia_fio.py:312–314`; `app/chamados/services/vigia_fio.py:669`; `app/chamados/services/vigia_fio.py:686`) |

**2. Equivalência de cada chave com B**

B recebe os mesmos colaboradores de entrada, mas uma janela contínua entre a menor e a maior data do censo; sua base carrega células dessa janela com padding. (`app/chamados/services/vigia_fio.py:639–641`; `app/ponto/turnos.py:2973–2984`)

| Chave de A | Comparação com B |
|---|---|
| `fila_ids` | **Mesma autoridade, query global e `agora`** na chamada de `alimentar_fila` feita por B. Entretanto o dado fica em `carga._portas`, privado; a saída pública publica apenas as portas pertencentes às ações exigíveis da tarefa. (`app/chamados/juizes.py:1558–1567`; `app/chamados/services/tarefa_do_dia.py:529–549`; `app/chamados/services/tarefa_do_dia.py:559–565`) |
| `verbo` | **Mesma autoridade, universo global e argumento temporal** nessa alimentação. O `verbos` público da tarefa é um subconjunto selecionado pelas ações, não o mapa global de A. (`app/chamados/juizes.py:1558`; `app/chamados/services/tarefa_do_dia.py:548–556`) |
| `fila_cel` | **Mesma autoridade, recorte diferente.** A recebe somente `_cels` dos pares enumerados; B passa todas as suas células dentro de `[ini,fim]`, podendo incluir dias intermediários. (`app/chamados/services/vigia_fio.py:600–608`; `app/chamados/services/tarefa_do_dia.py:531–533`) |
| `vivas` | **Não é o mesmo carregamento nem o mesmo predicado aplicado ao mesmo universo.** A usa `celula_id__in` e julga todas as carregadas. B consulta `colaborador_id__in`, `conversa=False`, `data_evento` dentro da janela e só aplica C6 às candidatas selecionadas. (`app/chamados/juizes.py:1599–1604`; `app/chamados/services/tarefa_do_dia.py:376–383`; `app/chamados/services/tarefa_do_dia.py:467–493`) |
| `colab` | **Não há equivalente público desse mapa.** B guarda células em `_cels` e cadastro em `_cadastro`, com outros universos/carregamentos; `porta_da_tarefa` devolve somente `rota`, `fila`, `verbos` e `fio`. (`app/ponto/turnos.py:2978–2984`; `app/chamados/services/tarefa_do_dia.py:372`; `app/chamados/services/tarefa_do_dia.py:565`) |
| `no_app` | **B não carrega esse conjunto pela autoridade `perguntas_do_app`.** Sua seleção usa as perguntas do fato e as ações da tarefa; a interface pública de porta não devolve presença no app. (`app/chamados/services/tarefa_do_dia.py:376–383`; `app/chamados/services/tarefa_do_dia.py:525–565`; `app/chamados/services/pergunta_no_app.py:49–58`) |
| `instancias` | **Não é o mesmo mapa de identidade.** B guarda os objetos recebidos em `_colabs`, mas recarrega `_cadastro`; para candidatas, substitui `p.disputa.colaborador` por esse cadastro recarregado. A utiliza o mapa semeado pelos objetos do censo. As `instancias` públicas da tarefa são linhas de marcos/ações. (`app/ponto/turnos.py:2978`; `app/chamados/services/tarefa_do_dia.py:372`; `app/chamados/services/tarefa_do_dia.py:481–482`; `app/chamados/juizes.py:1598–1600`; `app/chamados/services/tarefa_do_dia.py:184–185`) |

A diferença exata da chamada C6 é:

- A: `pergunta_viva(p, agora=agora)`. (`app/chamados/juizes.py:1603`)
- B: `pergunta_viva(p, hoje=_hoje(self.agora), carga=carga_da_vida(...), agora=self.agora)`, somente nas candidatas. A seleção considera grade, isenção, tipo do dia, tranca, exportação e posições das perguntas; pode dispensar C6 para perguntas normais dentro da grade. (`app/chamados/services/tarefa_do_dia.py:467–493`)
- Com `agora` preenchido, o wrapper de A também deriva `hoje=_hoje(agora)`. Portanto não há diferença necessária no dia civil entre A e B; há diferença de argumentos, alimentação, universo avaliado e uso do memo avulso. (`app/chamados/juizes.py:883–894`)

B ainda faz uma leitura própria de fila para suas âncoras: `qs_na_fila(_base=ChamadoColaborador.objects.filter(pk__in=ancoras), agora=agora)`, guardada em `_fila`. Essa leitura recortada é distinta da alimentação global guardada em `_portas`. (`app/chamados/services/tarefa_do_dia.py:415–418`; `app/chamados/services/tarefa_do_dia.py:529–533`)

**3. Quantas chamadas de `alimentar_fila` por rodada**

No caminho direto, com `dias` não vazio e execução concluída, são **duas**; se `dias` está vazio, retorna antes delas. (`app/chamados/services/vigia_fio.py:594–595`; `app/chamados/juizes.py:1594`; `app/chamados/services/tarefa_do_dia.py:529–533`)

| Sítio | Argumentos |
|---|---|
| A → `alimentar_portas` → `alimentar_fila` | IDs não vazios normalizados das células dos pares retidos; `agora` recebido pelo censo. (`app/chamados/services/vigia_fio.py:602–608`; `app/chamados/juizes.py:1593–1594`) |
| Primeira `carga.tarefa(...)` de B | Todas as células de `_cels` dentro de `[ini,fim]`; `agora=self.agora`. As tarefas seguintes reutilizam `_portas`. (`app/chamados/services/tarefa_do_dia.py:427`; `app/chamados/services/tarefa_do_dia.py:529–534`) |

**Incluindo a expansão transitiva da lista, a contagem em execução concluída é `2 + b + j`:**

- `b=1` se esta chamada efetivamente carregar a lista e `beco_em_lote` encontrar células; caso contrário, `b=0`. A lista é carregada no máximo uma vez por objeto, e o beco retorna antes da alimentação quando não encontra células. (`app/chamados/services/vigia_fio.py:205–216`; `app/escala/services/cadastro_realidade.py:483–486`; `app/escala/services/cadastro_realidade.py:873`)
- `j` é o número de janelas distintas **`(ini,fim)`** com perguntas vivas no beco. Para cada uma, ele cria uma carga B e consulta tarefas, produzindo uma alimentação de fila por carga. (`app/escala/services/cadastro_realidade.py:488–497`; `app/chamados/services/tarefa_do_dia.py:529–533`)
- As janelas extras vêm das competências abertas, cortadas em ontem; as células extras exigem empresa correspondente, colaborador ativo, `veredito='indefinida'` e protesto `PREVISTO_COM_DIA_INDEFINIDO`. (`app/escala/services/cadastro_realidade.py:478–481`; `app/escala/services/cadastro_realidade.py:571–582`)

A lista pode já ter sido carregada pelo `censo()` anterior: o comando compartilha o mesmo objeto entre os dois censos. Nesse caso, seus carregamentos não se repetem dentro de `censo_alcance`. (`app/chamados/management/commands/vigiar_fio_morto.py:170–176`; `app/chamados/services/vigia_fio.py:205–207`)

Quanto à diferença entre os dois resultados principais:

- `verbo` e `fila_ids` têm a mesma definição global, sem recorte por empresa, colaborador ou célula; somente `fila_cel` é recortada pelos argumentos. (`app/chamados/juizes.py:1557–1566`)
- Com o mesmo estado lido e o mesmo `agora`, o recorte das células não altera `verbo`/`fila_ids`; pode alterar `fila_cel`. São leituras separadas, e o código contempla corrida entre partição e fila. Divergência efetivamente ocorrida em produção: **[nao verificado]**. (`app/chamados/juizes.py:1558–1566`)
- O censo avulso cria `ListaDoAdmin(hoje, empresas)` **sem `agora`**; nesse percurso, `lista()` pode usar seu próprio `timezone.now()`. O comando, por sua vez, constrói a lista com o instante da rodada. (`app/chamados/services/vigia_fio.py:627`; `app/chamados/services/vigia_fio.py:199–201`; `app/chamados/services/vigia_fio.py:216`; `app/escala/services/cadastro_realidade.py:803–804`; `app/chamados/management/commands/vigiar_fio_morto.py:166–170`)

**4. Carregamentos de `PerguntaDisputa` e `Colaborador`**

Para `PerguntaDisputa`, o núcleo contém **três carregamentos de objetos**, mais **duas projeções de créditos**, além dos ramos condicionais abaixo. (`app/chamados/juizes.py:1599`; `app/chamados/services/vigia_fio.py:618–619`; `app/chamados/services/tarefa_do_dia.py:376–380`; `app/chamados/services/raiox.py:135–138`)

| Sítio | Frequência e filtro |
|---|---|
| A | Uma vez: `celula_id__in=_cels`, `select_related('disputa','celula')`, ordem PK. (`app/chamados/juizes.py:1599–1600`) |
| `perg_todas` | Uma vez: mesmo filtro de células e mesmos relacionamentos; sem o filtro Python de vida. Recarrega o universo SQL de A. (`app/chamados/services/vigia_fio.py:618–620`) |
| B | Uma vez: `colaborador_id__in`, `conversa=False`, `data_evento__gte=ini`, `data_evento__lte=fim`; `select_related('disputa__colaborador__empresa','celula','chamado_destino')`. (`app/chamados/services/tarefa_do_dia.py:376–380`) |
| Créditos da fila | Uma projeção por `alimentar_fila`, portanto `2+b+j` no grafo descrito: destino ou pai em vivos, `q_vivas()`, exclusão materializada + `VL_SIM`; cinco campos, sem objetos de pergunta/colaborador. (`app/chamados/services/verbos.py:34`; `app/chamados/services/raiox.py:56–58`; `app/chamados/services/raiox.py:74`; `app/chamados/services/raiox.py:135–138`) |
| `_no_app` | Uma consulta por colaborador cujo cache `no_app` seja solicitado: `disputa__colaborador=colab`, `q_vivas()`, `select_related('disputa','celula')`; sem recorte por datas/células/conversa. (`app/chamados/juizes.py:1613–1621`; `app/chamados/services/pergunta_no_app.py:49–58`) |
| Calendário do “Resolver dia” | Uma consulta por bloco de datas de cada colaborador sem porta em dia passado: `disputa__colaborador=colaborador`, `data_evento__range`, exclui `resposta_colab=''`; `select_related('disputa__colaborador','celula','chamado_destino')`. Os blocos seguem o teto de 366 dias. (`app/chamados/services/vigia_fio.py:642–643`; `app/chamados/services/vigia_fio.py:266–271`; `app/colaboradores/services/calendario.py:114`; `app/colaboradores/services/calendario.py:128–137`; `app/colaboradores/services/calendario.py:472–476`) |
| Expansão do beco | Mais `b` carregamentos A e `j` carregamentos B, com os universos próprios do beco/janelas. (`app/escala/services/cadastro_realidade.py:478–497`) |

A consulta A e `perg_todas` têm o mesmo universo SQL; `vivas` é a seleção Python da primeira. B, app, calendário e créditos podem se sobrepor a elas, mas seus filtros não definem conjuntos iguais nem uma relação geral de inclusão entre todos eles. Interseções concretas: **[nao verificado]**. (`app/chamados/juizes.py:1599–1604`; `app/chamados/services/vigia_fio.py:618–620`; `app/chamados/services/tarefa_do_dia.py:376–380`; `app/chamados/services/pergunta_no_app.py:49–58`; `app/colaboradores/services/calendario.py:473–474`; `app/chamados/services/raiox.py:56–74`)

Para `Colaborador`, há **duas cargas explícitas no núcleo**, sobre os mesmos IDs: a do censo e a recarga `_cadastro` de B. B também materializa colaboradores pelo JOIN de sua consulta de perguntas, mesmo depois substituindo os das candidatas pelo cadastro. (`app/chamados/services/vigia_fio.py:607`; `app/chamados/services/tarefa_do_dia.py:370–380`; `app/chamados/services/tarefa_do_dia.py:481–482`)

| Sítio adicional de `Colaborador` | Frequência/filtro |
|---|---|
| Formação do universo: export | Por empresa × competência: `FechamentoMensal.filter(colaborador__empresa=empresa, mes=mes, ano=ano).select_related('colaborador')`. (`app/chamados/services/vigia_fio.py:586–589`; `app/folha/fora_da_folha.py:130`; `app/folha/export.py:785–791`) |
| Formação do universo: retidos | Por empresa × competência: `Colaborador.filter(pk__in=_ids_furo).select_related('empresa')`. Os IDs vêm dos itens classificados como `furo_espelho`, portanto repetem um subconjunto do carregamento anterior. (`app/folha/fora_da_folha.py:133–142`) |
| `_no_app` | Fallback `filter(pk=colab_id).first()` somente se o ID faltar em `instancias`; o censo semeia o mapa com seus colaboradores. (`app/chamados/juizes.py:1598`; `app/chamados/juizes.py:1617–1619`; `app/chamados/services/vigia_fio.py:607–608`) |
| Calendário | JOIN `disputa__colaborador` por bloco descrito acima; o mapa substitui a instância carregada pela recebida. (`app/colaboradores/services/calendario.py:472–475`) |
| Beco | Se houver células: `filter(pk__in={colaboradores das células}).select_related('empresa')`; cada carga B extra também repete sua recarga de cadastro e seu JOIN. (`app/escala/services/cadastro_realidade.py:485–493`; `app/chamados/services/tarefa_do_dia.py:372–380`) |
| Lista: afastados | Lê PKs de `exclude(situacao='desligado')`, opcionalmente filtrado por empresa, com `situacao='afastado'`; o queryset também limita a consulta do juiz de afastamento. Havendo PKs, carrega `filter(pk__in=pks).select_related('empresa').order_by('pk')`. (`app/escala/services/cadastro_realidade.py:612–624`; `app/colaboradores/services/situacao_exibida.py:44–47`) |
| Lista: nunca bateu | Por empresa, projeção de PKs com `pk__in` nos c

[truncado em 20000 de 23006 caracteres]

## 13/11 15:17 · sessao f0888a9c60e1 · - · - · origem=Ronald (tmux)

Achados da frente telas/Haiku/RED:

1. **BLOQUEANTE — saldo negativo perde o campo no caminho até Haiku.** A API envia `saldo_negativo_dias`, mas a ferramenta reconstrói o payload sem ele: `app/api/views_mensageria.py:836`; `mensageria/nucleo/ferramentas.py:2109-2119`. Contra-exemplo: oito períodos antigos positivos e o nono com saldo −3. A ordenação privilegia os antigos e a API corta em oito (`app/ferias/situacao.py:163-164`; `app/api/views_mensageria.py:41,842`); Haiku recebe apenas os positivos, `saldo_dias` positivo e nenhum número de saldo negativo. O aviso ainda informa consumo acima do direito, portanto o passivo não fica inteiramente oculto (`app/ferias/situacao.py:179-190`). Na tela principal e no drawer, o saldo individual é exibido sem corte (`app/templates/ferias/saldo.html:41`; `app/templates/ferias/_secao_colab.html:50`).

2. **NAO-BLOQ — RED aceita erro de programação como recusa de domínio.** `_recusou` captura qualquer `Exception` (`app/ferias/test_hj090_red_por_valor.py:45-51`). Contra-exemplo: `executar_agendamento` passa a levantar `TypeError` antes de escrever; os testes continuam verdes porque houve exceção e o banco permaneceu intacto (`app/ferias/test_hj090_red_por_valor.py:77-79,91-93`). Os testes históricos também descartam a resposta e conferem apenas ausência de escrita (`app/ferias/test_hj090_red_por_valor.py:40-42,67-71,82-87,96-98`): uma recusa antecipada por autorização satisfaz essas asserções. Mordida **executada** na base: [nao verificado]. O delta fornecido permite inferir mordida por valor dos casos de saldo na antiga porta, mas não substitui execução.

3. **NAO-BLOQ — redirecionamento convencional pode escolher período incapaz de receber o pedido.** Escolhe o primeiro `p.pode_agendar`, que exige apenas status adquirido e saldo positivo (`app/ferias/views.py:221-228`; `app/ferias/models.py:233-234`). Contra-exemplo: primeiro período adquirido com saldo 10, segundo adquirido com saldo 30, pedido futuro de 30 dias: abre o primeiro formulário com `value=30`, `max=10` (`app/templates/ferias/agendar.html:22`). O período correto não é procurado. Existência desse arranjo entre os 32 casos: [nao verificado].

Curas delimitadas:

- **Triagem 9:** prefill do POST convencional implementado em `app/ferias/views.py:213-228,331-339` e `app/templates/ferias/agendar.html:18,22`. O teste cobre redirect e GET preenchido (`app/ferias/test_hj090_um_juiz.py:445-458`); aceitação dos mesmos 32 casos de produção: [nao verificado].
- **Triagem 10:** `status_de_periodo` consulta a propriedade registrada, inclusive para objetos com os mesmos campos (`app/ferias/models.py:95-98,219-220`; `app/core/juizes.py:264`).
- **Triagem 11, parte procedente:** histórico repassa `hoje` ao status do agendamento e do período (`app/ferias/services.py:451,476`). Outros relógios internos continuam em `app/ferias/services.py:491,510,610,637,700`.
- **Triagem 12:** inclusão dos períodos negativos e agregado separados implementados (`app/ferias/situacao.py:300-302`); caminho Haiku permanece parcial conforme achado 1.
- **Triagem 8:** universo corrigido para `Ausencia(tipo='ferias')`, sem filtro de status, cruzado com `batidas_apuraveis` (`app/ferias/management/commands/listar_ferias_com_batida.py:40-46`); competência atual ordenada primeiro e status impresso (`:61-69`). `--desde` obrigatório remove a data produtiva cravada (`:25`). Correspondência aos 40 dias e quatro de novembro: [nao verificado].

## 13/11 15:17 · sessao 8d553393ec76 · - · - · origem=Ronald (tmux)

- **BLOQUEANTE — triagem 7 meio-curada: contador continua sem caminho ao ESTADO.** O tripwire agora retira `OK` nos dois sentidos e alarma `soma_maior`, mas só imprime (`app/ferias/management/commands/tripwire_status_ferias.py:38–57`). O leitor genérico existe: compara `esperado` com `censo` (`bin/fio_estado.py:125–126`) e imprime qualquer divergência (`bin/fio_estado.py:191–194`); o ESTADO o chama (`bin/gerar_estado.py:260–261`). Porém o produtor desse carimbo não entrega os contadores de férias em `esperado` nem `censo` (`app/chamados/management/commands/vigiar_fio_morto.py:233–241`). **Contra-exemplo:** PA com concedidos 30/soma 33: o tripwire retorna 2, mas a rodada do vigia não acrescenta essa divergência ao carimbo consumido pelo ESTADO. Ocorrência efetiva em produção: [nao verificado].

- **NAO-BLOQ — comando preexistente escreve sem `--apply`, já declarado como fila na triagem.** `alertar_ferias_vencendo` chama `p.sincronizar_status(hoje)` incondicionalmente (`app/ferias/management/commands/alertar_ferias_vencendo.py:14–32`), que delega à porta escritora (`app/ferias/models.py:152–158`). **Contra-exemplo:** executar apenas `alertar_ferias_vencendo` com PA adquirido cujo limite venceu muda seu status, além de emitir notificações (`app/ferias/management/commands/alertar_ferias_vencendo.py:35–53`).

Conferência das listas, sem novo bloqueante demonstrado:

- `listar_ferias_dp` imprime os dois números no sentido concedidos>soma; períodos posteriores incluem origem e status usado, anteriores adquiridos com saldo (`app/ferias/management/commands/listar_ferias_dp.py:55–75`). A origem é inferida de texto livre de `obs`, não de campo estruturado (`app/ferias/management/commands/listar_ferias_dp.py:26–32`); correspondência das 52 linhas reais e separação agregada exigida no RELATO: [nao verificado].
- A simulação não persiste: cancela dentro de transação, pergunta à validação normal e força rollback (`app/ferias/management/commands/listar_ferias_dp.py:98–111`). Aceitação efetiva dos mesmos 32 casos: [nao verificado].
- A lista com batidas usa `Ausencia(tipo='ferias')`, sem filtro de status, cruza `batidas_apuraveis`, imprime status, marca batidas da véspera e prioriza competência atual (`app/ferias/management/commands/listar_ferias_com_batida.py:40–69`). Reprodução dos 40 dias/four casos prioritários do anexo: [nao verificado].
- A data padrão cravada saiu desse comando: `--desde` é obrigatório (`app/ferias/management/commands/listar_ferias_com_batida.py:24–25`).

## 13/11 15:17 · sessao 5f68d2093f96 · - · - · origem=Ronald (tmux)

Achado adicional para consolidar:

- **BLOQUEANTE — edição do direito continua julgando foto antes da trava.** `app/ferias/views.py:844` compara `dias_direito` somente com `dias_concedidos` carregado pela request; `app/ferias/services.py:728` abre atomic, mas `:729-733` grava sem reler/travar nem recalcular status. Contra-exemplo: request lê concedidos=0; outra porta agenda 30 sob trava; edição de direito=20, já aprovada pela foto, grava depois e produz saldo=-10. Mesmo sem corrida, direito30/concedidos20/vendidos10 aceita direito20 e produz saldo=-10: a validação ignora vendidos (`app/ferias/views.py:844`; juiz `app/ferias/models.py:220`). É escritor de **insumo** do saldo fora da trava, embora não escreva diretamente os quatro campos solicitados.

- **NAO-BLOQ, estrutural — exclusão inverte a ordem nominal.** Trava PA e depois apaga ausência e agendamento (`app/ferias/services.py:744`, `:749`; `app/ferias/models.py:456-460`; `app/ponto/services/ausencia.py:1267-1271`), contra a ordem documentada AG→PA→Ausencia (`app/ferias/services.py:557-563`). Deadlock entre portas oficiais **[nao verificado]**: a exclusão só aceita cancelados (`app/ferias/services.py:745-747`) e `_travar_agendamento` recusa cancelado antes de pedir PA (`app/ferias/services.py:561-563`).

Censo de escritores encontrado:

| Campo | Escritores |
|---|---|
| PA.concedidos | realinhar `services.py:156-158`; reverter `:179-181`; histórico `:474-477`; executar `:509-511`; cancelar DP `:577-579`; cancelar admin `:604-606`; trocar janela `:635-651`; aprovar solicitação `:706-708` |
| PA.vendidos | abono integrado `services.py:259-261`; histórico `:475-477`; venda isolada `:806-808` |
| PA.status | Escritores acima por `aplicar_status`; sincronizar `services.py:330-344`; garantir existente `:375-378`; criação por defaults `:369-370` |
| PA.saldo | Property, sem coluna/escritor próprio: `models.py:218-220` |
| AG.status/dias/datas | histórico `services.py:447-473`; executar `:506-508`; aprovar `:695-705`; cancelar `:580-581`, `:612-613`; trocar `:636-649`; cron `:825-829` |
| Ausencia férias | AG.save→espelhar `models.py:449-454`; criar dias `ausencia.py:1203-1208`; remover cancelados `:1189-1193`; apagar janela `:1263-1271`; rejeitar em massa `:1282-1284` |

Leitores centrais:

- Saldo/status: juiz `app/ferias/models.py:98`, `:220`, `:233-242`; validação normal `app/ferias/views.py:250-258`; abono `app/ferias/services.py:294-302`; histórico `:464-472`; remarcação `:631-633`; aprovação `:684-691`; venda `:799-803`.
- Concedidos/status/AG.dias: contador e soma `app/ferias/services.py:105-126`; realinhamento/reversão `:137`, `:152-176`; DP `app/ferias/management/commands/listar_ferias_dp.py:55-84`.
- Tela: `app/templates/ferias/_secao_colab.html:42`, `:49-51`, `:117-120`; `app/templates/ferias/saldo.html:37-46`, `:59-66`.
- Haiku: `app/api/views_mensageria.py:837-844`; situação agrega positivos e negativos separadamente `app/ferias/situacao.py:300-302`.
- Ausencia férias: materializador lê status/datas/dono do PA `app/ponto/services/ausencia.py:1180-1186`; lista com batida lê tipo/data/status `app/ferias/management/commands/listar_ferias_com_batida.py:43-45`, `:68-70`.

Observações de escopo:

- Abono integrado pede a mesma trava PA três vezes (`app/ferias/services.py:245`, `:493`, `:258`), sem mudança da ordem.
- A porta genérica de editar ausência pode alterar férias sem trava PA/AG (`app/ponto/services/ausencia.py:465-509`); mudar estado/reabrir também (`:818-836`, `:1142-1149`). A triagem fornecida separa isso para HJ-091 **[nao verificado]**; não apresentar como novo bloqueio HJ-090.
- `_trilha` estrita cobre realinhamento/reversão (`app/ferias/services.py:161-162`, `:182-183`, `:404-411`). Criação das ausências ainda usa evento best effort (`app/ponto/services/ausencia.py:440-448`; `app/core/observ.py:105`, `:139`); demais portas de saldo não chamam `_trilha` nos seus corpos citados acima.

## 13/11 15:15 · sessao cee2ecca4203 · DESLIGADO-NAO-TRAVA · verificador · origem=Code

**BLOQUEANTE — o RED novo prova transporte de `hoje`, mas deixa passar a volta do relógio interno.** Ele espia `folha.export.informativo_rescisao` e verifica somente os argumentos recebidos; não verifica leituras do relógio nem um resultado dependente da data. (`app/folha/tests/test_lei_desligado_nao_trava.py:132`; `app/folha/tests/test_lei_desligado_nao_trava.py:136`)

**Contraexemplo concreto:** substituir `hoje = hoje or _tz.localdate()` por `hoje = _tz.localdate()`. O spy continua recebendo `06/10`, embora o interior use `25/10`. A fixture tem células `nunca_bateu`; esse ramo não aplica o filtro `d < hoje`. Portanto, esse mutante escapa da asserção nova. Execução do teste Django completo com esse mutante: **[nao verificado]**. (`app/folha/export.py:1238`; `app/folha/tests/test_lei_desligado_nao_trava.py:37`; `app/folha/export.py:1273`; `app/folha/export.py:1294`)

A cura produtiva **existe para o informativo do lavrador**: captura local em `:48`, passagem em `:65`, transporte pelo retrato em `:1163` e pelo helper em `:1225`, chegando ao laço em `:1228`. O RED morde retirar essa passagem; não morde ignorá-la dentro da autoridade. (`app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/management/commands/lavrar_retrato_folha.py:65`; `app/folha/retrato_folha.py:1163`; `app/folha/retrato_folha.py:1225`; `app/folha/export.py:1228`)

**NAO-BLOQ neste delta — a documentação chama de “borda” uma leitura que continua no interior.** O comentário novo atribui ao Haiku uma leitura “na borda dele”, mas `o_que_segura` omite `hoje`; quem lê é `_dias_sem_decisao`. (`app/folha/retrato_folha.py:1211`; `app/folha/fora_da_folha.py:241`; `app/folha/export.py:1238`)

**Contraexemplo concreto:** desligado com vínculo, sem célula em 05/10 e demissão posterior. Uma chamada iniciada em 05/10 às 23:59 que alcance o fallback depois da meia-noite passa a incluir 05/10 pelo teste `d < hoje`. Esse caminho continua aberto; a injeção do lavrador não o alcança. (`app/folha/fora_da_folha.py:241`; `app/folha/export.py:1238`; `app/folha/export.py:1271`)

| Caminho | Quem lê o relógio; relação com o informativo |
|---|---|
| Lavrador | Lê uma data local e a injeta até `informativo_rescisao`. (`app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/retrato_folha.py:1225`) |
| `montar_linhas` avulso | Aceita `hoje=None`; nessa modalidade cada desligado elegível pode chegar ao fallback interno. (`app/folha/retrato_folha.py:1120`; `app/folha/retrato_folha.py:1225`; `app/folha/export.py:1238`) |
| Tela e drawer | Leem o retrato persistido. O relógio da tela escolhe competência; não recalcula o informativo. (`app/folha/views_retrato.py:176`; `app/folha/views_retrato.py:190`; `app/folha/views_retrato.py:196`; `app/colaboradores/views.py:984`; `app/templates/folha/_retrato_o_que_fazer.html:60`) |
| Haiku `o_que_segura` | Omite `hoje`; leitura efetiva no interior do export. (`app/folha/fora_da_folha.py:241`; `app/folha/export.py:1238`) |
| Vigia | Captura data local, mas segue por `dias_que_seguram` e retenção, sem chamar o informativo. Também não transporta seu `hoje` até esse retentor. (`app/chamados/management/commands/vigiar_fio_morto.py:169`; `app/chamados/management/commands/vigiar_fio_morto.py:178`; `app/chamados/services/vigia_fio.py:586`; `app/folha/fora_da_folha.py:98`) |

Os relógios identificados nesses caminhos usam `localtime(...).date()` ou `localdate()`, com configuração da cópia `America/Sao_Paulo`, `USE_TZ=True`. **Não há divergência UTC demonstrada no `hoje` do informativo.** Configuração efetiva de produção: **[nao verificado]**. (`app/config/settings/base.py:93`; `app/config/settings/base.py:95`; `app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/export.py:1238`)

**NAO-BLOQ — o cabeçalho da bolha omite diferenças entre agendamento e consumação.** “Agendado só grava na data” vale para o valor `situacao='desligado'`; não vale para `data_demissao` e motivo, gravados antecipadamente. A saída “encerra vínculo e abre chamado DP” também precisa distinguir o ramo agendado. (`app/colaboradores/services/desligamento.py:9`; `app/colaboradores/services/desligamento.py:11`; `app/colaboradores/services/desligamento.py:110`)

**Contraexemplo concreto:** agendar amanhã grava data/motivo hoje e retorna antes do encerramento de vínculo e da criação do chamado. O cron depois chama o mesmo executor; não constitui segundo escritor do valor `desligado`. (`app/colaboradores/services/desligamento.py:107`; `app/colaboradores/services/desligamento.py:126`; `app/colaboradores/services/desligamento.py:148`; `app/colaboradores/services/desligamento.py:188`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`)

`esta_desligado` corresponde ao predicado inscrito: lê somente `situacao`. Já `julgar_desligamento` recebe POST e lê `date.today()` internamente; “para o juiz, só o colaborador” só descreve corretamente o primeiro. O selo de mapa cobra presença das etiquetas, sem demonstrar a verdade dessas descrições. (`app/colaboradores/services/desligamento.py:7`; `app/colaboradores/services/desligamento.py:32`; `app/colaboradores/services/desligamento.py:35`; `app/colaboradores/services/desligamento.py:60`; `app/core/tests/test_selo_mapa_contrato.py:41`; `app/core/tests/test_selo_mapa_contrato.py:60`)

**NAO-BLOQ — o rótulo de consultas foi corrigido, mas a documentação atribui a segunda consulta à fonte errada.** O teste agora anuncia corretamente **uma chamada** ao helper mockado. O helper continua consultando colaboradores e, por desligado elegível, células e **vínculos**, não “férias/ausências”. (`app/folha/tests/test_retrato_folha_quadros.py:1345`; `app/folha/tests/test_retrato_folha_quadros.py:1351`; `app/folha/retrato_folha.py:1210`; `app/folha/retrato_folha.py:1222`; `app/folha/export.py:1247`; `app/folha/export.py:1253`)

**Contraexemplo concreto:** um desligado sem qualquer férias/ausência ainda percorre as consultas de `CelulaDia` e `EscalaColaborador`. O custo `1+2D` permanece para essas consultas-base; redução do N+1 não foi implementada nesse caminho. Latência produtiva: **[nao verificado]**. (`app/folha/export.py:1247`; `app/folha/export.py:1253`; `app/escala/alimentacao.py:148`; `app/escala/alimentacao.py:186`)

**D1 tem mordida para a regressão caracterizada.** A expectativa nova `[]` é efetivamente comparada: `retencao` integra `APURACAO`, `_medir` lê `fm.motivos_espelho` e `_confere_barrado` compara essa chave. Restaurar `['sem_celula']` nesse resultado falha contra as duas expectativas novas. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:52`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:329`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:361`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:644`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:675`)

A cadeia passa pelo recálculo real, que escreve `motivos_espelho` usando o wrapper do retentor. Isso cobre a retenção materializada do caso D1; execução nesta revisão: **[nao verificado]**. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:604`; `app/ponto/services/fechamento.py:1067`; `app/folha/export.py:1307`)

Os escritores, leitores e significados relevantes ficam assim:

| Campo/valor | Escritor → leitor; significado |
|---|---|
| `hoje` | Lavrador → retrato → informativo → laço; data civil usada nos ramos de célula ausente ou sem veredito. Há fallback interno quando omitido. (`app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/retrato_folha.py:1225`; `app/folha/export.py:1238`; `app/folha/export.py:1295`) |
| `informativo` | Export produz dias; helper converte para ISO; `montar_linhas` monta a chave; lavrador persiste; drawer lê. Não se confunde com `datas`/`dias` de retenção. (`app/folha/export.py:1228`; `app/folha/retrato_folha.py:1225`; `app/folha/retrato_folha.py:1193`; `app/folha/management/commands/lavrar_retrato_folha.py:76`; `app/templates/folha/_retrato_o_que_fazer.html:60`) |
| `situacao` | Executor grava `desligado` pela porta; juiz lê. Reativação grava `ativo`: unicidade do escritor do **valor desligado** não significa unicidade de escritor do campo inteiro. (`app/colaboradores/services/desligamento.py:129`; `app/colaboradores/services/desligamento.py:133`; `app/colaboradores/services/desligamento.py:32`; `app/colaboradores/services/desligamento.py:235`) |
| `data_demissao` / motivo | Executor grava antecipadamente ou na consumação; cron lê o agendamento; export lê a data para rescisão e recorte. **A data representa intenção futura ou desligamento consumado**, distinguíveis pela situação. (`app/colaboradores/services/desligamento.py:110`; `app/colaboradores/services/desligamento.py:130`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`; `app/folha/export.py:60`; `app/folha/export.py:1241`) |
| `retencao` de D1 | Fechamento escreve `motivos_espelho`; teste o copia para `retencao` e compara com `MEDIDO`. É lista de motivos, não quantidade de dias. (`app/ponto/services/fechamento.py:1067`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:644`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:675`; `app/folha/export.py:1298`) |
| `desligados_retidos` | Fonte produz lista de ids; vigia agrega seu comprimento por empresa/competência. Há mudança explícita de lista para inteiro; não é contagem global de pessoas únicas. (`app/folha/fora_da_folha.py:144`; `app/folha/fora_da_folha.py:146`; `app/chamados/services/vigia_fio.py:584`; `app/chamados/services/vigia_fio.py:589`) |

O Raio-X **continua sem cura**: seleciona pelo export, recorta a demissão e reconstrói dias com `furos_por_dia`; o preview continua contando motivos como dias. Contraexemplo: três dias com um mesmo motivo aparecem como `1` nesse preview. São resíduos, não novas derivações introduzidas pela injeção de `hoje`. (`app/folha/views.py:591`; `app/folha/views.py:612`; `app/folha/views.py:616`; `app/folha/views.py:398`; `app/folha/export.py:1298`)

No código produtivo acrescentado, não identifiquei segundo juiz ou segundo escritor fora da porta: permanece a delegação ao mesmo laço e a persistência pelo lavrador. A classe de reprovação nova demonstrada é **RED sem mordida para a propriedade anunciada**. Suíte integral, opus, pouso e medição produtiva de competência 11: **[nao verificado]**. (`app/folha/retrato_folha.py:1225`; `app/folha/export.py:1228`; `app/folha/management/commands/lavrar_retrato_folha.py:74`; `app/folha/tests/test_lei_desligado_nao_trava.py:136`)

## 13/11 15:15 · sessao 049bf09ab549 · - · - · origem=Ronald (tmux)

Contagem transitiva confirmada: em execução normal com `dias` não vazio, **`alimentar_fila = 2 + b + j`**. As duas cargas principais chamam por `alimentar_portas` e pela primeira `CargaDaTarefa.tarefa`; `b` indica a alimentação extra de `beco_em_lote` quando a lista é carregada e encontra células; `j` é o número de janelas distintas `(ini,fim)` com perguntas vivas usadas nas cargas extras da tarefa. Sem dias, retorna antes das duas cargas. Referências: `app/chamados/services/vigia_fio.py:594`, `app/chamados/juizes.py:1594`, `app/chamados/services/tarefa_do_dia.py:529`, `app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:486`, `app/escala/services/cadastro_realidade.py:488`, `app/escala/services/cadastro_realidade.py:492`, `app/escala/services/cadastro_realidade.py:495`.

A lista chama `_folga_nao_definida_sem_porta` uma vez; ela reúne todas as janelas e chama `beco_em_lote` uma vez. A `ListaDoAdmin` já carregada não repete essa execução. Referências: `app/escala/services/cadastro_realidade.py:873`, `app/escala/services/cadastro_realidade.py:574`, `app/escala/services/cadastro_realidade.py:582`, `app/chamados/services/vigia_fio.py:205`.

Cada `alimentar_fila` também consulta linhas de `PerguntaDisputa` pelos créditos: destino em vivos OU chamado da disputa em vivos; acrescenta `q_vivas()` e exclui materializada com `VL_SIM`; lê cinco campos com `values_list`, sem materializar objetos de `PerguntaDisputa`/`Colaborador`. Referências: `app/chamados/services/verbos.py:34`, `app/chamados/services/raiox.py:56`, `app/chamados/services/raiox.py:74`, `app/chamados/services/raiox.py:135`.

Cargas transitivas adicionais de `PerguntaDisputa`:

- Por bloco de datas e colaborador sem porta em dia passado, calendário consulta `disputa__colaborador=colaborador`, `data_evento__range=(data_ini,data_fim)`, exclui `resposta_colab=''`, com `select_related('disputa__colaborador','celula','chamado_destino')`. Julga `_pv(_q)` sem `agora`. O JOIN também materializa `Colaborador`, embora o mapa depois imponha a instância semeada. Referências: `app/chamados/services/vigia_fio.py:642`, `app/chamados/services/vigia_fio.py:266`, `app/colaboradores/services/calendario.py:128`, `app/colaboradores/services/calendario.py:472`.
- Cada alimentação extra do beco repete o sítio de perguntas A; cada carga extra de tarefa repete o sítio B. Referências: `app/escala/services/cadastro_realidade.py:486`, `app/escala/services/cadastro_realidade.py:492`.

Cargas transitivas de `Colaborador`, além do núcleo A/B:

| Sítio | Filtro / quantidade |
|---|---|
| `app/folha/export.py:785` | Por empresa×competência: `FechamentoMensal.filter(colaborador__empresa=empresa,mes=mes,ano=ano).select_related('colaborador')`, chamado por `dias_que_seguram` em `app/folha/fora_da_folha.py:130`. |
| `app/folha/fora_da_folha.py:142` | Outra carga por empresa×competência: `Colaborador.filter(pk__in=_ids_furo).select_related('empresa')`. `_ids_furo` sai dos mesmos itens do export, portanto esse conjunto é subconjunto do anterior (`app/folha/fora_da_folha.py:133`). |
| `app/colaboradores/services/calendario.py:474` | JOIN descrito acima, uma carga por bloco; colaborador já conhecido e semeado em `app/colaboradores/services/calendario.py:475`. |
| `app/escala/services/cadastro_realidade.py:485` | Se beco encontra células: `Colaborador.filter(pk__in={ids das células}).select_related('empresa')`. |
| `app/escala/services/cadastro_realidade.py:612` | `_afastado_sem_registro`: queryset `exclude(situacao='desligado')`, opcional empresa; lê `pk` com `situacao='afastado'` em `:616`. O mesmo queryset entra como subquery de ausência em `:615`, por `app/colaboradores/services/situacao_exibida.py:44` e `app/ponto/turnos.py:2715`. |
| `app/escala/services/cadastro_realidade.py:624` | Se houver afastados: `filter(pk__in=pks).select_related('empresa').order_by('pk')`. |
| `app/escala/services/cadastro_realidade.py:185` | Por empresa, lê somente PKs de `filter(pk__in=ids de células nunca_bateu,situacao='desligado')`; loop em `:179`, células em `:181`. |
| `app/escala/services/cadastro_realidade.py:777` | `_vinculos`: `filter(pk__in=ids).select_related('empresa')`, onde IDs são lavra ∪ cadastro (`:895–900`); filtro de empresa ocorre depois da carga (`:907–909`). |
| `app/escala/services/cadastro_realidade.py:1025` | `select_related('empresa').get(pk=cid)`, uma vez por colaborador no memo local; apenas donos divergentes sem C11 utilizável (`:1015–1024`). |
| `app/ponto/supra_juiz.py:177` | Leitura escalar `Colaborador.filter(pk=...).values_list('data_demissao').first()` por dia sem vínculo, alcançada por `um_dono_por_dia → InsumosDoDono.dono`; condição em `:174`, chamada em `app/ponto/management/commands/e6_oraculo.py:183`. Beco fornece cadastro e evita essa rota (`app/escala/services/cadastro_realidade.py:534`). |

Não encontrei outras consultas de `PerguntaDisputa` nas rotas de lista/`InsumosDoDono` inspecionadas; `InsumosDoDono` carrega batidas, turnos, lavras e cadastro, com `sem_motor=True` impedindo chamada ao espelho. Referências: `app/ponto/management/commands/e6_oraculo.py:130`, `app/ponto/management/commands/e6_oraculo.py:156`, `app/ponto/management/commands/e6_oraculo.py:162`, `app/ponto/management/commands/e6_oraculo.py:174`, `app/escala/services/cadastro_realidade.py:1033`.

Quantidade efetivamente executada, cardinalidades e interseções concretas em produção: **[nao verificado]**.

## 13/11 15:15 · sessao c1cfee3aaef7 · - · - · origem=Ronald (tmux)

Levantamento de A e leitores, somente código.

- `alimentar_fila` escreve `verbo` com `particionar_verbos(qs_vivos(), agora=agora)['mapa']`; consulta a fila global com `qs_na_fila(agora=agora).values_list('pk','celula_id')`; descarta pks ausentes de `verbo`; `fila_ids` permanece global, e somente `fila_cel` é recortado pelos ids recebidos. `app/chamados/juizes.py:1557`, `app/chamados/juizes.py:1558`, `app/chamados/juizes.py:1561`, `app/chamados/juizes.py:1562`, `app/chamados/juizes.py:1564`, `app/chamados/juizes.py:1565`, `app/chamados/juizes.py:1567`.
- A fonte `qs_vivos` usa `ChamadoColaborador`, sem filtros opcionais nas chamadas acima, com `status_local__in=VIVOS`; a base declara `select_related('colaborador','colaborador__empresa','colaborador__posto','respondido_por')` e `prefetch_related('followups')`; a partição utiliza projeção por `values_list` e remove prefetch. `app/chamados/services/painel.py:98`, `app/chamados/services/painel.py:100`, `app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:155`, `app/chamados/services/verbos.py:44`.
- A fila acrescenta `status_local__in=COBRANCA` e exclui `silenciado_ate__gt=agora` (ou `timezone.now()` se `None`). `app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:189`.
- `verbo` também depende de créditos de perguntas: `PerguntaDisputa.filter(Q(chamado_destino__in=vivo)|Q(disputa__chamado__in=vivo))`, `q_vivas()`, exclusão de `materializada_em` preenchido com `veredito_lavrado=VL_SIM`, projeção de cinco campos; não materializa `Colaborador` nessa projeção. `app/chamados/services/verbos.py:34`, `app/chamados/services/raiox.py:56`, `app/chamados/services/raiox.py:74`, `app/chamados/services/raiox.py:135`.
- `alimentar_portas` escreve `vivas` lendo `PerguntaDisputa.filter(celula_id__in=_ids).select_related('disputa','celula').order_by('pk')`, passando por identity map e `pergunta_viva(p, agora=agora)` em TODAS as linhas carregadas. `app/chamados/juizes.py:1599`, `app/chamados/juizes.py:1600`, `app/chamados/juizes.py:1603`, `app/chamados/juizes.py:1604`.
- Escreve `instancias` inicialmente de `colabs`; `colab` vem de `CelulaDia.filter(pk__in=_ids).values_list('pk','colaborador_id')`; inicia `no_app={}`. `app/chamados/juizes.py:1598`, `app/chamados/juizes.py:1606`, `app/chamados/juizes.py:1607`.

Leituras no censo:

| Chave | Leituras e destino |
|---|---|
| `fila_ids` | Alias `fila_pk` em `app/chamados/services/vigia_fio.py:609`; pertença da âncora na formação do véu em `app/chamados/services/vigia_fio.py:674`. |
| `verbo` | Não há leitura no tronco atual de `censo_alcance`: carga A em `app/chamados/services/vigia_fio.py:608`, e porta B em `app/chamados/services/vigia_fio.py:640`, `app/chamados/services/vigia_fio.py:641`. Ausência global de outros acessos dinâmicos: [não verificado]. |
| `fila_cel` | Mesma situação de `verbo`; a porta do censo vem de B em `app/chamados/services/vigia_fio.py:641`. |
| `vivas` | Alias `perg` em `app/chamados/services/vigia_fio.py:614`; âncoras para carregar status em `app/chamados/services/vigia_fio.py:621`; `ps` do dia em `app/chamados/services/vigia_fio.py:646`; estados de âncoras/véu em `app/chamados/services/vigia_fio.py:670`, `app/chamados/services/vigia_fio.py:673`; causas e detalhes em `app/chamados/services/vigia_fio.py:682`, `app/chamados/services/vigia_fio.py:684`, `app/chamados/services/vigia_fio.py:686`, `app/chamados/services/vigia_fio.py:690`, `app/chamados/services/vigia_fio.py:694`, `app/chamados/services/vigia_fio.py:698`, `app/chamados/services/vigia_fio.py:700`. |
| `colab` | Não é usado pelo wrapper `no_app`: ele passa diretamente o `cid` recebido a `_no_app`, em `app/chamados/services/vigia_fio.py:624`, `app/chamados/services/vigia_fio.py:625`. |
| `no_app` | `_no_app` lê presença/cache e retorna conjunto em `app/chamados/juizes.py:1613`, `app/chamados/juizes.py:1621`; o censo consulta pertença de `p.pk` em `app/chamados/services/vigia_fio.py:700`. |
| `instancias` | Segundo carregamento de perguntas (`perg_todas`) compartilha mapa em `app/chamados/services/vigia_fio.py:618`, `app/chamados/services/vigia_fio.py:619`; `_no_app` usa mapa em `app/chamados/juizes.py:1616`. |

`perg_todas` é variável, não função: repete a query por `celula_id__in=_cels`, `select_related('disputa','celula')`, sem filtro de vitalidade; passa o conjunto ao `dono_fora_do_fio`. `app/chamados/services/vigia_fio.py:617`, `app/chamados/services/vigia_fio.py:618`, `app/chamados/services/vigia_fio.py:662`, `app/chamados/services/vigia_fio.py:663`.

`dono_fora_do_fio` usa essas perguntas em `p.estado == VALIDADA` e `_e_aceite_de_folga(p)`; não recebe o dicionário `alim`. `app/chamados/services/vigia_fio.py:274`, `app/chamados/services/vigia_fio.py:311`. `p.estado` chama `pergunta_viva(self)` sem `agora` se `validada_em` é nulo; quando preenchido, retorna antes. `app/chamados/models.py:3067`, `app/chamados/models.py:3070`.

`respondida_sem_texto` lê `respondida_em` e `resposta_colab`; `chamado_que_lista` lê `chamado_destino_id`, ou `disputa.chamado_id`. `app/chamados/juizes.py:1518`, `app/chamados/services/fio.py:116`, `app/chamados/services/fio.py:117`.

`_no_app` pode escrever tanto `instancias` quanto `no_app`; busca `Colaborador.filter(pk=colab_id).first()` somente se id não estiver no mapa; escreve os ids devolvidos por `perguntas_do_app(_c)` sem passar `agora`. `app/chamados/juizes.py:1617`, `app/chamados/juizes.py:1618`, `app/chamados/juizes.py:1620`. `perguntas_do_app` consulta por `disputa__colaborador=colab`, aplica `q_vivas()`, `select_related('disputa','celula')`, `order_by('id')`, e julga `pergunta_viva(p)` sem argumentos temporais/carga. `app/chamados/services/pergunta_no_app.py:49`, `app/chamados/services/pergunta_no_app.py:50`, `app/chamados/services/pergunta_no_app.py:56`, `app/chamados/services/pergunta_no_app.py:58`.

A função de identity map não faz consulta explícita: troca o colaborador pela instância existente; após o consumidor, acrescenta instância que ele tenha carregado. `app/chamados/juizes.py:1179`, `app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1183`, `app/chamados/juizes.py:1185`.

Cuidado com a contagem por rodada: duas chamadas `alimentar_fila` são as do tronco A/B com universo não vazio: A em `app/chamados/juizes.py:1594`; B preguiçosa, uma vez por carga, em `app/chamados/services/tarefa_do_dia.py:529`, `app/chamados/services/tarefa_do_dia.py:531`. Contudo, a expansão transitiva de `ListaDoAdmin` pode acrescentar outras:

1. `ListaDoAdmin` carrega uma vez se ainda não carregada, chamando `cadastro_realidade.lista`. `app/chamados/services/vigia_fio.py:205`, `app/chamados/services/vigia_fio.py:208`, `app/chamados/services/vigia_fio.py:216`.
2. `lista` chama `_folga_nao_definida_sem_porta`, que chama `beco_em_lote` uma vez se há janelas. `app/escala/services/cadastro_realidade.py:873`, `app/escala/services/cadastro_realidade.py:582`.
3. `beco_em_lote` sai se não há células; caso contrário chama outro `alimentar_portas` e monta uma carga B por janela distinta com perguntas vivas, consultando tarefas dessas janelas. Em execução completa isso acrescenta `1+J` chamadas de `alimentar_fila`, onde `J` é o número dessas janelas. `app/escala/services/cadastro_realidade.py:483`, `app/escala/services/cadastro_realidade.py:486`, `app/escala/services/cadastro_realidade.py:488`, `app/escala/services/cadastro_realidade.py:492`, `app/escala/services/cadastro_realidade.py:496`, `app/escala/services/cadastro_realidade.py:497`.
4. As células adicionais são ativas, `veredito='indefinida'`, protesto `PREVISTO_COM_DIA_INDEFINIDO` e dentro das janelas; seus colaboradores são carregados por pk com `select_related('empresa')`. `app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:479`, `app/escala/services/cadastro_realidade.py:480`, `app/escala/services/cadastro_realidade.py:485`.
5. O censo cria a lista default SEM `agora`; a lista nesse caminho acaba lendo `timezone.now()`. Objeto fornecido pelo chamador pode trazer outro estado/instante, não verificável sem execução. `app/chamados/services/vigia_fio.py:627`, `app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:216`, `app/escala/services/cadastro_realidade.py:803`.

Outra carga condicional de perguntas/colaborador no grafo do censo é a do calendário: `_resolver_da_rodada` chama `resolver_dia_oferecido_nas_datas`, que monta calendários em blocos até 366 dias; consulta `PerguntaDisputa` por `disputa__colaborador=colaborador`, `data_evento__range`, exclui `resposta_colab=''`, faz `select_related('disputa__colaborador','celula','chamado_destino')`, e julga `_pv(_q)` sem `agora`. `app/chamados/services/vigia_fio.py:270`, `app/chamados/services/vigia_fio.py:642`, `app/colaboradores/services/calendario.py:114`, `app/colaboradores/services/calendario.py:129`, `app/colaboradores/services/calendario.py:132`, `app/colaboradores/services/calendario.py:473`, `app/colaboradores/services/calendario.py:474`, `app/colaboradores/services/calendario.py:476`.

Outros consumidores executáveis localizados fora dos testes: [nao verificado]

- `cadastro_realidade.beco_em_lote`: chama A e lê apenas `vivas` diretamente. `app/escala/services/cadastro_realidade.py:486`, `app/escala/services/cadastro_realidade.py:490`, `app/escala/services/cadastro_realidade.py:496`.
- `juizes.porta_alcancavel_do_dia`: chama A se não recebeu alimentação, e lê `fila_cel`, `verbo`, `vivas`, `fila_ids`, `colab`, além de `no_app`/`instancias` via `_no_app`. `app/chamados/juizes.py:1676`, `app/chamados/juizes.py:1677`, `app/chamados/juizes.py:1678`, `app/chamados/juizes.py:1679`, `app/chamados/juizes.py:1681`, `app/chamados/juizes.py:1682`, `app/chamados/juizes.py:1686`.
- Comando de diagnóstico `diff_e3_sombra.Command.comparar`: chama A, passa à porta C17 e a `casos_do_par`; esta última lê `fila_cel`, `vivas`, `fila_ids`. `app/chamados/management/commands/diff_e3_sombra.py:116`, `app/chamados/management/commands/diff_e3_sombra.py:127`, `app/chamados/management/commands/diff_e3_sombra.py:131`, `app/chamados/management/commands/diff_e3_sombra.py:64`, `app/chamados/management/commands/diff_e3_sombra.py:65`, `app/chamados/management/commands/diff_e3_sombra.py:66`.

Busca textual em todo repositório não localizou outra chamada executável C17 externa ao comando de diagnóstico; resolução/importação dinâmica não foi demonstrada [não verificado].

## 13/11 15:13 · sessao 4e7d28618bda · - · - · origem=Ronald (tmux)

Auditoria de B, somente pelo código:

- **Entrada e universo:** `censo_alcance` abre B com `list(colabs.values())`, menor e maior data do conjunto do censo e o mesmo `agora`; depois chama `carga.tarefa(...)` para cada par do censo (`app/chamados/services/vigia_fio.py:639-641`). `CargaDaJanela` guarda esses colaboradores em `_colabs`, carrega células da janela com padding e as organiza em `_cels`; não recarrega `Colaborador` nessa base (`app/ponto/turnos.py:2973-2992`).
- **Recarga de cadastro:** B executa `Colaborador.objects.select_related('empresa').in_bulk(ids)` se houver IDs, mesmo recebendo instâncias já carregadas (`app/chamados/services/tarefa_do_dia.py:370-372`). B também carrega colaboradores por JOIN na query de perguntas, por `select_related('disputa__colaborador__empresa', ...)` (`app/chamados/services/tarefa_do_dia.py:376-380`).
- **Perguntas de B:** uma query de `PerguntaDisputa` com `colaborador_id__in=ids`, `conversa=False`, `data_evento__gte=self.ini`, `data_evento__lte=self.fim`; `select_related('disputa__colaborador__empresa', 'celula', 'chamado_destino')`; `order_by('colaborador_id', 'pk')`; agrupa em `_perg[(colaborador_id,data_evento)]` (`app/chamados/services/tarefa_do_dia.py:376-383`). A lê por `celula_id__in=_ids`, sem filtro de conversa/data, `select_related('disputa','celula')` e ordem PK (`app/chamados/juizes.py:1599-1604`). Os universos podem se sobrepor, mas não são definidos pelo mesmo predicado (mesmas referências).
- **C6 em B:** `_ler_vida` seleciona candidatas por grade, isenção, tipo do dia, tranca, exportação e exposição fora dos marcos normais; em dia de trabalho cujas perguntas estão todas nos marcos da grade, pula C6; quando necessário, seleciona todas as perguntas daquele dia (`app/chamados/services/tarefa_do_dia.py:467-488`). Semeia `p.disputa.colaborador` com `_cadastro[cid]`, cria `carga_da_vida(self,candidatas)` e chama `pergunta_viva(p, hoje=_hoje(self.agora), carga=carga, agora=self.agora)` apenas para candidatas (`app/chamados/services/tarefa_do_dia.py:481-493`).
- **Comparação de relógio:** A chama `pergunta_viva(p, agora=agora)` para todas as perguntas carregadas (`app/chamados/juizes.py:1600-1604`). O wrapper transforma `hoje=None` em `_hoje(agora)` se houver `agora`, portanto não é correto dizer que A e B necessariamente usam dias civis diferentes; os argumentos/caminhos diferem porque B fornece carga, que desvia do memo avulso (`app/chamados/juizes.py:883-894`). `CargaDaVida` reutiliza o cadastro e a tranca de B; não faz outra query de `PerguntaDisputa` nem de `Colaborador` no próprio corpo (`app/chamados/services/carga_pergunta_viva.py:77-112`).
- **Fila preliminar de B:** além de `alimentar_fila`, o construtor chama `qs_na_fila(_base=ChamadoColaborador.objects.filter(pk__in=ancoras), agora=agora)` para as âncoras das perguntas de B e guarda `_fila` (`app/chamados/services/tarefa_do_dia.py:415-418`). Isso é distinto da alimentação global de portas (mesmas referências; `app/chamados/juizes.py:1558-1567`).
- **Segunda chamada de `alimentar_fila`:** primeira `tarefa()` inicia `_portas` com todas as células de `_cels` dentro de `[ini,fim]`, usando `agora=self.agora`; chamadas seguintes reutilizam `_portas` (`app/chamados/services/tarefa_do_dia.py:427`, `app/chamados/services/tarefa_do_dia.py:529-534`). A chama com `_cels` dos pares exatos do censo (`app/chamados/services/vigia_fio.py:600-608`). Assim os argumentos de célula diferem: B pode incluir dias intermediários de todos os colaboradores; A só células dos pares enumerados (mesmas referências).
- **O recorte de célula só afeta `fila_cel`:** `alimentar_fila` sempre particiona `qs_vivos()` global com `agora`, sempre consulta `qs_na_fila(agora=agora)` global para `fila_ids`, e apenas testa `cel_id in _ids` ao montar `fila_cel` (`app/chamados/juizes.py:1557-1567`). Logo, mesmo estado do banco e mesmo instante implicam mesma definição para `verbo`/`fila_ids`; diferentes listas de célula não tornam esses dois conjuntos recortados (mesma referência). Há leituras separadas e o próprio código trata corrida entre elas; ocorrência real de divergência em produção: **[não verificado]** (`app/chamados/juizes.py:1561-1563`).
- **Autoridade/query de fila:** `qs_vivos()` usa `montar_qs_filtrado(...)` e filtra `status_local__in=VIVOS` (`app/chamados/services/painel.py:153-155`); a base tem `select_related('colaborador','colaborador__empresa','colaborador__posto','respondido_por')`, prefetch followups e anotações/ordenação (`app/chamados/services/painel.py:98-115`). `qs_na_fila` acrescenta `status_local__in=COBRANCA` e exclui `silenciado_ate__gt=agora` (`app/chamados/services/painel.py:188-190`). A partição usa `values_list` e remove prefetch, não materializa os colaboradores desses JOINs (`app/chamados/services/verbos.py:44-46`).
- **Leitura adicional de perguntas por chamada de `alimentar_fila`:** `particionar_verbos` chama `creditos_perguntas(vivo_qs)` (`app/chamados/services/verbos.py:34`). Este lê `PerguntaDisputa` cujo destino ou chamado da disputa está no universo vivo, aplica `q_vivas()`, exclui `materializada_em__isnull=False` junto com `veredito_lavrado=VL_SIM`, e materializa `values_list('chamado_destino_id','disputa_id','disputa__chamado_id','respondida_em','resposta_colab')`; sem `select_related`, sem conversa/data/célula e sem chamada Python de C6 (`app/chamados/services/raiox.py:56-58`, `app/chamados/services/raiox.py:74`, `app/chamados/services/raiox.py:135-144`). Portanto são duas execuções dessa leitura no caminho normal com dias, uma por cada `alimentar_fila` (mesmas referências; `app/chamados/juizes.py:1594`; `app/chamados/services/tarefa_do_dia.py:529-533`).
- **API de B e equivalência das sete chaves:** B guarda os três dados de fila no atributo privado `_portas`; `_portas_da_tarefa` lê `fila_ids`, `verbo`, `fila_cel`, intersecta com portas inteiras das ações exigíveis e publica `rota`, `fila`, `verbos`, `fio` (`app/chamados/services/tarefa_do_dia.py:529-556`). `porta_da_tarefa` retorna somente esses quatro campos quando existe rota (`app/chamados/services/tarefa_do_dia.py:559-565`). B não oferece um equivalente público de `alim` com `vivas`, `colab`, `no_app`, `instancias`; `_perg` não é o conjunto `vivas`, `_cadastro`/`_colabs` não são o mesmo identity map, e as `instancias` públicas de tarefa representam marcos, não instâncias ORM de colaboradores (`app/chamados/services/tarefa_do_dia.py:372-383`, `app/chamados/services/tarefa_do_dia.py:511-534`, `app/chamados/services/tarefa_do_dia.py:184-185`; `app/chamados/juizes.py:1595-1607`).

## 13/11 15:13 · sessao 25297c2a7a00 · BUG-PAR · verificador · origem=Code

**BLOQUEANTE: o delta ainda contém regressão de retorno, divergências tela/porta e caminho que pode regravar competência exportada.** (`app/chamados/services/materializacao.py:171`, `app/chamados/services/materializacao.py:1113`, `app/chamados/services/materializacao.py:686`, `app/ponto/services/fechamento.py:1160`)

Os contraexemplos abaixo são construídos por leitura do código; sua ocorrência em produção, a execução da suíte e a evidência RED/GREEN/mutante permanecem **[não verificado]**. [nao verificado]

1. **BLOQUEANTE — `None` significa tanto “fora do intervalo” quanto “irmã gravada”.**

   Para motivo fora do intervalo, `par_do_intervalo` retorna `(None, None, None)`; `_gravar_irma_do_par` repassa esse último `None`. O chamador interpreta isso como sucesso e afirma que a outra metade gravou. (`app/chamados/services/materializacao.py:171`, `app/chamados/services/materializacao.py:1371`, `app/chamados/services/materializacao.py:1113`)

   **Contraexemplo:** pergunta `orfao_14h`, vizinha do mesmo tipo, sem absolvição por preenchimento de furo, lâmpada acesa. Nenhuma irmã existe ou grava; mesmo assim ocorre `_Reprovada` com “a outra metade do intervalo gravou”, antes do ramo que encerraria pela via `paridade_recusada`. Isso também deixa o item 4 parcialmente descoberto: o texto continua podendo dizer algo que não aconteceu. (`app/chamados/services/materializacao.py:1116`, `app/chamados/services/materializacao.py:1146`, `app/chamados/services/materializacao.py:1170`)

2. **BLOQUEANTE — a função compartilhada ainda não reúne os impedimentos da porta.**

   `estado_aplicacao_pergunta` só consulta `par_do_intervalo` quando encontra conflito de paridade sem absolvição por furo; fora desse ramo retorna `aplicavel`. A porta aplica `DATA_EM_DISPUTA` antes de qualquer ramo de materialização. (`app/chamados/services/materializacao.py:674`, `app/chamados/services/materializacao.py:677`, `app/chamados/services/materializacao.py:686`, `app/chamados/services/materializacao.py:1481`)

   **Contraexemplo:** chão E18:30, S01:00, S06:30; pergunta de volta E02:00 com dia divergente do chamado e `data_conferida_em=None`. Isoladamente a volta alterna corretamente: a tela promete gravação, mas a porta recusa pela data. Portanto, o item 3 não está integralmente curado. (`app/chamados/services/materializacao.py:673`, `app/chamados/services/materializacao.py:686`, `app/chamados/services/materializacao.py:1481`)

   Existe ainda impedimento de competência no escritor que a elegibilidade não consulta: o plantio passa `fato_de_chao=False`; `registrar_batida` recusa fechamento aprovado/exportado. **Contraexemplo:** par que fecha a paridade em competência cujo `FechamentoMensal.status='exportado'`: tela aplicável, clique recusado. (`app/chamados/services/materializacao.py:157`, `app/chamados/services/materializacao.py:1173`, `app/ponto/registro_batida.py:132`, `app/ponto/janelas.py:87`)

3. **BLOQUEANTE — validar pode tocar o gravado da competência 10 exportada, mesmo recusando o plantio.**

   A validação agenda recálculo antes de tentar materializar; o callback chama o escritor mensal, cujo ramo de fechamento não aberto atualiza horas e `atualizado_em`. A guarda temporária cobre somente 09/2026. (`app/chamados/services/validacao.py:543`, `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1160`, `app/ponto/services/fechamento.py:36`)

   **Contraexemplo:** pergunta devolvida pela porta 103, de competência 10 exportada, para colaborador incluído no universo do cálculo. O admin valida; a batida recusa pela competência, mas o recálculo já agendado executa após o commit. Esse caminho não passa pela geração de Pauta DP do lote. Mudança numérica efetiva em produção: **[não verificado]**. (`app/chamados/services/validacao.py:545`, `app/chamados/services/validacao.py:564`, `app/ponto/registro_batida.py:135`, `app/ponto/services/fechamento.py:1824`, `app/chamados/services/passivo_validado.py:470`)

4. **BLOQUEANTE — a ordem global foi adicionada ao lote, mas continua ausente no laço da disputa.**

   O lote chama `_travar_o_ato` no topo da transação. A primeira fase de `validar_pergunta` também toma a trava canônica, e a porta 103 toma Colaborador → Disputa → Chamado → Pergunta. Entretanto, depois de sair do `atomic`, a validação chama o laço da disputa, que chama `_materializar_uma` sem tomar essa ordem no topo. (`app/chamados/services/passivo_validado.py:379`, `app/chamados/services/validacao.py:529`, `app/chamados/services/validacao.py:259`, `app/chamados/services/validacao.py:570`, `app/chamados/services/materializacao.py:1669`)

   **Contraexemplo de deadlock possível:** A entra pelo laço numa pergunta sem marco identificável e grava `pendencia_admin`, adquirindo a linha Pergunta; B entra pelo lote ou porta 103, trava Colaborador e espera essa Pergunta; A prossegue até reconciliar o destino e pede Colaborador. Forma-se P→C / C→P. O `save` individual da pergunta não exige previamente o registro da trava do colaborador. Deadlock observado: **[não verificado]**. (`app/chamados/services/materializacao.py:1080`, `app/chamados/services/materializacao.py:1220`, `app/chamados/models.py:712`, `app/chamados/models.py:2687`, `app/chamados/models.py:2713`, `app/chamados/models.py:3465`)

5. **BLOQUEANTE — o rótulo 103 não chega ao modal no caminho aplicável.**

   O Python coloca “anterior a 18/10” e o dia no detalhe de `aplicavel`, mas o modal imprime esse detalhe somente para `bloqueada_paridade` e `ja_satisfeita`. (`app/chamados/services/materializacao.py:680`, `app/templates/chamados/partials/modal_fio.html:239`)

   **Contraexemplo:** pergunta devolvida com irmã respondida e par válido. O dicionário contém o rótulo; o modal não o apresenta. O histórico não resolve: `respostas_anteriores` ignora `DEVOLUCAO`, pois `ATOS_COM_FOTO` contém apenas reabertura e recusa. O teste novo confere o dicionário, sem renderizar a tela. (`app/chamados/services/materializacao.py:678`, `app/templates/chamados/partials/modal_fio.html:248`, `app/chamados/models.py:3599`, `app/chamados/models.py:3955`, `app/chamados/tests/test_bug_par_projetado.py:512`)

6. **BLOQUEANTE — a trilha da devolução é incompleta e sua foto pode faltar.**

   A porta registra autor/motivo em `DecisaoPergunta` e chama `evento` com antes/depois. Porém, apaga também `materializada_por_id`, omitido das duas fotos. **Contraexemplo:** carimbo antigo assinado pelo usuário 17: a devolução elimina essa assinatura sem registrá-la na foto da porta. O comando guarda esse campo na reversão, mas a porta isolada não oferece a mesma garantia. (`app/chamados/services/validacao.py:263`, `app/chamados/services/validacao.py:275`, `app/chamados/services/validacao.py:284`, `app/chamados/models.py:3234`, `app/chamados/management/commands/bug_par_103.py:57`)

   **Outro contraexemplo:** falha no INSERT de `LogAuditoria`. `evento` desfaz seu próprio savepoint, engole a falha e retorna `None`; a porta não verifica esse retorno e pode concluir a devolução com `True`. Resta a decisão com motivo/ator, mas sem a foto antes/depois. (`app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`, `app/chamados/services/validacao.py:280`, `app/chamados/services/validacao.py:287`)

7. **BLOQUEANTE — a reversão é escrita antes, mas pode ser sobrescrita por outro apply.**

   A ordem normal está correta: fecha o arquivo de reversão antes de chamar a primeira devolução. Entretanto, o nome tem resolução de um segundo e a abertura usa modo `w`. (`app/chamados/management/commands/bug_par_103.py:70`, `app/chamados/management/commands/bug_par_103.py:71`, `app/chamados/management/commands/bug_par_103.py:78`)

   **Contraexemplo:** dois applies no mesmo segundo, inclusive para empresas diferentes, usam o mesmo nome; o segundo trunca a reversão do primeiro. O comando não usa criação exclusiva nem identifica a execução/empresa no nome. Ocorrência real: **[não verificado]**. (`app/chamados/management/commands/bug_par_103.py:19`, `app/chamados/management/commands/bug_par_103.py:70`)

8. **NAO-BLOQ — o item 5 melhora, mas “gravada pela irmã neste ato” continua sendo uma inferência.**

   O lote evita reprocessar a irmã no caso coberto pelo teste, relendo o carimbo. Entretanto, `_pela_irma` significa apenas “estava nulo na instância e agora não está nulo”; não prova autoria, via ou gravação neste ato. (`app/chamados/services/passivo_validado.py:551`, `app/chamados/services/passivo_validado.py:554`, `app/chamados/tests/test_bug_par_projetado.py:368`)

   **Contraexemplo:** outra transação encerra a pergunta por identidade entre a coleta e a trava. O lote conta `plantada` embora esse ato não tenha plantado sua batida. Incidência no raspado: **[não verificado]**. (`app/chamados/services/passivo_validado.py:379`, `app/chamados/services/passivo_validado.py:553`, `app/chamados/services/passivo_validado.py:555`)

9. **NAO-BLOQ — os selos novos não cobrem todas as garantias anunciadas.**

   O teste de trava verifica C/D/Chamado antes do primeiro INSERT de batida, mas não verifica Pergunta nem os demais caminhos. **Contraexemplo:** manter essas consultas e introduzir uma escrita anterior de Pergunta não é diretamente cobrado pelas asserções. (`app/chamados/tests/test_bug_par_projetado.py:393`, `app/chamados/tests/test_bug_par_projetado.py:399`, `app/chamados/tests/test_bug_par_projetado.py:405`)

   O item 9 ganhou testes dos dois ramos defensivos; o rollback testa `_materializar_uma` com ambas já validadas. **Contraexemplo descoberto pelo teste:** mover a validação da irmã para fora do savepoint de `_validar_o_par` não seria cobrado por esse caso, que não passa por essa validação nem verifica seu desfazimento. Execução e mutante: **[não verificado]**. (`app/chamados/tests/test_bug_par_projetado.py:422`, `app/chamados/tests/test_bug_par_projetado.py:449`, `app/chamados/tests/test_bug_par_projetado.py:454`, `app/chamados/services/validacao.py:450`)

   A allowlist dispensa a função inteira: qualquer leitura indevida adicionada dentro de uma função listada continua excluída pelo selo. Isso inclui o novo julgamento do par e o lote. (`app/chamados/tests/test_contract_leitores_vitalidade.py:108`, `app/chamados/tests/test_contract_leitores_vitalidade.py:121`, `app/chamados/tests/test_contract_leitores_vitalidade.py:341`)

**Situação dos procedentes da triagem, limitada à leitura estática:**

| Item | Resultado |
|---|---|
| 2 | A irmã implausível passou a ser checada pelo helper comum; a equivalência completa tela/porta continua falsa pelo achado 2. (`app/chamados/services/materializacao.py:182`, `app/chamados/services/validacao.py:445`) |
| 3 | Parcial: datas das duas metades entram no helper, mas a tela só o chama no ramo de conflito de paridade. (`app/chamados/services/materializacao.py:188`, `app/chamados/services/materializacao.py:674`) |
| 4 | Remove “colada” e produz falta da metade/distância; há a regressão do achado 1. O aviso traduzido continua genérico quanto à distância. (`app/chamados/catalogo/recusa.py:29`, `app/chamados/services/materializacao.py:1124`, `app/chamados/services/acoes_disputa.py:34`) |
| 5 | Caso comum coberto; ressalva do achado 8. (`app/chamados/services/passivo_validado.py:551`) |
| 6 | Corrigido no topo do lote; não em todos os caminhos de escrita, conforme achado 4. (`app/chamados/services/passivo_validado.py:379`, `app/chamados/services/materializacao.py:1669`) |
| 9 | Dois testes adicionados; cobertura da validação da irmã incompleta, conforme achado 9. (`app/chamados/tests/test_bug_par_projetado.py:422`, `app/chamados/tests/test_bug_par_projetado.py:437`) |

**BUG-PAR-103: o predicado usa corte exclusivo pelo instante do carimbo**, anterior a `18/10/2026 00:00 America/Sao_Paulo`, equivalente a `03:00 UTC`; não usa o dia do turno. Assim, `18/10 02:59:59 UTC` entra e `03:00 UTC` não entra, satisfeitas as demais condições. (`app/chamados/services/materializacao.py:227`, `app/chamados/services/materializacao.py:255`, `app/chamados/services/materializacao.py:258`, `app/config/settings/base.py:93`)

A lâmpada consultada é a da célula atualmente ligada à pergunta, admitindo `False` e `None`; não é uma fotografia histórica do instante da absolvição. Para intervalo com múltiplas pausas, esse juiz exige todas as lâmpadas correspondentes acesas. A equivalência desse universo com as 103 linhas medidas não pode ser conferida sem os carimbos/células individuais: **[não verificado]**. (`app/chamados/services/materializacao.py:259`, `app/chamados/juizes.py:944`, `app/chamados/juizes.py:960`, `app/chamados/juizes.py:971`)

O contador real calcula `total_aval = universo + len(bug_par_103)` e o comando imprime esse valor; não há 163 fixado. O total efetivamente retornado para a população de produção continua **[não verificado]**. (`app/chamados/services/materializacao.py:320`, `app/chamados/management/commands/bug_par_103.py:61`)

**Escritores e leitores dos estados envolvidos:**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `validada_em` / `validada_por` | Validação e devoluções; também preenchimento automático em `carimbar(resposta='validar')` e limpeza por nova resposta. (`app/chamados/services/validacao.py:136`, `app/chamados/services/validacao.py:201`, `app/chamados/services/validacao.py:270`, `app/chamados/models.py:3160`, `app/chamados/services/disputa_emissao.py:1480`) | Elegibilidade/validação da irmã e seleção do laço. (`app/chamados/services/materializacao.py:180`, `app/chamados/services/validacao.py:451`, `app/chamados/services/materializacao.py:1657`) |
| `materializada_em` / `materializada_por` / carimbo | `_escrever_carimbo`, pelas portas de carimbar/descarimbar; reset em lote. (`app/chamados/models.py:3123`, `app/chamados/models.py:3234`, `app/chamados/models.py:3306`) | Seleção da irmã, predicado 103, contador, tela, prova do par e lote. (`app/chamados/services/materializacao.py:138`, `app/chamados/services/materializacao.py:257`, `app/chamados/services/materializacao.py:303`, `app/chamados/services/materializacao.py:654`, `app/chamados/services/validacao.py:454`, `app/chamados/services/passivo_validado.py:552`) |
| `via_resolucao` | Porta conjunta do carimbo; também atualização isolada por fato/relavra. (`app/chamados/models.py:3127`, `app/chamados/models.py:3315`, `app/chamados/services/relavra_via.py:118`) | Predicado 103, tela e explicador. (`app/chamados/services/materializacao.py:257`, `app/chamados/services/materializacao.py:658`, `app/chamados/services/explicador.py:95`) |
| `recusa_motivo` | Materializador, porta de identidade, recusa admin e resposta de escape. (`app/chamados/services/materializacao.py:1079`, `app/chamados/services/materializacao.py:1348`, `app/chamados/models.py:3213`, `app/chamados/services/validacao.py:695`, `app/chamados/services/disputa_emissao.py:1497`) | Classificação do universo, explicador e tradução do aviso. (`app/chamados/services/materializacao.py:241`, `app/chamados/services/explicador.py:90`, `app/chamados/services/acoes_disputa.py:34`) |
| `data_conferida_em` | Conferência da data. (`app/chamados/services/validacao.py:775`) | Helper do par e gate da materialização, em duas conjunções separadas. (`app/chamados/services/materializacao.py:151`, `app/chamados/services/materializacao.py:1481`) |
| `DecisaoPergunta` / motivo 103 | Porta `registrar`, chamada pela devolução. (`app/chamados/models.py:4029`, `app/chamados/services/validacao.py:275`) | Rótulo 103 e histórico, com a lacuna de apresentação do achado 5. (`app/chamados/services/materializacao.py:265`, `app/chamados/models.py:3594`) |

Há dois significados relevantes: `materializada_em` também representa encerramento sem batida; `validada_em` pode resultar de preenchimento automático, além da validação explícita do admin. (`app/chamados/services/materializacao.py:654`, `app/chamados/services/materializacao.py:658`, `app/chamados/models.py:3160`)

Nos caminhos examinados, o plantio continua passando por `registro_batida`, e a hora deriva de `_ts_tipo_do_plantio`; não há segundo escritor/derivador demonstrado nesses trechos. Há relógio interno para o novo `ato_em`, mas ele não decide o corte das 103; divergência de elegibilidade causada por esse relógio é **[não verificado]**. (`app/chamados/services/materializacao.py:195`, `app/chamados/services/materializacao.py:203`, `app/chamados/services/materializacao.py:1172`, `app/chamados/services/validacao.py:276`, `app/chamados/services/materializacao.py:258`)

## 13/11 15:12 · sessao 591bbeaab94c · - · - · origem=Ronald (tmux)

**Cura restrita ao informativo da lavra:** o lavrador captura `timezone.localtime(timezone.now()).date()` uma vez e passa por `montar_linhas(..., hoje=_hoje)` → `_informativo_rescisao(..., hoje=hoje)` → `export.informativo_rescisao(..., hoje=hoje)` → `_dias_sem_decisao(..., hoje)`. Com data fornecida, o fallback `localdate()` não executa. (`app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/management/commands/lavrar_retrato_folha.py:65`; `app/folha/retrato_folha.py:1163`; `app/folha/retrato_folha.py:1225`; `app/folha/export.py:1228`; `app/folha/export.py:1238`)

| Caminho | Quem lê relógio / chega ao informativo? |
|---|---|
| Lavrador | Captura dia local na borda, como acima. (`app/folha/management/commands/lavrar_retrato_folha.py:48`) |
| `montar_linhas` avulso | Default `hoje=None`; helper também aceita `None`; se omitido, cada desligado elegível relê relógio no interior de `_dias_sem_decisao`. Não encontrei outro chamador de produção de `montar_linhas` além do lavrador. (`app/folha/retrato_folha.py:1120`; `app/folha/retrato_folha.py:1200`; `app/folha/retrato_folha.py:1225`; `app/folha/export.py:1238`; `app/folha/management/commands/lavrar_retrato_folha.py:65`) |
| Tela / drawer | Não alcança `informativo_rescisao`: lê o JSON persistido. Seu hoje local escolhe competência, sem recomputar informativo. (`app/folha/views_retrato.py:176`; `app/folha/views_retrato.py:190`; `app/folha/views_retrato.py:196`; `app/colaboradores/views.py:982`; `app/colaboradores/views.py:984`; `app/folha/retrato_folha.py:110`) |
| Haiku `o_que_segura` | Omite `hoje`; o relógio é lido no interior de `export._dias_sem_decisao`, não na borda Haiku. (`app/folha/fora_da_folha.py:241`; `app/folha/export.py:1228`; `app/folha/export.py:1238`) |
| Vigia | Não alcança `informativo_rescisao`: captura dia local e chama `censo_alcance`, que chama `dias_que_seguram` e a retenção. (`app/chamados/management/commands/vigiar_fio_morto.py:168`; `app/chamados/management/commands/vigiar_fio_morto.py:169`; `app/chamados/management/commands/vigiar_fio_morto.py:178`; `app/chamados/services/vigia_fio.py:586`; `app/folha/fora_da_folha.py:137`; `app/folha/fora_da_folha.py:98`) |

**NAO-BLOQ no delta, resíduo de relógio interno explicitamente preservado:** a documentação nova diz que Haiku lê relógio “na borda dele”, mas a chamada omite a data e a leitura efetiva ocorre dentro do laço compartilhado. Contraexemplo: um desligado com vínculo, sem célula em 05/10 e demissão posterior; chamada começada em 05/10 às 23:59 que só alcança o fallback após meia-noite passa a incluir 05/10, pelo ramo `d < hoje`. Esse resíduo já está no caminho preexistente, não é cura abrangente de relógio. (`app/folha/retrato_folha.py:1211`; `app/folha/fora_da_folha.py:241`; `app/folha/export.py:1238`; `app/folha/export.py:1271`)

**Outro limite preexistente:** o `hoje` do lavrador só foi injetado no informativo; a fonte chamada antes dele ainda não o recebe. O vigia também não injeta seu hoje em `dias_que_seguram`. Portanto “uma leitura de relógio na rodada” não vale para toda a cadeia de retenção. Contraexemplo equivalente ao acima para um ativo faz a fonte variar ao atravessar meia-noite. (`app/folha/retrato_folha.py:1157`; `app/folha/retrato_folha.py:1163`; `app/chamados/services/vigia_fio.py:566`; `app/chamados/services/vigia_fio.py:586`; `app/folha/fora_da_folha.py:98`; `app/folha/export.py:1238`; `app/folha/export.py:1271`)

**Fuso:** os leitores identificados usam dia civil local por `localtime(...).date()` ou `localdate()`, e a configuração da cópia é `America/Sao_Paulo`, `USE_TZ=True`. Não há divergência UTC demonstrada nesses caminhos do informativo; `timezone.now()` seguido de conversão local não produz dia UTC. Configuração efetiva produtiva e eventual ativação de outro fuso durante execução: **[nao verificado]**. (`app/config/settings/base.py:93`; `app/config/settings/base.py:95`; `app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/export.py:1238`; `app/chamados/management/commands/vigiar_fio_morto.py:169`)

## 13/11 15:12 · sessao a2457deca3e5 · - · - · origem=Ronald (tmux)

- **NAO-BLOQ — inscrição de bolha descreve entradas/saídas de forma incompleta.** `@entra ... para o juiz, so o colaborador` só descreve `esta_desligado`; `julgar_desligamento` também lê POST e relógio interno. `@sai` não distingue executor agendado do imediato. Contraexemplo: agendar amanhã grava `data_demissao`/motivo agora, retorna sem encerrar vínculo e sem abrir chamado DP. (`app/colaboradores/services/desligamento.py:7`; `app/colaboradores/services/desligamento.py:35`; `app/colaboradores/services/desligamento.py:40`; `app/colaboradores/services/desligamento.py:60`; `app/colaboradores/services/desligamento.py:107`; `app/colaboradores/services/desligamento.py:110`; `app/colaboradores/services/desligamento.py:126`)

- **Não comprovo segundo escritor do valor `desligado`.** O cron agendado julga e chama o mesmo executor; a atribuição produtiva literal identificada ocorre no executor, que persiste pela porta canônica. A frase “agendado só grava na data” é correta apenas para `situacao='desligado'`; é falsa para `data_demissao`, escrita antecipadamente. (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`; `app/colaboradores/services/desligamento.py:11`; `app/colaboradores/services/desligamento.py:110`; `app/colaboradores/services/desligamento.py:129`; `app/colaboradores/services/desligamento.py:133`; `app/colaboradores/services/colaborador.py:73`)

- **`situacao` tem outros escritores legítimos; “um escritor” não vale para o campo inteiro.** Reativação atribui `ativo`, limpa data/motivo e persiste pela mesma porta. `data_demissao` significa data planejada enquanto ativo e data de desligamento quando consumado; o cron lê conjuntamente data e situação para distinguir os estados. (`app/colaboradores/services/desligamento.py:235`; `app/colaboradores/services/desligamento.py:239`; `app/colaboradores/services/desligamento.py:110`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`)

- **NAO-BLOQ — documentação nova erra a segunda consulta do informativo.** Diz “células + férias/ausências”, mas carrega células e vínculos. Contraexemplo: mesmo sem férias/ausências, cada desligado elegível consulta `CelulaDia` e `EscalaColaborador`. (`app/folha/retrato_folha.py:1210`; `app/folha/export.py:1247`; `app/folha/export.py:1253`; `app/escala/alimentacao.py:186`; `app/escala/alimentacao.py:148`)

- **O selo de mapa garante presença das etiquetas, não a verdade do contrato.** Um cabeçalho completo com entradas/saídas falsas continua satisfazendo os testes: eles consultam ausência de inscrição e estrutura do parser. (`app/core/tests/test_selo_mapa_contrato.py:41`; `app/core/tests/test_selo_mapa_contrato.py:60`)

Execução de testes, mutantes e ocorrência produtiva: **[nao verificado]**. [nao verificado]

## 13/11 15:12 · sessao 3777adbae6fd · HJ-093 · sonda · origem=Code

Transcrição literal do anexo; `[não verificado]` aplica-se a todas as linhas abaixo quanto à confirmação independente em produção.

```text
JUST 3331 {'id': '3331', 'batida_entrada': '139122', 'data_turno': '2026-10-27', 'tipo': 'esqueci_saida', 'saida_informada': 'None', 'latitude': 'None', 'longitude': 'None', 'status': 'aprovada', 'aprovada_por': '657', 'aprovada_em': '2026-10-28 11:31:15.607936+00:00', 'motivo_rejeicao': '', 'batida_gerada': 'None', 'chamado_origem': '35329', 'criado_em': '2026-10-27 22:20:18.830771+00:00', 'atualizado_em': '2026-10-28 11:31:15.608025+00:00'}
FROTA2 2026-11 com_justificativa 33 realizado0_COM_batida_casada 9 colabs 9 realizado0_SEM_batida 2
FROTA2 2026-10 com_justificativa 134 realizado0_COM_batida_casada 23 colabs 21 realizado0_SEM_batida 5
FROTA2 2026-09 com_justificativa 16 realizado0_COM_batida_casada 3 colabs 2 realizado0_SEM_batida 0
FROTA2 2026-08 com_justificativa 24 realizado0_COM_batida_casada 10 colabs 5 realizado0_SEM_batida 0
CASOS 2026-11 [(140889, 56, '2026-10-25', 'disp', 1, 'luz', 1, 'prev', 720), (127808, 976, '2026-10-26', 'disp', 3, 'luz', 1, 'prev', 660), (135337, 496, '2026-10-22', 'disp', 1, 'luz', 1, 'prev', 720), (140580, 750, '2026-10-26', 'disp', 1, 'luz', 1, 'prev', 720), (141802, 996, '2026-10-26', 'disp', 3, 'luz', 1, 'prev', 440), (131421, 912, '2026-10-23', 'disp', 3, 'luz', 1, 'prev', 660), (121745, 962, '2026-10-25', 'disp', 2, 'luz', 2, 'prev', 440), (129875, 932, '2026-10-27', 'disp', 1, 'luz', 1, 'prev', 720), (143004, 1004, '2026-10-27', 'disp', 3, 'luz', 1, 'prev', 660)]
CASOS 2026-10 [(118689, 56, '2026-10-07', 'disp', 1, 'luz', 1, 'prev', 720), (112074, 567, '2026-10-12', 'disp', 3, 'luz', 1, 'prev', 660), (114134, 206, '2026-09-22', 'disp', 3, 'luz', 1, 'prev', 480), (110059, 341, '2026-10-07', 'disp', 1, 'luz', 1, 'prev', 720), (108830, 340, '2026-10-08', 'disp', 3, 'luz', 1, 'prev', 660), (113955, 438, '2026-09-23', 'disp', 3, 'luz', 1, 'prev', 660), (112352, 347, '2026-09-30', 'disp', 1, 'luz', 1, 'prev', 720), (112358, 347, '2026-10-06', 'disp', 1, 'luz', 1, 'prev', 720), (106510, 922, '2026-09-28', 'disp', 1, 'luz', 1, 'prev', 480), (106526, 922, '2026-10-14', 'disp', 1, 'luz', 1, 'prev', 480), (110352, 389, '2026-09-30', 'disp', 1, 'luz', 1, 'prev', 720), (105323, 789, '2026-10-11', 'disp', 3, 'luz', 1, 'prev', 440), (105365, 938, '2026-09-23', 'disp', 1, 'luz', 1, 'prev', 720), (33812, 564, '2026-09-24', 'disp', 3, 'luz', 1, 'prev', 480), (116944, 358, '2026-10-02', 'disp', 3, 'luz', 1, 'prev', 480), (105667, 931, '2026-09-25', 'disp', 1, 'luz', 1, 'prev', 480), (33879, 310, '2026-09-21', 'disp', 3, 'luz', 1, 'prev', 440), (104743, 228, '2026-10-01', 'disp', 1, 'luz', 1, 'prev', 720), (113727, 468, '2026-10-05', 'disp', 3, 'luz', 1, 'prev', 440), (116515, 829, '2026-09-23', 'disp', 3, 'luz', 1, 'prev', 440), (106125, 331, '2026-10-03', 'disp', 3, 'luz', 1, 'prev', 660), (118347, 643, '2026-09-25', 'disp', 1, 'luz', 1, 'prev', 720), (111910, 396, '2026-09-28', 'disp', 1, 'luz', 1, 'prev', 720)]
CASOS 2026-09 [(24651, 697, '2026-09-01', 'disp', 3, 'luz', 1, 'prev', 540), (24660, 697, '2026-09-10', 'disp', 3, 'luz', 1, 'prev', 540), (21683, 228, '2026-09-14', 'disp', 1, 'luz', 1, 'prev', 720)]
CASOS 2026-08 [(3283, 408, '2026-08-17', 'disp', 3, 'luz', 1, 'prev', 660), (2261, 855, '2026-08-18', 'disp', 3, 'luz', 1, 'prev', 660), (8127, 518, '2026-07-25', 'disp', 3, 'luz', 1, 'prev', 440), (8131, 518, '2026-07-29', 'disp', 3, 'luz', 1, 'prev', 440), (8148, 518, '2026-08-15', 'disp', 3, 'luz', 1, 'prev', 440), (8150, 518, '2026-08-17', 'disp', 3, 'luz', 1, 'prev', 440), (16234, 510, '2026-08-10', 'disp', 3, 'luz', 1, 'prev', 420), (1013, 863, '2026-08-10', 'disp', 3, 'luz', 1, 'prev', 660), (1021, 863, '2026-08-18', 'disp', 3, 'luz', 1, 'prev', 660), (1023, 863, '2026-08-20', 'disp', 3, 'luz', 1, 'prev', 660)]
FIM
```

Sim: o trecho fornecido termina em `FIM`, sem erro exibido. [não verificado]

`realizado0_COM_batida_casada`: realizado zero com alguma lâmpada `luz != None`, indicando batida real casada a marco, conforme a definição fornecida. [não verificado]  
`realizado0_SEM_batida`: realizado zero no dia inteiro justificado/dispensado, sem batida; a dispensa registra `luz=None`. [não verificado]; [cartorio.py:786](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ponto/services/cartorio.py:786)  
`acesa=True` sozinha não distingue os grupos: a dispensa também acende a lâmpada, mantendo `luz=None` e registrando `dispensada`. [cartorio.py:807](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ponto/services/cartorio.py:807)

A JUST `3331` traz tipo `esqueci_saida`, status `aprovada` e turno `2026-10-27`. [não verificado] O tipo significa “Esqueci de bater saída”. [models.py:119](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ponto/models.py:119)

Ela nomeia `batida_entrada=139122`; traz `saida_informada=None` e `batida_gerada=None`, sem ID ou horário explícito de marco no recorte. [não verificado] No modelo, esses campos representam, respectivamente, a batida de entrada do turno, a saída declarada e a batida de saída criada após aprovação. [models.py:139](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ponto/models.py:139), [models.py:149](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ponto/models.py:149), [models.py:167](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ponto/models.py:167)

_4642 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
