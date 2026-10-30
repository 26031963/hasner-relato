# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 18:51** · publicado em **30/10/2026 18:54** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 18:51 · sessao 06895aaa58a5 · - · - · origem=Ronald (tmux)

Relatório somente por leitura estática; reprodução em banco, ocorrência em produção e tempo medido: [nao verificado].

1. Dois conflitos concretos com escritores/leitores existentes

- `PerguntaDisputa.abrir()` reaponta pergunta **existente**, não só cria: busca `ex`, valida apenas E4-c, atribui destino, relavra e salva; não chama a nova proteção de costura. Portanto a F1 protege `fio.reapontar_resolvedora()`, mas deixa o reaponte pelo dedup costurar viva em terminal. A exceção documentada no novo helper só menciona “nascimento”, insuficiente para este ramo existente (`app/chamados/models.py:2292`, `app/chamados/models.py:2295`, `app/chamados/models.py:2300`, `app/chamados/models.py:2314`; proteção: `app/chamados/services/fio.py:748`, `app/chamados/services/fio.py:821`). O contrato já declara os dois arquivos como escritores de destino (`app/core/tests/test_contract_escritores_estado.py:60`).

- A âncora nova usa destino absoluto, caindo no pai **somente quando destino é NULL**; os leitores vivos da mesa e do app caem no pai também quando destino está terminal (`app/chamados/models.py:296`; `app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:142`; `app/api/views.py:1043`). O juiz de respostas pendentes já lê a mesma regra de crédito vivo (`app/chamados/juizes.py:1474`). Contraexemplo estático: pai vivo de módulo `disputa_supervisao`, destino terminal, pergunta muda sem materialização. A pergunta é creditada ao pai por API/raiox, mas `_vivas_ancoradas` não a coleta; `reconciliar()` pode fechar o pai, e C6 continua dando a pergunta por viva porque sem materialização retorna True (`app/chamados/models.py:296`, `app/chamados/models.py:601`, `app/chamados/models.py:607`, `app/chamados/models.py:574`; `app/chamados/juizes.py:1076`). Ocorrência desse arranjo em produção: [nao verificado].

- Outra passagem sem guarda: instância antiga `resolvido`, banco reaberto por outra instância e pergunta viva criada/reatada, seguida de `.encerrar()` na instância antiga. `encerrar` decide pela instância, gate e tripwire só atuam quando o estado em memória pertence a `VIVOS`; assim grava `fechado` sobre o banco vivo sem consultar a pergunta (`app/chamados/models.py:1412`, `app/chamados/models.py:356`, `app/chamados/models.py:201`, `app/chamados/models.py:206`, `app/chamados/models.py:250`). A leitura fresca já existe em `reconciliar`, porém não em `encerrar` (`app/chamados/models.py:592`). Reprodução executada: [nao verificado].

2. Censo de escritores

| Campo | Escritores persistentes de produção | Escrita em memória / nascimento / acervo |
|---|---|---|
| `ChamadoColaborador.status_local` | Núcleo `_gravar_estado`, atribui e salva (`app/chamados/models.py:211`, `app/chamados/models.py:250`). | `_vivas_ancoradas` troca/devolve em memória (`app/chamados/models.py:302`, `app/chamados/models.py:312`); `criar` nasce por create (`app/chamados/models.py:970`); migration 0039 atualiza acervo e reverse (`app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:33`). |
| `resolvido_em` | Núcleo carimba terminal ou limpa vivo (`app/chamados/models.py:227`, `app/chamados/models.py:231`). | Nascimento pode receber campos via `**campos` (`app/chamados/models.py:970`). |
| `via_encerramento` | Núcleo grava via ou limpa (`app/chamados/models.py:228`, `app/chamados/models.py:232`). | Migration backfill/reverse (`app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:25`, `app/chamados/migrations/0039_a3_via_backfill.py:28`, `app/chamados/migrations/0039_a3_via_backfill.py:34`). |
| `urgencia` | Ritual rebaixa (`app/chamados/models.py:408`), núcleo restaura (`app/chamados/models.py:234`), `rebaixar` salva (`app/chamados/models.py:415`), `_rebaixar_sem_post_save` persiste via update dinâmico (`app/chamados/models.py:550`, `app/chamados/models.py:555`), `escalar_urgencia` atribui e salva (`app/chamados/models.py:472`). | Nascimento aceita após validação (`app/chamados/models.py:967`, `app/chamados/models.py:970`). |
| `PerguntaDisputa.chamado_destino` | `abrir` no dedup (`app/chamados/models.py:2300`, `app/chamados/models.py:2314`) e nascimento (`app/chamados/models.py:2316`); `fio.reapontar_resolvedora` (`app/chamados/services/fio.py:828`). | Avaliação temporária no novo gate (`app/chamados/models.py:308`); novo helper de costura troca/devolve (`app/chamados/services/fio.py:758`, `app/chamados/services/fio.py:762`). |
| `materializada_em` + `via_resolucao` | Portas do model: admin, tranca, fantasma, reaberturas e ressuscitação (`app/chamados/models.py:2362`, `app/chamados/models.py:2376`, `app/chamados/models.py:2401`, `app/chamados/models.py:2416`, `app/chamados/models.py:2436`, `app/chamados/models.py:2446`, `app/chamados/models.py:1377`). Reconciliador carimba sem-turno e célula absolveu (`app/chamados/reconciliador.py:402`, `app/chamados/reconciliador.py:617`). Materializador carimba declaração/ausência, recusa paridade/colisão e aprovação (`app/chamados/services/materializacao.py:509`, `app/chamados/services/materializacao.py:526`, `app/chamados/services/materializacao.py:858`, `app/chamados/services/materializacao.py:1043`, `app/chamados/services/materializacao.py:1149`, `app/chamados/services/materializacao.py:1177`, `app/chamados/services/materializacao.py:1339`). Dedup retrata duplicata (`app/chamados/services/fio.py:875`). Emissão resolve por chamado/fato e desfaz absorção (`app/chamados/services/disputa_emissao.py:1562`, `app/chamados/services/disputa_emissao.py:1644`, `app/chamados/services/disputa_emissao.py:1360`). Relavra troca via/desfaz carimbo (`app/chamados/services/relavra_via.py:123`, `app/chamados/services/relavra_via.py:231`). | `encerrar_por_fato_no_espelho` só troca via (`app/chamados/models.py:2389`). `regularizacao_ext.py:198` é campo homônimo de **RegularizacaoExterna**, não pergunta (`app/chamados/services/regularizacao_ext.py:197`). |
| `DisputaSupervisao.fechada_em` | Fechamento em `fechar` (`app/chamados/models.py:1730`, `app/chamados/models.py:1734`); limpeza por `reabrir_admin` e `reabrir_sistema` (`app/chamados/models.py:1932`, `app/chamados/models.py:1956`). | Default NULL (`app/chamados/models.py:1557`). |
| `LogAuditoria` ações pedidas | Persistência comum por `evento` (`app/core/observ.py:133`). `estado_chamado` nasce no núcleo (`app/chamados/models.py:246`); `fechamento_recusado` em `_registrar_recusa` (`app/chamados/models.py:347`); `pergunta_reatada` em `_gate_pergunta_viva` (`app/chamados/models.py:377`). | `evento` é best-effort e pode retornar sem linha após engolir erro (`app/core/observ.py:105`, `app/core/observ.py:139`). |

3. Censo agrupado de leitores que interferem na fatia

- `status_local`: motor das portas (`app/chamados/models.py:118`, `app/chamados/models.py:596`, `app/chamados/models.py:664`, `app/chamados/models.py:1412`); pergunta materializada/C6 (`app/chamados/juizes.py:700`, `app/chamados/juizes.py:774`); C8 e escolha de cobrança (`app/chamados/juizes.py:1621`, `app/chamados/juizes.py:1675`); receivers de estado (`app/chamados/signals.py:33`, `app/chamados/signals.py:70`, `app/chamados/signals.py:81`, `app/chamados/signals.py:169`, `app/chamados/signals.py:230`); fila, painel e partição (`app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:187`, `app/chamados/services/verbos.py:227`, `app/chamados/services/desenlace.py:48`); fio e próximo card (`app/chamados/services/fio.py:385`, `app/chamados/services/fio.py:399`, `app/chamados/services/fio.py:428`); API (`app/api/views.py:1047`, `app/api/views.py:1145`, `app/api/views.py:1155`, `app/api/views_mensageria.py:417`); relatórios (`app/relatorios/views.py:745`, `app/relatorios/pdf_chamados.py:116`); apresentação (`app/templates/chamados/partials/_lista_chamados.html:33`, `app/templates/chamados/partials/_card_atendimento.html:19`, `app/templates/chamados/partials/modal_fio.html:493`, `app/templates/chamados/detalhe_local.html:279`); auditoria de invariantes (`app/chamados/management/commands/auditar_invariantes_chamados.py:573`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1067`).

- `resolvido_em`: núcleo preserva terminal→terminal (`app/chamados/models.py:226`); reabertura administrativa mede fato novo desde carimbo (`app/chamados/models.py:1214`, `app/chamados/models.py:1218`); fila/indicadores contam encerramentos (`app/chamados/services/fila.py:109`, `app/chamados/services/painel.py:243`, `app/chamados/services/painel.py:247`, `app/chamados/services/verbos.py:234`); invalidação de painel usa máximo (`app/chamados/services/painel.py:65`); arquivo ordena (`app/chamados/views_arquivar.py:157`); detalhe e relatório exibem (`app/templates/chamados/detalhe.html:56`, `app/templates/chamados/detalhe_local.html:88`, `app/templates/relatorios/historico_chamados.html:127`); vigia terminal sem carimbo (`app/chamados/management/commands/auditar_invariantes_chamados.py:571`).

- `via_encerramento`: nascimento/revival não reabre cascade (`app/chamados/models.py:890`, `app/chamados/services/adesao.py:90`); guarda administrativa (`app/chamados/models.py:1214`); tranca reabre por esta via (`app/chamados/services/competencia_trancada.py:252`); emissão não toca trancada (`app/chamados/services/disputa_emissao.py:1531`); arquivo/pauta (`app/chamados/views_arquivar.py:64`, `app/chamados/views_arquivar.py:156`); diagnóstico de cobertura de furos (`app/ponto/services/furos_sem_cobranca.py:69`); invariantes exigem via (`app/chamados/management/commands/auditar_invariantes_chamados.py:1067`).

- `urgencia`: SLA (`app/chamados/services/sla.py:30`, `app/chamados/services/painel.py:375`), ordenação/filtro/contadores (`app/chamados/services/painel.py:102`, `app/chamados/services/painel.py:118`, `app/chamados/services/painel.py:223`), verbos (`app/chamados/services/verbos.py:194`), API (`app/api/views.py:1150`, `app/api/views_mensageria.py:419`), templates (`app/templates/chamados/fio_colaborador.html:165`, `app/templates/chamados/partials/tabela_chamados.html:30`, `app/templates/chamados/partials/_lista_chamados.html:68`).

- `chamado_destino`: âncora absoluta (`app/chamados/services/fio.py:116`), crédito vivo da mesa (`app/chamados/services/raiox.py:60`, `app/chamados/services/raiox.py:142`), crédito do app (`app/api/views.py:1046`), serialização de pergunta (`app/api/views.py:2626`, `app/api/views.py:2642`), juiz da materialização (`app/chamados/juizes.py:699`), juiz das irmãs (`app/chamados/juizes.py:1474`), derivação do marco (`app/chamados/juizes.py:1303`, `app/chamados/juizes.py:1430`), busca de disputa (`app/chamados/services/fio.py:135`), tranca (`app/chamados/services/competencia_trancada.py:136`), materialização de resolvidas (`app/chamados/services/disputa_emissao.py:1537`), filas/pergunta no app (`app/chamados/services/pergunta_no_app.py:116`, `app/chamados/services/pergunta_no_app.py:139`).

- `materializada_em`/`via_resolucao`: C6 usa carimbo e vias para tranca, identidade, lâmpada, ausência de materialização e fato (`app/chamados/juizes.py:1015`, `app/chamados/juizes.py:1044`, `app/chamados/juizes.py:1050`, `app/chamados/juizes.py:1076`, `app/chamados/juizes.py:1083`); juiz de materializada usa vias (`app/chamados/juizes.py:603`); estado exibido da pergunta (`app/chamados/models.py:2341`); `respostas_sem_veredito` exclui materializadas (`app/chamados/models.py:1654`); materializador seleciona não materializadas (`app/chamados/services/materializacao.py:417`, `app/chamados/services/materializacao.py:1289`); relavra seleciona carimbos desfazíveis (`app/chamados/services/relavra_via.py:179`); raiox esconde materializada comprovada por lavra (`app/chamados/services/raiox.py:133`); cache agregado usa timestamp (`app/chamados/veredito_lavrado.py:163`); vigia I8 usa carimbos (`app/chamados/management/commands/auditar_invariantes_chamados.py:954`); UI relata via (`app/templates/chamados/partials/_card_atendimento.html:186`). O leitor de fila de flip de paridade é filtro, não escritor (`app/ponto/services/flip_auto.py:129`).

- `fechada_em`: receivers fecham pai/materializam/retomam regularização (`app/chamados/signals.py:28`, `app/chamados/signals.py:106`, `app/chamados/signals.py:130`); API limita disputas abertas (`app/api/views.py:1008`, `app/api/views.py:1030`, `app/api/views.py:2746`); fio resolve e impede operação em fechada (`app/chamados/services/fio.py:131`, `app/chamados/services/fio.py:594`, `app/chamados/services/fio.py:627`); validação pode reabrir (`app/chamados/services/validacao.py:199`); reconciliador/materializador fecham somente aberta (`app/chamados/reconciliador.py:413`, `app/chamados/reconciliador.py:635`, `app/chamados/services/materializacao.py:542`, `app/chamados/services/materializacao.py:1233`); cascata e tranca (`app/colaboradores/services/desligamento.py:139`, `app/chamados/services/competencia_trancada.py:145`); crons de disputa/becos (`app/chamados/management/commands/auditar_disputas.py:87`, `app/chamados/management/commands/fechar_becos_disputa.py:32`, `app/ponto/management/commands/alertar_disputas_pendentes.py:34`); vigia viva em disputa fechada (`app/chamados/services/vigia_fio.py:388`); cadastro/situação (`app/colaboradores/services/drawer.py:70`, `app/colaboradores/services/situacional.py:117`, `app/colaboradores/painel_cell.py:32`); tela (`app/templates/chamados/partials/modal_fio.html:372`).

- Logs: `fechamento_recusado` e `estado_chamado` são lidos pelo dedup da recusa (`app/chamados/models.py:342`, `app/chamados/models.py:345`). `estado_chamado` alimenta história e dois vigias (`app/chamados/services/historico.py:44`, `app/chamados/services/historico.py:86`, `app/chamados/services/vigia_loop.py:29`, `app/chamados/services/vigia_loop.py:114`). `pergunta_reatada` não apresentou leitor especializado na busca nominal da árvore; o leitor genérico de auditoria aceita todas as ações e filtro literal (`app/core/views.py:705`, `app/core/views.py:710`). História do chamado só inclui `estado_chamado`, logo não mostra a nova reata ou recusa (`app/chamados/services/historico.py:44`, `app/chamados/services/historico.py:86`). As ações livres já são admitidas pelo armazenamento, embora as três não pertençam ao menu estático `ACOES` (`app/core/models.py:286`, `app/core/models.py:296`; menu: `app/core/views.py:738`).

4. Limites adicionais do censo

- Migrações 0039 escrevem terminal fora da porta como acervo; o selo novo exclui migrations, mas a instrução da migration é executável em instalação/migração, e não há evidência de seu estado de aplicação nesta sonda (`app/chamados/migrations/0039_a3_via_backfill.py:24`; aplicação: [nao verificado]).
- O arquivo `app/chamados/admin.py` está vazio na cópia inspecionada; nenhum leitor/escritor explícito desses campos ali foi encontrado [nao verificado quanto a comportamento de registro genérico externo].
- Testes/fixtures escrevem estados deliberadamente por create/update; não devem ser contados como escritores de produção; o próprio novo selo declara a exclusão (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:6`).
- Novas ações de auditoria usam a infraestrutura best-effort existente; “recusa com uma linha garantida” não é garantia transacional dura porque `evento()` engole falha e permite continuidade (`app/core/observ.py:98`, `app/core/observ.py:105`, `app/core/observ.py:133`).

## 30/10 18:51 · sessao 95a24d27d2ce · - · - · origem=Ronald (tmux)

Achados estáticos; execução de testes e ocorrência em produção [nao verificado]. [nao verificado]

1. **Bypass por instância velha.** Uma instância carregada como `resolvido` pode sobreviver à reabertura feita por outra instância (`resolvido -> em_analise` permitido em `app/chamados/catalogo/motor.py:102`). Depois de nascer pergunta viva, `velha.encerrar()` decide pelo estado velho (`app/chamados/models.py:1412`), pula a guarda (`app/chamados/models.py:356`) e pula o tripwire (`app/chamados/models.py:206`), gravando `fechado`. Só `reconciliar` recarrega os campos do banco (`app/chamados/models.py:592`). O teste novo explicitamente permite terminal→terminal com pergunta ancorada (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:205`).

2. **Ações humanas deixam efeitos antes da nova recusa.** Autorizar geofence pode criar advertência (`app/chamados/services/acoes_chamado.py:193`), anexar nota (`app/chamados/services/acoes_chamado.py:218`) e então receber `ValueError` da nova guarda no `transicionar` (`app/chamados/services/acoes_chamado.py:222`; `app/chamados/models.py:159`). Não há atomic abrangendo esse método; a recusa deixa chamado vivo e permite repetir a advertência (`app/chamados/services/acoes_chamado.py:176`). Agenda e escala têm atomic, mas a exceção propaga sem tratamento nas ações e na casca (`app/chamados/services/acoes_chamado.py:102`, `app/chamados/services/acoes_chamado.py:140`; `app/chamados/views.py:459`).

3. **Dois chamadores ignoram a recusa de `encerrar`.** Troca de vínculo segue criando novo aceite depois de `_ch_old.encerrar()` sem conferir retorno (`app/colaboradores/services/vinculo.py:384`, `app/colaboradores/services/vinculo.py:393`). Regularização externa já grava estado materializado e nota, ignora `encerrar()` e retorna `materializada=True` (`app/chamados/services/regularizacao_ext.py:197`, `app/chamados/services/regularizacao_ext.py:205`, `app/chamados/services/regularizacao_ext.py:207`). Isso não prova que os efeitos de domínio sejam incorretos, mas invalida a expectativa de que o chamado necessariamente saiu da fila nessas chamadas (`app/colaboradores/services/vinculo.py:374`; `app/chamados/models.py:1418`).

4. **Atomicidade de `Disputa.fechar` tem limite.** A nova ordem escreve pergunta, disputa e depois chamado pelo signal (`app/chamados/models.py:1722`, `app/chamados/models.py:1734`; `app/chamados/signals.py:42`). Uma falha do fechamento do pai é capturada pelo receiver; portanto não desfaz necessariamente pergunta e disputa, apesar do comentário “falha leva os dois” (`app/chamados/signals.py:46`; `app/chamados/models.py:1720`). Há ordem inversa em outro caminho: save do chamado dispara resolução que escreve perguntas (`app/chamados/models.py:250`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1583`). Deadlock efetivo [nao verificado].

5. **Desligamento muda a trilha intermediária.** A disputa fecha primeiro e seu signal pode produzir `resolvido/sistema`; depois o laço inclui `resolvido` e produz `fechado/cascade` (`app/colaboradores/services/desligamento.py:139`, `app/colaboradores/services/desligamento.py:150`; `app/chamados/catalogo/motor.py:32`, `app/chamados/catalogo/motor.py:95`). O segundo ato preserva `resolvido_em`, regrava a via e deixa outra linha `estado_chamado` (`app/chamados/models.py:226`, `app/chamados/models.py:228`, `app/chamados/models.py:246`). A descrição do chamado de auditoria continua afirmando que a cascata fechou chamados/disputas mesmo havendo recusas (`app/colaboradores/services/desligamento.py:155`, `app/colaboradores/services/desligamento.py:170`).

Testes existentes previsivelmente vermelhos, sem execução: [nao verificado]

- `LacoHxBug31Test.test_resposta_creditada_ao_pai_SEGURA_a_retratacao`, `test_MORDE_validada_libera_o_pai`, `test_retida_duas_vezes_UMA_linha_de_trilha`, `test_retida_MORDE_pendencia_nova_grava_de_novo`: encerram destino com resposta viva, ignoram o novo `False` e pressupõem destino morto (`app/chamados/tests/test_laco_hx_x_bug31.py:107`, `:129`, `:140`, `:153`). Destino vivo deixa de creditar a resposta ao pai (`app/chamados/juizes.py:1474`), então a retenção esperada não acontece e a retratação do pai prossegue (`app/chamados/reconciliador.py:520`).

- `RespostaVisivelTest.test_resposta_pendente_mantem_pai_visivel` e `test_resposta_pendente_tem_precedencia_sobre_encerrar`: pai encerra porque a âncora é o destino; destino agora recusa encerrar, permanecendo vivo (`app/chamados/tests/test_b2d_resposta_visivel.py:57`, `:104`; `app/chamados/models.py:298`, `app/chamados/models.py:1418`). O HX não reabre o pai, porque o mesmo juiz não lhe credita resposta enquanto o destino vive (`app/chamados/services/disputa_emissao.py:1598`; `app/chamados/juizes.py:1474`).

- Candidatos por custo: `test_gravar_estado_sem_pergunta`, `test_gate_da_premissa_fecha_com_dia_e_celula`, `test_gate_da_premissa_recusa_com_dia_e_celula`, `test_registrar_batida_com_1_furo_vivo_que_ela_fecha`, `test_registrar_batida_com_3_furos_vivos_que_ela_fecha` mantêm tetos anteriores (`app/ponto/tests/test_selo_chokepoint_escrita.py:112`, `:114`, `:115`, `:157`; testes em `:286`, `:319`, `:325`, `:360`, `:365`). O novo atomic e as consultas de `_vivas_ancoradas` acrescentam trabalho nesses caminhos (`app/chamados/models.py:569`, `app/chamados/models.py:601`, `app/chamados/models.py:207`). Contagens exatas e quais excedem o teto [nao verificado].

Os signals de disputa e de resolução já tratam `False` ou isolam exceções; os crons principais conferem o retorno antes de contar/anotar (`app/chamados/signals.py:42`, `app/chamados/signals.py:91`; `app/chamados/services/competencia_trancada.py:177`; `app/ponto/services/cartorio.py:1163`, `:1175`, `:1196`). Os descartes em `detectar_ausencias` e materialização são explicitamente autorizados pelo selo existente, sem nota/contador posterior (`app/chamados/tests/test_selo_reconciliar_le_o_retorno.py:22`).

## 30/10 18:51 · sessao ec3bc7bfd574 · - · - · origem=Ronald (tmux)

Achado confirmado por execução em memória, sem Django/banco: `NucleoUnicoContract.test_um_escritor_de_status_local` fica vermelho. O selo antigo só libera a atribuição literal `self.status_local = novo`; a nova restauração `self.status_local = _antes` entra em `ofensores`. Referências: `app/chamados/tests/test_contract_status_nucleo.py:7-9,23-28`; `app/chamados/models.py:312`.

O selo AST novo passa na árvore atual, mas deixa passar os seguintes mutantes, comprovados executando suas funções com Python stdlib em memória:

- `d={'status_local':'fechado'}; qs.update(**d)` e `update_or_create(defaults=d)`: ele só reconhece dicionário literal no próprio argumento. `app/chamados/tests/test_o261e_f1_selo_encerramento.py:44-45,67-73`.
- `C(pk=1,status_local='fechado').save(update_fields=['status_local'])`: todo construtor é ignorado, sem comprovar nascimento. `app/chamados/tests/test_o261e_f1_selo_encerramento.py:61-80,140-147`.
- `ch.__dict__['status_local']='fechado'; ch.save()` e SQL direto: formas ausentes do detector. `app/chamados/tests/test_o261e_f1_selo_encerramento.py:55-80`.
- Um `save()` acrescentado dentro de `_vivas_ancoradas`: a exceção libera a função inteira pelo nome; não verifica ausência de persistência nem restauração no `finally`. `app/chamados/tests/test_o261e_f1_selo_encerramento.py:27-28,86-87`.
- Trocar a condição do tripwire por `if False:` ou trocar seu `raise` por `print`: o checker ainda retorna `True`, pois compara somente números de linha; o mutante atual só remove uma chamada textual. `app/chamados/tests/test_o261e_f1_selo_encerramento.py:99-110,159-163`; guarda efetiva em `app/chamados/models.py:206-210`.

Não encontrei esses bypass hipotéticos usados atualmente: o scanner novo encontrou apenas as atribuições em `app/chamados/models.py:211,302,312` e nenhuma fora de suas permissões. A execução Django dos testes permanece [nao verificado].

Regressões funcionais previstas por leitura:

- `RespostaVisivelTest.test_resposta_pendente_mantem_pai_visivel` e `test_resposta_pendente_tem_precedencia_sobre_encerrar`: o teste fecha primeiro o pai e depois tenta fechar o destino com resposta viva. Agora o destino recusa; o crédito permanece no destino vivo, e o resolver não reabre o pai. As expectativas de pai vivo tendem a falhar. `app/chamados/tests/test_b2d_resposta_visivel.py:57-67,104-115`; `app/chamados/models.py:356-370,1415-1420`; `app/chamados/services/disputa_emissao.py:1596-1606`; `app/chamados/juizes.py:1471-1476`.
- `LacoHxBug31Test.test_resposta_creditada_ao_pai_SEGURA_a_retratacao`, `test_MORDE_validada_libera_o_pai`, `test_retida_duas_vezes_UMA_linha_de_trilha` e `test_retida_MORDE_pendencia_nova_grava_de_novo`: dependem de `dest.encerrar()` produzir destino morto com resposta pendente. A nova recusa conserva destino vivo, impedindo o crédito ao pai e a retenção esperada. `app/chamados/tests/test_laco_hx_x_bug31.py:107-118,129-164`; `app/chamados/juizes.py:1471-1476`; `app/chamados/reconciliador.py:448-451`. Execução e eventuais efeitos de fixtures/dedup: [nao verificado].

Coberturas ausentes nos testes novos: rollback após reapontar, falha na segunda pergunta de um plano, concorrência, `encerrar_por_competencia_trancada`, reata por `cancelar`/`encerrar` e estabilidade dos dados exportados; os casos presentes estão em `app/chamados/tests/test_o261e_f1_porta_encerramento.py:81-254`. O teste de competência entregue apenas simula o juiz de exportação e verifica o status final; não verifica integridade dos campos de pergunta/exportada. `app/chamados/tests/test_o261e_f1_porta_encerramento.py:113-118`.

## 30/10 18:46 · sessao 06f71c2afe78 · O261-V-FASE · verificador · origem=Code

**O delta 3 morde, mas restam falhas na obra inteira:** o horário ainda pode misturar vínculos, o chat calcula a frota no request e a leitura nova aceita uma lavra antiga incompleta (`app/escala/services/propositor.py:526`; `app/core/views_ia.py:572`; `app/escala/services/fase_12x36.py:99`).

1. **Escritores, leitores e autoridades**

| Campo/valor | Quem escreve ou produz | Quem lê |
|---|---|---|
| `MetricaSnapshot`: chave, escopo, datas, `valor`, `detalhe` | `fase_12x36.lavrar`: chave fixa, `tenant/0`, datas recebidas e `valor=len(colabs_de_desvio)` (`app/escala/services/fase_12x36.py:75`, `app/escala/services/fase_12x36.py:84`). Chamador: censo (`app/ponto/management/commands/censo_fase_12x36.py:250`). | `ultimo_lavrado` lê o **detalhe**, acrescentando `data_ref` e `medido_em`; os consumidores contam IDs, sem ler diretamente `valor` (`app/escala/services/fase_12x36.py:96`, `app/escala/services/fase_12x36.py:102`, `app/api/views_mensageria.py:1675`). |
| Detalhe: `colabs_de_desvio`, `linhas_de_desvio`, `automaticos`, `propostas`, `linhas_medidas`, `linhas`, `criterio`, `esperado` | O censo separa desvio/automático/proposta e monta o JSON (`app/ponto/management/commands/censo_fase_12x36.py:227`, `app/ponto/management/commands/censo_fase_12x36.py:251`). `propostas` aqui contém **IDs**, não propostas aplicáveis (`app/ponto/management/commands/censo_fase_12x36.py:255`). | IDs: fila e pauta (`app/chamados/services/fila_por_causa.py:174`; `app/escala/services/fase_12x36.py:117`). Linhas/contadores/critério: API e comando vigia (`app/api/views_mensageria.py:1675`; `app/ponto/management/commands/vigiar_familia_escala.py:25`). |
| Detalhe: `itens`, com data, EC/template do dia, tipo, `aplicavel`, colaborador, empresa, competência e dono | Dias e piso são montados no censo; a lavra acrescenta `dono_do_dia='CADASTRO'` (`app/ponto/management/commands/censo_fase_12x36.py:208`, `app/ponto/management/commands/censo_fase_12x36.py:259`). | C10 lê os itens; o propositor lê os pares `(colab, ec_do_dia)` para `fase_invertida` (`app/escala/services/cadastro_realidade.py:339`; `app/escala/services/propositor.py:491`). |
| Assinatura **C10** da lista | `_fase_deslocada` formata o item; `lista` acrescenta ação, URL, dono e contagens (`app/escala/services/cadastro_realidade.py:335`, `app/escala/services/cadastro_realidade.py:647`). | Tela e PDF usam a mesma lista (`app/escala/views.py:490`; `app/templates/escala/cadastro_x_realidade.html:28`; `app/escala/services/cadastro_realidade.py:759`). `dias_na_lista` alimenta o vigia do fio (`app/escala/services/cadastro_realidade.py:717`; `app/chamados/services/vigia_fio.py:259`). |
| Retorno de `propositor.propostas` | Produz uma proposta por vínculo, com `vinculo_id`, evidência, motivo, piso e chamados; retorna também contadores de universo/ritmo/pausa (`app/escala/services/propositor.py:537`, `app/escala/services/propositor.py:583`). | Fio (`app/chamados/services/proposta_no_fio.py:63`, `app/chamados/services/proposta_no_fio.py:102`); painel paginado (`app/chamados/services/painel.py:520`); ficha (`app/colaboradores/services/ficha.py:176`); chat (`app/core/views_ia.py:572`); API (`app/api/views_mensageria.py:2019`); pauta (`app/escala/services/propositor.py:638`); comandos (`app/escala/management/commands/propostas_de_escala.py:26`; `app/escala/management/commands/ritmo_discordante.py:18`). |
| Retorno de `da_assinatura`: `(proposta ou None, erro)` | Filtra colaborador+assinatura e recusa multiplicidade (`app/escala/services/propositor.py:666`). | Fio verifica o erro e depois mantém sua escolha da primeira proposta; chat usa a proposta devolvida (`app/chamados/services/proposta_no_fio.py:107`; `app/core/views_ia.py:575`). |
| `piso_de_aplicacao` | Lê `janela_atual`; `_piso_de_aplicacao` é sua casca (`app/escala/services/propositor.py:590`, `app/escala/services/propositor.py:607`). | Censo marca `aplicavel`; propositor exclui EC encerrado antes do piso e publica `aplicar_desde`; aplicação passa essa data à porta existente (`app/ponto/management/commands/censo_fase_12x36.py:106`, `app/ponto/management/commands/censo_fase_12x36.py:210`; `app/escala/services/propositor.py:459`, `app/escala/services/propositor.py:558`, `app/escala/services/propositor.py:745`). |

A escolha do vínculo delega à autoridade indicada em **JUIZES**, assim como a janela; a escolha específica do censo como autoridade da fase está autorizada no aval (`app/docs/JUIZES.md:35`, `app/docs/JUIZES.md:61`, `app/docs/JUIZES.md:75`; `app/docs/PROMPTS.md:1686`).

**Sim, ficou consumidor de escrita fora do diff sem adaptação:** a CLI seleciona apenas por colaborador e aplica a primeira proposta, sem passar pela guarda de ambiguidade (`app/escala/management/commands/propostas_de_escala.py:30`, `app/escala/management/commands/propostas_de_escala.py:50`). Contraexemplo: A encerrado e B ativo têm a mesma assinatura; `--aplicar COLAB` aplica A silenciosamente. A chamada da função real, extraída em memória com dependências simuladas, confirmou essa seleção; a guarda existente recusaria as duas (`app/escala/services/propositor.py:674`). Isso responde ao censo de consumidores externos; a delimitação de escopo dos sítios tocados está em `CLAUDE.md:603`.

2. **Achados na obra inteira**

- **P1 — Recorte por vínculo incompleto: horário de B contamina proposta de A.** Os dias pertencem ao EC pela geradora, mas a mediana recebe todas as batidas entre `min(_meus)` e `max(_meus)`; seu filtro considera somente esse intervalo (`app/escala/alimentacao.py:237`; `app/escala/services/propositor.py:526`; `app/escala/services/propositor.py:152`).  
  **Contraexemplo:** A responde por 22–24/10 e 01–03/11, com seis turnos 08–16; B responde por 25–31/10, com sete turnos 16–00. A mediana real de A é `(480,960)`, mas a chamada atual retorna `(960,0)`. Essa mediana governa a escolha do template de ritmo, podendo sugerir 16h para A (`app/escala/services/propositor.py:170`, `app/escala/services/propositor.py:327`). Viola a coerência do recorte e L-041/L-099 (`app/docs/LEIS.md:89`; `CLAUDE.md:308`). A reprodução foi em memória; ocorrência em produção: **[nao verificado]**.

- **P2 — Custo de frota no request do chat.** `_proposta_de_escala` chama `propostas()` sem `colaborador_id`, tanto no preview quanto no aceite (`app/core/views_ia.py:572`, `app/core/views_ia.py:588`, `app/core/views_ia.py:646`).  
  **Contraexemplo:** uma solicitação para X carrega colaboradores, células e vínculos da frota e percorre seus dias; só depois seleciona X (`app/escala/services/propositor.py:427`, `app/escala/services/propositor.py:436`, `app/escala/services/propositor.py:445`; `app/core/views_ia.py:578`). O próprio contrato exige estreitamento no POST (`app/escala/services/propositor.py:395`). É herdado em função tocada, portanto alcançado pelo escopo declarado (`CLAUDE.md:603`). Tempo e consultas atuais: **[nao verificado]**.

- **P2 — Pré-requisito de transição: lavra antiga aceita como completa.** A base gravava IDs, sem `itens`, `linhas`, `criterio` ou `linhas_medidas`; o leitor novo aceita esse detalhe sem verificar o formato (`../d80c98a5dba444e0bdab60764d90ee6a817ed828/app/ponto/management/commands/censo_fase_12x36.py:200`; `app/escala/services/fase_12x36.py:99`).  
  **Contraexemplo:** snapshot antigo com um colaborador em desvio produz C10 vazia e gatilho `fase_invertida` vazio, enquanto fila/pauta mantêm o ID positivo; a API responde `n=1`, `sem_medida=False`, `vinculos=[]` (`app/escala/services/cadastro_realidade.py:339`; `app/escala/services/propositor.py:491`; `app/chamados/services/fila_por_causa.py:174`; `app/api/views_mensageria.py:1675`). As funções reais reproduziram essa incompatibilidade em memória. A execução periódica declarada é diária às 07:38; relavra anterior ao pouso: **[nao verificado]** (`app/config/crons.py:614`). A inconsistência contraria consumidor lendo uma mesma lâmpada e L-099 (`CLAUDE.md:276`, `CLAUDE.md:308`).

- **P3 — Contador de colaboradores rotulado como vínculos.** A pauta conta `colabs_de_desvio`, mas anuncia “vínculo(s)” (`app/escala/services/fase_12x36.py:117`, `app/escala/services/fase_12x36.py:122`).  
  **Contraexemplo:** dois EC em desvio da mesma pessoa geram `n=1` e “1 vínculo”. O censo deduplica explicitamente os colaboradores; o rótulo deixa de representar sua unidade (`app/ponto/management/commands/censo_fase_12x36.py:253`; `CLAUDE.md:35`).

Nas demais classes, conferi escritor único da chave, morte do detector paralelo, relógio capturado na borda do comando, ausência de âncora na C10 e passagem da aplicação pela porta existente (`app/escala/services/fase_12x36.py:78`; `app/escala/tests/test_o261_v_fase_na_lista.py:213`; `app/ponto/management/commands/censo_fase_12x36.py:62`; `app/escala/tests/test_o261_v_fase_na_lista.py:152`; `app/escala/services/propositor.py:755`). A unificação das âncoras permanece expressamente destinada à obra própria (`app/docs/PROMPTS.md:1686`).

3. **Delta da volta 3**

**Sim, morde por inspeção e pela avaliação da expressão real da guarda:** `--lavrar` com `--min-plantoes > 6` retorna antes da medição/escrita (`app/ponto/management/commands/censo_fase_12x36.py:74`).

O teste guarda uma lavra não vazia, executa `--min-plantoes 100 --lavrar` e exige detalhe idêntico. Sem a cláusula nova, o filtro elimina as linhas e a lavra substitui o detalhe por vazio: a igualdade falha (`app/escala/tests/test_o261_v_fase_na_lista.py:142`, `app/escala/tests/test_o261_v_fase_na_lista.py:148`, `app/escala/tests/test_o261_v_fase_na_lista.py:150`; `app/ponto/management/commands/censo_fase_12x36.py:156`, `app/ponto/management/commands/censo_fase_12x36.py:251`).

Execução Django, RED/GREEN históricos, suíte completa e estado de produção: **[nao verificado]**. [nao verificado]

## 30/10 18:44 · sessao 5dd8e4bbf7a0 · - · - · origem=Ronald (tmux)

Há um achado procedente de **pré-requisito de transição**: a nova leitura aceita lavra antiga como completa, embora ela não contenha os novos campos.

- A base grava `colabs_de_desvio`, `linhas_de_desvio`, `automaticos`, `propostas` e `esperado`, sem `itens`, `linhas`, `criterio` ou `linhas_medidas` (`../d80c98a5dba444e0bdab60764d90ee6a817ed828/app/ponto/management/commands/censo_fase_12x36.py:200`).
- `ultimo_lavrado` aceita esse detalhe e só retorna `sem_medida=True` quando inexiste snapshot; não verifica seu formato (`app/escala/services/fase_12x36.py:99`).
- C10 lê `itens` ausente como vazio; o gatilho `fase_invertida` também (`app/escala/services/cadastro_realidade.py:339`; `app/escala/services/propositor.py:491`).
- Pauta e fila continuam lendo os IDs positivos; a API responde `sem_medida=False`, contador positivo e lista vazia (`app/escala/services/fase_12x36.py:116`; `app/chamados/services/fila_por_causa.py:174`; `app/api/views_mensageria.py:1675`).

**Contraexemplo executado em memória:** usando AST das funções reais e um snapshot sintético no formato da base, com `colabs_de_desvio=[17]`, obtive C10 `{}` e API `{n: 1, sem_medida: False, vinculos: [], criterio: None}`. Não houve acesso a banco nem escrita em arquivo.

Não encontrei exigência documentada de relavra antes da liberação; o mecanismo declarado continua diário às 07:38 (`app/config/crons.py:614`). Os registros do aval autorizam o novo detalhe, sem declarar sua preparação prévia ao pouso (`app/docs/PROMPTS.md:1686`; `app/docs/RELATO.md:5`). Existência desse estado em produção ou relavra já efetuada: **[nao verificado]**.

Isso pede relavra garantida antes de liberar os novos leitores ou tratamento explícito do formato incompleto. Os testes atuais cobrem ausência total de lavra e lavra nova, sem caso de lavra antiga (`app/escala/tests/test_o261_v_fase_na_lista.py:75`; `:198`).

## 30/10 18:44 · sessao 908d403c1dd5 · - · - · origem=Ronald (tmux)

Mapa e achados enviados ao pai. Dois problemas reproduzidos com funções reais extraídas por AST e dependências simuladas em memória, sem banco nem escrita:

1. A CLI continua escolhendo a primeira proposta por colaborador e aplicando sem a guarda de ambiguidade; duas propostas com a mesma assinatura resultaram na aplicação do ec1 (`app/escala/management/commands/propostas_de_escala.py:30`, `:50`; guarda em `app/escala/services/propositor.py:666`).

2. A mediana do horário usa o intervalo min/max dos dias do vínculo, incluindo turnos pertencentes a outro vínculo quando a posse dos dias é descontínua. No contraexemplo, o horário de A mudou de `(480, 960)` para `(960, 0)` (`app/escala/services/propositor.py:526`, filtro em `:152`; autoridade das geradoras em `app/escala/alimentacao.py:237`).

O delta acrescenta a rejeição de `--lavrar --min-plantoes 100` e a chamada adversarial no teste que compara o detalhe anterior; estaticamente a asserção detecta a volta da sobrescrita (`app/ponto/management/commands/censo_fase_12x36.py:74`; `app/escala/tests/test_o261_v_fase_na_lista.py:148`). Execução Django desse teste [nao verificado].

## 30/10 18:43 · sessao 856eb03ecb9f · - · - · origem=Ronald (tmux)

Achado procedente: **custo de frota no request, em função tocada pela obra**. `_proposta_de_escala` chama `propositor.propostas()` sem `colaborador_id` (`app/core/views_ia.py:572`), tanto no preview (`:588`) quanto no aceite (`:646`). O propositor percorre todos os colaboradores e seus dias antes da seleção da pessoa (`app/escala/services/propositor.py:427`, `:436`, `:445`). Contraexemplo: pedir correção de X numa frota com N pessoas executa julgamento para N e só depois filtra X (`app/core/views_ia.py:578`). O próprio contrato exige estreitar o universo no POST (`app/escala/services/propositor.py:395`). É herdado, mas a função foi tocada; a regra inclui sítios tocados no escopo (`CLAUDE.md:603`). Tempo e consultas atuais: [nao verificado].

Não encontrei quebra executável dos consumidores da API pela alteração do JSON: o cliente retorna o payload (`mensageria/nucleo/core_client.py:246`) e a ferramenta apenas acrescenta `cliente` (`mensageria/nucleo/ferramentas.py:1407`). Os comentários ainda descrevem o predicado antigo de dois lados, três vezes cada (`mensageria/nucleo/ferramentas.py:1399`; `app/api/views_mensageria.py:1660`), embora a resposta passe a publicar o critério lavrado (`app/api/views_mensageria.py:1676`). Efeito na resposta do modelo: [nao verificado].

## 30/10 18:43 · sessao 5dd8e4bbf7a0 · - · - · origem=Ronald (tmux)

Não encontrei novo achado procedente no censo/lavra; o predicado continua `paridade < 90` e `plantoes >= 6`, e a lavra usa esse mesmo universo (`app/ponto/management/commands/censo_fase_12x36.py:227`, `:251`).

O delta 3 morde:

- A guarda recusa `--lavrar --min-plantoes > 6` antes da medição e da escrita (`app/ponto/management/commands/censo_fase_12x36.py:74`).
- O teste primeiro lavra o caso, guarda seu detalhe, executa `--min-plantoes 100 --lavrar` e exige detalhe idêntico (`app/escala/tests/test_o261_v_fase_na_lista.py:142`, `:148`, `:150`).
- Sem a cláusula nova, o filtro elimina as linhas, `_desvio` fica vazio e a lavra substitui o detalhe anterior por vazio; portanto, a igualdade falha (`app/ponto/management/commands/censo_fase_12x36.py:156`, `:227`, `:251`; `app/escala/services/fase_12x36.py:84`).
- Executei somente a expressão real da guarda extraída por AST, em memória, com Python3 `-B`: na base `f517…:72`, valores 1/6/7/100 não recusam; no atual `:74`, 7/100 recusam. Execução da suíte Django e RED/GREEN históricos: [nao verificado]. [nao verificado]

Verificações adicionais:

- `--min-plantoes <= 6` não remove elemento que satisfaz o predicado final `>= 6`; `--ano` sem `--mes` não altera as janelas usadas (`app/ponto/management/commands/censo_fase_12x36.py:80`, `:156`, `:227`).
- O relógio é capturado na borda `handle`; esse mesmo dia alimenta janela, corte dos dias, piso e data da lavra (`app/ponto/management/commands/censo_fase_12x36.py:62`, `:83`, `:106`, `:166`, `:263`).
- A escolha do censo como autoridade e a morte do detector paralelo têm autorização específica registrada; não constitui juiz novo sem corte (`app/docs/PROMPTS.md:1686`; `app/escala/services/fase_12x36.py:67`).
- A escrita da chave está concentrada em `lavrar`, por `update_or_create` com chave/escopo/data; o teste exige uma linha após duas lavras (`app/escala/services/fase_12x36.py:78`; `app/escala/tests/test_o261_v_fase_na_lista.py:158`).

## 30/10 18:39 · sessao 061c3d20e043 · O261-E-CELULA-20 · sonda · origem=Code

**(1) Transcrição dos números.** Fonte: saída fornecida na mensagem; sem conferência independente em PROD. [nao verificado]

```text
(i-a) PASSIVO desligamento: vivas abertas sob ancora TERMINAL via=cascade de desligado=43 (chamados=25); qualquer via de desligado=49
(i-a) via_encerramento das ancoras terminais com viva aberta: {'admin': 519, 'legado': 7, 'sistema': 1171, 'cascade': 46}
(i-b) HIPOTETICO: vivas abertas sob ancora NAO_FECHADA de colab nao desligado=4735 (chamados=1816, colabs=326)
(i-b) destas, seguiriam VIVAS depois da porta desligamento da disputa (disputa fechada antes OU pergunta com resposta sem veredito)=440 (chamados=292)
(i-b) motivo: disputa ja fechada=87 | resposta sem veredito=353
(ii) chamados VIVOS com viva aberta ancorada=1281
(ii) destes, reconciliaria() fecharia HOJE=106
(ii) por modulo: {'disputa_supervisao': 69, 'disputa_supervisao_manual': 7, 'chamados': 3, 'turno_aberto': 5, 'batida_ausente': 22}
(ii) passariam a REATA (todas as vivas reapontaveis)=18 chamados / 31 perguntas | RECUSA=88 chamados / 554 perguntas
ERROS={} total=0
```

A linha `ERROS` informa **zero erros contabilizados**; portanto, a saída não relata contaminação por erro. [nao verificado] Isso não comprova ausência de exceções internas: o cálculo de `pergunta_viva()` captura exceções e pode continuar ou retornar `False`; se ocorreu nesta execução, permanece [nao verificado]. Evidência dos tratamentos: [app/chamados/juizes.py:1073](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/juizes.py:1073), [app/chamados/juizes.py:1084](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/juizes.py:1084).

**(2) A correspondência exata entre rótulos e filtros da sonda não pôde ser verificada:** `logs/o261e/f1_medida_p1.py` não foi encontrado nesta cópia; faltam suas linhas para auditar seleção, exclusões, deduplicação e tratamento de erros. [nao verificado] A regra que ignora `logs/` existe, mas não comprova a causa da ausência: [.gitignore:53](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/.gitignore:53).

O código disponível permite delimitar estes pontos:

- **(i-a): `cascade` não identifica exclusivamente desligamento.** `encerrar()` grava a via associada a `ENCERRAR`, que é `cascade`; a regularização externa também chama essa porta. Logo, selecionar colaborador desligado + via `cascade` não comprova que o encerramento ocorreu por desligamento. [app/chamados/models.py:1293](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/models.py:1293), [app/chamados/catalogo/motor.py:95](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/catalogo/motor.py:95), [app/chamados/services/regularizacao_ext.py:205](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/services/regularizacao_ext.py:205).

- **(i-a)/(i-b): `TERMINAIS` e `NAO_FECHADOS` se sobrepõem.** `TERMINAIS` contém `resolvido`, `fechado`, `cancelado` e `superado`; `NAO_FECHADOS` contém `aberto`, `em_analise`, `registrado` e **`resolvido`**. Portanto, incluir resolvidos em (i-b) corresponde ao conjunto oficial; os dois universos não são complementares. [app/chamados/catalogo/motor.py:24](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/catalogo/motor.py:24), [app/chamados/catalogo/motor.py:30](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/catalogo/motor.py:30).

- **(i-b): os motivos de sobrevivência anunciados correspondem qualitativamente à cascata.** Ela seleciona chamados em `NAO_FECHADOS`, encerra-os e depois percorre apenas disputas abertas, pulando aquelas com `respostas_sem_veredito()`. [app/colaboradores/services/desligamento.py:132](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/colaboradores/services/desligamento.py:132), [app/colaboradores/services/desligamento.py:147](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/colaboradores/services/desligamento.py:147). **A guarda examina toda a disputa**, exigindo validação e materialização nulas, resposta por timestamp **ou** texto e dia não trancado; não exige `pergunta_viva()`. Uma sonda que examinasse somente a pergunta contada aplicaria outro filtro; se fez isso, [nao verificado]. [app/chamados/models.py:1524](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/models.py:1524).

- **(ii): `reconciliaria()` mede autorização da porta naquele instante.** Compartilha a decisão de `reconciliar()`, usando `registrar=False`; consulta o motor e a guarda da premissa. O método isolado não verifica o gatilho que levaria uma rotina a chamá-lo. [app/chamados/models.py:434](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/models.py:434), [app/chamados/models.py:447](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/models.py:447), [app/chamados/models.py:467](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/models.py:467). “Fecharia” significa aqui passar a **`resolvido`**, conforme o evento `RECONCILIAR`. [app/chamados/catalogo/motor.py:111](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/catalogo/motor.py:111).

- **REATA/RECUSA:** a porta efetiva consulta `resolvedora_valida()` antes de escrever o destino. O alvo e o predicado usados pela sonda para produzir 18/88 não puderam ser conferidos. [app/chamados/services/fio.py:792](/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/services/fio.py:792); [nao verificado].

**Não é possível apontar como comprovado um filtro divergente da sonda sem seu conteúdo.** Também permanecem [nao verificado] o significado operacional de “vivas abertas”, a escolha da âncora e se o histograma conta perguntas ou âncoras distintas.

## 30/10 18:38 · sessao a12c7f21f5be · - · - · origem=Ronald (tmux)

Evidências relevantes aos itens (i-a)/(i-b), sem atribuir filtros à sonda ausente [não verificado]:

- `VIVOS = ('aberto', 'em_analise', 'registrado')`; `TERMINAIS = ('resolvido', 'fechado', 'cancelado', 'superado')`; `NAO_FECHADOS = VIVOS + ('resolvido',)`. Logo, **resolvido pertence tanto a TERMINAIS quanto a NAO_FECHADOS**; esses universos não são complementares. `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:31`, `app/chamados/catalogo/motor.py:32`.
- O desligamento encerra os chamados do colaborador em `NAO_FECHADOS`, depois percorre somente disputas com `fechada_em IS NULL`; pula qualquer disputa cuja lista `respostas_sem_veredito()` não seja vazia; fecha as demais pela porta `desligamento`. `app/colaboradores/services/desligamento.py:132`, `app/colaboradores/services/desligamento.py:137`, `app/colaboradores/services/desligamento.py:147`, `app/colaboradores/services/desligamento.py:148`, `app/colaboradores/services/desligamento.py:150`.
- Isso sustenta qualitativamente os dois motivos de sobrevivência anunciados: disputa previamente fechada não entra na seleção; disputa aberta com resposta sem veredito é pulada. Os totais 440/87/353 e a fidelidade dos filtros da sonda seguem [não verificado].
- `respostas_sem_veredito()` examina **todas as perguntas da disputa**, exigindo `validada_em is None`, `materializada_em is None`, resposta indicada por timestamp **OU** texto e dia não trancado. Não exige `pergunta_viva(q)`. `app/chamados/models.py:1524`, `app/chamados/models.py:1525`, `app/chamados/models.py:1526`, `app/chamados/models.py:1527`, `app/chamados/models.py:1528`.
- A porta de desligamento fecha cada pergunta viva com `fechar_por_admin`, que escreve `via_resolucao='admin_decidiu'` e `materializada_em`. A vitalidade então reconhece essa via como encerramento por identidade. `app/chamados/models.py:1595`, `app/chamados/models.py:1598`, `app/chamados/models.py:2227`, `app/chamados/models.py:2228`, `app/chamados/juizes.py:1039`, `app/chamados/juizes.py:1044`.
- **`via='cascade'` não prova causa histórica “desligamento”**: todo evento `ENCERRAR` recebe essa via; há chamadores em troca de escala, regularização externa e fechamento retroativo. `app/chamados/catalogo/motor.py:95`, `app/chamados/models.py:1297`, `app/colaboradores/services/vinculo.py:384`, `app/colaboradores/services/vinculo.py:387`, `app/chamados/services/regularizacao_ext.py:205`, `app/chamados/reconciliador.py:498`.
- A função `pergunta_viva()` não testa status da âncora nem fechamento da disputa; decide por validação, tranca, resposta, identidade, marco, célula, janela e materialização. Portanto pergunta viva sob chamado terminal/disputa fechada é possível pelo predicado. `app/chamados/juizes.py:1005`, `app/chamados/juizes.py:1006`, `app/chamados/juizes.py:1010`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1044`, `app/chamados/juizes.py:1050`, `app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1066`, `app/chamados/juizes.py:1076`, `app/chamados/juizes.py:1083`.
- `ERROS={}` externo não estabelece ausência de toda exceção interna: `pergunta_viva` captura exceções, registra por `registrar_engolido` e segue ou retorna `False`. Ocorrência real em PROD é [não verificado]. `app/chamados/juizes.py:1073`, `app/chamados/juizes.py:1075`, `app/chamados/juizes.py:1084`, `app/chamados/juizes.py:1087`.
- Para “competência aberta”, a função de medição existente `competencia_do_dia_entregue()` retorna `None` sem dia, usa a janela da empresa e consulta entrega vigente; a cerca do reconciliador exclui sem dia, entregue e trancado. Qual dessas composições a sonda utiliza é [não verificado]. `app/chamados/services/vigia_fio.py:467`, `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:474`, `app/chamados/reconciliador.py:705`, `app/chamados/reconciliador.py:713`, `app/chamados/reconciliador.py:716`.

## 30/10 18:38 · sessao 8c8c833f4e55 · - · - · origem=Ronald (tmux)

- `reconciliaria()` usa `_decisao_do_fechamento(registrar=False)`, a mesma decisão chamada por `reconciliar()`; assim, mede autorização dessa porta naquele instante. O método, isoladamente, não verifica qual gatilho levaria um cron a chamá-la. (`app/chamados/models.py:434-451`, `app/chamados/models.py:453-472`).
- A decisão relê o status do banco, pede ao motor o evento `RECONCILIAR` e aplica a guarda da premissa. O motor admite `aberto`, `em_analise` e `registrado`, levando-os a **`resolvido`**, não ao status `fechado`. (`app/chamados/models.py:462-471`, `app/chamados/catalogo/motor.py:111`, `app/chamados/catalogo/motor.py:157-164`).
- A guarda permite não-furo; para furo, permite sem dia apurável, premissa regularizada para folha, agrupamento sob pai vivo ou outra cobrança viva do mesmo colaborador/dia. Fora desses casos, o dry-run retorna falso. (`app/chamados/models.py:484-512`).
- Os conjuntos oficiais são `VIVOS=('aberto','em_analise','registrado')`, `TERMINAIS=('resolvido','fechado','cancelado','superado')` e `NAO_FECHADOS=VIVOS+('resolvido',)`. Logo, `resolvido` pertence simultaneamente a `TERMINAIS` e `NAO_FECHADOS`; não se pode tratar os rótulos como conjuntos complementares. (`app/chamados/catalogo/motor.py:24-32`).
- Caso a sonda use `resolvedora_valida` para REATA, esse juiz verifica compatibilidade entre dia/turno: permite ausência de dia, ausência de chamado, escopo diferente de DIA, ausência de data do chamado ou falha de parsing; permite dias adjacentes quando o cruzamento da meia-noite for verdadeiro **ou desconhecido**; recusa as demais divergências. A utilização desse juiz pela sonda e o candidato escolhido são [não verificado]. (`app/chamados/juizes.py:1702-1738`).
- A porta efetiva `reapontar_resolvedora` aplica o dia recebido antes de consultar `resolvedora_valida`, desfaz a data em memória se recusado e, se permitido, escreve o destino e lavra o veredito. Não deve ser executada nesta auditoria somente leitura. (`app/chamados/services/fio.py:785-801`).
- Não pude validar os filtros ou citar linhas de `logs/o261e/f1_medida_p1.py`: o arquivo não foi encontrado na cópia examinada. [não verificado]

## 30/10 18:38 · sessao 5fc863b2c045 · O261-V-FASE · verificador · origem=Code

Encontrei **duas lacunas residuais**: a troca temporal de vínculo ainda atravessa a assinatura, e `--min-plantoes` ainda permite lavrar um recorte. (`app/escala/services/propositor.py:674–680`; `app/ponto/management/commands/censo_fase_12x36.py:72–76,154–155`.)

**1. Onde o delta ainda falha**

- **Residual de A — selo sem mordida no tempo.** O admin lê a proposta do vínculo A; antes do aceite, A deixa de gerar proposta e sobra B com a mesma assinatura. `da_assinatura` encontra uma única correspondência e aceita B. A assinatura exclui `vinculo_id`; a sessão e os formulários transportam apenas pessoa/assinatura; o escritor usa o vínculo da proposta recalculada. A guarda nova resolve a colisão simultânea, mas não essa substituição temporal. (`app/escala/services/propositor.py:661–680,755–758`; `app/core/views_ia.py:617–619,646–655`; `app/templates/chamados/partials/modal_fio.html:50–57`; `app/chamados/services/proposta_no_fio.py:107–115`.)

  Reproduzi em memória: preview no EC1, conjunto recalculado contendo somente EC2 com assinatura igual, e ambos os caminhos devolveram EC2. É um contraexemplo dos seletores; ocorrência em produção é **[não verificado]**. (`app/core/views_ia.py:573–579`; `app/chamados/services/proposta_no_fio.py:107–115`.)

- **Residual de F1 — pré-requisito incompleto da lavra integral.** `--lavrar --min-plantoes 100` continua permitido. O filtro remove linhas antes do critério fixo `>= 6`; a lavra pode substituir a fotografia por zero desvios, ainda declarando esse critério. É recorte da entrada, sem mudar o predicado congelado de F3. (`app/ponto/management/commands/censo_fase_12x36.py:44–45,72–76,153–155,225,249–260`; `app/escala/services/fase_12x36.py:84–86`.)

O rastreio dos valores ficou assim:

| Valor | Quem produz/escreve | Quem lê |
|---|---|---|
| Retorno de `da_assinatura` | Produz `(p, erro)` em `app/escala/services/propositor.py:674–680` | Chat consome `p` e descarta o diagnóstico: `app/core/views_ia.py:575–576`. Fio consome apenas o erro e volta à primeira proposta: `app/chamados/services/proposta_no_fio.py:107–115`. |
| Corte da lavra | `_hoje`/`_encerrado`: `app/ponto/management/commands/censo_fase_12x36.py:62–63` | Filtra `furos`/`folga_trab`: `:164–165`; alimenta contagens e dias: `:195–213`; payload: `:249–261`; persistência: `app/escala/services/fase_12x36.py:84–86`. |
| Retorno de `_proposta_de_escala` | Produz `(p, assinatura)` ou `(None, None)`: `app/core/views_ia.py:573–580` | Preview: `:588–619`; aceite e chamada ao escritor: `:646–655`. |

A lavra é relida centralmente por `ultimo_lavrado`; seus consumidores são C10, propositor, fila, pauta, API e vigia. (`app/escala/services/fase_12x36.py:90–105,116`; `app/escala/services/cadastro_realidade.py:339`; `app/escala/services/propositor.py:491–492`; `app/chamados/services/fila_por_causa.py:174`; `app/api/views_mensageria.py:1672`; `app/ponto/management/commands/vigiar_familia_escala.py:24`.)

Nos caminhos rastreados, a aplicação continua apontando ao mesmo escritor, e o piso continua vindo da mesma função; não encontrei segundo escritor ou derivação paralela nesses pontos. (`app/core/intake.py:49–53`; `app/core/views_ia.py:655`; `app/chamados/services/proposta_no_fio.py:145`; `app/escala/services/propositor.py:590–610`; `app/ponto/management/commands/censo_fase_12x36.py:104`.)

Quanto ao relógio, o cron declarado roda às 07:38 e o comando usa um único `_hoje`. Violação temporal adicional nesse caminho é **[não verificado]**. (`app/config/crons.py:614–615`; `app/ponto/management/commands/censo_fase_12x36.py:62–63,164–165`.)

**2. Dependência do comportamento antigo**

- **`assinatura_esperada=None`: preservado.** O preview continua recebendo a primeira proposta da pessoa e guardando sua assinatura. (`app/core/views_ia.py:577–579,588,617–619`; base `7753f9df…/app/core/views_ia.py:573–578`.)
- **Chat com assinatura única: preservado.** Continua encontrando a correspondência mesmo quando ela não é a primeira. A diferença é recusar múltiplas correspondências. (`app/core/views_ia.py:573–576`; `app/escala/services/propositor.py:674–680`; base `7753f9df…/app/core/views_ia.py:573–579`.)
- **Fio: preservada a exigência da primeira proposta.** `_p` é ignorado; uma assinatura única na segunda posição continua sendo recusada. A ordem permanece ausência → primeira/comparação, com ambiguidade inserida entre elas. (`app/chamados/services/proposta_no_fio.py:102–115`; base `7753f9df…/app/chamados/services/proposta_no_fio.py:102–111`.)
- Gate continua antes do recálculo; motivo obrigatório continua antes dele na recusa; tratamento de grade por dia continua depois. Não identifiquei dependência quebrada nessa ordem. (`app/chamados/services/proposta_no_fio.py:130–145,162–170`.)

**3. Os testes novos mordem?** [nao verificado]

- **Assinatura: morde o helper.** Executei o método isolado em memória: passou; substituir a guarda por “aceitar a primeira” fez falhar. Porém ele não chama chat, fio ou escritor, nem cobre A→B; retirar a integração dos consumidores não afeta suas asserções. (`app/escala/tests/test_o261_v_fase_na_lista.py:252–265`.)
- **Ontem: pela leitura, morde a regressão específica.** Exige excluir 05/11 preservando um dia antigo. A fixture é diurna 07–19; não demonstra um turno noturno efetivamente ainda vivo. (`app/escala/tests/test_o261_v_fase_na_lista.py:60–61,130–137`.)
- **Recorte: pela leitura, morde empresa/mês.** Exige preservar a fotografia após ambas as tentativas, mas não cobre `--min-plantoes`. (`app/escala/tests/test_o261_v_fase_na_lista.py:139–149`.)

Execução da suíte Django e validação com banco: **[não verificado]**. [nao verificado]

## 30/10 18:36 · sessao c74e6237718f · - · - · origem=Ronald (tmux)

Achado procedente: **selo sem mordida na troca temporal de vínculo A→B**. A assinatura exclui `vinculo_id`; `da_assinatura` recusa somente quando há mais de uma correspondência na corrida atual. Se A foi exibido e depois deixou de gerar proposta, B com o mesmo conteúdo passa a ser a única correspondência e é aceito sob o aceite de A. (`app/escala/services/propositor.py:661-680`.)

O caminho completo existe:

- O chat guarda apenas colaborador e assinatura e, no aceite, aplica a proposta encontrada por essa assinatura. (`app/core/views_ia.py:617-619`, `app/core/views_ia.py:646-655`.)
- O fio envia somente a assinatura; `_recalcular` recusa duplicatas atuais, depois aceita a primeira proposta se a assinatura coincide. (`app/templates/chamados/partials/modal_fio.html:50-57`, `app/chamados/services/fio.py:505-512`, `app/chamados/services/proposta_no_fio.py:107-115`.)
- A escrita usa o `vinculo_id` da proposta encontrada, sem receber o vínculo originalmente exibido. (`app/escala/services/propositor.py:755-765`.)

Cenário possível por leitura: A e B com ritmo diário sob 12x36 podem receber o mesmo template diário, com âncora `None` e mesmo piso de aplicação; corrigir A por outra tela remove sua proposta enquanto B permanece. É inferência do algoritmo, não ocorrência em produção. (`app/escala/services/propositor.py:161-185`, `app/escala/services/propositor.py:327-329`, `app/escala/services/propositor.py:495-558`, `app/escala/services/propositor.py:590-592`.) Transportar o vínculo esperado separadamente preservaria a chave de silêncio da recusa. (`app/escala/services/propositor.py:691-710`.)

Quanto à compatibilidade: `assinatura_esperada=None` continua escolhendo a primeira proposta da pessoa; o preview depende desse ramo. `_recalcular` preserva a checagem de ausência antes da comparação da primeira proposta, acrescentando a recusa de ambiguidade entre ambas. (`app/core/views_ia.py:573-588`, `app/chamados/services/proposta_no_fio.py:102-115`; base `7753f9df…/app/core/views_ia.py:573-578`, `7753f9df…/app/chamados/services/proposta_no_fio.py:102-111`.)

O teste novo morde a duplicidade simultânea do helper, mas não chama fio, chat ou escritor e não testa a transição A→B; retirar as chamadas do helper dos consumidores não afeta suas asserções. (`app/escala/tests/test_o261_v_fase_na_lista.py:252-265`.) Os testes antigos verificam assinatura diferente, desaparecimento total, gate e motivo obrigatório; nenhum cobre outro vínculo sobrevivente com assinatura igual. (`app/chamados/tests/test_proposta_no_fio.py:94-175`.)

## 30/10 18:36 · sessao 4b9d5d256dc1 · - · - · origem=Ronald (tmux)

Achado procedente: **F1 residual — `--min-plantoes` ainda permite substituir a lavra por um recorte**. O argumento aceita qualquer inteiro, o filtro elimina linhas antes do critério fixo de desvio (`>= 6`), e a recusa nova só considera `--empresa`/`--mes`. Portanto `--lavrar --min-plantoes 100` pode gravar “zero desvios”, mantendo no payload a descrição `>= 6`; o escritor sobrescreve a fotografia do mesmo dia. Isso trata do universo da lavra, sem alterar o predicado congelado de F3. Evidências: `app/ponto/management/commands/censo_fase_12x36.py:44`, `:72`, `:153`, `:225`, `:249`, `:260`; `app/escala/services/fase_12x36.py:84`.

Escritores/leitores rastreados:

- Corte: `_hoje` e `_encerrado` são definidos no comando; `furos` e `folga_trab` usam o corte e alimentam contagens e itens por dia. `app/ponto/management/commands/censo_fase_12x36.py:62`, `:164`, `:195`, `:206`.
- Lavra: o comando monta o payload e chama `fase_12x36.lavrar`; esta grava `MetricaSnapshot`. `app/ponto/management/commands/censo_fase_12x36.py:248`; `app/escala/services/fase_12x36.py:78`.
- Leitura central: `ultimo_lavrado`, selecionando a mais recente até a data opcional. `app/escala/services/fase_12x36.py:90`.
- Leitores: C10 (`itens`), propositor (`colab`, `ec_do_dia`), fila (`colabs_de_desvio`), pauta, API e vigia. `app/escala/services/cadastro_realidade.py:339`; `app/escala/services/propositor.py:491`; `app/chamados/services/fila_por_causa.py:174`; `app/escala/services/fase_12x36.py:116`; `app/api/views_mensageria.py:1672`; `app/ponto/management/commands/vigiar_familia_escala.py:24`.

Os dois testes novos do censo mordem o comportamento específico alterado: o temporal exige retirar ontem preservando um dia antigo; o de recorte exige preservar a fotografia após `--empresa` e `--mes`. Entretanto, o primeiro usa template diurno 07–19 e não exercita um turno noturno ainda vivo; o segundo não cobre `--min-plantoes`. Evidências: `app/escala/tests/test_o261_v_fase_na_lista.py:60`, `:130`, `:139`.

Sobre relógio: o cron declarado roda às 07:38, enquanto a execução manual não tem guarda horária. O envelope de 26 h pertence ao fechamento do par; a expiração da aberta usa fim previsto + 4 h. Não provei uma violação para um template 12x36 normal com o dado disponível; ocorrência em produção é [não verificado]. Evidências: `app/config/crons.py:614`; `app/ponto/management/commands/censo_fase_12x36.py:62`; `app/ponto/turnos.py:1431`, `:1997`.

Comparação textual com a cópia-base confirmou que o delta do censo contém somente o novo corte e a recusa de recortes por empresa/mês; execução de testes não realizada. [não verificado] [nao verificado]

## 30/10 18:32 · sessao 5d1a28e5c21b · O261-V-FASE · verificador · origem=Code

**Parecer: reprovo esta construção.** A abertura para vários ECs da mesma pessoa não foi acompanhada pela identidade da proposta no aceite: o fio escolhe a primeira proposta, e a assinatura não inclui `vinculo_id`. Isso permite aceitar outro vínculo sob a assinatura apresentada anteriormente. `app/escala/services/propositor.py:452`, `app/escala/services/propositor.py:661`, `app/chamados/services/proposta_no_fio.py:106`.

A análise abaixo distingue falhas da costura nova de problemas herdados. A suíte Django, os mutantes e o RED histórico permanecem **[não verificado]**. A colisão de assinatura e sua aceitação por `_recalcular` foram reproduzidas isoladamente em memória, usando as funções reais e substituindo apenas a fonte de propostas; integração com banco **[não verificado]**. `app/escala/services/propositor.py:654`, `app/chamados/services/proposta_no_fio.py:92`.

**1. Quem escreve e quem lê**

O comando calcula o detalhe; `fase_12x36.lavrar()` persiste a fotografia; `ultimo_lavrado()` lê o detalhe e acrescenta os metadados do snapshot. O cron existente já chama `censo_fase_12x36 --min-plantoes 6 --lavrar`. `app/ponto/management/commands/censo_fase_12x36.py:239`, `app/escala/services/fase_12x36.py:78`, `app/escala/services/fase_12x36.py:90`, `app/config/crons.py:614`.

| Campo/valor | Escritor em produção | Leitor em produção |
|---|---|---|
| Chave `desvio_de_cadastro_12x36`, escopo `tenant/0`, `data_ref`, `data_ini` | `lavrar()` faz `update_or_create`. `app/escala/services/fase_12x36.py:84` | `ultimo_lavrado()` filtra pela chave/escopo e escolhe o último snapshot. `app/escala/services/fase_12x36.py:96` |
| `valor` | `len(colabs_de_desvio)`. `app/escala/services/fase_12x36.py:86` | Não localizei leitor específico deste `valor`; os consumidores abaixo contam o detalhe. `app/escala/services/fase_12x36.py:102`, `app/api/views_mensageria.py:1675` |
| `detalhe.colabs_de_desvio` | IDs distintos das linhas em desvio. `app/ponto/management/commands/censo_fase_12x36.py:242` | Pauta, fila por causa, API e comando semanal. `app/escala/services/fase_12x36.py:117`, `app/chamados/services/fila_por_causa.py:174`, `app/api/views_mensageria.py:1675`, `app/ponto/management/commands/vigiar_familia_escala.py:27` |
| `detalhe.linhas` | Resumos por colaborador/competência: `colab`, `empresa`, `competencia`, `ec`, `template`, plantões, casados, paridade, furos e folga trabalhada. `app/ponto/management/commands/censo_fase_12x36.py:245` | API e impressão do comando semanal. `app/api/views_mensageria.py:1677`, `app/ponto/management/commands/vigiar_familia_escala.py:28` |
| `detalhe.itens` | Dias das linhas em desvio, acrescidos de colaborador, empresa, competência e dono. `app/ponto/management/commands/censo_fase_12x36.py:248` | Formatador C10 e gatilho do propositor. `app/escala/services/cadastro_realidade.py:339`, `app/escala/services/propositor.py:491` |
| `data`, `tipo`, `template_do_dia` | Produzidos a partir de furos/folgas trabalhadas e do vínculo escolhido para cada dia. `app/ponto/management/commands/censo_fase_12x36.py:197` | C10 conserva os campos pertinentes e monta o texto. `app/escala/services/cadastro_realidade.py:341` |
| `ec_do_dia` | Resultado de `vinculo_do_dia`, levado ao item diário. `app/ponto/management/commands/censo_fase_12x36.py:131`, `app/ponto/management/commands/censo_fase_12x36.py:197` | C10 conserva/exibe o EC; propositor identifica o par `(colab, ec)`. `app/escala/services/cadastro_realidade.py:341`, `app/escala/services/propositor.py:491`, `app/escala/services/propositor.py:513` |
| `dono_do_dia` | Literal `CADASTRO` na lavra. `app/ponto/management/commands/censo_fase_12x36.py:249` | C10, contagem por dono e índice diário usado pelo vigia do fio. `app/escala/services/cadastro_realidade.py:646`, `app/escala/services/cadastro_realidade.py:655`, `app/escala/services/cadastro_realidade.py:718`, `app/chamados/services/vigia_fio.py:266` |
| `aplicavel` | Comparação `dia >= piso_de_aplicacao(empresa, hoje)`. `app/ponto/management/commands/censo_fase_12x36.py:98`, `app/ponto/management/commands/censo_fase_12x36.py:199` | C10 repassa o booleano para `acao()`, que habilita o link. `app/escala/services/cadastro_realidade.py:343`, `app/escala/services/cadastro_realidade.py:647`, `app/escala/services/cadastro_realidade.py:494` |
| Demais contadores/metadados do detalhe | `linhas_de_desvio`, `linhas_medidas`, `automaticos`, `propostas`, `criterio`, `esperado`. `app/ponto/management/commands/censo_fase_12x36.py:240` | `linhas_medidas` e `criterio` chegam à API/comando; não localizei consumidor específico novo para os demais. `app/api/views_mensageria.py:1675`, `app/ponto/management/commands/vigiar_familia_escala.py:25` |

Para **`cadastro_realidade.lista()`**, `_fase_deslocada()` produz as assinaturas C10; `lista()` acrescenta descrição, dono, ocorrência, ação e URL, agrega por colaborador/posto e produz os totais por assinatura. `app/escala/services/cadastro_realidade.py:335`, `app/escala/services/cadastro_realidade.py:644`, `app/escala/services/cadastro_realidade.py:684`, `app/escala/services/cadastro_realidade.py:694`.

Os leitores são a tela e o PDF, ambos alimentados pela mesma chamada de `lista()`, além do vigia do fio, que consome `data` e `dono_do_dia` pelo índice `dias_na_lista()`. A coluna externa de posto/template continua sendo a do vínculo de hoje; o EC histórico aparece dentro do detalhe C10. `app/escala/views.py:490`, `app/templates/escala/cadastro_x_realidade.html:21`, `app/escala/services/cadastro_realidade.py:752`, `app/chamados/services/vigia_fio.py:259`, `app/escala/services/cadastro_realidade.py:514`, `app/escala/services/cadastro_realidade.py:345`.

Para **`propositor.propostas()`**, o escritor do retorno é o próprio propositor, sem persistir a proposta. Cada proposta recebe identidade do colaborador/EC, cadastro atual, evidência, motivo de entrada, `aplicar_desde` e chamados que morreriam; o envelope contém `fonte`, `n`, `propostas`, `universo`, `janela_dias` e os contadores/listas de ritmo e pausa. `app/escala/services/propositor.py:537`, `app/escala/services/propositor.py:573`, `app/escala/services/propositor.py:583`.

| Leitor de `propostas()` | Uso |
|---|---|
| Fio | Exibe a primeira proposta; recalcula antes de aceitar/recusar. `app/chamados/services/proposta_no_fio.py:63`, `app/chamados/services/proposta_no_fio.py:102` |
| Painel | Badge de existência para os colaboradores da página. `app/chamados/services/painel.py:520` |
| Ficha | Resume a primeira proposta e sua aplicabilidade. `app/colaboradores/services/ficha.py:176` |
| Chat | Recalcula para preview/confirmação. `app/core/views_ia.py:572` |
| API mensageria | Formata propostas para resposta. `app/api/views_mensageria.py:2019` |
| Pauta e comandos | Consomem envelope, propostas e evidências. `app/escala/services/propositor.py:638`, `app/escala/management/commands/propostas_de_escala.py:26`, `app/escala/management/commands/ritmo_discordante.py:18` |

A decisão, separadamente, é gravada por `recusar()`/`aplicar()` em `DecisaoProposta`. O aceite chama a porta existente de correção retroativa. `app/escala/services/propositor.py:690`, `app/escala/services/propositor.py:738`, `app/escala/services/propositor.py:746`.

**`piso_de_aplicacao`** calcula o início da competência corrente por `janela_atual`; no tratamento de exceção devolve `hoje`. Seus leitores são o censo, o filtro de EC encerrado e a montagem de `aplicar_desde`, através do wrapper `_piso_de_aplicacao`. `app/escala/services/propositor.py:595`, `app/escala/services/propositor.py:609`, `app/ponto/management/commands/censo_fase_12x36.py:98`, `app/escala/services/propositor.py:459`, `app/escala/services/propositor.py:558`, `app/escala/services/propositor.py:592`.

**2. Achados**

**A. P1 — Aceite e recusa não distinguem os ECs agora admitidos. Classe: identidade incompleta na porta; consumidor não migrado.** A assinatura contém somente tipo/template/âncora/início. O fio recalcula, pega `[0]` e compara essa assinatura; a recusa também cala por colaborador e assinatura. `app/escala/services/propositor.py:661`, `app/chamados/services/proposta_no_fio.py:106`, `app/escala/services/propositor.py:580`, `app/escala/services/propositor.py:674`.

Contraexemplo: hoje 16/11, corte 21; A é 12x36 de 19/10–01/11, B é 12x36 de 02–15/11, ambos 08–16; há batidas completas segunda–sexta nos dois trechos e template 5x2 08–16 disponível. Ambos podem propor o mesmo template, sem âncora, desde 21/10. Se outro admin corrige A entre preview e POST, sobra B com a mesma assinatura: `_recalcular` aceita B sob o aceite apresentado para A. Recusar A também pode calar B. O candidato por ritmo permite essa troca. `app/escala/services/propositor.py:161`, `app/escala/services/propositor.py:183`, `app/chamados/services/proposta_no_fio.py:107`. Caso completo com Django **[não verificado]**; colisão e aceite isolados reproduzidos nas funções citadas.

**B. P1 — A ação C10 não seleciona o EC indicado pela linha. Classe: perda da identidade da fonte na passagem ao consumidor.** A assinatura conserva `ec_do_dia`, mas a URL transporta somente o colaborador. O fio escolhe a primeira proposta, cuja ordenação prioriza chamados resolvidos. Havendo A e B elegíveis, clicar na linha de A pode apresentar B, mesmo com assinaturas diferentes. `app/escala/services/cadastro_realidade.py:341`, `app/escala/services/cadastro_realidade.py:495`, `app/chamados/services/proposta_no_fio.py:66`, `app/escala/services/propositor.py:582`.

**C. P2 — C10 oferece correção sem proposta disponível. Classe: ação antes do pré-requisito.** `aplicavel` significa apenas “depois do piso”; o censo exige seis plantões por competência, enquanto o propositor exige seis por EC. `app/ponto/management/commands/censo_fase_12x36.py:199`, `app/ponto/management/commands/censo_fase_12x36.py:216`, `app/escala/services/propositor.py:502`.

Contraexemplo: hoje 05/11; A cobre 21–28/10, prevê ímpares e tem quatro pares completos em 22/24/26/28; B cobre desde 29/10 e tem três plantões corretos em 30/10, 01/11 e 03/11. A competência tem sete plantões, paridade 42,9% e C10 de A com ação habilitada; A tem quatro dias batidos e B três, portanto nenhum ganha proposta. `app/ponto/management/commands/censo_fase_12x36.py:150`, `app/escala/services/cadastro_realidade.py:494`, `app/escala/services/propositor.py:497`. Reprodução Django **[não verificado]**.

**D. P2 — O novo teto é somente por data. Classe: teto temporal insuficiente.** `d < hoje` exclui hoje/futuro dos itens, mas não prova encerramento do turno de ontem; a lei explicitamente rejeita esse teto para cross-meia-noite. `app/ponto/management/commands/censo_fase_12x36.py:155`, `CLAUDE.md:330`.

Contraexemplo: cron de 05/11 às 07:38; escala 22–10, âncora 21/10; plantões completos deslocados em 22/24/26/28/30 de outubro e 01/03 de novembro já colocam a competência em desvio. O previsto de 04/11, sem batida, vira `trabalho_sem_batida` antes de seu término às 10h de 05/11. `app/config/crons.py:614`, `app/ponto/management/commands/censo_fase_12x36.py:197`. Reprodução Django **[não verificado]**.

**E. P2 — A fila continua julgando todos os dias pelo colaborador. Classe: redução indevida da fonte única.** A migração lê apenas `colabs_de_desvio`; depois qualquer chamado dessa pessoa cai em `grade_errada`, sem consultar o dia/EC lavrado. `app/chamados/services/fila_por_causa.py:174`, `app/chamados/services/fila_por_causa.py:220`.

Contraexemplo: A errado até 28/10, B correto desde 29/10; uma falta real em 03/11 sob B recebe causa `grade_errada` porque A mantém o colaborador no censo. A simplificação é herdada, mas a nova fonte já fornece a granularidade que esse leitor descarta. `app/escala/tests/test_o261_v_fase_na_lista.py:13`, `app/ponto/management/commands/censo_fase_12x36.py:248`. Reprodução Django **[não verificado]**.

**F. Riscos herdados que passam a afetar a autoridade única.**

- **Lavra parcial substitui a global:** `--empresa`/`--mes` estreitam a medição, mas a escrita usa sempre tenant/0/data. Uma execução `--empresa A --lavrar` pode retirar B da fotografia lida por todos. Classe: universo truncado na fonte; LEI-AKITA 8. `app/ponto/management/commands/censo_fase_12x36.py:74`, `app/ponto/management/commands/censo_fase_12x36.py:90`, `app/escala/services/fase_12x36.py:84`, `CLAUDE.md:35`.
- **`aplicavel` envelhece na virada:** em 21/11 antes da nova lavra, o snapshot de 20/11 ainda pode habilitar dias anteriores ao novo piso. O leitor aceita a última fotografia sem verificar essa compatibilidade temporal. Classe: pré-requisito temporal da fotografia. `app/escala/services/fase_12x36.py:99`, `app/escala/services/cadastro_realidade.py:343`, `app/escala/services/cadastro_realidade.py:494`.
- **“Plantão real” não exige par fechado:** o censo inclui toda `data_turno` retornada, enquanto o wrapper não exclui abertos. Seis entradas soltas podem atingir o piso de desvio. Isso é herdado do juiz escolhido, portanto não atribuo ao diff uma alteração desse predicado. `app/ponto/management/commands/censo_fase_12x36.py:147`, `app/ponto/turnos.py:1973`, `app/ponto/management/commands/censo_fase_12x36.py:216`.

**Conferência das outras classes pedidas:**

| Classe | Resultado |
|---|---|
| Segundo juiz | Não encontrei sobrevivência executável de `fora_de_fase`; os consumidores passaram à lavra. Os quatro sítios de âncora permanecem declaradamente fora desta obra. `app/escala/services/fase_12x36.py:10`, `app/escala/services/fase_12x36.py:67` |
| Segundo escritor fora da porta | Não encontrei introdução: intenção → propositor → porta retroativa → mutador do vínculo. `app/core/intake.py:49`, `app/chamados/services/proposta_no_fio.py:141`, `app/escala/services/propositor.py:738`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:211` |
| Relógio fora da borda | Permanece herdado: `_recusadas_recentes(hoje)` ignora o dia recebido e chama `timezone.now()`. O piso público usa o argumento recebido. `app/escala/services/propositor.py:666`, `app/escala/services/propositor.py:673`, `app/escala/services/propositor.py:609` |
| Ordem de trava / L-052 | Não encontrei nova aquisição que sustente acusação de inversão; a regeneração mantém `atomic`, guarda de exportadas e proteção de células humanas. `app/ponto/portas/celula.py:199`, `app/ponto/portas/celula.py:206`, `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:251` |
| Custo de frota no request | C10 lê snapshot; permanecem chamadas de frota no chat/API de propostas, anteriores a esta costura. Tempo/custo atual **[não verificado]**. `app/escala/services/cadastro_realidade.py:339`, `app/core/views_ia.py:572`, `app/api/views_mensageria.py:2019` |
| Pouso antes do pré-requisito | A base lavrava sem `itens`/`linhas`; os novos leitores aceitam ausência desses campos como listas vazias. É necessário conferir uma lavra no formato novo antes de declarar a migração efetiva; execução desse pré-requisito **[não verificado]**. `d80c98a5:app/ponto/management/commands/censo_fase_12x36.py:203`, `app/escala/services/cadastro_realidade.py:339`, `app/escala/services/propositor.py:492` |

**3. Os testes mordem?** [nao verificado]

A tabela descreve a consequência das asserções por leitura estática; execução dos testes e dos mutantes **[não verificado]**. [nao verificado]

| Teste novo em `test_o261_v_fase_na_lista.py` | Mordida e limite | [nao verificado]
|---|---|
| Lavra corrente — `app/escala/tests/test_o261_v_fase_na_lista.py:94` | Retirar dias exigidos, EC histórico, dono ou aplicabilidade corrente quebra. Não exige conjunto exato nem unicidade. |
| Não atribuir a B — `app/escala/tests/test_o261_v_fase_na_lista.py:108` | Atribuir qualquer item a B quebra; outro EC incorreto pode escapar. |
| Hoje/futuro — `app/escala/tests/test_o261_v_fase_na_lista.py:114` | Retirar o corte dos itens quebra. Não cobre ontem ainda em curso nem influência de turno aberto no julgamento. |
| Competência anterior — `app/escala/tests/test_o261_v_fase_na_lista.py:121` | Omitir os quatro dias examinados, trocar seu EC ou habilitá-los quebra. |
| Lavra sem âncora — `app/escala/tests/test_o261_v_fase_na_lista.py:130` | Acrescentar chaves proibidas quebra; isoladamente passa com itens vazios. |
| Idempotência — `app/escala/tests/test_o261_v_fase_na_lista.py:136` | Duplicar snapshot do mesmo dia ou variar o detalhe quebra; não prova preservação de outros dias. |
| C10 diária/vigia — `app/escala/tests/test_o261_v_fase_na_lista.py:148` | Retirar C10, EC/dono/URL ou integração diária quebra. Duplicar cada item pode passar: o dicionário colapsa duplicatas e a contagem cresce junto. |
| Dia antes do piso — `app/escala/tests/test_o261_v_fase_na_lista.py:161` | Habilitar URL em 17/10 ou retirar essa linha quebra. |
| Sem âncora no texto — `app/escala/tests/test_o261_v_fase_na_lista.py:168` | Introduzir “ancora” no detalhe/ação quebra; exige C10 presente. |
| Sem lavra — `app/escala/tests/test_o261_v_fase_na_lista.py:176` | Voltar a produzir C10 diretamente das batidas quebra; não exige indicação de ausência de medida. |
| Custo — `app/escala/tests/test_o261_v_fase_na_lista.py:180` | Queries extras no formatador **sem lavra** quebram; não protege o request completo nem a leitura com lavra presente. |
| Detector morreu — `app/escala/tests/test_o261_v_fase_na_lista.py:191` | Selo de forma: também reprova a alternativa autorizada de manter `fora_de_fase` apenas como leitor. |
| Pauta lê lavra — `app/escala/tests/test_o261_v_fase_na_lista.py:196` | Boa mordida: apaga batidas depois de lavrar; voltar ao cálculo vivo quebra. |
| API lê lavra — `app/escala/tests/test_o261_v_fase_na_lista.py:204` | Mesma mordida; não valida todo o formato do retorno. |
| Proposta do encerrado — `app/escala/tests/test_o261_v_fase_na_lista.py:222` | Retirar A ou contaminar com B quebra. **Retirar a leitura da lavra pode passar:** o teste não lavra. |
| Aplicação no EC histórico — `app/escala/tests/test_o261_v_fase_na_lista.py:231` | Morde alteração indevida de B e ausência de correção de A. Não passa pelo clique C10→fio nem verifica células/competência lavrada. |

Os dois testes de proposta não criam células previstas nem chamam `_lavrar()`: o previsto fica vazio, `_score` abre a candidatura por razão infinita e o gatilho da lavra fica dispensável. Assim, eles não defendem a costura central `ultimo_lavrado → fase_invertida`. `app/escala/tests/test_o261_v_fase_na_lista.py:63`, `app/escala/tests/test_o261_v_fase_na_lista.py:224`, `app/escala/services/propositor.py:199`, `app/escala/services/propositor.py:483`, `app/escala/services/propositor.py:522`.

Nos alterados:

- **`test_fase_12x36.py`:** coerente, invertido, noturno e ausência de turno agora consultam a lavra real. O invertido é a testemunha positiva contra um detector sempre vazio; os negativos sozinhos não bastariam. `app/escala/tests/test_fase_12x36.py:87`, `app/escala/tests/test_fase_12x36.py:99`, `app/escala/tests/test_fase_12x36.py:112`, `app/escala/tests/test_fase_12x36.py:126`.
- **Pauta zero/nascimento/teto/encerramento:** mordem esses comportamentos; o encerramento exige nova lavra depois da retirada das batidas. `app/escala/tests/test_fase_12x36.py:149`, `app/escala/tests/test_fase_12x36.py:157`, `app/escala/tests/test_fase_12x36.py:167`, `app/escala/tests/test_fase_12x36.py:175`.
- **`test_ancora_sugerida.py`:** saiu o selo do formato do detector morto; permanecem os sete casos puros de sugestão, desempate, evidência curta, ganho, motivo, ausência de âncora e dia trabalhado. Não distinguem piso seis de oito. `app/escala/tests/test_ancora_sugerida.py:43`, `app/escala/tests/test_ancora_sugerida.py:52`, `app/escala/tests/test_ancora_sugerida.py:59`, `app/escala/tests/test_ancora_sugerida.py:67`, `app/escala/tests/test_ancora_sugerida.py:75`, `app/escala/tests/test_ancora_sugerida.py:83`, `app/escala/tests/test_ancora_sugerida.py:90`, `app/escala/tests/test_ancora_sugerida.py:98`.
- **`test_semselo5...`:** o selo morde `ativa=True` detectável e exige uma chamada chamada `vinculo_do

[truncado em 20000 de 22568 caracteres]

## 30/10 18:30 · sessao 00a1bfe531de · - · - · origem=Ronald (tmux)

Achados procedentes no recorte:

1. **Aceite pode atingir outro EC; recusa de um pode calar outro.** O diff permite várias propostas da mesma pessoa (`app/escala/services/propositor.py:443`, `app/escala/services/propositor.py:541`), mas a assinatura continua contendo apenas `tipo/template_id/ancora/aplicar_desde`, sem `vinculo_id` (`app/escala/services/propositor.py:661`). O fio escolhe exclusivamente a primeira proposta e compara apenas essa assinatura (`app/chamados/services/proposta_no_fio.py:102`, `app/chamados/services/proposta_no_fio.py:106`). A recusa também usa apenas colaborador e assinatura (`app/escala/services/propositor.py:580`, `app/escala/services/propositor.py:674`).
   
   Contraexemplo: hoje 16/11, corte 21; EC A 12x36 de 19/10 a 01/11, EC B 12x36 de 02/11 a 15/11, ambos 08–16; batidas seg–sex nos respectivos trechos, dez dias por EC; template 5x2 08–16 existente. Ambos podem produzir a mesma troca de template, âncora `None`, início 21/10; o candidato por ritmo admite esse template (`app/escala/services/propositor.py:161`, `app/escala/services/propositor.py:183`; sem–sex no modelo: `app/escala/models.py:852`). Se A desaparece após correção por outro admin, o POST da tela de A aceita B com a mesma assinatura (`app/chamados/services/proposta_no_fio.py:107`).
   
   Confirmei a colisão executando somente em memória as funções reais extraídas por AST, com `python3 -B`: A/B retornam `67ed00e7fec03e2c676f82f33a2c1ced`; `_recalcular` aceita B sem erro. Os pontos executados estão em `app/escala/services/propositor.py:654` e `app/chamados/services/proposta_no_fio.py:92`.

2. **C10 oferece ação cujo pré-requisito não existe.** A lista dá link ao fio somente pelo booleano `aplicavel` (`app/escala/services/cadastro_realidade.py:489`, `app/escala/services/cadastro_realidade.py:495`); esse booleano compara dia e piso (`app/ponto/management/commands/censo_fase_12x36.py:199`). O censo exige seis plantões agregados na competência (`app/ponto/management/commands/censo_fase_12x36.py:148`, `app/ponto/management/commands/censo_fase_12x36.py:216`), enquanto o propositor exige seis por EC (`app/escala/services/propositor.py:497`, `app/escala/services/propositor.py:502`).
   
   Contraexemplo: hoje 05/11, corte 21; A 21–28/10, 12x36 07–19 com previsão ímpar e quatro pares completos em 22/24/26/28; B desde 29/10, corretamente prevendo três plantões completos em 30/10, 01/11 e 03/11. A competência tem sete plantões e paridade 42,9%, gerando C10 aplicável de A; A tem quatro dias batidos e B três, portanto nenhum tem proposta. A ação termina em fio sem correção disponível, pelos gates acima.

3. **A ação C10 não transporta a identidade do EC.** Embora a assinatura da lista preserve `ec_do_dia`, o link contém apenas colaborador (`app/escala/services/cadastro_realidade.py:341`, `app/escala/services/cadastro_realidade.py:495`); o fio mostra a primeira proposta por pessoa (`app/chamados/services/proposta_no_fio.py:63`, `app/chamados/services/proposta_no_fio.py:66`), ordenada por chamados resolvidos (`app/escala/services/propositor.py:582`). Havendo propostas para A e B, a linha de A pode abrir uma proposta de B. É o problema de migração do consumidor tornado possível pelo novo universo.

Leitores/escritores de produção:

- `propostas()` calcula e devolve o envelope e seus campos em `app/escala/services/propositor.py:537`, `app/escala/services/propositor.py:583`; não persiste a proposta. Leitores: fio (`app/chamados/services/proposta_no_fio.py:63`, `app/chamados/services/proposta_no_fio.py:102`), badge paginado (`app/chamados/services/painel.py:517`), ficha (`app/colaboradores/services/ficha.py:175`), preview/confirmação do chat (`app/core/views_ia.py:572`, `app/core/views_ia.py:645`), API mensageria (`app/api/views_mensageria.py:2019`), pauta (`app/escala/services/propositor.py:638`), comandos (`app/escala/management/commands/propostas_de_escala.py:26`, `app/escala/management/commands/ritmo_discordante.py:18`).
- `piso_de_aplicacao` calcula o início via `janela_atual` (`app/escala/services/propositor.py:595`, `app/escala/services/propositor.py:609`); leitores diretos: filtro dos EC encerrados (`app/escala/services/propositor.py:459`), wrapper que fornece `aplicar_desde` (`app/escala/services/propositor.py:558`, `app/escala/services/propositor.py:592`) e censo (`app/ponto/management/commands/censo_fase_12x36.py:98`).
- A decisão persiste em `DecisaoProposta` por `recusar`/`aplicar` (`app/escala/services/propositor.py:690`, `app/escala/services/propositor.py:746`). O escritor de escala declarado continua sendo `propositor.aplicar` (`app/core/intake.py:49`), que delega à porta existente (`app/escala/services/propositor.py:738`).

Verificações sem achado novo:

- Não encontrei segundo escritor introduzido nesse recorte: fio delega ao escritor declarado (`app/chamados/services/proposta_no_fio.py:141`), que chama a porta e seu mutador (`app/escala/services/propositor.py:738`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:211`; `app/colaboradores/services/vinculo.py:839`).
- A regeneração mantém guarda de exportadas e células humanas (`app/ponto/portas/celula.py:199`, `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:251`) e usa `atomic` (`app/ponto/portas/celula.py:206`). Não identifiquei nova aquisição de trava que sustente achado de ordem.
- Relógio fora da borda **herdado**: `_recusadas_recentes(hoje)` ignora o argumento e usa `timezone.now()` (`app/escala/services/propositor.py:666`, `app/escala/services/propositor.py:673`). O piso público usa o dia recebido (`app/escala/services/propositor.py:609`).
- Custo de frota no request **herdado e ainda presente** em `app/core/views_ia.py:572` e `app/api/views_mensageria.py:2019`; a rota específica da fase foi migrada para lavra (`app/api/views_mensageria.py:1673`).
- O teste alterado do fio só verifica assinatura Python e texto `__iter__`, sem provar funcionamento de um ID/lista (`app/chamados/tests/test_proposta_no_fio.py:185`). Os testes de tela velha fornecem somente uma proposta e não cobrem colisão entre EC (`app/chamados/tests/test_proposta_no_fio.py:128`, `app/chamados/tests/test_proposta_no_fio.py:140`).
- Os dois novos testes de proposta exercitam apenas A elegível/B inelegível, e a aplicação direta de A (`app/escala/tests/test_o261_v_fase_na_lista.py:222`, `app/escala/tests/test_o261_v_fase_na_lista.py:231`); não passam pelo clique C10→fio nem cobrem dois EC elegíveis.

## 30/10 18:29 · sessao 66325653280c · - · - · origem=Ronald (tmux)

Auditoria somente por leitura; nenhum teste foi executado. [não verificado] [nao verificado]

**Escritor e leitores da lavra**

| Campo | Quem produz/grava | Quem lê em produção |
|---|---|---|
| Chave `desvio_de_cadastro_12x36` | `fase_12x36.CHAVE`, persistida por `lavrar`, chamado pelo censo: `app/escala/services/fase_12x36.py:75`, `:78`, `:84`; `app/ponto/management/commands/censo_fase_12x36.py:239` | `ultimo_lavrado`: `app/escala/services/fase_12x36.py:96` |
| `valor` | `len(colabs_de_desvio)`: `app/escala/services/fase_12x36.py:86` | Não encontrei leitor produtivo específico; os consumidores leem `detalhe` e contam os colaboradores: `app/escala/services/fase_12x36.py:102`, `app/api/views_mensageria.py:1675` |
| `colabs_de_desvio` | Censo: `app/ponto/management/commands/censo_fase_12x36.py:242` | Pauta: `app/escala/services/fase_12x36.py:117`; fila: `app/chamados/services/fila_por_causa.py:174`; API: `app/api/views_mensageria.py:1675`; vigia: `app/ponto/management/commands/vigiar_familia_escala.py:27` |
| `linhas` | Resumo por colaborador e competência: `app/ponto/management/commands/censo_fase_12x36.py:245` | API: `app/api/views_mensageria.py:1677`; comando: `app/ponto/management/commands/vigiar_familia_escala.py:28` |
| `itens` | Dias das competências em desvio: `app/ponto/management/commands/censo_fase_12x36.py:248` | C10: `app/escala/services/cadastro_realidade.py:339`; gatilho de proposta: `app/escala/services/propositor.py:491` |
| `ec_do_dia` | Resultado de `vinculo_do_dia`: `app/ponto/management/commands/censo_fase_12x36.py:131`, `:135`, `:197` | C10: `app/escala/services/cadastro_realidade.py:341`; proposta identifica `(colab, ec)`: `app/escala/services/propositor.py:491`, `:513` |
| `dono_do_dia` | Literal `CADASTRO`: `app/ponto/management/commands/censo_fase_12x36.py:249` | C10: `app/escala/services/cadastro_realidade.py:342`, `:646`; índice para vigia: `:718` |
| `aplicavel` | Comparação com piso do propositor: `app/ponto/management/commands/censo_fase_12x36.py:98`, `:199`, `:203` | C10: `app/escala/services/cadastro_realidade.py:343`, `:647`; ação/link: `:494` |

**Achados a considerar**

1. **Teto por data ainda julga turno não encerrado.** O novo corte é apenas `d < _hoje`, exatamente a proteção que a seção 6 diz ser insuficiente em turno noturno. `app/ponto/management/commands/censo_fase_12x36.py:155`; `CLAUDE.md:330`.

   Contraexemplo: hoje 05/11 às 07:38; cadastro 12x36 das 22h às 10h; 04/11 é previsto, sem batida, e seis plantões anteriores fora do previsto já colocam a competência em desvio. A lavra publica 04/11 como `trabalho_sem_batida` antes do encerramento às 10h. O cron roda às 07:38. `app/ponto/management/commands/censo_fase_12x36.py:197`; `app/config/crons.py:614`. O teste cobre hoje/futuro, mas usa diurno e não cobre ontem ainda em curso. `app/escala/tests/test_o261_v_fase_na_lista.py:114`, `:60`.

2. **Lavratura parcial pode substituir a autoridade global.** `--empresa` filtra o universo; `--mes` também pode restringi-lo; a escrita continua usando a mesma chave tenant/0/data de hoje. `app/ponto/management/commands/censo_fase_12x36.py:39`, `:43`, `:74`, `:90`; `app/escala/services/fase_12x36.py:84`.

   Contraexemplo: A e B possuem fase deslocada; após lavra completa, executar `--empresa A --lavrar` apaga B da fotografia consumida pela lista, fila, pauta e API. É mecanismo herdado que passa a afetar todos esses consumidores pela nova autoridade única. `app/escala/services/fase_12x36.py:86`; `app/escala/services/cadastro_realidade.py:339`.

3. **A fila descarta a granularidade por dia.** A migração lê só colaboradores em desvio; qualquer chamado da pessoa cai em `grade_errada`, independentemente do dia ou EC. `app/chamados/services/fila_por_causa.py:174`, `:220`.

   Contraexemplo adaptado da própria fixture: EC A errado até 28/10; EC B correto desde 29/10; falta real em 03/11 sob B. Como A mantém a pessoa no censo, o chamado da falta sob B recebe causa `grade_errada`. A fixture distingue esses segmentos, mas nenhum teste novo exercita essa classificação. `app/escala/tests/test_o261_v_fase_na_lista.py:13`, `:14`; `app/chamados/services/fila_por_causa.py:221`. A simplificação existia anteriormente; a nova lavra oferece os dias que o leitor continua descartando. `app/ponto/management/commands/censo_fase_12x36.py:248`.

4. **Risco herdado: “plantão real” inclui turno aberto/orfão.** O censo transforma todos os turnos retornados em datas, sem verificar fechamento ou saída; o wrapper retorna também turnos abertos. `app/ponto/management/commands/censo_fase_12x36.py:147`; `app/ponto/turnos.py:1973`.

   Seis entradas soltas em folgas, sem saídas, podem satisfazer o piso e produzir paridade zero, com dono `CADASTRO`. O detector removido exigia turno fechado; o censo escolhido já não exigia. É necessário distinguir defeito herdado do predicado expressamente preservado pelo aval. `app/ponto/management/commands/censo_fase_12x36.py:216`, `:249`.

5. **Foto de `aplicavel` atravessa a mudança de competência.** O último snapshot não exige data atual; a lista utiliza seu booleano sem outro corte. `app/escala/services/fase_12x36.py:96`, `:99`; `app/escala/services/cadastro_realidade.py:343`, `:494`.

   Em 21/11 antes da nova lavra, uma fotografia de 20/11 ainda oferece ação para outubro/novembro anterior ao novo piso. É limitação de frescor/pré-requisito da fotografia, não justificativa para recalcular no request. `app/ponto/management/commands/censo_fase_12x36.py:199`; `app/escala/services/cadastro_realidade.py:493`.

**Testes** [nao verificado]

Os casos novos mordem a existência dos dias, a atribuição ao EC encerrado, os dois lados do piso, ausência de hoje/futuro e idempotência; remover essas partes quebra as respectivas asserções. `app/escala/tests/test_o261_v_fase_na_lista.py:94`, `:108`, `:114`, `:121`, `:136`.

Pauta e API têm boa testemunha: após lavrar, apagam as batidas e exigem manutenção do resultado. Reintroduzir cálculo vivo quebra os casos. `app/escala/tests/test_o261_v_fase_na_lista.py:196`, `:204`.

A ausência de `fora_de_fase` é teste de forma: quebra inclusive uma implementação autorizada que mantivesse o nome apenas como leitor da lavra. `app/escala/tests/test_o261_v_fase_na_lista.py:191`; a própria regra citada permite “ler ou morrer”. `:8`.

O teste sem âncora nos itens não exige itens presentes e pode passar vazio; os demais testes positivos da mesma classe compensam parte dessa vacuidade. `app/escala/tests/test_o261_v_fase_na_lista.py:130`, `:100`.

O teste de uma consulta impede varredura dentro de `_fase_deslocada`, mas não mede todo `lista()` nem request completo. `app/escala/tests/test_o261_v_fase_na_lista.py:180`, `:184`.

**Migração de chamadas**

Não encontrei chamadas/importações executáveis pendentes de `fora_de_fase` ou das constantes removidas em produção, testes, templates ou JS; a implementação foi apagada e os cinco caminhos declarados migraram à lavra. `app/escala/services/fase_12x36.py:67`, `:74`; `app/api/views_mensageria.py:1671`; `app/chamados/services/fila_por_causa.py:153`; `app/escala/services/propositor.py:491`; `app/ponto/management/commands/vigiar_familia_escala.py:22`.

A mensageria apenas repassa o JSON, sem acessar campos antigos; não encontrei consumidor executável não migrado desse formato. `mensageria/nucleo/ferramentas.py:1407`, `:1409`; `mensageria/nucleo/core_client.py:244`.

Restam textos antigos sobre o predicado de dois lados/três ocorrências, sem execução: API, ferramenta e documentação do fio; podem induzir manutenção incorreta, mas não são chamadas órfãs. `app/api/views_mensageria.py:1660`; `mensageria/nucleo/ferramentas.py:1399`; `app/chamados/services/proposta_no_fio.py:17`.

## 30/10 18:28 · sessao a1d2daa0f14f · - · - · origem=Ronald (tmux)

Análise estática; não executei testes nem mutantes, respeitando somente leitura. [não verificado] [nao verificado]

**Testes novos — `app/escala/tests/test_o261_v_fase_na_lista.py`:**

| Teste, linha | Se a cura sair/mudar | [nao verificado]
|---|---|
| `test_MORDE_lavra_da_competencia_CORRENTE...`:94 | Morde ausência dos oito dias exigidos, atribuição a EC B, ausência de dono CADASTRO e `aplicavel=False` nos dias correntes; não verifica conjunto exato nem unicidade. |
| `test_MORDE_nenhum_dia_e_atribuido...`:108 | Morde qualquer item atribuído a B; permite outros ECs incorretos desde que B não apareça. |
| `test_MORDE_hoje_e_futuro_nunca_entram`:114 | Morde itens `>=05/11`; exige saída não vazia. Não prova que turnos reais de hoje/futuro deixaram de influenciar o julgamento. |
| `test_MORDE_competencia_anterior...`:121 | Morde omissão dos dias 17–20/10, EC errado nesses quatro dias ou aplicação antes do piso. |
| `test_lavra_sem_ancora_proposta`:130 | Morde chaves contendo `ancora`, `proposta` ou `virada` nos itens; pode passar sem itens, embora os demais testes façam a lavra positiva. | [nao verificado]
| `test_idempotente`:136 | Morde duplicação de snapshots no mesmo dia ou detalhe variável; não verifica preservação de snapshots de outros dias. |
| `test_MORDE_C10_um_item_por_dia...`:148 | Morde falta de C10 em 25/10, dono/EC/url errados e falta de integração com `dias_na_lista`; **não morde duplicação por dia**: `por_dia` colapsa duplicatas e o total é comparado com `len(c10)`, que também cresce. |
| `test_MORDE_dia_antes_do_piso...`:161 | Morde ausência da C10 em 17/10 ou URL habilitada nesse dia. |
| `test_MORDE_sem_ancora_na_linha`:168 | Morde texto contendo `ancora` em detalhe/ação; exige C10 não vazia. |
| `test_sem_lavra_a_lista...`:176 | Morde C10 calculada das batidas sem snapshot. Não exige explicitamente marcador de ausência de medida. |
| `test_MORDE_a_lista_nao_mede...`:180 | Morde queries adicionais em `_fase_deslocada(None)` **sem lavra**; não mede `lista()` nem cenário com lavra presente. |
| `test_MORDE_fora_de_fase_morreu`:191 | É selo de forma: falha se surgir uma função homônima que apenas lê a lavra, alternativa autorizada pelo pedido literal. |
| `test_MORDE_pauta_le_a_lavra...`:196 | Morde retirada da leitura persistida e volta ao cálculo das batidas, pois apaga batidas após lavrar e exige `n=1`. |
| `test_MORDE_api_da_mensageria...`:204 | Mesma mordida na API; token é mockado, leitura da fase é real. Não valida formato completo de `vinculos`. |
| `test_MORDE_proposta_nasce...`:222 | Morde perda do EC encerrado, contaminação por B e contagem diferente de 11 batidos. **Não morde leitura da lavra pelo propositor**, pois não chama `_lavrar()`. |
| `test_MORDE_aplicar_corrige...`:231 | Morde ausência de correção de A, alteração da âncora/ativa de B e alteração de fim/ativa de A; usa `aplicar` real. Não verifica células, fechamento lavrado, nem passagem pelo POST real do fio. |

Cada avaliação acima deriva das asserções nas respectivas linhas do arquivo indicado. O fixture das propostas cria ECs e batidas, sem células previstas; portanto `prev` fica vazio (`app/escala/services/propositor.py:483` e `:510`), `_score` resulta infinito (`:199`–`:207`) e a candidatura entra por `razao > 1` (`:522`). Remover `ultimo_lavrado`/`invertidos` (`:489`–`:492`, `:513`) deixa esses dois casos capazes de passar.

**Testes alterados:** [nao verificado]

- `test_fase_12x36.py`: os quatro casos passam a consultar a lavra real: coerente não acende (`:87`), invertido acende (`:99`), noturno coerente não acende (`:112`), ausência de turno não acende (`:126`). Só o positivo invertido impede que uma implementação que nunca detecta desvio passe. Os negativos isolados passam por ausência de sinal; juntos há contraste positivo. A migração retira as antigas exigências de três ocorrências de cada lado, coerente com a troca do predicado autorizada; o arquivo ainda conserva essa descrição histórica no cabeçalho (`:14`–`:16`).
- Pauta zero (`test_fase_12x36.py:149`), nascimento (`:157`), teto semanal (`:167`) e encerramento (`:175`) mordem as respectivas ações reais. O último agora exige nova lavra após apagar batidas (`:183`–`:186`); uma pauta que reconta batidas diretamente pode continuar satisfazendo esses casos, sendo a independência da fonte protegida pelo teste novo de `:196`. [nao verificado]
- `test_ancora_sugerida.py`: apenas removeu `NaListaTest`, que inspecionava fonte do detector morto; os sete testes puros permanecem (`:43`, `:52`, `:59`, `:67`, `:75`, `:83`, `:90`). Eles mordem sugestão positiva, desempate recente, recusa com duas batidas, ganho nulo, motivo e pertença da âncora aos dias trabalhados; **não distinguem piso 6 de 8**, embora o código use 6 (`fase_12x36.py:22`) e o cabeçalho do teste ainda descreva 8 (`test_ancora_sugerida.py:15`). [nao verificado]
- `test_semselo5_ativo_nao_responde_por_dia.py`: adiciona `propostas` aos CURADOS (`:38`–`:40`) e remove detector/propositor das exceções antigas. `test_os_curados...` (`:379`) morde `ativa=True` detectável pelo scanner e exige chamada com nome `vinculo_do_dia`; **não prova uso do resultado**. Uma chamada morta satisfaz a presença de nome, e trocar por helper equivalente com outro nome provoca falso vermelho (`:384`–`:388`). As demais provas adversariais testam o scanner, não o comportamento do propositor (`:391`–`:424`).
- `test_proposta_no_fio.py:185`: única alteração sobrevivente é inspeção de assinatura para `colaborador_id` e busca textual de `__iter__` (`:189`–`:192`). **Não compara** respostas para escalar/lista/frota e não limita consultas. Pode permanecer verde com implementação semanticamente incorreta, desde que conserve essas duas formas. O selo textual de pré-carga do detector foi removido com ele; demais testes do arquivo não foram alterados e mockam `propostas`/`aplicar` (`:73`–`:178`). [nao verificado]

**Lacunas relevantes para o parecer:**

- Não há proteção das bordas 5/6 plantões e 89,9/90% nos testes novos/alterados: o golden usa 7 plantões com 42,9% e 10 com 0% (`test_o261_v_fase_na_lista.py:18`–`:25`), enquanto os casos antigos são coerente/invertido (`test_fase_12x36.py:87`–`:124`). Um limiar `<50%` no censo preservaria esses casos, embora viole a regra `<90%` (`censo_fase_12x36.py:216`). [nao verificado]
- Não há teste de virada de competência com lavra antiga e `aplicavel` envelhecido: todos os casos novos estão congelados em 05/11 (`test_o261_v_fase_na_lista.py:91`, `:145`). [nao verificado]
- Novos testes de leitor cobrem pauta e API, não `fila_por_causa` e `vigiar_familia_escala` (`test_o261_v_fase_na_lista.py:189`–`:216`). [nao verificado]
- Não vi evidência de RED executado nesses arquivos; o cabeçalho declara “escritos ANTES” (`test_o261_v_fase_na_lista.py:3`), mas a execução histórica permanece [não verificado]. A lei exige RED evidenciado (`CLAUDE.md:28`, `:348`) e teste que falhe quando a costura sair (`CLAUDE.md:350`–`:355`). [nao verificado]

## 30/10 18:23 · sessao d9783118c056 · O261-E-CELULA-20 · censo · origem=Code

**Há um núcleo de persistência do estado, mas falta uma guarda comum de encerramento que consulte todas as perguntas do chamado.** `_gravar_estado` concentra a transição física; `transicionar`, `reconciliar`, `cancelar` e `encerrar` chegam a ele com guardas diferentes. `pergunta_viva` existe como juiz, porém não é uma condição obrigatória anterior a toda transição terminal. `app/chamados/models.py:122`, `app/chamados/models.py:185`, `app/chamados/models.py:434`, `app/chamados/models.py:527`, `app/chamados/models.py:1288`.

**O campo do chamado é `status_local`, não `status`.** `ENCERRADOS` contém `resolvido/fechado`; `TERMINAIS` acrescenta `cancelado/superado`. `registrado` pertence a `VIVOS`. Arquivar/silenciar é outro eixo, sem alteração de estado. `app/chamados/models.py:46`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:31`, `app/chamados/models.py:81`.

O método foi inspeção estática por operação: AST de atribuições, chamadas ORM, argumentos e dicionários; conferência de `save`, `update`, `bulk_update`, `setattr`, SQL e criação; depois leitura das condições e rastreamento dos chamadores. Testes foram separados dos atores de produção; migrações e comandos operacionais estão identificados abaixo. Não executei Django, testes ou banco, nem criei, editei ou apaguei arquivos. Identidade Git do snapshot como `main` no hash informado, execução real, banco e cron instalado: **[nao verificado]**. [nao verificado]

O limite é explícito: o censo cobre os sítios de código presentes e as expansões estáticas identificáveis; geração dinâmica de campos/modelos, chamadas externas por shell, monkeypatches, triggers e SQL fora da árvore permanecem **[nao verificado]**. O próprio censo existente distingue operações sintáticas de atribuição e verbos ORM, sem provar execução. `bin/esmeril_chamado_censo.py:99`, `bin/esmeril_chamado_censo.py:113`.

**1. Escritores do estado terminal**

A escrita física de transição é `self.status_local = novo`, seguida de `save(update_fields=campos)`. A validação comum exige uma via terminal reconhecida, mas não consulta C6 antes de salvar. A relavra das perguntas acontece **depois** do save quando cruza a fronteira `ENCERRADOS`. `app/chamados/models.py:192`, `app/chamados/models.py:197`, `app/chamados/models.py:204`, `app/chamados/models.py:231`, `app/chamados/models.py:243`.

| Entrada de escrita | Valor e condição | Pergunta viva antes? |
|---|---|---|
| `transicionar` / `fechar` | `resolvido` ou `fechado`, conforme motor/perfil; fechado recusa justificativa pendente; resolvido de furo exige premissa regularizada. `app/chamados/models.py:122`, `app/chamados/models.py:130`, `app/chamados/models.py:165`, `app/chamados/models.py:262` | **Parcial:** para `ENCERRADOS`, consulta a primeira disputa aberta cujo pai é o chamado; bloqueia se vazia ou com `fio_vivo`. Não percorre todas as perguntas que apontam ao chamado como destino. `app/chamados/models.py:142`, `app/chamados/juizes.py:1817`, `app/chamados/juizes.py:1820` |
| `reconciliar` | `VIVOS → resolvido`; relê estado; aplica guarda de premissa para folha, agrupamento sob pai vivo ou outra cobrança viva. `app/chamados/models.py:434`, `app/chamados/models.py:463`, `app/chamados/models.py:474`, `app/chamados/models.py:489`, `app/chamados/models.py:498` | **Não como guarda geral C6.** A decisão é da premissa/regularização. `app/chamados/models.py:474` |
| `cancelar` | `VIVOS → cancelado`, permitido pelo motor. `app/chamados/models.py:527`, `app/chamados/models.py:532`, `app/chamados/models.py:537` | Não. `app/chamados/models.py:527` |
| `superar` | `VIVOS → superado`; superador diferente de si e não terminal. `app/chamados/models.py:543`, `app/chamados/models.py:547`, `app/chamados/models.py:560` | Não; está declarado sem chamador de produção. `bin/esmeril_chamado_censo.py:143` |
| `encerrar` | `aberto/em_analise/registrado/resolvido → fechado`, via cascade. `app/chamados/models.py:1288`, `app/chamados/models.py:1293`, `app/chamados/models.py:1297` | Não. `app/chamados/models.py:1288` |
| `encerrar_por_competencia_trancada` | `VIVOS → fechado`, via `competencia_trancada`; serviço verifica a competência. `app/chamados/models.py:1300`, `app/chamados/models.py:1305`, `app/chamados/models.py:1310`, `app/chamados/services/competencia_trancada.py:158` | Não no método. `app/chamados/models.py:1300` |

No inventário seguinte, **R** significa `reconciliar → resolvido`, **E** significa `encerrar → fechado`, **H** significa `transicionar/fechar → resolvido ou fechado`, e **C** significa `cancelar → cancelado`. Essas entradas herdam as guardas da tabela anterior; verificações adicionais de C6 estão assinaladas. `app/chamados/models.py:262`, `app/chamados/models.py:434`, `app/chamados/models.py:527`, `app/chamados/models.py:1288`.

| Chamador e disparador | Entrada e condição própria |
|---|---|
| Signal de disputa fechada | **R**, pai existente ainda fora de `ENCERRADOS`; o receiver não consulta C6 novamente. `app/chamados/signals.py:28`, `app/chamados/signals.py:33`, `app/chamados/signals.py:42` |
| Reconciliação por colaborador: evento de batida e cron | Retratação quando a premissa morreu; retém irmãs respondidas sem consumo. **R**. `app/chamados/reconciliador.py:439`, `app/chamados/reconciliador.py:520`, `app/chamados/reconciliador.py:525`, `app/chamados/reconciliador.py:1348`, `app/chamados/reconciliador.py:1369` |
| Consumidor do evento da célula | Célula absolveu chamado de turno → retratação **R**. `app/chamados/reconciliador.py:933`, `app/chamados/reconciliador.py:951`, `app/chamados/reconciliador.py:955` |
| Aprovação de justificativa / ajuste retroativo | Fecho forçado com retenção → **E**. `app/chamados/reconciliador.py:487`, `app/chamados/reconciliador.py:494`, `app/chamados/reconciliador.py:498`, `app/ponto/services/justificativa.py:79`, `app/ponto/services/justificativa.py:385` |
| Desligamento | Todos `NAO_FECHADOS` do colaborador → **E**. `app/colaboradores/services/desligamento.py:132`, `app/colaboradores/services/desligamento.py:137` |
| Nova escala/aceite | Chamados vivos anteriores de `escala_aceite` → **E**. `app/colaboradores/services/vinculo.py:373`, `app/colaboradores/services/vinculo.py:384`, `app/colaboradores/services/vinculo.py:387` |
| Regularização externa materializada | Havendo chamado → **E**. `app/chamados/services/regularizacao_ext.py:197`, `app/chamados/services/regularizacao_ext.py:201`, `app/chamados/services/regularizacao_ext.py:205` |
| Competência trancada | Aplicação da tranca → entrada específica, `fechado`. `app/chamados/services/competencia_trancada.py:156`, `app/chamados/services/competencia_trancada.py:160`, `app/chamados/services/competencia_trancada.py:177` |
| Adesão: agrupamento de filhos | Furo no período agrupado sob pai → **R**. `app/chamados/services/adesao.py:102`, `app/chamados/services/adesao.py:107`, `app/chamados/services/adesao.py:112`, `app/chamados/services/adesao.py:115` |
| Adesão: estoque | Pai `nunca_bateu` com batida, fechamento habilitado e fora de dry-run → **R**. `app/chamados/services/adesao.py:142`, `app/chamados/services/adesao.py:148`, `app/chamados/services/adesao.py:150` |
| Evento de batida / pós-vínculo | Batida não retratada na janela do pai → **R**. `app/colaboradores/services/pos_vinculo.py:17`, `app/colaboradores/services/pos_vinculo.py:36`, `app/colaboradores/services/pos_vinculo.py:41` |
| Aceite de sugestão de folga | Fecha LIMBO-FOLGAS vivos do template → **R**. `app/chamados/services/limbo_folgas.py:73`, `app/chamados/services/limbo_folgas.py:91`, `app/chamados/services/limbo_folgas.py:98`, `app/chamados/services/limbo_folgas.py:101` |
| Agrupamento sob LIMBO | Furo ainda não agrupado → **R**; detector é chamador. `app/chamados/services/limbo_folgas.py:214`, `app/chamados/services/limbo_folgas.py:221`, `app/ponto/management/commands/detectar_ausencias.py:756` |
| Silêncio por ausência | Ausência aprovada, premissa morta e aplicação habilitada → **R**. `app/chamados/services/silencio_ausencia.py:93`, `app/chamados/services/silencio_ausencia.py:100` |
| Evento de ausência | Ausência saiu de `ABERTAS` → **R**; cron correspondente é vigia. `app/ponto/services/ausencia.py:670`, `app/ponto/services/ausencia.py:678`, `app/ponto/services/ausencia.py:705`, `app/ponto/services/ausencia.py:709` |
| Materialização de ausência respondida | Fecha destino somente quando ausência não está aberta → **R**. `app/chamados/services/materializacao.py:531`, `app/chamados/services/materializacao.py:538` |
| Materialização: destino e clones | **R**, com retenção de irmãs respondidas sem consumo. `app/chamados/services/materializacao.py:679`, `app/chamados/services/materializacao.py:701`, `app/chamados/services/materializacao.py:711`, `app/chamados/services/materializacao.py:717` |
| Entrada sincronizada | Todos os marcos cobrados acesos → **R**. `app/ponto/services/entrada_sincronizada.py:19`, `app/ponto/services/entrada_sincronizada.py:28`, `app/ponto/services/entrada_sincronizada.py:31` |
| Triagem/registro de batida | Entrada fecha falta; saída fecha o turno correspondente, recusando outro turno aberto; exige premissa morta → **R**. `app/ponto/services/triagem_batida.py:522`, `app/ponto/services/triagem_batida.py:535`, `app/ponto/services/triagem_batida.py:580`, `app/ponto/services/triagem_batida.py:593`, `app/ponto/services/triagem_batida.py:597` |
| Cobertura designada/passada | Origem viva e premissa morta; sobrevivente por último → **R**. `app/ponto/services/cobertura.py:114`, `app/ponto/services/cobertura.py:122`, `app/ponto/services/cobertura.py:309`, `app/ponto/services/cobertura.py:318` |
| Admin decide o dia | Fecha perguntas por decisão administrativa e reconcilia chamados → **R**. `app/ponto/services/veredito_celula.py:160`, `app/ponto/services/veredito_celula.py:177`, `app/ponto/services/veredito_celula.py:183` |
| Cartório: lastro | Aplicando, marcos cobrados acesos ou inexistentes → **R**. `app/ponto/services/cartorio.py:1153`, `app/ponto/services/cartorio.py:1163`, `app/ponto/services/cartorio.py:1173`, `app/ponto/services/cartorio.py:1175` |
| Cartório: cobrança indevida | `COBRANCA_EM_DIA_SEM_TRABALHO` ou `COBRANCA_SEM_FURO` → **R**. `app/ponto/services/cartorio.py:1188`, `app/ponto/services/cartorio.py:1196` |
| Cartório: perguntas duplicadas | Sem fio vivo, sem resposta escrita e todas `VIA_DUPLICATA` → **R**. **Aqui há consulta local do fio/C6.** `app/ponto/services/cartorio.py:1258`, `app/ponto/services/cartorio.py:1261`, `app/ponto/services/cartorio.py:1263`, `app/ponto/services/cartorio.py:1266` |
| Fechador de lastro | Quadro `com_lastro`, aplicação habilitada; relê estado vivo → **R**. `app/ponto/services/lastro.py:300`, `app/ponto/services/lastro.py:304`, `app/ponto/services/lastro.py:310`, `app/ponto/services/lastro.py:317` |
| Feriado/folga: evento e cron | Dia espúrio pela autoridade do dia → **R**. `app/chamados/services/feriado_reavaliar.py:8`, `app/chamados/services/feriado_reavaliar.py:23`, `app/chamados/services/feriado_reavaliar.py:36`, `app/chamados/services/feriado_reavaliar.py:47` |
| Cluster espúrio | Premissa morta dos chamados do colaborador com escala → **R**. `app/ponto/services/cluster_espurio.py:35`, `app/ponto/services/cluster_espurio.py:46`, `app/ponto/services/cluster_espurio.py:73`, `app/ponto/services/cluster_espurio.py:80` |
| `detectar_ausencias` | Limpa LIMBO antigo; fecha por afastamento, cobertura ou entrada → **R**. `app/ponto/management/commands/detectar_ausencias.py:158`, `app/ponto/management/commands/detectar_ausencias.py:812`, `app/ponto/management/commands/detectar_ausencias.py:824`, `app/ponto/management/commands/detectar_ausencias.py:834` |
| `detectar_intervalo_ausente` | Premissa morta; aplicação reconcilia, dry consulta possibilidade → **R**. `app/ponto/management/commands/detectar_intervalo_ausente.py:115`, `app/ponto/management/commands/detectar_intervalo_ausente.py:128`, `app/ponto/management/commands/detectar_intervalo_ausente.py:134` |
| `marcar_feriado_do_posto` | Alvos do feriado → **R**. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:116`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:126` |
| `curar_residuo_bug115` | Ata sem furo → **R**. `app/ponto/management/commands/curar_residuo_bug115.py:150` |
| `auditar_chamados_orfaos --fechar` | `disputa_supervisao` sem disputa aberta → **R**. `app/chamados/management/commands/auditar_chamados_orfaos.py:34`, `app/chamados/management/commands/auditar_chamados_orfaos.py:58`, `app/chamados/management/commands/auditar_chamados_orfaos.py:67` |
| `fechar_chamados_meio_periodo --apply` | Usuário assinante superuser ativo → **H**, default `fechado`. `app/ponto/management/commands/fechar_chamados_meio_periodo.py:94`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:103`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109` |
| Avisos de régua / vínculo migrados à lista | `--apply` → **C**. `app/escala/management/commands/fechar_avisos_regua_na_lista.py:32`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:40`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57` |
| Tela de benefício | Aprovar → `resolvido`; outra ação → `fechado`, via **H**. `app/colaboradores/views.py:2055`, `app/colaboradores/views.py:2068` |
| Telas de exceção, agenda, escala e geofence | Decisão → `resolvido`, via **H**. `app/chamados/services/acoes_chamado.py:70`, `app/chamados/services/acoes_chamado.py:108`, `app/chamados/services/acoes_chamado.py:149`, `app/chamados/services/acoes_chamado.py:222`, `app/chamados/services/acoes_chamado.py:277` |
| Resposta livre / fio | Novo estado solicitado, limitado por **H** e RBAC. `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/acoes_chamado.py:315`, `app/chamados/services/fio.py:703`, `app/chamados/services/fio.py:715` |
| Smoke operacional | Exercita transições **H** e recusas. `app/chamados/management/commands/smoke_chamados.py:134`, `app/chamados/management/commands/smoke_chamados.py:147`, `app/chamados/management/commands/smoke_chamados.py:163` |

Há duas superfícies adicionais que **não passam pelo núcleo de transição**:

- **Nascimento:** `criar(status=...)` preserva status diferente de `aberto` e entrega-o ao `objects.create`; o smoke cria estados `fechado/resolvido`. `abrir` monta estado inicial, mas incorpora `defaults` antes do `get_or_create`. Assim, concentrar transições em `_gravar_estado` não fecha a entrada genérica de criação. `app/chamados/models.py:702`, `app/chamados/models.py:704`, `app/chamados/models.py:735`, `app/chamados/models.py:818`, `app/chamados/models.py:827`, `app/chamados/models.py:851`, `app/chamados/management/commands/smoke_chamados.py:122`, `app/chamados/management/commands/smoke_chamados.py:146`.
- **Migração 0039:** `update` direto `cancelado → superado` quando há irmão único do mesmo colaborador/dia nos módulos de furo; reversa `superado → cancelado`. Não passa pelo núcleo ou C6. `app/chamados/migrations/0039_a3_via_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:20`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:33`.

Nos modelos relacionados, `DisputaSupervisao` encerra por `fechada_em`, com porta `fechar/pode_fechar`, não por `status`. `RegularizacaoExterna` possui `status` próprio e FK para chamado: `materializada` é gravado pelo materializador; `recusada` devolve à supervisão e mantém o chamado aberto. `app/chamados/models.py:1504`, `app/chamados/models.py:1530`, `app/chamados/models.py:2744`, `app/chamados/models.py:2763`, `app/chamados/models.py:2772`, `app/chamados/services/regularizacao_ext.py:197`, `app/chamados/services/regularizacao_ext.py:261`.

Não foi localizado outro modelo persistente de chamado com estado terminal na árvore **[nao verificado quanto a modelos dinâmicos/externos]**. O cliente GLPI abre ticket com `status=1` e lê estados externos 5/6; não é um segundo modelo ORM local. `app/chamados/glpi_client.py:1`, `app/chamados/glpi_client.py:126`, `app/chamados/glpi_client.py:161`, `app/chamados/glpi_client.py:166`.

**2. Escritores de `materializada_em`**

O campo da pergunta pertence a **`PerguntaDisputa`**, ligada ao chamado por `chamado_destino`; há outro campo homônimo em **`RegularizacaoExterna`**. `app/chamados/models.py:1935`, `app/chamados/models.py:2379`, `app/chamados/models.py:2387`, `app/chamados/models.py:2744`, `app/chamados/models.py:2766`.

A enumeração abaixo contém **20 sítios explícitos em 19 funções de 7 arquivos**: 13 carimbos de pergunta, 1 carimbo de regularização e 6 limpezas de carimbo. Os dois ramos de `resolver_perguntas_por_chamado_resolvido` são contados separadamente. As referências de escrita estão em cada linha da tabela.

| Escritor / escrita | O que carimba ou limpa; porta consultada; quem chama |
|---|---|
| `PerguntaDisputa.fechar_por_admin` | `admin_decidiu`, instante e autor; sem C6 dentro do método. Chamado pela decisão do dia, validação de folga e fechamento administrativo/desligamento da disputa. C7 calcula vivas, mas a decisão administrativa pode encerrá-las. `app/chamados/models.py:2213`, `app/chamados/models.py:2228`, `app/chamados/models.py:1556`, `app/chamados/models.py:1595`, `app/ponto/services/veredito_celula.py:177`, `app/chamados/services/validacao.py:292` |
| `PerguntaDisputa.fechar_por_competencia_trancada` | `competencia_trancada`, instante e autor; sem C6. Sem chamador produtivo direto localizado **[nao verificado quanto a chamada dinâmica]**; o serviço atual manda não carimbar perguntas. `app/chamados/models.py:2233`, `app/chamados/models.py:2242`, `app/chamados/services/competencia_trancada.py:171`, `bin/esmeril_chamado_censo.py:135` |
| `PerguntaDisputa.encerrar_fantasma` | `fantasma`, instante e autor. Método não julga; consumidor exige C6 viva, sem resposta, marco aceso e dia encerrado antes de chamar. `app/chamados/models.py:2258`, `app/chamados/models.py:2267`, `app/chamados/reconciliador.py:1033`, `app/chamados/reconciliador.py:1039`, `app/chamados/reconciliador.py:1087`, `app/chamados/reconciliador.py:1095` |
| `lavrar_sem_turno_aberto` | `sem_turno_aberto`; sem texto/carimbo/turno aberto e **C6 morta**. Reatador, evento de batida e cron. `app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:399`, `app/chamados/reconciliador.py:402`, `app/chamados/reconciliador.py:1519` |
| `retratar_perguntas_absolvidas_por_celula` | `celula_absolveu`; célula julgada não acusadora, sem marcos pendentes e **C6 morta**. Reconciliação por colaborador: evento e cron. `app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:607`, `app/chamados/reconciliador.py:613`, `app/chamados/reconciliador.py:617`, `app/chamados/reconciliador.py:1350` |
| `_materializar_ausencia_de_resposta` | Absorve perguntas-hora do dia sem carimbo; via `declaracao` ou `ausencia_dia`. Não usa C6 como guarda prévia. Resposta web/API nos escapes `FALTA_DIA/ATESTADO`. `app/chamados/services/materializacao.py:478`, `app/chamados/services/materializacao.py:499`, `app/chamados/services/materializacao.py:509`, `app/chamados/services/materializacao.py:525`, `app/chamados/services/disputa_emissao.py:1292` |
| `_materializar_resposta_hora`, recusa | `paridade_recusada` somente com `fato_do_marco_no_espelho is True`; outra porta de fato, sem C6. Unidade materializadora, validação e lote. `app/chamados/services/materializacao.py:815`, `app/chamados/services/materializacao.py:840`, `app/chamados/services/materializacao.py:858`, `app/chamados/services/materializacao.py:1125` |
| `_carimbar_recusa` | `fato_ocupado` para recusa classificada `COLISAO`, se carimbo nulo; sem C6. Materializador e lote do passivo. `app/chamados/services/materializacao.py:1028`, `app/chamados/services/materializacao.py:1037`, `app/chamados/services/materiali

[truncado em 20000 de 60396 caracteres]

## 30/10 18:22 · sessao d7e74ac23a0e · CODEX-CONTEXTO-DA-CASA-AB2-CORRECAO · censo · origem=Code

A classificação considera a cópia atual. C1, C4, C14, C17 e C19 estão corrigidos: o template lê o rótulo, o teste limita o delta cego, a renderização usa sentinela, a singular não repassa o mapa aos marcos e o teste agora chama a singular alimentada. (`app/templates/colaboradores/partials/_calendario_grade.html:107`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:183`, `:110`; `app/ponto/turnos.py:2080`; `app/ponto/tests/test_c1_dias_encerrados.py:74`)

O1 recebe `n/a` quanto ao ponto indicado do código: a expressão apontada não está na linha 720. A mensagem do commit 5cc57c18 permanece `[não verificado]`. (`app/colaboradores/services/calendario.py:720`)

C10 descreve o teste isolado: o teste seguinte rejeita a mutação global “ontem sempre previsto”. C11 permanece pela ausência de cenário de feriado; já existe cenário de ausência. C26 confirma a alimentação parcial e o julgamento do intervalo; consequência incorreta em produção é `[não verificado]`. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:57`, `:63`, `:133`; `app/ponto/services/cartorio.py:746`, `:853`; `app/ponto/turnos.py:2136`)

Os trechos abaixo sustentam as notas positivas ou parciais. Listar um cenário de teste, sem identificar sua lacuna, foi considerado `EM_PARTE`. [nao verificado]

| Volta/resposta | Itens | Trecho da resposta e referência |
|---|---|---|
| par1/A | O2, C2 | “A régua suspeita também esconde o motivo recebido”; descreve o cruzamento sem fim, não O2 com fim previsto. (`app/colaboradores/services/calendario.py:139`, `:141`, `:722`) |
| par1/A | O3 | “A folha mantém seu próprio critério de dia passado”; toca a separação temporal sem apontar a duplicação como defeito. (`app/colaboradores/services/calendario.py:723`; `app/folha/export.py:1237`) |
| par1/A | O4 | “uma consulta adicional incondicional”; “subtrai uma base que já contém a consulta adicional fixa”. (`app/ponto/turnos.py:2130`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`) |
| par1/A | O5 | “esse instante segue à juíza e essa data segue ao retentor”; não identifica `_agora_retr`. (`app/colaboradores/services/calendario.py:258`, `:712`, `:726`) |
| par1/A | C3 | “a exceção do dia da demissão”; não aponta a divergência contratual da tela. (`app/folha/export.py:1211`, `:1237`) |
| par1/A | C5 | “O RED cego verifica a premissa na juíza e na folha”; não identifica a fabricação local que passaria. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:97`) |
| par1/A | C6, C22 | “A propriedade C1 com células alimentadas não contém batidas para provar o pareamento com essa alimentação”; não discute alimentação divergente do banco. (`app/ponto/tests/test_c1_dias_encerrados.py:62`, `:66`) |
| par1/A | C7–C13 | “meia-noite, turno encadeado anterior a ontem, falta depois do fim, veredito lavrado, folga, ausência e isenção”; enumera os temas, sem apontar essas lacunas. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47`, `:57`, `:61`, `:65`, `:113`, `:124`, `:147`) |
| par1/A | C20 | “encaminha células com chave `(colaborador_id, data)` a `turnos_do_colab`, que exige chave `data` e a embrulha novamente”. (`app/ponto/turnos.py:2132`, `:1928`) |
| par1/A | C23 | “o template é renderizado com sentinela em `status_rotulo`”; não questiona comentário ou conteúdo oculto. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:108`, `:110`) |
| par1/A | C24 | “a mudança repercute além da cor da tela”; identifica os leitores, sem apontar o desaparecimento do botão. (`app/colaboradores/services/calendario.py:204`; `app/templates/colaboradores/partials/_calendario_grade.html:125`) |
| par1/B | O2 | “a resposta da folha ainda pode desaparecer na classificação”; o caso descrito é sem fim e sem vínculo ativo. (`app/colaboradores/services/calendario.py:134`, `:158`, `:722`) |
| par1/B | O3 | “não classificaria essa distinção, isoladamente, como juiz novo ou teto indevido por data”. (`app/colaboradores/services/calendario.py:714`, `:723`) |
| par1/B | O4 | “sempre um SELECT de vínculos”; “esse custo se repete por calendário montado”. (`app/ponto/turnos.py:2130`; `app/relatorios/furos.py:184`) |
| par1/B | O5 | “o prazo de desfazer captura outro `now()`”. (`app/colaboradores/services/calendario.py:343`) |
| par1/B | C2 | “A correção precisa preservar A2”; não identifica a régua suspeita substituindo o motivo. (`app/colaboradores/services/calendario.py:124`, `:139`, `:141`) |
| par1/B | C5 | “A sentinela prova que o template lê o campo; não prova que o rótulo veio da autoridade da folha”; aponta outra limitação da dependência. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:108`) |
| par1/B | C6, C22 | “C1 possui batidas, mas suas chamadas não exercitam esse argumento”; não distingue a fixture alimentada, sem batidas, nem o conteúdo persistido. (`app/ponto/tests/test_c1_dias_encerrados.py:28`, `:62`, `:66`) |
| par1/B | C7–C13 | “antes do turno, meia-noite, turno encadeado além de ontem, depois do fim, veredito lavrado”; também enumera “folga, ausência e isenção”, sem essas lacunas. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47`, `:57`, `:61`, `:65`, `:113`, `:124`, `:147`) |
| par1/B | C18 | “O teste de cobertura também admite que A2 decide antes da cobertura que ele aparenta verificar”. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:134`, `:138`) |
| par1/B | C23 | “A sentinela prova que o template lê o campo”; não verifica a localização no HTML. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:110`) |
| par1/B | C24 | “o vigia que consulta ‘Resolver dia’”; identifica o alcance, sem apontar a retirada da oferta. (`app/chamados/services/vigia_fio.py:294`; `app/colaboradores/services/calendario.py:204`) |
| par2/A | O2, C2 | “se houver régua suspeita, `_classificar` retorna `escala_suspeita` antes de ler `retido_folha`”; trata o caso sem fim. (`app/colaboradores/services/calendario.py:139`, `:141`, `:722`) |
| par2/A | O3 | “Calendário monta `_em_curso` e `_sem_fim`”; rastreia o filtro, sem identificar sua duplicação na folha. (`app/colaboradores/services/calendario.py:714`, `:722`; `app/folha/export.py:1237`) |
| par2/A | O4 | “Acréscimo estrutural comum: uma consulta de vínculos por montagem”; “o custo comum cancela na subtração”. (`app/ponto/turnos.py:2130`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`) |
| par2/A | O5 | “desfazer retratação captura outro instante”. (`app/colaboradores/services/calendario.py:343`) |
| par2/A | C6, C22 | “o C1 alimentado testa DNA, sem geradora concorrente”; identifica a lacuna da geradora, não a alimentação igual ao banco. (`app/ponto/tests/test_c1_dias_encerrados.py:62`, `:64`) |
| par2/A | C8 | “O teste novo preserva apenas `nunca_bateu`; falta o adversarial `concorde`”. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:65`, `:74`) |
| par2/A | C7, C9–C13 | “meia-noite, turno até dois dias depois, falta após encerramento”; também cita testes de veredito, folga, ausência e isenção, sem apontar essas lacunas. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47`, `:57`, `:61`, `:113`, `:124`, `:133`, `:147`) |
| par2/A | C20 | “A chave fica aninhada; a busca da geradora deixa de encontrar a célula”. (`app/ponto/turnos.py:2132`, `:1928`; `app/escala/alimentacao.py:237`) |
| par2/A | C23 | “O render usa sentinela para exigir leitura efetiva de `status_rotulo`”; não aponta a aceitação de conteúdo oculto. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:108`, `:110`) |
| par2/A | C24 | “Resolver dia”; “Template e vigia do fio”; rastreia o leitor sem apontar o desaparecimento da oferta. (`app/colaboradores/services/calendario.py:204`; `app/templates/colaboradores/partials/_calendario_grade.html:125`) |
| par2/B | O2, O3, C2 | “O passado `sem_previsto` sai de `_em_curso`”; “preservando a precedência humana e o veredito”; não aponta O2 com fim nem a duplicação temporal como defeito. (`app/colaboradores/services/calendario.py:714`, `:723`, `:124`, `:141`) |
| par2/B | O4 | “esconde a consulta adicional incondicional de vínculos”. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177`; `app/ponto/turnos.py:2130`) |
| par2/B | O5 | “O segundo `now()` do calendário serve ao prazo de desfazer retratação”. (`app/colaboradores/services/calendario.py:343`) |
| par2/B | C5 | “substituir a consulta por `_cego_folha = {d: 'sem_celula' for d in _sem_fim}` preservaria as expectativas”. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:97`, `:177`; `app/colaboradores/services/calendario.py:722`) |
| par2/B | C6, C22 | “Não cobrem [...] o caminho `turnos=` com batidas nessa nova prova C1”; toca a prova alimentada, sem identificar alimentação igual ao banco ou perda da geradora. (`app/ponto/tests/test_c1_dias_encerrados.py:62`, `:66`) |
| par2/B | C7–C13 | “antes do turno, madrugada, depois do fim, turno encadeado além de ontem, decisão lavrada”; também cita “folga, ausência e isenção”, sem essas lacunas. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47`, `:57`, `:61`, `:65`, `:113`, `:124`, `:147`) |
| par2/B | C18 | “os casos de veredito, folga, ausência e isenção escapam por outros ramos”; não identifica especificamente a prova do universo da cobrança. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:133`, `:138`) |
| par2/B | C20 | “criando chaves aninhadas no adaptador”; “o cartório o utiliza”. (`app/ponto/turnos.py:2132`, `:1928`; `app/ponto/services/cartorio.py:853`) |
| par2/B | C23 | “consumo do campo pelo template mediante sentinela”; não aponta a localização não verificada. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:110`) |
| par2/B | C24 | “a mudança de status também alcança a oferta de ação e sua contagem”; não identifica o desaparecimento em competência anterior. (`app/colaboradores/services/calendario.py:204`, `:914`) |

ITEM|C1|app/templates/colaboradores/partials/_calendario_grade.html:107|Grade não apresenta o rótulo do dia cego produzido pelo calendário.
ITEM|C2|app/colaboradores/services/calendario.py:124|Precedências de ausência, folga, feriado ou régua suspeita podem substituir sem_celula retornado pela folha.
ITEM|C3|app/folha/export.py:1180|Calendário e folha divergem no recorte de admissão/demissão, inclusive no próprio dia da demissão.
ITEM|C4|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:177|Prova relativa de custo não limita o acréscimo constante exclusivo do caminho cego.
ITEM|C5|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:97|Teste aceita fabricar sem_certidao/sem_celula localmente sem depender da resposta da folha. [nao verificado]
ITEM|C6|app/ponto/tests/test_c1_dias_encerrados.py:53|Propriedade C1 não prova equivalência quando a alimentação diverge do conteúdo persistido.
ITEM|C7|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:61|Teste depois do fim não cobre a igualdade agora == fim. [nao verificado]
ITEM|C8|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:65|Proteção de veredito testa somente nunca_bateu e deixa outros vereditos sem cobertura.
ITEM|C9|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47|Teste encadeado não impede manter previsto depois do encerramento desse DNA. [nao verificado]
ITEM|C10|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:57|Teste de meia-noite isolado aceita previsto incondicionalmente para ontem. [nao verificado]
ITEM|C11|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:124|Teste da folga não sela as precedências de ausência e feriado no caminho cego. [nao verificado]
ITEM|C12|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:147|Teste do isento não sela a isenção na folha nem o rótulo apresentado. [nao verificado]
ITEM|C13|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:113|Teste com veredito deixa passar consulta desnecessária à folha preservando o resultado. [nao verificado]
ITEM|C14|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:104|Teste de renderização aceita frase literal em lugar da leitura de status_rotulo. [nao verificado]
ITEM|C15|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:104|Teste de renderização não exercita os demais status do ramo residual. [nao verificado]
ITEM|C16|app/colaboradores/services/calendario.py:57|Rótulo de ausencia promete abono próprio mesmo para ausência de efeito SUPRIME.
ITEM|C17|app/ponto/turnos.py:2079|Repasse singular do mapa por data do fechamento aos marcos por tupla perde o DNA e usa o template.
ITEM|C18|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:133|Teste da ausência pendente não sela o universo da cobrança porque A2 decide antes. [nao verificado]
ITEM|C19|app/ponto/tests/test_c1_dias_encerrados.py:70|Teste C1 não exercita a singular alimentada com mapa por data para proteger a correção do fechamento. [nao verificado]
ITEM|C20|app/ponto/turnos.py:2132|Plural repassa mapa por tupla ao pareador por data, produzindo chave aninhada e perdendo a geradora.
ITEM|C21|app/ponto/services/vigia_de_hora.py:57|Vigia entrega mapa por tupla à singular que o repassa ao pareador por data.
ITEM|C22|app/ponto/tests/test_c1_dias_encerrados.py:62|Fixture C1 alimentada não tem geradora nem batidas para detectar perda da geradora no pareamento.
ITEM|C23|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:110|Asserção textual de renderização não verifica se o atributo está em elemento apresentado, permitindo comentário ou conteúdo oculto.
ITEM|C24|app/colaboradores/services/calendario.py:204|Mudança de falta para previsto pode retirar Resolver dia de passado ainda em curso da competência anterior.
ITEM|C25|app/colaboradores/services/calendario.py:624|Falha capturada na carga A2 deixa cobertura independente alcançar retenção sem a precedência correspondente.
ITEM|C26|app/ponto/services/cartorio.py:853|Segunda passada alimenta somente células reconciliadas, embora dias_encerrados julgue todo o intervalo entre extremos.
NOTA|CALENDARIO-HOJE_38fe9889_par1|O1|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par1|O2|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|O3|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|O4|existia|A=VIU|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|O5|existia|A=EM_PARTE|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|O6|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|O7|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C1|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par1|C2|existia|A=VIU|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C3|existia|A=EM_PARTE|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C4|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par1|C5|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C6|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C7|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C8|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C9|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C10|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C11|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C12|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C13|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C14|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par1|C15|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C16|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C17|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par1|C18|existia|A=NAO_VIU|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C19|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par1|C20|existia|A=VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C21|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C22|existia|A=VIU|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C23|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C24|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par1|C25|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par1|C26|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|O1|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par2|O2|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|O3|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|O4|existia|A=VIU|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|O5|existia|A=VIU|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|O6|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|O7|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C1|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par2|C2|existia|A=VIU|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C4|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par2|C5|existia|A=NAO_VIU|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C6|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C7|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C8|existia|A=VIU|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C9|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C10|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C11|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C12|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C13|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C14|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par2|C15|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C16|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C17|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par2|C18|existia|A=NAO_VIU|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C19|n/a|A=n/a|B=n/a
NOTA|CALENDARIO-HOJE_38fe9889_par2|C20|existia|A=VIU|B=VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C21|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C22|existia|A=VIU|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C23|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C24|existia|A=EM_PARTE|B=EM_PARTE
NOTA|CALENDARIO-HOJE_38fe9889_par2|C25|existia|A=NAO_VIU|B=NAO_VIU
NOTA|CALENDARIO-HOJE_38fe9889_par2|C26|existia|A=NAO_VIU|B=NAO_VIU
NOVO|CALENDARIO-HOJE_38fe9889_par1|A|app/colaboradores/services/calendario.py:351|Sem vínculo ativo, vínculo histórico válido e célula de trabalho não impedem descartar sem_celula e publicar sem_dados.
NOVO|CALENDARIO-HOJE_38fe9889_par1|A|app/colaboradores/legenda_calendario.py:18|Legenda do copiloto ainda define previsto exclusivamente como dia futuro; teste verifica nomes, não explicações. [nao verificado]
NOVO|CALENDARIO-HOJE_38fe9889_par1|A|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:19|Novos testes não exercitam a alimentação turnos= com lista não vazia. [nao verificado]
NOVO|CALENDARIO-HOJE_38fe9889_par1|B|app/colaboradores/services/calendario.py:351|Sem vínculo ativo, calendário perde a retenção de dia coberto por vínculo histórico e publica sem_dados.
NOVO|CALENDARIO-HOJE_38fe9889_par1|B|app/colaboradores/services/calendario.py:64|Calendário escreve localmente a frase do dia cego, em vez de consumir rótulo produzido pela autoridade da folha.
NOVO|CALENDARIO-HOJE_38fe9889_par1|B|app/colaboradores/legenda_calendario.py:18|Explicação de previsto entregue ao copiloto ficou restrita ao futuro e seu teste não verifica a descrição. [nao verificado]
NOVO|CALENDARIO-HOJE_38fe9889_par1|B|app/colaboradores/services/calendario.py:727|Filtro de retenções consulta lista de candidatos por registro e pode acrescentar custo quadrático de CPU.
NOVO|CALENDARIO-HOJE_38fe9889_par1|B|app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:20|Fixture sem batidas não cobre a nova entrada turnos= com conteúdo.
NOVO|CALENDARIO-HOJE_38fe9889_par2|A|app/colaboradores/services/calendario.py:822|Veredito concorde sem batida desliga a proteção temporal e pode produzir falta enquanto a juíza responde relogio.
NOVO|CALENDARIO-HOJ

[truncado em 20000 de 20889 caracteres]

_705 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
