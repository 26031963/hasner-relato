# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 18:58** · publicado em **31/10/2026 18:59** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 18:58 · sessao 2a565ed96184 · BUG-ENC-CELULAS-FORMA · censo · origem=Code

**O desvio comprovado é na chave usada para escolher o vínculo do turno:** `dias_encerrados` recebe `(pk,data)` e, sem `turnos=`, repassa esse mapa ao caminho que espera `data`; ali nasce `(pk,(pk,data))`. O papel da ata continua sendo lido pelos valores das células. (`app/ponto/turnos.py:2130`, `app/ponto/turnos.py:2148`, `app/ponto/turnos.py:1954`, `app/ponto/turnos.py:1958`, `app/ponto/turnos.py:1629`)

Impacto em colaboradores reais, quantidade de dias atingidos e execução de um RED: **[nao verificado]**.

1. **Funções, formas e chamadores**

O inventário abaixo considera chamadas estáticas, inclusive aliases importados; chamadas construídas dinamicamente são **[nao verificado]**.

| Função | Forma esperada e inferência |
|---|---|
| `papel_por_minuto_da_ata` | Constrói `{data: CelulaDia}` quando carrega. Entretanto, operacionalmente a chave é **opaca**: percorre `chave,cel`, consulta o objeto e usa a chave somente no mapa intermediário. Portanto, receber `(pk,data)` não torna a ata invisível. (`app/ponto/turnos.py:1584`, `app/ponto/turnos.py:1629`, `app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1702`, `app/ponto/turnos.py:1729`) |
| `turnos_do_colab` | `{data: CelulaDia}`, explicitamente documentado e produzido pela carga própria. (`app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1884`) |
| `_turnos_do_colab_puro` | `{data: CelulaDia}`; transforma cada chave em `(cid,data)` para perguntar pelo vínculo. (`app/ponto/turnos.py:1921`, `app/ponto/turnos.py:1954`, `app/ponto/turnos.py:1958`) |
| `dia_encerrado` | Por `data`: repassa para `dia_encerrado_motivo`, que alimenta `turnos_do_colab`. (`app/ponto/turnos.py:2075`, `app/ponto/turnos.py:2096`) |
| `dia_encerrado_motivo` | Por `data` para os turnos. Não repassa esse parâmetro ao julgamento puro nem à carga de vínculo. (`app/ponto/turnos.py:2096`, `app/ponto/turnos.py:2097`) |
| `_dia_encerrado_puro` | `(pk,data)`: repassa ao julgamento que consulta o DNA pelo fim previsto. (`app/ponto/turnos.py:2102`, `app/ponto/turnos.py:2119`, `app/escala/models.py:1095`) |
| `_dia_encerrado_motivo_puro` | `(pk,data)` para `fim_turno` → `marcos_turno` → `ec.marcos_do_dia`. (`app/ponto/turnos.py:2119`, `app/escala/servico_jornada.py:92`, `app/escala/servico_jornada.py:69`, `app/escala/models.py:1095`) |
| `dias_encerrados` | Documenta `(pk,data)` e usa essa forma para vínculo/DNA; porém repassa o parâmetro original sem conversão para `turnos_do_colab` quando `turnos is None`. (`app/ponto/turnos.py:2130`, `app/ponto/turnos.py:2145`, `app/ponto/turnos.py:2148`, `app/ponto/turnos.py:2164`) |
| `_dias_encerrados_puro`, parâmetro equivalente `cels` | `(pk,data)`, explicitamente documentado e passado ao vínculo e ao julgamento do fim previsto. (`app/ponto/turnos.py:2153`, `app/ponto/turnos.py:2164`, `app/ponto/turnos.py:2165`) |
| `_turno_aberto_puro` | `(pk,data)` para o DNA; converte explicitamente para `data` antes de alimentar o papel da ata. (`app/ponto/turnos.py:2232`, `app/ponto/turnos.py:2254`, `app/ponto/turnos.py:2273`, `app/ponto/turnos.py:2274`) |

**Chamadores de `papel_por_minuto_da_ata`:**

- Corpo dos turnos: mapa recebido, nominalmente por `data`. (`app/ponto/turnos.py:1937`)
- Turno aberto: `None` ou conversão explícita de `(pk,data)` para `data`. (`app/ponto/turnos.py:2270`, `app/ponto/turnos.py:2274`)
- Motor: `_cels_d`, por `data`, construído imediatamente antes. (`app/ponto/motor_calculo_v2.py:448`, `app/ponto/motor_calculo_v2.py:450`)
- Testes: todos passam compreensões por `data`. (`app/ponto/tests/test_o68b_papel_da_ata_no_motor.py:89`, `app/ponto/tests/test_o68b_papel_da_ata_no_motor.py:248`, `app/ponto/tests/test_o68b_papel_da_ata_no_motor.py:455`, `app/ponto/tests/test_o68b_papel_da_ata_no_motor.py:551`, `app/ponto/tests/test_o68b_papel_da_ata_no_motor.py:590`, `app/ponto/tests/test_o68b_papel_da_ata_no_motor.py:652`)

**Chamadores de `_turnos_do_colab_puro`:**

- Casca: repassa `celulas`; a carga própria converte para `data`, mas uma alimentação externa é repassada como veio. (`app/ponto/turnos.py:1884`, `app/ponto/turnos.py:1913`)
- Lote: converte explicitamente para `data`. (`app/ponto/turnos.py:3003`, `app/ponto/turnos.py:3005`)

**Chamadores de `turnos_do_colab` com alimentação explícita:**

| Chamador | Forma passada |
|---|---|
| `app/ponto/turnos.py:2096` | Parâmetro original de `dia_encerrado_motivo`, sem conversão. |
| `app/ponto/turnos.py:2148` | Parâmetro original de `dias_encerrados`: `(pk,data)` pelo contrato de `app/ponto/turnos.py:2130`. |
| `app/escala/utils.py:1524` | Conversão explícita para `data`, alias `_tdc_pre`. |
| `app/escala/utils.py:1838` | `_cels_juiz`, por `data`, com cópias contendo a ata da montagem atual. (`app/escala/utils.py:1826`, `app/escala/utils.py:1837`) |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88` | `cels`, por `data`, construído em `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`. |
| `app/ponto/tests/test_o142_turno_invariante_na_janela.py:169`, `app/ponto/tests/test_o142_turno_invariante_na_janela.py:172`, `app/ponto/tests/test_o142_turno_invariante_na_janela.py:176` | Parâmetro do helper; default mapa vazio, ou `None` nas chamadas de `app/ponto/tests/test_o142_turno_invariante_na_janela.py:242` e `app/ponto/tests/test_o142_turno_invariante_na_janela.py:243`. |
| `app/ponto/tests/test_o142_turno_invariante_na_janela.py:203`, `app/ponto/tests/test_o142_turno_invariante_na_janela.py:204`, `app/ponto/tests/test_o142_turno_invariante_na_janela.py:214` | `{}`. |
| `app/escala/tests/test_saida_valida_vira_orfa.py:158` | `{DIA: cel}`. |

**Demais chamadores de `turnos_do_colab`: todos omitem `celulas`, portanto usam `None` e a carga própria por `data`.** (`app/ponto/turnos.py:1848`, `app/ponto/turnos.py:1882`, `app/ponto/turnos.py:1884`)

- No próprio módulo: `app/ponto/turnos.py:778`, `app/ponto/turnos.py:2179`, `app/ponto/turnos.py:2909`, `app/ponto/turnos.py:3041`.
- Núcleo, leitura e views: `app/ponto/nucleo.py:47`; `app/ponto/turno_leitura.py:89`, `app/ponto/turno_leitura.py:95`; `app/ponto/views.py:377`, `app/ponto/views.py:1586`, `app/ponto/views.py:3167`; `app/api/views.py:1449`.
- Escala: `app/escala/regua_defesa.py:358`; `app/escala/servico_jornada.py:250`, `app/escala/servico_jornada.py:392`; `app/escala/services/escala_auto.py:87`; `app/escala/services/impacto_troca.py:18`.
- Chamados: `app/chamados/models.py:1260`; `app/chamados/juizes.py:369`, `app/chamados/juizes.py:1727`; `app/chamados/detector_anomalias.py:155`; `app/chamados/services/materializacao.py:109`, `app/chamados/services/materializacao.py:206`; `app/chamados/services/disputa_emissao.py:644`.
- Apresentação: `app/relatorios/pdf_espelho.py:213`, `app/relatorios/pdf_espelho.py:447`; `app/colaboradores/services/calendario.py:301`; `app/colaboradores/services/drawer.py:45`.
- Serviços de ponto: `app/ponto/services/espelho.py:625`; `app/ponto/services/bordas_realizado.py:66`; `app/ponto/services/teto_na_lista.py:68`; `app/ponto/services/vigia_pareador_celula.py:97`; `app/ponto/services/esmeril_espelho.py:98`; `app/ponto/services/triagem_batida.py:552`; `app/ponto/services/regularizacao.py:61`; `app/ponto/services/passivo_s84.py:82`.
- Commands: `app/ponto/management/commands/detectar_par_relampago.py:101`; `app/ponto/management/commands/dna_x_batida_real.py:69`; `app/ponto/management/commands/marcar_abandono_provisorio.py:46`; `app/ponto/management/commands/detectar_ausencias.py:336`; `app/ponto/management/commands/vigia_plantio_overnight.py:38`; `app/ponto/management/commands/verificar_nucleo.py:29`; `app/ponto/management/commands/e6_oraculo.py:147`; `app/ponto/management/commands/censo_fase_12x36.py:180`; `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124`.
- Testes de ponto: `app/ponto/tests/test_retratada_geometria.py:39`; `app/ponto/tests/test_turnos_borda_direita.py:37`; `app/ponto/tests/test_pareador_dna_multivig.py:55`; `app/ponto/tests/test_bateria_lavratura.py:1406`; `app/ponto/tests/test_vinculo_do_dia_pela_celula.py:144`, `app/ponto/tests/test_vinculo_do_dia_pela_celula.py:156`; `app/ponto/tests/test_pareador_dna.py:50`; `app/ponto/tests/test_o280_bateria_da_lei.py:3010`; `app/ponto/tests/test_f9d_n1_caracterizacao_eco_flush.py:96`; `app/ponto/tests/test_turnos_do_colab_troca_escala.py:53`.
- Outros testes: `app/relatorios/tests/test_pdf_le_o_juiz_do_dia.py:61`; `app/relatorios/tests/test_pdf_sem_regra_propria.py:94`; `app/escala/tests/test_ata_x_turno_entrada.py:82`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:96`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:199`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:76`, `app/escala/tests/test_saida_valida_cross_meia_noite.py:90`; `app/escala/tests/test_saida_valida_vira_orfa.py:92`, `app/escala/tests/test_saida_valida_vira_orfa.py:196`; `app/api/tests/test_espelho_app_fonte_e_geometria.py:322`; `app/chamados/tests/test_orfas_pelo_evento.py:391`.

**Chamadores de `dia_encerrado`:**

- Com alimentação por `data`: fechamento, `_cel_do_dia` construído com o pad. (`app/ponto/services/fechamento.py:354`, `app/ponto/services/fechamento.py:359`, `app/ponto/services/fechamento.py:361`)
- Com alimentação `(pk,data)`: vigia de hora; entra na mesma incompatibilidade do caminho dos turnos. (`app/ponto/services/vigia_de_hora.py:57`, `app/ponto/turnos.py:2096`, `app/ponto/turnos.py:1954`)
- Sem alimentação: `app/ponto/services/flip_auto.py:285`; `app/ponto/management/commands/detectar_ausencias.py:473`; `app/ponto/tests/test_c1_dias_encerrados.py:32`; `app/chamados/tests/test_orfas_pelo_evento.py:181`, `app/chamados/tests/test_orfas_pelo_evento.py:279`.
- Sem alimentação, testes da juíza: `app/ponto/tests/test_juiz_dia_encerrado.py:80`, `app/ponto/tests/test_juiz_dia_encerrado.py:84`, `app/ponto/tests/test_juiz_dia_encerrado.py:88`, `app/ponto/tests/test_juiz_dia_encerrado.py:91`, `app/ponto/tests/test_juiz_dia_encerrado.py:96`, `app/ponto/tests/test_juiz_dia_encerrado.py:97`, `app/ponto/tests/test_juiz_dia_encerrado.py:101`, `app/ponto/tests/test_juiz_dia_encerrado.py:107`, `app/ponto/tests/test_juiz_dia_encerrado.py:121`, `app/ponto/tests/test_juiz_dia_encerrado.py:122`, `app/ponto/tests/test_juiz_dia_encerrado.py:123`, `app/ponto/tests/test_juiz_dia_encerrado.py:126`, `app/ponto/tests/test_juiz_dia_encerrado.py:130`, `app/ponto/tests/test_juiz_dia_encerrado.py:137`, `app/ponto/tests/test_juiz_dia_encerrado.py:149`, `app/ponto/tests/test_juiz_dia_encerrado.py:150`, `app/ponto/tests/test_juiz_dia_encerrado.py:155`, `app/ponto/tests/test_juiz_dia_encerrado.py:156`.

**Chamadores de `dia_encerrado_motivo`:**

- Repasse do parâmetro original: `app/ponto/turnos.py:2075`.
- Por `data`, conversão explícita: `app/ponto/tests/test_c1_dias_encerrados.py:73`, `app/ponto/tests/test_c1_dias_encerrados.py:74`.
- Sem alimentação: `app/ponto/tests/test_c1_dias_encerrados.py:70`; `app/chamados/reconciliador.py:1045`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:87`; `app/chamados/tests/test_orfas_pelo_evento.py:278`, `app/chamados/tests/test_orfas_pelo_evento.py:321`, `app/chamados/tests/test_orfas_pelo_evento.py:342`, `app/chamados/tests/test_orfas_pelo_evento.py:398`, `app/chamados/tests/test_orfas_pelo_evento.py:399`, `app/chamados/tests/test_orfas_pelo_evento.py:419`.

**Chamadores dos outros corpos:**

| Função | Chamadores e forma |
|---|---|
| `_dia_encerrado_puro` | Nenhuma chamada estática encontrada. A docstring diz que o carregador a lê, mas o carregador atual chama a variante com motivo. (`app/ponto/turnos.py:2101`, `app/ponto/turnos.py:2164`) |
| `_dia_encerrado_motivo_puro` | Sem células em `app/ponto/turnos.py:2096`; repasse em `app/ponto/turnos.py:2102`; composto `cels` em `app/ponto/turnos.py:2164`, `app/ponto/turnos.py:2165`. |
| `_dias_encerrados_puro` | Composto `cels` na casca, `app/ponto/turnos.py:2147`; composto reconstruído pelo lote, `app/ponto/turnos.py:3012`, `app/ponto/turnos.py:2976`. |
| `_turno_aberto_puro` | Sem células em `app/ponto/turnos.py:2222`; composto em `app/ponto/turnos.py:2360`, construído em `app/ponto/turnos.py:2332`. |

**Todos os chamadores da função livre `dias_encerrados`:**

- Com `(pk,data)`, **sem turnos**: cartório e as duas chamadas do teste alimentado. (`app/ponto/services/cartorio.py:911`; `app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/ponto/tests/test_c1_dias_encerrados.py:77`)
- Com `(pk,data)` **e turnos**: calendário. (`app/colaboradores/services/calendario.py:697`, `app/colaboradores/services/calendario.py:698`)
- Sem células/turnos: `app/ponto/turnos.py:2177`; `app/ponto/management/commands/supra_juiz.py:108`; `app/ponto/management/commands/emitir_furo_retroativo.py:62`; `app/ponto/tests/test_c1_dias_encerrados.py:28`, `app/ponto/tests/test_c1_dias_encerrados.py:51`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:73`.

O método `CargaDaJanela.dias_encerrados` não recebe células; seus chamadores são o leitor de status e o painel. (`app/ponto/turnos.py:3009`, `app/ponto/status_dia.py:196`, `app/core/services/painel_op.py:334`)

Há ainda outro significado de “células”: `horas_reais_do_dia(dia,...)` recebe `dia['celulas']` como **lista de dicts da grade**, indexada por posição, não mapa de `CelulaDia`. Chamadores: grade, leitor da ata e testes de cobertura parcial. (`app/ponto/turnos.py:2667`; `app/escala/utils.py:1194`; `app/escala/services/leitor_celula.py:402`; `app/escala/tests/test_cobertura_parcial_periodo.py:63`, `app/escala/tests/test_cobertura_parcial_periodo.py:72`, `app/escala/tests/test_cobertura_parcial_periodo.py:74`, `app/escala/tests/test_cobertura_parcial_periodo.py:79`, `app/escala/tests/test_cobertura_parcial_periodo.py:144`)

2. **Cargas de `dias_encerrados` e `CargaDaJanela`**

| Insumo/operação | Casca `dias_encerrados` | Lote |
|---|---|---|
| Instante | `timezone.now()` se omitido. (`app/ponto/turnos.py:2140`) | O método recebe `agora` obrigatoriamente. (`app/ponto/turnos.py:3009`) |
| Janela das células do encerramento | `[ini,fim]`, porta `celulas_do_periodo([pk],ini,fim)`, somente se `celulas is None`. (`app/ponto/turnos.py:2145`) | Construtor carrega a janela com pad; método recorta `[ini,fim]`. (`app/ponto/turnos.py:2951`, `app/ponto/turnos.py:2976`, `app/ponto/turnos.py:3012`) |
| Filtro da porta de células | Colaborador em lista e datas inclusivas; sem filtro de `trabalha`, origem ou geradora. (`app/escala/alimentacao.py:41`, `app/escala/alimentacao.py:43`, `app/escala/alimentacao.py:45`) | Mesma porta, vários IDs. (`app/ponto/turnos.py:2951`) |
| Vínculos do encerramento | **Todos** os vínculos do colaborador, `select_related('tipo_escala')`; sem filtro de atividade/vigência. (`app/ponto/turnos.py:2146`) | Todos os vínculos dos IDs, mesmo `select_related`, agrupados por colaborador. (`app/ponto/turnos.py:2961`, `app/ponto/turnos.py:2962`) |
| Turnos recebidos | Usa diretamente `turnos`, quando fornecido. (`app/ponto/turnos.py:2148`) | Calcula e memoiza por colaborador. (`app/ponto/turnos.py:2995`, `app/ponto/turnos.py:3003`, `app/ponto/turnos.py:3007`) |
| Turnos não recebidos | Chama `turnos_do_colab(colaborador,ini,fim,celulas)` com o argumento original. (`app/ponto/turnos.py:2148`) | Alimenta o mesmo corpo com mapa convertido para `data`. (`app/ponto/turnos.py:3005`) |
| Pad dos turnos | `ini-2 dias` a `fim+1 dia`. (`app/ponto/turnos.py:1845`, `app/ponto/turnos.py:1860`) | Mesmo pad. (`app/ponto/turnos.py:2948`) |
| Batidas dos turnos | Colaborador, `retratada_em IS NULL`, datas inclusivas do pad. (`app/ponto/turnos.py:1861`, `app/ponto/turnos.py:1864`) | Porta `batidas_apuraveis(None)`, mesmos cortes, IDs em lote, ordenação por colaborador/timestamp. (`app/ponto/turnos.py:2990`, `app/ponto/turnos.py:2993`; filtro da porta em `app/ponto/turnos.py:2872`) |
| Células dos turnos | Se `celulas is None`, carrega o pad e converte para `data`; caso contrário, usa somente o mapa recebido. (`app/ponto/turnos.py:1882`, `app/ponto/turnos.py:1884`) | Usa células já carregadas do pad e converte para `data`. (`app/ponto/turnos.py:2996`, `app/ponto/turnos.py:3005`) |
| Geradoras/`mais=` | IDs das geradoras dos valores recebidos; candidatos entre `pad_ini` e **fim**, sem estender até `pad_fim`. (`app/ponto/turnos.py:1885`, `app/ponto/turnos.py:1886`) | IDs das geradoras de toda a carga; mesma janela `pad_ini..fim`. (`app/ponto/turnos.py:2952`, `app/ponto/turnos.py:2957`, `app/ponto/turnos.py:2958`) |
| Candidatos | Porta singular delega à plural. (`app/escala/alimentacao.py:153`, `app/escala/alimentacao.py:154`) | Porta plural diretamente. (`app/ponto/turnos.py:2957`) |
| Filtro dos candidatos | Início até o fim; ativa **ou** com `data_fim`; fim nulo **ou** alcançando o início; tipo relacionado. Depois busca os pks faltantes de `mais=` sem repetir esses filtros. (`app/escala/alimentacao.py:116`, `app/escala/alimentacao.py:123`, `app/escala/alimentacao.py:125`) | Mesmo corpo da porta. (`app/escala/alimentacao.py:154`, `app/ponto/turnos.py:2957`) |
| Vínculo usado pela leitura da ata | `_escalas_da_ata([cid])`, chamado pelo leitor de papel quando `escalas=None`. (`app/ponto/turnos.py:1591`, `app/ponto/turnos.py:1913`) | `_escalas_da_ata(ids)` antecipadamente. (`app/ponto/turnos.py:2963`) |
| Escolha dessa escala da ata | Todos os vínculos, ordenados por colaborador, `ativa`, início; a última linha vence. Não é escolha diária pela geradora. (`app/ponto/turnos.py:1556`, `app/ponto/turnos.py:1558`, `app/ponto/turnos.py:1559`) | Mesma função. (`app/ponto/turnos.py:2963`) |
| DNA dos turnos | Provedor chama `marcos_dna_periodo` **sem `celulas=`**, portanto consulta novamente o banco. (`app/ponto/turnos.py:1910`, `app/escala/utils.py:100`) | Provedor passa `celulas=cels_k`, usando a carga existente. (`app/ponto/turnos.py:3001`, `app/escala/utils.py:96`) |
| Última batida | Agregação preguiçosa `Max(timestamp)` de todas as apuráveis do colaborador, sem janela. (`app/ponto/turnos.py:1896`, `app/ponto/turnos.py:1900`) | Agregação preguiçosa por colaborador de todas as apuráveis dos IDs, sem janela. (`app/ponto/turnos.py:2967`, `app/ponto/turnos.py:2971`) |
| Julgamento final | Agrupa turnos por `data_turno`; para cada dia pergunta vínculo e motivo com o mapa composto. (`app/ponto/turnos.py:2159`, `app/ponto/turnos.py:2164`, `app/ponto/turnos.py:2165`) | Chama esse mesmo corpo. (`app/ponto/turnos.py:3012`) |

**Diferenças atuais demonstráveis:**

- A casca alimentada com mapa composto e sem turnos chega ao corpo de turnos com forma errada; o lote converte corretamente. Isso pode selecionar vínculos diferentes para a geometria, embora o julgamento externo do encerramento receba a célula correta. (`app/ponto/turnos.py:2148`, `app/ponto/turnos.py:1954`, `app/ponto/turnos.py:1958`, `app/ponto/turnos.py:2164`, `app/ponto/turnos.py:3005`)
- A casca com células fornecidas não completa a janela do pad; o cartório fornece somente `cels` do julgamento. O lote carrega o pad inteiro. (`app/ponto/services/cartorio.py:888`, `app/ponto/services/cartorio.py:911`, `app/ponto/turnos.py:1882`, `app/ponto/turnos.py:2951`)
- O DNA usado pelo pareador é recarregado pela casca, mas vem da carga existente no lote. Essa diferença de alimentação existe mesmo compartilhando o corpo. (`app/ponto/turnos.py:1910`, `app/ponto/turnos.py:3001`)
- A escolha por período da escala que interpreta a ata é compartilhada pelos dois caminhos; o próprio código a declara como dívida. (`app/ponto/turnos.py:1539`, `app/ponto/turnos.py:1541`, `app/ponto/turnos.py:1591`, `app/ponto/turnos.py:2963`)

O ponto concreto de comparação é **cartório, casca alimentada**, contra **D4/painel, lote**. Mesmo colaborador e janela não garantem insumos equivalentes nesses caminhos por causa das diferenças acima; um caso de resultado diferente é a fixture proposta no item 5, ainda sem execução integral. (`app/ponto/services/cartorio.py:911`, `app/ponto/status_dia.py:189`, `app/ponto/status_dia.py:196`, `app/core/services/painel_op.py:327`, `app/core/services/painel_op.py:334`;

[truncado em 20000 de 30734 caracteres]

## 31/10 18:55 · sessao 6608b9c0f29d · - · - · origem=Ronald (tmux)

**4. Teste existente.** O único teste direto encontrado com `celulas` e sem `turnos` é `C1DiasEncerradosTest.test_propriedade_com_as_celulas_ALIMENTADAS`: chamadas em `app/ponto/tests/test_c1_dias_encerrados.py:66` e `:77`. Ele cria DNA `10:00–14:00` sobre template `19:00–07:00`, compara motivos com a porta singular e afirma encerramento às 15h; não afirma papel nem vínculo (`app/ponto/tests/test_c1_dias_encerrados.py:53–77`). A fixture compartilhada cria cadastro e vínculos; batidas dependem de `_bate`, que esse teste não chama (`app/ponto/tests/test_juiz_dia_encerrado.py:44–75`).

**5. Fixture RED proposta — resultado de execução `[nao verificado]`.**

- Um colaborador ativo, não isento; dois vínculos cobrindo D: B geradora inativa, `data_fim=D`, template `19:00–07:00` sem intervalos; A ativa mais recente, template `19:00–07:00`, intervalo `06:00–07:00`. B ganha quando a célula é vista; A ganha no fallback sem célula (`app/escala/alimentacao.py:237–254`).
- Uma célula D, `trabalha=True`, geradora B, DNA congelado `hi19:00/hii03:00/hfi04:00/hf07:00`; ata com bordas acesas e pausas apagadas. DNA congelado pode diferir do template vivo pela soberania da célula (`app/docs/LEIS.md:71`).
- Duas batidas apuráveis: D às 19h E e D+1 às 07h S; nenhuma posterior. Instante D+1 às 08h. A ausência de batida posterior impede fechar automaticamente a S absorvida como pausa (`app/ponto/turnos.py:1168–1173`).

A forma errada produz chave `(pk,(pk,data))` ao adaptar o mapa e perde B na escolha do segmento (`app/ponto/turnos.py:1954–1958`). Com A, a S das 07h em D+1 cai no intervalo do template: não há DNA de D+1 no mapa, e empate com a borda também absorve a batida (`app/ponto/turnos.py:1227–1242`, `:1354–1368`). Com B, essa absorção não ocorre e E/S fecha (`app/ponto/turnos.py:1489–1490`).

O julgamento externo continua escolhendo B pela célula corretamente chaveada; às 08h, a aberta ainda vive até 07h+4h, enquanto o par fechado já satisfaz o fim previsto das 07h (`app/ponto/turnos.py:2164–2165`, `:2004–2028`, `:2114–2122`). A diferença entra no cartório como `turno_encerrado` e pode separar `FURO_PARCIAL` de ausência desse código; a execução integral desta fixture permanece `[nao verificado]` (`app/ponto/services/cartorio.py:911`, `:1222`; `app/ponto/supra_juiz.py:374`, `:402–412`).

O gabarito é a célula/DNA soberano, papel da ata e encerramento por instante/fato, preservando o contrato D4; não a equivalência com o resultado atual (`app/docs/LEIS.md:71`, `:100`, `:119`, `:136`; `app/docs/ADR/0002-celula-e-o-dia.md:21–22`, `:63–64`).

## 31/10 18:55 · sessao 5a306e745d25 · - · - · origem=Ronald (tmux)

Inventário da Q1, por leitura estática. Prefixo `app/` em todos os caminhos abaixo.

**Funções e forma**

| Função em `ponto/turnos.py` | Forma lida/esperada e evidência |
|---|---|
| `papel_por_minuto_da_ata` :1563 | Forma construída quando carrega: `{data: CelulaDia}` (:1583–1585). **Operacionalmente a chave é opaca**: itera `_d, cel` (:1629), consulta `periodos_do_dia(cel, te)` (:1631), guarda `_dias[_d]` (:1702) e percorre novamente sem calcular nada sobre `_d` (:1729). Portanto não descarta a ata por receber chaves compostas. |
| `turnos_do_colab` :1848 | `{data: CelulaDia}` explícito (:1852) e produzido pelo carregamento (:1884). Consome valores no `mais=` (:1886) e entrega mapa sem conversão ao corpo (:1913). |
| `_turnos_do_colab_puro` :1916 | `{data: CelulaDia}` explícito (:1921); transforma cada chave `_d` em `(_cid, _d)` (:1954), entregando à juíza do vínculo (:1958). |
| `dia_encerrado` :2031 | Mesmo mapa por-data de `turnos_do_colab`: só repassa a `dia_encerrado_motivo` (:2075), cujo repasse posicional aos turnos ocorre em :2096. |
| `dia_encerrado_motivo` :2090 | Por-data para `turnos_do_colab` (:2096). **Não** repassa o mapa ao `_dia_encerrado_motivo_puro` (:2096–2097). |
| `_dia_encerrado_puro` :2100 | Mapa composto requerido pelo DNA: só repassa a `_dia_encerrado_motivo_puro` (:2102); este repassa ao `fim_turno` (:2119). Contrato explícito do DNA em `escala/models.py:1075` e seguintes. |
| `_dia_encerrado_motivo_puro` :2105 | `{(colaborador_id,data): CelulaDia}` para o DNA, pelo repasse a `fim_turno` (:2119), que chega a `EscalaColaborador.marcos_do_dia`; o docstring identifica esse contrato (:2111). |
| `dias_encerrados` :2125 | Documenta `{(colaborador_id,data): CelulaDia}` (:2130); usa-o como `cels` (:2145) para vínculo/DNA (:2164–2165). Porém repassa **o mesmo parâmetro original, sem conversão**, a `turnos_do_colab` (:2148), que exige por-data (:1852/:1954). |
| `_dias_encerrados_puro` :2152 | Composto explícito (:2153); alimenta `vinculo_do_dia` e o fim previsto (:2164–2165). |
| `_turno_aberto_puro` :2225 | Composto explícito (:2231–2233); alimenta `esc.marcos_do_dia` (:2254) e converte para por-data ao alimentar o papel da ata (:2270–2275). |

**Desvio comprovado da forma:** com `celulas={(pk,d): cel}` e sem `turnos=`, `dias_encerrados` entrega esse mapa a `turnos_do_colab` (:2148); `_turnos_do_colab_puro` monta `{(pk,(pk,d)):cel}` (:1954), enquanto a consulta do vínculo pede `(pk,d)` (:1958; implementação `escala/alimentacao.py:196` e seguintes). O papel continua vendo os valores das células, conforme :1629/:1631/:1702/:1729; portanto “a ata inteira fica invisível” não é conclusão sustentada pelo código.

**Todos os chamadores de `papel_por_minuto_da_ata`**

- `ponto/turnos.py:1937`: o mapa recebido pelo corpo, nominalmente por-data (:1921); pode ser composto vindo de :2148.
- `ponto/turnos.py:2270`: `None` ou conversão explícita composto → por-data (:2273–2274).
- `ponto/motor_calculo_v2.py:450`: `_cels_d`, por-data pela compreensão :448.
- `ponto/tests/test_o68b_papel_da_ata_no_motor.py:89`, :248: `{c.data:c for c in celulas}`; :455 e :652: `{cel.data:cel}`; :551: `{c.data:c for c in self._cels()}`; :590: `{c.data:c for c in cels}`.

**Todos os chamadores de `_turnos_do_colab_puro`**

- `ponto/turnos.py:1913`: `celulas`, nominalmente por-data; sem alimentação, convertido em :1884.
- `ponto/turnos.py:3003`: por-data construído explicitamente em :3005.

**Todos os chamadores de `turnos_do_colab` com `celulas` explícito**

- `ponto/turnos.py:2096`: parâmetro `celulas` de `dia_encerrado_motivo`, sem transformação.
- `ponto/turnos.py:2148`: parâmetro de `dias_encerrados`, composto pelo contrato :2130, sem transformação.
- `escala/utils.py:1524`, alias `_tdc_pre`: conversão para por-data explícita.
- `escala/utils.py:1838`: `_cels_juiz`, por-data construído em :1820–1837, incluindo cópia com ata fresca.
- `ponto/management/commands/emitir_turno_aberto_retroativo.py:88`: `cels`, por-data construído em :86–87; janela de células `ini-2` a `fim+2`.
- `ponto/tests/test_o142_turno_invariante_na_janela.py:169`, :172, :176: parâmetro do helper `_tres_janelas`; default é `VAZIO = MappingProxyType({})` (:54/:161), e as chamadas :242–243 passam `None`. As chamadas :203, :204, :214 passam `{}`.
- `escala/tests/test_saida_valida_vira_orfa.py:158`: `{DIA:cel}`.

**Todos os demais chamadores de `turnos_do_colab`: omitem `celulas`, portanto recebem o default `None` e o carregamento/conversão de `ponto/turnos.py:1882–1884`**

- `ponto/turnos.py:778`, :2179, :2909, :3041.
- `ponto/turno_leitura.py:89`, :95.
- `ponto/nucleo.py:47`.
- `ponto/views.py:377`, :1586, :3167.
- `relatorios/pdf_espelho.py:213` (`_tdc0`), :447.
- `escala/regua_defesa.py:358`.
- `escala/servico_jornada.py:250`, :392 (`_tdc`).
- `api/views.py:1449`.
- `chamados/models.py:1260` (`_tdc`).
- `chamados/juizes.py:369`, :1727.
- `chamados/detector_anomalias.py:155`.
- `colaboradores/services/calendario.py:301`.
- `colaboradores/services/drawer.py:45` (`_tdc`).
- `ponto/services/espelho.py:625` (`_tdc_esp`).
- `ponto/services/bordas_realizado.py:66`.
- `ponto/services/teto_na_lista.py:68`.
- `ponto/services/vigia_pareador_celula.py:97`.
- `ponto/services/esmeril_espelho.py:98`.
- `ponto/services/triagem_batida.py:552`.
- `ponto/services/regularizacao.py:61`.
- `ponto/services/passivo_s84.py:82`.
- `ponto/management/commands/detectar_par_relampago.py:101`.
- `ponto/management/commands/dna_x_batida_real.py:69`.
- `ponto/management/commands/marcar_abandono_provisorio.py:46`.
- `ponto/management/commands/detectar_ausencias.py:336` (`_tdc_a84`).
- `ponto/management/commands/vigia_plantio_overnight.py:38`.
- `ponto/management/commands/verificar_nucleo.py:29`.
- `ponto/management/commands/e6_oraculo.py:147`.
- `ponto/management/commands/censo_fase_12x36.py:180`.
- `relatorios/management/commands/vinculo_do_dia_divergentes.py:124`.
- `escala/services/escala_auto.py:87`.
- `escala/services/impacto_troca.py:18`.
- `chamados/services/materializacao.py:109`, :206.
- `chamados/services/disputa_emissao.py:644`.
- Testes: `ponto/tests/test_retratada_geometria.py:39`; `test_turnos_borda_direita.py:37`; `test_pareador_dna_multivig.py:55`; `test_bateria_lavratura.py:1406`; `test_vinculo_do_dia_pela_celula.py:144`, :156; `test_pareador_dna.py:50`; `test_o280_bateria_da_lei.py:3010`; `test_f9d_n1_caracterizacao_eco_flush.py:96`; `test_turnos_do_colab_troca_escala.py:53`.
- Testes: `relatorios/tests/test_pdf_le_o_juiz_do_dia.py:61`; `test_pdf_sem_regra_propria.py:94`; `escala/tests/test_ata_x_turno_entrada.py:82`; `test_juiz_do_par_ata_casa_pelo_turno.py:96`, :199; `test_saida_valida_cross_meia_noite.py:76`, :90; `test_saida_valida_vira_orfa.py:92`, :196; `api/tests/test_espelho_app_fonte_e_geometria.py:322`; `chamados/tests/test_orfas_pelo_evento.py:391` (`turnos.turnos_do_colab`).

**Todos os chamadores de `dia_encerrado`**

Com alimentação:

- `ponto/services/fechamento.py:361`, alias `_dia_encerrado`: `_cel_do_dia` por-data, construído em :355–359 e limitado à janela do pad (:354).
- `ponto/services/vigia_de_hora.py:57`: `{(c.pk,cel.data):cel}` composto — conflita com o repasse por-data do caminho singular (`ponto/turnos.py:2096/:1954`).

Sem alimentação:

- `ponto/services/flip_auto.py:285`.
- `ponto/management/commands/detectar_ausencias.py:473`.
- `ponto/tests/test_c1_dias_encerrados.py:32`.
- `ponto/tests/test_juiz_dia_encerrado.py:80`, :84, :88, :91, :96, :97, :101, :107, :121, :122, :123, :126, :130, :137, :149, :150, :155, :156.
- `chamados/tests/test_orfas_pelo_evento.py:181`, :279 (`turnos.dia_encerrado`).

**Todos os chamadores de `dia_encerrado_motivo`**

- `ponto/turnos.py:2075`: `celulas` original do singular.
- `ponto/tests/test_c1_dias_encerrados.py:74`: `por_data`, conversão explícita em :73.
- Sem alimentação: `ponto/tests/test_c1_dias_encerrados.py:70`; `chamados/reconciliador.py:1045` (`_turnos.dia_encerrado_motivo`); `colaboradores/tests/test_calendario_hoje_nao_e_falta.py:87`; `chamados/tests/test_orfas_pelo_evento.py:278`, :321, :342, :398, :399, :419 (qualificado por `turnos`).

**`_dia_encerrado_puro`**

Nenhuma chamada real encontrada no inventário AST; só a definição em `ponto/turnos.py:2100` e a docstring :2101. A docstring diz que `dias_encerrados` a lê, mas o corpo atual chama `_dia_encerrado_motivo_puro` em :2164. Ausência global de chamadas dinâmicas: `[nao verificado]`.

**Todos os chamadores de `_dia_encerrado_motivo_puro`**

- `ponto/turnos.py:2096`: omite `celulas`, default `None`.
- `ponto/turnos.py:2102`: repassa `celulas`.
- `ponto/turnos.py:2164`: composto `cels`, também passado em :2165.

**Todos os chamadores da função livre `dias_encerrados`**

Com alimentação:

- `ponto/services/cartorio.py:911`: `{(c.pk,x.data):x for x in cels}`, **sem `turnos`**.
- `colaboradores/services/calendario.py:697`, alias `_dias_encerrados`: `_cels_mes` composto; passa também `turnos=[...]` em :698.
- `ponto/tests/test_c1_dias_encerrados.py:66`, :77: `cels` composto da porta `celulas_do_periodo` (:64), **sem `turnos`**.

Sem alimentação:

- `ponto/turnos.py:2177`.
- `ponto/management/commands/supra_juiz.py:108`.
- `ponto/management/commands/emitir_furo_retroativo.py:62`.
- `ponto/tests/test_c1_dias_encerrados.py:28`, :51.
- `colaboradores/tests/test_calendario_hoje_nao_e_falta.py:73`.

Não confundir com chamadas do método de lote, que não possui argumento `celulas`: `ponto/status_dia.py:196` e `core/services/painel_op.py:334` chamam `carga.dias_encerrados`, definido em `ponto/turnos.py:3009`.

**Todos os chamadores de `_dias_encerrados_puro`**

- `ponto/turnos.py:2147`: composto `cels`.
- `ponto/turnos.py:3012`: composto retornado por `_celulas_de`, cuja construção é explícita em :2976.

**Todos os chamadores de `_turno_aberto_puro`**

- `ponto/turnos.py:2222`: omite `celulas`, default `None`.
- `ponto/turnos.py:2360`: composto `celulas`, construído em :2332–2333 e passado em :2361.

**Q4 já delimitada**

O único teste encontrado que exercita a função livre `dias_encerrados` com `celulas` e sem `turnos` é `test_propriedade_com_as_celulas_ALIMENTADAS`, em `ponto/tests/test_c1_dias_encerrados.py:53`, nas chamadas :66 e :77. Cria célula com DNA 10–14 para vínculo/template noturno (:62–63), mas **não cria batidas nesse teste** (:59–77); afirma motivos contra a singular (:70/:74) e `None` às 15h (:77). Não afirma papel da ata nem identidade de vínculo (:66–77).

**Diferenças adicionais casca/lote**

A casca `turnos_do_colab` calcula DNA por `marcos_dna_periodo(_cid,pad_ini,pad_fim)` **sem `celulas=`** (`ponto/turnos.py:1910`), enquanto o lote usa `marcos_dna_periodo(cid,...,celulas=cels_k)` (:3001). O lote sempre converte para por-data antes do corpo (:3005), enquanto o caminho `dias_encerrados(...,celulas=composto,turnos=None)` repassa composto diretamente (:2148/:1913).

## 31/10 18:55 · sessao 8041d9ecbd96 · - · - · origem=Ronald (tmux)

Chamadas e argumentos:

- Há uma chamada direta a `dias_encerrados` no cartório: `dias_encerrados(c, d0, d1, agora=agora, celulas={(c.pk, x.data): x for x in cels})`; `turnos` e `motivos` não são passados. `d0/d1` vêm da primeira/última célula do julgamento; o mapa usa `(pk,data)`. Resultado fica em cache e `_turno_encerrado(data_)` devolve `.get(data_, False)`. (`app/ponto/services/cartorio.py:888`, `app/ponto/services/cartorio.py:908`, `app/ponto/services/cartorio.py:911`, `app/ponto/services/cartorio.py:912`)
- O arquivo não contém chamada direta a `turnos_do_colab`; as duas chamadas indiretas pertencem à montagem de grade invocada pelo cartório: `eu.montar_grade_prevista_periodo(c, bs, dias=n_dias, ref=ref, agora=agora)`. A grade chama `_tdc_pre(colab, _ini_al, ref, celulas={data:cel ...})`, com ata gravada, e depois `turnos_do_colab(colab, _ini_al, ref, celulas=_cels_juiz)`, também com chaves `data`, mas células copiadas contendo ata sintética de `lampadas/orfas`. (`app/ponto/services/cartorio.py:932`, `app/escala/utils.py:1522`, `app/escala/utils.py:1524`, `app/escala/utils.py:1820`, `app/escala/utils.py:1826`, `app/escala/utils.py:1837`, `app/escala/utils.py:1838`)

Dependências posteriores:

- O resultado de encerramento entra em `_em_curso` e na impressão como `enc=0`, podendo mudar o portão de idempotência. (`app/ponto/services/cartorio.py:1149`, `app/ponto/services/cartorio.py:1151`, `app/ponto/services/cartorio.py:1159`, `app/ponto/services/cartorio.py:1163`)
- O mesmo resultado é passado como `turno_encerrado` ao classificador; quando há teto explícito, passa `True`. O classificador usa esse booleano nos ramos de furo sem batida, fato no dia e furo parcial. (`app/ponto/services/cartorio.py:1222`, `app/ponto/supra_juiz.py:278`, `app/ponto/supra_juiz.py:305`, `app/ponto/supra_juiz.py:308`, `app/ponto/supra_juiz.py:374`, `app/ponto/supra_juiz.py:419`)
- Os códigos resultantes alimentam o veredito efetivo e a lavra; `fatos_do_status` recebe esse mesmo veredito. A porta calcula o status por `status_da_lavra(veredito=v, **fatos)` e escreve `status_dia/status_rotulo` junto do veredito. (`app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:1258`, `app/ponto/services/cartorio.py:1565`, `app/ponto/services/cartorio.py:1569`, `app/ponto/portas/celula.py:698`, `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:773`)
- A ata guarda `lampadas`, com `tipo/hora/acesa/luz/tipo_real`; o “papel da ata” é lido desses dados, não de um campo único `ata['papel']`. O cartório monta a ata atual antes do classificador e depois a entrega à lavra. (`app/ponto/services/cartorio.py:541`, `app/ponto/services/cartorio.py:568`, `app/ponto/services/cartorio.py:1184`, `app/ponto/services/cartorio.py:1188`, `app/ponto/services/cartorio.py:1563`, `app/ponto/services/cartorio.py:1565`)
- O papel anterior participa da grade: `_turnos_pre` permite alinhamento pelo turno fechado; depois `_cels_juiz` fornece a ata da montagem atual para o cálculo dos turnos. (`app/escala/utils.py:1524`, `app/escala/utils.py:1536`, `app/escala/utils.py:1537`, `app/escala/utils.py:1832`, `app/escala/utils.py:1838`)
- `turnos_do_colab` lê o papel em `papel_por_minuto_da_ata`, que consulta `periodos_do_dia(cel, te)`; o pareador recebe esse mapa. Mesmo com chaves `(pk,data)`, o leitor de papel percorre os **valores** e chama o juiz com a célula: não se pode concluir “papel ausente” apenas da forma da chave. (`app/ponto/turnos.py:1937`, `app/ponto/turnos.py:1629`, `app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1990`)
- A diferença de forma afeta a seleção de vínculo: `_turnos_do_colab_puro` transforma cada chave recebida em `(_cid,_d)`; recebendo `(pk,data)`, produz chave aninhada `(pk,(pk,data))`. `vinculo_do_dia` consulta `(pk,data)`, portanto essa consulta não encontra a célula; a seleção passa ao caminho sem geradora visível. (`app/ponto/turnos.py:1954`, `app/ponto/turnos.py:1958`, `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:238`, `app/escala/alimentacao.py:212`)
- O cadastro do classificador usa outro caminho corretamente alimentado: todos os vínculos e o mapa `(pk,data)` são entregues a `cadastro_do_dia`, que chama `vinculo_do_dia`. A grade também chama esse juiz com seu mapa `(pk,data)`. (`app/ponto/services/cartorio.py:900`, `app/ponto/services/cartorio.py:901`, `app/ponto/services/cartorio.py:902`, `app/ponto/supra_juiz.py:161`, `app/escala/utils.py:1252`, `app/escala/utils.py:1255`)
- Outras consultas de vínculo no cartório: `marcos_vencidos` passa a própria célula num mapa `(pk,data)`; emissão de furo e emissão parcial chamam `_vinculo_do_dia(c,data)`, alias de `escala_vigente`. (`app/ponto/services/cartorio.py:36`, `app/ponto/services/cartorio.py:686`, `app/ponto/services/cartorio.py:1503`, `app/ponto/services/cartorio.py:1607`)

Leitores:

- `ponto/juiz_batida.py::periodos_do_dia` lê `celula.ata['lampadas']/['orfas']`, conferindo contra o DNA; é consumido pelo papel dos turnos, motor, export e cadastro versus realidade. (`app/ponto/juiz_batida.py:157`, `app/ponto/juiz_batida.py:160`, `app/ponto/juiz_batida.py:161`, `app/ponto/juiz_batida.py:174`, `app/ponto/turnos.py:1631`, `app/ponto/motor_calculo_v2.py:566`, `app/folha/export.py:1124`, `app/escala/services/cadastro_realidade.py:212`)
- `leitor_celula._celulas_da_ata` transforma `lampada.tipo` em `celula['papel']`; o propositor compara esse papel com `tipo_real` para `TIPO_INCOERENTE`. A grade lavrada também transporta minutos, tipo do dia e veredito. (`app/escala/services/leitor_celula.py:100`, `app/escala/services/leitor_celula.py:106`, `app/escala/services/leitor_celula.py:107`, `app/ponto/propositor.py:66`, `app/ponto/propositor.py:68`, `app/escala/services/leitor_celula.py:345`, `app/escala/services/leitor_celula.py:360`, `app/escala/services/leitor_celula.py:366`)
- `status_dos_dias_lavrados` lê a coluna `status_dia` somente depois do julgamento de relógio pela `CargaDaJanela`; antes disso devolve `previsto/aberto`. `status_do_dia_lavrado` delega ao lote. O painel lê o resultado em `linha['dia']`. (`app/ponto/status_dia.py:170`, `app/ponto/status_dia.py:195`, `app/ponto/status_dia.py:196`, `app/ponto/status_dia.py:197`, `app/ponto/status_dia.py:203`, `app/core/services/painel_op.py:334`, `app/core/services/painel_op.py:336`, `app/core/services/painel_op.py:338`)
- O próprio cartório lê `cel.status_dia` para decidir se a complementação já foi feita. A porta lê o status para preservar/comparar o estado e registrar a alteração. (`app/ponto/services/cartorio.py:727`, `app/ponto/portas/celula.py:697`, `app/ponto/portas/celula.py:703`, `app/ponto/portas/celula.py:723`, `app/ponto/portas/celula.py:763`)

## 31/10 18:28 · sessao fd9f4ba4699d · TRAVA-COBERTURA · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 18:26 · sessao 723f7ac8aa19 · - · - · origem=Ronald (tmux)

Achados confirmados por leitura; execução dos testes, estado do Docker, banco e hook instalado `[nao verificado]`. [nao verificado]

1. **Ainda há disparos de suite fora da trava na cópia:** `app/core/hooks/pre-push.sh:54` define `_teste_irmao` com `docker run` direto; os disparos são arsenal CI em `app/core/hooks/pre-push.sh:82` e tenants em `app/core/hooks/pre-push.sh:87`. CI aponta por default para `juliani_db_test` em `app/config/settings/ci.py:12`; teste_motor crava esse host em `app/config/settings/teste_motor.py:6`. Esse hook ainda extrai senha do container e a põe no argv em `app/core/hooks/pre-push.sh:59` e `app/core/hooks/pre-push.sh:82`. O scanner novo ignora `.sh` de `app/`, pois só chama o scanner shell quando `base == 'bin'`, em `bin/tests/test_trava_teste.sh:130`. Instalação/ativação desse hook antigo `[nao verificado]`; o instalador atual aponta para `bin/pre-push.sh` e instala wrapper em `bin/hooks.sh:10`, `bin/hooks.sh:51`.

2. **rc75 novo dos moldes ainda acaba como CAIU_FATIA.** Os ramos novos escrevem “nao rodou” e saem zero em `bin/molde_fatia/rodar.sh:34`, `bin/molde_fatia/rodar.sh:53`, `bin/molde_fatia/rodar.sh:55`, `bin/molde_autocura/rodar.sh:28`, `bin/molde_autocura/rodar.sh:32`. Porém a esteira exige `green.out`, registra `NAO LANCADA`, termina com rc1 e escreve `fim` em `bin/molde_fatia/esteira.sh:15`, `bin/molde_fatia/esteira.sh:29`; idem autocura em `bin/molde_autocura/esteira.sh:14`, `bin/molde_autocura/esteira.sh:27`. O trap chama o classificador em `bin/fim_de_fatia.sh:30`, `bin/fim_de_fatia.sh:36`; `estado_final` delega para `classificar` em `app/core/esteira_vigia.py:340`. A nova frase não consta no catálogo de infra em `app/core/esteira_vigia.py:81`; com `fim`, cai em `CAIU_FATIA, 'fim sem causa conhecida'` em `app/core/esteira_vigia.py:153`. CAIU_FATIA fica fora do relance em `app/core/esteira_vigia.py:545`.

3. **Outro consumidor de TesteNaoRodou ainda condena:** `_isolar` captura a exceção e devolve `None` em `app/core/esteira_vigia.py:740`; o chamador `classificar` trata todo lado diferente de `'arvore'` como CAIU_FATIA, inclusive `None`, em `app/core/esteira_vigia.py:140`. O vínculo real passa a lambda `_isolar` em `app/core/esteira_vigia.py:1525`.

4. **Outros consumidores de 75 permanecem errados:**
   - Régua perde o rc do processo protegido e converte ausência de `Ran` em `ST=FALHOU`, depois carimba e sai1: `bin/regua.sh:165`, `bin/regua.sh:178`, `bin/regua.sh:196`, `bin/regua.sh:205`.
   - Vigia, se o segundo run `.sozinhos` retorna75, extrai `IDS2` vazio e publica VERDE: `bin/vigia_arvore.sh:106`, `bin/vigia_arvore.sh:113`, `bin/vigia_arvore.sh:121`.
   - Régua de relógio não guarda75: compara conjunto vazio com conhecidos, declara todos CURADOS e sai1, ou sai OK se lista vazia: `bin/regua_relogio.sh:51`, `bin/regua_relogio.sh:58`, `bin/regua_relogio.sh:63`, `bin/regua_relogio.sh:73`, `bin/regua_relogio.sh:76`.
   - Calendário novo guarda75, mas ainda deixa143/125/erro sem veredito chegar à comparação e sugerir CURADOS/OK: `bin/regua_calendario.sh:51`, `bin/regua_calendario.sh:54`, `bin/regua_calendario.sh:59`, `bin/regua_calendario.sh:63`.
   - Isolamento guarda75, mas depois julga apenas pelo grep;143 com saída parcial OK poderia virar verde: `bin/isolamento.sh:37`, `bin/isolamento.sh:38`.
   - No teto, várias rodadas75 terminam com mensagem “ARVORE VERMELHA” e exit1, sem distinção: `bin/isolamento.sh:69`, `bin/isolamento.sh:73`.

5. **O consumidor prometido do --quem não foi migrado.** Handoff consulta só flock e imprime `**LIVRE**`, sem consultar container órfão, em `bin/handoff_sessao.sh:223`, `bin/handoff_sessao.sh:230`. Assim a melhoria de `--quem` não chega a essa saída após SIGKILL; esse efeito decorre da consulta exclusiva a flock em `bin/handoff_sessao.sh:230`.

6. **Censo dos demais disparos protegidos:** suite `bin/suite.sh:96`; régua `bin/regua.sh:165`; pre-push `bin/pre-push.sh:142`; relógio `bin/regua_relogio.sh:51`; vigia primeira/segunda execução via função em `bin/vigia_arvore.sh:83`, `bin/vigia_arvore.sh:106`, confirmação em `bin/vigia_arvore.sh:131`; calendário `bin/regua_calendario.sh:46`; isolamento `bin/isolamento.sh:33`; placar `bin/placar_code.sh:126`; moldes `bin/molde_fatia/rodar.sh:30`, `bin/molde_fatia/rodar.sh:44`, `bin/molde_autocura/rodar.sh:22`; Python `app/core/esteira_vigia.py:781`. A configuração de crons declara o vigia em `app/config/crons.py:277`; agendamento real `[nao verificado]`.

7. **Docker sem trava, para separar de suite:** `bin/molde_fatia/rodar.sh:19` roda `gerar_diagrama` com settings CI e envfile, mas seu handle somente chama `gerar` em `app/core/management/commands/gerar_diagrama.py:949`; uso real de conexão nesse lançamento `[nao verificado]`. `bin/db_teste.sh:53` cria o servidor `juliani_db_test` sem trava e ainda leva senha no argv em `bin/db_teste.sh:55`; é criação do servidor, não `manage.py test`, conforme comando em `bin/db_teste.sh:56`.

8. **Ordem e herança:** cadeia adquire raia→árvore em `bin/trava.sh:60`, `bin/trava.sh:64`, depois régua em `bin/molde_fatia/cadeia.sh:14`, e teste via `bin/regua.sh:165`; pre-push adquire régua antes de teste em `bin/pre-push.sh:80`, `bin/pre-push.sh:142`. O integrador toma raia/árvore em `bin/integrador.sh:32`, régua em `bin/integrador.sh:51`, então Python em `bin/integrador.sh:52`. No ramo de commit local, `exec 8>/tmp/regua.lock` sobrescreve o fd8 que `pegar_arvore` havia usado para cadeia, em `bin/integrador.sh:41` contra `bin/trava.sh:64`; defeito preexistente ao delta anexado `[nao verificado]`.

9. **Fonte do envfile:** único escritor de produção identificado é `teste_envfile`, com `mktemp`/write/mv em `bin/recursos.sh:139`, `bin/recursos.sh:140`, `bin/recursos.sh:143`. A senha nasce por outro estado, `.senha_teste`, em `bin/db_teste.sh:63`; recursos a lê em `bin/recursos.sh:119`, com fallback para printenv em `bin/recursos.sh:122`. O hook antigo ignora essa fonte e deriva outra credencial via printenv em `app/core/hooks/pre-push.sh:59`.

10. **Placar ainda não valida processo:** captura o primeiro número impresso via sed/head em `bin/placar_code.sh:132`; se o teste imprimir `credencial_sem_trilha=0` e falhar depois, a falha não invalida `_cred`. O fallback `nao_medido` só cobre ausência de número em `bin/placar_code.sh:133`; `_n_cred` aceita esse zero em `bin/placar_code.sh:538`.

## 31/10 18:26 · sessao 075407f36985 · - · - · origem=Ronald (tmux)

Achados estáticos; não executei testes nem mutações. Os resultados de mutações abaixo são previsões pelo código `[nao verificado]`. [nao verificado]

1. **rc 75 ainda prende fatia como culpada.** Os moldes gravam “RED/GREEN/MAPA nao rodou” e saem 0 (`bin/molde_fatia/rodar.sh:34`, `:53`, `:55`; `bin/molde_autocura/rodar.sh:28`, `:32`). A esteira vê falta de GREEN, escreve `NAO LANCADA`, termina com rc 1 e `fim` (`bin/molde_fatia/esteira.sh:15-17`, `:29-30`; `bin/molde_autocura/esteira.sh:14-16`, `:27-28`). Nenhuma dessas frases integra `CAUSAS_ARVORE` (`app/core/esteira_vigia.py:78-82`); `classificar` chega a `CAIU_FATIA, 'fim sem causa conhecida'` (`:153`). O trap chama esse classificador e grava `fatia.done` (`bin/fim_de_fatia.sh:30-39`; `app/core/esteira_vigia.py:340`, `:1643-1647`). O relance automático aceita apenas NOVA/CAIU_ARVORE, deixando essa fatia presa (`app/core/esteira_vigia.py:545-546`).

2. **O novo catch de `_isolar` também pode transformar não rodou em culpa.** `_isolar` devolve `None` quando recebe TesteNaoRodou (`app/core/esteira_vigia.py:740-743`), mas seu consumidor classifica qualquer resultado diferente de `'arvore'` como CAIU_FATIA — incluindo `None`, com “GREEN vermelho (nao isolado)” (`:140-144`). A passada efetivamente conecta essas funções (`:1525`).

3. **O integrador pode pousar com selo novo vermelho e log antigo verde.** `_suite` completa descarta o rc de `bin/regua.sh`, procura o `/tmp/regua_*` mais recente e deriva verde exclusivamente desse arquivo (`app/core/integrador_lote.py:375-381`). A régua executa os selos host antes do `docker run` e sai imediatamente quando algum falha (`bin/regua.sh:138-143`); o log da execução atual só é aberto no redirecionamento de `:169`. Assim, falha de um selo novo pode deixar a régua sem log novo e o integrador julgar o último log antigo OK (`app/core/integrador_lote.py:375-381`). O integrador já aplicou e recarregou a cópia antes disso (`:627-639`) e, se lê verde, segue `_subir` (`:706-708`). A ausência de alpine causa explicitamente falha no selo novo (`bin/tests/test_trava_dono_para_filho.sh:21-24`).

4. **A régua ainda converte rc 75 em vermelho.** Ela não captura o rc da trava (`bin/regua.sh:165-170`), interpreta ausência de `Ran` como FALHOU (`:178`), grava `REGUA_STATUS=FALHOU` (`:196-203`) e sai 1 (`:205-206`). Isso perde a distinção exigida de 75 (`bin/regua.sh:165-206`). Para o integrador, o log atual sem nomes normalmente cai na proteção de infraestrutura (`app/core/integrador_lote.py:641-653`); o defeito de log antigo acima permanece (`:375-381`).

5. **A conferência da quarentena protege 75/sem-veredito, mas ainda inventa verdes em outros casos.** `_conferir_quarentena` captura TesteNaoRodou e não solta ninguém (`app/core/integrador_lote.py:785-788`), porém ignora o bool devolvido por `_rodar_testes` e considera verde todo ID sem ocorrência na regex de FAIL/ERROR (`:786`, `:789-791`). Exemplo possível pelo código: rc 143 com linha OK retorna False (`app/core/esteira_vigia.py:791-796`), mas, sem FAIL/ERROR, a conferência libera todos (`app/core/integrador_lote.py:790-791`). Um `ERROR: setUpClass (mod.Classe)` também não coincide com IDs `mod.Classe.test_x`; a comparação é exata (`:790-791`). A liberação exige prova positiva de execução por teste; o código atual usa ausência de falha (`:791`).

6. **Há leituras e escritores da quarentena fora de uma transação comum.** O integrador tira seu retrato `fora` antes de esperar pela trava (`app/core/integrador_lote.py:782`, `:786`); o runner reconsulta a lista posteriormente e seleciona somente testes ainda nela (`app/core/runner_quarentena.py:77-83`). No fim, o integrador libera IDs do retrato original pela ausência de FAIL (`app/core/integrador_lote.py:791`). Separadamente, `quarentenar` acrescenta diretamente à lista (`app/core/esteira_vigia.py:1228-1234`) e `conferir_quarentena` reescreve um retrato via `.novo` (`:1252-1256`), sem lock comum nessas funções. Intercalação pode perder uma inclusão ou liberar ID que não executou nessa conferência `[nao verificado]`.

7. **Os `finally` novos do bisect acertam o caminho TesteNaoRodou, mas não cobrem a cópia.** A exceção da suite desfaz a metade e sobe sem escolher metade (`app/core/integrador_lote.py:801-807`); idem para candidata única (`:810-816`). O chamador só então grava `_nao_julgado` (`:687-690`), cujo estado é CAIU_ARVORE, sem remover a fatia da fila (`:768-774`). Entretanto, `_copiar` fica antes do `try` (`:799-801`, `:809-810`); erro durante a cópia não passa por esses finally. O bool de `_copiar` também é ignorado nessas chamadas, apesar de poder ser False por baseline divergente (`:311-316`). Esse último problema já existia no contexto anterior ao delta fornecido.

8. **CAIU_ARVORE de não julgado ainda herda política de árvore vermelha.** `_nao_julgado` distingue o motivo textual, mas usa o mesmo estado (`app/core/integrador_lote.py:771-773`). `decidir` bloqueia relance de todo CAIU_ARVORE quando o estado global da árvore é VERMELHO, sem distinguir “pista ocupada” (`app/core/esteira_vigia.py:545-548`), e aplica limites/repetição ao mesmo estado (`:551-563`).

9. **Auto-revert publica antes de descobrir se a confirmação pode rodar.** O git revert acontece em `app/core/esteira_vigia.py:1297`, deploy em `:1301-1302`, e só depois `_rodar_testes` obtém recursos e espera pista (`:1305-1306`; `_rodar_testes`:775-790`). O catch novo conserva o revert, grava `ultimo_revert` e pauta conferência manual (`:1303`, `:1307-1313`), portanto não dá falso veredito de cura. Mas a mensagem “APLICADO” não confere rc do deploy, descartado em `:1301-1302`; efetivo estado em produção é `[nao verificado]`. A publicação anterior à confirmação já constava no diff-base fornecido.

10. **Relógio fora da borda permanece na autocura.** `autocura` deriva nome temporário de `datetime.datetime.now()` e instante de `datetime.date.today()` diretamente, em leituras distintas (`app/core/esteira_vigia.py:1433-1435`), enquanto outros caminhos recebem `agora` (`:1276-1281`, `:1347-1351`). O delta não altera essas leituras, mas a nova espera pela trava pode separar ainda mais instante de fabricação e execução (`:1455`; `:781-790`). Impacto em virada de dia é `[nao verificado]`.

Mapeamento do estado Python:

- `TesteNaoRodou`/`PistaOcupada`: definidos em `app/core/esteira_vigia.py:749-754`; produzidos por recursos inválidos (`:762-763`), rc75 (`:791-792`) e ausência de linha de veredito (`:793-795`). Consumidos por `_isolar` (`:742`), `auto_revert` (`:1307`), `curar_por_relogio` (`:1364`), `autocura` (`:1456`), integrador isolamento/bisect (`app/core/integrador_lote.py:660`, `:689`) e quarentena (`:787`).
- CAIU_ARVORE de não julgado: escrito por `_nao_julgado` → `_escrever_done` → `ev.escrever_done` (`app/core/integrador_lote.py:773`, `:281-283`; `app/core/esteira_vigia.py:192-197`); lido por `ler_done`/`estado_do_done`/`passada` e política de relance (`app/core/esteira_vigia.py:176-205`, `:1521-1525`, `:545-563`).
- Quarentena: lê lista em `app/core/integrador_lote.py:781-782`, executa com SO_QUARENTENA (`:786`), runner lê/filtra (`app/core/runner_quarentena.py:77-83`), extrai falhas (`app/core/integrador_lote.py:789-791`), reescreve removendo verdes (`app/core/esteira_vigia.py:1252-1256`). Inclusão ocorre em `app/core/esteira_vigia.py:1226-1238`.

Selo `test_pista_ocupada_nao_e_veredito.py`:

- Mutação mínima prevista vermelha: trocar `raise PistaOcupada(...)` por `return False` em `app/core/esteira_vigia.py:792`; o teste exige exceção com nome PistaOcupada (`app/core/tests/test_pista_ocupada_nao_e_veredito.py:51-58`). Remover o catch da quarentena também falha (`:82-88`; produção `app/core/integrador_lote.py:785-788`) `[nao verificado]`.
- Mutação mínima prevista invisível: remover `rc == 0 and` do retorno em `app/core/esteira_vigia.py:796`. O selo só combina rc0+OK e rc1+FAILED (`app/core/tests/test_pista_ocupada_nao_e_veredito.py:69-71`), não rc1/143+OK `[nao verificado]`.
- Outra mutação invisível: apagar os novos finally do bisect (`app/core/integrador_lote.py:803-805`, `:812-813`). Nenhum dos cinco testes chama `_bisect`/`rodar`; suas chamadas são `_rodar_testes`, `_isolamento` e `_conferir_quarentena` (`app/core/tests/test_pista_ocupada_nao_e_veredito.py:47-88`) `[nao verificado]`.
- O selo substitui todo `subprocess.run` por fake (`app/core/tests/test_pista_ocupada_nao_e_veredito.py:26-35`, `:47-49`); portanto não prova flock real, recursos reais, execução Docker, identidade de dono ou consumidores finais da classificação (`:51-88`).

## 31/10 18:26 · sessao 59b23d8ecc7f · - · - · origem=Ronald (tmux)

Achados da exploração, somente por leitura:

1. **P1 — aquisição pode conceder a vez com órfão ainda vivo.** `docker stop` falho é descartado por `|| true`; não há confirmação de que o container morreu e, sem container sem rótulo, o laço termina e executa o próximo comando. Uma falha de parada pode produzir dois runs. `bin/trava_teste.sh:119`, `bin/trava_teste.sh:126`, `bin/trava_teste.sh:128`, `bin/trava_teste.sh:151`.

2. **P1 — TOCTOU entre observar e lançar.** O flock é adquirido antes das consultas Docker, portanto participantes do mesmo lock são serializados; entretanto `docker ps` em `:115` e o lançamento em `:151` não são operação atômica com o daemon. Um disparador externo pode iniciar suite após a consulta. Outra sequência possível: SIGKILL durante criação, nova aquisição consulta antes de o container anterior aparecer, então ambos os containers nascem. Ocorrência real dessas sequências: `[nao verificado]`. `bin/trava_teste.sh:96`, `bin/trava_teste.sh:115`, `bin/trava_teste.sh:151`.

3. **P1/P2 — handoff continua declarando LIVRE sem conferir órfãos.** Ele consulta diretamente o flock e imprime `LIVRE`; não usa a nova saída do `--quem`. Após SIGKILL, pode contradizer o detector e publicar pista livre com container vivo. `bin/handoff_sessao.sh:223`, `bin/handoff_sessao.sh:230`, `bin/handoff_sessao.sh:231`; detector em `bin/trava_teste.sh:71`, `bin/trava_teste.sh:76`.

4. **Propriedade e sinais.** A chave deriva do basename do lock; o valor é PID mais starttime. A leitura de `/proc` remove o campo comm e usa o índice correspondente ao campo 22. PID reciclado com starttime diferente é considerado dono morto; processo com mesmo PID/starttime conserva o container e recebe 75. Container sem rótulo não é selecionado para `docker stop` na aquisição. `bin/trava_teste.sh:37`, `bin/trava_teste.sh:39`, `bin/trava_teste.sh:47`, `bin/trava_teste.sh:101`, `bin/trava_teste.sh:121`, `bin/trava_teste.sh:126`.

5. **A propriedade é inferida do rótulo, não autenticada.** Qualquer container com essa mesma chave e dono considerado morto pode ser parado, independentemente de imagem/comando; não há filtro adicional de suite/banco. Container com outra chave `hasner.trava.*` também fica fora da detecção de sem rótulo. Colisão concreta ou rótulo forjado em produção: `[nao verificado]`. `bin/trava_teste.sh:52`, `bin/trava_teste.sh:56`, `bin/trava_teste.sh:119`.

6. **SIGKILL após nascimento.** O comando protegido fecha fd9, de modo que o Docker não conserva o flock por esse descritor; o container já criado pode sobreviver. `--quem` encontra qualquer container rotulado desta chave e imprime `ORFAO`; a próxima aquisição testa o dono e tenta pará-lo. `bin/trava_teste.sh:151`, `bin/trava_teste.sh:75`, `bin/trava_teste.sh:121`, `bin/trava_teste.sh:126`. Helpers executados antes do comando protegido não têm o mesmo fechamento explícito de fd9; liberação rigorosamente imediata em qualquer ponto do SIGKILL: `[nao verificado]`, `bin/trava_teste.sh:115`.

7. **TERM/INT/HUP.** O trap consulta containers com chave **e valor do dono corrente**, tenta parar, termina o processo cliente e espera, saindo 143. Há janela quando o sinal chega antes de o container ser visível: consulta vazia, cliente recebe TERM, e a trava não reconsulta o daemon. Garantia de que Docker cancelou criação nesse caso: `[nao verificado]`. `bin/trava_teste.sh:141`, `bin/trava_teste.sh:144`, `bin/trava_teste.sh:146`, `bin/trava_teste.sh:148`.

8. **`--quem` não valida o dono para chamar de ORFAO.** Com lock livre, qualquer container desta chave recebe esse nome, mesmo com PID/starttime vivo; a aquisição distingue esse caso e devolve 75. A saída é conservadora, porém semanticamente diferente entre consulta e aquisição. `bin/trava_teste.sh:75`, `bin/trava_teste.sh:76`, `bin/trava_teste.sh:121`.

9. **Timeout não é teto rigoroso.** O prazo usa relógio de parede e dorme dez segundos sem limitar ao restante; `--espera 2` pode esperar aproximadamente dez segundos quando há suite sem rótulo. Paradas de órfãos também podem consumir dez segundos cada sem testar o limite. Duração observada: `[nao verificado]`. `bin/trava_teste.sh:95`, `bin/trava_teste.sh:126`, `bin/trava_teste.sh:129`, `bin/trava_teste.sh:134`.

Mutações dos dois selos examinados — previsões estáticas, execução `[nao verificado]`:

| Selo | Mutação mínima que deve deixá-lo vermelho | Mutação que pode escapar |
|---|---|---|
| `test_trava_teste.sh` | Trocar `exit 75` da falha do flock por `exit 74`; o selo exige exatamente 75. `bin/trava_teste.sh:99`, `bin/tests/test_trava_teste.sh:233`, `bin/tests/test_trava_teste.sh:236`. | Trocar `if _vivo "$dono"` por `if false`: permite parar container de dono vivo; os casos dinâmicos desse selo não criam container com dono vivo sem flock. `bin/trava_teste.sh:121`, `bin/tests/test_trava_teste.sh:216`, `bin/tests/test_trava_teste.sh:244`. |
| `test_trava_dono_para_filho.sh` | Remover `trap _parar TERM INT HUP`: TERM deixa o container e a asserção de morte falha. `bin/trava_teste.sh:150`, `bin/tests/test_trava_dono_para_filho.sh:52`, `bin/tests/test_trava_dono_para_filho.sh:54`. | A mesma remoção da guarda `_vivo` passa pelos cenários apresentados: eles cobrem TERM do próprio dono, dono morto por KILL e container sem rótulo; não cobrem rótulo de dono vivo/PID reciclado nem falha de `docker stop`. `bin/tests/test_trava_dono_para_filho.sh:45`, `bin/tests/test_trava_dono_para_filho.sh:59`, `bin/tests/test_trava_dono_para_filho.sh:89`. |

Fragilidades adicionais do scanner:

- Em Shell, uma simples substring `trava_teste.sh` em qualquer trecho do corpo torna a função elegível, sem provar que protege cada execução. `bin/tests/test_trava_teste.sh:83`, `bin/tests/test_trava_teste.sh:90`.
- Em Python, qualquer constante terminada em `trava_teste.sh` dentro da função satisfaz a guarda, sem vínculo com o comando executado. `bin/tests/test_trava_teste.sh:117`, `bin/tests/test_trava_teste.sh:120`.
- Um comando executável contendo `pgrep`/`pkill`/`pidof` em qualquer posição da linha lógica é ignorado; não exige que seja uma busca. `bin/tests/test_trava_teste.sh:14`, `bin/tests/test_trava_teste.sh:85`.
- Só percorre `bin` e `app`; exclui diretórios `tests` e não cobre hooks fora desses caminhos nem invocações sem literal `manage.py test`. Erro de sintaxe Python é silenciosamente ignorado. `bin/tests/test_trava_teste.sh:44`, `bin/tests/test_trava_teste.sh:97`, `bin/tests/test_trava_teste.sh:125`, `bin/tests/test_trava_teste.sh:127`.
- Os selos de host são chamados **antes** da aquisição da trava de teste, evitando reentrância nessa ligação. Contudo o selo novo exige Alpine previamente presente e fixa `--cpuset-cpus 4-7`, duplicando a fonte de recursos. `bin/regua.sh:138`, `bin/regua.sh:165`, `bin/tests/test_trava_dono_para_filho.sh:20`, `bin/tests/test_trava_dono_para_filho.sh:46`, `bin/recursos.sh:41`, `bin/recursos.sh:81`.

## 31/10 18:13 · sessao 1a27420ed51b · GRADE-FOLGA-DO-VINCULO · verificador · origem=Code

**A cura específica unifica a validade da `FolgaDia`; não unifica toda pergunta “este dia é folga?”.** Precedência e grade passaram a consultar o mesmo seletor, mas continuam existindo decisões próprias de classificação no calendário e na grade. A Pauta DP desta cura também não está encadeada ao rejulgamento. (`app/ponto/precedencia.py:217`; `app/ponto/precedencia.py:358`; `app/escala/utils.py:1124`; `app/colaboradores/services/calendario.py:108`; `app/chamados/services/passivo_validado.py:438`.)

Certeza **alta** abaixo significa fluxo diretamente identificado; **média**, conclusão por busca ou inferência estática. Execução de testes, mutantes, ensaio de impacto e correspondência Git da cópia aos commits informados: **[não verificado]**. [nao verificado]

1. **Quem escreve, quem lê e qual pergunta foi unificada**

| Campo/função | Quem escreve ou produz | Quem lê |
|---|---|---|
| Folga presa ao vínculo encerrado | Calendário materializado: `gerar_foto_folgas`; manual: `lancar_folga`; edição interna: `_foto_do_dia`. Reparentamento transfere somente as manuais. | Carregador da pessoa inteira → seletor → precedência e overlay da grade. |
| Referências | `app/escala/folgas.py:173`; `app/escala/folgas.py:259`; `app/escala/folgas.py:284`; `app/escala/folgas.py:405` | `app/ponto/precedencia.py:207`; `app/ponto/precedencia.py:217`; `app/ponto/precedencia.py:358`; `app/escala/utils.py:1124` |
| `ata.tipo_dia` | A grade produz `tipo_dia`; `ata_do_dia` copia; cartório entrega à porta; porta persiste a ata. | Leitor da célula; juiz de cobrança `dia_sem_trabalho`. |
| Referências | `app/escala/utils.py:1126`; `app/ponto/services/cartorio.py:581`; `app/ponto/services/cartorio.py:1573`; `app/ponto/portas/celula.py:768` | `app/escala/services/leitor_celula.py:344`; `app/chamados/juizes.py:408` |
| `eh_trabalho` publicado pelo calendário | Previsão inicial vem de `ec.eh_dia_trabalho`; `trabalho_do_dia` aplica o fato humano/feriado; calendário publica o resultado. | `_classificar` e a oferta de “Resolver dia”. | [nao verificado]
| Referências | `app/colaboradores/services/calendario.py:785`; `app/colaboradores/services/calendario.py:840`; `app/colaboradores/services/calendario.py:908` | `app/colaboradores/services/calendario.py:126`; `app/colaboradores/services/calendario.py:202` |
| `folga_humana` | Helper `folga_humana_ou_feriado`, projetado no dict do calendário e nos fatos da lavra. | `trabalho_do_dia`; regra de status da lavra quando o veredito é `indefinida`. |
| Referências | `app/ponto/precedencia.py:179`; `app/colaboradores/services/calendario.py:820`; `app/ponto/status_dia.py:139` | `app/colaboradores/services/calendario.py:103`; `app/ponto/status_dia.py:102` |
| `REGRA_VERSOES` / `versao_para` | Declaração no cartório; leitor calcula a versão da competência. | Impressão de insumos do cartório, que decide se precisa rejulgar. |
| Referências | `app/ponto/services/cartorio.py:110`; `app/ponto/services/cartorio.py:192` | `app/ponto/services/cartorio.py:961`; `app/ponto/services/cartorio.py:1165`; `app/ponto/services/cartorio.py:1175` |
| `grade_espelho_janela` | Monta pela grade comum e aplica o recorte. | PDF, fallback do espelho, mapa de divergência e simulação. |
| Referências | `app/escala/utils.py:2286`; `app/escala/utils.py:2287` | `app/relatorios/pdf_espelho.py:471`; `app/ponto/services/espelho.py:503`; `app/ponto/management/commands/mapa_divergencia.py:104`; `app/ponto/services/simular_dia.py:7` |

**Certeza alta para os caminhos da tabela.** O diff modifica leituras e projeções em memória; a persistência da ata continua pela porta existente. (`app/ponto/portas/celula.py:787`; `app/ponto/portas/celula.py:807`.)

A contagem precisa para a pergunta desta obra é:

- **Um predicado:** `folga_lancada_vale_no_dia`: manual vale; calendário exige o vínculo do dia; `None` não vale. (`app/ponto/precedencia.py:236`.)
- **Um seletor**, que percorre as candidatas até encontrar uma válida. (`app/ponto/precedencia.py:217`.)
- **Dois sítios consumidores:** precedência e overlay da grade. Os dois builders chegam ao overlay. (`app/ponto/precedencia.py:358`; `app/escala/utils.py:1124`; `app/escala/utils.py:770`; `app/escala/utils.py:1794`.)

Para a pergunta ampla, **não há juiz único de todos os sentidos de “folga”**. O registro separa trabalho do vínculo, fato vencedor e status; o calendário ainda responde status por `_classificar`, como pendência declarada de tela. A grade também publica folga em ramos próprios. Isso preexiste à v5 e impede transformar a contagem estreita acima em “um único sítio no sistema”. **Alta.** (`app/core/juizes.py:158`; `app/core/juizes.py:160`; `app/core/juizes.py:173`; `app/core/juizes.py:819`; `app/escala/utils.py:727`; `app/escala/utils.py:989`; `app/escala/utils.py:1751`.)

2. **Portão de versão e código morto da volta 4**

Não encontrei definição ou chamada produtiva de `VERSAO_GRADE_FOLGA` nem de `_ata_abaixo_da_regra_de_folga`; restam referências históricas. O corpo atual de `grade_espelho_janela` monta e recorta, sem consultar versão ou ata. **Média para a ausência por busca; alta para o corpo da função.** (`app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:5`; `app/ponto/services/cartorio.py:178`; `app/escala/utils.py:2286`.)

**Permanece horizonte de rejulgamento automático**, pela versão incorporada à impressão. Isso não seleciona a regra da grade: impressão igual pula o julgamento, salvo `--forcar`. **Alta.** (`app/ponto/services/cartorio.py:366`; `app/ponto/services/cartorio.py:1165`; `app/ponto/services/cartorio.py:1171`; `app/ponto/services/cartorio.py:1175`.)

Não identifiquei função morta da volta 4: `trabalho_do_dia` tem dois chamadores produtivos; `grade_espelho_janela` possui os leitores relacionados acima. `_folga_avulsa` também possui chamador, portanto não é cadáver L-111. A retirada do portão está documentada, mas não há lápide local junto à função atual. **Alta nos chamadores; média na conclusão negativa.** (`app/colaboradores/services/calendario.py:126`; `app/colaboradores/services/calendario.py:840`; `app/ponto/precedencia.py:501`; `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:5`; `app/escala/utils.py:2272`.)

3. **Competência exportada: alcance e preservação**

A v5 permite que **grade viva e ata relavrada mudem**, inclusive abaixo do horizonte. O command seleciona células passadas sem piso de competência, repassa `--forcar`, monta a grade atual e lavra seu `tipo_dia`. **Alta.** (`app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/management/commands/processar_cartorio.py:61`; `app/ponto/management/commands/processar_cartorio.py:65`; `app/ponto/services/cartorio.py:945`; `app/ponto/services/cartorio.py:581`; `app/ponto/services/cartorio.py:1573`.)

Esse caminho pode modificar ata, veredito, status e carimbos; não é uma regeneração do DNA. **Alta.** (`app/ponto/portas/celula.py:773`; `app/ponto/portas/celula.py:800`; `app/ponto/portas/celula.py:805`.)

**FechamentoMensal e TXT ficam fora das escritas diretas desse caminho por construção**, com testemunha adicional no teste. A porta salva a célula e insere evento; não escreve esses dois modelos. Isso não constitui imunidade universal: outro caminho, `recalcular_fechamento_mes`, admite exportadas e atualiza o fechamento. **Alta no caminho direto; média quanto a efeitos indiretos não exercitados.** (`app/ponto/portas/celula.py:630`; `app/ponto/portas/celula.py:639`; `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:99`; `app/ponto/services/fechamento.py:469`; `app/ponto/services/fechamento.py:1128`.)

O teste comprova menos que “registro inteiro intacto”: o hash exclui carimbos e aprovador; `_txt()` observa apenas hash, conteúdo, quantidade de linhas e invalidação do registro antigo. **Alta.** (`app/ponto/portas/regen_exportada.py:37`; `app/ponto/portas/regen_exportada.py:56`; `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:78`.)

**Quem gera Pauta DP:** `_pauta_da_competencia`, no fluxo de passivo validado, monta pago → devido e escreve cabeçalho e filhas pela porta de pautas. Seu chamador vem após REGEN e exige mudança do gravado. Não encontrei chamada correspondente no rejulgamento desta cura; a declaração da v3 apenas promete a Pauta. **Alta no gerador; média na ausência de integração.** (`app/chamados/services/passivo_validado.py:148`; `app/chamados/services/passivo_validado.py:192`; `app/chamados/services/passivo_validado.py:229`; `app/chamados/services/passivo_validado.py:438`; `app/chamados/services/passivo_validado.py:443`; `app/ponto/services/cartorio.py:179`.)

Entrega da Pauta DP referente à diferença específica da v5: **[não verificado]**.

4. **Os RED mordem?**

A tabela descreve **mordida prevista por inspeção**, não execução de mutantes. Resultado RED/GREEN: **[não verificado]**.

| Teste novo | Deve matar | Deixa passar | [nao verificado]
|---|---|---|
| Calendário: manual no velho | Excluir encerrados; publicar `eh_trabalho` cru. | Inverter apenas a condição das folgas de calendário. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:56`.) |
| Calendário: calendário do velho | Aceitar calendário alheio. | Excluir encerrados: elimina justamente a candidata inválida. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:66`.) |
| Calendário: calendário do ativo, geradora velha | Usar ativo em lugar da geradora; aceitar qualquer calendário. | Excluir folgas de encerrados: a candidata pertence ao ativo. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:102`.) |
| Grade: calendário alheio | Aceitar qualquer calendário; inverter correspondência de vínculo. | Excluir encerrados. (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:70`.) |
| Grade: manual do velho | Excluir encerrados; exigir vínculo igual também para manual. | Inverter apenas o comparador dos calendários. (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:75`.) |
| Grade: calendário do vínculo correto | Rejeitar calendário do vínculo do dia. | Excluir encerrados. (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:80`.) |
| Grade: inválida de menor PK + válida | Guardar somente a primeira candidata. | Aceitar qualquer calendário ou inverter o comparador: alguma candidata ainda produz folga. (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`.) |
| Grade: manual fora da janela | Restringir universo aos vínculos da janela/ativos. | Aceitar indiscriminadamente calendários. (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:91`.) |
| Toda competência: premissa de versão | Horizonte errado. | Todos os mutantes somente de grade/folga. (`app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:67`.) |
| Toda competência: setembro | Voltar leitura da ata abaixo da v3. | Excluir encerrados. (`app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:72`.) |
| Toda competência: agosto | Voltar portão para versão `None`. | Excluir encerrados. (`app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:77`.) |
| Toda competência: simulação | Voltar portão nesse caminho. | Excluir encerrados. (`app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:82`.) |
| Toda competência: anti-vacuidade manual | Excluir manual do encerrado; ler a ata antiga. | Aceitar qualquer calendário. (`app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:88`.) |
| Exportada: premissa | Versão/fixture incorreta. | Mutantes no rejulgamento, preservação e render. (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:86`.) |
| Exportada: ata folga → trabalho | Impedir relavra ou restaurar regra antiga na alimentação da ata. | Excluir calendário do encerrado; alterar fechamento junto. (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:92`.) |
| Exportada: gravado/TXT | Alterar campos efetivamente observados; impedir correção da ata. | Alterar carimbos excluídos do hash; criar outra exportação; omitir Pauta DP. (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:97`; `app/ponto/portas/regen_exportada.py:37`.) |
| Exportada: testemunha | Divergência entre grade e ata. | **Ambas erradas como folga em 15/09**; render que leia a ata já corrigida. (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:108`.) |

Os cinco casos de vínculo rodam explicitamente nos dois builders. Os testes sem portão não repetem essa parametrização: restaurar o portão **somente no legacy** pode escapar com a configuração padrão. **Alta.** (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:97`; `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:32`; `app/config/settings/base.py:165`; `app/escala/utils.py:2035`.)

Os dois testes alterados de O219 mordem a declaração literal e o horizonte efetivo da v3; deixam passar portão em render, juiz de folga errado e escritor extra. **Alta.** (`app/ponto/tests/test_o219_versao_da_regra.py:76`; `app/ponto/tests/test_o219_versao_da_regra.py:85`.)

A mordida específica de retirar `--forcar` não está demonstrada: o teste não afirma impressão previamente em dia, e as batidas da fixture podem provocar julgamento por signal. **[não verificado]**. (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:58`; `app/ponto/signals.py:85`; `app/ponto/services/cartorio.py:1171`.)

5. **Contraexemplos**

| Caso | Resposta encontrada e certeza |
|---|---|
| Vínculo encerrado no meio da competência | Dentro da vigência, a geradora pode mandar mesmo inativa; fora dela, não ganha legitimidade por ser geradora. Calendário do antigo após a troca é rejeitado; manual permanece válida. **Alta.** (`app/escala/alimentacao.py:237`; `app/ponto/precedencia.py:238`.) |
| Dois vínculos no mesmo dia | A geradora decide; sem geradora, há desempate entre vínculos que cobrem. Duas `FolgaDia` são permitidas por vínculos diferentes; a inválida de menor PK não esconde a válida. **Alta.** (`app/escala/alimentacao.py:237`; `app/escala/alimentacao.py:253`; `app/escala/models.py:1571`; `app/ponto/precedencia.py:217`.) |
| Manual sobre célula de trabalho | Manual vence: calendário publica `eh_trabalho=False`; sem batida, status folga; overlay limpa células e órfãs. **Alta.** (`app/ponto/precedencia.py:238`; `app/colaboradores/services/calendario.py:103`; `app/colaboradores/services/calendario.py:147`; `app/escala/utils.py:1126`.) |
| Feriado com batida | Se a A2 declarou feriado, calendário publica `False`; turno aberto continua aberto; fechado segue como folga trabalhada. Escala que trabalha em feriado pode conservar faixa trabalho. **Alta.** (`app/colaboradores/services/calendario.py:103`; `app/colaboradores/services/calendario.py:157`; `app/colaboradores/services/calendario.py:159`; `app/ponto/precedencia.py:373`.) |
| Ausência de subtipo folga | **Divergência residual:** A2 classifica `folga_compensatoria`/`feriado_folga` como folga; calendário reconhece fato humano; grade grava `tipo_dia='ausencia'` para slug diferente de falta. Não prova erro financeiro, mas refuta igualdade geral dos rótulos. **Alta.** (`app/ponto/precedencia.py:62`; `app/ponto/precedencia.py:191`; `app/escala/utils.py:1188`; `app/colaboradores/services/calendario.py:820`.) |
| Colaborador sem vínculo e sem outros fatos | Precedência responde sem jurisdição, `faixa=None`; grade legacy e por turno podem publicar folga. Logo ausência de previsão não tem uma única representação. **Alta.** (`app/ponto/precedencia.py:385`; `app/escala/utils.py:987`; `app/escala/utils.py:1751`.) |

Os testes novos não atacam especificamente feriado com batida, ausência de subtipo folga ou colaborador sem vínculo. **Alta por inspeção das fixtures.** (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:29`; `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:41`; `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:44`.)

6. **Classes de reprovação procuradas**

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando registrado | **Residual preexistente:** `_classificar` mantém regra de status, enquanto o registrado é `status_do_dia_lavrado`. O novo `trabalho_do_dia` também repete a seleção feriado/folga humana já reconhecida pelo helper; não duplica o comparador de vínculo. **Alta.** (`app/colaboradores/services/calendario.py:108`; `app/core/juizes.py:173`; `app/core/juizes.py:819`; `app/colaboradores/services/calendario.py:103`; `app/ponto/precedencia.py:189`.) |
| Segundo escritor fora da porta | Não identificado no caminho alterado: ata passa por lavrar/carimbar/salvar. Updates diretos dos testes montam fixtures. **Alta nesse escopo.** (`app/ponto/services/cartorio.py:1573`; `app/ponto/portas/celula.py:807`; `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:74`.) |
| Relógio fora da borda | **Existe, preexistente:** `julgar_celula` recebe `agora`, mas lê `localdate()` e `now()` antes de usá-lo. Calendário também lê outro instante para desfazer retratação. **Alta.** (`app/ponto/services/cartorio.py:1700`; `app/ponto/services/cartorio.py:1705`; `app/ponto/services/cartorio.py:1726`; `app/ponto/services/cartorio.py:1729`; `app/colaboradores/services/calendario.py:341`.) |
| Derivação paralela da fonte única | Corrigida para validade da `FolgaDia`; permanece para classificações amplas e casos sem vínculo/subtipo folga relacionados acima. **Alta.** (`app/escala/utils.py:1124`; `app/escala/utils.py:989`; `app/escala/utils.py:1188`.) |
| Selo/RED sem mordida | Há mordida funcional relevante, mas igualdade ata/grade admite ambos errados; preservação observa subconjunto; Pauta não é afirmada. **Alta estática.** (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:97`; `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:113`.) |
| Ordem de trava | Nenhuma inversão nova demonstrada. Save da célula e evento são atômicos; a ordem Disputa → Colaborador permanece declarada. Concorrência real: **[não verificado]**. (`app/ponto/portas/celula.py:639`; `app/chamados/models.py:841`; `app/chamados/reconciliador.py:1268`.) |
| Pouso antes do pré-requisito | A declaração exige ensaio `--apply` antes do pouso. Evidência v5 de ensaio, suite e pouso não foi fornecida: **[não verificado]**. (`app/ponto/services/cartorio.py:173`.) |
| Fallback | Permanece aceitação de objeto sem `calendario_origem_id` como manual; exceção na previsão vira `None`; espelho conserva fallback para builder. **Alta.** (`app/ponto/precedencia.py:238`; `app/ponto/precedencia.py:369`; `app/ponto/services/espelho.py:500`.) |

**Aceite limitado à inspeção da cura de validade da `FolgaDia`.** Aceite integral exigiria comprovar a entrega da diferença à Pauta DP e a execução dos RED/ensaios v5; esses pontos permanecem **[não verificado]**. (`app/ponto/services/cartorio.py:173`; `app/ponto/services/cartorio.py:179`.)

## 31/10 18:11 · sessao 26e0cdafa60b · E4-7-SAIDA-III · verificador · origem=Code

**Não daria a volta 3 por fechada: há um selo existente que falha após a extração.** Executei somente em memória o método estrutural O65: ele termina com `AssertionError: turnos_do_colab deixou de chamar parear_turnos`. A chamada passou para `_turnos_do_colab_puro`, mas o selo continua procurando-a na casca. (`app/ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:180–187`; `app/ponto/turnos.py:1913,1990`)

**A transformação em lote parece preservar o predicado por inspeção; o novo teste de paridade não certifica equivalência com o corpo antigo.** Ele compara o lote com o singular atual, que delega ao mesmo lote. Execução SQL, suíte completa e RED histórico em `c2717f8c`: **[nao verificado]**. (`app/ponto/status_dia.py:169–170`; `app/core/tests/test_e4_7iii_lote_d4.py:111–122`)

**1. Quem escreve e quem lê**

As funções D4 escrevem respostas em memória; quem persiste `CelulaDia.status_dia/status_rotulo` continua sendo a porta: `_aplicar_status`, chamada por `lavrar_veredito`, com gravação por `salvar_e_emitir`. O painel escreve campos de apresentação. (`app/ponto/status_dia.py:193–208`; `app/ponto/portas/celula.py:701–706,752–775`; `app/core/services/painel_op.py:333,338`)

| Elemento | Corpo/escrita | Leitores/chamadores |
|---|---|---|
| `status_do_dia_lavrado` | Casca que retorna um elemento do lote. (`app/ponto/status_dia.py:157–170`) | Teste E3 e teste novo de paridade; registro vivo continua singular. (`app/ponto/tests/test_e3_status_lavrado.py:200`; `app/core/tests/test_e4_7iii_lote_d4.py:118`; `app/core/juizes.py:173`) |
| `status_dos_dias_lavrados` | Agrupa por data e escreve `out[(pk,data)]`. (`app/ponto/status_dia.py:184–208`) | Singular, painel e testes plural/vazio. (`app/ponto/status_dia.py:170`; `app/core/services/painel_op.py:336`; `app/core/tests/test_e4_7iii_lote_d4.py:114,127`) |
| `_turnos_do_colab_puro` | Ordena candidatos, marca ecos, compõe papel e segmentos, chama pareador. (`app/ponto/turnos.py:1928–1995`) | Casca e `CargaDaJanela.turnos`. (`app/ponto/turnos.py:1913,3003`) |
| `_dias_encerrados_puro` | Escreve mapa data→booleano/motivo pelo juiz existente. (`app/ponto/turnos.py:2158–2168`) | Casca e `CargaDaJanela.dias_encerrados`. (`app/ponto/turnos.py:2147,3012`) |
| `_dia_das_batidas_puro` | Escreve mapa pk→dia, primeiro pelo turno, depois data local para não reclamadas. (`app/ponto/turnos.py:2919–2927`) | Casca e `CargaDaJanela.dia_das_batidas`. (`app/ponto/turnos.py:2911,3018`) |
| `_d4_em_lote` | Preenche `data_turno` e `dia`. (`app/core/services/painel_op.py:329–338`) | Construtor do painel; teste A2 substitui-o por mock. (`app/core/services/painel_op.py:236`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:295`) |
| `linha['dia']` | Nasce `None`; recebe resposta D4 depois do laço. (`app/core/services/painel_op.py:232,338`) | Card lê rótulo e status. (`app/templates/core/_painel_card.html:66–67`) |
| `linha['data_turno']` | Nasce do turno vivo ou hoje; encerrada recebe dia da entrada identificado pela carga. (`app/core/services/painel_op.py:222,330–333`) | Há asserções sobre o campo; o botão Designar não o recebe. (`app/core/tests/test_e4_7iii_painel_e_presenca.py:264`; `app/templates/core/_painel_card.html:69`) |

Cada método de `CargaDaJanela` tem estes escritores/leitores:

| Método | O que produz/carrega | Quem chama |
|---|---|---|
| `__init__` | Células, candidatos, vínculos, escala da ata e caches. (`app/ponto/turnos.py:2944–2965`) | D4 por data e painel por janela. (`app/ponto/status_dia.py:189`; `app/core/services/painel_op.py:327`) |
| `_ultima_de` | Cache do máximo timestamp por colaborador. (`app/ponto/turnos.py:2967–2972`) | Callback fornecido ao corpo do turno. (`app/ponto/turnos.py:3006`) |
| `_celulas_de` | Mapa com chaves `(cid,data)` recortado. (`app/ponto/turnos.py:2974–2976`) | `turnos` e `dias_encerrados`. (`app/ponto/turnos.py:2996,3012`) |
| `celula` | Uma célula, sem carregar novamente. (`app/ponto/turnos.py:2978–2980`) | D4. (`app/ponto/status_dia.py:191`) |
| `turnos` | Universo de batidas e cache dos turnos por colaborador. (`app/ponto/turnos.py:2982–3007`) | D4, `dias_encerrados`, `dia_das_batidas`. (`app/ponto/status_dia.py:195`; `app/ponto/turnos.py:3013,3017`) |
| `dias_encerrados` | Resposta do corpo compartilhado. (`app/ponto/turnos.py:3009–3013`) | D4 e painel. (`app/ponto/status_dia.py:196`; `app/core/services/painel_op.py:334`) |
| `dia_das_batidas` | Resposta do corpo compartilhado. (`app/ponto/turnos.py:3015–3018`) | Painel. (`app/core/services/painel_op.py:330`) |

O inventário de chamadas executáveis das cascas, fora da extração, é:

- **`turnos_do_colab`, núcleo e views:** `app/ponto/turnos.py:778,2096,2148,2179,2909,3041`; `app/ponto/turno_leitura.py:89,95`; `app/ponto/nucleo.py:47`; `app/ponto/views.py:377,1586,3167`; `app/api/views.py:1449`; `app/relatorios/pdf_espelho.py:213,447`; `app/chamados/models.py:1260`; `app/chamados/juizes.py:369,1727`; `app/chamados/detector_anomalias.py:155`.
- **`turnos_do_colab`, escala:** `app/escala/utils.py:1524,1838`; `app/escala/regua_defesa.py:358`; `app/escala/servico_jornada.py:250,392`; `app/escala/services/escala_auto.py:87`; `app/escala/services/impacto_troca.py:18`.
- **`turnos_do_colab`, demais services:** `app/colaboradores/services/calendario.py:301`; `app/colaboradores/services/drawer.py:45`; `app/ponto/services/espelho.py:625`; `app/ponto/services/bordas_realizado.py:66`; `app/ponto/services/teto_na_lista.py:68`; `app/ponto/services/vigia_pareador_celula.py:97`; `app/ponto/services/esmeril_espelho.py:98`; `app/ponto/services/triagem_batida.py:552`; `app/ponto/services/regularizacao.py:61`; `app/ponto/services/passivo_s84.py:82`; `app/chamados/services/materializacao.py:109,204`; `app/chamados/services/disputa_emissao.py:644`.
- **`turnos_do_colab`, commands:** `app/ponto/management/commands/detectar_par_relampago.py:101`; `app/ponto/management/commands/dna_x_batida_real.py:69`; `app/ponto/management/commands/marcar_abandono_provisorio.py:46`; `app/ponto/management/commands/detectar_ausencias.py:336`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`; `app/ponto/management/commands/vigia_plantio_overnight.py:38`; `app/ponto/management/commands/verificar_nucleo.py:29`; `app/ponto/management/commands/e6_oraculo.py:147`; `app/ponto/management/commands/censo_fase_12x36.py:180`; `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124`.
- **`dias_encerrados`:** `app/ponto/turnos.py:2177`; `app/colaboradores/services/calendario.py:697`; `app/ponto/services/cartorio.py:910`; `app/ponto/management/commands/supra_juiz.py:108`; `app/ponto/management/commands/emitir_furo_retroativo.py:62`.
- **`dia_das_batidas`:** `app/api/views.py:1446,2090`; `app/ponto/services/espelho.py:462`.

As views do painel consomem o construtor inteiro, tanto na página como nos lotes visuais. (`app/core/views.py:607,632`)

Chamadas diretas das cascas em testes/selos: [nao verificado]

- **`turnos_do_colab`, ponto:** `app/ponto/tests/test_retratada_geometria.py:39`; `app/ponto/tests/test_turnos_borda_direita.py:37`; `app/ponto/tests/test_pareador_dna_multivig.py:55`; `app/ponto/tests/test_bateria_lavratura.py:1406`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:169,172,176,203,204,214`; `app/ponto/tests/test_vinculo_do_dia_pela_celula.py:144,156`; `app/ponto/tests/test_pareador_dna.py:50`; `app/ponto/tests/test_o280_bateria_da_lei.py:3010`; `app/ponto/tests/test_f9d_n1_caracterizacao_eco_flush.py:96`; `app/ponto/tests/test_turnos_do_colab_troca_escala.py:53`.
- **`turnos_do_colab`, outros apps:** `app/relatorios/tests/test_pdf_le_o_juiz_do_dia.py:61`; `app/relatorios/tests/test_pdf_sem_regra_propria.py:94`; `app/escala/tests/test_ata_x_turno_entrada.py:82`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:96,199`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:76,90`; `app/escala/tests/test_saida_valida_vira_orfa.py:92,158,196`; `app/api/tests/test_espelho_app_fonte_e_geometria.py:322`; `app/chamados/tests/test_orfas_pelo_evento.py:391`.
- **`dias_encerrados`:** `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:73`; `app/ponto/tests/test_c1_dias_encerrados.py:28,51,66,77`.
- **`dia_das_batidas`:** `app/ponto/tests/test_continuacao_turno_aberto.py:30,37`; `app/relatorios/tests/test_pdf_sem_regra_propria.py:129`; `app/api/tests/test_espelho_app_fonte_e_geometria.py:296`.

Há também consumidores estruturais e instrumentação: O65 procura a chamada direta; B5-j procura os alimentadores; testes de perfil substituem a casca; contrato PDF conta referências. (`app/ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:180`; `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:67`; `app/ponto/tests/test_haiku_turno_invariante.py:64,75,84,102`; `app/ponto/tests/test_col129_teto_vira_lista.py:104–109`; `app/chamados/tests/test_profile_e53.py:67`; `app/chamados/tests/test_selo_teto_modal.py:105`; `app/chamados/tests/test_profile_placar_rodinha.py:101`; `app/relatorios/tests/test_pdf_nao_calcula.py:37,52`)

Registro, medidor e `bin/` leem referências/metadados: registro D4 singular e geometria; medidor exclui o corpo plural; leitor host executa esse medidor; censo de turno nomeia a família. (`app/core/juizes.py:173,177,204`; `app/core/medida_leitor_do_dia.py:296`; `bin/leitor_dia_estado.py:32–35`; `bin/esmeril_turno_censo.py:63`; `bin/tests/juizes_antes_da_trava.txt:54`)

**2. Paridade das cargas**

Não identifiquei contraexemplo determinístico **lote × corpo anterior transcrito**, com cadastro/batidas estáveis e os contratos de alimentação respeitados; a equivalência executada contra o histórico integral permanece **[nao verificado]**. O predicado atual conserva os quatro caminhos: sem célula, relógio, lavrado e não lavrado. (`app/ponto/status_dia.py:191–208`)

| Carga | Comparação |
|---|---|
| **Células e chaves** | A porta devolve `{(cid,data):cel}`; a casca converte para `{data:cel}`; o lote separa por colaborador e faz a mesma conversão para o corpo do turno. Para encerramento volta a fornecer chaves `(cid,data)`. (`app/escala/alimentacao.py:41–46`; `app/ponto/turnos.py:1882–1884,2951–2955,2974–2976,3005,3012`) |
| **Janela com pad** | Ambas usam `ini−2` até `fim+1`. O D4 abre uma carga `[data,data]` por data distinta, preservando a janela singular. (`app/ponto/turnos.py:1832–1845,1860,2948`; `app/ponto/status_dia.py:188–189`) |
| **Escalas candidatas** | Mesma porta: o singular já delegava ao plural. Mesmos limites `pad_ini..fim`, vigência declarada e geradoras em `mais`; ordenação por `data_inicio` ocorre no corpo compartilhado. (`app/escala/alimentacao.py:116–127,153–154`; `app/ponto/turnos.py:1885–1886,1928,2957–2958`) |
| **Escalas da ata** | Mesma `_escalas_da_ata`: último vínculo na ordem colaborador/ativa/início. Casca chega por `papel_por_minuto_da_ata`; lote pré-carrega. A dívida dessa escolha por período continua declarada. (`app/ponto/turnos.py:1536–1559,1591,1937,2963`) |
| **Batidas: universo e janela** | Casca filtra vivas diretamente; lote usa `batidas_apuraveis`, com os mesmos limites por data. Não aparece filtro adicional por aprovação. (`app/ponto/turnos.py:1861–1864,2871–2879,2990–2993`) |
| **Ordem** | Casca herda colaborador/timestamp; lote ordena cid/timestamp. Dentro de um colaborador a ordem é timestamp; o modelo declara unicidade colaborador/timestamp. (`app/ponto/models.py:105–106`; `app/ponto/turnos.py:2993`) |
| **Carimbos e cópias** | Lote copia antes do corpo; `_marca_ecos_flush` atua nessas instâncias. O corpo copia novamente por segmento antes do pareador, isolando `_intra_dur` e outros carimbos entre segmentos. (`app/ponto/turnos.py:1934,1977–1992,3004`) |
| **Última batida** | Mesmo máximo da vida inteira no universo vivo. Casca agrega um colaborador; lote agrega todos uma vez. (`app/ponto/turnos.py:1896–1901,2967–2972`) |
| **`marcos_dna_periodo`** | Casca consulta preguiçosamente a janela; lote fornece as células dessa mesma janela, evitando SQL no provedor. (`app/ponto/turnos.py:1907–1911,2999–3002`; `app/escala/utils.py:96–107`) |
| **Vínculos do encerramento** | Ambas carregam todos os vínculos com `tipo_escala` e passam pelo mesmo `vinculo_do_dia` e `_dia_encerrado_motivo_puro`. (`app/ponto/turnos.py:2145–2148,2164–2165,2960–2962,3012–3013`) |

**Existe contraexemplo entre leitores, sem recorrer ao turno de 61 horas da triagem.** Tome o colaborador da fixture `b`: ativo, 12x36 07–19, âncora em 17/09/2026; E às 07h e S às 19h; célula trabalha/DNA 07–19, sem ata, veredito ou status; agora local 20h de 17/09. (`app/core/tests/test_e4_7iii_painel_e_presenca.py:50–59`; `app/core/tests/test_e4_7iii_lote_d4.py:64,75–81`)

- O painel tem presença `encerrado`, o previsto já terminou e o D4 devolve **`previsto`, fonte `nao_lavrado`**, porque a data ainda é hoje. (`app/core/services/painel_op.py:192–194,334–338`; `app/ponto/turnos.py:2113–2122`; `app/ponto/status_dia.py:207–208`)
- O calendário recebe zero células da ata, não preenche `celstat_map`, vê batidas e turno fechado e cai em **`sem_certidao`**. Essa é inferência do fluxo; ensaio com banco **[nao verificado]**. (`app/escala/services/leitor_celula.py:146–148,330–338,354–365`; `app/colaboradores/services/calendario.py:475–496,799–815,152–154`)
- O espelho classifica separadamente: com batidas, **`ok`**, ou **`atencao`** se o motor marcar inconsistência; nenhum dos dois é `previsto`. A classificação exata do motor nessa fixture é **[nao verificado]**. (`app/ponto/services/espelho.py:159–164,183,666–670`)

Essa divergência entre leitores é compatível com a migração E4 ainda pendente no registro e no medidor; não demonstra regressão causada pelo lote. (`app/core/juizes.py:169–173`; `app/core/medida_leitor_do_dia.py:320–331`)

**3. Cascas e chamadores antigos**

Por comparação estática com o delta fornecido, a extração preserva assinaturas, defaults, cargas e predicados das **três cascas**; preservação executada de todas as queries contra `973753a2`: **[nao verificado]**. (`app/ponto/turnos.py:1848–1913,2125–2149,2882–2913`)

- `turnos_do_colab(c,i,f,celulas=None)` continua aceitando células por **data**; sem alimentação carrega células, candidatos, batidas e ata; mantém os provedores preguiçosos de última batida e DNA. Mesmo com células fornecidas, o provedor DNA da casca continua consultando por conta própria quando acionado. (`app/ponto/turnos.py:1852,1882–1913,1937`)
- `dias_encerrados(...,agora=None,celulas=None,turnos=None,motivos=False)` continua carregando vínculos mesmo quando recebe células e turnos; com `turnos` fornecidos evita refazer o pareamento. (`app/ponto/turnos.py:2125,2140–2149`)
- `dia_das_batidas(...,turnos=None)` continua consultando as batidas vivas com `.only('id','timestamp')`, inclusive quando recebe os turnos. (`app/ponto/turnos.py:2907–2913`)

A assimetria antiga permanece: `turnos_do_colab` espera `{data:cel}`, enquanto `dias_encerrados` documenta `{(cid,data):cel}` e repassa `celulas` à primeira se `turnos` não vierem. A extração não tornou os formatos intercambiáveis. (`app/ponto/turnos.py:1852,1954,2130,2145–2148`)

**4. Custo e mordida**

**Painel: teto constante de queries em N; lote arbitrário: cresce com datas distintas.** O painel usa uma janela fixa e só encaminha ao D4 datas aprovadas dentro dela; o plural abre uma carga por data distinta. Portanto não é correto prometer queries constantes no número de pares quando cada par traz outra data. (`app/core/services/painel_op.py:327–336`; `app/ponto/turnos.py:2162–2167`; `app/ponto/status_dia.py:188–189`)

Não encontrei retorno a SQL por colaborador nos provedores da carga: última batida é um agregado global memoizado; DNA usa células já carregadas; candidatos, vínculos e ata são carregados em lote com `tipo_escala`. Há consultas **condicionais por carga** para geradoras faltantes e última batida, portanto “constante” significa teto, não contagem idêntica para qualquer distribuição de dados. (`app/ponto/turnos.py:2957–2972,2999–3006`; `app/escala/alimentacao.py:123–127`)

**O RED de custo morde estruturalmente a volta ao singular por linha:** exige 1/50/500 encerrados, D4 em todos, status `ok` e igualdade das contagens; o singular atual construiria uma carga por chamada. O teste mede a segunda montagem, após aquecimento. Execução desse RED: **[nao verificado]**. (`app/core/tests/test_e4_7iii_lote_d4.py:33–61`; `app/ponto/status_dia.py:170,189`)

A MEDIDA antiga agora também exige igualdade entre 3 e 6; o teste irmão sem D4 substitui `_d4_em_lote`, e o filtro continua sendo cobrado separadamente. (`app/core/tests/test_e4_7iii_painel_e_presenca.py:293–328`)

**O novo teste de paridade tem mordida limitada:** pode detectar contaminação entre colaboradores ou diferenças entre executar juntos e separados, mas deixa passar erro compartilhado pelo plural de um e muitos. Exemplo: trocar `previsto` por `aberto` no ramo de relógio conserva a fonte `relogio`; ambas as chamadas novas concordariam e a asserção final das quatro fontes continuaria satisfeita. (`app/ponto/status_dia.py:170,198–201`; `app/core/tests/test_e4_7iii_lote_d4.py:114–122`)

Há expectativas semânticas independentes nos testes E3 antigos; portanto a deficiência é do **novo certificado de paridade contra o corpo anterior**, não ausência total de testes de status. (`app/ponto/tests/test_e3_status_lavrado.py:209,217,240–249`)

**5. Classes de reprovação**

| Classe | Resultado |
|---|---|
| **Segundo juiz D4** | Não identificado: registro continua singular, e ele delega ao único corpo plural. Os dois carregadores chamam os mesmos corpos do turno/encerramento/dia da batida. (`app/core/juizes.py:173`; `app/ponto/status_dia.py:170`; `app/ponto/turnos.py:1913,2147,2911,3003,3012,3018`) |
| **Segundo escritor persistente** | Não identificado no delta: coluna continua aplicada e salva pela porta; painel altera dict. O censo AST do selo escritor, executado em memória, retornou zero violações segundo seu critério. (`app/ponto/portas/celula.py:705,773–775`; `app/core/services/painel_op.py:333,338`; `app/ponto/tests/test_selo_veredito_so_pela_porta.py:181–194`) |
| **Relógio nos novos corpos** | `CargaDaJanela` não lê agora/hoje; `_d4_em_lote` recebe ambos. O plural tem `timezone.now()` se `agora=None`, herdando o fallback singular; no painel isso não dispara, pois o instante capturado na borda é repassado. (`app/ponto/turnos.py:2944–3018`; `app/core/services/painel_op.py:51–52,236,313,336`; `app/ponto/status_dia.py:182–183,207`) |
| **Relógio na cadeia inteira** | Não está todo na borda: há `date.today()` herdado em `intervalo_cadastrado`, chamado por `periodos_do_dia`, usado pelo leitor da ata. Impacto concreto dessa leitura nos resultados desta obra: **[nao verificado]**. (`app/ponto/juiz_batida.py:125,270`; `app/ponto/turnos.py:1631`) |
| **Derivação paralela** | Não nasceu outro predicado D4 no carregador; continuam derivadores antigos de calendário/espelho, declarados como dívida. (`app/ponto/status_dia.py:195–208`; `app/ponto/turnos.py:3003,3012,3018`; `app/core/medida_leitor_do_dia.py:320–331`) |
| **Selo sem mordida** | Custo tem mordida; paridade não protege erro compartilhado contra o antigo; O65 ficou vermelho por âncora desatualizada. (`app/core/tests/test_e4_7iii_lote_d4.py:47–61,111–122`; `app/ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:180–187`) |

**B5-j vê os alimentadores atuais da casca e da carga**, porque reconhece o nome literal `_turnos_do_colab_puro` e ambos estão no próprio `turnos.py`; a carga monta o universo dentro do método que chama o corpo. Uma substituição em memória da porta por query crua nesse método foi detectada pelo analisador. (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:67,114–127,143`; `app/ponto/turnos.py:1913,2990–3003`)

**Não vê todo futuro chamador possível do corpo:** alias do novo puro não é resolvido; além disso, `_producao` só inclui arquivos contendo o texto `parear_turnos`. Um módulo apenas com `_turnos_do_colab_puro`, ou sua chamada por alias, escap

[truncado em 20000 de 23776 caracteres]

## 31/10 18:11 · sessao a752e4c3abf6 · - · - · origem=Ronald (tmux)

Análise estática; execução dos testes e mutantes: **[não verificado]**. As referências abaixo usam os caminhos completos definidos na primeira coluna. [nao verificado]

| Teste | Mutante que deve matar | Mutante que deixa passar | [nao verificado]
|---|---|---|
| `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:56` | Excluir folgas do vínculo encerrado do carregador; voltar a publicar `eh_trabalho` cru da célula: exige `folga` e `False` (:58–64). **Certeza alta.** | Inverter só o comparador de vínculo das folgas de calendário: a fixture é manual (:58; `app/ponto/precedencia.py:238`). **Alta.** |
| Mesmo arquivo, :66 | Aceitar toda folga de calendário independentemente do vínculo: exige `falta`/`True` (:68–73). **Alta.** | Excluir folgas de vínculos encerrados: apagar essa candidata inválida preserva o resultado esperado (:39–43,68–73). **Alta.** |
| Mesmo arquivo, :102 | Usar vínculo ativo em vez da geradora; aceitar qualquer calendário: geradora é velha, folga pertence ao ativo (:90–109). **Alta.** | Excluir folgas dos vínculos encerrados: a candidata pertence ao ativo (:104). **Alta.** |
| `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:70` | Aceitar calendário alheio / inverter `==` do juiz (:71–73; `app/ponto/precedencia.py:240`). **Alta.** | Excluir vínculo encerrado: remover calendário inválido conserva `trabalho` (:48–55,71–73). **Alta.** |
| Mesmo arquivo, :75 | Excluir folga manual do vínculo encerrado; exigir correspondência de vínculo também para manual (:76–78). **Alta.** | Inverter só o comparador dos calendários: manual sai antes desse comparador (`app/ponto/precedencia.py:238–240`). **Alta.** |
| Mesmo arquivo, :80 | Rejeitar calendário do vínculo do dia / inverter correspondência do vínculo (:81–83; `app/ponto/precedencia.py:240`). **Alta, inferência estática.** | Excluir vínculo encerrado: candidata pertence ao novo (:51–55,81). **Alta.** |
| Mesmo arquivo, :85 | Considerar apenas a candidata de menor PK e descartá-la sem procurar a seguinte (:86–89; `app/ponto/precedencia.py:217–220`). **Alta.** | Aceitar qualquer calendário; excluir velho; inverter comparador: cada mutante ainda encontra alguma candidata que produz `folga` (:86–89). **Alta.** |
| Mesmo arquivo, :91 | Restringir universo aos vínculos da janela ou ativos: manual está presa ao velho fora da janela (:48–52,92–94). **Alta.** | Aceitar indiscriminadamente calendários: fixture só contém manual (:92). **Alta.** |
| `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:67` | Alterar o horizonte efetivo de setembro/agosto (:69–70). **Alta.** | Todos os mutantes só de folga/render: esse teste só chama `versao_para` (:68–70). **Alta.** |
| Mesmo arquivo, :72 | Restaurar leitura da ATA abaixo da v3; aceitar calendário alheio: ATA diz `folga`, exige `trabalho` (:73–75). **Alta.** | Excluir velho do universo: a candidata inválida some, mas o esperado continua `trabalho` (:43–55,73–75). **Alta.** |
| Mesmo arquivo, :77 | Restaurar portão também para versão `None` (:78–80). **Alta.** | Excluir velho: conserva `trabalho` (:43–55,78–80). **Alta.** |
| Mesmo arquivo, :82 | Restaurar portão no caminho `simular_dia.grade_dia` (:83–86; `app/ponto/services/simular_dia.py:5–8`). **Alta.** | Excluir velho: conserva `trabalho` (:43–55,84–86). **Alta.** |
| Mesmo arquivo, :88 | Restaurar leitura da ATA abaixo da v3; excluir manual do encerrado: ATA diz `trabalho`, exige `folga` (:89–93). **Alta.** | Aceitar qualquer calendário: caso examinado é manual (:57,89–93). **Alta.** |
| `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:86` | Alterar versão de setembro ou deixar de criar a exportação/ATA antiga (:88–90). **Alta.** | Mutantes no rejulgamento, preservação e render: não chama `_rejulgar` (:86–90). **Alta.** |
| Mesmo arquivo, :92 | Impedir relavra da exportada ou restaurar portão no caminho que alimenta a lavra: exige ATA `trabalho` (:93–95). **Alta.** | Excluir calendário do vínculo encerrado: também resulta em `trabalho` (:54–65,93–95). **Alta.** |
| Mesmo arquivo, :97 | Alterar campos cobertos pelo hash do fechamento ou pelos quatro campos do TXT; impedir correção da ATA (:99–106). **Alta.** | Alterar campos excluídos do hash; deixar de gerar Pauta DP: não há assert de Pauta, e o hash exclui carimbos (`app/ponto/portas/regen_exportada.py:37–38,56–74`; teste :97–106). **Alta.** |
| Mesmo arquivo, :108 | Fazer grade e ATA discordarem; estragar `trabalho` do dia 14/09 (:113–115). **Alta.** | **Ambos responderem erradamente `folga` em 15/09**: só exige igualdade nesse dia. Também passa um render que volte a ler a ATA **depois** da relavra (:110–115). **Alta.** |
| `app/ponto/tests/test_o219_versao_da_regra.py:67` — alterado | Remover v3 ou mudar seu horizonte: compara tupla literal (:76). **Alta.** | Voltar portão no render ou excluir encerrados: nenhuma consulta de grade/folga (:67–76). **Alta.** |
| Mesmo arquivo, :78 — alterado | Declarar v3 sem `versao_para` efetivamente devolver 3 em outubro (:85–93). **Alta.** | Portão de render, juiz de folga errado ou escritor extra: testa somente versão por data (:85–93). **Alta.** |

Os cinco casos da grade de vínculo são efetivamente herdados pelos dois builders mediante `override_settings(True/False)`; isso tem cobertura explícita do rollback (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:97–104`). Os testes sem portão não repetem essa parametrização; com a configuração padrão `True`, um portão restaurado **só no legacy** pode escapar (`app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:32–93`; `app/config/settings/base.py:165`; `app/escala/utils.py:2033–2039`). **Certeza alta sobre a lacuna, execução [não verificado].**

A anti-vacuidade do fechamento é útil: exige hash distinto de nenhum registro e exige a ATA corrigida antes de comparar preservação; não prova identidade de todas as colunas, pois o hash exclui campos e `_txt()` compara apenas `hash_sha256`, `conteudo`, `total_linhas`, `invalidada_em` (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:78–81,99–106`; `app/ponto/portas/regen_exportada.py:37–38`). **Alta.**

A mordida específica de `--forcar` permanece **[não verificado]**: a fixture só força a ATA antiga, sem afirmar que a impressão já está em dia; contudo as batidas disparam julgamento por signal, de modo que não se pode concluir estaticamente que retirar `forcar` passaria (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:58–74`; `app/ponto/signals.py:85–93,131–135`; `app/ponto/services/cartorio.py:1171–1177`).

Os novos testes não exercitam feriado com batida, ausência de subtipo folga ou colaborador sem vínculo; suas fixtures possuem vínculos e usam `FolgaDia` manual/de calendário, portanto mutantes restritos a essas situações não são atacados por estes casos (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:29–109`; `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:41–94`; `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:35–93`; `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:44–115`). **Alta.**

## 31/10 18:10 · sessao 98021fb68766 · - · - · origem=Ronald (tmux)

Alta certeza por leitura estática; execução dos casos e resultados em produção [não verificado].

- **Pergunta estreita — “esta FolgaDia vale neste dia?”:** **1 predicado**, `folga_lancada_vale_no_dia`: `None` não vale, manual vale, calendário exige mesmo vínculo (`app/ponto/precedencia.py:236`). **1 seletor**, que percorre todas as candidatas (`app/ponto/precedencia.py:217`). **2 consumidores diretos** do seletor: precedência (`app/ponto/precedencia.py:358`) e overlay da grade (`app/escala/utils.py:1124`). O carregador também é único, consulta a pessoa inteira e guarda todas por data em ordem de PK (`app/ponto/precedencia.py:207`).

- **Pergunta ampla — “este dia é folga?”:** não cabe contar apenas esse predicado. O registro separa trabalho do vínculo, fato vencedor e status (`app/core/juizes.py:158`, `app/core/juizes.py:160`, `app/core/juizes.py:173`). O calendário continua classificando status por `_classificar`, pendência já declarada da família tela (`app/colaboradores/services/calendario.py:108`; `app/core/juizes.py:819`). A grade continua publicando folga em outros ramos além do overlay (`app/escala/utils.py:727`, `app/escala/utils.py:989`, `app/escala/utils.py:1751`), e a lavra tem seu próprio mapeamento para status folga (`app/ponto/status_dia.py:105`). Logo “um sítio” é verdadeiro somente para a validade da **FolgaDia lançada**, não para todos os significados de folga.

- **Duplicação residual no novo helper:** `folga_humana_ou_feriado` já reconhece feriado ou faixa folga humana (`app/ponto/precedencia.py:188`); `trabalho_do_dia` volta a testar faixa feriado e faixa folga junto do booleano (`app/colaboradores/services/calendario.py:102`). Com o dict produzido pelo fluxo normal, é redundante; o helper recebe também a previsão e projeta `False` sobre ela (`app/colaboradores/services/calendario.py:105`, `app/colaboradores/services/calendario.py:819`). Não é segundo predicado de validade de vínculo, mas mantém uma regra local de dia de trabalho no calendário, enquanto o registro nomeia `EscalaColaborador.eh_dia_trabalho` (`app/core/juizes.py:158`).

- **Não há função morta da volta 4 identificada:** `trabalho_do_dia` tem chamadas de produção em `_classificar` e na publicação (`app/colaboradores/services/calendario.py:126`, `app/colaboradores/services/calendario.py:840`). `grade_espelho_janela` tem consumidores por alias no PDF, espelho e mapa, além de simulação (`app/relatorios/pdf_espelho.py:193`, `app/relatorios/pdf_espelho.py:471`, `app/relatorios/pdf_espelho.py:539`; `app/ponto/services/espelho.py:496`, `app/ponto/services/espelho.py:503`; `app/ponto/management/commands/mapa_divergencia.py:104`; `app/ponto/services/simular_dia.py:7`). `_folga_avulsa` tem chamador no veto (`app/ponto/precedencia.py:501`).

- **Portão:** busca textual encontrou `VERSAO_GRADE_FOLGA`, `_ata_abaixo_da_regra_de_folga` e o AST antigo somente em comentários/documentação, sem definição Python viva; a retirada está descrita em `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:5` e `app/ponto/services/cartorio.py:178`. O corpo vivo da porta chama builder e clipa, sem ler ata ou versão (`app/escala/utils.py:2286`). `versao_para` conserva consumidor no cartório (`app/ponto/services/cartorio.py:961`), não render nos resultados da busca. Ausência universal de referência dinâmica [não verificado]. Não há lápide local da função removida em torno da porta; só essa descrição histórica (`app/escala/utils.py:2272`; `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:5`).

**Escritores e fontes**

- `FolgaDia` armazena vínculo/data/origem e unicidade vínculo+data (`app/escala/models.py:1553`, `app/escala/models.py:1557`, `app/escala/models.py:1571`). Foto de calendário é escrita por `gerar_foto_folgas`, com `bulk_create` e origem preenchida (`app/escala/folgas.py:150`, `app/escala/folgas.py:173`); manual passa por `lancar_folga`, com veto e relavra (`app/escala/folgas.py:257`); edição interna por `_foto_do_dia` (`app/escala/folgas.py:267`, `app/escala/folgas.py:284`). Reparentamento escreve apenas vínculo e exclui calendário (`app/escala/folgas.py:405`, `app/escala/folgas.py:435`). O Plano chama essas portas (`app/escala/views_plano_folgas.py:124`).

- Grade escreve `tipo_dia` no builder base (`app/escala/utils.py:727`, `app/escala/utils.py:752`), no por turno (`app/escala/utils.py:1751`, `app/escala/utils.py:1757`), no overlay folga (`app/escala/utils.py:1126`) e ausência (`app/escala/utils.py:1182`, `app/escala/utils.py:1188`). A precedência escreve apenas dict de fatos, incluindo `folga_lancada` booleano, sem persistência (`app/ponto/precedencia.py:398`, `app/ponto/precedencia.py:408`).

**Contraexemplos**

1. **Encerrado no meio da competência:** geradora válida dentro da vigência ganha mesmo inativa; geradora fora da vigência devolve `None`, sem escolher outra (`app/escala/alimentacao.py:237`). Sem geradora, desempate `(ativa, data_inicio)` entre quem cobre (`app/escala/alimentacao.py:253`). Folga de calendário do antigo depois da troca é rejeitada pelo predicado; manual é aceita (`app/ponto/precedencia.py:238`). Certeza alta.

2. **Dois vínculos/duas folgas no mesmo dia:** modelo permite duas FolgaDia desde que vínculos diferentes (`app/escala/models.py:1571`); carregador guarda ambas e seletor pula a inválida (`app/ponto/precedencia.py:207`, `app/ponto/precedencia.py:217`). Vínculo do dia vem da geradora ou do desempate acima (`app/escala/alimentacao.py:237`). Certeza alta.

3. **Ausência de subtipo folga:** **divergência residual confirmada no fluxo**. `folga_compensatoria` e `feriado_folga` entram na faixa folga da A2 (`app/ponto/precedencia.py:62`), e helper humano reconhece subtipo (`app/ponto/precedencia.py:191`). A cobertura aprovada não exclui esses tipos (`app/ponto/turnos.py:2487`, `app/ponto/turnos.py:2734`); a grade transforma todo slug diferente de falta em `tipo_dia='ausencia'` (`app/escala/utils.py:1187`). Calendário transforma esse fato humano em `eh_trabalho=False` e status folga sem batida (`app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:147`). Assim, não há igualdade geral grade `tipo_dia` × precedência `faixa` × calendário `status`; essa diferença preexiste ao diff, que não altera `_marcar_ausencias` (`app/escala/utils.py:1131`). Certeza alta por fluxo; execução [não verificado].

4. **Sem vínculo e sem outros fatos:** A2 diz sem jurisdição/`faixa=None` (`app/ponto/precedencia.py:385`); grade legacy diz folga (`app/escala/utils.py:987`) e por turno também publica folga quando não montou dia de turno (`app/escala/utils.py:1730`, `app/escala/utils.py:1751`). Certeza alta. Manual histórica ainda pode ser aceita mesmo com vínculo do dia `None` (`app/ponto/precedencia.py:238`); sem nenhum vínculo em toda a pessoa, não existirá FolgaDia associada por essa FK (`app/escala/models.py:1553`).

**Classes estruturais adicionais**

- **Relógio fora da borda:** por turno e porta usam `agora` recebido (`app/escala/utils.py:1242`, `app/escala/utils.py:2281`). Legacy e single conservam `timezone.localdate()` se chamados diretamente sem ref (`app/escala/utils.py:964`, `app/escala/utils.py:658`). Wrapper sempre lhes fornece ref (`app/escala/utils.py:2038`); portanto presença residual alta certeza, introdução pelo diff não demonstrada.

- **Fallback:** novo predicado usa `getattr(..., None)` e aceita objeto sem `calendario_origem_id` como manual (`app/ponto/precedencia.py:238`); comentário ainda diz “calendário passa True”, embora calendário agora deixe carregar objetos pelo carregador (`app/ponto/precedencia.py:230`; `app/colaboradores/services/calendario.py:617`). Precedência engole exceção de aritmética e vira `previsto=None` (`app/ponto/precedencia.py:367`). Esses comportamentos não são prova de erro no caso normal, mas são fallback vivo.

- **Derivação paralela:** a regra estreita antiga foi removida do overlay, que delega ao mesmo seletor (`app/escala/utils.py:1115`, `app/escala/utils.py:1124`). A regra ampla segue espalhada nos sítios acima; a própria pendência de status do calendário permanece registrada (`app/core/juizes.py:819`).

## 31/10 18:10 · sessao 86998d0f1cc8 · - · - · origem=Ronald (tmux)

Auditoria estática, sem executar testes nem acessar banco/log. Certeza **alta** = fluxo diretamente lido; **média** = conclusão negativa por busca/caminho; execução real de RED/suite/ensaio = **[não verificado]**. [nao verificado]

1. **Exportada pode mudar na célula/ata, sem trocar DNA por esse caminho — alta.** `processar_cartorio` seleciona todas as células passadas da empresa, sem piso por competência, e repassa `--forcar`: `app/ponto/management/commands/processar_cartorio.py:49`, `:61`, `:65`. O cartório monta a grade viva em `app/ponto/services/cartorio.py:945`; `ata_do_dia` copia `tipo_dia` dessa grade em `:581`; a lavra usa essa ata em `:1571`, `:1573`; salva ata/veredito e carimbos pela porta em `:1593`. O escritor físico da ata é `app/ponto/portas/celula.py:735`, `:767`; `carimbar` reúne campos e chama `salvar_e_emitir` em `:805`, `:807`; save + evento estão no mesmo atomic em `:639`. Não há regeneração de DNA nesse fluxo: campos persistidos estão delimitados em `:773`, `:805`.

2. **Fechamento/TXT intactos: propriedade estrutural deste caminho, com testemunha de teste; não imunidade universal — alta/média.** Cartório grava pelas portas acima; a porta salva `CelulaDia` e cria `EventoCelula`, `app/ponto/portas/celula.py:630`, `:640`. O teste exerce o command real e confere mudança de ata junto à manutenção do hash e TXT, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:83`, `:99`, `:103`, `:104`, `:106`. Contudo, “só REGEN pode alterar o fechamento” não é verdade universal da árvore: `recalcular_fechamento_mes` admite exportadas e tem escritor canônico direto `FechamentoMensal.objects.filter(...).update(...)`, `app/ponto/services/fechamento.py:469`, `:516`, `:1128`. A trava do `FechamentoMensal.save` só cobre status aprovado e é contornada deliberadamente pelo update canônico, `app/ponto/models.py:492`, `:498`; `app/ponto/services/fechamento.py:1115`.

3. **Pauta DP da cura não está conectada — média/alta.** A promessa está apenas na declaração da v3: `app/ponto/services/cartorio.py:179`. Código real de pago x devido existe em `app/chamados/services/passivo_validado.py::_pauta_da_competencia`, `:148`; monta “pago -> devido” em `:178`, `:192`, grava cabeçalho e filhas pela porta `escrever` em `:229`, `:233`. Chamador vem de outro fluxo, plantio do passivo seguido de REGEN, e só gera essa pauta se mudou o hash, `:427`, `:438`, `:442`, `:443`. REGEN devolve antes/depois, hashes, campos alterados e réguas em `app/ponto/portas/regen_exportada.py:216`; não cria a pauta por si. Logo não se pode afirmar que rejulgar pela v5 automaticamente entrega Pauta DP; entrega dessa diferença específica = **[não verificado]**. Há comentário antigo explicitando que Pauta é ato próprio posterior, `app/ponto/services/cartorio.py:155`.

4. **Portão de versão não sobrevive como seleção de regra/render — alta/média.** Busca de símbolos em todos os `.py` encontrou uso produtivo de `REGRA_VERSOES` e `versao_para` só no cartório: declaração `app/ponto/services/cartorio.py:110`, validação `:181`, leitor puro `:192`, cache `:959`. Versão entra na impressão em `:366`, consumida nos ramos nunca-bateu `:1013` e normal `:1165`; igualdade da impressão pula sem `--forcar`, `:1018`, `:1175`. Portanto há horizonte de relavratura automática, não seleção de regra da grade. `VERSAO_GRADE_FOLGA`, `_ata_abaixo_da_regra_de_folga` e `OsQuatroLeitoresChegamPeloPortaoTest` aparecem apenas em documentação histórica, `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:7`, `:15`; nenhuma definição/chamada viva encontrada. A afirmação negativa vem de busca, portanto certeza média.

5. **Testes da exportada têm mordida parcial — alta por inspeção, resultado RED [não verificado].** [nao verificado]
   - `test_MORDE_rejulgado...`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:92`, deveria matar a restauração de regra velha no escritor/ata, pois exige folga→trabalho. Sozinho deixa passar mutação que altera fechamento/TXT junto.
   - `test_o_gravado...`, `:97`, exige a mudança de ata e invariância dos observados. Deixa passar mutante que toca só campos excluídos do hash: `atualizado_em`, `aprovado_em`, `aprovado_por`, `previsto_em`, `derivado_em` etc., `app/ponto/portas/regen_exportada.py:37`, filtrados em `:56`. Não é igualdade da linha inteira.
   - `_txt`, teste `:79`, compara apenas `(hash_sha256, conteudo, total_linhas, invalidada_em)` do registro antigo. Deixa passar criação de nova exportação vigente e mudança de outros metadados. [nao verificado]
   - `test_a_testemunha...`, `:108`, exige só igualdade entre ata/grade no dia corrigido e trabalho no velho; mutante que faz ambos responderem folga no dia corrigido passaria essa função isolada, `:113`.
   - `test_premissa...`, `:86`, não prova correção, só fixture/versão.

6. **Relógio fora do parâmetro injetado já existe — alta, preexistente ao diff.** `julgar_celula(..., agora=None)` recebe relógio em `app/ponto/services/cartorio.py:1700`, mas calcula `hoje = _tz.localdate()` em `:1705` e compara `_tz.now()` com fim do turno em `:1726`; só depois usa o `agora` fornecido em `:1729`. Uma simulação com `agora` histórico cruza dois relógios. No command, relógio está na borda, `app/ponto/management/commands/processar_cartorio.py:38`. `versao_para` é puro e sem relógio, `app/ponto/services/cartorio.py:205`.

7. **Ordem de trava: nenhum lock novo no diff; sem inversão demonstrada — média.** A ordem da casa é Disputa→Colaborador, `app/chamados/models.py:841`; consumidor de eventos declara que não toma esses locks por conta própria, `app/chamados/reconciliador.py:1268`. A porta da célula faz atomic curto save+evento, `app/ponto/portas/celula.py:639`. Concorrência real e deadlock = **[não verificado]**.

8. **Pré-requisitos de pouso continuam sem prova disponível — alta quanto à exigência.** A própria declaração v3 exige ensaio `--apply` na sombra antes do pouso, com dias rejulgados/movimento de minutos/FORA-DO-HORIZONTE, `app/ponto/services/cartorio.py:173`. A regra de pista exige suite inteira verde antes de opus final e proíbe Codex aprovar pouso sozinho, `CLAUDE.md:549`, `:552`. Os anexos concedem mudança de lei; não fornecem execução desses requisitos. RED/GREEN v5, suite v5, ensaio v5, reversão e opus final = **[não verificado]**. RELATO contém números históricos da v4 e referências a logs, mas não equivalem a ensaio v5, `app/docs/RELATO.md:22`, `:32`.

## 31/10 18:08 · sessao 55f75766a99e · - · - · origem=Ronald (tmux)

**Paridade estática das cargas:** não achei contraexemplo determinístico entre lote e corpo antigo para uma fotografia estável e entradas válidas; execução contra banco/histórico integral permanece [nao verificado]. As cargas seguem as mesmas portas e janelas:

- **Células:** `celulas_do_periodo` produz `{(cid,data):cel}`; a casca converte para `{data:cel}`, e o lote faz a mesma conversão por colaborador. Pad comum: `ini−2` até `fim+1`. `app/escala/alimentacao.py:41-46`; `app/ponto/turnos.py:1832-1845`, `1882-1884`, `2951-2955`, `2974-2976`, `3005`.
- **Candidatos:** casca e lote usam a mesma implementação plural; janela `pad_ini..fim`, geradoras incluídas por `mais`, mesmos filtros e ordenação. O corpo comum reordena por `data_inicio`. `app/escala/alimentacao.py:116-127`, `153-154`; `app/ponto/turnos.py:1885-1886`, `1928`, `2957-2958`.
- **Ata:** a casca chega a `_escalas_da_ata` por `papel_por_minuto_da_ata`; o lote pré-carrega a mesma função. `app/ponto/turnos.py:1556-1559`, `1590-1591`, `1937`, `2963`.
- **Batidas:** mesmo filtro de retratadas e mesma janela. A ordenação por colaborador/timestamp é equivalente dentro de um colaborador; não há desempate explícito entre timestamps iguais. `app/ponto/models.py:105`; `app/ponto/turnos.py:1861-1864`, `2871-2879`, `2990-2993`. Divergência concreta causada por esse empate: [nao verificado].
- **Carimbos:** lote copia antes de `_marca_ecos_flush`, e o corpo comum copia novamente por segmento antes do pareador. `app/ponto/turnos.py:1934`, `1977-1981`, `3004`.
- **Última batida:** mesmo universo vivo da vida inteira; casca agrega individualmente, lote agrega todos uma vez. `app/ponto/turnos.py:1896-1901`, `2967-2972`.
- **DNA:** casca consulta preguiçosamente; lote fornece as células completas e faz zero queries nesse provedor. `app/ponto/turnos.py:1907-1911`, `2999-3002`; `app/escala/utils.py:96-107`.
- **Vínculos de encerramento:** ambas carregam todos os vínculos com `tipo_escala` e executam o mesmo corpo. `app/ponto/turnos.py:2145-2148`, `2164-2165`, `2960-2962`, `3012-3013`.

**Cascas:** as assinaturas/defaults permanecem e a extração preserva estaticamente os carregamentos, formatos documentados e predicados anteriores. `turnos_do_colab` espera células por `data`; `dias_encerrados` espera chaves `(cid,data)`. A incompatibilidade desses dois formatos ao repassar células sem fornecer turnos já existia no corpo anterior apresentado; não foi introduzida pela extração. `app/ponto/turnos.py:1848-1853`, `1913`, `1954`, `2125-2131`, `2145-2148`, `2907-2913`.

**Custo:** há teto constante de queries por janela, não igualdade absoluta para qualquer distribuição de dados: a segunda query dos candidatos e o agregado da última batida são condicionais. O lote também cresce com o número de datas distintas. Não identifiquei provedor novo que consulte por colaborador dentro de `CargaDaJanela`. `app/escala/alimentacao.py:123-127`; `app/ponto/turnos.py:2967-2972`, `2999-3006`; `app/ponto/status_dia.py:188-196`.

**Mordida dos testes:** [nao verificado]

- Custo exige 1/50/500 encerrados, todos com D4 e status `ok`, e igualdade das contagens; retorno ao singular por linha acrescentaria queries. Mordida estática identificada; execução RED real [nao verificado]. `app/core/tests/test_e4_7iii_lote_d4.py:47-61`.
- Paridade não prova equivalência com 973753a2: o singular atual chama o próprio lote. Uma regressão compartilhada que preserve as fontes passa — por exemplo, trocar o resultado `'previsto'` por `'aberto'` no ramo de relógio. O teste compara as duas formas e exige somente o conjunto de fontes. `app/ponto/status_dia.py:169-170`, `198-201`; `app/core/tests/test_e4_7iii_lote_d4.py:111-122`.

**Contraexemplo painel × calendário, normal e herdado:** use o colaborador da fixture `b`: 12x36 ativo 07–19, âncora 17/09/2026; E às 07h, S às 19h; célula `trabalha=True`, DNA 07–19, sem ata/veredito/status; agora local 20h. Esse cadastro e essas batidas já estão descritos nas fixtures. `app/core/tests/test_e4_7iii_painel_e_presenca.py:50-59`; `app/core/tests/test_e4_7iii_lote_d4.py:64`, `75-81`.

Nesse cenário, o painel marca `encerrado`, passa pelo encerramento do previsto e recebe D4 **`previsto/nao_lavrado`**, pois a data é hoje. `app/core/services/painel_op.py:192-194`, `332-338`; `app/ponto/turnos.py:2113-2122`; `app/ponto/status_dia.py:207-208`.

O calendário recebe células vazias da ata, não preenche `celstat_map`, vê E/S e turno fechado e cai em **`sem_certidao`**. `app/escala/services/leitor_celula.py:146-148`, `330-338`, `354-365`; `app/colaboradores/services/calendario.py:475-496`, `715-726`, `788-815`, `152-154`. Isso é inferência determinística do fluxo; ensaio com banco [nao verificado]. O ramo D4 de hoje já está no corpo antigo fornecido, e calendário/espelho estão fora do delta: diferença herdada, não regressão demonstrada da v3. O antecedente antigo depende do diff fornecido, sem arquivo local histórico: [nao verificado].

No espelho, a view usa `montar_semanas`, cujo status com batidas é `ok`, ou `atencao` se houver inconsistência — ambos distintos de `previsto`; a classificação exata do motor para essa fixture não foi executada. `app/ponto/views.py:183-184`; `app/ponto/services/espelho.py:159-164`, `666-670`.

## 31/10 18:07 · sessao 7dbdc6ddabbe · - · - · origem=Ronald (tmux)

Inventário de produção completo enviado nas mensagens anteriores, com os chamadores das três cascas, puros, métodos de `CargaDaJanela`, painel, commands, views e testes. [nao verificado]

Pontos adicionais para consolidar:

- O registro continua apontando ao singular em `app/core/juizes.py:173`; o singular delega integralmente ao plural em `app/ponto/status_dia.py:170`. O contrato verifica existência da referência, preservada, em `app/ponto/tests/test_contract_juiz_celula.py:65`.
- Escrita persistente de `status_dia/status_rotulo`: `app/ponto/portas/celula.py:705`, pela lavra declarada em `app/ponto/portas/celula.py:709`; D4 só lê a coluna em `app/ponto/status_dia.py:203`.
- Escrita de `linha['dia']`: inicial `None` em `app/core/services/painel_op.py:232`, preenchimento em `app/core/services/painel_op.py:338`; HTML lê status/rótulo em `app/templates/core/_painel_card.html:66`.
- Escrita de `linha['data_turno']`: inicial em `app/core/services/painel_op.py:222`, correção em `app/core/services/painel_op.py:333`. O botão Designar não recebe esse campo em `app/templates/core/_painel_card.html:69`; datas do modal saem do relógio do navegador e misturam data UTC com hora local em `app/templates/core/painel_operacional.html:294`.
- A atualização automática perde `status=`: URL contém apenas `fragment` e empresa em `app/templates/core/painel_operacional.html:147`. A view mantém filtro bruto no contexto em `app/core/views.py:669`, embora o serviço normalize-o em `app/core/services/painel_op.py:62`.
- Cada lote visual reconstrói o painel inteiro em `app/core/views.py:607`, depois fatia os postos em `app/core/views.py:613`; constante em queries não significa custo total independente do número de requisições de lote.
- Espelho continua com classificação própria em `app/ponto/services/espelho.py:152`; calendário também em `app/colaboradores/services/calendario.py:93`. Por exemplo sintético, uma lavra `'irregular'` mostrada pelo painel não pode aparecer com a mesma palavra no espelho: o D4 lê `'irregular'` em `app/ponto/status_dia.py:203`, enquanto espelho retorna `'atencao'` ou `'ok'` havendo batidas sem ausência em `app/ponto/services/espelho.py:159`. Execução desse cenário [nao verificado].
- B5-j atual vê casca e `Carga.turnos` porque ambas chamam literalmente o corpo em `app/ponto/turnos.py:1913` e `app/ponto/turnos.py:3003`. Entretanto, alias do novo corpo não é resolvido: condição cobre apenas `parear_turnos` em `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:71`. E o censo de arquivos continua exigindo texto `parear_turnos` em `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:143`; módulo só com `_turnos_do_colab_puro` escaparia.
- Outro selo perde cobertura para o nome plural: `PROIBIDOS` do cron só lista singular em `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:125`; o teste usa essa lista em `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:148`.
- Medidor e seu leitor host seguem a mesma implementação: `app/core/medida_leitor_do_dia.py:296` acompanha o corpo plural; `bin/leitor_dia_estado.py:35` chama o censo; `app/core/tests/test_medida_leitor_do_dia.py:74` cobra exclusões mortas. Não há referência de chave singular fixa nesse teste; a contagem usa chaves encontradas em `app/core/medida_leitor_do_dia.py:365`.
- Não execute `bin/tests/test_juiz_novo_tem_corte.sh` nesta auditoria: ele pode escrever sua base em `bin/tests/test_juiz_novo_tem_corte.sh:32` e `bin/tests/test_juiz_novo_tem_corte.sh:52`.

## 31/10 18:07 · sessao d0acfab94381 · - · - · origem=Ronald (tmux)

Conclusões dos selos e classes de reprovação:

- **Custo tem mordida estrutural contra singular por linha:** cria cumulativamente 1/50/500 colaboradores, exige todas as linhas encerradas e todos os D4 presentes com `ok`, mede a segunda montagem real e cobra igualdade das contagens. Voltar ao singular atual por linha cria `CargaDaJanela` por chamada, portanto volta a consultar por linha. A execução real/RED em `c2717f8c` é [nao verificado]. Referências: `app/core/tests/test_e4_7iii_lote_d4.py:33`, `:40`, `:47`, `:55`, `:57`, `:60`; `app/ponto/status_dia.py:170`, `:189`.

- **Paridade nova não prova equivalência com 973753a2:** o teste compara lote atual com singular atual, e o singular atual chama exatamente o lote com um par. Pode pegar contaminação entre colaboradores/datas, mas erro comum ao carregador plural de um e muitos passa. Sua cobertura só exige o conjunto de quatro fontes; não exige valores esperados independentes em cada par. Referências: `app/core/tests/test_e4_7iii_lote_d4.py:111`, `:118`, `:122`; `app/ponto/status_dia.py:169`. Existem expectativas independentes nos testes E3 antigos, portanto não afirmar ausência total de testes semânticos: `app/ponto/tests/test_e3_status_lavrado.py:209`, `:217`, `:240`, `:243`, `:249`.

- **B5-j vê os dois alimentadores atuais:** adiciona o nome `_turnos_do_colab_puro`, e ambos seus chamadores estão em `turnos.py`, arquivo que contém `parear_turnos`; Carga carrega as batidas dentro do método que chama o puro. Mutação exclusivamente em memória substituindo a porta de Carga por `Batida.objects` foi denunciada na linha 2990. Referências: `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:67`, `:114`, `:143`; `app/ponto/turnos.py:2990`, `:3003`.

- **B5-j não vê “todo universo” irrestritamente:** `_producao` só inclui arquivo contendo a string `parear_turnos`; um novo módulo que somente importe/chame `_turnos_do_colab_puro` fica fora. `_apelidos` resolve alias apenas de `parear_turnos`; alias do puro passa. Seu teste novo não testa alias nem seleção de produção. Demonstração em memória de import `..._turnos_do_colab_puro as puro` com query crua devolveu `([], [])`. Referências: `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:71`, `:143`, `:187`. É lacuna adicional ao gap interprocedural já documentado em `:26`.

- **Censo B5-j puro executado sem Django/escrita:** zero crus, seis copiadores; teto continua oito. Isso aceita cópias manuais e deixa folga para duas novas, em vez de garantir porta única. Critério e teto: `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:122`, `:155`, `:169`.

- **Não há segundo juiz D4 de predicado:** registro vivo continua apontando ao singular, que delega ao único corpo plural; puros compartilhados alimentam as duas cargas. Referências: `app/core/juizes.py:173`; `app/ponto/status_dia.py:170`, `:195`; `app/ponto/turnos.py:3003`, `:3012`, `:3018`. `JUIZES.md:74` continua consistente com a entrada registrada.

- **Não há segundo escritor persistente introduzido:** D4 só monta resposta; painel altera campos do dict de apresentação. A porta aplica `status_dia/status_rotulo` e salva no ato da lavra. Censo AST do selo escritor, executado em memória sem runner/Django, retornou zero violações. Referências: `app/ponto/status_dia.py:193`, `:204`; `app/core/services/painel_op.py:333`, `:338`; `app/ponto/portas/celula.py:703`, `:773`; critério em `app/ponto/tests/test_selo_veredito_so_pela_porta.py:171`, `:181`, mordida em `:259`.

- **Relógio:** Carga não consulta relógio; `_d4_em_lote` recebe `hoje/agora` e os repassa; painel captura uma vez. Plural tem fallback `timezone.now()` quando `agora=None`, movido do singular segundo o diff fornecido, e `localdate(agora)` usa esse instante. Não é leitura por linha nem relógio novo no carregador. Referências: `app/ponto/turnos.py:2944`, `:3009`; `app/core/services/painel_op.py:51`, `:313`, `:334`, `:336`; `app/ponto/status_dia.py:182`, `:207`.

- **Medidor com chave movida funciona:** rodado apenas como módulo puro via `compile/exec`, censo deu `numero=5`, `status=5`, `valor=0`, e listas `novos/sumidos/sitios_mudaram/exclusoes_mortas/ilegiveis` vazias. A exclusão plural está viva com dois literais detectados (`previsto/sem_certidao`); singular delegado deixa de produzir palavras. Referências: `app/core/medida_leitor_do_dia.py:296`, `:350`, `:392`, `:406`; `app/ponto/status_dia.py:170`, `:207`. Isso não certifica procedência: limitações explícitas do detector incluem indireção, relógio sem palavra, templates/JS e troca de palavras mantendo contagem — `app/core/medida_leitor_do_dia.py:337`.

## 31/10 18:06 · sessao 36c105c9bd16 · F1C4-3-EMISSORES · censo · origem=Code

**A classificação sustentada pelos anexos é: (b) para os três leitores e X1/X2; (c) para RED e as duas pendências de sombra; (e) para a decisão documental do BLOQUEIA 1, os rascunhos e o estado do BACKLOG.** É uma classificação dos trechos fornecidos; eventual cobertura por perguntas anteriores não anexadas permanece **[não verificado]**.

As citações de `.q/.r` e triagem abaixo reproduzem a colagem: seu arquivo físico e sua numeração são **[não verificado]**. As referências ao código têm arquivo:linha da cópia.

1. **BLOQUEIA 1 — (e) NÃO PERGUNTADO: formalizar a interpretação estreita do aval.** A restrição a `trabalha=None` está explícita no código; o aval registrado diz “pergunta so em competencia aberta; exportada -> Pauta DP”. A PARADA 3 exige publicar a decisão contestável, com números, reversão e prova. (`app/chamados/services/fabrica_da_celula.py:149`; `app/chamados/services/fabrica_da_celula.py:153`; `app/docs/PROMPTS.md:1719`; `CLAUDE.md:454`)

   **Pergunta:** v12 apresenta como avais “JUIZ dia_sem_trabalho” e “F1C4-3-NONE-COBRA”; o item 5 pergunta se os documentos “descrevem o que o codigo faz hoje”. Não cobra conciliar o aval das **11:5x** com L-050/O44 nem publicar essa escolha pela PARADA 3. **Resposta:** v12, item 5, examina preenchimento do tipo, `None`, ata corrente e `_protesta`; não denuncia essa decisão documental ausente. **[não verificado]**

   A v8.r viu “A exceção do limbo exportado pode ser contornada pelo lote”, mas esse é outro achado: bypass do caminho existente. Isso não demonstra que tenha visto a falta de publicação da interpretação. O CONTESTÁVEL em `ESTACIONADO.md`, item 4, consta somente do opus fornecido. **[não verificado]**

2. **FILA — `flip_auto.py:212` — (b) VIU E A TRIAGEM DESCARTOU.** O código exige véspera `trabalha=True` nesse ramo. (`app/ponto/services/flip_auto.py:212`)

   **Pergunta v12.3:** “qualquer outro sitio em chamados/ e ponto/ que decida ‘dia sem trabalho’ ou ‘dia de trabalho’ […] liste-os”. **Resposta v12.3:** “`flip_auto` […] no noturno exige véspera `True` para recuar”; qualifica como “decisão geométrica entre dia/véspera”. **Triagem 3:** “DESCARTADO p/ esta obra — respondem o PREVISTO […] ou outra pergunta”. Portanto, o leitor foi identificado; a necessidade de uma linha individual de encaminhamento não foi denunciada expressamente. **[não verificado]**

3. **FILA — `auditar_invariantes_chamados.py:1542` — (b) VIU E A TRIAGEM DESCARTOU.** A seleção pula célula ausente ou `trabalha is not True`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1542`)

   **Pergunta v12.3:** “qualquer outro sitio […] liste-os”. **Resposta v12.3:** “auditor de chamados (`not trabalha`/`is not True`)”, citando inclusive `:1542`; distingue seu universo de invariantes/diagnósticos da pergunta de cobrança. **Triagem 3:** descarte por “PREVISTO […] ou outra pergunta”. Novamente, houve identificação do leitor, sem denúncia expressa da falta de linha individual. **[não verificado]**

4. **FILA — `declaracao_texto.py:90–91` — (b) VIU E A TRIAGEM DESCARTOU, com motivo específico.** O trecho trata os tipos projetados; o veto adicional a tipo diferente de trabalho está em `:95`. (`app/chamados/services/declaracao_texto.py:90`; `app/chamados/services/declaracao_texto.py:95`)

   **Pergunta v12.1:** “Para cada leitor […] mesma autoridade […] ou tem predicado proprio?”. **Resposta v12.1:** “sobrevive o veto próprio `tipo_dia != 'trabalho'`”. **Triagem 1b:** “DESCARTADO p/ esta obra — pergunta de MARCOS (quais marcos faltam), ja na fila F1C4-3-NONE-LEITORES / EMISSORES-FILA […] vai ao opus”. A triagem considerou suficiente o encaminhamento genérico; o opus cobra a pendência nominal. **[não verificado]**

5. **FILA — rascunhos `c5c43360`/`fbf3882d` sem LEI-AKITA no histórico — (e) NÃO PERGUNTADO.** O postmortem anterior já registra essa classificação para `c5c43360` e promete incluir mensagens de commit na pergunta futura. (`app/docs/RELATO.md:14`)

   **Pergunta v12:** “Leia o codigo na copia”; não solicita examinar mensagens dos commits. **Resposta:** não apresenta auditoria das mensagens desses dois rascunhos. O pedido genérico de “qualquer outro defeito no delta” não explicita esse tema. A ausência efetiva da linha em ambos os históricos, especialmente `fbf3882d`, permanece **[não verificado]**.

6. **FILA — RED dos quatro emissores sem log encontrado — (c) FORA DO ALCANCE DEMONSTRADO.** Os quatro casos existem no código; isso comprova presença dos testes, não execução histórica em vermelho. (`app/chamados/tests/test_f1c4_3_emissores.py:80`; `:100`; `:118`; `:140`)

   **Perguntas:** v12.4 e v8.3 pedem “qual mutacao minima […] o deixa vermelho”, não fornecem o log histórico desses quatro emissores. **Respostas:** v12 declara “Execução dos testes Django e dos mutantes completos: [não verificado]”; v8 também limita suas conclusões à análise do código. **[não verificado]** [nao verificado]

   O RELATO afirma “RED fbf3882d evidenciado”, mas essa declaração documental não substitui o log procurado pelo opus. (`app/docs/RELATO.md:140`) O log e seu resultado efetivo permanecem **[não verificado]**.

7. **FILA — sombra medida em `c5c43360`, não na árvore que pousa — (c) FORA DO ALCANCE DEMONSTRADO.** A obrigação de sombra com chamados novos por emissor antes do pouso está documentada. (`app/docs/BACKLOG.md:833`)

   **Pergunta v12.5:** documentos “descrevem o que o codigo faz hoje?”. **Resposta v12.5:** “As quantidades e resultados de sombra registrados no BACKLOG são declarações documentais; sua confirmação independente permanece [não verificado]”. **Triagem 6:** afirma medição “pelo cartorio real (`v12_sombra.log`)”. Esse log não foi fornecido; a atribuição à árvore `c5c43360` vem do opus e permanece **[não verificado]**.

8. **FILA — segunda passada da sombra sem log — (c) FORA DO ALCANCE DEMONSTRADO.**

   **Pergunta v8.3:** pede analisar os testes novos; **resposta v8.3:** “A segunda passada escreve outra pauta” descreve o efeito previsto de uma mutação no teste, não uma segunda execução registrada da sombra de produção. **Resposta v12.5:** mantém a confirmação independente da sombra como “[não verificado]”. Portanto, essa menção à segunda passada dos testes não comprova a segunda passada exigida pelo opus. **[não verificado]** [nao verificado]

9. **FILA — BACKLOG F1C4-3-EMISSORES “em construcao” — (e) NÃO PERGUNTADO quanto à atualização do estado.** O texto está presente. (`app/docs/BACKLOG.md:833`)

   **Pergunta v12.5:** nomeia o BACKLOG com foco em “SUPRA-DELEGA-DIA-SEM-TRABALHO, CARTORIO-PROTESTA-1A-CLAUSULA” e correspondência com o código. **Resposta v12.5:** examina esses contratos, sem apontar o estado da linha EMISSORES. Classifico como **(e)** porque o encerramento/estado da obra não foi explicitamente perguntado; ler a pergunta como auditoria de todo o BACKLOG permitiria **(a)**, mas ampliaria seu recorte expresso. Além disso, código implementado não comprova obra encerrada. **[não verificado]**

10. **FILA — X1, concorrência na Pauta — (b) VIU E A TRIAGEM DESCARTOU.** A consulta de deduplicação antecede a chamada de escrita; as constraints do modelo não estabelecem unicidade por dia/motivo. (`app/chamados/services/fabrica_da_celula.py:180`; `:185`; `app/pautas/models.py:123`)

    **Pergunta v12:** “Aponte tambem qualquer outro defeito no delta”. **Resposta:** “a nova deduplicação de Pauta permite duplicatas sob concorrência”. **Triagem X1:** “DESCARTADO p/ esta volta — desenho da volta 3, sem unicidade no modelo; fila; vai ao opus”. A v8.r também já identificava a possibilidade. Duplicação efetiva em produção permanece **[não verificado]**.

11. **FILA — X2, catraca AST não prova fluxo — (b) VIU E A TRIAGEM DESCARTOU COMO BLOQUEIO.** Para os commands, a catraca procura a chamada pelo nome no arquivo; o adaptador exige a chave por acesso direto. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:228`; `app/ponto/supra_juiz.py:70`)

    **Pergunta v12:** “Aponte tambem qualquer outro defeito no delta”. **Resposta:** “Manter essa chamada e trocar o argumento de `classificar_dia` por `{}` continua satisfazendo a verificação por nomes”. **Triagem X2:** “DESCARTADO como bloqueio — `{}` com vivos>0/prev 0/cob None levanta KeyError na porta […] fila; vai ao opus”. O motivo aceita a falha em runtime como contenção; não refuta a deficiência da catraca apontada pelo Codex. **[não verificado]**

## 31/10 18:05 · sessao 8b041bea5bba · - · - · origem=Ronald (tmux)

- **Rascunhos sem LEI-AKITA: (e) NÃO PERGUNTADO.** O postmortem anterior já registra essa classe para `c5c43360` e promete incluir mensagens dos commits na pergunta seguinte (`app/docs/RELATO.md:14`). O `v12.q` fornecido pede “Leia o codigo na copia” e correspondência documental; não pede mensagens/histórico. A ausência de LEI-AKITA em `fbf3882d` permanece **[não verificado]**.

- **RED dos quatro emissores sem log: (c), execução fora do alcance demonstrado.** Existem casos dos quatro emissores em `app/chamados/tests/test_f1c4_3_emissores.py:80`, `:100`, `:118`, `:140`. `RELATO.md:140` declara “RED fbf3882d evidenciado (Ran 12 FAILED failures=4...)”, mas isso é registro documental, não o log. No anexo `v12.r`, o limite é explícito: “Execução dos testes Django e dos mutantes completos: [não verificado]”. Log efetivo ausente e resultado histórico: **[não verificado]**.

- **Sombra em árvore anterior e segunda passada sem log: (c).** O requisito de sombra com chamados novos por emissor antes do pouso está em `app/docs/BACKLOG.md:833`. O `v12.r` fornecido diz: “As quantidades e resultados de sombra registrados no BACKLOG são declarações documentais; sua confirmação independente permanece [não verificado]”. Árvore `c5c43360` e segunda passada específica: **[não verificado]**.

- **BACKLOG “em construcao”: ocorrência confirmada; classe exige cuidado.** O texto existe em `app/docs/BACKLOG.md:833`. O `v12.q5` pergunta se BACKLOG e outros documentos “descrevem o que o codigo faz hoje”; o `v12.r5` não menciona esse estado **[não verificado: anexos fornecidos sem arquivo local]**. Pode ser **(a)** sob leitura ampla da pergunta documental; sob leitura estrita, o estado de encerramento não foi perguntado, portanto **(e)**. Código implementado não demonstra obra encerrada, e o opus fornecido ainda a reprova **[não verificado]**.

## 31/10 18:05 · sessao 47f011ccdfdd · - · - · origem=Ronald (tmux)

**BLOQUEIA 1: classificação recomendada (e) — NÃO PERGUNTADO**, limitada às perguntas fornecidas; a falha documental é verificável em princípio por leitura, portanto não cabe classificá-la automaticamente como (c). O aval literal manda “pergunta so em competencia aberta; exportada -> Pauta DP”, enquanto a implementação restringe a exceção a `cel.trabalha is None` e invoca O44 para continuar perguntando quando `True`. (`app/docs/PROMPTS.md:1719`; `app/chamados/services/fabrica_da_celula.py:149-157`)

A obrigação de publicar a escolha contestável com números e reversão consta em `CLAUDE.md:454-458`; a lei L-050 registra que a fronteira de exportado para chamado/pergunta deixou de existir por O44. (`app/docs/LEIS.md:91`)

Trechos dos anexos que sustentam **(e)**:

- **Pergunta v12:** “Avais do dono que regem a obra: ‘JUIZ dia_sem_trabalho’ [...] e F1C4-3-NONE-COBRA”; pergunta 5: “Os textos de CORTES.md, BACKLOG.md (SUPRA-DELEGA-DIA-SEM-TRABALHO, CARTORIO-PROTESTA-1A-CLAUSULA), JUIZES.md e o comentario de _C18 descrevem o que o codigo faz hoje?”. Não solicita conferir a interpretação do aval das 11:5x nem a publicação pela PARADA 3. `[não verificado: anexos colados, sem arquivo:linha na cópia]`
- **Resposta v12, item 5:** “Correspondência entre documentos e código” trata de preenchimento do tipo, ressalva de `None`, ata corrente e `_protesta`; não apresenta a falta de decisão formal sobre exportadas. `[não verificado: anexo colado, sem arquivo:linha]`
- **Resposta v8, outros defeitos:** “A exceção do limbo exportado pode ser contornada pelo lote.” Isso identifica outro problema — bypass pelo FIO MUDO —, não a ausência de publicação da interpretação contestável; logo não demonstra classe (b) para o BLOQUEIA 1. `[não verificado: anexo colado, sem arquivo:linha]`

A pergunta genérica “qualquer outro defeito” oferece oportunidade de descoberta, mas não torna explicitamente perguntada a exigência documental específica da PARADA 3. `[não verificado: inferência de classificação sobre os anexos]`

**Limite:** não localizei `ESTACIONADO.md` na cópia; a afirmação de que seu item 4 já dizia CONTESTÁVEL depende exclusivamente do opus fornecido. `[não verificado]`

## 31/10 17:58 · sessao b1f133212c08 · PADRAO-DE-CIMA · verificador · origem=Code

**Pré-veredito: REPROVA.** Há caminhos que liberam por falta de sinal, uma liberação anterior que pode valer na primeira observação e uma corrida entre estado e escopo. **Certeza alta sobre os fluxos; ocorrência em produção [não verificado].** (`bin/disjuntor.py:1423`, `bin/disjuntor.py:1477`, `bin/disjuntor.py:1374`, `bin/disjuntor.py:1401`, `bin/disjuntor.py:1421`)

Fiz leitura estática e sondas isoladas em memória, com I/O substituído. Resultado das suítes completas: **[não verificado]**; executá-las criaria e apagaria arquivos, contrariando sua restrição. (`bin/tests/test_padrao_de_cima.sh:29`, `bin/tests/test_padrao_de_cima.sh:36`, `bin/tests/test_lei_akita.sh:168`)

**1. Quem escreve e quem lê**

Certeza **alta** nesta tabela; autoria humana efetiva e execução em produção ficam **[não verificado]**.

| Campo/arquivo | Quem escreve | Quem lê |
|---|---|---|
| **RELATO — topo** | Escrita manual é reconhecida pelo código; o diff não escreve `DESENHO` nem `PADRAO-DE-CIMA`. A porta de alarmes preserva o topo. (`app/core/esteira_vigia.py:829`, `app/core/esteira_vigia.py:839`, `app/core/esteira_vigia.py:872`) | Recorte central em `_bloco_do_topo`; PAREI/ESPERANDO por `_linha_do_topo`; B3 por `_topo_consistente`; PDC por `_topo_de` sobre os bytes do corpo. (`bin/hook_stop_fila1.py:400`, `bin/hook_stop_fila1.py:415`, `bin/disjuntor.py:676`, `bin/disjuntor.py:1359`) |
| **RELATO — corpo e negativas** | `relato_negacao` chama `esteira_vigia.no_relato`, que toma flock do diretório e publica por `replace`. **Também existem escritores fora dessa porta:** `gerar_cortes` e `deploy_agendado`. (`bin/disjuntor.py:234`, `bin/disjuntor.py:236`, `app/core/esteira_vigia.py:848`, `app/core/esteira_vigia.py:884`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:44`) | PDC varre o arquivo inteiro; fora do diff, Stop verifica prova, handoff lê o bloco inicial e publicação lê o RELATO. (`bin/disjuntor.py:1351`, `bin/disjuntor.py:1365`, `bin/hook_stop_fila1.py:1176`, `bin/handoff_sessao.sh:92`, `bin/relato.sh:56`) |
| **BACKLOG — bloco OBRAS** | Bloco declarado manual; `gerar_backlog` preserva-o e regrava o documento; `backlog_estado` altera a célula de estado diretamente. (`bin/gerar_backlog.py:103`, `bin/gerar_backlog.py:119`, `bin/gerar_backlog.py:140`, `bin/backlog_estado.py:58`, `bin/backlog_estado.py:59`) | Selo PDC, Stop/fila, handoff e gerador do resumo. **O resolvedor PDC não consulta BACKLOG.** (`bin/tests/test_padrao_de_cima.sh:328`, `bin/hook_stop_fila1.py:173`, `bin/handoff_sessao.sh:54`, `bin/gerar_backlog_topo.py:12`, `bin/disjuntor.py:1282`) |
| **`PROGRAMAS`** | Cadastro literal no código, separado do BACKLOG. (`bin/disjuntor.py:1110`) | `_PdcResolvedor.entidades`, somente sem entidade; selo verifica as chaves contra OBRAS. (`bin/disjuntor.py:1282`, `bin/tests/test_padrao_de_cima.sh:333`) |
| **GLOSSARIO / PLANO-PADROES** | Estes leitores não os alteram; padrão novo exige corte. Autor efetivo de cada alteração: **[não verificado]**. (`bin/padrao_vocab.py:34`, `CLAUDE.md:70`) | `padrao_vocab.vocabulario`: duas primeiras colunas do GLOSSARIO, negritos do NORTE e itens dos ADRs. (`bin/padrao_vocab.py:41`, `bin/padrao_vocab.py:48`, `bin/padrao_vocab.py:53`) |
| **Vocabulário em memória** | Produzido por `padrao_vocab.vocabulario`, normalizado por `limpo`. (`bin/padrao_vocab.py:21`, `bin/padrao_vocab.py:57`) | LEI-AKITA e PDC. (`bin/tests/test_lei_akita.sh:89`, `bin/tests/test_lei_akita.sh:110`, `bin/disjuntor.py:1327`, `bin/disjuntor.py:1315`) |
| **Estado PDC:** `eventos`, `visto_em`, `entidades`, `nomeados`, `libera_vistas`, `disparadas` | `decide_pdc`, persistido por `_com_estado` sob flock. (`bin/disjuntor.py:1360`, `bin/disjuntor.py:1373`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1401`, `bin/disjuntor.py:1405`, `bin/disjuntor.py:260`, `bin/disjuntor.py:281`) | Janela/agrupamento, `estado_pdc` e painel. (`bin/disjuntor.py:1287`, `bin/disjuntor.py:1292`, `bin/disjuntor.py:1458`, `bin/disjuntor.py:1537`) |
| **Escopo:** `em`, `disparadas`, `obras_com_evento` | `_pdc_grava_escopo`, depois da transição do estado. (`bin/disjuntor.py:1421`, `bin/disjuntor.py:1432`) | Fallback de `estado_pdc`. A decisão consulta `disparadas`; não valida frescor de `em` nem usa `obras_com_evento` nesse ramo. (`bin/disjuntor.py:1446`, `bin/disjuntor.py:1463`) |
| **`disjuntor.log` / painel** | `trilha`; PDC anuncia disparo/liberação após tentativa de persistência. (`bin/disjuntor.py:135`, `bin/disjuntor.py:1413`) | `ler` conta eventos; `_pdc_painel` lê o estado; `linha` apresenta os contadores. (`bin/disjuntor.py:2055`, `bin/disjuntor.py:2077`, `bin/disjuntor.py:1537`, `bin/disjuntor.py:2189`) |
| **`DISJUNTOR_DESLIGADO`** | Escritor autorizado: terminal do Ronald; Code recebe guarda contra ferramentas que nomeiem a chave. Fixtures também a criam. (`bin/disjuntor_settings.json:3`, `bin/disjuntor.py:167`, `bin/disjuntor.py:171`, `bin/tests/test_padrao_de_cima.sh:313`) | `desligado`, por `lexists`, inclusive symlink; usado nas portas B3/PDC, PreToolUse e Stop. (`bin/disjuntor.py:153`, `bin/disjuntor.py:1070`, `bin/disjuntor.py:1504`, `bin/disjuntor.py:1979`, `bin/hook_stop_fila1.py:1039`) |
| **B3: estado e triagem** | `decide_b3`, `rearma_b3` e `triagem`, pela trava comum do estado. (`bin/disjuntor.py:985`, `bin/disjuntor.py:1058`, `bin/disjuntor.py:838`, `bin/disjuntor.py:844`) | `julga_b3` → `estado_b3` → `b3_parada`/`_nega_b3`; Stop chama `julga_b3`. (`bin/disjuntor.py:1006`, `bin/disjuntor.py:1015`, `bin/disjuntor.py:1072`, `bin/disjuntor.py:1889`, `bin/hook_stop_fila1.py:1048`) |

Os hooks configurados em `.claude/` apontam para caminhos absolutos da árvore viva. Que a sessão de produção executava exatamente este hash: **[não verificado]**. (`.claude/settings.json:9`, `.claude/settings.json:68`)

**2. O vocabulário tem uma fonte?**

**Uma implementação da regra, alimentada por dois documentos.** Os dois consumidores chamam `padrao_vocab.vocabulario`; a extração saiu do heredoc do LEI-AKITA. **Certeza alta.** (`bin/padrao_vocab.py:34`, `bin/tests/test_lei_akita.sh:89`, `bin/disjuntor.py:1327`)

**Não é garantida a mesma lista concreta:** LEI-AKITA lê os documentos de `ALVO` por `git show`; PDC lê o disco da raiz. A igualdade depende desses conteúdos coincidirem. **Certeza alta; coincidência em produção [não verificado].** (`bin/tests/test_lei_akita.sh:75`, `bin/tests/test_lei_akita.sh:77`, `bin/disjuntor.py:1325`)

Há regex próprias para **formatos diferentes**: campo LEI-AKITA e quatro campos da liberação PDC. Ambas delegam a validade do termo ao vocabulário compartilhado. Há ainda termos esperados no MORDE e o conjunto `B3_LEIS`, que julga o desenho B3, não o universo de `padrao=`. **Certeza alta.** (`bin/tests/test_lei_akita.sh:99`, `bin/disjuntor.py:1130`, `bin/tests/test_lei_akita.sh:110`, `bin/disjuntor.py:1315`, `bin/tests/test_lei_akita.sh:163`, `bin/disjuntor.py:87`)

Outra implementação equivalente escondida em toda a árvore: **[não verificado]**; não foi localizada na busca textual.

**3. Falhas e limites**

| Situação | Comportamento e certeza |
|---|---|
| **GLOSSARIO ausente** | **Alta:** LEI-AKITA recebe `VocabIlegivel` e falha pela ausência de conclusão do juiz. PDC não libera nem consome a candidata. Porém o vocabulário só é consultado quando há linha candidata no topo: ausência do documento não provoca recusa global. (`bin/padrao_vocab.py:46`, `bin/tests/test_lei_akita.sh:90`, `bin/tests/test_lei_akita.sh:298`, `bin/disjuntor.py:1384`, `bin/disjuntor.py:1388`, `bin/disjuntor.py:1390`) |
| **RELATO legível, sem topo útil** | **Alta:** o recorte pode retornar `[]`; isso não libera entidade disparada. Um arquivo vazio também não fornece novos eventos. Não existe validação separada de “topo obrigatório”. (`bin/hook_stop_fila1.py:427`, `bin/hook_stop_fila1.py:443`, `bin/disjuntor.py:1365`, `bin/disjuntor.py:1383`) |
| **RELATO ilegível com estado legível** | **Alta, confirmado em memória:** `decide_pdc` preenche `nao_sei`, mas `estado_pdc` ignora-o. Sem disparada conhecida para a obra, retorna **LIVRE**. Com estado vazio, o portão fica verde sem ter lido a fonte. (`bin/disjuntor.py:1423`, `bin/disjuntor.py:1474`, `bin/disjuntor.py:1477`) |
| **BACKLOG perde uma chave cadastrada** | **Alta:** runtime permanece igual, pois usa o dict literal; o selo reprova a divergência. Se a obra faltar no próprio dict e não resolver modelo, recebe entidade vazia e não agrupa. (`bin/disjuntor.py:1110`, `bin/disjuntor.py:1282`, `bin/disjuntor.py:1284`, `bin/tests/test_padrao_de_cima.sh:333`) |
| **Codex rc 75** | **Alta:** limite identificado registra `limite.log`, orienta seguir por sonnet e retorna 75, sem sidecar válido desta chamada. A continuação efetiva da obra é **[não verificado]**; o wrapper apenas emite a orientação. (`bin/codex_pergunta.sh:186`, `bin/codex_pergunta.sh:191`, `bin/codex_pergunta.sh:192`, `bin/codex_pergunta.sh:194`, `bin/codex_pergunta.sh:201`) |
| **Disjuntor desligado** | **Alta:** portas B3/PDC retornam zero; PreToolUse e Stop pulam os disjuntores. A guarda contra alterar a chave continua ativa. (`bin/disjuntor.py:1070`, `bin/disjuntor.py:1504`, `bin/disjuntor.py:1978`, `bin/disjuntor.py:1979`, `bin/hook_stop_fila1.py:1039`) |
| **Falha do censo/resolvedor** | **Alta, confirmado em memória:** exceção vira `entidades=[]`, persistida no evento. Restaurar o censo não reclassifica o mesmo SHA, porque só eventos novos são resolvidos. Pode permanecer verde por ausência de entidade. (`bin/disjuntor.py:1366`, `bin/disjuntor.py:1369`, `bin/disjuntor.py:1373`, `bin/disjuntor.py:1298`) |
| **Falha ao gravar liberação** | **Alta:** a liberação é recolocada no resultado como disparada; nesta chamada continua recusada. (`bin/disjuntor.py:1418`, `bin/disjuntor.py:1419`) |
| **Estado ilegível** | **Alta:** usa escopo legível; fora dele retorna LIVRE. Sem escopo, recusa obras com evento; sem RELATO legível, recusa todas. (`bin/disjuntor.py:1461`, `bin/disjuntor.py:1465`, `bin/disjuntor.py:1471`) |

Mais dois bloqueios:

- **Liberação anterior aceita na primeira observação — alta, confirmado em memória.** Encontrando pela primeira vez dois desenhos e uma liberação válida já no topo, o ciclo dispara e libera imediatamente. O mtime é descartado; não existe fotografia das linhas anteriores ao disparo como no B3. O selo cobre somente liberação consumida numa observação anterior com um desenho. (`bin/disjuntor.py:1351`, `bin/disjuntor.py:1374`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1401`, `bin/tests/test_padrao_de_cima.sh:195`, `bin/tests/test_padrao_de_cima.sh:197`)
- **Escopo pode regredir — alta, inferência de concorrência.** A gravação ocorre após liberar a trava do estado. A decide primeiro e pausa; B decide depois e publica escopo novo; A publica o antigo por cima. Na próxima falha do estado, uma obra omitida pelo escopo antigo recebe LIVRE. `os.replace` evita arquivo parcial, mas não essa inversão temporal. (`bin/disjuntor.py:295`, `bin/disjuntor.py:1416`, `bin/disjuntor.py:1421`, `bin/disjuntor.py:1437`, `bin/disjuntor.py:1465`)

**4. Os selos mordem?**

**Há mordida comportamental, com lacunas.** Execução integral e sobrevivência experimental dos mutantes abaixo: **[não verificado]**; os graus indicados são por inspeção.

| Selo | Mordida existente | Mutante simples que pode escapar |
|---|---|---|
| **`test_padrao_de_cima.sh`** | **Alta:** portas reais, liberações, redisparo, janela, entidades distintas, desligamento e controles sem mutação. Apagar o gate do wrapper tem mutante declarado. (`bin/tests/test_padrao_de_cima.sh:110`, `bin/tests/test_padrao_de_cima.sh:179`, `bin/tests/test_padrao_de_cima.sh:285`, `bin/tests/test_padrao_de_cima.sh:377`, `bin/tests/test_padrao_de_cima.sh:388`) | **Alta por inspeção:** remover somente `O261-E-CELULA-20-UM-DONO` do cadastro, ou trocar seu destino. O selo exige apenas chaves existentes e dict não vazio; os cenários de programa usam outras duas obras. (`bin/disjuntor.py:1113`, `bin/tests/test_padrao_de_cima.sh:157`, `bin/tests/test_padrao_de_cima.sh:333`) |
| **`test_lei_akita.sh`** | **Alta:** lista vazia, inversão da aceitação e retirada da validação são atingidas pelos termos obrigatórios e casos positivos/negativos. (`bin/tests/test_lei_akita.sh:163`, `bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:203`) | **Alta por inspeção:** substituir o `raise VocabIlegivel` do GLOSSARIO por `pass`. Os cenários fornecem as fontes; não exercitam sua ausência. O mutante permitiria vocabulário parcial quando o GLOSSARIO faltasse. (`bin/padrao_vocab.py:45`, `bin/padrao_vocab.py:46`, `bin/tests/test_lei_akita.sh:172`, `bin/tests/test_padrao_de_cima.sh:74`) |
| **`test_disjuntor.sh`** | **Alta:** verifica leituras dentro das transições e agora exige duas tomadas da trava no verificador final, B3 e PDC. (`bin/tests/test_disjuntor.sh:845`, `bin/tests/test_disjuntor.sh:879`, `bin/tests/test_disjuntor.sh:882`) | **Alta por inspeção, considerando este selo isoladamente:** apagar a consulta ao vocabulário em `_pdc_libera`. Seus acréscimos não exercitam liberação PDC; o selo específico é quem possui esse mutante. (`bin/disjuntor.py:1315`, `bin/tests/test_disjuntor.sh:432`, `bin/tests/test_disjuntor.sh:882`, `bin/tests/test_padrao_de_cima.sh:396`) |

Há duas mordidas contaminadas:

- **Alta:** `B3-foto-parcial-engolida` injeta `H._bloco_do_topo()` onde `H` não está definido; os imports são locais a outras funções. A morte pode ser `NameError`, aceito pelo harness como qualquer falha. (`bin/tests/test_disjuntor.sh:1663`, `bin/tests/test_disjuntor.sh:1667`, `bin/disjuntor.py:676`, `bin/disjuntor.py:688`, `bin/disjuntor.py:702`, `bin/tests/test_disjuntor.sh:1491`)
- **Alta:** retirar a guarda “entidade já disparada” leva a `disp[ent]` inexistente no cenário antecipado; o harness também aceita esse erro como morte. Isso não prova a semântica temporal pretendida. (`bin/tests/test_padrao_de_cima.sh:195`, `bin/tests/test_padrao_de_cima.sh:397`, `bin/disjuntor.py:1402`, `bin/tests/test_padrao_de_cima.sh:375`)

**5. BUG 6**

**Curado na porta defeituosa do wrapper. Certeza alta.** `b3_parada` testa a chave antes de consultar estado; o teste específico remove exatamente essa guarda no mutante. (`bin/disjuntor.py:1070`, `bin/tests/test_padrao_de_cima.sh:319`, `bin/tests/test_padrao_de_cima.sh:385`)

Os outros caminhos produtivos encontrados também têm guarda: PreToolUse antes de `pretool`, e Stop antes de `julga_b3`. **Não encontrei outro chamador produtivo desprotegido.** As funções internas `decide_b3`/`estado_b3` continuam sem guarda própria; uma chamada direta não herda o desligamento. **Certeza alta nos caminhos citados; ausência absoluta de outro chamador [não verificado].** (`bin/disjuntor.py:1979`, `bin/disjuntor.py:1930`, `bin/disjuntor.py:1944`, `bin/hook_stop_fila1.py:1039`, `bin/hook_stop_fila1.py:1048`, `bin/disjuntor.py:903`, `bin/disjuntor.py:1015`)

**6. Classes históricas procuradas**

| Classe | Resultado e grau de certeza |
|---|---|
| **Segundo juiz duplicando registrado** | Reuso do censo e do juiz do vocabulário, confirmado. `_so_modelos` acrescenta filtro de herança; `censo.modelos` coleta classes mais amplamente. Duplicação de juiz de negócio não demonstrada; ausência absoluta **[não verificado]**. (`bin/disjuntor.py:1197`, `bin/disjuntor.py:1216`, `bin/disjuntor.py:1242`, `app/core/censo_escritas.py:90`, `app/core/censo_escritas.py:109`) |
| **Segundo escritor fora da porta** | **Encontrado, alta:** `gerar_cortes` e `deploy_agendado` regravam RELATO fora do flock comum; risco de sobrescrever atualização concorrente. Resíduo já declarado no código. (`bin/gerar_cortes.py:117`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:37`, `bin/deploy_agendado.sh:44`, `app/core/esteira_vigia.py:839`) |
| **Relógio fora da borda** | **Encontrado, alta:** hooks injetam `agora`, mas CLIs deixam `decide_pdc` e `decide_b3` chamar `time.time()` internamente. Trilha também lê relógio próprio. (`bin/disjuntor.py:1977`, `bin/disjuntor.py:1506`, `bin/disjuntor.py:1344`, `bin/disjuntor.py:903`, `bin/disjuntor.py:140`) |
| **TOCTOU / fotos diferentes** | **RELATO corpo/topo curado, alta:** recorte dos mesmos bytes validados. **Modelos ainda expostos, alta condicional:** censo lê `models`, filtro relê sem fotografia comum. O painel também abre a trilha quatro vezes, podendo misturar instantes. (`bin/disjuntor.py:709`, `bin/disjuntor.py:713`, `bin/disjuntor.py:1359`, `app/core/censo_escritas.py:105`, `bin/disjuntor.py:1232`, `bin/disjuntor.py:2055`, `bin/disjuntor.py:2058`) |
| **Selo sem mordida** | **Encontradas lacunas e mortes por exceção, alta**, detalhadas no item 4. Os selos estão ligados à régua por glob; LEI-AKITA também ao pre-push. Execução efetiva **[não verificado]**. (`bin/tests/test_disjuntor.sh:1667`, `bin/tests/test_padrao_de_cima.sh:375`, `bin/regua.sh:138`, `bin/pre-push.sh:38`) |
| **Ordem de trava** | **Alta:** wrapper toma slot → resposta → estado; triagem toma resposta compartilhada → estado. Não identifiquei inversão nesses percursos. O defeito concreto é publicar escopo fora da trava. (`bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:165`, `bin/disjuntor.py:812`, `bin/disjuntor.py:844`, `bin/disjuntor.py:1421`) |
| **Fallback** | **Encontrado, alta:** escopo antigo pode liberar; RELATO ilegível perde `nao_sei`; falha de entidade vira vazio persistente. O fallback por programa, por sua vez, é explicitamente cadastrado. (`bin/disjuntor.py:1465`, `bin/disjuntor.py:1423`, `bin/disjuntor.py:1477`, `bin/disjuntor.py:1370`, `bin/disjuntor.py:1282`) |
| **argv com dado sensível** | **Alta quanto à forma:** novos gates passam somente `$OBRA`; pergunta e anexos chegam ao Codex pelo stdin. Porém `CODEX_OBRA` só é validado como não vazio, sem raspagem. Dado sensível efetivamente presente nele: **[não verificado]**. (`bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:45`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/codex_pergunta.sh:178`) |

_1191 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
