# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 00:44** · publicado em **15/11/2026 00:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 00:44 · sessao db1f8fd7598c · - · - · origem=Ronald (tmux)

Textos escritos que sustentam casos de especificação, sem usar o código como gabarito: a L-110 exige “cadastro tal, batidas tais” e manda a pergunta sem resposta pela regra para a lista separada (`app/docs/LEIS.md:136`).

| Transição do mapa | Caso escrito pela regra → esperado | Texto da regra |
|---|---|---|
| N1 + P1 | Cadastro com intervalo fixo; entrada, retorno do intervalo e saída registrados, saída para intervalo ausente; pergunta inexistente → emitir UMA pergunta do marco apagado, a partir do sinal da célula. | “lampada apagada no miolo passa a gerar UMA pergunta […] nascida do sinal da celula (nunca de varredura)” (`app/docs/CORTES.json:772`). |
| N1 — selfie | Cadastro de colaborador com batida de aparelho APK sem foto; prazo de 90 minutos vencido → cobrança abre disputa; em PWA, ausência de foto no ato → cobrança abre disputa. | “foto ausente e COBRANCA (abre disputa […]): […] prazo do CANAL (PWA = no ato; APK = 90 min)” (`app/docs/CORTES.md:94`). |
| P2 — dedup | Mesmo cadastro, dia, marco faltante e pergunta do mesmo fato; sinal repetido → uma pergunta desse fato, sem duplicação. O texto não decide a dona fechada nem autoriza reapontamento arbitrário. | “UMA pergunta” e “dedup pela chave do fato” (`app/docs/CORTES.json:772`). |
| F1 | Cadastro com dia já resolvido; perguntas do questionário sem viva e nenhuma resposta sem veredito → disputa elegível a fechar; resposta sem veredito → fechamento recusado. | “fecha quando `pergunta_viva(disputa)` esta vazio (C6), e a porta […] recusa resposta sem veredito” (`app/docs/BACKLOG.md:1549`). O motivo persistido `vazia` para zero perguntas não recebe gabarito específico nesse texto. |
| F1 — recusa por idade | Cadastro e batidas ainda deixando fato sem solução; pergunta muda há 30 dias → idade não absolve nem fecha; pergunta parada vai à lista administrativa. | “silencio de 30 dias e QUALQUER fechamento por idade MORREM como absolvicao — pergunta parada vira lista do admin, nao fechamento” (`app/docs/BACKLOG.md:1549`). |
| F2 + P10 no desligamento | Cadastro desligado com pergunta já respondida sem veredito → encerrar também essa pergunta por via própria de desligamento e listar dia/pergunta/resposta no chamado de revisão; muda também recebe via própria. | O corte antigo de manter respondida viva foi substituído (`app/docs/PROMPTS.md:2222`); novo conteúdo registrado em `app/docs/BACKLOG.md:732`; “via propria desligamento tambem para as mudas” (`app/docs/CORTES.json:1610`). Não declarar a cura implementada com base nessa regra. |
| F3 — respondida | Cadastro ativo, pergunta respondida sem decisão do dia; mero encerramento administrativo da disputa → a regra escrita de 28/10 rejeita tratá-la como decisão humana que não ocorreu. A extensão exata da revogação de 13/11 ao `admin_encerra`, fora de desligamento, fica [nao verificado]. | “encerrar a disputa […] nao decide o dia” e proibição de `admin_decidiu` em já respondida (`app/docs/CORTES.json:1258`); o corte sucessor trata desligamento (`app/docs/BACKLOG.md:732`). |
| F4 / tranca por leitura | Cadastro com dia em competência trancada, sem toque humano posterior → C6 lê a tranca sem novo carimbo; destrancar devolve vitalidade; toque humano posterior exclui aquele colab-dia da tranca. | “C6 LE a tranca […] sem carimbo e sem estado novo; destrancar devolve a pergunta viva” (`app/docs/PROMPTS.md:1338`); exceção do toque humano (`app/docs/PROMPTS.md:1342`). A escolha de fechar a raiz pelo motivo `competencia_trancada` continua sem gabarito específico nessas cláusulas. |
| R1/R2 — desligado | Cadastro desligado e disputa fechada → não reabrir a disputa do desligado. | “so chamado; disputa de desligado nao reabre” (`app/docs/CORTES.json:1617`). O texto não decide as demais reaberturas. |
| M2 + P4, por episódio de foto | Cadastro com batida que originou cobrança de foto; foto entregue, inclusive fora do prazo → extinguir a cobrança do episódio identificado por `batida_origem`. | “por episodio via batida_origem” e “foto fora do prazo tambem extingue” (`app/docs/CORTES.json:1596`). O texto não determina a cadeia exata de campos de resposta/validação nem o fechamento de toda raiz. |
| P3, primeira resposta humana | Cadastro ativo, pergunta sem resposta; admin tenta dar a primeira resposta em nome do colaborador → não permitido. | “admin nao da a primeira resposta pelo colab” (`app/docs/CORTES.json:1659`). Essa cláusula não responde ao caso de nova resposta invalidar validação anterior. |
| P5 + P9 — intervalo | Cadastro com intervalo fixo faltante, pergunta respondida com hora e validada pelo admin → registrar intervalo e retirar indenização do dia; sem resposta ou recusada → indenizar pela política escrita. | “Resposta validada pelo admin vira o registro do intervalo e a indenizacao do dia sai; sem resposta ou recusada […] Art. 71 par. 4, indeniza” (`app/docs/CORTES.json:772`). Não usar isso como regra genérica de plausibilidade/folga de toda pergunta. |
| P9 — fato que chega após resposta | Cadastro coerente com escala/posto, pergunta respondida, batida posterior acende o marco: concorda com a resposta pela régua de validação automática → pergunta morre sozinha; discorda → permanece viva para admin. | Corte literal registrado em `app/docs/BACKLOG.md:1731`. |
| P10 — duplicata com episódio | Mesmo cadastro, batida/dia/motivo comprovadamente iguais; pergunta COM episódio e outra SEM episódio → conservar a COM episódio e retirar a SEM pela via fora do fato, com trilha; sem prova de mesmo fato, conservar ambas. | D8 RAIZ (`app/docs/BACKLOG.md:2016`). Essa regra não responde a todas as outras vias de identidade. |
| Destino F1 | Cadastro, dia e fato únicos, dois chamados candidatos do mesmo fato com PKs distintos; mesma pergunta reapresentada em ordens diferentes → destino eleito pelo fato, empate pelo menor PK; um escritor. | “escritor unico […] eleitor por fato com empate menor PK” (`app/docs/PROMPTS.md:2085`); L-007: “campo de estado tem 1 escritor canonico + tripwire” (`app/docs/LEIS.md:65`). |
| T1 — trilha de ação crítica | Mesmo cadastro/batidas; ação crítica muda estado → registrar usuário, antes/depois e motivo. | “acao critica grava usuario, antes/depois e motivo” (`app/docs/LEIS.md:65`). Isso não define sozinho os oito atos nem imutabilidade da sequência. |

A regra da L-033 dá um caso de conservação: cadastro com jornada, entrada e saída presentes e só intervalo faltando → dia pagável e pergunta segue aberta (`app/docs/LEIS.md:81`). Não transformá-la em autorização genérica para fechar a pergunta.

Os invariantes escritos para a matriz são: aberta tem ao menos uma viva; push somente com viva pendente daquele colaborador; eleitor único do destino; fonte única do dia (`app/docs/CORTES.json:1694`). Eles permitem casos positivos/negativos de emissão e push, mas não resolvem a escolha entre agregar à dona fechada, reabrir ou criar outra: essa decisão permanece expressamente a pergunta N5 da F1 (`app/docs/CORTES.json:1708`).

Transições/partes sem resposta normativa específica encontrada nos textos examinados e que devem ir a C, preservando o comportamento atual somente em A:

- N2: critério de agregar motivos/contexto à mesma raiz; N3: motivo/pergunta novo em dona fechada; N1: quando criar raiz vazia e como tratar geração sem pergunta viva — N5 já está preservada no corte (`app/docs/CORTES.json:1708`); escolha normativa adicional [nao verificado].
- F1: motivo `vazia` versus `veredito` e precisão de exceções; F3: mecanismo das mudas e alcance da revogação; F4: fechamento da raiz por tranca, além de C6 por leitura [nao verificado].
- R1: teto administrativo 2 e justificativa mínima 20; R2: conjunto de causas sistêmicas além da proibição para desligado; R3: teto colaborador 2 e critérios exatos de corrigibilidade [nao verificado].
- M1: destino das perguntas na retratação de motivo; M2: campos exatos do automatismo da foto, além da extinção por episódio [nao verificado].
- P2: troca de célula/destino e conservação da dona; P3: sobrescrita de respondida/validada e invalidação anterior; P4: reparo pela trilha e data do carimbo [nao verificado].
- P5: demais tipos de resposta, plausibilidade e ramo folga; P6: devolução da validação recusada e reabertura da dona; P7: motivo obrigatório e limpeza da resposta na recusa; P8: limpeza completa na correção/desabsorção [nao verificado].
- P9: todas as demais vias e condições de carimbo; P10: `admin_decidiu`, `motivo_retratado`, `paridade_recusada`, `fato_ocupado` fora dos recortes normativos acima; P11: condições gerais de descarimbo/ressuscitação; P12/P13: troca de via e relavra [nao verificado].
- V1/V2: escolha dos três valores e política de falha/backfill; T1: oito atos e imutabilidade; I1: regra do preenchimento histórico de identidade [nao verificado].

Cuidado com cortes curtos sem a pergunta de origem: `LU-20 = nao validavel` e `LU-21 = reinicia` estão escritos, mas o referente completo não está nesses registros; não atribuir escopo inventado (`app/docs/CORTES.json:1673`, `app/docs/CORTES.json:1680`).

## 15/11 00:44 · sessao 215a8f9a2af7 · - · - · origem=Ronald (tmux)

- Universo real: `F.dias_que_seguram(empresa, mes, ano)` chama `classificar_export` e `_dias_que_retem`; esta delega a `motivos_retencao_celula(..., com_dias=True)`. Não reconstruir pelo TSV. (`app/folha/fora_da_folha.py:109`, `:126`, `:144`, `:105`)
- Carga/tarefa: `F._carga(colabs, ini, fim, AGORA)` → `carga_da_tarefa`; usar `carga.tarefa(colab, dia)` e `porta_da_tarefa(t)['fio']`. (`app/folha/fora_da_folha.py:58`, `app/chamados/services/tarefa_do_dia.py:648`, `:549`, `:638`)
- Vencimento deve preservar o anexo: ler **pai da disputa**, `p.disputa.chamado.prazo_sla`, e chamar `prazo_estourou(prazo, AGORA)`, cujo critério é `prazo <= agora`. Não trocar pelo destino retornado por `chamado_que_lista`, nem pelo prazo derivado de cobrança. (`app/chamados/catalogo/motor.py:54`, `app/chamados/services/fio.py:97`, `:117`; seleção do anexo `[nao verificado]`)
- Previsto único: `escala.utils.minutos_previstos_do_dia(colab, data, celulas=None, folgas=None, escalas=None)`. Ela consulta vínculo, dia de trabalho, marcos e todas as pausas; não usa `minutos_jornada`. (`app/escala/utils.py:2230`, `:2234`, `:2260`, `:2266`, `:2269`, `:2291`, `:2320`)
- Para alimentar essa função pela carga: `carga.celulas_de(cid, ini, fim)` oferece o formato `{(cid, data): celula}`; `carga._vinculos[cid]` contém todos os vínculos carregados, compatíveis com o contrato de `escalas`. (`app/ponto/turnos.py:2988`, `:2990`, `:3003`; `app/escala/utils.py:2252`)
- Relógio: `dias_que_seguram` chega a `timezone.localdate()`; congelar `AGORA` evita calendário divergente. A carga declara que vários estados continuam atuais mesmo recebendo horizonte histórico. (`app/folha/export.py:1238`; `app/chamados/services/tarefa_do_dia.py:370`, `:374`)
- Proteção de leitura já existente: `somente_leitura`, em `diff_e3_sombra.py`, recusa comandos múltiplos, CTE, `INTO`, `NEXTVAL`, operações de escrita e outros SQL não reconhecidos. O comando instala wrappers em todos os aliases. (`app/chamados/management/commands/diff_e3_sombra.py:30`, `:94`)
- **Dependência crítica:** antes da carga, ler usuário reservado com SELECT e abrir `tranca_em_lote(sistema_pk=...)`, como o comando existente faz; isso impede dependências de chegarem ao `get_user_sistema/get_or_create`. Ausência do usuário deve ser erro, sem criação. (`app/chamados/management/commands/diff_e3_sombra.py:96`, `:100`; `app/chamados/tests/test_e3_diff_sombra.py:26`, `:38`; `app/chamados/juizes.py:1225`)
- Escritor de `prazo_sla`: `ChamadoColaborador.definir_prazo_sla`, atribuição e `save` exclusivos dessa porta; emissores também fornecem o prazo na criação. Leitor do vencimento: `prazo_estourou`. (`app/chamados/models.py:574`, `:589`, `:592`; `app/chamados/services/disputa_emissao.py:581`, `:1117`, `:1352`, `:1725`; `app/chamados/services/cobranca.py:178`; `app/chamados/catalogo/motor.py:54`)
- Escritor de `CelulaDia.veredito/protestos`: `ponto.portas.celula.lavrar_veredito`; leitor da retenção: `_dias_sem_decisao`. (`app/ponto/portas/celula.py:1032`, `:1092`, `:1093`, `:1098`; `app/folha/export.py:1273`, `:1286`, `:1293`)
- O envelope só monta a árvore em `/app` e a sonda em `/sonda/sonda.py`; não disponibiliza automaticamente o TSV colado. Portanto, a conferência precisa de texto embutido ou entrada explicitamente fornecida e legível. (`bin/sonda_leitura.sh:214`, `:217`, `:238`, `:240`)
- As leituras atuais da tarefa carregam `veredito_lavrado` e executam `_tarefa_pura`/`_portas_da_tarefa`, sem chamada explícita ao materializador nesses trechos. (`app/chamados/services/tarefa_do_dia.py:421`, `:424`, `:549`, `:557`, `:567`)

## 15/11 00:44 · sessao 31ff7cb1277f · - · - · origem=Ronald (tmux)

Transitivos confirmados:

- **A19 e projeção:** `DecisaoPergunta.registrar` insere DP e chama `PerguntaDisputa.acordar_celulas` (`app/chamados/models.py:4096`, `:4097`). A reabertura também chama `acordar_celulas` explicitamente (`app/chamados/services/disputa_emissao.py:1635`).
- **`acordar_celulas` apenas lê `CelulaDia`** para localizar célula e delega inserção do evento (`app/chamados/models.py:3323`, `:3325`, `:3327`, `:3331`). A escrita concreta é **`escala.EventoCelula(celula_id, colaborador_id, data)`**, em `app/ponto/portas/celula.py:954`; `criado_em` e defaults de processamento estão em `app/escala/models.py:1860`.
- **A18 colisão** salva C/via, acionando o receiver da outbox (`app/chamados/signals.py:253`, `:257`, `:259`). Gravar somente `recusa_motivo` não passa pelo conjunto `CAMPOS_QUE_ACORDAM` (`app/chamados/models.py:3303`, `app/chamados/signals.py:257`).
- **Não há escrita direta de `CelulaDia`, `FechamentoMensal.derivado_em` ou L** nas funções `acordar_celulas`, `reabrir_para_correcao`, `carimbar_identidade` e `DecisaoPergunta.registrar` examinadas (`app/chamados/models.py:3314`, `:3251`, `:4086`; `app/chamados/services/disputa_emissao.py:1562`). Generalização para todo despacho dinâmico: **[nao verificado]**.
- A função de invalidação de FM escreveria `derivado_em=None` e `atualizado_em` quando chamada, mas pertence a outro caminho; nenhuma chamada direta A18/A19/DP foi encontrada (`app/ponto/services/fechamento.py:1616`, `:1656`; cobertura dinâmica **[nao verificado]**).
- **L** tem escritores próprios: `gravar_lavras_em_lote` (`app/chamados/veredito_lavrado.py:240`, `:250`) e `lavrar`, que grava `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`:270`, `:272`, `:273`, `:275`).

Existe cascata **após commit**: qualquer `PerguntaDisputa.save` agenda `becos.fechar_beco_da_disputa` (`app/chamados/signals.py:230`, `:240`). Se o classificador real permitir, chama resolver e `DisputaSupervisao.fechar` (`app/chamados/services/becos.py:25`, `:54`, `:57`, `:74`). Esse fechamento grava F (`app/chamados/models.py:1996`–`:2000`) e dispara os receivers da disputa, inclusive transição do chamado-pai e retomada de regularização (`app/chamados/signals.py:43`, `:139`, `:146`, `:165`). Quantidade/campos efetivamente atingidos nessa cascata sem dados/alcance leitor completo: **[nao verificado]**.

O consumidor da outbox é outra execução, `consumir_eventos_celula`, que chama rejunção, retratações, lastro e fábrica (`app/chamados/reconciliador.py:1316`, `:1365`–`:1388`). Seu efeito não deve ser confundido com o simples INSERT do evento (`app/ponto/portas/celula.py:954`).

A19 também cria `LogAuditoria` pelos dois adaptadores de auditoria (`app/chamados/services/validacao.py:637`, `:643`; `app/core/auditoria.py:22`; `app/core/observ.py:134`–`:138`), além da nota já informada.

## 15/11 00:44 · sessao 697894d24fd9 · - · - · origem=Ronald (tmux)

Conferência das referências sob minha responsabilidade:

| Citação anexada | Estado | Sítio desta cópia e conteúdo |
|---|---|---|
| `CLAUDE.md:142` | SEGUE | Motor exige aval explícito. (`CLAUDE.md:142`) |
| `CLAUDE.md:143` | SEGUE | Porta única de escrita da batida; motor não fabrica batida. (`CLAUDE.md:143`) |
| `CLAUDE.md:146` | SEGUE | UTC no banco; apresentação UTC-3; sonda usa `timezone.localtime`. (`CLAUDE.md:146`) |
| `CLAUDE.md:148` | SEGUE | Grade como fonte única de previsto. (`CLAUDE.md:148`) |
| `CLAUDE.md:178` | SEGUE | Idempotência estrutural: 2x = mesmo estado, uma trilha, zero duplicata; parâmetro consumido ou declarado sem efeito. (`CLAUDE.md:178`) |
| `CLAUDE.md:179` | SEGUE | Separação ESTRUTURA/CADASTRO/BATIDA; só ESTRUTURA é cura de código. (`CLAUDE.md:179`) |
| `CLAUDE.md:411` | SEGUE | Gravado exportado não muda por caminho genérico; regime da porta específica nas linhas seguintes. (`CLAUDE.md:411`) |
| `app/docs/LEIS.md:107` | SEGUE | L-080 inclui UTC/UTC-3. (`app/docs/LEIS.md:107`) |
| `app/docs/LEIS.md:136` | SEGUE | L-110: casos pela regra antes do código, perguntas sem resposta escrita separadas, código atual não é gabarito. (`app/docs/LEIS.md:136`) |
| `app/docs/LEIS.md:145` | SEGUE | L-124: horizonte para frente, passado FOTO até fato novo, backfill único. (`app/docs/LEIS.md:145`) |
| `app/docs/LEIS.md:152` citado como L-114 | MUDOU | L-114 está em `app/docs/LEIS.md:153`; a linha 152 agora contém L-115. (`app/docs/LEIS.md:152`; `app/docs/LEIS.md:153`) |
| `app/docs/LEIS.md:153` citado como L-113 | MUDOU | L-113 está em `app/docs/LEIS.md:154`; conserva nascimento permitido em exportada, trilha e DIFF anterior ao ato de frota. (`app/docs/LEIS.md:154`) |
| `app/docs/JUIZES.md:14` | SEGUE | Registro vivo prevalece sobre lookup. (`app/docs/JUIZES.md:14`) |
| `app/docs/JUIZES.md:20` | SEGUE | `turnos_do_colab` continua no lookup da geometria do turno. (`app/docs/JUIZES.md:20`) |
| `app/docs/JUIZES.md:38` | SEGUE | `validar_pergunta` continua escritor único de `validada_em`. (`app/docs/JUIZES.md:38`) |
| `app/core/juizes.py:208` citado como registro `turnos_do_colab` | MUDOU | Registro em `app/core/juizes.py:212`; pergunta: a que turno/data_turno pertence a batida. A linha 208 hoje registra folga lançada. (`app/core/juizes.py:208`; `app/core/juizes.py:212`) |
| `app/core/juizes.py:1199` citado como registro `selfie_ausente` | MUDOU | Registro em `app/core/juizes.py:1205`; comentários de prazo/canal em 1202–1204. (`app/core/juizes.py:1202`; `app/core/juizes.py:1205`) |
| `app/docs/CORTES.md:283` citado como corte da selfie | MUDOU | Texto integral em `app/docs/CORTES.md:334`; resumo também em 94. A linha 283 atual trata de outras assinaturas. (`app/docs/CORTES.md:283`; `app/docs/CORTES.md:334`) |
| `BACKLOG.md:2109` na matriz | MUDOU | Caminho correto `app/docs/BACKLOG.md:2166`: mesmo desencontro emissor/leitores, entrega do episódio e selo AST; texto recebeu atualização de aval. (`app/docs/BACKLOG.md:2166`) |
| `app/colaboradores/services/ficha.py:215` | SEGUE | Lê `app['ultima_batida']` de ficha; escritor próprio em 147, sem relação com contexto da disputa. (`app/colaboradores/services/ficha.py:147`; `app/colaboradores/services/ficha.py:215`) |
| `app/escala/services/furos_diarios.py:281` | SEGUE | Escreve data civil local de última batida em linha de relatório, outro domínio. (`app/escala/services/furos_diarios.py:281`) |
| `app/relatorios/views.py:1675` | SEGUE | Formata a última batida da linha de relatório. (`app/relatorios/views.py:1675`) |

Nenhuma referência normativa acima SUMIU; houve deslocamentos e duas referências antigas de LEIS passaram a apontar outra lei. (`app/docs/LEIS.md:152`; `app/docs/LEIS.md:153`; `app/docs/LEIS.md:154`)

Há dois cortes específicos desta obra, ausentes do envelope anexado:

- LU-09 diz literalmente “por episodio via batida_origem” e que foto fora do prazo também extingue. É corte explícito para D5. (`app/docs/CORTES.md:441`; `app/docs/CORTES.json:1596`)
- LU-10 diz “retirar o campo sem leitor”, sem nomear o campo. Sua aplicação a `qtd` permanece **[nao verificado]**; o texto normativo reproduzido em JSON também não identifica o campo. (`app/docs/CORTES.md:442`; `app/docs/CORTES.json:1603`)

A declaração posterior de que D1–D4 estariam decididos não substitui o teor de um corte: o registro não reproduz as decisões nem identifica o enunciado que escolhe o transporte, o alcance do merge ou o significado do timestamp. Portanto, essas escolhas devem continuar como perguntas separadas, sem resposta **[nao verificado quanto ao corte específico]**.

Para o CENSO: o juiz do dia de turno tem registro verificável; o juiz de ausência de foto tem registro e corte verificáveis; a validação tem lookup verificável. (`app/core/juizes.py:212`; `app/core/juizes.py:1205`; `app/docs/CORTES.md:334`; `app/docs/JUIZES.md:38`)

Não transportar dados adicionais de produção encontrados em documentos da árvore: eles não pertencem ao envelope autorizado **[nao verificado pelo dado fornecido]**.

## 15/11 00:44 · sessao fcbe4f6072a4 · - · - · origem=Ronald (tmux)

Leitores comprovados do intervalo e comportamento:

| Leitor | Evidência | Tratamento de fim < início |
|---|---|---|
| `escalas_do_periodo_por_colab` | `app/escala/alimentacao.py:149-151` | Filtra interseção (`início <= fim_janela`, fim nulo ou `fim >= início_janela`); **não verifica** `fim >= início`. Pode incluir um invertido numa janela suficientemente abrangente. `mais=` readmite geradoras fora do filtro, `:155-159`. |
| `escalas_do_periodo` | `app/escala/alimentacao.py:185-186` | Delega ao anterior. |
| `vinculo_cobre_a_data` | `app/escala/alimentacao.py:214-216` | Exige `início <= dia <= fim`, além de vigência declarada. Invertido não cobre nenhum dia. |
| `q_vinculo_cobre_a_data` | `app/escala/alimentacao.py:223-225` | Mesma regra em SQL. |
| `vinculo_do_dia`, geradora | `app/escala/alimentacao.py:279-284` | Recusa dia fora dos dois limites; invertido sempre retorna `None`. |
| `vinculo_do_dia`, fallback | `app/escala/alimentacao.py:285-286` | Usa `vinculo_cobre_a_data`; invertido não vira candidato. |
| `vinculo_do_dia_carregado` | `app/escala/alimentacao.py:289` | Carregador da juíza, sem novo predicado; docstring em `:292-294`. |
| `previstas_do_periodo` | `app/escala/alimentacao.py:390-393` | Recorta o intervalo e pula explicitamente `fim < ini`. |
| Apuração por fatias | `app/ponto/services/fechamento.py:206-207`, `:242-243` | Recorta e pula explicitamente fatia invertida. |
| `vinculos_inconsistentes` | `app/escala/services/vinculos_inconsistentes.py:24-37` | Detecta invertidos em `:30-31`, exclui-os da análise de sobreposição. |
| `vigencia_sem_trilha` | `app/escala/services/vigencia_sem_trilha.py:66-77` | Usa `vigencias_impossiveis`, separa o passivo e não o soma aos demais sem trilha. |
| `rejulgar_furos_do_vinculo` | `app/escala/regua_defesa.py:686-688` | Usa `data__range=(data_inicio, min(data_fim, ontem))`; não valida par, range invertido seleciona zero. |
| `piso_da_fase` | `app/escala/services/fase_na_tela.py:160-165` | Cobertura pela juíza SQL; sem cobertura, retorna menor início de todos os vínculos, sem conferir fim. |
| Propositor | `app/escala/services/propositor.py:448`, `:460-461` | Seleciona pelo juiz; depois pula fim anterior ao piso de aplicação. |
| Exportação, explicação de cadastro zero | `app/folha/export.py:938-957` | Passa lista inteira ao previsto por dia; lê finais apenas para mostrar o último encerramento. |
| Signals de mudança de vigência | `app/escala/signals.py:25-28`, `:57-75`, `:121-126`, `:200-204` | Comparam vigência anterior/atual e calculam janela de regeneração; não exigem `fim >= início` nesses trechos. |

Consumidores que delegam a cobertura/seleção à função real:

- Folgas: `app/escala/folgas.py:451`, `:487`, `:493`, `:539`, `:624`.
- Semana do posto: `app/escala/services/semana_do_posto.py:75`.
- Cadastro realidade: `app/escala/services/cadastro_realidade.py:489`, `:509`, `:756`, `:760`.
- Consultas de colaboradores: `app/colaboradores/queries.py:64`.
- Calendário: `app/colaboradores/services/calendario.py:505-506`.
- Ficha: `app/colaboradores/services/ficha.py:129`; exibição de início/fim em `:86-93`, trocas por início em `:131-135`.
- Serviço jornada: `app/escala/servico_jornada.py:63-66`.
- Geração de células, outros vínculos: `app/ponto/management/commands/gerar_celulas.py:283-287`.
- Porta de regeneração: `app/ponto/portas/celula.py:229`, `:339`, `:906-909`.
- Regeneração exportada: `app/ponto/portas/regen_exportada.py:147-152`.
- Diagnóstico de escala: `app/ponto/services/diagnostico_escala.py:53`.
- Espelho: `app/ponto/services/espelho.py:286`, `:1065`.
- PDF: `app/relatorios/pdf_espelho.py:307`, `:571`.
- Calculador: `app/ponto/calculador/alimentacao.py:192-196`, carregamento por limite inicial em `:421`.
- Turnos: `app/ponto/turnos.py:1960`, `:2166`, `:2361`, `:2986`, `:3058`.
- Precedência: `app/ponto/precedencia.py:312`, `:323`, `:349`.
- Prévia folha: `app/folha/previa.py:62`, `:81`.
- Exportação: `app/folha/export.py:359`, `:1252`.
- Feriados: `app/core/feriados.py:154`, `:161`.
- Régua CCT: `app/core/regua_cct.py:329`.
- Supremo juiz: `app/ponto/supra_juiz.py:174`, `:185`.
- Núcleo: `app/ponto/nucleo.py:15`, `:26`.
- Limbo folgas: `app/chamados/services/limbo_folgas.py:200`.
- Invariantes chamados: `app/chamados/management/commands/auditar_invariantes_chamados.py:361`, `:391`, `:1538`.
- Triagem batida: `app/ponto/services/triagem_batida.py:149`, `:402`.
- API mensageria: `app/api/views_mensageria.py:979`.
- Cartório: `app/ponto/services/cartorio.py:1768`, `:1903`.
- Vigia futuro: `app/ponto/services/vigia_cadastro_futuro.py:58`.
- Vigia autoridade: `app/ponto/management/commands/vigiar_motor_autoridade.py:51`.
- Cadastro futuro: `app/ponto/services/cadastro_futuro.py:70`.

Leitores unilaterais não demonstram suposição sobre `fim >= início`: piso do espelho usa apenas início em `app/ponto/services/espelho.py:43-45`; detector ausência usa apenas início em `app/ponto/management/commands/detectar_ausencias.py:177`, `:247`; supra diagnóstico usa apenas início em `app/ponto/management/commands/supra_juiz.py:149`; tripwire usa início e teto em `app/ponto/management/commands/tripwire_celulas.py:67`. Consultas de “aberto” excluem todos os finais não nulos: `app/chamados/detector_anomalias.py:58-62`, `app/escala/services/escala_auto.py:172`, `app/ponto/services/cluster_espurio.py:185`, `app/relatorios/furos.py:61`, `app/chamados/services/disputa_emissao.py:341`.

Diagnóstico conserva regra antiga explicitamente: `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:117-118`. Gerador smoke usa interseção, sem validação do par: `app/ponto/management/commands/gerar_batidas_smoke_motor.py:73-79`.

Executei o censo AST **existente**, somente em memória com `python3 -B`/`runpy`, sem Django nem banco: `copias('app')` retornou conjunto vazio. Algoritmo em `app/core/censo_pergunta2.py:64-117`; cânones em `:28-32`, baseline em `:40`, outros modelos em `:43-60`. Esse selo detecta pares de limites, não todo acesso unilateral; seus limites estão declarados em `:10-18`. Portanto, chamar isso de censo universal de qualquer acesso dinâmico seria `[nao verificado]`.

Atenção: a cópia contém uma constraint **condicional**; ler o contexto antes de afirmar que protege todo passivo: `app/escala/models.py:1039-1040`.

## 15/11 00:43 · sessao 9dc5c8ea9331 · - · - · origem=Ronald (tmux)

A autoridade do dinheiro por dia é `DiaPago` versão `motor`, lida por `linhas_do_periodo` e interpretada por `pago_do_dia`/`rubricas_do_dia`; o TXT lê `FechamentoMensal` gravado e `eventos_do_fechamento`, portanto `fechamento_lido` não substitui o lavrado atual. (`app/ponto/services/dia_pago.py:833`; `app/ponto/services/dia_pago.py:867`; `app/ponto/services/dia_pago.py:901`; `app/folha/export.py:719`; `app/folha/export.py:730`; `app/ponto/services/fechamento.py:1735`)

| Campos lidos | Escritor | Leitor real |
|---|---|---|
| `DiaPago` chaves `colaborador/mes/ano/data/tipo/versao`, campos de `CAMPOS_DIA` | `lavrar`: atribuições `:173`, `:190`, `:234`, `:244`, `:259`, `:269`, `:274`, `:288`, persistência `:345`–`:360` | `linhas_do_periodo`: `app/ponto/services/dia_pago.py:855`; `rubricas_do_dia`: `:914`; `pago_do_dia`: `:890` |
| `DiaPago.minutos_abonados/minutos_previstos/minutos_realizados` | `por_dia_da_grade` calcula mapas: `app/ponto/services/dia_pago.py:364`; `lavrar` incorpora: `:288`; `retratar_exportada` atualiza: `:481`, `:495`, `:538` | `soma_do_periodo`: `app/ponto/services/dia_pago.py:784` |
| `DiaPago.detalhe.folga_feriado_min` | `app/ponto/services/dia_pago.py:257` | `pago_do_dia`: `app/ponto/services/dia_pago.py:897` |
| `DiaPago.horas_reflexo_dsr` | Ajuste `data=None`: `app/ponto/services/dia_pago.py:350`; retrato exportada: `:506`, `:516`, `:542` | `soma_da_competencia`: `app/ponto/services/dia_pago.py:550` |
| `FechamentoMensal` existência/chaves | `get_or_create`: `app/ponto/services/fechamento.py:898`; persistência `:1144` | `soma_do_periodo`: `app/ponto/services/dia_pago.py:805`; TXT: `app/folha/export.py:719` |
| `FechamentoMensal.semanas_dsr_ok/semanas_dsr_perdido/horas_reflexo_dsr` | `app/ponto/services/fechamento.py:1101` | Reflexo copiado ao retrato: `app/ponto/services/dia_pago.py:506`; contador: `app/inteligencia/views.py:119` |

Sem fechamento deve ser `sem apuracao ainda`, sem linha com fechamento deve ser `sem lavratura`; nenhum desses silêncios deve virar zero. (`app/ponto/services/dia_pago.py:761`; `app/ponto/services/dia_pago.py:805`)

O DSR é semanal no motor e o reflexo é ajuste da competência sem data; não pode ser rateado aos dias nem contado repetidamente por cada dia do recorte. (`app/ponto/motor_calculo_v2.py:2590`; `app/ponto/motor_calculo_v2.py:2629`; `app/ponto/motor_calculo_v2.py:3460`; `app/ponto/services/dia_pago.py:348`; `app/ponto/services/dia_pago.py:755`)

Não foi encontrada API real de contrafactual completo que receba “este dia passa a falta/abono” e devolva simultaneamente minutos e DSR; fechamento dessa ausência em toda a árvore: [nao verificado]. As funções próximas não fazem essa pergunta:

- `minutos_de_falta_por_dia` lê ausências já gravadas; `_rateio_por_dia` distribui minutos de uma ausência recebida. (`app/folha/export.py:125`; `app/folha/export.py:95`)
- `por_dia_da_grade` calcula abono de grade já julgada. (`app/ponto/services/dia_pago.py:364`)
- `efeitos_da_ausencia` consulta as ausências/justificativas atuais. (`app/ponto/services/efeito_ausencia.py:28`; `app/ponto/services/efeito_ausencia.py:40`; `app/ponto/services/efeito_ausencia.py:53`)
- `veredito_celula` cria ausência e lavra o dia; é escritor. (`app/ponto/services/veredito_celula.py:331`; `app/ponto/services/veredito_celula.py:337`; `app/ponto/services/veredito_celula.py:352`)
- `simular_dia` oferece grade, teste de coerência e flips em memória, sem simulação de ausência/DSR. (`app/ponto/services/simular_dia.py:5`; `app/ponto/services/simular_dia.py:11`; `app/ponto/services/simular_dia.py:26`)

Recomendação: imprimir os campos pagos separadamente em minutos, mantendo os campos `minutos_*` na unidade nativa; não somar adicional noturno, extras/subsets e trabalho numa quantidade única de “minutos pagos”. Os subsets noturnos/feriado já pertencem aos totais. (`app/ponto/services/dia_pago.py:34`; `app/ponto/services/dia_pago.py:904`; `app/folha/export.py:424`)

## 15/11 00:43 · sessao e11c49530784 · - · - · origem=Ronald (tmux)

Funções reais encontradas, sem executar banco nem gravar arquivos [nao verificado]:

| Pergunta | Função real e limite |
|---|---|
| Previsto diário | `escala.utils.minutos_previstos_do_dia(colab, data, celulas=None, folgas=None, escalas=None)`; seleciona vínculo, pede dia de trabalho, marcos e pausas. `app/escala/utils.py:2230`, `:2260`, `:2266`, `:2269`, `:2291`. |
| Abono efetivo já decidido na grade | `ponto.services.dia_pago.por_dia_da_grade(grade)` devolve mapas `(abono, previsto, realizado)`; usa `tipo_dia`, `minutos_previstos` e `minutos_realizados`; cobre somente a parcela prevista não realizada. `app/ponto/services/dia_pago.py:364`, `:387`, `:393`, `:395`, `:402`. |
| Ausência real → justificadas/suprimidas | `efeitos_da_ausencia(colaborador, data_ini, data_fim)` consulta justificativas e ausências reais; não aceita cenário hipotético. `app/ponto/services/efeito_ausencia.py:28`, `:40`, `:47`, `:53`, `:59`. |
| Falta decidida em minutos por dia | `folha.export.minutos_de_falta_por_dia` consulta ausências reais que descontam e chama `_rateio_por_dia`; não calcula “tornar este dia falta”. `app/folha/export.py:125`, `:138`, `:140`. |
| DSR | `MotorBase.calcular_mes` calcula faltas e direito por semana; reflexo também usa dados semanais. `app/ponto/motor_calculo_v2.py:2265`, `:2593`, `:2600`, `:2629`, `:2636`, `:2651`, `:2655`. `MotorComercial.calcular_mes` recompõe DSR próprio. `app/ponto/motor_calculo_v2.py:3308`, `:3460`. |
| Leitura mensal recalculada | `fechamento_lido` chama a função de fechamento com `somente_leitura=True`; sua interface não recebe cenário. `app/ponto/services/fechamento.py:1735`, `:1759`. |
| Ato que transforma em falta/justificado | `veredito_celula` chama `criar_ausencia`, fecha fio e lavra célula; é escritor, inadequado à sonda. `app/ponto/services/veredito_celula.py:332`, `:337`, `:347`, `:352`. |
| Simulação já existente | `simular_dia.grade_dia` gera grade sobre batidas; `com_flips` simula inversões. Não fornece cenário de ausência/abono/DSR nessas interfaces. `app/ponto/services/simular_dia.py:5`, `:26`. |

Não identifiquei função real pronta que receba “este dia passa a ausência descontante/abonado” e devolva o delta completo em minutos e DSR; existência em outros caminhos permanece [nao verificado]. A montagem dos conjuntos que o motor recebe está dentro do fechamento, ligada à cobertura real das ausências; substituí-la na sonda criaria interpretação própria do cenário. `app/ponto/services/fechamento.py:660`, `:683`, `:693`, `:724`, `:739`.

Deltas individuais de DSR não podem ser somados como parcelas independentes: o cálculo decide a semana com base no conjunto de faltas e acumula semanas perdidas, sem atribuir uma parcela a cada dia. `app/ponto/motor_calculo_v2.py:2587`, `:2600`, `:2605`, `:2655`.

Para cumprir literalmente o pedido sem criar regra, imprimir `sem funcao real` para os contrafactuais completos; “manter” pode continuar exibindo a leitura existente, mas uma API específica de delta desse cenário não foi identificada [nao verificado].

## 15/11 00:43 · sessao 1b274078e0f5 · - · - · origem=Ronald (tmux)

Referências e decisões para a sonda:

- `EscalaColaborador` não tem campo de criação ou alteração. A classe começa em `app/escala/models.py:871` e termina antes de `AgendaDia`, em `app/escala/models.py:1399`; seus únicos `DateTimeField` são `regua_avaliada_em` (`:884`) e `regua_corroborada_em` (`:888`), que não representam criação/alteração. Emitir `campo_criacao_alteracao=null`, sem substituir por datas de régua.
- Perguntar desligamento a `esta_desligado(colaborador)`, implementada em `app/colaboradores/services/desligamento.py:23-32` e registrada em `app/core/juizes.py:271`. Ela compara o estado gravado com `'desligado'`. Não rotular automaticamente a resposta falsa como “ativo”: há também `afastado` e `ferias` em `app/colaboradores/models.py:295-300`. Emitir o booleano do juiz e `situacao` real.
- Competência de uma data: `ponto.janelas.janela_atual(data, empresa)`, em `app/ponto/janelas.py:60-63`. O mês/ano são os da data final da janela, conforme `periodo_apuracao`, em `app/ponto/janelas.py:10-21`. Para um intervalo, iterar blocos de competência avançando do fim da janela para o dia seguinte.
- Classificação exportada/aberta: chamar `competencia_entregue(empresa_id, mes, ano)`, em `app/folha/export.py:556-594`, registrada em `app/core/juizes.py:337`. Ela exige uma `ExportacaoDominio` vigente e não invalidada; não substituir por `FechamentoMensal.status`.
- Batidas: `batidas_apuraveis` filtra timestamps com `>= ini` e `<= fim`, em `app/ponto/turnos.py:2875-2883`. Passar datas simples perderia as horas do último dia. Usar limites conscientes de fuso: meia-noite inicial e último microssegundo do dia final; ou chamar o juiz com início consciente e acrescentar `timestamp__lt` à meia-noite do dia seguinte ao final.
- Outro vínculo cobrindo todo o intervalo: usar a função real `vinculo_cobre_a_data(other, lo)` e a mesma para `hi`; seu predicado contínuo está em `app/escala/alimentacao.py:214-216`. Isso verifica existência de um vínculo individual cobrindo ambas as pontas, distinto de união de vários vínculos.
- Há precedente de `execute_wrapper` que aceita somente SELECT e recusa CTE, múltiplos comandos, `INTO` e palavras de escrita/efeito em `app/chamados/management/commands/diff_e3_sombra.py:30-38`; instalação em todos os aliases, `:92-95`. Não imprimir SQL ou parâmetros em erro.
- Backend é `django_tenants.postgresql_backend`, em `app/config/settings/saas.py:85`; o envelope invoca `tenant_command shell --schema=juliani`, em `bin/sonda_leitura.sh:239-240`. O comportamento interno de `SET search_path` está em dependência externa, ausente da cópia: `[não verificado]`.
- Não executar o envelope nesta sessão: embora o corpo da sonda possa usar somente SELECT, o envelope reescreve credencial/role (`bin/sonda_leitura.sh:126-143`), concede permissões (`:157-181`), cria temporário (`:216-218`) e grava trilha (`:248-250`). Entregar somente o comando futuro como texto.

## 15/11 00:43 · sessao 863362a85a00 · - · - · origem=Ronald (tmux)

- E4B real é `backfill_fio_morto_e4b`. O alcance básico é `censo_alcance` filtrado por `rota == 'sem_porta'`; unidade dia-colaborador, não pergunta. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`, `:79`; `app/chamados/services/vigia_fio.py:549`)
- Esse universo percorre competências de `competencias_a_lavrar`, atual e anterior ainda não entregue, via `dias_que_seguram`; não representa todo histórico exportado. (`app/chamados/services/vigia_fio.py:505`, `:584`, `:590`)
- O complemento exportado efetivamente declarado é `_exportadas_classe_e(empresas, hoje)`: perguntas das empresas visíveis em `[hoje−45, hoje)`, competência não explicitamente aberta e `reabrir_credor_de_pergunta_viva(..., executar=False) != None`; deduplica por `(colaborador_id, data_evento)`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:153`, `:164`, `:166`, `:173`, `:175`, `:178`)
- Esse planejamento inclui recusas; não significa “aplicaria”. A porta calcula `credor_toca_exportada` e limbo e, com `executar=False`, retorna `recusa:<motivo>` ou `reabriria`, sem executar reabertura. (`app/chamados/services/disputa_emissao.py:1842`, `:1846`, `:1856`)
- `ato_do_dia` é seleção real de leitura das portas por dia: `reabrir_credor`, `registrar_decisao`, `perguntar`, `materializar`, `rejulgar`; usa a tarefa real e o plano de reabertura. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:82`, `:87`, `:90`, `:97`, `:109`)
- Para “projeção” há função de alcance pronta: `backfill_decisoes_pergunta.linhas_a_criar()`, retornando `{(empresa_id, mes, ano): {'resposta': [(cid, pk)], 'validacao': [...]}}`. Seu universo declarado é pergunta com R ou V não nulo e `q_do_horizonte_ou_sem_dia() OR conversa`; conversa e dia nulo vão a baldes sem competência de fato. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `:55`, `:56`, `:59`, `:73`, `:75`, `:79`, `:82`)
- A projeção produz decisões `RESPOSTA` com `pergunta_id/ato/ato_em/ato_texto/ato_motivo`, e `VALIDACAO` com esses campos mais `ato_por_id`. Só correspondência exata `(pergunta_id, ato, ato_em)` evita nova linha. Escritor factual é `DecisaoPergunta.registrar`, via `bulk_create`. (`app/chamados/models.py:4070`, `:4074`, `:4076`, `:4078`, `:4080`, `:4086`, `:4096`)
- O comando E1 apenas mede essas linhas; seu apply escreve itens 2 e 4, não projeção. O E4B pode chamar o executor de projeção por `registrar_decisao`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3`, `:6`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:138`, `:141`)
- O anexo `fatias.md` descreve troca de leitores e não identifica operação que aplique a “fatia de projeção”; atribuir automaticamente o alcance E1 acima à E4c é **[nao verificado]**.
- A função real dia→competência entregue é `competencia_do_dia_entregue`: `janela_atual` → `competencia_entregue`; dia nulo retorna `None`, não aberta. (`app/chamados/services/vigia_fio.py:843`, `:855`, `:859`, `:862`)
- Envelope esperado: `bin/sonda_leitura.sh --sombra <arquivo.py>`; utiliza settings de sombra e shell Django `tenant_command --schema=juliani`. O próprio envelope cria/regrava arquivos e infraestrutura; nesta tarefa somente seu texto deve ser entregue. (`bin/sonda_leitura.sh:56`, `:94`, `:126`, `:134`, `:216`, `:239`, `:249`)

## 15/11 00:43 · sessao b34c0ce428a2 · - · - · origem=Ronald (tmux)

- **Juiz de exportada:** `competencia_entregue(empresa_id, mes, ano, registro=False, *, exportacoes=None)`, em `app/folha/export.py:556`. Lê exportações da tripla, aplica `anotar_vigencia`/`so_vigentes` e aceita o vigente somente com `invalidada_em is None`, em `app/folha/export.py:580`. Não equivale a `.exists()` sobre `ExportacaoDominio`, em `app/folha/export.py:591`.

- **TXT a partir do gravado:** `montar_linhas_export` seleciona `FechamentoMensal` da empresa/competência com `status` aberto/aprovado, em `app/folha/export.py:717`; consulta a classificação dos colaboradores em `app/folha/export.py:714`. Os valores das linhas vêm de `eventos_do_fechamento`, em `app/folha/export.py:730`.

- **Campos monetários lidos:** `horas_extras_100_feriado`, `horas_extras_100`, `horas_extras_50`, `horas_folga_trabalhada`, `horas_noturnas`, `horas_intra_indenizada`, `horas_atraso` e `horas_saida_antecipada`, em `app/folha/export.py:420` e `app/folha/export.py:439`. Mapeamento evento→rubrica: `RubricaDominio` ativa da empresa sobre a global, em `app/folha/export.py:984`.

- **Faltas no TXT:** datas vêm das ausências aprovadas cujo efeito vigente desconta, por `datas_falta` → `datas_falta_janela`, em `app/folha/export.py:184` e `app/folha/export.py:169`; entram no bloco de falta em `app/folha/export.py:740`. `minutos_de_falta_por_dia` também fornece os minutos usados pela derivação de `horas_falta`, em `app/folha/export.py:125` e `app/ponto/services/fechamento.py:873`.

- **Nenhuma leitura direta de R/V/C/recusa da pergunta** foi encontrada em `folha/export.py`; a montagem lê os campos de FM e as autoridades de ausência acima. Exaustividade transitiva por execução dinâmica: **[nao verificado]**.

- **ExportacaoDominio:** guarda a cópia do TXT em `conteudo`, `hash_sha256` e `total_linhas`, em `app/folha/models.py:82`; `registrar` recebe o conteúdo pronto, calcula seu hash e cria um registro histórico, em `app/folha/models.py:174`. Não deriva minutos ou valores de perguntas, em `app/folha/models.py:176`.

- **Comparação real, sem escrita:** `txt_vigente_x_gravado` obtém o vigente pelo juiz e compara seu hash com o TXT montado do gravado atual, em `app/folha/export.py:655`. Isso mede divergência atual; não mede o efeito hipotético de A18/A19, em `app/folha/export.py:657`.

- **Derivação real em leitura:** `fechamento_lido` chama `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True)`, em `app/ponto/services/fechamento.py:1759`. O objeto é destacado, sem persistência de FM, em `app/ponto/services/fechamento.py:890` e `app/ponto/services/fechamento.py:1128`. Não passar `lavrar_dias_pagos=True`: essa combinação escreve `DiaPago`, em `app/ponto/services/fechamento.py:1191`. Comparar esse objeto com o gravado mede defasagem atual, não o delta atribuível a uma operação hipotética específica: **[nao verificado]**.

- **Possível ambiguidade a registrar:** a árvore chama os “itens 18–19” da E4B de “FIM DO PROGRAMA”, em `app/docs/BACKLOG.md:1977`; a grade registra “itens 18–19; autorização das exportadas”, em `app/docs/GRADE-MESTRA.md:717`. Portanto não equiparar silenciosamente esses itens às portas **A18/A19** da matriz. O enunciado anexado aponta explicitamente para A18/A19; correspondência dos dois vocabulários: **[nao verificado]**.

- **Outro alcance real encontrado:** `projecoes_vencidas_qs(mes, ano, empresa_id)` seleciona FM com `derivado_em=NULL`, em `app/ponto/services/fechamento.py:1668` e `app/ponto/services/fechamento.py:1696`. Sem mês/ano usa a competência corrente de cada empresa, em `app/ponto/services/fechamento.py:1692`. Não há evidência de que esse alcance seja a “fatia de projeção da E4c” do anexo: **[nao verificado]**.

## 15/11 00:43 · sessao 31ff7cb1277f · - · - · origem=Ronald (tmux)

A18/A19 foram localizadas; os endereços do anexo mudaram na cópia.

- **A18 técnica:** `_carimbar_recusa` em `app/chamados/services/materializacao.py:1089`. Grava `PerguntaDisputa.recusa_motivo` em `:1108`/`:1109`; colisão sem carimbo delega a `carimbar_identidade` em `:1105`. Essa porta grava `materializada_em`, `materializada_por_id`, `via_resolucao` (`app/chamados/models.py:3182`, `:3183`, `:3184`, `:3268`, `:3269`) e, quando passado, `recusa_motivo` (`:3270`–`:3273`). O resultado técnico produz nota `recusa_materializacao` (`materializacao.py:1113`), via `FollowupChamado.objects.create` (`app/chamados/services/nota.py:88`, `:103`, `:107`). Não cria DP de recusa humana nesse corpo (`materializacao.py:1089`).
- **A19 humana:** `recusar_pergunta` em `app/chamados/services/validacao.py:562`. Grava `PerguntaDisputa.recusa_motivo` (`:619`, `:625`), chama `reabrir_para_correcao(..., ato='recusa')` (`:628`). Esta prepara limpeza de `respondida_em`, `validada_em`, `validada_por`, `resposta_colab` (`app/chamados/services/disputa_emissao.py:1595`), registra DP com `pergunta_id`, `ato`, `ato_texto`, foto anterior R/V/autor, `ato_em`, `ato_por`, `ato_motivo` (`:1617`–`:1623`) e delega limpeza adicional de C/via (`:1634`; `app/chamados/models.py:3363`–`:3365`). Acorda células (`disputa_emissao.py:1635`), produz nota/auditoria/push (`validacao.py:633`, `:637`, `:643`, `:648`).
- **Alcance A18/A19:** nenhuma das duas funções expõe modo de planejamento em leitura (`materializacao.py:1089`; `validacao.py:562`). Selecionar candidatas copiando suas guardas não seria chamar alcance real. A sonda deve contar `sem alcance chamavel`; número de linhas que efetivamente tocariam: **[nao verificado]**.

A **projeção legada** dispõe de alcance leitor real: `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `linhas_a_criar()`. Retorna mapa por `(empresa_id, mês, ano)` com atos `resposta`/`validacao` e pares `(colaborador_id, pergunta_pk)` (`:45`, `:82`, `:91`). Seu universo é pergunta com R ou V, no horizonte canônico ou sem dia, mais conversa (`:55`–`:61`). Conversa e dia ausente são baldes próprios (`:73`–`:76`); competência factual vem de `competencia_do_fato` (`:79`).

O produtor canônico é `DecisaoPergunta.linhas_da_projecao_sem_linha` (`app/chamados/models.py:4057`), com correspondência pergunta+ato+instante (`:4070`, `:4074`, `:4078`). Produziria novas DP com:

- Resposta: `pergunta_id`, `ato`, `ato_em`, `ato_texto`, `ato_motivo` (`:4076`, `:4077`).
- Validação: os mesmos campos, mais `ato_por_id` (`:4080`–`:4082`).

Executor: `DecisaoPergunta.registrar` (`models.py:4086`), inserção em `:4096`, seguida de eventos das células em `:4097`. O comando existente declara que **não aplica** essa fatia (`backfill_decisoes_pergunta.py:3`–`:6`). Identificar essa operação como a “fatia de projeção da E4c” a partir somente do `fatias.md` fornecido: **[nao verificado]**.

## 15/11 00:35 · sessao 9fef593b873d · A20-E-COMBOIO · leitura · origem=Code

**O par conferido tem fonte histórica no código: `LogAuditoria`.** A existência e a integridade dessa trilha em produção, assim como os números pedidos, estão **[nao verificado]**. O payload é escrito em `app/chamados/services/validacao.py:702-707`.

**(a)** Os campos pertencem a `PerguntaDisputa`: `data_conferida_em` é `DateTimeField` anulável; `data_conferida_por` é FK anulável para `auth.User`, com `SET_NULL` (`app/chamados/models.py:2536-2540`).

O único escritor explícito atual da conferência é `conferir_data_do_fato`: atribui `timezone.now()` e `autor`, salvando somente esses dois campos. O contrato de escritores declara esse mesmo endereço; a view apenas verifica POST/permissão e delega (`app/chamados/services/validacao.py:699-701`; `app/core/tests/test_contract_escritores_estado.py:113-117`; `app/chamados/views.py:1306-1314`). A FK também admite nulificação pela exclusão do usuário, por `SET_NULL` (`app/chamados/models.py:2538`).

A porta exige justificativa, lê o par vigente, recusa concordância e recusa novo carimbo se `data_conferida_em` já estiver preenchida (`app/chamados/services/validacao.py:689-698`). Após salvar, solicita a auditoria com este payload (`app/chamados/services/validacao.py:702-707`):

```python
acao = 'conferir_data'
modelo = 'PerguntaDisputa'
objeto_id = p.pk
descricao = (
    'Data do fato conferida: vale %s (o chamado dizia %s). %s'
    % (ev, turno, just)
)
valor_antes = {'data_turno': turno.isoformat() if turno else None}
valor_depois = {
    'data_evento': ev.isoformat() if ev else None,
    'justificativa': just,
}
```

Portanto, o par histórico ordenado é **`(valor_depois['data_evento'], valor_antes['data_turno'])`**. Esses JSONs são encaminhados à gravação de `LogAuditoria`; a auditoria é *best-effort*, podendo falhar depois do carimbo (`app/core/auditoria.py:22-29`; `app/core/observ.py:105-139`; `app/chamados/services/validacao.py:701-707`).

`DecisaoPergunta` não possui ato de conferência nem campos dedicados ao par; a porta de conferência não registra decisão nessa sequência (`app/chamados/models.py:4000-4027`; `app/chamados/services/validacao.py:683-710`). Assim, **não cabe afirmar que o par nunca foi registrado**: existe a trilha estruturada acima. Sua cobertura real está **[nao verificado]**.

**(b)** O par vigente é **`(ev, turno)` retornado por `data_do_fato_de(p)`**: `ev = p.data_evento`; `turno = dia_do_fato(chamado_que_lista(p), fortes=True)` (`app/chamados/services/fio.py:77-88`). `data_evento` é campo anulável da pergunta (`app/chamados/models.py:2501-2504`).

O chamado considerado é o destino, com fallback para o chamado da disputa. Seu dia forte prioriza `chamado.celula.data`, depois a chave do catálogo e o fallback `contexto_json['data_turno']`; sem dia forte retorna `None` (`app/chamados/services/fio.py:116-117`; `app/chamados/juizes.py:98-128`). Logo, ler apenas o contexto bruto não reproduz a função real (`app/chamados/services/fio.py:84-88`).

O materializador lê essa função, mas sua liberação atual consulta apenas a presença do carimbo: `_diverge and p.data_conferida_em is None` (`app/chamados/services/materializacao.py:1197-1199`).

**(c)** Cabe usar **`competencia_do_dia_entregue(p.data_evento, empresa, memo)`**: ela compõe `janela_atual` com `competencia_entregue`, usando mês/ano do **fim da janela da empresa**, e retorna `None` sem data (`app/chamados/services/vigia_fio.py:843-865`; `app/ponto/janelas.py:60-63`).

`True` identifica exportada; `False`, aberta pelo critério K6 — exportação vigente e não invalidada. Isso não depende do estado aberto/fechado da disputa (`app/folha/export.py:580-594`). A sonda preserva `sem_data` como terceira parte, conforme o anexo fornecido **[nao verificado]**.

**(d)** Texto proposto, sem execução em produção **[nao verificado]**. A recuperação será conservadora: exige datas ISO completas e um único par histórico distinto; trilhas repetidas com o mesmo par são aceitas. Ausência, payload inválido ou pares conflitantes entram em `NAO_RECUPERAVEL`. A comparação usa a tupla vigente inteira, inclusive eventual componente `None`; não usa o booleano de divergência entre os dois dias atuais. Essas são regras explícitas desta sonda proposta **[nao verificado]**.

```python
# A20-E-COMBOIO item (1): texto proposto; execucao [nao verificado].
# Para o mesmo envelope Django/tenant --sombra do anexo.
#
# Universo: PerguntaDisputa.data_conferida_em preenchida.
# Fonte: app/chamados/models.py:2536.
# Par historico: LogAuditoria conferir_data.
# Fonte: app/chamados/services/validacao.py:702-707.
# Par vigente: data_do_fato_de(p), componentes 0 e 2.
# Fonte: app/chamados/services/fio.py:77-88.
# Particao: competencia_do_dia_entregue sobre data_evento vigente.
# Fonte: app/chamados/services/vigia_fio.py:843-865.
#
# Unidade: pergunta; nenhuma chamada a portas de escrita.
# Saida: contagens, #pk e emp; sem descricao/justificativa/datas/pessoas.

import datetime
import io
import logging
import re
from collections import Counter, defaultdict
from contextlib import ExitStack, redirect_stdout, redirect_stderr
from itertools import islice

from django.db import connections

linhas = []
erros = 0
bloqueios = 0
universo = None
total = 0
completo = False
nivel_anterior = logging.root.manager.disable

estados = ('DIFERENTE', 'IGUAL', 'NAO_RECUPERAVEL')
particoes = ('exportada', 'aberta', 'sem_data')
parte = Counter()       # (estado, particao) -> perguntas
por_emp = Counter()     # (empresa_id, estado, particao) -> perguntas
motivos_nr = Counter()
exemplos = defaultdict(list)
memo = {}


def somente_select(execute, sql, params, many, context):
    global bloqueios
    texto = sql.strip()
    if (many or not re.match(r"(?is)^SELECT\b", texto) or ";" in texto
            or re.search(r"(?is)\bINTO\b|\bFOR\s+(UPDATE|SHARE)\b",
                         texto)):
        bloqueios += 1
        raise RuntimeError("SQL bloqueado")
    return execute(sql, params, many, context)


def dia_iso(valor):
    # Nulo, chave ausente e formato inesperado nao viram data presumida.
    if not isinstance(valor, str):
        raise ValueError("dia invalido")
    dia = datetime.date.fromisoformat(valor)
    if dia.isoformat() != valor:
        raise ValueError("dia nao canonico")
    return dia


try:
    logging.disable(logging.CRITICAL)
    with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
        with ExitStack() as pilha:
            for alias in connections:
                pilha.enter_context(
                    connections[alias].execute_wrapper(somente_select)
                )

            from chamados.models import PerguntaDisputa
            from chamados.services.fio import data_do_fato_de
            from chamados.services.vigia_fio import (
                competencia_do_dia_entregue,
            )
            from core.models import LogAuditoria

            qs = (
                PerguntaDisputa.objects
                .filter(data_conferida_em__isnull=False)
                .select_related(
                    'colaborador',
                    'chamado_destino__celula',
                    'disputa__chamado__celula',
                )
                .order_by('pk')
            )
            universo = qs.count()
            it = qs.iterator(chunk_size=500)

            while True:
                lote = list(islice(it, 500))
                if not lote:
                    break

                ids = [p.pk for p in lote]
                vistos = Counter()
                pares = defaultdict(set)
                invalidos = set()

                # Le somente identificador e os dois componentes JSON.
                # Nao carrega descricao nem justificativa.
                logs = (
                    LogAuditoria.objects
                    .filter(
                        acao='conferir_data',
                        modelo='PerguntaDisputa',
                        objeto_id__in=ids,
                    )
                    .order_by('pk')
                    .values_list(
                        'objeto_id',
                        'valor_antes__data_turno',
                        'valor_depois__data_evento',
                    )
                )

                for pid, turno_antigo, evento_antigo in logs.iterator(
                        chunk_size=2000):
                    vistos[pid] += 1
                    try:
                        par = (
                            dia_iso(evento_antigo),
                            dia_iso(turno_antigo),
                        )
                    except (TypeError, ValueError, OverflowError):
                        invalidos.add(pid)
                    else:
                        pares[pid].add(par)

                for p in lote:
                    ev, _diverge, turno = data_do_fato_de(p)
                    vigente = (ev, turno)
                    emp = p.colaborador.empresa_id

                    entregue = competencia_do_dia_entregue(
                        ev, emp, memo
                    )
                    if entregue is True:
                        k = 'exportada'
                    elif entregue is False:
                        k = 'aberta'
                    elif entregue is None:
                        k = 'sem_data'
                    else:
                        raise RuntimeError("retorno de particao invalido")

                    motivo = None
                    if vistos[p.pk] == 0:
                        motivo = 'sem_trilha'
                    elif p.pk in invalidos:
                        motivo = 'payload_invalido'
                    elif len(pares[p.pk]) != 1:
                        motivo = 'pares_conflitantes'

                    if motivo is not None:
                        estado = 'NAO_RECUPERAVEL'
                        motivos_nr[motivo] += 1
                    else:
                        conferido = next(iter(pares[p.pk]))
                        estado = (
                            'DIFERENTE'
                            if vigente != conferido
                            else 'IGUAL'
                        )

                    parte[(estado, k)] += 1
                    por_emp[(emp, estado, k)] += 1
                    chave = (estado, k)
                    if len(exemplos[chave]) < 10:
                        exemplos[chave].append(p.pk)
                    total += 1

            soma_partes = sum(parte.values())
            soma_empresas = sum(por_emp.values())

            if not (total == universo == soma_partes == soma_empresas):
                raise RuntimeError("soma do universo nao confere")

            for estado in estados:
                for k in particoes:
                    soma_emp = sum(
                        n for (emp, e, particao), n in por_emp.items()
                        if e == estado and particao == k
                    )
                    if soma_emp != parte[(estado, k)]:
                        raise RuntimeError("soma por empresa nao confere")

            if sum(motivos_nr.values()) != sum(
                    parte[('NAO_RECUPERAVEL', k)]
                    for k in particoes):
                raise RuntimeError("soma dos motivos nao confere")

            if bloqueios:
                raise RuntimeError("houve SQL bloqueado")

            linhas.append(
                f"UNIVERSO|perguntas_com_data_conferida_em={universo}"
            )

            for estado in estados:
                for k in particoes:
                    pks = ','.join(
                        f"#{pk}" for pk in exemplos[(estado, k)]
                    )
                    linhas.append(
                        f"PARTE|{estado}|{k}"
                        f"|perguntas={parte[(estado, k)]}"
                        f"|exemplos={pks}"
                    )

                n = sum(parte[(estado, k)] for k in particoes)
                n_emp = sum(
                    v for (emp, e, k), v in por_emp.items()
                    if e == estado
                )
                linhas.append(
                    f"TOTAL|{estado}|perguntas={n}"
                    f"|soma_das_partes={n}"
                    f"|soma_empresas={n_emp}|bate={n == n_emp}"
                )

            empresas = sorted({emp for emp, e, k in por_emp})
            for emp in empresas:
                for estado in estados:
                    for k in particoes:
                        linhas.append(
                            f"EMPRESA|emp{emp}|{estado}|{k}"
                            f"|perguntas={por_emp[(emp, estado, k)]}"
                        )

            nr = sum(
                parte[('NAO_RECUPERAVEL', k)] for k in particoes
            )
            linhas.append(
                f"NAO_RECUPERAVEL|perguntas={nr}"
                f"|perguntas_com_data_conferida_em={universo}"
            )
            for motivo, n in sorted(motivos_nr.items()):
                linhas.append(
                    f"MOTIVO_NR|{motivo}|perguntas={n}"
                )

            completo = True

except Exception as exc:
    erros += 1
    # Nao imprime mensagem da excecao: pode conter dados/SQL/parametros.
    linhas = [f"ERRO|tipo={type(exc).__name__}"]

finally:
    logging.disable(nivel_anterior)

soma_partes = sum(parte.values())
soma_empresas = sum(por_emp.values())
diferentes = sum(parte[('DIFERENTE', k)] for k in particoes)
soma_emp_diferentes = sum(
    n for (emp, estado, k), n in por_emp.items()
    if estado == 'DIFERENTE'
)
nr = sum(parte[('NAO_RECUPERAVEL', k)] for k in particoes)

bate = (
    completo
    and erros == 0
    and bloqueios == 0
    and universo == total == soma_partes == soma_empresas
    and diferentes == soma_emp_diferentes
)
medicao_integral = bate and nr == 0

for linha in linhas:
    print(linha)

print(
    f"FIM|erros={erros + bloqueios}"
    f"|sql_bloqueados={bloqueios}"
    f"|completo={completo}"
    f"|universo={universo}"
    f"|total={total}"
    f"|soma_das_partes={soma_partes}"
    f"|soma_empresas={soma_empresas}"
    f"|diferentes={diferentes}"
    f"|soma_empresas_diferentes={soma_emp_diferentes}"
    f"|nao_recuperavel={nr}"
    f"|bate={bate}"
    f"|medicao_integral={medicao_integral}"
)
```

Com `NAO_RECUPERAVEL > 0`, `DIFERENTE` informa apenas os casos demonstrados pela trilha recuperada; o total exato pedido permanece **[nao verificado]**.

## 15/11 00:34 · sessao b01514387813 · CAMINHO-ATE-E4C-RELIDO · censo · origem=Code

**(a) O “17 de 57” bate para os arquivos-alvo das F1–F9.** O inventário literal identificável é maior: **87 nomes de arquivos**, incluindo o próprio `preparo.md`; **86 sem essa autorreferência**. São **57 alvos + 30 arquivos auxiliares**, com **23 no cruzamento: 17 alvos + seis auxiliares**. Referências repetidas e caminhos relativos/absolutos do mesmo arquivo foram unificados. `preparo.md:3–23`, `preparo.md:90–557`, `preparo.md:582`, `preparo.md:609–617`; `diff_nomes.txt:1–115`.

A menção abreviada `RELATO/TICKETS` não identifica arquivos inequivocamente; sua expansão e eventual acréscimo ao total/cruzamento ficam **[nao verificado]**. `preparo.md:564`.

Abaixo, **† identifica os 23 arquivos do cruzamento**. Nos agrupamentos, o diretório indicado aplica-se a cada nome enumerado.

- **F1 — 1:** `app/chamados/juizes.py`†. `preparo.md:90–139`.
- **F2 — 1:** `app/chamados/models.py`†. `preparo.md:140–207`.
- **F3 — 5:** em `app/chamados/services/`: `validacao.py`†, `verbos.py`, `painel.py`, `acoes_disputa.py`†; mais `app/chamados/veredito_lavrado.py`. `preparo.md:208–261`.
- **F4 — 5:** em `app/chamados/services/`: `disputa_emissao.py`†, `materializacao.py`†, `relavra_via.py`, `passivo_validado.py`, `fabrica_da_celula.py`. `preparo.md:262–323`.
- **F5 — 14:** `app/chamados/reconciliador.py`; em `app/chamados/services/`: `fio.py`†, `explicador.py`†, `declaracao_texto.py`, `vigia_fio.py`†, `raiox.py`†, `tarefa_do_dia.py`†, `cobranca.py`†, `pergunta_no_app.py`†, `becos.py`, `furo_sem_canal.py`†, `historico.py`, `competencia_trancada.py`, `vigia_carimbo.py`. `preparo.md:324–391`.
- **F6 — 7:** `app/api/views.py`†, `app/api/views_mensageria.py`, `app/chamados/views_cobrar.py`, `app/chamados/views.py`, `app/chamados/relatorios_disputa.py`†, `app/chamados/canal.py`, `app/chamados/templatetags/chamados_tags.py`. `preparo.md:392–437`.
- **F7 — 4:** em `app/templates/chamados/partials/`: `_card_atendimento.html`†, `modal_fio.html`, `_pergunta_card.html`, `_lote_preview.html`. `preparo.md:438–468`.
- **F8 — 8:** em `app/ponto/services/`: `procedencia_batida.py`, `flip_auto.py`, `cartorio.py`, `veredito_celula.py`, `dia_do_colab.py`; em `app/colaboradores/services/`: `calendario.py`, `ficha.py`; mais `app/folha/previa.py`. `preparo.md:469–502`.
- **F9 — 12:** em `app/chamados/management/commands/`: `auditar_invariantes_chamados.py`, `reabrir_orfas_enterradas.py`, `backfill_decisoes_pergunta.py`, `encerrar_perguntas_com_fato.py`, `auditar_disputas.py`, `relavrar_via_i6.py`, `curar_perguntas_em_limbo.py`, `migrar_data_evento_dia_do_turno.py`, `dedup_perguntas_fato.py`, `validar_classe_a.py`; em `app/ponto/management/commands/`: `reconciliar_fantasmas.py`, `reconciliar_grade.py`. `preparo.md:503–557`.

Os **30 adicionais** são:

| Quantidade | Arquivos citados | Referência |
|---:|---|---|
| 7 | `logs/AVAIS-1313/prepara_a_espinha_com_agentes.txt`; `logs/PREPARACAO/censos/E4c/fatias.md`; `logs/E4C/censo.r`; `logs/E4C/aval_expand_contract.txt`; `logs/AVAIS-1313/sexta_noite_1311.txt`; `app/docs/LEIS.md`†; `app/docs/BACKLOG.md`† | `preparo.md:3–10` |
| 1 | `manage.py` — caminho exato **[nao verificado]** | `preparo.md:4` |
| 5 | `censo_ast.py`; `e3/ast.tsv`; `main/ast.tsv`; `unidades.tsv`; `unid_dest.tsv` | `preparo.md:17–19`, `preparo.md:615–617` |
| 2 | `app/core/juizes.py`†; `app/docs/JUIZES.md`† | `preparo.md:40` |
| 2 | `logs/E4C/particao_volta1.txt`; `app/chamados/tests/test_e4_escritores.py` | `preparo.md:71` |
| 1 | `CLAUDE.md` | `preparo.md:81` |
| 1 | `arquivos_em_voo.txt` | `preparo.md:93` |
| 2 | `app/chamados/services/atos_pergunta.py`; `logs/FIO-E0-V4-E4-ESCRITORES/volta2/E4A_PARQUE.txt` | `preparo.md:144–145` |
| 3 | `app/chamados/tests/test_e3_pendentes_app.py`†; `app/templates/chamados/detalhe_local.html`; `app/templates/chamados/meu_atendimento.html` | `preparo.md:441–444` |
| 3 | `app/ponto/services/efeito_ausencia.py`; `app/escala/services/leitor_celula.py`; `app/ponto/motor_calculo_v2.py` | `preparo.md:472` |
| 1 | `app/docs/CORTES.md`† | `preparo.md:582` |
| 1 | `logs/FIO-E0-V4-E4B/remedida.r` | `preparo.md:609` |
| 1 | `preparo.md`, autorreferência | `preparo.md:614` |

Portanto, **não sobram nem faltam arquivos no conjunto dos 17 alvos assinalados pelo preparo**. Para representar todos os nomes identificáveis citados, faltariam **30 no denominador e seis no numerador**: `core/juizes.py`, `docs/BACKLOG.md`, `docs/CORTES.md`, `docs/JUIZES.md`, `docs/LEIS.md` e `chamados/tests/test_e3_pendentes_app.py`, todos sob `app/`. `preparo.md:20–23`; `diff_nomes.txt:2`, `diff_nomes.txt:46`, `diff_nomes.txt:76–85`, `diff_nomes.txt:113`.

**(b) Tabela do cruzamento.** Os commits abaixo são os relacionados **ao arquivo** no anexo. Atribuir uma alteração de sítio a um commit individual, sem seu diff de conteúdo, permanece **[nao verificado]**. A numeração dos anexos começa na primeira linha de seus respectivos corpos. [nao verificado]

| Fatia | Arquivo | Commit(s) do arquivo | Veredito sobre a fatia | Arquivo:linha na cópia | [nao verificado]
|---|---|---|---|---|
| F1 | `app/chamados/juizes.py` | `0a567a225`, `be856fb8b`, `0544c994d`, `ae9989de1`. `diff_commits_por_arquivo.txt:5–8` | **Mantém SEGUE VALIDA:** predicado e faces SQL ainda leem projeção; a fila usa a face SQL, mas ordena pelo carimbo. | `app/chamados/juizes.py:1528`, `app/chamados/juizes.py:1548`, `app/chamados/juizes.py:1556`, `app/chamados/juizes.py:1722`, `app/chamados/juizes.py:1724` |
| F2 | `app/chamados/models.py` | `32f4192de`, `0a567a225`, `5319676ee`, `0544c994d`, `cb7064355`, `99ed67ff0`, `c70b7511c`, `ae66e55af`, `53712af62`. `diff_commits_por_arquivo.txt:27–35` | **Mantém SEGUE VALIDA** para estado, seleção e apresentação. **Mantém MUDA** para `_nascer`: guarda de entrada do escritor. FE conserva **carimbo OU texto**; conservação E4b continua deliberadamente na projeção. | `app/chamados/models.py:3124`, `app/chamados/models.py:3454`, `app/chamados/models.py:3641`, `app/chamados/models.py:3664`, `app/chamados/models.py:2922`, `app/chamados/models.py:1906–1909`, `app/chamados/models.py:4067` |
| F3 | `app/chamados/services/validacao.py` | `0a567a225`, `f1fe1d367`. `diff_commits_por_arquivo.txt:88–89` | **Mantém SEGUE VALIDA** para guardas R/V. **Mantém SOME** a pendência de impedir recusa fora do fato: a porta já retorna antes da escrita e reabre com `ato='recusa'`. | `app/chamados/services/validacao.py:602`, `app/chamados/services/validacao.py:605`, `app/chamados/services/validacao.py:609`, `app/chamados/services/validacao.py:613`, `app/chamados/services/validacao.py:628` |
| F3 | `app/chamados/services/acoes_disputa.py` | `f1fe1d367`. `diff_commits_por_arquivo.txt:39` | **Mantém SEGUE VALIDA** para a guarda crua. **Mantém SOME** o tratamento pendente da recusa fora do fato: já vira informação. `recusa_motivo` continua sendo POST. | `app/chamados/services/acoes_disputa.py:95`, `app/chamados/services/acoes_disputa.py:81`, `app/chamados/services/acoes_disputa.py:85` |
| F4 | `app/chamados/services/disputa_emissao.py` | `f1fe1d367`, `99ed67ff0`. `diff_commits_por_arquivo.txt:43–44` | **Mantém SEGUE VALIDA** para leitura R/V/C. **Mantém SOME** a unificação pendente da reabertura, já feita pela porta do lote. **Mantém MUDA** o “reconferir depois da cura” do automático: a guarda já existe. | `app/chamados/services/disputa_emissao.py:1621–1623`, `app/chamados/services/disputa_emissao.py:1634`, `app/chamados/services/disputa_emissao.py:1657` |
| F4 | `app/chamados/services/materializacao.py` | `f1fe1d367`. `diff_commits_por_arquivo.txt:65` | **Mantém SEGUE VALIDA** para o texto da resposta; permanecem os sítios técnicos A18/A06/A20 identificados na volta 1. | `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:1095`, `app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1199` |
| F5 | `app/chamados/services/fio.py` | `cb7064355`, `046d7c085`, `99ed67ff0`, `ae66e55af`, `513ff8077`, `de125d5e1`. `diff_commits_por_arquivo.txt:48–53` | **Mantém SEGUE VALIDA:** espera do texto, guarda do POST e mérito da duplicata ainda leem R. | `app/chamados/services/fio.py:187`, `app/chamados/services/fio.py:698`, `app/chamados/services/fio.py:1271`, `app/chamados/services/fio.py:1283` |
| F5 | `app/chamados/services/explicador.py` | `ae66e55af`. `diff_commits_por_arquivo.txt:46` | **Mantém SEGUE VALIDA:** apresentação ainda lê R; permanecem pendência, recusa técnica e identidade. | `app/chamados/services/explicador.py:79`, `app/chamados/services/explicador.py:85`, `app/chamados/services/explicador.py:90`, `app/chamados/services/explicador.py:95` |
| F5 | `app/chamados/services/vigia_fio.py` | `aa0e27c11`, `b4a87aecb`, `52110c313`, `660ee8805`, `0a567a225`, `456f0c6ad`, `be856fb8b`, `2b0739004`, `3c55819f0`, `cb7064355`, `ae9989de1`, `992be9c60`. **Correção:** os dois últimos blocos desta lista pertencem ao histórico? **[nao verificado]** | **Mantém SEGUE VALIDA** para R em `censo_alcance`. **Mantém MUDA** para `respondida_sem_resposta`: mede o par escrito desde um marco; equivalência com contagem de atos vazios **[nao verificado]**. | `app/chamados/services/vigia_fio.py:694`, `app/chamados/services/vigia_fio.py:795–807` |
| F5 | `app/chamados/services/raiox.py` | `0f2b6dd47`. `diff_commits_por_arquivo.txt:72` | **Mantém SEGUE VALIDA:** crédito ainda lê projeção e distingue carimbo **e** texto. | `app/chamados/services/raiox.py:135–144` |
| F5 | `app/chamados/services/tarefa_do_dia.py` | `1ee368261`, `b1acb7985`, `aa0e27c11`, `b4a87aecb`, `52110c313`, `660ee8805`, `456f0c6ad`, `be856fb8b`, `2b0739004`, `3c55819f0`, `e01da1d51`, `cdf9ae7ee`, `992be9c60`. `diff_commits_por_arquivo.txt:74–86` | **Mantém MUDA** a referência de T e a porta calculada pela âncora na fila. A06 permanece na leitura da via. | `app/chamados/services/tarefa_do_dia.py:131`, `app/chamados/services/tarefa_do_dia.py:142`, `app/chamados/services/tarefa_do_dia.py:245` |
| F5 | `app/chamados/services/cobranca.py` | `cb7064355`. `diff_commits_por_arquivo.txt:41` | **Mantém MUDA** o censo indireto: `_pendentes` lê a consulta; `_calados` lê espera do texto **e** P. | `app/chamados/services/cobranca.py:47–48`, `app/chamados/services/cobranca.py:241` |
| F5 | `app/chamados/services/pergunta_no_app.py` | `1ee368261`, `b1acb7985`, `52110c313`, `be856fb8b`. `diff_commits_por_arquivo.txt:67–70` | **Mantém SOME** o predicado próprio da consulta, cuja função permanece delegando. **Mantém SEGUE VALIDA** a migração da partição e **MUDA** o RED: saídas são `pendentes`/`historico`. | `app/chamados/services/pergunta_no_app.py:64`, `app/chamados/services/pergunta_no_app.py:73`, `app/chamados/services/pergunta_no_app.py:81–85` |
| F5 | `app/chamados/services/furo_sem_canal.py` | `aa0e27c11`, `b4a87aecb`, `52110c313`, `660ee8805`, `85daa9f14`, `be856fb8b`, `3c55819f0`, `cb7064355`, `ae9989de1`. `diff_commits_por_arquivo.txt:55–63` | **Mantém SOME** o sítio cru. `medir` permanece lendo tarefa e porta publicada; sem canal é ausência dessa porta. | `app/chamados/services/furo_sem_canal.py:43`, `app/chamados/services/furo_sem_canal.py:96–104` |
| F6 | `app/api/views.py` | `80bf4354a`, `de125d5e1`. `diff_commits_por_arquivo.txt:2–3` | **Mantém SEGUE VALIDA** para apresentação/guardas e `aguarda_admin`. **Mantém SOME** a derivação crua da correção, já delegada. | `app/api/views.py:1159–1161`, `app/api/views.py:1175`, `app/api/views.py:2549`, `app/api/views.py:2557`, `app/api/views.py:2616` |
| F6 | `app/chamados/relatorios_disputa.py` | `cb7064355`. `diff_commits_por_arquivo.txt:37` | **Mantém SOME** o predicado próprio; `classificar` permanece lendo FE e `aguarda_o_colaborador`. | `app/chamados/relatorios_disputa.py:11`, `app/chamados/relatorios_disputa.py:24–26` |
| F7 | `app/templates/chamados/partials/_card_atendimento.html` | `cb7064355`. `diff_commits_por_arquivo.txt:404` | **Mantém SEGUE VALIDA** para apresentação R; **SOME** o julgamento cru de envio, já delegado a `resposta_enviada`; **MUDA** a referência da mistura técnica para 189/192. | `app/templates/chamados/partials/_card_atendimento.html:100`, `app/templates/chamados/partials/_card_atendimento.html:106`, `app/templates/chamados/partials/_card_atendimento.html:114`, `app/templates/chamados/partials/_card_atendimento.html:189–192` |
| F2/D01; F5 — apoio | `app/core/juizes.py` | `0a567a225`, `992be9c60`. `diff_commits_por_arquivo.txt:305–306` | **Mantém SEGUE VALIDA** a distinção C17/C19. Registro mantém ambos; comentário documenta a migração dos chamadores para a porta de T. | `app/core/juizes.py:135–138`, `app/core/juizes.py:159`, `app/core/juizes.py:297–299` |
| F7 — apoio | `app/chamados/tests/test_e3_pendentes_app.py` | `992be9c60`. `diff_commits_por_arquivo.txt:209` | **Mantém MUDA:** é catraca C17, com um chamador permitido. **SOME** essa referência como prova da alegada catraca E17 de templates. | `app/chamados/tests/test_e3_pendentes_app.py:21`, `app/chamados/tests/test_e3_pendentes_app.py:118`, `app/chamados/tests/test_e3_pendentes_app.py:145–148` |
| Transversal; F5 — nova leitura | `app/docs/LEIS.md` | `183d3d27a`, `a3145c9ba`. `diff_commits_por_arquivo.txt:328–329` | **SEGUE VALIDA** L-127. **MUDA** o contexto documental do fio: L-129 já está registrada e determina leitura do fio único de T pela Folha. | `app/docs/LEIS.md:148–149` |
| F3/F6 — nova leitura | `app/docs/BACKLOG.md` | `183d3d27a`, `b12e86556`. `diff_commits_por_arquivo.txt:310–311` | **SEGUE VALIDA** D04/D05: atos humanos, fases conservadas, exigibilidade aditiva e `aguarda_admin` até o efeito. Referência atual é 1999; a ordem mantém censo rerodado. | `app/docs/BACKLOG.md:1999`, `app/docs/BACKLOG.md:9` |
| F2/D01; F5 — nova leitura | `app/docs/JUIZES.md` | `0a567a225`, `be856fb8b`, `4112a571e`. `diff_commits_por_arquivo.txt:324–326` | **SEGUE VALIDA** T como autoridade por instância/DP. O documento é lookup; D01 não aparece no bloco de chamados do registro lido. | `app/docs/JUIZES.md:7–12`, `app/docs/JUIZES.md:78–87`; `app/core/juizes.py:280–304` |
| F3/F4/F7 — A20; nova leitura | `app/docs/CORTES.md` | `b12e86556`. `diff_commits_por_arquivo.txt:315` | **MUDA** a afirmação de pergunta A20 ainda sem resposta: LU-08 escolhe que a conferência vale só para o par conferido. BACKLOG conserva a redação antiga. Implementação dessa escolha **[nao verificado]**. | `app/docs/CORTES.md:440`; `app/docs/BACKLOG.md:1975` |

**Correção da célula de commits de `vigia_fio.py`:** a lista fornecida no anexo é exatamente `aa0e27c11`, `b4a87aecb`, `52110c313`, `660ee8805`, `0a567a225`, `456f0c6ad`, `be856fb8b`, `e01da1d51`, `cdf9ae7ee`, `992be9c60`? **[nao verificado]**

**(c) F8 e F9: nenhum arquivo-alvo está no diff — 0/8 e 0/12.** Os comandos alterados em `diff_nomes.txt:3–6` têm nomes diferentes dos comandos enumerados na F9; não entram nela por serem comandos. `preparo.md:469–557`; `diff_nomes.txt:1–115`.

## 15/11 00:32 · sessao 76caf82e94c3 · - · - · origem=Ronald (tmux)

(a) O par conferido é recuperável **quando existe a auditoria correspondente**, por dados estruturados; sua existência em produção é [nao verificado]. O escritor fornece ambos os dias ao log em `app/chamados/services/validacao.py:702-707`.

- Modelo: `PerguntaDisputa.data_conferida_em` é `DateTimeField(null=True, blank=True)` em `app/chamados/models.py:2536`; `data_conferida_por` é FK nullable para `auth.User`, com `SET_NULL`, em `app/chamados/models.py:2537-2540`.
- Único escritor explícito atual dos carimbos: `conferir_data_do_fato`, atribuindo `timezone.now()` e `autor`, e salvando somente esses dois campos, em `app/chamados/services/validacao.py:699-701`. O contrato P13 declara essa unicidade em `app/core/tests/test_contract_escritores_estado.py:113-117`.
- A view apenas verifica POST/permissão e delega; não constitui outro escritor dos campos: `app/chamados/views.py:1306-1314`.
- A porta exige justificativa, consulta `data_do_fato_de(p)`, recusa concordância e recusa pergunta já carimbada: `app/chamados/services/validacao.py:689-698`.
- Junto ao carimbo, solicita `LogAuditoria` com `acao='conferir_data'`, `modelo='PerguntaDisputa'`, `objeto_id=p.pk`; descrição contendo ambos os dias e justificativa; `valor_antes={'data_turno': turno.isoformat() if turno else None}` e `valor_depois={'data_evento': ev.isoformat() if ev else None, 'justificativa': just}`: `app/chamados/services/validacao.py:702-707`.
- Os JSONs residem em `LogAuditoria.valor_antes/valor_depois`, com `objeto_id`, `usuario` e `criado_em`: `app/core/models.py:295-307`. `registrar_log` encaminha os payloads sem reconstruí-los em `app/core/auditoria.py:22-24`; `evento` os grava em `app/core/observ.py:133-138`.
- **Carimbo não garante log:** auditoria é best-effort e engole falhas, em `app/core/auditoria.py:25-29` e `app/core/observ.py:105-139`; o save do carimbo ocorre antes da chamada de auditoria, em `app/chamados/services/validacao.py:701-702`.
- `DecisaoPergunta` não possui ato de conferência nem colunas para esse par: vocabulário em `app/chamados/models.py:4000-4011`, campos em `app/chamados/models.py:4014-4027`. A porta de conferência não chama `DecisaoPergunta.registrar`: seu corpo está em `app/chamados/services/validacao.py:683-710`.
- Os campos de conferência não entram em `CAMPOS_QUE_ACORDAM`, em `app/chamados/models.py:3303-3305`; por isso o receiver de outbox retorna nesse save específico, conforme `app/chamados/signals.py:257-259`.

Para a sonda, buscar os logs por `(acao='conferir_data', modelo='PerguntaDisputa', objeto_id=p.pk)`, recuperar o par ordenado `(valor_depois['data_evento'], valor_antes['data_turno'])` e comparar com o par vigente. Ausência, payload inválido ou pares conflitantes devem ficar em `NAO_RECUPERAVEL`; escolher arbitrariamente o log mais recente não resolve a ambiguidade [nao verificado]. Não é possível exigir igualdade entre `LogAuditoria.criado_em` e `data_conferida_em`: o primeiro é `auto_now_add` em `app/core/models.py:307`, enquanto o segundo é calculado antes, em `app/chamados/services/validacao.py:699-702`.

## 15/11 00:32 · sessao 831e9043d8e0 · - · - · origem=Ronald (tmux)

O par vigente é **`(ev, turno)` devolvido por `data_do_fato_de(p)`**, e não simplesmente duas colunas da pergunta: `ev = p.data_evento`; `turno = dia_do_fato(chamado_que_lista(p), fortes=True)`; a divergência é `bool(ev and turno and ev != turno)`. (`app/chamados/services/fio.py:77-94`)

- `PerguntaDisputa.data_evento` é `DateField` anulável. (`app/chamados/models.py:2501-2504`)
- O chamado usado é `p.chamado_destino`, com fallback para `p.disputa.chamado`. (`app/chamados/services/fio.py:116-117`)
- O dia forte desse chamado vem primeiro de **`chamado.celula.data`**; depois da chave declarada pelo catálogo, com fallback para `contexto_json['data_turno']`. Com `fortes=True`, ausência de dia forte retorna `None`, sem consultar batidas/carimbos. (`app/chamados/juizes.py:98-128`)
- O materializador lê esse mesmo par, mas libera a divergência verificando somente se `p.data_conferida_em` está preenchida. (`app/chamados/services/materializacao.py:1197-1199`)
- A porta de conferência lê exatamente esse par e o grava na auditoria: `valor_antes['data_turno'] = turno.isoformat()` e `valor_depois['data_evento'] = ev.isoformat()`, além da justificativa. Portanto existe uma fonte histórica possível; sua presença e integridade em produção estão **[nao verificado]**. (`app/chamados/services/validacao.py:693-707`)

A partição do anexo **cabe neste pedido**: chamar `competencia_do_dia_entregue(p.data_evento, p.colaborador.empresa_id, memo)`. A função retorna `None` sem dia; nos demais casos usa a janela da empresa e o mês/ano do **fim** da janela para consultar K6. (`app/chamados/services/vigia_fio.py:855-865`; `app/chamados/models.py:2511`)

- `True` → `exportada`; `False` → `aberta`; `None` → `sem_data`, preservando a parte própria do anexo. A função retorna esses três estados; o tratamento de `None` pertence ao chamador. (`app/chamados/services/vigia_fio.py:843-865`)
- `janela_atual` respeita o corte da empresa; aceita também seu PK, consultando `dia_inicio_competencia`. (`app/ponto/janelas.py:42-63`)
- K6 exige `ExportacaoDominio` vigente e não invalidada. “Aberta”, nessa medição, significa o resultado falso desse juiz, sem depender do estado da disputa. (`app/folha/export.py:580-594`)

## 15/11 00:32 · sessao ee221ef3acda · - · - · origem=Ronald (tmux)

Numeração do anexo, iniciada em `# E4c...` como linha 1:

| Referência | Linha de `preparo.md` |
|---|---:|
| F1 | 90 |
| F2 | 140 |
| F3 | 208 |
| F4 | 262 |
| F5 | 324 |
| F6 | 392 |
| F7 | 438 |
| F8 | 469 |
| F9 | 503 |
| `arquivos_em_voo.txt` | 93 |
| `atos_pergunta.py` | 144 |
| `E4A_PARQUE.txt` | 145 |
| `test_e3_pendentes_app.py` | 441 |
| `detalhe_local.html` | 443 |
| `meu_atendimento.html` | 444 |
| `efeito_ausencia.py`, `leitor_celula.py`, `motor_calculo_v2.py` | 472 |
| `CORTES.md` | 582 |
| `remedida.r` | 609 |
| Autorreferência `preparo.md` | 614 |
| TSV absolutos | 615–617 |

**Correção do censo anterior:** a leitura literal acrescenta `manage.py` (`preparo.md:4`) e `logs/FIO-E0-V4-E4B/remedida.r` (`preparo.md:609`). Portanto são **87 nomes de arquivos citados = 57 alvos + 30 adicionais**, incluindo a autorreferência; **86 sem a autorreferência**. O cruzamento permanece **23**: 17 alvos + seis apoios. `preparo.md:4`, `preparo.md:20`, `preparo.md:609`, `preparo.md:614`; `diff_nomes.txt:1`.

A identidade de `manage.py` é [nao verificado]: a cópia contém `app/manage.py` e `mensageria/manage.py`, e o preparo usa apenas o basename. `RELATO/TICKETS` em `preparo.md:564` não identifica caminhos inequivocamente; não somei arquivos adicionais por essa menção.

## 15/11 00:31 · sessao 14c61e9af45a · E3-RESTO-R3 · construtor · origem=Code

Entrega registrada com a tabela dos 42 casos, censos, RED/GREEN, comandos, mutantes e R5: [e3_r3_c5_entrega.txt](/home/ronald/wt-codex-e3-resto-r3-231625/app/chamados/tests/e3_r3_c5_entrega.txt:1).

Sem banco: **18 + 168 testes verdes; 32/32 mutantes mortos**. Permanecem **quatro falhas de cores**; ORM não foi executado. [Evidências](/home/ronald/wt-codex-e3-resto-r3-231625/app/chamados/tests/e3_r3_c5_entrega.txt:98). [nao verificado]

Autoriza acrescentar somente o token `laranja` no partial da barra? A proposta passou nos 121 testes dos leitores em memória. [Proposta concreta](/home/ronald/wt-codex-e3-resto-r3-231625/app/chamados/tests/e3_r3_c5_entrega.txt:137). [nao verificado]

A confirmação é necessária porque seu pedido proíbe editar templates. Mantive o template e os selos intactos; também preservei `config/crons.py` pela proibição inicial. [Pendências](/home/ronald/wt-codex-e3-resto-r3-231625/app/chamados/tests/e3_r3_c5_entrega.txt:140).

## 15/11 00:24 · sessao ee221ef3acda · - · - · origem=Ronald (tmux)

Censo normalizado do anexo: **57 arquivos nas F1–F9**, mais **28 arquivos de apoio/autorreferência = 85 arquivos citados**. Sem contar a autorreferência ao próprio `preparo.md`, são 84. As referências relativas/absolutas às mesmas saídas TSV foram unificadas. [nao verificado: anexo fornecido no texto, sem arquivo local numerado]

| Fatia | Quantidade |
|---|---:|
| F1 | 1 |
| F2 | 1 |
| F3 | 5 |
| F4 | 5 |
| F5 | 14 |
| F6 | 7 |
| F7 | 4 |
| F8 | 8 |
| F9 | 12 |
| Total | 57 |

[nao verificado: cálculo sobre o anexo textual]

Os **17 alvos cruzados** são exatamente os 17 da volta 1: `app/api/views.py`; `app/chamados/juizes.py`; `app/chamados/models.py`; `app/chamados/relatorios_disputa.py`; `app/chamados/services/acoes_disputa.py`; `cobranca.py`; `disputa_emissao.py`; `explicador.py`; `fio.py`; `furo_sem_canal.py`; `materializacao.py`; `pergunta_no_app.py`; `raiox.py`; `tarefa_do_dia.py`; `validacao.py`; `vigia_fio.py`; `app/templates/chamados/partials/_card_atendimento.html` (os basenames intermediários ficam em `app/chamados/services/`). [nao verificado: cruzamento dos anexos textuais]

Os **28 adicionais citados** são: [nao verificado: inventário do anexo textual]

1. `logs/AVAIS-1313/prepara_a_espinha_com_agentes.txt`
2. `logs/PREPARACAO/censos/E4c/fatias.md`
3. `logs/E4C/censo.r`
4. `logs/E4C/aval_expand_contract.txt`
5. `app/docs/LEIS.md`
6. `logs/AVAIS-1313/sexta_noite_1311.txt`
7. `app/docs/BACKLOG.md`
8. `censo_ast.py`
9. `e3/ast.tsv`
10. `main/ast.tsv`
11. `unidades.tsv`
12. `unid_dest.tsv`
13. `app/core/juizes.py`
14. `app/docs/JUIZES.md`
15. `logs/E4C/particao_volta1.txt`
16. `CLAUDE.md`
17. `arquivos_em_voo.txt`
18. `app/chamados/services/atos_pergunta.py`
19. `logs/FIO-E0-V4-E4-ESCRITORES/volta2/E4A_PARQUE.txt`
20. `app/chamados/tests/test_e4_escritores.py`
21. `app/chamados/tests/test_e3_pendentes_app.py`
22. `app/templates/chamados/detalhe_local.html`
23. `app/templates/chamados/meu_atendimento.html`
24. `app/ponto/services/efeito_ausencia.py`
25. `app/escala/services/leitor_celula.py`
26. `app/ponto/motor_calculo_v2.py`
27. `app/docs/CORTES.md`
28. `preparo.md`, autorreferência final.

Destes, **seis** também aparecem no diff: `app/core/juizes.py`, `app/docs/BACKLOG.md`, `app/docs/LEIS.md`, `app/docs/JUIZES.md`, `app/docs/CORTES.md`, `app/chamados/tests/test_e3_pendentes_app.py`. Portanto **o cruzamento literal completo é 23/85**, e **o cruzamento dos alvos das fatias é 17/57**. O “17 de 57” bate no seu escopo; para “todos os arquivos citados”, faltam os 28 de apoio no denominador e seis deles no numerador. [nao verificado: cruzamento dos anexos textuais]

F8 e F9: **nenhum dos seus 8+12 arquivos-alvo** está em `diff_nomes.txt`; os comandos alterados no diff possuem outros nomes e não devem ser assimilados aos comandos da F9. [nao verificado: comparação dos anexos textuais]

## 15/11 00:24 · sessao b51159c45936 · - · - · origem=Ronald (tmux)

| Fatia | Arquivo | Commit(s) no anexo por arquivo | Veredito | Prova em `4b75cba10` |
|---|---|---|---|---|
| Transversal; F3/F6 | `app/docs/LEIS.md` | `183d3d27a`, `a3145c9ba` — `diff_commits_por_arquivo.txt` [nao verificado quanto à linha específica] | **SEGUE VALIDA** L-127, Expand-Contract. **MUDA** o contexto do fio: L-129 já está registrada. | `app/docs/LEIS.md:148`; `app/docs/LEIS.md:149` |
| Transversal; F3/F6 | `app/docs/BACKLOG.md` | `183d3d27a`, `b12e86556` — anexo [nao verificado quanto à linha específica] | **SEGUE VALIDA** D04 = atos humanos; D05 conserva fases, exigibilidade aditiva e `aguarda_admin` até efeito. Referência corrigida para `:1999`. Ordem exige censo rerodado. | `app/docs/BACKLOG.md:1999`; `app/docs/BACKLOG.md:9` |
| F2/D01; F5 | `app/docs/JUIZES.md` | `0a567a225`, `be856fb8b`, `4112a571e` — anexo [nao verificado quanto à linha específica] | **SEGUE VALIDA** T como autoridade por instância e DP; sua porta é a fonte dos leitores migrados. O documento declara-se lookup e remete ao registro vivo. D01 não aparece na tabela lida, nem no registro de chamados. | `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:78`; `app/docs/JUIZES.md:80`; `app/docs/JUIZES.md:85`; `app/docs/JUIZES.md:87`; `app/core/juizes.py:280` |
| F2/D01; F5 | `app/core/juizes.py` | `0a567a225`, `992be9c60` — anexo [nao verificado quanto à linha específica] | **SEGUE VALIDA** a distinção C17/C19 já apresentada na volta 1. Registro conserva C17 e C19 separados; comentário fixa migração dos chamadores para `porta_da_tarefa`. D01 segue sem entrada no bloco de chamados. | `app/core/juizes.py:135`; `app/core/juizes.py:138`; `app/core/juizes.py:159`; `app/core/juizes.py:297`; `app/core/juizes.py:299`; `app/core/juizes.py:302` |
| F3/F4/F7, exceção A20; seção 10 | `app/docs/CORTES.md` | `b12e86556` — anexo [nao verificado quanto à linha específica] | **MUDA** a afirmação do preparo de que resta uma pergunta de lei A20 não respondida: LU-08 já escolhe “conferência vale só para o par conferido”. A regra não comprova implementação; esta permanece [nao verificado]. BACKLOG conserva a formulação antiga. | `app/docs/CORTES.md:440`; `app/docs/BACKLOG.md:1975` |
| F7, apoio | `app/chamados/tests/test_e3_pendentes_app.py` | `992be9c60` — anexo [nao verificado quanto à linha específica] | Mantém **MUDA** da volta 1: o preparo atribui E17 de templates a um teste que efetivamente limita chamadores C17. **SOME** esta referência como prova de catraca de templates. | `app/chamados/tests/test_e3_pendentes_app.py:21`; `app/chamados/tests/test_e3_pendentes_app.py:118`; `app/chamados/tests/test_e3_pendentes_app.py:145`; `app/chamados/tests/test_e3_pendentes_app.py:148` |

Nenhuma dessas referências permite atribuir um trecho individual ao commit informado: o anexo fornece histórico por arquivo, sem diff de conteúdo. [nao verificado] [nao verificado]

## 15/11 00:23 · sessao 1e80124d62b9 · - · - · origem=Ronald (tmux)

| Fatia | Arquivo | Manutenção da volta 1 | Evidência em `4b75cba10` |
|---|---|---|---|
| F1 | `app/chamados/juizes.py` | **Mantém SEGUE VALIDA**: predicado e faces SQL ainda leem projeção; fila usa a face SQL e ordena pelo carimbo. | `app/chamados/juizes.py:1528`, `app/chamados/juizes.py:1548`, `app/chamados/juizes.py:1556`, `app/chamados/juizes.py:1722`, `app/chamados/juizes.py:1724` |
| F2 | `app/chamados/models.py` | **Mantém SEGUE VALIDA** para estado, queryset e apresentação; **mantém MUDA** para `_nascer`, cuja leitura é guarda de entrada do escritor. FE conserva ausência de V/C e **carimbo OU texto**, respeitando tranca; conservação E4b segue deliberadamente na projeção. | `app/chamados/models.py:3124`, `app/chamados/models.py:3454`, `app/chamados/models.py:3468`, `app/chamados/models.py:3641`, `app/chamados/models.py:3664`, `app/chamados/models.py:2922`, `app/chamados/models.py:1906`, `app/chamados/models.py:1907`, `app/chamados/models.py:1909`, `app/chamados/models.py:4067` |
| F3 | `app/chamados/services/validacao.py` | **Mantém SEGUE VALIDA** para guardas R/V; **mantém SOME** para o trabalho pendente de impedir recusa fora do fato: já retorna antes da escrita e chama a reabertura com `ato='recusa'`. | `app/chamados/services/validacao.py:602`, `app/chamados/services/validacao.py:605`, `app/chamados/services/validacao.py:609`, `app/chamados/services/validacao.py:613`, `app/chamados/services/validacao.py:628` |
| F3 | `app/chamados/services/acoes_disputa.py` | **Mantém SEGUE VALIDA** para migração da guarda crua de pendência; **mantém SOME** para tratamento pendente da recusa fora do fato: já traduz para informação. `recusa_motivo` continua sendo POST. | `app/chamados/services/acoes_disputa.py:95`, `app/chamados/services/acoes_disputa.py:81`, `app/chamados/services/acoes_disputa.py:85` |
| F4 | `app/chamados/services/disputa_emissao.py` | **Mantém SEGUE VALIDA** para leituras R/V/C; **mantém SOME** para unificação da reabertura, já na porta do lote; **mantém MUDA** para “reconferir depois da cura” do automático: a guarda já está implementada. | `app/chamados/services/disputa_emissao.py:1621`, `app/chamados/services/disputa_emissao.py:1623`, `app/chamados/services/disputa_emissao.py:1634`, `app/chamados/services/disputa_emissao.py:1592`, `app/chamados/services/disputa_emissao.py:1649`, `app/chamados/services/disputa_emissao.py:1657` |
| F4 | `app/chamados/services/materializacao.py` | **Mantém SEGUE VALIDA** para migração do texto; preservam-se os sítios técnicos A18/A06/A20 citados na volta 1. | `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:1095`, `app/chamados/services/materializacao.py:1099`, `app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1199` |

_5440 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
