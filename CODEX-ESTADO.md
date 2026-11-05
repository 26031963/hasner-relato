# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 16:26** · publicado em **05/11/2026 16:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 16:26 · sessao e6c483046b87 · - · - · origem=Ronald (tmux)

Na v4, os contraexemplos procedentes de transferência/identidade foram fechados: aplicar trava Empresa e Ausencia, compara identidade/minutos e lê a empresa atual sob trava de Colaborador antes de escrever; reverter segue a mesma ordem e verifica identidade, empresa, minutos e entrega. (`app/ponto/management/commands/rederivar_falta_declarada.py:180`, `app/ponto/management/commands/rederivar_falta_declarada.py:183`, `app/ponto/management/commands/rederivar_falta_declarada.py:189`, `app/ponto/management/commands/rederivar_falta_declarada.py:244`, `app/ponto/management/commands/rederivar_falta_declarada.py:255`.)

`ExportacaoDominio.registrar` toma a mesma trava antes de criar o registro; o arquivo de reversão permanece exclusivo, com abertura `x`. (`app/folha/models.py:179`, `app/folha/models.py:181`, `app/folha/models.py:186`, `app/ponto/management/commands/rederivar_falta_declarada.py:169`, `app/ponto/management/commands/rederivar_falta_declarada.py:172`.)

Há um sítio aberto verificável da classe **FERRAMENTA**: os testes continuam reconhecendo a trava por fragmentos SQL e ordem, sem conferir qual Empresa foi efetivamente travada. Um filtro por PK inexistente em `travar_entrega` ainda emitiria o SQL esperado pelos testes; é sobrevivente já descrito em `codex_v3.r`, item 4, “Mesma trava Empresa”. A sobrevivência é dedução estática; execução do mutante [nao verificado]. (`app/folha/export.py:565`, `app/ponto/tests/test_h06_reversao_guardas.py:249`, `app/ponto/tests/test_h06_reversao_guardas.py:350`, `app/ponto/tests/test_h06_reversao_guardas.py:357`.)

Existe também limite residual na **prova exclusiva do ato**, já mencionado em `codex_v2.r`, item 5, “não há estado de consumo do arquivo”: aplicar R1 720→420, reverter R1 420→720, aplicar R2 720→420 e reverter novamente R1 satisfaz as mesmas comparações. O arquivo é apenas lido, e o comando confere estado atual, sem consumir o arquivo anterior; prejuízo financeiro e ocorrência em produção [nao verificado]. Esse limite não deve entrar na contagem histórica como novo procedente. (`app/ponto/management/commands/rederivar_falta_declarada.py:87`, `app/ponto/management/commands/rederivar_falta_declarada.py:236`, `app/ponto/management/commands/rederivar_falta_declarada.py:255`, `app/ponto/management/commands/rederivar_falta_declarada.py:268`, `app/ponto/services/ausencia.py:531`.)

## 05/11 16:26 · sessao 90b85dbddf23 · - · - · origem=Ronald (tmux)

Achados no código, sem acesso a estado de produção:

- **Espera sem limite total:** o wrapper espera o gate com `flock 9` sem prazo e procura slot em laço; o `timeout` só começa no `codex exec`. Portanto a saída por falha de `contrato.txt:3` não cobre a espera anterior à execução. PD/AUT usam o mesmo conjunto de slots das chamadas de verificação, podendo atrasá-las; ocorrência real [nao verificado]. (`bin/codex_pergunta.sh:137`, `bin/codex_pergunta.sh:142`, `bin/codex_pergunta.sh:150`, `bin/codex_pergunta.sh:174`; `contrato.txt:1`, `contrato.txt:3`.)

- **Prazo de execução e prazo de resposta separados:** `CODEX_TIMEOUT` vale 900 s por padrão, é aplicado à trava da resposta e depois novamente ao processo Codex. A redação de falha não explicita esse tempo nem a espera de slot, apesar de o contrato de instrumento vigente pedir erro, timeout e limite. (`bin/codex_pergunta.sh:46`, `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:174`; `app/docs/PISTA.md:172`; `contrato.txt:3`.)

- **Construtor como quem espera:** `contrato.txt:2` põe o construtor como esperador, mas não diz como a espera ocorre. PISTA exige “quem espera e como”, sem agente ocioso ou turno vazio. Não é possível afirmar que haverá agente ocioso: [nao verificado]. (`contrato.txt:2`; `app/docs/PISTA.md:171`, `app/docs/PISTA.md:172`.)

- **Recusa antes de existir rc do wrapper:** o disjuntor pode negar consultas Codex de qualquer papel por orçamento; sem `CODEX_VOLTA`, o parser assume `meio`. O contrato prevê rc 75 ou “falha do Codex”, mas não explicita o tratamento de negativa do hook, em que a consulta nem executa. Isso é ponto de espera indefinida se a sessão insistir em obter a autópsia antes da próxima volta; insistência real [nao verificado]. (`bin/disjuntor.py:1336`, `bin/disjuntor.py:1373`, `bin/disjuntor.py:1383`; `contrato.txt:1`, `contrato.txt:2`, `contrato.txt:3`.)

- **Censo não libera o verificador/B3:** só `papel=verificador` consulta B3 no wrapper; a triagem formal também rejeita `papel=censo`. Assim a resposta AUT/PD não produz liberação de B3 nem pode ser contada automaticamente como volta formal de verificação. O contrato já diz que não substitui verificador, portanto não há violação demonstrada, mas seria incorreto tomar a autópsia como liberação. (`bin/codex_pergunta.sh:164`; `bin/disjuntor.py:798`; `contrato.txt:1`, `contrato.txt:3`.)

- **Pouso já aprovado:** a salvaguarda está explícita, sem bloqueio novo demonstrado pelo texto. Atraso indireto continua possível pelos slots compartilhados; aconteceu ou afetou pouso concreto [nao verificado]. (`contrato.txt:2`; `bin/codex_pergunta.sh:137`, `bin/codex_pergunta.sh:143`.)

- **Escopo e prioridade:** contrato preserva literalmente “fila não se reordena”, mas não repete teto 2 nem a proteção nominal de F3/H12+H06 do aval anterior. Isso é omissão documental, não prova de mudança da fila. (`contrato.txt:2`; `avais.txt:1`.)

- **Regra vigente preservada:** a reprovação final já para a obra na primeira ocorrência; autópsia na segunda reprovação não pode ser lida como permissão para avançar ignorando a primeira. O contrato não declara essa permissão. (`CLAUDE.md:271`, `CLAUDE.md:272`; `bin/disjuntor.py:76`; `contrato.txt:1`; `avais.txt:2`.)

## 05/11 16:26 · sessao febf73418c30 · - · - · origem=Ronald (tmux)

- **Lei específica já responde (b):** “chamado de cluster NAO fecha por idade; so morre por lastro”; fechamento por janela é dívida. (`app/docs/BACKLOG.md:424-426`)
- **Juiz da premissa:** `_C3` aponta para `premissa_morta`; `avaliar` conserva fato ausente/`None`, conserva verdadeiro e declara morta a premissa somente para valor falso coletado. (`app/core/juizes.py:263`; `app/chamados/catalogo/premissa.py:115-150`)
- **Assinatura de cluster ainda não tem juiz único declarado:** `SEM_JUIZ['batida']` registra que `detectar_cluster_espurio --apply` e o cluster-guard de `escala/utils.py` decidem em sítios diferentes. Isso é pergunta distinta de “a premissa morreu?”. (`app/core/juizes.py:109`; `app/core/juizes.py:1184-1186`)
- **Transição, vida e identidade temporal:** registro aponta `VIVOS`, `decidir`, `data_do_chamado` e `competencia_do_chamado`. (`app/core/juizes.py:261-275`)
- **Dado desconhecido não é regularização:** `_celula_furada` retorna `None` sem célula e sem `lampadas` na ata; a premissa conserva `None`. (`app/chamados/juizes.py:457-462`; `app/chamados/juizes.py:471-476`; `app/chamados/catalogo/premissa.py:127-130`)
- **Dia sem trabalho:** juiz registrado `_C18`; `trabalha=False` cala, `trabalha=None` isoladamente cobra, ata com tipo preenchido diferente de trabalho cala, ressalvada ausência parcial que cobra o resto. (`app/core/juizes.py:136-149`; `app/core/juizes.py:278`; `app/chamados/juizes.py:398-426`; `app/docs/CORTES.md:416`)
- **Competência 21→20:** leis L-026 e L-090 exigem corte da empresa, nunca mês civil; `janela_fechamento` e `janela_atual` leem `corte_da_empresa`. (`app/docs/LEIS.md:77`; `app/docs/LEIS.md:123`; `app/ponto/janelas.py:36-63`)
- **Tranca é fato próprio:** `dia_em_competencia_trancada` consulta `PeriodoFechado` não reaberto e testa a data dentro da janela, inclusive bordas; `competencia_do_chamado` toma o fato mais recente, incluindo perguntas vivas. (`app/chamados/juizes.py:1701-1719`; `app/chamados/juizes.py:1978-2000`)
- **Tranca possui exceções concretas:** o serviço preserva resgatados posteriores ao fechamento e chamados com perguntas vivas preservadas; competência apenas passada sem `PeriodoFechado` não produz seleção. (`app/chamados/services/competencia_trancada.py:82-105`; `app/chamados/services/competencia_trancada.py:135-146`)
- **Cross-meia-noite:** registro distingue pertencimento da batida ao turno, marco ocupado e cruzamento previsto; `marcos_turno` encadeia os instantes e avança um dia quando o próximo horário é menor ou igual ao anterior; início igual ao fim representa 24h. (`app/core/juizes.py:196-204`; `app/escala/servico_jornada.py:61-86`; `app/escala/servico_jornada.py:96-103`)
- **Vínculo trocado:** juiz registrado `vinculo_do_dia`; geradora da célula manda dentro da vigência, falta da geradora carregada ou data fora de vigência devolve `None`; sem geradora escolhe pelos vínculos que cobrem a data. (`app/core/juizes.py:1285-1286`; `app/escala/alimentacao.py:269-286`)
- **Lei transversal:** consumidor lê célula/ata; L-061 não equipara transição do chamado ao julgamento do fato. (`app/docs/LEIS.md:95`)
- **A matriz continua declaradamente parcial pela O169:** nota exige O168, O169 e LASTRO e proíbe tornar a célula verde por mera reclassificação do papel do sítio. (`app/core/contratos_estruturais.py:260-269`)
- **Autoridade do documento:** `JUIZES.md` é lookup e o código vivo manda; o próprio lookup ainda declara SLA/disputa sem juiz enquanto o registro vivo já contém autoridades. (`app/docs/JUIZES.md:8-15`; `app/docs/JUIZES.md:51-54`; `app/core/juizes.py:267-269`)
- **Medição real de 30 dias, RED executado de 15 dias e estado atual de produção:** [não verificado].

## 05/11 16:26 · sessao e57de376397d · - · - · origem=Ronald (tmux)

Achados literais dos anexos; referências usam a numeração das cinco linhas do contrato e das duas linhas dos avais. (contrato.txt:1; avais.txt:2)

1. **Itens do aval sem cobertura explícita:** o contrato não enumera os cinco recortes do censo — escreve/lê, juiz/lei, contraexemplo, RED e irmãos — nem as três classes da autópsia — implementação/desenho/ferramenta. `CODEX_PAPEL=censo` identifica o papel, mas não declara esses conteúdos. (avais.txt:2; contrato.txt:1)
   Também não repete “piloto, não regra”, “nenhuma lei nova” e “CLAUDE.md intocado”; ausência textual não prova que houve mudança de lei ou arquivo. (avais.txt:2; contrato.txt:1; contrato.txt:2; contrato.txt:3; contrato.txt:4; contrato.txt:5)

2. **Desvio efetivo declarado:** O169 recebe o PD depois de o construtor já estar aberto, embora o aval exija o censo antes do construtor. O próprio contrato chama isso de desvio; autorização posterior para essa exceção não consta do dado. (contrato.txt:5; avais.txt:2)
   **Acréscimos sem previsão expressa no aval:** seguir sem PD em falha; proibir código na resposta e aplicar “sem desenho” indistintamente às duas peças; atribuir ao Code o desligamento e ao chat a decisão posterior. São cláusulas adicionais; sua simples existência não demonstra lei nova. (contrato.txt:3; contrato.txt:5; avais.txt:2)

3. **Espera criada:** a peça 1 antecede a abertura do construtor e a peça 2 antecede a próxima volta; há dependência de uma resposta do instrumento nesses dois pontos. O contrato libera em falha, mas não estabelece prazo de espera até a falha ser declarada. (contrato.txt:1; contrato.txt:2; contrato.txt:3)
   O contrato protege expressamente pouso já aprovado, ordem da fila e autoridade do verificador/opus final. Bloqueio real desses três por esse instrumento: [nao verificado]. (contrato.txt:2; contrato.txt:3) [nao verificado]

4. **Medidas sem fonte nomeada:** voltas Codex e opus até aprovação; tokens do construtor; tokens do PD; classificação “item útil / reprovação já prevista / reprovação nova”. A linha da medida não identifica arquivo ou log de origem nem ligação entre os registros por obra. (contrato.txt:4)
   A régua aponta apenas “obras de 05/11 já no RELATO”, sem selecionar as obras ou localizar seus registros de medida. A tabela final tem destino declarado — RELATO —, mas isso não identifica a origem dos valores. (contrato.txt:4; contrato.txt:5)

## 05/11 16:25 · sessao 40ff844da7b0 · - · - · origem=Ronald (tmux)

Passo 2 mantém o juiz registrado e o universo estático correto, mas introduz conservação temporal dos insumos entre dias. O registro aponta `minutos_previstos_do_dia`; a grade o chama com células já carregadas e vínculos carregados uma única vez. (app/core/juizes.py:173; app/escala/services/leitor_celula.py:350,389–395)

1. **Contraexemplo concreto: vínculo encerrado durante a grade.** col01 tem EC iniciada em 01/07/2026, ativa, sem fim; células de 04 e 05/08/2026 trabalham, DNA 08:00–17:00 com pausa 12:00–13:00, ata sem previsto. A grade percorre do fim para o início e julga 05/08, conservando EC na lista. Durante a chamada, `fechar_vigencia(EC, 03/08/2026, autor=admin)` confirma `ativa=False/data_fim=03/08`. Ao julgar 04/08, o novo caminho usa EC aberta conservada e produz **480**; o anterior recarrega EC, a geradora fica fora da vigência e produz **0**. (app/escala/services/leitor_celula.py:359–360,389–395; app/colaboradores/services/vinculo.py:561,612–625; app/escala/utils.py:2201–2204,2282–2295,2326–2341; app/escala/alimentacao.py:279–283) Execução e ocorrência: [nao verificado].

2. **Contraexemplo concreto: DNA regenerado durante a grade.** A grade carrega célula com DNA 08:00–17:00/12:00–13:00; antes de calcular o dia, a porta real regenera seu DNA para 08:00–18:00/12:00–13:00. Novo caminho usa a célula conservada e produz **480**; anterior relia célula no juiz e produziria **540**, mantendo ata sem previsto nas condições do cenário. (app/escala/services/leitor_celula.py:350,382–395; app/ponto/portas/celula.py:243–265; app/escala/utils.py:2158–2160,2279–2280,2290–2295,2312,2326–2341; app/escala/models.py:1124–1133,1196–1201) Execução e ocorrência: [nao verificado].

3. **Universos estáticos.** Geradora fora da janela continua carregada porque a consulta não restringe datas; fora da vigência retorna `None`; com múltiplos vínculos a geradora manda; sem geradora ocorre desempate por `(ativa, data_inicio)`; lista vazia produz zero. (app/escala/services/leitor_celula.py:391–392; app/escala/alimentacao.py:269–286; app/escala/utils.py:2277–2278) Dia sem célula retorna `sem_celula/0` antes do juiz; eventual diferença contra o avulso é preexistente ao diff fornecido. (app/escala/services/leitor_celula.py:373–378; diff_v3_p2.patch:[nao verificado])

4. **Escritores e leitores.**

   | Insumo | Escritores | Leitores |
   |---|---|---|
   | `celulas`, chaves `(colab.pk, data)` | Grade transforma sua carga em mapa. (app/escala/services/leitor_celula.py:350,393) | Juiz do vínculo, trabalho, marcos e pausas. (app/escala/utils.py:2280–2290,2312; app/escala/alimentacao.py:269–270; app/escala/models.py:1087–1096,1117–1133,1182–1208) |
   | Colaborador/data/trabalha/geradora/DNA da célula | Porta cria, regenera, restaura ou atualiza. (app/ponto/portas/celula.py:247–248,259–265,538–546,601–604,618–623) | Grade e autoridades acima. (app/escala/services/leitor_celula.py:350–362,389–395; app/escala/alimentacao.py:269–283) |
   | DNA marcos/pausas | Produtor congela os valores do template. (app/ponto/management/commands/gerar_celulas.py:73–103) | EC lê marcos e todas as pausas; previsto calcula duração líquida. (app/escala/models.py:1124–1133,1189–1208; app/escala/utils.py:2290–2341) |
   | `escalas` | Grade carrega lista inteira. (app/escala/services/leitor_celula.py:389–392) | `_esc_vigente_do_dia` entrega lista ao juiz; vazio significa nenhum vínculo. (app/escala/utils.py:2175–2187,2200–2215,2277–2283) |
   | EC: colaborador/template/início/fim/ativa | Porta cria ou atualiza; fechamento; correção retroativa. (app/colaboradores/services/vinculo.py:265–288,612–625,909–925) | Juiz seleciona geradora/vigência ou desempata; EC entrega trabalho/marcos/pausas. (app/escala/alimentacao.py:205–216,269–286; app/escala/models.py:1087–1208) |
   | Template de horários/pausas | Cadastro escreve e salva, podendo propagar regeneração. (app/escala/services/cadastro_tipo.py:223–235,264–266,306,334–350) | Fallback dos marcos/pausas da EC. (app/escala/models.py:326–345,1133,1208) |
   | `ata.minutos_previstos` | Cartório produz; porta persiste ata. (app/ponto/services/cartorio.py:696–705; app/ponto/portas/celula.py:735–751,775–783) | Grade prefere esse valor e só chama juiz se ausente. (app/escala/services/leitor_celula.py:382–395) |

5. **Selos.** O teste de desempenho morde a reversão do passo 2 por inspeção; o teste de valor passaria com o passo 2 revertido, porque compara o mesmo juiz em cenário estático. (app/escala/tests/test_snc2_grade_previsto_em_lote.py:83–109) A fixture sempre cria célula e geradora, usa vínculos adjacentes e não intercala escritores: não cobre sem célula, sem geradora, ausência de vínculos, sobreposição simultânea ou mudança durante a grade. (app/escala/tests/test_snc2_grade_previsto_em_lote.py:53–80,96–109) Execução dos testes/mutantes: [nao verificado].

6. **Alcance real.** Leitores incluem folha, espelho, relatórios, calendário, calculadores, furos, suprajuiz, worklist, declaração, emissão e materialização. (app/folha/export.py:327; app/ponto/services/espelho.py:519; app/relatorios/furos.py:132,194; app/colaboradores/services/calendario.py:334; app/inteligencia/calculadores.py:42,143,332; app/escala/services/furos_diarios.py:43; app/ponto/management/commands/supra_juiz.py:129; app/ponto/worklist.py:18; app/chamados/services/declaracao_texto.py:82; app/chamados/services/disputa_emissao.py:66; app/chamados/services/materializacao.py:233,282)

**Conclusão do subescopo: REPROVA pela divergência temporal demonstrável dos itens 1 e 2; produção e execução permanecem [nao verificado].**

## 05/11 16:25 · sessao 96f0544b3ad5 · - · - · origem=Ronald (tmux)

A cura da tranca entre colaboradores está presente: `dia_da_pergunta_trancado` usa `_memo_da_tranca(colab, lote=False)`, preservando dono request/colaborador para empresa e tranca; somente toque SIM e usuário sistema continuam usando o lote. (app/chamados/juizes.py:940–954, app/chamados/juizes.py:966–979, app/chamados/juizes.py:1043–1049, app/chamados/juizes.py:1068–1072)

O contraexemplo da célula antecipada permanece no diff total, e o descarte da triagem não sustenta a equivalência exigida: os carimbos da pergunta já serem antigos nos dois caminhos não impede uma célula fresca no caminho anterior produzir resposta diferente da célula carregada antecipadamente no novo. (app/chamados/services/pergunta_no_app.py:46–53, app/chamados/juizes.py:801, app/chamados/juizes.py:1135–1196)

1. **Contraexemplo concreto remanescente.** Competência aberta; P2 sem validação, resposta ou materialização; célula C com `veredito='furo'`. A lista de `perguntas_do_app` carrega P2 e C, mas P2 ainda não foi julgada. Durante o lote, o admin confirma Resolver `trabalhou`; o serviço fecha perguntas por instâncias próprias e lavra o veredito de C. P2 da lista conserva os carimbos antigos em ambos caminhos, mas no novo conserva também C=`furo`, retornando `True`; no anterior fornecido, `_chave_memo_pv` carregaria C depois do ato, com `trabalhou`, e sem marcos pendentes retornaria `False`. (app/chamados/services/pergunta_no_app.py:46–53, app/ponto/services/veredito_celula.py:174–178, app/ponto/services/veredito_celula.py:325–336, app/chamados/juizes.py:801, app/chamados/juizes.py:1181–1196)

   Portanto, alegar que a célula passou a compartilhar a fotografia dos carimbos não demonstra “mesmo veredito”; o exemplo usa exatamente os mesmos carimbos antigos nas duas execuções e muda apenas o instante da leitura da célula. (app/chamados/juizes.py:1135–1196, app/chamados/services/pergunta_no_app.py:46–53)

2. **Memo: escritores e leitores.**

   | Estado | Escritor | Leitor/dono |
   |---|---|---|
   | `_tranca_memo` | `_memo_da_tranca`, por `setdefault`; teste remove explicitamente. (app/chamados/juizes.py:979, app/ponto/tests/test_selo_chokepoint_escrita.py:417) | Request atual; sem request, colaborador. (app/chamados/juizes.py:975–979) |
   | `('empresa', empresa_id)` | `dia_da_pergunta_trancado` guarda `colab.empresa`. (app/chamados/juizes.py:941–943) | A mesma função passa a empresa a `janela_atual`; sempre fora do dono lote. (app/chamados/juizes.py:940–944) |
   | `('tranca', empresa_id, mês, ano)` | A mesma função guarda `periodo_trancado`, inclusive `None`. (app/chamados/juizes.py:945–947) | A mesma função lê presença e `fechado_em`; sempre fora do dono lote. (app/chamados/juizes.py:948–954) |
   | `('toque', colab.pk, data, desde)` | `_admin_tocou_o_dia_depois` grava somente `True`. (app/chamados/juizes.py:1044–1049) | A mesma função retorna SIM guardado; dentro do lote, dono lote; fora, request/colaborador. (app/chamados/juizes.py:972–979, app/chamados/juizes.py:1043–1047) |
   | `('sistema',)` | `_toque_humano_no_dia` guarda PK de `get_user_sistema`. (app/chamados/juizes.py:1068–1070) | A mesma função exclui esse usuário da trilha humana; compartilha o dono do memo de toque. (app/chamados/juizes.py:1071–1072) |
   | `LOTE_DA_TRANCA` | Declaração e context manager `set/reset`. (app/chamados/juizes.py:990–1003) | `_memo_da_tranca`, guarda reentrante e testes do comando. (app/chamados/juizes.py:972–974, app/chamados/juizes.py:996, app/chamados/tests/test_snc2_toque_em_lote.py:208–213) |
   | `REQUISICAO_DA_TRILHA` | Middleware instala/restaura; teste instala/restaura. (app/core/middleware.py:76–82, app/chamados/tests/test_snc2_toque_em_lote.py:235–243) | Escolha do dono fora do lote e usuário/IP da trilha. (app/chamados/juizes.py:975–979, app/core/observ.py:108–118) |

3. **Fontes, escritores e leitores fora do diff.**

   | Fonte | Escrita | Leitura |
   |---|---|---|
   | Empresa do colaborador | Edição atribui `empresa_id`; transferência atribui empresa do posto e salva pela porta. (app/colaboradores/views.py:1313, app/colaboradores/views.py:1383–1388, app/colaboradores/services/colaborador.py:73–76) | C6 lê empresa/ID para classificar competência. (app/chamados/juizes.py:932–945) |
   | Corte da empresa | Formulário recebe corte e chama `atualizar_empresa`, que salva. (app/colaboradores/views.py:496–499, app/colaboradores/views.py:521–523, app/colaboradores/services/estrutura.py:17–20) | `janela_atual` lê corte, classifica o mês e retorna o período. (app/ponto/janelas.py:24–33, app/ponto/janelas.py:50–63) |
   | Tranca/reabertura | `trancar_periodo` cria ou retranca; `reabrir_periodo` grava `reaberto_em`. (app/ponto/services/fechamento.py:1392–1403, app/ponto/services/fechamento.py:1421–1425) | `periodo_trancado` seleciona somente linha com `reaberto_em` nulo. (app/ponto/services/fechamento.py:1448–1454) |
   | Usuário sistema | Helper usa `get_or_create`; comando garantidor cria/normaliza. (app/core/utils.py:18, app/core/management/commands/garantir_user_sistema.py:13–41) | Exclusão do autor sistema na trilha humana. (app/chamados/juizes.py:1068–1072) |
   | Trilha: usuário, ação, modelo, objeto, descrição e tempo | `registrar_log` adapta para `evento`; `evento` cria; `criado_em` automático. (app/core/auditoria.py:22–24, app/core/observ.py:134–138, app/core/models.py:295–307) | Filtro estritamente posterior e atos/vínculos do dia. (app/chamados/juizes.py:1071–1084) |
   | Cobrar | Grava ação, colaborador e `dia=ISO` antes do cartório. (app/chamados/services/cobrar_dia.py:29–39) | Primeiro `exists`. (app/chamados/juizes.py:1073–1075) |
   | Resolver | Serviço e porta da célula gravam ação `veredito_celula`. (app/ponto/services/veredito_celula.py:329–336, app/ponto/portas/celula.py:770–789) | Busca trilha nas células atuais do dia, sem filtrar `via`. (app/chamados/juizes.py:1076–1080) |
   | Desarquivar | Porta grava estado e chama `_trilha`→`evento`. (app/chamados/services/arquivo.py:193–209, app/chamados/services/arquivo.py:130–138) | Reconstrói chamados atuais ligados às células do colaborador/dia. (app/chamados/juizes.py:1081–1084) |
   | Identidade da pergunta | `PerguntaDisputa.abrir` recebe disputa/data, busca célula e cria. (app/chamados/models.py:2387–2419, app/chamados/models.py:2465–2466) | C6 lê disputa/colaborador/data e célula. (app/chamados/juizes.py:931–954, app/chamados/juizes.py:1147–1196) |
   | Carimbo da pergunta | `_escrever_carimbo`; Resolver chama `fechar_por_admin` em instâncias consultadas por ele. (app/chamados/models.py:2545–2549, app/chamados/models.py:2497–2512, app/ponto/services/veredito_celula.py:174–178) | Primeiros degraus e identidade no C6. (app/chamados/juizes.py:1135–1172) |

4. **Monotonicidade do SIM por vínculos.** Não há suporte para afirmar que `apontar_celula` remove ou substitui o vínculo: ela recusa `None` e retorna sem escrever quando já existe célula. Assim, o cenário de desfazer SIM por reapontamento nessa porta não está demonstrado. (app/chamados/services/vinculos.py:73–78, app/chamados/services/vinculos.py:92–100)

   `LogAuditoria.usuario` admite `SET_NULL` em exclusão do usuário, mas um escritor concreto sobrevivente que realize essa exclusão durante o lote não foi demonstrado nesta auditoria; usar isso como defeito confirmado seria [nao verificado]. (app/core/models.py:295)

5. **Mordida dos REDs v3.** Reabrir/trancar usam outro colaborador e portas reais, discriminando a tranca compartilhada do v2; retirada integral do lote do comando não os afetaria, pois abrem `_lote()` diretamente. (app/chamados/tests/test_snc2_toque_em_lote.py:171–198)

   O teste do comando observa somente `censo`; morde retirada integral do contexto, mas não retirada de `censo_alcance` ou dos três contadores do contexto. (app/chamados/tests/test_snc2_toque_em_lote.py:200–213, app/chamados/management/commands/vigiar_fio_morto.py:171–176)

   Os testes fora de lote/requisição posterior seguem guardas que poderiam passar com a cura v3 revertida; não exercitam reabertura, tranca nem corte. (app/chamados/tests/test_snc2_toque_em_lote.py:215–243)

6. **Classes adicionais.** O contexto só instala um dicionário e restaura o token: não estabelece transação nem trava contra atos externos. O comando captura `agora/hoje`, mas isso não alimenta cada dependência interna de C6. (app/chamados/juizes.py:993–1003, app/chamados/management/commands/vigiar_fio_morto.py:159–176)

A regra exige levar descartes ao opus, suíte inteira verde antes da leitura final e revisão de escritores/leitores fora do diff; o relato fornecido de suítes parciais não permite comprovar a suíte inteira da árvore. (CLAUDE.md:368–373; execução integral: [nao verificado]) [nao verificado]

## 05/11 16:24 · sessao 4d5958956d51 · - · - · origem=Ronald (tmux)

Censo estático de travas explícitas:

- **Aplicar:** `atomic` → Empresa por `travar_entrega` → consulta `Ausencia.select_for_update` → consulta `Colaborador.select_for_update`, ordenada por PK → UPDATE Ausencia pela porta; rechecagem de entrega é SELECT simples. (`app/ponto/management/commands/rederivar_falta_declarada.py:180`, `:181`, `:182`, `:189`, `:199`, `:210`; `app/folha/export.py:565`; `app/ponto/management/commands/rederivar_falta_declarada.py:56`; `app/ponto/services/ausencia.py:508`.)
- **Reverter:** `atomic` → Empresas em PK crescente → consulta Ausencia com `select_for_update` → Colaboradores em PK crescente → SELECT entrega → UPDATE Ausencia pela reposição. (`app/ponto/management/commands/rederivar_falta_declarada.py:244`, `:245`, `:247`, `:248`, `:263`, `:269`; `app/ponto/services/ausencia.py:532`.)
- **Ressalva decisiva:** a consulta Ausencia não restringe `of=('self',)` nem substitui a ordenação padrão, que contém `colaborador__nome`; portanto a declaração “só Ausencia antes de Colaborador” não está provada. O SQL e a ordem física entre linhas/tabelas do JOIN são **[nao verificado]**. (`app/ponto/models.py:819`; `app/ponto/management/commands/rederivar_falta_declarada.py:182`, `:247`.)
- **FechamentoMensal:** `foto` apenas consulta; não adiciona trava `FOR UPDATE` nem escrita de fechamento a esses dois caminhos. (`app/ponto/management/commands/carimbo_gravado.py:53`; `app/ponto/management/commands/rederivar_falta_declarada.py:195`, `:213`.)

Outros escritores confirmados com duas dessas tabelas na mesma transação:

1. **ExportacaoDominio.registrar:** Empresa `FOR UPDATE` → INSERT ExportacaoDominio. (`app/folha/models.py:179`, `:181`, `:186`.)
2. **Aprovação de irmãos:** atomic → UPDATE Ausencia em `mudar_estado` → UPDATE FechamentoMensal em `invalidar_fechamento`; repete A→F no laço, mantendo as travas anteriores até o commit. (`app/ponto/views.py:2638`, `:2640`; `app/ponto/services/ausencia.py:915`, `:839`, `:950`; `app/ponto/services/fechamento.py:1635`, `:1640`.)

Correções para evitar falsos positivos:

- **Desligamento não comprova C→F via signal:** grava Colaborador e chama `fechar_vinculos_ativos`, mas este usa `QuerySet.update`, sem `post_save` de escala. (`app/colaboradores/services/desligamento.py:103`, `:108`, `:123`; `app/colaboradores/services/vinculo.py:816`, `:615`.)
- **Transferência não chama fechamento pelo histórico:** Colaborador→HistoricoVinculo; a porta do histórico apenas faz `create`. (`app/colaboradores/views.py:1384`, `:1388`, `:1393`; `app/colaboradores/services/historico_vinculo.py:25`.)
- **Gerar TXT não demonstra F→Empresa na mesma transação:** aprovação ocorre antes de `registrar`, sem atomic externo identificado nessa view. (`app/folha/views.py:252`, `:297`; `app/folha/models.py:179`.)
- **Validação não demonstra C→A:** seu atomic agenda o recálculo por evento e materializa hora; processamento posterior da disputa começa fora daquele atomic. (`app/chamados/services/validacao.py:395`, `:407`, `:424`, `:430`.)

Não demonstrei ciclo concreto entre os caminhos explicitamente confirmados. **Exaustividade transitiva de todos os writers/sinais e deadlock efetivo: [nao verificado].** Uma eventual aresta Colaborador→Empresa por verificação de FK na transferência depende do SQL/constraint efetiva e deve ser apresentada como inferência **[nao verificado]**, não como deadlock provado. (`app/colaboradores/views.py:1386`, `:1388`.)

## 05/11 16:24 · sessao 228de9519217 · - · - · origem=Ronald (tmux)

Conclusões por inspeção; execução dos testes, mutantes e logs: [nao verificado]. [nao verificado]

| Selo atual | Cura v1 revertida: volta ao toque plural/NÃO de 144c7fac | Cura v3 revertida: empresa/tranca novamente no lote de 76626871 | Comando sem lote |
|---|---|---|---|
| Custo usuário/trilha | Falharia pelo crescimento exigido de trilha, incompatível com coleta única. (app/chamados/tests/test_snc2_toque_em_lote.py:102–111) | Passaria: não intercala escrita de empresa/tranca. (app/chamados/tests/test_snc2_toque_em_lote.py:102–111) | Passaria: abre `_lote()` diretamente. (app/chamados/tests/test_snc2_toque_em_lote.py:87–98) |
| Paridade estática | Passaria: todos os atos/vínculos precedem o julgamento. (app/chamados/tests/test_snc2_toque_em_lote.py:113–135) | Passaria pelo mesmo motivo. (app/chamados/tests/test_snc2_toque_em_lote.py:113–135) | Passaria: helper abre seu lote. (app/chamados/tests/test_snc2_toque_em_lote.py:82–85) |
| Cobrar durante lote | Falharia: introduz toque depois de aquecer. (app/chamados/tests/test_snc2_toque_em_lote.py:137–149) | Passaria: não muda empresa/tranca. (app/chamados/tests/test_snc2_toque_em_lote.py:137–149) | Passaria: lote direto. (app/chamados/tests/test_snc2_toque_em_lote.py:142) |
| Chamado ganha célula | Falharia pelo conjunto anterior de vínculos. A fixture muda a FK diretamente, sem a porta. (app/chamados/tests/test_snc2_toque_em_lote.py:151–164) | Passaria: não muda empresa/tranca. (app/chamados/tests/test_snc2_toque_em_lote.py:151–164) | Passaria: lote direto. (app/chamados/tests/test_snc2_toque_em_lote.py:160) |
| Reabrir durante lote | Falharia quando a tranca permanecer no lote. (app/chamados/tests/test_snc2_toque_em_lote.py:171–184) | Falharia: P2 de outro colaborador continuaria morta. (app/chamados/tests/test_snc2_toque_em_lote.py:176–184) | Passaria: lote direto. (app/chamados/tests/test_snc2_toque_em_lote.py:178) |
| Trancar durante lote | Falharia quando `None` permanecer no lote. (app/chamados/tests/test_snc2_toque_em_lote.py:186–198) | Falharia: P2 de outro colaborador continuaria viva. (app/chamados/tests/test_snc2_toque_em_lote.py:191–198) | Passaria: lote direto. (app/chamados/tests/test_snc2_toque_em_lote.py:194) |
| Comando no lote | Passaria: contexto já integra os pais fornecidos. (app/chamados/tests/test_snc2_toque_em_lote.py:200–213; diff_v3_total.patch fornecido: [nao verificado] para histórico Git independente) | Passaria. (app/chamados/tests/test_snc2_toque_em_lote.py:200–213) | Falharia: observa presença no `censo`. (app/chamados/tests/test_snc2_toque_em_lote.py:205–213) |
| Guarda por instância | Passaria: não abre lote nem request. (app/chamados/tests/test_snc2_toque_em_lote.py:215–223) | Passaria. (app/chamados/tests/test_snc2_toque_em_lote.py:215–223) | Passaria. (app/chamados/tests/test_snc2_toque_em_lote.py:215–223) |
| Request posterior | Passaria: o request é instalado depois de terminar o lote. (app/chamados/tests/test_snc2_toque_em_lote.py:225–243) | Passaria: nenhum ato muda empresa/tranca. (app/chamados/tests/test_snc2_toque_em_lote.py:225–243) | Passaria. (app/chamados/tests/test_snc2_toque_em_lote.py:225–243) |

O selo do comando não observa `censo_alcance`, `viva_em_disputa_fechada`, `respondida_sem_resposta` ou `materializada_viva`: esses consumidores poderiam sair do contexto mantendo a observação atual verde. (app/chamados/tests/test_snc2_toque_em_lote.py:205–212; app/chamados/management/commands/vigiar_fio_morto.py:171–176)

No passo 2, a paridade de valor passaria com a otimização revertida porque compara a grade ao mesmo juiz avulso; o teste de custo é quem distingue a alimentação nova. A fixture não contém célula sem geradora, dia sem célula ou intercalação de escrita durante a chamada. (app/escala/tests/test_snc2_grade_previsto_em_lote.py:59–80, app/escala/tests/test_snc2_grade_previsto_em_lote.py:89–109)

Triagem fornecida, linha a linha:

- Cabeçalho: identidade histórica conferida no commit não pode ser confirmada pela cópia sem metadados Git. [nao verificado] [nao verificado]
- Item 1: curado o compartilhamento novo entre colaboradores: empresa/tranca agora usam `lote=False`; os dois REDs passam pelas portas reais e distinguem o pai fornecido. (app/chamados/juizes.py:937–948; app/chamados/tests/test_snc2_toque_em_lote.py:171–198)
- Item 2: a mesma mudança retira empresa do lote, portanto cura o prolongamento entre colaboradores frescos; não há RED próprio de alteração do corte no arquivo novo. (app/chamados/juizes.py:940–944; app/chamados/tests/test_snc2_toque_em_lote.py:102–243)
- Item 3: **descarte não se sustenta**. Carimbos da pergunta já antigos não tornam indiferente o instante de leitura da célula. Resolver fecha perguntas e grava célula; a pergunta já carregada conserva seus carimbos em ambos os caminhos. Antes, sua célula lazy podia ser carregada depois do ato por `_chave_memo_pv`, vendo `trabalhou`; agora a célula veio no snapshot anterior e pode continuar `furo`, dando resultado oposto. (app/ponto/services/veredito_celula.py:324–330; app/chamados/services/pergunta_no_app.py:46–53; app/chamados/juizes.py:801, app/chamados/juizes.py:1181–1196)
- Item 4: sustentado enquanto distinção entre RED e guarda: reabrir/trancar mordem a cura nova; paridade estática e request posterior não a distinguem. (app/chamados/tests/test_snc2_toque_em_lote.py:113–135, app/chamados/tests/test_snc2_toque_em_lote.py:171–198, app/chamados/tests/test_snc2_toque_em_lote.py:225–243)
- Item 5: existência das dependências é demonstrável; “herdado” não as exclui da revisão. `get_user_sistema` pode criar usuário; C6 ainda lê relógio interno; a decisão fila/bloqueio cabe ao opus com resposta e triagem anexadas. (app/core/utils.py:18; app/chamados/juizes.py:562, app/chamados/juizes.py:1197; CLAUDE.md:368–373) Suíte inteira lançada/concluída e sua árvore certificada: [nao verificado].

Classes adicionais:

- C6 e previsto continuam apontando aos endereços registrados. (app/core/juizes.py:266; app/core/juizes.py:173)
- O comando captura `agora/hoje`, mas `_dentro_da_janela_viva` continua lendo `date.today()` dentro de C6. (app/chamados/management/commands/vigiar_fio_morto.py:159–164; app/chamados/juizes.py:562, app/chamados/juizes.py:1197)
- O contexto apenas instala/restaura dicionário, sem estabelecer transação ou trava contra escritores externos. (app/chamados/juizes.py:993–1003)
- A regra exige suíte inteira verde antes da leitura final; também exige manter a árvore certificada até pousar e apagar sítio órfão no mesmo marco. Cumprimento operacional: [nao verificado]. (CLAUDE.md:317–328, CLAUDE.md:369–373) [nao verificado]
- Há chamadores reais dos sítios novos: contexto no comando, helper de toque no C6 e grade em produção. (app/chamados/management/commands/vigiar_fio_morto.py:171; app/chamados/juizes.py:1047; app/ponto/worklist.py:18; app/relatorios/furos.py:132; app/inteligencia/calculadores.py:42)

## 05/11 16:23 · sessao bfa532940f3c · CONFIG-RAIZ · leitura · origem=Code

Referências `anexo/...:N` numeram o conteúdo de cada anexo a partir de 1.

**1. Pousos com B3 aberto**

Pelo horário registrado em `pousos.txt`, há uma ocorrência positiva, com ressalva sobre a reescrita posterior do hash. (`anexo/pousos.txt:30`; `anexo/disjuntor.log:32–33`; `app/docs/RELATO.md:6197–6204`.)

| Pouso: hash e hora | Obra | B3 no horário registrado | Fonte | [nao verificado]
|---|---|---|---|
| `97bcb01c`, 02/11 04:14:23 −0300 | `COBERTURA-PARCIAL-UM-JUIZ`, marco `COBPARC-H10` | Disparo **02:53:56**; **aberto às 04:14**; liberação **04:24:42** | `anexo/pousos.txt:30`; `anexo/disjuntor.log:32–33`; `app/docs/RELATO.md:6197,6202` |

A ressalva é material: o RELATO identifica o merge original como `b5e69ac7`, registra sua reescrita para `97bcb01c` às 04:41, com árvore idêntica, e o push às 05:06. Portanto, o **pouso da árvore relatado às 04:14** ocorreu com B3 aberto; o timestamp herdado não demonstra que o hash reescrito `97bcb01c` já existia naquele instante. Hora exata da reescrita: **não medido**. (`app/docs/RELATO.md:6198,6203–6204`; `anexo/disjuntor.log:33`.)

“Libera o BLOQUEIA” às 03:15 e o APROVADO às 03:23 não equivalem à liberação do disjuntor: o código não contém liberação automática por aprovação posterior; exige a transição específica abaixo. (`app/docs/RELATO.md:6194–6196`; `bin/disjuntor.py:896–943`.)

A mecânica no código da cópia é:

| Transição | Regra |
|---|---|
| **Dispara** | Uma reprovação de verificador em leitura final ou três voltas Codex verificador triadas PROCEDENTE. A coleta exige `agentType=verificador`, prompt reconhecido como final e veredito reconhecido no `SubagentHandback`. (`bin/disjuntor.py:76–77,546–568,736–749,909–921`.) |
| **Libera** | Nas duas primeiras linhas úteis do topo: `DESENHO <obra>` com lei aceita, pergunta e referência existente, ou `ERRO-BOBO <obra>` com motivo. A obra deve estar no cabeçalho; SHA não pode estar consumido nem fotografado no disparo; `mtime >= disparado_em`. O passe que acaba de disparar não libera. (`bin/hook_stop_fila1.py:415–443`; `bin/disjuntor.py:626–654,912–945`.) |
| **Estado após liberar** | Move IDs para `liberado`, esvazia `reprovados`/`codex`, grava `disparado=false` e consome o SHA. Reprovação nova pode disparar novamente. (`bin/disjuntor.py:896–912,936–940`.) |
| **Rearma** | `--rearma-b3` arquiva em `rearmado` as reprovações e voltas já triadas e zera contagens/disparo; requer triagem legível e gravação bem sucedida. (`bin/disjuntor.py:1003–1033`.) |
| **Migração histórica** | Reprovações antigas já guardadas são arquivadas sem disparo retroativo. A trilha registra essa migração às 16:32:12 de 31/10; o primeiro disparo anexado, às 08:35, registrava quatro reprovações. (`bin/disjuntor.py:693–710`; `anexo/disjuntor.log:1,3–9`.) |

Exclusões e limites relevantes:

| Caso | Resultado do cruzamento |
|---|---|
| F5C `a2246c0e`, 31/10 05:13:24 | O disparo da obra mãe aparece somente às 08:35:25. As quatro reprovações eram anteriores ao pouso; não cabe retroagir o disparo. B3 naquele pouso: **não medido**. (`app/docs/RELATO.md:471,476`; `anexo/disjuntor.log:1`.) |
| F1C4-3-NONE-COBRA `4f771457`, 31/10 09:19:26 | Há reprovação relatada, mas a trilha anexada registra apenas MIGRA, arquivando uma reprovação às 16:32. B3 aberto no pouso: **não medido**. (`app/docs/RELATO.md:473,480`; `anexo/disjuntor.log:3`; `bin/disjuntor.py:693–710`.) |
| COBPARC-FILA-ROTULO `e1c08457`, 02/11 06:38:34 | Disparo registrado às 07:08:00, depois do timestamp do pouso; liberação às 07:09:24. Não entra como aberto às 06:38. (`anexo/pousos.txt:29`; `anexo/disjuntor.log:34–35`; `app/docs/RELATO.md:6206–6213`.) |
| Cobertura anterior ao primeiro disparo anexado | **Não medido** pela trilha fornecida. O RELATO-ARQUIVO descreve arquivamento de conteúdo anterior ao recorte e não acrescenta medição B3 utilizável aqui. (`anexo/disjuntor.log:1`; `app/docs/RELATO-ARQUIVO.md:42–46`.) |

**2. Mesmo Stop executado duas vezes**

Os comandos Stop de projeto e usuário são iguais. O script aproveita somente `transcript_path` do payload e não guarda identidade de evento: uma segunda invocação refaz a decisão usando o estado deixado pela primeira. O comportamento de deduplicação do Code permanece **[não verificado]**. (`.claude/settings.json:3–13`; `anexo/user_settings.json:3–13`; `bin/hook_stop_fila1.py:1184–1192`.)

| Estado lido/escrito | Persistência e efeito da segunda execução |
|---|---|
| **BACKLOG e RELATO** | Relê marcador, itens, PAREI e ESPERANDO; o checador de prova precede todas as portas. O Stop não escreve linhas no RELATO: solicita linhas pelo `reason`; o teto registra alarme na trilha. (`bin/hook_stop_fila1.py:170–239,400–443,957–959,972–973,1174–1181,1198–1203,1284–1293`.) |
| **Base de afirmações** `bin/tests/afirma_sem_prova_base.txt` | O checador lê a base; se estiver ausente, cria-a e retorna zero. Após essa criação, a segunda execução lê a base existente. (`bin/relato_afirma_com_prova.py:54,62–77,94–111`.) |
| **Trilha** `logs/hook_stop_fila1.log` | Apende data, decisão, motivo, item e HEAD em cada registro, inclusive ALLOW. Ao alcançar a fila, escreve `BLOCK fila 1 com item aberto` antes de verificar teto/B2. A segunda execução acrescenta outra avaliação. (`bin/hook_stop_fila1.py:40–53,123–133,1249–1250`.) |
| **Teto** `logs/hook_stop_fila1.json` | Lê/grava `{head,blocks,item}`. Mesmo HEAD, sem trabalho em curso: incrementa novamente. HEAD diferente começa em 1; contador ilegível começa vazio. A condição efetiva é **`n > 5`**, portanto libera no sexto bloqueio contado. Duas execuções podem consumir duas unidades no mesmo evento. (`bin/hook_stop_fila1.py:1255–1293`.) |
| **Suite/push em curso** | Consulta `ps -eo args`, reconhecendo `manage.py test` e `git push`. Nesse ramo preserva `blocks` — ou usa 1 —, não aplica teto e não chama B2; ainda registra o BLOCK da fila. (`bin/hook_stop_fila1.py:373–397,1249,1272–1295`.) |
| **Espera por arquivo** `logs/hook_stop_espera.json` | Guarda, por caminho real/dispositivo/inode, o maior `mtime_ns` observado; usa `.lock`, temporário `.hook_stop_espera.*` e substituição do JSON. Primeira observação nunca prova; uma observação com avanço prova e grava o avanço. A segunda execução, sem novo avanço, responde **“arquivo nao andou”** e perde essa porta de liberação. (`bin/hook_stop_fila1.py:563–642,1231–1235`.) |
| **Memo da espera** `_ESPERA_MEMO` | Evita duas medições dentro do mesmo processo, compartilhando resultado entre orçamento e main. Não persiste entre duas invocações do comando. (`bin/hook_stop_fila1.py:685–697,1309–1310`.) |
| **Espera por PID** | Relê `/proc`: ancestrais, `stat`, executável, argumentos e identidade do processo. Não grava memória consumível de PID; a primeira execução não invalida por si essa prova. (`bin/hook_stop_fila1.py:474–550`.) |
| **Arquivos protegidos da prova** | RELATO, BACKLOG, estados/trilhas do hook e orçamento, pisos, disjuntor, locks e cota são rejeitados como `arquivo=`, inclusive por hardlink. As próprias escritas do hook nesses arquivos não fabricam avanço válido. (`bin/hook_stop_fila1.py:583–600`.) |
| **Handoff e guarda de CLEAR** | Lê `mtime` de `HANDOFF-SESSAO.md`, consulta `logs/sombra.lock` e agentes de fundo sem entrega. Não gera handoff. A segunda execução compara essas provas com as horas já persistidas. (`bin/hook_stop_fila1.py:778–795,913–943,967–973`.) |
| **Entradas do orçamento** | Lê transcript, `logs/sessao/ultimo_clear.json`, `logs/deploy/historico.log` e `logs/sessao/pisos.log`. Mede contexto, marco e piso do ciclo. (`bin/orcamento_contexto.py:57–65,89–232,321–345`.) |
| **Piso** `logs/sessao/pisos.log` | O primeiro Stop que consegue medir o primeiro turno do ciclo apende o piso e consome `piso_pendente`; a segunda execução não repete esse registro quando o estado foi salvo. (`bin/hook_stop_fila1.py:891–907`; `bin/orcamento_contexto.py:198–207`.) |
| **Orçamento** `logs/sessao/orcamento_estado.json` e `orcamento.log` | Usa `.lock`, temporário `.orcamento_estado.*`, substituição do JSON e eventos após salvar. Guarda marco, desligamento, pendência, última medida e disparo/flags. A segunda execução atualiza a medida/hora e encontra as transições já registradas. (`bin/hook_stop_fila1.py:729–769,798–907`.) |
| **PREPARA-CLEAR** | Primeiro disparo grava `disparo` e emite `siga`; o seguinte não redispara, seguindo o caminho normal com lembrete. “Sem handoff”, adiamento e fim de marco também têm flags que impedem repetir o mesmo `siga` especial. (`bin/hook_stop_fila1.py:918–943,964–1006`.) |
| **Janela CLEAR** | O primeiro par válido grava `clear_parei_desde`; a segunda execução não reinicia o prazo. O prazo efetivo é **10 minutos**. Expirado, grava `desfecho=seguiu`, pede retirar a linha e zera o teto. (`bin/orcamento_contexto.py:56`; `bin/hook_stop_fila1.py:944–959,1067–1073,1220–1223`.) |
| **Chave** `DISJUNTOR_DESLIGADO` | Sua existência é consultada nas chamadas contidas do disjuntor; presente, desativa B4/B2/recados nesse caminho. (`bin/hook_stop_fila1.py:1034–1040`; `bin/disjuntor.py:144–146`.) |
| **Escada/B4** | Lê cota e última mensagem humana; grava `b4_para.desde` em `disjuntor_estado.json`. A primeira execução cria a escada e emite `siga` exclusivo; a segunda encontra o estado e recebe **lembrete**, podendo alcançar orçamento/fila/teto/B2. PAREI COTA com handoff suficientemente novo libera; sem escada remove `b4_para`. (`bin/disjuntor.py:1064–1117,1202–1222`; `bin/hook_stop_fila1.py:1206–1214`.) |
| **B2** | Conta as linhas BLOCK da fila na última hora, incluindo a recém-escrita; consulta commits e mtimes de arquivos sujos nas worktrees, com exclusões para docs/logs/duração dos crons. Guarda `b2.ultimo` e limita o anúncio a uma vez por hora, mas continua retornando liberação quando a condição vale. (`bin/disjuntor.py:70–75,1135–1197`.) |
| **B3 nos recados** | Lê estado, `logs/codex/triagem.log`, transcripts/meta dos verificadores e topo+mtime do RELATO; pode disparar/liberar e salvar. A mesma reprovação não conta duas vezes; IDs liberados/rearmados são ignorados. A linha fotografada no primeiro disparo não libera a segunda execução. (`bin/hook_stop_fila1.py:1047–1052,1300`; `bin/disjuntor.py:736–750,823–965`.) |
| **B1 nos recados** | Lê disparados persistidos e cruza com agentes vivos sem entrega; o Stop não faz nova medição B1. Sem mudança nas fontes, repete o pedido de TaskStop. (`bin/hook_stop_fila1.py:1053–1057`; `bin/disjuntor.py:489–490`.) |
| **Persistência do disjuntor** | Usa `disjuntor_estado.json.lock`, temporário `.disjuntor_estado.*`, substituição do estado e `disjuntor.log`. Pode regravar estado idêntico; anúncios dependem das transições. (`bin/disjuntor.py:237–288,1202–1222`.) |

Dois efeitos merecem destaque:

- Se já existiam duas linhas BLOCK da fila na hora, a primeira avaliação pode ser a terceira e a segunda a quarta: **a segunda pode liberar por B2**, caso chegue a esse ramo e não haja commit/arquivo novo nem DIFF/apply. A contagem é de avaliações registradas, não de `siga` efetivamente exibidos. (`bin/hook_stop_fila1.py:1249,1284–1297`; `bin/disjuntor.py:1168–1197`.)
- A primeira execução pode provar `ESPERANDO | arquivo=...`, registrar gatilho como PENDENTE e liberar; a segunda, sem avanço do mtime, perde a prova e pode transformar a pendência em PREPARA-CLEAR. (`bin/hook_stop_fila1.py:620–642,979–1006,1231–1235`.)

Qual desses ramos ocorreria no evento concreto: **não medido**, pois os estados de teto, espera e orçamento e o transcript desse evento não foram fornecidos. (`bin/hook_stop_fila1.py:35–36,461,710–711,1187–1190`.)

**3. O que o PreToolUse negaria na virada**

O JSON anexado não sustenta uma lista concreta de agentes negados: contém B3, mas não contém medições B1 nem cota. Além disso, o PreToolUse mede ou consulta suas fontes novamente. (`anexo/disjuntor_estado.json:1`; `bin/disjuntor.py:464–486,1064–1076,1426–1475`.)

| Regra/alvo | Resultado sustentado pelo dado e regra de recusa |
|---|---|
| **B1, por agente** | Agentes concretos: **não medido**. Com `agent_id`, nega ferramentas exceto `SubagentHandback` quando o agente dispara B1; suspende B1 durante DIFF/apply. A medição exige cópia própria citada, idade de pelo menos 15 minutos, tokens nos últimos 15 minutos e nenhum arquivo novo nas cópias nesse intervalo. Tipos de leitura são isentos; medida desconhecida não dispara. (`bin/disjuntor.py:68–69,90–93,413–486,1434–1439`.) |
| **B3, por obra** | **Cada obra presente no objeto `b3` está com `disparado=false` e `reprovados=[]`.** F1C4-3-EMISSORES conserva uma volta Codex, abaixo do limiar três. Logo, o retrato não contém obra já disparada. (`anexo/disjuntor_estado.json:1`; `bin/disjuntor.py:76–77,909–912`.) |
| **B3 na decisão efetiva** | Nega novo `Agent` do tipo `verificador` em leitura final da obra parada; também a chamada reconhecida de `codex_pergunta.sh` como verificador daquela obra. Não é uma proibição geral de ferramentas/construtores da obra. Antes de decidir, varre as fontes novamente. (`bin/disjuntor.py:1400–1407,1440–1459`.) |
| **B3 e fonte desconhecida** | O anexo registra `b3_triagem_linhas=1`; portanto a fonte precisa preservar a linha já lida. Triagem ausente após leitura, truncada, menor ou ilegível gera NAO_SEI; no ramo B3, NAO_SEI **recusa o verificador**. Situação real dessa fonte na virada: **não medido**. (`anexo/disjuntor_estado.json:1`; `bin/disjuntor.py:823–854,986–1000,1400–1407`.) |
| **B4, qualquer obra/modelo** | Cota concreta: **não medido**. Cota válida ≥75% nega todo `Agent` novo; ≥85% ou última mensagem humana começando por “escada” também nega todo novo Agent. A decisão não seleciona obra/modelo e não bloqueia SendMessage por esse ramo. (`bin/disjuntor.py:85–87,1064–1130,1451–1475`.) |
| **Guarda da chave** | Independentemente do estado anexado, nega Bash contendo `DISJUNTOR_DESLIG` ou `--rearma-b3`; também qualquer ferramenta cujo `file_path`/`notebook_path` tenha basename começando por `DISJUNTOR_DESLIG`. Vale inclusive com o disjuntor desligado. (`bin/disjuntor.py:152–165,1486–1488`.) |
| **Guarda dos settings** | Em `.claude/settings.json` ou `.claude/settings.local.json`, nega Write sem o literal `disjuntor.py pretool`; nega Edit/MultiEdit quando uma substituição contém `disjuntor.py` no texto antigo e não no novo. Essa é a comparação implementada. (`bin/disjuntor.py:149,166–175`.) |

A existência de `DISJUNTOR_DESLIGADO` desativa as decisões normais, preservando a guarda da chave. Sua existência no host da virada: **não medido**. Sem `agent_id`, fora de Agent e de Bash com `codex_pergunta`, `pretool()` retorna antes de I/O; a guarda já foi aplicada pelo chamador. (`bin/disjuntor.py:144–146,1430–1432,1486–1488`.)

Além dos quatro itens pedidos, o código também contém recusas de ANEXO-ENXUTO e B4-CODEX para chamadas reconhecidas ao wrapper; orçamento/cota Codex concretos: **não medido** pelos anexos. (`bin/disjuntor.py:1325–1389,1440–1450`.)

**4. Compact: contexto, marco e PreCompact**

O matcher `compact` declara `cat HANDOFF-SESSAO.md` e `marco_de_clear.py compact`; o matcher `clear` declara os equivalentes com argumento `clear`. PreCompact manual e automático declaram o gerador do handoff. (`.claude/settings.json:14–60`.)

| Comando | O que imprime/grava |
|---|---|
| **`cat HANDOFF-SESSAO.md`** | O arquivo da cópia mede **54 linhas / 2.834 bytes**. Por execução do `cat`, esse é o conteúdo emitido. A fotografia armazenada está datada de **05/11 10:32**; seus HEAD, fila e travas são os valores escritos naquela fotografia. (`app/docs/HANDOFF-SESSAO.md:1–54`; `.claude/settings.json:40`.) |
| **Teto do handoff** | **60 linhas**, sem teto de bytes implementado. Acima disso, guarda as primeiras 59 linhas e acrescenta aviso como linha 60; pode cortar a seção final. (`bin/handoff_sessao.sh:22,293–302`.) |
| **`marco_de_clear.py compact` — saída** | Não imprime nada; absorve exceções e retorna zero. (`bin/marco_de_clear.py:10–11,23–66`.) |
| **Marco — conteúdo** | Lê JSON do stdin; payload inválido vira `{}`. Grava `logs/sessao/ultimo_clear.json` com `hora`, `iso`, `origem="compact"`, `session_id`, `source` e `clear_horas`. Conserva os últimos 20 horários de clear anteriores; compact não acrescenta seu próprio horário nessa lista. (`bin/marco_de_clear.py:19–20,25–57`.) |
| **Marco — persistência** | Cria diretório e `.lock`, toma flock exclusivo, lê a hora sob trava, escreve temporário `.ultimo_clear.*` e substitui o JSON. (`bin/marco_de_clear.py:36–63`.) |
| **Efeito posterior no Stop** | O novo marco encerra o ciclo anterior de orçamento; o Stop passa a registrar o piso do ciclo novo quando consegue medi-lo. Essa transição acontece no Stop, não por impressão do marco. (`bin/hook_stop_fila1.py:862–904`.) |

O **PreCompact gera uma fotografia nova** em `app/docs/HANDOFF-SESSAO.md`, usando temporário fora do repositório e capturando stdout/stderr do bloco gerador. (`bin/handoff_sessao.sh:17–27,291–300`.) Ele:

- Coleta HEAD, executa **`git fetch -q`**, lê origin/main, commits a empurrar e até oito linhas de status. (`bin/handoff_sessao.sh:30–41`.)
- Consulta o hook para ORDEM VIVA, item em curso e vereditos de PAREI; lê perguntas do topo do RELATO, imprimindo até quatro com corte de 220 caracteres. (`bin/handoff_sessao.sh:44–125`.)
- Cruza PROMPTS do dia com BACKLOG, mostra até seis avais recentes não cumpridos, lê placar estrutural, raias e último aval COTA. (`bin/handoff_sessao.sh:128–211`.)
- Consulta as travas de teste e sombra por `flock`, e o último deploy pelo stamp. (`bin/handoff_sessao.sh:221–240`.)
- Executa `gerar_avais.py`, que lê `PENDENTES_RONALD.json` e **reescreve `AVAIS.md`**. Essa chamada não usa `--escrever`, portanto não grava a normalização no JSON. (`bin/handoff_sessao.sh:243–250`; `bin/gerar_avais.py:29–31,94–108`.)
- Acrescenta lista travada quando aplicável, próximo passo, aplica o teto e imprime `handoff_sessao: <caminho> (<N> linhas, teto 60)`. (`bin/handoff_sessao.sh:255–302`.)

Assim, no par SessionStart(compact), a saída textual declarada é o HANDOFF inteiro; o marco acrescenta **zero texto** e atualiza o estado do ciclo. A incorporação efetiva pelo runtime e a ativação dos hooks na sessão concreta permanecem **[não verificado]**. (`.claude/settings.json:34–46`; `bin/marco_de_clear.py:10–11,23–66`.)

## 05/11 16:23 · sessao c52d76bb4d40 · - · - · origem=Ronald (tmux)

Análise estática; não executei testes ou mutantes. Resultados executados além do RED fornecido: [nao verificado]. [nao verificado]

| Teste novo | Mordida demonstrável pelo código | Mutante de produção que deixa este teste verde | [nao verificado]
|---|---|---|
| Aplicar/empresa | A mudança é injetada depois de `alvo`; retirar a reconferência permite prosseguir e viola o `assertRaises`. (`app/ponto/tests/test_h06_reversao_guardas.py:293`, `app/ponto/tests/test_h06_reversao_guardas.py:301`, `app/ponto/management/commands/rederivar_falta_declarada.py:189`.) | Retirar apenas `select_for_update` da leitura de Colaborador, mantendo a consulta e comparação: o caso é serial. (`app/ponto/management/commands/rederivar_falta_declarada.py:56`, `app/ponto/tests/test_h06_reversao_guardas.py:305`.) |
| Aplicar/status | **Não isola a guarda de identidade.** (`app/ponto/tests/test_h06_reversao_guardas.py:309`, `app/ponto/tests/test_h06_reversao_guardas.py:313`.) | **Remover `status` de `CAMPOS_DO_ATO`.** A edição zera minutos; `save` não deriva com status `pendente`; a comparação `aus.minutos != g` levanta outro `CommandError` e o rollback preserva exatamente `('pendente',720)`. (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `app/ponto/services/ausencia.py:507`, `app/ponto/models.py:877`, `app/ponto/management/commands/rederivar_falta_declarada.py:202`.) |
| Aplicar/data_fim | Morde remover `data_fim` da identidade: a edição reconstrói o fim a partir de `dias_corridos=1`, deriva420 e deixa de lançar a exceção exigida. (`app/ponto/tests/test_h06_reversao_guardas.py:315`, `app/ponto/services/ausencia.py:477`, `app/ponto/services/ausencia.py:499`, `app/ponto/management/commands/rederivar_falta_declarada.py:202`.) | Retirar a trava de Ausencia preservando a leitura: caso serial. (`app/ponto/management/commands/rederivar_falta_declarada.py:182`, `app/ponto/tests/test_h06_reversao_guardas.py:318`.) |
| Reverter/status | Morde retirar `status` da identidade: restariam empresa/minutos/entrega válidos, permitindo a reposição em vez da exceção. (`app/ponto/tests/test_h06_reversao_guardas.py:322`, `app/ponto/management/commands/rederivar_falta_declarada.py:255`, `app/ponto/management/commands/rederivar_falta_declarada.py:269`.) | Retirar a trava de Colaborador mantendo a consulta: o teste altera status em execução serial. (`app/ponto/management/commands/rederivar_falta_declarada.py:56`, `app/ponto/tests/test_h06_reversao_guardas.py:325`.) |
| Reverter/data_fim | Morde retirar `data_fim` da identidade; as duas datas continuam na mesma competência aberta, permitindo a reposição. (`app/ponto/tests/test_h06_reversao_guardas.py:331`, `app/ponto/management/commands/rederivar_falta_declarada.py:255`, `app/ponto/management/commands/rederivar_falta_declarada.py:263`.) | Retirar a trava de Colaborador mantendo a consulta: caso serial. (`app/ponto/management/commands/rederivar_falta_declarada.py:56`, `app/ponto/tests/test_h06_reversao_guardas.py:334`.) |
| Ordem aplicar | Morde ausência/inversão dos SQLs reconhecidos e escrita anterior ao primeiro SQL de Colaborador. (`app/ponto/tests/test_h06_reversao_guardas.py:350`, `app/ponto/tests/test_h06_reversao_guardas.py:360`, `app/ponto/tests/test_h06_reversao_guardas.py:365`.) | Em `_empresa_de_hoje`, travar `pk=-1` e depois ler os colaboradores reais **sem trava**. O teste reconhece somente tabela/`FOR UPDATE`, sem conferir parâmetros ou linhas efetivamente travadas. (`app/ponto/management/commands/rederivar_falta_declarada.py:56`, `app/ponto/tests/test_h06_reversao_guardas.py:352`.) |
| Ordem reverter | Mesma mordida de SQL, agora pelo caminho da volta. (`app/ponto/tests/test_h06_reversao_guardas.py:369`, `app/ponto/tests/test_h06_reversao_guardas.py:373`.) | Mesmo mutante de trava vazia seguido de leitura real sem trava. (`app/ponto/management/commands/rederivar_falta_declarada.py:56`, `app/ponto/tests/test_h06_reversao_guardas.py:352`.) |
| Contador da tela | Morde restaurar o contador por presença de data: a fixture tem duas datas, mas espera1. Morde também zerar o contador. (`app/ponto/tests/test_h06_reversao_guardas.py:169`, `app/ponto/tests/test_h06_reversao_guardas.py:170`, `app/ponto/tests/test_h06_reversao_guardas.py:387`, `app/ponto/views.py:1215`.) | Fixar `n_rescisao=1`; ou clonar o predicado na view/produtor em vez de consultar a autoridade. O teste verifica o número desse cenário, não a proveniência da decisão. (`app/ponto/views.py:1215`, `app/ponto/services/fechamento_lista.py:109`, `app/ponto/tests/test_h06_reversao_guardas.py:387`.) |

O RED fornecido com três falhas é compatível com a existência prévia das guardas de identidade e da correção do contador; teste já verde antes desta cura não é, sozinho, prova de ausência de mordida. O caso aplicar/status tem, entretanto, o falso positivo concreto acima. (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `app/ponto/management/commands/rederivar_falta_declarada.py:184`, `app/ponto/views.py:1215`; execução independente do RED: [nao verificado].)

Quanto aos descartes:

- **D1, invalidar:** a justificativa de monotonicidade é compatível com o código. `invalidar` somente preenche invalidação; o juiz exige vigente sem `invalidada_em`. Não cria uma entrega nem desinvalida registro. (`app/folha/models.py:154`, `app/folha/models.py:158`, `app/folha/models.py:162`, `app/folha/export.py:593`, `app/folha/export.py:595`.)
- **D2:** afirmar inexistência de deadlock a partir da ausência de prova anterior não resolve a nova ordem Colaborador/Ausencia; a conferência global pertence ao censo do outro agente. Estado final: [nao verificado].
- **D3:** 480 e rateio uniforme permanecem visíveis; formatos antigos são recusados pela lista de chaves exigidas. O aval consultado aparta explicitamente MODO-COMUM, mas não contém dispensa explícita de compatibilidade desses arquivos. Existência de arquivos afetados em produção: [nao verificado]. (`app/ponto/turnos.py:2580`, `app/ponto/turnos.py:2598`, `app/folha/export.py:121`, `app/ponto/management/commands/rederivar_falta_declarada.py:239`, `app/docs/PROMPTS.md:1861`.)
- **D4:** **falsa a justificativa “TOCTOU não tem caso determinístico”.** O teste já fixa o nome completo por `_selo`/PID/UUID; um wrapper de `open` pode criar a sentinela imediatamente antes de delegar ao `open` real. A implementação atual `x` recusaria; o mutante `exists`→`open(w)` truncaria, pois seu `exists` já teria retornado falso. Não exige dois processos nem janela de nanossegundo. Este teste proposto não foi executado: [nao verificado]. (`app/ponto/tests/test_h06_reversao_guardas.py:233`, `app/ponto/tests/test_h06_reversao_guardas.py:236`, `app/ponto/management/commands/rederivar_falta_declarada.py:172`.)

Sobre os achados anteriores, a corrida específica da empresa foi corrigida no código: ambos os caminhos consultam Colaborador sob `select_for_update` e recusam empresa diferente antes de editar/repor. Isso não prova ordem global segura, que depende do censo de outros escritores. (`app/ponto/management/commands/rederivar_falta_declarada.py:56`, `app/ponto/management/commands/rederivar_falta_declarada.py:189`, `app/ponto/management/commands/rederivar_falta_declarada.py:193`, `app/ponto/management/commands/rederivar_falta_declarada.py:248`, `app/ponto/management/commands/rederivar_falta_declarada.py:258`.)

## 05/11 16:23 · sessao 5c5c48b03a62 · - · - · origem=Ronald (tmux)

Censo estático dos escritores e leitores de produção relevantes ao delta; execução, dados adicionais e exaustividade por SQL dinâmico: [nao verificado].

**Colaborador**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `empresa_id` | Porta persistente: `gravar`, `salvar_campos`, `salvar_inteiro`, `atualizar_em_lote`, `criar_colaborador` (`app/colaboradores/services/colaborador.py:55`, `:65`, `:73`, `:80`, `:91`). Valores vêm da admissão (`app/colaboradores/views.py:1182`, `:1186`), edição geral (`app/colaboradores/views.py:1313`, `:1342`), transferência de posto cross-CNPJ (`app/colaboradores/views.py:1384`, `:1386`, `:1388`). `salvar_inteiro` também persiste empresa antiga em memória mesmo quando o chamador só mudou outro campo: salva objeto inteiro, sem conferir nomes (`app/colaboradores/services/colaborador.py:73`, `:75`, `:76`; exemplo `app/colaboradores/views.py:327`, `:329`). | Censo do ato filtra empresa (`app/ponto/management/commands/rederivar_falta_declarada.py:87`); nova leitura protegida retorna empresa por colaborador (`:56`, `:57`); aplicar compara em `:190`, `:191`; reverter compara em `:258`. Lista usa empresa para janela e chave de cache (`app/ponto/services/fechamento_lista.py:103`, `:104`, `:109`). Universo/competência lê empresa (`app/ponto/services/fechamento.py:423`, `:428`, `:435`); carimbo filtra empresa atual do colaborador (`app/ponto/management/commands/carimbo_gravado.py:53`). |
| `situacao` | Default `ativo` (`app/colaboradores/models.py:323`); admissão explícita (`app/colaboradores/views.py:1205`); desligamento (`app/colaboradores/services/desligamento.py:104`, `:108`); reativação (`:210`, `:214`); lançamento INSS (`app/ponto/views.py:2430`, `:2432`); reversor de afastado (`app/ponto/management/commands/reverter_situacao_afastado.py:73`). Todos delegam à porta (`app/colaboradores/services/colaborador.py:32`, `:61`, `:76`). | Universo de fechamento (`app/ponto/services/fechamento.py:402`, `:423`); exportação/âncora filtra ativo (`app/folha/export.py:814`); vínculo recusa desligado (`app/colaboradores/services/vinculo.py:89`); adesão recusa desligado (`app/chamados/services/adesao.py:66`). |
| `data_demissao` | Nullable definido no modelo (`app/colaboradores/models.py:325`); agendamento salva data sem desligar (`app/colaboradores/services/desligamento.py:82`, `:85`, `:86`); desligamento efetivo atribui e salva (`:105`, `:108`); reativação limpa (`:211`, `:214`). Cron seleciona agendados vencidos e chama o serviço (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `:39`, `:51`). | Predicado compartilhado do delta (`app/folha/export.py:60`), exportação chama (`app/folha/export.py:870`), lista chama (`app/ponto/services/fechamento_lista.py:109`), contador lê `rescisao` (`app/ponto/views.py:1215`), template lê booleano e usa data no texto (`app/templates/ponto/_fech_linha.html:2`, `:4`). Universo histórico lê limite inferior (`app/ponto/services/fechamento.py:423`, `:429`); exportação recorta cálculo pela data (`app/folha/export.py:1091`, `:1092`, `:1212`, `:1213`); célula lê admissão/demissão (`app/escala/services/leitor_celula.py:351`). |

A operação `cadastro` não enumera `empresa`, mas isso **não impede** a edição de empresa: `salvar_inteiro` confere somente a operação e chama `save()` inteiro; portanto omitir esse escritor seria um censo falso (`app/colaboradores/services/colaborador.py:34`, `:73`, `:75`, `:76`; `app/colaboradores/views.py:1313`, `:1342`).

Admin de Colaborador é somente leitura, portanto não é escritor atual (`app/colaboradores/admin.py:48`, `:66`, `:71`).

**Ausencia**

| Campo | Quem escreve/deriva | Quem lê no ato e leitores relevantes |
|---|---|---|
| `colaborador_id` | Criação monta kwargs e salva (`app/ponto/services/ausencia.py:342`, `:385`, `:418`). FK é protegida (`app/ponto/models.py:743`, `:744`). Não encontrei atribuição posterior nos escritores da porta examinados; exaustividade dinâmica [nao verificado]. | Identidade (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `:48`), trava/leitura de empresa (`:189`, `:248`), comparação reversão (`:255`), cobertura (`app/ponto/turnos.py:2529`, `:2552`). |
| `tipo` | Criação (`app/ponto/services/ausencia.py:342`); edição (`:497`, `:508`); troca dispara rejulgamento do status (`:512`, `:513`). | Censo `falta` (`app/ponto/management/commands/rederivar_falta_declarada.py:87`), identidade (`:44`, `:184`, `:255`), derivação do modelo (`app/ponto/models.py:877`), cobertura/parcialidade (`app/ponto/turnos.py:2598`), efeito (`app/ponto/models.py:842`), financeiro (`app/ponto/services/fechamento.py:665`, `:685`). |
| `data` | Criação (`app/ponto/services/ausencia.py:342`); edição (`:498`, `:508`). | Censo/janela/soma (`app/ponto/management/commands/rederivar_falta_declarada.py:89`, `:93`, `:95`), identidade (`:44`, `:184`, `:255`), competências tocadas (`:68`), cobertura/expansão (`app/folha/export.py:70`, `:88`, `:90`), créditos (`app/ponto/services/credito_parcial.py:24`, `:25`). |
| `data_fim` | Derivador da porta (`app/ponto/services/ausencia.py:66`, `:477`), criação (`:343`), edição (`:499`, `:508`), completamento do modelo para dias>1 (`app/ponto/models.py:902`, `:907`), mudança de estado persiste as derivações (`app/ponto/services/ausencia.py:838`, `:839`), reparo `rederivar_fim` chama save (`:1297`, `:1300`). | Identidade (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `:184`, `:255`), censo/range (`:93`), competências (`:68`), modelo soma até fim (`app/ponto/models.py:882`), expansão (`app/folha/export.py:70`), parcialidade (`app/ponto/turnos.py:2599`). |
| `status` | Default (`app/ponto/models.py:799`, `:800`); criação recebe argumento ou `estado_inicial` (`app/ponto/services/ausencia.py:333`, `:344`, `:418`); transição (`:828`, `:839`); **bulk update de rejeição de férias**, que não passa por `mudar_estado` mas fica no mesmo módulo/porta (`:1277`, `:1285`, `:1287`). | Censo aprovado (`app/ponto/management/commands/rederivar_falta_declarada.py:87`), identidade e reconferência (`:44`, `:184`, `:255`), modelo decide derivação e carimbo (`app/ponto/models.py:872`, `:877`), exportação filtra aprovadas (`app/folha/export.py:88`), veredito visual (`app/ponto/services/dia_decidido.py:481`). |
| `minutos` | Default (`app/ponto/models.py:755`, `:756`); criação (`app/ponto/services/ausencia.py:343`, `:418`); edição zera/recebe valor (`:507`, `:508`); modelo deriva da grade diária (`app/ponto/models.py:877`, `:885`, `:887`); transição inclui minutos na persistência (`app/ponto/services/ausencia.py:838`, `:839`); reversão excepcional atribui inteiro e salva (`:531`, `:532`). | Censo/grade/arquivo (`app/ponto/management/commands/rederivar_falta_declarada.py:88`, `:101`, `:173`), reconferência e pós-derivação (`:183`, `:202`), reversão (`:261`, `:269`); rateio financeiro (`app/folha/export.py:121`), créditos (`app/ponto/services/credito_parcial.py:24`, `:25`), fechamento (`app/ponto/services/fechamento.py:665`, `:669`, `:685`), efeito (`app/ponto/services/efeito_ausencia.py:44`, `:46`, `:51`), parcialidade (`app/ponto/turnos.py:2600`), apresentação (`app/ponto/models.py:866`, `app/ponto/services/dia_decidido.py:488`, `app/templates/ponto/ausencias.html:163`). |
| `dias_corridos` | Default (`app/ponto/models.py:761`, `:762`), criação (`app/ponto/services/ausencia.py:342`), edição (`:500`); modelo deriva pelo range (`app/ponto/models.py:899`, `:901`). | Aplicar passa dias para edição (`app/ponto/management/commands/rederivar_falta_declarada.py:199`), modelo usa para completar fim (`app/ponto/models.py:893`, `:902`, `:907`), apresentação (`app/templates/ponto/ausencias.html:160`, `:161`). |
| `chamado_origem_id` | Porta só inclui se recebido (`app/ponto/services/ausencia.py:352`, `:353`), alimentada pelos dois sítios da materialização (`app/chamados/services/materializacao.py:490`, `:526`) e abandono (`app/ponto/management/commands/marcar_abandono_provisorio.py:84`). FK `SET_NULL` permite também limpeza referencial ao apagar chamado (`app/ponto/models.py:786`, `:787`). | Seleção do passivo (`app/ponto/management/commands/rederivar_falta_declarada.py:88`), trilha de criação (`app/ponto/services/ausencia.py:447`), seleção de chamados vinculados/decididos (`app/ponto/services/ausencia.py:708`, `:719`). |

Os chamadores de entrada principais são materialização, lançamento DP, submissão do colaborador, edição, API, regularização, abandono e férias; os sítios H06 omitem minutos (`app/chamados/services/materializacao.py:484`, `:519`; `app/ponto/views.py:2412`, `:2533`, `:2791`; `app/api/views.py:1820`; `app/ponto/services/regularizacao.py:119`; `app/ponto/management/commands/marcar_abandono_provisorio.py:80`; `app/ponto/services/ausencia.py:1206`).

**ExportacaoDominio**

- `registrar` cria empresa, mês, ano, usuário, nome, hash, quantidade, flag de reexportação e conteúdo sob `atomic` e trava Empresa (`app/folha/models.py:174`, `:179`, `:181`, `:186`, `:187`, `:188`, `:189`, `:190`).
- `invalidar` é outro escritor persistente **na mesma porta**: grava `invalidada_em`, motivo e substituta com `save(update_fields=...)`, sem adquirir trava própria (`app/folha/models.py:143`, `:158`, `:159`, `:161`, `:162`; porta declarada `app/core/portas.py:504`).
- Chamadores: geração da view chama registrar (`app/folha/views.py:297`); regeração registra nova e invalida anterior (`app/folha/management/commands/regerar_txt_dominio.py:118`, `:122`).
- Leitores: juiz de entrega lê empresa/mês/ano, vigência e invalidação (`app/folha/export.py:593`, `:594`, `:595`); property/anotador leem data e PK para ordenar vigência (`app/folha/models.py:134`, `:136`, `:216`, `:219`, `:224`); listas/histórico/download/PDF (`app/folha/views.py:445`, `:522`, `:554`, `:827`; `app/folha/pdf_export.py:55`); haiku lê conteúdo e registros (`app/folha/haiku_export.py:67`, `:99`, `:100`, `:157`); vigia TXT (`app/folha/services/vigia_txt.py:201`); universo de geração de células (`app/ponto/management/commands/gerar_celulas.py:161`).

## 05/11 16:21 · sessao 154e27994ef1 · - · - · origem=Ronald (tmux)

O Stop não identifica nem deduplica evento: do payload só aproveita `transcript_path`; duas chamadas do comando percorrem a decisão novamente, com o estado que a anterior deixou (`bin/hook_stop_fila1.py:1184-1192`). O efeito concreto depende dos estados não anexados (`logs/hook_stop_fila1.json`, `hook_stop_espera.json`, `orcamento_estado.json`, cota e transcript): **não medido** (`bin/hook_stop_fila1.py:35-36`, `bin/hook_stop_fila1.py:461`, `bin/hook_stop_fila1.py:710-711`, `bin/disjuntor.py:63`).

Inventário de leitura/gravação entre chamadas:

| Estado | Leitura e gravação | O que muda na segunda execução |
|---|---|---|
| `app/docs/BACKLOG.md` | Lê marcador `ORDEM-VIVA-TOPO`, bloco OBRAS, itens fechados/parados/andáveis. Não escreve (`bin/hook_stop_fila1.py:170-176`, `bin/hook_stop_fila1.py:196-239`, `bin/hook_stop_fila1.py:299-332`). | Relê a mesma autoridade; sem mudança externa, o item continua o mesmo (`bin/hook_stop_fila1.py:306-309`). |
| `app/docs/RELATO.md` | Lê duas primeiras linhas úteis para painel; PAREI/ESPERANDO usam a primeira (`bin/hook_stop_fila1.py:400-443`). O checador lê RELATO inteiro antes de qualquer porta (`bin/hook_stop_fila1.py:1174-1181`, `bin/hook_stop_fila1.py:1194-1203`). | Checa novamente. O Stop **não escreve linhas no RELATO**: teto só registra alarme; textos sobre CLEAR são instruções emitidas, não gravações (`bin/hook_stop_fila1.py:957-959`, `bin/hook_stop_fila1.py:972-973`, `bin/hook_stop_fila1.py:1284-1293`). |
| `bin/tests/afirma_sem_prova_base.txt` | O subprocesso de prova lê a base; se ausente, cria a base das afirmações antigas e retorna zero (`bin/relato_afirma_com_prova.py:54`, `bin/relato_afirma_com_prova.py:62-77`, `bin/relato_afirma_com_prova.py:100-111`). | Após primeira criação, segunda lê a base existente. Se já existe, o caminho normal só lê (`bin/relato_afirma_com_prova.py:94-111`). |
| `logs/hook_stop_fila1.log` | Cada decisão/alarme apenda data, decisão, motivo, item e HEAD; ALLOW também é registrado (`bin/hook_stop_fila1.py:40-53`, `bin/hook_stop_fila1.py:123-133`). | Acrescenta outras linhas; se alcançar fila, acrescenta outro `BLOCK fila 1 com item aberto` **antes** do teto/B2 (`bin/hook_stop_fila1.py:1249-1250`). |
| `logs/hook_stop_fila1.json` | Lê `{head,blocks,item}`; ilegível vira `{}`. Sem trabalho em curso, mesmo HEAD incrementa `blocks`; HEAD diferente começa em 1. Grava novo JSON (`bin/hook_stop_fila1.py:1255-1280`). | Se ambas chegam aqui, soma duas unidades no mesmo HEAD; `n>5` libera, portanto uma segunda execução pode cruzar o teto e liberar (`bin/hook_stop_fila1.py:1277-1293`). O caminho CLEAR expirado pode zerar esse arquivo (`bin/hook_stop_fila1.py:1067-1073`, `bin/hook_stop_fila1.py:1220-1223`). |
| Processos da suite/push | `ps -eo args`; reconhece `manage.py test` e `git push` (`bin/hook_stop_fila1.py:373-397`). | Enquanto reconhece trabalho em curso, preserva `blocks` (ou usa 1), não aplica teto e não chama B2; ambas ainda apendam `BLOCK` da fila (`bin/hook_stop_fila1.py:1249`, `bin/hook_stop_fila1.py:1272-1295`). |
| `logs/hook_stop_espera.json` e `.lock` | `arquivo=` faz stat do alvo; chave = caminho real + dispositivo + inode; valor = máximo do mtime em nanossegundos já visto. Serializa por flock e substitui JSON via temporário `.hook_stop_espera.*` (`bin/hook_stop_fila1.py:563-642`). | **Prova consumível:** primeira observação nunca prova; observação com mtime avançado prova e grava o avanço; segunda sem novo avanço responde “arquivo nao andou” e deixa de liberar por ESPERANDO (`bin/hook_stop_fila1.py:620-642`, `bin/hook_stop_fila1.py:1231-1235`). |
| Memória `_ESPERA_MEMO` | Guarda resultado uma vez **por processo/Stop**, para orçamento e main compartilharem a mesma observação (`bin/hook_stop_fila1.py:685-697`). | Novo processo começa sem memo; não protege contra duas invocações do comando (`bin/hook_stop_fila1.py:685-697`, `bin/hook_stop_fila1.py:1309-1310`). |
| `/proc` para `pid=` | Lê cadeia ancestral, `stat`, `exe`, `cmdline`, compara identidade e starttime; não persiste memória PID (`bin/hook_stop_fila1.py:474-550`). | Refaz prova; se processo continua válido, a primeira execução não consome essa prova (`bin/hook_stop_fila1.py:523-550`). |
| Arquivos excluídos de prova de espera | RELATO, BACKLOG, trilha/contador/memória do Stop, orçamento, `pisos.log`, disjuntor, locks e cota são rejeitados por identidade dispositivo/inode, inclusive hardlinks (`bin/hook_stop_fila1.py:583-600`). | Escritas do próprio Stop nesses arquivos não fabricam prova `arquivo=` na segunda chamada (`bin/hook_stop_fila1.py:583-600`). |
| `app/docs/HANDOFF-SESSAO.md` | Lê `st_mtime`; não reescreve handoff (`bin/hook_stop_fila1.py:778-782`). | Mesmo handoff é comparado às horas persistidas do disparo/adiamento de CLEAR e da escada (`bin/hook_stop_fila1.py:913-914`, `bin/hook_stop_fila1.py:967-968`, `bin/disjuntor.py:1217-1218`). |
| `logs/sombra.lock` | Consulta se DIFF/apply está em curso; orçamento e B2/B4 usam a guarda (`bin/hook_stop_fila1.py:785-795`, `bin/disjuntor.py:307-308`, `bin/disjuntor.py:1186`, `bin/disjuntor.py:1211-1212`). | Refaz consulta; o Stop não cria prova de entrega ao consultá-la (`bin/hook_stop_fila1.py:790-795`). |
| Transcript principal | Só o caminho vem do payload; leitores medem último usage/contexto, início, primeira resposta do ciclo, mensagem humana e agentes de fundo sem entrega (`bin/hook_stop_fila1.py:1187-1190`, `bin/orcamento_contexto.py:89-148`, `bin/hook_stop_fila1.py:899`, `bin/disjuntor.py:366-382`, `bin/disjuntor.py:1079-1105`). | Relê; com arquivo estável mede o mesmo conteúdo, mas usa nova hora do Stop (`bin/hook_stop_fila1.py:844-845`, `bin/hook_stop_fila1.py:904-907`). |
| `logs/sessao/ultimo_clear.json`, `logs/deploy/historico.log`, `logs/sessao/pisos.log` | Orçamento lê marco/ciclo, histórico de deploy e piso; primeiro Stop que mede primeiro turno do ciclo apenda piso e consome `piso_pendente` (`bin/orcamento_contexto.py:57-63`, `bin/orcamento_contexto.py:152-232`, `bin/hook_stop_fila1.py:862-904`). | Estado salvo impede repetir piso do mesmo ciclo; segunda atualiza `ultima_medida` e hora (`bin/hook_stop_fila1.py:896-907`). |
| `logs/sessao/orcamento_estado.json`, `.lock`, `.orcamento_estado.*`, `orcamento.log` | Sob flock lê e substitui estado; eventos só são apendados após salvar (`bin/hook_stop_fila1.py:729-769`, `bin/hook_stop_fila1.py:798-858`). Guarda marco, desligado, pendência, medida e disparo/flags (`bin/hook_stop_fila1.py:853-907`, `bin/hook_stop_fila1.py:910-1006`). | Primeiro PREPARA-CLEAR cria `disparo` e emite `siga`; segundo não redispara, segue caminho normal com lembrete (`bin/hook_stop_fila1.py:964-976`, `bin/hook_stop_fila1.py:1001-1006`). Estados “sem handoff”, adiamento e fim de marco também emitem `siga` só na primeira ocorrência (`bin/hook_stop_fila1.py:918-943`, `bin/hook_stop_fila1.py:967-973`). |
| Janela CLEAR | Primeiro Stop que vê par válido PAREI CLEAR + handoff grava `clear_parei_desde`; prazo vigente é **10 min**, embora comentários antigos citem 30 (`bin/hook_stop_fila1.py:944-959`, `bin/orcamento_contexto.py:56`). | Segunda não reinicia o prazo. Dentro da janela libera; depois grava `desfecho=seguiu`, pede retirada da linha e zera teto (`bin/hook_stop_fila1.py:950-959`, `bin/hook_stop_fila1.py:1220-1223`). |
| `DISJUNTOR_DESLIGADO` | Checa existência, inclusive link, em cada chamada contida do disjuntor (`bin/hook_stop_fila1.py:1034-1040`, `bin/disjuntor.py:144-146`). | Se presente, B4/B2/recados ficam desativados nas duas chamadas (`bin/hook_stop_fila1.py:1039-1040`). |
| `logs/sessao/cota.json`, escada e `disjuntor_estado.json.b4_para` | Lê cota válida na janela e última humana começando “escada”; primeiro B4 Stop cria `b4_para.desde`, apenda DISJUNTOR B4 e emite `siga` exclusivo (`bin/disjuntor.py:1064-1117`, `bin/disjuntor.py:1202-1222`). | Segunda sem par válido recebe **lembrete**, continua ao orçamento/fila/teto/B2. Com PAREI COTA e handoff `mtime>=desde`, libera; sem escada remove `b4_para` (`bin/disjuntor.py:1206-1222`, `bin/hook_stop_fila1.py:1206-1214`). |
| `disjuntor_estado.json.b2.ultimo`, trilhas e git/worktrees | B2 conta linhas `BLOCK fila 1 com item aberto` na última hora, incluindo a da chamada atual; consulta commits e mtimes dos arquivos sujos em todas worktrees, excluindo docs/logs e duração dos crons (`bin/disjuntor.py:70-75`, `bin/disjuntor.py:1135-1197`). | Duplicação faz avançar a contagem mesmo sem dois turnos: terceira→quarta linha pode ativar B2 e liberar a segunda chamada. Atualiza `b2.ultimo`/anuncia no máximo uma vez/hora, **mas continua retornando motivo e liberando mesmo dentro da hora** (`bin/disjuntor.py:1186-1197`, `bin/hook_stop_fila1.py:1294-1297`). |
| B3: estado, `logs/codex/triagem.log`, transcripts/meta dos verificadores, topo+mtime RELATO | Só no ramo final de recados chama juiz B3. Lê rejeições e triagens, acumula IDs únicos, fotografa topo, dispara/libera e salva estado sob lock (`bin/hook_stop_fila1.py:1047-1052`, `bin/hook_stop_fila1.py:1300`, `bin/disjuntor.py:736-750`, `bin/disjuntor.py:823-965`). | Mesma reprovação não é contada duas vezes; IDs liberados/rearmados são ignorados (`bin/disjuntor.py:896-907`). Se primeira disparou, mesma linha do topo não libera segunda: SHA fotografado é proibido; sem alteração externa também não surge linha liberadora nova (`bin/disjuntor.py:930-945`). |
| B1 | Recados leem disparados já persistidos e os cruzam com agentes de fundo ainda sem entrega; Stop não chama medição `b1_do_agente` (`bin/hook_stop_fila1.py:1053-1057`, `bin/disjuntor.py:489-490`). | Sem alteração externa, repete recomendação TaskStop; não aumenta contador B1 (`bin/hook_stop_fila1.py:1053-1057`). |
| Gravação geral disjuntor | `logs/sessao/disjuntor_estado.json.lock`, temporário `.disjuntor_estado.*`, substituição de estado e `disjuntor.log`; b4_stop usa esse caminho mesmo para retornar None (`bin/disjuntor.py:237-288`, `bin/disjuntor.py:1202-1222`). | Pode regravar fisicamente estado idêntico; logs de transição dependem das flags, não simplesmente da segunda chamada (`bin/disjuntor.py:268-285`, `bin/disjuntor.py:1213-1216`). |

Um efeito cruzado relevante: primeira execução pode provar `ESPERANDO: … | arquivo=…`, guardar gatilho de contexto como **PENDENTE** e liberar; segunda sem novo mtime perde essa prova, recupera PENDENTE e pode emitir PREPARA-CLEAR. Isso decorre das regras de mtime e da persistência de pendência, sem depender de hipótese de deduplicação do Code (`bin/hook_stop_fila1.py:620-642`, `bin/hook_stop_fila1.py:979-1006`, `bin/hook_stop_fila1.py:1231-1233`).

A chamada normal pode registrar `BLOCK fila…` e terminar ALLOW por teto/B2; portanto a contagem B2 é de **linhas de avaliação**, não prova de que a interface exibiu dois `siga` (`bin/hook_stop_fila1.py:1249`, `bin/hook_stop_fila1.py:1284-1297`, `bin/disjuntor.py:1168-1171`).

## 05/11 16:21 · sessao 9f9661c11e00 · - · - · origem=Ronald (tmux)

Uma ocorrência positiva pela hora registrada no anexo, com ressalva sobre reescrita do hash:

| Pouso | Obra | B3 | Fontes | [nao verificado]
|---|---|---|---|
| `97bcb01c`, 02/11 04:14:23 −0300, timestamp do anexo; pouso da árvore original relatado às 04:14 | COBERTURA-PARCIAL-UM-JUIZ / COBPARC-H10 | Disparo 02:53:56; **aberto às 04:14**; liberação 04:24:42 | `pousos.txt:30`; `disjuntor.log:32`; `disjuntor.log:33`; `app/docs/RELATO.md:6197`; `app/docs/RELATO.md:6202` |

**Ressalva necessária:** o RELATO registra que o merge original era `b5e69ac7`, implantado às 04:35, e foi reescrito para `97bcb01c` após o registro das 04:41, preservando a árvore. O push ocorreu às 05:06. Portanto, o pouso relatado às 04:14 teve B3 aberto; a criação efetiva de `97bcb01c` e seu push ocorreram depois da liberação. Hora exata da reescrita: **não medido**. (`app/docs/RELATO.md:6198`, `app/docs/RELATO.md:6203`, `app/docs/RELATO.md:6204`; `disjuntor.log:33`.)

A aprovação às 03:23 e a frase “Libera o BLOQUEIA” às 03:15 não constituem liberação automática do B3: o código exige a linha válida no topo, nova perante a fotografia, não consumida e com `mtime >= disparado_em`. (`app/docs/RELATO.md:6194`, `app/docs/RELATO.md:6196`; `bin/disjuntor.py:930`.)

Mecânica a citar:

- Dispara com uma reprovação final ou três voltas Codex procedentes; a coleta final exige `agentType=verificador` e o veredito reconhecido no `SubagentHandback`. (`bin/disjuntor.py:76`, `bin/disjuntor.py:77`, `bin/disjuntor.py:546`, `bin/disjuntor.py:736`, `bin/disjuntor.py:912`.)
- No primeiro passe que dispara, executa `continue`: nenhuma liberação acontece nesse mesmo passe. (`bin/disjuntor.py:912`, `bin/disjuntor.py:921`.)
- Liberação aceita DESENHO com lei válida, pergunta e referência existente; ou ERRO-BOBO com motivo alfanumérico. A obra deve estar no cabeçalho da linha. (`bin/disjuntor.py:626`.)
- Liberação move IDs para `liberado`, esvazia reprovações/Codex, desliga `disparado` e consome o SHA; rearme move todos os IDs já triados para `rearmado` e também zera. (`bin/disjuntor.py:930`, `bin/disjuntor.py:1003`.)
- Migração de limiar arquiva reprovações antigas já guardadas sem disparar retroativamente; anúncios dependem de persistência bem sucedida. (`bin/disjuntor.py:693`, `bin/disjuntor.py:948`.)

Exclusões e limites:

- F5C `a2246c0e` pousou às 05:13:24 de 31/10; o primeiro disparo registrado da obra mãe foi às 08:35:25. As quatro reprovações eram antigas e o instrumento entrou no ar em `af56ff85`; não retroagir esse disparo ao pouso da F5C. (`pousos.txt`, linha do hash; `disjuntor.log:1`; `app/docs/RELATO.md:471`, `app/docs/RELATO.md:476`.)
- F1C4-3-NONE-COBRA `4f771457`, 31/10 09:19:26: há reprovação relatada, mas o anexo só registra MIGRA às 16:32:12, arquivando uma reprovação; B3 aberto naquele pouso: **não medido**, sem evidência positiva para inclusão. (`pousos.txt`, linha do hash; `disjuntor.log:3`; `app/docs/RELATO.md:480`; `bin/disjuntor.py:693`.)
- COBPARC-FILA-ROTULO `e1c08457`, 02/11 06:38:34: disparo somente às 07:08:00, liberação às 07:09:24; não incluir como aberto no timestamp do pouso. (`pousos.txt:29`; `disjuntor.log:34`; `disjuntor.log:35`; `app/docs/RELATO.md:6206`, `app/docs/RELATO.md:6210`.)
- Antes do primeiro registro de 31/10 08:35:25, cobertura histórica pela trilha anexada: **não medido**. (`disjuntor.log:1`.)
- O arquivo histórico informa arquivamento de conteúdo de 19/10 a 17/10; não forneceu confirmação adicional útil para o recorte solicitado. (`app/docs/RELATO-ARQUIVO.md:42`.)

## 05/11 16:20 · sessao d9ecee284088 · - · - · origem=Ronald (tmux)

**Item 4 — fontes e efeitos exatos**

- `SessionStart` com matcher `compact` declara, em ordem, `cat .../HANDOFF-SESSAO.md` e `python3 .../marco_de_clear.py compact`; o matcher `clear` declara os mesmos comandos, com argumento `clear`. `.claude/settings.json:34-60`.
- `PreCompact` declara `bash .../bin/handoff_sessao.sh` tanto para `manual` como para `auto`. `.claude/settings.json:14-32`.
- O HANDOFF da cópia mede **54 linhas e 2.834 bytes** (`wc -lc`, conteúdo em `app/docs/HANDOFF-SESSAO.md:1-54`). O arquivo contém a fotografia datada de **05/11/2026 10:32**, inclusive seus próprios HEAD/origin/main registrados; são conteúdo da fotografia. `app/docs/HANDOFF-SESSAO.md:3-16`.
- O `cat` declarado despeja esse arquivo inteiro; no caso do conteúdo da cópia, 54 linhas/2.834 bytes por execução. `.claude/settings.json:40`; `app/docs/HANDOFF-SESSAO.md:1-54`.
- O teto declarado e aplicado pelo gerador é **60 linhas**, sem teto de bytes: `TETO=60`; se exceder, grava as primeiras 59 e acrescenta aviso de corte como linha 60; caso contrário move o temporário inteiro para a saída. `bin/handoff_sessao.sh:22`; `bin/handoff_sessao.sh:293-302`.

**`marco_de_clear.py compact`**

- **Não imprime nada**; o `main` não contém emissão e retorna 0, inclusive após exceções absorvidas. `bin/marco_de_clear.py:10-11`; `bin/marco_de_clear.py:23-66`.
- Lê stdin como JSON; payload ilegível ou diferente de objeto vira `{}`. O argumento fixa `origem="compact"`; `source` e `session_id` continuam vindos do payload, com string vazia como fallback. `bin/marco_de_clear.py:25-32`; `bin/marco_de_clear.py:44-45`.
- Determina a raiz por `HOOK_STOP_RAIZ` ou pela localização do script e escreve `logs/sessao/ultimo_clear.json`. `bin/marco_de_clear.py:19-20`.
- Cria o diretório e abre/cria `ultimo_clear.json.lock` com modo `0600` e `O_NOFOLLOW`; toma `flock` exclusivo e só então lê a hora atual. `bin/marco_de_clear.py:36-43`.
- O objeto gravado tem `hora` epoch, `iso` local (`%Y-%m-%d %H:%M:%S`), `origem`, `session_id`, `source` e `clear_horas`. `bin/marco_de_clear.py:43-57`.
- Lê o marco anterior via `orcamento_contexto.abre`, preserva somente valores numéricos de `clear_horas`, conserva os últimos 20 e **só acrescenta uma nova hora à lista quando `origem == "clear"`**. Portanto `compact` atualiza o marco e conserva a lista sem inserir seu próprio horário nela. `bin/marco_de_clear.py:47-57`.
- Grava JSON em temporário `.ultimo_clear.*` no mesmo diretório e substitui o marco com `os.replace`; fecha a trava em `finally`; falhas são silenciadas. `bin/marco_de_clear.py:58-66`.
- O leitor `abre` exige arquivo regular, usa `O_NOFOLLOW|O_NONBLOCK` e recusa links/FIFOs etc. `bin/orcamento_contexto.py:68-81`.

**O que `PreCompact` faz**

- Determina a raiz pelo caminho do script, saída `app/docs/HANDOFF-SESSAO.md`, temporário `${TMPDIR:-/tmp}/handoff_sessao.$$`; produz o documento por comandos e captura stdout **e stderr** do bloco nesse temporário. `bin/handoff_sessao.sh:17-27`; `bin/handoff_sessao.sh:291`.
- Coleta HEAD, executa **`git fetch -q`**, lê origin/main, conta commits a empurrar e captura até oito linhas de `git status --short`. `bin/handoff_sessao.sh:30-41`.
- Importa `hook_stop_fila1`, lê BACKLOG, pergunta ao hook a ORDEM VIVA e o próximo item da fila. `bin/handoff_sessao.sh:44-78`.
- Lê todo o bloco inicial do RELATO até a primeira seção `## `; seleciona `PAREI`, `lei:` e `!:`, imprime até quatro linhas cortadas a 220 caracteres e obtém do hook os vereditos “trava o item em curso?”/“devolve o turno?”. `bin/handoff_sessao.sh:85-125`.
- Lê PROMPTS do dia, cruza os IDs com BACKLOG e consulta `_item_fechado`; imprime até seis avais mais recentes ainda não fechados e contagem dos demais. `bin/handoff_sessao.sh:128-171`.
- Lê a frase de placar em `app/core/placar_estrutural.py`, itens abertos da raia no BACKLOG, branches `raia20-*` com commits à frente de main e o último aval `COTA-*` no PROMPTS. `bin/handoff_sessao.sh:174-211`.
- Verifica as travas declaradas `/tmp/juliani_db_test.lock` e `logs/sombra.lock` via `flock -n`; lê `logs/deploy.stamp` se existir. `bin/handoff_sessao.sh:221-240`.
- Executa **`gerar_avais.py`**, lê seu contador e até oito itens de `AVAIS.md`. `bin/handoff_sessao.sh:243-250`. Esse comando lê `app/docs/PENDENTES_RONALD.json` e **reescreve `app/docs/AVAIS.md`**; sem `--escrever`, como nesta chamada, não grava a normalização no JSON. `bin/gerar_avais.py:29-31`; `bin/gerar_avais.py:94-108`.
- Se não houver próximo item livre, consulta `travas_da_fila()` e imprime até 12 itens travados; por fim consulta novamente o próximo item e imprime o próximo passo. `bin/handoff_sessao.sh:255-290`.
- Aplica o corte de 60 linhas, substitui o HANDOFF e imprime a mensagem `handoff_sessao: <caminho> (<N> linhas, teto 60)`. O código não protege a seção final contra o corte: retém literalmente as primeiras 59 linhas mais o aviso quando necessário. `bin/handoff_sessao.sh:293-302`.

**Limite factual:** o que efetivamente será incorporado pelo runtime e se esses hooks estiveram ativos na sessão não é demonstrado por esses arquivos; o código comprova as declarações e os efeitos dos comandos quando executados. `[não verificado]`

## 05/11 16:18 · sessao fa0a1a80f2f8 · LASTRO2 · censo · origem=Code

**Fora de `TIPO_CHOICES` não significa fora do catálogo válido.** A criação valida pelo catálogo vivo e exclui deliberadamente `tipo` do `full_clean` do modelo. Portanto, um código cadastrado pode ser gravado mesmo ausente dos choices. (`app/ponto/services/ausencia.py:193-195,385-387`; `app/ponto/catalogo/ausencias.py:240-285`)

O dado fornecido sobre a ausência **5356** não informa seu `data`, nem o cadastro de `TipoAusencia` vigente na criação. Sua competência e a validade de `'folga'` naquele momento são **[nao verificado]**.

1. Os escritores e o caminho de `'folga'`

| Escritor operacional | Valida antes de gravar? | Evidência |
|---|---|---|
| `criar_ausencia`: constrói `Ausencia(**kwargs)` e chama `save()` completo | **Sim, catálogo vivo**; não exige presença em `TIPO_CHOICES` | `app/ponto/services/ausencia.py:193-195,341-345,385-387,418` |
| `editar_ausencia_registro`: atribui `ausencia.tipo = tipo` e salva | **Sim, catálogo vivo**; não exige choices | `app/ponto/services/ausencia.py:473-475,497-508` |
| `rederivar_fim`: chama `save()` completo | **Não valida tipo**; regrava o valor carregado | `app/ponto/services/ausencia.py:1277-1282`; chamador: `app/ponto/management/commands/curar_ausencia_sem_fim.py:42-46` |
| `Ausencia.save`, persistência comum aos anteriores | **Não valida catálogo nem choices**; deriva carimbo, minutos e intervalo, depois chama `super().save()` | `app/ponto/models.py:866-906` |

Os demais `save(update_fields=...)` operacionais não incluem `tipo`: prazo, estado, documento, descrição e decisão. O `QuerySet.update` existente altera somente estado/carimbo. (`app/ponto/services/ausencia.py:536,807-819,843,896-911,1114,1124,1132,1265-1267`)

Não apareceu escritor operacional de `tipo` por `update`, `bulk_update` ou `bulk_create` no censo da cópia. O varredor contempla essas operações e separa testes/migrations; essa conclusão não cobre execução externa arbitrária. (`app/core/censo_escritas.py:54-82`) Execução externa em produção: **[nao verificado]**.

Os caminhos que chegam aos escritores validados são:

| Entrada | Destino e validação |
|---|---|
| Lançamento pelo admin da aplicação | Recebe `tipo` do POST e chama `criar_ausencia`, já aprovada. `app/ponto/views.py:2368,2405-2411` |
| Submissão PWA | Chama `criar_ausencia`. `app/ponto/views.py:2526` |
| API do aplicativo | Chama `criar_ausencia`; traduz apenas aliases legados de atestado/luto. `app/api/views.py:1768-1771,1820` |
| Copiloto | Chama `criar_ausencia`, aprovada. `app/ponto/services/checar_ausencia.py:43-48` |
| Veredito humano da célula | Passa `falta` ou `tipo_ausencia` à criação. `app/ponto/services/veredito_celula.py:311-317` |
| Materialização de disputa | Passa tipo do mapa/argumento à criação. `app/chamados/services/materializacao.py:484,519` |
| Regularização | Passa `falta` por `garantir_ausencia`. `app/ponto/services/regularizacao.py:118-122` |
| Abandono provisório | Passa `falta` por `garantir_ausencia`. `app/ponto/management/commands/marcar_abandono_provisorio.py:76-84` |
| Espelhamento de férias | Passa literalmente `ferias` à criação. `app/ponto/services/ausencia.py:1186-1191` |
| Wrapper idempotente | `garantir_ausencia` delega à criação validada. `app/ponto/services/ausencia.py:453-462` |
| Edição HTTP | Chama `editar_ausencia_registro`. `app/ponto/views.py:2784-2792` |

O formulário também valida pelo catálogo vivo, e não pelos choices. O Django admin atual proíbe adicionar, alterar e excluir `Ausencia`. (`app/ponto/forms.py:60-68`; `app/ponto/admin.py:37-47`; `app/core/admin.py:59-75`)

**Nenhum dos escritores identificados fixa literalmente `'folga'` como `Ausencia.tipo`.** Eles recebem o tipo como argumento; o produtor de férias fixa `ferias`. (`app/ponto/services/ausencia.py:130-134,342,465-467,497,1187`)

Existe, porém, um caminho dinâmico para gravar `'folga'`:

- O cadastro aceita código derivado do POST ou nome, verificando vazio e duplicidade; pode, portanto, cadastrar código `folga` ativo. (`app/ponto/views_tipos_ausencia.py:80-94`; `app/ponto/services/ausencia.py:1285-1294`)
- Tipos ativos entram em `codigos_validos()`. Se `folga` estiver nesse conjunto, a criação e a edição o aceitam. (`app/ponto/catalogo/ausencias.py:249-252,267-285`; `app/ponto/services/ausencia.py:194,474`)
- Os selects de lançamento, submissão e edição recebem valores dinâmicos; o JS envia `FormData` sem traduzir o tipo. A API também anuncia o catálogo vivo. (`app/templates/ponto/lancar_ausencia.html:26-34`; `app/templates/ponto/submeter_ausencia.html:21-30`; `app/templates/ponto/partials/painel_ausencia.html:134-138`; `app/templates/ponto/ausencias.html:249`; `app/api/views.py:1867`)

Existência, efeito e estado ativo de um cadastro `folga` em produção: **[nao verificado]**.

Os literais `tipo='folga'` encontrados no aceite de disputa seguem outro destino: `declarar_dia` despacha para `lancar_folga(..., 'FD')`, sem criar `Ausencia`. (`app/chamados/services/validacao.py:280,291`; `app/ponto/services/declarar_dia.py:167,179-191`)

A porta central emite exatamente auditoria `acao='criar', modelo='Ausencia'`, mas isso não identifica seu chamador. Atribuir a ausência 5356 a um endpoint específico permanece **[nao verificado]**. (`app/ponto/services/ausencia.py:441-448`)

2. Os leitores e o tratamento do desconhecido

Nesta tabela, **desconhecido** significa ausente do catálogo retornado por `vigente()`. Esse catálogo usa `TipoAusencia` ativo, com semente como fallback quando a leitura falha ou fica vazia. (`app/ponto/catalogo/ausencias.py:240-254,267-285`)

| Leitor | O que faz com tipo desconhecido | Evidência |
|---|---|---|
| Catálogo de efeitos e `Ausencia.efeito` | Efeito `None`; rótulo é o código; duração padrão `por_lei`; fica fora dos conjuntos de abono, desconto, bloqueio, parcial, médico, documento e intervalo indeterminado | `app/ponto/catalogo/ausencias.py:192-195,293-310,326-358,385-387,424-467`; `app/ponto/models.py:837-840` |
| `precedencia.faixa_de` | Cai em **`atestado`**. Isso também acontece com literal `folga`: o mapa só associa `folga_compensatoria` e `feriado_folga` à faixa folga | `app/ponto/precedencia.py:52-79` |
| `fatos_do_periodo` | Inclui a ausência não rejeitada, preserva o código e a coloca na faixa atestado; pode vencer feriado, folga e trabalho | `app/ponto/precedencia.py:273-279,336-339,379-405` |
| Cobertura, sobreposição, expansão e vigência | Pode cobrir a data inicial ou intervalo fechado. Sem `data_fim`, desconhecido cobre somente a data inicial. Folha exige aprovada; cobrança aceita não rejeitada | `app/ponto/turnos.py:2509-2518,2529-2533,2552-2557,2753-2790,2815-2824,3087-3095` |
| Parcial/integral | Não é parcial. Sendo diferente de `falta`, explica o dia inteiro; porém não entra no mapa de integrais **abonados** enviado ao motor, que exige efeito `ABONA` | `app/ponto/turnos.py:2595-2600,2668-2690` |
| Afastamentos | Não entra no universo restrito a `afastamento_inss` | `app/ponto/turnos.py:2702-2719` |
| Grade | Aprovada vira `tipo_dia='ausencia'`, preserva código/id, marca parcial falso e limpa células/órfãs; pode sobrepor uma folga da grade | `app/escala/utils.py:1208-1212,1231-1251,1272-1274` |
| Cartório | Copia precedência para a ata; se aprovada e integral, dispensa lâmpadas usando o código literal em `dispensada` | `app/ponto/services/cartorio.py:766-778,1947-1956` |
| Suprajuiz | Usa cobertura e parcialidade sem revalidar código; pode impedir cobrança de furo ou classificar trabalho/cobrança em ausência, conforme os demais insumos | `app/ponto/supra_juiz.py:255-269,304-316,400-405,530-534` |
| **Fechamento financeiro** | **Suprime o previsto**, porque `None != DESCONTA`; não abona nem concede crédito de minutos pagos | `app/ponto/services/fechamento.py:664-669,684-692,720-723` |
| **Espelho/PDF — coletor de efeitos** | **Não justifica nem suprime**: exige comparação explícita com `ABONA` ou `SUPRIME`. Diverge do fechamento para efeito `None` | `app/ponto/services/efeito_ausencia.py:43-61`; consumidores: `app/ponto/services/espelho.py:1068`; `app/relatorios/pdf_espelho.py:332-333` |
| Motor e calculador de pontualidade | Recebem o mapa de integrais abonados; desconhecido não obtém por esse mapa o teto que zera atraso/saída. O código do tipo aparece apenas na mensagem desse ramo | `app/ponto/turnos.py:2684-2690`; `app/ponto/services/fechamento.py:742`; `app/ponto/motor_calculo_v2.py:1085-1105`; `app/ponto/calculador/regras.py:117-120` |
| Export de faltas/8792 | Não entra: seleção exige tipo do conjunto `descontam_vigentes()` | `app/folha/export.py:75-81,127-131,168-170` |
| Export de abonos/8932 | Não entra: seleção exige tipo abonado pelo catálogo | `app/folha/export.py:182-195` |
| **Veredito visual** | **Retorna `ABONADO` para efeito `None`**, salvo rejeitada; apresentação de aberta tem ramo pendente | `app/ponto/services/dia_decidido.py:447-450,460-481` |
| Status do dia | Faixa atestado com efeito `None` resulta em `ausencia`; somente `DESCONTA` vira falta nesse ramo | `app/ponto/status_dia.py:296-319` |
| Classificação/materialização de cobranças | Ausência desconhecida não parcial pode calar classificação de falta e pergunta; cobertura diferente de `falta` também suspende adesão/furos | `app/escala/servico_jornada.py:340-345`; `app/chamados/services/materializacao.py:78-82`; `app/chamados/services/adesao.py:68-70`; `app/escala/services/furos_diarios.py:223-224`; `app/ponto/management/commands/reconciliar_grade.py:402` |
| Estado, documento e decisão | Estado inicial: aguardando decisão, sem prazo documental. Desconhecido não exige documento e não é considerado contradição documental por desconto | `app/ponto/services/ausencia.py:591-618,763-781`; `app/ponto/models.py:723-729`; `app/ponto/services/atestados_lote.py:25-29` |
| Crédito parcial e aviso na batida | Não recebe crédito de tipo pago nem aviso reservado aos bloqueantes | `app/ponto/services/credito_parcial.py:23-26`; `app/ponto/services/aviso_da_batida.py:77-92` |
| Universo médico | Fica fora dos alertas e relatórios restritos aos tipos médicos | `app/ponto/services/alertas_ausencia.py:49-50`; `app/ponto/views.py:2223,2830`; `app/relatorios/views.py:282` |
| Inteligência | Resumo exclui dos contadores de abono/desconto; score inclui não rejeitada como não abonada, penalizando 10 pontos; alertas recorrentes incluem no limiar de três registros | `app/inteligencia/resumo.py:41-44`; `app/inteligencia/management/commands/calcular_scores.py:11-16`; `app/inteligencia/management/commands/gerar_alertas.py:54-61` |
| Vigias | Conta tipo fora do catálogo entre não rejeitadas; desconhecido aprovado também participa das auditorias de discordância e ausência sem efeito | `app/ponto/services/vigia_ausencia.py:102-105,180-184`; `app/ponto/services/vigia_ausencia_veredito.py:49-57`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44-60` |
| Rótulo/ícone | Código cru, rótulo curto limitado e ícone padrão de atestado | `app/ponto/catalogo/ausencias.py:175-181,298-300` |

Essas são consequências condicionais do código. Efeito financeiro, ata ou export efetivamente associados à ausência 5356: **[nao verificado]**.

3. Uma sonda somente de leitura

O recorte abaixo conta **registros pela competência que contém `Ausencia.data`**, usando a empresa atual do colaborador. Não soma dias nem todas as competências atravessadas pelo intervalo. A janela vem de `janela_atual`; entrega vem exclusivamente de `competencia_entregue`. (`app/ponto/models.py:750-754`; `app/colaboradores/models.py:321`; `app/ponto/janelas.py:60-63`; `app/folha/export.py:545,569-575`)

A saída separa `fora_choices` e `fora_catalogo`, inclui todos os status e informa registrador somente por ID, agrupado por mês de criação. Os campos correspondentes estão em `app/ponto/models.py:690-705,799-811`. Empresa histórica na data do fato: **[nao verificado]**.

Código para o Python recebido por `bin/sonda_leitura.sh`, que já abre shell Django no schema `juliani`. O wrapper também escreve credencial/trilha; por isso, sua execução não integra esta resposta sob a regra absoluta de nenhum arquivo alterado. (`bin/sonda_leitura.sh:126-143,235-240,248-250`) Números de produção: **[nao verificado]**.

```python
import json
from collections import Counter

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Empresa
from ponto.models import Ausencia, TipoAusencia
from ponto.catalogo import ausencias as CAT
from ponto.janelas import janela_atual
from folha.export import competencia_entregue

NM = "nao medido"


def medir():
    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cur.execute("SELECT current_user, current_schema()")
            if cur.fetchone() != ("leitor", "juliani"):
                raise RuntimeError("contexto_incorreto")

        # app/ponto/catalogo/ausencias.py:240-285.
        # A leitura explícita não deixa falha de SELECT parecer catálogo vazio.
        ativos = set(
            TipoAusencia.objects.filter(ativo=True)
            .values_list("codigo", flat=True)
        )
        CAT.invalidar_cache()
        catalogo = CAT.codigos_validos()
        if ativos and catalogo != ativos:
            raise RuntimeError("catalogo_inconsistente")

        choices = {codigo for codigo, _ in Ausencia.TIPO_CHOICES}

        # app/ponto/models.py:743-811. Nenhum nome/CPF selecionado.
        campos = (
            "tipo", "status", "data", "registrado_por_id", "criado_em",
            "colaborador__empresa_id",
        )
        # União dos dois universos; sem filtro de status ou período.
        linhas = list(
            Ausencia.objects.order_by()
            .exclude(tipo__in=sorted(choices & catalogo))
            .values(*campos)
        )
        ids = {a["colaborador__empresa_id"] for a in linhas}
        empresas = {
            e.pk: e
            for e in Empresa.objects.filter(pk__in=ids)
            .only("pk", "dia_inicio_competencia")
        }

        grupos, origens = Counter(), Counter()
        totais, entregas = Counter(), {}

        for a in linhas:
            universos = []
            if a["tipo"] not in choices:
                universos.append("fora_choices")
            if a["tipo"] not in catalogo:
                universos.append("fora_catalogo")

            criado = a["criado_em"]
            if criado is None:
                raise ValueError("criacao_ausente")
            if timezone.is_aware(criado):
                criado = timezone.localtime(criado)
            mes_criacao = criado.strftime("%Y-%m")

            eid = a["colaborador__empresa_id"]
            if eid is None or eid not in empresas:
                raise ValueError("empresa_ausente")

            # app/ponto/janelas.py:60-63.
            ini, fim = janela_atual(a["data"], empresas[eid])
            chave = (eid, fim.month, fim.year)
            if chave not in entregas:
                # app/folha/export.py:545,569-575; somente SELECT.
                entregas[chave] = competencia_entregue(*chave)

            for universo in universos:
                totais[universo] += 1
                grupos[
                    (universo, a["tipo"], a["status"], eid, ini, fim)
                ] += 1
                origens[
                    (universo, a["tipo"], a["registrado_por_id"], mes_criacao)
                ] += 1

        detalhes = []
        totais_entregues = Counter()
        for (u, tipo, status, eid, ini, fim), n in sorted(grupos.items()):
            entregue = entregas[(eid, fim.month, fim.year)]
            # Zero somente após False medido pelo juiz.
            ne = n if entregue else 0
            totais_entregues[u] += ne
            detalhes.append(dict(
                universo=u,
                tipo=tipo,
                status=status,
                empresa_id=eid,
                competencia=fim.strftime("%Y-%m"),
                janela_inicio=ini.isoformat(),
                janela_fim=fim.isoformat(),
                quantidade=n,
                em_competencia_entregue=ne,
            ))

        return dict(
            fonte_catalogo=(
                "cadastro_ativo" if ativos else "semente_cadastro_vazio"
            ),
            totais={
                u: dict(
                    quantidade=totais[u],
                    em_competencia_entregue=totais_entregues[u],
                )
                for u in ("fora_choices", "fora_catalogo")
            },
            por_tipo_status_empresa_competencia=detalhes,
            por_tipo_registrador_mes=[
                dict(
                    universo=u,
                    tipo=t,
                    registrado_por_id=uid,
                    mes_criacao=mes,
                    quantidade=n,
                )
                for (u, t, uid, mes), n in sorted(origens.items())
            ],
            impacto_financeiro=NM,
            outras_competencias_atravessadas_pelo_intervalo=NM,
        )


try:
    resultado = medir()
except Exception as exc:
    # Não publica resultados parciais nem transforma falha em zero.
    # Não imprime mensagem, SQL, objetos ou traceback.
    resultado = dict(
        totais=NM,
        por_tipo_status_empresa_competencia=NM,
        por_tipo_registrador_mes=NM,
        em_competencia_entregue=NM,
        erro_classe=type(exc).__name__,
    )

print(json.dumps(resultado, ensure_ascii=False, sort_keys=True))
```

## 05/11 16:15 · sessao 462f84cc4015 · CODEX-SUFOCA-20-futurologia · leitura · origem=Code

**A falha mais previsível é em 14/11: vence a exceção que hoje permite instalar o crontab apesar de a sombra não caber no horário.** Também encontrei falhas condicionais em remarcação de férias, publicação de recibos adicionais e acompanhamento da competência após 21/11. (`app/core/tests/marca_cron15.py:24`; `app/core/management/commands/crons.py:97`; `app/ferias/services.py:393`; `app/holerite/motor.py:344`; `app/folha/management/commands/pre_fechamento.py:89`)

Recorte adotado: 05/11/2026 a 05/12/2026; quatro semanas terminam em 03/12. Natal e virada de ano entram como extrapolação solicitada. A data inicial vem do ambiente, sem comprovação documental na cópia. [nao verificado]

Os contra-exemplos abaixo são sintéticos: o anexo fornece autoridades, mas não cadastros, batidas ou estados individuais que permitam afirmar ocorrência em produção. [nao verificado]

1. **14/11 — instalação de crons perde a exceção temporária.**

   A sombra tem duração registrada de **5.188 segundos**; a derivação exige início às 03:56, anterior ao piso 04:05, e levanta `CronNaoCabe`. Executei essa função em memória e obtive essa recusa. A marca vale até 13/11 inclusive; depois disso, o render de instalação propaga a exceção mesmo recebendo a linha válida do host. (`app/config/crons_duracao.json:78`; `app/config/crons.py:266`; `app/config/crons.py:270`; `app/core/tests/marca_cron15.py:52`; `app/core/management/commands/crons.py:97`)

   **Contra-exemplo:** manter esta cópia e sua duração; em 14/11 executar `bin/crons.sh install` com crontab válido. O render falha e o script sai antes da instalação. Os 15 testes nomeados também perdem `expectedFailure`. Isso não demonstra interrupção dos crons já instalados. (`bin/crons.sh:23`; `app/core/tests/marca_cron15.py:29`; `app/core/tests/marca_cron15.py:57`)

   **Certeza alta; dono ESTRUTURA. Especulação:** uma instalação necessária após o vencimento ficará bloqueada se a duração continuar nessa condição; execução futura e duração real não foram verificadas. [nao verificado]

2. **Remarcar férias pode deixar novembro coberto por férias que foram transferidas.**

   A edição comum troca as datas e salva; o espelhamento consulta ausências somente dentro do intervalo **novo**, deixando as antigas fora da limpeza. A porta distinta de correção administrativa contém explicitamente a limpeza do intervalo anterior. (`app/ferias/views.py:667`; `app/ferias/services.py:399`; `app/ferias/models.py:434`; `app/ponto/services/ausencia.py:1165`; `app/ferias/services.py:383`)

   **Contra-exemplo:** em 05/11, agendamento futuro de 10–23/11, com 14 ausências de férias já materializadas; remarcar pela edição comum para 01–14/12, com saldo suficiente e sem colisão. As ausências de novembro permanecem e podem nascer as de dezembro. O leitor de agendamentos passa a enxergar somente dezembro, enquanto a precedência ainda recebe as ausências de novembro. (`app/ferias/services.py:401`; `app/ponto/services/ausencia.py:1182`; `app/ponto/services/ausencia.py:1195`; `app/ferias/services.py:51`; `app/ponto/precedencia.py:336`)

   **Certeza alta pela leitura estática; dono ESTRUTURA. Especulação:** remarcações para o recesso ampliarão esse estado inconsistente; existência desses agendamentos em produção é [nao verificado].

3. **13º em lote posterior pode ser descartado como recibo já publicado.** [nao verificado]

   O importador considera existente qualquer holerite do mesmo colaborador, mês e ano; não distingue mensal de 13º nessa decisão. O novo recibo recebe `ja_publicado`, e a gravação publica somente itens `ok`. (`app/holerite/motor.py:323`; `app/holerite/motor.py:344`; `app/holerite/motor.py:467`)

   **Contra-exemplo:** mensal de 11/2026 já publicado; depois, importar recibo reconhecido pelo parser da primeira parcela do 13º para a mesma pessoa e competência 11/2026. O segundo documento é pulado. A ordem inversa também bloqueia o documento posterior. Dois recibos no mesmo primeiro lote podem receber sequências diferentes; o problema é a carga posterior. (`app/holerite/motor.py:323`; `app/holerite/motor.py:345`; `app/holerite/motor.py:471`)

   **Certeza alta para esse fluxo; dono ESTRUTURA. Especulação:** o DP enviará o 13º em lote separado usando a mesma competência. Layout, procedimento real e pagamento do 13º são [nao verificado].

4. **21/11 — pré-fechamento muda para competência 12 e deixa a pauta 11 congelada.**

   O contador aceita somente chamados da janela corrente. A atualização das pautas também filtra exclusivamente a âncora corrente; prazo vencido não retira uma pauta de `vivas()`. (`app/chamados/services/fila_por_causa.py:375`; `app/folha/management/commands/pre_fechamento.py:89`; `app/pautas/models.py:57`)

   **Contra-exemplo:** corte 21; em 20/11, um chamado sobre 19/11 na gaveta `respondido_a_decidir` gera pauta 11 com “1 bloqueio”. Em 21/11, sem chamados da competência 12, o contador retorna zero, mas só alcança pautas da âncora 12. Resolver o chamado antigo em 22/11 não atualiza a pauta 11 por esse comando. (`app/chamados/services/fila_por_causa.py:343`; `app/chamados/services/fila_por_causa.py:378`; `app/folha/management/commands/pre_fechamento.py:91`)

   **Certeza alta do mecanismo, média do defeito operacional**, pois o escopo corrente é explícito. **Dono ESTRUTURA. Especulação:** o DP tomará a pauta antiga como retrato atualizado; ocorrência real é [nao verificado]. (`app/chamados/services/fila_por_causa.py:355`)

5. **21–30/11 — mapa diário continua examinando competência 11.**

   O cron passa mês e ano civis do servidor; o comando usa esses argumentos para construir as janelas examinadas. Para corte 21, a competência corrente já será 12 nesse intervalo. (`app/config/crons.py:644`; `app/ponto/management/commands/mapa_divergencia.py:44`; `app/ponto/management/commands/mapa_divergencia.py:93`; `app/ponto/janelas.py:27`)

   **Contra-exemplo:** divergência entre leitores em um dia de 23/11, pertencente à competência 12. A execução de 24/11 recebe `--mes=11`, cuja janela termina em 20/11; esse dia fica fora deste vigia até a mudança do mês civil. (`app/config/crons.py:644`; `app/ponto/janelas.py:16`; `app/ponto/management/commands/mapa_divergencia.py:104`)

   **Certeza alta; dono ESTRUTURA. Especulação:** uma divergência nova demorará a aparecer por esse instrumento; cobertura pelos demais vigias é [nao verificado].

6. **15/11 e 20/11 — feriado depende de linha cadastrada, não da tabela de importação.**

   A tabela admite essas datas, mas o juiz consulta `Feriado` pela data completa. A allowlist não cria automaticamente as linhas. (`app/core/views_config.py:118`; `app/core/views_config.py:119`; `app/core/views_config.py:141`; `app/core/feriados.py:27`)

   **Contra-exemplo:** 20/11/2026 sem linha nacional cadastrada; colaborador 5x2, célula prevista de trabalho, sem ausência nem folga. O juiz não reconhece feriado e a precedência conserva trabalho. Ausência de batidas pode então alimentar cobrança conforme o restante do fluxo. (`app/core/feriados.py:82`; `app/core/feriados.py:95`; `app/ponto/precedencia.py:341`; `app/ponto/precedencia.py:382`; `app/ponto/precedencia.py:386`)

   **Certeza alta do reconhecimento incorreto nesse cadastro; dono CADASTRO. Especulação:** faltarão linhas em novembro; o anexo não mostra a tabela efetivamente cadastrada. [nao verificado]

Não consegui verificar cadastros de feriados, aplicações CCT, agendamentos, lotes de holerites, crontab instalado, execução de comandos ou quantidade de pessoas atingidas; esses dados não constam do anexo. [nao verificado]

Sobre os demais pontos pedidos:

- **CCT até 31/01/2028:** o literal está em comentário; a execução lê vigências cadastradas, inclusivamente. Com fim efetivo nessa data, não há vencimento dentro do recorte. Vigência ausente, vencida ou aplicação empatada pode levar ao piso legal; os registros reais são [nao verificado]. (`app/core/regua_cct.py:130`; `app/core/regua_cct.py:261`; `app/core/regua_cct.py:269`; `app/core/regua_cct.py:366`)
- **Férias coletivas:** a porta de lote usa o julgamento individual; uma primeira fração de dez dias, sem outra de pelo menos 14, é recusada. Não há dado fornecido que permita concluir adequação jurídica ou ocorrência de coletivas nesse formato. (`app/ferias/views.py:569`; `app/ferias/models.py:264`; `app/ferias/models.py:361`) [nao verificado]
- **Virada de ano:** as funções puras examinadas tratam dezembro/janeiro; 21/12/2026 e 01/01/2027 pertencem à janela 21/12–20/01. Isso não certifica todos os consumidores. (`app/ponto/janelas.py:17`; `app/ponto/janelas.py:27`)
- **Datas literais:** o censo executado retornou zero `DECIDE`, mas exclui arquivos de testes — precisamente onde está o prazo CRON-15 importado pelo comando de produção. Portanto, esse zero não afasta a falha de 14/11. (`app/core/datas_cravadas.py:25`; `app/core/datas_cravadas.py:86`; `app/core/management/commands/crons.py:92`)

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 |
|---|---|---|---|---|
| Exceção dos crons vence | `app/core/management/commands/crons.py:97` | Instalar em 14/11 mantendo duração 5.188 s | Alta | ESTRUTURA |
| Férias antigas sobrevivem à remarcação | `app/ferias/services.py:399`; `app/ponto/services/ausencia.py:1165` | Mover 10–23/11 para 01–14/12 | Alta, estática | ESTRUTURA |
| Recibo adicional é pulado | `app/holerite/motor.py:344` | Mensal 11 publicado; 13º 11 em lote posterior | Alta, condicional | ESTRUTURA |
| Pauta 11 deixa de ser atualizada | `app/folha/management/commands/pre_fechamento.py:89` | Resolver em 22/11 chamado que gerou pauta em 20/11 | Alta mecanismo; média impacto | ESTRUTURA |
| Vigia fica na competência anterior | `app/config/crons.py:644` | Divergência de 23/11 fora da janela examinada em 24/11 | Alta | ESTRUTURA |
| Feriado ausente vira trabalho | `app/core/feriados.py:27`; `app/ponto/precedencia.py:386` | 20/11 sem linha nacional, célula de trabalho | Alta, condicional | CADASTRO |

## 05/11 16:15 · sessao 242c2f748e9d · CODEX-SUFOCA-18-backlog · leitura · origem=Code

**Ainda há riscos de cálculo e exportação:** exclusão por demissão fora da janela, falta fixada em 720 minutos, crédito de atraso sobrescrito e erros no recorte noturno. As evidências e os limites estão na tabela abaixo (`app/folha/export.py:840`; `app/chamados/services/materializacao.py:519`; `app/ponto/services/dia_pago.py:328`; `app/ponto/motor_calculo_v2.py:110`).

A tabela delimita o que consegui julgar; **não constitui fechamento dos cerca de 161 itens** [nao verificado]. Nenhum arquivo foi alterado. Não executei a suíte; houve reproduções isoladas em memória de funções e trechos do motor. Commits de correção, implantação e resultados em produção: **[nao verificado]**. [nao verificado]

Todos os contraexemplos abaixo são **sintéticos**, salvo os casos explicitamente identificados como relatos documentais não reproduzidos. “JA RESOLVIDO” significa resolução no código citado; quando houve reenquadramento como conforme, isso aparece expressamente. O dono L-099 refere-se ao mecanismo demonstrado, sem atribuir dono a colaboradores reais: o juiz depende também dos insumos de cadastro e batidas (`app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:104`, `app/ponto/management/commands/e6_oraculo.py:110`).

Não consegui verificar frequência, dinheiro efetivamente pago, passivo corrigido ou estado atual dos casos nominais [nao verificado]. **Especulação:** nenhuma causa de produção foi escolhida entre hipóteses concorrentes.

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 |
|---|---|---|---|---|
| **H12-DEMISSAO-NA-JANELA — AINDA VALE** | `app/folha/export.py:834`, `app/folha/export.py:840` | Competência 10/2026, janela 21/09–20/10, fechamento existente e demissão em **10/11**: já sai como `rescisao_modulo_proprio`. O ramo testa apenas demissão não nula, sem comparar a janela. | **Alta** | **ESTRUTURA** |
| **H06-FALTA-PELA-GRADE — AINDA VALE** | `app/chamados/services/materializacao.py:519`; `app/ponto/models.py:875`; `app/ponto/services/fechamento.py:865` | Jornada cadastrada de **480 min**, resposta `FALTA_DIA`, ausência nova: escritor passa **720 min**. Como os minutos já estão preenchidos, o modelo não deriva a jornada; o fechamento soma **12 h**, excedendo as 8 h previstas. | **Alta** | **ESTRUTURA** |
| **V3-LAVRAR-ATRASO-CREDITO — AINDA VALE** | `app/ponto/services/dia_pago.py:190`, `app/ponto/services/dia_pago.py:328`; `app/ponto/calculador/regras.py:855`; `app/ponto/services/fechamento.py:1261` | **Reproduzido em memória:** atraso 60 min, crédito 30 min, rubrica de atraso bruto 1 h. Primeiro resulta em **0,5 h**; depois a rubrica sobrescreve para **1 h**, embora o detalhe continue registrando crédito de 30 min. | **Alta** | **ESTRUTURA** |
| **V3-HE-NOTURNA-POR-PECA — AINDA VALE** | `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:109` | **Reproduzido:** turno 22–07h, intervalo 05h30–06h30, HE50 de 60 min, prorrogação ligada: split retorna **zero HE noturna**. A janela é recalculada pelo trecho extra após 05h, perdendo a prorrogação do turno original. | **Alta**, pela regra interna | **ESTRUTURA** |
| **V3-NOTURNO-RECORTE-LONGO — AINDA VALE** | `app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:116` | **Reproduzido:** 00h até 00h seguinte, prorrogação ligada, retorna **1.560 min para duração de 1.440 min**: faixa estendida sobrepõe 22–24h. Com 27 h retorna zero. Entrada efetiva de turnos acima de 26 h pelo pareador: [nao verificado]. | **Alta** para a função | **ESTRUTURA** |
| **V3-MOTOR-FERIADO-HE100-NOTURNA — AINDA VALE nos ramos Comercial/TurnoPartido** | `app/ponto/motor_calculo_v2.py:3323`, `app/ponto/motor_calculo_v2.py:3337`, `app/ponto/motor_calculo_v2.py:3352`; `app/ponto/motor_calculo_v2.py:3095` | **Bloco Comercial reproduzido:** período com 480 min, dobra de feriado ligada e split noturno preenchido. O bloco zera o split, grava HE100=480 e sai por `continue`, deixando **HE100 noturna=0**. Não generalizo para todos os motores. | **Alta** | **ESTRUTURA** |
| **V3-RETOMA-FILA, item 3: CCT pela data do dia — AINDA VALE no calculador** | `app/ponto/calculador/alimentacao.py:390`, `app/ponto/calculador/alimentacao.py:404`, `app/ponto/calculador/alimentacao.py:528` | Janela começa 21/10; aplicação da convenção muda em 01/11, mantendo a escala. Turno de 02/11 recebe flags resolvidas em **21/10**, pois elas são carregadas uma vez e reutilizadas por dia. Impacto numérico final: [nao verificado]. | **Alta** na seleção; **média** no impacto | **ESTRUTURA** |
| **V3-P5-PRORROGACAO-POR-INTERVALO — JA RESOLVIDO no desconto isolado** | `app/ponto/motor_calculo_v2.py:164`, `app/ponto/motor_calculo_v2.py:191`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:111` | **Antigo contraexemplo reproduzido:** turno 22–07h, intervalo 05h30–06h30, prorrogação ligada. Agora desconta **60 min noturnos**, usando a janela do turno. Isso não resolve os dois defeitos de recorte acima. | **Alta** | **ESTRUTURA**, corrigida nesse ponto |
| **STATUS-ENCERRADO-DA-COMPETENCIA-NAO-EXISTE — AINDA VALE** | `app/ponto/models.py:399`; `app/folha/export.py:668`, `app/folha/export.py:698`, `app/folha/export.py:764` | O modelo oferece somente `aberto/aprovado/exportado`. **Hipótese rotulada:** introduzir `encerrado` sem migrar leitores pode produzir classificador dizendo `entra` e montador excluindo pela tupla de status. Nenhuma linha real com esse estado foi demonstrada. | **Alta** na ausência; **média** no efeito hipotético | **ESTRUTURA** |
| **COBPARC-AUSENCIA-CALA-DOIS-JUIZES — AINDA VALE** | `app/chamados/services/materializacao.py:78`; `app/chamados/juizes.py:446`, `app/chamados/juizes.py:475`; `app/ponto/turnos.py:2632` | Ausência parcial vencedora de 240 min, sem período, célula trabalhando e marco apagado: materializador responde **coberto** pela ausência sem período; `_celula_furada` não aplica essa absolvição e pode responder **furado** pela ata. | **Alta** | **ESTRUTURA** na divergência; ausência sem período é **CADASTRO** |
| **COBPARC-FUROS-LEITOR-PARCIAL — JA RESOLVIDO nos dois leitores nominais** | `app/escala/services/furos_diarios.py:42`, `app/escala/services/furos_diarios.py:49`; `app/inteligencia/calculadores.py:49`; `app/chamados/juizes.py:398` | Ata de ausência parcial, marco dispensado entre 08–12h e marco apagado às 17h: os dois leitores já incluem o dia pelo predicado comum. Os outros quatro sítios citados no item não foram julgados [nao verificado]. | **Alta**, escopo limitado | **ESTRUTURA**, corrigida nos dois leitores |
| **GRADE-FOLGA-GERADOR — AINDA VALE** | `app/escala/models.py:1280`, `app/escala/models.py:1309`, `app/escala/models.py:1365`; `app/ponto/precedencia.py:235` | Folga **manual** guardada no vínculo A encerrado, em data gerada pelo B ativo: juiz aceita a folga da pessoa; gerador de B lê apenas suas próprias fotos e pode produzir trabalho. | **Alta** | **ESTRUTURA** |
| **GRADE-FOLGA-C5 — AINDA VALE** | `app/chamados/management/commands/auditar_invariantes_chamados.py:1531`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1541`; `app/ponto/precedencia.py:237` | Folga **de calendário** de A encerrado, data coberta por B que prevê trabalho: juiz rejeita a folga antiga; auditor a inclui pela existência de B e acusa a célula correta. | **Alta** | **ESTRUTURA** do auditor |
| **FASE-CONFLITANTE-VINCULO-DO-DIA — AINDA VALE** | `app/escala/services/fase_conflitante.py:71`, `app/escala/services/fase_conflitante.py:80`; `app/escala/alimentacao.py:228` | B ativo começa em novembro; janela medida alcança outubro, gerado por A. O medidor aplica B e suas fotos também aos dias de outubro, sem escolher o vínculo de cada dia. | **Alta** | **ESTRUTURA** do instrumento |
| **JUSTIFICATIVA-ADMIN-FORA-DA-PORTA — AINDA VALE** | `app/ponto/admin.py:18`; `app/ponto/models.py:159`; `app/ponto/services/justificativa.py:199`, `app/ponto/services/justificativa.py:214`; `app/ponto/signals.py:161` | Usuário autorizado no Django Admin altera pendente para aprovada: contorna a porta que encerra origem, invalida fechamento e registra log de domínio. **Ressalva:** o signal ainda julga o dia; não é correto afirmar ausência absoluta de eventos ou trilha. | **Alta** | **ESTRUTURA** |
| **ISENTO-PONTO-JUIZ-REGISTRADO — AINDA VALE parcialmente** | `app/core/constants.py:13`; `app/ponto/services/cartorio.py:1140`, `app/ponto/services/cartorio.py:1229`, `app/ponto/services/cartorio.py:1419`; `app/docs/CORTES.md:419` | Há função declarada como juiz e leituras diretas da flag. **Hipótese de manutenção:** mudar a função sem mudar a flag faria impressão e classificação divergirem. Hoje são equivalentes. A alegação antiga de ausência de corte não vale integralmente: existe ISENTO-H10. | **Alta** nas leituras; **média** no restante do item | **ESTRUTURA** |
| **ISENTO-FLAG-ATEMPORAL — AINDA VALE na falta de vigência** | `app/colaboradores/models.py:294`; `app/core/constants.py:18`; `app/ponto/services/cartorio.py:520`, `app/ponto/services/cartorio.py:1354` | Alterar a flag hoje muda o insumo ao rejulgar outubro, sem data de início da isenção. **Não sustento “histórico inteiro”**: a impressão tem guarda `regra >= 4`. Alcance efetivo em competências exportadas: [nao verificado]. | **Alta** na atemporalidade | **ESTRUTURA** da representação; vigência individual [nao verificado] |
| **V-FASE-FILA-DONO-DEPENDE-DA-JANELA / V-FASE-DONO-AS-OF — JA RESOLVIDO no mecanismo descrito** | `app/ponto/management/commands/e6_oraculo.py:160`; `app/escala/services/fase_12x36.py:134`, `app/escala/services/fase_12x36.py:144` | Dia 10/10 coberto por lavra antiga; nova lavra mede só novembro. O leitor busca a lavra mais recente **que cobriu 10/10**, incluindo `itens_reabertos`; avançar a janela sozinho não descarta a medida antiga. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-01 — JA RESOLVIDO: conta vazia consultada ao juiz** | `app/folha/export.py:809`; `app/ponto/views.py:1297`, `app/ponto/views.py:1309` | Fechamento aberto com zero horas no lote cego: recebe `conta_vazia` do classificador e é separado pelo consumidor. A decisão deixou de ser query própria no botão. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-02 — JA RESOLVIDO: âncora consultada ao juiz** | `app/folha/export.py:787`, `app/folha/export.py:813`; `app/ponto/views.py:1171` | Colaborador ativo 12x36 sem âncora: juiz emite `sem_ancora_12x36`; pilula e aprovação consomem essa resposta. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-03 — JA RESOLVIDO: aprovação individual lê as travas** | `app/ponto/views.py:1481`, `app/ponto/views.py:1507`, `app/ponto/views.py:1510` | Foto gravada sem turno aberto, leitura atual com turno aberto: aprovação individual injeta a leitura no classificador e recusa pela trava atual. Não decide somente pela foto. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-04 — JA RESOLVIDO por enquadramento como conforme** | `app/ponto/views.py:1367`; `app/core/juizes.py:510`; `app/ponto/tests/test_contract_juiz_fechamento.py:117` | Aprovação parcial, com alguém pulado, não tranca a competência. O contrato registrado deixou de tratar essa condição como defeito. Não houve remoção desse `if`. | **Alta** no contrato da cópia | **ESTRUTURA**, pendência reenquadrada |
| **FECH-05 — JA RESOLVIDO por enquadramento como conforme** | `app/folha/views.py:244`, `app/folha/views.py:246`; `app/core/juizes.py:510` | Geração do TXT pode carimbar aprovados sem trancar. Isso é comportamento conforme ao contrato registrado; o carimbo já filtra as travas aplicáveis. | **Alta** no contrato da cópia | **ESTRUTURA**, pendência reenquadrada |
| **FECH-06 — JA RESOLVIDO: botão não desaparece por existir aprovado** | `app/ponto/views.py:1169`, `app/ponto/views.py:1170` | Uma linha aprovada e outra aberta sem trava: botão continua disponível por `any` sobre os abertos elegíveis. A condição antiga `total_aprovados == 0` não decide mais. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-07 — JA RESOLVIDO por enquadramento como conforme** | `app/ponto/services/fechamento.py:1121`, `app/ponto/services/fechamento.py:1144`; `app/core/juizes.py:515` | Fechamento aprovado pode receber atualização dos campos calculados, preservando o carimbo. Esse comportamento permanece e foi declarado conforme; “aprovado” não é trava do recálculo. | **Alta** no contrato da cópia | **ESTRUTURA**, pendência reenquadrada |
| **FECH-08 — JA RESOLVIDO quanto à suposta obrigação de consultar tranca** | `app/folha/views.py:152`; `app/core/juizes.py:514`; `app/ponto/tests/test_contract_juiz_fechamento.py:117` | Período trancado com fechamento aberto/aprovado continua elegível ao caminho do TXT. O contrato explicitamente cobra que TXT e recálculo não consultem tranca. Não equivale a resolver o novo estado `encerrado`. | **Alta** no escopo declarado | **ESTRUTURA**, pendência reenquadrada |
| **FECH-09 — JA RESOLVIDO: painel lê tranca viva** | `app/ponto/views.py:1027`; `app/ponto/services/fechamento.py:1454` | Linha aprovada sem `PeriodoFechado` vivo: painel não chama o período de fechado apenas pelo aprovado. Linha aberta com tranca viva: reconhece a tranca. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-10 — JA RESOLVIDO: ranking distingue entrega de aprovação** | `app/folha/services/ranking_he.py:85`, `app/folha/services/ranking_he.py:87` | Fechamento aberto com TXT vigente entregue pode receber `lavrado`; aprovado sem entrega recebe `parcial`. A resposta consulta `competencia_entregue`. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-11 — JA RESOLVIDO por correção de enquadramento** | `app/holerite/matriz.py:39`, `app/holerite/matriz.py:51`, `app/holerite/matriz.py:89`; `app/core/juizes.py:590` | Em 21/09, corte padrão 21, seleção recente aponta setembro por `janela_anterior`. `_limites(2026,9)` continua civil, pois serve ao universo do holerite, outra pergunta. Não considero `_limites` uma função corrigida. | **Alta** na distinção | **ESTRUTURA**, item mal enquadrado |
| **FECH-12 — JA RESOLVIDO: ranking abre na competência anterior** | `app/folha/services/ranking_he.py:72`, `app/folha/services/ranking_he.py:106` | Hoje 04/10, corte 21, existem fechamentos parciais de outubro: default continua setembro, pela janela anterior, em vez do último fechamento existente. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-13 — JA RESOLVIDO no sítio nominal removido** | `app/core/juizes.py:618`; `app/folha/export.py:545` | Antigo `dia_em_competencia_exportada` deixou de ser o sítio dessa derivação; existe autoridade específica de entrega. Reconstituição do comportamento antigo e efeitos nos casos de produção: [nao verificado]. | **Média**, para baixa nominal | **ESTRUTURA**, sítio removido |
| **FECH-14 — JA RESOLVIDO: default do POST de recálculo** | `app/ponto/views.py:1237`, `app/ponto/views.py:1238`; `app/ponto/janelas.py:24` | Em 25/12, corte 21, POST sem mês/ano aponta **01/2027**, não dezembro civil. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-15 — JA RESOLVIDO nas telas de fechamento** | `app/ponto/views.py:1071`, `app/ponto/views.py:1106`; `app/core/juizes.py:635` | Em 25/09, corte 21, GET sem competência abre outubro. O `hoje.month` remanescente na lista de ausências não responde por fechamento; foi reenquadrado expressamente. | **Alta**, nesse escopo | **ESTRUTURA**, corrigida |
| **FECH-16 — JA RESOLVIDO no painel** | `app/ponto/views.py:981`, `app/ponto/views.py:982` | Em 25/09, painel de colaborador com corte 21 abre competência outubro pelo juiz da empresa. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-17 — JA RESOLVIDO: justificativas pela janela** | `app/ponto/views.py:997`, `app/ponto/views.py:1000` | Competência outubro, corte 21: justificativa de **22/09 entra**; a de **25/10 fica fora**. O filtro usa intervalo, não `data_turno__month=10`. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-18 — JA RESOLVIDO na origem do universo da tela** | `app/ponto/views.py:1114`; `app/ponto/services/fechamento.py:423` | Corte 21, competência outubro, desligado em 10/09: fica fora; não entra apenas porque cairia no antigo envelope iniciado em 02/09. | **Alta** | **ESTRUTURA**, corrigida |
| **FECH-19 — JA RESOLVIDO no universo do escritor** | `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:428`, `app/ponto/services/fechamento.py:545` | Mesmo caso anterior, inclusive sem empresa selecionada: universo combina as janelas de cada empresa. O envelope de cortes continua como pré-carga, não como critério de inclusão. | **Alta** | **ESTRUTURA**, corrigida |
| **SOMBRA-X-PROD-COL988-124639 — NAO SE SABE** | Relato em `app/docs/BACKLOG.md:744` | Caso documental col988, célula 124639, 11/10, julgamentos distintos entre ambientes. Sem as duas fotografias de insumos, não discrimino configuração, cadastro ou defeito. Reprodução: [nao verificado]. | **Baixa** sobre o estado atual | **[nao verificado]** |
| **ATA-DEFASADA-COMP09 — NAO SE SABE** | `app/docs/BACKLOG.md:764` | O próprio registro contém medidas contestadas de execução forçada e bump. Sem atas, versões e insumos correspondentes, não sustento quantidade afetada nem causa. | **Baixa** | **[nao verificado]** |
| **O327 — NAO SE SABE quanto à cobertura atual** | `app/docs/BACKLOG.md:1618` | Relato de 107/608 não julgados por campos sem dono. Não há registros fornecidos para repetir a lavratura desses colaboradores; não trato os demais como verdes nem os 107 como falhas atuais comprovadas. | **Baixa** | **[nao verificado]**; falha do medidor não prova dono do dado |
| **CARIMBO-QUE-A-CELULA-DESMENTE — NAO SE SABE quanto ao passivo e ao dono** | `app/docs/BACKLOG.md:1573` | Caso documental pergunta 38225, dia 29/09, carimbo `celula_absolveu` com célula acusando. Falta comparar o estado no ato do carimbo com o rejulgamento posterior; escolher uma causa seria **especulação**. | **Baixa** | **[nao verificado]** |
| **O261-b — NAO SE SABE quanto à execução e saneamento do lote** | `app/docs/BACKLOG.md:2098` | Universo documental de perguntas antigas com `via_resolucao='chamado_encerrado'`. Sem as linhas e trilhas, não confirmo os 3.923 casos, reavaliação posterior ou residual atual. | **Baixa** | **[nao verificado]** |

## 05/11 16:15 · sessao 5dbc26f2028d · - · - · origem=Ronald (tmux)

Escritores operacionais encontrados:

| Escritor | Validação de `tipo` |
|---|---|
| Construção `Ausencia(**kwargs)` e `a.save()` — `app/ponto/services/ausencia.py:341`, `:385`, `:418` | Catálogo vivo em `:193-195`; exclui expressamente `tipo` de `full_clean` em `:387`. Aceita código fora de `TIPO_CHOICES` se estiver no catálogo vivo. |
| `editar_ausencia_registro`: atribuição `ausencia.tipo = tipo`, save completo — mesmo arquivo `:497`, `:508` | Catálogo vivo em `:473-475`; não exige choices. |
| `rederivar_fim`: save completo — mesmo arquivo `:1280` | Não valida tipo; pode regravar valor existente fora dos choices ou catálogo. Chamado pelo comando `app/ponto/management/commands/curar_ausencia_sem_fim.py:42`, `:46`. |
| `Ausencia.save` — `app/ponto/models.py:866`, `:906` | Não chama validação de choices/catalogação; deriva carimbo, minutos e range, depois `super().save`. |

Chamadores que chegam à criação e, portanto, herdam a checagem de catálogo:

- Admin da aplicação: `app/ponto/views.py:2368`, `:2405` — recebe tipo do POST, cria aprovada em `:2410-2411`.
- PWA: `app/ponto/views.py:2499`, `:2526` — checagem adicional de catálogo.
- API app: `app/api/views.py:1820` — recebe tipo; tradução legada em `:1768-1771` só contempla `atestado_medico`, `atestado_acompanhante`, `obito`.
- Copiloto: `app/ponto/services/checar_ausencia.py:46` — aprovada.
- Veredito humano: `app/ponto/services/veredito_celula.py:311`, `:315` — `falta` ou argumento `tipo_ausencia`.
- Disputa: `app/chamados/services/materializacao.py:484`, `:519` — mapa da resposta ou argumento.
- Regularização: `app/ponto/services/regularizacao.py:118` — literal `falta` em `:119`.
- Abandono provisório: `app/ponto/management/commands/marcar_abandono_provisorio.py:76` — literal `falta` em `:79`.
- Férias: `app/ponto/services/ausencia.py:1186` — literal `ferias` em `:1187`.
- Wrapper idempotente `garantir_ausencia`: `app/ponto/services/ausencia.py:461`.
- Edição HTTP: `app/ponto/views.py:2784` → `editar_ausencia_registro`, tipo do POST em `:2786`.

O censo AST existente, executado em memória, também identificou saves com `update_fields` em `app/ponto/services/ausencia.py:536`, `:819`, `:843`, `:911`, `:1114`, `:1124`, `:1132`: nenhum inclui `tipo`; o update de `:1265-1267` só altera status/carimbo. As migrations de dados `0057_a4_backfill_pendente.py:43`, `:65`, `0058_a4_relogio_recomeca.py:42`, `:63`, `0059_a4_d0_no_primeiro_dia_do_cron.py:36` só alteram status/prazo. Não apareceu update/bulk operacional de `Ausencia.tipo` nas buscas e no censo; o censo exclui testes e migrations por projeto (`app/core/censo_escritas.py:54`, `:66`, `:70`).

**Não encontrei escritor de `Ausencia` que fixe literalmente `'folga'`.** Existe um caminho condicionado ao cadastro: `salvar_tipo_ausencia` aceita código derivado de POST/nome em `app/ponto/views_tipos_ausencia.py:89-94`, incluindo `folga`; recebe ativo em `:80`; grava `TipoAusencia` por `app/ponto/services/ausencia.py:1294`. Código ativo entra no catálogo consultado em `app/ponto/catalogo/ausencias.py:249-252`, `:278`, `:284-285`. Existência/estado desse cadastro em produção: **[nao verificado]**.

As telas enviam opções dinâmicas:

- Lançar: `app/templates/ponto/lancar_ausencia.html:26-34`.
- Submeter: `app/templates/ponto/submeter_ausencia.html:21-30`.
- Editar: `app/templates/ponto/partials/painel_ausencia.html:134-138`.
- JS envia `new FormData(f)` sem tradução de tipo: `app/templates/ponto/ausencias.html:249`.
- API anuncia choices do catálogo vivo: `app/api/views.py:1867`.

Os literais `'folga'` em `app/chamados/services/validacao.py:280`, `:291` chegam a `declarar_dia`, que despacha folga em `app/ponto/services/declarar_dia.py:167` para `_escrever_folga`: este chama `escala.folgas.lancar_folga(..., 'FD')` em `:179-191`, sem criação de Ausencia.

O Django admin atual não é escritor: `app/ponto/admin.py:37-47` usa `SoLeitura`; proíbe add/change/delete em `app/core/admin.py:59-66` e torna todos os campos somente leitura em `:68-75`.

O log `acao='criar', modelo='Ausencia'` é compatível com a porta central, que o emite em `app/ponto/services/ausencia.py:441-448`, mas não identifica qual chamador originou a linha. Atribuição do pk 5356 a endpoint específico: **[nao verificado]**.

## 05/11 16:14 · sessao bbf14dffc8ac · - · - · origem=Ronald (tmux)

Censo de leitores decisórios, somente na cópia. “Desconhecido” abaixo significa ausente de `vigente()`; estar fora de `TIPO_CHOICES` não basta, pois `vigente()` consulta `TipoAusencia` ativo e só usa a semente como fallback (`app/ponto/catalogo/ausencias.py:240-254`, `:267-285`). Logo, se `folga` existe no cadastro de prod é [nao verificado].

| Leitor | Comportamento para desconhecido | Evidência |
|---|---|---|
| Catálogo de efeitos | `efeito_vigente` retorna `None`; rótulo retorna o próprio código; não pertence a abonados/descontados/bloqueantes/parciais/médicos/documentais/indeterminados. `fim_de` retorna `por_lei`. | `app/ponto/catalogo/ausencias.py:192-195`, `:293-310`, `:326-358`, `:385-387`, `:424-435`, `:403-407` |
| Propriedade `Ausencia.efeito` | Delega ao efeito vigente, portanto `None`. | `app/ponto/models.py:837-840` |
| `precedencia.faixa_de` | Todo código fora do mapa fixo cai em `atestado`, mesmo desconhecido ao cadastro; literal `folga` também cai em `atestado`, pois só `folga_compensatoria` e `feriado_folga` estão na faixa folga. | `app/ponto/precedencia.py:52-64`, `:76-79` |
| `fatos_do_periodo` | Inclui não rejeitadas sem validar código; desconhecido participa como faixa atestado, preserva código em `tipo_do_dia`; vence feriado/folga/trabalho se não houver faixa anterior. | `app/ponto/precedencia.py:273-279`, `:336-339`, `:379-405`; ordem `:47-48` |
| Cobertura/expansão/vigência | Desconhecido pode cobrir data inicial ou range fechado; `data_fim=None` cobre só a data inicial, pois não está em `TIPOS_EM_ABERTO`. Folha só aprovada; cobrança toda não rejeitada. Desempate maior data e pk. | `app/ponto/turnos.py:2509-2518`, `:2529-2533`, `:2552-2557`, `:2753-2770`, `:2774-2790`, `:2815-2824`, `:3087-3095` |
| Parcial/integral | Não pertence aos parciais, logo `cobre_parte_do_dia=False`. Se diferente de literal `falta`, `ausencia_explica_o_dia_inteiro=True`; porém mapa de integrais que alimenta o motor exige adicionalmente efeito `ABONA`, portanto desconhecido não entra. | `app/ponto/turnos.py:2595-2600`, `:2668-2675`, `:2684-2690` |
| Afastamento | Não entra: restringe aos tipos `('afastamento_inss',)`. | `app/ponto/turnos.py:2702-2719` |
| Grade `_marcar_ausencias` | Aprovação desconhecida vira `tipo_dia='ausencia'`, preserva tipo/id, marca parcial falso, limpa células e órfãs; também sobrepõe dia originalmente folga porque código difere de `falta`. | `app/escala/utils.py:1208-1212`, `:1231-1251`, `:1272-1274` |
| Cartório, lâmpadas | Se aprovada e grade não parcial, dispensa lâmpadas e escreve código literal desconhecido em `dispensada`; nenhuma validação do catálogo. | `app/ponto/services/cartorio.py:766-778` |
| Cartório, precedência | Copia resultado do juiz, inclusive código desconhecido e faixa atestado, para a ata. | `app/ponto/services/cartorio.py:1947-1956` |
| Suprajuiz | Consome status de cobertura e flag parcial da grade, não revalida código. Aprovação desconhecida conta como cobertura integral; impede ramo de furo, pode acusar trabalho em ausência ou cobrança em dia coberto conforme outros insumos. | `app/ponto/supra_juiz.py:255-269`, `:304-316`, `:400-405`, `:530-534` |
| **Fechamento financeiro** | **`None != DESCONTA`: desconhecido aprovado adiciona dia a `_datas_suprimidas`**, sem abonar. Não recebe crédito de minutos dos tipos pagos. | `app/ponto/services/fechamento.py:664-669`, `:684-692` |
| **Espelho/PDF via `efeitos_da_ausencia`** | **Diverge do fechamento**: só suprime se efeito é exatamente `SUPRIME`; desconhecido não justifica, não suprime, não recebe crédito nem entra nos integrais abonados. | `app/ponto/services/efeito_ausencia.py:43-61`; chamadas `app/ponto/services/espelho.py:1068`, `app/relatorios/pdf_espelho.py:332-333` |
| Motor | Não decide pela string desconhecida: recebe mapa de integrais abonados; teto que zera atraso/saída depende de presença nesse mapa. Código aparece só na mensagem. Como produtor exige `ABONA`, desconhecido não obtém esse teto. | `app/ponto/services/fechamento.py:682-683`, `:742`; `app/ponto/turnos.py:2689-2690`; `app/ponto/motor_calculo_v2.py:1085-1105` |
| Export 8792/faltas | Catálogo restringe a `descontam_vigentes`; desconhecido não entra na falta/minutos de falta. | `app/folha/export.py:75-81`, `:127-131`, `:168-170` |
| Export 8932/abonos | Catálogo restringe a abonados menos `FORA_DO_8932`; desconhecido não entra. | `app/folha/export.py:182-195` |
| **Veredito visual `dia_decidido`** | **Efeito `None` cai no retorno `ABONADO`**, salvo rejeitada; pedido aberto tem apresentação `PENDENTE` por outro ramo. | `app/ponto/services/dia_decidido.py:447-450`, `:460-473`, `:476-481` |
| Status do dia sem lavra/batida | Faixa atestado com efeito None vira status `ausencia`, pois só efeito DESCONTA vira `falta`. | `app/ponto/status_dia.py:296-319` |
| Classificador de falta | Qualquer ausência não rejeitada não parcial cala cobrança; desconhecido não é parcial, portanto pode retornar `(None,None,None)`. | `app/escala/servico_jornada.py:340-345` |
| Materialização, cobertura | Desconhecido não falta e integral cala pergunta. | `app/chamados/services/materializacao.py:78-82`; integral `app/ponto/turnos.py:2675` |
| Adesão | Cobertura desconhecida diferente de `falta` faz pular cobrança de adesão. | `app/chamados/services/adesao.py:68-70` |
| Furos diários / reconciliar grade | Cobertura desconhecida diferente de falta entra como ausência, suspendendo cobrança. | `app/escala/services/furos_diarios.py:223-224`, `app/ponto/management/commands/reconciliar_grade.py:402` |
| Crédito parcial | Só tipos pagos: desconhecido não ganha crédito. | `app/ponto/services/credito_parcial.py:23-26` |
| Estado/documento/aprovação | `estado_inicial` desconhecido cai aguardando decisão sem prazo; contradição documento só DESCONTA (None passa); gate documental só membros de `TIPOS_EXIGEM_DOC` (desconhecido passa). | `app/ponto/services/ausencia.py:763-781`, `:591-597`, `:610-618` |
| Ausência `exige_documento` | Delega ao estado inicial, portanto desconhecido retorna falso. | `app/ponto/models.py:723-729` |
| Exportar documentos | `_exige_documento` desconhecido retorna falso. | `app/ponto/services/atestados_lote.py:25-29` |
| Aviso na batida | Restrição a tipos bloqueantes com frase cadastrada: desconhecido não gera aviso. | `app/ponto/services/aviso_da_batida.py:77-92` |
| Alertas médicos / relatórios médicos | Desconhecido não é médico, fica fora. | `app/ponto/services/alertas_ausencia.py:49-50`, `app/ponto/views.py:2223`, `:2830`, `app/relatorios/views.py:282` |
| Resumo inteligência | Fora tanto de faltas como de justificadas, pois ambos usam conjuntos do catálogo. | `app/inteligencia/resumo.py:41-44` |
| Score assiduidade | Desconhecido não rejeitado permanece após excluir abonados: penaliza 10 pontos por registro. | `app/inteligencia/management/commands/calcular_scores.py:11-16` |
| Alertas recorrentes | Desconhecido não rejeitado permanece no universo não abonado; participa do limiar de 3 registros no mês. | `app/inteligencia/management/commands/gerar_alertas.py:54-61` |
| Vigia de família | Classifica não rejeitadas com código ausente de catálogo em `tipos_fora_do_catalogo`; informa contagem e tipos, não corrige código. | `app/ponto/services/vigia_ausencia.py:102-105`, `:180-184` |
| Vigia ausência/veredito | Desconhecido aprovado não parcial é candidato a discordância com célula acusadora, agrupado por código. | `app/ponto/services/vigia_ausencia_veredito.py:49-57` |
| Comando ausências sem efeito | Desconhecido não é DESCONTA nem parcial, portanto não é descartado da auditoria de cobertura sem efeito. | `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44-60` |
| Rótulo/ícone | Código cru como rótulo vigente; rótulo curto limitado a 12; ícone padrão de atestado. | `app/ponto/catalogo/ausencias.py:175`, `:178-181`, `:298-300` |

Não transformar comentários históricos do código em dado de prod atual; por exemplo o comentário sobre “3 aprovadas” em `fechamento.py:674-675` não é medição autorizada pelo anexo.

Literal `folga` como declaração do colaborador é explicitamente distinguido de tipo de ausência em `app/chamados/services/declaracao_texto.py:145-156`; o menu admite `folga`, mas isso por si não prova gravação de Ausencia (`:160-169`).

## 05/11 16:14 · sessao d31d6314ef56 · - · - · origem=Ronald (tmux)

Achados sazonais, somente leitura:

1. **14/11: expira a exceção de cron; instalação e 15 testes voltam a falhar com esta configuração.** O prazo é 13/11/2026 inclusive; depois disso o decorador devolve os testes sem `expectedFailure` (`app/core/tests/marca_cron15.py:24`, `:52`, `:57`). O render também propaga `CronNaoCabe` após o prazo (`app/core/management/commands/crons.py:93`, `:97`). O JSON contém `sombra.sh=5188` segundos (`app/config/crons_duracao.json:78`); o cálculo arredonda para 87 minutos e exige começar antes do piso 04:05, considerando vizinho 05:25 e tolerância de dois minutos (`app/config/crons.py:182`, `:183`, `:219`, `:266`). **Contraexemplo:** relógio do processo em 14/11, código e JSON iguais à cópia, render para instalação mesmo com linha válida de sombra no host. **Certeza alta; ESTRUTURA.** Não prova interrupção do cron já instalado; crontab e execução reais são **[não verificado]**.

2. **Remarcar férias deixa ausências da janela anterior.** A remarcação troca datas e salva sem limpar o intervalo antigo (`app/ferias/services.py:393`, `:400`, `:406`). O `save` espelha novamente (`app/ferias/models.py:434`), mas o espelhamento consulta apenas o intervalo novo (`app/ponto/services/ausencia.py:1165`); a limpeza posterior percorre somente essas linhas (`:1195`). A outra porta de correção conhece o problema e apaga expressamente o intervalo antigo (`app/ferias/services.py:374`, `:385`). **Contraexemplo:** em 05/11, AG agendado 10–23/11 com 14 ausências aprovadas; editar para 01–14/12, mantendo 14 dias e sem colisão. As ausências de novembro permanecem; o agendamento passa a dezembro. O leitor `em_gozo_hoje` consulta as datas do AG (`app/ferias/services.py:47`, `:63`). **Certeza alta estática; ESTRUTURA.** Existência desse estado em produção: **[não verificado]**.

3. **13º em lote posterior colide com o holerite mensal da mesma competência.** A importação considera publicado qualquer holerite do mesmo colaborador/mês/ano (`app/holerite/motor.py:323`), classifica o novo recibo como `ja_publicado` (`:344`) e publica somente `ok` (`:467`). A identidade não inclui tipo de folha; o modelo contém competência e sequência (`app/holerite/models.py:107`), e a sequência reinicia dentro de cada publicação (`app/holerite/motor.py:465`). **Contraexemplo:** mensal 11/2026 publicado; carregar depois recibo reconhecido da primeira parcela do 13º para o mesmo colaborador e 11/2026: o novo recibo será pulado. **Certeza alta do comportamento; ESTRUTURA. Especulação rotulada:** que o DP carregará o 13º dessa forma nas quatro semanas é **[não verificado]**. Dois recibos juntos no primeiro lote recebem sequências diferentes (`app/holerite/motor.py:471`, `:480`).

Limitações relevantes:

- “Férias em lote” usa a validação individual (`app/ferias/views.py:569`, `:605`). Um PA sem frações anteriores, submetido com dez dias, é recusado pela exigência de uma fração de pelo menos 14 dias (`app/ferias/models.py:264`, `:365`). Aplicabilidade de regra específica de férias coletivas e utilização operacional desse caminho: **[não verificado]**; não apresentaria como infração legal provada.
- `aviso_emitido` é calculado pela antecedência, sem prova de comunicação nesse trecho: marcar férias para 20/12 em 20/11 produz `True` (`app/ferias/services.py:313`, `:323`), embora o campo se descreva como aviso emitido (`app/ferias/models.py:305`). Entrega real do aviso: **[não verificado]**.
- Ausência de agendamento automático de férias é deliberada, portanto não constitui achado por si só (`app/ferias/situacao.py:1`).

_1829 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
