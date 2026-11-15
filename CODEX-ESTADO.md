# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 20:01** · publicado em **15/11/2026 20:02** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 20:01 · sessao 6a0cf11c44b9 · - · - · origem=Ronald (tmux)

Auditoria estática concluída; nenhum teste foi executado e não houve acesso a banco/log. Evidência de execução e medidas novas de produção: **[nao verificado]**. [nao verificado]

**Escritores e guardas do par**

| Sítio atual | O que escreve/lê e relação com o corte |
|---|---|
| `app/chamados/models.py:2525`, `:2526`, `:3097`, `:3100` | Nascimento: defaults `('', None)` ou campos recebidos; salva inserção. Guarda anterior em `:2922`–`:2923` só consulta recusa de instante sem texto; trilha R/V no mesmo atomic de `:2924`, registro `:3102`. |
| `app/chamados/services/disputa_emissao.py:1446`, `:1476`, `:1481`, `:1482`, `:1522` | Resposta canônica: normaliza texto, exige não vazio, grava texto + instante. Atomic `:1517`; ato `RESPOSTA` `:1525`. Já produz par completo. |
| `app/chamados/services/disputa_emissao.py:1663`, `:1671`, `:1673`, `:1674`, `:1676` | Automática: no-op quando já existe texto; guarda atual só recusa `I∧¬T`; grava os dois campos e `RESPOSTA` (`:1675`, `:1677`). Entrada com texto e instante `None` ainda precisa da recusa simétrica definida no corte. |
| `app/api/foto_recebida.py:25`, `:42`, `:48` | Delegado automático: texto constante não vazio e instante `agora`; validação delegada em `:51`. |
| `app/chamados/services/disputa_emissao.py:1691`, `:1695`, `:1697` | Cura de limbo: lê `¬I∧T`, escreve somente instante fornecido; ato `RESPOSTA` no mesmo atomic (`:1696`, `:1698`). Não confundir com a nova recusa de **criar** limbo: pode continuar reparando legado, conforme finalidade já existente. |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`, `:69` | Dois delegados: carimba pelo instante da nota ou zera pela reabertura. |
| `app/chamados/models.py:3213`, `:3214`, `:3215`, `:3235` | `carimbar(resposta=...)`: só completa instante se texto não vazio e instante ausente; também pode completar validação. Guarda foto anterior em `:3209`–`:3210`; desfaz memória em `:3244`–`:3248`. |
| `app/chamados/services/materializacao.py:563`, `:567`, `:569` | Absorção delega `carimbar(resposta='validar')`; pode completar instante. Atomic inclui ato `ABSORCAO`. |
| `app/chamados/services/materializacao.py:1226`, `:1283`, `:1284` | Materialização granular delega `carimbar(resposta='validar')`; pode completar instante de texto legado. Esse corpo não registra ato próprio de resposta/validação. Garantia geral de decisão para todo chamador: **[nao verificado]**. |
| `app/chamados/services/disputa_emissao.py:2000`, `:2007`, `:2008` | Resolver por chamado: protege par completo sem materialização de fato; restante delega carimbo com `resposta='validar'`. |
| `app/chamados/services/disputa_emissao.py:2078`, `:2081`, `:2084` | Ramo sem credor: lê existência de texto, delega carimbo com `resposta='responder'`; auditoria em `:2084`, sem `DecisaoPergunta` próprio nesse trecho. |
| `app/chamados/models.py:3343`, `:3348`, `:3351`, `:3354`, `:3363`, `:3365` | Executor de lote: aceita par completo ou metade e consulta outra coluna. **Bypass atual:** se só recebe `respondida_em=None`, não lê texto nem valida resultado (`:3349`–`:3352`); permite produzir `¬I∧T`. |
| `app/chamados/services/disputa_emissao.py:1609`, `:1632`, `:1635`, `:1645`, `:1646`, `:1648` | Reabertura comum: já zera instante e texto; guarda texto, instante e validação anteriores no ato `reabertura`/`recusa`, dentro do atomic `:1621`; exclui fora do fato. |
| `app/chamados/models.py:1610`, `:1611`, `:1613` | Ressuscitação atual: preserva projeções R/V sem linha, **zera somente instante**, texto permanece. Não há `REABERTURA` obrigatória nesse bloco; `linhas_da_projecao_sem_linha` só gera atos condicionais em `:4074`, `:4078`. |
| `app/chamados/models.py:1531`, `:1533`, `:1553`, `:1554`, `:1555` | Universo atual da ressuscitação só recebe materializadas com texto SQL `NULL` ou `''`; simulação também só limpa instante. Ambos precisam representar a mesma zeragem integral quando a pergunta for efetivamente ressuscitada. Alterar seletividade do universo além disso não decorre automaticamente do corte. |
| `app/chamados/services/validacao.py:623`, `:628`, `:629` | Recusa delega zeragem integral à reabertura com ato `recusa`. |
| `app/chamados/services/materializacao.py:681`, `:688` | Devolução de absorvidas delega zeragem integral; fornece autor e motivo. |
| `app/chamados/models.py:2299`, `:2301` | Reabertura pelo colaborador delega zeragem integral com autor/motivo. |
| `app/chamados/services/acoes_disputa.py:103`; `app/chamados/views.py:1436`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69` | Outros delegados de reabertura; par e ato ficam na transação da porta `disputa_emissao.py:1621`, `:1632`, `:1648`. |
| `app/chamados/models.py:3529`, `:3534`, `:3559`, `:3560`, `:3563`, `:3566` | `save` valida transição do par e memoriza foto. Hoje permite par ilegal preexistente inalterado e escrita só em outros campos; não recusa `¬I∧T`, pois porta final só consulta instante sem texto (`app/chamados/services/fio.py:1595`, `:1602`). |
| `bin/sombra_regras_pessoais.py:85`; `bin/sombra_mascara_pessoal.py:10`, `:55`, `:57`, `:62`, `:64` | Escritor operacional de texto via tabela e `bulk_update`, restrito ao banco `sombra`; não é escritor de produção. |
| `app/chamados/services/validacao.py:224`, `:225`, `:227`, `:230` | Devolver validação conserva o par; só escreve validação e registra `DEVOLUCAO`. Não é zeragem de resposta. |

Portas adicionais de alcance que **hoje não escrevem o par**:

- `PerguntaDisputa.reabrir_por_competencia` promete voltar “com a resposta que tinha” e só descarimba (`app/chamados/models.py:3416`, `:3418`, `:3421`); merece constar da fronteira do item (2). `descarimbar` salva apenas materialização/via (`app/chamados/models.py:3286`, `:3293`, `:3295`).
- `reabrir_fantasma` também só descarimba (`app/chamados/models.py:3388`, `:3395`, `:3397`); seu chamador escolhe a pergunta, conforme docstring em `:3392`–`:3394`.
- `reabrir_por_resposta` tira carimbo por chegada de **resposta nova**, dentro da resposta canônica; zerar o par nesse ponto apagaria a resposta recém-recebida (`app/chamados/models.py:3400`, `:3412`, `:3414`; `app/chamados/services/disputa_emissao.py:1481`, `:1499`).
- `relavra_via.desfazer_carimbo_sem_lastro` só descarimba e pode reabrir o continente disputa; não escreve par (`app/chamados/services/relavra_via.py:246`, `:259`, `:261`).

**Testes: asserções que de fato mudam** [nao verificado]

| Causa | Asserção atual e mudança derivada |
|---|---|
| Juiz único `I∧T` | `test_e3_10_cura_no_escritor.py:132`, `:133`: `assertIn(vazia/espacos, lista)` passam a exclusão para legado `I∧¬T`; `:134` mantém inclusão do par completo. |
| Juiz único `I∧T` | `test_e3_10_cura_no_escritor.py:140`, `:147`: listas iguais a `[vazia.pk]` passam a `[]`. `:141` (`assertFalse(d.pode_fechar(...))`) **não deve ser invertida automaticamente**: `pode_fechar` também considera fio vivo; nova resposta completa do método requer examinar o restante da fixture e do juiz. |
| Juiz único `I∧T` | `test_o168_pai_com_conversa_no_gate.py:283`, `:285`: instante sem texto atualmente retém `[p.pk]`; passa a `[]`. `:287`: texto sem instante também atualmente retém `[p.pk]`; passa a `[]`. `:289` já exige `[]` após validação. |
| Partição do app/estoque | `test_o261e_f5_alcance.py:194`: espera pendentes `[]` para instante sem texto; pergunta viva em disputa aberta passa a pendente com o novo predicado. Consequências atuais `:196` `(fio_morto=1, pergunta=0, app=0)` e `:197` causa `b` precisam ser reavaliadas com a porta alcançável. `:199` razão `respondida_sem_texto` só desaparece pela **cura do estoque**; trocar leitor sozinho não limpa o dado. |
| Recusa simétrica | `test_contract_par_resposta.py:113` afirma que o detector não acusa trecho só instante, com mensagem “carimbo-sem-texto, que e legitimo”. Esse contrato unidirecional (`:81`) precisa cobrir também instante sem texto; a asserção atual não prova legalidade de valor algum em runtime. |

**Testes que precisam mudar preparação/cobertura, sem inverter automaticamente a asserção** [nao verificado]

- `test_pergunta_estado.py:57`–`:60`: comentário e fixture dizem reabertura mantém texto sem instante; o novo corte exige fixture `('', None)`, porém `estado == 'pendente'` e viva continuam expectativas compatíveis. Não há teste do escritor nessa classe (`:19`–`:20` só instancia o modelo). [nao verificado]
- `test_materializar_carimba_respondida.py:62`–`:63`: hoje persiste texto sem instante; esse `save` passa a ser recusado. Asserções de cura de legado `:76`–`:79` podem continuar se a fixture representar legado por escrita crua controlada; não é autorização para continuar criando meio-estado pela porta.
- `test_porta_resposta_pergunta.py:244`–`:245`: mesmo problema de fixture persistida só texto. Asserções `:247`, `:249`, `:251`, `:254`, `:255` conferem cura histórica/idempotência e não se tornam falsas só pela nova recusa.
- `test_e4b_c6_orm.py:72`–`:77`: fixture `limbo=True` usa `objects.create` com texto sem instante; deve passar a provar recusa da porta ou representar explicitamente legado. Asserções de diagnóstico de limbo `:130`, `:132`, `:142` permanecem possíveis para estoque legado; não há dado fornecido que prove estoque `¬I∧T` zerado pelo item (3). `test_e4b_c11_orm.py:121`–`:122` herda esse caso por `:99`–`:101`.
- `test_e3_10_cura_no_escritor.py:61`–`:69`, `:104`–`:107`, `:166`–`:170`, `:176`–`:184`, `:205`–`:212` já recusam instante sem texto; precisam de contrapartes texto sem instante e lote que só zera instante. Asserções atuais permanecem pertinentes.
- `test_e3_10_cura_no_escritor.py:192`–`:197` permite legado ilegal inalterado em outros campos e sua saída para pendente. Se o corte pretende rejeitar também `save()` do **mesmo** legado inalterado, essa asserção mudaria; essa extensão além de impedir gravar pares novos é **[nao verificado]**.
- `test_o261e_f2_porta_carimbo.py:720` usa `update` para montar legado texto sem instante; asserções `:725`–`:731` provam recusa/rollback do carimbo e podem continuar pertinentes.
- `test_o261_resposta_vazia.py:64` monta meio-estado legado por `update`; `:85`, `:95` já proíbem fabricar instante sem texto; `:109`, `:113` conferem recusa do carimbo pelo C6 e não são automaticamente invertidas pelo novo juiz.
- `test_o261e_f2_ramo_e_i20.py:265` exige preservação do legado `('07:55', None, None)`: o item (3) nomeia `I∧¬T`, portanto não implica limpar esse outro estoque.
- `test_reabrir_ressuscita.py:44`–`:49` exige que **pergunta não selecionada para ressuscitação** continue com resposta e materialização. Isso testa o universo atual SQL só texto vazio (`models.py:1533`), não uma ressuscitação que mantém texto. A seleção não deve ser ampliada sem distinguir essa questão.
- `test_tranca_de_verdade.py:61` e `test_competencia_trancada.py:155` preservam texto ao **trancar/encerrar**, não ao reabrir; não trocam de asserção pelo item (2). `test_tranca_de_verdade.py:74`–`:89` cobre reabrir competência, mas não confere zeragem do par nem foto anterior.

**Testes já conformes que devem permanecer como provas** [nao verificado]

- Reabrir zera par e guarda anterior: `test_reabrir_nao_destroi.py:49`, `:50`, `:52`, `:58`; pendente não fabrica ato em `:64`; repetição não duplica em `:72`.
- Porta única de reabertura e zeragem conjunta: `test_e3_porta_A_reabrir.py:38`, `:41`, `:42`; ato de carimbo sozinho `:51`; recusa comum zera par e registra `recusa` `:82`–`:83`.
- Contrato textual de zeragem já exige ambos: `test_reabertura_fantasma.py:63`–`:65`.
- `test_o261_ressuscita_respeita_o_marco.py:118`–`:121` já exige instante/validação/materialização limpos nos alvos; `:126`–`:128` preserva alvos excluídos pelo marco aceso. Falta nessa prova conferir texto e `REABERTURA` com foto anterior.
- Contratos estáticos texto+instante em `test_contract_texto_com_carimbo.py:119`, `:124`, `:136`, `:138`, `:166`–`:172` continuam úteis, porém não provam recusa runtime, atomicidade nem igualdade entre leitores.

Não localizei teste dedicado que aplique exatamente a cura das **143 vivas**, preserve todas validadas/materializadas e confira separação aberta/entregue; existência em arquivo não aberto: **[nao verificado]**. [nao verificado]

## 15/11 20:01 · sessao 130dfb79b02a · - · - · origem=Ronald (tmux)

Todos os sítios abaixo foram abertos na cópia; nenhuma execução de Django, banco ou log. Correspondência Git/HEAD: [nao verificado].

Use `J=I∧T`, com `T=bool((texto or '').strip())`, sem substituir vitalidade, validação, materialização, tranca, janela ou identidade; o código atual já possui esse significado na face SQL `q_respondida_com_texto`, mas ainda não a função `pergunta_respondida`. (`app/chamados/juizes.py:1523`; `app/chamados/juizes.py:1554`; `app/chamados/models.py:3436`.)

A tabela é por R do censo, com finalidade e destino. “J SQL” significa face SQL do juiz; “J” significa Python/property/contexto do template.

| Classe/R | Uso atual e finalidade | Destino e estado divergente |
|---|---|---|
| A/R13 | C6 usa `I∧T`, depois decide concordância/admin/vias/lâmpada. (`app/chamados/juizes.py:1403`; `app/chamados/juizes.py:1420`.) | J sobre projeção temporal, sem alteração por par. |
| A/R16 | Face SQL `I∧T`, com regex de todos os brancos Python. (`app/chamados/juizes.py:1539`; `app/chamados/juizes.py:1559`.) | Aproveitável como J SQL; sem alteração por par. |
| A/R19 | Fila global exige `q_vivas`, `q_respondida_com_texto`, C6 Python. (`app/chamados/juizes.py:1725`; `app/chamados/juizes.py:1728`.) | J SQL; sem alteração por par. |
| A/R29 | Auditor I21 usa I + `.exclude(resposta_colab='')`, sem trim, e exclui validada/materializada. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1572`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1575`.) | J SQL; diverge `I∧brancos`, classificados hoje como texto. |
| A/R52 | Degrau resposta de `q_vivas` delega à face E. (`app/chamados/models.py:3453`; `app/chamados/models.py:3454`; `app/chamados/models.py:3468`.) | J SQL; sem alteração por par. `q_vivas` segue continente do C6, não igualdade. (`app/chamados/models.py:3436`; `app/chamados/models.py:3439`.) |
| A/R79 | Resolvedor preserva pergunta com `I∧T` e sem materialização de fato. (`app/chamados/services/disputa_emissao.py:2000`; `app/chamados/services/disputa_emissao.py:2001`.) | J + juiz de materialização; sem alteração por par. |
| A/R96 | Janela visível herda degrau resposta do `q_vivas`. (`app/chamados/services/janela_colab.py:63`.) | Herda J SQL; sem alteração por par. |
| A/R110 | App pré-filtra `q_vivas`, confirma C6. (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/services/pergunta_no_app.py:61`.) | Herda J SQL/J no C6; sem alteração por par. |
| A/R112 | Crédito da mesa: tuplas com I e `not texto_vazio`, complemento explícito. (`app/chamados/services/raiox.py:138`; `app/chamados/services/raiox.py:143`; `app/chamados/services/raiox.py:144`.) | J/adaptador de tupla, sem reconstituir predicado; sem alteração por par. |
| A/R125 | Vigia viva em disputa fechada herda `q_vivas`. (`app/chamados/services/vigia_fio.py:769`.) | Herda J SQL; sem alteração por par. |
| A/R141 | Fecho do fio herda `q_vivas`. (`app/ponto/services/veredito_celula.py:181`.) | Herda J SQL; sem alteração por par. |

O corpo B é `sem_veredito_entre`: `¬validada ∧ ¬materializada ∧ (I∨T) ∧ (ignorar_tranca∨¬trancada)`. Trocar somente a cláusula `I∨T` por J muda ambas as metades (`I∧¬T`, `¬I∧T`), dentro dos demais filtros; os delegados abaixo herdam esse delta. (`app/chamados/models.py:1905`; `app/chamados/models.py:1909`.)

| R/B | Sítio e finalidade |
|---|---|
| 24 | Irmãs sem consumo; retenção/crédito. (`app/chamados/juizes.py:2079`.) |
| 27 | Auditor I12; retenção de irmãs. (`app/chamados/management/commands/auditar_invariantes_chamados.py:984`.) |
| 38 | Lote validar classe A; fila. (`app/chamados/management/commands/validar_classe_a.py:54`.) |
| 42 | Corpo do juiz sem veredito; retenção. (`app/chamados/models.py:1907`; `app/chamados/models.py:1908`.) |
| 43 | Lista da disputa; retenção. (`app/chamados/models.py:1889`.) |
| 44 | Pode fechar; ausência da lista também combinada com ausência de fio vivo. (`app/chamados/models.py:1917`.) |
| 45 | Fechar; lista não vazia recusa fechamento. (`app/chamados/models.py:1950`; `app/chamados/models.py:1952`.) |
| 60 | Retenção da retratação. (`app/chamados/reconciliador.py:448`.) |
| 62 | Encerrar órfãs pela lâmpada. (`app/chamados/reconciliador.py:1164`.) |
| 69 | Medir competência trancada; ignora tranca. (`app/chamados/services/competencia_trancada.py:126`.) |
| 80 | Resolvedor; irmãs protegem pai. (`app/chamados/services/disputa_emissao.py:2030`.) |
| 85 | Gaveta da fila por causa. (`app/chamados/services/fila_por_causa.py:226`.) |
| 104 | Materializador; irmãs impedem fecho. (`app/chamados/services/materializacao.py:768`.) |
| 133 | Desligamento; retenção. (`app/colaboradores/services/desligamento.py:165`.) |
| 134 | Ficha; interseção da lista sem veredito com vivas. (`app/colaboradores/services/ficha.py:168`; `app/colaboradores/services/ficha.py:169`.) |
| 142 | CSF pelos universos pai/destino, ignorando tranca. (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574`.) |

Todos os C abaixo trocam I por J — J SQL quando em `.filter`/contagem, J nos demais — e mudam `I∧¬T`; testes adicionais de domínio continuam podendo impedir diferença no resultado externo. (`app/chamados/models.py:3641`; `app/chamados/juizes.py:1559`.)

| R/C | Predicado e finalidade |
|---|---|
| 4 | I seleciona `respondidas` no histórico app. (`app/api/views.py:2577`.) |
| 5 | I informa `ja_respondida` à resposta/correção. (`app/api/views.py:2620`; `app/api/views.py:2627`.) |
| 18 | I + lastro/vitalidade para corrigíveis. (`app/chamados/juizes.py:1718`; `app/chamados/juizes.py:1719`.) |
| 25 | Falta de I integra contraprova I21a. (`app/chamados/management/commands/auditar_invariantes_chamados.py:177`.) |
| 26 | I em disputa aberta no auditor I2. (`app/chamados/management/commands/auditar_invariantes_chamados.py:592`.) |
| 30 | ¬I no auditor C2. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1603`.) |
| 49 | I distingue pendente/respondida após precedências de fase/C6. (`app/chamados/models.py:3124`; `app/chamados/models.py:3128`; `app/chamados/models.py:3131`.) |
| 56 | Property `respondida=I`. (`app/chamados/models.py:3641`.) |
| 58 | I gera linha RESPOSTA ao recompor trilha; texto apenas copiado. **Não descartar o fato legado vazio:** esta é preservação histórica, diferente da classificação corrente. (`app/chamados/models.py:4067`; `app/chamados/models.py:4074`; `app/chamados/models.py:4077`.) |
| 64 | Conta ¬I para reabrir disputa. (`app/chamados/services/acoes_disputa.py:95`.) |
| 65 | Alguma viva ¬I impede regularização. (`app/chamados/services/acoes_disputa.py:130`.) |
| 67 | ¬I e destino não resolvido em becos. (`app/chamados/services/becos.py:30`.) |
| 71 | `bool(I)` informa reenvio ao escritor de declaração. (`app/chamados/services/declaracao_texto.py:419`.) |
| 72 | ¬I dispara resposta na aplicação da declaração. (`app/chamados/services/declaracao_texto.py:687`.) |
| 76 | Conta ¬I para recobrar questionário. (`app/chamados/services/disputa_emissao.py:1734`.) |
| 77 | I exclui porta de reabrir credor; exige C6 viva. (`app/chamados/services/disputa_emissao.py:1842`.) |
| 89 | Viva ¬I impede regularização no POST do fio. (`app/chamados/services/fio.py:698`.) |
| 94 | I seleciona evento temporal de resposta, com `desde`; distinguir evento legado de estado corrente antes de migrar. (`app/chamados/services/historico.py:99`; `app/chamados/services/historico.py:105`; `app/chamados/services/historico.py:106`.) |
| 107 | I + validada + não materializada seleciona lote de materialização. (`app/chamados/services/materializacao.py:1375`; `app/chamados/services/materializacao.py:1379`.) |
| 111 | ¬I + disputa aberta => pendente app. (`app/chamados/services/pergunta_no_app.py:81`.) |
| 116 | ¬I => `sem_resposta` na entrada validar. (`app/chamados/services/validacao.py:418`.) |
| 117 | ¬I => `ja_pendente` na entrada recusar. (`app/chamados/services/validacao.py:605`.) |
| 121 | I nos últimos sete dias e posterior à cobrança => contador de aceite por pessoa. (`app/chamados/services/verbos.py:335`; `app/chamados/services/verbos.py:342`; `app/chamados/services/verbos.py:345`.) |
| 123 | I sob âncora terminal => causa c do alcance. (`app/chamados/services/vigia_fio.py:694`.) |
| 128 | I informa `ja_respondida` na view responder. (`app/chamados/views.py:1280`.) |
| 129 | ¬I + C6 para pendentes vivas na reabertura. (`app/chamados/views.py:1416`; `app/chamados/views.py:1417`.) |
| 130 | Vivas I para validar; ¬I para aguardando colaborador. (`app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:176`.) |
| 137 | I antes de fio vivo no reconciliador de grade. (`app/ponto/management/commands/reconciliar_grade.py:461`.) |
| 143 | Contadores folha ¬I/I, acrescidos de disputa/validação. (`bin/censo_folha.py:28`; `bin/censo_folha.py:29`.) |
| 148 | I governa rótulo e caixa da pergunta. (`app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29`.) |
| 150 | ¬I na pergunta cega; I governa caixa de resposta. (`app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217`.) |

D após strip diverge `¬I∧T` se a pergunta feita é “respondida?”. Para guardas de conteúdo (“não plantar sem texto”, parse/display, preservar texto histórico), conservar leitura de texto e pôr J na entrada onde o corte exige resposta; não trocar a necessidade de texto por uma afirmação sobre fase. (`app/chamados/services/materializacao.py:835`; `app/chamados/templatetags/chamados_tags.py:142`.)

| R/D | Uso e observação |
|---|---|
| 8 | SQL `texto=''` encontra pergunta muda com resposta no fio; finalidade diagnóstico. Ao usar `¬J`, passa a alcançar texto sem instante e brancos; exige dados do fio para resultado final. (`app/chamados/canal.py:53`; `app/chamados/canal.py:65`; `app/chamados/canal.py:72`.) |
| 31 | `any(T)` interrompe auditor F1. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1655`.) |
| 34 | `.only(texto)` alimenta eleição canônica, pelo mesmo corpo R90. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:504`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:506`.) |
| 37 | `T` elege com resposta na deduplicação. (`app/chamados/management/commands/dedup_perguntas_fato.py:75`.) |
| 40 | Universo de ressuscitação restringe texto NULL/`''`, sem trim, além de materializada e fora do fato. `¬J` ampliaria também perguntas com texto; isso é mudança de universo, não consequência automática de “reabrir zera par”. (`app/chamados/models.py:1530`; `app/chamados/models.py:1535`.) |
| 59 | T impede lavratura de órfã. (`app/chamados/reconciliador.py:365`.) |
| 61 | T impede veredito de órfã. (`app/chamados/reconciliador.py:1083`.) |
| 74 | T impede resposta automática. (`app/chamados/services/disputa_emissao.py:1663`.) |
| 81 | T só rotula auditoria “com resposta”; carimbo usa porta própria. (`app/chamados/services/disputa_emissao.py:2078`; `app/chamados/services/disputa_emissao.py:2081`; `app/chamados/services/disputa_emissao.py:2086`.) |
| 90 | T prefere canônica com resposta. (`app/chamados/services/fio.py:1271`.) |
| 91 | T + via admin protege mérito perdedor. (`app/chamados/services/fio.py:1282`; `app/chamados/services/fio.py:1283`.) |
| 99 | T + tipo hora governa estado de aplicação. (`app/chamados/services/materializacao.py:438`.) |
| 105 | ¬T recusa plantio e gera recusa; guarda de texto deve continuar além do gate J. (`app/chamados/services/materializacao.py:835`; `app/chamados/services/materializacao.py:842`.) |
| 126 | ¬T retorna leitura vazia; apresentação de conteúdo. (`app/chamados/templatetags/chamados_tags.py:142`.) |
| 132 | SQL `texto!=''` e C6 governa indicador de calendário; J SQL exclui `¬I∧T` e brancos com qualquer I. (`app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/calendario.py:476`.) |
| 135 | ¬T e fora do conjunto sem veredito => sem resposta na ficha; migrar em conjunto com B, pois o `falou` muda. (`app/colaboradores/services/ficha.py:169`; `app/colaboradores/services/ficha.py:171`.) |
| 136 | `any(T)` => evidência de fala na prévia; J se a descrição passar a ser “respondida”, distinguir preservação de evidência textual. (`app/folha/previa.py:139`; `app/folha/previa.py:142`.) |
| 138 | `any(T)` isenta resposta do cartório/dedup. (`app/ponto/services/cartorio.py:1637`; `app/ponto/services/cartorio.py:1641`; `app/ponto/services/cartorio.py:1648`.) |
| 145 | Verdade crua de texto governa ramos de exibição; se for rótulo “respondida”, J; se mero conteúdo, permanece apresentação. Diverge também brancos. (`app/templates/chamados/partials/_card_atendimento.html:141`; `app/templates/chamados/partials/_card_atendimento.html:182`.) |

F é leitura de conteúdo/data e não decisão de “respondida”; não converter payload/texto em booleano. Existem dependências dos gates e da garantia do par:

- R1/R3/R6 payload app, datas e texto. (`app/api/views.py:1163`; `app/api/views.py:1165`; `app/api/views.py:2561`; `app/api/views.py:2563`; `app/api/views.py:2637`.)
- R7 item da fila assume I e chama `.isoformat()`/subtração sem guarda; a fila nova E preserva essa premissa. (`app/api/views_mensageria.py:567`; `app/api/views_mensageria.py:569`; `app/api/views_mensageria.py:570`.)
- R39 amostra de resposta. (`app/chamados/management/commands/validar_classe_a.py:80`.)
- R57 resolve rótulo e devolve texto original. (`app/chamados/models.py:3664`; `app/chamados/models.py:3669`; `app/chamados/models.py:3677`; `app/chamados/models.py:3684`; `app/chamados/models.py:3686`.)
- R83/R84 texto na explicação. (`app/chamados/services/explicador.py:32`; `app/chamados/services/explicador.py:79`; `app/chamados/services/explicador.py:80`.)
- R95 trecho de texto no evento de linha do tempo. (`app/chamados/services/historico.py:109`.)
- R120 idade da fila. (`app/chamados/services/validacao.py:959`.)
- R131 amostra da proposta. (`app/chamados/views_cobrar.py:291`.)
- R139 resposta no dia do colaborador. (`app/ponto/services/dia_do_colab.py:70`.)
- R144 conteúdo/data/input. (`app/templates/chamados/partials/_card_atendimento.html:106`; `app/templates/chamados/partials/_card_atendimento.html:143`; `app/templates/chamados/partials/_card_atendimento.html:148`; `app/templates/chamados/partials/_card_atendimento.html:183`; `app/templates/chamados/partials/_card_atendimento.html:391`.)
- R147 preview de lote. (`app/templates/chamados/partials/_lote_preview.html:24`.)
- R149 resposta formatada/data depois do gate R148. (`app/templates/chamados/partials/_pergunta_card.html:31`; `app/templates/chamados/partials/_pergunta_card.html:32`.)
- R151 conteúdo/data depois do gate R150. (`app/templates/chamados/partials/modal_fio.html:219`; `app/templates/chamados/partials/modal_fio.html:220`.)

Complementos do censo:

- `_item` da API lê `p.estado`, serializa fase e calcula `aguarda_admin=RESPONDIDA`: migrar por autoridade de `estado`; delta `I∧¬T` somente onde as precedências não produzem VALIDADA/RESOLVIDA. (`app/api/views.py:2550`; `app/api/views.py:2552`; `app/api/views.py:2553`; `app/chamados/models.py:3124`; `app/chamados/models.py:3128`.)
- `explicar_pergunta` escolhe dono/ação pelo estado PENDENTE/RESPONDIDA, além da exibição R84: herda a mesma mudança. (`app/chamados/services/explicador.py:72`; `app/chamados/services/explicador.py:73`; `app/chamados/services/explicador.py:77`.)
- `tarefa_do_dia._pela_decisao` lê última DP: RESPOSTA com `ato_texto` vazio => responder; com texto => validar. É gate por ato/status, não o mesmo par da PerguntaDisputa; não confundir com autorização de trocar a autoridade temporal inteira. (`app/chamados/services/tarefa_do_dia.py:147`; `app/chamados/services/tarefa_do_dia.py:151`.)
- A carga da tarefa lê atos ordenados/cortados por horizonte e delega pendentes à mesma partição app. (`app/chamados/services/tarefa_do_dia.py:410`; `app/chamados/services/tarefa_do_dia.py:420`; `app/chamados/services/tarefa_do_dia.py:513`; `app/chamados/services/tarefa_do_dia.py:514`.)
- Comparações VALIDADA/RESOLVIDA não decidem “respondida”: fio registra ato após validação; vigia identifica aceite de folga validado; template rotula resolvida. Não migrar essas comparações de fase para J. (`app/chamados/services/fio.py:652`; `app/chamados/services/vigia_fio.py:311`; `app/templates/chamados/partials/_card_atendimento.html:189`.)

Sobre produção: não há no dado anexado uma contagem observada por estado/sítio. “143 vivas” é previsão literal do aval, não prova de que só 143 leituras mudam. Existem leitores C sobre consolidadas, por exemplo o lote de materialização com `validada_em!=NULL`, e `respondida` não filtra fase; logo o script deve enumerar também `I∧¬T` validada/materializada sem alterá-las. (`app/chamados/services/materializacao.py:1375`; `app/chamados/services/materializacao.py:1379`; `app/chamados/models.py:3641`; números em produção: [nao verificado].)

## 15/11 20:00 · sessao 022007b1addb · - · - · origem=Ronald (tmux)

Para a sonda, a competência deve vir de `competencia_do_dia_entregue`: ela compõe `janela_atual` com o corte da empresa e `competencia_entregue`; devolve `None` sem dia. “Entregue” significa existir `ExportacaoDominio` vigente e não revogada, sem depender de tranca ou de disputa aberta. (`app/chamados/services/vigia_fio.py:843`; `app/chamados/services/vigia_fio.py:859`; `app/folha/export.py:556`; `app/folha/export.py:588`.)

Vitalidade deve vir do C6 real. `q_vivas` garante somente continência; além disso, uma pergunta materializada pode continuar viva, portanto “viva” e “validada/materializada” precisam aparecer em eixos separados. (`app/chamados/models.py:3436`; `app/chamados/juizes.py:1420`; `app/chamados/juizes.py:1432`.)

O C6 engole algumas exceções e devolve um resultado conservador. A sonda precisa conferir o contador de engolidos e classificar aquela leitura como erro, evitando contar a falha como pergunta morta. (`app/chamados/juizes.py:1476`; `app/chamados/juizes.py:1489`; `app/core/observ.py:60`; `app/core/observ.py:73`.)

Script proposto, sem execução nesta cópia: **[nao verificado]**. Pressupõe um shell Django já configurado no tenant correto, iniciado com `PYTHONDONTWRITEBYTECODE=1`; não lê arquivos de log. **[nao verificado]**

```python
import io
import json
import logging
from collections import Counter
from contextlib import redirect_stdout, redirect_stderr

from django.db import connection, transaction
from django.utils import timezone

from chamados.models import PerguntaDisputa
from chamados.juizes import (
    pergunta_viva,
    q_respondida_com_texto,
    texto_vazio,
    um_colaborador_por_identidade,
)
from chamados.services.vigia_fio import competencia_do_dia_entregue
from core.observ import engolidos

# Fontes:
# campos: chamados/models.py:2525-2527
# vazio: chamados/juizes.py:1523-1525
# face SQL atual: chamados/juizes.py:1559
# vida exata: chamados/juizes.py:865
# competência: chamados/services/vigia_fio.py:843-865
# precedência V/M no estado: chamados/models.py:3124-3131
#
# As famílias abaixo medem diferenças do componente "respondida".
# Os demais filtros e agregações de cada sítio precisam ser considerados
# na interpretação; estes números não equivalem ao resultado final
# de todas as filas, portas ou contadores.
#
# O alvo de estoque segue o corte fornecido, não executa sua correção.

universo = 0
erros = []
estados = Counter()
deltas = Counter()
linhas = []
estoque = {"aberta": [], "entregue": [], "indecidivel": []}
memo_competencias = {}
fatal = None
nivel_anterior = logging.root.manager.disable

def emitir(obj):
    print(json.dumps(obj, ensure_ascii=False, sort_keys=True))

def medir(func, *args, **kwargs):
    antes = engolidos()["total"]
    try:
        # Impede impressão incidental de nomes/textos pelos leitores.
        with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
            valor = func(*args, **kwargs)
    except Exception as exc:
        return None, type(exc).__name__
    if engolidos()["total"] != antes:
        return None, "erro_engolido"
    return valor, None

try:
    if connection.vendor != "postgresql":
        raise RuntimeError("sonda_exige_postgresql")
    if connection.in_atomic_block:
        raise RuntimeError("iniciar_fora_de_atomic")
    if getattr(connection, "schema_name", None) in (None, "public"):
        raise RuntimeError("tenant_nao_selecionado")

    # Evita handlers de logging gravarem mensagens incidentais.
    # O contador de engolidos continua funcionando.
    logging.disable(logging.CRITICAL)

    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )

        agora = timezone.now()
        universo = PerguntaDisputa.objects.count()

        # Universo inteiro: não usar q_vivas como se fosse C6 exato.
        qs = (
            PerguntaDisputa.objects
            .select_related(
                "disputa__colaborador__empresa",
                "celula",
                "chamado_destino",
                "chamado_destino__celula",
            )
            .order_by("pk")
        )
        perguntas = list(um_colaborador_por_identidade(qs))

        ids_sql = set(
            PerguntaDisputa.objects.filter(q_respondida_com_texto())
            .values_list("pk", flat=True)
        )

        for p in perguntas:
            I = p.respondida_em is not None
            T = not texto_vazio(p.resposta_colab)
            novo = I and T
            bruto = bool(p.resposta_colab)
            diferente_de_vazio = (
                p.resposta_colab is not None and p.resposta_colab != ""
            )
            formato = (
                "NULL" if p.resposta_colab is None
                else "vazio_exato" if p.resposta_colab == ""
                else "brancos" if not T
                else "texto"
            )
            par = (
                "I_T" if I and T
                else "I_sem_T" if I
                else "T_sem_I" if T
                else "sem_I_sem_T"
            )
            protecao = (
                ("V" if p.validada_em is not None else "-")
                + ("M" if p.materializada_em is not None else "-")
            )
            disputa = (
                "aberta" if p.disputa.fechada_em is None else "fechada"
            )

            viva, erro_viva = medir(pergunta_viva, p, agora=agora)
            entregue, erro_comp = medir(
                competencia_do_dia_entregue,
                p.data_evento,
                p.disputa.colaborador.empresa,
                memo=memo_competencias,
            )
            comp = (
                "indecidivel" if erro_comp or entregue is None
                else "entregue" if entregue
                else "aberta"
            )
            vida = (
                "erro" if erro_viva
                else "viva" if viva
                else "morta"
            )

            for etapa, erro in (
                ("vitalidade", erro_viva),
                ("competencia", erro_comp),
            ):
                if erro:
                    erros.append({
                        "pergunta": p.pk,
                        "pessoa": f"col{p.disputa.colaborador_id:03d}",
                        "etapa": etapa,
                        "erro": erro,
                    })

            if (p.pk in ids_sql) != novo:
                erros.append({
                    "pergunta": p.pk,
                    "etapa": "equivalencia_face_sql",
                    "erro": "python_sql_divergentes",
                })

            chave = (comp, vida, protecao, disputa, par, formato)
            estados[chave] += 1

            antigos = {
                "A_instante_E_texto": I and T,
                "B_instante_OU_texto": I or T,
                "C_so_instante": I,
                "D_so_texto_strip": T,
                "D_texto_verdade_crua": bruto,
                "D_texto_diferente_vazio_sem_trim": diferente_de_vazio,
                "A_instante_E_texto_sem_trim": I and diferente_de_vazio,
            }
            familias = [
                nome for nome, antigo in antigos.items()
                if bool(antigo) != novo
            ]
            for familia in familias:
                deltas[(familia,) + chave] += 1

            alvo = (
                not erro_viva and bool(viva)
                and protecao == "--"
                and I and not T
            )
            if alvo:
                estoque[comp].append(p.pk)

            # Não imprime resposta, nome, CPF nem conteúdo de exceção.
            if familias or alvo:
                linhas.append({
                    "pergunta": p.pk,
                    "pessoa": f"col{p.disputa.colaborador_id:03d}",
                    "competencia": comp,
                    "vida": vida,
                    "protecao_VM": protecao,
                    "disputa": disputa,
                    "par": par,
                    "formato_texto": formato,
                    "familias_divergentes": familias,
                    "alvo_estoque": alvo,
                })

except Exception as exc:
    fatal = type(exc).__name__
    erros.append({"etapa": "fatal", "erro": fatal})
finally:
    logging.disable(nivel_anterior)

# Primeira linha da SAÍDA: universo e total de erros.
emitir({"universo": universo, "total_erros": len(erros), "fatal": fatal})

for chave, n in sorted(estados.items()):
    comp, vida, protecao, disputa, par, formato = chave
    emitir({
        "tipo": "estado", "competencia": comp, "vida": vida,
        "protecao_VM": protecao, "disputa": disputa,
        "par": par, "formato_texto": formato, "n": n,
    })

for chave, n in sorted(deltas.items()):
    familia, comp, vida, protecao, disputa, par, formato = chave
    emitir({
        "tipo": "divergencia_do_componente",
        "familia": familia, "competencia": comp, "vida": vida,
        "protecao_VM": protecao, "disputa": disputa,
        "par": par, "formato_texto": formato, "n": n,
    })

for linha in linhas:
    emitir({"tipo": "pergunta_divergente", **linha})
for erro in erros:
    emitir({"tipo": "erro", **erro})

emitir({
    "tipo": "confronto_com_corte_fornecido",
    "esperado": {
        "total": 143,
        "aberta_pks": [38251, 38252],
        "entregue_n": 141,
    },
    "medido": {
        "total": sum(map(len, estoque.values())),
        "aberta_pks": estoque["aberta"],
        "entregue_n": len(estoque["entregue"]),
        "entregue_pks": estoque["entregue"],
        "indecidivel_pks": estoque["indecidivel"],
    },
    "medicao_completa": fatal is None and not erros,
})
```

`ΔB` alcança as duas metades ilegais; `ΔC` alcança instante sem texto; `ΔD` com `strip` alcança texto sem instante; igualdade com `''` ou verdade crua também pode divergir em brancos. Esses são estados candidatos de mudança do componente; filtros de validação, materialização, vitalidade, prazo, agrupamento e existência ainda podem impedir mudança do resultado final. (`app/chamados/models.py:1905`; `app/chamados/models.py:3124`; `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/juizes.py:1523`.)

Quantidade efetivamente encontrada, igualdade aos 143 esperados e distribuição das 141 entregues: **[nao verificado]**.

## 15/11 20:00 · sessao e34ba78245e1 · FERIADO-USA-A-LEI-DE-1609 · sonda · origem=Code

Transcrição do dado fornecido [nao verificado]:

```text
UNIVERSO HOJE=2026-11-15 lote=15 pks=[328, 1111, 1208, 1247, 1264, 1287, 1303, 1349, 1350, 1355, 1358, 1398, 1409, 1425, 1430]
EC 1208 col940 no_lote=True gravado=True lei=False ciclo=5x2 ini=2026-09-11 fim=None
EC 1255 col945 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-09-11 fim=None
EC 1418 col985 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-10-14 fim=None
EC 1421 col987 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-10-13 fim=None
EC 1424 col990 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-10-14 fim=None
EC 1517 col1018 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-11-04 fim=None
EC 1522 col1023 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-11-03 fim=None
EC 1523 col1024 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-11-04 fim=None
total_erros=0
```

Só o **EC 1208 (col940)** está no lote (`no_lote=True`), segundo a saída fornecida. [nao verificado]

## 15/11 19:59 · sessao 1a301cb1c9f7 · FERIADO-USA-A-LEI-DE-1609 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 1: PROCEDE; item 2: PROCEDE; item 3: NÃO PROCEDE como escrito; item 4: PROCEDE**, pelos fundamentos abaixo (`app/escala/models.py:1096`; `app/ponto/services/fechamento.py:1607`; `CLAUDE.md:269`).

As referências abaixo são da cópia fornecida. A identidade Git dessa cópia com o HEAD `988df8bfb7b341382d5b84dea73b9b9c18a84032` é **[nao verificado]**: não há metadados Git disponíveis.

**1. CORTE — PROCEDE como candidata condicionada à medida.**

As premissas centrais conferem, com uma precisão: a flag não é lida incondicionalmente para toda data; o caminho aritmético só consulta `feriado_vira_folga` quando `base is True`, e o predicado só suprime trabalho nos ciclos declarados (`app/escala/models.py:1369`; `app/escala/models.py:1390`; `app/escala/models.py:1392`).

- Havendo célula, `eh_dia_trabalho` devolve `cel.trabalha`; sem célula, chama `eh_dia_trabalho_calculado`. Essa cadeia não testa a vigência antes da consulta à flag (`app/escala/models.py:1092`; `app/escala/models.py:1095`; `app/escala/models.py:1096`; `app/escala/models.py:1369`; `app/escala/models.py:1390`).
- Os endereços completos dos três leitores externos são **`app/ponto/services/fechamento.py:949`**, **`app/ponto/precedencia.py:377`** e **`app/ponto/calculador/alimentacao.py:521`**. Os nomes sem pasta do aval são abreviações; não identificam arquivos na raiz.
- Existe também o leitor interno, usado pelo gerador: `app/escala/models.py:1369`. Outros leitores diretos da **flag** aparecem no contador, na sincronização e nas cópias de trechos (`app/escala/models.py:929`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:48`; `app/colaboradores/services/vinculo.py:974`; `app/colaboradores/services/vinculo.py:1092`; `app/colaboradores/services/vinculo.py:1113`).
- O gerador atual restringe a geração à cobertura do vínculo antes de chamar a aritmética. Portanto, preservar seu comportamento **dentro** da vigência é compatível com o desenho; a preservação pela implementação futura é **[nao verificado]** (`app/ponto/portas/celula.py:906`; `app/ponto/portas/celula.py:911`).

Não identifiquei colisão de **“SEM célula e FORA da vigência não lê a flag”** com os contratos citados:

- L-020: “CELULA SOBERANA: dia + marcos sao DADO com DNA congelado”; o corte proposto se limita justamente à ausência dessa testemunha (`app/docs/LEIS.md:71`).
- Seção 4: a grade é fonte única; os métodos leem a célula, com fallback aritmético/template (`CLAUDE.md:148`; `CLAUDE.md:155`).
- Sem célula, o contrato de `vinculo_do_dia` declara “a aritmetica do vinculo que cobre a data”; E1 escreve “a geradora da celula so vale DENTRO da vigencia” e “Vigencia, nao o flag ativo” (`app/escala/alimentacao.py:244`; `app/escala/alimentacao.py:272`).
- Os cortes registram uma autoridade para vínculo do dia e outra para cobertura, ambas já existentes; não é necessário inventar outro juiz (`app/docs/CORTES.md:335`; `app/docs/CORTES.md:338`).
- A lei de 16/09 decide como um vínculo **NOVO** nasce: posto declarado vence; sem declaração, comerciais 5x2/6x1 folgam; vínculo existente não passa novamente pelo padrão. Ela não determina o uso desse flag antes do nascimento do vínculo (`app/escala/models.py:900`; `app/escala/models.py:906`; `app/escala/models.py:908`; `app/escala/models.py:916`).

Isso não decide que todo dia anterior ao vínculo deva deixar de contar no banco: essa é a pergunta distinta do item 4 (`app/ponto/motor_calculo_v2.py:3449`; `app/ponto/motor_calculo_v2.py:3451`).

**2. MEDIR ANTES — PROCEDE como medida de leitura; condição ainda não demonstrada.**

A medida deve alcançar os dias efetivamente consultados pelo fechamento, incluindo a semana completa usada pelo banco: o motor recua até segunda-feira e consulta os sete dias, mesmo quando parte deles está fora do período apurado (`app/ponto/motor_calculo_v2.py:3436`; `app/ponto/motor_calculo_v2.py:3441`; `app/ponto/motor_calculo_v2.py:3449`).

A população da frota inteira, sua partição ENTREGUE/aberta e o resultado “ENTREGUE fora do lote = 0” **não constam do dado fornecido [nao verificado]**. O resumo de 15 vínculos/31 colab-competências não demonstra essa condição de frota **[nao verificado]**.

Também não cabe substituir “todos os vínculos” por `ativa=True`: a cobertura inclui vínculo encerrado com fim declarado, e a escolha sem célula passa pelo juiz de cobertura (`app/escala/alimentacao.py:249`; `app/escala/alimentacao.py:285`).

**3. CONDIÇÃO LITERAL — NÃO PROCEDE como escrito.**

Há quatro pontos que impedem certificar o literal:

1. **“31/31 iguais” depende dos dois lados da comparação.** Comparar **antes do lote** com **lote + cura** pode fechar com col940 estabilizado. Comparar a sombra anterior, já com col940 alterado, com a sombra curada exige que esse próprio caso mude para recuperar o valor anterior. A referência escolhida pelo aval é **[nao verificado]**. O mecanismo que permite essa diferença está na consulta semanal ao fallback (`app/ponto/motor_calculo_v2.py:3449`; `app/escala/models.py:1096`; `app/escala/models.py:1369`).

2. **Não há prova de identidade do lote desde 13/11.** O histórico escrito menciona 16 vínculos, enquanto o impacto registrado menciona 15/31. Esses textos demonstram divergência entre registros; não demonstram quem saiu, por quê, nem que o universo mudou em produção (`app/docs/BACKLOG.md:729`; `app/docs/RELATO.md:4`). Identidade congelada do lote: **[nao verificado]**.

3. **“Qualquer campo gravado” inclui carimbos.** A porta salva a flag; o `post_save` chama `invalidar_previsto`; essa função zera `previsto_em` e muda `atualizado_em` sem filtro de competência/exportação (`app/colaboradores/services/vinculo.py:894`; `app/escala/signals.py:137`; `app/escala/signals.py:146`; `app/ponto/services/fechamento.py:1611`). Se algum fechamento ENTREGUE dos alvos tiver `previsto_em` preenchido, o literal falha mesmo com dinheiro estável. A existência desses registros é **[nao verificado]**. O achado já está registrado fora do escopo (`app/docs/BACKLOG.md:730`).

4. **A sombra padrão não prova todos os campos.** O retorno recortado omite `motivos_espelho`, `causa_espelho`, `previsto_em` e `atualizado_em`; além disso, `previsto_em` é produzido com relógio. Igualdade numérica e preservação do registro precisam ser verificadas separadamente; o retorno padrão sozinho não certifica o literal (`app/ponto/services/fechamento.py:454`; `app/ponto/services/fechamento.py:1086`; `app/ponto/services/fechamento.py:1128`).

O **caso oposto já está coberto no código de teste**: vínculo iniciado em 01/01, feriado em 21/04, flag False resulta em folga; True mantém trabalho (`app/escala/tests/test_escala_models.py:255`; `app/escala/tests/test_escala_models.py:268`; `app/escala/tests/test_escala_models.py:275`). Há ainda vínculo com vigência delimitada e feriado interno suprimido pela flag False (`app/ponto/tests/test_precedencia_vinculo_encerrado.py:43`; `app/ponto/tests/test_precedencia_vinculo_encerrado.py:68`). Execução desses testes nesta revisão: **[nao verificado]**.

Quanto à Q4, **não é possível afirmar que nenhuma outra competência exportada muda [nao verificado]**. Alterar `models.py` pode mudar dinheiro sem editar o motor: ele consulta o método do vínculo, usa a contagem como denominador e calcula o saldo; o fechamento persiste esse saldo (`app/ponto/motor_calculo_v2.py:3449`; `app/ponto/motor_calculo_v2.py:3451`; `app/ponto/motor_calculo_v2.py:3455`; `app/ponto/services/fechamento.py:1180`). O mesmo predicado também seleciona rubricas de feriado (`app/ponto/services/fechamento.py:949`; `app/ponto/services/fechamento.py:955`; `app/ponto/calculador/alimentacao.py:521`).

A cura isolada pode ser uma alteração de leitura; o lote, porém, escreve vínculo e regenera células, além da invalidação descrita acima (`app/colaboradores/services/vinculo.py:894`; `app/colaboradores/services/vinculo.py:904`; `app/colaboradores/services/vinculo.py:910`). “Motor intocado” não certifica “dinheiro intocado”; dado de vínculo/escala e gravado exportado continuam sujeitos ao escopo literal do `!` (`CLAUDE.md:265`; `CLAUDE.md:267`; `CLAUDE.md:411`).

**4. REGISTRAR `lei:` SEM DECIDIR — PROCEDE.**

Registrar a pergunta sobre os dias anteriores ao início do vínculo, sem implementar resposta, corresponde à L-117 e à PARADA 3 (`app/docs/LEIS.md:138`; `CLAUDE.md:269`).

E1 resolve **qual vínculo vale na data**; o banco atual pergunta ao mesmo vínculo pelos sete dias da semana. Logo, a candidata (a) não resolve por si toda a pergunta (b), e esta revisão não autoriza alteração do motor nem do gravado exportado (`app/escala/alimentacao.py:272`; `app/escala/alimentacao.py:285`; `app/ponto/motor_calculo_v2.py:3433`; `app/ponto/motor_calculo_v2.py:3449`; `CLAUDE.md:142`; `CLAUDE.md:411`).

**Quem escreve e quem lê `trabalha_em_feriado`.**

| Papel | Arquivo:linha |
|---|---|
| Declara o campo/default | `app/escala/models.py:896` |
| Decide o padrão de nascimento | `app/escala/models.py:905` |
| Cria vínculo com o padrão | `app/colaboradores/services/vinculo.py:296` |
| Altera flag existente | `app/colaboradores/services/vinculo.py:893`; `app/colaboradores/services/vinculo.py:894` |
| Lê e copia para trechos | `app/colaboradores/services/vinculo.py:974`; `app/colaboradores/services/vinculo.py:1092`; `app/colaboradores/services/vinculo.py:1113` |
| Lê para decidir feriado | `app/escala/models.py:1390` |
| Lê para contador | `app/escala/models.py:929` |
| Lê na sincronização; escreve pela porta | `app/colaboradores/management/commands/sincronizar_feriado_admin.py:48`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:56` |

Os quatro leitores executáveis encontrados de `feriado_vira_folga` são `app/escala/models.py:1369`, `app/ponto/services/fechamento.py:949`, `app/ponto/precedencia.py:377` e `app/ponto/calculador/alimentacao.py:521`. Leitores indiretos relevantes incluem o motor e os caminhos de previsto (`app/ponto/motor_calculo_v2.py:2448`; `app/ponto/motor_calculo_v2.py:3449`; `app/ponto/services/fechamento.py:731`; `app/ponto/calculador/alimentacao.py:445`; `app/ponto/precedencia.py:538`).

**Quem escreve `CelulaDia`.**

| Operação | Arquivo:linha |
|---|---|
| Geração/regeneração | `app/ponto/portas/celula.py:265`; `app/ponto/portas/celula.py:279`; `app/ponto/portas/celula.py:293`; `app/ponto/portas/celula.py:310`; `app/ponto/portas/celula.py:914`; `app/ponto/portas/celula.py:926` |
| Edição humana | `app/ponto/portas/celula.py:638`; `app/ponto/portas/celula.py:647` |
| Reversão | `app/ponto/portas/celula.py:836` |
| Veredito/ata/status | `app/ponto/portas/celula.py:1098` |
| Persistência comum e carimbos | `app/ponto/portas/celula.py:957`; `app/ponto/portas/celula.py:1201`; `app/ponto/portas/celula.py:1216`; `app/ponto/portas/celula.py:1237` |
| Efeito referencial ao remover vínculo | O próprio código reconhece escrita por `SET_NULL` e recusa referência humana antes da remoção: `app/colaboradores/services/vinculo.py:1023`; `app/colaboradores/services/vinculo.py:1029` |

**Quem lê `CelulaDia`.** Os leitores dos métodos do vínculo incluem previsto, marcos, pausas e extra declarada (`app/escala/models.py:1092`; `app/escala/models.py:1122`; `app/escala/models.py:1187`; `app/escala/models.py:1232`). A lista abaixo registra entradas de consulta ORM por arquivo, incluindo aliases; consumidores transitivos de mapas recebidos não constituem censo exaustivo **[nao verificado]**.

```text
app/chamados/juizes.py:411
app/chamados/models.py:1287
app/chamados/reconciliador.py:693
app/chamados/rejuncao.py:88
app/chamados/veredito_lavrado.py:191
app/chamados/services/adesao.py:33
app/chamados/services/celula_carimbada.py:96
app/chamados/services/disputa_emissao.py:951
app/chamados/services/fabrica_da_celula.py:299
app/chamados/services/fila_por_causa.py:202
app/chamados/services/fio.py:1073
app/chamados/services/foto_e4b.py:269
app/chamados/services/furo_sem_canal.py:78
app/chamados/services/materializacao.py:171
app/chamados/services/resposta_tardia.py:44
app/chamados/services/silencio_ausencia.py:41
app/chamados/services/vigia_fio.py:378
app/chamados/management/commands/auditar_invariantes_chamados.py:266
app/chamados/management/commands/backfill_fio_morto_e4b.py:315
app/chamados/management/commands/carimbar_por_batida_coincidente.py:82
app/chamados/management/commands/diff_e3_sombra.py:114
app/chamados/management/commands/disparar_perguntas_competencia.py:178
app/chamados/management/commands/materializar_passivo_validado.py:223
app/chamados/management/commands/nomear_marco_por_lampada.py:59

app/colaboradores/views.py:2874
app/colaboradores/services/calendario.py:411
app/colaboradores/services/feriado_do_posto.py:94
app/colaboradores/services/pos_vinculo.py:62
app/colaboradores/services/vinculo.py:1025
app/colaboradores/management/commands/corrigir_escala_retroativa.py:98
app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:46
app/core/views.py:396

app/escala/alimentacao.py:75
app/escala/folgas.py:621
app/escala/regua_defesa.py:687
app/escala/servico_jornada.py:47
app/escala/utils.py:49
app/escala/services/cadastro_realidade.py:166
app/escala/services/escala_auto_executor.py:57
app/escala/services/fase_12x36.py:188
app/escala/services/furos_diarios.py:313
app/escala/services/leitor_celula.py:362
app/escala/services/propositor.py:472
app/escala/services/regeneracao.py:101
app/escala/services/semana_do_posto.py:85
app/escala/management/commands/celulas_contra_o_template.py:29

app/folha/export.py:949
app/folha/porta_export.py:532
app/folha/previa.py:50
app/folha/validacao_pdf.py:44
app/folha/services/sem_furo.py:20
app/folha/services/vigia_pausa.py:73
app/pautas/services.py:423

app/ponto/marcos.py:155
app/ponto/nucleo.py:92
app/ponto/portas/celula.py:226
app/ponto/signals.py:127
app/ponto/turnos.py:1586
app/ponto/worklist.py:21
app/ponto/services/cadastro_futuro.py:31
app/ponto/services/cartorio.py:390
app/ponto/services/cluster_espurio.py:46
app/ponto/services/dia_do_colab.py:16
app/ponto/services/diagnostico_escala.py:135
app/ponto/services/esmeril_espelho.py:60
app/ponto/services/faltas_de_hoje.py:42
app/ponto/services/fechamento.py:605
app/ponto/services/flip_auto.py:211
app/ponto/services/furos_sem_cobranca.py:61
app/ponto/services/furos_vetados.py:61
app/ponto/services/lastro.py:193
app/ponto/services/passivo_s84.py:94
app/ponto/services/pautas_dos_discordantes.py:72
app/ponto/services/veredito_celula.py:30
app/ponto/services/vigia_ausencia_veredito.py:42
app/ponto/services/vigia_cadastro_futuro.py:31
app/ponto/services/vigia_de_hora.py:21
app/ponto/services/vigia_entrada.py:78
app/ponto/services/vigia_pareador_celula.py:69
app/ponto/management/commands/aplicar_09_corte_b.py:137
app/ponto/management/commands/aplicar_col900.py:163
app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:47
app/ponto/management/commands/backfill_b5_insumos.py:60
app/ponto/management/commands/celula_veredito_velho.py:86
app/ponto/management/commands/censo_col900.py:104
app/ponto/management/commands/censo_fase_12x36.py:393
app/ponto/management/commands/curar_residuo_bug115.py:81
app/ponto/management/commands/detectar_ausencias.py:469
app/ponto/management/commands/detectar_intervalo_ausente.py:46
app/ponto/management/commands/diff_calculador.py:274
app/ponto/management/commands/diff_espelho_alimentacao.py:99
app/ponto/management/commands/dna_x_batida_real.py:58
app/ponto/management/commands/emitir_furo_retroativo.py:69
app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86
app/ponto/management/commands/gerar_celulas.py:316
app/ponto/management/commands/lavrar_status_competencia.py:42
app/ponto/management/commands/plano_b_no_dinheiro.py:142
app/ponto/management/commands/processar_cartorio.py:49
app/ponto/management/commands/recusar_ponta_pequena.py:124
app/ponto/management/commands/supra_juiz.py:165
app/ponto/management/commands/tripwire_celulas.py:21
app/ponto/management/commands/vigia_b5_ausencia.py:54

app/relatorios/cartao_pela_celula.py:286
app/relatorios/dia_da_frota.py:34
```

Anexo existente: [JUIZES.md](/home/ronald/codex_ws/988df8bfb7b341382d5b84dea73b9b9c18a84032/app/docs/JUIZES.md:101). Ali constam feriado, célula/precedência e vínculo/cobertura; o próprio documento distingue sua tabela de consulta do registro vivo (`app/docs/JUIZES.md:101`; `app/docs/JUIZES.md:112`; `app/docs/JUIZES.md:130`; `app/docs/JUIZES.md:10`).

## 15/11 19:56 · sessao 37c3dd6890b3 · - · - · origem=Ronald (tmux)

`trabalha_em_feriado` — censo de referências executáveis de produção:

| Papel | Quem |
|---|---|
| Declara/default | `app/escala/models.py:896` (`default=True` em :897). |
| Decide padrão novo (16/09) | `app/escala/models.py:905`; posto declarado vence em :911-913; comercial 5x2/6x1 em :916-917. |
| Escreve ao criar vínculo | `app/colaboradores/services/vinculo.py:287`, valor pelo padrão em :296. |
| Escreve alteração existente | `app/colaboradores/services/vinculo.py:869`, compara/lê :890-892, atribui e salva :893-894. |
| Lê/copia em novos trechos | `app/colaboradores/services/vinculo.py:974`, :1092, :1113. |
| Lê para regra feriado | `app/escala/models.py:1390`. |
| Lê para contador vigentes | `app/escala/models.py:929`. |
| Lê no comando de sincronização | `app/colaboradores/management/commands/sincronizar_feriado_admin.py:48`, :52; alteração passa pela porta em :56. |

`feriado_vira_folga` — quatro leitores executáveis encontrados; o aval nomeia os três externos e omite o gerador: `app/escala/models.py:1369`; `app/ponto/services/fechamento.py:949`; `app/ponto/precedencia.py:377`; `app/ponto/calculador/alimentacao.py:521`.

Leitores indiretos relevantes da flag pelo fallback `eh_dia_trabalho → eh_dia_trabalho_calculado → feriado_vira_folga`: `app/ponto/motor_calculo_v2.py:2448`, :3449 (previstos semanais BH); `app/ponto/services/fechamento.py:731`; `app/ponto/calculador/alimentacao.py:445`; `app/ponto/precedencia.py:538`. A cadeia está em `app/escala/models.py:1094-1096` e :1369. Portanto, deixar o arquivo do motor intocado não prova que sua saída ficará igual; os leitores mencionados estão no motor (`app/ponto/motor_calculo_v2.py:2448`, :3449).

`CelulaDia` — escritores:

- Porta única declarada: `app/core/portas.py:107-112`.
- Geração/regeneração, gravações reais: `app/ponto/portas/celula.py:265`, :279, :293, :310, :914, :926; gerador consulta a aritmética em :289 e :911.
- Edição humana: `app/ponto/portas/celula.py:572`, criação :638 e atualização :647.
- Reversão de regeneração: `app/ponto/portas/celula.py:779`, atualização :836.
- Veredito/ata/status: `app/ponto/portas/celula.py:1032-1036`, atribuições :1091-1096 e gravação :1098; método da entidade só delega (`app/escala/models.py:1819-1828`).
- Carimbo: `app/ponto/portas/celula.py:1181-1201`; precedência na ata :1205; insumos em lote :1237; reset do julgamento futuro :1124.
- Persistência central comum: `app/ponto/portas/celula.py:957-965`.

`CelulaDia` — leitores centrais para esta revisão:

| Pergunta | Leitor |
|---|---|
| Carga da janela | `app/escala/alimentacao.py:54`, query :75. |
| Vínculo do dia | `app/escala/alimentacao.py:269-286` (geradora e vigência). |
| Dia previsto | `app/escala/models.py:1069`, query :1092, retorno :1095. |
| Marcos/DNA e horários | `app/escala/models.py:1122`, :1187, :1232. |
| Grade/célula | `app/escala/utils.py:1054`, :2138; seleção da geradora :2181, :2191. |
| Precedência | `app/ponto/precedencia.py:308` (carga); juiz declarado em `app/docs/JUIZES.md:114-115`. |
| Fechamento | `app/ponto/services/fechamento.py:604-607`. |
| Motor | `app/ponto/motor_calculo_v2.py:2448`, :3449 por `eh_dia_trabalho`; leitura de célula pelo método em `app/escala/models.py:1089-1095`. |
| Alimentação do calculador | `app/ponto/calculador/alimentacao.py:445` pelo mesmo método. |
| Exportação/folha | `app/folha/export.py:949`, :1247; `app/folha/previa.py:50`; `app/folha/validacao_pdf.py:44`; `app/folha/porta_export.py:532`. |
| Cartório | `app/ponto/services/cartorio.py:390`, :2027. |
| Turnos/núcleo | `app/ponto/turnos.py:1586`, :2334; `app/ponto/nucleo.py:92`. |
| Status/veredito | `app/ponto/services/veredito_celula.py:30`, :107, :153; autoridade de status em `app/docs/JUIZES.md:120-121`. |
| Chamados/porta | `app/chamados/juizes.py:411`, :1286, :1631; `app/chamados/models.py:1287`, :2979, :3325, :3327; `app/chamados/reconciliador.py:693`, :704, :868, :879, :1027, :1295, :1307. |
| Relatórios/telas | `app/relatorios/cartao_pela_celula.py:286`; `app/relatorios/dia_da_frota.py:34`; `app/core/views.py:396`; `app/pautas/services.py:423`; `app/colaboradores/views.py:2874`; `app/colaboradores/services/calendario.py:411`. |

Há muitos outros leitores diagnósticos/vigias/comandos além desses; tabela acima **não é censo exaustivo de toda consulta a `CelulaDia`**. Exemplos comprovados: `app/ponto/services/vigia_entrada.py:78`, `app/ponto/services/vigia_ausencia_veredito.py:42`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/escala/services/cadastro_realidade.py:166`, `app/chamados/services/foto_e4b.py:269`.

Anexo real existente: `app/docs/JUIZES.md:101-134`. Define feriado em :101-102; dia previsto “célula; senão aritmética do VÍNCULO” em :112-113; vínculo do dia em :130-131; cobertura em :132-134. Registro vivo confirma dia previsto/precedência em `app/core/juizes.py:186-192`; porta de escrita da célula em `app/core/portas.py:107-112`.

## 15/11 19:56 · sessao c8cda0f10a4a · DISPUTA-ZUMBI-1110 · sonda · origem=Code

Transcrição do dado fornecido; conferência independente em produção: [nao verificado].

```text
UNIVERSO abertas=468 pode_fechar_True=0 total_erros=0 HOJE=2026-11-15
abertas                                 468
pode_fechar_False                       468
total_erros=0
```

**Sim, `total_erros=0`.** Portanto, pelo dado fornecido, a medida conclui **zero disputas abertas com C7=True**, entre 468 abertas, em 2026-11-15. [nao verificado]

Na cópia, C7 retorna True quando não há respostas sem veredito nem fio vivo. (`app/chamados/models.py:1911–1917`) A identidade dessa cópia com o HEAD/hash informado é [nao verificado]: o diretório não contém metadados Git.

**Não há PF_TRUE para classificar individualmente no dado fornecido.** Assim, não há caso que exija apontar campo faltante ou caminho de fechamento omitido. [nao verificado]

| Classe conferida no código da cópia | PF_TRUE no dado fornecido |
|---|---:|
| (i) Vazia, com abstenção por ata: o emissor consulta C7 e preserva aberta quando `_absteve_por_ata`. (`app/chamados/services/disputa_emissao.py:1292–1301`) | 0 [nao verificado] |
| (ii) Pai vivo fora do conjunto encerrável: após C7=True, separa em `disputas_com_pai_vivo_fora`. (`app/chamados/services/competencia_trancada.py:173–183`) | 0 [nao verificado] |
| (iii) Todas as perguntas em dias trancados: a tranca elimina a vivacidade e exclui essas respostas da lista sem veredito. (`app/chamados/juizes.py:1406–1409`; `app/chamados/models.py:1903–1909`) | 0 [nao verificado] |
| (iv) Beco FECHAVEL: o classificador consulta C7; a entrada do evento encaminha o FECHAVEL ao fechamento. (`app/chamados/services/becos.py:25–33`, `66–78`) | 0 [nao verificado] |
| (v) Outro: nenhum PF_TRUE fornecido para identificar caminho omitido. [nao verificado] | 0 [nao verificado] |

**Total distribuído: 0 PF_TRUE.** [nao verificado]

## 15/11 19:56 · sessao 57ad847f7f6f · - · - · origem=Ronald (tmux)

Q3 — a condição é possível sob uma referência específica, mas o literal deixa ambiguidades que precisam ser explicitadas:

- Comparar **antes do lote** com **cura + lote** pode produzir 31/31 iguais: o movimento declarado é produzido pela alteração do flag no fallback, e a candidata (a) pretende neutralizar essa dependência. Comparar **impacto anterior depois do lote** com **impacto curado** exige que col940 mude −6,44→−4,24; nesse referencial, “31/31 iguais” contradiz a estabilização pretendida. A mudança anterior está registrada em `app/docs/RELATO.md:4`; a escolha exata dos lados da nova comparação é **[nao verificado]**.
- O histórico declara 16 vínculos em 13/11 (`app/docs/RELATO.md:738`; `app/docs/BACKLOG.md:729`), enquanto o impacto atual declara 15 vínculos/31 colab-competência (`app/docs/RELATO.md:4`). A identidade do excluído, o motivo e o universo congelado desde 13/11 são **[nao verificado]**.
- “Qualquer campo gravado” alcança um efeito adicional da porta: `marcar_trabalha_em_feriado` salva o vínculo (`app/colaboradores/services/vinculo.py:894`); o signal invalida o previsto (`app/escala/signals.py:137`, `app/escala/signals.py:146`); `invalidar_previsto` zera `previsto_em` e altera `atualizado_em` de todos os fechamentos daquele colaborador que tenham `previsto_em`, sem filtro de competência/exportação (`app/ponto/services/fechamento.py:1607`). Existir fechamento entregue nessas condições é **[nao verificado]**. O próprio backlog registra esse efeito como fila (`app/docs/BACKLOG.md:730`).
- O retorno padrão da sombra cobre apenas 26 campos e omite `motivos_espelho`, `causa_espelho`, `previsto_em` e `atualizado_em`; não basta para provar literalmente “qualquer campo gravado” (`app/ponto/services/fechamento.py:454`, `app/ponto/services/fechamento.py:1128`). Um confronto indiscriminado de carimbos recalculados também inclui valores de relógio: `previsto_em=timezone.now()` (`app/ponto/services/fechamento.py:1086`).

O teste oposto já existe: [nao verificado]

- Teste direto: vínculo com início 01/01, feriado de 21/04, flag False → `eh_dia_trabalho=False`; True → trabalho (`app/escala/tests/test_escala_models.py:255`, `app/escala/tests/test_escala_models.py:268`, `app/escala/tests/test_escala_models.py:275`).
- Vigência delimitada explicitamente: vínculo 01/01–13/12 (`app/ponto/tests/test_precedencia_vinculo_encerrado.py:43`), feriado em 10/12 (`app/ponto/tests/test_precedencia_vinculo_encerrado.py:36`), flag False suprime previsto (`app/ponto/tests/test_precedencia_vinculo_encerrado.py:68`). Resultado de execução nesta revisão: **[nao verificado]**.

Q4 — não é possível afirmar “nenhuma outra exportada muda” com os dados fornecidos: a população da frota e o impacto depois da cura são **[nao verificado]**. O caminho monetário é comprovável sem editar o motor: o banco pergunta ao vínculo por todos os sete dias da semana, inclusive anteriores ao início da fatia (`app/ponto/motor_calculo_v2.py:3436`, `app/ponto/motor_calculo_v2.py:3449`), usa a quantidade como denominador (`app/ponto/motor_calculo_v2.py:3451`) e grava o saldo resultante (`app/ponto/services/fechamento.py:1104`, `app/ponto/services/fechamento.py:1180`). Portanto, alterar `escala/models.py` pode alterar comportamento monetário do motor sem alterar o arquivo do motor (`app/escala/models.py:1096`, `app/escala/models.py:1369`).

Há também leitores de rubrica: fechamento separa folga de feriado por `feriado_vira_folga` e promove HE100 (`app/ponto/services/fechamento.py:949`, `app/ponto/services/fechamento.py:955`); calculador alimenta a mesma pergunta (`app/ponto/calculador/alimentacao.py:521`) e escolhe a rubrica (`app/ponto/calculador/regras.py:684`). O impacto anterior declara zero mudança dessa rubrica nos 31, sem demonstrar a frota inteira (`app/docs/BACKLOG.md:801`).

O candidato (a), isoladamente, pode ser implementado sem escritor de dados e sem editar `motor_calculo_v2.py`; a implementação concreta ainda é **[nao verificado]**. O lote existente, porém, escreve flag e células pelas portas (`app/colaboradores/services/vinculo.py:894`, `app/colaboradores/services/vinculo.py:904`, `app/colaboradores/services/vinculo.py:910`), além da invalidação de fechamento acima.

## 15/11 19:56 · sessao 7ffdf687cbb2 · FERIADO-USA-A-LEI-DE-1609 · sonda · origem=Code

1. Transcrição do dado raspado; execução não conferida em banco/log. [não verificado]

```text
UNIVERSO HOJE=2026-11-15 colab_comp_examinados=3809 dia_colab_feriado_examinados=7662 competencias=7+14 (ENTREGUE+ABERTA) candidatos_A=118 dia-colab em 87 colab-comp | POPULACAO ENTREGUE_FORA=1 ENTREGUE_LOTE=1 ABERTA_FORA=6 ABERTA_LOTE=0 | total_erros=0
A_colab_comp_candidatos               87
A_dia_colab_candidatos               118
B_fechamento_lido_chamadas            87
B_sem_resposta                        12
competencias_ABERTA                   14
competencias_ENTREGUE                  7
lote_vinculos                         15
total_erros=0
```

**Sim: o dado informa `total_erros=0`.** Não apresenta traceback; o significado das 12 ocorrências de `B_sem_resposta` depende do script ausente. [não verificado]

2. ACERTOS transcritos. FORA = flag gravada; LOTE = flag da lei em memória, conforme definição fornecida. [não verificado]

| Classe | Modo | Colaborador | Competência | Dia | EC | Leitor | Fonte |
|---|---|---|---|---|---|---|---|
| ENTREGUE | LOTE | col940 | 09/2026 | 2026-09-07 | 1208 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ENTREGUE | FORA | col945 | 09/2026 | 2026-09-07 | 1255 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ABERTA | FORA | col985 | 10/2026 | 2026-10-12 | 1418 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ABERTA | FORA | col987 | 10/2026 | 2026-10-12 | 1421 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ABERTA | FORA | col990 | 10/2026 | 2026-10-12 | 1424 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ABERTA | FORA | col1018 | 11/2026 | 2026-11-02 | 1517 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ABERTA | FORA | col1023 | 11/2026 | 2026-11-02 | 1522 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ABERTA | FORA | col1024 | 11/2026 | 2026-11-02 | 1523 | ponto/motor_calculo_v2.py:3449 | [não verificado] |

3. O único leitor citado corresponde a **`MotorComercial.calcular_mes`**, na apuração semanal do banco de horas. A classe e o método estão em `app/ponto/motor_calculo_v2.py:3199` e `app/ponto/motor_calculo_v2.py:3308`; a consulta ocorre em `app/ponto/motor_calculo_v2.py:3449`.

**É caminho de motor e fechamento:** `fechamento_lido` chama `recalcular_fechamento_mes(..., somente_leitura=True)`, que passa por `apurar_por_fatia` e chama `motor.calcular_mes`. (`app/ponto/services/fechamento.py:1759`, `app/ponto/services/fechamento.py:754`, `app/ponto/services/fechamento.py:286`)

Sem `CelulaDia`, `eh_dia_trabalho` chama o cálculo aritmético; uma base `True` vira `False` quando `feriado_vira_folga` aceita flag, ciclo e feriado. (`app/escala/models.py:1094`, `app/escala/models.py:1096`, `app/escala/models.py:1369`, `app/escala/models.py:1390`, `app/escala/models.py:1392`, `app/escala/models.py:1395`)

Li os arquivos disponíveis da cópia; sem metadados Git, a correspondência com o HEAD solicitado não pôde ser certificada. [não verificado]

4. **Não foi possível auditar o pré-filtro:** `logs/FERIADO-LEI-1609/g_frota/sonda_populacao.py` não está disponível nesta cópia. Portanto, A ser superconjunto de B permanece **[não verificado]**.

Há uma condição concreta para confrontar com A: o leitor recua até segunda-feira, monta semanas completas e consulta os sete dias; só depois restringe a contagem ao período. Assim, consulta também dias anteriores a `data_ini` e posteriores a `data_fim`. (`app/ponto/motor_calculo_v2.py:3436`, `app/ponto/motor_calculo_v2.py:3440`, `app/ponto/motor_calculo_v2.py:3441`, `app/ponto/motor_calculo_v2.py:3449`, `app/ponto/motor_calculo_v2.py:3450`)

Se A limitar os candidatos às datas da competência/fatia, descartará feriados dessas bordas que B consulta. A existência desse corte em A é **[não verificado]**; sem o script, isso é um contraexemplo condicional, não um defeito confirmado.

5. **ENTREGUE FORA = 1** (`col945`, 09/2026); **ENTREGUE LOTE = 1** (`col940`, 09/2026), conforme o dado raspado. [não verificado]

## 15/11 19:56 · sessao aae31e3519ff · - · - · origem=Ronald (tmux)

Q2: **não identifiquei colisão do desenho (a)**, desde que se limite a “SEM célula e FORA da vigência” e preserve o resultado dentro dela. A célula é soberana e o fallback é aritmético/template — `CLAUDE.md:148-157`, `app/docs/LEIS.md:71`. Sem célula, `vinculo_do_dia` declara usar “a aritmética do vínculo que cobre a data” — `app/escala/alimentacao.py:244-247`; cobertura exige `data_inicio <= data`, fim inclusivo e vigência declarada — `app/escala/alimentacao.py:205-216`. E1 diz literalmente: “a geradora da celula so vale DENTRO da vigencia (...) fora dela a juiza devolve None (...) Vigencia, nao o flag ativo” — `app/escala/alimentacao.py:272-283`. As autoridades únicas estão registradas em `app/docs/CORTES.md:335` e `app/docs/CORTES.md:338`.

A lei de **16/09 sobre o padrão de feriado** trata do nascimento de vínculo NOVO: “O posto que DECLAROU (...) vence”; sem declaração, comerciais 5x2/6x1 folgam, demais trabalham; “Vinculo que ja existe nao passa por aqui — quem muda esses e o DP” — `app/escala/models.py:900-917`. Ela não autoriza ler o flag do vínculo em qualquer data, nem manda regravar vínculos existentes — `app/escala/models.py:906-910`.

**Item 4: PROCEDE como registro de pergunta, sem implementação ou decisão.** L-117 manda levar a pergunta ao topo com números e seguir a fila, sem propor lei nova — `app/docs/LEIS.md:138`, `CLAUDE.md:269-273`. E1 determina quem vale na data; o código do banco semanal, entretanto, consulta o mesmo `_ec` pelos sete dias da semana e usa os dias previstos no denominador — `app/ponto/motor_calculo_v2.py:3433-3453`. Portanto não concluir que o corte (a) resolve a pergunta (b) inteira.

Q4, cautela relevante: uma mudança em `EscalaColaborador.eh_dia_trabalho` pode alterar **indiretamente** o banco do motor sem editar `motor_calculo_v2.py`, porque ele chama esse método em `app/ponto/motor_calculo_v2.py:3449`, calcula a proporção em `:3450-3452` e grava o resultado em `:3455`. Quantos colaboradores/competências mudariam é **[nao verificado]** sem a sonda de frota.

Os limites escritos continuam: mexer no GRAVADO exportado, dado de ESCALA/VÍNCULO ou `motor_calculo_v2.py` exige `!` — `CLAUDE.md:265-268`, `CLAUDE.md:142`; dinheiro não exportado requer reversão prévia, exportada intacta e prova de frota — `CLAUDE.md:406-410`; exportado exige critério e porta própria — `CLAUDE.md:411-417`. **Não assumir proteção automática pela recusa do recalculador:** L-113 documenta que a recusa genérica de exportada caiu, substituída por trilha — `app/docs/LEIS.md:154`.

## 15/11 19:55 · sessao 58dc16a71f83 · - · - · origem=Ronald (tmux)

O único leitor citado, `ponto/motor_calculo_v2.py:3449`, corresponde a `MotorComercial.calcular_mes`: classe em `app/ponto/motor_calculo_v2.py:3199`, método em `app/ponto/motor_calculo_v2.py:3308`.

- **É caminho de motor e fechamento.** `fechamento_lido` chama `recalcular_fechamento_mes` com `somente_leitura=True`; este chama `apurar_por_fatia`, que executa `motor.calcular_mes`. (`app/ponto/services/fechamento.py:1759`, `app/ponto/services/fechamento.py:754`, `app/ponto/services/fechamento.py:286`)
- **O leitor consulta semanas completas:** recua o cursor até segunda, constrói os sete dias e chama `eh_dia_trabalho` em todos; somente depois conta os dias dentro do período. Pode consultar dias anteriores a `data_ini` e posteriores a `data_fim`. (`app/ponto/motor_calculo_v2.py:3436`, `app/ponto/motor_calculo_v2.py:3441`, `app/ponto/motor_calculo_v2.py:3449`, `app/ponto/motor_calculo_v2.py:3450`)
- **O vínculo consultado é o da fatia:** o motor recebe `escala_colaborador=esc`, armazena em `self._ec`, recuperado como `_vinc_bh`. (`app/ponto/services/fechamento.py:289`, `app/ponto/motor_calculo_v2.py:2309`, `app/ponto/motor_calculo_v2.py:3433`)
- **Sem CelulaDia**, `eh_dia_trabalho` usa o cálculo aritmético; quando `base is True`, `feriado_vira_folga` pode devolver folga. O predicado exige flag falsa, ciclo contemplado e feriado no lugar do vínculo; não testa vigência. (`app/escala/models.py:1087`, `app/escala/models.py:1096`, `app/escala/models.py:1369`, `app/escala/models.py:1389`)
- **Ponto para confrontar com Etapa A:** qualquer pré-filtro limitado à janela da competência/fatia descartaria feriado na borda da semana que o leitor real consulta; isso é um contraexemplo condicional, cuja presença no script depende de sua leitura. (`app/ponto/motor_calculo_v2.py:3436`, `app/ponto/motor_calculo_v2.py:3440`, `app/ponto/motor_calculo_v2.py:3449`)

## 15/11 19:52 · sessao fa73c9ea025e · DISPUTA-ZUMBI-1110 · censo · origem=Code

**1. Existe contador hoje? Sim, em recortes; não localizei um contador global exato de “aberta + C7=True” nos caminhos lidos.** O estado aberto é `fechada_em is None`; C7 consulta perguntas, respostas sem veredito e fio vivo. (`app/chamados/models.py:1871`, `1911–1917`)

| Caminho | O que efetivamente cobre |
|---|---|
| `becos` / `fechar_becos_disputa --alarme` | **Conta o estado pedido dentro dos becos:** disputa aberta, pai terminal e nenhuma pergunta considerada visível pelo filtro local. Só depois pergunta C7; imprime fecháveis/barradas e sai com rc 2 se houver fechável. Não há janela de datas. (`app/chamados/services/becos.py:25–33`; `app/chamados/management/commands/fechar_becos_disputa.py:32–44`, `67–70`) |
| `vigia_carimbo` | Conta **perguntas de hora/data_hora validadas**, com ou sem registro, independentemente de disputa aberta/fechada. Não pergunta C7. (`app/chamados/services/vigia_carimbo.py:95–132`) |
| `desenlace` | Apresentação por chamado: recebe uma explicação com `pode_fechar` e produz estado/ação. Não varre nem conta disputas. (`app/chamados/services/desenlace.py:44–67`) |
| `reconciliador` | Pergunta C7 para fechar disputas atingidas pelos atos de reconciliação; não acumula o censo global pedido. O vigia `reconciliar_chamados` conta premissas mortas, perguntas absolvidas, rejunção e outbox. (`app/chamados/reconciliador.py:412–414`, `609–615`, `1158–1173`; `app/chamados/management/commands/reconciliar_chamados.py:89–124`) |
| `crons_quebrados` | Conta últimas execuções com saída diferente de 0 e 2; seu universo são corridas de cron nas últimas 26 horas, não disputas. (`app/core/services/crons_quebrados.py:16–17`, `24–47`) |
| `config/crons.py` | Agenda beco às **07:10**, reconciliador a cada **15 minutos** e carimbo como elo após fio morto. Essas entradas não acrescentam um censo global de disputas. (`app/config/crons.py:663`, `395`, `1774–1776`) |

**Outros contadores encontrados:**

- **Relatório de pendências:** percorre todas as abertas e usa C7 para `PRONTA_FECHAR`, mas separa as sem perguntas em `SEM_PERGUNTA` **antes** de chamar C7. Portanto, `PRONTA_FECHAR` não equivale ao total global pedido. É sob demanda, sem saída de alarme. (`app/chamados/relatorios_disputa.py:11–28`, `45–58`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:14–40`; `app/config/crons.py:1463`)
- **Competência trancada:** conta abertas com C7=True ligadas às perguntas daquela competência, **excluindo pai vivo fora do conjunto que será encerrado**. O comando mostra disputas e uma soma de chamados+perguntas+disputas; `placar_code.sh` recolhe essa soma. Não tem cron próprio nem rc 2 nesse comando. (`app/chamados/services/competencia_trancada.py:119–126`, `173–183`, `346–355`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:12–30`; `bin/placar_code.sh:285–289`; `app/config/crons.py:1295–1297`)
- **Vigia do fio:** `disputa_vazia()` conta o motivo de fechamento `vazia`; `viva_em_disputa_fechada()` conta perguntas vivas em disputas **fechadas**. Nenhum corresponde ao estado solicitado. (`app/chamados/services/vigia_fio.py:748–780`)

Quantidade atual em produção, execuções efetivamente instaladas e ocorrência concreta desses estados: **[nao verificado]**.

**2. Opções de sítio, preservando C7 e sem alterar `config/crons.py`.**

Considere **N** disputas abertas, **B** abertas que passam pelo filtro de beco e **E** abertas sem perguntas; são universos derivados dos filtros existentes. (`app/chamados/management/commands/fechar_becos_disputa.py:32–43`; `app/chamados/services/becos.py:27–33`; `app/chamados/relatorios_disputa.py:12–14`)

| Opção técnica | Universo e custo de C7 |
|---|---|
| **(a) Estender `fechar_becos_disputa --alarme`** | Contador geral sobre N abertas: **N chamadas**, uma por disputa. Hoje são B chamadas. Acrescentar uma passagem global mantendo a classificação atual sem reaproveitamento custaria **N+B**. Já existe execução diária com `--alarme`; a aplicação pode continuar restrita aos becos. (`app/chamados/services/becos.py:25–33`; `app/chamados/management/commands/fechar_becos_disputa.py:30–57`; `app/config/crons.py:663`) |
| **(b) Acrescentar a outro vigia existente** | No **carimbo**, seriam N chamadas novas, além da classificação atual das perguntas validadas; é necessário carregar as abertas separadamente. No **reconciliador**, seriam N chamadas novas **a cada 15 minutos**, além dos censos atuais. (`app/chamados/services/vigia_carimbo.py:97–116`; `app/chamados/management/commands/reconciliar_chamados.py:57–100`; `app/config/crons.py:395`, `1774–1776`) |
| **(c) Contador sob demanda / relatório** | Um contador próprio sobre abertas custaria N chamadas. Reaproveitar o relatório atual exige incluir as E vazias: hoje ele chama C7 apenas N−E vezes. O relatório não possui `--alarme` nem agendamento. (`app/chamados/relatorios_disputa.py:12–25`, `45–52`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:11–40`; `app/config/crons.py:1463`) |

**Custo de uma chamada:** C7 materializa a lista inteira de perguntas; avalia respostas sem veredito e, se essa lista estiver vazia, executa `fio_vivo`, que interrompe na primeira pergunta viva. Sem `perguntas=` ou prefetch, há carregamento da relação. (`app/chamados/models.py:1889–1917`; `app/chamados/juizes.py:1507–1516`)

As consultas transitivas variam conforme o caso: tranca da competência e toque administrativo; célula/veredito/ata/lâmpadas; classificação de falta; provas de materialização, incluindo batidas e trilha. Logo **não há número fixo de SQL por disputa**; prefetch e memos alteram o custo. (`app/chamados/juizes.py:977–995`, `1013–1024`, `1402–1488`, `755–800`; `app/chamados/models.py:1905–1909`) Quantidade de queries e duração atuais em produção: **[nao verificado]**.

**Como o rc chega ao placar:**

1. O comando levanta `SystemExit(2)`; o envelope captura a saída, grava `exit` no JSONL e devolve o mesmo rc. Rc 2 também deixa carimbo de conclusão. (`app/chamados/management/commands/fechar_becos_disputa.py:67–70`; `bin/cron_run.sh:11–25`, `38`)
2. `crons_quebrados.EXITS_DE_ALARME=(0,2)` exclui rc 2 dos quebrados; o envelope também não abre pauta de quebra por esse rc. (`app/core/services/crons_quebrados.py:16`, `46–47`; `bin/cron_run.sh:27–35`)
3. `pipeline_placar` acumula rc 2 em `alarmes` e pode mostrar a linha como `ALARME`. **Esse estado não entra na lista que determina o rc do próprio pipeline**: entram `ERRO`, `NAO_RODOU` e `DEPENDENCIA_QUEBRADA`. Assim, pode haver linha `ALARME` junto de `A5 OK`. (`app/core/management/commands/pipeline_placar.py:74–80`, `118–137`, `151–163`)
4. Um comando novo, sem cadastro e executado pelo envelope, aparece como `ORFAO`; isso sozinho não o inclui nos alarmes A5. (`app/core/management/commands/pipeline_placar.py:134–137`, `156–163`)

A opção **(a)** já dispõe de universo inicial, agenda e transporte de rc; porém sua mensagem atual atribui o achado ao **evento de beco**, significado que precisaria distinguir do contador geral. (`app/chamados/management/commands/fechar_becos_disputa.py:32–33`, `68–70`; `app/config/crons.py:663`)

**3. Quem escreve, quem lê e onde a semântica diverge.**

**Escritores de `fechada_em`:**

- `DisputaSupervisao.fechar()` grava instante, autor, resolução e motivo. (`app/chamados/models.py:1996–2000`)
- `reabrir_admin()` e `reabrir_sistema()` limpam esses campos. (`app/chamados/models.py:2213–2225`, `2237–2241`)
- `pode_fechar()` é leitura: produz bool; não consulta o próprio `fechada_em`, o pai nem flags de operação em curso. (`app/chamados/models.py:1911–1917`)

**Chamadores diretos de C7 identificados no código:**

| Finalidade | Leitores |
|---|---|
| Classificação/apresentação | Relatório e explicador. (`app/chamados/relatorios_disputa.py:24`; `app/chamados/services/explicador.py:139`) |
| Beco e tranca | Classificar/fechar beco; medir/aplicar encerramento da competência. (`app/chamados/services/becos.py:33`, `56`; `app/chamados/services/competencia_trancada.py:178`, `227`) |
| Emissão/materialização | Autoencerramento, resolução por chamado e conclusão da materialização. (`app/chamados/services/disputa_emissao.py:1292`, `2112`; `app/chamados/services/materializacao.py:591`, `1322`) |
| Reconciliação | Fechamento das disputas alcançadas pelos respectivos atos. (`app/chamados/reconciliador.py:412`, `612`, `1171`) |
| Entrada/foto | Sincronização de entrada e upload de foto. (`app/ponto/services/entrada_sincronizada.py:59`; `app/api/foto_recebida.py:83`) |

**Leitores de `fechada_em` com efeitos ou apresentação relevantes:**

- Signals pedem fechamento do pai, retomam regularizações e materializam pendentes aprovadas. (`app/chamados/signals.py:29–46`, `131–146`, `155–165`)
- Regularização externa bloqueia qualquer disputa aberta; o helper público também procura qualquer aberta. (`app/chamados/services/regularizacao_ext.py:59–66`; `app/chamados/services/disputa_emissao.py:34–42`)
- App particiona pendência/histórico; API seleciona abertas; tela conta abertas. (`app/chamados/services/pergunta_no_app.py:76–85`; `app/api/views.py:1016–1035`; `app/chamados/views.py:909–912`)
- Situação e painel identificam colaborador com disputa aberta. (`app/colaboradores/services/situacional.py:117`; `app/colaboradores/painel_cell.py:31–32`)

**ESMERIL-SEMANTICA — diferenças concretas:**

- C7=True **não significa “tudo validado e gravado”**: aceita disputa vazia e perguntas de dias trancados; `desenlace` usa exatamente aquele texto para `pode_fechar=True`. (`app/chamados/models.py:1903–1917`; `app/chamados/services/desenlace.py:62–65`)
- O relatório rotula `PRONTA_FECHAR` como **“FALTA MATERIALIZAR”**, embora a classificação venha de C7. (`app/chamados/relatorios_disputa.py:24–25`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:29`)
- Disputa vazia é fechável pelo C7, mas bloqueia o encerramento do pai no leitor `disputa_viva_do_chamado`. São respostas diferentes em domínios diferentes. (`app/chamados/models.py:1916–1917`; `app/chamados/juizes.py:2440–2446`; `app/chamados/models.py:153–160`)
- Há leitor legado que aceita **qualquer disputa fechada do colaborador** como prova de regularização de chamado de furo sem dia. (`app/chamados/models.py:187–190`)
- Rc 2 significa alarme de negócio, não cron quebrado; sua não propagação ao agregado A5 está inclusive fixada por teste. (`app/core/services/crons_quebrados.py:16`, `46`; `app/chamados/tests/test_contract_crons.py:186–208`)

**4. Testes existentes de `fechar_becos_disputa` e impacto.** [nao verificado]

| Teste | Contrato fixado | [nao verificado]
|---|---|
| `BecoPeloEventoDaPergunta.test_o_cron_virou_VIGIA_alarme_nao_escreve_e_sai_2` | `--alarme --apply` dá rc **2** e mantém a disputa aberta. (`app/chamados/tests/test_varredura_becos_evento_pergunta.py:97–109`) |
| `FecharBecosCommandTest.test_command_fecha_beco_pai_resolvido` | Apesar do nome, exige **C7=False e disputa aberta**, pois a pergunta continua viva. (`app/chamados/tests/test_fio_disputa_resolvido.py:149–156`) |
| `test_command_nao_fecha_pai_aberto` | `--apply` preserva disputa com pai aberto. (`app/chamados/tests/test_fio_disputa_resolvido.py:158–162`) |
| `test_command_dry_run_nao_fecha` | Execução sem `--apply` preserva aberta. (`app/chamados/tests/test_fio_disputa_resolvido.py:164–168`) |
| `test_idempotente_e_uma_linha_de_trilha` | Rodar `--apply` após o evento conserva uma trilha de fechamento. (`app/chamados/tests/test_varredura_becos_evento_pergunta.py:83–95`) |
| `test_o_cron_e_o_receiver_chamam_a_MESMA_porta` | Command delega a `fechar_beco`, sem incorporar o corpo de resolução. (`app/chamados/tests/test_varredura_becos_evento_pergunta.py:111–117`) |

Nos testes encontrados, stdout/stderr são capturados sem comparação textual; o assert explícito de rc é o primeiro acima. (`app/chamados/tests/test_fio_disputa_resolvido.py:152`, `160`, `166`; `app/chamados/tests/test_varredura_becos_evento_pergunta.py:106–109`)

**Se apenas o alarme ganhar o universo geral, preservando `--apply`, nenhum desses asserts precisa necessariamente mudar.** Eles não fixam rc normal nem saída para aberta+C7=True fora do beco; o novo contrato precisaria desses casos. (`app/chamados/tests/test_varredura_becos_evento_pergunta.py:97–117`; `app/chamados/tests/test_fio_disputa_resolvido.py:149–168`) Resultado de execução dos testes: **[nao verificado]**.

**5. Contraexemplos e resposta de C7.**

| Caso | Resposta de C7 e evidência |
|---|---|
| **Sem ata, aguardando informação** | **True para disputa vazia**, mas o emissor deliberadamente não fecha quando `_absteve_por_ata`. O teste fixa zero perguntas, disputa aberta e aviso `auto_fecho_disputa`. É um contraexemplo concreto à inferência “C7=True implica fechamento esquecido”. (`app/chamados/models.py:1916–1917`; `app/chamados/juizes.py:1516`; `app/chamados/services/disputa_emissao.py:1292–1301`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51–61`) |
| **Pai vivo fora da competência encerrável** | **True**, explicitamente: o código passa por C7 e depois conserva a disputa em `disputas_com_pai_vivo_fora`, para evitar encerrar o pai por tabela. (`app/chamados/services/competencia_trancada.py:24–28`, `173–183`) |
| **Todas as perguntas em dias trancados** | **True**: C6 as considera mortas e a lista sem veredito exclui respostas trancadas. Há testes explícitos, inclusive com resposta do colaborador. Tranca isoladamente não prova que manter aberta seja o destino esperado: também há teste exigindo fechamento pela porta da tranca. (`app/chamados/juizes.py:1406–1409`; `app/chamados/models.py:1903–1909`; `app/chamados/tests/test_c7_red_minimos.py:361–382`, `329–350`) |
| **Materialização em curso** | C7 **não testa “em voo”**; o S143 pula esse fechamento por guarda anterior. Portanto pode continuar True conforme as perguntas. A flag é ContextVar dentro de transação; observação desse intervalo por um cron externo: **[nao verificado]**. (`app/chamados/models.py:1911–1917`; `app/chamados/services/disputa_emissao.py:2110–2113`; `app/chamados/services/materializacao.py:1137–1161`) |
| **Pai legitimamente aguardando conversa humana** | Isso não determina sozinho C7 da disputa: o gate do pai admite que a disputa feche enquanto ele permanece humano. Se a própria disputa possui resposta sem veredito, não materializada e fora da tranca, C7 responde **False**. (`app/chamados/models.py:699–712`, `1905–1917`) |

**Os casos acima demonstram que “aberta+C7=True” não equivale automaticamente a “o evento de beco falhou”.** No contador literal solicitado, esses estados entram; uma regra que os dispense de alarmar não está estabelecida pelo material fornecido: **[nao verificado]**. (`app/chamados/services/becos.py:25–33`; `app/chamados/services/disputa_emissao.py:1292–1295`; `app/chamados/services/competencia_trancada.py:178–183`)

## 15/11 19:50 · sessao 4317c1388896 · - · - · origem=Ronald (tmux)

Inventário de leitura/escrita consolidado:

- `fechada_em`: `fechar()` grava o instante em `app/chamados/models.py:1996` e salva junto com autor, resolução e motivo em `:2000`; `reabrir_admin()` limpa em `:2213` e salva em `:2225`; `reabrir_sistema()` limpa em `:2237` e salva em `:2241`. Desligamento chama a porta, sem `update` paralelo: `app/colaboradores/services/desligamento.py:164-167`.
- `pode_fechar()` calcula somente ausência de resposta sem veredito e ausência de fio vivo; não verifica aberta/fechada, pai ou operação em curso: `app/chamados/models.py:1911-1917`.
- Chamadores C7: `app/chamados/relatorios_disputa.py:24`; `app/api/foto_recebida.py:83`; `app/chamados/services/becos.py:33,56`; `app/chamados/services/competencia_trancada.py:178,227`; `app/chamados/services/explicador.py:139`; `app/chamados/services/disputa_emissao.py:1292,2112`; `app/chamados/services/materializacao.py:591,1322`; `app/chamados/reconciliador.py:412,612,1171`; `app/ponto/services/entrada_sincronizada.py:59`.
- Leitores estruturais de `fechada_em`: signals fecham pai pelo gate (`app/chamados/signals.py:29-46`), retomam regularizações após commit (`:131-146`) e materializam pendentes aprovadas (`:155-165`); regularização externa bloqueia qualquer aberta (`app/chamados/services/regularizacao_ext.py:59-66`); app usa aberta para pendência/histórico (`app/chamados/services/pergunta_no_app.py:81-85`; `app/api/views.py:1016-1035`); tela conta todas abertas (`app/chamados/views.py:909-912`).
- Fora chamados: mapa de situação (`app/colaboradores/services/situacional.py:117`), célula de painel (`app/colaboradores/painel_cell.py:31-32`), banner (`app/colaboradores/services/drawer.py:69-78`). Helper central consulta qualquer aberta em `app/chamados/services/disputa_emissao.py:34-42`; consumidores indiretos: `app/ponto/services/disputa.py:31`, `app/ponto/views.py:143-144`, `app/api/views.py:1335-1336`, `app/colaboradores/mixins.py:304-307`, `app/relatorios/pdf_espelho.py:1082-1085`. Comportamento indireto da folha: [nao verificado].

Semânticas divergentes:

- Disputa vazia tem C7=True (`app/chamados/models.py:1916-1917`; `app/chamados/juizes.py:1516`), mas relatório classifica antes como `SEM_PERGUNTA` (`app/chamados/relatorios_disputa.py:13-14`) e juiz do bloqueio do pai considera vazia bloqueadora (`app/chamados/juizes.py:2440-2446`; consumidor `app/chamados/models.py:153-160`).
- Leitor legado de chamado de furo sem dia considera existência de qualquer disputa fechada do colaborador prova de regularização: `app/chamados/models.py:187-190`.
- `EXITS_DE_ALARME=(0,2)` exclui rc 2 de cron quebrado (`app/core/services/crons_quebrados.py:16,46-47`); pipeline distingue rc 2 como alarme (`app/core/management/commands/pipeline_placar.py:76-80`), mostra `ALARME` (`:124-125`), porém não o inclui no agregado A5 (`:131-132`), que determina seu próprio rc (`:158-163`).

Contraexemplos de aberta+C7=True:

- **Sem ata:** escritor deliberadamente preserva aberta mesmo C7=True quando `_absteve_por_ata` (`app/chamados/services/disputa_emissao.py:1292-1295`); teste `DisputaMudaSemAtaTest.test_sem_ata_nao_fecha_e_acende` fixa perguntas=0 e aberta (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51-61`). É contraexemplo concreto de teste/código, não ocorrência demonstrada em produção.
- **Pai vivo fora da tranca:** medidor só depois de C7=True preserva disputa cujo pai vivo não será encerrado (`app/chamados/services/competencia_trancada.py:173-183`); propósito declarado em `:24-28`.
- **Materialização em voo:** fecho S143 pula disputa com pergunta em voo antes de C7 (`app/chamados/services/disputa_emissao.py:2110-2113`); sinal é ContextVar dentro de atomic (`app/chamados/services/materializacao.py:1137-1161`). C7 não olha esse sinal (`app/chamados/models.py:1911-1917`); observabilidade desse intervalo por cron externo: [nao verificado].
- **Tranca:** C6 retorna False na tranca (`app/chamados/juizes.py:1406-1409`) e lista sem veredito exclui resposta trancada (`app/chamados/models.py:1903-1909`); portanto, se todas as perguntas estão assim, C7=True. Testes fixam isso em `app/chamados/tests/test_c7_red_minimos.py:361-367,377-382`. Tranca sozinha não justifica afirmar C7=False.
- **Conversa legítima do pai:** guarda do pai e guarda da disputa têm domínios distintos; pai pode ficar humano enquanto disputa fecha (`app/chamados/models.py:699-712`). C7=False quando a própria disputa possui resposta não materializada nem validada fora da tranca (`app/chamados/models.py:1905-1917`).

## 15/11 19:50 · sessao 0792a45ace27 · - · - · origem=Ronald (tmux)

Leitura concluída, sem alterações. Evidências para Q1/Q2:

- **Existe contador parcial de aberta+C7=True:** `fechar_becos_disputa` seleciona todas as abertas, mas chama C7 somente após `becos.classificar` exigir pai terminal e nenhuma pergunta visível; imprime candidatos e dá rc2 com `--alarme`. Universo: becos, sem janela temporal. `app/chamados/management/commands/fechar_becos_disputa.py:32–44,67–70`; `app/chamados/services/becos.py:25–33`.
- **Existe outra classificação parcial:** relatório de pendências percorre todas abertas e classifica `PRONTA_FECHAR` pelo C7, exceto as sem perguntas, classificadas antes como `SEM_PERGUNTA`. Command imprime categorias/contagens sem rc2; é sob demanda. `app/chamados/relatorios_disputa.py:11–28,45–58`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:14–40`; `app/config/crons.py:1463`.
- **Existe contador de competência trancada:** `medir` pergunta C7 às abertas vinculadas às perguntas da competência e conta `disputas`; exclui pai vivo fora do conjunto de chamados que encerra. `medir_frota` soma por competências com tranca viva; command imprime contagens e soma chamados+perguntas+disputas, sem rc2. Está no `placar_code`, sem cron próprio. `app/chamados/services/competencia_trancada.py:119–126,173–183,331–356`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:12–30`; `app/config/crons.py:1291–1297`; `bin/placar_code.sh:287–289`.
- **Não encontrei contador exato global de todas abertas+C7=True nos caminhos lidos.** O vigia de carimbo conta perguntas hora/data_hora validadas, independentemente de disputa aberta/fechada; não chama C7. `app/chamados/services/vigia_carimbo.py:95–132`. `desenlace` apresenta estado/ação recebendo explicação, sem varredura nem contagem. `app/chamados/services/desenlace.py:44–67`. Reconciliador chama C7 para fechar disputas atingidas por determinados eventos, não para censo global: `app/chamados/reconciliador.py:411–414,600–615,1158–1173`.

**Sítios possíveis, ainda sem implementação:**

1. **Estender `fechar_becos_disputa --alarme`:** N chamadas C7 para N abertas; hoje são B chamadas, B=becos candidatos à classificação. Se a implementação acrescentar consulta global e conservar `classificar` sem reaproveitamento, seriam N+B. `app/chamados/management/commands/fechar_becos_disputa.py:32–43`; `app/chamados/services/becos.py:25–33`. Já agendado 07:10 com `--alarme`. `app/config/crons.py:663`.
2. **Acrescentar ao vigia de carimbo:** N chamadas C7 novas além do seu universo atual P de perguntas validadas; a alimentação atual não cobre todas disputas abertas. Ele já executa `--alarme`, como elo após fio morto; rc2 hoje significa censo cego, e passivo só entra com outra flag. `app/chamados/services/vigia_carimbo.py:97–132`; `app/chamados/management/commands/vigiar_carimbo_sem_registro.py:38–39,56–78`; `app/config/crons.py:1770–1776`.
3. **Acrescentar a `reconciliar_chamados --alarme`:** N chamadas C7 novas a cada 15 minutos. Universo atual é chamados vivos, perguntas e resíduos do outbox, com exclusões próprias por evento pendente; não representa todas abertas. `app/chamados/management/commands/reconciliar_chamados.py:57–100,118–124`; `app/config/crons.py:395`.
4. **Relatório/contador sob demanda:** reaproveitar relatório exige incluir vazias para equivaler ao predicado exato; hoje são N−E chamadas C7, E=abertas sem perguntas. Não possui caminho próprio de rc2 nem agendamento. `app/chamados/relatorios_disputa.py:11–25`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:11–40`; `app/config/crons.py:1463`.

**Custo comum do C7:** carrega lista de perguntas, avalia respostas sem veredito e, se nenhuma, `fio_vivo` com curto circuito. `app/chamados/models.py:1889–1917`; `app/chamados/juizes.py:1507–1516`. Respostas sem veredito podem consultar tranca por pergunta; C6 consulta tranca, célula/ata/lâmpadas, classificação de falta e testemunha de materialização conforme os ramos percorridos. Contagem exata de SQL depende dos estados e caches; não é constante por disputa. `app/chamados/models.py:1905–1909`; `app/chamados/juizes.py:977–995,1013–1024,1402–1488`. Tempo/quantidade de queries na produção atual: [nao verificado].

**Caminho do rc2 ao placar e semântica:**

- `cron_run.sh` recolhe rc, grava JSONL e devolve rc; rc2 também deixa carimbo de fim e não gera pauta de cron quebrado. `bin/cron_run.sh:11–13,19–25,27–38`.
- `crons_quebrados` usa `EXITS_DE_ALARME=(0,2)` e exclui ambos do contador de quebrados. `app/core/services/crons_quebrados.py:16,39–47`.
- `pipeline_placar.medidos` conta rc2 como `alarmes`, separado de `erros`; `placar` pode mostrar situação `ALARME`. Entretanto só `ERRO`, `NAO_RODOU` e `DEPENDENCIA_QUEBRADA` entram na lista que faz o próprio pipeline sair rc2. Assim uma linha `ALARME` pode coexistir com `A5 OK` e saída normal. `app/core/management/commands/pipeline_placar.py:61–81,118–137,151–163`.
- Diferença adicional: `crons_quebrados` considera última corrida nas 26h; pipeline agrega todas corridas do dia. `app/core/services/crons_quebrados.py:17,39–46`; `app/core/management/commands/pipeline_placar.py:66–81`.
- Mudar o alarme de beco para universo global altera significado específico da mensagem atual “beco(s) que o evento nao fechou”; o transporte do rc2 permanece “alarme de negócio”. `app/chamados/management/commands/fechar_becos_disputa.py:68–70`; `bin/cron_run.sh:19–22`.

**Contraexemplo de código:** competência trancada conserva disputa aberta com C7=True quando seu pai vivo está fora do conjunto encerrável. É exclusão explícita para impedir fechamento do pai por tabela. `app/chamados/services/competencia_trancada.py:24–28,173–183`. Ocorrência concreta na produção: [nao verificado].

## 15/11 19:49 · sessao 15f9f9749b4d · - · - · origem=Ronald (tmux)

Inventário somente leitura, sem execução de testes. [nao verificado]

- **Único teste encontrado que fixa o rc do command:** `BecoPeloEventoDaPergunta.test_o_cron_virou_VIGIA_alarme_nao_escreve_e_sai_2`, em `app/chamados/tests/test_varredura_becos_evento_pergunta.py:97`; cria beco via `update` (:102–104), executa `--alarme --apply` (:106), exige `SystemExit(2)` (:107) e disputa ainda aberta (:109). Esse contrato continua válido ao ampliar o universo do alarme.
- **Não achei teste que compare texto de stdout/stderr desse command.** Os chamadores capturam `StringIO()` sem examinar seu conteúdo: `app/chamados/tests/test_fio_disputa_resolvido.py:152`, `:160`, `:166`; `app/chamados/tests/test_varredura_becos_evento_pergunta.py:93`, `:106`.
- **Outros contratos do command:** `test_command_fecha_beco_pai_resolvido` (:149), apesar do nome, exige `pode_fechar=False` e disputa aberta quando pergunta continua viva (:154–156); `test_command_nao_fecha_pai_aberto` (:158–162); `test_command_dry_run_nao_fecha` (:164–168), todos em `app/chamados/tests/test_fio_disputa_resolvido.py`.
- **Delegação e idempotência:** `test_idempotente_e_uma_linha_de_trilha`, em `app/chamados/tests/test_varredura_becos_evento_pergunta.py:83`, chama `--apply` (:93) e exige uma trilha (:94–95); `test_o_cron_e_o_receiver_chamam_a_MESMA_porta` (:111–117) exige referência a `fechar_beco` e proíbe corpo próprio contendo `resolver_perguntas_por_chamado_resolvido`.
- **Quais mudariam:** se a ampliação afetar somente a contagem de `--alarme` e preservar aplicação/porta, nenhum desses asserts precisa mudar obrigatoriamente; falta acrescentar teste que morda aberta+C7=True fora do beco, zero candidatos/rc normal e saída nova. A inexistência desses asserts decorre do inventário acima; o resultado após alteração é [nao verificado]. [nao verificado]

Casos úteis para pergunta 5:

1. **Pai aberto legítimo, pergunta já validada:** `test_pai_aberto_NAO_fecha`, em `app/chamados/tests/test_varredura_becos_evento_pergunta.py:71–81`, mantém pai aberto, valida a pergunta e exige disputa ainda aberta. O resultado `pode_fechar=True` é inferência do corpo de C7, que só consulta perguntas/respostas, sem pai: `app/chamados/models.py:1911–1917`. O filtro atual recusa esse universo antes de C7: `app/chamados/services/becos.py:29–33`. O teste não afirma que esse caso deva ou não alarmar.
2. **Dia trancado:** fixture `_trancada`, em `app/chamados/tests/test_c7_red_minimos.py:352–359`, cria disputa e pergunta, depois apenas `PeriodoFechado`; `test_RED_o_C6_le_a_tranca_sem_escrever` (:361–367) exige pergunta morta e `pode_fechar=True`; `test_RED_resposta_de_dia_trancado_nao_segura_a_disputa` (:377–382) exige `respostas_sem_veredito=[]` e `pode_fechar=True`, mesmo com resposta. Tranca é causa de C7=True, não exceção interna de alarme: `app/chamados/models.py:1903–1909`, `:1917`.
3. **“Não deveria alarmar” por estar em tranca:** não estabelecido pelo teste acima; pelo contrário, `test_RED_trancada_fecha_pela_porta_com_trilha_e_nao_carimba`, em `app/chamados/tests/test_c7_red_minimos.py:329–350`, exige que `competencia_trancada.encerrar` feche a disputa (:340–344). A política de tolerar a janela entre trancar e encerrar é [nao verificado].
4. **Fechamento em curso:** `_EM_CURSO` protege reentrada do evento, em `app/chamados/services/becos.py:62–80`; C7 não consulta essa estrutura: `app/chamados/models.py:1911–1917`. Logo C7 pode continuar True durante essa fase (inferência); flag global de “em curso” como exclusão do contador ou concorrência real de produção é [nao verificado].

## 15/11 19:42 · sessao 0de49a4c7cf5 · E3-RESTO-R3 · leitura · origem=Code

**(a) 4.2 muda a origem das empresas, a cobertura declarada e o alarme estrutural.** Hoje o vigia descobre empresas pelos retratos existentes; lavrador e frescor começam em `empresas_visiveis()`. Portanto, uma empresa visível sem foto desaparece do vigia antes de suas competências serem examinadas (`app/folha/vigia_retido_calado.py:46`, `app/folha/vigia_retido_calado.py:51`, `app/folha/management/commands/lavrar_retrato_folha.py:51`, `app/folha/retrato_folha.py:1410`).

O universo passa a ser **visíveis ∩ recorte recebido × competências por empresa**. A lista nomeada de faltas já existe; falta preenchê-la também para empresas sem nenhuma foto e consumi-la como estrutural. Hoje `ALARMA_SEMPRE` contém somente `cego`, que não acusa cobertura parcial quando existe algum bloqueado (`app/folha/vigia_retido_calado.py:58`, `app/folha/vigia_retido_calado.py:89`, `app/folha/management/commands/vigiar_retido_calado.py:16`).

**Chamadores e testes atingidos** [nao verificado]

| Classe | Sítios |
|---|---|
| Produção direta | Command importa e chama `censo`: `app/folha/management/commands/vigiar_retido_calado.py:14`, `app/folha/management/commands/vigiar_retido_calado.py:33`. |
| Produção indireta | Cron executa com `--alarme`, dependente do lavrador: `app/config/crons.py:1681`, `app/config/crons.py:1683`. |
| Testes ORM diretos | `app/folha/tests/test_vigia_retido_calado.py:29`, `app/folha/tests/test_vigia_retido_calado.py:90`, `app/folha/tests/test_vigia_retido_calado.py:126`, `app/folha/tests/test_vigia_retido_calado.py:139`, `app/folha/tests/test_vigia_retido_calado.py:140`, `app/folha/tests/test_vigia_retido_calado.py:161`. |
| Testes ORM pelo command | `app/folha/tests/test_vigia_retido_calado.py:62`, `app/folha/tests/test_vigia_retido_calado.py:65`, `app/folha/tests/test_vigia_retido_calado.py:67`, `app/folha/tests/test_vigia_retido_calado.py:149`; interceptação com função real em `app/folha/tests/test_vigia_retido_calado.py:148`. |
| Testes sem banco, função real extraída | `app/chamados/tests/test_e3_r3_c4_sem_banco.py:154`, `app/chamados/tests/test_e3_r3_c4_sem_banco.py:157`, `app/chamados/tests/test_e3_r3_c5_sem_banco.py:103`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:253`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:350`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:85`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:74`. |
| Testes negativos da assinatura | Omissão de `hoje` deve continuar recusada: `app/chamados/tests/test_e3_r3_c5_sem_banco.py:243`, `app/chamados/tests/test_e3_r3_c5_sem_banco.py:245`. |
| Testes do command com substituto de `censo` | `app/chamados/tests/test_e3_r3_c11_sem_banco.py:275`, `app/chamados/tests/test_e3_r3_c11_sem_banco.py:281`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:317`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:321`. |

Precisam de mudança ou ampliação:

- **Sem foto alguma:** exigir os pares ausentes nomeados, além de `cego=1`; os casos atuais só cobram vazio/calados/cego (`app/folha/tests/test_vigia_retido_calado.py:31`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:111`).
- **Cobertura parcial estrutural:** o teste atual aceita `--alarme` com apenas a foto 10, faltando 11; esse resultado passa a ser alarme, ou a fixture precisa completar a cobertura (`app/folha/tests/test_vigia_retido_calado.py:64`, `app/folha/tests/test_vigia_retido_calado.py:65`).
- **Positivo preservado apesar da falta:** acrescentar a prova do alarme ao que já verifica falta nomeada e positivo conhecido (`app/folha/tests/test_vigia_retido_calado.py:186`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:103`).
- **Visibilidade e recorte:** acrescentar visível sem foto, inativa/diagnóstica com foto, seleção mista e seleção vazia; o selo atual cobre compartilhamento das competências, sem cobrar a origem das empresas (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:127`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:133`).
- **Infraestrutura sem banco:** adaptar os substitutos acima à consulta de empresas visíveis; na fase leitora eles oferecem essencialmente retratos e serviços auxiliares (`app/chamados/tests/test_e3_r3_c4_sem_banco.py:149`, `app/chamados/tests/test_e3_r3_c5_sem_banco.py:96`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:246`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:341`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:77`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:63`).
- **Substituto do command:** acrescentar a saída estrutural; hoje o dict c11 só fornece três chaves, enquanto o command acessa `d[k]` (`app/chamados/tests/test_e3_r3_c11_sem_banco.py:275`, `app/folha/management/commands/vigiar_retido_calado.py:38`).
- **Mutantes:** manter alvos vivos após refatorar o loop; os de seleção/falta dependem das expressões atuais (`app/chamados/tests/e3_r3_c16_mutantes.py:26`, `app/chamados/tests/e3_r3_c16_mutantes.py:28`).

**Campos: quem escreve, quem lê e significado**

4.2 pode conservar as nove chaves existentes: a mudança é de seleção e cobertura; `censo` monta um dict em memória e o devolve (`app/folha/vigia_retido_calado.py:61`, `app/folha/vigia_retido_calado.py:90`).

| Campo/valor | Quem escreve → quem lê; significado |
|---|---|
| `--empresa` → `empresas` | Command produz IDs ou `None`; vigia lê o recorte. O lavrador já intersecta sua PK com visíveis; o vigia atual apenas filtra fotos (`app/folha/management/commands/vigiar_retido_calado.py:31`, `app/folha/vigia_retido_calado.py:47`, `app/folha/management/commands/lavrar_retrato_folha.py:53`). |
| Empresa: `pk`, `ativa`, `cnpj`, `razao_social`, `dia_inicio_competencia` | Cadastro encaminha valores às portas `criar_empresa`/`atualizar_empresa`; elas gravam. Há também normalização de CNPJ. Visibilidade lê ativa/CNPJ e ordena razão social; janelas leem o corte (`app/colaboradores/views.py:521`, `app/colaboradores/views.py:532`, `app/colaboradores/services/estrutura.py:15`, `app/colaboradores/services/estrutura.py:21`, `app/colaboradores/services/estrutura.py:28`, `app/colaboradores/queries.py:85`, `app/ponto/janelas.py:50`). |
| `hoje` | Command fornece o dia da borda; vigia entrega ao helper de competências, ao balde e à C5 (`app/folha/management/commands/vigiar_retido_calado.py:33`, `app/folha/vigia_retido_calado.py:55`, `app/folha/vigia_retido_calado.py:60`, `app/folha/vigia_retido_calado.py:66`). |
| `(mes, ano)` esperado | `competencias_do_retrato` produz fins de anterior/atual, sem repetição; lavrador, frescor e vigia consomem. É competência pelo corte empresarial (`app/folha/retrato_folha.py:1380`, `app/folha/retrato_folha.py:1382`, `app/folha/management/commands/lavrar_retrato_folha.py:61`, `app/folha/retrato_folha.py:1411`, `app/folha/vigia_retido_calado.py:55`). |
| Foto: `empresa/mes/ano/linhas/lavrado_em` | Lavrador grava por trio, com linhas e instante inicial da rodada. Vigia lê par/linhas; frescor lê também o carimbo. `lavrado_em` não seleciona o universo do vigia (`app/folha/management/commands/lavrar_retrato_folha.py:77`, `app/folha/management/commands/lavrar_retrato_folha.py:79`, `app/folha/vigia_retido_calado.py:56`, `app/folha/vigia_retido_calado.py:65`, `app/folha/retrato_folha.py:1413`). |
| Linha: `colab/status/motivo/datas/dono/dias_sem_leitura` | `montar_linhas` escreve; porta de bloqueados lê status/motivo, dono lê fio/datas, vigia lê colaborador e presença/tamanho dos dias sem leitura. **`dono` gravado significa fio**, depois projetado em balde (`app/folha/retrato_folha.py:1237`, `app/folha/retrato_folha.py:1248`, `app/folha/retrato_folha.py:91`, `app/folha/retrato_folha.py:446`, `app/folha/retrato_folha.py:490`, `app/folha/vigia_retido_calado.py:67`, `app/folha/vigia_retido_calado.py:72`). |
| C5: conjunto de colaboradores roteados | `colabs_nunca_bateu` produz; vigia cruza com `linha.colab`. Deve receber os IDs **efetivos** do recorte visível: hoje `None` significa todas as empresas no auxiliar. Sua janela continua anterior.início até ontem (`app/escala/services/cadastro_realidade.py:163`, `app/escala/services/cadastro_realidade.py:167`, `app/folha/vigia_retido_calado.py:60`, `app/folha/vigia_retido_calado.py:67`). |

| Saída do dict | Escritor → leitor; efeito |
|---|---|
| `retratos` | Vigia acrescenta `(empresa,mes,ano)`; formatador conta e `cego` verifica vazio. Exclui fotos de empresas fora do universo (`app/folha/vigia_retido_calado.py:76`, `app/folha/vigia_retido_calado.py:95`, `app/folha/vigia_retido_calado.py:89`). |
| `competencias_sem_retrato` | Vigia produz; formatador conta e nomeia. Passa a abranger empresa sem foto alguma e a alimentar o estrutural (`app/folha/vigia_retido_calado.py:58`, `app/folha/vigia_retido_calado.py:61`, `app/folha/vigia_retido_calado.py:95`, `app/folha/vigia_retido_calado.py:96`). |
| `bloqueados/calados/roteados` | Vigia soma; formatador publica. Command lê `calados` no passivo. Unidade permanece pessoa por retrato/competência (`app/folha/vigia_retido_calado.py:16`, `app/folha/vigia_retido_calado.py:77`, `app/folha/vigia_retido_calado.py:99`, `app/folha/vigia_retido_calado.py:101`, `app/folha/vigia_retido_calado.py:102`, `app/folha/management/commands/vigiar_retido_calado.py:17`). |
| `dias_sem_leitura_sistema` | Vigia agrega; formatador e alarme passivo leem. Positivo conhecido sobrevive; `None` permanece distinto de zero (`app/folha/vigia_retido_calado.py:88`, `app/folha/vigia_retido_calado.py:103`, `app/folha/management/commands/vigiar_retido_calado.py:17`). |
| `linhas_sem_campo_leitura` | Vigia soma; formatador publica. Mede legado sem campo, não competência sem foto (`app/folha/vigia_retido_calado.py:74`, `app/folha/vigia_retido_calado.py:80`, `app/folha/vigia_retido_calado.py:104`). |
| `por_empresa` | Vigia grava mês/ano e contadores; formatador lê mês/ano/bloqueados/calados/roteados. A chave é **`(empresa,ano,mes)`**, ordem diferente das duas listas; testes leem também o aninhado sem leitura (`app/folha/vigia_retido_calado.py:83`, `app/folha/vigia_retido_calado.py:98`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:309`). |
| `cego` | Vigia calcula “sem fotos ou sem bloqueados”; formatador e estrutural atual leem. Não substitui o contador de pares ausentes (`app/folha/vigia_retido_calado.py:89`, `app/folha/vigia_retido_calado.py:102`, `app/folha/management/commands/vigiar_retido_calado.py:16`). |

**Não há significado uniforme em todos os consumidores.** Frescor usa visíveis; a tela pode selecionar empresa oculta por PK; C5 com `None` usa todas. Competência automática vem da janela, mas lavra explícita e URL podem indicar outro par. O dia civil do carimbo não é o mês da competência (`app/folha/retrato_folha.py:1410`, `app/folha/views_retrato.py:161`, `app/escala/services/cadastro_realidade.py:163`, `app/folha/management/commands/lavrar_retrato_folha.py:57`, `app/folha/views_retrato.py:181`, `app/ponto/janelas.py:11`).

**Destinos das saídas**

| Destino | O que muda |
|---|---|
| Cron | Mantém `--alarme`; falta parcial passa a poder produzir rc2. Comentário “só pelo cego” precisa acompanhar o novo estrutural (`app/config/crons.py:1679`, `app/config/crons.py:1681`, `app/folha/management/commands/vigiar_retido_calado.py:41`). |
| Placar | Não lê o dict VRC: envelope grava `exit`; placar conta `exit==2` como alarme. Muda potencialmente o valor, sem exigir nova chave no placar (`bin/cron_run.sh:13`, `app/core/management/commands/pipeline_placar.py:77`). |
| Tela | Não consome o dict VRC: consulta fotos e frescor. Seu aviso continua eliminando empresas sem foto; 4.2 sozinho não corrige isso (`app/folha/views_retrato.py:192`, `app/folha/views_retrato.py:209`, `app/folha/views_retrato.py:210`). |
| RELATO | É registro histórico de universo/medidas, não leitor executável. Números novos de produção: **[nao verificado]** (`app/docs/RELATO.md:4349`, `app/docs/RELATO.md:1644`). |
| Contador de frescor | `retratos_desatualizados` permanece separado, contando também `nunca lavrado`; não recebe VRC (`app/folha/retrato_folha.py:1454`). |

**Outros universos próprios**

Não consigo certificar a lista **exata dentro do diff `f31a8d09e × base geral`**: comparação **[nao verificado]**. Os artefatos disponíveis identificam bases parciais diferentes, LU3 e c12 (`app/chamados/tests/e3_lu3_entrega.txt:2`, `app/chamados/tests/e3_r3_c12_entrega.txt:1`).

Na cópia, além do vigia retido, permanecem:

| Leitor | Seleção própria |
|---|---|
| Recorte de tela/PDF/raiox | PK/preferência pode escapar das visíveis; `todas` aplica a competência atual da primeira empresa; URL e navegação escolhem outros pares (`app/folha/views_retrato.py:161`, `app/folha/views_retrato.py:179`, `app/folha/views_retrato.py:181`, `app/folha/views_retrato.py:189`, `app/folha/views_retrato.py:192`). |
| Aviso de frescor | Restringe às empresas com foto existente (`app/folha/views_retrato.py:209`, `app/folha/views_retrato.py:211`). |
| Painel do colaborador | Empresa do colaborador cruzada com visíveis; par exato da URL (`app/colaboradores/views.py:975`, `app/colaboradores/views.py:982`). |
| Vigia fio, alcance | Visíveis/interseção; atual e anterior somente se não entregue (`app/chamados/services/vigia_fio.py:517`, `app/chamados/services/vigia_fio.py:535`, `app/ponto/services/he_pendente_lavrado.py:223`). |
| Vigia fio antigo / ListaDoAdmin | Antigo parte de chamados vivos com célula; lista cruza visíveis separadamente (`app/chamados/services/vigia_fio.py:290`, `app/chamados/services/vigia_fio.py:292`, `app/chamados/services/vigia_fio.py:191`). |
| C5 subsidiária | Todas quando `None`; anterior.início até ontem (`app/escala/services/cadastro_realidade.py:163`, `app/escala/services/cadastro_realidade.py:167`). |
| Drawer | Recompõe anterior por `janela_atual(início_atual−1 dia)` e aplica piso visual (`app/colaboradores/services/drawer.py:115`, `app/colaboradores/services/drawer.py:116`, `app/colaboradores/services/drawer.py:118`). |
| Calendário / consulta plural | Modos civil/competência/custom escolhem períodos; consulta plural calcula a atual para excluir dias da oferta (`app/colaboradores/services/calendario.py:194`, `app/colaboradores/services/calendario.py:205`, `app/colaboradores/services/calendario.py:227`, `app/colaboradores/services/calendario.py:143`). |
| `CargaDaTarefa._ler_exportadas` | Deriva competência de cada dia recebido (`app/chamados/services/tarefa_do_dia.py:471`, `app/chamados/services/tarefa_do_dia.py:482`). |
| `escalas_em_limbo` | Escalas ativas e foto `FolgaDia` do mês civil; é universo diagnóstico próprio (`app/relatorios/furos.py:60`, `app/relatorios/furos.py:88`). |

Calendário, drawer, vigia fio e dataset constam do diff parcial LU3 documentado; isso não prova a pertença de todos esses sítios ao diff integral solicitado (`app/chamados/tests/e3_lu3_entrega.txt:151`, `app/chamados/tests/e3_lu3_entrega.txt:163`, `app/chamados/tests/e3_lu3_entrega.txt:164`, `app/chamados/tests/e3_lu3_entrega.txt:168`).

**(b) A lista decidida não equivale ao conjunto de relógios no salto 1.**

| Classificação | Sítio e motivo |
|---|---|
| Dentro correto | `furos_espelho → escalas_em_limbo`: `localdate()` próprio no salto 1, apesar do instante/dia disponíveis na view (`app/relatorios/views.py:1519`, `app/relatorios/views.py:1520`, `app/relatorios/views.py:1554`, `app/relatorios/furos.py:59`). |
| Falta no conjunto salto 1, mas fora expresso | PDF → `gerar_pdf_auditavel`: outro `now()`; recorte devolve somente o dia (`app/folha/views_retrato.py:245`, `app/folha/views_retrato.py:441`, `app/relatorios/pdf_base.py:133`). |
| Falta no conjunto salto 1, mas fora expresso | Raiox → `placar_aceite`: **duas chamadas**, `now()` e `localdate()`, no ramo colab (`app/folha/views_retrato.py:511`, `app/folha/views_retrato.py:513`, `app/chamados/services/verbos.py:286`, `app/chamados/services/verbos.py:287`). |
| Falta sintática, fallback suprimido | Calendário/drawer → `contexto_calendario`: contém `now()`, mas recebe instante nesses caminhos (`app/colaboradores/views.py:1952`, `app/colaboradores/services/drawer.py:124`, `app/colaboradores/services/calendario.py:192`). |
| Falta sintática considerando também auxiliares do anexo | `ListaDoAdmin._carregar → lista`: `now()` no salto 1; suprimido quando `self.agora` foi fornecido (`app/chamados/services/vigia_fio.py:179`, `app/chamados/services/vigia_fio.py:194`, `app/escala/services/cadastro_realidade.py:780`). |
| Sobra **como sítio de relógio salto 1** | `dataset_furos` não chama relógio; repassa `agora`. Seu D4 fica no salto 2. Exigir argumento continua sendo correção de contrato dentro do escopo decidido (`app/relatorios/furos.py:103`, `app/relatorios/furos.py:123`, `app/relatorios/furos.py:164`, `app/ponto/status_dia.py:253`). |

Selo e docstrings não são nós de relógio; não sobram da obra por isso. O defeito do selo é concreto: conjunto de nomes elimina chamadas repetidas, e o outro helper conta alvos de atribuição. A identificação exata do rótulo “docstrings G3” permanece **[nao verificado]** (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:162`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:182`).

Essas faltas descrevem a diferença para o censo; PDF e placar continuam sendo exclusões expressas da decisão recebida **[nao verificado quanto a qualquer ampliação posterior do escopo]**.

**(c) Contrato proposto para c17 — execução e resultados [nao verificado].**

1. Visíveis ∩ `--empresa` governam vigia e C5 → teste da função real com visível/oculta, recorte misto/vazio e IDs entregues à C5 (`app/folha/vigia_retido_calado.py:46`, `app/folha/vigia_retido_calado.py:60`).
2. Empresa visível sem foto sai com todos os pares faltantes → ampliar o caso vazio para cobrar pares e saída nomeada (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:111`, `app/folha/vigia_retido_calado.py:96`).
3. Falta parcial é estrutural, positivo permanece → teste real do command exige rc2 sem passivo e conserva contagens conhecidas (`app/folha/tests/test_vigia_retido_calado.py:65`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:103`).
4. Janela por empresa conserva meia-noite/relavra e exclui competência velha explícita → preservar casos comportamentais e mutantes com alvo vivo (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:78`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:86`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:95`).
5. Unidades, legado/`None` e ordem dos trios permanecem explícitos → testes de pessoa/competência, dia-colab e positivo entre empresas (`app/folha/vigia_retido_calado.py:76`, `app/folha/vigia_retido_calado.py:83`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:324`).
6. Limbo usa o dia da borda → teste da função real junto à meia-noite, proibindo relógio interno e verificando o dia recebido (`app/relatorios/views.py:1554`, `app/relatorios/furos.py:59`).
7. `dataset_furos` exige instante válido → omissão/`None` recusados; teste real verifica transporte ao D4/calendário e atualiza chamadores de teste (`app/relatorios/furos.py:103`, `app/relatorios/furos.py:123`, `app/relatorios/tests/test_e4_8_furos_le_a_lavra.py:92`).
8. “Um relógio” conta chamadas → selo rejeita dois `now()`, chamada solta e alias; conversões com instante explícito não somam leitura (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:159`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:180`).
9. Docstrings descrevem o contrato efetivo e suas exclusões → revisão G3 identificada, sem prometer fecho transitivo sem relógios (`app/folha/vigia_retido_calado.py:16`, `app/folha/retrato_folha.py:1372`, `app/relatorios/furos.py:109`).

## 15/11 19:40 · sessao dd6001c0d496 · - · - · origem=Ronald (tmux)

Todas as referências abaixo são relativas à raiz da cópia; não executei testes nem fiz alterações. [nao verificado]

**Chamadores de `censo`**

| Classe | Sítios |
|---|---|
| Produção direta | Import e chamada no command: `app/folha/management/commands/vigiar_retido_calado.py:14`, `:33`. |
| Produção indireta | Cron `ENC(... args='--alarme')`: `app/config/crons.py:1681`; dependência do lavrador `:1683`; papel `vigia` `:2260`. |
| Testes ORM diretos | Helpers `app/folha/tests/test_vigia_retido_calado.py:29`, `:161`; chamadas `:90`, `:126`, `:139`, `:140`; command interceptado com `wraps=vrc.censo` `:148`. |
| Testes ORM pelo command | `app/folha/tests/test_vigia_retido_calado.py:62`, `:65`, `:67`, `:149`. |
| Testes sem banco, função real extraída | `app/chamados/tests/test_e3_r3_c4_sem_banco.py:154`, `:157`; `test_e3_r3_c5_sem_banco.py:103`; `test_e3_r3_c6_sem_banco.py:253`; `test_e3_r3_c14_sem_banco.py:350`; `test_e3_r3_c15_sem_banco.py:85`; `test_e3_r3_c16_sem_banco.py:74`. |
| Chamadas negativas de assinatura | `app/chamados/tests/test_e3_r3_c5_sem_banco.py:243`, `:245`: sem `hoje`, devem continuar `TypeError`. |
| Testes do command com censo substituído | `app/chamados/tests/test_e3_r3_c11_sem_banco.py:275`, `:281`; `test_e3_r3_c14_sem_banco.py:317`, `:321`. |

**Retorno e leitores**

`censo` escreve as nove chaves: `retratos`, `competencias_sem_retrato`, `bloqueados`, `calados`, `roteados`, `dias_sem_leitura_sistema`, `linhas_sem_campo_leitura`, `por_empresa`, `cego` (`app/folha/vigia_retido_calado.py:61`, `:62`, `:88`, `:89`).

| Saída | Quem escreve | Quem lê e efeito de 4.2 |
|---|---|---|
| `retratos`: `(empresa,mes,ano)` | `app/folha/vigia_retido_calado.py:76` | `linhas` imprime quantidade `:95`; `cego` usa vazio `:89`. Passa a conter somente fotos nas visíveis/interseção e nas janelas. |
| `competencias_sem_retrato`: `(empresa,mes,ano)` | `app/folha/vigia_retido_calado.py:58`, `:61` | `linhas` conta `:95` e nomeia cada par `:96`. Precisa passar a incluir empresa visível sem **nenhuma** foto; hoje loop só existe a partir das fotos `:51`. |
| `bloqueados`, `calados`, `roteados` | `app/folha/vigia_retido_calado.py:77`, `:78`, `:79` | `linhas`: universo `:95`, roteados `:101`, contador `:102`; command alarma `calados` somente com passivo, `app/folha/management/commands/vigiar_retido_calado.py:17`, `:37`. Valores podem mudar pela seleção de fotos. |
| `dias_sem_leitura_sistema` | `app/folha/vigia_retido_calado.py:88` | `linhas` `:103`, `:105`; command passivo `app/folha/management/commands/vigiar_retido_calado.py:17`. Mantém positivo conhecido; `None` só sem soma conhecida e com legado `app/folha/vigia_retido_calado.py:88`. |
| `linhas_sem_campo_leitura` | `app/folha/vigia_retido_calado.py:80` | `linhas` `:104`, `:105`; muda somente seleção do universo. |
| `por_empresa`: chave `(empresa,ano,mes)` | `app/folha/vigia_retido_calado.py:83` | `linhas` `:98`, `:99` lê mês/ano/bloqueados/calados/roteados. Campos aninhados de leitura são cobrados nos testes `app/chamados/tests/test_e3_r3_c6_sem_banco.py:269`, `test_e3_r3_c14_sem_banco.py:309`, `:352`. **Ordem difere** de `retratos` e `competencias_sem_retrato`. |
| `cego` | `app/folha/vigia_retido_calado.py:89` | `linhas` `:102`; único estrutural atual, `app/folha/management/commands/vigiar_retido_calado.py:16`, acionado `:38`. Não identifica cobertura parcial se já existe um bloqueado. |

O contador de ausências **já é impresso**, mas ainda não é estrutural no alarme: o command só tem `ALARMA_SEMPRE=('cego',)` (`app/folha/management/commands/vigiar_retido_calado.py:16`), enquanto `competencias_sem_retrato` é apenas listado (`app/folha/vigia_retido_calado.py:94`, `:96`). Para cumprir “par sem retrato no contador estrutural”, é preciso consumir a falta no alarme, preservando sua lista nomeada; a alternativa exata de nome/schema adicional não foi decidida no texto **[nao verificado]**.

**Cron, placar, tela, RELATO, contador**

- Cron continua chamando o command com `--alarme`; comentário “só pelo cego” fica incorreto se a falta de par virar estrutural (`app/config/crons.py:1679`, `:1681`; `app/folha/management/commands/vigiar_retido_calado.py:5`).
- Placar não recebe o dict: `encadeado` passa pelo envelope (`bin/encadeado.sh:102`); o envelope grava `nome/emp/ini/fim/dur/exit` (`bin/cron_run.sh:13`); `pipeline_placar` conta `exit==2` como alarme (`app/core/management/commands/pipeline_placar.py:77`). Assim mudam potencialmente rc/alarmes/duração, sem nova chave no JSON do placar.
- Tela consulta fotos diretamente (`app/folha/views_retrato.py:192`), e frescor separado (`:210`); template mostra `frescor_aviso[*].rotulo_admin` (`app/templates/folha/_retrato_barra.html:22`). Não é migração de consumidor VRC: permanece divergência de universo porque aviso só aceita empresas com foto (`app/folha/views_retrato.py:209`, `:211`).
- `RELATO` registra medidas históricas, não é consumidor executável: explica origem/unidade e command (`app/docs/RELATO.md:4347`, `:4349`), conserva smoke histórico (`:4353`, `:1644`). Não inferir números novos desses registros **[nao verificado]**.
- Contador separado `retratos_desatualizados` é `len(frescor)` com `desatualizado` **ou** `nunca lavrado`, sem VRC (`app/folha/retrato_folha.py:1448`, `:1454`).

**Testes que precisam mudar ou ser ampliados** [nao verificado]

1. Empresa sem qualquer foto: exigir pares nomeados; hoje só exige `cego=1` (`app/folha/tests/test_vigia_retido_calado.py:31`, `:33`; sem banco `app/chamados/tests/test_e3_r3_c16_sem_banco.py:111`, `:113`).
2. Alarme de cobertura parcial: hoje aceita `--alarme` com só foto 10 e falta 11 (`app/folha/tests/test_vigia_retido_calado.py:64`, `:65`); passa a precisar foto 11 ou esperar rc2.
3. Caso de falta parcial preserva positivo (`app/folha/tests/test_vigia_retido_calado.py:186`, `:190`; sem banco `app/chamados/tests/test_e3_r3_c16_sem_banco.py:103`, `:108`): acrescentar verificação estrutural/command.
4. Selo compartilhado prova apenas helper de competências, não empresas (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:127`, `:133`, `:134`): acrescentar visíveis/interseção e empresa sem fotos.
5. Adaptar fixtures sem banco ao import/consulta de `empresas_visiveis`; hoje c4/c5/c6/c14/c15/c16 só simulam fotos na fase leitora (`app/chamados/tests/test_e3_r3_c4_sem_banco.py:149`; `test_e3_r3_c5_sem_banco.py:96`; `test_e3_r3_c6_sem_banco.py:246`; `test_e3_r3_c14_sem_banco.py:341`; `test_e3_r3_c15_sem_banco.py:77`; `test_e3_r3_c16_sem_banco.py:63`).
6. Mock c11 precisa chave estrutural se command mantiver acesso `d[k]`; retorno atual tem apenas `cego/calados/dias_sem_leitura_sistema` (`app/chamados/tests/test_e3_r3_c11_sem_banco.py:275`; acesso `app/folha/management/commands/vigiar_retido_calado.py:38`).
7. Mutantes de seleção/falta c16 dependem de strings do loop antigo e precisam alvo vivo se refatorado (`app/chamados/tests/e3_r3_c16_mutantes.py:26`, `:28`). Mutantes de c4/c5/c6/c14 preservam contratos de balde/unidade/desconhecido (`e3_r3_c4_mutantes.py:33`, `e3_r3_c5_mutantes.py:44`, `e3_r3_c6_mutantes.py:45`, `e3_r3_c14_mutantes.py:53`).

**Insumos: escritor/leitor e significado**

- `Empresa.ativa/cnpj/razao_social`: visibilidade definida por `empresas_visiveis` = ativa, exclui CNPJ `11222333*`, ordena razão social (`app/colaboradores/queries.py:85`). Escritores desses campos não investigados **[nao verificado]**.
- `--empresa`: command escreve lista de IDs ou `None` (`app/folha/management/commands/vigiar_retido_calado.py:31`); censo lê filtro (`app/folha/vigia_retido_calado.py:47`). Lavrador usa uma PK intersectando visíveis (`app/folha/management/commands/lavrar_retrato_folha.py:51`, `:53`).
- `hoje`: command escreve pelo relógio da borda (`app/folha/management/commands/vigiar_retido_calado.py:33`); helper usa para janelas (`app/folha/retrato_folha.py:1379`), balde usa para hoje/futuro (`:491`), C5 recebe (`app/folha/vigia_retido_calado.py:60`).
- Foto `empresa/mes/ano/linhas/lavrado_em`: escritor lavrador `update_or_create` por trio, defaults linhas/carimbo (`app/folha/management/commands/lavrar_retrato_folha.py:77`); unicidade garantida (`app/folha/models.py:366`). Censo lê empresa/mes/ano/linhas (`app/folha/vigia_retido_calado.py:51`, `:56`, `:65`), não usa `lavrado_em` para universo; frescor lê carimbo (`app/folha/retrato_folha.py:1413`).
- Linhas `colab/status/motivo/dias/datas/dias_sem_leitura/dias_sem_porta/dono`: montador escreve (`app/folha/retrato_folha.py:1237`); VRC pede bloqueados pela porta `linhas_do_retrato` (`app/folha/vigia_retido_calado.py:65`), balde por `dono_da_linha` (`:66`), C5 pelo `colab` (`:67`), dias sem leitura por presença e tamanho (`:72`, `:73`). `dono` gravado significa **fio**, não balde (`app/folha/retrato_folha.py:1246`, `:1248`; leitor `:446`, `:486`).
- C5: hoje recebe IDs originais; `None` significa **todas** em `colabs_nunca_bateu`, inclusive ocultas (`app/folha/vigia_retido_calado.py:60`; `app/escala/services/cadastro_realidade.py:163`). C17 deve passar IDs efetivamente visíveis/intersectados para alinhar empresas; sua janela continua anterior até ontem (`cadastro_realidade.py:162`, `:167`), não “competência do dia civil”.
- Não há mesmo significado em todos os consumidores: consulta documenta que export/internos não usam visibilidade de propósito (`app/colaboradores/queries.py:82`); vigia hoje parte de todas as fotos (`app/folha/vigia_retido_calado.py:46`), frescor usa visíveis (`app/folha/retrato_folha.py:1410`), tela por PK pode pegar oculta (`app/folha/views_retrato.py:161`). Competência é fim da janela por empresa (`app/folha/retrato_folha.py:1380`), enquanto filtro civil `lavrado_em` é explicitamente recusado (`app/folha/vigia_retido_calado.py:23`).

## 15/11 19:39 · sessao bb4d3cb27c21 · - · - · origem=Ronald (tmux)

(b) **O conjunto de correções de relógio não coincide com “todos os sítios de relógio no salto 1”**; o anexo distingue leituras próprias, fallbacks suprimidos e omissões, e o código confirma essa distinção (`app/relatorios/furos.py:59`, `app/colaboradores/services/calendario.py:192`, `app/chamados/juizes.py:888`).

| Classificação | Sítio e motivo |
|---|---|
| **DENTRO correto** | `furos_espelho → escalas_em_limbo`: salto 1, leitura própria `timezone.localdate()`, embora a view já tenha `_agora` e `hoje`; falta transmitir o dia recebido (`app/relatorios/views.py:1519`, `app/relatorios/views.py:1520`, `app/relatorios/views.py:1554`, `app/relatorios/furos.py:59`). |
| **FALTA ao conjunto de sítios salto 1, mas FORA expresso** | `retrato_pdf → gerar_pdf_auditavel`: salto 1, outra chamada `timezone.now()`; `_recorte_da_barra` só devolve `hoje`, não `_agora` (`app/folha/views_retrato.py:245`, `app/folha/views_retrato.py:386`, `app/folha/views_retrato.py:441`, `app/relatorios/pdf_base.py:133`). |
| **FALTA ao conjunto de sítios salto 1, mas FORA expresso** | `retrato_raiox → placar_aceite`: salto 1, **duas** leituras próprias, `.now()` e `.localdate()`, no ramo `dono == ['colab']` (`app/folha/views_retrato.py:511`, `app/folha/views_retrato.py:513`, `app/chamados/services/verbos.py:286`, `app/chamados/services/verbos.py:287`). |
| **Sítio salto 1 ausente da lista; fallback já suprimido** | `calendario_ponto/contexto_drawer → contexto_calendario`: o helper contém `.now()`, mas ambas as bordas fornecem `agora`; não representa nova leitura nesses caminhos (`app/colaboradores/views.py:1952`, `app/colaboradores/services/drawer.py:123`, `app/colaboradores/services/drawer.py:124`, `app/colaboradores/services/calendario.py:192`). |
| **Sítio salto 1 adicional se incluídas as entradas auxiliares do anexo** | `ListaDoAdmin._carregar → cadastro_realidade.lista`: `.now()` no salto 1; fica suprimido quando `self.agora` foi fornecido, mas o construtor permite `None` (`app/chamados/services/vigia_fio.py:177`, `app/chamados/services/vigia_fio.py:179`, `app/chamados/services/vigia_fio.py:194`, `app/escala/services/cadastro_realidade.py:780`). |
| **SOBRA somente se a lista for intitulada “sítios de relógio salto 1”** | `dataset_furos` não lê relógio: tem `agora=None` e repassa o argumento ao D4 e calendário; o D4 está no salto 2 da view. Tornar `agora` obrigatório é endurecimento de contrato válido, não remoção de relógio próprio no salto 1 (`app/relatorios/furos.py:103`, `app/relatorios/furos.py:123`, `app/relatorios/furos.py:164`, `app/ponto/status_dia.py:253`). |

**Não classificaria selo e docstrings como SOBRA da obra:** são instrumentos e documentação, não nós de relógio do grafo; o defeito do selo é concreto, pois `_le_relogio` usa conjunto de nomes e `_instante_da_borda` conta alvos de atribuição (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:162`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:182`). O selo do drawer só compara `['now']`; o do relatório combina esse conjunto com a quantidade de alvos, sem contar todas as chamadas (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:152`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:156`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:158`).

**Não são relógios salto 1 faltantes:** `viva_em_disputa_fechada` e `carimbos_sem_lastro_por_dono` chamam `pergunta_viva` sem argumentos no salto 1, mas `pergunta_viva` não consulta relógio: apenas deriva o dia se recebeu instante; os relógios surgem nos degraus seguintes (`app/chamados/services/vigia_fio.py:718`, `app/chamados/services/vigia_fio.py:776`, `app/chamados/juizes.py:886`, `app/chamados/juizes.py:888`, `app/chamados/juizes.py:897`). A propriedade `estado` é salto 2 a partir de `dono_fora_do_fio`, não salto 1 dos censos principais (`app/chamados/services/vigia_fio.py:259`, `app/chamados/services/vigia_fio.py:387`, `app/chamados/services/vigia_fio.py:615`).

A identidade exata de “docstrings G3” não aparece nomeada nos arquivos examinados **[nao verificado]**. Documentação evidentemente afetada por 4.2: o vigia promete “universo do retrato gravado” e descreve faltantes, embora derive empresas apenas dos retratos existentes; a docstring do helper promete que os três consumidores medem o mesmo universo (`app/folha/vigia_retido_calado.py:16`, `app/folha/vigia_retido_calado.py:27`, `app/folha/vigia_retido_calado.py:51`, `app/folha/retrato_folha.py:1372`). O help text do carimbo já diz “INICIO da rodada”, também na migração declarada apenas de help text (`app/folha/models.py:358`, `app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py:1`, `app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py:16`).

## 15/11 19:39 · sessao 94c8f53bb9d0 · - · - · origem=Ronald (tmux)

A comparação exata `f31a8d09e × base da obra` fica **[nao verificado]**: não há metadados Git nesta cópia; os artefatos identificam bases parciais distintas — LU3 `522b7eb48` (`app/chamados/tests/e3_lu3_censo.txt:3`; `app/chamados/tests/e3_lu3_entrega.txt:2`), LU2 `de702cb2f` (`app/chamados/tests/e3_lu2_censo.txt:1`) e c12 `20f5a0338` (`app/chamados/tests/e3_r3_c12_entrega.txt:1`).

Outros leitores que montam universos, confirmados no código:

| Leitor | Universo próprio e divergência |
|---|---|
| `_recorte_da_barra`, compartilhado por tela/PDF/raiox | Dropdown visível, mas seleção por PK não cruza visibilidade; `todas` usa a competência atual da primeira empresa; URL escolhe outro par; navegação calcula ±1 mês (`app/folha/views_retrato.py:147`, `:153`, `:157`, `:161`, `:164`, `:179`, `:181`, `:189`, `:192`). As três views chamam o recorte (`app/folha/views_retrato.py:258`, `:386`, `:487`). |
| Aviso de frescor | Refiltra o frescor pelas empresas com foto existente, omitindo justamente as sem retrato (`app/folha/views_retrato.py:209`, `:211`). |
| Painel do colaborador | Empresa do colaborador cruzada com visíveis, competência exata da URL; não percorre as duas janelas (`app/colaboradores/views.py:975`, `:977`, `:982`). |
| `vigia_fio.censo_alcance` | Visíveis com interseção, mas usa `competencias_a_lavrar`, cuja anterior só entra se não entregue (`app/chamados/services/vigia_fio.py:517`, `:520`, `:535`; `app/ponto/services/he_pendente_lavrado.py:220`, `:223`). |
| `vigia_fio.censo` antigo | Chamados vivos com célula, sem cruzar visibilidade quando `empresas=None`; não escolhe duas competências (`app/chamados/services/vigia_fio.py:290`, `:292`). |
| `ListaDoAdmin` | Cruza novamente empresas visíveis e o recorte fornecido (`app/chamados/services/vigia_fio.py:191`, `:193`, `:194`). |
| C5 `colabs_nunca_bateu` | `None` significa todas as empresas; seleciona células entre início da anterior e ontem. Em 4.2 deve receber os IDs **efetivos visíveis**, para não conservar o universo largo subsidiário (`app/escala/services/cadastro_realidade.py:162`, `:163`, `:167`; chamada atual `app/folha/vigia_retido_calado.py:60`). |
| `contexto_drawer` | Recompõe atual e anterior por `_ja(atual_ini-1dia)`, com piso visual próprio (`app/colaboradores/services/drawer.py:111`, `:115`, `:116`, `:118`, `:123`). |
| `contexto_calendario` | Default mês/ano civil; modo competência consulta `janela_fechamento`; demais modos constroem períodos próprios (`app/colaboradores/services/calendario.py:194`, `:205`, `:209`, `:210`, `:227`, `:239`). |
| Consulta plural do calendário | Calcula janela atual da empresa para excluir dias correntes da oferta (`app/colaboradores/services/calendario.py:140`, `:143`, `:150`). |
| `CargaDaTarefa._ler_exportadas` | Deriva competência de cada dia recebido, sem usar as duas competências de hoje (`app/chamados/services/tarefa_do_dia.py:467`, `:471`, `:479`, `:482`). |
| `escalas_em_limbo` | Universo de escalas ativas, recorte opcional de empresa e foto `FolgaDia` do mês civil (`app/relatorios/furos.py:60`, `:63`, `:86`, `:88`). |

Os módulos `vigia_fio`, calendário, drawer e furos constam do diff **parcial LU3** documentado (`app/chamados/tests/e3_lu3_entrega.txt:151`, `:163`, `:164`, `:168`); pertença de cada função ao diff integral solicitado: **[nao verificado]**.

Escritores e significado das fontes usadas por 4.2:

- `Empresa.ativa`, `cnpj` e `dia_inicio_competencia` são campos definidos em `app/colaboradores/models.py:43`, `:44`, `:46`. A tela recebe os valores e chama `atualizar_empresa`/`criar_empresa` (`app/colaboradores/views.py:492`, `:493`, `:496`, `:521`, `:532`); as portas gravam por `create`/`save` (`app/colaboradores/services/estrutura.py:13`, `:15`, `:18`, `:21`). Também existe escritor explícito de normalização do CNPJ (`app/colaboradores/services/estrutura.py:25`, `:28`), e `EmpresaAdmin` deixa apenas `regime_trabalhista` somente leitura (`app/colaboradores/admin.py:8`, `:19`).
- `empresas_visiveis` lê `ativa=True` e exclui CNPJ `11222333*` (`app/colaboradores/queries.py:85`, `:86`); o contrato documentado do helper diz que integrações e lógica interna não o usam deliberadamente (`app/colaboradores/queries.py:79`, `:82`). Logo, a equivalência “empresa = visível” **não é universal** (`app/escala/services/cadastro_realidade.py:163`; `app/chamados/services/vigia_fio.py:290`).
- `competencias_do_retrato` deriva mês/ano do fim de anterior/atual, com deduplicação (`app/folha/retrato_folha.py:1377`, `:1380`, `:1382`). As janelas leem o corte empresarial; competência corresponde ao mês do **fim**, não ao mês civil de hoje (`app/ponto/janelas.py:10`, `:16`, `:36`, `:50`, `:60`, `:66`).
- `RetratoFolha.empresa/mes/ano/linhas/lavrado_em` são gravados pelo lavrador via `update_or_create`; `lavrado_em` recebe o instante inicial da rodada (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `:77`, `:79`). A chave única é empresa/mês/ano (`app/folha/models.py:366`). Frescor lê o par e o carimbo (`app/folha/retrato_folha.py:1412`, `:1413`); vigia lê o par e as linhas, sem selecionar por `lavrado_em` (`app/folha/vigia_retido_calado.py:51`, `:55`, `:65`).
- O lavrador explícito pode gravar competência fora das duas janelas (`app/folha/management/commands/lavrar_retrato_folha.py:57`, `:58`); a tela e o painel podem lê-la por URL (`app/folha/views_retrato.py:181`, `:192`; `app/colaboradores/views.py:975`, `:982`). Portanto, “competência lida = competência das duas janelas automáticas” também **não é universal**.

## 15/11 18:53 · sessao 3b3ebdd4be3c · E3-RESTO-R3 · censo · origem=Code

Os caminhos abaixo são relativos a `app/`. Distingo **leitura própria**, **fallback suprimido pelo argumento recebido** e **omissão de argumento**. Execução efetiva em produção: **[não verificado]**.

O caminho solicitado `folha/vigia_fio.py` não foi localizado **[não verificado]**. Incluí separadamente o módulo encontrado, `chamados/services/vigia_fio.py`, cujas entradas são `censo` e `censo_alcance`; equivalência com o caminho solicitado: **[não verificado]** (`chamados/services/vigia_fio.py:266`, `chamados/services/vigia_fio.py:487`).

**1. Relógio transitivo — até seis chamadas**

**Instante disponível nas bordas**

| Borda | O que tem em mãos |
|---|---|
| `lavrar_retrato_folha.Command.handle` | `_agora` e seu dia local (`folha/management/commands/lavrar_retrato_folha.py:49`, `folha/management/commands/lavrar_retrato_folha.py:50`). |
| `vigiar_retido_calado.Command.handle` | Lê somente o dia, diretamente no argumento de `censo`; não conserva um `datetime` (`folha/management/commands/vigiar_retido_calado.py:33`). |
| `_recorte_da_barra` | `_agora` e `_hoje`; devolve o dia às views, mas não devolve `_agora` (`folha/views_retrato.py:177`, `folha/views_retrato.py:178`, `folha/views_retrato.py:238`, `folha/views_retrato.py:245`). |
| `calendario_ponto` | `_agora`, passado ao calendário (`colaboradores/views.py:1951`, `colaboradores/views.py:1952`). |
| `contexto_drawer` | `agora` e dia local (`colaboradores/services/drawer.py:21`, `colaboradores/services/drawer.py:22`). |
| `furos_espelho` | `_agora`, passado ao dataset (`relatorios/views.py:1519`, `relatorios/views.py:1543`). |
| `frescor_dos_retratos` | `agora` obrigatório (`folha/retrato_folha.py:1386`). |
| `vigia_fio.censo` / `censo_alcance` | O primeiro recebe ou lê `agora`; o segundo o exige (`chamados/services/vigia_fio.py:266`, `chamados/services/vigia_fio.py:282`, `chamados/services/vigia_fio.py:487`). |

**Lavrador**

- **Salto 0:** `handle → timezone.now()` — leitura da borda (`folha/management/commands/lavrar_retrato_folha.py:49`).
- **Salto 3:** `handle → contadores → linhas_do_retrato → recortar`. `recortar` aceita `hoje=None`, e `linhas_do_retrato` o omite. Aqui só há eixo `f` ou nenhum grupo; não se consulta o leitor de dono (`folha/management/commands/lavrar_retrato_folha.py:68`, `folha/retrato_folha.py:120`, `folha/retrato_folha.py:121`, `folha/retrato_folha.py:111`, `folha/retrato_folha.py:635`, `folha/retrato_folha.py:632`).
- **Salto 4:** `handle → montar_linhas → _informativo_rescisao → informativo_rescisao → _dias_sem_decisao`. Contém `_tz.localdate()`, mas **o fallback está suprimido**: o dia da borda atravessa todas essas chamadas (`folha/management/commands/lavrar_retrato_folha.py:65`, `folha/retrato_folha.py:1220`, `folha/retrato_folha.py:1288`, `folha/export.py:1228`, `folha/export.py:1238`).
- **Salto 5:** `handle → montar_linhas → fora_da_folha → dias_que_seguram → classificar_export → motivos_retencao_espelho`. A última chamada **omite `hoje`**, cujo default é `None`. `motivos_retencao_celula` fica no salto 6; seu `_dias_sem_decisao`, com relógio, fica no **salto 7**, fora deste teto (`folha/management/commands/lavrar_retrato_folha.py:65`, `folha/retrato_folha.py:1214`, `folha/fora_da_folha.py:200`, `folha/fora_da_folha.py:147`, `folha/export.py:968`, `folha/export.py:1301`, `folha/export.py:1307`, `folha/export.py:1212`).
- **Salto 6:** `handle → montar_linhas → fora_da_folha → _carga → carga_da_tarefa → CargaDaTarefa.__init__ → qs_na_fila`. Contém `timezone.now()`, **suprimido pelo `agora` propagado**. O construtor é chamado pela classe devolvida pela fábrica local (`folha/management/commands/lavrar_retrato_folha.py:65`, `folha/retrato_folha.py:1214`, `folha/fora_da_folha.py:205`, `folha/fora_da_folha.py:67`, `chamados/services/tarefa_do_dia.py:730`, `chamados/services/tarefa_do_dia.py:452`, `chamados/services/painel.py:190`).

**Vigia do retido calado**

- **Salto 0:** `handle → timezone.localdate()` — leitura da borda (`folha/management/commands/vigiar_retido_calado.py:33`).
- **Salto 2:** `handle → censo → colabs_nunca_bateu`. Contém `timezone.localdate()`, **suprimido pelo `hoje` posicional recebido** (`folha/management/commands/vigiar_retido_calado.py:33`, `folha/vigia_retido_calado.py:60`, `escala/services/cadastro_realidade.py:161`).
- **Salto 3:** `handle → censo → linhas_do_retrato → recortar`. **Omite `hoje=None`**, no mesmo caminho restrito ao eixo `f` descrito acima (`folha/management/commands/vigiar_retido_calado.py:33`, `folha/vigia_retido_calado.py:65`, `folha/retrato_folha.py:111`, `folha/retrato_folha.py:635`).

**Recorte e suas três views**

- `_recorte_da_barra → timezone.now()` é a leitura direta. `retrato_folha`, `retrato_pdf` e `retrato_raiox` chegam a ela por suas chamadas ao recorte (`folha/views_retrato.py:177`, `folha/views_retrato.py:258`, `folha/views_retrato.py:386`, `folha/views_retrato.py:487`).
- `_recorte_da_barra → linhas_do_retrato → recortar` **omite `hoje=None`**, no salto 2 do recorte e no salto 3 de cada view. Novamente, não consulta o eixo de dono nesse caminho (`folha/views_retrato.py:198`, `folha/retrato_folha.py:111`, `folha/retrato_folha.py:632`, `folha/retrato_folha.py:635`).
- `retrato_pdf → gerar_pdf_auditavel → timezone.now()` acrescenta uma **leitura própria**, no salto 1. A view já recebeu o dia do recorte; o instante completo não veio no retorno (`folha/views_retrato.py:386`, `folha/views_retrato.py:441`, `folha/views_retrato.py:245`, `relatorios/pdf_base.py:133`).
- `retrato_raiox → placar_aceite` acrescenta **duas leituras próprias**, `_tz.now()` e `_tz.localdate()`, no salto 1, quando `dono == ['colab']`. O import de `placar_aceite` é local (`folha/views_retrato.py:511`, `folha/views_retrato.py:512`, `folha/views_retrato.py:513`, `chamados/services/verbos.py:286`, `chamados/services/verbos.py:287`).
- `frescor_dos_retratos` não acrescenta leitura autônoma nesse fecho: converte `agora`, passa data explícita às janelas e formata carimbos explícitos (`folha/retrato_folha.py:1408`, `folha/retrato_folha.py:1411`, `folha/retrato_folha.py:1441`, `folha/retrato_folha.py:1444`, `ponto/janelas.py:60`, `ponto/janelas.py:66`).

**Família compartilhada por calendário, drawer e relatório**

Use estes prefixos para as cadeias da tabela:

- **K:** `calendario_ponto → contexto_calendario`, passando `agora` (`colaboradores/views.py:1952`).
- **D:** `contexto_drawer → contexto_calendario`, passando `agora` (`colaboradores/services/drawer.py:123`, `colaboradores/services/drawer.py:124`).
- **F:** `furos_espelho → dataset_furos → contexto_calendario`, passando `agora` nas duas arestas (`relatorios/views.py:1543`, `relatorios/furos.py:160`, `relatorios/furos.py:164`).

| Cadeia depois do prefixo | Salto K/D; F | Leitura ou omissão |
|---|---:|---|
| `contexto_calendario` | 1; 2 | `timezone.now()` **suprimido** pelo instante recebido (`colaboradores/services/calendario.py:192`). |
| `contexto_calendario → status_dos_dias_lavrados` | 2; 3 | `now()` **suprimido**; recebe `_agora` (`colaboradores/services/calendario.py:638`, `ponto/status_dia.py:252`, `ponto/status_dia.py:253`). |
| `contexto_calendario → pergunta_viva` | 2; 3 | Import local com apelido `_pv`; **omite `hoje=None`**, mas fornece `agora`, do qual o juiz deriva o dia (`colaboradores/services/calendario.py:498`, `colaboradores/services/calendario.py:506`, `chamados/juizes.py:865`, `chamados/juizes.py:888`). |
| `… → pergunta_viva → _pergunta_viva_calc → _dentro_da_janela_viva` | 4; 5 | `date.today()` **suprimido** pelo dia derivado do instante (`chamados/juizes.py:897`, `chamados/juizes.py:1468`, `chamados/juizes.py:595`). |
| `… → pergunta_viva → _pergunta_viva_calc → classificar_falta` | 4; 5 | Import local `_cf`; `_tz.now()` **suprimido** pelo `agora` recebido (`chamados/juizes.py:1470`, `chamados/juizes.py:1472`, `chamados/juizes.py:1474`, `escala/servico_jornada.py:387`). |
| `contexto_calendario → folha_manda → turno_aberto_de` | 3; 4 | `now()` **suprimido** pelo instante recebido (`colaboradores/services/calendario.py:614`, `relatorios/cartao_pela_celula.py:398`, `ponto/turnos.py:2291`). |
| `contexto_calendario → folha_manda → ate_ontem` | 3; 4 | `localdate()` **suprimido** pelo dia fornecido (`colaboradores/services/calendario.py:614`, `relatorios/cartao_pela_celula.py:413`, `relatorios/cartao_pela_celula.py:40`). |
| `contexto_calendario → folha_manda → totais_da_folha → fechamento_lido → recalcular_fechamento_mes` | 5; 6 | **Leituras próprias** em `timezone.now()` nas linhas 616 e 1086; o instante da borda não é transportado até o recálculo (`colaboradores/services/calendario.py:614`, `relatorios/cartao_pela_celula.py:323`, `relatorios/cartao_pela_celula.py:73`, `ponto/services/fechamento.py:1759`, `ponto/services/fechamento.py:616`, `ponto/services/fechamento.py:1086`). |
| `… → recalcular_fechamento_mes → motivos_retencao_espelho` | 6; 7, fora | Import local `_mre`; **omite `hoje=None`** (`ponto/services/fechamento.py:1066`, `ponto/services/fechamento.py:1067`, `folha/export.py:1301`). |
| `contexto_calendario → CargaDaJanela.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado` | 6; 7, fora | **Leitura própria de `date.today()`**, quando existe tipo de escala; inclui despacho pela instância `_carga` (`colaboradores/services/calendario.py:252`, `ponto/turnos.py:3036`, `ponto/turnos.py:1939`, `ponto/turnos.py:1633`, `ponto/juiz_batida.py:270`, `ponto/juiz_batida.py:117`, `ponto/juiz_batida.py:125`). |

Duas condições delimitam essa tabela:

- `folha_manda → totais_da_folha` exige competência inteira e fechamento existente. `recalcular_fechamento_mes` também contém relógios nas linhas 1143, 1161 e 1189, mas esses ramos de escrita ficam **suprimidos** por `somente_leitura=True` nesse caminho (`relatorios/cartao_pela_celula.py:321`, `relatorios/cartao_pela_celula.py:71`, `ponto/services/fechamento.py:1761`, `ponto/services/fechamento.py:1128`, `ponto/services/fechamento.py:1143`, `ponto/services/fechamento.py:1161`, `ponto/services/fechamento.py:1189`).
- Pelo prefixo **F**, os caminhos através de `folha_manda` são apenas alcançabilidade **sintática**: `NAO_PERGUNTADO` impede a execução desse bloco (`relatorios/furos.py:164`, `colaboradores/services/calendario.py:594`, `colaboradores/services/calendario.py:597`, `colaboradores/services/calendario.py:614`).

**Ramos adicionais do drawer e do relatório**

- `contexto_drawer → perguntas_vivas_de → pergunta_viva`: import local `_vivas_de` de `chamados.models`, reexportado de `chamados.juizes`. **Omite `hoje` e `agora`**, apesar de o drawer já possuir ambos (`colaboradores/services/drawer.py:21`, `colaboradores/services/drawer.py:79`, `colaboradores/services/drawer.py:80`, `chamados/models.py:4226`, `chamados/juizes.py:1504`, `chamados/juizes.py:865`).
- Esse ramo chega a `_dentro_da_janela_viva` e `classificar_falta` no **salto 4**, permitindo suas leituras próprias; pela perna `_pergunta_materializada_de_fato`, chega novamente aos dois no **salto 5** (`chamados/juizes.py:897`, `chamados/juizes.py:1468`, `chamados/juizes.py:1472`, `chamados/juizes.py:1486`, `chamados/juizes.py:696`, `chamados/juizes.py:701`, `chamados/juizes.py:595`, `escala/servico_jornada.py:387`).
- Na mesma cadeia, `_pergunta_materializada_de_fato` **omite efetivamente `agora`** no salto 4, e `_slot_esperado` o omite no salto 5: os `**` condicionais viram `{}` quando o instante é `None`. As assinaturas têm default `None` (`chamados/juizes.py:1486`, `chamados/juizes.py:1487`, `chamados/juizes.py:665`, `chamados/juizes.py:755`, `chamados/juizes.py:344`).
- `contexto_drawer → turnos_do_colab → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado`: **`date.today()` próprio no salto 5**. O import local é `_tdc` (`colaboradores/services/drawer.py:44`, `colaboradores/services/drawer.py:47`, `ponto/turnos.py:1915`, `ponto/turnos.py:1939`, `ponto/turnos.py:1633`, `ponto/juiz_batida.py:270`, `ponto/juiz_batida.py:125`).
- `furos_espelho → escalas_em_limbo`: **`timezone.localdate()` próprio no salto 1**, embora `_agora` já exista na borda (`relatorios/views.py:1519`, `relatorios/views.py:1554`, `relatorios/furos.py:59`).
- `furos_espelho → dataset_furos → status_dos_dias_lavrados`: contém `now()` no **salto 2**, **suprimido** pelo instante recebido. O default `agora=None` de `dataset_furos` permanece, mas essa borda fornece o argumento (`relatorios/views.py:1543`, `relatorios/furos.py:103`, `relatorios/furos.py:123`, `ponto/status_dia.py:253`).

**Complemento: módulo real `chamados/services/vigia_fio.py`**

Para tornar as cadeias seguintes explícitas:

- **T(C/A):** `censo:359` ou `censo_alcance:593 → CargaDaTarefa.tarefa → resolver_dia_oferecido_nas_datas → contexto_calendario` (`chamados/services/vigia_fio.py:359`, `chamados/services/vigia_fio.py:593`, `chamados/services/tarefa_do_dia.py:613`, `colaboradores/services/calendario.py:157`).
- **L(C):** `censo → dono_fora_do_fio → dia_do_admin → _carregar → cadastro_realidade.lista` (`chamados/services/vigia_fio.py:387`, `chamados/services/vigia_fio.py:261`, `chamados/services/vigia_fio.py:202`, `chamados/services/vigia_fio.py:194`).
- **L(A):** `censo_alcance → dia_na_lista → _carregar → cadastro_realidade.lista` (`chamados/services/vigia_fio.py:621`, `chamados/services/vigia_fio.py:210`, `chamados/services/vigia_fio.py:194`).
- **P(C/A):** `censo:387` ou `censo_alcance:615 → dono_fora_do_fio → PerguntaDisputa.estado → pergunta_viva`. A propriedade **omite ambos os argumentos**; acesso à propriedade conta como chamada (`chamados/services/vigia_fio.py:387`, `chamados/services/vigia_fio.py:615`, `chamados/services/vigia_fio.py:259`, `chamados/models.py:3127`, `chamados/juizes.py:865`).

| Cadeia | Saltos C / A | Leitura e transporte do instante |
|---|---:|---|
| `censo → now()` | 0 / — | Fallback da própria borda, quando não recebeu `agora` (`chamados/services/vigia_fio.py:282`). |
| `censo/censo_alcance → carga_da_tarefa → CargaDaTarefa.__init__ → qs_na_fila` | 3 / 3 | `now()` **suprimido** pelo instante propagado (`chamados/services/vigia_fio.py:357`, `chamados/services/vigia_fio.py:592`, `chamados/services/tarefa_do_dia.py:730`, `chamados/services/tarefa_do_dia.py:452`, `chamados/services/painel.py:190`). |
| `T(C/A)` | 3 / 3 | `contexto_calendario:192`, fallback suprimido; o instante atravessa a função plural (`chamados/services/tarefa_do_dia.py:614`, `colaboradores/services/calendario.py:159`, `colaboradores/services/calendario.py:192`). |
| `T(C/A) → status_dos_dias_lavrados` | 4 / 4 | `now()` suprimido (`colaboradores/services/calendario.py:638`, `ponto/status_dia.py:253`). |
| `T(C/A) → folha_manda → turno_aberto_de / ate_ontem` | 5 / 5 | `now()` e `localdate()` suprimidos (`colaboradores/services/calendario.py:614`, `relatorios/cartao_pela_celula.py:398`, `relatorios/cartao_pela_celula.py:413`, `ponto/turnos.py:2291`, `relatorios/cartao_pela_celula.py:40`). |
| `… → tarefa → ler → alimentar_fila → particionar_verbos / qs_na_fila` | 4 / 4 | Função local `ler`; ambos os relógios suprimidos pelo instante recebido (`chamados/services/tarefa_do_dia.py:600`, `chamados/services/tarefa_do_dia.py:588`, `chamados/services/tarefa_do_dia.py:590`, `chamados/juizes.py:1573`, `chamados/juizes.py:1576`, `chamados/juizes.py:1584`, `chamados/services/verbos.py:35`, `chamados/services/painel.py:190`). |
| `L(C/A)` | 4 / 3 | `lista → now()`, suprimido pelo `self.agora` fornecido pelos censos (`chamados/services/vigia_fio.py:194`, `escala/services/cadastro_realidade.py:780`). |
| `L(C/A) → _jornada_que_mente` | 5 / 4 | **`date.today()` próprio**; `lista` passa somente empresas (`escala/services/cadastro_realidade.py:791`, `escala/services/cadastro_realidade.py:224`). |
| `L(C/A) → _afastado_sem_registro` | 5 / 4 | `localdate()` suprimido pelo `hoje` passado (`escala/services/cadastro_realidade.py:808`, `escala/services/cadastro_realidade.py:588`). |
| `L(C/A) → _afastado_sem_registro → afastados_hoje` | 6 / 5 | **`localdate()` próprio**: a chamada não fornece o dia (`escala/services/cadastro_realidade.py:592`, `colaboradores/services/situacao_exibida.py:45`). |
| `L(C/A) → _nunca_bateu → colabs_nunca_bateu` | 6 / 5 | `localdate()` suprimido pelo `hoje` propagado (`escala/services/cadastro_realidade.py:864`, `escala/services/cadastro_realidade.py:179`, `escala/services/cadastro_realidade.py:161`). |
| `L(A) → _jornada_que_mente → periodos_do_dia → intervalo_cadastrado` | 7, fora / 6 | **`date.today()` próprio**, quando existe tipo de escala (`escala/services/cadastro_realidade.py:238`, `ponto/juiz_batida.py:270`, `ponto/juiz_batida.py:125`). |
| `P(C/A) → _pergunta_viva_calc → _dentro_da_janela_viva / classificar_falta` | 5 / 5 | **Leituras próprias**: a propriedade perdeu o instante que os censos possuem (`chamados/models.py:3127`, `chamados/juizes.py:897`, `chamados/juizes.py:1468`, `chamados/juizes.py:1472`, `chamados/juizes.py:595`, `escala/servico_jornada.py:387`). |
| `P(C/A) → _pergunta_viva_calc → _pergunta_materializada_de_fato →` esses dois leitores | 6 / 6 | Mesmas leituras próprias; `agora` é omitido pela expansão condicional (`chamados/juizes.py:1486`, `chamados/juizes.py:1487`, `chamados/juizes.py:696`, `chamados/juizes.py:701`). |

Ainda no fecho desses censos:

- `censo → vigia_carimbo.classificar → _pergunta_materializada_de_fato`: **omite `hoje` e `agora` no salto 2**. O import local `_juiz` passa por reexportação de `chamados.models`. Seus leitores `_dentro_da_janela_viva` e `classificar_falta` ficam no salto 3 e podem ler relógio próprio (`chamados/services/vigia_fio.py:415`, `chamados/services/vigia_carimbo.py:95`, `chamados/services/vigia_carimbo.py:106`, `chamados/models.py:4226`, `chamados/juizes.py:665`, `chamados/juizes.py:696`, `chamados/juizes.py:701`).
- `censo → classificar → _slot_esperado`: **omite `agora` no salto 2**; também existe a omissão condicional pela perna `_pergunta_materializada_de_fato → _slot_esperado` (`chamados/services/vigia_fio.py:415`, `chamados/services/vigia_carimbo.py:110`, `chamados/juizes.py:344`, `chamados/juizes.py:755`).
- `censo_alcance → dias_que_seguram → classificar_export → motivos_retencao_espelho`: **omite `hoje` no salto 3**. Seguindo `motivos_retencao_celula → _dias_sem_decisao`, alcança **`localdate()` próprio no salto 5**. Pela outra perna, `_dias_que_retem`, o mesmo leitor fica no salto 4, mas com fallback suprimido (`chamados/services/vigia_fio.py:537`, `folha/fora_da_folha.py:147`, `folha/export.py:968`, `folha/export.py:1307`, `folha/export.py:1212`, `folha/export.py:1238`, `folha/fora_da_folha.py:165`, `folha/fora_da_folha.py:121`).
- `censo_alcance → alimentar_portas → alimentar_fila → particionar_verbos / qs_na_fila`: relógios no **salto 3**, suprimidos pelo instante propagado (`chamados/services/vigia_fio.py:560`, `chamados/juizes.py:1619`, `chamados/juizes.py:1573`, `chamados/juizes.py:1576`, `chamados/services/verbos.py:35`, `chamados/services/painel.py:190`).
- `censo_alcance → alimentar_portas → pergunta_viva`: **omite `hoje` no salto 2**, mas fornece `agora`; seus leitores ficam no salto 4 e têm fallbacks suprimidos (`chamados/services/vigia_fio.py:560`, `chamados/juizes.py:1628`, `chamados/juizes.py:888`, `chamados/juizes.py:1468`, `chamados/juizes.py:1472`).
- `T(C/A) → pergunta_viva` **omite `hoje` no salto 4**, fornecendo o instante. `… → tarefa → _ler_vida → pergunta_viva` fornece ambos. As expansões condicionais posteriores fornecem `agora` nesses caminhos (`colaboradores/services/calendario.py:506`, `chamados/services/tarefa_do_dia.py:574`, `chamados/services/tarefa_do_dia.py:537`, `chamados/services/tarefa_do_dia.py:541`, `chamados/juizes.py:1487`, `chamados/juizes.py:755`).
- `censo/censo_alcance → tarefa → _ler_vida → carga_da_vida → CargaDaVida.__init__ → _slot_esperado`: **omite `agora` no salto 5**, mas passa `carga=self`; o juiz recupera o instante dessa carga (`chamados/services/vigia_fio.py:359`, `chamados/servic

[truncado em 20000 de 35668 caracteres]

_5745 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
