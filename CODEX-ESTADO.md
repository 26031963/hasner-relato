# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 17:48** · publicado em **18/11/2026 17:48** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 17:48 · sessao 5fea559ba4a8 · E4C-F6F7 · construtor · origem=Code

A ligação do contador ao vigia está implementada em [vigiar_fio_morto.py](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/vigiar_fio_morto.py:186): uma chamada por rodada, com o recorte por empresa usado pela irmã ([vigia_fio.py:770](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/vigia_fio.py:770)). O valor vai à linha impressa ([linha 216](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/vigiar_fio_morto.py:216)) e ao censo do carimbo ([linha 262](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/vigiar_fio_morto.py:262)), com esperado zero e sem alarme ([linha 101](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/vigiar_fio_morto.py:101)).

O [teste novo](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:38) cobre ausência e presença da linha, impressão, esperado, alarme, recorte e chamada única. **RED, GREEN e mutantes não foram executados**, pois usam banco; o resultado de cada rodada permanece [não verificado]. O `py_compile` passou com a escrita de bytecode interceptada; Ruff não está disponível [não verificado em outros ambientes]. [nao verificado]

Comando para o Code executar o RED sobre a base com o teste novo e depois o GREEN com a cura: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 manage.py test chamados.tests.test_f7_vigia_validada_sem_linha
```

Mutantes propostos, cada um aplicado isoladamente a [vigiar_fio_morto.py](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/vigiar_fio_morto.py:186), seguidos pelo mesmo comando acima e pela restauração da alteração; morte [não verificada]: [nao verificado]

- Gravar `validada_projetada_sem_linha=0` no censo: o caso sem linha deve falhar ([teste:38](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:38)). [nao verificado]
- Remover o filtro `perguntas.filter(...)`: o caso do recorte deve falhar ([teste:55](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:55)). [nao verificado]
- Chamar o contador duas vezes: a verificação de chamada única deve falhar ([teste:59](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:59)). [nao verificado]

Há uma incompatibilidade previsível para a suíte: o teste existente compara os dicionários completos do carimbo sem a nova chave ([test_o261_vigia_fio.py:835](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_o261_vigia_fio.py:835), [linha 859](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_o261_vigia_fio.py:859)). Não foi alterado por estar fora do escopo literal; falha em execução [não verificado]. [nao verificado]

LEI-AKITA: origem=vigiar_fio_morto.py::handle ([linha 160](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/vigiar_fio_morto.py:160)), testemunha=contador_validada_projetada_sem_linha, que lê DecisaoPergunta.linhas_da_projecao_sem_linha ([backfill:94](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/management/commands/backfill_decisoes_pergunta.py:94)), RED=[teste:38](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:38) [não executado], quem-mais-le=bin/fio_estado.py ([linha 98](/home/ronald/wt-codex-e4c-f6f7-174627/bin/fio_estado.py:98)) e bin/gerar_estado.py ([linha 260](/home/ronald/wt-codex-e4c-f6f7-174627/bin/gerar_estado.py:260)), juizes novos=0, padrao=Single Source of Truth ([GLOSSARIO.md:11](/home/ronald/wt-codex-e4c-f6f7-174627/app/docs/GLOSSARIO.md:11)).

## 18/11 17:47 · sessao 125392a09247 · DECLARADO-X-GRAVADO · censo · origem=Code

1. **Não há `save()` ou `update()` posterior na cauda de `validar_pergunta` que regrave esses campos.** A validação é carimbada antes da materialização; depois da chamada da unidade há uma possível devolução ao admin e, em seguida, a varredura da disputa. Ao voltar da varredura, o código faz `refresh_from_db` de `validada_em` e `materializada_em`, registra logs e retorna. Portanto, nessa cauda, um save posterior não desfaria uma reabertura. `estado` é uma propriedade calculada a partir da sequência de decisões e dos carimbos, não um campo salvo ali. A análise não comprova o efeito de callbacks ou outros caminhos indiretos: **[não verificado]**. [validacao.py:743](app/chamados/services/validacao.py:743), [validacao.py:769](app/chamados/services/validacao.py:769), [validacao.py:786](app/chamados/services/validacao.py:786), [validacao.py:805](app/chamados/services/validacao.py:805), [validacao.py:827](app/chamados/services/validacao.py:827), [models.py:3225](app/chamados/models.py:3225)

2. **`reabrir_para_correcao` aceita pergunta já validada**, desde que esteja no queryset fornecido e não seja linha fora do fato; a própria trilha contempla uma validação anterior. Ela limpa `respondida_em`, `validada_em`, `validada_por` e `resposta_colab`; a porta de lote também limpa `materializada_em`, `materializada_por` e `via_resolucao`. Não grava `estado`, que é calculado. [disputa_emissao.py:1617](app/chamados/services/disputa_emissao.py:1617), [disputa_emissao.py:1639](app/chamados/services/disputa_emissao.py:1639), [disputa_emissao.py:1653](app/chamados/services/disputa_emissao.py:1653), [models.py:3448](app/chamados/models.py:3448), [models.py:3225](app/chamados/models.py:3225)

   Para cada linha elegível da qual retira resposta, validação ou carimbo, grava uma `DecisaoPergunta`: por padrão, ato `reabertura`, com a resposta e os carimbos anteriores, instante, autor e `motivo` truncado a 240 caracteres em **`ato_motivo`**. Uma pergunta já vazia não produz linha. A função não copia esse argumento para `PerguntaDisputa.recusa_motivo`; a porta de recusa faz essa escrita separadamente e passa `ato='recusa'`. [disputa_emissao.py:1618](app/chamados/services/disputa_emissao.py:1618), [disputa_emissao.py:1639](app/chamados/services/disputa_emissao.py:1639), [validacao.py:916](app/chamados/services/validacao.py:916), [validacao.py:920](app/chamados/services/validacao.py:920)

3. **O modal do admin lê o motivo da trilha:** `respostas_anteriores` devolve as decisões com foto, e o modal exibe `ato_motivo` quando preenchido. Ele também pode mostrar `recusa_motivo` em bloco de pendência do admin. [models.py:3793](app/chamados/models.py:3793), [modal_fio.html:204](app/templates/chamados/partials/modal_fio.html:204), [modal_fio.html:222](app/templates/chamados/partials/modal_fio.html:222)

   `consulta_do_app` seleciona e particiona perguntas; a API serializa `motivo` **da pergunta**, mas não inclui `ato_motivo` da reabertura nem `recusa_motivo` no item. Assim, essa representação não entrega o texto passado à reabertura. [pergunta_no_app.py:45](app/chamados/services/pergunta_no_app.py:45), [pergunta_no_app.py:68](app/chamados/services/pergunta_no_app.py:68), [views.py:2549](app/api/views.py:2549)

   A tela web do colaborador pode exibir `recusa_motivo` em condições específicas para uma pergunta apresentada no ramo de encerrada/validada; isso **não demonstra** que exiba `ato_motivo` quando a pergunta volta pendente. Na recusa, o push é genérico; a porta também cria uma nota no chamado com o motivo, mas a visualização dessa nota pelo colaborador neste caso está **[não verificado]**. Quanto ao aplicativo instalado e à entrega efetiva do texto: **[não verificado]**. [ _card_atendimento.html:179](app/templates/chamados/partials/_card_atendimento.html:179), [validacao.py:927](app/chamados/services/validacao.py:927), [validacao.py:943](app/chamados/services/validacao.py:943)

4. `recusar_pergunta` devolve ao colaborador uma resposta recusada, mas retorna `ja_validada` antes da reabertura se `validada_em` estiver preenchido; por isso não atende à resposta já validada. [validacao.py:898](app/chamados/services/validacao.py:898), [validacao.py:925](app/chamados/services/validacao.py:925)

   `devolver_validacao_recusada` aceita o caso de resposta já validada recusada pela materialização, mas devolve **ao admin**, preservando a resposta; exige resposta declarada de tipo `hora`, ausência de materialização e motivo de recusa. Não cobre `data_hora` por esse predicado e não devolve a resposta ao colaborador. [validacao.py:185](app/chamados/services/validacao.py:185), [validacao.py:237](app/chamados/services/validacao.py:237), [validacao.py:262](app/chamados/services/validacao.py:262)

   A porta de baixo nível `reabrir_para_correcao` consegue limpar uma pergunta validada, mas, isoladamente, guarda o motivo na trilha e a API examinada não o fornece ao colaborador; a existência de outra porta completa para os dois ramos está **[não verificado]**. [disputa_emissao.py:1584](app/chamados/services/disputa_emissao.py:1584), [views.py:2549](app/api/views.py:2549)

5. **frase literal: nao escrita** nos cinco arquivos indicados, conforme a busca pedida pelos termos e pela identificação da obra. O texto fornecido relata a exigência de voltar ao colaborador “com a frase”, mas não fornece a frase literal. A formulação exigida pela lei permanece **[não verificado]**. [LEIS.md](app/docs/LEIS.md), [CORTES.json](app/docs/CORTES.json), [CORTES.md](app/docs/CORTES.md), [DOSSIES.md](app/docs/DOSSIES.md), [RELATO-ARQUIVO.md](app/docs/RELATO-ARQUIVO.md)

6. A unidade que processa `hora`, `data_hora` e `escolha` é `_materializar_uma`; os chamadores de produção encontrados são:

   - Validação corrente e par: [validacao.py:642](app/chamados/services/validacao.py:642), [validacao.py:775](app/chamados/services/validacao.py:775).
   - Irmã do intervalo já validada: [materializacao.py:1724](app/chamados/services/materializacao.py:1724), [materializacao.py:1744](app/chamados/services/materializacao.py:1744).
   - Varredura da disputa: [materializacao.py:2104](app/chamados/services/materializacao.py:2104), [materializacao.py:2125](app/chamados/services/materializacao.py:2125).
   - Lote do passivo: [passivo_validado.py:529](app/chamados/services/passivo_validado.py:529), [passivo_validado.py:537](app/chamados/services/passivo_validado.py:537).

   A varredura é chamada também pelo fluxo do admin, pela ação de disputa e pelo comando de backfill: [fio.py:718](app/chamados/services/fio.py:718), [acoes_disputa.py:143](app/chamados/services/acoes_disputa.py:143), [backfill_fio_morto_e4b.py:147](app/chamados/management/commands/backfill_fio_morto_e4b.py:147); além da validação, [validacao.py:786](app/chamados/services/validacao.py:786). A varredura filtra `validada_em` preenchido e confere novamente antes da unidade; a irmã puxada pelo materializador é exigida validada. O estado efetivo de todas as perguntas no lote do passivo está **[não verificado]**. [materializacao.py:2104](app/chamados/services/materializacao.py:2104), [materializacao.py:2123](app/chamados/services/materializacao.py:2123), [materializacao.py:1724](app/chamados/services/materializacao.py:1724)

   Há ainda **outra materialização**, `materializar_pendentes_aprovadas`, chamada pelo signal de fechamento da disputa. Seu filtro não exige `validada_em`; ela consulta o juiz e tenta carimbar perguntas aprovadas, sem fabricar batida. Portanto há caminho de materialização de pendente que pode começar sem validação, mas ele é distinto da unidade dos ramos `hora`/`data_hora`. [signals.py:149](app/chamados/signals.py:149), [signals.py:165](app/chamados/signals.py:165), [materializacao.py:2139](app/chamados/services/materializacao.py:2139), [materializacao.py:2167](app/chamados/services/materializacao.py:2167)

A contagem abaixo considera uma questão de lei: o texto literal da frase exigida. [não verificado]

perguntas de lei: 1

## 18/11 17:46 · sessao 45232c15941a · REPROVAS-1811-1740 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 2:** as perguntas **1 e 3 PROCEDEM**. A cópia registra uma ponta F6F7 com suite c10 verde, mas não comprova a c12 nem define o resultado necessário para abrir a F9. [app/docs/RELATO.md:41](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:41) [não verificado]

1. **Premissas — PROCEDE.** O brief da F9 foi registrado como refeito contra o main `d6da98509`; o censo originalmente tinha uma pergunta de lei, depois respondida com “perguntas de lei 0”. Os números 24/68 do brief, porém, são registrados como não remedidos. [app/docs/RELATO.md:45](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:45) [app/docs/RELATO.md:42](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:42) [app/docs/RELATO.md:35](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:35)

   A F9 depende de F1/F2 na ponta `e78031f79`; nessa árvore há registro de **suite c10** verde, e o opus final estava aberto. Isso não comprova a existência, o hash ou o estado verde da ponta **c12** que o item condiciona à abertura. [app/docs/RELATO.md:32](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:32) [app/docs/RELATO.md:41](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:41) [app/docs/RELATO.md:4](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:4) [não verificado]

2. **Colisão com o aval vigente — CONFERE, com guardas cumulativas.** A decisão das 16:13 efetivamente manda a F9 nascer sobre a ponta **pousada**; o anexo fornecido declara substituí-la somente nesse ponto. O despacho POUSOS-COMBOIO registra F9 e F6F7 após as 18h; a vedação de ESPURIAS-ABRE proíbe, até o fim da simulação, pousos que toquem folha, export, retenção ou DiaPago. O anexo determina F6F7 após a simulação e F9 depois dela. [app/docs/RELATO.md:32](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:32) [app/docs/RELATO.md:65](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:65) [app/docs/CORTES.json:2019](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.json:2019) [anexo fornecido; arquivo na cópia não verificado]

   RAIA-INTERVALO registra a vez de DECLARADO-X-GRAVADO e manda medir arquivo comum com raia viva na abertura. O teto geral permite dois construtores **sem arquivo em comum**; além dele, o wrapper recusa uma segunda chamada de **Codex construtor** enquanto a vaga única estiver ocupada. Ocupação e cruzamento na abertura da F9: [não verificado]. [app/docs/RELATO.md:8](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:8) [app/docs/RELATO.md:15](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:15) [app/docs/CORTES.json:1349](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.json:1349) [bin/codex_pergunta.sh:253](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/codex_pergunta.sh:253)

3. **Condições no literal — PROCEDE.** “C12 GREEN” não identifica no dado fornecido qual recorte ou suite deve estar verde, nem aponta resultado ou hash da c12. O registro disponível é a suite inteira **c10**, `Ran 16885`, sem `rc` gravado em arquivo. [app/docs/RELATO.md:41](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:41) [não verificado]

   “Codex verificador dela sem bloqueio” também não aponta laudo da c12 nem esclarece o tratamento de achados procedentes triados ou pendentes. O registro de verificador com zero bloqueantes é de volta anterior; a recusa do wrapper por B3 parado ou estado ilegível é outra guarda e não prova um laudo sem bloqueantes. [app/docs/RELATO.md:111](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:111) [bin/codex_pergunta.sh:393](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/codex_pergunta.sh:393) [não verificado]

4. **Exportada, dinheiro ou `!` fora do declarado — CONFERE quanto ao texto da abertura; efeitos da obra [não verificado].** O item fornecido autoriza abertura em cópia e posterga o pouso; não declara apply ou alteração em produção. A conferência histórica da F6F7 registrou zero arquivos de código em folha, export, retenção e DiaPago, mas ela não demonstra o conteúdo da futura c12 nem os efeitos da F9. [app/docs/RELATO.md:43](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:43) [anexo fornecido; arquivo na cópia não verificado] [não verificado]

A cópia disponibilizada não contém metadados Git para confirmar independentemente o hash informado como HEAD. [não verificado]

## 18/11 17:45 · sessao f71e2d7a1389 · - · - · origem=Ronald (tmux)

- O wrapper aplica o limite de **um Codex construtor por vez** por `flock` no repositório vivo; vaga ocupada causa recusa com `rc 2`. Ele também exige censo aprovado antes da vaga e que o `HEAD` da cópia corresponda ao hash informado. [bin/codex_pergunta.sh:241](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/codex_pergunta.sh:241) [bin/codex_pergunta.sh:248](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/codex_pergunta.sh:248)

- A evidência registrada é de **suite inteira verde da F6F7 c10**, na árvore `e78031f79`, com `Ran 16885` e sem `rc` gravado; não demonstra uma cura c12 verde. [app/docs/RELATO.md:41](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:41) O teste, o hash e o resultado de “c12 GREEN” são [não verificado].

- O papel verificador é de leitura; o wrapper também recusa sua volta se a obra estiver parada pelo B3 ou se o estado/trilha for ilegível. Isso não comprova o resultado de uma revisão da c12 nem define, por si só, um laudo sem bloqueantes. [bin/codex_pergunta.sh:2](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/codex_pergunta.sh:2) [bin/codex_pergunta.sh:393](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/codex_pergunta.sh:393)

- A conferência histórica da F6F7 registra zero arquivos de código em folha, exportação, retenção e DiaPago; isso não comprova o conteúdo futuro da c12 nem o efeito da F9. [app/docs/RELATO.md:43](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:43) Efeitos futuros sobre exportada, dinheiro ou `!`: [não verificado].

## 18/11 17:45 · sessao b8d10940a2d4 · - · - · origem=Ronald (tmux)

- A decisão das 16:13 diz que a F9 nasce sobre a ponta **pousada**, depois da F6F7; o item 2 do anexo declara substituir essa decisão somente nesse ponto. A divergência é expressa pelo próprio anexo. [app/docs/RELATO.md:32](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:32); anexo `aval.txt` fornecido pelo usuário: [não verificado em arquivo da cópia].

- A vedação de véspera proíbe pousos que toquem folha, export, retenção ou DiaPago desde 18/11 às 18h até o fim da simulação de 19/11. O item 1 do anexo determina que a F6F7 pouse depois da simulação; o item 2 determina que a F9 pouse depois dela. [app/docs/CORTES.json:2019](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.json:2019); anexo: [não verificado em arquivo da cópia].

- O despacho de POUSOS-COMBOIO registra ESCALA, F6F7, F9 e EVAL após 18h, com F9 e F6F7 antes; ele não registra exceção à vedação de véspera. [app/docs/RELATO.md:65](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:65); [app/docs/CORTES.json:2019](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.json:2019).

- A RAIA-INTERVALO registra a vez da **(b) DECLARADO-X-GRAVADO**, seguida pela (a), e determina medir arquivo comum com raia viva na abertura. O item 2 do anexo prevê a F9 como próxima a entrar caso a (b) ocupe a segunda vaga. A disponibilidade e o cruzamento concretos na abertura são [não verificado]. [app/docs/RELATO.md:8](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:8); [app/docs/RELATO.md:15](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:15).

- O teto de dois construtores não elimina, por si só, a condição de não cruzar arquivo: o registro vigente admite dois construtores **sem arquivo em comum**. [app/docs/CORTES.json:1349](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.json:1349); [app/docs/CORTES.json:1350](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.json:1350).

## 18/11 17:45 · sessao ea0e7d131448 · - · - · origem=Ronald (tmux)

A cópia registra o brief da F9 refeito contra o main e a resposta à pergunta de lei, com “perguntas de lei 0”, em [RELATO.md:45](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:45) e [RELATO.md:42](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:42). A revisão registra que os números 24/68 do brief não foram remedidos em [RELATO.md:35](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:35).

A ponta `e78031f79` da F6F7 tem suite **c10** verde registrada, e a F9 depende de F1/F2 nessa ponta, conforme [RELATO.md:41](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:41) e [RELATO.md:32](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:32). Uma ponta **c12 GREEN** e o resultado do verificador dessa c12: **[não verificado]**. O registro de verificador sem bloqueante em [RELATO.md:111](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/RELATO.md:111) refere-se a uma volta anterior, não à c12.

## 18/11 17:39 · sessao 8dcf99a94415 · RAIA-INTERVALO · leitura · origem=Code

A sonda abaixo **não foi executada**. A ata está em `CelulaDia.ata`, mas não encontrei um carimbo que registre todas as alterações dela: `veredito_em` não avança quando a ata muda sob decisão administrativa, e `carimbar_precedencia` também altera a ata sem atualizar esse campo. Por isso, a sonda imprime `UA=?` e, pela regra solicitada para dado faltante, `VELHO=?`; não seria correto usar `editada_em`, que carimba edição do dia/DNA ([app/escala/models.py:1732](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1732), [app/escala/models.py:1778](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1778), [app/ponto/portas/celula.py:1060](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1060), [app/ponto/portas/celula.py:1205](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1205)).

`HOJE` é o veredito derivado dos códigos devolvidos pela chamada do **command**, não uma previsão de escrita do cartório: este último pode preservar uma decisão administrativa e tem outros insumos próprios ([app/escala/models.py:1624](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1624), [app/ponto/portas/celula.py:970](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:970), [app/ponto/management/commands/supra_juiz.py:239](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:239)). O leitor da grade lê a ata lavrada; portanto, `HOJE` não prova uma reconstrução independente da ata ([app/escala/services/leitor_celula.py:350](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/leitor_celula.py:350), [app/escala/services/leitor_celula.py:377](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/leitor_celula.py:377)).

A classificação é declarada pura; a transação PostgreSQL `READ ONLY` protege também os carregadores usados pela sonda, recusando escritas ([app/ponto/supra_juiz.py:9](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:9)). A sonda exige o schema ativo `juliani`; o uso em qualquer outro ambiente fica recusado [não verificado].

```python
import datetime as dt
from collections import Counter

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import CelulaDia, EscalaColaborador, derivar_veredito
from escala.services.leitor_celula import grade_da_celula
from ponto.models import Batida
from ponto.turnos import ausencia_cobre, batidas_apuraveis, dias_encerrados
from ponto.supra_juiz import cadastro_do_dia, classificar_dia
from chamados.models import ChamadoColaborador
from chamados.juizes import fio_vivo
from chamados.catalogo.furo_modulos import FURO_MODULES
from chamados.catalogo.modulos import data_do_chamado
from chamados.catalogo.motor import VIVOS

G = {'G1': [(622, d) for d in ('27/10', '29/10', '04/11', '05/11', '12/11', '13/11', '16/11')],
     'G2': [(362, '22/10'), (945, '29/10'), (735, '16/11')],
     'G3': [(583, '12/11'), (829, '11/11'), (369, '12/11')],
     'G4': [(224, '28/10'), (469, '01/11'), (924, '11/11'), (631, '03/11'), (988, '31/10'), (988, '03/11'),
            (836, '04/11'), (1000, '03/11'), (1000, '13/11'), (962, '05/11'), (962, '09/11'), (962, '16/11'),
            (991, '09/11'), (991, '11/11')],
     'G5': [(787, '30/10')]}

erros = Counter()
tz = timezone.get_current_timezone()


def executar_etapa(etapa, funcao):
    # Savepoint permite continuar mesmo apos erro de banco; nao imprime
    # mensagens de excecao, que poderiam conter dados pessoais.
    try:
        with transaction.atomic():
            return True, funcao()
    except Exception as exc:
        erros[(etapa, type(exc).__name__)] += 1
        return False, None


def hora_local(valor):
    if valor is None:
        return '?'
    return timezone.localtime(valor, tz).strftime('%d/%m/%Y_%H:%M:%S')


def classificar_como_command(c, data):
    # Mesmo caminho e argumentos de:
    # ponto/management/commands/supra_juiz.py:102-116,120-156,173-178,
    # 186-202 e 205-234. A janela pedida aqui e um unico dia.
    ini = fim = data
    janela_ini = timezone.make_aware(
        dt.datetime.combine(ini - dt.timedelta(days=1), dt.time.min)
    )
    janela_fim = timezone.make_aware(
        dt.datetime.combine(fim + dt.timedelta(days=3), dt.time.min)
    )
    bs = list(Batida.objects.filter(
        colaborador=c,
        timestamp__gte=janela_ini,
        timestamp__lt=janela_fim,
    ).order_by('timestamp'))
    if not bs:
        # O command pula este colaborador nesta janela.
        raise ValueError('command_sem_batidas_na_janela')

    encerrados = dias_encerrados(c, ini, fim)
    grade = grade_da_celula(c, ini, fim)
    primeira = Batida.objects.filter(
        colaborador=c
    ).order_by('timestamp').first()
    adesao = timezone.localdate(primeira.timestamp) if primeira else None

    vigs = list(EscalaColaborador.objects.filter(
        colaborador=c
    ).select_related('tipo_escala').order_by('data_inicio'))

    def sem_fase(dref):
        ec = None
        for v in vigs:
            if v.data_inicio and v.data_inicio <= dref:
                ec = v
        if ec is None or ec.tipo_escala is None:
            return False
        if ec.tipo_escala.tipo_ciclo not in ('12x36', '24x48'):
            return False
        a = ec.data_ancora_colaborador
        return a is None or a.year <= 1940

    cadastro = cadastro_do_dia(
        c.pk,
        list(EscalaColaborador.objects.filter(
            colaborador_id=c.pk
        ).select_related('tipo_escala')),
        {(c.pk, x.data): x for x in CelulaDia.objects.filter(
            colaborador_id=c.pk, data__range=(ini, fim)
        )},
        data_demissao=c.data_demissao,
    )
    regua_deg = ChamadoColaborador.objects.filter(
        colaborador=c,
        modulo_origem='regua_degenerada',
        status_local__in=VIVOS,
    ).exists()

    chs = {}
    for ch in (ChamadoColaborador.objects.filter(
            colaborador=c, modulo_origem__in=FURO_MODULES
    ).prefetch_related('perguntas_que_resolvem')):
        d = data_do_chamado(ch.modulo_origem, ch.contexto_json)
        if d:
            chs.setdefault(str(d), []).append(ch)

    dias = [
        x for x in (grade.get('dias') or [])
        if x.get('data') == data and ini <= x.get('data') <= fim
    ]
    if len(dias) != 1:
        raise ValueError('grade_nao_devolveu_um_dia')

    dia = dias[0]
    lista = chs.get(data.isoformat(), [])
    vivos = [x for x in lista if x.status_local in VIVOS]
    com_fio = [
        x for x in vivos if fio_vivo(x.perguntas_que_resolvem.all())
    ]
    cob = ausencia_cobre(c, data, para='cobranca')
    n_bat = sum(1 for b in bs if timezone.localdate(b.timestamp) == data)

    resultado = classificar_dia(
        dia, dia.get('marcos') or [],
        chamados_vivos=len(vivos),
        chamados_total=len(lista),
        cobrancas_vivas=len(com_fio),
        cobertura_status=getattr(cob, 'status', None),
        data=data,
        em_operacao_desde=adesao,
        isento_ponto=bool(c.isento_ponto),
        batidas_no_dia=n_bat,
        hoje=timezone.localdate(),
        turno_encerrado=encerrados.get(data, False),
        sem_fase=sem_fase(data),
        regua_degenerada=regua_deg,
        **cadastro(data)
    )
    codigos = [cod for cod, detalhe in resultado]
    veredito = derivar_veredito(codigos)

    # Os dois retornos de classificar_dia sao o retorno antecipado de
    # isento e o retorno final. Nao se afirma qual predicado interno
    # deixou o resultado vazio.
    ramo = None
    if veredito == 'concorde':
        ramo = 'ISENTO_PONTO' if bool(c.isento_ponto) else 'RETORNO_FINAL'
    return veredito, codigos, ramo


def ultima_batida(c, data):
    ini = timezone.make_aware(dt.datetime.combine(data, dt.time.min), tz)
    fim = timezone.make_aware(
        dt.datetime.combine(data + dt.timedelta(days=1), dt.time.min), tz
    )
    # batidas_apuraveis usa fim inclusivo; exclui-se a meia-noite seguinte.
    bats = [b for b in batidas_apuraveis(c, ini, fim) if b.timestamp < fim]
    if not bats:
        return None
    # "Ultima batida" pela ordem de timestamp da autoridade;
    # pk desempata marcas com o mesmo timestamp.
    return max(bats, key=lambda b: (b.timestamp, b.pk)).criado_em


def rodar():
    resumo = {
        grupo: Counter({'sim': 0, 'nao': 0, '?': 0,
                        'acusa': 0, 'concorde': 0, 'hoje_?': 0})
        for grupo in G
    }

    for grupo, casos in G.items():
        for pk, dm in casos:
            dd, mm = dm.split('/')
            data = dt.date(2026, int(mm), int(dd))
            grav = julg = hoje = codigos = ub = ramo = None

            ok_col, col = executar_etapa(
                'colaborador', lambda: Colaborador.objects.get(pk=pk)
            )
            if ok_col:
                ok_cel, cel = executar_etapa(
                    'celula',
                    lambda: CelulaDia.objects.filter(
                        colaborador_id=pk, data=data
                    ).first()
                )
                if ok_cel and cel is not None:
                    grav = cel.veredito
                    julg = cel.julgada_em

                ok_hoje, atual = executar_etapa(
                    'classificacao', lambda: classificar_como_command(col, data)
                )
                if ok_hoje:
                    hoje, codigos, ramo = atual

                ok_ub, ub = executar_etapa(
                    'batida', lambda: ultima_batida(col, data)
                )

            # Nao ha carimbo demonstrado para todas as alteracoes de ata.
            ua = None
            velho = '?'

            r = resumo[grupo]
            r[velho] += 1
            if hoje is None:
                r['hoje_?'] += 1
            elif hoje == 'concorde':
                r['concorde'] += 1
            else:
                r['acusa'] += 1

            cod_txt = '?' if codigos is None else (
                ','.join(codigos) if codigos else '-'
            )
            linha = (
                'D %s col%d %s GRAV=%s JULG=%s HOJE=%s COD=%s '
                'UB=%s UA=%s VELHO=%s'
                % (grupo, pk, dm, grav or '?', hora_local(julg),
                   hoje or '?', cod_txt, hora_local(ub),
                   hora_local(ua), velho)
            )
            if hoje == 'concorde':
                linha += ' RAMO %s' % (ramo or '?')
            print(linha)

    for grupo in G:
        r = resumo[grupo]
        print(
            'RESUMO %s VELHO sim=%d nao=%d ?=%d '
            'HOJE acusa=%d concorde=%d ?=%d'
            % (grupo, r['sim'], r['nao'], r['?'],
               r['acusa'], r['concorde'], r['hoje_?'])
        )


try:
    if connection.in_atomic_block:
        raise RuntimeError('transacao_preexistente')
    if connection.vendor != 'postgresql':
        raise RuntimeError('banco_nao_postgresql')

    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute('SET TRANSACTION READ ONLY')
            cursor.execute('SELECT current_schema()')
            if cursor.fetchone()[0] != 'juliani':
                raise RuntimeError('schema_ativo_nao_e_juliani')
        rodar()
except Exception as exc:
    erros[('sonda', type(exc).__name__)] += 1

total_erros = sum(erros.values())
print('ERROS %d' % total_erros)
for (etapa, tipo), quantidade in sorted(erros.items()):
    print('ERROS etapa=%s tipo=%s n=%d' % (etapa, tipo, quantidade))
print('FIM_SONDA_VELHO')
```

As referências principais da sonda são:

- `CelulaDia.veredito`, `veredito_via`, `veredito_em`, `protestos` e `ata`: [app/escala/models.py:1769](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1769). `julgada_em`: [app/escala/models.py:1738](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1738).
- Montagem dos argumentos do command: [app/ponto/management/commands/supra_juiz.py:102](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:102), [linha 120](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:120), [linha 142](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:142), [linha 173](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:173), [linha 186](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:186) e [linha 205](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/supra_juiz.py:205).
- `classificar_dia` e seus retornos: [app/ponto/supra_juiz.py:217](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:217), [linha 233](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:233) e [linha 549](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:549). `cadastro_do_dia`: [linha 155](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:155).
- `batidas_apuraveis`, inclusive ordenação e limite superior inclusivo: [app/ponto/turnos.py:2868](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/turnos.py:2868); `Batida.criado_em`: [app/ponto/models.py:79](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/models.py:79). `dias_encerrados` e `ausencia_cobre`: [app/ponto/turnos.py:2133](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/turnos.py:2133), [linha 2550](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/turnos.py:2550).
- `fio_vivo`, `data_do_chamado` e `FURO_MODULES`: [app/chamados/juizes.py:1510](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/juizes.py:1510), [app/chamados/catalogo/modulos.py:219](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/catalogo/modulos.py:219), [app/chamados/catalogo/furo_modulos.py:15](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/catalogo/furo_modulos.py:15).

## 18/11 17:38 · sessao 328937483a19 · E4C-F6F7 · leitura · origem=Code

A sonda abaixo é uma **proposta não executada**. A compatibilidade com `c8b6d1ec2`, a presença da coluna no banco sombra, os dados e o resultado do diff estão **[não verificados]**: esta cópia não permite conferir o diff pedido contra o main. O script registra incompatibilidades por etapa, inclusive `TypeError`; uma execução com `ERROS > 0` não constitui medida completa.

A 0056 usa o predicado `ato='resposta'` e `exclude(ato_motivo='')` ([migração 0056:12](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12)). A sonda usa SQL para reproduzi-lo porque o manager do modelo atual proíbe `update` ([models.py:4121](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/models.py:4121), [models.py:4197](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/models.py:4197)).

O universo `CD` é o solicitado: situação `ativo` **ou** alguma batida com data civil local na janela da empresa, cruzado com todos os dias dessa janela ([models.py:22](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/ponto/models.py:22), [colaboradores/models.py:323](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/colaboradores/models.py:323)). Ele é mais amplo que o universo do vigia: `dias_que_seguram` seleciona retidos por `furo_espelho` e seus dias acusadores ([fora_da_folha.py:125](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:125), [vigia_fio.py:530](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/vigia_fio.py:530)). `G` é o grupo real **da pessoa**, repetido em seus dias `CD`; `-` significa que ela não aparece como retida com grupo ([fora_da_folha.py:94](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:94), [fora_da_folha.py:210](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:210)).

`PQ` cobre todas as perguntas visíveis com **linhas consecutivas** `VALIDACAO`, `RESPOSTA` com `mantem=True`, inclusive fora da competência; a ordem é `(ato_em, pk)` ([models.py:4199](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/models.py:4199)). Uma pergunta sem dia recebe `T=ERRO P=ERRO` e conta como erro. O instante fixo deve ser igual nas duas rodadas: a tarefa corta decisões por ele ([tarefa_do_dia.py:484](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:484)). Rode as duas medições sobre a mesma fotografia do banco e no mesmo dia civil: `classificar_export` também lê o relógio próprio ([fora_da_folha.py:137](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:137)). Igualdade da fotografia e condições do banco sombra: **[não verificado]**.

```python
# Executar SOMENTE no banco sombra, nas duas versões, com o mesmo AGORA.
# Exemplo no manage.py shell:
#     exec(open('/caminho/sonda.py').read())
#
# A sonda não verifica que o banco é sombra. Essa identificação é
# responsabilidade de quem executa. Não imprime nomes, CPF ou mensagens
# de exceção.

import calendar
import datetime
from collections import defaultdict

from django.db import connection, transaction
from django.db.models import Q
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador, Empresa
from ponto.models import Batida
from chamados.models import DecisaoPergunta, PerguntaDisputa
from chamados.services.tarefa_do_dia import carga_da_tarefa, porta_da_tarefa
from folha.fora_da_folha import fora_da_folha
from ponto.janelas import janela_fechamento


# Mesmo instante nas duas rodadas. Alterar somente antes de AMBAS as rodadas.
AGORA = datetime.datetime(
    2026, 11, 18, 20, 30, 0, tzinfo=datetime.timezone.utc
)
MES, ANO = 11, 2026

erros = []


class RollbackSonda(Exception):
    pass


class EscritaNaoPermitida(Exception):
    pass


def erro(etapa, exc):
    # Não imprime str(exc): mensagens podem conter dados pessoais.
    erros.append((etapa, type(exc).__name__))


def tentar(etapa, funcao):
    # O savepoint permite continuar após um erro SQL sem deixar a
    # transação externa quebrada. O resultado mal sucedido é None.
    try:
        with transaction.atomic():
            return True, funcao()
    except Exception as exc:
        erro(etapa, exc)
        return False, None


def canon(valor):
    """Representação determinística sem serializar objetos desconhecidos."""
    if valor is None or isinstance(valor, (bool, int, float, str)):
        return valor
    if isinstance(valor, (datetime.date, datetime.time)):
        return valor.isoformat()
    if isinstance(valor, dict):
        return tuple(
            (canon(k), canon(v))
            for k, v in sorted(valor.items(), key=lambda item: repr(item[0]))
        )
    if isinstance(valor, (list, tuple)):
        return tuple(canon(v) for v in valor)
    if isinstance(valor, (set, frozenset)):
        return tuple(sorted((canon(v) for v in valor), key=repr))
    raise TypeError("tipo não suportado na assinatura")


def assinatura_tarefa(t):
    # Não inclui agora nem celula_id. Mantém a ordem das instâncias e ações,
    # pois ela pode fazer parte da decisão publicada. [nao verificado]
    campos_dia = (
        "nao_sei", "razao", "rota", "fila", "verbos", "fio",
        "acao", "ausencia",
    )
    campos_instancia = (
        "instancia", "tipo", "hora", "data_civil", "lampada",
        "vencido", "motivo", "pergunta_id", "razao", "acoes",
    )
    return canon({
        "dia": {k: t.get(k) for k in campos_dia},
        "instancias": [
            {k: inst.get(k) for k in campos_instancia}
            for inst in t.get("instancias", ())
        ],
    })


def assinatura_porta(p):
    return canon(p)


def meses_dia(dia):
    ini = dia.replace(day=1)
    fim = dia.replace(day=calendar.monthrange(dia.year, dia.month)[1])
    return ini, fim


def dias(ini, fim):
    for n in range((fim - ini).days + 1):
        yield ini + datetime.timedelta(days=n)


# Bloqueia comandos SQL que possam escrever durante as leituras.
# O único UPDATE autorizado é executado com a chave temporariamente ligada.
permitir_update_0056 = False


def guarda_sql(execute, sql, params, many, context):
    primeiro = sql.lstrip().split(None, 1)[0].upper() if sql.strip() else ""
    permitidos = {
        "SELECT", "SHOW", "SAVEPOINT", "RELEASE", "ROLLBACK",
    }
    if primeiro == "UPDATE" and permitir_update_0056:
        return execute(sql, params, many, context)
    if primeiro not in permitidos:
        raise EscritaNaoPermitida()
    return execute(sql, params, many, context)


def executar():
    global permitir_update_0056

    print("AGORA", AGORA.isoformat())
    print("DIA_CIVIL_EXECUCAO", timezone.localdate().isoformat())
    print("DIA_CIVIL_AGORA", timezone.localdate(AGORA).isoformat())

    tabela = connection.ops.quote_name(DecisaoPergunta._meta.db_table)
    coluna_ato = connection.ops.quote_name("ato")
    coluna_motivo = connection.ops.quote_name("ato_motivo")
    coluna_mantem = connection.ops.quote_name("resposta_mantem_validacao")

    # ato_motivo é NOT NULL no modelo desta cópia. Este SQL reproduz
    # filter(ato='resposta').exclude(ato_motivo='') da 0056.
    predicado = "%s = %%s AND %s <> %%s" % (coluna_ato, coluna_motivo)

    with connection.execute_wrapper(guarda_sql):
        with connection.cursor() as cursor:
            cursor.execute(
                "SELECT COUNT(*) FROM %s WHERE %s" % (tabela, predicado),
                ["resposta", ""],
            )
            print("PRE_0056", cursor.fetchone()[0])

            permitir_update_0056 = True
            try:
                cursor.execute(
                    "UPDATE %s SET %s = %%s WHERE %s"
                    % (tabela, coluna_mantem, predicado),
                    [True, "resposta", ""],
                )
            finally:
                permitir_update_0056 = False

        hoje = timezone.localdate(AGORA)

        ok, empresas = tentar(
            "empresas",
            lambda: list(Empresa.objects.all().order_by("pk")),
        )
        if not ok:
            return

        janelas = {}
        grupos = {}
        pares_cd = set()
        colabs = {}

        for empresa in empresas:
            eid = empresa.pk
            ok, janela = tentar(
                "janela",
                lambda e=empresa: janela_fechamento(MES, ANO, e),
            )
            if not ok:
                continue
            ini, fim = janela
            janelas[eid] = (ini, fim)
            print(
                "EMPRESA", eid,
                "CORTE", empresa.dia_inicio_competencia,
                "INI", ini.isoformat(),
                "FIM", fim.isoformat(),
            )

            # __date usa a zona ativa do Django para o timestamp.
            def carregar_colabs():
                ids_batida = Batida.objects.filter(
                    colaborador__empresa_id=eid,
                    timestamp__date__gte=ini,
                    timestamp__date__lte=fim,
                ).values("colaborador_id")
                return list(
                    Colaborador.objects.filter(empresa_id=eid)
                    .filter(Q(situacao="ativo") | Q(pk__in=ids_batida))
                    .select_related("empresa")
                    .order_by("pk")
                )

            ok, lista = tentar("universo_cd", carregar_colabs)
            if ok:
                for c in lista:
                    colabs[c.pk] = c
                    for dia in dias(ini, fim):
                        pares_cd.add((c.pk, dia))

            # Chama a função real que calcula o grupo da pessoa.
            ok, resultado = tentar(
                "fora_da_folha",
                lambda e=empresa: fora_da_folha(
                    e, MES, ANO, agora=AGORA
                ),
            )
            if ok:
                for linha in resultado["colabs"]:
                    grupos[linha["colab"]] = linha["grupo"] or "-"
            else:
                # Falha da classificação não pode parecer ausência de grupo.
                grupos[eid, "erro_empresa"] = True

        # Ler a coluna diretamente também permite detectar sua ausência no
        # banco sem exigir que o modelo Python do main a declare.
        def pares_validacao_resposta():
            sql = (
                "SELECT pergunta_id, ato, %s "
                "FROM %s ORDER BY pergunta_id, ato_em, id"
                % (coluna_mantem, tabela)
            )
            encontrados = set()
            anterior_pergunta = None
            anterior_ato = None
            with connection.cursor() as cursor:
                cursor.execute(sql)
                while True:
                    bloco = cursor.fetchmany(2000)
                    if not bloco:
                        break
                    for pergunta_id, ato, mantem in bloco:
                        if pergunta_id != anterior_pergunta:
                            anterior_ato = None
                        if (
                            anterior_ato == "validacao"
                            and ato == "resposta"
                            and mantem is True
                        ):
                            encontrados.add(pergunta_id)
                        anterior_pergunta = pergunta_id
                        anterior_ato = ato
            return encontrados

        ok, ids_perguntas = tentar("sequencia_pq", pares_validacao_resposta)
        if not ok:
            ids_perguntas = set()

        def carregar_perguntas():
            return list(
                PerguntaDisputa.objects.filter(pk__in=ids_perguntas)
                .select_related("disputa__colaborador__empresa")
                .order_by("pk")
            )

        ok, perguntas = tentar("perguntas_pq", carregar_perguntas)
        if not ok:
            perguntas = []

        pares_pq = []
        for p in perguntas:
            c = p.disputa.colaborador
            colabs[c.pk] = c
            pares_pq.append((p.pk, c.pk, p.data_evento, c.empresa_id))

        # Uma carga por empresa/janela. O grupo da competência inclui todos
        # os pares CD e as perguntas PQ que caem nela. Perguntas de outros
        # períodos são agrupadas por mês civil.
        lotes = defaultdict(set)
        for cid, dia in pares_cd:
            c = colabs[cid]
            ini, fim = janelas[c.empresa_id]
            lotes[(c.empresa_id, ini, fim)].add(cid)

        for pk, cid, dia, eid in pares_pq:
            if dia is None:
                erro("pq_sem_dia", ValueError())
                continue
            janela = janelas.get(eid)
            if janela and janela[0] <= dia <= janela[1]:
                ini, fim = janela
            else:
                ini, fim = meses_dia(dia)
            lotes[(eid, ini, fim)].add(cid)

        resultados = {}

        for (eid, ini, fim), ids in sorted(lotes.items()):
            lista = [colabs[cid] for cid in sorted(ids)]
            ok, carga = tentar(
                "carga_da_tarefa",
                lambda cs=lista, a=ini, b=fim: carga_da_tarefa(
                    cs, a, b, AGORA
                ),
            )
            if not ok:
                continue

            necessarios = {
                (cid, dia)
                for cid, dia in pares_cd
                if cid in ids and colabs[cid].empresa_id == eid
                and ini <= dia <= fim
            }
            necessarios.update(
                (cid, dia)
                for _, cid, dia, peid in pares_pq
                if dia is not None and peid == eid
                and cid in ids and ini <= dia <= fim
            )

            for cid, dia in sorted(necessarios):
                chave = (cid, dia)
                if chave in resultados:
                    continue

                ok, tarefa = tentar(
                    "tarefa",
                    lambda c=colabs[cid], d=dia: carga.tarefa(c, d),
                )
                if not ok:
                    resultados[chave] = ("ERRO", "ERRO")
                    continue

                ok_t, sig_t = tentar(
                    "assinatura_tarefa",
                    lambda t=tarefa: assinatura_tarefa(t),
                )
                ok_p, porta = tentar(
                    "porta_da_tarefa",
                    lambda t=tarefa: porta_da_tarefa(t),
                )
                if ok_p:
                    ok_p, sig_p = tentar(
                        "assinatura_porta",
                        lambda p=porta: assinatura_porta(p),
                    )
                resultados[chave] = (
                    repr(sig_t) if ok_t else "ERRO",
                    repr(sig_p) if ok_p else "ERRO",
                )

        for cid, dia in sorted(pares_cd):
            c = colabs[cid]
            if grupos.get((c.empresa_id, "erro_empresa")):
                grupo = "ERRO"
            else:
                grupo = grupos.get(cid, "-")
            t, p = resultados.get((cid, dia), ("ERRO", "ERRO"))
            print(
                "CD col%d %s T=%s P=%s G=%s"
                % (cid, dia.isoformat(), t, p, grupo)
            )

        for pk, cid, dia, _eid in sorted(pares_pq):
            if dia is None:
                data = "-"
                t, p = "ERRO", "ERRO"
            else:
                data = dia.isoformat()
                t, p = resultados.get((cid, dia), ("ERRO", "ERRO"))
            print("PQ %s col%d %s T=%s P=%s" % (pk, cid, data, t, p))


try:
    with schema_context("juliani"):
        try:
            with transaction.atomic():
                try:
                    executar()
                except Exception as exc:
                    erro("execucao", exc)
                # Sempre sair do atomic por exceção: desfaz a 0056 simulada.
                raise RollbackSonda()
        except RollbackSonda:
            pass
except Exception as exc:
    erro("schema_ou_rollback", exc)
finally:
    print("ERROS", len(erros))
    for etapa, tipo in erros[:20]:
        print("ERRO", etapa, tipo)
    print("FIM_SONDA_DIFF_RETIDO")
```

Funções reais chamadas diretamente pela sonda nesta cópia:

- `janela_fechamento`: [janelas.py:54](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/ponto/janelas.py:54).
- `fora_da_folha`: [fora_da_folha.py:177](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:177); ela chama `dias_que_seguram` e calcula o grupo por `_grupo` ([fora_da_folha.py:125](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:125), [fora_da_folha.py:94](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/folha/fora_da_folha.py:94)).
- `carga_da_tarefa` e `CargaDaTarefa.tarefa`: [tarefa_do_dia.py:795](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:795), [tarefa_do_dia.py:632](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:632). É o mesmo caminho em lote usado pelo vigia ([vigia_fio.py:591](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/vigia_fio.py:591)); a casca `tarefa_do_dia` também delega a `carga.tarefa` ([tarefa_do_dia.py:800](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:800)).
- `porta_da_tarefa`: [tarefa_do_dia.py:779](/home/ronald/codex_ws/0726b5c986370207be4ce0a440bbe71230dc87fa/app/chamados/services/tarefa_do_dia.py:779).

Existência e assinatura dessas funções no main `c8b6d1ec2`: **[não verificado]**. Não é possível afirmar, com esta cópia e o anexo fornecido, que alguma delas exista somente em um dos lados.

## 18/11 17:37 · sessao d471b06a0eb8 · DECLARADO-X-GRAVADO · construtor · origem=Code

**Parei sem implementar a cura:** não identifiquei uma porta existente que devolva ao colaborador, com a frase exigida, uma resposta já validada nos dois ramos abrangidos. A materialização é chamada após o carimbo de validação. [validacao.py:743](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/validacao.py:743), [materializacao.py:1923](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/materializacao.py:1923)

A porta `recusar_pergunta` devolve a pergunta ao colaborador, mas recusa perguntas já validadas. A outra porta, `devolver_validacao_recusada`, devolve a resposta ao **admin**, preserva a resposta e se restringe ao caso de resposta de hora; ela não realiza a devolução pedida ao colaborador para o ramo `data_hora`. [validacao.py:898](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/validacao.py:898), [validacao.py:237](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/validacao.py:237)

A porta de reabertura limpa a resposta e registra o motivo na trilha, mas a representação da pergunta na API examinada não inclui esse motivo. A recusa existente envia um push genérico; a entrega da frase exigida ao colaborador por essa via está **[não verificado]**. [disputa_emissao.py:1584](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/disputa_emissao.py:1584), [api/views.py:2549](/home/ronald/wt-codex-declarado-x-gravado-173308/app/api/views.py:2549), [validacao.py:943](/home/ronald/wt-codex-declarado-x-gravado-173308/app/chamados/services/validacao.py:943) [nao verificado]

Falta, portanto, uma porta chamável que trate a devolução de resposta **já validada**, para `hora` e `data_hora`, e disponibilize a frase ao colaborador. Sua localização e implementação exigidas estão **[não verificado]**. A frase literal também não consta do dado fornecido **[não verificado]**.

Nenhum arquivo foi alterado; R1–R10, mutantes, compilação e testes não foram executados. A contagem de palavras de band-aid no diff está **[não verificado]**. A entrega e a linha LEI-AKITA solicitadas permanecem pendentes; não apresento os REDs como existentes. [nao verificado]

## 18/11 17:36 · sessao 7a0a1b4d842c · REPROVAS-1811-1740 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 1: PROCEDE.** O item 2 segue com as reservas abaixo. [não verificado: hash do HEAD por Git; a cópia disponível não tem metadados Git.]

### 1. E4C-F6F7 — PROCEDE

1. **Premissas: conferem parcialmente.** A linha 164 do main devolve a tarefa ao colaborador em caso de recusa ou reabertura; ela não demonstra o laço atribuído à ponta F6F7 ([tarefa_do_dia.py:142](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/tarefa_do_dia.py:142)). A cadeia de leitura existe: `fora_da_folha` lê a porta, deriva o fio e agrupa a pessoa pelos fios dos dias; o retrato consome essa saída ([fora_da_folha.py:70](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:70), [fora_da_folha.py:94](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:94), [fora_da_folha.py:265](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:265), [retrato_folha.py:1212](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/retrato_folha.py:1212)). Cadastro, vigia e pautas também são leitores, direta ou indiretamente ([cadastro_realidade.py:472](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/cadastro_realidade.py:472), [vigia_fio.py:594](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/vigia_fio.py:594), [pautas_dos_discordantes.py:70](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/pautas_dos_discordantes.py:70)).

   **Há vigia de produção onde um contador pode ser integrado:** `vigiar_fio_morto` chama contadores, registra seus valores e esperados, e está encadeado na rotina ([vigiar_fio_morto.py:100](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/vigiar_fio_morto.py:100), [vigiar_fio_morto.py:179](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/vigiar_fio_morto.py:179), [vigiar_fio_morto.py:244](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/vigiar_fio_morto.py:244), [crons.py:1737](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/config/crons.py:1737)). O contador nomeado da F7 e seu caso que morde na ponta: **[não verificado]**.

2. **Colide com o aval vigente.** A autorização de pousar na véspera se o diff der zero conflita com a vedação fornecida de pouso que toque folha ou retenção entre 18/11 às 18h e o fim da simulação, pois a cadeia de leitores alcança folha e retrato ([fora_da_folha.py:70](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:70), [retrato_folha.py:1212](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/retrato_folha.py:1212)). O zero medido é uma condição adicional do aval anexo; não elimina literalmente a vedação fornecida. A precedência F6F7 antes da F9 e o compact continuam condições informadas; seu cumprimento **[não verificado]**.

3. **O literal não fecha ainda.** Para corte 21, competência 11/2026 compreende **21/10/2026 a 20/11/2026**, inclusive; outros cortes da empresa produzem outros intervalos ([janelas.py:10](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/janelas.py:10), [janelas.py:36](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/janelas.py:36)). O diff existente é de leitura e percorre colaboradores e dias, mas compara C17 com a tarefa da própria cópia; suas projeções não executam os censos e sua saída é agregada e anonimizada. Ele não é a porta pronta para executar **main versus ponta curada** nem entregar a tabela de mudança de grupo e lista nominal pedidas ([diff_e3_sombra.py:1](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/diff_e3_sombra.py:1), [diff_e3_sombra.py:46](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/diff_e3_sombra.py:46), [diff_e3_sombra.py:127](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/management/commands/diff_e3_sombra.py:127)). Outra porta capaz desse confronto **[não verificado]**.

4. **Exportada, dinheiro ou `!`:** há alcance sobre a apresentação do retido e o retrato lavrado, já reconhecido na retificação do aval; o cron de retrato usa `--apply` ([retrato_folha.py:1207](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/retrato_folha.py:1207), [crons.py:1650](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/config/crons.py:1650)). Alteração de competência exportada, dinheiro ou outro `!` causado pela ponta **[não verificado]**.

### 2. ESCALA-NAO-DESCREVE — CONFERE, com alcance limitado

1. **Premissas:** o veredito fornecido relata selo sem mordida e ausência de log do mutante; a cópia da obra e `test_escala_nao_descreve.py:61-62` **[não verificado]**. Também **[não verificado]** se a cura já feita mata o mutante, se o log existe e se os commits citados contêm a linha exigida. [nao verificado]

2. **Colisão:** o pouso indicado para depois da simulação é compatível com a janela vigente fornecida; o rito, o compact e a ordem eventualmente aplicável continuam pendentes de comprovação **[não verificado]**. [nao verificado]

3. **Literal:** o aval exige log do mutante nomeado e linha `LEI-AKITA` com `padrao=`; o fechamento dessas condições **[não verificado]**. O aceite para os pontos do veredito classificados como fila também **[não verificado]**.

4. **Exportada, dinheiro ou `!`:** o dado fornecido declara que a porta de vínculo regenera células e reconcilia, mas deixa **não verificado** o efeito sobre competência entregue; o código presente confirma a chamada de reconciliação ([vinculo.py:346](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/colaboradores/services/vinculo.py:346)). Efeito sobre exportada, dinheiro ou outro `!` **[não verificado]**.

## 18/11 17:33 · sessao ae999d6c0dcd · E4C-F6F7 · construtor · origem=Code

A cura liga o `handle` ao contador uma vez e publica as quatro chaves devolvidas por ele, antes da bifurcação de `--aplicar`, sem nova escrita ([backfill_decisoes_pergunta.py:595](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:595)).

O teste integrado cria uma pergunta validada sem linha de validação e confere a saída do comando ([test_e4c_f6f7_c11_backfill.py:60](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c11_backfill.py:60)). **RED e GREEN integrados: [não verificado]**, pois precisam de banco e não foram executados. Comando para quem certifica: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f6f7_c11_backfill.BackfillPublicaContadorF7
```

A verificação sem banco foi executada antes da cura e falhou com `AssertionError: RED: handle nao chama o contador`; depois, passou com `GREEN: costura sem banco`. Ela verifica a chamada, a posição anterior ao ramo `--aplicar` e o uso das quatro chaves com valores sentinela ([test_e4c_f6f7_c11_backfill.py:9](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c11_backfill.py:9)). Comando GREEN: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py
```

Os cinco mutantes em memória morreram: M1 remove chamada e linha; M2–M5 fixam, respectivamente, número, esperado, dono e nome ([test_e4c_f6f7_c11_backfill.py:13](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c11_backfill.py:13)). Cada comando deve falhar por asserção:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M1
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M2
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M3
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M4
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_e4c_f6f7_c11_backfill.py M5
```

`py_compile` passou nos dois arquivos, com a escrita de bytecode desativada: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import py_compile
py_compile._write_atomic = lambda *args, **kwargs: None
for p in ('chamados/management/commands/backfill_decisoes_pergunta.py',
          'chamados/tests/test_e4c_f6f7_c11_backfill.py'):
    py_compile.compile(p, cfile=p + '.pyc', doraise=True)
    print('py_compile OK:', p)
PY
```

LEI-AKITA: origem=[backfill_decisoes_pergunta.py::handle](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:595), testemunha=[contador_validada_projetada_sem_linha](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:94) (lê `DecisaoPergunta.linhas_da_projecao_sem_linha`), RED=[teste integrado](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c11_backfill.py:61) [não verificado]; verificação sem banco RED executada, quem-mais-le=[teste F7](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_f7_contador_validada_sem_linha.py:31) e [teste sem banco C7](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:51), juizes novos=0, padrao=[Parallel Change / Expand-Contract](/home/ronald/wt-codex-e4c-f6f7-132608/app/docs/GLOSSARIO.md:18). [nao verificado]

## 18/11 17:33 · sessao 681f0c12c82c · E4C-F6F7 · leitura · origem=Code

A sonda abaixo mede **pares candidato de colaborador e dia que também são dias de retenção**, além de colaboradores distintos nessa interseção. Ela não calcula o número de pessoas que efetivamente mudariam de grupo A–E. Na cópia, o grupo depende dos fios de **todos** os dias retidos, e a seleção da porta depende de mais condições além da sequência de atos. [app/folha/fora_da_folha.py:94](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/folha/fora_da_folha.py:94), [app/folha/fora_da_folha.py:210](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/folha/fora_da_folha.py:210), [app/chamados/services/tarefa_do_dia.py:694](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/tarefa_do_dia.py:694)

A cópia não tem metadados Git acessíveis; por isso, **não foi possível confirmar nenhuma das funções no main pai**, nem afirmar que alguma esteja ausente nele. [não verificado] Na cópia, as dependências de domínio usadas são `es_da_ressuscitacao`, `empresas_visiveis`, `competencias_a_lavrar` e `dias_que_seguram`. [app/chamados/models.py:4221](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/models.py:4221), [app/colaboradores/queries.py:78](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/colaboradores/queries.py:78), [app/ponto/services/he_pendente_lavrado.py:209](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/ponto/services/he_pendente_lavrado.py:209), [app/folha/fora_da_folha.py:125](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/folha/fora_da_folha.py:125)

O predicado da 0056 é `ato='resposta'` e `ato_motivo` diferente de vazio; a simulação também preserva `True` já existente nas demais respostas. [app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12) A sequência usa o horizonte, a ordem e a exclusão de ressuscitação da carga; o vínculo ao dia usa `conversa=False`, `colaborador_id` e `data_evento`. [app/chamados/services/tarefa_do_dia.py:393](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/tarefa_do_dia.py:393), [app/chamados/services/tarefa_do_dia.py:506](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/tarefa_do_dia.py:506)

O universo usa a função real `dias_que_seguram`, chamada por `fora_da_folha` e pelo vigia; o recorte de empresas e competências segue o vigia. A função de competências inclui a atual e, quando não entregue, a anterior: isso não demonstra cobertura de todas as competências históricas abertas. [app/folha/fora_da_folha.py:200](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/folha/fora_da_folha.py:200), [app/chamados/services/vigia_fio.py:529](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/vigia_fio.py:529), [app/ponto/services/he_pendente_lavrado.py:209](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/ponto/services/he_pendente_lavrado.py:209)

Execute o texto pelo shell de `tenant_command` indicado, por exemplo redirecionando o arquivo de entrada. A sonda não foi executada aqui. [não verificado] Ela bloqueia comandos SQL que não comecem por `SELECT`, não imprime mensagens de exceção que possam conter dados pessoais e invalida os números se `ERROS > 0`; exceções eventualmente capturadas dentro das próprias funções chamadas não são observáveis por esse contador. [não verificado]

```python
# Sonda F6F7 item 1.
# Executar dentro de: manage.py tenant_command shell --schema=juliani
# Somente leitura; nao muda schema, nao abre atomic e nao grava arquivos.

from itertools import groupby

erros = 0
etapa = "inicializacao"


def somente_select(execute, sql, params, many, context):
    # Defesa adicional: rejeita comandos que nao sejam SELECT.
    if not sql.lstrip().upper().startswith("SELECT"):
        raise RuntimeError("SQL nao SELECT bloqueado")
    return execute(sql, params, many, context)


try:
    from django.db import connection
    from django.utils import timezone

    from chamados.models import DecisaoPergunta as D
    from colaboradores.queries import empresas_visiveis
    from folha.fora_da_folha import dias_que_seguram
    from ponto.services.he_pendente_lavrado import competencias_a_lavrar

    agora = timezone.now()
    hoje = timezone.localdate(agora)

    with connection.execute_wrapper(somente_select):
        etapa = "predicado_0056"
        # Mesmo predicado da migracao, sem update.
        marcadas = set(
            D.objects.filter(ato="resposta")
            .exclude(ato_motivo="")
            .values_list("pk", flat=True)
        )
        print("RESPOSTAS_PREDICADO_0056", len(marcadas))

        etapa = "sequencias"
        # Uma cauda pode conter respostas ja True e respostas que a 0056
        # marcaria. Examinar as perguntas com qualquer uma dessas respostas.
        candidatas = set(
            D.objects.filter(ato="resposta")
            .filter(resposta_mantem_validacao=True)
            .values_list("pergunta_id", flat=True)
        )
        candidatas.update(
            D.objects.filter(pk__in=marcadas)
            .values_list("pergunta_id", flat=True)
        )

        decisoes = (
            D.objects.filter(pergunta_id__in=candidatas, ato_em__lte=agora)
            .select_related("pergunta")
            .order_by("pergunta_id", "ato_em", "pk")
        )

        perguntas_mudam_ato = 0
        pares_perguntas_do_fato = set()
        perguntas_sem_dia_ou_colaborador = 0

        for pergunta_id, linhas in groupby(
            decisoes.iterator(), key=lambda d: d.pergunta_id
        ):
            sequencia = [
                d for d in linhas if not D.es_da_ressuscitacao(d)
            ]
            if not sequencia:
                continue

            k = len(sequencia)
            while k:
                d = sequencia[k - 1]
                mantem_depois = (
                    d.ato == D.RESPOSTA
                    and (d.resposta_mantem_validacao or d.pk in marcadas)
                )
                if not mantem_depois:
                    break
                k -= 1

            if not (
                0 < k < len(sequencia)
                and sequencia[k - 1].ato == D.VALIDACAO
            ):
                continue

            perguntas_mudam_ato += 1
            p = sequencia[-1].pergunta

            # A carga da tarefa usa apenas perguntas do fato.
            if p.conversa:
                continue
            if p.colaborador_id is None or p.data_evento is None:
                perguntas_sem_dia_ou_colaborador += 1
                continue
            pares_perguntas_do_fato.add((p.colaborador_id, p.data_evento))

        print("PERGUNTAS_MUDAM_ATO", perguntas_mudam_ato)
        print("PARES_PERGUNTAS_DO_FATO", len(pares_perguntas_do_fato))
        print(
            "PERGUNTAS_DO_FATO_SEM_DIA_OU_COLABORADOR",
            perguntas_sem_dia_ou_colaborador,
        )

        etapa = "universo_retidos"
        # Mesmo recorte de empresa e competencia do censo de alcance.
        # Conjuntos removem repeticoes entre competencias.
        retidos = set()
        pares_retidos = set()
        competencias = 0

        for empresa in empresas_visiveis():
            for mes, ano in competencias_a_lavrar(empresa, hoje):
                competencias += 1
                universo = dias_que_seguram(empresa, mes, ano, hoje=hoje)
                retidos.update(universo["furo"])
                for cid, (_motivos, dias) in universo["dias"].items():
                    for dia in dias:
                        pares_retidos.add((cid, dia["data"]))

        intersecao = pares_perguntas_do_fato & pares_retidos
        colaboradores_intersecao = {cid for cid, _dia in intersecao}

        print("COMPETENCIAS_EXAMINADAS", competencias)
        print("RETIDOS_DISTINTOS_FURO_ESPELHO", len(retidos))
        print("PARES_DIAS_RETIDOS_DISTINTOS", len(pares_retidos))
        print("INTERSECAO_PARES", len(intersecao))
        print("INTERSECAO_COLABORADORES_DISTINTOS", len(colaboradores_intersecao))
        for cid, dia in sorted(intersecao)[:20]:
            print("PAR", "col%s" % cid, dia.isoformat())

except Exception as exc:
    erros += 1
    # Sem str(exc) ou traceback: podem conter dados pessoais.
    print("ERRO", etapa, type(exc).__name__)
finally:
    print("ERROS", erros)
    print("FIM_SONDA_F6F7_ITEM1")
```

Um resultado zero na interseção só elimina candidatos **nesse recorte e instante**; não substitui o diff de `T`/porta, nem prova ausência de mudança após outros dados ou em competências fora do recorte. [app/chamados/services/tarefa_do_dia.py:484](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/chamados/services/tarefa_do_dia.py:484), [app/ponto/services/he_pendente_lavrado.py:209](/home/ronald/codex_ws/e78031f79b8769c1680f6ea0b4a5ce22eaa1ce55/app/ponto/services/he_pendente_lavrado.py:209)

## 18/11 17:32 · sessao c2bbe7b05200 · ESCALA-NAO-DESCREVE · verificador · origem=Code

**O c4 distingue os dois mutantes pela conta da fixture; a evidência de execução dos mutantes permanece [não verificado].** A mudança usa uma órfã que o caminho normal examinado de produção não gera, e o item 7 do veredito contém divergências do literal do aval que não podem ser tratadas como resolvidas sem aceite explícito. [app/escala/tests/test_escala_nao_descreve.py:63; app/escala/utils.py:621; anexo `veredito.md`, item 7]

1. **Mutantes de R1.** A fixture coloca em A `08:10`, `08:20`, `8h` e `14:00`; R1 espera carga **4**. [app/escala/tests/test_escala_nao_descreve.py:63; app/escala/tests/test_escala_nao_descreve.py:91]

   - `n_orfas = 3 + 1 = 4`, pois soma o comprimento das listas. [app/ponto/services/esmeril_espelho.py:130]
   - `_hm` lê `08:10` e `08:20` na faixa 16, lê `14:00` na faixa 28 e retorna `None` para `8h`; assim, `a5 = {16: 2, 28: 1}`. [app/ponto/services/esmeril_espelho.py:31; app/ponto/services/esmeril_espelho.py:132]
   - Portanto, `len(a5) = 2` e `sum(a5.values()) = 3`: ambos divergem do 4 exigido pelo teste, que recebe a medida pela lavra. [app/ponto/services/esmeril_espelho.py:142; app/ponto/services/esmeril_espelho.py:235; app/escala/tests/test_escala_nao_descreve.py:88]

   O diff anexo relata uma falha para cada mutante e restauração do arquivo, mas os logs dessa execução não foram fornecidos: **rodada efetiva [não verificado]**. [anexo `delta.diff`, mensagem do commit] [nao verificado]

2. **Escritor e formato.** O cartório monta `ata['orfas']` com `str(h)` para cada elemento de `dia['orfas']`, passa a ata à operação de lavra e a porta atribui a nova ata à `CelulaDia`. [app/ponto/services/cartorio.py:730; app/ponto/services/cartorio.py:745; app/ponto/services/cartorio.py:1852; app/ponto/portas/celula.py:1049; app/ponto/portas/celula.py:1090]

   Nos caminhos de montagem examinados, as horas vêm de `strftime('%H:%M')`, inclusive as horas de células que depois viram órfãs; logo, **`'8h'` não é forma gerada por esses caminhos**, embora `ata_do_dia` isoladamente aceite e converta qualquer valor recebido. Dados antigos, escritos por outro caminho ou corrompidos: **[não verificado]**. [app/escala/utils.py:621; app/escala/utils.py:662; app/escala/utils.py:1722; app/escala/utils.py:326; app/escala/utils.py:1832; app/ponto/services/cartorio.py:745]

   Duas órfãs legíveis na mesma faixa fazem `len(a5)` diferir de `n_orfas`, **mas não** fazem `sum(a5.values())` diferir: cada órfã lida incrementa o contador uma vez. Para a soma diferir é necessário ao menos um elemento que `_hm` não leia; não identifiquei outra forma produzida pelo caminho normal examinado. [app/ponto/services/esmeril_espelho.py:130; app/ponto/services/esmeril_espelho.py:135]

   O selo pode manter o caso defensivo `'8h'` para a regra literal de contar elementos, mas deve identificá-lo como caso sintético; não há, na evidência examinada, uma forma normal de produção comprovada para substituí-lo e ainda matar esse mutante. [app/escala/tests/test_escala_nao_descreve.py:5; app/ponto/services/esmeril_espelho.py:130]

3. **Outros testes do módulo.** A nova fixture tem apenas **duas** órfãs na faixa 16 e uma na faixa 28; A5 exige ao menos três na mesma faixa, portanto não cria A5 para A. Isso preserva as pré-condições de R1 e R3 de que A não tenha assinatura nem proposta, quanto a essa alteração de A5. [app/escala/tests/test_escala_nao_descreve.py:63; app/ponto/services/esmeril_espelho.py:148; app/escala/tests/test_escala_nao_descreve.py:89; app/escala/tests/test_escala_nao_descreve.py:105] Resultado efetivo dos demais testes: **[não verificado]**.

4. **`HistoricoVinculo.de`.** `executar_vinculo` fotografa o vínculo ativo anterior como `código desde data · turno data-ou-"-"`, passa esse texto a `registrar_mudanca`, que cria `HistoricoVinculo.de`. [app/colaboradores/services/vinculo.py:182; app/colaboradores/services/vinculo.py:349; app/colaboradores/services/historico_vinculo.py:7]

   Na fixture de R4, X começa no template de código `ENDL-5`; portanto, o texto anterior esperado contém `ENDL-5`, como a nova asserção exige. [app/escala/tests/test_escala_nao_descreve.py:127; app/escala/tests/test_escala_nao_descreve.py:135; app/escala/tests/test_escala_nao_descreve.py:140; app/escala/tests/test_escala_nao_descreve.py:288]

5. **Classificação dos itens do veredito**, considerando o literal reproduzido no próprio anexo; o texto integral do aval é **[não verificado]**:

   - **Item 3 — FILA.** Há leitores separados da classificação de ciclo, mas o anexo caracteriza o leitor anterior como herdado e a obra como acrescentando outro leitor; isso, por si só, não demonstra violação da proibição literal de *juiz novo*. [app/escala/services/cadastro_realidade.py:849; app/escala/regua_defesa.py:313; app/colaboradores/services/vinculo.py:128; anexo `veredito.md`, itens 2–3]
   - **Item 4 — BLOQUEIA o selo de R5.** O caso declarado exige que o POST direto seja recusado pela porta, mas o teste só verifica que não houve vínculo; o POST retorna no predicado da view antes de chamar `realocar_vinculo_para_destino`. Assim, não prova a recusa da porta indicada em R4/R5. [app/escala/tests/test_escala_nao_descreve.py:12; app/escala/tests/test_escala_nao_descreve.py:15; app/escala/tests/test_escala_nao_descreve.py:299; app/escala/views.py:498; app/escala/views.py:502; app/escala/regua_defesa.py:313]
   - **Item 6 — FILA**, com risco real de proposta antiga: o leitor busca o último snapshot sem limite de idade e a view usa sua proposta para aplicar a partir de hoje; o anexo não reproduz exigência literal de validade temporal. Existência ou efeito de lavra vencida em produção: **[não verificado]**. [app/ponto/services/esmeril_espelho.py:371; app/escala/views.py:493; app/escala/regua_defesa.py:315; anexo `veredito.md`, item 6]
   - **Item 7 — BLOQUEIA por divergência literal pendente de aceite.** Segundo o anexo, o aval inclui `ciclo` no mapa e manda o link ao wizard; o código omite `ciclo` e aponta à tela de vínculo. A justificativa técnica para ler o ciclo atual não comprova alteração do aval. [anexo `veredito.md`, item 7; app/ponto/services/esmeril_espelho.py:245; app/escala/services/cadastro_realidade.py:13; app/escala/tests/test_escala_nao_descreve.py:299]
   - **Item 8 — FILA**, pelo que é verificável: a L-040 ainda cita o selo antigo, e a L-042 tem a coluna `PROTEGE` vazia; o portão cobra citação quando o diff toca função declarada nessa coluna. Não há prova disponível de uma exigência literal do aval que transforme esses dois apontamentos em bloqueio desta obra. Resultado do portão sobre o range de pouso: **[não verificado]**. [app/docs/LEIS.md:88; app/docs/LEIS.md:90; bin/tests/test_lei_protege_sitio.sh:13; bin/tests/test_lei_protege_sitio.sh:200]

## 18/11 17:25 · sessao c8d57d9f8e0f · LOTE-CONHECE-A-IRMA · sonda · origem=Code

Na saída raspada fornecida:

- Universo de perguntas de intervalo-hora na fila: **550**. [não verificado]
- Em `b_conversa` por conflito: **72**. [não verificado]
- Com conflito somente de batida de irmã gravada: **23**. [não verificado]
  - Com exatamente uma irmã gravada: **23**. [não verificado]
  - Com mais de uma irmã gravada, no contador de irmã ambígua: **0**. [não verificado]
- Erros: **0**. [não verificado]

A conferência do método da sonda ficou **não verificada**: o arquivo `logs/RAIA-INTERVALO/sonda_irma_ambigua.py` não foi encontrado na cópia. [não verificado]

No código, `classe_do_lote` só marca conflito depois de passar pelas verificações anteriores e usa batidas apuráveis próximas ao instante do slot. [validacao.py](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/validacao.py:1177) Já `metade_irma_do_intervalo` procura uma única irmã respondida **ainda não materializada**, com filtros adicionais. [materializacao.py](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:139) Isso não permite confirmar que o método da sonda, que relata irmãs gravadas, corresponda ao método pretendido ou detectar eventual divergência. [não verificado]

## 18/11 17:24 · sessao df1a716c32a1 · RAIA-INTERVALO · sonda · origem=Code

Na saída fornecida, **7 perguntas** estão em `espera_declarado_x_gravado`: **7 dentro do U163 e 0 fora**. O contador `respostas_intervalo_recusadas_paridade()['espera_declarado']` também tem **7**; a interseção é **7**, com **0 só na função** e **0 só no contador**. Os IDs informados são 41706, 45598, 45599, 47207, 50176, 50177 e 51526. [não verificado em arquivo:linha; números transcritos da saída fornecida]

Esses conjuntos coincidem **nesta medição**, embora os critérios permitam diferença. O contador começa pelas perguntas do U163 e acrescenta à lista as não carimbadas cujo estado é espera; a função consulta o estado de uma pergunta de intervalo tipo hora sem exigir, ela própria, que o PK consultado pertença ao U163. [materializacao.py:323](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:323), [materializacao.py:341](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:341), [materializacao.py:762](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:762)

O portão examina as metades que seriam gravadas no ato e exige U163 para a **metade divergente**. Quando o ato grava um par, isso permite, em princípio, que a consulta de uma pergunta fora do U163 retorne espera por causa da irmã dentro do U163; essa pergunta ficaria fora do contador, que seleciona pelo próprio PK. Nesta saída, não apareceu nenhum caso assim. [materializacao.py:748](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:748), [materializacao.py:923](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:923), [não verificado em arquivo:linha para o resultado da sonda]

**Erros à parte:** a sonda informa `erros 0` e `ERROS []`; a saída também contém mensagens `ERROR` de `tipo_marco_divergente`, algumas repetidas. Esses registros não devem ser somados às sete esperas nem confundidos com a lista de erros da sonda. [não verificado em arquivo:linha; saída fornecida]

As **79** do aval são identificadas como contagem histórica; a saída atual, por si só, não mostra a interseção nem a evolução entre aquelas 79 e as 7 atuais. Qualquer relação adicional é **[não verificado]**. [RELATO.md:13](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/docs/RELATO.md:13)

## 18/11 17:23 · sessao 9f77027bbad7 · RAIA-INTERVALO · sonda · origem=Code

A sonda fornecida relata **“não” para zero faltantes em todos os 28 dias**. Em nenhum deles ela mostra uma batida ocupando o marco que a ata aponta como faltante. A saída identifica a sonda como `sonda28.py`, mas esse arquivo não foi localizado nesta cópia; os dados abaixo são transcrição do material fornecido e, quanto à execução independente, **[não verificado]**.

Em cada linha: `colaborador data grupo — grade vê 0 faltantes?; marco que a ata dá por apagado; batida que o ocupa (hora, tipo, origem → marco casado)`.

- col622 27/10 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col622 29/10 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col622 04/11 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col622 05/11 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col622 12/11 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col622 13/11 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col622 16/11 G1 — N; S 12:00; nenhuma mostrada. [não verificado]
- col362 22/10 G2 — N; S 12:00; nenhuma mostrada. [não verificado]
- col945 29/10 G2 — N; S 12:00; nenhuma mostrada. [não verificado]
- col735 16/11 G2 — N; S 12:00; nenhuma mostrada. [não verificado]
- col583 12/11 G3 — N; S 11:00; nenhuma mostrada. [não verificado]
- col829 11/11 G3 — N; S 14:40; nenhuma mostrada. [não verificado]
- col369 12/11 G3 — N; S 12:00; nenhuma mostrada. [não verificado]
- col224 28/10 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col469 01/11 G4 — N; E 19:00; nenhuma mostrada. [não verificado]
- col924 11/11 G4 — N; E 19:00; nenhuma mostrada. [não verificado]
- col631 03/11 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col988 31/10 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col988 03/11 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col836 04/11 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col1000 03/11 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col1000 13/11 G4 — N; E 13:00; nenhuma mostrada. [não verificado]
- col962 05/11 G4 — N; E 18:00; nenhuma mostrada. [não verificado]
- col962 09/11 G4 — N; E 18:00; nenhuma mostrada. [não verificado]
- col962 16/11 G4 — N; E 18:00; nenhuma mostrada. [não verificado]
- col991 09/11 G4 — N; E 17:00; nenhuma mostrada. [não verificado]
- col991 11/11 G4 — N; E 17:00; nenhuma mostrada. [não verificado]
- col787 30/10 G5 — N; E 08:00 e E 13:00; nenhuma mostrada para qualquer dos dois. [não verificado]

A saída mostra, por exemplo, que em col369 12/11 as batidas **12:53:12 S/app** e **13:55:44 S/app** casaram com `NENHUM`; em col787 30/10, **10:32:17 E/app** também casou com `NENHUM`. Não há, portanto, marco casado a atribuir a essas batidas no dado fornecido. [não verificado]

**Resposta por grupo:** G1, G2, G3, G4 e G5: **não**, a grade não vê zero faltantes em nenhum dos respectivos dias relatados; e o passo em que uma batida ocuparia o marco apagado é **[não verificado] em todos os grupos**. A saída não mostra essa ocupação e não permite classificá-la como ocorrida dentro ou fora de `escala/utils.py`. [não verificado]

Nesta cópia, o caminho de casamento está em [`_match_marcos`](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/escala/utils.py:595), que pode usar alinhamento por papel ou a rotina de alinhamento; o cartório monta as lâmpadas a partir das células e também contém um ramo de proximidade para marco ausente da régua em [`lampadas_do_dia`](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/ponto/services/cartorio.py:656). O leitor transforma as lâmpadas da ata em células e deriva `missing` de `acesa` em [`_celulas_da_ata`](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/escala/services/leitor_celula.py:114). A existência desses caminhos no código não identifica qual ramo foi executado para cada registro da sonda **[não verificado]**.

## 18/11 17:21 · sessao d73e96b3b221 · LOTE-CONHECE-A-IRMA · sonda · origem=Code

No dado raspado, os contadores são **[não verificado]**:

- Universo de perguntas de intervalo-hora na fila: **550**
- Em `b_conversa` por conflito: **72**
- Conflito só de batida de irmã gravada: **23**
  - Com exatamente uma irmã gravada: **23**
  - Com mais de uma irmã gravada — contador de irmã ambígua: **0**
- Erros: **0**

A conferência do método da sonda ficou **[não verificada]**: o arquivo indicado não está disponível nesta cópia. O código de `classe_do_lote` marca conflito pela existência de qualquer batida apurável no intervalo de 60 minutos em torno do instante do slot, sem identificar sua origem ([validacao.py:1133](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/validacao.py:1133), [validacao.py:1204](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/validacao.py:1204)).

Há uma **diferença de critério** a considerar: `metade_irma_do_intervalo` procura irmã **ainda não materializada** e só retorna uma quando há exatamente uma candidata; isso não equivale diretamente à contagem de irmãs já gravadas ([materializacao.py:139](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:139)). Sem o código da sonda, não é possível confirmar se ela tratou essa diferença nem se as 23 sairiam do conflito pela cura **[não verificado]**.

## 18/11 17:21 · sessao 6fe46adaff6b · - · - · origem=Ronald (tmux)

A classificação `b_conversa` por conflito só ocorre depois de passar pelos testes de resposta legível, marco, AM/PM e envelope; ela consulta batidas apuráveis em até 60 minutos do instante do slot ([validacao.py:1177](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/validacao.py:1177), [validacao.py:1133](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/validacao.py:1133)).

`metade_irma_do_intervalo` procura irmã **ainda não materializada** e só retorna uma candidata única, após outros filtros ([materializacao.py:139](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:139)). Seu critério, portanto, não é diretamente a contagem de irmãs já gravadas.

A sonda indicada não foi encontrada nesta cópia; a comparação direta de seu método com essas funções está [não verificado].

## 18/11 17:21 · sessao 30f63ae7fee3 · - · - · origem=Ronald (tmux)

A função [metade_irma_do_intervalo](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/materializacao.py:139) procura uma irmã **não gravada** e retorna uma candidata somente se houver exatamente uma; também exige resposta com texto e exclui perguntas fora do fato ou em voo. A contagem pedida de irmãs **gravadas** mede outra população.

A função [classe_do_lote](/home/ronald/codex_ws/eeadcfc61a124d908636b8015a590f26708f5e78/app/chamados/services/validacao.py:1204) considera conflito a existência de qualquer batida apurável perto do instante do slot, sem excluir batida de irmã.

O arquivo da sonda indicado não foi localizado na cópia; a comparação do método efetivamente usado pela sonda com essas funções está [não verificado].

## 18/11 17:21 · sessao da741c9eb6ab · - · - · origem=Ronald (tmux)

A sonda `logs/RAIA-INTERVALO/sonda_irma_ambigua.py` não está presente na cópia consultada. Assim, sua metodologia e o hash informado não puderam ser conferidos; a correspondência entre seus contadores e as funções de produção é **[não verificado]**.

_7068 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
