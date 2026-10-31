# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 16:28** · publicado em **31/10/2026 16:28** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 31/10 16:19 · sessao 3f534d69f56a · - · - · origem=Ronald (tmux)

**Achado 1: (c), defeito documental criado na v11, após a última chamada guardada, v8.** O antecedente “predicado próprio no supra” já era conhecido, mas isso não equivale ao defeito novo “migrou e deixou corte/backlog divergentes”. (`opus_v11.txt`, item 1; `v8.q`, abertura; `v4.r`, item 2.)

- Pergunta pertinente: `v4.q`, item 2: “Ha sitio de producao que ainda responde [...] por conta propria [...] sem perguntar a chamados/juizes.py::dia_sem_trabalho?”
- Resposta: `v4.r`, item 2: “a migração está declarada no corte, mas não realizada nesse sítio.”
- Evidência local do antecedente: `app/ponto/supra_juiz.py:443` mantém predicado próprio; `app/docs/BACKLOG.md:806` mantém `SUPRA-DELEGA-DIA-SEM-TRABALHO` aberta.
- Entrada: tema supra já aparece na **v1**; registro/corte aparece na **v4**; contradição documental causada pela migração pertence à **v11** conforme `opus_v11.txt`, item 1.

**Achado 3: origem exata da catraca e primeira volta são [nao verificado].** A cópia não contém `com_insumo_dia_sem_trabalho`; o sítio final não pode ser confrontado diretamente. O tema genérico “testes exigem delegação?” aparece na **v1**, e Codex apontou explicitamente esse tipo de mutante sobrevivente. (`v1.q`, itens 4–5; `v1.r`, item 4.) [nao verificado]

- Pergunta: `v1.q`, item 4: “Os testes novos mordem a reversao de cada um dos 4 sitios individualmente?” [nao verificado]
- Resposta: `v1.r`, item 4: “também não exigem estruturalmente delegação ao juiz — um clone equivalente passa nos asserts de retorno.”
- Isso **não prova que Codex viu o defeito específico** “nome da função aceito com `ata=cel.ata`”. Sua primeira exposição e classificação definitiva dependem de saber quando nasceu a catraca, informação ausente. [nao verificado]
- Não usar **(b)**: nenhuma linha fornecida de triagem vincula esse mutante ao descarte. [nao verificado]

**Achado 4: o predicado do cartório foi visto; a consequência final de cláusula inatingível não foi apontada nos registros fornecidos.** A classificação única exige separar antecedente de alteração posterior. (`v1.r`, item 3; `v2.r`, item 6; `v3.r`, item 6; `opus_v11.txt`, item 4.)

- Pergunta explícita: `v3.q`, item 6: “QUEM MAIS LE celula com trabalha=None em chamados/ e ponto/ (cartorio, vigias, protesto FURO_SEM_COBRANCA, contadores) [...]?”
- Resposta explícita: `v3.r`, item 6: “Cartório | `None` passa em `_protesta`”.
- Pergunta ainda mais direta: `v4.q`, item 2: “Ha sitio de producao que ainda responde [...] por conta propria [...]?”
- A resposta `v4.r`, item 2, enumera predicados próprios, mas **omite `_protesta`**; no item 1 descreve `_C18` e não registra esse sítio. (`v4.r`, itens 1–2.)
- O predicado existe na cópia em `app/ponto/services/cartorio.py:1204–1205`: `return _cel.trabalha is not False and _acesos(x, _ata) is not True`.
- O juiz antigo lê `cel.ata` e começa pela cláusula `cel.trabalha is False`. (`app/chamados/juizes.py:407–408`.)
- Entrada verificável: sítio/predicado já na **v1**; `_C18` na **v4**. Primeira volta em que a delegação nova tornou a cláusula inatingível: **[nao verificado]**.
- **(a)** para o predicado inteiro seria incorreto, pois ele foi visto; **(b)** exige descarte documentado que não temos. Para “inatingível após delegação” pode caber **(c)** se a delegação nasceu após v8, mas essa cronologia específica precisa de evidência adicional. [nao verificado]

**Cuidado com triagem local:** `app/docs/RELATO.md:75` diz “2o predicado do supra descartado para fila”, mas refere-se à obra anterior **F1C4-3-NONE-COBRA**, não ao defeito documental da v11 EMISSORES. `app/docs/RELATO.md:29` contém triagem v1 EMISSORES, mas seus rótulos “P2a/P2b” não são identificáveis univocamente na resposta v1 fornecida. Não sustenta atribuição automática a **(b)**. (`app/docs/RELATO.md:29`; `app/docs/RELATO.md:75`.)

## 31/10 16:18 · sessao 14252be083ec · - · - · origem=Ronald (tmux)

**B3 presente × B3 prometido**

- O código desta cópia ainda usa limiar 3, acumula reprovações por agente/obra e libera por `--rearma-b3`, transferindo agentes para `rearmado`; não há consumo de linha nesta implementação (`bin/disjuntor.py:68`, `bin/disjuntor.py:545`, `bin/disjuntor.py:585`, `bin/disjuntor.py:1170`).
- O topo com `DESENHO` apenas cala o lembrete do Stop; a negativa do PreToolUse continua enquanto a obra estiver disparada (`bin/hook_stop_fila1.py:411`, `bin/hook_stop_fila1.py:1049`, `bin/disjuntor.py:947`).
- O contrato B3 **documentado** já exige reconhecedor único `linha_libera_b3`, duas linhas úteis do topo, linha posterior ao disparo (`mtime`), hash consumido uma vez e reprovações transferidas para `liberado`; isso está no texto, ainda não neste código (`app/docs/RELATO.md:11`, `app/docs/RELATO.md:12`). A versão triestado v4 mencionada pelo usuário não foi fornecida como código: [nao verificado].
- O contrato B3 documentado ainda diz “falha aberto”; o envelope real do PreToolUse captura qualquer exceção e libera. Assim, escrever simplesmente “NAO SEI = nega (molde B3 v4)” não descreve a implementação disponível (`app/docs/RELATO.md:13`, `bin/disjuntor.py:989`). No Stop, exceção também retorna `None`, mantendo o caminho anterior (`bin/hook_stop_fila1.py:1035`, `bin/hook_stop_fila1.py:1061`).

**Lacunas/contradições no anexo**

O anexo foi recebido apenas no prompt; referências ao conteúdo dele abaixo são [nao verificado no repo].

1. Falta declarar como **ação normativa** o limiar “segunda DESENHO da mesma entidade/programa em 24 h”; ele aparece apenas em MORDE. Falta integralmente a linha liberadora do aval `PADRAO-DE-CIMA <entidade>: classe=... padrao=... armadilha=... obra=...`, seus campos válidos, quem a escreve e como verifica vínculo com a obra bloqueada. [nao verificado]
2. Incluir `PERGUNTA DE DESENHO` amplia a forma literal `DESENHO <obra>` do aval; precisa dizer se é alias aceito, ocorrência distinta ou repetição da mesma pergunta. Também faltam deduplicação topo/corpo/arquivo e identidade estável do evento. [nao verificado]
3. “Com hora nas últimas 24 h” não define data/ano/fuso, `12:4x`, herança do cabeçalho, falta de hora ou instante futuro. “Tabela/modelo (ou PROGRAMA)” não resolve múltiplos modelos, aliases, prioridade entidade versus programa ou ausência de classificação. [nao verificado]
4. “NAO SEI = nega” precisa especificar **quem** será negado quando a entidade da ocorrência ou da chamada é desconhecida; negar tudo muda o escopo do aval “obras daquela entidade”. O envelope disponível deixa exceções passarem (`bin/disjuntor.py:989`). [nao verificado para a política nova]
5. “Mesma mecânica do linha_libera_b3” depende de código futuro: o texto B3 disponível especifica frescor, hash e transferência para `liberado`, mas o código só implementa rearme explícito (`app/docs/RELATO.md:12`, `bin/disjuntor.py:585`).
6. Falta definir se o disparo expira com a janela, fica travado até liberação, se a linha libera uma chamada ou um episódio e como tratar terceira ocorrência. No B3 documentado, a reprovação seguinte dispara novamente (`app/docs/RELATO.md:12`). [nao verificado para PADRAO-DE-CIMA]
7. Faltam limites de tamanho/tempo de varredura, erro de leitura parcial, estado corrompido, lock ocupado, gravação falhada e concorrência de dois consumidores. O estado atual usa flock e rename, mas permite manter decisão mesmo se a persistência falhar: isso é crítico para “consumir uma vez” (`bin/disjuntor.py:219`, `bin/disjuntor.py:230`, `bin/disjuntor.py:250`, `bin/disjuntor.py:254`).
8. O painel atual conta disparos pelo primeiro token depois de `DISJUNTOR` — regra, não entidade — e negativas globais; não conta liberações. A lista exibida de regras é fixa. Portanto, a medida anunciada pelo anexo ainda exige contrato de eventos/identidade, esperado e erro de medida (`bin/disjuntor.py:1030`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1055`, `bin/disjuntor.py:1147`).
9. “Ronald ou `pausar.sh`” não identifica arquivo de desligamento, cobertura nem condição de religar. O mecanismo atual só lê existência de `DISJUNTOR_DESLIGADO`, reservado ao terminal do Ronald; o Code é impedido até de citar a chave em Bash. `pausar.sh` é escritor genérico: não há integração automática dele com um novo instrumento (`bin/disjuntor.py:128`, `bin/disjuntor.py:144`, `bin/disjuntor.py:979`, `bin/pausar.sh:31`). O vigia só examina os nomes `esteira.pausada` e `esteira.integrando` nessa rotina; não fiscaliza automaticamente qualquer arquivo novo de pausa (`app/core/esteira_vigia.py:472`).

**Escritores/leitores relevantes**

| Arquivo/estado | Quem escreve | Quem lê |
|---|---|---|
| `app/docs/RELATO.md` — topo humano | Escrita manual é reconhecida explicitamente; identidade do agente que escreveu cada linha: [nao verificado] (`app/core/esteira_vigia.py:829`, `app/core/esteira_vigia.py:839`). | Stop lê duas linhas úteis, ignorando título e brancos; B3 busca DESENHO e ID nessa janela (`bin/hook_stop_fila1.py:420`, `bin/hook_stop_fila1.py:411`). Leitor novo PADRAO-DE-CIMA: [nao verificado]. |
| `RELATO.md` — negativas de máquina | `main_pretool → relato_negacao → esteira_vigia.no_relato`; escreve seção ALARMES, preservando topo, com trava no diretório e temporário único (`bin/disjuntor.py:988`, `bin/disjuntor.py:209`, `app/core/esteira_vigia.py:834`, `app/core/esteira_vigia.py:859`). | Stop checa afirmações sem prova por subprocesso (`bin/hook_stop_fila1.py:1172`, `bin/hook_stop_fila1.py:1178`). |
| `logs/sessao/disjuntor_estado.json` + `.lock` | `_com_estado`; varredura B3 e rearme o modificam (`bin/disjuntor.py:219`, `bin/disjuntor.py:560`, `bin/disjuntor.py:585`). | `ler_estado`, `b3_disparados`; PreToolUse e Stop chamam varredura que também escreve (`bin/disjuntor.py:265`, `bin/disjuntor.py:598`, `bin/disjuntor.py:950`, `bin/hook_stop_fila1.py:1050`). Estado novo de consumo por entidade: [nao verificado]. |
| `logs/sessao/disjuntor.log` | `trilha`, `dispara`, rearme, `main_pretool`; append, falha de trilha é silenciada (`bin/disjuntor.py:112`, `bin/disjuntor.py:123`, `bin/disjuntor.py:590`, `bin/disjuntor.py:985`). | `disjuntor.ler` calcula 24 h; `linha` formata; `gerar_estado.py` chama ambos (`bin/disjuntor.py:1030`, `bin/disjuntor.py:1147`, `bin/gerar_estado.py:277`). |
| `DISJUNTOR_DESLIGADO` | Ronald fora do Code, segundo contrato e guarda (`bin/disjuntor.py:28`, `bin/disjuntor.py:136`). | `desligado`, chamado antes de agir no PreToolUse e Stop (`bin/disjuntor.py:128`, `bin/disjuntor.py:980`, `bin/hook_stop_fila1.py:1040`). |
| Arquivo arbitrário de pausa + `logs/pausas.log` | `pausar.sh` grava QUEM/QUANDO/POR_QUE/SAIDA, rename e trilha; `--soltar` registra e remove (`bin/pausar.sh:24`, `bin/pausar.sh:31`, `bin/pausar.sh:39`). | `pausar.sh --ler`; vigia reconhece dono/saída para dois nomes específicos (`bin/pausar.sh:19`, `app/core/esteira_vigia.py:472`, `app/core/esteira_vigia.py:481`). |
| `.claude/settings.json` | O molde atribui aplicação à sessão no pouso; autoria efetiva desta cópia: [nao verificado] (`bin/disjuntor_settings.json:2`). | Configura Stop e PreToolUse com caminhos absolutos do host; ativação real em produção: [nao verificado] (`.claude/settings.json:9`, `.claude/settings.json:68`). |
| Pergunta/resposta do Codex | Escritor da pergunta: [nao verificado]; resposta é destino `-o` do `codex exec` (`bin/codex_pergunta.sh:40`, `bin/codex_pergunta.sh:157`). | Wrapper lê pergunta/anexos e monta prompt (`bin/codex_pergunta.sh:111`, `bin/codex_pergunta.sh:131`). PreToolUse atual não lê a pergunta para extrair obra (`bin/disjuntor.py:849`). |

Detalhe útil para pergunta 3: wrapper exige `CODEX_OBRA` e papel por ambiente, mas `_codex_cmds` atual só extrai papel/volta/anexos; não extrai `CODEX_OBRA`, pergunta nem entidade. Assim B3 atual não usa `_obra_de` em chamada Codex; ele só a usa no Agent verificador final (`bin/codex_pergunta.sh:43`, `bin/disjuntor.py:859`, `bin/disjuntor.py:947`).

## 31/10 16:18 · sessao 2237df423709 · - · - · origem=Ronald (tmux)

Há bases reaproveitáveis para **escritas**, mas elas não entregam uma função geral “arquivo:linha → entidade lida ou escrita”; `varrer_arvore` entrega sítios de escrita, `PORTAS` declara escritores permitidos e `JUIZES` declara autoridades por pergunta/família. (`app/core/censo_escritas.py:469`; `app/core/portas.py:9`; `app/core/juizes.py:3`)

**Fonte principal para arquivo:linha → entidade**

- `app/core/censo_escritas.py` é puro AST/os, sem Django; `varrer_arvore(app_root)` devolve registros `{arquivo, linha, entidade, op, expr}`, com caminhos relativos a `app/`. A operação da linha é identificada por `no.lineno`, portanto o resultado localiza o início da chamada, não todo seu intervalo. (`app/core/censo_escritas.py:1`; `app/core/censo_escritas.py:361`; `app/core/censo_escritas.py:469`; `app/core/censo_escritas.py:489`)
- Reconhece construtores `Modelo(...)`, operações ORM de criação/alteração/exclusão, `save/delete` de variável ligada ao modelo na função e `self` na classe do modelo. Preserva aliases de importação e amarrações de querysets/`get_object_or_404`. (`app/core/censo_escritas.py:6`; `app/core/censo_escritas.py:261`; `app/core/censo_escritas.py:285`; `app/core/censo_escritas.py:335`; `app/core/censo_escritas.py:364`; `app/core/censo_escritas.py:401`)
- Já fornece inferências explicitamente marcadas: `[campo]`, `[update_fields]`, `[relacionado]`, `[nome]`. A última é declarada fraca; nome mais longo vence, empate não atribui. Gerente reverso e campo só entram quando identificam um modelo único. (`app/core/censo_escritas.py:15`; `app/core/censo_escritas.py:20`; `app/core/censo_escritas.py:38`; `app/core/censo_escritas.py:151`; `app/core/censo_escritas.py:165`; `app/core/censo_escritas.py:182`)
- O comando `censo_escritas` já publica `arquivo:linha entidade expressão`, inclusive o balde não atribuível, e declara que só lê arquivo, sem banco. Para o instrumento no host, seria reaproveitável o módulo puro, sem executar `manage.py`. (`app/core/management/commands/censo_escritas.py:1`; `app/core/management/commands/censo_escritas.py:25`; `app/core/management/commands/censo_escritas.py:35`; `app/core/management/commands/censo_escritas.py:39`)

**Outros mapas úteis, com limites**

| Fonte | O que responde / limite |
|---|---|
| `core/portas.py::PORTAS` | Entidade → módulos que podem escrever; não prova que toda linha desses módulos pertence àquela entidade. (`app/core/portas.py:9`) |
| `core/portas.py::FAMILIA_DE` | Entidade → família. Família não pode substituir entidade sem mudar o aval. `CelulaDia` e `FuroDiario` têm a mesma família, por exemplo. (`app/core/portas.py:38`; `app/core/portas.py:44`) |
| `core/juizes.py::JUIZES` | Família/pergunta → `arquivo::função`/`Classe.método`; diz quem decide, não fornece entidade lida. (`app/core/juizes.py:3`; `app/core/juizes.py:7`; `app/core/juizes.py:156`) |
| `core/mapa/contrato.py` | Lê docstring de módulo com `@bolha`, `@entra`, `@sai`, `@chama` etc.; `@bolha` é família e não há campo estruturado de modelo lido/escrito. (`app/core/mapa/contrato.py:13`; `app/core/mapa/contrato.py:39`; `app/core/mapa/contrato.py:83`) |
| `bin/esmeril_chamado_censo.py::_onde` | Já mapeia números de linha para `(classe, função)` por intervalos AST; útil para associar uma citação a função, mas não resolve sua entidade. (`bin/esmeril_chamado_censo.py:76`) |
| `core/medida_leitor_do_dia.py` | Mede derivação de status/valor com vocabulários e lista de leitores; não é catálogo geral de modelos lidos. (`app/core/medida_leitor_do_dia.py:14`; `app/core/medida_leitor_do_dia.py:32`; `app/core/medida_leitor_do_dia.py:70`) |

**Ambiguidades reais**

- Inverter porta por arquivo pode devolver várias entidades: `ponto/portas/celula.py` escreve `CelulaDia` e `FuroDiario`; `ponto/services/fechamento.py` escreve `FechamentoMensal` e `PeriodoFechado`. Não se pode escolher a primeira. (`app/core/portas.py:111`; `app/core/portas.py:112`; `app/core/portas.py:495`; `app/core/portas.py:496`)
- `x.save()` com origem desconhecida, parâmetro, `form.save()` e `apps.get_model` podem ficar com `entidade=None`; o contrato do censo exige publicar esse balde. (`app/core/censo_escritas.py:43`; `app/core/censo_escritas.py:429`; `app/core/censo_escritas.py:507`)
- O AST de `modelos()` aceita qualquer classe de topo com base em `models.py`, não apenas modelo Django. O repo documenta o falso positivo `ResolvedoraCruzada(ValueError)` e o teste que passou a consultar o registro Django para validar `FAMILIA_DE`. Portanto a lista AST sozinha não comprova modelo. (`app/core/censo_escritas.py:108`; `app/core/portas.py:60`; `app/core/tests/test_censo_escritas.py:264`)
- Modelos são chaveados somente por nome da classe, com `setdefault`; duplicidade de nomes em apps distintas não se distingue pelo resultado desse cadastro. (`app/core/censo_escritas.py:89`; `app/core/censo_escritas.py:111`)
- Arquivo ilegível e fonte com erro sintático podem virar ausência no censo atual: `varrer_fonte` retorna `[]` em `SyntaxError`; `varrer_arvore` ignora erro de leitura. Para usar como juiz NAO_SEI, seria necessário distinguir erro de “nenhuma escrita encontrada”. (`app/core/censo_escritas.py:451`; `app/core/censo_escritas.py:485`)
- Inferir por campo ou padrão não prova o modelo. Outro censo do próprio repo declara explicitamente que datas de início/fim não distinguem vínculo de férias/período aquisitivo/ordem, e classifica nominalmente as exceções. (`app/core/censo_pergunta2.py:15`; `app/core/censo_pergunta2.py:42`)
- “Chamado” do aval não é o nome da classe principal existente: ela é `ChamadoColaborador`; normalização/alias “Chamado → ChamadoColaborador” não está estabelecida nos mapas examinados. (`app/chamados/models.py:21`; `app/core/portas.py:58`; alias: [nao verificado])

**Caminho determinístico proposto**

Usar a citação normalizada relativamente a `app/`, identificar o nó/função por AST, consultar os sítios de `censo_escritas` nessa chamada/função e aceitar somente entidade única sustentada pelo código; conservar marca de inferência. Quando a linha é só leitura, ou função toca vários modelos, ou a citação não bate com a árvore, devolver NAO_SEI em vez de expandir para a família. Esta é proposta, não comportamento implementado. (bases: `app/core/censo_escritas.py:469`; `bin/esmeril_chamado_censo.py:76`; proposta: [nao verificado])

**Ligação obra → PROGRAMA**

Há vínculos documentais concretos, porém textuais e com várias formas:

- `O-PROJECOES` tem `PROGRAMA: complemento STATUS-DO-DIA-NA-CELULA do PROGRAMA-CELULA-E-O-DIA`. (`app/docs/BACKLOG.md:730`)
- `O280` tem `PROGRAMA: E3 do PROGRAMA-CELULA-E-O-DIA`. (`app/docs/BACKLOG.md:1398`)
- `V3-BATERIA-DE-CASOS-PELA-REGRA` tem `PROGRAMA: E1 do PROGRAMA-CELULA-E-O-DIA`. (`app/docs/BACKLOG.md:1399`)
- `O261-E-CELULA-20-UM-DONO` declara em prosa “esta obra é a E2” desse programa. (`app/docs/BACKLOG.md:782`)
- `E4-CAL-RETIDO-FOLHA-NO-LEITOR` declara “linha da E4 do PROGRAMA...”, enquanto `PROGRAMA-CELULA-E-O-DIA-DUAS-PISTAS` é complemento do programa. (`app/docs/BACKLOG.md:774`; `app/docs/BACKLOG.md:773`)
- ADR 0002 identifica seu programa, mas também mostra que programa não equivale a uma só tabela: status em `CelulaDia`, valor em `DiaPago`. (`app/docs/ADR/0002-celula-e-o-dia.md:4`; `app/docs/ADR/0002-celula-e-o-dia.md:8`)

Assim, há material para declarar um vínculo explícito e auditável, mas interpretar “complemento”, etapas, programa pai e família como equivalentes seria nova regra. Parser único já implementado de obra→programa/entidade: [nao verificado].

## 31/10 16:18 · sessao 3fb9a544fd51 · - · - · origem=Ronald (tmux)

P1 — formas e limites observados:

| Forma | Evidência | Hora e entidade |
|---|---|---|
| Forma literal apenas como **exemplo de contrato**: ``DESENHO <obra>: lei=... pergunta=... <arquivo>:<linha>`` | `app/docs/RELATO.md:12` | Sem hora; placeholders não constituem evento. Excluir contratos/exemplos é necessário para não contar ocorrência fictícia. |
| Topo real: `REPROVA-E-DESENHO V3-RETOMA (12:4x, ...)` | `app/docs/RELATO.md:4` | Obra explícita, hora aproximada sem data; várias referências (motor, alimentação, regras, núcleo) e `CCT.feriados`, sem entidade única declarada. |
| `> **[RESPONDIDA 23:5x...] PERGUNTA DE DESENHO O261-E-F5 (...), 30/10 23:4x:**` | `app/docs/RELATO.md:141` | Data/hora aproximada do evento; distinguir hora da resposta `23:5x`, do disparo e do evento `23:4x`. “família chamado” aparece, mas também célula, folha, vigia, referências `fora_da_folha.py:51` e `raiox.py:151`; primeiro arquivo:linha não define entidade por si. |
| `PERGUNTA DE DESENHO (nao devolve turno) -- **GEO-SILENCIO-DE-PING: ...` | `app/docs/RELATO-ARQUIVO.md:983` | Obra após texto intermediário, sem hora própria nem arquivo:linha na linha; referências de código aparecem nos itens seguintes, por exemplo `ponto/presenca.py:12` em `:985`, `static/pwa.js:404` em `:987`. Timestamp exato [nao verificado]. |
| Prosa respondida/riscada: `~~(pergunta de DESENHO -- ...` | `app/docs/RELATO.md:947` | Hora disponível é da resposta `26/10 22:1x`, não da abertura; nomes de funções sem localização. Contagem como novo evento exige decisão de contrato [nao verificado]. |
| Prosa de custo/classificação, não marcador de evento: `(DESENHO)`, `= DESENHO`, `DESENHO, NAO DEFEITO` | `app/docs/RELATO.md:1302`, `:1333`, `:4058`; `app/docs/RELATO-ARQUIVO.md:6876`, `:7130` | Buscar apenas substring `DESENHO` conta classificações, títulos e menções históricas, além de perguntas. |
| Título de desenho: `### O DESENHO DA LINHA (ponto.OcorrenciaHE)` | `app/docs/RELATO-ARQUIVO.md:22802` | Modelo explícito, mas nenhuma obra/evento no formato avalizado; título pai contém `02/10 03:5x` em `:22783`. |

O selo de hora existente só reconhece `^## DD/MM HH:M[x\d]`, fixa TZ São Paulo, transforma dezena em `HH:M0` e infere ano de `git blame`; ele verifica título futuro, não entrega hora por evento (`bin/tests/test_hora_da_secao_nao_e_futura.sh:21`, `:25`, `:41-49`). Portanto `23:4x` oferece intervalo de dez minutos, sem instante exato para uma borda estrita de 24h (`app/docs/RELATO.md:141`).

Não é seguro herdar sempre o título anterior: a primeira seção do RELATO está em `:185`, datada 30/10, enquanto o preâmbulo contém entradas 31/10 (`app/docs/RELATO.md:41`, `:57`, `:63`). O arquivo histórico é anexado no fim para preservar citações, pode ter `###` que perderam seu `##` original e não é uma sequência temporal simples (`app/docs/RELATO-ARQUIVO.md:23-30`). A dieta inclusive manteve cópias de contratos no vivo para leitores de testes, enquanto a história ficou no arquivo; contar vivo+arquivo sem identidade/deduplicação pode contar representação repetida (`app/docs/RELATO-ARQUIVO.md:34-40`).

P5 — RELATO e arquivo:

- **Topo manual / perguntas DESENHO:** o código documenta “topo escrito a mão” e preserva seus bytes; autoria concreta das linhas observadas [nao verificado] (`app/core/esteira_vigia.py:829-840`). A conduta manda perguntas de desenho ao topo (`CLAUDE.md:458-459`).
- **RELATO escritor de máquina:** `app/core/esteira_vigia.py::no_relato`, seção `## ALARMES DA ESTEIRA`; flock do diretório, temporário único e `os.replace` (`app/core/esteira_vigia.py:815-840`, `:844-884`). Este escritor altera mtime do documento inteiro; mtime não prova hora de cada DESENHO (`:879-884`).
- **Outros escritores do RELATO:** gerador de SEUS CORTES lê e regrava inteiro (`bin/gerar_cortes.py:114-136`); deploy agendado insere uma linha e regrava (`bin/deploy_agendado.sh:31-44`). O próprio escritor único declara que estes e o Edit manual não tomam sua trava (`app/core/esteira_vigia.py:839-840`).
- **Leitor do topo atual:** hook lê só as duas primeiras linhas úteis, pula cabeçalho/brancas, tira crases iniciais; não lê todo corpo para B3 (`bin/hook_stop_fila1.py:420-444`). `_pergunta_b3_escrita` aceita substring `DESENHO` e identidade de obra por `DJ.obras_da_linha` (`:411-417`).
- **Leitores/prova/publicação:** checador lê RELATO e verifica afirmações fortes com PROVA em janela de seis linhas (`bin/relato_afirma_com_prova.py:33-50`, `:86-99`); wrapper de publicação o invoca e raspa/copia o RELATO para destino público (`bin/relato.sh:54-56`, `:98-104`). Publicador não é escritor do RELATO original nesses trechos.
- **RELATO-ARQUIVO:** regra exige mover material de mais de três dias no mesmo commit (`CLAUDE.md:610-613`); lei registra isso como conduta sem guarda mecânica (`app/docs/LEIS.md:135`). Cabeçalho documenta cortes manuais por conteúdo, movimento integral e append para preservar linhas (`app/docs/RELATO-ARQUIVO.md:3-14`, `:23-30`, `:42-50`). Escritor automático/estado de rotação [nao verificado].

## 31/10 16:16 · sessao fb824fd80c4f · ADR-0003 · censo · origem=Code

Leitura estática do código. **O total absoluto de leitores do repositório ficou `[nao verificado]`**: abaixo conto funções comprovadas, com até oito exemplos por campo. `≥8` significa oito leitores comprovados apresentados; `0 localizado` não prova inexistência. Execução em produção, conteúdo efetivamente armazenado no JSON e correspondência do snapshot com o HASH informado: `[nao verificado]`.

Os caminhos são relativos a `app/`. Para reduzir repetição nas tabelas, as referências abreviadas têm estas expansões literais:

| Referência | Arquivo |
|---|---|
| `M:linha` | `escala/models.py:linha` |
| `P:linha` | `ponto/portas/celula.py:linha` |
| `C:linha` | `ponto/services/cartorio.py:linha` |
| `G:linha` | `ponto/management/commands/gerar_celulas.py:linha` |
| `L:linha` | `escala/services/leitor_celula.py:linha` |
| `J:linha` | `chamados/juizes.py:linha` |
| `F:linha` | `folha/export.py:linha` |
| `S:linha` | `ponto/status_dia.py:linha` |
| `T:linha` | `ponto/turnos.py:linha` |
| `U:linha` | `escala/utils.py:linha` |

Os nomes abreviados de escritores usados abaixo são:

| Nome | Função e persistência |
|---|---|
| **GER** | `gerar_celulas_periodo`: cria e salva células — `P:593`, `P:605`, `P:615`. |
| **REG** | `regenerar_celulas_vinculo`: cria, limpa previsão fora da vigência ou regrava previsão — `P:231`, `P:247`, `P:259`. |
| **REV** | `reverter_regeneracao_dia`: restaura DNA anterior e salva — `P:527`, `P:538`. |
| **LAV** | `lavrar_veredito`: atribui julgamento/status e persiste por `salvar_e_emitir` — `P:709`, `P:743`, `P:760`, `P:775`. |
| **CAR** | `carimbar`: grava impressão, datas e insumos, incluindo campos preparados por LAV — `P:787`, `P:800`, `P:807`. |
| **PREC** | `carimbar_precedencia`: grava `ata.precedencia` — `P:811`, `P:819`, `P:822`. |
| **LOTE** | `carimbar_insumos_em_lote`: atribui insumos e executa `bulk_update` — `P:826`, `P:838`, `P:843`. |
| **SAVE** | `salvar_e_emitir`: `cel.save(update_fields=campos)`, compartilhado por LAV/CAR/PREC — `P:634`, `P:640`. |

`regenerar_celulas_dia` delega a REG com janela de um dia; não contém outra gravação (`P:474`). A fachada `CelulaDia.lavrar_veredito` delega a LAV (`M:1794`).

Os portões e eventos referenciados nas tabelas significam:

| Código | Fato comprovado |
|---|---|
| **PG** | GER pula `origem='editada'`; no passado só preenche DNA nulo; hoje/futuro podem ser regravados. Nenhuma consulta de exportação, tranca, `FechamentoMensal` ou versão nesse corpo — `P:555`, `P:598`, `P:600`, `P:609`. |
| **PR** | REG e REV consultam `_competencias_exportadas`; `apesar_da_lavra` dispensa essa guarda — `P:196`, `P:199`, `P:238`, `P:520`. A consulta usa `competencia_fechada_para_celula`, que combina TXT vigente com holerite do colaborador em lote publicado — `G:173`, `F:599`, `F:603`. `FechamentoMensal.status` não é esse portão — `G:132`. REG também protege origem humana — `P:226`, `P:251`; REV exige origem gerada e DNA anterior — `P:518`. | [nao verificado]
| **PJ** | Lavra/carimbo normais: **nenhum portão de competência exportada/trancada ou `FechamentoMensal`** nos escritores — `P:709`, `P:787`, `P:811`. Há idempotência por conteúdo — `P:753`, `P:795`, `P:816`. O cartório compara impressão; `forcar` permite rejulgamento — `C:1157`, `C:1161`. |
| **PS** | Exclusivamente no command `lavrar_status_competencia`: barra `competencia_entregue` — `ponto/management/commands/lavrar_status_competencia.py:40`; completa somente status nulo com carimbo atual — `C:726`, `C:729`, `C:733`. |
| **PB** | Backfill de insumos: nenhum portão de competência; seleciona células julgadas e chama LOTE — `ponto/management/commands/backfill_b5_insumos.py:60`, `:72`, `:81`. |
| **EG** | GER pelo cron 05:50 — `config/crons.py:407`; alterações de vigência/âncora chamam REG — `escala/signals.py:71`, `:117`; alteração de template — `escala/services/cadastro_tipo.py:348`; atualização de folgas — `escala/folgas.py:220`; edição do vínculo — `colaboradores/services/vinculo.py:320`. |
| **EJ** | Cartório pelo cron 06:27 — `config/crons.py:597`; vigia de hora a cada dez minutos — `config/crons.py:348`; signals de batida save/delete, ausência save/delete e justificativa save — `ponto/signals.py:132`, `:139`, `:144`, `:151`, `:162`; chamado criado/mudança de status — `chamados/signals.py:165`, `:180`. |
| **EA** | Decisão humana chama `cel.lavrar_veredito(via='admin', …)` — `ponto/services/veredito_celula.py:330`. |
| **EB** | Backfill de insumos — `ponto/management/commands/backfill_b5_insumos.py:72`, `:81`; lote de status — `ponto/management/commands/lavrar_status_competencia.py:55`. |

Na coluna **FP**, “não” significa que **o valor da coluna não é serializado diretamente no fingerprint**. A definição integral das partes está em `C:333`–`C:390`; influências indiretas por seleção de insumos aparecem depois no grafo.

| Coluna | Escritores de produção | Fonte do valor gravado | Regravação/evento | Portão | Leitores comprovados; contagem | FP |
|---|---|---|---|---|---|---|
| `colaborador` | GER `P:593`; REG `P:247`. | `ec.colaborador_id` — `P:247`, `P:594`. | Só criação nos escritores encontrados; alteração posterior `[nao verificado]`. | GER: PG; REG: PR. | **≥8**: `celulas_do_periodo` (`escala/alimentacao.py:42`); `escala_vigente` (`escala/servico_jornada.py:46`); `propostas` (`escala/services/propositor.py:488`); `montar_previa` (`folha/previa.py:51`); `dia_da_frota` (`relatorios/dia_da_frota.py:42`); `lastro.julgar` (`ponto/services/lastro.py:233`); `aplicar_precedencia_na_celula` (`C:1674`); `emitir_evento_celula` (`P:631`). | Não — `C:333`. |
| `data` | GER `P:594`; REG `P:247`. | Dia iterado na janela solicitada — `P:208`, `P:571`. | Só criação nos escritores encontrados; alteração posterior `[nao verificado]`. | GER: PG; REG: PR. | **≥8**: `celulas_do_periodo` (`escala/alimentacao.py:42`); `escala_vigente` (`escala/servico_jornada.py:46`); `propostas` (`escala/services/propositor.py:488`); `grade_da_celula` (`L:325`); `motivos_retencao_celula` (`F:1188`); `montar_previa` (`folha/previa.py:77`); `orfas_e_isencao_da_ata` (`relatorios/cartao_pela_celula.py:288`); `emitir_evento_celula` (`P:631`). | Não diretamente; seleciona janela/versão — `C:1104`, `C:1148`, `C:947`. |
| `trabalha` | GER `P:595`, `P:612`; REG `P:231`, `P:247`, `P:260`; REV `P:532`. | Cálculo EC/template — `P:243`, `P:590`; FolgaDia — `M:1257`, `M:1327`; âncora/ciclo — `M:1260`; feriado/praça — `M:1340`, `M:1362`; REV usa `dna_anterior.marcos` — `P:527`, `P:532`. | Sim: EG, regeneração explícita e reversão — `P:260`, `P:532`. | PG/PR conforme escritor. | **≥8**: `dia_sem_trabalho` (`J:408`); `eh_dia_trabalho` (`M:1072`); `grade_da_celula` (`L:345`); `classificar_falta` (`escala/servico_jornada.py:303`); `contexto_do_dia_texto` (`pautas/services.py:410`); `etiquetas_pela_celula` (`ponto/services/flip_auto.py:208`); `_veredito_do_dia` (`ponto/services/dia_do_colab.py:121`); `dia_da_frota` (`relatorios/dia_da_frota.py:47`). | Não — `C:333`. |
| `origem` | GER/REG gravam `'gerada'` — `P:248`, `P:595`; default — `M:1659`. | Literal/default, não batidas — mesmas linhas. | Escritor posterior de `'editada'` não localizado `[nao verificado]`. | Criação GER: nenhum de competência; REG: PR. | **6**: `CelulaDia.__str__` (`M:1810`); `celulas_contra_o_template.Command.handle` (`escala/management/commands/celulas_contra_o_template.py:74`); GER (`P:598`); REG (`P:226`); REV (`P:518`); `dia_do_colab` (`ponto/services/dia_do_colab.py:90`). | Não — `C:333`. |
| `escala_geradora` | GER `P:595`, `P:613`; REG `P:248`, `P:260`; REV `P:535`; declaração `SET_NULL` — `M:1661`. | EC fornecida; na reversão, `dna_anterior.vinculo_id` — `P:533`. | Sim: EG/REG/REV; deleção da EC pode nulificar pela relação — `M:1661`. | PG/PR; `SET_NULL`: nenhum portão de competência declarado no campo — `M:1661`. | **7**: `vinculo_do_dia` (`escala/alimentacao.py:238`); `contraprova_piso` (`chamados/management/commands/auditar_invariantes_chamados.py:389`); GER (`P:611`); REG (`P:228`); `regenerar_em_exportada` (`ponto/portas/regen_exportada.py:141`); `turnos_abertos_de` (`T:2316`); `dia_da_frota`, pré-carga utilizada na consulta do vínculo (`relatorios/dia_da_frota.py:35`, `:42`). | Não — `C:333`. |
| `dna` | GER `P:596`, `P:604`, `P:614`; REG `P:232`, `P:248`, `P:260`; REV `P:530`. | `montar_dna`: template/marcos/pausas/ciclo/jornada/intervalo/extra declarada — `G:76`, `G:81`, `G:95`, `G:119`; condicionado a `trabalha` — `G:79`; REV copia `dna_anterior` — `P:527`. | Sim: EG/REG/REV; GER também enriquece DNA nulo passado — `P:603`. | PG/PR. | **≥8**: `marcos_do_dia` (`M:1099`); `pausas_do_dia` (`M:1162`); `extra_declarada_do_dia` (`M:1206`); `marcos_dna_periodo` (`U:97`); `pausas_de_celulas` (`U:130`); `contexto_do_dia_texto` (`pautas/services.py:406`); `periodos_do_dia` (`ponto/juiz_batida.py:158`); `_julgar_colab_corpo` (`C:998`). | **Sim: somente `dna_versao` e `marcos`** — `C:337`. |
| `gerada_em` | `auto_now_add`, nos creates GER/REG — `M:1670`, `P:247`, `P:593`. | Relógio na criação — `M:1670`. | Não regravado pelos caminhos encontrados; comportamento declarado — `M:1675`. | Criação GER/REG conforme PG/PR. | **2**: `marcos_do_lote` (`chamados/veredito_lavrado.py:153`); `janelas_reabertas` (`escala/services/fase_12x36.py:190`). | Não — `C:333`. |
| `regenerada_em` | REG `P:262`; REV `P:536`. | `timezone.now()` — mesmas linhas. | Sim em regeneração alterada/reversão. O ramo de limpeza fora da vigência salva só `trabalha/dna` — `P:231`, `P:233`. | PR. | **7**: `marcos_do_lote` (`chamados/veredito_lavrado.py:153`); `janelas_reabertas` (`escala/services/fase_12x36.py:189`); `houve_fato_novo` (`C:295`); `dia_do_colab` (`ponto/services/dia_do_colab.py:91`); `fila_de_orfas` (`escala/services/regeneracao.py:145`); `_marcar_cobrancas_orfas` (`G:242`); `supra_juiz.Command.handle` (`ponto/management/commands/supra_juiz.py:153`). | Não — `C:333`. |
| `regeneracoes` | Default `0` — `M:1691`; REG `P:263`; REV `P:537`. | Valor anterior, ou zero, mais um — `P:263`, `P:537`. | Sim em REG/REV; GER não grava contador — `P:615`; limpeza fora da vigência também não — `P:233`. | PR. | **2**: REG (`P:263`); REV (`P:537`). | Não — `C:333`. |
| `dna_anterior` | REG `P:261`; REV limpa — `P:531`. | DNA imediatamente anterior — `P:259`; na reversão, literal `None` — `P:531`. | Sim: sobrescrito por nova regeneração; apagado na reversão — mesmas linhas. | PR. | **2**: REV (`P:518`, `P:527`); `aplicar_col900.Command.handle` (`ponto/management/commands/aplicar_col900.py:68`). | Não — `C:333`. |
| `editada_em` | Nenhum escritor de produção localizado `[nao verificado]`; o serviço registra “nenhum declarado hoje” — `escala/services/fase_12x36.py:161`. | `[nao verificado]`. | `[nao verificado]`. | `[nao verificado]`. | **3**: `marcos_do_lote` (`chamados/veredito_lavrado.py:154`); `janelas_reabertas` (`escala/services/fase_12x36.py:190`); `houve_fato_novo` (`C:295`). | Não — `C:333`. |
| `editada_por` | Nenhum escritor explícito localizado `[nao verificado]`; relação declara `SET_NULL` — `M:1701`. | Escrita humana `[nao verificado]`; nulificação por deleção declarada — `M:1702`. | Deleção do usuário: `SET_NULL` — `M:1702`; demais eventos `[nao verificado]`. | Nenhum no campo — `M:1702`; demais caminhos `[nao verificado]`. | **0 localizado** `[nao verificado]`. | Não — `C:333`. |
| `julgada_em` | CAR/SAVE — `P:800`, `P:807`, `P:640`. | Argumento `agora` do julgamento — `P:800`. | Sim por EJ quando impressão/insumos/lavra diferem; repetição idêntica retorna sem salvar — `P:795`. | PJ. | **7**: `marcos_do_lote` (`chamados/veredito_lavrado.py:154`); `janelas_reabertas` (`escala/services/fase_12x36.py:189`); CAR (`P:796`); `_julgar_colab_corpo` (`C:1000`); `celula_veredito_velho.Command.handle` (`ponto/management/commands/celula_veredito_velho.py:109`); `backfill_b5_insumos.Command.handle` (`ponto/management/commands/backfill_b5_insumos.py:61`); `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:55`). | Não — `C:333`. |
| `impressao` | CAR/SAVE — `P:801`, `P:807`. | `impressao_insumos` — `C:998`, `C:1148`; composição detalhada abaixo — `C:333`. | Sim por EJ; comparação controla pulo — `C:1157`; `forcar` ignora pulo — `C:1161`. | PJ; versão participa do hash, não é bloqueio — `C:352`. | **3**: CAR (`P:797`); `_julgar_colab_corpo` (`C:1000`, `C:1157`); `celula_veredito_velho.Command.handle` (`ponto/management/commands/celula_veredito_velho.py:110`). | Não é insumo do próprio hash — `C:333`. |
| `veredito` | LAV, persistido por SAVE/CAR — `P:770`, `P:775`, `P:807`. | Cartório: literal ou `derivar_veredito(codigos)` — `P:658`; códigos vêm da grade, batidas, cobertura, chamados/fio e cadastro — `C:1207`; humano fornece decisão — `P:750`, `ponto/services/veredito_celula.py:330`. | Sim por EJ/EA; cartório preserva decisão admin — `P:738`; idempotência — `P:753`. | PJ; precedência do admin é guarda de decisão, não de competência — `P:738`. | **≥8**: `_trilha_da_recusa` (`chamados/models.py:1148`); `retratar_chamados_absolvidos_por_celula` (`chamados/reconciliador.py:963`); `_absolvido_pela_celula` (`chamados/reconciliador.py:982`); `motivos_a_perguntar` (`chamados/services/fabrica_da_celula.py:86`); `grade_da_celula` (`L:365`); `motivos_retencao_celula` (`F:1215`); `montar_previa` (`folha/previa.py:94`); `dia_da_frota` (`relatorios/dia_da_frota.py:51`). | Não — `C:333`. |
| `veredito_via` | LAV — `P:771`, `P:775`. | Argumento validado: `cartorio/admin` — `P:720`, `P:771`. | Sim por EJ/EA, respeitando decisão admin — `P:738`. | PJ. | **≥8**: `veredito_que_a_lavra_escreve` (`P:656`); `_status_alvo` (`P:694`); LAV (`P:722`); `_trilha_da_recusa` (`chamados/models.py:1154`); `retratar_chamados_absolvidos_por_celula` (`chamados/reconciliador.py:964`); `_chave_memo_pv` (`J:793`); `_pergunta_viva_calc` (`J:1046`); `contexto_calendario` (`colaboradores/services/calendario.py:513`). | Não — `C:333`. |
| `veredito_em` | LAV — `P:772`, `P:775`. | `agora or timezone.now()` — `P:772`. | Sim quando muda julgamento/ata; completar só status não move essa data — `P:753`, `P:757`. | PJ; lote de status PS não move o campo — `P:757`. | **4**: `marcos_do_lote` (`chamados/veredito_lavrado.py:153`); `contexto_calendario` (`colaboradores/services/calendario.py:513`); `decisao_do_dia` (`ponto/services/veredito_celula.py:109`); `celula_veredito_velho.Command.handle` (`ponto/management/commands/celula_veredito_velho.py:107`). | Não — `C:333`. |
| `protestos` | LAV — `P:769`, `P:775`. | Cartório copia códigos atuais — `P:746`, `C:1207`; admin preserva a própria coluna — `P:751`. | Sim por EJ; EA pode salvar a lista preservada — `P:751`, `P:769`. | PJ. | **6**: `motivos_retencao_celula` (`F:1228`); `nao_retem_pela_pausa` (`F:1147`); LAV (`P:722`); `_completar_status` (`C:733`); `julgar_colab` (`C:864`); `celula_veredito_velho.Command.handle` (`ponto/management/commands/celula_veredito_velho.py:108`). | Não — `C:333`. |
| `ata` | LAV `P:741`, `P:768`; PREC `P:819`; persistência SAVE/CAR — `P:640`, `P:807`. | Grade/DNA/batidas/ausências/relógio — `C:564`, `C:610`, `C:713`; precedência — `C:1674`. LAV preserva `precedencia` anterior — `P:732`. | Sim por EJ; precedência aplicada no julgamento unitário após a lavra — `C:1721`; mesmo veredito com ata diferente também relavra — `P:735`, `P:767`. | PJ; decisão admin não impede atualizar ata — `P:738`, `P:741`. | **≥8**: `dia_sem_trabalho` (`J:407`); `_celula_furada` (`J:442`); `grade_da_celula` (`L:331`); `montar_previa` (`folha/previa.py:83`); `referencia_do_espelho` (`folha/validacao_pdf.py:47`); `periodos_do_dia` (`ponto/juiz_batida.py:157`); LAV (`P:731`); PREC (`P:816`). | Não diretamente — `C:333`; pode influenciar insumos indiretos, conforme grafo. |
| `ausencia` | CAR `P:802`; LOTE `P:838`, `P:843`; relação `SET_NULL` — `M:1764`. | `ausencia_cobre(..., para='cobranca')` — `C:981`, `C:1106`; backfill — `ponto/management/commands/backfill_b5_insumos.py:72`. | Sim por EJ/EB; deleção também pode nulificar — `M:1765`. | CAR: PJ; LOTE: PB; `SET_NULL`: nenhum declarado no campo — `M:1765`. | **4**: `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:75`); CAR (`P:797`); LOTE (`P:835`); `assinaturas_do_colab` (`ponto/services/esmeril_espelho.py:133`). | Não: o hash recebe **status da cobertura consultada**, não esta FK — `C:335`, `C:1106`, `C:1148`. |
| `tem_entrada` | CAR `P:803`; LOTE `P:839`, `P:843`. | `insumos_da_ata(ata,dna)`: lâmpada externa E/`dna.marcos.hi`; sem lâmpadas consulta `n_orfas` — `C:643`, `C:649`, `C:650`, `C:665`. | Sim por EJ/EB — `C:1578`, `ponto/management/commands/backfill_b5_insumos.py:73`. | PJ/PB. | **3**: `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:80`); CAR (`P:798`); LOTE (`P:836`). | Não — `C:333`. |
| `tem_saida` | CAR `P:803`; LOTE `P:839`, `P:843`. | Mesma função, lâmpada externa S/`dna.marcos.hf` — `C:650`, `C:655`, `C:665`. | Sim por EJ/EB — `C:1578`, `ponto/management/commands/backfill_b5_insumos.py:73`. | PJ/PB. | **3**: `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:80`); CAR (`P:798`); LOTE (`P:836`). | Não — `C:333`. |
| `insumos_em` | CAR `P:804`; LOTE `P:840`, `P:843`. | Argumento `agora`; backfill lê relógio — `P:804`, `ponto/management/commands/backfill_b5_insumos.py:59`. | Sim quando carimbo/insumos diferem; repetição idêntica não grava — `P:795`, `P:835`. | PJ/PB. | **3**: `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:71`); CAR (`P:796`); LOTE (`P:835`). | Não — `C:333`. |
| `status_dia` | `_aplicar_status`, dentro de LAV; SAVE/CAR persistem — `P:705`, `P:743`, `P:760`, `P:773`. | `_status_alvo`: veredito/via + fatos atuais da grade — `P:694`, `P:698`; mapa admin — `S:52`, `S:64`; fatos incluem batidas, previsto/realizado, isenção, parcial e precedência humana — `S:124`, `S:143`. | Sim por EJ/EA; EB completa somente nulo/carimbo atual — `C:726`, `C:733`. | Lavra normal PJ; command PS. | **5**: `status_do_dia_lavrado` (`S:181`); `_completar_status` (`C:726`); `_status_alvo` (`P:697`); `_aplicar_status` (`P:703`); LAV (`P:723`). | Não — `C:333`. |
| `status_rotulo` | Mesmo escritor/persistência de status — `P:705`, `P:743`, `P:760`, `P:773`. | Regra de status e vocabulário; exceção de rótulo do isento — `S:28`, `S:48`, `S:60`, `S:120`. | Sim por EJ/EA/EB, no mesmo ato do status — `P:705`, `C:733`. | PJ/PS. | **3**: `status_do_dia_lavrado` (`S:182`); `_status_alvo` (`P:697`); `_aplicar_status` (`P:703`). | Não — `C:333`. |

O produtor atual emite nove chaves de topo em `ata_do_dia`; PREC acrescenta a décima, `precedencia` (`C:567`, `C:591`, `P:819`). Há oito chaves possíveis em cada item de `lampadas`, contando as opcionais (`C:501`, `C:540`, `C:424`, `C:620`, `C:714`), e seis dentro de `precedencia` (`C:1675`). Os elementos de `orfas` e `precedencia.suprimidos` são strings, sem subchaves (`C:574`, `ponto/precedencia.py:351`).

Na tabela seguinte, **LAV/CAR** significa a preparação de `ata` por LAV e sua persistência por SAVE ou CAR (`P:768`, `P:775`, `P:807`). **EJ/PJ** mantém exatamente os eventos e portões definidos acima. Todas as chaves têm **FP não diretamente**, pelo corpo de `impressao_insumos` (`C:333`–`C:390`).

| Chave de `ata` | Escritores de produção | Fonte do valor gravado | Regravação/evento | Portão | Leitores comprovados; contagem | FP |
|---|---|---|---|---|---|---|
| `tipo_dia` | `ata_do_dia` → LAV/CAR — `C:567`. | `dia.tipo_dia`; overlays FolgaDia/Ausencia/isenção na grade — `U:1109`, `U:1137`, `U:1785`. | Sim, EJ — `C:1557`. | PJ. | **3**: `dia_sem_trabalho` (`J:408`); `grade_da_celula` (`L:344`); `medir_ata_fantasma` (`ponto/management/commands/tripwire_celulas.py:342`). | Não — `C:333`. |
| `lampadas` | `lampadas_do_dia`/`ata_do_dia` → LAV/CAR — `C:565`; transformações por dispensa/teto — `C:615`, `C:712`. | Match da grade, marcos/DNA, órfãs e sinal de realizado — `C:444`, `C:456`, `C:565`. | Sim, EJ — `C:1557`. | PJ. | **≥8**: `_celulas_da_ata` (`L:93`); `n_ok_lavrado` (`L:81`); `_celula_furada

[truncado em 20000 de 37797 caracteres]

## 31/10 16:12 · sessao 16cb111707cb · - · - · origem=Ronald (tmux)

Leitores enviados nas mensagens anteriores, com até oito funções comprovadas por coluna/chave e contagens do conjunto enumerado. O total absoluto do repositório permanece `[nao verificado]`.

Correções finais:

- `ata.precedencia`: **2 leitores confirmados** — `carimbar_precedencia` (`app/ponto/portas/celula.py:816`) e `lavrar_veredito`, que preserva a chave durante a substituição da ata (`app/ponto/portas/celula.py:732`).
- `julgada_em`: **7 funções confirmadas** nas referências enviadas; `processar_cartorio` não conta como leitor direto desse campo: seleciona por colaborador/data e delega (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:61`).
- `regenerada_em`: retire a referência a `regenerar_celulas_vinculo:207`; aquela linha lê `data`, não `regenerada_em` (`app/ponto/portas/celula.py:207`).
- Os nomes corretos são `_chave_memo_pv` para a leitura de veredito/via (`app/chamados/juizes.py:793`) e `decisao_do_dia` para a leitura ORM de veredito/via/em (`app/ponto/services/veredito_celula.py:109`).
- `ata.marcos`: nenhuma chave produtora/leitor direto localizado `[nao verificado]`; o construtor atual emite `lampadas`, não `marcos` (`app/ponto/services/cartorio.py:567`).
- `editada_por`, `ata.lampadas[].dispensada` e os seis filhos de `ata.precedencia`: nenhum leitor individual localizado; exaustividade `[nao verificado]`.
- Os totais enviados cobrem funções confirmadas de `app/`, incluindo comandos operacionais; total semântico dos scripts de `bin/` `[nao verificado]`.

## 31/10 16:12 · sessao 758e4f350d15 · - · - · origem=Ronald (tmux)

Todos os caminhos abaixo são leitura estática; referências relativas à raiz.

| Campo | Escrita, fonte e regravação comprovadas |
|---|---|
| `julgada_em` | `carimbar`: recebe `agora`, grava `cel.julgada_em=agora`; salva por `salvar_e_emitir` (`app/ponto/portas/celula.py:800`, `:807`, `:640`). Regrava quando hash/ausência/pontas/lavra diferem; caso idêntico retorna sem salvar (`:795-799`). |
| `impressao` | `carimbar`, valor `impressao_insumos`; atribuição `app/ponto/portas/celula.py:801`; chamadas do cartório `app/ponto/services/cartorio.py:998`, `:1148`, `:1579`. |
| `ausencia` | `carimbar` e `carimbar_insumos_em_lote`, atribuições `app/ponto/portas/celula.py:802`, `:838`; `bulk_update` `:843`. Fonte `ausencia_cobre(...para='cobranca')`: `app/ponto/services/cartorio.py:981`, `:1106`; backfill `app/ponto/management/commands/backfill_b5_insumos.py:72`. |
| `tem_entrada`, `tem_saida` | `carimbar` e lote: `app/ponto/portas/celula.py:803`, `:839`, `:843`. Fonte `insumos_da_ata(ata,dna)` `app/ponto/services/cartorio.py:1578`; lê `ata.lampadas` e `dna.marcos.hi/hf`, confrontando tipo/hora/acesa; sem lâmpadas usa `n_orfas` (`:643-665`). |
| `insumos_em` | `carimbar` e lote; relógio `agora` (`app/ponto/portas/celula.py:804`, `:840`). |
| `veredito` | `lavrar_veredito` atribui/salva (`app/ponto/portas/celula.py:770`, `:775`). Cartório usa literal ou `derivar_veredito(codigos)` (`:647-664`); fonte normal classificação da grade, cobertura, batidas, chamados/fio, cadastro e relógio (`app/ponto/services/cartorio.py:1204-1217`, `:1244-1245`). Admin prevalece sobre cartório (`app/ponto/portas/celula.py:738-744`). |
| `veredito_via` | `lavrar_veredito`, parâmetro `via` (`app/ponto/portas/celula.py:771`, `:775`); validado contra vias declaradas (`:720-721`). |
| `veredito_em` | `lavrar_veredito`, `agora or timezone.now()` (`app/ponto/portas/celula.py:772`). Atualização exclusiva do status não o move (`:753-766`). |
| `protestos` | `lavrar_veredito`, cartório copia `codigos`, admin preserva lista existente (`app/ponto/portas/celula.py:746`, `:751`, `:769`). |
| `ata` | `lavrar_veredito` (`app/ponto/portas/celula.py:741`, `:768`, `:775`) e `carimbar_precedencia` (`:819-822`). Lavra preserva somente chave de outra operação `precedencia` (`:94`, `:730-734`). |
| `status_dia`, `status_rotulo` | `_aplicar_status` atribui ambos (`app/ponto/portas/celula.py:701-706`), persistidos por `lavrar_veredito` (`:743`, `:760`, `:773-775`). Fonte `_status_alvo`: decisão admin ou `status_da_lavra(veredito=v, **fatos)` (`:686-698`). Fatos da grade: tipo, previsto, batidas casadas/órfãs/realizado, isenção e parcial; para `indefinida`, precedência de FolgaDia/feriado (`app/ponto/status_dia.py:124-149`). Regras/rotulagem `:60-121`. |

Portões e gatilhos:

- Cartório normal: fila de **todas** as células passadas; `processar_cartorio.Command.handle` não recorta exportação/tranca/FechamentoMensal (`app/ponto/management/commands/processar_cartorio.py:49-65`). Funções de persistência acima tampouco consultam esses portões (`app/ponto/portas/celula.py:709-843`).
- `REGRA_VERSOES=((1,'2026-09'),(2,'2026-10'))` (`app/ponto/services/cartorio.py:110`). `versao_para` usa `janela_atual` e maior versão cujo horizonte alcança competência (`:191-197`): entra no hash (`:352-353`), não impede escrita. `forcar` ignora igualdade da impressão (`:1161-1163`).
- `lavrar_status_competencia` **barra entregue** por `competencia_entregue` (`app/ponto/management/commands/lavrar_status_competencia.py:40-42`). Executa `julgar_colab(completar_status=True)` (`:55-56`); só completa status NULL com carimbo atual, lendo `veredito/protestos` existentes (`app/ponto/services/cartorio.py:726-735`). Não confundir essa guarda com a lavra cotidiana.
- Backfill insumos seleciona apenas `julgada_em NOT NULL`, não tem corte competência; lê ausência/ata/dna e chama lote (`app/ponto/management/commands/backfill_b5_insumos.py:60-84`).
- Cron real `processar_cartorio --apply`: **06:27**, dependente de `reconciliar_grade` (`app/config/crons.py:597`); comentários do serviço citando 06:28 não são o agendamento atual. Vigia de hora: `*/10 * * * *`, `--apply` (`:348`).
- Signals: batida save/delete (`app/ponto/signals.py:132-140`); Ausencia save/delete (`:144-152`); Justificativa save (`:162-165`); todos chegam a `julgar_celula` (`:85-93`). Chamado criado ou com mudança `status_local`: `app/chamados/signals.py:165-180`.
- `julgar_celula` barra futuro/inexistente e, salvo teto, espera turno de hoje (`app/ponto/services/cartorio.py:1700-1713`); aplica precedência após julgar (`:1721-1723`).

`ata` atual tem **10 chaves de topo**: nove produzidas por `ata_do_dia`, mais `precedencia`. `ata.marcos` não aparece nos produtores atuais abaixo; `marcos` pertence a `dna.marcos` ou à grade (`app/ponto/services/cartorio.py:444-449`, `:932`).

| Chave de ata | Produtor/fonte |
|---|---|
| `tipo_dia` | `ata_do_dia`: `dia.tipo_dia` (`app/ponto/services/cartorio.py:567`). |
| `lampadas` | `ata_do_dia` chama `lampadas_do_dia` com células da grade, marcos, DNA, órfãs e sinal de realizado (`:564-567`). |
| `minutos_previstos` | Inteiro de `dia.minutos_previstos` (`:568`); grade calcula segmentos previstos E→S (`app/escala/utils.py:1578-1582`); intermitente pode copiar realizado (`:2008-2010`). |
| `minutos_realizados` | Inteiro de `dia.minutos_realizados` (`app/ponto/services/cartorio.py:569`); grade usa `realizado_do_dia` e zero explícito quando sem turno (`app/escala/utils.py:1985-1998`). |
| `sem_ancora` | Booleano do dia da grade (`app/ponto/services/cartorio.py:570`). |
| `n_orfas` | `len(dia.orfas)` (`:571`). |
| `orfas[]` | Horas convertidas em `str`; elementos sem subchaves (`:574`). |
| `n_missing` | Contagem `lampadas[].acesa is False`, ou missing da grade (`:575-576`); recalculada após dispensa (`:622`) e teto (`:716`). |
| `n_celulas` | `len(lampadas)` (`:591`). |
| `lampadas[].tipo` | Papel do marco do DNA/grade (`:449`, `:501`, `:540`, `:551`). |
| `lampadas[].hora` | Hora prevista do DNA/grade (`:501`, `:540`, `:551`). |
| `lampadas[].acesa` | Match da grade, proximidade quando marco ausente da régua, ou desconhecido (`:495-510`, `:540-553`); ausência aprovada integral/justificativa pode tornar True (`:610-620`); teto torna None (`:713-714`). |
| `lampadas[].luz` | Hora real casada, ou None (`:501-507`, `:541`, `:552`); dispensa zera luz (`:619`). |
| `lampadas[].tipo_real` | Tipo real fornecido pela grade; None quando ausente/proximidade sem tipo (`:503-507`, `:542`, `:553`). |
| `lampadas[].isencao` | Opcional True quando célula da grade marca isenção (`:424-425`, `:543`, `:554`). |
| `lampadas[].dispensada` | Opcional tipo da Ausencia integral aprovada, ou `'justificativa'` (`:610-620`). |
| `lampadas[].pendente` | Opcional True quando marco ainda não venceu (`:713-714`). |
| `precedencia` | `aplicar_precedencia_na_celula` consulta `fatos_do_dia` e chama `carimbar_precedencia` (`:1673-1682`); grava `app/ponto/portas/celula.py:819-822`. |
| `precedencia.tipo_do_dia` | Vencedor dos fatos (`app/ponto/services/cartorio.py:1676`; `app/ponto/precedencia.py:349-356`). |
| `precedencia.faixa` | Faixa do vencedor (`app/ponto/services/cartorio.py:1677`; `app/ponto/precedencia.py:350`, `:357`). |
| `precedencia.rotulo` | `ROTULOS`/tipo capitalizado (`app/ponto/services/cartorio.py:1678`; `app/ponto/precedencia.py:358`). |
| `precedencia.suprimidos[]` | Lista de **strings**, faixas perdedoras únicas (`app/ponto/services/cartorio.py:1679`; `app/ponto/precedencia.py:351-354`). Não contém dicionários/subchaves; vocabulário `PRECEDENCIA` (`:46`). |
| `precedencia.era_dia_previsto` | `trabalha` da célula, ou cálculo EC quando desconhecido (`app/ponto/services/cartorio.py:1680`; `app/ponto/precedencia.py:305-327`). |
| `precedencia.texto` | `texto_da_supressao(f)`; deriva vencedor e suprimidos (`app/ponto/services/cartorio.py:1681`; `app/ponto/precedencia.py:375-380`). |

Fontes adicionais da ata:

- Grade carrega EC/template, células da janela e FolgaDia (`app/escala/utils.py:1240-1260`, `:1356-1376`), batidas (`:1267`), aplica overlays FolgaDia/Ausencia/isenção (`:1785-1790`).
- FolgaDia altera tipo, limpa células/órfãs (`:1109-1120`); Ausencia vem de cobertura para folha, Justificativa aprovada do banco (`:1137-1159`, `:1170-1203`).
- Precedência consulta Ausencia não rejeitada, FolgaDia, feriados da praça e vínculos/células (`app/ponto/precedencia.py:227-245`, `:271-307`, `:332-360`).
- Não encontrei caminho comprovado de `AplicacaoConvencao`/régua CCT à produção da ata/status; atribuir essa fonte: `[nao verificado]`. A “régua” do veto do fingerprint vem de template degenerado/suspeito (`app/ponto/services/furos_vetados.py:33-43`).

Fingerprint:

`sha256('|'.join(partes).encode()).hexdigest()[:64]` (`app/ponto/services/cartorio.py:390`).

Partes obrigatórias: batidas `pk:tipo:timestamp.isoformat`, cobertura `str(status)`, chamados `pk:status_local`, **somente** `dna.dna_versao` concatenado com `dna.marcos` (`:333-338`). Opcionais: veto (`:339-340`), teto de marcos vencidos (`:344-345`), versão (`:352-353`), turno em curso `enc` (`:362-363`), quantidade de fios vivos (`:377-378`), parcial e estado de período (`:385-389`). Chamador normal fornece esses fatos (`:1104-1153`); nunca-bateu usa batidas/chamados vazios, cobertura, DNA, versão e parcial (`:998-999`). **Ata e demais colunas não são partes diretas** nesse corpo (`:333-389`).

Arestas principais, origem → destino:

- `dna → impressao`: `app/ponto/services/cartorio.py:337`.
- `data → impressao`: versão por competência e teto/turno (`:945-947`, `:1104-1153`).
- `ata.lampadas`, `ata.n_orfas`, `dna.marcos → tem_entrada/tem_saida`: `:643-665`.
- `veredito`, `veredito_via → status_dia/status_rotulo`: `app/ponto/portas/celula.py:694-698`; regra/rotulação `app/ponto/status_dia.py:64-121`.
- `protestos → veredito/status` no completar-status: `app/ponto/services/cartorio.py:733-735`.
- `trabalha → veredito`: `_protesta` `:1204-1208`; cadastro sem vínculo lê trabalha (`app/ponto/supra_juiz.py:170-174`).
- `trabalha → ata.precedencia.era_dia_previsto → demais precedencia`: `app/ponto/precedencia.py:305-340`, `:349-360`.
- `ata.precedencia anterior → ata nova`: preservação `app/ponto/portas/celula.py:730-734`.
- `ata.lampadas → ata.n_missing/n_celulas`: `app/ponto/services/cartorio.py:575`, `:591`.
- `ata.lampadas atuais → veredito`: `app/ponto/services/cartorio.py:1182-1208`, `:1244-1245`.

**Há ciclo temporal comprovado `ata anterior → ata nova`:**

1. Grade prévia chama `turnos_do_colab` alimentando células persistidas (`app/escala/utils.py:1519-1526`).
2. `turnos_do_colab` chama `papel_por_minuto_da_ata` (`app/ponto/turnos.py:1911`), que consulta `periodos_do_dia(cel,te)` (`:1629-1636`); juiz lê `ata/dna` (`app/ponto/juiz_batida.py:157-161`).
3. Grade passa turno ao matcher (`app/escala/utils.py:1643-1645`); matcher usa papéis para construir células (`:562-585`).
4. Cartório transforma células em ata persistida (`app/ponto/services/cartorio.py:564-567`, `:1557-1580`).

Também há dependência de **outra célula**: as luzes de todas são acumuladas e impedem tratar a luz vizinha como órfã (`app/ponto/turnos.py:1652`, `:1733-1735`); reconciliação pode retirar órfã de D+1 ao preencher saída de D (`app/escala/utils.py:1911-1913`, `:1936`, `:1979-1980`). Para calcular realizado, a grade já substitui atas por cópias com lâmpadas atuais em memória (`:1821-1838`), mas isso não remove a leitura anterior usada no pré-alinhamento (`:1519-1526`).

_1140 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
