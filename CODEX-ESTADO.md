# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 17:58** · publicado em **13/11/2026 17:59** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 17:58 · sessao 998bf5247530 · - · - · origem=Ronald (tmux)

LEU = código/documento da cópia. INFERIU = comparação dos anexos com esse código. Os diffs anexos não têm arquivo local verificável; suas mudanças são “LEU anexo; [não verificado no HEAD]” até a comparação abaixo.

**Resultado medido da comparação.** Calculei em memória o SHA1 no formato de blob Git dos 21 arquivos de produção dos diffs 164050/215253: 20 são byte a byte iguais ao lado anterior de pelo menos um dos diffs. A exceção é `app/folha/export.py`, cujo arquivo diverge, mas o hunk de `prontidao` continua igual (`app/folha/export.py:1095`). O HEAD de `ausencia.py` e `cartorio.py` coincide com a base da 164050, enquanto a 215253 os tinha rebasedos em bases diferentes. Não foi realizado merge ou apply; conflito mecânico de merge é [não verificado].

| Raia | O que resolve no anexo | Situação lida no HEAD | Sobreposição/conflito |
|---|---|---|---|
| `lastro3` rescaldo | Troca recuo linear pelo juiz do par; extrai compositor `grade_do_julgamento`; exige papel/cadeia, trata noturno e final versus intervalo; elimina tolerância local. | Pendência continua: `TOLERANCIA_MIN` em `lastro.py:59`; recuo próprio em `:299`, comparação por tipo/instante linear em `:307`; grade montada inline no cartório em `cartorio.py:1156` e `:1175`. | **Parte superada:** o diff altera `fechar()` e `--apply`, que não são a porta atual. Hoje comando é vigia (`commands/fechar_cobranca_com_lastro.py:20`, `:44`) e escritor por evento é `lastro.py:364`. Builder exige `agora=` (`escala/utils.py:2084`), ausente no compositor do diff antigo. Não transplantar literalmente: INFERIU destas referências. |
| `204847`, c3 | Mapa explícito, `TipoSemFaixa`, contador de tipos ativos sem faixa. | Continua o default `'atestado'` em `precedencia.py:76`; mapa pequeno em `:52`; contador não aparece na lista que começa em `vigia_ausencia.py:154`. | É a mesma raiz c3 incluída no histórico 215253; aplicar duas vezes seria duplicar mudança (LEU anexo; INFERIU). |
| `164050`, c3–c12 | Além da c3: separa cobertura de rótulo, preserva efeito histórico de inativos, transporta campos pela ata, recusa nomeada em leitores e cadastro, isola colaborador no cartório; recusa fica em buraco. | Os sítios correspondentes continuam com o comportamento anterior, detalhados abaixo. | É base documental escolhida pelo redesenho, `BACKLOG.md:1837`. Sobrepõe quase toda a 215253 até c12; não são duas obras independentes (LEU anexo; INFERIU). |
| `215253`, c3–c19 | Inclui c3–c12; depois remove montagem própria de grade no PDF, conserva recusa no builder, põe instante com fuso em órfãs desde o juiz, transporta ata→tela→PDF, identifica órfã por instante nos três consumidores e na reconciliação. | Continua só HH:MM no produtor (`escala/utils.py:326`), na ata (`cartorio.py:745`), no leitor (`leitor_celula.py:187`), no PDF (`pdf_espelho.py:174`, `:472`, `:609`, `:716`, `:960`). | Diverge da 164050 justamente nos consumidores posteriores e formato. **Documento mais novo interrompe este caminho:** `BACKLOG.md:1833` diz “orfas_instantes sai” e obra parada; `:1837` diz c20 encerrada, voltar à base 164050 e construir após acessor. `:2089` proíbe formato e leitor no mesmo pouso. Conflito de desenho/ordem LEU; aptidão de merge [não verificado]. |
| `lastro-folga`, c2a–c2c | Guarda `Ausencia.save`: recusa introduzir tipo fora do catálogo vivo; cache por schema; portas traduzem recusa; `update_fields` sem tipo não pergunta. | `Ausencia.save` começa por carimbar decisão e termina em `super().save`, sem guarda de tipo (`ponto/models.py:868`, `:908`). Cache do catálogo é único no processo (`catalogo/ausencias.py:257`). | Complementa guarda de faixa, mas não a substitui: “tipo no catálogo” e “tipo tem faixa” são perguntas distintas; guarda de faixa no cadastro também não existe (`services/ausencia.py:1302`). Conflito textual com FAIXA no catálogo/cache e portas de ausência; precisa composição, não sobrepor versões (INFERIU). Diff integral dessa raia não foi fornecido; merge exato [não verificado]. |

**Censo dos sítios dos diffs FAIXA que ainda estão antes da mudança.** Todos os caminhos abaixo têm prefixo `app/`; são LEU.

| Sítio HEAD | Antes ainda observado / mudança correspondente nos anexos |
|---|---|
| `ponto/precedencia.py:52`, `:76`, `:259`, `:273`, `:381` | Mapa/default atestado; sem canal de recusas; universo exclui rejeitada, sem exceção `troca_de_plantao`; acumula faixa sem recusa. |
| `ponto/catalogo/ausencias.py:240`, `:249`, `:257`, `:267`, `:293` | Só ativos, cache único, efeito de existente usa o mesmo catálogo ativo. |
| `ponto/services/ausencia.py:1302` | Cadastro copia campos/salva sem recusa de código/sem faixa. |
| `ponto/views_tipos_ausencia.py:82`, `:86`, `:94` | Chama cadastro sem tratar `AusenciaInvalida`. |
| `ponto/services/vigia_ausencia.py:154`; `pautas/semanal.py:55` | Lista e rótulos sem `tipos_ativos_sem_faixa`. |
| `escala/utils.py:1156`, `:1202`, `:1220` | Overlay não chama A2, rotula todo não-falta como `ausencia`; não transporta recusa. |
| `ponto/services/cartorio.py:730`, `:766`, `:772`, `:1244` | Ata transporta parcial, não os três campos `ausencia_tipo/id/label`; cobertura inteira usa rótulo ausencia/falta. |
| `escala/services/leitor_celula.py:429`, `:433` | Transporta parcial, não campos de cobertura. |
| `ponto/services/dia_pago.py:387`, `:395` | Universo trabalho/ausencia; abono condicionado ao rótulo. |
| `folha/export.py:1095` | Prontidão ainda deriva seus mapas inline; é o único arquivo cujo blob inteiro difere da base dos anexos. |
| `ponto/services/fechamento.py:1046`, `:1081`, `:1087`, `:1303` | Universo pelo rótulo e exceções genéricas; nenhuma propagação específica de `TipoSemFaixa`. |
| `ponto/management/commands/processar_cartorio.py:65`; `cartorio.py:1954` | Sem isolamento/contador por colaborador recusado. |
| `ponto/status_dia.py:221`, `:298`, `:301`, `:307` | Só fatos, sem recusas. |
| `colaboradores/services/calendario.py:499`, `:606`, `:661`, `:715` | Só fatos; chama `do_dia` diretamente. |
| `escala/services/semana_do_posto.py:151`, `:168`, `:178` | Só explicado/buraco; exceção genérica vira trabalho. |
| `ferias/situacao.py:72`, `:102`, `:246`, `:320` | Sem canal de aviso por tipo sem faixa. |
| `ponto/services/dia_decidido.py:371` | Reconhece somente `tipo == 'folga'`, sem `folga_da_escala`. |
| `ponto/services/dia_do_colab.py:15` | Consulta A2 avulsa, sem representação da recusa. |
| `ponto/services/espelho.py:297`, `:489`, `:644`, `:648` | Sem recusa do builder/da A2; sem publicação de instantes das órfãs. |
| `relatorios/pdf_espelho.py:475`, `:543`, `:598` | Faz duas perguntas à grade antes de consultar a tela. |
| `escala/utils.py:1650`, `:1675`, `:2005`, `:2039` | Dry-run e reconciliação de órfã continuam identificando/removendo por HH:MM. |
| `escala/utils.py:1084` | Adaptador perde as órfãs quando não há marcos. |

**Pendências que os anexos não resolvem, comprovadas no HEAD.**

- Dependência da grade da janela continua registrada como aberta, não curada: `BACKLOG.md:1066`. O rescaldo escolhe janela unitária, mas isso não prova invariância do construtor (INFERIU; `cartorio.py:1156`, `:1175`).
- Cartório tem outro casamento próprio: soma horas de colunas e órfãs (`cartorio.py:627`), quando falta coluna casa por proximidade linear ≤30 min (`:666`) e acende (`:672`); ata continua gravando órfãs (`:745`). A própria documentação chama isso de segundo juiz (`BACKLOG.md:2091`) e o comentário descreve a invariante disjunta/teste (`cartorio.py:682`, `:702`). Nenhuma das cópias fornecidas remove esse ramo (LEU anexo; INFERIU). [nao verificado]
- `como_acendeu` ainda substitui `luz` faltante pela hora prevista (`lastro.py:340`); backlog afirma que não procede por ausência de escritor produtivo (`BACKLOG.md:1787`), mas uma sonda de produção atual não está disponível: [não verificado].
- As provas c3–c19 são listadas nos anexos, mas os diffs de testes não foram fornecidos para as raias FAIXA e não estão na árvore; que passam/mordem nesta cópia é [não verificado]. [nao verificado]
- Acessor tipado está apenas no plano/fila (`BACKLOG.md:1833`, `:2088`); arquivo `escala/services/ata.py` ausente nesta cópia: existência de implementação pousada [não verificado].

**Perguntas de lei: separar lacuna de regra já respondida.**

1. **Efeito de tipo desconhecido é lacuna candidata, 4 sítios.** Fecha suprimindo (`fechamento.py:698`–`:701`), coletor não o inclui em nenhum efeito (`efeito_ausencia.py:57`–`:62`), visual chama ABONADO (`dia_decidido.py:468`–`:473`), precedência chama atestado (`precedencia.py:76`). A divergência está registrada em `BACKLOG.md:1812`; regra única explícita para `efeito=None` não localizada [não verificado]. Não equiparar faixa de precedência a efeito financeiro: as perguntas são diferentes (`precedencia.py:45`; `dia_pago.py:372`).
2. **Não contar como nova pergunta** raio versus juiz do par: a ordem de substituição está expressa e a opção raio foi superada (`BACKLOG.md:1756`).
3. **Não contar como nova pergunta** cluster morrer por idade: já decidiu L-125 (`LEIS.md:146`; `CORTES.md:430`).
4. **Não contar como conflito vigente** reconstruir passado versus FOTO: L-124 declara a revogação, preserva fato novo e exportada (`LEIS.md:145`; `CORTES.md:419`). O redesenho fixa ausência de bump na cópia, mas prova de que a nova regra atua só no horizonte é [não verificado].
5. **Não inventar decisão** sobre quantas das demais divergências são “perguntas de lei”: proximidade órfã/acesa é violação potencial de juiz único/invariante já escrita (`cartorio.py:691`, `:702`; `LEIS.md:60`), não demonstra por si duas leis conflitantes.

## 13/11 17:57 · sessao e38e884bd20f · - · - · origem=Ronald (tmux)

Q2 — leitura estática, sem executar nem escrever.

**LEU:** a isenção C7 item 8 permanece no predicado default: respostas pendentes de dia trancado são excluídas por `not dia_da_pergunta_trancado(q)`; `ignorar_tranca=True` desativa essa exclusão. O delta muda somente os dois leitores CSF para `True`. [app/chamados/models.py:1889](app/chamados/models.py:1889), [app/chamados/models.py:1902](app/chamados/models.py:1902), [app/ponto/supra_juiz.py:573](app/ponto/supra_juiz.py:573), [app/ponto/supra_juiz.py:574](app/ponto/supra_juiz.py:574).

**LEU — leitores diretos e efeito:**

| Leitor | Regime atual | Efeito do delta |
|---|---|---|
| `DisputaSupervisao.pode_fechar` | Default, preserva C7; exige também ausência de fio vivo | Nenhuma mudança direta. [app/chamados/models.py:1909](app/chamados/models.py:1909) |
| `DisputaSupervisao.fechar` | Default na recusa de resposta sem veredito | Nenhuma mudança direta; fechamento da disputa pode ocorrer em dia trancado. [app/chamados/models.py:1942](app/chamados/models.py:1942) |
| Desligamento | Default antes de fechar disputa | Nenhuma mudança direta. [app/colaboradores/services/desligamento.py:141](app/colaboradores/services/desligamento.py:141) |
| Ficha do colaborador | Default, intersectado com perguntas do app | Nenhuma mudança direta. [app/colaboradores/services/ficha.py:169](app/colaboradores/services/ficha.py:169) |
| Gaveta “respondido_a_decidir” | Default | Nenhuma mudança direta. [app/chamados/services/fila_por_causa.py:226](app/chamados/services/fila_por_causa.py:226) |
| Reconciliador de órfãs | Default antes de `pode_fechar/fechar` | Nenhuma mudança direta. [app/chamados/reconciliador.py:1164](app/chamados/reconciliador.py:1164), [app/chamados/reconciliador.py:1171](app/chamados/reconciliador.py:1171) |
| Comando `validar_classe_a` | Default, a partir da gaveta | Nenhuma mudança direta. [app/chamados/management/commands/validar_classe_a.py:44](app/chamados/management/commands/validar_classe_a.py:44), [app/chamados/management/commands/validar_classe_a.py:54](app/chamados/management/commands/validar_classe_a.py:54) |
| Medição da tranca/Pauta DP | `ignorar_tranca=True` | Já ignorava a isenção; nenhuma mudança direta. [app/chamados/services/competencia_trancada.py:126](app/chamados/services/competencia_trancada.py:126) |
| Retratação `irmas_respondidas_sem_consumo` | `ignorar_tranca=True` | Já ignorava a isenção; nenhuma mudança direta. [app/chamados/juizes.py:2035](app/chamados/juizes.py:2035) |
| `perguntas_que_seguram_csf` | `True` nos universos pai e destino | Passa a segurar também resposta sem veredito cujo dia foi calado pela tranca. [app/ponto/supra_juiz.py:573](app/ponto/supra_juiz.py:573), [app/ponto/supra_juiz.py:574](app/ponto/supra_juiz.py:574) |

**LEU:** o C6 também preserva C7: antes de considerar resposta, `dia_da_pergunta_trancado` torna a pergunta morta. O juiz real exige dia de turno em competência trancada e nenhum toque posterior do admin; conversa não tem competência de fato. [app/chamados/juizes.py:1403](app/chamados/juizes.py:1403), [app/chamados/juizes.py:974](app/chamados/juizes.py:974), [app/chamados/juizes.py:334](app/chamados/juizes.py:334), [app/chamados/juizes.py:1078](app/chamados/juizes.py:1078).

**INFERIU:** a disputa pode fechar normalmente por C7 enquanto seu pai permanece vivo pela guarda CSF alterada: o signal chama `reconciliar`, que passa pela guarda da conversa; essa guarda usa a lista CSF inteira. Portanto há mudança indireta no resultado da Defesa 1, embora `fechar/pode_fechar` não mudem. [app/chamados/signals.py:43](app/chamados/signals.py:43), [app/chamados/models.py:686](app/chamados/models.py:686), [app/chamados/models.py:719](app/chamados/models.py:719), [app/ponto/supra_juiz.py:573](app/ponto/supra_juiz.py:573).

**LEU — saída existente:** `competencia_trancada.medir` seleciona chamados vivos cuja **competência do chamado** coincide com a trancada, exclui resgatados/exceções e retém os que tenham pergunta viva; os demais entram em `r['chamados']`. `encerrar` chama `encerrar_por_competencia_trancada`, cuja guarda é `_gate_pergunta_viva`, sem `_gate_conversa_do_pai`; grava `via=competencia_trancada`. Assim a nova recusa CSF não barra essa via. [app/chamados/services/competencia_trancada.py:101](app/chamados/services/competencia_trancada.py:101), [app/chamados/services/competencia_trancada.py:109](app/chamados/services/competencia_trancada.py:109), [app/chamados/services/competencia_trancada.py:160](app/chamados/services/competencia_trancada.py:160), [app/chamados/services/competencia_trancada.py:209](app/chamados/services/competencia_trancada.py:209), [app/chamados/models.py:1684](app/chamados/models.py:1684), [app/chamados/models.py:1686](app/chamados/models.py:1686).

**LEU — limite crucial:** essa saída não é cron automático para acervo já trancado: dispara somente no **ato** de criação/retranca; `ato = criado or reaberto_em is not None`. O passivo depende do comando `encerrar_por_competencia_trancada --apply`; o registro de crons declara expressamente que ele não é cron. [app/ponto/services/fechamento.py:1411](app/ponto/services/fechamento.py:1411), [app/ponto/services/fechamento.py:1420](app/ponto/services/fechamento.py:1420), [app/config/crons.py:1276](app/config/crons.py:1276), [app/chamados/management/commands/encerrar_por_competencia_trancada.py:29](app/chamados/management/commands/encerrar_por_competencia_trancada.py:29).

**LEU/INFERIU:** Pauta DP **não tira chamado da fila por si**: recebe ids e escreve texto pedindo decisão no calendário; o fechamento foi feito antes pela via `competencia_trancada`. Há inclusive retorno anterior à criação da Pauta se `r['acoes'] == 0`. Portanto “vai para Pauta DP” não prova saída automática de um pai retido agora em tranca antiga. [app/chamados/services/competencia_trancada.py:192](app/chamados/services/competencia_trancada.py:192), [app/chamados/services/competencia_trancada.py:232](app/chamados/services/competencia_trancada.py:232), [app/chamados/services/competencia_trancada.py:248](app/chamados/services/competencia_trancada.py:248), [app/chamados/services/competencia_trancada.py:256](app/chamados/services/competencia_trancada.py:256).

**INFERIU:** não há beco absoluto demonstrado — existe via de encerramento da tranca —, mas não há garantia de progresso automático sob a hipótese “tranca já viva, admin nunca toca”. A nova guarda não dispara essa limpeza; se o chamado estiver fora da competência selecionada, sem dia, resgatado ou em módulo excluído, nem essa passagem o seleciona. A competência do chamado é julgada separadamente e pode divergir da competência de uma pergunta. [app/ponto/supra_juiz.py:573](app/ponto/supra_juiz.py:573), [app/chamados/services/competencia_trancada.py:105](app/chamados/services/competencia_trancada.py:105), [app/chamados/services/competencia_trancada.py:113](app/chamados/services/competencia_trancada.py:113), [app/chamados/services/competencia_trancada.py:161](app/chamados/services/competencia_trancada.py:161), [app/chamados/juizes.py:2352](app/chamados/juizes.py:2352). Chamado específico da frota nessa situação: **[não verificado]**.

## 13/11 17:57 · sessao d93793f5cd28 · - · - · origem=Ronald (tmux)

**5 — LEU, mapa de escrita/leitura.** O predicado usado pelos dois caminhos lê `validada_em`, `materializada_em`, `respondida_em`, `resposta_colab` e, se `ignorar_tranca=False`, a tranca; o delta muda somente a opção de leitura da tranca, sem escrever esses campos: `app/chamados/models.py:1889-1902`, `app/ponto/supra_juiz.py:562-577`.

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| `resposta_colab` | Resposta humana em `responder_pergunta_disputa_core`, junto de `respondida_em`: `app/chamados/services/disputa_emissao.py:1462-1469`; resposta automática sem sobrescrever texto existente: `:1647-1656`; reabertura limpa: `:1593`. | Juiz O168 aceita texto não vazio como fala mesmo sem carimbo: `app/chamados/models.py:1898-1901`; C6 exige carimbo **e** texto na sua cláusula da resposta: `app/chamados/juizes.py:1417`; plantio usa o conteúdo como hora: `app/chamados/services/materializacao.py:412-419`. |
| `respondida_em` | Resposta humana recebe `now()`: `app/chamados/services/disputa_emissao.py:1462-1468`; automática recebe instante fornecido: `:1652-1653`; cura de limbo recebe o instante testemunhado pela trilha: `:1661-1678`; porta `carimbar(resposta=...)` preenche nulo somente com texto: `app/chamados/models.py:3186-3189`; reabertura limpa: `app/chamados/services/disputa_emissao.py:1593`; ressuscitação limpa em lote: `app/chamados/models.py:1613`. | O168 lê somente nulidade: `app/chamados/models.py:1900`; validar recusa sem ele: `app/chamados/services/validacao.py:416-418`; materialização seleciona apenas não nulo: `app/chamados/services/materializacao.py:1375-1378`; estado da pergunta o usa para distinguir pendente/respondida: `app/chamados/models.py:3097-3104`. |
| `validada_em` | Ato de validar em `_carimbar_validacao`, `timezone.now()` e linha da sequência com mesmo instante: `app/chamados/services/validacao.py:150-164`; nova resposta desfaz validação antiga: `app/chamados/services/disputa_emissao.py:1470-1482`; devolução por recusa limpa: `app/chamados/services/validacao.py:218-228`; reabertura limpa: `app/chamados/services/disputa_emissao.py:1593`; porta de simetria `carimbar(resposta='validar')` preenche nulo: `app/chamados/models.py:3186-3193`; ressuscitação limpa: `:1613`. | O168 considera qualquer não nulo julgado: `app/chamados/models.py:1899`; C6 encerra antes dos demais degraus: `app/chamados/juizes.py:1400-1402`; validar recusa repetição: `app/chamados/services/validacao.py:419-421`; materializador exige não nulo: `app/chamados/services/materializacao.py:1375-1378`. |
| `materializada_em` | Atribuição central em `_escrever_carimbo`: `app/chamados/models.py:3153-3157`; `carimbar` usa instante fornecido ou `now()` e consulta C6 antes de salvar: `:3180-3208`; identidade carimba sem consultar C6: `:3224-3246`; descarimbar avulso e lote limpam junto de autor/via: `:3259-3268`, `:3307-3322`. | O168 exclui qualquer pergunta materializada: `app/chamados/models.py:1899`; C6 lê carimbo e via: `app/chamados/juizes.py:1385-1395`, `:1412-1413`; fila de plantio exige nulo: `app/chamados/services/materializacao.py:1375-1378`. |
| Tranca (`PeriodoFechado`, `reaberto_em`, `fechado_em`) | Nascimento/retranca em `trancar_periodo`: `app/ponto/services/fechamento.py:1400-1419`; `fechado_em` nasce com `auto_now_add`: `app/ponto/models.py:926`; reabertura preenche `reaberto_em` e desativa: `app/ponto/services/fechamento.py:1432-1441`. | `periodo_trancado`/`competencia_trancada` leem existência com `reaberto_em IS NULL`; `ativo` é documental: `app/ponto/services/fechamento.py:1464-1480`. Pergunta trancada exige também ausência de toque humano posterior: `app/chamados/juizes.py:1062-1079`, `:1237-1251`. O168 atual ignora essa leitura nos dois universos CSF: `app/ponto/supra_juiz.py:575-576`; retratação também: `app/chamados/juizes.py:2033-2035`. |

**LEU — toque que muda o resultado da tranca:** o leitor distingue cobrança manual, resolução do dia e desarquivamento; resolução exige assinatura `via='admin'` ou chave `acoes`, excluindo lavra do cartório herdando usuário humano: `app/chamados/juizes.py:1083-1093`, `:1245-1251`, `:1292-1298`. Escritas localizadas: cobrança manual `app/chamados/services/cobrar_dia.py:39`; desarquivar chama `arquivo.levantar(...acao='desarquivar_chamado')` em `app/chamados/views_arquivar.py:77`; constante da via humana `app/ponto/services/veredito_celula.py:76`.

**LEU — dia do turno versus dia civil:** carimbos são instantes dos atos, não datas do turno, pelas atribuições `now()` acima. `data_evento` da conversa é dia civil do pedido (`localdate()`): `app/chamados/models.py:2850-2858`; o contrato distingue explicitamente esse dia do dia do turno da pergunta factual: `:2868-2870`. A tranca usa `_dia_de_turno_da_pergunta`, que devolve `None` para conversa e `data_evento` para fato: `app/chamados/juizes.py:334-338`, `:989-992`; calcula competência por `janela_atual(d, empresa)`, nunca pelo mês civil cru: `:1062-1066`.

**INFERIU — não há significado uniforme em todos os leitores:** o juiz O168 aceita **carimbo OU texto** (`app/chamados/models.py:1900-1901`), validar exige carimbo (`app/chamados/services/validacao.py:416-418`) e C6 exige carimbo **E texto** na cláusula de resposta (`app/chamados/juizes.py:1417`). Logo texto sem `respondida_em` pode segurar CSF/retratação e ser recusado como `sem_resposta` na validação; é divergência herdada, não alteração deste delta (mesmas referências). A cura existente para esse meio estado exige instante testemunhado pela trilha: `app/chamados/services/disputa_emissao.py:1661-1678`; disponibilidade dessa trilha para casos de produção: **[não verificado]**.

**INFERIU — `materializada_em` também tem vocabulário excessivo:** o help text promete “batida criada + chamado fechado” (`app/chamados/models.py:3449-3451`), mas a porta de identidade atende admin/dedup/colisão e grava o carimbo sem plantio obrigatório (`:3224-3246`). Portanto carimbo não demonstra dinheiro nem batida nova; operacionalmente O168 o trata como processamento suficiente para excluir a pergunta (`:1899`).

## 13/11 17:57 · sessao cb5af247cf08 · - · - · origem=Ronald (tmux)

LEU: a matriz e as provas foram enviadas ao agente principal, com estes pontos críticos:

- O chamado usa `resolvido_em`, não `fechado_em`; os campos estão em `app/chamados/models.py:52`, `:54` e `:60`.
- A escrita persistente de transição passa por `_gravar_estado`: `app/chamados/models.py:201`, `:228`, `:245`, `:249`, `:296`. O nascimento escreve por `abrir`/`criar`: `:939`, `:972`, `:1096`.
- `_vivas_entre` altera `status_local` somente em memória e restaura no `finally`: `app/chamados/models.py:356`, `:371`.
- O coletor existente mais próximo da pergunta única de lastro é `premissa_regularizada`: `app/chamados/models.py:1253`; a decisão operacional compartilhada por `reconciliar` e `reconciliaria` é `_decisao_do_fechamento`: `:664`.
- A trilha de estado é chamada antes do `save`, no mesmo atomic: `app/chamados/models.py:280`, `:292`, `:296`; entretanto, `evento` é best-effort e pode devolver `None`: `app/core/observ.py:94`, `:105`, `:133`, `:139`. INFERIU: o HEAD não garante incondicionalmente “uma trilha por estado persistido”.
- As recusas repetidas de reabertura por premissa regularizada/admin sem fato novo gravam evento a cada chamada: `app/chamados/models.py:1395`, `:1401`. O fechamento possui deduplicação específica de recusas: `:397`.
- Sem dia do fato, o gate do fechamento permite encerrar e registra sua ignorância: `app/chamados/models.py:733`; o coletor `premissa_regularizada` responde `False`: `:1272`.
- INFERIU: há alcance de lei a esclarecer entre reabertura pelo gate `reabrir`, que exige fato novo para fechamento de admin (`app/chamados/models.py:1343`), e a transição humana, cuja matriz permite `resolvido → em_analise` e override de admin TI (`app/chamados/catalogo/motor.py:102`, `:150`), aplicada sem aquela guarda (`app/chamados/models.py:199`).
- Arquivar por idade é véu, preservando estado: `app/chamados/services/arquivo.py:141`, `:154`, `:166`. Arquivar pela tranca encerra com via própria: `app/chamados/models.py:1623`, `:1640`.
- `superado` permanece no catálogo, mas sua porta foi retirada por ausência de chamador: `app/chamados/models.py:795`; não deve aparecer como transição executável corrente.

Não executei Django nem testes; as provas reportadas são testes lidos, com referências enviadas ao principal. [não verificado: execução] [nao verificado]

## 13/11 17:57 · sessao 95147ecea9fa · - · - · origem=Ronald (tmux)

**1 — LEU:** O predicado ficou igual nos dois caminhos: CSF e retratação chamam `sem_veredito_entre(..., ignorar_tranca=True)`; `pode_executar_csf` nega a lista resultante. (`app/ponto/supra_juiz.py:573–588`; `app/chamados/juizes.py:2033–2035`.)

**1 — INFERIU:** O BLOQ “CSF libera / retratação segura” foi eliminado para a mesma pergunta nas mesmas condições, mas os universos continuam distintos:

- CSF = perguntas de todas as disputas do pai **∪** perguntas com `chamado_destino=ch`. (`app/ponto/supra_juiz.py:558`, `:573–574`.)
- Retratação = perguntas com `chamado_destino=ch` **∪** perguntas do pai cujo destino não está VIVO. (`app/chamados/juizes.py:2033–2035`.)
- Portanto a retratação é subconjunto do CSF. Ainda existe **CSF segura / retratação libera**: resposta pendente do pai creditada a outro destino vivo. Pergunta creditada ao chamado mas fora do pai entra em ambos pelo destino. (`app/ponto/supra_juiz.py:558`, `:573–574`; `app/chamados/juizes.py:2033–2035`.)

**3 — LEU:** `_perguntas_do_pai` não usa `select_related`. Entretanto o delta passa `ignorar_tranca=True` nos dois universos; o `or` evita executar `dia_da_pergunta_trancado`, e só se leem campos da própria pergunta. O caminho que acessaria disputa/colaborador está em `dia_da_pergunta_trancado`. (`app/ponto/supra_juiz.py:558`, `:573–574`; `app/chamados/models.py:1898–1902`; `app/chamados/juizes.py:991–992`.)

**3 — INFERIU:** Não sobra query por pergunta nesse gate atual. A nota utiliza a lista `_pend` já calculada e seus PKs, sem recarregar pergunta por pergunta. (`app/chamados/models.py:719`, `:735–739`.)

**3 — LEU:** O novo selo deixa todas as N perguntas pendentes e compara queries com 1 e 3 disputas. Ele chama `registrar=False`, portanto não mede a escrita da nota. (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47`, `:56–57`, `:69–75`.) Execução e contagens reais: **[não verificado]**.

**4 — INFERIU:** Os dois mutantes são pegos pelo **mesmo `assertEqual` da linha 307**, em iterações diferentes:

- Retirar o kwarg do **destino** (`supra_juiz.py:574`): falha primeiro com `quem='destino'`.
- Retirar o kwarg do **pai** (`supra_juiz.py:573`): destino passa; falha com `quem='pai'`.

A fixture tem pai e destino distintos, uma pergunta ligada aos dois, e tranca simulada como verdadeira; o predicado sem kwarg exclui a pergunta. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:44–57`, `:300–309`; `app/chamados/models.py:1902`.)

**4 — LEU:** O `assertFalse(pode_executar_csf(...))` fica na linha 310; o mutante já abortaria no `assertEqual` anterior. O RED verifica `irmas_respondidas_sem_consumo(destino)`, mas **não verifica `irmas_respondidas_sem_consumo(pai)`**, portanto não demonstra igualdade dos universos. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:304–310`.) Execução dos mutantes: **[não verificado]**.

## 13/11 17:56 · sessao a5c16e32d31f · BUG-PAR · verificador · origem=Code

**BLOQUEIA.** Há generalização indevida de COLISÃO para PARIDADE, um caminho que pode regravar competência 10 exportada e uma recusa terminal ainda apresentada como “Tudo validado e gravado”. (`app/chamados/services/materializacao.py:1238`; `app/chamados/services/validacao.py:555`; `app/chamados/services/desenlace.py:72`)

Suíte, execução dos REDs, mutantes e ocorrência dos contraexemplos na produção: **[não verificado]**. [nao verificado]

**1 — BLOQUEIA: a primeira cura fecha o caso da irmã, mas altera também a colisão alheia.**

O ramo pergunta somente se `_conflito` traduz como COLISAO. Não identifica a batida ofensora. Entretanto, o chokepoint procura **qualquer** batida oposta na janela e devolve texto sem o identificador dela. Logo, “colisão pode ser com ela” não prova que o rollback removeu a causa. (`app/chamados/services/materializacao.py:1237`; `app/ponto/registro_batida.py:139`; `app/ponto/registro_batida.py:146`)

Contraexemplo sintético:

- Chão: `E 18:30`, **E alheia 01:08**, `S 06:30`; respostas a plantar: `S 01:10` e `E 02:00`.
- Chamado cobra `S 01:10`, enquanto o DNA vigente tem o marco S às `01:00`. Essa diferença é representável: o plantio prefere o marco cobrado no chamado; a exceção de colisão consulta os marcos vigentes e tolera cinco minutos. (`app/chamados/services/materializacao.py:488`; `app/chamados/services/materializacao.py:649`; `app/ponto/registro_batida.py:35`)
- A projeção aceita a vizinhança das duas metades; a irmã `E 02:00` grava. A tentativa de `S 01:10` colide com a **E alheia 01:08**, a dois minutos, fora da exceção do marco próprio. O rollback remove a irmã, mas preserva a E alheia. (`app/chamados/services/materializacao.py:227`; `app/chamados/services/materializacao.py:1442`; `app/ponto/registro_batida.py:145`)
- Mesmo assim, a cura grava motivo PARIDADE e deixa `materializada_em` nulo; o contrato existente manda COLISAO para `VIA_FATO_OCUPADO`. Na validação humana, a devolução também limpa `validada_em`. (`app/chamados/services/materializacao.py:1240`; `app/chamados/services/materializacao.py:1402`; `app/chamados/services/validacao.py:192`; `app/chamados/services/validacao.py:201`)

Portanto, para a colisão alheia persistente, deveria continuar valendo `fato_ocupado`. Com os mesmos insumos, sucessivas tentativas repetem a recusa e a devolução ao admin. Isso é um ciclo operacional de revalidação; não há laço computacional infinito demonstrado. (`app/chamados/services/validacao.py:570`; `app/chamados/services/validacao.py:575`)

**OK quanto ao catálogo e ao corte:** a frase nova casa a PARIDADE correta. A única regra anterior também retorna PARIDADE; COMPETENCIA e COLISAO vêm depois. A prioridade é da regra, pois o catálogo usa primeira correspondência com `any()`, independentemente da posição da expressão no texto. (`app/chamados/catalogo/recusa.py:29`; `app/chamados/catalogo/recusa.py:34`; `app/chamados/catalogo/recusa.py:89`)

Para `Pergunta #51530`, timestamp sem microssegundos e origem `disputa_s84_retro`, a composição nova tem **235 caracteres**. Se uma composição maior ultrapassar 240, o prefixo classificatório continua preservado; detalhes finais podem ser cortados. (`app/chamados/services/materializacao.py:1240`; `app/ponto/registro_batida.py:147`; `app/chamados/services/materializacao.py:1399`)

**2 — OK: os cinco chamadores querem o universo apurável.**

| Chamador | O que julga |
|---|---|
| `app/chamados/services/materializacao.py:215` | Se a metade isolada exige o par. |
| `app/chamados/services/materializacao.py:228` | Paridade com a irmã projetada. |
| `app/chamados/services/materializacao.py:712` | Antevisão do efeito do plantio na tela. |
| `app/chamados/services/materializacao.py:1150` | Guarda efetiva do plantio. |
| `app/chamados/services/materializacao.py:1161` | Rechecagem depois da irmã gravada. |

Nenhum desses usos apresenta o espelho cru. A autoridade distingue explicitamente apuração, que exclui retratadas, de exibição de marcação pela Portaria 671. A cura consulta essa autoridade nos dois lados. (`app/ponto/turnos.py:2856`; `app/ponto/turnos.py:2876`; `app/chamados/services/materializacao.py:110`; `app/chamados/services/materializacao.py:113`)

`passivo_validado.py:497` descreve **outra guarda, adicional**: comparar a geometria do dia antes/depois pelo pareador. Ela continua necessária: alternância local pode passar enquanto um plantio avulso transforma `E* S` em `E* S E`. Não há divergência introduzida pela troca de universo. (`app/chamados/services/passivo_validado.py:492`; `app/chamados/services/passivo_validado.py:527`; `app/chamados/services/passivo_validado.py:557`; `app/chamados/services/materializacao.py:454`)

Não confundir com `_batida_existente_na_janela`: esse leitor permanece deliberadamente cru para impedir replantio de marca já retratada, pergunta diferente da paridade apurável. (`app/chamados/juizes.py:243`; `app/chamados/juizes.py:806`)

**3 — BLOQUEIA no significado de `recusa_motivo`; portas de carimbo preservadas.**

Censo dos escritores e leitores decisórios:

| Campo | Quem escreve | Quem lê para decidir |
|---|---|---|
| `via_resolucao` | Porta central `_escrever_carimbo`; descarimbo em lote; troca por fato no espelho; relavra. (`app/chamados/models.py:3127`; `app/chamados/models.py:3308`; `app/chamados/models.py:3317`; `app/chamados/services/relavra_via.py:125`) | C6/identidade, lastro, estado de aplicação, universo 103 e flip automático. (`app/chamados/juizes.py:1438`; `app/chamados/juizes.py:679`; `app/chamados/services/materializacao.py:692`; `app/chamados/services/materializacao.py:276`; `app/ponto/services/flip_auto.py:129`) |
| `materializada_em` | `_escrever_carimbo` e descarimbo em lote; a porta 103 chama `descarimbar`. (`app/chamados/models.py:3125`; `app/chamados/models.py:3308`; `app/chamados/services/validacao.py:268`) | Estado, C6, aplicação, seleção do materializador, terminalidade da recusa, devolução e elegibilidade 103. (`app/chamados/models.py:3069`; `app/chamados/juizes.py:1438`; `app/chamados/services/materializacao.py:688`; `app/chamados/services/materializacao.py:1744`; `app/chamados/services/materializacao.py:1402`; `app/chamados/services/validacao.py:192`; `app/chamados/services/materializacao.py:276`) |
| `validada_em` | Validação canônica; devoluções comum/103; resposta nova; reabertura individual/lote; simetria da porta de carimbo. (`app/chamados/services/validacao.py:136`; `app/chamados/services/validacao.py:201`; `app/chamados/services/validacao.py:271`; `app/chamados/services/disputa_emissao.py:1480`; `app/chamados/models.py:1508`; `app/chamados/models.py:1567`; `app/chamados/models.py:3161`) | C6, estado, impedimento de nova validação, exigência de irmã validada e fila de materialização. (`app/chamados/juizes.py:1401`; `app/chamados/models.py:3067`; `app/chamados/services/validacao.py:498`; `app/chamados/services/materializacao.py:195`; `app/chamados/services/materializacao.py:1743`) |
| `recusa_motivo` | Recusa do materializador; pendência de marco; recusa humana; contestação respondida; duplicata, usando também a porta de identidade. (`app/chamados/services/materializacao.py:1411`; `app/chamados/services/materializacao.py:1125`; `app/chamados/services/validacao.py:705`; `app/chamados/services/disputa_emissao.py:1497`; `app/chamados/services/fio.py:1399`; `app/chamados/models.py:3214`) | Classificador/terminalidade, contador de paridade/103, presença exigida pela devolução, explicador, flip por regex e alarmes de ausência de motivo. (`app/chamados/services/materializacao.py:1400`; `app/chamados/services/materializacao.py:262`; `app/chamados/services/validacao.py:194`; `app/chamados/services/explicador.py:90`; `app/ponto/services/flip_auto.py:132`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`) |
| `Batida.retratada_em` | Escrita física por `anotar`; retratação fornece o instante; desfazer humano fornece `None`. (`app/ponto/registro_batida.py:253`; `app/ponto/registro_batida.py:274`; `app/ponto/services/retratar_humano.py:207`) | Universo apurável, deduplicação/colisão do chokepoint, pareamento e lastro. (`app/ponto/turnos.py:2876`; `app/ponto/registro_batida.py:109`; `app/ponto/registro_batida.py:141`; `app/ponto/turnos.py:1864`; `app/chamados/juizes.py:755`) |
| Carimbo `BUG-PAR-103` | Porta trava, reconsulta elegibilidade, descarimba, limpa validação e registra DEVOLUCAO/evento antes/depois; falta de evento aborta. (`app/chamados/services/validacao.py:259`; `app/chamados/services/validacao.py:268`; `app/chamados/services/validacao.py:276`; `app/chamados/services/validacao.py:281`; `app/chamados/services/validacao.py:288`) | Rótulo consulta precisamente DEVOLUCAO cujo motivo começa com `BUG-PAR-103`; estado de aplicação incorpora esse rótulo. (`app/chamados/services/materializacao.py:285`; `app/chamados/services/materializacao.py:697`) |

O problema de dois significados é concreto: texto livre de recusa humana compartilha campo com expressões que determinam código e terminalidade. A mensagem nova muda **estado**, contador e orientação ao admin, além da redação. (`app/chamados/services/validacao.py:705`; `app/chamados/catalogo/recusa.py:89`; `app/chamados/services/materializacao.py:1402`; `app/chamados/services/materializacao.py:262`)

Outros consumidores de tradução são o passivo e as respostas de tela. Eles passam a apresentar “vizinha do MESMO tipo” para uma causa original de **tipo oposto**. (`app/chamados/services/passivo_validado.py:144`; `app/chamados/services/fio.py:741`; `app/chamados/services/acoes_disputa.py:34`; `app/chamados/services/acoes_disputa.py:174`; `app/chamados/services/validacao.py:643`; `app/chamados/catalogo/recusa.py:35`; `app/chamados/catalogo/recusa.py:75`)

O regex independente do flip exige `batida vizinha #…`; a frase nova não fornece isso. Contudo, o leitor também exige `VIA_PARIDADE_RECUSADA`, que o ramo novo não grava: não há novo flip demonstrado por essa mensagem. (`app/ponto/services/flip_auto.py:129`; `app/ponto/services/flip_auto.py:132`; `app/chamados/services/materializacao.py:1402`)

Completude de leitores indiretos/dinâmicos: **[não verificado]**.

**4 — BLOQUEIA: regressões sem proteção suficiente e complemento da Central incompleto.**

- **Segundo escritor/juiz — OK no caminho examinado.** A irmã passa por `_materializar_uma`, a batida por `registrar_batida`, e tela/validação/materialização consultam `par_do_intervalo`. A projeção é temporária e restaurada em `finally`. (`app/chamados/services/materializacao.py:178`; `app/chamados/services/materializacao.py:1219`; `app/chamados/services/materializacao.py:1440`)
- **Universo manual — OK.** Os dois lados usam `batidas_apuraveis`; não há cópia manual do filtro nessa cura. (`app/chamados/services/materializacao.py:110`; `app/chamados/services/materializacao.py:113`)
- **Relógio — NAO-BLOQUEIA adicional.** O corte 103 compara o instante do carimbo com limite fixo no fuso da casa; não depende de “agora” durante a classificação. (`app/chamados/services/materializacao.py:273`; `app/chamados/services/materializacao.py:277`)
- **RED_2c — proteção insuficiente.** Injeta COLISAO em `01:00`, com irmã às `02:00`: não demonstra colisão real com a irmã dentro da janela de até 300 segundos. Também não exige preservar COMPETENCIA_APROVADA. O mutante “toda recusa vira PARIDADE” satisfaz suas asserções; RED_2b cobra rollback, mas não cobra o código da competência. (`app/chamados/tests/test_bug_par_projetado.py:690`; `app/chamados/tests/test_bug_par_projetado.py:695`; `app/chamados/tests/test_bug_par_projetado.py:707`; `app/chamados/tests/test_bug_par_projetado.py:672`; `app/chamados/tests/test_bug_par_projetado.py:679`; `app/ponto/registro_batida.py:31`)
- **RED_2d — OK por inspeção.** Se somente o anterior for curado, o primeiro cenário falha no posterior retratado; se somente o posterior for curado, o segundo cenário falha no anterior retratado. Execução dos mutantes: **[não verificado]**. (`app/chamados/tests/test_bug_par_projetado.py:715`; `app/chamados/tests/test_bug_par_projetado.py:717`; `app/chamados/tests/test_bug_par_projetado.py:721`; `app/chamados/tests/test_bug_par_projetado.py:723`)
- **Exceção convertida em prosa — NAO-BLOQUEIA adicional, mas permanece.** `_materializar_uma` captura qualquer `Exception`, transforma em texto e entrega esse texto ao classificador de estado. Uma exceção concreta engolida na produção: **[não verificado]**. (`app/chamados/services/materializacao.py:1636`; `app/chamados/services/materializacao.py:1648`)

**BLOQUEIA adicional: a Central ainda mente em `VIA_FATO_OCUPADO`.** Contraexemplo: pergunta HORA recusada por colisão alheia, encerrada com essa via, sem batida plantada. O lastro aceita a identidade; `estado_aplicacao_pergunta` retorna `aplicada` para toda materializada exceto PARIDADE; o explicador só marca `gravou=False` para `recusada_paridade`. Assim, com a disputa quitada, o desenlace continua dizendo **“Tudo validado e gravado.”** (`app/chamados/models.py:3823`; `app/chamados/juizes.py:679`; `app/chamados/juizes.py:691`; `app/chamados/services/materializacao.py:688`; `app/chamados/services/explicador.py:101`; `app/chamados/services/desenlace.py:65`; `app/chamados/services/desenlace.py:72`)

O teste da Central cobre somente `VIA_PARIDADE_RECUSADA`; o controle seguinte presume “gravado” quando o item não traz a marca. (`app/chamados/tests/test_bug_par_projetado.py:261`; `app/chamados/tests/test_bug_par_projetado.py:274`)

**5 — BLOQUEIA: há efeito potencial em dinheiro, inclusive exportada.**

**Competência aberta:** sim, uma colisão antes terminal agora pode ficar reaplicável. Se o obstáculo alheio for posteriormente corrigido/retratado, uma nova validação pode plantar o par e alterar a hora apurada. O escritor de batida dispara recálculo por evento. Ocorrência e valor na frota: **[não verificado]**. (`app/chamados/services/materializacao.py:1238`; `app/chamados/services/materializacao.py:1402`; `app/chamados/services/validacao.py:201`; `app/ponto/registro_batida.py:160`; `app/ponto/services/fechamento.py:1809`)

**Competência 10 exportada:** há problema mesmo sem plantar batida:

1. A porta 103 devolve a pergunta para validação do admin. (`app/chamados/services/validacao.py:268`)
2. `validar_pergunta` agenda recálculo **antes** da tentativa de materialização. (`app/chamados/services/validacao.py:544`; `app/chamados/services/validacao.py:555`)
3. O chokepoint recusa a batida em competência aprovada/exportada. A devolução limpa a validação, mas a transação externa termina sem rollback. (`app/ponto/janelas.py:87`; `app/ponto/registro_batida.py:132`; `app/chamados/services/validacao.py:201`; `app/chamados/services/validacao.py:575`)
4. No commit, o callback chama o recálculo, que atualiza os campos calculados do fechamento aprovado/exportado. A guarda especial cobre somente **09/2026**. (`app/ponto/services/fechamento.py:1807`; `app/ponto/services/fechamento.py:1824`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:36`)

Contraexemplo: fechamento 10 exportado com 100 horas gravadas e derivação atual de 110 horas. Validar uma pergunta devolvida pode regravar 110, embora o plantio seja recusado. Os valores são sintéticos; existência desse desvio na produção **[não verificado]**. O caminho permite a escrita, contrariando a condição específica de preservar o gravado do aval fornecido. (`app/ponto/services/fechamento.py:1162`; `app/chamados/services/validacao.py:555`)

**6 — Os NAO-BLOQ da triagem3: um sobe para BLOQUEIA.**

| Achado anterior | Veredito desta volta |
|---|---|
| Mutantes RED5/RED7 escapam | **NAO-BLOQUEIA isoladamente; sobrevivência exata [não verificado].** RED5 cobra conteúdo renderizado; RED7 cobra dois arquivos e conteúdo `[0,1]`. Isso não demonstra quais mutações foram executadas. (`app/chamados/tests/test_bug_par_projetado.py:766`; `app/chamados/tests/test_bug_par_projetado.py:838`) |
| Texto `FORA_DE_PIPELINE` | **NAO-BLOQUEIA isoladamente; divergência permanece.** Continua “exportada segue a L-113”. Alterar a frase não corrige o caminho financeiro acima. (`app/config/crons.py:774`; `app/chamados/services/validacao.py:555`) |
| Caminho de dinheiro | **BLOQUEIA**, pelos passos do item 5. DRY default não protege a validação humana posterior. O DRY retorna antes da escrita do arquivo. (`app/chamados/services/validacao.py:555`; `app/chamados/management/commands/bug_par_103.py:65`) |
| Foto de reversão antes da trava | **NAO-BLOQUEIA isoladamente, risco confirmado.** O comando fotografa/grava antes de chamar a porta; só nela ocorre a trava. A trilha efetiva antes/depois é lida sob trava. (`app/chamados/management/commands/bug_par_103.py:48`; `app/chamados/management/commands/bug_par_103.py:77`; `app/chamados/management/commands/bug_par_103.py:85`; `app/chamados/services/validacao.py:259`; `app/chamados/services/validacao.py:263`) |

Contraexemplo da reversão: A fotografa; B devolve a pergunta; A adquire a trava e retorna `não_caso`. O arquivo de A inclui uma pergunta que A não alterou. Restaurá-lo cegamente desfaria B; aplicador desse JSON e ocorrência concorrente **[não verificado]**. (`app/chamados/management/commands/bug_par_103.py:55`; `app/chamados/services/validacao.py:261`; `app/chamados/management/commands/bug_par_103.py:91`)

**7 — NAO-BLOQUEIA adicional; LEI-AKITA do commit [não verificado].** [nao verificado]

A mensagem/metadados do commit não estão disponíveis nesta cópia sem `.git`; portanto, **presença e teor da linha LEI-AKITA de `4ab30d147` [não verificado]**. Os campos obrigatórios são `origem`, `testemunha`, `RED`, `quem-mais-le`, `juizes novos` e `padrao`. (`CLAUDE.md:62`)

Contra o código, a testemunha do universo foi centralizada corretamente, e a irmã usa o escritor existente. Entretanto, uma declaração de cura completa da classe COLISAO, de RED adversarial suficiente ou de preservação da exportada não seria verdadeira pelos contraexemplos acima. (`app/chamados/services/materializacao.py:110`; `app/chamados/services/materializacao.py:1442`; `app/chamados/services/materializacao.py:1238`; `app/chamados/tests/test_bug_par_projetado.py:695`; `app/chamados/services/validacao.py:555`)

## 13/11 17:55 · sessao 4725a19810ec · - · - · origem=Ronald (tmux)

**BLOQUEIA — complemento incompleto.** Contraexemplo: pergunta de hora respondida, recusada por `COLISAO`, com carimbo `VIA_FATO_OCUPADO`; `_carimbar_recusa` produz esse estado sem criar batida (`app/chamados/services/materializacao.py:1402`, `app/chamados/services/materializacao.py:1408`).

Esse carimbo passa no lastro por identidade (`app/chamados/models.py:2531`, `app/chamados/models.py:3823`, `app/chamados/juizes.py:679`). A testemunha devolve `aplicada`, porque excepciona somente `VIA_PARIDADE_RECUSADA` (`app/chamados/services/materializacao.py:688`). O explicador consequentemente não define `gravou=False` (`app/chamados/services/explicador.py:99`), e a Central imprime **“Tudo validado e gravado.”** (`app/chamados/services/desenlace.py:65`, `app/chamados/services/desenlace.py:72`).

Reprodução em memória com os quatro corpos reais extraídos por AST confirmou: `lastro=True → aplicada → resolvida sem gravou → Tudo validado e gravado`. O `pode_fechar=True` foi alimentação sintética, seguindo o molde do teste existente (`app/chamados/tests/test_bug_par_projetado.py:256`); ocorrência em produção: **[não verificado]**.

O teste do complemento cobre exclusivamente paridade recusada (`app/chamados/tests/test_bug_par_projetado.py:261`); o outro presume que ausência da marca significa gravação (`app/chamados/tests/test_bug_par_projetado.py:272`).

## 13/11 17:55 · sessao 5da9bf11e56d · HJ-090 · verificador · origem=Code

**BLOQUEIA a aprovação final:** a reversão ainda aceita registro anterior a uma remarcação e pode desfazer um apply posterior. As guardas conferem valores e `[pk, dias]`, sem identificar datas nem o ato revertido (`app/ferias/services.py:137,176-179,635-636`).

A identidade da cópia com o commit informado, o histórico Git, a execução da suíte e qualquer ocorrência em produção ficam **[nao verificado]**. A cópia disponibilizada não contém `.git`; não executei testes que escrevem arquivos ou banco. **[nao verificado]** [nao verificado]

**1. Triagem2: fechamento de cada PROCEDENTE**

| Item | Veredito | Conferência e contraexemplo |
|---|---|---|
| 1 — registro depois do atomic | **OK para a falha original; BLOQUEIA a reversão completa** | Agora write, flush e fsync acontecem dentro do atomic externo, mantendo a trava até a saída dele; falha desfaz o período (`app/ferias/management/commands/realinhar_concedidos_ferias.py:123-130`). Persistem os contraexemplos abaixo. |
| 2 — edição sobre foto, sem vendidos | **OK para o julgamento solicitado** | A porta relê sob trava e exige direito ≥ concedidos + vendidos; a view traduz `RecusaNaPorta` (`app/ferias/services.py:730-742`; `app/ferias/views.py:844-851`). Não localizei caminho adicional de edição do PA em admin/API/mensageria; o admin de férias registra somente `Afastamento` (`app/ferias/admin.py:2-5`). Existência de outro caminho externo à cópia: **[nao verificado]**. |
| 3 — contador ausente do carimbo | **OK** | Os dois sentidos entram no censo e em esperado=0; o leitor genérico compara e apresenta essas chaves (`app/chamados/management/commands/vigiar_fio_morto.py:98-99,227-246`; `bin/fio_estado.py:121-126,191-194`). |
| 4 — mensageria descarta saldo negativo | **OK no código; prova de regressão incompleta** | O produtor entrega o número e a ferramenta agora o repassa (`app/api/views_mensageria.py:835-836`; `mensageria/nucleo/ferramentas.py:2113-2115`). A retirada desse repasse não é exercitada pelos testes de volta3 (`app/ferias/test_hj090_volta3.py:1-245`). |
| 5 — `_recusou` aceita qualquer exceção | **OK** | Agora exige `RecusaNaPorta` e a mensagem esperada; há casos para propagar `TypeError` e rejeitar outro motivo (`app/ferias/test_hj090_red_por_valor.py:45-51`; `app/ferias/test_hj090_volta3.py:234-245`). |

Os resíduos concretos do item 1 são:

- **BLOQUEIA — remarcação não invalida o registro antigo.** PA com concedidos=10 e agendamento de 20 dias: realinhar grava 20; remarcar o mesmo agendamento mantendo 20 dias preserva PK, dias e concedidos; reverter o registro anterior passa e grava 10. A identidade ignora as datas, embora o contrato diga que remarcado não deve ser tocado (`app/ferias/services.py:134-137,168-179,635-636`). O teste existente troca o PK; não cobre remarcação do mesmo objeto (`app/ferias/test_hj090_um_juiz.py:212-218`).
- **BLOQUEIA — intenção sem commit pode desfazer outro apply.** A falha no fsync depois do flush pode deixar uma linha completa enquanto o atomic desfaz o banco. Depois, outro apply pode produzir os mesmos valores e vivos; reverter o arquivo da tentativa falha desfaz esse segundo ato. Também ocorre com apply A → reverter A → apply B → repetir reversão A (`app/ferias/management/commands/realinhar_concedidos_ferias.py:129-130`; `app/ferias/test_hj090_volta3.py:107-109`; `app/ferias/services.py:176-179`).
- **OK — JSON sintaticamente corrompido no meio não é tratado como cauda.** Uma linha completa inválida levanta antes do início das reversões; somente o EOF sem newline é ignorado (`app/ferias/management/commands/realinhar_concedidos_ferias.py:43-51,81-84`).
- **NAO-BLOQUEIA isoladamente — falta validar a estrutura inteira antes de reverter.** Uma linha `{}` no meio é aceita pelo parser; o `KeyError` aparece no processamento, depois de períodos anteriores poderem ter sido revertidos. Conferi esse comportamento executando somente a função extraída, com entrada em memória (`app/ferias/management/commands/realinhar_concedidos_ferias.py:49,81-84`). O teste cobre sintaxe inválida, não estrutura inválida (`app/ferias/test_hj090_volta3.py:150-158`).

Não encontrei, no apply normal do command, a antiga janela “banco confirmado, registro ainda não escrito” (`app/ferias/management/commands/realinhar_concedidos_ferias.py:123-130`). A porta pública, isoladamente, grava sem arquivo, mas exige trilha no banco; o command é o chamador produtivo localizado dessa porta (`app/ferias/services.py:150-164,404-412`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:124`).

**2. Escritores, leitores e significados**

O censo dos campos centrais ficou assim:

| Campo | Escritores persistentes | Leitores centrais |
|---|---|---|
| PA.`dias_direito` | Criação com defaults; art.130; sincronização; edição (`app/ferias/services.py:335-357,369-370,739-742`; `app/ferias/views.py:184-187`) | Status, saldo, teto de venda, validação da edição e apresentação (`app/ferias/models.py:98,220,238`; `app/ferias/services.py:732-735`; `app/templates/ferias/_secao_colab.html:48`) |
| PA.`dias_concedidos` | Realinhar, reverter, histórico, agendar, cancelar DP/admin, remarcar e aprovar solicitação (`app/ferias/services.py:156-158,179-181,474-477,509-511,577-581,604-613,635-651,706-708`) | Saldo, contador, reversão, edição, filtros e apresentação (`app/ferias/models.py:220`; `app/ferias/services.py:125-126,155,176,732`; `app/ferias/views.py:32,61`; `app/templates/ferias/_secao_colab.html:49`) |
| PA.`dias_vendidos` | Agendamento com abono, histórico e venda avulsa (`app/ferias/services.py:258-261,475-477,818-820`) | Saldo, vendáveis, consumo na edição e apresentação (`app/ferias/models.py:220,238`; `app/ferias/services.py:732`; `app/templates/ferias/_secao_colab.html:51`) |
| PA.`dias_saldo` | **Sem coluna/escritor:** property direito − concedidos − vendidos (`app/ferias/models.py:218-220`) | Portas de saldo/remarcação; situação/API/IA; listas, telas e relatórios (`app/ferias/services.py:297-302,470-472,631-633,690-691,814-815`; `app/ferias/situacao.py:300-302`; `app/api/views_mensageria.py:841`; `app/core/views_ia.py:492,532`; `app/ferias/management/commands/listar_ferias_dp.py:60,68`; `app/relatorios/services.py:316`; `app/relatorios/views.py:370`; `app/templates/ferias/_secao_colab.html:50,68`) |
| PA.`status` | Juiz atribui em memória; portas anteriores persistem; sincronização e garantia também escrevem (`app/ferias/models.py:207-215`; `app/ferias/services.py:330-344,374-378,740-742`) | Habilitação, listas, situação, API, relatórios e alertas (`app/ferias/models.py:234,242`; `app/ferias/views.py:31-32,57-61`; `app/ferias/services.py:208,685`; `app/api/views_mensageria.py:841`; `app/relatorios/services.py:285-310`; `app/ferias/management/commands/alertar_ferias_vencendo.py:23,35-38`) |
| Agendamento.`dias`/datas | Histórico, agendamento normal, solicitação aprovada e troca de janela (`app/ferias/services.py:449-450,506-508,636-649,697-705`) | Fracionamento, saldo/remarcação, soma, cobertura, listas, API e exportação (`app/ferias/models.py:363-386`; `app/ferias/services.py:105-106,137,577,635`; `app/ferias/management/commands/listar_ferias_dp.py:73-84`; `app/api/views.py:2244-2246`; `app/folha/export.py:491-501`) |
| Agendamento.`status` | Default na criação; juiz no histórico; cancelamentos; troca de janela; alinhamento por data (`app/ferias/models.py:319`; `app/ferias/services.py:451,506-508,580-581,612-613,641-649,695-705,837-841`) | Soma/identidade, guardas, espelho, situação, listas e API (`app/ferias/services.py:106,137,561-562`; `app/ponto/services/ausencia.py:1181`; `app/ferias/situacao.py:165-169`; `app/ferias/management/commands/listar_ferias_dp.py:62`; `app/api/views.py:2247-2253`) |

Outros consumidores identificados de PA/agendamento são os painéis do colaborador e de chamados, aviso da batida, inteligência, situação por mensageria e painel de ausência (`app/colaboradores/views.py:2847,2900`; `app/chamados/views.py:627`; `app/ponto/services/aviso_da_batida.py:74`; `app/inteligencia/views.py:17,91-98,275-289`; `app/inteligencia/management/commands/gerar_alertas.py:86,100`; `app/api/views_mensageria.py:903-905,1048-1050`; `app/ponto/views.py:2021-2026`).

Para as demais fontes:

| Fonte | Quem escreve | Quem lê |
|---|---|---|
| `Ausencia(tipo='ferias')` | Espelho cria aprovada por dia, remove no cancelamento, apaga janela antiga e rejeita no cancelamento admin (`app/ponto/services/ausencia.py:1190-1215,1263-1284`) | Guarda do agendamento, lista com batida, vigia e cobertura comum/folha (`app/ferias/models.py:433-436`; `app/ferias/management/commands/listar_ferias_com_batida.py:40-64`; `app/ponto/services/vigia_ausencia.py:214-220`; `app/ponto/turnos.py:2511-2535`; `app/ponto/services/fechamento.py:683-687`) |
| `logs/HJ-090/realinhar_*` | Command: snapshot e JSONL da intenção (`app/ferias/management/commands/realinhar_concedidos_ferias.py:99-130`) | Loader do command; porta confere concedidos e vivos (`app/ferias/management/commands/realinhar_concedidos_ferias.py:39-52,76-85`; `app/ferias/services.py:175-179`) |
| `ferias_soma_maior` / `ferias_concedidos_maior` | Vigia, pelo contador comum, com esperado zero (`app/chamados/management/commands/vigiar_fio_morto.py:98-99,229-246`) | Comparação e apresentação genéricas do carimbo (`bin/fio_estado.py:121-126,191-194`) |

**BLOQUEIA — a trava do saldo não serializa a colisão entre períodos diferentes da mesma pessoa.** Dois POSTs podem travar PAs distintos, ambos validar a mesma janela antes dos inserts e ambos criar agendamento. A validação de sobreposição é consulta, sem trava comum do colaborador; eventual duplicata de Ausencia vira recusa por dia e não necessariamente desfaz o agendamento (`app/ferias/services.py:492-508`; `app/ferias/models.py:423-454`; `app/ponto/services/ausencia.py:417-435,1210-1218`). Trata-se de interleaving permitido pelo código; reprodução concorrente em banco **[nao verificado]**.

**NAO-BLOQUEIA — sobrevive escritor público de direito sem trava.** `aplicar_art130(..., salvar=True)` salva diretamente; o caminho produtivo localizado o chama com `salvar=False`, já sob trava. Corrida por endpoint atual **[nao verificado]** (`app/ferias/services.py:327-335,350-357`; `app/ferias/models.py:160-164`).

Há leitores com significados incompatíveis:

- **NAO-BLOQUEIA — direito manual versus recalculado.** Editar direito para 35 com consumo 30 pode retornar sucesso e, na própria resposta HTMX, recalcular para 30: a resposta sincroniza períodos adquiridos e reaplica art.130. A intenção de preservar edição manual não está demonstrada no material fornecido (`app/ferias/services.py:333-344,739-742`; `app/ferias/views.py:795-802,854-855`; `app/ferias/models.py:54-55`).
- **NAO-BLOQUEIA — status inexistente no leitor.** Inteligência procura agendamento `aprovado`; os estados são agendado/em_gozo/concluido/cancelado. Um agendamento normal dos próximos 30 dias desaparece desse KPI e dessa lista (`app/inteligencia/views.py:94,278,286`; `app/ferias/models.py:296-300`).
- **NAO-BLOQUEIA — “vigente pelas datas” versus “vigente pelo status”.** A autoridade inclui concluído com data viva; situação e aviso de retorno filtram apenas agendado/em_gozo (`app/ferias/services.py:49-64`; `app/ferias/situacao.py:165-169`; `app/ferias/management/commands/alertar_retornos.py:20-22`).
- **OK na unidade central:** gozo usa dias corridos inclusivos; o espelho cria um registro por dia; exportação recorta pela competência e cruza o dia civil local da batida. A lista administrativa declara outro universo e identifica plantão da véspera (`app/ferias/services.py:450`; `app/ponto/services/ausencia.py:1195-1208`; `app/folha/export.py:486-501`; `app/ferias/management/commands/listar_ferias_com_batida.py:4-12,44-64`).

**3. Ordem de trava e rollback externo**

**NAO-BLOQUEIA — a inversão da exclusão permanece, mas o deadlock específico da triagem2 não está demonstrado.** A ordem usual é AG → PA → Ausencia; exclusão faz PA → Ausencia → delete do AG. Os escritores que recusam cancelado param antes de adquirir o PA, cortando o ciclo proposto naquele caso (`app/ferias/services.py:419,560-564,756-762`; `app/ferias/models.py:456-460`). Deadlock reproduzido: **[nao verificado]**.

**OK — a repetição da trava no agendamento com abono mantém a mesma ordem.** A porta trava PA, chama o agendamento que relê o mesmo PA e depois o relê novamente para vender. Não identifiquei inversão nessa sequência (`app/ferias/services.py:245-258,493`).

**NAO-BLOQUEIA isoladamente — arquivo pode sobreviver a rollback ancestral.** Se `call_command` for envolvido por outro atomic, o atomic do command pode ser somente savepoint; um rollback posterior deixa a intenção em disco. O código documenta expressamente intenção sem confirmação, mas imprime `APPLY` antes de qualquer commit ancestral (`app/ferias/management/commands/realinhar_concedidos_ferias.py:13-15,123-131`). Chamador produtivo que faça esse envolvimento: **[nao verificado]**. A reversão equivocada após estado posterior coincidente é o bloqueio já descrito no item 1 (`app/ferias/services.py:176-179`).

**4. Classes de reprovação, RED e costura**

**BLOQUEIA — criação e edição derivam o ciclo anual de maneiras diferentes.** Criar usa aniversário menos um dia; editar usa 364 dias fixos. Para início em 01/01/2024, criação termina em 31/12/2024 e edição do mesmo início termina em 30/12/2024, deslocando também a concessão (`app/ferias/views.py:140-144,169-173`; `app/ferias/services.py:736-738`).

**NAO-BLOQUEIA — continuam derivações paralelas de vencimento.** A autoridade registrada é `status_de_periodo`; `periodos_vencendo` exige status persistido adquirido, enquanto `vencendo` consulta o juiz. Um ciclo adquirido pelas datas, ainda marcado em_curso, aparece em um e some do outro. Há também janelas de 60 mantidas separadamente (`app/core/juizes.py:265`; `app/ferias/services.py:45,201-209`; `app/ferias/models.py:249,255-261`; `app/ferias/situacao.py:55,206-209`; `app/ferias/management/commands/pautas_de_ferias.py:68-74`).

**NAO-BLOQUEIA esta rodada — relógios internos da fila anterior persistem.** Aviso e status podem consultar dias diferentes durante a virada, inclusive na nova edição de período, que chama `aplicar_status()` sem receber hoje (`app/ferias/services.py:491,510,637-650,740`; `app/ferias/models.py:94,283`).

O vigia também não cumpre integralmente sua promessa de uma leitura: não transmite `agora` ao alcance; a alimentação acaba consultando novamente o relógio da fila. Um silêncio que expire durante a rodada pode ser julgado depois do instante assinado no carimbo (`app/chamados/management/commands/vigiar_fio_morto.py:162-176,232-233`; `app/chamados/services/vigia_fio.py:604`; `app/chamados/juizes.py:1576,1543`; `app/chamados/services/painel.py:190`). O command novo lê `localdate()` e `now()` separadamente, podendo entregar dias distintos ao carregador na meia-noite (`app/ferias/management/commands/listar_ferias_com_batida.py:36,52`).

**NAO-BLOQUEIA — exceções de programação ainda viram resultado de negócio.** `aprovar_solicitacao` já salva aprovada e captura qualquer exceção da materialização; um `TypeError` vira aviso e a view retorna aprovação com sucesso. Não é silêncio absoluto, mas o erro deixa de propagar (`app/ferias/services.py:676-679,693-710`; `app/ferias/views.py:780-782`).

**BLOQUEIA a alegação de trilha obrigatória em toda a obra.** Na correção de datas, a porta conclui o atomic antes de a view registrar o histórico; a auditoria é best-effort. Falha da trilha pode deixar a correção aplicada e a mensagem afirmar motivo registrado (`app/ferias/services.py:649-652`; `app/ferias/views.py:525,536-544`; `app/core/auditoria.py:17-29`). O realinhamento/reversão resolve isso com `_trilha`, mas essa proteção não foi generalizada (`app/ferias/services.py:161-162,182-183,404-412`).

Quanto à mordida dos testes: [nao verificado]

| Cura | Veredito por inspeção |
|---|---|
| Registro antes do commit | **OK** para write/short-write/flush/fsync do arquivo e preservação do período anterior (`app/ferias/test_hj090_volta3.py:98-121`). |
| Cauda truncada | **OK** para EOF parcial e sintaxe inválida completa; não cobre estrutura inválida, remarcação ou repetição de registro antigo (`app/ferias/test_hj090_volta3.py:123-158`; `app/ferias/test_hj090_um_juiz.py:149-155,212-218`). |
| Direito sob trava | **OK** para vendidos, foto velha, limite e presença de `FOR UPDATE` (`app/ferias/test_hj090_volta3.py:166-189`). |
| Carimbo | **OK** contra zero fixo, omissão e troca dos sentidos: usa resultados 1 e 2 (`app/ferias/test_hj090_volta3.py:206-229`). |
| Repasse na mensageria | **Sem mordida demonstrada**: volta3 não exercita a ferramenta (`mensageria/nucleo/ferramentas.py:2114`; `app/ferias/test_hj090_volta3.py:1-245`). |
| Fsync dos diretórios | **Sem mordida demonstrada**: a injeção restringe a falha ao fd do arquivo (`app/ferias/test_hj090_volta3.py:71-74`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:116-121`). |

Execução e RED anterior dessas provas: **[nao verificado]**.

**OK — não identifiquei perda da prova anterior de fio mudo pela costura nova.** Ela dubla somente férias; continuam afirmados os argumentos do contador de fio mudo, seus valores/esperados e o dia civil local. A integração de férias tem caso separado com contador real (`app/core/tests/test_fio_mudo_placar.py:129-139,230-247`; `app/ferias/test_hj090_volta3.py:206-229`).

**5. `FORA_DE_PIPELINE`**

| Command | Veredito |
|---|---|
| `listar_ferias_com_batida` | **OK** quanto a somente consultar/imprimir e exigir `--desde` (`app/config/crons.py:771-773`; `app/ferias/management/commands/listar_ferias_com_batida.py:24-25,40-69`). O tripwire citado não vigia esse mesmo universo: PA concedidos=soma com batida nas férias produz contador zero e ainda pode aparecer na lista (`app/ferias/services.py:123-127`; `app/ferias/management/commands/listar_ferias_com_batida.py:40-46`). |
| `listar_ferias_dp` | **NAO-BLOQUEIA — motivo incompleto.** “Só leitura” omite `--simular`, que cancela e escreve dentro de transação posteriormente desfeita, admitido apenas no banco chamado sombra (`app/config/crons.py:774-776`; `app/ferias/management/commands/listar_ferias_dp.py:52-53,98-115`). |
| `realinhar_concedidos_ferias` | **OK** para DRY default e apply com autor. Reverter também escreve, por modo explícito e com autor, sem exigir `--apply` (`app/config/crons.py:777-779`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:65-85,95-109`). |

O desenho 4a exige vigia de varredura e proíbe transformá-lo em reparador; não fundamenta tornar o realinhamento rotina (`CLAUDE.md:169-173`). Exigência de cadência específica para essas duas listas: **[nao verificado]**.

O command já apontado na triagem2 continua escrevendo sem `--apply`: `alertar_ferias_vencendo` chama a sincronização que altera status e direito; está agendado sem flag (`app/ferias/management/commands/alertar_ferias_vencendo.py:14-32`; `app/ferias/services.py:333-344`; `app/config/crons.py:708`). **NAO-BLOQUEIA**, resíduo já declarado.

**6. Impacto em dinheiro**

**BLOQUEIA afirmar que as fontes lidas pela folha ficam cercadas pela competência não exportada.** Histórico materializa férias; remarcação apaga ausências antigas e grava a janela nova; cancelamento admin rejeita/remove o espelho. Esses caminhos não fazem ali a consulta à exportação (`app/ferias/services.py:443-477,604-613,629-651`). A porta declara explicitamente que período fechado não recusa a materialização (`app/ponto/services/ausencia.py:1169-1170`).

Contraexemplo: um agendamento passado cobre dia com batida e retém o colaborador no TXT; remarcar para outro intervalo pode retirar essa retenção. A exportação consulta as datas atuais do agendamento e decide `ferias_com_batida` por elas (`app/folha/export.py:491-501,876-884`). A cobertura aprovada também alimenta o fechamento (`app/ponto/turnos.py:2516-2517`; `app/ponto/services/fechamento.py:683-687`).

Isso demonstra alteração de fonte e da 

[truncado em 20000 de 21079 caracteres]

## 13/11 17:51 · sessao 2860ede427e9 · - · - · origem=Ronald (tmux)

**BLOQUEIA — dinheiro do passivo em competência 10 exportada.** A porta 103 limpa carimbo/validação sem alterar resposta, deixando o admin validar novamente (`app/chamados/services/validacao.py:268-280`). Essa validação agenda recálculo **antes** da materialização (`:544-560`); se a batida for recusada, desfaz o carimbo sem reverter o atomic externo (`:570-577`; devolução `:197-210`). O callback roda no commit e chama o escritor do fechamento (`app/ponto/services/fechamento.py:1807-1824`), que atualiza inclusive campos calculados de aprovado/exportado (`:1145-1180`). A guarda temporária protege somente 09/2026 (`:36-43`, aplicação `:560-575`), portanto não protege 10/2026. Contraexemplo: pergunta 103 da competência 10 exportada, FM gravado divergente da derivação atual; admin valida, plantio recusa por FM exportado (`app/ponto/janelas.py:85-88`; `app/ponto/registro_batida.py:132-137`), mas o callback regrava as horas. Viola o aval raspado “validar não toca o gravado” **[anexo sem arquivo:linha]**. Existência/quantidade desse estado na produção **[não verificado]**.

**NAO-BLOQUEIA — FORA_DE_PIPELINE continua divergente, mas acompanha o problema anterior.** Texto continua remetendo à L-113 (`app/config/crons.py:770-774`), enquanto o aval específico exige preservação do gravado **[anexo sem arquivo:linha]**. Corrigir somente a redação não impede o recálculo acima (`app/chamados/services/validacao.py:553-560`).

**NAO-BLOQUEIA isoladamente — foto de reversão permanece antes da trava.** Comando coleta campos (`app/chamados/management/commands/bug_par_103.py:48-60`), grava JSON (`:77-79`) e depois chama a porta (`:83-85`), cuja trava só ocorre em `app/chamados/services/validacao.py:258-263`. Contraexemplo: A grava snapshot; B devolve a mesma pergunta; A chega à trava e retorna não-caso (`:261-262`). O arquivo A inclui pergunta que A não alterou; usá-lo cegamente desfaria B. Não há aplicador desse JSON demonstrado **[não verificado]**. A trilha efetiva contém antes/depois lidos sob trava (`:263-287`), mitigando reversão manual.

**OK — DRY não escreve pelo comando.** Retorna antes de `makedirs`/`open` (`app/chamados/management/commands/bug_par_103.py:65-79`); o teste correspondente só verifica carimbo preservado/saída DRY (`app/chamados/tests/test_bug_par_projetado.py:544-553`). Execução do teste **[não verificado]**.

**NAO-BLOQUEIA — RED_2c não protege a classificação de competência fechada.** Só injeta COLISAO e exige PARIDADE (`app/chamados/tests/test_bug_par_projetado.py:695-707`). Mutante “qualquer recusa do chokepoint vira PARIDADE” satisfaz essas asserções. O vizinho RED_2b injeta competência fechada, mas cobra apenas rollback/carimbo nulo (`:672-681`), sem cobrar código COMPETENCIA. Execução de mutantes **[não verificado]**.

**RED5/RED7 da triagem:** os testes atuais de modal cobram frases de apresentação (`app/chamados/tests/test_bug_par_projetado.py:766-798`); RED7 executa dois applies e cobra dois arquivos, com conteúdos de tamanhos `[0,1]` (`:838-857`). Scripts/mutações exatas chamados “RED5/RED7” e sua sobrevivência **[não verificado]**; não inferir execução pelo nome dos testes.

**LEI-AKITA do commit:** mensagem/metadados de 4ab30d147 ausentes nesta cópia sem `.git` **[não verificado]**. Não certifiquei presença/veracidade dos campos da linha do commit. A regra exige `origem`, `testemunha`, `RED`, `quem-mais-le`, `juizes novos`, `padrao` (`CLAUDE.md:60-68`); confirmar números de linha desse último trecho pelo parent, pois foram lidos inicialmente sem `nl` **[não verificado]**.

## 13/11 17:51 · sessao e1bf90c104d0 · - · - · origem=Ronald (tmux)

Item 3 — censo de escrita/leitura e efeitos da mensagem nova:

| Campo | Escritores efetivos | Leitores decisórios |
|---|---|---|
| `via_resolucao` | Atribuição central em `app/chamados/models.py:3127`, usada por `carimbar`, `carimbar_identidade`, `descarimbar`; lote em `app/chamados/models.py:3308`; duas portas que só trocam via: `app/chamados/models.py:3317` e `app/chamados/services/relavra_via.py:125`. | C6 lê projeção temporal em `app/chamados/juizes.py:1386` e identidade em `app/chamados/juizes.py:1438`; lastro em `app/chamados/juizes.py:679`; SQL de vivas em `app/chamados/models.py:3398`; estado de aplicação em `app/chamados/services/materializacao.py:692`; universo103 em `app/chamados/services/materializacao.py:276`; flip automático em `app/ponto/services/flip_auto.py:129`. |
| `materializada_em` | Atribuição central `app/chamados/models.py:3125`; lote `app/chamados/models.py:3308`; `BUG-PAR-103` chama descarimbo, sem escrita paralela, em `app/chamados/services/validacao.py:268`. | C6 `app/chamados/juizes.py:1386`, `app/chamados/juizes.py:1438`, `app/chamados/juizes.py:1444`; estado `app/chamados/models.py:3069`; aplicação `app/chamados/services/materializacao.py:688`; fila `app/chamados/services/materializacao.py:1744`; terminalidade COLISAO `app/chamados/services/materializacao.py:1402`; devolução `app/chamados/services/validacao.py:192`; universo103 `app/chamados/services/materializacao.py:276`. |
| `validada_em` | Validação `app/chamados/services/validacao.py:136`; devolução comum `app/chamados/services/validacao.py:201`; devolução103 `app/chamados/services/validacao.py:271`; zeragem na resposta/correção `app/chamados/services/disputa_emissao.py:1480`; zeragem na reabertura `app/chamados/models.py:1508` e lote `app/chamados/models.py:1567`; autovalidação pela porta do carimbo `app/chamados/models.py:3161`; restauração em memória `app/chamados/models.py:3190`. | C6 mata validada em `app/chamados/juizes.py:1401`; estado `app/chamados/models.py:3067`; guarda de já validada `app/chamados/services/validacao.py:498`; exigência da irmã `app/chamados/services/materializacao.py:195`; fila `app/chamados/services/materializacao.py:1743`; projeção histórica `app/chamados/juizes.py:1385`; trilha a partir da projeção `app/chamados/models.py:4021`. |
| `recusa_motivo` | Cinco produtores semânticos: recusa da porta `app/chamados/services/materializacao.py:1411` (ou mesma gravação da identidade via `app/chamados/models.py:3214`); pendência de marco `app/chamados/services/materializacao.py:1125`; recusa humana `app/chamados/services/validacao.py:705`; contestação respondida `app/chamados/services/disputa_emissao.py:1497`; duplicata por porta identidade `app/chamados/services/fio.py:1399`. | Código do motivo decide carimbo terminal em `app/chamados/services/materializacao.py:1400`; universo de paridade/103 em `app/chamados/services/materializacao.py:262`; retorno do passivo em `app/chamados/services/passivo_validado.py:144` e `app/chamados/services/passivo_validado.py:567`; devolução exige presença em `app/chamados/services/validacao.py:194`; regex do flip em `app/ponto/services/flip_auto.py:132`; I18/I19 testam vazio em `app/chamados/management/commands/auditar_invariantes_chamados.py:1075` e `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`. |
| `Batida.retratada_em` | Escrita dinâmica `anotar`: `app/ponto/registro_batida.py:253`; retratação canônica fornece `timezone.now()` em `app/ponto/registro_batida.py:274`; desfazer humano fornece `None` em `app/ponto/services/retratar_humano.py:207`. Chamam retratação: humano `app/ponto/services/retratar_humano.py:179`, geofence `app/chamados/services/acoes_chamado.py:277`, par relâmpago `app/ponto/management/commands/detectar_par_relampago.py:224`, passivos `app/ponto/management/commands/diff_passivo_s84.py:82` e `app/ponto/management/commands/passivo_disputa_retro.py:164`. | Chokepoint filtra retratadas em `app/ponto/registro_batida.py:109` e `app/ponto/registro_batida.py:141`; pareador `app/ponto/turnos.py:1864`; lastro `app/chamados/juizes.py:755`; novo vizinho delega universo em `app/chamados/services/materializacao.py:107`; outros consumidores de apuração usam porta/filtragem em `app/ponto/turnos.py:1902`, `app/ponto/turnos.py:2221`, `app/ponto/turnos.py:2353`, `app/ponto/turnos.py:2384`, `app/ponto/turnos.py:2469`, `app/ponto/turnos.py:2876`, `app/ponto/turnos.py:2915`. |

**OK — carimbo BUG-PAR-103 usa porta e trilha.** A elegibilidade vem do predicado único em `app/chamados/services/materializacao.py:265`; a porta trava e reconsulta em `app/chamados/services/validacao.py:258`; descarimba e zera validação em `app/chamados/services/validacao.py:268`; grava `DecisaoPergunta.DEVOLUCAO` cujo motivo começa com BUG-PAR-103 em `app/chamados/services/validacao.py:276`; grava evento antes/depois em `app/chamados/services/validacao.py:281`; ausência do evento aborta em `app/chamados/services/validacao.py:288`. O leitor do rótulo consulta precisamente esse ato e prefixo em `app/chamados/services/materializacao.py:285`, chamado pela testemunha em `app/chamados/services/materializacao.py:697`.

**BLOQUEIA — texto livre controla terminalidade e agora generaliza COLISAO.** `_carimbar_recusa` trunca em 240 ANTES de traduzir e só carimba terminal se tradução = COLISAO em `app/chamados/services/materializacao.py:1399`. A nova mensagem classifica PARIDADE pela regra `guarda de paridade`/`par quebrado`, antes da regra COLISAO, em `app/chamados/catalogo/recusa.py:34`; isso é o comportamento pretendido para irmã desfeita, mas também transforma colisão ALHEIA persistente em pendente porque o ramo só verifica o código, sem identidade da batida, em `app/chamados/services/materializacao.py:1238`. A devolução então zera validação quando `materializada_em=None`, em `app/chamados/services/validacao.py:192` e `app/chamados/services/validacao.py:201`. O contador passa a contar essa colisão como paridade em `app/chamados/services/materializacao.py:262`. Contraexemplo: registrar a segunda metade recusa por batida alheia que permanece após rollback; qualquer nova validação repete o mesmo ramo e volta ao admin, sem produzir `fato_ocupado` — sequência demonstrável pela composição das linhas citadas; ocorrência em produção `[não verificado]`.

**OK — marca nova não cai numa regra anterior errada.** A única regra anterior à PARIDADE genérica procura `falta a outra metade do intervalo`, também de código PARIDADE, em `app/chamados/catalogo/recusa.py:29`; depois vêm COMPETENCIA em `app/chamados/catalogo/recusa.py:43` e COLISAO em `app/chamados/catalogo/recusa.py:74`. O catálogo usa `any()` e primeira regra correspondente em `app/chamados/catalogo/recusa.py:89`, portanto posição da marca dentro da string não muda prioridade; pô-la no começo apenas protege da truncagem de `app/chamados/services/materializacao.py:1399`.

**NAO-BLOQUEIA isoladamente — censo dos outros leitores textuais.** Tradução para mensagem aparece em `app/chamados/services/fio.py:741`, `app/chamados/services/acoes_disputa.py:34`, `app/chamados/services/acoes_disputa.py:174` e `app/chamados/services/validacao.py:643`; a nova classificação muda a instrução mostrada para “vizinha do MESMO tipo”, embora a causa original seja oposto em menos de 300s, conforme `app/chamados/catalogo/recusa.py:35` e `app/chamados/catalogo/recusa.py:75`. O regex independente do flip exige `batida vizinha #<id> (<tipo>` em `app/ponto/services/flip_auto.py:132`; a nova mensagem não contém essa estrutura (`app/chamados/services/materializacao.py:1240`), mas a própria consulta exige `via_resolucao=VIA_PARIDADE_RECUSADA` em `app/ponto/services/flip_auto.py:129`, via que o ramo novo não grava porque PARIDADE não carimba em `app/chamados/services/materializacao.py:1402`. Portanto o texto novo não cria um flip automático por esse leitor.

**OK quanto a escritor novo do delta — portas já têm tripwires.** O conjunto permitido para carimbo e via está explícito em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:44` e `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`, e o selo compara escritores em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:226` e `app/chamados/tests/test_o261e_f2_porta_carimbo.py:238`. `validada_em` tem dono e exceções por módulo em `app/chamados/tests/test_contract_dono_validada.py:27` e `app/chamados/tests/test_contract_dono_validada.py:30`; a descrição “allowlist vazia” está desatualizada diante das três entradas nesse mesmo arquivo (`app/chamados/tests/test_contract_dono_validada.py:14`, `app/chamados/tests/test_contract_dono_validada.py:34`). Nenhum teste foi executado; verde da suíte `[não verificado]`.

Este censo cobre os produtores semânticos e leitores decisórios dos campos no fluxo BUG-PAR; completude de todos os leitores indiretos/dinâmicos e comandos de toda a árvore `[não verificado]`.

## 13/11 17:50 · sessao 4725a19810ec · - · - · origem=Ronald (tmux)

**Item 2 — OK na troca de universo.** Os cinco usos julgam efeito de plantio, não exibem marcação:

| Chamador | Pergunta respondida |
|---|---|
| `app/chamados/services/materializacao.py:215` | A metade sozinha exige par? |
| `app/chamados/services/materializacao.py:228` | Cada metade aceita a irmã projetada? |
| `app/chamados/services/materializacao.py:712` | A tela pode anunciar plantio aplicável? |
| `app/chamados/services/materializacao.py:1150` | O plantio é permitido pela paridade? |
| `app/chamados/services/materializacao.py:1161` | Após gravar a irmã, a paridade continua válida? |

A lei distingue apuração de exibição crua (`app/docs/LEIS.md:76`); a porta exclui somente retratadas, preservando pendentes de DP (`app/ponto/turnos.py:2856`, `app/ponto/turnos.py:2876`). A troca usa essa porta nos dois lados, sem remontar universo manual (`app/chamados/services/materializacao.py:110`, `app/chamados/services/materializacao.py:113`).

**Contraprova ao possível falso positivo do cru:** `_batida_existente_na_janela` permanece deliberadamente crua (`app/chamados/juizes.py:243`). O contrato explica que sua pergunta é impedir replantio da marca já retratada; ignorá-la faria ciclo cluster → retratação → replantio (`app/chamados/juizes.py:806`). Isso não justifica cru na guarda de vizinhança, que responde outra pergunta (`app/chamados/services/materializacao.py:94`).

**`passivo_validado.py:497` — OK, sem divergência introduzida.** O trecho descreve uma guarda adicional de geometria do dia inteiro; mede antes, chama o mesmo materializador e reprova se aumentam turnos abertos/saídas órfãs (`app/chamados/services/passivo_validado.py:527`, `app/chamados/services/passivo_validado.py:537`, `app/chamados/services/passivo_validado.py:557`). A contagem consulta o pareador, sem construir batidas em paralelo (`app/chamados/services/materializacao.py:453`). Contraexemplo da própria especificação: plantar avulsa pode manter alternância local e transformar `E* S` em `E* S E`; essa guarda adicional continua necessária (`app/chamados/services/passivo_validado.py:492`).

**RED_2d — OK por inspeção: exige as duas curas.** Se somente anterior migrar, a primeira consulta ainda encontrará `E 03:00` retratada no posterior, escondendo `S 06:30`, e falhará em `assertIsNotNone` (`app/chamados/tests/test_bug_par_projetado.py:715`, `app/chamados/tests/test_bug_par_projetado.py:717`). Se somente posterior migrar, o segundo cenário deixa `S 00:30` retratada esconder a `E 18:30` anterior, falhando na segunda consulta (`app/chamados/tests/test_bug_par_projetado.py:721`, `app/chamados/tests/test_bug_par_projetado.py:723`). Execução efetiva/mutante: **[não verificado]**.

**Classes de regressão — nenhum segundo escritor de Batida nesse caminho identificado.** A irmã passa pelo mesmo `_materializar_uma`, com projeção temporária protegida por `finally` (`app/chamados/services/materializacao.py:1440`); o plantio efetivo chama `registrar_batida` (`app/chamados/services/materializacao.py:1219`). `par_do_intervalo` concentra os mesmos critérios para tela, validação e escrita, com ambos os lados projetados (`app/chamados/services/materializacao.py:178`, `app/chamados/services/materializacao.py:227`).

**Exceções — risco existente, não introduzido pelo delta apresentado.** `_materializar_uma` transforma qualquer `Exception` em texto de recusa; o `finally` o envia ao classificador textual (`app/chamados/services/materializacao.py:1636`, `app/chamados/services/materializacao.py:1648`). Já a segunda opinião posicional, em erro, registra warning e retorna falso: restringe a autorização, sem aprovar plantio no escuro (`app/chamados/services/materializacao.py:421`). Um erro concreto engolido em produção é **[não verificado]**.

## 13/11 17:50 · sessao 206e3fa43e2b · - · - · origem=Ronald (tmux)

Censo e achados enviados; principais pontos para consolidação:

- **Cura 2 fecha o julgamento da edição:** único caller produtivo encontrado é `app/ferias/views.py:846`; julgamento usa linha relida e travada em `app/ferias/services.py:731-742`. Admin de férias registra somente Afastamento (`app/ferias/admin.py:2-5`).

- **Escritor público de direito ainda sem trava:** `aplicar_art130(pa, salvar=True)` salva diretamente em `app/ferias/services.py:350-357`, exposto por `app/ferias/models.py:160-164`. O caller produtivo encontrado usa `salvar=False` sob trava (`app/ferias/services.py:327-335`); corrida por endpoint atual **[nao verificado]**.

- **Direito editado pode ser imediatamente sobrescrito:** editar para 35 com consumo 30 gera status adquirido (`app/ferias/services.py:739-742`); resposta HTMX chama contexto (`app/ferias/views.py:854-855`), que sincroniza períodos (`app/ferias/views.py:795-802`); sincronização recalcula direito pelo art.130 (`app/ferias/services.py:333-344`). Sem faltas, volta a 30 pelo juiz (`app/ferias/models.py:54-55`). Contraprova estática; execução **[nao verificado]**.

- **Concorrência entre PAs diferentes não é serializada por colaborador:** cada criação trava apenas seu PA (`app/ferias/services.py:492-503`). As duas requisições podem passar pelo veto antes dos inserts (`app/ferias/models.py:423-442`). A constraint da Ausencia não necessariamente desfaz o Agendamento: INSERT tem savepoint (`app/ponto/services/ausencia.py:417-418`), duplicata vira AusenciaInvalida (`app/ponto/services/ausencia.py:430-435`), e espelho captura por dia e continua (`app/ponto/services/ausencia.py:1210-1218`). Interleaving estático possível; reprodução runtime **[nao verificado]**.

- **Ordem usual:** AG→PA (`app/ferias/services.py:560-564`), criação apenas PA (`app/ferias/services.py:493`), Ausencia depois. Exclusão inverte para PA→Ausencia→AG (`app/ferias/services.py:756-762`, `app/ferias/models.py:456-460`), mas os caminhos para cancelados recusam antes de pedir PA (`app/ferias/services.py:561-562`); deadlock desse caso **[nao verificado]**.

- **Fontes da folha mudam sem consultar exportada:** troca apaga Ausencias antigas e grava datas novas (`app/ferias/services.py:629-651`), histórico cria e materializa (`app/ferias/services.py:443-477`). A porta declara que período fechado não recusa férias (`app/ponto/services/ausencia.py:1169-1170`). Folha lê cobertura aprovada (`app/ponto/turnos.py:2516-2517`, `app/ponto/services/fechamento.py:683-687`); export lê datas inclusivas de Agendamento (`app/folha/export.py:491-493`) e altera retenção de quem teve batida (`app/folha/export.py:876-884`). Alteração de TXT já emitido ou de FechamentoMensal gravado **[nao verificado]**; a proteção deste último é distinta (`CLAUDE.md:411`).

- **Leitor com status errado:** KPI/lista próximos 30 dias filtra `aprovado` (`app/inteligencia/views.py:94`, `app/inteligencia/views.py:278`, `app/inteligencia/views.py:286`), enquanto escolhas de Agendamento são agendado/em_gozo/concluido/cancelado (`app/ferias/models.py:296-300`). Agendamento normal fica invisível nesse leitor.

- **Trilha fora do commit de correção:** correção de datas executa porta e só depois registra_log (`app/ferias/views.py:525`, `app/ferias/views.py:536`); atomic da porta termina antes (`app/ferias/services.py:649-652`). Falha posterior de trilha deixa mudança aplicada; `_trilha` obrigatório aparece somente no realinhamento/reversão (`app/ferias/services.py:161`, `app/ferias/services.py:182`).

Censo Ausencia ferias: cria aprovada por dia (`app/ponto/services/ausencia.py:1203-1208`), apaga no espelho (`app/ponto/services/ausencia.py:1190-1192`, `app/ponto/services/ausencia.py:1212-1215`), apaga janela (`app/ponto/services/ausencia.py:1263-1271`), rejeita admin (`app/ponto/services/ausencia.py:1282-1284`). Leitores diretos adicionais: guarda de sobreposição (`app/ferias/models.py:433-436`), lista com batida (`app/ferias/management/commands/listar_ferias_com_batida.py:40-64`), vigia de ausência (`app/ponto/services/vigia_ausencia.py:214-220`). Cobertura comum inclusiva e efeitos por destinatário: `app/ponto/turnos.py:2511-2535`, `app/ponto/turnos.py:2735-2782`.

## 13/11 17:50 · sessao aee2833cbbc9 · - · - · origem=Ronald (tmux)

1. **Cura 1: fecha a falha original de write/flush/fsync, mas a reversão continua incompleta.** O registro é escrito e sincronizado dentro do atomic externo antes da saída que confirma o período; erro desfaz o banco (`app/ferias/management/commands/realinhar_concedidos_ferias.py:123`, `:127`, `:129`, `:130`). Os testes mordem as quatro falhas de escrita e preservação do primeiro período (`app/ferias/test_hj090_volta3.py:98`, `:111`). Execução desses testes: [nao verificado].

2. **BLOQUEIA — intenção não aplicada pode reverter um apply posterior.** Uma falha em fsync depois do flush pode deixar linha completa, com rollback do período; o teste reconhece essa possibilidade (`app/ferias/management/commands/realinhar_concedidos_ferias.py:129`; `app/ferias/test_hj090_volta3.py:107`). Contraexemplo: apply A falha; apply B, posterior, realinha o mesmo período e mesmos agendamentos; `--reverter` do arquivo A passa porque a guarda só compara concedidos e `[pk,dias]`, desfazendo B (`app/ferias/services.py:176`, `:179`). Também ocorre com apply A → reverter A → apply B → repetir reverter A. Não há identidade do ato nem conferência da trilha nessa guarda (`app/ferias/services.py:176`). Os testes existentes só alteram concedidos ou trocam o PK dos agendamentos (`app/ferias/test_hj090_um_juiz.py:149`, `:212`).

3. **BLOQUEIA — remarcação com os mesmos dias não invalida a reversão.** O contrato promete recusar período cujo agendamento foi remarcado (`app/ferias/services.py:170`), mas a identidade contém somente PK e dias (`app/ferias/services.py:134`, `:137`). Contraexemplo: direito 30, concedidos 10, agendamento de 20; realinha para 20; remarca o mesmo agendamento para outra janela mantendo 20 dias; a remarcação preserva concedidos e `[pk,dias]` (`app/ferias/services.py:635`, `:636`); reverter o registro anterior passa e grava 10 (`app/ferias/services.py:176`, `:179`). O teste troca o agendamento por outro PK e não cobre esse caso (`app/ferias/test_hj090_um_juiz.py:212`).

4. **NAO-BLOQUEIA/limitação de integridade — parser valida sintaxe, não estrutura.** Cauda sem newline só pode ser a última linha; JSON sintaticamente inválido em linha completa provoca recusa antes de qualquer reversão (`app/ferias/management/commands/realinhar_concedidos_ferias.py:43`, `:49`, `:51`, `:81`). Portanto a hipótese de ignorar JSON inválido no meio **não procede nesse sentido**. Contudo `{}` com newline no meio é JSON válido, entra no registro e causa `KeyError` apenas no processamento; períodos anteriores já podem ter sido revertidos (`app/ferias/management/commands/realinhar_concedidos_ferias.py:49`, `:82`, `:83`). O teste cobre somente `{invalido}\n` (`app/ferias/test_hj090_volta3.py:150`, `:155`).

5. **Rollback ancestral: possível na API, caller produtivo adicional não demonstrado.** O command usa atomic comum, sem impedir aninhamento; seu arquivo permanece se um atomic ancestral posteriormente desfizer o banco (`app/ferias/management/commands/realinhar_concedidos_ferias.py:123`, `:130`). A documentação o chama expressamente de intenção sem confirmação (`:13`). O teste herda de `TestCase`, logo não prova commit efetivo por período (`app/ferias/test_hj090_volta3.py:81`; `app/ferias/test_hj090_um_juiz.py:63`). Caller produtivo que envolva esse command em outro atomic: [nao verificado]. A porta pública escreve sem arquivo, mas grava trilha obrigatória; único caller produtivo localizado é o command (`app/ferias/services.py:150`, `:161`, `:164`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:124`).

6. **Mordida dos testes:** curas de direito, trava, foto velha, vendidos, contador nos dois sentidos e exceção específica possuem casos que atingem seus mutantes naturais (`app/ferias/test_hj090_volta3.py:166`, `:173`, `:181`, `:206`, `:234`, `:241`). Não há mordida para retirar fsync dos diretórios: a injeção só falha no descritor do arquivo (`app/ferias/test_hj090_volta3.py:71`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:116`). A cura da mensageria não é exercitada por volta3; retirar o repasse não muda seus asserts (`mensageria/nucleo/ferramentas.py:2114`; `app/ferias/test_hj090_volta3.py:206`, `:245`).

7. **Costura do placar: não identifiquei perda de prova anterior.** A costura acrescenta somente a nova derivação externa; mantém command, gravação real, argumentos do contador e asserts anteriores (`app/core/tests/test_fio_mudo_placar.py:116`, `:129`, `:131`, `:135`, `:136`). A integração de férias tem teste separado com contador real envolvido por `wraps`, dados divergentes 1×2 e conferência do carimbo (`app/ferias/test_hj090_volta3.py:206`, `:215`, `:219`, `:224`).

8. **Possível confusão entre tenants — evidência de desenho, ocorrência não verificada.** O registro não contém schema, apenas período/antes/depois; reversão procura o PK no tenant corrente (`app/ferias/services.py:164`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:83`). Se dois tenants tiverem PKs e `[pk,dias]` iguais, arquivo do primeiro pode alterar o segundo (`app/ferias/services.py:176`, `:179`). Invocação equivocada em produção: [nao verificado].

## 13/11 17:50 · sessao 7f1417515870 · - · - · origem=Ronald (tmux)

Achados e evidências do recorte:

1. **OK — triagem2 #3, contador no carimbo.** O produtor usa `ferias_concedidos_x_soma`, grava ambos sentidos e declara esperado zero; o leitor compara todas as chaves de `esperado` e publica divergências sem limiar próprio. Contraexemplo removido: soma>concedidos agora aparece no ESTADO. `app/chamados/management/commands/vigiar_fio_morto.py:98-99,227-246`; `bin/fio_estado.py:121-126,191-194`; `bin/gerar_estado.py:260-261`.

2. **OK — triagem2 #4, mensageria.** `saldo_negativo_dias` passa do resultado do juiz para API e ferramenta, preservando inclusive o número negativo. `app/ferias/situacao.py:297-302`; `app/api/views_mensageria.py:835-836`; `mensageria/nucleo/ferramentas.py:2113-2115`. O novo teste `CarimboFerias` cobre contadores distintos `(1,2)`, esperado zero e chamada ao contador real; morde omissão, zero fixo e troca de sentidos. `app/ferias/test_hj090_volta3.py:206-229`. Execução dos testes [nao verificado].

3. **NAO-BLOQUEIA — relógio ainda escapa no vigia, preexistente.** O comentário promete uma leitura; `handle` captura `agora`, mas não o transmite a `censo_alcance`. Este chama `alimentar_portas` sem `agora`, que chega como `None` à fila e provoca novo `timezone.now()`. Contraexemplo: instante capturado 23:59:59, chamado silenciado até 00:00:00; consulta posterior já o considera porta, carimbo assina instante/dia anterior. `app/chamados/management/commands/vigiar_fio_morto.py:162-176,232-233`; `app/chamados/services/vigia_fio.py:604`; `app/chamados/juizes.py:1576,1531-1543`; `app/chamados/services/painel.py:190`. O contador novo de férias não usa relógio. `app/ferias/services.py:116-127`.

4. **NAO-BLOQUEIA — dois relógios no command novo.** `listar_ferias_com_batida` lê `localdate()` e `now()` separadamente. Contraexemplo: virada da meia-noite entre chamadas entrega `hoje` do dia anterior e `agora` do seguinte ao carregador e à ordenação de competências. `app/ferias/management/commands/listar_ferias_com_batida.py:36,52,61`.

5. **NAO-BLOQUEIA — motivo incompleto em FORA_DE_PIPELINE.** `listar_ferias_dp` é descrito como “só leitura, sem efeito em dado”, porém `--simular` cancela via porta e escreve dentro de transação desfeita. A própria recusa da flag admite a escrita; só aceita `DATABASES.default.NAME == 'sombra'`. Persistência dessas alterações não demonstrada: há rollback explícito e tratamento depois do atomic. `app/config/crons.py:774-776`; `app/ferias/management/commands/listar_ferias_dp.py:52-53,98-115`. A guarda não confere `HOST`, embora a configuração da sombra declare nome e host como requisitos de isolamento. `app/config/settings/sombra.py:29-50`.

   Os motivos dos outros dois são compatíveis: lista exige `--desde`, consulta ausências/batidas e imprime; realinhamento sai antes da escrita no DRY e exige autor para apply/reverter. `app/config/crons.py:771-779`; `app/ferias/management/commands/listar_ferias_com_batida.py:24-25,40-69`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:65-85,95-109`. Não há fundamento no desenho 4a para transformar reparo pontual em rotina; o desenho determina vigia de varredura, sem reparo. `CLAUDE.md:169-173`.

6. **OK — costura de fio mudo preserva a prova anterior identificável.** A costura nova dubla somente a derivação de férias; o contador de fio mudo continua com chamada e valores afirmados, incluindo esperado diferente de zero, dia civil local e alarme no placar. `app/core/tests/test_fio_mudo_placar.py:129-139,230-247`. O placar continua consumindo exclusivamente as chaves fio mudo e nunca bateu; essa costura não prova alarme de férias no placar, nem o código atual oferece esse alarme. `app/core/management/commands/pipeline_placar.py:85-107,135-136`. Antes/depois da execução da suite [nao verificado].

7. **NAO-BLOQUEIA — leitores antigos ainda divergem do significado declarado de férias vigentes.** A autoridade inclui qualquer agendamento não cancelado coberto pelas datas, explicitamente inclusive concluído com data viva; `situacao_de_ferias` e `alertar_retornos` filtram só agendado/em_gozo. Contraexemplo: concluído com datas no presente é férias para o juiz e folha, mas some da lista da API e do aviso de retorno. `app/ferias/services.py:49-64`; `app/ferias/situacao.py:165-169,303`; `app/api/views_mensageria.py:843-846`; `app/ferias/management/commands/alertar_retornos.py:20-22`.

8. **NAO-BLOQUEIA — derivação paralela de vencimento, preexistente.** `periodos_vencendo` exige `status='adquirido'` persistido; o juiz `vencendo` pergunta `status_de_periodo` ao vivo. Contraexemplo: aquisição completou e cron ainda não atualizou em_curso: modelo diz vencendo, service omite. `app/ferias/services.py:201-209`; `app/ferias/models.py:255-260`. Janelas de 60 dias também continuam separadas em `services`, `situacao` e `pautas_de_ferias`, apesar de existir `DIAS_VENCENDO` no modelo. `app/ferias/services.py:45`; `app/ferias/situacao.py:55,206-209`; `app/ferias/management/commands/pautas_de_ferias.py:68-74`; `app/ferias/models.py:249,255`.

9. **NAO-BLOQUEIA — command escreve sem --apply, já listado na triagem2.** `alertar_ferias_vencendo` chama sincronização que pode gravar status e direito; não declara argumentos de apply e está no cron sem flag. `app/ferias/management/commands/alertar_ferias_vencendo.py:14-32`; `app/ferias/models.py:152-158`; `app/config/crons.py:708`. A mesma rotina também envia push e relata falha individual sem apagar o erro da saída. `app/ferias/management/commands/alertar_ferias_vencendo.py:84-88`.

## 13/11 17:46 · sessao a92e0d0adb4b · E3-FORMA-SQL · leitura · origem=Code

**Resultado: 0 regressões demonstradas pelos casos E3 escritos; os 79 ficam em “sem caso escrito para a combinação observada”.** Isso deixa a decisão normativa desses 79 em aberto; a perda de porta está demonstrada, mas sua classificação como violação não. (`v5_det.txt:1–79`; `app/chamados/tests/test_e3_portas_tarefa.py:21–127`)

Referências abreviadas:

- **V:n** = `v5_det.txt:n`, contando **somente as linhas `V5_DET`** fornecidas.
- **T:n** = [app/chamados/tests/test_e3_portas_tarefa.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/test_e3_portas_tarefa.py):n.
- **S:n** = [app/chamados/services/tarefa_do_dia.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py):n.
- **J:n** = [app/chamados/juizes.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py):n.
- **F:n** = [app/chamados/tests/e3_fixtures.py](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/tests/e3_fixtures.py):n.

**1–2. Classes, contagens, exemplos e cobertura.**

A classificação abaixo agrupa multiplicidade e ordem das razões; preserva seus conjuntos, módulo/status e diferenças de âncora e ações. `P=passivo_sem_linha`, `A=aceso`, `R=aguarda_resposta`, `W=dia_sem_trabalho`, `V=veredito_sem_lavra_vigente`, `J=encerrada_pelo_juiz`. (`V:1–79`)

Na coluna âncora: **C** = presente, `fila_ids=true/fila_cel=true`; **G** = presente, `true/false`; **X** = presente, `false/false`; **—** = nenhuma pergunta/âncora exposta nas instâncias. Isso não comprova inexistência de perguntas suprimidas pela tarefa. (`V:1–79`; `S:262–271`)

Módulos: **TA**=`turno_aberto`, **BA**=`batida_ausente`, **DS**=`disputa_supervisao`, **AC**=`ausencia_colab`, **PR**=`par_relampago`, **TA24**=`turno_aberto_24h`; status **a**=`aberto`, **e**=`em_analise`. A coluna descreve os chamados relacionados no V5; módulo/status de âncoras não relacionadas ali permanecem **[não verificado]**. (`V:1–79`)

**A coluna de pks é exaustiva; seus primeiros dois elementos são os exemplos. Classes unitárias só têm um exemplo disponível.** (`V:1–79`)

| Classe | Rota ATUAL | Módulo/status | Razões; âncora; ações | n | celula_pk — lista completa e evidência | Cobertura |
|---|---|---|---|---:|---|---|
| C01 | fila | TA/e | A,R; X; responder/colaborador/null | 1 | 141714 (V:1) | SC-R |
| C02 | fila | TA/e | A,P; C; vazias | 2 | 130162 (V:2), 135778 (V:17) | SC-P |
| C03 | fila | BA/e | A,P,R; R em X, P em X e C; responder/colaborador/null | 1 | 118931 (V:3) | SC-RP |
| C04 | fila | BA/a | A,P; C; vazias | 6 | 140522 (V:4), 140524 (V:5), 140707 (V:7), 119517 (V:10), 122073 (V:65), 128516 (V:69) | SC-P |
| C05 | fila | BA/a | P; C; vazias | 12 | 140928 (V:8), 104784 (V:9), 129384 (V:12), 104040 (V:13), 116436 (V:23), 106963 (V:33), 124364 (V:43), 124370 (V:44), 123033 (V:46), 132144 (V:61), 106350 (V:64), 124774 (V:73) | SC-P |
| C06 | fila | BA/e + DS/a | A,P; C e G, incluindo linha sintética; vazias | 1 | 139432 (V:6) | SC-P |
| C07 | fila | TA/e | A; —; vazias | 9 | 140331 (V:11), 135067 (V:18), 110506 (V:25), 138672 (V:26), 138912 (V:39), 138914 (V:40), 138916 (V:41), 140580 (V:42), 106594 (V:63) | SC-A |
| C08 | fila | BA/e | A,P; C; vazias | 14 | 133364 (V:14), 111346 (V:15), 111845 (V:16), 116432 (V:22), 129745 (V:28), 129749 (V:29), 129751 (V:30), 137166 (V:31), 33784 (V:47), 128519 (V:70), 141797 (V:74), 143313 (V:77), 143393 (V:78), 143395 (V:79) | SC-P |
| C09 | fila | AC/a | A; —; vazias | 2 | 129083 (V:19), 129090 (V:20) | SC-A |
| C10 | fila | AC/e | A; —; vazias | 1 | 129093 (V:21) | SC-A |
| C11 | fila | TA/a | A; —; vazias | 1 | 137154 (V:24) | SC-A |
| C12 | pergunta | DS/e | A,P; G; vazias | 1 | 113307 (V:27) | SC-P |
| C13 | fila | TA/a | A,R; X; responder/colaborador/null | 1 | 129598 (V:32) | SC-R |
| C14 | fila | BA/a | A; —; vazias | 9 | 130931 (V:34), 130933 (V:35), 130935 (V:36), 130939 (V:38), 129567 (V:54), 129569 (V:55), 129573 (V:57), 129577 (V:58), 130781 (V:60) | SC-A |
| C15 | fila | BA/e | A; —; vazias | 4 | 130937 (V:37), 129559 (V:50), 129561 (V:51), 130026 (V:66) | SC-A |
| C16 | fila | TA/e + DS/a | A; —; vazias | 1 | 129217 (V:45) | SC-A |
| C17 | pergunta | DS/a | P,W; G; vazias | 2 | 130346 (V:48), 130348 (V:49) | SC-PW |
| C18 | fila | BA/e + PR/a | A; —; vazias | 1 | 129563 (V:52) | SC-A |
| C19 | fila | BA/a | A,P; C, em linhas sintéticas; vazias | 1 | 129565 (V:53) | SC-P |
| C20 | fila | BA/e + DS/a | A; —; vazias | 2 | 129571 (V:56), 130044 (V:67) | SC-A |
| C21 | fila | BA/e | A,V; C; vazias | 1 | 130771 (V:59) | SC-V |
| C22 | fila | BA/a + TA24/a | A,P; C; vazias | 1 | 132052 (V:62) | SC-P |
| C23 | fila | BA/e | P; C e G; vazias | 1 | 120325 (V:68) | SC-P |
| C24 | pergunta | DS/a | P; G; vazias | 2 | 128533 (V:71), 127477 (V:72) | SC-P |
| C25 | fila | BA/e | A,R; X; responder/colaborador/null | 1 | 141810 (V:75) | SC-R |
| C26 | fila | TA/e + BA/e | A,J; C; vazias | 1 | 142013 (V:76) | SC-J |
| **Total** | | | | **79** | **V:1–79** | |

Os códigos de cobertura significam:

| Cobertura | Caso escrito confrontado | O fato que falta para aplicar o caso | Decisão normativa pelos testes E3 | [nao verificado]
|---|---|---|---|
| SC-A | `test_rota_so_fila_sem_pergunta`, T:32–38 | O teste monta marco **apagado** e exige `perguntar/sistema/emissor`; aqui todas as instâncias expostas estão acesas, sem ações. | **Sem caso escrito** para estes fatos; ATUAL/FUTURO indeterminado. | [nao verificado]
| SC-P | `test_porta_fila_quando_ancora_e_da_celula`, T:67–73; caso A/B, T:21–47 | Falta um caso que imponha porta **apesar de passivo sem linha e ações vazias**. A fixture do teste cria pergunta sem resposta/validação; não constrói passivo. (`F:65–78`) | **Sem caso escrito** para esta combinação; ATUAL/FUTURO indeterminado. | [nao verificado]
| SC-R | `test_fila_A_pergunta_sob_B`, T:21–47 | O caso exige B como âncora viva e ação com porta B; no V5 a âncora está fora de `fila_ids`, com porta da ação `null`. | **Sem caso escrito** para B fora da fila; ATUAL/FUTURO indeterminado. |
| SC-RP | Mesmos casos, T:21–47,67–73 | Faltam simultaneamente o caso de B fora da fila e o de passivo com ações vazias no mesmo dia. | **Sem caso escrito** para a combinação. |
| SC-PW | Caso A/B, T:21–47; âncora da célula, T:67–73 | Falta cenário de dia sem trabalho com passivo, âncora global e nenhuma ação; também não há A residual na fila da célula. | **Sem caso escrito** para a combinação. |
| SC-V | T:67–73 | Falta cenário de validação sem lavra vigente que determine porta mesmo sem ação. | **Sem caso escrito** para a combinação. |
| SC-J | T:67–73 | Falta cenário de encerramento pelo juiz que determine porta mesmo sem ação. | **Sem caso escrito** para a combinação. |

O caso de isenção não se aplica aos fatos expostos: ele exige `razao='isento_ponto'`, ausente nos 79. Os testes de relógio também exigem valores de silêncio e instantes específicos não fornecidos para essas âncoras. (`T:57–65,84–111`; `V:1–79`) [nao verificado]

**3. REGRESSÃO: lista completa demonstrável = `[]`; quantidade = 0.** Nenhuma classe reproduz integralmente um caso E3 que imponha porta. Aplicar T:67–73 apenas pela presença da âncora descartaria o passivo e as decisões que distinguem os fatos. (`T:21–127`; `F:65–78`; `V:1–79`)

O caminho concreto que elimina a porta está identificado, mesmo sem obrigação normativa demonstrada:

| Classes | Onde a cópia deixa de publicar porta |
|---|---|
| C07, C09–C11, C14–C16, C18, C20 | A linha nasce com ações vazias; marco aceso recebe `aceso` e encerra sua passagem pelo laço. (`S:258–271`) |
| C02, C04–C06, C08, C12, C17, C19, C22–C24 | Passivo devolve `acoes=None`; a tarefa mantém a lista vazia e marca `nao_sei`. Exposição administrativa segue a mesma recusa de inventar ação. (`S:130–131,249–255,308–313`) |
| C01, C13, C25 | A âncora só vira porta se estiver na fila; fora dela, a ação `responder` recebe `porta=None`. (`S:128–133`) |
| C03 | Combina os dois caminhos anteriores: resposta sem porta e passivo sem ação. (`S:128–133,308–313`; `V:3`) |
| C21 | Sem lavra vigente, retorna razão e `acoes=None`. (`S:143–146,308–313`) |
| C26 | Encerramento pelo juiz retorna ações vazias. (`S:149–151,313`) |
| Todas | `_portas_da_tarefa` aceita somente portas inteiras pertencentes a `fila_ids`; sem portas, escreve `rota=None`; `porta_da_tarefa` devolve `None`. (`S:547–565`) |

Há contratos E2 que corroboram essas distinções: passivo exige ações vazias; marco aceso dispensa ação; âncora silenciada admite `responder` com `porta=None`. O motivo concreto de exclusão das quatro âncoras V5 da fila permanece **[não verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:396–409,487–495,867–873`; `V:1,3,32,75`)

**4. Restantes: 79 sem caso escrito completo; 0 demonstrados como “caso E3 que manda FUTURO”.** A lista completa dos 79 é a coluna de pks das classes C01–C26, sem exclusões; a lista do segundo conjunto é `[]`. (`V:1–79`; `T:21–127`)

A conferência factual fecha em **30 somente acesos; 41 com passivo e eventual aceso; 2 com passivo e dia sem trabalho; 3 com aceso e resposta; 1 com aceso, passivo e resposta; 1 sem lavra vigente; 1 encerrado pelo juiz = 79**. Há **75 dias com ações vazias e 4 com ações não vazias**. (`V:1–79`)

**5. REGRESSÃO demonstrada com dono ESTRUTURA: 0; lista `[]`.** Isso decorre do conjunto de regressões demonstradas estar vazio, sem necessidade de redistribuir os donos registrados na volta 4. (`V:1–79`; `T:21–127`)

O agregado **ESTRUTURA 44 / CADASTRO 6 / BATIDA 29** não fornece o pareamento individual dos 79 com seus donos. Uma lista individual dos 44 ESTRUTURA, derivada apenas desse agregado, seria **[não verificado]**. (`a1v4_julga.r:16`, contando as linhas textuais do anexo fornecido)

**6. Quem escreve, quem lê e diferenças de significado.**

| Campo | Quem escreve/calcula | Quem lê |
|---|---|---|
| `ancora` exibida no V5 | A tarefa **não escreve esse campo nas instâncias**; escreve `pergunta_id` e `acoes`. Calcula localmente `anc=chamado_que_lista(p)`. A implementação do enriquecimento da sonda é **[não verificado]**. (`S:128,258–260,318–320`) | A tarefa resolve a âncora pela pergunta, sem ler `instancia['ancora']`. (`S:127–129`) |
| Identidade da âncora | `chamado_que_lista` retorna `chamado_destino_id`, com fallback para `disputa.chamado_id`. (`app/chamados/services/fio.py:97–117`) | A carga consulta a fila dessas âncoras; `_pela_decisao` usa sua pertença para preencher a porta. (`S:415–418,128–133`) |
| `acoes[].acao/de/porta` | `_acao` monta os campos; `_pela_decisao` escolhe ações; `_tarefa_pura` as publica nas instâncias. (`S:114–162,249–255,302–313`) | `_portas_da_tarefa` lê porta e dono; o backfill lê razões e ações, inclusive humanos sem porta. (`S:547–556`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:97–117`) |
| `fila_ids` / `fila_cel` | `alimentar_fila` constrói a fila global com verbo e o agrupamento por célula. (`J:1558–1567`) | C17 admite chamado da célula e âncora global; tarefa usa a fila global para autorizar porta e a fila da célula para escolher rota/subconjunto `fila`. (`J:1677–1687`; `S:548–555`) |
| Verbo do ATUAL | `particionar_verbos` calcula o mapa por módulo, créditos das perguntas e silêncio; `alimentar_fila` executa a partição sobre `qs_vivos()`. (`app/chamados/services/verbos.py:30–52`; `J:1558`) | C17 copia o mapa; a tarefa copia o mesmo mapa para as portas selecionadas pelas ações. (`J:1678,1682`; `S:549`) |
| Porta publicada | `_portas_da_tarefa` escreve rota/fila/verbos/fio; `porta_da_tarefa` projeta esses campos ou `None`. (`S:547–565`) | Folha, pauta, vigia, beco e furo sem canal consomem essa projeção. (`app/folha/fora_da_folha.py:71–89`; `app/ponto/services/pautas_dos_discordantes.py:79–84`; `app/chamados/services/vigia_fio.py:640–654`; `app/escala/services/cadastro_realidade.py:492–503`; `app/chamados/services/furo_sem_canal.py:93–101`) |

As diferenças semânticas verificáveis são:

- **Chamado do dia versus chamado da pergunta:** a folha publica identificadores somente de `porta['fila']`, isto é, chamados da célula; as demais chaves de `verbos` podem incluir âncoras externas. Isso está explicitado no leitor. (`app/folha/fora_da_folha.py:235–242`; `S:549–551`)
- **Fila global versus fila da célula:** uma âncora G pode autorizar `porta_pergunta`; não precisa pertencer a `fila_cel`. Esses conjuntos têm funções distintas no código. (`S:548–555`; `J:1677–1687`)
- **Verbo versus dono:** C17 calcula fio pelos verbos; a tarefa calcula fio pelos donos das ações. O teste admite expressamente `responder` junto de verbo `validar`; o vigia preserva essa distinção. (`J:1633–1639,1688`; `S:552–556`; `T:75–82`; `app/chamados/services/vigia_fio.py:649–653`)
- **Ausência de porta versus ninguém esperando:** a documentação da pauta diz “não estão esperando NINGUÉM”, mas seu predicado efetivo lê ausência de porta. A tarefa pode ter ação humana sem porta ou `nao_sei`; a folha já descreve essa limitação. (`app/ponto/services/pautas_dos_discordantes.py:47–56,79–84`; `S:129–133,544,562–565`; `app/folha/fora_da_folha.py:52–58`)
- **Rótulo do instrumento versus caso escrito:** `casos_do_par` chama de `so_fila_sem_pergunta` qualquer `fila and not vivas`; não exige marco apagado nem ação `perguntar`. Esse rótulo, sozinho, não comprova cobertura por T:32–38. (`app/chamados/management/commands/diff_e3_sombra.py:63–70`; `T:32–38`)

## 13/11 17:45 · sessao a0cfb94c0d12 · - · - · origem=Ronald (tmux)

Contagem conferida: **79 colab-dias, 75 com ações vazias e 4 com ações não vazias**. Os quatro são `141714`, `118931`, `129598`, `141810`. (`v5_det.txt:1,3,32,75`, contando somente linhas `V5_DET`; demais linhas V5:1–79)

A tabela preserva módulo/status e separa misturas e linhas sintéticas. Ignora somente multiplicidade e ordem das razões. Abreviações: `A=aceso`, `P=passivo_sem_linha`, `R=aguarda_resposta`, `W=dia_sem_trabalho`, `V=veredito_sem_lavra_vigente`, `J=encerrada_pelo_juiz`; `e=em_analise`, `a=aberto`. `mesma` significa âncora em ambas as filas; `global` significa `fila_ids=True/fila_cel=False`; `fora` significa ambos falsos. Módulos abreviados por prefixo inequívoco. (`v5_det.txt:1–79`)

| # | Rota | Módulo/status | Razões/âncora/ações | n | Lista completa com linha V5 |
|---|---|---|---|---:|---|
| 1 | fila | turno_aberto/e | A,R; R fora; responder/null | 1 | 141714:1 |
| 2 | fila | turno_aberto/e | A,P; mesma; vazias | 2 | 130162:2, 135778:17 |
| 3 | fila | batida_ausente/e | A,P,R; R fora; P fora e mesma; responder/null | 1 | 118931:3 |
| 4 | fila | batida_ausente/a | P ou A,P; mesma; vazias | 18 | 140522:4, 140524:5, 140707:7, 140928:8, 104784:9, 119517:10, 129384:12, 104040:13, 116436:23, 106963:33, 124364:43, 124370:44, 123033:46, 132144:61, 106350:64, 122073:65, 128516:69, 124774:73 |
| 5 | fila | batida_ausente/e + disputa_supervisao/a | A,P; mesma+global, sintética; vazias | 1 | 139432:6 |
| 6 | fila | turno_aberto/e | A; sem âncora; vazias | 9 | 140331:11, 135067:18, 110506:25, 138672:26, 138912:39, 138914:40, 138916:41, 140580:42, 106594:63 |
| 7 | fila | batida_ausente/e | P ou A,P; mesma; vazias | 14 | 133364:14, 111346:15, 111845:16, 116432:22, 129745:28, 129749:29, 129751:30, 137166:31, 33784:47, 128519:70, 141797:74, 143313:77, 143393:78, 143395:79 |
| 8 | fila | ausencia_colab/a | A; sem âncora; vazias | 2 | 129083:19, 129090:20 |
| 9 | fila | ausencia_colab/e | A; sem âncora; vazias | 1 | 129093:21 |
| 10 | fila | turno_aberto/a | A; sem âncora; vazias | 1 | 137154:24 |
| 11 | pergunta | disputa_supervisao/e | A,P; global; vazias | 1 | 113307:27 |
| 12 | fila | turno_aberto/a | A,R; fora; responder/null | 1 | 129598:32 |
| 13 | fila | batida_ausente/a | A; sem âncora; vazias | 9 | 130931:34, 130933:35, 130935:36, 130939:38, 129567:54, 129569:55, 129573:57, 129577:58, 130781:60 |
| 14 | fila | batida_ausente/e | A; sem âncora; vazias | 4 | 130937:37, 129559:50, 129561:51, 130026:66 |
| 15 | fila | turno_aberto/e + disputa_supervisao/a | A; sem âncora; vazias | 1 | 129217:45 |
| 16 | pergunta | disputa_supervisao/a | P,W; global; vazias | 2 | 130346:48, 130348:49 |
| 17 | fila | batida_ausente/e + par_relampago/a | A; sem âncora; vazias | 1 | 129563:52 |
| 18 | fila | batida_ausente/a | A,P; mesma, sintéticas; vazias | 1 | 129565:53 |
| 19 | fila | batida_ausente/e + disputa_supervisao/a | A; sem âncora; vazias | 2 | 129571:56, 130044:67 |
| 20 | fila | batida_ausente/e | A,V; mesma; vazias | 1 | 130771:59 |
| 21 | fila | batida_ausente/a + turno_aberto_24h/a | A,P; mesma; vazias | 1 | 132052:62 |
| 22 | fila | batida_ausente/e | P; mesma+global; vazias | 1 | 120325:68 |
| 23 | pergunta | disputa_supervisao/a | P; global; vazias | 2 | 128533:71, 127477:72 |
| 24 | fila | batida_ausente/e | A,R; fora; responder/null | 1 | 141810:75 |
| 25 | fila | turno_aberto/e + batida_ausente/e | A,J; mesma; vazias | 1 | 142013:76 |
| **Total** | | | | **79** | V5:1–79 |

**Atenção de precisão:** classes 4 e 7 agrupam conjuntos `P` e `A,P`; se “padrão” exigir igualdade exata do conjunto de razões, devem ser subdivididas. A tabela preserva os fatos no texto de cada classe e permite duas amostras nos grupos de n≥2; grupos unitários não têm segundo exemplo. (`v5_det.txt:4–10,12–16,22–23,28–31,33,43–47,61,64–65,69–70,73–74,77–79`)

Consolidação ampla útil: **30 somente A; 41 com P e eventual A; 2 P,W; 3 A,R; 1 A,P,R; 1 A,V; 1 A,J =79**. (`v5_det.txt:1–79`)

## 13/11 17:43 · sessao 0fcc9646b8a7 · - · - · origem=Ronald (tmux)

| Campo | Escritor | Leitor e significado |
|---|---|---|
| Âncora da pergunta | `app/chamados/services/fio.py:97–117`: `chamado_destino_id`, com fallback para `disputa.chamado_id`. A tarefa a calcula em variável local, `app/chamados/services/tarefa_do_dia.py:127–129`. | `_pela_decisao` atribui essa âncora à porta da ação somente quando pertence à fila recebida, `tarefa_do_dia.py:129`. A carga forma essa fila consultando `qs_na_fila` sobre as âncoras das perguntas, `tarefa_do_dia.py:415–418`. |
| Campo `ancora` do V5 | Não consta da construção das instâncias em `tarefa_do_dia.py:258–260,318–320`. O V5 o descreve como enriquecimento da sonda; implementação dessa escrita **[não verificado]**. | A tarefa não lê um campo `instancia['ancora']`; calcula a âncora pelo objeto pergunta, `tarefa_do_dia.py:127–129`. |
| `acoes[].acao/de/porta` | `_acao` monta os três campos, `tarefa_do_dia.py:114–115`; `_pela_decisao` escolhe ações conforme decisão/passivo, `:128–162`; `_tarefa_pura` publica nas instâncias, `:249–255,302–313`. | `_portas_da_tarefa` achata as ações, mantém portas inteiras pertencentes à fila global, lê o verbo do painel e calcula fio pelos donos, `:547–556`. O backfill lê razões/ações/portas, inclusive humanos com `porta=None`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:97–117`. |
| `fila_ids` e `fila_cel` | `app/chamados/juizes.py:1558–1567`: `fila_ids` contém a fila global com verbo; `fila_cel` agrupa chamados por `celula_id` nas células pedidas. | C17 lê chamados da célula e soma âncoras da pergunta pertencentes à fila global, `juizes.py:1677–1682`. A tarefa usa `fila_ids` para autorizar porta e `fila_cel` somente para determinar `porta_fila` e o subconjunto `fila`, `tarefa_do_dia.py:548–555`. |
| Verbo do ATUAL | `app/chamados/services/verbos.py:30–52`: partição usa módulo, créditos de perguntas e silêncio. `juizes.py:1558` executa sobre `qs_vivos()`. | C17 copia esses verbos em `juizes.py:1678,1682`; traduz verbo em fio por `:1633–1639,1688`. A tarefa copia o mesmo mapa para portas escolhidas pelas ações, mas calcula fio pelos donos das ações, `tarefa_do_dia.py:549,552–556`. |

Leitores de produção da porta publicada: [nao verificado]

- Folha lê `porta_da_tarefa`, fio e `fila`, sem reconstruir âncora: `app/folha/fora_da_folha.py:71–89,233–242`.
- Pauta lê o fio projetado pela folha: `app/ponto/services/pautas_dos_discordantes.py:79–84`.
- Vigia lê rota/fio; explicita que verbo do painel não decide quem age: `app/chamados/services/vigia_fio.py:640–654`.
- Beco exige ausência de porta da tarefa e mantém a âncora da pergunta separadamente: `app/escala/services/cadastro_realidade.py:492–503`.
- Furo sem canal lê presença de porta/rota e distingue `nao_sei`: `app/chamados/services/furo_sem_canal.py:93–101`.
- Instrumento de sombra compara C17 com a projeção `porta_da_tarefa`: `app/chamados/management/commands/diff_e3_sombra.py:127–130`.

Há descrição mais ampla que o predicado na pauta: “não estão esperando NINGUÉM” e “pergunta viva [...] chamado vivo JA tem dono esperando”, embora o código efetivo teste ausência de porta publicada; esta pode existir com ações humanas sem porta. (`app/ponto/services/pautas_dos_discordantes.py:47–56,79–84`; `app/chamados/services/tarefa_do_dia.py:129,133,548–565`)

A folha limita o identificador nomeado ao chamado da célula publicado em `fila`, preservando a distinção entre chamado do dia e âncora da pergunta; não nomeia todas as chaves de `verbos`. (`app/folha/fora_da_folha.py:235–242`; `app/chamados/juizes.py:1654–1657`)

Cuidado normativo: `test_porta_fila_quando_ancora_e_da_celula` constrói pergunta sem resposta/passivo e exige verbo `cobrar`; não escreve o cenário `passivo_sem_linha + acoes=[] + validar/decidir`. O caso A/B exige B vivo na fila e ação com porta B. Os quatro dias humanos do V5 têm âncora fora de ambas as filas, portanto não reproduzem esse caso. (`app/chamados/tests/test_e3_portas_tarefa.py:21–47,67–73`; `v5_det.txt:1,3,32,74`, contando somente `V5_DET`; conferir numeração com a recontagem do agente principal)

## 13/11 17:43 · sessao ae8ca03f0083 · - · - · origem=Ronald (tmux)

Os fatos V5 não demonstram regressão coberta integralmente por `test_e3_portas_tarefa.py`:

| Padrão V5 | Aplicabilidade estrita do caso E3 |
|---|---|
| `passivo_sem_linha`, âncora na fila/célula, ações vazias | **Sem caso escrito E3 completo.** O teste de âncora da célula cria pergunta sem resposta/validação; não cria passivo. (`app/chamados/tests/test_e3_portas_tarefa.py:67–73`; `app/chamados/tests/e3_fixtures.py:65–78`) |
| Tudo `aceso`, perguntas/âncoras ausentes, ações vazias | **Sem caso escrito E3 completo.** “Só fila sem pergunta” monta marco apagado e exige ação `perguntar/sistema/emissor`; os dias inteiramente acesos não reproduzem essa entrada. (`app/chamados/tests/test_e3_portas_tarefa.py:32–38`) |
| `aguarda_resposta`, âncora existente mas fora `fila_ids`, `responder/colaborador/None` | **Sem caso escrito E3 completo.** O caso A/B exige B como âncora **viva** e publica porta B; B fora da fila não reproduz esse fato. (`app/chamados/tests/test_e3_portas_tarefa.py:21–28,40–47`) |
| `veredito_sem_lavra_vigente` ou `encerrada_pelo_juiz`, ações vazias | **Sem caso escrito E3 completo:** os testes de porta não constroem essas decisões/lavras. (`app/chamados/tests/test_e3_portas_tarefa.py:21–111`) |

Há contratos E2 que explicitamente impedem tratar esses resultados como regressões apenas pela existência de âncora:

- Passivo: `passivo_sem_linha`, ações vazias e `nao_sei=True`. (`app/chamados/tests/test_e2_tarefa_do_dia.py:487–495`)
- Marco aceso: nenhuma ação, inclusive havendo pergunta respondida. (`app/chamados/tests/test_e2_tarefa_do_dia.py:867–873`)
- Âncora silenciada: permanece `responder/colaborador`, porém `porta=None`. (`app/chamados/tests/test_e2_tarefa_do_dia.py:396–409`)

O caminho de ausência de porta é explícito: passivo devolve `None` antes de publicar ação; marco aceso encerra a instância; âncora fora da fila vira `porta=None`; a publicação aceita apenas portas inteiras presentes em `fila_ids`. (`app/chamados/services/tarefa_do_dia.py:128–133,262–271,308–313,547–565`)

Portanto, pela leitura estrita dos casos E3 solicitados, **zero regressões demonstradas**, sem concluir que toda perda esteja normativamente correta. A existência de um caso E3 que determine porta para essas combinações permanece **[não verificado]**.

## 13/11 17:42 · sessao 2c7360c1be05 · O168 · verificador · origem=Code

**REPROVA.** Há dois bloqueantes: a constância de custo continua sem prova para várias respostas pendentes, e CSF/retratação ainda respondem diferente ao mesmo caso trancado. (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47`; `app/chamados/models.py:1902`; `app/chamados/juizes.py:2035`)

1. **Os seis itens do opus v6**

| Item | Resultado da cura |
|---|---|
| 1 — mapa desatualizado | **Curado na cópia.** Os dois endereços estão no mapa; executei o selo, que passou, e comparei o resultado do gerador em memória: igualdade exata. Estarem no mesmo commit é [não verificado]. (`app/docs/MAPA-JUIZES.md:106`, `:114`; `bin/tests/test_mapa_juizes.sh:66`; `bin/gerar_mapa_juizes.py:66`) |
| 2 — frota não medida | **Há medição fornecida**, com universo não vazio e categorias compatíveis com B. A execução pelo juiz real permanece [não verificado]: o script da sonda não acompanha os anexos nem foi localizado na cópia. (anexos `frota_b_head.out:4`; `frota_b_v8.out:4`) |
| 3 — contador lendo só trilha | **Curado para o contraexemplo da resposta recusada.** Agora exige também `not pode_executar_csf(_ch)`; existe teste que mantém a trilha antiga, recusa a resposta e exige contador zero. Execução Django desse teste [não verificado]. (`app/chamados/juizes.py:2530`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:187`, `:195`) |
| 4 — custo constante | **Não curado integralmente.** A consulta por disputa saiu, mas permanecem consultas por pergunta pendente; o teste mantém apenas uma pendente quando aumenta N. Contraexemplo no item 9. (`app/ponto/supra_juiz.py:558`; `app/chamados/models.py:1902`; `app/chamados/tests/test_o168_custo_da_guarda_constante.py:47`) |
| 5 — “MESMO predicado” falso | **Curado documentalmente.** MD e JSON marcam expressamente a afirmação como superada. (`app/docs/CORTES.md:448`; `app/docs/CORTES.json:1405`) |
| 6 — nota contraditória/redundância | **Redundância removida; sequência explicada.** A nota só nasce após `_pend` não vazio. O comentário distingue recusa da cascata e fechamento posterior pelo desligamento; esse fechamento continua fora da guarda. (`app/chamados/models.py:720`, `:735`; `app/colaboradores/services/desligamento.py:151`, `:163`) |

2. **O diff realiza B e somente B?**

No gate de `reconciliar`, implementa a seleção de B: resposta validada ou materializada deixa de segurar; resposta sem ambos segura, ressalvada a tranca. Defesa 1 chega a esse gate. O fechamento ainda depende também das guardas existentes de premissa e pergunta viva. (`app/chamados/models.py:683`, `:686`, `:689`, `:1898`; `app/chamados/signals.py:43`)

**BLOQUEANTE — a “mesma pergunta” da retratação continua divergente.** Contraexemplo: pergunta creditada ao chamado, respondida, não validada, não materializada, com dia trancado. O CSF libera porque respeita a tranca; a retratação segura porque passa `ignorar_tranca=True`. O executor consulta o primeiro e depois chama a segunda. (`app/chamados/models.py:1902`; `app/chamados/juizes.py:2033`, `:2035`; `app/chamados/reconciliador.py:448`, `:519`; `app/ponto/management/commands/supra_juiz.py:261`, `:286`)

Executei o predicado extraído em memória: esse caso devolveu **zero pendentes** normalmente e **uma pendente** com `ignorar_tranca=True`. Portanto houve unificação do corpo, mas não da resposta efetiva exigida pelo aval. (`app/chamados/models.py:1898`; `app/chamados/juizes.py:2035`; anexo `aval.txt:2`)

Há fechamentos fora desse juiz: desligamento, aceite de vínculo, regularização externa e competência trancada usam outras portas. `encerrar` consulta pergunta viva, sem consultar a guarda da conversa. Existe também desligamento por cron. (`app/colaboradores/services/desligamento.py:163`; `app/colaboradores/services/vinculo.py:388`; `app/chamados/services/regularizacao_ext.py:206`; `app/chamados/services/competencia_trancada.py:209`; `app/chamados/models.py:1664`; `app/config/crons.py:440`)

O selo declara essas exceções por nome e cita outro aval; a autorização literal dessas exceções é **[não verificado]**, pois esse aval não foi fornecido. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:3`, `:27`)

3. **Frota B e dia trancado**

A soma fornecida fecha: **862 com conversa = 513 segurados + 349 liberados**; os liberados se dividem em **345 toda julgada + 4 pendentes**. Isso é compatível com o predicado atual, que exclui validadas, materializadas e perguntas trancadas. Não comprova execução pelo juiz real nem fechamento efetivo. (anexo `frota_b_v8.out:4`; `app/chamados/models.py:1898`; `app/ponto/management/commands/supra_juiz.py:286`)

A classificação dos quatro como “dia trancado” não aparece nos campos do JSON fornecido: **[não verificado]**. Se forem os casos descritos, podem ser liberados pelo juiz e novamente retidos pela retratação, conforme o contraexemplo anterior. (`app/chamados/juizes.py:2035`; `app/chamados/reconciliador.py:519`)

A exceção não está enumerada no aval B literal. Ela é **herdada do juiz existente**, documentada como regra anterior da tranca; não nasceu nesta cura. (anexo `aval.txt:2`; `app/chamados/models.py:1896`; `app/chamados/juizes.py:978`)

4. **Quem escreve e quem lê**

| Campo/valor | Escritores | Leitores e significado |
|---|---|---|
| `resposta_colab`, `respondida_em` | Resposta normal grava o par; resposta automática por outro canal também; recuperação pela trilha escreve só o carimbo; reabertura limpa ambos. (`app/chamados/services/disputa_emissao.py:1467`, `:1593`, `:1652`, `:1674`) | O juiz aceita carimbo **OU texto**. Já `estado`/`respondida` dependem do carimbo; materialização exige texto. (`app/chamados/models.py:1900`, `:3101`, `:3552`; `app/chamados/services/materializacao.py:844`) |
| `validada_em` | Validação escreve; devolução e nova resposta limpam; `carimbar(resposta='validar')` também pode preencher automaticamente. (`app/chamados/services/validacao.py:157`, `:222`; `app/chamados/services/disputa_emissao.py:1480`; `app/chamados/models.py:3190`) | Juiz considera qualquer preenchimento suficiente para não segurar; materializador usa o campo para selecionar trabalho. Logo o campo isolado não prova julgamento humano. (`app/chamados/models.py:1899`; `app/chamados/services/materializacao.py:1375`) |
| `materializada_em` | Atribuidor central `_escrever_carimbo`, usado pelas portas de carimbo e desfazimento. Também recebe encerramento por identidade/admin sem criar batida. (`app/chamados/models.py:3153`, `:3120`, `:3241`, `:3259`) | Juiz deixa de segurar quando preenchido. Portanto “materializada” não equivale necessariamente a batida criada. (`app/chamados/models.py:1899`, `:3106`) |
| `fechamento_recusado` | `_registrar_recusa` grava a trilha; o gate fornece motivo `conversa_exige_decisao_humana`. (`app/chamados/models.py:397`, `:408`, `:723`) | Contador usa a trilha como universo histórico e consulta o juiz atual. A nota visível é outra escrita, pela porta de notas. (`app/chamados/juizes.py:2525`, `:2531`; `app/chamados/models.py:735`; `app/chamados/services/nota.py:75`) |
| `pais_segurados_pela_conversa` | Não persiste contador: calcula fila ∩ histórico de recusa ∩ juiz atual segurando. (`app/chamados/juizes.py:2525`, `:2530`) | Leitor encontrado é o teste; não representa toda a frota segurada, pois exige recusa histórica e presença na fila. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:75`; `app/chamados/juizes.py:2515`) |
| `respostas_sem_veredito` / `sem_veredito_entre` | Não gravam estado; o método registrado delega ao corpo compartilhado. (`app/chamados/models.py:1886`, `:1889`) | Fechamento/prontidão, desligamento, ficha, reconciliador, fila por causa, tranca, retratação e CSF. Alguns passam `ignorar_tranca=True`. (`app/chamados/models.py:1909`, `:1942`; `app/colaboradores/services/desligamento.py:141`; `app/colaboradores/services/ficha.py:169`; `app/chamados/reconciliador.py:1164`; `app/chamados/services/fila_por_causa.py:226`; `app/chamados/services/competencia_trancada.py:126`; `app/chamados/juizes.py:2035`; `app/ponto/supra_juiz.py:567`) |
| `perguntas_que_seguram_csf` | Calcula lista ordenada, unindo origem e destino; não grava. (`app/ponto/supra_juiz.py:567`) | Bool registrado e gate/nota leem a mesma lista. (`app/ponto/supra_juiz.py:582`; `app/chamados/models.py:719`, `:738`) |

**Há incompatibilidade concreta de significado:** texto `'07:00'` sem `respondida_em`, sem validação/materialização, segura pelo juiz, mas validar retorna `sem_resposta` e recusar retorna `ja_pendente`. A nota nova manda justamente validar/devolver. Existe reparo pela trilha; sua disponibilidade nesse fluxo de tela é [não verificado]. (`app/chamados/models.py:1900`, `:736`; `app/chamados/services/validacao.py:416`, `:603`; `app/chamados/services/disputa_emissao.py:1661`)

5. **LEI-AKITA e `padrao=`**

O histórico `main..ac8fbe5cc` não está acessível: a pasta não é um repositório Git. Linhas de cada commit, `juizes novos=0`, `padrao=` e presença das curas no mesmo commit: **[não verificado]**.

Pelo critério funcional proposto, **há funções novas compartilhadas**: `sem_veredito_entre` e `perguntas_que_seguram_csf`. Entretanto são extração do corpo existente e composição dos universos; não encontrei segunda cópia da condição material de resposta sem veredito. O padrão realizado é centralização/delegação, com a divergência parametrizada da tranca já apontada. (`app/chamados/models.py:1886`, `:1898`; `app/ponto/supra_juiz.py:561`; `app/chamados/juizes.py:2035`)

6. **Corte na cópia**

**Sim.** Lendo o arquivo da cópia, a frase pedida de nascimento de `DisputaSupervisao.respostas_sem_veredito` está na linha O168-JUIZ-DO-VEREDITO. O registro vivo aponta para o mesmo endereço: `chamados/models.py::DisputaSupervisao.respostas_sem_veredito`. (`app/docs/CORTES.md:449`; `app/core/juizes.py:294`)

7. **Dinheiro**

Não há prova de alteração de entra/fora da folha, rubrica ou valor dos 349; competências e efeitos reais: **[não verificado]**. A classificação de exportação consulta outros insumos, que não foram comparados nos anexos. (`app/folha/export.py:765`, `:830`)

Também **não é demonstrável impacto financeiro zero**: retratar pode resolver perguntas e fechar disputa; fechar disputa agenda retomada de regularizações confirmadas; essa retomada pode criar, corrigir ou remover batidas. (`app/chamados/reconciliador.py:537`; `app/chamados/services/disputa_emissao.py:1987`; `app/chamados/signals.py:139`, `:146`; `app/chamados/services/regularizacao_ext.py:78`, `:137`, `:225`)

Os anexos não informam quantos dos 349 percorrem essa cadeia, nem se atingem competência 10 ou 11. **[não verificado]**

8. **Classes de reprovação**

Há **parâmetro que produz duas respostas à mesma pergunta**, **campo com significados distintos entre leitores** e **selo novo que deixa escapar mutante óbvio**. Os dois primeiros estão nos contraexemplos da tranca e do texto sem carimbo. (`app/chamados/juizes.py:2035`; `app/chamados/models.py:1902`; `app/chamados/services/validacao.py:416`)

No selo da regularização, executei em memória o método AST com original e mutante `if _disp` → `if not _disp`: **ambos passaram**. Ele confere ordem e presença de `return`, sem conferir a polaridade. Há, porém, teste comportamental existente incompatível com esse mutante; não afirmo fuga da suíte inteira. (`app/chamados/tests/test_o168_fechadores_fora_do_gate.py:217`; `app/chamados/services/regularizacao_ext.py:62`; `app/chamados/tests/test_guard_disputa_reg_externa.py:40`)

Há ainda relógio transitivo fora da borda no contador novo: não recebe `agora`, chama a fila sem instante, e esta usa `timezone.now()`. Contraexemplo de composição: fila avaliada em `t` e contador depois do vencimento de `silenciado_ate` dão universos diferentes. Divergência ocorrida em produção é [não verificado]. (`app/chamados/juizes.py:2511`, `:2530`; `app/chamados/services/painel.py:190`; `app/docs/PLANO-PADROES.md:92`)

Não identifiquei nova escrita direta de estado fora das portas nem nova aquisição de trava no trecho acrescentado; isso não certifica concorrência executada. (`app/chamados/models.py:723`, `:735`; `app/ponto/supra_juiz.py:552`; `app/chamados/juizes.py:2530`)

9. **Custo — bloqueante remanescente**

**O teste não prova constância para três disputas pendentes.** Ele deixa pendente apenas `i == 0`; as demais ficam validadas e pulam a leitura da tranca. (`app/chamados/tests/test_o168_custo_da_guarda_constante.py:47`; `app/chamados/models.py:1899`)

Contraexemplo: mantenha o fixture e deixe as **N respostas pendentes**. Cada uma chama a tranca, que acessa `p.disputa.colaborador`; os universos são carregados sem `select_related`. Mesmo sem data, essas FKs são avaliadas antes do retorno por dia ausente. Executei o predicado em memória: uma pendente chamou a tranca uma vez; três chamaram três vezes. O número exato de SQL em banco é [não verificado], mas o caminho por item permanece. (`app/ponto/supra_juiz.py:558`, `:568`; `app/chamados/models.py:1902`; `app/chamados/juizes.py:991`, `:1024`)

Os tetos passaram para **219/279**, com justificativa textual e referência a medições; os artefatos dessas medições não foram fornecidos, portanto seus resultados são [não verificado]. O próprio comentário limita o cenário medido a zero respostas que consultam tranca: ele não cobre o contraexemplo pendente. (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`)

## 13/11 17:42 · sessao 3fd4fd38ee19 · BO-[nome]-1311-ROTEIRO · sonda · origem=Code

`BYTES_DA_SAIDA 214375` e o aval anexo chegaram. A correspondência 01→col1019, 02→col996, 03→col1032, 04→col246 e 05→col265 é consistente entre UNIVERSO, FICHA e EXTRA recebidos. [nao verificado]

**Não houve nova execução nesta sessão, nem reproduzo abaixo a saída integral.** O arquivo executável da sonda não foi localizado na cópia; recebi sua transcrição raspada. [nao verificado] O wrapper executa a sonda antes de chamar o Codex e entrega seu resultado pelo prompt (`bin/codex_pergunta.sh:15`; `bin/codex_pergunta.sh:324`; `bin/codex_pergunta.sh:357`).

O extrato diagnóstico recebido foi este; ele **não substitui os blocos FICHA/EXTRA integrais**. [nao verificado]

```text
2026-11-13 17:32:55,831 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=46757 resposta=16:01 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 17:32:56,007 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=55375 resposta=14:30 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 17:32:56,026 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=55411 resposta=15:50 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
ERROS {}
BYTES_DA_SAIDA 214375 (teto do wrapper 300000 com os anexos)
```

O mapa final veio vazio, mas existem três diagnósticos `ERROR`; não certifico aplicação correta das perguntas 46757, 55375 e 55411. [nao verificado] O código registra a divergência e continua escolhendo `t1 or t2`; portanto, receber uma previsão depois do diagnóstico não comprova concordância entre as fontes (`app/chamados/services/materializacao.py:302`; `app/chamados/services/materializacao.py:310`; `app/core/observ.py:73`).

Os dados individuais abaixo estão somente na transcrição recebida, sem arquivo local numerado; por isso levam `[nao verificado]`. As referências de código identificam o comportamento conferido na cópia.

**Critério comum:** nenhuma pergunta será classificada **a — VALIDA-HOJE**. `estado_aplicacao_pergunta` prevê derivação, reuso e paridade, mas não projeta a nova lavra nem a retenção posterior (`app/chamados/services/materializacao.py:449`; `app/chamados/services/materializacao.py:463`). O juiz real da retenção é `motivos_retencao_celula`, pelo laço `_dias_sem_decisao`: lê veredito acusador, protestos e exceção de pausa (`app/folha/export.py:1212`; `app/folha/export.py:1273`; `app/folha/export.py:1293`). A exceção exige protestos exatamente `{'FURO_PARCIAL'}` e confirmação de que só faltam marcos de pausa (`app/folha/export.py:1173`; `app/folha/export.py:1148`).

“Não medido” também será usado quando faltar declaração ou diagnóstico de cadastro: validar pergunta sem resposta é recusado, e isso não comprova falta de vínculo ou de marcos (`app/chamados/services/validacao.py:393`). Assim, a classe **d — CADASTRO** não será presumida.

## Caso 01 -- col1019

**Comp 11: nenhum dia retido; nenhuma pergunta viva.** [nao verificado]

| Pergunta | Dia | Declarado | `estado_aplicacao` | Disputa | Classe |
|---|---|---|---|---|---|
| 54117 | 03/11 | hora / nulo | nulo | 7877 fechada | não medido; histórica. [nao verificado] |
| 54118 | 03/11 | hora / nulo | nulo | 7877 fechada | não medido; histórica. [nao verificado] |
| 54119 | 05/11 | hora / nulo | nulo | 7877 fechada | não medido; histórica. [nao verificado] |

As três estão com `viva=false`, via `celula_absolveu`, materializadas em 13/11 às 07:16:02; o chamado 37070 e a disputa 7877 estão encerrados. [nao verificado]

O recorte não contém batidas. Há abonos aprovados nos dias 03, 05, 07, 09, 10 e 12/11; as células correspondentes aparecem como `ausencia`, com veredito `concorde`. [nao verificado]

**Roteiro:** não há dia retido a retirar nesta fotografia. Os vínculos e as regenerações constam do dado, mas a queixa de tela permanece sem print e sem diagnóstico adicional. [nao verificado]

## Caso 02 -- col996

**Comp 11: quatro dias retidos — 21/10, 30/10, 03/11 e 12/11 — todos por `espelho_furo`. Há 16 perguntas vivas, todas na disputa 7263 aberta. A pergunta 46751 é de 20/10, fora da comp 11, e continua incluída por estar viva.** [nao verificado]

Nesta tabela, **D** significa **b — ESPERA OBRA DECLARADO-X-GRAVADO**, existente em `app/docs/BACKLOG.md:1936`.

| Pergunta | Dia / motivo | Tipo / declarado | `estado_aplicacao` recebido | Classe exclusiva |
|---|---|---|---|---|
| 46751 | 20/10 · saída sem entrada | hora / 07:50 | `bloqueada_paridade`: vizinha E 07:00 | **c — DECISÃO DO ADMIN**. [nao verificado] |
| 46756 | 21/10 · intervalo saída | hora / 15:00 | `aplicavel`: E 15:00 | **b — D**. [nao verificado] |
| 46757 | 21/10 · intervalo volta | hora / 16:01 | `aplicavel`: E 14:30; diagnóstico de divergência | **b — D**. [nao verificado] |
| 46758 | 21/10 · `orfao_14h` | hora / 21:05 | `aplicavel`: S 21:05 | **não medido**: saída da retenção não projetada. [nao verificado] |
| 48773 | 22/10 · intervalo saída | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |
| 57211 | 22/10 · intervalo volta | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |
| 52035 | 30/10 · `orfao_14h` | hora / 15:50 | `aplicavel`: S 15:50 | **não medido**: saída da retenção não projetada. [nao verificado] |
| 53565 | 03/11 · intervalo volta | hora / nulo | nulo | **não medido**: falta declaração; existe E 14:29:51 no dado. [nao verificado] |
| 53644 | 03/11 · `orfao_14h` | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |
| 53920 | 04/11 · intervalo saída | hora / 13:01 | `ja_satisfeita`: S 12:54 existente | **c — DECISÃO DO ADMIN**. [nao verificado] |
| 55313 | 08/11 · saída sem entrada | hora / 07:00 | `ja_satisfeita`: E 06:58 existente | **c — DECISÃO DO ADMIN**. [nao verificado] |
| 55375 | 08/11 · intervalo saída | hora / 14:30 | `aplicavel`: S 13:00; diagnóstico de divergência | **b — D**. [nao verificado] |
| 55411 | 08/11 · intervalo volta | hora / 15:50 | `aplicavel`: E 14:30; diagnóstico de divergência | **b — D**. [nao verificado] |
| 56923 | 11/11 · intervalo volta | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |
| 57210 | 11/11 · intervalo saída | hora / 13:00 | `aplicavel`: S 13:00 | **não medido**: o dia já não retém na fotografia. [nao verificado] |
| 57214 | 12/11 · `orfao_14h` | hora / nulo | nulo | **não medido**: falta declaração do colaborador. [nao verificado] |

**DECLARADO-X-GRAVADO.** O código substitui a hora respondida pela hora do marco nos intervalos (`app/chamados/services/materializacao.py:409`). O tipo prioriza o marco do chamado-destino sobre o da ata, inclusive quando divergem (`app/chamados/services/materializacao.py:274`; `app/chamados/services/materializacao.py:302`). A saída de pausa tem papel S e a volta, E (`app/escala/models.py:1652`). Na 46756, o dado contém declaração de hora, sem declaração separada de E/S; o desencontro demonstrado é entre o motivo de saída de pausa e a previsão E. [nao verificado]

Há também passivo já gravado:

| Pergunta | Declarado | Batida vinculada recebida |
|---|---|---|
| 52619 | 14:28 | 153845 · E 14:30. [nao verificado] |
| 53947 | 14:29 | 153848 · E 14:30. [nao verificado] |
| 55006 | 14:31 | 153851 · E 14:30. [nao verificado] |

Esses exemplos cabem no complemento de **DECLARADO-X-GRAVADO-PASSIVO-11**, obra existente em `app/docs/BACKLOG.md:1937`. O estado `aplicada` não comprova fidelidade ao declarado: nesse ramo a função retorna apenas pela presença de `materializada_em` (`app/chamados/services/materializacao.py:440`; `app/chamados/services/materializacao.py:448`).

**Decisões do admin.**

- **46751:** conferir o fato de 20/10 e a entrada E 07:00; a previsão recusa a aplicação por paridade. [nao verificado] A tela **Resolver dia** oferece Trabalhou, Faltou e Justificado; a view chama a porta de decisão do dia (`app/templates/ponto/partials/_veredito_dia.html:6`; `app/templates/ponto/partials/_veredito_dia.html:21`; `app/ponto/views.py:3340`). O êxito de uma dessas decisões neste caso não foi medido. [nao verificado]
- **53920 e 55313:** decidir se aceita o fato já registrado ou se precisa corrigir o fato. [nao verificado] Validar pelo fio pode quitar a pergunta, mas `ja_satisfeita` expressamente preserva o espelho; não grava a hora declarada (`app/chamados/services/materializacao.py:453`; `app/templates/chamados/partials/modal_fio.html:279`; `app/chamados/services/fio.py:585`).

**Dias retidos e roteiro.**

| Dia / célula | Dependência identificada | O que efetivamente tira a retenção |
|---|---|---|
| 21/10 · 141797 | 46756/46757: **b — DECLARADO-X-GRAVADO**; 46758 sem efeito posterior medido. [nao verificado] | Tratar o desencontro antes de validar; depois conferir nova lavra e retentor. A combinação que libera o dia não foi medida. [nao verificado] |
| 30/10 · 141806 | 52035 prevê S 15:50; existe S 21:28:20 no dado. [nao verificado] | **Não medido**: não foi calculado o veredito/retentor após o plantio. [nao verificado] |
| 03/11 · 141810 | 53565 e 53644 sem declaração. [nao verificado] | O colaborador informa o ocorrido e os horários; depois se confere aplicação e nova retenção. **Não medido** qual resposta libera o dia. [nao verificado] |
| 12/11 · 141819 | 57214 sem declaração. [nao verificado] | O colaborador informa o ocorrido e o horário pedido. **Não medido** o desfecho do dia. [nao verificado] |

A porta de resposta pertence ao colaborador e exige interação com o relógio para resposta de hora (`app/chamados/views.py:1228`; `app/chamados/views.py:1265`; `app/chamados/views.py:1274`).

**08/11 e 11/11 não retêm**, embora tenham perguntas vivas e células com `furo`; não acrescento esses dias à contagem. [nao verificado]

Para cobrir também as **71 perguntas históricas citadas**, todas recebem **não medido para nova validação**; permanecem na disputa 7263 aberta, mas individualmente estão `viva=false`. [nao verificado]

- **Hora, estado `aplicada` — pk=declarado:** 46755=12:55; 49271=15:51; 49272=07:00; 49273=13:00; 49650=13:00; 57212=14:30; 50103=15:50; 50104=07:00; 50105=13:00; 51082=FOLGA; 51124=13:00; 51142=14:30; 51218=21:18; 51747=15:50; 52221=07:03; 52265=13:00; 52283=14:30; 52367=15:50; 52542=06:57; 52594=13:00; 52619=14:28; 52727=15:56; 53947=14:29; 54029=15:52; 54245=07:00; 54292=13:01; 54317=14:29; 54393=15:51; 54592=FOLGA; 54634=FOLGA; 54664=FOLGA; 54936=06:57; 54980=13:00; 55006=14:31; 55104=15:55; 55518=15:52; 55786=13:00; 55812=14:30; 55950=15:50; 56313=13:00; 56342=14:30; 56466=15:50; 57131=07:00; 57184=13:00; 57199=14:30. [nao verificado] [nao verificado]
- **Hora, declarado nulo, estado nulo:** 48771, 48772, 49648, 49649, 50399, 50400, 50401, 50402, 50734, 50786, 50906, 51748, 51913, 53496, 53544, 53880, 54748, 55739, 56249, 56724, 56922 e 57553. [nao verificado]
- **Escolha, declarado nulo, estado nulo:** 53040. [nao verificado]
- **20/10, hora, históricas citadas apenas na FICHA:** 46752=13:01, 46753=07:00 e 46754=15:55. Estão materializadas via declaração; seu `estado_aplicacao` não veio no EXTRA. [nao verificado]

## Caso 03 -- col1032

**Comp 11: nenhum dia retido; nenhuma pergunta viva.** [nao verificado]

| Perguntas | Dia | Tipo / declarado | `estado_aplicacao` | Disputa | Classe |
|---|---|---|---|---|---|
| 57457, 57458, 57459, 57460 | 11/11 | hora / nulo | nulo | 8120 fechada | não medido; históricas. [nao verificado] |
| 57019 | 12/11 | escolha / nulo | nulo | 8097 fechada | não medido; histórica. [nao verificado] |
| 57129 | 12/11 | escolha / nulo | nulo | 8100 fechada | não medido; histórica. [nao verificado] |
| 57157 | 12/11 | escolha / nulo | nulo | 8102 fechada | não medido; histórica. [nao verificado] |
| 57279 | 12/11 | escolha / nulo | nulo | 8108 fechada | não medido; histórica. [nao verificado] |
| 57446 | 13/11 | hora / nulo | nulo | 8118 fechada | não medido; histórica. [nao verificado] |

As nove foram materializadas via `celula_absolveu`. As quatro batidas de 11/11 estão retratadas e não aprovadas; há abono aprovado 5574. Em 12/11 há quatro batidas aprovadas, e a célula consta `ok/concorde`; 13/11 consta `folga/concorde`. [nao verificado]

**Roteiro:** nenhum dia retido a retirar nesta fotografia. A divergência de tela aguardando print permanece sem diagnóstico adicional. [nao verificado]

## Caso 04 -- col246

**Comp 11: retém somente 01/11, célula 136215, por `espelho_cobranca_indevida`. Há duas perguntas vivas na disputa 7755 fechada.** [nao verificado]

| Pergunta | Motivo | Tipo / declarado | `estado_aplicacao` | Disputa | Classe exclusiva |
|---|---|---|---|---|---|
| 52485 | batida fora de escala grave | escolha / nulo | nulo | 7755 fechada | **b — ESPERA OBRA FIO-E0-V4-E3-LEITORES**, com conferência do passivo E4B. [nao verificado] |
| 52715 | `orfao_14h` | hora / nulo | nulo | 7755 fechada | **b — ESPERA OBRA FIO-E0-V4-E3-LEITORES**, com conferência do passivo E4B. [nao verificado] |

O complemento cabe na **E3-LEITORES**, que reúne fio, tela e app sob `tarefa_do_dia`, e na **E4B**, que cobre o passivo do fio morto (`app/docs/BACKLOG.md:1899`; `app/docs/BACKLOG.md:1921`). A linha da E4B registra pouso; isso não demonstra execução ou cura deste caso.

**Qual chamado faz a cobrança?** O dado lista o **36424**, `turno_aberto`, em análise, associado a 01/11; a justificativa 3432 está rejeitada. O pai 36324 está resolvido. [nao verificado] O cartório obtém as cobranças pelos módulos de furo e pelo dia do contexto, não simplesmente pelo pai da disputa (`app/ponto/services/cartorio.py:127`; `app/ponto/services/cartorio.py:135`).

**O elo exato pergunta→chamado não foi medido:** faltam `chamado_destino`, relacionados, contexto, ata e protestos. Não certifico a FK **52715→36424**, nem que 52485 seja a autora da cobrança. [nao verificado] O veredito `cobranca_indevida` pode derivar de dois códigos diferentes; a sonda não os discriminou (`app/escala/models.py:1613`).

**Por que `celula_absolveu` não encerrou:**

1. `cobranca_indevida` é veredito acusador (`app/escala/models.py:1767`).
2. A seleção de perguntas absolvíveis exclui células acusadoras (`app/chamados/reconciliador.py:642`).
3. O juiz C6 mantém viva a pergunta sob esse veredito (`app/chamados/juizes.py:1450`).
4. **A disputa fechada não é a causa dessa exclusão:** `celula_absolveu` alcança também disputas fechadas (`app/chamados/reconciliador.py:572`). Já a alternativa de órfãs por lâmpada filtra somente disputas abertas (`app/chamados/reconciliador.py:1050`).

**Sítio que deveria encerrar a cobrança:** quando reconhece `COBRANCA_SEM_FURO` ou `COBRANCA_EM_DIA_SEM_TRABALHO`, o cartório chama a reconciliação canônica e agenda rejulgamento (`app/ponto/services/cartorio.py:1565`; `app/ponto/services/cartorio.py:1573`; `app/ponto/services/cartorio.py:1576`). Há também encerramento por marco satisfeito (`app/ponto/services/cartorio.py:1539`).

**Roteiro do dia 01/11 — não medido:** conferir a âncora das duas perguntas e o resultado de `tarefa_do_dia`/alcance E4B; identificar a cobrança efetiva, conferir sua reconciliação e o rejulgamento; então consultar novamente o retentor. O resultado dessas portas e a razão histórica de não terem encerrado 36424 não constam do dado. [nao verificado] O backfill E4B não executa rejulgamento: registra `rejulgar_janela`, portanto não basta presumir que ele liberará a folha (`app/chamados/management/commands/backfill_fio_morto_e4b.py:19`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:148`).

As nove históricas citadas têm declarado e estado nulos, `viva=false`, classe **não medido para nova validação**. [nao verificado]

| Perguntas | Tipo | Disputa fechada |
|---|---|---|
| 49603; 49645 | escolha; hora | 7452. [nao verificado] |
| 52482 | hora | 7755. [nao verificado] |
| 54190; 54192 | hora; escolha | 7891. [nao verificado] |
| 54880; 54882; 54986; 55097 | hora; escolha; hora; hora | 7956. [nao verificado] |

## Caso 05 -- col265

**Comp 11: retém somente 31/10, célula 135811, por `espelho_cobrado`. Há três perguntas vivas.** [nao verificado]

| Pergunta | Dia / motivo | Tipo / declarado | `estado_aplicacao` | Disputa | Classe exclusiva |
|---|---|---|---|---|---|
| 52184 | 31/10 · batida fora de escala grave | escolha / Confirma | nulo | 7715 fechada | **c — DECISÃO DO ADMIN**. [nao verificado] |
| 52362 | 31/10 · `orfao_14h` | hora / nulo | nulo | 7715 fechada | **b — ESPERA OBRA RAIZ-DA-DISPUTA**. [nao verificado] |
| 55248 | 08/11 · batida fora de escala grave | escolha / Confirma | nulo | 7984 aberta | **c — DECISÃO DO ADMIN**. [nao verificado] |

**52184 e 55248:** o admin decide se aceita a confirmação, pelo botão **Validar** no fio (`app/templates/chamados/partials/modal_fio.html:279`). A disputa fechada não impede, por si, validar uma pergunta viva: a porta recupera sua própria disputa e permite o ato sem reabrir a raiz (`app/chamados/services/fio.py:585`; `app/chamados/services/fio.py:600`).

Para escolhas, `estado_aplicacao=None` significa fora do escopo dessa previsão (`app/chamados/services/materializacao.py:438`). A materialização da escolha não cria nem remove batidas e reconcilia destino/clones (`app/chamados/services/materializacao.py:1069`; `app/chamados/services/materializacao.py:1082`). Portanto, nenhuma das duas recebe classe a.

**52362:** falta declaração e a dona está fechada. [nao verificado] A consulta do app coloca em pendentes somente perguntas sem resposta **e** em disputa aberta; as demais vivas ficam no histórico (`app/chamados/services/pergunta_no_app.py:61`; `app/chamados/services/pergunta_no_app.py:71`). O complemento cabe em **RAIZ-DA-DISPUTA**, que já contém a decisão sobre dona fechada — reabrir, nova ou Pauta — sem criar id (`app/docs/BACKLOG.md:1931`).

**Roteiro do dia 31/10 — não medido:** decidir 52184 e restabelecer o caminho exigível da 52362 são ações identificadas; não foi demonstrado que elas bastem para liberar a célula. [nao verificado] O cartório só escolhe `cobrado` nesse ramo quando há fio que protesta; isso depende dos marcos e da ata (`app/ponto/services/cartorio.py:1458`; `app/ponto/services/cartorio.py:1479`; `app/ponto/services/cartorio.py:1502`). Esses insumos e o julgamento posterior não vieram na sonda. [nao verificado]

Não atribuo automaticamente a retenção ao chamado 36213 nem à pergunta 52362: o dado não fornece os vínculos e a ata necessários para essa identificação. [nao verificado]

**08/11 não retém**, apesar da 55248 viva em disputa aberta. O atestado 5594 de 13–14/11 está aguardando decisão, mas não aparece como retenção no resultado fornecido. [nao verificado]

As nove históricas citadas recebem classe **não medido para nova validação**:

| Perguntas | Tipo / declarado | `estado_aplicacao` | Disputa fechada |
|---|---|---|---|
| 50066 | escolha / Confirma | nulo | 7504. [nao verificado] |
| 50096 | hora / nulo | nulo | 7504. [nao verificado] |
| 51554, 51648, 51662, 51744 | hora / nulo | nulo | 7639. [nao verificado] |
| 52174 | hora / nulo | nulo | 7715. [nao verificado] |
| 55244, 55505 | hora / nulo | nulo | 7984, atualmente aberta. [nao verificado] |

## Contagem do lote

Contagem das **perguntas vivas**, com uma classe por pergunta. [nao verificado]

| Caso | a | b | c | d | não medido | Total |
|---|---:|---:|---:|---:|---:|---:|
| 01 | 0 | 0 | 0 | 0 | 0 | 0 |
| 02 | 0 | 4 | 3 | 0 | 9 | 16 |
| 03 | 0 | 0 | 0 | 0 | 0 | 0 |
| 04 | 0 | 2 | 0 | 0 | 0 | 2 |
| 05 | 0 | 1 | 2 | 0 | 0 | 3 |
| **Total** | **0** | **7** | **5** | **0** | **9** | **21** |

Contagem dos **colab-dias retidos**, classificando a dependência demonstrada; o efeito de liberação permanece não medido onde indicado no roteiro. [nao verificado]

| Caso | a | b | c | d | não medido | Total |
|---|---:|---:|---:|---:|---:|---:|
| 01 | 0 | 0 | 0 | 0 | 0 | 0 |
| 02 | 0 | 1 | 0 | 0 | 3 | 4 |
| 03 | 0 | 

[truncado em 20000 de 20509 caracteres]

_4729 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
