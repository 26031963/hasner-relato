# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 18:13** · publicado em **15/11/2026 18:13** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 18:13 · sessao e3b4179907c7 · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **G3 sem selo que morda a regressão textual — baixa.** O teste c15 contém apenas G1 e G2 (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:81`, `:185`); os 15 mutantes também cobrem exclusivamente essas duas curas (`app/chamados/tests/e3_r3_c15_mutantes.py:27`). Reintroduzi, apenas em memória, textos antigos contraditórios em tarefa_do_dia, fora_da_folha, retrato_folha e models: os nove testes c15 continuam verdes. O selo textual c14 cobre outros sítios explicitamente enumerados (`app/chamados/tests/test_e3_r3_c14_sem_banco.py:409`). A cura documental existe nos arquivos vivos, portanto não foi feita só no teste/texto de entrega; falta a mordida pedida para G3 (`app/chamados/services/tarefa_do_dia.py:32`; `app/folha/fora_da_folha.py:77`; `app/folha/retrato_folha.py:474`; `app/folha/models.py:306`).

2. **Contrato documental de RetratoFolha ainda contraditório — baixa.**
   - Afirma que linha antiga sem as três chaves cai em fio `ninguem` (`app/folha/models.py:315`), mas `{status:'fora', motivo:'sem_codigo_dominio'}` sem `dono` cai em `admin`: `fio_da_linha` usa `_fio_cru` (`app/folha/retrato_folha.py:449`), que retorna admin para motivo diferente de furo (`:374`).
   - Enumeração de grupos ainda diz A/B/C/D (`app/folha/models.py:308`), embora `_grupo(['nao_sei'])` retorne E (`app/folha/fora_da_folha.py:112`).
   - Schema de saldo ainda enumera somente `1|2a5|6mais|None` (`app/folha/models.py:307`), embora motivo cadastral produza `cadastro` (`app/folha/retrato_folha.py:424`).

Resultados executados sem gravação:

- c15: nove testes verdes, quinze mutantes mortos por asserção, sem erro de infraestrutura; o runner rejeita morte por erro (`app/chamados/tests/e3_r3_c15_mutantes.py:80`).
- c5+c6+nome_indefinido: 34 testes verdes (`app/chamados/tests/test_e3_r3_c5_sem_banco.py:24`; `app/chamados/tests/test_e3_r3_c6_sem_banco.py:87`; `app/chamados/tests/test_e3_r3_c6_nome_indefinido.py:29`).
- Vizinhos de desligamento têm lei explícita justificando inversão, preservam gêmeo ativo e verificam informativo (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:1`, `:102`, `:116`; `app/folha/tests/test_lei_desligado_nao_trava.py:79`, `:103`).
- Medida mantém números registrados 9/85 e critérios A1=6/B1=82 (`app/core/tests/test_medida_turno_do_par.py:100`); os três sítios novos aparecem na classificação informativa (`:391`, `:415`).
- Enfraquecimento relativo dos testes entre hashes anteriores: **[nao verificado]**, porque esta cópia não disponibiliza histórico Git acessível. [nao verificado]

## 15/11 18:13 · sessao 091be4eebcf9 · - · - · origem=Ronald (tmux)

- **G1, média: cura parcial.** O escritor grava `_agora` em todos os retratos (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `:83`), mas o vigia ainda identifica rodada por dia civil local (`app/folha/vigia_retido_calado.py:47-50`). O próprio comentário promete ler a última lavra (`:20-25`).

  Casos concretos reproduzidos executando `censo` real com dependências em memória:
  1. Legado: comp10 carimbada em **14/11 23:59:59**, com um dia sem leitura; comp11 em **15/11 00:00:01**, sem linhas bloqueadas. Retorna apenas comp11, `dias_sem_leitura_sistema=0`, `calados=0`, `cego=1`; o positivo de comp10 desaparece pela seleção em `:48-50`, antes das somas em `:65-80`.
  2. Duas rodadas novas no mesmo dia: às06h, execução explícita de comp09 grava positivo1; às07h, execução completa grava comp10 e comp11, ambas zero. O vigia retorna **09+10+11 e positivo1**, embora a rodada completa mais recente contenha apenas10/11. Execução explícita é permitida em `lavrar_retrato_folha.py:57-58`; universo automático vem de `:62-69`; competências antigas permanecem porque `update_or_create` só substitui sua própria chave (`:81-83`). Não é versão física antiga da mesma competência: a unicidade está em `app/folha/models.py:363-364`.
  3. Rodada completa comp10/11 às06h, comp10 positivo1; relavra parcial de comp11 às07h, zero: continuam ambas e positivo1. Isso confirma que o resultado depende da interpretação “união do dia”, não “última rodada” (`vigia_retido_calado.py:47-50`). A entrega assume deliberadamente essa interpretação, apesar da cura pedida na triagem. [nao verificado: ocorrência desses casos em produção]

- **Mordida G1:** os dois mutantes registrados em `app/chamados/tests/e3_r3_c15_mutantes.py:28-29` foram executados e morreram com falha de asserção, sem erro de infraestrutura; sustentam a cura do escritor nos testes `app/chamados/tests/test_e3_r3_c15_sem_banco.py:85-92`. O leitor não é protegido: substituir em memória `vigia_retido_calado.py:48` o dia pelo instante manteve os três testes G1 verdes. A fixture só produz uma rodada (`test_e3_r3_c15_sem_banco.py:35-78`) e não distingue agrupamento por dia de agrupamento por instante. O teste vizinho exige dois carimbos diferentes, T0 e T0+1min, como uma lavra (`app/folha/tests/test_vigia_retido_calado.py:42-52`); essa fixture representa o escritor antigo e não justifica conservar a ambiguidade do leitor após a mudança.

- **Leitores de `RetratoFolha.lavrado_em`:**
  - Vigia: dia civil local identifica a última lavra (`app/folha/vigia_retido_calado.py:47-50`).
  - Frescor: instante comparado ao maior `FechamentoMensal.atualizado_em`; fechamento estritamente posterior significa desatualizado, igualdade significa atualizado (`app/folha/retrato_folha.py:1401-1411`). Assume início da rodada, explicitamente conservador (`:1382`).
  - Rótulo: converte o instante para horário local e apresenta “retrato lavrado em” (`app/folha/retrato_folha.py:1430-1434`).
  - View: usa o mínimo dos instantes dos retratos exibidos (`app/folha/views_retrato.py:201-203`), repassa como `atualizado` (`:241`, `:319`), e a barra mostra “atualizado HH:MM” (`app/templates/folha/_retrato_barra.html:22`).
  - Escritor único encontrado: `app/folha/management/commands/lavrar_retrato_folha.py:81-83`. Outros campos homônimos encontrados pertencem a DiaPago/Placar/Pauta, não a RetratoFolha (`app/folha/porta_export.py:283-287`, `app/chamados/views.py:843-848`).

- **G3/semântica, baixa:** o help_text ainda define o campo como “Quando o lavrador escreveu esta linha” (`app/folha/models.py:354-357`), mas ele passou a conter o início da rodada, possivelmente anterior à montagem e à gravação daquela competência (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `:69`, `:83`). Há três sentidos concorrentes: início da rodada, momento da escrita e dia identificador de rodada; os locais acima demonstram a divergência.

**Recomendação do recorte B/G1: REPROVA**, pela seleção incorreta do universo ainda em `app/folha/vigia_retido_calado.py:47-50`.

## 15/11 18:03 · sessao 6740547b38e3 · JUIZ-RESPONDIDA · censo · origem=Code

Há critérios diferentes para perguntas diferentes: a fila global usa **instante E texto**, a retenção sem veredito usa **instante OU texto**, e a entrada de `validar_pergunta` verifica **só instante**. Não escolhi entre eles. (`app/chamados/juizes.py:1725`; `app/chamados/models.py:1907`; `app/chamados/services/validacao.py:418`.)

**Árvore:** examinei a cópia fornecida. Correspondência com `HEAD` de `main` e com o hash informado: **[nao verificado]**, pois não há metadados Git acessíveis. Execução dos testes: **[nao verificado]**; abaixo descrevo suas asserções estaticamente. [nao verificado]

**Convenções do censo:** `I` = instante presente; `T` = texto que sobra de `(texto or '').strip()`. Onde o código usa verdade crua, igualdade com `''`, comparação de conteúdo ou intervalo temporal, isso está explicitado. (`app/chamados/juizes.py:1523`; `app/chamados/juizes.py:1531`.)

Classes: **A** = `instante E texto`; **B** = `instante OU texto`; **C** = `so instante`; **D** = `so texto`; **E** = `outro`, detalhado na célula; **F** = `exibicao (so mostra)`. A classificação considera o uso dos dois campos naquele sítio; os demais filtros continuam indicados.

**(a) Leitores**

Os números R identificam usos separados por função/finalidade, inclusive quando reunidos numa linha. Q identifica a finalidade normalizada para a contagem final. Os delegados explícitos aparecem além do corpo que consultam.

| R | Arquivo:linha — função | Campos | Classe / predicado aplicado | Pergunta/finalidade | [nao verificado]
|---|---|---|---|---|
| 1 | `app/api/views.py:1163`, `:1165` — `api_chamados_v2` | I,T | **F** — serializa resposta e data | Q01 Payload do app |
| 2 | `app/api/views.py:1179` — `api_chamados_v2` | I,T, delegados | **E** — permite correção quando `not aguarda_o_colaborador(...)`, além dos outros limites | Q02 Correção/reenvio |
| 3 | `app/api/views.py:2561`, `:2563` — `api_disputa_perguntas._item` | I,T | **F** — serialização | Q01 Payload do app |
| 4 | `app/api/views.py:2577` — `api_disputa_perguntas` | I | **C** — `I` seleciona `respondidas` dentro do histórico | Q03 Partição pendente/histórico |
| 5 | `app/api/views.py:2620` — `api_disputa_responder` | I | **C** — `ja_respondida = I` | Q02 Correção/reenvio |
| 6 | `app/api/views.py:2637` — `api_disputa_responder` | I | **F** — devolve instante gravado | Q01 Payload do app |
| 7 | `app/api/views_mensageria.py:567`, `:569`, `:570` — `_item_validar` | I,T | **F** — resposta, data e dias de espera | Q04 Fila de validação |
| 8 | `app/chamados/canal.py:65` — `perguntas_mudas_com_resposta` | T | **D** — SQL `resposta_colab=''`, sem trim | Q05 Resposta encontrada no fio |
| 9 | `app/chamados/juizes.py:351`, `:363` — `_slot_esperado` | I,T | **E** — rejeita resposta posterior ao instante consultado; interpreta `data_hora` ou delega ao plantio | Q06 Slot/materialização |
| 10 | `app/chamados/juizes.py:855`, `:856` — `_chave_memo_pv` | I,T | **E** — valores integram chave de memo; `getattr` literal | Q07 Cache/invalidação — técnica |
| 11 | `app/chamados/juizes.py:1359`, `:1371`, `:1376` — `_resposta_concorda_com_a_luz` | T | **E** — parse da hora e concordância classe A com luz do marco | Q08 Vitalidade |
| 12 | `app/chamados/juizes.py:1388`, `:1393` — `projecao_temporal_da_pergunta` | I,T | **E** — se instante posterior ao teto, projeta localmente `(None, '')` | Q09 Projeção temporal — técnica |
| 13 | `app/chamados/juizes.py:1403`, `:1420` — `_pergunta_viva_calc` | I,T, via projeção | **A** — degrau da resposta: `I and T`; a vitalidade completa segue os demais degraus | Q08 Vitalidade |
| 14 | `app/chamados/juizes.py:1531` — `respondida_sem_texto` | I,T | **E** — `I ∧ ¬T` | Q10 Integridade do par — técnica |
| 15 | `app/chamados/juizes.py:1551` — `q_respondida_sem_texto` | I,T | **E** — face SQL de `I ∧ vazio(strip)`; contempla NULL | Q10 Integridade do par — técnica |
| 16 | `app/chamados/juizes.py:1559` — `q_respondida_com_texto` | I,T | **A** — instante não nulo e regex de texto após trim | Q10 Integridade do par — técnica |
| 17 | `app/chamados/juizes.py:1706` — `porta_alcancavel_do_dia` | I,T, delegados | **E** — exclui `respondida_sem_texto`; exige fila e presença no app | Q11 Alcance/porta |
| 18 | `app/chamados/juizes.py:1718` — `perguntas_corrigiveis` | I | **C** — `I`, mais `pergunta_sem_lastro_ou_viva` | Q02 Correção/reenvio |
| 19 | `app/chamados/juizes.py:1725`, `:1728` — `perguntas_respondidas_a_validar` | I,T | **A** — `q_respondida_com_texto`, `q_vivas` e confirmação Python de vitalidade | Q04 Fila de validação |
| 20 | `app/chamados/juizes.py:1753`, `:1756` — `declarou_hora` | T | **E** — texto não vazio que não seja maiúsculo nem `[descartado...` | Q06 Slot/materialização |
| 21–23 | `app/chamados/juizes.py:1900` — `delta_da_resposta`; `:2004` — `classificar_resposta_validacao`; `:2026` — `motivo_implausivel_validacao` | T | **E**, em cada função — interpreta conteúdo/hora, classe ou plausibilidade | Q12 Classificação da resposta |
| 24 | `app/chamados/juizes.py:2079` — `irmas_respondidas_sem_consumo` | I,T, delegados | **B** — `sem_veredito_entre(..., ignorar_tranca=True)`; universo do crédito | Q13 Retenção por resposta sem veredito |
| 25–26 | `app/chamados/management/commands/auditar_invariantes_chamados.py:177` — `_contraprova_i21a`; `:592` — `Command.handle`, I2 | I | **C**, em ambos — falta/presença do instante, com outros filtros | Q14 Auditoria de invariantes |
| 27 | `app/chamados/management/commands/auditar_invariantes_chamados.py:984` — `Command.handle`, I12 | I,T, delegados | **B** — consulta `irmas_respondidas_sem_consumo` | Q14 Auditoria de invariantes |
| 28 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1105`, `:1106` — `Command.handle`, I20 | I,T | **E** — instante nulo e SQL `texto != ''`; sem trim | Q14 Auditoria de invariantes |
| 29 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1572`, `:1575` — `Command.handle`, I21 | I,T | **A** — instante presente e SQL `texto != ''`; sem trim; não validada/materializada | Q14 Auditoria de invariantes |
| 30 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1603` — `Command.handle`, C2 | I | **C** — instante nulo | Q14 Auditoria de invariantes |
| 31 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1655` — `Command.handle`, F1 | T | **D** — existência de texto após strip interrompe o ramo | Q14 Auditoria de invariantes |
| 32 | `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`, `:60` — `linhas_a_criar` | I,T | **E** — `I OU validada_em`; carrega o par por `.only()` e delega reconstrução | Q15 Preservação/recomposição da trilha |
| 33 | `app/chamados/management/commands/backfill_decisoes_pergunta.py:326` — `aplicar` | T | **E** — copia texto para ato `SILENCIO` | Q15 Preservação/recomposição da trilha |
| 34 | `app/chamados/management/commands/backfill_decisoes_pergunta.py:504`, `:506` — `Command.handle` | T | **D** — `.only()` fornece texto à eleição de canônica | Q16 Deduplicação/mérito |
| 35 | `app/chamados/management/commands/curar_perguntas_em_limbo.py:32` — `nota_da_resposta` | T | **E** — procura no followup o conteúdo literal da resposta, após strip | Q17 Diagnóstico/cura de limbo |
| 36 | `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `:69` — `Command.handle` | I,T | **E** — instante nulo e SQL `texto != ''`; limpeza mantém filtro de instante nulo | Q17 Diagnóstico/cura de limbo |
| 37 | `app/chamados/management/commands/dedup_perguntas_fato.py:75` — `Command.handle` | T | **D** — texto após strip | Q16 Deduplicação/mérito |
| 38 | `app/chamados/management/commands/validar_classe_a.py:54` — `Command.handle` | I,T, delegados | **B** — `respostas_sem_veredito()` | Q04 Fila de validação |
| 39 | `app/chamados/management/commands/validar_classe_a.py:80` — `Command.handle` | T | **F** — amostra truncada no relatório do comando | Q20 Exibição da resposta |
| 40 | `app/chamados/models.py:1533` — `ChamadoColaborador._universo_da_ressuscitacao` | T | **D** — SQL texto NULL ou `''`, sem trim | Q18 Ressuscitação |
| 41 | `app/chamados/models.py:1610`, `:1611` — `ChamadoColaborador.ressuscitar_perguntas` | I,T | **E** — carrega par por `.only()` para preservar projeções sem linha | Q15 Preservação/recomposição da trilha |
| 42 | `app/chamados/models.py:1907`, `:1908` — `DisputaSupervisao.sem_veredito_entre` | I,T | **B** — `I or T`; sem validação/materialização; tranca conforme parâmetro | Q13 Retenção por resposta sem veredito |
| 43–45 | `app/chamados/models.py:1889` — `respostas_sem_veredito`; `:1917` — `pode_fechar`; `:1950` — `fechar` | I,T, delegados | **B**, nos três — consultam a mesma lista; `pode_fechar` também exige ausência de fio vivo | Q13 Retenção por resposta sem veredito |
| 46 | `app/chamados/models.py:1990` — `DisputaSupervisao.fechar` | T | **E** — copia texto para ato `SILENCIO` | Q15 Preservação/recomposição da trilha |
| 47 | `app/chamados/models.py:2122` — `DisputaSupervisao.remover_motivo` | T | **E** — copia texto para ato `RETRATACAO` | Q15 Preservação/recomposição da trilha |
| 48 | `app/chamados/models.py:2922`, `:2923` — `PerguntaDisputa._nascer` | I,T | **E** — recusa nascimento com `I ∧ ¬T` | Q10 Integridade do par — técnica |
| 49 | `app/chamados/models.py:3128`, `:3131` — `PerguntaDisputa.estado` | I | **C** — pendente/respondida pelo instante, após precedências de validação/vitalidade/materialização | Q19 Estado apresentado |
| 50 | `app/chamados/models.py:3210`, `:3213`, `:3214` — `PerguntaDisputa.carimbar` | I,T | **E** — guarda foto anterior; só completa instante se houver texto e faltar instante | Q10 Integridade do par — técnica |
| 51 | `app/chamados/models.py:3343`, `:3351`, `:3354` — `PerguntaDisputa.descarimbar_em_lote` | I,T | **E** — consulta metade não recebida por `.values_list()` e recusa par resultante ilegal | Q10 Integridade do par — técnica |
| 52 | `app/chamados/models.py:3454`, `:3468` — `PerguntaDisputa.q_vivas` | I,T, delegados | **A** — degrau de resposta usa `q_respondida_com_texto`; queryset completo combina outros degraus | Q08 Vitalidade |
| 53 | `app/chamados/models.py:3534` — `PerguntaDisputa.save` | I,T | **E** — guarda par carregado depois do save | Q10 Integridade do par — técnica |
| 54 | `app/chamados/models.py:3555`, `:3557` — `PerguntaDisputa._par_lido` | I,T | **E** — lê par de `__dict__` somente se ambas as colunas estiverem carregadas | Q10 Integridade do par — técnica |
| 55 | `app/chamados/models.py:3562`, `:3566` — `PerguntaDisputa._recusa_par_ilegal` | I,T | **E** — compara foto e recusa transição para `I ∧ ¬T` | Q10 Integridade do par — técnica |
| 56 | `app/chamados/models.py:3641` — `PerguntaDisputa.respondida` | I | **C** — `respondida_em is not None` | Q19 Estado apresentado |
| 57 | `app/chamados/models.py:3664`, `:3669`, `:3677`, `:3684`, `:3686` — `PerguntaDisputa.resposta_display` | T | **F** — resolve rótulo de posto/escala/opção ou devolve texto | Q20 Exibição da resposta |
| 58 | `app/chamados/models.py:4067`, `:4074`, `:4077` — `DecisaoPergunta.linhas_da_projecao_sem_linha` | I,T | **C** — ato de resposta exige instante; texto é copiado, sem exigir presença; validação tem ramo próprio | Q15 Preservação/recomposição da trilha |
| 59 | `app/chamados/reconciliador.py:365` — `lavrar_sem_turno_aberto` | T | **D** — texto após strip impede lavratura | Q21 Reconciliação de órfãs |
| 60 | `app/chamados/reconciliador.py:448` — `retencao_da_retratacao` | I,T, delegados | **B** — irmãs sem consumo | Q13 Retenção por resposta sem veredito |
| 61 | `app/chamados/reconciliador.py:1083` — `veredito_orfa` | T | **D** — texto após strip impede o ramo | Q21 Reconciliação de órfãs |
| 62 | `app/chamados/reconciliador.py:1164` — `encerrar_orfas_pela_lampada` | I,T, delegados | **B** — lista sem veredito | Q13 Retenção por resposta sem veredito |
| 63 | `app/chamados/relatorios_disputa.py:24`, `:26` — `classificar` | I,T, delegados | **E** — C7 primeiro; depois existência de viva com `¬I ∨ ¬T` distingue colaborador/supervisão | Q22 Cobrança/dono da espera |
| 64 | `app/chamados/services/acoes_disputa.py:95` — `acao_reabrir_disputa` | I | **C** — conta instante nulo | Q02 Correção/reenvio |
| 65 | `app/chamados/services/acoes_disputa.py:130` — `acao_regularizar_disputa` | I | **C** — alguma viva sem instante impede regularizar | Q23 Entrada da regularização |
| 66 | `app/chamados/services/acoes_disputa.py:353`, `:359` — `acao_corrigir_resposta` | T | **E** — preserva texto anterior em nota de correção | Q15 Preservação/recomposição da trilha |
| 67 | `app/chamados/services/becos.py:30` — `classificar` | I | **C** — instante nulo e destino não resolvido | Q21 Reconciliação de órfãs |
| 68 | `app/chamados/services/cobranca.py:237`, `:241` — `_calados` | I,T, delegados | **E** — viva que espera texto: `¬I ∨ ¬T`; disputa aberta | Q22 Cobrança/dono da espera |
| 69 | `app/chamados/services/competencia_trancada.py:126` — `medir` | I,T, delegados | **B** — sem veredito ignorando tranca | Q13 Retenção por resposta sem veredito |
| 70 | `app/chamados/services/declaracao_texto.py:417` — `declarar_hora_colab` | I,T | **E** — instante verdadeiro e texto exatamente igual à hora solicitada | Q06 Slot/materialização |
| 71 | `app/chamados/services/declaracao_texto.py:419` — `declarar_hora_colab` | I | **C** — `bool(instante)` informa reenvio ao escritor | Q02 Correção/reenvio |
| 72 | `app/chamados/services/declaracao_texto.py:687` — `aplicar_declaracao` | I | **C** — ausência de instante dispara gravação de resposta | Q23 Entrada da regularização |
| 73 | `app/chamados/services/disputa_emissao.py:1635`, `:1646` — `reabrir_para_correcao` | I,T | **E** — cria foto se texto OU instante OU validação OU materialização; exclui fora do fato | Q15 Preservação/recomposição da trilha |
| 74 | `app/chamados/services/disputa_emissao.py:1663` — `responder_automaticamente` | T | **D** — texto após strip já existente impede resposta automática | Q24 Resposta automática |
| 75 | `app/chamados/services/disputa_emissao.py:1691` — `carimbar_resposta_pela_trilha` | I,T | **E** — só age com `¬I ∧ T` | Q17 Diagnóstico/cura de limbo |
| 76 | `app/chamados/services/disputa_emissao.py:1734` — `recobrar_questionario` | I | **C** — conta instante nulo | Q22 Cobrança/dono da espera |
| 77 | `app/chamados/services/disputa_emissao.py:1842` — `reabrir_credor_de_pergunta_viva` | I | **C** — instante presente exclui esta porta; também exige viva | Q11 Alcance/porta |
| 78 | `app/chamados/services/disputa_emissao.py:1860` — `reabrir_credor_de_pergunta_viva` | I,T | **E** — carga contém limbo se `¬I ∧ T` | Q17 Diagnóstico/cura de limbo |
| 79 | `app/chamados/services/disputa_emissao.py:2000` — `_resolver_perguntas_por_chamado_resolvido` | I,T | **A** — par com texto não materializado de fato é preservado | Q13 Retenção por resposta sem veredito |
| 80 | `app/chamados/services/disputa_emissao.py:2030` — mesma função | I,T, delegados | **B** — irmãs sem consumo protegem pai | Q13 Retenção por resposta sem veredito |
| 81 | `app/chamados/services/disputa_emissao.py:2078` — mesma função | T | **D** — `_tem_resp = bool(strip(texto))` | Q13 Retenção por resposta sem veredito |
| 82 | `app/chamados/services/disputa_emissao.py:2073`, `:2090` — mesma função | I | **E** — fotos antes/depois para auditoria | Q15 Preservação/recomposição da trilha |
| 83–84 | `app/chamados/services/explicador.py:32` — `_explicar_validada`; `:79`, `:80` — `explicar_pergunta` | T | **F**, em ambos — texto compõe explicação ao usuário | Q20 Exibição da resposta |
| 85 | `app/chamados/services/fila_por_causa.py:226` — `_gaveta_de` | I,T, delegados | **B** — existência de resposta sem veredito seleciona gaveta | Q04 Fila de validação |
| 86 | `app/chamados/services/fio.py:174` — `aguarda_o_colaborador` | I,T, delegados | **E** — existe viva dentro do recorte `¬I ∨ ¬T` | Q22 Cobrança/dono da espera |
| 87 | `app/chamados/services/fio.py:187` — `espera_o_texto_do_colaborador` | I,T | **E** — `¬I or respondida_sem_texto(p)`, equivalente ao recorte `¬I ∨ ¬T` | Q22 Cobrança/dono da espera |
| 88 | `app/chamados/services/fio.py:357`, `:378` — `montar_chamados_fio` | I,T, delegados | **E** — deriva `resposta_enviada` e `todas_resp` pelos dois leitores anteriores | Q22 Cobrança/dono da espera |
| 89 | `app/chamados/services/fio.py:698` — `processar_post_fio` | I | **C** — alguma viva sem instante impede regularização | Q23 Entrada da regularização |
| 90–91 | `app/chamados/services/fio.py:1271` — `canonica_do_grupo`; `:1283` — `merito_perdedor` | T | **D**, em ambos — texto após strip; segundo também exige via admin | Q16 Deduplicação/mérito |
| 92 | `app/chamados/services/fio.py:1602` — `recusa_respondida_sem_texto` | I,T | **E** — recusa `I ∧ ¬T` pelo predicado delegado | Q10 Integridade do par — técnica |
| 93 | `app/chamados/services/foto_e4b.py:179`, `:180` — `nascimentos` | I | **E** — transição antes não nulo/depois nulo em qualquer assinatura listada | Q18 Ressuscitação |
| 94 | `app/chamados/services/historico.py:99`, `:105`, `:106` — `_eventos_dos_chamados` | I | **C** — `getattr(p, campo)` seleciona evento pelo instante e marco `desde` | Q25 Linha do tempo |
| 95 | `app/chamados/services/historico.py:109` — mesma função | T | **F** — texto truncado ou “sem resposta” no evento | Q25 Linha do tempo |
| 96 | `app/chamados/services/janela_colab.py:63` — `chamados_visiveis_colab` | I,T, delegados | **A** — degrau de resposta herdado de `q_vivas`, dentro da consulta abrangente | Q11 Alcance/porta |
| 97 | `app/chamados/services/materializacao.py:280` — `_tipo_pelo_marco` | T | **E** — converte resposta em minutos para identificar marco/tipo | Q06 Slot/materialização |
| 98 | `app/chamados/services/materializacao.py:359`, `:373`, `:412` — `_ts_tipo_do_plantio` | T | **E** — escape/default do relógio, régua e parse da hora | Q06 Slot/materialização |
| 99 | `app/chamados/services/materializacao.py:438` — `estado_aplicacao_pergunta` | T | **D** — exige texto após strip, além de tipo hora | Q06 Slot/materialização |
| 100 | `app/chamados/services/materializacao.py:474` — `_declarar_ausencia_de_pergunta` | T | **E** — conteúdo da resposta determina ausência/opção | Q26 Escape/ausência/folga |
| 101 | `app/chamados/services/materializacao.py:571` — `_materializar_ausencia_de_resposta` | T | **E** — copia texto para ato `ABSORCAO` | Q15 Preservação/recomposição da trilha |
| 102 | `app/chamados/services/materializacao.py:633` — `perguntas_absorvidas_por` | T | **E** — SQL `resposta_colab__in=escapes` | Q26 Escape/ausência/folga |
| 103 | `app/chamados/services/materializacao.py:674`, `:675` — `devolver_absorvidas_da_ausencia_rejeitada` | I,T | **E** — foto do estado anterior à devolução | Q15 Preservação/recomposição da trilha |
| 104 | `app/chamados/services/materializacao.py:768` — `_reconciliar_destino_e_clones._fecha` | I,T, delegados | **B** — irmãs sem consumo impedem fecho | Q13 Retenção por resposta sem veredito |
| 105 | `app/chamados/services/materializacao.py:835` — `_materializar_resposta_hora` | T | **D** — sem texto após strip recusa plantio | Q06 Slot/materialização |
| 106 | `app/chamados/services/materializacao.py:1001`, `:1010` — `_materializar_resposta_data_hora` | T | **E** — parse de data/hora; conteúdo também entra no erro/auditoria | Q06 Slot/materialização |
| 107 | `app/chamados/services/materializacao.py:1376` — `materializar_perguntas_validadas_da_disputa` | I | **C** — instante presente, validada e não materializada | Q06 Slot/materialização |
| 108 | `app/chamados/services/painel.py:67` — `marcador_do_placar` | I | **E** — `Max('respondida_em')` para marcador | Q07 Cache/invalidação — técnica |
| 109 | `app/chamados/services/passivo_validado.py:66`, `:83` — `_reversao_de` | I | **E** — `getattr` dirigido por `CAMPOS_REVERSAO`, serializado em ISO | Q15 Preservação/recomposição da trilha |
| 110 | `app/chamados/services/pergunta_no_app.py:53`, `:61` — `perguntas_do_app` | I,T, delegad

[truncado em 20000 de 54902 caracteres]

## 15/11 17:51 · sessao 24efca2a894e · - · - · origem=Ronald (tmux)

| Sítio escritor | Função / valor | `DecisaoPergunta` e transação |
|---|---|---|
| `app/chamados/models.py:3097`, `:3100` | `PerguntaDisputa._nascer`: instancia com `**campos`, salva inserção; recebe o par opcionalmente; defaults `resposta_colab=''`, `respondida_em=NULL` (`:2525`, `:2526`). Recusa instante sem texto antes da escrita (`:2922`). | Mesma transação: `registrar(linhas_da_projecao_sem_linha([p]))` (`:3102`), sob `atomic(savepoint=False)` (`:2924`) e `atomic` de `_sob_contrato` (`:2897`). Atos condicionais `resposta` / `validacao` conforme assinaturas (`:4074`, `:4078`); nascimento pendente produz lista vazia (`:4067`). |
| `app/chamados/services/disputa_emissao.py:1481`, `:1482`, `:1522` | `responder_pergunta_disputa_core`: texto normalizado por `.strip()` (`:1446`), instante `_tz.now()` (`:1476`); `save(update_fields=_campos)`. | Mesma transação `atomic` (`:1517`): ato `RESPOSTA` em `:1525`; também preserva projeção anterior sem linha (`:1480`, `:1525`). |
| `app/chamados/services/disputa_emissao.py:1673`, `:1674`, `:1676` | `responder_automaticamente`: `resposta_colab=texto`, `respondida_em=instante`, `save(update_fields=['resposta_colab','respondida_em'])`; guarda texto existente (`:1663`) e recusa par ilegal (`:1671`). | Mesma transação (`:1675`): ato `RESPOSTA` (`:1677`), mais projeção anterior sem linha (`:1672`). Chamador de foto passa literal não vazio definido em `app/api/foto_recebida.py:25`, chamada `:48`. |
| `app/chamados/services/disputa_emissao.py:1695`, `:1697` | `carimbar_resposta_pela_trilha`: `respondida_em=instante`; apenas com texto e sem instante (`:1691`); `save(update_fields=['respondida_em'])`. | Mesma transação (`:1696`): ato `RESPOSTA` (`:1698`). Instante vem de `nota.criado_em` no comando (`app/chamados/management/commands/curar_perguntas_em_limbo.py:67`). |
| `app/chamados/models.py:3215`, `:3235` | `PerguntaDisputa.carimbar`: `respondida_em=quando`, somente se `resposta` habilitada, texto não vazio e instante ausente (`:3213`, `:3214`); acrescenta campo à lista salva (`:3216`). `quando` recebido ou `now()` (`:3207`). | A porta não registra ato; `save` abre transação própria (`:3532`). A garantia de decisão depende do chamador — ver linhas seguintes. |
| `app/chamados/services/materializacao.py:567` | `_materializar_ausencia_de_resposta`: chama `carimbar(..., resposta='validar')`; pode preencher instante de irmã com texto. | Mesma transação (`:563`): ato `ABSORCAO` (`:569`), condicionado ao sucesso do carimbo (`:568`). |
| `app/chamados/services/materializacao.py:1283` | `_materializar_uma`: chama `carimbar(..., resposta='validar')`; pode preencher instante com texto. | Corpo tem `atomic` (`:1226`) mas não registra decisão própria. Na rota `validar_pergunta`, compartilha transação externa (`app/chamados/services/validacao.py:450`) com ato `VALIDACAO` (`:460` → `:164`) quando unidade chamada em `:488`. O lote posterior é chamado fora desse bloco (`:494`); não há garantia geral de ato nesta escrita. |
| `app/chamados/services/disputa_emissao.py:2007` | `_resolver_perguntas_por_chamado_resolvido`: `carimbar(..., resposta='validar')`. | Nenhum ato próprio de `DecisaoPergunta` no corpo. Transação garantida pelo wrapper `resolver_perguntas_por_chamado_resolvido` (`:1900`); existência de ato incidental em todo chamador: **[nao verificado]**. |
| `app/chamados/services/disputa_emissao.py:2081` | Mesma função: `carimbar(..., resposta='responder')`; somente instante com texto pela porta (`models.py:3213`). | Nenhum ato próprio de `DecisaoPergunta` no corpo; escreve evento de auditoria (`:2084`), que não é decisão. Transação do wrapper (`:1900`). |
| `app/chamados/services/disputa_emissao.py:1609`, `:1648` → `app/chamados/models.py:3363` | `reabrir_para_correcao`: passa `respondida_em=None`, `resposta_colab=''` em `campos`; porta `descarimbar_em_lote` executa `update(..., **outros)` (`models.py:3365`). | Mesma transação (`disputa_emissao.py:1621`): ato parametrizado `ato` (`:1632`), default `reabertura` (`:1576`); `recusa` via `validacao.py:629`. Registra somente linhas das quais retira texto/carimbo/validação/materialização (`:1645`, `:1646`); exclui vias fora do fato na decisão e na escrita (`models.py:3364`). |
| `app/chamados/models.py:1613` → `:3363` | `ChamadoColaborador.ressuscitar_perguntas`: zera somente `respondida_em` do par; não apaga texto; seleção prévia admite texto NULL ou `''` (`:1533`). | Mesma transação (`:1604`): preserva decisões projetadas ainda sem linha (`:1610`). Atos `resposta`/`validacao` condicionais (`:4074`, `:4078`); não cria ato novo de reabertura, podendo registrar nenhum se não há projeção órfã. |
| `app/chamados/services/validacao.py:628` | `recusar_pergunta`: delega zeragem do par a `reabrir_para_correcao`, com `ato='recusa'` (`:629`). | Mesma transação externa (`:623`) e interna da porta (`disputa_emissao.py:1621`), ato `recusa` (`:1632`). |
| `app/chamados/services/materializacao.py:688` | `devolver_absorvidas_da_ausencia_rejeitada`: delega zeragem a `reabrir_para_correcao`. | Mesma transação externa (`:681`), ato default `reabertura` (`disputa_emissao.py:1576`, `:1632`). |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py:69` | `Command.handle`: delega limpeza de texto sem instante à reabertura; só executa após `--apply` (`:64`). | Mesma transação da porta, ato `reabertura` conforme condição da foto (`disputa_emissao.py:1621`, `:1632`, `:1646`). |
| `app/chamados/models.py:1555` | `_universo_da_ressuscitacao`: `_q.respondida_em=None` em cópia (`:1541`). | **Só memória**, sem persistência nem ato desta atribuição (`:1556`). |
| `app/chamados/models.py:3246` | `_desfazer_em_memoria`: restaura `self.respondida_em=respondida` da foto anterior (`:3244`). | **Só memória**, sem persistência; chamado ao recusar ou falhar carimbo (`:3224`, `:3227`, `:3238`). |
| `bin/sombra_mascara_pessoal.py:57`, `:62`, `:64` | Escritor operacional **da cópia sombra**: `setattr(o,c,novo)` e `_base_manager.bulk_update`; tabela inclui `PerguntaDisputa.resposta_colab` (`bin/sombra_regras_pessoais.py:85`). `novo=texto(v)` (`sombra_mascara_pessoal.py:55`); transformação preserva formatos aceitos ou produz marcador/hash (`sombra_regras_pessoais.py:29`). | Nenhum ato próprio da alteração; transação da máscara (`sombra_mascara_pessoal.py:16`). Banco exigido `sombra` (`:10`); não contar como escritor de produção. |

Observações verificadas: `PerguntaDisputa.descarimbar()` só salva os três campos de materialização, sem tocar o par (`app/chamados/models.py:3293`); o `bulk_update` de lavras salva apenas `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`app/chamados/veredito_lavrado.py:237`, `:250`). `devolver_validacao_recusada` conserva a resposta e salva somente `validada_em`/`validada_por` (`app/chamados/services/validacao.py:204`, `:227`), embora registre ato `devolucao` (`:230`).

Script AST inicial executado via stdin, sem arquivo:

```python
import ast, pathlib
fields = {'respondida_em', 'resposta_colab'}

for p in pathlib.Path('.').rglob('*.py'):
    if 'tests' in p.parts or 'migrations' in p.parts:
        continue
    try:
        t = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError):
        continue
    parent = {c: n for n in ast.walk(t)
              for c in ast.iter_child_nodes(n)}

    def fn(n):
        while n in parent:
            n = parent[n]
            if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef)):
                return n.name
        return '<module>'

    for n in ast.walk(t):
        hit = False
        if isinstance(n, (ast.Assign, ast.AnnAssign, ast.AugAssign)):
            targets = n.targets if isinstance(n, ast.Assign) else [n.target]
            hit = any(
                isinstance(x, ast.Attribute) and x.attr in fields
                for target in targets for x in ast.walk(target)
            )
        if isinstance(n, ast.Call):
            name = ast.unparse(n.func)
            if name.endswith((
                '.update', '.create', '.get_or_create',
                '.update_or_create', '.bulk_update', '.save'
            )) or name == 'setattr':
                hit = any(
                    isinstance(x, ast.Constant)
                    and isinstance(x.value, str)
                    and x.value in fields
                    for x in ast.walk(n)
                ) or any(k.arg in fields for k in n.keywords)
        if hit:
            print(f'{p}:{n.lineno} {fn(n)} {ast.unparse(n)}')
```

Foram executados complementos AST para: chamadas com keywords/literais dos campos (achou `dict(...)` e chamada indireta de lote); todas as chamadas `carimbar`, `descarimbar_em_lote`, `reabrir_para_correcao`, `bulk_update`; `setattr`/`getattr` literais e subscripts de escrita; constantes contendo campos e termos SQL de escrita. O primeiro script sozinho **não prova completude**, pois não faz fluxo de dados interprocedural/aliases; a conferência dos sítios indiretos acima foi manual a partir da AST. Nenhum SQL literal de escrita dos campos foi encontrado pelos complementos; SQL gerado dinamicamente arbitrário permanece **[nao verificado]**.

## 15/11 17:51 · sessao dfb8f9bc785a · - · - · origem=Ronald (tmux)

Inventário de testes obtido por AST e conferência das asserções. Os testes não foram executados; resultado em execução: [nao verificado]. [nao verificado]

| Teste / asserção | Predicado efetivamente fixado | Corte/lei citado no próprio teste | [nao verificado]
|---|---|---|
| `app/chamados/tests/test_e3_forma_sql_do_juiz.py:91`, `:95` — `test_face_sem_texto_igual_ao_juiz` / `test_face_com_texto_igual_ao_juiz` | As faces SQL equivalem, em `filter`, `exclude` e negação, a **instante E vazio(strip)** / **instante E texto(strip)**; comparação definida em `:69-80`. | `E3-FORMA-SQL-DO-JUIZ`, aval Ronald 12/11 19:1x, itens 1 e 2, em `:1-15`. |
| `app/chamados/tests/test_e3_forma_sql_do_juiz.py:185`, `:190` | `q_vivas`: instante + brancos não entra pelo degrau da resposta; instante + `' 14:20 '` entra. | Mesma origem `:1-15`; especificação de `q_vivas` em `:160-164`. |
| `app/chamados/tests/test_e3_forma_sql_do_juiz.py:259`, `:260` | Fila de validar inclui par com texto; exclui instante + espaços. | Mesma origem `:1-15`. |
| `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:283`, `:287`, `:289` — `test_RED_retratacao_pergunta_o_MESMO_juiz_do_veredito` | `irmas_respondidas_sem_consumo`: **instante OU texto**; instante sem texto segura, texto sem instante segura, com validação libera. | Docstring `:276-279`: aval `O168-JUIZ-DO-VEREDITO`, literalmente contrapõe antigo E ao atual OU; medição 13/11. |
| `app/chamados/tests/test_e3_10_cura_no_escritor.py:132`, `:133`, `:134`, `:140`, `:141`, `:147` | `respostas_sem_veredito` inclui instante + vazio/espaços/par; impede fechamento; `sem_veredito_entre` inclui instante sem texto. | Docstring da classe `:122-124`: `E3-V8-TEXTO-VAZIO`, 14/11 07:4x, “caracterizacao invertida do E3-10 item 2”. O header antigo `:10-12` não descreve essa caracterização atual. |
| `app/chamados/tests/test_service_validar_puro.py:101`, `:102`, `:103` — `test_05_sem_resposta_nao_valida` | Texto `'17:00'` sem instante é recusado por `validar_pergunta`: `sem_resposta`; fixa necessidade do instante, sem provar sozinho suficiência. Fixture `:49-51`, retirada do instante `:98`. | Header `:1-13`: peça 1 da fatia 2-A, 09/08. Corte específico para esse predicado: [nao verificado]. |
| `app/chamados/tests/test_pergunta_estado.py:59`, `:60` — `test_reabertura_volta_a_ser_viva` | `.estado`: texto `'19:00'` sem instante resulta `pendente`; junto de `:35-37`, par resulta `respondida`. | Comentário `:57`: reabertura limpa instante/validação e mantém texto; sem corte nomeado nesse teste [nao verificado]. |
| `app/chamados/tests/test_o261e_f5_alcance.py:194`, `:196`, `:197`, `:199`, `:200` | Instante sem texto sai dos pendentes do app; tarefa preserva `respondida_sem_texto`; vigia mede fio morto causa `b`, sem porta. | Docstring `:185-189`: aval `E3-V10-DEGRAU-DONO-E-PORTA` 14/11 16:16; explicita diferença entre app e crédito. |
| `app/chamados/tests/test_o261e_f5_alcance.py:378`, `:380`, `:386`, `:388` | Sem resposta: porta app/colab e verbo cobrar; depois de gravar par + decisão (`:381-384`): porta pergunta/admin, validar. | Docstring `:373-374`: `E3 b3`, aval `E3-TRIAGEM-FABLE` 12/11; comentário `:382`: E1. |
| `app/chamados/tests/test_e4b_c6_orm.py:130`, `:132` — `_provar`, exercitado por `test_RED_limbo_misto_fecha_mortas_orfa_e_entrega_motivo_proprio` `:153-156` | Texto sem instante na irmã morta produz recusa `credor_carrega_limbo`, materializa mortas e conserva fechamento do chamado. Fixture: texto `:77`, instante nulo `:86`; assert de materialização `:115`. | Comentário `:154-155`: F1 e S143; header `:1`: c6, admin fecha, S143 recusa só reabertura. |
| `app/chamados/tests/test_e4b_c11_orm.py:121-122` — `test_RED_A6_beneficio_rejeitado_limbo` | Repete caracterização de limbo de c6 pelo caminho de benefício rejeitado: herda classe c6 em `:27`, chama `_provar(limbo=...)` em `:99-101`, com asserts efetivos em `test_e4b_c6_orm.py:130-132`. | Header c11 `:1-4`: provas ORM c11, S143 e C6 reais; nomes F1/S143 no comentário herdado c6 `:154-155`. |
| `test_e4c_f1v6_orm.py` | Arquivo não encontrado na árvore disponibilizada; conteúdo, predicado e origem: **[nao verificado]**. | **[nao verificado]**. |
| `app/chamados/tests/test_e3_item1b.py:94`, `:108`, `:114` | Relatório: viva com instante sem texto → `AGUARDA_COLAB`; par → `AGUARDA_SUPERVISAO`; sem resposta → `AGUARDA_COLAB`. | Header `:1-12`: E3 item1b, LEI-AKITA 13, casos pela regra. |
| `app/chamados/tests/test_e3_item1b.py:124`, `:130`, `:136` | Cobrança: instante sem texto conta como calado; par não conta; morta pelo C6 não conta. | Header `:3-10`: LEI-AKITA 13, item 1(c). |
| `app/chamados/tests/test_e3_item1b.py:154`, `:155`, `:156`, `:162`, `:163`, `:171` | Template do colaborador: instante sem texto não exibe “Resposta enviada”/“Reenviar resposta”; par exibe; contexto derivado pode apagar caixa mesmo com par. | Header `:11-12`: item 1(d); teste de contexto `:165-166`. |
| `app/chamados/tests/test_e3_leitores_fio_disputa.py:131`, `:136`, `:141`, `:146` | `todas_respondidas`: morta sem resposta não fica devendo; viva sem resposta ou instante sem texto fica devendo; viva com par não fica. | Header `:2-11`: `FIO-E0-V4-E3-LEITORES`, aval `CASO-7445-COMPLEMENTO-E3`; C6, C7, `respondida_sem_texto`. |
| `app/chamados/tests/test_e3_leitores_fio_disputa.py:161`, `:167` | API: instante sem texto não pode solicitar correção; com par pode. | Mesma origem `:2-11`; comentário `:149`: API lê mesma derivação. |
| `app/chamados/tests/test_sem_resposta_nao_planta_batida.py:99`, `:104`, `:111`, `:125` | Materialização recusa instante sem texto; par com texto produz batida. Fixação efetiva distingue texto, com os carimbos mantidos nas duas fixtures (`:72-86`). | Aval Ronald 30/09 ~13:3x em `:2`; lei “motor NUNCA fabrica batida” em `:9-10`; medição dos 8.622 carimbos sem texto em `:116-120`. **A frase OU do header `:14-15` diverge dessas asserções.** |
| `app/chamados/tests/test_o261_resposta_vazia.py:85`, `:95`, `:109`, `:113`, `:149`, `:151`, `:153`, `:155` | Escritor não carimba vazio nem espaços; recusa carimbar texto se C6 o daria vivo; contador conta instante sem texto apenas após marco, espaços equivalem a vazio. | Header `:2-17`: `O261-RESPOSTA-VAZIA-COM-LEITOR-VIVO`, aval 29/10 17:2x; comentário `:97-102`: inversão O261-E-F2, mandato F2 e LEI-AKITA 2. |
| `app/chamados/tests/test_e3_10_cura_no_escritor.py:63-69`, `:77`, `:106-107`, `:168-170`, `:176-184`, `:205-212` | Guardas de escrita: resposta automática/nascimento/save/lote recusam transição para instante sem texto; par legal passa; legado ilegal pode alterar outros campos (`:186-197`). | Header `:1-13`: E3-10, aval Ronald 12/11 item 2; classe `:152-155`: E3 item1b item 3, LEI-AKITA 7. |
| `app/chamados/tests/test_reabrir_nao_destroi.py:49-52`, `:58`, `:64`, `:72` | Devolução: zera texto/instante/validação; guarda texto e instante originais na decisão `reabertura`; pendente não fabrica versão; duas devoluções geram uma versão. | Header `:1-5`: `REABRIR-NAO-DESTROI`, aval 21/10 RAIA D, resposta é fato. |
| `app/chamados/tests/test_e3_porta_A_reabrir.py:41-42`, `:51`, `:62-64`, `:82-83` | Reabertura comum zera par/materialização/via; carimbo sozinho deixa ato `reabertura`; fora do fato conserva par e não gera decisão; recusa comum zera par e gera ato `recusa`. | Header `:1-10`: aval `E3-PORTA-DA-RESPOSTA = (A)`, item 2, Single Writer Principle. |
| `app/chamados/tests/test_contract_par_resposta.py:90-95`, `:111-113`; `app/chamados/tests/test_contract_texto_com_carimbo.py:119`, `:124`, `:136`, `:138`, `:166-172` | Contratos estáticos de escrita do par; não provam equivalência dos leitores nem atomicidade por execução. | Primeiro `:1-2`: HX-PAR-RESPOSTA, E2 item 2, 03/09; segundo `:1-5`: BUG121, 12/09, ordem “MESMO atomic”. |

Script efetivamente usado para localizar candidatos Python (a interpretação acima foi conferida lendo os corpos e asserções):

```bash
python3 -B - <<'PY'
import ast, pathlib
for p in pathlib.Path('app').rglob('test*.py'):
    try:
        t = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError):
        continue
    hits = []
    for n in ast.walk(t):
        if isinstance(n, ast.Attribute) and n.attr in {
            'respondida_em', 'resposta_colab'
        }:
            hits.append(n.lineno)
        if isinstance(n, ast.keyword) and n.arg and any(
            x in n.arg.split('__')
            for x in ('respondida_em', 'resposta_colab')
        ):
            hits.append(n.lineno)
        if isinstance(n, ast.Constant) and isinstance(n.value, str) \
                and n.value.split('__')[-1] in (
                    'respondida_em', 'resposta_colab'
                ):
            hits.append(n.lineno)
    if hits:
        print(p, ','.join(map(str, sorted(set(hits)))))
PY
```

Script adicional efetivamente usado para localizar testes dos helpers OR, inclusive arquivos sem acesso direto aos campos: [nao verificado]

```bash
python3 -B - <<'PY'
import ast, pathlib
targets = {
    'sem_veredito_entre',
    'irmas_respondidas_sem_consumo',
    'respostas_sem_veredito',
}
for p in pathlib.Path('app').rglob('test*.py'):
    try:
        t = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError):
        continue
    for n in ast.walk(t):
        if isinstance(n, (ast.Attribute, ast.Name)) and \
                getattr(n, 'attr', getattr(n, 'id', '')) in targets:
            print(p, n.lineno, ast.unparse(n))
PY
```

Limite: esses scripts descobrem candidatos e helpers pelo AST; não fazem resolução de tipos/imports nem provam cobertura exaustiva de predicados exercitados indiretamente. O script central do censo deve incorporar a resolução de aliases solicitada pelo usuário; estes são os scripts auxiliares de investigação de testes. [nao verificado]

## 15/11 17:51 · sessao 834316ebacdd · JUIZ-RESPONDIDA · sonda · origem=Code

1. O universo informado não é vazio: **50292** perguntas, **14790** candidatas em `q_vivas` e **899** com ato `RESPOSTA`. A soma fecha: **8192 + 9626 + 0 + 32474 = 50292**. E3 é zero por diferença; não há linha `CONT E3` na saída. [nao verificado]

2. Tabela consolidada do dado fornecido. Nas quatro colunas booleanas, cada célula apresenta **False / True**; os cruzamentos completos estão na transcrição abaixo. [nao verificado]

| Estado | Situação | Total | validada F/T | materializada F/T | viva F/T | ato RESPOSTA F/T | Fonte |
|---|---|---:|---:|---:|---:|---:|---|
| E1 instante E texto | ABERTA | 4011 | 1100 / 2911 | 974 / 3037 | 3059 / 952 | 3193 / 818 | [nao verificado] |
| E1 instante E texto | ENTREGUE | 4181 | 346 / 3835 | 139 / 4042 | 4014 / 167 | 4124 / 57 | [nao verificado] |
| E2 instante sem texto | ABERTA | 1502 | 214 / 1288 | 2 / 1500 | 1500 / 2 | 1502 / 0 | [nao verificado] |
| E2 instante sem texto | ENTREGUE | 8124 | 703 / 7421 | 111 / 8013 | 7983 / 141 | 8124 / 0 | [nao verificado] |
| E3 texto sem instante¹ | ABERTA | 0 | 0 / 0 | 0 / 0 | 0 / 0 | 0 / 0 | [nao verificado] |
| E3 texto sem instante¹ | ENTREGUE | 0 | 0 / 0 | 0 / 0 | 0 / 0 | 0 / 0 | [nao verificado] |
| E4 nenhum | ABERTA | 19017 | 19017 / 0 | 7244 / 11773 | 11847 / 7170 | 18993 / 24 | [nao verificado] |
| E4 nenhum | ENTREGUE | 13457 | 13457 / 0 | 3808 / 9649 | 10652 / 2805 | 13457 / 0 | [nao verificado] |

¹ E3 deduzido por diferença, sem contagem `CONT` explícita. As somas internas da tabela conferem com os totais e marginais fornecidos; o total com ato é **875 + 24 = 899**. [nao verificado]

3. **As expressões exatas usadas pelo script não são verificáveis:** `sonda_quadro.py` não foi localizado na cópia e seu conteúdo não acompanha a saída fornecida. [nao verificado]

A correspondência lógica dos rótulos é a seguinte, tomando `I = p.respondida_em is not None` e `T = not texto_vazio(p.resposta_colab)`; atribuir essas expressões ao script permanece [nao verificado].

| Estado | Expressão lógica do rótulo | Evidência na cópia |
|---|---|---|
| E1 | `I and T` | Predicado descrito e face SQL `q_respondida_com_texto()` em [app/chamados/juizes.py:1554](/home/ronald/codex_ws/64d09171097ada8e10f851c6358e63ca293d643f/app/chamados/juizes.py:1554) |
| E2 | `I and not T` | Predicado em [app/chamados/juizes.py:1531](/home/ronald/codex_ws/64d09171097ada8e10f851c6358e63ca293d643f/app/chamados/juizes.py:1531); face SQL em [app/chamados/juizes.py:1551](/home/ronald/codex_ws/64d09171097ada8e10f851c6358e63ca293d643f/app/chamados/juizes.py:1551) |
| E3 | `not I and T` | [nao verificado] |
| E4 | `not I and not T` | [nao verificado] |

O juiz define `texto_vazio(t)` como `not (t or '').strip()`. [app/chamados/juizes.py:1525](/home/ronald/codex_ws/64d09171097ada8e10f851c6358e63ca293d643f/app/chamados/juizes.py:1525)

Sobre E3 ter face no juiz: o enunciado informa que o script declara que não; essa declaração não aparece na saída transcrita, e não foi possível conferir o script. [nao verificado]

4. `PKS_VIVAS_ABERTA`: **E2 = 2**, PKs **38251 e 38252**; **E3 = 0**, lista vazia. Total das duas listas: **2**. [nao verificado]

Transcrição integral da saída fornecida, sem nova execução nem verificação em banco ou logs. [nao verificado]

```text
sonda_leitura: sonda_quadro.py [8d93c0f8e754674b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO PerguntaDisputa total 50292 | candidatas q_vivas 14790 | com ato RESPOSTA 899
SOMA_DOS_ESTADOS 50292 TOTAL 50292 FECHA
CONT E1_instante_E_texto ABERTA 4011
CONT E1_instante_E_texto ABERTA validada False materializada False viva True ato False 497
CONT E1_instante_E_texto ABERTA validada False materializada False viva True ato True 428
CONT E1_instante_E_texto ABERTA validada False materializada True viva False ato False 106
CONT E1_instante_E_texto ABERTA validada False materializada True viva False ato True 42
CONT E1_instante_E_texto ABERTA validada False materializada True viva True ato False 24
CONT E1_instante_E_texto ABERTA validada False materializada True viva True ato True 3
CONT E1_instante_E_texto ABERTA validada True materializada False viva False ato False 47
CONT E1_instante_E_texto ABERTA validada True materializada False viva False ato True 2
CONT E1_instante_E_texto ABERTA validada True materializada True viva False ato False 2519
CONT E1_instante_E_texto ABERTA validada True materializada True viva False ato True 343
CONT E1_instante_E_texto ABERTA viva False 3059
CONT E1_instante_E_texto ABERTA viva True 952
CONT E1_instante_E_texto ENTREGUE 4181
CONT E1_instante_E_texto ENTREGUE validada False materializada False viva False ato False 4
CONT E1_instante_E_texto ENTREGUE validada False materializada False viva True ato False 55
CONT E1_instante_E_texto ENTREGUE validada False materializada False viva True ato True 30
CONT E1_instante_E_texto ENTREGUE validada False materializada True viva False ato False 174
CONT E1_instante_E_texto ENTREGUE validada False materializada True viva False ato True 1
CONT E1_instante_E_texto ENTREGUE validada False materializada True viva True ato False 82
CONT E1_instante_E_texto ENTREGUE validada True materializada False viva False ato False 49
CONT E1_instante_E_texto ENTREGUE validada True materializada False viva False ato True 1
CONT E1_instante_E_texto ENTREGUE validada True materializada True viva False ato False 3760
CONT E1_instante_E_texto ENTREGUE validada True materializada True viva False ato True 25
CONT E1_instante_E_texto ENTREGUE viva False 4014
CONT E1_instante_E_texto ENTREGUE viva True 167
CONT E1_instante_E_texto ato_RESPOSTA False 7317
CONT E1_instante_E_texto ato_RESPOSTA True 875
CONT E1_instante_E_texto materializada False 1113
CONT E1_instante_E_texto materializada True 7079
CONT E1_instante_E_texto validada False 1446
CONT E1_instante_E_texto validada True 6746
CONT E1_instante_E_texto viva False 7073
CONT E1_instante_E_texto viva True 1119
CONT E1_instante_E_texto 8192
CONT E2_instante_sem_texto ABERTA 1502
CONT E2_instante_sem_texto ABERTA validada False materializada False viva True ato False 2
CONT E2_instante_sem_texto ABERTA validada False materializada True viva False ato False 212
CONT E2_instante_sem_texto ABERTA validada True materializada True viva False ato False 1288
CONT E2_instante_sem_texto ABERTA viva False 1500
CONT E2_instante_sem_texto ABERTA viva True 2
CONT E2_instante_sem_texto ENTREGUE 8124
CONT E2_instante_sem_texto ENTREGUE validada False materializada False viva True ato False 111
CONT E2_instante_sem_texto ENTREGUE validada False materializada True viva False ato False 562
CONT E2_instante_sem_texto ENTREGUE validada False materializada True viva True ato False 30
CONT E2_instante_sem_texto ENTREGUE validada True materializada True viva False ato False 7421
CONT E2_instante_sem_texto ENTREGUE viva False 7983
CONT E2_instante_sem_texto ENTREGUE viva True 141
CONT E2_instante_sem_texto ato_RESPOSTA False 9626
CONT E2_instante_sem_texto materializada False 113
CONT E2_instante_sem_texto materializada True 9513
CONT E2_instante_sem_texto validada False 917
CONT E2_instante_sem_texto validada True 8709
CONT E2_instante_sem_texto viva False 9483
CONT E2_instante_sem_texto viva True 143
CONT E2_instante_sem_texto 9626
CONT E4_nenhum ABERTA 19017
CONT E4_nenhum ABERTA validada False materializada False viva False ato False 115
CONT E4_nenhum ABERTA validada False materializada False viva False ato True 1
CONT E4_nenhum ABERTA validada False materializada False viva True ato False 7109
CONT E4_nenhum ABERTA validada False materializada False viva True ato True 19
CONT E4_nenhum ABERTA validada False materializada True viva False ato False 11727
CONT E4_nenhum ABERTA validada False materializada True viva False ato True 4
CONT E4_nenhum ABERTA validada False materializada True viva True ato False 42
CONT E4_nenhum ABERTA viva False 11847
CONT E4_nenhum ABERTA viva True 7170
CONT E4_nenhum ENTREGUE 13457
CONT E4_nenhum ENTREGUE validada False materializada False viva False ato False 1221
CONT E4_nenhum ENTREGUE validada False materializada False viva True ato False 2587
CONT E4_nenhum ENTREGUE validada False materializada True viva False ato False 9431
CONT E4_nenhum ENTREGUE validada False materializada True viva True ato False 218
CONT E4_nenhum ENTREGUE viva False 10652
CONT E4_nenhum ENTREGUE viva True 2805
CONT E4_nenhum ato_RESPOSTA False 32450
CONT E4_nenhum ato_RESPOSTA True 24
CONT E4_nenhum materializada False 11052
CONT E4_nenhum materializada True 21422
CONT E4_nenhum validada False 32474
CONT E4_nenhum viva False 22499
CONT E4_nenhum viva True 9975
CONT E4_nenhum 32474
PKS_VIVAS_ABERTA E2_instante_sem_texto 2 38251 38252
PKS_VIVAS_ABERTA E3_texto_sem_instante 0 
pergunta_viva chamada 14790 vezes; segundos 53.1
```

## 15/11 17:50 · sessao 9f06cb853912 · - · - · origem=Ronald (tmux)

Item (d), pronto para consolidar. Os literais abaixo são trechos exatos; não interpretei qual predicado deveria prevalecer.

| Tema | Literal | Fonte |
|---|---|---|
| Lei geral do escritor e trilha | “UM ESCRITOR POR ESTADO, NADA EM BRANCO: campo de estado tem 1 escritor canonico + tripwire; acao critica grava usuario, antes/depois e motivo” | `app/docs/LEIS.md:65` |
| Divisão documental das portas | “Q1 identidade/via admin = models.py (abrir, fechar_por_admin, reabrir_por_competencia; desde a O261-E-F2 a PORTA UNICA DO CARIMBO carimbar/carimbar_identidade/descarimbar” e “Q2 resposta = services/disputa_emissao.py (responder_pergunta_disputa_core e irmas; absorve api/foto_recebida.py:50)” | `app/docs/DOSSIES.md:46` |
| Resposta vazia com instante | “(b) respondida_em com resposta vazia” e “(2) escritor de respondida_em sem resposta (p#36272/36273)” | `app/docs/BACKLOG.md:1053` |
| Predicado sem texto duplicado | “o predicado 'respondida sem resposta' tem copia em `chamados/juizes.py:1020` (dentro do C6) e em `chamados/services/vigia_fio.py::respondida_sem_texto` (:680, o unico do vigia apos o F5) -- um so juiz” | `app/docs/BACKLOG.md:1043` |
| Texto sem instante | “(3) texto sem respondida_em: o juiz segura (models.py:1900-1901, juizes.py:2035) mas validar/recusar exigem carimbo (validacao.py:416-418), nota models.py:736-738 pede ato impossivel” | `app/docs/BACKLOG.md:1478` |
| Instante sem texto: tratamento do passivo | “142 passivas NAO reabrem no app, vao a LISTA DO ADMIN "respondida pelo sistema sem o colab" (L-099) sem zerar respondida_em; col331/#22394 fica; cura na porta segue” | `app/docs/BACKLOG.md:2030` |
| Instante sem texto: encaminhamento | “LU-06 = regra unica = particao_do_app; respondida sem texto e da E3-10-CURA-NO-ESCRITOR” | `app/docs/CORTES.md:438` |
| Face SQL e vazio com espaços | “face SQL de respondida_sem_texto (trim de texto_vazio) troca a forma inline de models.py q_vivas e os 9 sitios respondida_em__isnull; selo de equivalencia com mutante sem trim; catraca pela forma de USO; os 17 do passivo medidos na sombra antes (app do colab ou supra mudou = PARA)” | `app/docs/BACKLOG.md:2035` |
| Duas portas dos campos | “PERGUNTA DE DESENHO (2 portas declaradas Q1+Q2 gravam respondida_em)” e “duas portas Q1+Q2, selo AST por campo + mutante 8o sitio” | `app/docs/BACKLOG.md:1978` |
| Resposta automática sobre validada | “responder_automaticamente (chamados/services/disputa_emissao.py ~1640-1666) so recusa quando ja ha texto; nao olha validada_em; chamador api/foto_recebida.py:48. Sem cura ate o censo” | `app/docs/BACKLOG.md:743` |
| Preservar resposta na reabertura | “**Lei do fato unico: resposta e FATO, e reabrir nao apaga fato.**” e “Reabrir tem de criar versao nova preservando a anterior consultavel no proprio chamado, e "Cobrar resposta" so vale para pendentes.” | `app/docs/BACKLOG.md:1149` |
| Zeragem histórica documentada | “o corpo de `disputa_emissao.py:1285-1310` e UM update (`respondida_em=None, validada_em=None, validada_por=None, resposta_colab=''`) e NAO toca `materializada_em`, `recusa_motivo`, `via_resolucao`.” | `app/docs/BACKLOG.md:1149` |
| Regra de preservação e cobrança | “preservar as respostas e cobrar so a pergunta pendente, com trilha; depois do export” e “PROIBIDO apagar resposta de colaborador; PROIBIDO juiz novo.” | `app/docs/BACKLOG.md:2549` |
| Devolução e contagem de atos | “D04 = ATOS humanos ocorridos (2 validacoes com devolucao no meio = 2); D05 = contrato publico CONSERVA as fases (app compilado nao muda), exigibilidade = campo NOVO aditivo (Expand-Contract, L-127); aguarda_admin com validacao sem efeito segue aguarda_admin ate o efeito” | `app/docs/BACKLOG.md:2032` |
| Devolver ao admin com gravação divergente | “PROIBIDO devolver ao admin resposta cuja validacao grava hora/tipo diferente do declarado” | `app/docs/BACKLOG.md:2027` |
| Instante não representa anuência | “sem o texto do colaborador nao se registra o horario dele, e `respondida_em` nao e anuencia” | `app/docs/BACKLOG.md:1667` |
| Passivo sem texto: testemunha e escritor | “158 passivos sem texto em que a testemunha diz 'espera' e a escrita planta a hora do marco (testemunha x escritor discordam).” | `app/docs/BACKLOG.md:1939` |
| Resposta sem veredito protege o pai | “`ponto/supra_juiz.py::pode_executar_csf` passa a PERGUNTAR a ele em vez de ler `respondida_em` cru: pai/destino seguro so com resposta SEM veredito; validada/materializada fecha normal pela Defesa 1.” | `app/docs/CORTES.md:415` |
| Admin não dá primeira resposta | “LU-18 = nao (admin nao da a primeira resposta pelo colab)” | `app/docs/CORTES.md:450` |
| Encerramento por desligamento, corte anterior | “a porta desligamento/admin_encerra deixa de carimbar via admin_decidiu em pergunta JA respondida (o carimbo diria uma decisao que nao houve -- L-007)” | `app/docs/CORTES.md:393` |
| Encerramento por desligamento, fila posterior | “desligamento.py::executar_desligamento fecha tambem a pergunta respondida sem veredito com via propria de desligamento (molde VIA_COMPETENCIA_TRANCADA), lista dia/pergunta/resposta no chamado desligamento_revisao” | `app/docs/BACKLOG.md:732` |

Não localizei texto específico sobre os quatro estados pedidos em `app/docs/LAPIDES.md`; ausência exaustiva de regra nesse documento: **[nao verificado]**. Em `LEIS.md`, o literal diretamente pertinente que encontrei foi a regra geral L-007 acima, sem equação específica entre instante e texto: **[nao verificado]** quanto à inexistência de outra formulação.

Comandos usados, todos somente leitura:

```bash
rg -n -i \
'respondida_em|resposta_colab|resposta.{0,35}vazi|texto.{0,30}instante|instante.{0,30}texto|devol.{0,50}resposta|resposta.{0,50}devol|descarimb.{0,50}resposta|JUIZ-RESPONDIDA|E4B-C6|E4B-C11|E4C-F1V6' \
app/docs/LEIS.md app/docs/CORTES.md app/docs/LAPIDES.md \
app/docs/BACKLOG.md app/docs/DOSSIES.md

rg -n -i \
'(devolv|devolu|descarimb|reabri|recus).{0,100}(respost|respondid)|(respost|respondid).{0,100}(devolv|devolu|descarimb|reabri|recus)|texto_vazio|sem.texto|vazi.{0,40}respost|respost.{0,40}vazi' \
app/docs/LEIS.md app/docs/CORTES.md app/docs/LAPIDES.md \
app/docs/BACKLOG.md app/docs/DOSSIES.md
```

Também executei consultas mais amplas por `respost|respondid|devol|descarimb|colab` e li faixas numeradas com `nl -ba ... | sed -n ...`; alguns resultados amplos foram truncados na saída da ferramenta, motivo para não certificar ausência. [nao verificado]

## 15/11 17:35 · sessao ef5c0b40f83d · E3-RESTO-R3 · verificador · origem=Code

**REPROVA.** As curas pontuais existem, mas permanece um caminho que publica zero apagando um positivo conhecido, além de relógios internos e contratos documentais contraditórios (`app/folha/vigia_retido_calado.py:44`; `app/folha/fora_da_folha.py:117`; `app/chamados/services/tarefa_do_dia.py:32`).

Identidade Git do hash/merge-base, incidência em produção e smoke nas duas cascas: **[nao verificado]**.

**Achados**

1. **Média — o vigia ainda pode apagar positivo conhecido, antes da soma corrigida.** O lavrador fixa `_agora`, calcula anterior e corrente nesse instante, mas grava outro `timezone.now()` para cada retrato (`app/folha/management/commands/lavrar_retrato_folha.py:49`; `app/folha/management/commands/lavrar_retrato_folha.py:69`; `app/folha/management/commands/lavrar_retrato_folha.py:80`). O vigia aceita somente retratos do mesmo **dia civil local** do mais recente da empresa (`app/folha/vigia_retido_calado.py:44`).

   Caso concreto: mesma empresa, competência anterior gravada às 23:59:59 com um dia sem leitura; corrente às 00:00:01 com zero. A anterior desaparece do universo; a soma corrigida recebe apenas a corrente e publica `dias_sem_leitura_sistema=0`, `cego=0` (`app/folha/vigia_retido_calado.py:47`; `app/folha/vigia_retido_calado.py:77`; `app/folha/vigia_retido_calado.py:78`). Os testes entre retratos usam o mesmo `AGORA` e não exercitam essa fronteira (`app/chamados/tests/test_e3_r3_c14_sem_banco.py:325`). Incidência em produção: **[nao verificado]**.

2. **Média — a passagem do relógio para a borda está incompleta nos caminhos de produção.**

   - A view captura `_hoje`, mas chama `frescor_dos_retratos()` sem fornecer o instante; o leitor consulta outro relógio e escolhe novamente as competências (`app/folha/views_retrato.py:176`; `app/folha/views_retrato.py:208`; `app/folha/retrato_folha.py:1386`). Caso: request iniciado no dia 20, consultando a competência anterior; o frescor executa após a virada para 21 e deixa essa competência fora do universo consultado, podendo suprimir seu aviso (`app/folha/retrato_folha.py:1390`; `app/folha/views_retrato.py:209`).
   - `fora_da_folha(..., agora=...)` carrega a tarefa nesse instante, mas a lista de dias passa por `_dias_que_retem` sem `hoje`; `_dias_sem_decisao` lê `localdate()` internamente (`app/folha/fora_da_folha.py:188`; `app/folha/fora_da_folha.py:193`; `app/folha/fora_da_folha.py:117`; `app/folha/export.py:1238`). Caso: ato iniciado antes da meia-noite, leitura depois dela; um dia sem célula passa a reter por `d < hoje`, embora o juiz da tarefa continue no instante anterior (`app/folha/export.py:1271`).

   O selo c14 não cobre esses relógios: sua lista de alvos não inclui `frescor_dos_retratos`, `_dias_que_retem` ou `_dias_sem_decisao` (`app/chamados/tests/test_e3_r3_c14_sem_banco.py:217`).

3. **Baixa — F6 permanece incompleto; a própria c14 deixou afirmações falsas.**

   - “fila residual nao cria acao” contradiz a publicação de `acao='decidir'` sem ação de instância (`app/chamados/services/tarefa_do_dia.py:32`; `app/chamados/services/tarefa_do_dia.py:705`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:103`).
   - `_fio_do_dia` documenta três retornos, mas também pode devolver `chamado` e `nao_sei` (`app/folha/fora_da_folha.py:77`; `app/folha/fora_da_folha.py:90`).
   - `dono_da_linha` promete comparação “POR INSTANTE”, enquanto compara dias ISO e a c14 recusa `datetime` (`app/folha/retrato_folha.py:474`; `app/folha/retrato_folha.py:490`; `app/folha/retrato_folha.py:462`).
   - O contrato do modelo ainda enumera quatro fios/grupos e diz que `ninguem` aparece como Admin, contrariando o balde atual (`app/folha/models.py:306`; `app/folha/models.py:319`; `app/folha/retrato_folha.py:483`).

4. **Baixa — texto técnico exposto na tela, preexistente no leitor auditado.** Com `tem_retrato=False`, o admin recebe `manage.py lavrar_retrato_folha --apply`, sem uma ação de produto correspondente (`app/templates/folha/_retrato_lista.html:17`; `app/templates/folha/_retrato_lista.html:20`).

**A. F1–F6 e mordida**

| Item | Resultado |
|---|---|
| **F1** | **Curado na fonte.** A exceção registra os itens em `sem_leitura`; a sincronização retém suas pautas. O teste atravessa esse caminho e o mutante remove justamente o registro (`app/ponto/management/commands/pautas_discordantes.py:113`; `app/ponto/services/pautas_dos_discordantes.py:198`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:194`; `app/chamados/tests/e3_r3_c14_mutantes.py:41`). |
| **F2** | **Curado para a mistura descrita.** Soma conhecida e ausência de campo ficam separadas; os mutantes retiram essa preservação. Não resolve a exclusão prévia do achado 1 (`app/folha/vigia_retido_calado.py:61`; `app/folha/vigia_retido_calado.py:77`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:306`; `app/chamados/tests/e3_r3_c14_mutantes.py:53`). |
| **F3** | **Curado no chamado original, incompleto no entorno.** O drawer fornece `hoje.date()` e os leitores alterados propagam o dia. Há testes e mutantes para retirar essa passagem, mas restam os caminhos do achado 2 (`app/colaboradores/views.py:986`; `app/folha/views_retrato.py:228`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:292`; `app/chamados/tests/e3_r3_c14_mutantes.py:42`). |
| **F4** | **Curado estruturalmente na fonte.** Raiz própria e seis alvos relativos substituem o elemento inteiro. Os mutantes retiram raiz/alvo; o teste não executa navegador (`app/templates/colaboradores/calendario_partial.html:39`; `app/templates/colaboradores/calendario_partial.html:43`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:76`; `app/chamados/tests/e3_r3_c14_mutantes.py:33`). Smoke: **[nao verificado]**. |
| **F5** | **Cura funcional real.** A tarefa publica `decidir` e o leitor mostra “Decidir no fio”; os mutantes retiram publicação e tradução. A fundamentação pela LU-01 tem a ressalva de B (`app/chamados/services/tarefa_do_dia.py:705`; `app/folha/retrato_folha.py:1017`; `app/chamados/tests/e3_r3_c14_mutantes.py:36`). |
| **F6** | **Trechos apontados corrigidos, cura documental incompleta.** Numerais, promessa de marcação e comentários de cron têm mutantes; sobrevivem as contradições do achado 3 (`app/folha/retrato_folha.py:164`; `app/folha/fora_da_folha.py:98`; `app/config/crons.py:2261`; `app/chamados/tests/e3_r3_c14_mutantes.py:58`). |

Reexecutei os **18 testes c14** e os **19 mutantes**: controles verdes, mutantes mortos por asserção; as definições e o executor estão em `app/chamados/tests/test_e3_r3_c14_sem_banco.py:68` e `app/chamados/tests/e3_r3_c14_mutantes.py:74`. A suíte de 14.584 testes relatada na entrega: **[nao verificado]**.

**B. `acao='decidir'` versus `None`**

A **LU-01 não determina o valor de `acao`**: determina “residual decidir: dono admin; chamado e rota”. Portanto, ela não obriga `None`, tampouco basta para afirmar que exige `'decidir'` (`app/docs/CORTES.md:433`).

A mudança não é apenas rótulo: `'decidir'` passa a entrar na coleção de ações e no JSON persistido; `None` era descartado dessa coleção (`app/folha/fora_da_folha.py:211`; `app/folha/retrato_folha.py:1248`).

| Escritor/leitor | Efeito |
|---|---|
| `_portas_da_tarefa` → `porta_da_tarefa` | Publica e copia `acao`; somente `resolver_dia` acrescenta o dia do turno (`app/chamados/services/tarefa_do_dia.py:706`; `app/chamados/services/tarefa_do_dia.py:720`). |
| Folha por pessoa/dia | `decidir` entra em `acoes`; `None` não gera ação para os links (`app/folha/fora_da_folha.py:211`; `app/folha/fora_da_folha.py:271`). |
| Lavrador | Copia as ações para `RetratoFolha.linhas` e persiste pela porta do command (`app/folha/retrato_folha.py:1248`; `app/folha/management/commands/lavrar_retrato_folha.py:78`). |
| UI | `decidir` → “Decidir no fio”; ausência de ação com fio admin → “Validar no fio”. Ambos levam a `modal_fio` (`app/folha/retrato_folha.py:1017`; `app/folha/retrato_folha.py:1065`; `app/folha/retrato_folha.py:989`). |
| Drawer | Desenha os links prontos (`app/colaboradores/views.py:987`; `app/templates/folha/_retrato_o_que_fazer.html:72`). |
| Pauta, vigia, furo e beco | Leem fio, rota, desconhecimento ou presença do envelope; a troca de `acao` não altera esses critérios (`app/ponto/services/pautas_dos_discordantes.py:87`; `app/chamados/services/vigia_fio.py:599`; `app/chamados/services/furo_sem_canal.py:98`; `app/escala/services/cadastro_realidade.py:472`). |
| API localizada | Lê o **verbo do chamado**, não `acao` da tarefa (`app/api/views_mensageria.py:395`; `app/api/views_mensageria.py:1152`). APK nativo: **[nao verificado]**. |

Os pins atuais exigem explicitamente o novo valor; isso verifica a implementação escolhida, não demonstra uma determinação da LU-01 (`app/chamados/tests/test_e3_portas_tarefa.py:256`; `app/chamados/tests/test_e3_r3_c12_sem_banco.py:17`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:245`). O comparativo com C17 **não certifica essa equivalência**, porque sua projeção omite `acao` (`app/chamados/management/commands/diff_e3_sombra.py:57`).

**C. Checklist**

1. **Escritores/leitores e significado.** `verbos` nasce da partição do painel; fila e pertinência vêm da alimentação; fio/rota/ação publicados nascem na tarefa (`app/chamados/services/verbos.py:30`; `app/chamados/juizes.py:1573`; `app/chamados/services/tarefa_do_dia.py:706`). `dias_sem_leitura`, `dias_sem_porta` e `datas` são produzidos por dia na Folha, copiados para o retrato e consumidos pelo vigia/tela (`app/folha/fora_da_folha.py:207`; `app/folha/fora_da_folha.py:234`; `app/folha/retrato_folha.py:1243`; `app/folha/vigia_retido_calado.py:61`). As divergências temporais estão nos achados 1–2; o recorte de datas usa limites inclusivos (`app/folha/retrato_folha.py:1342`).

2. **Segundo juiz:** nenhum novo confirmado no delta. A publicação consulta a escada registrada; pauta e vigia leem a porta. A seleção por dia civil de lavra permanece uma regra própria do vigia, com o defeito do achado 1 (`app/chamados/services/tarefa_do_dia.py:703`; `app/ponto/services/pautas_dos_discordantes.py:87`; `app/chamados/services/vigia_fio.py:601`; `app/folha/vigia_retido_calado.py:45`).

3. **Segundo escritor/porta:** nenhum novo confirmado nos caminhos alterados. Pauta continua passando por `escrever`/`marcar_feita_pelo_sistema`, sob `if apply`; falha do espelho só acrescenta testemunho à lista. **Dry por padrão preservado** (`app/ponto/services/pautas_dos_discordantes.py:201`; `app/ponto/services/pautas_dos_discordantes.py:205`; `app/ponto/management/commands/pautas_discordantes.py:54`; `app/ponto/management/commands/pautas_discordantes.py:114`).

4. **Relógio fora da borda:** achados 1–2. Há também `contexto_calendario` chamado sem `agora` e outro `localdate()` na view; efeito dessa segunda leitura no template: **[nao verificado]** (`app/colaboradores/views.py:1949`; `app/colaboradores/services/calendario.py:192`; `app/colaboradores/views.py:1963`).

5. **Crons:** no delta fornecido mudaram comentários. Os papéis continuam coerentes: vigia lê/alarma; lavrador grava métrica somente com `--apply` (`app/config/crons.py:2261`; `app/config/crons.py:2289`; `app/folha/management/commands/vigiar_retido_calado.py:33`; `app/chamados/management/commands/lavrar_furo_sem_canal.py:42`).

6. **Selo/RED:** há mordida das seis curas pontuais. Limitações: C1 é estrutural; C4 não cobre todos os relógios; D2 ignora ação; os pins de `decidir` fixam a escolha sem provar sua autorização normativa (`app/chamados/tests/test_e3_r3_c14_sem_banco.py:60`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:217`; `app/chamados/management/commands/diff_e3_sombra.py:57`; `app/chamados/tests/test_e3_r3_c14_sem_banco.py:111`).

7. **Templates:** nenhum cálculo paralelo de números encontrado nos examinados: barra lê `it.numero`, lista lê `l.dias`; a contagem vem de `len(recortar(...))` (`app/templates/core/_barra_gestao.html:67`; `app/templates/folha/_retrato_lista.html:66`; `app/folha/retrato_folha.py:839`). Ambas as bases carregam HTMX e incluem o drawer; quebra adicional não confirmada, smoke **[nao verificado]** (`app/templates/base.html:19`; `app/templates/base.html:399`; `app/templates/base_app.html:19`; `app/templates/base_app.html:154`). Texto técnico: achado 4.

8. **Docstrings/comentários:** achado 3; F6 não está integralmente saneado (`app/chamados/services/tarefa_do_dia.py:32`; `app/folha/fora_da_folha.py:77`; `app/folha/models.py:319`).

9. **ESMERIL-SEMANTICA:** `lavrado_em` funciona como instante de persistência no escritor e como identificador de rodada por dia civil no vigia — incompatibilidade concreta do achado 1 (`app/folha/management/commands/lavrar_retrato_folha.py:80`; `app/folha/vigia_retido_calado.py:45`). `acao` agora reúne ação exigível de instância e verbo residual do painel, sem atualizar o contrato que negava essa segunda origem (`app/chamados/services/tarefa_do_dia.py:699`; `app/chamados/services/tarefa_do_dia.py:705`; `app/chamados/services/tarefa_do_dia.py:32`).

**VEREDITO: REPROVA.**

## 15/11 17:33 · sessao 1a8cc5f5e33a · - · - · origem=Ronald (tmux)

Achados deste recorte:

- **Baixa — F6 ainda incompleto:** `_fio_do_dia` documenta somente `'admin' | 'colab' | 'nada'`, mas delega a `_fio_da_porta`, que também devolve `'nao_sei'` e o `'chamado'` publicado. Caso: envelope sem rota com `nao_sei=True` devolve `'nao_sei'`; porta de chamado devolve `'chamado'`. `app/folha/fora_da_folha.py:77`, `:82`, `:88`, `:90`; universo publicado em `:38`.
- **Baixa — comentário contradiz o contrato temporal novo:** `dono_da_linha` afirma “POR INSTANTE … nunca por data solta”, mas compara datas ISO contra `_hoje_iso(hoje)`; c14 passa a recusar expressamente `datetime`. `app/folha/retrato_folha.py:474`, `:490`, `:462`. Caso: passar instante falha; passar dia é justamente o caminho aceito. O teste confirma a recusa do instante: `app/chamados/tests/test_e3_r3_c14_sem_banco.py:290`.
- **Baixa — comentário antigo no template tocado:** afirma que falta o núcleo e que o botão nasce desligado, embora o núcleo já implemente o modo e sempre entregue `competencia_rotulo`. `app/templates/colaboradores/calendario_partial.html:7`, `:31`; implementação em `app/colaboradores/services/calendario.py:205`, `:206`, `:841`.
- **Baixa — texto técnico no produto, preexistente:** sem retrato, a tela do admin publica o comando `manage.py lavrar_retrato_folha --apply`. Caso concreto: `tem_retrato=False`. `app/templates/folha/_retrato_lista.html:17`, `:19`, `:20`. Drawer também publica “Datas (ISO)” e explicação do lavrador: `app/templates/folha/_retrato_o_que_fazer.html:43`.

F4: cura real na fonte. Raiz própria em `app/templates/colaboradores/calendario_partial.html:39`; seis links usam `closest [data-calendario-raiz]` e `outerHTML` em `:43`, `:47`, `:51`, `:54`, `:57`, `:60`. A resposta é esse fragmento em `app/colaboradores/views.py:1965`; o drawer processa HTMX após inserir o HTML em `app/templates/core/_drawer_generico.html:33`. As duas bases carregam HTMX e incluem o drawer: `app/templates/base.html:19`, `:399`; `app/templates/base_app.html:19`, `:154`. Quebra adicional das cascas: nenhum achado; smoke de clique [nao verificado].

Executei C1+C7: seis testes verdes; os sete mutantes correspondentes morreram por asserção. Referências dos testes: `app/chamados/tests/test_e3_r3_c14_sem_banco.py:76`, `:93`, `:406`, `:424`, `:438`; mutantes: `app/chamados/tests/e3_r3_c14_mutantes.py:33`, `:35`, `:58`, `:60`, `:62`, `:63`. O teste de navegação é estrutural, sem navegador: `app/chamados/tests/test_e3_r3_c14_sem_banco.py:60`, `:76`.

F6 original: numerais, promessa de marcação e comentários de cron foram corrigidos na fonte, com mordida dos mutantes acima. `app/folha/retrato_folha.py:164`, `:525`; `app/folha/fora_da_folha.py:98`; `app/config/crons.py:2261`, `:2289`.

C5/crons: só comentários mudaram no delta fornecido [nao verificado no histórico Git]. Os papéis continuam coerentes: vigia sem `--apply`, leitura e alarme em `app/folha/management/commands/vigiar_retido_calado.py:25`, `:33`, `:38`; lavra grava `MetricaSnapshot` somente com `--apply` em `app/chamados/management/commands/lavrar_furo_sem_canal.py:42`, `:50`; a medida lê a porta publicada em `app/chamados/services/furo_sem_canal.py:98`, `:99`.

Barra/retrato: nenhum número derivado no HTML examinado. A barra mostra `it.numero` em `app/templates/core/_barra_gestao.html:67`, calculado por `len(recortar(...))` na autoridade em `app/folha/retrato_folha.py:839`; a lista mostra `l.dias` em `app/templates/folha/_retrato_lista.html:66`.

Relógio remanescente: `calendario_ponto` chama `contexto_calendario` sem `agora` em `app/colaboradores/views.py:1949`; este lê `timezone.now()` em `app/colaboradores/services/calendario.py:192`. A view ainda lê `_tz.localdate()` em `app/colaboradores/views.py:1963`, mas o `ctx.hoje` sobrescrito não é consumido nos templates examinados; efeito concreto dessa segunda leitura [nao verificado].

## 15/11 17:33 · sessao 9c59887f7e4e · - · - · origem=Ronald (tmux)

B/F5: cura funcional real, com ressalva de contrato e documentação.

- LU-01 diz apenas “residual decidir: dono admin; chamado e rota”; não fixa `acao=None` nem `acao='decidir'`. Portanto, só essa lei não sustenta acusação de violação pelo novo valor. `app/docs/CORTES.md:433`.
- A alteração existe na produção: publica o verbo residual como `acao` e o leitor traduz `'decidir'` para “Decidir no fio”. `app/chamados/services/tarefa_do_dia.py:699`, `app/chamados/services/tarefa_do_dia.py:705`, `app/folha/retrato_folha.py:1017`.
- **Achado baixo, C8/F6:** continuam afirmações falsas: “fila residual nao cria acao” e “LU-01 muda so o fio residual decidir”, enquanto o residual sem ação de instância passa a publicar `acao='decidir'`. Caso sintético já existe no teste. `app/chamados/services/tarefa_do_dia.py:32`, `app/chamados/services/tarefa_do_dia.py:630`, `app/chamados/services/tarefa_do_dia.py:705`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:103`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:111`.
- **Ressalva C6:** o teste de equivalência com C17 não verifica `acao`: a projeção omite esse campo, embora a Folha agora o consuma. Ele não certifica a troca `None → decidir`; o teste C2 separado é que a fixa. `app/chamados/management/commands/diff_e3_sombra.py:57`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:138`, `app/folha/fora_da_folha.py:211`.
- Rodei os dois testes C2: verdes; três mutantes C2 mortos por asserção. Sem registro persistido desta execução: [nao verificado]. Definição dos mutantes: `app/chamados/tests/e3_r3_c14_mutantes.py:36`.

Mapa dos leitores de `acao`:

| Caminho | `'decidir'` versus `None` |
|---|---|
| Projeção `porta_da_tarefa` | Copia ambos; tratamento especial apenas para `resolver_dia`. `app/chamados/services/tarefa_do_dia.py:720` |
| Folha por pessoa | `'decidir'` entra em `acoes`; `None` é descartado. `app/folha/fora_da_folha.py:211` |
| Folha por dia | Ambos aparecem em `_acao`; apenas valor truthy vai à construção dos links. `app/folha/fora_da_folha.py:271` |
| Lavra do retrato | Persiste as ações recebidas; não reinterpreta. `app/folha/retrato_folha.py:1248`, `app/folha/management/commands/lavrar_retrato_folha.py:78` |
| Links da UI | `'decidir'` gera “Decidir no fio”; sem ação, fio admin cai em “Validar no fio”. `app/folha/retrato_folha.py:1017`, `app/folha/retrato_folha.py:1043`, `app/folha/retrato_folha.py:1065`, `app/folha/retrato_folha.py:989` |
| Drawer | Recebe links prontos e os desenha. `app/colaboradores/views.py:987`, `app/templates/folha/_retrato_o_que_fazer.html:72` |
| Pauta | Lê fio; diferença de `acao` não altera esse leitor. `app/ponto/services/pautas_dos_discordantes.py:87` |
| Vigia | Lê rota, fio e desconhecimento; não `acao`. `app/chamados/services/vigia_fio.py:599` |
| Furo sem canal | Lê rota e desconhecimento. `app/chamados/services/furo_sem_canal.py:98` |
| Beco | Lê presença do envelope. `app/escala/services/cadastro_realidade.py:472` |
| API localizada | Lê verbo da partição do chamado, outra fonte/campo; a troca na tarefa não chega por esses caminhos. `app/api/views_mensageria.py:395`, `app/api/views_mensageria.py:453`, `app/api/views_mensageria.py:1152` |
| APK nativo | Fonte nativa não localizada: [nao verificado]. |

Não encontrei defeito funcional comprovado adicional causado por `acao='decidir'`; a obrigatoriedade desse novo valor por lei permanece [nao verificado].

## 15/11 17:32 · sessao ba66660e8e57 · - · - · origem=Ronald (tmux)

F1: cura real; nenhum novo defeito confirmado no caminho auditado. A falha do espelho acrescenta os itens a `sem_leitura` (`app/ponto/management/commands/pautas_discordantes.py:105–115`); `handle` transmite essa lista (`:133–134`), e `sincronizar` separa as pautas retidas das que fechará (`app/ponto/services/pautas_dos_discordantes.py:194–199`). A escrita permanece sob `if apply`, pelas portas `escrever` e `marcar_feita_pelo_sistema` (`:201–213`); o argumento `--apply` usa `store_true` (`app/ponto/management/commands/pautas_discordantes.py:54`). O teste percorre `handle → apurar → sincronizar`, verifica zero fechamento e uma pauta retida (`app/chamados/tests/test_e3_r3_c14_sem_banco.py:154–205`); o mutante remove o registro que segura a pauta (`app/chamados/tests/e3_r3_c14_mutantes.py:41`). Reexecutei os testes F1/F2: seis verdes; os quatro mutantes correspondentes morreram por asserção, sem erro de infraestrutura [nao verificado: resultado de execução sem arquivo persistido].

F2: cura real para a mistura descrita na triagem. O contador separa linhas sem campo da soma conhecida e conserva positivos por empresa e no total (`app/folha/vigia_retido_calado.py:61–77`). Testes sustentam mistura na mesma empresa, entre empresas e somente legado (`app/chamados/tests/test_e3_r3_c14_sem_banco.py:306–351`); os três mutantes retiram essas curas (`app/chamados/tests/e3_r3_c14_mutantes.py:53–57`).

Achado adicional — gravidade média: **a mesma lavra pode perder uma competência na meia-noite**. O lavrador fixa `_agora` uma vez (`app/folha/management/commands/lavrar_retrato_folha.py:49`), monta anterior e atual nesse instante (`:62–69`), mas grava `lavrado_em=timezone.now()` novamente para cada retrato (`:78–80`). O vigia seleciona somente retratos cujo dia civil local coincide com o mais recente da empresa (`app/folha/vigia_retido_calado.py:44–47`). Caso concreto: anterior gravada às 23:59:59 com um dia sem leitura; atual às 00:00:01 sem dias sem leitura. A anterior desaparece do universo e o contador publica zero com `cego=0` (`:54–78`). Incidência em produção [nao verificado]. A reprodução em memória com o corpo real de `censo` retornou somente a atual, zero e `cego=0` [nao verificado: resultado não persistido]. Os testes c14 usam o mesmo `AGORA` nos retratos e não exercitam essa fronteira (`app/chamados/tests/test_e3_r3_c14_sem_banco.py:325–326`).

Achado documental adicional — gravidade baixa: o contrato do modelo ainda enumera quatro fios e grupos A/B/C/D (`app/folha/models.py:306–308`) e promete que legado `ninguem` aparece como Admin (`:319–321`), embora os comentários/mapas novos descrevam cinco fios e balde próprio sem ação (`app/folha/retrato_folha.py:159–177`).

## 15/11 17:24 · sessao 30c7b5361d1d · TRIAGEM-DA-TRAVA · verificador · origem=Code

**AVAL-DEVOLVIDO — ITEM B.** O item A permanece independente e registrado como vigente. [app/docs/RELATO.md:9208](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/app/docs/RELATO.md:9208)

1. **NAO PROCEDE — as premissas não conferem integralmente.**

   `_GUARDA_MOTIVO` está exatamente em **:236**, mas essa linha declara a regex; a conferência ocorre em **:255**, com recusa rc 2 em **:262**. Hoje verifica a forma do motivo, sem consultar aprovação ou hash. `--dir` existe e escolhe o diretório montado para executar os testes; não certifica sua identidade. [bin/suite.sh:236](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:236), [bin/suite.sh:255](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:255), [bin/suite.sh:136](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:136), [bin/suite.sh:284](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:284)

   O selo existe, contém recusas comportamentais e exige que o mutante `aceita_sem_motivo` seja detectado. Entretanto, seu positivo atual aceita pre-opus sem fornecer prova de aprovação. Execução do selo nesta revisão: **[nao verificado]**. [bin/tests/test_suite_motivo.sh:46](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/tests/test_suite_motivo.sh:46), [bin/tests/test_suite_motivo.sh:61](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/tests/test_suite_motivo.sh:61), [bin/tests/test_suite_motivo.sh:156](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/tests/test_suite_motivo.sh:156)

   **A premissa dos três chamadores está errada:** `volta_intermediaria.sh` chama `suite.sh --dir … --volta …`, atravessando a guarda. Somente `regua.sh` e `pre-push.sh`, entre esses três, executam pela trava própria fora dela. [bin/volta_intermediaria.sh:32](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/volta_intermediaria.sh:32), [bin/regua.sh:165](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/regua.sh:165), [bin/pre-push.sh:142](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/pre-push.sh:142)

   **Já existe escritor de lastro por hash:** `<resposta>.id` registra obra, papel, hash, PID, fim e SHA da resposta. Pode ser lido sem escritor novo, mas comprova identidade e sucesso da chamada, não “zero achados procedentes”. `.rc` é escrito somente para construtor; a triagem existente distingue `procedente|descartado`, sem certificado específico de aprovação limpa. Existência de artefato concreto aprovado em produção: **[nao verificado]**. [bin/codex_pergunta.sh:464](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/codex_pergunta.sh:464), [bin/codex_pergunta.sh:274](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/codex_pergunta.sh:274), [bin/disjuntor.py:869](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/disjuntor.py:869)

2. **PROCEDE — há colisão com uma saída vigente do CODEX.**

   O item 5(d) manda subir ao opus **após mais de três voltas Codex, sem esperar**; o item 5(c) exige suite inteira verde antes desse opus. A guarda literal que exige aprovação prévia para liberar a suite não contempla essa saída: pode impedir o requisito necessário ao encaminhamento obrigatório. [CLAUDE.md:377](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:377)

   Também é preciso preservar a triagem: achados descartados com motivo seguem anexados ao opus, que decide. Exigir uma resposta textual de “APROVA” do Codex seria mais restritivo que essa regra. [CLAUDE.md:375](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:375)

   Motivo obrigatório e chamadas próprias da régua/pre-push são compatíveis com a seção 3 e com o contrato registrado de TRAVA-MENOS-FILA-COM-SELO. O literal original desse aval em `CORTES.md`: **[nao verificado]**; o conteúdo verificável está nas referências seguintes. [CLAUDE.md:101](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:101), [app/docs/RELATO.md:9059](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/app/docs/RELATO.md:9059)

   Contrato de cinco linhas antes da construção confere. “UMA vez” conserva a ressalva vigente: **mudou a forma do contrato, exige nova chamada**. A consolidação CODEX da 7b prevalece sobre blocos antigos da PISTA. [app/docs/PISTA.md:170](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/app/docs/PISTA.md:170), [app/docs/PISTA.md:10](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/app/docs/PISTA.md:10)

3. **PROCEDE — há condições que não fecham no literal.**

   **Delta:** é permitido nas voltas intermediárias. Uma revisão do delta pode produzir lastro para o **novo commit**, passado ao wrapper; a prova do hash anterior não passa automaticamente a valer para ele. Portanto, delta e identidade por hash são compatíveis, mas a herança de aprovação entre hashes não está demonstrada. [CLAUDE.md:366](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:366), [bin/codex_pergunta.sh:79](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/codex_pergunta.sh:79), [bin/codex_pergunta.sh:467](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/codex_pergunta.sh:467)

   **Identidade da árvore:** o sidecar identifica um commit; `--dir` monta conteúdo de um diretório. O fluxo atual não confere que os bytes montados correspondem àquele commit, nem revalida isso depois da espera pela trava. “Hash de `--dir`” precisa fechar essa relação; igualdade efetiva durante a execução: **[nao verificado]**. [bin/suite.sh:284](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:284), [bin/suite.sh:317](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:317)

   **Substituto com rc 75:** a regra permite sonnet com motivo, mas o wrapper sai por limite antes de criar o `.id`. Protocolo existente que grave aprovação desse substituto vinculada ao hash: **[nao verificado]**. O rc 75 da trava significa outra coisa: a vez não chegou. [CLAUDE.md:360](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:360), [bin/codex_pergunta.sh:451](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/codex_pergunta.sh:451), [bin/trava_teste.sh:18](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/trava_teste.sh:18)

   **Contadores:** falta fonte histórica completa demonstrada. A suite remove seu rc temporário; a trava sobrescreve dono/início por “livre”; a volta intermediária grava resultado e duração, sem estado simultâneo da segunda pista. A régua possui carimbo próprio com resultado e horário, mas não cobre todas as chamadas de `suite.sh`. O histórico geral continua registrado como obra separada aberta. Valores dos dois contadores nas 24 horas: **[nao verificado]**. [bin/suite.sh:367](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:367), [bin/trava_teste.sh:40](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/trava_teste.sh:40), [bin/volta_intermediaria.sh:45](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/volta_intermediaria.sh:45), [bin/regua.sh:196](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/regua.sh:196), [app/docs/BACKLOG.md:2581](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/app/docs/BACKLOG.md:2581)

   **Sem `--dir`:** `pre-opus <OBRA>` é uso canônico para a árvore viva, mas B só nomeia “hash de `--dir`”. Deixar passar abre esse caminho; exigir `--dir` muda o comando canônico e esbarra na recusa atual de passar a viva explicitamente. Se for literalmente apenas `"pre-opus"`, sem obra, já retorna rc 2. [CLAUDE.md:96](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:96), [bin/suite.sh:289](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:289), [bin/suite.sh:298](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/suite.sh:298), [bin/tests/test_suite_motivo.sh:48](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/bin/tests/test_suite_motivo.sh:48)

4. **NAO PROCEDE — não há ato fora do declarado demonstrado no escopo B.**

   O registro limita B a guarda, selo e contadores, em instrumento com pouso próprio; não encomenda alteração de exportada, dinheiro ou regra de negócio. Esses atos continuam sujeitos ao `!`, e config do Code permanece proibida. Respeito efetivo aos limites de config, trava e motivo `pouso` pela implementação futura: **[nao verificado]**. [app/docs/BACKLOG.md:1954](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/app/docs/BACKLOG.md:1954), [CLAUDE.md:265](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:265), [CLAUDE.md:350](/home/ronald/codex_ws/a56ea92aa8c8617cac5f7a064a5723d46bf5200f/CLAUDE.md:350)

## 15/11 17:22 · sessao f0dfa0f0ad86 · - · - · origem=Ronald (tmux)

- **Há escritor existente de identidade por hash**, mas existência de aprovação concreta em produção é **[nao verificado]**. O wrapper resolve o argumento como commit completo (`bin/codex_pergunta.sh:79`), arquiva esse commit (`bin/codex_pergunta.sh:311`) e, se o processo retorna 0 e produz resposta nova, grava `<resposta>.id` com obra, papel, hash completo, pid, fim e SHA da resposta (`bin/codex_pergunta.sh:464`). Esse sidecar já pode ser lido sem novo escritor; não constitui sozinho aprovação sem achados: o wrapper só testa sucesso do processo e presença da resposta (`bin/codex_pergunta.sh:428`, `bin/codex_pergunta.sh:466`).

- **`.rc` não é prova atual do verificador.** O arquivo `<resposta>.rc` só é escrito para `PAPEL=construtor` (`bin/codex_pergunta.sh:274`). `.r` é convenção de nome do arquivo de resposta fornecido pelo chamador; o wrapper aceita `RESPOSTA="$3"` (`bin/codex_pergunta.sh:67`).

- **Existe outro escritor registrado, mas não de aprovação sem achados:** `disjuntor.py --triagem` aceita sidecar de verificador, confere SHA e grava a triagem (`bin/disjuntor.py:892`, `bin/disjuntor.py:910`). Seus vereditos são `procedente|descartado` (`bin/disjuntor.py:139`); “procedente” significa achado procedente, não resposta “0 procedentes”, conforme CODEX item 5(b) (`CLAUDE.md:375`).

- **rc75 não deixa aprovação substituta nesse protocolo.** O wrapper registra limite e sai 75 antes da criação do sidecar (`bin/codex_pergunta.sh:451`, `bin/codex_pergunta.sh:459`, `bin/codex_pergunta.sh:464`). A lei permite leitura sonnet com rc75/falha e motivo no RELATO (`CLAUDE.md:360`; `app/docs/PISTA.md:94`), mas escritor que transforme a decisão desse substituto em aprovação verificável por hash é **[nao verificado]**.

- **Delta não contradiz necessariamente hash igual.** A pergunta de volta intermediária pode conter só delta (`CLAUDE.md:366`); passando o novo commit ao wrapper, o sidecar identifica o novo hash (`bin/codex_pergunta.sh:79`, `bin/codex_pergunta.sh:467`). Reutilizar prova de hash anterior para árvore modificada não é sustentado por esse mecanismo (`bin/codex_pergunta.sh:467`).

- **“Hash de `--dir`” precisa de definição.** Hoje `--dir` é apenas caminho (`bin/suite.sh:136`), montado como diretório (`bin/suite.sh:284`). Não há nesse caminho conferência HEAD/limpeza entre diretório e commit; a porta `--montagem` monta assets/cache, sem esse vínculo (`bin/arvore_do_push.sh:28`). Cópia archive pode nem ter `.git`, pois nasce por `git archive` (`bin/codex_pergunta.sh:315`). Sem `--dir`, roda a árvore viva por default (`bin/suite.sh:23`, `bin/suite.sh:297`); o literal “hash de --dir” não define esse caso **[nao verificado]**.

- **Fonte histórica suficiente para os dois contadores não foi demonstrada.** A suite executa testes sem gravar evento persistente de seleção/hash/data (`bin/suite.sh:317`); o rc temporário é removido (`bin/suite.sh:338`, `bin/suite.sh:367`). A trava grava dono+pid e início, sobrescrevendo com “livre” na saída; não guarda histórico nem pista viva (`bin/trava_teste.sh:23`, `bin/trava_teste.sh:40`). A volta intermediária grava OUT.log/.fim e duração, mas não estado da segunda pista (`bin/volta_intermediaria.sh:32`, `bin/volta_intermediaria.sh:42`). O ESTADO lê atualmente chamadas/limites Codex (`bin/gerar_estado.py:267`; `bin/codex_limite_estado.py:49`), não esses eventos de suite.

- **Segunda pista é externa ao repo.** O contrato registrado diz “servidor do GLPI; nenhum arquivo do repo”, com estado consultado por `ssh pista estado` (`app/docs/BACKLOG.md:2039`). Há fotografias textuais de 10:22 e 11:3x (`app/docs/RELATO.md:161`, `app/docs/RELATO.md:88`); não provam estado simultâneo a cada volta nem fornecem histórico completo dos contadores. Valores efetivos nas 24h são **[nao verificado]**.

## 15/11 17:22 · sessao f1505d748fb8 · E4C-F1V3 · leitura · origem=Code

**D1+J1 apontam uma candidata a cura: C12/desligamento. Os três vermelhos de limbo de C6/C11 continuam vermelhos, pois suas próprias asserções exigem texto sem instante vivo. O verde final de C12 não foi verificado por execução.** (`chamados/tests/test_e4b_c12_orm.py:179`; `chamados/tests/test_e4b_c6_orm.py:82`; `chamados/tests/test_e4b_c11_orm.py:122`.)

Todos os caminhos abaixo são relativos a `app/`. Não achei `.git` nesta árvore; `git diff 151b0666a..d36ecbddd` falhou com “not a git repository”. Procurei também arquivos de diff e fotografias históricas. A fotografia encontrada identifica outro BASE, `97bbaa60`, e não substitui o main solicitado. Portanto, **a lista completa dos sítios alterados por c2..c9 e sua comparação literal com main ficam [nao verificado]**. (`chamados/tests/e4c_f1v5_base_juizes.txt:1`.)

**(a) Escritores e leitores encontrados na cópia.**

Os escritores abaixo permitem rastrear os valores consumidos pelos sítios seguintes; a presença e o comportamento desses escritores no main são [nao verificado].

| Escritor na cópia | Texto e instante escritos |
|---|---|
| `responder_pergunta_disputa_core` | Escreve `resposta_colab=resposta` e `respondida_em=_agora`; registra RESPOSTA com os mesmos texto e instante. (`chamados/services/disputa_emissao.py:1481`; `chamados/services/disputa_emissao.py:1525`.) |
| Escrita de resposta por texto | Escreve `resposta_colab=texto`, `respondida_em=instante` e a RESPOSTA correspondente. (`chamados/services/disputa_emissao.py:1673`; `chamados/services/disputa_emissao.py:1677`.) |
| Recuperação pela trilha | Preenche `respondida_em=instante`; registra RESPOSTA usando o texto já existente na projeção. (`chamados/services/disputa_emissao.py:1695`; `chamados/services/disputa_emissao.py:1698`.) |
| `PerguntaDisputa.carimbar` | Com `resposta` solicitado e texto preenchido, preenche o instante ausente; com `'validar'`, também preenche validação. (`chamados/models.py:3260`.) |
| `reabrir_para_correcao` | Registra a foto anterior e zera texto e instante da projeção. (`chamados/services/disputa_emissao.py:1609`; `chamados/services/disputa_emissao.py:1632`.) |
| Ressuscitação | Registra REABERTURA com texto/foto anteriores; depois zera `respondida_em` e validação, conservando o texto. (`chamados/models.py:1655`; `chamados/models.py:1660`.) |
| Conservação do legado | Converte projeção com instante sem linha correspondente em payload RESPOSTA, copiando texto e instante. (`chamados/models.py:4121`.) |
| `DecisaoPergunta.registrar` | Persiste os payloads recebidos por `bulk_create`; `ato_em` e `ato_texto` são campos próprios da decisão. (`chamados/models.py:4063`; `chamados/models.py:4065`; `chamados/models.py:4143`.) |

Na tabela seguinte, “texto” significa texto após `strip()`. “Não classifica” significa que o sítio transporta ou interpreta valores, sem decidir sozinho que a pergunta está respondida. A condição de cada sítio **no main** permanece [nao verificado], pela ausência do diff.

| Sítio leitor na cópia | Fonte e predicado efetivo |
|---|---|
| `atos_pergunta.leitura` | RESPOSTA fornece `ato_texto` e `ato_em`; RECUSA/REABERTURA limpam ambos; DEVOLUÇÃO limpa somente validação. Não exige texto para transportar o instante. (`chamados/services/atos_pergunta.py:46`; `chamados/services/atos_pergunta.py:52`; `chamados/services/atos_pergunta.py:56`.) |
| `_leitura_conservada`, objeto sem relação/PK | Transporta diretamente texto e instante projetados, recortando instante futuro. Não classifica. (`chamados/juizes.py:296`; `chamados/juizes.py:307`.) |
| `_leitura_conservada`, objeto persistido | Combina atos reais e conservação R/V; pode conservar texto legado **sem instante**, sem fabricar decisão. (`chamados/juizes.py:321`; `chamados/juizes.py:336`; `chamados/juizes.py:352`.) |
| `_leitura_da_pergunta` | Obtém os atos por carga, prefetch ou relação e entrega à conservação. Não acrescenta predicado de respondida. (`chamados/juizes.py:362`; `chamados/juizes.py:375`.) |
| `_slot_esperado`, `hora` | Usa texto dos atos pelo embrulho; não exige o par completo, embora impeça resposta posterior ao horizonte de fornecer slot. (`chamados/juizes.py:500`; `chamados/juizes.py:525`.) |
| `_slot_esperado`, `data_hora` | Interpreta diretamente `atos.resposta`; não exige instante de resposta. (`chamados/juizes.py:512`.) |
| `_resposta_concorda_com_a_luz` | Lê **texto** da leitura dos atos para comparar a hora. (`chamados/juizes.py:1511`.) |
| `projecao_temporal_da_pergunta` | Transporta instante e texto da leitura, além de validação/materialização/via. Não classifica. (`chamados/juizes.py:1539`; `chamados/juizes.py:1550`.) |
| `_pergunta_viva_calc`, degrau da resposta | Aplica **só texto**; o instante retornado em `respondida` não participa desse `if`. (`chamados/juizes.py:1554`; `chamados/juizes.py:1572`.) |
| `respondida_sem_texto` | Aplica **instante E ausência de texto**. É detector do estado ilegal, não o predicado positivo D1. (`chamados/juizes.py:1683`.) |
| `q_respondida_com_texto` | RESPOSTA corrente com texto, acrescida da conservação; esta inclui legado **com texto e sem instante**, quando não há ato terminal. Portanto, não implementa D1 estrito. (`chamados/juizes.py:1715`; `chamados/juizes.py:1737`; `chamados/juizes.py:1759`.) |
| `PerguntaDisputa.q_vivas` | Consome aquela face SQL como ramo da resposta; continua sendo prefiltro de vitalidade. (`chamados/models.py:3500`; `chamados/models.py:3515`.) |
| `perguntas_corrigiveis` | Aplica **instante OU texto**, mais a proteção de lastro/vitalidade. (`chamados/juizes.py:1921`.) |
| `perguntas_respondidas_a_validar` | Filtra pela face SQL anterior e pelo C6; ordena pelo instante da leitura. Assim, admite o legado sem instante conservado pela face SQL. (`chamados/juizes.py:1929`; `chamados/juizes.py:1932`.) |
| `declarou_hora` | Interpreta **só texto** dos atos, excluindo escapes e descartados. (`chamados/juizes.py:1958`.) |
| `delta_da_resposta` | Interpreta **só texto** dos atos para calcular delta. (`chamados/juizes.py:2105`.) |
| `classificar_resposta_validacao` | Em escolha, interpreta **só texto**; em hora, delega ao delta. (`chamados/juizes.py:2209`; `chamados/juizes.py:2216`.) |
| `motivo_implausivel_validacao` | Interpreta **só texto** dos atos para comparar com o marco. (`chamados/juizes.py:2232`.) |
| `validar_pergunta` | Seu gate inicial exige **só `respondida_em` projetado**. Não achei teste de texto nesse gate. (`chamados/services/validacao.py:418`.) |
| `_item_validar`, mensageria | Transporta **instante dos atos**, mas ainda transporta **texto da projeção**; aceita instante nulo. (`api/views_mensageria.py:564`; `api/views_mensageria.py:574`; `api/views_mensageria.py:576`.) |
| Cargas de leitura | Transportam `ato_em`/`ato_texto`, conservações e snapshots; não definem respondida. (`chamados/services/carga_pergunta_viva.py:80`; `chamados/services/carga_pergunta_viva.py:88`; `chamados/services/carga_pergunta_viva.py:258`; `chamados/services/carga_pergunta_viva.py:281`.) |
| Fio, app e relavra | Alimentam a leitura dos atos antes de consultar o juiz; não criam outro predicado positivo nesse carregamento. (`chamados/services/fio.py:255`; `chamados/services/pergunta_no_app.py:66`; `chamados/services/relavra_via.py:235`.) |

Há ainda leitores relevantes para a interpretação de D1: a partição do app usa **só instante projetado**, enquanto `sem_veredito_entre` usa **instante OU texto projetados**. Não posso afirmar que esses sítios pertençam ao diff c2..c9. (`chamados/services/pergunta_no_app.py:87`; `chamados/models.py:1952`; pertença ao diff [nao verificado].)

**(b) Veredito por teste dos anexos.** [nao verificado]

Os estados vermelhos/erros são os informados nos anexos. As setas abaixo são previsões por inspeção, com execução D1+J1 **[nao verificado]**. “?” significa que não há evidência para determinar o resultado posterior.

| Teste | c9 → D1+J1 | Motivo e asserção | [nao verificado]
|---|---|---|
| `test_v8_R11_um_instante_ate_o_app`, máquina 2026-07-25 | erro → ? | Fixture sem resposta; D1 não aponta mudança desse ramo. Não achei traceback no dado fornecido. Asserções: `chamados/tests/test_e3_portas_tarefa.py:634`; fixture: `chamados/tests/e3_v8_apoio.py:167`. |
| Mesmo teste, máquina 2027-01-21 | erro → ? | Mesmo limite de evidência e mesmas asserções. (`chamados/tests/test_e3_portas_tarefa.py:634`; `chamados/tests/test_e3_portas_tarefa.py:636`.) |
| `test_e1_v8…test_BUG113_resposta_nova_reabre_a_MESMA_ausencia_e_reabsorve` | vermelho → vermelho previsto | Herda o teste de ausência; a resposta nova escreve o par completo e `ATESTADO` não deriva slot. (`chamados/tests/test_e1_v8.py:118`; `chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:163`; `chamados/services/disputa_emissao.py:1481`; `chamados/services/materializacao.py:359`.) |
| `test_o261_e1_ausencia_rejeitada_devolve…test_BUG113_resposta_nova_reabre_a_MESMA_ausencia_e_reabsorve` | vermelho → vermelho previsto | Mesma cadeia; não identifiquei cura por D1/J1 nas verificações de reabertura e reabsorção. (`chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:165`; `chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:169`.) |
| `test_determinismo_juiz_antes_e_depois_do_ato_recusado` | vermelho → vermelho previsto | Fixture com par completo; D1 mantém sua condição verdadeira. Asserções: `chamados/tests/test_pvmuda_carimbo_em_voo.py:128`; fixture: `chamados/tests/test_pvmuda_carimbo_em_voo.py:64`. |
| `test_endpoint_do_app_filtra_por_q_vivas` | **vermelho → vermelho** | O teste exige exatamente um filtro de compreensão; a cópia possui também o filtro de carregamento por tranca. D1/J1 não o removem. (`chamados/tests/test_contract_pergunta_viva.py:82`; `chamados/services/pergunta_no_app.py:65`; `chamados/services/pergunta_no_app.py:67`.) |
| `test_idempotente_validar_2x_a_recusada_mesmo_estado_uma_trilha` | vermelho → vermelho previsto | Par completo; DEVOLUÇÃO preserva resposta e instante, alterando validação. (`chamados/tests/test_o261_recusa_fala.py:67`; `chamados/tests/test_o261_recusa_fala.py:158`; `chamados/services/atos_pergunta.py:56`.) |
| `test_irmas_que_gravaram_ficam_como_estao` | vermelho → vermelho previsto | Mesmo cenário com par completo; nenhuma cura específica identificada nas verificações das irmãs. (`chamados/tests/test_o261_recusa_fala.py:67`; `chamados/tests/test_o261_recusa_fala.py:124`; `chamados/tests/test_o261_recusa_fala.py:133`.) |
| `test_propriedades_sob_intercalacao` | vermelho → ? | A máquina de estados pode responder novamente escrevendo só projeção após recusa. Sem o traço reduzido do Hypothesis, não se pode assumir igualdade entre ato e projeção para J1. (`chamados/tests/test_pvmuda_carimbo_em_voo.py:237`; `chamados/tests/test_pvmuda_carimbo_em_voo.py:243`; asserções: `chamados/tests/test_pvmuda_carimbo_em_voo.py:276`; `chamados/tests/test_pvmuda_carimbo_em_voo.py:289`.) |
| `test_recusa_da_porta_do_ponto_na_porta_nova_nao_reabre_a_disputa_e_deixa_trilha` | vermelho → vermelho previsto | Fixture com par completo; materializador substituído por mock de recusa. D1/J1 não corrigem essa devolução. (`chamados/tests/test_fio_le_o_juiz.py:167`; `chamados/tests/test_fio_le_o_juiz.py:251`; `chamados/tests/test_fio_le_o_juiz.py:257`.) |
| C11 `test_RED_A6_beneficio_rejeitado_limbo` | **vermelho → vermelho** | `limbo=True` chega à preparação que exige texto sem instante vivo; D1 falha antes do ato. (`chamados/tests/test_e4b_c11_orm.py:122`; `chamados/tests/test_e4b_c6_orm.py:82`.) |
| `test_RED_alimentacao_e_no_app_contam_a_tranca_uma_vez` | vermelho → vermelho previsto | Fixture já tem texto e instante; a asserção cobra contagem de tranca, sem cura identificada por D1/J1. (`chamados/tests/test_n1_perguntas_do_app.py:29`; `chamados/tests/test_n1_perguntas_do_app.py:153`; `chamados/tests/test_n1_perguntas_do_app.py:157`.) |
| C12 `test_RED_desligamento` | **vermelho → verde candidato** | D1 permite ao S128 carimbar o limbo; a segunda cascata pode satisfazer `admin_decidiu`. (`chamados/services/disputa_emissao.py:1987`; `chamados/services/disputa_emissao.py:2007`; `chamados/tests/test_e4b_c12_orm.py:171`; `chamados/tests/test_e4b_c12_orm.py:179`.) |
| C11 `test_RED_limbo_misto_fecha_mortas_orfa_e_entrega_motivo_proprio` | **vermelho → vermelho** | Método herdado de C6; D1 contradiz a própria precondição. (`chamados/tests/test_e4b_c11_orm.py:27`; `chamados/tests/test_e4b_c6_orm.py:82`.) |
| C6 `test_RED_limbo_misto_fecha_mortas_orfa_e_entrega_motivo_proprio` | **vermelho → vermelho** | Mesmo conflito; também permanece incompatível a expectativa posterior de conservar o limbo sem carimbo. (`chamados/tests/test_e4b_c6_orm.py:82`; `chamados/tests/test_e4b_c6_orm.py:116`.) |
| `test_RED_recusa_por_paridade_devolve_ao_admin_com_o_motivo` | vermelho → vermelho previsto | Par completo; DEVOLUÇÃO não remove a resposta. D1/J1 não mostram cura para as verificações da recusa. (`chamados/tests/test_o261_recusa_fala.py:67`; `chamados/tests/test_o261_recusa_fala.py:95`; `chamados/services/atos_pergunta.py:56`.) |
| `test_RED_segunda_tentativa_e_o_estado_de_prod` | vermelho → vermelho previsto | Fixture completa; nenhuma mudança identificada no estado/trilha exigidos pela segunda tentativa. (`chamados/tests/test_pvmuda_carimbo_em_voo.py:64`; `chamados/tests/test_pvmuda_carimbo_em_voo.py:113`; `chamados/tests/test_pvmuda_carimbo_em_voo.py:118`.) |
| `test_RED_validar_recusada_por_paridade_nao_fecha_a_disputa` | vermelho → vermelho previsto | Mesmo par completo; D1 não muda sua classificação. (`chamados/tests/test_pvmuda_carimbo_em_voo.py:64`; `chamados/tests/test_pvmuda_carimbo_em_voo.py:87`; `chamados/tests/test_pvmuda_carimbo_em_voo.py:93`.) |

A candidata C12 depende desta cadeia concreta: o limbo tem texto sem instante; a primeira cascata ainda encontra a proteção `instante OU texto`; D1 deixa o limbo morrer pelo C6 e o S128 chamar `carimbar(..., resposta='validar')`; essa porta preenche resposta/validação; na segunda cascata, a disputa pode fechar por desligamento. Isso explica a possível cura da asserção final, sem certificar o teste inteiro. (`chamados/tests/test_e4b_c12_orm.py:58`; `chamados/models.py:1954`; `colaboradores/services/desligamento.py:165`; `chamados/services/disputa_emissao.py:2007`; `chamados/models.py:3260`; `chamados/tests/test_e4b_c12_orm.py:179`.)

| Teste de ponto/core | c9 → D1+J1 | Motivo e asserção | [nao verificado]
|---|---|---|
| `test_calendario` | vermelho → ? | Cobra medição SQL; fixture já tem o par completo. (`core/tests/test_selo_performance.py:322`; `core/tests/test_selo_performance.py:344`; `core/tests/test_selo_performance.py:406`.) |
| `test_chamados_modal` | vermelho → ? | Mesma limitação: não há nova contagem nem diagnóstico da falha. (`core/tests/test_selo_performance.py:344`; `core/tests/test_selo_performance.py:380`.) |
| `test_pendencias_disputa` | vermelho → ? | Mesma limitação de medição. (`core/tests/test_selo_performance.py:344`; `core/tests/test_selo_performance.py:357`.) |
| `test_MORDE_o_mmd_commitado_bate_com_o_codigo` | vermelho → ? | Exige igualdade entre geração e artefato; o anexo não mostra a divergência. (`core/tests/test_selo_diagrama_do_codigo.py:46`.) |
| `test_MORDE_pendente_curado_sai_da_lista` | **vermelho → vermelho** | A impressão registrada não existe literalmente na chamada atual, que acrescentou `agora=agora`; D1/J1 não alteram isso. (`core/juizes.py:878`; `colaboradores/services/drawer.py:121`; `core/tests/test_contract_juiz_tela.py:38`.) |
| `test_MORDE_uma_aresta_a_mais_no_mmd_derruba_o_selo` | vermelho → ? | Sem traceback, não se sabe se falhou a detecção da adulteração ou a restauração. (`core/tests/test_selo_diagrama_do_codigo.py:93`; `core/tests/test_selo_diagrama_do_codigo.py:98`.) |
| `test_registrar_batida_com_1_furo_vivo_que_ela_fecha` | vermelho → ? | Cobra fechamento, callbacks e consultas; o cenário não insere pergunta explicitamente. (`ponto/tests/test_selo_chokepoint_escrita.py:416`; `ponto/tests/test_selo_chokepoint_escrita.py:437`; `ponto/tests/test_selo_chokepoint_escrita.py:289`; `ponto/tests/test_selo_chokepoint_escrita.py:293`.) |
| `test_registrar_batida_com_3_furos_vivos_que_ela_fecha` | vermelho → ? | Mesmo cenário com três chamados; sem cura semântica demonstrada. (`ponto/tests/test_selo_chokepoint_escrita.py:416`; `ponto/tests/test_selo_chokepoint_escrita.py:442`; `ponto/tests/test_selo_chokepoint_escrita.py:293`.) |
| `test_registrar_batida_com_3_furos_vivos_que_ficam` | vermelho → ? | Cobra permanência dos vivos e custo da escrita; sem medição contrafactual. (`ponto/tests/test_selo_chokepoint_escrita.py:456`; `ponto/tests/test_selo_chokepoint_escrita.py:477`; `ponto/tests/test_selo_chokepoint_escrita.py:293`.) |
| `test_ruff_zero` | vermelho → ? | Cobra Ruff na árvore inteira; não achei os diagnósticos no dado fornecido. (`core/tests/test_contract_esmeril.py:23`; `core/tests/test_contract_esmeril.py:24`.) |

Para os demais testes C6/C11/C12, **não identifiquei mudança semântica causada por D1/J1**: as preparações comuns não têm resposta, e C12 acrescenta limbo especificamente no desligamento. O estado c9 dos testes ausentes dos anexos e seu resultado posterior continuam **[nao verificado]**; ausência da lista de falhas não prova verde. (`chamados/tests/test_e4b_c6_orm.py:71`; `chamados/tests/test_e4b_c11_orm.py:99`; `chamados/tests/test_e4b_c12_orm.py:136`.)

A tabela completa esses testes individualmente. “?” nas duas posições indica estado não fornecido, sem alteração prevista: [nao verificado]

| Teste — C6 e também sua execução herdada em C11 | c9 → D1+J1 | Asserção pertinente | [nao verificado]
|---|---|---|
| `test_RED_exportada_mista_fecha_mortas_orfa_e_entrega_pauta` | ? → ? | Carimbos e pauta: `chamados/tests/test_e4b_c6_orm.py:118`; `chamados/tests/test_e4b_c6_orm.py:128`. |
| `test_RED_fio_tambem_entrega_pauta_no_fechamento` | ? → ? | Mesmo corpo: `chamados/tests/test_e4b_c6_orm.py:113`; `chamados/tests/test_e4b_c6_orm.py:128`. |
| `test_RED_passivo_ja_encerrado_nao_ganha_pauta` | ? → ? | `chamados/tests/test_e4b_c6_orm.py:174`. |
| `test_RED_c7_conversa_sem_competencia` | ? → ? | `chamados/tests/test_e4b_c6_orm.py:190`. |
| `test_RED_c7_trava_antes_exists_mesma_ancora` | ? → ? | `chamados/tests/test_e4b_c6_orm.py:216`. |
| `test_RED_c7_texto_cabe_com_limbo_longo` | ? → ? | Mede texto de pauta, sem classificar resposta: `chamados/tests/test_e4b_c6_orm.py:238`; `chamados/tests/test_e4b_c6_orm.py:241`. |
| `test_RED_c8_todas_ancoras_antes_de_escrever` | ? → ? | Encaminha ao selo SQL: `chamados/tests/test_e4b_c6_orm.py:246`; asserção: `chamados/tests/test_e4b_c6_orm.py:301`. |
| `test_RED_c9_contador_dp_antes_das_ancoras` | ? → ? | Encaminha ao selo SQL: `chamados/tests/test_e4b_c6_orm.py:249`; asserção: `chamados/tests/test_e4b_c6_orm.py:301`. |
| `test_RED_c9_lote_vazio_nao_toca_contador` | ? → ? | `chamados/tests/test_e4b_c6_orm.py:264`; `chamados/tests/test_e4b_c6_orm.py:265`. |
| `test_RED_c7_recusa_rollback_nao_entrega` | ? → ? | `chamados/tests/test_e4b_c6_orm.py:321`; `chamados/tests/test_e4b_c6_orm.py:322`. |

A execução duplicada em C11 decorre da herança da classe C6. (`chamados/tests/test_e4b_c11_orm.py:27`.)

| Demais testes próprios de C11 | c9 → D1+J1 | Asserção/caminho pertinente | [nao verificado]
|---|---|---|
| `test_RED_A1_excecao` | ? → ? | Usa cenário sem limbo e verificações comuns: `chamados/tests/test_e4b_c11_orm.py:104`; `chamados/tests/test_e4b_c11_orm.py:101`; `chamados/tests/test_e4b_c6_orm.py:128`. |
| `test_RED_A2_agenda` | ? → ? | `chamados/tests/test_e4b_c11_orm.py:107`; chamada da agenda: `chamados/tests/test_e4b_c11_orm.py:94`. |
| `test_RED_A3_escala` | ? → ? | `chamados/tests/test_e4b_c11_orm.py:110`; verificações comuns: `chamados/tests/test_e4b_c6_orm.py:128`. |
| `test_RED_A4_autorizar_geofence` | ? → ? | `chamados/tests/test_e4b_c11_orm.py:113`; registro: `chamados/tests/test_e4b_c11_orm.py:92`. |
| `test_RED_A5_recusar_geofence` | ? → ? | `chamados/tests/test_e4b_c11_orm.py:116`; retratação: `c

[truncado em 20000 de 26345 caracteres]

## 15/11 17:22 · sessao a0d55dc0e9f7 · - · - · origem=Ronald (tmux)

Conclusões para consolidar, todas só da cópia:

- **Pergunta 2 — PROCEDE, com colisão/omissão específica:** a guarda literal exige aprovação para o hash, mas CODEX item 5 admite achado descartado com motivo e anexo ao opus, e manda subir ao opus após mais de três voltas sem esperar. O B não declara como esses caminhos continuam atravessando a guarda. Evidência: `CLAUDE.md:375-379`. A exigência de suite inteira verde antes do opus continua: `CLAUDE.md:377-378`, `app/docs/PISTA.md:113-114`.
- **Sem colisão demonstrada** com motivo obrigatório, porta canônica, prioridade de instrumento ou pouso próprio: `CLAUDE.md:95-105`, `CLAUDE.md:290-295`, `CLAUDE.md:325-328`. O próprio registro B conserva redefinição, teto 2 livre, revisão e contrato antes da construção: `app/docs/BACKLOG.md:1954`.
- **Contrato não é autorização de chamada única para sempre:** contrato de cinco linhas e revisão prévia conferem; se a forma mudar, exige nova chamada. `CLAUDE.md:394`, `app/docs/PISTA.md:170-176`. O literal “UMA vez antes” precisa ser lido junto dessa condição.
- **Delta:** voltas do meio podem verificar só o delta (`CLAUDE.md:366-368`); pre-verificador recebe diff do hash da cópia (`CLAUDE.md:372-373`). Isso permite revisar delta, mas não define mecanismo que transfira prova do hash antigo ao novo [nao verificado].
- **rc 75:** substituição da leitura por sonnet/falha do Codex é prevista com motivo (`CLAUDE.md:358-362`, `app/docs/PISTA.md:91-96`). Essa regra não define certificado persistente de aprovação por hash que a guarda poderia ler [nao verificado]. Não confundir com rc 75 da trava: ali é “a vez não chegou” (`CLAUDE.md:103-105`).
- **A permanece independente:** está registrado vigente, enquanto B está só registrado para revisão antes de construir: `app/docs/RELATO.md:9208`, `app/docs/BACKLOG.md:1954`.
- **TRAVA-MENOS-FILA-COM-SELO não consta nominalmente em `app/docs/CORTES.md` nesta cópia** [nao verificado quanto ao literal original]. Seu conteúdo verificável está em `CLAUDE.md:101-105`, `app/docs/RELATO.md:9057-9063` e `app/docs/PROMPTS.md:2087`. O contrato original declara regua/pre-push fora dessa porta (`app/docs/RELATO.md:9059`) e separa histórico/ESTADO por cruzarem trava+gerador (`app/docs/RELATO.md:9063`, `app/docs/BACKLOG.md:2581`).
- **Pergunta 4 — NAO PROCEDE no escopo declarado:** instrumento com pouso próprio é permitido (`CLAUDE.md:325-328`); config do Code é proibida (`CLAUDE.md:350-352`), e exportada/escala/vínculo/atalho pedem `!` (`CLAUDE.md:265-268`). Nenhum desses atos é necessário pelo escopo B registrado (`app/docs/BACKLOG.md:1954`). Implementação futura respeitando o escopo: [nao verificado].
- Para precedência normativa, a consolidação 7b é mais nova e prevalece sobre blocos antigos de PISTA (`app/docs/PISTA.md:10-12`).

## 15/11 17:22 · sessao b835b1f65803 · - · - · origem=Ronald (tmux)

- **Premissa da guarda: PROCEDE com correção.** `_GUARDA_MOTIVO` existe exatamente em `bin/suite.sh:236`, mas essa linha só declara a regex; a decisão é `bin/suite.sh:255` e a recusa rc 2 é `bin/suite.sh:262`. A regex admite `pre-opus <OBRA>` e `pouso <OBRA>`; não consulta prova nem hash. (`bin/suite.sh:236`, `bin/suite.sh:255`)

- **`--dir` existe e escolhe a árvore: PROCEDE.** Parse em `bin/suite.sh:136`, atribuição em `bin/suite.sh:137`; descoberta da seleção efetiva em `${DIR:-$RAIZ}/app` em `bin/suite.sh:238`; montagem da cópia em `bin/suite.sh:284`–`296`. A árvore viva passada explicitamente como `--dir` é recusada pela proteção de limpeza; sem `--dir`, usa `$RAIZ/app`. (`bin/suite.sh:289`, `bin/suite.sh:290`, `bin/suite.sh:298`)

- **Selo existente que morde: PROCEDE para a guarda atual de motivo.** O selo executa a suite real numa árvore falsa e detecta se chegou à trava, recusando também custo anterior à recusa; exige rc 2 sem motivo e aceita `pre-opus FIO-E0-V4-E3`. (`bin/tests/test_suite_motivo.sh:3`, `bin/tests/test_suite_motivo.sh:37`, `bin/tests/test_suite_motivo.sh:41`, `bin/tests/test_suite_motivo.sh:46`, `bin/tests/test_suite_motivo.sh:61`) Há mutantes explícitos, inclusive `aceita_sem_motivo`, e o juiz precisa reprovar cada mutante. (`bin/tests/test_suite_motivo.sh:156`, `bin/tests/test_suite_motivo.sh:162`, `bin/tests/test_suite_motivo.sh:164`) **Não prova hoje aprovação prévia por hash**: o positivo atual passa sem fornecer tal prova. (`bin/tests/test_suite_motivo.sh:61`)

- **“Os três chamam por caminho próprio, fora da guarda”: NAO PROCEDE.** `volta_intermediaria.sh` chama a própria suite com `--dir` e `--volta`, atravessando a guarda; se o helper produzir seleção inteira, a guarda atual também a alcança. (`bin/volta_intermediaria.sh:32`, `bin/suite.sh:164`, `bin/suite.sh:255`) `regua.sh` e `pre-push.sh` usam diretamente `trava_teste.sh` e `manage.py test`, fora dessa guarda. (`bin/regua.sh:165`, `bin/regua.sh:168`, `bin/pre-push.sh:142`, `bin/pre-push.sh:186`)

- **Lacuna literal sem `--dir`: PROCEDE.** `--motivo "pre-opus <OBRA>"` sem `--dir` é uso documentado para árvore viva, e hoje usa `$RAIZ/app`; o item B citado só especifica prova para “hash de `--dir`”, deixando esse caminho sem condição explícita de prova. (`bin/suite.sh:23`, `bin/suite.sh:129`, `bin/suite.sh:298`; literal do anexo, sem arquivo:linha disponível [nao verificado]) Se a pergunta quer dizer literalmente `--motivo "pre-opus"` **sem nome de obra**, já recusa rc 2, independentemente de `--dir`. (`bin/suite.sh:236`, `bin/suite.sh:255`, `bin/tests/test_suite_motivo.sh:48`)

- **Momento da prova:** colocar a guarda junto à atual significa verificar antes de env-file, montagem e espera pela trava; a trava só é pedida posteriormente. (`bin/suite.sh:255`, `bin/suite.sh:276`, `bin/suite.sh:284`, `bin/suite.sh:317`) A árvore viva permanece um bind mount de `$RAIZ/app`; uma prova anterior à espera, sozinha, não comprova que esse conteúdo permaneceu igual até a execução. (`bin/suite.sh:298`, `bin/suite.sh:317`, `bin/suite.sh:322`; ausência de mutação durante a espera [nao verificado])

- **Escopo material do código atual:** a porta executa testes em container com settings CI; `REGUA_DB`, quando existente, é apenas repassado. (`bin/suite.sh:305`, `bin/suite.sh:306`, `bin/suite.sh:319`, `bin/suite.sh:324`, `bin/suite.sh:325`) Nenhuma implementação do item B foi fornecida para provar ausência futura de alterações em exportada, dinheiro, config do Code, trava ou pouso [nao verificado]. O literal do anexo proíbe essas três últimas mudanças, mas o cumprimento futuro não é provado pelo código atual [nao verificado].

Inspeção estática apenas; não executei o selo, pois ele cria, copia e remove arquivos temporários. (`bin/tests/test_suite_motivo.sh:10`, `bin/tests/test_suite_motivo.sh:11`, `bin/tests/test_suite_motivo.sh:15`, `bin/tests/test_suite_motivo.sh:16`)

## 15/11 17:18 · sessao d2bf99cfb25b · - · - · origem=Ronald (tmux)

Leitura concluída sem execução; não há evidência suficiente para certificar nenhuma cura dos vermelhos informados. Previsões abaixo são [nao verificado], exceto a permanência da falha estática demonstrada pelo código.

| Teste | Previsão D1+J1 | Referências | [nao verificado]
|---|---|---|
| BUG113, nas duas classes | Sem cura identificada | Asserções `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:163`, `:165`, `:169`, `:170`; herança `app/chamados/tests/test_e1_v8.py:118`. Escritor grava o par em `app/chamados/services/disputa_emissao.py:1481`; escape não deriva slot em `app/chamados/services/materializacao.py:359`. |
| Recusa por paridade | Sem cura identificada | Asserções `app/chamados/tests/test_o261_recusa_fala.py:95`; fixture com par completo `:67`. Devolução preserva resposta em `app/chamados/services/validacao.py:213`, `:230`; leitura preserva texto/instante em `app/chamados/services/atos_pergunta.py:56`. |
| Irmãs que gravaram | Sem cura identificada | `app/chamados/tests/test_o261_recusa_fala.py:116`, `:124`, `:126`, `:133`; mesma fixture com par completo `:67`. |
| Idempotência da recusada | Sem cura identificada | `app/chamados/tests/test_o261_recusa_fala.py:158`, `:159`, `:161`, `:162`; mesmo fluxo de devolução acima. |
| Carimbo em voo: validar recusada | Sem cura identificada | Fixture completa `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:64`; asserções `:87`, `:90`, `:92`, `:93`. |
| Carimbo em voo: segunda tentativa | Sem cura identificada | `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:105`, `:113`, `:114`, `:118`. |
| Carimbo em voo: determinismo | Sem cura identificada | `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:128`, `:130`. |
| Propriedades sob intercalação | Indeterminado | Asserções `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:276`, `:281`, `:289`; a regra de nova resposta escreve somente projeção em `:243`, após possível recusa `:237`. Assim, igualdade entre texto dos atos e projeção não está garantida em todas as trajetórias. |
| Endpoint filtra por q_vivas | **Permanece vermelho** | Teste exige exatamente um filtro de list comprehension em `app/chamados/tests/test_contract_pergunta_viva.py:78`; código possui filtro adicional em `app/chamados/services/pergunta_no_app.py:65`, além de `:67`. D1/J1 não removem esse filtro. |
| Recusa da porta nova | Sem cura identificada | Fixture completa `app/chamados/tests/test_fio_le_o_juiz.py:167`; materializador substituído por mock em `:251`; asserções `:257`, `:258`, `:259`, `:260`. |
| Alimentação e _no_app contam tranca | Sem cura identificada | Fixture completa `app/chamados/tests/test_n1_perguntas_do_app.py:29`; asserções `:153`, `:154`, `:156`, `:157`; contador mede `periodo_trancado` em `:65`. |
| R11, duas máquinas | Indeterminado | Asserções `app/chamados/tests/test_e3_portas_tarefa.py:634`, `:636`; fixture sem resposta em `app/chamados/tests/e3_v8_apoio.py:167`; corpos e mocks carregados em `:189`. Não achei traceback no dado fornecido; causa específica e estado posterior [nao verificado]. |

Nos fluxos determinísticos de paridade, D1 acrescentaria condição verdadeira ao par completo das fixtures; J1 recebe o mesmo texto conservado. A conservação nasce em `app/chamados/models.py:4123`; devolução altera validação, preservando resposta, em `app/chamados/services/atos_pergunta.py:56`. Isso não demonstra cura dos vermelhos.

Ressalva textual: a cópia testa **somente instante** em `app/chamados/services/validacao.py:418`, enquanto o C6 testa **somente texto** em `app/chamados/juizes.py:1572`. A afirmação sobre o predicado literal do main não pode ser comprovada por essas linhas da cópia [nao verificado].

O artefato `app/chamados/tests/e4c_f1v5_base_juizes.txt:1` identifica uma fotografia do BASE `97bbaa60`, não do main solicitado; portanto, não serve como substituto do diff contra `151b0666a` (`:2`, `:3`).

## 15/11 17:18 · sessao 66db28c118f0 · - · - · origem=Ronald (tmux)

**Embrulho e leitores**

- A única instanciação produtiva encontrada de `_PerguntaParaPlantio` é `_slot_esperado`, que passa `atos.resposta` à classe e entrega o objeto a `_ts_tipo_do_plantio` (`app/chamados/juizes.py:525`). A classe só substitui `resposta_colab`; todos os demais atributos são delegados à pergunta (`app/chamados/juizes.py:384-391`). Portanto, não expõe atributo público exclusivo exigido por outro leitor (`app/chamados/juizes.py:388-391`).
- Leitores indiretos desse objeto: `_ts_tipo_do_plantio` lê `chamado_destino`, `motivo`, `data_evento`, `pk` e `resposta_colab`; `_tipo_pelo_marco`, chamado pelo primeiro, lê `motivo`, `resposta_colab`, `data_evento` e `pk` (`app/chamados/services/materializacao.py:342-359`, `:390`, `:412-421`, `:275-289`, `:308`). Esses atributos existem na pergunta original; a única diferença é o valor de `resposta_colab` substituído pelo embrulho (`app/chamados/juizes.py:389-391`).
- Ocorrências adicionais encontradas na busca global são suporte/testes/documentos: carregamento no ambiente recortado (`app/chamados/tests/e3_c8b_apoio.py:128-129`); exigência da presença do símbolo (`app/chamados/tests/test_e4c_f1v2_harnesses.py:55-56`, `:105`); mutante que remove o símbolo (`app/chamados/tests/e4c_f1v2_mutantes.py:61-65`); teste que instancia a classe e exige texto substituído, pergunta original intacta e ausência do campo em `vars` do embrulho (`app/chamados/tests/test_e4c_f1v7_regressoes.py:55-58`); relatos históricos (`app/chamados/tests/e4c_f1v7_entrega.txt:9`, `app/chamados/tests/e4c_f1v7_red.txt:38`).

**J1: fonte realmente usada**

- Materializador de hora chama `_ts_tipo_do_plantio` com a pergunta original e lê `p.resposta_colab`; o slot passa o embrulho com texto dos atos (`app/chamados/services/materializacao.py:852`; `:359`, `:412`; `app/chamados/juizes.py:525`).
- Existe outra divergência: slot de `data_hora` lê `atos.resposta`, enquanto o materializador de `data_hora` lê `p.resposta_colab` (`app/chamados/juizes.py:512-519`; `app/chamados/services/materializacao.py:1001-1008`). Assim, **só retirar o embrulho na linha 525 não basta para cumprir a expressão “mesma fonte que o materializador” para todos os tipos** (`app/chamados/juizes.py:513`; `app/chamados/services/materializacao.py:1001`).
- Texto sem instante pode continuar alimentando cálculo de slot sem ser classificado como “respondida”; esse caso está explicitamente testado sem fabricar instante (`app/chamados/tests/test_e4c_f1v6_matriz.py:143-146`). Portanto, esse leitor não demonstra contradição lógica com D1 (`app/chamados/tests/test_e4c_f1v6_matriz.py:144-146`). Há, entretanto, testes adicionais da obra que exigem texto sem instante em filas chamadas “respondidas a validar”, potencialmente incompatíveis com D1 aplicado a essas filas (`app/chamados/tests/test_e4c_f1v6_orm.py:54-58`).

**Ponto/core: tabela para consolidação**

“Vermelho” vem do anexo fornecido; onde não há transição demonstrável, mantenho resultado contrafactual `[nao verificado]`.

| Teste | c9 → D1+J1 | Fundamentação | [nao verificado]
|---|---|---|
| `test_calendario` | vermelho → `[nao verificado]`; sem cura demonstrada | Mede contagem SQL da rota e compara teto; as perguntas da fixture já têm texto e instante (`app/core/tests/test_selo_performance.py:402-406`, `:333-350`, `:317-322`). |
| `test_chamados_modal` | vermelho → `[nao verificado]`; sem cura demonstrada | Mesma medição; fixture já satisfaz D1 (`app/core/tests/test_selo_performance.py:377-380`, `:333-350`, `:317-322`). |
| `test_pendencias_disputa` | vermelho → `[nao verificado]`; sem cura demonstrada | Mesma medição; fixture já satisfaz D1 (`app/core/tests/test_selo_performance.py:354-357`, `:333-350`, `:317-322`). |
| `test_MORDE_pendente_curado_sai_da_lista` | vermelho → vermelho, se somente D1/J1 | Causa estática concreta encontrada: registro exige chamada completa numa linha; código agora acrescenta `agora=agora` e fecha chamada na linha seguinte, faltando a impressão literal (`app/core/juizes.py:878`; `app/colaboradores/services/drawer.py:121-122`; asserção `app/core/tests/test_contract_juiz_tela.py:35-38`). |
| `test_MORDE_o_mmd_commitado_bate_com_o_codigo` | vermelho → `[nao verificado]` | Exige igualdade entre artefatos e geração; D1/J1 não especificam atualização desses artefatos nem foi fornecida a divergência (`app/core/tests/test_selo_diagrama_do_codigo.py:44-47`; `app/core/management/commands/gerar_diagrama.py:921-930`). |
| `test_MORDE_uma_aresta_a_mais_no_mmd_derruba_o_selo` | vermelho → `[nao verificado]` | Duas asserções: arquivo adulterado retorna 1 e arquivo restaurado retorna 0. Sem traceback não se sabe qual falhou (`app/core/tests/test_selo_diagrama_do_codigo.py:90-98`). O próprio teste escreve arquivo, portanto não deve ser executado nesta tarefa (`app/core/tests/test_selo_diagrama_do_codigo.py:90-96`). |
| `test_registrar_batida_com_1_furo_vivo_que_ela_fecha` | vermelho → `[nao verificado]`; sem cura semântica demonstrada | Fixture cria célula/chamados de furo; verifica fechamento, callbacks e consultas, sem inserir PerguntaDisputa nesse cenário (`app/ponto/tests/test_selo_chokepoint_escrita.py:413-433`, `:435-438`, `:287-298`). |
| `test_registrar_batida_com_3_furos_vivos_que_ela_fecha` | vermelho → `[nao verificado]`; sem cura semântica demonstrada | Mesmo cenário com três chamados; verifica três fechamentos e custos (`app/ponto/tests/test_selo_chokepoint_escrita.py:416-433`, `:440-443`, `:287-298`). |
| `test_registrar_batida_com_3_furos_vivos_que_ficam` | vermelho → `[nao verificado]`; sem cura semântica demonstrada | Fixture cria célula/chamados cobrando saída e registra entrada; verifica vivos e custos (`app/ponto/tests/test_selo_chokepoint_escrita.py:453-468`, `:475-478`, `:287-298`). |
| `test_ruff_zero` | vermelho → `[nao verificado]` | Asserta código de saída do Ruff sobre a árvore inteira; anexo não fornece diagnósticos que permitam atribuir a falha a D1/J1 (`app/core/tests/test_contract_esmeril.py:22-24`). |

As medições de consultas ou callbacks não podem ser substituídas pela constatação de que o predicado mudou: os testes cobram valores medidos e o anexo só fornece nomes das falhas (`app/core/tests/test_selo_performance.py:338-350`; `app/ponto/tests/test_selo_chokepoint_escrita.py:287-298`; diagnóstico concreto das falhas: `[nao verificado]`).

## 15/11 17:17 · sessao 86555b64bf04 · - · - · origem=Ronald (tmux)

Resultados por inspeção; execução com D1+J1: [nao verificado].

| Teste | Estado informado → previsão D1+J1 | Fundamentação | [nao verificado]
|---|---|---|
| C6 `test_RED_limbo_misto_fecha_mortas_orfa_e_entrega_motivo_proprio` | vermelho → vermelho | A própria preparação exige texto **sem instante** vivo em `app/chamados/tests/test_e4b_c6_orm.py:82`; D1 torna essa pergunta morta pela célula `ok` (`app/chamados/juizes.py:1618`). Falharia antes do ato. |
| C11 mesmo teste herdado | vermelho → vermelho | Herança em `app/chamados/tests/test_e4b_c11_orm.py:27`; mesmo assert C6 `:82`. |
| C11 `test_RED_A6_beneficio_rejeitado_limbo` | vermelho → vermelho | Aciona `limbo=True` (`app/chamados/tests/test_e4b_c11_orm.py:122`), chama `_provar` (`:101`), chega ao mesmo assert C6 `:82`. |
| C12 `test_RED_desligamento` | vermelho → verde previsto [nao verificado] | Limbo sem instante em `app/chamados/tests/test_e4b_c12_orm.py:58`; D1 permite que S128 o carimbe; segunda cascata consegue carimbar a muda `admin_decidiu`, satisfazendo `:179`. |
| Demais testes C6/C11/C12 | sem mudança lógica identificada; estados de execução [nao verificado] | Preparação padrão sem texto (`app/chamados/tests/test_e4b_c6_orm.py:71`); somente desligamento acrescenta limbo em C12 (`app/chamados/tests/test_e4b_c12_orm.py:136`). Não criam divergência texto do ato/projeção para J1. |

Cadeia C12: a primeira tentativa continua retida pelo juiz próprio `respostas_sem_veredito`, cujo predicado é instante **OU** texto (`app/chamados/models.py:1952`), consultado em `app/colaboradores/services/desligamento.py:165`. Entretanto, no fechamento do chamado, D1 torna o limbo morto; S128 deixa de pulá-lo (`app/chamados/services/disputa_emissao.py:1987`), chama `carimbar(... resposta='validar')` (`:2007`), que preenche resposta/validação (`app/chamados/models.py:3260`). Na segunda execução (`app/chamados/tests/test_e4b_c12_orm.py:171`), a disputa deixa de ter a pendência e a porta de desligamento carimba a muda (`app/chamados/models.py:2029`).

No subteste desligamento **não exportado**, nenhuma mudança identificada: a pergunta viva não exportada continua impedindo o fechamento e a varredura do limbo; a expectativa é chamado aberto (`app/chamados/tests/test_e4b_c12_orm.py:166`).

Contradição explícita dos **testes** com D1: assert C6 `:82`, expectativas de conservar o limbo sem carimbo em `:116`, e os três testes que os utilizam. Isso não demonstra necessidade técnica de texto sem instante no plantio. O predicado distinto de proteção `respostas_sem_veredito` permanece OR na cópia (`app/chamados/models.py:1954`).

Cobertura: C6 tem 11 métodos de teste; C11 acrescenta 13 e herda os 11 C6 (`app/chamados/tests/test_e4b_c11_orm.py:27`); C12 tem 10, incluindo sete subcenários em `test_nao_exportada_zero_pauta_por_gatilho` (`app/chamados/tests/test_e4b_c12_orm.py:209`). Nenhum verde→vermelho adicional foi identificado [nao verificado].

## 15/11 17:17 · sessao f0c6835096f7 · E4C-F1V3 · sonda · origem=Code

Não executei o envelope: ele cria/reescreve arquivos e acrescenta uma trilha, contrariando a regra de somente leitura (bin/sonda_leitura.sh:132, bin/sonda_leitura.sh:134, bin/sonda_leitura.sh:216, bin/sonda_leitura.sh:250).

O universo fornecido não é vazio: **898 perguntas com ato RESPOSTA; 50.283 perguntas no total**. [nao verificado]

| Classe | ABE | EXP |
|---|---:|---:|
| TEXTO_DIFERE | 24 [nao verificado] | Não consta [nao verificado] |
| RESPONDIDA_SEM_ATO | 4695 [nao verificado] | 12248 [nao verificado] |
| ATO_SEM_RESPONDIDA_EM | 24 [nao verificado] | Não consta [nao verificado] |

Transcrição integral da saída fornecida; todo o bloco é **[nao verificado]**, sem nova execução:

```text
sonda_leitura: sonda_ato_x_projecao.py [2f7516615f14db9b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO perguntas_com_ato_RESPOSTA 898 perguntas_total 50283
CONT ('ATO_SEM_RESPONDIDA_EM', 'ABE') 24
CONT ('ATO_SEM_RESPONDIDA_EM', 'ABE', 'emp2 06/2026') 1
CONT ('ATO_SEM_RESPONDIDA_EM', 'ABE', 'emp2 10/2026') 2
CONT ('ATO_SEM_RESPONDIDA_EM', 'ABE', 'emp2 11/2026') 13
CONT ('ATO_SEM_RESPONDIDA_EM', 'ABE', 'emp3 10/2026') 2
CONT ('ATO_SEM_RESPONDIDA_EM', 'ABE', 'emp3 11/2026') 6
CONT ('RESPONDIDA_SEM_ATO', 'ABE') 4695
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp1 08/2026') 2
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp1 10/2026') 38
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp1 11/2026') 4
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp2 05/2026') 6
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp2 06/2026') 1
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp2 07/2026') 139
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp2 10/2026') 1889
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp2 11/2026') 468
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp29 07/2026') 1
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp3 05/2026') 2
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp3 06/2026') 21
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp3 08/2026') 1252
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp3 10/2026') 290
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp3 11/2026') 156
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp4 05/2026') 7
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp4 08/2026') 270
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp4 10/2026') 96
CONT ('RESPONDIDA_SEM_ATO', 'ABE', 'emp4 11/2026') 53
CONT ('RESPONDIDA_SEM_ATO', 'EXP') 12248
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp2 08/2026') 7909
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp2 09/2026') 2334
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp3 07/2026') 907
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp3 09/2026') 473
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp4 06/2026') 235
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp4 07/2026') 327
CONT ('RESPONDIDA_SEM_ATO', 'EXP', 'emp4 09/2026') 63
CONT ('TEXTO_DIFERE', 'ABE') 24
CONT ('TEXTO_DIFERE', 'ABE', 'emp2 06/2026') 1
CONT ('TEXTO_DIFERE', 'ABE', 'emp2 10/2026') 2
CONT ('TEXTO_DIFERE', 'ABE', 'emp2 11/2026') 13
CONT ('TEXTO_DIFERE', 'ABE', 'emp3 10/2026') 2
CONT ('TEXTO_DIFERE', 'ABE', 'emp3 11/2026') 6
PKS TEXTO_DIFERE 24 38887 39180 42244 46751 48773 51830 52502 52698 53491 53644 53782 53900 54260 54385 55295 55313 55641 56287 56608 56718 56883 56923 57211 57214
PKS RESPONDIDA_SEM_ATO 16943 6 7 8 118 119 120 158 174 175 176 180 181 183 184 185 186 189 190 193 194 200 202 203 207 208 263 264 265 266 267 268 272 287 289 292 293 294 295 301 302 305 307 308 309 310 311 313 320 321 322 323 324 325 326 327 328 329 330 331 334 335 337 345 348 349 350 351 354 355 357 358 359 363 364 365 366 369 370 373 374 375 381 390 398 400 401 405 406 407 412 415 418 419 420 424 427 430 435 439 445 448 454 455 456 457 458 459 461 463 466 470 473 476 482 483 484 488 490 495 496 499 504 505 510 511 513 514 516 518 519 521 524 525 526 528 531 534 540 545 547 550 551 553 557 560 562 563 564 565 566 567 569 570 576 577 578 612 613 614 615 616 626 627 671 672 673 674 675 676 677 678 690 691 692 700 701 703 704 705 706 707 709 710 711 712 713 714 715 716 717 719 720 721 723 725 726 727 728 729 730
PKS ATO_SEM_RESPONDIDA_EM 24 38887 39180 42244 46751 48773 51830 52502 52698 53491 53644 53782 53900 54260 54385 55295 55313 55641 56287 56608 56718 56883 56923 57211 57214
```

_5712 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
