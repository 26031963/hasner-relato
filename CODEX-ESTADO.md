# CODEX-ESTADO

> Ultima resposta processada: **20/11/2026 10:28** · publicado em **20/11/2026 10:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 20/11 10:28 · sessao 9cb397456650 · E4C-F5-CONSTROI · construtor · origem=Code

Implementei as duas correções da E-A1-V2 em `disputa_emissao.py` e acrescentei dois REDs com mutantes em memória ao arquivo de testes indicado. Conferi a sintaxe, a aplicação das mutações e a preservação dos testes anteriores. Execução dos REDs, dos mutantes e efeitos em produção [nao verificado]. A certificação fica com o Code e o opus final. [disputa_emissao.py:1483](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1483) · [test_e4c_f5_a1_leitura_sob_trava.py:97](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:97) [nao verificado]

### Diff por função

- **`reabrir_para_correcao`** consulta só os IDs dos donos antes de `trava_na_ordem`. Captura `_linhas` depois de `exigir_colaboradores_travados`, antes da escrita das decisões e do `descarimbar_em_lote`. O lote acorda a célula dessa leitura no mesmo `atomic`, conforme LASTRO-PARTE-3. [disputa_emissao.py:1626](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1626) · [disputa_emissao.py:1653](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1653)
- **`responder_pergunta_disputa_core`** relê `chamado_destino` junto de `data_evento` sob a trava do dono. Removi o `pop('decisoes')` antecipado e seu comentário. Com `ja=None`, a foto do passivo consulta `DecisaoPergunta.objects` quando há assinatura na projeção. `registrar` invalida o prefetch da mesma instância antes de a cauda chamar `resposta_display`. [disputa_emissao.py:1478](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1478) · [models.py:4276](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:4276) · [models.py:4381](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:4381) · [disputa_emissao.py:1565](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1565)

### Campos da resposta

| Campo | Escrita sob a trava do colaborador | Leitura depois da trava e decisão sobre o refresh |
|---|---|---|
| `resposta_colab` | Resposta humana. `disputa_emissao.py:1481,1494,1526` | A foto do passivo usa o texto anterior. Permanece. `models.py:4285–4294` |
| `respondida_em` | Resposta humana. `disputa_emissao.py:1481,1495,1526` | A foto do passivo usa o instante anterior para localizar a decisão. Permanece. `models.py:4276–4289` |
| `validada_em` | Validação chama `_carimbar_validacao` sob a trava. `validacao.py:168–175,737–745` | A foto do passivo e o ramo que limpa a assinatura usam o instante anterior. Permanece. `models.py:4290–4294`, `disputa_emissao.py:1487,1497–1508` |
| `validada_por_id` | A mesma validação grava o autor. `validacao.py:168–175,737–745` | A foto do passivo e a trilha guardam o autor anterior. Permanece. `models.py:4290–4294`, `disputa_emissao.py:1487,1539–1543` |
| `materializada_em` | `carimbar`, por exemplo na absorção sob a trava. `models.py:3317–3321,3348–3349,3372`, `materializacao.py:1060–1065` | `reabrir_por_resposta` verifica se há carimbo. Permanece. `models.py:3550–3552` |
| `via_resolucao` | O mesmo carimbo grava a via. `models.py:3317–3321,3348–3349,3372`, `materializacao.py:1060–1065` | A via governa o descarimbo e a guarda de saída do fato. Permanece. `models.py:3421–3428,3550–3552` |
| `data_evento` | Reapontamento e rechaveamento. `fio.py:919–942,977–981,1063–1066,1090–1096` | A cerca, o log e a materialização de ausência usam o dia. Permanece. `fio.py:1407–1425`, `disputa_emissao.py:1523–1524,1548–1551`, `materializacao.py:1019–1030` |
| `chamado_destino_id` | O reapontamento pode gravá-lo no mesmo `save` do dia. O reuso também grava destino. `fio.py:919–942,977–981`, `models.py:3060,3195–3221` | O resgate e a ausência usam a FK. Entrou como `chamado_destino`. `materializacao.py:1024–1030,1213–1225`, `disputa_emissao.py:1483–1484` |
| `materializada_por_id` | O mesmo carimbo grava o autor. `models.py:3317–3321,3348–3349,3372`, `materializacao.py:1060–1065` | Ficou fora. O descarimbo atribui `None` diretamente, sem consultar o autor anterior. `models.py:3423–3433,3550–3552` |
| `celula_id` | Rechaveamento e reuso. `fio.py:1063–1066,1087–1096`, `models.py:3060,3180–3182,3212–3222` | Ficou fora. Tanto o signal da resposta quanto `registrar` buscam a célula por queryset próprio. `signals.py:252–259`, `models.py:3445–3448,4388–4389` |
| `recusa_motivo` | Resposta de escape e recusa do admin. `disputa_emissao.py:1481,1523–1526`, `validacao.py:919–925` | Ficou fora. O escape atribui o motivo novo diretamente. `disputa_emissao.py:1512–1526` |
| `pk`, `disputa_id`, `conversa`, `motivo`, `pergunta`, `tipo_resposta`, `opcoes` | Os escritores localizados são de nascimento. Escrita posterior sob a trava para esses campos [nao verificado]. `models.py:3056–3060,3224–3232`, `disputa_emissao.py:448–460` | Ficaram fora. A identidade e a FK entram no registro e no signal, `conversa` entra na cerca, e os demais campos entram no log ou na nota. `models.py:3837–3863,4373`, `signals.py:229–240`, `fio.py:1414–1425`, `disputa_emissao.py:1548–1568` |

### Dono fora de `_donos`

Nas duas portas interpostas, o rechaveamento grava dia, célula e lavra, e o reapontamento grava destino, dia quando aplicável e lavra. O colaborador da pergunta é derivado da disputa na inserção. Uma porta de produção que troque o dono de uma pergunta existente [nao verificado]. [fio.py:977](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/fio.py:977) · [fio.py:1090](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/fio.py:1090) · [models.py:3697](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:3697) · [models.py:3738](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:3738)

Se outro dono aparecer, a primeira conferência recebe apenas `_donos`. A porta do lote reavalia os donos do queryset. Se o novo dono ainda estiver no lote e faltar no registro de travas da transação, ela lança `ColaboradorNaoTravado` antes do `UPDATE`. Se ele já estiver registrado por uma trava externa, a conferência passa. O diff mantém as chamadas e a ordem de trava existentes, e usa a conferência da própria porta para esse caso. [disputa_emissao.py:1629](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1629) · [models.py:3497](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:3497) · [trava_do_colaborador.py:55](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/trava_do_colaborador.py:55)

### REDs e comandos para o Code

- **RED C.** Interpõe o rechaveamento real e conta só os eventos posteriores àquela porta. Com a leitura antecipada, a falha esperada é a asserção da linha **144**. O evento da decisão aponta para a célula atual e o evento do lote aponta para a antiga. [teste:126](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:126) · [teste:144](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:144) [nao verificado]
- **RED D.** Interpõe o reapontamento real com o destino antigo já carregado na instância. Sem o destino no refresh, a falha esperada é a asserção da linha **168**. O banco tem o par novo, mas a instância combina o dia novo com o destino antigo. [teste:146](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:146) · [teste:168](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:168) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_a1_leitura_sob_trava --settings=config.settings.ci --noinput

CORTES_F2_MUTANTE=a1_linhas_antes_da_trava PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_a1_leitura_sob_trava.LeituraDeFioSobTravaTest.test_RED_C_reabertura_acorda_a_celula_rechaveada --settings=config.settings.ci --noinput

CORTES_F2_MUTANTE=a1_refresh_sem_destino PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_a1_leitura_sob_trava.LeituraDeFioSobTravaTest.test_RED_D_resposta_rele_dia_e_destino_juntos --settings=config.settings.ci --noinput
```

As edições ficaram nas duas funções e nos acréscimos ao arquivo de testes, conforme o escopo da volta. A corrida inversa pertence à porta de validação, que carrega a instância antes de adquirir a trava. Os REDs usam a interposição de `TestCase` para aferir a ordem das leituras. Bloqueio real entre conexões [nao verificado]. [validacao.py:674](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:674) · [validacao.py:731](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:731) · [test_e1_v9.py:119](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e1_v9.py:119) [nao verificado]

## 20/11 10:27 · sessao 9eef540820a5 · E4C-F5-CONSTROI · verificador · origem=Code

Revisei a cópia indicada como `8b0d0ddcf391843a6d25dfe2e428c5e2c06724ff`. O hash pelo Git está [nao verificado]. A árvore contém A1(a), com o relógio da reabertura após a trava, e A1(b), com a instância da resposta relida após a trava. O dado raspado informa A=0 e B=0 em 51.953 perguntas na sonda anterior. A remedição na árvore final, os testes ORM e os efeitos em produção estão [nao verificado]. `app/chamados/services/disputa_emissao.py:1478–1495,1628–1644` `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:32–88` `r.txt:1`

### Item 1 — Devolução

- **P1.** A guarda atual lê a coluna. Com resposta de hora, pergunta sem materialização e motivo de recusa, ela limpa B, em que só a coluna está preenchida. Em A, em que só a composta lê validação, o bloco é pulado. A fila SQL ainda seleciona pela coluna e depois confere a composta sob a trava. `app/chamados/services/validacao.py:185–194,262–283` `app/chamados/tests/test_e4c_f5_voltaD_portas.py:38–61,383–392` `app/chamados/services/materializacao.py:2111–2133`
- **P2.** A catraca aceita a guarda dupla. Confirmei em memória que `_vermelhos` retorna vazio para `composta(perg).validada_em or perg.validada_em`. O AST aceita a leitura da composta, as fotos reconhecidas de `antes`/`depois` e o acesso direto ao campo que a própria função atribui no mesmo receptor. Essa última regra é sintática e vale mesmo quando a atribuição vem depois da guarda. `getattr` cru continua acusado, e seleções `only`/`values*` ou `refresh_from_db` de campo cru são rastreadas quando alimentam decisões. A catraca tem **zero allowlist nominal**. `FUNCOES` é a lista dos sítios fiscalizados e inclui a devolução. O selo de vitalidade, separado, já tem uma entrada para essa função. O 1025 autoriza a guarda dupla em resposta ao 0545. O teste atual da volta D ainda exige **zero** linhas `DEVOLUCAO` em A. A classe vii do 0545 autoriza trocar essa expectativa na forma que ela delimita. `app/chamados/tests/test_e4c_f5_selo_catraca.py:11–26,70–79,94–105,116–137,152–197,265–307,334–376` `app/chamados/tests/test_contract_leitores_vitalidade.py:213–219,347–348` `app/chamados/tests/test_e4c_f5_voltaD_portas.py:383–392` `f5_ato_x_coluna_invariante_2011_0545.txt:3,6,9` `respostas_f5_travas_2011_1025.txt:4`
- **P3.** A troca literal da guarda deixa dois problemas. O refresh parcial pode conservar um prefetch antigo de `decisoes` e fazer a guarda perder A. Mesmo com leitura fresca, em A, numa disputa já aberta e com a foto padrão, a função grava a primeira `DEVOLUCAO`, compara duas fotos iguais da coluna e retorna `None` como se já tivesse devolvido. O evento `validacao_devolvida` fica sem rodar, embora a validação lida tenha mudado. Isso contraria o retorno documentado e deixa a mudança fora da trilha de antes/depois exigida pela L-007. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:371–381` `app/chamados/services/validacao.py:178–182,237–240,262–305` `app/chamados/models.py:4252–4258,4342–4351` `app/docs/LEIS.md:65`
- **P4.** O efeito direto é escrever pergunta e decisão e, conforme o caso, reabrir a disputa. A chamada de produção localizada faz o K6 antes do carimbo e de novo antes da devolução. O K6 cobre metades de intervalo. A função avulsa carece dessa guarda e a cerca genérica só atua dentro de ato cercado. O caminho de validação já pode acionar materialização e recálculo. A mudança literal da guarda fica em `validacao.py`, sem escrita direta em folha, escala, vínculo ou motor. Alcance efetivo em entregue e delta de dinheiro estão [nao verificado]. `app/chamados/services/validacao.py:270–305,737–785` `app/chamados/services/materializacao.py:808–832` `app/chamados/models.py:3691–3707` `app/chamados/services/fio.py:1394–1409` `app/docs/CORTES.md:520,530` `app/docs/LEIS.md:125,155`

### Item 2 — Par ato × coluna

**P1.** A composta devolve a validação de `leitura`. `VALIDACAO` preenche. `DEVOLUCAO`, `RECUSA`, `REABERTURA` e `RESPOSTA` com `resposta_mantem_validacao=False` limpam se vencerem na sequência. A ordem é por instante, depois linhas reais por PK, depois a linha sintética do mesmo instante. **`ABSORCAO` é neutra para validação.** Quando a absorção preenche a coluna, a composta pode enxergar a `VALIDACAO` sintética que a própria coluna produz. `app/chamados/models.py:4283–4295,4297–4317,4333–4362`

| Escrita na cópia | Outra metade na mesma transação |
|---|---|
| `_carimbar_validacao` grava coluna e `VALIDACAO`. | **Sim**, com o mesmo instante. `app/chamados/services/validacao.py:161–175` |
| `devolver_validacao_recusada` e `devolver_paridade_bug_par_103` zeram a coluna e gravam `DEVOLUCAO`. | **Sim**, nos ramos que gravam. A guarda atual da primeira porta exige coluna preenchida. `app/chamados/services/validacao.py:262–283,332–359` |
| Resposta humana grava `RESPOSTA(False)` e inclui a limpeza no `save` se a coluna relida estiver preenchida. | **Sim** nesse ramo, depois da trava. **Sem escrita da coluna** se ela já estiver nula, inclusive se houver validação vigente só por ato. A1(b) fecha a omissão por instância velha descrita na devolução anterior. `app/chamados/services/disputa_emissao.py:1478–1510,1528–1533` `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:56–88` |
| `reabrir_para_correcao` grava `REABERTURA` ou `RECUSA` e zera pelo `UPDATE` comum. `ressuscitar_perguntas` usa o mesmo `UPDATE` com `REABERTURA` marcada. | **Sim**, quando há ato e limpeza. A reabertura omite a linha se sua foto estiver vazia. A ressuscitação grava para o universo relido sob a trava. A1(a) já colhe o relógio da reabertura depois da trava. `app/chamados/services/disputa_emissao.py:1616–1657` `app/chamados/services/validacao.py:923–929` `app/chamados/models.py:1646–1674,1701–1722,3508–3511` |
| `_nascer` insere a coluna pelo `force_insert`. | **Sim** se recebe `validada_em` preenchida, pois promove `VALIDACAO` no mesmo `atomic`. Por padrão a coluna nasce nula. `app/chamados/models.py:2657–2659,3053–3060,3229–3234,4290–4295` |
| `PerguntaDisputa.carimbar` com `resposta='validar'`, texto e coluna nula escreve `validada_em`. Os chamadores desse ramo são a absorção, o materializador e o resolvedor S143. | **Sem `VALIDACAO` real pareada no ramo.** A absorção grava `ABSORCAO` no mesmo `atomic`. Materializador e S143 chamam a porta dentro de transação, com o efeito de validação vindo da própria coluna quando o ramo a preenche. `app/chamados/models.py:3344–3377` `app/chamados/services/materializacao.py:1060–1069,1928–1936,1981–1992` `app/chamados/services/disputa_emissao.py:1911–1919,2024–2033` |

O escritor físico da sequência é `registrar`. A resposta automática e a resposta pela trilha gravam `RESPOSTA(True)`, que preserva a validação. As promoções do passivo gravam linha real a partir da coluna existente, também no comando E4B e na porta L-130, que só zera os campos da resposta. Promover pode afetar a ordem com outro ato no mesmo instante. A migração histórica 0056 também muda a entrada do leitor ao atualizar `resposta_mantem_validacao`, sem escrita de `validada_em`. A sondagem da ressuscitação e o desfazer de `carimbar` só atribuem a coluna em memória. `app/chamados/models.py:1569–1594,3379–3386,4283–4295,4333–4351,4365–4375` `app/chamados/services/disputa_emissao.py:1680–1709` `app/chamados/services/validacao.py:429–441` `app/chamados/management/commands/backfill_fio_morto_e4b.py:147–150` `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:1–14`

- **P2.** O 1025 inclui expressamente o sentido ato → coluna, que faltava ao censo do 0545. Os selos vigentes cobram dono do campo e porta única da sequência. O corte da ressuscitação exige preservar o ato compensador marcado e a fila do colaborador. A L-130 exige conservar a limpeza conjunta de instante e texto. A F7 conserva a leitura sintética até o Contract. Isso importa porque o novo critério, se medir apenas a resposta da composta, pode aceitar a ausência da linha física que o 0545 manda o mutante denunciar. `respostas_f5_travas_2011_1025.txt:5,9` `f5_ato_x_coluna_invariante_2011_0545.txt:5–10` `app/chamados/tests/test_contract_dono_validada.py:26–71` `app/chamados/tests/test_e1_entradas.py:185–226` `app/docs/CORTES.md:544–545` `app/docs/LEIS.md:148,151` `app/chamados/management/commands/backfill_decisoes_pergunta.py:95–109`
- **P3.** Há duas condições distintas sob a palavra “par”. Em B, a devolução limpa uma coluna preenchida e a composta continua nula. Em A, a guarda dupla pode gravar uma devolução efetiva sobre uma coluna já nula. Ambas as transições restabelecem a equivalência, mas violam a exigência literal de que os dois valores mudem. Além disso, confirmei em memória que remover a linha `VALIDACAO`, deixando o carimbo, pode devolver exatamente o mesmo instante pela linha sintética. Portanto, A=0/B=0 e a classe vii, lidos só pela equivalência, podem deixar esse mutante sobreviver. O vigia desta cópia mede falta de linha física e divergência de texto, sem par A/B publicado. `app/chamados/models.py:4290–4295,4333–4362` `app/chamados/services/validacao.py:262–283` `app/chamados/tests/test_e4c_f5_voltaD_portas.py:38–61,383–392` `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197–216` `app/chamados/management/commands/vigiar_fio_morto.py:192–196,224–229,269–277` `f5_ato_x_coluna_invariante_2011_0545.txt:6–10`
- **P4.** O selo, por si, é estático. O critério universal alcança escritores no plantio, no S143 e na absorção. O S143 roda cercado, o K6 tem o recorte de intervalo, e a validação comum já chama recálculo. A migração histórica está fora dos sítios de execução da F5. O aval conserva a proibição de tocar `folha/` e de criar ato de reparo. Escrita nova em escala, vínculo ou motor e delta efetivo em exportada ou dinheiro estão [nao verificado]. `app/chamados/services/disputa_emissao.py:1911–1924,2024–2032` `app/chamados/services/materializacao.py:808–832,1868–1876,1981–1992` `app/chamados/services/validacao.py:737–757` `app/ponto/services/fechamento.py:1798–1824` `app/docs/LEIS.md:125,155` `f5_ato_x_coluna_invariante_2011_0545.txt:9`

### Item 4 — Absorção da ausência

- **P1.** Antes da trava, o caminho lê disputa, dia e destino do escape, chama a porta da ausência, captura `agora`, avalia a lista das irmãs de hora ainda sem carimbo e calcula `_via` pelo texto de cada irmã. Depois usa o mesmo `q`, `_via` e `agora` em `carimbar`, e registra o texto de `q` e os dois vínculos em `ABSORCAO`. A trava existente é a linha do **Colaborador**, por irmã, dentro do `atomic`, via `trava_na_ordem` e `select_for_update`. `carimbar` aplica o estado em memória, consulta o C6 e só persiste se ele aceitar. `app/chamados/services/materializacao.py:1019–1069` `app/chamados/juizes.py:1785–1803` `app/chamados/models.py:2830–2836,2868–2878,3344–3377`
- **P2.** O 1025 altera expressamente o A1 do 0945 nesse sítio ao manter o relógio comum. Isso preserva a chave usada por `perguntas_absorvidas_por`. O leitor busca o escape pelo tipo de ausência, colaborador e dia, encontra suas chaves de disputa e carimbo, e exclui perguntas com batida de origem viva. Quem o chama é `devolver_absorvidas_da_ausencia_rejeitada`, acionada pelo `on_commit` do signal de ausência. O teste O261 exige absorver escape e irmã muda, excluir a do outro dia da absorção e, após rejeitar, devolver as duas a pendente e vivas, reabrir a disputa e gravar uma trilha cujo antes contém `ATESTADO`. Outro selo exige `ABSORCAO.ato_em == materializada_em` em cada irmã absorvida. `retoma_2011_0945.txt:4` `respostas_f5_travas_2011_1025.txt:7,9` `app/chamados/services/materializacao.py:1106–1139,1159–1194` `app/ponto/signals.py:250–272` `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:76–104` `app/chamados/tests/test_e1_entradas.py:239–251` `app/docs/PROMPTS.md:1377–1378`
- **P3.** Reler a instância pode corrigir o texto velho após uma recusa, mas a expressão deixa em aberto se a seleção e `_via` também serão reavaliadas. `carimbar` pode sobrescrever um carimbo já existente. Há ainda uma divergência possível com a instância fresca. O lote colhe T1, uma resposta humana grava `RESPOSTA(False)` em T2 > T1 enquanto a irmã aguarda, e a absorção relê texto presente e coluna nula. Se C6 aceitar o carimbo, a coluna fica com T1, a validação sintética ordena antes da resposta de T2 e a composta fica nula. Isso produz B mesmo preservando o relógio comum. O teste O261 citado cobre a devolução do grupo, e a sequência concorrente no ORM está [nao verificado]. `app/chamados/services/materializacao.py:1033–1069` `app/chamados/services/disputa_emissao.py:1478–1510,1528–1533` `app/chamados/models.py:3344–3377,4283–4295,4333–4351` `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:87–104`
- **P4.** A releitura é local a `materializacao.py`. O fluxo já cria ou reutiliza ausência, grava os vínculos do ato e pode reconciliar chamado e fechar disputa. A ausência alimenta o cartório e o cálculo de faltas. O laço de absorção carece de veto explícito de exportada e a cerca genérica depende do contexto. O item mantém o leitor do grupo e os vínculos no escopo declarado, sem pedir edição de escala, vínculo cadastral ou motor. Efeito efetivo em entregue ou dinheiro [nao verificado]. `app/chamados/services/materializacao.py:1019–1032,1052–1089` `app/chamados/models.py:4201–4204` `app/chamados/services/fio.py:1394–1409` `app/ponto/signals.py:85–93,143–147` `app/ponto/services/fechamento.py:634–639,869–875` `respostas_f5_travas_2011_1025.txt:7,9`

ITEM 1: PROCEDE -- a catraca aceita a guarda dupla, mas o refresh pode esconder A e a primeira devolução em A pode sair como repetição, sem trilha de antes/depois. `app/chamados/tests/test_e4c_f5_selo_catraca.py:265–279` `app/chamados/tests/test_e4c_f5_voltaD_portas.py:371–381` `app/chamados/services/validacao.py:262–305`

ITEM 2: PROCEDE -- a mudança bilateral literal conflita com a limpeza de A/B, e a validação sintética pode esconder do teste a retirada da linha real. `app/chamados/services/validacao.py:262–283` `app/chamados/models.py:4290–4295,4333–4362`

ITEM 4: PROCEDE -- o relógio comum preserva a chave do grupo, mas ainda permite B após uma resposta humana posterior a esse relógio, mesmo com a irmã relida sob a trava. `app/chamados/services/materializacao.py:1033–1069,1126–1136` `app/chamados/services/disputa_emissao.py:1495–1510,1528–1533` `app/chamados/models.py:3349–3359,4333–4351`

## 20/11 10:18 · sessao eaff21486ef3 · E4C-F5-CONSTROI · verificador · origem=Code

**O diff corrige as duas corridas exercitadas pelos REDs. A leitura sob trava ainda é parcial.** Em `reabrir_para_correcao`, a foto da decisão já vinha do banco sob a trava na base. O diff move o relógio, mas a função ainda emite o evento de célula com a foto anterior à espera. Em `responder_pergunta_disputa_core`, a validação e a materialização passam a vir do banco sob a trava. A instância ainda pode combinar o dia relido com um destino antigo. `app/chamados/services/disputa_emissao.py:1480–1499,1527–1534,1630–1657` (base), `:1483–1513,1637` (diff).

O blob local de `app/chamados/services/disputa_emissao.py` coincide com o prefixo `8d3d3218b` do diff. A cópia não tem metadados Git para autenticar o hash do commit. Hash do commit, execução dos REDs, contagem medida de consultas e ocorrência em produção [nao verificado]. A revisão foi somente leitura.

## 1. Escritores e leitores

A trava explícita de ambas as funções é a linha de `Colaborador`, ou o conjunto dessas linhas em ordem de PK no lote. `trava_do_colaborador` delega a `trava_na_ordem`. Disputa, Chamado e Pergunta só são travados explicitamente quando passados à porta ou incluídos pela opção de universo. `app/chamados/models.py:2830–2848,2868–2904`.

| Campo | Escritores e trava |
|---|---|
| `resposta_colab`, `respondida_em` | Resposta humana no `save` sob o Colaborador, `disputa_emissao.py:1517–1534` na base e `:1480–1533` no diff. Reabertura, ressuscitação e devolução L-130 passam os dois campos à porta do lote, que exige os donos já travados. `app/chamados/services/disputa_emissao.py:1617,1637,1656`, `app/chamados/models.py:1651,1721–1722,3483–3511`, `app/chamados/services/validacao.py:430–441`. O nascimento aceita o par em `campos` e insere sob o Colaborador. `app/chamados/models.py:3053–3060,3229–3234`. `carimbar` também preenche `respondida_em` vazio quando recebe texto e modo `responder` ou `validar`. A trava vem do chamador, como no resolvedor de chamado, na absorção e no plantio. `app/chamados/models.py:3344–3372`, `app/chamados/services/disputa_emissao.py:1913–1920,2031–2032,2102–2105`, `app/chamados/services/materializacao.py:1060–1066,1987,2071–2072,2122–2132`. O upload automático escreve o par e o comando de limbo escreve só o instante. Nesses dois caminhos há `atomic` local sem aquisição explícita do Colaborador antes da escrita. `app/chamados/services/disputa_emissao.py:1671–1688,1700–1710`, `app/api/foto_recebida.py:38–51,65–74`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:65–70`. |
| `validada_em`, `validada_por` | `_carimbar_validacao` grava os dois e a linha `VALIDACAO`. Na porta pública e nos dois ramos de folga, recebe Colaborador, Disputas e Chamados já travados. A irmã do intervalo herda a trava da validação. `app/chamados/services/validacao.py:161–175,517–521,549–552,633–643,737–745,771–777`. `carimbar(resposta='validar')` preenche os nulos com texto, sob as travas dos chamadores acima. A resposta humana e a reabertura limpam ambos sob o Colaborador. A ressuscitação limpa ambos sob C → D → Ch → P. A devolução de recusa herda a trava de `validar_pergunta` no caminho de produção localizado. A devolução BUG-PAR toma C → D → Ch → P. O nascimento aceita assinaturas em `campos`. A FK do autor declara `SET_NULL` na exclusão de `User`. `app/chamados/models.py:1651,1721–1722,3229–3234,3354–3357,3655–3660`, `app/chamados/services/validacao.py:262–283,332–359,731–785`, `app/chamados/services/disputa_emissao.py:1617,1637,1656` e `:1481,1499–1510` (diff). |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo` atribui os três campos do carimbo em memória. Persistem por `carimbar`, `carimbar_identidade`, `descarimbar` e `descarimbar_em_lote`. O lote exige C e as portas de instância herdam a trava de quem chama. Além dos caminhos acima, há fecho e retratação de disputa sob C → D → Ch, com P explícita na retratação, dedup sob C → D → P, e descarimbo de relavra sob C → D → P. `app/chamados/models.py:2090–2112,2211–2216,2240–2248,3317–3372,3402–3410,3423–3433,3497–3511`, `app/chamados/services/fio.py:1484–1509`, `app/chamados/services/relavra_via.py:230–246`, `app/ponto/services/veredito_celula.py:174–197`. `via_resolucao` também é escrita sozinha por `encerrar_por_fato_no_espelho` e por `relavrar`. Os comandos de aplicação desses dois caminhos e o de reabertura por competência mostram `atomic` sem aquisição explícita de C no trecho que grava. `app/chamados/models.py:3513–3521,3554–3560`, `app/chamados/services/relavra_via.py:117–128`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:66–70`, `app/chamados/management/commands/relavrar_via_i6.py:100–102`, `app/chamados/services/competencia_trancada.py:275–282`. No reconciliador há C explícita para `VIA_CELULA_ABSOLVEU`. A cobertura por trava externa em todos os chamadores de `lavrar_sem_turno_aberto` e `encerrar_fantasma` fica [nao verificado]. `app/chamados/reconciliador.py:408,643–646,1200–1202,1673`. |
| `recusa_motivo` | A resposta humana grava no mesmo `save` sob C para os dois escapes. `recusar_pergunta` grava sob C antes de delegar a foto e a limpeza. `carimbar_identidade` grava o motivo recebido no dedup sob C → D → P e na colisão do materializador. O materializador também grava diretamente a pendência com motivo e a recusa por `_carimbar_recusa`. Essas rotinas herdam a trava do ato de validação ou plantio. A recusa de geometria do passivo sai do savepoint do dia, dentro do ato externo de plantio. `app/chamados/services/disputa_emissao.py:1514–1528` (diff), `app/chamados/services/validacao.py:919–929,737–745,771–785`, `app/chamados/models.py:3406–3410`, `app/chamados/services/fio.py:1484–1509`, `app/chamados/services/materializacao.py:1366–1372,1689–1709,2008–2009,2071–2072,2122–2132`, `app/chamados/services/passivo_validado.py:373–381,443,525–526,564–566`. |
| `data_evento` | A atribuição central é `gravar_dia_do_fato`. Persiste no nascimento sob C, no reapontamento que data uma pergunta sob C e no rechaveamento sob C → D → P. O comando de migração chama o rechaveamento. `app/chamados/services/fio.py:843–854,919–942,977–981,1059–1066,1090–1096`, `app/chamados/models.py:3056–3060,3229–3232`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:416,522`. |
| `DecisaoPergunta.ato_em` | O insert fica em `registrar`, que abre `atomic` e herda as travas. Escrevem o instante nos payloads a resposta humana, reabertura/recusa, resposta automática e pela trilha, validação, as duas devoluções, absorção, ressuscitação, silêncio e retratação. As travas são as dos atos acima. O passivo usa os instantes das colunas no nascimento, nas portas que o preservam e no backfill sob C → P. O backfill de silêncio toma C → D → P. `app/chamados/models.py:1673–1674,1701–1719,2095–2112,2210–2216,2242–2246,3234,4285–4294,4365–4389`, `app/chamados/services/disputa_emissao.py:1532–1534,1639–1645,1686–1688,1708–1710`, `app/chamados/services/validacao.py:173–175,280–283,355–359,440`, `app/chamados/services/materializacao.py:1063–1069`, `app/ponto/services/veredito_celula.py:175–197`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:147–150`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:244,258,350–354`. |
| `respondida_original_em`, `validada_original_em`, `validada_original_por_id` | Reabertura/recusa escreve a foto da **coluna**, sob C. Ressuscitação escreve a foto da **leitura do ato**, sob C → D → Ch → P, buscando o autor na última validação daquele instante. A FK do autor original declara `SET_NULL`. `app/chamados/services/disputa_emissao.py:1637–1654`, `app/chamados/models.py:1651,1705–1719,4197–4200,4232–4236`. |
| `LogAuditoria` de `validacao_desfeita_por_resposta` | O core pede o evento quando `_validada_antes[0]` existe. `core.observ.evento` faz o `INSERT` best-effort em `atomic` próprio, **depois de sair do `atomic` local da resposta**. Em chamada avulsa, o insert fica fora da trava de C. Nas duas declarações com `atomic` externo, a trava externa ainda vigora. `app/chamados/services/disputa_emissao.py:1517–1546`, `app/core/observ.py:94–105,124–139`, `app/chamados/services/declaracao_texto.py:350–354,421–428,656–658,695–698`. |

**Leitores identificados por campo.** As referências cobrem os acessos do diff e seus encadeamentos e os consumidores de produção localizados abaixo. Exaustividade para todos os acessos indiretos, templates, comandos e SQL fora da cópia [nao verificado].

| Campo | Leitores |
|---|---|
| `resposta_colab`, `respondida_em` | Foto da reabertura, passivo sem ato, par anterior do `save`, resposta automática e cura pela trilha. C21 e as faces SQL do par, C6 pela leitura composta, fila de correção e de validação, materializador, histórico, log/tela/API e mensageria. `app/chamados/services/disputa_emissao.py:1643–1654,1671,1701–1709`, `app/chamados/models.py:3724–3736,3810–3840,4276–4294,4360–4362`, `app/chamados/juizes.py:1383–1429,1538–1598,1762–1775`, `app/chamados/services/materializacao.py:1357–1367,2109–2115`, `app/chamados/services/historico.py:100–111`, `app/api/views.py:1158–1165,2579–2581,2663`, `app/api/views_mensageria.py:569–572`. |
| `validada_em`, `validada_por` | A tupla da resposta e a foto da reabertura, o passivo, a composição e sua face SQL, as portas de validação/recusa, a fila por coluna do materializador, a foto da rejeição de ausência, o painel/verbos e a procedência de batida. `app/chamados/services/disputa_emissao.py:1489,1499` (diff), `:1535–1546,1643–1654` (base), `app/chamados/models.py:3346–3357,3575–3585,4285–4294,4360–4362`, `app/chamados/services/validacao.py:178–182,262–283,683–689,900–917`, `app/chamados/services/materializacao.py:1171–1175,2109–2131`, `app/chamados/services/painel.py:67`, `app/chamados/services/verbos.py:264–270`, `app/ponto/services/procedencia_batida.py:71–72`. |
| `materializada_em`, `via_resolucao` | Guardas do descarimbo, foto do lote, composição/C6 e fila SQL, recusa por colisão e relavra, explicador e card. `app/chamados/models.py:3346–3349,3400–3428,3499–3511,3550–3559,3593–3645,4360–4362`, `app/chamados/services/disputa_emissao.py:1643–1654`, `app/chamados/juizes.py:1383–1429,1503–1507`, `app/chamados/services/materializacao.py:1695–1699,2109–2131`, `app/chamados/services/relavra_via.py:117–138,236–252`, `app/chamados/services/explicador.py:118`, `app/templates/chamados/partials/_card_atendimento.html:191–195`. |
| `recusa_motivo` | O escape atribui o valor sem ler o anterior. Leem o anterior a validação/devolução, a recusa do materializador, o explicador, a auditoria de invariantes, os templates e o flip de paridade. `app/chamados/services/disputa_emissao.py:1523–1527` (diff), `app/chamados/services/validacao.py:178–182,262–283,342–345,915–919`, `app/chamados/services/materializacao.py:1695–1709`, `app/chamados/services/explicador.py:113`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1083–1085,1122–1123`, `app/templates/chamados/partials/modal_fio.html:226`, `app/templates/chamados/partials/_card_atendimento.html:195`, `app/ponto/services/flip_auto.py:132`. |
| `data_evento` | Outbox, cerca de competência, juiz do dia da pergunta, dedup/reapontamento/rechaveamento, lavra, escape/log/ausência, API e histórico. `app/chamados/models.py:3445–3468`, `app/chamados/services/fio.py:919–932,1065–1096,1414–1428`, `app/chamados/juizes.py:337–341`, `app/chamados/veredito_lavrado.py:273`, `app/chamados/services/disputa_emissao.py:1429,1551–1554` (base) e `:1525–1526` (diff), `app/chamados/services/materializacao.py:1019–1036`, `app/api/views.py:1158–1159,2657–2658`, `app/chamados/services/historico.py:109`. |
| `ato_em`, `*_original_*`, evento de auditoria | `ato_em` entra na correspondência do passivo, na ordenação da sequência por instante/PK, na face SQL, no horizonte da tarefa, na sonda da ressuscitação e na tela de histórico. A tela lê `respondida_original_em` e `validada_original_por`, com prefetch do autor. A restrição lê os três campos originais. Para `validada_original_em`, leitores de aplicação além da restrição e das asserções localizadas [nao verificado]. O evento é lido pelo filtro genérico de auditoria e nominalmente pelos testes. `app/chamados/models.py:1591–1592,1710–1717,3575–3584,4225–4236,4279–4282,4333–4339`, `app/chamados/services/tarefa_do_dia.py:387–434`, `app/chamados/services/fio.py:260–267`, `app/templates/chamados/partials/modal_fio.html:210–212`, `app/core/views.py:705–730`, `app/chamados/tests/test_reabrir_nao_destroi.py:50–52`, `app/chamados/tests/test_fio_le_o_juiz.py:152–164`, `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:51–54,85–88` (diff). |

## 2. Valores usados depois da aquisição

**`reabrir_para_correcao`**

- `qs` é parâmetro. A primeira avaliação faz `values_list` no banco e materializa `_linhas = [(celula_id, disputa__colaborador_id, data_evento)]` **antes** da trava. `_donos` deriva dessa lista em memória e é usado também na conferência imediatamente após a aquisição. `_linhas` segue para `acordar_celulas` depois do `UPDATE`. Outra transação pode trocar dia e célula enquanto esta espera. O rechaveamento faz justamente isso sob C. `app/chamados/services/disputa_emissao.py:1633–1638,1656–1657`, `app/chamados/models.py:3445–3468`, `app/chamados/services/fio.py:1063–1066,1090–1098`.
- A foto `_pk, _txt, _resp, _val, _vpor, _mat, _via` vem de **outro** `values_list`, sob a trava, já na base. Texto, instante e autor alimentam o ato. Via exclui as linhas fora do fato. Texto, resposta, validação ou materialização determinam se há algo a fotografar. O lote reavalia `qs` para conferir donos, aplicar a cerca e atualizar. A seleção construída pelo chamador pode ser anterior e fixa, como o `pk__in` de `perguntas_corrigiveis`, ou conter predicados reavaliados, como o `respondida_em__isnull=True` do comando de limbo. `app/chamados/services/disputa_emissao.py:1639–1656`, `app/chamados/models.py:3497–3511`, `app/chamados/juizes.py:1762–1767`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:70`.
- `_agora` vem do relógio sob a trava no diff. `ato`, `por` e `motivo` vêm do chamador. O tipo de ato é conferido antes, o motivo é truncado na montagem e `por` é passado como FK. `campos` **é local**, montado antes com `None` e `''`. Esses valores locais e os argumentos Python permanecem os mesmos durante a espera. A existência da linha referenciada por `por` é uma questão separada da identidade do objeto em memória. As constantes de via vêm da classe, e `n` vem do `UPDATE` sob a trava. `app/chamados/services/disputa_emissao.py:1617,1627–1629,1640–1658`, `:1637` (diff), `app/chamados/models.py:4191–4196`.

**`responder_pergunta_disputa_core`**

| Valor | Fonte e uso após a aquisição |
|---|---|
| `p`, `p.pk`, `_dono`, `p.disputa` | `p` é a instância do chamador. `_dono = p.disputa.colaborador_id` é lido dentro do `atomic`, **antes** da aquisição, de uma FK já carregada ou por consulta se estiver sem cache. A conferência usa esse mesmo ID. `pk` identifica o refresh, o save, a decisão, a auditoria e a nota. O refresh parcial deixa a relação `disputa` disponível como veio. O código permite fechar/reabrir a disputa durante a espera e o log pode usar `fechada_em` antigo. Um escritor de produção que troque a identidade do dono da pergunta, nessa janela, [nao verificado]. `app/chamados/services/disputa_emissao.py:1480–1484` (diff), `:1532–1554,1566–1577` (base), `app/chamados/models.py:2118–2122,2335–2347,2359–2363,2626–2645,3743–3748`. |
| Os sete campos relidos | Vêm do banco sob C e são copiados para `p`. `_sem_linha` lê o par e a validação **antes de sobrescrevê-los**. `_validada_antes` fica na memória e alimenta o log posterior. `materializada_em` e `via_resolucao` decidem o descarimbo. `data_evento` alimenta a mensagem do escape, o log, a cerca quando ativa e a materialização de ausência. `app/chamados/services/disputa_emissao.py:1483–1489,1495–1528` (diff), `:1542–1563` (base), `app/chamados/models.py:3550–3552,4276–4294`, `app/chamados/services/fio.py:1407–1428`, `app/chamados/services/materializacao.py:1019–1032`. |
| `decisoes`, `_sem_linha`, `_campos`, relógio | O prefetch de `decisoes` pode ter vindo de antes da espera. O diff o descarta. `_sem_linha` consulta o banco por `pergunta_id/ato/ato_em` se houver resposta ou validação na coluna. O relógio é lido em seguida. `_campos` nasce sob a trava e soma os campos de validação, os do descarimbo e o motivo conforme o ramo. A resposta e sua linha usam o mesmo `_agora`. `app/chamados/services/disputa_emissao.py:1483–1499,1508–1533` (diff), `app/chamados/models.py:3423–3433,4276–4294`. |
| `resposta`, `user`, `ja_respondida`, literais | `resposta` é o parâmetro normalizado e aceito antes da trava. Depois vira texto da coluna e do ato, escolhe o escape/ausência e entra nos logs e na nota. `user` é parâmetro, com `pk` consultado ao montar o ato, e segue ao resgate, ausência e nota. Validade da FK em face de exclusão concorrente [nao verificado]. `ja_respondida` fica como o chamador passou. Pode representar um estado que mudou durante a espera, pois web e API o calculam antes. Só altera o sufixo do log e a etiqueta da nota. A lista de escapes que viram ausência é a constante do materializador. `app/chamados/services/disputa_emissao.py:1446–1473,1532–1534,1549–1578`, `app/chamados/views.py:1274–1278`, `app/api/views.py:2645–2653`, `app/chamados/services/materializacao.py:1097–1103`. |
| Tipo, opções, destino, pai, motivo, enunciado e valores consultados na cauda | `tipo_resposta` e `opcoes` são campos da instância. O tipo e o JSON voltam a ser usados por `resposta_display`, junto à sequência lida depois do `registrar`. `motivo` e `pergunta` da instância entram no log e no follow-up. `p.chamado_destino` e `p.disputa.chamado` podem vir do cache ou de consulta preguiçosa pelos IDs que a instância ainda possui. A resolvedora pode mudar pela porta de reapontamento sob C. O core também busca o pai por `p.disputa.chamado_id`, lê o rótulo de posto/escala se aplicável e calcula a data local e o hash da nota após sair do `atomic` local. Resgate e ausência são chamados nessa mesma cauda. `app/chamados/services/disputa_emissao.py:1549–1578`, `app/chamados/models.py:3837–3863`, `app/chamados/services/fio.py:919–942,977–981`, `app/chamados/services/materializacao.py:1019–1036,1088–1089,1213–1225`. |

## 3. Cobertura do refresh e efeito no objeto do chamador

- **`reabrir_por_resposta`** lê `materializada_em` e `via_resolucao`, ambos na lista. `fora_do_fato` volta a ler a via. `materializada_por_id` fica fora da lista. Nesse caminho é escrito como `None` quando há descarimbo, sem leitura do autor anterior. **`linhas_da_projecao_sem_linha`** lê também `pk`, usado como identidade, e os quatro campos de resposta/validação que estão no refresh. **`registrar`** lê `p.pk`, o cache de `decisoes` e o `__dict__` para invalidar `_pv_memo`. Lê célula, dono e dia por outro queryset no banco. `app/chamados/models.py:3317–3321,3413–3433,3445–3448,3550–3552,4276–4294,4365–4389`, `app/chamados/services/disputa_emissao.py:1483–1488` (diff).
- **`save` e signals**. O `save` usa `_state.adding`, `pk`, `update_fields` e o par já atribuído, com consulta do par persistido. Se a cerca estiver ativa, lê ainda `conversa` e `p.disputa.colaborador_id` da instância/relação e compara com a foto gravada. `conversa`, `disputa` e `disputa_id` ficam fora do refresh. O signal do beco lê `instance.disputa_id` e agenda releitura do banco após commit. O do outbox lê `instance.pk` e os nomes dos campos do save, e busca `celula_id`, colaborador e dia no banco. Portanto `celula_id` fora da lista não torna a leitura do outbox da **resposta** velha. `app/chamados/models.py:3691–3736,3440–3448`, `app/chamados/services/fio.py:1407–1428`, `app/chamados/signals.py:229–240,252–259`, `app/chamados/services/becos.py:80–89`.
- **Objeto do chamador**. Sim, é o mesmo `p`. `data_evento` pode mudar de forma observável sem entrar nos campos do `UPDATE`. A API lê esse dia no log após a chamada. `materializada_em` e `via_resoluca

[truncado em 20000 de 32729 caracteres]

## 20/11 10:04 · sessao 4d66f93abba8 · TRAVAS-SEM-MEMORIA · leitura · origem=Code

Auditoria estática da cópia e dos anexos fornecidos, sem executar suites nem publicar. O hash da cópia, a revisão em `logs/TRAVAS-SEM-MEMORIA/rev_c_0945/r.txt:66–77`, a publicação das cinco linhas e o envio único ao Codex ficam [nao verificado]. A regra exige publicação e envio antes da construção. `CLAUDE.md:394`, `app/docs/PISTA.md:169–176`, `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:30,44`.

### 1. Quem chama

- **O que se sustenta.** A volta intermediária roda ruff e selo e então chama `suite.sh --volta` para um **recorte**. Se a seleção efetiva cobrir a descoberta inteira e vier sem motivo canônico, sai `rc 2`. O settings da cópia tem um `PreToolUse` genérico. O disjuntor pode liberar cedo o Bash da sessão principal e, na chamada Codex, extrai papel, obra e anexos. O registro novo para PRONTO e ficha é proposta. `bin/volta_intermediaria.sh:18–34`, `bin/suite.sh:162–180,215–263`, `.claude/settings.json:62–73`, `bin/disjuntor.py:1448–1463,1837–1863`, `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:47,58`.
- **O que falta.** A linha omite o executor que M3 manda reconferir obra e SHA imediatamente antes de criar a ref e pedir o job. Falta comprovar qual comando chega ao registro carregado na sessão da raiz e que a ficha conferida é a mesma versão lida pelo wrapper. A cópia documenta sessão aberta em `app/` com hooks da raiz inertes. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:26,45–47,58`, `bin/codex_pergunta.sh:323–335`, `app/docs/RELATO.md:3341`.
- **Sobra do recorte.** M1 ainda instala **dois** registros. M5 ainda começa com “T2 faz” e termina com “O hook lê o mesmo medidor na hora do envio”. A leitura no recebimento do aval pertence ao recorte retirado. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:44,49–54,63`, `retoma_2011_0945.txt:20–21`.

### 2. Quem espera

- **O que se sustenta.** PISTA-2 documenta `ssh pista espera <id>` com o rc da suite. O RELATO registra resultados com `Ran` e rc, mas a revisão anterior registra que faltava demonstrar essa saída pelo próprio `espera`. Na VM, `suite.sh` aguarda a trava e devolve o rc do processo. No ESTADO, a espera já é síncrona: `relato.sh` aguarda o gerador, que chama o medidor, e aguarda a raspagem antes do commit. `app/docs/BACKLOG.md:2191`, `app/docs/RELATO.md:65,1497,1529`, `bin/suite.sh:316–325,336–372`, `bin/relato.sh:37–38,114–147`, `bin/gerar_estado.py:211–224`.
- **O que falta.** Faltam a prova de `Ran`, veredito e rc para o mesmo ID pelo caminho de espera da pista e a prova de que a recusa da ficha chega antes de qualquer entrada no wrapper. O RELATO também pode ficar retido enquanto o ESTADO segue. A leitura pelo chat da versão publicada do rascunho fica [nao verificado]. `app/docs/RELATO.md:65`, `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:58–59,82,91`, `bin/relato.sh:44–73,114–119`.
- **Sobra do recorte.** A resposta de T2 “no evento de entrada” saiu da linha revisada. O smoke do V4 ainda exige os dois eventos e A1/A2/A3/A8. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:64,90,94`, `retoma_2011_0945.txt:21`.

### 3. Falha e limite

- **O que se sustenta.** Na cópia, `suite.sh` barra inteira sem motivo canônico e executa pela trava da VM. O wrapper abre e raspa anexos. As duas recusas literais são propostas de M3 e M9a. No medidor, `_git` devolve vazio em erro ou timeout tratado. Uma declaração sem caminho ganha branch `(sem worktree no disco)`. A base ainda carece de `rev-parse HEAD` por cópia e da saída `raia=<nome> SEM WORKTREE`. `bin/suite.sh:236–263,316–325`, `bin/codex_pergunta.sh:57–68,99–100,323–335`, `bin/raias_estado.py:42–48,96–106,125–154`, `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:47,49,58`.
- **O que falta.** `--espera` limita a aquisição da trava da VM. A fila remota documentada roda um job por vez, e seu teto e cancelamento constam como pendentes. Falta delimitar o fim da espera remota, provar o ESTADO publicado quando um caminho existe mas o HEAD falha, e identificar a resposta ao “ponto (1) devolvido acima”. O anexo dá a exceção imediata só à F5, mantém o pouso na VM e submete o escopo mais amplo de TRAVAS à revisão. A resposta ao ponto (1) fica [nao verificado]. `bin/trava_teste.sh:14–19,22–42`, `app/docs/BACKLOG.md:2191,2194`, `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:81`, `retoma_2011_0945.txt:5,20–21`.
- **Sobra do recorte.** A recusa do **aval** da linha original e as condições de M6–M8 saíram. A linha revisada ainda precisa ser lida com a distinção do anexo: `SEM WORKTREE` é saída do ESTADO em M5. A devolução do aval por HEAD indisponível era M7/A6. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:49–54,65,78`, `retoma_2011_0945.txt:21`.

### 4. Medida

- **O que se sustenta.** E1/E2 especificam três tokens de cópia e uma declaração sem worktree, depois a mudança de um único token ao avançar um HEAD. FD1–FD6 especificam três recusas com zero chegadas e três entradas com uma chegada. São resultados esperados. Execução, latência e publicação ficam [nao verificado]. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:81–83,110–112`.
- **O que falta.** Falta vincular por caso a porta, o SHA, o veredito, as chamadas efetivas e a linha de recusa ou publicação. Na ficha, faltam as referências dos itens contados e as recolagens efetivas do Ronald, ambas previstas em M9b. No ESTADO, falta a base de comparação do tempo adicional e a prova de igualdade dos **tokens** antes e depois da raspagem na mesma geração. O raspador pode alterar texto e a publicação ainda depende de commit e push. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:59,66,81–83,107,110–112`, `bin/raspar_publico.py:23–41`, `bin/relato.sh:114–147`.
- **Sobra do recorte.** A linha revisada conserva E1/E2, que ficam. No V4 ainda aparecem a repetição de A10 em RED e SELO, os mutantes de T2, A1–A14, o marco de ativação e as recolagens de A11–A14 no PRONTO. São referências ao trecho retirado. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:78–90,108–110,114`, `retoma_2011_0945.txt:21`.

### 5. Quem desliga

- **O que se sustenta.** Restaurar o settings local pode retirar o registro Bash proposto e, com ele, a recusa da ficha prevista nesse registro. A cópia tem precedente de reversão em duas partes, settings local e commit. Para a rota, “retira a seleção” deixa uma lacuna: M2 manda recusar pre-opus sem `PISTA_LATERAL`, e M3 prevê guarda também no executor. Tirar a chave e o hook pode deixar a guarda do executor ativa. O `SUITE_PISTA=0` do RELATO pertence a outro contrato, registrado como ainda sem construção. `.gitignore:66`, `app/docs/RELATO.md:81,4356`, `app/docs/BACKLOG.md:2195`, `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:45–47,58`.
- **O que falta.** Faltam a identificação do settings anterior e do commit a reverter, a condição de saída da rota no executor e a prova pós-desligamento. Para M5, “o ESTADO seguinte” depende de nova geração, raspagem e push bem-sucedidos. Falta provar a retirada dos tokens na publicação e esclarecer o efeito da reversão sobre `listar` e o JSON, que usam o mesmo módulo. `app/docs/PISTA.md:171–175`, `bin/relato.sh:37–38,114–147`, `bin/raia_agente.sh:80–81`, `bin/raias_estado.py:163–175`.
- **Sobra do recorte.** A linha revisada usa um registro. A restauração de **dois** permanece no contrato original, e a instalação de dois permanece no roteiro manual e no PRONTO da ficha. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:67,92–94,110`, `retoma_2011_0945.txt:21`.

### Quem mais chama, espera ou lê o medidor

**Sim.** O leitor direto no caminho de publicação é `gerar_estado.py`, que importa `ler`, `linha` e `linha_sujas`. Acima dele, o `hasner-relato.timer` aciona o serviço `oneshot`, que chama `relato.sh`. O `listar`, já nomeado na linha 1, também espera a chamada síncrona ao medidor. `bin/gerar_estado.py:211–224`, `bin/hasner-relato.timer:1–8`, `bin/hasner-relato.service:9–15`, `bin/relato.sh:37–38`, `bin/raia_agente.sh:80–81`.

Há também a **CLI direta**, em JSON ou `--linhas`, com uso direto registrado no RELATO-ARQUIVO, e o **selo de host**, que importa `ler` e `linha` na fixture e é aguardado pela régua. O raspador lê o ESTADO gerado. Leitura direta do medidor por HAIKU ou pelo placar, carga do timer no host e outros consumidores externos ficam [nao verificado]. `bin/raias_estado.py:163–179`, `app/docs/RELATO-ARQUIVO.md:39288`, `bin/tests/test_raia_declarada_no_estado.sh:43–78`, `bin/regua.sh:134–145`, `bin/relato.sh:114–119`, `bin/raspar_publico.py:23–31`, `bin/gerar_estado.py:211–216`.

## 20/11 10:03 · sessao 9814ae4a94d5 · E4C-F5-CONSTROI · construtor · origem=Code

Diff aplicado na cópia sobre `652d3d0de`. Os únicos arquivos alterados são [disputa_emissao.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1483) e o novo [test_e4c_f5_a1_leitura_sob_trava.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:22). Conferi a sintaxe e o escopo do diff por inspeção estática. RED, verde, mutantes e contagem SQL em execução ficam **[nao verificado]**.

## Cura e SQL

- **A — `app/chamados/services/disputa_emissao.py::reabrir_para_correcao`.** A única mudança foi mover `_agora` para depois de `exigir_colaboradores_travados`. `_linhas` e `_donos` continuam alimentando a aquisição da mesma trava. **Zero SQL adicional** pelo diff. `app/chamados/services/disputa_emissao.py:1628–1644`.
- **B — `app/chamados/services/disputa_emissao.py::responder_pergunta_disputa_core`.** O `refresh_from_db` parcial roda na própria `p`, depois de `_exigir` e antes de qualquer atribuição. A foto, `_validada_antes` e `_campos` usam a releitura. Os retornos antecipados e o uso de `ja_respondida` foram preservados. A auditoria pós-`atomic` recebe a validação relida e a API lê `respondida_em` da mesma instância. O dublê aceita `refresh_from_db(**kw)`. `app/chamados/services/disputa_emissao.py:1439–1553` · `app/api/views.py:2645–2664` · `app/chamados/tests/test_e4_expand.py:69–73`.
- **SQL de B por inspeção.** Um `SELECT` adicional de pergunta. O `SELECT` existente de decisões passa a rodar também quando a carga velha estava sem os dois carimbos e a releitura encontra algum. No RED B, a resposta anterior já faz essa consulta rodar na base. Quando a releitura revela uma validação derrubada, também passa a ocorrer o `INSERT` da auditoria existente, com seu `atomic`. `app/chamados/services/disputa_emissao.py:1483–1489,1534–1545` · `app/chamados/models.py:4276–4282` · `app/core/observ.py:133–138` · `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:56–60`.

| Campo relido em B | Uso |
|---|---|
| `resposta_colab` | Texto da resposta ou validação anterior na foto do passivo. `app/chamados/models.py:4285–4294` |
| `respondida_em` | Decide se há resposta a fotografar e fornece o instante usado para achar a linha correspondente. `app/chamados/models.py:4276–4289` |
| `validada_em` | Decide se há validação a fotografar e se o par deve ser limpo. Também alimenta o antes da auditoria. `app/chamados/models.py:4290–4294` · `app/chamados/services/disputa_emissao.py:1489,1499–1510,1534–1545` |
| `validada_por_id` | Autor da validação na foto e no antes da auditoria. `app/chamados/models.py:4292–4294` · `app/chamados/services/disputa_emissao.py:1489,1544` |
| `materializada_em` | Decide se existe carimbo a reabrir. `app/chamados/models.py:3538–3552` |
| `via_resolucao` | Decide se a via permite descarimbar. O autor do carimbo é atribuído pelo descarimbo, que lê a via para proteger a linha fora do fato. `app/chamados/models.py:3317–3321,3413–3433,3550–3552` |
| `data_evento` | Incluí o dia porque o ramo de escape o grava em `recusa_motivo` e o dia pode ser regravado pelo rechaveamento. `app/chamados/services/disputa_emissao.py:1514–1527` · `app/chamados/services/fio.py:843–854` |

O refresh parcial preserva prefetches, como documenta o teste vizinho. A cura remove a entrada `decisoes` antes da foto e chama `linhas_da_projecao_sem_linha([p])` com `ja=None`. Quando há carimbo, esse caminho consulta `DecisaoPergunta.objects` diretamente. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:371–381` · `app/chamados/services/disputa_emissao.py:1483–1488` · `app/chamados/models.py:4274–4282`.

## REDs e mutantes

Os dois REDs interceptam uma chamada à trava. Antes da delegação, outra instância ORM passa pela porta real `_carimbar_validacao`, que grava coluna e `VALIDACAO` com o mesmo instante. O relógio controlado avança antes de a porta em teste continuar. `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:16–46,56–71` · `app/chamados/services/validacao.py:161–175`.

- **A.** Na base, a asserção da **composta nula** deve falhar. A coluna já foi limpa, mas `REABERTURA(t1)` vem antes de `VALIDACAO(t2)` na redução. **Mutante A:** mover a linha `_agora = timezone.now()` para a primeira linha do `atomic`, antes de `_linhas`. O RED A deve matar o mutante na mesma asserção. `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:48–54` · `app/chamados/models.py:4333–4351` · `app/chamados/services/disputa_emissao.py:1628–1640`.
- **B.** Na base, a asserção da **coluna nula** deve falhar. A `RESPOSTA(False)` posterior já limpa a composta, mas `_campos` omite o par validado entre a carga e a trava. O teste também exige uma única `VALIDACAO` original, a resposta posterior e a auditoria com o antes e o depois. **Mutante B:** inserir `_validada_na_carga = p.validada_em is not None` imediatamente antes do refresh e trocar a inclusão do par por `_campos += ['validada_em', 'validada_por'] if _validada_na_carga else []`. O RED B deve matar o mutante na asserção da coluna. `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:73–88` · `app/chamados/services/disputa_emissao.py:1482–1489,1498–1510,1528–1545`.

## Leitores do novo `ato_em`

Em A, o instante pode mudar a ordem e o resultado de `leitura` e `composta`, o rótulo de `estado` e a face SQL `q_validada_pelo_ato`. Também pode mudar quais atos entram no horizonte de `tarefa_do_dia`, qual ato a tarefa toma como último, e quais atos a sonda de ressuscitação vê depois do seu corte. O histórico usa a ordem dos atos e o modal exibe o horário da reabertura ou recusa. `app/chamados/models.py:1591–1595,3249–3268,3563–3585,3818–3834,4225,4333–4362` · `app/chamados/services/tarefa_do_dia.py:142–169,387–429` · `app/templates/chamados/partials/modal_fio.html:205–212`.

Em B, `respondida_em` e o `ato_em` da resposta continuam compartilhando o relógio sob a trava. A diferença possível para a leitura do ato e o histórico vem da foto do passivo relida. Para a auditoria, vem da validação anterior agora capturada. `app/chamados/services/disputa_emissao.py:1483–1497,1528–1545` · `app/chamados/models.py:3818–3834,4276–4295,4333–4351`.

## Comandos para o Code

Na cópia de ensaio da base, usar apenas o módulo novo sobre `652d3d0de` e rodar os dois comandos individuais. Para cada mutante, usar o comando individual correspondente sobre a cura com aquele patch. Os mutantes foram descritos acima. `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:32,56`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings.ci --noinput \
  chamados.tests.test_e4c_f5_a1_leitura_sob_trava.LeituraSobTravaTest.test_RED_A_reabertura_posterior_a_validacao_limpa_coluna_e_composta

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings.ci --noinput \
  chamados.tests.test_e4c_f5_a1_leitura_sob_trava.LeituraSobTravaTest.test_RED_B_resposta_posterior_a_validacao_limpa_coluna_e_preserva_trilha
```

Na cópia corrigida, rodar o módulo novo e os sete vizinhos com o runner de ensaio. `app/config/settings/ci.py:6–18,38–41`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings.ci --noinput \
  chamados.tests.test_e4c_f5_a1_leitura_sob_trava \
  chamados.tests.test_e4_expand \
  chamados.tests.test_e4_expand_orm \
  chamados.tests.test_porta_resposta_pergunta \
  chamados.tests.test_f2_ressuscitar_tarefa_igual \
  chamados.tests.test_e4c_f5_voltaB_sitios \
  chamados.tests.test_e4c_f5_selo_catraca \
  chamados.tests.test_e4c_f5_selo_chamada
```

## 20/11 09:54 · sessao 0dc5f411bdcf · CADEIA-20-PASSO-1 · leitura · origem=Code

## I. Respostas Q1 a Q5

### Q1 — Por que o fecho do chamado restante devolveu `false` em P1?

**O gate da pergunta viva é compatível com a sequência medida. A cláusula que devolveu cada `false` fica [nao verificado].** Em P1, o corpo inicial fechou a cobrança indevida. A segunda passada ainda leu o chamado restante vivo, com fio e sem protesto, e tentou fechá-lo antes de gravar `concorde`. Foi esse carimbo que fez o fio cair de 1 para 0. Em P2, a primeira leitura já viu o fio morto e o fecho teve sucesso. A sequência aparece para **128355** (`39234`), **129943** (`36531`) e **138359** (`37355`). Anexo 8, células citadas, `seq 1262–1274 → 1559–1565`, `894–906 → 1500–1506` e `65–77 → 1431–1437`. `app/ponto/services/cartorio.py:1527–1547,1683–1707,1726–1738,2012–2037`

O `false` de `reconciliar()` vem de `_decisao_do_fechamento`: ela recarrega o estado, consulta o motor, passa pela premissa e pela conversa do pai, e só então chega à guarda da pergunta viva. Esta última pergunta ao C6 com o **estado terminal do chamado** em memória. Se uma pergunta ancorada de competência aberta seguir viva e faltar destino legítimo para reatá-la, recusa com `pergunta_viva_na_competencia_aberta`. A consulta de fio do cartório e o universo de perguntas ancoradas da guarda são leituras distintas. O extrato não traz a trilha da recusa por chamado que permitiria distinguir a guarda da premissa da guarda da pergunta viva. `app/chamados/models.py:355–405,422–443,654–668,684–705,776–830` `app/ponto/services/cartorio.py:1527–1532` Anexos 6–8, células 128355, 129943 e 138359.

### Q2 — Dá para perguntar ao C6 antes de carimbar?

**Sim, para uma lavra candidata e um conjunto fixo de perguntas e demais insumos.** A porta `veredito_que_a_lavra_escreve` devolve o par efetivo, inclusive a prioridade do admin. O cartório já expõe ata e par do julgamento em curso, e `celula_como_o_julgamento_deixa` faz a cópia em memória para leitura. A ressuscitação já usa o precedente completo: copia a pergunta com cache de FK próprio, projeta a célula que ela referencia, prepara o descarimbo em memória e consulta `pergunta_viva`. O C6 usa a ata, o veredito e a via, além dos demais degraus e do instante informado. Fecho, reaponte, criação e efeitos em outro dia exigem nova leitura dos insumos que essas portas efetivamente deixarem. `app/ponto/services/cartorio.py:67–85,1578–1583,1654–1661` `app/ponto/portas/celula.py:970–1006` `app/chamados/juizes.py:415–429,866–898,1403–1491` `app/chamados/models.py:1549–1576`

O literal **“COM o carimbo (e a resposta) aplicados”** está em `PerguntaDisputa.carimbar`: aplica os campos em memória, chama `pergunta_viva(self)`, restaura se a pergunta seguir viva e só salva após a aceitação. Para projetar a lavra da **célula**, o molde mais direto é a cópia usada pela ressuscitação. O gate real de fecho continua sendo a autoridade sobre o chamado. `app/chamados/models.py:1560–1576,3299–3348,684–705`

### Q3 — Onde está o corte da terceira passada?

Na cópia, **“Nao ha 3a passada por lei”** está no comentário de `app/ponto/services/cartorio.py:1158–1164`. O código consome a fila uma vez e conta o que sobrar. A releitura por emissão é outra via existente e também conta a fila que produzir. O `:1053` desta cópia pertence à conferência das razões da sobra. A referência `:1053` no main fica [nao verificado]. `app/ponto/services/cartorio.py:1047–1055,1152–1164,1209–1231`

Procurei o limite em `CORTES.md`, `CORTES.json`, `LEIS.md` e `LAPIDES.md`. O corte que nomeia a CADEIA-20 registra o juiz da razão da sobra. O corte de fecho de três trata de quem sobrevive pelo C8. A L-124 rege a vigência. A ocorrência de “terceira passada” nas lápides é da revisão do `CLAUDE.md`. **Corte geral escrito para o número de passadas do cartório [nao verificado].** Para esta obra, a vedação expressa está no **C3 do anexo 3**. Se a frase do código invoca um corte geral, qual é o seu literal e onde está registrado? `app/docs/CORTES.md:441,462,563` `app/docs/CORTES.json:1251,1364,2123–2126` `app/docs/LEIS.md:145` `app/docs/LAPIDES.md:23–42`

### Q4 — O que a fábrica devolve, e quais células ainda importam?

`perguntar_pela_celula` devolve **uma lista de motivos**. Pode descarimbar antes de chegar a esse retorno, inclusive num caminho que devolve `[]`. A função interna `perguntar` devolve **uma contagem de carimbos desfeitos**, mas o chamador descarta essa contagem, assim como descarta o retorno da chamada direta de descarimbo. O cartório usa a lista para `perguntas_no_julgamento`. A releitura consulta o aumento de `emitidos` no ato do colaborador. Logo o retorno atual não certifica criação, descarimbo nem mudança de projeção. `app/chamados/services/fabrica_da_celula.py:160–175,178–184,201–220` `app/chamados/services/relavra_via.py:236–263` `app/ponto/services/cartorio.py:1114,1179–1184,1209–1222,2136–2146`

Na projeção do V3, **F** é presença de vivo com fio e **FP** é presença de vivo com fio **que protesta no mesmo chamado**. O resultado para as três no braço `em_curso` é este. `app/ponto/services/cartorio.py:1527–1547,1605–1617,1652–1653`

| Célula | O que sobrevive à granularidade (i) |
|---|---|
| **138541** | **Importa com certeza no extrato.** A fábrica inseriu `60149` para `36617` e fez `F:0→1`. O contador estava `22/22`. A leitura inicial de P2 viu o novo fio. Anexo 8, célula 138541, `seq 1037, 1136–1138, 1412`. |
| **129476** | **Importa se FP mudou.** A fábrica inseriu `60113` para `36172`, com `11/11` no contador. `fio:1→2` conserva F. Se o chamado que ganhou fio protesta e o anterior com fio não protestava, FP muda. O lastro necessário para confirmar isso fica [nao verificado]. Anexo 8, célula 129476, `seq 333, 635–637, 1475`. |
| **129490** | **Escrita descoberta, sem delta fotografado.** A fábrica inseriu e reapontou `60114` para `39014`, que já tinha fio em P1. P1, P2 e P3 registram os mesmos chamados com fio e o mesmo hash. A importância para a projeção do V3 fica [nao verificado] fora das fotos, inclusive quanto a P/FP não publicados e a transientes. Serve como controle de escrita sem mudança demonstrada. Anexo 8, célula 129490, `seq 356, 693–694, 1485, 1627, 1669`. | [nao verificado]

### Q5 — Qual cobertura o desenho precisa entregar?

**Classe 1:** levar a perda de fio e o fecho do remanescente para dentro de P1, com a leitura da segunda passada já vendo o estado final. A queda de fio ocorreu depois da última leitura em **três PKs × três braços**, nove ocorrências. **Classe 2:** alcançar a mudança relevante da fábrica pela única releitura existente, inclusive quando `emitidos` não subir. Escrever uma pergunta e mudar uma das cinco presenças são condições distintas nos extratos. Escolho **A1 + B1** abaixo. A D mediu a base. Os efeitos dos desenhos ficam [nao verificado] até a prova. Anexos 6–8, células 128355, 129943, 138359, 129476, 138541 e 129490. `app/ponto/services/cartorio.py:1123–1231,1527–1558,1605–1707,2035–2037`

## II. Desenhos por classe

Os quatro desenhos propõem alterações em `app/ponto/services/cartorio.py` e casos em `app/escala/tests/test_cadeia20_volta3.py:142–172`. Chamam as portas e os juízes existentes. Os arquivos citados como selos e leitores entram na verificação, sem alteração proposta por estes desenhos. A lista integral de arquivos da F5 fica [nao verificado]. O conflito conhecido de `chamados/juizes.py` é do **MUDA 2 já aprovado**, tratado à parte no rascunho. `app/docs/RELATO.md:11,28,34–37` `app/ponto/services/cartorio.py:1082–1090,1169–1184,1654–1661,2012–2037`

| Desenho | Arquivos, custo e risco para testes, selos e leitores | Células que pretende zerar | [nao verificado]
|---|---|---|
| **A1 — Recompor a primeira lavra depois do fecho confirmado** | **Cartório + teste da volta 3.** Depois de o corpo inicial fechar a cobrança que gerou `COBRANCA_SEM_FURO`, recarregar os chamados e o fio da célula candidata antes do primeiro carimbo. Reusar a grade e o classificador, projetar a lavra efetiva e consultar C6. Se a primeira lavra extinguiera o fio do lote já adiado, E2 e o gate real podem fechar o restante antes de consumir a fila. **Custo:** zero nova passada, uma carga dirigida de chamados/perguntas, uma classificação adicional e leituras C6 nos candidatos. Total SQL e duração [nao verificado]. **Risco:** publicar `cods` intermediários, acordar fábrica com o conjunto errado ou ressuscitar chamado já fechado. Conferir `out['cods']`, o selo da ordem do fecho e os leitores da ata. `app/ponto/services/cartorio.py:1123–1164,1170–1184,1614–1661,1726–1738,2012–2037` `app/ponto/tests/test_cods_do_ato.py:77–105` `app/ponto/tests/test_o261_cobrado_le_o_fio.py:393–420` `app/escala/services/leitor_celula.py:194–222` | **128355, 129943, 138359**, nos três braços, se o fecho real após a primeira lavra autorizar a transição. Anexos 6–8, nessas células. |
| **A2 — Concluir o fecho dentro da segunda passada, antes da assinatura** | **Cartório + teste da volta 3.** No segundo corpo, para a célula candidata, persistir a lavra pela porta existente, executar E2 e fecho pelas portas existentes e recarregar a projeção real antes do `carimbar` daquele corpo. O bloco só cabe se a cerca de MUDA 4 alcançar também os alvos do E2 e da cascata. Manter essas operações na mesma transação e parar pelo C3 se a lavra deixar de descrever o estado obtido. **Custo:** zero nova passada de fila, um save adicional da lavra, uma execução de E2 e uma recarga localizada nos casos ativados. Total SQL [nao verificado]. **Risco:** separar o save conjunto atual, mudar a ordem de `EventoCelula` ou acordar alvos indevidos do E2. Conferir o selo de escritor, o consumidor e o custo do caminho de batida. `app/ponto/services/cartorio.py:1152–1164,2012–2037` `app/ponto/portas/celula.py:941–965,1032–1041,1090–1098,1181–1202` `app/chamados/reconciliador.py:634–665,1414–1438` `app/ponto/tests/test_selo_veredito_so_pela_porta.py:181–195,270` `app/ponto/tests/test_selo_chokepoint_escrita.py:196–235` | **128355, 129943, 138359**, nos três braços, se o gate fechar e a releitura localizada confirmar a lavra. Anexos 6–8, nessas células. |
| **B1 — Abrir a releitura pela diferença nas cinco presenças** | **Cartório + teste da volta 3.** Comparar a projeção nas bordas da fábrica, por PK elegível do ato, incluindo alvos em outros dias. Usar dados frescos após a fábrica e a mesma projeção que alimenta a impressão. `emitidos` conserva seu sentido. **Custo:** até duas cargas em lote para as fotos, mais o C6 correspondente e até uma execução da releitura existente nos atos antes descobertos. Uma emissão que já abriu a via não duplica a releitura. Total SQL [nao verificado]. **Risco:** perder FP ou ler cache anterior à fábrica, deixar escapar efeito em outro dia, inflar contadores. Conferir os selos de escopo de `cods` e descarimbo e os leitores de veredito/fio. `app/ponto/services/cartorio.py:141–157,1165–1231,1527–1558,1605–1617,1652–1653` `app/ponto/tests/test_cods_do_ato.py:77–105` `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:126–137,152–187` | **138541** em `em_curso` e **129476** se mudar FP. **129490** fica como controle de projeção igual, a demonstrar. Anexo 8, nessas células. |
| **B2 — Abrir a releitura quando a fábrica tiver rodado no universo elegível** | **Cartório + teste da volta 3.** Manter a via por emissão e, sem emissão, executar a mesma releitura uma vez quando houver chamada à fábrica em célula elegível. Preservar a janela de entrada e aplicar o horizonte e `_K6` ao conjunto que a nova condição pode julgar. A guarda decide o que relavrar. **Custo:** zero fotos adicionais, até uma passagem de leitura da janela por ato elegível antes descoberto, inclusive quando o hash ficar igual. Total SQL [nao verificado]. **Risco:** custo de frota, trabalho indevido no limite de competência e contagem de leitura como relavra. Conferir os selos de janela, fábrica sob `apply` e `cods` por ato. `app/ponto/services/cartorio.py:1179–1231,1268–1273,1552–1558` `app/chamados/tests/test_fabrica_por_evento.py:60–74` `app/ponto/tests/test_cods_do_ato.py:77–105` `app/folha/export.py:556–594` | **138541** em `em_curso` e **129476** se mudar FP. Lê **129490** depois da escrita, com pulo se a projeção ficar igual. Anexo 8, nessas células. |

**Universo completo das nove.** A1 e A2 miram **128355, 129943 e 138359**. B1 e B2 miram **138541** e, condicionalmente, **129476**, com **129490** como controle. **142995 e 143945** já tiveram releitura pela emissão em `encerrado` e `cruza`, e vieram estáveis em `em_curso`. **129134** já ficou estável nos braços fixos da D. Em `cruza`, a mudança de `enc` é esperada, seguida da emissão e da releitura no P3. Preservar esse controle faz parte do teste. Anexos 6–8, respectivas células, em especial 129134 no anexo 6 `seq 1007–1008, 1636, 1807`, anexo 7 `1777–1838`, anexo 8 `721–722, 1426, 1614`. `app/ponto/services/cartorio.py:523–532,1209–1222,1540–1558` `app/ponto/turnos.py:2124–2134`

## III. Rascunho de aval

aval Ronald: CADEIA-20-COBERTURA-V1 (rascunho Codex 20/11) -- unidade: célula-dia, impressão da ata; lei: L-124 e célula soberana, complemento ao V3 bedd9c89a6e6, nenhuma lei nova (`app/docs/LEIS.md:145`, `app/docs/GLOSSARIO.md:11`, anexo 2)

PAREI RASCUNHO — só CADEIA-20-PASSO-1. A medida D põe três perdas de fio depois da segunda leitura de P1 em cada braço e duas mudanças de fio depois da fábrica sem releitura em `em_curso`. A solução proposta usa a segunda passada para o primeiro caso e altera a condição da releitura existente para o segundo. Ronald, aprova o gatilho adicional por mudança da projeção após a fábrica, mantendo o limite de uma releitura e nenhuma terceira passada de fila? O C3 do anexo 3 pede aval para essa alteração. Anexos 6–8, células 128355, 129943, 138359, 129476 e 138541. `app/ponto/services/cartorio.py:1152–1164,1179–1231`

### FONTE

- **Base de leitura e de H09b** — `a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6`, a cópia anterior à mudança de impressão do V3, com v5 presente no código. O hash é o informado no enunciado e em D `em_curso`; a identidade Git local fica [nao verificado]. A fonte normativa do complemento é a ficha, o V3 aprovado e o C3 dos anexos 1–3. `app/docs/RELATO.md:100` `app/ponto/services/cartorio.py:161–164,493–578`
- **Testemunhas** — a leitura, a lavra e o carimbo da célula, com a mesma janela e relógio do ato. A medida D registra a ordem observada de leitura e escrita. Seus intervalos de fotos deixam transientes [nao verificado]. Anexos 6–9. `app/ponto/services/cartorio.py:1110–1123,1152–1157,1268–1273,1541–1558,2012–2037`

### MUDA

1. **Granularidade e cobertura do MUDA 1 do V3.** Dentro da vigência de MUDA 3, a mesma leitura do julgamento fornece as cinco presenças, chamado de batida total, vivo, vivo que protesta, vivo com fio C6 e vivo que tem fio C6 e protesta. Conservar o caso de vivo sem fio e o ramo sem adesão. Preservar a ordem da janela, a prioridade da decisão do admin e a cerca de MUDA 4 antes das portas atingidas, inclusive em cascata. `app/ponto/services/cartorio.py:1399–1415,1527–1547,1605–1661` `app/ponto/portas/celula.py:970–987` `app/folha/export.py:556–594`
   - **Classe 1 — recomputar após o fecho confirmado no corpo inicial.** Se o corpo inicial fechar a cobrança que motivou `COBRANCA_SEM_FURO` e restar candidato do lote sem protesto, recarregar a lista e o fio atingidos, para a mesma célula e janela, antes de lavrar. Pedir ao mesmo classificador os códigos e à própria porta o par a lavrar. Usar a ata e o par candidatos na cópia em memória da célula que a pergunta de fato referencia e consultar C6 com o relógio do ato. Reaproveitar o fecho adiado do lote quando a lavra proposta extinguir o fio. A ordem efetiva é primeiro carimbo, E2, fecho canônico do restante e segunda passada com o estado gravado. O gate real dá a última palavra e cada recusa mantém sua trilha. Esta escolha antecipa a leitura e mantém o E2, o fecho e a fila nos pontos existentes, com zero passada externa adicional. Ao surgir uma divergência da projeção ou fecho residual descoberto, aplicar C3. `app/ponto/services/cartorio.py:1123–1164,1578–1583,1614–1707,1726–1738,2012–2037` `app/ponto/portas/celula.py:970–1006` `app/chamados/juizes.py:866–898,1450–1469` `app/chamados/models.py:654–668,684–705`
   - **Classe 2 — comparar a projeção nas bordas da fábrica.** Nos atos com fábrica elegível, comparar por PK as cinco presenças antes e depois da etapa usando as mesmas leituras do cartório, com dados frescos na segunda foto. Incluir as células elegíveis do ato alcançáveis pelos efeitos em outros dias. Preservar o gatilho por emissão. Após aprovação deste complemento, uma mudança nessas presenças também chama a única releitura existente, sem `forcar`, com a janela original e por meio da guarda da impressão. Preservar o significado de `emitidos`, os contadores do ato e uma execução da fábrica. A escolha pega a primeira presença de fio em 138541 e pega 129476 se a interseção mudar. Evita abrir a releitura só por uma escrita com projeção igual, como se espera confirmar em 129490. Escrita relevante que sobreviver à última leitura volta ao C3 com célula, etapa e números. Anexo 8, células 138541, 129476 e 129490. `app/ponto/services/cartorio.py:141–157,1152–1231,1268–1273,1527–1558,1605–1617,1652–1653,2136–2146`
   - **C1 do 0215** — usar a etapa escritora já testemunhada pela D. **138541** ganhou fio na fábrica em P1, anexo 6 `seq 1347/1349 → 1385`, anexo 7 `162/164 → 200`, anexo 8 `1136/1138 → P2 1412`. **142995** ganhou pergunta e estado `em_analise` no repasse de órfãos do emissor em P1, anexo 6 `214/216/217/219 → 279` e anexo 7 `1238/1240/1241/1243 → 1303`. **143945** teve reaponte, `analisar()` e inserção no repasse de órfãos do emissor em P1, anexo 6 `299/301/303/305 → 338` e anexo 7 `1323/1325/1327/1329 → 1362`, com `fio:1` nas leituras. O braço `em_curso` traz 142995 e 143945 estáveis, anexo 8 `seq 204/1539/1639` e `269/1544/1644`. Isso fecha a atribuição por etapa na D. O escritor do ensaio histórico de `insumo.out` e o efeito individual das perguntas em 143945 ficam [nao verificado]. `app/ponto/services/cartorio.py:1179–1184,1209–1222,1952–1955` `app/ponto/management/commands/detectar_ausencias.py:788–807` `app/chamados/services/fabrica_da_celula.py:206–220` `app/chamados/services/disputa_emissao.py:1091–1099,1258–1284`
2. **C2 do 0215 — controle de R4, com MUDA 3 prevalecendo.** H09, aberta e já carimbada, reaponta pela porta existente a única pergunta viva entre dois chamados vivos do mesmo dia, dos quais só um protesta. Provar que muda a interseção fio × protesta e que a pergunta continua viva, `pk:status_local` e `fio:1` são iguais e não entrou outro fato. O esperado é a foto R4 estável. `app/docs/LEIS.md:145` `app/chamados/services/fio.py:956–984` `app/ponto/services/cartorio.py:502–507,543–547,1552–1558,1605–1617,1652–1653`
   - **H09b** usa a base `a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6` acima e a revisão com o V3 e este complemento. Em 2026-09, exigir `_K6=False` em ambos, a mesma foto inicial, PKs e insumos, o mesmo relógio em cada ponto de comparação e o mesmo caminho real com `forcar=False`, mantendo o sinal real. Aplicar `aberto → em_analise` com fio constante e demonstrar que os bytes de entrada e o SHA-256 legado mudam entre antes e depois em cada braço. Recarregar do banco e exigir igualdade byte a byte **entre os braços** após a transição, após o caminho de julgamento e nas fotos seguintes, para os quatro campos persistidos `impressao`, `ata`, `veredito` e `julgada_em`. Comparar UTF-8 dos valores canônicos, com chaves da ata ordenadas, ordem de listas preservada, `null` distinto de `[]` e o mesmo fuso e precisão de timestamp. Esses quatro campos são o perímetro do comparador H09b. Os demais campos e as escritas laterais ficam fora do comparador H09b. As provas de entrega e dinheiro seguem com o perímetro do V3. `app/folha/export.py:556–594` `app/ponto/services/cartorio.py:493–578,1541–1558,2174–2217` `app/chamados/catalogo/motor.py:109–112` `app/chamados/signals.py:189–218` `app/ponto/po

[truncado em 20000 de 30506 caracteres]

## 20/11 09:54 · sessao 1a750bcefa20 · E4C-F5-CONSTROI · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 20/11 09:50 · sessao eb15a60504c1 · TRAVAS-SEM-MEMORIA · verificador · origem=Code

**PROCEDE · TÉCNICO.** M1–M5 e M9–M9b podem ser construídos sem a validação de avais de T2. O recorte ainda precisa fechar a contagem de registros, as referências a A10, o selo e o smoke. **PROCEDE · LEI/DINHEIRO/!** A pre-opus da espinha precisa ter a exceção nominal conciliada com a opção A vigente. O pouso continua certificado na VM. `complemento.txt:3` · `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:44–59,87–112` · `app/docs/RELATO.md:65` · `app/docs/BACKLOG.md:2191,2194–2195`

`V4` nas referências abaixo é `rascunho_TRAVAS-SEM-MEMORIA-V4.txt`. A numeração dos anexos começa no primeiro literal após o cabeçalho de cada anexo. `V4:40`

## 1. As premissas conferem?

| Veredito | Conferência |
|---|---|
| **SEGUE — F9** | Está em **`V4:39`**, sob `FONTE`. O literal é **“0 devoluções por divergência demonstráveis, lista confirmada vazia, total real [nao verificado]”**. A data de 19/11 vem de `V4:38`. O BACKLOG de `TRAVAS-T2-AVAL-LE-A-COPIA` deve carregar esse qualificador. `complemento.txt:3` · `V4:38–40` |
| **SEGUE — limite da medida** | Os seis candidatos são identificados por título ou resumo. Faltam a primeira linha recebida, os alvos e prefixos colados, a hora do evento e os HEADs naquele instante. As 41 entradas fora do `main` são exposição no histórico. O total de devoluções e os avanços efetivos por volta ficam **[nao verificado]**. `V4:37–40,52` |
| **SEGUE — suite e Git** | A suite resolve `--volta` antes de julgar a seleção efetiva. Inteira sem motivo canônico sai `rc 2`. A volta roda ruff e selo e chama a suite. Régua e pre-push têm portas próprias, e o hook Git repassa argv e stdin. `bin/suite.sh:156–180,215–263,316–325` · `bin/volta_intermediaria.sh:18–47` · `bin/regua.sh:84–101,134–169` · `bin/hooks.sh:58–65` · `bin/pre-push.sh:27–74,159–200` |
| **SEGUE — ESTADO e ficha** | O medidor hoje cruza declarações e worktrees, devolve as só sujas por nome e ainda carece de HEAD por cópia. O wrapper recebe, confere e raspa anexos. O `PreToolUse` da cópia extrai papel, obra e anexos. O settings da cópia carece de `UserPromptSubmit`. `bin/raias_estado.py:51–135` · `bin/gerar_estado.py:211–224` · `bin/codex_pergunta.sh:57–68,99–100,323–335` · `bin/disjuntor.py:1448–1463,1837–1863` · `.claude/settings.json:2–73` |
| **SEGUE — prova disponível** | A interface da pista e exemplos de execução estão documentados. `HEAD = main = eb1e49c1e`, SHA do original aprovado, disponibilidade atual da pista, carga dos hooks na sessão viva e resultados dos testes propostos ficam **[nao verificado]**. `app/docs/BACKLOG.md:2191` · `app/docs/RELATO.md:201,1497,1529` · `V4:30,90–91` · `complemento.txt:1–3` |

## 2. Colide com aval vigente?

- **PROCEDE · LEI/DINHEIRO/! — espinha.** A opção A deixa a pre-opus da espinha na VM. O C1 de `PISTA-VOLTOU-1711` retirou justamente a ampliação da espinha e remeteu a rota à `PISTA-CERTIFICA-POUSO`. M2 traz a exceção nominal aprovada para **“nem a suite que o opus recebe”**. Falta explicitar que ela também substitui a rota da opção A **só para a pre-opus da espinha admitida por M2–M4**, e dizer como essa exceção se aplica à condição das três peças. Isso alcança L1b e o job real do PRONTO 2. `complemento.txt:1–3` · `V4:45,75,107,118` · `app/docs/PROMPTS.md:2459,2462` · `app/docs/RELATO.md:65` · `app/docs/BACKLOG.md:2191,2194–2195`
- **SEGUE — ficha e piloto.** O V4 registra expressamente a troca de forma e autoria do desenho e a inclusão da recusa mecânica. O complemento aprova o desenho A no registro Bash. O rito de `APROVA RASCUNHO`, a condição para virar padrão e as guardas do wrapper seguem pelos avais citados na transcrição. Os literais originais fora dela ficam **[nao verificado]**. `complemento.txt:1–3` · `V4:5–11,26–28,57–59,100,123–125` · `bin/codex_pergunta.sh:241–259`
- **SEGUE — §7b e executor.** A sessão constrói em cópia nas esperas da F5, o Codex verifica o diff e o instrumento tem pouso próprio. O contrato vem antes do código. O PRONTO registra pendências após mais de três voltas e a inteira verde continua sendo a prova antes do opus. `complemento.txt:3` · `V4:44,48,73,106–107` · `CLAUDE.md:309–319,325–331,369–394` [nao verificado]
- **SEGUE — parada.** `PAREI RASCUNHO` registra a recusa da obra e deixa a fila andável seguir. Deve preservar a janela útil do topo e o `ESPERANDO` comprovado. O Stop já tem suas próprias condições para devolver o turno. `V4:58,82,91` · `CLAUDE.md:262–275` · `bin/hook_stop_fila1.py:413–524,1248–1277,1381–1404`

## 3. O que não fecha no literal?

| Veredito | Condição a fechar |
|---|---|
| **PROCEDE · TÉCNICO — rota sem chave** | M2 manda recusar inteira pre-opus sem `PISTA_LATERAL`. Falta delimitar quais pedidos entram nessa porta, incluindo o comando canônico sem `--dir`, e como distinguir uma recusa de admissão da contingência VM após admissão. V2 deve medir **`testes_rc=2` e `rc=1` da volta**, pois o agregador converte o vermelho. `V4:45–48,74–76` · `CLAUDE.md:95–106` · `bin/suite.sh:129–150,255–263` · `bin/volta_intermediaria.sh:31–47` |
| **PROCEDE · TÉCNICO — identidade e retorno da pista** | Fixar qual RELATO operacional fornece o marcador do SHA. Exigir o marcador dentro do próprio commit que ele nomeia cria autorreferência. Na execução, vincular seleção, PRONTO, ref e job ao SHA conferido e definir o tratamento das opções sem equivalente remoto documentado, como `--parallel auto`, flags extras e `--seco`. O job real deve provar `Ran`, veredito e rc, com o limite da espera explicitado. `V4:45–48,63–66,73–76,107` · `bin/suite.sh:134–146,197–213,265–270` · `app/docs/BACKLOG.md:2191` · `app/docs/RELATO.md:65,81` |
| **PROCEDE · TÉCNICO — união de M5** | O dicionário atual por basename pode colapsar duas worktrees de mesmo nome. M5 promete um token por cópia resolvida. Definir identidade e representação de nomes no ESTADO, e cobrir declaração com caminho cujo HEAD falha. E1 cobre declaração sem worktree. `V4:49–50,81` · `bin/raias_estado.py:42–48,73–109,125–135` |
| **PROCEDE · TÉCNICO — prova da ficha** | Fechar como o hook e o wrapper recebem os mesmos bytes da ficha. O leitor atual de `--anexo` termina no espaço, enquanto o wrapper recebe o argumento inteiro. Fixar a raiz de resolução da referência, o commit de `main` observado e a identidade do `r.txt` com seu `.id` ou registro. FD1–FD6 ainda deixam sem prova troca de conteúdo, `.id` incompatível, cabeçalho incorreto e as duas formas adicionais de `DECISAO`. `V4:26,57–58,82–83` · `bin/disjuntor.py:855–903,1448–1463` · `bin/codex_pergunta.sh:57–68,323–335,464–469` |

## 4. Toca exportada, dinheiro ou `!`?

- **SEGUE — alcance declarado.** O recorte descreve roteamento de testes, metadados Git, ficha e documentação. As portas de exportada, dinheiro, escala, vínculo e regra de negócio permanecem fora. A pista recebe árvore commitada e SHA/opções de teste. O smoke usa destino e dados de fixture pela publicação raspada. Efeito em produção **[nao verificado]**. `V4:44–59,90–100,144` · `app/docs/BACKLOG.md:2191` · `bin/relato.sh:30–38,114–147` · `CLAUDE.md:265–268,401–417`
- **PROCEDE · LEI/DINHEIRO/! — limite do desvio.** V3 pode provar a contingência **já registrada** para recorte lateral, com uma linha de causa. Estendê-la à espinha ou usá-la para passar por recusa de PRONTO, chave inválida, vermelho de teste ou selo exigido extrapola o literal. O pré-voo da inteira e a certificação do pouso continuam devidos. `V4:45–48,74–77,98,107` · `app/docs/RELATO.md:65,81,1010,1748` · `CLAUDE.md:265–268,320–328,372–381`

## a. Dependências dos itens retirados

| Veredito | Dependência e ajuste |
|---|---|
| **PROCEDE · TÉCNICO — M1 e instalação** | Dois registros viram **um registro Bash novo**, compartilhado por T1 e ficha. Ajustar M1, contrato 1 e 5, prova de carga, PRONTO 5, mão do Ronald 1/3 a 3/3 e desligamento. Sai a comprovação de M8 na sessão de trabalho. O `PreToolUse` preexistente está configurado com matcher vazio e é parte das guardas vigentes. `V4:44,47,58,63,67,88–94,110,125,132,141` · `.claude/settings.json:62–72` |
| **SEGUE — M2–M4** | PRONTO por obra/SHA, executor, recorte e contingência da pista não dependem de M6–M8 nem da alternativa T2. O problema de literal da rota continua o das perguntas 2 e 3. `V4:45–48,73–77,107` · `bin/suite.sh:156–180,215–263` |
| **PROCEDE · TÉCNICO — M5** | Ficam medidor, HEAD completo, publicação HEAD12 e `SEM WORKTREE`. Sai a frase “o hook lê o mesmo medidor na hora do envio”, cujo consumidor era T2. Saem a sintaxe de alvos/prefixos, leitura recente, delta, devolução e marco de M6–M8; E1/E2 são independentes. `V4:49–54,78–81,108,130–131,135,142–143` · `bin/gerar_estado.py:211–224` |
| **PROCEDE · TÉCNICO — M9/M9a** | O desenho A usa M3. O desenho B dependia do registro de T2 para declarar custo de zero registros adicionais. A10 sai do RED, selo da ficha e PRONTO 5. A obrigação de deixar passar leitura, sonda e despacho continua e precisa ser provada diretamente no Bash da ficha. `V4:26–28,57–58,79,82,89,98–100,110,137,151` · `bin/disjuntor.py:1837–1863` |
| **SEGUE — M9b** | Depende do rito de publicação e SHA de M1. Devoluções por origem e recolagens efetivas são medidas da ficha, independentes de F9 e M6b. A descrição do desenho B fica como comparação histórica, com o custo referido à base que incluía T2. `V4:20,26–28,39,44,52,59,112` |
| **PROCEDE · TÉCNICO — referências restantes** | Remover `U` da fixture, A1–A14, referências cruzadas e os bloqueios dados como respondidos por recado, corte, delta e recolagem de T2. Na `LEI-AKITA`, substituir `P1–E2` pela enumeração dos grupos sobreviventes. F8/F9 continuam como fonte do registro no BACKLOG. `V4:38–39,71–83,114,127–143,151` · `complemento.txt:3` |

## b. PRONTO e smoke depois do complemento

| Item | Veredito e redação resultante |
|---|---|
| **PRONTO 1** | **PROCEDE · TÉCNICO — muda.** Cinco linhas do contrato recortado no RELATO, um envio ao Codex, diff e configuração local conferidos pela sessão e pelo verificador. Medir **um** registro Bash novo. `V4:44,63–67,106` · `CLAUDE.md:369–394` |
| **PRONTO 2** | **SEGUE — fica**, sujeito à resolução da espinha. P1–P5, V1–V3, L1a–L1d, L2–L4, R1 e G1–G2 com contagens por porta. Job real de pre-opus da espinha com id, SHA, `Ran`, veredito e rc, zero jobs sem PRONTO, registro de suspensão em divergência e pouso certificado na VM. `V4:73–77,107` · `app/docs/BACKLOG.md:2191` |
| **PRONTO 3** | **SEGUE — muda para “M5: E1 e E2”.** E1 prova três tokens e uma declaração `SEM WORKTREE` no gerado e no publicado. E2 prova a troca só do token da cópia que avançou. Saem A1–A14, topo de devolução, continuidade e marco de T2, prova de delta e contagens de recolagem da validação. `complemento.txt:3` · `V4:78–81,108` · `bin/relato.sh:37–38,114–147` |
| **PRONTO 4** | **PROCEDE · TÉCNICO — muda.** Ficam RED na base pelo motivo da regra, controle íntegro verde e smoke com sessão, digest, até 10 min e encerramento. Pela leitura literal de “do PRONTO 4 [...] saem os mutantes”, saem os sete: retirar T1/P1, retirar T2/A2, aceitar qualquer SHA/P2, ignorar segunda cópia/A4, omitir HEAD/E1, aceitar qualquer descendente/A12 e ignorar idade/A13. Se o corte era só dos mutantes **de T2**, os três sobreviventes são P1, P2 e E1. Sem eles, os casos funcionais ficam e perde-se a prova por mutação dessas três costuras. `complemento.txt:3` · `V4:87–90,109` | [nao verificado]
| **PRONTO 5** | **PROCEDE · TÉCNICO — muda.** FD1–FD6, três recusas literais com zero chegadas, três entradas com uma chegada e próximo andável registrado após cada recusa. Conferir um registro novo. Sai A10. Sem uma passagem direta pela porta do (d), fica sem prova que leitura, sonda e despacho comuns chegam às guardas atuais. `V4:79,82–83,89,110` · `bin/codex_pergunta.sh:241–259` |
| **PRONTO 6** | **SEGUE — fica.** RED, controle e mutante próprio da ficha, com FD1 e FD3 vermelhos. No mesmo smoke, uma recusa e uma entrada com contagem, sessão e digest. Esse mutante está no PRONTO 6. `complemento.txt:3` · `V4:89,91,111` |
| **PRONTO 7** | **SEGUE — fica.** Dois ou três desenhos medidos, um redigido inteiro, uma linha por ID/SHA com origens, referências e recolagens efetivas. Se o chat escolher outro desenho, registrar pedido e novo SHA. `V4:59,112` |
| **Smoke pré-pouso** | **PROCEDE · TÉCNICO — muda.** Ronald abre na raiz candidata pelo comando e teto de V4. Com SSH, runners, nomes e destino de publicação da fixture, provar carga do Bash por P1/P3 e FD1/FD2, V1, E1 e continuidade após a recusa da ficha. Registrar sessão, digest, tempo e encerramento. Saem `UserPromptSubmit`, A1/A2/A3/A8 e a prova de ativação M8. Evento ausente ou timeout segura o pouso. O job real fica no PRONTO 2. `V4:90–94,107,111` · `.claude/settings.json:3–73` · `bin/relato.sh:23–38` |
| **Instalação manual** | **PROCEDE · TÉCNICO — muda.** Se houver negativa, as três etapas usam o trecho de **um** registro, backup, JSON válido, digest e prova do Bash na raiz candidata. Na instalação de trabalho, provar a carga desse Bash. Sai o marco M8. Os comandos efetivos da descartável precisam apontar para a fixture, pois o settings entregue tem caminhos absolutos da árvore de trabalho. `V4:92–94` · `.claude/settings.json:9,20,29,40–57,68` · `app/docs/RELATO.md:3341,4356,4411` |

## c. Arquivos e selos

- **SEGUE — código e configuração.** M2–M4 tocam `bin/suite.sh`, a integração de `bin/volta_intermediaria.sh` e o verificador comum do registro Bash e do executor. M9a usa esse mesmo registro em `.claude/settings.json`. O V4 cita `bin/disjuntor.py` como molde e leitor atual, sem fechar se o verificador novo mora ali ou em arquivo próprio. O wrapper é consumidor e alvo de regressão. `V4:44–48,58,87–89` · `bin/disjuntor.py:1448–1463,1796–1798,1837–1863` · `bin/codex_pergunta.sh:323–335` · `.gitignore:66`
- **SEGUE — medidor e docs.** M5 muda `bin/raias_estado.py` e a apresentação consumida por `bin/gerar_estado.py`. `bin/raia_agente.sh listar` também lê o compositor e `bin/relato.sh` publica o resultado. M1/M4/M9* escrevem evidências em `app/docs/RELATO.md`, M3 lê também `app/docs/RELATO-ARQUIVO.md` e o complemento manda registrar o item em `app/docs/BACKLOG.md`. Esses são caminhos sob `app/` de documentação operacional. O escopo descrito não pede edição de código Django. `complemento.txt:3` · `V4:44,47–49,57–59,98–100` · `bin/raia_agente.sh:80–81` · `bin/gerar_estado.py:211–224` · `bin/relato.sh:114–130`
- **PROCEDE · TÉCNICO — selos de host.** O selo novo `bin/tests/test_travas_sem_memoria.sh` entra pelo glob da régua. A expectativa nova é rota remota admitida e tokens no ESTADO. Os selos antigos continuam cobrando motivo e seleção efetiva, trava no ramo VM, montagem, escritor único e guardas do wrapper. `test_suite_motivo.sh` tem âncoras textuais e `test_suite_sh.sh` tem regex do comando canônico. Se forem atingidas pelo diff, adaptar a fixture ou a âncora com a mesma prova. `.git/hooks`, `bin/hooks.sh`, `bin/pre-push.sh` e `bin/regua.sh` são caminhos de regressão em R1/G1/G2. `V4:77,87–89,107` · `bin/regua.sh:134–145` · `bin/tests/test_suite_motivo.sh:14–42,159–215` · `bin/tests/test_suite_sh.sh:22–69` · `bin/tests/test_raia_declarada_no_estado.sh:29–41,57–84` · `bin/hooks.sh:58–65`

## d. Contrato de cinco linhas

**PROCEDE · TÉCNICO.** O V4 traz as cinco rubricas. O texto recortado ainda precisa explicitar a espera, a medida e o desligamento da publicação de M5, além de remover as referências ao registro retirado. `V4:44,49,63–67,81` · `CLAUDE.md:394`

| Linha | Como fica |
|---|---|
| **Quem chama** | Sessão/raia chama suite ou volta pelo Bash da raiz. A sessão entrega a ficha no mesmo registro Bash. `relato.sh` chama o gerador, que lê o medidor. `V4:45–49,58,63` · `bin/relato.sh:37–38` · `bin/gerar_estado.py:215–224` |
| **Quem espera** | A sessão espera o job em shell de fundo e recebe a decisão da ficha antes do wrapper. O chat lê o rascunho no RELATO. O publicador espera o gerador e a raspagem antes de publicar. `V4:64` · `bin/relato.sh:37–38,114–147` |
| **Falha/limite** | Recusa restrita ao pedido ou à ficha. Pista só com commit admitido, contingência no alcance vigente e suspensão por divergência. M5 publica `SEM WORKTREE` para declaração sem HEAD resolvido. Fixar o limite do job e da espera da pista. `V4:45–49,58,65,90` · `app/docs/BACKLOG.md:2191` |
| **Medida** | Porta, obra, SHA, rc/veredito, chamadas SSH/VM, id e tempo do job. Na ficha, chegadas ao wrapper, devoluções por origem e recolagens. No ESTADO, E1/E2 com HEADs de referência e comparação do gerado com o publicado na mesma geração. `V4:59,66,73–83,108,112` | [nao verificado]
| **Quem desliga** | Ronald restaura a configuração anterior ao registro Bash novo, retira a seleção da rota e registra no RELATO. Falta nomear a reversão da saída de M5 pelo pouso próprio do instrumento. `V4:49,67` · `CLAUDE.md:325–328,394` | [nao verificado]

## e. Ordem em esperas da F5

| Passo | Veredito, RED e selo |
|---|---|
| **1. Fechar M1 e a fixture** | **PROCEDE · TÉCNICO.** Fechar o texto recortado e a exceção da espinha, publicar o contrato e enviar uma vez. Capturar o RED de P1, E1 e FD1 na base com fixture íntegra. O selo despacha o evento pelo settings candidato e conta os efeitos. `complemento.txt:3` · `V4:44,63–73,81–89` · `CLAUDE.md:28–29,394` |
| **2. M5** | **SEGUE.** RED E1/E2 com Git real, HEAD conhecido e publicação de fixture. Selo no medidor e na publicação raspada, incluindo as só sujas e o leitor `raia_agente.sh listar`. Isolar também o provedor de nomes, pois `DEST_RELATO` sozinho troca só o destino. `V4:49,81,87,108` · `bin/relato.sh:23–38,114–119` · `bin/relato_nomes.sh:8–23` · `bin/raia_agente.sh:80–81` |
| **3. M2–M4 em duas esperas** | **SEGUE.** Primeiro V1–V3, L2–L4 e R1/G1/G2 para rota e regressões. Depois P1–P5 e L1a–L1d para PRONTO e executor. Selo com SSH e runners falsos, seleção efetiva, chamada via settings e chamada direta ao executor. A prova do job real entra após resolver a exceção da espinha. `V4:45–48,73–77,87–88,107` · `bin/suite.sh:162–180,215–263` |
| **4. M9/M9a, depois M9b** | **SEGUE.** RED FD1–FD6 pelo mesmo registro Bash com contagem das chegadas. Selo com controle íntegro, mutante da ficha e passagem direta das chamadas comuns. Para M9b, a prova é um rascunho de fixture conferido contra o rito de publicação, SHA, origens e recolagens. `V4:57–59,82–89,110–112` |
| **5. Integração e pouso próprio** | **SEGUE.** Rodar o selo completo com as regressões de host, conferir diff com o verificador e executar o smoke recortado. Fechar PRONTO 2 com a prova real e certificar o pouso na VM. Distribuir os passos pelas esperas comprovadas da F5. Duração e disponibilidade dessas janelas **[nao verificado]**. `complemento.txt:3` · `V4:87–94,106–112` · `bin/regua.sh:134–145` · `CLAUDE.md:316–328,372–381` |

## 20/11 09:49 · sessao 06588a80f15d · E4C-F5-CONSTROI · verificador · origem=Code

**AVAL-DEVOLVIDO no item 1.** O mesmo `atomic` pode produzir A ou B. A frota raspada registrou A=0 e B=0 em 51.953 perguntas. A ocorrência em produção e a remedição na árvore final estão [nao verificado]. `f5_vd2_frota.out:3–8` `app/chamados/services/disputa_emissao.py:1480–1534,1629–1658` `app/chamados/services/materializacao.py:1033–1069`

**Item 2 mantém o PAREI POUSO** até a prova nominal das 16. **Item 3 SEGUE** com a isenção restrita ao sítio e o tripwire. `f5_ato_x_coluna_invariante_2011_0545.txt:12,14` `app/chamados/tests/test_e4c_f5_selo_chamada.py:161–179,265–279`

## Item 1 — Ato × coluna

| Pergunta | Veredito | Motivo |
|---|---|---|
| **1. Premissas** | **PROCEDE · TÉCNICO** | As 11 guardas ficam no ato e a fila do materializador ainda seleciona pela coluna. O censo confirma uma sequência para A e duas para B. A equivalência prometida falha mesmo com as escritas bem-sucedidas. `f5_vd2_frota.out:51–78` `app/chamados/services/materializacao.py:2111–2132` `app/chamados/services/disputa_emissao.py:1483–1534,1629–1658` `app/chamados/services/materializacao.py:1033–1069` |
| **2. Avais vigentes** | **PROCEDE · LEI/DINHEIRO/!** | O 0815 exclui mover casos em entregue/exportada pelas portas da volta de escrita e o 1710 proíbe tocar entregue. B libera escritas de pergunta sem veto geral de entrega. A classe vii é exceção expressa ao 2050 para os testes descritos. O 0110 só aceita delta de leitura medido e atribuído. `e4c_f5_constroi_1911_0815.txt:3` `e4c_f5_constroi_a_1911_1710.txt:12` `f5_classe_nao_caso_1911_2050.txt:3–6` `f5_chokepoint_teto_medido_2011_0110.txt:2–5` `f5_ato_x_coluna_invariante_2011_0545.txt:6` `app/chamados/services/fio.py:1394–1409` |
| **3. Condição literal** | **PROCEDE · TÉCNICO** | O censo e o selo propostos verificam a presença de um par no `atomic`. Falta exigir coerência da ordem dos atos e cobrir o caminho inverso, em que o ato desfaz a validação e o `UPDATE` omite a coluna. `f5_ato_x_coluna_invariante_2011_0545.txt:5–10` `app/chamados/services/disputa_emissao.py:1483–1534,1629–1658` |
| **4. Exportada, dinheiro ou `!`** | **PROCEDE · LEI/DINHEIRO/!** | Há caminho de escrita da pergunta antes da guarda K6 da validação. A validação que passa pode chegar a batida e ao recálculo do gravado. POST efetivo, diferença em folha e valor pago estão [nao verificado]. `app/chamados/services/acoes_disputa.py:349–385` `app/chamados/services/validacao.py:737–757` `app/chamados/services/materializacao.py:1493–1503` `app/ponto/services/fechamento.py:1798–1824` |

**P1a — PROCEDE · TÉCNICO.** Há **sete pontos ORM de produção** que persistem a coluna, com **oito sítios** se os dois chamadores do `UPDATE` comum forem contados separadamente. A relação do aval sobra com duas atribuições em memória e falta o `force_insert`. A correspondência abaixo usa as âncoras de main fornecidas pelo aval. O checkout independente de main está [nao verificado]. `f5_ato_x_coluna_invariante_2011_0545.txt:5` `app/chamados/models.py:1569–1589,1719–1722,3229–3234,3372–3386,3508–3511`

| Forma | Escritores na cópia |
|---|---|
| `save(update_fields=...)` | `_carimbar_validacao`, `devolver_validacao_recusada`, `devolver_paridade_bug_par_103`, `responder_pergunta_disputa_core` no ramo que inclui a coluna e `PerguntaDisputa.carimbar` no ramo `resposta='validar'` com texto e coluna nula. `app/chamados/services/validacao.py:168–175,270–283,350–358` `app/chamados/services/disputa_emissao.py:1483–1496,1527–1534` `app/chamados/models.py:3350–3372` |
| `QuerySet.update` em lote | `descarimbar_em_lote` recebe `validada_em=None` de `ressuscitar_perguntas` e `reabrir_para_correcao`. A chamada L-130 usa o mesmo helper com campos de resposta. `app/chamados/models.py:1719–1722,3508–3511` `app/chamados/services/disputa_emissao.py:1617,1656` `app/chamados/services/validacao.py:429–446` |
| `force_insert` e outros `bulk` | `_nascer` insere a coluna, nula por padrão, e registra a linha correspondente se receber validação em `**campos`. O `bulk_update` localizado limita-se à lavra. O `bulk_create` localizado insere `DecisaoPergunta`. `app/chamados/models.py:2659,3229–3234,4290–4295,4373–4375` `app/chamados/veredito_lavrado.py:237–250` |
| Sobras | `models.py:1573` e `:3360` da lista de main correspondem, pelo censo da cópia, à sondagem em cópia e à restauração em memória. `app/chamados/models.py:1569–1589,3373–3386` |

**P1b — PROCEDE · TÉCNICO, PAREI expresso.** Em B, com resposta de hora, motivo de recusa e `materializada_em` nula, a guarda pelo ato pula o bloco que limpa a coluna e grava `DEVOLUCAO`. O caminho pode seguir para a reabertura da disputa. O teste atual mostra que a guarda pela coluna limpa B. `matriz_c3.txt:13` `app/chamados/services/validacao.py:262–295` `app/chamados/tests/test_e4c_f5_voltaD_portas.py:383–392`

**P3a — PROCEDE · TÉCNICO.** **Não.** “Para CADA escritor da coluna validada_em” alcança a função como sítio, mas a pergunta “se o ato é gravado ou desfeito na MESMA transação” só percorre coluna → ato. No ramo com instância antiga, `RESPOSTA(False)` é gravada e `validada_em` fica fora de `_campos`. `f5_ato_x_coluna_invariante_2011_0545.txt:5,7` `app/chamados/services/disputa_emissao.py:1483–1496,1517–1534`

**P3b — PROCEDE · TÉCNICO.** `ABSORCAO` satisfaz a leitura literal de **qualquer ato** no mesmo `atomic`. O par de **validação** fica sem linha `VALIDACAO` real. A composta lê a validação sintética no instante da coluna. Uma `RECUSA` posterior vence essa linha e a composta devolve validação nula. `f5_ato_x_coluna_invariante_2011_0545.txt:5` `app/chamados/services/materializacao.py:1060–1069` `app/chamados/models.py:4290–4303,4333–4362`

**P3c — PROCEDE · TÉCNICO.** “Todos sim = segue” aceita a reabertura porque o par existe. A resposta sem coluna fica fora das duas frases como **caminho**. A absorção passa como “sim” se qualquer ato valer e cai em “algum não” se o aval exigir a `VALIDACAO` correspondente. Essa distinção precisa estar no literal. `f5_ato_x_coluna_invariante_2011_0545.txt:5,7,9` `app/chamados/services/disputa_emissao.py:1483–1534,1629–1658` `app/chamados/services/materializacao.py:1060–1069`

| Sequência | Menor cura da corrida |
|---|---|
| **A · Reabertura** | Colher `ato_em` depois da trava, junto da foto que já é lida depois dela. Basta mover o relógio. `app/chamados/services/disputa_emissao.py:1629–1656` |
| **B · Resposta** | Reler a pergunta sob a trava existente antes de colher a foto do passivo e montar `update_fields`. Limpar a assinatura no mesmo `save` quando a releitura trouxer a coluna preenchida. `app/chamados/services/disputa_emissao.py:1476–1499,1517–1534` |
| **B · Absorção** | Reler a irmã e sua elegibilidade sob a trava e usar o instante daí para carimbo e `ABSORCAO`. Na sequência descrita, a recusa já limpou o texto e a coluna, e `carimbar` só preenche a validação com texto. `app/chamados/services/materializacao.py:1033–1069` `app/chamados/services/disputa_emissao.py:1617,1656` `app/chamados/models.py:3350–3357` |

Essas três curas de concorrência cabem nos escritores atuais e dispensam ato de reparo, backfill, campo novo, volta da guarda à coluna e migração da fila. A eventual decisão de fazer a absorção gravar **validação real nova** pede autorização sobre o significado do ato. A redução vigente preserva a validação em `ABSORCAO`. `f5_ato_x_coluna_invariante_2011_0545.txt:5,9` `app/chamados/models.py:4297–4303,4342–4351` `app/chamados/services/materializacao.py:1060–1069`

**P3d — SEGUE para as curas locais.** Reabertura reaproveita a foto existente. Resposta e absorção acrescentam leitura se fizerem `SELECT` novo. O selo está em `app/ponto/tests/`, cobre batida, `_gravar_estado` e seus desdobramentos, e não exercita essas duas portas nos cenários medidos. Alterar `carimbar` genericamente alcançaria o resolvedor S143. Delta e suite estão [nao verificado]. `app/ponto/tests/test_selo_chokepoint_escrita.py:319–329,356–403,438–510` `app/chamados/services/disputa_emissao.py:2025–2033` `app/chamados/signals.py:107–119`

**P3d · harness — SEGUE.** As curas locais podem ser verificadas com ORM real. A opção B da pureza é que mexeria com harnesses que injetam `PerguntaDisputa`, como os de `test_e3_degrau_dono_e_porta.py:510–511` e `test_e3_r3_c6_sem_banco.py:290–291`. Um refresh só de campos pode conservar o prefetch velho de `decisoes`. `resposta_f5_duble_1911_1945.txt:2–7` `f5_classe_nao_caso_1911_2050.txt:3–5` `app/chamados/tests/test_e4c_f5_selo_chamada.py:165–170` `app/chamados/tests/test_e4c_f5_voltaD_portas.py:371–381`

**P3e — PROCEDE · TÉCNICO.** Hoje **(a)** só pode ser testado como RED para as três intercalações. Use duas conexões, barreira no trecho anterior à trava, portas reais e releitura nova após os dois commits. Afirme a equivalência e o desfecho esperado do ato. K47 e os REDs atuais fabricam divergência com `update` direto. `app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:220–254` `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197–216` `app/chamados/tests/test_e4c_f5_voltaD_portas.py:33–60`

Para **(b)**, reintroduza cada janela como mutante e exija que o teste falhe. Para o mutante que suprime `VALIDACAO`, confira também a linha real, pois o fallback sintético pode deixar a equivalência verde. A âncora de `retry_reativa` confere estaticamente. Execução do mutante e A/B finais estão [nao verificado]. `app/chamados/models.py:4290–4295,4333–4351` `app/chamados/tests/bug_par_mutantes.py:68–71` `app/chamados/services/validacao.py:683–689` `f5_ato_x_coluna_invariante_2011_0545.txt:6–10`

**P4a — PROCEDE · LEI/DINHEIRO/!.** Passar a guarda em B tem efeitos diferentes por porta. As condições abaixo continuam necessárias. A sonda não executou os POSTs reais. `app/chamados/services/acoes_disputa.py:338–385` `app/chamados/services/validacao.py:888–929` `app/chamados/services/declaracao_texto.py:336–350,651–670` `f5_vd2_frota.out:91–92`

| Em B | O que pode acontecer e qual guarda existe |
|---|---|
| **Corrigir e recusar** | Corrigir pode gravar nota e `RESPOSTA(False)` antes de chamar `validar_pergunta`. Recusar ainda exige resposta vigente e pergunta dentro do fato, depois grava motivo e `RECUSA` e limpa os campos. Nessas escritas iniciais, a cerca de entrega só age dentro de ato cercado. Assim podem tocar a pergunta em entregue se os demais requisitos passarem. `app/chamados/services/acoes_disputa.py:349–385` `app/chamados/services/validacao.py:900–929` `app/chamados/models.py:3497–3511,3697–3707` `app/chamados/services/fio.py:1394–1409` |
| **Declarar e aplicar declaração** | Com marco faltante e pergunta elegível, declarar grava resposta e aguarda validação. Aplicar pode gravar resposta antes de chamar a validação. O prefixo confere marco e proposta, sem veto geral de entrega à escrita da resposta. `app/chamados/services/declaracao_texto.py:332–350,365–428,617–637,651–699` `app/chamados/services/disputa_emissao.py:1517–1534` |
| **Validar, lote e irmã** | B pode liberar carimbo individual, entrada do lote e carimbo da irmã. O retorno do lote só contabiliza. K6 veta **metades de intervalo** pelo dia do fato antes do carimbo e é conferido de novo no fluxo de hora. Fora desse alcance, F6 veta **criar batida institucional** se o `FechamentoMensal` estiver aprovado/exportado. São guardas distintas. `app/chamados/services/validacao.py:641–644,683–694,737–757,779–785,1067–1098` `app/chamados/services/materializacao.py:808–832,1872–1876` `app/ponto/registro_batida.py:147–161` `app/ponto/janelas.py:75–89` |
| **Fecho e materializador** | B pode habilitar o carimbo sem credor, sujeito ao C6 e à cerca de entrega do resolvedor. A releitura do materializador barra B e o plantio que exige irmã validada também a barra. No outro ramo do par, B faz voltar a checagem de implausibilidade. `app/chamados/services/disputa_emissao.py:1913–1925,2080–2106` `app/chamados/services/materializacao.py:175–208,2123–2133` |

**Efeito em dinheiro.** Corrigir, aplicar ou validar podem chegar ao plantio e ao recálculo de `FechamentoMensal` e `DiaPago`. O recalculador tem guarda temporária específica da 09 e registra recálculo de exportadas fora dela. Valor efetivamente alterado, alteração de TXT entregue e ocorrência em produção estão [nao verificado]. `app/chamados/services/validacao.py:745–757` `app/chamados/services/materializacao.py:1493–1503` `app/ponto/services/fechamento.py:19–43,539–547,1160–1196,1798–1824`

**P4b — SEGUE.** `vigiar_fio_morto` já percorre o universo de perguntas, chama o contador de texto e publica o carimbo. É o encaixe mais direto. `vigiar_carimbo_sem_registro` já é encadeado, mas seu censo próprio recorta perguntas de hora validadas. `auditar_invariantes_chamados` também roda e já tem contadores da família. `app/config/crons.py:542,1737–1739,1775–1777` `app/chamados/management/commands/vigiar_fio_morto.py:186–196,268–291` `app/chamados/management/commands/vigiar_carimbo_sem_registro.py:42–71` `app/chamados/management/commands/auditar_invariantes_chamados.py:1085–1094`

O contador A/B pode ler `composta` e a coluna em lote, com dono ESTRUTURA e **juízes novos = 0**. O contador F7 já existente mede falta de linha física, uma condição diferente. Se o encaixe em `handle` introduzir um novo global, o harness em memória que recompila esse método precisa receber o dublê correspondente. `app/chamados/services/vigia_texto_resposta.py:11–20` `app/chamados/management/commands/backfill_decisoes_pergunta.py:95–118` `app/chamados/tests/test_e4a2_c5_memoria.py:118–143` `app/core/juizes.py:190`

**P4c — PROCEDE · LEI.** “Escritor único” existe nos dois documentos. O desenho do 0545 conserva vários escritores da coluna e pede pareamento transacional e um selo de contrato. Isso realiza uma *fitness function* sobre o invariante. A projeção com um escritor está na E4D. A L-123 exige que o `padrao=` descreva o que a cura realiza. `app/docs/GLOSSARIO.md:12` `app/docs/PLANO-PADROES.md:54–67` `app/docs/BACKLOG.md:2023` `f5_ato_x_coluna_invariante_2011_0545.txt:3–10,21`

## Item 2 — Display

| Pergunta | Veredito | Motivo |
|---|---|---|
| **1. Premissas** | **SEGUE** | O anexo 4 dá 16 diferenças literais e zero nas duas comparações indicadas. A property da cópia lê o texto do ato e formata posto, escala e opções. `f5_property_display_copia.out:3–9` `app/chamados/models.py:3836–3863` |
| **2. Avais vigentes** | **SEGUE** | O 0545 dá uma exceção nominal à condição de pouso do 1830. A prova pedida é de leitura e cabe com a implementação atual de `resposta_display`. `respostas_f5_constroi_2_1911_1830.txt:6,11` `f5_ato_x_coluna_invariante_2011_0545.txt:12` `app/chamados/models.py:3836–3863` |
| **3. Condição literal** | **PROCEDE · TÉCNICO para liberar o pouso** | As duas contagens zero comparam ato e coluna na sonda. Falta a comparação da saída exibida em main × cópia para cada um dos 16 PKs. Resultado [nao verificado]. `f5_property_display_copia.out:7–9` `f5_ato_x_coluna_invariante_2011_0545.txt:12` | [nao verificado]
| **4. Exportada, dinheiro ou `!`** | **SEGUE** | A prova solicitada percorre caminhos de leitura de texto. `app/chamados/models.py:3836–3863` `app/api/views.py:2566–2582` `app/templates/chamados/partials/modal_fio.html:217–221` |

**P3f — PROCEDE · TÉCNICO para a prova nominal.** Na cópia, `_pergunta_card.html` chama `perg.resposta_display`. Meu Atendimento usa a property com fallback para `resposta_colab`. `api_chamados_v2` e `api_disputa_perguntas` entregam ambos os campos. O modal do fio imprime `perg.resposta_colab` diretamente e usa `leitura_da_resposta` para o diagnóstico de horário. `app/templates/chamados/partials/_pergunta_card.html:29–43` `app/templates/chamados/partials/_card_atendimento.html:105–106,141–143,185–186,394` `app/api/views.py:1148–1165,2538,2566–2581` `app/templates/chamados/partials/modal_fio.html:217–221,274–287` `app/chamados/templatetags/chamados_tags.py:145–176`

Para main, o 1830 documenta que `resposta_display` lê o ato. Os chamadores naquele checkout, o campo que o cliente nativo efetivamente renderiza e a igualdade das saídas para os 16 estão [nao verificado]. `respostas_f5_constroi_2_1911_1830.txt:6` `f5_ato_x_coluna_invariante_2011_0545.txt:12`

## Item 3 — Pureza

| Pergunta | Veredito | Motivo |
|---|---|---|
| **1. Premissas** | **SEGUE** | A referência está em `projecao_temporal_da_pergunta`, linha **1401** da cópia. O aval informa main `:1398`. Identidade textual com main [nao verificado]. `app/chamados/juizes.py:1383–1403` `f5_ato_x_coluna_invariante_2011_0545.txt:14` |
| **2. Avais vigentes** | **SEGUE** | A exceção nominal do 0545 acomoda a referência à tupla dentro da regra de pureza do 1710. A opção B fica na fila E4D, preservando o escopo de adaptação do 1945. `e4c_f5_constroi_a_1911_1710.txt:4` `resposta_f5_duble_1911_1945.txt:2–7` `f5_ato_x_coluna_invariante_2011_0545.txt:14` |
| **3. Condição literal** | **SEGUE** | O tripwire é implementável no selo atual. Hoje ele isenta o atributo por forma AST dentro das funções examinadas. A trava pela linha exata ainda falta. `app/chamados/tests/test_e4c_f5_selo_chamada.py:161–179,265–279` |
| **4. Exportada, dinheiro ou `!`** | **SEGUE** | A isenção proposta atua na verificação estática de uma referência a uma tupla de vias. `app/chamados/models.py:2603,2724` `app/chamados/juizes.py:1399–1403` `app/chamados/tests/test_e4c_f5_selo_chamada.py:265–279` |

**P3g — SEGUE.** O selo é `SeloPurezaTest.test_os_dois_juizes_leem_o_objeto_recebido`, por `_impurezas`. Restrinja a isenção a **uma ocorrência** no `if` de `projecao_temporal_da_pergunta` e confira a linha-fonte literal pelo `lineno` do nó. Uma mudança na linha retira a isenção. Outra referência ao modelo continua passando pela regra normal do selo, inclusive dentro do mesmo arquivo. `app/chamados/juizes.py:1383,1401` `app/chamados/tests/test_e4c_f5_selo_chamada.py:161–179,265–279`

## 20/11 05:31 · sessao 703e2804c33e · E4C-F5-CONSTROI · censo · origem=Code

Encontrei **um caminho para A e dois para B**. Os três podem chegar ao `COMMIT` com todas as escritas previstas bem-sucedidas. O primeiro grava uma reabertura com horário anterior ao da validação que ela limpa. Os outros dois usam uma instância lida antes da trava. `app/chamados/services/disputa_emissao.py:1480–1534,1629–1658`, `app/chamados/services/materializacao.py:1033–1069`

Censo estático da árvore no diretório `76ce6695c4142ca0577285407fe0068f618a3c89`, em leitura apenas. A cópia não contém metadados Git para confirmar o hash de forma independente. A ocorrência das corridas em produção está [nao verificado].

### Como a composta decide A e B

`composta(p).validada_em` vem de `leitura(p)`. A leitura ordena as decisões reais por `ato_em` e `pk`. Se a coluna está preenchida e falta uma linha `VALIDACAO` **do mesmo instante**, ela acrescenta uma validação sintética naquele instante, depois de todas as linhas reais empatadas. Assim, **coluna preenchida sem linha correspondente e sem ato de desfazimento estritamente posterior torna B impossível**. A falta da linha, sozinha, conserva a validação. Se a linha correspondente existe, vale a ordem das linhas reais, inclusive o desempate por `pk`. `app/chamados/models.py:4276–4295,4333–4362`

Os atos que zeram a validação **na leitura** são `DEVOLUCAO`, `RECUSA`, `REABERTURA` inclusive a marcada `da_ressuscitacao`, e `RESPOSTA` com `resposta_mantem_validacao=False`. A redução só altera o valor retornado. Entre os escritores localizados, a resposta humana pode gravar esse desfazimento sem incluir a coluna no `UPDATE`. `SILENCIO`, `RETRATACAO`, `ABSORCAO` e `RESPOSTA` com a marca `True` preservam a validação na redução. `app/chamados/models.py:4205–4220,4302–4317,4342–4351`, `app/chamados/services/disputa_emissao.py:1483–1496,1527–1534`

### Censo por sítio

Nos pares abaixo, **atomic comum** significa que as duas escritas estão na mesma transação e têm um limite de rollback comum. `save()` e `registrar()` têm seus próprios `atomic` internos. Uma exceção que sai do bloco comum desfaz o que já foi escrito, mesmo que um chamador a capture depois. Outra transação não vê a primeira escrita isolada antes do `COMMIT` no PostgreSQL. Os vereditos consideram uma instância recarregada após o commit, relógio crescente e o passivo da própria árvore. A configuração efetiva de isolamento do servidor está [nao verificado]. `app/chamados/models.py:3691–3707,4333–4339,4365–4390`, `app/config/settings/prod.py:9–18`, `app/config/settings/saas.py:83–92`

| Sítio | Coluna e ato | Transação, falha, captura e visibilidade | Veredito |
|---|---|---|---|
| `_carimbar_validacao` | `save(update_fields=...)` preenche a coluna → registra `VALIDACAO` no mesmo instante, com eventual foto do passivo. | Atomic comum, rollback do par, sem captura entre as escritas. Vale para a validação normal, o aceite de folga e a irmã do par. Tanto o rollback da folga quanto a captura de `_ParDesfeito` operam sobre blocos que já cobrem o par. `app/chamados/services/validacao.py:161–175,512–528,546–577,639–650,731–745` | **NAO-PRODUZ** |
| `devolver_validacao_recusada` | Recarrega a coluna. Se preenchida, zera por `save` → registra `DEVOLUCAO`, com eventual foto do passivo. | Atomic comum, rollback do par, sem captura entre as escritas. A reabertura da disputa e a auditoria vêm depois do par. No fluxo de validação, a trava do colaborador já está em vigor. `app/chamados/services/validacao.py:262–305,731–745,771–785` | **NAO-PRODUZ** |
| `devolver_paridade_bug_par_103` | Zera por `save` → registra `DEVOLUCAO`, com eventual foto do passivo. O command delega à porta. | Atomic comum com releitura sob trava, rollback do par, sem captura entre as escritas. A falta da trilha de auditoria também levanta dentro do bloco. `app/chamados/services/validacao.py:328–371`, `app/chamados/management/commands/bug_par_103.py:108–116` | **NAO-PRODUZ** |
| `responder_pergunta_disputa_core` | `save` zera a coluna **se a instância que chegou já a trazia preenchida** → sempre registra `RESPOSTA(False)` no caminho de sucesso. | Atomic comum, rollback do par quando há as duas escritas, sem captura entre elas. A decisão de incluir `validada_em` em `update_fields` vem **antes** da trava. Depois da trava, renova o horário da resposta e conserva a lista de campos. As capturas de auditoria e follow-up vêm depois. `app/chamados/services/disputa_emissao.py:1480–1534,1549–1580` | **PRODUZ-B** |
| `reabrir_para_correcao`, chamado pela supervisão | Registra `REABERTURA` com foto → zera por `descarimbar_em_lote(...).update(**outros)`. As duas cascas de supervisão chegam à porta sem adquirir a trava do colaborador. | Atomic comum, rollback do par, sem captura entre as escritas. Captura `ato_em` **antes** da trava e lê a foto **depois** dela. As cascas passam um queryset de PKs já escolhidos, então uma validação concorrente pode entrar na foto e ser posterior à reabertura na ordem da leitura. `app/chamados/services/disputa_emissao.py:1617–1658`, `app/chamados/models.py:3508–3511`, `app/chamados/juizes.py:1762–1767`, `app/chamados/views.py:1366–1367,1415–1432`, `app/chamados/services/acoes_disputa.py:92–106` | **PRODUZ-A** |
| `reabrir_para_correcao`, chamado por recusa, pedido do colaborador ou rejeição de ausência | O mesmo `UPDATE` zera a coluna, precedido por `RECUSA` na recusa explícita e por `REABERTURA` nos outros dois. | Atomic comum, rollback do par, sem captura entre as escritas. Esses chamadores já têm a trava do colaborador quando a porta captura `ato_em`. Na rejeição de ausência, a captura do consumidor está fora da porta, em `on_commit`. `app/chamados/services/validacao.py:920–929`, `app/chamados/models.py:2414–2426`, `app/chamados/services/materializacao.py:1178–1194`, `app/ponto/signals.py:250–272` | **NAO-PRODUZ** |
| `curar_perguntas_em_limbo --apply`, ramo de limpeza | Delega ao mesmo par `REABERTURA` → `UPDATE`, com PKs fixos e `respondida_em IS NULL`. | Atomic comum da porta, sem captura entre as escritas. Usa a porta com horário anterior à trava. A existência, no conjunto fixo do command, de um alvo que continue elegível depois de uma validação concorrente está [nao verificado]. `app/chamados/management/commands/curar_perguntas_em_limbo.py:40–70`, `app/chamados/services/disputa_emissao.py:1629–1658` | **[nao verificado]** para A nesse command |
| `ChamadoColaborador.ressuscitar_perguntas` | Registra o passivo que falta → registra `REABERTURA` marcada → zera a coluna por `descarimbar_em_lote`, restrito aos IDs relidos. | Atomic comum, rollback do conjunto. Relê os alvos e marca a reabertura depois da trava. A captura da auditoria vem depois do bloco. A atribuição `_q.validada_em = None` da sondagem é só numa cópia em memória. `app/chamados/models.py:1569–1595,1646–1674,1701–1749` | **NAO-PRODUZ** |
| `PerguntaDisputa.carimbar`, chamado por `_materializar_ausencia_de_resposta` | Com texto e coluna nula na instância, `resposta='validar'` pode preencher a coluna → registra **`ABSORCAO`**, sem `VALIDACAO` real. | Atomic comum por irmã, rollback do carimbo e da absorção, sem captura entre ambos. O instante e as instâncias vêm antes da trava por irmã. Uma `RECUSA` posterior a esse instante pode vencer a validação sintética. Se o C6 ainda assim der a pergunta por morta, o `save` grava a coluna. `app/chamados/services/materializacao.py:1033–1069`, `app/chamados/models.py:3344–3376`, `app/chamados/juizes.py:1406–1440,1458–1474` | **PRODUZ-B** |
| `PerguntaDisputa.carimbar`, chamado por `_materializar_uma` | Pode preencher a coluna pelo mesmo `save`, sem inserir `VALIDACAO`. Usa instante colhido dentro do ato de materialização. | `save` e materialização ficam no atomic do plantio. As capturas de reprovação e erro vêm após a saída do bloco. A validação que o chamador já gravou pode ficar, com sua coluna e seu ato. Quando o carimbo acrescenta a coluna, a leitura tem o passivo sintético. `app/chamados/services/materializacao.py:1928–1998,2117–2133`, `app/chamados/services/validacao.py:731–785`, `app/chamados/models.py:3344–3376,4290–4295` | **NAO-PRODUZ** |
| `PerguntaDisputa.carimbar`, chamado pelo S128/S143 | Pode preencher a coluna pelo mesmo `save`, sem inserir `VALIDACAO`. Usa `now()` sob a trava do colaborador. | Atomic do resolvedor ou do chamador. O S143 captura fora do seu savepoint. O novo carimbo é lido como passivo. O outro ramo que passa `resposta='responder'` só pode acrescentar `respondida_em`. `app/chamados/services/disputa_emissao.py:1906–1925,2025–2033,2098–2106`, `app/chamados/signals.py:107–119`, `app/chamados/models.py:3350–3357,4290–4295` | **NAO-PRODUZ** |
| `PerguntaDisputa._nascer` | `save(force_insert=True)` grava a coluna, nula por padrão. Se `**campos` trouxer validação, registra a linha correspondente. Os emissores localizados não passam esse campo. | Atomic de `_sob_contrato`, `_nascer` usa `savepoint=False` e `registrar` aninha o seu. Falha desfaz o nascimento. A captura do log vem depois das duas escritas. `app/chamados/models.py:2659,2999–3032,3056–3060,3229–3246`, `app/chamados/services/disputa_emissao.py:448–461,848–860,1383–1392`, `app/chamados/services/acoes_disputa.py:267–272`, `app/chamados/services/declaracao_texto.py:375–379,665–669` | **NAO-PRODUZ** |
| `responder_automaticamente` e `carimbar_resposta_pela_trilha` | Gravam campos de resposta e `RESPOSTA(True)`. A primeira também pode materializar uma `VALIDACAO` sintética já presente na projeção. No upload de foto, a validação nova é uma chamada separada a `validar_pergunta`. | Cada par de resposta tem atomic comum, rollback das duas escritas, sem captura intermediária. A resposta automática pode ficar visível antes da validação delegada. `app/chamados/services/disputa_emissao.py:1662–1714`, `app/api/foto_recebida.py:38–53`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:65–70` | **NAO-PRODUZ** |
| `backfill_fio_morto_e4b --aplicar` e `devolver_instante_sem_texto_l130` | O backfill materializa o passivo de validação com a coluna intacta. A L-130 exige coluna nula e registra, quando falta, só `RESPOSTA(True)`. | Backfill sob trava e savepoint por dia, com rollback explícito se o dia falha. A L-130 usa atomic comum, sem captura intermediária, e levanta se a escrita do lote falhar. `app/chamados/management/commands/backfill_fio_morto_e4b.py:147–150,289–325,339–372`, `app/chamados/services/validacao.py:382–399,429–446`, `app/chamados/models.py:4285–4295` | **NAO-PRODUZ** |
| Demais inserções auditadas | `SILENCIO` em `DisputaSupervisao.fechar`, `fechar_o_fio_do_dia` e `backfill_decisoes_pergunta.aplicar`. `RETRATACAO` em `remover_motivo`. Nenhum altera a coluna ou o resultado da validação na redução. `backfill_decisoes_pergunta` só mede as validações que faltam. | Sem par coluna–validação para dividir. Cada inserção usa o atomic de `registrar`. `app/chamados/models.py:2103–2112,2240–2248,4342–4351,4374–4375`, `app/ponto/services/veredito_celula.py:189–197`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:44–68,350–358,607–624` | **NAO-PRODUZ** |

**Escritas indiretas e fronteiras.** O `bulk_create` de decisões localizado é o de `registrar`. O `save` do modelo admite inserção de instância nova, mas o censo não encontrou chamador de produção por `create`, `save` direto ou `bulk_create` por fora da porta. O `bulk_update` localizado de `PerguntaDisputa` grava só os três campos da lavra. As cópias e restaurações de valor em memória, incluindo `_desfazer_em_memoria` e os `refresh_from_db` após rollback, não escrevem a coluna de volta. `app/chamados/models.py:3373–3386,4146–4156,4252–4258,4365–4389`, `app/chamados/veredito_lavrado.py:237–250`, `app/chamados/services/validacao.py:262–263,575–577,646–656,791,1088`

**Admin e SQL.** O Django Admin está roteado, mas a busca não encontrou registro de nenhum dos dois modelos. Não localizei SQL cru de aplicação específico que escreva essa coluna ou insira essas decisões. Há um `pg_restore` operacional de schema inteiro, sem `--single-transaction` no comando. O conteúdo de um dump e o estado após uma restauração parcial estão [nao verificado]. `app/config/urls.py:23–28`, `scripts/backup_tenant.sh:9–12`, `scripts/restore_tenant.sh:3–9`

### Sequências mínimas que produzem A ou B

1. **PRODUZ-A — supervisão reabre enquanto outra transação valida.** T1 entra em `validar_pergunta` e adquire a trava do colaborador. T2, em `reabrir_disputa_supervisao` ou `acao_reabrir_disputa`, seleciona a pergunta ainda corrigível, entra em `reabrir_para_correcao`, marca `t1` e espera a trava. T1 grava coluna + `VALIDACAO(t2)`, com `t2 > t1`, e comita. T2 grava `REABERTURA(t1)`, zera a coluna e comita. Na composta, `VALIDACAO(t2)` vem depois da reabertura. `app/chamados/services/validacao.py:161–175,731–745`, `app/chamados/views.py:1415–1432`, `app/chamados/services/acoes_disputa.py:99–106`, `app/chamados/services/disputa_emissao.py:1629–1658`, `app/chamados/models.py:4335–4351`
2. **PRODUZ-B — resposta humana usa a coluna nula da instância antiga.** T1, pela resposta web ou API, carrega a pergunta viva com coluna nula. T2 executa `validar_pergunta` e comita coluna + `VALIDACAO`. T1 segue com a instância antiga, adquire a trava e grava `RESPOSTA(False)` com horário posterior. Seu `save` omite `validada_em`. A coluna de T2 fica preenchida e a resposta de T1 desfaz a validação na composta. `app/chamados/views.py:1237–1246,1274–1278`, `app/api/views.py:2634–2653`, `app/chamados/services/validacao.py:161–175,731–745`, `app/chamados/services/disputa_emissao.py:1483–1534`, `app/chamados/models.py:4342–4351`
3. **PRODUZ-B — absorção grava um carimbo anterior à recusa concorrente.** T1 responde `FALTA_DIA`, entra em `_materializar_ausencia_de_resposta`, marca `t1` e carrega uma irmã ainda não materializada, com texto e sem validação. Antes de T1 travar a irmã pelo colaborador, T2 executa `recusar_pergunta` para ela e comita `RECUSA(t2)`, com `t2 > t1`, limpando a resposta. T1 adquire a trava e usa a irmã antiga. Se o C6 a der por morta mesmo com a validação desfeita, por exemplo numa célula julgada que não acusa e sem marcos pendentes, T1 grava `validada_em=t1` e `ABSORCAO(t1)`. Na releitura, `RECUSA(t2)` vence o passivo sintético de `t1`. `app/chamados/services/disputa_emissao.py:1559–1563`, `app/chamados/services/materializacao.py:1033–1069,1100–1103`, `app/chamados/services/validacao.py:900–929`, `app/chamados/models.py:3344–3376,4333–4351`, `app/chamados/juizes.py:1458–1474`

## 20/11 05:31 · sessao b6cdac3ea3bf · CADEIA-20-PASSO-1 · leitura · origem=Code

# CADEIA-20-PASSO-1 · braço `cruza`

Três mudanças de insumo ficaram depois da última leitura de P1: **138359, 129943 e 128355 perderam o fio no carimbo da segunda passada**. A primeira `LEITURA` com `fio:0` em cada uma foi a inicial de P2. Em P2, o fecho do chamado que ainda estava vivo teve sua própria segunda passada. `cruza.sem_foto.txt`, seq 395, 407, 1089, 1101, 1458, 1470, 1608–1636, 1675–1700 e 1729–1754 · `app/ponto/services/cartorio.py:1152–1164,1209–1231,1683–1707,2035–2037`

Conferi o código da cópia em modo somente leitura. A identidade Git da cópia, o script original, as linhas `FOTO` e `LOTE` e a confirmação independente de `ERRO=0` estão **[nao verificado]**. A execução descrita abaixo é a do extrato `cruza`, com relógios de 00:30 em P1 e P2 e 06:27 em P3. `cruza.sem_foto.txt`, seq 1 · `legenda_do_script.txt:1–5`

## 1 e 4 · Cada escrita e delta, com a leitura posterior

`E` é `ESCRITA` e `D` é `DELTA`. Todos os números de sequência são do anexo 3. **(a)** é a segunda passada que consome `rejulgar`, usa a janela original e roda com `forcar=True`. **(b)** é a releitura da lista inteira do mesmo colaborador, depois da fábrica, se `emitidos` subiu naquele ato, com `forcar=False`. **(c)** indica ausência das duas vias após o evento naquele passe. `LEITURA` registra o cálculo da impressão antes da guarda, inclusive quando a célula é pulada. `cadeia_20_v3_complemento_2011_0215.txt:6–7` · `legenda_do_script.txt:1–3` · `app/ponto/management/commands/processar_cartorio.py:63–69` · `app/ponto/services/cartorio.py:1114–1123,1152–1164,1179–1184,1209–1231,1541–1558`

| Célula | Rodada, etapa e evento | Leitura posterior no mesmo passe |
|---|---|---|
| **128355** | **P1, corpo inicial:** fecho por `COBRANCA_SEM_FURO`, `39255 aberto → resolvido` (E **1416**, D **1418**). **P1, carimbo da segunda passada:** `fio:1 → fio:0` (D **1470**). **P2, corpo inicial:** fecho por não protesto, `39234 em_analise → resolvido`, `fio:0 → ausente` (E **1733**, D **1735**). No sinal desse fecho, carimbo da pergunta **58553** com `chamado_encerrado` (E **1737**). `app/ponto/services/cartorio.py:1691–1707,1726–1738,2035–2037` · `app/ponto/portas/celula.py:1181–1202` · `app/chamados/signals.py:91–117` · `app/chamados/services/disputa_emissao.py:2005–2030` | **(a) 1458** para E 1416/D 1418, fila **1430 → 1450**. **(c)** para D 1470, primeira leitura posterior **1729**, inicial de P2. **(a) 1754** para E 1733/D 1735/E 1737, fila **1746 → 1752**. |
| **129943** | **P1, corpo inicial:** fecho por `COBRANCA_SEM_FURO`, `36625 aberto → resolvido` (E **1052**, D **1054**). **P1, carimbo da segunda passada:** `fio:1 → fio:0` (D **1101**). **P2, corpo inicial:** fecho por não protesto, `36531 em_analise → resolvido`, `fio:0 → ausente` (E **1679**, D **1681**). No sinal desse fecho, carimbo da pergunta **53186** com `chamado_encerrado` (E **1683**). `app/ponto/services/cartorio.py:1691–1707,1726–1738,2035–2037` · `app/ponto/portas/celula.py:1181–1202` · `app/chamados/signals.py:91–117` · `app/chamados/services/disputa_emissao.py:2005–2030` | **(a) 1089** para E 1052/D 1054, fila **1066 → 1087**. **(c)** para D 1101, primeira leitura posterior **1675**, inicial de P2. **(a) 1700** para E 1679/D 1681/E 1683, fila **1692 → 1698**. |
| **138359** | **P1, corpo inicial:** fecho por `COBRANCA_SEM_FURO`, `37433 aberto → resolvido` (E **352**, D **354**). **P1, carimbo da segunda passada:** `fio:1 → fio:0` (D **407**). **P2, corpo inicial:** fecho por não protesto, `37355 em_analise → resolvido`, `fio:0 → ausente` (E **1612**, D **1614**). `app/ponto/services/cartorio.py:1691–1707,1726–1738,2035–2037` · `app/ponto/portas/celula.py:1181–1202` | **(a) 395** para E 352/D 354, fila **366 → 393**. **(c)** para D 407, primeira leitura posterior **1608**, inicial de P2. **(a) 1636** para E 1612/D 1614, fila **1628 → 1634**. |
| **129476** | **P1, repasse de órfãos dentro do emissor do corpo inicial:** inserção da pergunta **69484** para **36124** (E **607**). A inicial **564**, a releitura **1031** e as iniciais de P2 **1662** e P3 **1797** dão o mesmo hash e `fio:1`. O `RESUMO` **1876** registra `nenhuma nas fotos` nas três rodadas. `app/ponto/management/commands/detectar_ausencias.py:788–807` · `app/chamados/services/disputa_emissao.py:851–861,887–898,1180–1192` · `app/ponto/services/cartorio.py:1527–1558` | **(b) 1031** ocorreu depois de E 607, no ato 869, com `emitidos 143 → 144` (**686**). A leitura preservou a projeção e passou pela guarda com hash igual. Efeito individual da pergunta sobre o C6 **[nao verificado]**. |
| **129490** | **P1, repasse de órfãos dentro do emissor do corpo inicial:** inserção de **69483** para **39014** (E **601**) e de **69485** para **39044** (E **663**, D **666**, `fio:1 → fio:2`). Ainda no repasse, `analisar()` pôs **39044** em `em_analise` (E **667**, D **670**). `app/ponto/management/commands/detectar_ausencias.py:788–807` · `app/chamados/services/disputa_emissao.py:1180–1192,1258–1266` · `app/chamados/models.py:859–866` | **(b) 1032** para todos, no ato 869, `emitidos 143 → 144` (**686**). Leu `fio:2` e o novo estado de 39044. O hash de D 666 já incluía `fio:2`, e o de D 670 acrescentou a transição de estado. **1032** coincide com D 670. O efeito isolado de E 601 **[nao verificado]**. |
| **138541** | **P1, fábrica após o corpo inicial:** inserção de **69171** para **36617** (E **162**), `fio:0 → fio:1` (D **164**). `app/ponto/services/cartorio.py:1179–1184` · `app/chamados/services/fabrica_da_celula.py:160–174,178–182,217–220` · `app/chamados/models.py:3205–3209` | **(b) 200** para ambos, com o hash de D 164. O ato 371 já tinha `emitidos 66 → 67` antes da fábrica (**73**, **162**). |
| **142995** | **P1, repasse de órfãos dentro do emissor do corpo inicial:** inserção de **69647** para **38675** (E **1238**, D **1240**, `fio:1 → fio:2`). Ainda no repasse, `analisar()` pôs **38675** em `em_analise` (E **1241**, D **1243**). `app/ponto/management/commands/detectar_ausencias.py:788–807` · `app/chamados/services/disputa_emissao.py:1258–1266` · `app/chamados/models.py:859–866` | **(b) 1303** para todos, com `emitidos 192 → 193` (**1250**). Leu `fio:2` e o novo estado de 38675. O hash coincide com D 1243, que inclui a alteração de estado posterior a D 1240. |
| **143945** | **P1, repasse de órfãos dentro do emissor do corpo inicial:** reuso de **57630**, com destino **38873 → 38907** (E **1323**), `analisar()` de **38907 aberto → em_analise** (E **1325**, D **1327**) e inserção de **69697** para **38907** em `perguntar_entradas_pendentes` (E **1329**). `fio:1` na inicial **1315** e na releitura **1362**. `app/ponto/management/commands/detectar_ausencias.py:788–807` · `app/chamados/services/disputa_emissao.py:1091–1099,1284` · `app/chamados/models.py:859–866,3189–3209` | **(b) 1362** para todos, com `emitidos 207 → 208` (**1335**). Leu o novo estado e o hash de D 1327. O resultado individual de C6 para E 1323 e E 1329 **[nao verificado]**. |
| **129134** | **Entre P2 e P3:** `enc:0 → ausente` (D **1777**, intervalo **1724–1776**), etapa e tipo **[nao verificado]**. **P3, emissor do corpo inicial:** nascimento de **43849** em `aberto`, lista vazia → `43849:aberto`, fio ausente → `fio:0` (E **1808**, D **1810**). No repasse, quatro perguntas para 43849, **69947–69950** (E **1811–1814**), e `analisar()` de **43849 aberto → em_analise** (E **1816**, D **1818**). No carimbo do corpo inicial, `fio:0 → fio:1` (D **1830**). `app/ponto/services/cartorio.py:1540–1545,1952–1955,2012–2037` · `app/ponto/management/commands/detectar_ausencias.py:694–714,788–807` · `app/chamados/services/disputa_emissao.py:1180–1192` · `app/ponto/portas/celula.py:1181–1202` | D **1777** já aparece na **inicial 1805**, com o mesmo hash. A **(b) 1837** também lê sem `enc` e é posterior a todas as E/D internas de P3, com `emitidos 10 → 11` (**1825**). Ela lê o chamado já em análise e o hash final de D 1830. `aberto` e `fio:0` foram estados intermediários fotografados, sem `LEITURA` registrada nesses estados. Qual das quatro perguntas passou no C6 **[nao verificado]**. |

Para o **C3**, há duas leituras de “escrita que não couber”, com efeitos distintos no enquadramento:

- **Se “escrita” inclui a lavra/carimbo que altera um insumo derivado**, a caixa **(c)** contém os carimbos de **138359, 129943 e 128355** em P1, com `fio:1 → fio:0` nos seq **407, 1101 e 1470**. O carimbo da inicial de **129134** em P3, `fio:0 → fio:1` no seq **1830**, cai em **(b)** pela releitura **1837**. `cadeia_20_v3_complemento_2011_0215.txt:6–7` · `app/ponto/services/cartorio.py:1209–1222,2012–2037` · `app/ponto/portas/celula.py:1181–1202`
- **Se “escrita” se restringe às linhas `ESCRITA` do SQL monitorado**, todas as escritas exibidas têm leitura posterior **(a)** ou **(b)** conforme a tabela. Os três carimbos continuam sendo deltas sem leitura posterior em P1. Além disso, se “couber” exigir **gatilho próprio da etapa** em vez de cobertura no ato observado, a fábrica de **138541** (E **162**, D **164**) e o carimbo de **129134** (D **1830**) ficam em **(c)** por esse critério: as respectivas releituras **200** e **1837** foram abertas pela emissão já contada nos seq **73** e **1825**. Os carimbos das perguntas **53186** e **58553** (E **1683** e **1737**) também dependem da fila do fecho que disparou o sinal. `cadeia_20_v3_complemento_2011_0215.txt:6–7` · `legenda_do_script.txt:2–3` · `app/ponto/services/cartorio.py:1179–1184,1209–1222,1695–1704,2035–2037` · `app/chamados/signals.py:91–117`

## 2 · Mudanças sem leitura posterior no passe

| Célula | Braço e rodada | Etapa e números | O que ficou para a leitura seguinte |
|---|---|---|---|
| **138359** | `cruza` P1 | Carimbo da segunda passada, última leitura **395**, D **407** `[405,406]`, resumo **1875**. `app/ponto/services/cartorio.py:1156–1164,2035–2037` · `app/ponto/portas/celula.py:1181–1202` | `fio:1 → fio:0`, com **37355** ainda `em_analise`. P2 lê em **1608**. |
| **129943** | `cruza` P1 | Carimbo da segunda passada, última leitura **1089**, D **1101** `[1099,1100]`, resumo **1874**. `app/ponto/services/cartorio.py:1156–1164,2035–2037` · `app/ponto/portas/celula.py:1181–1202` | `fio:1 → fio:0`, com **36531** ainda `em_analise`. P2 lê em **1675**. |
| **128355** | `cruza` P1 | Carimbo da segunda passada, última leitura **1458**, D **1470** `[1468,1469]`, resumo **1873**. `app/ponto/services/cartorio.py:1156–1164,2035–2037` · `app/ponto/portas/celula.py:1181–1202` | `fio:1 → fio:0`, com **39234** ainda `em_analise`. P2 lê em **1729**. |

Nas nove linhas `RESUMO`, toda ocorrência de `releu_depois: nao` vem com `primeira_mudanca: nenhuma nas fotos`. Nos três casos acima o `sim` cobre **o primeiro delta**, de estado do chamado. O `0` em `escritas_depois_da_ultima_leitura` conta linhas monitoradas de chamado ou pergunta. Os três deltas posteriores são da etapa de carimbo da **célula**. Em **129476**, o `nao` de P1 convive com E **607** e a releitura **1031**, que ocorreu depois da escrita com o mesmo hash da inicial **564**. `cruza.sem_foto.txt`, seq 395, 407, 564, 607, 1031, 1089, 1101, 1458, 1470 e 1873–1881 · `legenda_do_script.txt:2–3` · `app/ponto/portas/celula.py:1181–1202`

No `RESUMO` **1881**, `[nao verificado]` é a **etapa e o tipo** do D **1777**. A sequência bruta põe a mudança entre as fotos **1724 e 1776**, antes da inicial **1805** e da releitura **1837**. O `false` ao lado de **1837** coincide com um hash diferente do D 1777, pois a releitura também traz **43849:em_analise** e `fio:1`. Qualquer escrita associada ao intervalo **[nao verificado]**. `cruza.sem_foto.txt`, seq 1670, 1777, 1805, 1830, 1837 e 1881 · `app/ponto/services/cartorio.py:523–532,1540–1545`

## 3 · Insumo que voltou a mudar em outra rodada

| Célula | P1 | P2 |
|---|---|---|
| **138359** | Corpo inicial fecha **37433** (E **352**, D **354**). Segunda passada ainda lê **37355** com fio (**395**), e seu carimbo derruba `fio:1 → fio:0` (**407**). | Inicial lê **37355** vivo sem fio (**1608–1609**). Fecho por não protesto resolve **37355** e tira `fio:0` da impressão (**1612–1614**). Segunda passada **1636**. |
| **129943** | Corpo inicial fecha **36625** (E **1052**, D **1054**). Segunda passada ainda lê **36531** com fio (**1089**), e seu carimbo derruba `fio:1 → fio:0` (**1101**). | Inicial lê **36531** vivo sem fio (**1675–1676**). Fecho por não protesto resolve **36531** e tira `fio:0` da impressão (**1679–1681**). O sinal carimba **53186** (**1683**). Segunda passada **1700**. |
| **128355** | Corpo inicial fecha **39255** (E **1416**, D **1418**). Segunda passada ainda lê **39234** com fio (**1458**), e seu carimbo derruba `fio:1 → fio:0` (**1470**). | Inicial lê **39234** vivo sem fio (**1729–1730**). Fecho por não protesto resolve **39234** e tira `fio:0` da impressão (**1733–1735**). O sinal carimba **58553** (**1737**). Segunda passada **1754**. |

Nas três células, a entrada que só chegou ao corpo em P2 foi **o chamado remanescente vivo, sem protesto e sem fio**. Em P1, o corpo inicial dispõe do adiamento por fio para os não protestantes. O E2 e o fecho adiado rodam antes da segunda passada. A segunda passada usa o padrão `adiar_pela_lampada=False`, mas sua leitura ainda vê o fio vivo. Seu juízo já retorna `codigos=[]` e zero protestos, portanto a lavra pode passar de `cobranca_indevida` para `concorde`. O C6 lê o veredito da célula, e os deltas de fio aparecem no carimbo, depois dessa leitura. A guarda de fecho consulta perguntas vivas no pós-estado e pode barrar o fecho se elas continuarem ancoradas sem reaponte válido. O ramo exato que devolveu `false` para cada chamado em P1 é **[nao verificado]**. Os `RETORNO` trazem células, sem PK do chamado. Em particular, **1457** vem antes da `LEITURA` **1458** no extrato. `cruza.sem_foto.txt`, seq 349, 390–407, 1049, 1084–1101, 1413, 1441–1470, 1608–1609, 1675–1676 e 1729–1730 · `app/ponto/services/cartorio.py:1123–1157,1264–1265,1628–1653,1683–1707,2012–2037` · `app/chamados/models.py:361–405,422–443,684–705` · `app/chamados/juizes.py:1450–1469` · `app/escala/models.py:1624–1628,1767–1768`

Em P2, a leitura da impressão já diverge do carimbo anterior pelo `fio:0`. O remanescente sem protesto fica fora do adiamento por fio, fecha no corpo inicial e enfileira a segunda passada. Com todos os chamados resolvidos, o código omite o campo `fio`. `cruza.sem_foto.txt`, seq 1608–1636, 1675–1700 e 1729–1754 · `app/ponto/services/cartorio.py:543–547,1527–1558,1683–1707`

**129134 mudou de insumo em P3.** P1 e P2 leram o mesmo hash, lista vazia e `enc:0` às 00:30. O `None → concorde` visto no objeto entre as iniciais é a primeira lavra, com o mesmo insumo. Em P3, às 06:27, o relógio já tinha cruzado o fim previsto das 01:00. O juiz exige `agora >= fim` quando há fim previsto e nenhuma aberta viva. O juízo então recebeu `turno_encerrado=true` e devolveu `FURO_SEM_COBRANCA`, que abriu o caminho da emissão de **43849** neste ensaio. `cruza.sem_foto.txt`, seq 1, 210–211, 1670 e 1777–1838 · `app/ponto/turnos.py:2124–2134` · `app/ponto/services/cartorio.py:1293–1297,1540–1545,1614–1625,1909–1938,1952–1955` · `app/ponto/supra_juiz.py:326–329,373–383`

## 5 · Limites da medida

- **[nao medido]** Estados transitórios entre fotos, SQL fora dos campos e das tabelas monitoradas, resultado por pergunta quando a leitura só registra a presença de fio por chamado, e a totalização de `LOTE` nesta entrega. `legenda_do_script.txt:1–3,5` · `app/ponto/services/cartorio.py:1527–1545` · `app/ponto/management/commands/processar_cartorio.py:70–74`
- **[nao medido]** Efeitos após o rollback descrito na legenda, incluindo callbacks `on_commit`, consumidores e escritores externos além do trecho capturado. A equivalência de recortar a frota também é **[nao medido]**. A cascata pode buscar clones por PK sem filtro de colaborador. A efetivação do rollback deste ensaio está **[nao verificado]**. `legenda_do_script.txt:4–5` · `app/ponto/management/commands/processar_cartorio.py:51–69` · `app/chamados/signals.py:134–165,229–240` · `app/chamados/services/materializacao.py:1271–1279`
- **[nao medido]** Ausência geral de escrita ou cobertura em rodadas com `nenhuma nas fotos`, em células sem evento e em braços em que a célula não se moveu. O extrato só permite afirmar o que aparece nas nove células de `cruza`. O caso **129476** mostra concretamente que a ausência de delta fotografado pode coexistir com uma escrita e uma releitura. Os outros dois braços estão **[nao medido]** nesta resposta. `cruza.sem_foto.txt`, seq 1, 607, 1031 e 1873–1881 · `legenda_do_script.txt:1–5`

## 20/11 05:30 · sessao a84dbcaaa77a · CADEIA-20-PASSO-1 · leitura · origem=Code

No braço `em_curso`, cinco células tiveram mudança de insumo **depois da última leitura de P1**. Em `128355`, `129943` e `138359`, o fio caiu de `1` para `0` no carimbo da segunda passada. Em `129476` e `138541`, a fábrica elevou o fio. A primeira leitura registrada com esses novos valores foi a inicial de P2. `129490` também teve duas escritas depois da última leitura de P1, com o mesmo hash de insumos nas três leituras registradas. Anexo 3, `seq 77, 906, 1274, 635–637, 693–694, 1136–1138, 1412, 1431, 1475, 1485, 1500, 1559, 1627` · `app/ponto/services/cartorio.py:1152–1184,1209–1222,1541–1547,2035–2037`.

O HEAD da worktree é `a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6`. O anexo fixa o relógio às `00:30` nas três rodadas deste braço. [HEAD:1](/home/ronald/saas-hasner/.git/worktrees/wt-codex-cadeia-20-passo-1-112113/HEAD:1) · `em_curso.sem_foto.txt`, `seq 1`.

## 1, 2 e 4. Escritas, deltas e leitura posterior por célula

Todos os `seq` da tabela são do anexo 3. **(a)** indica leitura posterior pela fila `rejulgar` do mesmo ato, consumida com `forcar=True`. **(b)** indica releitura posterior por emissão no mesmo ato, depois da fábrica, com `forcar=False` e condicionada a `emitidos > emitidos_entrada`. **(c)** indica ausência das duas leituras posteriores no ato observado. `LEITURA` registra o cálculo da impressão mesmo quando a guarda pula o julgamento. `cadeia_20_v3_complemento_2011_0215.txt:6–7` · `legenda_do_script.txt:1–3` · `app/ponto/services/cartorio.py:1114,1152–1164,1179–1184,1209–1231,1541–1558`.

| Célula | Rodada, etapa e mudança | Leitura posterior e caixa |
|---|---|---|
| **128355** | **P1, corpo inicial**, cobrança sem furo. `39255 aberto → resolvido`, `ESCRITA 1220`, `DELTA 1222`. **P1, carimbo da segunda passada**, `fio:1 → fio:0`, `DELTA 1274`, intervalo `1272–1273`. **P2, corpo inicial**, fecho por não protesto de `39234 em_analise → resolvido`, `fio:0 → ausente`, `ESCRITA 1563`, `DELTA 1565`. No mesmo fecho, o sinal materializou a pergunta `58553` pela via `chamado_encerrado`, `ESCRITA 1567`. | **(a)** `1220/1222 → 1262`, fila `1234/1254`. **(c)** `1274`, última leitura `1262`, novo hash lido em P2 `1559`. **(a)** `1563/1565/1567 → 1584`, fila `1576/1582`. A leitura `1584` confirma o insumo do fecho. Efeito adicional da materialização no hash [nao verificado]. `app/ponto/services/cartorio.py:1152–1157,1691–1704,1726–1738,2035–2037` · `app/ponto/portas/celula.py:1181–1202` · `app/chamados/signals.py:91–117` · `app/chamados/services/disputa_emissao.py:2005–2030`. |
| **129943** | **P1, corpo inicial**, cobrança sem furo. `36625 aberto → resolvido`, `ESCRITA 857`, `DELTA 859`. **P1, carimbo da segunda passada**, `fio:1 → fio:0`, `DELTA 906`, intervalo `904–905`. **P2, corpo inicial**, fecho por não protesto de `36531 em_analise → resolvido`, `fio:0 → ausente`, `ESCRITA 1504`, `DELTA 1506`. No mesmo fecho, o sinal materializou a pergunta `53186` pela via `chamado_encerrado`, `ESCRITA 1508`. | **(a)** `857/859 → 894`, fila `871/892`. **(c)** `906`, última leitura `894`, novo hash lido em P2 `1500`. **(a)** `1504/1506/1508 → 1525`, fila `1517/1523`. A leitura `1525` confirma o insumo do fecho. Efeito adicional da materialização no hash [nao verificado]. `app/ponto/services/cartorio.py:1152–1157,1691–1704,1726–1738,2035–2037` · `app/ponto/portas/celula.py:1181–1202` · `app/chamados/signals.py:91–117` · `app/chamados/services/disputa_emissao.py:2005–2030`. |
| **138359** | **P1, corpo inicial**, cobrança sem furo. `37433 aberto → resolvido`, `ESCRITA 22`, `DELTA 24`. **P1, carimbo da segunda passada**, `fio:1 → fio:0`, `DELTA 77`, intervalo `75–76`. **P2, corpo inicial**, fecho por não protesto de `37355 em_analise → resolvido`, `fio:0 → ausente`, `ESCRITA 1435`, `DELTA 1437`. | **(a)** `22/24 → 65`, fila `36/63`. **(c)** `77`, última leitura `65`, novo hash lido em P2 `1431`. **(a)** `1435/1437 → 1459`, fila `1451/1457`. O `RETORNO 1443` registra `reabrir=True` na cascata, sem PK do chamado reaberto. Uma escrita adicional atribuível à célula fica [nao verificado]. `app/ponto/services/cartorio.py:1152–1157,1691–1704,1726–1738,2035–2037` · `app/ponto/portas/celula.py:1181–1202` · `app/chamados/services/disputa_emissao.py:2043–2056`. |
| **129476** | **P1, fábrica**, inseriu a pergunta `60113` ligada ao chamado `36172`, `ESCRITA 635`, `conta:false`. `fio:1 → fio:2`, `DELTA 637`, intervalo `633–636`. | **(c)** `635/637`, última leitura `333`. P2 `1475` lê o hash do delta e mostra `36172` com fio. P3 `1626` mantém esse hash. O contador no instante da escrita era `11/11`, entrada/atual. `app/ponto/services/cartorio.py:1179–1184,1209–1222,1527–1545` · `app/chamados/services/fabrica_da_celula.py:160–174,206–219` · `app/chamados/models.py:3205–3209`. |
| **129490** | **P1, fábrica**, inseriu a pergunta `60114` com destino nulo, `ESCRITA 693`, e reapontou a mesma pergunta para `39014`, `ESCRITA 694`. Ambas têm `conta:false`. O extrato traz o vínculo antes/depois e `campos:{}` nas duas. | **(c)** `693/694`, última leitura `356`. P2 `1485` e P3 `1627` repetem o hash de P1, com `39014` já com fio desde `356`, `39044` sem fio e `fio:1` nas três. O RESUMO `1669` registra “nenhuma nas fotos”. Efeito transitório entre fotos [nao medido]. `app/ponto/services/cartorio.py:1179–1184,1209–1222,1527–1545` · `app/chamados/services/fabrica_da_celula.py:172–174,217–219` · `app/chamados/services/disputa_emissao.py:468–474` · `app/chamados/reconciliador.py:1004,1672` · `app/chamados/services/fio.py:973–977`. |
| **138541** | **P1, fábrica**, inseriu a pergunta `60149` ligada ao chamado `36617`, `ESCRITA 1136`, `conta:false`. `fio:0 → fio:1`, `DELTA 1138`, intervalo `1135–1137`. | **(c)** `1136/1138`, última leitura `1037`. P2 `1412` lê o hash do delta e mostra `36617` com fio. P3 `1609` mantém esse hash. O contador no instante da escrita era `22/22`, entrada/atual. `app/ponto/services/cartorio.py:1179–1184,1209–1222,1527–1545` · `app/chamados/services/fabrica_da_celula.py:160–174,206–219` · `app/chamados/models.py:3205–3209`. |
| **142995** | P1, P2 e P3 registram o mesmo hash, com `38675` e `38622` já em `em_analise` na primeira leitura e `fio:1`. `seq 204, 1539, 1639, 1671`. | Sem `ESCRITA` ou `DELTA` publicado para classificar. Etapa da transição histórica citada no C1 [nao verificado]. `app/ponto/services/cartorio.py:1527–1558`. |
| **143945** | P1, P2 e P3 registram o mesmo hash, com `38907` já em `em_analise` na primeira leitura e `fio:1`. `seq 269, 1544, 1644, 1672`. | Sem `ESCRITA` ou `DELTA` publicado para classificar. Etapa da transição histórica citada no C1 [nao verificado]. `app/ponto/services/cartorio.py:1527–1558`. |
| **129134** | P1, P2 e P3 leem lista vazia, `enc:0` e o mesmo hash. Às `00:30`, o fim previsto registrado era `01:00`, com motivo `relogio`. O `JUIZO` de P1 registra `turno_encerrado:false` e `codigos:[]`. `seq 721–722, 1426, 1614, 1673`. | Sem `ESCRITA` ou `DELTA` publicado para classificar. O código inclui `enc:0` quando o dia passado ainda está em curso. `app/ponto/services/cartorio.py:1293–1297,1540–1545,1614–1625` · `app/ponto/turnos.py:2124–2134`. |

Os RESUMOS com **primeira mudança registrada** e `releu_depois: nao` são `1668` e `1670`, ambos em P1. O `0` de `escritas_depois_da_ultima_leitura` convive com os `INSERT` de `635` e `1136`, ambos com `conta:false`. O mesmo vale para `693/694` em `129490`. Em `1665–1667`, o `sim` de P1 se refere ao primeiro fecho. Os deltas de carimbo `1274`, `906` e `77` vêm depois das últimas leituras indicadas nesses resumos. Nenhum dos nove RESUMOS fornecidos traz `[nao verificado]`. A legenda prevê esse valor quando uma leitura cai dentro do intervalo da mudança. Qualquer ocorrência fora do extrato fica [nao verificado]. Anexo 3, `seq 635–637, 693–694, 1136–1138, 1665–1673` · `legenda_do_script.txt:2–3` · `app/ponto/services/cartorio.py:1152–1157,1541–1547,2035–2037`.

**As duas leituras de “couber” no C3.** Pela leitura de **cobertura no ato observado**, valem as caixas da tabela e nenhum evento vai para **(b)**. Pela leitura de **encaixe na sequência do código**, os carimbos `77/906/1274` já são executados *dentro* da segunda passada. Contam em **(a)** se a expressão designar o corpo que os executa. Se designar uma leitura capaz de absorver o novo insumo, só poderiam entrar em **(b)** mediante emissão contada antes do teste, pois o cálculo da segunda passada já ocorreu. As escritas da fábrica `635/693/694/1136` também estão antes desse teste, de modo que cabem em **(b)** por posição e sob a mesma condição. Nenhuma dessas possibilidades condicionais produziu leitura posterior no braço observado. O C3 também diz “escrita” sem restringir a palavra a delta da impressão. Se o alcance for toda `ESCRITA`, `693/694` ficam em **(c)** no ato observado. Se o alcance for apenas mudança medida de insumo, `693/694` ficam fora desse juízo. Pela mesma leitura restrita, `1508/1567` têm leitura posterior da célula, com delta próprio [nao verificado]. `cadeia_20_v3_complemento_2011_0215.txt:6–7` · Anexo 3, `seq 65, 77, 635–637, 693–694, 894, 906, 1136–1138, 1262, 1274, 1506–1508, 1525, 1565–1567, 1584, 1665–1670` · `app/ponto/services/cartorio.py:1152–1184,1209–1231,1541–1547,2035–2037`.

## 3. Insumo que mudou em P1 e voltou a mudar em P2

| Célula | P1 | P2 |
|---|---|---|
| **128355** | Corpo inicial fechou `39255`, `1220/1222`. A segunda passou lendo `39234` vivo com fio, `1262/1263`. A tentativa de fecho nessa passada retornou `false`, `1267/1269`. Seu carimbo baixou o fio para `0`, `1274`. | O corpo inicial leu `39234` vivo, sem fio e sem protesto, `1559/1560`. Fechou-o, `1563/1565`, e o sinal carimbou `58553`, `1567`. `app/ponto/services/cartorio.py:1691–1704,1726–1738,2035–2037` · `app/chamados/signals.py:91–117`. |
| **129943** | Corpo inicial fechou `36625`, `857/859`. A segunda passou lendo `36531` vivo com fio, `894/895`. A tentativa de fecho retornou `false`, `899/901`. Seu carimbo baixou o fio para `0`, `906`. | O corpo inicial leu `36531` vivo, sem fio e sem protesto, `1500/1501`. Fechou-o, `1504/1506`, e o sinal carimbou `53186`, `1508`. `app/ponto/services/cartorio.py:1691–1704,1726–1738,2035–2037` · `app/chamados/signals.py:91–117`. |
| **138359** | Corpo inicial fechou `37433`, `22/24`. A segunda passou lendo `37355` vivo com fio, `65/66`. A tentativa de fecho retornou `false`, `70/72`. Seu carimbo baixou o fio para `0`, `77`. | O corpo inicial leu `37355` vivo, sem fio e sem protesto, `1431/1432`. Fechou-o, `1435/1437`. `app/ponto/services/cartorio.py:1691–1704,1726–1738,2035–2037`. |

Nas três, **o `fio:0` surgiu no fim de P1 e só entrou em `_com_fio` na leitura de P2**. O corpo carrega os chamados e calcula o fio antes de julgar e carimbar. No corpo inicial, um chamado sem protesto que ainda está em `_com_fio` põe o lote de não protestantes no fecho adiado. Em P1, o fecho adiado nas três células retornou `false`, assim como as tentativas mostradas na segunda passada. Em P2, a entrada com `_com_fio` vazio deixa o sobrevivente sem protesto seguir direto para `reconciliar()` em `1695`. A lavra pode mudar a vitalidade pelo C6, que lê o veredito da célula. A guarda específica que devolveu cada `false` em P1 fica [nao verificado], pois `reconciliar()` também consulta a premissa e a conversa do pai antes da guarda de pergunta viva. Anexo 3, `seq 60/62, 889/891, 1245/1247, 65–77, 894–906, 1262–1274, 1431–1437, 1500–1506, 1559–1565` · `app/ponto/services/cartorio.py:1123–1157,1462,1527–1545,1605–1617,1683–1704,2012–2037` · `app/chamados/juizes.py:1450–1469` · `app/chamados/models.py:422–443,654–668,686–705`.

P3 lê os hashes que já apareceram nas segundas passadas de P2 para essas três células. Em `129476` e `138541`, P2 e P3 apenas registram o novo insumo já produzido pela fábrica em P1, sem um segundo delta publicado. Anexo 3, `seq 637, 1138, 1412, 1459, 1475, 1525, 1584, 1609, 1619, 1626, 1634, 1659, 1665–1670` · `app/ponto/services/cartorio.py:1541–1558`.

## 5. Limites da medida

- **[nao medido]** Mudanças transitórias entre as bordas das fotos, escritas fora dos campos monitorados e efeitos fora do alcance dos ganchos. O extrato contém os `DELTA` e omite `FOTO` e `LOTE`. O script em `logs/sombra/` e a execução efetiva do rollback [nao verificado]. `legenda_do_script.txt:1–5` · `bin/sombra.sh:483–492`.
- **[nao medido]** Efeito de callbacks `on_commit`, consumidores ou escritores externos além do que aparece nas linhas entregues. A equivalência de rodar só as nove células também fica [nao verificado]. O comando carrega a frota por empresa e a cascata pode alcançar chamados relacionados por PK. `legenda_do_script.txt:4–5` · `app/ponto/management/commands/processar_cartorio.py:51–69` · `app/chamados/signals.py:122–146,149–167` · `app/chamados/services/materializacao.py:1271–1279`.
- **[nao medido]** Etapa escritora de uma mudança nas células sem evento publicado neste braço, `142995`, `143945` e `129134`. Para `129490`, a escrita SQL está medida e a mudança de insumo entre fotos fica [nao medido]. Para `129134`, a passagem de `enc:0` a turno encerrado fica [nao medido] neste relógio fixo. Anexo 3, `seq 1, 693–694, 721–722, 1426, 1614, 1669, 1671–1673` · `app/ponto/services/cartorio.py:1527–1547` · `app/ponto/turnos.py:2124–2134`.
- **[nao medido]** Os outros braços, a etapa escritora onde uma célula ficou sem movimento e qualquer extrapolação para produção. `ERRO=0` nos três braços é dado do enunciado. A conferência independente da execução e dos totais de lote fica [nao verificado]. Anexo 3, `seq 1, 1665–1673` · `legenda_do_script.txt:4–5` · `app/ponto/management/commands/processar_cartorio.py:69–74`.

## 20/11 05:30 · sessao 1060e319a965 · CADEIA-20-PASSO-1 · leitura · origem=Code

No braço **`encerrado`**, três mudanças de insumo ficaram sem leitura posterior em P1. Todas foram `fio:1 → fio:0` no carimbo da 2ª passada, nas células **128355, 129943 e 138359**. A leitura inicial de P2 já viu `fio:0` e, nessa rodada, o cartório fechou o último chamado vivo de cada uma. Anexo 3, `seq` 77, 1070, 1493, 1641–1647, 1693–1699 e 1752–1758. `app/ponto/services/cartorio.py:1527–1558,1683–1704,2035–2037`.

## 1. Escritas, deltas e leituras nas nove células

Todos os `seq` abaixo são do anexo 3. O passe é o ato de `julgar_colab` daquele colaborador na rodada. **A** é a 2ª passada, que consome `rejulgar` e roda com `forcar=True`. **B** é a releitura convocada quando `emitidos` sobe no ato, depois da fábrica, sobre a lista inteira e sujeita à guarda da impressão. **C** indica que nenhuma dessas duas leituras veio depois do evento no mesmo passe. Uma `LEITURA` prova que a projeção da célula foi lida, inclusive na guarda, conforme a legenda do anexo 4. `app/ponto/management/commands/processar_cartorio.py:45,63–69` · `app/ponto/services/cartorio.py:1114–1123,1152–1164,1179–1181,1209–1231,1541–1558`.

| Célula | Rodada, etapa e evento | Leitura posterior no mesmo passe |
|---|---|---|
| **128355** | **P1.** Fecho por `COBRANCA_SEM_FURO` no corpo inicial. `39255` passou de `aberto` a `resolvido` (ESCRITA **1439**, DELTA **1441**). Depois, o carimbo da 2ª passada fez `fio:1 → 0` (DELTA **1493**). `app/ponto/services/cartorio.py:1726–1738,2035–2037` · `app/ponto/portas/celula.py:1181–1202`. **P2.** Fecho por não protesto no corpo inicial. `39234` passou de `em_analise` a `resolvido`, e `fio:0` saiu da impressão (ESCRITA **1756**, DELTA **1758**). O sinal do fecho materializou a pergunta `58553` pela via `chamado_encerrado` (ESCRITA **1760**). `app/ponto/services/cartorio.py:1683–1704` · `app/chamados/signals.py:91–117` · `app/chamados/services/disputa_emissao.py:2009–2030`. | **A**, **1481** para 1439/1441, com fila **1453 → 1473**. **C** para 1493. **A**, **1777** para 1756/1758/1760, com fila **1769 → 1775**. `app/ponto/services/cartorio.py:1152–1157,1209–1222,1541–1547`. |
| **129943** | **P1.** Mesmo ramo de cobrança indevida. `36625` passou de `aberto` a `resolvido` (ESCRITA **1021**, DELTA **1023**). O carimbo da 2ª passada fez `fio:1 → 0` (DELTA **1070**). `app/ponto/services/cartorio.py:1726–1738,2035–2037` · `app/ponto/portas/celula.py:1181–1202`. **P2.** Fecho por não protesto no corpo inicial. `36531` passou de `em_analise` a `resolvido`, e `fio:0` saiu da impressão (ESCRITA **1697**, DELTA **1699**). O sinal materializou a pergunta `53186` pela via `chamado_encerrado` (ESCRITA **1701**). `app/ponto/services/cartorio.py:1683–1704` · `app/chamados/signals.py:91–117` · `app/chamados/services/disputa_emissao.py:2009–2030`. | **A**, **1058** para 1021/1023, com fila **1035 → 1056**. **C** para 1070. **A**, **1718** para 1697/1699/1701, com fila **1710 → 1716**. `app/ponto/services/cartorio.py:1152–1157,1209–1222,1541–1547`. |
| **138359** | **P1.** Mesmo ramo de cobrança indevida. `37433` passou de `aberto` a `resolvido` (ESCRITA **22**, DELTA **24**). O carimbo da 2ª passada fez `fio:1 → 0` (DELTA **77**). `app/ponto/services/cartorio.py:1726–1738,2035–2037` · `app/ponto/portas/celula.py:1181–1202`. **P2.** Fecho por não protesto no corpo inicial. `37355` passou de `em_analise` a `resolvido`, e `fio:0` saiu da impressão (ESCRITA **1645**, DELTA **1647**). `app/ponto/services/cartorio.py:1683–1704` · `app/chamados/models.py:654–668`. | **A**, **65** para 22/24, com fila **36 → 63**. **C** para 77. **A**, **1669** para 1645/1647, com fila **1661 → 1667**. `app/ponto/services/cartorio.py:1152–1157,1209–1222,1541–1547`. |
| **129476** | **P1.** No repasse do emissor no ato `869`, o complemento de marcos inseriu a pergunta `68298` com destino `36124` (ESCRITA **421**). O chamado já tinha fio em **378**. A leitura **845** mantém a mesma lista, `fio:1` e o mesmo hash. O extrato traz a inserção e nenhum DELTA da célula. RESUMO **1861**. `app/ponto/management/commands/detectar_ausencias.py:791–807` · `app/chamados/services/disputa_emissao.py:887–898,1180–1192`. | **B**, **845** depois de 421. O contador do ato compartilhado foi `111 → 112` (**500**, **845–846**). A leitura alcançou a guarda com impressões iguais. `app/ponto/services/cartorio.py:1114,1209–1222,1552–1558,2136–2141`. |
| **129490** | **P1.** Repasse do emissor no corpo inicial. O complemento de marcos inseriu a pergunta `68297` para `39014` (ESCRITA **415**). A criação da pergunta `68299` para `39044` fez `fio:1 → 2` (ESCRITA **477**, DELTA **480**). Depois, `analisar()` mudou `39044` de `aberto` a `em_analise` (ESCRITA **481**, DELTA **484**). `app/ponto/services/cartorio.py:1952–1955` · `app/chamados/services/disputa_emissao.py:1189–1192,1258–1266` · `app/chamados/models.py:859–866`. | **B**, **846** depois de todos esses eventos. O hash coincide com o DELTA **484**, e a leitura contém também o `fio:2` de **480**. O juízo veio em **847**. `app/ponto/services/cartorio.py:1209–1222,1541–1558,1614–1627`. |
| **138541** | **P1.** A fábrica, após o corpo inicial, inseriu `68711` para `36617` (ESCRITA **1347**) e o fio foi de `0 → 1` (DELTA **1349**). O contador já tinha subido de `223 → 224` no ato (**1201**, **1258**). `app/ponto/services/cartorio.py:1179–1184,2136–2141` · `app/chamados/services/fabrica_da_celula.py:160–174,206–219`. | **B**, **1385** para 1347/1349, com o hash do delta e `36617` vivo com fio. `app/ponto/services/cartorio.py:1209–1222,1527–1547`. |
| **142995** | **P1.** Repasse do emissor no corpo inicial. A inserção de `67931` para `38675` fez `fio:1 → 2` (ESCRITA **214**, DELTA **216**). Depois, `analisar()` mudou `38675` de `aberto` a `em_analise` (ESCRITA **217**, DELTA **219**). `app/ponto/services/cartorio.py:1952–1955` · `app/chamados/services/disputa_emissao.py:1258–1266` · `app/chamados/models.py:859–866`. | **B**, **279** depois dos quatro eventos, com `fio:2`, `38675:em_analise` e o hash de **219**. `emitidos` foi `10 → 11` (**204**, **226**, **279**). `app/ponto/services/cartorio.py:1114,1209–1222,1541–1547,2136–2141`. |
| **143945** | **P1.** Repasse do emissor no corpo inicial. O complemento de marcos reapontou `57630` de `38873` para `38907` (ESCRITA **299**). `analisar()` mudou `38907` de `aberto` a `em_analise` (ESCRITA **301**, DELTA **303**). O repasse inseriu também `68262` para `38907` ao perguntar entradas pendentes (ESCRITA **305**). `fio:1` aparece nas duas leituras (**291**, **338**). `app/ponto/management/commands/detectar_ausencias.py:791–807` · `app/chamados/services/disputa_emissao.py:1091–1099,1284` · `app/chamados/models.py:3171–3175,3195–3209,859–866`. | **B**, **338** depois dos quatro eventos. A leitura mostra a transição de estado e o hash de **303**. O efeito individual das duas escritas de pergunta na projeção é [nao verificado]. `emitidos` foi `100 → 101` (**291**, **311**, **338**). `app/ponto/services/cartorio.py:1114,1209–1222,1527–1547,2136–2141`. |
| **129134** | **P1.** O juízo deu `turno_encerrado=true` e `FURO_SEM_COBRANCA` (**863**). O emissor abriu `43299:aberto`, e o campo de fio passou de ausente a `fio:0` (ESCRITA **865**, DELTA **867**). A fábrica inseriu as quatro perguntas `68317`, `68318`, `68319` e `68320` para `43299` (ESCRITAS **1000–1003**), com delta de `fio:0 → 1` no intervalo da fábrica (**1005**). Qual das quatro fez o fio acender primeiro é [nao verificado]. `app/ponto/services/cartorio.py:1909–1910,1952–1955,1179–1184` · `app/ponto/management/commands/detectar_ausencias.py:694–714` · `app/chamados/services/fabrica_da_celula.py:160–174,206–219`. | **B**, **1007** para todos esses eventos. A leitura já viu `43299:aberto` com `fio:1`, e o juízo foi **1008**. `emitidos` foi `116 → 117` (**862**, **873**, **1007**). P2 **1636** e P3 **1807** leram o mesmo hash de **1007**, com `cobrado` no objeto. RESUMO **1866**. `app/ponto/services/cartorio.py:1114,1209–1222,1541–1558,1628–1653`. |

As três materializações de pergunta **não** devem ser inferidas a partir dos fechos de P2. O extrato registra duas `ESCRITA` desse tipo, **1701** e **1760**, com `materializada_em` preenchido às `06:27−03:00`, `materializada_por_id` de `null` para `666` e `via_resolucao` de vazia para `chamado_encerrado`. Para **138359**, há retornos de cascata em **1651/1653**, mas uma gravação equivalente de pergunta é [nao verificado]. `app/chamados/signals.py:91–117` · `app/chamados/models.py:3334–3348`.

## 2. Eventos sem leitura posterior no mesmo passe

| Célula | Braço e rodada | Etapa, números e primeira leitura posterior |
|---|---|---|
| **128355** | `encerrado`, **P1** | Carimbo da 2ª passada. Última leitura **1481**, intervalo **1491–1492**, DELTA **1493**. `39234` perdeu o fio (`1 → 0`) e ainda estava `em_analise` na leitura inicial de P2, **1752**. `app/ponto/services/cartorio.py:1527–1547,2035–2037` · `app/ponto/portas/celula.py:1181–1202`. |
| **129943** | `encerrado`, **P1** | Carimbo da 2ª passada. Última leitura **1058**, intervalo **1068–1069**, DELTA **1070**. `36531` perdeu o fio (`1 → 0`) e ainda estava `em_analise` na leitura inicial de P2, **1693**. `app/ponto/services/cartorio.py:1527–1547,2035–2037` · `app/ponto/portas/celula.py:1181–1202`. |
| **138359** | `encerrado`, **P1** | Carimbo da 2ª passada. Última leitura **65**, intervalo **75–76**, DELTA **77**. `37355` perdeu o fio (`1 → 0`) e ainda estava `em_analise` na leitura inicial de P2, **1641**. `app/ponto/services/cartorio.py:1527–1547,2035–2037` · `app/ponto/portas/celula.py:1181–1202`. |

Nenhuma linha `ESCRITA` publicada ficou depois da última `LEITURA` de sua célula no mesmo passe. Nos três casos acima, o `RESUMO` diz `sim` para a **primeira** mudança, que foi o fecho no corpo inicial. O zero em `escritas_depois_da_ultima_leitura` conta linhas monitoradas de chamado ou pergunta. O delta tardio ocorreu no carimbo da **célula**, que grava a impressão calculada antes dele. Anexo 3, `seq` 65/77, 1058/1070, 1481/1493 e 1858–1860. Anexo 4, `RESUMO`. `app/ponto/services/cartorio.py:1541–1547,2035–2037` · `app/ponto/portas/celula.py:1194–1202`.

Nos nove `RESUMO` fornecidos, todo `releu_depois: nao` vem acompanhado de `primeira_mudanca: "nenhuma nas fotos"`. Nenhum `RESUMO` fornecido traz `[nao verificado]`. O caso que exige conferir o bruto é **129476/P1**. A primeira mudança nas fotos é “nenhuma”, mas houve a ESCRITA **421** e a `LEITURA` posterior **845**, com hash igual ao de **378**. A legenda prevê `[nao verificado]` quando a leitura cai dentro do intervalo do delta. Uma ocorrência dessa ordem ambígua em outro braço é [nao medido]. Anexo 3, `seq` 1858–1866, 378, 421 e 845. Anexo 4, `RESUMO`. `app/ponto/services/cartorio.py:1541–1558`.

## 3. Mudança em P1 e novamente em P2

São **128355**, **129943** e **138359**. Em P1, o corpo inicial resolveu o chamado que protestava, e a 2ª passada ainda leu o outro chamado vivo **com fio**. O carimbo dessa 2ª passada deixou a célula `concorde` e produziu `fio:0`. Em P2, o corpo inicial leu o chamado restante vivo, **sem protesto e sem fio**, fechou-o e colocou a célula na fila. A 2ª passada de P2 leu ambos resolvidos, com o campo de fio ausente. Anexo 3, `seq` 1436–1441, 1481–1493, 1752–1778, 1018–1023, 1058–1070, 1693–1719, 19–24, 65–77 e 1641–1670. `app/ponto/services/cartorio.py:543–547,1527–1547,1607–1617,1683–1704,1726–1738`.

A condição nova foi **o último chamado continuar vivo, mas já sem fio depois da lavra de P1**. O corpo calcula `_com_fio` antes do juízo e do carimbo. Em P1, as segundas leituras ainda entregaram `com_fio=1` ao juízo, e o caminho de fecho por não protesto registrou retornos `false`. `reconciliar()` exige que as guardas da premissa e da pergunta viva permitam o fecho, e C6 pode mudar seu resultado ao ler o veredito e a ata gravados. A cláusula exata que devolveu cada `false` é [nao verificado]. Com a fila já consumida e sem releitura por emissão registrada para esses atos, P2 foi o primeiro corpo que recarregou os chamados e calculou a impressão com `fio:0`. Isso mudou o hash diante da impressão persistida em P1, e o corpo chegou ao fecho mesmo com o relógio fixo. Anexo 3, `seq` 1, 65–77, 1058–1070, 1481–1493, 1641–1661, 1693–1710 e 1752–1769. `app/ponto/services/cartorio.py:1152–1164,1209–1222,1462,1527–1558,1683–1704,2035–2037` · `app/chamados/models.py:422–443,654–668,683–705` · `app/chamados/juizes.py:1450–1469`.

As outras seis células não têm novo delta em P2 ou P3 no extrato. Em particular, **129134** já abriu a cobrança, ganhou fio e foi relida em P1 no braço `encerrado`. Os números e a sequência de rodadas da matriz do anexo 2 não identificam os eventos desse braço. Anexo 3, `seq` 862–873, 1000–1008, 1636, 1807 e 1861–1866. `app/ponto/services/cartorio.py:1179–1184,1209–1222,1540–1547`.

## 4. As três caixas do C3

- **Se “couber” significa que a via existente efetivamente leu a célula depois da escrita no mesmo ato**, valem as caixas da primeira tabela. A caixa **C** contém os três deltas de carimbo listados na segunda tabela. As escritas de pergunta sem delta individual também tiveram leitura posterior da célula. Para **129490**, **142995** e **129134**, o `false` na tupla do `RESUMO` da primeira mudança coincide com um hash posterior diferente, que já contém uma segunda mudança. As leituras **846**, **279** e **1007** mostram o estado acumulado. Anexo 1, C3. Anexo 3, `seq` 216/219/279, 480/484/846, 867/1005/1007 e 1862/1864/1866. `app/ponto/services/cartorio.py:1152–1157,1209–1222,1541–1558`.
- **Se “couber” exige que a própria etapa acione a via**, os três deltas de carimbo continuam em **C**. Nessa leitura também ficam sem via própria as escritas da **fábrica** em **129134** (**1000–1003**, DELTA **1005**, `fio:0 → 1**) e **138541** (**1347**, DELTA **1349**, `fio:0 → 1`), o **repasse para outro dia do ato `869`** em **129476** (**421**, inserção de `68298` para `36124`, sem delta registrado) e os **carimbos de pergunta pelo sinal do fecho** em **129943** (**1701**, `53186**) e **128355** (**1760**, `58553**`), ambos pela via `chamado_encerrado`. Eles foram alcançados pela emissão contada no ato ou pela fila criada pelo fecho do chamado. O repasse feito dentro das emissões contadas de **142995**, **143945** e **129490** está no caminho de **B**, assim como a abertura de `43299` em **129134**. O anexo 1 não explicita se a cobertura por esse encadeamento basta. Anexo 1, C3. Anexo 3, `seq` 226, 311, 421, 500, 845–846, 873, 1000–1007, 1258, 1347–1385, 1701–1718 e 1760–1777. `app/ponto/services/cartorio.py:1152–1157,1179–1184,1209–1222,1695–1704,1952–1955,2136–2141` · `app/ponto/management/commands/detectar_ausencias.py:791–807` · `app/chamados/signals.py:91–117`.

## 5. Limites da medida

- **[nao medido]** Deltas transitórios entre fotos, SQL fora dos campos e vínculos monitorados, efeitos de callbacks `on_commit`, consumidores e escritores externos fora do trecho observado. A ordem desta resposta se restringe aos eventos publicados. Anexo 4, `FOTO`, `ESCRITA`, `RESUMO` e `BRACO`. `app/chamados/signals.py:134–146,229–240` · `app/chamados/reconciliador.py:1414–1438`.
- **[nao medido]** Comportamento de células sem evento registrado e equivalência de ensaiar uma frota recortada. A legenda diz que a sonda preserva a frota e a ordem do comando. A cascata pode alcançar clones por PK, sem filtro de colaborador naquela consulta. Anexo 4. `app/ponto/management/commands/processar_cartorio.py:51–69` · `app/chamados/signals.py:149–167` · `app/chamados/services/materializacao.py:1269–1279`.
- **[nao medido]** Cobertura de uma mudança nos outros braços, inclusive quando uma célula não se moveu neles. Em `encerrado`, **129476** demonstra uma escrita e uma leitura posterior com o mesmo hash. Isso mede a ordem, e o efeito individual da pergunta na vitalidade é [nao verificado]. Para **129134**, o extrato mede `turno_encerrado=true` desde P1, com o mesmo relógio nas três rodadas. A passagem de `enc:0` para ausência de `enc` do controle da matriz é [nao medido] aqui. Anexo 3, `seq` 1, 378/421/845, 862–863, 1007–1008, 1636 e 1807. `app/ponto/services/cartorio.py:1293–1297,1527–1547,1624` · `app/ponto/turnos.py:2117–2134`.
- **[nao verificado]** Identidade do HEAD da cópia, script em `logs/sombra/`, saída integral com `FOTO` e `LOTE`, confirmação de `ERRO=0` e do rollback desta execução. A cópia disponível não tinha metadados Git nem `logs/sombra/`. A legenda descreve o rollback, e o código aberto do embrulho mostra a chamada ao script externo. Anexos 3 e 4. `bin/sombra.sh:481–492`.

## 20/11 04:15 · sessao 0e2ebed515d6 · E4C-F5-CONSTROI · sonda · origem=Code

```python
from collections import Counter, defaultdict
from functools import lru_cache
from itertools import product
from logging import disable, root
from types import SimpleNamespace
from unittest.mock import patch


# E4C-F5-CONSTROI, árvore solicitada 76ce6695c4142ca0577285407fe0068f618a3c89.
# Identidade Git da cópia e execução na sombra [nao verificado].
PAGINA = 2000
FALHOU = object()
SIM_NAO = {True: "sim", False: "nao"}
CLASSES = {(True, False): "A", (False, True): "B"}
FAIXAS = ("sim", "nao", "sem-dia", "NAO-MEDIDO")

MODELS = "app/chamados/models.py"
JUIZES = "app/chamados/juizes.py"
ACOES = "app/chamados/services/acoes_disputa.py"
DECL = "app/chamados/services/declaracao_texto.py"
EMISSAO = "app/chamados/services/disputa_emissao.py"
MAT = "app/chamados/services/materializacao.py"
VAL = "app/chamados/services/validacao.py"
VIGIA = "app/chamados/services/vigia_carimbo.py"
PASSIVO = "app/chamados/services/passivo_validado.py"
REC = "app/chamados/reconciliador.py"
K6 = "app/chamados/services/vigia_fio.py:789 app/folha/export.py:556"

GUARDAS = {
    "corrigir": f"{ACOES}:349",
    "declarar": f"{DECL}:229",
    "fecho_sem_credor": f"{EMISSAO}:2085",
    "par_exige_validada": f"{MAT}:201",
    "par_implausivel_padrao": f"{MAT}:203",
    "par_implausivel_plantio": f"{MAT}:203",
    "materializador_releitura": f"{MAT}:2130",
    "validar_irma": f"{VAL}:641",
    "validar": f"{VAL}:687",
    "recusar": f"{VAL}:901",
    "lote_entrada": f"{VAL}:1067",
    "lote_retorno": f"{VAL}:1089",
    "devolver_coluna_atual": f"{VAL}:270",
}
PORTAS_M4 = {
    "corrigir": f"{ACOES}:338 {ACOES}:349 {EMISSAO}:1455",
    "recusar": f"{VAL}:888 {VAL}:901 {VAL}:905 {MODELS}:3413",
    "declarar": f"{DECL}:229 {DECL}:322 {DECL}:362 {DECL}:651",
    "aplicar_declaracao": f"{DECL}:229 {DECL}:596 {DECL}:651 {DECL}:670",
}
FILAS = {
    "materializador": f"copiado {MAT}:2111",
    "vigia_carimbo": f"copiado {VIGIA}:97",
    "reparo_passivo": f"copiado {PASSIVO}:327 {VIGIA}:97",
    "absolviveis": f"importado {REC}:671",
    "orfas": f"importado {REC}:1539",
}

erros = Counter()
amostras_erro = defaultdict(list)
m1 = Counter()
pks_m1 = defaultdict(list)
m2 = Counter()
m3 = Counter()
m3_combinacoes = Counter()
m4 = defaultdict(list)
m4_materializadas = defaultdict(list)
m5 = Counter()
m5_c6 = Counter()
m5_materializador_precedidas = Counter()
m6 = Counter()
rotulos_motivo = {}
universo = None
lidas = 0
concluida = False


def falha(medida, pks, exc):
    tipo = type(exc).__name__
    for pk in pks:
        erros[medida] += 1
        if len(amostras_erro[medida]) < 5:
            amostras_erro[medida].append((pk, tipo))


def analisar(transaction):
    global universo, lidas, concluida

    from django.contrib.auth.models import User
    from django.db.models import Exists, OuterRef
    from django.utils import timezone

    from chamados import juizes as j
    from chamados.catalogo.modulos import CALADOS_POR_ISENCAO
    from chamados.catalogo.motor import ENCERRADOS, VIA_COMPETENCIA_TRANCADA, VIVOS
    from chamados.models import (
        ChamadoColaborador,
        DecisaoPergunta as D,
        DisputaSupervisao,
        FollowupChamado,
        PerguntaDisputa as P,
    )
    from chamados.reconciliador import perguntas_orfas_reatavies, qs_perguntas_absolviveis
    from chamados.services import declaracao_texto as decl
    from chamados.services import materializacao as mat
    from chamados.services import validacao as val
    from chamados.services.fio import universo_do_fato
    from folha import export
    from folha.models import ExportacaoDominio, anotar_vigencia
    from ponto.janelas import janela_atual

    def medir(medida, pks, leitura, *, sql=True):
        try:
            if sql:
                with transaction.atomic():
                    return leitura()
            return leitura()
        except Exception as exc:
            falha(medida, pks, exc)
            return FALHOU

    @lru_cache(maxsize=1)
    def usuario_sistema_leitura():
        # Consumidor em juizes.py:1228. Substitui só a busca do usuário.
        return User.objects.only("pk").get(username="sistema")

    def propagar_engolido(_logger, _operacao, exc, **_contexto):
        # core/observ.py:73 e chamados/juizes.py:1484, :1497.
        raise exc

    agora = timezone.now()
    hoje = timezone.localdate(agora) if timezone.is_aware(agora) else agora.date()
    tipos = {valor for valor, _ in P._meta.get_field("tipo_resposta").flatchoices}
    atos = {valor for valor, _ in D.ATOS}
    motivos_fixos = {
        "": "vazio",
        D.MOTIVO_PROJECAO_SEM_LINHA: "projecao_sem_linha",
        "resposta automatica (fato entregue por outro caminho)": "resposta_automatica",
        "backfill E1 v5: admin_decidiu sem ato (E2 B1)": "backfill_admin_decidiu",
    }

    def ultimo_ato(p):
        # models.py:4191, :4225, :4329. Última linha física, inclusive empate por pk.
        linhas = list(p.decisoes.all())
        if not linhas:
            return ("sem-linha", "sem-linha", "sem-linha", "sem-linha")
        d = linhas[-1]
        instante = timezone.localtime(d.ato_em) if timezone.is_aware(d.ato_em) else d.ato_em
        data = instante.date()
        motivo = motivos_fixos.get(d.ato_motivo)
        if motivo is None:
            motivo = rotulos_motivo.setdefault(
                d.ato_motivo, f"motivo_reservado_{len(rotulos_motivo) + 1}"
            )
        faixa = "hoje" if data == hoje else "anterior" if data < hoje else "futuro"
        return (data.strftime("%Y-%m"), d.ato if d.ato in atos else "fora-catalogo", motivo, faixa)

    def marcar5(guarda, classe, resultado):
        if resultado is not FALHOU:
            m5[guarda, classe, resultado] += 1

    perguntas = P.objects.select_related(
        "celula", "chamado_destino__celula", "disputa__chamado__celula",
        "disputa__colaborador__empresa",
    ).prefetch_related("decisoes")

    # Materializador: mesmo Q e mesmos predicados da relação disputa.perguntas.
    prontas = P.objects.filter(
        j.q_respondida_com_texto() | j.q_respondida_sem_texto(),
        validada_em__isnull=False,
        materializada_em__isnull=True,
    )
    # Vigia sem recorte de empresa/célula. O reparo consome este mesmo universo.
    vigia = P.objects.filter(
        validada_em__isnull=False, tipo_resposta__in=("hora", "data_hora")
    )

    def selecao_m3(pks):
        colab = OuterRef("disputa__colaborador_id")
        pk = OuterRef("pk")
        return list(
            P.objects.filter(pk__in=pks).annotate(
                s_mat=Exists(prontas.filter(pk=pk)),
                s_vigia=Exists(vigia.filter(pk=pk)),
                s_abs=Exists(qs_perguntas_absolviveis(colab).filter(pk=pk)),
                s_orfa=Exists(perguntas_orfas_reatavies(colab).filter(pk=pk)),
            ).values_list("s_mat", "s_vigia", "s_abs", "s_orfa")
        )

    exportacoes_por_chave = {}
    k6_real = export.competencia_entregue

    def juiz_k6(empresa_id, mes, ano, registro=False, *, exportacoes=None):
        chave = (getattr(empresa_id, "pk", empresa_id), int(mes), int(ano))
        if exportacoes is None:
            exportacoes = exportacoes_por_chave.get(chave)
        return k6_real(*chave, registro=registro, exportacoes=exportacoes)

    def carregar_k6(chaves):
        # Alimentação em lote do argumento público de folha/export.py:582.
        chaves = set(chaves).difference(exportacoes_por_chave)
        if not chaves:
            return
        registros = anotar_vigencia(list(ExportacaoDominio.objects.filter(
            empresa_id__in={e for e, _, _ in chaves if e is not None},
            mes__in={m for _, m, _ in chaves},
            ano__in={a for _, _, a in chaves},
        ).only("pk", "empresa_id", "mes", "ano", "invalidada_em", "gerado_em")))
        grupos = {chave: [] for chave in chaves}
        for r in registros:
            chave = (r.empresa_id, r.mes, r.ano)
            if chave in grupos:
                grupos[chave].append(r)
        for chave, grupo in grupos.items():
            exportacoes_por_chave[chave] = grupo

    def chave_dia(p, dia):
        if dia is None:
            return None
        empresa = p.disputa.colaborador.empresa
        fim = janela_atual(dia, empresa)[1]
        return (getattr(empresa, "pk", None), fim.month, fim.year)

    def faixas_k6(divergentes):
        chaves = {}
        for p, _, _ in divergentes:
            chaves[p.pk] = medir(
                "M2_M4_K6", (p.pk,), lambda: chave_dia(p, j._dia_de_turno_da_pergunta(p)),
                sql=False,
            )
        indexadas = [p.pk for p, _, _ in divergentes if isinstance(chaves[p.pk], tuple)]
        carga = medir(
            "M2_M4_K6", indexadas,
            lambda: carregar_k6(chaves[pk] for pk in indexadas),
        )
        resultado = {}
        for p, _, _ in divergentes:
            chave = chaves[p.pk]
            if chave is None:
                resultado[p.pk] = "sem-dia"
            elif chave is FALHOU or (carga is FALHOU and chave not in exportacoes_por_chave):
                resultado[p.pk] = "NAO-MEDIDO"
            else:
                entregue = medir("M2_M4_K6", (p.pk,), lambda: juiz_k6(*chave), sql=False)
                resultado[p.pk] = "NAO-MEDIDO" if entregue is FALHOU else SIM_NAO[entregue]
        return resultado

    def carregar_irmas(alvos):
        # Cópia em lote do SQL de metade_irma_do_intervalo, materializacao.py:139.
        chaves = {(p.disputa_id, p.data_evento) for p in alvos}
        base = perguntas.filter(
            disputa_id__in={d for d, _ in chaves},
            data_evento__in={dia for _, dia in chaves},
            tipo_resposta="hora", motivo__in=tuple(mat._PAR_DO_INTERVALO),
        )
        candidatas = set(base.filter(
            j.q_respondida_com_texto(), materializada_em__isnull=True
        ).values_list("pk", flat=True))
        grupos = defaultdict(list)
        for p in base.order_by("pk").iterator(chunk_size=PAGINA):
            if (p.disputa_id, p.data_evento) in chaves:
                grupos[p.disputa_id, p.data_evento, p.motivo].append(p)
        return chaves, grupos, candidatas

    def validar_antes_da_escrita(p, leitura, *, confirmar):
        # validacao.py:682–745. Retornos desta função são rótulos da sonda.
        if not j.pergunta_respondida(leitura) or leitura.validada_em is not None:
            return "retorna"
        if val._resposta_de_hora(p) and mat.espera_declarado_x_gravado(p):
            return "retorna"
        if j.motivo_implausivel_validacao(p) and not confirmar:
            return "retorna"
        if val._e_aceite_de_folga(p):
            return "porta_folga"
        if val.entrega_que_barra_a_validacao(p, val.irma_da_validacao(p)):
            return "retorna"
        return "carimba"

    def prefixo_plantio(p, leitura):
        # materializacao.py:1872, :1893, :1920, :1931, :1306, :1366, :1380, :1396.
        if (val.entrega_que_barra_a_validacao(p)
                or mat._data_em_disputa(p) or mat.espera_na_escrita(p.disputa, p)
                or not j.pergunta_respondida(leitura)):
            return False
        ts, tipo = mat._ts_tipo_do_plantio(p.disputa, p, [])
        if ts is None:
            return False
        colab = p.disputa.colaborador
        return (
            j._batida_existente_na_janela(colab, ts, tipo) is None
            and mat._vizinha_mesmo_tipo(colab, ts, tipo) is not None
            and not mat._plantio_preenche_furo_na_data(colab, ts, pergunta=p)
        )

    guardas_par = (
        "par_exige_validada", "par_implausivel_padrao",
        "par_implausivel_plantio", "validar_irma",
    )

    def medir_pares(divergentes, carga_irmas, irma):
        if carga_irmas is FALHOU:
            return
        _, grupos, _ = carga_irmas
        for q, classe, leitura in divergentes:
            oposto = mat._PAR_DO_INTERVALO.get(q.motivo)
            primarias = grupos.get((q.disputa_id, q.data_evento, oposto), ())

            def prefixos():
                # materializacao.py:189–203. O alvo da guarda é q, não p.
                return [
                    (p, lido)
                    for p in primarias
                    if (lido := D.composta(p)) is not None
                    and j.pergunta_respondida(lido)
                    and (alvo := irma(p.disputa, p)) is not None
                    and alvo.pk == q.pk
                    and not alvo.pendencia_admin
                ]

            ps = medir("M5_PAR_PREFIXO", (q.pk,), prefixos, sql=False)
            if ps is FALHOU:
                continue
            marcar5("par_implausivel_padrao", classe, bool(ps))
            plantio = medir(
                "M5_PAR_PLANTIO", (q.pk,),
                lambda: any(prefixo_plantio(p, lido) for p, lido in ps),
            )
            marcar5("par_exige_validada", classe, plantio)
            marcar5(
                "par_implausivel_plantio", classe,
                FALHOU if plantio is FALHOU else plantio and leitura.validada_em is not None,
            )

            def chega_validar_irma():
                # validacao.py:633, :699, :742, :745–773. Foto anterior ao recálculo.
                for p, lido in ps:
                    if p.materializada_em is not None or not val._resposta_de_hora(p):
                        continue
                    if validar_antes_da_escrita(p, lido, confirmar=True) != "carimba":
                        continue
                    alvo, _, _ = mat.par_do_intervalo(p.disputa, p)
                    if alvo is not None and alvo.pk == q.pk:
                        return True
                return False

            marcar5("validar_irma", classe, medir("M5_VALIDAR_IRMA", (q.pk,), chega_validar_irma))

    def candidatas_sem_credor(divergentes):
        # disputa_emissao.py:1946, :2071–2085. Disparador da foto = chamado
        # resolvido/fechado do mesmo colaborador, com via admitida pelo prefixo.
        pks = [p.pk for p, _, _ in divergentes]
        outros = P.objects.filter(
            disputa_id=OuterRef("disputa_id"), chamado_destino__status_local__in=VIVOS,
        )
        disparadores = ChamadoColaborador.objects.filter(
            colaborador_id=OuterRef("disputa__colaborador_id"), status_local__in=ENCERRADOS,
        ).exclude(via_encerramento=VIA_COMPETENCIA_TRANCADA)
        return set(P.objects.filter(
            pk__in=pks, disputa__fechada_em__isnull=True, materializada_em__isnull=True,
        ).exclude(
            disputa__chamado__status_local__in=VIVOS,
        ).filter(~Exists(outros), Exists(disparadores)).values_list("pk", flat=True))

    def medir_c6(alvos):
        # A vitalidade vem depois de :2085. Mede o próximo veto separadamente.
        ps = [p for p, _, _ in alvos]
        with j.tranca_da_consulta(ps):
            for p, classe, _ in alvos:
                viva = medir("M5_C6", (p.pk,), lambda: j.pergunta_viva(p))
                if viva is not FALHOU:
                    m5_c6[classe, viva] += 1

    @lru_cache(maxsize=4096)
    def faltantes(colab, dia):
        return decl._marcos_faltantes(colab, dia.isoformat())

    @lru_cache(maxsize=4096)
    def alvo_declaracao(colab_pk, motivo, dia):
        return decl._segura_da_lista(universo_do_fato(colab_pk, motivo, dia))

    def ancoras(p):
        return {
            ch.pk: ch
            for ch in (p.chamado_destino, p.disputa.chamado)
            if ch is not None and ch.colaborador_id == p.disputa.colaborador_id
            and (ch.modulo_origem or "") in CALADOS_POR_ISENCAO
        }

    def insumos_declaracao(bs):
        ids = {pk for p in bs for pk in ancoras(p)}
        horas = defaultdict(list)
        for ch_pk, texto in FollowupChamado.objects.filter(
            chamado_id__in=ids, autor_tipo="colaborador",
        ).order_by().values_list("chamado_id", "conteudo").iterator(chunk_size=PAGINA):
            hora = decl.extrair_hora(texto)
            if hora:
                horas[ch_pk].append(hora)
        abertas = {}
        for colab_pk, d_pk in DisputaSupervisao.objects.filter(
            colaborador_id__in={p.disputa.colaborador_id for p in bs},
            fechada_em__isnull=True,
        ).values_list("colaborador_id", "pk"):
            abertas.setdefault(colab_pk, d_pk)
        return horas, abertas

    def prefixo_declaracao(p, fontes, *, aplicar):
        # declaracao_texto.py:322–380, :605–637, :651–670.
        horas, abertas = fontes
        colab = p.disputa.colaborador
        for ch in ancoras(p).values():
            dia_ch = j.dia_do_fato(ch)
            if dia_ch is None:
                continue
            estado, lista = faltantes(colab, dia_ch)
            if estado == decl.ESTADO_SEM_CELULA:
                continue
            entradas = horas[ch.pk] if aplicar else [marco[3] for marco in lista]
            for hora in entradas:
                casado = decl._casar_hora(lista, hora)
                if casado is None:
                    continue
                dia, pos, tipo, _ = casado
                motivo = decl._MOTIVO_POR_MARCO[pos, tipo]
                if motivo != p.motivo:
                    continue
                alvo = alvo_declaracao(colab.pk, motivo, dia)
                if alvo is None or alvo.pk != p.pk:
                    continue
                if p.disputa_id == abertas.get(colab.pk) and j.pergunta_sem_lastro_ou_viva(p):
                    return True
                if j.resolvedora_valida(ch, p, dia_do_fato=dia)[0]:
                    return True
        return False

    def medir_portas(p, classe, leitura, faixa, fontes):
        # POST unitário com argumentos válidos para corrigir/recusar.
        for guarda in ("corrigir", "recusar", "lote_entrada"):
            marcar5(guarda, classe, True)
        respondeu = j.pergunta_respondida(leitura)
        marcar5("validar", classe, respondeu)
        marcar5(
            "devolver_coluna_atual", classe,
            val._resposta_de_hora(p) and p.materializada_em is None
            and bool((p.recusa_motivo or "").strip()),
        )

        # Guarda de declaração isolada, chamada real.
        validada_declaracao = medir(
            "M4_M5_DECLARAR_GUARDA", (p.pk,), lambda: decl._resposta_validada(p), sql=False,
        )
        if validada_declaracao is not FALHOU:
            marcar5("declarar", classe, validada_declaracao != (p.validada_em is not None))

        def retorno_lote():
            # Lote unitário sem confirmação de implausibilidade. A guarda anterior
            # usa o ato real. Uma recusa anterior à escrita conserva a foto.
            if leitura.validada_em is not None or val.classe_do_lote(p)[0] != val.LOTE_A:
                return False
            etapa = validar_antes_da_escrita(p, leitura, confirmar=False)
            return True if etapa == "retorna" else None

        marcar5("lote_retorno", classe, medir("M5_LOTE_RETORNO", (p.pk,), retorno_lote))
        if classe != "B":
            return

        if p.materializada_em is not None:
            m4_materializadas[faixa].append(p.pk)
        for porta, prefixo in (
            ("corrigir", p.disputa.aberta and p.tipo_resposta == "hora"),
            ("recusar", respondeu and not p.fora_do_fato()),
        ):
            m4[porta, faixa, "GUARDA_LIBERA"].append(p.pk)
            if prefixo:
                m4[porta, faixa, "PREFIXO_PASSA"].append(p.pk)

        for porta in ("declarar", "aplicar_declaracao"):
            if validada_declaracao is FALHOU or validada_declaracao:
                continue
            m4[porta, faixa, "GUARDA_LIBERA"].append(p.pk)
            if fontes is FALHOU:
                continue
            prefixo = medir(
                f"M4_{porta}", (p.pk,),
                lambda: prefixo_declaracao(p, fontes, aplicar=(porta == "aplicar_declaracao")),
            )
            if prefixo is True:
                m4[porta, faixa, "PREFIXO_PASSA"].append(p.pk)

    def medir_pagina(pagina):
        divergentes = []


[truncado em 20000 de 32963 caracteres]

## 20/11 04:04 · sessao 16fb3fee2234 · E4C-F5-CONSTROI · verificador · origem=Code

**E4C-F5-CONSTROI — BLOQUEIA.** A volta D permite que a validação vigente impeça novas ações enquanto o materializador e o vigia excluem a mesma pergunta pela coluna nula. No sentido inverso, correção e recusa passam a admitir escritas que a coluna antes barrava, inclusive em competência entregue. `app/chamados/services/validacao.py:687`, `app/chamados/services/validacao.py:900`, `app/chamados/services/materializacao.py:2111`, `app/chamados/services/vigia_carimbo.py:97`, `app/chamados/services/acoes_disputa.py:349`, `app/chamados/services/acoes_disputa.py:375`.

Usei o diff fornecido para o antes e os arquivos da cópia para o depois. Conferi a catraca e as substituições dos mutantes em memória, em modo somente leitura. Identidade Git da cópia, execução da suíte, ocorrências e contagens em produção [nao verificado]. [nao verificado]

## 1. Quem escreve e quem lê — BLOQUEIA

Chamo de **A** a validação que a `composta` dá como vigente com coluna nula, e de **B** a coluna preenchida com validação que a `composta` dá como desfeita. Em B, a ausência de uma linha `VALIDACAO` por si só é insuficiente. O leitor incorpora o carimbo sem linha correspondente como passivo, no instante da coluna. Uma `RESPOSTA` humana posterior que desfaça a validação produz B, como faz a fixture do delta. `app/chamados/models.py:4276`, `app/chamados/models.py:4290`, `app/chamados/models.py:4333`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:35`.

| Campo ou sequência | Escritores no caminho auditado | Leitores que decidem |
|---|---|---|
| `PerguntaDisputa.validada_em` | `_carimbar_validacao` grava a assinatura e o ato, inclusive para irmã e folga. As duas devoluções limpam a assinatura. Resposta humana, reabertura/recusa e ressuscitação também limpam. O nascimento pode receber a assinatura e `carimbar(resposta='validar')` pode preencher a coluna. `app/chamados/services/validacao.py:161`, `app/chamados/services/validacao.py:270`, `app/chamados/services/validacao.py:347`, `app/chamados/services/validacao.py:641`, `app/chamados/services/disputa_emissao.py:1484`, `app/chamados/services/disputa_emissao.py:1617`, `app/chamados/models.py:1721`, `app/chamados/models.py:3229`, `app/chamados/models.py:3354`. | Materializador, vigia/reparo, absolvíveis e órfãs do reconciliador, devolução, estoque L130, os próprios escritores ao decidir o que limpar, sintetizar ou preencher, e os templates ao mostrar os botões. As janelas de painel, verbos e `flip_auto` também selecionam ou contam pela coluna. `app/chamados/services/materializacao.py:2111`, `app/chamados/services/vigia_carimbo.py:97`, `app/chamados/reconciliador.py:678`, `app/chamados/reconciliador.py:1100`, `app/chamados/services/validacao.py:270`, `app/chamados/services/validacao.py:393`, `app/chamados/models.py:4290`, `app/templates/chamados/partials/_pergunta_card.html:42`, `app/templates/chamados/partials/modal_fio.html:228`, `app/chamados/services/painel.py:268`, `app/chamados/services/verbos.py:264`, `app/ponto/services/flip_auto.py:130`. |
| Sequência `DecisaoPergunta` | A inserção passa por `registrar`. Chamam a porta resposta humana, automática e pela trilha, validação e devoluções, recusa/reabertura, ressuscitação, silêncio, retratação, absorção, nascimento assinado e backfills. A 0056 atualiza historicamente a flag das respostas. `app/chamados/models.py:4365`, `app/chamados/services/disputa_emissao.py:1532`, `app/chamados/services/disputa_emissao.py:1639`, `app/chamados/services/disputa_emissao.py:1686`, `app/chamados/services/disputa_emissao.py:1708`, `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:280`, `app/chamados/services/validacao.py:355`, `app/chamados/services/validacao.py:440`, `app/chamados/models.py:1673`, `app/chamados/models.py:1719`, `app/chamados/models.py:2110`, `app/chamados/models.py:2242`, `app/chamados/models.py:3234`, `app/chamados/services/materializacao.py:1066`, `app/ponto/services/veredito_celula.py:195`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:150`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:351`, `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:14`. | As guardas da tabela do item 2, C6, respostas sem veredito/C7, fila de validação e âncoras, por `composta` ou pela face SQL da validação. A tarefa do dia lê a ordem dos atos para atribuir a próxima ação. A ressuscitação lê a sequência para compensar e o BUG-PAR-103 procura `DEVOLUCAO`. Histórico, respostas anteriores e vigia de texto também consomem a sequência. `app/chamados/models.py:357`, `app/chamados/models.py:1707`, `app/chamados/models.py:2023`, `app/chamados/models.py:3563`, `app/chamados/models.py:3636`, `app/chamados/models.py:3826`, `app/chamados/juizes.py:1406`, `app/chamados/juizes.py:1770`, `app/chamados/services/tarefa_do_dia.py:154`, `app/chamados/services/materializacao.py:270`, `app/chamados/services/historico.py:98`, `app/chamados/services/vigia_texto_resposta.py:23`. |
| `PerguntaDisputa.materializada_em` | `_escrever_carimbo` atribui. `carimbar`, `carimbar_identidade` e `descarimbar` persistem, e `descarimbar_em_lote` limpa em SQL. Materializador, colisão, absorção, S143, S144 e reconciliador chamam essas portas. `app/chamados/models.py:3317`, `app/chamados/models.py:3372`, `app/chamados/models.py:3410`, `app/chamados/models.py:3432`, `app/chamados/models.py:3509`, `app/chamados/services/materializacao.py:1064`, `app/chamados/services/materializacao.py:1699`, `app/chamados/services/materializacao.py:1987`, `app/chamados/services/materializacao.py:2187`, `app/chamados/services/disputa_emissao.py:2031`, `app/chamados/services/disputa_emissao.py:2105`, `app/chamados/reconciliador.py:408`, `app/chamados/reconciliador.py:646`. | Seleção de irmã, confirmação do par, descarte no materializador, devolução, estado da pergunta, testemunha da tela, S143/S144, reconciliador, relavra, reparo do passivo, C6 e respostas sem veredito. O lastro pode contradizer o carimbo, e o vigia testa justamente essa diferença. `app/chamados/services/materializacao.py:146`, `app/chamados/services/materializacao.py:879`, `app/chamados/services/materializacao.py:2127`, `app/chamados/services/materializacao.py:2174`, `app/chamados/services/validacao.py:264`, `app/chamados/services/validacao.py:644`, `app/chamados/services/validacao.py:771`, `app/chamados/models.py:2025`, `app/chamados/models.py:3266`, `app/chamados/services/disputa_emissao.py:2084`, `app/chamados/reconciliador.py:369`, `app/chamados/reconciliador.py:680`, `app/chamados/services/relavra_via.py:181`, `app/chamados/services/passivo_validado.py:529`, `app/chamados/juizes.py:1393`, `app/chamados/services/vigia_carimbo.py:105`. |

A troca de teste booleano por `is not None` é equivalente para `datetime` ou `None`. A divergência relevante é **qual instante** passa a contar. A irmã em A pode agora ser plantada sem nova linha `VALIDACAO`. No plantio real, `carimbar(resposta='validar')` preenche a coluna nula com instante e autor do plantio. A próxima `leitura` pode incorporar esse carimbo como validação sintética posterior e devolver o instante novo por cima do ato original. O escritor é anterior ao delta, mas esse caminho até ele foi aberto pela C8. `app/chamados/services/materializacao.py:200`, `app/chamados/services/materializacao.py:1738`, `app/chamados/services/materializacao.py:1987`, `app/chamados/services/validacao.py:641`, `app/chamados/models.py:3354`, `app/chamados/models.py:4290`, `app/chamados/models.py:4337`.

## 2. Cada guarda e os dois sentidos — BLOQUEIA

Na tabela, «passa» significa que aquela guarda libera. A escrita depende dos demais requisitos da porta.

| Guarda | A — ato validado, coluna nula | B — ato sem validação, coluna preenchida |
|---|---|---|
| Corrigir | Antes podia corrigir e chamar validar. Agora retorna `info` sem corrigir. | Antes retornava `info`. Agora pode gravar resposta e ato, limpar o carimbo antigo e só então tentar validar. Uma recusa dessa validação deixa a correção gravada. `app/chamados/services/acoes_disputa.py:349`, `app/chamados/services/acoes_disputa.py:375`, `app/chamados/services/acoes_disputa.py:383`, `app/chamados/services/disputa_emissao.py:1484`. |
| Declarações e testemunha da tela | Antes a guarda liberava. Agora as duas portas podem retornar `validada_sem_registro`, e a testemunha oferece `aceita=False`. | Antes barrava. Agora a declaração do colaborador pode gravar a resposta ou retornar `ja_declarada`. No aplicar do admin, se a pergunta já está respondida, segue direto para validar a resposta que já estava gravada. `app/chamados/services/declaracao_texto.py:229`, `app/chamados/services/declaracao_texto.py:280`, `app/chamados/services/declaracao_texto.py:380`, `app/chamados/services/declaracao_texto.py:414`, `app/chamados/services/declaracao_texto.py:670`, `app/chamados/services/declaracao_texto.py:685`. |
| S143 sem credor | Antes entrava na seleção se M era nulo. Agora fica fora. | Antes ficava fora. Agora entra, mas o C6 ainda pode vetar o carimbo se a resposta mantiver a pergunta viva. Se carimbar, usa `resposta='responder'`. Fechar a disputa é outra decisão, pela cauda do C7. `app/chamados/services/disputa_emissao.py:2080`, `app/chamados/services/disputa_emissao.py:2093`, `app/chamados/services/disputa_emissao.py:2105`, `app/chamados/services/disputa_emissao.py:2126`, `app/chamados/juizes.py:1428`. |
| Irmã já validada | Antes devolvia «sem validação». Agora pode puxar a irmã para o plantio. | Antes passava por essa guarda. Agora bloqueia o plantio da irmã. `app/chamados/services/materializacao.py:200`, `app/chamados/services/materializacao.py:1738`. |
| Implausibilidade da irmã | Antes consultava a implausibilidade e podia impedir o par. Agora pula a consulta. | Antes pulava. Agora consulta e pode impedir o par, se a guarda anterior não tiver retornado. `app/chamados/services/materializacao.py:201`, `app/chamados/services/materializacao.py:203`. |
| Materializador sob trava | Se a coluna já estava nula na seleção, fica fora antes e agora. Se foi selecionada com a coluna preenchida e chega à trava em A, antes pulava e agora pode materializar. | Com resposta elegível e M nulo, entra no SQL nos dois lados. Antes tentava materializar. Agora pula sem limpar a coluna nem registrar erro naquele ramo. `app/chamados/services/materializacao.py:2111`, `app/chamados/services/materializacao.py:2123`, `app/chamados/services/materializacao.py:2130`. |
| Validar o par | Antes escrevia outra `VALIDACAO` da irmã. Agora pula essa escrita, com o efeito do carimbo descrito no item 1 se o plantio ocorrer. | Antes pulava a assinatura. Agora registra `VALIDACAO`. O savepoint exige os dois M preenchidos ou desfaz a tentativa. `app/chamados/services/validacao.py:633`, `app/chamados/services/validacao.py:641`, `app/chamados/services/validacao.py:644`. |
| Validar a pergunta | Com resposta elegível, antes podia assinar e tentar o plantio. Agora retorna `ja_validada` antes da trava e da materialização. | Antes retornava `ja_validada`. Agora pode assinar, materializar ou devolver. Se M já estiver preenchido, pula a unidade própria e pode retornar «ponto gravado» sem novo plantio ou nova prova de lastro da pergunta. `app/chamados/services/validacao.py:683`, `app/chamados/services/validacao.py:742`, `app/chamados/services/validacao.py:771`, `app/chamados/services/validacao.py:807`, `app/chamados/services/validacao.py:838`. |
| Recusar | Antes podia recusar e devolver ao colaborador. Agora retorna `ja_validada`. | Antes barrava. Agora pode gravar `RECUSA` e limpar resposta, validação e M. Se M já existia, a limpeza do carimbo da pergunta por essa porta não retrata a batida. `app/chamados/services/validacao.py:900`, `app/chamados/services/validacao.py:923`, `app/chamados/services/disputa_emissao.py:1617`, `app/chamados/services/disputa_emissao.py:1656`, `app/chamados/models.py:3509`. |
| Lote, entrada | Antes podia encaminhar à ação. Agora entra em `recusados` com «Ja validada». | Antes recusava na entrada. Agora pode chegar à ação se for classe A. A classificação ainda examina os dados e o conflito do slot. `app/chamados/services/validacao.py:1059`, `app/chamados/services/validacao.py:1067`, `app/chamados/services/validacao.py:1181`. |
| Lote, após a ação | Antes entrava em `recusados` se o refresh trouxesse coluna nula. Agora conta em `validadas` e também em `nao_gravados` se o retorno da ação indicar falha. | Antes contava em `validadas` pelo carimbo. Agora entra em `recusados`, mesmo se a ação tiver devolvido `sucesso`. Essa guarda contabiliza, sem escrever na pergunta ou na disputa. `app/chamados/services/validacao.py:1081`, `app/chamados/services/validacao.py:1088`. |

**Filas e disputa.** O vigia e o reparo do passivo excluem A. No caso B de hora ou data/hora, o vigia inclui a pergunta e o reparo pode chamar `_materializar_uma` diretamente, sem a nova guarda do materializador normal e sem reler V no refresh sob trava. Os universos de absolvíveis e órfãs do reconciliador fazem o inverso pelo SQL cru. A pode entrar nesses universos se atender aos filtros restantes, mas as órfãs ainda exigem C6 viva. B fica fora dos dois. Na fila de validação normal, a face SQL usa o ato para V, enquanto o filtro de resposta ainda exige instante projetado. `app/chamados/services/vigia_carimbo.py:97`, `app/chamados/services/passivo_validado.py:327`, `app/chamados/services/passivo_validado.py:529`, `app/chamados/reconciliador.py:678`, `app/chamados/reconciliador.py:713`, `app/chamados/reconciliador.py:1100`, `app/chamados/reconciliador.py:1136`, `app/chamados/juizes.py:1583`, `app/chamados/juizes.py:1770`.

**Caso sem saída ordinária.** Uma pergunta em A, com resposta válida, M nulo, sem lastro, sem irmã elegível e fora das absolvições do reconciliador, perde a tentativa de validar que antes podia aplicar a resposta. As portas individuais vetam pela validação vigente, a resposta direta web/app veta pelo C6, S143 sem credor a exclui e materializador/vigia não a selecionam. A disputa pode fechar sem aplicação se essa for a única pendência: C6 dá a pergunta por morta, respostas sem veredito não a retêm e S144 exige lastro. Há reabertura explícita para perguntas elegíveis, mas esse ato zera a resposta e a assinatura para começar de novo. A tarefa pode indicar `MATERIALIZAR` para o ato sem efeito mesmo com a pergunta fora da seleção do materializador. `app/chamados/services/validacao.py:687`, `app/chamados/views.py:1239`, `app/api/views.py:2636`, `app/chamados/services/disputa_emissao.py:2084`, `app/chamados/services/materializacao.py:2111`, `app/chamados/juizes.py:1406`, `app/chamados/models.py:2023`, `app/chamados/services/materializacao.py:2183`, `app/chamados/juizes.py:1762`, `app/chamados/services/acoes_disputa.py:100`, `app/chamados/services/tarefa_do_dia.py:173`.

A Central recebe as instâncias ORM e decide rótulo e disponibilidade de ações pela coluna. Em B, pode esconder ações que o servidor aceitaria. Em A, pode oferecer ações que o servidor agora veta. O cartão de detalhe também usa V cru para decidir se renderiza as ações. `app/chamados/services/fio.py:251`, `app/templates/chamados/partials/modal_fio.html:228`, `app/templates/chamados/partials/modal_fio.html:315`, `app/chamados/services/acoes_chamado.py:350`, `app/templates/chamados/partials/_pergunta_card.html:42`.

## 3. Devolução após a validação — BLOQUEIA

Há duas fontes no encadeamento. Na entrada, `validar_pergunta` decide pelo ato. No fluxo normal que chega à devolução, grava primeiro a coluna e a linha `VALIDACAO` juntas, tenta materializar e então relê a coluna para a limpeza. Esse percurso ordinário produz uma assinatura nova coerente antes da devolução. A divergência A existente na entrada retorna cedo e sequer chega à limpeza. `app/chamados/services/validacao.py:161`, `app/chamados/services/validacao.py:683`, `app/chamados/services/validacao.py:745`, `app/chamados/services/validacao.py:771`, `app/chamados/services/validacao.py:262`.

Na chamada direta, com recusa registrada, resposta de hora e M nulo, A pode reabrir a disputa **sem desfazer a validação do ato**. B limpa a coluna e grava `DEVOLUCAO` de uma validação já desfeita. Esse ato muda a leitura da tarefa do dia para `materializacao_recusada`. O teste novo fixa os dois resultados para a chamada direta, com disputa aberta. Chamadas diretas em produção [nao verificado]. `app/chamados/services/validacao.py:264`, `app/chamados/services/validacao.py:270`, `app/chamados/services/validacao.py:284`, `app/chamados/services/tarefa_do_dia.py:165`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:383`.

## 4. Banco, trava e cache — BLOQUEIA

| Caminho | O que a guarda lê |
|---|---|
| S143 sem credor | O wrapper pega a trava do colaborador antes de o corpo montar a lista. O prefetch da lista é feito sob essa trava. O encerramento por tranca retorna antes da guarda. `app/chamados/services/disputa_emissao.py:1907`, `app/chamados/services/disputa_emissao.py:1913`, `app/chamados/services/disputa_emissao.py:1946`, `app/chamados/services/disputa_emissao.py:2080`. |
| Declarações | As portas elegem a candidata depois de `_trava_do_ato`. A carga do universo é nova e traz decisões. Quando a costura tem sucesso, o refresh integral descarta essa carga e a guarda consulta novamente. A testemunha de tela usa a foto carregada sem passar pela trava da escrita. `app/chamados/services/declaracao_texto.py:252`, `app/chamados/services/declaracao_texto.py:301`, `app/chamados/services/declaracao_texto.py:354`, `app/chamados/services/declaracao_texto.py:372`, `app/chamados/services/declaracao_texto.py:658`, `app/chamados/services/fio.py:1963`. |
| Par e plantio | A irmã vem de consulta nova sem prefetch. As duas guardas de `par_do_intervalo` compartilham uma leitura e `_validar_o_par` consulta a irmã novamente. No materializador, a guarda usa outro `get` depois de `travar_o_ato_do_plantio`, portanto lê as decisões dessa nova instância. `app/chamados/services/materializacao.py:139`, `app/chamados/services/materializacao.py:195`, `app/chamados/services/materializacao.py:2123`, `app/chamados/services/validacao.py:633`, `app/chamados/services/validacao.py:641`. |
| Lote | A guarda de entrada usa o prefetch de **todas** as refs feito antes do laço. Se uma validação for desfeita por outra instância nesse intervalo, uma ref ainda marcada como validada é pulada sem releitura. Uma irmã também pode ser assinada pelo processamento de uma ref anterior. A guarda pós-ação usa refresh integral e consulta o ato de novo. `app/chamados/services/validacao.py:641`, `app/chamados/services/validacao.py:1059`, `app/chamados/services/validacao.py:1067`, `app/chamados/services/validacao.py:1088`. |
| Validar, recusar e corrigir individualmente | As guardas usam instâncias novas, sem prefetch, **antes** da respectiva trava. A decisão não é repetida sob trava. Outra transação pode validar nesse intervalo e a operação iniciada antes ainda assinar outra vez, recusar ou sobrescrever a resposta. A janela já existia com a coluna. A C8 muda quais estados chegam a ela. `app/chamados/services/validacao.py:678`, `app/chamados/services/validacao.py:737`, `app/chamados/services/validacao.py:896`, `app/chamados/services/validacao.py:923`, `app/chamados/services/acoes_disputa.py:345`, `app/chamados/services/disputa_emissao.py:1517`. |

`registrar` invalida o cache apenas da instância recebida na linha. Refresh parcial conserva `decisoes`. Há ainda um caminho preexistente no reconciliador: ele faz prefetch e julga as absolvíveis, adquire a trava dentro do laço e passa **a mesma instância** para `carimbar`, que consulta C6. Se o chamador não tinha a trava desde a carga, um ato concorrente pode estar fora daquele cache. `app/chamados/models.py:4333`, `app/chamados/models.py:4381`, `app/chamados/models.py:3358`, `app/chamados/reconciliador.py:635`, `app/chamados/reconciliador.py:702`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:371`.

## 5. Custo e REDs 3×6 — BLOQUEIA

- **Consultas por item novas.

[truncado em 20000 de 31218 caracteres]

## 20/11 04:00 · sessao a8398df51329 · - · - · origem=Ronald (tmux)

1. **BLOQUEIA.** A C8 mudou as portas para o ato, mas o materializador, o vigia e o reparo ainda selecionam pela coluna. O modal e o cartão também decidem se exibem ações pela coluna. Uma pergunta com ato sem validação e coluna preenchida pode aparecer como «Validada» e perder os botões mesmo quando a porta aceitaria a operação. [Materializador](app/chamados/services/materializacao.py:2111), [vigia](app/chamados/services/vigia_carimbo.py:97), [reparo](app/chamados/services/passivo_validado.py:327), [modal](app/templates/chamados/partials/modal_fio.html:228), [cartão](app/templates/chamados/partials/_pergunta_card.html:42).

2. **BLOQUEIA.** Com ato validado, coluna nula, resposta válida, `materializada_em` nulo e sem lastro, a pergunta pode ficar sem caminho ordinário para aplicar a validação vigente. Validar, recusar, corrigir e declarar passam a vetar, S143 exclui, o materializador e o vigia deixam a pergunta fora e o C7 pode fechar a disputa sem plantio. A reabertura explícita é possível em casos elegíveis e reinicia a pergunta. No inverso, o materializador normal pula após a trava, mas o reparo do passivo ainda chama a unidade por força da seleção do vigia. [Validação](app/chamados/services/validacao.py:687), [recusa](app/chamados/services/validacao.py:901), [correção](app/chamados/services/acoes_disputa.py:349), [declaração](app/chamados/services/declaracao_texto.py:380), [S143](app/chamados/services/disputa_emissao.py:2084), [C7](app/chamados/models.py:2023), [reabertura](app/chamados/juizes.py:1762), [reparo sob trava](app/chamados/services/passivo_validado.py:529).

3. **BLOQUEIA no estado divergente.** O fluxo normal consulta o ato antes, carimba coluna e ato juntos e só então usa a coluna para limpar. Uma pergunta com ato validado e coluna nula nem entra nessa limpeza pelo caminho normal. Chamada diretamente após recusa, a função pode reabrir a disputa sem lançar `DEVOLUCAO`, deixando a validação vigente. No sentido inverso, ela limpa a coluna e lança `DEVOLUCAO` sem validação vigente. O RED de devolução aceita esse comportamento. [Assinatura](app/chamados/services/validacao.py:168), [chamada](app/chamados/services/validacao.py:771), [devolução](app/chamados/services/validacao.py:262), [RED](app/chamados/tests/test_e4c_f5_voltaD_portas.py:382).

4. **BLOQUEIA.** Materializador relê em uma instância nova após a trava, a irmã é consultada de novo, S143 carrega após a trava do colaborador e o lote faz refresh integral após a ação. A pré-guarda do lote usa o prefetch inicial e pode decidir com ato obsoleto. A guarda inicial de validar e recusar roda antes da trava e o estado não é revalidado antes da escrita. O reconciliador também carrega decisões antes da trava em `perguntas_absolvidas_pela_celula` e chama `carimbar` com aquela mesma instância sob a trava. [Materializador](app/chamados/services/materializacao.py:2123), [irmã](app/chamados/services/materializacao.py:146), [S143](app/chamados/services/disputa_emissao.py:1914), [lote](app/chamados/services/validacao.py:1059), [refresh](app/chamados/services/validacao.py:1088), [validar](app/chamados/services/validacao.py:737), [recusar](app/chamados/services/validacao.py:923), [reconciliador](app/chamados/reconciliador.py:635), [prefetch](app/chamados/reconciliador.py:702), [reuso do cache](app/chamados/models.py:4333).

8. **BLOQUEIA.** Para uma metade de intervalo de competência entregue com ato sem validação e coluna preenchida, a correção antes retornava `info`. Agora grava nova resposta e seu ato antes de `validar_pergunta` encontrar K6. A recusa também passa a gravar `RECUSA` e apagar resposta e carimbos, e o guardião de escrita só consulta K6 dentro de ato cercado. A ocorrência e a contagem em produção são [nao verificado]. [Correção](app/chamados/services/acoes_disputa.py:349), [escrita da resposta](app/chamados/services/disputa_emissao.py:1517), [K6](app/chamados/services/validacao.py:742), [recusa](app/chamados/services/validacao.py:900), [limpeza](app/chamados/models.py:3509), [cerca](app/chamados/services/fio.py:1394).

## 20/11 04:00 · sessao 9f7c87b58659 · - · - · origem=Ronald (tmux)

**6. BLOQUEIA.** Os 14 mutantes têm testes associados. Verifiquei em memória que cada âncora tem uma ocorrência e que as 14 substituições compilam. As mortes na suíte são [nao verificado]. Os REDs de corrigir, declarar, aplicar, fechar, validar, recusar e lote combinam retorno e estado persistido. Os dois REDs da irmã demonstram a decisão da guarda por tupla e motivo. O RED do materializador injeta a divergência depois da seleção SQL e substitui o plantio. O do fecho substitui C6 e `pode_fechar`. O da devolução consagra a leitura por coluna, sem mutante e sem percorrer `validar_pergunta`. `app/chamados/tests/e4c_f5_voltaD_mutantes.py:13–65`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:71–149`, `:207–331`, `:345–392`.

A catraca deixa passar `ler = getattr; if ler(perg, "validada_em"): plantar(perg)` e uma chamada a um auxiliar de módulo que leia a coluna. Também considera `Outro.composta(perg).validada_em` autorizada pelo nome do método. Verifiquei em memória que `if perg.validada_em: plantar(perg)` seguido por uma atribuição independente a `perg.validada_em` passa. O critério de escritor cobre o corpo inteiro e confere o receptor, sem verificar se a decisão é de limpeza. No código real a isenção já cobre a condição que decide gravar um ato `DEVOLUCAO`. `app/chamados/tests/test_e4c_f5_selo_catraca.py:37–39`, `:70–79`, `:94–105`, `:265–280`, `:298–307`, `app/chamados/services/validacao.py:270–283`.

**7. BLOQUEIA.** O delta usa o compositor e os juízes existentes e conserva as portas e leituras de relógio. A mudança amplia a dependência de fotos anteriores à trava em validar, corrigir e recusar. A coluna e o ato continuam decidindo a mesma validação em etapas diferentes da materialização. A alteração do harness é textual em `retry_reativa`, com o mesmo alvo, a mesma mutação `if False` e o mesmo teste. Não há remoção de asserção ou alteração de esperado existente no delta anexado. `app/chamados/models.py:4354–4375`, `app/chamados/services/validacao.py:683–689`, `:731–745`, `:900–929`, `app/chamados/services/acoes_disputa.py:345–349`, `:374–387`, `app/chamados/services/materializacao.py:2111–2133`, `app/chamados/tests/bug_par_mutantes.py:68–71`. A história de cada commit é [nao verificado].

**8. BLOQUEIA.** Com ato sem validação e carimbo antigo preenchido, a correção de uma metade de intervalo entregue passa pela guarda nova, grava resposta e limpa o carimbo, e só depois recebe a recusa K6 em `validar_pergunta`. A ação não desfaz a correção quando a validação recusa. A recusa pela supervisão também passa a escrever nesse estado e reabre a pergunta sem cerca de competência no caminho direto. As cascas HTTP verificam o gestor, a disputa aberta e a âncora do chamado. O volume de casos entregues é [nao verificado]. `app/chamados/views.py:505–536`, `app/chamados/services/acoes_disputa.py:345–349`, `:374–387`, `app/chamados/services/disputa_emissao.py:1480–1534`, `:1617–1658`, `app/chamados/services/validacao.py:737–745`, `:900–929`, `app/chamados/services/materializacao.py:808–830`, `app/chamados/services/fio.py:1394–1409`. A matriz também manda migrar a guarda de devolução. A entrega conserva a coluna nessa guarda e o teste fixa esse comportamento. `app/chamados/services/validacao.py:262–283`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:383–392`.

**Custo para o item 5.** Os 3×6 de fecho e tela usam perguntas distintas e cobram o `SELECT` de decisões por lote. O do lote cobre apenas refs barradas na entrada, portanto não alcança o `SELECT` por item aceito após o refresh. O de propostas aumenta notas de um único fato, portanto cobre memoização desse fato, com o custo por fato distinto de fora. O novo C11 confere PKs julgados, PKs devolvidos e consultas iguais e não nulas, com mutantes distintos para os dois prefetches. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:151–205`, `:333–343`, `app/chamados/services/validacao.py:1081–1097`, `app/chamados/services/declaracao_texto.py:625–639`, `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:24–40`, `:219–240`, `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–11`, `:24–54`.

## 20/11 03:59 · sessao 114cf4388c80 · - · - · origem=Ronald (tmux)

**5. BLOQUEIA.** O S84 acrescenta uma consulta à sequência por pergunta ainda não materializada, depois da trava e do `get` individual. O lote classe A acrescenta uma consulta após cada ação encaminhada, porque o `refresh_from_db()` integral descarta o prefetch. O par acrescenta consulta da irmã recuperada em nova instância, inclusive quando chamado por lista, e `_validar_o_par` volta a consultar a mesma irmã. `app/chamados/services/materializacao.py:139-150,195-205,328-346,2118-2132`, `app/chamados/services/validacao.py:633-642,1059-1069,1081-1094`, `app/chamados/services/explicador.py:131-134,159-172`, `app/chamados/models.py:4328-4333`.

A BATIDA alcança S143 via cartório, reconciliador e `post_save` de chamado. A guarda nova usa o prefetch que já existia, com um lote por disputa, e altera quais perguntas entram no caminho de carimbo. O selo anterior registra uma leitura avulsa no reconciliador no cenário de três chamados que ficam. Os quatro tetos anotados são 234, 298, 146 e 221. O run desta árvore é [nao verificado]. `app/ponto/registro_batida.py:170-180`, `app/ponto/signals.py:131-135,206-215`, `app/ponto/services/cartorio.py:1580,1598,1619`, `app/chamados/reconciliador.py:573-587,1486-1519`, `app/chamados/signals.py:91-117`, `app/chamados/services/disputa_emissao.py:2074-2115`, `app/ponto/tests/test_selo_chokepoint_escrita.py:196-206,220-230,248-253,260-267`.

Os REDs 3×6 de S143, tela e entrada do lote passam por seis perguntas e cobram `[1,1]` contra mutantes que removem o prefetch. S143 usa uma disputa e a entrada manda todos os itens direto para `Ja validada`, sem medir as consultas de pós-ação. Propostas cresce de três para seis followups sobre um único fato, então protege o memo por fato. Os dois REDs novos do C11 cobram os PKs julgados e devolvidos, igualdade e presença de SELECT, com remoção dirigida de K1 ou K2. A unidade da métrica é o SELECT cujo primeiro `FROM` é a tabela de decisões. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:151-205,333-343`, `app/chamados/tests/e4c_f5_voltaD_mutantes.py:40-47`, `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:23-40,219-240`, `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9-11,24-53,57-70`, `app/chamados/tests/test_e4c_f5_opus_models.py:22-32`.

**Achado adicional nos itens 1, 2 e 6.** Uma irmã com ato validado e coluna nula agora pode ser plantada. `_validar_o_par` pula a nova linha de validação, mas o plantio real chama `carimbar(..., resposta='validar')`, que preenche a coluna nula com o instante e autor do plantio. A leitura sintetiza esse carimbo sem linha, que pode sobrepor o instante real da validação anterior. Os REDs de par e materialização substituem a unidade pelo carimbo de identidade, que não toca a validação. `app/chamados/services/materializacao.py:200-205,1738-1750,1981-1992`, `app/chamados/services/validacao.py:633-644`, `app/chamados/models.py:3344-3377,4290-4294,4333-4351`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:244-275,284-291`.

## 20/11 03:51 · sessao aef07fb5f215 · E4C-F5-CONSTROI · construtor · origem=Code

A volta D2 alterou só os três arquivos de teste abaixo. Na C8, `test_producao_sem_leituras_cruas` retornou **`[]`** na conferência AST, sem banco nem runner Django. [test_e4c_f5_selo_catraca.py:298](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:298) [nao verificado]

- **`test_e4c_f5_selo_catraca.py`** — Os dois juízes puros saíram de `FUNCOES`. O selo vizinho verifica a composta no chamador. A regra do escritor aceita a leitura `Load` e o `refresh_from_db` quando há `Store` do mesmo campo, no mesmo receptor e na mesma função. Os sintéticos confirmaram que outro receptor, outro campo e leitura sem escrita continuam acusados. As cinco mutações AST abaixo foram rejeitadas por asserção. [Universo](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:12) [Selo de chamada](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_chamada.py:183) [Regra](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:265) [Sintéticos](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:314)
- **`test_e4c_f5_voltaD_portas.py`** — `_cenario()` usa `nullcontext()` quando chamado sem mutante. `test_devolucao_e_o_escritor_da_coluna` espera `None` e zero `DEVOLUCAO` para ato validado com coluna nula. Para ato sem V com coluna preenchida, espera coluna limpa, `True` e uma `DEVOLUCAO`. A execução com banco fica [nao verificado]. [_cenario](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:24) [Teste](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:383) [Guarda e gravação](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:270) [Retornos](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:295) [nao verificado]
- **`bug_par_mutantes.py`** — Só mudaram as duas strings de `retry_reativa`. A âncora usa `if leitura.validada_em:`, ocorre uma vez em `validar_pergunta` e a substituição por `if False:` passou no parse. A rejeição do mutante pelo teste com banco fica [nao verificado]. [Mutante](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/bug_par_mutantes.py:68) [Guarda atual](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:687) [nao verificado]

**Comandos AST a partir de `app/`**. O bloco chama diretamente as asserções do selo e aplica os mutantes em memória. [SeloCatracaTest](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_catraca.py:297)

```bash
catraca_d2() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B - "$1" <<'PY'
from pathlib import Path
from sys import argv
from unittest import TestCase

p = Path('chamados/tests/test_e4c_f5_selo_catraca.py').resolve()
fonte = p.read_text(encoding='utf-8').replace('from django.test import SimpleTestCase\n', '', 1)

def carregar(src):
    ns = {'__file__': str(p), 'SimpleTestCase': TestCase}
    exec(compile(src, str(p), 'exec'), ns)
    return ns['SeloCatracaTest']()

chave = 'escritas.get(ast.dump(receptor), frozenset())'
escritor = 'test_PASSA_leitura_e_refresh_do_proprio_campo'
producao = 'test_producao_sem_leituras_cruas'
mutantes = {
    'ignorar_receptor': (chave, 'set().union(*escritas.values())',
                         'test_MORDE_escrita_em_outro_receptor'),
    'ignorar_campo': (chave, 'CAMPOS if ast.dump(receptor) in escritas else frozenset()',
                      'test_MORDE_escrita_em_outro_campo'),
    'remover_regra': ('if escritas:', 'if False:', escritor),
    'remover_regra_refresh': (
        'campos.difference(self.campos_do_escritor.get(id(no), ()))', 'campos', escritor),
    'reincluir_juizes': (
        'FUNCOES = {',
        "FUNCOES = {\n    'chamados/juizes.py': ('projecao_temporal_da_pergunta', 'pergunta_respondida'),",
        producao),
}
modo = argv[1]
if modo == 'controle':
    teste = carregar(fonte) [nao verificado]
    for nome in sorted(n for n in dir(teste) if n.startswith('test_')): [nao verificado]
        getattr(teste, nome)() [nao verificado]
        print(nome, '[]' if nome == producao else 'OK')
else:
    antes, depois, alvo = mutantes[modo]
    assert fonte.count(antes) == 1, modo
    teste = carregar(fonte.replace(antes, depois, 1)) [nao verificado]
    try:
        getattr(teste, alvo)() [nao verificado]
    except AssertionError:
        print(modo, 'rejeitado por', alvo)
    else:
        raise AssertionError(f'{modo}: sobreviveu')
PY
}

catraca_d2 controle
catraca_d2 ignorar_receptor
catraca_d2 ignorar_campo
catraca_d2 remover_regra
catraca_d2 remover_regra_refresh
catraca_d2 reincluir_juizes
```

**Comandos para o Code**. Controle da volta D e mutante `retry_reativa` [nao verificado]. [PortasVoltaDTest](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:22) [Runner de mutantes](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/bug_par_mutantes.py:189)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings.ci \
  chamados.tests.test_e4c_f5_voltaD_portas

PYTHONDONTWRITEBYTECODE=1 python3 -B -m chamados.tests.bug_par_mutantes \
  --django --mutante retry_reativa
```

O mutante da devolução troca em memória a guarda da coluna pela composta. O teste isolado deve falhar por asserção [nao verificado]. [Guarda](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:270) [Teste](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:383) [nao verificado]

```bash
DJANGO_SETTINGS_MODULE=config.settings.ci PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
from inspect import getsource
from unittest.mock import patch

import django
django.setup()

from django.core.management import call_command
from chamados.services import validacao

original = validacao.devolver_validacao_recusada
fonte = getsource(original)
antes = '    if perg.validada_em is not None:'
depois = (
    '    from chamados.models import DecisaoPergunta\n'
    '    if DecisaoPergunta.composta(perg).validada_em is not None:'
)
assert fonte.count(antes) == 1
ns = vars(validacao).copy()
exec(compile(fonte.replace(antes, depois, 1), original.__code__.co_filename, 'exec'), ns)
with patch.object(validacao, original.__name__, ns[original.__name__]):
    call_command(
        'test',
        'chamados.tests.test_e4c_f5_voltaD_portas.PortasVoltaDTest.test_devolucao_e_o_escritor_da_coluna',
        interactive=False,
    )
PY
```

**`git status --short` esperado e observado em `app/`**

```text
 M chamados/tests/bug_par_mutantes.py
 M chamados/tests/test_e4c_f5_selo_catraca.py
 M chamados/tests/test_e4c_f5_voltaD_portas.py
```

## 20/11 03:44 · sessao 642381ebabdc · E4C-F5-CONSTROI · construtor · origem=Code

**Volta D parcial.** Migrei 11 das 13 menções. Preservei o `refresh` e a guarda de `devolver_validacao_recusada` pelo bloqueio do item 5. A catraca nova acusa esses dois pontos e três leituras nos dois juízes estruturais. (`chamados/services/validacao.py:262–280`, `chamados/juizes.py:1391`, `chamados/juizes.py:1560`, `chamados/tests/test_e4c_f5_selo_catraca.py:269`)

Compilei em memória os oito arquivos e os 14 mutantes, conferi as âncoras e executei isoladamente os casos sintéticos do selo. REDs de banco, mortes dos mutantes, tetos e impacto em produção ficam **[nao verificado]**. (`chamados/tests/e4c_f5_voltaD_mutantes.py:13`, `chamados/tests/test_e4c_f5_selo_catraca.py:285–322`)

### Sítios

Os caminhos da tabela são relativos a `chamados/`. As linhas à esquerda são da base. Os custos são de leitura das decisões por inspeção, com **0 quando há prefetch e 1 quando a composta precisa ler a relação**. A medição fica **[nao verificado]**. (`chamados/models.py:4321–4334`, `chamados/models.py:4354–4362`)

| Antes → depois | Consultas adicionais | RED em `tests/test_e4c_f5_voltaD_portas.py` | Mutante |
|---|---|---|---|
| `services/acoes_disputa.py:349 → :349` · corrigir usa a composta da pergunta | +1 no `.get()` avulso | `:70` | `corrigir` |
| `services/declaracao_texto.py:228 → :229` · guarda compartilhada usa a composta | +0 com a candidata ou testemunha prefetched. +1 após releitura integral ou se `abrir` devolver instância sem prefetch | `:88`, `:109`, `:165`, `:182` | `declaracao` |
| `services/disputa_emissao.py:2085 → :2085` · seleção S143 usa a composta | +0, prefetch existente em `:2080` | `:136`, `:150` | `fecho`, `fecho_sem_lote` |
| `services/materializacao.py:200 → :201` · plantio exige V da irmã | +1 por irmã encontrada, compartilhada com a guarda seguinte | `:220` | `irma_plantio` |
| `services/materializacao.py:202 → :203` · implausibilidade usa V da mesma irmã | +0 depois da composta em `:200` | `:230` | `irma_implausivel` |
| `services/materializacao.py:2128 → :2130` · usa a composta da instância relida sob trava | +1 por candidata que chega à guarda após o `.get()` de `:2124` | `:244` | `materializar` |
| `services/validacao.py:262 → :262` · refresh parcial preservado | +0 | Diagnóstico em `:370` | — |
| `services/validacao.py:269 → :270` · guarda de devolução preservada | +0 | Diagnóstico em `:382` | — |
| `services/validacao.py:640 → :641` · carimbo do par usa a composta da irmã | +1 por par aceito, pois a irmã chega sem prefetch | `:271` | `validar_par` |
| `services/validacao.py:685 → :687` · reutiliza `leitura` de `:683` | +0 | `:295` | `validar` |
| `services/validacao.py:898 → :901` · antecipa e reutiliza `antes_do_ato` | +0 quando o fluxo já chegava à composta. +1 quando a coluna preenchida antes encerrava a chamada sem consultá-la | `:307` | `recusar` |
| `services/validacao.py:1065 → :1067` · guarda de entrada usa a composta | +1 por lote com perguntas encontradas, +0 por item | `:319`, `:332` | `lote_entrada`, `lote_entrada_sem_prefetch` |
| `services/validacao.py:1086 → :1089` · classificação usa a composta após refresh integral | +1 por item que chamou a ação | `:344` | `lote_releitura` |

### Releituras, lotes e devolução

- **Refresh.** Para o Django pinado, `refresh_from_db(fields=[...])` preserva o prefetch de `decisoes` quando a relação fica fora de `fields`. `refresh_from_db()` integral descarta o prefetch. O diagnóstico exercita as duas formas com um ato posterior à carga. A confirmação no Django da sessão fica **[nao verificado]**. A materialização recebe outra instância pelo `.get()` depois da trava. O descarte explícito de `registrar` alcança a instância passada ao escritor. (`requirements.txt:13`, `chamados/tests/test_e4c_f5_voltaD_portas.py:370–380`, `chamados/services/materializacao.py:2122–2132`, `chamados/models.py:4381–4387`)
- **Declaração em laço.** `_porta_recusaria_por_carimbo` chama `_resposta_validada`. `projecao_por_marco` itera por marco. O fio e a API o chamam por cartão, usando `insumos_em_regularizacao`, que carrega `decisoes` em lote. Sem `por_fato`, a projeção já carrega por marco. `propostas_do_chamado` itera por follow-up e carrega uma vez por fato distinto. Os REDs cobram um SELECT de decisões em 3 e 6 fatos da tela e em 3 e 6 propostas do mesmo fato. O RED do S143 cobra o mesmo com 3 e 6 perguntas. (`chamados/services/declaracao_texto.py:280–287`, `:470–480`, `:585–592`, `:626–639`, `chamados/services/fio.py:415–425`, `api/views.py:1088–1094`, `chamados/tests/test_e4c_f5_voltaD_portas.py:150–206`)
- **Origem da irmã.** Plantio, tela e validação entram por `par_do_intervalo`. A própria função busca a irmã em `metade_irma_do_intervalo`, num queryset sem prefetch, por disputa, motivo complementar e dia. O plantio passa `irma_validada=True`. A validação recebe de volta essa mesma instância. (`chamados/services/materializacao.py:139–150`, `:195–203`, `:925–930`, `:1737–1740`, `chamados/services/validacao.py:632–643`)
- **Bloqueio da devolução.** Com ato validado e coluna nula, a troca gravaria `DEVOLUCAO`; a foto ainda poderia ficar igual e o retorno ser `None`. Com ato sem V e coluna preenchida, a troca deixaria `validada_em` e o autor residuais, onde hoje a porta os limpa. Isso piora a projeção. A fila do materializador e o vigia selecionam pela coluna preenchida, e os recortes de absolvíveis e órfãs do reconciliador exigem coluna nula. Por isso preservei `:262` e `:270`, sem acrescentar descarte de cache a essa porta. (`chamados/services/validacao.py:262–304`, `chamados/services/materializacao.py:2111–2115`, `chamados/services/vigia_carimbo.py:97–105`, `chamados/reconciliador.py:678–682`, `:1096–1103`)
- **Batida.** O fecho alcança S143 pelo `post_save` do chamado. A guarda alterada nesse caminho usa `_todas`, já prefetched. Os quatro tetos da cópia são **234 / 298** para um e três furos que fecham e **146 / 221** para um e três que ficam. O delta executado fica **[nao verificado]**. (`ponto/signals.py:131–135`, `chamados/models.py:308`, `chamados/signals.py:93–117`, `chamados/services/disputa_emissao.py:2080–2085`, `ponto/tests/test_selo_chokepoint_escrita.py:206`, `:230`, `:253`, `:267`)

### Selo e testes existentes [nao verificado]

- A varredura literal ficou vermelha na devolução e nos parâmetros estruturais de `projecao_temporal_da_pergunta` e `pergunta_respondida`. A regra local exige ancestralidade de `.composta()` ou `.leitura()` dentro do corpo. Os juízes recebem a leitura dos chamadores, e o placar entrega a estrutura da face SQL ao segundo juiz. Mantive o universo pedido e o selo anterior. (`chamados/tests/test_e4c_f5_selo_catraca.py:14–28`, `:269–279`, `chamados/juizes.py:1391`, `:1408–1409`, `:1560`, `chamados/services/raiox.py:135–147`, `chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:210–251`)
- **K47** espera carimbo justamente para ato validado com projeção sem V. A expectativa de carimbo conflita com a guarda nova. O mutante existente **`retry_reativa`** também precisa de nova âncora, pois busca literalmente `if perg.validada_em:` em `validar_pergunta`. As duas pendências estão nos arquivos existentes. (`chamados/tests/test_e4c_f5_voltaB_lotes.py:197–216`, `chamados/tests/bug_par_mutantes.py:68–71`, `chamados/services/validacao.py:683–689`)

### Comandos para a sessão

Labels dos REDs, da catraca e dos quatro tetos. A catraca de produção acusa os pontos acima. (`chamados/tests/test_e4c_f5_voltaD_portas.py:23`, `chamados/tests/test_e4c_f5_selo_catraca.py:268–279`, `ponto/tests/test_selo_chokepoint_escrita.py:467–509`)

```bash
export PYTHONDONTWRITEBYTECODE=1

python3 manage.py test chamados.tests.test_e4c_f5_voltaD_portas
python3 manage.py test chamados.tests.test_e4c_f5_selo_catraca

teto=ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest
python3 manage.py test \
  "$teto.test_registrar_batida_com_1_furo_vivo_que_ela_fecha" \
  "$teto.test_registrar_batida_com_3_furos_vivos_que_ela_fecha" \
  "$teto.test_registrar_batida_com_1_furo_vivo_que_fica" \
  "$teto.test_registrar_batida_com_3_furos_vivos_que_ficam"
```

Cada comando abaixo ativa um mutante e executa o teste que deve falhar na asserção de negócio ou de custo. (`chamados/tests/e4c_f5_voltaD_mutantes.py:13–69`, `chamados/tests/test_e4c_f5_voltaD_portas.py:70–368`)

```bash
m() {
  F5_VOLTA_D_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    "chamados.tests.test_e4c_f5_voltaD_portas.PortasVoltaDTest.test_RED_$2"
}

m corrigir corrigir
m declaracao declaracao
m fecho fecho
m irma_plantio irma_plantio
m irma_implausivel irma_implausivel
m materializar materializar_ato_mudou_sob_trava
m validar_par validar_par
m validar validar
m recusar recusar
m lote_entrada lote_entrada
m lote_releitura lote_rele_ato_escrito_em_outra_instancia
m fecho_sem_lote fecho_3x6
m tela_sem_lote tela_3x6
m lote_entrada_sem_prefetch lote_entrada_3x6
```

`git status --porcelain` observado, com os caminhos relativos à raiz Git.

```text
 M app/chamados/services/acoes_disputa.py
 M app/chamados/services/declaracao_texto.py
 M app/chamados/services/disputa_emissao.py
 M app/chamados/services/materializacao.py
 M app/chamados/services/validacao.py
?? app/chamados/tests/e4c_f5_voltaD_mutantes.py
?? app/chamados/tests/test_e4c_f5_selo_catraca.py
?? app/chamados/tests/test_e4c_f5_voltaD_portas.py
```

_7485 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
