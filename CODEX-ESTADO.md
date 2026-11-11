# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 10:01** · publicado em **11/11/2026 10:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 10:01 · sessao a47458d8e632 · - · - · origem=Ronald (tmux)

**Selos/mutantes — conclusão estática:** os dois harnesses recusam erro de infraestrutura e exigem `resultado.failures`; uma exceção de import/sintaxe/banco não conta como mutante morto. O legado também roda controle da fonte intacta. Não há caminho explícito de sucesso com zero falhas nos mutantes. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:278`, `:286`, `:287`, `:295`; `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:137`, `:143`, `:144`.)

Os **oito mutantes do legado** têm mordidas concretas:

| Mutante | Asserção que o mata |
|---|---|
| `fechar_por_idade` | O chamado de 20 dias deve continuar VIVO após cron (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:175`). |
| `relavrar_via_em_nao_trancada` | `aplicadas == 1`, depois status VIVO (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:109`). |
| `reabre_trancada` | O relatório precisa trazer `relavrar_via_tranca, True` (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:132`). **Aqui a mordida é na classificação do relatório; a porta real continua recusando a reabertura** (`app/chamados/models.py:1009`). |
| `reabrir_fora_da_porta` | Exatamente mais uma linha `estado_chamado` (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:113`). O `QuerySet.update` do mutante pula `_gravar_estado` (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:221`). |
| `apply_sem_esperados` | `abortado` verdadeiro para `None`, vazio e conjunto diferente (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:142`, `:145`). |
| `dry_escreve` | Linha deve ficar `aplicado=False`, `aplicadas=0`, foto intacta (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:95`, `:97`, `:98`). |
| `universo_proprio` | Só o chamado com assinatura entra nos PKs/linhas (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:150`, `:154`, `:155`). |
| `forca_desligado` | `aplicado=False`, `aplicadas=0`, foto intacta (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:164`, `:165`, `:166`). |

**Execução:** rodei apenas os dois módulos puros `test_o169_cluster_encerrado_sem_lastro` e `test_o169_cluster_encerrado_mutantes`, com bytecode desabilitado: 62 testes, OK, incluindo todos os 36 mutantes. Nenhum arquivo de resultado foi criado; a execução não tem evidência arquivo:linha própria [nao verificado]. O contrato declara não usar Django/banco/rede/temporários e executa fontes em namespaces isolados (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:1`, `:31`, `:103`). A suite Django do legado não foi executada [nao verificado].

**Limites reais dos selos:**

- O harness aceita **qualquer** `AssertionError` no caso alvo, inclusive de fixture; não exige uma asserção específica. No legado, controle intacto reduz esse risco; no contador, cada caso normal também existe, mas não há controle intacto dentro da classe de mutantes. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:65`, `:72`, `:287`, `:295`; `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:133`, `:148`.)
- O comentário “cada caso abre seu savepoint (`_fixture_setup`)" não corresponde ao mecanismo visível: chama diretamente `.run(resultado)`, dentro de `transaction.atomic()` próprio e rollback `_Desfaz`. O isolamento visível é esse atomic externo; não há chamada explícita `_fixture_setup`. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:245`, `:258`, `:260`, `:261`.)
- As notas só são verificadas por **contagem maior**, sem conteúdo/tipo/exatamente uma; a trilha extra só confere via anterior e status posterior, sem ator/motivo completos. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`, `:116`, `:117`, `:118`.)

**Mutações relevantes do diff sem mordida nos testes perguntados:** [nao verificado]

1. Remover/mover a trava ou remover a releitura do contador: todos os casos são sequenciais, sem escritor concorrente/instrumentação de ordem. (`app/ponto/services/cluster_espurio.py:266`, `:268`, `:270`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:89` a `:205`.)
2. Remover `colaborador_id=o['colab']` do comando legado: o teste de comando prepara apenas um colaborador; a verificação existente do filtro no contrato puro passa pelo caminho normal, sem `legado`. (`app/ponto/management/commands/detectar_cluster_espurio.py:55`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:184`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:164`, `:404`.)
3. Trocar `recusa` por `None` sempre, ou não atualizar a recusa após falha da porta: não há asserção sobre essa chave, inclusive no desligado. (`app/ponto/services/cluster_espurio.py:254`, `:274`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:158`.)
4. Alterar ator/motivo da auditoria ou texto/tipo da nota mantendo contagem: os testes perguntados não mordem esses valores. (`app/ponto/services/cluster_espurio.py:277`, `:281`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`, `:118`.)
5. Trocar aceitação de conjuntos esperados por dependência da ordem: não há APPLY feliz com dois ou mais PKs, em ordem invertida/duplicados. (`app/ponto/services/cluster_espurio.py:259`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:106`, `:131`, `:142`.)
6. Não há cenário de tranca surgindo/desaparecendo entre medição e apply, perda de assinatura no intervalo, ou erro depois de primeiro PK de lote: os cenários perguntados usam dados estáveis e APPLY por um PK. (`app/ponto/services/cluster_espurio.py:249`, `:263`, `:270`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:100`, `:127`.)

**Outro defeito concreto:** APPLY com todos os itens trancados, com porta recusando tudo, ou mesmo `esperados=[]`/universo vazio ainda chama `get_user_sistema()` antes do loop; esse helper faz `get_or_create` e pode gravar User se faltar. Logo “NADA se escreve” não é universal. `_foto` não olha User e não captura isso. (`app/ponto/services/cluster_espurio.py:262`, `:264`; `app/core/utils.py:18`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75`.)

## 11/11 10:00 · sessao 021c9279df7a · - · - · origem=Ronald (tmux)

2. **Sem juiz novo no corpo do diff:** universo vem de `contador_cluster_encerrado_sem_lastro()['pks']`; assinatura continua em `_leitura_da_celula`; dia usa `dia_do_fato`; tranca usa a mesma casca de `renascer`; recusa delega a `recusa_da_reabertura` — `app/ponto/services/cluster_espurio.py:245`, `app/ponto/services/cluster_espurio.py:249`, `app/ponto/services/cluster_espurio.py:251`, `app/ponto/services/cluster_espurio.py:254`, `app/ponto/services/cluster_espurio.py:270`, `app/ponto/services/cluster_espurio.py:126`.

Há uma diferença **preexistente, agora exposta pelo relatório**: `_dia_em_competencia_trancada` usa a competência do chamado e a existência de `PeriodoFechado` vivo; não desconta toque do admin — `app/chamados/models.py:1038`, `app/chamados/models.py:1044`. C20, declarado como autoridade da tranca do dia, desconta explicitamente toque humano posterior — `app/core/juizes.py:156`, `app/core/juizes.py:161`, `app/chamados/juizes.py:1033`, `app/chamados/juizes.py:1041`. Não chamar essa casca de C20 sem distinguir as perguntas.

Outra diferença relevante: o dia mostrado/medido vem de `dia_do_fato(fortes=True)`, mas a competência usada na tranca vem do fato **mais recente**, incluindo ausência e perguntas vivas; a classificação pode, portanto, tratar outra competência que a do dia cuja assinatura foi contada — `app/ponto/services/cluster_espurio.py:114`, `app/ponto/services/cluster_espurio.py:251`, `app/chamados/models.py:1043`, `app/chamados/juizes.py:2275`, `app/chamados/juizes.py:2283`, `app/chamados/juizes.py:2290`.

3. **Nenhuma leitura explícita nova de relógio** no contador ou tratamento; o comando normal lê `timezone.localdate()` na borda, e `--legado` retorna antes dela — `app/ponto/services/cluster_espurio.py:88`, `app/ponto/services/cluster_espurio.py:224`, `app/ponto/management/commands/detectar_cluster_espurio.py:67`, `app/ponto/management/commands/detectar_cluster_espurio.py:70`. Os carimbos automáticos de nota/atualização são leituras de relógio de persistência preexistentes, não uma decisão nova por idade — `app/chamados/models.py:68`, `app/chamados/models.py:1674`. Não confundir `timezone.localdate(b.timestamp)` com leitura do relógio atual: ela converte um instante dado — `app/chamados/juizes.py:53`.

4. **Ordem de trava:** `abrir` faz `atomic → trava do colaborador → primeira leitura → renascer` — `app/chamados/models.py:902`, `app/chamados/models.py:905`, `app/chamados/models.py:906`, `app/chamados/models.py:922`. O novo tratamento faz `atomic → leitura de ch+colaborador → trava → contador → renascer`; a primeira leitura ocorre antes da trava — `app/ponto/services/cluster_espurio.py:266`, `app/ponto/services/cluster_espurio.py:267`, `app/ponto/services/cluster_espurio.py:268`, `app/ponto/services/cluster_espurio.py:270`, `app/ponto/services/cluster_espurio.py:273`. Isso viola a orientação explícita de reler a instância depois da aquisição — `app/chamados/models.py:2638`.

`resposta_tardia` não tem essa sequência: consulta estado e célula e chama diretamente o renascimento; a trava vem posteriormente, quando o escritor grava estado — `app/chamados/services/resposta_tardia.py:35`, `app/chamados/services/resposta_tardia.py:44`, `app/chamados/services/resposta_tardia.py:46`, `app/chamados/models.py:280`, `app/chamados/models.py:290`.

Não identifiquei uma inversão nova comprovada: antes de adquirir C, o novo corpo só faz leitura comum; o escritor adquire C→D→Ch e a porta de ressuscitação C→D→Ch→P — `app/ponto/services/cluster_espurio.py:267`, `app/chamados/models.py:289`, `app/chamados/models.py:1562`, `app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`, `app/chamados/models.py:2692`. Ausência universal de deadlock é [nao verificado].

**TOCTOU concreto:** a releitura do contador não atualiza `ch` nem seu colaborador já carregado. Se outro ato alterar `situacao`, contexto ou via enquanto espera a trava, `renascer` pode usar o objeto anterior; `reabrir` só refresca campos quando detecta mudança de status — `app/ponto/services/cluster_espurio.py:267`, `app/ponto/services/cluster_espurio.py:270`, `app/chamados/models.py:1005`, `app/chamados/models.py:1007`, `app/chamados/models.py:1373`, `app/chamados/models.py:1375`.

`esperados` compara apenas a fotografia inicial; sob a trava só verifica se cada pk ainda pertence ao contador, sem conferir igualdade de conjuntos. Novos candidatos não abortam o lote; candidatos desaparecidos são pulados; a classificação inicial `trancada` também decide quais linhas sequer entram no apply — `app/ponto/services/cluster_espurio.py:259`, `app/ponto/services/cluster_espurio.py:264`, `app/ponto/services/cluster_espurio.py:270`. Não há transação do lote: falha posterior deixa anteriores já commitados — `app/ponto/services/cluster_espurio.py:263`, `app/ponto/services/cluster_espurio.py:266`.

A trava C não serializa necessariamente a mudança de `PeriodoFechado`: `trancar_periodo` grava a tranca antes do gancho que trata os chamados, e `reabrir_periodo` também grava antes do gancho — `app/ponto/services/fechamento.py:1408`, `app/ponto/services/fechamento.py:1419`, `app/ponto/services/fechamento.py:1428`, `app/ponto/services/fechamento.py:1441`, `app/ponto/services/fechamento.py:1448`.

5. **`pks` e `numero`: coerentes na implementação:** `pks` enumera chamados com assinatura e `numero=len(pks)`; o alarme compara `numero` ao esperado e o tratamento usa os pks. `universo.total` é outro denominador, anterior ao filtro da assinatura — `app/ponto/services/cluster_espurio.py:126`, `app/ponto/services/cluster_espurio.py:127`, `app/ponto/services/cluster_espurio.py:149`, `app/ponto/services/cluster_espurio.py:246`, `app/ponto/management/commands/detectar_cluster_espurio.py:100`.

`trancada` significa a mesma chamada que a guarda de `renascer`, em estado estável; pode divergir temporalmente porque a linha foi montada antes do apply — `app/ponto/services/cluster_espurio.py:249`, `app/chamados/models.py:1009`, `app/ponto/services/cluster_espurio.py:263`.

`aplicado/aplicadas` contam somente retorno verdadeiro do renascimento, não efeitos totais ou tentativas — `app/ponto/services/cluster_espurio.py:273`, `app/ponto/services/cluster_espurio.py:274`, `app/ponto/services/cluster_espurio.py:282`. Porta recusada pode mesmo assim escrever trilha de veto ou migrar perguntas/anexar nota e retornar falso — `app/chamados/models.py:1017`, `app/chamados/models.py:1415`, `app/chamados/models.py:1427`, `app/chamados/models.py:1436`.

**`recusa` é incompleta:** relatório consulta só as guardas de `reabrir`; desligado/cascade/tranca pertencem às guardas adicionais de `renascer`; após retorno falso nada atualiza o motivo. Assim desligado pode aparecer `ativo=False`, `recusa=None`, `aplicado=False` — `app/ponto/services/cluster_espurio.py:254`, `app/ponto/services/cluster_espurio.py:255`, `app/ponto/services/cluster_espurio.py:274`, `app/chamados/models.py:1005`, `app/chamados/models.py:1007`, `app/chamados/models.py:1009`.

Outros defeitos concretos:

- `get_user_sistema()` roda antes de pular linhas trancadas e fora dos atomics; usa `get_or_create`. Até lote vazio ou totalmente parado pode criar `auth_user` se o sistema faltar — `app/ponto/services/cluster_espurio.py:262`, `app/ponto/services/cluster_espurio.py:264`, `app/core/utils.py:18`.
- A nova trilha obrigatória usa escritor best-effort, cujo retorno pode ser `None`; o tratamento ignora o retorno e conta aplicação mesmo sem a trilha persistida — `app/core/observ.py:96`, `app/core/observ.py:105`, `app/core/observ.py:139`, `app/ponto/services/cluster_espurio.py:277`, `app/ponto/services/cluster_espurio.py:282`.
- Descrição da auditoria afirma “encerrado por idade pelo cron velho”, mas o próprio contador declara não distinguir nota/trilha/motivo e incluir decisões administrativas por reconciliar; proveniência por idade de cada pk é [nao verificado] — `app/ponto/services/cluster_espurio.py:93`, `app/ponto/services/cluster_espurio.py:94`, `app/ponto/services/cluster_espurio.py:278`.


## 11/11 10:00 · sessao 5604be92552c · - · - · origem=Ronald (tmux)

O diff respeita os escritores declarados dos campos principais:

- `status_local`, `via_encerramento` e `resolvido_em`: tratamento chama `_renascer_por_premissa_viva`; esta chama `reabrir`, que chama `_gravar_estado`. O núcleo altera e salva os três campos; `models.py` é porta declarada. Não há escrita direta nova desses campos no serviço. `app/ponto/services/cluster_espurio.py:273`; `app/chamados/models.py:1019`; `app/chamados/models.py:1442`; `app/chamados/models.py:228`; `app/chamados/models.py:244`; `app/chamados/models.py:248`; `app/chamados/models.py:296`; `app/core/portas.py:218`.
- Nota de renascimento: `_renascer_por_premissa_viva` usa `nota.anexar_ocorrencia`, escritor declarado de `FollowupChamado`; a porta cria a nota. Leitores incluem histórico, detalhe e painel. `app/chamados/models.py:1021`; `app/chamados/services/nota.py:75`; `app/chamados/services/nota.py:103`; `app/core/portas.py:241`; `app/chamados/services/historico.py:80`; `app/chamados/views.py:352`; `app/chamados/services/painel.py:362`.
- `LogAuditoria`: núcleo emite `estado_chamado`; serviço emite `reabrir_legado_cluster_o169`; ambos passam por `core.observ.evento`. O histórico e os vigias leem `estado_chamado`; a ação nova tem leitor específico no teste. `app/chamados/models.py:292`; `app/ponto/services/cluster_espurio.py:277`; `app/core/observ.py:94`; `app/core/observ.py:134`; `app/chamados/services/historico.py:86`; `app/chamados/services/vigia_loop.py:28`; `app/chamados/services/vigia_loop.py:114`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`.
- `CelulaDia.ata`: tratamento apenas lê via contador e `_leitura_da_celula`; produtor é cartório, persistência passa pela porta da célula. `app/ponto/services/cluster_espurio.py:245`; `app/ponto/services/cluster_espurio.py:126`; `app/ponto/services/cluster_espurio.py:27`; `app/ponto/services/cartorio.py:696`; `app/ponto/services/cartorio.py:1811`; `app/ponto/portas/celula.py:974`; `app/ponto/portas/celula.py:1033`; `app/core/portas.py:112`.
- `PeriodoFechado`: tratamento apenas consulta indiretamente; escritores de tranca/reabertura são `trancar_periodo`/`reabrir_periodo` em fechamento, módulo declarado. `_dia_em_competencia_trancada` lê `competencia_trancada`, que consulta `reaberto_em__isnull=True`. `app/ponto/services/fechamento.py:1408`; `app/ponto/services/fechamento.py:1437`; `app/core/portas.py:501`; `app/chamados/models.py:1033`; `app/ponto/services/fechamento.py:1473`.

Leitores relevantes dos valores alterados:

- Contador filtra `ENCERRADOS` e via `RECONCILIAR`; relatório lê status/via antes e depois. `app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:112`; `app/ponto/services/cluster_espurio.py:251`; `app/ponto/services/cluster_espurio.py:272`; `app/ponto/services/cluster_espurio.py:280`.
- Via de tranca determina desarquivamento e devolução à fila quando período reabre. `app/chamados/views_arquivar.py:64`; `app/chamados/services/competencia_trancada.py:259`.
- `resolvido_em` participa da recusa por fechamento administrativo sem fato novo e das estatísticas. `app/chamados/models.py:1342`; `app/chamados/services/painel.py:245`; `app/chamados/services/fila.py:109`.
- Os valores reais são `RECONCILIAR → 'sistema'` e tranca → `'competencia_trancada'`. `app/chamados/catalogo/motor.py:88`; `app/chamados/catalogo/motor.py:92`; `app/chamados/catalogo/motor.py:95`.

Defeitos e nuance do ramo parado:

- A trilha não é garantida atomicamente: `evento` é best-effort, engole falha e retorna `None`; o tratamento ignora o retorno e marca aplicado. Pode reabrir sem a linha obrigatória do legado; o núcleo usa o mesmo emissor para `estado_chamado`. `app/core/observ.py:98`; `app/core/observ.py:105`; `app/core/observ.py:133`; `app/core/observ.py:139`; `app/ponto/services/cluster_espurio.py:277`; `app/ponto/services/cluster_espurio.py:282`; `app/chamados/models.py:292`.
- `get_user_sistema()` pode criar usuário antes do laço, mesmo universo vazio ou composto somente de linhas trancadas. Logo “nada se escreve” no ramo parado não vale para qualquer entidade do banco se faltar esse usuário. `app/ponto/services/cluster_espurio.py:262`; `app/core/utils.py:18`.
- Não encontrei gate que relavre **somente a via para tranca, preservando o status encerrado**: o gate dedicado recusa qualquer estado fora de `VIVOS`. `app/chamados/models.py:1622`; `app/chamados/models.py:1630`.
- A alegação literal “nenhuma porta troca a via de chamado já encerrado sem reabrir” é excessiva: `resolvido → fechado` é permitido; `transicionar → _aplicar → _gravar_estado` troca via para `admin`, preservando `resolvido_em`, sem estado vivo intermediário. Isso não resolve a relavra solicitada para tranca. `app/ponto/services/cluster_espurio.py:220`; `app/chamados/catalogo/motor.py:30`; `app/chamados/catalogo/motor.py:102`; `app/chamados/models.py:128`; `app/chamados/models.py:199`; `app/chamados/models.py:447`; `app/chamados/models.py:242`.

## 11/11 10:00 · sessao e11873e160bf · FIO-E0-V4-E3 · sonda · origem=Code

Não reexecutei a sonda. O envelope reescreve credenciais, cria/remove arquivos temporários e acrescenta log, contrariando sua regra de não escrever arquivos. [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/bin/sonda_leitura.sh:126), [bin/sonda_leitura.sh:216](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/bin/sonda_leitura.sh:216), [bin/sonda_leitura.sh:248](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/bin/sonda_leitura.sh:248) A identificação desta cópia como HEAD do hash informado não pôde ser confirmada: não há repositório Git reconhecido no diretório. [nao verificado]

Transcrição integral da saída fornecida, sem nova execução. [nao verificado]

```text
sonda_leitura: sonda_x1_comp11_v2.py [c1ce461ce6ce3acb] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

PERG pk 54831 col237 2026-11-07 liga=celula data_ev=2026-11-07 mat=sim valid=nao via=celula_absolveu cel_ver=fato_em_ausencia disp_fechada=sim pai=nenhum viva=True motivo=batida_fora_escala_grave
celula.acusa n 76
celula.nao_acusa n 1
pares n 77
pergunta.fora_universo n 1
pergunta.total n 1
pergunta.viva n 1
```

Na saída fornecida não consta traceback, `sonda_nao_verificada` nem linha `CHAMADO_VIVO_X1`. Não há contadores adicionais transcritos; não os completei com zeros. [nao verificado]

Não existe linha PERG com `mat=nao valid=nao` no dado fornecido; portanto, não há caso individual dessa classe para mapear. [nao verificado] O caminho condicional de absolvição seria `qs_perguntas_absolviveis` → `perguntas_absolvidas_pela_celula` → `retratar_perguntas_absolvidas_por_celula`: exige materialização e validação nulas, célula com veredito preenchido que não acusa, pergunta morta pelo juiz e dia não trancado; então chama `carimbar(VIA_CELULA_ABSOLVEU, ...)`. Deixar de acusar sozinho não basta. [app/chamados/reconciliador.py:627](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:627), [app/chamados/reconciliador.py:661](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:661), [app/chamados/reconciliador.py:595](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:595)

Para a **PERG 54831**, com via preenchida e `viva=True` no dado: [nao verificado]

- **O carimbo não garante morte.** `fato_em_ausencia` pertence a `VEREDITOS_ACUSAM`; quando alcança esse ramo, o juiz retorna `True`, mesmo havendo materialização. `celula_absolveu` não integra as vias de identidade que encerram independentemente do fato. [app/escala/models.py:1767](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/escala/models.py:1767), [app/chamados/juizes.py:1372](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/juizes.py:1372), [app/chamados/models.py:2510](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/models.py:2510)

- **O reconciliador não a recarimba nesse estado**, nem se a célula passar a não acusar: sua seleção exige `materializada_em IS NULL`. O reatador de órfãs também exige materialização nula e disputa aberta. [app/chamados/reconciliador.py:629](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:629), [app/chamados/reconciliador.py:1500](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:1500)

- **A cascata S143/S128 também não a recarimba:** o receiver chama o resolvedor, cujo primeiro conjunto exige materialização nula e disputa aberta; o ramo sem credor pula disputas fechadas e exige materialização e validação nulas. [app/chamados/signals.py:115](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/signals.py:115), [app/chamados/services/disputa_emissao.py:1823](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/disputa_emissao.py:1823), [app/chamados/services/disputa_emissao.py:1907](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/disputa_emissao.py:1907), [app/chamados/services/disputa_emissao.py:1916](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/disputa_emissao.py:1916)

- **S144 também a exclui:** delega a `materializar_pendentes_aprovadas`, que seleciona somente materialização nula. [app/chamados/signals.py:162](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/signals.py:162), [app/chamados/services/materializacao.py:1418](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/materializacao.py:1418)

- **Existe outro escritor capaz de desfazer o carimbo**, se receber essa pergunta e confirmar sua elegibilidade/vitalidade: `desfazer_carimbo_sem_lastro`, cujo padrão permite reabrir disputa fechada, limpa os três campos do carimbo e reabre a disputa. Porém, o julgamento automático chama essa porta com `reabrir_fechada=False`, excluindo disputas fechadas. Isso impede concluir que “nenhum escritor existe”, mas esses caminhos automáticos não corrigem essa linha no estado informado. [app/chamados/services/relavra_via.py:187](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/relavra_via.py:187), [app/chamados/services/relavra_via.py:238](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/relavra_via.py:238), [app/chamados/services/relavra_via.py:246](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/relavra_via.py:246), [app/chamados/services/relavra_via.py:260](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/relavra_via.py:260), [app/chamados/services/fabrica_da_celula.py:209](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/fabrica_da_celula.py:209)

O dado não comprova quem gravou o carimbo original, quando o veredito mudou ou se alguma dessas portas foi executada para a PK 54831. No anexo, `pai=nenhum` representa `chamado_destino`, não o chamado-pai da disputa. [nao verificado]

## 11/11 09:55 · sessao eacf68e547f2 · FIO-E0-V4-E3 · sonda · origem=Code

1) Transcrição de todos os contadores e linhas `VIVA`/`CHAMADO_VIVO` fornecidos; execução em produção sem verificação independente: [nao verificado]

```text
VIVA pk 54831 col237 2026-11-07
chamado.total n 1
pares n 77
pergunta.total n 1
pergunta.via.celula_absolveu n 1
pergunta.viva n 1
```

Não aparece linha `CHAMADO_VIVO`, nem contador `chamado.vivo` impresso na saída anexada. [nao verificado]

2) Não aparece `sonda_nao_verificada` nem traceback no dado fornecido. [nao verificado]

3) **Não: os recortes não são equivalentes.** O alcance possível pelo código é maior, pelos seguintes caminhos. (`app/chamados/reconciliador.py:627`, `app/chamados/reconciliador.py:608`)

- **Perguntas com data ausente ou divergente.** O reconciliador seleciona pelo colaborador da disputa, exige `materializada_em` e `validada_em` nulos e célula vinculada com veredito preenchido que não acusa. Não restringe `data_evento`, `celula__data` nem competência. Assim, perguntas vinculadas às células desses dias, mas com `data_evento=NULL` ou fora dos dias listados para aquele colaborador, ficam fora da sonda e podem entrar como candidatas no reconciliador. (`app/chamados/reconciliador.py:627–642`)

- **Candidata não significa pergunta efetivamente absolvida.** O gerador só entrega a pergunta quando `pergunta_viva(p)` é falsa e o dia não está trancado; o escritor ainda depende do retorno de `carimbar`. Portanto, a diferença de filtros demonstra alcance possível, sem provar que existam registros omitidos elegíveis em produção. (`app/chamados/reconciliador.py:661–672`, `app/chamados/reconciliador.py:595–597`)

- **Chamado-pai sem célula no dia sondado.** Após carimbar, o escritor pode fechar a disputa; `pode_fechar()` consulta todas as perguntas dela, sem recorte de dia. O signal busca o chamado-pai pelo seu PK e tenta reconciliá-lo, sem exigir `celula__data=dia`. Um pai sem célula ou com célula fora dos dias sondados escapa da consulta de chamados da sonda, embora possa ser alcançado por esse caminho. (`app/chamados/reconciliador.py:598–614`, `app/chamados/models.py:1848–1853`, `app/chamados/signals.py:28–42`)

- **Perguntas adicionais pela cascata do pai.** Se o chamado for encerrado, o signal pode chamar o resolvedor, cujo filtro usa `chamado_destino`, materialização nula e disputa aberta, sem limitar `data_evento`. Essas perguntas também podem ficar fora da sonda; a cascata aplica suas próprias guardas antes de carimbar. (`app/chamados/signals.py:96–115`, `app/chamados/services/disputa_emissao.py:1823–1827`, `app/chamados/services/disputa_emissao.py:1837–1865`)

Separadamente das duas funções perguntadas, a rotina irmã `retratar_chamados_absolvidos_por_celula` também alcança chamados vivos de turno sem exigir célula vinculada: determina o dia por `dia_do_fato` e busca a célula por colaborador/data. Esse dia pode vir do contexto, da batida ou da disputa. (`app/chamados/reconciliador.py:996–1004`, `app/chamados/reconciliador.py:1023–1026`, `app/chamados/juizes.py:98–138`)

Existência e quantidade de registros nessas lacunas, causa da pergunta viva e resultado de eventual reconciliação em produção: [nao verificado].

## 11/11 09:37 · sessao 2d68990e4812 · RAIZ-DA-DISPUTA · sonda · origem=Code

**O aval resolve a identidade da pausa por leitura da célula; D8 continua sendo a pergunta de lei aberta sobre BATIDA do acervo sem episódio versus BATIDA com episódio.** (`app/escala/models.py:1652`; `app/chamados/services/fio.py:1274`; complemento do aval recebido: [nao verificado].)

Os números abaixo transcrevem o dado raspado fornecido. Uso **[nao verificado]** para sua proveniência sem arquivo local numerado; as referências de código explicam os critérios, sem certificar novamente a produção. Identidade Git da cópia com o hash informado: **[nao verificado]**.

1. **Preenchimento dos `[aguarda sonda]`**

| Item do censo | Preenchimento |
|---|---|
| **77 colab-dias** | A saída informa **7.057 colab-dias com ambos os lados**, **2.777 sem um dos lados**, **31 com multiplicidade em alguma posição** e **94 com exatamente uma pergunta por posição e metade validada/outra viva**. Correspondência nominal com os antigos 77: **[nao verificado]**. |
| **486 grupos / 848 excedentes** | Não aparecem `D08.grupo`, `D08.grupos`, `D08.excedentes` nem os baldes do plano. Total atual, distribuição retratável/protegida e correspondência com 486/848: **[nao verificado]**. |
| **18 DS com `pode_fechar=True`** | A saída informa **486 DS abertas**, das quais **23 com C7 verdadeiro**. Correspondência com as antigas 18 e causa histórica da permanência abertas: **[nao verificado]**. |
| **51522/51523/col960** | A emissão contém **as duas perguntas**, ambas de **col960**; `caso.col960_confere=1` significa que a conjunção dessa conferência passou. Resposta, validação, materialização, via, DS atual, data, intervalo e participação nos outros agregados: **[nao verificado]**. | [nao verificado]

Os antigos 77, 18 e 486/848 estão documentados, respectivamente, em `app/docs/BACKLOG.md:1882`, `app/docs/BACKLOG.md:1883` e `app/docs/BACKLOG.md:1887`. **94 não comprova crescimento de 77; 23 não comprova crescimento de 18**, porque não foi fornecida a correspondência dos universos. **[nao verificado]**

As **23 DS** emitidas são os PKs **2395, 2721, 3958, 3994, 4001, 4004, 4013, 4015, 4020, 5148, 5155, 5167, 5522, 5934, 6308, 7288, 7290, 7299, 7788, 7959, 7995, 7996 e 8006**. **[nao verificado]**

A interpretação de cada família de contadores é:

| Contadores e números recebidos — **[nao verificado]** | O que medem; o que não medem |
|---|---|
| `D01.colab_dia_candidato=7057`; `falta_lado=2777`; `multiplas_linhas_na_posicao=31`; emissões `identidade_a_conferir` | A sonda agrupa por **colaborador/data**, usando posições derivadas do motivo; multiplicidade conta colab-dias, não perguntas excedentes. Não exige mesma DS nem DS aberta e não certifica intervalo específico. Duas pausas ainda compartilham motivo/posição. (`app/chamados/services/declaracao_texto.py:48`, `:55`; composição da sonda: [nao verificado].) |
| `D01.metade_validada_outra_viva=94` | Entre grupos com uma pergunta de cada posição, testa validação de uma e C6 verdadeiro da outra. Não demonstra que sejam saída/volta **da mesma pausa**, nem tempo de espera ou causa da situação. (`app/chamados/juizes.py:1324`; composição da sonda: [nao verificado].) |
| `D02.validada_sem_lastro=1181`; `D03.carimbo_sem_lastro=4291` | Validação ou materialização presente, respectivamente, com `_pergunta_materializada_de_fato` retornando exatamente `False`. Esse leitor também reconhece encerramentos sem batida; não são contagens de validações inválidas ou plantios fracassados. (`app/chamados/juizes.py:647`, `:662`, `:688`.) |
| `D03.aplicacao`: `aplicada=5641`, `aplicavel=322`, `bloqueada_paridade=182`, `invalida=327`, `ja_satisfeita=76`, `recusada_paridade=322` | Classificações atuais de perguntas **hora com texto**, pelo leitor de aplicação. Não são retornos de execução da porta. `aplicada` decorre do carimbo, excetuada a via de paridade; `ja_satisfeita` exige batida encontrada e aprovada. (`app/chamados/services/materializacao.py:425`, `:432`, `:434`, `:438`, `:442`, `:448`, `:453`, `:457`.) |
| `D03.aplicada_sem_lastro=779` | Interseção entre classificação `aplicada` e lastro `False`. Não demonstra 779 plantios malsucedidos: o leitor chama de aplicada quase toda pergunta carimbada. (`app/chamados/services/materializacao.py:434`, `:442`; `app/chamados/juizes.py:647`.) |
| `D03.reuso_nao_aprovado=102` | Perguntas cujo slot derivável encontra batida não aprovada. Não conta atos históricos de reuso nem batidas distintas. (`app/chamados/juizes.py:235`; `app/chamados/services/materializacao.py:448`, `:859`, `:865`.) |
| `D05.pendencia_com_carimbo=79`; `recusa_com_lastro=5970` | Coexistências atuais de campos: pendência/materialização e texto de recusa/lastro verdadeiro. Não demonstram estados exclusivos ou contradição: pendência pode acompanhar prosseguimento, e recusa é texto persistido. (`app/chamados/models.py:2556`, `:2560`; `app/chamados/services/materializacao.py:850`.) |
| `D05.codigo`: `ABRE_TURNO=2`, `COLISAO=27`, `COMPETENCIA_APROVADA=41`, `DATA_EM_DISPUTA=33`, `NAO_GERA_BATIDA=53`, `OUTRO=5868`, `PARIDADE=418` | Classificação do **texto atual** de recusa pelo primeiro padrão correspondente; `OUTRO` é o residual. Não enumera recusas históricas nem particiona os 94 candidatos por causa. (`app/chamados/catalogo/recusa.py:25`, `:77`, `:81`, `:88`.) |
| `D07.vizinha_retratada=15` | Perguntas hora/texto com slot derivável cuja vizinha retornada tem retratação. Não exige ausência de reuso nem efetivo bloqueio pela guarda; não prova 15 recusas causadas por retratadas. A busca compara tipo sem excluir explicitamente retratadas. (`app/chamados/services/materializacao.py:94`, `:98`, `:100`, `:104`; composição da sonda: [nao verificado].) |

| Contadores T e números recebidos — **[nao verificado]** | O que medem; o que não medem |
|---|---|
| `T01.resposta_presente=17408`; `T02.validacao_a_invalidar_por_nova_resposta=15239`; `T03.ja_validada=15239`; `T03.sem_resposta=31108` | Presença/ausência dos timestamps. T02 conta validações existentes que uma nova resposta admissível limparia; não mede novas respostas recebidas, invalidações executadas ou admissibilidade futura. (`app/chamados/models.py:2448`; `app/chamados/services/disputa_emissao.py:1470`, `:1480`; `app/chamados/services/validacao.py:393`, `:396`.) |
| `T07.fila_normal=97` | Resposta e validação presentes, materialização ausente: o predicado da fila normal. Não garante sucesso da materialização nem conta pares prontos. (`app/chamados/services/materializacao.py:1369`.) |
| `T08.data_por_conferir=157` | Divergência de data retornada pelo leitor, sem conferência. Não conta recusas efetivamente executadas. (`app/chamados/services/fio.py:40`; `app/chamados/services/materializacao.py:1191`, `:1193`.) |
| `T09.hora_sem_texto=39699` | Perguntas de tipo hora sem texto após `strip`, inclusive fora da fila de materialização. Não significa 39.699 tentativas recusadas. (`app/chamados/services/materializacao.py:829`; filtro da sonda: [nao verificado].) |
| `T10.tipo_pendente=745` | Derivações hora/texto que acrescentaram pendência de tipo não conferido à lista em memória. Não significa que a pendência foi gravada; o caminho pode prosseguir. (`app/chamados/services/materializacao.py:330`, `:386`, `:395`.) |
| `T11.reuso_no_slot=3700` | Perguntas com slot derivável para as quais o leitor encontra batida do mesmo tipo na janela. Não conta reusos executados nem batidas distintas. (`app/chamados/juizes.py:235`, `:243`.) |
| `T12.paridade_sem_lampada=442`; `T13.paridade_com_lampada=368` | Sem reuso encontrado, com vizinha do mesmo tipo e leitor de furo exatamente `False`; separam lâmpada `não True` de lâmpada `True`. T12 inclui lâmpada desconhecida. Não são recusas/carimbos executados nem predizem o resultado da aplicação conjunta do par. (`app/chamados/services/materializacao.py:875`, `:901`, `:924`; conjunção da sonda: [nao verificado].) |
| `T18.absolvivel=1` | Itens produzidos pelo leitor de absolvíveis, nos colaboradores alcançados pelas perguntas carregadas. Aplica C6 e exclui tranca; não conta DS fecháveis ou carimbos executados. (`app/chamados/reconciliador.py:645`, `:661`, `:667`, `:672`.) |
| `T19.encerra=1` | Órfãs do universo próprio cujo veredito corrente retorna `ENCERRA`. Não significa encerramento executado. (`app/chamados/reconciliador.py:1044`, `:1048`, `:1060`, `:1092`.) |
| `T20.DS_abertas=486`; `T20.C7_true=23` | DS com fechamento nulo e, entre elas, as que passam no C7 completo: nenhuma resposta sem veredito e nenhum fio vivo. Não informa por que ficaram abertas, nem equivale aos 486 **grupos** antigos. (`app/chamados/models.py:1842`, `:1848`, `:1853`; universo da sonda: [nao verificado].) |

Os contadores `estado.r*.v*.m*` são combinações de presença dos timestamps **resposta/validação/materialização**, não a propriedade `estado`, o C6 ou uma história de transições. (`app/chamados/models.py:2448`, `:2449`, `:3033`, `:3393`; construção da sonda: [nao verificado].)

| Combinação | Número recebido |
|---|---:|
| `r0.v0.m0` | 11.115 — [nao verificado] |
| `r0.v0.m1` | 19.993 — [nao verificado] |
| `r1.v0.m0` | 972 — [nao verificado] |
| `r1.v0.m1` | 1.197 — [nao verificado] |
| `r1.v1.m0` | 97 — [nao verificado] |
| `r1.v1.m1` | 15.142 — [nao verificado] |

`via.*` conta perguntas pelo **valor atual não vazio** de `via_resolucao`; valores fora de `VIAS` vão para `nao_catalogada`. Não mede atos históricos nem prova lastro. (`app/chamados/models.py:2525`, `:2561`; agrupamento da sonda: [nao verificado].)

| Vias | Números recebidos — **[nao verificado]** |
|---|---|
| `adesao`; `admin_decidiu`; `ausencia_dia`; `cadastro_folga` | 2.296; 3.071; 1.114; 341 |
| `celula_absolveu`; `chamado_encerrado`; `competencia_trancada`; `completude_turno` | 3.644; 7.021; 1.949; 3.380 |
| `declaracao`; `duplicata`; `fantasma`; `fato_ocupado` | 5.425; 4.689; 1.047; 27 |
| `juiz_aprovou`; `legado_sem_lastro`; `nao_catalogada`; `paridade_recusada`; `sem_turno_aberto` | 339; 24; 325; 324; 1.316 |

2. **Falhas e respostas ausentes**

Não há `sonda_nao_verificada` nem `nao_verificado.*` na saída recebida. Portanto, **nenhum ponto de falha foi informado**; não há base para localizar uma exceção em produção. **[nao verificado]**

A ausência de D08 também **não prova falha**. No script anexado, suas chaves só são incrementadas dentro do laço; o leitor pode devolver uma lista vazia. A presença das emissões posteriores de DS e caso não sustenta afirmar aborto naquele laço. (`app/chamados/services/fio.py:1942`, `:1993`; execução e integralidade da saída: [nao verificado].)

Assim, ficam sem resposta **a reconciliação dos 486/848 com o universo real, os baldes de retratação, o alcance de D8 e as divergências numéricas do explicador/relatório contra C7**. Não preencho essas ausências com zero. **[nao verificado]**

Ausência de marcador externo tampouco certifica ausência de falhas internas: o leitor de furo captura exceção e retorna `False`; o gerador de absolvíveis captura exceção e ignora a pergunta. Ocorrência dessas falhas nesta execução: **[nao verificado]**. (`app/chamados/services/materializacao.py:184`; `app/chamados/reconciliador.py:669`.)

3. **AST: já existe a função “estas duas perguntas são o par da mesma pausa”?**

**Não identifiquei esse predicado no AST inspecionado.** Ausência universal por reflexão/código dinâmico: **[nao verificado]**. Existem a representação dos pares na célula e leitores de posições/instâncias, com contratos diferentes:

| Sítio existente | Contrato |
|---|---|
| `CelulaDia.PARES_PAUSA_DNA` | Declara os pares `hii/hfi` e `hii2/hfi2`; a entidade fornece tipos e ordem canônica. É a identidade dos **marcos**, não uma comparação entre perguntas. (`app/escala/models.py:1652`, `:1654`, `:1659`.) |
| `EscalaColaborador.pausas_do_dia` | Lê todas as pausas completas do DNA; célula existente sem marcos devolve vazio. `intervalo_do_dia` lê somente a primeira. (`app/escala/models.py:1163`, `:1189`, `:1197`, `:1202`.) |
| `grade_da_celula` / `_marcos_faltantes` | Transportam marcos/lâmpadas e retornam faltantes como dia, posição, tipo e hora. (`app/escala/services/leitor_celula.py:114`, `:148`; `app/chamados/services/declaracao_texto.py:86`, `:100`.) |
| `posicao_do_marco` | Primeiro=`inicio`, último=`fim`, intermediário S=`int_ida`, E=`int_volta`. Sozinha não distingue o intervalo. (`app/chamados/services/declaracao_texto.py:75`, `:79`, `:83`.) |
| `tarefa_do_dia._posicoes` / `instancias_do_turno` | Enumeram repetições em memória, incluindo `int_ida:2`/`int_volta:2`, e publicam instância, posição, tipo, hora e lâmpada. (`app/chamados/services/tarefa_do_dia.py:57`, `:65`, `:67`, `:91`, `:101`.) |
| `fato_do_marco_no_espelho` | Para motivos de intervalo, considera os marcos correspondentes de **todas** as pausas; não identifica qual pausa uma pergunta individual cobra. (`app/chamados/juizes.py:911`, `:919`, `:922`.) |

Há uma perda concreta do índice na pergunta persistida: `_chave_de_identidade` deriva `instancia_marco` apenas do motivo e rejeita um valor diferente, como o sufixo `:2`. O comentário reconhece a separação das pausas como pendente. (`app/chamados/models.py:3437`, `:3440`, `:3441`, `:3445`; `app/chamados/services/declaracao_texto.py:49`.)

Quem produz/escreve esses dados:

- **DNA e lâmpadas:** `montar_dna` produz os pares de marcos; `montar_lampadas` produz chave, tipo, hora e instante. (`app/ponto/management/commands/gerar_celulas.py:73`, `:83`, `:92`, `:36`, `:60`, `:68`.)
- **Persistência da célula:** portas de geração, regeneração, edição e reversão. (`app/ponto/portas/celula.py:857`, `:256`, `:272`, `:591`, `:604`, `:786`.)
- **Posição da pergunta:** `save` chama `_chave_de_identidade` no nascimento, que escreve a posição simples. (`app/chamados/models.py:3420`, `:3440`.)
- **Índice da tarefa:** `_posicoes` o produz apenas na estrutura retornada em memória. (`app/chamados/services/tarefa_do_dia.py:65`, `:67`.)

Portanto, o encaixe técnico deve consumir esses marcos da célula e preservar sua identidade até as perguntas; o aval proíbe derivação paralela, juiz novo e gravação de “aguardando metade”. **[nao verificado — determinação do anexo recebido.]**

4. **D1–D8 depois do aval**

| Decisão | O que permanece aberto | Natureza e fundamento |
|---|---|---|
| **D1 — unidade/espera/identidade** | Implementar o ato conjunto e ligar cada pergunta ao intervalo correto da célula. A validação e o lote atuais ainda operam individualmente. (`app/chamados/services/validacao.py:461`, `:463`; `app/chamados/services/materializacao.py:1375`.) | **Decisão fechada; execução técnica aberta.** Par no mesmo ato e estado derivado já registrados; fonte única S133 e L-099 para ausência de cadastro. (`app/docs/BACKLOG.md:1929`, `:1930`; `CLAUDE.md:148`; `app/docs/LEIS.md:122`.) Sem marco, seguir sozinho: **[nao verificado — aval recebido]**. |
| **D2 — resposta/validação/reversão** | Harmonizar resposta humana, timestamps supridos e reversões, preservando atos e trilha. (`app/chamados/services/disputa_emissao.py:1480`; `app/chamados/models.py:3132`, `:3135`; `app/chamados/services/validacao.py:199`.) | **Adequação técnica sob lei escrita**, L-007: escritor canônico e auditoria. (`app/docs/LEIS.md:65`.) |
| **D3 — aplicada versus encerrada** | Adequar leitores que tratam carimbo como aplicação; os 779 são interseção corrente, não diagnóstico histórico. (`app/chamados/services/materializacao.py:434`, `:442`; número: [nao verificado].) | **Adequação técnica sob lei escrita**, testemunha lê autoridade; identidade pode encerrar sem plantar. (`app/docs/LEIS.md:60`; `app/chamados/services/materializacao.py:924`, `:1099`.) |
| **D4 — escritores de via** | Organizar carimbo, descarimbo, troca isolada e relavra sob portas canônicas com trilha. (`app/chamados/models.py:3101`, `:3266`, `:3275`; `app/chamados/services/relavra_via.py:125`.) | **Adequação técnica sob L-007**, sem pergunta de lei nova. (`app/docs/LEIS.md:65`.) |
| **D5 — pendência/recusa** | Adequar consumidores para não inferirem espera da metade desses campos. Os campos têm outros usos atuais. (`app/chamados/services/materializacao.py:850`; `app/chamados/services/disputa_emissao.py:1497`.) | **Adequação técnica**; estado explícito superado e fonte/escritor únicos já escritos. (`app/docs/BACKLOG.md:1930`; `app/docs/LEIS.md:60`, `:65`.) |
| **D6 — fechamento** | Migrar explicador/relatório para C7 e implementar reação da raiz; causalidade histórica das DS continua sem resposta. (`app/chamados/services/explicador.py:137`; `app/chamados/relatorios_disputa.py:20`; histórico: [nao verificado].) | **Autoridade decidida por lei escrita:** C7 único. (`app/docs/CORTES.md:272`; `app/core/juizes.py:279`.) Autor do fecho automático = autor do ato que carimbou a última pergunta, com motivo próprio: **decisão técnica explícita**, [nao verificado — aval recebido]. |
| **D7 — vizinhança/universo/papel** | Corrigir e verificar a guarda pelos leitores existentes; 15 vizinhas retratadas não medem seu efeito causal. (`app/chamados/services/materializacao.py:94`, `:104`; número/causalidade: [nao verificado].) | **Adequação técnica sob leis escritas** L-025 e L-087: universo apurável e papel pelo marco. (`app/docs/LEIS.md:76`, `:119`.) |
| **D8 — acervo sem episódio** | Definir quando BATIDA sem episódio e BATIDA com episódio são o mesmo fato, permitindo retratação entre elas. Alcance nos 486/848 permanece sem medição apresentada. (`app/chamados/services/fio.py:1274`, `:1275`; alcance: [nao verificado].) | **Pergunta de lei aberta.** O comportamento conservador atual mantém `sem_episodio_intocada`; o aval da pausa não fornece resposta para essa identidade. (`app/chamados/services/fio.py:1294`, `:1295`; alcance do aval recebido: [nao verificado].) |

**Perguntas de lei restantes: 1 — D8.** (`app/chamados/services/fio.py:1275`.)

## 11/11 09:35 · sessao ad850049a991 · - · - · origem=Ronald (tmux)

A sonda inline não tem arquivo local disponível; os números abaixo vêm apenas do anexo, portanto sua proveniência por `arquivo:linha` é **[nao verificado]**. Os leitores citados foram conferidos na cópia.

| Contador / saída | O que efetivamente mede / limite |
|---|---|
| `D01.colab_dia_candidato=7057` | Chaves `(colaborador_id,data_evento)` com pelo menos uma pergunta de cada posição, após exclusão de conversa/fora do fato e conferência `instancia_marco == instancia_do_motivo`. A posição vem de `app/chamados/services/declaracao_texto.py:55`. Não certifica intervalo específico; duas pausas ainda compartilham posição/motivo, limitação declarada em `:49`. |
| `D01.falta_lado=2777` | Chaves candidatas que não têm ambos os lados. Não são necessariamente pausas incompletas da célula; é ausência de perguntas em uma posição. **[nao verificado por arquivo:linha: lógica da sonda inline]** |
| `D01.multiplas_linhas_na_posicao=31` | Colab-dias com ambos os lados e cardinalidade diferente de um em pelo menos um lado. Incluídos nos 7057 e excluídos do teste de metade validada. Não conta duplicatas retratáveis ou quantidade de intervalos. **[nao verificado por arquivo:linha: sonda inline]** | [nao verificado]
| `D01.metade_validada_outra_viva=94` | Colab-dias com exatamente uma pergunta por posição e `(validada(a) ∧ C6(b)) ∨ (validada(b) ∧ C6(a))`. C6 vem de `app/chamados/juizes.py:839`; validada retorna morta em `:1325`. Não demonstra 94 pares reais da mesma pausa, nem histórico de espera, devolução ou recusa. |
| `D02.validada_sem_lastro=1181` | Perguntas com validação e `_pergunta_materializada_de_fato is False`. Esse leitor admite encerramentos por identidade e outras testemunhas, e não significa exclusivamente existência de batida nova: `app/chamados/juizes.py:647`, `:662`, `:688`, `:734`. Não prova 1181 validações inválidas. |
| `D03.carimbo_sem_lastro=4291` | Materialização timestamp presente e leitor de lastro `False`; mesma ressalva acima. |
| `D03.aplicacao.*` | Só perguntas `tipo_resposta='hora'` e texto não vazio. Retornos: aplicada 5641; aplicável 322; bloqueada_paridade 182; inválida 327; já_satisfeita 76; recusada_paridade 322. `app/chamados/services/materializacao.py:432`, `:434`, `:438`, `:442`, `:444`, `:448`, `:452`, `:457`. Não são retornos de execução da porta. |
| `D03.aplicada_sem_lastro=779` | Interseção de estado de aplicação `aplicada` e lastro `False`. O estado `aplicada` decorre do carimbo para qualquer via que não seja paridade: `app/chamados/services/materializacao.py:434`, `:438`, `:442`. Não equivale a 779 plantios malsucedidos. |
| `T11.reuso_no_slot=3700`; `D03.reuso_nao_aprovado=102` | Entre perguntas hora/texto com slot derivável, encontro do leitor de reuso; 102 encontram batida não aprovada. Uso real no escritor em `app/chamados/services/materializacao.py:859`, `:865`; UI requer aprovação em `:448`. Não demonstra 3700 reusos históricos. |
| `D05.pendencia_com_carimbo=79` | Campo de pendência não vazio e timestamp de materialização. Pendência pode coexistir com prosseguimento do plantio: `app/chamados/services/materializacao.py:845`, `:850`, `:852`. |
| `D05.recusa_com_lastro=5970` | Texto atual de recusa não vazio com lastro `True`. Não demonstra contradição ou recusa atual impeditiva, porque é texto remanescente e lastro de leitura atual. **[nao verificado por arquivo:linha: conjunção da sonda]** |
| `D05.codigo.*` | Classificação do texto atual de recusa por primeiro padrão textual correspondente; `OUTRO` é fallback. `app/chamados/catalogo/recusa.py:77`, `:81`, `:88`. Contagens: ABRE_TURNO 2; COLISAO 27; COMPETENCIA_APROVADA 41; DATA_EM_DISPUTA 33; NAO_GERA_BATIDA 53; OUTRO 5868; PARIDADE 418. Não é enumeração de atos históricos nem causas exclusivas dos 94 colab-dias. |
| `D07.vizinha_retratada=15` | Para hora/texto com slot derivável, o leitor retorna vizinha do mesmo tipo cuja `retratada_em` está preenchida. Busca anterior/posterior sem filtro de retratação: `app/chamados/services/materializacao.py:94`, `:98`, `:100`, `:104`. Não exige que a guarda bloqueie; nem que não haja reuso; não prova 15 recusas causadas por retratadas. |
| `T18.absolvivel=1` | Quantidade produzida por `perguntas_absolvidas_pela_celula` nos colaboradores alcançados pela lista de perguntas. O leitor aplica C6 e exclui dia trancado: `app/chamados/reconciliador.py:645`, `:661`, `:667`, `:672`. Não é fechamento executado ou DS fechável. |
| `T19.encerra=1` | Órfãs no universo `materializada=None`, `validada=None`, DS aberta, motivos específicos e dia presente, cujo veredito atual é ENCERRA: `app/chamados/reconciliador.py:1044`, `:1048`, `:1082`, `:1084`, `:1086`, `:1090`, `:1092`. Não é carimbo executado. |
| `T20.DS_abertas=486`; `T20.C7_true=23` | DS com `fechada_em=None`; dentre elas, C7 retorna `True`. C7 exige ausência de resposta sem veredito e fio vivo: `app/chamados/models.py:1842`, `:1848`, `:1853`. Não explica por que não fecharam nem identifica as antigas 18. |
| Caso | Existem exatamente as duas perguntas pedidas, ambas col960, segundo `caso_51522_51523 n2` e `caso.col960_confere n1`. Não foram impressos timestamps, C6, DS, data, marco ou intervalo delas. Estado e pertencimento a 94/486/23/grupos continuam **[nao verificado]**. |

**D08 não foi medido na saída disponibilizada.** Não há `D08.grupo`, `D08.grupos`, `D08.excedentes` nem baldes. As assinaturas do inline são compatíveis com a cópia: `app/chamados/services/fio.py:1225`, `:1259`, `:1905`, `:1923`. `grupos_de_colisao` começa com lista vazia e pode devolvê-la (`:1942`, `:1993`); o `Counter` inline só toca D08 dentro do laço. Assim, ausência de chaves não demonstra falha e não autoriza preencher o censo como zero. Os 486 DS abertas não são evidência dos 486 grupos antigos.

Não aparece `sonda_nao_verificada` ou `nao_verificado.*` no dado. T20, T18/T19 e caso presentes tornam indevida a afirmação de aborto no laço D08 para o inline exatamente mostrado. Qual script exato produziu a saída e por que D08 está ausente: **[nao verificado]**.

**Limite dos próprios leitores:** `_plantio_preenche_furo_na_data` engole exceção e retorna `False` (`app/chamados/services/materializacao.py:184`); o gerador de absolvíveis captura exceção e ignora pergunta (`app/chamados/reconciliador.py:669`). Portanto ausência de `nao_verificado.*` externo não certifica inexistência de falhas internas. Logs não fornecidos: **[nao verificado]**.

**Divergências C7:** explicador calcula somente `not fio_vivo` (`app/chamados/services/explicador.py:137`); relatório publica PRONTA_FECHAR se não houver vivas (`app/chamados/relatorios_disputa.py:18`, `:20`). Nenhum número de divergências foi impresso. Não convertê-lo em zero sem esclarecer a instrumentação/saída.

## 11/11 09:35 · sessao 9ad8f4f4c039 · - · - · origem=Ronald (tmux)

Não localizei no AST um predicado que receba **duas `PerguntaDisputa` e responda se pertencem à mesma pausa**. A ausência universal por reflexão, código gerado ou chamadas dinâmicas permanece **[nao verificado]**. A inspeção executada em memória percorreu 838 arquivos Python de runtime em `app/` e `bin/`, com 5.422 funções e nenhum erro de parse; essa execução não tem artefato `arquivo:linha` **[nao verificado por arquivo:linha]**.

O par dos marcos já está representado na própria célula: `CelulaDia.PARES_PAUSA_DNA` contém `('hii','hfi')` e `('hii2','hfi2')`; a entidade atribui S/E e fornece a ordem canônica. Isso identifica **marcos**, sem comparar perguntas. (`app/escala/models.py:1652`, `:1654`, `:1659`, `:1672`.)

| Leitor existente | O que responde |
|---|---|
| `EscalaColaborador.pausas_do_dia` | Todas as pausas do DNA da célula; célula existente sem marcos devolve `()`. Template somente quando falta célula. (`app/escala/models.py:1166`, `:1189`, `:1197`, `:1202`, `:1208`.) |
| `intervalo_do_dia` | Somente a primeira pausa retornada pela irmã acima. (`app/escala/models.py:1163`.) |
| `grade_da_celula` / `_celulas_da_ata` | Marcos e lâmpadas lavrados do dia; expõe tipo/hora, preservando ordem. (`app/escala/services/leitor_celula.py:114`, `:122`, `:148`, `:350`.) |
| `posicao_do_marco` | Primeiro=`inicio`, último=`fim`, intermediário S=`int_ida`, E=`int_volta`. Não identifica sozinho qual intervalo. (`app/chamados/services/declaracao_texto.py:75`, `:79`, `:83`.) |
| `_marcos_faltantes` | Lê a grade e retorna `(dia,pos,tipo,hora)` dos marcos faltantes. Não devolve índice da pausa. (`app/chamados/services/declaracao_texto.py:100`, `:121`, `:130`, `:139`.) |
| `tarefa_do_dia._posicoes` | Enumera repetições em memória: `int_ida`, `int_ida:2`, `int_volta`, `int_volta:2`. (`app/chamados/services/tarefa_do_dia.py:57`, `:65`, `:67`.) |
| `instancias_do_turno` | Publica instância, posição, tipo, hora, lâmpada e data civil a partir do dia do leitor único. (`app/chamados/services/tarefa_do_dia.py:91`, `:96`, `:101`.) |
| `_pos_da_pergunta` | Lê `instancia_marco`, com alternativa pelo motivo; não compara duas perguntas. (`app/chamados/services/tarefa_do_dia.py:106`.) |
| `pausas_dna_periodo` / `pausas_de_celulas` | Leitores em lote das pausas pelas chaves da própria entidade. (`app/escala/utils.py:27`, `:57`, `:111`, `:141`.) |

Há um impedimento concreto à persistência do índice já produzido pela tarefa: o nascimento da pergunta deriva `instancia_marco` somente do motivo e **rejeita valor diferente**, como `int_ida:2`. O próprio comentário reconhece a separação de duas pausas como pendente. (`app/chamados/models.py:3425`, `:3437`, `:3440`, `:3441`, `:3445`.) A tabela motivo→posição também documenta que duas pausas caem no mesmo motivo; `_complementar_marcos_faltantes` consome os marcos, mas descarta `_hora` ao criar a pergunta. (`app/chamados/services/declaracao_texto.py:48`, `:55`; `app/chamados/services/disputa_emissao.py:873`, `:881`.)

Os produtores/escritores relacionados são:

- `montar_dna` produz `hii/hfi` e o segundo par pelas chaves de `PARES_PAUSA_DNA`; `montar_lampadas` produz as lâmpadas com `chave`, tipo, hora e instante. (`app/ponto/management/commands/gerar_celulas.py:73`, `:81`, `:92`, `:94`, `:36`, `:60`, `:68`.)
- A porta da célula persiste geração, regeneração, edição humana e reversão. (`app/ponto/portas/celula.py:816`, `:857`, `:110`, `:256`, `:272`, `:534`, `:591`, `:604`, `:740`, `:786`.)
- `PerguntaDisputa.save` chama `_chave_de_identidade` na inserção; esta escreve a posição simples em `instancia_marco`. (`app/chamados/models.py:3413`, `:3420`, `:3425`, `:3440`.)

Portanto, existem a autoridade dos pares de marcos e leitores de posições/instâncias; a ligação persistida **pergunta→marco específico da pausa** ainda perde o índice nos sítios acima. Não se deve chamar `_posicoes` de juiz “estas duas perguntas são o mesmo par”: seu contrato é enumerar os marcos da ata. (`app/chamados/services/tarefa_do_dia.py:57`, `:67`; `app/chamados/models.py:3437`, `:3445`.)

## 11/11 09:34 · sessao b113ca5b5451 · - · - · origem=Ronald (tmux)

Após o aval, **D1–D7 não têm pergunta de lei pendente; continuam com trabalho técnico de implementação/verificação. D8 mantém a pergunta de lei sobre identidade BATIDA do acervo versus BATIDA com episódio.** O novo aval resolve especificamente a identidade da pausa pela célula; não responde à identidade de D8. (`app/chamados/services/fio.py:1274`, `app/chamados/services/fio.py:1275`; conteúdo do aval recebido: **[nao verificado por arquivo:linha]**.)

| D | Situação após aval | Lei escrita versus decisão técnica; evidência |
|---|---|---|
| **D1** | **Decisão fechada; implementação aberta.** Aplicar o par no mesmo ato, esperar a outra metade em estado derivado; identificar saída/volta do mesmo intervalo pelos marcos da célula. Sem marcos, cada pergunta segue sozinha. | O ato conjunto está em `app/docs/BACKLOG.md:1929`; estado explícito foi superado pelo derivado em `app/docs/BACKLOG.md:1930`. Fonte única de previsto: S133/L-024, `app/docs/LEIS.md:75`; testemunha lê a autoridade: `CLAUDE.md:20`; cadastro não se cura por código: L-099, `app/docs/LEIS.md:122`. O complemento específico do novo aval está apenas no texto recebido: **[nao verificado por arquivo:linha]**. Hoje a validação ainda materializa uma P (`app/chamados/services/validacao.py:461`, `:463`) e o lote itera individualmente (`app/chamados/services/materializacao.py:1375`, `:1376`). |
| **D2** | **Sem pergunta nova; harmonização técnica aberta.** Preservar diferença entre resposta humana, projeção suprida e reversão. | Escritor canônico e trilha são L-007 (`app/docs/LEIS.md:65`). Nova resposta limpa validação (`app/chamados/services/disputa_emissao.py:1468`, `:1480`); devolução também limpa, registrando DEVOLUÇÃO (`app/chamados/services/validacao.py:199`, `:205`); carimbo pode suprir resposta/validação (`app/chamados/models.py:3132`, `:3135`). Como compatibilizar essas portas é decisão técnica sob a lei escrita. |
| **D3** | **Sem pergunta nova; significado e leitores técnicos abertos.** Carimbo terminal não prova plantio. | Testemunhas leem autoridade (`CLAUDE.md:20`; `app/docs/LEIS.md:60`). Há encerramento por identidade sem criar batida (`app/chamados/services/materializacao.py:924`, `:1099`), enquanto o leitor de aplicação chama quase todo carimbo de “aplicada” (`app/chamados/services/materializacao.py:434`, `:438`, `:442`) e histórico o descreve como fato registrado (`app/chamados/services/historico.py:103`, `:104`). Adequação dessas testemunhas é técnica. |
| **D4** | **Sem pergunta nova; convergência técnica aberta.** | Um escritor canônico e exceção com trilha: L-007 (`app/docs/LEIS.md:65`; `CLAUDE.md:32`). Existem atribuição comum (`app/chamados/models.py:3101`), descarimbo em lote (`:3266`), troca isolada (`:3275`, `:3276`) e relavra (`app/chamados/services/relavra_via.py:125`, `:127`). Organizar as portas existentes sob a obrigação já escrita é técnico. |
| **D5** | **Sem pergunta nova; consumidores técnicos abertos.** Não tratar pendência/recusa como “aguarda metade”. | Estado explícito foi superado (`app/docs/BACKLOG.md:1930`); fonte e escritor únicos são lei (`CLAUDE.md:20`, `:32`). Pendência grava motivo (`app/chamados/services/materializacao.py:850`, `:851`); contestação humana também grava motivo (`app/chamados/services/disputa_emissao.py:1497`); COLISÃO pode encerrar identidade (`app/chamados/services/materializacao.py:1093`, `:1099`). Separar esses usos é técnico. |
| **D6** | **Autoridade fechada; migração e causalidade histórica abertas.** | C7 é juiz único por corte expresso (`app/docs/CORTES.md:272`), registrado em `app/core/juizes.py:279`; sua regra completa está em `app/chamados/models.py:1853`. Explicador só consulta fio vivo (`app/chamados/services/explicador.py:137`), relatório publica PRONTA_FECHAR sem vivas (`app/chamados/relatorios_disputa.py:18`, `:20`). Migrá-los é técnico. Autor automático = autor do último ato com motivo próprio é decisão técnica explícita no novo aval: **[nao verificado por arquivo:linha]**. Razão histórica de cada DS continuar aberta: **[nao verificado]**. |
| **D7** | **Lei fechada; avaliação/correção técnica aberta.** | Universo apurável exclui retratada: L-025 (`app/docs/LEIS.md:76`); papel vem do marco, não do tipo gravado: L-087 (`app/docs/LEIS.md:119`). A guarda atual lê `Batida.objects` e compara `.tipo` (`app/chamados/services/materializacao.py:94`, `:98`, `:100`, `:104`). Isso não autoriza remover a guarda sem definir/testar comportamento pela autoridade existente; solução é técnica. |
| **D8** | **Pergunta de lei ainda aberta.** Quando BATIDA do acervo sem episódio representa o mesmo fato de BATIDA com episódio, permitindo retratação entre ambas? | O código declara literalmente a lei aberta e conserva o acervo (`app/chamados/services/fio.py:1274`, `:1275`, `:1294`, `:1295`). O plano também protege mérito (`:1290`, `:1291`) e episódios distintos (`:1287`, `:1292`, `:1293`). O novo aval sobre pares da pausa não responde a essa identidade: **[nao verificado por arquivo:linha quanto ao alcance do anexo]**. |

Referências dos agregados antigos:

- **77 colab-dias**, 48 colabs e 18 na competência 11 estão registrados em `app/docs/BACKLOG.md:1882`.
- **18 DS com pode_fechar=True**, 77 pares e 848 duplicadas aparecem na descrição da raiz em `app/docs/BACKLOG.md:1883`.
- **486 grupos/848 excedentes em disputa aberta** estão registrados em `app/docs/BACKLOG.md:1887`.

Essas linhas documentam a medição anterior; não demonstram equivalência entre seu universo e o da sonda recebida. (`app/docs/BACKLOG.md:1882`, `:1883`, `:1887`; equivalência dos universos: **[nao verificado]**.)

## 11/11 09:26 · sessao d8f1db422342 · FIO-E0-V4-E4B · construtor · origem=Code

Corrigi a fixture em [test_e4b_c4_orm.py:375](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:375). **GREEN ORM e morte ORM de `sem_rollback` continuam [nao verificado].**

1. **A deficiência demonstrada é a falta de protesto cobrável na fixture.** A lavra sem `codigos` grava `protestos=[]` (`ponto/portas/celula.py:1011`); o c2 recusa criar o chamado sem esse protesto (`chamados/reconciliador.py:867`). A assertion exige pergunta pendente **com destino**, portanto sua falha não demonstra ausência de qualquer pergunta (`chamados/tests/test_e4b_c4_orm.py:429`).

   O aviso é impresso em `chamados/services/declaracao_texto.py:71`, chamado pela abstenção em `chamados/services/disputa_emissao.py:911`. O aviso fornecido nomeia 21/09, dia de P90; o vizinho é 02/09 (`chamados/tests/test_e4b_c4_orm.py:21`, `chamados/tests/test_e4_backfill.py:28`). Pelo ponto alcançado na falha fornecida, as checagens de vizinho resolvido e ausência de rollback integral já haviam passado (`chamados/tests/test_e4b_c4_orm.py:426`).

2. **A fixture agora lavra `FURO_PARCIAL` e verifica protesto persistido e ata legível**, incluindo a entrada apagada (`chamados/tests/test_e4b_c4_orm.py:375`). Permanecem a precondição de persistência e a assertion-alvo de P90 (`chamados/tests/test_e4b_c4_orm.py:429`, `chamados/tests/test_e4b_c4_orm.py:434`).

3. **Defeito de produção perdendo escrita aceita: [nao verificado].** A correção ficou nos testes. O novo ensaio executa a chamada extraída da fixture, a lavra real e o corpo inteiro do c2; suas bordas simuladas não demonstram persistência ou sinais ORM ([test_e4b_c4v11_memoria.py:1](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4v11_memoria.py:1)). [nao verificado]

Diff central:

```diff
-        cel_vizinha.lavrar_veredito(via='cartorio', veredito='furo', ata=self.cel.ata)
+        cel_vizinha.lavrar_veredito(via='cartorio', veredito='furo',
+                                   codigos=['FURO_PARCIAL'], ata=self.cel.ata)
```

O diff também acrescenta as precondições em `test_e4b_c4_orm.py:377`, o novo RED com três mutantes em `test_e4b_c4v11_memoria.py:31` e sua inclusão no roteiro em [e4b_c4_verificar.py:68](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_verificar.py:68).

Resultados locais observados:

| Verificação | rc | Resultado / referência |
|---|---:|---|
| RED antes da correção | 1 | Assertion de destino; `test_e4b_c4v11_memoria.py:114` |
| Memória completa após alterações | 0 | 90 testes; comando abaixo | [nao verificado]
| `--selos` | 0 | 40 testes; `e4b_c4_verificar.py:238` | [nao verificado]
| `--orm-preflight` | 0 | 8 enxertos; `e4b_c4_verificar.py:78` |
| `sem_protestos` | 1 | Assertion de destino; `test_e4b_c4v11_memoria.py:114` |
| `protesto_nao_cobravel` | 1 | Mesma assertion; `test_e4b_c4v11_memoria.py:114` |
| `roteiro_sem_fixture` | 1 | Assertion de inclusão; `test_e4b_c4v11_memoria.py:52` |
| `sem_rollback`, memória | 1 | Assertion-alvo de P90; `test_e4b_c4v10_memoria.py:89` |
| 24 mutantes c4v7 | 1 cada | `AssertionError`, sem erros; catálogo em `test_e4b_c4v7_memoria.py:441` |
| GREEN / mutantes ORM | — | [nao verificado] |

Comandos reproduzíveis:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  chamados.tests.test_e4b_classe_e_memoria \
  chamados.tests.test_e4b_c4_tranca_memoria \
  chamados.tests.test_e4b_c4v7_memoria \
  chamados.tests.test_e4b_c4v10_memoria \
  chamados.tests.test_e4b_c4v11_memoria \
  chamados.tests.test_e4b_c4_base \
  chamados.tests.test_e4b_c4_orm_protocolo -q

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --selos
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --orm-preflight

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v11_memoria --mutante sem_protestos
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v11_memoria --mutante protesto_nao_cobravel
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v11_memoria --mutante roteiro_sem_fixture
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v10_memoria --mutante sem_rollback
```

Para a sessão executar o ORM, primeiro o GREEN isolado, depois `sem_rollback` e o roteiro completo. A assertion esperada do mutante permanece em `chamados/tests/test_e4b_c4_orm.py:435`; os resultados abaixo são [nao verificado].

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4b_c4_orm.CercaC4ORMTest.test_RED_lote_misto_desfaz_recusado_enquanto_vizinho_pousa --noinput

E4B_C4_MUTANTE=sem_rollback PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4b_c4_orm.CercaC4ORMTest.test_RED_lote_misto_desfaz_recusado_enquanto_vizinho_pousa --noinput

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --orm
```

## 11/11 09:19 · sessao 492833d88911 · RAIZ-DA-DISPUTA · censo · origem=Code

**F3/F4 têm oito decisões identificadas e uma pergunta de lei: a identidade entre BATIDA do acervo sem episódio e BATIDA com episódio.** A aplicação conjunta do par, a espera pela outra metade, o estado derivado e a autoridade C7 já têm decisão registrada; suas divergências atuais são de implementação. (`app/chamados/services/fio.py:1275`; `app/docs/BACKLOG.md:1929`, `:1930`; `app/docs/CORTES.md:272`.)

Identidade Git da cópia com o hash informado: **[nao verificado]**. Estado atual de col960 e das perguntas 51522/51523: **[nao verificado; aguarda sonda]**. Os anexos F0/F1 permanecem como base, sem novo levantamento de nascimento, dona ou destino nesta entrega. **[nao verificado por arquivo:linha: referência aos anexos recebidos]**

Os números fornecidos são **77 colab-dias**, **486 grupos/848 excedentes** e **18 DS com `pode_fechar=True`**; não constituem partição por causa, transição ou decisão. A ligação dos 77 e dos 18 à obra está registrada; equivalência dos 486/848 ao universo do código e distribuição de todos esses números: **[aguarda sonda]**. (`app/docs/BACKLOG.md:1882`, `:1883`; chave dos grupos: `app/chamados/services/fio.py:1923`, `:1945`, `:1976`.)

O censo abaixo usa os seguintes prefixos. Cada arquivo da primeira coluna, combinado com cada número das demais colunas, constitui uma referência `arquivo:linha`.

| Prefixo | Caminho |
|---|---|
| C | `app/chamados/` |
| S | `app/chamados/services/` |
| Q | `app/chamados/management/commands/` |
| H | `app/templates/chamados/partials/` |
| P | `app/ponto/` |
| T | `app/chamados/tests/` |

**DENTRO** significa `C/models.py` ou `S/`, conforme o perímetro pedido; **FORA** compreende juízes, reconciliador, commands, views, templates e `bin/`. Essa classificação é por localização, não uma prova de que todo método localizado em `models.py` execute pela raiz DS. (`app/chamados/models.py:1848`, `:3103`; `app/chamados/reconciliador.py:340`.)

Os nomes reais e as distinções de estado são:

| Pedido | Nome real / natureza |
|---|---|
| respondida | Coluna `PerguntaDisputa.respondida_em`; `respondida` é **propriedade**, definida por timestamp não nulo. (`C/models.py:2448`, `:3496`, `:3498`.) |
| validada | Coluna `PerguntaDisputa.validada_em`, acompanhada de autor nas portas. (`C/models.py:2449`; `S/validacao.py:134`, `:135`.) |
| materializada | Coluna `PerguntaDisputa.materializada_em`, acompanhada de autor; seu help text fala em batida criada, mas portas também a usam para encerramento sem plantio. (`C/models.py:3393`, `:3395`; `S/materializacao.py:924`, `:1099`.) |
| via | Coluna `PerguntaDisputa.via_resolucao`. (`C/models.py:2561`.) |
| pendência | Coluna `PerguntaDisputa.pendencia_admin`; não é via de encerramento. (`C/models.py:2556`, `:2558`.) |
| recusa | Coluna `PerguntaDisputa.recusa_motivo`. (`C/models.py:2560`.) |
| pode_fechar | **Método derivado de `DisputaSupervisao`, não coluna nem property**: ausência de resposta sem veredito **e** ausência de fio vivo. (`C/models.py:1848`, `:1853`.) |
| estado | **Propriedade derivada da pergunta**; traduz validação, materialização, resposta e C6. (`C/models.py:3033`, `:3041`, `:3044`, `:3048`.) |

O AST foi executado por `python3 -B` em stdin, com revisão dos fluxos dinâmicos; não foi gravado artefato de execução. **[nao verificado por arquivo:linha quanto à execução]** A unidade das tabelas é **uma linha por `(campo, arquivo)`**, reunindo os sítios desse arquivo. Portanto as contagens abaixo são linhas das tabelas, não funções, atribuições ou registros de produção. **[nao verificado por arquivo:linha; reproduzível pelo script ao fim]**

| Campo | Linhas da tabela |
|---|---:|
| `respondida_em` | 33 |
| `validada_em` | 22 |
| `materializada_em` | 23 |
| `via_resolucao` | 21 |
| `pendencia_admin` | 6 |
| `recusa_motivo` | 12 |
| `pode_fechar`, incluindo derivações concorrentes | 11 |
| Total | 128 |

Nas tabelas, **m** é atribuição em memória; **P** é persistência associada; **W** é inserção/update ou delegação de escrita. A restauração em memória e o proxy de ressuscitação estão separados das escritas persistentes. (`C/models.py:1507`, `:1508`, `:3152`, `:3159`, `:3264`.)

O levantamento de **`respondida_em`** é:

| Arquivo / fronteira | Escritor ou memória | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; ressuscitação W1566; lote W3266; carimbo m3132/P3152; **só memória** m1508,3163 | 1564,1844,3045,3048,3127,3131,3348,3498,3924,3931,3932,3933 |
| `S/disputa_emissao.py` — DENTRO | resposta m1468/P1508; correção W1630,1632; automática m1653/P1655; trilha m1674/P1676 | 1619,1670,1712,1856,1929,1946 |
| `S/acoes_disputa.py` — DENTRO | — | 95,130 |
| `S/becos.py` — DENTRO | — | 30 |
| `S/cobranca.py` — DENTRO | — | 228 |
| `S/declaracao_texto.py` — DENTRO | — | 417,419,687 |
| `S/fio.py` — DENTRO | — | 340,660 |
| `S/furo_sem_canal.py` — DENTRO | — | 80 |
| `S/historico.py` — DENTRO | — | `getattr`105 |
| `S/materializacao.py` — DENTRO | — | 669,1369 |
| `S/painel.py` — DENTRO | — | 67 |
| `S/passivo_validado.py` — DENTRO | — | snapshot dinâmico83 |
| `S/pergunta_no_app.py` — DENTRO | — | 71 |
| `S/raiox.py` — DENTRO | — | 138 |
| `S/validacao.py` — DENTRO | — | 393,576,581,924 |
| `S/verbos.py` — DENTRO | — | 309,312 |
| `S/vigia_fio.py` — DENTRO | — | 675,783 |
| `app/api/views.py` — FORA | — | 1172,1184,2568,2582,2625,2642 |
| `app/api/views_mensageria.py` — FORA | — | 569,570 |
| `C/juizes.py` — FORA | — | 829,1341,1449,1594,1601,1604,1949 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 177,592,1105,1572,1602 |
| `Q/backfill_decisoes_pergunta.py` — FORA | — | 56,60 |
| `Q/curar_perguntas_em_limbo.py` — FORA | — | 46,69 |
| `C/relatorios_disputa.py` — FORA | — | 21 |
| `C/veredito_lavrado.py` — FORA | — | 206,208 |
| `C/views.py` — FORA | — | 1280,1416 |
| `C/views_cobrar.py` — FORA | — | 169,175 |
| `P/management/commands/reconciliar_grade.py` — FORA | — | 460 |
| `P/supra_juiz.py` — FORA | — | 545,548 |
| `H/_card_atendimento.html` — FORA | — | 94,100,137,155,384,385 |
| `H/_pergunta_card.html` — FORA | — | 24,29,32 |
| `H/modal_fio.html` — FORA | — | 185,217,220 |
| `bin/censo_folha.py` — FORA | — | filtros28,29 |

O levantamento de **`validada_em`** é:

| Arquivo / fronteira | Escritor ou memória | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; ressuscitação W1566; lote W3266; carimbo m3135/P3152; **só memória** m1507,3164 | 309,344,1564,1843,3041,3127,3134,3359,3924,3935,3936,3937 |
| `S/disputa_emissao.py` — DENTRO | resposta nova m1480/P1508; correção W1630,1632 | 1470,1471,1620,1917 |
| `S/validacao.py` — DENTRO | `_carimbar_validacao` m134/P137; devolução m199/P202 | 139,147,188,195,345,396,472,573,732,753 |
| `S/acoes_disputa.py` — DENTRO | — | 345 |
| `S/declaracao_texto.py` — DENTRO | — | 227 |
| `S/historico.py` — DENTRO | — | `getattr`105 |
| `S/materializacao.py` — DENTRO | — | 670,1369 |
| `S/painel.py` — DENTRO | — | 67,270,271,272,273 |
| `S/passivo_validado.py` — DENTRO | — | snapshot83 |
| `S/verbos.py` — DENTRO | — | 239,246 |
| `S/vigia_carimbo.py` — DENTRO | — | 97 |
| `C/juizes.py` — FORA | — | 829,1325,1949 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 205,1075,1572,1602,1765,1778 |
| `Q/backfill_decisoes_pergunta.py` — FORA | — | 56,60,100 |
| `Q/encerrar_perguntas_com_fato.py` — FORA | — | 31 |
| `Q/reabrir_orfas_enterradas.py` — FORA | — | 67 |
| `C/reconciliador.py` — FORA | — | 627,1048 |
| `P/services/flip_auto.py` — FORA | — | 128 |
| `P/services/procedencia_batida.py` — FORA | — | 74 |
| `H/_pergunta_card.html` — FORA | — | 20,23,42 |
| `H/modal_fio.html` — FORA | — | 228 |
| `bin/censo_folha.py` — FORA | — | 29 |

O levantamento de **`materializada_em`** é:

| Arquivo / fronteira | Escritor ou memória | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; `_escrever_carimbo` m3099; portas P3152,3190,3212; lote W3264 | 1483,1843,1925,3043,3046,3056,3126,3182,3183,3284,3296,3313,3350,3352 |
| `S/disputa_emissao.py` — DENTRO | — | 226,762,1070,1089,1323,1621,1823,1917 |
| `S/fabrica_da_celula.py` — DENTRO | — | 170 |
| `S/historico.py` — DENTRO | — | `getattr`105 |
| `S/materializacao.py` — DENTRO | — | 434,532,623,627,632,671,1089,1093,1369,1418 |
| `S/painel.py` — DENTRO | — | 67 |
| `S/passivo_validado.py` — DENTRO | — | snapshot83 |
| `S/raiox.py` — DENTRO | — | 135,175 |
| `S/relavra_via.py` — DENTRO | — | 181,244 |
| `S/validacao.py` — DENTRO | — | 188,190,461,472,490 |
| `C/juizes.py` — FORA | — | 288,662,689,831,1336,1362,1368,1399,1417,1949 |
| `Q/auditar_disputas.py` — FORA | — | 69,148 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 470,538,587,905,955,1075,1156,1572,1602,1765 |
| `Q/backfill_decisoes_pergunta.py` — FORA | — | 100,325 |
| `Q/encerrar_perguntas_com_fato.py` — FORA | — | 31 |
| `Q/migrar_data_evento_dia_do_turno.py` — FORA | — | 147 |
| `Q/reabrir_orfas_enterradas.py` — FORA | — | 67,88,91,129,142 |
| `C/reconciliador.py` — FORA | — | 366,627,1048,1144,1154,1500 |
| `C/veredito_lavrado.py` — FORA | — | 202,204 |
| `C/views.py` — FORA | — | 1257 |
| `P/management/commands/reconciliar_fantasmas.py` — FORA | — | 211 |
| `P/services/procedencia_batida.py` — FORA | — | 74 |
| `P/services/veredito_celula.py` — FORA | — | 195 |

O levantamento de **`via_resolucao`** é:

| Arquivo / fronteira | Escritor ou memória | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; `_escrever_carimbo` m3101; portas P3152,3190,3212; lote W3264; `encerrar_por_fato_no_espelho` m3275/P3276 | 1483,1920,2054,2060,2722,2755,3055,3126,3201,3264,3273,3284,3296,3313,3320,3350,3352,3485,3491 |
| `S/relavra_via.py` — DENTRO | **`relavrar` m125/P127** | 118,121,123,153,181,245 |
| `S/competencia_trancada.py` — DENTRO | — | 251 |
| `S/disputa_emissao.py` — DENTRO | — | 327,398,417,436,1927,1942,1945 |
| `S/explicador.py` — DENTRO | — | 95 |
| `S/fabrica_da_celula.py` — DENTRO | — | 148 |
| `S/fio.py` — DENTRO | — | 1099,1166,1243,1350,1431,1852,1888,1910,1954 |
| `S/materializacao.py` — DENTRO | — | 438,623,668,1089 |
| `S/passivo_validado.py` — DENTRO | — | 83,140,141,142,515 |
| `S/tarefa_do_dia.py` — DENTRO | — | 227,228 |
| `C/juizes.py` — FORA | — | 662,674,830,1336,1362 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 202,206,433,542,955,1710 |
| `Q/backfill_decisoes_pergunta.py` — FORA | — | 100,314,330,398,504 |
| `Q/encerrar_perguntas_com_fato.py` — FORA | — | 31,42,49,66,76 |
| `Q/migrar_data_evento_dia_do_turno.py` — FORA | — | 70,71,72 |
| `Q/reabrir_orfas_enterradas.py` — FORA | — | 67,129,142,160 |
| `Q/relavrar_via_i6.py` — FORA | — | 58,59,62,66,68,88 |
| `C/reconciliador.py` — FORA | — | 1143,1154 |
| `P/services/cartorio.py` — FORA | — | 1610 |
| `P/services/flip_auto.py` — FORA | — | 128 |
| `H/_card_atendimento.html` — FORA | — | 186 |

O levantamento de **`pendencia_admin`** é:

| Arquivo / fronteira | Escritor | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; default2556 | — |
| `S/materializacao.py` — DENTRO | `_materializar_resposta_hora` m850/P851 | — |
| `S/explicador.py` — DENTRO | — | 85,90 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 1112 |
| `H/_card_atendimento.html` — FORA | — | 183 |
| `H/modal_fio.html` — FORA | — | 222,225 |

O levantamento de **`recusa_motivo`** é:

| Arquivo / fronteira | Escritor | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | nascimento W3017; `carimbar_identidade` m3188/P3190 | — |
| `S/disputa_emissao.py` — DENTRO | contestação m1497/P1508 | — |
| `S/materializacao.py` — DENTRO | pendência m850/P851; recusa m1102/P1103 | refresh1089; comparação1092 |
| `S/validacao.py` — DENTRO | recusa admin m584/P590 | 147,188,192,208,225,228,582,604 |
| `S/explicador.py` — DENTRO | — | 90 |
| `S/passivo_validado.py` — DENTRO | — | snapshot83 |
| `Q/auditar_invariantes_chamados.py` — FORA | — | 535,1075,1112 |
| `P/services/flip_auto.py` — FORA | — | regex132 |
| `H/_card_atendimento.html` — FORA | — | 186 |
| `H/modal_fio.html` — FORA | — | 226 |
| `bin/sombra_mascara_pessoal.py` — FORA | `setattr`57; `bulk_update`62,64 | carregamento51; `getattr`54; comparação56 |
| `bin/sombra_selo_pessoal.py` — FORA | — | SQL77 |

A escrita externa da máscara é efetivamente da pergunta: o inventário inclui `PerguntaDisputa.recusa_motivo`; o programa exige banco `sombra`. Ela não deve ser confundida com escritor ordinário de produção, nem omitida do censo de formas de escrita. (`bin/sombra_regras_pessoais.py:85`; `bin/sombra_mascara_pessoal.py:10`, `:57`, `:62`.)

O levantamento de **`pode_fechar` e decisões concorrentes de fechamento** é:

| Arquivo / fronteira | Definição/derivação | Leitor |
|---|---|---|
| `C/models.py` — DENTRO | juiz `DisputaSupervisao.pode_fechar`1848,1853 | — |
| `S/explicador.py` — DENTRO | bool parcial137; publicação163 | uso local138,139 |
| `C/relatorios_disputa.py` — FORA | projeção concorrente `PRONTA_FECHAR`20 | C6 em18 |
| `S/becos.py` — DENTRO | — | 33,56 |
| `S/competencia_trancada.py` — DENTRO | — | 153,202 |
| `S/desenlace.py` — DENTRO | — | `dict.get`63 |
| `S/disputa_emissao.py` — DENTRO | — | 1278,1963 |
| `S/materializacao.py` — DENTRO | — | 585,1316 |
| `C/reconciliador.py` — FORA | — | 411,611,1170 |
| `P/services/entrada_sincronizada.py` — FORA | — | 58 |
| `H/modal_fio.html` — FORA | — | 435,446 |

As indireções relevantes para interpretar essas tabelas são:

| Forma | Resultado conferido |
|---|---|
| `update(**campos)` | `reabrir_para_correcao` monta resposta/validação nulas e texto vazio em1593; persiste diretamente em1632 ou entrega ao descarimbo em1630. (`S/disputa_emissao.py:1593`, `:1630`, `:1632`.) |
| `update(..., **outros)` | O descarimbo escreve materialização/autor/via e aceita campos adicionais; os chamadores atuais passam resposta/validação. A guarda veta apenas o trio do carimbo em `outros`. (`C/models.py:1566`, `:3255`, `:3266`; `S/disputa_emissao.py:1593`.) |
| `save(update_fields=lista)` | A resposta salva a lista montada em1508; carimbo salva em3152; identidade em3190; descarimbo em3212. A string na lista é mecanismo de persistência, não leitura do valor. (`S/disputa_emissao.py:1508`; `C/models.py:3152`, `:3190`, `:3212`.) |
| `bulk_update` próximo ao domínio | `gravar_lavras_em_lote` escreve somente `veredito_lavrado`, `julgado_em`, `janela_expira_em`; não escreve os seis campos acima. (`C/veredito_lavrado.py:237`, `:250`.) |
| `setattr` genérico | A porta de texto permite título/assunto/descrição; `core.reexibir` altera memória. Não são prova de escrita persistente desses estados. (`S/texto.py:18`, `:25`, `:33`; `app/core/reexibir.py:45`, `:49`.) |
| `F()` | O enumerador inclui `F` como leitor de campo; escrita dos alvos por `F()` não foi identificada nos sítios classificados. Ausência universal por expressões construídas: **[nao verificado]**. |
| SQL | A máscara/selo foram discriminados; o SQL do reconciliador aberto é advisory lock/unlock. SQL dinâmico arbitrário escrevendo os alvos: **[nao verificado]**. (`bin/sombra_selo_pessoal.py:77`; `C/reconciliador.py:1235`, `:1242`.) |
| Signals | `update_fields` é lido como metadado de disparo; o signal emite evento e agenda beco. Não é leitura do conteúdo de cada coluna. (`C/signals.py:227`, `:238`, `:255`, `:257`.) |
| Estado derivado | API serializa `p.estado`; explicador, fio e vigia também o consomem. São leitores indiretos dos campos/juízes já discriminados. (`app/api/views.py:2573`; `S/explicador.py:72`; `S/fio.py:614`; `S/vigia_fio.py:311`.) |

As migrations ficam fora das 128 linhas de runtime:

| Campo | Schema / leitura executável |
|---|---|
| resposta/validação | `C/migrations/0014_perguntadisputa.py:25`, `:26` |
| materialização | `C/migrations/0016_s84_pergunta_chamado_destino.py:23` |
| pendência | `C/migrations/0043_hx_pendencia_admin.py:15` |
| recusa | `C/migrations/0040_a27_recusa_motivo.py:15` |
| via | criação `C/migrations/0033_perguntadisputa_via_resolucao.py:15`; alterações em `0035_via_admin_decidiu.py:15`, `0036_a1_pergunta_celula.py:22`, `0037_a3_estado_honesto.py:31`, `0040_a27_recusa_motivo.py:20`, `0041_b2_via_legado_sem_lastro.py:15`, `0042_hx_via_ausencia_dia.py:15`, `0046_via_celula_absolveu.py:15`, `0048_via_competencia_trancada.py:14`, `0051_decisao_pergunta.py:110` |
| via, leitura de universo | `C/migrations/0053_pergunta_unica_por_familia.py:32` |

Completude contra reflexão, receivers de tipo desconhecido, SQL montado e todos os consumidores transitivos: **[nao verificado]**. O instrumento conserva uma fila de revisão dessas formas; não transforma ausência de literal em prova de ausência.

A matriz seguinte descreve **comportamento da cópia**, não a execução em produção. Os testes foram inspecionados; execução da suíte nesta sessão: **[nao verificado]**. “Sem teste” significa cobertura específica não identificada nesta leitura, não ausência global certificada. **[nao verificado quanto à cobertura global]** [nao verificado]

| ID / porta e entrada | Juiz consultado | Operação | Persistido | Retorno | Prova |
|---|---|---|---|---|---|
| **T01 responder**, entrada admissível | Guardas por tipo/opções; porta de descarimbo | Core de resposta | Texto, `respondida_em`, decisão RESPOSTA; eventual descarimbo | `(True,None)` | `S/disputa_emissao.py:1433`, `:1467`, `:1485`, `:1508`, `:1511`, `:1559`; `T/test_reresposta_invalida_validacao.py:67` |
| **T02 responder novamente**, tinha validação | Mesmas guardas | Invalida assinatura anterior | Nova resposta; validação/autor nulos; histórico conservado | Mesmo contrato | `S/disputa_emissao.py:1470`, `:1480`, `:1511`; `T/test_reresposta_invalida_validacao.py:55` |
| **T03 validar**, sem pergunta/resposta ou já validada; implausível sem confirmação | Filtro PK/DS; carimbos; `motivo_implausivel_validacao` | Recusa antes da escrita | Nada por esses ramos | `nao_encontrada`, `sem_resposta`, `ja_validada`, `implausivel` | `S/validacao.py:385`, `:393`, `:396`, `:404`; sem teste específico |
| **T04 validar hora admissível** | Implausibilidade; materializador individual | Valida **uma P**, antes do laço | Validação/autor e decisão VALIDAÇÃO; efeitos da unidade | `validada` ou `ja_no_espelho` | `S/validacao.py:134`, `:139`, `:435`, `:461`, `:463`, `:515`, `:517`; `T/test_o261_recusa_fala.py:135` |
| **T05 validar**, materialização recusada sem encerramento e com motivo | `_resposta_de_hora`; carimbo nulo; motivo | Devolve validação individual | Limpa validação/autor, mantém resposta, registra DEVOLUÇÃO; pode reabrir DS | `nao_gravou`, `devolvida=True`; repetição pode dar `None` interno | `S/validacao.py:188`, `:199`, `:205`, `:217`, `:473`, `:499`; `T/test_o261_recusa_fala.py:87`, `:148`, `:164` |
| **T06 validar**, recusa já encerrou identidade | Devolução lê materialização | Não desfaz validação | Encerramento permanece | Devolução `False`; resultado final depende dos erros | `S/validacao.py:190`, `:490`; `T/test_o261_recusa_fala.py:190` |
| **T07 materializar lote normal** | Fila por timestamps; unidade; C7 na cauda | Itera **uma P por vez** | Efeitos individuais; eventual fecho da DS | Dict de materializadas, reusos, pendentes, fecho e erros | `S/materializacao.py:1369`, `:1375`, `:1378`, `:1381`; `T/test_o317_passivo_pela_porta.py:188`, `:481` |
| **T08 materializar**, dias divergem sem conferência | `fio.data_do_fato_de` | Recusa nomeada | `recusa_motivo`; nenhuma batida desse ramo | `False` | `S/materializacao.py:1191`, `:1193`, `:1213`, `:1214`; sem teste específico |
| **T09 hora sem texto humano** | Guarda de anuência | Recusa antes de derivar horário | Motivo na P/fio | Ramo `None`; unidade `False` | `S/materializacao.py:829`, `:836`, `:841`, `:1227`; sem teste específico aberto |
| **T10 tipo não conferido** | `entrada_de_marco`, grade/célula | Usa tipo disponível pelo motivo e registra pendência | `pendencia_admin` + motivo; pode prosseguir | Não é recusa obrigatória | `S/materializacao.py:386`, `:395`, `:397`, `:850`, `:851`; sem teste específico |
| **T11 batida já no slot** | `_batida_existente_na_janela` | Reusa; anota origem; reconcilia | Pode alte

[truncado em 20000 de 49203 caracteres]

## 11/11 09:15 · sessao 5d9a4f9c90fa · FIO-E0-V4-E3 · construtor · origem=Code

Cura aplicada somente em `chamados/tests`; os arquivos alterados e novos estão registrados em [e3fix11_execucao.txt:132](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:132). [nao verificado]

O censo dos harnesses ficou assim:

| Executor | Como monta o namespace | Mesmo furo? |
|---|---|---|
| S2/S3 | Dicionário manual; `funcao()` compila somente a função solicitada, sem copiar globais. [helper:14](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e4_apoio.py:14) | Sim; corrigido carregando a constante e o leitor reais. [teste:147](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:147) | [nao verificado]
| `test_e3_nb_impressao_memoria` | Seleção fixa por AST, já incluindo função e constante. [teste:173](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:173) | Não. | [nao verificado]
| `x1_mutantes --sem-banco` | Reutiliza o harness anterior. [executor:77](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:77) | Não. |
| `x1_mutantes`, instalação ORM | Executa definições em `modulo.__dict__`, incluindo função e constante. [executor:100](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:100) | Não pelo código; execução ORM [nao verificado]. |
| `e3_pos_suite_sem_banco` | Invoca o próprio S2/S3; herdava o defeito. [executor:78](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3_pos_suite_sem_banco.py:78) | Corrigido pela mesma alteração. |

Os outros testes que selecionam `_julgar_colab_corpo` por AST apenas inspecionam sua estrutura: [test_o219:217](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_o219_versao_da_regra.py:217), [test_e3_isento:112](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_isento_na_impressao.py:112). [nao verificado]

Com `cobertura='aprovada'`, **a expectativa original permanece**: `pulados=1`, sem rejulgamento, preservando impressão e veredito anteriores. O caso usa 07/11/2026, dentro do horizonte; porém a cobertura integral exclui o marcador X1. A impressão está deliberadamente cega à cobertura e a chamada usa `apply_=False`. Portanto, nenhuma assertion precisou ser relaxada. [teste:127](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:127), [cartorio:1229](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1229), [teste:169](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:169). A cobertura visível continua tendo seu caso separado de rejulgamento para `concorde`. [memória:414](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:414) [nao verificado]

O diff da cura acrescenta estas linhas, preservando a assertion original:

```diff
+                ns['HORIZONTE_SEM_ADESAO'] = literais('ponto/services/cartorio.py')['HORIZONTE_SEM_ADESAO']
+                funcao('ponto/services/cartorio.py', 'sem_adesao_no_horizonte', ns)
+                self.assertIn('sem_adesao_no_horizonte', ns.keys(),
+                              'harness deve carregar o leitor real do cartorio')
+                self.assertIn('HORIZONTE_SEM_ADESAO', ns.keys(),
+                              'harness deve carregar a declaracao lida pelo leitor')
+                leitor = ns['sem_adesao_no_horizonte']
+                # Corte default 21: 20/10 ainda e comp10; o dia do caso (07/11) e comp11.
+                self.assertFalse(leitor(dt.date(2026, 10, 20)))
+                self.assertTrue(leitor(dia))
...
+                self.assertEqual(out['nunca_bateu'], 0)
+                self.assertEqual(cel.impressao, legado)
+                self.assertEqual(cel.veredito, 'nunca_bateu')
```

Fonte: [teste:147](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:147). O novo [executor de mutantes:18](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3_namespace_mutantes.py:18) remove cada dependência ou substitui o leitor por constantes, somente em memória. [nao verificado]

Cada link abaixo contém o **comando exato**, RC e resultado:

| Execução | RC | Resultado |
|---|---:|---|
| [Suíte solicitada](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:14) | 0 | **31 OK** | [nao verificado]
| [X1 green](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:19) | 0 | 24 OK |
| [X1 sem_horizonte_x1](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:25) | 1 | Assertion; ERROR=0 |
| [X1 horizonte_na_10](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:32) | 1 | Assertion; ERROR=0 |
| [X1 literal_incondicional](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:39) | 1 | Assertion; ERROR=0 |
| [X1 sem_fonte_no_gancho](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:46) | 1 | Assertion; ERROR=0 |
| [X1 so_acusador](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:53) | 1 | Assertion; ERROR=0 |
| [X1 parcial_tira_marcador](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:60) | 1 | Assertion; ERROR=0 |
| [RED sem_leitor](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:67) | 1 | Dois subcasos por assertion |
| [sem_declaracao](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:74) | 1 | Dois subcasos por assertion |
| [leitor_constante_true](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:81) | 1 | Dois subcasos por assertion |
| [leitor_constante_false](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:88) | 1 | Dois subcasos por assertion |
| [declaracao_na_10](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:95) | 1 | Dois subcasos por assertion |
| [Mutantes S2/S3 existentes](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:102) | 1 cada | Assertion; ERROR=0 |
| [Pós-suite e selos](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:114) | 0 | 7+1+15+35 OK | [nao verificado]
| [Ruff](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:121) | 127 | Indisponível |
| [Diff check](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3fix11_execucao.txt:129) | 0 | Sem apontamentos |

`status_aprovada_tira_marcador` exige ORM e não possui alvo sem banco. [x1_mutantes:122](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:122). Certificação Code/opus final: [nao verificado].

## 11/11 09:15 · sessao 0ca26e8461ed · FIO-E0-V4-E4C · censo · origem=Code

**CENSO-ANTES-DA-VAGA — FIO-E0-V4-E4C**

**Campos com retirada lógica prevista:** R = `resposta_colab`, `respondida_em`; V = `validada_em`, `validada_por`; C = `materializada_em`, `materializada_por`, `via_resolucao`; F = `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`. A retirada depende do ramo: as exceções abaixo continuam lendo projeção. (`app/chamados/models.py:3844`; `app/chamados/models.py:3126`; `app/chamados/models.py:1880`; `app/docs/PROMPTS.md:2034`)

**Exceções:** A03, reabertura do continente; A05, somente assinatura; A06, identidade/duplicata; A17, pendência técnica; A18, recusa técnica; A20, conferência de data. `pendencia_admin`, recusa técnica e `data_conferida_em/por` entram no inventário, mas permanecem fontes nesses ramos. `recusa_motivo` exige separação entre decisão humana, advertência e resultado técnico. (`app/docs/PROMPTS.md:2034`; `app/chamados/services/validacao.py:593`; `app/chamados/services/disputa_emissao.py:1497`; `app/chamados/services/materializacao.py:855`; `app/chamados/services/materializacao.py:1107`)

**Perguntas de lei identificadas: D04 e D05, abaixo.** Não atribuí automaticamente o zero do censo de escritores ao censo de leitores: a lei exige a regra para a nova resposta, não equivalência presumida com o código anterior. (`app/docs/LEIS.md:136`)

Entrega no corpo da resposta; nenhuma gravação de relatório, teste, importação da aplicação, banco ou log produtivo realizada. Evidência persistida dessa execução: **[nao verificado]**. Identidade Git do hash informado, pai da E3 e diff entre eles: **[nao verificado]** — `git log` nesta raiz retornou “not a git repository”. [nao verificado]

O script ao fim foi executado por `python3 -B`, via stdin. O levantamento combina AST Python, busca sintática de templates e candidatos JS; a revisão abaixo elimina homônimos e entradas de formulário. Reflexão irrestrita, SQL montado dinamicamente, serialização implícita e consumidores externos: **[nao verificado]**.

**Autoridades e códigos usados nas tabelas**

| Código | Pergunta e destino existente | Registro |
|---|---|---|
| **T** | Ação exigível, dono e porta: `tarefa_do_dia` | `app/chamados/services/tarefa_do_dia.py:562`; `app/core/juizes.py:291`; `app/docs/JUIZES.md:78` |
| **P** | Pergunta viva: `pergunta_viva` | `app/chamados/juizes.py:862`; `app/core/juizes.py:278` |
| **FI** | Alguma pergunta viva na coleção: `fio_vivo` | `app/chamados/juizes.py:1488`; `app/core/juizes.py:287` |
| **FE** | Disputa pode fechar: `DisputaSupervisao.pode_fechar` | `app/chamados/models.py:1848`; `app/core/juizes.py:279` |
| **E** | Efeito confirmado: juiz `_pergunta_materializada_de_fato`, pela lâmpada vigente consumida por T | `app/chamados/juizes.py:662`; `app/chamados/services/tarefa_do_dia.py:119`; `app/docs/JUIZES.md:81` |
| **TR** | Dia trancado: `dia_da_pergunta_trancado` / `dias_trancados` | `app/chamados/juizes.py:974`; `app/chamados/juizes.py:1004`; `app/core/juizes.py:292` |
| **D01** | Texto, instante, assinatura e motivo da decisão: sequência DP; juiz específico de apresentação não registrado | `app/chamados/models.py:3840`; `app/core/juizes.py:272` |
| **D02** | Estado histórico do continente: falta autoridade factual equivalente demonstrada; preservar A03 | `app/chamados/models.py:2126`; `app/docs/PROMPTS.md:2034` |
| **D03** | História/procedência do processamento: DP e efeitos, sem confundir carimbo com efeito | `app/chamados/models.py:3840`; `app/chamados/services/tarefa_do_dia.py:141` |
| **D04** | Indicadores de atividade: decisão sem juiz específico confirmado | `app/chamados/services/verbos.py:239`; `app/chamados/services/painel.py:268`; `app/core/juizes.py:272` |
| **D05** | Rótulos de fase publicados pela API/tela: equivalência com T não estabelecida | `app/chamados/models.py:3034`; `app/api/views.py:2555` |
| **D06** | Invalidação de cache/ETag: dependência técnica de decisões **e efeitos** | `app/chamados/services/painel.py:58`; `app/chamados/services/painel.py:67`; `app/chamados/veredito_lavrado.py:151` |

Apontar para um juiz existente ainda não elimina a dependência: P recebe R/V/C pela projeção temporal; FE usa `respostas_sem_veredito`; E ainda consulta C/via; T consulta projeção para detectar passivo e conserva via de identidade. Esses corpos também precisam entrar na migração, respeitando as exceções. (`app/chamados/juizes.py:1369`; `app/chamados/juizes.py:1384`; `app/chamados/models.py:1853`; `app/chamados/juizes.py:679`; `app/chamados/services/tarefa_do_dia.py:398`; `app/chamados/services/tarefa_do_dia.py:230`)

**Inventário — modelos e juízes**

Nas tabelas, `def → leitura` separa a definição da ocorrência consumida. Os caminhos abreviados são: **M** = `app/chamados/models.py`; **J** = `app/chamados/juizes.py`; **S/** = `app/chamados/services/`. Cada função nomeada constitui uma unidade do censo.

| Leitor | Campos / resposta atual | Destino |
|---|---|---|
| M:128 `ChamadoColaborador.transicionar` → M:190 | F; disputas relacionadas à transição | T/FE; continente D02 |
| M:201 `_gravar_estado` → M:310 | V; perguntas atingidas pela reconciliação | T + D01 |
| M:336 `_ancoradas` → M:344 | V; perguntas ancoradas pertinentes | P/T |
| M:1468 `_universo_da_ressuscitacao` → M:1484 | C/R/via; universo que pode voltar | P/E; identidade A06 |
| M:1512 `ressuscitar_perguntas` → M:1563, M:1564, M:1574 | R/V/F; conservação e seleção da volta | D01/D03/P; F=D02 |
| M:1823 `DisputaSupervisao.aberta` → M:1824 | F; continente aberto | D02; dependência A03 |
| M:1826 `respostas_sem_veredito` → M:1843 | R/V/C; impedimento de fechamento | FE, cujo corpo precisa migrar |
| M:1848 `pode_fechar` → M:1853 | Leitura indireta de R/V/C | Permanece FE, sem segundo juiz |
| M:1855 `fechar` → M:1879, M:1920 | F/R/C/via; idempotência e quitação | FE/E/D01; história D02/D03 |
| M:1971 `remover_motivo` → M:2031, M:2055 | F/R/via; retratação | DP `RETRATACAO`; A06 na identidade |
| M:2126 `reabrir_admin` → M:2138 | F; necessidade da reabertura | **A03 permanece** |
| M:2163 `reabrir_sistema` → M:2167 | F; religar continente | T para exigibilidade; D02 para continente |
| M:2182 `reabrir_por_colab` → M:2224 | F; gate de correção | T/D01; gesto do continente D02/A03 |
| M:2250 `limpar_resolucao_residual` → M:2262 | F/resolução; limpeza residual | D02/D03; retirada coordenada com E4d |
| M:2276 `DisputaSupervisao.__str__` → M:2277 | `aberta` indireta | D02 |
| M:2629 `PerguntaDisputa.trava_na_ordem` → M:2673 | F em Q/`values_list`; universo das travas | D02; não substituir ordem de trava por T |
| M:2697 `do_fato` → M:2724 | via; identidade | **A06 permanece** |
| M:2746 `q_dos_fatos` → M:2755 | via em Q; identidade | **A06 permanece** |
| M:2825 `_nascer` → M:3019 | R/V indiretos na conservação legada | D01/D03; cruza E4b |
| M:3034 `estado` → M:3041 | R/V/C; fase exibida | **D05** |
| M:3050 `fechar_por_admin` → M:3055 | C/via; idempotência | E/DP; identidade A06 |
| M:3103 `carimbar` → M:3126 | C/R/V/autores; foto e assinaturas | E/D03; **A05 somente assinatura** |
| M:3168 `carimbar_identidade` → M:3182 | C/autor; preservar carimbo | **A06 permanece** |
| M:3193 `fora_do_fato` → M:3201 | via; terminalidade da identidade | **A06 permanece** |
| M:3203 `descarimbar` → M:3206 | `fora_do_fato` indireto | **A06 no gate** |
| M:3252 `descarimbar_em_lote` → M:3265 | via; excluir identidade terminal | **A06 no gate** |
| M:3268 `encerrar_por_fato_no_espelho` → M:3273 | via/`fora_do_fato`; idempotência | E; identidade A06 |
| M:3279 `encerrar_fantasma` → M:3284 | C/via; idempotência | E/D03 |
| M:3289 `reabrir_fantasma` → M:3296 | C/via; reversibilidade | E/D03 |
| M:3301 `reabrir_por_resposta` → M:3313 | C/via; reversibilidade | DP/E |
| M:3317 `reabrir_por_competencia` → M:3320 | via; reversibilidade | TR/DP |
| M:3326 `q_vivas` → M:3348, M:3359 | R/V/C/via em Q; pré-filtro | P, conservando superset; A06 |
| M:3497 `respondida` → M:3498 | R; presença do carimbo | D01; ação por T |
| M:3519 `resposta_display` → M:3521 | R; conteúdo discriminado | D01; preservar tipo/opções |
| M:3914 `DecisaoPergunta.linhas_da_projecao_sem_linha` → M:3924 | R/V/autoria; detectar legado sem ato | **Conservação E4b**, não substituir por “nenhuma decisão” |

As condições dos índices em `PerguntaDisputa.Meta` também dependem de via; são dependências de schema, não funções adicionais no total de leitores. Permanecem no ramo A06. (`app/chamados/models.py:3485`; `app/chamados/models.py:3491`; `app/docs/PROMPTS.md:2034`)

| Leitor | Campos / pergunta respondida | Destino |
|---|---|---|
| J:282 `_retratada_com_trilha` → J:288 | C; ordem temporal da retratação | DP/D03; A06 |
| J:341 `_slot_esperado` → J:348, J:360 | R; slot esperado | D01, mantendo interpretação do juiz |
| J:662 `_pergunta_materializada_de_fato` → J:679, J:707 | C/via; efeito confirmado | Permanece E; retirar C derivado, preservar exceções |
| J:821 `_chave_memo_pv` → J:852 | R/V/C/via; chave da memoização | D06; A06 na identidade |
| J:1314 `_resposta_concorda_com_a_luz` → J:1340 | R; concordância com o fato | D01; mantém julgamento existente |
| J:1362 `projecao_temporal_da_pergunta` → J:1369 | R/V/C/via; valores no horizonte | DP por `ato_em` + E; A06 |
| J:1383 `_pergunta_viva_calc` → J:1384 | Consome a projeção temporal | P, migrando seu insumo |
| J:1476 `pergunta_sem_lastro_ou_viva` → J:1480 | C; proteção de resposta regularizada | P/E |
| J:1509 `respondida_sem_texto` → J:1512 | R; carimbo sem conteúdo | D01; conservação D03 |
| J:1655 `perguntas_corrigiveis` → J:1657 | R; universo corrigível | T/D01 |
| J:1662 `perguntas_respondidas_a_validar` → J:1664 | R/`q_vivas`; fila de validação | T/P/D01 |
| J:1678 `declarou_hora` → J:1693 | R; declaração interpretável | D01 |
| J:1830 `delta_da_resposta` → J:1840 | R; distância do marco | D01; preservar régua existente |
| J:1936 `classificar_resposta_validacao` → J:1944 | R; classe da aprovação | D01; mantém juiz existente |
| J:1957 `motivo_implausivel_validacao` → J:1966 | R; motivo de implausibilidade | D01; mantém juiz existente |
| J:1980 `irmas_respondidas_sem_consumo` → J:2013 | R/V/C; declarações ainda não consumidas | DP/E |
| J:2371 `disputa_viva_do_chamado` → J:2379 | F; relação chamado/continente | D02; ação por T/FI |

**Inventário — serviços internos**

| Arquivo e leitor | Leitura / o que responde | Destino |
|---|---|---|
| S/`tarefa_do_dia.py:164` `_tarefa_pura` → :230 | via; exclusão da identidade | **A06 permanece** |
| S/`tarefa_do_dia.py:361` `CargaDaTarefa.__init__` → :398 | R/V indiretos; passivo sem decisão | D01/D03; **gate E4b** |
| S/`tarefa_do_dia.py:449` `_carregar_vida` → :491 | P indireto; exposição administrativa | P/T, incluindo migração do insumo de P |
| S/`pergunta_no_app.py:45` `perguntas_do_app` → :50 | `q_vivas`; coleção ampla do app | P |
| S/`pergunta_no_app.py:61` `consulta_do_app` → :71 | R/F; pendentes versus histórico | T/D01; D02 no histórico do continente |
| S/`cobranca.py:221` `_calados` → :229 | R/F/`q_vivas`; quem ainda não respondeu | T/P/D01 |
| S/`janela_colab.py:49` `chamados_visiveis_colab` → :63 | `q_vivas`; visibilidade | P/T |
| S/`declaracao_texto.py:216` `_resposta_validada` → :227 | V; preservar aprovação | D01 |
| S/`declaracao_texto.py:306` `declarar_hora_colab` → :417 | R; preservar voz/instante | D01 |
| S/`declaracao_texto.py:641` `aplicar_declaracao` → :687 | R; resposta já entregue | D01/T |
| S/`validacao.py:127` `_carimbar_validacao` → :139 | R/V; conteúdo e instante do ato | D01; leitura dentro de escritor existente |
| S/`validacao.py:144` `_foto_da_validacao` → :147 | V/autor/recusa/F; fotografia | D01/D03/D02; A18 técnico |
| S/`validacao.py:163` `devolver_validacao_recusada` → :188 | R/V/C/recusa/F; devolução | DP `DEVOLUCAO`/E; A18 |
| S/`validacao.py:234` `_e_aceite_de_folga` → :236 | R; aceite de folga | D01 |
| S/`validacao.py:239` `_aceitar_a_folga` → :345 | V/autor por `refresh_from_db`; restauração | D01/D03 |
| S/`validacao.py:373` `validar_pergunta` → :393, :461 | R/V/C; guardas e resultado | T/DP/E |
| S/`validacao.py:533` `recusar_pergunta` → :573, :580 | R/V/recusa; recusabilidade e foto | DP `RECUSA`; não inferir ato de string técnica |
| S/`validacao.py:625` `conferir_data_do_fato` → :662 | conferência; idempotência | **A20 permanece** |
| S/`validacao.py:695` `validar_lote_classe_a` → :732 | V; evitar aprovação repetida | T/D01 |
| S/`validacao.py:845` `classe_do_lote` → :851 | R; classe do lote | D01 |
| S/`validacao.py:912` `fila_validar_classificada` → :924 | R; entrada na fila | T/D01 |
| S/`materializacao.py:245` `_tipo_pelo_marco` → :279 | R; tipo do plantio | D01 |
| S/`materializacao.py:332` `_ts_tipo_do_plantio` → :358 | R; instante e escape | D01; mantém ajuste temporal existente |
| S/`materializacao.py:430` `estado_aplicacao_pergunta` → :437 | R/C/via; aplicação/idempotência | DP/E; A06/A18 por ramo |
| S/`materializacao.py:467` `_declarar_ausencia_de_pergunta` → :473 | R; ausência declarada | DP + efeito Ausência |
| S/`materializacao.py:501` `_materializar_ausencia_de_resposta` → :538, :570, :590 | R/C/F; absorção | DP `ABSORCAO`/E/FE; D02 |
| S/`materializacao.py:608` `perguntas_absorvidas_por` → :630 | R/C/via; vínculo com ausência | DP `ABSORCAO` + Ausência |
| S/`materializacao.py:644` `devolver_absorvidas_da_ausencia_rejeitada` → :673 | R/V/C/autores; fotografia da volta | DP/D03 + estado da Ausência |
| S/`materializacao.py:789` `_materializar_resposta_hora` → :834 | R; declaração aplicada | D01; **A17 no resultado técnico** | [nao verificado]
| S/`materializacao.py:983` `_materializar_resposta_data_hora` → :1000 | R; interpretação textual | D01 |
| S/`materializacao.py:1088` `_carimbar_recusa` → :1094 | C/via/recusa; idempotência técnica | **A18/A06 permanecem** |
| S/`materializacao.py:1163` `_materializar_uma` → :1198 | conferência; autorização diante de dois dias | **A20 permanece** |
| S/`materializacao.py:1307` `_fechar_disputa_se_quitada` → :1321 | F/resolução; fechamento existente | FE; história D02 |
| S/`materializacao.py:1343` `materializar_perguntas_validadas_da_disputa` → :1375 | R/V/C; fila de execução | T/DP/E |
| S/`materializacao.py:1395` `materializar_pendentes_aprovadas` → :1424 | C; seleção pendente | T/E |
| S/`disputa_emissao.py:34` `colaborador_em_disputa` → :41 | F; continente aberto | D02; exigibilidade por T |
| S/`disputa_emissao.py:166` `_derivar_chamado_destino` → :228 | C; destino pertinente | P/E |
| S/`disputa_emissao.py:300` `gerar_perguntas_disputa` → :328 | via; dedup | **A06 permanece** |
| S/`disputa_emissao.py:474` `_abrir_ou_agregar_disputa` → :532 | F; continente reutilizável | D02/A03 |
| S/`disputa_emissao.py:747` `motivo_fora_da_fabrica` → :762 | C; causa de retenção | P/E |
| S/`disputa_emissao.py:945` `fecho_do_repasse` → :978 | F em Q; universo costurado | D02 |
| S/`disputa_emissao.py:1027` `criar_disputa_a_partir_de_chamados_orfaos` → :1070, :1097 | C/F; reuso | E/P/FE; D02 |
| S/`disputa_emissao.py:1302` `criar_disputa_ancora_de_chamado` → :1326 | C/F; âncora reutilizável | E/P; D02 |
| S/`disputa_emissao.py:1425` `responder_pergunta_disputa_core` → :1470, :1522 | V/autor/F; invalidação anterior e contexto | DP/D03; D02 |
| S/`disputa_emissao.py:1562` `reabrir_para_correcao` → :1619 | R/V/C/autores em `values_list`; foto anterior | DP/D03 |
| S/`disputa_emissao.py:1638` `responder_automaticamente` → :1647 | R; preservar texto existente | D01 |
| S/`disputa_emissao.py:1661` `carimbar_resposta_pela_trilha` → :1670 | R; texto sem instante | D01/D03, instante testemunhado |
| S/`disputa_emissao.py:1690` `recobrar_questionario` → :1713 | R; resposta já entregue | T/D01 |
| S/`disputa_emissao.py:1801` `_resolver_perguntas_por_chamado_resolvido` → :1825, :1856, :1927 | R/V/C/F/via; consequências do chamado | DP/P/E/FE; A06 e D02 por ramo |
| S/`fio.py:120` `_disputa_do_chamado` → :131 | F; continente apresentado | Autoridade já citada em `app/docs/JUIZES.md:40`; D02 no insumo |
| S/`fio.py:163` `montar_chamados_fio` → :187, :224, :340 | F/R/`aberta`/`q_vivas`; apresentação | P/T/D01; D02 e D05 |
| S/`fio.py:485` `processar_post_fio` → :600, :633, :660 | F/R; ramo do POST | T/D01; D02 no gesto do continente |
| S/`fio.py:1084` `reuso_sem_rechave` → :1100 | via; reuso | **A06 permanece** |
| S/`fio.py:1152` `sem_dia_na_saida` → :1167 | via; invariante de identidade | **A06 permanece** |
| S/`fio.py:1225` `canonica_do_grupo` → :1232 | R; escolha canônica | D01 + **A06** |
| S/`fio.py:1236` `merito_perdedor` → :1243 | R/via; mérito da canônica | D01 + **A06** |
| S/`fio.py:1345` `retratar_duplicata` → :1350 | via; duplicata já retratada | **A06 permanece** |
| S/`fio.py:1422` `_no_fato` → :1433 | via; universo de identidade | **A06 permanece** |
| S/`fio.py:1840` `fecho_cruza_dias` → :1853 | via; fecho da identidade | **A06 permanece** |
| S/`fio.py:1878` `perguntas_sobre_batida_retratada` → :1889 | via; diagnóstico | **A06 permanece** |
| S/`fio.py:1905` `universo_do_grupo` → :1911 | via; universo | **A06 permanece** |
| S/`fio.py:1923` `grupos_de_colisao` → :1956 | via; colisões | **A06 permanece** |
| S/`fabrica_da_celula.py:92` `motivos_a_perguntar` → :149 | via; dedup | **A06 permanece** |
| S/`fabrica_da_celula.py:160` `perguntar` → :171 | C; pergunta reaproveitável | P/E |
| S/`explicador.py:25` `_explicar_validada` → :32 | R; explicação | D01/E |
| S/`explicador.py:68` `explicar_pergunta` → :72, :85, :90, :95 | fase/R/pendência/recusa/via | T/E/D05; **A17/A18/A06** |
| S/`explicador.py:108` `por_que_nao_fecha` → :137 | `fio_vivo`; “pode fechar” | **FE**, não apenas FI |
| S/`raiox.py:51` `creditos_perguntas` → :135 | C/R em SQL/tuplas; crédito do painel | P/E/T/D01 |
| S/`raiox.py:153` `pergunta_sem_acao_possivel` → :175 | C; clique morto | E/T |
| S/`relavra_via.py:106` `relavrar` → :118 | via; necessidade da relavra | E/L; A06 fora da derivação |
| S/`relavra_via.py:142` `relavrar_dia` → :155 | via; seleção da relavra | E/L |
| S/`relavra_via.py:171` `q_carimbo_desfazivel` → :181 | C/via/F; seleção reversível | P/E; A06 e D02 |
| S/`relavra_via.py:187` `desfazer_carimbo_sem_lastro` → :244 | C/via/F; diagnóstico/foto | P/E/D03; A06 |
| S/`passivo_validado.py:131` `_balde_da_recusa` → :140 | via; classificação do passivo | E/T; **A18/A06** |
| S/`passivo_validado.py:474` `_plantar_o_dia` → :515 | via; aplicação do passivo | E/T; **A18/A06** |
| S/`historico.py:69` `_eventos_dos_chamados` → :98, :105 | `getattr(p,campo)` de R/V/C; linha do tempo | **D03**, DP e efeitos, não só último ato |
| S/`painel.py:50` `marcador_do_placar` → :67 | `Max(R/C/V)`; invalidação | **D06** |
| S/`painel.py:198` `montar_contexto_painel` → :262 | autor de V; indicador | **D04** |
| S/`painel.py:268` `_jan_val` → :270 | V por janela; atividade | **D04** |
| S/`verbos.py:218` `placar_equipe` → :239 | V/autor; ações por pessoa | **D04** |
| S/`verbos.py:253` `placar_aceite` → :311, :318 | R por `values`/dict; aceite após cobrança | **D04**, conservando definição escrita de aceite |
| S/`fila_por_causa.py:214` `_gaveta_de` → :225 | F e respostas sem veredito; gaveta | T/FE; D02 |
| S/`acoes_disputa.py:90` `acao_reabrir_disputa` → :95 | R; perguntas alcançadas | T/D01 |
| S/`acoes_disputa.py:118` `acao_regularizar_disputa` → :130 | R; regularização | T/D01/FE |
| S/`acoes_disputa.py:235` `acao_reabrir_disputa_admin` → :255 | F; escolher continente | **A03/D02** |
| S/`acoes_disputa.py:302` `acao_corrigir_resposta` → :345 | V/F/R; correção | T/D01; D02 |
| S/`acoes_chamado.py:327` `montar_contexto_detalhe` → :336 | F; detalhe | D02/T |
| S/`becos.py:25` `classificar` → :27 | F/R; resposta presa em fechada | T/FE/D01; D02 |
| S/`becos.py:36` `fechar_beco` → :56 | F e `pode_fechar` | FE; D02 |
| S/`competencia_trancada.py:65` `medir` → :113, :151 | F e respostas sem veredito | TR/FE; D02 |
| S/`competencia_trancada.py:163` `encerrar` → :196 | F; disputas alcançadas | TR/FE; D02 |
| S/`competencia_trancada.py:235` `reabrir` → :251 | via; reversibilidade | TR/DP; A06 por ram

[truncado em 20000 de 48601 caracteres]

## 11/11 09:08 · sessao f1ecbbc31623 · FIO-E0-V4-E4B · construtor · origem=Code

1. **Quem mascara a remoção é `_Desfaz`, provocando rollback do `atomic` inteiro** quando todos os dias são recusados: [backfill_fio_morto_e4b.py:349](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/management/commands/backfill_fio_morto_e4b.py:349).

2. **O lote misto precisa do rollback individual.** A recusa segue para o próximo dia; havendo um aceito, não ocorre `_Desfaz`: [backfill_fio_morto_e4b.py:327](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/management/commands/backfill_fio_morto_e4b.py:327). Acrescentei o caso ORM exigindo vizinho persistido, P90/D90/Ch12 restaurados, foto integral restaurada entre tentativas e desaparecimento dos nascimentos recusados: [test_e4b_c4_orm.py:404](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:404). Cascata natural em produção: [nao verificado].

3. **Não se aplica substituir um rollback redundante.** Mantive o enxerto `sem_rollback` e apontei-o ao novo caso: [e4b_c4_verificar.py:37](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_verificar.py:37). A morte no ORM continua [nao verificado].

Entrega: [diff completo](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10.patch:1) e [comandos/resultados](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:1).

| Verificação | rc | Resultado |
|---|---:|---|
| Memória | 0 | [88 OK](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:6) |
| REDs registrados | 0 | [40 OK](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:10) |
| `--selos` | 0 | [40 OK](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:14) |
| `--orm-preflight` | 0 | [8 enxertos válidos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:18) |
| `sem_rollback`, memória | 1 | [Uma falha na assertion-alvo](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:28) |
| `rollback_roteiro_antigo` | 1 | [Assertion prevista](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:38) |
| `rollback_falha_aceita` | 1 | [Cinco subtests falham, sem erros](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:44) |

Para a sessão, os comandos exatos de **GREEN isolado**, **mutante isolado** e **roteiro ORM completo** estão no [registro:53](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4v10_resultados.txt:53). Certificação ORM e opus final: [nao verificado].

## 11/11 09:08 · sessao 4aeaf4c941a5 · - · - · origem=Ronald (tmux)

Censo externo executado por AST via `python3 -B` em stdin; sem import Django, acesso ao banco ou suíte. Prova por arquivo da execução do instrumento: [nao verificado]. O recorte abaixo exclui `app/chamados/models.py`, `app/chamados/services/`, testes e migrations; atributos, argumentos ORM e strings de `getattr`/`values`/`only` foram capturados. Não prova completude contra reflexão/SQL construído. [nao verificado]

**Contagens externas do AST**, por `(arquivo, linha, forma)`:

| Campo | Ocorrências | Linhas distintas |
|---|---:|---:|
| `respondida_em` | 35 | 33 |
| `validada_em` | 19 | 18 |
| `materializada_em` | 42 | 42 |
| `via_resolucao` | 38 | 38 |
| `pendencia_admin` | 1 | 1 |
| `recusa_motivo` | 4 | 4 |
| `pode_fechar` | 4 | 4 |

Essas contagens excluem os complementos dinâmicos/templates abaixo e precisam ser apresentadas com essa unidade; sua execução não tem arquivo de log por restrição de só leitura. [nao verificado por arquivo:linha]

**Leitores externos — referências completas do estrato direto.** `R` inclui atributo/getattr, `F` seleção ORM e `V` projeção/carregamento. Cada número pertence ao arquivo da primeira coluna.

| Arquivo | Campo e linhas |
|---|---|
| `app/api/views.py` | `respondida_em` R:1172,1184,2568,2582,2625,2642 |
| `app/api/views_mensageria.py` | `respondida_em` R:569,570 |
| `app/chamados/juizes.py` | `respondida_em` R:829,1341,1449; F:1594,1601,1949; ordenação:1601 |
| `app/chamados/juizes.py` | `validada_em` R:829,1325; F:1949 |
| `app/chamados/juizes.py` | `materializada_em` R:288,662,689,831,1336,1362,1368,1399,1417; F:1949 |
| `app/chamados/juizes.py` | `via_resolucao` R:662,674,830,1336,1362 |
| `app/chamados/management/commands/auditar_disputas.py` | `materializada_em` F:69,148 |
| `app/chamados/management/commands/auditar_invariantes_chamados.py` | `respondida_em` R:177,592; F:1105,1572,1602 |
| Mesmo arquivo | `validada_em` R:205; F:1075,1572,1602,1765,1778 |
| Mesmo arquivo | `materializada_em` V:469; R:538,905; F:587,955,1075,1156,1572,1602,1765 |
| Mesmo arquivo | `via_resolucao` R:202,206,433,542,1710; F:955 |
| Mesmo arquivo | `recusa_motivo` F:535,1075,1112; `pendencia_admin` exclude:1112 |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py` | `respondida_em` Q/only:56; `validada_em` Q/only:56,F:100; `materializada_em` F:100,R:325; `via_resolucao` F:100,R:314,330,V:398,504 |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py` | `respondida_em` F:46,69 |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py` | `materializada_em`, `validada_em`, `via_resolucao` F:31; `via_resolucao` R:42,49,66,76 |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py` | `via_resolucao` R:70,71,72; `materializada_em` R:147 |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py` | `validada_em` F:67; `materializada_em` F:67,R:88,91,129,142; `via_resolucao` F:67,R:129,142,160 |
| `app/chamados/management/commands/relavrar_via_i6.py` | `via_resolucao` R:58,59,62,66,68,88 |
| `app/chamados/reconciliador.py` | `materializada_em` R:366,1144,1154,F:627,1048,1500; `validada_em` F:627,1048; `via_resolucao` R:1143,1154; `pode_fechar()` chamadas:411,611,1170 |
| `app/chamados/relatorios_disputa.py` | `respondida_em` R:21 |
| `app/chamados/veredito_lavrado.py` | `materializada_em` F:202; `respondida_em` F:206 |
| `app/chamados/views.py` | `materializada_em` R:1257; `respondida_em` R:1280,F:1416 |
| `app/chamados/views_cobrar.py` | `respondida_em` R:169,F:175 |
| `app/ponto/management/commands/reconciliar_fantasmas.py` | `materializada_em` F:211 |
| `app/ponto/management/commands/reconciliar_grade.py` | `respondida_em` F:460 |
| `app/ponto/services/cartorio.py` | `via_resolucao` R:1610 |
| `app/ponto/services/entrada_sincronizada.py` | `pode_fechar()` chamada:58 |
| `app/ponto/services/flip_auto.py` | `via_resolucao`, `validada_em` F:128; `recusa_motivo` R:132 |
| `app/ponto/services/procedencia_batida.py` | `validada_em`, `materializada_em` R:74 |
| `app/ponto/services/veredito_celula.py` | `materializada_em` R:195 |
| `app/ponto/supra_juiz.py` | `respondida_em` R:545,548 |
| `bin/censo_folha.py` | `respondida_em` F:28,29; `validada_em` F:29 |

As linhas ORM indicam o início da chamada AST; argumentos multilinha podem estar nas linhas imediatamente seguintes. Exemplo conferido: `encerrar_perguntas_com_fato.py:31` inicia a seleção, com os filtros em :32 e :33.

**Escritor externo dinâmico obrigatório.**

| Campo | Escritor | Leitores/seleção | Escopo |
|---|---|---|---|
| `P.recusa_motivo` | `setattr(o,c,novo)` em `bin/sombra_mascara_pessoal.py:57`, persistido por `bulk_update` em :62 e :64 | `.only('pk',*campos)` :51; `getattr` :54; comparação :56 | `TEXTO` registra explicitamente `PerguntaDisputa.recusa_motivo` em `bin/sombra_regras_pessoais.py:85`; script exige banco `sombra` em `bin/sombra_mascara_pessoal.py:10` |
| `P.recusa_motivo` | — | SQL `coalesce`/regex em `bin/sombra_selo_pessoal.py:77` | Itera o mesmo inventário `R.TEXTO` em :74 |

Não encontrei atribuição literal externa a esses sete nomes no recorte AST. Ausência universal de escrita por `F()`, kwargs/reflexão/SQL construído: [nao verificado].

**Templates, separados do AST Python.**

| Arquivo | Campo | Linhas |
|---|---|---|
| `app/templates/chamados/partials/modal_fio.html` | `respondida_em` | 185,217,220 |
| Mesmo arquivo | `pendencia_admin` / display | 222,225 |
| Mesmo arquivo | `recusa_motivo` | 226 |
| Mesmo arquivo | `validada_em` | 228 |
| Mesmo arquivo | `chamado.explicacao.pode_fechar` | 435,446 |
| `app/templates/chamados/partials/_card_atendimento.html` | `respondida_em` | 94,100,137,155,384,385 |
| Mesmo arquivo | `pendencia_admin` | 183 |
| Mesmo arquivo | `via_resolucao`, display e fallback `recusa_motivo` | 186 |
| `app/templates/chamados/partials/_pergunta_card.html` | `validada_em` | 20,23,42 |
| Mesmo arquivo | `respondida_em` | 24,29,32 |

Inputs `name="recusa_motivo"` são ingresso de resposta, não leitura persistida: `modal_fio.html:61`, :295 e `_pergunta_card.html:114`.

**Migrations, separadas.**

| Campo | Definição/alteração |
|---|---|
| `respondida_em` | `app/chamados/migrations/0014_perguntadisputa.py:25` |
| `validada_em` | Mesmo arquivo :26 |
| `materializada_em` | `app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:23` |
| `pendencia_admin` | `app/chamados/migrations/0043_hx_pendencia_admin.py:15` |
| `recusa_motivo` | `app/chamados/migrations/0040_a27_recusa_motivo.py:15` |
| `via_resolucao` | Criação `0033_perguntadisputa_via_resolucao.py:15`; alterações `0035_via_admin_decidiu.py:15`, `0036_a1_pergunta_celula.py:22`, `0037_a3_estado_honesto.py:31`, `0040_a27_recusa_motivo.py:20`, `0041_b2_via_legado_sem_lastro.py:15`, `0042_hx_via_ausencia_dia.py:15`, `0046_via_celula_absolveu.py:15`, `0048_via_competencia_trancada.py:14`, `0051_decisao_pergunta.py:110` |
| `via_resolucao` leitura executável | `app/chamados/migrations/0053_pergunta_unica_por_familia.py:32`, `Q` do universo do fato |

`materializada_em` de `RegularizacaoExterna` na migration `0021_regularizacaoexterna_regularizacaoitembatida_and_more.py:26` é homônimo e fica fora do censo de PerguntaDisputa.

**Divergências adicionais para a consolidação.**

1. **Relatório também re-julga C7.** `relatorios_disputa.classificar` usa C6 e devolve `PRONTA_FECHAR` quando não há vivas, sem consultar `respostas_sem_veredito`; C7 acrescenta precisamente essa guarda. (`app/chamados/relatorios_disputa.py:18`, :20; `app/chamados/models.py:1843`, :1853.) Deve compor a mesma decisão do explicador, não apenas a tela.

2. **Recusa é texto e insumo operacional.** `fila_flip_recusas` interpreta regex de `recusa_motivo` para extrair PK de Batida. A máscara transforma esse texto na sombra. Isso não demonstra defeito em produção, mas estabelece dois significados/leitores diferentes do campo. (`app/ponto/services/flip_auto.py:132`, :135; `bin/sombra_mascara_pessoal.py:57`, :62.)

3. **Validação e materialização se confundem na data apresentada de procedência.** `procedencia` usa `validada_em or materializada_em` e `validada_por or materializada_por`; seu resultado é procedência da marcação, não veredito de fechamento. (`app/ponto/services/procedencia_batida.py:72`, :74.)

4. **Respondida como proteção de conversa.** `pode_executar_csf` recusa a retratação por qualquer `respondida_em` preenchido nas perguntas da DS ou resolvedoras. Esse leitor responde “há conversa que exige decisão humana”, enquanto API/template apresentam resposta recebida e os seletores admin classificam fila. (`app/ponto/supra_juiz.py:539`, :545, :548; `app/api/views.py:2568`; `app/chamados/views_cobrar.py:169`.)

5. **`pode_fechar` não é coluna.** É método derivado da DS, definido em `models.py:1848`, retorna a conjunção de ausência de respostas sem veredito e ausência de fio vivo em :1853. O homônimo de apresentação vem do dict `explicacao` consumido pelo template em :435/:446; não confundir sua “escrita” com persistência da DS.

## 11/11 09:07 · sessao 5a294bb62c7c · - · - · origem=Ronald (tmux)

Use `E=app/chamados/services/disputa_emissao.py`, `V=app/chamados/services/validacao.py`, `MT=app/chamados/services/materializacao.py`, `T=app/chamados/tests/`. Provas abaixo foram lidas; execução dos testes [nao verificado]. [nao verificado]

| Porta / entrada | Juiz consultado | Operação / persistido | Retorno | Prova |
|---|---|---|---|---|
| Responder; resposta vazia, escolha inválida, hora fora formato ou `OUTRO_MOTIVO` em hora | Guardas locais por `tipo_resposta`/opções | Recusa antes de salvar. `E:1433`, `:1435`, `:1445`, `:1448`, `:1451`, `:1458` | `(False, mensagem)`; contrato `E:1428` | sem teste específico aberto | [nao verificado]
| Responder; primeira resposta admissível | Validação local do formato; `reabrir_por_resposta` para carimbo sem lastro | `resposta_colab`, `respondida_em`; `DecisaoPergunta.RESPOSTA` no mesmo atomic; eventual descarimbo pela porta. `E:1467`, `:1468`, `:1485`, `:1503`, `:1508`, `:1511` | `(ok, erro)`; `E:1428` | `T/test_reresposta_invalida_validacao.py:67` |
| Responder novamente; tinha validação | Mesmas guardas | Limpa `validada_em/por`, preserva histórico anterior na sequência; resposta nova recebe instante próprio. `E:1470`, `:1480`, `:1511` | Mesmo contrato | `T/test_reresposta_invalida_validacao.py:55` |
| Responder escape `FOLGA_CONTESTA` | Roteamento explícito | Grava `recusa_motivo` com a resposta; exige decisão admin, não planta nessa etapa. `E:1486`, `:1497`, `:1508` | Mesmo contrato | sem teste específico aberto | [nao verificado]
| Responder `ESCAPES_QUE_VIRAM_AUSENCIA` | Tabela do materializador / `estado_inicial` conforme comentário | Após resposta salva, chama `_materializar_ausencia_de_resposta`. `E:1538`, `:1541` | Mesmo contrato | sem teste específico aberto | [nao verificado]
| Validar; PK inválido, sem resposta, já validada | Filtro PK/DS; carimbos individuais | Sem gravação. `V:385`, `:389`, `:393`, `:396` | `nao_encontrada`, `sem_resposta`, `ja_validada`. `V:391`, `:394`, `:397` | sem teste comportamental específico aberto | [nao verificado]
| Validar; implausível sem confirmação | `motivo_implausivel_validacao` | Sem gravação. `V:404`, `:405` | `implausivel`. `V:406` | sem teste específico aberto | [nao verificado]
| Validar resposta hora admissível | Implausibilidade; unidade materializadora; depois C7 na cauda | Sob atomic/travas C→D→Ch, grava `validada_em/por` e decisão VALIDAÇÃO; roda **uma pergunta** antes do laço. `V:425`, `:431`, `:435`, `:461`, `:463`; escrita `V:134`, `:137`, `:139` | `validada` se gravou; `ja_no_espelho` se reuso. `V:515`, `:517` | `T/test_o261_recusa_fala.py:135` |
| Validar hora; unidade recusou sem encerrar e há motivo | `_resposta_de_hora`; `materializada_em is None`; motivo preenchido | Limpa **só esta** `validada_em/por`, preserva resposta, registra DEVOLUÇÃO; pode reabrir DS, salvo `sem_reabrir`. `V:188`, `:190`, `:192`, `:199`, `:205`, `:217`, `:464` | `nao_gravou`, `devolvida=True`; repetição igual não duplica trilha. `V:473`, `:499` | `T/test_o261_recusa_fala.py:87`, `:110`, `:148`, `:164` |
| Validar; recusa já encerrou identidade | Mesma devolução verifica materialização | Não desfaz validação quando `materializada_em` preenchida. `V:190` | Devolução `False`; retorno final depende erros. `V:191`, `:490` | `T/test_o261_recusa_fala.py:190` |
| Materializar lote normal | Fila por carimbos; unidade; C6/C7 | Filtra resposta+validação não nulas/materialização nula; itera **individualmente**; cauda fecha se C7 permitir. `MT:1369`, `:1375`, `:1376`, `:1378`, `:1316`, `:1333` | Dict `materializadas/reusos/pendentes/disputa_fechada/erros`. `MT:1381` | `T/test_o317_passivo_pela_porta.py:188`, `:207`, `:481`, `:502` |
| Unidade; dia diverge sem conferência | `fio.data_do_fato_de` | Não planta; grava recusa na pergunta. `MT:1191`, `:1193`, `:1213` | `False`. `MT:1214` | sem teste específico aberto | [nao verificado]
| Unidade hora sem texto humano | Guarda de anuência por `resposta_colab.strip` | Não planta; recusa escrita. `MT:829`, `:836` | Ramo `None`; unidade `False`. `MT:841`, `:1227` | referência interna a `T/test_a27_recusa_com_dono.py` em `MT:831`; teste não aberto |
| Unidade hora; marco indeterminado | `_tipo_pelo_marco` / `entrada_de_marco` / `grade_da_celula` | **Não recusa necessariamente**: mantém tipo pelo motivo e devolve pendência `tipo_nao_conferido`; escritor persiste pendência+motivo. `MT:386`, `:387`, `:395`, `:397`, `:400`, `:850`, `:851` | Continua até demais guardas. `MT:404`, `:414`, `:422` | sem teste específico aberto | [nao verificado]
| Hora; batida no slot já existe | `_batida_existente_na_janela` | Reusa, emite aviso `__REUSO__`, pode trocar `pergunta_origem` por `anotar`; reconcilia destino/clones. `MT:859`, `:865`, `:866`, `:946`, `:948`, `:974` | Ramo `True`; aviso deslocado para `reusos`. `MT:975`, `:1291`, `:1295` | `T/test_o317_passivo_pela_porta.py:207` cobre controle normal; teste específico de reuso não aberto |
| Paridade; vizinha mesmo tipo, sem furo que absolva, lâmpada `False/None` | `_vizinha_mesmo_tipo`, `_plantio_preenche_furo_na_data`, `fato_do_marco_no_espelho` | Não planta; `_carimbar_recusa` sem via terminal PARIDADE; pela validação, devolução limpa validação. `MT:875`, `:876`, `:900`, `:901`, `:913`; `V:464` | Ramo `None`; unidade `False`; validação `nao_gravou`. `MT:916`, `:1227`; `V:500` | `T/test_guarda_paridade_materializador.py:59`; `T/test_o261_paridade_respeita_a_lampada.py:174`, `:192`, `:202` |
| Paridade; mesma guarda, lâmpada exatamente `True` | Mesmo juiz da lâmpada | **Não planta**, mas `carimbar_identidade(VIA_PARIDADE_RECUSADA)` grava encerramento. `MT:901`, `:924` | Ramo `None`, unidade `False`, embora haja escrita de estado terminal. `MT:925`, `:1227` | `T/test_o261_paridade_respeita_a_lampada.py:183`; `T/test_o317_passivo_pela_porta.py:361` |
| Guarda paridade; furo do próprio dia | `_plantio_preenche_furo_na_data` lê célula/ata e `plantio_preenche_furo` | Pode atravessar a guarda. A véspera não integra a consulta atual. `MT:170`, `:181`, `:876` | Prossegue para porta Batida. `MT:927` | `T/test_o261_vespera_nao_absolve.py:76`, `:85` |
| Hora elegível sem vizinha impeditiva | `registrar_batida`, lastro `_pergunta_materializada_de_fato`, C6 em `carimbar` | Cria Batida institucional, anota origem, reconcilia destino; carimba via declaração ou via fato com `resposta='validar'`, relavra. `MT:927`, `:929`, `:930`, `:948`, `:974`, `:1233`, `:1277` | Unidade `True`. `MT:1282`, `:1299` | `T/test_guarda_paridade_materializador.py:90`; `T/test_o261_recusa_fala.py:135` |
| Batida criada/reusada, mas lastro/C6 reprova | Lastro e C6 | Exceção `_Reprovada` dentro atomic desfaz Batida/carimbo; finally grava motivo fora do savepoint. `MT:1220`, `:1233`, `:1239`, `:1277`, `:1279`, `:1283`, `:1297` | `False` e erro. `MT:1299` | `T/test_hx_materializa_depois_julga.py:30` |
| Recusa textual classificada COLISÃO | `traduzir_recusa` | `carimbar_identidade(VIA_FATO_OCUPADO)`; motivo no mesmo save; demais códigos só alteram motivo. `MT:1091`, `:1093`, `:1099`, `:1102` | `_carimbar_recusa` não possui retorno explícito. `MT:1107` | `T/test_o261_recusa_fala.py:190` prova devolução não mexe no encerrado |
| Reentrada durante plantio | `pergunta_em_voo` transacional | O materializador é escritor da P em voo; S143/S144 devem pulá-la. `MT:1141`, `:1144`, `:1146`, `:1150`, `:1153`, `:1425` | Contexto termina em commit/rollback/exceção. `MT:1147`, `:1149` | `T/test_pep_reentrada.py:109`, `:134`, `:141`, `:176`, `:193`, `:305` |

Divergências para consolidação:

1. **Unidade individual × par**. Opções atuais: pré-materializar uma pergunta na validação; depois iterar prontas individualmente; reparador também usa a unidade com fila distinta (`V:461`, `:463`, `:469`; `MT:1162`, `:1369`, `:1375`). A lei já manda par em um ato, metade espera, e estado derivado: `app/docs/BACKLOG.md:1929`, `:1930`. Não é nova pergunta de lei.

2. **`validada_em` = veredito humano × preenchimento simétrico pelo carimbo**. Validador escreve assinatura humana (`V:134`, `:135`), materializador pede `carimbar(..., resposta='validar')`, que pode suprir carimbos (`MT:1273`, `:1277`). O lookup “escritor único” não prova uma única atribuição física; desenho do escritor já decidido por L-007 (`app/docs/LEIS.md:65`). Não redefinir o campo silenciosamente.

3. **`materializada_em` = aplicação × encerramento sem Batida**. Paridade autorizada grava identidade e retorna `None` (`MT:924`, `:925`); COLISÃO também encerra sem plantio (`MT:1099`); caminho normal usa mesmo carimbo depois de julgar lastro (`MT:1233`, `:1277`). O enum já distingue vias; não exige campo novo (`MT:438`, `:439`; `app/docs/BACKLOG.md:1930`).

4. **Leitor `estado_aplicacao_pergunta` re-julga e não acompanha toda a escrita**. Se carimbada, só separa paridade e chama o restante `aplicada`, sem lastro (`MT:434`, `:438`, `:442`). Sem carimbo exige `existente.aprovada` para `ja_satisfeita`, enquanto escritor reusa qualquer Batida encontrada (`MT:448`; `:859`, `:865`). A previsão `bloqueada_paridade` não consulta lâmpada, mas escritor consulta para decidir identidade (`MT:452`, `:453`, `:454`; `:900`, `:901`, `:924`). L-002/testemunha e célula soberana já decidem quem manda (`app/docs/LEIS.md:60`, `:71`; `CLAUDE.md:167`).

5. **`pendencia_admin`/`recusa_motivo` não significam exclusivamente “espera outra metade”**. Pendência nasce também por tipo sem marco, com plantio permitido (`MT:387`, `:395`, `:397`, `:850`); recusa recebe contestação colab (`E:1497`), recusa materializador (`MT:1102`), recusa admin (`V:584`). Derivar espera só por esses campos colide com significados já existentes; estado explícito foi superado (`app/docs/BACKLOG.md:1930`).

6. **Universo cru da vizinhança × apurável**. `_vizinha_mesmo_tipo` usa `Batida.objects` sem excluir `retratada_em`, e compara `Batida.tipo` (`MT:94`, `:98`, `:100`, `:104`). L-025 e L-087 já fornecem universo/papel; não é opção de lei nova (`app/docs/LEIS.md:76`, `:119`).

Atribuição quantitativa dos 77 colab-dias, 486 grupos/848 excedentes e 18 DS a estas transições: **[aguarda sonda]**. Os documentos registram os agregados e vínculo à obra, mas não fornecem PKs ou partição pelas condições acima (`app/docs/BACKLOG.md:1882`, `:1883`). O caso 51522/51523 foi fornecido no prompt; estado efetivo das duas linhas **[nao verificado]**.

## 11/11 09:07 · sessao e58cbb9aa56f · - · - · origem=Ronald (tmux)

Recorte conferido por leitura; testes abaixo foram abertos, não executados. Resultado de execução: [nao verificado]. [nao verificado]

### Matriz — reconciliador e fechamento

| Porta / entrada | Juiz consultado | Operação / persistido | Retorno | Prova lida |
|---|---|---|---|---|
| `lavrar_sem_turno_aberto`: motivo C1, texto vazio, sem materialização, sem turno aberto naquele dia | `_tem_turno_aberto` e depois C6 `pergunta_viva`; pergunta viva barra | `p.carimbar(VIA_SEM_TURNO_ABERTO, user_sistema)`; C7 depois pode chamar `d.fechar` | `False` nas guardas/carimbo recusado; `True` após carimbo, mesmo que DS não feche | Código `app/chamados/reconciliador.py:362`, `:364`, `:366`, `:371`, `:398`, `:405`, `:411`, `:414`; testes `app/chamados/tests/test_o261_lavra_le_a_lampada.py:129`, `:148`, `:172` |
| `retratar_perguntas_absolvidas_por_celula`: P sem validação/materialização, célula julgada que não acusa | C6; tranca excluída separadamente | Atomic por P, trava C, `carimbar(VIA_CELULA_ABSOLVEU)`; acumula DS, depois C7 e fechamento | Número de carimbos; exceções individuais são logadas e laço segue | `app/chamados/reconciliador.py:584`, `:592`, `:595`, `:611`, `:617`, seleção `:627`, juiz `:661`, tranca `:667`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:70`, `:75`, `:84`, `:105` |
| `encerrar_orfas_pela_lampada`: motivos `orfao_14h`/`saida_sem_entrada`, DS aberta, sem carimbo/validação, com dia | `veredito_orfa`: texto vazio, lâmpada do próprio marco=True, C6 viva, teto temporal e `dia_encerrado_motivo` | `p.encerrar_fantasma(user)` e evento; para DS atingidas consulta respostas sem veredito, depois C7, chama fechar | Dict `encerradas`, `adiadas`, `disputas_barradas`, `linhas`; falha de fechamento pode entrar em `falhas` | `app/chamados/reconciliador.py:1040`, `:1048`, `:1082`, `:1084`, `:1086`, `:1088`, `:1090`, `:1128`, `:1147`, `:1163`, `:1170`, `:1175`; `app/chamados/tests/test_orfas_pelo_evento.py:96`, `:112`, `:124`, `:133`, `:150`, `:167` |
| C7, leitura | `respostas_sem_veredito()` e `fio_vivo(list(perguntas))` | Nenhuma coluna `pode_fechar`; método derivado | `not respostas_sem_veredito and not fio_vivo` | `app/chamados/models.py:1848`, `:1853`; registro `app/core/juizes.py:279`; `app/docs/CORTES.md:272` |
| Fechar normal, DS aberta, resposta sem veredito | Lista canônica: validação nula, materialização nula, timestamp ou texto de resposta; dia trancado não bloqueia | Recusa antes das escritas | `ValueError` | `app/chamados/models.py:1842`, `:1846`, `:1886`, `:1888`; `app/chamados/tests/test_c7_red_minimos.py:160`, `:377` |
| Fechar normal, DS aberta, P viva | C6 por pergunta | Recusa | `ValueError` | `app/chamados/models.py:1895`, `:1898`; `app/chamados/tests/test_c7_red_minimos.py:132` |
| Fechar normal, DS aberta, sem resposta sem veredito nem P viva | Mesmas duas guardas de C7, implementadas novamente no corpo de `fechar` | `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; motivo `vazia` com zero P, `veredito` com alguma P; evento | `True` | `app/chamados/models.py:1886`, `:1896`, `:1903`, `:1932`, `:1936`, `:1941`; `app/chamados/tests/test_c7_red_minimos.py:147`, `:174`, `:185` |
| Fechar `desligamento` / `admin_encerra` | Resposta sem veredito continua vetando; exceção nomeada admite silêncio vivo | Antes do save DS, `fechar_por_admin` nas vivas, decisão SILENCIO e eventos; depois quarteteto do fechamento | `True`; `admin_encerra` exige motivo | `app/chamados/models.py:1886`, `:1918`, `:1921`, `:1924`, `:1932`, `:1943`; `app/chamados/tests/test_c7_red_minimos.py:209`, `:221`, `:234`, `:261` |
| Fechar competência trancada | Tranca por leitura de C6/lista de respostas | Fecha DS com motivo específico; não carimba P | `True` pela porta | `app/chamados/models.py:1865`; `app/chamados/tests/test_c7_red_minimos.py:329`, `:361`, `:369`, `:377` |
| Fechar DS já fechada, mesma entrada / entrada diferente | Idempotência por autor, resolução, motivo | Sem nova escrita na mesma entrada | `False` / `ValueError` | `app/chamados/models.py:1879`, `:1882`, `:1883`; `app/chamados/tests/test_c7_red_minimos.py:428`, `:439` |

A leitura de respostas e de vivas acontece antes do atomic/trava interno do fechamento; o atomic começa em `app/chamados/models.py:1905`, após as leituras `:1886` e `:1895`. Efeito concorrente em produção: [nao verificado].

### Decisões candidatas

1. **Bool de fechamento com dois significados, já decidido pela lei.** O método C7 exige ausência de resposta sem veredito **e** ausência de fio vivo (`app/chamados/models.py:1853`). O explicador escreve `pode = not _fio_vivo(ps)` e publica isso sob a mesma chave `pode_fechar` (`app/chamados/services/explicador.py:137`, `:163`). Opções presentes: critério completo C7 versus critério parcial do explicador. A lei escolhe C7, sem pergunta nova: `app/docs/CORTES.md:272`, `app/docs/LEIS.md:60`, `app/core/juizes.py:279`.

2. **Carimbo não equivale a batida criada, já reconhecido pelo código.** A UI distingue `materializada_em` com `VIA_PARIDADE_RECUSADA` de aplicação (`app/chamados/services/materializacao.py:434`, `:438`, `:439`), mas todo outro carimbo retorna `aplicada` imediatamente (`:442`). Opções de leitura presentes: carimbo genérico / via específica / prova do fato. A obrigação de ler a mesma autoridade já consta da L-002 e L-061 (`app/docs/LEIS.md:60`, `:95`); a classificação de todas as vias no caso vivo depende da sonda [aguarda sonda].

3. **`validada_em` encerra vitalidade sem provar aplicação, e `estado` traduz outra pergunta.** C6 retorna morto imediatamente com validação (`app/chamados/juizes.py:1325`). `P.estado` retorna `VALIDADA` quando validada e não materializada, `RESOLVIDA` se ambas (`app/chamados/models.py:3041`, `:3043`). O mandato do par já exige aplicar ambas num ato e esperar a metade; estado explícito foi superado pelo derivado (`app/docs/BACKLOG.md:1929`, `:1930`). Não reabrir lei sobre “criar estado espera”; esse desenho já está decidido.

4. **Acervo BATIDA sem episódio versus episódio — pergunta de lei real.** `plano_de_retratacao` declara a lei aberta para permitir identidade/retratação entre as classes (`app/chamados/services/fio.py:1274`, `:1275`). Opções existentes: preservar por `sem_episodio_intocada`; retratar somente o restante aceito pelo plano (`:1294`, `:1297`). Outras proteções não são perguntas novas: mérito do admin `:1290`, múltiplos episódios `:1292`. Pergunta: “Quando a linha BATIDA sem episódio e a linha com episódio representam o mesmo fato, autorizando uma retratar a outra?” Alcance nos 486/848 [aguarda sonda].

5. **Várias caudas fecham DS; “só beco” não descreve esta cópia.** Reconciliador tem caudas C7 em `app/chamados/reconciliador.py:411`, `:611`, `:1170`. O que explica cada uma das 18 DS depende de PKs/estados e histórico, ausentes no raspado [aguarda sonda]. A lei de fechamento já foi decidida por C7 e L-007 (`app/docs/CORTES.md:272`, `app/docs/LEIS.md:65`).

Número de perguntas de lei identificadas neste recorte: **1**, a de identidade do acervo sem episódio (`app/chamados/services/fio.py:1275`). Não atribuir aos 77/486/848/18 nenhuma fração sem sonda.

### Funções para a sonda

- `fio.grupos_de_colisao(colaborador_id=None, dia_antes_de=None, fora_do_recorte=None)` devolve `(familia, chave, pks)`; chama o universo real da porta (`app/chamados/services/fio.py:1923`, `:1976`, `:1988`).
- `fio.universo_do_grupo(pks)` carrega linhas do fato, exclui conversa/vias fora do fato e chama `fecho_do_fato` (`app/chamados/services/fio.py:1905`, `:1910`, `:1912`).
- `fio.canonica_do_grupo(ps)` prefere texto não vazio e depois menor PK (`app/chamados/services/fio.py:1231`, `:1232`, `:1233`).
- `fio.plano_de_retratacao(perdedoras, canonica)` retorna `retrata` como objetos; `merito_recusado`, `episodios_unidos`, `sem_episodio_intocada` como PKs; `canonica_sem_episodio` é marcador (`app/chamados/services/fio.py:1261`, `:1277`, `:1280`, `:1290`).
- As chaves são MARCO `(colaborador_id, data_evento, motivo, instancia_marco)` e BATIDA `(batida_origem_id, motivo)`; o fecho pode dividir uma chave BATIDA por dia. Agrupar tudo apenas por colaborador-dia não reproduz os 486 grupos do juiz (`app/chamados/services/fio.py:1372`, `:1963`, `:1976`).
- Não há função canônica implementada de elegibilidade conjunta do par identificada no recorte; o mandato está no BACKLOG (`app/docs/BACKLOG.md:1929`). Um agrupamento diagnóstico colab-dia não deve ser apresentado como juiz do par [nao verificado: existência global fora do recorte].
- `pergunta_viva` memoiza só em memória (`app/chamados/juizes.py:863`, `:864`), mas seus caminhos de exceção logam via `registrar_engolido` (`:1397`, `:1409`), que usa `logging.error` (`app/core/observ.py:73`, `:79`). Para uma sonda que nada grava, usar envelope SQL `SET TRANSACTION READ ONLY` em todos os aliases consultados, sem chamar portas mutadoras, e desabilitar logging durante a leitura. Não usar `rollback` como licença para executar os materializadores; eles não são leitura.

Amostra positiva já aberta de leitores relevantes: lista de respostas `app/chamados/models.py:1843`, C7 `:1853`, C6 `app/chamados/juizes.py:1325`, explicador `app/chamados/services/explicador.py:137`, seleção reconciliador `app/chamados/reconciliador.py:629`.

Amostra positiva já aberta de escritores relevantes: fechamento `app/chamados/models.py:1932`; delegações de carimbo `app/chamados/reconciliador.py:405`, `:595`, `:1147`; decisão de silêncio `app/chamados/models.py:1924`. Escritores físicos do carimbo devem ser completados pelo outro explorer.

## 11/11 09:07 · sessao 12e26c039336 · - · - · origem=Ronald (tmux)

Inventário de interfaces conferido por AST em memória (`python3 -B`, stdin), complementado pela leitura dos templates; script não importa a aplicação. Execução produtiva e consumidores externos: [nao verificado].

| Arquivo / função | Leitura e pergunta respondida |
|---|---|
| `app/api/views.py:973 api_chamados_v2` | F em `:1025`, `:1041`: representação de disputas abertas; R em `:1170`, `:1172`: texto e instante no JSON; R em `:1184`: habilita correção. |
| `app/api/views.py:2531 api_disputa_perguntas` | R em `:2582`: subconjunto `respondidas` compatível com APK. |
| `app/api/views.py:2554 api_disputa_perguntas._item` | `estado` em `:2555`, R em `:2566`, `:2568`, `resposta_display` em `:2567`: estado, espera por admin, resposta e instante do item JSON. |
| `app/api/views.py:2589 api_disputa_responder` | R em `:2625`, `:2642`: reenvio e instante devolvido após resposta. |
| `app/api/views.py:2675 api_solicitar_correcao_disputa` | F SQL em `:2683`: correção somente em continente aberto. |
| `app/api/views_mensageria.py:554 _item_validar` | R em `:567`, `:569`, `:570`: texto, instante e dias de espera na fila IA; chamado por `api_mensageria_validar` em `:546`. |
| `app/api/foto_recebida.py:57 resolver_disputas_de_foto` | F SQL em `:62`: disputas de foto alcançadas pelo upload. `resolucao=` em `:80` é argumento de escrita, não leitor. |
| `app/chamados/views.py:359 detalhe_chamado_local` | F SQL em `:471`, `:495`, `:512`, `:530`: continente/perguntas do detalhe e cards. |
| `app/chamados/views.py:882 modal_fio_colaborador._resp_modal_full` | F SQL em `:911`: disputa carregada no modal. |
| `app/chamados/views.py:1228 responder_pergunta_disputa` | C em `:1257`: proteção contra alteração de resposta regularizada; R em `:1280`: resposta anterior/reenvio. |
| `app/chamados/views.py:1317 solicitar_correcao_disputa` | F SQL em `:1329`: gate de continente aberto. |
| `app/chamados/views.py:1372 reabrir_disputa_supervisao` | R SQL em `:1416`: perguntas pendentes que, após C6, determinam recobrança versus correção. |
| `app/chamados/views_cobrar.py:145 validar_inline` | F em `:161`, `:164`; R em `:169`, `:176`: continente, respostas disponíveis para validar e perguntas ainda aguardando colaborador. |
| `app/chamados/views_cobrar.py:275 proposta_lote` | R em `:291`: amostra JSON da proposta de validação. |
| `app/chamados/relatorios_disputa.py:11 classificar` | R em `:21`: `AGUARDA_COLAB` versus `AGUARDA_SUPERVISAO`, depois de selecionar vivas. |
| `app/chamados/relatorios_disputa.py:26 coletar` | F SQL em `:41`: universo do relatório de disputas abertas. Consumidores: `app/colaboradores/views.py:2997`, `app/chamados/management/commands/relatorio_pendencias_disputa.py:15`. |
| `app/chamados/templatetags/chamados_tags.py:107 leitura_da_resposta` | `getattr(resposta_colab)` em `:128`: evita interpretar resposta vazia; consulta o juiz de classificação em `:133`, depois produz frase em `:138`. |

| Template / unidade de render | Leituras |
|---|---|
| `app/templates/chamados/partials/modal_fio.html` | R `:185`, `:217`, `:219`, `:220`; V `:228`; A20 instante/autor `:170`, `:171`; A17 pendência `:222`, `:225`; recusa `:226`; F histórico `:379`; indiretos `disputa.aberta` `:76`, `:326`, filtro `leitura_da_resposta` `:250`. |
| `app/templates/chamados/partials/_card_atendimento.html` | R `:94`, `:100`, `:108`, `:135`, `:137`, `:142`, `:148`, `:155`, `:176`, `:177`, `:384`, `:385`; indiretos `viva` `:77`, `estado` `:183`, `resposta_display` `:100`, `:137`, `:177`, `:385`; A17 `:183`; via/recusa `:186`. A linha `:186` mistura vias derivadas e duplicata A06: classificação precisa ser por ramo. |
| `app/templates/chamados/partials/_pergunta_card.html` | V `:20`, `:23`, `:42`; R `:24`, `:29`, `:32`; `resposta_display` `:31`; filtro `leitura_da_resposta` `:43`. |
| `app/templates/chamados/partials/_lote_preview.html:24` | R: resposta na prévia de validação em lote. |
| `app/templates/chamados/fio_colaborador.html:164` e `:169` | `disputa.aberta`: cor do continente. |

Os vínculos entre render e template estão em `app/templates/chamados/detalhe_local.html:310`, `app/chamados/views.py:404`, `app/templates/chamados/meu_atendimento.html:127`, `:144`, `app/chamados/views.py:927` e `app/chamados/views_cobrar.py:362`.

Nenhum consumidor JS local dos nomes de campos legados foi encontrado na busca estática; exaustividade dinâmica e APK: [nao verificado]. O aparente `p.estado` em `app/templates/core/_copiloto.html:407` pertence às pautas carregadas por `/pautas/aba/` em `:359`, não a `PerguntaDisputa`. Inputs `name="recusa_motivo"` em `app/templates/chamados/partials/modal_fio.html:61`, `:295` e `_pergunta_card.html:114` são nomes de formulário, não leituras da projeção.

## 11/11 09:07 · sessao 896f3b04993a · - · - · origem=Ronald (tmux)

Dependências verificadas por AST de imports, sem importar a aplicação:

| Dependência | Evidência |
|---|---|
| `models ↔ juizes` | `app/chamados/models.py:153`; `app/chamados/juizes.py:11` |
| `models ↔ fio` | `app/chamados/models.py:433`; `app/chamados/services/fio.py:157` |
| `models ↔ disputa_emissao` | `app/chamados/models.py:2219`; `app/chamados/services/disputa_emissao.py:16` |
| `juizes ↔ materializacao` | `app/chamados/juizes.py:371`; `app/chamados/services/materializacao.py:155` |
| `fio ↔ vigia_fio` | `app/chamados/services/fio.py:774`; `app/chamados/services/vigia_fio.py:558` |
| `models ↔ reconciliador` | `app/chamados/models.py:977`; `app/chamados/reconciliador.py:241` |
| `declaracao_texto → validacao → disputa_emissao → declaracao_texto` | `app/chamados/services/declaracao_texto.py:648`; `app/chamados/services/validacao.py:559`; `app/chamados/services/disputa_emissao.py:859` |
| `tarefa_do_dia → juizes/models/fio/painel/lavra` | `app/chamados/services/tarefa_do_dia.py:123`, `:124`, `:125`, `:364`, `:401` |
| APIs/dashboard/cobrança → consulta central | `app/api/views.py:324`, `:1004`, `:2542`; `app/api/views_core.py:313`; `app/core/views.py:252`; `app/chamados/services/cobranca.py:47`; `app/chamados/services/aviso_pendencia.py:8` |

A amostra de 18 arquivos forma uma componente de dependências cíclicas com 17 arquivos: `models`, `juizes`, `reconciliador`, `fio`, `materializacao`, `disputa_emissao`, `declaracao_texto`, `validacao`, `pergunta_no_app`, `veredito_lavrado`, `raiox`, `explicador`, `relavra_via`, `vigia_fio`, `fabrica_da_celula`, `painel`, `verbos`. Isso impede inventar uma ordem topológica de pousos independentes apenas pelos imports; os ciclos acima são as testemunhas. Os imports locais permitem carga tardia, mas continuam sendo dependências de execução. (`app/chamados/models.py:433`; `app/chamados/services/fio.py:304`; `app/chamados/services/ex­plicador.py:69`; `app/chamados/services/raiox.py:186`; `app/chamados/services/verbos.py:9`; `app/chamados/services/painel.py:194`; `app/chamados/services/fabrica_da_celula.py:138`; `app/chamados/services/relavra_via.py:209`)

Ordem consumível pelo brief:

1. Gate da E4b medida; só depois migrar fonte. A ordem escrita é E4a → E4b → E4c → E4d, com exceções explícitas. (`app/docs/PROMPTS.md:2034`; `:2054`)
2. Núcleo de leitores/properties/querysets e a alimentação de `tarefa_do_dia`, tratado como lote coordenado quando tocar arquivos compartilhados ou ciclos acima. A consulta central ainda depende de `q_vivas`, C6 e R/F. (`app/chamados/services/pergunta_no_app.py:47`, `:50`, `:58`, `:71`)
3. Cascas/API/dashboard/cobrança e respectivos templates/JS, consumindo as interfaces estabilizadas; todas as mudanças em `app/api/views.py` pertencem ao mesmo lote de arquivo. (`app/api/views.py:324`, `:1004`, `:2542`; `app/core/views.py:252`; `app/chamados/services/cobranca.py:47`)
4. Auditorias, vigias e instrumentos que hoje observam projeções; separar conservação histórica da pergunta corrente, preservando diagnóstico da migração. (`app/chamados/management/commands/diff_e3_sombra.py:22`; `app/chamados/services/vigia_fio.py:803`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`)
5. E4d só após o selo E4c, preservando as exceções; parar escrita não autoriza retirar fisicamente o acervo exportado. (`app/docs/PROMPTS.md:2034`; `:2026`)

`backfill_fio_morto_e4b` e `foto_e4b` não foram encontrados pelos nomes nesta cópia; implementação, estrutura da foto, lista de arquivos atingidos e contrato dessas interfaces: **[nao verificado]**. O contrato operacional verificável limita o passivo a competências não exportadas e manda exportadas para lista do `!`. (`app/docs/PROMPTS.md:2026`)

Não há repositório Git disponível nesta raiz: `git log` retorna “not a git repository”. Portanto identidade Git de `2e9d…`, pai da obra E3 e lista exata de arquivos do diff solicitado: **[nao verificado]**. O relato local menciona `e3fix9`, uma base e outra foto, mas isso não substitui o diff deste hash. (`app/docs/relato_fio_e0_v4_e3_x1.txt:5`; `:7`)

Para o delta pós-pouso, a lista **declarada no anexo**, cuja inclusão efetiva no diff continua **[nao verificado]**, é: `app/api/views.py`, `app/chamados/services/aviso_pendencia.py`, `app/chamados/services/pergunta_no_app.py`, `app/chamados/services/tarefa_do_dia.py`, `app/chamados/services/cobranca.py`, `app/chamados/juizes.py`. As interfaces atuais existem em `app/api/views.py:1004`, `app/chamados/services/aviso_pendencia.py:8`, `app/chamados/services/pergunta_no_app.py:61`, `app/chamados/services/tarefa_do_dia.py:562`, `app/chamados/services/cobranca.py:47`, `app/chamados/juizes.py:1516`.

## 11/11 09:07 · sessao 8ce0aaba136e · BO-ADMIN-1111 · sonda · origem=Code

**As duas perguntas estão respondidas, sem validação e sem materialização. Existem ações de tela que aceitam esse estado; um caminho que conclua ambas com as mesmas horas e batidas permanece [não verificado].** O explicador devolveu `aguarda_validacao`, dono `admin`, para ambas. (`app/chamados/services/explicador.py:77`; sonda: `validada_em = None`, `materializada_em = None` e retornos de `explicar_pergunta`.)

**1. Estado gravado — tabela solicitada em (a)**

Cada valor abaixo reproduz o respectivo bloco `PerguntaDisputa pk=51522/51523` da sonda; a última coluna identifica o campo no código.

| Campo | #51522 | #51523 | Código |
|---|---|---|---|
| Colaborador / célula | col960 / 128547 | col960 / 128547 | `app/chamados/models.py:2414`, `:2433` |
| Motivo / instância | `intervalo_saida` / `int_ida` | `intervalo_volta` / `int_volta` | `app/chamados/models.py:2421`, `:2434` |
| `data_evento` | 21/10/2026 | 21/10/2026 | `app/chamados/models.py:2423` |
| `resposta_colab` | `01:00` | `02:00` | `app/chamados/models.py:2447` |
| `respondida_em` | 08/11/2026 11:04:01 | 08/11/2026 11:04:11 | `app/chamados/models.py:2448` |
| `validada_em` | `None` | `None` | `app/chamados/models.py:2449` |
| `materializada_em` | `None` | `None` | `app/chamados/models.py:3393` |
| Via — nome real: `via_resolucao` | `''` | `''` | `app/chamados/models.py:2561` |
| `pendencia_admin` | `''` | `''` | `app/chamados/models.py:2556` |
| `veredito_lavrado` | `nao` | `nao` | `app/chamados/models.py:2585` |
| `julgado_em` | 11/11/2026 08:15:10 | 11/11/2026 08:15:10 | `app/chamados/models.py:2590` |
| Disputa / destino | 5644 / 35664 | 5644 / 35664 | `app/chamados/models.py:2416`, `:3385` |

O campo equivalente pedido é realmente **`recusa_motivo`**. A sonda trouxe os seguintes textos; o formato corresponde ao erro de paridade em `app/chamados/services/materializacao.py:878`, persistido pelo `finally` em `:1296` e por `_carimbar_recusa` em `:1101`.

> #51522: `Pergunta #51522: batida vizinha #133794 (S 22/10 06:30) tem o mesmo tipo do plantio (S) - par quebrado; nao materializado (guarda de paridade).`

> #51523: `Pergunta #51523: batida vizinha #133756 (E 21/10 18:32) tem o mesmo tipo do plantio (E) - par quebrado; nao materializado (guarda de paridade).`

A disputa **5644 está aberta**, pois a sonda mostra `fechada_em = None`. Seu chamado-pai **25270** tem `status_local = 'aberto'`; o chamado-destino comum **35664** tem `status_local = 'em_analise'`, `resolvido_em = None`. (`app/chamados/models.py:1749`, `:1757`, `:52`, `:3385`; blocos correspondentes da sonda.)

O `veredito_lavrado='nao'` significa **“NAO materializada de fato”** no campo da pergunta; não é prova de decisão administrativa sobre o dia. (`app/chamados/models.py:2582`; sonda: `veredito_lavrado = 'nao'`.)

**2. Ramo do plantio**

Para ambas, a classificação sustentada pelo estado é **recusa de paridade sem carimbo terminal**, no caminho de `_materializar_resposta_hora`. O texto gravado identifica a guarda em `app/chamados/services/materializacao.py:875` e `:878`; a sonda retorna `fato_do_marco_no_espelho(p) -> False`, que, aplicado ao código desta cópia, entra em `:901`, chama `_carimbar_recusa` em `:913` e retorna em `:916`.

**Não estão carimbadas com `VIA_PARIDADE_RECUSADA`:** a sonda mostra `via_resolucao=''` e `materializada_em=None`; o carimbo dessa via ocorre somente depois daquele retorno, em `app/chamados/services/materializacao.py:924`.

O motivo completo com a vizinha fica gravado porque o `finally` chama novamente `_carimbar_recusa` com o último erro. Essa função só carimba identidade quando a tradução é `COLISAO`; para `PARIDADE`, atualiza o motivo sem preencher materialização ou via. (`app/chamados/services/materializacao.py:1296`, `:1091`, `:1093`, `:1101`; classificação `PARIDADE` em `app/chamados/catalogo/recusa.py:26`.)

A validação nula é compatível com o fluxo que desfaz a validação quando uma resposta de hora não foi materializada e tem recusa escrita, preservando a resposta do colab. **A chamada histórica, seu autor e seu instante não foram fornecidos: [não verificado].** (`app/chamados/services/validacao.py:190`, `:199`, `:200`; sonda: resposta preservada, validação nula, recusa preenchida.)

**3. Vizinhas e resultado do juiz**

| Pergunta | Vizinha indicada na recusa | Tipo | Hora local fornecida | Origem | `fato_do_marco_no_espelho` |
|---|---:|---|---|---|---|
| #51522 | #133794 | `S` | 22/10/2026 06:30:52 | `app` | `False` |
| #51523 | #133756 | `E` | 21/10/2026 18:32:07 | `app` | `False` |

Sustentação: blocos `Batida pk=133794/133756`, respectivos `recusa_motivo` e chamadas literais do juiz na sonda; seleção da vizinha em `app/chamados/services/materializacao.py:85`, `:94`, `:100`; correspondência dos motivos aos marcos em `app/chamados/juizes.py:887` e `:888`.

O juiz real lê DNA e lâmpadas da célula e retorna `all(_vistas)`. O `False` fornecido não autoriza reconstruir a ATA completa nem atribuir erro às vizinhas. (`app/chamados/juizes.py:910`, `:927`, `:933`; ATA completa e correção factual das batidas: [não verificado].)

**4. ADMIN: texto, badge, botões e portas**

A sonda devolveu literalmente:

| Pergunta | Estado / dono | Motivo | Ação |
|---|---|---|---|
| #51522 | `aguarda_validacao` / `admin` | `colaborador respondeu 01:00` | `Validar ou recusar a resposta "01:00".` |
| #51523 | `aguarda_validacao` / `admin` | `colaborador respondeu 02:00` | `Validar ou recusar a resposta "02:00".` |

Esse é o degrau `RESPONDIDA`, que retorna antes dos ramos de pendência e do juiz de materialização. (`app/chamados/models.py:3041`, `:3048`; `app/chamados/services/explicador.py:77`, `:85`, `:93`.)

No detalhe, o badge previsto é **“⚠ aguardando validacao”**, seguido da resposta e do horário de envio. No modal, os campos dados não entram nos badges “Pendencia do admin” ou “Validada”; oferecem o ramo de ações sobre a resposta. (`app/templates/chamados/partials/_pergunta_card.html:24`, `:29`; `app/templates/chamados/partials/modal_fio.html:217`, `:222`, `:228`, `:239`.)

A sonda também devolveu `pode_fechar=False` e **“Nao fecha: 30 aguardando sua validacao.”** Isso descreve a disputa inteira; os detalhes das demais pendências estão truncados, portanto [não verificado]. (`app/chamados/services/explicador.py:148`, `:162`; trecho fornecido de `por_que_nao_fecha`.)

As ações das duas devem usar o **chamado-destino 35664**, conforme a precedência de `chamado_que_lista`. A porta confere essa mesma âncora. (`app/chamados/services/fio.py:117`, `:598`, `:635`; sonda: `chamado_destino = 35664`.)

| Ação de tela | Botão e porta | Aplicação ao estado fornecido |
|---|---|---|
| **Validar** | Botão: `app/templates/chamados/partials/modal_fio.html:279`. View do modal: `app/chamados/views.py:869`; encaminhamento: `app/chamados/services/fio.py:585`. | **ACEITA tentar:** respondidas e não validadas passam nas guardas de `app/chamados/services/validacao.py:393` e `:396`. Executa o materializador em `:463`; se a guarda de paridade continuar satisfeita, recusa a gravação e devolve a validação em `:464`. |
| **Rejeitar resposta** | Botão e motivo: `app/templates/chamados/partials/modal_fio.html:290`, `:295`, `:298`. Encaminhamento: `app/chamados/services/fio.py:625`, `:637`. | **ACEITA com motivo:** ambas têm resposta e validação nula. Recusa motivo vazio, pergunta validada ou já pendente em `app/chamados/services/validacao.py:562`, `:573`, `:576`. Reabre para correção em `:593`. **Move para o colab; não materializa o marco.** |
| **Corrigir a hora pela supervisão** | Campos e botão: `app/templates/chamados/partials/modal_fio.html:300`, `:306`, `:309`, `:312`. Encaminhamento: `app/chamados/services/fio.py:639`. | **ACEITA o estado**, com HH:MM e motivo: ambas são de hora, não validadas e de disputa aberta. As recusas estão em `app/chamados/services/acoes_disputa.py:336`, `:338`, `:345`, `:347`, `:349`. Depois grava a resposta e usa a mesma validação/materialização em `:371`, `:379`; conclusão [não verificado]. |
| **Inverter tipo da vizinha** | Clique no horário → “Inverter tipo”: `app/templates/colaboradores/partials/_calendario_grade.html:121`. View: `app/ponto/views.py:3260`. | **ACEITA os campos conhecidos da batida:** ambas são E/S e não retratadas. **RECUSA** sem permissão ou com competência fechada: `app/ponto/views.py:3263`; `app/ponto/services/flip_batida.py:15`, `:21`, `:26`. Permissão, competência e necessidade factual da inversão: [não verificado]. |
| **Retratar a vizinha** | Clique no horário → motivo → “Não foi trabalho (retratar)”: `app/templates/colaboradores/partials/_calendario_grade.html:121`. View: `app/ponto/views.py:3274`. | **ACEITA o estado conhecido**, exigindo usuário autorizado e motivo cadastrado ativo; recusas em `app/ponto/services/retratar_humano.py:142`, `:145`, `:168`, `:170`. Motivo disponível, permissão e fato “não foi trabalho”: [não verificado]. |
| **Resolver dia** | “Resolver dia” → “Trabalhou no horário da escala”, falta ou justificado: `app/templates/ponto/partials/_veredito_dia.html:6`, `:21`, `:22`, `:23`. View: `app/ponto/views.py:3312`. | A **view não recusa simplesmente por pergunta respondida**, mas a **tela não oferece enquanto houver pergunta viva com resposta**: `app/colaboradores/services/calendario.py:472`, `:476`, `:738`; template: `app/templates/colaboradores/partials/_calendario_grade.html:125`. Disponibilidade efetiva: [não verificado]. O serviço ainda pode recusar dia sem trabalho, sem marcos ou sem preenchimento coerente: `app/ponto/services/veredito_celula.py:242`, `:248`, `:308`. |
| **Registrar por regularização externa** | “Adicionar batida nova”, intervalo e “Enviar ao colaborador para confirmacao”: `app/templates/chamados/regularizacao_editar.html:49`, `:57`, `:67`. Views: `app/chamados/views_regularizacao.py:42`, `:74`. | **ACEITA preparar proposta**, mas **RECUSA materializar com disputa aberta** em `app/chamados/services/regularizacao_ext.py:58`, `:61`. A sonda prova a disputa 5644 aberta; portanto não é saída de gravação enquanto esse estado permanecer. |

**Retratar não prova eliminar a recusa de paridade:** a busca da vizinha usa `Batida.objects` sem filtro por `retratada_em` ou `aprovada`. (`app/chamados/services/materializacao.py:94`, `:100`.)

**“Corrigir” também não garante mudar o ponto do plantio apenas trocando o texto:** para intervalo, havendo marco no chamado-destino, o materializador usa a hora desse marco. A sonda mostra `marcos_faltantes` com `01:00/S` e `02:00/E`. (`app/chamados/services/materializacao.py:407`, `:409`, `:411`.)

Há ainda **“Aplicar … e validar”** para declaração em texto, mas exige proposta existente; conserva a resposta já preenchida e volta à mesma validação. Existência de proposta aplicável no caso: [não verificado]. (`app/templates/chamados/partials/modal_fio.html:143`; `app/chamados/services/declaracao_texto.py:649`, `:687`, `:696`.)

**5. Recado do colab**

**O texto efetivamente visto numa tentativa não veio na sonda: [não verificado].** O código permite distinguir:

- **No questionário**, quando a pergunta está visível/viva, os campos fornecidos produzem “Resposta enviada” e **“Aguardando a supervisao conferir. Se precisar corrigir, e so responder de novo abaixo.”** (`app/templates/chamados/partials/_card_atendimento.html:77`, `:94`, `:99`, `:101`; sonda: `respondida_em` preenchida nas duas.)
- **Ao reenviar HH:MM sem tocar no relógio**, a view devolve **“Escolha a hora no relogio antes de enviar.”** Isso depende de `resposta_tocou != '1'`, dado ausente: [não verificado]. (`app/chamados/views.py:1274`, `:1278`.)
- **Na porta “Enviar hora” do chamado**, se ela selecionar estas mesmas perguntas e receber novamente `01:00`/`02:00`, a condição resposta preenchida + mesma hora devolve **“Horario ja informado; aguardando a supervisao.”** A seleção e a tentativa concretas são [não verificado]. (`app/chamados/services/declaracao_texto.py:363`, `:417`, `:418`; retorno à tela em `app/chamados/views.py:1100`.)
- O recado **“resposta anterior ja validada… supervisao precisa refazer esse horario” não é sustentado pelo estado destas duas**, pois exige `validada_em != None`, enquanto ambas têm `None`. (`app/chamados/services/declaracao_texto.py:227`, `:378`, `:412`.)

A própria visibilidade/vitalidade não fica provada apenas por `aguarda_validacao`: o estado pode retornar `RESPONDIDA` no fallback, e o juiz consulta tranca e decisão da célula, não raspadas integralmente. (`app/chamados/models.py:3048`; `app/chamados/juizes.py:1329`, `:1338`; resultado atual de `pergunta_viva`: [não verificado].)

**(b) EXISTE saída pela tela?**

**SIM, para movimentar a espera atual:** a porta **Rejeitar resposta** aceita o estado das duas com motivo e as devolve para correção; **Corrigir** aceita uma hora fundamentada e tenta gravá-la. **Concluir as duas mantendo as mesmas horas e batidas: [não verificado].** (`app/chamados/services/validacao.py:562`, `:573`, `:576`, `:593`; `app/chamados/services/acoes_disputa.py:334`, `:371`, `:379`.)

Passo a passo comprovado pelo código, para admin autorizado e com motivo factual para pedir nova confirmação:

1. Abrir o fio de **col960**, chamado **35664**, onde as perguntas se ancoram. (`app/chamados/views.py:869`; `app/chamados/services/fio.py:117`; sonda: destino 35664.)
2. Na **#51522**, preencher o motivo e clicar **“Rejeitar resposta”**; repetir na **#51523**, se ambas precisarem de nova confirmação. (`app/templates/chamados/partials/modal_fio.html:295`, `:298`; aceitação em `app/chamados/services/validacao.py:573`, `:576`.)
3. O efeito é devolver as perguntas ao colab para correção, não concluir o dia. (`app/chamados/services/validacao.py:593`, `:604`.)

Não há prova suficiente para declarar **“TRAVA SEM SAÍDA PROVADA”**, nem para prometer conclusão por inverter/retratar vizinhas ou por “Resolver dia”; faltam os fatos e condições identificados acima: [não verificado].

**(c) Texto ao admin**

> Abra o atendimento de col960, chamado #35664, e confira as respostas das perguntas #51522 e #51523. Se precisar de nova confirmação, escreva o motivo em cada pergunta e clique em “Rejeitar resposta”; ela volta ao colaborador para correção. Se já tiver confirmado outra hora, preencha a hora e o motivo em “Corrigir” e confira o resultado apresentado após o envio. (`app/chamados/services/fio.py:117`; `app/templates/chamados/partials/modal_fio.html:295`, `:298`, `:306`, `:309`, `:312`; `app/chamados/services/acoes_disputa.py:381`.)

_3758 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
