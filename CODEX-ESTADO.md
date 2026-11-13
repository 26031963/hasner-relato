# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 17:32** · publicado em **13/11/2026 17:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 17:32 · sessao be11735563d0 · BUG-PAR · verificador · origem=Code

Reprovo a volta pelo **encerramento indevido após o novo rollback do par**: a irmã é desfeita, mas a pergunta pode receber `fato_ocupado` por essa mesma batida desfeita. [app/chamados/services/materializacao.py:1233](/home/ronald/codex_ws/b7f6efa8b632ac52fa87e5b325ba6d4031dc57b6/app/chamados/services/materializacao.py:1233), `app/chamados/services/materializacao.py:1639`, `app/chamados/services/materializacao.py:1399`.

Suite completa, execução dos mutantes e incidência dos contraexemplos na produção: **[nao verificado]**. [nao verificado]

**1. BLOQUEANTE — o rollback apaga a irmã, mas a colisão com ela encerra a pergunta.**

O novo `_Reprovada` desfaz o plantio da irmã quando a outra metade encontra conflito. Entretanto, conserva `colisao tipo-oposto` na mensagem; fora do savepoint, o `finally` entrega essa mensagem a `_carimbar_recusa`. O catálogo responde `COLISAO`, e a porta grava `VIA_FATO_OCUPADO`, sem exigir uma batida sobrevivente. `app/chamados/services/materializacao.py:1228`, `app/chamados/services/materializacao.py:1625`, `app/chamados/services/materializacao.py:1639`, `app/chamados/catalogo/recusa.py:74`, `app/chamados/services/materializacao.py:1393`, `app/chamados/models.py:3194`.

Contraexemplo concreto:

- Template 19h–07h, intervalo previsto 01h/02h; chão E18h30/S06h30; chamado com marcos faltantes **S01h30/E01h32**; duas respostas já validadas. O plantio usa as horas do chamado, preservadas pelo ajuste noturno; a irmã validada dispensa nova avaliação de implausibilidade. `app/chamados/services/materializacao.py:195`, `app/chamados/services/materializacao.py:645`, `app/chamados/juizes.py:322`.
- A projeção aceita S01h30/E01h32; E01h32 grava primeiro. S01h30 encontra essa E dentro da janela de colisão e fora da tolerância do seu marco próprio 01h00. `app/chamados/services/materializacao.py:219`, `app/chamados/services/materializacao.py:227`, `app/chamados/services/materializacao.py:1433`, `app/ponto/registro_batida.py:21`, `app/ponto/registro_batida.py:35`, `app/ponto/registro_batida.py:145`.
- E01h32 volta no rollback, mas S01h30 fica carimbada como `fato_ocupado`. O RED2b simula **competência fechada**, cuja tradução não produz esse carimbo; portanto não exercita o defeito. `app/chamados/services/materializacao.py:1233`, `app/chamados/services/materializacao.py:1399`, `app/chamados/tests/test_bug_par_projetado.py:672`.

A contraprova em memória, usando a expressão real do novo erro e o corpo real de `_carimbar_recusa`, produziu `COLISAO / fato_ocupado / CARIMBADA`; execução integral com ORM: **[nao verificado]**. `app/chamados/services/materializacao.py:1233`, `app/chamados/services/materializacao.py:1383`.

**2. BLOQUEANTE residual, preexistente — batida retratada ainda decide a paridade.**

`_vizinha_mesmo_tipo` consulta batidas sem excluir `retratada_em`, embora o modelo declare a retratada fora da geometria e apuração. A projeção continua usando essa vizinhança. `app/chamados/services/materializacao.py:108`, `app/chamados/services/materializacao.py:112`, `app/chamados/services/materializacao.py:228`, `app/ponto/models.py:64`.

Contraexemplo: chão vivo E18h30/S00h30/S06h30, **E00h45 retratada**, par pretendido S01h/E02h. A E retratada esconde a S00h30 da guarda; o par pode passar apesar da sequência viva resultante conter S00h30–S01h. No ensaio em memória do corpo real, com a retratada a guarda retornou `None`; retirando-a, retornou a S00h30 ofensora. Plantio integral desse cenário: **[nao verificado]**. `app/chamados/services/materializacao.py:108`, `app/chamados/services/materializacao.py:117`, `app/chamados/services/materializacao.py:121`.

Isso não é atributo faltando na namedtuple: é o universo consultado pela guarda, preservado no delta fornecido. `app/chamados/services/materializacao.py:90`, `app/chamados/services/materializacao.py:108`.

**3. NAO-BLOQ — situação dos sete itens procedentes da triagem.**

| Item | Cura presente no código | Limite encontrado |
|---|---|---|
| **1** | Fora de intervalo e ausência de resposta retornam impedimentos textuais; `_gravar_irma_do_par` os repassa. `app/chamados/services/materializacao.py:186`, `app/chamados/services/materializacao.py:1425` | Não identifiquei contraexemplo restante para o falso “a irmã gravou” nessa saída. Os REDs verificam retorno e recusa. `app/chamados/tests/test_bug_par_projetado.py:589`, `app/chamados/tests/test_bug_par_projetado.py:598` |
| **2** | A testemunha consulta DATA_EM_DISPUTA e a autoridade `competencia_fechada`, inclusive para ambos os instantes do par. `app/chamados/services/materializacao.py:700`, `app/chamados/services/materializacao.py:667`, `app/chamados/services/materializacao.py:718`, `app/chamados/services/materializacao.py:729` | A cura dessas duas guardas está presente; o novo tratamento de conflito contém o bloqueante 1. `app/chamados/services/materializacao.py:1228` |
| **4** | Laço usa `atomic`, helper compartilhado de trava e releitura da pergunta. Lote chama o mesmo helper. `app/chamados/services/materializacao.py:1743`, `app/chamados/services/passivo_validado.py:379` | Corrida ou inversão remanescente causada por essa extração: **[nao verificado]**. |
| **5** | Os dois ramos do modal mostram `aplicavel`; bloqueios de data/competência também aparecem. `app/templates/chamados/partials/modal_fio.html:230`, `app/templates/chamados/partials/modal_fio.html:238`, `app/templates/chamados/partials/modal_fio.html:252` | O teste do ramo validado admite o mutante descrito abaixo. `app/chamados/tests/test_bug_par_projetado.py:731` |
| **6** | Foto inclui `materializada_por_id`; `evento() is None` levanta dentro do `atomic`. `app/chamados/services/validacao.py:264`, `app/chamados/services/validacao.py:285`, `app/chamados/services/validacao.py:288` | O RED confere rollback e ausência de DEVOLUCAO. `app/chamados/tests/test_bug_par_projetado.py:776` |
| **7** | Nome contém empresas/UUID e abertura exclusiva `'x'`. `app/chamados/management/commands/bug_par_103.py:74`, `app/chamados/management/commands/bug_par_103.py:77` | Código curado; abertura exclusiva sem mordida específica. `app/chamados/tests/test_bug_par_projetado.py:794` |
| **9** | Selo exige as quatro tabelas em ordem, incluindo Pergunta, antes da escrita. `app/chamados/tests/test_bug_par_projetado.py:556`, `app/chamados/tests/test_bug_par_projetado.py:710` | Confere tabela/ordem, sem conferir os PKs das duas metades. `app/chamados/tests/test_bug_par_projetado.py:560` |

**4. NAO-BLOQ — dois mutantes óbvios escapam dos REDs novos.**

- **RED5:** apagar somente o bloco `aplicavel` da pergunta validada. O teste valida apenas a saída; a volta continua imprimindo a mesma frase pelo ramo não validado. A busca no HTML inteiro continua satisfeita. `app/chamados/tests/test_bug_par_projetado.py:734`, `app/chamados/tests/test_bug_par_projetado.py:737`, `app/templates/chamados/partials/modal_fio.html:238`, `app/templates/chamados/partials/modal_fio.html:252`.
- **RED7:** trocar `'x'` por `'w'`. Dois UUIDs distintos continuam criando dois arquivos; o teste não provoca colisão de nome. `app/chamados/management/commands/bug_par_103.py:76`, `app/chamados/management/commands/bug_par_103.py:77`, `app/chamados/tests/test_bug_par_projetado.py:803`.

Essas fugas são inferências das fixtures e asserções; execução dos mutantes: **[nao verificado]**. Os REDs dos itens 1, 2, 4/9 e 6 têm asserções diretamente contrárias à retirada das respectivas curas. `app/chamados/tests/test_bug_par_projetado.py:595`, `app/chamados/tests/test_bug_par_projetado.py:627`, `app/chamados/tests/test_bug_par_projetado.py:642`, `app/chamados/tests/test_bug_par_projetado.py:565`, `app/chamados/tests/test_bug_par_projetado.py:779`.

**5. NAO-BLOQ — namedtuple compatível com os leitores encontrados; brancos não retiram respostas do contador final.**

A tupla oferece `pk`, `colaborador`, `timestamp` e `tipo`. Os consumidores encontrados usam `tipo/timestamp` e, nas mensagens, `pk`; não exigem `.colaborador_id`, `.retratada_em` nem métodos de `Batida`. A hora desses consumidores passa por `hora_local`, que converte datetimes aware para o fuso local. `app/chamados/services/materializacao.py:90`, `app/chamados/services/materializacao.py:230`, `app/chamados/services/materializacao.py:1164`, `app/chamados/services/materializacao.py:1174`, `app/core/fmt.py:10`.

O SQL deixa de carregar respostas só de brancos. Contudo, no delta fornecido, `_recusa_e_de_paridade` já exigia `.strip()`; o contador continua aplicando esse predicado antes de contar, e a elegibilidade103 também depende dele. Portanto a exclusão SQL não demonstra mudança do **universo final** das 163. `app/chamados/juizes.py:1520`, `app/chamados/juizes.py:1551`, `app/chamados/services/materializacao.py:261`, `app/chamados/services/materializacao.py:278`, `app/chamados/services/materializacao.py:323`.

A mudança efetiva está em `_situacao_da_irma`: irmã com carimbo de resposta e texto só de brancos passa a `sem_carimbo_nao_respondida`. Isso muda `irma/por_irma`, sem alterar por si o total. `app/chamados/services/materializacao.py:299`, `app/chamados/services/materializacao.py:329`, `app/chamados/services/materializacao.py:337`.

Consumidores: comando lê contador/seleção103; porta reconsulta elegibilidade sob trava; modal lê a testemunha; rótulo103 lê DEVOLUCAO; tarefa do dia usa DEVOLUCAO para devolver a ação ao admin. Contagem efetiva de 163 nesta produção: **[nao verificado]**. `app/chamados/management/commands/bug_par_103.py:44`, `app/chamados/services/validacao.py:259`, `app/chamados/services/fio.py:373`, `app/chamados/services/materializacao.py:285`, `app/chamados/services/tarefa_do_dia.py:141`.

**6. NAO-BLOQ — registro operacional impreciso; linha LEI-AKITA do commit indisponível.** [nao verificado]

`FORA_DE_PIPELINE` mistura o contador163 com o universo operado: o comando chama a devolução somente para `c['bug_par_103']`. Contraexemplo: uma das 60 já sem carimbo integra o contador, mas não recebe essa devolução. `app/config/crons.py:773`, `app/chamados/management/commands/bug_par_103.py:45`, `app/chamados/services/materializacao.py:343`.

Flags reais: `--empresa` repetível, `--autor` STAFF obrigatório no apply, `--apply`; default DRY; reversão gravada antes das chamadas à porta. “Só com aval” é instrução operacional, sem flag de conferência do aval. `app/chamados/management/commands/bug_par_103.py:26`, `app/chamados/management/commands/bug_par_103.py:65`, `app/chamados/management/commands/bug_par_103.py:77`, `app/chamados/management/commands/bug_par_103.py:95`, `app/config/crons.py:774`.

A cópia não disponibilizou histórico Git; conferir a **linha LEI-AKITA do commit b7f6efa8b** ficou **[nao verificado]**. Pelo delta fornecido, `_competencia_que_barra` apenas agrega consultas à autoridade existente; não inventa critério. “Juízes novos=0” é compatível com essa alteração. `app/chamados/services/materializacao.py:667`, `app/ponto/janelas.py:75`.

A busca textual em `CORTES.md` não encontrou nascimento nominal para `_competencia_que_barra`, `par_do_intervalo`, `paridade_absolvida_bug_par_103`, `metade_irma_do_intervalo`, `_data_em_disputa`, `estado_aplicacao_pergunta`, `texto_vazio` ou `q_respondida_com_texto`. Não identifiquei autoridade nova introduzida **neste delta** que exija outra frase; conferência por `git grep`/histórico: **[nao verificado]**. A exigência de corte está em `CLAUDE.md:74`.

**7. DINHEIRO — NAO-BLOQ do delta, mas permanece o caminho preexistente que reescreve exportada.**

Validar agenda recálculo **antes** de materializar; a recusa posterior por competência fechada não desfaz automaticamente esse agendamento. O callback recalcula o fechamento da competência do fato, e o escritor atualiza inclusive os campos calculados do fechamento não aberto. Contraexemplo: validar resposta da comp10 exportada, receber recusa de plantio e ainda executar recálculo de comp10. É o item 3 já classificado FORA na triagem fornecida. `app/chamados/services/validacao.py:544`, `app/chamados/services/validacao.py:555`, `app/chamados/services/validacao.py:570`, `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1160`.

Para comp11 aberta, o caminho do par pode criar batidas e disparar recálculo; para comp10 exportada, a criação administrativa direta continua barrada por `competencia_fechada`. Mudança efetiva de **entra/fora, rubrica, valor ou batida**, por competência, no dado de produção fornecido: **[nao verificado]**. `app/chamados/services/materializacao.py:1220`, `app/ponto/registro_batida.py:132`, `app/ponto/registro_batida.py:157`.

O `--apply`103 diretamente descarimba a pergunta e escreve decisão/auditoria. Entretanto, seu `save` também acorda beco e outbox; por isso não há base para prometer ausência absoluta de efeitos indiretos sobre o gravado. `app/chamados/services/validacao.py:268`, `app/chamados/services/validacao.py:273`, `app/chamados/services/validacao.py:276`, `app/chamados/signals.py:229`, `app/chamados/signals.py:252`.

Contraexemplo condicional: elegível103 em disputa aberta, pai resolvido, dia trancado e nenhuma outra pendência; o beco fecha a disputa, cujo sinal retoma regularização externa confirmada. Essa regularização chama `registrar_batida` sem `fato_de_chao=False`; o default deriva `True`, podendo atravessar a guarda de competência e recalcular. Existência desse conjunto na frota: **[nao verificado]**. `app/chamados/models.py:1847`, `app/chamados/juizes.py:1405`, `app/chamados/services/becos.py:33`, `app/chamados/services/becos.py:57`, `app/chamados/signals.py:139`, `app/chamados/services/regularizacao_ext.py:225`, `app/chamados/services/regularizacao_ext.py:78`, `app/ponto/registro_batida.py:130`.

**8. NAO-BLOQ — arquivo de reversão pode conter uma foto anterior à efetivamente removida.**

O comando fotografa sem trava; a porta trava e relê depois. Contraexemplo: elegível103 com `validada_em=None`; depois da foto, outro admin valida; a devolução remove essa validação, ausente no JSON. A auditoria registra o antes correto, mas o arquivo de reversão isolado fica desatualizado. Ocorrência real: **[nao verificado]**. `app/chamados/management/commands/bug_par_103.py:48`, `app/chamados/management/commands/bug_par_103.py:77`, `app/chamados/services/validacao.py:259`, `app/chamados/services/validacao.py:263`, `app/chamados/services/validacao.py:271`, `app/chamados/services/validacao.py:544`.

**9. NAO-BLOQ — mapa dos campos e significados sobrepostos.**

| Campo | Quem escreve | Quem lê no caminho examinado |
|---|---|---|
| `resposta_colab` | Resposta humana e automática; reabertura limpa. `app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1595` | Derivação, elegibilidade103 e classificação da recusa. `app/chamados/services/materializacao.py:595`, `app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:261` |
| `respondida_em` | Resposta humana/automática; reconstrução pela trilha; carimbo com texto; reabertura limpa. `app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1660`, `app/chamados/services/disputa_emissao.py:1681`, `app/chamados/models.py:3156`, `app/chamados/services/disputa_emissao.py:1595` | Validação, C6, fila e contador. `app/chamados/services/validacao.py:495`, `app/chamados/juizes.py:1400`, `app/chamados/services/materializacao.py:1733`, `app/chamados/juizes.py:1556` |
| `materializada_em` | Setter do carimbo, chamado por carimbar/identidade/descarimbar. `app/chamados/models.py:3123`, `app/chamados/models.py:3155`, `app/chamados/models.py:3211`, `app/chamados/models.py:3234` | Tela, contador, fila e C6. `app/chamados/services/materializacao.py:688`, `app/chamados/services/materializacao.py:325`, `app/chamados/services/materializacao.py:1735`, `app/chamados/juizes.py:1444` |
| `via_resolucao` | Mesmo setter; relavra também altera a via. `app/chamados/models.py:3127`, `app/chamados/services/relavra_via.py:125` | Elegibilidade103, tela e C6. `app/chamados/services/materializacao.py:276`, `app/chamados/services/materializacao.py:692`, `app/chamados/juizes.py:1438` |
| `recusa_motivo` | Escape, materializador, recusa humana e duplicata. `app/chamados/services/disputa_emissao.py:1497`, `app/chamados/services/materializacao.py:1402`, `app/chamados/services/validacao.py:705`, `app/chamados/services/fio.py:1399` | Catálogo de paridade, seleção e foto103. `app/chamados/services/materializacao.py:262`, `app/chamados/services/materializacao.py:251`, `app/chamados/services/validacao.py:266` |
| DEVOLUCAO | Porta103 chama a porta única da sequência. `app/chamados/services/validacao.py:276`, `app/chamados/models.py:4029` | Rótulo103 e tarefa do admin. Histórico de respostas não a inclui. `app/chamados/services/materializacao.py:285`, `app/chamados/services/tarefa_do_dia.py:141`, `app/chamados/models.py:3599`, `app/chamados/models.py:3955` |

`materializada_em` significa também **encerramento sem plantio**; `respondida_em` não prova fala humana; `recusa_motivo` reúne impedimento técnico, escape, rejeição humana e duplicata. Esses significados já coexistem e exigem leitura pelas autoridades próprias. `app/chamados/services/materializacao.py:689`, `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1497`, `app/chamados/services/validacao.py:705`, `app/chamados/services/fio.py:1399`.

Não encontrei segundo escritor de Batida introduzido pela tupla: o plantio continua em `registrar_batida`, e a allowlist desse gate permanece vazia. A ampliação do selo da validação nomeia a função exata da devolução, sem liberar o módulo inteiro. `app/chamados/services/materializacao.py:90`, `app/chamados/services/materializacao.py:1219`, `app/ponto/tests/test_chokepoint_batida_gate.py:21`, `app/chamados/tests/test_porta_validacao_pergunta.py:82`.

## 13/11 17:31 · sessao 46c38e4be95e · BO-[nome]-1311-ROTEIRO · sonda · origem=Code

**Saída integral da sonda: não disponível.** O conteúdo recebido termina dentro da FICHA 05, sem o aval literal, sem `sonda_roteiro.py` e sem os casos 06–10. Não executei outra sonda. A execução e a raspagem pertencem ao envelope anterior ao Codex; o resultado chega pelo texto do pedido. (`bin/codex_pergunta.sh:15`, `bin/codex_pergunta.sh:325`; anexos ausentes [nao verificado])

As três mensagens de erro presentes no fragmento recebido são estas, reproduzidas integralmente; não constituem a saída inteira da sonda. [nao verificado]

```text
2026-11-13 17:24:34,808 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=46757 resposta=16:01 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 17:24:34,921 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=55375 resposta=14:30 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 17:24:34,933 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=55411 resposta=15:50 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
```

**O roteiro abaixo é parcial.** As linhas baseadas no texto raspado recebem `[nao verificado]` porque não existe arquivo local correspondente. A correspondência entre `item` e posição no UNIVERSO do aval não pode ser conferida sem o aval; nos casos 01–04, FICHA e EXTRA concordam entre si quanto a `item` e `colab`. [nao verificado]

O juiz da retenção é `motivos_retencao_celula` → `_dias_sem_decisao`: célula acusadora retém, salvo as exceções dos protestos não cobráveis e da pausa. A exceção da pausa lê `periodos_do_dia`, exige ausência de desalinhamento, período aberto e dúvida, e exige que todos os papéis apagados sejam pausa. (`app/folha/export.py:1212`, `app/folha/export.py:1273`, `app/folha/export.py:1293`, `app/folha/export.py:1148`, `app/folha/export.py:1173`)

`estado_aplicacao=aplicavel` não mede esse resultado posterior: a escrita ainda pode sofrer rollback pelo juiz de lastro e pelo carimbo C6. Portanto, nenhuma pergunta abaixo recebe **(a) VALIDA-HOJE** apenas por apresentar `aplicavel`. (`app/chamados/services/materializacao.py:449`, `app/chamados/services/materializacao.py:1238`, `app/chamados/services/materializacao.py:1283`)

## Caso 01 -- col1019

A sonda fornecida mostra **zero perguntas vivas e zero dias retidos na comp 11**. As três perguntas históricas estão sem declaração, com `estado_aplicacao=null`, materializadas por `celula_absolveu`, na disputa 7877 fechada. [nao verificado]

Há seis abonos aprovados, em 03, 05, 07, 09, 10 e 12/11; não há batidas listadas. O chamado 37062 permanece aberto em cadastro, subtipo `nunca_bateu`. Não há dia retido a liberar neste caso. Print e comportamento observado na tela: [nao verificado]

## Caso 02 -- col996

**Não conclui os itens afetados pelos três ERRORs:** perguntas 46757, 55375 e 55411. As classes abaixo indicam encaminhamento; não certificam sucesso de validação. [nao verificado]

Todas as 16 perguntas vivas abaixo pertencem à **disputa 7263 aberta**. Tipo de resposta: `hora`. Os tipos E/S vêm do motivo e da projeção informada, conforme identificado na tabela. [nao verificado]

| Pergunta | Dia | Declarado | `estado_aplicacao` fornecido | Classe única |
|---|---|---|---|---|
| 46751 | 20/10 | entrada 07:50 | `bloqueada_paridade`: existe E 07:00 | **(c) DECISAO DO ADMIN** [nao verificado] |
| 46756 | 21/10 | intervalo saída 15:00 | `aplicavel`: E 15:00 | **(b) ESPERA OBRA DECLARADO-X-GRAVADO** [nao verificado] |
| 46757 | 21/10 | intervalo volta 16:01 | `aplicavel`: E 14:30; ERROR | **(b) ESPERA OBRA DECLARADO-X-GRAVADO** [nao verificado] |
| 46758 | 21/10 | saída 21:05 | `aplicavel`: S 21:05 | **nao medido**: retirada da retenção após o ato [nao verificado] |
| 48773 | 22/10 | intervalo saída `null` | `null` | **(d) CADASTRO**: colab informa a saída efetiva da pausa [nao verificado] |
| 57211 | 22/10 | intervalo volta `null` | `null` | **(d) CADASTRO**: colab informa a volta efetiva [nao verificado] |
| 52035 | 30/10 | saída 15:50 | `aplicavel`: S 15:50 | **nao medido**: retirada da retenção após o ato [nao verificado] |
| 53565 | 03/11 | intervalo volta `null` | `null` | **(d) CADASTRO**: colab informa a volta efetiva [nao verificado] |
| 53644 | 03/11 | saída `null` | `null` | **(d) CADASTRO**: colab informa a saída efetiva [nao verificado] |
| 53920 | 04/11 | intervalo saída 13:01 | `ja_satisfeita`: S 12:54; espelho não muda | **(c) DECISAO DO ADMIN** [nao verificado] |
| 55313 | 08/11 | entrada 07:00 | `ja_satisfeita`: E 06:58; espelho não muda | **(c) DECISAO DO ADMIN** [nao verificado] |
| 55375 | 08/11 | intervalo saída 14:30 | `aplicavel`: S 13:00; ERROR | **(b) ESPERA OBRA DECLARADO-X-GRAVADO** [nao verificado] |
| 55411 | 08/11 | intervalo volta 15:50 | `aplicavel`: E 14:30; ERROR | **(b) ESPERA OBRA DECLARADO-X-GRAVADO** [nao verificado] |
| 56923 | 11/11 | intervalo volta `null` | `null` | **(d) CADASTRO**: colab informa a volta efetiva [nao verificado] |
| 57210 | 11/11 | intervalo saída 13:00 | `aplicavel`: S 13:00 | **nao medido**: resultado posterior; dia já não retém [nao verificado] |
| 57214 | 12/11 | saída `null` | `null` | **(d) CADASTRO**: colab informa a saída efetiva [nao verificado] |

A obra **DECLARADO-X-GRAVADO** existe em `app/docs/BACKLOG.md:1936`; o passivo da comp 11 tem **DECLARADO-X-GRAVADO-PASSIVO-11**, em `app/docs/BACKLOG.md:1937`. O código substitui a hora respondida de intervalo pela hora do marco faltante; o tipo prioriza o marco do chamado e registra divergência com a ata. (`app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:302`, `app/chamados/services/materializacao.py:409`)

Nas decisões administrativas, é necessário decidir qual horário deve prevalecer: a batida existente ou uma correção fundamentada. O modal do fio oferece **Rejeitar resposta**, com motivo, e **Corrigir**, com hora e motivo; a correção chama novamente a validação e pode ser recusada. Isso não certifica a liberação do dia. (`app/templates/chamados/partials/modal_fio.html:292`, `app/templates/chamados/partials/modal_fio.html:303`, `app/chamados/services/acoes_disputa.py:334`, `app/chamados/services/acoes_disputa.py:379`)

Os únicos dias retidos na comp 11 são estes; perguntas vivas nos demais dias não acrescentam retenção ao universo. [nao verificado]

| Dia retido | Motivo | O que falta para encaminhar a saída |
|---|---|---|
| 21/10 | `espelho_furo` | Obra para os intervalos divergentes; saída declarada disponível. Liberação posterior não medida; pergunta 46757 com ERROR. [nao verificado] |
| 30/10 | `espelho_furo` | Saída declarada disponível; falta medir a transação completa e o juiz de retenção depois dela. **nao medido**. [nao verificado] |
| 03/11 | `espelho_furo` | Colab informar volta da pausa e saída; depois, decisão/materialização e nova leitura da retenção. **CADASTRO**. [nao verificado] |
| 12/11 | `espelho_furo` | Colab informar a saída; depois, decisão/materialização e nova leitura da retenção. **CADASTRO**. [nao verificado] |

## Caso 03 -- col1032

A sonda fornecida mostra **zero perguntas vivas e zero dias retidos na comp 11**. As nove perguntas históricas têm declarado e `estado_aplicacao` nulos, foram materializadas por `celula_absolveu` e pertencem a disputas fechadas. [nao verificado]

As quatro batidas de 11/11 estão retratadas e não aprovadas; há abono aprovado nesse dia. As quatro batidas de 12/11 estão aprovadas, e a célula está `concorde/ok`. Não há dia retido a liberar. Print e comportamento observado na tela: [nao verificado]

## Caso 04 -- col246

| Pergunta viva | Dia | Tipo/declarado | `estado_aplicacao` | Disputa | Classe única |
|---|---|---|---|---|---|
| 52485 | 01/11 | escolha / `null` | `null` | 7755 **fechada** | **(b) ESPERA OBRA FIO-E0-V4-E3-LEITORES** [nao verificado] |
| 52715 | 01/11 | hora, saída / `null` | `null` | 7755 **fechada** | **(b) ESPERA OBRA FIO-E0-V4-E3-LEITORES** [nao verificado] |

O único dia retido é **01/11**, célula 136215, por `espelho_cobranca_indevida`. O chamado de turno ainda vivo mostrado nesse dia é **36424**, `turno_aberto/em_analise`. A sonda não fornece `chamado_destino`, ata ou protestos: **não prova a ligação exata de uma dessas perguntas ao chamado 36424**, nem qual protesto produziu o veredito. [nao verificado]

O cartório carrega chamados de `FURO_MODULES` por dia; `COBRANCA_SEM_FURO` exige trabalho, realizado suficiente, todas as células casadas e chamado vivo. Esse protesto mapeia para `cobranca_indevida`. (`app/ponto/services/cartorio.py:127`, `app/ponto/services/cartorio.py:135`, `app/ponto/supra_juiz.py:467`, `app/escala/models.py:1613`)

**Por que `celula_absolveu` não encerrou:** `cobranca_indevida` é veredito acusador, e a seleção de perguntas absolvíveis exclui células acusadoras. O sítio que efetivamente carimba a absolvição é `retratar_perguntas_absolvidas_por_celula`, em `p.carimbar(VIA_CELULA_ABSOLVEU)`. O fechamento do chamado também recusa célula acusadora; o ramo separado de órfãs exige disputa aberta. (`app/escala/models.py:1767`, `app/chamados/reconciliador.py:642`, `app/chamados/reconciliador.py:596`, `app/chamados/reconciliador.py:1028`, `app/chamados/reconciliador.py:1050`)

Encaminhamento: complementar **E3-LEITORES**, existente e aberta, com esse fio inconsistente e a medição dos vínculos ausentes. **E4B existe, mas o BACKLOG a registra como pousada**; não cabe apresentá-la como obra aberta. O ato exato que retirará 01/11 da retenção permanece [nao verificado]. (`app/docs/BACKLOG.md:1899`, `app/docs/BACKLOG.md:1921`)

## Caso 05 -- col265

A FICHA termina dentro da lista de células, antes das perguntas, disputas e do EXTRA. Quantidade de perguntas vivas, declarações, estados, disputas e dias retidos: **[nao verificado]**.

O complemento solicitado cabe em **RAIZ-DA-DISPUTA**, existente: trata a relação raiz–filhos, escritores externos ao agregado e disputa que não reage. Não é possível atribuir perguntas ou colab-dias deste caso à obra sem a continuação da sonda. (`app/docs/BACKLOG.md:1931`; aplicação individual [nao verificado])

## Caso 06 -- colNNN [nao verificado]

Identidade, três dias recusados, quatro perguntas vivas e vínculos: **não recebidos**. Não há classificação individual nem contagem deste caso. [nao verificado]

Nesta cópia, `leitor_celula.py::grade_da_celula` é leitor; a recusa encontrada está em `ponto/portas/celula.py`, que pula datas contidas nas janelas fechadas enumeradas. (`app/escala/services/leitor_celula.py:350`, `app/ponto/portas/celula.py:221`, `app/ponto/portas/celula.py:284`)

**Não está provado bug de mês civil nesse sítio.** As janelas vêm de `janela_competencia`, baseada em `Empresa.dia_inicio_competencia`; com corte 21, a comp 10 é 21/09–20/10. Sem as três datas e o cadastro da empresa do caso, seu enquadramento individual permanece [nao verificado]. (`app/ponto/management/commands/gerar_celulas.py:185`, `app/folha/export.py:47`, `app/ponto/janelas.py:50`, `app/colaboradores/models.py:46`)

Há reatividade implementada para rejulgar células tocadas e agendar reconciliação no commit. Existem as obras relacionadas **CONTRATO-20-DOIS-EVENTOS**, **CONTRATO-20-E2-QUAL-ATO** e **E2-D3-TRABALHA-FORA-DA-IMPRESSAO**; escolher qual cobre as quatro perguntas exige os dados ausentes. (`app/ponto/portas/celula.py:460`, `app/ponto/portas/celula.py:528`, `app/docs/BACKLOG.md:1711`, `app/docs/BACKLOG.md:1071`, `app/docs/BACKLOG.md:988`)

## Caso 07 -- colNNN [nao verificado]

A tela efetivamente utilizada e os dados individuais não foram recebidos. [nao verificado]

O código demonstra a diferença: o **detalhe do chamado** exige disputa aberta e pode devolver “Pergunta invalida.”; o **fio do colaborador**, também usado pelo modal, permite validar pergunta viva em disputa fechada com `sem_reabrir=True`. (`app/chamados/views.py:493`, `app/chamados/views.py:500`, `app/chamados/views.py:550`, `app/chamados/services/fio.py:595`, `app/chamados/services/fio.py:600`, `app/chamados/views.py:889`)

O BACKLOG registra **HJ-093**, col1004, pergunta 50873 e célula 143004 de 27/10: distingue a decisão humana sobre a pergunta da obra **JUSTIFICATIVA-PAGA-O-PERIODO**. Isso é registro documental; confirmar que corresponde à posição 07 do aval e ao estado da sonda permanece [nao verificado]. (`app/docs/BACKLOG.md:742`, `app/docs/BACKLOG.md:1872`)

## Caso 08 -- colNNN [nao verificado]

Identidade, perguntas, disputas e retenção não foram recebidas. [nao verificado]

**CONTRATO-DE-ENTRADA-DO-FIO** já existe. O desencontro é comprovado no código: o emissor retroativo entrega `foto_ausente.batida_id`, enquanto o nascimento procura `ultima_batida.id`; sem episódio, a geração deduplica por disputa/motivo. Cabe complementar essa obra, sem novo id. (`app/docs/BACKLOG.md:2100`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`, `app/chamados/models.py:3464`, `app/chamados/services/disputa_emissao.py:325`)

## Caso 09 -- colNNN [nao verificado]

Identidade, perguntas, tipos declarados e dias retidos não foram recebidos. [nao verificado]

A família existente inclui **TIPO-NA-FONTE** e **TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO**, esta em fila 2. (`app/docs/BACKLOG.md:1833`, `app/docs/BACKLOG.md:741`)

**O ocultamento dos horários de dia só com saídas é comprovado no template:** a view `calendario_ponto` monta o contexto e renderiza o calendário; o bloco de horários exige `dia.entradas or dia.retratadas`, e só dentro dele percorre `dia.batidas`. Assim, somente S, sem retratadas, não imprime os horários. Cabe complemento de UI na fila 2; associação aos dias deste caso [nao verificado]. (`app/colaboradores/views.py:1938`, `app/colaboradores/views.py:1949`, `app/colaboradores/views.py:1965`, `app/templates/colaboradores/partials/_calendario_grade.html:118`, `app/templates/colaboradores/partials/_calendario_grade.html:121`)

## Caso 10 -- colNNN [nao verificado]

Identidade, intervalos, perguntas, disputas e retenção não foram recebidos. O destino documental pedido é **DECLARADO-X-GRAVADO**, com passivo da comp 11 em **DECLARADO-X-GRAVADO-PASSIVO-11**; aplicação individual e classificação permanecem [nao verificado]. (`app/docs/BACKLOG.md:1936`, `app/docs/BACKLOG.md:1937`)

## Contagem parcial por classe

Universo efetivamente disponível para esta contagem: **casos 01–04**, com 18 perguntas vivas e cinco colab-dias retidos. Perguntas históricas encerradas não entram na contagem de vivas. [nao verificado]

| Classe | Perguntas vivas | Colab-dias retidos |
|---|---:|---:|
| (a) VALIDA-HOJE | 0 | 0 |
| (b) ESPERA OBRA | 6 | 2 |
| (c) DECISAO DO ADMIN | 3 | 0 |
| (d) CADASTRO | 6 | 2 |
| nao medido | 3 | 1 |
| **Total parcial** | **18** | **5** |

Conferência parcial: perguntas **0 + 16 + 0 + 2 = 18**; dias retidos **0 + 4 + 0 + 1 = 5**. Os encaminhamentos de obra não representam itens concluídos, especialmente nas três perguntas com ERROR. [nao verificado]

**A soma do universo dos dez casos não pode ser conferida:** faltam o restante do caso 05, os casos 06–10 e o aval literal. Não há base para completar esses números ou declarar o roteiro integral concluído. [nao verificado]

## 13/11 17:29 · sessao fd5d6d89c280 · - · - · origem=Ronald (tmux)

**NAO-BLOQ — reversão não prova exclusividade sob colisão.** O código usa UUID e abertura `'x'`, curando a sobrescrita; porém o mutante `'x' → 'w'` escapa ao RED porque os UUIDs continuam distintos. Falta fixar UUID e provocar colisão. `app/chamados/management/commands/bug_par_103.py:74`; `app/chamados/tests/test_bug_par_projetado.py:794`.

**NAO-BLOQ — arquivo de reversão pode fotografar estado anterior ao efetivamente removido.** O comando fotografa sem trava, grava o JSON e depois chama a porta, que trava e relê. Contraexemplo: pergunta elegível com `validada_em=None`; após a foto, outro admin valida; a devolução remove essa validação nova, ausente do JSON. O LogAuditoria registra o antes correto, permitindo recuperação pela trilha. `app/chamados/management/commands/bug_par_103.py:48`; `app/chamados/management/commands/bug_par_103.py:77`; `app/chamados/services/validacao.py:259`; `app/chamados/services/validacao.py:263`; `app/chamados/services/validacao.py:544`. Incidência na frota: [nao verificado].

**DINHEIRO — ressalva preexistente ao delta.** Validar posteriormente agenda recálculo mesmo quando a materialização será recusada; o recálculo escreve também fechamento não aberto. Contraexemplo: resposta da competência 10 exportada, materialização recusada por competência fechada, callback ainda recalcula seu `FechamentoMensal`. Isso corresponde ao item 3 já declarado FORA na triagem fornecida. `app/chamados/services/validacao.py:544`; `app/chamados/services/validacao.py:555`; `app/ponto/services/fechamento.py:1807`; `app/ponto/services/fechamento.py:1160`. Variação efetiva de valor/rubrica/entra-fora na produção: [nao verificado].

**DINHEIRO — não afirmar ausência absoluta de efeitos indiretos do `--apply` 103.** A escrita da devolução acorda beco no `on_commit` e emite outbox. Um elegível em disputa aberta, pai resolvido e dia trancado pode deixar a disputa fechável; seu fechamento retoma regularizações externas confirmadas. Estas registram batidas com `autoridade_humana=False` sem explicitar `fato_de_chao=False`, fazendo o chokepoint derivar `True`, pular competência e disparar recálculo. Contraexemplo estático: regularização confirmada pendente da competência 10 para esse colaborador. Cadeia: `app/chamados/services/validacao.py:273`; `app/chamados/signals.py:229`; `app/chamados/services/becos.py:25`; `app/chamados/models.py:1847`; `app/chamados/juizes.py:1405`; `app/chamados/services/becos.py:57`; `app/chamados/signals.py:122`; `app/chamados/services/regularizacao_ext.py:219`; `app/chamados/services/regularizacao_ext.py:78`; `app/ponto/registro_batida.py:130`; `app/ponto/registro_batida.py:157`. Existência desse conjunto na produção: [nao verificado].

**Itens 6 e 7: cura direta demonstrável por leitura.** A foto agora inclui `materializada_por_id`; `evento() is None` levanta dentro do atomic e desfaz descarimbo, validação e decisão; arquivo usa nome único e abertura exclusiva. `app/chamados/services/validacao.py:263`; `app/chamados/services/validacao.py:285`; `app/chamados/services/validacao.py:288`; `app/chamados/management/commands/bug_par_103.py:74`.

Mapa de campos:

| Campo | Escritores concretos | Leitores relevantes |
|---|---|---|
| `resposta_colab` / `respondida_em` | Resposta real `disputa_emissao.py:1467`; limpeza `:1595`, aplicada `:1634`; resposta automática `:1659`; reconstrução do carimbo `:1681`; simetria com texto `models.py:3156` | Validação `validacao.py:495`; disputa `models.py:1847`; C6 `juizes.py:1400`; contador `materializacao.py:248`; elegibilidade103 `:274` | [nao verificado]
| `materializada_em` / `via_resolucao` | Setter `models.py:3123`; carimbo `:3155`; identidade `:3211`; descarimbo `:3234`; lote `:3308`; via relavrada `relavra_via.py:125`, `models.py:3317` | Tela `materializacao.py:689`; contador `:325`; fila `:1733`; C6 `juizes.py:1438` |
| `recusa_motivo` | Escape `disputa_emissao.py:1497`; pendência `materializacao.py:1125`; recusa materializadora `:1402`; duplicata `fio.py:1399`; recusa humana `validacao.py:705` | Classificador de paridade `materializacao.py:262`; contador `:251`; foto103 `validacao.py:266`; devolução preserva `:286` |
| `DecisaoPergunta.DEVOLUCAO` | Porta103 `validacao.py:276`; porta única da sequência `models.py:4038` | Rótulo103 `materializacao.py:285`; tarefa do admin `tarefa_do_dia.py:141`; histórico omite por `models.py:3599` e enum `:3955` |

Os caminhos da tabela são relativos a `app/chamados/`. `recusa_motivo` já reúne escape, duplicata, recusa humana e impedimento do materializador; `materializada_em` já representa encerramento por identidade além de plantio, portanto não prova criação de batida. `app/chamados/services/disputa_emissao.py:1497`; `app/chamados/services/fio.py:1399`; `app/chamados/services/validacao.py:705`; `app/chamados/models.py:3194`.

## 13/11 17:29 · sessao 266251db7a54 · - · - · origem=Ronald (tmux)

Caso 04:

- O dado mostra p52485 (`escolha`, declarado `null`, estado `null`) e p52715 (`hora`, declarado `null`, estado `null`) vivas na disputa7755 fechada; ch36424 está `em_analise`, `turno_aberto`, em 01/11; cel136215 retém por `cobranca_indevida`. Esses fatos vieram apenas do texto fornecido, sem arquivo local: [nao verificado].
- **Não dá para provar a FK p52715 → ch36424:** a sonda não contém `chamado_destino`, nem ata/protestos, nem valores de contexto. A correspondência exata e qual código gerou o veredito ficam [nao verificado].
- O cartório carrega apenas `FURO_MODULES`, agrupados por `data_do_chamado`: `app/ponto/services/cartorio.py:127`, `:135`, `:138`; `turno_aberto` está nesse vocabulário, enquanto `batida_fora_escala_grave` não está: `app/chamados/catalogo/furo_modulos.py:15`.
- O ramo que gera `COBRANCA_SEM_FURO` exige trabalho, previsto, realizado ≥ previsto, chamado vivo e todas as células casadas: `app/ponto/supra_juiz.py:467`. O código vira `cobranca_indevida`: `app/escala/models.py:1613`.
- **Por que `celula_absolveu` não alcança as duas enquanto essa célula acusa:** `cobranca_indevida` pertence a `VEREDITOS_ACUSAM`: `app/escala/models.py:1767`; a seleção de perguntas absolvíveis exclui esses vereditos: `app/chamados/reconciliador.py:638` e `:642`. Mesmo após a seleção, o juiz `pergunta_viva` barra quem continua viva: `app/chamados/reconciliador.py:662`.
- **Sítio que carimba a absolvição:** `retratar_perguntas_absolvidas_por_celula`, laço e `p.carimbar(VIA_CELULA_ABSOLVEU)`: `app/chamados/reconciliador.py:585`, `:596`. O fechamento posterior da disputa passa por `pode_fechar`/`fechar`: `:612`, `:613`.
- O ramo separado de órfãs só seleciona disputa aberta: `app/chamados/reconciliador.py:1049`; portanto não alcança p52715 enquanto sua disputa permanece fechada. A aplicação à p52715 depende do dado fornecido: [nao verificado].
- O fechamento do chamado por absolvição também exclui célula acusadora: `app/chamados/reconciliador.py:1027`, `:1028`. O consumidor chama primeiro o carimbo das perguntas e depois a retratação dos chamados: `:1366`, `:1369`.
- A ordem prevista para chamados de marcos satisfeitos é lavrar → E2 → reconciliar: `app/ponto/services/cartorio.py:1001`, `:1006`, `:1011`; a seleção dos marcos que não protestam e o adiamento ficam em `:1533`, `:1534`.
- Complemento existente **FIO-E0-V4-E3-LEITORES**, aberto no texto do BACKLOG: `app/docs/BACKLOG.md:1899`. **FIO-E0-V4-E4B** existe em `:1921`, mas está descrito como **pousado**, portanto não chamá-lo de obra aberta. Escopo E4b: backfill do fio morto pela porta canônica.

Caso 05:

- **RAIZ-DA-DISPUTA**: `app/docs/BACKLOG.md:1931`, raiz ← filhos, escritores fora do agregado e disputa que não reage; fases F0–F7, incluindo F2=BO-DISPUTA e F4=BUG-PAR.
- **RAIZ-DA-DISPUTA-F0F1**: `app/docs/BACKLOG.md:1909`, três sítios de destino em duas funções, três eleitores divergentes; o registro diz que o bug segue até F2.
- Como a FICHA05 foi cortada dentro de `celulas`, não há perguntas, disputas, EXTRA/retenção nem vínculos suficientes para atribuir pk ou classes individuais: [nao verificado].

Caso 08:

- **CONTRATO-DE-ENTRADA-DO-FIO** já existe: `app/docs/BACKLOG.md:2100`; fila depois da E3 e comboio DESLIGADO/FERIADO, cura no emissor entregando episódio, selo AST `FAMILIA_BATIDA`, passivo pela porta.
- Código comprova o desencontro: emissor retroativo passa `contexto['foto_ausente']['batida_id']`: `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`, `:98`, `:100`.
- O nascimento só procura `contexto['ultima_batida']['id']` na família BATIDA; ausente, devolve `None`: `app/chamados/models.py:3461`, `:3464`, `:3465`.
- Sem episódio, a geração deduplica por disputa/motivo: `app/chamados/services/disputa_emissao.py:325`, `:327`, `:328`.
- Nenhum dado individual do caso08 foi recebido: colNNN, pk, perguntas, disputa e retenção [nao verificado].

## 13/11 17:29 · sessao f6a1bffa0071 · - · - · origem=Ronald (tmux)

**Mapa 06 — guarda da regeneração**

- Nesta cópia, `app/escala/services/leitor_celula.py` termina na linha 451 e só oferece leitura da célula; `grade_da_celula` começa em `app/escala/services/leitor_celula.py:350`. Não encontrei nesse arquivo recusa de regeneração. O ponto real da recusa é `app/ponto/portas/celula.py:284`: se `any(a <= d <= z for a, z, _m, _a in _exp)`, incrementa os barrados e pula o dia em `:285-288`.
- `_exp` vem de `_competencias_exportadas` em `app/ponto/portas/celula.py:221`; o enumerador consulta `competencia_fechada_para_celula` em `app/ponto/management/commands/gerar_celulas.py:182`, deriva `ini,fim` com `janela_competencia(emp, mes, ano)` em `:185`, e devolve as janelas em `:188`.
- `janela_competencia` delega à fonte única em `app/folha/export.py:47-51`; ela usa `Empresa.dia_inicio_competencia` por `app/ponto/janelas.py:50-57`. Campo/default 21 e significado 21→20: `app/colaboradores/models.py:46`.
- **Não há bug provado de mês civil nesse sítio:** a guarda compara a data com as janelas reais. Quais foram os três dias recusados, se eram da comp 10, e qual autoridade os lavrou: **[nao verificado]**, pois caso 06 não foi recebido.
- A reatividade já existe: `_o_chao_do_dia_mudou` rejulga as células tocadas em `app/ponto/portas/celula.py:460-472`; resolve competência por `janela_atual`, `:485-496`; agenda rejunção e reavaliação de faltas espúrias no commit em `:528-540`. Essa última trata `batida_ausente` por `app/chamados/services/feriado_reavaliar.py:65-85`.
- Obras relacionadas existentes: `CONTRATO-20-DOIS-EVENTOS`, `app/docs/BACKLOG.md:1711`; `CONTRATO-20-E2-QUAL-ATO`, `:1071`; `E2-D3-TRABALHA-FORA-DA-IMPRESSAO`, `:988` (insumo `trabalha` fora da impressão e alguns caminhos de regeneração sem rejulgamento). Classificar as quatro perguntas do caso 06 contra qualquer uma dessas obras exige os vínculos/perguntas que faltam: **[nao verificado]**.

**Mapa 07 — telas divergentes**

- A tela de **detalhe do chamado** filtra a pergunta exigindo disputa aberta em `app/chamados/views.py:493-496`; recusa com **“Pergunta invalida.”** em `:499-500`. Render dessa rota: `app/chamados/views.py:549-550`, template `chamados/detalhe_local.html`.
- O **fio do colaborador** usa outra condição: carrega a pergunta sem filtrar disputa aberta em `app/chamados/services/fio.py:595-597`; se disputa fechada, permite validar pergunta que `pergunta_viva` considera viva em `:600-609`, com `sem_reabrir=True`. View do fio e POST compartilhado: `app/chamados/views.py:659-674`; template `chamados/fio_colaborador.html` em `:697`. Modal usa o mesmo serviço: `app/chamados/views.py:869-889`.
- Portanto há divergência demonstrável de código para **viva em disputa fechada**; afirmar qual tela foi efetivamente clicada no caso 07 fica **[nao verificado]** sem o respectivo dado.
- O BACKLOG registra o caso de `col1004`, pergunta `50873`, em `app/docs/BACKLOG.md:742`: resposta sem veredito → decisão humana pela O168; cadastro de isenção pelo DP; dia 27/10, célula 143004, realizado 0 e justificativa 3331 aprovada sem saída informada.
- A obra `JUSTIFICATIVA-PAGA-O-PERIODO` está em `app/docs/BACKLOG.md:1872`, explicitamente incluindo `col1004`/143004/27/10. Ela é sobre pagamento do período coberto; não substitui a decisão administrativa da pergunta, registrada separadamente em `:742`.

**Mapa 09 — família TIPO e calendário que oculta somente saídas**

- IDs existentes: `TIPO-NA-FONTE`, `app/docs/BACKLOG.md:1833`; `TIPO-ITEM3-NAOBLOQ-V5C`, `:1834`; `TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO`, `:741` (fila 2, registrada, nada construído).
- **Bug de apresentação provado:** view `calendario_ponto`, `app/colaboradores/views.py:1938`, chama `contexto_calendario` em `:1949` e renderiza `colaboradores/calendario_partial.html` em `:1965`. O serviço separa entradas e saídas em `app/colaboradores/services/calendario.py:658-659`. Porém o template `app/templates/colaboradores/partials/_calendario_grade.html:118` abre o bloco de horários somente com `dia.entradas or dia.retratadas`; o loop que imprime todas as batidas fica dentro dessa guarda em `:121`. Assim, dia com somente S e nenhuma retratada não imprime os horários.
- Encaixe: complemento de UI, fila 2, conforme pedido; associação ao colaborador/dias/perguntas do caso 09 **[nao verificado]**, pois os dados não chegaram.

## 13/11 17:29 · sessao 3f16d859193a · - · - · origem=Ronald (tmux)

Leitura estrita dos contratos:

| Fato/caso | Cobertura escrita e decisão demonstrável |
|---|---|
| Fila residual, nenhuma pergunta, ação `perguntar/sistema/emissor` | `test_rota_so_fila_sem_pergunta`: FUTURO sem porta, fila vazia; exige esses fatos completos. (`app/chamados/tests/test_e3_portas_tarefa.py:32–38`) |
| A residual na célula; pergunta ancorada em B fora dela | `test_fila_A_pergunta_sob_B`: FUTURO, rota da pergunta, somente B; exige identidade/localização da âncora e porta B. (`app/chamados/tests/test_e3_portas_tarefa.py:21–47`) |
| Verbo de B no universo do painel | Deve preservar exatamente o verbo do painel; não reparticionar só A. (`app/chamados/tests/test_e3_portas_tarefa.py:49–55`) |
| Âncora da própria célula | Deve publicar `porta_fila` e o chamado da ação. (`app/chamados/tests/test_e3_portas_tarefa.py:67–73`) |
| `responder/colaborador` com verbo `validar` | É combinação expressamente admitida no caso GPS; não prova inconsistência entre ação e verbo. Identificar esse caso estrito exige `modulo_origem`, ausente do anexo. (`app/chamados/tests/test_e3_portas_tarefa.py:75–82`) |
| Isenção com fila residual | FUTURO sem porta/ações; exige `isento_ponto`. (`app/chamados/tests/test_e3_portas_tarefa.py:57–65`) |
| Relógio/silêncio | Os testes exigem `silenciado_ate` e instantes explícitos; esses fatos individuais faltam no anexo. (`app/chamados/tests/test_e3_portas_tarefa.py:84–111`) |
| Demais combinações de razões | **Sem caso escrito nesse arquivo** para a combinação integral; razões e pares `[ação,de]` não substituem âncora, porta e pertença na fila. (`app/chamados/tests/test_e3_portas_tarefa.py:21–127`; `app/chamados/services/tarefa_do_dia.py:128–142,547–565`) |

A ação `responder/colaborador` pode existir com `porta=None`: a tarefa usa a âncora apenas quando ela pertence à fila e a publicação exige porta inteira presente na alimentação global. Há inclusive teste E2 que espera essa ação sem porta durante silêncio. (`app/chamados/services/tarefa_do_dia.py:128–133,547–565`; `app/chamados/tests/test_e2_tarefa_do_dia.py:396–409`)

Portanto, os A2 com ação humana e FUTURO sem porta não demonstram, isoladamente, regressão normativa nem ATUAL obrigatório. Falta o fato da âncora exigido pelos testes. (`app/chamados/tests/test_e3_portas_tarefa.py:40–47,67–73`; `app/chamados/services/tarefa_do_dia.py:128–133,548`)

Todos os A2entra apresentados têm porta no ATUAL; logo **nenhum desses registros demonstra “dia que C17 não alcançava”**. Não cabe repartir automaticamente os 79 em regressão versus universo novo: há uma terceira classe necessária, “perda de porta observada, obrigação normativa não demonstrada”. (`a2_entram_saem.txt:1–79`; `app/chamados/juizes.py:1674–1688`)

Campos novos relativos ao resumo V3:

| Campo/fato | Escrita | Leitura e significado |
|---|---|---|
| `razao=sem_pergunta`; ação `perguntar/sistema` | Marco sem pergunta recebe porta `emissor`. (`app/chamados/services/tarefa_do_dia.py:293–304`) | Publicação calcula fio `sistema`, mas não publica porta humana por uma string. Backfill reconhece a ação como `perguntar`. (`app/chamados/services/tarefa_do_dia.py:547–556`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:100–113`) |
| `devolvida_ao_colaborador` | Último ato `RECUSA` ou `REABERTURA`; ação responder. (`app/chamados/services/tarefa_do_dia.py:139–140`) | Publicação lê ação/porta, não a razão. Backfill agrega razões para diagnóstico. (`app/chamados/services/tarefa_do_dia.py:547–556`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:116–117`) |
| `materializacao_recusada` | Último ato `DEVOLUCAO`; ação validar/admin. (`app/chamados/services/tarefa_do_dia.py:141–142`) | Mesmo caminho de publicação; existe teste E2 explícito dessa volta ao admin, fora do arquivo T solicitado. (`app/chamados/services/tarefa_do_dia.py:547–556`; `app/chamados/tests/test_e2_tarefa_do_dia.py:221–229`) |
| `lampada=None`, `vencido=None` em instância fora da grade | A tarefa cria linha sintética com ambos `None`, depois consulta `expor_admin`. (`app/chamados/services/tarefa_do_dia.py:243–255,314–323`) | **Significado adicional:** essa linha não tem slot do marco; seus `None` não percorrem os testes `lampada_nao_sabe` e `marco_fora_do_turno`, que pertencem ao laço anterior. (`app/chamados/services/tarefa_do_dia.py:256–278,314–323`) |
| `lampada` de marco normal | Leitor preserva `acesa_lavrada`; tarefa copia. (`app/escala/services/leitor_celula.py:119–137`; `app/chamados/services/tarefa_do_dia.py:87–105,258–260`) | `True` dispensa; `None` indica desconhecimento. O campo `missing` do leitor trata `None` como acesa, mas não é o campo consumido pela tarefa. (`app/escala/services/leitor_celula.py:119–137`; `app/chamados/services/tarefa_do_dia.py:262–278`) |
| `vencido` de marco normal | Cartório calcula instante + 30 minutos ≤ agora; carga repassa e tarefa casa por tipo/hora. (`app/ponto/services/cartorio.py:862,882–893`; `app/ponto/turnos.py:3060–3066`; `app/chamados/services/tarefa_do_dia.py:74–105`) | `None` significa teto não casado; `False` significa ainda não vencido. A dispensa por dia sem trabalho/lâmpada acesa precede essa leitura. (`app/chamados/services/tarefa_do_dia.py:262–282`) |

As instâncias fora da grade mencionadas no anexo incluem `134521` (`batida_fora_escala_grave`, validação), `139432` (mesmo motivo, passivo) e `129565` (intervalos sem slot). O ponto semântico relevante é a construção sintética, não uma conversão de `None` em lâmpada apagada. (`a1v4_det.txt`, registros desses pks; `app/chamados/services/tarefa_do_dia.py:314–323`)

## 13/11 17:28 · sessao b727d621d5f7 · - · - · origem=Ronald (tmux)

1. **BLOQUEANTE — colisão com irmã desfeita encerra a pergunta por fato inexistente.** O novo ramo desfaz a irmã quando o chokepoint recusa a outra metade, mas mantém no erro a marca `colisao tipo-oposto`; o `finally` transforma essa marca em `COLISAO` e carimba `VIA_FATO_OCUPADO`, fora do savepoint desfeito. Referências: `app/chamados/services/materializacao.py:1228`, `app/chamados/services/materializacao.py:1638`, `app/chamados/catalogo/recusa.py:74`, `app/chamados/services/materializacao.py:1393`, `app/chamados/models.py:3194`.

   Contraexemplo: template 19h–07h, intervalo previsto 01h/02h; chão E18h30/S06h30; chamado com marcos faltantes S01h30/E01h32; duas respostas já validadas. A projeção aceita o par; a irmã E01h32 grava; S01h30 colide com ela; o novo `raise` desfaz E01h32, mas o `finally` encerra S01h30 por `fato_ocupado`. As horas do chamado prevalecem no plantio (`app/chamados/services/materializacao.py:645`); overnight conserva hora/minuto (`app/chamados/juizes.py:322`); a irmã não exige nova avaliação de implausibilidade quando já validada (`app/chamados/services/materializacao.py:195`); ambos os horários estão fora da exceção de cinco minutos do marco próprio (`app/ponto/registro_batida.py:35`). Execução do cenário: [nao verificado].

2. **NAO-BLOQ — RED5 não prova o ramo da pergunta validada.** Apagar somente o ramo `aplicavel` dentro de `perg.validada_em` deixa a frase ser impressa pela outra metade, ainda não validada; o teste procura a substring no HTML inteiro. Referências: `app/templates/chamados/partials/modal_fio.html:238`, `app/templates/chamados/partials/modal_fio.html:252`, `app/chamados/tests/test_bug_par_projetado.py:731`. Execução do mutante: [nao verificado].

3. **NAO-BLOQ — RED7 não prova abertura exclusiva.** O mutante `'x' → 'w'` mantém dois arquivos porque cada chamada usa UUID diferente; falta forçar colisão do nome para cobrar a proteção contra sobrescrita. Referências: `app/chamados/management/commands/bug_par_103.py:76`, `app/chamados/tests/test_bug_par_projetado.py:794`. Execução do mutante: [nao verificado].

4. **Itens 1, 4 e parte procedente do 9: curados no caminho examinado.** Ausência de par retorna impedimento textual e `_gravar_irma_do_par` o repassa (`app/chamados/services/materializacao.py:186`, `app/chamados/services/materializacao.py:1425`). O laço usa `atomic`, a função compartilhada de trava e releitura da pergunta (`app/chamados/services/materializacao.py:1743`); o lote usa a mesma função (`app/chamados/services/passivo_validado.py:379`). O selo agora exige `FOR UPDATE` das quatro tabelas antes da escrita (`app/chamados/tests/test_bug_par_projetado.py:556`, `app/chamados/tests/test_bug_par_projetado.py:699`). Omitir todas as travas de Pergunta é detectável por essa asserção; execução: [nao verificado].

## 13/11 17:28 · sessao 5a63087c342f · - · - · origem=Ronald (tmux)

A hipótese está **confirmada apenas para a guarda de escrita**: `recusa_respondida_sem_texto` consulta o predicado em `app/chamados/services/fio.py:1602` e levanta `RespondidaSemTexto` em `:1604`; essa classe herda `ResolvedoraCruzada` (`app/chamados/models.py:2325`), que herda `ValueError` (`:2293`). O predicado de leitura `respondida_sem_texto` retorna somente booleano; ele não levanta essa exceção. (`app/chamados/juizes.py:1515–1518`)

| Caminho que chama a guarda | Localização | Consequência no código |
|---|---|---|
| Nascimento de pergunta | `app/chamados/models.py:2864–2867` | Recusa antes da trava e do restante do nascimento. |
| Resposta automática | `app/chamados/services/disputa_emissao.py:1653–1664` | Recusa antes de alterar a instância, salvar e registrar a decisão. |
| `PerguntaDisputa.save` | `app/chamados/models.py:3471–3477,3502–3509` | Recusa transição do par para estado ilegal antes do `super().save`. Outro campo ou mesmo par já carregado pode passar. |
| `descarimbar_em_lote` | `app/chamados/models.py:3288–3298,3306–3308` | Recusa o par pedido antes do `update`. |

**A exceção de escrita não serve apenas para contar: ela impede a escrita correspondente.** Os testes verificam estado preservado após a recusa. (`app/chamados/tests/test_e3_10_cura_no_escritor.py:155–177,193–206`)

Não há captura local nesses caminhos de nascimento e resposta automática: `_sob_contrato` deixa a exceção atravessar o `atomic`; `responder_automaticamente` também a propaga. (`app/chamados/models.py:2840–2843`; `app/chamados/services/disputa_emissao.py:1657–1665`) As capturas nominadas encontradas são de testes: `app/chamados/tests/test_e3_item1b.py:193–199` e os `assertRaises` citados acima. Os `except ResolvedoraCruzada` de `app/chamados/juizes.py:2284,2297` cercam guardas de destino/batida, não essa guarda. (`app/chamados/juizes.py:2282–2285,2291–2298`)

**Atribuir as +9626 exceções à classe `RespondidaSemTexto`, identificar quem as captura na sonda ou afirmar que só alteram sua conta permanece [não verificado].** A cópia não contém o código da sonda `e3i` nem a implementação de `excecoes_registradas`; o anexo não fornece classes nem stack traces. [não verificado]

A leitura da tarefa possui uma razão textual homônima, sem levantar exceção: última decisão `RESPOSTA` com texto vazio produz `razao='respondida_sem_texto'` e ação `responder/colaborador`. Isso pode alterar as portas publicadas pela tarefa; não demonstra que a exceção de escrita ocorreu. (`app/chamados/services/tarefa_do_dia.py:135–138,547–556`)

O agregado **26→28 não compara o mesmo identificador de leitor**: o anexo registra `vigia.censo_alcance::observado::alimentar_portas` na base e `vigia.censo_alcance::observado::carga.tarefa` na cópia. Use referência às linhas correspondentes do anexo `a2_excecoes_por_leitor.txt`; não atribuir o delta à mesma função. [não verificado quanto à execução; fato textual fornecido no anexo]

## 13/11 17:28 · sessao 5fca96ae5aae · - · - · origem=Ronald (tmux)

- **Obra existente:** `DECLARADO-X-GRAVADO`, [app/docs/BACKLOG.md:1936](app/docs/BACKLOG.md:1936); passivo da comp 11 tem obra própria `DECLARADO-X-GRAVADO-PASSIVO-11`, [app/docs/BACKLOG.md:1937](app/docs/BACKLOG.md:1937).

- **Causa concreta:** `_ts_tipo_do_plantio` começa com a resposta livre, mas, para `intervalo_saida`/`intervalo_volta`, substitui-a pela hora do marco faltante quando encontra esse marco. Isso explica por código o declarado diferente do gravado; o sítio exato é [app/chamados/services/materializacao.py:409](app/chamados/services/materializacao.py:409), especialmente linhas 412–416. O tipo vem primeiro do marco do chamado; a ata é segunda fonte. Quando discordam, prevalece o chamado e registra-se `tipo_marco_divergente`; [app/chamados/services/materializacao.py:274](app/chamados/services/materializacao.py:274), [app/chamados/services/materializacao.py:302](app/chamados/services/materializacao.py:302).

- **`aplicada` não comprova declarado gravado:** `estado_aplicacao_pergunta` devolve `aplicada` para pergunta com resposta e `materializada_em`, salvo a via especial de paridade; não compara nessa saída a resposta com uma batida concreta. [app/chamados/services/materializacao.py:438](app/chamados/services/materializacao.py:438)

- **`aplicavel` não comprova VALIDA-HOJE:** a projeção deriva tipo/hora, testa batida existente e paridade, e devolve `aplicavel`; não executa a transação nem calcula retenção posterior. [app/chamados/services/materializacao.py:449](app/chamados/services/materializacao.py:449) A escrita ainda passa pelo juiz de lastro, que pode provocar rollback, e pelo carimbo C6, que também pode recusar e desfazer a batida. [app/chamados/services/materializacao.py:1238](app/chamados/services/materializacao.py:1238), [app/chamados/services/materializacao.py:1283](app/chamados/services/materializacao.py:1283)

- **Juiz real da saída da retenção:** `motivos_retencao_celula` chama `_dias_sem_decisao`; veredito acusador retém, salvo protestos todos não cobráveis ou `nao_retem_pela_pausa`. [app/folha/export.py:1212](app/folha/export.py:1212), [app/folha/export.py:1273](app/folha/export.py:1273), [app/folha/export.py:1293](app/folha/export.py:1293) A exceção da pausa exige protesto exatamente `FURO_PARCIAL`; consulta `periodos_do_dia`, recusa desalinhamento, período aberto e `nao_sei`, e exige que todos os papéis apagados sejam pausa. [app/folha/export.py:1148](app/folha/export.py:1148), [app/folha/export.py:1173](app/folha/export.py:1173) Protestos/papéis e retenção **depois** de cada validação não aparecem no dado recebido: **nao medido** para sustentar classe a. [nao verificado]

- **02:** contei 16 perguntas `viva=true`: `46751, 46756, 46757, 46758, 48773, 57211, 52035, 53565, 53644, 53920, 55313, 55375, 55411, 56923, 57210, 57214`. São fatos do texto fornecido, sem arquivo local correspondente. [nao verificado]

- **Divergências visíveis em 02:** `46756` declarado 15:00, projeção E15:00; `46757` declarado 16:01, projeção E14:30; `55375` declarado 14:30, projeção S13:00; `55411` declarado 15:50, projeção E14:30. Três destas perguntas (`46757`, `55375`, `55411`) emitiram ERROR na leitura fornecida; pelo critério do pedido, os itens afetados não concluem. [nao verificado]

- **02 — retenção fornecida:** quatro colab-dias na comp 11: 21/10, 30/10, 03/11, 12/11; todos `espelho_furo`. Nenhum dos outros dias com pergunta viva deve ser somado como retido só por haver pergunta. [nao verificado]

- **10:** não veio no texto herdado, que termina no meio da FICHA 05; perguntas, intervalos e retenção do caso 10 permanecem [nao verificado].

## 13/11 17:27 · sessao f3d816f50d59 · - · - · origem=Ronald (tmux)

- **NAO-BLOQ — registro do comando é impreciso.** Diz devolver “respostas recusadas por paridade (contador 163)”, mas o comando seleciona exclusivamente `c['bug_par_103']`; as 60 sem carimbo integram `total_aval`, sem entrarem nessa devolução. Contraexemplo: uma das 60 aparece no contador e não recebe chamada à porta. Fontes: `app/config/crons.py:772`, `app/chamados/management/commands/bug_par_103.py:44`, `app/chamados/services/materializacao.py:343`.

- **NAO-BLOQ — RED7 não prova abertura exclusiva.** O teste verifica dois nomes diferentes, sem forçar colisão. O mutante `'x' → 'w'` conserva nomes diferentes por UUID e não tem asserção que o distinga; execução do mutante [nao verificado]. Fontes: `app/chamados/tests/test_bug_par_projetado.py:794`, `app/chamados/tests/test_bug_par_projetado.py:808`, `app/chamados/management/commands/bug_par_103.py:76`, `app/chamados/management/commands/bug_par_103.py:77`.

- **Defeito residual preexistente, potencialmente BLOQUEANTE — vizinhança consulta batida retratada.** As consultas não filtram `retratada_em`, embora o modelo declare retratada fora da geometria. Contraexemplo sintético: E18:30/S06:30 vivas, S01:30 retratada e par pretendido S01/E02. A S retratada bloqueia a projeção de S01; o par apurável seria E-S-E-S. Execução [nao verificado]; o defeito não foi introduzido pelo delta fornecido. Fontes: `app/chamados/services/materializacao.py:108`, `app/chamados/services/materializacao.py:112`, `app/chamados/services/materializacao.py:228`, `app/ponto/models.py:64`.

Verificações solicitadas:

- A tupla preserva os campos efetivamente consumidos: `pk`, `tipo`, `timestamp`; não identifiquei leitor de atributo adicional. Há reexportação em `models.py`, sem consumo adicional identificado. A formatação converte timestamp aware para hora local. Fontes: `app/chamados/services/materializacao.py:90`, `app/chamados/services/materializacao.py:230`, `app/chamados/services/materializacao.py:1164`, `app/chamados/services/materializacao.py:1174`, `app/chamados/models.py:4174`, `app/core/fmt.py:10`.

- O SQL passa a excluir somente brancos, mas **o universo final do contador já os excluía**: `_recusa_e_de_paridade` anteriormente usava `strip`, conforme delta fornecido [nao verificado por histórico Git]; atualmente delega ao mesmo `texto_vazio`, e o contador aplica esse predicado antes de contar. `paridade_absolvida_bug_par_103` também já dependia desse predicado. A alteração efetiva é a classificação da irmã: carimbo+brancos passa de `sem_carimbo_respondida` para `sem_carimbo_nao_respondida`. Fontes: `app/chamados/juizes.py:1520`, `app/chamados/juizes.py:1551`, `app/chamados/services/materializacao.py:261`, `app/chamados/services/materializacao.py:278`, `app/chamados/services/materializacao.py:301`, `app/chamados/services/materializacao.py:323`.

- Consumidores: comando lê contador e seleção103; porta reconsulta predicado103 sob trava; tela lê estado de aplicação; rótulo103 lê `DecisaoPergunta.DEVOLUCAO`, sem recalcular elegibilidade. Fontes: `app/chamados/management/commands/bug_par_103.py:44`, `app/chamados/services/validacao.py:259`, `app/chamados/services/materializacao.py:285`, `app/chamados/services/fio.py:373`, `app/templates/chamados/partials/modal_fio.html:230`.

- `_competencia_que_barra` apenas agrega chamadas à autoridade existente `competencia_fechada`; não cria critério novo. `par_do_intervalo` já existia no delta fornecido. Portanto, “juízes novos=0” é defensável **para esse delta**; a linha LEI-AKITA do commit não está disponível na cópia [nao verificado]. A busca textual em `CORTES.md` pelos nomes relevantes não encontrou frase de nascimento [nao verificado por Git]; não identifiquei corte novo exigido por esse agregador. Fontes: `app/chamados/services/materializacao.py:667`, `CLAUDE.md:74`.

- Flags reais: `--empresa` repetível, `--autor` STAFF obrigatório em apply, `--apply`; default DRY. “Só com aval” é instrução operacional do registro, sem flag que verifique aval. Reversão usa UUID, empresas e abertura exclusiva antes de chamar a porta. Fontes: `app/chamados/management/commands/bug_par_103.py:26`, `app/chamados/management/commands/bug_par_103.py:65`, `app/chamados/management/commands/bug_par_103.py:74`, `app/chamados/management/commands/bug_par_103.py:95`, `app/config/crons.py:774`.

## 13/11 17:09 · sessao 234786b384a9 · E3-FORMA-SQL · leitura · origem=Code

**A recontagem fecha em 170 colab-dias: 62 ESTRUTURA, 34 CADASTRO e 74 BATIDA. Há 21 com ação humana e FUTURO=`nada`. Não há, pelos casos escritos solicitados e pelos fatos apresentados, colab-dia demonstrado como ATUAL obrigatório.** (`a1v3_det.txt:1–170`; `app/chamados/tests/test_e3_portas_tarefa.py:21–127`)

Referências abreviadas:

- **D:n** = `a1v3_det.txt:n`, contando somente as linhas `V3_DET` coladas, desde 1.
- **T:n** = [app/chamados/tests/test_e3_portas_tarefa.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/test_e3_portas_tarefa.py):n.
- **S:n** = [app/chamados/services/tarefa_do_dia.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py):n.
- **P:n** = [app/chamados/management/commands/diff_e3_sombra.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py):n.

**1. Grupo × dono L-099**

Contagens calculadas sobre as 170 linhas fornecidas; os donos são os registrados no anexo. (D:1–170)

| Grupo | ESTRUTURA | CADASTRO | BATIDA | None | Total |
|---|---:|---:|---:|---:|---:|
| G2 | 17 | 10 | 41 | 0 | 68 |
| G3 | 18 | 3 | 9 | 0 | 30 |
| G4 | 26 | 16 | 23 | 0 | 65 |
| G8 | 1 | 5 | 1 | 0 | 7 |
| **Total** | **62** | **34** | **74** | **0** | **170** |

**2. Combinações de razões e cobertura pela regra escrita**

O teste “só fila sem pergunta” exige uma instância com ação `perguntar/sistema/emissor`; os testes de porta humana exigem âncora concreta na própria célula ou sob B. O caso de isenção exige `isento_ponto`. Nenhum desses cenários pode ser identificado integralmente apenas pela ausência de porta no anexo. (T:21–47,57–82) [nao verificado]

Nas tabelas, **SC = “sem caso escrito”; escolha ATUAL/FUTURO não determinada por esse arquivo**. As letras seguintes identificam a lacuna:

| Código | Lacuna de cobertura ou fato faltante |
|---|---|
| **F** | Falta caso escrito para a combinação apresentada com fila/porta residual. Não reproduz o teste “sem pergunta + perguntar/sistema/emissor”, nem o de isenção. (T:32–38,57–65) | [nao verificado]
| **H** | Há ação humana de pergunta, mas faltam **porta da ação, identidade/localização da âncora e presença na fila global** para aplicar o caso da própria célula ou A/B. O anexo registra somente `[acao,de]`. (D:1–170; T:21–47,67–82) |
| **U** | Falta caso nesse arquivo para `ausencia_a_decidir`, inclusive sua combinação com `aceso/nao_venceu`. A ação não comprova porta de chamado: o código a cria com porta ausente. (T:21–127; S:114–115,283–291) |
| **C** | Falta caso nesse arquivo para `celula_acusa_sem_pergunta_viva` com `validar/admin`; o código cria essa ação sem porta. (T:21–127; S:329–336) |

As combinações abaixo **preservam a multiplicidade**, desconsiderando apenas a ordem. Exemplo: `A³P` significa três instâncias `aceso` e uma `passivo_sem_linha`. Todas as contagens foram calculadas sobre D:1–170; cada exemplo tem sua linha individual.

Legenda:

`A=aceso`; `P=passivo_sem_linha`; `R=aguarda_resposta`; `N=nao_venceu`; `U=ausencia_a_decidir`; `V=veredito_sem_lavra_vigente`; `E=efeito_sem_lampada`; `D=dia_sem_trabalho`; `C=celula_acusa_sem_pergunta_viva`; `L=aguarda_validacao`; `J=encerrada_pelo_juiz`.

| Grupo G2 — combinação | n | Exemplos de celula_pk | Caso/decisão |
|---|---:|---|---|
| A³P | 17 | 120067 (D:1), 118928 (D:6) | SC/F |
| A²P² | 7 | 119985 (D:2), 108826 (D:56) | SC/F |
| AP | 15 | 140114 (D:21), 117245 (D:55) | SC/F |
| AP³ | 1 | 108035 (D:129) | SC/F |
| P⁴ | 3 | 108622 (D:13), 109494 (D:42) | SC/F |
| A⁴ | 3 | 129083 (D:47), 129090 (D:48) | SC/F |
| A³R | 4 | 118930 (D:7), 118934 (D:11) | SC/H |
| A²R² | 5 | 118932 (D:9), 105773 (D:17) | SC/H |
| AR³ | 2 | 129598 (D:85), 116554 (D:104) | SC/H |
| AP²R | 1 | 118931 (D:8) | SC/H |
| AV | 3 | 140121 (D:22), 140122 (D:23) | SC/F |
| A³V | 1 | 130771 (D:131) | SC/F |
| A³E | 1 | 130929 (D:89) | SC/F |
| U⁴ | 1 | 138643 (D:98) | SC/U |
| U² | 1 | 129575 (D:127) | SC/U |
| CD³ | 1 | 120319 (D:150) | SC/C |
| A³L | 1 | 141817 (D:166) | SC/H |
| AJ | 1 | 142013 (D:167) | SC/F |
| **Total** | **68** | D:1–170 | |

| Grupo G3 — combinação | n | Exemplos de celula_pk | Caso/decisão |
|---|---:|---|---|
| A³R | 2 | 141714 (D:3), 141810 (D:165) | SC/H |
| A³P | 2 | 115432 (D:33), 137166 (D:84) | SC/F |
| A⁴P | 1 | 135811 (D:50) | SC/F |
| A³N | 2 | 132340 (D:34), 127949 (D:130) | SC/F |
| A²N² | 1 | 133374 (D:39) | SC/F |
| AN³ | 1 | 130294 (D:61) | SC/F |
| N⁴ | 2 | 129189 (D:36), 130418 (D:145) | SC/F |
| A⁴ | 5 | 136215 (D:43), 137154 (D:53) | SC/F |
| A² | 11 | 121500 (D:57), 138912 (D:95) | SC/F |
| U² | 1 | 117062 (D:54) | SC/U |
| U⁴ | 1 | 112501 (D:115) | SC/U |
| ANU² | 1 | 127887 (D:159) | SC/U |
| **Total** | **30** | D:1–170 | |

| Grupo G4 — combinação | n | Exemplos de celula_pk | Caso/decisão |
|---|---:|---|---|
| A³P | 12 | 130160 (D:4), 130162 (D:5) | SC/F |
| A²P² | 10 | 118933 (D:10), 108633 (D:14) | SC/F |
| AP² | 1 | 139432 (D:20) | SC/F |
| AP³ | 3 | 111346 (D:40), 122073 (D:142) | SC/F |
| AP | 9 | 119517 (D:28), 115650 (D:35) | SC/F |
| P⁴ | 10 | 140928 (D:26), 104784 (D:27) | SC/F |
| P² | 4 | 116436 (D:52), 124364 (D:101) | SC/F |
| A⁴ | 11 | 140331 (D:29), 130931 (D:90) | SC/F |
| A² | 5 | 135067 (D:46), 110506 (D:58) | SC/F |
| **Total** | **65** | D:1–170 | |

| Grupo G8 — combinação | n | Exemplos de celula_pk | Caso/decisão |
|---|---:|---|---|
| N⁴ | 1 | 131162 (D:30) | SC/F |
| D⁴ | 1 | 116182 (D:41) | SC/F |
| A³P | 1 | 113307 (D:62) | SC/F |
| DP | 2 | 130346 (D:117), 130348 (D:118) | SC/F |
| P⁴ | 2 | 128533 (D:155), 127477 (D:160) | SC/F |
| **Total** | **7** | D:1–170 | |

Não aparecem razões `lampada_nao_sabe` nem `ausencia_aprovada` nestas 170 linhas. Existem instâncias com `lampada=null`, mas nelas a razão apresentada é `passivo_sem_linha`; `null` não autoriza renomear a razão. (D:1–170, particularmente D:20,50,122)

**3. ATUAL obrigatório + dono ESTRUTURA**

**Lista comprovada: vazia.** Isso significa **zero demonstrados**, não prova de que nenhum defeito exista: falta cobertura escrita para as combinações ou o fato da âncora exigido pelos testes. (T:21–127; D:1–170) [nao verificado]

Os **sete ESTRUTURA com ação humana e FUTURO=`nada`** são os seguintes. Eles não podem ser promovidos automaticamente a “ATUAL obrigatório”. (D:8–9,17,98,115,127,150; T:40–47,67–73)

| Grupo | celula_pk — lista completa dessa condição | Código que produz a redução ao FUTURO |
|---|---|---|
| G2 | 118931 (D:8), 118932 (D:9), 105773 (D:17) | A ação recebe âncora somente se ela estiver na fila; publicação exige porta inteira pertencente à fila global. Sem rota, retorna `None`, projetado como `nada`. (S:128–133,547–565; P:49) |
| G2 | 138643 (D:98), 129575 (D:127) | `decidir_ausencia/admin` nasce sem porta; depois passa pela mesma redução. (S:114–115,283–291,547–565; P:49) |
| G2 | 120319 (D:150) | `validar/admin` da acusação sem pergunta nasce sem porta; depois passa pela mesma redução. (S:335–336,547–565; P:49) |
| G3 | 112501 (D:115) | `decidir_ausencia/admin` nasce sem porta; depois passa pela mesma redução. (S:114–115,283–291,547–565; P:49) |

**4. CADASTRO e BATIDA — lista do admin**

| Grupo | CADASTRO | BATIDA | Total admin |
|---|---:|---:|---:|
| G2 | 10 | 41 | 51 |
| G3 | 3 | 9 | 12 |
| G4 | 16 | 23 | 39 |
| G8 | 5 | 1 | 6 |
| **Total** | **34** | **74** | **108** |

Contagens: D:1–170. A separação CADASTRO/BATIDA para pauta, com ESTRUTURA como fatia, está escrita em `app/ponto/services/pautas_dos_discordantes.py:40–41`.

**5. FUTURO=`nada` com ação humana**

**Sim: 21 colab-dias, sendo G2=16, G3=5 e G4/G8=0.** A lista completa é esta. (D:1–170)

| Grupo | Ação/de | celula_pk | Escrita da ação |
|---|---|---|---|
| G2 | responder/colaborador | 118930 (D:7), 118931 (D:8), 118932 (D:9), 118934 (D:11), 105773 (D:17), 106286 (D:78), 129598 (D:85), 116546 (D:103), 116554 (D:104), 116542 (D:112), 103972 (D:114), 105151 (D:148) | S:130–133,308–313 |
| G2 | decidir_ausencia/admin | 138643 (D:98), 129575 (D:127) | S:283–291 |
| G2 | validar/admin | 120319 (D:150) | S:335–336 |
| G2 | validar/admin | 141817 (D:166) | S:135–138,308–313 |
| G3 | responder/colaborador | 141714 (D:3), 141810 (D:165) | S:130–133,308–313 |
| G3 | decidir_ausencia/admin | 117062 (D:54), 112501 (D:115), 127887 (D:159) | S:283–291 |

**Essa observação não demonstra contradição interna da tarefa:** seu `fio` resume os donos das ações, mesmo sem porta; a função posterior `porta_da_tarefa` devolve `None` sem rota, e a projeção da folha chama isso de `nada`. O código declara expressamente que ausência de porta não significa ausência de tarefa. (S:540–565; P:49–59)

Há inclusive teste escrito admitindo `responder/colaborador` com `porta=None` quando o chamado está silenciado. (`app/chamados/tests/test_e2_tarefa_do_dia.py:396–409`)

**6. Quem escreve, quem lê e significado dos campos**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| **razao** | `_tarefa_pura` inicializa e escreve por ramo; `_pela_decisao` fornece as razões relativas à pergunta; `expor_admin` pode substituí-las. (S:118–163,249–260,262–313,318–336) | A publicação da porta não consulta razão de instância. O diff consulta somente a razão **geral da tarefa** para classificar isenção. O backfill reconhece `passivo_sem_linha` e agrega razões para explicar o resultado. (S:547–556; P:63–70; `app/chamados/management/commands/backfill_fio_morto_e4b.py:97–117`) |
| **lampada** | `instancias_do_turno` copia `acesa_lavrada`; `_tarefa_pura` copia para a instância publicada. (S:87–105,258–260) A origem é `ata.lampadas[].acesa`, preservada pelo leitor único. (`app/escala/services/leitor_celula.py:134–137`) A ata é montada pelo cartório. (`app/ponto/services/cartorio.py:672–678,711–738`) | A tarefa distingue `True`, `None` e o ramo restante, normalmente `False`. `True` dispensa antes do teto; `None` produz `lampada_nao_sabe`; depois são examinados os valores de `vencido`. (S:262–282) |
| **vencido** | O cartório calcula `quando + 30 minutos <= agora`; a carga fornece esse resultado, que é casado por tipo/hora e copiado para a instância. (`app/ponto/services/cartorio.py:862,882–893`; `app/ponto/turnos.py:3060–3066`; S:74–105,506–514) | A tarefa interpreta `None` como teto desconhecido e `False` como ainda não vencido. O teste ocorre depois da dispensa por dia sem trabalho/lâmpada acesa. (S:262–282) |
| **acoes** | `_acao` define `{acao,de,porta}`, com porta padrão `None`; decisões, ausência, emissão de pergunta e acusação da célula criam as ações. (S:114–162,283–313,335–336) | `_portas_da_tarefa` reúne as ações, publica somente portas inteiras presentes na fila global e calcula o fio pelos donos. O backfill também lê ações e reconhece humanos sem porta. (S:547–556; `app/chamados/management/commands/backfill_fio_morto_e4b.py:100–115`) |

**`lampada=None` não equivale a `False`.** O leitor único converte `None` em acesa para seu campo `missing`, mas mantém o valor original em `acesa_lavrada`, que é o campo usado pela tarefa. Não há essa conversão na tarefa. (`app/escala/services/leitor_celula.py:119–137`; S:87–105,272–282)

**`vencido` é por instante do marco encadeado no turno, incluindo travessia da meia-noite; não é comparação por dia civil.** `data_civil` é extraída separadamente do instante casado. (`app/ponto/services/cartorio.py:865–893`; S:74–105)

**`porta_da_tarefa` e as projeções do diff não reinterpretam `lampada`, `vencido` ou razão de instância: não leem esses campos.** A redução acontece na publicação da porta e descarta o fio humano quando não há rota. (S:547–565; P:46–59)

Há, porém, **diferença entre descrição e predicado na pauta**: o texto diz “não estão esperando NINGUÉM”, enquanto o código seleciona ausência de porta, condição compatível com os humanos sem porta acima. A folha documenta o significado mais estreito: nenhuma ação humana exigível passa por uma porta da fila. (`app/ponto/services/pautas_dos_discordantes.py:46–56,79–84`; `app/folha/fora_da_folha.py:52–58,77–89`; S:547–565)

## 13/11 17:07 · sessao 04d6f45b7ddb · - · - · origem=Ronald (tmux)

Recontagem exata das 170 linhas `V3_DET`, numeradas desde 1:

| Grupo | ESTRUTURA | CADASTRO | BATIDA | None | Total |
|---|---:|---:|---:|---:|---:|
| G2 | 17 | 10 | 41 | 0 | 68 |
| G3 | 18 | 3 | 9 | 0 | 30 |
| G4 | 26 | 16 | 23 | 0 | 65 |
| G8 | 1 | 5 | 1 | 0 | 7 |
| Total | 62 | 34 | 74 | 0 | 170 |

Fonte: `a1v3_det.txt:1–170` do texto fornecido.

Combinações abaixo usam **conjunto de razões distintas**, sem distinguir multiplicidade.

| Grupo | Razões | n | Exemplos |
|---|---|---:|---|
| G2 | aceso + passivo_sem_linha | 40 | 120067 (:1), 119985 (:2) |
| G2 | aceso + aguarda_resposta | 11 | 118930 (:7), 118932 (:9) |
| G2 | aceso + passivo_sem_linha + aguarda_resposta | 1 | 118931 (:8) |
| G2 | passivo_sem_linha | 3 | 108622 (:13), 109494 (:42) |
| G2 | aceso + veredito_sem_lavra_vigente | 4 | 140121 (:22), 140122 (:23) |
| G2 | aceso | 3 | 129083 (:47), 129090 (:48) |
| G2 | aceso + efeito_sem_lampada | 1 | 130929 (:89) |
| G2 | ausencia_a_decidir | 2 | 138643 (:98), 129575 (:127) |
| G2 | celula_acusa_sem_pergunta_viva + dia_sem_trabalho | 1 | 120319 (:150) |
| G2 | aceso + aguarda_validacao | 1 | 141817 (:166) |
| G2 | aceso + encerrada_pelo_juiz | 1 | 142013 (:167) |
| G3 | aceso + aguarda_resposta | 2 | 141714 (:3), 141810 (:165) |
| G3 | aceso + passivo_sem_linha | 3 | 115432 (:33), 135811 (:50) |
| G3 | aceso + nao_venceu | 4 | 132340 (:34), 133374 (:39) |
| G3 | nao_venceu | 2 | 129189 (:36), 130418 (:145) |
| G3 | aceso | 16 | 136215 (:43), 137154 (:53) |
| G3 | ausencia_a_decidir | 2 | 117062 (:54), 112501 (:115) |
| G3 | aceso + nao_venceu + ausencia_a_decidir | 1 | 127887 (:159) |
| G4 | aceso + passivo_sem_linha | 35 | 130160 (:4), 130162 (:5) |
| G4 | passivo_sem_linha | 14 | 140928 (:26), 104784 (:27) |
| G4 | aceso | 16 | 140331 (:29), 135067 (:46) |
| G8 | nao_venceu | 1 | 131162 (:30) |
| G8 | dia_sem_trabalho | 1 | 116182 (:41) |
| G8 | aceso + passivo_sem_linha | 1 | 113307 (:62) |
| G8 | dia_sem_trabalho + passivo_sem_linha | 2 | 130346 (:117), 130348 (:118) |
| G8 | passivo_sem_linha | 2 | 128533 (:155), 127477 (:160) |

Todas as referências `:n` acima são `a1v3_det.txt:n`. Não aparecem razões `lampada_nao_sabe` nem `ausencia_aprovada` nas 170 linhas. (`a1v3_det.txt:1–170`)

Ações humanas com FUTURO `nada`: **21 colab-dias**, G2=16, G3=5, G4=G8=0. (`a1v3_det.txt:1–170`)

| Grupo / ação | Lista completa |
|---|---|
| G2 responder | 118930 (:7), 118931 (:8), 118932 (:9), 118934 (:11), 105773 (:17), 106286 (:78), 129598 (:85), 116546 (:103), 116554 (:104), 116542 (:112), 103972 (:114), 105151 (:148) |
| G2 decidir_ausencia | 138643 (:98), 129575 (:127) |
| G2 validar | 120319 (:150), 141817 (:166) |
| G3 responder | 141714 (:3), 141810 (:165) |
| G3 decidir_ausencia | 117062 (:54), 112501 (:115), 127887 (:159) |

Desses, **7 têm dono ESTRUTURA**: G2=118931 (:8), 118932 (:9), 105773 (:17), 138643 (:98), 129575 (:127), 120319 (:150); G3=112501 (:115). Isso identifica fatos e donos; não determina que ATUAL seja correto. (`a1v3_det.txt:8–150`, referências individuais na lista)

Lista admin CADASTRO+BATIDA: G2=51, G3=12, G4=39, G8=6, total=108. (`a1v3_det.txt:1–170`)

## 13/11 17:06 · sessao fa8f1832d687 · - · - · origem=Ronald (tmux)

Leituras e escritores encontrados, todos na cópia:

| Campo de instância | Quem escreve | Quem lê e significado |
|---|---|---|
| `lampada` | `app/chamados/services/tarefa_do_dia.py:104–105` copia o valor de `acesa_lavrada`, obtido em `:87–91`. O leitor único copia `ata.lampadas[].acesa` diretamente em `app/escala/services/leitor_celula.py:134–137`. A ata é montada em `app/ponto/services/cartorio.py:730–738`, com valores True/None/False em `:672–678,711–724`; o teto pode trocar False por None em `:907–917`. | A tarefa distingue True (`tarefa_do_dia.py:262–271`), None (`:272–275`) e False que segue ao teto (`:276–282`). O outro campo `missing` do leitor trata None como acesa (`leitor_celula.py:119–120`), mas **a tarefa usa `acesa_lavrada`**, preservando o triestado (`:134–137`). |
| `vencido` | `app/ponto/services/cartorio.py:892–893`: `quando + timedelta(minutes=teto_min) <= agora`, com default de 30 min em `:862`. A carga usa esse corpo em `app/ponto/turnos.py:3060–3066`; a tarefa chama com `self.agora` e copia o resultado em `tarefa_do_dia.py:506–514,104–105`. | `tarefa_do_dia.py:276–282`: None = marco sem teto conhecido; False = ainda não venceu. O cálculo usa **instante do marco encadeado no turno**, não comparação do dia civil (`cartorio.py:865–868,882–893`). |
| `razao` | Inicialização e cópia em `tarefa_do_dia.py:258–260`; ramificações escrevem em `:263,273,277,281,285–290,296,303,309`. `_pela_decisao` fornece razões de pergunta em `:130–163`. Exposição administrativa escreve em `:249–255`; instâncias adicionais em `:318–323`; `celula_acusa_sem_pergunta_viva` em `:335–336`. | Não determina a publicação da porta: `_portas_da_tarefa` não a consulta (`:547–556`). Leitor operacional adicional `app/chamados/management/commands/backfill_fio_morto_e4b.py:97–99` reconhece `passivo_sem_linha`; `:116–117` agrega razões para explicar ausência de ato. O diff usa apenas **razão da tarefa**, para reconhecer isenção (`diff_e3_sombra.py:70`). |
| `acoes` | Formato e default de porta em `tarefa_do_dia.py:114–115`; decisões em `:130–162`; ausência em `:286–291`; perguntar em `:300,304`; cópia da decisão em `:308–313`; exposição admin em `:253–255`; acusação sem pergunta em `:335–336`. | `_portas_da_tarefa` achata ações em `:547`, publica só porta inteira ∩ fila global em `:548–551`, e calcula **fio da tarefa pelos donos das ações** em `:552–556`. Backfill lê ações e reconhece humanos sem porta em `backfill_fio_morto_e4b.py:100–115`. |

**Ação humana com FUTURO=`nada` não significa que o fio interno da tarefa seja `nada`.** A tarefa calcula `fio='colab'/'admin'` pelos donos mesmo sem porta inteira; a projeção posterior pode descartá-lo ao retornar None. Isso é explícito no comentário “ausência de porta não diz ausência de tarefa”. (`app/chamados/services/tarefa_do_dia.py:540–556`)

Cadeia exata que produz esse resultado:

1. `responder/validar` só recebem âncora inteira se ela estiver na fila particular carregada: `tarefa_do_dia.py:128–140`; a carga dessa fila está em `:415–418`.
2. `decidir_ausencia/admin` pode nascer diretamente com porta None: `:114–115,283–291`.
3. `celula_acusa_sem_pergunta_viva` produz `validar/admin` com porta None: `:335–336`.
4. Só `porta` inteira pertencente a `alimentacao['fila_ids']` entra na publicação: `:547–548`.
5. Sem essas portas, `rota=None`; o fio interno ainda preserva o humano: `:552–556`.
6. `porta_da_tarefa` devolve None quando a rota é None: `:559–565`.
7. O diff converte None em folha `fio='nada'`, vigia `fio=None/exige_admin=False`, beco `sem_porta=True`, pauta `sem_fio=True`: `app/chamados/management/commands/diff_e3_sombra.py:46–59`.

**Porta e projeções não reinterpretam `lampada`, `vencido` nem razão por instância:** elas não leem esses campos. A redução perde essas informações e também o fio humano quando não há rota. (`tarefa_do_dia.py:547–565`; `diff_e3_sombra.py:46–59`)

Há diferença entre a descrição textual da pauta — “não estão esperando NINGUÉM” — e o predicado executado, que testa ausência de porta por meio de `_fio_do_dia`. Esse predicado admite ações humanas sem porta. (`app/ponto/services/pautas_dos_discordantes.py:46–56,79–84`; `app/folha/fora_da_folha.py:77–89`; `tarefa_do_dia.py:283–291,547–565`)

A folha documenta a interpretação mais estreita: nenhuma ação exigível **passa por uma porta da fila**, e seu texto de `nada` diz que alguém precisa olhar. (`app/folha/fora_da_folha.py:52–58`)

## 13/11 17:05 · sessao c41b2ea51ab4 · - · - · origem=Ronald (tmux)

Em `test_e3_portas_tarefa.py`, nenhum caso cobre explicitamente as combinações `aceso`, `nao_venceu`, `lampada_nao_sabe`, `passivo_sem_linha`, `veredito_sem_lavra_vigente`, `ausencia_aprovada`, `ausencia_a_decidir`, `dia_sem_trabalho` ou `encerrada_pelo_juiz` observadas no V3; esse arquivo escreve os casos abaixo. (`app/chamados/tests/test_e3_portas_tarefa.py:21–127`)

| Caso escrito | Premissas necessárias e resultado |
|---|---|
| Só fila sem pergunta | Marco apagado, nenhuma pergunta, ação `perguntar/sistema/emissor`; rota ausente e fila vazia. FUTURO nesses campos, desde que as premissas sejam comprovadas. (`app/chamados/tests/test_e3_portas_tarefa.py:32–38`) |
| A residual, pergunta sob B | Âncora B viva fora da célula; ação responder com `porta=b.pk`; rota da pergunta, fio colab e verbo cobrar de B. (`app/chamados/tests/test_e3_portas_tarefa.py:21–30,40–47`) |
| Âncora da própria célula | Pergunta ancorada no chamado da célula, publicado na fila; rota da fila, fio colab. (`app/chamados/tests/test_e3_portas_tarefa.py:67–73`) |
| Isento | `isento_ponto=True`, razão geral `isento_ponto`, nenhuma ação/rota/fila/verbo e fio nada. As razões gerais vazias do V3 não reproduzem esse caso. (`app/chamados/tests/test_e3_portas_tarefa.py:57–65`) |
| Verbo independente da ação | Responder pode coexistir com verbo validar de B; continua exigindo B do cenário A/B. (`app/chamados/tests/test_e3_portas_tarefa.py:75–82`) |
| Relógio | Testa silêncio do chamado e propagação de `agora`; não testa vencimento de marco nem lâmpada desconhecida. (`app/chamados/tests/test_e3_portas_tarefa.py:84–111`) |

Para subgrupos com ação humana, o fato decisivo ausente no V3 é a **porta da ação e sua presença na fila global**, além da localização da âncora para distinguir os cenários A/B e própria célula; o V3 trouxe apenas `[acao,de]`. Os testes exigem essas identidades concretas. (`app/chamados/tests/test_e3_portas_tarefa.py:40–47,67–73`)

**Ação humana com FUTURO `nada` não prova contradição interna.** O contrato implementado escreve expressamente que “ausencia de porta nao diz ausencia de tarefa”; o fio pode ser humano e a rota ausente. Só portas inteiras presentes na fila global produzem rota; `porta_da_tarefa` retorna `None` sem rota, e o diff projeta `None` como `nada`. (`app/chamados/services/tarefa_do_dia.py:540–565`; `app/chamados/management/commands/diff_e3_sombra.py:46–59`)

Há inclusive caso escrito E2 em que a ação é `responder/colaborador`, mas `porta=None`, pois a âncora está silenciada. Portanto essa combinação é admitida pela tarefa; não há esse caso específico no arquivo E3 solicitado. (`app/chamados/tests/test_e2_tarefa_do_dia.py:396–409`; `app/chamados/tests/test_e3_portas_tarefa.py:21–127`)

O anexo permite listar os humanos com FUTURO `nada`, mas não transformar essa lista em “ATUAL obrigatório” nem em bloqueantes ESTRUTURA sem demonstrar a porta pertinente. (`app/chamados/services/tarefa_do_dia.py:128–133,547–565`; `app/chamados/tests/test_e3_portas_tarefa.py:40–47,67–73`)

## 13/11 16:50 · sessao b2ed85ab4ddc · LEI-DESLIGADO-NAO-TRAVA · sonda · origem=Code

| Empresa | Competência | entra / None | fora / furo_espelho | fora / rescisao_modulo_proprio | fora / ferias_com_batida | fora / sem_codigo_dominio | Contado / CLS_TOTAL | Bate? | Referência |
|---|---|---:|---:|---:|---:|---:|---:|---|---|
| 2 | 2026-10 | 239 | 178 | 59 | 0 | 1 | 477 / 477 | Sim | [nao verificado] |
| 2 | 2026-11 | 194 | 235 | 22 | 1 | 1 | 453 / 453 | Sim | [nao verificado] |
| 3 | 2026-10 | 80 | 26 | 11 | 1 | 1 | 119 / 119 | Sim | [nao verificado] |
| 3 | 2026-11 | 52 | 50 | 12 | 1 | 0 | 115 / 115 | Sim | [nao verificado] |
| 4 | 2026-10 | 14 | 8 | 1 | 0 | 1 | 24 / 24 | Sim | [nao verificado] |
| 4 | 2026-11 | 14 | 7 | 2 | 0 | 1 | 24 / 24 | Sim | [nao verificado] |

| Empresa | Linhas DESL | Referência |
|---|---:|---|
| 2 | 309 | [nao verificado] |
| 3 | 60 | [nao verificado] |
| 4 | 13 | [nao verificado] |

`EXP_TOTAL 26 0` [nao verificado]

## 13/11 16:50 · sessao b1896194f747 · - · - · origem=Ronald (tmux)

Conferência independente confirmou: DESL empresa 2 = **309**, empresa 3 = **60**, empresa 4 = **13**; total = **382**. Transcrevi os IDs por empresa em memória e contei; quantidade de IDs únicos também foi 309/60/13. Fonte: bloco fornecido, sem arquivo:linha [nao verificado].

## 13/11 16:50 · sessao 7cbf4a9c7c5a · - · - · origem=Ronald (tmux)

Empresa 2, 2026-11 [nao verificado]:

| Status | Motivo | Linhas |
|---|---|---:|
| entra | None | 194 |
| fora | furo_espelho | 235 |
| fora | rescisao_modulo_proprio | 22 |
| fora | ferias_com_batida | 1 |
| fora | sem_codigo_dominio | 1 |

Total contado: **453**, bate com `CLS_TOTAL 2 2026-11 453` [nao verificado].

Checagem em memória: transcrição de cada ID com sua categoria; **453 registros e 453 IDs distintos** [nao verificado].

## 13/11 16:50 · sessao d52a31c2a0bf · - · - · origem=Ronald (tmux)

Empresa 2 × 2026-10: entra/None **239**; fora/furo_espelho **178**; fora/rescisao_modulo_proprio **59**; fora/sem_codigo_dominio **1**; outros **0**. Soma **477**, bate com `CLS_TOTAL 2 2026-10 477`. [nao verificado]

Método: contagem em memória dos pares ID+categoria das linhas fornecidas; **477 IDs únicos**. [nao verificado]

## 13/11 16:49 · sessao b1896194f747 · - · - · origem=Ronald (tmux)

| Empresa | Competência | entra / None | fora / furo_espelho | fora / rescisao_modulo_proprio | fora / ferias_com_batida | fora / sem_codigo_dominio | Total contado | CLS_TOTAL bate? | Evidência |
|---|---|---:|---:|---:|---:|---:|---:|---|---|
| 3 | 2026-10 | 80 | 26 | 11 | 1 | 1 | 119 | Sim, 119 | [nao verificado] |
| 3 | 2026-11 | 52 | 50 | 12 | 1 | 0 | 115 | Sim, 115 | [nao verificado] |
| 4 | 2026-10 | 14 | 8 | 1 | 0 | 1 | 24 | Sim, 24 | [nao verificado] |
| 4 | 2026-11 | 14 | 7 | 2 | 0 | 1 | 24 | Sim, 24 | [nao verificado] |

Checagem: transcrição em memória de cada ID e categoria, contagem por categoria e conferência de IDs únicos: 119, 115, 24 e 24 respectivamente; nenhum ID duplicado em cada grupo. Fonte: bloco fornecido, sem arquivo:linha [nao verificado].

## 13/11 16:48 · sessao b424e8236fa7 · LEI-DESLIGADO-NAO-TRAVA · sonda · origem=Code

| Empresa | Competência | Linhas CLS | CLS_TOTAL | Batem? |
|---|---|---:|---:|---|
| 2 | 2026-10 | 477 | 477 | Sim [nao verificado] |
| 2 | 2026-11 | 453 | 453 | Sim [nao verificado] |
| 3 | 2026-10 | 119 | 119 | Sim [nao verificado] |
| 3 | 2026-11 | 115 | 115 | Sim [nao verificado] |
| 4 | 2026-10 | 24 | 24 | Sim [nao verificado] |
| 4 | 2026-11 | 24 | 24 | Sim [nao verificado] |

_4697 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
