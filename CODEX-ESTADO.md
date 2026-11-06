# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 08:57** · publicado em **06/11/2026 09:00** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 08:57 · sessao 70a8568b4804 · CXR-NA-FILA · leitura · origem=Code

**CXR-TELA-LE-O-MOTOR está no bloco OBRAS/fila 1**, cuja abertura declara “A FILA 1 VIVA”; sua linha é a 775. Estado: **“registrada — depois da JUIZ-DO-PAR-DUAS-ENTRADAS”**. Dono da obra: **[nao verificado]** — a linha não o declara, e o cabeçalho tem `estado`, `aval` e `nota`, sem coluna `dono`. A dependência já consta como **NO AR em 29/10**. ([BACKLOG.md:714](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:714), [BACKLOG.md:726](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:726), [BACKLOG.md:753](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:753), [BACKLOG.md:775](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:775))

Para fechar, o texto exige levar à tela os dias de `dias_cadastro_x_realidade`, inclusive `l084_pausa`/L-084, mediante **assinatura com contador diário lavrado + cron + serviço**, evitando varrer a frota no request. RELATO, PROMPTS e CORTES confirmam seu registro como obra separada; não registram ali seu fechamento. ([BACKLOG.md:775](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:775), [RELATO.md:1385](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:1385), [PROMPTS.md:1558](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/PROMPTS.md:1558), [CORTES.md:318](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/CORTES.md:318))

**Existe código da tela e da lista**, mas a integração pedida não aparece implementada no corpo de `lista()`: ele monta assinaturas de cadastro e lê a lavra do esmeril. Existe leitura de `dias_cadastro_x_realidade` em `InsumosDoDono.cxr()`, usada para decidir **dono**, o que não equivale a expor todos esses dias na tela. ([views.py:484](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/escala/views.py:484), [cadastro_realidade.py:710](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/escala/services/cadastro_realidade.py:710), [cadastro_realidade.py:723](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/escala/services/cadastro_realidade.py:723), [cadastro_realidade.py:802](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/escala/services/cadastro_realidade.py:802), [e6_oraculo.py:160](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:160))

Os movimentos **185→200** e **18→39** são tratados como dado raspado fornecido; sua conferência em PROD e o vínculo individual dos casos com uma cura permanecem **[nao verificado]**.

Nas definições abaixo, ambas as classes já passaram pelos filtros: dia retido na folha, passado, sem porta alcançável e sem enquadramento nos baldes de lei. ([vigia_fio.py:585](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:585), [vigia_fio.py:642](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:642), [vigia_fio.py:648](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:648), [vigia_fio.py:654](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:654))

| classe | definicao (arquivo:linha) | obra que cobre | mecanismo | estado da obra | o que falta |
|---|---|---|---|---|---|
| **d** | Com célula, sem chamado vivo nem pergunta viva; **colaborador presente na lista CxR, mas dia explicitamente ausente**: `not ps and no_dia == (False, None) and lista.colab_na_lista(cid)`. A classe não determina dono. ([vigia_fio.py:523](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:523), [vigia_fio.py:667](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:667), [vigia_fio.py:677](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:677)) | **CXR-TELA-LE-O-MOTOR cobre a omissão de dias do motor na tela; não demonstra cura dos d/ESTRUTURA.** ([BACKLOG.md:775](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:775), [RELATO.md:41](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:41)) | Se a cura acrescentar o **dia**, com dono CADASTRO/BATIDA, ele poderá sair para `fio_do_admin`; acrescentar apenas a pessoa não basta. **ESTRUTURA não sai por essa regra.** Mantidos os insumos, o juiz já consulta a CxR do motor antes de devolver ESTRUTURA; mudar a leitura da tela não muda esse dono. Se um dia ESTRUTURA passar a constar na lista, pode apenas mudar de `d` para `e/dia_na_lista_dono_estrutura`, ainda sem porta. ([cadastro_realidade.py:955](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/escala/services/cadastro_realidade.py:955), [vigia_fio.py:228](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:228), [e6_oraculo.py:183](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:183), [vigia_fio.py:681](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:681)) | Registrada, depois da JUIZ-DO-PAR. ([BACKLOG.md:775](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:775)) | Implementar a leitura pela assinatura/lavra/cron/serviço descritos. Quantos dos casos raspados pertencem aos dias omitidos do motor: **[nao verificado]**. ([BACKLOG.md:775](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:775)) |
| **e-sem-viva** | `causa='e'`, `detalhe='sem_pergunta_viva'`: com célula, sem chamado vivo e sem pergunta viva, **depois de excluir `d` e o detalhe `dia_na_lista_dono_*`**. Pode haver perguntas antigas já mortas/validadas; não significa “nunca houve pergunta”. ([vigia_fio.py:667](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:667), [vigia_fio.py:677](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:677), [vigia_fio.py:681](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:681), [test_o261e_f5_alcance.py:427](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/tests/test_o261e_f5_alcance.py:427)) | **C20-FIO-MUDO não cobre diretamente esse estado pelo universo declarado**: trata `batida_ausente` **vivo** sem pergunta viva. ([RELATO.md:48](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:48)) | Seu evento atua na abertura do chamado e na recuperação de perdas de pergunta. Já `e-sem-viva` não tem chamado vivo no dia — caso contrário cairia antes em `a`, ou teria porta. O RELATO declara **zero do estoque 494/495 alcançável pelo evento**, mantendo o cron para esse estoque. ([BACKLOG.md:1769](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1769), [vigia_fio.py:664](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:664), [RELATO.md:6670](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:6670)) | **ABERTA, em raia**; código aprovado pelo opus, suíte registrada como verde, pouso esperando lei. ([BACKLOG.md:1769](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1769), [RELATO.md:6674](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:6674)) | Decidir reabrir disputa fechada ao reatar versus apenas oferecer Validar; quatro dos cinco atos continuam sem evento, além das seis vivas sob outro chamado vivo. ([RELATO.md:13](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:13), [RELATO.md:6670](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:6670), [RELATO.md:6671](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:6671)) |
| **e-sem-viva** | Mesmo ramo `e/sem_pergunta_viva`. ([vigia_fio.py:685](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:685)) | **LEI-FIO-MUDO-B: [nao verificado]**; não localizei obra com esse identificador. | Existe uma **alternativa B** na pendência da C20: manter disputa fechada e apresentar pergunta ao admin com Validar. Isso trata pergunta reatada; não demonstra emissão para os casos sem fio nem comprova uma obra chamada LEI-FIO-MUDO-B. ([RELATO.md:13](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:13)) | **[nao verificado]** para esse identificador. | Texto e registro dessa obra: **[nao verificado]**. |
| **e-sem-viva** | Mesmo ramo `e/sem_pergunta_viva`. ([vigia_fio.py:685](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:685)) | **O261 / GRUPO-D-DIA-ACUSA-SEM-FIO** é a cobertura documental para dia acusando sem pergunta e sem chamado; alcance aos 39: **[nao verificado]**. ([BACKLOG.md:1450](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1450), [BACKLOG.md:1105](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1105)) | O261 manda identificar quem acendeu a lâmpada sem emitir e curar o emissor que a lê. GRUPO-D registra encaminhamento de `nunca_bateu` à lista DP e cura separada de `sem_celula`; esta última é **outro detalhe de e**, não `e-sem-viva`. ([BACKLOG.md:1450](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1450), [BACKLOG.md:1105](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1105), [vigia_fio.py:667](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:667)) | O261 registrada como FECHADA em 24/10; GRUPO-D como NO AR em 25/10, com essas curas nomeadas. ([BACKLOG.md:1450](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1450), [BACKLOG.md:1105](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1105)) | Identificar a origem dos casos atuais e sua correspondência com os recortes curados: **[nao verificado]**. O complemento exige **cura por classe depois da medida**, posteriormente absorvida pela O261-E. ([BACKLOG.md:963](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:963)) |

**Quem escreve e quem lê a classificação:**

| papel | funções e evidência |
|---|---|
| **Deriva/escreve em memória** | `censo_alcance()` atribui `d` e `e/sem_pergunta_viva`, incrementa contadores; sua função interna `_listar()` guarda causa e detalhe por dia. O serviço declara nenhuma escrita no banco. ([vigia_fio.py:545](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:545), [vigia_fio.py:625](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:625), [vigia_fio.py:677](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:677), [vigia_fio.py:685](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:685), [vigia_fio.py:695](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:695)) |
| **Lê/formata/imprime** | `linhas_alcance()` lê contadores de causa e detalhe; `vigiar_fio_morto.Command.handle()` chama o censo e imprime essas linhas. ([vigia_fio.py:716](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:716), [vigiar_fio_morto.py:173](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/management/commands/vigiar_fio_morto.py:173), [vigiar_fio_morto.py:183](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/management/commands/vigiar_fio_morto.py:183)) |
| **Persiste a fotografia** | `Command.handle()` entrega o bloco `alcance` inteiro a `_carimbar()`, que grava JSON e substitui o arquivo. ([vigiar_fio_morto.py:211](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/management/commands/vigiar_fio_morto.py:211), [vigiar_fio_morto.py:223](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/management/commands/vigiar_fio_morto.py:223), [vigiar_fio_morto.py:132](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/management/commands/vigiar_fio_morto.py:132)) |
| **Lê o carimbo, sem expor d/e individualmente** | `bin/fio_estado.py::ler()` carrega o JSON; `_ler_carimbo()` lê `alcance`, mas expõe universo, cegueira e hoje/futuro. ([fio_estado.py:98](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/bin/fio_estado.py:98), [fio_estado.py:142](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/bin/fio_estado.py:142), [fio_estado.py:151](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/bin/fio_estado.py:151)) |
| **Decide dono, separadamente da classe** | `InsumosDoDono.cxr()` lê os dias do motor; `.dono()` chama `dono_da_divergencia()`, que decide CADASTRO/BATIDA/ESTRUTURA. A própria QUEIMA-A distingue “CxR do MOTOR, não a da tela”. ([e6_oraculo.py:154](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:154), [e6_oraculo.py:172](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:172), [e6_oraculo.py:98](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:98), [RELATO.md:41](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:41)) |
| **Produz o cruzamento classe × dono do anexo** | Implementação e equivalência das sondas raspadas: **[nao verificado]**. |

## 06/11 08:54 · sessao 2a5c63b89837 · - · - · origem=Ronald (tmux)

- **CXR está no bloco OBRAS/fila 1**, entre `OBRAS:INICIO` e `OBRAS:FIM`; o comentário declara que o hook lê esse bloco e cobra o primeiro aberto. `app/docs/BACKLOG.md:712`, `app/docs/BACKLOG.md:714`, `app/docs/BACKLOG.md:723`, `app/docs/BACKLOG.md:1800`.
- **Estado:** “registrada — depois da JUIZ-DO-PAR-DUAS-ENTRADAS”. A linha não declara dono; a tabela tem colunas `id/o que/estado/aval/nota/quando`, sem coluna dono. **Dono nominal [nao verificado]**. A dependência consta como NO AR em 29/10. `app/docs/BACKLOG.md:726`, `app/docs/BACKLOG.md:753`, `app/docs/BACKLOG.md:775`.
- **Entrega exigida:** a tela deve ler `dias_cadastro_x_realidade` do motor e mostrar dias `l084_pausa`/L-084, usando assinatura com contador diário lavrado + cron + serviço para evitar frota no request. O corte separou essa tela da cura do juiz do par. `app/docs/BACKLOG.md:775`, `app/docs/CORTES.md:318`, `app/docs/RELATO.md:1385`, `app/docs/PROMPTS.md:1558`.

- **C20-FIO-MUDO:** obra aberta na fila 1, evento na abertura de `batida_ausente` + cinco atos de perda. Seu universo é chamado `batida_ausente` **vivo**, sem pergunta viva; não equivale à classe `e-sem-viva`, que só ocorre após o ramo que classificaria chamado vivo como `a`. `app/docs/BACKLOG.md:1769`, `app/docs/RELATO.md:48`, `app/chamados/services/vigia_fio.py:664`, `app/chamados/services/vigia_fio.py:669`, `app/chamados/services/vigia_fio.py:685`.
- **C20 continua na raia:** registro mais recente informa suite verde, opus aprovou, pouso espera decisão de lei. O evento alcança zero dos 494/495 do estoque; estoque continua com cron, quatro dos cinco atos seguem sem evento e seis perguntas vivas sob outro chamado vivo permanecem incompletas. `app/docs/RELATO.md:6670`, `app/docs/RELATO.md:6671`, `app/docs/RELATO.md:6674`.
- **Decisão pendente C20:** reatar pergunta deve reabrir disputa fechada e produzir seus efeitos ou apenas apresentá-la ao admin com Validar, sem reabertura/push. `app/docs/RELATO.md:13`.
- **LEI-FIO-MUDO-B:** não encontrada no repositório nem nos quatro documentos solicitados; existência/estado/mecanismo **[nao verificado]**.

**Outras candidatas para e-sem-viva:**

| Candidata | Cobertura demonstrável | Limite |
|---|---|---|
| O261-FIO-MORTO-PELO-RETRATO | Mede classes a–e e exige cura por classe depois da medida; portanto é guarda-chuva documental da investigação. `app/docs/BACKLOG.md:963` | Estado descreve curas a/b/c e absorção pela O261-E; não documenta cura específica integral de `e-sem-viva`. `app/docs/BACKLOG.md:963`, `app/docs/BACKLOG.md:859` |
| O261 / GRUPO-D | Corte apensado trata lâmpada acesa sem pergunta/chamado: identificar emissor faltante e fazê-lo ler a lâmpada; núcleo inicial é `furo/FURO_SEM_COBRANCA`, no horizonte e não desligado. `app/docs/BACKLOG.md:1450` | A própria linha impede alargamento automático a `nunca_bateu`, `indefinida` e `fato_em_ausencia`; o fechamento em 24/10 refere-se ao contador então utilizado. `app/docs/BACKLOG.md:1450` |
| GRUPO-D-DIA-ACUSA-SEM-FIO | NO AR 25/10: `nunca_bateu` encaminhado ao DP; `sem_celula` remetido à SEM-CELULA-11. `app/docs/BACKLOG.md:1105` | Não demonstra que os 39 atuais pertençam às origens corrigidas. Alcance aos 39 **[nao verificado]** |
| SEM-CELULA-11-DEVE-TER-CELULA | Corrige gerador que excluía afastado INSS com vínculo ativo. `app/docs/BACKLOG.md:1620` | Ausência de célula é outro detalhe de `e`, `sem_celula`; não é `e-sem-viva`. `app/chamados/services/vigia_fio.py:667`, `app/chamados/services/vigia_fio.py:685` |

**Conclusão para consolidação:** há obras gerais e curas históricas por origem, mas **nenhuma cura específica registrada foi demonstrada cobrir integralmente os 39 `e-sem-viva`**; atribuição nominal desses casos a qualquer uma delas **[nao verificado]**. Os documentos mandam medir a origem antes da cura. `app/docs/BACKLOG.md:963`, `app/docs/BACKLOG.md:1450`, `app/docs/RELATO.md:2457`, `app/docs/RELATO.md:2458`.

## 06/11 08:54 · sessao f410b0a503a7 · - · - · origem=Ronald (tmux)

Definições e precedência:

- `d` é `colab_na_lista_cxr_dia_fora`: dia retido na folha, passado, sem porta, sem balde de lei, com célula, sem chamado vivo (senão cai em `a`), sem pergunta **viva**, com `dia_na_lista == (False, None)` e colaborador presente na lista da tela. O teste que atribui `d` está em `app/chamados/services/vigia_fio.py:677`; o comentário explicita que nenhum dono L-099 é lido em `app/chamados/services/vigia_fio.py:523`. As exclusões anteriores estão em `app/chamados/services/vigia_fio.py:642`, `app/chamados/services/vigia_fio.py:648`, `app/chamados/services/vigia_fio.py:654` e `app/chamados/services/vigia_fio.py:667`.
- `e-sem-viva` é o par `causa='e'`, `detalhe='sem_pergunta_viva'`: depois das exclusões anteriores e do teste de `d`, não há pergunta viva; também não caiu no caso “dia na lista com dono ESTRUTURA/nenhum”, que recebe detalhe próprio. A atribuição está em `app/chamados/services/vigia_fio.py:680`, `app/chamados/services/vigia_fio.py:681` e `app/chamados/services/vigia_fio.py:685`. Portanto, “sem pergunta viva” não significa necessariamente nunca ter tido pergunta: o teste com pergunta já validada também resulta nessa classe em `app/chamados/tests/test_o261e_f5_alcance.py:427`.
- As perguntas `ps` são exclusivamente as vivas fornecidas pelo juiz: `app/chamados/services/vigia_fio.py:610`; `alimentar_portas` usa `pergunta_viva` para montá-las em `app/chamados/juizes.py:1300`.
- A lista lida para distinguir `d` é a tela: `ListaDoAdmin._carregar` chama `escala.services.cadastro_realidade.lista`, deriva `dias_na_lista` e o conjunto de colaboradores em `app/chamados/services/vigia_fio.py:212`. `dia_na_lista` retorna `(True,dono)`, `(False,None)` ou `None` em falha, conforme `app/chamados/services/vigia_fio.py:230`; `colab_na_lista` lê presença em `app/chamados/services/vigia_fio.py:248`.
- Estar na lista com dono CADASTRO/BATIDA faz o dia sair antes das classes, para `fio_do_admin`: `app/chamados/services/vigia_fio.py:222`, `app/chamados/services/vigia_fio.py:313`. ESTRUTURA não satisfaz esse predicado: `app/chamados/services/vigia_fio.py:169`, `app/chamados/services/vigia_fio.py:228`.

Quem escreve e lê:

| Papel | Função e evidência |
|---|---|
| Escreve a classificação em memória | `censo_alcance`: cria contadores em `app/chamados/services/vigia_fio.py:571`; atribui `d` em `app/chamados/services/vigia_fio.py:677`; atribui `e-sem-viva` em `app/chamados/services/vigia_fio.py:685`; soma classe/detalhe em `app/chamados/services/vigia_fio.py:695`. |
| Escreve o detalhe por dia em memória | Função interna `_listar`, que acrescenta `[célula,colaborador,data,competência,rota,causa,detalhe,papel]`: `app/chamados/services/vigia_fio.py:625`; recebe classe/detalhe em `app/chamados/services/vigia_fio.py:699`. |
| Lê e formata classe/detalhe | `linhas_alcance`: `app/chamados/services/vigia_fio.py:703`, particularmente `app/chamados/services/vigia_fio.py:716`. |
| Lê o resultado e imprime | `Command.handle`: mede em `app/chamados/management/commands/vigiar_fio_morto.py:173`; imprime as linhas em `app/chamados/management/commands/vigiar_fio_morto.py:183`. |
| Persiste a fotografia contendo as classes | `Command.handle` entrega o bloco `alcance` integral ao carimbo em `app/chamados/management/commands/vigiar_fio_morto.py:211` e `app/chamados/management/commands/vigiar_fio_morto.py:223`; `_carimbar` grava JSON e substitui arquivo em `app/chamados/management/commands/vigiar_fio_morto.py:115`, `app/chamados/management/commands/vigiar_fio_morto.py:132`. |
| Lê o carimbo, sem consumir d/e especificamente | `bin/fio_estado.py::ler`: `bin/fio_estado.py:98`, `bin/fio_estado.py:104`; `_ler_carimbo` usa `alcance` em `bin/fio_estado.py:142`, mas expõe universo, cegueira e hoje/futuro em `bin/fio_estado.py:151`. |
| Lê para produzir tabela classe×dono do anexo | Implementação das sondas ausentes: **[nao verificado]**. |

Dono L-099 é outra pergunta: `InsumosDoDono.cxr` lê `dias_cadastro_x_realidade` do **motor** em `app/ponto/management/commands/e6_oraculo.py:154` e `app/ponto/management/commands/e6_oraculo.py:160`; `InsumosDoDono.dono` chama o juiz em `app/ponto/management/commands/e6_oraculo.py:172` e `app/ponto/management/commands/e6_oraculo.py:183`. A decisão CADASTRO/BATIDA/ESTRUTURA está em `app/ponto/management/commands/e6_oraculo.py:98`, com retorno final condicionado à presença no motor em `app/ponto/management/commands/e6_oraculo.py:110`.

O registro QUEIMA-A confirma a separação “CxR do MOTOR, não a da tela” para `d+ESTRUTURA` em `app/docs/RELATO.md:41`.

## 06/11 08:50 · sessao c44cc1260d0d · PEP-REENTRADA · verificador · origem=Code

**Não daria aceite à garantia de escritor único ainda:** o diff bloqueia o S143, mas existe outra cadeia síncrona que pode fechar a mesma disputa e acionar o S144, cujo carimbo não consulta `_EM_VOO`. (`app/chamados/reconciliador.py:602`; `app/chamados/signals.py:140`; `app/chamados/services/materializacao.py:1395`)

A análise abaixo é estática. Execução dos testes, mutantes, reprodução em produção e confirmação independente do hash contra o pai: **[não verificado]**. [nao verificado]

1) **Quem escreve e quem lê os estados.**

| Estado | Escritores | Leitores e finalidade |
|---|---|---|
| `PerguntaDisputa.materializada_em` | A atribuição individual central é `_escrever_carimbo`; `carimbar`, `carimbar_identidade` e `descarimbar` passam por ela. O descarimbo em lote escreve por `update`. (`app/chamados/models.py:2556`; `app/chamados/models.py:2586`; `app/chamados/models.py:2636`; `app/chamados/models.py:2648`; `app/chamados/models.py:2663`) | C6, juiz do lastro, estado da pergunta, seleção do materializador/S144 e seleção do S143. (`app/chamados/juizes.py:647`; `app/chamados/juizes.py:1177`; `app/chamados/juizes.py:1207`; `app/chamados/models.py:2499`; `app/chamados/services/materializacao.py:1343`; `app/chamados/services/materializacao.py:1390`; `app/chamados/services/disputa_emissao.py:1574`) |
| `materializada_por` | As mesmas portas, junto do instante e da via; autor sem `pk` vira autor nulo. (`app/chamados/models.py:2559`; `app/chamados/models.py:2582`; `app/chamados/models.py:2639`; `app/chamados/models.py:2664`) | Snapshot/restauração da porta, procedência da batida, agrupamento/foto da absorção por ausência e auditoria. Não integra os predicados C6/lastro mostrados. (`app/chamados/models.py:2583`; `app/ponto/services/procedencia_batida.py:71`; `app/chamados/services/materializacao.py:654`; `app/chamados/reconciliador.py:1134`; `app/chamados/juizes.py:1135`; `app/chamados/juizes.py:632`) |
| `via_resolucao` | As portas do trio; também há escritores preexistentes só da via: `encerrar_por_fato_no_espelho` e `relavra_via.relavrar`. O diff não cria atribuição nova desse campo. (`app/chamados/models.py:2560`; `app/chamados/models.py:2673`; `app/chamados/services/relavra_via.py:123`; `app/chamados/services/materializacao.py:1248`) | C6/lastro, explicação de estado, recusa, relavra e trilha do S143. (`app/chamados/juizes.py:647`; `app/chamados/juizes.py:1145`; `app/chamados/juizes.py:1171`; `app/chamados/services/explicador.py:95`; `app/chamados/services/materializacao.py:1064`; `app/chamados/services/relavra_via.py:116`; `app/chamados/services/disputa_emissao.py:1676`) |
| `DisputaSupervisao.fechada_em` / `resolucao` | O fechamento grava ambos em `fechar`. Reabertura administrativa e sistêmica limpam ambos; `limpar_resolucao_residual` limpa apenas o texto de disputa aberta. (`app/chamados/models.py:1861`; `app/chamados/models.py:2063`; `app/chamados/models.py:2087`; `app/chamados/models.py:2173`) | Estado aberto, idempotência, seleção/cauda do S143, cauda do materializador, receivers e apresentação do fio. A resolução também é fallback do texto do fechamento granular. (`app/chamados/models.py:1765`; `app/chamados/models.py:1820`; `app/chamados/services/disputa_emissao.py:1575`; `app/chamados/services/disputa_emissao.py:1710`; `app/chamados/services/materializacao.py:1287`; `app/chamados/services/materializacao.py:1299`; `app/chamados/signals.py:28`; `app/chamados/signals.py:45`; `app/chamados/services/fio.py:131`) |
| `_EM_VOO` | Criado com conjunto vazio; `materializacao_em_voo` instala a união dos `pk` e restaura o token no `finally`. (`app/chamados/services/materializacao.py:1103`; `app/chamados/services/materializacao.py:1117`; `app/chamados/services/materializacao.py:1121`) | `pergunta_em_voo`; em produção, seus consumidores são as três guardas acrescentadas ao S143. (`app/chamados/services/materializacao.py:1126`; `app/chamados/services/disputa_emissao.py:1586`; `app/chamados/services/disputa_emissao.py:1671`; `app/chamados/services/disputa_emissao.py:1710`) |

Os **oito sítios de chamada de `carimbar` em produção** são estes; as chamadas dos testes não constituem escritores adicionais de produção. (`app/chamados/models.py:2685`; `app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:586`; `app/chamados/services/disputa_emissao.py:1612`; `app/chamados/services/disputa_emissao.py:1686`; `app/chamados/services/materializacao.py:549`; `app/chamados/services/materializacao.py:1248`; `app/chamados/services/materializacao.py:1395`)

| Sítio | Alcançabilidade durante o bloco e efeito sobre a P em voo |
|---|---|
| Materializador, `app/chamados/services/materializacao.py:1248` | **Direta:** é o carimbo final pretendido, com declaração ou via do fato, depois do juiz do lastro. (`app/chamados/services/materializacao.py:1203`; `app/chamados/services/materializacao.py:1232`) |
| S143 principal, `app/chamados/services/disputa_emissao.py:1612` | **Síncrona:** reconciliação do destino → `post_save` → S143. Agora pula a P em voo antes de chegar ao carimbo. (`app/chamados/services/materializacao.py:746`; `app/chamados/services/materializacao.py:949`; `app/chamados/signals.py:75`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1586`) |
| S143 sem credor, `app/chamados/services/disputa_emissao.py:1686` | **Mesmo receiver:** agora também pula a P em voo; normalmente a P validada já estaria excluída de `_abertas`. (`app/chamados/services/disputa_emissao.py:1665`; `app/chamados/services/disputa_emissao.py:1671`) |
| S144, `app/chamados/services/materializacao.py:1395` | **Síncrona se alguma rota fechar a disputa:** não consulta voo; carimba `VIA_JUIZ_APROVOU` quando o lastro aprova. (`app/chamados/signals.py:124`; `app/chamados/signals.py:140`; `app/chamados/services/materializacao.py:1391`) |
| Absorção por ausência, `app/chamados/services/materializacao.py:549` | Não é chamada diretamente por `_materializar_uma`; é chamada pelo escritor da resposta. Se executada reentradamente para o mesmo dia, não tem guarda de voo e tenta carimbar as perguntas-hora selecionadas. Aresta canônica do plantio até esse escritor da resposta: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1330`; `app/chamados/services/materializacao.py:531`) |
| Sem turno aberto, `app/chamados/reconciliador.py:405` | **Alcançável pelo signal da batida**, via reconciliação de órfãs. A P-hora normal com texto é barrada pela guarda de resposta; outras perguntas continuam elegíveis. (`app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1417`; `app/chamados/reconciliador.py:1603`; `app/chamados/reconciliador.py:364`) |
| Célula absolveu, `app/chamados/reconciliador.py:586` | **Alcançável pelo signal da batida.** Exclui diretamente a P validada, mas pode carimbar uma irmã e fechar a mesma disputa, chegando indiretamente ao S144. (`app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1416`; `app/chamados/reconciliador.py:620`; `app/chamados/reconciliador.py:602`) |
| `encerrar_fantasma`, `app/chamados/models.py:2685` | Chamado pelo consumidor de eventos da célula; não encontrei ligação síncrona desse consumidor ao bloco revisado. Não tem guarda de voo própria. (`app/chamados/reconciliador.py:1105`; `app/chamados/reconciliador.py:1136`; `app/chamados/reconciliador.py:1363`) |

Há ainda os carimbos **de identidade**, fora desse inventário de chamadas a `carimbar`: paridade e colisão pertencem ao materializador e usam sua porta própria; não representam proteção contra outro escritor reentrante. (`app/chamados/services/materializacao.py:899`; `app/chamados/services/materializacao.py:1074`; `app/chamados/models.py:2625`)

Os **quinze sítios de chamada de fechamento de `DisputaSupervisao` em produção** encontrados são:

| Sítio | Dentro do bloco revisado? Consequência para a P em voo |
|---|---|
| `app/chamados/services/disputa_emissao.py:1713` | **Sim**, S143 síncrono; a guarda nova impede fechar disputa com P em voo. (`app/chamados/services/disputa_emissao.py:1710`) |
| `app/chamados/reconciliador.py:602` | **Sim**, signal da batida; sem guarda de voo, pode disparar S144 sobre P. (`app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1416`; `app/chamados/signals.py:140`) |
| `app/chamados/reconciliador.py:412` | **Sim**, reconciliação de órfãs; a P normal com texto não é lavrada ali, mas o fecho ocasionado por outra pergunta não tem guarda de voo. (`app/chamados/reconciliador.py:364`; `app/chamados/reconciliador.py:1603`) |
| `app/chamados/services/disputa_emissao.py:1118` | **Condicionalmente**, pelo emissor acionado pelo cartório durante o signal da batida; esse fecho usa C7 e não consulta voo. (`app/ponto/signals.py:135`; `app/ponto/services/cartorio.py:1989`; `app/ponto/services/cartorio.py:1740`; `app/ponto/management/commands/detectar_ausencias.py:801`; `app/chamados/services/disputa_emissao.py:1112`) |
| `app/chamados/services/materializacao.py:1304` | No fluxo normal, **depois** das unidades e respectivos marcadores; numa chamada pública aninhada, pode executar enquanto uma unidade externa ainda está em voo. Não consulta voo. (`app/chamados/services/materializacao.py:1346`; `app/chamados/services/materializacao.py:1349`; `app/chamados/services/materializacao.py:1287`) |
| `app/chamados/services/materializacao.py:568` | Rota da resposta/ausência, sem guarda de voo; ligação síncrona canônica ao plantio: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1330`) |
| `app/chamados/services/becos.py:52` | O receiver agenda sua entrada por `on_commit`: normalmente roda depois do bloco, sem a marca. (`app/chamados/signals.py:197`; `app/chamados/signals.py:202`) |
| `app/chamados/reconciliador.py:1160` | Consumidor do evento da célula; ligação síncrona ao bloco: **[não verificado]**. (`app/chamados/reconciliador.py:1105`; `app/chamados/reconciliador.py:1363`) |
| `app/chamados/models.py:1879` | Porta `admin_encerra`; ligação a partir do plantio: **[não verificado]**. Se chamada, pode carimbar perguntas vivas por decisão administrativa antes do fechamento. (`app/chamados/models.py:1852`; `app/chamados/models.py:1855`) |
| `app/chamados/models.py:1986` | Remoção do último motivo; os chamadores incluem signals de colaborador/escala. Ligação desses eventos ao plantio revisado: **[não verificado]**. Essa porta apaga as perguntas do motivo antes de fechar. (`app/colaboradores/signals.py:77`; `app/escala/signals.py:161`; `app/chamados/models.py:1976`) |
| `app/api/foto_recebida.py:80` | Rota de upload; ligação síncrona ao plantio: **[não verificado]**. |
| `app/ponto/services/entrada_sincronizada.py:59` | Rota chamada pelas APIs de sync; ligação a partir do plantio: **[não verificado]**. (`app/api/views.py:805`; `app/api/views_core.py:805`) |
| `app/chamados/services/competencia_trancada.py:204` | Porta da tranca; ligação síncrona ao plantio: **[não verificado]**. |
| `app/colaboradores/services/desligamento.py:142` | Porta de desligamento; ligação síncrona ao plantio: **[não verificado]**. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | Comando; ligação síncrona ao plantio: **[não verificado]**. |

O contraexemplo principal não depende das rotas marcadas como não verificadas:

- O plantio grava a batida e aciona o reconciliador sincronamente. (`app/chamados/services/materializacao.py:902`; `app/ponto/signals.py:206`; `app/ponto/signals.py:215`)
- Uma irmã Q, não validada, sem carimbo e com célula julgada que não acusa, pode entrar na absolvição e ser carimbada. (`app/chamados/reconciliador.py:617`; `app/chamados/reconciliador.py:627`; `app/chamados/reconciliador.py:586`)
- A P em voo, por estar validada, já é morta pelo C6; portanto não impede C7 de liberar o fechamento da mesma disputa. (`app/chamados/juizes.py:1136`; `app/chamados/models.py:1794`; `app/chamados/reconciliador.py:602`)
- O fechamento aciona S144; o lastro pode encontrar a batida recém-gravada mesmo antes de ela receber `pergunta_origem`. S144 carimba P com `VIA_JUIZ_APROVOU`; depois o materializador pode carimbá-la novamente com declaração. (`app/chamados/signals.py:140`; `app/chamados/juizes.py:774`; `app/chamados/services/materializacao.py:921`; `app/chamados/services/materializacao.py:1395`; `app/chamados/services/materializacao.py:1248`)

É uma possibilidade demonstrada pela composição do código; fixture executada e ocorrência produtiva: **[não verificado]**.

2) **Escopo do sinal.**

| Situação | Resultado |
|---|---|
| Saída normal/commit | O `reset` executa **antes** da saída do `atomic`, pela ordem inversa dos context managers. Se houver transação externa, o sinal termina mesmo sem commit físico naquele ponto. (`app/chamados/services/materializacao.py:1191`; `app/chamados/services/materializacao.py:1121`) |
| `_Reprovada` | A exceção atravessa os context managers: restaura o token, desfaz o atomic e só então é tratada. (`app/chamados/services/materializacao.py:1210`; `app/chamados/services/materializacao.py:1120`; `app/chamados/services/materializacao.py:1254`) |
| Outra exceção | Mesmo `finally`; `Exception` é acumulada como erro fora do bloco. Exceções que não forem capturadas também atravessam o `finally`. (`app/chamados/services/materializacao.py:1120`; `app/chamados/services/materializacao.py:1256`) |
| `return` interno | Também passa pelo `finally`. (`app/chamados/services/materializacao.py:1199`; `app/chamados/services/materializacao.py:1120`) |
| Rollback de savepoint interno | Não remove a marca externa: ela pertence ao context manager externo, não ao savepoint. Se houver marcador interno, sua saída restaura o conjunto anterior. (`app/chamados/services/materializacao.py:1117`; `app/chamados/services/materializacao.py:1121`) |
| Outra P aninhada | União dos dois `pk`; ao sair da interna, restaura somente a externa. (`app/chamados/services/materializacao.py:1117`; `app/chamados/services/materializacao.py:1121`) |
| Mesma P aninhada | O conjunto continua contendo o mesmo `pk`; sair da interna preserva a marca externa. **Não impede materializar/carimbar a mesma P novamente.** (`app/chamados/services/materializacao.py:1117`; `app/chamados/services/materializacao.py:1129`; `app/chamados/services/materializacao.py:1248`) |

No fluxo síncrono mostrado, não há saída do bloco que dispense a restauração do token. O objeto é de módulo, mas o valor é de contexto; não é um conjunto global compartilhado entre as quatro threads configuradas do gthread. (`app/chamados/services/materializacao.py:1103`; `app/chamados/services/materializacao.py:1120`; `app/gunicorn.conf.py:19`)

O `on_commit` normal, executado depois desse bloco, não vê a marca criada por ele, porque ela já foi restaurada. Outra request/thread independente ou task/cron separado não recebe automaticamente esse valor. (`app/chamados/services/materializacao.py:1191`; `app/chamados/services/materializacao.py:1121`; `app/chamados/signals.py:121`; `app/chamados/signals.py:202`)

A promessa absoluta “nenhum caminho sobrevive” exige ressalva: um contexto explicitamente copiado ou task que herde o contexto pode conservar um snapshot que o `reset` do contexto original não limpa. O sinal contém apenas `pk`, sem identidade da transação. Caminho concreto de cópia/herança desse contexto na aplicação: **[não verificado]**. (`app/chamados/services/materializacao.py:1103`; `app/chamados/services/materializacao.py:1117`; `app/chamados/services/materializacao.py:1121`)

3) **Aliases e conexões.**

**Não é genericamente correto para múltiplos aliases.** `_em_atomic` consulta somente `django.db.connection`, isto é, a conexão padrão; o materializador e o receiver S143 também abrem `atomic()` sem `using`, e o receiver não encaminha o alias recebido pelo signal. (`app/chamados/services/materializacao.py:1106`; `app/chamados/services/materializacao.py:1191`; `app/chamados/signals.py:76`; `app/chamados/signals.py:91`)

Assim, um atomic só no alias B não satisfaz a guarda; inversamente, estar em atomic no padrão não prova que uma operação feita em B pertença à transação protegida. O conjunto também não distingue dois objetos com mesmo `pk` em bancos diferentes. (`app/chamados/services/materializacao.py:1108`; `app/chamados/services/materializacao.py:1117`; `app/chamados/services/materializacao.py:1126`)

Na configuração SaaS da cópia há apenas `default`, com tenants por schema. Os chamadores encontrados do S143 não selecionam alias na chamada; execução efetiva em conexão diferente ou configuração produtiva divergente: **[não verificado]**. (`app/config/settings/saas.py:83`; `app/config/settings/saas.py:93`; `app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:1631`; `app/chamados/services/becos.py:49`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`)

4) **O `continue` da cauda e o aval.**

Minha leitura é que ele é **compatível como bloqueio de uma rota indireta de carimbo**: impedir só o laço principal deixaria a cauda fechar D e S144 carimbar P. Não cria escritor novo, mas muda observavelmente momento, autor e texto do fechamento; é uma decisão adicional de ordenação, não um resultado de C7. (`app/chamados/services/disputa_emissao.py:1710`; `app/chamados/services/disputa_emissao.py:1713`; `app/chamados/signals.py:140`; `app/chamados/services/materializacao.py:1395`)

O caminho normal posterior é `_fechar_disputa_se_quitada`, depois do laço. O texto será `resolucao_pedida`, senão a resolução já existente na instância, senão `Regularizacao granular: N pergunta(s) materializada(s).` Portanto, “será sempre a resolução granular” seria incorreto. (`app/chamados/services/materializacao.py:1349`; `app/chamados/services/materializacao.py:1299`; `app/chamados/services/materializacao.py:1304`)

O `continue` não deixa veto persistente: a marca desaparece, e a cauda normal ainda consulta C7. Se outra pergunta permanecer viva ou com resposta sem veredito, a disputa permanece aberta por esse juiz; sua resolução futura depende de outro ato/evento, cuja ocorrência é **[não verificado]**. (`app/chamados/services/materializacao.py:1121`; `app/chamados/services/materializacao.py:1287`; `app/chamados/models.py:1794`)

Se o materializador reprovar, o fecho S143 que antes ocorresse **dentro do mesmo atomic** também seria revertido; pulá-lo não elimina um fechamento que sobreviveria legitimamente ao rollback. A porta de validação possui tratamento posterior para devolver validação recusada e reabrir a disputa. (`app/chamados/services/materializacao.py:1191`; `app/chamados/services/materializacao.py:1210`; `app/chamados/services/validacao.py:425`; `app/chamados/services/validacao.py:199`)

Há uma distinção importante: P validada pode ser considerada morta pelo C6 mesmo sem carimbo. Logo a cauda pode fechar após uma tentativa malsucedida; “P sem `materializada_em` implica disputa necessariamente aberta” não é lei desse código. (`app/chamados/juizes.py:1136`; `app/chamados/services/materializacao.py:1287`)

Não identifiquei abertura eterna causada exclusivamente por esse `continue`; tampouco o código garante progresso futuro sem novos atos. A afirmação do comentário de que “quem fecha essa disputa é o materializador” é ampla demais diante do fecho do reconciliador descrito no item 1. (`app/chamados/services/disputa_emissao.py:1708`; `app/chamados/reconciliador.py:602`)

5) **Contornos.**

| Caso | Resultado por leitura |
|---|---|
| `hora` | Recebe o marcador antes do ramo; reconcilia destino/clones antes do carimbo final. (`app/chamados/services/materializacao.py:1191`; `app/chamados/services/materializacao.py:1193`; `app/chamados/services/materializacao.py:949`; `app/chamados/services/materializacao.py:1248`) |
| `data_hora` | Mesma proteção; destino é opcional e a reconciliação ocorre no fim do ramo. (`app/chamados/services/materializacao.py:1195`; `app/chamados/services/materializacao.py:969`; `app/chamados/services/materializacao.py:1034`) |
| `escolha` | Mesma proteção; pode declarar ausência e reconciliar destinos/clones, sem exigir o juiz de batida usado 

[truncado em 20000 de 31138 caracteres]

## 06/11 08:47 · sessao b0d20fd3ef34 · - · - · origem=Ronald (tmux)

Inventário de produção: **8 chamadas de `PerguntaDisputa.carimbar` e 15 chamadas de `DisputaSupervisao.fechar`** encontradas na cópia, discriminadas abaixo; testes e migrações ficam fora deste inventário. Os próprios sítios são as referências de evidência. [nao verificado]

### Todos os `carimbar`

| Sítio | Alcançabilidade no bloco de `_materializar_uma` / efeito sobre P |
|---|---|
| `app/chamados/services/materializacao.py:1248` | **Direta**: escritor pretendido, com `VIA_DECLARACAO` quando `declarou_hora(p)`, conforme `:1232-1249`. |
| `app/chamados/services/disputa_emissao.py:1612` | **Síncrona**, por fecho de destino/clones → `post_save` → S143 (`app/chamados/services/materializacao.py:949`; `app/chamados/signals.py:75-94`). Agora pula P em voo em `disputa_emissao.py:1587`. |
| `app/chamados/services/disputa_emissao.py:1686` | Mesmo S143, ramo `_abertas`; nova guarda em `:1671`. Além disso `_abertas` exclui validada (`:1666`), portanto a P canônica validada já não entrava. |
| `app/chamados/services/materializacao.py:1395` | **Síncrona indireta** se qualquer caminho fechar D durante plantio: `app/chamados/signals.py:124-140`. **Não consulta em voo**; pode carimbar P em `VIA_JUIZ_APROVOU` antes de `:1248`. |
| `app/chamados/reconciliador.py:586` | **Síncrona** por signal da Batida (`app/ponto/signals.py:206-215`, `reconciliador.py:1416`). Não atinge a P canônica validada: filtro `validada_em__isnull=True` em `reconciliador.py:620`. Pode carimbar irmã Q e fechar a mesma D em `:602`, disparando S144 sobre P. |
| `app/chamados/reconciliador.py:405` | **Síncrona** via reconciliação de órfãs (`:1417`, `:1493-1511`, `:1603`); P com resposta textual é excluída por `:364-365`, portanto a P canônica `hora` não recebe esse carimbo. Sem guarda `_EM_VOO`. |
| `app/chamados/models.py:2685` | Via `encerrar_fantasma`, chamada pelo consumidor de EventoCelula (`reconciliador.py:1136`, `:1363`). Não encontrei aresta desse consumidor no fluxo síncrono do plantio; consumidor descrito como cron em `reconciliador.py:1105`. Sem guarda `_EM_VOO`. |
| `app/chamados/services/materializacao.py:549` | Ausência declarada por resposta: chamador `disputa_emissao.py:1327-1330`, **antes e fora do caminho `_materializar_uma`**. `_materializar_resposta_escolha` chama outro helper de ausência (`materializacao.py:1052-1054`). Sem guarda `_EM_VOO`; entrada explícita/aninhada poderia atingir P `hora` ainda nula pelo filtro `:531-532`. |

### Achado principal: bypass concreto do S144

Condições suficientes por leitura:

1. P validada, ainda não materializada; Q irmã, mesma disputa, não validada/não materializada, com célula julgada que não acusa (`app/chamados/reconciliador.py:617-631`).
2. Plantio de P cria Batida (`app/chamados/services/materializacao.py:902`; `app/ponto/registro_batida.py:157-159`). `post_save` julga a célula e depois reconcilia chamados (`app/ponto/signals.py:131-135`, `:206-215`).
3. Q passa no C6 e recebe carimbo `celula_absolveu` (`reconciliador.py:644-662`, `:586`); a disputa pode fechar em `:602`.
4. P **não segura C7** porque `validada_em` mata a vitalidade imediatamente (`app/chamados/juizes.py:1135-1137`); C7 só combina ausência de resposta sem veredito e fio vivo (`app/chamados/models.py:1783-1794`).
5. Fechar D dispara S144 imediatamente (`app/chamados/signals.py:124-140`), que considera P ainda nula e não verifica `_EM_VOO` (`materializacao.py:1389-1395`).
6. Lastro pode aprovar P **antes do vínculo `pergunta_origem`**: o juiz tenta origem, mas aceita a batida viva na janela (`app/chamados/juizes.py:718-728`, `:774-776`). A origem só é anotada depois do primeiro `registrar_batida` (`materializacao.py:921-925`).
7. P recebe `VIA_JUIZ_APROVOU` e depois o materializador torna a chamar `carimbar` em `materializacao.py:1248`; a porta não exige que o carimbo anterior seja nulo (`models.py:2583-2609`).

É caminho estático reproduzível sob as condições acima; ocorrência real e execução desse cenário **[não verificado]**.

### Todos os fechos de disputa

| Sítio | Dentro do bloco? / consequência |
|---|---|
| `app/chamados/services/disputa_emissao.py:1713` | Sim, S143; agora pula D com qualquer P em voo em `:1710-1711`. |
| `app/chamados/reconciliador.py:602` | **Sim**, pela Batida e por eventual E2 dentro do cartório (`app/ponto/services/cartorio.py:958-959`); sem guarda em voo, pode atingir mesma D e acionar S144. |
| `app/chamados/reconciliador.py:412` | **Sim**, pela reconciliação de órfãs (`:1417`, `:1603`); pode fechar D após carimbar outra P, sem guarda em voo. |
| `app/chamados/services/disputa_emissao.py:1118` | **Condicionalmente sim**: cartório chama emissor em `app/ponto/services/cartorio.py:1740`/`:1778`; emissor abre fio por `app/ponto/management/commands/detectar_ausencias.py:787-801` → `criar_disputa_a_partir_de_chamados_orfaos`. Não é a aresta da `fabrica_da_celula`; esta chama outra função em `fabrica_da_celula.py:139`. Sem guarda em voo. |
| `app/chamados/services/materializacao.py:1304` | No fluxo normal **depois** do bloco, ao terminar o laço (`:1346-1350`). Sob reentrada aninhada de outro materializador, poderia ocorrer enquanto a marca externa continua ativa. |
| `app/chamados/services/materializacao.py:568` | Resposta de ausência em `disputa_emissao.py:1330`, não chamada por `_materializar_uma`; não encontrei aresta síncrona a partir do bloco. Sem guarda em voo. |
| `app/chamados/services/becos.py:52` | Callback agendado pelo save de Pergunta (`app/chamados/signals.py:191-202`); execução normal após sair da transação/bloco. Recorte/juiz em `becos.py:25-33`, `:48-52`. |
| `app/chamados/reconciliador.py:1160` | Consumidor de EventoCelula (`:1363`), não encontrei aresta dentro do bloco; sem guarda em voo. |
| `app/chamados/models.py:1879` | Porta humana `admin_encerra`; não encontrei aresta a partir do plantio. Fecha pela exceção e pode `fechar_por_admin` vivas (`:1852-1855`). |
| `app/chamados/models.py:1986` | Remoção do último motivo. Callers nos signals de Posto (`app/colaboradores/signals.py:77`) e da âncora de Escala (`app/escala/signals.py:161`); plantio de `data_hora` explicitamente não grava mais fase (`materializacao.py:1001-1004`). Não encontrei aresta do plantio a esses eventos. |
| `app/api/foto_recebida.py:80` | Upload de foto; não encontrei aresta do plantio. |
| `app/ponto/services/entrada_sincronizada.py:59` | Sync API: `app/api/views.py:805`, `views_core.py:805`; não encontrei aresta do plantio. |
| `app/chamados/services/competencia_trancada.py:204` | Aplicação de tranca; não encontrei aresta do plantio. |
| `app/colaboradores/services/desligamento.py:142` | Desligamento; não encontrei aresta do plantio. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | Comando do acervo; não encontrei aresta do plantio. |

Todos os fechos que chegam a `DisputaSupervisao.fechar` fazem o save de D (`models.py:1861-1865`) e portanto podem acionar o S144 síncrono (`signals.py:124-140`); a proteção atual está exclusivamente no chamador S143 (`disputa_emissao.py:1710`).

### Campos: escritores e leitores

- **Trio de carimbo**: atribuição central em `app/chamados/models.py:2556-2560`; chamado por `carimbar` (`:2583-2609`), identidade (`:2636-2644`), descarimbar (`:2646-2652`). Lote limpa os três juntos (`:2663-2664`). Escritores isolados existentes de via: `encerrar_por_fato_no_espelho` (`:2673-2674`) e relavra (`app/chamados/services/relavra_via.py:116-126`).
- **Fechada_em/resolucao**: fecho escreve juntos em `models.py:1861-1865`; reabrir admin limpa em `:2063-2075`; sistema em `:2087-2091`; `limpar_resolucao_residual` limpa só texto de disputa aberta (`:2170-2174`). Assim “só fechar escreve resolucao” é inexato sem mencionar as portas de reabertura/limpeza.
- **Leitores semânticos de materializada_em/via**: C6 (`app/chamados/juizes.py:1145`, `:1171`, `:1177`, `:1207-1214`), lastro (`:647-673`, `:718-778`), assinatura de memo (`:813-814`), estado de Pergunta (`models.py:2499-2505`), fila do materializador (`materializacao.py:1340-1343`), S144 (`:1389-1395`), agrupamento de ausência (`:607-614`), aplicação UI (`:433-441`), S143 (`disputa_emissao.py:1574`, `:1666`).
- **Materializada_por**: autoria/snapshot/trilha, não condição de C6/lastro: `models.py:2583`, `:2636-2639`; `materializacao.py:654`; `reconciliador.py:1134`, `:1144`; procedência da Batida escolhe `validada_por or materializada_por` (`app/ponto/services/procedencia_batida.py:71-74`).
- **Outros leitores de apresentação/auditoria**: `app/chamados/services/explicador.py:95` via; `historico.py:103` instante; `painel.py:67` instante agregado; `raiox.py:133`, `:173`; `passivo_validado.py:64`, `:140-142`; `app/ponto/services/flip_auto.py:129`.
- **Fechada_em**: propriedade aberta (`models.py:1765`), idempotência de fecho (`:1820-1824`), reabrir (`:2052`, `:2081`), seleção e cauda S143 (`disputa_emissao.py:1630`, `:1654`, `:1656`, `:1710`), cauda materializador (`materializacao.py:1287`), gates/seleção de fio (`fio.py:131`, `:135`, `:184`, `:596`, `:629`), validação (`validacao.py:196-201`), UI (`app/api/views.py:1008`, `:1030`, `:2694`; `app/chamados/views.py:471`, `:495`, `:512`, `:530`, `:911`, `:1329`).
- **Resolucao**: idempotência (`models.py:1821`), fallback materializador (`materializacao.py:1299-1300`), texto do fio após fecho (`app/chamados/signals.py:45`), limpeza residual (`models.py:2170-2173`).
- **`_EM_VOO`**: definição `materializacao.py:1102`; `.set` no contexto `:1117`; `.reset` em `:1121`; leitura no predicado `:1126`; consumidores S143 `disputa_emissao.py:1587`, `:1671`, `:1710`. Não é fonte consultada por C6, lastro, reconciliador ou S144 nesses respectivos sítios.

## 06/11 08:46 · sessao 802b8b8173a0 · - · - · origem=Ronald (tmux)

Auditoria estática; execução de testes e mutantes: [não verificado]. [nao verificado]

**Mordida e mutantes sobreviventes, teste a teste** [nao verificado]

| Teste (`app/chamados/tests/test_pep_reentrada.py`) | O que realmente observa; mutante que pode continuar verde |
|---|---|
| `test_MORDE_a_P_em_voo_tem_UM_escritor_o_materializador` — :83 | Observa destino terminado, existência de Batida e exatamente um `carimbar` **bem-sucedido** com `VIA_DECLARACAO` (:87-92). Morde a remoção da guarda principal e, indiretamente, da cauda: sem a cauda, fechar dispara S144, que carimba `VIA_JUIZ_APROVOU` antes do materializador (`signals.py:139-140`; `materializacao.py:1395`, :1248). **Sobrevive:** desativar o receiver S143 inteiro; destino e materializador ainda podem cumprir as três asserções, e os testes diretos de S143 não verificam a ligação do receiver (:74-76, :169-182; `signals.py:75-92`). O spy ignora tentativas que retornam `False`, `carimbar_identidade` e escrita direta (:69-73). | [nao verificado]
| `test_a_via_gravada_e_a_do_materializador` — :94 | Só vê via/autor finais (:98-99). **Sobrevive:** remover todas as guardas de voo do S143; o carimbo final do materializador regrava declaração/user (`materializacao.py:1248-1249`), pois `carimbar` não protege carimbo já gravado e atribui/grava novamente (`models.py:2583-2586`, :2609). É estruturalmente incapaz de detectar o segundo escritor antigo. |
| `test_o_S143_nao_fecha_a_disputa_em_nome_dele_no_meio_do_plantio` — :101 | Só vê disputa e resolução **finais**, sem observar o meio do plantio (:104-107). **Sobrevive:** remover a guarda da cauda; o S143 fecha uma instância obtida pela consulta/refresh (`disputa_emissao.py:1616`, :1703, :1713), mas o `d` original do materializador segue com `fechada_em` em memória. A cauda do materializador não atualiza esse `d` e pode fechar novamente com texto granular (`materializacao.py:1287`, :1299-1304); `fechar` também verifica o estado da própria instância, não busca estado atual (`models.py:1820-1824`, :1861-1865). Logo a resolução S128 intermediária pode ser sobrescrita, deixando este teste verde também no fluxo antigo. | [nao verificado]
| `test_o_sinal_existe_DURANTE_o_plantio_e_morre_na_saida` — :112 | Observa sinal na entrada de `_materializar_resposta_hora` e após sucesso (:118-124). **Sobrevive:** reset apenas no sucesso, com vazamento em exceção; não há exceção neste cenário. Não prova permanência até o carimbo nem fronteira de commit real (:118-124; :1248). | [nao verificado]
| `test_MORDE_o_sinal_morre_no_ROLLBACK` — :126 | Força juiz de lastro falso, vê P sem carimbo e getter falso (:129-133). **Sobrevive:** o materializador deixar de instalar o sinal completamente; a propriedade “não sobrevive” passa sem jamais tê-lo visto nascer. Não verifica Batida/destino efetivamente revertidos (:131-133). A origem `_Reprovada` está em `materializacao.py:1203-1210`. |
| `test_MORDE_o_sinal_morre_na_EXCECAO_que_atravessa` — :135 | Testa o context manager diretamente e vê getter falso após `_Boom` (:141-144). **Sobrevive:** context manager só fazer `yield`, sem instalar sinal; falta asserção positiva antes de levantar. Não testa exceção real atravessando `_materializar_uma`, que captura `Exception` (`materializacao.py:1256-1257`). |
| `test_aninhado_cada_P_e_a_sua` — :146 | Cobre duas P distintas, duas entradas/saídas normais e restauração do pai (:150-155). **Sobrevive:** implementar conjunto global mutável com add/remove por P; neste cenário síncrono distinto ele produz os mesmos resultados. Não cobre mesma P aninhada, reentrada real de `_materializar_uma`, exceção aninhada, isolamento entre contextos ou rollback de savepoint (:150-155). |
| `test_P_fora_de_voo_o_S143_segue_carimbando` — :167 | Controle positivo deliberado: chamada direta, uma P e batida real (:157-174). Passar no código antigo é esperado; não é RED da cura. **Sobrevive:** retirar `_em_atomic()` do getter; nenhuma P foi instalada neste cenário (:170-174; `materializacao.py:1126`). Não exige via nem autor (:173-174). |
| `test_MORDE_P_em_voo_o_S143_pula` — :176 | Chamada direta com sinal; exige contador zero e P sem carimbo (:181-185). Morde retirada da guarda principal. **Sobrevive:** S143 pular **todas** as perguntas sempre que houver qualquer voo; o cenário tem uma P, sem controle simultâneo de outra P (:157-165, :180-185). |
| `test_MORDE_fora_de_atomic_recusa` — :190 | Exige exceção sem atomic e getter falso (:194-197). **Sobrevive:** o context manager sempre levantar `RuntimeError`, inclusive dentro de atomic; este teste só usa a borda proibida. Também não distingue `_EM_VOO` limpo de valor escondido pelo `and _em_atomic()` (`materializacao.py:1126`). | [nao verificado]

**Vacuidade e alcance**

- Os testes de via final (:94-99) e resolução final (:101-107) não comprovam o defeito intermediário: o próprio fluxo pode sobrescrever a evidência; as respectivas sobrescritas estão em `materializacao.py:1248-1249`, :1299-1304 e `models.py:1861-1865`, :2586-2609. A confirmação de verde no pai: [não verificado]. [nao verificado]
- O RED forte é a lista de carimbos bem-sucedidos (:69-73, :90-92), que preserva evidência do escritor intermediário mesmo após sobrescrita final. Ele não comprova sozinho que S143 foi realmente chamado, pois o receiver pode ser desligado (`signals.py:75-92`).
- A fixture só prepara `hora`, uma P validada, um destino inicialmente aberto (:43-62). Não há casos de `data_hora`, `escolha`, sem destino, destino já fechado, clones com outras P, ausência/S144 invocado por outro caminho, alias diferente, mesma P aninhada, thread/contexto copiado ou savepoint interno revertido (:43-62, :83-197).
- O helper executa `captureOnCommitCallbacks(execute=True)` num `TestCase` (:36, :74-76); não constitui observação de commit real em `TransactionTestCase`. Comportamento de commit real nessa suíte: [não verificado]. [nao verificado]

**Classes de reprovação**

- **Segundo juiz:** C6 e C7 vivos são `pergunta_viva` e `DisputaSupervisao.pode_fechar` (`app/core/juizes.py:266-267`). O novo estado não copia sua regra; identifica propriedade temporal de uma P (`materializacao.py:1115-1126`). Porém a cauda adiciona veto antes de C7 (`disputa_emissao.py:1710-1712`); é decisão de ordem/ownership adicional e deve ser avaliada pelo aval literal, não apresentada como se viesse de C7.
- **JUIZES.md divergente:** diz “disputa fecha e SLA: SEM juiz” (`app/docs/JUIZES.md:54`), mas manda prevalecer o registro vivo (:10-14), onde C7 está registrado (`app/core/juizes.py:267`).
- **Segundo escritor fora da porta:** o diff não acrescenta escrita direta desses campos; a saída preservada passa por `p.carimbar` (`materializacao.py:1248-1249`). A guarda só protege os caminhos de S143 explicitamente cobertos (`disputa_emissao.py:1586`, :1671, :1710); proteção universal de toda chamada possível não vem dela (`materializacao.py:1395` não consulta o sinal).
- **Relógio fora da borda:** as novas funções usam só `ContextVar`, conjunto de pk e `in_atomic_block`, sem leitura de relógio (`materializacao.py:1103-1126`). As leituras de relógio próximas seguem nos carimbos/serviços já existentes (:1215; `disputa_emissao.py:1612`, :1669).
- **Derivação paralela:** não surge derivação nova de via ou lastro no trecho adicionado; o materializador continua lendo `declarou_hora`/`via_pelo_fato` (:1232-1249), e S143 continua usando C6/via pelo fato (`disputa_emissao.py:1592`, :1612, :1674, :1684).
- **Selo/RED sem mordida:** há mordida de escritor pela espionagem, mas os dois testes finais admitem sobrescrita; limites acima (`test_pep_reentrada.py:69-73`, :94-107). [nao verificado]
- **Ordem de trava:** a entrada fica atomic → ContextVar e a saída restaura token antes da saída de atomic (`materializacao.py:1191`, :1117-1121). Não aparece novo `select_for_update` nessas adições; comportamento concorrente real: [não verificado].
- **Pouso antes do pré-requisito:** carimbo final segue depois do plantio e da aprovação do juiz, ambos dentro do bloco (:1192-1210, :1248-1252); a cauda de fechamento regular segue depois do laço (:1346-1350). [nao verificado]
- **Flag global sobrevivente:** o objeto `ContextVar` é de módulo (:1103), mas o valor é instalado com token e restaurado no `finally` (:1117-1121); não é um booleano global de processo. Garantia de isolamento por thread/task e risco de contexto copiado requerem distinguir semântica do ContextVar de identidade de transação; execução desses contornos: [não verificado].

## 06/11 08:45 · sessao 4d47178d85d2 · - · - · origem=Ronald (tmux)

Escopo e aliases, por leitura estática:

- `_EM_VOO` é um objeto de módulo, mas o valor é um `frozenset` por contexto; o único escritor faz união com `p.pk` e restaura o token em `finally`. O fluxo normal não deixa o valor sobreviver ao próprio bloco. (`app/chamados/services/materializacao.py:1103`, `:1117`, `:1120`)
- Em `with atomic(), materializacao_em_voo(p)`, o marcador sai primeiro: o `reset` precede commit ou rollback do `atomic`. `_Reprovada` e exceções comuns são tratadas depois dessa saída; `return` interno também atravessa `finally`. (`app/chamados/services/materializacao.py:1191`, `:1199`, `:1210`, `:1254`, `:1256`)
- Rollback de savepoint interno não desfaz o marcador do bloco externo. Uma entrada aninhada restaura exatamente o contexto anterior: outra P acrescenta outro pk; a mesma P continua marcada quando o bloco interno sai. Isso não impede executar `_materializar_uma` duas vezes sobre a mesma P — é identificação, não trava de reentrada. (`app/chamados/services/materializacao.py:1117`, `:1121`, `:1129`)
- Callback normal de `on_commit` executado depois do bloco não verá a marca criada por esse bloco: ela já foi restaurada antes de `atomic.__exit__`. Há callbacks dessa forma nos receivers. (`app/chamados/services/materializacao.py:1191`, `:1121`; `app/chamados/signals.py:121`, `:314`)
- Outra thread/contexto independente não compartilha automaticamente o valor só porque o objeto está no módulo. A cópia configura `gthread`, quatro threads; implantação efetiva [não verificado]. (`app/chamados/services/materializacao.py:1103`; `app/gunicorn.conf.py:19`)
- A garantia universal “nada sobrevive” excede a implementação: um contexto explicitamente copiado ou uma tarefa que herde o contexto pode conservar o snapshot; o `reset` original restaura apenas o contexto original. Caminho concreto com `copy_context/create_task` na cópia: [não verificado]. O código armazena apenas pk, sem identidade de transação ou contexto filho. (`app/chamados/services/materializacao.py:1103`, `:1117`, `:1121`)
- Não é uma abstração segura para múltiplos aliases: `_em_atomic` consulta exclusivamente `django.db.connection`, e tanto o materializador como o receiver S143 abrem `atomic()` sem `using`. O receiver aceita `**kwargs`, mas não encaminha o alias do signal. (`app/chamados/services/materializacao.py:1106`, `:1191`; `app/chamados/signals.py:76`, `:91`)
- Os perfis SaaS e prod mostrados declaram somente `default`; SaaS usa `TenantSyncRouter` com backend de schemas. Não identifiquei chamador de S143 selecionando outra conexão; configuração efetiva de produção [não verificado]. (`app/config/settings/saas.py:83`, `:93`; `app/config/settings/prod.py:9`)
- Além do receiver, chamam S143 `_retratar`, `reatar_pergunta_orfa`, `fechar_beco` e o comando `reconciliar_fantasmas`, todos sem alias na chamada. (`app/chamados/reconciliador.py:536`, `:1631`; `app/chamados/services/becos.py:49`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`)
- O import local novo não introduz a dependência: `disputa_emissao` já importa `materializacao` no topo. O inverso existe como import local de `reabrir_para_correcao`; `models` reexporta primeiro `materializacao`, depois `disputa_emissao`. O import novo ocorre durante a função, depois da carga usual dos módulos, sem evidência estática de novo ciclo de inicialização. (`app/chamados/services/disputa_emissao.py:7`, `:1578`; `app/chamados/services/materializacao.py:642`; `app/chamados/models.py:3276`, `:3281`)
- `pergunta_em_voo` e seu import não consultam banco. A cauda nova, porém, avalia `d.perguntas.all()` para cada candidata aberta, mesmo quando `_EM_VOO` está vazio: acrescenta consulta/carregamento das perguntas nessa posição; custo medido [não verificado]. (`app/chamados/services/materializacao.py:1126`; `app/chamados/services/disputa_emissao.py:1710`)

## 06/11 08:45 · sessao 183ceb44a177 · LASTRO-PARTE-3 · verificador · origem=Code

**Há uma lacuna procedente, baixa: retirar a chamada de varredura de `_ancoradas` do selo deixa as cinco verificações verdes. A costura testa o scanner isoladamente, sem garantir sua inclusão no selo.** `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:252`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:283`.

1. **Kwargs alheios: possível; limitação procedente, baixa. Contaminação atual: não procedente.** O scanner percorre qualquer `ast.Call` e aceita qualquer kwarg cujo primeiro segmento coincida com campo da pergunta; não verifica método nem modelo. Assim, uma chamada hipotética `OutroModelo.objects.filter(recusa_motivo='x')` dentro da função seria contabilizada. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:233`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:235`.

   Na `_ancoradas` atual, todos os kwargs encontrados pertencem ao filtro de `PerguntaDisputa` ou aos seus `Q`. `select_related` e `order_by` recebem argumentos posicionais, que esse scanner não coleta. `app/chamados/models.py:327`; `app/chamados/models.py:328`; `app/chamados/models.py:329`; `app/chamados/models.py:330`.

   Essa sobreinclusão pode inflar `lidos`, mas não esconder campo ausente no teste principal: acrescentar elementos não elimina uma diferença já existente em `lidos - CAMPOS_QUE_ACORDAM`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:258`.

2. **Fecho FK escondendo um lado ausente: não procedente; sem severidade.** O fechamento amplia apenas os campos exigidos, não `CAMPOS_QUE_ACORDAM`. Para uma FK detectada, exige ambos os nomes; se faltar qualquer lado no conjunto produtivo, a diferença permanece não vazia. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:242`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:245`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:258`.

   Caso concreto: a leitura de `celula` exige também `celula_id`; retirar somente `celula_id` do conjunto reprova o selo principal. Hoje ambos estão presentes. `app/chamados/juizes.py:1147`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:219`; `app/chamados/models.py:2662`.

3. **Mordida das costuras: parcial; lacuna procedente, baixa.**

   | Mutante | Resultado das cinco verificações do selo |
   |---|---|
   | Retirar `celula_id` de `CAMPOS_QUE_ACORDAM` | O teste principal falha; a costura FK continua passando, pois sua própria expressão já subtrai esse campo. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:258`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:278`. |
   | Desativar o fechamento FK | A costura FK falha no `assertIn('celula_id', lidos)`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:277`. |
   | Fazer o scanner de kwargs retornar vazio | A costura do filtro falha. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:284`. |
   | Retirar somente a chamada do scanner de `_lidos_pelo_c6` | **Todas passam:** a costura chama o helper diretamente, e os campos atuais do filtro já aparecem nas outras funções. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:251`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:283`; `app/chamados/models.py:344`; `app/chamados/juizes.py:1136`. |

   Esses resultados foram reproduzidos em memória com os métodos extraídos da cópia e metadados substitutos derivados das declarações dos campos; execução integrada com Django: **[nao verificado]**. Código exercitado: `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:188`.

4. **Campo funcional direto faltante: não procedente; sem severidade.** As leituras diretas, filtros e pares FK resultam nos mesmos 12 nomes presentes em `CAMPOS_QUE_ACORDAM`: os seis campos escalares e os pares `celula`, `disputa` e `chamado_destino`. `app/chamados/juizes.py:1135`; `app/chamados/models.py:327`; `app/chamados/models.py:344`; `app/chamados/models.py:2661`; `app/chamados/models.py:2662`.

   Há duas ressalvas de escopo: `_ancoradas` usa `pk` para ordenação, fora do conjunto; e as leituras transitivas de `motivo`/`tipo_resposta` continuam fora, como declarado. A ordenação não acrescenta condição de seleção; a exclusão dos helpers permanece explícita, sem nova cura neste delta. `app/chamados/models.py:330`; `app/chamados/models.py:2660`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:195`; `app/chamados/juizes.py:886`; `app/chamados/juizes.py:1099`.

## 06/11 08:40 · sessao bf99151fb9fa · LASTRO-PARTE-3 · verificador · origem=Code

**O achado 3 está fechado nos caminhos encontrados que passam por `PerguntaDisputa.save`, no banco `default`. O RED tem mordida por inspeção; sua execução contra o mutante não foi verificada. O selo melhorou, mas ainda tem lacunas de cobertura.** `app/chamados/models.py:2847`; `app/chamados/signals.py:215`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:249`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:209`.

**1. Atomicidade e caminhos de escrita**

**Achado 3 anterior: corrigido; severidade alta eliminada nos caminhos abaixo.** O `atomic` envolve todo o `super().save()`, incluindo o receiver síncrono que chama a captura e a emissão. Não existe captura da exceção dentro desse receiver. `app/chamados/models.py:2854`; `app/chamados/signals.py:215`; `app/chamados/signals.py:221`.

| Caminho | Conferência |
|---|---|
| `save()` direto e `save(update_fields=...)` | Ambos passam pelo override. O receiver filtra `update_fields`, quando fornecido. `app/chamados/models.py:2847`; `app/chamados/signals.py:219`. |
| `carimbar` | Persiste por `self.save`; restaura carimbo e lavra em memória quando há exceção. `app/chamados/models.py:2609`; `app/chamados/models.py:2610`. |
| `carimbar_identidade` e `descarimbar` | Persistem pelo mesmo `save`. `app/chamados/models.py:2644`; `app/chamados/models.py:2652`. |
| `_escrever_carimbo` | Só atribui em memória; a persistência pertence às portas acima. `app/chamados/models.py:2556`. |
| `abrir` | Reutilização salva pelo override; nascimento usa `objects.create`, mas o receiver exclui deliberadamente `created=True`. `app/chamados/models.py:2474`; `app/chamados/models.py:2476`; `app/chamados/signals.py:216`. |
| Materialização | O carimbo passa por `save`; plantio e carimbo continuam dentro do `atomic` abrangente da materialização. `app/chamados/services/materializacao.py:1145`; `app/chamados/services/materializacao.py:1202`. |

**Existem bypasses; não equivalem a um defeito novo comprovado.** `app/chamados/models.py:2699`; `app/chamados/veredito_lavrado.py:207`.

- **Reabertura:** usa `update`, mas captura as linhas antes e emite no mesmo `atomic`, inclusive com `desfazer_absorcao=True`. **Não procedente como quebra de atomicidade.** `app/chamados/services/disputa_emissao.py:1401`; `app/chamados/services/disputa_emissao.py:1405`; `app/chamados/services/disputa_emissao.py:1414`; `app/chamados/services/disputa_emissao.py:1417`.
- **Ressuscitação:** chama `descarimbar_em_lote`, cujo `update` não emite evento. **Bypass técnico procedente; impacto funcional dos antigos achados 2/7 não procedente nos chamadores encontrados.** `app/chamados/models.py:1504`; `app/chamados/models.py:2699`.
- **O descarte de 2/7 procede:** `reabrir` grava o estado antes da ressuscitação; o cartório exige chamado em `VIVOS`; o desarquivamento exige sucesso de `reabrir` antes da ressuscitação adicional. `app/chamados/models.py:1393`; `app/chamados/models.py:1402`; `app/ponto/services/cartorio.py:1592`; `app/ponto/services/cartorio.py:1622`; `app/chamados/views_arquivar.py:70`; `app/chamados/views_arquivar.py:86`.
- **Backfill da lavra:** `bulk_update` escreve somente `veredito_lavrado`, `julgado_em` e `janela_expira_em`, excluídos dos campos que acordam. **Não procedente como omissão de evento desta obra.** `app/chamados/veredito_lavrado.py:194`; `app/chamados/veredito_lavrado.py:207`; `app/chamados/models.py:2660`.

A garantia tem limites explícitos: pergunta sem célula resolvível não emite; `raw` e nascimento são excluídos; o `atomic` não encaminha `using`. A configuração SaaS mostra somente `default`; caminho produtivo da pergunta em outro alias: **[nao verificado]**. `app/chamados/models.py:2685`; `app/chamados/signals.py:216`; `app/chamados/models.py:2854`; `app/config/settings/saas.py:83`.

As FKs `celula` e `chamado_destino` também permitem `SET_NULL`, uma via indireta de alteração fora desse `save`. Contraexemplo funcional por deleção no escopo da obra: **[nao verificado]**. `app/chamados/models.py:2210`; `app/chamados/models.py:2821`.

**2. Efeito do savepoint nos chamadores**

**Mudança de comportamento procedente; regressão não demonstrada.** O caso concreto é S144: o fechamento da disputa abre uma transação, dispara o receiver de materialização e depois escreve a trilha. Esse receiver captura `Exception`; o novo savepoint permite reverter a pergunta/evento sem inutilizar a transação externa. Isso corresponde ao best-effort declarado ali. `app/chamados/models.py:1846`; `app/chamados/models.py:1865`; `app/chamados/models.py:1866`; `app/chamados/signals.py:125`; `app/chamados/signals.py:140`; `app/chamados/signals.py:141`; `app/chamados/services/materializacao.py:1349`; `app/chamados/models.py:2854`.

Os demais casos relevantes são:

- **Reconciliador:** captura a exceção de `carimbar` e segue; agora a pergunta volta no banco, e `carimbar` restaura também a memória. `app/chamados/reconciliador.py:583`; `app/chamados/reconciliador.py:591`; `app/chamados/models.py:2610`; `app/chamados/models.py:2854`.
- **Materialização:** a captura ocorre depois de sair do `atomic` abrangente; o savepoint novo não conserva uma batida cujo carimbo falhou. `app/chamados/services/materializacao.py:1145`; `app/chamados/services/materializacao.py:1202`; `app/chamados/services/materializacao.py:1208`; `app/chamados/services/materializacao.py:1210`.
- **S143:** já tinha savepoint próprio e captura fora dele. `app/chamados/signals.py:88`; `app/chamados/signals.py:91`; `app/chamados/signals.py:93`.
- **`select_for_update`:** o desfazimento de carimbo mantém sua transação e suas travas anteriores; o `save` fica aninhado nela. `app/chamados/services/relavra_via.py:217`; `app/chamados/services/relavra_via.py:221`; `app/chamados/services/relavra_via.py:231`.
- **Callback do beco:** em autocommit, passa a aguardar a conclusão do `atomic` do save; uma falha na emissão impede esse sucesso. `app/chamados/signals.py:202`; `app/chamados/signals.py:221`; `app/chamados/models.py:2854`.

Captura explícita de `IntegrityError` ou `get_or_create` **da pergunta**, capaz de produzir outra mudança concreta: **[nao verificado]**. O `get_or_create` do gate examinado pertence a `ChamadoColaborador`. `app/chamados/models.py:932`.

**3. Mordida do RED de atomicidade**

**Por inspeção, morde o mutante “retirar o `atomic`”; hipótese de teste vacuamente verde não procedente.** O teste salva `validada_em`, exige a exceção e consulta novamente o banco antes do encerramento do teste. A transação externa do `TestCase`, sozinha, não desfaz a escrita naquele ponto apenas porque `assertRaises` capturou um `RuntimeError`. A fixture confirma previamente que `validada_em` era nulo. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:255`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:258`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:261`.

O patch atinge o nome correto: `acordar_celulas` importa `emitir_evento_celula` **dentro da função**, do mesmo módulo alterado pelo teste; não usa um alias previamente capturado no receiver. `app/chamados/models.py:2677`; `app/chamados/models.py:2688`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:258`.

Não localizei nesse teste outro rollback que mascarasse a retirada do override. Execução real do RED/GREEN e do mutante com Django: **[nao verificado]**. O teste cobre uma falha síncrona simulada na emissão, pelo caminho `save(update_fields=['validada_em'])`; não constitui uma execução de todas as portas. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:258`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:260`.

O antigo achado 9 também está corrigido no teste: agora exige pelo menos uma pergunta exercitada, mantém a igualdade exata e exige ausência de eventos adicionais na repetição. Resultado executado: **[nao verificado]**. `app/ponto/tests/test_o_outbox_celula.py:118`; `app/ponto/tests/test_o_outbox_celula.py:124`; `app/ponto/tests/test_o_outbox_celula.py:130`.

**4. Cobertura do selo**

**Cobertura dos campos diretos atuais: procedente. Cobertura universal: não.** O conjunto extraído atualmente contém `celula`, `chamado_destino`, `chamado_destino_id`, `data_evento`, `disputa`, `materializada_em`, `respondida_em`, `resposta_colab`, `validada_em` e `via_resolucao`; todos estão em `CAMPOS_QUE_ACORDAM`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:198`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:221`; `app/chamados/models.py:2660`.

**Lacuna residual do selo — procedente, severidade baixa; sem campo mutável atual descoberto faltando no conjunto.**

- `_ancoradas` não é inspecionada. Seus filtros por string e `Q` escapam como sítios; os campos atuais da pergunta nesses filtros já estão representados nas funções varridas. `app/chamados/models.py:327`; `app/chamados/models.py:328`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:221`.
- O scanner só reconhece atributos e `getattr` literal sobre `p`/`_p`. Apelidos diferentes, `values_list`, `F`, `Q` e strings de filtro escapam. A execução em memória do scanner real confirmou esses escapes com leituras mutantes de `recusa_motivo`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:196`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:209`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:212`.
- Retirar `celula_id` e `disputa_id` do conjunto não seria detectado: o resultado extraído contém `celula`/`disputa`, e o selo não exige os pares nome/attname. Hoje os pares estão presentes. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:204`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:227`; `app/chamados/models.py:2662`.
- Helpers continuam fora, expressamente declarado. Eles leem `motivo` e `tipo_resposta`, ausentes do conjunto; o nascimento os escreve, mas alteração produtiva posterior não foi demonstrada. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:193`; `app/chamados/juizes.py:886`; `app/chamados/juizes.py:1099`; `app/chamados/juizes.py:1103`; `app/chamados/models.py:2476`; `app/chamados/models.py:2477`.

As costuras de `validada_em` e da âncora agora existem; portanto “selo inteiramente sem mordida” não procede. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:229`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:235`.

**5. Quem escreve e quem lê**

O inventário abaixo identifica os escritores encontrados e os leitores **nesta cadeia**; não declara um inventário de todas as telas e relatórios. Os pontos de transporte são o receiver e a reabertura. `app/chamados/signals.py:221`; `app/chamados/services/disputa_emissao.py:1405`; `app/chamados/services/disputa_emissao.py:1417`.

| Elemento | Escritor/definição e leitores/chamadores |
|---|---|
| `CAMPOS_QUE_ACORDAM` da pergunta | Definido em `app/chamados/models.py:2660`; lido produtivamente pelo filtro em `app/chamados/signals.py:219`. |
| Conjunto homônimo da célula | Contém `veredito/ata`; lido por `salvar_e_emitir`. São conjuntos distintos. `app/ponto/portas/celula.py:663`; `app/ponto/portas/celula.py:682`. |
| `linhas_do_evento` | Lê `celula_id`, `disputa__colaborador_id`, `data_evento`; chamado pelo receiver e pela reabertura. `app/chamados/models.py:2668`; `app/chamados/signals.py:221`; `app/chamados/services/disputa_emissao.py:1405`. |
| `acordar_celulas` | Resolve célula por FK/fallback, deduplica localmente e chama a porta de emissão; seus chamadores são receiver e reabertura. `app/chamados/models.py:2682`; `app/chamados/models.py:2684`; `app/chamados/models.py:2688`; `app/chamados/signals.py:221`; `app/chamados/services/disputa_emissao.py:1417`. |
| `PerguntaDisputa.save` | Persiste por `super().save` dentro do novo `atomic`; portas e bypasses discriminados na resposta 1. `app/chamados/models.py:2854`; `app/chamados/models.py:2855`. |

| Campo da pergunta | Quem escreve | Quem lê nesta cadeia |
|---|---|---|
| `resposta_colab` | Resposta, automática e reabertura. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1433`; `app/chamados/services/disputa_emissao.py:1393`. | C6 e comparação com luz. `app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1113`. |
| `respondida_em` | Resposta, automática, trilha, carimbo, reabertura e ressuscitação. `app/chamados/services/disputa_emissao.py:1283`; `app/chamados/services/disputa_emissao.py:1434`; `app/chamados/services/disputa_emissao.py:1450`; `app/chamados/models.py:2589`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1504`. | C6. `app/chamados/juizes.py:1150`. |
| `validada_em` | Validação, devolução, resposta nova, carimbo, reabertura e ressuscitação. `app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/models.py:2592`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1504`. | C6 e coletor. `app/chamados/juizes.py:1136`; `app/chamados/models.py:327`. |
| `materializada_em` | Porta do carimbo e descarimbo coletivo. `app/chamados/models.py:2558`; `app/chamados/models.py:2699`. | C6. `app/chamados/juizes.py:1145`; `app/chamados/juizes.py:1177`; `app/chamados/juizes.py:1207`. |
| `via_resolucao` | Carimbo, lote, fato no espelho e relavra de via. `app/chamados/models.py:2560`; `app/chamados/models.py:2700`; `app/chamados/models.py:2709`; `app/chamados/services/relavra_via.py:123`. | C6. `app/chamados/juizes.py:1145`; `app/chamados/juizes.py:1171`. |
| `data_evento` | Nascimento e preenchimento de data ausente no reaponte. `app/chamados/models.py:2476`; `app/chamados/services/fio.py:834`. | C6, tranca, gate e captura. `app/chamados/juizes.py:1197`; `app/chamados/juizes.py:931`; `app/chamados/models.py:347`; `app/chamados/models.py:2668`. |
| `celula`/`celula_id` | Nascimento, reuso de `abrir`, além da política `SET_NULL`. `app/chamados/models.py:2477`; `app/chamados/models.py:2436`; `app/chamados/models.py:2210`. | C6 e captura/resolução do evento. `app/chamados/juizes.py:1147`; `app/chamados/models.py:2668`; `app/chamados/models.py:2682`. |
| `disputa`/`disputa_id` | Nascimento; mudança posterior: **[nao verificado]**. `app/chamados/models.py:2476`. | C6, gate e captura do colaborador. `app/chamados/juizes.py:1200`; `app/chamados/models.py:345`; `app/chamados/models.py:2668`. |
| `chamado_destino`/`_id` | Nascimento, reuso, reaponte e política `SET_NULL`. `app/chamados/models.py:2477`; `app/chamados/models.py:2450`; `app/chamados/services/fio.py:845`; `app/chamados/models.py:2821`. | Coletor, gate e helper de materialização. `app/chamados/models.py:328`; `app/chamados/models.py:344`; `app/chamados/juizes.py:743`. |

A captura lê ainda o colaborador da disputa; a emissão lê `pk/colaborador_id/data` da célula encontrada. O nascimento da disputa atribui seu colaborador. `app/chamados/models.py:2668`; `app/ponto/portas/celula.py:675`; `app/chamados/services/disputa_emissao.py:1182`.

| Campo de `EventoCelula` | Quem escreve | Quem lê |
|---|---|---|
| `pk` | Identidade da linha inserida pela porta. `app/ponto/portas/celula.py:675`. | Exclusão dos lidos e atualização do lote. `app/chamados/reconciliador.py:1335`; `app/chamados/reconciliador.py:1378`. |
| `celula_id` | Porta de emissão. `app/ponto/portas/celula.py:675`. | Vigia e semeadura. `app/chamados/reconciliador.py:1262`; `app/chamados/reconciliador.py:1284`. |
| `colaborador_id` | Mesma porta. `app/ponto/portas/celula.py:675`. | Agrupamento, carregamento e vigia de lastro. `app/chamados/reconciliador.py:1339`; `app/chamados/reconciliador.py:1345`; `app/ponto/services/lastro.py:421`. |
| `data` | Mesma porta. `app/ponto/portas/celula.py:675`. | Não delimita o consumo, agrupado por colaborador; outro leitor produtivo: **[nao verificado]**. `app/chamados/reconciliador.py:1339`. |
| `criado_em` | `auto_now_add`. `app/escala/models.py:1860`. | Ordenação e idade. `app/chamados/reconciliador.py:1332`; `app/chamados/reconciliador.py:1250`. |
| `processado_em` | Inicialmente nulo; consumidor marca sucesso. `app/escala/models.py:1861`; `app/chamados/reconciliador.py:1386`. | Pendentes, mortos e retenção. `app/chamados/reconciliador.py:1238`; `app/chamados/reconciliador.py:1244`; `app/chamados/reconciliador.py:1393`. |
| `tentativas` | Default zero; consumidor incrementa. `app/escala/models.py:1862`; `app/chamados/reconciliador.py:1380`. | Seleção de pendentes/mortos e incremento. `app/chamados/reconciliador.py:1238`; `app/chamados/reconciliador.py:1244`; `app/chamados/reconciliador.py:1380`. |
| `erro` | Default vazio; consumidor grava falha. `app/escala/models.py:1863`; `app/chamados/reconciliador.py:1380`. | Leitor produtivo adicional: **[nao verificado]**. |

**Segundo escritor de INSERT fora da porta: não procedente.** O novo helper chama `emitir_evento_celula`; a criação continua naquela porta. O consumidor já escreve os metadados de processamento, sem introduzir outro produtor. `app/chamados/models.py:2688`; `app/ponto/portas/celula.py:675`; `app/chamados/reconciliador.py:1380`; `app/chamados/reconciliador.py:1386`.

**6. Classes de reprovação**

| Classe | Veredito e severidade |
|---|---|
| Segundo juiz duplicando autoridade | **Não procedente no acréscimo; sem severidade.** Signal transporta, consumidor chama as autoridades existentes e C6 permanece registrado. `app/chamados/signals.py:221`; `app/chamados/reconciliador.py:1372`; `app/core/juizes.py:266`. |
| Segundo escritor fora da porta | **Não procedente; sem severidade.** `app/chamados/models.py:2688`; `app/ponto/portas/celula.py:675`. |
| Relógio fora da borda | **Procedente na cadeia herdada; média, não introduzida nesta obra.** C6 chama helper que lê `date.today()`. `app/chamados/juizes.py:1197`; `app/chamados/juizes.py:562`. |
| Derivação paralela de fonte única | **Não procedente como novo julgamento; sem severidade.** Há repetição logística de seleção FK/fallback, sem cálculo próprio de vitalidade. `app/chamados/models.py:2681`; `app/chamados/reconciliador.py:1295`. |
| Selo/RED sem mordida | **RED de atomicidade: hipótese não procedente por inspeção. Selo: lacuna parcial procedente, baixa**, discriminada na resposta 4. Execução Django: **[nao verificado]**. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:209`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:249`. |
| Inversão de trava | **Não demonstrada; severidade não atribuída.** Helper acrescenta SELECT simples e INSERT em evento sem FK; a marcação do consumidor ocorre depois, em transação curta. `app/chamados/models.py:2682`; `app/chamados/models.py:2688`; `app/escala/models.py:1856`; `app/chamados/reconciliador.py:1376`. |
| Pouso antes do pré-requisito | APIs necessárias presentes na cópia; ordem efetiva de commits/deploy: **[nao verificado]**. `app/ponto/portas/celula.py:666`; `app/chamados/reconciliador.py:1304`. |

A lavra não produz a realimentação apontada na triagem: seus três campos ficam fora do conjunto. Carimbos feitos pelo próprio consumidor podem gerar outra passagem, com convergência nos filtros encontrados; a deduplicação por colaborador vale por lote, e o laço tem teto de tempo. **Laço infinito não demonstrado.** `app/chamados/veredito_lavrado.py:231`; `app/chamados/models.py:2660`; `app/chamados/reconciliador.py:619`; `app/chamados/reconciliador.py:1339`; `app/chamados/reconciliador.py:1391`.

## 06/11 08:38 · sessao b5cf1af1f565 · - · - · origem=Ronald (tmux)

Q4 — **cobertura atual suficiente para os campos diretos; cobertura universal não procede.** O selo extrai atualmente `celula`, `chamado_destino`, `chamado_destino_id`, `data_evento`, `disputa`, `materializada_em`, `respondida_em`, `resposta_colab`, `validada_em`, `via_resolucao`; todos constam no conjunto. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:198`; `app/chamados/models.py:2660`.

- **Lacuna do selo — média, procedente quanto à promessa universal.** `_ancoradas` não integra as funções inspecionadas; seus filtros por strings e `Q()` escapam. Hoje seus campos de Pergunta relevantes já aparecem nas funções varridas, portanto não há omissão funcional atual demonstrada nessa coleta. `app/chamados/models.py:327`; `app/chamados/models.py:328`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:221`.
- **Apelidos e ORM — lacuna demonstrada em memória.** Executei `_lidos` real com `_meta` simulado a partir dos campos do modelo e funções reais compiladas preservando arquivo/linha. Mutantes acrescentando leitura de `recusa_motivo` por `alias = p; alias.recusa_motivo`, `values_list('recusa_motivo')`, `Q(recusa_motivo='x')` e `F('recusa_motivo')` produziram o mesmo conjunto; as asserções atuais continuariam passando. O scanner reconhece apenas atributos e `getattr` literais sobre `p`/`_p`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:196`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:209`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:212`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:227`.
- **Helpers — exclusão expressamente declarada.** `motivo` e `tipo_resposta` continuam lidos transitivamente e ausentes de `CAMPOS_QUE_ACORDAM`; não localizei escrita produtiva posterior ao nascimento na busca dirigida. Alterações dinâmicas: [nao verificado]. `app/chamados/juizes.py:886`; `app/chamados/juizes.py:1099`; `app/chamados/juizes.py:1103`; `app/chamados/juizes.py:672`; `app/chamados/models.py:2476`; `app/chamados/models.py:2660`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:193`.
- **Pares FK — lacuna adicional baixa.** Retirar `celula_id` e `disputa_id` do conjunto não seria detectado: o conjunto extraído contém somente `celula` e `disputa`. O scanner aceita nomes/attnames, mas não exige sua equivalência. Ele ainda conta atribuição como leitura, incluindo `_p.chamado_destino = self`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:204`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:209`; `app/chamados/models.py:350`.

Q6:

| Classe | Resultado |
|---|---|
| Segundo juiz | **Não procedente no acréscimo.** Signal transporta; consumidor usa as autoridades existentes; C6 permanece registrado. `app/chamados/signals.py:215`; `app/chamados/reconciliador.py:1354`; `app/chamados/reconciliador.py:1372`; `app/core/juizes.py:266`. |
| Segundo escritor do evento | **Não procedente.** Helper chama a porta; INSERT continua nela. `app/chamados/models.py:2688`; `app/ponto/portas/celula.py:675`. |
| Relógio fora da borda | **Procedente na cadeia herdada**, sem nova leitura nos helpers adicionados: C6 chama função que lê `date.today()`. `app/chamados/juizes.py:1197`; `app/chamados/juizes.py:562`; `app/chamados/models.py:2665`; `app/chamados/models.py:2671`. |
| Derivação paralela de julgamento | **Não procedente no acréscimo.** A repetição é logística, escolhendo célula por FK/fallback, sem julgar vitalidade. `app/chamados/models.py:2681`; `app/chamados/reconciliador.py:1295`. |
| Selo sem mordida universal | **Procedente, média**, com limites acima; a correção específica de `getattr` e da âncora está presente. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:212`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:235`. |
| Inversão de trava nova | **Não demonstrada.** Helper acrescenta SELECT simples e INSERT em evento sem FK; consumidor marca depois da reconciliação em atomic curto. `app/chamados/models.py:2682`; `app/chamados/models.py:2688`; `app/escala/models.py:1856`; `app/chamados/reconciliador.py:1376`. |
| Pouso antes do pré-requisito | APIs necessárias estão na cópia; ordem efetiva de deploy/commits: **[nao verificado]**. `app/ponto/portas/celula.py:666`; `app/chamados/reconciliador.py:1304`. |

Sonda apenas em memória, sem Django/banco e sem alterações de arquivos; resultado da suíte Django/RED histórico: **[nao verificado]**. [nao verificado]

## 06/11 08:37 · sessao fe5ab7bf9462 · - · - · origem=Ronald (tmux)

**Inventário e bypasses — conferência estática, somente leitura.**

| Elemento | Escritor / chamadores | Leitor |
|---|---|---|
| `PerguntaDisputa.CAMPOS_QUE_ACORDAM` | Declaração única em `app/chamados/models.py:2660` | Filtro produtivo do receiver em `app/chamados/signals.py:219`; testes em `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:225` |
| `linhas_do_evento` | Definição em `app/chamados/models.py:2665` | Receiver em `app/chamados/signals.py:221`; reabertura em `app/chamados/services/disputa_emissao.py:1405` |
| `acordar_celulas` | Definição em `app/chamados/models.py:2671` | Receiver em `app/chamados/signals.py:221`; reabertura em `app/chamados/services/disputa_emissao.py:1417` |
| `PerguntaDisputa.save` | Override envolve `super().save` em `transaction.atomic()` em `app/chamados/models.py:2853` | Carimbo comum em `app/chamados/models.py:2609`; identidade em `:2644`; descarimbo em `:2652`; reuso de `abrir` em `:2474`; respostas em `app/chamados/services/disputa_emissao.py:1301`, `:1435`, `:1451`; validação em `app/chamados/services/validacao.py:133`, `:190`; reaponte em `app/chamados/services/fio.py:847`; relavra de via em `app/chamados/services/relavra_via.py:125` |

| Campo da pergunta | Quem grava | Quem lê na cadeia |
|---|---|---|
| `resposta_colab` | Resposta em `app/chamados/services/disputa_emissao.py:1282`, automática em `:1433`; reabertura em `:1393`, persistida em `:1416` | C6 em `app/chamados/juizes.py:1150`; comparação com luz em `:1113` |
| `respondida_em` | Resposta em `app/chamados/services/disputa_emissao.py:1283`, automática em `:1434`, trilha em `:1450`; carimbo em `app/chamados/models.py:2589`; reabertura em `app/chamados/services/disputa_emissao.py:1393`; ressuscitação em `app/chamados/models.py:1504` | C6 em `app/chamados/juizes.py:1150` |
| `validada_em` | Validação em `app/chamados/services/validacao.py:131`, devolução em `:188`; resposta nova em `app/chamados/services/disputa_emissao.py:1295`; carimbo em `app/chamados/models.py:2592`; reabertura em `app/chamados/services/disputa_emissao.py:1393`; ressuscitação em `app/chamados/models.py:1504` | C6 em `app/chamados/juizes.py:1136`; coletor em `app/chamados/models.py:327` |
| `materializada_em` | Atribuição canônica em `app/chamados/models.py:2558`; persistência em `:2609`, `:2644`, `:2652`; lote em `:2699` | C6 em `app/chamados/juizes.py:1145`, `:1171`, `:1177`, `:1207` |
| `via_resolucao` | Carimbo em `app/chamados/models.py:2560`; lote em `:2700`; fato no espelho em `:2709`; relavra em `app/chamados/services/relavra_via.py:123` | C6 em `app/chamados/juizes.py:1145`, `:1171` |
| `data_evento` | Nascimento em `app/chamados/models.py:2476`; preenchimento ausente em `app/chamados/services/fio.py:834`, persistido em `:847` | C6 em `app/chamados/juizes.py:1197`; tranca em `:931`; gate em `app/chamados/models.py:347`; captura em `:2668` |
| `celula` / `_id` | Nascimento em `app/chamados/models.py:2477`; reuso em `:2436`, persistido em `:2474`; FK `SET_NULL` em `:2209` | C6 em `app/chamados/juizes.py:1147`; captura em `app/chamados/models.py:2668`; resolução em `:2682` |
| `disputa` / `_id` | Nascimento em `app/chamados/models.py:2476`; alteração posterior não localizada [nao verificado] | C6 em `app/chamados/juizes.py:1200`; gate em `app/chamados/models.py:345`; captura em `:2668` |
| `chamado_destino` / `_id` | Nascimento em `app/chamados/models.py:2477`; reuso em `:2450`, persistido em `:2474`; reaponte em `app/chamados/services/fio.py:845`, persistido em `:847`; FK `SET_NULL` em `app/chamados/models.py:2821` | Coletor em `app/chamados/models.py:328`; gate em `:344`; helper de materialização em `app/chamados/juizes.py:743` |

A captura lê `disputa__colaborador_id`; a emissão lê `pk`, `colaborador_id` e `data` da célula encontrada. `app/chamados/models.py:2668`, `:2682`, `:2684`; `app/ponto/portas/celula.py:675`.

| Campo do evento | Escritor | Leitor |
|---|---|---|
| `celula_id` | INSERT da porta em `app/ponto/portas/celula.py:675` | Vigia em `app/chamados/reconciliador.py:1262`; semeadura em `:1284`, `:1297` |
| `colaborador_id` | Mesma porta em `app/ponto/portas/celula.py:675` | Agrupamento em `app/chamados/reconciliador.py:1339`; carregamento em `:1345`; vigia de lastro em `app/ponto/services/lastro.py:421` |
| `data` | Mesma porta em `app/ponto/portas/celula.py:675` | Consumidor não usa para limitar o recorte, agrupando pelo colaborador em `app/chamados/reconciliador.py:1337`; outro leitor produtivo [nao verificado] |
| `criado_em` | `auto_now_add` em `app/escala/models.py:1860` | Ordenação em `app/chamados/reconciliador.py:1332`; idade em `:1250` |
| `processado_em` | Inicialmente nulo em `app/escala/models.py:1861`; consumidor em `app/chamados/reconciliador.py:1386` | Pendentes em `:1238`; mortos em `:1244`; retenção em `:1393` |
| `tentativas` | Default zero em `app/escala/models.py:1862`; incremento em `app/chamados/reconciliador.py:1380` | Filtros em `:1238`, `:1244`; incremento em `:1380` |
| `erro` | Default vazio em `app/escala/models.py:1863`; consumidor em `app/chamados/reconciliador.py:1380` | Outro leitor produtivo [nao verificado] |

**Segundo escritor fora da porta: não procedente no acréscimo, sem severidade.** O helper novo chama `emitir_evento_celula`; o INSERT permanece na porta. As atualizações de processamento continuam no consumidor, sem novo INSERT lateral. `app/chamados/models.py:2688`; `app/ponto/portas/celula.py:675`; `app/chamados/reconciliador.py:1380`, `:1386`.

**Bypass da ressuscitação: existe tecnicamente; achado funcional de ausência de reabertura não procedente nos chamadores encontrados, sem severidade demonstrada.** `ressuscitar_perguntas` chama o lote sem evento próprio. Entretanto, `reabrir` grava o estado antes de ressuscitar; o cartório exige `VIVOS`; desarquivar reabre antes da segunda ressuscitação. `app/chamados/models.py:1504`, `:2699`, `:1391`, `:1393`, `:1402`; `app/ponto/services/cartorio.py:1591`, `:1592`, `:1622`; `app/chamados/views_arquivar.py:70`, `:86`.

**Outros bypasses explícitos encontrados:** a reabertura usa `update`, com captura e evento no mesmo `atomic`; o backfill usa `bulk_update` somente dos campos de lavra, excluídos do conjunto que acorda. `app/chamados/services/disputa_emissao.py:1401`, `:1405`, `:1414`, `:1416`, `:1417`; `app/chamados/veredito_lavrado.py:194`, `:207`; `app/chamados/models.py:2660`.

**Limite adicional:** as FKs `celula` e `chamado_destino` declaram `SET_NULL`, uma possibilidade de atualização indireta pelo coletor de deleção. Contraexemplo funcional dessa via no escopo do diff: [nao verificado]. `app/chamados/models.py:2209`, `:2821`.

**Multi-DB:** o novo `atomic` não recebe `using`; o `save` aceita `**kwargs`. Caminho produtivo usando outro alias não localizado [nao verificado]; a configuração SaaS declara `default` e `TenantSyncRouter`. Não há base para reportar defeito alcançável por esse ponto. `app/chamados/models.py:2847`, `:2854`; `app/config/settings/saas.py:83`, `:93`.

## 06/11 08:37 · sessao fb07214db4c7 · - · - · origem=Ronald (tmux)

Q2 — **mudança de comportamento em falhas confirmada; regressão não demonstrada**:

- **S144 é o caso concreto mais relevante.** `DisputaSupervisao.fechar()` abre `atomic`, salva a disputa e escreve sua trilha; esse save dispara o receiver que chama `materializar_pendentes_aprovadas()` e captura `Exception` sem savepoint próprio. Essa função carimba perguntas. O novo `PerguntaDisputa.save()` passa a isolar a falha da pergunta/evento, permitindo que o fechamento externo prossiga; antes, uma exceção de banco podia deixar a transação externa marcada para rollback. É compatível com o best-effort declarado pelo receiver, e não prova regressão. `app/chamados/models.py:1846`, `app/chamados/models.py:1865`, `app/chamados/models.py:1866`; `app/chamados/signals.py:125`, `app/chamados/signals.py:132`, `app/chamados/signals.py:140`, `app/chamados/signals.py:141`; `app/chamados/services/materializacao.py:1343`, `app/chamados/services/materializacao.py:1349`; `app/chamados/models.py:2854`.

- **Reconciliador engole a exceção e continua**, mas agora a pergunta não fica gravada sem evento: chama `carimbar()` dentro do try e captura `Exception`; `carimbar()` restaura também carimbo e lavra em memória e relança. Sem transação externa, o novo `atomic` reverte pergunta/evento; se houver transação externa, fornece o savepoint local. A existência dessa transação externa em cada execução efetiva é [nao verificado]. `app/chamados/reconciliador.py:583`, `app/chamados/reconciliador.py:586`, `app/chamados/reconciliador.py:591`; `app/chamados/models.py:2605`, `app/chamados/models.py:2609`, `app/chamados/models.py:2610`, `app/chamados/models.py:2854`.

- **Materialização preserva o rollback abrangente.** `_materializar_uma()` já envolve plantio e carimbo num `atomic`, e captura a exceção depois de sair dele; portanto o savepoint da pergunta não permite conservar a batida quando seu carimbo/evento falha. O `finally` continua registrando a recusa. `app/chamados/services/materializacao.py:1144`, `app/chamados/services/materializacao.py:1145`, `app/chamados/services/materializacao.py:1202`, `app/chamados/services/materializacao.py:1208`, `app/chamados/services/materializacao.py:1210`, `app/chamados/services/materializacao.py:1221`.

- **S143 já isolava as falhas.** O receiver que resolve perguntas ao fechar chamado já tinha seu próprio `atomic`, com captura fora dele; não há redução nova do escopo externo de rollback nesse caminho. `app/chamados/signals.py:83`, `app/chamados/signals.py:88`, `app/chamados/signals.py:91`, `app/chamados/signals.py:93`.

- **`select_for_update` permanece nos mesmos chamadores.** O desfazimento de carimbo abre atomic, trava pergunta/disputa, verifica C6 e chama `descarimbar()`; não captura exceção localmente. O savepoint novo fica dentro dessa transação e não substitui a trava já adquirida. `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:227`, `app/chamados/services/relavra_via.py:231`.

- **Callbacks mudam de momento no save em autocommit:** `_becos_post_save_pergunta` registra `on_commit`; com o novo atomic, a callback só fica liberada após a sequência do save/receiver de outbox concluir. A emissão síncrona que falha impede o sucesso desse atomic. `app/chamados/signals.py:191`, `app/chamados/signals.py:202`, `app/chamados/signals.py:215`, `app/chamados/signals.py:221`; `app/chamados/models.py:2854`.

- **Captura explícita de `IntegrityError` do save de PerguntaDisputa ou `get_or_create` dessa entidade:** não localizada; ausência universal [nao verificado]. O `get_or_create` do gate mostrado é de `ChamadoColaborador`, sem relação com o override novo de save da pergunta. `app/chamados/models.py:932`.

- **Multi-DB:** o override usa `transaction.atomic()` sem `using`, e encaminha os argumentos a `super().save`; portanto a garantia não é genericamente estendida a outro alias. Contudo, a configuração saas apresentada só declara `default`, e nenhum chamador produtivo de PerguntaDisputa com outro alias foi localizado; defeito atual por multi-DB [nao verificado]. `app/chamados/models.py:2847`, `app/chamados/models.py:2854`, `app/chamados/models.py:2855`; `app/config/settings/saas.py:83`, `app/config/settings/saas.py:93`.

## 06/11 08:32 · sessao 44cd8afa75f9 · FIO-MORTO-POR-DONO-0611 · sonda · origem=Code

**PROD**

A conferência integral dos scripts ficou **[nao verificado]**: `conta_prod.py`, `sonda_dono.py` e `logs/a683/sonda_sem_fio.py` não foram encontrados na cópia. Os números abaixo são os dados raspados fornecidos, com as somas recalculadas; equivalência das sondas e hashes contra seus conteúdos: **[nao verificado]**.

O código permite conferir o contrato: universo deduplicado por colaborador/data nas competências consideradas; `limite=0` preserva todos os dias `sem_porta`; hoje/futuro é separado antes desse contador. Portanto, o balde `de_hoje_pra_frente=10` informado em PROD não integra seus 319 `sem_porta`. ([vigia_fio.py:581](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:581), [vigia_fio.py:625](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:625), [vigia_fio.py:648](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:648); valores PROD: [nao verificado])

| Medida de 06/11 | Universo | Sem porta | Fonte |
|---|---:|---:|---|
| PROD, contador vivo às 08:27:18 | 2.144 | **319** | Dado fornecido [nao verificado] |
| SOMBRA, dump das 05:02, consultado às 08:28:22 | 2.127 | **322** | Dado fornecido [nao verificado] |
| PROD − SOMBRA | +17 | −3 | Cálculo sobre o dado [nao verificado] |

O dado compara o estado do dump das 05:02 com uma leitura posterior de PROD. Mostra universos diferentes e três dias sem porta a menos em PROD; quais mudanças produziram essa diferença e qualquer atribuição a pouso: **[nao verificado]**. [nao verificado]

**SOMBRA**

O método `InsumosDoDono.dono` chama o juiz que retorna ESTRUTURA, CADASTRO ou BATIDA; a classe do censo não substitui essa medição. A implementação da convenção “não medido → INDETERMINADO” na sonda ausente: **[nao verificado]**. ([e6_oraculo.py:172](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:172), [e6_oraculo.py:98](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:98))

Há uma inconsistência no pedido: **247/159/12 não corresponde ao `TAB` nem ao `TRANS` de 05/11 fornecidos**, que reconstituem **244/162/12**. A tabela abaixo usa essa decomposição, também registrada na cópia; reconciliar os três dias entre essas duas distribuições: **[nao verificado]**. ([RELATO.md:38](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:38), [RELATO.md:40](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:40))

As classes `e-terminal` e `e-sem-viva` correspondem, respectivamente, aos detalhes `pergunta_viva_sob_chamado_terminal` e `sem_pergunta_viva` do censo. ([vigia_fio.py:685](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:685))

“Novos” significa **fora dos 418 sem porta de 05/11**; não demonstra entrada nova no universo. O motivo das quatro saídas do universo, a identidade das diferenças entre PROD e SOMBRA e a causa operacional das mudanças de rota ficam **[nao verificado]**.

| SOMBRA | Universo | Sem porta | ESTRUTURA | CADASTRO | BATIDA | INDETERMINADO | Fonte |
|---|---:|---:|---:|---:|---:|---:|---|
| 05/11, `TAB`/`TRANS` | 1.987 | 418 | 244 | 162 | 12 | 0 | Anexo [nao verificado] |
| 06/11 | 2.127 | 322 | 278 | 31 | 13 | 0 | Anexo [nao verificado] |

| HOJE, por dono | Passado | Hoje pra frente | Total | Fonte |
|---|---:|---:|---:|---|
| ESTRUTURA | 278 | 0 | **278** | Anexo [nao verificado] |
| CADASTRO | 31 | 0 | **31** | Anexo [nao verificado] |
| BATIDA | 13 | 0 | **13** | Anexo [nao verificado] |
| INDETERMINADO | 0 | 0 | **0** | Anexo [nao verificado] |
| **Total** | **322** | **0** | **322** | Anexo [nao verificado] |

| Classe — valores **05/11 → 06/11** | ESTRUTURA | CADASTRO | BATIDA | INDETERMINADO | Total | Fonte |
|---|---:|---:|---:|---:|---:|---|
| a | 0 → 0 | 0 → 0 | 2 → 2 | 0 → 0 | **2 → 2** | Anexo [nao verificado] |
| b | 0 → 0 | 1 → 0 | 0 → 0 | 0 → 0 | **1 → 0** | Anexo [nao verificado] |
| c | 8 → 8 | 20 → 5 | 2 → 3 | 0 → 0 | **30 → 16** | Anexo [nao verificado] |
| d | 185 → 200 | 9 → 14 | 3 → 3 | 0 → 0 | **197 → 217** | Anexo [nao verificado] |
| e-terminal | 33 → 31 | 132 → 12 | 5 → 5 | 0 → 0 | **170 → 48** | Anexo [nao verificado] |
| e-sem-viva | 18 → 39 | 0 → 0 | 0 → 0 | 0 → 0 | **18 → 39** | Anexo [nao verificado] |
| **Total** | **244 → 278** | **162 → 31** | **12 → 13** | **0 → 0** | **418 → 322** | Anexo [nao verificado] |

| Destino dos 418 de 05/11 | Dias | Detalhamento | Fonte |
|---|---:|---|---|
| Saíram do universo porque a competência saiu | **0** | Nenhuma ocorrência informada | `TRANS` [nao verificado] |
| Saíram do universo com competência ainda presente | **4** | c/CADASTRO: 1; d/BATIDA: 1; e-terminal/ESTRUTURA: 2 | `TRANS` [nao verificado] |
| Seguem no universo → `fio_do_admin` | **135** | b/CADASTRO: 1; c/CADASTRO: 14; e-terminal/CADASTRO: 120 | `TRANS` [nao verificado] |
| Seguem no universo → `porta_pergunta` | **1** | e-terminal/CADASTRO: 1 | `TRANS` [nao verificado] |
| Seguem sem porta, mesma classe e dono | **278** | Decomposição abaixo | `TRANS` [nao verificado] |
| Seguem sem porta, classe ou dono mudou | **0** | Nenhuma ocorrência informada | `TRANS` [nao verificado] |
| **TRANS_SOMA** | **418** | **0 + 4 + 135 + 1 + 278 + 0 = 418** | Cálculo sobre o anexo [nao verificado] |

| Classe/dono | Seguem sem porta, mesma classe/dono | Novos sem porta | HOJE | Fonte |
|---|---:|---:|---:|---|
| a / BATIDA | 2 | 0 | 2 | Anexo [nao verificado] |
| b / CADASTRO | 0 | 0 | 0 | Anexo [nao verificado] |
| c / ESTRUTURA | 8 | 0 | 8 | Anexo [nao verificado] |
| c / CADASTRO | 5 | 0 | 5 | Anexo [nao verificado] |
| c / BATIDA | 2 | 1 | 3 | Anexo [nao verificado] |
| d / ESTRUTURA | 185 | 15 | 200 | Anexo [nao verificado] |
| d / CADASTRO | 9 | 5 | 14 | Anexo [nao verificado] |
| d / BATIDA | 2 | 1 | 3 | Anexo [nao verificado] |
| e-terminal / ESTRUTURA | 31 | 0 | 31 | Anexo [nao verificado] |
| e-terminal / CADASTRO | 11 | 1 | 12 | Anexo [nao verificado] |
| e-terminal / BATIDA | 5 | 0 | 5 | Anexo [nao verificado] |
| e-sem-viva / ESTRUTURA | 18 | 21 | 39 | Anexo [nao verificado] |
| **Total** | **278** | **44** | **322** | Anexo [nao verificado] |

| Conferência | Soma recalculada | Resultado informado | Fonte |
|---|---|---|---|
| Universo PROD | 1.038 + 330 + 10 + 288 + 14 + 2 + 143 + 319 = **2.144** | Confere | Anexo [nao verificado] |
| Universo SOMBRA 05/11 | 1.000 + 312 + 0 + 238 + 17 + 0 + 2 + 418 = **1.987** | Confere | Anexo [nao verificado] |
| Universo SOMBRA 06/11 | 1.035 + 323 + 0 + 288 + 15 + 0 + 144 + 322 = **2.127** | Confere | Anexo [nao verificado] |
| `SOMA_CONFERE dono` | 278 + 31 + 13 + 0 = **322** | True | Anexo [nao verificado] |
| `SOMA_CONFERE classe` | 2 + 0 + 16 + 217 + 48 + 39 = **322** | True | Anexo [nao verificado] |
| `SOMA_CONFERE tempo` | 322 + 0 = **322** | True | Anexo [nao verificado] |
| `TRANS_SOMA` | 4 + 136 + 278 = **418** | True | Anexo [nao verificado] |
| `HOJE = SEGUE + NOVO` | 278 + 44 = **322** | True | Anexo [nao verificado] |

`rc_prod=0; rc_sombra=0; hashes impressos: conta_prod.py=d1c151d5c135c518, sonda_dono.py=454e4be486841a45, sonda_sem_fio.py=24efa49ef6157099; somas: universos 2144/1987/2127, dono=classe=tempo=322, TRANS=418, SEGUE+NOVO=278+44=322.` — dado fornecido [nao verificado].

## 06/11 08:30 · sessao f8387567b3d9 · - · - · origem=Ronald (tmux)

Auditoria somente leitura do censo:

- Universo = dias retornados por `dias_que_seguram`, nas competências de `competencias_a_lavrar`, para empresas visíveis; chave única `(colaborador_id, data)` e contador `len(dias)`. [app/chamados/services/vigia_fio.py:566](app/chamados/services/vigia_fio.py:566), [app/chamados/services/vigia_fio.py:581](app/chamados/services/vigia_fio.py:581)
- `dias_que_seguram` seleciona colaboradores retidos por `furo_espelho` e lê seus dias de retenção. [app/folha/fora_da_folha.py:118](app/folha/fora_da_folha.py:118)
- Competências = atual e anterior enquanto esta não estiver entregue; empresas visíveis = ativas, excluído prefixo de diagnóstico. [app/ponto/services/he_pendente_lavrado.py:220](app/ponto/services/he_pendente_lavrado.py:220), [app/colaboradores/queries.py:85](app/colaboradores/queries.py:85)
- Ordem da classificação: porta alcançável → hoje/futuro → baldes legais → sem porta. Consequentemente, `sem_porta` nesta implementação só contém passado; `de_hoje_pra_frente=10` de PROD pertence a outro balde, não aos 319 sem porta. [app/chamados/services/vigia_fio.py:642](app/chamados/services/vigia_fio.py:642), [app/chamados/services/vigia_fio.py:648](app/chamados/services/vigia_fio.py:648), [app/chamados/services/vigia_fio.py:654](app/chamados/services/vigia_fio.py:654)
- `limite=0` não afeta contador nem omite dia sem porta; afeta somente a listagem das outras rotas. [app/chamados/services/vigia_fio.py:625](app/chamados/services/vigia_fio.py:625)
- Classes: a = véu vivo; b = resposta vazia; c = respondida com destino terminal; d = sem pergunta viva, colaborador na lista e dia fora dela; e = demais detalhes. `e-sem-viva` corresponde a `sem_pergunta_viva`; `e-terminal` a `pergunta_viva_sob_chamado_terminal`. A classe d não mede dono. [app/chamados/services/vigia_fio.py:667](app/chamados/services/vigia_fio.py:667), [app/chamados/services/vigia_fio.py:677](app/chamados/services/vigia_fio.py:677), [app/chamados/services/vigia_fio.py:685](app/chamados/services/vigia_fio.py:685)
- `fio_do_admin` é balde distinto, aplicado após as três primeiras leis quando `lista.dia_do_admin` responde verdadeiro. [app/chamados/services/vigia_fio.py:306](app/chamados/services/vigia_fio.py:306)
- Porta é chamada da célula na fila ou pergunta viva no app com âncora na fila; a rota prefere `porta_fila` se ambas existirem. [app/chamados/juizes.py:1373](app/chamados/juizes.py:1373)
- Formato anexado coincide com `linhas_alcance` da cópia. [app/chamados/services/vigia_fio.py:705](app/chamados/services/vigia_fio.py:705)

Não localizei `logs/a683/sonda_sem_fio.py`, `sonda_dono.py` ou `conta_prod.py` na cópia; conferir os scripts, sua equivalência ou recalcular seus hashes permanece **[não verificado]**. `.gitignore` ignora `logs/`, mas isso não prova por que os arquivos não estão presentes. [.gitignore:53](.gitignore:53)

O valor entre colchetes em `sonda_leitura:` é o prefixo de 16 caracteres do SHA-256 da cópia do script efetivamente executada, não hash da árvore. [bin/sonda_leitura.sh:219](bin/sonda_leitura.sh:219), [bin/sonda_leitura.sh:221](bin/sonda_leitura.sh:221)

## 06/11 08:30 · sessao 34229530d650 · - · - · origem=Ronald (tmux)

- `InsumosDoDono.dono` reúne cadastro, turno e fase e chama o juiz único; consulta CxR quando necessário: `app/ponto/management/commands/e6_oraculo.py:172-183`.
- A precedência executável é: folga indefinida/sem vínculo → CADASTRO; batida fora do turno ou contagem ímpar → BATIDA; fase deslocada → CADASTRO; CxR → CADASTRO; restante → ESTRUTURA. `app/ponto/management/commands/e6_oraculo.py:98-110`.
- O carregador de CxR usa `dias_cadastro_x_realidade` do resultado do motor. `app/ponto/management/commands/e6_oraculo.py:154-162`.
- O juiz retorna somente esses três donos; INDETERMINADO deve indicar ausência de medição, conforme a convenção do dado fornecido. `app/ponto/management/commands/e6_oraculo.py:98-110`; implementação dessa convenção na sonda: [não verificado].
- Os scripts `sonda_sem_fio.py` e `sonda_dono.py` não foram encontrados na cópia pesquisada; equivalência e hashes das sondas: [não verificado].
- Há divergência no próprio pedido: o cabeçalho informa 05/11 = ESTRUTURA 247 / CADASTRO 159 / BATIDA 12, enquanto `TAB` e `TRANS` fornecidos somam **244 / 162 / 12**, total 418. Valores documentados também em `app/docs/RELATO.md:40-41`; prevalência do cabeçalho sobre os anexos: [não verificado].

## 06/11 08:27 · sessao 9de3fb453a9a · LASTRO-PARTE-3 · verificador · origem=Code

1. **PROCEDENTE — inventário de escritores e leitores; sem gravidade própria.** O conjunto da pergunta é declarado em `app/chamados/models.py:2660`; quem o lê em produção é o filtro do receiver, em `app/chamados/signals.py:219`. O conjunto homônimo da **célula** contém `veredito/ata` e é consumido por `salvar_e_emitir`; são conjuntos distintos. `app/ponto/portas/celula.py:663`; `app/ponto/portas/celula.py:682`.

   | Campo da PerguntaDisputa | Quem escreve | Quem lê nesta cadeia |
   |---|---|---|
   | `resposta_colab` | Resposta normal/automática e reabertura. `app/chamados/services/disputa_emissao.py:1282`, `:1433`, `:1393` | C6 decide pela resposta preenchida; helper compara com a luz. `app/chamados/juizes.py:1150`, `:1113` |
   | `respondida_em` | Resposta normal/automática/pela trilha; carimbo com resposta; reabertura e ressuscitação. `app/chamados/services/disputa_emissao.py:1283`, `:1434`, `:1450`; `app/chamados/models.py:2589`, `:1504`; `app/chamados/services/disputa_emissao.py:1393` | C6. `app/chamados/juizes.py:1150` |
   | `validada_em` | Validação, devolução, correção da resposta, carimbo com validação, reabertura e ressuscitação. `app/chamados/services/validacao.py:131`, `:188`; `app/chamados/services/disputa_emissao.py:1295`, `:1393`; `app/chamados/models.py:2592`, `:1504` | C6 e coletor do gate. `app/chamados/juizes.py:1136`; `app/chamados/models.py:327` |
   | `materializada_em` | `_escrever_carimbo`, persistido por `carimbar`, `carimbar_identidade`, `descarimbar`; lote por `descarimbar_em_lote`. `app/chamados/models.py:2558`, `:2609`, `:2644`, `:2652`, `:2699` | C6. `app/chamados/juizes.py:1145`, `:1171`, `:1177`, `:1207` |
   | `via_resolucao` | Portas do carimbo; também `encerrar_por_fato_no_espelho` e `relavrar`. `app/chamados/models.py:2560`, `:2709`; `app/chamados/services/relavra_via.py:123` | C6. `app/chamados/juizes.py:1145`, `:1171` |
   | `data_evento` | Nascimento e preenchimento de data ausente em `reapontar_resolvedora`. `app/chamados/models.py:2476`; `app/chamados/services/fio.py:832–847` | C6, tranca, gate e captura do evento. `app/chamados/juizes.py:1197`, `:931`; `app/chamados/models.py:347`, `:2668` |
   | `celula` / `celula_id` | Nascimento e reutilização em `abrir`; a FK também declara `SET_NULL`. `app/chamados/models.py:2477`, `:2435–2474`, `:2209` | C6 lê a relação; emissor captura e resolve a FK. `app/chamados/juizes.py:1147`; `app/chamados/models.py:2668`, `:2681` |
   | `disputa` / `disputa_id` | Nascimento; alteração posterior não demonstrada. `app/chamados/models.py:2476`; [nao verificado] | C6 obtém colaborador; captura obtém `disputa__colaborador_id`; gate obtém o chamado pai. `app/chamados/juizes.py:1200`; `app/chamados/models.py:2668`, `:328` |
   | `chamado_destino` / `_id` | Nascimento, reutilização em `abrir` e `reapontar_resolvedora`; FK declara `SET_NULL`. `app/chamados/models.py:2477`, `:2450–2474`, `:2821`; `app/chamados/services/fio.py:845–847` | Coletor/gate e helper de materialização efetiva. `app/chamados/models.py:328`, `:344–350`; `app/chamados/juizes.py:743` |
   | `motivo`, `tipo_resposta` | Nascimento pela porta `abrir`; alteração posterior não localizada. `app/chamados/models.py:2476–2477`; [nao verificado] | **Leitura transitiva do C6**, pelos helpers de luz, marco e materialização. `app/chamados/juizes.py:1099–1103`, `:886`, `:672–674` |

   A captura lê ainda o colaborador da disputa; a emissão usa `pk/colaborador_id/data` da célula encontrada. Há criação desses identificadores nas portas de disputa e célula. `app/chamados/models.py:2668`, `:2682–2688`; `app/chamados/services/disputa_emissao.py:1182–1186`; `app/ponto/portas/celula.py:253`.

   | Campo de EventoCelula | Quem escreve | Quem lê |
   |---|---|---|
   | `id` | Identidade da linha criada pela porta. `app/ponto/portas/celula.py:675` | Exclusão dos já lidos e atualização do lote. `app/chamados/reconciliador.py:1332–1341`, `:1378` |
   | `celula_id` | `emitir_evento_celula`. `app/ponto/portas/celula.py:675` | Vigia e deduplicação da semeadura. `app/chamados/reconciliador.py:1262`, `:1284`, `:1297` |
   | `colaborador_id` | Mesma porta. `app/ponto/portas/celula.py:675` | Agrupamento e carregamento do colaborador; também vigia de lastro. `app/chamados/reconciliador.py:1339–1345`; `app/ponto/services/lastro.py:421` |
   | `data` | Mesma porta. `app/ponto/portas/celula.py:675` | Não delimita o consumo: este varre o colaborador inteiro. Outro leitor produtivo não localizado. `app/chamados/reconciliador.py:1339–1372`; [nao verificado] |
   | `criado_em` | `auto_now_add`. `app/escala/models.py:1860` | Ordenação e idade do pendente. `app/chamados/reconciliador.py:1332`, `:1250–1251` |
   | `processado_em` | Inicialmente nulo; consumidor marca sucesso. `app/escala/models.py:1861`; `app/chamados/reconciliador.py:1386` | Seleção de pendentes, mortos e retenção. `app/chamados/reconciliador.py:1238`, `:1244`, `:1393` |
   | `tentativas` | Inicialmente zero; consumidor incrementa em falha. `app/escala/models.py:1862`; `app/chamados/reconciliador.py:1380` | Seleção de pendentes/mortos e incremento. `app/chamados/reconciliador.py:1238`, `:1244`, `:1380` |
   | `erro` | Inicialmente vazio; consumidor grava as falhas. `app/escala/models.py:1863`; `app/chamados/reconciliador.py:1380` | Leitor produtivo não localizado. [nao verificado] |

2. **PROCEDENTE — bypass existente; gravidade baixa nos chamadores encontrados.** O escritor que atende à pergunta é `ressuscitar_perguntas → descarimbar_em_lote`: altera `respondida_em`, `validada_em`, `materializada_em` e `via_resolucao` por `update`, sem `post_save`, sem `reabrir_para_correcao` e sem emissão própria. `app/chamados/models.py:1504`; `app/chamados/models.py:2699–2700`.

   O outro caminho coletivo, `reabrir_para_correcao`, está coberto pela emissão explícita. O `bulk_update` da lavra escreve somente `veredito_lavrado/julgado_em/janela_expira_em`, não os insumos diretos do C6. `app/chamados/services/disputa_emissao.py:1413–1417`; `app/chamados/veredito_lavrado.py:194–207`.

   Outros bypasses por raw SQL, escritores dinâmicos ou alteração produtiva de `motivo/tipo_resposta`: **não demonstrados**. [nao verificado]

3. **PROCEDENTE — alta: falta atomicidade entre pergunta e evento em caminhos de produção.** O receiver não abre transação; `carimbar()` também não envolve seu `save` em `atomic`. O consumidor chama `carimbar()` sem transação abrangente e engole sua exceção. Assim, **em autocommit, falha no INSERT do evento pode deixar a pergunta gravada sem evento**, embora o método tenha lançado exceção. `app/chamados/signals.py:215–221`; `app/chamados/models.py:2605–2613`; `app/chamados/reconciliador.py:582–592`, `:1354`.

   Nesse caminho, a falha não entra na lista `falhas` do consumidor, que pode marcar o evento anterior como processado. A pergunta carimbada também deixa o filtro da próxima tentativa. `app/chamados/reconciliador.py:591–592`, `:619`, `:1354`, `:1386`. Ocorrência efetiva dessa falha em produção: [nao verificado].

   **Evento sobrevivendo ao rollback: ausente nos caminhos demonstrados.** A reabertura insere no mesmo `atomic` do update; o aceite de folga pode emitir e depois pedir rollback, mas evento e validação pertencem à mesma transação. `app/chamados/services/disputa_emissao.py:1401–1417`; `app/chamados/services/validacao.py:289–313`.

   **Leitura externa antes do commit: não demonstrada.** O signal apenas insere; quem consome faz outra consulta à fila. A validação de hora protege carimbo e eventual devolução no mesmo `atomic`. `app/chamados/signals.py:221`; `app/chamados/reconciliador.py:1332`; `app/chamados/services/validacao.py:395–427`. A sincronização do signal, isoladamente, não prova publicação antecipada; o defeito comprovável é a ausência de transação comum quando o escritor está em autocommit. `app/chamados/models.py:2609`; `app/chamados/signals.py:221`.

4. **Hipótese afastada — sem gravidade: a reabertura não muda a identidade capturada.** Seu update limpa resposta e validação; `desfazer_absorcao` acrescenta apenas limpeza do carimbo. Não altera `celula`, `data_evento`, `disputa` nem colaborador. Portanto, pelo código dessa porta, a célula de antes é a mesma de depois. `app/chamados/services/disputa_emissao.py:1393`, `:1405`, `:1413–1417`; `app/chamados/models.py:2699–2700`.

   Mudança concorrente desses identificadores por outro escritor durante a captura: [nao verificado].

5. **PROCEDENTE — realimentação presente, com convergência nos caminhos examinados; sem laço infinito demonstrado.** O consumidor carimba perguntas, esses carimbos acordam o outbox e o laço pode consumir os novos IDs na mesma execução. `app/chamados/reconciliador.py:1354`, `:1364`, `:1331–1335`; `app/chamados/models.py:2585–2609`.

   Os pontos de convergência são concretos: perguntas absolvidas e órfãs deixam os universos que exigem `materializada_em IS NULL`; `encerrar_fantasma` possui ainda guarda de no-op. `app/chamados/reconciliador.py:617–620`, `:1037–1040`; `app/chamados/models.py:2718–2721`.

   `_gravar_estado` relavra perguntas, mas essa lavra salva apenas os três campos de cache, excluídos do conjunto que acorda; portanto essa escrita não gera outro evento da pergunta. `app/chamados/models.py:291–295`; `app/chamados/veredito_lavrado.py:227–231`; `app/chamados/models.py:2660–2662`; `app/chamados/signals.py:219–220`.

   Convergência universal sob alterações concorrentes: [nao verificado].

6. **Hipótese de rajada por `save()` sem `update_fields` — não demonstrada.** Os escritores identificados usam listas explícitas, inclusive resposta, validação, carimbo, reaponte e relavra; o lote de backfill usa `bulk_update` só dos campos da lavra. `app/chamados/services/disputa_emissao.py:1301`; `app/chamados/services/validacao.py:133`; `app/chamados/models.py:2609`, `:2644`, `:2474`; `app/chamados/services/fio.py:847`; `app/chamados/services/relavra_via.py:125`; `app/chamados/veredito_lavrado.py:207`.

   **Há amplificação prevista mesmo com `update_fields`:** N saves que toquem campos relevantes, todos com célula, podem produzir N eventos, inclusive para a mesma célula, porque `vistos` é local a cada chamada. Na reabertura coletiva, a deduplicação vale para todas as linhas daquela chamada. `app/chamados/signals.py:219–221`; `app/chamados/models.py:2678–2688`; `app/chamados/services/disputa_emissao.py:1417`.

   Volume real, frequência e custo em produção: [nao verificado].

7. **PROCEDENTE — omissão de `ressuscitar_perguntas`, mas necessidade de REABRIR afastada nos chamadores atuais.** Ela pode tornar uma pergunta viva: simula a remoção dos carimbos, exige C6 viva e depois faz o update correspondente. `app/chamados/models.py:1455–1458`, `:1504`.

   Porém, nos chamadores encontrados, o chamado já está vivo: `reabrir()` grava seu novo estado **antes** de ressuscitar; o cartório pula chamados fora de `VIVOS`; o desarquivamento reabre antes da ressuscitação adicional. `app/chamados/models.py:1390–1402`; `app/ponto/services/cartorio.py:1591–1593`, `:1622`; `app/chamados/views_arquivar.py:70`, `:86`.

   Portanto, não há contraexemplo demonstrado de chamado terminal precisando renascer por essa omissão. Isso sustenta a exceção nos chamadores atuais, não a frase geral “tornar viva nunca exige evento”. `app/chamados/models.py:1390–1402`; `app/ponto/services/cartorio.py:1591–1622`. Outros chamadores em produção: [nao verificado].

8. **PROCEDENTE — média: o selo promete cobertura maior que a implementada; demais classes discriminadas abaixo.**

   | Classe | Veredito e prova |
   |---|---|
   | Segundo juiz duplicando autoridade | **Ausente no acréscimo.** O signal transporta o evento; o consumidor chama as autoridades existentes. C6 continua registrado. `app/chamados/signals.py:215–221`; `app/chamados/reconciliador.py:1353–1372`; `app/core/juizes.py:266` |
   | Segundo escritor de EventoCelula fora da porta | **Ausente no acréscimo.** O helper chama `emitir_evento_celula`, que faz o INSERT. `app/chamados/models.py:2688`; `app/ponto/portas/celula.py:675` |
   | Relógio fora da borda | **Presente na cadeia herdada:** `_dentro_da_janela_viva` lê `date.today()`, chamado pelo C6. Os novos helpers não acrescentam relógio. `app/chamados/juizes.py:562`, `:1197`; `app/chamados/models.py:2664–2689` |
   | Derivação paralela de julgamento | **Ausente no acréscimo.** Há repetição logística da escolha FK/fallback usada na semeadura, sem novo cálculo de vitalidade. `app/chamados/models.py:2681–2688`; `app/chamados/reconciliador.py:1295–1299` |
   | Selo sem mordida suficiente | **Presente.** O AST só percorre `_pergunta_viva_calc` e atributos diretos `p.campo`; não visita helpers, `getattr` nem o coletor do gate. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:200–209` |
   | Inversão de trava introduzida | **Não demonstrada.** A emissão acrescenta SELECT simples de célula e INSERT; o consumidor marca eventos num atomic posterior à reconciliação. `app/chamados/models.py:2682–2688`; `app/chamados/reconciliador.py:1376–1386` |
   | Pouso antes do pré-requisito | As APIs de porta/consumo estão na cópia. Ordem efetiva de commits, deploy e pré-requisitos operacionais: [nao verificado]. `app/ponto/portas/celula.py:666`; `app/chamados/reconciliador.py:1304` |

   **O selo não cobre o coletor que lê `chamado_destino`.** Retirar `chamado_destino/chamado_destino_id` do conjunto não seria detectado por ele, embora o gate use esses campos; hoje ambos estão incluídos. Também ignora `getattr(p, 'celula')` e as leituras transitivas de `motivo/tipo_resposta`, estes ausentes do conjunto. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:200–203`; `app/chamados/models.py:328`, `:344–350`, `:2662`; `app/chamados/juizes.py:1147`, `:1099–1103`.

   Os testes comportamentais **mordem os três caminhos representados por construção**: esvaziam a fila, comprovam retenção inicial, executam atos reais, exigem evento e verificam estado/segunda rodada. Não exercitam falha do INSERT da pergunta em autocommit; a base é `TestCase`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:50`, `:89–125`, `:134–185`. RED/GREEN executado: [nao verificado].

9. **Hipótese de maquiagem nas contagens — não demonstrada; os ajustes descrevem comportamento compatível com o código.**

   Em `test_o_outbox_celula`, a igualdade continua exata e a repetição continua exigindo zero eventos adicionais. Somar os saves de perguntas relevantes corresponde ao novo produtor. Porém, o teste não exige `len(perguntas) > 0`, portanto isoladamente não garante que esse terceiro produtor foi exercitado. `app/ponto/tests/test_o_outbox_celula.py:104–129`.

   Em `test_orfas...`, um carimbo pode gerar evento novo e provocar outra passagem do colaborador; o consumidor exclui os **IDs já lidos**, não o colaborador inteiro. A asserção adicional mantém todos os eventos pendentes com uma tentativa, distinguindo nova passagem de releitura do mesmo evento. `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:144–152`; `app/chamados/reconciliador.py:1332–1339`.

   Esses ajustes não corrigem nem detectam a quebra de atomicidade do achado 3. Resultado executado dos selos e contagem efetiva de duas falhas: [nao verificado].

## 06/11 08:25 · sessao 2cec561183f3 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE — bypass existente, sem dano demonstrado nos chamadores atuais.** `ressuscitar_perguntas` chama `descarimbar_em_lote(..., validada_em=None, respondida_em=None)`; a porta faz `QuerySet.update` de `materializada_em`, `via_resolucao` e demais campos sem emitir evento. São insumos do C6 capazes de mudar morta→viva. (`app/chamados/models.py:1504`, `app/chamados/models.py:2699`, `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1207`)

   Contudo, o chamado da própria porta já está vivo nos três chamadores encontrados: `reabrir` grava o estado antes de ressuscitar; cartório pula chamados fora de `VIVOS`; desarquivar chama `reabrir` antes de repetir a ressuscitação. Assim, **não demonstrei necessidade de reabrir esse chamado por evento**. (`app/chamados/models.py:1390`, `app/chamados/models.py:1402`, `app/ponto/services/cartorio.py:1591`, `app/ponto/services/cartorio.py:1622`, `app/chamados/views_arquivar.py:70`, `app/chamados/views_arquivar.py:86`)

   O cartório pede novo julgamento depois de ressuscitar; o universo avalia previamente a pergunta com os campos limpos e a célula do julgamento em curso. (`app/ponto/services/cartorio.py:1641`, `app/ponto/services/cartorio.py:1647`, `app/chamados/models.py:1448`, `app/chamados/models.py:1458`)

2. **Escritores por campo do conjunto:**

   | Campo | Escritores |
   |---|---|
   | `resposta_colab` | Resposta normal, automática e reabertura. (`app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1301`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1416`, `app/chamados/services/disputa_emissao.py:1433`) |
   | `respondida_em` | Resposta normal/automática/pela trilha; `carimbar(resposta=...)`; reabertura; ressuscitação. (`app/chamados/services/disputa_emissao.py:1283`, `app/chamados/services/disputa_emissao.py:1434`, `app/chamados/services/disputa_emissao.py:1450`, `app/chamados/models.py:2589`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1504`) |
   | `validada_em` | Validação, devolução da validação, correção da resposta, `carimbar(resposta='validar')`, reabertura e ressuscitação. (`app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/models.py:2592`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1504`) |
   | `materializada_em` | Atribuição central `_escrever_carimbo`; persistência por `carimbar`, `carimbar_identidade`, `descarimbar`; atualização coletiva por `descarimbar_em_lote`. (`app/chamados/models.py:2558`, `app/chamados/models.py:2609`, `app/chamados/models.py:2644`, `app/chamados/models.py:2652`, `app/chamados/models.py:2699`) |
   | `via_resolucao` | Mesmas portas de carimbo; adicionalmente `encerrar_por_fato_no_espelho` e `relavrar`. (`app/chamados/models.py:2560`, `app/chamados/models.py:2709`, `app/chamados/models.py:2710`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/relavra_via.py:125`) |
   | `data_evento` | Nascimento e preenchimento de data ausente em `reapontar_resolvedora`. (`app/chamados/models.py:2476`, `app/chamados/services/fio.py:832`, `app/chamados/services/fio.py:834`, `app/chamados/services/fio.py:847`) |
   | `celula`/`celula_id` | Nascimento; reapontamento da pergunta existente em `abrir`. A FK declara `SET_NULL` na exclusão da célula, mas não localizei caminho produtivo que exclua `CelulaDia`: alcance desse escritor implícito [nao verificado]. (`app/chamados/models.py:2477`, `app/chamados/models.py:2435`, `app/chamados/models.py:2474`, `app/chamados/models.py:2209`) |
   | `chamado_destino`/`_id` | Nascimento, reapontamento em `abrir` e `reapontar_resolvedora`. A FK declara `SET_NULL` na exclusão do chamado; `apagar_em_ensaio` exclui todos os chamados, explicitamente limitado a comandos de ensaio. (`app/chamados/models.py:2477`, `app/chamados/models.py:2450`, `app/chamados/models.py:2474`, `app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:847`, `app/chamados/models.py:2821`, `app/chamados/models.py:531`, `app/chamados/models.py:539`) |
   | `disputa`/`disputa_id` | Nascimento em `abrir`; escritor posterior não demonstrado. (`app/chamados/models.py:2476`; [nao verificado]) |

3. **PROCEDENTE — cobertura declarada do selo é estreita.** Além das leituras diretas, C6 chama helpers que leem `tipo_resposta`, `motivo` e `chamado_destino`; os dois primeiros não estão em `CAMPOS_QUE_ACORDAM`. Não encontrei escritor produtivo posterior ao nascimento que altere `motivo`/`tipo_resposta`; defeito ativo por esses campos permanece hipótese. (`app/chamados/juizes.py:1162`, `app/chamados/juizes.py:1214`, `app/chamados/juizes.py:1099`, `app/chamados/juizes.py:1103`, `app/chamados/juizes.py:886`, `app/chamados/juizes.py:743`, `app/chamados/models.py:2660`; [nao verificado] quanto ao escritor ausente)

4. **Rajada por `save()` sem `update_fields`: não demonstrada.** Todos os sítios atribuídos a PerguntaDisputa pelo censo de produção usam `update_fields`; o lote de lavra usa somente `veredito_lavrado`, `julgado_em`, `janela_expira_em`, fora do conjunto que acorda. Não há evidência de cron genérico salvando perguntas integralmente. (`app/chamados/models.py:2474`, `app/chamados/models.py:2609`, `app/chamados/models.py:2644`, `app/chamados/models.py:2652`, `app/chamados/models.py:2710`, `app/chamados/services/fio.py:847`, `app/chamados/services/disputa_emissao.py:1301`, `app/chamados/services/disputa_emissao.py:1435`, `app/chamados/services/disputa_emissao.py:1451`, `app/chamados/services/validacao.py:133`, `app/chamados/services/validacao.py:190`, `app/chamados/services/relavra_via.py:125`, `app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/veredito_lavrado.py:231`; ausência universal [nao verificado])

5. **Reabertura e captura anterior: sem mudança de identidade na porta atual.** O dicionário atualizado limpa resposta e validação; a opção `desfazer_absorcao` acrescenta apenas limpeza do carimbo. Não altera `celula`, `data_evento` nem `disputa`/colaborador; logo capturar as linhas antes corresponde à mesma identidade depois, sem considerar concorrência externa. (`app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1405`, `app/chamados/services/disputa_emissao.py:1413`, `app/chamados/services/disputa_emissao.py:1417`, `app/chamados/models.py:2699`)

## 06/11 08:25 · sessao 87ea1e057353 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE — média — selo incompleto para o gate.** `CamposQueAcordamCobremOC6Test` analisa apenas `_pergunta_viva_calc` e apenas atributos diretos `p.campo`; não percorre `_ancoradas`, `_vivas_entre`, helpers nem `getattr`. Portanto remover `chamado_destino` e `chamado_destino_id` de `CAMPOS_QUE_ACORDAM` passaria esse selo, embora o gate leia ambos; hoje estão incluídos corretamente. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:200-209`; `app/chamados/models.py:327-350`; `app/chamados/models.py:2662`. O próprio `getattr(p, 'celula', None)` fica fora da varredura. `app/chamados/juizes.py:1147`.

2. **PROCEDENTE — testes novos mordem os três caminhos representados, por inspeção estática.** A fixture esvazia a fila, exige C6 viva e gate retendo; o teste exige evento depois do ato, saída/permanência correta e segunda rodada sem eventos nem mudança de estado. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:89-105`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:111-125`. Os atos usam as funções reais de resposta, recusa e carimbo administrativo. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:134-137`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:153-156`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:172-175`. RED/GREEN executado: **[nao verificado]**.

3. **PROCEDENTE — baixa — falta teste da atomicidade da nova emissão.** Os testes novos derivam de `TestCase` e exercitam o caminho feliz; não há neles falha injetada em `emitir_evento_celula` para testar rollback da pergunta. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:50`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:134-185`. O vizinho possui essa injeção para a célula, que é outra porta. `app/ponto/tests/test_o_outbox_celula.py:141-145`.

4. **Ajuste `test_o_outbox_celula`: compatível com a nova produção de eventos.** O teste continua comparando contagem exata e exigindo ausência de eventos na repetição; adiciona ao esperado os saves de perguntas existentes que acordam o consumidor. `app/ponto/tests/test_o_outbox_celula.py:104-129`. A comparação não assegura que houve qualquer pergunta salva, pois exige `len(salvos) >= 1`, mas não exige `len(perguntas) >= 1`; essa cobertura fica nos testes novos. `app/ponto/tests/test_o_outbox_celula.py:117-123`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:114`.

5. **Ajuste `test_orfas...`: não concluir que “2 falhas” está errado apenas porque o carimbo converge.** O carimbo tira a muda do universo de órfãs, que exige `materializada_em IS NULL`; a segunda volta não a carimba novamente. `app/chamados/reconciliador.py:1037-1040`; `app/chamados/reconciliador.py:1135-1146`. Entretanto há outro caminho para o mesmo `fechar` mockado: a retratação da premissa chama `_retratar`, cujo resolver varre disputas abertas do colaborador e chama `d.fechar` quando `pode_fechar` permite. `app/chamados/reconciliador.py:1357-1364`; `app/chamados/reconciliador.py:522-536`; `app/chamados/services/disputa_emissao.py:1648-1650`; `app/chamados/services/disputa_emissao.py:1694-1701`. O teste preserva eventos falhos pendentes e uma tentativa por evento. `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:147-152`. Resultado executado: **[nao verificado]**.

6. **Classes no código acrescentado:** segundo juiz **ausente** — o signal transporta evento e o consumidor delega aos caminhos existentes; C6 continua registrado como `pergunta_viva`. `app/chamados/signals.py:215-221`; `app/chamados/reconciliador.py:1353-1372`; `app/core/juizes.py:266`. Segundo escritor de `EventoCelula` fora da porta **ausente** — o modelo chama a porta, que faz o INSERT. `app/chamados/models.py:2688`; `app/ponto/portas/celula.py:675`. Novo relógio fora da borda **ausente nos trechos novos** — os helpers e o receiver não consultam relógio. `app/chamados/models.py:2664-2689`; `app/chamados/signals.py:215-221`. Derivação paralela de julgamento **ausente nesses trechos**; há repetição da seleção logística FK/fallback já usada pela semeadura. `app/chamados/models.py:2681-2684`; `app/chamados/reconciliador.py:1295-1296`. Pré-requisitos de API existem na cópia; sequência efetiva do pouso **[nao verificado]**. `app/ponto/portas/celula.py:666`; `app/chamados/reconciliador.py:1304`.

## 06/11 08:24 · sessao 11aa941cbe04 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE — média/alta condicional: a pergunta e o outbox não são atômicos em todos os escritores.** `salvar_e_emitir` abre `atomic` para célula+evento (`app/ponto/portas/celula.py:683`); o receiver da pergunta não abre transação (`app/chamados/signals.py:215-221`), tampouco `carimbar` em torno de seu save (`app/chamados/models.py:2605-2613`). Há chamada de produção sem atomic local: `retratar_perguntas_absolvidas_por_celula` chama `p.carimbar` e engole exceção (`app/chamados/reconciliador.py:582-592`). Em autocommit, hipótese de falha do INSERT deixa pergunta alterada sem evento; o próprio consumidor chama essa função sem receber `falhas` (`app/chamados/reconciliador.py:1354`) e pode marcar o evento antigo processado (`app/chamados/reconciliador.py:1386`). Falha efetiva em produção: **[nao verificado]**.

2. **Ausente no caminho examinado: evento sobrevivente ao rollback apenas porque emissão é síncrona.** Reabertura engloba captura/update/emissão no mesmo atomic (`app/chamados/services/disputa_emissao.py:1401-1417`). Há rollback explícito depois de carimbar validação no aceite de folga (`app/chamados/services/validacao.py:289-313`), mas o receiver insere no atomic existente (`app/chamados/signals.py:215-221`), logo a inferência no mesmo banco é rollback conjunto; divergência de aliases/conexões: **[nao verificado]**. Consumidor não roda inline no receiver: ele busca a fila por SELECT (`app/chamados/reconciliador.py:1332`). Em autocommit a pergunta já foi salva antes do INSERT, por exemplo resposta em `app/chamados/services/disputa_emissao.py:1301`; não foi demonstrada leitura externa de dado não commitado **[nao verificado]**.

3. **Realimentação presente, com convergência para dados estáveis nos caminhos examinados.** Consumidor carimba perguntas (`app/chamados/reconciliador.py:1354` e `:1364`), e carimbo salva campos que acordam (`app/chamados/models.py:2585-2609`); nova volta é possível pelo laço de busca de novos IDs (`app/chamados/reconciliador.py:1331-1335`). Após carimbo, o candidato absolvido deixa o universo `materializada_em__isnull=True` (`app/chamados/reconciliador.py:617-620`), e a órfã deixa universo equivalente (`app/chamados/reconciliador.py:1037-1040`); `encerrar_fantasma` ainda tem no-op explícito (`app/chamados/models.py:2718-2721`). `_gravar_estado` relavra as perguntas (`app/chamados/models.py:291-295`), mas a lavra salva somente `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`app/chamados/veredito_lavrado.py:227-231`), disjuntos do conjunto (`app/chamados/models.py:2660-2662`), portanto receiver os ignora (`app/chamados/signals.py:219-220`). Garantia global com concorrência ou mudanças externas: **[nao verificado]**.

4. **Mapa EventoCelula.** Inserção de `celula_id`, `colaborador_id`, `data` é da porta única (`app/ponto/portas/celula.py:675`); `criado_em` é `auto_now_add`, `tentativas=0`, `erro=''`, `processado_em=NULL` pelos campos (`app/escala/models.py:1857-1863`). Consumidor lê PK e `criado_em` para ordenação/exclusão (`app/chamados/reconciliador.py:1332-1335`), `colaborador_id` para agrupar e carregar colaborador (`:1339-1345`); lê `processado_em`/`tentativas` na seleção (`:1238`), grava `tentativas`/`erro` (`:1380`), grava `processado_em` (`:1386`) e remove por retenção (`:1393`). `criado_em` alimenta também idade (`:1250-1251`), `celula_id` alimenta vigia e dedup da semeadura (`:1262`, `:1284`, `:1297`). O consumidor varre colaborador inteiro e não usa `data` nem `celula_id` para delimitar a reconciliação (`:1339-1372`); leitor de produção de `erro` ou `data` do evento não localizado na busca de referências **[nao verificado]**.

5. **Ordem de trava: nova emissão não demonstrou inversão.** Consumidor não segura atomic abrangendo a reconciliação, apenas marca os eventos num atomic posterior (`app/chamados/reconciliador.py:1344-1386`); exclusividade do comando usa advisory try-lock (`app/chamados/management/commands/consumir_eventos_celula.py:19-23`, `app/chamados/reconciliador.py:1223-1231`). `reconciliar` trava chamado antes de `_gravar_estado` (`app/chamados/models.py:616-620`, `:673-675`); relavra pergunta só depois do save do chamado (`:279-295`). Nova porta busca célula com SELECT simples e insere evento (`app/chamados/models.py:2681-2688`); não há `select_for_update` nela. Ausência de qualquer deadlock em produção: **[nao verificado]**.

6. **Sobre teste de órfãs: ajuste admite nova volta, não leitura repetida do mesmo evento.** Teste espera duas falhas, mantém evento pendente e verifica todas as tentativas iguais a 1 (`app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:144-152`); exclusão por IDs lidos garante isso no código (`app/chamados/reconciliador.py:1332-1335`). A contagem não é prova suficiente de convergência universal; o ponto que limita as emissões de carimbo é o filtro de materialização acima (`app/chamados/reconciliador.py:619`, `:1038`). Teste não executado por restrição de somente leitura **[nao verificado]**.

## 06/11 08:06 · sessao f4de5888dd8a · LASTRO-PARTE-3 · censo · origem=Code

**1. Censo dos escritores**

Os caminhos abaixo distinguem **a instrução que persiste** da função que a chama. `_escrever_carimbo` e `_desfazer_em_memoria` atribuem valores em memória; a persistência acontece nas portas que executam `save`/`update`. (`app/chamados/models.py:2556`, `app/chamados/models.py:2609`, `app/chamados/models.py:2616`)

Na coluna de transação, “sem atomic próprio” significa que a função não abre um `atomic` explícito; ela pode herdar a transação do chamador. Os exemplos de herança estão discriminados depois das tabelas. Os ganchos da coluna final são adicionais ao receiver comum descrito na resposta 2. (`app/chamados/models.py:2562`, `app/chamados/services/disputa_emissao.py:1247`, `app/chamados/signals.py:191`)

| Função | Forma e campos persistidos | Atomic explícito | EventoCelula / `reconciliar()` depois |
|---|---|---|---|
| `PerguntaDisputa.abrir`, nascimento | `cls.objects.create`: fornece `disputa`, `data_evento`, `celula` e demais argumentos; os outros campos pedidos nascem pelos defaults ou por `**campos`. (`app/chamados/models.py:2476`, `:2227`, `:2322`, `:2791`) | Sem próprio. (`app/chamados/models.py:2398`) | O corpo termina com auditoria e retorno. O chamador `gerar_perguntas_disputa` pode, **depois**, chamar `reatar_pergunta_orfa`, condicionalmente. (`app/chamados/models.py:2478`, `:2489`; `app/chamados/services/disputa_emissao.py:404`, `:408`) |
| `PerguntaDisputa.abrir`, reaproveitamento | `ex.save(update_fields=upd)`: `celula` quando encontrada e diferente; `chamado_destino` quando reapontado e permitido; acrescenta `veredito_lavrado`, `julgado_em`, `janela_expira_em`. (`app/chamados/models.py:2433`, `:2450`, `:2471`, `:2474`) | Sem próprio. (`app/chamados/models.py:2398`) | Retorna imediatamente após esse bloco; sem emissão/reconciliação explícita no ramo. (`app/chamados/models.py:2474`, `:2475`) |
| `PerguntaDisputa.carimbar` | `self.save(update_fields=campos)`: base `materializada_em`, `materializada_por`, `via_resolucao`; acrescenta `respondida_em` se solicitado, houver texto e faltar carimbo; `validada_em`, `validada_por` com `resposta='validar'` e validação ausente; os três campos de lavra se `lavrar_veredito=True`. (`app/chamados/models.py:2585`, `:2587`, `:2591`, `:2606`, `:2609`) | Sem próprio; contextos abaixo. (`app/chamados/models.py:2562`) | Depende do chamador: há fechamento de disputa e reconciliação de destinos depois, discriminados abaixo. (`app/chamados/reconciliador.py:586`, `:602`; `app/chamados/services/materializacao.py:549`, `:564`) |
| `PerguntaDisputa.carimbar_identidade` | `self.save`: `['via_resolucao','materializada_em','materializada_por']`, mais `recusa_motivo` quando fornecido. (`app/chamados/models.py:2640`, `:2644`) | Sem próprio; contextos abaixo. (`app/chamados/models.py:2625`) | Depende do chamador; `fechar_por_admin` integra caminhos que reconciliam chamados depois. (`app/chamados/models.py:2522`; `app/ponto/services/veredito_celula.py:177`, `:183`) |
| `PerguntaDisputa.descarimbar` | Com `salvar=True`: `self.save(update_fields=['via_resolucao','materializada_em','materializada_por'])`; zera o trio. Com `False`, somente memória. (`app/chamados/models.py:2646`, `:2652`) | Sem próprio; herdado nos caminhos abaixo. (`app/chamados/models.py:2646`) | Sem emissão/reconciliação própria; devolve a lista de campos. (`app/chamados/models.py:2650`, `:2653`) |
| `PerguntaDisputa.descarimbar_em_lote` | `PerguntaDisputa.objects.filter(...).update(materializada_em=None, materializada_por=None, via_resolucao='', **outros)`. (`app/chamados/models.py:2663`) | Sem próprio; herda, por exemplo, o atomic de `reabrir_para_correcao`. (`app/chamados/services/disputa_emissao.py:1401`, `:1412`) | Retorna o resultado do `update`; sem gancho próprio. (`app/chamados/models.py:2663`) |
| `PerguntaDisputa.encerrar_por_fato_no_espelho` | `self.save(update_fields=['via_resolucao'])`, escrevendo `VIA_FATO_MATERIALIZADO`; já nessa via é no-op. (`app/chamados/models.py:2671`, `:2674`) | O comando chamador abre atomic por pergunta. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:67`, `:68`) | A porta retorna após salvar; sem emissão/reconciliação própria. (`app/chamados/models.py:2674`, `:2675`) |
| `responder_pergunta_disputa_core` | `p.save(update_fields=_campos)`: sempre `resposta_colab`, `respondida_em`; acrescenta `validada_em`, `validada_por` para desfazer validação anterior; acrescenta o trio do descarimbo quando `reabrir_por_resposta()` o devolve. (`app/chamados/services/disputa_emissao.py:1282`, `:1295`, `:1300`, `:1301`; `app/chamados/models.py:2711`) | Sem próprio; há chamadores com e sem atomic local. (`app/chamados/services/disputa_emissao.py:1247`; `app/chamados/services/declaracao_texto.py:302`, `:368`; `app/chamados/views.py:1283`) | Chama `_ressuscitar_por_resposta`; para escapes de ausência chama materialização, que pode reconciliar destinos. Resposta HORA simples não tem chamada explícita ao emissor/reconciliação nesse corpo. (`app/chamados/services/disputa_emissao.py:1326`, `:1330`, `:1359`; `app/chamados/services/materializacao.py:563`, `:564`) |
| `reabrir_para_correcao` | `qs.update(**campos)`: `respondida_em=None`, `validada_em=None`, `validada_por=None`, `resposta_colab=''`. Com `desfazer_absorcao=True`, delega esses campos a `descarimbar_em_lote`, limpando também o trio de materialização. (`app/chamados/services/disputa_emissao.py:1393`, `:1412`, `:1413`) | **Próprio**, envolvendo histórico e alteração. (`app/chamados/services/disputa_emissao.py:1401`, `:1403`) | Não emite nem chama `reconciliar()` no corpo; termina nos retornos das escritas. (`app/chamados/services/disputa_emissao.py:1412`, `:1413`) |
| `responder_automaticamente` | `p.save(update_fields=['resposta_colab','respondida_em'])`. (`app/chamados/services/disputa_emissao.py:1428`, `:1430`) | Sem próprio. (`app/chamados/services/disputa_emissao.py:1417`) | Retorna após salvar; o chamador da foto chama `validar_pergunta` depois. (`app/chamados/services/disputa_emissao.py:1431`; `app/api/foto_recebida.py:48`, `:51`) |
| `carimbar_resposta_pela_trilha` | `p.save(update_fields=['respondida_em'])`. (`app/chamados/services/disputa_emissao.py:1445`, `:1446`) | Sem próprio. (`app/chamados/services/disputa_emissao.py:1434`) | Depois registra evento de **observabilidade**, não `EventoCelula`, e retorna. (`app/chamados/services/disputa_emissao.py:1447`, `:1450`) |
| `reapontar_resolvedora` | `p.save`: `['chamado_destino']`, mais `data_evento` **somente se estava ausente**, mais `['veredito_lavrado','julgado_em','janela_expira_em']`. (`app/chamados/services/fio.py:831`, `:832`, `:834`, `:847`) | **Próprio**, `atomic(savepoint=False)`. (`app/chamados/services/fio.py:837`) | Sem emissão/reconciliação interna. Há chamador que reconcilia depois: `limbo_folgas.agrupar_furo`. (`app/chamados/services/fio.py:849`; `app/chamados/services/limbo_folgas.py:268`, `:276`, `:277`) |
| `relavrar` | `p.save(update_fields=['via_resolucao','veredito_lavrado','julgado_em','janela_expira_em'])`. (`app/chamados/services/relavra_via.py:123`, `:125`) | Sem próprio; o comando `relavrar_via_i6` envolve a chamada em atomic. (`app/chamados/services/relavra_via.py:104`; `app/chamados/management/commands/relavrar_via_i6.py:101`, `:102`) | Depois registra observabilidade e retorna; sem emissão/reconciliação explícita. (`app/chamados/services/relavra_via.py:130`, `:137`) |
| `_carimbar_validacao` | `perg.save(update_fields=['validada_em','validada_por'])`. (`app/chamados/services/validacao.py:131`, `:133`) | Sem próprio, mas os três sítios de chamada estão nos atomics de `_aceitar_a_folga` ou `validar_pergunta`. (`app/chamados/services/validacao.py:262`, `:263`, `:289`, `:290`, `:395`, `:396`) | O aceite de folga fecha o fio ou declara o dia; a validação normal chama materialização, cujo fechamento de destinos usa `reconciliar()`. (`app/chamados/services/validacao.py:270`, `:291`, `:430`; `app/chamados/services/materializacao.py:741`) |
| `devolver_validacao_recusada` | `perg.save(update_fields=['validada_em','validada_por'])`, zerando ambos. (`app/chamados/services/validacao.py:188`, `:190`) | Sem próprio; seu chamador `validar_pergunta` a executa dentro do atomic. (`app/chamados/services/validacao.py:395`, `:425`) | Pode reabrir a disputa por `reabrir_sistema`; não chama diretamente emissor/reconciliação. (`app/chamados/services/validacao.py:199`, `:200`, `:205`) |

Os contextos das portas do modelo completam a distinção entre escritor e chamador:

- **Nascimento/reaproveitamento:** `gerar_perguntas_disputa` chama `abrir`; os caminhos `criar_disputa_a_partir_de_chamados_orfaos` e `criar_disputa_ancora_de_chamado` possuem atomic exterior. (`app/chamados/services/disputa_emissao.py:389`, `:898`, `:1148`, `:1195`)
- **`carimbar`:** recebe chamadas de `lavrar_sem_turno`, absolvição pela célula, encerramento de fantasma, resolução por chamado encerrado, absorção por ausência e materialização. Há atomic próprio do chamador em `_retratar` e no encerramento de órfãs; a absolvição pela célula não recebe um atomic exterior do consumidor. (`app/chamados/reconciliador.py:405`, `:522`, `:586`, `:1135`, `:1136`, `:1314`; `app/chamados/services/disputa_emissao.py:1603`, `:1675`; `app/chamados/services/materializacao.py:549`, `:1202`, `:1349`)
- **`carimbar_identidade`:** os quatro chamadores são `fechar_por_admin`, `retratar_duplicata`, `_materializar_resposta_hora` e `_carimbar_recusa`. O fechamento administrativo da disputa envolve o primeiro em atomic; o fechamento do fio do dia salva perguntas e depois reconcilia chamados. (`app/chamados/models.py:2522`, `:1846`, `:1855`; `app/chamados/services/fio.py:891`; `app/chamados/services/materializacao.py:894`, `:1069`; `app/ponto/services/veredito_celula.py:177`, `:183`)
- **`descarimbar`:** é usado por `reabrir_fantasma`, `reabrir_por_competencia`, `reabrir_por_resposta(salvar=False)` e `desfazer_carimbo_sem_lastro`. Os três caminhos que persistem têm atomic nos respectivos chamadores mostrados. (`app/chamados/models.py:2696`, `:2713`, `:2720`; `app/chamados/services/relavra_via.py:217`, `:231`; `app/chamados/services/competencia_trancada.py:253`, `:254`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`, `:147`)
- **`descarimbar_em_lote`:** além da correção de questionário, recebe `validada_em=None`, `validada_por=None`, `respondida_em=None` de `ressuscitar_perguntas`; essa função não abre atomic próprio. (`app/chamados/models.py:1461`, `:1504`; `app/chamados/services/disputa_emissao.py:1401`, `:1412`)

Não apareceu no censo outro `save` dos oito campos sem `update_fields`, nem `bulk_create`/`bulk_update` deles. O `bulk_update` de `PerguntaDisputa` existente grava exclusivamente os três campos de lavra. A inexistência universal de outra forma dinâmica não reconhecida: **[nao verificado]**. (`app/chamados/veredito_lavrado.py:194`, `:207`)

**2. Receivers de PerguntaDisputa**

O receiver explícito encontrado é **`_becos_post_save_pergunta`**, de `post_save`. Ele ignora `raw` e pergunta sem `disputa_id`, captura o ID da disputa e agenda `fechar_beco_da_disputa` com `transaction.on_commit`, sob `engolir`. Não filtra por `created` nem pelos campos alterados. (`app/chamados/signals.py:191`)

A porta do beco relê a disputa, verifica se é fechável, protege reentrada por `_EM_CURSO` e, em atomic, resolve perguntas dos chamados encerrados e fecha a disputa quando permitido. O fechamento da disputa pode chamar `reconciliar()` do chamado-pai pelo outro receiver. Portanto um `save` de pergunta **já pode chegar indiretamente à reconciliação**, mas isso é condicional ao beco e às guardas de fechamento. (`app/chamados/services/becos.py:25`, `:41`, `:49`, `:52`, `:65`; `app/chamados/signals.py:18`, `:41`, `:42`)

Não localizei `pre_save` de `PerguntaDisputa` nem outra conexão explícita na cópia; registro dinâmico em execução: **[nao verificado]**. O carregamento dos sinais do app acontece em `ready()`. (`app/chamados/apps.py:8`)

**3. Selos e impacto de acrescentar chamadas ao emissor**

| Selo | Enumeração/exigência |
|---|---|
| `test_porta_pergunta_identidade` | Declara sete atos e exige sete escritas censadas no módulo do modelo; também exige a tupla exata de portas. **A contagem inclui a exclusão de perguntas por `remover_motivo`.** (`app/chamados/tests/test_porta_pergunta_identidade.py:60`, `:77`, `:102`, `:111`) |
| `test_porta_resposta_pergunta` | Exige quatro sítios: dois em `responder_pergunta_disputa_core`, um em `carimbar_resposta_pela_trilha`, um em `responder_automaticamente`; vincula contagem à função por AST. (`app/chamados/tests/test_porta_resposta_pergunta.py:66`, `:97`, `:107`) |
| `test_porta_desfecho_pergunta` | Exige um sítio em `_materializar_resposta_hora`, um em `_carimbar_recusa` e **zero escrita direta de pergunta em signals**. (`app/chamados/tests/test_porta_desfecho_pergunta.py:72`, `:102`, `:138`) |
| `test_porta_reapontamento_pergunta` | Exige uma escrita em `fio.reapontar_resolvedora`, zero no reconciliador; proíbe atribuições de `data_evento`/`chamado_destino` no reconciliador e escrita direta no comando de dedup. (`app/chamados/tests/test_porta_reapontamento_pergunta.py:77`, `:113`, `:147`, `:163`) |
| `test_porta_veredito_pergunta` | Exige uma escrita em cada função: `lavrar`, `gravar_lavras_em_lote`, `relavrar`, com conferência por função. (`app/chamados/tests/test_porta_veredito_pergunta.py:94`, `:123`) |
| `test_porta_validacao_pergunta` | Enumera `_carimbar_validacao`, `recusar_pergunta`, `conferir_data_do_fato`, `devolver_validacao_recusada`; exige uma escrita em cada e zero nas cascas views/ações. (`app/chamados/tests/test_porta_validacao_pergunta.py:72`, `:95`, `:122`) |
| `test_o261e_f2_porta_carimbo` | Allowlist por arquivo/função para atribuição de materialização e via; exige nenhum escritor fora e nenhum declarado fantasma. Enumera também referências a `_escrever_carimbo`, `_desfazer_em_memoria`, `carimbar_identidade`, incluindo as vias permitidas dos quatro chamadores de identidade. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35`, `:44`, `:224`, `:373`, `:398`, `:485`) |
| `test_contract_escritores_estado` | Enumera arquivos escritores de `data_evento`, materialização, validação e resposta; usa regex sobre código sem comentários e rejeita novos escritores e declarações fantasmas. (`app/core/tests/test_contract_escritores_estado.py:61`, `:83`, `:133`, `:177`, `:200`) |
| `test_contract_dono_validada` | Dono é `validacao.py`; allowlist efetiva contém `disputa_emissao.py`, `materializacao.py`, `models.py`. Procura atribuição/`update` de validação e exige zero fora. (`app/chamados/tests/test_contract_dono_validada.py:26`, `:30`, `:47`, `:65`) |
| `test_chokepoint_chamado_gate` | Exige toda escrita censada da família dentro das portas declaradas, sem pendências/exceções. As oito portas de pergunta estão registradas em `core/portas.py`. (`app/chamados/tests/test_chokepoint_chamado_gate.py:316`, `:347`, `:391`; `app/core/portas.py:392`) |
| `test_reabertura_fantasma` | Enumera três origens antigas, proíbe nelas a string de reabertura crua e exige os quatro campos zerados na fonte da porta canônica. (`app/chamados/tests/test_reabertura_fantasma.py:43`, `:53`, `:68`) |
| `test_porta_remocao_motivo` | Exige `remover_motivo` entre os escritores censados de pergunta e zero escrita de disputa/pergunta nas duas origens antigas. (`app/chamados/tests/test_porta_remocao_motivo.py:140`, `:156`) |

Há ainda verificadores sintáticos da forma das escritas: texto acompanhado de instante na mesma função/chamada; materialização acompanhada de autor; retorno de `carimbar` não ignorado. Eles não enumeram chamadores do emissor. (`app/chamados/tests/test_contract_par_resposta.py:65`, `:87`; `app/chamados/tests/test_contract_texto_com_carimbo.py:60`, `:148`; `app/chamados/tests/test_contract_autor_muda.py:29`; `app/chamados/tests/test_o261e_f2_fila.py:40`)

**Não encontrei allowlist/contagem de chamadores de `emitir_evento_celula`.** Os selos próximos verificam outras coisas: `SeloSaveDaPortaTest` inspeciona saves de veredito/ata **dentro do arquivo da porta**; o gate da célula verifica funções que contêm escrita, não seus chamadores. Inexistência universal de outro selo: **[nao verificado]**. (`app/ponto/tests/test_o_outbox_celula.py:632`; `app/ponto/tests/test_chokepoint_celula_gate.py:62`)

Consequência estática para as duas alterações hipotéticas:

- **Receiver somente chamando `emitir_evento_celula`:** os enumeradores acima não o tratam como nova escrita direta de pergunta. O censo não segue chamadas de serviço interproceduralmente. Se o receiver passasse a salvar pergunta diretamente, o selo de signals e o gate da família acusariam. (`app/core/censo_escritas.py:364`; `app/chamados/tests/test_porta_desfecho_pergunta.py:138`; `app/chamados/tests/test_chokepoint_chamado_gate.py:347`)
- **`reabrir_para_correcao` somente chamando o emissor:** também não acrescenta escrita direta de pergunta para esses enumeradores. Se acrescentasse nova escrita reconhecida pelo censo, a contagem/vinculação de funções da porta da resposta acusaria. (`app/core/censo_escritas.py:364`; `app/chamados/tests/test_porta_resposta_pergunta.py:66`, `:97`, `:107`)

Resultado comportamental da suíte após qualquer dessas mudanças: **[nao verificado]**. O teste de reentrega existente exige uma retratação e nenhum followup duplicado, mas aceita eventos adicionais gerados pela própria retratação. (`app/ponto/tests/test_o_outbox_celula.py:149`, `:161`, `:170`)

**4. Forma usada para encontrar a CelulaDia e nulabilidade**

No nascimento de pergunta de fato, a busca é pelo par **`(disputa.colaborador_id, data_evento)`**; o resultado é gravado em `p.celula`. No C6, a decisão sobre veredito usa diretamente **`p.celula`**. A semeadura do outbox já usa a forma “FK quando existe; senão busca pelo par”. (`app/chamados/models.py:2406`, `:2430`, `:2476`; `app/chamados/juizes.py:1147`, `:1180`; `app/chamados/reconciliador.py:1295`)

Essas formas não têm equivalência garantida: `_celulas_da_pergunta` só reaproveita a FK quando colaborador e data conferem; caso contrário devolve `None` para a lei consultar por seus próprios meios. (`app/chamados/juizes.py:621`)

- **`celula=NULL`, `data_evento` preenchida: permitido e alcançável.** Na conversa, `abrir` troca data ausente por `localdate()`, mas mantém `celula=None`, pois pula a busca reservada à pergunta de fato. Na pergunta de fato, `.first()` também pode não encontrar célula. (`app/chamados/models.py:2407`, `:2424`, `:2428`, `:2430`, `:2476`)
- **`data_evento=NULL`: permitido pelo modelo.** A porta `abrir` preenche a ausência; `reapontar_resolvedora` contém tratamento para pergunta já existente sem data. Existência dessas linhas no PROD fornecido: **[nao verificado]**. (`app/chamados/models.py:2218`, `:2424`; `app/chamados/services/fio.py:832`)
- A FK aceita `NULL`, usa `SET_NULL`, e a `Meta` de pergunta não declara constraint conjugando FK e data. (`app/chamados/models.py:2209`, `:2811`)

**5. Contraexemplo de realimentação pelo consumidor**

**Existe caminho `consumidor → save de pergunta → novo evento → consumidor`. Isso demonstra realimentação; laço infinito não fica demonstrado.** O emissor insere um evento novo e o consumidor percorre novos pendentes em `while True`, excluindo somente os PKs já lidos naquela execução. (`app/ponto/portas/celula.py:673`; `app/chamados/reconciliador.py:1329`)

| Caminho alcançado pelo consumidor | Persistência de PerguntaDisputa |
|---|---|
| Absolvição pela célula | Consumidor chama `retratar_perguntas_absolvidas_por_celula`, que chama `p.carimbar`; o save real está no modelo. (`app/chamados/reconciliador.py:1353`, `:586`; `app/chamados/models.py:2609`) |
| Órfã encerrada pela lâmpada | `encerrar_orfas_pela_lampada → encerrar_fantasma → carimbar → save`. (`app/chamados/reconciliador.py:1363`, `:1136`; `app/chamados/models.py:2685`, `:2609`) |
| Retratação por célula ou premissa | `_retratar → resolver_pergunta

[truncado em 20000 de 26767 caracteres]

_2147 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
