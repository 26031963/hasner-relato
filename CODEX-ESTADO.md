# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 03:28** · publicado em **19/11/2026 03:29** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 03:28 · sessao d379f5ba58d0 · ESCALA-NAO-DESCREVE · sonda · origem=Code

Não posso entregar a sonda que chama `anomalias_da_frota`: há um caminho que pode escrever em arquivo de log, e a instrução foi não usar a função se ela escrever qualquer coisa.

A função chama `assinaturas_do_colab`, que pode chamar `get_motor_cct`; se a leitura do parâmetro falhar, `get_motor_cct` registra uma exceção. Na configuração herdada pela sombra, o logger raiz envia erros ao handler de arquivo `/app/logs/errors.log`. A transação `READ ONLY` do molde protege o banco, mas não impede essa escrita em arquivo. [app/ponto/services/esmeril_espelho.py:235](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:235), [app/ponto/services/esmeril_espelho.py:170](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:170), [app/core/regua_cct.py:442](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/regua_cct.py:442), [app/config/settings/sombra.py:27](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/config/settings/sombra.py:27), [app/config/settings/saas.py:136](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/config/settings/saas.py:136), [app/config/settings/saas.py:183](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/config/settings/saas.py:183).

Também há escrita de warning no caso de empate entre aplicações de CCT vigentes; se esse caso ocorre no dado da sombra é [não verificado]. [app/core/regua_cct.py:265](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/regua_cct.py:265), [app/core/regua_cct.py:366](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/regua_cct.py:366).

A janela solicitada confere: o detector recebe `hoje - 60 dias` até `hoje - 1 dia`. A porta `turnos_do_colab` retorna turnos por `data_turno` na janela, mas seu código consulta `Batida` diretamente com `retratada_em__isnull=True`; ela não chama `batidas_apuraveis` nessa carga. [app/ponto/services/esmeril_espelho.py:241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:241), [app/ponto/turnos.py:1850](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:1850), [app/ponto/turnos.py:1867](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:1867).

Universo, contagens e ocorrência efetiva dos caminhos de log: [não verificado].

perguntas de lei: 0

## 19/11 03:17 · sessao c55e102a4eb1 · DST-L2-JZ1 · verificador · origem=Code

**Veredito: BLOQUEIA.** Há uma corrida que pode sobrescrever uma decisão do DP, uma janela em que o fecho fica sem seus efeitos associados e uma mudança de significado de “fato novo” que o R8 não cobre. A exclusão de `_APROVANDO` também altera o universo literal de D3 e precisa de decisão de lei. [app/ponto/services/justificativa.py:304](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:304), [app/ponto/services/cartorio.py:1030](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1030), [app/chamados/models.py:1389](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/models.py:1389).

**Achados**

1. **BLOQUEIA — decisão concorrente pode ser sobrescrita.** Trecho: `filter(..., status='pendente')`, seguido de `list(_qs...)` e depois `j.save(update_fields=['status', ...])`, sem trava da justificativa nem condição de status no update. Entre a leitura e o save automático, o DP pode aprovar a mesma linha; o save automático pode então gravar `rejeitada` sobre a aprovação. A porta do DP usa `atomic`, trava os chamados na ordem e faz `select_for_update()` da justificativa; o escritor novo não compartilha essa proteção. A execução efetiva dessa corrida em banco está [não verificado]. [app/ponto/services/justificativa.py:304](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:304), [app/ponto/services/justificativa.py:319](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:319), [app/ponto/services/justificativa.py:140](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:140), [app/ponto/services/justificativa.py:364](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:364).

2. **BLOQUEIA — fecho, rejeição e ocorrência não formam necessariamente uma transação.** Trecho: `_fech = [... transicionar_com_pauta(...)]` e, só depois, `rejeitar_justificativas_dos_chamados(...)` e `_ocorrencia_da_rejeicao_no_fecho(...)`; o mesmo ocorre em `_porm`. O `atomic` da porta de transição termina ao retornar, antes dos dois novos efeitos. Quando não houver transação externa, uma falha posterior pode deixar chamado fechado com justificativa pendente, ou justificativa rejeitada sem ocorrência D5. O reconciliador, em contraste, envolve seu ato na transação de S128. Quais entradas reais fornecem uma transação externa em todos os casos estão [não verificado]. [app/ponto/services/cartorio.py:1030](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1030), [app/ponto/services/cartorio.py:1570](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1570), [app/chamados/services/fecho_runtime.py:161](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/fecho_runtime.py:161), [app/chamados/reconciliador.py:543](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:543).

3. **BLOQUEIA — a rejeição automática passa a ser fato novo para outro chamado do mesmo dia.** Trecho: o novo save inclui `atualizado_em`, e `houve_fato_novo` aceita qualquer justificativa do colaborador e dia com esse carimbo em `(desde, ate]`. Se um irmão foi encerrado via admin antes dessa rejeição e os demais fatos não satisfaziam o predicado, sua `recusa_da_reabertura` deixa de retornar `admin_sem_fato_novo`; outras guardas ainda podem recusar a reabertura. O teste R8 verifica o chamado encerrado pelo sistema e só imprime `fato`, sem afirmar seu valor nem criar o irmão administrativo. Portanto não prova a ausência desse efeito. [app/ponto/services/justificativa.py:322](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:322), [app/ponto/services/cartorio.py:393](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:393), [app/ponto/services/cartorio.py:405](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:405), [app/chamados/models.py:1389](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/models.py:1389), [app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:146](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:146).

4. **BLOQUEIA — pergunta de lei: `_APROVANDO` foi excluída de D3 mesmo com portão aberto.** Trecho: `_qs.exclude(pk=_APROVANDO.get())` ocorre **antes** do `if portao_de_retencao`, de modo que vale também no reconciliador, que chama com `False`. Se o ponteiro é a própria J em aprovação, o predicado de retenção não responde `justificativa_alheia`; ainda assim a J pendente apontada é excluída. Isso contraria o universo literal de D3 fornecido no enunciado; não é consequência da condição R7, que fala de retenção respondendo reter. O teste C4 demonstra a razão prática pretendida — evitar rejeição transitória e ocorrência falsa —, mas a autorização dessa exceção ao universo de D3 está **[não verificado]**. Pergunta: a exclusão deve valer nos dois caminhos, inclusive no reconciliador? [app/ponto/services/justificativa.py:303](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:303), [app/chamados/reconciliador.py:452](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:452), [app/chamados/reconciliador.py:574](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:574), [app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:343](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:343).

5. **BLOQUEIA — R9 não sela o leitor até o ESTADO.** Trecho: o teste verifica a chave no carimbo e a saída do comando, mas não executa `fio_estado.linha` nem o gerador do ESTADO. Apagar ou quebrar o leitor final pode deixar esse teste verde. O leitor existe no código: compara genericamente as chaves do carimbo e mostra apenas as fora do esperado; o gerador o chama. Os logs RED/verde, mutantes por sítio e medições antes/depois exigidos no dado fornecido estão **[não verificado]**; a árvore examinada não é um repositório Git, portanto o hash e o diff também não foram confirmados independentemente. [app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:273](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:273), [bin/fio_estado.py:121](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/bin/fio_estado.py:121), [bin/fio_estado.py:191](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/bin/fio_estado.py:191), [bin/gerar_estado.py:260](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/bin/gerar_estado.py:260).

**Campos e efeitos comprovados**

| Campo ou dado | Quem escreve e quem lê, nos caminhos examinados |
|---|---|
| `Justificativa.status` | A função nova grava `rejeitada`; aprovação e rejeição do DP também escrevem status. O próprio lote e D4 filtram `pendente`; API e painel leem o estado. Há outros escritores de rejeição existentes: sua conformidade com a porta aplicável está [não verificado]. [justificativa.py:304](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:304), [justificativa.py:205](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:205), [justificativa.py:374](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:374), [api/views.py:2105](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/api/views.py:2105), [ponto/views.py:1739](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/views.py:1739). |
| `descricao` | O colaborador a fornece pela API; a rejeição automática acrescenta motivo ao mesmo texto; a API devolve o campo ao colaborador. Assim, descrição de entrada e anotação do sistema compartilham o campo. [api/views.py:1994](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/api/views.py:1994), [justificativa.py:323](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:323), [api/views.py:2107](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/api/views.py:2107). |
| `atualizado_em` | `auto_now`, incluído explicitamente no save novo; o painel usa a **data** para decisões “hoje” e `houve_fato_novo` usa o **instante** como mudança de insumo. A configuração usa `USE_TZ=True` e fuso `America/Sao_Paulo`. [ponto/models.py:179](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/models.py:179), [justificativa.py:324](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:324), [ponto/views.py:1741](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/views.py:1741), [cartorio.py:405](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:405), [base.py:93](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/config/settings/base.py:93). |
| Ponteiro e `chamado_origem` | `apontar_justificativa` escreve o ponteiro; a API pode escrever a origem ao criar ou completar a justificativa. O lote lê a união das duas relações; D4 também lê ambas. Outros escritores e leitores completos estão [não verificado]. [vinculos.py:81](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/vinculos.py:81), [api/views.py:1989](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/api/views.py:1989), [justificativa.py:294](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:294), [justificativa.py:342](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:342). |
| `criado_em` e `resolvido_em` | O primeiro nasce com `auto_now_add`; a transição terminal carimba o segundo e a reabertura o limpa. D4 compara **instantes**, com `criado_em > resolvido_em` estrito: igualdade não conta. Sua unidade é **pares justificativa × chamado**, deduplicados entre ponteiro e origem, não necessariamente justificativas distintas. [ponto/models.py:179](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/models.py:179), [chamados/models.py:243](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/models.py:243), [justificativa.py:330](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:330). |
| Ocorrência D5 | O cartório chama `anexar_ocorrencia` após rejeição, passando conteúdo, tipo e IDs; a porta grava `FollowupChamado` e deriva chave por chamado, tipo, referência e autor. O teste lê ocorrências por conteúdo. Outros consumidores do follow-up estão [não verificado]. [cartorio.py:47](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:47), [nota.py:62](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/nota.py:62), [nota.py:88](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/nota.py:88), [test_dst_l2_jz1_c3_lote_portao.py:43](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:43). |
| Chave nova do vigia | O vigia chama D4, grava o valor no `censo` e zero em `esperado`; `fio_estado` compara os dois e só exibe a chave quando divergem. O cálculo é de frota, inclusive quando o comando imprime em recorte de empresa; o carimbo é escrito apenas para frota. [vigiar_fio_morto.py:103](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/management/commands/vigiar_fio_morto.py:103), [vigiar_fio_morto.py:190](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/management/commands/vigiar_fio_morto.py:190), [vigiar_fio_morto.py:243](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/management/commands/vigiar_fio_morto.py:243), [bin/fio_estado.py:124](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/bin/fio_estado.py:124). |

**Dinheiro e restante do checklist.** A alteração nova grava status, descrição, carimbo e ocorrência; não encontrei, nesses trechos, escrita direta em motor, folha, espelho, `DiaPago` ou export. Isso não prova ausência de efeito indireto: fechamento lê justificativas **aprovadas**, e a corrida do achado 1 pode alterar esse conjunto. Impacto efetivo em dinheiro ou export está [não verificado]. [justificativa.py:319](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:319), [fechamento.py:660](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/fechamento.py:660).

O `post_save` sai por `em_julgamento` dentro do cartório; fora dele chama `julgar_celula`. Os testes declaram os dois cenários, mas sua execução e a morte dos mutantes estão [não verificado]. Não demonstrei segundo juiz registrado, relógio novo fora da borda ou pouso realizado; esses pontos permanecem [não verificado]. [ponto/signals.py:85](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/signals.py:85), [ponto/signals.py:161](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/signals.py:161), [cartorio.py:992](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:992).

BLOQUEIA: 5 | FILA: 0 | perguntas de lei: 1

## 19/11 03:12 · sessao da34ab3038c3 · ESCALA-NAO-DESCREVE · sonda · origem=Code

A medição pedida **não pode ser concluída com a janela exata a partir da lavra descrita pelo código**: a geração aceita um `hoje` fornecido pelo chamador, mas a proposta guardada não registra esse valor nem os limites da janela. `lavrado_em` registra o relógio ao fim da geração; usá-lo como `hoje` exigiria uma hipótese. [app/ponto/services/esmeril_espelho.py:215](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:215), [app/ponto/services/esmeril_espelho.py:241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:241), [app/ponto/services/esmeril_espelho.py:246](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:246), [app/ponto/services/esmeril_espelho.py:253](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:253).

**A. Código**

1. **Gravação e leitura.** `lavrar` chama `gravar_lavra(d)` e também escreve um espelho JSON; o banco é a autoridade. O caminho padrão do arquivo é `logs/esmeril_espelho.json`. [app/ponto/services/esmeril_espelho.py:28](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:28), [app/ponto/services/esmeril_espelho.py:321](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:321).

   Trechos literais da gravação:

   ```python
   CHAVE_LAVRA = 'esmeril_espelho'
   ```

   ```python
   MetricaSnapshot.objects.update_or_create(
       chave=CHAVE_LAVRA, escopo='frota', escopo_id=None, data_ref=hoje,
       ...
       'valor': None, 'detalhe': d or {}})
   ```

   O campo é `MetricaSnapshot.detalhe`, um `JSONField`; `ler_lavra()` busca o snapshot mais recente por `data_ref` e, na sombra, verifica o marcador antes de devolvê-lo. [app/inteligencia/models.py:81](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/inteligencia/models.py:81), [app/inteligencia/models.py:95](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/inteligencia/models.py:95), [app/ponto/services/esmeril_espelho.py:291](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:291), [app/ponto/services/esmeril_espelho.py:359](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:359).

   Trecho literal do mapa, com chave igual ao PK do colaborador convertido em string:

   ```python
   proposta[str(c.pk)] = {'tipo_escala_id': p['tipo_escala_id'], 'codigo': p['codigo'],
                          'confianca': r['confianca']}
   ```

   O mapa é incluído no retrato sob a chave `proposta`; só recebe resultados com proposta, para colaboradores com assinatura. [app/ponto/services/esmeril_espelho.py:239](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:239), [app/ponto/services/esmeril_espelho.py:253](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:253).

2. **Dia usado pelo detector.** Ele consulta `batidas_apuraveis` e agrupa cada batida pela data civil local do seu próprio instante:

   ```python
   for b in batidas_apuraveis(colab, de, ate):
       lt = _tz.localtime(b.timestamp)
       por_dia[lt.date()].append(lt)
   ```

   Assim, uma batida após meia-noite cai no dia civil seguinte, não necessariamente no dia do turno; o próprio módulo declara a possibilidade de inflar `dias` e deslocar a entrada modal. [app/escala/detector_proposta.py:18](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/detector_proposta.py:18), [app/escala/detector_proposta.py:49](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/detector_proposta.py:49).

3. **Cíclica e não cíclica.** A lavra não guarda o ciclo. A tela consulta o `tipo_ciclo` **atual** do `TipoEscala` pelo ID da proposta. Sua constante é literalmente:

   ```python
   CICLICOS = ('12x36', '24x48')
   ```

   O predicado retorna `'ciclico'` para esses dois valores e `'aplicar'` para os demais que passem pelas outras condições; um template apagado não entra no mapa. A classificação histórica no momento da lavra é [não verificado]. [app/escala/models.py:386](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/models.py:386), [app/escala/services/cadastro_realidade.py:819](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:819), [app/escala/services/cadastro_realidade.py:830](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:830).

4. **Confiança e botão.** O campo é `confianca`. Para proposta existente, o detector atribui `alta` ou `media`; `insuficiente` acompanha `proposta: None`. [app/escala/detector_proposta.py:118](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/detector_proposta.py:118).

   Trechos literais:

   ```python
   if n >= MIN_DIAS_ALTA and coerente and estavel:
       conf, pq = 'alta', ...
   ...
       conf, pq = 'media', '; '.join(_faltas)
   ```

   O predicado do botão não consulta `confianca`; a view a lê para compor o motivo passado à porta de gravação:

   ```python
   if botao_da_proposta(v, proposta, sig, ciclos_dos_destinos({'p': proposta})) != 'aplicar':
   ```

   ```python
   motivo='proposta do esmeril (lavra de %s): %s, confianca %s' % (
       lavra.get('lavrado_em') or '?', proposta.get('codigo') or '?', proposta.get('confianca') or '?'),
   ```

   [app/escala/services/cadastro_realidade.py:830](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:830), [app/escala/views.py:493](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/views.py:493).

**B. Sonda parcial, não executada**

A sonda abaixo lê o universo da lavra pela função real, classifica pelo ciclo atual e conta confiança e empresa. Para cada proposta não cíclica, registra erro: sem os limites comprovados não chama o pareador para produzir uma contagem que poderia pertencer a outra janela. Portanto, **não entrega a lista de expostos nem um resultado noturno utilizável quando houver propostas não cíclicas**. O dado raspado fornecido não informa o conteúdo da lavra nem os limites efetivos: [não verificado]. A ausência de lavra no banco resulta em `UNIVERSO=0` com motivo, conforme `ler_lavra()`; falha da guarda ou do marcador resulta em erro. [app/ponto/services/esmeril_espelho.py:359](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:359).

```python
import collections
import os
import sys

from django.conf import settings
from django.db import connection, transaction

from colaboradores.models import Colaborador
from escala.models import TipoEscala
from escala.services.cadastro_realidade import ciclos_dos_destinos
from ponto.services.esmeril_espelho import ler_lavra


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def main():
    universo = 0
    expostos = 0
    erros = 0
    etapa = "GUARDA_OU_LAVRA"
    classes = collections.Counter()
    confiancas = collections.Counter()
    empresas = collections.Counter()
    linhas = []

    try:
        with transaction.atomic():
            guarda_sombra()
            lavra = ler_lavra()

            if lavra is None:
                linhas.append("MOTIVO=SEM_LAVRA_VALIDA_NO_BANCO")
            else:
                etapa = "MAPA_PROPOSTA"
                if "proposta" not in lavra:
                    linhas.append("MOTIVO=CAMPO_PROPOSTA_AUSENTE_NA_LAVRA")
                else:
                    propostas = lavra["proposta"]
                    if not isinstance(propostas, dict):
                        raise ValueError("mapa_proposta")
                    universo = len(propostas)

                    etapa = "CICLOS_ATUAIS"
                    # Uma proposta malformada não deve impedir os demais casos.
                    validas = {
                        chave: p
                        for chave, p in propostas.items()
                        if isinstance(p, dict)
                    }
                    ciclos = ciclos_dos_destinos(validas)

                    for chave, proposta in propostas.items():
                        cid = None
                        try:
                            # Savepoint permite seguir após erro de consulta por caso.
                            with transaction.atomic():
                                if not isinstance(chave, str):
                                    raise ValueError("chave")
                                cid = int(chave)
                                if cid <= 0 or str(cid) != chave:
                                    raise ValueError("chave")
                                if not isinstance(proposta, dict):
                                    raise ValueError("proposta")

                                tipo_id = proposta.get("tipo_escala_id")
                                if tipo_id not in ciclos:
                                    raise ValueError("tipo_ausente")
                                ciclo = ciclos[tipo_id]
                                if not isinstance(ciclo, str):
                                    raise ValueError("ciclo")

                                colab = Colaborador.objects.only(
                                    "id", "empresa_id"
                                ).get(pk=cid)
                                empresa = colab.empresa_id
                                if empresa is None:
                                    raise ValueError("empresa_ausente")

                                conf = proposta.get("confianca")
                                if conf not in ("alta", "media", "baixa",
                                               "insuficiente"):
                                    raise ValueError("confianca")

                                classe = (
                                    "ciclica"
                                    if ciclo in TipoEscala.CICLICOS
                                    else "nao_ciclica"
                                )

                            classes[classe] += 1
                            confiancas[conf] += 1
                            empresas[empresa] += 1

                            if classe == "nao_ciclica":
                                erros += 1
                                linhas.append(
                                    "ERRO col%d empresa=%s "
                                    "motivo=JANELA_DO_DETECTOR_NAO_COMPROVADA"
                                    % (cid, empresa)
                                )
                        except Exception:
                            erros += 1
                            if cid is not None and cid > 0:
                                linhas.append(
                                    "ERRO col%d motivo=CASO_NAO_MEDIDO" % cid
                                )
                            else:
                                linhas.append("ERRO motivo=CHAVE_INVALIDA")

                    linhas.append(
                        "CLASSIFICADAS ciclicas=%d nao_ciclicas=%d"
                        % (classes["ciclica"], classes["nao_ciclica"])
                    )
                    for conf, total in sorted(confiancas.items()):
                        linhas.append(
                            "CONFIANCA %s=%d" % (conf, total)
                        )
                    for empresa, total in sorted(
                        empresas.items(), key=lambda item: str(item[0])
                    ):
                        linhas.append(
                            "EMPRESA %s propostas=%d" % (empresa, total)
                        )
                    linhas.append(
                        "EXPOSTOS=NAO_MEDIDO "
                        "motivo=JANELA_DO_DETECTOR_NAO_COMPROVADA"
                    )
    except Exception:
        erros += 1
        linhas.append("ERRO etapa=" + etapa)

    for linha in linhas:
        print(linha)
    print("CONTADOR=%d UNIVERSO=%d ERROS=%d" % (expostos, universo, erros))
    print("FIM_SONDA_NOTURNO")
    return 0 if erros == 0 else 1


sys.exit(main())
```

Para obter o número solicitado é preciso comprovar, por evidência externa à proposta guardada, qual foi o `hoje` passado à geração daquela lavra; a identificação desse valor é [não verificado]. [app/ponto/services/esmeril_espelho.py:215](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:215), [app/ponto/services/esmeril_espelho.py:241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:241).

perguntas de lei: 1

## 19/11 02:25 · sessao 71601a41dd9b · ESCALA-NAO-DESCREVE · verificador · origem=Code

**Não considero a árvore pronta para o opus final.** Há dois bloqueios identificáveis pela leitura: a proposta passa a ser guardada apesar da L-042, e a nova ação permite aplicar uma proposta calculada por dia civil mesmo no contraexemplo noturno que o próprio detector declara problemático. [app/docs/LEIS.md:89] [app/ponto/services/esmeril_espelho.py:241] [app/escala/detector_proposta.py:18] [app/escala/views.py:502]

1. **Quem escreve e quem lê.**

   - `ata.orfas` é lido de cada `CelulaDia`; `assinaturas_do_colab` soma seus comprimentos em `medida['orfas']`. `anomalias_da_frota` escreve os mapas `orfas` e `orfas_desde`, este último com o início da janela usada na medição. `lavrar` entrega o resultado a `gravar_lavra`, que grava o snapshot; `ler_lavra` lê esse snapshot. [app/ponto/services/esmeril_espelho.py:99] [app/ponto/services/esmeril_espelho.py:130] [app/ponto/services/esmeril_espelho.py:233] [app/ponto/services/esmeril_espelho.py:321] [app/ponto/services/esmeril_espelho.py:342] [app/ponto/services/esmeril_espelho.py:359]

   - `lista` lê esses mapas e escreve `carga`, `carga_desde` e `carga_texto` nas linhas; `resumo_da_carga` lê as linhas e produz a frase e os totais. Tela e PDF leem `linhas` e `carga_resumo`; os leitores anteriores ainda podem ler `postos`. A carga é contagem de órfãs, e a data é o início da janela, não a data da primeira órfã. [app/escala/services/cadastro_realidade.py:784] [app/escala/services/cadastro_realidade.py:969] [app/escala/services/cadastro_realidade.py:981] [app/escala/services/cadastro_realidade.py:1053] [app/templates/escala/cadastro_x_realidade.html:17] [app/escala/services/cadastro_realidade.py:1183] [app/escala/services/cadastro_realidade.py:1149]

   - `propor_escala` produz a proposta e a confiança; `anomalias_da_frota` grava tanto a frase `destino` quanto `proposta` estruturada. `lista` e `aplicar_proposta_da_lista` leem a proposta da lavra. O ciclo é relido de `TipoEscala` por `ciclos_dos_destinos`; `_vinculos` lê o vínculo de hoje usando as autoridades de cobertura e vínculo do dia registradas em `JUIZES.md`. [app/escala/detector_proposta.py:118] [app/ponto/services/esmeril_espelho.py:241] [app/escala/services/cadastro_realidade.py:753] [app/escala/services/cadastro_realidade.py:819] [app/escala/views.py:493] [app/docs/JUIZES.md:142]

   - `botao_da_proposta` lê vínculo, assinatura, proposta e ciclos; a tela lê seu resultado, e a view o consulta novamente antes do POST. A view passa destino, `hoje` e `motivo` a `realocar_vinculo_para_destino`, que chama `julgar_vinculo` e `executar_vinculo`; `confirm_txt` chega à observação do histórico pela porta de vínculo. [app/escala/services/cadastro_realidade.py:830] [app/templates/escala/cadastro_x_realidade.html:32] [app/escala/views.py:496] [app/escala/regua_defesa.py:315] [app/colaboradores/services/vinculo.py:146] [app/colaboradores/services/vinculo.py:355]

2. **Catraca: bate.** Comparei em memória o literal `LINHAS_REGISTRADAS` com os itens de `censo('app')`, usando exatamente a chave e as linhas cobradas pelo teste: 9 identidades de pareadores e 85 de derivações, sem divergências. O medidor registra a linha AST da chamada reconhecida, não a linha da definição. Em particular, `entradas_fora_do_inicio/B1` é `[524]` e `assinaturas_do_colab/B1` é `[179, 186]`. Isso verifica a catraca estática, não a semântica da jornada. [app/core/tests/test_medida_turno_do_par.py:558] [app/core/medida_turno_do_par.py:197] [app/escala/regua_defesa.py:524] [app/ponto/services/esmeril_espelho.py:179] [app/ponto/services/esmeril_espelho.py:186] [nao verificado]

3. **Delta da catraca contra cada pai:** nos dois diffs fornecidos, somente `app/core/tests/test_medida_turno_do_par.py` muda; nenhum `.py` de produção muda **nesse delta**. A cópia não contém `.git`, portanto a comparação dos pais foi feita pelos anexos fornecidos. [app/core/tests/test_medida_turno_do_par.py:247] [não verificado]

4. **Classes de reprovação.**

   - **BLOQUEIA 1 — proposta guardada:** a L-042 diz que a proposta é recalculada e não guardada; a obra passa a gravar `proposta` estruturada na lavra e a ação posterior a lê. O teste novo exige essa gravação, portanto não é apenas comentário ou intenção. [app/docs/LEIS.md:89] [app/ponto/services/esmeril_espelho.py:243] [app/escala/views.py:493] [app/escala/tests/test_escala_nao_descreve.py:100] [nao verificado]

   - A decisão de destino cíclico aparece tanto no predicado do botão quanto na porta de realocação. É duplicação de guarda observável; não identifiquei em `JUIZES.md` um juiz registrado para essa pergunta que permita classificá-la, por si só, como segundo juiz registrado. [app/escala/services/cadastro_realidade.py:849] [app/escala/regua_defesa.py:313] [app/docs/JUIZES.md:142]

   - A escrita do vínculo encontrada passa por `julgar_vinculo` e `executar_vinculo`; a nova view lê `hoje` e o passa adiante, embora a função de realocação conserve leitura própria para seus outros chamadores. Segundo escritor fora da porta, relógio novo fora da borda, derivação paralela de fonte única ou selo sem mordida: **presença não demonstrada nesta verificação**. [app/escala/views.py:496] [app/escala/regua_defesa.py:315] [app/escala/regua_defesa.py:319] [não verificado]

5. **Contraexemplo de integração — BLOQUEIA 2.** Considere seis turnos noturnos consecutivos de 21h a 5h. O detector já existente agrupa por data civil: as saídas de madrugada e entradas da noite podem formar dias com primeira batida às 5h e última às 21h. Se houver template não cíclico compatível com esses horários, ele pode ser proposto com confiança média; a nova ação aceita proposta não cíclica sem exigir confiança alta e pode gravar esse template a partir de hoje. O próprio detector documenta que o agrupamento civil desloca a entrada modal no noturno e afirma que nada é escrito a partir dele; a ação introduzida pela obra altera essa consequência. É um contraexemplo deduzido do código, não uma ocorrência de produção observada. [app/escala/detector_proposta.py:18] [app/escala/detector_proposta.py:49] [app/escala/detector_proposta.py:95] [app/escala/detector_proposta.py:139] [app/escala/services/cadastro_realidade.py:830] [app/escala/views.py:502]

Execução dos testes Django e ocorrência dos casos em produção: [não verificado]. [nao verificado]

BLOQUEIA: 2  
perguntas de lei: 0

## 19/11 02:20 · sessao 2836601fc984 · CHECK-DE-TELA · sonda · origem=Code

A sonda abaixo é **parcial e não foi executada**: a referência, os tempos, as consultas e a correspondência da cópia ao hash são [não verificado]. A cópia não contém metadados Git verificáveis. [não verificado]

Há dois limites que a sonda declara como erro:

- Não foi comprovada uma rota de tela para `tarefa_do_dia`; a função encontrada recebe colaborador, dia do turno e instante. A sonda não a apresenta como uma tela medida. [app/chamados/services/tarefa_do_dia.py:738]
- No placar sem filtro, se houver retrato lavrado, não foi comprovado o tamanho do universo que produziu aquele retrato. Nesse caso, a carga **b** é declarada não medida; a sonda não troca esse universo por uma contagem atual. [app/chamados/views.py:840]

A porta informada também merece uma correção: o código de `--rodar` usa a rede Docker `saas-hasner_default`, com proxies apontados para a porta local 9; ele não declara `--network none`. Antes de executar a sonda, `--rodar` chama `lavra_de_prod`; o script informa que esse acesso a produção é de leitura. [bin/sombra.sh:60] [bin/sombra.sh:183] [bin/sombra.sh:486]

Rastreio das medições propostas:

1. **Espelho em tela:** `/ponto/espelho/` → `espelho_ponto` → `espelho_do_colab`. **PDF:** `/relatorios/espelho/avulso/` → `espelho_pdf_avulso` → `gerar_pdf_espelho`; o GET abre o formulário e a geração é chamada pelo ramo POST, por isso a sonda mede diretamente o serviço, sem POST. [app/config/urls.py:29] [app/ponto/urls.py:10] [app/ponto/views.py:96] [app/ponto/views.py:160] [app/relatorios/urls.py:28] [app/relatorios/views.py:1050] [app/relatorios/views.py:1126] [app/relatorios/views.py:1145] [app/relatorios/services.py:49]
2. **Tarefa do dia:** rota e view [não verificado]; serviço `tarefa_do_dia`. [app/chamados/services/tarefa_do_dia.py:738]
3. **Central:** `/chamados/` → `painel_gestao` → `montar_contexto_painel` e `pendentes_na_central`. [app/config/urls.py:28] [app/chamados/urls.py:9] [app/chamados/views.py:721] [app/chamados/views.py:726] [app/chamados/views.py:759]
4. **Placar:** `/chamados/placar/` → `placar_json` → `etag_do_placar` e, conforme o ramo, `PlacarNavbar` ou `montar_contexto_painel`. [app/chamados/urls.py:24] [app/chamados/views.py:771] [app/chamados/views.py:812] [app/chamados/views.py:840]
5. **App:** `/api/me/` → `api_me` → leitores incluindo `turno_aberto_de` e `projecao_do_app`. Cada resposta trata de um colaborador. [app/config/urls.py:37] [app/api/urls.py:64] [app/api/views.py:284] [app/api/views.py:306] [app/api/views.py:315] [app/api/views.py:336]

A seleção inclui todos os colaboradores cadastrados, inclusive os sem batidas, e usa a janela corrente de cada empresa; em empate, a mediana inferior e o primeiro máximo por PK. Se o colaborador escolhido não tiver usuário ativo, a carga falha explicitamente. Para o app, ambas as chamadas têm universo de um colaborador: a segunda usa o mesmo máximo de batidas escolhido para o espelho, sem afirmar que seja o usuário mais custoso do app. [app/ponto/models.py:22] [app/ponto/janelas.py:60] [app/colaboradores/models.py:351] [app/api/views.py:336]

O `RequestFactory` recebe usuário, tenant, sessão e mensagens; as views identificadas leem `request.user`, e o processador de contexto também o lê. Outros atributos de middleware necessários em todos os caminhos são [não verificado]. [app/ponto/views.py:113] [app/chamados/views.py:723] [app/api/views.py:286] [app/core/context_processors.py:7]

```python
# Sonda para exec(open(script).read()), em tenant_command shell --schema=juliani.
# Nao imprime respostas, nomes, CPF, SQL nem mensagens de excecao.

import contextlib
import io
import logging
import re
from time import perf_counter

from django.conf import settings
from django.contrib.auth import get_user_model
from django.contrib.messages.storage.fallback import FallbackStorage
from django.contrib.sessions.middleware import SessionMiddleware
from django.db import connection, transaction
from django.db.models import Count, Q
from django.test import RequestFactory
from django.test.utils import CaptureQueriesContext
from django.utils import timezone
from rest_framework.test import force_authenticate

from colaboradores.models import Colaborador
from ponto.janelas import janela_atual
from ponto.views import espelho_ponto
from relatorios.services import gerar_pdf_espelho
from chamados.views import painel_gestao, placar_json
from chamados.services.painel import montar_contexto_painel, qs_vivos
from core.models import PlacarNavbar
from api.views import api_me


class DesfazerSonda(Exception):
    pass


class NaoMedida(Exception):
    pass


erros = 0
fabrica = RequestFactory()
nivel_logging_anterior = logging.root.manager.disable
logging.disable(logging.CRITICAL)


def guardar_sombra():
    cfg = settings.DATABASES["default"]
    if cfg.get("NAME") != "sombra" or cfg.get("HOST") != "sombra_db":
        raise NaoMedida("configuracao fora da sombra")
    if getattr(connection, "schema_name", None) != "juliani":
        raise NaoMedida("schema diferente de juliani")
    with connection.cursor() as cursor:
        cursor.execute("SELECT current_database(), current_schema()")
        banco, schema = cursor.fetchone()
    if banco != "sombra" or schema != "juliani":
        raise NaoMedida("banco ou schema inesperado")


def calcular_selecao():
    """Uma contagem por colaborador na janela corrente da sua empresa."""
    guardar_sombra()
    hoje = timezone.localdate()
    linhas = []
    empresas = list(
        Colaborador.objects.order_by()
        .values_list("empresa_id", flat=True)
        .distinct()
    )
    for empresa_id in empresas:
        inicio, fim = janela_atual(hoje, empresa_id)
        qs = (
            Colaborador.objects.filter(empresa_id=empresa_id)
            .select_related("empresa", "usuario")
            .annotate(
                n_batidas=Count(
                    "batidas",
                    filter=Q(
                        batidas__timestamp__date__gte=inicio,
                        batidas__timestamp__date__lte=fim,
                    ),
                )
            )
        )
        linhas.extend(qs)
    if not linhas:
        raise NaoMedida("nenhum colaborador cadastrado")

    linhas.sort(key=lambda c: (c.n_batidas, c.pk))
    mediana = linhas[(len(linhas) - 1) // 2]
    maior_contagem = linhas[-1].n_batidas
    maior = next(c for c in linhas if c.n_batidas == maior_contagem)
    return mediana, maior, len(linhas)


selecao_cache = None


def selecao():
    global selecao_cache
    if selecao_cache is None:
        selecao_cache = calcular_selecao()
        mediana, maior, total = selecao_cache
        print(
            "SELECAO universo_colaboradores=%d mediana=col%d batidas=%d "
            "maximo=col%d batidas=%d"
            % (total, mediana.pk, mediana.n_batidas, maior.pk, maior.n_batidas)
        )
    return selecao_cache


def usuario_colab(colab):
    if not colab.usuario_id or not colab.usuario.is_active:
        raise NaoMedida("colaborador escolhido sem usuario ativo")
    return colab.usuario


def usuario_gestor():
    # Staff satisfaz _is_gestor e tambem a porta do PDF avulso.
    user = get_user_model().objects.filter(
        is_active=True, is_staff=True
    ).order_by("pk").first()
    if user is None:
        raise NaoMedida("nenhum usuario staff ativo")
    return user


def pedido(caminho, user, parametros=None, drf=False):
    req = fabrica.get(caminho, data=parametros or {})
    req.user = user
    req.tenant = connection.tenant
    SessionMiddleware(lambda request: None).process_request(req)
    req._messages = FallbackStorage(req)
    if drf:
        force_authenticate(req, user=user)
    return req


def executar_revertendo(funcao, medir=False):
    """
    A funcao, incluindo renderizacao e avaliacao pedida pelo chamador, roda
    dentro de atomic. O rollback e provocado pela excecao propria.
    """
    resultado = None
    consultas = escritas = 0
    ms = 0.0
    try:
        with transaction.atomic():
            if medir:
                with CaptureQueriesContext(connection) as capturadas:
                    inicio = perf_counter()
                    resultado = funcao()
                    if hasattr(resultado, "render") and not getattr(
                        resultado, "is_rendered", True
                    ):
                        resultado.render()
                    ms = (perf_counter() - inicio) * 1000
                consultas = len(capturadas)
                escritas = sum(
                    bool(re.match(
                        r"^\s*(?:INSERT|UPDATE|DELETE)\b",
                        consulta["sql"],
                        flags=re.IGNORECASE,
                    ))
                    for consulta in capturadas
                )
            else:
                resultado = funcao()
            raise DesfazerSonda()
    except DesfazerSonda:
        return resultado, consultas, escritas, ms


def verificar_resposta(resposta):
    status = getattr(resposta, "status_code", None)
    if status is not None and status != 200:
        raise NaoMedida("view nao retornou HTTP 200")
    return resposta


def medir(numero, nome, carga, universo, funcao):
    # Views e servicos podem imprimir dados; descartar sua saida.
    with contextlib.redirect_stdout(io.StringIO()), \
         contextlib.redirect_stderr(io.StringIO()):
        _, consultas, escritas, ms = executar_revertendo(funcao, medir=True)
    print(
        "TELA %d %s carga=%s universo=%d consultas=%d escritas=%d ms=%.3f"
        % (numero, nome, carga, universo, consultas, escritas, ms)
    )


def falha(numero, nome, carga, exc):
    # Nao imprimir str(exc): pode conter nome, CPF ou outros dados.
    global erros
    erros = 1
    print(
        "TELA %d NAO MEDIDA %s carga=%s motivo=%s"
        % (numero, nome, carga, type(exc).__name__)
    )


def tentar(numero, nome, carga, funcao):
    try:
        funcao()
    except Exception as exc:
        falha(numero, nome, carga, exc)


def janela_colab(colab):
    return janela_atual(timezone.localdate(), colab.empresa_id)


def medir_espelho(carga, colab):
    user = usuario_colab(colab)
    _, fim = janela_colab(colab)
    req = pedido(
        "/ponto/espelho/",
        user,
        {"mes": fim.month, "ano": fim.year},
    )
    print("AMOSTRA tela=1 carga=%s col%d u%d" % (carga, colab.pk, user.pk))
    medir(
        1, "espelho", carga, 1,
        lambda: verificar_resposta(espelho_ponto(req)),
    )


def medir_pdf(carga, colab):
    user = usuario_gestor()
    _, fim = janela_colab(colab)
    req = pedido("/relatorios/espelho/avulso/", user)
    print("AMOSTRA tela=1_pdf carga=%s col%d u%d" % (carga, colab.pk, user.pk))

    def gerar():
        resultado = gerar_pdf_espelho(
            colaborador=colab,
            mes=fim.month,
            ano=fim.year,
            request=req,
            user=user,
            chamado=None,
        )
        if not resultado.get("pdf_bytes"):
            raise NaoMedida("servico nao devolveu PDF")
        return None

    medir(1, "espelho_pdf_servico", carga, 1, gerar)


def total_central(colab_id):
    # O total e calculado pela mesma montagem da Central, com rollback proprio.
    def apurar():
        ctx = montar_contexto_painel(
            f_status="",
            f_urgencia="",
            f_q="",
            f_colab=str(colab_id) if colab_id is not None else "",
            f_empresa="",
            f_praca="",
            pagina=1,
            f_verbo="",
            f_modo="resumo",
        )
        return int(ctx["total"])

    with contextlib.redirect_stdout(io.StringIO()), \
         contextlib.redirect_stderr(io.StringIO()):
        total, _, _, _ = executar_revertendo(apurar)
    return total


def medir_central(carga, colab_id):
    user = usuario_gestor()
    parametros = {} if colab_id is None else {"colab_id": str(colab_id)}
    universo = total_central(colab_id)
    req = pedido("/chamados/", user, parametros)
    print("AMOSTRA tela=3 carga=%s u%d" % (carga, user.pk))
    medir(
        3, "central", carga, universo,
        lambda: verificar_resposta(painel_gestao(req)),
    )


def medir_placar(carga, colab_id):
    user = usuario_gestor()
    parametros = {} if colab_id is None else {"colab_id": str(colab_id)}

    if colab_id is None:
        retrato = PlacarNavbar.objects.filter(pk=1).first()
        if retrato is not None and retrato.payload_placar:
            raise NaoMedida("universo do retrato lavrado nao comprovado")
        universo = qs_vivos().count()
    else:
        universo = qs_vivos(f_colab=str(colab_id)).count()

    req = pedido("/chamados/placar/", user, parametros)
    print("AMOSTRA tela=4 carga=%s u%d" % (carga, user.pk))
    medir(
        4, "placar", carga, universo,
        lambda: verificar_resposta(placar_json(req)),
    )


def medir_api(carga, colab):
    user = usuario_colab(colab)
    req = pedido("/api/me/", user, drf=True)
    print("AMOSTRA tela=5 carga=%s col%d u%d" % (carga, colab.pk, user.pk))
    medir(
        5, "api_me", carga, 1,
        lambda: verificar_resposta(api_me(req)),
    )


try:
    try:
        mediana, maior, _ = selecao()
        for carga, colab in (("a", mediana), ("b", maior)):
            tentar(1, "espelho", carga, lambda c=carga, x=colab: medir_espelho(c, x))
            tentar(1, "espelho_pdf_servico", carga, lambda c=carga, x=colab: medir_pdf(c, x))
    except Exception as exc:
        falha(1, "espelho_e_pdf", "a_b", exc)

    try:
        guardar_sombra()
        raise NaoMedida("rota e view da tarefa do dia nao comprovadas")
    except Exception as exc:
        falha(2, "tarefa_do_dia", "a_b", exc)

    try:
        mediana, _, _ = selecao()
        tentar(3, "central", "a", lambda: medir_central("a", mediana.pk))
        tentar(3, "central", "b", lambda: medir_central("b", None))
    except Exception as exc:
        falha(3, "central", "a_b", exc)

    try:
        mediana, _, _ = selecao()
        tentar(4, "placar", "a", lambda: medir_placar("a", mediana.pk))
        tentar(4, "placar", "b", lambda: medir_placar("b", None))
    except Exception as exc:
        falha(4, "placar", "a_b", exc)

    try:
        mediana, maior, _ = selecao()
        tentar(5, "api_me", "a", lambda: medir_api("a", mediana))
        tentar(5, "api_me", "b", lambda: medir_api("b", maior))
    except Exception as exc:
        falha(5, "api_me", "a_b", exc)
finally:
    logging.disable(nivel_logging_anterior)
    print("ERROS=%d" % erros)
    print("FIM_CHECK_TELA")
```

Na Central, `universo` é o total de chamados da lista padrão antes da paginação; a página padrão agrupa colaboradores e pagina esses grupos. No placar recomputado, `universo` é a contagem atual de chamados vivos do filtro. A Central captura internamente falhas do atalho de HE, portanto HTTP 200 não prova que esse atalho foi montado. [app/chamados/services/painel.py:333] [app/chamados/services/painel.py:449] [app/chamados/services/painel.py:498] [app/chamados/services/painel.py:135] [app/chamados/views.py:759]

A contagem de escritas registra comandos SQL iniciados por `INSERT`, `UPDATE` ou `DELETE`; escritas indiretas que não apareçam nesses comandos são [não verificado]. O script não foi testado no envelope de execução: compatibilidade em execução é [não verificado]. A gravação do resultado de referência também permanece [não verificado].

perguntas de lei: 0

## 19/11 02:20 · sessao 6ea3da4bc833 · ESCALA-NAO-DESCREVE · verificador · origem=Code

1. **BLOQUEIA — a interação semântica com o comboio continua [não verificado].** A prova fornecida de composição textual resolve a pendência de sobreposição de arquivos da c7: ela informa que os dois arquivos comuns não têm linhas do merge fora da união dos pais e que os arquivos exclusivos são idênticos aos respectivos lados. A prova está em `composicao.txt` fornecido na pergunta; numeração de linhas do anexo **[não verificado]**. Ela não mostra, porém, quais símbolos o comboio alterou nem seus contratos anteriores.

   Os pontos de contato identificáveis que precisam ser comparados com o diff do comboio são:

   | Símbolo usado pela ESCALA | Uso e definição na cópia | Mudança pelo comboio; composição |
   |---|---|---|
   | `_is_gestor` | A nova ação o usa em [views.py:484](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/views.py:484); definição em [chamados/views.py:35](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/chamados/views.py:35). | Assinatura, retorno e significado anteriores **[não verificado]**; composição **[não verificado]**. |
   | `turnos_do_colab`, `batidas_apuraveis` | O esmeril os importa em [esmeril_espelho.py:92](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:92); definições em [turnos.py:1850](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:1850) e [turnos.py:2872](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:2872). | Alteração desses símbolos e composição **[não verificado]**. |
   | `dias_com_ausencia_inteira_do_periodo` | Uso em [esmeril_espelho.py:169](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:169); definição em [turnos.py:2707](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:2707). | Alteração e composição **[não verificado]**. |
   | `ChamadoColaborador` | Importado pelo esmeril em [esmeril_espelho.py:89](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:89); classe em [models.py:33](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/chamados/models.py:33). | Alterações pertinentes de campos ou significado e composição **[não verificado]**. |
   | `minutos_previstos_do_dia`, `marco_ocupado_pela_batida` | Usados pela régua em [regua_defesa.py:355](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/regua_defesa.py:355) e [regua_defesa.py:492](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/regua_defesa.py:492); definições em [utils.py:2241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/utils.py:2241) e [utils.py:387](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/utils.py:387). | Alteração e composição **[não verificado]**. |

   Esses são **candidatos**, não uma lista comprovada de símbolos efetivamente mudados pelo comboio; a exaustividade também é **[não verificado]**. A lista e o `stat` fornecidos identificam arquivos alterados, mas não o delta de seus símbolos **[não verificado]**. É necessário o diff completo base → comboio ou os objetos Git para concluir a comparação; a cópia consultada não é um repositório Git **[não verificado]**.

2. **NÃO BLOQUEIA — o F6 vem da base**, pela combinação da prova fornecida e do diff completo da ESCALA: `cadastro_realidade.py` consta entre os arquivos exclusivos da ESCALA, o merge é informado como idêntico a esse lado, e o diff base → ESCALA não altera `um_dono_por_dia`. A função F6 está em [cadastro_realidade.py:1076](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:1076), e sua chamada precede a leitura da lavra em [cadastro_realidade.py:957](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:957). A conferência direta dos objetos Git é **[não verificado]**.

3. **NÃO BLOQUEIA — L-040 e L-130 não se contradizem no texto examinado.** A primeira determina a lista Cadastro × Realidade ordenada por carga e o mesmo conteúdo no PDF em [LEIS.md:88](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/docs/LEIS.md:88); a segunda define quando uma pergunta está respondida e o tratamento da resposta ao reabrir em [LEIS.md:151](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/docs/LEIS.md:151). Que L-130 seja a única lei alterada pelo comboio é **[não verificado]**, pois seu diff não foi fornecido.

BLOQUEIA: 1  
perguntas de lei: 0

## 19/11 02:19 · sessao 7d81c532abdb · - · - · origem=Ronald (tmux)

- Espelho: `/ponto/espelho/` chama `espelho_ponto`, que lê `request.user.colaborador`, calcula a janela por empresa e chama `espelho_do_colab`. [app/config/urls.py:29](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/config/urls.py:29) [app/ponto/urls.py:10](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/urls.py:10) [app/ponto/views.py:96](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/views.py:96) [app/ponto/views.py:113](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/views.py:113) [app/ponto/views.py:156](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/views.py:156)

- PDF avulso: o GET de `/relatorios/espelho/avulso/` mostra o formulário; a geração chama o serviço `gerar_pdf_espelho` no ramo POST. O serviço pode ser medido diretamente, mas pode recusar uma disputa aberta. [app/relatorios/urls.py:28](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/urls.py:28) [app/relatorios/views.py:1050](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/views.py:1050) [app/relatorios/views.py:1126](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/views.py:1126) [app/relatorios/views.py:1145](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/views.py:1145) [app/relatorios/services.py:49](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/services.py:49) [app/relatorios/pdf_espelho.py:1084](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/pdf_espelho.py:1084)

- O PDF auditável grava `RegistroPDFAuditavel`, e seu conteúdo inclui nome e CPF: a medição precisa reverter a transação e não imprimir nem salvar os bytes. [app/relatorios/pdf_base.py:151](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/pdf_base.py:151) [app/relatorios/pdf_espelho.py:738](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/pdf_espelho.py:738)

- A competência corrente varia com o corte da empresa; `janela_atual` fornece seus limites. A contagem pode usar `Batida.colaborador` e `Batida.timestamp`, incluindo colaboradores com zero batidas no universo da mediana. [app/ponto/janelas.py:36](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/janelas.py:36) [app/ponto/janelas.py:60](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/janelas.py:60) [app/ponto/models.py:22](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/models.py:22)

## 19/11 02:19 · sessao 17766839b9d7 · E4C-F3-LAVRA-FOTO · verificador · origem=Code

**Parecer: BLOQUEIA.** O teste tem mordida nas formas exemplificadas, mas não garante a propriedade geral prometida; há também uma contradição temporal concreta entre o selo e a premissa usada para confiar na lavra. ([test_f3_lavra_foto_sem_decisao.py:3](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:3), [veredito_lavrado.py:213](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:213))

1. **BLOQUEIA — selo com escape.** O AST detecta os exemplos de import direto, apelido de import, módulo, `get_model`, manager relacionado e função aninhada; os testes de mutação correspondentes estão em [test_f3_lavra_foto_sem_decisao.py:165](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:165) e [test_f3_lavra_foto_sem_decisao.py:200](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:200). Mas ele registra apelidos de import sem propagar uma atribuição posterior, e a captura SQL só observa os caminhos executados pela fixture. ([test_f3_lavra_foto_sem_decisao.py:75](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:75), [test_f3_lavra_foto_sem_decisao.py:312](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:312))

   Mutante proposto, sem alterar arquivos: no módulo, `from chamados.models import DecisaoPergunta as _DP` seguido de `_X = _DP`; dentro de `marcos_do_lote`, `if ate is not None and ate.year == 2001: list(_X.objects.all())`. O corpo usa `_X`, nome que o detector não adiciona ao conjunto de apelidos; o ramo não executa com os dois valores da fixture, `ATE` em 9998 e `None`. Assim, a leitura escaparia ao AST e à captura SQL. ([test_f3_lavra_foto_sem_decisao.py:78](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:78), [test_f3_lavra_foto_sem_decisao.py:119](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:119), [test_f3_lavra_foto_sem_decisao.py:272](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:272)) Resultado efetivo da execução do mutante: **[não verificado]**.

2. **NÃO BLOQUEIA — localização e anti-vacuidade.** O detector procura a função de módulo pelo nome e exige exatamente uma ocorrência; o teste também verifica que um nome ausente levanta erro. Não há allowlist no detector: ele forma os nomes relacionados a partir do modelo e acrescenta nomes fixos proibidos. ([test_f3_lavra_foto_sem_decisao.py:51](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:51), [test_f3_lavra_foto_sem_decisao.py:98](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:98), [test_f3_lavra_foto_sem_decisao.py:158](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:158))

3. **NÃO BLOQUEIA — escopo do diff e mecânica do teste.** O diff fornecido contém somente o novo teste; alteração de `.py` de produção pelo diff: não. A cópia não tem metadados Git disponíveis para confirmar independentemente o hash ou comparar a base: **[não verificado]**. O teste usa datas explícitas para `ATE`, `MARCO` e `ATO`; porém, o `setUpTestData` cria colaborador e disputa cujos modelos têm `auto_now_add` em `criado_em` e `aberta_em`. Esses campos não são os marcos consultados pela função inspecionada. ([test_f3_lavra_foto_sem_decisao.py:271](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:271), [colaboradores/models.py:380](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/colaboradores/models.py:380), [chamados/models.py:1893](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/models.py:1893), [veredito_lavrado.py:177](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:177)) Os `subTest` envolvem assertivas e não há `try` que as engula no teste novo. ([test_f3_lavra_foto_sem_decisao.py:308](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:308), [test_f3_lavra_foto_sem_decisao.py:316](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:316))

4. **BLOQUEIA — premissa temporal da lavra.** `marcos_do_lote` usa o máximo de `respondida_em`, cortado por `ate`; `esta_stale` só considera esse marco quando ele é posterior a `julgado_em`. ([veredito_lavrado.py:201](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:201), [veredito_lavrado.py:230](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:230)) Já a cura pela trilha pode escrever **agora** um `respondida_em` antigo, correspondente à nota, e registrar uma decisão com esse mesmo instante antigo. ([disputa_emissao.py:1692](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/disputa_emissao.py:1692), [curar_perguntas_em_limbo.py:68](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/management/commands/curar_perguntas_em_limbo.py:68))

   Consequência demonstrável pelo código: se o instante da nota for anterior à lavra, a escrita posterior não gera, por esse campo, um marco maior que `julgado_em`; excluir a decisão também impede que sua inserção mude os marcos. Isso contradiz a premissa de `esta_stale` de detectar insumo alterado depois da lavra e afeta o consumidor da tarefa, que confia na lavra quando ela não está stale. ([veredito_lavrado.py:213](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:213), [tarefa_do_dia.py:433](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/tarefa_do_dia.py:433)) Ocorrência e impacto em produção: **[não verificado]**.

   Aqui `respondida_em` representa o instante atribuído ao ato, mas é também usado como indicador de quando um insumo mudou para fins de invalidação; esses instantes podem divergir numa cura retroativa. A regra desejada para corrigir essa divergência é uma **pergunta de lei**, pois o brief exclui alterar o leitor e o carimbo. ([disputa_emissao.py:1693](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/disputa_emissao.py:1693), [veredito_lavrado.py:213](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:213); escopo do brief fornecido: **[não verificado em arquivo local]**)

5. **NÃO BLOQUEIA — censo dos caminhos principais e derivação paralela.** `marcos_do_lote` deriva os marcos de batidas, células e perguntas; não grava esses insumos. `esta_stale`, `fio`, `raiox`, `tarefa_do_dia` e o auditor consomem os marcos. ([veredito_lavrado.py:151](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:151), [fio.py:282](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/fio.py:282), [raiox.py:38](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/raiox.py:38), [tarefa_do_dia.py:435](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/tarefa_do_dia.py:435), [auditar_invariantes_chamados.py:1174](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/management/commands/auditar_invariantes_chamados.py:1174))

   A resposta humana grava `respondida_em` e registra a decisão com o mesmo instante; a resposta automática e a cura pela trilha também escrevem ambos. `DecisaoPergunta.registrar` grava a sequência; `DecisaoPergunta.leitura` lê as decisões e incorpora o passivo da projeção. ([disputa_emissao.py:1527](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/disputa_emissao.py:1527), [disputa_emissao.py:1681](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/disputa_emissao.py:1681), [disputa_emissao.py:1705](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/disputa_emissao.py:1705), [models.py:4295](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/models.py:4295), [models.py:4329](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/models.py:4329)) O novo teste não introduz derivação paralela em produção; a duplicação de lógica de stale já existente em `raiox` está declarada no próprio código. ([raiox.py:20](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/raiox.py:20), [raiox.py:39](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/raiox.py:39))

Execução da suíte, logs de RED e resultados em banco: **[não verificado]**. [nao verificado]

BLOQUEIA: 2  
perguntas de lei: 1

## 19/11 02:18 · sessao 3ac06bef2621 · - · - · origem=Ronald (tmux)

Achados enviados ao agente principal, com rotas, serviços, permissões, filtros e limites de verificação dos universos. [app/chamados/views.py:719] [app/chamados/views.py:770] [app/chamados/services/painel.py:198]

## 19/11 02:18 · sessao bfe32ac9cfed · - · - · origem=Ronald (tmux)

- **Tarefa do dia:** `tarefa_do_dia` é um serviço que recebe colaborador, dia do turno e instante; não comprovei uma rota de tela correspondente. [app/chamados/services/tarefa_do_dia.py:738] A equivalência com “Meu Atendimento” é [não verificado]; essa rota chama `montar_chamados_fio` e pode registrar auditoria no GET. [app/chamados/urls.py:16] [app/chamados/views.py:566] [app/chamados/views.py:583]
- **Consulta do app:** `/api/me/` chama `api_me`, exige autenticação e usa `request.user.colaborador`. [app/config/urls.py:37] [app/api/urls.py:64] [app/api/views.py:282] Entre os serviços chamados estão `turno_aberto_de`, `proximo_tipo_de` e `projecao_do_app`. [app/api/views.py:306] [app/api/views.py:308] [app/api/views.py:315] A resposta trata de um colaborador e contém seu nome, por isso não deve ser impressa pela sonda. [app/api/views.py:336]

## 19/11 02:18 · sessao e37c8a1bfe88 · ESPELHO-VERDADE · sonda · origem=Code

A lista nominal por PK e os totais **não estão no dado raspado**; são **[não verificado]**. O anexo contém o molde e uma referência a outra sonda, mas não contém a saída desta apuração. O script abaixo é entregue como texto e não foi executado. [não verificado]

**A) Caminho até o dono**

- O comando percorre empresas ativas e colaboradores com `FechamentoMensal` na competência pedida; `--so-txt` é um filtro opcional, que a sonda abaixo não aplica. A janela é calculada por empresa. [app/ponto/management/commands/e6_oraculo.py:303](app/ponto/management/commands/e6_oraculo.py#L303)
- Para cada colaborador, obtém o espelho e constrói `InsumosDoDono`. Percorre os dias da janela e exclui os sem linha no espelho, isentos ou com ausência, sem minutos lavrados, com zero em ambos e aqueles cuja diferença é a janela declarada. Os minutos do espelho vêm primeiro de `minutos_realizados`, depois de `pago_h`; se ambos faltam, não há comparação. [app/ponto/management/commands/e6_oraculo.py:314](app/ponto/management/commands/e6_oraculo.py#L314), [app/ponto/management/commands/e6_oraculo.py:263](app/ponto/management/commands/e6_oraculo.py#L263)
- Entre os dias comparados, só os com diferença absoluta **maior que 10 minutos** chegam a `_ins.dono(dia)`. Assim, “universo avaliado por dono” abaixo significa dias divergentes que chegaram ao juiz, não todos os dias comparados. [app/ponto/management/commands/e6_oraculo.py:362](app/ponto/management/commands/e6_oraculo.py#L362)
- `horas_do_dia` vem de `minutos_do_oraculo` sobre timestamps locais de `batidas_apuraveis`, numa janela de 12 horas antes do início até 14 horas depois do dia seguinte ao fim; `dono()` passa as horas atribuídas à data. `em_cadastro_x_realidade` é a pertinência da data ao conjunto extraído de `dias_cadastro_x_realidade` do resultado do espelho. [app/ponto/management/commands/e6_oraculo.py:149](app/ponto/management/commands/e6_oraculo.py#L149), [app/ponto/management/commands/e6_oraculo.py:162](app/ponto/management/commands/e6_oraculo.py#L162), [app/ponto/management/commands/e6_oraculo.py:182](app/ponto/management/commands/e6_oraculo.py#L182)
- As três flags de cadastro vêm de `cadastro_do_dia_carregado`: ele consulta o vínculo do dia e, quando não existe, também a célula e a data de demissão; com vínculo, consulta `defeito_folga_nao_definida`. [app/ponto/supra_juiz.py:179](app/ponto/supra_juiz.py#L179), [app/ponto/supra_juiz.py:194](app/ponto/supra_juiz.py#L194)
- `batida_fora_de_turno` usa `realizado_do_dia` e exige turno aberto com entrada solta presente entre as batidas apuráveis; os turnos são carregados para a janela ampliada em um dia de cada lado. `fase_deslocada` vem da pertinência da data aos dias retornados por `dias_de_fase_as_of`. [app/ponto/management/commands/e6_oraculo.py:156](app/ponto/management/commands/e6_oraculo.py#L156), [app/ponto/management/commands/e6_oraculo.py:174](app/ponto/management/commands/e6_oraculo.py#L174), [app/ponto/management/commands/e6_oraculo.py:198](app/ponto/management/commands/e6_oraculo.py#L198)
- Há **uma única cláusula executável que devolve ESTRUTURA**: o lado falso do retorno condicional em [app/ponto/management/commands/e6_oraculo.py:110](app/ponto/management/commands/e6_oraculo.py#L110). Para alcançá-la, nenhum dos retornos anteriores de cadastro, batida ou fase pode ter ocorrido, e `em_cadastro_x_realidade` deve ser falso. [app/ponto/management/commands/e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py#L98)

**B) Sonda**

O juiz devolve só o dono. A sonda registra, com `sys.settrace`, a linha de retorno efetivamente executada pela função real; para `ESTRUTURA`, exige a linha 110 verificada nesta cópia e falha se o código mudou. Não reconstrói as condições do juiz. [app/ponto/management/commands/e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py#L98)

Para cada empresa, usa a janela que contém a data do ato como corrente e exige que ela não esteja trancada. Define “última fechada” como a competência anterior mais recente com tranca viva; a autoridade do código define fechada como trancada e usa `reaberto_em` para identificar a tranca viva. Se faltar uma dessas condições, a sonda falha e não apresenta contagens parciais. [app/ponto/janelas.py:60](app/ponto/janelas.py#L60), [app/ponto/services/fechamento.py:1464](app/ponto/services/fechamento.py#L1464)

Executar no mesmo envelope Django de leitura do anexo, com a árvore montada em leitura e o processo limitado ao cpuset de teste; a sonda também verifica sua afinidade. A fonte de recursos declara por padrão CPUs 4–7 para teste. [bin/sombra.sh:182](bin/sombra.sh#L182), [bin/recursos.sh:37](bin/recursos.sh#L37)

```python
# Sonda como texto: executar no envelope Django da sombra, tenant juliani.
# Executar Python com -B e processo no cpuset de teste. [nao verificado]
# Não imprime nome nem CPF. Nenhum arquivo é criado por este código.

import sys
sys.dont_write_bytecode = True

import collections
import datetime
import os

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Empresa, Colaborador
from ponto.models import FechamentoMensal, PeriodoFechado
from ponto import janelas
from ponto.services.espelho import espelho_do_colab
from ponto.services.fechamento import competencia_trancada
from ponto.management.commands import e6_oraculo as e6


DONOS = ("ESTRUTURA", "CADASTRO", "BATIDA")
ARQUIVO_JUIZ = "app/ponto/management/commands/e6_oraculo.py"
LINHA_ESTRUTURA = 110


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")

    # TESTE_CPUSET pode ser fornecido pelo envelope a partir de recursos.sh.
    especificacao = os.environ.get("TESTE_CPUSET", "4-7")
    permitidas = set()
    for parte in especificacao.split(","):
        limites = parte.strip().split("-")
        if len(limites) == 1:
            permitidas.add(int(limites[0]))
        elif len(limites) == 2:
            inicio, fim = map(int, limites)
            if inicio > fim:
                raise RuntimeError("cpuset_invalido")
            permitidas.update(range(inicio, fim + 1))
        else:
            raise RuntimeError("cpuset_invalido")
    afinidade = os.sched_getaffinity(0)
    if not afinidade or not afinidade.issubset(permitidas):
        raise RuntimeError("cpuset")

    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")

    with connection.cursor() as cursor:
        cursor.execute(
            "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ READ ONLY"
        )
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def dono_e_linha(insumos, dia):
    # Observa o retorno da função real chamada por InsumosDoDono.dono().
    if sys.gettrace() is not None:
        raise RuntimeError("trace_preexistente")

    retornos = []

    def observar(frame, evento, argumento):
        if frame.f_code is e6.dono_da_divergencia.__code__:
            if evento == "return":
                retornos.append((frame.f_lineno, argumento))
            return observar
        return observar

    try:
        sys.settrace(observar)
        dono = insumos.dono(dia)
    finally:
        sys.settrace(None)

    # Com o espelho já fornecido, dono() deve chamar o juiz uma vez.
    if len(retornos) != 1 or retornos[0][1] != dono:
        raise RuntimeError("retorno_do_juiz")
    if dono not in DONOS:
        raise RuntimeError("dono_desconhecido")

    linha = retornos[0][0]
    if dono == "ESTRUTURA" and linha != LINHA_ESTRUTURA:
        raise RuntimeError("clausula_mudou")
    return dono, linha


def main():
    etapa = "GUARDA"
    try:
        linhas = []
        contagem_donos = collections.Counter()
        por_clausula = collections.Counter()
        por_empresa = collections.Counter()
        universo = 0

        with transaction.atomic():
            guarda_sombra()
            agora = timezone.now()
            hoje = timezone.localtime(agora).date()

            etapa = "COMPETENCIAS"
            empresas = list(Empresa.objects.filter(ativa=True).order_by("pk"))

            for empresa in empresas:
                ini_atual, fim_atual = janelas.janela_atual(hoje, empresa)
                atual = (fim_atual.year, fim_atual.month)

                if competencia_trancada(empresa.pk, atual[1], atual[0]):
                    raise RuntimeError("corrente_trancada")

                # A ordem ano/mês identifica a competência mais recente,
                # anterior à corrente, cuja tranca não foi reaberta.
                trancas = PeriodoFechado.objects.filter(
                    empresa=empresa, reaberto_em__isnull=True
                ).order_by("-ano", "-mes")
                fechada = next(
                    (
                        (p.ano, p.mes)
                        for p in trancas
                        if (p.ano, p.mes) < atual
                    ),
                    None,
                )
                if fechada is None:
                    raise RuntimeError("sem_fechada_anterior")

                for tipo, (ano, mes) in (
                    ("corrente", atual),
                    ("ultima_fechada", fechada),
                ):
                    etapa = "JANELA"
                    ini, fim = janelas.janela_fechamento(mes, ano, empresa)
                    competencia = "%04d-%02d" % (ano, mes)
                    linhas.append(
                        "ESCOPO empresa=%s tipo=%s competencia=%s inicio=%s fim=%s"
                        % (empresa.pk, tipo, competencia, ini, fim)
                    )

                    ids = FechamentoMensal.objects.filter(
                        mes=mes, ano=ano, colaborador__empresa=empresa
                    ).values_list("colaborador_id", flat=True)

                    colaboradores = Colaborador.objects.filter(
                        pk__in=ids
                    ).order_by("pk")

                    for colab in colaboradores:
                        etapa = "ESPELHO"
                        espelho = espelho_do_colab(
                            colab, hoje, ini, fim, agora=agora
                        )
                        dias_espelho = {
                            str(item.get("data"))[:10]: item
                            for item in (espelho.get("dias") or [])
                        }

                        etapa = "INSUMOS"
                        insumos = e6.InsumosDoDono(
                            colab, ini, fim, hoje,
                            espelho=espelho, agora=agora
                        )
                        minutos = insumos.oraculo.minutos

                        dia = ini
                        while dia <= fim:
                            chave = str(dia)
                            registro = dias_espelho.get(chave)

                            if registro is not None:
                                if not (
                                    registro.get("isencao")
                                    or registro.get("ausencia") is not None
                                ):
                                    m_esp, _ = e6.minutos_do_espelho(registro)
                                    if m_esp is not None:
                                        m_e6 = minutos.get(chave, 0.0)
                                        if not (m_esp == 0 and m_e6 == 0):
                                            diferenca = m_e6 - m_esp
                                            if not e6.diferenca_e_a_janela_declarada(
                                                diferenca, registro
                                            ):
                                                if abs(diferenca) > 10:
                                                    etapa = "DONO"
                                                    dono, linha = dono_e_linha(
                                                        insumos, dia
                                                    )
                                                    universo += 1
                                                    contagem_donos[dono] += 1

                                                    if dono == "ESTRUTURA":
                                                        clausula = "%s:%d" % (
                                                            ARQUIVO_JUIZ, linha
                                                        )
                                                        por_clausula[clausula] += 1
                                                        por_empresa[empresa.pk] += 1
                                                        linhas.append(
                                                            "ESTRUTURA %s colab_pk=%s "
                                                            "empresa=%s dia=%s clausula=%s"
                                                            % (
                                                                competencia,
                                                                colab.pk,
                                                                empresa.pk,
                                                                chave,
                                                                clausula,
                                                            )
                                                        )
                            dia += datetime.timedelta(days=1)

            etapa = "CONFERENCIA"
            contador = sum(contagem_donos[dono] for dono in DONOS)
            if contador != universo:
                raise RuntimeError("soma_donos")

            for clausula, quantidade in sorted(por_clausula.items()):
                linhas.append(
                    "RESUMO_CLAUSULA clausula=%s total=%d"
                    % (clausula, quantidade)
                )
            for empresa in empresas:
                linhas.append(
                    "RESUMO_EMPRESA empresa=%s estrutura=%d"
                    % (empresa.pk, por_empresa[empresa.pk])
                )
            for dono in DONOS:
                linhas.append(
                    "RESUMO_DONO dono=%s total=%d"
                    % (dono, contagem_donos[dono])
                )
            linhas.append(
                "CONFERENCIA contador=%d universo_avaliado=%d "
                "contador==universo=%s"
                % (contador, universo, "s" if contador == universo else "n")
            )

        for linha in linhas:
            print(linha)
        print("ERROS=0")
        print("FIM_SONDA_EV_V1")
        return 0

    except Exception:
        print("ERRO etapa=%s" % etapa)
        print("ERROS=1")
        print("FIM_SONDA_EV_V1")
        return 1


sys.exit(main())
```

A execução, a disponibilidade das duas competências para cada empresa e a saída sem efeitos indiretos das funções chamadas permanecem **[não verificado]**. A transação `READ ONLY` faz a execução falhar caso uma dessas funções tente escrever no banco; os demais efeitos indiretos não foram comprovados. [não verificado]

perguntas de lei: 0

## 19/11 02:18 · sessao 0761ec828ae3 · HOOK-B3-PAREI-LEGITIMO · leitura · origem=Code

O pré-desenho encontra uma lacuna de representação: **B3 parado e fila sem outro item andável não bastam, por si, para o hook liberar o Stop**. A regra 7b exige `!` no topo ou ESPERANDO provado; a parada da obra por desenho não aparece como terceira porta explícita. Assim, a liberação sem uma dessas portas depende de uma **pergunta de lei**, não de uma conclusão técnica já demonstrada. [CLAUDE.md:262] [CLAUDE.md:269] [CLAUDE.md:274] [CLAUDE.md:294] [bin/hook_stop_fila1.py:1381]

A igualdade da cópia com o hash informado permanece **[não verificado]**: `git rev-parse HEAD` falhou nesta leitura. A ocorrência concreta e o texto do PAREI, a lista de andáveis e o processo detectado nos eventos raspados também permanecem **[não verificado]**. [não verificado]

### A) Matriz

Cada linha abaixo representa todas as combinações dos campos marcados `*`, inclusive B3 sim e não. Isso cobre as 32 combinações pedidas sem atribuir ao B3 um efeito que o hook não consulta nessa porta. `P=sim` significa apenas que o PAREI nomeia o item; **não prova** que contém `!` ou passa os demais filtros. `E=sim` inclui o censo provado reconhecido pelo hook. [bin/hook_stop_fila1.py:1232] [bin/hook_stop_fila1.py:1260] [bin/hook_stop_fila1.py:1275] [bin/hook_stop_fila1.py:1280] [bin/hook_stop_fila1.py:1286] [bin/hook_stop_fila1.py:798]

A matriz considera o **caminho normal**, com item aberto e marcador válido, sem decisão anterior de prova do RELATO, B4 ou orçamento; essas portas têm precedência. Exceção posterior pode liberar por falha aberta. Sem essas condições adicionais, a decisão final de uma combinação é **[não verificado]**. [bin/hook_stop_fila1.py:1349] [bin/hook_stop_fila1.py:1361] [bin/hook_stop_fila1.py:1372] [bin/hook_stop_fila1.py:1400] [bin/hook_stop_fila1.py:1460]

| B3 | Outro andável A | Suite/push S | PAREI nomeia P | ESPERANDO provado E | Decisão atual, nas condições acima | Desejável segundo 7b | [nao verificado]
|---|---|---|---|---|---|---|
| * | * | * | * | sim | **ALLOW** por PAREI válido, se essa porta vier primeiro; senão por ESPERANDO. [bin/hook_stop_fila1.py:1381] [bin/hook_stop_fila1.py:1386] | **ALLOW** por ESPERANDO provado. [CLAUDE.md:262] |
| * | não | * | sim | não | **ALLOW** se o PAREI passar `!` e os demais filtros; caso contrário, aplicar linhas abaixo para S. [bin/hook_stop_fila1.py:1243] [bin/hook_stop_fila1.py:1263] [bin/hook_stop_fila1.py:1275] [bin/hook_stop_fila1.py:1310] | **ALLOW** se a trava for `!` aplicável; sem isso, **siga**, ressalvada a pergunta de lei abaixo. [CLAUDE.md:262] [CLAUDE.md:265] |
| * | sim | sim | sim | não | **BLOCK**: PAREI não libera com outro andável; espera impede teto e B2. [bin/hook_stop_fila1.py:1245] [bin/hook_stop_fila1.py:1427] [bin/hook_stop_fila1.py:1450] [bin/hook_stop_fila1.py:1459] | **Siga** para outro item, mesmo com a obra parada. [CLAUDE.md:274] [CLAUDE.md:296] |
| * | sim | não | sim | não | **BLOCK**, salvo liberação por teto ou B2. [bin/hook_stop_fila1.py:1245] [bin/hook_stop_fila1.py:1439] [bin/hook_stop_fila1.py:1450] | **Siga** para outro item. [CLAUDE.md:274] [CLAUDE.md:296] |
| * | * | sim | não | não | **BLOCK**: não há PAREI genérico aplicável; espera impede teto e B2. [bin/hook_stop_fila1.py:1243] [bin/hook_stop_fila1.py:1427] [bin/hook_stop_fila1.py:1450] [bin/hook_stop_fila1.py:1459] | **Siga** pela redação literal do item 1; se B3=sim e A=não, eventual exceção é **[não verificado]**. [CLAUDE.md:262] [CLAUDE.md:274] |
| * | * | não | não | não | **BLOCK**, salvo teto ou B2. [bin/hook_stop_fila1.py:1243] [bin/hook_stop_fila1.py:1439] [bin/hook_stop_fila1.py:1450] | **Siga** pela redação literal do item 1; se B3=sim e A=não, eventual exceção é **[não verificado]**. [CLAUDE.md:262] [CLAUDE.md:274] |

Na segunda linha, quando o PAREI não passa os filtros, o desfecho é **BLOCK** com S=sim ou **BLOCK/ALLOW pelo teto ou B2** com S=não; não se pode escolher entre esses resultados sem o estado do contador e do B2. [bin/hook_stop_fila1.py:1427] [bin/hook_stop_fila1.py:1439] [bin/hook_stop_fila1.py:1450]

Há uma tensão que a matriz não deve esconder: a redação de 7b diz “tudo o mais é siga”, mas o próprio código conserva teto e B2 como saídas de armadilha. Se essas saídas constituem exceções normativas ao item 1 para cada estado da matriz, isso é **[não verificado]**. [CLAUDE.md:262] [bin/hook_stop_fila1.py:1439] [bin/hook_stop_fila1.py:1450]

### B) Células divergentes e opções de mudança

**Divergência demonstrada sob leitura literal:** nas linhas com E=não, S=não e sem PAREI válido, o teto ou B2 **podem liberar** embora 7b diga `siga`; o mesmo ocorre com outro andável. Não se demonstra que ocorreu em qualquer chamada concreta. [CLAUDE.md:262] [bin/hook_stop_fila1.py:1439] [bin/hook_stop_fila1.py:1450]

**Célula motivadora:** B3=sim, A=não, E=não, sem PAREI válido e S=sim resulta em **BLOCK** no caminho normal. Classificar esse BLOCK como erro de lei e desejar ALLOW é **[não verificado]**: 7b manda parar a obra, mas só explicita `!` e ESPERANDO provado como portas de devolução. Com S=não, o resultado adicionalmente depende de teto/B2. [CLAUDE.md:262] [CLAUDE.md:274] [bin/hook_stop_fila1.py:1427] [bin/hook_stop_fila1.py:1450]

Opções para decisão, sem patch:

1. **Usar a porta existente de `!`:** verificar se o aval pendente do B3 pode ser representado como PAREI com `!`, ID inteiro e sem outro andável. O menor ponto de reconhecimento é `_parei_do_em_curso`; `_relato_parou` mantém a guarda dos outros itens. Se a forma já satisfizer os filtros, nenhuma mudança de código é demonstrada necessária. Se `!` nesse caso é a expressão autorizada pela lei é **[não verificado]**. [bin/hook_stop_fila1.py:1243] [bin/hook_stop_fila1.py:1260] [bin/hook_stop_fila1.py:1275] [bin/hook_stop_fila1.py:1310] [CLAUDE.md:294]

2. **Nova porta específica de B3**, somente se houver corte de lei: ponto de decisão mínimo em `main`, junto às portas de PAREI/ESPERANDO, preservando a prova anterior e a guarda de outro andável. Seria necessário definir como provar B3 ativo, associá-lo ao item em curso e reconhecer que não há outra obra andável; a suficiência de qualquer fonte para isso é **[não verificado]**. A leitura de `linhas_libera_b3` não basta como prova de B3 ativo: ela reconhece candidatas de linha, enquanto `decide_b3` também examina estado, fotografia, consumo e tempo. [bin/hook_stop_fila1.py:1353] [bin/hook_stop_fila1.py:1381] [bin/hook_stop_fila1.py:1386] [bin/disjuntor.py:728] [bin/disjuntor.py:1024] [bin/disjuntor.py:1029]

3. **Ajustar o recado de bloqueio**, sem criar porta: o ponto menor é a escolha de `item`/recado em `main`, para não mandar avançar uma obra comprovadamente parada em B3. Isso não resolve a devolução do turno; o predicado confiável para o recado é **[não verificado]**. [bin/hook_stop_fila1.py:1391] [bin/hook_stop_fila1.py:1453]

4. **Rever teto/B2**, caso a pergunta de lei determine que não podem liberar nas células identificadas: os menores pontos de decisão são as condições em `main` para teto e B2. Desligá-los genericamente teria alcance maior que a célula B3 e alteraria válvulas existentes. [bin/hook_stop_fila1.py:1439] [bin/hook_stop_fila1.py:1450]

### C) Leitores e alcance

- **Topo compartilhado:** `_topo` recorta faixa de censo e duas linhas úteis; `_linha_do_topo` entrega a primeira ao PAREI, e `_esperando_com_prova` lê o mesmo recorte. O classificador `_util_do_topo` também é usado por `painel_do_topo`, chamado pelo escritor do censo. Alterar recorte ou normalização afeta mais que a válvula. [bin/hook_stop_fila1.py:413] [bin/hook_stop_fila1.py:467] [bin/hook_stop_fila1.py:495] [bin/hook_stop_fila1.py:507] [bin/hook_stop_fila1.py:798] [bin/disjuntor.py:328]
- **B3:** `_topo_consistente` passa o recorte compartilhado a `linhas_libera_b3`; o reconhecedor aceita formas específicas de `DESENHO` e `ERRO-BOBO`, e `decide_b3` aplica verificações adicionais antes de liberar. Alterar o topo pode mudar o que o B3 enxerga. [bin/disjuntor.py:728] [bin/disjuntor.py:764] [bin/disjuntor.py:788] [bin/disjuntor.py:1029]
- **Outras portas do Stop:** orçamento consulta a linha e `_relato_parou`; B4 recebe a linha do topo; o próprio `main` consulta novamente `_parei_do_em_curso` para o recado. Alterar o juiz compartilhado pode mudar esses consumidores, mesmo sem alterar `main`. [bin/hook_stop_fila1.py:1064] [bin/hook_stop_fila1.py:1146] [bin/hook_stop_fila1.py:1181] [bin/hook_stop_fila1.py:1200] [bin/hook_stop_fila1.py:1392]
- **Handoff e alarme:** o handoff chama os dois juízes de PAREI e consulta faixa/linha para prova de censo; o alarme lê PAREI nas primeiras 40 linhas e chama o reconhecedor de censo. [bin/handoff_sessao.sh:89] [bin/handoff_sessao.sh:121] [bin/handoff_sessao.sh:123] [bin/handoff_sessao.sh:129] [bin/alarme_sessao_ociosa.py:120] [bin/alarme_sessao_ociosa.py:135]
- Outros leitores que uma mudança concreta poderia alcançar: **[não verificado]**. [não verificado]

### D) Selos e RED faltante

Os selos foram **lidos, não executados**; sua passagem nesta cópia é **[não verificado]**. Os scripts consultados criam e removem fixtures, incompatíveis com a instrução de somente leitura. [bin/tests/test_hook_parei_com_andavel.sh:15] [bin/tests/test_hook_parei_so_lei.sh:34]

| Opção | Selos existentes relevantes | Caso RED a especificar |
|---|---|---|
| 1 — porta `!` | Decisão técnica e lei sem `!` bloqueiam; `!` do item libera. O selo de andável exige bloqueio com outro item livre e liberação quando os demais estão travados; os selos de ID e item em curso protegem identidade e SONDA. [bin/tests/test_hook_parei_so_lei.sh:40] [bin/tests/test_hook_parei_so_lei.sh:48] [bin/tests/test_hook_parei_so_lei.sh:59] [bin/tests/test_hook_parei_com_andavel.sh:45] [bin/tests/test_hook_parei_com_andavel.sh:55] [bin/tests/test_hook_parei_casa_id_inteiro.sh:56] [bin/tests/test_hook_parei_trava_o_item_em_curso.sh:71] | B3 ativo da obra em curso, todos os demais comprovadamente não andáveis e PAREI no formato de `!` que a lei autorizar: exigir ALLOW; par com outro andável exige BLOCK. Se a forma já for aceita, o primeiro caso pode já estar verde; isso é **[não verificado]**. [bin/hook_stop_fila1.py:1243] | [nao verificado]
| 2 — nova porta B3 | O selo do disjuntor verifica forma, posição, idade e consumo da linha B3; os selos de PAREI, andável e ESPERANDO protegem as portas atuais. [bin/tests/test_disjuntor.sh:359] [bin/tests/test_disjuntor.sh:373] [bin/tests/test_disjuntor.sh:391] [bin/tests/test_hook_parei_com_andavel.sh:45] [bin/tests/test_hook_esperando_com_prova.sh:94] | **Somente após corte de lei:** B3 ativo e associado ao item, demais não andáveis, sem `!` e sem ESPERANDO provado, suite/push em curso → ALLOW pela nova porta; pares exigem BLOCK para outro andável, B3 ausente, obra diferente ou prova de B3 insuficiente. A expectativa ALLOW hoje é **[não verificado]**. [CLAUDE.md:262] [bin/hook_stop_fila1.py:1427] | [nao verificado]
| 3 — recado | O selo de andável verifica que o recado não aponta o item travado e menciona o outro; o selo de processo exige que o bloqueio nomeie item livre. [bin/tests/test_hook_parei_com_andavel.sh:51] [bin/tests/test_hook_nao_para_por_processo.sh:47] | B3 ativo, nenhum outro andável, BLOCK mantido: recado identifica a trava e não manda construir a obra parada; par com outro andável aponta esse outro. Formulação e fonte da prova B3 são **[não verificado]**. [CLAUDE.md:274] |
| 4 — teto/B2 | O selo de espera exige bloqueio durante suite e liberação do teto sem espera; o selo de processo protege item livre com suite. [bin/tests/test_hook_teto_nao_conta_espera.sh:61] [bin/tests/test_hook_teto_nao_conta_espera.sh:95] [bin/tests/test_hook_nao_para_por_processo.sh:47] | Após corte de lei, contador acima do teto e B2 disparado, com cada combinação de B3 e outro andável pertinente: afirmar a decisão escolhida e preservar o caso de controle que a lei mantiver. A decisão esperada é **[não verificado]**. [bin/hook_stop_fila1.py:1439] [bin/hook_stop_fila1.py:1450] | [nao verificado]

**perguntas de lei: 2**

1. Com B3 ativo, nenhum outro item andável, sem `!` aplicável e sem ESPERANDO provado, a decisão é: **(a)** manter `siga`; **(b)** criar porta específica de devolução; ou **(c)** outra forma literal? A necessidade de corte decorre da porta limitada do item 1 e da parada da obra no item 4. [CLAUDE.md:262] [CLAUDE.md:269] [CLAUDE.md:274]
2. O teto e B2 continuam podendo liberar com item aberto e sem as portas do item 1: **(a)** sim, como exceções de armadilha; **(b)** não; ou **(c)** apenas sob condições a definir? [CLAUDE.md:262] [bin/hook_stop_fila1.py:1439] [bin/hook_stop_fila1.py:1450]

**regras de pista que a valvula muda: N = [não verificado]**. Para a opção 1, nenhuma mudança é demonstrada se a forma já passar; para a opção 2, a regra nova e seu nome dependem do corte; para a opção 4, a candidata explícita é `HOOK-TETO-NAO-CONTA-ESPERA`. `HOOK-PAREI-COM-ANDAVEL` permanece preservada nas opções acima se a guarda de outro andável for mantida. [bin/hook_stop_fila1.py:1232] [bin/hook_stop_fila1.py:1418]

## 19/11 02:16 · sessao 377e7316361b · ESCALA-NAO-DESCREVE · verificador · origem=Code

**BLOQUEIA — a união semântica dos pais não está comprovada.** A cópia não dispõe dos objetos Git dos pais nem da base comum; os anexos fornecem apenas os diffs da catraca e a lista de `.py` de produção trazidos sobre o comboio. Portanto, não é possível concluir que o merge automático de `app/docs/LEIS.md` e dos arquivos de produção preservou a união dos dois lados, nem enumerar com segurança todos os arquivos tocados por ambos. **[não verificado]**

1. **Catraca — NÃO BLOQUEIA.** Executei `censo('app')` em leitura e comparei, chave a chave, os dicionários construídos como no teste: 9 entradas de `pareadores` e 85 de `derivacoes_de_dia`; igualdade total e nenhum arquivo ilegível. A entrada do censo lê e analisa os arquivos de produção em [medida_turno_do_par.py:448](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/medida_turno_do_par.py:448); o teste define a comparação em [test_medida_turno_do_par.py:558](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/tests/test_medida_turno_do_par.py:558). O argumento `app` corresponde à pasta que `censo` espera; a chamada literal `censo('.')`, com diretório corrente `app`, não foi executada. **[não verificado]**

   Os dois valores alterados contra o comboio estão registrados em [test_medida_turno_do_par.py:280](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/tests/test_medida_turno_do_par.py:280) e [test_medida_turno_do_par.py:323](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/tests/test_medida_turno_do_par.py:323); os sítios são [regua_defesa.py:524](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/regua_defesa.py:524), [esmeril_espelho.py:179](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:179) e [esmeril_espelho.py:186](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:186). A igualdade foi verificada; se os números foram efetivamente gerados pelo medidor, sem digitação, é **[não verificado]**.

2. **Composição dos pais — BLOQUEIA, por falta de prova, não por defeito semântico demonstrado.** O caso mais relevante para revisão é `app/escala/services/cadastro_realidade.py`: a árvore contém a montagem da lista da ESCALA em [cadastro_realidade.py:960](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:960) e código identificado como F6 em [cadastro_realidade.py:1076](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:1076). Isso mostra coexistência na cópia, mas não prova quais alterações cada pai fez nem se a composição preservou integralmente sua semântica. Para esse arquivo, para `LEIS.md` e para os demais arquivos potencialmente tocados pelos dois lados, composição por arquivo: **[não verificado]**.

   A afirmação de que nenhum `.py` de produção mudou *na resolução do conflito*, e a identificação exaustiva de combinações que nenhum pai tinha, também permanecem **[não verificado]**.

3. **Testes da ESCALA — NÃO BLOQUEIA pelo que foi observado.** Em [test_escala_nao_descreve.py](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/tests/test_escala_nao_descreve.py:1), não identifiquei asserção que dependa de número de linha; há, por exemplo, asserções sobre a ordem e os valores da lista em [test_escala_nao_descreve.py:194](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/tests/test_escala_nao_descreve.py:194) e sobre a trilha do vínculo em [test_escala_nao_descreve.py:275](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/tests/test_escala_nao_descreve.py:275). Se algum comportamento desses testes foi alterado pelo comboio, e se há outros testes da ESCALA pertinentes fora desse arquivo, permanece **[não verificado]**. Os testes Django não foram executados. **[não verificado]**

4. **Escritores e leitores observáveis — NÃO BLOQUEIA quanto ao rastreamento abaixo; completude do delta, [não verificado].**

   - `medida['orfas']`: `assinaturas_do_colab` conta as órfãs e escreve a medida em [esmeril_espelho.py:119](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:119); `anomalias_da_frota` a lê e produz `orfas` e `orfas_desde` em [esmeril_espelho.py:233](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:233).
   - `proposta`: `anomalias_da_frota` a produz em [esmeril_espelho.py:241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:241). `lavrar` entrega o retrato a `gravar_lavra`, que o persiste em `detalhe`, e `ler_lavra` o recupera em [esmeril_espelho.py:321](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:321). `lista` lê os três mapas em [cadastro_realidade.py:960](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:960); a ação também lê a proposta em [views.py:493](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/views.py:493).
   - `carga`, `carga_desde`, `carga_texto`, `linhas` e `carga_resumo`: `lista` monta os campos em [cadastro_realidade.py:981](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:981) e ordena e publica a lista e o resumo em [cadastro_realidade.py:1053](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:1053). A tela os lê em [cadastro_x_realidade.html:17](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/templates/escala/cadastro_x_realidade.html:17) e o PDF em [cadastro_realidade.py:1183](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:1183).
   - `botao_da_proposta` é calculado em [cadastro_realidade.py:830](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:830), lido pela lista em [cadastro_realidade.py:987](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:987) e novamente pela porta em [views.py:498](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/views.py:498). `folga_trabalhada_no_6x1` é usado tanto pela ação da assinatura quanto pelo botão em [cadastro_realidade.py:717](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:717) e [cadastro_realidade.py:845](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:845).
   - A view fornece `motivo` a `realocar_vinculo_para_destino` em [views.py:502](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/views.py:502); a função o passa como `confirm_txt` em [regua_defesa.py:316](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/regua_defesa.py:316); a porta o lê no veredito e o encaminha à observação da trilha em [vinculo.py:144](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/colaboradores/services/vinculo.py:144) e [vinculo.py:349](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/colaboradores/services/vinculo.py:349).

Nenhuma questão de interpretação de lei foi demonstrada pelos elementos examinados; a pendência é de evidência da composição. **[não verificado]**

BLOQUEIA: 1  
perguntas de lei: 0

## 19/11 02:09 · sessao 0e79919b9af3 · E4C-F3-LAVRA-FOTO · verificador · origem=Code

**NAO BLOQUEIA — a camada SQL fecha os cinco caminhos exercitados pelos mutantes no contexto da fixture**, somando-se à AST que já existia. Isso não prova a ausência de toda leitura possível de `DecisaoPergunta`: a garantia dinâmica só vale para as execuções capturadas, e a própria docstring declara o resíduo dos ramos não executados que a AST não reconhece. [teste:3](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:3), [teste:312](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:312)

1. **Mutantes e escapes da v2 — NAO BLOQUEIA.** O anexo `mutantes_c3.txt` registra duas falhas de `test_sql_marcos_nao_le_decisao` para cada mutante, uma com `ate` futuro e outra com `None`; não traz os detalhes das asserções. A captura testa a presença do nome da tabela de decisões no SQL. [mutantes_c3.txt:1–20, dado fornecido], [teste:316](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:316)

   | Mutante | Caminho efetivamente inserido |
   |---|---|
   | V1 | Alias de `getattr`, nome montado e consulta avaliada com `list`: corresponde ao escape dinâmico da v2. [mk.py:6, dado fornecido] |
   | V2 | Cursor em variável e nome da tabela montado: corresponde ao escape de SQL bruto da v2. [mk.py:7, dado fornecido] |
   | V3 | Itera perguntas da fixture e chama `respostas_anteriores()`, que percorre `self.decisoes.all()`: corresponde ao método de modelo apontado na v2. [mk.py:8, dado fornecido], [modelo:3793](app/chamados/models.py:3793) |
   | V4 | Chama por alias um helper de topo que avalia consulta a decisões: corresponde ao escape do fecho. [mk.py:9, dado fornecido], [mk.py:13, dado fornecido] |
   | V5 | Chama método de classe local que avalia consulta a decisões: corresponde ao outro escape do fecho. [mk.py:10, dado fornecido], [mk.py:14, dado fornecido] |

   As **cinco mutações não são todos os exemplos da v2**: a reexportação já chamada `D` e o lookup ORM montado não têm mutante específico neste anexo. A captura deve detectar SQL com a tabela nas execuções alcançadas, mas a execução dessas duas formas está **[nao verificado]**. [verif_v2.r, dado fornecido], [teste:320](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:320)

2. **Alcance da fixture e resíduo — NAO BLOQUEIA.** As duas consultas de `PerguntaDisputa` são construídas para ambos os valores de `ate`; a pergunta tem `respondida_em` definido antes das chamadas. O teste exige que alguma consulta mencione a tabela, mas não distingue individualmente as duas consultas nem prova seus resultados. [produção:201](app/chamados/veredito_lavrado.py:201), [teste:288](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:288), [teste:318](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:318)

   Ficam sem exercício demonstrado, por exemplo, uma pergunta com `materializada_em` preenchido, um marco posterior ao `ate` e entradas ou condições alternativas que desviem a execução. A docstring nomeia com honestidade a classe do resíduo — ramo não executado **e** não reconhecido pela AST — sem enumerar cada cenário. [modelo:3647](app/chamados/models.py:3647), [produção:163](app/chamados/veredito_lavrado.py:163), [teste:6](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:6)

3. **CORTE — NAO BLOQUEIA, com limite de evidência.** O teste grava uma decisão em 3/1/2020, posterior ao `respondida_em` fixado em 2/1/2020, e compara os dicionários antes e depois para os dois horizontes. Assim, morde uma implementação que faça essa decisão aumentar o máximo observado nessa fixture. Ele não afirma que o valor anterior seja exatamente `MARCO`; outro marco maior poderia mascarar a mudança. A mordida de um mutante específico que acrescente `Max(ato_em)` está **[nao verificado]**. [teste:272](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:272), [teste:298](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:298), [produção:169](app/chamados/veredito_lavrado.py:169)

4. **Relógio, fixture, subtest e Ruff — NAO BLOQUEIA.** `MARCO`, `ATO` e o horizonte são datetimes fixos e conscientes de UTC; `respondida_em` e `ato_em` não são `auto_now_add`. Os campos `Batida.criado_em` e `CelulaDia.gerada_em`, que também podem contribuir ao máximo, são `auto_now_add`; a ausência de efeitos indiretos que criem registros deles na preparação está **[nao verificado]**. [teste:272](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:272), [modelo:2635](app/chamados/models.py:2635), [modelo:4168](app/chamados/models.py:4168), [batida:79](app/ponto/models.py:79), [célula:1702](app/escala/models.py:1702)

   A atualização direta na fixture cria uma projeção com instante e texto vazio, contornando a guarda do `save`; isso é relevante para interpretar a fixture, embora a consulta de marcos leia o instante. `setUpTestData` prepara essa linha para os testes; os `subTest` registram as falhas por horizonte, como mostra o anexo dos mutantes. Aprovação da suíte sem mutação e resultado do Ruff estão **[nao verificado]**; a configuração seleciona `F`, `E9` e `B`. [teste:287](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:287), [modelo:3679](app/chamados/models.py:3679), [mutantes_c3.txt:1–20, dado fornecido], [ruff.toml:4](app/ruff.toml:4)

5. **Escritores e leitores — NAO BLOQUEIA.**

   - `PerguntaDisputa.respondida_em`: o teste escreve por `update`; em produção, a resposta humana e a automática atribuem e salvam o instante, a trilha também o escreve, e a reabertura pode zerá-lo. `marcos_do_lote` filtra e agrega esse campo; `DecisaoPergunta.linhas_da_projecao_sem_linha` também o lê para formar linhas de projeção. Esta lista identifica caminhos comprovados, não um censo exaustivo. [teste:291](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:291), [resposta humana:1527](app/chamados/services/disputa_emissao.py:1527), [automática:1683](app/chamados/services/disputa_emissao.py:1683), [trilha:1705](app/chamados/services/disputa_emissao.py:1705), [reabertura:1617](app/chamados/services/disputa_emissao.py:1617), [leitor:206](app/chamados/veredito_lavrado.py:206), [projeção:4251](app/chamados/models.py:4251)
   - `DecisaoPergunta.ato_em`: o teste fornece o instante no `create`; em produção, portas como a resposta humana e a automática fornecem o instante a `registrar`, que cria as linhas. `DecisaoPergunta.leitura` usa o campo para ordenar a sequência. A implementação atual de `marcos_do_lote` não o lê diretamente. [teste:307](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:307), [resposta humana:1532](app/chamados/services/disputa_emissao.py:1532), [automática:1686](app/chamados/services/disputa_emissao.py:1686), [registrar:4329](app/chamados/models.py:4329), [leitura:4308](app/chamados/models.py:4308), [marcos:151](app/chamados/veredito_lavrado.py:151)

A identidade Git da cópia e a correspondência completa do diff com a base são **[nao verificado]**: a consulta local informou ausência de repositório Git. O diff fornecido declara apenas a criação do teste, mas não substitui essa verificação. [diff.txt:1, dado fornecido] [nao verificado]

BLOQUEIA: 0  
perguntas de lei: 0

## 19/11 02:03 · sessao 2ee7e747fa4d · DST-L2-JZ1 · verificador · origem=Code

**AVAL-DEVOLVIDO: (1) e (3).** Há uma incompatibilidade literal entre D3 “intacto” e a retenção exigida por R7; além disso, o teto exato depende de medir o desenho conjunto, inclusive a leitura da retenção. O item (2) contém uma exceção nominal a D5 e usa uma porta existente. ([justificativa.py:266](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:266), [reconciliador.py:439](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/reconciliador.py:439), [nota.py:75](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/services/nota.py:75); anexos fornecidos sem linhas: [não verificado])

Os anexos foram fornecidos no texto, sem numeração de linhas; suas afirmações são identificadas abaixo pelo nome e marcadas **[não verificado]** quando não confirmadas pela cópia.

### (1) Tetos — PROCEDE

1. **Premissas.** A função atual consulta por chamado: `Q(chamado_origem_id=ch.pk)`, somado ao ponteiro, e executa o filtro de pendentes; os dois fechos a chamam em laço após as transições bem-sucedidas. ([justificativa.py:262](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:262), [cartorio.py:1016](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/cartorio.py:1016), [cartorio.py:1555](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/cartorio.py:1555))

   As 12 chaves e seus tetos atuais são:

   | Chave | Teto |
   |---|---:|
   | `registrar_batida (sem pergunta)` | 24 |
   | `registrar_batida (3 perguntas)` | 62 |
   | `_gravar_estado (sem pergunta)` | 30 |
   | `_gravar_estado (3 perguntas, CRUZANDO)` | 77 |
   | `reconciliar (gate da premissa, FECHA com dia e celula)` | 62 |
   | `reconciliar (gate da premissa, RECUSA com dia e celula)` | 14 |
   | `registrar_batida (furo vivo, 1 chamado fecha)` | **224** |
   | `registrar_batida (furo vivo, 3 chamados fecham)` | **288** |
   | `registrar_batida (furo vivo, 1 chamado FICA)` | 145 |
   | `registrar_batida (furo vivo, 3 chamados FICAM)` | 219 |
   | `registrar_batida (orfa de intervalo vetada)` | 31 |
   | `_gravar_estado (3 perguntas, NAO cruza)` | 10 |

   As duas em negrito são as chaves de fecho de FURO que o aval altera; a chave do gate `reconciliar (... FECHA ...)` também nomeia fecho e permanece entre as outras dez. ([test_selo_chokepoint_escrita.py:73](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_selo_chokepoint_escrita.py:73), [test_selo_chokepoint_escrita.py:134](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_selo_chokepoint_escrita.py:134), [test_selo_chokepoint_escrita.py:196](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_selo_chokepoint_escrita.py:196), [test_selo_chokepoint_escrita.py:210](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_selo_chokepoint_escrita.py:210))

2. **Aval vigente.** O novo aval corta expressamente a proibição anterior de mexer nesses dois tetos e troca a interface por chamado por lote. Porém, declara D3 intacto; a ressalva de R7 entra em conflito com essa declaração no caso descrito no item (3). (Anexos `aval.txt` e `brief_c2.md`: [não verificado]; [justificativa.py:266](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:266))

3. **Literal.** O selo conta todas as queries capturadas, não apenas `SELECT`, e compara o total ao teto. A ocorrência do item (2) cria `FollowupChamado` e, portanto, sua escrita conta se ocorrer dentro da captura. Nos cenários citados na pergunta de desenho, o dado fornecido relata zero justificativas e zero `UPDATE`; assim, pela condição do novo aval, também não haveria ocorrência por rejeição nesses cenários. O `+1` final **não está demonstrado**: R7 também exige consultar a retenção existente, e o custo do desenho conjunto não foi medido nos dados fornecidos. Qualquer delta diferente do corte exige a pergunta de desenho com SQL, como o próprio aval determina. ([test_selo_chokepoint_escrita.py:287](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_selo_chokepoint_escrita.py:287), [test_selo_chokepoint_escrita.py:428](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_selo_chokepoint_escrita.py:428), [nota.py:102](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/services/nota.py:102); `PERGUNTA-DE-DESENHO.txt` e `aval.txt`: [não verificado])

4. **Exportada, dinheiro ou `!`.** O desenho de lote declara mudar a busca e os tetos; alcance efetivo sobre exportada, dinheiro ou `!`: **[não verificado]**. A função atual grava a justificativa, inclusive `atualizado_em`, e o aval exige censo de dinheiro antes da conclusão. ([justificativa.py:281](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:281); `aval.txt`: [não verificado])

### (2) Selo JZ1 × D5 — NÃO PROCEDE

1. **Premissas.** O Fix A chama `_nota.anexar_ocorrencia`; a porta usa `autor_tipo='sistema'` por padrão e cria um `FollowupChamado`. O selo exige ao menos um followup de sistema e seu fixture já cria a justificativa com `chamado_origem`. ([reconciliador.py:572](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/reconciliador.py:572), [nota.py:75](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/services/nota.py:75), [nota.py:102](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/services/nota.py:102), [test_jz1_autoridade_unica.py:41](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_jz1_autoridade_unica.py:41), [test_jz1_autoridade_unica.py:60](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_jz1_autoridade_unica.py:60))

2. **Aval vigente.** D5 proibia followup novo no fecho; o novo aval declara uma exceção nominal quando houve rejeição. A exceção altera D5 nesse ponto e preserva o veto a followup sem rejeição e a S128 no fecho. (`brief_c2.md` e `aval.txt`: [não verificado])

3. **Literal.** A porta existente é suficiente para criar o tipo de registro que o selo conta; a passagem do teste com a implementação proposta continua **[não verificado]**. A ocorrência tem custo SQL, mas sua condição impede que seja criada nos cenários de teto descritos como sem rejeições. ([nota.py:75](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/services/nota.py:75), [test_jz1_autoridade_unica.py:60](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_jz1_autoridade_unica.py:60); `PERGUNTA-DE-DESENHO.txt` e `aval.txt`: [não verificado])

4. **Exportada, dinheiro ou `!`.** A escrita adicional declarada é uma ocorrência no chamado; efeito fora desse escopo, inclusive sobre exportada, dinheiro ou `!`: **[não verificado]**. ([nota.py:78](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/services/nota.py:78))

### (3) R7, R8 e R9 — PROCEDE

1. **Premissas.** A retenção existente consulta irmãs respondidas sem consumo e, durante `_APROVANDO`, retém quando o ponteiro aponta outra justificativa pendente. A função nova atual não consulta essa retenção. O cartório lê `atualizado_em` da justificativa nas linhas **391–392**, não na 387, que inicia a consulta de ausência. Existe o gerador de ESTADO e existe a função do contador D4; na busca da cópia, só foram encontrados a definição do contador e referências no teste, não um leitor no gerador. ([reconciliador.py:439](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/reconciliador.py:439), [justificativa.py:262](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:262), [cartorio.py:387](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/cartorio.py:387), [cartorio.py:391](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/cartorio.py:391), [gerar_estado.py:178](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/bin/gerar_estado.py:178), [justificativa.py:289](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:289), [test_dst_l2_jz1_rejeita_justificativa.py:346](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/tests/test_dst_l2_jz1_rejeita_justificativa.py:346))

2. **Aval vigente.** R7 manda não rejeitar quando a retenção responde reter. No caso de `_APROVANDO` com ponteiro para outra pendente, isso contraria o literal de D3, segundo o qual a pendente apontada é rejeitada, e o item (1) diz que D3 permanece intacto. É necessária uma decisão explícita sobre essa sobreposição. ([reconciliador.py:445](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/reconciliador.py:445), [justificativa.py:266](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:266); `aval.txt`: [não verificado])

3. **Literal.** R8 exige prova da segunda rodada sem alteração; a leitura de `atualizado_em` demonstra a razão para testá-la, não demonstra que o ciclo já está fechado. R9 exige um leitor efetivo para o contador; ele ainda não foi encontrado na cópia. R7 deve usar o mesmo predicado e seu custo precisa entrar na medição do item (1). ([cartorio.py:391](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/cartorio.py:391), [justificativa.py:289](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:289); `aval.txt`: [não verificado])

4. **Exportada, dinheiro ou `!`.** Efeito dos novos testes e do leitor pretendido sobre exportada, dinheiro ou `!`: **[não verificado]**. O contador atual é uma leitura que devolve a quantidade de pares; a rejeição atual grava a justificativa. ([justificativa.py:281](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:281), [justificativa.py:301](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:301))

**Pergunta de lei:** quando `_APROVANDO` faz a retenção responder `justificativa_alheia`, a pendente apontada fica preservada por R7, como exceção a D3, ou é rejeitada por D3? ([reconciliador.py:452](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/chamados/reconciliador.py:452), [justificativa.py:266](/home/ronald/codex_ws/e9f7edf83a74f7f8e96a87fd9c1f44e5690e4bcf/app/ponto/services/justificativa.py:266); `aval.txt`: [não verificado])

AVAL-DEVOLVIDO: (1), (3)  
perguntas de lei: 1

## 19/11 02:00 · sessao 3f83936ddcb6 · E4C-F3-LAVRA-FOTO · construtor · origem=Code

Acrescentei a testemunha de comportamento e de SQL em [test_f3_lavra_foto_sem_decisao.py](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:271). A camada AST e seus testes permanecem intactos; a docstring registra o resíduo de cobertura ([arquivo:1](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:1)). [nao verificado]

A base é `django.test.TestCase`, seguindo a criação ORM de empresa, praça, posto, colaborador, disputa e pergunta do [teste vizinho](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:29). O horizonte fixo UTC no ano 9998 fica além dos timestamps produzidos pelo relógio durante uma execução ordinária; o marco da pergunta e o instante do ato também são fixos ([fixture:271](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:271)). [nao verificado]

Os testes novos são: [nao verificado]

- `test_corte_decisao_posterior_nao_move_marcos`: compara antes e depois da inserção para os dois horizontes ([arquivo:298](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:298)).
- `test_sql_marcos_nao_le_decisao`: verifica a ausência da tabela da decisão e a presença da tabela da pergunta no SQL capturado ([arquivo:312](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:312)).
- `test_MORDE_captura_detecta_leitura_real_de_decisao`: controla o instrumento com uma consulta avaliada ([arquivo:324](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:324)).

Comando para a sessão executar a classe, inclusive após aplicar cada mutante do verificador:

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_f3_lavra_foto_sem_decisao.LavraFotoComBancoTest
```

A execução dos testes e a mordida nos mutantes são **[não verificado]**. Não executei testes, conforme solicitado; a sessão aplica os mutantes no arquivo real. [nao verificado]

## 19/11 02:00 · sessao acd426f3b1aa · CHECK-DE-TELA · sonda · origem=Code

Não consegui produzir uma sonda confiável para as cinco telas. A referência **não foi medida**, e não tenho confirmação de que a cópia corresponda ao hash informado: o diretório disponível não contém metadados Git verificáveis. [não verificado]

O impedimento concreto é que o envelope `--sombra` usa a role `leitor`, sem permissão de escrita. Uma transação com rollback não concede essa permissão: um GET que tente escrever pode falhar ou seguir outro caminho de erro, portanto não se pode tratar sua duração como a medição normal da tela. [bin/sonda_leitura.sh:4](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:4) [bin/sonda_leitura.sh:11](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:11)

O rastreio parcial encontrou:

- **Espelho:** `/ponto/espelho/` chama `espelho_ponto`, que chama `espelho_do_colab`. A rota de PDF avulso encontrada exige staff e gera o PDF por POST; não comprovei que seja a tela do colaborador pedida. Escritas indiretas do GET: [não verificado]. [app/config/urls.py:29](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/config/urls.py:29) [app/ponto/urls.py:10](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/urls.py:10) [app/ponto/views.py:160](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/views.py:160) [app/relatorios/views.py:1050](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/views.py:1050)
- **Tarefa do dia:** existe o serviço `tarefa_do_dia(colab, dia_do_turno, agora)`, mas não comprovei a rota de tela nem o universo que ela mostra. Escritas indiretas: [não verificado]. [app/chamados/services/tarefa_do_dia.py:738](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/tarefa_do_dia.py:738)
- **Fila do admin:** `/chamados/` chama `painel_gestao`, que chama `montar_contexto_painel` e, na página completa, `pendentes_na_central`. Escritas indiretas do GET: [não verificado]. [app/config/urls.py:28](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/config/urls.py:28) [app/chamados/urls.py:9](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/urls.py:9) [app/chamados/views.py:721](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/views.py:721) [app/chamados/views.py:759](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/views.py:759)
- **Placar:** `/chamados/placar/` chama `placar_json`; conforme filtro e disponibilidade do resultado lavrado, lê `PlacarNavbar` ou chama `montar_contexto_painel`. O próprio código documenta escrita condicional ao relavrar perguntas desatualizadas; o estado da sombra atual é [não verificado]. [app/chamados/urls.py:24](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/urls.py:24) [app/chamados/views.py:781](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/views.py:781) [app/chamados/views.py:840](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/views.py:840)
- **Consulta do app:** `/api/me/` chama `api_me`, que consulta, entre outros leitores, `turno_aberto_de` e `projecao_do_app`. Que seja a consulta pedida na abertura e que seus leitores indiretos escrevam ou não: [não verificado]. [app/api/urls.py:64](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/api/urls.py:64) [app/api/views.py:284](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/api/views.py:284) [app/api/views.py:306](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/api/views.py:306) [app/api/views.py:315](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/api/views.py:315)

Também há um GET de atendimento do colaborador que pode registrar auditoria, mas não comprovei que ele seja a tela “tarefa do dia” solicitada. [app/chamados/views.py:559](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/views.py:559) [app/chamados/views.py:583](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/views.py:583)

Ficaram pendentes o script, a verificação completa das escritas e os universos para as duas cargas. Não tenho resultados TSV a apresentar. [não verificado]

perguntas de lei: 0

## 19/11 01:56 · sessao 40d3f8bb6fa7 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | tipo | classe | trecho literal curto |
|---|---|---|---|---|---|---|
| chamados/management/commands/estoque_l130_instante_sem_texto.py:60 | handle | validada_em | PerguntaDisputa | rótulo/metadado sem efeito | ii | `'validada_em': None` |
| chamados/models.py:3031 | _nascer | respondida_em | PerguntaDisputa | leitura | ii | `campos.get('respondida_em')` |
| chamados/models.py:3032 | _nascer | resposta_colab | PerguntaDisputa | leitura | ii | `campos.get('resposta_colab')` |
| chamados/models.py:3324 | carimbar | materializada_em, materializada_por, via_resolucao | PerguntaDisputa | escrita | ii | `campos = ['materializada_em', …]` |
| chamados/models.py:3329 | carimbar | respondida_em | PerguntaDisputa | escrita | ii | `campos.append('respondida_em')` |
| chamados/models.py:3333 | carimbar | validada_em, validada_por | PerguntaDisputa | escrita | ii | `campos += ['validada_em', 'validada_por']` |
| chamados/models.py:3382 | carimbar_identidade | via_resolucao, materializada_em, materializada_por | PerguntaDisputa | escrita | ii | `campos = ['via_resolucao', …]` |
| chamados/models.py:3406 | descarimbar | via_resolucao, materializada_em, materializada_por | PerguntaDisputa | escrita | ii | `campos = ['via_resolucao', …]` |
| chamados/models.py:3417 | escopo da classe | respondida_em, resposta_colab, validada_em, materializada_em, via_resolucao, data_evento | PerguntaDisputa | leitura | iii | `CAMPOS_QUE_ACORDAM` |
| chamados/models.py:3454 | descarimbar_em_lote | materializada_em, materializada_por, materializada_por_id, via_resolucao | PerguntaDisputa | leitura | ii | `set(outros) & {...}` |
| chamados/models.py:3459 | descarimbar_em_lote | respondida_em, resposta_colab | PerguntaDisputa | leitura | ii | `& set(outros)` |
| chamados/models.py:3464 | descarimbar_em_lote | respondida_em, resposta_colab | PerguntaDisputa | leitura | ii | `outros['respondida_em']` |
| chamados/models.py:3465 | descarimbar_em_lote | respondida_em | PerguntaDisputa | leitura | ii | `'respondida_em' in _par` |
| chamados/models.py:3469 | descarimbar_em_lote | respondida_em | PerguntaDisputa | leitura | ii | `outros['respondida_em']` |
| chamados/models.py:3472 | descarimbar_em_lote | resposta_colab | PerguntaDisputa | leitura | ii | `outros['resposta_colab']` |
| chamados/models.py:3557 | q_validada_pelo_ato | validada_em | PerguntaDisputa | leitura | vi | `OuterRef('validada_em')` |
| chamados/models.py:3558 | q_validada_pelo_ato | validada_em | PerguntaDisputa | leitura | vi | `ato_em__gt=OuterRef('validada_em')` |
| chamados/models.py:3690 | escopo da classe | respondida_em, resposta_colab | PerguntaDisputa | leitura | ii | `_PAR_DA_RESPOSTA` |
| chamados/services/disputa_emissao.py:1483 | responder_pergunta_disputa_core | resposta_colab, respondida_em | PerguntaDisputa | escrita | ii | `_campos = ['resposta_colab', 'respondida_em']` |
| chamados/services/disputa_emissao.py:1496 | responder_pergunta_disputa_core | validada_em, validada_por | PerguntaDisputa | escrita | ii | `_campos += ['validada_em', 'validada_por']` |
| chamados/services/disputa_emissao.py:1545 | responder_pergunta_disputa_core | validada_em, validada_por | PerguntaDisputa | rótulo/metadado sem efeito | v | `antes={'validada_em': …}` |
| chamados/services/disputa_emissao.py:1546 | responder_pergunta_disputa_core | validada_em, validada_por | PerguntaDisputa | rótulo/metadado sem efeito | v | `depois={'validada_em': None, …}` |
| chamados/services/disputa_emissao.py:1685 | responder_automaticamente | resposta_colab, respondida_em | PerguntaDisputa | escrita | ii | `p.save(update_fields=[…])` |
| chamados/services/disputa_emissao.py:1707 | carimbar_resposta_pela_trilha | respondida_em | PerguntaDisputa | escrita | ii | `update_fields=['respondida_em']` |
| chamados/services/disputa_emissao.py:1713 | carimbar_resposta_pela_trilha | respondida_em | PerguntaDisputa | rótulo/metadado sem efeito | v | `depois={'respondida_em': …}` |
| chamados/services/disputa_emissao.py:2094 | _resolver_perguntas_por_chamado_resolvido | materializada_em, via_resolucao | PerguntaDisputa | rótulo/metadado sem efeito | v | `_antes = {'materializada_em': None, …}` |
| chamados/services/foto_e4b.py:180 | nascimentos | materializada_em, respondida_em, validada_em | PerguntaDisputa | leitura | iii | `a[c] is not None and d[c] is None` |
| chamados/services/historico.py:99 | _eventos_dos_chamados | respondida_em | PerguntaDisputa | leitura | v | `('respondida_em', 'pergunta_respondida', …)` |
| chamados/services/historico.py:101 | _eventos_dos_chamados | validada_em | PerguntaDisputa | leitura | v | `('validada_em', 'pergunta_validada', …)` |
| chamados/services/historico.py:103 | _eventos_dos_chamados | materializada_em | PerguntaDisputa | leitura | v | `('materializada_em', 'pergunta_materializada', …)` |
| chamados/services/materializacao.py:1689 | _carimbar_recusa | materializada_em, via_resolucao, recusa_motivo | PerguntaDisputa | leitura | ii | `p.refresh_from_db(fields=[…])` |
| chamados/services/painel.py:67 | marcador_do_placar | respondida_em, materializada_em, validada_em | PerguntaDisputa | leitura | vi | `Max('respondida_em')` |
| chamados/services/passivo_validado.py:64 | escopo do módulo | materializada_em, materializada_por_id, via_resolucao | PerguntaDisputa | leitura | ii | `CAMPOS_REVERSAO = (…)` |
| chamados/services/passivo_validado.py:66 | escopo do módulo | respondida_em, validada_em, validada_por_id, recusa_motivo | PerguntaDisputa | leitura | ii | `'respondida_em', 'validada_em'` |
| chamados/services/passivo_validado.py:535 | _plantar_o_dia | materializada_em, via_resolucao, materializada_por | PerguntaDisputa | leitura | ii | `p.refresh_from_db(fields=[…])` |
| chamados/services/regularizacao_ext.py:201 | materializar_regularizacao_externa | status, materializada_em, materializada_por | outro modelo: RegularizacaoExterna | escrita | — | `reg.save(update_fields=[…])` |
| chamados/services/relavra_via.py:252 | desfazer_carimbo_sem_lastro | materializada_em, via_resolucao | PerguntaDisputa | rótulo/metadado sem efeito | v | `depois={'materializada_em': None, …}` |
| chamados/services/tarefa_do_dia.py:418 | VistaNoHorizonte.__getattr__ | respondida_em, validada_em | PerguntaDisputa | leitura | iii | `valor > self._agora` |
| chamados/services/validacao.py:171 | _carimbar_validacao | validada_em, validada_por | PerguntaDisputa | escrita | ii | `perg.save(update_fields=[…])` |
| chamados/services/validacao.py:262 | devolver_validacao_recusada | validada_em, validada_por, materializada_em, recusa_motivo | PerguntaDisputa | leitura | i | `perg.refresh_from_db(fields=[…])` |
| chamados/services/validacao.py:263 | devolver_validacao_recusada | resposta_colab, tipo_resposta | PerguntaDisputa | leitura | ii | `'resposta_colab', 'tipo_resposta'` |
| chamados/services/validacao.py:276 | devolver_validacao_recusada | validada_em, validada_por | PerguntaDisputa | escrita | ii | `update_fields=['validada_em', 'validada_por']` |
| chamados/services/validacao.py:300 | devolver_validacao_recusada | validada_em, validada_por_id | PerguntaDisputa | rótulo/metadado sem efeito | v | `antes={'validada_em': …}` |
| chamados/services/validacao.py:302 | devolver_validacao_recusada | validada_em, validada_por_id, recusa_motivo | PerguntaDisputa | rótulo/metadado sem efeito | v | `depois={'validada_em': None, …}` |
| chamados/services/validacao.py:351 | devolver_paridade_bug_par_103 | validada_em, validada_por | PerguntaDisputa | escrita | ii | `update_fields=campos + […]` |
| chamados/services/validacao.py:363 | devolver_paridade_bug_par_103 | materializada_em, via_resolucao, materializada_por_id | PerguntaDisputa | rótulo/metadado sem efeito | v | `depois={'materializada_em': None, …}` |
| chamados/services/validacao.py:364 | devolver_paridade_bug_par_103 | validada_em, validada_por_id, recusa_motivo | PerguntaDisputa | rótulo/metadado sem efeito | v | `'validada_em': None` |
| chamados/services/validacao.py:453 | devolver_instante_sem_texto_l130 | respondida_em, resposta_colab, via_resolucao | PerguntaDisputa | rótulo/metadado sem efeito | v | `depois={'respondida_em': None, …}` |
| chamados/services/validacao.py:576 | _aceitar_a_folga | validada_em, validada_por | PerguntaDisputa | leitura | ii | `perg.refresh_from_db(fields=[…])` |
| chamados/services/validacao.py:789 | validar_pergunta | validada_em, materializada_em | PerguntaDisputa | leitura | v | `perg.refresh_from_db(fields=[…])` |
| chamados/services/validacao.py:932 | recusar_pergunta | resposta_colab | PerguntaDisputa | leitura | v | `antes['resposta_colab']` |
| chamados/services/validacao.py:937 | recusar_pergunta | respondida_em, resposta_colab | PerguntaDisputa | rótulo/metadado sem efeito | v | `'respondida_em': None, 'resposta_colab': ''` |
| chamados/services/verbos.py:344 | placar_aceite | respondida_em | PerguntaDisputa | leitura | v | `pg['respondida_em'] > cob_por_colab[cid]` |
| chamados/services/verbos.py:347 | placar_aceite | respondida_em | PerguntaDisputa | leitura | v | `_tz.localtime(pg['respondida_em'])` |
| chamados/veredito_lavrado.py:204 | marcos_do_lote | materializada_em | PerguntaDisputa | leitura | iv | `annotate(m=Max('materializada_em'))` |
| chamados/veredito_lavrado.py:208 | marcos_do_lote | respondida_em | PerguntaDisputa | leitura | iv | `annotate(m=Max('respondida_em'))` |

linhas: 56 de 56

Totais por classe: i: 1; ii: 28; iii: 3; iv: 2; v: 18; vi: 3. Outro modelo: 1.

- **i — chamados/services/validacao.py:262:** `validar_pergunta` chama `devolver_validacao_recusada` em chamados/services/validacao.py:781. Sim: a leitura de `validada_em` participa da decisão de quando a própria porta escreve, pela condição em chamados/services/validacao.py:270 e pelo `save` em chamados/services/validacao.py:276.

perguntas de lei: 0

## 19/11 01:55 · sessao d86cb0ea8a17 · E4D · sonda · origem=Code

A limpeza L130 grava a ação `l130_instante_sem_texto_devolvida` para `PerguntaDisputa` e preserva, quando necessário, a resposta anterior como `RESPOSTA`; ela não grava `REABERTURA` nem `DEVOLUCAO`. A escolha entre as três opções de D3 permanece **[nao verificado]**. [app/chamados/services/validacao.py:414](app/chamados/services/validacao.py#L414), [app/chamados/services/validacao.py:436](app/chamados/services/validacao.py#L436), [app/chamados/services/validacao.py:449](app/chamados/services/validacao.py#L449).

**A — Escrita e leitura**

As três opções ainda exigem definir seus escritores: o código atual da L130 não escreve nenhum dos três atos propostos, e os sítios de backfill de um ato ainda não definido são **[nao verificado]**. A porta comum atual para inserir decisões é `DecisaoPergunta.registrar`; além da L130, os backfills existentes a usam para registrar linhas da projeção, e um deles também registra `SILENCIO`. [app/chamados/models.py:4329](app/chamados/models.py#L4329), [app/chamados/management/commands/backfill_fio_morto_e4b.py:141](app/chamados/management/commands/backfill_fio_morto_e4b.py#L141), [app/chamados/management/commands/backfill_decisoes_pergunta.py:324](app/chamados/management/commands/backfill_decisoes_pergunta.py#L324).

| Opção | Comportamento comprovado dos leitores atuais |
|---|---|
| **1. Novo valor de `ato`** | `DecisaoPergunta.leitura` o ignora: só `RESPOSTA` e `VALIDACAO` atribuem valores, e os conjuntos de limpeza não incluem um novo valor. A resposta anterior continuaria na leitura. `respostas_anteriores` também o ignora. A tarefa do dia, se ele for o último ato considerado, retorna `ato_desconhecido:<ato>` e nenhuma ação. [app/chamados/models.py:4277](app/chamados/models.py#L4277), [app/chamados/models.py:4317](app/chamados/models.py#L4317), [app/chamados/models.py:3800](app/chamados/models.py#L3800), [app/chamados/services/tarefa_do_dia.py:158](app/chamados/services/tarefa_do_dia.py#L158), [app/chamados/services/tarefa_do_dia.py:187](app/chamados/services/tarefa_do_dia.py#L187). |
| **2. Valor existente com marca** | A leitura atual decide pelo valor de `ato`, sem consultar uma marca L130; portanto, o efeito é o do valor escolhido. A tarefa do dia só conhece a exclusão especial da marca `da_ressuscitacao`, que está restrita a `REABERTURA`; não há comportamento comprovado para uma nova marca L130. [app/chamados/models.py:4317](app/chamados/models.py#L4317), [app/chamados/models.py:4208](app/chamados/models.py#L4208), [app/chamados/services/tarefa_do_dia.py:421](app/chamados/services/tarefa_do_dia.py#L421). |
| **3. Saneamento separado** | O escritor, o formato, a ordem em relação às decisões e o consumo pelo projetor são **[nao verificado]**. O leitor atual só monta sua sequência com `p.decisoes` e o passivo da projeção; um registro separado que não entre nessa sequência não muda essa leitura hoje. [app/chamados/models.py:4308](app/chamados/models.py#L4308). |

Na opção 2, os efeitos dos valores existentes na leitura dos três campos são estes: [app/chamados/models.py:4272](app/chamados/models.py#L4272), [app/chamados/models.py:4317](app/chamados/models.py#L4317).

| Valor candidato | Efeito atual e consequência para a limpeza |
|---|---|
| `REABERTURA` ou `RECUSA` | Limpam resposta **e validação** na leitura; a tarefa do dia os classifica como `devolvida_ao_colaborador`, com ação de responder. A tela de respostas anteriores inclui a linha de foto. A porta normal de reabertura também zera `validada_em` e `validada_por`, além do par e carimbo; a elegibilidade L130 exige `validada_em` e `materializada_em` nulos. O efeito de todos os demais campos e efeitos colaterais de chamar essa porta em vez da limpeza L130 é **[nao verificado]**. [app/chamados/models.py:4277](app/chamados/models.py#L4277), [app/chamados/services/tarefa_do_dia.py:163](app/chamados/services/tarefa_do_dia.py#L163), [app/chamados/models.py:3806](app/chamados/models.py#L3806), [app/chamados/services/disputa_emissao.py:1617](app/chamados/services/disputa_emissao.py#L1617), [app/chamados/services/validacao.py:392](app/chamados/services/validacao.py#L392). |
| `DEVOLUCAO` | Limpa a validação, mas **não** a resposta, na leitura; como último ato, a tarefa do dia pede validação pelo admin. Uma `DEVOLUCAO` com motivo específico `BUG-PAR-103` também é reconhecida pelo leitor desse selo. [app/chamados/models.py:4277](app/chamados/models.py#L4277), [app/chamados/services/tarefa_do_dia.py:165](app/chamados/services/tarefa_do_dia.py#L165), [app/chamados/services/materializacao.py:267](app/chamados/services/materializacao.py#L267). |
| `RESPOSTA` | Instala instante e texto; pode limpar validação conforme `resposta_mantem_validacao`. A tarefa distingue texto vazio de texto preenchido. Não representa, por si, a retirada da resposta. [app/chamados/models.py:4280](app/chamados/models.py#L4280), [app/chamados/models.py:4318](app/chamados/models.py#L4318), [app/chamados/services/tarefa_do_dia.py:159](app/chamados/services/tarefa_do_dia.py#L159). |
| `VALIDACAO` | Instala validação; a tarefa avalia a lavra e o efeito. Não representa a retirada da resposta. [app/chamados/models.py:4320](app/chamados/models.py#L4320), [app/chamados/services/tarefa_do_dia.py:167](app/chamados/services/tarefa_do_dia.py#L167). |
| `SILENCIO`, `RETRATACAO`, `ABSORCAO` | Não alteram os três campos da leitura. A tarefa atribui significado próprio a silêncio e absorção e trata retratação em outro ponto; `ABSORCAO` exige as duas ligações previstas na restrição do modelo. [app/chamados/models.py:4276](app/chamados/models.py#L4276), [app/chamados/services/tarefa_do_dia.py:176](app/chamados/services/tarefa_do_dia.py#L176), [app/chamados/services/tarefa_do_dia.py:260](app/chamados/services/tarefa_do_dia.py#L260), [app/chamados/models.py:4204](app/chamados/models.py#L4204). |

A face SQL da validação usa os mesmos conjuntos de limpeza e precisaria refletir uma mudança que alterasse a validação; as telas e propriedades que chamam `leitura` recebem o resultado dela. O inventário **exaustivo** dos demais leitores, selos e comandos de auditoria, bem como seu comportamento individual, permanece **[nao verificado]**. [app/chamados/models.py:3539](app/chamados/models.py#L3539), [app/chamados/models.py:3551](app/chamados/models.py#L3551), [app/chamados/models.py:4299](app/chamados/models.py#L4299).

**B — Choice e contagem**

O choice atual contém oito valores: `resposta`, `validacao`, `devolucao`, `recusa`, `reabertura`, `silencio`, `retratacao` e `absorcao`. [app/chamados/models.py:4152](app/chamados/models.py#L4152).

O número **total** de leitores com match exaustivo é **[nao verificado]**. Um ponto comprovado que precisa ser revisto para um novo ato é `_pela_decisao`: ele possui ramos para os valores que trata e devolve `ato_desconhecido` para o restante; `RETRATACAO` também é tratada antes, no chamador. Isso não prova que seja o único ponto a migrar. [app/chamados/services/tarefa_do_dia.py:142](app/chamados/services/tarefa_do_dia.py#L142), [app/chamados/services/tarefa_do_dia.py:187](app/chamados/services/tarefa_do_dia.py#L187), [app/chamados/services/tarefa_do_dia.py:259](app/chamados/services/tarefa_do_dia.py#L259).

**C — Sonda de frota**

A sonda abaixo conta perguntas distintas encontradas pelo log, por empresa atual; separa explicitamente pergunta ausente e competência sem dia. Para divergência, exige que a resposta vigente reconstruída seja uma linha `RESPOSTA` anterior ou simultânea ao último log L130 da pergunta **e** que o par reconstruído divirja do par atual da coluna. Isso mede o estado atual; não prova a divergência imediatamente após uma limpeza se houve alterações posteriores. A ordem de atos usada por `leitura` é `(ato_em, pk)`, com passivo adicional da projeção. [app/core/models.py:295](app/core/models.py#L295), [app/chamados/services/validacao.py:449](app/chamados/services/validacao.py#L449), [app/chamados/models.py:4295](app/chamados/models.py#L4295).

O juiz é `materializacao.competencia_do_fato`, o mesmo chamado pela elegibilidade L130; ele compõe a janela do dia do fato com `folha.export.competencia_entregue` e devolve `None` sem dia. Os resultados de produção são **[nao verificado]**: a sonda não foi executada. [app/chamados/services/validacao.py:400](app/chamados/services/validacao.py#L400), [app/chamados/services/materializacao.py:788](app/chamados/services/materializacao.py#L788).

```python
import collections
import os
import sys

from django.conf import settings
from django.db import connection, transaction

from chamados.models import DecisaoPergunta, PerguntaDisputa
from chamados.services.materializacao import competencia_do_fato
from core.models import LogAuditoria


ACAO = "l130_instante_sem_texto_devolvida"
MODELO = "PerguntaDisputa"


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def main():
    etapa = "GUARDA_SOMBRA"
    try:
        with transaction.atomic():
            guarda_sombra()

            etapa = "LOGS"
            logs = list(
                LogAuditoria.objects.filter(acao=ACAO, modelo=MODELO)
                .order_by("objeto_id", "criado_em", "pk")
                .values_list("objeto_id", "criado_em", "pk")
            )
            ultimo_log = {}
            for pergunta_id, criado_em, log_id in logs:
                if pergunta_id is None:
                    raise RuntimeError("log_sem_objeto")
                ultimo_log[pergunta_id] = (criado_em, log_id)

            etapa = "PERGUNTAS"
            perguntas = list(
                PerguntaDisputa.objects.filter(pk__in=ultimo_log)
                .select_related("disputa__colaborador__empresa")
                .prefetch_related("decisoes")
                .order_by("pk")
            )
            por_pk = {p.pk: p for p in perguntas}
            ausentes = set(ultimo_log) - set(por_pk)

            universo = collections.Counter()
            divergencia = collections.Counter()
            competencia = collections.Counter()
            exemplos = collections.defaultdict(list)

            etapa = "CLASSIFICACAO"
            for p in perguntas:
                colab = p.disputa.colaborador
                emp = colab.empresa_id
                universo[emp] += 1

                momento_log = ultimo_log[p.pk][0]
                leitura = DecisaoPergunta.leitura(p)
                resposta_real_anterior = any(
                    d.ato == DecisaoPergunta.RESPOSTA
                    and d.ato_em == leitura.respondida_em
                    and d.ato_em <= momento_log
                    for d in p.decisoes.all()
                )
                par_lido = (leitura.respondida_em, leitura.resposta_colab or "")
                par_coluna = (p.respondida_em, p.resposta_colab or "")
                diverge = resposta_real_anterior and par_lido != par_coluna
                divergencia[(emp, "sim" if diverge else "nao")] += 1

                k6 = competencia_do_fato(p, colab.empresa)
                estado = (
                    "sem_dia" if k6 is None
                    else "entregue" if k6[2]
                    else "nao_entregue"
                )
                competencia[(emp, estado)] += 1
                exemplos[(emp, "sim" if diverge else "nao", estado)].append(
                    (p.pk, colab.pk)
                )

            etapa = "SAIDA"
            linhas = [
                "LOGS registros=%d perguntas_distintas=%d"
                % (len(logs), len(ultimo_log)),
                "PERGUNTAS_AUSENTES=%d" % len(ausentes),
            ]
            for pk in sorted(ausentes):
                linhas.append("AUSENTE pergunta_pk=%d" % pk)

            total_universo = 0
            total_div = 0
            total_comp = 0
            for emp in sorted(universo, key=str):
                n = universo[emp]
                nd = sum(divergencia[(emp, x)] for x in ("sim", "nao"))
                nc = sum(
                    competencia[(emp, x)]
                    for x in ("entregue", "nao_entregue", "sem_dia")
                )
                total_universo += n
                total_div += nd
                total_comp += nc

                linhas.append(
                    "EMPRESA pk=%s limpas=%d divergentes=%d "
                    "nao_divergentes=%d entregues=%d nao_entregues=%d sem_dia=%d"
                    % (
                        emp, n,
                        divergencia[(emp, "sim")],
                        divergencia[(emp, "nao")],
                        competencia[(emp, "entregue")],
                        competencia[(emp, "nao_entregue")],
                        competencia[(emp, "sem_dia")],
                    )
                )
                linhas.append(
                    "CONFERENCIA emp=%s divergencia_contador=%d "
                    "competencia_contador=%d universo=%d contador==universo=%s"
                    % (emp, nd, nc, n, "s" if nd == n and nc == n else "n")
                )
                if nd != n or nc != n:
                    raise RuntimeError("soma_empresa")

            for (emp, div, estado), pares in sorted(
                exemplos.items(), key=lambda item: tuple(map(str, item[0]))
            ):
                for pergunta_pk, colab_pk in pares[:2]:
                    linhas.append(
                        "EXEMPLO emp=%s divergente=%s competencia=%s "
                        "pergunta_pk=%d col%d"
                        % (emp, div, estado, pergunta_pk, colab_pk)
                    )

            total_com_ausentes = total_universo + len(ausentes)
            confere = (
                total_div == total_universo
                and total_comp == total_universo
                and total_com_ausentes == len(ultimo_log)
            )
            linhas.append(
                "CONFERENCIA_TOTAL divergencia_contador=%d "
                "competencia_contador=%d universo_existente=%d ausentes=%d "
                "universo_logs=%d contador==universo=%s"
                % (
                    total_div, total_comp, total_universo, len(ausentes),
                    len(ultimo_log), "s" if confere else "n",
                )
            )
            if not confere:
                raise RuntimeError("soma_total")

        for linha in linhas:
            print(linha)
        print("ERROS=0")
        print("FIM_SONDA_D3_L130")
        return 0
    except Exception:
        print("ERRO " + etapa)
        print("ERROS=1")
        print("FIM_SONDA_D3_L130")
        return 1


sys.exit(main())
```

perguntas de lei: 2

1. Qual representação adotar para a limpeza reconstruível: **(1)** novo ato compensador, **(2)** ato existente com marca explícita, ou **(3)** saneamento separado consumido pelo projetor? A decisão atual é **[nao verificado]**. [app/chamados/services/validacao.py:414](app/chamados/services/validacao.py#L414).
2. Como tratar a reconstrução do histórico e o estoque em competência entregue: **(a)** reconstruir automaticamente, **(b)** tratar por procedimento específico, ou **(c)** outra regra? A decisão atual é **[nao verificado]**. [app/chamados/services/validacao.py:381](app/chamados/services/validacao.py#L381).

## 19/11 01:54 · sessao 69c8d1976b61 · E4C-F3-LAVRA-FOTO · verificador · origem=Code

**BLOQUEIA:** o selo ainda permite leituras de `DecisaoPergunta` dentro de funções alcançadas no próprio módulo. O fecho segue somente chamadas cujo alvo é um nome que coincide diretamente com uma função de topo; não resolve aliases, chamadas por atributo nem métodos de classes. [teste:49](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:49)

Executei o detector isolado sobre a fonte e contramutações **em memória**: os exemplos de escape abaixo retornaram `[]`, salvo onde indicado. A execução das consultas e da suíte Django é **[não verificado]**. O detector sobre a fonte atual também retornou `[]`. [teste:41](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:41)

### Cura dos itens da volta 1

| Item | Resultado |
|---|---|
| **Helper transitivo** | **Parcial.** M7 prova a chamada direta por nome, inclusive em dois níveis; alias de função e método de classe no mesmo módulo escapam. [detector:60](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:60), [M7:217](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:217) |
| **Reexportação** | **Parcial.** M8 importa um símbolo ainda chamado `DecisaoPergunta`; o registro só reconhece esse nome original. Uma reexportação já chamada `D` escapa. [detector:68](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:68), [M8:227](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:227) |
| **`getattr`/`get_model` e nomes montados** | **Parcial.** M9 morde `getattr` chamado diretamente com expressão não constante; um alias de `getattr` com o nome montado escapa. [detector:97](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:97), [M9:233](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:233) |
| **Prefetch, string, `**dict` e segundo relacionamento** | **Parcial.** M10 e M11 mordem os literais `decisoes` e `absorcoes_feitas__isnull`; um caminho montado por concatenação escapa. Os dois nomes vêm dos relacionamentos declarados no modelo. [detector:88](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:88), [M10–M11:239](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:239), [modelo:4166](app/chamados/models.py:4166), [modelo:4176](app/chamados/models.py:4176) |
| **Caminho ORM indireto e SQL bruto** | **Parcial.** M12 morde o nome literal da tabela; SQL montado em partes escapa. M11 demonstra um caminho ORM literal, não todos os caminhos montados. [detector:92](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:92), [M12:249](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:249) |

Contramutações concretas, inseridas em `marcos_do_lote`, salvo a definição do helper. Elas retornaram `[]` no detector isolado; a validade e execução das consultas contra um banco são **[não verificado]**. [produção:151](app/chamados/veredito_lavrado.py:151), [teste:109](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:109)

```python
# Helper definido no mesmo módulo; as duas últimas linhas vão em marcos_do_lote.
def _leitor(): return __import__('chamados.models', fromlist=['DecisaoPergunta']).DecisaoPergunta.objects.all()
leitura = _leitor
leitura()
```

A forma acima também contém acesso dinâmico no helper não visitado. Uma versão com import direto no helper igualmente retornou `[]`; é essa que demonstra a falha do fecho, independentemente do acesso dinâmico. [teste:60](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:60)

```python
from chamados.tests.test_e4c_f2_leitura_do_ato import D
D.objects.all()
```

`D` é uma reexportação existente de `DecisaoPergunta`. [origem:19](app/chamados/tests/test_e4c_f2_leitura_do_ato.py:19)

```python
import chamados.models as cm
g = getattr
g(cm, 'Decisao' + 'Pergunta').objects.all()
```

```python
from chamados.models import PerguntaDisputa as PD
list(PD.objects.filter(**{'absorcoes_' + 'feitas__isnull': False}))
```

```python
from django.db import connection
c = connection.cursor()
c.execute('SELECT * FROM chamados_' + 'decisao' + 'pergunta')
```

O último exemplo escapa porque `execute` sobre a variável `c` não corresponde ao formato especial `cursor().execute`, e nenhuma das constantes contém o nome completo procurado. [teste:92](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:92)

### Formas adicionais de alcance

- **Alias de helper ou lambda que o chama: escapa e cabe no contrato do módulo.** O helper pode ser uma função de topo de `veredito_lavrado.py`; `leitura = _leitor; leitura()` não faz o detector entrar nela. Uma lambda que invoque `_leitor()` diretamente é percorrida por `ast.walk` e essa chamada direta é seguida; o escape demonstrado é o alias da função. [teste:49](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:49)
- **Método de classe definida no próprio módulo: escapa e cabe no contrato.** Uma classe `Leitor` com método `ler` que importe e consulte `DecisaoPergunta`, seguida de `Leitor().ler()` no alvo, retornou `[]`: métodos de classe não entram no mapa de funções de topo. [teste:49](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:49)
- **Método de instância de modelo em outro módulo: escapa.** Após importar `PerguntaDisputa` como `PD`, `list(PD.objects.first().respostas_anteriores())` retornou `[]`; o método existente percorre `self.decisoes.all()`. Se uma chamada a esse método deve ser proibida por este contrato limitado ao fecho *do módulo* é **pergunta de lei**. A execução com uma pergunta existente é **[não verificado]**. [modelo:3793](app/chamados/models.py:3793), [teste:62](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:62)
- **Helper em outro módulo: escapa quanto ao corpo.** O detector monta o fecho apenas com funções do AST da fonte recebida; ampliar a obrigação para o corpo importado é a mesma pergunta de lei sobre fronteira de módulo. [teste:43](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:43), [teste:49](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:49)
- **Atributo encadeado com nome literal: não escapou no exemplo `f(cm).DecisaoPergunta.objects.all()`.** O detector reconhece o atributo pelo segmento `DecisaoPergunta`, mesmo com base encadeada. Isso não prova cobertura de atributos cujo nome seja construído dinamicamente. [teste:117](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:117)

### Falso positivo e classes de reprovação

Não encontrei falso positivo no trecho atual: o detector isolado retornou lista vazia para a fonte atual. Isso não prova ausência de falsos positivos em futuras alterações; por exemplo, ele trata nomes de atributos e strings que coincidam com segmentos protegidos sem verificar o tipo do objeto ou se a string é caminho ORM. [teste:92](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:92), [teste:117](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:117)

- **Selo sem mordida:** há mordida demonstrada pela lógica e pelos mutantes declarados, mas ainda é **parcial**; os escapes internos acima constituem um único achado bloqueante consolidado. A execução da suíte M1–M12 é **[não verificado]**. [teste:217](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:217)
- **Fonte única:** os `related_name` são lidos por AST diretamente de `models.py`, em vez de serem copiados como conjunto fixo no detector. Há, porém, o literal adicional `decisoes` no teste de controle e segmentos fixos para o nome do modelo; o extrator só contempla as formas sintáticas indicadas no código. [teste:16](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:16), [teste:89](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:89), [teste:212](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:212)
- **Campo com dois significados:** não há campo de produção novo no diff fornecido; existência dessa classe no restante do sistema é **[não verificado]**. [teste:1](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:1)
- **Alvo ausente:** o detector exige exatamente uma função de topo com o nome solicitado, e há teste para ausência. [teste:44](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:44), [teste:151](app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:151)

A identidade Git da cópia com o hash informado é **[não verificado]**: o diretório consultado não tem metadados Git. A correspondência do diff completo com a base também é **[não verificado]**.

BLOQUEIA: 1  
perguntas de lei: 1

_7266 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
