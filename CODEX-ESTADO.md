# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 10:30** · publicado em **17/11/2026 10:30** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 10:30 · sessao eed7ba59b0a8 · - · - · origem=Ronald (tmux)

A fila precisa ignorar o ato compensatório **antes de escolher a última decisão aplicável**: hoje `_pela_decisao` lê `decisoes[-1]`; F1 desconhecido cai em `ato_desconhecido`/`None`, e F2 `REABERTURA` cai em `devolvida_ao_colaborador`/`RESPONDER`. (`app/chamados/services/tarefa_do_dia.py:156–187`)

O filtro pode ficar no carregamento único da sequência da tarefa, preservando o horizonte `ato_em <= agora` e a decisão anterior; isso também evita encobrir `RETRATACAO`, que `_tarefa_pura` verifica pela última linha. Para F1 filtra o tipo novo; para F2 filtra apenas a `REABERTURA` identificada explicitamente como a ressuscitação automática. (`app/chamados/services/tarefa_do_dia.py:259–261`, `:421–426`)

| Última decisão aplicável anterior | Comportamento a conservar na tarefa |
|---|---|
| `VALIDACAO` | Sem lavra vigente: `nao_sei`; lavra `nao`: materializar/sistema; lavra `sim` com efeito: rejulgar/sistema; `sim` sem efeito: encerrada. (`app/chamados/services/tarefa_do_dia.py:167–175`) |
| `RESPOSTA` | Texto vazio: responder/colaborador; texto não vazio: validar/admin. (`app/chamados/services/tarefa_do_dia.py:159–162`) |
| `SILENCIO` | `silencio_decidido`, ações vazias. (`app/chamados/services/tarefa_do_dia.py:176–177`) |
| Nenhuma | `aguarda_resposta`, responder/colaborador. (`app/chamados/services/tarefa_do_dia.py:156–157`) |

A resposta legal — instante **e** texto — fica fora do universo normal de ressuscitar: há `materializada_em != NULL` e filtro `q_nao_respondida`; texto sem instante/validação também fica fora. Portanto a linha “respondida sem validação” exige separar resposta legal, que é **NO-OP**, do legado instante sem texto, que pode entrar. (`app/chamados/models.py:1542–1559`)

`DecisaoPergunta` não tem campo `origem`; há `ato_por` e `ato_motivo`. F2 precisa tornar concreta a distinção técnica escolhida, sem pressupor que `ato_por` humano significa evento humano; o código documenta automatismo herdando usuário de request em outro fluxo. (`app/chamados/models.py:4110–4123`; `app/chamados/juizes.py:1251–1253`)

Provas já existentes:

- `test_f1nova_b1_juiz_respondida.py:311–329` mede somente razão/ações da instância; `:331–335` compara antes/depois para sem decisão e `SILENCIO`; `:352–358` verifica segunda chamada sem duplicar atos. Não prova igualdade integral de `tarefa_do_dia` nas 2.025 unidades. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:311–358`)
- Controle de `REABERTURA` comum: continua devolvendo para responder/colaborador. (`app/chamados/tests/test_e2_tarefa_do_dia.py:849–865`)
- Teto de consultas constante entre lote pequeno e maior já tem selo. (`app/chamados/tests/test_e2_tarefa_do_dia.py:913–938`)
- A caracterização de validada ressuscitada afirma expressamente a divergência atual: projeção limpa; leitura mantém validação; estado `VALIDADA`; sai de `q_vivas`. O teste precisa virar RED da compensação, preservando também controle não ressuscitado. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:413–428`)

Executei apenas uma sonda em memória, `python3 -B`, extraindo AST de `_pela_decisao` e substituindo seus três imports por objetos simples: confirmou os ramos acima nos quatro casos, sem arquivos nem banco. Sua abrangência é local à função; igualdade de integração/main permanece `[nao verificado]`. (`app/chamados/services/tarefa_do_dia.py:142–187`)

A cópia não oferece `.git`; `git branch --all` falhou. Comparação real com main e sombra das 2.025 medidas: `[nao verificado]`.

## 17/11 10:29 · sessao c8e40a3bbf61 · - · - · origem=Ronald (tmux)

Achados de escritores e matriz:

- `ressuscitar_perguntas` grava somente o passivo R/V antes de limpar a projeção; não grava ato compensatório. O próprio código explica que evitou `REABERTURA` porque a fila a interpreta como devolução ao colaborador. `app/chamados/models.py:1633`, `app/chamados/models.py:1644`, `app/chamados/models.py:1651`.
- Respondida legal — instante E texto — sem validação não entra no universo normal: exige `q_nao_respondida()`. Validada com resposta legal também fica fora; validada sem resposta pode entrar. A resposta que chega entre a seleção do universo e a trava é corrida explicitamente reconhecida no comentário. `app/chamados/models.py:1542`, `app/chamados/models.py:1545`, `app/chamados/models.py:1642`.
- “Sem ato” não equivale a qualquer pendente: precisa de `materializada_em` preenchida, passar pelos filtros de via, marco apagado e C6 na cópia limpa. `app/chamados/models.py:1543`, `app/chamados/models.py:1547`, `app/chamados/models.py:1569`, `app/chamados/models.py:1572`.

**Armadilha decisiva de F2:** já existe uma `REABERTURA` automática por usuário sistema que deliberadamente devolve ao colaborador: ausência rejeitada. O motivo diz “voltam ao colaborador”, e a escrita passa `por=get_user_sistema()`. Portanto, ignorar toda `REABERTURA` cujo autor seja sistema quebra comportamento existente. `app/chamados/services/materializacao.py:1158`, `app/chamados/services/materializacao.py:1167`.

`DecisaoPergunta` não possui campo `origem`; tem `ato_por` e `ato_motivo`. Autor nulo também já é aceito em reabertura legítima: command de cura chama `reabrir_para_correcao` sem `por`. Assim, F2 exige identificação nominal inequívoca da ressuscitação; autor sistema/nulo isoladamente não basta. `app/chamados/models.py:4110`, `app/chamados/models.py:4113`, `app/chamados/models.py:4115`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`.

Escritores automáticos encontrados:

| Escritor | Ato/autoria |
|---|---|
| `responder_automaticamente` | `RESPOSTA`, sem autor, motivo fixo “resposta automatica…”; chamado no upload de foto e seguido de validação pelo dono. `app/chamados/services/disputa_emissao.py:1678`; `app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`. |
| `carimbar_resposta_pela_trilha` | `RESPOSTA`, sem autor, motivo “pela trilha…”, instante testemunhado pela nota; command de cura chama essa porta. `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/services/disputa_emissao.py:1699`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`. |
| `linhas_da_projecao_sem_linha` | Reconstitui `RESPOSTA` sem autor e `VALIDACAO` preservando `validada_por_id`; motivo fixo de passivo. `app/chamados/models.py:4175`; `app/chamados/models.py:4179`. |
| Nascimento / ressuscitação / backfill fio morto | Registram as linhas da porta de passivo. `app/chamados/models.py:3160`; `app/chamados/models.py:1633`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`. |
| Fechar disputa por desligamento/admin | `SILENCIO`, autor recebido pelo fechamento; usa resposta vigente da sequência. `app/chamados/models.py:2029`; `app/chamados/models.py:2036`. |
| Fechar fio do dia | `SILENCIO`, autor herdado do ato de decidir o dia. `app/ponto/services/veredito_celula.py:190`; `app/ponto/services/veredito_celula.py:195`. |
| Backfill de admin decidiu | `SILENCIO`, instante e autor originais do carimbo, motivo identificando backfill. `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`. |
| Remover motivo | `RETRATACAO`, autor recebido, texto vigente; carimba identidade retratada. `app/chamados/models.py:2166`; `app/chamados/models.py:2168`; `app/chamados/models.py:2173`. |
| Absorver irmãs pela ausência | `ABSORCAO`, autor herdado da declaração; liga escape e ausência. `app/chamados/services/materializacao.py:1046`; `app/chamados/services/materializacao.py:1048`. |
| Devolver validação recusada pela materialização | `DEVOLUCAO`, preserva resposta, autor herdado da chamada e motivo da recusa. `app/chamados/services/validacao.py:269`; `app/chamados/services/validacao.py:279`. |
| Ausência rejeitada | `REABERTURA`, usuário sistema, com finalidade explícita de devolver ao colaborador. `app/chamados/services/materializacao.py:1158`; `app/chamados/services/materializacao.py:1167`; `app/chamados/services/disputa_emissao.py:1631`. |
| Command `validar_classe_a` | Usa o mesmo dono de `VALIDACAO`, com autor nominal passado por argumento. `app/chamados/management/commands/validar_classe_a.py:89`; `app/chamados/management/commands/validar_classe_a.py:100`; `app/chamados/services/validacao.py:173`. |

Confusão humano/sistema:

- A fila não lê autoria: qualquer `RECUSA`/`REABERTURA` última é `devolvida_ao_colaborador`; qualquer `DEVOLUCAO` última é materialização recusada para admin. `app/chamados/services/tarefa_do_dia.py:163`, `app/chamados/services/tarefa_do_dia.py:165`.
- `leitura` e a face SQL distinguem resposta humana por `ato_motivo` vazio, não por autoria. Automáticas e passivo usam motivo não vazio. `app/chamados/models.py:4195`, `app/chamados/models.py:4205`, `app/chamados/models.py:3500`.
- O histórico exibe o rótulo do ato, autor quando presente e motivo quando presente; uma `REABERTURA` automática continua com o rótulo genérico “Reaberta para correcao”. `app/templates/chamados/partials/modal_fio.html:212`, `app/chamados/models.py:4103`.
- O juiz de toque humano na tranca consulta `LogAuditoria`, não `DecisaoPergunta`; além da autoria, exige assinatura de ação humana no caso de resolver dia. O ato novo isolado não é toque humano nesse leitor. `app/chamados/juizes.py:1241`, `app/chamados/juizes.py:1250`, `app/chamados/juizes.py:1263`.

Provas existentes aproveitáveis:

- Defeito validada ressuscitada já tem caracterização que afirma projeção limpa, leitura ainda validada e exclusão de `q_vivas`; deve inverter como RED do novo corte. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:413`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:423`.
- Controle atual de instante sem texto ressuscitado verifica projeção limpa, não respondida pelo ato, pendente e presente no SQL largo e nas ancoradas. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:388`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:399`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:410`.
- Comparação da fila antes/depois existe para sem decisão e último `SILENCIO`, com relógio fixado; compara razão e ações. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:311`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:331`.
- O teste antigo também proíbe `REABERTURA`/`RECUSA` e exige somente fotos R/V; esse contrato de teste precisará ser alinhado à exceção nominal do novo aval, se F2 for escolhida. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:270`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:280`.

## 17/11 10:29 · sessao cec9f1117055 · - · - · origem=Ronald (tmux)

Achados só por leitura:

- A coexistência “não validada pelo ato E viva pelo C6” com fila conservada é possível como desenho: C6 lê a projeção (`app/chamados/juizes.py:1389`, `:1404`), enquanto tarefa lê a última decisão (`app/chamados/services/tarefa_do_dia.py:158`). A porta já inclui somente perguntas que C6 considera vivas numa cópia com V/R/T zerados e carimbo retirado (`app/chamados/models.py:1567`, `:1572`, `:1576`).
- Nenhuma forma existente implementa essa conservação genericamente: REABERTURA/RECUSA manda responder o colaborador (`app/chamados/services/tarefa_do_dia.py:163`); DEVOLUCAO manda validar o admin (`:165`); tipo novo cai em `ato_desconhecido`/`None` (`:187`). A carga lê todos os atos, sem filtro por origem (`:421`–`:424`).
- Portanto, “REABERTURA de origem sistema que o leitor distingue” demanda alteração desse leitor: atualmente ele não olha ator/motivo ao decidir REABERTURA (`app/chamados/services/tarefa_do_dia.py:158`, `:163`). Não existe campo próprio de origem; existem ator e motivo (`app/chamados/models.py:4113`, `:4115`).
- Há tensão literal com a proibição de mudar `tarefa_do_dia` do COMPLEMENTO-1 fornecido: o novo item 4 explicita exceção somente para ato novo [nao verificado]. Para preservar fila com o novo ato, a exceção relativa ao leitor deveria ser explicitada; nenhuma implementação pronta disso está na cópia (`app/chamados/services/tarefa_do_dia.py:163`, `:187`, `:421`).
- “Respondida sem validação” precisa separar resposta legal dos pares ilegais: resposta legal fica fora do universo por `q_nao_respondida` (`app/chamados/models.py:1542`, `:1545`); texto sem instante e sem validação também é excluído (`:1558`). O caso instante sem texto é exercitado pelo controle existente (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:388`, `:396`).
- Os testes existentes de fila comparam antes/depois da execução real de ressuscitar para sem decisão e SILENCIO (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319`, `:326`, `:329`, `:331`, `:334`). Esse é o formato necessário para provar o evento. Comparar main/árvore sobre registros ainda não ressuscitados pode dar diff zero sem exercitar o ato novo; não demonstra, sozinho, conservação após a transição [nao verificado].
- A caracterização de validada ressuscitada confirma V projetada nula, V do ato preservada e exclusão de `q_vivas` (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:423`, `:425`, `:426`, `:428`). A própria anotação “0 ressuscitáveis com validação” do teste não é medida comprovada por esse código (`:417`–`:418`).
- `fio.py:255` declara literalmente a continência e usa `q_vivas` antes de C6 nas disputas fechadas (`app/chamados/services/fio.py:255`, `:256`, `:269`). A implementação de `q_vivas` exige não validada pelo ato (`app/chamados/models.py:3562`), enquanto C6 usa V projetada (`app/chamados/juizes.py:1404`–`:1406`).

## 17/11 10:29 · sessao 7d69f8e67677 · - · - · origem=Ronald (tmux)

Achados para consolidação, só leitura:

- Premissa central confirmada: ressuscitar registra apenas fotos de R/V e limpa V/R/texto na projeção, sem limpador na sequência (`app/chamados/models.py:1633`, `app/chamados/models.py:1651`). A leitura conserva a VALIDACAO até ato que a limpe (`app/chamados/models.py:4191`, `app/chamados/models.py:4233`). O teste caracteriza exatamente projeção nula, leitura validada e exclusão de q_vivas (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:423`).
- Vigência real citável: L-130 documenta par inteiro zerado, trilha anterior, ausência de devolução e lei RESSUSCITAR-DEVOLVE-AO-COLABORADOR pendente (`app/docs/LEIS.md:151`). “MANTEM A FILA DE HOJE” está escrito no código, com motivo para evitar REABERTURA comum (`app/chamados/models.py:1644`). O número 97 ali é relato em comentário, sem medida independente disponível [nao verificado].
- A exceção **nominal** do item 4 do anexo fornecido resolve explicitamente a proibição anterior de ato novo; essa autorização é texto recebido no prompt, não arquivo localizado [nao verificado]. Não resolve automaticamente a conservação de todos os leitores nem a guarda exportada: `app/docs/CORTES.md:487`.
- “Não validada pelo ato E viva pelo C6” não contradiz, por si só, ausência de ação: a tarefa admite SILENCIO → `[]`, independentemente de V/R da leitura (`app/chamados/services/tarefa_do_dia.py:176`); vida C6 lê a projeção (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1404`). Para conservar todas as saídas da tarefa, ato novo desconhecido não serve sem adaptação: cai em `ato_desconhecido`, enquanto REABERTURA comum devolve ao colaborador (`app/chamados/services/tarefa_do_dia.py:163`, `app/chamados/services/tarefa_do_dia.py:187`).
- Conservação da fila envolve mais que ignorar o novo ato em `_pela_decisao`: a carga da tarefa também obtém pendentes do app e afirma depender da continência q_vivas/C6 (`app/chamados/services/tarefa_do_dia.py:524`). A análise do outro explorer deverá fechar esse contraexemplo.
- Impactos operacionais existem: a face filtra `_ancoradas` em qualquer competência (`app/chamados/models.py:343`, `app/chamados/models.py:357`); a leitura filtra respostas sem veredito (`app/chamados/models.py:1952`) que alimentam CSF (`app/ponto/supra_juiz.py:573`). Registrar acorda células (`app/chamados/models.py:4259`) emitindo EventoCelula (`app/chamados/models.py:3393`), embora ressuscitar já acordasse antes (`app/chamados/models.py:1653`). Dinheiro/exportada mudarem com o futuro ato: [nao verificado]; dinheiro/exportada permanecerem neutros com o futuro ato: [nao verificado].
- Guarda vigente explícita: supra_juiz em exportada não muda; diferença exige PAREI ! com tabela (`app/docs/CORTES.md:487`). Os zeros da entrega anterior não medem código futuro [nao verificado].
- Não encontrei os complementos RESPOSTAS-F2 como arquivos nesta cópia; não atribuir a eles números de linha fabricados [nao verificado].

## 17/11 10:29 · sessao 3cd7c9dcfe11 · - · - · origem=Ronald (tmux)

- **Limpeza e falta do ato compensatório conferem.** O escritor registra fotos RESPOSTA/VALIDACAO antes da limpeza (`app/chamados/models.py:1633`); limpa `validada_em`, `validada_por`, `respondida_em`, `resposta_colab` (`app/chamados/models.py:1651`); a porta também limpa o carimbo de materialização (`app/chamados/models.py:3397`). Não é literalmente “sem trilha”: existe auditoria genérica `perguntas_ressuscitadas`, mas sem ato de desfazimento em `DecisaoPergunta` (`app/chamados/models.py:1675`). As fotos preservam texto e validação históricos (`app/chamados/models.py:4175`, `app/chamados/models.py:4179`).

- **A validação fantasma é demonstrada pelo código.** `DecisaoPergunta.leitura` reconstitui VALIDACAO e só limpa quando encontra os atos declarados (`app/chamados/models.py:4233`, `app/chamados/models.py:4235`); os limpadores vigentes são DEVOLUCAO/RECUSA/REABERTURA, com RESPOSTA humana adicional para validação (`app/chamados/models.py:4191`, `app/chamados/models.py:4205`). A caracterização existente ressuscita uma validada, verifica projeção nula e leitura ainda validada, estado VALIDADA e exclusão de `q_vivas` (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:421`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:425`).

- **C6 lê projeção, e o universo inclui validada sem resposta.** O universo exige materialização e `q_nao_respondida`, não ausência de validação (`app/chamados/models.py:1542`); a face negada cobre ausência de instante ou instante sem texto (`app/chamados/juizes.py:1583`). A seleção copia a pergunta, limpa carimbo/V/R/texto e exige `pergunta_viva` nessa cópia (`app/chamados/models.py:1572`). O C6 lê V/R/texto projetados (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1404`) e mata primeiro pela validação projetada (`app/chamados/juizes.py:1405`).

- **A premissa da continência é literal.** `fio.py:255` declara `q_vivas` como pré-filtro contendo toda viva; a documentação do próprio filtro repete que contrato é continência, nunca igualdade (`app/chamados/services/fio.py:255`, `app/chamados/models.py:3531`). Contudo, o filtro passou a excluir validada pelo ato (`app/chamados/models.py:3562`), enquanto o C6 continua lendo a projeção (`app/chamados/juizes.py:1404`).

- **Há caminho real de produção, sem depender da medição.** Cartório chama a porta (`app/ponto/services/cartorio.py:1674`), reabrir chamado também (`app/chamados/models.py:1510`), e resgate pela tranca repete a chamada após auditoria (`app/chamados/views_arquivar.py:86`).

- **“2025 medidas” é dado fornecido, não medição reproduzida nesta cópia.** O anexo `opus_final.md`, item 3, atribui 2.025 a `r2/f2.r:14` e contrapõe inicialização de `Counter` em `remedida.md:455`; esses arquivos não estão nesta cópia [nao verificado]. O número de validadas dentro das 2.025 não foi dado [nao verificado]. A docstring do teste ainda afirma “0 ressuscitáveis com validação” citando a entrega, sem executar medição (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:417`); portanto isso é uma alegação documental, não prova de frota.

- **Ponto literal importante para item 4:** o universo atual exclui respostas legais completas pela face negada do juiz (`app/chamados/models.py:1545`, `app/chamados/juizes.py:1552`). A matriz de pré-desenho “respondida sem validação” precisa distinguir resposta legal completa — não ressuscitável pela porta atual — de instante sem texto ou estado só no ato (`app/chamados/juizes.py:1534`, `app/chamados/juizes.py:1552`).

## 17/11 10:24 · sessao 962e33240ae4 · E4C-F2 · censo · origem=Code

Referências aos anexos usam linhas contadas desde a primeira linha de cada bloco, incluindo linhas vazias. A classificação abaixo trata da cobertura do tema, sem decidir a procedência dos achados.

1. **Ressuscitada VALIDADA versus C6/fio — b: visto e descartado.**

   **Pergunta:** “algum deles agora DISCORDA de um leitor migrado” e “validada ressuscitada vira o que?” (`v1.q:26–28`, `v1.q:34–35`).

   **Resposta:** “Ressuscitar pergunta validada conserva justamente a validação que a porta apaga”; também aponta que “A pergunta excluída não chega ao juiz”, citando o fio (`v1.r:5–7`, `v1.r:38–42`).

   **Triagem:** “#1, #4 ressuscitar -> DESCARTADO para a F2”. Motivo declarado: pergunta de desenho, ato novo proibido e alteração de RESSUSCITAR fora da F2; a volta 2 acrescenta “frota 0, caracterizado” (`entrega.md:68`, `entrega.md:80`).

2. **Caminhos reais que chamam ressuscitar — a: tema perguntado, ligação com esses chamadores não apresentada.**

   **Pergunta:** pede “QUEM ESCREVE (todo escritor)” e pergunta “Ha outro caminho com o mesmo formato?” (`v1.q:13–17`, `v1.q:34–35`).

   **Resposta:** apresenta o contraexemplo dentro da ressuscitação e identifica outro reset, `devolver_instante_sem_texto_l130`; esses trechos não apresentam a ligação cartório → ressuscitação, desarquivamento → ressuscitação ou reabertura → ressuscitação (`v1.r:7–11`). Esses chamadores estão na cópia (`app/ponto/services/cartorio.py:1674`, `app/chamados/views_arquivar.py:86`, `app/chamados/models.py:1510`).

   **Triagem correspondente ao tema:** descarta ressuscitar como desenho fora da F2; não apresenta análise desses três chamadores nessa justificativa (`entrega.md:68`, `entrega.md:80`). **A classificação a se limita à rastreabilidade dos chamadores; o mecanismo já havia sido visto no item 1.**

3. **“Ressuscitáveis = 0” e medida de produção — c: dado de produção fora do alcance demonstrado.**

   **Pergunta:** exige conservação “mesmo valor por pk antes e depois”, mas esse trecho não fornece a medição de ressuscitáveis (`v1.q:7–8`).

   **Resposta:** declara expressamente “incidência em produção, contagens mencionadas em comentários [...] [nao verificado]” (`v1.r:3`).

   **Triagem:** usa “ressuscitaveis_agora = 0 (remedida.md:455)” e repete “frota 0” nos descartes (`entrega.md:7–8`, `entrega.md:80`, `entrega.md:104–105`). A contraposição com Counter e 2.025 ressuscitáveis consta do Opus; validação independente dessas medidas: **[nao verificado]** (`opus_final.md:4`).

4. **N+1 no app/API — b: visto e descartado.**

   **Pergunta:** pede explicitamente “custo N+1” (`v1.q:33`).

   **Resposta:** “Há N+1 novo em caminhos sem prefetch”; descreve o carregamento do app, `_item`, `estado`, `resposta_display` e “duas buscas adicionais de decisões por pergunta” (`v1.r:71–73`).

   **Triagem:** “#6 N+1 -> DESCARTADO (fila): medido; o custo real que apareceu foi o do JIT, curado”. Esse é o motivo declarado do descarte; a resposta separava contagem executada e latência como não verificadas (`entrega.md:77`, `v1.r:77`).

5. **`ato_motivo` com dois significados e sem protocolo imposto — b: visto e descartado.**

   **Pergunta:** “Algum leitor de `ato_motivo` supoe outro significado (campo com dois significados)?” (`v2.q:17–20`).

   **Resposta:** “`ato_motivo` acumula explicação e controle de efeito”; acrescenta que “O campo permanece texto livre e `registrar` não valida esse protocolo” (`v2.r:26`).

   **Triagem:** “DESCARTADO (fila)”. Motivo declarado: atualmente motivo vazio só vem da porta humana; selar o vocabulário ficou para a fase de remoção da projeção (`entrega.md:81–82`).

6. **L-130: reset inteiro versus leitura pelo ato — b: visto no tema, descartado junto com ressuscitar.**

   **Pergunta:** pede comparar cada reset da projeção com a leitura do ato e procurar outro caminho semelhante à ressuscitação (`v1.q:15–17`, `v1.q:34–35`).

   **Resposta:** identifica `devolver_instante_sem_texto_l130`: “fotografa a resposta e zera o par sem ato limpador”, mas “permanece divergência do instante bruto” (`v1.r:11`). Isso toca diretamente o tema do reset que não se reproduz integralmente na leitura; o trecho não formula uma conclusão nominal sobre `LEIS.md:151` (`v1.r:11`).

   **Triagem temática:** ressuscitar foi descartado como pergunta de desenho; o registro R2 invoca o controle “zera o par, nao volta”. Não há nesses trechos um descarte nominal separado chamado L-130 (`entrega.md:68`, `entrega.md:102–103`). A afirmação sobre CSF exportada com zero diferenças aparece separadamente na conservação (`entrega.md:60`).

CLASSES: a=1 b=4 c=1 d=0 e=0

## 17/11 10:15 · sessao f3a5c41825e1 · CONTADOR-CODEX-OCIOSO · verificador · origem=Code

A volta 3 ainda reprova: persistem divergências temporais, entrada inconsistente pode produzir `ok`, uma sessão pode derrubar a formatação do ESTADO e três mutantes recebem selo por diferenças de descrição/classe, preservando `SEM DADO`. (`bin/disjuntor.py:2036`, `bin/disjuntor.py:2132`, `bin/codex_limite_estado.py:213`, `bin/codex_limite_estado.py:467`, `bin/tests/test_codex_ociosidade.sh:138`, `bin/tests/test_codex_ociosidade.sh:200`)

Execução integral do selo no host, igualdade dos fusos em produção e ocorrência dos defeitos em produção: [nao verificado].

**1) Os achados anteriores foram resolvidos no código?**

| Achado de v1.r/v2.r | Situação nesta cópia |
|---|---|
| Segundo juiz em últimas chamadas; observações diferentes de PID | **Resolvido na composição principal:** `ler()` compartilha `_foto`; o predicado calcula `em_curso`, e últimas lê esse valor. (`bin/codex_limite_estado.py:93`, `bin/codex_limite_estado.py:161`, `bin/codex_limite_estado.py:184`) |
| Reintrodução desse segundo juiz sobrevivia ao selo | **Resolvido para o mutante indicado:** M29 reavalia o predicado; o caso exige exatamente uma consulta de PID. (`bin/tests/test_codex_ociosidade.sh:207`, `bin/tests/test_codex_ociosidade.sh:214`, `bin/tests/test_codex_ociosidade.sh:271`) |
| Garantia de carimbo quando o item nasce ABERTO | **Não demonstrada:** a fixture escreve a fila; produtor real e procedimento de abertura: [nao verificado]. (`bin/tests/test_codex_ociosidade.sh:40`) |
| Truncamento de minutos, segundos e subsegundos | **Resolvido:** idade conserva `total_seconds()` e comparação usa `>1800`; há caso de 30min00,5s. (`bin/codex_limite_estado.py:251`, `bin/codex_limite_estado.py:304`, `bin/tests/test_codex_ociosidade.sh:190`) |
| Janelas de 24 horas divergentes | **Parcial:** limite, papéis e contexto compartilham `_na_janela`; o censo continua incluindo a borda inferior. (`bin/codex_limite_estado.py:59`, `bin/codex_limite_estado.py:83`, `bin/codex_limite_estado.py:172`, `bin/codex_limite_estado.py:291`, `bin/disjuntor.py:2132`) |
| Relógio próprio do disjuntor | **Não resolvido:** chama `ler_chamadas(raiz)` sem transmitir seu `agora`. (`bin/disjuntor.py:2036`, `bin/codex_limite_estado.py:167`) |
| Linhas curtas, hora inválida, tipo desconhecido e futuro viravam silêncio | **Resolvido na ociosidade:** são contabilizados e impedem `ok`. Os adaptadores auxiliares ainda eliminam esses diagnósticos. (`bin/codex_limite_estado.py:133`, `bin/codex_limite_estado.py:138`, `bin/codex_limite_estado.py:141`, `bin/codex_limite_estado.py:283`, `bin/codex_limite_estado.py:154`) |
| PID inválido/erro do SO virava morto | **Resolvido para chamadas sem fim:** produz valor desconhecido e SEM DADO. Com fim correspondente, PID não é examinado. (`bin/codex_limite_estado.py:196`, `bin/codex_limite_estado.py:206`, `bin/codex_limite_estado.py:213`, `bin/codex_limite_estado.py:287`) |
| `chamadas.log` ilegível derrubava `ler()` | **Resolvido nesse caminho:** guarda envolve foto, papéis e últimas. (`bin/codex_limite_estado.py:92`, `bin/codex_limite_estado.py:96`) |
| Log ausente publicava papéis zerados/últimas vazias | **Resolvido em `ler()`:** ambos tornam-se `None`, publicados como SEM DADO. (`bin/codex_limite_estado.py:95`, `bin/codex_limite_estado.py:99`, `bin/codex_limite_estado.py:447`, `bin/codex_limite_estado.py:501`) |
| `rc` misturava código e situação | **Separação resolvida:** tupla de cinco campos e consumidor adaptado. Também passou a exigir sintaxe inteira para `rc`/duração. (`bin/codex_limite_estado.py:137`, `bin/codex_limite_estado.py:187`, `bin/codex_limite_estado.py:505`) |
| `fim` com `rc=lixo` ou `dur=lixo` encerrava chamada | **Resolvido para esses exemplos.** Consistência cronológica continua sem validação. (`bin/codex_limite_estado.py:137`, `bin/codex_limite_estado.py:147`, `bin/codex_limite_estado.py:213`) |
| Sessões com `payload=[1]` ou `used_percent="lixo"` derrubavam ESTADO | **Exemplos corrigidos:** geram `ValueError`, convertido em SEM DADO. A garantia geral continua incompleta, conforme item 3. (`bin/codex_limite_estado.py:338`, `bin/codex_limite_estado.py:348`, `bin/codex_limite_estado.py:89`) |
| Exceções capturadas eram impressas como `FALHA:` e certificadas | **Corrigido nos pontos apontados:** seguem para `EXCECAO:`, rejeitada pelo executor. Isso não elimina mortes meramente textuais. (`bin/tests/test_codex_ociosidade.sh:52`, `bin/tests/test_codex_ociosidade.sh:135`, `bin/tests/test_codex_ociosidade.sh:166`, `bin/tests/test_codex_ociosidade.sh:177`, `bin/tests/test_codex_ociosidade.sh:236`) |
| M2 dependia de exceção; M11 dependia de motivo | **Esses mutantes foram corrigidos:** M2 exige mais de um ABERTO; M11 transforma erro em registro vazio, alterando estado. (`bin/tests/test_codex_ociosidade.sh:244`, `bin/tests/test_codex_ociosidade.sh:253`) |
| Diretório era caso vacuamente ausente; faltavam bordas | **Resolvido:** fila diretório alcança `open`; existem casos de 30min59s, subsegundo e início exatamente em `agora`. (`bin/codex_limite_estado.py:225`, `bin/tests/test_codex_ociosidade.sh:85`, `bin/tests/test_codex_ociosidade.sh:95`, `bin/tests/test_codex_ociosidade.sh:113`, `bin/tests/test_codex_ociosidade.sh:190`) |

**2) Quem escreve, quem lê e qual significado usa?**

| Campo | Escritor | Leitores e significado |
|---|---|---|
| `ini`: hora, obra, papel, hash, PID, pergunta | Wrapper grava hora local sem offset, hash de 12 caracteres, `$$` e pergunta raspada, antes da espera por slot. (`bin/codex_pergunta.sh:295`, `bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:364`) | `_registro` conserva hora, obra, papel, PID e chave `(obra,hash,pid)`; descarta pergunta. Papéis conta inícios; foto alimenta últimas e ociosidade. Portanto, início inclui espera do wrapper. (`bin/codex_limite_estado.py:143`, `bin/codex_limite_estado.py:145`, `bin/codex_limite_estado.py:94`, `bin/codex_limite_estado.py:105`) |
| `fim`: hora, obra, papel, hash, PID, sessão, rc, duração | Trap `_saida`, após `INI=1`; sessão ou `-`, `rc=$?`, duração em segundos pela diferença de epoch desde `T0C`. (`bin/codex_pergunta.sh:265`, `bin/codex_pergunta.sh:269`, `bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:282`) | Parser valida hora e inteiros, mas conserva somente sessão/rc/duração como texto, indexados pela chave. Descarta hora e papel do fim. `_em_curso` considera a existência da chave suficiente para encerrar. (`bin/codex_limite_estado.py:131`, `bin/codex_limite_estado.py:137`, `bin/codex_limite_estado.py:147`, `bin/codex_limite_estado.py:213`) |
| `fila.md`: identificador, estado, data/hora | Escrita demonstrada na fixture; escritor real: [nao verificado]. (`bin/tests/test_codex_ociosidade.sh:40`) | Leitor reconhece quatro estados; conta ABERTO, exige carimbo com precisão de minuto e calcula idade em segundos, incluindo fração. Ociosidade consome quantidade, idade e erro. (`bin/codex_limite_estado.py:53`, `bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:251`, `bin/codex_limite_estado.py:295`) |
| `limite.log`: hora, obra, mensagem | Wrapper escreve quando RC não é 0/124 e encontra mensagem de limite/indisponibilidade. (`bin/codex_pergunta.sh:451`, `bin/codex_pergunta.sh:453`, `bin/codex_pergunta.sh:456`) | `ler()` exige três campos e hora válida/não futura; conta registros sem revalidar obra/mensagem. Ausência significa zero pelo contrato declarado. (`bin/codex_limite_estado.py:6`, `bin/codex_limite_estado.py:73`, `bin/codex_limite_estado.py:78`) |
| Tupla `(hora,obra,papel,rc,situacao)` | `ultimas_chamadas`; `rc` é texto numérico ou `None`, situação ocupa campo separado. (`bin/codex_limite_estado.py:185`, `bin/codex_limite_estado.py:187`) | `_ultimas_txt` desempacota cinco campos; testes verificam código e situação separadamente. (`bin/codex_limite_estado.py:505`, `bin/tests/test_codex_ociosidade.sh:68`, `bin/tests/test_codex_ociosidade.sh:72`) |

Outros consumidores são procedência de sessões, tokens por papel e censo do disjuntor. Eles relêem pelo adaptador `_linhas_chamadas`, que devolve apenas inícios/fins e elimina os contadores de erro. (`bin/codex_limite_estado.py:151`, `bin/codex_limite_estado.py:154`, `bin/codex_limite_estado.py:312`, `bin/codex_limite_estado.py:418`, `bin/gerar_codex_estado.py:176`, `bin/disjuntor.py:2126`)

**Não há significado temporal uniforme entre todos:** módulo usa `(agora−24h,agora]`; censo usa `[agora−86400s,agora]`, convertendo hora local para epoch; contagem por papel do disjuntor captura outro relógio. (`bin/codex_limite_estado.py:59`, `bin/disjuntor.py:2085`, `bin/disjuntor.py:2132`, `bin/disjuntor.py:2036`)

Produtores não gravam offset e leitores usam datetime ingênuo. Igualdade efetiva dos fusos dos processos e fuso do produtor real da fila: [nao verificado]. (`bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:456`, `bin/codex_limite_estado.py:131`, `bin/codex_limite_estado.py:243`)

**3) Ainda há entrada virando zero/ok ou leitura derrubando o gerador? Sim.**

- **Conclusão cronologicamente impossível vira `ok`.** Um `ini` às 09:50 e `fim` às 09:00 da mesma chave são aceitos: o horário do fim é descartado e sua existência encerra a chamada. Com fila sem ABERTO, resulta `em_curso=0`, `estado=ok`, situação `terminou`. Mesmo PID `abc` passa nesse caminho, pois o fim evita a consulta ao PID. (`bin/codex_limite_estado.py:147`, `bin/codex_limite_estado.py:185`, `bin/codex_limite_estado.py:213`, `bin/codex_limite_estado.py:304`)
- **Sessão ainda pode derrubar `linha(ler())`.** `used_percent` com inteiro `10**400`, escrito por extenso no JSON, passa pela validação de `int`; depois `float()` levanta `OverflowError` fora da guarda. O gerador chama essa composição sem proteção local e `relato.sh` propaga falha. (`bin/codex_limite_estado.py:348`, `bin/codex_limite_estado.py:467`, `bin/codex_limite_estado.py:472`, `bin/gerar_estado.py:270`, `bin/relato.sh:38`)
- **Outras corrupções viram ausência de informação:** `rate_limits=false`, `0` ou `[]` tornam-se `{}`; `primary=[1]` torna-se `None`; JSON inválido contendo `rate_limits` é ignorado. Podem publicar `[sem used_percent]` ou `[sem rate_limits nas sessoes]`, sem classificar corrupção como SEM DADO. (`bin/codex_limite_estado.py:334`, `bin/codex_limite_estado.py:340`, `bin/codex_limite_estado.py:350`, `bin/codex_limite_estado.py:461`, `bin/codex_limite_estado.py:468`)

**4) Cada mutante morre pela propriedade? Não.**

Na reprodução dos casos Python em memória, com E/S e PIDs simulados, original passou e M1–M31 produziram `FALHA:` sem exceção escapada. Os casos e o mecanismo avaliados estão em `bin/tests/test_codex_ociosidade.sh:29`, `bin/tests/test_codex_ociosidade.sh:215`, `bin/tests/test_codex_ociosidade.sh:235`. Execução integral do shell com permissões/PIDs reais: [nao verificado].

| Mutantes | O que efetivamente os mata |
|---|---|
| M1–M3 | Estado incorreto diante de chamada viva, um ABERTO antigo ou idade abaixo/no limiar. (`bin/tests/test_codex_ociosidade.sh:61`, `bin/tests/test_codex_ociosidade.sh:63`, `bin/tests/test_codex_ociosidade.sh:84`, `bin/tests/test_codex_ociosidade.sh:86`) |
| M4, M8 | PID morto contado/publicado como vivo; M4 também viola quantidade de consultas. (`bin/tests/test_codex_ociosidade.sh:66`, `bin/tests/test_codex_ociosidade.sh:68`, `bin/tests/test_codex_ociosidade.sh:214`) |
| M5–M7, M9–M14 | Perda de SEM DADO diante de ausência, carimbo inválido, fila/log ilegível ou PID desconhecido. (`bin/tests/test_codex_ociosidade.sh:76`, `bin/tests/test_codex_ociosidade.sh:80`, `bin/tests/test_codex_ociosidade.sh:81`, `bin/tests/test_codex_ociosidade.sh:114`, `bin/tests/test_codex_ociosidade.sh:124`, `bin/tests/test_codex_ociosidade.sh:129`, `bin/tests/test_codex_ociosidade.sh:146`, `bin/tests/test_codex_ociosidade.sh:149`, `bin/tests/test_codex_ociosidade.sh:155`) |
| M15, M28 | Idades acima de 30 minutos deixam de acusar. (`bin/tests/test_codex_ociosidade.sh:85`, `bin/tests/test_codex_ociosidade.sh:190`) |
| M16, M18 | Contagens incluem borda inferior ou excluem `agora`. (`bin/tests/test_codex_ociosidade.sh:98`, `bin/tests/test_codex_ociosidade.sh:100`) |
| M17, M19 | Futuro perde SEM DADO; situação sintética entra em `rc`. (`bin/tests/test_codex_ociosidade.sh:103`, `bin/tests/test_codex_ociosidade.sh:74`) |
| **M20** | **Só descrição:** continua SEM DADO, mas publica “nao lido” em vez de “ilegivel: PermissionError”. (`bin/tests/test_codex_ociosidade.sh:138`, `bin/tests/test_codex_ociosidade.sh:139`, `bin/tests/test_codex_ociosidade.sh:262`, `bin/codex_limite_estado.py:448`) |
| **M21** | **Só descrição/classe:** a guarda apaga `{}`/`[]`, preservando SEM DADO; muda para “ilegivel: None”. A foto relê e continua SEM DADO. (`bin/codex_limite_estado.py:98`, `bin/codex_limite_estado.py:99`, `bin/codex_limite_estado.py:273`, `bin/tests/test_codex_ociosidade.sh:138`, `bin/tests/test_codex_ociosidade.sh:263`) |
| M22 | Comparação de estado: retorna `ok` diante de RuntimeError deliberadamente injetado. A exceção é antecedente esperado do caso; a morte vem da resposta errada, não de escape. (`bin/tests/test_codex_ociosidade.sh:117`, `bin/tests/test_codex_ociosidade.sh:120`, `bin/tests/test_codex_ociosidade.sh:54`) |
| M23–M27 | Limite ilegível vira zero; contador de inválidas desaparece; erro semanal vira ausência; chamadas ausentes viram zero/vazio; fim inválido passa a concluir. (`bin/tests/test_codex_ociosidade.sh:164`, `bin/tests/test_codex_ociosidade.sh:170`, `bin/tests/test_codex_ociosidade.sh:175`, `bin/tests/test_codex_ociosidade.sh:182`, `bin/tests/test_codex_ociosidade.sh:183`, `bin/tests/test_codex_ociosidade.sh:188`) |
| M29 | Duas consultas de PID em vez de uma. (`bin/tests/test_codex_ociosidade.sh:214`, `bin/tests/test_codex_ociosidade.sh:271`) |
| M30 | Validação deixa `erro_semanal=None`. Há mordida na classificação da entrada, mas o `if/elif` evita exercitar a formatação que levantaria `ValueError`. (`bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:201`, `bin/tests/test_codex_ociosidade.sh:272`, `bin/codex_limite_estado.py:467`) |
| **M31** | **Só classe:** troca ValueError explícito por AttributeError em `pl.get`; a mesma guarda mantém SEM DADO. O caso exige exatamente ValueError. (`bin/codex_limite_estado.py:89`, `bin/codex_limite_estado.py:338`, `bin/codex_limite_estado.py:340`, `bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:273`) |

M20, M21 e M31 são equivalentes quanto à propriedade **preservar SEM DADO**, embora suas saídas completas difiram. O executor aceita qualquer `FALHA:` após excluir exceções; não distingue propriedade de descrição/classe. (`bin/codex_limite_estado.py:99`, `bin/codex_limite_estado.py:447`, `bin/codex_limite_estado.py:458`, `bin/tests/test_codex_ociosidade.sh:236`, `bin/tests/test_codex_ociosidade.sh:238`)

O relaxamento adicional `b>=1` → `b>=0` continua equivalente sob os invariantes: zero ABERTO implica idade `None`, barrada por `s is not None`. M2 usa `b>1`, alteração diferente. A vacuidade antiga do diretório foi corrigida; garantia de escrita do carimbo continua sem produtor demonstrado. (`bin/codex_limite_estado.py:225`, `bin/codex_limite_estado.py:259`, `bin/codex_limite_estado.py:304`, `bin/tests/test_codex_ociosidade.sh:244`; produtor: [nao verificado])

**5) Classes solicitadas.**

- **Segundo juiz:** removido da composição principal e sua reintrodução agora é detectada por M29. (`bin/codex_limite_estado.py:161`, `bin/codex_limite_estado.py:184`, `bin/tests/test_codex_ociosidade.sh:214`)
- **Relógio fora da borda:** permanece no disjuntor. (`bin/disjuntor.py:2036`, `bin/codex_limite_estado.py:167`)
- **Derivação paralela:** permanece na janela inclusiva do censo e nos consumidores que descartam diagnósticos do parser. (`bin/disjuntor.py:2132`, `bin/codex_limite_estado.py:154`)
- **Campo com dois significados:** `rc`/situação foi separado; a validação semântica do fim continua incompleta. (`bin/codex_limite_estado.py:187`, `bin/codex_limite_estado.py:147`, `bin/codex_limite_estado.py:213`)
- **Selo sem mordida:** permanece em M20/M21/M31 quanto à propriedade pedida; morte depende de descrição/classe. (`bin/tests/test_codex_ociosidade.sh:138`, `bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:262`, `bin/tests/test_codex_ociosidade.sh:273`)

VEREDITO: REPROVA -- cronologia inválida ainda vira ok, sessão pode derrubar o ESTADO, contrato temporal diverge e três mutantes morrem só por descrição/classe (bin/codex_limite_estado.py:213, bin/codex_limite_estado.py:467; bin/gerar_estado.py:270; bin/disjuntor.py:2036, bin/disjuntor.py:2132; bin/tests/test_codex_ociosidade.sh:138, bin/tests/test_codex_ociosidade.sh:200).

## 17/11 10:13 · sessao 436fe2ea11f4 · - · - · origem=Ronald (tmux)

Reprodução **somente em memória**, dos casos Python extraídos do selo, com E/S e PIDs simulados: original passou; M1–M31 produziram `FALHA:`, sem `EXCECAO:` nem exceção escapada. Isso verifica a lógica dos casos; execução integral do shell, permissões/PIDs reais e ocorrência em produção: [nao verificado]. Os casos e o executor correspondem a `bin/tests/test_codex_ociosidade.sh:29`, `bin/tests/test_codex_ociosidade.sh:215`, `bin/tests/test_codex_ociosidade.sh:235`.

| Mutantes | Resultado/classificação e asserção |
|---|---|
| M1 | Propriedade: chamada viva passa a ACUSA; comparação de estado. (`bin/tests/test_codex_ociosidade.sh:63`, `bin/tests/test_codex_ociosidade.sh:243`) |
| M2 | Propriedade: um ABERTO antigo deixa de acusar. (`bin/tests/test_codex_ociosidade.sh:61`, `bin/tests/test_codex_ociosidade.sh:244`) |
| M3 | Propriedade: 30min00s e 10min passam a acusar. (`bin/tests/test_codex_ociosidade.sh:84`, `bin/tests/test_codex_ociosidade.sh:86`, `bin/tests/test_codex_ociosidade.sh:245`) |
| M4 | Propriedade: PID morto/ilegível passa a vivo; também elimina consulta de PID e viola contador. (`bin/tests/test_codex_ociosidade.sh:66`, `bin/tests/test_codex_ociosidade.sh:149`, `bin/tests/test_codex_ociosidade.sh:214`, `bin/tests/test_codex_ociosidade.sh:246`) |
| M5–M7 | Propriedade: ABERTO sem hora, fila ausente e log ausente deixam SEM DADO. (`bin/tests/test_codex_ociosidade.sh:76`, `bin/tests/test_codex_ociosidade.sh:80`, `bin/tests/test_codex_ociosidade.sh:81`, `bin/tests/test_codex_ociosidade.sh:247`) |
| M8 | Propriedade: PID morto publicado “em curso”. (`bin/tests/test_codex_ociosidade.sh:68`, `bin/tests/test_codex_ociosidade.sh:250`) |
| M9–M10 | Propriedade: fila malformada/diretório/ilegível passa a `ok`. (`bin/tests/test_codex_ociosidade.sh:114`, `bin/tests/test_codex_ociosidade.sh:124`, `bin/tests/test_codex_ociosidade.sh:127`, `bin/tests/test_codex_ociosidade.sh:251`) |
| M11–M14 | Propriedade: log ilegível/corrompido ou PID desconhecido deixa SEM DADO. (`bin/tests/test_codex_ociosidade.sh:129`, `bin/tests/test_codex_ociosidade.sh:146`, `bin/tests/test_codex_ociosidade.sh:149`, `bin/tests/test_codex_ociosidade.sh:155`, `bin/tests/test_codex_ociosidade.sh:253`) |
| M15 | Propriedade: 30min59s e 30min00,5s deixam de acusar. (`bin/tests/test_codex_ociosidade.sh:85`, `bin/tests/test_codex_ociosidade.sh:190`, `bin/tests/test_codex_ociosidade.sh:257`) |
| M16, M18 | Propriedade: contagens incluem borda inferior ou excluem `agora`. (`bin/tests/test_codex_ociosidade.sh:98`, `bin/tests/test_codex_ociosidade.sh:100`, `bin/tests/test_codex_ociosidade.sh:258`, `bin/tests/test_codex_ociosidade.sh:260`) |
| M17 | Propriedade: chamada futura deixa SEM DADO. (`bin/tests/test_codex_ociosidade.sh:103`, `bin/tests/test_codex_ociosidade.sh:259`) |
| M19 | Propriedade: situação sintética entra em `rc`. (`bin/tests/test_codex_ociosidade.sh:74`, `bin/tests/test_codex_ociosidade.sh:261`) |
| **M20** | **Somente texto do motivo:** auxiliares continuam SEM DADO, agora “chamadas.log nao lido”. Morre nas exigências literais de “ilegivel: PermissionError”. (`bin/tests/test_codex_ociosidade.sh:138`, `bin/tests/test_codex_ociosidade.sh:139`, `bin/tests/test_codex_ociosidade.sh:262`; `bin/codex_limite_estado.py:447`, `bin/codex_limite_estado.py:501`) |
| **M21** | **Somente texto/classe:** `sem_dado_chamadas='ilegivel: None'` continua verdadeiro, e a guarda apaga `{}`/`[]`; foto relê e permanece SEM DADO. Morre apenas nas mesmas exigências literais. **Equivalente quanto à propriedade SEM DADO, não quanto a todos os campos/saída.** (`bin/codex_limite_estado.py:97`, `bin/codex_limite_estado.py:98`, `bin/codex_limite_estado.py:99`, `bin/codex_limite_estado.py:273`; `bin/tests/test_codex_ociosidade.sh:138`, `bin/tests/test_codex_ociosidade.sh:139`, `bin/tests/test_codex_ociosidade.sh:263`) |
| M22 | Propriedade: estado vira `ok` diante do RuntimeError esperado. A morte é comparação de estado; há injeção explícita de exceção com `excecao=True`, não exceção escapada certificada como morte. (`bin/tests/test_codex_ociosidade.sh:117`, `bin/tests/test_codex_ociosidade.sh:120`, `bin/tests/test_codex_ociosidade.sh:54`, `bin/tests/test_codex_ociosidade.sh:264`) |
| M23 | Propriedade: limite ilegível passa a zero publicado. (`bin/tests/test_codex_ociosidade.sh:164`, `bin/tests/test_codex_ociosidade.sh:265`) |
| M24 | Propriedade: inválidas de limite ficam zero, em lugar de dois. (`bin/tests/test_codex_ociosidade.sh:170`, `bin/tests/test_codex_ociosidade.sh:266`) |
| M25 | Propriedade: erro semanal vira ausência sem classe/SEM DADO. (`bin/tests/test_codex_ociosidade.sh:175`, `bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:267`) |
| M26 | Propriedade: log ausente publica papéis zerados e últimas vazias. (`bin/tests/test_codex_ociosidade.sh:182`, `bin/tests/test_codex_ociosidade.sh:183`, `bin/tests/test_codex_ociosidade.sh:268`) |
| M27 | Propriedade: `fim` inválido encerra a chamada e remove SEM DADO. (`bin/tests/test_codex_ociosidade.sh:185`, `bin/tests/test_codex_ociosidade.sh:188`, `bin/tests/test_codex_ociosidade.sh:269`) |
| M28 | Propriedade: 30min00,5s deixa de acusar. (`bin/tests/test_codex_ociosidade.sh:190`, `bin/tests/test_codex_ociosidade.sh:270`) |
| M29 | Propriedade: consulta de PID duplica, contador recebe 2 em vez de 1. O sobrevivente adicional da v2 agora morre. (`bin/tests/test_codex_ociosidade.sh:207`, `bin/tests/test_codex_ociosidade.sh:214`, `bin/tests/test_codex_ociosidade.sh:271`) |
| M30 | Propriedade de validação: `erro_semanal=None` em vez de ValueError. **O `if/elif` evita executar `linha(d20)` quando isso ocorre**, então a conversão posterior `float('lixo')` não é exercitada nesse mutante. (`bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:201`, `bin/tests/test_codex_ociosidade.sh:272`; `bin/codex_limite_estado.py:467`) |
| **M31** | **Somente classe da exceção:** remoção da validação troca ValueError explícito por AttributeError em `pl.get`; `ler()` captura ambos e mantém SEM DADO. A comparação exige exatamente ValueError. **Equivalente quanto à propriedade SEM DADO.** (`bin/codex_limite_estado.py:89`, `bin/codex_limite_estado.py:338`, `bin/codex_limite_estado.py:340`; `bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:273`) |

O executor agora rejeita `Traceback`/`EXCECAO:` antes de aceitar `FALHA:`, e os captures corrigidos encaminham exceções para `excecoes`; a falha anterior de certificar escapes foi corrigida nesses pontos. O executor ainda não distingue divergência de propriedade de divergência apenas textual, como M20/M21/M31. (`bin/tests/test_codex_ociosidade.sh:52`, `bin/tests/test_codex_ociosidade.sh:135`, `bin/tests/test_codex_ociosidade.sh:166`, `bin/tests/test_codex_ociosidade.sh:177`, `bin/tests/test_codex_ociosidade.sh:236`, `bin/tests/test_codex_ociosidade.sh:238`)

O relaxamento adicional `b>=1`→`b>=0` continua equivalente sob os invariantes: zero ABERTO implica idade `None`, barrada por `s is not None`; M2 testa `b>1`, uma mutação diferente. O caso “fila é diretório” agora alcança `open`, portanto a vacuidade antiga foi corrigida. (`bin/codex_limite_estado.py:225`, `bin/codex_limite_estado.py:259`, `bin/codex_limite_estado.py:304`; `bin/tests/test_codex_ociosidade.sh:113`, `bin/tests/test_codex_ociosidade.sh:244`)

## 17/11 10:13 · sessao 6fee9ce867f3 · RESPOSTAS-1711-1005 · verificador · origem=Code

**AVAL-DEVOLVIDO: item 3**, pela premissa de encaminhamento atual e pela expressão ampla “ausência registrada”. O corte que atribui CADASTRO pode resolver a escolha aberta, mas esse destino ainda precisa chegar ao juiz e à lista. `app/docs/LEIS.md:122`; `app/docs/RELATO.md:352`; `app/ponto/management/commands/e6_oraculo.py:53`; `app/escala/services/cadastro_realidade.py:728`.

**Item 4 CONFERE para a opção A do conflito texto corrente versus foto de REABERTURA/RECUSA.** Isso não significa que todo consumidor ignore `ato_texto`: a tarefa já lê esse campo quando o último ato é RESPOSTA. `app/docs/LEIS.md:151`; `app/chamados/services/disputa_emissao.py:1632`; `app/chamados/services/tarefa_do_dia.py:159`.

**Item 3 — as quatro perguntas**

1. **As premissas conferem? Parcialmente.** L-099 define ESTRUTURA/CADASTRO/BATIDA, reserva construção a ESTRUTURA e destina CADASTRO/BATIDA à lista do admin. Ela não atribuía, sozinha, CADASTRO a FATO_EM_AUSENCIA; essa escolha permanecia aberta. `app/docs/LEIS.md:122`; `app/docs/RELATO.md:352`.

   O juiz existente é `e6_oraculo.dono_da_divergencia`, carregado por `InsumosDoDono.dono(data)`. Sua assinatura não recebe FATO_EM_AUSENCIA; seus ramos ainda podem devolver BATIDA ou ESTRUTURA. Portanto, o novo dono não está realizado nesse juiz. `app/ponto/management/commands/e6_oraculo.py:53`; `app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:182`.

   **Existe sinal na lista Cadastro x Realidade, mas não o caminho geral prometido:** A9 tem rótulo “ausência aprovada com batida”, dono literal `juiz`, exige `ausencia_id` e pelo menos duas batidas. Entra agregado, sem data/dono diário, e recebe a ação **“Com o sistema: em cura (nao e do cadastro)”**, sem URL. `app/ponto/services/esmeril_espelho.py:23`; `app/ponto/services/esmeril_espelho.py:133`; `app/escala/services/cadastro_realidade.py:923`; `app/escala/services/cadastro_realidade.py:728`.

   A rota reconhecida pelo vigia é **Cadastro x Realidade → `dias_na_lista` → `ListaDoAdmin.dia_do_admin` → `fio_do_admin`**. Ela exige assinatura datada e dono CADASTRO/BATIDA; o A9 agregado não cumpre essas condições. `app/escala/services/cadastro_realidade.py:1047`; `app/chamados/services/vigia_fio.py:200`; `app/chamados/services/vigia_fio.py:261`.

   Há outra exposição administrativa, restrita: a tarefa oferece VALIDAR/ADMIN quando o dia não é exportado, está sem trabalho, tem veredito `fato_em_ausencia` e nenhuma pergunta viva C6. Isso não constitui a rota geral à lista com dono L-099. `app/chamados/services/tarefa_do_dia.py:352`.

2. **Colide com corte vigente?** Atribuir CADASTRO resolve a escolha aberta e é compatível com L-099; não autoriza ampliar a classe FATO_EM_AUSENCIA. Parcial sem período continua CADASTRO com código próprio; parcial fora do período não entra nesse FATO. `app/docs/LEIS.md:122`; `app/docs/RELATO.md:352`; `app/docs/CORTES.md:386`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:403`.

3. **Alguma condição não fecha no literal? Sim.** “Ausência registrada” abrange mais que a classificação atual: o juiz distingue cobertura aprovada, período parcial e fato do dia; coexistência de registro e batida não basta. O brief deve referir-se à **classe lavrada pelo juiz**, preservando essas distinções. `app/ponto/supra_juiz.py:392`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:413`; `app/ponto/turnos.py:2523`.

   O bloqueio por cruzamento pode permanecer como condição do aval; o cruzamento autenticado atual com B2+delta/F2 não é demonstrável pelas listas disponíveis no dado. [nao verificado]

4. **Toca exportada, dinheiro ou `!` além do declarado?** O item manda registrar o corte e manter a obra sem abrir; não concede execução de ajustes de dados. [nao verificado: literal do anexo]

   As ações mencionadas têm efeito financeiro: ajustar ausência pode mudar insumo da folha e acordar projeções; “descartar batida” deve significar **retratar**, preservando o registro e retirando minutos da apuração. `app/ponto/services/ausencia.py:989`; `app/ponto/services/ausencia.py:1123`; `app/ponto/registro_batida.py:289`; `app/ponto/services/retratar_humano.py:134`.

   Não cabe presumir exportadas genericamente protegidas: L-113 alterou a restrição anterior, e o evento pode recalcular competência exportada. Isso não transforma o registro do item 3 em autorização de correção ou ato de frota. `app/docs/LEIS.md:155`; `app/ponto/services/fechamento.py:1790`; `app/docs/LEIS.md:62`.

**Item 4 — as quatro perguntas**

1. **As premissas conferem? Sim, no conflito da opção A.** Reabrir/recusar guarda a resposta retirada em `ato_texto` e limpa o par corrente. O histórico de respostas anteriores lê essas fotos. `app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1632`; `app/chamados/services/disputa_emissao.py:1648`; `app/chamados/models.py:3667`.

   Os leitores comprovados do texto corrente são:

   | Leitor | Evidência |
   |---|---|
   | Juiz `pergunta_respondida` e C6 | `app/chamados/juizes.py:1552`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1423` |
   | Validação e materialização de batida | `app/chamados/services/validacao.py:1094`; `app/chamados/services/materializacao.py:625`; `app/chamados/services/materializacao.py:678` |
   | Calendário e prévia da folha | `app/colaboradores/services/calendario.py:494`; `app/folha/previa.py:142` |
   | Raio X, API e apresentação | `app/chamados/services/raiox.py:138`; `app/api/views.py:2561`; `app/chamados/models.py:3686` |
   | Guarda do fechamento por SILENCIO | `app/chamados/models.py:1923`; `app/chamados/models.py:1968` |

   **SILENCIO** consulta respostas sem veredito pelo par corrente e registra o ato copiando `resposta_colab`; não recupera o texto retirado de REABERTURA/RECUSA. Na tarefa, último ato SILENCIO retorna nenhuma ação. `app/chamados/models.py:1923`; `app/chamados/models.py:2005`; `app/chamados/services/tarefa_do_dia.py:176`.

   **Exceção à leitura universal:** a tarefa consulta `ult.ato_texto` no ramo RESPOSTA; em REABERTURA/RECUSA consulta o tipo do ato. Logo, “texto fica histórico” não pode virar proibição de toda leitura operacional de `ato_texto`. `app/chamados/services/tarefa_do_dia.py:159`; `app/chamados/services/tarefa_do_dia.py:163`.

2. **Colide com corte vigente?** A opção A conserva a L-130: reabertura/ressuscitação limpa a resposta inteira e mantém o texto anterior na trilha. Continua vigente o corte F8 anterior: validação humana legada não é revogada por RESPOSTA sintética posterior, com nenhuma batida validada alterada. `app/docs/LEIS.md:151`; `app/docs/CORTES.md:488`.

3. **Alguma condição não fecha no literal?** A escolha documental fecha nesse recorte; a construção futura continua devendo a medida de neutralidade exigida pelo corte anterior. `app/docs/CORTES.md:488`.

   O raspado declara 226 divergências, distribuídas em 214 REABERTURAS e 12 RECUSAS, mas não comprova o DIFF real nem neutralidade das batidas; as linhas `COL` somam 191, divergindo do total declarado. [nao verificado]

4. **Toca exportada, dinheiro ou `!` além do declarado?** Conservar a fonte corrente e registrar a escolha não exige substituir texto, plantar batida ou recalcular folha. A futura implementação não recebe certificado de dinheiro zero: o materializador usa esse texto para produzir fatos, e o corte anterior mantém a parada se o DIFF mudar o julgamento em exportada. `app/chamados/services/materializacao.py:678`; `app/docs/CORTES.md:488`.

**Quem escreve e quem lê os campos**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `resposta_colab` | Resposta humana e automática pelas portas de `disputa_emissao`; reabertura/recusa e ressuscitação zeram pela porta de descarimbo. `app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1648`; `app/chamados/models.py:1629`. | Juiz/C6, validação, materializador, calendário, prévia, API, apresentação e guarda do SILENCIO, nos sítios relacionados acima. |
| `ato_texto` | Porta única `DecisaoPergunta.registrar`; recebe resposta, reabertura/recusa, validação, devolução, SILENCIO, absorção e backfill. `app/chamados/models.py:4110`; `app/chamados/services/disputa_emissao.py:1525`; `app/chamados/services/disputa_emissao.py:1632`; `app/chamados/services/validacao.py:175`; `app/chamados/services/validacao.py:281`; `app/chamados/models.py:2005`; `app/chamados/services/materializacao.py:1047`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:326`. | Histórico de respostas anteriores; tarefa no ramo último ato RESPOSTA. `app/chamados/models.py:3675`; `app/chamados/services/tarefa_do_dia.py:160`. |
| Ausência/status/período | Colaborador/API e admin chamam criação/edição; transição normal usa `mudar_estado`, decisão/período usa `decidir`. Férias derivadas têm escrita própria de status na mesma família de serviço. `app/api/views.py:1812`; `app/ponto/views.py:2948`; `app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:1088`; `app/ponto/services/ausencia.py:1101`; `app/ponto/services/ausencia.py:1496`. | Cobertura: folha somente aprovada, cobrança não rejeitada; suprajuiz, tarefa, vigia e fechamento consomem esses insumos. `app/ponto/turnos.py:2523`; `app/ponto/supra_juiz.py:395`; `app/chamados/services/tarefa_do_dia.py:178`; `app/chamados/services/vigia_fio.py:256`; `app/ponto/services/fechamento.py:638`. |
| Batida | API e materializador chamam `registrar_batida`; correção usa `anotar`; retratação humana autorizada chama `retratar_batida`, que preserva a linha. `app/api/views_core.py:645`; `app/chamados/services/materializacao.py:1570`; `app/ponto/registro_batida.py:255`; `app/ponto/services/retratar_humano.py:163`; `app/ponto/registro_batida.py:287`. | `batidas_apuraveis` exclui retratadas; juiz de dono, fechamento e export consomem a apuração; A9 conta batidas para sinalização. `app/ponto/turnos.py:2868`; `app/ponto/management/commands/e6_oraculo.py:150`; `app/ponto/services/fechamento.py:632`; `app/folha/export.py:499`; `app/ponto/services/esmeril_espelho.py:133`. |

VEREDITO: item 3 PROCEDE; item 4 CONFERE -- falta a rota diária CADASTRO/admin prometida no item 3 (app/escala/services/cadastro_realidade.py:728; app/escala/services/cadastro_realidade.py:1047); opção A conserva texto corrente e foto histórica de REABERTURA/RECUSA (app/docs/LEIS.md:151; app/chamados/services/disputa_emissao.py:1632).

## 17/11 10:13 · sessao 689b1f17ac1e · - · - · origem=Ronald (tmux)

A composição ainda pode cair por entrada de sessão; confirmei `linha(ler())` somente em memória, sem escrever arquivos. O guard de `ler()` protege a leitura semanal, mas não a formatação posterior; `used_percent=10**400` é aceito como `int`, `erro_semanal` fica `None`, e `float(...)` levanta `OverflowError` na montagem da frase. A chamada do gerador não tem guarda local; `relato.sh` propaga saída 1. (`bin/codex_limite_estado.py:87`, `bin/codex_limite_estado.py:348`, `bin/codex_limite_estado.py:467`, `bin/codex_limite_estado.py:472`, `bin/gerar_estado.py:270`, `bin/relato.sh:38`)

Reprodução usada: E/S substituída por `StringIO`, sessão `{"payload":{"rate_limits":{"primary":{"used_percent":10**400,"window_minutes":10080}}}}` — com o inteiro escrito por extenso no JSON —, chamadas concluídas e fila vazia. Resultado: `ler erro_semanal=None ociosidade=ok`; `linha(ler()) => OverflowError int too large to convert to float`. Isso decorre diretamente da validação que admite qualquer inteiro e da conversão posterior. (`bin/codex_limite_estado.py:348`, `bin/codex_limite_estado.py:467`)

Outros casos de esquema inválido permanecem tratados como ausência de informação:

- `rate_limits=false`, `rate_limits=0` ou `rate_limits=[]` viram `{}` pelo `or {}` e não recebem `erro_semanal`; o resultado é `[sem used_percent]`. (`bin/codex_limite_estado.py:340`, `bin/codex_limite_estado.py:352`, `bin/codex_limite_estado.py:468`)
- `primary=[1]` vira `primary=None`, também sem SEM DADO pela fonte; confirmei em memória. (`bin/codex_limite_estado.py:345`, `bin/codex_limite_estado.py:350`, `bin/codex_limite_estado.py:468`)
- Uma última linha contendo `rate_limits` com JSON sintaticamente inválido é ignorada; se não há outra candidata, retorna `None` e publica `[sem rate_limits nas sessoes]`, sem classificar a corrupção. (`bin/codex_limite_estado.py:328`, `bin/codex_limite_estado.py:334`, `bin/codex_limite_estado.py:353`, `bin/codex_limite_estado.py:460`)

A validação do `fim` foi ampliada para exigir `rc`/`dur` inteiros, mas ainda descarta o horário do fim e aceita PID inválido quando existe fim da mesma chave: `_em_curso` retorna `False` antes de consultar `_pid_vivo`. Em memória, `ini` às 09:50 com PID `abc` e `fim` às 08:00 da mesma chave produziu `descartadas=0`, `em_curso=0`, `estado=ok` e últimas `('0','terminou')` com fila vazia. Portanto, não há validação de cronologia ou da identidade do PID de chamadas concluídas. (`bin/codex_limite_estado.py:136`, `bin/codex_limite_estado.py:143`, `bin/codex_limite_estado.py:147`, `bin/codex_limite_estado.py:213`, `bin/codex_limite_estado.py:304`)

Os exemplos específicos de v2 com `payload=[1]` e `used_percent="lixo"` estão corrigidos: o leitor levanta `ValueError`, e `ler()` converte para SEM DADO antes da frase. O reparo não cobre todos os valores numéricos nem todas as estruturas inválidas. (`bin/codex_limite_estado.py:338`, `bin/codex_limite_estado.py:348`, `bin/codex_limite_estado.py:89`, `bin/codex_limite_estado.py:458`)

Execução integral do selo e ocorrência desses defeitos em produção: [nao verificado].

## 17/11 10:12 · sessao 39528a968e59 · - · - · origem=Ronald (tmux)

Persistem dois achados de v2: relógio próprio na contagem por papel do disjuntor e borda inferior inclusiva no censo. O produtor real da fila continua [nao verificado]. (`bin/disjuntor.py:2036`, `bin/codex_limite_estado.py:167`, `bin/disjuntor.py:2132`, `bin/tests/test_codex_ociosidade.sh:40`)

| Campo | Quem escreve | Quem lê / contrato |
|---|---|---|
| `ini`: hora, obra, papel, hash, PID, pergunta | Wrapper grava `date` sem offset, `OBRA`, `PAPEL`, hash de 12 caracteres, `$$`, pergunta raspada. O registro antecede a espera por slot. (`bin/codex_pergunta.sh:295`, `bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:364`) | `_registro` conserva hora, obra, papel, PID e chave `(obra,hash,pid)`; pergunta descartada. Papéis conta inícios; últimas e ociosidade compartilham `_foto`. (`bin/codex_limite_estado.py:143`, `bin/codex_limite_estado.py:145`, `bin/codex_limite_estado.py:94`, `bin/codex_limite_estado.py:105`) |
| `fim`: hora, obra, papel, hash, PID, sessão, rc, duração | `_saida`, pelo trap EXIT após `INI=1`; `rc=$?`, sessão ou `-`, duração em segundos por diferença de epoch desde `T0C`. (`bin/codex_pergunta.sh:265`, `bin/codex_pergunta.sh:269`, `bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:282`) | Valida timestamp e sintaxe inteira de rc/duração; conserva sessão, rc e duração como texto. Timestamp e papel do fim são descartados. `_em_curso` usa apenas a existência da chave. Não compara fim com início nem duração com timestamps. (`bin/codex_limite_estado.py:131`, `bin/codex_limite_estado.py:137`, `bin/codex_limite_estado.py:147`, `bin/codex_limite_estado.py:213`) |
| `fila.md`: identificador, estado, data/hora | Escritor real e procedimento de abertura: [nao verificado]. Fixture escreve o arquivo. (`bin/tests/test_codex_ociosidade.sh:39`) | `ler_fila_reserva` aceita os quatro estados; conta ABERTO e exige carimbo em minutos; calcula idade em segundos de ponto flutuante, sem truncamento. Ociosidade consome quantidade/idade/erro. (`bin/codex_limite_estado.py:53`, `bin/codex_limite_estado.py:240`, `bin/codex_limite_estado.py:251`, `bin/codex_limite_estado.py:295`) |
| `limite.log`: hora, obra, mensagem | Wrapper escreve quando RC diferente de 0/124 e mensagem corresponde ao padrão de limite/indisponibilidade. (`bin/codex_pergunta.sh:451`, `bin/codex_pergunta.sh:453`, `bin/codex_pergunta.sh:456`) | `ler` exige três campos e timestamp válido/não futuro, mas não valida obra/mensagem. Ausência significa zero por contrato declarado. (`bin/codex_limite_estado.py:6`, `bin/codex_limite_estado.py:73`, `bin/codex_limite_estado.py:78`) |
| Tupla `(hora,obra,papel,rc,situacao)` | `ultimas_chamadas` escreve cinco campos; rc é texto numérico ou `None`, situação é campo separado. (`bin/codex_limite_estado.py:185`, `bin/codex_limite_estado.py:187`) | `_ultimas_txt` desempacota cinco campos e apresenta rc quando existe; os testes verificam índices 3 e 4. (`bin/codex_limite_estado.py:505`, `bin/tests/test_codex_ociosidade.sh:68`, `bin/tests/test_codex_ociosidade.sh:72`) |

Outros consumidores relêem chamadas pelo adaptador que elimina diagnósticos de entradas inválidas/futuras: procedência de sessões, tokens por papel e censo. Procedência alimenta `gerar_codex_estado`; tokens alimentam o disjuntor. (`bin/codex_limite_estado.py:151`, `bin/codex_limite_estado.py:154`, `bin/codex_limite_estado.py:312`, `bin/codex_limite_estado.py:418`, `bin/disjuntor.py:2126`, `bin/gerar_codex_estado.py:176`, `bin/disjuntor.py:2028`)

O módulo compartilha `(agora−24h,agora]` para limite, papéis e contexto horário. O censo usa `[agora−86400s,agora]` e converte timestamps locais para epoch; portanto as bordas não são uniformes. O disjuntor também consulta papéis sem transmitir seu relógio. (`bin/codex_limite_estado.py:59`, `bin/codex_limite_estado.py:83`, `bin/codex_limite_estado.py:172`, `bin/codex_limite_estado.py:291`, `bin/disjuntor.py:2085`, `bin/disjuntor.py:2132`, `bin/disjuntor.py:2036`)

Os produtores usam hora local sem offset e o parser cria datetime ingênuo; igualdade efetiva entre os fusos dos processos: [nao verificado]. (`bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:456`, `bin/codex_limite_estado.py:63`, `bin/codex_limite_estado.py:131`)

Há erro semântico de entrada que ainda pode virar `ok`: um `fim` da mesma chave anterior ao `ini` passa pela validação sintática, encerra a chamada e não gera diagnóstico; com fila sem ABERTO a decisão retorna `ok`. Também PID inválido associado a fim não é examinado, porque o predicado retorna antes da consulta de PID. Ocorrência em produção: [nao verificado]. (`bin/codex_limite_estado.py:137`, `bin/codex_limite_estado.py:147`, `bin/codex_limite_estado.py:213`, `bin/codex_limite_estado.py:304`)

## 17/11 10:12 · sessao 2691ab6f2cfa · PORTA-DO-EXPORT-SEM-BECO · leitura · origem=Code

**(b)** A justificativa literal é **“SELO DE PORTA, nao de pipeline”**: conferir tela == PDF == fechamento == TXT antes do export. Agendar mediria competências que ninguém exportará naquele dia e “encher[ia] log de verde”; o comando também reúne os dois contadores que antes existiam soltos. (`app/config/crons.py:887-897`)

Os documentos sustentam o seguinte:

- **RELATO-ARQUIVO:** registra que o contrato exigia cada comando em `CRONS` ou declarado com motivo em `FORA_DE_PIPELINE`; o autor escreveu “Declarei: selo de PORTA, nao de pipeline”, repetindo a justificativa contra agendamento. É uma decisão de implementação documentada; aval nominal específico para essa classificação: **[nao verificado]**. (`app/docs/RELATO-ARQUIVO.md:5014-5019`)
- **BACKLOG:** registra `SELO-TXT` fechado em 28/09, igualdade dos quatro leitores, selo verde nos 205 daquele universo e “Export so depois disto, com o `!`”. Essa linha não determina periodicidade diária. (`app/docs/BACKLOG.md:1203`)
- **CORTES:** declara registrar cortes/avais de Ronald; ordem específica para manter esse selo fora do pipeline: **[nao verificado]**. Os cortes sobre “carimbo da porta cega” tratam do carimbo de aprovação e de conta vazia/turno aberto. (`app/docs/CORTES.md:5-7`, `app/docs/CORTES.md:315`, `app/docs/CORTES.md:317`)
- **LAPIDES:** declara guardar histórias copiadas do `CLAUDE.md`; decisão específica sobre o agendamento deste selo: **[nao verificado]**. (`app/docs/LAPIDES.md:7-21`)

“Roda no ATO” é a formulação de `crons.py`; o caminho executável atual exige medição anterior pelo comando e **somente confere** durante o POST. (`app/config/crons.py:889-896`, `app/folha/views.py:124-133`) Git log e autoria histórica além desses registros: **[nao verificado]**.

**(c)** Nesta cópia, a cadeia operacional é:

- **Escreve:** `selo_leitores_no_mesmo_numero.Command.handle` chama `medir` e depois `lavrar` para cada empresa ativa. `lavrar` grava `MetricaSnapshot` com `CHAVE='porta_export_leitores'`; grava inclusive resultado negativo ou divergente, não apenas verde. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:33`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-95`, `app/folha/porta_export.py:18`, `app/folha/porta_export.py:768-796`)
- **Lê:** `conferir` busca o snapshot de hoje e verifica competência e valor; `gerar_dominio_txt` consome sua mensagem e recusa antes de montar o TXT. Só valor zero passa essa porta. (`app/folha/porta_export.py:800-821`, `app/folha/porta_export.py:865-867`, `app/folha/views.py:128-133`)

O censo funcional de produção é **um escritor (`lavrar`) e um leitor (`conferir`)**, com comando e view como respectivos chamadores. O helper `lavrar_verde_para_teste` também chama `lavrar`, mas se declara exclusivo de testes e fica fora desse censo. (`app/folha/porta_export.py:768`, `app/folha/porta_export.py:800`, `app/folha/porta_export.py:870-881`)

A identidade persistida é **chave + escopo + empresa + dia**; mês/ano ficam somente nos `defaults.detalhe`. A restrição única do modelo confirma essa identidade sem competência. (`app/folha/porta_export.py:772-776`, `app/inteligencia/models.py:102-105`)

- **Mesma empresa, mesmo dia:** carimbar A e depois B atualiza a mesma linha, substituindo valor e detalhe pelos de B. Pedir A passa a receber recusa de competência incompatível; pedir B ainda depende de valor zero. Empresas diferentes têm chaves separadas. (`app/folha/porta_export.py:772-776`, `app/folha/porta_export.py:815-821`)
- **Mesma empresa, dias diferentes:** são linhas distintas. Se A foi carimbada ontem e B hoje, A de ontem permanece no histórico, mas não libera export hoje; havendo B hoje, pedir A recebe incompatibilidade. Sem qualquer linha hoje, recebe “NAO FOI MEDIDA HOJE”. (`app/inteligencia/models.py:102-105`, `app/folha/porta_export.py:772-776`, `app/folha/porta_export.py:807-817`)

**(d)** A string literal, concatenada, da ausência de carimbo é esta; os `%s` recebem razão social e comando. (`app/folha/porta_export.py:812-813`)

```text
A PORTA DO EXPORT NAO FOI MEDIDA HOJE para %s. O TXT nao sai sem a prova de que a tela, o PDF, o fechamento e o TXT estao no mesmo numero. Rode: %s
```

O segundo `%s` contém este comando literal formatado com mês e ano pedidos. (`app/folha/porta_export.py:809-810`)

```text
docker exec saas_core python manage.py tenant_command selo_leitores_no_mesmo_numero --schema=juliani --mes %02d --ano %s
```

O botão mostra **`Gerar TXT`** e faz POST para `folha:gerar_txt`; a view passa a recusa intacta como `msg=_barra`, com `ok=False`, redirecionando para `folha:exportar`. O helper guarda a mensagem de erro, a view de destino renderiza `folha/exportar.html`, e o template a imprime literalmente por `{{ m }}`. (`app/templates/folha/index.html:63`, `app/templates/folha/index.html:107-109`, `app/folha/views.py:128-133`, `app/core/respostas.py:50-51`, `app/core/respostas.py:68-71`, `app/folha/views.py:92`, `app/templates/folha/exportar.html:10`) Razão social e mensagem efetivamente vistas em produção: **[nao verificado]**.

**(e)** O pré-desenho usa a infraestrutura declarada: `saas_ui`, `gthread`, `timeout=30`, com orientação “trabalho longo vai pra job”. Os ~100 s citados na porta são uma medida histórica documentada; tempo atual da competência 11 por empresa: **[nao verificado]**. A própria view ressalva que `timeout=30` não implica matar toda requisição longa aos 30 s. (`CLAUDE.md:89`, `app/gunicorn.conf.py:19-22`, `app/folha/porta_export.py:7-12`, `app/folha/views.py:182-195`)

| Alternativa | Quem escreveria | O que o admin veria | Risco inferido do código |
|---|---|---|---|
| Medir no clique, dentro do request | O request chamaria `medir → lavrar`, antes de `conferir`. (`app/folha/porta_export.py:768`, `app/folha/porta_export.py:800`) | Espera pela medição; depois, continuidade do TXT ou recusa. (`app/folha/views.py:129-134`) | Trabalho longo ocupa atendimento da UI; duração e resultado real de timeout **[nao verificado]**. (`CLAUDE.md:89`, `app/gunicorn.conf.py:19-22`) |
| Cron diário | O agendador acionaria o comando existente, que chama `lavrar`. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-92`) | Conferência do carimbo pronto; falta, outra competência ou falha continuam recusando. (`app/folha/porta_export.py:807-821`) | Medição sem export previsto; outra competência pode substituir a primeira; mudanças posteriores no mesmo dia não invalidam automaticamente a foto. (`app/config/crons.py:894-895`, `app/folha/porta_export.py:772-776`, `app/folha/porta_export.py:807-818`) |
| Cron + nova medição no clique | Dois gatilhos chamariam o mesmo `lavrar`. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:92`, `app/folha/porta_export.py:768`) | Espera pela nova medição e recebe seu resultado. (`app/folha/views.py:129-134`) | Repete trabalho e mantém o custo no request; ambos disputam a mesma chave diária por empresa. (`app/folha/porta_export.py:7-12`, `app/folha/porta_export.py:772-776`) |

PREPARA: perguntas de lei 0; escritores do carimbo 1; leitores 1

## 17/11 10:12 · sessao 07165033cd08 · - · - · origem=Ronald (tmux)

Item 4: **CONFERE**, entendido no recorte da opção A: conservar o texto corrente para os leitores existentes e manter o texto retirado por REABERTURA/RECUSA na trilha; esse comportamento já corresponde à L-130. `app/docs/LEIS.md:151`; `app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1632`; `app/chamados/services/disputa_emissao.py:1648`.

1. **Premissas e leitores.** `resposta_colab` é o texto lido pelo juiz `pergunta_respondida`, que exige instante E texto após trim; C6 usa a projeção corrente temporal. `app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1552`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1423`.

   Outros leitores comprovados do texto corrente:

   - Calendário, último recurso: consulta texto e C6. `app/colaboradores/services/calendario.py:494`; `app/colaboradores/services/calendario.py:504`; `app/colaboradores/services/calendario.py:506`.
   - Prévia, `_material`: decide pela presença do texto. `app/folha/previa.py:139`; `app/folha/previa.py:142`.
   - Materializador: texto para interpretar/plantar hora. `app/chamados/services/materializacao.py:625`; `app/chamados/services/materializacao.py:678`; `app/chamados/services/materializacao.py:718`.
   - Validação/classificação: texto para escape, confirmação e hora. `app/chamados/services/validacao.py:1094`; `app/chamados/services/validacao.py:1105`.
   - Raio X: par corrente submetido ao juiz. `app/chamados/services/raiox.py:138`; `app/chamados/services/raiox.py:147`.
   - API e apresentação: campo corrente e seu display. `app/api/views.py:1163`; `app/api/views.py:2561`; `app/chamados/models.py:3686`; `app/chamados/models.py:3710`.
   - SILENCIO: a guarda de respostas sem veredito lê o par corrente; o fechamento grava uma decisão copiando esse texto. `app/chamados/models.py:1910`; `app/chamados/models.py:1923`; `app/chamados/models.py:1968`; `app/chamados/models.py:2005`.

   **Exceção que deve ficar explícita:** não vale afirmar que *todo* leitor atual ignora `ato_texto`. A tarefa usa `ult.ato_texto` no ramo RESPOSTA; nos ramos REABERTURA/RECUSA usa o tipo do ato, e SILENCIO retorna nenhuma ação sem ler texto. `app/chamados/services/tarefa_do_dia.py:159`; `app/chamados/services/tarefa_do_dia.py:160`; `app/chamados/services/tarefa_do_dia.py:163`; `app/chamados/services/tarefa_do_dia.py:176`. O raspado atribui as 226 divergências a 214 REABERTURAS e 12 RECUSAS, portanto não demonstra divergências no ramo RESPOSTA. [nao verificado]

2. **Colisão.** Não identifiquei colisão da opção A nesse recorte: L-130 exige o texto anterior na trilha e o par inteiro zerado; L-002 exige autoridade compartilhada. `app/docs/LEIS.md:151`; `app/docs/LEIS.md:60`. Permanece o corte F8 anterior: validação humana legada segue válida apesar de RESPOSTA sintética posterior, com medida de neutralidade e nenhuma batida validada alterada. `app/docs/CORTES.md:488`.

3. **Literal/condições.** A afirmação “`ato_texto` fica histórico” precisa ser lida como texto retirado no recorte P2, não como proibição universal de leitura do campo: a tarefa já o lê. `app/chamados/models.py:4020`; `app/chamados/models.py:3667`; `app/chamados/services/tarefa_do_dia.py:160`. O raspado não comprova DIFF real da F8, neutralidade das batidas, nem a população integral: totais `NUMERO` e soma das linhas `COL` divergem. [nao verificado]

4. **Exportada/dinheiro/!.** Registrar a opção A não requer trocar texto, materializar batidas ou regerar folha; os escritores mostrados só explicam o comportamento existente. `app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1632`. Não é prova de dinheiro zero numa futura construção: o materializador deriva plantio do texto, e o corte anterior exige parar se a classificação exportada mudar. `app/chamados/services/materializacao.py:678`; `app/chamados/services/materializacao.py:718`; `app/docs/CORTES.md:488`. Eventual efeito produtivo atual é [nao verificado].

**Mapa de escrita/leitura dos dois campos:**

| Campo | Escritores | Leitores |
|---|---|---|
| `resposta_colab` | Resposta humana: `disputa_emissao.py:1481`, persistida em `:1522`; resposta automática `:1674`, persistida em `:1677`; reabertura/recusa fornece vazio em `:1609` e delega em `:1648` à porta `models.py:3362`/`:3391`; ressuscitar também zera o par `models.py:1630`; nascimento aceita campos pela porta `models.py:2930` com guarda em `:2949`. | Juiz/C6, calendário, prévia, materializador, validação, Raio X, API, display e guarda do SILENCIO, conforme referências acima. |
| `ato_texto` | Porta única `DecisaoPergunta.registrar`, `models.py:4110`/`:4120`; recebe linhas da resposta `disputa_emissao.py:1525`, reabertura/recusa `:1632`, automática `:1678`, recuperação pela trilha `:1699`; validação `validacao.py:175`, devolução `:281`/`:356`; SILENCIO `models.py:2005`, retratação `:2137`, absorção `materializacao.py:1047`, decisão do dia `ponto/services/veredito_celula.py:197`, backfill `backfill_decisoes_pergunta.py:326`. | Histórico `models.py:3667`/`:3675`; tarefa, último ato RESPOSTA, `tarefa_do_dia.py:159`/`:160`. |

Não há alteração direta de `ato_texto`: decisões existentes recusam reescrita, exclusão e atualização em lote. `app/chamados/models.py:4002`; `app/chamados/models.py:4070`; `app/chamados/models.py:4075`.

## 17/11 10:12 · sessao 2c2cbc136325 · - · - · origem=Ronald (tmux)

Mapa verificado para os campos do item 3:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| Ausência: tipo, datas, minutos | Colaborador pela API chama `criar_ausencia`; admin cria e edita pela mesma família de portas. `app/api/views.py:1812`; `app/ponto/views.py:2682`; `app/ponto/views.py:2948`; escrita em `app/ponto/services/ausencia.py:483` e `app/ponto/services/ausencia.py:648`. | Autoridades `ausencias_do_periodo`/`ausencia_cobre` leem intervalo e efeito; fechamento consome o queryset da folha. `app/ponto/turnos.py:2535`; `app/ponto/turnos.py:2550`; `app/ponto/services/fechamento.py:638`. |
| Ausência: status | Nasce de `estado_inicial`, salvo em `criar_ausencia`; transição normal por `mudar_estado`; decisão DP passa por `decidir`. `app/ponto/services/ausencia.py:475`; `app/ponto/services/ausencia.py:486`; `app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:1088`. **Exceção real:** cancelamento de férias escreve `status='rejeitada'` diretamente, com trilha dentro de atomic. `app/ponto/services/ausencia.py:1496`; `app/ponto/services/ausencia.py:1499`. | `_pelo_efeito`: folha só aprovada, cobrança qualquer não rejeitada. `app/ponto/turnos.py:2523`. Supra juiz recebe `cobertura_status` e considera aprovada em `app/ponto/supra_juiz.py:268`; fechamento pede `para='folha'` em `app/ponto/services/fechamento.py:639`. |
| Ausência: hora_ini/hora_fim | DP pela mesma porta `decidir`, inclusive edição da aprovada; trilha antes/depois. `app/ponto/views.py:2822`; `app/ponto/services/ausencia.py:1099`; `app/ponto/services/ausencia.py:1108`. | Juiz de período e supra juiz distinguem sem período, fora, dentro/indeterminado. `app/ponto/turnos.py:2634`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:403`; `app/ponto/supra_juiz.py:413`. |
| Batida: nascimento | Porta `registrar_batida`; API de chão e materializador institucional são chamadores. `app/ponto/registro_batida.py:118`; `app/api/views_core.py:645`; `app/chamados/services/materializacao.py:1455`; `app/chamados/services/materializacao.py:1570`. | Universo apurável por `batidas_apuraveis`, apenas `retratada_em NULL`; fechamento e export consomem essa autoridade. `app/ponto/turnos.py:2868`; `app/ponto/turnos.py:2888`; `app/ponto/services/fechamento.py:632`; `app/folha/export.py:499`. |
| Batida: correção/retratação | `anotar` grava campos existentes; `retratar_batida` grava `aprovada=False` e `retratada_em`; admin chama `retratar_humano.retratar`, exigindo permissão e motivo ativo. `app/ponto/registro_batida.py:255`; `app/ponto/registro_batida.py:274`; `app/ponto/registro_batida.py:287`; `app/ponto/services/retratar_humano.py:163`. | Apuração exclui retratada; exibição de marcação conserva cru por contrato do próprio leitor. `app/ponto/turnos.py:2871`. |

Pontos materiais para a pergunta 4:

- “Descartar a batida” precisa significar **retratar**, preservando o registro: a porta declara “batida não se apaga”. `app/ponto/registro_batida.py:289`.
- A ação humana pode mudar dinheiro: a própria permissão explica que retratar “tira minuto da apuração”; retratação agenda reprocessamento do fechamento. `app/ponto/services/retratar_humano.py:134`; `app/ponto/registro_batida.py:297`; `app/ponto/services/fechamento.py:1809`.
- Ajustar ausência também muda insumo da folha e invalida projeções dos intervalos anterior e novo; não é ação financeiramente neutra. `app/ponto/services/ausencia.py:154`; `app/ponto/services/ausencia.py:669`; `app/ponto/services/ausencia.py:1123`.
- Não afirmar proteção genérica de toda exportada: evento pode recalcular exportada; a guarda concreta vigente é 09/2026 nas empresas que exportaram. `app/ponto/services/fechamento.py:1790`; `app/ponto/services/fechamento.py:36`; `app/ponto/services/fechamento.py:558`.
- Criação institucional de batida tem guarda de competência aprovada/exportada; fato de chão não é barrado. `app/ponto/registro_batida.py:147`; `app/ponto/registro_batida.py:156`; `app/ponto/janelas.py:75`. Essa guarda não é chamada pelo corpo de retratação humana inspecionado. `app/ponto/services/retratar_humano.py:150`.

Condição literal do FATO_EM_AUSENCIA é mais estreita que “ausência registrada”: cobertura **aprovada**; parcial sem período produz outro código CADASTRO; parcial fora não produz FATO_EM_AUSENCIA; há ramo com batida sem par e realizado zero. `app/ponto/supra_juiz.py:268`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:403`; `app/ponto/supra_juiz.py:456`.

## 17/11 10:11 · sessao c31d5ab43975 · RESSUSCITAR-DEVOLVE-MEDIDA · sonda · origem=Code

O modo `--sonda` executa o envelope antes de fornecer o dado raspado ao Codex (`bin/codex_pergunta.sh:329`, `bin/codex_pergunta.sh:334`). Segue a saída recebida, sem nova execução [nao verificado]:

```text
sonda_leitura: sonda.py [5cd5c1baa70789e6] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO 2174
CLASSES {'entregue': 1116, 'aberta': 1058} erros 0
ABERTAS_COLABS_DISTINTOS 64
MAIOR_POR_PESSOA 100 col883
TOP5 [('col883', 100), ('col697', 82), ('col510', 55), ('col855', 50), ('col925', 46)]
SOMA_CONFERE True
FIM_ITEM7_OK
```

Leitura: bate numericamente com as 2174 candidatas informadas das 09:04; diferença 0. Identidade das candidatas [nao verificado]. `sem_dia=0` e `desconhecido=0` inferidos do fechamento 1116 + 1058 = 2174; contadores não explicitados na saída [nao verificado].

ITEM7: universo 2174; entregue 1116; aberta 1058; sem_dia 0; desconhecido 0; colabs abertas 64; maior por pessoa 100 [nao verificado]

## 17/11 10:11 · sessao 1f02adf6c2a6 · - · - · origem=Ronald (tmux)

**Item 3: sugiro PROCEDE (AVAL-DEVOLVIDO), pela premissa de encaminhamento atual; o corte de dono CADASTRO em si cabe na lei.** L-099 distingue ESTRUTURA/CADASTRO/BATIDA e manda apenas ESTRUTURA à construção; não fixa FATO_EM_AUSENCIA como CADASTRO. Essa escolha continuava aberta no registro. `app/docs/LEIS.md:122`; `app/docs/RELATO.md:352`; `app/docs/RELATO.md:422`.

1. **Premissas e caminho atual.** O juiz `dono_da_divergencia` não recebe ausência/FATO_EM_AUSENCIA e ainda pode devolver BATIDA por turno aberto/paridade ou ESTRUTURA pelo residual; `InsumosDoDono.dono(data)` também não monta tal insumo. Portanto o novo corte ainda precisa chegar ao juiz existente. `app/ponto/management/commands/e6_oraculo.py:53`; `app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:182`.

   Há sinal A9 na lista Cadastro x Realidade: rótulo “ausencia aprovada com batida no dia”, dono literal “juiz”; sua seleção exige `CelulaDia.ausencia_id` e pelo menos duas batidas. A lista o apresenta agregado, sem `data` ou `dono_do_dia`, e a ação cai no padrão **“Com o sistema: em cura (nao e do cadastro)”**, sem URL. Isso não comprova o caminho prometido de cada dia ao admin para corrigir ausência ou retratar batida. `app/ponto/services/esmeril_espelho.py:23`; `app/ponto/services/esmeril_espelho.py:133`; `app/escala/services/cadastro_realidade.py:923`; `app/escala/services/cadastro_realidade.py:728`.

   O leitor do vigia consulta exatamente a lista Cadastro x Realidade; `dias_na_lista` exige assinatura datada e o balde `fio_do_admin` só aceita dono BATIDA/CADASTRO nessa lista. A9 não satisfaz essas condições. `app/chamados/services/vigia_fio.py:189`; `app/chamados/services/vigia_fio.py:200`; `app/chamados/services/vigia_fio.py:261`; `app/escala/services/cadastro_realidade.py:1047`.

   Existe outro caminho limitado: a tarefa expõe `VALIDAR/ADMIN` para dia **não exportado**, sem trabalho, veredito `fato_em_ausencia` e nenhuma pergunta viva C6. Isso não é atribuição do dono L-099 nem ida geral à lista Cadastro x Realidade; a composição posterior ainda exige alcançabilidade da ação administrativa. `app/chamados/services/tarefa_do_dia.py:357`; `app/chamados/services/tarefa_do_dia.py:654`.

2. **Colisão com cortes.** Dar CADASTRO a essa classe resolve escolha aberta e respeita a distinção de L-099; não permite redefinir o que é FATO_EM_AUSENCIA. Ausência parcial sem período é outra classe, nunca FATO; batida fora do período não é FATO; hora ilegível sem hora conhecida dentro mantém FATO por corte posterior. `app/docs/LEIS.md:122`; `app/docs/RELATO.md:352`; `app/docs/CORTES.md:386`; `app/docs/CORTES.md:401`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:403`; `app/ponto/supra_juiz.py:413`.

3. **Literal que não fecha.** “Batida em dia com ausência registrada” é mais amplo que o código: a classificação exige cobertura **aprovada**, condições do dia e tratamento do período parcial; ausência aberta pode suspender cobrança sem ser cobertura financeira aprovada. A expressão deve nomear a classe lavrada pelo juiz, não toda coexistência de registro e batida. `app/ponto/supra_juiz.py:392`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:450`; `app/ponto/turnos.py:2523`.

   A condição de esperar cruzamentos permanece válida como condição do anexo; a existência atual das interseções autenticadas B2+delta/F2 depende das listas não disponíveis. [nao verificado]

4. **Exportada/dinheiro/!.** Registrar dono e brief não exige corrigir dados; a correção futura por humano muda insumo de cálculo. A porta de ausência acorda projeção quando muda insumo da folha; retratar batida tira minuto da apuração e exige usuário/permissão. Não converter o corte em execução desses atos ou autorização em exportada: L-092 exige conservar gravado/Pauta DP. `app/ponto/services/ausencia.py:989`; `app/ponto/services/ausencia.py:1112`; `app/ponto/services/retratar_humano.py:131`; `app/docs/LEIS.md:125`.

**Campos ausência/status e batida, para a consolidação:**

- **Ausência/status — escritores:** `criar_ausencia`; edição de tipo/data por `editar_ausencia_registro`; status por `mudar_estado`, que valida transição e grava; `decidir` usa essa porta e grava período/carimbos no mesmo ato. Views admin chamam essas portas para aprovação, edição do período da aprovada e cancelamento. `app/ponto/services/ausencia.py:262`; `app/ponto/services/ausencia.py:614`; `app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:975`; `app/ponto/services/ausencia.py:1087`; `app/ponto/services/ausencia.py:1101`; `app/ponto/views.py:2788`; `app/ponto/views.py:2811`; `app/ponto/views.py:2886`.
- **Ausência/status — leitores:** cobertura para folha filtra `aprovada`, cobrança usa não rejeitada; suprajuiz lê cobertura/período; tarefa lê status da absorção e cobertura; vigia distingue ausência aberta. `app/ponto/turnos.py:2523`; `app/ponto/supra_juiz.py:395`; `app/chamados/services/tarefa_do_dia.py:178`; `app/chamados/services/tarefa_do_dia.py:309`; `app/chamados/services/vigia_fio.py:256`.
- **Batida — retratação:** `retratar_humano.retratar` verifica usuário, permissão e motivo cadastrado, depois chama `registro_batida.retratar_batida`; esta grava `aprovada=False` e `retratada_em`, preservando o registro. `app/ponto/services/retratar_humano.py:122`; `app/ponto/services/retratar_humano.py:150`; `app/ponto/services/retratar_humano.py:179`; `app/ponto/registro_batida.py:287`.
- **Batida — leitores relevantes ao item:** `InsumosDoDono` lê `batidas_apuraveis` e turnos; esmeril conta batidas por dia para A9; suprajuiz lê realizado/órfãs e cobertura para FATO. `app/ponto/management/commands/e6_oraculo.py:150`; `app/ponto/management/commands/e6_oraculo.py:156`; `app/ponto/services/esmeril_espelho.py:120`; `app/ponto/services/esmeril_espelho.py:133`; `app/ponto/supra_juiz.py:392`.

## 17/11 10:10 · sessao 0f381d53517e · - · - · origem=Ronald (tmux)

- **Motivo declarado no código:** “SELO DE PORTA, nao de pipeline (28/09)”; antecede export com tela == PDF == fechamento == TXT e zero divergência no universo TXT. O comentário diz que roda no ato/manual antes, e que agendar mediria competência que ninguém exportará naquele dia, enchendo log de verde. Também registra que os dois contadores anteriores existiam soltos e ninguém os rodava juntos. `app/config/crons.py:887-897`.
- **RELATO confirma como a classificação nasceu:** o contrato `test_todo_command_tem_casa` exigiu casa em `CRONS` ou declaração motivada em `FORA_DE_PIPELINE`; a declaração foi “selo de PORTA, nao de pipeline”, com a mesma justificativa contra agendamento. Isso é relato da decisão de implementação, não prova de aval nominal de Ronald. `app/docs/RELATO-ARQUIVO.md:5014-5019`; aval nominal para essa classificação: **[não verificado]**.
- **Origem funcional no RELATO:** os dois contadores já existiam e não eram rodados juntos; foi criado um comando com uma pergunta/uma resposta. O registro de 28/09 publica o selo verde nos 205 do TXT, sem allowlist. `app/docs/RELATO-ARQUIVO.md:4988-5008`.
- **BACKLOG registra o resultado, não uma proibição geral de cron:** `SELO-TXT`, fechado em 28/09, exige igualdade dos quatro leitores no universo TXT e registra selo verde nos 205; “Export so depois disto, com o `!`”. `app/docs/BACKLOG.md:1203`.
- **CORTES:** não localizei entrada específica que mande manter esse selo em `FORA_DE_PIPELINE` ou proíba cron diário; tal autoria/mandato é **[não verificado]**. O arquivo se declara registro dos cortes/avais de Ronald e é gerado de CORTES.json. `app/docs/CORTES.md:1-7`. As entradas sobre “carimbo da porta cega” são outra decisão: conta vazia versus turno aberto no ato de aprovação, não a periodicidade do selo de leitores. `app/docs/CORTES.md:315`, `app/docs/CORTES.md:317`.
- **LAPIDES:** não localizei lápide específica sobre a escolha `FORA_DE_PIPELINE`; relação direta com essa decisão: **[não verificado]**. O arquivo declara conter histórias copiadas do CLAUDE, e não um catálogo completo de decisões. `app/docs/LAPIDES.md:7-21`. A lápide 43 fala de selos de host que ninguém chamava; não demonstra a razão deste selo de export. `app/docs/LAPIDES.md:501-506`.
- **Limite importante:** “roda no ATO” é a intenção registrada em crons/RELATO; se a view realmente mede/lavra durante o request deve ser respondido pelo censo de chamadas, não inferido desse comentário. `app/config/crons.py:889-896`; `app/docs/RELATO-ARQUIVO.md:5016-5019`.
- Git log/autoria histórica do sítio: **[não verificado]**, conforme indisponibilidade fornecida.

## 17/11 10:10 · sessao df5b7b1a54c1 · - · - · origem=Ronald (tmux)

(d) Texto literal de ausência do carimbo, concatenando as strings de `app/folha/porta_export.py:812-813`:

```text
A PORTA DO EXPORT NAO FOI MEDIDA HOJE para %s. O TXT nao sai sem a prova de que a tela, o PDF, o fechamento e o TXT estao no mesmo numero. Rode: %s
```

Os `%s` são `empresa.razao_social` e `_cmd`; `_cmd` é construído em `app/folha/porta_export.py:809-810`:

```text
docker exec saas_core python manage.py tenant_command selo_leitores_no_mesmo_numero --schema=juliani --mes %02d --ano %s
```

A cadeia de tela é:

- Botão literal `Gerar TXT` em `app/templates/folha/index.html:109`, dentro do form POST para `folha:gerar_txt` em `app/templates/folha/index.html:63`; rota para `gerar_dominio_txt` em `app/folha/urls.py:14`.
- A view chama `conferir(empresa, mes, ano)` e passa o retorno intacto como `msg=_barra`, com `ok=False` e redirect para `folha:exportar`: `app/folha/views.py:128-133`.
- `ok=False` vira tipo `erro`: `app/core/respostas.py:50-51`; em POST normal, registra Django message e redireciona: `app/core/respostas.py:68-71`.
- O destino renderiza `folha/exportar.html`: `app/folha/views.py:92`; o template exibe a mensagem por `{{ m }}`: `app/templates/folha/exportar.html:10`.
- Caso receba `HX-Request: true`, o helper devolve 204 e `HX-Trigger` com `showToast`, mesmo `msg`: `app/core/respostas.py:63-66`. O form de `Gerar TXT` citado usa POST HTML normal: `app/templates/folha/index.html:63`.

(e) Evidência para o pré-desenho:

- `CLAUDE.md:89`: `saas_ui = todas as telas (gthread, timeout=30: trabalho longo vai pra job)`.
- Config literal: `worker_class = "gthread"`, `threads = 4`, `timeout = 30`, `graceful_timeout = 30`: `app/gunicorn.conf.py:19-22`.
- A justificativa histórica no comentário da view é medir cinco perguntas custar dois espelhos completos por colaborador, `~100 s para os 205 do TXT`, considerado inaceitável no POST; command mede/lavra e view confere: `app/folha/views.py:124-127`. Isso não mede competência 11, tempo atual ou empresas [não verificado].
- O código não demonstra que todo request gthread com mais de 30 s morre obrigatoriamente; esse resultado literal é [não verificado]. Pode apresentar risco de request longo/ocupação de threads e conflito com a orientação operacional citada: `CLAUDE.md:89`; `app/gunicorn.conf.py:19-22`.

## 17/11 09:50 · sessao 1e4b3d0bf14d · CONTADOR-CODEX-OCIOSO · verificador · origem=Code

**A volta 2 ainda reprova:** há reparos efetivos, mas entradas de sessão continuam podendo derrubar o ESTADO, o disjuntor mantém outro relógio/borda e quatro mutantes recebem o selo após exceções convertidas em `FALHA:`. (`bin/codex_limite_estado.py:325`, `bin/codex_limite_estado.py:448`, `bin/gerar_estado.py:270`, `bin/disjuntor.py:2036`, `bin/disjuntor.py:2132`, `bin/tests/test_codex_ociosidade.sh:200`)

Conferi por leitura e simulação dos casos em memória, com arquivos e PIDs simulados. Execução integral do selo no host e ocorrência dos defeitos em produção: [nao verificado].

**1) Os achados de v1.r foram resolvidos?**

| Achado de v1.r | Situação no código atual |
|---|---|
| Segundo juiz em `ultimas_chamadas` | **Resolvido na composição principal.** `ler()` compartilha `_foto`; ela calcula `_em_curso`, e últimas chamadas lê o resultado. (`bin/codex_limite_estado.py:93`, `bin/codex_limite_estado.py:94`, `bin/codex_limite_estado.py:153`, `bin/codex_limite_estado.py:176`) |
| Garantia de carimbo quando nasce ABERTO | **Não demonstrada.** A fixture escreve a fila; produtor real e procedimento de abertura: [nao verificado]. (`bin/tests/test_codex_ociosidade.sh:39`) |
| Truncamento de 30min59s | **Resolvido para segundos inteiros:** 1859 supera 1800, com caso explícito. Ainda há truncamento subsegundo: 30min00,500s vira 1800 e não acusa. (`bin/codex_limite_estado.py:51`, `bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:296`, `bin/tests/test_codex_ociosidade.sh:84`) |
| Janelas de 24h divergentes | **Parcial.** Limite, papéis e contexto horário usam `_na_janela`; o censo do disjuntor mantém a borda inferior inclusiva. (`bin/codex_limite_estado.py:59`, `bin/codex_limite_estado.py:83`, `bin/codex_limite_estado.py:164`, `bin/codex_limite_estado.py:283`, `bin/disjuntor.py:2132`) |
| Linhas curtas, hora inválida e tipo desconhecido ignorados | **Resolvido para essas classes na ociosidade:** são contabilizados e produzem SEM DADO. A validação dos demais campos continua incompleta. (`bin/codex_limite_estado.py:125`, `bin/codex_limite_estado.py:129`, `bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:275`) |
| PID inválido ou erro do SO tratado como morto | **Resolvido para chamadas sem fim:** retorna `None`, que torna a foto SEM DADO. (`bin/codex_limite_estado.py:188`, `bin/codex_limite_estado.py:198`, `bin/codex_limite_estado.py:279`) |
| `chamadas.log` sem permissão derruba `ler()` | **Resolvido nesse caminho:** a guarda envolve foto, papéis e últimas chamadas. A segurança do caminho completo continua incompleta por causa das sessões. (`bin/codex_limite_estado.py:92`, `bin/codex_limite_estado.py:95`, `bin/codex_limite_estado.py:89`, `bin/codex_limite_estado.py:325`) |
| Executor certifica exceção como morte | **Parcial:** rejeita traceback e `EXCECAO:`, mas os próprios casos convertem exceções em `FALHA:`, aceitas pelo executor. (`bin/tests/test_codex_ociosidade.sh:51`, `bin/tests/test_codex_ociosidade.sh:134`, `bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:202`) |
| M2 dependia da exceção `None > 30`; M11 dependia do motivo | **Corrigidos nos mutantes atuais:** M2 muda o requisito para mais de um ABERTO; M11 transforma erro em registro vazio, alterando o estado esperado. (`bin/tests/test_codex_ociosidade.sh:208`, `bin/tests/test_codex_ociosidade.sh:217`, `bin/tests/test_codex_ociosidade.sh:60`, `bin/tests/test_codex_ociosidade.sh:128`) |
| Diretório era caso vacuamente “ausente”; faltavam bordas | **Corrigidos:** fila diretório chega ao `open`; existem casos de 30min59s e início igual a `agora`. (`bin/codex_limite_estado.py:214`, `bin/codex_limite_estado.py:217`, `bin/tests/test_codex_ociosidade.sh:84`, `bin/tests/test_codex_ociosidade.sh:93`, `bin/tests/test_codex_ociosidade.sh:113`) |
| `rc` continha código ou situação sintética | **Separação implementada:** tupla com cinco campos e consumidor adaptado. Entretanto, o código não valida que o `rc` recebido seja numérico. (`bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:178`, `bin/codex_limite_estado.py:179`, `bin/codex_limite_estado.py:486`) |

**2) Quem escreve, quem lê e quais significados compartilham?**

| Campo/fonte | Quem escreve | Quem lê e significado efetivo |
|---|---|---|
| `chamadas.log/ini`: hora, obra, papel, hash, PID, pergunta | Wrapper: hora sem offset, hash com 12 caracteres, PID `$$`, pergunta raspada; grava antes da espera por slot. (`bin/codex_pergunta.sh:295`, `bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:364`) | `_registro` interpreta hora, obra, papel e chave `(obra,hash,pid)`; não conserva a pergunta. Portanto, o início inclui espera do wrapper, sem comprovar execução do Codex. (`bin/codex_limite_estado.py:124`, `bin/codex_limite_estado.py:135`, `bin/codex_limite_estado.py:137`, `bin/disjuntor.py:2073`) |
| `chamadas.log/fim`: hora, chave, papel, sessão, rc, duração | `_saida`, pelo trap EXIT após `INI=1`; duração em segundos pela diferença de epoch. (`bin/codex_pergunta.sh:265`, `bin/codex_pergunta.sh:269`, `bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:273`, `bin/codex_pergunta.sh:282`) | Parser valida hora/tamanho, mas conserva somente sessão, rc e duração como texto. `_em_curso` usa existência da chave; últimas usa rc; procedência e tokens usam sessão. A hora do fim é descartada, sem conferência de fim posterior ao início. (`bin/codex_limite_estado.py:129`, `bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:175`, `bin/codex_limite_estado.py:205`, `bin/codex_limite_estado.py:306`, `bin/codex_limite_estado.py:403`) |
| `fila.md`: estado e carimbo | Escrita demonstrada na fixture; escritor real: [nao verificado]. (`bin/tests/test_codex_ociosidade.sh:39`) | `ler_fila_reserva` exige estado conhecido e data+hora para ABERTO; produz quantidade, idade em segundos inteiros e erro. Ociosidade consome os três. (`bin/codex_limite_estado.py:227`, `bin/codex_limite_estado.py:235`, `bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:251`, `bin/codex_limite_estado.py:287`) |
| `limite.log`: hora, obra, mensagem | Wrapper grava uma linha quando reconhece limite/indisponibilidade. (`bin/codex_pergunta.sh:451`, `bin/codex_pergunta.sh:452`, `bin/codex_pergunta.sh:456`) | `ler()` exige três campos e hora válida; conta linhas, sem usar obra/mensagem para validar a classificação. Publica total, janela, última hora e inválidas. (`bin/codex_limite_estado.py:73`, `bin/codex_limite_estado.py:75`, `bin/codex_limite_estado.py:81`, `bin/codex_limite_estado.py:97`) |
| Registro/foto internos | `_registro` produz existência, inícios, fins, descartadas e futuras; `_foto` acrescenta `em_curso=True/False/None`. (`bin/codex_limite_estado.py:105`, `bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:153`) | `ler()` compartilha o registro com papéis, últimas e ociosidade; esta consome os contadores de erro e o valor trivalente. (`bin/codex_limite_estado.py:94`, `bin/codex_limite_estado.py:101`, `bin/codex_limite_estado.py:273`, `bin/codex_limite_estado.py:280`) |
| Tupla `(hora,obra,papel,rc,situacao)` | `ultimas_chamadas`: `rc=None` sem fim; situação em campo próprio. (`bin/codex_limite_estado.py:177`, `bin/codex_limite_estado.py:179`) | `_ultimas_txt` desempacota cinco campos; testes verificam situação no índice 4 e rc no 3. (`bin/codex_limite_estado.py:486`, `bin/tests/test_codex_ociosidade.sh:67`, `bin/tests/test_codex_ociosidade.sh:71`) |

Outros consumidores de chamadas são procedência no painel, tokens por papel e censo do disjuntor; `_linhas_chamadas` entrega somente inícios/fins, eliminando os contadores de entradas descartadas. (`bin/gerar_codex_estado.py:176`, `bin/codex_limite_estado.py:399`, `bin/disjuntor.py:2126`, `bin/codex_limite_estado.py:145`, `bin/codex_limite_estado.py:146`)

**Não há contrato temporal uniforme entre todos:** o módulo usa `(agora−24h,agora]`, mas o censo usa `[agora−24h,agora]`. O disjuntor também chama a contagem por papel sem transmitir seu `agora`. (`bin/codex_limite_estado.py:59`, `bin/disjuntor.py:2132`, `bin/disjuntor.py:2036`, `bin/codex_limite_estado.py:159`)

Os produtores gravam hora sem offset; o leitor usa `strptime` ingênuo e relógio local. Igualdade efetiva dos fusos dos processos: [nao verificado]. (`bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:308`, `bin/codex_pergunta.sh:456`, `bin/codex_limite_estado.py:63`, `bin/codex_limite_estado.py:124`)

**3) Ainda existe entrada virando zero/ok ou leitura derrubando o ESTADO? Sim.**

- **`fim` malformado ainda vira conclusão válida.** Na simulação, `rc=lixo` e `dur=lixo` foram aceitos, encerraram a chave e produziram `em_curso=0`, `estado=ok` com fila sem ABERTO; últimas publicou `('lixo','terminou')`. O parser não valida esses campos. (`bin/codex_limite_estado.py:129`, `bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:205`, `bin/codex_limite_estado.py:273`, `bin/codex_limite_estado.py:296`)
- **JSON válido com estrutura errada derruba a leitura semanal.** Por exemplo, `{"payload":[1],"rate_limits":true}` produz `AttributeError` no acesso a `.get`. O tratamento externo dessa leitura captura somente `OSError`; `gerar_estado.py` não protege a chamada. (`bin/codex_limite_estado.py:325`, `bin/codex_limite_estado.py:89`, `bin/gerar_estado.py:270`)
- **`used_percent="lixo"` derruba a composição.** O leitor conserva o valor e `_semanal_txt` chama `float`, produzindo `ValueError` fora das guardas de leitura. (`bin/codex_limite_estado.py:331`, `bin/codex_limite_estado.py:448`, `bin/codex_limite_estado.py:453`, `bin/gerar_estado.py:270`)
- **Log ausente ainda aparece como zero nas seções auxiliares:** papéis zerados e últimas vazias; a ociosidade corretamente permanece SEM DADO. (`bin/codex_limite_estado.py:115`, `bin/codex_limite_estado.py:161`, `bin/codex_limite_estado.py:174`, `bin/codex_limite_estado.py:270`)

A queda do gerador continua propagando saída 1 para `relato.sh`. (`bin/relato.sh:38`)

**4) Cada mutante morre pela propriedade?**

Na reprodução dos casos Python em memória, o original passou e os 25 mutantes divergiram. Isso não comprova a execução integral do shell nem permissões/PIDs reais: [nao verificado]. [nao verificado]

| Mutante | Divergência observada e asserção correspondente |
|---|---|
| M1 | Chamada viva passa a ACUSA. (`bin/tests/test_codex_ociosidade.sh:62`, `bin/tests/test_codex_ociosidade.sh:207`) |
| M2 | Um ABERTO antigo deixa de acusar. (`bin/tests/test_codex_ociosidade.sh:60`, `bin/tests/test_codex_ociosidade.sh:208`) |
| M3 | 30min00s e 10min passam a acusar. (`bin/tests/test_codex_ociosidade.sh:83`, `bin/tests/test_codex_ociosidade.sh:85`, `bin/tests/test_codex_ociosidade.sh:209`) |
| M4, M8 | PID morto conta como vivo ou recebe situação “em curso”. (`bin/tests/test_codex_ociosidade.sh:65`, `bin/tests/test_codex_ociosidade.sh:67`, `bin/tests/test_codex_ociosidade.sh:210`, `bin/tests/test_codex_ociosidade.sh:214`) |
| M5, M6, M7 | Sem carimbo, fila ausente ou log ausente passam a `ok`. (`bin/tests/test_codex_ociosidade.sh:75`, `bin/tests/test_codex_ociosidade.sh:79`, `bin/tests/test_codex_ociosidade.sh:80`, `bin/tests/test_codex_ociosidade.sh:211`) |
| M9, M10 | Fila malformada, diretório ou sem permissão passa a `ok`. (`bin/tests/test_codex_ociosidade.sh:113`, `bin/tests/test_codex_ociosidade.sh:123`, `bin/tests/test_codex_ociosidade.sh:126`, `bin/tests/test_codex_ociosidade.sh:215`, `bin/tests/test_codex_ociosidade.sh:216`) |
| M11, M12, M13, M14 | Log ilegível/corrompido ou PID desconhecido passa de SEM DADO para ACUSA. (`bin/tests/test_codex_ociosidade.sh:128`, `bin/tests/test_codex_ociosidade.sh:145`, `bin/tests/test_codex_ociosidade.sh:148`, `bin/tests/test_codex_ociosidade.sh:154`, `bin/tests/test_codex_ociosidade.sh:217`) |
| M15 | 30min59s deixa de acusar. (`bin/tests/test_codex_ociosidade.sh:84`, `bin/tests/test_codex_ociosidade.sh:221`) |
| M16, M18 | Contagens incluem a borda inferior ou excluem `agora`. (`bin/tests/test_codex_ociosidade.sh:97`, `bin/tests/test_codex_ociosidade.sh:99`, `bin/tests/test_codex_ociosidade.sh:222`, `bin/tests/test_codex_ociosidade.sh:224`) |
| M17 | Log futuro deixa de produzir SEM DADO. (`bin/tests/test_codex_ociosidade.sh:102`, `bin/tests/test_codex_ociosidade.sh:223`) |
| M19 | Situação sintética entra em rc. (`bin/tests/test_codex_ociosidade.sh:72`, `bin/tests/test_codex_ociosidade.sh:225`) |
| M21 | Papéis/últimas publicam zero ou ausência de registro após erro. (`bin/tests/test_codex_ociosidade.sh:137`, `bin/tests/test_codex_ociosidade.sh:139`, `bin/tests/test_codex_ociosidade.sh:227`) |
| M24 | Contador de linhas inválidas fica zero em vez de dois. (`bin/tests/test_codex_ociosidade.sh:169`, `bin/tests/test_codex_ociosidade.sh:230`) |
| **M20, M22, M23, M25** | **Morrem exclusivamente por exceção convertida em `FALHA: ... LEVANTOU`.** (`bin/tests/test_codex_ociosidade.sh:51`, `bin/tests/test_codex_ociosidade.sh:134`, `bin/tests/test_codex_ociosidade.sh:165`, `bin/tests/test_codex_ociosidade.sh:176`) |

Os quatro últimos verificam uma regressão útil de “nunca levanta”, mas **não satisfazem o critério pedido de morte sem exceção**. O executor não reconhece essa origem porque rejeita apenas traceback/`EXCECAO:` e depois aceita qualquer `FALHA:`. Nenhum dos outros mutantes depende somente de mudança no motivo. (`bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:202`, `bin/tests/test_codex_ociosidade.sh:53`, `bin/tests/test_codex_ociosidade.sh:169`)

Há duas ressalvas adicionais:

- **M8 não prova unicidade.** Um mutante adicional que troca a leitura de `em_curso` por nova chamada a `_em_curso(c,reg['fins'])` sobreviveu aos casos em memória: reintroduz a consulta duplicada, mas conserva as respostas com PIDs estáveis. M8 atual introduz situação errada, sem medir quantidade de avaliações. (`bin/codex_limite_estado.py:153`, `bin/codex_limite_estado.py:176`, `bin/codex_limite_estado.py:207`, `bin/tests/test_codex_ociosidade.sh:214`)
- **O relaxamento `b>=1` para `b>=0` é equivalente sob os invariantes atuais:** zero ABERTO implica idade `None`, barrada por `s is not None`. Esse mutante adicional sobreviveu em memória; M2 atual usa `b>1`, uma alteração diferente. (`bin/codex_limite_estado.py:232`, `bin/codex_limite_estado.py:251`, `bin/codex_limite_estado.py:296`, `bin/tests/test_codex_ociosidade.sh:208`)

**5) Classes solicitadas**

- **Segundo juiz:** removido da composição atual; o selo ainda deixa sobreviver sua reintrodução correta. (`bin/codex_limite_estado.py:153`, `bin/codex_limite_estado.py:176`, `bin/tests/test_codex_ociosidade.sh:214`)
- **Relógio fora da borda:** permanece na chamada do disjuntor sem `agora`. (`bin/disjuntor.py:2036`, `bin/codex_limite_estado.py:159`)
- **Derivação paralela:** permanece na janela inclusiva do censo. (`bin/disjuntor.py:2132`, `bin/codex_limite_estado.py:59`)
- **Campo com dois significados:** separação estrutural corrigida; falta validar rc para impedir texto inválido de entrar nesse campo. (`bin/codex_limite_estado.py:139`, `bin/codex_limite_estado.py:179`, `bin/codex_limite_estado.py:486`)
- **Selo sem mordida:** parcialmente corrigido; persistem a certificação das quatro exceções e a falta de detecção da avaliação duplicada. (`bin/tests/test_codex_ociosidade.sh:51`, `bin/tests/test_codex_ociosidade.sh:134`, `bin/tests/test_codex_ociosidade.sh:200`, `bin/tests/test_codex_ociosidade.sh:214`)

VEREDITO: REPROVA -- entrada inválida ainda vira zero/ok, sessões podem derrubar o ESTADO e o selo aceita mortes por exceção (bin/codex_limite_estado.py:139, bin/codex_limite_estado.py:325, bin/codex_limite_estado.py:448; bin/gerar_estado.py:270; bin/tests/test_codex_ociosidade.sh:202).

## 17/11 09:48 · sessao 77c456e611f7 · - · - · origem=Ronald (tmux)

Reprodução em memória: baseline verde; todos os 25 mutantes atuais geram `FALHA`, sem traceback escapado. Execução integral do selo: [nao verificado]. [nao verificado]

- M1–M19 e M24 apresentam divergências em estado, contagem, idade ou campos; M21 detecta publicação de zero/ausência de SEM DADO. Nenhum desses depende somente do texto do motivo. (`bin/tests/test_codex_ociosidade.sh:53`, `bin/tests/test_codex_ociosidade.sh:67`, `bin/tests/test_codex_ociosidade.sh:137`, `bin/tests/test_codex_ociosidade.sh:169`)
- M20/M22/M23/M25 morrem exclusivamente porque exceções são convertidas pelo teste em `FALHA: ... LEVANTOU`. Isso verifica a propriedade “nunca levanta”, mas o executor aceita mortes por exceção, contrariando seu critério declarado. (`bin/tests/test_codex_ociosidade.sh:51`, `bin/tests/test_codex_ociosidade.sh:134`, `bin/tests/test_codex_ociosidade.sh:165`, `bin/tests/test_codex_ociosidade.sh:176`, `bin/tests/test_codex_ociosidade.sh:200`)
- A antiga vacuidade “fila é diretório” foi corrigida: `exists` permite chegar ao `open`, cujo `IsADirectoryError` produz SEM DADO. (`bin/codex_limite_estado.py:214`, `bin/codex_limite_estado.py:217`, `bin/codex_limite_estado.py:219`; `bin/tests/test_codex_ociosidade.sh:113`)
- As bordas 30min59s e início exatamente em `agora` agora têm comparações explícitas. (`bin/tests/test_codex_ociosidade.sh:84`, `bin/tests/test_codex_ociosidade.sh:93`, `bin/tests/test_codex_ociosidade.sh:97`)

Dois mutantes adicionais sobreviveram a todos os casos em memória:

1. Trocar `v = c['em_curso']` por `v = _em_curso(c, reg['fins'])`: reintroduz avaliação duplicada, mas os PIDs estáveis não revelam a consulta adicional. M8 testa rótulo errado, não unicidade. (`bin/codex_limite_estado.py:153`, `bin/codex_limite_estado.py:176`, `bin/codex_limite_estado.py:207`; `bin/tests/test_codex_ociosidade.sh:214`)
2. Trocar `b >= 1` por `b >= 0`: equivalente sob os invariantes atuais, pois `b=0` implica idade `None`, barrada por `s is not None`. M2 atual usa `b > 1`, que é diferente desse relaxamento. (`bin/codex_limite_estado.py:232`, `bin/codex_limite_estado.py:243`, `bin/codex_limite_estado.py:251`, `bin/codex_limite_estado.py:296`; `bin/tests/test_codex_ociosidade.sh:208`)

_6463 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
