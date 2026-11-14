# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 04:54** · publicado em **14/11/2026 04:54** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 04:54 · sessao cc383e046e61 · HJ-091 · sonda · origem=Code

| Campo | Valor | Referência |
|---|---|---|
| universo_admin_encerrado_colab_com_ferias | 845 | [nao verificado] |
| com_dia_do_fato | 419 | [nao verificado] |
| dia_coberto_por_ferias | 27 | [nao verificado] |
| veto_admin_sem_fato_novo_ativo_hoje | 16 | [nao verificado] |
| erros | 0 | [nao verificado] |
| pks_veto | [3331, 3444, 3934, 4533, 4879, 5968, 6184, 7081, 7084, 7805, 7809, 8640, 11141, 11776, 12526, 12801] | [nao verificado] |

A saída fornecida não contém erro/traceback. Ocorrências de `ERRO`: **0**. [nao verificado]

## 14/11 04:13 · sessao 3310ddf303f2 · E3-FORMA-SQL · leitura · origem=Code

O código atual exclui a resposta carimbada sem texto; os testes da E3 exigem essa exclusão, enquanto testes da O168 exigem inclusão. Essa incompatibilidade é explícita nas asserções. (`app/chamados/models.py:1908`; `app/chamados/tests/test_e3_10_cura_no_escritor.py:131`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:285`.)

Não executei testes nem alterei arquivos. Os vermelhos abaixo são deduções por leitura, sem certificação da suíte. [nao verificado] [nao verificado]

**1. Textos literais e casos nomeados**

**R-a:** não achei `E3-10`, `respondida_sem_texto` ou `sem_veredito_entre` nos três arquivos pedidos: `app/docs/CORTES.md`, `app/docs/LEIS.md` e `app/docs/DOSSIES.md`. A íntegra do aval E3-10 item 2 não foi localizada nesses arquivos. [nao verificado]

Há estes registros fora deles:

- Registro literal: “cura na PORTA com RED+mutante; respostas_sem_veredito pergunta ao predicado; passivo so se o app dos 17 nao muda.” Nomeia o universo `respondida_sem_texto`, sem escrever literalmente `texto=''`. (`app/docs/PROMPTS.md:2171`.)
- O BACKLOG repete: “cura na porta com RED+mutante; respostas_sem_veredito pergunta ao predicado”. (`app/docs/BACKLOG.md:1944`.)
- O comentário que atribui a mudança ao item 2 diz: “respondida SEM texto (HX-PAR-RESPOSTA) nao e resposta a julgar: a bola segue com o colaborador (C6/fio), nao com o admin.” (`app/chamados/models.py:1900–1901`.)
- O teste da E3 nomeia expressamente `''` e espaços e exige que ambos saiam da lista. Isso é evidência da implementação/teste da regra, não a íntegra do aval. (`app/chamados/tests/test_e3_10_cura_no_escritor.py:125–133`.)

**R-b:** o registro literal encontrado diz:

> “`ponto/supra_juiz.py::pode_executar_csf` passa a PERGUNTAR a ele em vez de ler `respondida_em` cru: pai/destino seguro so com resposta SEM veredito; validada/materializada fecha normal pela Defesa 1.”

O juiz nomeado nessa linha é `DisputaSupervisao.respostas_sem_veredito`. A linha também proíbe “(A) segurar todo pai respondido” e “(C) fechar o pai nas 3 views”. Não menciona `texto=''`, `sem_veredito_entre` ou retratação. (`app/docs/CORTES.md:449`.)

A retratação aparece em outros registros:

- “pode_executar_csf (…) e reconciliador.py:1465 perguntam a DisputaSupervisao.respostas_sem_veredito”. (`app/docs/PROMPTS.md:2208`.)
- O corpo compartilhado atribui ao aval “um predicado so”, para CSF e retratação sobre universos diferentes. (`app/chamados/models.py:1894–1898`.)
- `irmas_respondidas_sem_consumo` atribui ao aval “mesma pergunta na retratacao por premissa morta” e identifica `sem_veredito_entre` como corpo de `respostas_sem_veredito`. (`app/chamados/juizes.py:2063–2066`.)

Assim, o registro de R-b nomeia **resposta esperando veredito**, distinguindo-a de validada/materializada; a inclusão específica de resposta vazia aparece no teste O168, não no texto de `CORTES.md:449`. (`app/docs/CORTES.md:449`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:282–285`.)

**2. Corpo atual e todos os chamadores encontrados**

O campo lido pelo juiz é `resposta_colab`. No escritor automático, o argumento `texto` é colocado nesse campo e `instante` em `respondida_em`. (`app/chamados/services/disputa_emissao.py:1659–1660`.)

O corpo atual exige simultaneamente:

- `validada_em is None` e `materializada_em is None`;
- carimbo preenchido **ou** texto que sobreviva a `.strip()`;
- **não** estar em `respondida_sem_texto`;
- dia não trancado, salvo `ignorar_tranca=True`. (`app/chamados/models.py:1904–1909`.)

`respondida_sem_texto` significa carimbo preenchido com texto vazio após `(t or '').strip()`. Portanto, `resposta_colab=''` fica fora da lista com ou sem carimbo; espaços também ficam fora. Texto não vazio sem carimbo continua podendo entrar. (`app/chamados/juizes.py:1520–1528`; `app/chamados/models.py:1904–1909`.)

As chamadas diretas a `sem_veredito_entre` encontradas nos arquivos Python são:

| Chamador | Universo / configuração | Fonte |
|---|---|---|
| `DisputaSupervisao.respostas_sem_veredito` | Todas as perguntas da disputa, ou `perguntas=` fornecidas; repassa a opção de tranca | `app/chamados/models.py:1889–1890` |
| `irmas_respondidas_sem_consumo` | Destino e crédito ao pai; `ignorar_tranca=True` | `app/chamados/juizes.py:2074–2076` |
| `perguntas_que_seguram_csf` | Todas as disputas do pai; `ignorar_tranca=True` | `app/ponto/supra_juiz.py:558`; `app/ponto/supra_juiz.py:573` |
| `perguntas_que_seguram_csf` | Perguntas que resolvem o destino; `ignorar_tranca=True` | `app/ponto/supra_juiz.py:574` |
| Fixture de teste `_D.respostas_sem_veredito` | Lista em memória; repassa a opção de tranca | `app/ponto/tests/test_supra_juiz_executor.py:45–46` |

Os chamadores diretos de produção de **`respostas_sem_veredito`**, que chegam ao corpo pela delegação acima, são:

| Chamador | Fonte |
|---|---|
| `DisputaSupervisao.pode_fechar` | `app/chamados/models.py:1917` |
| `DisputaSupervisao.fechar` | `app/chamados/models.py:1950` |
| `encerrar_orfas_pela_lampada` | `app/chamados/reconciliador.py:1164` |
| `competencia_trancada.medir` | `app/chamados/services/competencia_trancada.py:126` |
| `fila_por_causa._gaveta_de` | `app/chamados/services/fila_por_causa.py:226` |
| `ficha_do_colab` | `app/colaboradores/services/ficha.py:169` |
| `executar_desligamento` | `app/colaboradores/services/desligamento.py:165` |
| `validar_classe_a.Command.handle` | `app/chamados/management/commands/validar_classe_a.py:54` |

As chamadas diretas de teste a **`respostas_sem_veredito`** são: [nao verificado]

| Teste | Chamada | [nao verificado]
|---|---|
| `RespostasSemVereditoPerguntaAoPredicadoTest.test_RED_respondida_sem_texto_nao_e_resposta_sem_veredito` | `app/chamados/tests/test_e3_10_cura_no_escritor.py:130` |
| `RespostasSemVereditoPerguntaAoPredicadoTest.test_pode_fechar_segue_a_lista_e_o_c6` | `app/chamados/tests/test_e3_10_cura_no_escritor.py:140` |
| `C7_08_CompetenciaTrancadaTest.test_RED_resposta_de_dia_trancado_nao_segura_a_disputa` | `app/chamados/tests/test_c7_red_minimos.py:381` |
| `PaiComConversaNoGateTest.test_RED_retratacao_pergunta_o_MESMO_juiz_do_veredito` | `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:285` |
| `PaiComConversaNoGateTest.test_RED_dia_trancado_CSF_e_retratacao_dao_a_MESMA_resposta` | `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:303` |

Há também instalação dinâmica dos métodos reais no teste de memória, inspeção de fonte no contrato e substituição por mock no teste de fechamento; não são chamadas diretas adicionais escritas com esses nomes. (`app/chamados/tests/test_e4b_c4v14_memoria.py:47–50`; `app/chamados/tests/test_contract_dedup_status.py:136–143`; `app/chamados/tests/test_termo_fechar.py:28`.)

**3. Opções, bloqueios, testes e tela** [nao verificado]

**A — Manter o corpo atual.** A vazia não segura pela lista de veredito, pelo CSF ou por `irmas_respondidas_sem_consumo`. Isso **não garante fechamento**: `pode_fechar` exige também ausência de fio vivo, e `fechar` verifica perguntas vivas separadamente. (`app/chamados/models.py:1908`; `app/ponto/supra_juiz.py:573–588`; `app/chamados/juizes.py:2076`; `app/chamados/models.py:1917`; `app/chamados/models.py:1959–1966`.)

Vermelhos dedutíveis com esse corpo:

| Teste | Motivo | [nao verificado]
|---|---|
| `PaiComConversaNoGateTest.test_RED_retratacao_pergunta_o_MESMO_juiz_do_veredito` | Exige que a pergunta carimbada com `''` apareça na retratação **e na lista da disputa**. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:282–285`.) |
| `ExecutorCsfTest.test_resposta_na_disputa_barra` | `_P('x')` tem carimbo, mas texto padrão `''`; exige CSF bloqueado. (`app/ponto/tests/test_supra_juiz_executor.py:24–29`; `app/ponto/tests/test_supra_juiz_executor.py:71–72`.) |
| `ExecutorCsfTest.test_resposta_direta_barra` | Mesmo caso vazio, pelo destino. (`app/ponto/tests/test_supra_juiz_executor.py:24–29`; `app/ponto/tests/test_supra_juiz_executor.py:74–75`.) |

**B — Restaurar exatamente o corpo do main em `96aa51407`.** Tentei o `git show` solicitado; a cópia não contém repositório Git. **Não sei qual é o corpo histórico exato nem seu diff integral.** [nao verificado]

O comentário do selo registra o merge desse main e a transferência dos leitores ao corpo compartilhado; isso não fornece o conteúdo histórico completo. (`app/chamados/tests/test_e3_catraca_respondedores.py:54–56`.)

**B′ — Contrafactual verificável: retirar somente `and not respondida_sem_texto(q)`.** Não afirmo que seja exatamente o main histórico. Essa retirada preservaria o “carimbo OU texto” descrito no teste O168 e acrescentaria à lista as carimbadas vazias ou só com espaços, respeitando ainda validação, materialização e tranca. (`app/chamados/models.py:1904–1909`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:277–285`.)

Comparando B′ com A:

| Leitor | O que passa a segurar ou incluir a mais |
|---|---|
| `fechar` / `pode_fechar` | Vazia passa a provocar recusa por resposta sem veredito; pode impedir fechamento mesmo sem fio vivo. (`app/chamados/models.py:1917`; `app/chamados/models.py:1950–1958`.) |
| Desligamento | Mais disputas são puladas pelo `continue`. (`app/colaboradores/services/desligamento.py:165–167`.) |
| Encerramento de órfãs | Mais disputas entram em `disputas_barradas`. (`app/chamados/reconciliador.py:1164–1173`.) |
| CSF e gate do pai | Mais perguntas bloqueiam CSF; o gate do pai pode registrar recusa e nota `retido_pela_conversa`. (`app/ponto/supra_juiz.py:573–588`; `app/chamados/models.py:719–740`.) |
| Retratação / materialização | Mais retenções `irma_orfa` e de destino/clones. (`app/chamados/reconciliador.py:448–450`; `app/chamados/services/materializacao.py:768–777`.) |
| HX e auditoria | A vazia pode sustentar reabertura do pai; na I12, pode passar de violação para piso BUG31, satisfeitas as demais condições. (`app/chamados/services/disputa_emissao.py:2010–2020`; `app/chamados/management/commands/auditar_invariantes_chamados.py:983–989`.) |
| Fila por causa | Mais chamados podem entrar em `respondido_a_decidir`. (`app/chamados/services/fila_por_causa.py:225–227`.) |
| Competência trancada | Mais IDs podem entrar em `decidir_pelo_dp`, inclusive de dia trancado. (`app/chamados/services/competencia_trancada.py:126–139`.) |
| Ficha | Entre as perguntas selecionadas, vazia passa de `sem_resposta` para `sem_veredito`. (`app/colaboradores/services/ficha.py:166–171`.) |
| `validar_classe_a` | Amplia o universo de pendentes a classificar; só a classificação A entra no lote aplicável. (`app/chamados/management/commands/validar_classe_a.py:54–67`.) |

Nesse contrafactual, ficam vermelhos os testes E3 `RespostasSemVereditoPerguntaAoPredicadoTest.test_RED_respondida_sem_texto_nao_e_resposta_sem_veredito` e `RespostasSemVereditoPerguntaAoPredicadoTest.test_pode_fechar_segue_a_lista_e_o_c6`: ambos exigem exclusão da vazia. (`app/chamados/tests/test_e3_10_cura_no_escritor.py:125–141`.)

**C — Dois predicados com nomes distintos:** por exemplo, conservar `sem_veredito_entre` excluindo vazias para a lista da disputa, e criar outro predicado, aqui denominado apenas para comparação `registros_sem_veredito_entre`, incluindo carimbo OU texto para CSF/retratação. O segundo nome é uma possibilidade de desenho, não código existente. [nao verificado]

Nessa distribuição, os leitores da lista manteriam os efeitos de A; CSF, gate do pai e leitores de `irmas_respondidas_sem_consumo` teriam os efeitos de B′. Essa divisão acompanha as chamadas atuais dos dois caminhos. (`app/chamados/models.py:1889`; `app/ponto/supra_juiz.py:573–574`; `app/chamados/juizes.py:2076`.)

A separação **não resolve todos os testes atuais**: o teste O168 exige também que `d.respostas_sem_veredito(...)` inclua a vazia, portanto continuaria vermelho na asserção da linha 285. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:283–285`.)

Além disso, migrar o CSF exclusivamente para um nome novo faria `JuizDoVereditoPorAstTest.test_o_juiz_nao_le_respondida_em_cru` perder a chamada reconhecida: a catraca aceita somente `respostas_sem_veredito` e `sem_veredito_entre`. Renomear também a delegação da lista afetaria `ContratoDedupDistingueStatusTest.test_porta_humana_de_disputa_presta_contas`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:313`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:367–399`; `app/chamados/tests/test_contract_dedup_status.py:139–143`.)

Os efeitos de tela dedutíveis para essas distribuições são:

| Superfície | Diferença condicionada |
|---|---|
| Pendências de disputas | Com a vazia ainda viva pelo C6, A e B′ continuam podendo mostrar **“Cobrar o colaborador”**. Sem fio vivo, sem outra pendência e fora da tranca, A permite **“Falta materializar”**; B′ leva a **“Voce valida agora”**. C seguiria A nessa tela. (`app/chamados/relatorios_disputa.py:24–28`; `app/chamados/services/fio.py:174–187`; `app/colaboradores/views.py:3003–3005`.) |
| Fio do pai | B′ e C podem acrescentar a nota **“Chamado mantido aberto…”** quando a vazia é o motivo da retenção. (`app/chamados/models.py:719–739`.) |
| Ficha com pendências | B′ muda contagens e pode trocar a ação de responder pela de validar; C seguiria A. (`app/colaboradores/services/ficha.py:169–171`; `app/colaboradores/services/ficha.py:190–193`; `app/colaboradores/services/ficha.py:245–246`.) |
| Pauta DP | B′ pode ampliar IDs/contagem de pauta nova; uma pauta viva já existente é retornada sem atualização. C seguiria A nesse leitor. (`app/chamados/services/competencia_trancada.py:232–249`.) |
| Cartão “Resposta enviada” | Alterar somente esses predicados não muda esse cartão: ele usa `espera_o_texto_do_colaborador`, que considera carimbo sem texto como espera de texto. (`app/chamados/services/fio.py:177–187`; `app/chamados/services/fio.py:357`; `app/templates/chamados/partials/_card_atendimento.html:100–109`.) |
| Pendentes/histórico do app | Essa separação usa diretamente carimbo e disputa aberta, sobre o universo do app; não chama o juiz do veredito. Não se pode equiparar exclusão da lista a reaparecimento automático em pendentes. (`app/chamados/services/pergunta_no_app.py:64–78`.) |

Não sei quais dessas mudanças ocorreriam no caso real, nem quantos chamados seriam afetados: o dado raspado fornecido contém a tabela de autoridades, sem a linha da pergunta e seus demais estados. [nao verificado]

**4. Regras gerais encontradas**

| Regra | O que permite ler neste tema |
|---|---|
| **L-039:** “FALTA TEM UM SIGNIFICADO = decidida (ausencia lancada cujo tipo desconta)” | Define falta; não define se resposta vazia é fala a julgar. (`app/docs/LEIS.md:87`.) |
| **L-088:** furo é “PERGUNTA em aberto e nao decisao” | Também não resolve a classificação do par carimbo/texto. (`app/docs/LEIS.md:120`.) |
| **C7:** fio vivo vazio **e** nenhuma resposta sem veredito | Impede deduzir fechamento apenas porque a vazia saiu da lista; não define, por si, se vazio integra essa lista. (`app/docs/CORTES.md:272`; `app/chamados/models.py:1917`.) |
| **Resposta sem veredito segura** | A documentação de `fechar` declara recusa em todas as portas; materializada não bloqueia. O ponto ainda dependente de definição é se o vazio constitui “resposta”. (`app/chamados/models.py:1874–1884`; `app/chamados/models.py:1922–1930`.) |
| **L-002 / um juiz por pergunta** | Sustenta os leitores consultarem a mesma autoridade; não escolhe sozinho a resposta dessa autoridade para `''`. (`app/docs/LEIS.md:60`; `CLAUDE.md:178`.) |
| **L-110** | Diz que o código existente não é gabarito e prevê pergunta de lei onde a regra não responde. Não autoriza eleger uma opção pelo teste ou código atual. (`app/docs/LEIS.md:136`.) |

Não achei uma regra geral adicional que determine expressamente **incluir ou excluir `texto=''`** nesse juiz, além dos registros específicos apresentados. Procurei em `LEIS.md`, `CORTES.md`, `DOSSIES.md`, `CLAUDE.md`, `JUIZES.md` e no registro de autoridades. [nao verificado]

O registro vivo pergunta “quais respostas do colaborador na disputa esperam veredito (nem validadas nem materializadas)?”, sem acrescentar uma definição de vazio. O anexo `JUIZES.md` declara ser lookup, remetendo a lei ao contrato do `CLAUDE.md`. (`app/core/juizes.py:302`; `app/docs/JUIZES.md:7–14`.)

**5. Compatibilidade por leitura**

**Sim, os dois comandos podem ser compatíveis:** R-a manda a lista consultar `respondida_sem_texto`; R-b manda CSF/retratação consultar o juiz dessa lista. O corpo atual faz justamente essa composição: lista → corpo compartilhado → predicado de texto; CSF e retratação chegam ao mesmo corpo. (`app/docs/PROMPTS.md:2171`; `app/chamados/models.py:1889–1909`; `app/ponto/supra_juiz.py:573–574`; `app/chamados/juizes.py:2076`.)

Exigir **que todos perguntem ao mesmo juiz** não determina, sozinho, que esse juiz inclua a vazia. O registro O168 em CORTES não escreve essa inclusão; o teste O168 a exige expressamente. (`app/docs/CORTES.md:449`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:282–285`.)

A incompatibilidade demonstrável está nas expectativas sobre a própria lista: E3 exige `NotIn(vazia, lista)`; O168 exige `[p.pk]` para a lista com a pergunta carimbada vazia. Separar apenas os leitores não elimina essa oposição. (`app/chamados/tests/test_e3_10_cura_no_escritor.py:127–132`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:282–285`.)

Se a íntegra do aval O168 pretendeu também redefinir o resultado para vazio, **não sei**: essa determinação não está no registro de CORTES citado, e a íntegra do aval não foi fornecida. (`app/docs/CORTES.md:449`; [nao verificado].)

## 14/11 04:10 · sessao ef94f3f336cf · - · - · origem=Ronald (tmux)

**Chamadores reais diretos, exaustivos por leitura AST de `app/**/*.py`:**

| Alvo | Chamador |
|---|---|
| `sem_veredito_entre` | `DisputaSupervisao.respostas_sem_veredito`: `app/chamados/models.py:1889` |
| `sem_veredito_entre` | `irmas_respondidas_sem_consumo`: `app/chamados/juizes.py:2076` |
| `sem_veredito_entre` | `perguntas_que_seguram_csf`, universo pai: `app/ponto/supra_juiz.py:573`; universo destino: `app/ponto/supra_juiz.py:574` |
| `sem_veredito_entre` — teste | Fixture `_D.respostas_sem_veredito` delega ao corpo real: `app/ponto/tests/test_supra_juiz_executor.py:46` |
| `respostas_sem_veredito` | `pode_fechar`: `app/chamados/models.py:1917`; `fechar`: `app/chamados/models.py:1950` |
| `respostas_sem_veredito` | `encerrar_orfas_pela_lampada`: `app/chamados/reconciliador.py:1164` |
| `respostas_sem_veredito` | `competencia_trancada.medir`: `app/chamados/services/competencia_trancada.py:126` |
| `respostas_sem_veredito` | `fila_por_causa._gaveta_de`: `app/chamados/services/fila_por_causa.py:226` |
| `respostas_sem_veredito` | `ficha_do_colab`: `app/colaboradores/services/ficha.py:169` |
| `respostas_sem_veredito` | `executar_desligamento`: `app/colaboradores/services/desligamento.py:165` |
| `respostas_sem_veredito` | Comando `validar_classe_a.Command.handle`: `app/chamados/management/commands/validar_classe_a.py:54` |

**Testes com chamadas reais de `respostas_sem_veredito`, exaustivos:** [nao verificado]

- `RespostasSemVereditoPerguntaAoPredicadoTest.test_RED_respondida_sem_texto_nao_e_resposta_sem_veredito`, chamada `app/chamados/tests/test_e3_10_cura_no_escritor.py:130`; exige vazia e espaços fora da lista, resposta textual dentro: `app/chamados/tests/test_e3_10_cura_no_escritor.py:127`.
- `RespostasSemVereditoPerguntaAoPredicadoTest.test_pode_fechar_segue_a_lista_e_o_c6`, chamada/exigência lista vazia `app/chamados/tests/test_e3_10_cura_no_escritor.py:140`; igualdade com `not fio_vivo`: `app/chamados/tests/test_e3_10_cura_no_escritor.py:141`.
- `C7_08_CompetenciaTrancadaTest.test_RED_resposta_de_dia_trancado_nao_segura_a_disputa`, chamada `app/chamados/tests/test_c7_red_minimos.py:381`.
- `PaiComConversaNoGateTest.test_RED_retratacao_pergunta_o_MESMO_juiz_do_veredito`, chamada `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:285`; a expectativa anterior, que já falha pelo corpo atual, exige carimbo com texto `''` retido por `irmas_respondidas_sem_consumo`: `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:282`.
- `PaiComConversaNoGateTest.test_RED_dia_trancado_CSF_e_retratacao_dao_a_MESMA_resposta`, chamada `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:303`; esse caso usa resposta textual da fixture e testa tranca, não vazio: `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:301`.

**Referências de teste que não são chamadas diretas:** [nao verificado]

- `test_contract_dedup_status.py` inspeciona fonte e exige `respostas_sem_veredito → self.sem_veredito_entre` literal; uma separação que retire essa delegação pode falhar nesse teste: `app/chamados/tests/test_contract_dedup_status.py:133`, `app/chamados/tests/test_contract_dedup_status.py:139`, `app/chamados/tests/test_contract_dedup_status.py:141`.
- `test_e4b_c4v14_memoria.py` instala dinamicamente os métodos reais e o corpo na fixture, portanto acaba executando-os indiretamente: `app/chamados/tests/test_e4b_c4v14_memoria.py:47`, `app/chamados/tests/test_e4b_c4v14_memoria.py:50`.
- `test_termo_fechar.py` substitui o método por mock: `app/chamados/tests/test_termo_fechar.py:28`.
- A catraca O168 aceita só os nomes `respostas_sem_veredito` e `sem_veredito_entre`: `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:313`; exige chamada alcançável do CSF: `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:394`. Se CSF for migrado exclusivamente a novo nome, `test_o_juiz_nao_le_respondida_em_cru` falha na exigência de chamada, salvo alteração da catraca: `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:399`.
- As chamadas presentes em strings de mutantes AST não são chamadas do corpo real: `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:417`, `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:429`, `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:434`.
- Arquivos `.txt` sob `e4b_c6_bases` são cópias de referência, não arquivos Python carregados por essas chamadas. [não verificado quanto a carregadores externos; inventário AST acima cobre `.py`]

**Efeito de incluir vazias novamente no corpo:**

- `fechar` ganha recusa de resposta sem veredito antes de verificar vivas; `pode_fechar` passa a negar para a vazia mesmo quando C6 não a mantém viva. O corpo atual não dispensa a guarda C6: `app/chamados/models.py:1917`, `app/chamados/models.py:1950`, `app/chamados/models.py:1960`.
- No desligamento, mais disputas são puladas pelo `continue`; no encerramento de órfãs pela lâmpada, mais disputas entram em `disputas_barradas`. Fechamento efetivo depende das demais guardas: `app/colaboradores/services/desligamento.py:165`, `app/chamados/reconciliador.py:1164`.
- CSF passa a negar por pai e destino; gate do pai passa a recusar e emitir nota “Chamado mantido aberto…” para esse motivo: `app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:588`, `app/chamados/models.py:719`, `app/chamados/models.py:735`.
- Retratação ganha retenção `irma_orfa`; materialização ganha retenção de destino/clones; fluxo HX passa a poder reabrir pai por essa vazia: `app/chamados/reconciliador.py:448`, `app/chamados/services/materializacao.py:768`, `app/chamados/services/disputa_emissao.py:2016`.
- Auditoria I12 muda vazia de violação para piso BUG31 quando a outra condição de TM fechado se cumpre: `app/chamados/management/commands/auditar_invariantes_chamados.py:983`.

**Tela, ficha e pauta:**

- Relatório de pendências consulta primeiro `pode_fechar`, depois `aguarda_o_colaborador`: `app/chamados/relatorios_disputa.py:24`. Para vazia viva, ambos corpos continuam em `AGUARDA_COLAB`; para vazia morta pelo C6, o atual pode retornar `PRONTA_FECHAR`, enquanto o corpo que inclui vazias pode retornar `AGUARDA_SUPERVISAO`, ressalvados tranca e outras perguntas: `app/chamados/relatorios_disputa.py:24`, `app/chamados/services/fio.py:174`, `app/chamados/services/fio.py:187`.
- Os títulos mostrados são “Falta materializar”, “Voce valida agora” e “Cobrar o colaborador”; o template mostra a quantidade por seção: `app/colaboradores/views.py:3003`, `app/templates/colaboradores/partials/pendencias_disputa.html:4`.
- Cartão do colaborador continua sem “Resposta enviada” para vazio em ambas opções: `resposta_enviada` usa outro predicado, `espera_o_texto_do_colaborador`, que inclui carimbo sem texto: `app/chamados/services/fio.py:357`, `app/chamados/services/fio.py:187`, `app/templates/chamados/partials/_card_atendimento.html:100`.
- Ficha: só considera perguntas presentes em `perguntas_do_app`; incluir vazia move a pergunta de `sem_resposta` para `sem_veredito`, muda contagens e ações recomendadas: `app/colaboradores/services/ficha.py:166`, `app/colaboradores/services/ficha.py:169`, `app/colaboradores/services/ficha.py:171`, `app/colaboradores/services/ficha.py:190`, `app/colaboradores/services/ficha.py:245`.
- Cabeçalho das telas não muda por essas pendências, pois a ficha do cabeçalho chama `com_pendencias=False`; API do copiloto recebe a ficha completa: `app/colaboradores/services/ficha.py:269`, `app/colaboradores/services/ficha.py:275`, `app/api/views_mensageria.py:1133`.
- Fila por causa ganha chamados na gaveta `respondido_a_decidir` caso disputa aberta; rótulo/dono/ação são “Respondido, esperando decisao”, DP, decidir: `app/chamados/services/fila_por_causa.py:225`, `app/chamados/services/fila_por_causa.py:95`. `quadro` recalcula contagens por gaveta e a API expõe esse quadro: `app/chamados/services/fila_por_causa.py:283`, `app/api/views_mensageria.py:1595`.
- Pauta DP: `medir` ignora tranca, inclui IDs não resgatados em `decidir_pelo_dp`; a aplicação cria pauta se a lista não estiver vazia. Pauta viva preexistente é retornada sem atualização: `app/chamados/services/competencia_trancada.py:126`, `app/chamados/services/competencia_trancada.py:137`, `app/chamados/services/competencia_trancada.py:232`, `app/chamados/services/competencia_trancada.py:243`. Assim incluir vazias pode ampliar IDs/contagem de pauta nova; não se pode prometer mudança da pauta já existente: `app/chamados/services/competencia_trancada.py:246`.
- `validar_classe_a` passa a contar vazias entre pendentes e “sem hora”, mas não a validá-las: coleta pela lista, classifica e só inclui classe A no lote: `app/chamados/management/commands/validar_classe_a.py:54`, `app/chamados/management/commands/validar_classe_a.py:63`, `app/chamados/management/commands/validar_classe_a.py:99`. Vazio de escolha retorna `None`; hora/data_hora vazia não converte em minutos; demais tipos não classificam hora: `app/chamados/juizes.py:2000`, `app/chamados/juizes.py:1893`, `app/chamados/juizes.py:1897`, `app/chamados/juizes.py:1907`, `app/chamados/juizes.py:1914`.

Testes não foram executados; vermelhos citados são dedução das asserções e do corpo lido. [não verificado por execução] [nao verificado]

## 14/11 04:08 · sessao 107d2772def7 · - · - · origem=Ronald (tmux)

Por leitura estática, há testes com expectativas incompatíveis para a mesma lista e o mesmo caso carimbado com texto `''`: E3 exige exclusão; O168 exige inclusão. Não executei a suíte. (`app/chamados/tests/test_e3_10_cura_no_escritor.py:125–133`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:275–285`.)

| Opção | Vermelhos dedutíveis pela leitura |
|---|---|
| Corpo atual, com `and not respondida_sem_texto(q)` | `PaiComConversaNoGateTest.test_RED_retratacao_pergunta_o_MESMO_juiz_do_veredito`: cria `respondida_em` existente + `resposta_colab=''` e exige `[p.pk]` na retratação e na lista. (`app/chamados/models.py:1904–1909`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:275–285`.) |
| Corpo atual | `ExecutorCsfTest.test_resposta_na_disputa_barra` e `ExecutorCsfTest.test_resposta_direta_barra`: `_P('x')` significa carimbo `'x'` com texto padrão `''`, mas os testes exigem bloqueio. (`app/ponto/tests/test_supra_juiz_executor.py:24–29`; `app/ponto/tests/test_supra_juiz_executor.py:71–75`.) |
| Retirar apenas `and not respondida_sem_texto(q)` | `RespostasSemVereditoPerguntaAoPredicadoTest.test_RED_respondida_sem_texto_nao_e_resposta_sem_veredito`: exige que `''` e espaços carimbados fiquem fora da lista. (`app/chamados/tests/test_e3_10_cura_no_escritor.py:125–133`.) |
| Retirar apenas essa condição | `RespostasSemVereditoPerguntaAoPredicadoTest.test_pode_fechar_segue_a_lista_e_o_c6`: exige lista vazia para carimbo + `''`. (`app/chamados/tests/test_e3_10_cura_no_escritor.py:135–141`.) |
| Separar: lista E3 exclui; CSF/retratação incluem | Os dois testes do executor deixam de conflitar com o comportamento; porém `test_RED_retratacao_pergunta_o_MESMO_juiz_do_veredito` continua exigindo também que **a lista E3** inclua a pergunta, na segunda asserção. (`app/ponto/tests/test_supra_juiz_executor.py:71–75`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:283–285`.) |

Para uma separação com nomes novos, há ainda testes estruturais dependentes da implementação escolhida: [nao verificado]

- `ContratoDedupDistingueStatusTest.test_porta_humana_de_disputa_presta_contas` exige literalmente `self.sem_veredito_entre(` dentro de `respostas_sem_veredito`; renomear o corpo chamado pode torná-lo vermelho. (`app/chamados/tests/test_contract_dedup_status.py:133–144`.)
- `JuizDoVereditoPorAstTest.test_o_juiz_nao_le_respondida_em_cru` reconhece como juiz apenas chamadas chamadas `respostas_sem_veredito` ou `sem_veredito_entre`, inclusive por apelido importado. Um novo nome exclusivo para CSF pode torná-lo vermelho. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:313`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:367–399`.)

O teste `test_RED_dia_trancado_CSF_e_retratacao_dao_a_MESMA_resposta` cobre outra dimensão: com resposta textual, CSF e retratação ignoram tranca e seguram, enquanto a lista padrão respeita a tranca e esvazia. Esse caso não distingue o filtro de texto vazio. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:291–310`; fixture com texto em `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:55–62`.)

Os testes de custo usam texto `'07:00'`; não fornecem uma expectativa para texto `''`. (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:43–49`.)

Tentei `git show 96aa51407:app/chamados/models.py`; falhou porque a cópia não é repositório Git. Portanto não verifiquei o corpo histórico, e “retirar apenas a condição” acima é um contrafactual explícito, sem afirmar que reproduz exatamente aquele commit. [não verificado]

## 14/11 04:08 · sessao f474698533df · - · - · origem=Ronald (tmux)

**Regra R-a: literal completo não localizado nos três arquivos pedidos.** Fiz grep por `E3-10`, `CURA.NO.ESCRITOR`, `respondida_sem_texto` e `sem_veredito_entre` em `app/docs/CORTES.md`, `app/docs/LEIS.md` e `app/docs/DOSSIES.md`: não achei nenhuma ocorrência desses identificadores nesses arquivos. A íntegra do aval de 12/11 item 2 fica **[não verificado]**; há ponteiros para `logs/AVAIS-1212/e3_10_cura_no_escritor.txt`, fora do material fornecido. Os registros disponíveis são:

- `app/docs/PROMPTS.md:2171`: literal da **linha de registro**, não da íntegra do aval: “E3-10-CURA-NO-ESCRITOR (aval Ronald): censo Codex do escritor das 143 vivas em respondida_sem_texto pela trilha; cura na PORTA com RED+mutante; respostas_sem_veredito pergunta ao predicado; passivo so se o app dos 17 nao muda.”
- `app/docs/BACKLOG.md:1944`: item (2) escrito “cura na porta com RED+mutante; respostas_sem_veredito pergunta ao predicado”; nomeia 143 vivas em `respondida_sem_texto`, sem explicitar `texto=''`.
- `app/chamados/models.py:1900-1901`: comentário do corpo atual atribui ao item 2: “‘o colaborador falou’ pergunta ao predicado UNICO -- respondida SEM texto (HX-PAR-RESPOSTA) nao e resposta a julgar: a bola segue com o colaborador (C6/fio), nao com o admin.”
- Assim, R-a nomeia o caso “carimbo de resposta sem texto”; o significado computável de vazio depende do predicado, não de um literal `texto=''` presente no aval disponível. `app/chamados/models.py:1899-1908`.

**Regra R-b: encontrada em CORTES, sem menção literal a vazio.**

- `app/docs/CORTES.md:449`: “corte Ronald: juiz DisputaSupervisao.respostas_sem_veredito nasce” e “`ponto/supra_juiz.py::pode_executar_csf` passa a PERGUNTAR a ele em vez de ler `respondida_em` cru: pai/destino seguro so com resposta SEM veredito; validada/materializada fecha normal pela Defesa 1.” Proíbe “(A) segurar todo pai respondido e (C) fechar o pai nas 3 views”.
- A mesma linha **não** contém `sem_veredito_entre`, retratação, `texto=''` nem a expressão “perguntam a ESSE juiz”. `app/docs/CORTES.md:449`.
- A migração do reconciliador está registrada em `app/docs/PROMPTS.md:2208`: “pode_executar_csf (...) e reconciliador.py:1465 perguntam a DisputaSupervisao.respostas_sem_veredito”.
- A interpretação da retratação está documentada no próprio código em `app/chamados/juizes.py:2063-2066`: “O PREDICADO E O DO JUIZ DO VEREDITO” / “mesma pergunta na retratacao por premissa morta”; usa `sem_veredito_entre`, corpo de `respostas_sem_veredito`, com `ignorar_tranca=True`. É comentário que atribui frase ao aval; íntegra do aval **[não verificado]**.
- O comentário de `sem_veredito_entre` atribui ao aval “um predicado so”, com CSF e retratação lendo a mesma pergunta sobre universos distintos. `app/chamados/models.py:1894-1898`.
- `app/docs/CORTES.md:448` marca expressamente como superada a afirmação anterior “MESMO predicado”: após (B) CSF passa a perguntar ao juiz em vez do carimbo cru; não declara que (B) define o comportamento de texto vazio.

**Regras gerais encontradas, sem eleger resposta para vazio.**

- L-039: “FALTA TEM UM SIGNIFICADO = decidida (ausencia lancada cujo tipo desconta)”. Trata classificação de falta, não o par carimbo/texto de resposta. `app/docs/LEIS.md:87`.
- Corte FALTA-UM-SIGNIFICADO: furo sem decisão aparece como “dias em aberto (a decidir)”, “nunca some, nunca vira falta sozinho”. Não define se texto vazio é resposta. `app/docs/CORTES.md:179`.
- L-088: furo é “marco nao cumprido, que e PERGUNTA em aberto e nao decisao”. Não define resposta vazia. `app/docs/LEIS.md:120`.
- L-002: “TESTEMUNHA LE, NAO RECALCULA: tela, PDF, TXT, badge e contador leem a MESMA autoridade”. Sustenta compartilhamento de autoridade, sem escolher valor do predicado para vazio. `app/docs/LEIS.md:60`.
- L-061: “O CONSUMIDOR LE A LAMPADA, nunca re-julga: chamado, pergunta, tela e folha leem a celula/ata”. Não define resposta vazia. `app/docs/LEIS.md:95`.
- C7: “DisputaSupervisao.pode_fechar (fio_vivo C6 vazio + nenhuma resposta sem veredito) e o juiz unico da pergunta C7 ‘a disputa pode fechar?’”. Não define texto vazio. `app/docs/CORTES.md:272`.
- A formulação geral “resposta sem veredito segura” existe na documentação do código: fechar é veredito sobre silêncio, não sobre resposta; resposta sem veredito está parada no admin; materializada não conta. `app/chamados/models.py:1874-1884`.
- `fechar` atribui ao aval C7 literal: “fecha quando `pergunta_viva(disputa)` esta vazio (C6), e a porta recusa resposta sem veredito, como ja faz”; comentário diz recusar em todas as portas. Nenhum desses trechos define se vazio conta como resposta. `app/chamados/models.py:1922-1930`.
- L-110: referência é regra, “o codigo de hoje nao e gabarito”; onde regra não responde, sobe pergunta de lei. É pertinente para não converter teste/código existente em decisão normativa. `app/docs/LEIS.md:136`.
- Contrato “1 JUIZ por pergunta”, não “um juiz para todas as perguntas”: `CLAUDE.md:178`. Dois predicados podem exigir distinguir perguntas; os textos disponíveis da O168 chamam CSF e retratação da **mesma** pergunta, portanto separar especificamente essas respostas não tem autorização literal encontrada. `app/chamados/models.py:1896-1898`; `app/chamados/juizes.py:2063-2066`. Corte para novo predicado ou duas candidatas é tratado em `CLAUDE.md:269-273`.
- Anexo JUIZES é lookup, não lei; a lei mora em CLAUDE 4b e registro vivo em core/juizes. `app/docs/JUIZES.md:7-14`.
- Registro vivo pergunta: “quais respostas do colaborador na disputa esperam veredito (nem validadas nem materializadas)?”, autoridade `respostas_sem_veredito`. Não acrescenta condição de vazio. `app/core/juizes.py:302`.

**Compatibilidade por leitura:** é possível satisfazer simultaneamente R-a como mudança no **conteúdo** da pergunta e R-b como mudança no **endereçamento** dos leitores. Atualmente `respostas_sem_veredito` delega a `sem_veredito_entre`; esse corpo consulta `respondida_sem_texto`; CSF/retratação compartilham esse corpo. Logo, excluir “respondida sem texto” do juiz não contradiz, por estrutura, exigir que todos perguntem a esse juiz. `app/chamados/models.py:1886-1909`; `app/chamados/juizes.py:2063-2076`. Se o aval O168 quis **também** obrigar inclusão de texto vazio, não sei: essa definição não está em `CORTES.md:449`, e a íntegra do aval não está no dado fornecido **[não verificado]**.

## 14/11 04:02 · sessao 1e7c0e692fc2 · HJ-091 · verificador · origem=Code

**[bloqueia] REPROVA nesta volta:** há passivo que o contador considera coberto, escritor de férias fora dos declarados e caminho web com exceção sem tratamento. O selo não detecta o escritor adicional. (`app/ferias/services.py:153`; `app/ponto/services/ausencia.py:1451`; `app/ferias/views.py:722`; `app/ponto/tests/selo_hj091.py:26`.)

Identidade do commit/base, completude do diff fornecido, resultados históricos RED/GREEN, execução de deploy e medidas de produção: **[nao verificado]**. [nao verificado]

**1. Aval realizado e escopo respeitado**

| Item | Parecer |
|---|---|
| FK nullable | **[fila] Realizado**, com `CASCADE`. (`app/ponto/models.py:796`; `app/ponto/migrations/0078_ausencia_agendamento_ferias.py:15`.) |
| Backfill pela descrição, DRY e contador | **[fila] Implementado**, com ressalvas de reversão, campos escritos e concorrência no item 9. (`app/ponto/management/commands/vincular_ausencias_ferias.py:21`; `app/ponto/services/ausencia.py:1377`.) |
| Espelho só reconhece o mesmo agendamento | **[fila] Realizado** pela FK; criação, exclusão e rejeição passaram a usar a origem. (`app/ponto/services/ausencia.py:1253`; `app/ponto/services/ausencia.py:1278`; `app/ponto/services/ausencia.py:1339`; `app/ponto/services/ausencia.py:1352`.) |
| Recusar portas genéricas | **[bloqueia] Parcial:** as guardas existem, mas sobra `rederivar_fim`, e nem todo chamador trata a nova recusa. (`app/ponto/services/ausencia.py:95`; `app/ponto/services/ausencia.py:1451`; `app/ponto/services/ausencia.py:1069`.) |
| Painel com período, dias, status e origem | **[fila] Realizado para origem resolvida.** O dia consultado aparece separado. (`app/ponto/services/ausencia.py:110`; `app/templates/ponto/partials/painel_ausencia.html:136`; `app/templates/ponto/partials/painel_ausencia.html:139`.) |
| Link para o agendamento | **[fila] Entrega mais fraca que o literal:** mostra o número, mas o link abre o saldo da pessoa, sem identificar o agendamento no endereço. (`app/ponto/services/ausencia.py:76`; `app/templates/ponto/partials/painel_ausencia.html:140`; `app/ferias/urls.py:13`.) |
| Sonda das três classes de passivo | **[bloqueia] Incompleta:** rejeitada própria dentro de agendamento vivo é considerada coberta; detalhe no item 6. (`app/ferias/services.py:146`; `app/ferias/services.py:153`.) |
| Lista ao DP por pessoa | **[bloqueia] Não comprovada:** o tripwire imprime totais; o backfill imprime IDs de ausências por motivo. Publicação da lista nominal/por pessoa e da sonda: **[nao verificado]**. (`app/ferias/management/commands/tripwire_status_ferias.py:64`; `app/ponto/management/commands/vincular_ausencias_ferias.py:45`.) |
| Contadores no ESTADO | **[bloqueia] Integração [nao verificado].** Consumo confirmado no tripwire; o teste verifica sua saída textual. (`app/ferias/management/commands/tripwire_status_ferias.py:43`; `app/ponto/tests/test_hj091_ferias_derivadas.py:294`.) |

**[fila]** No diff fornecido, `folha/export.py` não aparece alterado; continua consultando `ferias_na_janela` para férias com batida. Igualdade integral desse arquivo com a base: **[nao verificado]**. (`app/folha/export.py:484`; `app/folha/export.py:491`.)

**[fila]** As recusas de criação avulsa e de anexo ampliam as portas afetadas, coerentemente com “escritor único”; a API agora trata a recusa do anexo. Os ajustes de precedência usam criação direta somente nas fixtures de teste. (`app/ponto/services/ausencia.py:247`; `app/ponto/services/ausencia.py:921`; `app/api/views.py:3111`; `app/ponto/tests/test_a2_precedencia.py:89`.)

**2. Quem escreve e quem lê**

**[fila] FK — escritores explícitos encontrados no código de aplicação:**

| Escritor | Operação |
|---|---|
| `espelhar_ferias → criar_ausencia` | Passa a origem; a porta inclui a FK na instância e salva. (`app/ponto/services/ausencia.py:1278`; `app/ponto/services/ausencia.py:414`; `app/ponto/services/ausencia.py:446`; `app/ponto/services/ausencia.py:479`.) |
| `vincular_ausencias_ferias` | Preenche FK nula por `update`. (`app/ponto/services/ausencia.py:1413`.) |
| `reverter_vinculo_ausencias_ferias` | Volta a FK para NULL, condicionado ao mesmo agendamento. (`app/ponto/services/ausencia.py:1436`.) |

**[fila] FK — leitores explícitos encontrados:**

| Leitor | Uso |
|---|---|
| Mensagem e painel | Identificação da origem e período integral. (`app/ponto/services/ausencia.py:85`; `app/ponto/services/ausencia.py:106`.) |
| `criar_ausencia` | Valida presença/tipo e restringe busca de rejeitada à mesma origem. (`app/ponto/services/ausencia.py:247`; `app/ponto/services/ausencia.py:249`; `app/ponto/services/ausencia.py:314`.) |
| Auditoria da criação | Registra o ID da origem. (`app/ponto/services/ausencia.py:509`.) |
| Espelho | Seleciona exclusivamente as próprias linhas. (`app/ponto/services/ausencia.py:1253`.) |
| Apagar/rejeitar férias | Seleciona pela origem. (`app/ponto/services/ausencia.py:1339`; `app/ponto/services/ausencia.py:1352`.) |
| Backfill/reversão | Seleção de NULL e condições dos updates. (`app/ponto/services/ausencia.py:1377`; `app/ponto/services/ausencia.py:1413`; `app/ponto/services/ausencia.py:1436`.) |
| Guarda de sobreposição | Define quais ausências são próprias. (`app/ferias/models.py:431`.) |
| Contador | Valida origem e monta pares origem/dia. (`app/ferias/services.py:147`; `app/ferias/services.py:154`.) |
| Exclusão da fonte | `CASCADE` elimina derivados; a exclusão da instância também chama a limpeza. (`app/ponto/models.py:797`; `app/ferias/models.py:453`.) |

**[bloqueia] Existe segundo escritor de férias fora dessas portas:** `curar_ausencia_sem_fim --apply` seleciona tipos vivos com duração maior que um, sem excluir férias; chama `rederivar_fim`, que salva sem guarda. O modelo pode preencher `data_fim`. Existência dessas linhas em produção: **[nao verificado]**. (`app/ponto/management/commands/curar_ausencia_sem_fim.py:29`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`; `app/ponto/services/ausencia.py:1451`; `app/ponto/models.py:912`.)

**[fila] Demais campos — escritores e leitores conferidos, inclusive fora do diff:**

| Campos | Escritores | Leitores |
|---|---|---|
| `tipo`, `colaborador`, `data` | Criação; edição guarda férias antes de alterar tipo/data. (`app/ponto/services/ausencia.py:401`; `app/ponto/services/ausencia.py:536`; `app/ponto/services/ausencia.py:561`.) | Guarda, espelho, backfill, contador e cobertura. (`app/ponto/services/ausencia.py:95`; `app/ponto/services/ausencia.py:1256`; `app/ponto/services/ausencia.py:1378`; `app/ferias/services.py:150`; `app/ponto/turnos.py:2554`.) |
| `data_fim`, `dias_corridos` | Criação/edição; derivação no modelo; `rederivar_fim`. (`app/ponto/services/ausencia.py:402`; `app/ponto/services/ausencia.py:563`; `app/ponto/models.py:911`; `app/ponto/models.py:917`; `app/ponto/services/ausencia.py:1451`.) | Cobertura/expansão, folha, relatórios, vigia e apresentação do dia. (`app/ponto/turnos.py:2772`; `app/ponto/turnos.py:2791`; `app/folha/export.py:70`; `app/relatorios/views.py:205`; `app/ponto/services/vigia_ausencia.py:136`; `app/ponto/services/dia_decidido.py:620`.) |
| `status` | Criação; `mudar_estado`; rejeição pela origem. (`app/ponto/services/ausencia.py:403`; `app/ponto/services/ausencia.py:889`; `app/ponto/services/ausencia.py:1353`.) | Contador, cobertura por efeito, vigias, API e materialização. (`app/ferias/services.py:146`; `app/ponto/turnos.py:2516`; `app/ponto/services/vigia_ausencia.py:108`; `app/api/views.py:1858`; `app/chamados/services/materializacao.py:662`.) |
| `minutos` | Espelho passa zero; criação/edição persistem; modelo deriva para falta aprovada. (`app/ponto/services/ausencia.py:1275`; `app/ponto/services/ausencia.py:402`; `app/ponto/services/ausencia.py:571`; `app/ponto/models.py:887`.) | Rateio da folha, API, lista e apresentação do dia. (`app/folha/export.py:121`; `app/api/views.py:1858`; `app/templates/ponto/ausencias.html:159`; `app/ponto/services/dia_decidido.py:496`.) |
| `descricao` | Criação, edição, decisão e reabertura; guardas públicas impedem férias nas portas genéricas. (`app/ponto/services/ausencia.py:403`; `app/ponto/services/ausencia.py:566`; `app/ponto/services/ausencia.py:987`; `app/ponto/services/ausencia.py:1206`.) | Backfill interpreta `#N`; painel/API/dia exibem texto. (`app/ponto/services/ausencia.py:1381`; `app/templates/ponto/partials/painel_ausencia.html:57`; `app/api/views.py:1858`; `app/ponto/services/dia_decidido.py:624`.) |
| `documento` | Criação, anexo e reabertura. (`app/ponto/services/ausencia.py:410`; `app/ponto/services/ausencia.py:923`; `app/ponto/services/ausencia.py:1196`.) | Estado inicial/rejulgamento, propriedade de tipo real, lista e dia. (`app/ponto/services/ausencia.py:392`; `app/ponto/services/ausencia.py:606`; `app/ponto/models.py:857`; `app/templates/ponto/ausencias.html:168`; `app/ponto/services/dia_decidido.py:623`.) |
| `aprovado_por`, `aprovada_em`, `registrado_por`, `atualizado_em` | Criação/decisão, carimbo do modelo, rejeição e backfill/reversão. (`app/ponto/services/ausencia.py:404`; `app/ponto/services/ausencia.py:407`; `app/ponto/services/ausencia.py:981`; `app/ponto/models.py:882`; `app/ponto/services/ausencia.py:1353`; `app/ponto/services/ausencia.py:1414`; `app/ponto/services/ausencia.py:1437`.) | Histórico e verificação de aprovador; o arquivo de reversão não conserva o timestamp anterior. (`app/chamados/services/historico.py:129`; `app/ponto/services/vigia_ausencia.py:120`; `app/ponto/services/ausencia.py:1409`.) |
| Agendamento: período/colaborador, início/fim, dias, status, agendador | Criação normal/histórica/solicitação; troca de janela; cancelamento e sincronização. (`app/ferias/services.py:519`; `app/ferias/services.py:579`; `app/ferias/services.py:773`; `app/ferias/services.py:714`; `app/ferias/services.py:654`; `app/ferias/services.py:686`; `app/ferias/services.py:938`.) | Espelho, backfill, painel, contador, saldo e folha. (`app/ponto/services/ausencia.py:1247`; `app/ponto/services/ausencia.py:1276`; `app/ponto/services/ausencia.py:1396`; `app/ponto/services/ausencia.py:110`; `app/ferias/services.py:149`; `app/ferias/services.py:105`; `app/folha/export.py:491`.) |

**[fila]** Outros consumidores conferidos dos mesmos campos: precedência, grade, espelho, motor, signals, carga de perguntas e foto do acervo. (`app/ponto/precedencia.py:338`; `app/escala/utils.py:1189`; `app/ponto/services/espelho.py:582`; `app/ponto/services/espelho.py:588`; `app/ponto/motor_calculo_v2.py:1092`; `app/ponto/signals.py:123`; `app/chamados/services/carga_pergunta_viva.py:101`; `app/chamados/services/foto_e4b.py:285`.)

Exaustividade para aliases dinâmicos, SQL e todos os leitores indiretos de todos esses campos: **[nao verificado]**.

**3. Segundo juiz / derivação paralela**

**[fila]** A sobreposição continua delegada ao juiz registrado: mudou a identificação das linhas próprias, sem copiar o julgamento. (`app/ferias/models.py:431`; `app/ferias/models.py:433`; `app/core/juizes.py:254`.)

**[fila]** `STATUS_QUE_ESPELHAM` é compartilhado pelos três novos consumidores, mas representa separadamente “não cancelado”, já usado por `ferias_na_janela`. Os conjuntos são equivalentes nos quatro status declarados; resta duplicidade de representação. (`app/ferias/services.py:132`; `app/ponto/services/ausencia.py:1248`; `app/ponto/services/ausencia.py:1375`; `app/ferias/services.py:156`; `app/ferias/services.py:53`; `app/ferias/models.py:296`.)

**[fila]** Isso não duplica `status_por_data` nem `status_de_periodo`: respondem, respectivamente, fase temporal do agendamento e situação do período aquisitivo. (`app/ferias/models.py:276`; `app/core/juizes.py:269`.)

**4. Relógio fora da borda**

**[fila]** Não encontrei relógio nas decisões novas de recusa, origem, seleção do espelho, contador ou elegibilidade do backfill. As novas chamadas de `now()` são carimbos; o tripwire lê o dia na borda do command. (`app/ponto/services/ausencia.py:95`; `app/ponto/services/ausencia.py:106`; `app/ponto/services/ausencia.py:1249`; `app/ferias/services.py:149`; `app/ponto/services/ausencia.py:1396`; `app/ponto/services/ausencia.py:1353`; `app/ponto/services/ausencia.py:1414`; `app/ponto/services/ausencia.py:1437`; `app/ferias/management/commands/tripwire_status_ferias.py:28`.)

**5. Os REDs mordem?**

**[fila]** O cancelamento canônico verifica permanência de `aprovada` e frase; os testes de origem alheia e cancelamento com linha deslocada verificam efeitos relevantes. Execução Django e morte efetiva dos mutantes nesta cópia: **[nao verificado]**. (`app/ponto/tests/test_hj091_ferias_derivadas.py:74`; `app/ponto/tests/test_hj091_ferias_derivadas.py:157`; `app/ponto/tests/test_hj091_ferias_derivadas.py:195`.)

**[fila] Sim, há teste que passa com a cura revertida:** `test_a_recusa_MORDE...` instala ele próprio a guarda vazia e espera rejeição. Ele é contraprova do mecanismo; o canônico anterior é quem deve matar esse mutante. (`app/ponto/tests/test_hj091_ferias_derivadas.py:128`; `app/ponto/tests/test_hj091_ferias_derivadas.py:132`; `app/ponto/tests/test_hj091_ferias_derivadas.py:135`.)

**[bloqueia] O selo não garante escritor único.** Não examina `save()` nem atribuição de status/FK. Na execução somente em memória, ambos os exemplos abaixo retornaram lista vazia; a árvore atual também não acusou escritor externo. (`app/ponto/tests/selo_hj091.py:26`; `app/ponto/tests/selo_hj091.py:78`; `app/ponto/tests/selo_hj091.py:89`.)

```python
a = Ausencia.objects.filter(tipo="ferias").first()
a.status = "rejeitada"
a.save()
```

```python
a = Ausencia.objects.filter(tipo="ferias").first()
a.agendamento_ferias = g
a.save(update_fields=["agendamento_ferias"])
```

**[fila]** A caracterização de remarcação sem origem espera `ValidationError` diretamente no modelo; não verifica a tradução dessa recusa pela view. Os testes APPLY não exigem arquivo anterior à escrita. (`app/ponto/tests/test_hj091_ferias_derivadas.py:442`; `app/ponto/tests/test_hj091_ferias_derivadas.py:318`; `app/ponto/tests/test_hj091_ferias_derivadas.py:355`.)

**6. ESMERIL-SEMÂNTICA**

**[bloqueia] Status/cobertura divergem.** O contador exclui rejeitadas do primeiro lado e aceita qualquer status no conjunto de dias cobertos. Uma rejeitada própria em agendamento vivo pode produzir **0/0**, embora folha exija aprovada e cobrança exclua rejeitada. Preservar o passivo sem corrigi-lo não dispensa denunciá-lo. (`app/ferias/services.py:146`; `app/ferias/services.py:153`; `app/ponto/turnos.py:2516`; `app/ponto/turnos.py:2519`.)

**[bloqueia]** O espelho conserva explicitamente a rejeitada própria, e o teste fixa essa preservação sem exigir denúncia pelo contador. Essa classe do passivo permanece sem detector correspondente. (`app/ponto/services/ausencia.py:1269`; `app/ponto/tests/test_hj091_ferias_derivadas.py:178`.)

**[fila]** Para linhas novas normais, as datas são inclusivas; cada derivado tem um dia, zero minutos e fim nulo interpretado como dia único. O painel lê os dias integrais do agendamento. (`app/ponto/services/ausencia.py:1265`; `app/ponto/services/ausencia.py:1275`; `app/ponto/turnos.py:2777`; `app/ponto/services/ausencia.py:110`.)

**[fila]** Para legado com range, há hipótese diferente: backfill confere somente `data`; contador considera somente `(origem, data)`, enquanto cobertura oficial expande até `data_fim`. Existência desse formato em produção: **[nao verificado]**. (`app/ponto/services/ausencia.py:1378`; `app/ponto/services/ausencia.py:1398`; `app/ferias/services.py:154`; `app/ponto/turnos.py:2772`.)

**7. Ordem de trava e de pouso** [nao verificado]

**[fila]** A ordem existente é agendamento → período → ausência. Na troca de janela, limpeza e save estão no mesmo `atomic`; o veto desfaz a limpeza. (`app/ferias/services.py:634`; `app/ferias/services.py:637`; `app/ferias/services.py:704`; `app/ferias/services.py:723`; `app/ferias/services.py:727`.)

**[bloqueia]** Migration e backfill não estão acoplados: a migration só adiciona a coluna, e o command está fora do pipeline. Garantia operacional de “mesmo ato, antes de qualquer save”: **[nao verificado]**. (`app/ponto/migrations/0078_ausencia_agendamento_ferias.py:15`; `app/config/crons.py:781`.)

**[fila] Entre migration e backfill:** linhas antigas ficam sem origem; o espelho não as reconhece, tenta criar e transforma colisões em recusa/Pauta. Mudança somente de status passa pela guarda. Cancelamento limpa somente linhas vinculadas, deixando as antigas NULL. (`app/ponto/services/ausencia.py:1253`; `app/ponto/services/ausencia.py:1273`; `app/ponto/services/ausencia.py:1288`; `app/ferias/models.py:406`; `app/ponto/services/ausencia.py:1339`.)

**[fila] Entre backfill e primeira gravação:** resolvidas já têm origem, sem disparar signals pelo `update`; não resolvidas continuam NULL. O primeiro espelho ainda preserva rejeitada própria. (`app/ponto/services/ausencia.py:1414`; `app/ponto/services/ausencia.py:1403`; `app/ponto/services/ausencia.py:1269`.)

**[bloqueia] Há porta com caminho para 500:** remarcação que cruza ausência sem origem levanta `ValidationError`; `editar_agendamento` captura somente duas outras exceções. A porta administrativa de correção captura `ValidationError`. Resposta HTTP executada: **[nao verificado]**. (`app/ferias/models.py:437`; `app/ferias/views.py:722`; `app/ferias/views.py:723`; `app/ferias/views.py:725`; `app/ferias/views.py:544`.)

**8. Chamadores das portas que recusam**

| Porta | Chamadores/tratamento |
|---|---|
| `decidir` | **[fila]** Wrapper em `app/ponto/views.py:88`; lotes em `app/ponto/views.py:2301`, `app/ponto/views.py:2337`, `app/ponto/views.py:2377`, com captura em `app/ponto/views.py:2306`, `app/ponto/views.py:2344`, `app/ponto/views.py:2383`; individuais/período em `app/ponto/views.py:2675`, `app/ponto/views.py:2708`, `app/ponto/views.py:2736`, `app/ponto/views.py:2772`, tratados em `app/ponto/views.py:2690`, `app/ponto/views.py:2709`, `app/ponto/views.py:2751`, `app/ponto/views.py:2786`. |
| Editar | **[fila]** Web e command H06 capturam a recusa. (`app/ponto/views.py:2834`; `app/ponto/views.py:2858`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:171`.) |
| Reverter | **[fila]** View captura; `_reabrir` recebe chamadas da criação e da reversão pública. (`app/ponto/views.py:2616`; `app/ponto/views.py:2620`; `app/ponto/services/ausencia.py:350`; `app/ponto/services/ausencia.py:1170`.) |
| Anexar | **[fila]** Web captura; API retorna 409. Reenvio da API não captura nessa chamada, mas férias são recusadas antes na criação. (`app/ponto/views.py:2158`; `app/ponto/views.py:2159`; `app/api/views.py:3111`; `app/api/views.py:1838`; `app/ponto/services/ausencia.py:247`; `app/api/views.py:1813`.) |
| `mudar_estado` | **[bloqueia]** Chamado por decisão, anexo, reabertura, rejulgamento e escalonador. Os dois últimos podem propagar a nova exceção. (`app/ponto/services/ausencia.py:978`; `app/ponto/services/ausencia.py:940`; `app/ponto/services/ausencia.py:1209`; `app/ponto/services/ausencia.py:609`; `app/ponto/services/ausencia.py:1069`.) |
| Criar | **[fila]** Administração, colaborador web, API, veredito da célula, copiloto e espelho têm captura. (`app/ponto/views.py:2455`; `app/ponto/views.py:2575`; `app/api/views.py:1813`; `app/ponto/services/veredito_celula.py:341`; `app/core/views_ia.py:988`; `app/ponto/services/ausencia.py:1280`.) |
| Garantir/materializar | **[fila]** Regularização e abandono chamam para falta; materialização declara falta/atestado. (`app/ponto/services/regularizacao.py:118`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76`; `app/chamados/services/materializacao.py:491`; `app/chamados/services/materializacao.py:527`; `app/chamados/services/materializacao.py:603`.) |

**[bloqueia, condicionado ao acervo]** Férias antigas em `aguardando_documento` podem interromper escalonamento ou `rejulgar_status_por_tipo --apply`: ambos alcançam a guarda sem captura por item. O escalonador está instalado com `--apply

[truncado em 20000 de 22593 caracteres]

## 14/11 03:59 · sessao 79cf44923d6c · - · - · origem=Ronald (tmux)

[跛bloqueia] O contador não detecta a classe “férias rejeitadas/canceladas dentro de agendamento vivo”: rejeitadas saem de `sem_agendamento`, mas entram em `cobertos`, produzindo 0/0 para um dia sem ausência aprovada. O espelho também conserva a rejeitada. Isso contradiz o significado de cobertura da folha e da cobrança; listar o passivo não exige corrigi-lo. (`app/ferias/services.py:146`, `app/ferias/services.py:153`, `app/ponto/services/ausencia.py:1269`, `app/ponto/turnos.py:2511`.)

[fila] O teste preserva expressamente a rejeitada, mas não exige que o contador a denuncie; portanto não cobre a classe do passivo exigida no aval. (`app/ponto/tests/test_hj091_ferias_derivadas.py:178`.)

[fila] O contador pressupõe uma linha por dia: usa apenas `(agendamento_id, data)`, enquanto os leitores oficiais expandem `data..data_fim`. O backfill nem carrega `data_fim`/`dias_corridos`; uma linha legada com intervalo pode receber FK por seu primeiro dia sem conferência do restante. Existência dessa classe em produção: [nao verificado]. (`app/ferias/services.py:153`, `app/ponto/services/ausencia.py:1377`, `app/ponto/turnos.py:2771`.)

[fila] `STATUS_QUE_ESPELHAM` centraliza espelho, backfill e contador, mas continua uma representação separada de “não cancelado”, já implementada em `ferias_na_janela`. Com os quatro status declarados são equivalentes; divergência para status fora dos choices em produção: [nao verificado]. Não duplica `status_por_data`: este decide a fase temporal; também não duplica `status_de_periodo`, que trata do período aquisitivo. (`app/ferias/services.py:49`, `app/ferias/services.py:132`, `app/ferias/models.py:276`, `app/ferias/models.py:296`, `app/core/juizes.py:269`.)

[fila] Sobreposição continua delegada ao juiz registrado: o diff troca somente a identificação das linhas próprias pela FK. (`app/ferias/models.py:431`, `app/ferias/models.py:433`, `app/core/juizes.py:254`.)

[fila] As decisões novas conferidas não consultam relógio: recusas, seleção da origem, espelho, contador e seleção do backfill usam argumentos/estado. As chamadas novas de `timezone.now()` gravam carimbos; o `hoje` do tripwire é lido na borda do command. (`app/ponto/services/ausencia.py:92`, `app/ponto/services/ausencia.py:101`, `app/ponto/services/ausencia.py:1248`, `app/ferias/services.py:146`, `app/ponto/services/ausencia.py:1377`, `app/ponto/services/ausencia.py:1353`, `app/ponto/services/ausencia.py:1414`, `app/ponto/services/ausencia.py:1437`, `app/ferias/management/commands/tripwire_status_ferias.py:28`.)

[fila] Inclusividade coerente nas linhas novas normais: espelho inclui início/fim, cria `dias_corridos=1` e `minutos=0`; ausência diária tem `data_fim=None`, interpretado como dia único pelos leitores oficiais. O painel lê o intervalo integral e `dias` do agendamento. (`app/ponto/services/ausencia.py:1265`, `app/ponto/services/ausencia.py:1275`, `app/ponto/services/ausencia.py:115`, `app/ponto/turnos.py:2772`, `app/ponto/services/ausencia.py:108`.)

Censo complementar de leitores fora do diff, sem garantia de exaustividade para aliases dinâmicos [nao verificado]:

- API de ausências: colaborador, tipo, data, status, descrição, minutos e criação; cobertura/status aberto no espelho do app. (`app/api/views.py:1846`, `app/api/views.py:1858`, `app/api/views.py:1560`, `app/api/views.py:1569`.)
- Espelho: cobertura de cobrança e folha; tipo para ícone. Motor: tipo da ausência do mapa de dias abonados. (`app/ponto/services/espelho.py:582`, `app/ponto/services/espelho.py:588`, `app/ponto/services/espelho.py:849`, `app/ponto/motor_calculo_v2.py:1092`.)
- Signals: tipo, data, fim e colaborador para recalcular dias. (`app/ponto/signals.py:123`, `app/ponto/signals.py:147`, `app/ponto/signals.py:152`.)
- Histórico, carga da pergunta e foto: colaborador, criação, tipo, data, fim, status por `_pelo_efeito`, registrador. (`app/chamados/services/historico.py:129`, `app/chamados/services/carga_pergunta_viva.py:101`, `app/chamados/services/foto_e4b.py:285`.)
- Materialização: tipo, data, colaborador e status rejeitada. (`app/chamados/services/materializacao.py:626`, `app/chamados/services/materializacao.py:662`, `app/chamados/services/materializacao.py:669`.)
- Lista/painel: dias corridos, minutos, documento, referência documental, descrição e intervalo. (`app/templates/ponto/ausencias.html:159`, `app/templates/ponto/ausencias.html:168`, `app/templates/ponto/partials/painel_ausencia.html:51`, `app/templates/ponto/partials/painel_ausencia.html:56`, `app/templates/ponto/partials/painel_ausencia.html:123`.)
- Agendamento: folha lê intervalo inclusivo; situação cruza batidas com `[início, fim+1dia)`; tela registra datas/dias/status; soma de saldo lê dias de não cancelados. (`app/folha/export.py:491`, `app/folha/export.py:501`, `app/ferias/situacao.py:271`, `app/ferias/situacao.py:280`, `app/ferias/views.py:528`, `app/ferias/services.py:105`.)

## 14/11 03:58 · sessao f65023c9f0e5 · - · - · origem=Ronald (tmux)

**[bloqueia] Escritor fora dos declarados:** `curar_ausencia_sem_fim --apply` não exclui férias: seleciona ausência viva com `dias_corridos > 1`, `data_fim=None` e tipo fora de `EM_ABERTO_VIVOS`; chama `rederivar_fim`, que executa `save()` sem guarda. O modelo pode então escrever `data_fim` de férias. Existência desse passivo em produção: [nao verificado]. (`app/ponto/management/commands/curar_ausencia_sem_fim.py:29`, `:46`; `app/ponto/services/ausencia.py:1448`; `app/ponto/models.py:912`.)

**[bloqueia] Selo não garante escritor único:** `_METODOS` omite `save`; atribuições só são denunciadas quando o atributo é `tipo` e o valor literal é `"ferias"`. Consequentemente, `a = Ausencia.objects.filter(tipo="ferias").first(); a.status="rejeitada"; a.save()` e `a.agendamento_ferias=g; a.save(...)` escapam. Ambos devolveram `[]` numa execução somente em memória com `python3 -B`. (`app/ponto/tests/selo_hj091.py:26`, `:76`, `:85`.) Também substituí somente em memória o corpo da guarda por `pass`: o conjunto de escritores identificado permaneceu idêntico. (`app/ponto/tests/selo_hj091.py:87`; `app/ponto/tests/test_hj091_ferias_derivadas.py:387`.)

**[bloqueia] Reversão durável opcional no serviço:** `vincular_ausencias_ferias(aplicar=True)` admite `registro_em=None`; nesse caso grava FK sem antes escrever arquivo de reversão. O comando fornece um caminho, mas o serviço público não exige. Os testes APPLY exercitam justamente a chamada sem arquivo e não comprovam a ordem arquivo → UPDATE. (`app/ponto/services/ausencia.py:1365`, `:1407`, `:1413`; `app/ponto/management/commands/vincular_ausencias_ferias.py:33`; `app/ponto/tests/test_hj091_ferias_derivadas.py:318`, `:333`, `:355`, `:445`.)

**[bloqueia, se exigido literalmente “só FK”]** Apply e reversão escrevem também `atualizado_em`; o arquivo só conserva pares ausência/agendamento, sem o timestamp anterior. Portanto a reversão não restaura todo o estado anterior. (`app/ponto/services/ausencia.py:1409`, `:1414`, `:1437`.)

**[fila] REDs e mutantes:**

- O canônico de cancelamento verifica estado aprovado e frase; ele distingue a cura do cancelamento permitido. (`app/ponto/tests/test_hj091_ferias_derivadas.py:68`.)
- O teste chamado `test_a_recusa_MORDE...` passa com a guarda já revertida: instala explicitamente a guarda no-op e espera rejeição. É uma contraprova invertida; quem deve matar o mutante é o canônico anterior. (`app/ponto/tests/test_hj091_ferias_derivadas.py:128`.)
- Remover apenas a guarda de `reverter_rejeicao` pode deixar o respectivo teste verde, porque `_reabrir` alcança a guarda redundante de `mudar_estado`. (`app/ponto/tests/test_hj091_ferias_derivadas.py:110`; `app/ponto/services/ausencia.py:1164`, `:1209`, `:881`.)
- Faltam testes de falha ao escrever reversão, arquivo já existente, preservação de todos os demais campos, origem cancelada no backfill e escrita por `save()`. A cobertura atual do backfill verifica contagens/FK e reversão por lista em memória. (`app/ponto/tests/test_hj091_ferias_derivadas.py:305`, `:314`, `:324`, `:351`, `:360`, `:376`.)

**[fila] Backfill — partes conferidas:**

- Idempotente em execução sequencial: lê apenas FK nula e o UPDATE repete essa condição; não cria ausências. Segunda execução com o mesmo arquivo pelo comando recusa antes de trabalhar; com outro arquivo ou pelo serviço, não encontra novos vínculos. (`app/ponto/services/ausencia.py:1377`, `:1413`; `app/ponto/management/commands/vincular_ausencias_ferias.py:36`.)
- Havendo caminho, o JSON é escrito com modo exclusivo antes da transação de UPDATE. (`app/ponto/services/ausencia.py:1407`.)
- Não escolhe entre IDs distintos: mais de uma marca distinta vira `marca_ambigua`; marcas repetidas do mesmo ID são deduplicadas. (`app/ponto/services/ausencia.py:1381`, `:1390`.)
- Não procura outro agendamento cobrindo o dia: exige o ID da descrição, existência, mesma pessoa, dia inclusivamente dentro da janela e status em `STATUS_QUE_ESPELHAM`. Um caso com marca `#77` inadequada e cobertura por `#114` permanece sem vínculo. (`app/ponto/services/ausencia.py:1393`, `:1396`, `:1398`, `:1400`.)
- Concorrência entre leitura e aplicação não é protegida por trava/revalidação do agendamento: a transação começa depois da seleção e o UPDATE revalida somente ausência/tipo/FK nula. O risco depende de haver gravações concorrentes, cuja ocorrência é [nao verificado]. (`app/ponto/services/ausencia.py:1377`, `:1382`, `:1410`, `:1413`.)

Não executei Django nem testes com banco; resultado runtime dos REDs: [nao verificado]. [nao verificado]

## 14/11 03:58 · sessao de254de1db85 · - · - · origem=Ronald (tmux)

**[bloqueia] Há porta web de remarcação com exceção sem tratamento para férias sem origem.** `editar_agendamento` chama `remarcar_agendamento` em `app/ferias/views.py:722` e captura apenas `AgendamentoNaoRemarcavel` e `SaldoInsuficienteParaRemarcar` em `app/ferias/views.py:723` e `app/ferias/views.py:725`. O caminho passa por `_trocar_janela` (`app/ferias/services.py:743`), salva o agendamento (`app/ferias/services.py:727`), exclui somente ausências com FK própria (`app/ferias/models.py:431`) e converte a colisão com uma ausência sem origem em `ValidationError` (`app/ferias/models.py:437`). Portanto, uma remarcação ainda permitida por data que atravesse uma linha sem origem deixa essa exceção sair da view; resposta real em execução: [nao verificado]. O teste caracteriza apenas `ag.save()` sob `assertRaises(ValidationError)`, sem testar a view (`app/ponto/tests/test_hj091_ferias_derivadas.py:435`, `app/ponto/tests/test_hj091_ferias_derivadas.py:442`). A outra porta, `editar_datas_admin`, captura `ValidationError` (`app/ferias/views.py:544`).

**[bloqueia condicionado ao acervo] Nem todo chamador trata a nova recusa.** Uma ausência `ferias` em `aguardando_documento` com prazo vencido faz o cron chamar `mudar_estado` sem captura por item (`app/ponto/services/ausencia.py:1059`, `app/ponto/services/ausencia.py:1069`); a guarda agora levanta `AusenciaInvalida` (`app/ponto/services/ausencia.py:879`, `app/ponto/services/ausencia.py:95`). O command também não captura a exceção (`app/ponto/management/commands/escalonar_documentos_ausencia.py:26`), e está instalado com `--apply` (`app/config/crons.py:715`). O command `rejulgar_status_por_tipo --apply` tem o mesmo problema: inclui todo `aguardando_documento` (`app/ponto/management/commands/rejulgar_status_por_tipo.py:23`) e chama a porta sem captura (`app/ponto/management/commands/rejulgar_status_por_tipo.py:31` → `app/ponto/services/ausencia.py:609`). Existência dessas linhas em produção: [nao verificado].

**Ordem de pouso e comportamento intermediário:** [nao verificado]

- A migration somente adiciona a FK nullable; não chama o backfill (`app/ponto/migrations/0078_ausencia_agendamento_ferias.py:14`). O comando está declarado fora do pipeline (`app/config/crons.py:781`). O deploy executar migration e backfill no mesmo ato: [nao verificado].
- Depois da migration e antes do backfill, o espelho não reconhece a linha sem origem como própria (`app/ponto/services/ausencia.py:1253`); tenta criar e transforma a recusa em Pauta (`app/ponto/services/ausencia.py:1273`, `app/ponto/services/ausencia.py:1279`, `app/ponto/services/ausencia.py:1288`). O teste fixa zero criações, cinco recusas e ausência de duplicata (`app/ponto/tests/test_hj091_ferias_derivadas.py:413`).
- Mudança somente de status passa pela guarda de sobreposição (`app/ferias/models.py:406`) e pode chamar o espelho (`app/ferias/models.py:448`); o teste confirma que a virada não levanta (`app/ponto/tests/test_hj091_ferias_derivadas.py:423`).
- Depois do backfill, linhas resolvidas já têm a FK; o teste consegue remarcar (`app/ponto/tests/test_hj091_ferias_derivadas.py:444`). As não resolvidas continuam sem origem e permanecem sujeitas ao veto da guarda (`app/ponto/services/ausencia.py:1403`, `app/ferias/models.py:431`).
- A ordem de trava das operações existentes é agendamento → período → ausência (`app/ferias/services.py:627`, `app/ferias/services.py:634`, `app/ferias/services.py:637`). `_trocar_janela` é atômica (`app/ferias/services.py:704`), apaga pela FK antes do save (`app/ferias/services.py:723`) e o rollback inclui essa limpeza caso haja veto (`app/ferias/services.py:698`).

**Censo dos chamadores das portas:**

| Porta | Chamadores e tratamento |
|---|---|
| `decidir` | Wrapper `_a4_decidir` (`app/ponto/views.py:88`); lotes aprovar/rejeitar/cancelar (`app/ponto/views.py:2301`, `app/ponto/views.py:2337`, `app/ponto/views.py:2377`), todos com captura de `AusenciaInvalida` (`app/ponto/views.py:2306`, `app/ponto/views.py:2344`, `app/ponto/views.py:2383`). Individuais aprovar/rejeitar/cancelar (`app/ponto/views.py:2675`, `app/ponto/views.py:2736`, `app/ponto/views.py:2772`), com captura (`app/ponto/views.py:2690`, `app/ponto/views.py:2751`, `app/ponto/views.py:2786`). Declaração de período chama em `app/ponto/views.py:2708` e captura em `app/ponto/views.py:2709`. |
| `editar_ausencia_registro` | View em `app/ponto/views.py:2834`, captura em `app/ponto/views.py:2858`; command H06 em `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`, captura em `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:171`. |
| `reverter_rejeicao` | View em `app/ponto/views.py:2616`, captura em `app/ponto/views.py:2620`. |
| `_reabrir` | `criar_ausencia` (`app/ponto/services/ausencia.py:350`) e `reverter_rejeicao` (`app/ponto/services/ausencia.py:1170`). A guarda de férias ocorre antes desses caminhos públicos (`app/ponto/services/ausencia.py:247`, `app/ponto/services/ausencia.py:1164`). |
| `anexar_documento` | Web (`app/ponto/views.py:2158`) captura em `app/ponto/views.py:2159`; API de anexo (`app/api/views.py:3110`) captura e retorna 409 (`app/api/views.py:3111`). Reenvio na API chama sem captura (`app/api/views.py:1838`), mas férias são recusadas antes em `criar_ausencia` (`app/ponto/services/ausencia.py:247`), cuja exceção é tratada pela API (`app/api/views.py:1813`). |
| `mudar_estado` | `decidir` (`app/ponto/services/ausencia.py:978`), `anexar_documento` (`app/ponto/services/ausencia.py:940`), `_reabrir` (`app/ponto/services/ausencia.py:1209`), `rejulgar_status_pelo_tipo` (`app/ponto/services/ausencia.py:609`) e escalonador (`app/ponto/services/ausencia.py:1069`); os dois últimos têm as lacunas descritas acima. |
| `criar_ausencia` | Web administrativa (`app/ponto/views.py:2447`, captura `app/ponto/views.py:2455`); web colaborador (`app/ponto/views.py:2568`, captura `app/ponto/views.py:2575`); API (`app/api/views.py:1805`, captura `app/api/views.py:1813`); veredito da célula (`app/ponto/services/veredito_celula.py:337`, captura `app/ponto/services/veredito_celula.py:341`); copiloto por wrapper (`app/ponto/services/checar_ausencia.py:46`, captura `app/core/views_ia.py:988`); espelho (`app/ponto/services/ausencia.py:1273`, captura `app/ponto/services/ausencia.py:1279`). |
| `garantir_ausencia` | Wrapper chama `criar_ausencia` (`app/ponto/services/ausencia.py:523`); regularização chama somente `falta` (`app/ponto/services/regularizacao.py:118`); abandono provisório chama somente `falta` (`app/ponto/management/commands/marcar_abandono_provisorio.py:76`). |
| Criação pela materialização | Chamadas em `app/chamados/services/materializacao.py:491` e `app/chamados/services/materializacao.py:527`; os escapes declarados são falta/atestado (`app/chamados/services/materializacao.py:603`). |

**[fila para censo de escritores] Existe escritor genérico adicional a conferir:** `rederivar_fim` executa `ausencia.save()` sem guarda de férias (`app/ponto/services/ausencia.py:1448`, `app/ponto/services/ausencia.py:1451`). O command que o chama não exclui expressamente `ferias`: seleciona duração maior que um e fim nulo, fora dos tipos abertos (`app/ponto/management/commands/curar_ausencia_sem_fim.py:29`) e grava em `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`. Existência de férias que satisfaçam o filtro em produção: [nao verificado].

## 14/11 03:57 · sessao 0cad00772e4d · HJ-091 · sonda · origem=Code

| Campo | Valor | Referência |
|---|---:|---|
| ferias_total | 3001 | [nao verificado] |
| status aprovada | 2997 | [nao verificado] |
| status rejeitada | 4 | [nao verificado] |
| ferias_em_ABERTAS | 0 | [nao verificado] |
| ids_abertas | [] | [nao verificado] |

A saída fornecida não contém erro nem traceback. [nao verificado]

## 14/11 03:39 · sessao fa514ab77cc5 · CORTE-DAS-TRAVAS · censo · origem=Code

1. **(a) O Codex não viu o bloqueio com o tema presente.** (`verif_final.q:4` e `verif_final.q:7`)

   **Pergunta — verif_final.q:** “cura do "andavel" do hook Stop”; “Cada peca faz o que o texto do aval diz, nem mais nem menos?” (`verif_final.q:4` e `verif_final.q:7`)

   **Resposta — verif_final.r:** “a cópia contém tetos em segundos por papel e exige ausência de outro item andável para PAREI devolver o turno” (`bin/codex_pergunta.sh:95`; `bin/hook_stop_fila1.py:1151`, referências da resposta).

2. **(b) O Codex viu e a triagem descartou.** (`verif_v2.q:6`)

   **Pergunta — verif_final.q:** “Mais: religar a escrita do logs/AUDITORIA-TRAVAS/triagem.log (parou em 10/11 13:42).” (`verif_final.q:4`)

   **Resposta — verif_final.r:** “O acréscimo no wrapper apenas imprime uma instrução; não chama o escritor de triagem” (`bin/codex_pergunta.sh:469`, referência da resposta).

   **Descarte literal — verif_v2.q:** “#5 destino logs/AUDITORIA-TRAVAS/triagem.log x logs/codex/triagem.log, wrapper so imprime: DESCARTADO (o arquivo de AUDITORIA e copia byte a byte da trilha unica; um 2o escritor violaria um-escritor-por-estado; triar exige juizo de quem leu a resposta) -- vai ao opus como contestavel.” (`verif_v2.q:6`)

3. **(a) O Codex não viu esse bloqueio com o tema presente.** (`verif_final.q:3`; `verif_v2.r:7`)

   **Pergunta — verif_final.q:** “PROIBIDO: afrouxar qualquer trava fora desta lista; usar "so leitor" em obra que toca dinheiro, escala, vinculo ou exportada.” (`verif_final.q:3`)

   **Resposta — verif_v2.r:** “Mesmo assim, esse arquivo continua “so leitor”: o ramo de templates não examina conteúdo.” (`verif_v2.r:7`; `bin/classe_da_obra.py:156`, referência da resposta).

4. **(a) O Codex não viu os casos residuais com o tema presente.** (`verif_v3.q:13`; `verif_v3.r:7`)

   **Pergunta — verif_v3.q:** “O ramo template de classe_da_obra.py: ha mudanca de template/static que move dinheiro, escrita ou universo julgado e ainda sai "so leitor"?” (`verif_v3.q:13`)

   **Resposta — verif_v3.r:** “A regex examina a linha alterada, sem reconhecer tags Django que produzem controles.”; “Portanto, a alteração muda uma entrada de escrita, embora escape da cura.” (`verif_v3.r:7`; `bin/classe_da_obra.py:175`, referência da resposta).

## 14/11 02:27 · sessao 20149fde517f · E3-FORMA-SQL · verificador · origem=Code

1. **[bloqueia] Falta a matriz normativa anunciada para validar integralmente as prioridades.** O anexo disponível se declara histórico da volta 1 e contém status R1–R16, mas não o contrato I1–I8 nem as linhas prescritivas originais. Ele registra R2 como “esperado admin, obtido colab”; essa descrição ficou desatualizada para o caso simultâneo simples, que agora espera e obtém `admin` no teste sem banco. A autorização das demais combinações permanece **[não verificado]**. (`app/docs/e3_v8_matriz.txt:1`; `app/docs/e3_v8_matriz.txt:5`; `app/docs/e3_v8_matriz.txt:9`; `app/chamados/tests/test_e3_portas_tarefa.py:459`)

   As referências abaixo são da cópia indicada. A correspondência criptográfica com o hash informado e a comparação Git com a base são **[não verificado]**.

2. **[fila] A prioridade nova depende da origem do dono; não estabelece “admin sempre vence colab”.** `_portas_da_tarefa` literalmente retorna `None` e modifica `out`; a publicação ocorre depois por `porta_da_tarefa`. O corpo elimina `colab` dos donos das ações quando há ação administrativa alcançável, acrescenta o dono residual e então escolhe `colab` primeiro. (`app/chamados/services/tarefa_do_dia.py:588`; `app/chamados/services/tarefa_do_dia.py:591`; `app/chamados/services/tarefa_do_dia.py:596`; `app/chamados/services/tarefa_do_dia.py:599`; `app/chamados/services/tarefa_do_dia.py:609`)

   Na tabela, **A** são donos humanos das ações exigíveis com porta na fila; **R** são donos correspondentes aos verbos residuais, antes de a escada residual escolher um deles. `C=colab`, `A=admin`. As nove combinações abaixo cobrem a presença dos dois donos nessas fontes; acrescentar `chamado` ou `sistema` não muda a precedência humana. (`app/chamados/services/tarefa_do_dia.py:588`; `app/chamados/services/tarefa_do_dia.py:597`; `app/chamados/juizes.py:1640`)

   | Donos das ações | Donos residuais | Prescrição recuperável da matriz | Docstring antes do diff | Docstring depois / resultado atual |
   |---|---|---|---|---|
   | nenhum | C+A | [não verificado] | colab | colab |
   | C | A | [não verificado] | colab | colab |
   | C | C+A | [não verificado] | colab | colab |
   | A | C | [não verificado] | colab | colab |
   | A | C+A | [não verificado] | colab | colab |
   | C+A | nenhum | **R2: admin** | colab | **admin** |
   | C+A | A | [não verificado] | colab | admin |
   | C+A | C | [não verificado] | colab | **colab** |
   | C+A | C+A | [não verificado] | colab | **colab** |

   Fontes de todas as saídas: ações em `app/chamados/services/tarefa_do_dia.py:588`, descarte em `:591`, residual em `:596`, seleção em `:597`; escada residual em `app/chamados/juizes.py:1640`. A prescrição R2 está registrada em `app/docs/e3_v8_matriz.txt:5`. A coluna anterior deriva do trecho removido no diff fornecido; conferência na base Git **[não verificado]**.

   **Contraexemplo concreto:** `validar@101 + responder@202` publica `admin`; acrescentar `303:cobrar`, residual do mesmo colab-dia, publica `colab`, conservadas as duas ações. Isso corresponde à docstring nova: fonte (1) prioriza admin entre ações; fonte (3) prioriza colab ao combinar residual. Não há divergência demonstrada entre essa docstring e o corpo; falta a prescrição independente da matriz para julgar a combinação. (`app/chamados/services/tarefa_do_dia.py:42`; `app/chamados/services/tarefa_do_dia.py:45`; `app/chamados/services/tarefa_do_dia.py:591`; `app/chamados/services/tarefa_do_dia.py:596`)

   **Linhas nomeadas:** R2 é o único corpo da matriz que coloca as duas ações humanas juntas: mesma porta `101` ou portas distintas `101/202`, obtendo `admin` nos dois casos. R1 contém apenas `responder`; o verbo `validar` da mesma porta não constitui outra ação administrativa. R3 é administrativo isolado. (`app/chamados/tests/test_e3_portas_tarefa.py:441`; `app/chamados/tests/test_e3_portas_tarefa.py:459`; `app/chamados/tests/test_e3_portas_tarefa.py:463`; `app/chamados/tests/test_e3_portas_tarefa.py:473`)

   R4–R5, R7–R12 e R14 não exercitam duas ações humanas simultâneas; R13 exercita a ação administrativa isolada; R15 varia uma pergunta e preserva residual `decidir`, sem afirmar `fio`; R6 está fora da obra e R16 é execução pendente. Portanto esses corpos não prescrevem as combinações adicionais da tabela. (`app/chamados/tests/test_e3_portas_tarefa.py:479`; `app/chamados/tests/test_e3_portas_tarefa.py:490`; `app/chamados/tests/test_e3_portas_tarefa.py:505`; `app/chamados/tests/test_e3_portas_tarefa.py:615`; `app/chamados/tests/test_e3_portas_tarefa.py:633`; `app/chamados/tests/test_e3_portas_tarefa.py:683`; `app/chamados/tests/test_e3_portas_tarefa.py:686`; `app/docs/e3_v8_matriz.txt:16`; `app/docs/e3_v8_matriz.txt:26`)

   **Outra combinação não nomeada:** `decidir_ausencia@101 + responder@202` também passa a publicar `admin`, porque a condição nova examina apenas `de`, não `acao`. R13 fornece `decidir_ausencia`, mas testa essa ação sozinha. Justificação normativa específica dessa extensão: **[não verificado]**. (`app/chamados/services/tarefa_do_dia.py:170`; `app/chamados/services/tarefa_do_dia.py:591`; `app/chamados/tests/test_e3_portas_tarefa.py:629`)

3. **[fila] Escrita e leitura dos cinco campos estão concentradas, mas os consumidores não têm todos o mesmo universo.** O mapa público é uma cópia de cinco chaves da tarefa, sem nova decisão ou persistência. (`app/chamados/services/tarefa_do_dia.py:603`; `app/chamados/services/tarefa_do_dia.py:609`)

   | Campo | Quem escreve/fornece | Quem lê |
   |---|---|---|
   | `fio` | ações + residual, seleção final: `app/chamados/services/tarefa_do_dia.py:588`, `:597`; residual: `app/chamados/juizes.py:1645` | Folha: `app/folha/fora_da_folha.py:89`, `:236`; Vigia: `app/chamados/services/vigia_fio.py:658`; diff: `app/chamados/management/commands/diff_e3_sombra.py:143` |
   | `nao_sei` | inicialização e causas da tarefa: `app/chamados/services/tarefa_do_dia.py:193`, `:258`, `:320` | Folha: `app/folha/fora_da_folha.py:245`; Vigia: `app/chamados/services/vigia_fio.py:651`; furo: `app/chamados/services/furo_sem_canal.py:98` |
   | `rota` | `app/chamados/services/tarefa_do_dia.py:599` | Vigia: `app/chamados/services/vigia_fio.py:657`; furo: `app/chamados/services/furo_sem_canal.py:97`; diff: `app/chamados/management/commands/diff_e3_sombra.py:143` |
   | `fila` | associação à célula: `app/chamados/juizes.py:1568`; filtro nomeável: `app/chamados/services/tarefa_do_dia.py:584` | Folha: `app/folha/fora_da_folha.py:241`; diff: `app/chamados/management/commands/diff_e3_sombra.py:50` |
   | `verbos` | partição: `app/chamados/services/verbos.py:47`; transporte: `app/chamados/juizes.py:1565`; projeção: `app/chamados/services/tarefa_do_dia.py:583` | filtro de `fila` e residual: `app/chamados/services/tarefa_do_dia.py:584`, `:596`; diff: `app/chamados/management/commands/diff_e3_sombra.py:144` |

   **Tela:** a Folha lê fios por dia, escolhe o grupo por colaborador e o retrato converte esse grupo no fio gravado sob `dono`; depois faz o agrupamento visual em baldes e escolhe a frase. Isso não reescreve a porta. O novo `nao_sei` não é transportado por esse caminho da tela. (`app/folha/fora_da_folha.py:190`; `app/folha/fora_da_folha.py:195`; `app/folha/retrato_folha.py:364`; `app/folha/retrato_folha.py:1187`; `app/folha/retrato_folha.py:438`; `app/folha/retrato_folha.py:481`; `app/folha/retrato_folha.py:1069`)

   Pauta lê ausência de fio pela Folha; beco testa existência da projeção; backfill preserva as oito colunas antigas com `linha[:8]`. (`app/ponto/services/pautas_dos_discordantes.py:81`; `app/escala/services/cadastro_realidade.py:497`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:253`)

   **Semântica e contraexemplos:**

   - Dia do turno e dia civil permanecem separados: uma saída após meia-noite pode ter `data_civil` seguinte sem mudar `dia_do_turno`. A associação sem célula usa colaborador e dia forte. (`app/chamados/services/tarefa_do_dia.py:113`; `app/chamados/services/tarefa_do_dia.py:228`; `app/chamados/services/tarefa_do_dia.py:577`; `app/chamados/juizes.py:1574`)
   - `None` significa nenhuma porta; `{}` não é saída válida. Substituir a projeção por `{}` provoca chave ausente nos leitores. Uma porta com `fila=()` continua sendo porta. (`app/chamados/services/tarefa_do_dia.py:609`; `app/folha/fora_da_folha.py:89`; `app/chamados/services/vigia_fio.py:651`; `app/chamados/services/furo_sem_canal.py:97`; `app/chamados/tests/test_e3_portas_tarefa.py:483`)
   - Vigia deduplica colab-dia; grupos da Folha contam colaboradores; furo conta células do seu recorte. Logo comparar diretamente esses totais não prova divergência nem igualdade em produção. (`app/chamados/services/vigia_fio.py:593`; `app/folha/fora_da_folha.py:195`; `app/chamados/services/furo_sem_canal.py:79`; `app/chamados/services/furo_sem_canal.py:109`)

4. **[fila] Checklist das classes reprovadoras: há limites remanescentes, sem novo defeito persistente demonstrado nos corpos alterados.**

   | Classe | Resultado e contraprova |
   |---|---|
   | Segundo juiz duplicando registrado | **Ausente nos corpos alterados.** A projeção copia a tarefa; C17 permanece apenas no comparador. A catraca rejeita um leitor adicional. (`app/chamados/services/tarefa_do_dia.py:609`; `app/chamados/management/commands/diff_e3_sombra.py:127`; `app/chamados/tests/test_e3_pendentes_app.py:145`; `:150`) |
   | Segundo escritor fora da porta | **Ausente nos corpos alterados.** A escrita produtiva acrescentada compõe `out` em memória. Persistência zero em integração ORM: **[não verificado]**. (`app/chamados/services/tarefa_do_dia.py:599`; `app/docs/e3_v8_matriz.txt:6`) |
   | Relógio fora da borda | **Ausente na cadeia E3 com instante injetado; presente no legado sem instante.** `consulta_do_app` ainda omite `agora`; os fallbacks internos continuam existentes. (`app/chamados/juizes.py:1619`; `:1632`; `app/chamados/services/pergunta_no_app.py:61`; `:73`; `app/chamados/juizes.py:592`; `app/chamados/services/verbos.py:35`; `app/chamados/services/painel.py:190`) |
   | Derivação paralela da decisão | **Ausente nos leitores alterados:** leem o mapa; sem porta, leem `nao_sei` da tarefa original. (`app/folha/fora_da_folha.py:235`; `:245`; `app/chamados/services/vigia_fio.py:644`; `:651`; `app/chamados/services/furo_sem_canal.py:98`) |
   | Ordem de trava | **Nenhuma nova ordem de locks SQL identificada nos corpos alterados.** Os contextos usados são de consulta da tranca. Comportamento concorrente ORM: **[não verificado]**. (`app/chamados/services/pergunta_no_app.py:60`; `app/chamados/management/commands/diff_e3_sombra.py:100`) |
   | Pouso antes do pré-requisito | Pouso efetivo **[não verificado]**. O próprio anexo deixa alcance ORM e R16 pendentes; não há prova fornecida de cumprimento desses pré-requisitos. (`app/docs/e3_v8_matriz.txt:6`; `:26`) |
   | Campo com dois significados | **Não demonstrado para dia/porta.** Há duas origens para `verbos[pk]=None`: registro sem verbo ou membro ausente da partição. A projeção perde essa proveniência, mas conserva ambos como ausência de verbo observado; nenhum erro adicional foi demonstrado por isso. (`app/chamados/catalogo/natureza.py:63`; `app/chamados/juizes.py:1565`; `:1651`) |

5. **[fila] Os dois selos destacados conservam mordida, com níveis diferentes de prova.**

   **C6:** executei os três mutantes sem banco; retirar C6, retirar `q_vivas` ou retirar o instante produziu, cada um, uma falha de assert e zero erros após controle verde. O executor exige essa distinção. (`app/chamados/tests/e3_v8_volta2_selos.py:11`; `:36`; `:46`; `app/chamados/tests/test_contract_pergunta_viva.py:81`; `:84`)

   **Golden:** por inspeção, a reversão “chamado vivo sem pergunta não move fio” é rejeitada: o teste cria chamado real, exige fio `chamado` e sua frase, mantém o balde e depois verifica a mudança para admin por projeção substituída. Não depende apenas da contagem do balde. Execução ORM dessa mordida: **[não verificado]**. (`app/folha/tests/test_haiku_fora_da_folha.py:517`; `:522`; `:523`; `:524`; `:535`; `:541`; `:542`)

6. **[fila] Expectativas alteradas: há respaldo referenciado, mas o golden não é justificado especificamente pela linha R7 fornecida.**

   | Alteração | Justificação recuperável |
   |---|---|
   | `colab_ids` nos argumentos do alimentador | R8, ligação sem célula. (`app/chamados/tests/test_e3_portas_tarefa.py:140`; `app/docs/e3_v8_matriz.txt:18`) |
   | F5B: `None/nada` → porta com verbo `None/chamado` | R7 explicitamente registra a inversão. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:291`; `app/docs/e3_v8_matriz.txt:17`) |
   | Texto do relógio no diff | R11 e transmissão atual do instante. (`app/chamados/tests/test_e3_diff_sombra.py:23`; `app/docs/e3_v8_matriz.txt:21`; `app/chamados/services/pergunta_no_app.py:61`) |
   | Assert AST do filtro C6 com `agora` | R11/L-128; preserva C6 e exige instante. (`app/chamados/tests/test_contract_pergunta_viva.py:81`; `app/docs/e3_v8_matriz.txt:21`; `app/docs/LEIS.md:149`) |
   | Mocks acrescentam `nao_sei`; assert de repasse | Contrato aditivo/independência de R5. (`app/chamados/tests/test_e3_bloqueantes.py:188`; `app/chamados/tests/test_o261e_f5_alcance.py:372`; `app/folha/tests/test_haiku_fora_da_folha.py:154`; `app/docs/e3_v8_matriz.txt:201`) |
   | Golden: chamado sem pergunta passa a mover fio | Cita E3-PORTA-SEM-CASO. R7 da tabela trata especificamente membro ausente da partição; I5 não foi fornecido. O respaldo geral aparece no contrato de publicação, mas o aval original está **[não verificado]**. (`app/folha/tests/test_haiku_fora_da_folha.py:502`; `app/docs/e3_v8_matriz.txt:17`; `app/chamados/services/tarefa_do_dia.py:559`) |
   | Mock golden: `decidir` → `validar`, mantendo fio admin | Ajuste de fixture; a expectativa final de admin continua explícita. (`app/folha/tests/test_haiku_fora_da_folha.py:532`; `:541`) |

   Portanto, não identifiquei inversão sem **referência alegada**; a autorização independente do golden por I5/aval integral e a extensão de R2 para `decidir_ausencia + responder` permanecem **[não verificado]**. (`app/folha/tests/test_haiku_fora_da_folha.py:502`; `app/chamados/services/tarefa_do_dia.py:170`; `:591`)

7. **[fila] O instrumento de sombra não compara o novo `nao_sei`.** `projecoes` enumera os campos dos consumidores, mas omite esse campo, agora lido por Folha e Vigia. **Contraexemplo:** duas portas iguais em rota/fio/fila/verbos, diferentes apenas em `nao_sei`, geram projeções iguais no comparador. Isso limita a evidência do instrumento; os testes específicos dos leitores cobrem separadamente o repasse. (`app/chamados/management/commands/diff_e3_sombra.py:46`; `:49`; `:53`; `:133`; `app/folha/fora_da_folha.py:245`; `app/chamados/services/vigia_fio.py:651`; `app/chamados/tests/test_e3_portas_tarefa.py:490`)

8. **[bloqueia] Alcance ORM e conclusão dos proibidos têm limites explícitos.** R2/R3 precisam confirmar crédito/partição reais; R8 precisa confirmar nascimento pelo escritor, dia forte e pertencimento à fila; R14 precisa confirmar integração e persistência; R16 precisa da execução da sessão. Os corpos preparados não substituem esses resultados. (`app/chamados/tests/test_e3_portas_tarefa.py:202`; `:220`; `:225`; `:267`; `:301`; `app/docs/e3_v8_matriz.txt:26`)

   Nos corpos produtivos alterados, não identifiquei juiz novo, encerramento de chamado ou supressão nova de chamado vivo: a união conserva portas residuais e apenas restringe a nomeação em `fila` à lista padrão. SC-A/R6 continua declarado fora da obra. (`app/chamados/services/tarefa_do_dia.py:575`; `:584`; `:599`; `app/docs/e3_v8_matriz.txt:16`)

   No diff textual fornecido não aparecem alterações em models/migrations/`core/juizes.py`/motor/crons/bin; confirmação de ausência de mudanças fora desse diff: **[não verificado]**.

   Reexecutei **26 testes sem banco e 7 da catraca C17, todos passando**. Isso não executa os casos ORM: o executor sem banco remove expressamente a classe correspondente. (`app/chamados/tests/e3_portas_regra.py:25`; `:27`; `:77`; `app/chamados/tests/test_e3_pendentes_app.py:118`)

## 14/11 02:24 · sessao 7f6a76a4a4f0 · - · - · origem=Ronald (tmux)

1. **Escritores e leitores do mapa.** Os cinco campos são projeção em memória; `porta_da_tarefa` copia cinco chaves da tarefa e devolve `None` quando não há rota (`app/chamados/services/tarefa_do_dia.py:603-609`).

| Campo | Escritor / fornecedor | Leitores de produção |
|---|---|---|
| `fio` | decisão final em `_portas_da_tarefa`: `app/chamados/services/tarefa_do_dia.py:588-600`; residual consulta `app/chamados/juizes.py:1645-1651` | Folha `app/folha/fora_da_folha.py:86-89,190,236`; Vigia `app/chamados/services/vigia_fio.py:658-659`; diff `app/chamados/management/commands/diff_e3_sombra.py:143` |
| `nao_sei` | inicialização `app/chamados/services/tarefa_do_dia.py:192-193`; causas `:215,224,258-262,284,288,320-323`; copiado na projeção `:609` | Folha `app/folha/fora_da_folha.py:245`; Vigia `app/chamados/services/vigia_fio.py:651-652,635`; furo `app/chamados/services/furo_sem_canal.py:98-100` |
| `rota` | `app/chamados/services/tarefa_do_dia.py:599-600` | projeção `:609`; Vigia `app/chamados/services/vigia_fio.py:657-659`; furo `app/chamados/services/furo_sem_canal.py:97`; diff `app/chamados/management/commands/diff_e3_sombra.py:143` |
| `fila` | associação célula `app/chamados/juizes.py:1567-1568`; filtro nomeável `app/chamados/services/tarefa_do_dia.py:584,600` | Folha `app/folha/fora_da_folha.py:241`; diff `app/chamados/management/commands/diff_e3_sombra.py:50,143` |
| `verbos` | partição `app/chamados/services/verbos.py:30-52`; transporte `app/chamados/juizes.py:1560,1565,1578`; recorte portas `app/chamados/services/tarefa_do_dia.py:583,600` | residual `:596`; nomeação fila `:584`; diff `app/chamados/management/commands/diff_e3_sombra.py:144` |

2. **Tela: leitores indiretos, sem segundo escritor da porta.** `fora_da_folha` lê cada fio, soma dias e escolhe grupo do colaborador (`app/folha/fora_da_folha.py:190-205`; prioridade grupo `:92-103`). `montar_linhas` consulta essa fonte e grava o grupo convertido em fio sob a chave `dono` (`app/folha/retrato_folha.py:1158,1187`; conversão `:364-368`). A tela lê o fio gravado (`:438-441`), converte-o em balde visual (`:192,474-481`) e lê a frase `DESTRAVA` (`:1069`). `nao_sei` não é transportado por esse caminho: a linha montada contém `dono`, tipo e datas, mas não esse campo (`:1187-1194`). Exibição por outros consumidores: **[não verificado]**.

3. **Unidades e `None`.** O dia da tarefa é o dia do turno (`app/chamados/services/tarefa_do_dia.py:192,228-246`); a data civil do marco permanece campo separado (`:112-113`). O vínculo sem célula usa `(colaborador_id,dia_do_turno)` e `dia_do_fato(fortes=True)` (`:577-578`; `app/chamados/juizes.py:1574-1576`). Vigia deduplica `(colab,data)` (`app/chamados/services/vigia_fio.py:591-594`); furo conta PKs de células (`app/chamados/services/furo_sem_canal.py:79-100,109`). Portanto, igualdade universal entre contagens em produção é **[não verificado]**.

   `None` é ausência de porta; `{}` não é saída válida da projeção (`app/chamados/services/tarefa_do_dia.py:604-609`). Contraexemplo: substituir a projeção por `{}` causa acesso a chave inexistente em Folha, Vigia e furo (`app/folha/fora_da_folha.py:89`; `app/chamados/services/vigia_fio.py:651`; `app/chamados/services/furo_sem_canal.py:97`). Uma porta válida com `fila=()` continua porta, por exemplo a âncora externa R4 (`app/chamados/tests/test_e3_portas_tarefa.py:477-478`, conferir numeração atual com agente da matriz).

4. **Classes do checklist, escopo produção alterada.**
   - **Segundo juiz duplicado: ausente nos corpos alterados.** A projeção copia a decisão, e C17 permanece apenas no comparador (`app/chamados/services/tarefa_do_dia.py:606-609`; `app/chamados/management/commands/diff_e3_sombra.py:127`; exceção registrada `app/core/juizes.py:132-138`).
   - **Segundo escritor persistente: ausente nos corpos alterados.** A escrita acrescentada é `out.update`, em memória (`app/chamados/services/tarefa_do_dia.py:599-600`).
   - **Relógio fora da borda: ausente na cadeia E3 injetada; presente no comportamento legado sem instante.** Cadeia atual: `alimentar_portas` guarda instante (`app/chamados/juizes.py:1619`), `_no_app` repassa (`:1632`), app entrega a C6 (`app/chamados/services/pergunta_no_app.py:61`), C6 converte o instante em dia local (`app/chamados/juizes.py:885-887`). Sem instante existem `date.today()` e `timezone.now()` internos (`app/chamados/juizes.py:592`; `app/chamados/services/verbos.py:35`; `app/chamados/services/painel.py:190`); a consulta normal do app continua omitindo `agora` (`app/chamados/services/pergunta_no_app.py:73`).
   - **Derivação paralela nos leitores alterados: ausente.** Todos leem a projeção ou, sem ela, apenas `nao_sei` da tarefa original (`app/folha/fora_da_folha.py:235-245`; `app/chamados/services/vigia_fio.py:643-659`; `app/chamados/services/furo_sem_canal.py:95-100`).
   - **Ordem de trava SQL nova: ausente nos corpos alterados.** O app usa contexto de consulta da tranca, e o diff envolve a consulta em `tranca_em_lote` (`app/chamados/services/pergunta_no_app.py:60`; `app/chamados/management/commands/diff_e3_sombra.py:100`). Efeito ORM e ausência global de persistência: **[não verificado]**.
   - **Campo com duas causas: presente, fila.** `None` no verbo pode vir da auditoria/registro (`app/chamados/catalogo/natureza.py:63,68,73`) ou da ausência do membro na partição (`app/chamados/juizes.py:1565`). Ambos são tratados como ausência de verbo observado; o residual conserva `fio=chamado` (`app/chamados/juizes.py:1647-1651`). Não há contraexemplo de erro adicional demonstrado só por essa perda de proveniência.

5. **Outros consumidores.** Pauta identifica ausência de fio pela Folha (`app/ponto/services/pautas_dos_discordantes.py:79-84`); beco testa diretamente `porta_da_tarefa(...) is not None` (`app/escala/services/cadastro_realidade.py:492-498`). Backfill aceita a nona coluna e preserva as oito anteriores por `linha[:8]` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:253`).

6. **Proibidos, limitação da prova.** Nos trechos produtivos do diff fornecido não aparece escrita em models/migrations/core/juizes/motor/crons/bin nem chamada de encerramento; os corpos alterados alimentam e projetam dados (`app/chamados/juizes.py:1549-1579`; `app/chamados/services/tarefa_do_dia.py:553-609`). Alteração fora do diff fornecido e confirmação criptográfica do hash: **[não verificado]**. A obra continua publicando chamados residuais e preservando histórico fora da lista padrão (`app/chamados/services/tarefa_do_dia.py:575-584`); R6 segue fora da obra conforme matriz (`app/docs/e3_v8_matriz.txt:16`).

## 14/11 02:24 · sessao b7b3cb0bd835 · - · - · origem=Ronald (tmux)

1. **[fila] Falta contrato normativo integral para julgar todas as combinações.** O arquivo é explicitamente histórico da volta 1 e contém status, não o contrato I1..I8 anunciado. R2 registra esperado `admin`, mas as demais prescrições de prioridade não estão presentes. Não usar a nova docstring como prova independente de autorização. `app/docs/e3_v8_matriz.txt:1`, `:5`, `:9`, `:30`.

2. **[fila] A nova prioridade é condicional à origem do dono.** Com `C=colab`, `A=admin`, ações alcançáveis e donos dos verbos residuais:

| Donos das ações | Donos residuais | Antes do diff | Hash atual |
|---|---|---|---|
| nenhum | C+A | colab | colab |
| C | A | colab | colab |
| C | C+A | colab | colab |
| A | C | colab | colab |
| A | C+A | colab | colab |
| C+A | nenhum | colab | admin |
| C+A | A | colab | admin |
| C+A | C | colab | colab |
| C+A | C+A | colab | colab |

O código remove `colab` dos donos das ações antes de acrescentar o residual, que pode recolocá-lo. A escada final volta a priorizar `colab`. Confirmei as nove saídas executando somente os corpos AST em memória. `app/chamados/services/tarefa_do_dia.py:588`, `:591`, `:596`, `:597`; `app/chamados/juizes.py:1640`, `:1645`.

Contraexemplo: `validar@101 + responder@202` devolve `admin`; acrescentar residual `303:cobrar` devolve `colab`, preservadas as duas ações. Isto corresponde à docstring nova — fonte (1), admin entre ações; fonte (3), colab ao combinar residual — mas não prova a prescrição integral da matriz. `app/chamados/services/tarefa_do_dia.py:42`, `:45`.

3. **[fila] R2 cobre apenas parte das dimensões possíveis.** O teste novo verifica duas ações humanas na mesma porta `101` e em portas distintas `101/202`, ambas ligadas à célula; em ambos os casos espera e obtém `admin`. `app/chamados/tests/test_e3_portas_tarefa.py:459`, `:463`, `:469`, `:470`.

Com `fila_cel={}` e as duas portas nas ações, o resultado é `porta_pergunta`, `fila=()`, `fio=admin`. Acrescentar residual `cobrar` fora dessas portas muda para `porta_fila`, `fio=colab`. Essas extensões não aparecem no corpo R2; alcance ORM não demonstrado. `app/chamados/services/tarefa_do_dia.py:571`, `:581`, `:599`; `app/docs/e3_v8_matriz.txt:29`.

4. **[fila] A correção identificada como R2 também muda ações administrativas diferentes de validar.** `decidir_ausencia@101 + responder@202`, ambas na fila, passa de `colab` para `admin`, porque o novo `if` olha apenas o dono. A razão R13 produz `decidir_ausencia`, mas seu teste contém apenas essa ação administrativa. Sustentação normativa específica para alterar essa combinação: **[não verificado]**. `app/chamados/services/tarefa_do_dia.py:170`, `:591`; `app/chamados/tests/test_e3_portas_tarefa.py:615`, `:629`, `:630`.

5. **[fila] Relação observável das linhas R com simultaneidade.**

- **R1:** só ação `responder`; `validar` é verbo da mesma porta, não outra ação admin. Prescreve/obtém `colab` no teste. `app/chamados/tests/test_e3_portas_tarefa.py:441`, `:444`, `:447`.
- **R2:** único corpo R contendo duas ações humanas simultâneas; prescreve/obtém `admin`, mesma porta ou portas distintas. `app/chamados/tests/test_e3_portas_tarefa.py:459`.
- **R3:** só ação admin, não prova concorrência humana. `app/chamados/tests/test_e3_portas_tarefa.py:473`.
- **R4:** pergunta encerrada, nenhuma ação humana; residual `decidir`. `app/chamados/tests/test_e3_portas_tarefa.py:479`.
- **R5:** substitui o mapa para testar leitores; não produz simultaneidade. `app/chamados/tests/test_e3_portas_tarefa.py:490`, `:497`.
- **R6:** fora da obra segundo o anexo. `app/docs/e3_v8_matriz.txt:16`.
- **R7:** membro sem verbo, sem ações humanas. `app/chamados/tests/test_e3_portas_tarefa.py:505`.
- **R8:** residual sem célula com verbo `decidir`, sem ações humanas. `app/chamados/tests/test_e3_portas_tarefa.py:518`.
- **R9:** passivo/âncora `validar`, sem ação colab simultânea. `app/chamados/tests/test_e3_portas_tarefa.py:283`, `:289`, `:537`.
- **R10:** nenhuma ação; conserva âncora. `app/chamados/tests/test_e3_portas_tarefa.py:291`, `:299`.
- **R11:** testa cadeia de relógio até C6, não concorrência de donos. `app/chamados/tests/test_e3_portas_tarefa.py:560`.
- **R12:** motivos de desconhecimento com residual `decidir`, não concorrência humana. `app/chamados/tests/test_e3_portas_tarefa.py:570`, `:586`.
- **R13:** ato desconhecido ou absorção pendente; teste administrativo isolado. `app/chamados/tests/test_e3_portas_tarefa.py:594`, `:615`.
- **R14:** paridade de universo/contagem; sem duas ações humanas. `app/chamados/tests/test_e3_portas_tarefa.py:633`, `:640`.
- **R15:** varia uma pergunta Q2..Q16 e conserva residual `101:decidir`; não põe ações colab+admin juntas e nem afirma `fio`. `app/chamados/tests/test_e3_portas_tarefa.py:651`, `:683`, `:686`.
- **R16:** execução de suíte pendente, não um caso de concorrência. `app/docs/e3_v8_matriz.txt:26`.

O teste anterior explícito `validar@101 + residual cobrar@202` permanece esperando `colab`; outro teste conserva essa expectativa com terceiro residual `decidir`. Portanto a cópia mantém contratos diferentes para admin vindo de ação e colab vindo do residual, coerentes com a docstring nova. A aprovação normativa dessa combinação além do material fornecido permanece **[não verificado]**. `app/chamados/tests/test_e3_portas_v6_sem_banco.py:32`; `app/chamados/tests/test_e3_portas_tarefa.py:429`.

## 14/11 02:23 · sessao 78ed26487d90 · - · - · origem=Ronald (tmux)

1. **Selos de C6 mordem a reversão.** Executei sem banco os três mutantes de `app/chamados/tests/e3_v8_volta2_selos.py:11`: `sem_C6`, `sem_q_vivas` e `sem_instante`; cada controle passou e cada mutante produziu uma falha de assert, zero erros. Os asserts são `app/chamados/tests/test_contract_pergunta_viva.py:81` e `:84`; o executor exige controle verde e falha real em `app/chamados/tests/e3_v8_volta2_selos.py:36` e `:46`.

2. **Golden conserva mordida por inspeção; execução ORM [nao verificado].** O chamado real nasce em `app/folha/tests/test_haiku_fora_da_folha.py:517`; as expectativas `{'chamado':1,'admin':1}` e `DESTRAVA['chamado']` em `:522` e `:523` rejeitam a volta do defeito “vivo sem pergunta não move fio”. O balde continua conferido em `:524` e `:525`; a projeção trocada posteriormente deve mover fio/frase para admin em `:535`, `:541` e `:542`. A fonte da tela passa por `montar_linhas` nos helpers `:402`, `:419` e `:432`.

3. **Expectativas alteradas e respaldo verificável:**

   | Alteração | Respaldo |
   |---|---|
   | `test_e3_portas_tarefa.py:140`: `colab_ids` no carregamento | R8, `app/docs/e3_v8_matriz.txt:18`; implementação `app/chamados/services/tarefa_do_dia.py:545` |
   | `test_o261e_f5b_juiz_porta.py:291`: None/nada → porta com verbo None/chamado | R7, `app/docs/e3_v8_matriz.txt:17`; aval (ii) alegado em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:277` |
   | `test_e3_diff_sombra.py:23`: texto do relógio | R11, `app/docs/e3_v8_matriz.txt:21`; cadeia `app/chamados/services/pergunta_no_app.py:61` |
   | `test_contract_pergunta_viva.py:81`: igualdade AST com `agora` | R11, `app/docs/e3_v8_matriz.txt:21`; L-128, `app/docs/LEIS.md:149` |
   | Golden `test_haiku_fora_da_folha.py:522`: chamado vivo sem pergunta muda fio | Aval E3-PORTA-SEM-CASO alegado em `:502`, já citado em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:163` e `:241` |
   | Mocks recebem `nao_sei`, com assert adicional em `test_haiku_fora_da_folha.py:154` | R5, `app/docs/e3_v8_matriz.txt:15`; semântica em `:201` |
   | Golden mock passa de verbo decidir para validar, mantendo fio admin em `test_haiku_fora_da_folha.py:532` | Adequação de fixture; o assert final de fio admin já permanece em `:541` |

4. **Limite documental relevante.** A matriz disponível contém status históricos R1–R16 em `app/docs/e3_v8_matriz.txt:9`, mas não contém o contrato anunciado I1–I8; I1 e I5 aparecem como referências nos comentários `app/folha/tests/test_haiku_fora_da_folha.py:149` e `:502`. O conteúdo literal original desses contratos/avais é **[nao verificado]**. Assim, nenhuma inversão de expectativa está totalmente sem justificativa alegada, mas a autorização integral não pode ser validada só pelo anexo disponível.

## 14/11 02:12 · sessao 359866b81b9d · E3-FORMA-SQL · construtor · origem=Code

R2 foi corrigida na origem. Em R14/R5, os leitores já consumiam o mapa; as falhas vinham das fixtures. [Cura e diagnóstico](docs/e3_v8_volta2.txt:16).

| Ocorrência | Classe | Cura |
|---|---|---|
| 1. KeyError porta_pergunta | II | [Fake com cinco chaves](docs/e3_v8_volta2.txt:11) |
| 2. KeyError porta_fila | II | [Fake com cinco chaves](docs/e3_v8_volta2.txt:12) |
| 3. KeyError historico | II | [Fake com cinco chaves](docs/e3_v8_volta2.txt:13) |
| 4. KeyError auditoria | II | [Fake com cinco chaves](docs/e3_v8_volta2.txt:14) |
| 5. R14 ORM | II | [Mesma competência e universo do export](docs/e3_v8_volta2.txt:15) |
| 6. R2 | I | [Admin precede entre ações alcançáveis](docs/e3_v8_volta2.txt:16) |
| 7. Golden | II | [Expectativa corrigida por R7/I5](docs/e3_v8_volta2.txt:17) |
| 8. Endpoint | III | [Selo AST preservando C6 e instante](docs/e3_v8_volta2.txt:18) |
| 9. Ruff | I | [Cinco achados corrigidos na origem](docs/e3_v8_volta2.txt:19) |
| 10. R14 puro | II | [Janela do fornecedor corrigida](docs/e3_v8_volta2.txt:20) |
| 11. R5 puro | II | [Mesmo defeito de janela](docs/e3_v8_volta2.txt:21) |

A entrega inclui [arquivos tocados](docs/e3_v8_volta2.txt:45), [quem mais lê cada função](docs/e3_v8_volta2.txt:66) e [mutantes novos com comandos individuais](docs/e3_v8_volta2.txt:125).

Localmente: [RED com quatro falhas por valor](docs/e3_v8_volta2_red.txt:107), [26 testes GREEN](docs/e3_v8_volta2_green.txt:29), [70 regressões OK](docs/e3_v8_volta2_regressoes.txt:3) e [resultados dos mutantes](docs/e3_v8_volta2.txt:111).

**Incompleto:** sete ocorrências exigem rerodada ORM; Ruff está indisponível. Esses resultados e a certificação seguem **[nao verificado]**. [Pendências e comando do recorte](docs/e3_v8_volta2.txt:143).

## 14/11 01:42 · sessao 7ad1129d1278 · CORTE-DAS-TRAVAS · verificador · origem=Code

**REPROVADO.** A cura melhora os casos apresentados, mas ainda permite escrita classificada como `so leitor` e pendências apagadas como zero. (`bin/classe_da_obra.py:144`, `bin/classe_da_obra.py:175`, `bin/disjuntor.py:1025`, `bin/disjuntor.py:1031`)

Os contraexemplos abaixo são sintéticos, exercitados em memória sobre o código da cópia; ocorrência em produção `[nao verificado]`. Execução integral dos selos `[nao verificado]`: os scripts criam e removem arquivos, contrariando a restrição desta verificação. (`bin/tests/test_triagem_pendente.sh:12`, `bin/tests/test_gate_pela_classe.sh:27`)

1. **[bloqueia] `bin/classe_da_obra.py:175` — controle de formulário gerado por template continua “só marcação”.**

   A regex examina a linha alterada, sem reconhecer tags Django que produzem controles. Contraexemplo sobre arquivo existente: trocar `{% opcao_mover "fechado" %}` por `{% opcao_mover "resolvido" %}` em `app/templates/chamados/detalhe.html:134` resulta em `so leitor`. A tag gera `<option value=...>`, dentro do campo `novo_status`; o backend lê esse valor e o entrega à transição. Portanto, a alteração muda uma entrada de escrita, embora escape da cura. (`app/chamados/templatetags/chamados_tags.py:77`, `app/templates/chamados/detalhe.html:130`, `app/chamados/services/acoes_chamado.py:296`, `app/chamados/services/acoes_chamado.py:325`, `bin/classe_da_obra.py:215`)

2. **[bloqueia] `bin/classe_da_obra.py:144` — conteúdo do diff confundido com cabeçalho.**

   Contraexemplo: dentro de `<script>`, trocar `--saldo;` por `++saldo;`. As linhas do diff são `---saldo;` e `+++saldo;`; ambas são descartadas como cabeçalho. Nenhuma chega à verificação de faixa de script, e a classe sai `so leitor`. O descarte também deixa de avançar a numeração dessas linhas. (`bin/classe_da_obra.py:144`, `bin/classe_da_obra.py:149`, `bin/classe_da_obra.py:153`, `bin/classe_da_obra.py:174`, `bin/classe_da_obra.py:181`, `bin/classe_da_obra.py:215`)

3. **[bloqueia] `bin/disjuntor.py:1025` — retrocesso de relógio vira zero silencioso.**

   Contraexemplo: `ini=10:00:00`, seguido no arquivo por `fim=09:59:59`, rc `0`, duração `0`, sem triagem. O `pop` remove a ocorrência, a comparação rejeita o par, e a CLI devolve rc `0`, em vez de `None`/rc `4`. O algoritmo anterior ainda apontaria pendência; aqui há afrouxamento do delta. (`bin/disjuntor.py:1024`, `bin/disjuntor.py:1025`, `bin/disjuntor.py:1045`, `bin/disjuntor.py:1052`)

   Duração zero não impede esse histórico de ser produzido: `T0C` é capturado antes do `ini`. Se `T0C=09:59:59`, o `ini` cruza para `10:00:00` e o relógio recua para `09:59:59`, o trap pode escrever fim anterior e duração zero, aceita pelo parser. Produção desse cenário `[nao verificado]`. (`bin/codex_pergunta.sh:265`, `bin/codex_pergunta.sh:305`, `bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:273`, `bin/codex_limite_estado.py:69`)

4. **[bloqueia] `bin/disjuntor.py:1031` — uma triagem pode quitar duas ocorrências.**

   Contraexemplo, mesma obra/hash/pid: ocorrência A em `[10:00,10:05]`, ocorrência B em `[10:05,10:10]`; apenas A está triada, com fim `10:05`. O intervalo inclusivo aceita esse mesmo instante para ambas e devolve zero. A linha não é consumida nem vinculada univocamente à ocorrência. Com relógio recuado e intervalos sobrepostos, o problema também ocorre longe da borda. (`bin/disjuntor.py:1011`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:1052`)

   Há ainda aceitação de instante inválido: terceira parte `2026-11-12T10:03:XX` satisfaz textualmente um intervalo `10:00..10:05`. `_triagens` valida a estrutura externa, mas não esse instante; o vigia corta e compara a string sem parse. O escritor normal gera formato válido; corrupção real `[nao verificado]`. (`bin/disjuntor.py:974`, `bin/disjuntor.py:1011`, `bin/disjuntor.py:1031`, `bin/codex_pergunta.sh:464`)

O rastreio dos campos responde à pergunta 1:

| Campo/valor | Quem escreve | Quem lê e significado |
|---|---|---|
| `ini/fim`, obra, papel, hash12, pid, sessão, rc, duração | Wrapper: `bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:305` | Parser único: `bin/codex_limite_estado.py:63`; chave sem papel em `bin/codex_limite_estado.py:74`. |
| `eventos` | Lista local preenchida pelo parser: `bin/codex_limite_estado.py:78`, `bin/codex_limite_estado.py:82` | Pareamento por ordem do arquivo: `bin/disjuntor.py:1020`. Não é uma nova trilha persistida. |
| “fim legível” | Predicado local: hora parseável, quebra final, tipo, campos mínimos, rc inteiro e duração com dígitos. `bin/codex_limite_estado.py:68` | Alimenta `ilegiveis`; o vigia transforma lista não vazia em `None`. `bin/codex_limite_estado.py:70`, `bin/disjuntor.py:1017`. |
| Fim do sidecar | Wrapper antes do EXIT/trap: `bin/codex_pergunta.sh:464`, `bin/codex_pergunta.sh:472`, `bin/codex_pergunta.sh:282` | `_le_id` e composição da volta: `bin/disjuntor.py:886`, `bin/disjuntor.py:928`. |
| Terceiro campo da volta, `hash12@pid@fim@sha12` | Composto e gravado por `--triagem`: `bin/disjuntor.py:928`, `bin/disjuntor.py:939` | B3 conserva a volta completa como identidade; vigia interpreta o terceiro componente como instante pertencente a intervalo. `bin/disjuntor.py:976`, `bin/disjuntor.py:985`, `bin/disjuntor.py:1011`, `bin/disjuntor.py:1031`. |
| Hora da própria linha de triagem | `datetime.now()`: `bin/disjuntor.py:939` | `_triagens` não usa esse primeiro campo para casar ocorrências. `bin/disjuntor.py:973`, `bin/disjuntor.py:976`. |
| Classe do template/obra | Decisão por arquivo e máximo pela precedência: `bin/classe_da_obra.py:212`, `bin/classe_da_obra.py:240` | Publicada pela CLI, consultada pelo portão e pelos testes. `bin/classe_da_obra.py:334`, `bin/classe_da_obra.py:277`, `bin/tests/test_gate_pela_classe.sh:70`. |

**Os significados não estão inteiramente alinhados.** As horas escritas são instantes com precisão de segundos, sem offset; o parser produz `datetime` sem fuso, enquanto o vigia compara strings. Uniformidade do fuso entre execuções `[nao verificado]`. O corte `desde` inclui igualdade, e o casamento da triagem inclui ambas as bordas. A identidade completa usada pelo B3 não equivale ao casamento temporal usado pelo vigia, como demonstra o achado 4. (`bin/codex_pergunta.sh:272`, `bin/codex_pergunta.sh:305`, `bin/codex_pergunta.sh:464`, `bin/codex_limite_estado.py:65`, `bin/disjuntor.py:1029`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:985`)

Quanto aos históricos pedidos na pergunta 3:

| Histórico construído | Resultado do código |
|---|---|
| `ini` sem `fim` | Não conta: pertence ao universo ainda não concluído. `bin/disjuntor.py:990`, `bin/disjuntor.py:1028`. |
| Dois `ini` consecutivos da mesma chave, depois `fim` | O segundo substitui o primeiro e recebe o fim. `bin/disjuntor.py:1022`, `bin/disjuntor.py:1024`. |
| `fim` de outra chave entre `ini` e `fim` | Não consome a primeira chave. `bin/disjuntor.py:1024`. |
| `fim` anterior ao `ini` | Descarta o par e pode retornar zero: achado 3. `bin/disjuntor.py:1025`, `bin/disjuntor.py:1052`. |

**O ramo template não afrouxa em execução normal:** o retorno anterior era leitor; agora é leitor ou escritor, e a agregação escolhe o máximo. Isso não elimina os falsos leitores dos achados 1–2. (`bin/classe_da_obra.py:214`, `bin/classe_da_obra.py:215`, `bin/classe_da_obra.py:240`)

5. **[fila] `bin/classe_da_obra.py:350` — existe afrouxamento excepcional para obra combinada com dinheiro.**

   Contraexemplo com falha Git injetada em memória: a obra contém arquivo `app/folha/...`, já classificado como dinheiro, e template; a nova leitura do diff desse template lança `Erro`. A CLI devolve `escritor`, rc `3`; com o retorno anterior do ramo template, devolve `dinheiro`, rc `0`. É consequência da política existente “erro = escritor”, agora alcançável por uma operação adicional do delta. Falha equivalente em produção `[nao verificado]`. (`bin/classe_da_obra.py:187`, `bin/classe_da_obra.py:137`, `bin/classe_da_obra.py:75`, `bin/classe_da_obra.py:352`, `bin/classe_da_obra.py:353`)

**A exigência numérica não muda os números dos outros leitores, na pergunta 4.** `legivel` somente alimenta `ilegiveis`; os ramos que montam `inis/fins` continuam sem consultar esse predicado. Permanecem iguais `ler_chamadas`, `ultimas_chamadas`, `sessoes_do_code`, `tokens_por_papel` e `_censo_24h`. As derivações de origem e tokens também recebem os mesmos retornos legados. (`bin/codex_limite_estado.py:70`, `bin/codex_limite_estado.py:75`, `bin/codex_limite_estado.py:79`, `bin/codex_limite_estado.py:91`, `bin/codex_limite_estado.py:99`, `bin/codex_limite_estado.py:109`, `bin/codex_limite_estado.py:204`, `bin/disjuntor.py:2254`, `bin/gerar_codex_estado.py:176`, `bin/disjuntor.py:2156`)

6. **[fila] `bin/codex_limite_estado.py:69` — duração emitida pode ser considerada ilegível.**

   O escritor calcula duração por subtração de relógio civil; um recuo pode produzir duração negativa, rejeitada por `isdigit()`. Nesse caso o vigia devolve `None`, conservadoramente; não é o falso zero do achado 3. (`bin/codex_pergunta.sh:265`, `bin/codex_pergunta.sh:273`, `bin/disjuntor.py:1017`)

As mordidas dos casos novos, na pergunta 5:

| Caso novo | Mutante da fonte que a asserção elimina |
|---|---|
| Marcação fora do script | Classificar como escritor qualquer template que contenha script, ignorando a posição da linha. `bin/tests/test_gate_pela_classe.sh:139`, `bin/classe_da_obra.py:179`. |
| Divisor dentro do script | Remover as faixas de script; M8 exercita explicitamente esse mutante. `bin/tests/test_gate_pela_classe.sh:140`, `bin/tests/test_gate_pela_classe.sh:143`, `bin/tests/test_gate_pela_classe.sh:144`. |
| Campo `name=` | Desativar `FORM.search`; M9 exercita explicitamente esse mutante. `bin/tests/test_gate_pela_classe.sh:141`, `bin/tests/test_gate_pela_classe.sh:145`, `bin/tests/test_gate_pela_classe.sh:146`. |
| Static `.js` | Desativar o ramo incondicional `.js`. `bin/tests/test_gate_pela_classe.sh:142`, `bin/classe_da_obra.py:171`. |
| PID reutilizado, exatamente uma pendência nova | Restaurar o casamento pelo último `fins[chave]`: produziria duas pendências. `bin/tests/test_triagem_pendente.sh:61`, `bin/codex_limite_estado.py:80`. |
| Inverso: nova triada, velha pendente | Mesmo mutante anterior: produziria zero. `bin/tests/test_triagem_pendente.sh:68`. |
| Sidecar um segundo antes | Trocar pertinência ao intervalo por igualdade dos fins. `bin/tests/test_triagem_pendente.sh:76`, `bin/disjuntor.py:1031`. |
| Rc/duração vazios; rc `abc` | Remover a exigência numérica: deixaria de retornar rc `4` nesses três casos. `bin/tests/test_triagem_pendente.sh:79`, `bin/tests/test_triagem_pendente.sh:83`, `bin/codex_limite_estado.py:69`. |

7. **[fila] `bin/tests/test_gate_pela_classe.sh:139` — há mutantes que passam em falso.**

   Trocar o `yield` do lado removido por `pass` preserva os quatro resultados novos, mas faz a remoção isolada de `<input name="valor">` sair `so leitor`. Os novos fixtures usam substituições, cujo lado adicionado ainda denuncia script/formulário. Tampouco exercitam tags Django ou a colisão `---`/`+++` dos bloqueios acima. (`bin/classe_da_obra.py:148`, `bin/tests/test_gate_pela_classe.sh:61`, `bin/tests/test_gate_pela_classe.sh:62`, `bin/tests/test_gate_pela_classe.sh:63`, `bin/tests/test_gate_pela_classe.sh:64`)

8. **[fila] `bin/tests/test_triagem_pendente.sh:79` — as novas asserções não fecham as classes de falha.**

   Sobrevivem: trocar `p[8].isdigit()` por `bool(p[8])`, aceitando duração `abc`; usar chave global no pareamento, pois os fixtures são seriais; usar `setdefault` para conservar o primeiro de dois `ini`; comparar horas só até minuto. Os casos novos não cobrem duração não numérica preenchida, intercalação de chaves, dois inícios consecutivos, retrocesso ou compartilhamento de borda. (`bin/codex_limite_estado.py:69`, `bin/disjuntor.py:1022`, `bin/disjuntor.py:1024`, `bin/disjuntor.py:1031`, `bin/tests/test_triagem_pendente.sh:18`, `bin/tests/test_triagem_pendente.sh:55`, `bin/tests/test_triagem_pendente.sh:79`)

9. **[fila] `bin/pre-push.sh:147` — grep corrigido, sem mordida correspondente nos casos novos.**

   O emissor escreve no stderr; `_teste_irmao` reúne as saídas e agora aceita `.CATRACA-AVISO:`. Reintroduzir a âncora não afetaria os novos casos de classificação/triagem. O filtro continua apenas imprimindo: o retorno é `_rc`. Destino do aviso e truncamento por `tail -12` permanecem na fila anterior. (`app/core/tests/test_medida_turno_do_par.py:467`, `bin/pre-push.sh:145`, `bin/pre-push.sh:147`, `bin/pre-push.sh:157`, `bin/tests/test_gate_pela_classe.sh:139`, `bin/tests/test_triagem_pendente.sh:59`)

No checklist da pergunta 6:

| Classe de reprovação | Resultado neste recorte |
|---|---|
| Segundo juiz duplicado | O delta continua usando o leitor único de chamadas e `_triagens`; a divergência nova é o significado de identidade versus intervalo, bloqueio 4. `bin/disjuntor.py:1003`, `bin/disjuntor.py:1014`, `bin/disjuntor.py:1031`. |
| Segundo escritor fora da porta | `eventos` é local; a escrita persistida continua em `--triagem`, sob a porta existente. `bin/codex_limite_estado.py:78`, `bin/disjuntor.py:938`, `bin/disjuntor.py:944`. |
| Relógio fora da borda | O problema demonstrado é o uso dos instantes persistidos sem tratamento suficiente de recuo/ambiguidade: bloqueios 3–4. `bin/disjuntor.py:1025`, `bin/disjuntor.py:1031`. |
| Derivação paralela de fonte única | Não nasce outro parser de chamadas; nasce uma associação temporal incapaz de garantir uma triagem por ocorrência. `bin/disjuntor.py:1014`, `bin/disjuntor.py:1031`. |
| Selo/RED sem mordida suficiente | Lacunas concretas nos achados 7–9. `bin/tests/test_gate_pela_classe.sh:139`, `bin/tests/test_triagem_pendente.sh:79`, `bin/pre-push.sh:147`. |
| Ordem de trava | A porta existente adquire a trava da resposta antes da triagem e usa a trava do estado para ler/acrescentar. `bin/disjuntor.py:912`, `bin/disjuntor.py:915`, `bin/disjuntor.py:944`. |
| Pouso antes do pré-requisito | O portão continua exigindo tree certificada; a insuficiência de `_limpa` permanece fila anterior. `bin/classe_da_obra.py:279`, `bin/classe_da_obra.py:301`, `bin/classe_da_obra.py:340`. |
| Campo com dois significados | Confirmado no terceiro componente da volta: identidade integral no B3, pertinência temporal no vigia. `bin/disjuntor.py:985`, `bin/disjuntor.py:1011`, `bin/disjuntor.py:1031`. |

A fila anterior não foi promovida a bloqueio novo: autoria de `ERRO-BOBO`, exclusões por caminho, ignorados em `_limpa`, mutantes combinados M2/M3/M4, vigia com `{}` e texto de LEIS continuam nos respectivos pontos. (`bin/disjuntor.py:772`, `bin/classe_da_obra.py:186`, `bin/classe_da_obra.py:301`, `bin/tests/test_gate_pela_classe.sh:120`, `bin/disjuntor.py:1003`, `app/docs/LEIS.md:66`)

**Veredito final: REPROVADO.** (`bin/classe_da_obra.py:144`, `bin/classe_da_obra.py:175`, `bin/disjuntor.py:1025`, `bin/disjuntor.py:1031`)

## 14/11 01:39 · sessao 3bde72701314 · - · - · origem=Ronald (tmux)

Achados no delta:

- **[bloqueia] `bin/disjuntor.py:1024-1026` — fim anterior ao ini desaparece como zero.** `pop` consome a ocorrência; a condição descarta o par e `[]` vira rc 0 em `bin/disjuntor.py:1052`. Contraexemplo sintético confirmado em memória: ini `10:00:00`, fim rc 0/dur 0 `09:59:59`, trilha vazia → `[]`. Esse formato pode sair do escritor: `T0C` é capturado antes do ini (`bin/codex_pergunta.sh:265,305`); T0C `09:59:59`, ini `10:00:00`, relógio recua, fim e leitura de epoch `09:59:59` produzem duração 0 (`bin/codex_pergunta.sh:272-273`). Produção desse cenário: `[nao verificado]`. O algoritmo anterior contaria pendência; o delta afrouxa esse caso.

- **[bloqueia] `bin/disjuntor.py:1031` — uma triagem quita várias ocorrências.** `any` não consome nem vincula univocamente a linha, e os intervalos são inclusivos. Contraexemplo sintético confirmado: mesma chave, ocorrências `10:00..10:05` e `10:05..10:10`; somente primeira triada em `10:05` → `[]`. Com recuo de relógio, primeira `10:00..10:05`, segunda `10:03..10:06`, única triagem `10:04:59` também quita ambas. O escritor conserva identidade completa `hash@pid@fim@sha` (`bin/disjuntor.py:928`); o vigia reduz a chave e aceita pertença temporal (`bin/disjuntor.py:1011,1031`).

- **[bloqueia ou agrupável ao anterior] `bin/disjuntor.py:1011,1031` — instante não é validado antes da comparação.** `_triagens` aceita qualquer identificador não vazio (`bin/disjuntor.py:974-976`). Contraexemplo sintético: terceira parte `2026-11-12T10:03:XX`, ini `10:00`, fim `10:05` → considerada triada pela comparação textual. Antes, a igualdade com o fim normal não casava. O escritor normal produz formato válido (`bin/codex_pergunta.sh:464`); corrupção em produção `[nao verificado]`.

Comportamentos examinados:

| Histórico sintético | Resultado | Referência |
|---|---|---|
| Ini sem fim | `[]`, coerente com universo concluído | `bin/disjuntor.py:990,1019-1033` |
| Dois ini consecutivos da mesma chave, depois fim | Somente o segundo pareia | `bin/disjuntor.py:1022-1026` |
| Fim de outra chave entre ini e fim | Não consome a ocorrência da primeira chave | `bin/disjuntor.py:1024` |
| Fim anterior ao ini | `[]`, problema bloqueante acima | `bin/disjuntor.py:1025` |

Escritores e leitores:

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| Ini/fim, chave, rc, duração | `bin/codex_pergunta.sh:272-273,305-306` | `bin/codex_limite_estado.py:63-82`; eventos por ordem do arquivo, `bin/disjuntor.py:1020-1026` |
| Fim do sidecar | `bin/codex_pergunta.sh:464-466` | `_le_id`, `bin/disjuntor.py:879-886`; compõe volta, `bin/disjuntor.py:928` |
| Terceira parte da volta | `bin/disjuntor.py:928,939-940` | Identidade completa no B3, `bin/disjuntor.py:976,984-985`; intervalo inclusivo no vigia, `bin/disjuntor.py:1011,1031` |
| Hora da própria triagem | `bin/disjuntor.py:939` | `_triagens` não valida nem usa esse primeiro campo temporal, `bin/disjuntor.py:973-976` |

Os escritores usam hora local textual sem offset (`bin/codex_pergunta.sh:272,305,464`; `bin/disjuntor.py:939`); o parser cria `datetime` sem fuso (`bin/codex_limite_estado.py:65`) e o vigia compara strings (`bin/disjuntor.py:1025,1029,1031`). Uniformidade de fuso entre execuções em produção: `[nao verificado]`.

Selos novos:

| Caso | Mutante que a asserção reprova | Mutante relevante que sobrevive |
|---|---|---|
| PID reutilizado, contagem exata | Algoritmo anterior com último `fins[chave]`: produziria duas pendências | Pareamento com chave global fixa: os históricos da suíte são seriais | [nao verificado]
| Inverso, nova triada/velha pendente | Mesmo algoritmo anterior: produziria zero | Mesmo mutante de chave global |
| Sidecar 1 s antes | Igualdade exata com fim do chamadas.log | Comparar instantes somente até minuto; a suíte continua verde | [nao verificado]
| Rc/duração vazios; rc `abc` | Remover as novas condições numéricas | Duração `.isdigit()` → `bool(p[8])`: nenhuma fixture testa duração não vazia e não numérica |

Referências das asserções: `bin/tests/test_triagem_pendente.sh:59-83`; fixtures seriais: `bin/tests/test_triagem_pendente.sh:18-28,55-56,81`. A suíte não executa explicitamente mutantes da triagem (`bin/tests/test_triagem_pendente.sh:59-83`). Outro sobrevivente é trocar a atribuição por `aberto.setdefault(...)`: passa os casos atuais, mas pareia o primeiro de dois ini consecutivos (`bin/disjuntor.py:1022`).

**Veredito deste recorte: REPROVADO**, pelos falsos zeros em `bin/disjuntor.py:1025,1031`.

## 14/11 01:39 · sessao abc6b9967f16 · - · - · origem=Ronald (tmux)

[bloqueia] `bin/classe_da_obra.py:175` e `bin/classe_da_obra.py:181`: detectar tokens na linha alterada não prova “só marcação”. Contraexemplo real, mutação hipotética em `app/templates/chamados/detalhe.html:134`: `{% opcao_mover "fechado" %}` → `{% opcao_mover "resolvido" %}` retorna `so leitor`, reproduzido em memória. A tag escreve `<option value=status_local>` em `app/chamados/templatetags/chamados_tags.py:77`; o campo `novo_status` está em `app/templates/chamados/detalhe.html:130`, é lido em `app/chamados/services/acoes_chamado.py:296` e determina a transição em `app/chamados/services/acoes_chamado.py:325`. A cura não fecha a classe de controles gerados por template.

[bloqueia] `bin/classe_da_obra.py:144`: o leitor confunde conteúdo do diff com cabeçalho. Contraexemplo sintético reproduzido em memória: dentro de `<script>`, substituir `--saldo;` por `++saldo;` produz linhas `---saldo;` e `+++saldo;`; ambas são descartadas. `_linhas_numeradas` entrega `[]`, `_template_escreve` devolve `None` em `bin/classe_da_obra.py:181` e a classe sai `so leitor` em `bin/classe_da_obra.py:215`. O descarte também deixa de incrementar os contadores usados em `bin/classe_da_obra.py:149` e `bin/classe_da_obra.py:153`.

[fila] `bin/tests/test_gate_pela_classe.sh:139`: os quatro casos novos não cobrem remoção nem demonstram leitura dos dois lados. Mutante concreto: trocar `yield '-', nb, linha[1:].strip()` por `pass` em `bin/classe_da_obra.py:148`. Em memória, ele conserva os quatro resultados novos e deixa a remoção isolada de `<input name="valor">` sair `so leitor`; HEAD devolve `escritor`. Os fixtures novos apenas substituem conteúdo em `bin/tests/test_gate_pela_classe.sh:61`.

Mapa das mordidas novas:

| Caso | Mutante que deve reprovar | Evidência |
|---|---|---|
| Marcação fora do script | Rejeitar qualquer arquivo que contenha script | `bin/tests/test_gate_pela_classe.sh:139`; seleção por linha em `bin/classe_da_obra.py:179` |
| Divisor dentro do script | Remover os intervalos de script | Mutante M8 em `bin/tests/test_gate_pela_classe.sh:143` e conferência em `bin/tests/test_gate_pela_classe.sh:144` |
| Campo `name=` | Desativar `FORM.search` | Mutante M9 em `bin/tests/test_gate_pela_classe.sh:145` e conferência em `bin/tests/test_gate_pela_classe.sh:146` |
| Static `.js` | Desativar o ramo `.js` | Caso em `bin/tests/test_gate_pela_classe.sh:142`; ramo em `bin/classe_da_obra.py:171`; mutante incorporado para esse ramo `[nao verificado]` |

O delta do ramo template é monotônico quando termina normalmente: antes retornava leitor; agora retorna leitor ou escritor em `bin/classe_da_obra.py:212`, `bin/classe_da_obra.py:214` e `bin/classe_da_obra.py:215`; a agregação escolhe a maior classe em `bin/classe_da_obra.py:240`. Afrouxamento normal causado por esse delta `[nao verificado]`.

A classe é calculada em `bin/classe_da_obra.py:238`, agregada em `bin/classe_da_obra.py:241` e publicada no stdout em `bin/classe_da_obra.py:334`. O portão consulta a classificação em `bin/classe_da_obra.py:277`; o selo lê o stdout, sem preservar o rc do Python, em `bin/tests/test_gate_pela_classe.sh:70`. O próprio instrumento declara não estar conectado à conduta em `bin/classe_da_obra.py:37`.

**Veredito deste recorte: REPROVADO**, pelos dois escapes do ramo novo em `bin/classe_da_obra.py:144` e `bin/classe_da_obra.py:175`.

## 14/11 01:38 · sessao 8f71c0724121 · - · - · origem=Ronald (tmux)

Q4: **nenhum número dos consumidores antigos muda por causa do novo teste numérico**. `legivel` só acrescenta à lista opcional `ilegiveis`; a montagem de `inis/fins` continua aceitando os mesmos registros que antes (`bin/codex_limite_estado.py:68-82`). Consumidores:

| Consumidor | Leitura | Efeito do delta |
|---|---|---|
| `ler_chamadas` | `bin/codex_limite_estado.py:91` | Nenhum; conta `ini` (`:92-93`). |
| `ultimas_chamadas` | `bin/codex_limite_estado.py:99` | Nenhum; mantém último fim por chave (`:102-103`). |
| `sessoes_do_code` | `bin/codex_limite_estado.py:109` | Nenhum; alimenta origem em `bin/gerar_codex_estado.py:176`. |
| `tokens_por_papel` | `bin/codex_limite_estado.py:204` | Nenhum; alimenta `bin/disjuntor.py:2156`. |
| `_censo_24h` | `bin/disjuntor.py:2254` | Nenhum; usa apenas `ini` (`:2259-2264`). |
| `triagem_pendente` | `bin/disjuntor.py:1014` | Novo consumidor de `eventos`; retorna `None` quando `ilegiveis` não vazia (`:1017-1018`). |

Confirmei em Python `-B`, inteiramente em memória, que os retornos legados permanecem iguais para rc/duração válidos, vazios, rc `abc` e duração `-1`; a distinção nova aparece somente na lista `ilegiveis` (`bin/codex_limite_estado.py:68-82`). Ocorrência desses valores em produção: `[nao verificado]`.

**Escritor versus leitor.** O wrapper escreve rc a partir de `$?`, duração como `date +%s - T0C`, e hora com `date '+%Y-%m-%d %H:%M:%S'` (`bin/codex_pergunta.sh:265-273`); escreve o início no mesmo formato (`:305-306`). O leitor converte a hora para `datetime` sem fuso, conserva a string original e mantém rc/duração como strings (`bin/codex_limite_estado.py:65-80`). O fuso real dos processos é `[nao verificado]`; não há offset nos campos escritos (`bin/codex_pergunta.sh:272`, `:305`).

**[fila] `bin/codex_limite_estado.py:69` — duração produzida pelo escritor pode ser considerada ilegível.** Um retrocesso do relógio pode tornar negativa a subtração feita pelo wrapper (`bin/codex_pergunta.sh:265`, `:273`); `'-1'.isdigit()` rejeita esse fim, levando o vigia a `None` (`bin/disjuntor.py:1017-1018`). É conservador, sem zero silencioso; ocorrência real `[nao verificado]`.

**Selos novos da triagem:**

| Caso novo | Mutante que deve reprovar |
|---|---|
| Somente nova ocorrência pendente, exatamente uma linha (`bin/tests/test_triagem_pendente.sh:59-62`) | Restaurar uso do último `fins[chave]`: atribui o fim novo às duas ocorrências, produzindo duas pendências. O armazenamento do último fim está em `bin/codex_limite_estado.py:80`. |
| Inverso: nova triada, velha pendente (`bin/tests/test_triagem_pendente.sh:64-69`) | Mesmo mutante: a triagem do fim novo cobre as duas entradas, produzindo zero; o código atual pareia ocorrências em `bin/disjuntor.py:1019-1027`. |
| Sidecar um segundo antes do trap (`bin/tests/test_triagem_pendente.sh:72-76`) | Trocar pertinência ao intervalo por igualdade entre os fins, em `bin/disjuntor.py:1032`. |
| rc/duração vazios e rc `abc` (`bin/tests/test_triagem_pendente.sh:79-83`) | Remover a validação numérica de `bin/codex_limite_estado.py:69`; deixa de retornar rc 4. |

**[fila] `bin/tests/test_triagem_pendente.sh:79` — falta duração não numérica preenchida.** Mutante sobrevivente concreto: trocar `p[8].isdigit()` por `bool(p[8])` em `bin/codex_limite_estado.py:69`. Os três casos novos continuam ilegíveis, mas `fim ... sessX 0 abc` passa como legível. Confirmei essa sobrevivência do predicado em memória; o caso ausente é visível na lista de fixtures (`bin/tests/test_triagem_pendente.sh:79`).

**[fila] `bin/pre-push.sh:147` — correção do grep sem selo novo correspondente.** O emissor escreve `CATRACA-AVISO:` no stderr (`app/core/tests/test_medida_turno_do_par.py:465-467`), e o consumidor une as saídas e agora reconhece o aviso com prefixo de progresso (`bin/pre-push.sh:145-147`). Reintroduzir `^CATRACA-AVISO:` não afeta os novos casos dos dois testes do delta, que exercitam classificação e triagem, sem chamar `_teste_irmao` (`bin/tests/test_gate_pela_classe.sh:139-146`; `bin/tests/test_triagem_pendente.sh:59-83`). O limite `tail -12` continua podendo omitir avisos anteriores; já existia no delta fornecido, portanto permanece fila (`bin/pre-push.sh:147`). O retorno continua sendo o rc da execução, sem novo julgamento pelo filtro (`:157`).

_4928 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
