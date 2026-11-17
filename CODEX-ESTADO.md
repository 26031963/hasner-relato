# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 00:57** · publicado em **17/11/2026 00:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 00:57 · sessao 379168b451c3 · - · - · origem=Ronald (tmux)

1. **Harness sem banco continua quebrado; bloqueia usá-lo como prova verde.** A nova composição extrai `_recusa_sem_anuencia`, mas essa função importa `chamados.juizes`; o teste dubla somente `django.utils` e `ponto.registro_batida`, deixando carregar `juizes.py` → `models.py` → `django.db`. Executei com `python3 -B` e `PYTHONDONTWRITEBYTECODE=1`: `ERROR ModuleNotFoundError: No module named 'django.db'`, antes da asserção sobre as batidas criadas. Referências: `app/chamados/tests/test_bug_par_sem_banco.py:181`, `:188`, `:192`, `:193`; `app/chamados/services/materializacao.py:1284`; `app/chamados/juizes.py:12`; `app/chamados/models.py:7`. **Herdado da B2:** o diff fornecido mostra que o antigo ramo hora já importava o mesmo juiz real; a adaptação da volta 3 não resolve o harness.

2. **A exclusão protege o texto, mas não o devolve à pendência nem informa o motivo aos chamadores.** O `continue` ocorre antes dos buckets `marco_aceso`/`c6_morta`, portanto essa pergunta não aparece nem na lista ressuscitável nem nas razões publicadas. `reabrir`, desarquivar e cartório continuam chamando a mesma porta; o cartório, recebendo zero e buckets vazios, não produz mensagem específica dessa exclusão. Referências: `app/chamados/models.py:1546`, `:1564`, `:1507`, `:1512`, `:1498`; `app/chamados/views_arquivar.py:86`; `app/ponto/services/cartorio.py:1674`, `:1685`, `:1690`. **Não classificaria isoladamente como bloqueante sem medir ocorrência/efeito de produção [nao verificado].**

3. **Há portas capazes de resolver esse legado; não existe garantia de saída automática geral.** A reabertura canônica guarda `ato_texto` mesmo sem instante e limpa par, validação e carimbo, mas os botões comuns selecionam `perguntas_corrigiveis`, que exige resposta legal; validar retorna `sem_resposta`, recusar retorna `ja_pendente`, e reabrir pelo formulário recobra essa pergunta. Referências: `app/chamados/services/disputa_emissao.py:1631`, `:1645`, `:1648`; `app/chamados/juizes.py:1756`; `app/chamados/services/validacao.py:681`, `:902`; `app/chamados/views.py:1418`, `:1423`, `:1427`; `app/chamados/services/acoes_disputa.py:98`, `:101`.

   O comando antigo de limbo pode carimbar pela nota ou limpar pela reabertura, mas exige exatamente a lista fechada de setembro; não cobre qualquer nova excluída. Referências: `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `:48`, `:67`, `:69`; `app/chamados/services/disputa_emissao.py:1683`, `:1696`.

   Se C6 a considera viva, o colaborador consegue responder novamente; o core grava par legal. Isso não necessariamente retira o carimbo: `reabrir_por_resposta` só limpa vias sem lastro. Referências: `app/chamados/juizes.py:1450`, `:1455`; `app/chamados/views.py:1239`, `:1242`; `app/chamados/services/disputa_emissao.py:1481`, `:1499`; `app/chamados/models.py:3450`. Portanto “sempre presa” seria afirmação excessiva; “automaticamente resolvida por outra porta” também seria.

4. **O comentário corrigido em 1626–1630 descreve a seleção nominal; o docstring continua falso.** Texto sem instante **com** validação fica elegível, e `linhas_da_projecao_sem_linha` pode guardá-lo em VALIDACAO antes de a porta zerá-lo. Portanto “Nunca toca … resposta com texto” não corresponde ao código. Referências: `app/chamados/models.py:1546`, `:1570`, `:1626`, `:1638`, `:4104`, `:4115`, `:4118`. **Não bloqueante: documentação.**

5. **Teste de preservação prende um contrato de resultado, com limite explícito.** A fixture cria precisamente texto sem instante/validação; o teste aceita preservação ou remoção acompanhada de qualquer linha com o texto anterior. Isso evita prender a implementação ao `continue`; sem a exclusão e sem nova trilha, a porta limpa o texto, enquanto o helper ignora a pergunta sem ambos os instantes. Referências: `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:345`, `:348`, `:349`; `app/chamados/models.py:1638`, `:4104`. Não verifica retorno, carimbos, classificação da exclusão, chamadores, corrida nem uma linha **nova** do ato; execução com banco [nao verificado].

6. **MaterializadorPerguntaAoJuizTest testa comportamento, mas só parte do contrato.** Os dois casos reais entram pelo laço, com legado gravado por SQL, e exigem ausência de materialização mais motivo de recusa. A escolha anterior materializava; data_hora anterior produzia erro de formato sem esse motivo, conforme o delta. Não exige followup, `pendencia_admin`, retorno, rollback, `declara_falta`, NBSP ou passagem pelo juiz por delegação. Referências: `app/chamados/tests/test_f1nova_b2_leitores.py:173`, `:175`, `:182`, `:184`, `:185`, `:187`, `:190`. Execução e mordida efetiva contra mutante [nao verificado].

7. **O261 recuperou o contrato completo.** `_estado` inclui materialização, autor, via, instante, validação e texto; a nova asserção compara todos, e verifica separadamente o fechamento da disputa. Não prende a forma da implementação. Referências: `app/chamados/tests/test_o261_resposta_vazia.py:72`, `:74`, `:115`, `:116`. O delta desta asserção amplia a cobertura; não há cura correspondente nesse delta cuja ausência ela necessariamente reprovaria. Execução [nao verificado].

8. **Corrida da ressuscitação merece limite, não acusação sem percurso concreto.** A nova exclusão é decidida antes da trava; depois a porta usa os IDs já escolhidos e não repete o predicado. Referências: `app/chamados/models.py:1600`, `:1607`, `:1619`, `:1621`, `:1638`. Não provei escritor de produção que, nessa janela, transforme a pergunta inicialmente admissível em texto sem instante/validação **sem já guardar esse texto**: a resposta grava par legal e trilha; reabertura grava foto; `devolver_validacao_recusada` não toca pergunta materializada. Referências: `app/chamados/services/disputa_emissao.py:1480`, `:1481`, `:1525`, `:1631`; `app/chamados/services/validacao.py:264`. Não apresentaria essa corrida como bloqueante demonstrado.

## 17/11 00:56 · sessao 3fdc22b39b70 · - · - · origem=Ronald (tmux)

1. **Sem bloqueante novo de plantio encontrado:** os três ramos verificam `_recusa_sem_anuencia` antes de produzir o efeito próprio: hora antes de calcular/plantar, data_hora antes do parse/plantio, escolha antes de criar ausência ou reconciliar destinos (`app/chamados/services/materializacao.py:1340`, `:1575`, `:1659`). O dispatcher contempla apenas esses três ramos; tipo diferente deixa `_batida_ok=False` e levanta `_Reprovada` antes do carimbo (`:1858`, `:1910`, `:1923`). A guarda delega ao juiz registrado, sem derivação paralela (`:1284`; `app/chamados/juizes.py:1544`).

2. **Caminhos conferidos:** a validação recusa um par ilegal antes de escrever validação por `pergunta_respondida` (`app/chamados/services/validacao.py:681`), escreve a validação em `:743` e chega à unidade diretamente para hora (`:769`) ou pelo laço (`:786`). O passivo chama `_materializar_uma` (`app/chamados/services/passivo_validado.py:529`); a irmã do par também (`app/chamados/services/materializacao.py:1729`). Portanto, não é literalmente “antes de qualquer escrita” no ato inteiro: o carimbo de validação e sua trilha precedem a guarda do ramo, depois da verificação inicial do mesmo juiz (`app/chamados/services/validacao.py:167`, `:743`).

3. **Recusa conserva o efeito anterior de hora:** motivo curto é escrito por `_carimbar_recusa`, o erro é acumulado e o ramo retorna `None` (`app/chamados/services/materializacao.py:1287`). A unidade retorna `False`, sem carimbo de aplicação, e seu `finally` chama novamente a porta com a mensagem completa (`:1916`, `:1987`). A recusa normal não levanta exceção nem solicita rollback; os efeitos próprios do ramo ainda não ocorreram (`:1909`, `:1340`, `:1575`, `:1659`). O savepoint do par pode desfazer inclusive a recusa da irmã quando ela não grava (`:1723`, `:1739`).

4. **`pendencia_admin` não é escrito por esta recusa.** `_carimbar_recusa` atualiza `recusa_motivo` e chama a porta da nota; só a categoria `COLISAO` carimba identidade (`app/chamados/services/materializacao.py:1674`). A pendência tipificada é escrita noutro ponto do ramo hora, depois do cálculo (`:1346`). Isso já era verdade na guarda anterior fornecida no delta; o comentário da helper promete mais do que o código faz (`:1278`). O legado ilegal continua salvável por outros campos, porque a guarda de transição retorna quando `update_fields` não altera o par (`app/chamados/models.py:3584`).

5. **Não bloqueante: erro inadequado para escolha.** “batida NAO plantada”/“O horario dele” é mensagem compartilhada (`app/chamados/services/materializacao.py:1289`), embora escolha resolva sem batida e possa declarar ausência (`:1646`, `:1662`). Em data_hora a mensagem ainda descreve plantio de horário (`:1590`). O followup também usa texto fixo “Ponto NAO gravado no espelho” e tradução genérica `OUTRO`, pois não há regra específica para “sem resposta” (`:1692`; `app/chamados/catalogo/recusa.py:25`, `:107`).

6. **Carimbos existentes fora da helper não são novos bypasses de plantio:** absorção por ausência chama `q.carimbar` e registra `ABSORCAO` (`app/chamados/services/materializacao.py:1046`); S144 chama a mesma porta depois de consultar lastro externo (`:2161`). A porta de carimbo pode preencher instante de resposta/validação com texto, quando `resposta='validar'`, e consulta C6 antes de salvar (`app/chamados/models.py:3250`). Essas rotas não passam pela helper; portanto, a alegação “nenhum materializa/carimba sem passar” é falsa, mas não nasceu neste delta (`app/chamados/services/materializacao.py:1046`, `:2165`).

Mapa para Q1:

- `respondida_em`/`resposta_colab`: o juiz exige instante **e** texto após `.strip()` (`app/chamados/juizes.py:1526`, `:1552`); a face SQL deriva os brancos de `str.isspace`, incluindo NBSP (`:1555`, `:1560`, `:1575`). A helper apenas lê esse juiz (`app/chamados/services/materializacao.py:1284`). Os parsers continuam distinguindo conteúdo válido de mera presença de texto; data_hora faz `.strip().replace('T',' ')` (`:1578`).
- `validada_em` e trilha: `_carimbar_validacao` preserva projeção anterior, grava instante/autor e linha `VALIDACAO` com texto (`app/chamados/services/validacao.py:167`). O laço lê validação não nula e aplicação nula, relendo a pergunta depois da trava (`app/chamados/services/materializacao.py:2091`, `:2102`).
- `materializada_em`: `_materializar_uma` escreve pela porta `carimbar`, depois dos efeitos e da conferência de lastro (`app/chamados/services/materializacao.py:1921`, `:1966`); a porta salva instante, autor e via (`app/chamados/models.py:3248`, `:3272`). `_carimbar_recusa` lê aplicação para decidir colisão terminal (`app/chamados/services/materializacao.py:1674`).
- `recusa_motivo`: escritor usado pela helper é `_carimbar_recusa` (`app/chamados/services/materializacao.py:1287`, `:1687`); leitores incluem explicador e devolução da validação (`app/chamados/services/explicador.py:113`; `app/chamados/services/validacao.py:266`). `pendencia_admin` tem escritor próprio em hora e prioridade no explicador (`app/chamados/services/materializacao.py:1350`; `app/chamados/services/explicador.py:108`).
- Followup destino: `_carimbar_recusa` chama `nota.anexar_ocorrencia` com identidade pergunta+categoria (`app/chamados/services/materializacao.py:1692`); a porta grava/deduplica por chamado, tipo, objeto e autor (`app/chamados/services/nota.py:95`). A segunda chamada da recusa conserva a primeira nota por essa chave (`:108`); as telas leem `followups` em ordem de criação (`app/chamados/views.py:352`).
- Os campos não têm todos o mesmo significado: C21 responde **existência de resposta**; lastro responde **efeito externo**; validade e materialização continuam estados distintos, consultados separadamente (`app/chamados/juizes.py:1544`, `:666`; `app/chamados/services/materializacao.py:2091`).

Não vi relógio novo, segundo juiz nem segundo escritor no trecho novo da helper: ela lê C21 e chama a porta existente (`app/chamados/services/materializacao.py:1284`). Execução e estoque real não foram medidos [nao verificado].

## 17/11 00:56 · sessao 6c1461968fb9 · - · - · origem=Ronald (tmux)

1. **Bloqueante: união de travas ainda usa fotografia anterior à trava do colaborador.** O comando monta `universo/elegiveis` fora do `atomic` e guarda as FKs das instâncias; só depois trava C/D/CH/P dessa fotografia (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:37`, `:45`, `:73`, `:77`). A porta chamada no laço relê as FKs e chama novamente `trava_na_ordem` (`app/chamados/services/validacao.py:424`, `:429`). Existe escritor real que troca o destino sob trava do colaborador: `reapontar_resolvedora` (`app/chamados/services/fio.py:936`, `:973`, `:975`). Se essa troca terminar entre a leitura inicial do comando e sua trava, o laço trava um **Chamado novo depois das Perguntas já travadas**. A afirmação “não adquire nada novo” não vale nessa execução (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:75`). Impacto em produção: [nao verificado].

2. **Não bloqueante no cenário estável: a união cobre as quatro tabelas que a porta trava.** A ordem efetiva de `trava_na_ordem` é colaboradores, disputas, chamados, perguntas, cada consulta ordenada por PK (`app/chamados/models.py:2780`, `:2789`, `:2802`, `:2806`). A porta individual trava somente esses mesmos quatro conjuntos (`app/chamados/services/validacao.py:429`). O restante registra `DecisaoPergunta`, atualiza a Pergunta e insere eventos de célula/auditoria; não chama outra porta de trava C/D/CH/P (`app/chamados/services/validacao.py:439`, `:440`, `:446`, `:449`; `app/chamados/models.py:3394`, `:3401`, `:4133`, `:4134`; `app/ponto/portas/celula.py:954`; `app/core/observ.py:133`). INSERTs/FK locks não fazem parte da ordem monitorada pelo rastreador (`app/chamados/tests/test_e1_v9.py:51`; `app/chamados/tests/test_e1_v5_ordem_trava.py:28`).

3. **Bloqueante para a garantia de conjunto exato sob concorrência: ela só é conferida antes da trava.** A comparação com a lista fechada acontece uma vez, sobre `pks` da fotografia (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:50`, `:54`). A porta revalida cada alvo e o contador final desfaz tudo se um deles deixar de ser elegível (`app/chamados/services/validacao.py:431`, `:432`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:90`). Entretanto, não existe nova consulta do universo para detectar outra pergunta que se torne elegível entre leitura e execução; o laço só percorre `linhas` antigas (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:57`, `:82`, `:90`). Ocorrência real: [nao verificado].

4. **Teste novo morde a inversão original, mas não prende a concorrência dos itens 1/3.** Ele cria duas disputas distintas e dois destinos antes da chamada; sem a união, a primeira porta chega à Pergunta antes da segunda Disputa/Chamado, o que o rastreador acusa por rank (`app/chamados/tests/test_f1nova_b2_leitores.py:299`, `:300`, `:301`, `:307`; `app/chamados/tests/test_e1_v9.py:113`). O rastreador observa consultas/UPDATEs reais e elimina somente linhas repetidas, portanto testa comportamento, não a forma da chamada (`app/chamados/tests/test_e1_v9.py:71`, `:78`, `:82`, `:87`). A fixture usa um colaborador e não altera FKs/lista entre leitura e trava (`app/chamados/tests/test_f1nova_b2_leitores.py:303`, `:306`, `:308`). RED/GREEN executado: [nao verificado].

5. **Rollback da porta/lote está preservado por estrutura.** Falta de UPDATE ou de auditoria levanta `TrilhaNaoGravada` dentro do `atomic` individual (`app/chamados/services/validacao.py:428`, `:444`, `:455`). O comando captura a exceção, mas exige duas devoluções e levanta `CommandError` dentro do `atomic` externo quando não obtém ambas, desfazendo também a outra devolução (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:73`, `:85`, `:90`). O arquivo de reversão permanece fora desse rollback e é uma fotografia anterior às travas (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:57`, `:65`, `:73`).

## 17/11 00:16 · sessao d624a857caf2 · EV-NA-BRECHA-1611 · sonda · origem=Code

**Não conclui:** a saída fornecida registra erro de envelope V0 e zero casos lidos. [nao verificado]

Não reexecutei a porta: ela cria e substitui arquivos, contrariando a restrição de somente leitura. [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/6ce96a55e348c14734c286ff5785ebf8c8a13336/bin/sonda_leitura.sh:126) [bin/sonda_leitura.sh:134](/home/ronald/codex_ws/6ce96a55e348c14734c286ff5785ebf8c8a13336/bin/sonda_leitura.sh:134)

Saída crua **fornecida no pedido**, sem nova execução: [nao verificado]

```text
sonda_leitura: i2_sonda.py [305bd1279efde7d3] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor

{"evento": "ERRO", "base": "520d7b55720888d80b46ffbca2ca73e59da40277", "fonte": "[nao verificado]", "etapa": "sonda", "mensagem": "RuntimeError: esta sonda pede o envelope V0 de 16/11/2026"}
{"evento": "VEREDITO", "base": "520d7b55720888d80b46ffbca2ca73e59da40277", "fonte": "[nao verificado ate executar no envelope V0]", "casos_lidos": 0, "total": 12, "donos_condicionais": [], "sem_dono": ["col369 2026-10-12", "col864 2026-10-12", "col114 2026-11-08", "col114 2026-11-15", "col723 2026-10-08", "col746 2026-09-24", "col876 2026-09-21", "col130 2026-10-29", "col220 2026-11-10", "col868 2026-09-22", "col516 2026-10-23", "col884 2026-11-08"], "veredito": "erros: 1"}
rc_sombra=0
```

“Sem dono” abaixo significa **não apurado pela sonda**; nenhum retorno de `InsumosDoDono.dono` foi demonstrado pelo dado fornecido. [nao verificado]

| Caso | Dono e motivo |
|---|---|
| col369 2026-10-12 | sem dono — erro de envelope V0; caso não lido. [nao verificado] |
| col864 2026-10-12 | sem dono — erro de envelope V0; caso não lido. [nao verificado] |
| col114 2026-11-08 | sem dono — erro de envelope V0; caso não lido. [nao verificado] |
| col114 2026-11-15 | sem dono — erro de envelope V0; caso não lido. [nao verificado] |
| col723 2026-10-08 | sem dono — erro de envelope V0; caso não lido. [nao verificado] |
| col746 2026-09-24 | sem dono — erro de envelope V0; caso não lido. [nao verificado] |
| col876 2026-09-21 | sem dono — erro de envelope V0; caso não lido. [nao verificado] |
| col130 2026-10-29 | sem dono — erro de envelope V0; caso não lido. [nao verificado] |
| col220 2026-11-10 | sem dono — erro de envelope V0; caso não lido. [nao verificado] |
| col868 2026-09-22 | sem dono — erro de envelope V0; caso não lido. [nao verificado] |
| col516 2026-10-23 | sem dono — erro de envelope V0; caso não lido. [nao verificado] |
| col884 2026-11-08 | sem dono — erro de envelope V0; caso não lido. [nao verificado] |

## 17/11 00:15 · sessao a9646a3a1d74 · EV-NA-BRECHA-1611 · sonda · origem=Code

Não executei novamente: `i1_sonda.py` não foi localizado [nao verificado]. O envelope reescreve arquivos ([bin/sonda_leitura.sh:132]( /home/ronald/codex_ws/6ce96a55e348c14734c286ff5785ebf8c8a13336/bin/sonda_leitura.sh:132)), contrariando sua regra de somente leitura.

Transcrição crua do dado fornecido, sem validação independente [nao verificado]:

```text
NUMERO classe_mesma comp10: 38
COBERTURA comp10 {'sem_realizado': 10727, 'comparados': 7990, 'dias_erro': 3}
NUMERO classe_mesma comp11: 39
COBERTURA comp11 {'sem_realizado': 9207, 'futuros': 2380, 'comparados': 6858}
EXEMPLO col263 2026-09-21 comp10 realizado 549 tela_mesmo_juiz 377
EXEMPLO col263 2026-09-22 comp10 realizado 548 tela_mesmo_juiz 370
EXEMPLO col263 2026-09-23 comp10 realizado 550 tela_mesmo_juiz 370
EXEMPLO col263 2026-09-24 comp10 realizado 551 tela_mesmo_juiz 370
EXEMPLO col263 2026-09-25 comp10 realizado 551 tela_mesmo_juiz 370
EXEMPLO col263 2026-09-26 comp10 realizado 551 tela_mesmo_juiz 377
EXEMPLO col263 2026-09-28 comp10 realizado 535 tela_mesmo_juiz 356
EXEMPLO col263 2026-09-29 comp10 realizado 551 tela_mesmo_juiz 370
EXEMPLO col263 2026-09-30 comp10 realizado 550 tela_mesmo_juiz 374
EXEMPLO col263 2026-10-01 comp10 realizado 540 tela_mesmo_juiz 382
VEREDITO INCOMPLETO [nao verificado] | erros: 3
```

1. col922, 01/11: o dado registra realizado 1382 e tela pelo mesmo juiz 481; diferença de 901 minutos [nao verificado].
2. col922, 04/11: o dado registra realizado 676 e tela pelo mesmo juiz 458; diferença de 218 minutos [nao verificado].
3. comp10: contagem informada de 38 casos, com 7990 dias comparados e 3 dias com erro [nao verificado].
4. comp11: contagem informada de 39 casos, com 6858 dias comparados e 2380 futuros [nao verificado].
5. Erros informados: `RuntimeError periodo da tela sem turno correspondente neste dia`, col277 em 03/10 e 19/10, col788 em 12/10; veredito incompleto, sem conclusão de reprodução ou total definitivo [nao verificado].

## 17/11 00:08 · sessao 76cb3264c26f · REVISAO-AVAL-C2PREP · verificador · origem=Code

1. **Premissas:** conferem nos registros: oito matrizes existentes e PREPARA anterior com remedidas, “depende CAIU”, classificação de lei e RED; despacho Codex paralelo, sem cópia/suite/trava (app/docs/RELATO.md:314,16–19,23). Conteúdo integral dos briefs, matrizes e logs: [nao verificado].

2. **Colisão:** nenhuma demonstrada para esta extensão de leitura; os cortes de F5/F6/F8 permanecem aplicáveis (app/docs/CORTES.md:488). A exceção nominal F2 sem esperar CLASSE-D consta como CONFERE (app/docs/RELATO.md:4). Ressalva: R1 da CLASSE-D foi separado para andar sem LU2-07/08; portanto a pergunta aberta não bloqueia toda a CLASSE-D (app/docs/RELATO.md:421). LU2-07 permanece aberta e LU2-08 parcialmente aberta (app/docs/RELATO.md:354,352).

3. **Literal:** nenhuma condição impossível demonstrada. O item 3 estabelece um gatilho ainda pendente: a volta 2 consta “rodando”; seu fechamento é [nao verificado] (app/docs/RELATO.md:19). i1/i2 já constam em curso, compatível com preservá-los (app/docs/RELATO.md:12). HEAD futuro, cruzamentos novos e reconferência das oito remedidas: [nao verificado].

4. **Exportada, dinheiro ou `!`:** nenhuma extrapolação demonstrada no escopo de leitura, que tem precedente sem construção (app/docs/RELATO.md:23; bin/codex_pergunta.sh:425). Permanecem os limites de kernel, exportada e `apply` para execução posterior (app/docs/RELATO.md:473; CLAUDE.md:265–268).

VEREDITO: CONFERE (app/docs/RELATO.md:4,23; app/docs/CORTES.md:488).

## 17/11 00:07 · sessao 234c5d704cdd · - · - · origem=Ronald (tmux)

- As cinco fatias existem no recorte E4c; o registro enumera nove fatias, com F3/F5/F6/F8 no grupo posterior à F2 e F9 no final. A preparação antecipada não equivale a alterar essa ordem de construção (`app/docs/RELATO.md:830`).
- O PREPARA anterior já separa leitura de construção: Codex, paralelo, sondas na volta 2, sem cópia, suite ou trava (`app/docs/RELATO.md:23`). O wrapper executa leitores em sandbox somente leitura e não ocupa vaga de construtor (`bin/codex_pergunta.sh:75`, `bin/codex_pergunta.sh:425`).
- As matrizes das oito fatias estavam registradas; as perguntas antigas de F5/F6/F8 receberam cortes, que o novo censo deve conservar (`app/docs/RELATO.md:314`, `app/docs/CORTES.md:488`).
- Os riscos futuros estão declarados: F3 exige `!` se atingir aceite de folga ou gravado exportado; F5 passa pelo kernel do reconciliador; F6 requer smoke; F8 passa pelo kernel de `flip_auto`; F9 permanece sem ato enquanto não rodar `apply` (`app/docs/RELATO.md:473`). Só registrar e ler não autoriza esses atos, cujo limite geral permanece em `CLAUDE.md:265`.
- A neutralidade das batidas na F8 ainda exige medida na sombra; o corte não permite trocar a expectativa por resultado presumido (`app/docs/CORTES.md:488`). Execução dessas sondas, REDs e cruzamentos novos: `[nao verificado]`.

Não identifiquei extrapolação de exportada/dinheiro/`!` no escopo literal de preparação por leitura; as restrições citadas continuam exigíveis na construção e no pouso (`app/docs/RELATO.md:473`, `CLAUDE.md:265`).

## 17/11 00:07 · sessao 933497f8c4c6 · - · - · origem=Ronald (tmux)

Para o COMPLEMENTO-2, não identifiquei colisão procedente:

- A exceção nominal da F2 sem esperar CLASSE-D já consta como revisão CONFERE em `app/docs/RELATO.md:4`; portanto a ordem antiga de `app/docs/BACKLOG.md:1990` e `app/docs/RELATO.md:373` não impede essa abertura.
- Ressalva de precisão: R1 da CLASSE-D foi desenhada para pousar sozinha sem esperar LU2-07/08 (`app/docs/RELATO.md:421`). Não afirmar que toda CLASSE-D depende dessas respostas. O C2, limitado à leitura, não altera esse desenho.
- A R1 permanece identificável no código: C5 agrega dias em uma assinatura por colaborador (`app/escala/services/cadastro_realidade.py:176`); C5 segue fora de `DONO_PELO_JUIZ` (`app/escala/services/cadastro_realidade.py:968`).
- LU2-08 está parcialmente decidida e parcialmente aberta: PARCIAL_SEM_PERIODO é CADASTRO; FATO_EM_AUSENCIA espera corte (`app/docs/RELATO.md:352`). LU2-07 distingue emp1, L-124 e assinatura datada C5, sem resolver tudo por uma única decisão (`app/docs/RELATO.md:354`).
- O brief completo e os logs mencionados não estão disponíveis nesta cópia; conteúdo além dos registros acima é `[nao verificado]`.

Conclusão desta subanálise: **CONFERE para o C2**, sem importar como bloqueio uma colisão de ordem já resolvida nominalmente (`app/docs/RELATO.md:4`).

## 17/11 00:07 · sessao a70e7bf2a2da · - · - · origem=Ronald (tmux)

Leitura literal do COMPLEMENTO-2:

- Formato e precedente conferem nos registros: três remedidas por fatia, base 520d7b557, contagem “depende CAIU”, lei, RED e extras em `app/docs/RELATO.md:16-19`; despacho Codex paralelo, sem cópia/suíte/trava em `app/docs/RELATO.md:23`; resumo das seis partes em `app/docs/BACKLOG.md:822`.
- As oito matrizes F2–F9 estão registradas como fechadas em `app/docs/RELATO.md:314`; três anteriores + cinco novas = oito, sem erro aritmético.
- A volta 2 anterior consta **rodando**, sem fecho comprovado: `app/docs/RELATO.md:19`. Logo item 3 define portão futuro; não autoriza afirmar que já abriu.
- i1/i2 estão registrados em curso em `app/docs/RELATO.md:12`, após despacho em `app/docs/RELATO.md:21`. Preservá-los não colide com o registro.
- Arquivos originais `logs/ESPINHA-BRIEFS/*/{brief,matriz,remedida}.md`, HEAD do main no despacho futuro e execução da reconferência do item 6: **[nao verificado]**. Não estão disponíveis no snapshot.
- Não achei condição literalmente impossível no complemento 2. A reconferência “no pouso da B2” pressupõe disponibilidade das oito entregas naquele marco, mas a ordem futura efetiva é **[nao verificado]**; não constitui colisão demonstrada. [nao verificado]

Ponto substantivo para consolidação: a justificativa do complemento 1 não representa todo o brief CLASSE-D. O registro diz que **R1 pousa sozinho entre F1 e F2 sem esperar LU2-07/08**, embora outros ramos esperem desenho: `app/docs/RELATO.md:421`. A exceção nominal pode mudar a ordem; não se deve concluir que LU2-07 bloqueia a CLASSE-D inteira.

## 17/11 00:01 · sessao f0af060a5ee8 · REVISAO-AVAL-LU207 · verificador · origem=Code

1. **Premissas do CORTE-LU2-07-EMPRESA-1 conferem no código:** `em_rollout` significa “Empresa em produção”, e a tela diz “Em operação” (`app/colaboradores/models.py:45`; `app/templates/colaboradores/empresa_form.html:35`). O vigia usa `empresas_visiveis()`, que não filtra esse campo; o cron mantém `(2,3,4)` literal (`app/chamados/services/vigia_fio.py:517`; `app/colaboradores/queries.py:85`; `app/config/crons.py:17`). Os 7 colaboradores, 3 ativos e valores atuais das flags constam apenas do anexo [nao verificado].

2. **Não identifiquei colisão do novo aval com o corte vigente.** A pergunta aberta é precisamente se emp1 opera (`app/docs/RELATO.md:354`). O anexo anterior foi sucedido pela REEMISSAO-1215; não autoriza reaplicar seus cortes: LU2-08 registra agora 21 aprovadas, zero sem aprovação, com parte ainda aberta (`app/docs/RELATO.md:341`; `app/docs/RELATO.md:352`). Permanecem D1/D2 e o complemento LU2-08; R1 já pode seguir independentemente dessas LU2 (`app/docs/RELATO.md:421`; `app/docs/RELATO.md:422`). A F2 também já foi liberada da espera pela CLASSE-D (`app/docs/RELATO.md:4`).

3. **Nenhuma condição impossível no literal localizada.** Os 46 são subconjunto: o alcance documentado de emp1 é 80 dias, portanto não se pode prometer redução total de exatamente 46 (`app/docs/RELATO.md:448`). A recontagem pode usar `censo_alcance`, que percorre empresa, competência e dia retido (`app/chamados/services/vigia_fio.py:487`; `app/chamados/services/vigia_fio.py:534`). Censo prévio, recontagem atual e brief integral: [nao verificado].

4. **O recorte do vigia comporta mudar o alarme sem relavrar dinheiro:** a função declara nenhuma escrita nem recálculo (`app/chamados/services/vigia_fio.py:490`). Alterar globalmente `empresas_visiveis()` ampliaria o alcance ao lavrador do retrato da folha; isso ultrapassaria a alteração localizada no vigia (`app/folha/management/commands/lavrar_retrato_folha.py:51`). Não há necessidade demonstrada de tocar exportada, dinheiro ou `!` para esse recorte de leitura (`app/chamados/services/vigia_fio.py:490`).

VEREDITO: CONFERE

## 17/11 00:01 · sessao 472843b72c27 · - · - · origem=Ronald (tmux)

- O novo aval preserva as pendências do brief: D1 (ramo C, 18 dias), D2 (ramo B, 26) e o complemento LU2-08; resolver LU2-07 não libera automaticamente toda a CLASSE-D. R1 já foi declarado independente de LU2-07/08. Referências: `app/docs/RELATO.md:421`, `app/docs/RELATO.md:422`. O brief literal indicado em `logs/CLASSE-D/brief/brief.md` não está disponível nesta cópia [nao verificado].

- O anexo antigo CORTES-LU2-07-08-10 foi superado pela REEMISSAO-1215; LU2-07/08 permaneceram abertas e LU2-10 ficou respondida como fato, sem ação. Portanto, não usar o anexo antigo como autorização vigente para retirar os 22 CADASTRO nem para concluir genericamente que toda ausência aprovada elimina retenção. Referências: `app/docs/RELATO.md:341`, `app/docs/RELATO.md:343`.

- LU2-08 atual distingue 10 PARCIAL_SEM_PERIODO, já decididos como CADASTRO, de 10 FATO_EM_AUSENCIA ainda esperando corte; contabiliza 21 com ausência aprovada, zero sem, e dos 38 “tarefa nada” apenas nove pertencem aos 21. Isso contraria a descrição antiga “19 aprovados + 2 sem aprovação”, mas o novo aval não a reaplica. Referências: `app/docs/RELATO.md:352`, `app/docs/RELATO.md:422`.

- O campo citado existe e diz literalmente “Empresa em producao no Hasner (piloto/rollout)”. O código também define colaboradores em operação usando empresa com `em_rollout=True`, além das condições próprias do colaborador. Referências: `app/colaboradores/models.py:45`, `app/colaboradores/queries.py:50`, `app/colaboradores/queries.py:65`.

- O vigia realmente usa `empresas_visiveis`, uma função declarada como fonte de UI. O censo soma dias retidos por empresa e competência, sem recalcular folha. Portanto, excluir empresa do universo do alarme não remove a retenção da folha nem altera os dados empresariais por essa leitura. Referências: `app/colaboradores/queries.py:78`, `app/chamados/services/vigia_fio.py:490`, `app/chamados/services/vigia_fio.py:517`, `app/chamados/services/vigia_fio.py:534`.

- Os 46 são subconjunto dos 177; o alcance total documentado da empresa 1 é 80 dias. O aval diz que os 46 sairão, sem prometer que serão os únicos; a recontagem pela função real antes da construção continua necessária. Referências: `app/docs/RELATO.md:354`, `app/docs/RELATO.md:448`. A futura contagem efetiva não foi executada [nao verificado].

- O alcance do censo usa competência atual e anterior quando ainda não entregue; a alteração proposta no vigia não implica relavrar `DiaPago`, `FechamentoMensal` nem TXT. Referências: `app/chamados/services/vigia_fio.py:535`, `app/ponto/services/he_pendente_lavrado.py:209`, `app/ponto/services/he_pendente_lavrado.py:223`.

Não encontrei condição adicional procedente no novo aval dentro deste recorte; a restrição “só registrar agora” e as pendências preservadas impedem tratá-lo como autorização imediata de construção. Referências: `app/docs/RELATO.md:421`, `app/docs/RELATO.md:422`; texto do novo aval fornecido pelo usuário [nao verificado em arquivo].

## 17/11 00:00 · sessao 68162d6ca69e · - · - · origem=Ronald (tmux)

- Premissa de cadastro confere: `Empresa.em_rollout` significa empresa em produção; a tela o apresenta como “Em operacao (rollout)” e o grava pela porta existente (`app/colaboradores/models.py:45`; `app/templates/colaboradores/empresa_form.html:34`; `app/colaboradores/views.py:513`; `app/colaboradores/views.py:539`; `app/colaboradores/services/estrutura.py:18`).
- `empresas_visiveis()` define universo de UI, filtrando atividade e diagnóstico; não consulta `em_rollout`. O vigia de alcance e a lista do admin consultam essa função (`app/colaboradores/queries.py:78`; `app/chamados/services/vigia_fio.py:189`; `app/chamados/services/vigia_fio.py:517`).
- O terceiro sítio literal existe: `EMPRESAS = (2, 3, 4)` e seu comentário o denomina universo em operação (`app/config/crons.py:17`).
- Os números atuais — sete colaboradores, três ativos, flags nas quatro empresas — dependem exclusivamente do anexo [nao verificado]. O registro anterior declara dois ativos, 80 dias de alcance e 46 dos 177; portanto o efeito total da futura mudança de universo não se resume automaticamente aos 46 (`app/docs/RELATO.md:448`).
- A recontagem exigida antes de construir é pertinente: `censo_alcance` percorre os dias retidos por empresa e competência; a função declara nenhuma escrita nem recálculo (`app/chamados/services/vigia_fio.py:487`; `app/chamados/services/vigia_fio.py:529`).
- A futura alteração deve atingir o consumidor pretendido. Mudar globalmente `empresas_visiveis()` também alcançaria retrato e outros contadores, que consultam a mesma função (`app/folha/management/commands/lavrar_retrato_folha.py:51`; `app/folha/vigia_retido_calado.py:54`; `app/folha/retrato_folha.py:1411`).
- Não identifiquei impossibilidade literal ou contradição semântica no primeiro aval: a unidade empresa corresponde ao campo; `colaboradores_em_operacao()` acrescenta escala vigente apenas para a unidade colaborador (`app/colaboradores/models.py:45`; `app/colaboradores/queries.py:50`).

## 17/11 00:00 · sessao 0d96f86729be · - · - · origem=Ronald (tmux)

- O aval anterior `CORTES-LU2-07-08-10` foi superado pela `REEMISSAO-1215`: LU2-07/08 voltaram abertas ao leitor; LU2-10 ficou respondida como fato, sem ação. Não usar o anexo antigo como corte vigente. `app/docs/RELATO.md:341`, `app/docs/RELATO.md:343`.
- LU2-07 vigente pergunta exatamente “emp1 está em operação?” e registra os 46 dias; o novo aval responde essa pergunta. Não responde o restante dos 155: 79 sob L-124, 42 emp1 e 34 retidos corretamente por ausência de assinatura datada no C5. `app/docs/RELATO.md:354`.
- Há diferença documental nos ativos: registro anterior diz **2 ativos**; novo dado diz **3 ativos**. Atualização entre sondas ou erro [nao verificado]; não tratar a diferença como contradição provada. `app/docs/RELATO.md:448`.
- O antigo LU2-08 não confere com a consolidação vigente: **21 aprovadas, zero sem aprovação**, dez PARCIAL_SEM_PERIODO = CADASTRO, dez FATO_EM_AUSENCIA ainda abertas; dos 38 sem tarefa, nove pertencem aos 21 e 29 não têm ausência. `app/docs/RELATO.md:352`.
- A lei vigente exige parcial sem período = CADASTRO, nunca FATO_EM_AUSENCIA. A referência `CORTES.md:370` no RELATO envelheceu; endereço atual **`app/docs/CORTES.md:386`**.
- O brief resumido contém mais dependências: R1/C5 diário, InsumosDoDono, contador tela/PDF, D1 ramo C18 e D2 ramo B26. A expressão do novo aval “só o que mais estiver aberto no brief” preserva essas pendências; seria incorreto resumir para “só LU2-08”. `app/docs/RELATO.md:421`, `app/docs/RELATO.md:422`.
- A ordem antiga “CLASSE-D antes da F2” continua no BACKLOG, mas foi expressamente superada quanto à espera: complemento PREPARA 23:5x permite F2 sem esperar CLASSE-D. Novo aval apenas “segue a fila” não colide com essa ordem atual. `app/docs/BACKLOG.md:1990`, `app/docs/RELATO.md:4`.
- L-006 dá suporte a cadastro pela UI; L-099 separa ESTRUTURA de CADASTRO/BATIDA. Nenhuma dessas linhas autoriza reaplicar o corte antigo superado nem extinguir as dependências restantes. `app/docs/LEIS.md:64`, `app/docs/LEIS.md:122`, `app/docs/RELATO.md:341`, `app/docs/RELATO.md:421`.
- Arquivos primários `logs/AVAIS-*`, `logs/CLASSE-D/brief/brief.md` e entregas referenciadas não estão disponíveis nesta cópia; conteúdo primário [nao verificado]. O resumo do brief existe em `app/docs/RELATO.md:421`.

## 16/11 23:54 · sessao 0f433dc81e6f · REVISAO-AVAL-2355 · verificador · origem=Code

1. **Premissas: conferem como mudança a construir.** K6 hoje exclui U163; o segundo laço do comando também exclui U163 e imprime uma lista textual `PAUTA DP`. Incluir esse comando no escopo resolve a lacuna da lista, sem criar `Pauta`. (`app/chamados/services/materializacao.py:795`; `app/chamados/management/commands/bug_par_103.py:72`; `app/chamados/management/commands/bug_par_103.py:85`)

   Os baldes entregue/sem dia existem, mas atualmente atendem ao ramo materializado elegível U103; uma U163 não materializada bloqueada cai em `outras` e continua em `pendentes_aval`. O complemento define o destino antes faltante; a implementação deverá conservar `pendentes_aval + resolvidas + entregue + sem_dia = congelados_no_recorte`. (`app/chamados/services/materializacao.py:316`; `app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:347`; `app/chamados/services/materializacao.py:356`)

2. **Colisões: há sucessão explícita da exceção anterior.** O registro anterior mantém U163 entregue sob F6, com aberto seguindo; o corte U163-NO-K6 já está registrado como ampliação para L1/L5, complemento da F4 sobre B2. A exceção e a matriz precisam acompanhar essa sucessão. (`app/docs/RELATO.md:44`; `app/docs/RELATO.md:52`; `app/docs/BACKLOG.md:823`; `app/docs/RELATO.md:21`)

   Continua vigente a condição de pousar F4 logo atrás de F1/F2 **ou** manter a costura J1 sob contador esperado zero no intervalo. (`app/docs/CORTES.md:487`)

3. **Literal: PROCEDE quanto ao RED.** “Irmã também U163 ou sem irmã” elimina o contraexemplo do K6 da irmã; o controle fora163 entregue confere. (`app/chamados/services/validacao.py:224`; `app/chamados/services/materializacao.py:795`; `app/chamados/tests/test_bug_par_volta.py:145`)

   Entretanto, isso ainda não garante **“hoje valida e grava”**: declarado diferente do gravado continua bloqueado pela espera U163; sem irmã, uma vizinha do mesmo tipo pode impedir a paridade; uma batida aprovada existente produz `ja_satisfeita`, sem nova batida. (`app/chamados/services/validacao.py:609`; `app/chamados/services/materializacao.py:936`; `app/chamados/services/materializacao.py:903`; `app/chamados/services/materializacao.py:928`; `app/chamados/services/materializacao.py:898`)

   O RED precisa fixar uma metade respondida, sem validação/materialização anterior, declarado igual ao gravado e plantio permitido pelas demais guardas, demonstrando criação de batida antes da mudança. (`app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:597`; `app/chamados/services/validacao.py:615`; `app/chamados/services/validacao.py:681`; `app/chamados/services/materializacao.py:891`; `app/chamados/services/materializacao.py:914`) A comprovação desse caso em produção permanece [nao verificado].

4. **Exportada, dinheiro e `!`: não aparece ampliação além do bloqueio declarado.** A guarda atua antes do carimbo/recalculo no admin e antes das escritas do materializador. (`app/chamados/services/validacao.py:652`; `app/chamados/services/validacao.py:655`; `app/chamados/services/validacao.py:666`; `app/chamados/services/materializacao.py:1829`) O recalculo herdado já está registrado; alterar `FechamentoMensal` exportado continua sob `!`. (`app/docs/RELATO.md:52`; `CLAUDE.md:411`) Impacto monetário efetivo: [nao verificado].

**VEREDITO: PROCEDE (item 3: a condição corrigida da irmã ainda não garante “hoje valida e grava”; declarado × gravado pode bloquear antes — `app/chamados/services/validacao.py:609`).**

## 16/11 23:54 · sessao c83d223213b2 · E4C-F1-B2 · verificador · origem=Code

**Parecer: devolver a volta 2 para correção.** Há três problemas no delta: aplicação de pergunta sem resposta no materializador, apagamento de texto sem preservação na trilha e aquisição de travas fora da ordem no lote. (`app/chamados/services/materializacao.py:2072`; `app/chamados/models.py:1629`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:73`)

A cópia não contém metadados Git acessíveis; HEAD e comparação independente com `086f13522` ficaram **[nao verificado]**. Considerei como delta o patch anexado. Não há, nos dados fornecidos, linhas de produção que permitam medir os casos abaixo; incidência e quantidade reais são **[nao verificado]**.

Os três achados são:

- **Materialização aplica uma `escolha` com I sem T.** A seleção ampliada admite I sem T de qualquer tipo, desde que validada e não materializada. A recusa por C21 existe no ramo `hora`; o ramo `escolha` reconcilia destino/clones e retorna sucesso sem exigir resposta. O chamador dispensa o juiz de lastro para esse tipo e solicita o carimbo. (`app/chamados/services/materializacao.py:2071`; `app/chamados/services/materializacao.py:1315`; `app/chamados/services/materializacao.py:1643`; `app/chamados/services/materializacao.py:1903`; `app/chamados/services/materializacao.py:1948`) A validação faz C6 considerar a pergunta morta, permitindo gravar materialização/via sem corrigir o par ilegal. (`app/chamados/juizes.py:1405`; `app/chamados/models.py:3241`; `app/chamados/models.py:3263`)

- **Ressuscitação pode apagar T sem I sem guardar o texto anterior.** `q_nao_respondida()` passou a incluir esse legado no universo; a porta posteriormente zera o texto. Contudo, `linhas_da_projecao_sem_linha()` descarta perguntas sem instante e sem validação; o log posterior guarda apenas IDs e estados. Assim, para legado sem outra trilha, o texto desaparece, contrariando sua preservação exigida pela L-130. (`app/chamados/models.py:1533`; `app/chamados/models.py:1613`; `app/chamados/models.py:1629`; `app/chamados/models.py:4095`; `app/chamados/models.py:1656`; `app/docs/LEIS.md:151`)

- **O `atomic` externo corrigiu a atomicidade, mas introduziu ordem de trava incorreta entre itens.** O comando mantém os dois atos na mesma transação, enquanto cada chamada trava somente C/D/Ch/P daquele item. Para disputas distintas, fica `C → D1 → P1 → D2 → P2`, adquirindo D2 depois de P1. O próprio teste cria uma disputa para cada alvo. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:73`; `app/chamados/services/validacao.py:429`; `app/chamados/models.py:2779`; `app/chamados/models.py:2796`; `app/chamados/tests/test_f1nova_b2_leitores.py:199`; `app/chamados/tests/test_f1nova_b2_leitores.py:208`) A ordem declarada exige travar o conjunto antes da primeira pergunta. Donos/disputas reais dos dois PKs e ocorrência de deadlock: **[nao verificado]**. (`app/chamados/models.py:2736`; `app/chamados/models.py:2759`)

**1. Os leitores migrados usam o mesmo significado de respondida, mas isso não garante que todos os efeitos respeitem esse significado.** C21 exige instante e texto após `strip()`. As faces SQL usam a classe de brancos derivada de `str.isspace()`, incluindo NBSP; a negativa explicita os dois ramos. (`app/chamados/juizes.py:1528`; `app/chamados/juizes.py:1552`; `app/chamados/juizes.py:1560`; `app/chamados/juizes.py:1580`; `app/chamados/juizes.py:1590`)

Os escritores dos valores consumidos nessa cadeia são:

| Valor | Quem escreve |
|---|---|
| `respondida_em` e `resposta_colab` na resposta humana | `responder_pergunta_disputa_core`: valida entrada, atribui ambos e grava resposta/trilha. (`app/chamados/services/disputa_emissao.py:1446`; `app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1522`) |
| Par na resposta automática | `responder_automaticamente`, após consultar a porta de integridade. (`app/chamados/services/disputa_emissao.py:1672`; `app/chamados/services/disputa_emissao.py:1674`) |
| Limpeza do par, materialização e `via_resolucao` | `descarimbar_em_lote`, chamada pela ressuscitação e pela devolução do estoque. (`app/chamados/models.py:3392`; `app/chamados/models.py:1629`; `app/chamados/services/validacao.py:440`) |
| Materialização e `via_resolucao` | Portas do carimbo; a atribuição central fica em `_escrever_carimbo`. (`app/chamados/models.py:3208`; `app/chamados/models.py:3214`) |
| Status do chamado | `_gravar_estado`, alcançado pelos gates de reconciliar, reabrir e encerrar. (`app/chamados/models.py:201`; `app/chamados/models.py:228`; `app/chamados/models.py:1681`) |
| Fechamento da disputa | `DisputaSupervisao.fechar`, gravando instante, autor, resolução e motivo juntos. (`app/chamados/models.py:2013`) |
| Pauta de recusa de reabertura | `_escrever_pautas`, pela porta `pautas.services.escrever`, agrupando pela competência do dia do fato. (`app/chamados/services/fecho_runtime.py:118`; `app/chamados/services/fecho_runtime.py:139`) |

Para cada sítio de produção tocado pelo delta:

| Sítio | Leitura e consequência |
|---|---|
| `api_disputa_perguntas` | Lê I/T por C21 para `respondidas`; também serializa o par cru. Não grava esses campos. (`app/api/views.py:2561`; `app/api/views.py:2578`) |
| `api_disputa_responder` | Lê C21 para distinguir resposta/correção; entrega a escrita ao core acima. (`app/api/views.py:2622`; `app/api/views.py:2629`) |
| `canal.perguntas_mudas_com_resposta` | Lê a face negativa e cruza com respostas do fio; produz diagnóstico, sem copiar texto para a pergunta. (`app/chamados/canal.py:64`; `app/chamados/canal.py:76`) |
| `juizes.q_nao_respondida` | Define a face SQL negativa; não escreve. (`app/chamados/juizes.py:1583`) |
| Auditor: contraprova I21a | Lê resposta por C21 e cobertura de ausência; produz alarme. (`app/chamados/management/commands/auditar_invariantes_chamados.py:176`) |
| Auditor: I2 | Lê materialização, status terminal do destino, C21 e fechamento da disputa para separar piso/violação. (`app/chamados/management/commands/auditar_invariantes_chamados.py:588`) |
| Auditor: F1 | Lê status vivo, vitalidade e C21 para classificar fio mudo. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1632`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1658`) |
| Comando de estoque | Seleciona I sem T; lê via/par para reversão e delega a escrita à porta. DRY retorna antes da escrita. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:37`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:51`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:76`) |
| `_universo_da_ressuscitacao` | Lê materialização, face negativa e vias fora do fato; projeta o estado limpo antes de perguntar C6. A ressuscitação efetiva limpa par/carimbo e reabre a disputa. (`app/chamados/models.py:1530`; `app/chamados/models.py:1553`; `app/chamados/models.py:1629`; `app/chamados/models.py:1633`) |
| `acao_regularizar_disputa` | Lê C21 nas vivas; bloqueia sem resposta ou chama o materializador. (`app/chamados/services/acoes_disputa.py:134`; `app/chamados/services/acoes_disputa.py:143`) |
| `becos.classificar` | Lê C21, status do pai/destino e C7. A classificação não fecha; `fechar_beco` executa as portas. (`app/chamados/services/becos.py:30`; `app/chamados/services/becos.py:54`) |
| `declarar_hora_colab` | Lê C21 e igualdade do conteúdo; entrega resposta nova ao core. (`app/chamados/services/declaracao_texto.py:418`; `app/chamados/services/declaracao_texto.py:420`) |
| `aplicar_declaracao` | Lê C21 para decidir se precisa registrar resposta pela porta canônica. (`app/chamados/services/declaracao_texto.py:688`) |
| `reabrir_credor_de_pergunta_viva` | Lê C21, C6, status da âncora, competência e limbo; reabre pela porta ou registra recusa. (`app/chamados/services/disputa_emissao.py:1843`; `app/chamados/services/disputa_emissao.py:1857`; `app/chamados/services/disputa_emissao.py:1873`) |
| `processar_post_fio` | Lê C21 nas vivas para liberar regularização; chama materialização. (`app/chamados/services/fio.py:707`; `app/chamados/services/fio.py:717`) |
| Materialização da disputa | Lê as duas faces, validação e materialização. Pode escrever carimbo/via e reconciliar chamados; contém o primeiro achado. (`app/chamados/services/materializacao.py:2071`; `app/chamados/services/materializacao.py:2092`) |
| `particao_do_app` | Lê C21 e fechamento da disputa; somente particiona pendentes/histórico. (`app/chamados/services/pergunta_no_app.py:82`) |
| `impedimento_estoque_l130` | Lê integridade, validação, materialização, fora do fato, C6 e competência entregue; somente seleciona. (`app/chamados/services/validacao.py:390`) |
| `devolver_instante_sem_texto_l130` | Relê o impedimento sob trava; preserva projeção anterior, limpa pela porta do lote e exige trilha. (`app/chamados/services/validacao.py:429`; `app/chamados/services/validacao.py:436`; `app/chamados/services/validacao.py:455`) |
| `validar_pergunta` / `recusar_pergunta` | Leem C21 antes de validar/materializar ou devolver pela reabertura. (`app/chamados/services/validacao.py:682`; `app/chamados/services/validacao.py:743`; `app/chamados/services/validacao.py:902`; `app/chamados/services/validacao.py:925`) |
| `placar_aceite` | Lê face positiva e instante para contagem temporal; não grava resposta/status. (`app/chamados/services/verbos.py:333`; `app/chamados/services/verbos.py:344`) |
| `vigia_fio.censo_alcance` | Lê integridade, C21 e status terminal da âncora para nomear causas. (`app/chamados/services/vigia_fio.py:634`) |
| View de resposta | Lê C21 e entrega a escrita ao core. (`app/chamados/views.py:1280`; `app/chamados/views.py:1283`) |
| `validar_inline` | Seleciona vivas respondidas por C21 e chama a ação de validação. (`app/chamados/views_cobrar.py:170`; `app/chamados/views_cobrar.py:210`) |
| `ficha_do_colab` | Lê a lista sem veredito e C21 para apresentar quem deve agir. (`app/colaboradores/services/ficha.py:169`; `app/colaboradores/services/ficha.py:172`; `app/colaboradores/services/ficha.py:191`) |

A property atual **delega**, sem copiar a expressão. Entretanto, a seleção ampliada do materializador permite aplicar algo que o próprio juiz considera não respondido. (`app/chamados/models.py:3665`; `app/chamados/services/materializacao.py:2072`)

**2. Restaram decisões próprias de produção fora de C21.** Encontrei duas que precisam migrar ou mudar explicitamente de finalidade:

| Sítio | Classificação |
|---|---|
| `espera_o_texto_do_colaborador` | **Deve migrar.** Reimplementa `não respondida` como ausência de I ou I sem T. O resultado é equivalente hoje, inclusive no trim, mas continua sendo outro corpo decisor. Alimenta confirmação de envio, correção e cobrança. (`app/chamados/services/fio.py:187`; `app/chamados/services/fio.py:359`; `app/chamados/services/fio.py:386`; `app/chamados/services/cobranca.py:241`) |
| Card vivo de hora: “Você respondeu… escolha de novo para corrigir” | **Deve migrar ou rotular conteúdo legado.** A frase depende só de texto truthy; T sem I pode aparecer como respondida enquanto o botão continua “Enviar resposta”. (`app/templates/chamados/partials/_card_atendimento.html:141`; `app/templates/chamados/partials/_card_atendimento.html:161`; `app/chamados/services/fio.py:359`) |

Os demais usos diretos encontrados têm outra pergunta legítima:

| Usos | Classificação |
|---|---|
| Serialização de `respondida_em` | Exposição do cru, sem decidir respondida. (`app/api/views.py:1165`; `app/api/views.py:2563`; `app/api/views_mensageria.py:569`) |
| Ordenação, idade e comparação de instantes | Leitura temporal legítima após seleção própria da finalidade. (`app/chamados/veredito_lavrado.py:207`; `app/chamados/services/painel.py:67`; `app/chamados/services/verbos.py:344`; `app/chamados/services/validacao.py:1256`) |
| Histórico/projeção e backfill de decisões | Reconstrução do ato ocorrido; não devem usar apenas C21 atual. Há, porém, a lacuna de preservação de T sem I apontada acima. (`app/chamados/models.py:4095`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`; `app/chamados/services/historico.py:99`) |
| Construção/conferência do par nas portas | Integridade de escrita, legítima. (`app/chamados/models.py:3241`; `app/chamados/models.py:3371`; `app/chamados/models.py:3586`) |
| Proteção contra sobrescrever texto na resposta automática | Preservação de conteúdo, legítima mesmo para legado sem I. (`app/chamados/services/disputa_emissao.py:1663`) |
| Cura por trilha e bloqueio de limbo na reabertura | Integridade/histórico, não definição geral de respondida. (`app/chamados/services/disputa_emissao.py:1692`; `app/chamados/services/disputa_emissao.py:1861`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`) |
| Censo I sem T | Integridade pelo predicado real, legítima. (`app/chamados/services/vigia_fio.py:747`) |
| `resposta_display`, seletores e resposta no cartão histórico | Exibição/interpretação do conteúdo cru, legítima. (`app/chamados/models.py:3692`; `app/templates/chamados/partials/_card_atendimento.html:114`; `app/templates/chamados/partials/_card_atendimento.html:154`; `app/templates/chamados/partials/_card_atendimento.html:182`) |
| Parsing de hora, escapes e folga | Interpretação do valor, não classificação de respondida. (`app/chamados/services/materializacao.py:625`; `app/chamados/services/materializacao.py:1562`; `app/chamados/services/validacao.py:467`) |
| Guarda de testemunha de aplicação indisponível | Conserva bloqueio de ação quando existe conteúdo a analisar; finalidade distinta de C21. (`app/chamados/services/fio.py:377`; `app/chamados/templatetags/chamados_tags.py:139`; `app/chamados/templatetags/chamados_tags.py:166`) |

O alarme I20 merece ajuste separado: procura texto diferente de `''`, sem trim, portanto também acusa espaços/NBSP sem I como preenchimento. É divergência de **integridade**, não outro leitor geral de respondida. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1106`; `app/chamados/juizes.py:1541`)

A afirmação de ausência absoluta de outros leitores, inclusive formas dinâmicas, fica **[nao verificado]**.

**3. No desligamento, os meios-pares deixam de reter a disputa; o resultado final também depende de vitalidade, tranca e outras perguntas.** A tabela assume pergunta não validada, não materializada e dia não trancado. (`app/chamados/models.py:1923`)

| Par | Retém por resposta sem veredito? | Consequência da cascata |
|---|---|---|
| I e T | Sim. | Pula o fechamento da disputa. (`app/colaboradores/services/desligamento.py:165`) |
| I sem T | Não. | Sem outra resposta legal retendo, a disputa pode fechar pela exceção de desligamento. (`app/chamados/models.py:1925`; `app/chamados/models.py:1999`) |
| T sem I | Não. | Mesma consequência; o legado deixou de ser retenção. (`app/chamados/models.py:1925`; `app/chamados/tests/test_e4b_c12_orm.py:229`) |
| Nenhum | Não. | Segue a porta de encerramento do silêncio. (`app/chamados/models.py:1999`) |

Na exceção, somente as perguntas que C6 considera vivas recebem `admin_decidiu`, linha SILENCIO e auditoria; depois fecha a disputa e tenta encerrar os chamados. Perguntas já mortas não entram nesse laço. (`app/chamados/models.py:1977`; `app/chamados/models.py:2000`; `app/chamados/models.py:2005`; `app/colaboradores/services/desligamento.py:187`)

**I∧T não garante manter aberto um chamado de competência entregue.** A guarda `_vivas_entre` exclui entregue, e `encerrar` não consulta o gate da conversa do pai. Logo pode permanecer disputa aberta com o chamado encerrado nesse recorte. (`app/chamados/models.py:364`; `app/chamados/models.py:1669`)

A Pauta desse caminho nasce de **recusa de reabertura coletada durante o ato**, agrupada pela competência; não nasce automaticamente porque existe uma pergunta exportada. (`app/chamados/services/fecho_runtime.py:118`; `app/chamados/services/fecho_runtime.py:139`; `app/chamados/services/fecho_runtime.py:165`)

Assim, há perda de Pauta no cenário T sem I: agora a disputa fecha antes, a pergunta viva recebe decisão e desaparece a recusa que originava a Pauta. O novo teste exige precisamente fechamento, `admin_decidiu` e zero Pauta. Isso é coerente com a substituição da retenção I OU T por I∧T e com a condição concreta do coletor de Pauta. (`app/chamados/tests/test_e4b_c12_orm.py:229`; `app/chamados/tests/test_e4b_c12_orm.py:232`; `app/chamados/models.py:1925`; `app/chamados/services/fecho_runtime.py:128`)

Isso **não** significa preservação integral de perguntas em entregue: a porta de desligamento pode gravar decisão/carimbo nelas. Preservação de exportações e fechamentos é uma afirmação mais estreita que preservação de perguntas. (`app/chamados/models.py:1999`; `app/chamados/tests/test_e4b_c12_orm.py:230`; `app/chamados/tests/test_e4b_c12_orm.py:233`)

**4. As mudanças dos três testes destacados não têm a mesma natureza.** [nao verificado]

- **`test_e3_degrau_dono_e_porta.py`:** conserva sujeito e assertions de partição, carga, C6 e relógio; acrescenta T à fixture que representa resposta legal. Não identifiquei enfraquecimento dessas assertions. (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:262`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:271`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:289`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:320`)

- **`test_e1_v10.py`:** altera a fixture `_ln` para fornecer I junto de T; mantém as verificações de mérito, partição e retratação. (`app/chamados/tests/test_e1_v10.py:118`; `app/chamados/tests/test_e1_v10.py:152`; `app/chamados/tests/test_e1_v10.py:169`)

- **`test_e4b_c12_orm.py`:** troca efetivamente o cenário geral de desligamento para resposta legal, mas essa resposta adicional está em **competência aberta**. O legado anterior permanece em teste separado. A mudança é compatível com I∧T, porém o `test_RED_desligamento` deixou de provar emissão de Pauta nesse gatilho e não prova retenção de uma resposta ancorada somente em entregue. (`app/chamados/tests/test_e4b_c12_orm.py:57`; `app/chamados/tests/test_e4b_c12_orm.py:148`; `app/chamados/tests/test_e4b_c12_orm.py:174`; `app/chamados/tests/test_e4b_c12_orm.py:207`)

Há **redução objetiva de cobertura** em `test_o261_resposta_vazia`: o estado completo incluía materialização, autor, via, I, validação e T; agora a comparação verifica apenas I/T. O fechamento permitido explica parte da mudança, mas faltam assertions específicas para os demais campos e para o novo fechamento da disputa nesse caso. (`app/chamados/tests/test_o261_resposta_vazia.py:74`; `app/chamados/tests/test_o261_resposta_vazia.py:113`)

`e4_apoio.juiz_respondida()` executa os corpos **reais extraídos da fonte** de `pergunta_respondida` e `texto_vazio`; não reimplementa a regra. É execução isolada desses corpos, não importação integral do módulo Django. (`app/chamados/tests/e4_apoio.py:8`; `app/chamados/tests/e4_apoio.py:19`)

O novo teste sentinela da property melhora a prova: exige delegação, impedindo que uma cópia da expressão passe apenas por produzir valores iguais. (`app/chamados/tests/test_f1nova_b2_leitores.py:87`)

**5. As classes de reprovação aparecem, com estas distinções:**

| Classe | Resultado |
|---|---|
| Segundo juiz/derivação paralela | Restou o decisor de `fio.espera_o_texto_do_colaborador`; o template também mantém uma decisão por texto cru. (`app/chamados/services/fio.py:187`; `app/templates/chamados/partials/_card_atendimento.html:141`) |
| Segundo escritor fora da porta | Os novos caminhos examinados delegam às portas existentes; os defeitos encontrados são de seleção, trilha e ordem, mesmo usando essas portas. (`app/chamados/services/validacao.py:440`; `app/chamados/services/materializacao.py:1948`) |
| Relógio fora da borda | O delta captura `hoje` no comando e o passa à seleção e à porta; não encontrei novo relógio dentro de C21. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:36`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:7

[truncado em 20000 de 23857 caracteres]

## 16/11 23:54 · sessao d698d1708027 · REVISAO-AVAL-2355 · verificador · origem=Code

1. **Premissas:** as quatro condições conferem com a enumeração da BUG-PAR; o complemento agora as atribui expressamente à F2, respondendo à devolução. LU2-07 permanece aberta. A sequência histórica completa até F7 é **[nao verificado]**; BUG-PAR → B2 → F2 → F4 tem respaldo documental. (app/docs/RELATO.md:158; app/docs/RELATO.md:4; app/docs/RELATO.md:349; app/docs/CORTES.md:495; anexo complemento1_prepara_f2_f4_f7_1611.txt:1)

2. **Colisão:** a precedência anterior era F1 → CLASSE-D → F2 → E4d. O complemento a altera nominalmente quanto à F2 e preserva CLASSE-D antes da E4d; portanto resolve a colisão apontada. Também substitui o encaixe técnico anterior de R1 entre F1/F2, que independia de LU2-07/08: esperar LU2-07 passa a ser ordem expressa, não dependência técnica demonstrada. (app/docs/BACKLOG.md:1990; app/docs/RELATO.md:368; app/docs/RELATO.md:416; anexo complemento1_prepara_f2_f4_f7_1611.txt:1)

3. **Literal:** fecha como autorização condicional. B2 consta aberta e “em voo”; seu pouso é **[nao verificado]**. Lei 0 da F2 está registrada na base `520d7b557`; cumprimento das quatro condições no HEAD pós-B2 é **[nao verificado]**. Cortar LU2-07 tampouco comprova fechamento integral da CLASSE-D: LU2-08 conserva parte aberta. São condições ainda por conferir, sem invalidar a resposta à devolução. (app/docs/RELATO.md:25; app/docs/RELATO.md:21; app/docs/RELATO.md:11; app/docs/RELATO.md:347; app/docs/RELATO.md:4)

4. **Exportada, dinheiro ou `!`:** o complemento declara registro e reordenação; não autoriza esses atos adicionais. Permanecem a guarda da F2 sobre exportada — DIFF mudou, PAREI `!` — e a conferência do recorte `_nascer`. Efeitos de execução futura sobre dinheiro ou gravado são **[nao verificado]**. (anexo complemento1_prepara_f2_f4_f7_1611.txt:1; app/docs/CORTES.md:487; app/docs/RELATO.md:468; CLAUDE.md:265)

**VEREDITO: CONFERE**

## 16/11 23:54 · sessao 5c40158a9144 · REVISAO-AVAL-2355 · verificador · origem=Code

1. **Premissas: parcialmente conferidas.** A recusa de `4633d2c17`, as docstrings nos dois arquivos e os 30+ descendentes/B2/AUSÊNCIA estão registrados em `app/docs/RELATO.md:8`. A classificação desses caminhos como negócio confere com `bin/tests/test_lei_akita.sh:152`; eles estão fora do registro de medida (`bin/tests/test_lei_akita.sh:172`). SHA completo, diff histórico, descendência e aceitação efetiva pelo hook: **[não verificado]**.

2. **Colisão: item 2 precisa de ressalva.** A nova exceção nominal segue a forma dos precedentes: crescimento exige novo aval, sem regravar histórico (`app/docs/CORTES.md:449`; `app/docs/CORTES.md:484`). Porém, “n/a medida ou instrumento tocando arquivo de negócio = recusa”, como proibição absoluta por arquivo, elimina a permissão vigente de alterar dados medidos nos módulos registrados: o juiz expressamente admite esse caso (`bin/tests/test_lei_akita.sh:348`; `app/docs/BACKLOG.md:1955`; `app/docs/RELATO.md:2095`). O literal precisa preservar L-123-MEDIDA-EM-PY.

3. **Condições literais: o predicado compartilhado já existe.** O hook chama o mesmo juiz utilizado pelo push (`bin/commit_msg.sh:29`; `bin/tests/test_lei_akita.sh:529`). Há limites declarados para `--amend` e cherry-pick/rebase; a causa concreta deste caso permanece **[não verificado]** (`bin/commit_msg.sh:10`). A catraca atual é 12, e o teste acrescenta um item dinamicamente, compatível com cobrar o 14º após a alteração (`bin/tests/test_lei_akita.sh:409`; `bin/tests/test_lei_akita.sh:990`). Os **45 commits** não estão comprovados: a tentativa documentada tinha 34 (`app/docs/RELATO.md:20`; **[não verificado]**).

4. **Exportada, dinheiro ou outro `!`: efeito adicional [não verificado].** O `!` documentado é justamente exceção de selo ou regravação (`app/docs/RELATO.md:7`). O pouso próprio do instrumento respeita o corte vigente (`app/docs/CORTES.md:304`); integridade financeira/exportada do pacote completo: **[não verificado]**.

**VEREDITO: PROCEDE (item 2: preservar explicitamente a permissão L-123-MEDIDA-EM-PY ao definir a recusa; `bin/tests/test_lei_akita.sh:348`).**

## 16/11 23:53 · sessao 24271795b704 · - · - · origem=Ronald (tmux)

**Item 3: PROCEDE — o RED ainda não está suficientemente condicionado.** Irmã U163/ausente elimina o bloqueio herdado pelo K6 da irmã, porque o predicado atual pula U163; o controle com irmã fora163 entregue confere. (`app/chamados/services/materializacao.py:795`; `app/chamados/services/validacao.py:224`; `app/chamados/tests/test_bug_par_volta.py:145`)

O literal ainda admite casos que **hoje não gravam**: U163 com declarado diferente do gravado espera mesmo com F6 aberto; ausência de irmã com vizinha do mesmo tipo impede fechar a paridade; batida aprovada existente resulta em `ja_satisfeita`, sem nova batida. (`app/chamados/services/materializacao.py:933`; `app/chamados/services/materializacao.py:936`; `app/chamados/services/materializacao.py:194`; `app/chamados/services/materializacao.py:903`; `app/chamados/services/materializacao.py:928`; `app/chamados/services/materializacao.py:898`)

Para tornar o RED discriminante, explicitar: metade de intervalo respondida, sem validação/materialização anterior, declarado igual ao gravado, plantio permitido pelas guardas de data/paridade/colisão, sem reuso e implausibilidade ausente ou devidamente confirmada; F6 aberto deve corresponder à competência do instante local de cada plantio. (`app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:597`; `app/chamados/services/validacao.py:609`; `app/chamados/services/validacao.py:615`; `app/chamados/services/validacao.py:681`; `app/chamados/services/materializacao.py:891`; `app/chamados/services/materializacao.py:914`; `app/ponto/janelas.py:84`)

A existência/execução de tal RED e de pergunta real que cumpra todas essas condições permanece [nao verificado].

## 16/11 23:53 · sessao 30eb8b95c3ab · - · - · origem=Ronald (tmux)

2. O complemento responde às três dúvidas registradas e mantém o encaixe F4 sobre B2; a exceção anterior U163 entregue/F6 precisa ser sucedida no registro e na matriz. (`app/docs/RELATO.md:5`; `app/docs/RELATO.md:21`; `app/docs/RELATO.md:44`; `app/docs/RELATO.md:52`; `app/docs/BACKLOG.md:823`) Continua vigente F4 logo atrás de F1/F2 **ou** contador J1 esperado zero no intervalo. (`app/docs/CORTES.md:487`)

4. O bloqueio ocorre antes do carimbo/recalculo no admin e antes de recusa/pendência/carimbo no escritor; corresponde à preservação declarada. (`app/chamados/services/validacao.py:652`; `app/chamados/services/validacao.py:655`; `app/chamados/services/validacao.py:666`; `app/chamados/services/materializacao.py:1829`) Recalculo herdado já está apontado; o complemento não autoriza alterar FechamentoMensal exportado, que continua sob `!`. (`app/docs/RELATO.md:44`; `app/docs/RELATO.md:52`; `CLAUDE.md:411`) Impacto monetário real: [nao verificado].

Cuidado de implementação: remover simplesmente a exclusão U163 do segundo laço faz sobreposição com U103 já inserida pelo primeiro; a lista deve conservar uma ocorrência por pergunta. (`app/chamados/management/commands/bug_par_103.py:53`; `app/chamados/management/commands/bug_par_103.py:63`; `app/chamados/management/commands/bug_par_103.py:71`; `app/chamados/management/commands/bug_par_103.py:77`; `app/chamados/services/universo_bug_par.py:30`)

## 16/11 23:53 · sessao 8556b2524238 · - · - · origem=Ronald (tmux)

- **Premissas documentadas:** `RELATO.md:7-8` registra a recusa de `4633d2c17`, `padrao=n/a (medida)`, somente docstrings nos dois arquivos, proposta de catraca 12→13 e descendência das cópias B2/AUSENCIA; os fatos de produção permanecem **[não verificado]**.
- **Precedentes compatíveis:** crescer a exceção exige novo aval e não regravar histórico (`CORTES.md:449`); a proibição literal posterior refere-se a commit novo **sem `padrao=`**, condição diferente de `n/a (medida)` (`CORTES.md:484`; `RELATO.md:8`). [nao verificado]
- **Causa já prevista:** o hook deveria recusar `n/a (instrumento)` tocando negócio no commit (`BACKLOG.md:778`); seus limites conhecidos incluem `--amend` julgando somente o delta e diferença de vocabulário (`BACKLOG.md:780`; `bin/commit_msg.sh:10-11`).
- **Implementação ainda futura:** catraca atual 12 (`bin/tests/test_lei_akita.sh:409`); o caso de item extra é construído dinamicamente, portanto pode continuar cobrando o 14º após a lista passar a 13 (`bin/tests/test_lei_akita.sh:990-999`).
- **Condição dos 45:** a tentativa anterior documentada tinha 34 commits (`RELATO.md:20`); quantidade atual 45 e presença no ar **[não verificado]**. [nao verificado]
- **Registro e pouso:** frase em CORTES deve entrar pela geração a partir de JSON (`CORTES.md:3`); pouso próprio do instrumento respeita o corte vigente (`CORTES.md:304`). [nao verificado]
- **Escopo:** o bloqueio declarado é justamente `!` para exceção de selo/regravação (`RELATO.md:7`); efeito adicional em dinheiro/exportadas pelo push completo **[não verificado]**. [nao verificado]

_6253 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
