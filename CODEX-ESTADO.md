# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 11:59** · publicado em **31/10/2026 12:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 11:59 · sessao 4fabd3e3aa53 · - · - · origem=Ronald (tmux)

Os três vizinhos não têm o mesmo resultado:

- **O111 perde alcance sobre o lado motor.** A proibição verifica somente `k = algo.date()` dentro de `handle`; removido o agrupador, passa sem examinar quem agora escolhe o dia: `dia_pago._dia_de`, que mantém fallback de calendário. A verificação textual permanece satisfeita pelo mapa usado para diagnóstico. Portanto preserva a proibição local, mas já não prova o agrupamento efetivo do v2. (`app/ponto/tests/test_dia_da_jornada_e_legivel.py:98-132`; `app/ponto/services/dia_pago.py:126-132`; comando antes: `app/ponto/management/commands/diff_calculador.py:211-244`.)

- **S5b mantém a pergunta do lado calculador:** continuam existindo chamadas reais de `insumos_do_calculador` e `rubricas_por_dia`; nenhum `do_dia` nem pareador proibido foi introduzido. Reproduzi seus predicados AST em memória após retirar o bloco motor: todos satisfeitos. Isso não comprova a nova delegação v2, pois essa não é a pergunta desses predicados. (`app/ponto/tests/test_s5b_porta_unica.py:60-101`; comando antes: `app/ponto/management/commands/diff_calculador.py:309-324`, `414-424`.)

- **Janela mantém seu alvo:** a chamada `minutos_do_oraculo(ts, pares=_pares)` continua presente e a chamada da porta continua fornecendo o caminho aceito pelo detector. Executei em memória a função AST extraída do teste: `[]` antes e após retirada do bloco motor. (`app/ponto/tests/test_chamador_do_calculador_aplica_a_janela.py:61-111`; comando antes: `app/ponto/management/commands/diff_calculador.py:312`, `414`.)

O **teste novo morde reversão integral**, pois desapareceriam os imports de `lavra_sem_gravar` e voltariam os atributos proibidos no `handle`. Entretanto **não morde a classe “derivação própria dentro de `lavra_sem_gravar`”**: os exemplos conferem valores fixos; a AST inspeciona exclusivamente `handle`; a suposta prova de chamada exige somente um `ast.Name`, sem exigir `Call` nem ligação entre retorno e `m_dia`. Um helper poderia chamar a lavra, descartar a captura e recomputar os mesmos valores, preservando todas essas provas. (`app/ponto/tests/test_diff_calculador_lado_v2_e_a_lavra.py:41-48`, `53-76`, `78-93`, `99-117`, arquivo novo conforme anexo.)

Os testes novos também não executam `Command.handle`, `_captar`, as guardas de versão/repetição, a obtenção dos kwargs reais ou os novos contadores. Assim, o instrumento pode perder a captura na integração sem essas provas comportamentais avermelharem. (`app/ponto/tests/test_diff_calculador_lado_v2_e_a_lavra.py:41-48`, `53-93`, `99-117`, conforme anexo.)

Não executei a suíte Django nem apliquei arquivos; o resultado de execução integral é **[não verificado]**. [nao verificado]

## 31/10 11:57 · sessao 3c9a49c5e069 · - · - · origem=Ronald (tmux)

Auditoria de fluxo, somente leitura. Prefixos abaixo: `F = app/ponto/services/fechamento.py`, `D = app/ponto/services/dia_pago.py`, `M = app/ponto/models.py`, `C = app/ponto/management/commands/diff_calculador.py`.

Tabela completa dos kwargs da chamada **motor** que `_captar` intercepta; o fechamento constrói a chamada em F:1168–1205 e `lavrar` recebe os parâmetros em D:135–141:

| Campo/valor | Escritor/origem | Leitor em `lavrar` |
|---|---|---|
| `colab`, `mes`, `ano` | F:432; colaborador iterado F:598 | Constructor dos dias/ajuste e filtro de substituição D:343,355,357 |
| `periodos` | Composto nasce F:182; apuração fatia F:270; composição concatena F:278–281; resultado entregue F:729–731 | D:173–186 |
| `ft_certa` | Partição por juiz de escala certa F:909–910; retira feriados F:926–928 | D:234–240; noturno/intra D:223–225 |
| `ft_feriado` | F:920–925, por escala vigente e `feriado_vira_folga`; mesmas identidades retiradas da certa F:927–928 | D:244–256 |
| `ft_sem_escala` | Complemento **por identidade** das outras duas partições F:948–949 | D:257–265 |
| `orfaos_por_dia` | Inicializa F:766; seleciona dias de calendário descobertos F:768–771; motor órfão F:802–804; soma por calendário F:813–817 | D:267–270 |
| `faltas_por_dia` | Inicializa F:828; autoridade `folha.export.minutos_de_falta_por_dia` F:848–850 | D:272–275 |
| `atraso_por_dia` | F:966 chama `credito_parcial.atraso_por_dia`; escritor deste mapa em `app/ponto/services/credito_parcial.py:36–44`, dia local calendário em :29–33 | D:190–195 |
| `credito_por_dia` | F:967; `credito_parcial.py:15–26` consulta ausências por autoridade e agrega minutos pagos pela `a.data` | D:191–195 |
| `reflexo_dsr` | Resultado composto soma F:282–284; fecha arredondado a quatro casas F:1071 | Ajuste, D:349 |
| `banco` | Resultado composto F:282–284; zero se sem movimento ou arredondado duas casas F:1072 | Ajuste, D:350 |
| `abono_por_dia`, `previsto_por_dia`, `realizado_por_dia` | Inicializa F:833; grade F:981–982; `por_dia_da_grade` F:1028–1029; derivação canônica D:384–400 | D:286–290 |
| `dia_da_jornada` | Motor escreve `id(p) -> dia`: `app/ponto/motor_calculo_v2.py:2869–2897`; fechamento **mescla**, preserva objetos F:278–281,295–300 | `_dia_de` D:109–132, chamado nos quatro laços D:174,235,245,258 |
| `furos_do_motor` | Motor `resultado.datas_falta`; teto uma vez F:1083–1085; juiz registrado chamado F:348–362; instante da passada F:595 | Gera linhas mesmo vazias D:279–281; booleano D:344 |
| `versao='motor'` | Literal F:1195 | Metadados D:333; filtro delete D:357 |
| `versao_motor` | Carimbo por fatia F:265; último carimbo entregue F:303,740 | Metadados e fallback D:333,340–345 |
| `regua_por_dia` | Escrito por dia de cada fatia F:268–269; entregue F:303,739 | D:336–345; contador de dias sem régua no ajuste D:353–354 |
| `evento_origem` | `'backfill'` no modo leitura, `'recalcular_fechamento_mes'` no modo escrita F:1205 | D:333–334; gravado nos constructores D:343–356 |

Os kwargs `versao_dna`, `rubricas`, `do_motor` não são enviados na chamada motor: ficam nos defaults D:139–141. `rubricas` é enviado pela chamada **oráculo**, F:1256; a chamada reutiliza `_lavrar_dia_pago` importado antes, F:1168,1245. O captador do anexo torna esse ramo no-op. [Anexo `fila1_cura.diff`, hunk de `_captar`; F:1233–1264.]

Comparabilidade e diferenças:

- **Para os mesmos insumos**, a nova captura reproduz os campos numéricos das linhas de dia que esta `lavrar` produziria. A função importa `DiaPago` dinamicamente em D:163, constrói as linhas em D:343–356 e só depois chama `filter(...).delete()` e `bulk_create`, D:357–358. O proxy do anexo intercepta exatamente essas operações. [D:163,343–358; anexo `fila1_cura.diff`, `lavra_sem_gravar`.]
- Não há perda por conversão de `Decimal` neste alvo: as rubricas e campos de grade de `DiaPago` são `FloatField`, M:326–351. O `FechamentoMensal` tem várias rubricas `DecimalField(...decimal_places=2)`, M:412–428; comparar com ele é outra pergunta.
- Os dias de períodos normais/FT seguem o mapa da jornada, D:128–132,174,235,245,258; **a classificação da FT continua por data local de entrada**, F:909–924. Atraso e órfãos também continuam agrupados por calendário, `credito_parcial.py:29–43` e F:813–817. São assimetrias preexistentes que a captura passa a mostrar fielmente.
- A nova referência incorpora, além das três rubricas faltantes, trabalhadas dos órfãos/FT sem escala, HE100 da FT feriado, noturno/intra das três partições FT e atraso líquido após crédito, D:178–193,223–274. A tabela anterior somava diretamente períodos e FT em C:249–268.
- Ajuste, detalhes, régua, furo, identificação e origem não são devolvidos pelo captador; `CAMPOS_DIA` inclui 15 campos e os dois de ajuste ficam separados, D:34–44; produção escreve metadados/detalhes/furo em D:333–356. `evento_origem` no DIFF é especificamente `'backfill'`, F:1205.
- Igualdade com as linhas **já gravadas hoje em produção**, incluindo alteração histórica de critério e insumos, é [não verificado]: não foram fornecidos registros de `DiaPago`.

Achado adicional importante:

**A triagem erra o contador para os excluídos pela guarda.** A guarda remove colaboradores **antes** do laço, F:545–548. O composto `ResultadoMes` só nasce dentro da apuração desse laço, F:598,729 e F:182. O comando testa `_res_fech` e dá `continue` com `sem_resultado_do_fechamento` **antes** do novo teste `_lav_fech`, C:154–162 mais hunk novo do anexo. Portanto um colaborador excluído pela guarda cai em **`sem_resultado_do_fechamento`**, não em `sem_lavra_do_fechamento`. [F:545–548,598,729,182; C:154–162; anexo `fila1_cura.diff`, hunk `@@ -160...`.] [nao verificado]

A guarda da 09/2026 existe na cópia, F:36–43; `somente_leitura=True, lavrar_dias_pagos=True` ativa a classificação como escrita, F:523–524. Assim, **se a empresa estiver exportada**, o instrumento exclui os guardados e não gera DIFF deles; quais empresas satisfazem isso no dado de produção é [não verificado]. A promessa de prova de frota da 09 precisa considerar essa exclusão. [F:523–558; C:140,154–162; anexo de triagem, itens b2/d7.]

A lavra capturada não escreve banco, mas o **comando inteiro pode gravar auditoria preexistente**: F:551–557 chama `evento_kw`; este chega a `LogAuditoria.objects.create` em `app/core/observ.py:142–148,133–138`. Portanto “modo somente leitura” global continua falso no ramo guardado; a afirmação estreita “a lavra não grava” é sustentada pelo anexo e D:357–358.

## 31/10 11:57 · sessao ac1a74f5de34 · - · - · origem=Ronald (tmux)

Achados da janela de substituição, com referências do lado DEPOIS calculadas pelo diff:

- **Caminho síncrono atual:** a função real `lavrar` importa `DiaPago` dentro da chamada, portanto recebe a classe captadora; usa constantes, construtor, `filter(...).delete()` e `bulk_create(...)`, todos cobertos pelo substituto. Seus auxiliares chamados nesse ramo não leem o modelo. Não identifiquei outro leitor síncrono de `DiaPago` nessa pilha motor. (`app/ponto/services/dia_pago.py:163`, `:167`, `:173`, `:223`, `:310`, `:343`, `:355`, `:357`; DEPOIS `diff_calculador.py:48`, `:52`, `:62`, `:70`.)

- **A troca é global ao processo:** outros importadores locais receberiam a classe falsa caso executassem simultaneamente no mesmo processo: `retratar_exportada`, soma da competência, vigia de furo, apuração da exportada, contagem de abonos, soma do período e linhas do período; também leitores da porta de exportação e dois comandos. Concorrência efetiva em produção é **[não verificado]**. (`app/ponto/services/dia_pago.py:442`, `:556`, `:585`, `:637`, `:695`, `:777`, `:852`; `app/folha/porta_export.py:71`, `:283`, `:330`; `app/ponto/management/commands/lavrar_dias_pagos.py:82`; `app/ponto/management/commands/apurar_furo_da_exportada.py:58`.)

- **Risco estrutural da concorrência:** uma chamada concorrente que já tivesse a função real poderia importar a classe falsa e adicionar suas linhas ao mesmo `_linhas`; a projeção final filtra somente `tipo` e `data`, sem conferir colaborador, competência ou versão. Outra chamada de fechamento que importasse `lavrar` durante o patch receberia `_captar`, alimentaria a medição e deixaria de executar a persistência real. Ocorrência concreta é **[não verificado]**. (`app/ponto/services/dia_pago.py:163`, `:343`, `:357`; `app/ponto/services/fechamento.py:1168`; DEPOIS `diff_calculador.py:59`, `:70`, `:78`, `:81`, `:203`, `:220`.)

- **Outro leitor certo de `dia_pago.lavrar`:** o fechamento faz o import local na linha 1168; o alias obtido é reutilizado para a chamada `versao='oraculo'`. O novo captador torna essa chamada um no-op explícito. O modelo falso já foi restaurado antes dessa segunda chamada sequencial. (`app/ponto/services/fechamento.py:1168`, `:1245`, `:1262`; DEPOIS `diff_calculador.py:74`, `:199`.)

- **A afirmação “qualquer coisa nova pedida ao modelo levanta AttributeError” é excessiva:** o construtor aceita quaisquer campos e `filter` aceita e ignora quaisquer filtros; mudanças nessas entradas podem passar silenciosamente. Não há falha atual desse tipo demonstrada, apenas ausência dessa garantia. (DEPOIS `diff_calculador.py:54`, `:58`, `:67`.)

- **Classes pedidas:** a cura não acrescenta regra monetária nem chamada de relógio; usa a distribuição existente, e os valores reais são `FloatField`, portanto o `float(...)` final não introduz uma conversão de Decimal ou arredondamento diário neste código. Não há aquisição nova de trava no helper; a fragilidade é falta de isolamento do patch global, não inversão demonstrada de trava. (`app/ponto/services/dia_pago.py:173`, `:190`, `:234`, `:244`, `:257`, `:267`; `app/ponto/models.py:326`, `:349`; DEPOIS `diff_calculador.py:25`, `:70`, `:81`.)

- **Limite de equivalência:** captura preserva rubricas numéricas do construtor, mas não reproduz o banco, suas restrições e o `auto_now`; descarta metadados e linha de ajuste na projeção. (`app/ponto/models.py:374`, `:380`, `:386`; `app/ponto/services/dia_pago.py:343`, `:355`; DEPOIS `diff_calculador.py:79`, `:81`.)

- **Escrita preexistente fora da lavra:** a chamada com `somente_leitura=True, lavrar_dias_pagos=True` continua entrando no predicado de escrita e pode registrar auditoria da guarda; a porta de auditoria faz `LogAuditoria.objects.create`. A guarda remove colaboradores antes da lavra. (`app/ponto/services/fechamento.py:523`, `:545`, `:548`, `:553`; `app/core/observ.py:94`, `:134`, `:142`.)

- **Leis:** L-095 identifica `dia_pago.lavrar` como sítio da lavratura; L-110 distingue medida de impacto do código velho de validação pela regra. A leitura da autoridade é adequada ao instrumento de impacto, mas não certifica a correção legal do valor produzido. (`app/docs/LEIS.md:113`, `:136`.)

## 31/10 11:50 · sessao 52743d37e746 · F1C4-3-EMISSORES · verificador · origem=Code

**1. P1 — O pré-requisito de pouso não está comprovado.** A sombra com **novos CHAMADOS por emissor**, e o atendimento ao teto de 50, são **[não verificado]** no material fornecido. Isso impede atestar o pouso; não prova que alguém já pousou antes da medição. A exigência de respeitar literalmente um aval condicional consta de `CLAUDE.md:37`. [nao verificado]

Há ainda uma diferença de unidade que a medição precisa respeitar:

- A fábrica seleciona **motivos de perguntas** e depois gera perguntas de disputa. (`app/chamados/services/fabrica_da_celula.py:111`, `app/chamados/services/fabrica_da_celula.py:124`)
- Materialização retorna uma **absolvição da guarda de paridade**, que pode permitir plantio; não é, nessa função, nascimento de chamado. (`app/chamados/services/materializacao.py:167`, `app/chamados/services/materializacao.py:818`)
- Cluster efetivamente chama `ChamadoColaborador.abrir` e conta os criados. (`app/ponto/services/cluster_espurio.py:87`, `app/ponto/services/cluster_espurio.py:105`)
- Intervalo **não emite chamados**: reconcilia legado e termina em `continue`, com a emissão aposentada. Portanto, contar resultados de `furos_intervalo` como novos chamados seria incorreto. (`app/ponto/management/commands/detectar_intervalo_ausente.py:137`, `app/ponto/management/commands/detectar_intervalo_ausente.py:148`, `app/ponto/management/commands/detectar_intervalo_ausente.py:155`)

**2. P2 — A migração dos quatro sítios deixa caminhos divergentes; há também uma contradição preexistente de emissão versus fechamento.**

**Divergência nova:** a fábrica por evento admite `trabalha=None`, mas o primeiro universo do lote e o vigia de hora continuam selecionando apenas `trabalha=True`. Uma célula limbo sem chamado pode passar pelo evento e ficar fora da recuperação/conferência desses caminhos. Isso demonstra **omissão**, não fechamento contrário da mesma cobrança. (`app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `app/ponto/services/vigia_de_hora.py:21`)

O lote contorna `motivos_a_perguntar`: chama diretamente `motivos_pela_ata`. A promessa de mesma regra entre evento e lote, portanto, não se sustenta para esse novo universo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:111`, `app/chamados/services/fabrica_da_celula.py:5`)

**Contradição condicional preexistente:** considere a mesma célula com `trabalha=False`, ata `tipo_dia='trabalho'`, marco apagado e chamado de furo vivo sem pergunta:

1. O universo “fio mudo” do lote não filtra `trabalha` e consulta os motivos pela ata. (`app/chamados/management/commands/disparar_perguntas_competencia.py:155`, `app/chamados/management/commands/disparar_perguntas_competencia.py:167`)
2. O leitor prioriza `ata.tipo_dia`; `_marcos_faltantes` aceita trabalho e recolhe o marco apagado. (`app/escala/services/leitor_celula.py:344`, `app/chamados/services/declaracao_texto.py:95`, `app/chamados/services/declaracao_texto.py:106`)
3. Sem pergunta existente, o lote inclui o fato e chega ao escritor de perguntas. (`app/chamados/management/commands/disparar_perguntas_competencia.py:220`, `app/chamados/management/commands/disparar_perguntas_competencia.py:225`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`)
4. A fábrica por evento recusa esse dia; o fechamento considera a premissa do chamado regularizada porque `dia_sem_trabalho` responde verdadeiro. (`app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/models.py:1232`, `app/chamados/models.py:699`)

É uma contradição demonstrável no código para a **mesma cobrança/dia**, mas sua ocorrência na frota é **[não verificado]**. Não nasce deste diff: o predicado anterior informado também recusava `False`. (`app/chamados/services/fabrica_da_celula.py:80`; predicado anterior no anexo `emis2_diff.txt`)

**3. P2 — Dois testes da fábrica não mordem a cláusula que a docstring promete; os demais positivos têm conteúdo.** [nao verificado]

A docstring afirma que retirar a cláusula de `tipo_dia` faz os quatro cobrarem. Isso é falso para a fábrica: depois do juiz existe outro veto `tipo_dia != 'trabalho'` em `_marcos_faltantes`. Retirar a cláusula 2 — ou ignorar o juiz no gate da fábrica — mantém os testes de folga e ausência verdes. (`app/chamados/tests/test_f1c4_3_emissores.py:23`, `app/chamados/services/fabrica_da_celula.py:49`, `app/chamados/services/disputa_emissao.py:827`, `app/chamados/services/declaracao_texto.py:95`)

A tabela identifica cada teste pelas linhas do arquivo e a mutação de produção que altera sua expectativa. A execução da suíte Django real é **[não verificado]**; as conclusões abaixo decorrem dos caminhos citados. [nao verificado]

| Teste em `app/chamados/tests/test_f1c4_3_emissores.py` | Mutação que o deixaria vermelho |
|---|---|
| `test_RED_fabrica_limbo_pergunta`, linha 74 | Restaurar `trabalha is not True` no gate: retorna vazio. (`app/chamados/services/fabrica_da_celula.py:80`) |
| `test_fabrica_folga_nao_pergunta`, linha 77 | **Só mudar o juiz não basta**: é preciso também deixar a folga atravessar o veto duplicado do leitor. (`app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/services/declaracao_texto.py:95`) |
| `test_MORDE_fabrica_dia_de_trabalho_pergunta`, linha 80 | Suprimir os motivos retornados pela fábrica. (`app/chamados/services/fabrica_da_celula.py:111`) |
| `test_RED_fabrica_ausencia_lavrada_com_trabalha_true_nao_pergunta`, linha 83 | **Só retirar a cláusula 2 não basta**: retirar também o veto de ausência no leitor faz surgir o motivo. (`app/chamados/juizes.py:408`, `app/chamados/services/declaracao_texto.py:95`) |
| `test_RED_materializacao_limbo_furo_absolve_o_plantio`, linha 93 | Restaurar `trabalha is not True` no gate. (`app/chamados/services/materializacao.py:160`) |
| `test_materializacao_folga_nao_absolve`, linha 96 | Retirar a cláusula 2 do juiz: chega ao preenchimento do furo e absolve. (`app/chamados/juizes.py:408`, `app/chamados/services/materializacao.py:167`) |
| `test_MORDE_materializacao_dia_de_trabalho_absolve`, linha 99 | Suprimir o retorno verdadeiro após reconhecer o preenchimento. (`app/chamados/services/materializacao.py:168`) |
| `test_RED_materializacao_ausencia_lavrada_com_trabalha_true_nao_absolve`, linha 102 | Retirar a cláusula 2 do juiz. (`app/chamados/juizes.py:408`, `app/chamados/services/materializacao.py:167`) |
| `test_RED_cluster_limbo_conta`, linha 111 | Restaurar o veto `trabalha is not True`, ou o veto `sem_ancora`. (`app/ponto/services/cluster_espurio.py:27`) |
| `test_cluster_folga_nao_conta`, linha 114 | Retirar a cláusula 2 do juiz. (`app/chamados/juizes.py:408`, `app/ponto/services/cluster_espurio.py:31`) |
| `test_MORDE_cluster_dia_de_trabalho_conta`, linha 117 | Exigir três órfãs em vez de duas. (`app/ponto/services/cluster_espurio.py:31`) |
| `test_RED_cluster_ausencia_lavrada_com_trabalha_true_nao_conta`, linha 120 | Retirar a cláusula 2 do juiz. (`app/chamados/juizes.py:408`, `app/ponto/services/cluster_espurio.py:31`) |
| `test_cluster_sem_ancora_com_trabalha_true_conta`, linha 123 | Reintroduzir `or ata.get('sem_ancora')`. (`app/ponto/services/cluster_espurio.py:27`) |
| `test_RED_intervalo_limbo_conta`, linha 133 | Restaurar `trabalha is not True`, ou o veto `sem_ancora`. (`app/ponto/management/commands/detectar_intervalo_ausente.py:50`) |
| `test_intervalo_folga_nao_conta`, linha 136 | Retirar a cláusula 2 do juiz. (`app/chamados/juizes.py:408`, `app/ponto/management/commands/detectar_intervalo_ausente.py:70`) |
| `test_MORDE_intervalo_dia_de_trabalho_conta`, linha 139 | Suprimir a inclusão dos marcos encontrados no resultado. (`app/ponto/management/commands/detectar_intervalo_ausente.py:86`) |
| `test_RED_intervalo_ausencia_lavrada_com_trabalha_true_nao_conta`, linha 142 | Retirar a cláusula 2 do juiz. (`app/chamados/juizes.py:408`, `app/ponto/management/commands/detectar_intervalo_ausente.py:70`) |
| `test_intervalo_sem_ancora_com_trabalha_true_conta`, linha 145 | Reintroduzir `or ata.get('sem_ancora')`. (`app/ponto/management/commands/detectar_intervalo_ausente.py:50`) |

**Não é fixture vazia nos positivos:** fábrica/materialização têm entrada acesa e saída apagada; cluster tem duas órfãs próximas e uma lâmpada apagada; intervalo tem pontas acesas e os dois marcos intermediários apagados. (`app/chamados/tests/test_f1c4_3_emissores.py:39`, `app/chamados/tests/test_f1c4_3_emissores.py:89`, `app/chamados/tests/test_f1c4_3_emissores.py:108`, `app/chamados/tests/test_f1c4_3_emissores.py:129`)

O intervalo usa as chaves corretas `hii/hfi` e exercita o ramo de intervalo do helper. Isso **não exercita emissão de chamado**, aposentada no command. (`app/chamados/tests/test_f1c4_3_emissores.py:130`, `app/escala/models.py:1620`, `app/ponto/management/commands/detectar_intervalo_ausente.py:59`, `app/ponto/management/commands/detectar_intervalo_ausente.py:148`)

Há uma limitação de representatividade: a fixture cria célula `True` com DNA de trabalho e depois troca `trabalha` por `None`; o gerador ordinário só preenche os marcos do DNA quando `trabalha is True`. O teste de intervalo limbo prova o comportamento dessa combinação sintética, não sua produção pelo gerador. (`app/chamados/tests/test_f1c4_3_emissores.py:63`, `app/chamados/tests/test_f1c4_3_emissores.py:65`, `app/ponto/management/commands/gerar_celulas.py:79`)

A classe também não cobre `trabalha=False`, `tipo_dia='falta'`, tipo ausente, paridade lote/evento ou nascimento/fechamento efetivo. Os positivos da fábrica aceitam qualquer lista não vazia, sem verificar o motivo exato. (`app/chamados/tests/test_f1c4_3_emissores.py:74`, `app/chamados/tests/test_f1c4_3_emissores.py:75`, `app/chamados/tests/test_f1c4_3_emissores.py:145`)

**4. P2 — A explicação nova de `sem_ancora` está incompleta; a alegação de ocorrência com `trabalha=True` não está demonstrada.**

Há **dois produtores**, não somente o protesto intermitente descrito nos comentários e na docstring:

- Intermitente: `_sem_anc_i` depende da existência de batidas e da ausência de âncora; depois alimenta `sem_ancora_por_data`. (`app/escala/utils.py:1456`, `app/escala/utils.py:1514`)
- Não intermitente: o ramo também grava `sem_ancora_por_data[d] = eh_dia_trabalho(...) is None`. Portanto, o campo também sinaliza o limbo convencional. (`app/escala/utils.py:1546`, `app/escala/utils.py:1701`)

O cartório realmente **copia** o flag para a ata; não calcula ali sua origem nem grava `trabalha`. (`app/ponto/services/cartorio.py:566`, `app/ponto/services/cartorio.py:569`)

O sweep intermitente não exige `cel.trabalha=True`; além disso, o template intermitente retorna `False`, e a geração usa o calculador do vínculo. Logo, “gravado também com `trabalha=True`” não foi demonstrado como trajetória normal pelos produtores citados; ocorrência na frota é **[não verificado]**. (`app/escala/utils.py:1471`, `app/escala/models.py:820`, `app/ponto/portas/celula.py:590`, `app/ponto/portas/celula.py:595`)

Os testes fabricam explicitamente essa combinação; isso fixa uma decisão do leitor, mas não comprova sua incidência em produção. A expressão “quase sempre” da docstring também é **[não verificado]**. (`app/chamados/tests/test_f1c4_3_emissores.py:9`, `app/chamados/tests/test_f1c4_3_emissores.py:60`, `app/chamados/tests/test_f1c4_3_emissores.py:123`)

**5. Informativo — Predicados antes/depois e matriz completa dos gates.**

Nos predicados abaixo, `s` representa `ata.sem_ancora`; o “antes” vem do diff fornecido. O novo juiz veta exatamente `trabalha is False` **ou** `tipo_dia` preenchido e diferente de `'trabalho'`; ele ignora `sem_ancora`. (`app/chamados/juizes.py:407`)

| Sítio | Veto anterior | Veto atual |
|---|---|---|
| `motivos_a_perguntar` | `trabalha is not True` ou colaborador não ativo | `dia_sem_trabalho(cel)` ou colaborador não ativo. (`app/chamados/services/fabrica_da_celula.py:80`) |
| `_plantio_preenche_furo_na_data` | `trabalha is not True` ou lâmpadas vazias | `dia_sem_trabalho(cel)` ou lâmpadas vazias. (`app/chamados/services/materializacao.py:160`) |
| `dias_com_cluster` | `trabalha is not True or s` | `dia_sem_trabalho(cel)`. (`app/ponto/services/cluster_espurio.py:27`) |
| `furos_intervalo` | `trabalha is not True or s` | `dia_sem_trabalho(cel)`. (`app/ponto/management/commands/detectar_intervalo_ausente.py:50`) |

A tabela seguinte cobre as **30 combinações** pedidas, agrupando valores equivalentes. `P` significa passa pelo gate de trabalho; `V`, vetado. “Ausente” significa chave ausente/`None`; se significar a **string literal** `'ausente'`, ela entra no grupo não trabalho. (`app/chamados/juizes.py:408`; gates citados acima)

| `trabalha` | `tipo_dia` | `sem_ancora` | Fábrica/materialização: antes → agora | Cluster/intervalo: antes → agora |
|---|---|---|---|---|
| `False` | qualquer dos cinco | ambos | V → V | V → V |
| `None` | trabalho ou ausente | ambos | **V → P** | **V → P** |
| `None` | folga/ausencia/falta | ambos | V → V | V → V |
| `True` | trabalho ou ausente | `False` | P → P | P → P |
| `True` | trabalho ou ausente | `True` | P → P | **V → P** |
| `True` | folga/ausencia/falta | `False` | **P → V** | **P → V** |
| `True` | folga/ausencia/falta | `True` | **P → V** | V → V |

**Passar pelo gate não equivale a cobrar.** As três variáveis não determinam o retorno completo: faltam lâmpadas, DNA, vencimento, veredito, órfãs e perguntas existentes. (`app/chamados/services/fabrica_da_celula.py:87`, `app/chamados/services/fabrica_da_celula.py:103`, `app/chamados/services/materializacao.py:167`, `app/ponto/services/cluster_espurio.py:31`, `app/ponto/management/commands/detectar_intervalo_ausente.py:61`)

Particularmente na fábrica, `True + tipo não trabalho` já terminava sem motivo pelo veto posterior; a mudança é no gate, não necessariamente no retorno. `None + tipo ausente` também atravessa o juiz novo, mas o leitor o rotula `indefinido` e `_marcos_faltantes` o pula. (`app/chamados/services/declaracao_texto.py:95`, `app/escala/services/leitor_celula.py:344`)

**6. Informativo — Escritores dos campos e demais leitores da pergunta.**

| Campo | Quem produz e quem persiste |
|---|---|
| `CelulaDia.trabalha` | A porta calcula pelo vínculo na geração e regeneração; força `False` fora do vínculo; na reversão deriva o valor da existência de marcos no DNA anterior. (`app/ponto/portas/celula.py:231`, `app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:260`, `app/ponto/portas/celula.py:532`, `app/ponto/portas/celula.py:590`, `app/ponto/portas/celula.py:612`) |
| `ata.tipo_dia` | O builder produz trabalho/folga; overlays produzem folga/falta/ausência. O cartório copia o tipo e entrega a ata à lavra da porta. (`app/escala/utils.py:1743`, `app/escala/utils.py:1749`, `app/escala/utils.py:1118`, `app/escala/utils.py:1174`, `app/escala/utils.py:1180`, `app/ponto/services/cartorio.py:566`, `app/ponto/services/cartorio.py:1520`) |
| `ata.sem_ancora` | Os dois caminhos descritos no achado 4 produzem o flag; o builder o entrega no dia; o cartório copia para a ata. A porta atribui a ata e persiste. (`app/escala/utils.py:1514`, `app/escala/utils.py:1701`, `app/escala/utils.py:1752`, `app/ponto/services/cartorio.py:569`, `app/ponto/portas/celula.py:725`, `app/ponto/portas/celula.py:732`) |

Os leitores próprios relevantes à cobrança, fora dos quatro sítios, incluem:

| Leitor | Critério próprio |
|---|---|
| Lote e vigia de hora | `trabalha=True`; fio mudo sem filtro equivalente. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `app/chamados/management/commands/disparar_perguntas_competencia.py:155`, `app/ponto/services/vigia_de_hora.py:21`) |
| Cartório | Protesto com `is not False`; determinado nascimento de furo exige `eh_dia_trabalho(...) is True`; espera do turno usa `is not False`. (`app/ponto/services/cartorio.py:1167`, `app/ponto/services/cartorio.py:1459`, `app/ponto/services/cartorio.py:1664`) |
| Reconciliar grade | `sem_ancora` pula criação; a retratação dos indefinidos já consulta o juiz. (`app/ponto/management/commands/reconciliar_grade.py:139`, `app/ponto/management/commands/reconciliar_grade.py:280`) |
| Detector de anomalias | Distingue `None` de `False`, ignorando o primeiro. (`app/chamados/detector_anomalias.py:164`) |
| Gate de marcos e classificador de falta | `False` veta. (`app/ponto/marcos.py:159`, `app/escala/servico_jornada.py:303`) |
| Declaração/fábrica pela ata | Exige `tipo_dia='trabalho'`. (`app/chamados/services/declaracao_texto.py:95`) |
| Outros caminhos da disputa | Tipo trabalho na derivação da data; `eh_dia_trabalho(...) is not False` na base de emissão. (`app/chamados/services/disputa_emissao.py:68`, `app/chamados/services/disputa_emissao.py:753`) |
| Suprajuiz | Trabalho **ou ausência parcial** é dia de cobrança; `sem_ancora` impede ramos de furo parcial. (`app/ponto/supra_juiz.py:281`, `app/ponto/supra_juiz.py:379`, `app/ponto/supra_juiz.py:396`) |

A ausência parcial merece atenção: o suprajuiz admite cobrança, enquanto `dia_sem_trabalho` rejeita qualquer ata `'ausencia'`. É divergência semântica preexistente; emissão e fechamento efetivamente contraditórios na frota por esse caso são **[não verificado]**. (`app/ponto/supra_juiz.py:281`, `app/chamados/juizes.py:408`)

Também há filtros próprios em fila por causa, vigia de entrada, semana do posto e presença em feriado (`trabalha=True`); em métricas, worklist, órfãs, propostas e simulação (`tipo_dia='trabalho'`); e em dinheiro/fechamento, que admitem trabalho/ausência. Esses filtros não bastam, isoladamente, para provar contradição da mesma cobrança. (`app/chamados/services/fila_por_causa.py:192`, `app/ponto/services/vigia_entrada.py:79`, `app/escala/services/semana_do_posto.py:86`, `app/colaboradores/services/feriado_do_posto.py:94`, `app/escala/services/furos_diarios.py:39`, `app/ponto/worklist.py:19`, `app/escala/orfa_classe.py:65`, `app/ponto/propositor.py:36`, `app/ponto/services/simular_dia.py:12`, `app/ponto/services/dia_pago.py:386`, `app/ponto/services/fechamento.py:1015`)

Outros consumidores encontrados são relatórios/inteligência, flip, materialização pelo veredito, exportação e bordas do realizado. (`app/relatorios/furos.py:151`, `app/inteligencia/calculadores.py:46`, `app/ponto/services/flip_auto.py:208`, `app/ponto/services/veredito_celula.py:221`, `app/folha/export.py:1071`, `app/ponto/services/bordas_realizado.py:74`)

**7. Checklist — Classes de reprovação.**

| Classe | Resultado nesta obra |
|---|---|
| **Segundo juiz que duplica registrado** | O diff consome uma função existente; não cria juiz. O registro aponta `eh_dia_trabalho` para a pergunta **do vínculo**, que devolve o campo da célula; `dia_sem_trabalho` acrescenta o fato da ata. Chamar ambos indistintamente de “único juiz do dia de trabalho” apaga essa distinção. (`app/core/juizes.py:158`, `app/docs/JUIZES.md:66`, `app/escala/models.py:1072`, `app/chamados/juizes.py:408`) |
| **Segundo escritor fora da porta** | Não identificado nas alterações de produção: os quatro passam a ler o juiz; a persistência dos campos examinados permanece na porta. A fixture escreve diretamente por `update`, mas é código de teste. (`app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/services/materializacao.py:160`, `app/ponto/services/cluster_espurio.py:27`, `app/ponto/management/commands/detectar_intervalo_ausente.py:50`, `app/ponto/portas/celula.py:725`, `app/chamados/tests/test_f1c4_3_emissores.py:65`) |
| **Relógio fora da borda** | O diff não acrescenta leitura do relógio. Há fallback preexistente dentro de `motivos_a_perguntar`, por `timezone.localtime()` sem instante recebido. (`app/chamados/services/fabrica_da_celula.py:84`) |
| **Derivação paralela de fonte única** | Sobrevive: lote, vigia, declaração e outros caminhos mantêm os predicados próprios relacionados no achado 6. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `app/ponto/services/vigia_de_hora.py:21`, `app/chamados/services/declaracao_texto.py:95`) |
| *

[truncado em 20000 de 21180 caracteres]

## 31/10 11:46 · sessao c6d9adf98c95 · - · - · origem=Ronald (tmux)

**Achado P2: dois testes da fábrica não protegem a cláusula 2 que a docstring promete proteger.** A docstring afirma que retirar a cláusula `ata.tipo_dia` faz os quatro cobrarem (`app/chamados/tests/test_f1c4_3_emissores.py:22-23`). Porém a fábrica consulta `motivos_pela_ata` (`app/chamados/services/fabrica_da_celula.py:111`), que consulta `_ata_do_dia` (`app/chamados/services/fabrica_da_celula.py:48-49`); este consulta `_marcos_faltantes` (`app/chamados/services/disputa_emissao.py:826-827`), que tem outro veto próprio `tipo_dia != 'trabalho'` (`app/chamados/services/declaracao_texto.py:90-96`). Assim, retirar a cláusula 2 do juiz ou ignorar o juiz inteiro ainda deixa os testes fábrica-folga e fábrica-ausência verdes (`app/chamados/tests/test_f1c4_3_emissores.py:77-84`; cadeia acima).

Validação adicional: executei extrações AST das funções reais em memória, com ORM, grade e tempo substituídos por dublês, usando `python3 -B`, sem escrita. Nesse experimento, os 18 casos deram os resultados esperados; removida a cláusula 2 do juiz, folga/ausência passaram a cobrar nos outros três sítios, mas permaneceram negativas na fábrica. Isso não foi execução da suíte Django; execução real dos testes: **[não verificado]**. [nao verificado]

Mapa por teste; as mutações abaixo são previsões pela leitura, corroboradas pelo experimento isolado quando indicado: [nao verificado]

| Teste — linha em `app/chamados/tests/test_f1c4_3_emissores.py` | Mutação de produção que o deixa vermelho |
|---|---|
| `RED_fabrica_limbo_pergunta`:74 | Restaurar `cel.trabalha is not True` no gate `app/chamados/services/fabrica_da_celula.py:80`: devolve `[]`. |
| `fabrica_folga_nao_pergunta`:77 | **Mutação somente no juiz não basta.** É necessário permitir folga também no veto de `app/chamados/services/declaracao_texto.py:95-96`; combinada com retirar o veto do juiz, surge motivo pela saída apagada (`app/chamados/services/fabrica_da_celula.py:59-64`). |
| `MORDE_fabrica_dia_de_trabalho_pergunta`:80 | Inverter o gate para bloquear trabalho no `app/chamados/services/fabrica_da_celula.py:80`, ou suprimir a montagem dos motivos em `:111`. |
| `RED_fabrica_ausencia_lavrada_com_trabalha_true_nao_pergunta`:83 | **Mutação somente na cláusula 2 não basta.** Retirar também o veto `tipo_dia != 'trabalho'` de `app/chamados/services/declaracao_texto.py:95-96` faz retornar motivo pela saída apagada (`app/chamados/services/fabrica_da_celula.py:59-64`). |
| `RED_materializacao_limbo_furo_absolve_o_plantio`:93 | Restaurar `cel.trabalha is not True` em `app/chamados/services/materializacao.py:160`: retorna `False` em `:169`. |
| `materializacao_folga_nao_absolve`:96 | Retirar a cláusula 2 de `app/chamados/juizes.py:408`: a célula chega ao propositor e retorna `True` (`app/chamados/services/materializacao.py:164-168`). |
| `MORDE_materializacao_dia_de_trabalho_absolve`:99 | Suprimir o `return True` em `app/chamados/services/materializacao.py:168`, deixando chegar a `:169`. |
| `RED_materializacao_ausencia_lavrada_com_trabalha_true_nao_absolve`:102 | Retirar a cláusula 2 de `app/chamados/juizes.py:408`: passa a absolver (`app/chamados/services/materializacao.py:164-168`). |
| `RED_cluster_limbo_conta`:111 | Restaurar `cel.trabalha is not True` no gate `app/ponto/services/cluster_espurio.py:27`, ou restaurar veto `sem_ancora` nesse gate. |
| `cluster_folga_nao_conta`:114 | Retirar cláusula 2 de `app/chamados/juizes.py:408`: encontra duas órfãs próximas e lâmpada apagada (`app/ponto/services/cluster_espurio.py:29-34`). |
| `MORDE_cluster_dia_de_trabalho_conta`:117 | Impedir `out.append` em `app/ponto/services/cluster_espurio.py:34`, ou exigir três órfãs em `:31`. |
| `RED_cluster_ausencia_lavrada_com_trabalha_true_nao_conta`:120 | Retirar cláusula 2 de `app/chamados/juizes.py:408`: conta o cluster (`app/ponto/services/cluster_espurio.py:29-34`). |
| `cluster_sem_ancora_com_trabalha_true_conta`:123 | Reintroduzir `or ata.get('sem_ancora')` no gate `app/ponto/services/cluster_espurio.py:27`. |
| `RED_intervalo_limbo_conta`:133 | Restaurar `cel.trabalha is not True` ou veto `sem_ancora` em `app/ponto/management/commands/detectar_intervalo_ausente.py:50`. |
| `intervalo_folga_nao_conta`:136 | Retirar cláusula 2 de `app/chamados/juizes.py:408`: encontra os dois marcos do intervalo (`app/ponto/management/commands/detectar_intervalo_ausente.py:70-86`). |
| `MORDE_intervalo_dia_de_trabalho_conta`:139 | Suprimir `out.append` em `app/ponto/management/commands/detectar_intervalo_ausente.py:86`, ou remover o par `('hii','hfi')` da fonte `app/escala/models.py:1620`. |
| `RED_intervalo_ausencia_lavrada_com_trabalha_true_nao_conta`:142 | Retirar cláusula 2 de `app/chamados/juizes.py:408`: encontra os dois marcos (`app/ponto/management/commands/detectar_intervalo_ausente.py:70-86`). |
| `intervalo_sem_ancora_com_trabalha_true_conta`:145 | Reintroduzir `or ata.get('sem_ancora')` em `app/ponto/management/commands/detectar_intervalo_ausente.py:50`. |

**Não identifiquei fixture vazia nos positivos pela leitura.** A fábrica recebe entrada acesa e saída apagada (`app/chamados/tests/test_f1c4_3_emissores.py:39-40,69-72`); o leitor transforma `acesa=False` em `missing=True` (`app/escala/services/leitor_celula.py:92-121`). Materialização recebe esses mesmos slots e plantio às 17h (`app/chamados/tests/test_f1c4_3_emissores.py:87-91`); o propositor exige que ele ocupe um furo sem deslocar a marca existente (`app/ponto/propositor.py:247-258`). Cluster recebe duas órfãs 18:56/18:58 e uma lâmpada apagada (`app/chamados/tests/test_f1c4_3_emissores.py:108`), satisfazendo o detector (`app/ponto/services/cluster_espurio.py:29-34`).

**A fixture de intervalo usa as chaves certas e percorre o ramo nomeado.** Ela define `hii=12:00` e `hfi=13:00`, com bordas acesas e miolo apagado (`app/chamados/tests/test_f1c4_3_emissores.py:127-131`); a fonte declara precisamente `('hii','hfi')` (`app/escala/models.py:1620`), o detector lê esse par (`app/ponto/management/commands/detectar_intervalo_ausente.py:57-60`) e o encadeador mantém os dois marcos (`app/escala/utils.py:889-903`).

**Limite de cobertura P2:** os testes verificam listas/booleanos de funções auxiliares, sem verificar nascimento ou fechamento efetivo de chamados (`app/chamados/tests/test_f1c4_3_emissores.py:69-146`). Em particular, intervalo não emite chamados: o command apenas reconcilia os legados e depois segue por `continue` (`app/ponto/management/commands/detectar_intervalo_ausente.py:121-155`). Materialização testa a absolvição da guarda, não uma emissão (`app/chamados/services/materializacao.py:128-136,167-169`). Os testes da fábrica aceitam qualquer lista não vazia, sem conferir qual motivo veio (`app/chamados/tests/test_f1c4_3_emissores.py:75,81`).

**Lacunas específicas:** não há teste `trabalha=False`, `tipo_dia='falta'`, chave `tipo_dia` ausente ou comparação lote × evento nesta classe (`app/chamados/tests/test_f1c4_3_emissores.py:74-146`).

## 31/10 11:46 · sessao 1c37aedab946 · - · - · origem=Ronald (tmux)

1. **P2 — omissão nova entre evento, lote e vigia.** O evento agora aceita `trabalha=None` quando a ata permite (`app/chamados/services/fabrica_da_celula.py:80`), mas o primeiro universo do lote continua selecionando exclusivamente `trabalha=True` (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`), assim como o vigia de hora (`app/ponto/services/vigia_de_hora.py:21`). O evento chama a fábrica depois do julgamento (`app/ponto/services/cartorio.py:798`); o lote chama diretamente `motivos_pela_ata`, contornando o predicado alterado (`app/chamados/management/commands/disparar_perguntas_competencia.py:111`). Isso prova uma omissão de recuperação/alarme para células None sem chamado; não prova emissão seguida de fechamento contrário. (`app/chamados/management/commands/disparar_perguntas_competencia.py:155`)

2. **P2, preexistente — fio mudo pode emitir sobre dia que o fechamento considera regularizado.** Condição demonstrável: `cel.trabalha=False`, `ata.tipo_dia='trabalho'`, marco apagado e chamado vivo ainda sem pergunta. O segundo universo do lote não filtra `trabalha` e consulta diretamente `motivos_pela_ata` (`app/chamados/management/commands/disparar_perguntas_competencia.py:155`, `app/chamados/management/commands/disparar_perguntas_competencia.py:167`). O leitor dá prioridade ao tipo da ata sobre `trabalha=False` (`app/escala/services/leitor_celula.py:344`); `_marcos_faltantes` aceita esse tipo e devolve os marcos apagados (`app/chamados/services/declaracao_texto.py:95`, `app/chamados/services/declaracao_texto.py:106`). Sem pergunta anterior, o lote inclui o fato e chama o escritor (`app/chamados/management/commands/disparar_perguntas_competencia.py:220`, `app/chamados/management/commands/disparar_perguntas_competencia.py:225`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`). A criação da disputa e das perguntas não reaplica `dia_sem_trabalho` (`app/chamados/services/disputa_emissao.py:598`, `app/chamados/services/disputa_emissao.py:389`). Entretanto, o evento recusa o mesmo dia (`app/chamados/services/fabrica_da_celula.py:80`) e o fechamento do chamado de furo considera sua premissa regularizada (`app/chamados/models.py:1232`, `app/chamados/models.py:699`). É contradição condicional do código; ocorrência em produção **[não verificado]**. Como a fábrica antiga também recusava False, não é regressão criada pelo diff fornecido **[não verificado quanto ao arquivo anterior; predicado consta do anexo]**.

3. **P2 — um dos quatro “emissores” não emite chamados.** `furos_intervalo` alimenta a reconciliação do legado (`app/ponto/management/commands/detectar_intervalo_ausente.py:118`, `app/ponto/management/commands/detectar_intervalo_ausente.py:121`); o nascimento foi aposentado e termina em `continue` incondicional (`app/ponto/management/commands/detectar_intervalo_ausente.py:148`, `app/ponto/management/commands/detectar_intervalo_ausente.py:155`). O resultado do helper não mede novos CHAMADOS desse emissor. (`app/ponto/management/commands/detectar_intervalo_ausente.py:154`)

4. **Registro: não imputar criação de segundo juiz ao diff.** A autoridade registrada para “o dia é de trabalho para o vínculo?” continua sendo `EscalaColaborador.eh_dia_trabalho` (`app/core/juizes.py:158`, `app/docs/JUIZES.md:66`), que devolve `cel.trabalha` diretamente (`app/escala/models.py:1072`). `dia_sem_trabalho` pergunta também pelo fato lavrado na ata (`app/chamados/juizes.py:398`, `app/chamados/juizes.py:408`). São perguntas distinguíveis: previsão do vínculo versus cobrança depois dos fatos. A função já existia e o diff apenas a consome **[anexo emis2_diff.txt]**. A alegação absoluta de “único juiz do dia de trabalho” precisa dessa delimitação. (`app/chamados/services/fabrica_da_celula.py:79`)

5. **Censo dos leitores próprios relevantes à cobrança, fora dos quatro:**

| Leitor | Predicado próprio |
|---|---|
| Lote da competência | `trabalha=True`; fio mudo sem filtro correspondente. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `app/chamados/management/commands/disparar_perguntas_competencia.py:155`) |
| Vigia de hora | `trabalha=True`. (`app/ponto/services/vigia_de_hora.py:21`) |
| Cartório | Protesta se `trabalha is not False`; emite determinado furo somente se `eh_dia_trabalho(...) is True`; espera turno quando `trabalha is not False`. (`app/ponto/services/cartorio.py:1167`, `app/ponto/services/cartorio.py:1459`, `app/ponto/services/cartorio.py:1664`) |
| Reconciliar grade | `sem_ancora` pula criação; a retratação dos indefinidos já consulta `dia_sem_trabalho`. (`app/ponto/management/commands/reconciliar_grade.py:139`, `app/ponto/management/commands/reconciliar_grade.py:280`) |
| Detector de anomalias | None ignora; False vira batida em folga. (`app/chamados/detector_anomalias.py:164`) |
| Gate comum de cobrança | False veta. (`app/ponto/marcos.py:159`) |
| Classificação de falta | False cala; cobertura integral também cala. (`app/escala/servico_jornada.py:303`, `app/escala/servico_jornada.py:344`) |
| Declaração e fábrica pela ata | `tipo_dia != 'trabalho'` pula. (`app/chamados/services/declaracao_texto.py:95`) |
| Derivador de data de furo | Exige tipo trabalho. (`app/chamados/services/disputa_emissao.py:68`) |
| Base de emissão de disputa | `eh_dia_trabalho(...) is not False`. (`app/chamados/services/disputa_emissao.py:753`) |
| Suprajuiz | Trabalho **ou ausência parcial** constitui dia de cobrança; `sem_ancora` impede os ramos de furo parcial. (`app/ponto/supra_juiz.py:281`, `app/ponto/supra_juiz.py:379`, `app/ponto/supra_juiz.py:396`) |

A ausência parcial é distinção particularmente relevante: o suprajuiz a admite como dia de cobrança, enquanto `dia_sem_trabalho` rejeita qualquer `tipo_dia='ausencia'`; essa divergência do juiz preexistia ao diff. (`app/ponto/supra_juiz.py:281`, `app/chamados/juizes.py:408`)

6. **Outros consumidores com filtros próprios, sem prova de emissão/fechamento contraditório da mesma cobrança:** fila por causa (`app/chamados/services/fila_por_causa.py:192`); métricas de furo (`app/escala/services/furos_diarios.py:39`); worklist (`app/ponto/worklist.py:19`); relatórios (`app/relatorios/furos.py:151`, `app/relatorios/furos.py:218`, `app/relatorios/furos.py:237`); inteligência (`app/inteligencia/calculadores.py:46`, `app/inteligencia/calculadores.py:141`); flip (`app/ponto/services/flip_auto.py:208`, `app/ponto/services/flip_auto.py:212`); classificação de órfãs (`app/escala/orfa_classe.py:65`); propositor (`app/ponto/propositor.py:36`, `app/ponto/propositor.py:210`); materialização pelo veredito (`app/ponto/services/veredito_celula.py:221`); vigia de entrada (`app/ponto/services/vigia_entrada.py:79`, `app/ponto/services/vigia_entrada.py:120`); simulação (`app/ponto/services/simular_dia.py:12`); dinheiro e fechamento (`app/ponto/services/dia_pago.py:386`, `app/ponto/services/fechamento.py:1015`, `app/folha/export.py:1071`); bordas do realizado (`app/ponto/services/bordas_realizado.py:74`); semana do posto (`app/escala/services/semana_do_posto.py:86`); presença em feriado (`app/colaboradores/services/feriado_do_posto.py:94`).

## 31/10 11:45 · sessao 8d6599a85837 · - · - · origem=Ronald (tmux)

1. **[Média] A explicação nova de `sem_ancora` reduz indevidamente seu significado ao intermitente.** Há dois produtores: o sweep intermitente grava `_sem_anc_i`, calculado pela ausência de âncora; o ramo **não intermitente** também grava `eh_dia_trabalho(...) is None`. Logo, `sem_ancora` também sinaliza limbo de escala convencional. (`app/escala/utils.py:1456`, `app/escala/utils.py:1514`, `app/escala/utils.py:1546`, `app/escala/utils.py:1701`)

2. **A cópia pelo cartório está corretamente descrita:** `ata_do_dia` copia `tipo_dia` e converte `sem_ancora` para bool; não escreve `CelulaDia.trabalha`. A ata chega à operação de lavra pela chamada do cartório. (`app/ponto/services/cartorio.py:566`, `app/ponto/services/cartorio.py:569`, `app/ponto/services/cartorio.py:1518`, `app/ponto/services/cartorio.py:1520`)

3. **“Gravado também com trabalha=True” não está demonstrado como trajetória normal pelos produtores citados.** O sweep calcula `sem_ancora` sem consultar/escrever `trabalha`, portanto não impede essa combinação; porém o template intermitente retorna `False`, e o gerador usa o valor calculado do vínculo. A ocorrência concreta de `True + sem_ancora=True` na frota é **[não verificado]**. (`app/escala/utils.py:1471`, `app/escala/utils.py:1514`, `app/escala/models.py:820`, `app/ponto/portas/celula.py:590`, `app/ponto/portas/celula.py:595`)

4. **Escritores de `CelulaDia.trabalha`:**
   - Regeneração: força `False` fora do vínculo; cria e atualiza usando `ec.eh_dia_trabalho_calculado(d)`. (`app/ponto/portas/celula.py:231`, `app/ponto/portas/celula.py:233`, `app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:260`, `app/ponto/portas/celula.py:264`)
   - Geração: cria e atualiza usando o mesmo calculador. (`app/ponto/portas/celula.py:590`, `app/ponto/portas/celula.py:595`, `app/ponto/portas/celula.py:612`, `app/ponto/portas/celula.py:615`)
   - Reversão: deriva o booleano de `dna_anterior.marcos is not None`. (`app/ponto/portas/celula.py:527`, `app/ponto/portas/celula.py:532`, `app/ponto/portas/celula.py:538`)
   - O command `gerar_celulas` reexporta as funções da porta; não constitui outro escritor. (`app/ponto/management/commands/gerar_celulas.py:13`)

5. **Produtores de `tipo_dia` e persistência da ata:**
   - Builders produzem `folga`/`trabalho`; overlays mudam para `folga`, `falta` e `ausencia`. (`app/escala/utils.py:727`, `app/escala/utils.py:752`, `app/escala/utils.py:1118`, `app/escala/utils.py:1174`, `app/escala/utils.py:1180`, `app/escala/utils.py:1743`, `app/escala/utils.py:1749`)
   - A porta recebe a ata, preserva a chave de outra operação e atribui/persiste o restante. (`app/ponto/portas/celula.py:697`, `app/ponto/portas/celula.py:703`, `app/ponto/portas/celula.py:711`, `app/ponto/portas/celula.py:725`, `app/ponto/portas/celula.py:732`)
   - `CelulaDia.lavrar_veredito` apenas delega; `carimbar_precedencia` conserva as demais chaves e altera somente `precedencia`. (`app/escala/models.py:1781`, `app/ponto/portas/celula.py:774`, `app/ponto/portas/celula.py:775`)
   - A atribuição de ata em `escala/utils.py` é cópia em memória, sem persistência; não é segundo escritor persistente. (`app/escala/utils.py:1826`, `app/escala/utils.py:1827`)

6. **[Média, limite dos testes] O limbo sintético com DNA de trabalho exige cuidado.** O gerador só preenche `dna.marcos` quando `trabalha is True`; criar célula True com marcos e depois trocar o campo para None não demonstra que o limbo ordinariamente gerado alcançará o mesmo ramo de intervalo. Isso limita a representatividade da fixture, sem tornar seu teste do leitor automaticamente inválido. (`app/ponto/management/commands/gerar_celulas.py:77`, `app/ponto/management/commands/gerar_celulas.py:79`, `app/ponto/management/commands/gerar_celulas.py:83`, `app/ponto/portas/celula.py:590`, `app/ponto/portas/celula.py:596`)

7. **Não identifiquei novo escritor persistente fora da porta nas alterações descritas:** os emissores somente passaram a consumir o juiz, e os escritores examinados permanecem na porta declarada. O gate existente declara allowlist vazia e censura escrita externa; sua execução nesta auditoria é **[não verificado]**. (`app/ponto/portas/celula.py:44`, `app/ponto/tests/test_chokepoint_celula_gate.py:21`, `app/ponto/tests/test_chokepoint_celula_gate.py:49`)

## 31/10 11:39 · sessao c60552d62fc3 · CODEX-LICOES · verificador · origem=Code

A v2 deixa lacunas de operação, concorrência e medição. Primeiro, a distinção de versão: **o wrapper da cópia é anterior ao anexo**; aceita quatro papéis e monta um cabeçalho menor. O contexto `4435ec64` aparece como pronto, com pouso retido. As referências abaixo são da cópia. (bin/codex_pergunta.sh:48; bin/codex_pergunta.sh:118; app/docs/BACKLOG.md:871)

1. **O gatilho pode chegar antes da correção provada.** A classificação acontece quando o opus reprova, e a reprovação devolve a obra ao ciclo de correção. Publicar a tabela não garante que já exista o commit/RED exigido para ensinar a lição. Falta declarar como o achado fica pendente e como a chamada retoma depois da prova. (CLAUDE.md:598; CLAUDE.md:614) [nao verificado]

2. **Uma `.q` e uma `.r` podem perder a prova da classificação.** A classificação vigente usa **todas** as perguntas, respostas e linhas de triagem, devolvendo os trechos por achado; voltas intermediárias podem conter apenas delta. O contrato precisa vincular a lição à linha dessa classificação e às evidências relevantes, identificando as voltas. (CLAUDE.md:614; CLAUDE.md:618)

3. **Rodar em fundo não assegura sair do caminho crítico.** Todos os papéis disputam o mesmo gate e os mesmos slots; uma lição pode atrasar o próximo verificador. A espera por slot ocorre antes do `timeout`. Se a consolidação chamar outro wrapper mantendo o slot da geração, também pode haver esgotamento circular de slots — comportamento futuro [nao verificado]. Defina quando o slot é liberado e o limite da espera. (bin/codex_pergunta.sh:137; bin/codex_pergunta.sh:150; bin/codex_pergunta.sh:153; bin/codex_pergunta.sh:157)

4. **Existe recusa antes de qualquer rc 75.** O disjuntor pode impedir a chamada no `PreToolUse`: acima do orçamento diário só passa a volta final; acima de 85% semanal exige escada. Isso vale para qualquer papel. O contrato não define o destino da lição/consolidação impedida nesse ponto. (bin/disjuntor.py:897; bin/disjuntor.py:906; bin/disjuntor.py:938; bin/tests/test_disjuntor.sh:489)

5. **A regra de falha está incompleta.** Além de 75, existem uso inválido, falha de cópia, raspagem recusada, timeout e erro genérico. O contrato permanente exige tratamento de erro, timeout e limite. A transcrição deve depender de sucesso integral da chamada, resposta nova e validação; não basta excluir 75 e formato inválido. (bin/codex_pergunta.sh:39; bin/codex_pergunta.sh:99; bin/codex_pergunta.sh:105; bin/codex_pergunta.sh:157; bin/tests/test_codex_limite.sh:36; CLAUDE.md:651)

6. **O limite de 20 tem uma transição contraditória.** Pelo item 3, primeiro pode haver mais de 20 ativas; se a consolidação falhar e “mantém”, permanecem mais de 20, contrariando o selo do item 4. A candidata precisa ficar fora da versão ativa até uma consolidação válida, ou ser recusada preservando a versão anterior com até 20. Trata-se de lacuna do protocolo proposto; implementação [nao verificado].

7. **Append atômico não resolve sozinho a consolidação concorrente.** Uma consolidação pode ler a versão A, outra chamada acrescentar uma lição e, depois, a primeira substituir tudo pelo resultado calculado sobre A. Falta exigir comparação da versão-base antes da substituição, além de definir alocação de IDs únicos e deduplicação do mesmo achado. O protocolo futuro não está demonstrado [nao verificado]; a regra vigente pede escritor canônico com tripwire. (CLAUDE.md:32)

8. **SHA com par no log não prova toda escrita externa.** Regravar externamente uma versão antiga já registrada preserva a condição “SHA possui par”. Além disso, arquivo e log são duas escritas: falta definir recuperação de interrupção entre elas e recusa quando o registro falha. O wrapper atual grava `ini` e continua sem testar o sucesso desse append; esse comportamento não basta para a nova garantia. (CLAUDE.md:32; bin/codex_pergunta.sh:88)

9. **O SHA pode identificar conteúdo diferente do recebido.** Hoje os anexos são raspados, concatenados e cortados em **300.000 bytes**, sem recusa por truncamento. Lições acrescentadas ao fim podem desaparecer; acrescentadas antes podem deslocar a correção provada. Vinte itens também não limitam bytes ou tokens. É necessário congelar uma versão por chamada, definir seu teto e ligar o SHA ao bloco efetivamente enviado. (bin/codex_pergunta.sh:111; bin/codex_pergunta.sh:114; bin/codex_pergunta.sh:115; bin/codex_pergunta.sh:133)

10. **O A/B precisa de uma exceção explícita a “toda chamada leva lições”.** O wrapper atual não oferece seleção do braço; fixa esforço `high`, mas não fixa modelo na invocação nem registra braço/modelo/anexos no `ini`. Também falta congelar as lições durante os pares e separar tarefas de avaliação das obras usadas para ensiná-las. Sem isso, repetição de um achado pode parecer generalização; a medição anterior já registrou sinal de eco. (bin/codex_pergunta.sh:32; bin/codex_pergunta.sh:88; bin/codex_pergunta.sh:157; app/docs/RELATO.md:176)

11. **Há leitores e testes além do wrapper para atualizar.** Os dois novos papéis hoje dão rc 2; liberá-los somente no wrapper faz contagens e tokens virarem `outro`. O SHA deve preservar as posições existentes do log e ganhar leitor próprio. Os selos atuais verificam sandbox, raspagem, procedência e limite; esses verdes não demonstram concorrência, consolidação, transcrição ou desligamento das lições. (bin/codex_pergunta.sh:48; bin/codex_limite_estado.py:23; bin/codex_limite_estado.py:63; bin/codex_limite_estado.py:78; bin/codex_limite_estado.py:194; bin/tests/test_codex_pergunta.sh:2; bin/tests/test_codex_sonda.sh:90; bin/tests/test_codex_limite.sh:31)

A medição proposta também precisa declarar seu alcance: gabarito do opus não cobre a classe **“nenhum viu”**, descoberta depois por suite, produção ou outra pessoa. Essa classe exige cura estrutural e selo; lição não substitui esse fechamento. (CLAUDE.md:623) [nao verificado]

## 31/10 11:38 · sessao 1e1616f4e617 · - · - · origem=Ronald (tmux)

Pontos concretos:

- **A cópia não contém o wrapper do anexo.** O local tem whitelist dos quatro papéis em `bin/codex_pergunta.sh:48`, prompt sem referência a AGENTS/BASE/juiz em `bin/codex_pergunta.sh:118`, e termina em `bin/codex_pergunta.sh:177`. Citar linhas do anexo como se fossem dessa cópia daria proveniência falsa. O estado do wrapper instalado no host é [nao verificado].

- **Os novos papéis precisam chegar também aos leitores.** `licao` e `licao-consolidar` são recusados pela whitelist atual (`bin/codex_pergunta.sh:48`); mesmo liberando o escritor, `PAPEIS` mantém só quatro valores (`bin/codex_limite_estado.py:23`), portanto suas contagens e tokens viram `outro` (`bin/codex_limite_estado.py:78`, `bin/codex_limite_estado.py:194`). O teste exige expressamente esse agrupamento para papel desconhecido (`bin/tests/test_codex_papel_estado.sh:32`).

- **Fundo não assegura “fora do caminho crítico”.** Há um gate bloqueante compartilhado e slots comuns a todos os papéis (`bin/codex_pergunta.sh:137`, `bin/codex_pergunta.sh:140`, `bin/codex_pergunta.sh:145`); uma lição ocupa slot até o processo sair (`bin/codex_pergunta.sh:153`). A chamada seguinte pode esperar por esses mesmos locks, e o timeout só começa no `codex exec`, depois da fila (`bin/codex_pergunta.sh:150`, `bin/codex_pergunta.sh:157`). Para consolidação, chamar outro wrapper enquanto mantém o slot também exige avaliar exaustão de slots — implementação futura [nao verificado].

- **O contrato esquece o disjuntor anterior ao wrapper.** Sem `CODEX_VOLTA`, a chamada é classificada como `meio` (`bin/disjuntor.py:861`). Acima do orçamento só `final` passa, qualquer papel (`bin/disjuntor.py:906`); acima de 85% precisa da escada medida (`bin/disjuntor.py:897`). O teste protege ambos os cortes (`bin/tests/test_disjuntor.sh:489`, `bin/tests/test_disjuntor.sh:493`). Tratar apenas rc 75 não define a lição impedida antes de executar.

- **Adicionar SHA em coluna errada quebra leitores; acrescentá-lo preserva posições, mas não traz auditoria automaticamente.** O parser usa posições fixas para chave, `ini`, `fim`, sessão e rc (`bin/codex_limite_estado.py:63`, `bin/codex_limite_estado.py:67`) e descarta qualquer metadado extra do `ini` (`bin/codex_limite_estado.py:65`). O selo atual verifica essas posições, sem SHA de lições (`bin/tests/test_codex_sonda.sh:94`, `bin/tests/test_codex_sonda.sh:95`). É necessário definir esquema e leitor da versão efetivamente anexada.

- **A/B ainda não é reproduzível pelo instrumento atual.** O esforço fica fixado em `high`, mas a invocação não fixa modelo (`bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:158`); o `ini` registra pergunta raspada, sem braço, modelo ou hashes dos anexos (`bin/codex_pergunta.sh:88`). Tokens são agregados por papel e obtidos das sessões locais, sem braço/versão de lição (`bin/codex_limite_estado.py:185`, `bin/codex_limite_estado.py:194`, `bin/codex_limite_estado.py:208`). Resultados de recall, precisão e generalização são [nao verificado].

- **Os testes existentes não sustentam o selo novo.** O estrutural cobra forma do wrapper (`bin/tests/test_codex_pergunta.sh:2`), o de sonda cobre raspagem/rc/procedência (`bin/tests/test_codex_sonda.sh:57`, `bin/tests/test_codex_sonda.sh:90`) e o de limite cobre rc 75/contador (`bin/tests/test_codex_limite.sh:31`, `bin/tests/test_codex_limite.sh:40`). A prova real verifica que o Codex não escreve na cópia/viva/tmp; não valida escrita autorizada do wrapper em LICOES (`bin/tests/prova_codex_pergunta_real.sh:24`, `bin/tests/prova_codex_pergunta_real.sh:26`). Esses verdes não provam formato, concorrência, consolidação, vínculo SHA/transcrição ou desligamento do novo contrato.

Não executei testes: os testes de sonda e limite criam arquivos temporários (`bin/tests/test_codex_sonda.sh:12`, `bin/tests/test_codex_limite.sh:9`).

## 31/10 11:38 · sessao 8dfc660db753 · - · - · origem=Ronald (tmux)

Pontos documentais que sustentam a revisão:

- **Gatilho antes da correção comprovada.** A tabela nasce “quando o opus reprova”; a reprovação manda voltar ao ciclo construtor/Codex. Portanto publicar a tabela não garante a existência de commit/RED da correção exigida pelo contrato. Falta declarar pendência e retomada depois da prova, ou adiar a publicação. Inferência de `CLAUDE.md:598-600` e `CLAUDE.md:614-617`. [nao verificado]

- **Dossiê de classificação versus anexo singular.** A classificação canônica exige TODAS as perguntas/respostas e as linhas de triagem, devolvendo os trechos por achado. O contrato oferece `.q` + `.r` sem explicitar qual volta nem ligar a lição ao trecho da tabela; voltas intermediárias podem conter apenas delta. A lição pode perder a demonstração de que o tema estava presente. `CLAUDE.md:614-619`.

- **O gabarito opus não cobre todos os falsos negativos.** Existe a categoria `(d) NENHUM VIU`, descoberta por suite, produção ou outras pessoas; ela exige cura estrutural, registro da invariante e selo. Logo medir apenas obras com achado do opus não estima essa família, e lição não substitui sua cura. `CLAUDE.md:623-629`. [nao verificado]

- **Falhas operacionais incompletas.** O contrato permanente exige erro, timeout e limite, além de responsável pelo desligamento. A v2 fala em formato inválido e rc75, deixando sem conduta explícita timeout e demais falhas durante geração/consolidação. `CLAUDE.md:651-657`.

- **Hash com par em log não prova autoria exclusiva.** A regra pede escritor canônico com tripwire; um conteúdo idêntico ao já registrado pode ser regravado fora do wrapper e manter o mesmo SHA. Assim o selo descrito prova correspondência de conteúdo, não toda escrita externa. Inferência de `CLAUDE.md:32-34`; implementação futura [nao verificado].

- **Dependência declarada realmente está pendente.** `4435ec64` está registrado como pronto, porém com pouso retido porque o wrapper passa a exigir `PERGUNTA:` e há construtores em voo. Não assumir essa interface implantada. `app/docs/BACKLOG.md:871`.

- **A avaliação anterior de contexto já recomenda vigiar eco.** O registro declara resultado neutro, ganho concentrado nos achados do próprio Codex, desempenho 3×4 nos achados opus e tokens 8,86M×6,03M; isso sustenta separar descoberta nova de repetição de lições na avaliação proposta. `app/docs/RELATO.md:176`.

`CLAUDE-MD-ENXUTO` e `CODEX-CONTEXTO-SEM-CONDUTA` não apareceram na busca documental feita; seu conteúdo e estado ficam [nao verificado].

## 31/10 11:35 · sessao 6dee5b2d66fb · CODEX-LICOES-PROBE0G · leitura · origem=Code

1) ZEBRA-07E712A0 [nao verificado]

2) `AGENTS.md` — diretório `/home/ronald/codex_ws/19d2a06fc144d68abf7cd05b67a4d150c49eb776`, fornecido no contexto pelo usuário; carregamento direto do arquivo [nao verificado].

## 31/10 11:35 · sessao 834fefaf8832 · CODEX-CONTEXTO-SEM-CONDUTA · verificador · origem=Code

**O principal problema é que excluir títulos não separa conduta de lei, e colocar o selo na pasta não garante sua execução no push.** (`app/docs/LEIS.md:22`; `bin/pre-push.sh:13–16`)

1. **O selo pode ficar sem chamador no pre-push.** O loop de `bin/tests/test_*.sh` está na régua; o pre-push declara expressamente que não executa esse loop. Precisa haver chamada explícita, conferindo o commit enviado, inclusive quando o arsenal é dispensado por “JÁ VERDE”. (`bin/regua.sh:138–145`; `bin/pre-push.sh:13–16,33–38,62–66`)

2. **INCLUIDAS/EXCLUIDAS, como enumeradas, não cobrem o CLAUDE atual.** Faltam classes para **1, 3, 5, 6b, 7 e 8**. O selo de cobertura prometido começaria vermelho nesta cópia. A seção 6b, particularmente, contém regras de aplicação da CCT e cálculo, portanto sua exclusão não seria mera retirada de conduta. É necessário conferir novamente essa classificação depois da dependência ENXUTO. (`CLAUDE.md:90,194,325,430,460,821`; `CLAUDE.md:433–457`)

3. **Conduta continua entrando pelas próprias fontes incluídas.** A seção 2 manda executar deploy e merge; 4b determina ordem de trabalho. LEIS declara regras como `CONDUTA` e contém construção em cópia e moratória de propostas. Esses textos não precisam reproduzir o título nem o hash das seções excluídas: o detector proposto pode ficar verde apesar deles. (`CLAUDE.md:137,168–176,320–323`; `app/docs/LEIS.md:22,63,138`)

4. **PLANO-PADROES não é integralmente FATO/LEI.** Contém fila, responsáveis e estados; identifica expressamente contexto, alternativas e consequências como **redação do Code a partir do aval e do RELATO**. Se entrar inteiro, volta a compartilhar interpretação do construtor com o verificador, contrariando a separação pretendida. Falta declarar o recorte desse arquivo. (`app/docs/PLANO-PADROES.md:7–13,26,59`; `app/docs/PROMPTS.md:1684`)

5. **Excluir uma seção inteira também pode retirar lei e suas retificações.** A seção 7b contém regras do TXT e condições de aplicação em competência aberta. Há inclusive divergência documental concreta: PLANO-PADROES exige “DIFF antes” pela 7b, enquanto a 7b restringe DIFF antecipado a exportada e troca de motor. O contrato precisa preservar a regra vigente por outra fonte completa; cobertura de títulos não demonstra isso. (`CLAUDE.md:677–693,708–725`; `app/docs/PLANO-PADROES.md:29–32`)

6. **O gerador anexado ainda descreve outro recorte.** Seu `projetar()` inclui **0/4/4a**, não **2/4/4a/4b**; PLANO-PADROES e JUIZES são apenas ponteiros; não há as tuplas INCLUIDAS/EXCLUIDAS. Portanto, o anexo não demonstra implementação deste contrato. Disponibilidade e funcionamento dessa versão na cópia: **[nao verificado]**. A dependência DA-CASA está registrada como aguardando pouso. (`app/docs/BACKLOG.md:871`)

7. **Gerar no disco não basta: o resultado precisa integrar o hash chamado.** O wrapper consome `git archive "$HASH"`, torna a cópia sem escrita e reutiliza o diretório existente sem reconferi-lo. Geração posterior ao commit não chega à chamada. O A/B também precisa identificar cópias distintas e controladas; trocar arquivos sob o mesmo hash compromete essa identidade. (`bin/codex_pergunta.sh:87,91–101,157–158`)

8. **O A/B está menos definido que o protocolo já registrado.** O aval anterior exige **n=2 por braço e chave invertida**; “mesma pergunta” não preserva sozinho esses controles. Também é preciso fixar código, modelo e anexos: o wrapper concatena os anexos e a pergunta, permitindo que a conduta retirada do AGENTS entre novamente no prompt. A medição anterior registra forte variação de tokens entre rodadas. (`app/docs/PROMPTS.md:1686`; `bin/codex_pergunta.sh:103–135`; `app/docs/RELATO.md:176,179`)

9. **O teto segue sem solução de recorte.** Os ~42,7 kB informados excedem 30.000; recusar a escrita protege o limite, mas não produz o contexto novo. O tamanho final e o tempo inferior a um segundo permanecem **[nao verificado]**. Os selos atuais do wrapper verificam forma/sandbox; a prova real existente verifica ausência de escrita, não carregamento do novo contexto. (`bin/tests/test_codex_pergunta.sh:10–33`; `bin/tests/prova_codex_pergunta_real.sh:15–28`)

## 31/10 11:33 · sessao f7baeb4ed248 · CODEX-LICOES-PROBE0 · leitura · origem=Code

1) ZEBRA-07E712A0 [nao verificado]

2) `AGENTS.md`, do diretório `/home/ronald/codex_ws/19d2a06fc144d68abf7cd05b67a4d150c49eb776`, fornecido no contexto pelo usuário. [nao verificado]

## 31/10 11:33 · sessao 4db2ad64e94e · CODEX-LICOES · verificador · origem=Code

**A cópia e o anexo fornecido representam estados diferentes.** Na cópia, o wrapper termina sem os blocos de AGENTS, base/diff e julgamento do brief presentes no anexo; o BACKLOG ainda registra o contexto aguardando pouso. As referências abaixo são da cópia. A origem/versionamento do wrapper anexado é [não verificado]. (`bin/codex_pergunta.sh:118`, `bin/codex_pergunta.sh:177`, `app/docs/BACKLOG.md:871`)

1. **Os dois papéis novos hoje nem chegam ao Codex.** `licao` e `licao-consolidar` caem na recusa com rc 2. Além de ampliar o wrapper, é necessário atualizar os leitores: chamadas e tokens desses papéis seriam agrupados em `outro`. (`bin/codex_pergunta.sh:48`, `bin/codex_limite_estado.py:23`, `bin/codex_limite_estado.py:78`, `bin/codex_limite_estado.py:194`)

2. **A pergunta fixa conflita com o cabeçalho existente.** O wrapper exige `arquivo:linha ou [nao verificado]`; a lição proíbe caminho e `:NNN`. Uma lição sustentada por citação seria recusada; usar `[nao verificado]` satisfaria o cabeçalho sem comprovar a lição. É necessária uma regra específica por papel, preservando a exigência nos demais — o selo atual cobra esse cabeçalho. (`bin/codex_pergunta.sh:124`, `bin/tests/test_codex_pergunta.sh:27`)

3. **O material do gatilho está subespecificado.** A classificação vigente recebe **todas** as perguntas/respostas, as linhas de triagem e devolve trechos por achado. O contrato passa “a pergunta .q + a resposta .r”, sem escolher a volta nem exigir a tabela classificada como anexo. Em obra com várias voltas, isso pode ensinar uma conclusão sem sua história ou evidência. Inferência baseada no fluxo existente. (`CLAUDE.md:598`, `CLAUDE.md:614`, `CLAUDE.md:616`, `CLAUDE.md:617`)

4. **“Arquivo na árvore viva” não significa “próxima chamada recebe”.** O wrapper lê um `git archive` do hash e reutiliza a cópia existente; não acompanha alterações posteriores no vivo. Para cumprir o contrato antes do próximo commit, precisa anexar explicitamente uma fotografia do arquivo em `$VIVO`, identificando sua versão. Usar `$WS` entrega conteúdo antigo; usar `$R` depende de onde o wrapper foi executado. (`bin/codex_pergunta.sh:29`, `bin/codex_pergunta.sh:30`, `bin/codex_pergunta.sh:87`, `bin/codex_pergunta.sh:91`; `app/docs/LEIS.md:132`)

5. **Anexar em qualquer papel acrescenta uma dependência de raspagem a todas as chamadas.** Hoje qualquer `--anexo` exige lista de nomes válida e pode encerrar com rc 4 antes do Codex. Os anexos também compartilham um corte silencioso de 300.000 bytes: dependendo da ordem, lições ou evidências podem desaparecer. O contrato precisa definir indisponibilidade, ordem e tratamento do excesso. (`bin/codex_pergunta.sh:104`, `bin/codex_pergunta.sh:105`, `bin/codex_pergunta.sh:111`, `bin/codex_pergunta.sh:115`; `bin/tests/test_codex_sonda.sh:76`)

6. **Rodar em fundo não garante ficar fora do caminho crítico.** Lições disputariam os mesmos seis slots com verificadores e sondas; o slot permanece ocupado até a saída. O timeout só começa depois da espera pelo slot. Portanto, uma lição pode atrasar a próxima chamada necessária à obra, mesmo sem alguém esperar diretamente por ela. (`bin/codex_pergunta.sh:47`, `bin/codex_pergunta.sh:138`, `bin/codex_pergunta.sh:150`, `bin/codex_pergunta.sh:153`, `bin/codex_pergunta.sh:157`)

7. **A consolidação contradiz o teto absoluto.** Contraexemplo: existem 40 linhas, entra a 41ª e a consolidação retorna 75 ou resposta inválida; “mantém” deixa 41 linhas, contrariando o selo. O retorno 75 já é um caminho explícito do wrapper; preservar o teto nesse cenário não está definido. **[não verificado quanto à implementação futura]** (`bin/codex_pergunta.sh:167`, `bin/codex_pergunta.sh:171`)

8. **O lock do append não resolve a substituição concorrente.** Se a consolidação ler uma versão, outra chamada acrescentar uma lição e depois ocorrer a substituição, o acréscimo pode ser perdido. Falta definir lock estável compartilhado entre append, leitura e publicação da consolidação, ou comparação de versão antes da troca. Os `flock` atuais controlam capacidade de execução, não o documento. **[não verificado quanto à implementação futura]** (`bin/codex_pergunta.sh:138`, `bin/codex_pergunta.sh:144`)

9. **A validação e os erros estão incompletos.** A consolidação exige apenas até 40 linhas com CLASSE: não repete a proibição de caminhos nem estabelece mínimo de uma linha. Também falta dizer que só se publica após rc 0, saída válida e `thread_id` presente; hoje o CLI recebe diretamente o destino da resposta, pode terminar com erro/timeout, e o SID ausente é apenas informado. (`bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:159`, `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:175`, `bin/codex_pergunta.sh:176`)

10. **O selo proposto não prova “escrita fora do wrapper”.** Documento e log são dois arquivos: falha entre as gravações pode produzir linha legítima sem par. A consolidação também cria novos textos e precisa registrar novos pares. Falta protocolo de recuperação e identidade do achado para impedir duplicação ao republicar a tabela; a identidade atual das chamadas usa obra/hash/pid, sem identificar o achado. Ausência de par comprovaria inconsistência, não sua causa. **[não verificado quanto à implementação futura]** (`bin/codex_pergunta.sh:53`, `bin/codex_pergunta.sh:59`, `bin/codex_limite_estado.py:63`)

11. **“Qualquer papel” reintroduz opinião no verificador.** Lições derivadas dos achados do opus voltariam como contexto das verificações seguintes, inclusive da mesma obra. Isso exige uma exceção explícita à independência atual: pergunta sem hipótese e proibição de achado do opus no brief do verificador. O risco de eco é uma inferência; há registro documental anterior desse sinal, sem comprovação aqui dos resultados brutos. (`CLAUDE.md:579`, `CLAUDE.md:585`, `app/docs/PROMPTS.md:1684`, `app/docs/RELATO.md:176`)

12. **A/B, desligamento e certificação ainda não têm encaixe definido.** O pouso e o protocolo de `CODEX-CONTEXTO-SEM-CONDUTA` são [não verificado]. É preciso congelar as lições entre os braços, garantir sua ausência no braço “sem”, registrar a versão enviada e definir a chave de desligamento. O precedente documentado exige mesma pergunta/hash e controle de posição; os testes atuais cobrem estrutura, raspagem e limite, sem certificar essas novas invariantes. Uma segunda chamada inline para consolidar também quebraria o selo que exige uma única ocorrência de `codex exec`. (`app/docs/RELATO.md:179`, `app/docs/PROMPTS.md:1686`, `bin/tests/test_codex_pergunta.sh:13`, `bin/tests/test_codex_sonda.sh:57`, `bin/tests/test_codex_sonda.sh:87`, `bin/tests/test_codex_limite.sh:31`)

## 31/10 11:33 · sessao 3f91e974b7bb · V3-RETOMA-FILA1 · verificador · origem=Code

O desenho acerta ao substituir a soma manual pela lavra, mas **não garante ausência de escrita nem comparabilidade completa**: há auditoria fora de `lavrar`, filtros diferentes de colaboradores/dias e critérios diferentes de data. (`app/ponto/services/fechamento.py:523–558`; `app/ponto/management/commands/diff_calculador.py:102–108`; `app/ponto/services/dia_pago.py:173–290`)

**(a) Quem escreve e quem lê**

O encadeamento efetivo é: motor produz resultados das fatias → fechamento compõe o `ResultadoMes` e prepara mapas/partições → `lavrar` distribui os valores e constrói `DiaPago` → o captador proposto substituiria a persistência por linhas em memória. (`app/ponto/services/fechamento.py:270–300`; `app/ponto/services/fechamento.py:1169–1205`; `app/ponto/services/dia_pago.py:343–358`)

| Valor/argumento | Quem produz ou escreve | Quem lê e o que escreve na lavra |
|---|---|---|
| `colab`, `mes`, `ano` | Laço do fechamento e argumentos do recalculo. (`app/ponto/services/fechamento.py:432–435`, `:598`) | Identificação das linhas e filtro da substituição. (`app/ponto/services/dia_pago.py:343`, `:355–357`) |
| `ResultadoMes` composto por colaborador | `apurar_por_fatia` cria o composto **antes** de calcular as fatias; concatena listas, soma acumuladores e mescla mapas. O recalculo recebe `_ap.resultado`. (`app/ponto/services/fechamento.py:182`, `:270–300`, `:729–731`) | O DIFF atual captura a primeira instância por `colaborador_id` com `setdefault`; depois troca apenas `aut.resultado`. (`app/ponto/management/commands/diff_calculador.py:131–135`, `:154–162`) |
| `periodos` | Motor de cada fatia; fechamento concatena mantendo os objetos. (`app/ponto/services/fechamento.py:270–281`) | Trabalhadas, noturnas, HE50, HE50 noturna, HE100, HE100 feriado, HE100 noturna, saída antecipada e intra. (`app/ponto/services/dia_pago.py:173–186`) |
| `ft_certa`, `ft_feriado`, `ft_sem_escala` | Fechamento parte `resultado.periodos_ft`: escala certa; retira feriados; calcula complemento **por identidade**. (`app/ponto/services/fechamento.py:908–928`, `:948–949`) | Respectivamente: folga; HE100 **e** HE100 feriado; trabalhadas. Todos carregam noturno e intra. (`app/ponto/services/dia_pago.py:223–265`) |
| `orfaos_por_dia` | Outra execução de motor, `_r_orfa`; fechamento agrupa seus períodos por data local. **Não incorpora esses períodos ao composto.** (`app/ponto/services/fechamento.py:802–818`) | Acrescenta trabalhadas e detalhe `o85_dia_sem_vinculo_min`. (`app/ponto/services/dia_pago.py:267–270`) |
| `faltas_por_dia` | `minutos_de_falta_por_dia`. (`app/ponto/services/fechamento.py:848–850`) | `horas_falta` e detalhe `falta_min`. (`app/ponto/services/dia_pago.py:272–275`) |
| `atraso_por_dia`, `credito_por_dia` | `credito_parcial.atraso_por_dia(resultado.periodos)` e `creditos_por_dia`. (`app/ponto/services/fechamento.py:962–968`) | Atraso líquido: `max(atraso − min(atraso, crédito), 0)/60`; detalhe do crédito utilizado. (`app/ponto/services/dia_pago.py:190–195`) |
| `abono_por_dia`, `previsto_por_dia`, `realizado_por_dia` | Grade do fechamento → `por_dia_da_grade`; abono cobre o previsto não realizado na ausência, realizado tem teto diário. (`app/ponto/services/fechamento.py:981–982`, `:1028–1032`; `app/ponto/services/dia_pago.py:384–400`) | Os três `minutos_*`, **em minutos**, podendo criar dias. (`app/ponto/services/dia_pago.py:286–290`) |
| `reflexo_dsr`, `banco` | Campos montados no fechamento; banco é zerado quando `sem_movimento`. (`app/ponto/services/fechamento.py:1069–1072`, `:1175–1176`) | Somente `horas_reflexo_dsr` e `saldo_banco_horas` do ajuste. (`app/ponto/services/dia_pago.py:348–356`) |
| `dia_da_jornada` | Motor escreve/mescla `id(periodo) → data`, incluindo FT; fechamento mescla mapas das fatias. (`app/ponto/motor_calculo_v2.py:1011–1013`, `:2878–2897`; `app/ponto/services/fechamento.py:295–300`) | Lavra consulta por `id(p)`; alimentação v3 também. Sem chave, lavra usa data local; alimentação v3 conta `sem_dia_da_jornada` e descarta o período. (`app/ponto/services/dia_pago.py:126–132`; `app/ponto/calculador/alimentacao.py:63–72`) |
| `furos_do_motor` | `resultado.datas_falta` passa por `_furos_encerrados`. (`app/ponto/services/fechamento.py:1083–1085`, `:1194`) | Cria dias mesmo sem horas e escreve `furo_apurado`: `None` significa não perguntado; lista vazia significa nenhum furo. (`app/ponto/services/dia_pago.py:279–281`, `:343–345`) |
| `versao`, `versao_motor`, `regua_por_dia`, `evento_origem` | Chamada motor fornece `'motor'`, impressão da régua, mapa das fatias e `'backfill'` no modo proposto. (`app/ponto/services/fechamento.py:268–269`, `:739–740`, `:1195–1205`) | Metadados comuns; régua diária substitui `versao_motor`, truncado em 32 caracteres; ausência de régua é contada no ajuste. (`app/ponto/services/dia_pago.py:333–356`) |
| `versao_dna` | As duas chamadas omitem; default `''`. (`app/ponto/services/dia_pago.py:139`; `app/ponto/services/fechamento.py:1169–1205`, `:1245–1264`) | Metadado comum. (`app/ponto/services/dia_pago.py:333–334`) |
| `rubricas`, `do_motor` | Chamada motor omite ambos; oráculo fornece `_rub_o`, produzido por `rubricas_por_dia`. (`app/ponto/services/fechamento.py:1169–1205`, `:1235–1256`) | Validação de dono de cada campo; substituição dos valores fornecidos, preservando campos declarados `do_motor`; detalhe `calculador_nao_decide`. (`app/ponto/services/dia_pago.py:310–331`) |

Nos objetos capturados, `lavrar` escreve `data`, `tipo`, identificação, rubricas, `detalhe` e proveniência; todas as rubricas começam em zero. O modelo declara essas rubricas como `FloatField`, além dos três campos de minutos. (`app/ponto/services/dia_pago.py:169–170`, `:333–356`; `app/ponto/models.py:325–351`)

O leitor do instrumento deve usar `data`, `tipo` e os campos de `alvo`: hoje `alvo` exclui falta, reflexo de DSR e banco; também não contém os três campos de grade nem os metadados. (`app/ponto/management/commands/diff_calculador.py:78`; `app/ponto/calculador/regras.py:400–435`)

Capturar kwargs não reproduz integralmente um objeto persistido: `lavrado_em` é automático, a PK não é fornecida pela lavra e o ajuste recebe `furo_apurado=None` pelo default do modelo. Esses campos não são necessários para a comparação das rubricas de `alvo`. (`app/ponto/models.py:281`, `:369–380`; `app/ponto/services/dia_pago.py:355–356`; `app/ponto/management/commands/diff_calculador.py:78`)

**(b) Escrita no banco e dependências do modelo**

**Dentro de `lavrar`, a substituição é viável.** O import de `DiaPago` ocorre dentro da função; as únicas operações de persistência são `filter(...).delete()` e `bulk_create(novas)`. Seus auxiliares usados nesse caminho trabalham em memória. (`app/ponto/services/dia_pago.py:50–76`, `:109–132`, `:163`, `:357–358`)

O substituto precisa também ser **construtor chamável**, aceitando os kwargs das linhas de dia e ajuste. Apenas constantes e manager não bastam. `lavrar` não exige `_meta`, `save`, PK, validação ou métodos próprios do modelo nesses caminhos. (`app/ponto/services/dia_pago.py:343–358`)

**A operação inteira, porém, pode escrever fora desse patch.** A combinação proposta ativa a busca das empresas exportadas; a guarda de setembro/2026 pode excluir colaboradores e chamar `evento_kw` antes de qualquer lavra. Esse caminho chega a `LogAuditoria.objects.create()`, sem passar por `DiaPago`. A ocorrência concreta depende de exportações e colaboradores que o dado fornecido não demonstra: **[não verificado]**. (`app/ponto/services/fechamento.py:36–43`, `:523–558`; `app/core/observ.py:133–148`)

Logo, o `finally` que restaura `DiaPago` resolve a duração do patch; **não prova “nenhuma escrita” do recalculo**. (`app/ponto/services/fechamento.py:551–557`; `app/ponto/services/dia_pago.py:163`, `:357–358`)

**(c) Dia, universo e rubricas comparáveis**

Há cinco desencontros materiais:

1. **Colaboradores:** o comando seleciona empresas ativas e IDs com `FechamentoMensal` existente; o recalculo usa `universo_da_competencia`, com seu predicado de situação/demissão, seguido da guarda. Capturar a empresa inteira não torna esses universos iguais. (`app/ponto/management/commands/diff_calculador.py:97–108`; `app/ponto/services/fechamento.py:423–429`, `:538–548`)

2. **Dias da lavra:** períodos, órfãos, faltas, furos e grade podem criar linhas; `lavrar` não recebe nem aplica `ini/fim`. A porta v3 aplica a janela e visita dias do núcleo mais os dias bloqueados publicados pelo motor. (`app/ponto/services/dia_pago.py:135–141`, `:173–290`; `app/ponto/calculador/alimentacao.py:454–462`)

3. **O DIFF ainda estreita a porta v3:** percorre somente `v.minutos`, embora `rubricas_por_dia` possa devolver também dias bloqueados sem segmento. Depois a união dos dias transforma lado ausente em `{}` e rubrica ausente em zero, sem cegueira registrada para dias nunca visitados. (`app/ponto/management/commands/diff_calculador.py:428–431`, `:469–472`, `:517–535`; `app/ponto/calculador/alimentacao.py:454–458`)

4. **O dia não é uniforme:** a lavra agrupa períodos pelo mapa da jornada, mas o fechamento classifica FT pela **data local da entrada**; atraso e órfãos também chegam agrupados pelo calendário. A v3 pergunta vínculo/escala certa pelo dia da jornada. Portanto, compartilhar o resultado não elimina esse desencontro de critério. Casos efetivamente atingidos: **[não verificado]**. (`app/ponto/services/fechamento.py:814–817`, `:909–924`; `app/ponto/services/credito_parcial.py:29–43`; `app/ponto/services/dia_pago.py:174`, `:190–195`; `app/ponto/calculador/alimentacao.py:460–511`)

5. **Compartilhar resultado não compartilha toda a alimentação:** o comando conserva `aut.esc` e `aut.feriados`; o fechamento usa seu vínculo de referência. O espelho pergunta pelo último dia da janela; o fechamento procura do fim para trás até encontrar vínculo. A porta resolve CCT/família antes do laço diário usando o template recebido. Divergência concreta: **[não verificado]**. (`app/ponto/management/commands/diff_calculador.py:151–163`, `:361`, `:417–423`; `app/ponto/services/espelho.py:1069–1070`; `app/ponto/services/fechamento.py:123–129`, `:826–827`; `app/ponto/calculador/alimentacao.py:377–395`)

Além de HE100 feriado e folga, a troca muda potencialmente estas rubricas do lado v2, comparada à soma manual atual:

| Rubrica | Mudança introduzida pela lavra |
|---|---|
| `horas_trabalhadas` | Acrescenta FT sem escala certa e órfãos. (`app/ponto/services/dia_pago.py:257–270`) |
| `horas_extras_100` | Acrescenta o trabalhado de FT feriado, além de preencher seu campo específico. (`app/ponto/services/dia_pago.py:244–251`) |
| `horas_noturnas` | Passa a carregar o noturno de todos os grupos FT. (`app/ponto/services/dia_pago.py:223–263`) |
| `horas_intra_indenizada` | Passa a carregar a intra de todos os grupos FT. (`app/ponto/services/dia_pago.py:223–263`) |
| `horas_atraso` | Usa mapa diário e abate crédito parcial, em vez da soma bruta dos períodos. (`app/ponto/services/dia_pago.py:190–195`; `app/ponto/management/commands/diff_calculador.py:261`) |
| `horas_extras_50_noturna`, `horas_extras_100_noturna` | Passam a ser preenchidas; a soma manual atual omite ambas. (`app/ponto/services/dia_pago.py:181`, `:184`; `app/ponto/management/commands/diff_calculador.py:253–268`) |

HE50 e saída antecipada dos períodos comuns mantêm a soma aritmética; ainda podem aparecer em uma comparação com universo diferente. (`app/ponto/services/dia_pago.py:180`, `:185`; `app/ponto/management/commands/diff_calculador.py:255`, `:262`, `:527–535`)

**(d) Outras falhas do desenho**

- **Ajuste não é dia:** a lavra sempre cria `tipo='ajuste', data=None`, mesmo sem dias, e seu retorno inclui essa linha. É necessário filtrá-la antes de construir o universo ordenado de datas; captura existente ou retorno positivo não prova dia comparável. (`app/ponto/services/dia_pago.py:346–359`; `app/ponto/management/commands/diff_calculador.py:527`)

- **O erro não chega necessariamente ao comando:** o fechamento absorve exceções da lavra motor, da preparação/lavra oráculo e do bloco externo por colaborador. `erro_lavra_do_fechamento` precisa ser registrado dentro do captador; captura parcial deve ser descartada. Ter `ResultadoMes` capturado não prova lavra concluída. (`app/ponto/services/fechamento.py:1206–1209`, `:1265–1276`; `app/ponto/management/commands/diff_calculador.py:131–135`)

- **Oráculo usa o mesmo nome já importado:** `_lavrar_dia_pago` é importado uma vez e reutilizado na segunda chamada. O wrapper precisa tratar `versao!='motor'` explicitamente; restaurar somente o atributo do módulo não altera esse alias local. A preparação do oráculo continua ocorrendo em outubro em diante, mesmo que sua chamada final vire no-op. (`app/ponto/services/fechamento.py:1168–1169`, `:1233–1264`)

- **É preciso preservar a função real e a identidade dos períodos:** chamar novamente `_dpm.lavrar` após substituí-la causaria recursão; a chamada real deve usar a referência salva. Recriar períodos invalidaria as chaves `id(p)` do mapa. (`app/ponto/management/commands/diff_calculador.py:121`, `:137–142`; `app/ponto/services/fechamento.py:278–300`; `app/ponto/services/dia_pago.py:129`)

- **Exigir apenas `--periodos-do-fechamento` é insuficiente:** sem `--pares-da-autoridade`, `_ins_porta=None`, a porta de rubricas não é chamada e `c_dia` fica vazio; a comparação pode tratar toda a lavra como divergência contra zero. (`app/ponto/management/commands/diff_calculador.py:325–337`, `:417–428`, `:527–535`)

- **A autoridade antiga ainda exclui colaboradores:** mesmo com lavra capturada, o comando chama `autoridade_do_periodo` antes de trocar o resultado; uma falha ali incrementa `erro_motor` e abandona o colaborador. (`app/ponto/management/commands/diff_calculador.py:145–162`)

- **O selo proposto cobre só parte do risco:** períodos sintéticos não exercitam guarda/auditoria, composição por fatias, órfãos, crédito parcial, dias sem segmentos e exceções absorvidas. Esses caminhos existem separadamente no código. (`app/ponto/services/fechamento.py:270–300`, `:545–557`, `:802–818`, `:962–969`, `:1206–1209`; `app/ponto/calculador/alimentacao.py:454–458`)

A indicação “Roda por ATO” está no cadastro do comando; já frota com `sem_lavra=0`, resultados dos re-DIFFs 09/10 e coluna `INSTRUMENTO` próxima de zero não foram demonstrados pelo anexo: **[não verificado]**. A saída atual contém motor, calculador, delta e contagens, sem coluna `INSTRUMENTO`. (`app/config/crons.py:966–970`; `app/ponto/management/commands/diff_calculador.py:550–556`)

## 31/10 11:33 · sessao 1835b36532b4 · - · - · origem=Ronald (tmux)

Achados materiais nas fontes:

1. **As classes declaradas não cobrem o CLAUDE atual.** Ficam sem classe as seções 1, 3, 5, 6b, 7 e 8. O selo de cobertura prometido começaria vermelho; classificar todas como excluídas também não é neutro: 6b contém regras de folha e seleção da CCT. Referências: `CLAUDE.md:90`, `CLAUDE.md:194`, `CLAUDE.md:325`, `CLAUDE.md:430`, `CLAUDE.md:460`, `CLAUDE.md:821`; conteúdo jurídico em `CLAUDE.md:433` e `CLAUDE.md:444`.

2. **“Excluir seções de conduta” não produz “sem conduta”.** As próprias seções incluídas misturam arquitetura com instruções ao executor: seção 2 manda publicar, resolver merge e fazer deploy imediatamente; 4b manda fechar itens, ordena R1/R2 e famílias F1. Referências: `CLAUDE.md:137`, `CLAUDE.md:168`, `CLAUDE.md:174`, `CLAUDE.md:303`, `CLAUDE.md:320`, `CLAUDE.md:323`. A seção 4 também contém aval para editar motor e forma obrigatória de sonda: `CLAUDE.md:242`, `CLAUDE.md:246`. [nao verificado]

3. **LEIS reintroduz a conduta excluída sem reproduzir seus cabeçalhos.** O próprio documento define `CONDUTA` como regra de processo, e suas linhas L-001, L-005, L-117 e L-123 reproduzem mandamentos de construção, fila, propostas e revisão. Um detector de título/hash do recorte original pode ficar verde diante dessas paráfrases. Referências: `app/docs/LEIS.md:22`, `app/docs/LEIS.md:59`, `app/docs/LEIS.md:63`, `app/docs/LEIS.md:138`, `app/docs/LEIS.md:144`.

4. **PLANO-PADROES inteiro viola a premissa de compartilhar somente FATO/LEI.** Há fila, estado de obra e dono; o arquivo identifica expressamente contexto, alternativas e consequências como redação do Code derivada do RELATO. Incluí-lo integralmente compartilha a interpretação do construtor que o desenho quer afastar do verificador. Referências: `app/docs/PLANO-PADROES.md:7`, `app/docs/PLANO-PADROES.md:26`, `app/docs/PLANO-PADROES.md:59`. Também inclui conduta de revisão/commit: `app/docs/PLANO-PADROES.md:53`, `app/docs/PLANO-PADROES.md:56`.

5. **Separar por seção pode remover lei junto da conduta.** Não se pode presumir que 7b só contém comportamento de agente: ali também estão regra do TXT como fotografia e a condição de apply em competência aberta. A correção é decidir recortes de conteúdo e sua autoridade; o título da seção sozinho não define a natureza de tudo que contém. Referências: `CLAUDE.md:683`, `CLAUDE.md:708`. Não afirme perda efetiva sem verificar se a mesma lei completa entra por LEIS.

6. **O vocabulário incluído mantém referências de conduta.** GLOSSARIO liga padrões a construir em cópia, comboio e proibição da LEI-AKITA. É outro caminho de entrada que a blacklist de títulos não mede. Referências: `app/docs/GLOSSARIO.md:18`, `app/docs/GLOSSARIO.md:20`, `app/docs/GLOSSARIO.md:23`.

Medição independente somente em memória: com CLAUDE 2/4/4a/4b da cópia, índice LEIS pelo algoritmo do anexo, MAPA a–h e GLOSSARIO/PLANO/JUIZES inteiros, o subtotal sem moldura/cabeçalho é **42.330 B**. Isso confirma o conflito com 30.000, mas o número não tem arquivo:linha próprio; no texto final, usar o ~42,7k fornecido com `[nao verificado]` ou identificar explicitamente como medição em memória.

## 31/10 11:32 · sessao 3bcf16e165d3 · - · - · origem=Ronald (tmux)

Pontos documentais para a consolidação:

- **A base atual é anterior ao anexo fornecido.** O wrapper da cópia não tem o cabeçalho de contexto nem perguntas fixas: monta somente regras, pergunta e anexos em `bin/codex_pergunta.sh:118-135`. O instrumento CODEX-CONTEXTO-DA-CASA consta como aguardando pouso em `app/docs/BACKLOG.md:871`; o texto anexado pelo usuário corresponde a outro estado [não verificado quanto à origem].
- **Dependência sem evidência nesta cópia.** CODEX-CONTEXTO-SEM-CONDUTA não apareceu na busca em CLAUDE.md/app/docs; seu pouso e protocolo A/B são [não verificado]. O instrumento documentado é CODEX-CONTEXTO-DA-CASA (`app/docs/BACKLOG.md:871`).
- **O gatilho precisa preservar a história inteira.** A classificação existente recebe TODOS os `.q`/`.r` e linhas de triagem e devolve os trechos por achado (`CLAUDE.md:614-618`). O contrato fala em “a pergunta .q + a resposta .r” sem definir qual volta; numa obra com várias voltas, anexar apenas um par pode esconder onde o tema apareceu [inferência: `CLAUDE.md:598-600`, `CLAUDE.md:616-618`].
- **Lições de opus para todo papel ameaçam a independência.** A pista exige pergunta sem hipótese (`CLAUDE.md:579`, `CLAUDE.md:585-589`) e o aval do contexto proíbe achado do opus/hipótese no verificador (`app/docs/PROMPTS.md:1684`). Reenviar a qualquer verificador as lições derivadas dos achados do opus reintroduz esse conteúdo por outra via [inferência: mesmas referências].
- **A/B necessita braços realmente comparáveis e gabarito separado.** O primeiro ensaio documentado variava só AGENTS.md, tinha mesma `.q`/hash e n=2; registrou viés de posição (`app/docs/RELATO.md:179`). O aval posterior exigiu chave invertida e gabarito do opus (`app/docs/PROMPTS.md:1686`). A re-medida registra ganho concentrado nos itens do próprio Codex e “sinal fraco de eco” (`app/docs/RELATO.md:176`); efeitos das novas lições continuam [não verificado].
- **L-106 autoriza levar o documento no próximo marco, sem dar publicação imediata ao commit.** A regra determina docs junto do código do marco (`CLAUDE.md:778-780`, `app/docs/LEIS.md:132`). Como o wrapper atual lê `git archive` e reutiliza a cópia do hash (`bin/codex_pergunta.sh:87`, `bin/codex_pergunta.sh:91-101`), lição recém-escrita no vivo só chega antes do commit se houver canal explícito de anexo do vivo; arquivo dentro da cópia não acompanha o vivo [inferência: mesmas referências].
- **Atualizar só o vocabulário do wrapper deixa a medição incompleta.** Os papéis conhecidos do ESTADO são quatro (`bin/codex_limite_estado.py:23`); papel novo vira `outro` tanto em chamadas (`bin/codex_limite_estado.py:74-78`) quanto em tokens (`bin/codex_limite_estado.py:191-194`).
- **O prompt fixo de “sem caminho” colide com a evidência exigida pelo wrapper.** Toda afirmação exige arquivo:linha ou `[nao verificado]` (`bin/codex_pergunta.sh:123-124`); uma lição verificada com citação de arquivo seria rejeitada pelo próprio contrato [inferência: referências e contrato do usuário].

## 31/10 11:32 · sessao 545919d7e475 · - · - · origem=Ronald (tmux)

Achados de integração e testes, apenas por leitura: [nao verificado]

- **A cópia e o anexo são versões diferentes.** O wrapper local termina em `bin/codex_pergunta.sh:177`; seu cabeçalho é montado diretamente em `bin/codex_pergunta.sh:118`, sem os blocos de AGENTS/base/juiz do brief presentes no dado fornecido. Não se pode atribuir esses blocos ao hash local. `app/docs/BACKLOG.md:871` ainda descreve o contexto como construção em outra cópia e pouso retido.

- **Os dois papéis novos são recusados antes da chamada:** a whitelist aceita apenas `leitura|censo|verificador|sonda` e sai com rc 2 nos demais casos. `bin/codex_pergunta.sh:48`. Há teste para papel inválido, sem caso de `licao` ou `licao-consolidar`: `bin/tests/test_codex_sonda.sh:99`.

- **Há conflito de formato com o cabeçalho atual:** toda afirmação deve carregar `arquivo:linha ou [nao verificado]`, enquanto a lição exige não conter caminhos. `bin/codex_pergunta.sh:124`. Remover essa regra globalmente quebra o selo estrutural que exige o texto: `bin/tests/test_codex_pergunta.sh:27`.

- **Somente ampliar a whitelist não integra as métricas.** O leitor mantém quatro papéis fixos e agrupa os demais em `outro`, inclusive nos tokens. `bin/codex_limite_estado.py:23`, `bin/codex_limite_estado.py:78`, `bin/codex_limite_estado.py:194`. O selo confirma esse agrupamento: `bin/tests/test_codex_papel_estado.sh:31`.

- **Consolidação pode colidir com o selo de uma única chamada.** O teste exige exatamente uma ocorrência executável de `codex … exec` no wrapper; adicionar uma segunda chamada inline para consolidar torna esse teste vermelho. `bin/tests/test_codex_sonda.sh:87`. Reutilizar o caminho comum evita essa colisão, mas a estratégia do contrato não está implementada. `bin/codex_pergunta.sh:157`.

- **Os testes existentes não certificam a transcrição proposta.** O selo estrutural cobre sandbox, diretório da cópia, teto, slot e cabeçalho; o comportamental cobre raspagem, falha fechada, procedência e papéis inválidos. `bin/tests/test_codex_pergunta.sh:13`, `bin/tests/test_codex_sonda.sh:57`, `bin/tests/test_codex_sonda.sh:90`. Validação `CLASSE`, caminhos, multiline, append concorrente, consolidação, integridade do par hash/thread e desligamento por A/B permanecem **[não verificado]**.

- **O tratamento de rc 75 existe, mas seu teste cobre apenas limite genérico.** O wrapper detecta os eventos e sai antes da captura de SID; o SID é extraído somente depois desse ramo. `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:175`. O selo verifica rc e número de linhas em `limite.log`, sem assegurar “nenhuma lição transcrita”. `bin/tests/test_codex_limite.sh:31`.

- **Background não elimina disputa por capacidade.** Todas as chamadas usam o mesmo gate e slots, mantidos até a saída; a geração de lição pode, portanto, ocupar slot que uma chamada posterior precisa. `bin/codex_pergunta.sh:138`, `bin/codex_pergunta.sh:153`. O timeout começa no `codex`, após a espera pelo slot. `bin/codex_pergunta.sh:157`.

## 31/10 11:32 · sessao 3488c57e5209 · - · - · origem=Ronald (tmux)

Achados materiais:

1. **Pasta de selos não garante execução no pre-push.** O loop `tests/test_*.sh` roda em `bin/regua.sh:138-145`. O próprio pre-push declara que esse loop não roda nele e chama alguns selos explicitamente (`bin/pre-push.sh:13-19`). A suite do arsenal é Django (`bin/pre-push.sh:155-184`). Portanto, só adicionar `test_agents_md_gerado.sh` à pasta deixa push direto sem essa guarda; a chamada deve verificar o commit enviado, cujo archive nasce em `bin/pre-push.sh:33-38`.

2. **O contexto consumido precisa estar commitado no hash, não apenas gerado no disco.** O wrapper extrai `git archive "$HASH"` e torna a cópia sem escrita (`bin/codex_pergunta.sh:91-100`); depois executa Codex nessa cópia (`bin/codex_pergunta.sh:157-158`). Gerar após o commit ou deixar `AGENTS.md` fora dele não atualiza o contexto dessa chamada.

3. **Cópia reutilizada não é conferida.** Se o diretório do hash já existe, o wrapper pula a criação inteira, sem comparar conteúdo com o archive (`bin/codex_pergunta.sh:91-101`). Logo, trocar o gerador não atualiza uma cópia existente do mesmo hash. A/B precisa de cópias isoladas e controle do conteúdo; o wrapper admite base alternativa por `CODEX_WS_BASE` (`bin/codex_pergunta.sh:87`).

4. **“Sem conduta” no AGENTS não impede conduta no prompt.** O wrapper concatena literalmente a pergunta (`bin/codex_pergunta.sh:118-135`) e aceita anexos, cuja transformação é raspagem de nomes/CPF, seguida de concatenação (`bin/codex_pergunta.sh:103-115`). Não há nessa montagem filtro de títulos/hash de conduta. Portanto, um selo só sobre AGENTS não prova ausência de conduta no contexto total recebido.

5. **Selos atuais não provam carregamento nem ausência de conduta no AGENTS.** O selo do wrapper verifica strings de forma, sandbox e `-C` (`bin/tests/test_codex_pergunta.sh:10-29`); sua mutação troca o sandbox (`bin/tests/test_codex_pergunta.sh:30-33`). O selo sonda usa Codex falso que só captura stdin (`bin/tests/test_codex_sonda.sh:21-25`), e a prova real existente mede ausência de escrita, não contexto carregado (`bin/tests/prova_codex_pergunta_real.sh:15-28`). É necessário distinguir “gerado conforme” de “conteúdo efetivamente recebido”.

Não apontaria rc 75 como conflito: ele é classificado depois da execução e devolvido sem regeneração de contexto (`bin/codex_pergunta.sh:163-171`). Tokens não entram em `chamadas.log`, que registra thread, rc e duração (`bin/codex_pergunta.sh:53-60`); a captura JSONL existe (`bin/codex_pergunta.sh:156-158`), mas a apuração do A/B prometido permanece [nao verificado].

## 31/10 11:32 · sessao 314427c48401 · - · - · origem=Ronald (tmux)

Achados de comparabilidade e falhas do contrato:

- **A captura muda mais rubricas que HE100 feriado e folga.** Passam a entrar os splits `horas_extras_50_noturna` e `horas_extras_100_noturna`; `horas_extras_100` recebe FT-feriado; `horas_trabalhadas` recebe FT sem escala e órfãos; noturnas e intra recebem todos os FT; atraso passa a usar crédito parcial. O laço atual não faz essas operações. `app/ponto/management/commands/diff_calculador.py:253-268`; `app/ponto/services/dia_pago.py:178-195`, `:223-270`.

- **Mesmo resultado não garante a mesma classificação do dia.** Fechamento classifica FT com `_certa` e `escala_vigente` na **data local da entrada**; lavra depois distribui pelo mapa `id(periodo) → jornada`. V3 decide escala certa, vínculo e feriado pelo **dia da jornada**. Há duas perguntas de data no caminho v2; incidência no dado de produção: [não verificado]. `app/ponto/services/fechamento.py:909-925`; `app/ponto/services/dia_pago.py:109-132`, `:234-258`; `app/ponto/calculador/alimentacao.py:460-511`.

- **Atraso também conserva calendário no v2.** `atraso_por_dia` usa a data local da entrada, e a lavra desconta o crédito nessa chave. V3 agrupa a pontualidade pelo mapa da jornada e não recebe o mapa de crédito parcial. `app/ponto/services/credito_parcial.py:15-44`; `app/ponto/services/dia_pago.py:190-195`; `app/ponto/calculador/alimentacao.py:227-228`, `:496-519`.

- **Os universos de dias diferem.** A lavra cria dias por períodos, atraso, órfãos, faltas, furos e grade; não contém filtro `ini <= dia <= fim`. V3 produz dias com minutos e ainda dias bloqueados pelo motor, com recorte da janela. `app/ponto/services/dia_pago.py:173-290`; `app/ponto/calculador/alimentacao.py:454-462`.

- **O DIFF descarta parte da resposta da porta v3.** Embora `rubricas_por_dia` inclua dias bloqueados sem segmento, o comando monta `c_dia` iterando somente `v.minutos`, não o retorno inteiro da porta. Assim, pode ignorar uma rubrica efetivamente devolvida; na união posterior, a ausência vira `{}` e então zero. A cegueira é registrada apenas para dias que passaram pelo laço. `app/ponto/management/commands/diff_calculador.py:417-431`, `:469-472`, `:501-535`; `app/ponto/calculador/alimentacao.py:454-458`.

- **A linha de ajuste exige tratamento explícito.** Ela nasce sempre, com `data=None`; guardar todas as linhas em `{dia: campos}` pode incluir `None` no `sorted` da união com datas. Filtrar somente rubricas de `alvo` não remove a linha. `app/ponto/services/dia_pago.py:346-356`; `app/ponto/management/commands/diff_calculador.py:527`.

- **Os universos de colaboradores não são iguais por construção.** O DIFF parte de IDs com `FechamentoMensal` já existente, depois aplica `--colab`/limite. A captura recalcula a empresa pelo `universo_da_competencia`. Também continua chamando `autoridade_do_periodo` e excluindo erros antes de consultar a captura. Portanto, `sem_lavra_do_fechamento == 0` não decorre do código. `app/ponto/management/commands/diff_calculador.py:97-108`, `:140-150`; `app/ponto/services/fechamento.py:538-541`.

- **Mesmo resultado não significa mesmos demais insumos.** O comando troca somente `aut.resultado`, conserva `aut.esc` e `aut.feriados`, e passa esses valores à porta. Fechamento usa `vinculo_de_referencia`; espelho usa vínculo no último dia da janela. A porta resolve CCT/família uma vez em `ini` com esse template e só depois ajusta template por dia. Possível efeito concreto na frota: [não verificado]. `app/ponto/management/commands/diff_calculador.py:151-163`, `:361`, `:417-423`; `app/ponto/services/fechamento.py:823-827`; `app/ponto/services/espelho.py:1069-1070`; `app/ponto/calculador/alimentacao.py:377-395`, `:476-486`.

- **Capturar primeira instância é dependência de ordem.** O hook atual guarda a primeira `ResultadoMes` por colaborador com `setdefault`; não identifica semanticamente o composto. O composto hoje nasce em `apurar_por_fatia` antes das chamadas por fatia, o que sustenta esse comportamento atual. `app/ponto/management/commands/diff_calculador.py:120-138`; `app/ponto/services/fechamento.py:182`, `:270`.

- **As duas lavras usam o mesmo nome local.** A chamada motor importa `_lavrar_dia_pago`, e a chamada oráculo reutiliza esse alias; o captador precisa ignorar versões diferentes de `motor`. Buscar a “real” pelo nome já substituído causaria recursão; a referência original precisa ser guardada antes do patch, como `_ol` hoje. `app/ponto/management/commands/diff_calculador.py:121-142`; `app/ponto/services/fechamento.py:1167-1169`, `:1195`, `:1233-1245`.

- **Falha de lavra não alcança automaticamente o comando.** Fechamento captura a exceção da chamada motor e apenas registra erro; o captador precisa contabilizá-la dentro da própria chamada e só publicar linhas completas após sucesso. Ausência, falha e chamada bem sucedida sem dias precisam permanecer distinguíveis; a lavra sempre devolve ao menos a linha de ajuste. `app/ponto/services/fechamento.py:1206-1209`; `app/ponto/services/dia_pago.py:346-359`.

_978 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
