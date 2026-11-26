# CODEX-ESTADO

> Ultima resposta processada: **26/11/2026 17:41** · publicado em **26/11/2026 17:45** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 26/11 17:41 · sessao 2871c389c64e · DIA-DA-BATIDA-JUIZ-UNICO · verificador · origem=Code

## Veredito — REPROVADO

1. **B1 corrige o ciclo apontado na F4c entre regularização e `apagar`, mas a ordem ainda falha em caminhos alcançáveis.** A regularização pode travar uma Pergunta no `post_save` e depois adquirir a trava de um Chamado no encerramento do lote. A nova trava do Colaborador também fecha um ciclo com o flip automático sobre a mesma Batida. `app/chamados/services/regularizacao_ext.py:122–123,153–157,203–207` · `app/chamados/reconciliador.py:673–694` · `app/chamados/models.py:773–777` · `app/ponto/management/commands/flip_automatico.py:62–68`
2. **A cura de B2 está no código. Falta confirmar o RED executável.** Nesta cópia, tanto o teste da versão atual quanto o mutante pararam no import de `django.db`. O mutante retornou `RC=2`, `FAIL=0`, `ERROR=1`. Sua queda por asserção fica **[nao verificado]**. `app/ponto/registro_batida.py:328–341` · `app/ponto/tests/test_juiz_unico_eventos.py:318–360` · `app/ponto/tests/juiz_unico_eventos_mutantes.py:225–232`

## Curas e mutantes

| Item | Sítio e teste que morde | Resultado na cópia | [nao verificado]
|---|---|---|
| **B1** | Trava no topo de `app/chamados/services/regularizacao_ext.py:122–124`. O teste espiona a ordem de entrada no `atomic`, trava, leitura de itens e escrita em `app/ponto/tests/test_juiz_unico_eventos.py:409–432`. | Verde na fonte atual. `f4d-b1-sem-trava` cai com **RC 1**, duas falhas por asserção, em `corrigir` e `remover`. `app/ponto/tests/juiz_unico_eventos_mutantes.py:123–125` · `app/ponto/tests/test_juiz_unico_eventos.py:431` |
| **B2** | O juiz está antes do `delete`, fora da proteção e do savepoint interno. O teste exige o mesmo `DatabaseError`, linha e PK preservados, zero trilha, agendamento, savepoint e commit, e um rollback. `app/ponto/registro_batida.py:333–341` · `app/ponto/tests/test_juiz_unico_eventos.py:318–360` | `f4d-b2-protecao-e-savepoint` restaura o bloco que engole a exceção exigida pelo teste. A execução parou antes dessa asserção. **[nao verificado]** `app/ponto/tests/juiz_unico_eventos_mutantes.py:126–134` · `app/ponto/tests/test_juiz_unico_eventos.py:333,351` |
| **B3** | INFO passou a carregar `processados=%d`. O teste exige `processados=0` e uma só chamada para dois dias da mesma competência. `app/ponto/services/fechamento.py:1841–1845` · `app/ponto/tests/test_juiz_unico_eventos.py:474–483` | Verde na fonte atual. `f4d-b3-info-antigo` cai com **RC 1** na mensagem. O RED de K3, que exige nova tentativa quando uma exceção sobe ao callback, também cai com **RC 1**. `app/ponto/tests/juiz_unico_eventos_mutantes.py:135–141,171–175` · `app/ponto/tests/test_juiz_unico_eventos.py:462–483` |
| **E5** | A promessa do cron saiu do comentário. A configuração da cópia declara que `recalcular_fechamento` fica sem cron. `app/ponto/registro_batida.py:192–194` · `app/config/crons.py:1148–1155` | Verde na fonte atual. `f4d-e5-comentario-antigo` cai com **RC 1** na asserção sobre o comentário. `app/ponto/tests/juiz_unico_eventos_mutantes.py:142–146,206–210` · `app/ponto/tests/test_juiz_unico_eventos.py:485–489` |

## B1 — Ordem dentro do `atomic`

- **Sim, é a primeira aquisição de trava de linha daquele `atomic`.** Com os argumentos usados, `trava_na_ordem` pede só o Colaborador. No par original, regularização e `apagar` agora pedem o mesmo Colaborador antes de escrever ou travar a Batida. A consulta que veta disputa aberta ainda roda antes da trava. Uma abertura concorrente pode passar entre a consulta e a aquisição, e o materializador segue sem reler. `app/chamados/services/regularizacao_ext.py:59–66,122–124` · `app/chamados/models.py:2901–2902,2933–2967` · `app/ponto/registro_batida.py:328–336` · `app/chamados/services/disputa_emissao.py:530–542,601–606`
- **Sequência interna.** `corrigir` faz `UPDATE` direto. `adicionar` delega o `INSERT` a `registrar_batida`. Os dois passam pelo `post_save`, que pode travar Disputas, Chamados e Perguntas via reconciliador. `remover` faz `delete` direto e aciona núcleo e cartório pelo `post_delete`. No fim, `transicionar_com_pauta(..., ato=reg.chamado.encerrar)` tenta travar explicitamente o Chamado do lote. Se o `post_save` já travou uma Pergunta e o Chamado do lote era terminal e estava fora do conjunto travado, a aquisição nova é **Pergunta → Chamado**. O teste de B1 usa escritas simuladas e `chamado_id=None`. `app/chamados/services/regularizacao_ext.py:73–83,133–157,203–207` · `app/ponto/registro_batida.py:178–182` · `app/ponto/signals.py:53–55,148–150,216–225` · `app/chamados/reconciliador.py:673–694` · `app/chamados/services/fecho_runtime.py:157–168` · `app/chamados/models.py:773–777,1785–1793,2952–2966` · `app/ponto/tests/test_juiz_unico_eventos.py:417–432`
- **Contraexemplo com dois atos.** T1, `flip_automatico --apply`, abre o `atomic` e atualiza a Batida B via `flip_tipo` → `anotar`. T2, a regularização, trava o Colaborador C e espera B ao tentar corrigir a mesma linha. T1 entra no reconciliador pelo `post_save` e espera C se houver pergunta elegível. Uma pergunta de disputa já fechada satisfaz o seletor, portanto esse cenário é compatível com a guarda da regularização. O ciclo é **T1 segura B e espera C, T2 segura C e espera B**. Execução em PostgreSQL e ocorrência em produção **[nao verificado]**. `app/ponto/management/commands/flip_automatico.py:62–68` · `app/ponto/services/flip_batida.py:25–33` · `app/ponto/registro_batida.py:281–287` · `app/ponto/signals.py:216–225` · `app/chamados/reconciliador.py:673–679,740–762,1580` · `app/chamados/services/regularizacao_ext.py:59–66,122–123,153–157`
- **Há também um ciclo possível com batida de chão.** A regularização segura C em `FOR UPDATE`. O app pode reservar no `INSERT` a chave única `(C, timestamp)` e esperar o `KEY SHARE` do FK em C. Se a regularização tenta corrigir outra Batida para esse mesmo instante, espera a chave ocupada pelo app. A hora válida do aparelho pode coincidir com a hora corrigida. Execução desse ciclo e efeito em produção **[nao verificado]**. `app/chamados/models.py:2938–2941` · `app/chamados/services/regularizacao_ext.py:122–123,144–157` · `app/api/views_core.py:492–495,643–659` · `app/api/hora_do_aparelho.py:135–146` · `app/ponto/registro_batida.py:178–180` · `app/ponto/models.py:22,102–106` · `app/colaboradores/tests/test_feriado_matriz_p3_trava.py:5–10`

## B2 — Chamadores e exceção

- **Produção.** O único chamador encontrado é o POST administrativo `rejeitar` de `batidas_offline_pendentes`, protegido por `ver_painel_situacional`. O `except Batida.DoesNotExist` cobre a consulta inicial. O erro do juiz sai da view antes da resposta de sucesso. Esse ato é a rejeição administrativa de marca offline, e nenhum chamador de chão foi encontrado para `apagar`. A resposta HTTP observada em produção é **[nao verificado]**. `app/ponto/urls.py:78` · `app/ponto/views.py:452–456,467–482,496–500` · `app/docs/LEIS.md:78`
- **Testes.** O chamador em `test_juiz_unico_eventos.py:352` captura `DatabaseError` com `assertRaises`. Os demais deixam uma exceção inesperada falhar o teste. `app/ponto/tests/test_juiz_unico_eventos.py:273,305,314,351–352,368,401` · `app/ponto/tests/test_porta_batida.py:80–81`
- **Limite da proteção.** Se a leitura viva falha, o erro sai do `atomic` antes do `delete`, da trilha e da limpeza do PK. Depois do `delete`, a união dos dias e o agendamento continuam sob `_proteger_fechamento_por_evento`. O callback roda em `on_commit` e conserva seus `except`. O `delete` e seus receivers ficam fora desse guard. `app/ponto/registro_batida.py:197–204,328–343` · `app/ponto/services/fechamento.py:1830–1856`

## B3 — Retorno e log

Com os dois defaults `False`, **todo retorno normal é `int`**. O contador começa em `0`, é incrementado com `1` e é o único retorno desse modo. Universo vazio, guarda que exclui o colaborador ou exceção por colaborador antes do incremento podem deixar `0`. O dicionário pertence ao modo leitura. O `%d` recebe o contador em argumentos separados do formato, com a competência já marcada. Falhas internas de `DiaPago` ainda podem coexistir com `processados=1`, e o `except` mensal registra PK e nome quando falha no nível do colaborador. `app/ponto/services/fechamento.py:468–471,574–584,636–639,1152–1160,1262–1265,1321–1335,1841–1845`

## AST — `except` e `suppress`

Nenhum novo. Contagem de `ast.ExceptHandler` e chamadas a `suppress`, da base do delta para a cópia. Em B2 saiu um **uso** de `_proteger_fechamento_por_evento`, cujo `except` segue atendendo os outros usos. `app/ponto/registro_batida.py:197–212,333–340` · `app/ponto/tests/juiz_unico_eventos_mutantes.py:126–134`

| Arquivo | `except` | `suppress` |
|---|---:|---:|
| `app/chamados/services/regularizacao_ext.py:104,112,158,191` | 4 → 4 | 0 → 0 |
| `app/ponto/registro_batida.py:18,200` | 2 → 2 | 0 → 0 |
| `app/ponto/services/fechamento.py:594,1105,1111,1262,1321,1327,1685,1846,1854` | 9 → 9 | 0 → 0 |
| `app/ponto/tests/juiz_unico_eventos_mutantes.py:82,189` | 0 → 0 | 0 → 0 |
| `app/ponto/tests/test_juiz_unico_eventos.py:318–360` | 0 → 0 | 1 → 0 |

## Classes e escritores/leitores do delta

**Ordem de trava** é a classe que reprova. O selo de B1 morde a remoção da chamada local, com alcance limitado aos dois ramos simulados. A escrita direta de correção e remoção fora da porta já existia. A correção de tipo e horário está registrada como classe 9. O delta reutiliza o juiz de evento, a competência do fato e o escritor mensal, sem introduzir relógio ou derivação paralela. `processados` tem um significado de contagem, mesmo com várias causas possíveis para zero. `app/ponto/tests/test_juiz_unico_eventos.py:409–432` · `app/chamados/services/regularizacao_ext.py:133–157` · `app/core/portas.py:98–105` · `app/docs/CORTES.md:441` · `app/ponto/registro_batida.py:333–340` · `app/ponto/services/fechamento.py:637,1326–1335,1830–1845`

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `RegularizacaoExterna.colaborador_id` e sentinela da trava | `criar_regularizacao_externa` grava o FK. Guarda, criação de Batida, trilha e nova trava leem a relação ou seu ID. `trava_na_ordem` registra a aquisição na conexão, lida por `colaboradores_travados` e `estampa_das_travas`. `app/chamados/services/regularizacao_ext.py:59–65,73–82,98,123,238–241` · `app/chamados/models.py:2938–2941,4592–4595` · `app/chamados/services/trava_do_colaborador.py:26–28,37–64,72–79` |
| `Batida.pk`, `colaborador_id`, `timestamp`, `tipo` | O `create` da porta gera a linha e o PK. A correção externa também grava `timestamp` e `tipo`. A view lê a marca para a trilha. `apagar` guarda o PK, relê a linha e entrega a marca viva ao juiz. Após sucesso, limpa o PK da instância recebida. `app/ponto/registro_batida.py:172–182,318–343` · `app/chamados/services/regularizacao_ext.py:151–157` · `app/ponto/views.py:474–489` · `app/ponto/turnos.py:3037–3047` |
| `.dias`, `dias_vivos`, `_dias_exclusao`, `dias_da_jornada` | `julgar_evento_da_batida` produz `.dias`. `apagar` guarda os dias vivos. O núcleo escreve `_dias_exclusao` no `post_delete`, e o cartório lê ou preenche pelo mesmo juiz. A porta une os dias e o callback consome a união, inclusive vazia. `app/ponto/turnos.py:3034–3057` · `app/ponto/signals.py:19–25,53–55,113–122,148–150` · `app/ponto/registro_batida.py:333–340` · `app/ponto/services/fechamento.py:1830–1841` |
| `motivo`, competência e `processados` | `apagar` fornece `motivo='exclusao'`. O resolvedor fornece mês e ano para a deduplicação, a chamada mensal e o log. O escritor mensal produz a contagem, lida agora pelo INFO e já lida pela tela e pelo comando. `app/ponto/registro_batida.py:340` · `app/ponto/services/fechamento.py:1618–1628,637,1326–1335,1830–1850` · `app/ponto/views.py:1280–1281` · `app/ponto/management/commands/recalcular_fechamento.py:130–134` |
| Texto de E5 | O comentário da porta é lido pelo teste via `Path.read_text`. O mutante injeta a versão antiga nessa leitura. `app/ponto/registro_batida.py:187–196` · `app/ponto/tests/test_juiz_unico_eventos.py:485–489` · `app/ponto/tests/juiz_unico_eventos_mutantes.py:142–146,206–210` |

## 26/11 17:40 · sessao 85c92057ec44 · O30-F3-CONSTRUIR · construtor · origem=Code

A c11 está aplicada na cópia. `_reabrir` agora avisa sobre possível anexo órfão se a execução falhar após gravar o upload sob `atomic` externo, inclusive por `KeyboardInterrupt`. A marca de sucesso é definida após a saída do `with`, e o teste de sucesso exige silêncio. O selo AST de `_foto_no_pdf` exige um único tratador com `(UnidentifiedImageError, OSError)`. [ausencia.py:1435](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1435) · [test_atestado_f3_contrato.py:241](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:241) · [test_atestado_f3_contrato.py:509](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:509) [nao verificado]

Da pasta `app/`, os comandos da tabela usam o executor de mutantes em memória. [mutantes_atestado_f3.py:365](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:365)

```bash
f3() {
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 "$@"
}
```

| Item | RED | GREEN | Mutantes |
|---|---|---|---|
| 1. Aviso em `_reabrir` | `f3 red_c10_reabrir_keyboard_interrupt` falhou na base com `assertLogs`, rc 1. [Teste:524](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:524) | `f3 green_c11` passou os 12 testes, incluindo `KeyboardInterrupt`, `RuntimeError` e sucesso sob `atomic` externo, rc 0. [Executor:404](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:404) | `f3 c11_reabrir_sem_marca_de_sucesso` caiu em `assertNoLogs`. `f3 c11_reabrir_sem_finally` caiu em `assertLogs`. Ambos rc 1. [Mutantes:125](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:125) | [nao verificado]
| 2. Tipo do tratador do PDF | `f3 c11_pdf_handler_amplo` passou com o selo c10 antes do reforço, rc 0. [Mutante:131](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:131) | `f3 green_c11` passou com o par de exceções especificado, rc 0. [Teste:241](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:241) | `f3 c11_pdf_handler_amplo` caiu na asserção que exige a tupla, rc 1. [Teste:244](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:244) | [nao verificado]

**Contagem AST c10 → c11**

| Arquivo | `ExceptHandler` | `suppress` |
|---|---:|---:|
| [ponto/services/ausencia.py:1436](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1436) | 13 → 13 | 0 → 0 |
| [ponto/services/atestados_lote.py:151](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/atestados_lote.py:151) | 3 → 3 | 0 → 0 |
| [ponto/tests/test_atestado_f3_contrato.py:528](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:528) | 0 → 0 | 0 → 0 |
| [ponto/tests/mutantes_atestado_f3.py:117](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:117) | 0 → 0 | 0 → 0 |

O chamador do PDF de lote é `lote_atestados`, em `ponto/views_atestado.py`. Pillow e PDF reais ficam para a suíte da obra [nao verificado]. [views_atestado.py:136](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views_atestado.py:136) · [test_atestado_f3_contrato.py:832](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:832)

**SHA.** O `HEAD` continua em `119aaa8bf8870c7eef8bbe3ecd76f703992b994e`. A entrega ficou no diff da cópia porque as regras vedam `git commit` e a operação escreveria fora de `app/`. SHA de commit c11 [nao verificado]. [mutantes_atestado_f3.py:25](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:25)

## 26/11 17:39 · sessao 65c5ec36379d · - · - · origem=Ronald (tmux)

**B1 corrige o ciclo original entre regularização e `apagar`, mas ainda há inversão de trava.** A nova chamada é a primeira instrução do atomic da materialização e trava apenas o Colaborador. A exclusão do DP também começa pelo Colaborador antes da Batida. O teste passou na cópia e o mutante caiu com RC 1, dois `FAIL` e zero `ERROR`. O teste usa mocks de trava e escrita, cobre corrigir e remover e passa `chamado_id=None`. `app/chamados/services/regularizacao_ext.py:122-124`, `app/chamados/models.py:2901-2902,2938-2967`, `app/ponto/registro_batida.py:328-336`, `app/ponto/tests/test_juiz_unico_eventos.py:409-432`, `app/ponto/tests/juiz_unico_eventos_mutantes.py:123-125`.

**Contraexemplo com dois atos.** O `flip_automatico --apply` entra no atomic e `flip_tipo` chama `anotar`, cujo UPDATE segura a Batida B. A regularização concorrente do mesmo Colaborador C segura C pelo delta e espera B ao corrigir. O `post_save` do flip chama o reconciliador, que tenta C quando há pergunta elegível. Uma pergunta de disputa já fechada satisfaz essa condição sem acionar a guarda de disputa aberta. A agenda da cópia contém esse comando. Ocorrência em produção [nao verificado]. `app/config/crons.py:707`, `app/ponto/management/commands/flip_automatico.py:51-68`, `app/ponto/services/flip_batida.py:21-32`, `app/ponto/registro_batida.py:264-287`, `app/chamados/services/regularizacao_ext.py:59-66,122-123,153-157`, `app/ponto/signals.py:216-225`, `app/chamados/reconciliador.py:651-653,673-694,740-762,1580`.

**Dentro do atomic.** Corrigir e adicionar executam `post_save` e podem travar o universo Disputa/Chamado/Pergunta no reconciliador. Remover executa os `post_delete` do núcleo e do cartório. O `transicionar_com_pauta` executa o `encerrar` do chamado do lote, que pede C, D abertas e o próprio Ch antes de decidir. Se o `post_save` já travou uma P e o chamado do lote é terminal e está fora dos destinos previamente travados, `encerrar` pode adquirir esse Ch pela primeira vez depois de P. A posse de C serializa esse exemplo com atos que também respeitam C primeiro. `app/chamados/services/regularizacao_ext.py:78-83,138,157,207`, `app/ponto/registro_batida.py:172-182`, `app/ponto/signals.py:53-55,141-150,216-225`, `app/chamados/reconciliador.py:686-694`, `app/chamados/services/fecho_runtime.py:157-168`, `app/chamados/models.py:773-777,1785-1793,2943-2966`, `app/chamados/catalogo/motor.py:24-31`, `app/chamados/services/acoes_chamado.py:322-329`.

A guarda de disputa ocorre antes da nova trava. Outro ato pode abrir D sob C entre a leitura da guarda e a aquisição de C pela regularização, que prossegue sem reler. Isso já existia no trecho do anexo. `app/chamados/services/regularizacao_ext.py:59-66,122-124`, `app/chamados/services/disputa_emissao.py:530-542,601-606`.

O `colaborador_id` novo é o FK gravado na criação do lote. A chamada também registra um sentinela de trava na fila da conexão, lido pelas portas que exigem ou identificam a trava corrente. `app/chamados/models.py:4592-4595,2939-2941`, `app/chamados/services/regularizacao_ext.py:238-241`, `app/chamados/services/trava_do_colaborador.py:26-28,37-64,72-80`.

O teste e o mutante B2 pararam no import de `django.db` no ambiente somente leitura. O mutante saiu RC 2 com `FAIL=0 ERROR=1`. Queda por asserção [nao verificado]. `app/ponto/tests/test_juiz_unico_eventos.py:333`, `app/ponto/tests/juiz_unico_eventos_mutantes.py:228-232`.

## 26/11 17:39 · sessao 715c4520f925 · - · - · origem=Ronald (tmux)

**B2 curado no código.** `apagar` trava e relê a batida, chama o juiz antes do `delete` e deixa o `DatabaseError` sair do `atomic`. A falha interrompe o ato antes do delete, agendamento, trilha e limpeza do PK. O agendamento pós-delete mantém `_proteger_fechamento_por_evento`; o recálculo roda no `on_commit` e conserva seus `except`. [app/ponto/registro_batida.py:318–343] [app/ponto/registro_batida.py:197–204] [app/ponto/services/fechamento.py:1830–1856]

- O único caller de produção encontrado é o POST `rejeitar` da tela offline. O `except Batida.DoesNotExist` cobre a consulta inicial. A exceção de `apagar` sai da view e impede a resposta `ok=True`. É um ato administrativo protegido por `ver_painel_situacional`, permissão que também pode alcançar supervisão e TI. Resposta HTTP observada em produção [nao verificado]. [app/ponto/urls.py:78] [app/ponto/views.py:452–456,467–482,496–500] [app/colaboradores/mixins.py:322–331,355–377] [app/docs/LEIS.md:78]
- Callers de teste em `test_porta_batida.py:80,81` deixam exceções subir. Em `test_juiz_unico_eventos.py:273,305,314,368,401` também. O caller de `:352` captura com `assertRaises(DatabaseError)`, afirma identidade, preservação das linhas e PK, zero trilha/savepoint/commit/on_commit e um rollback. O teste e o mutante `f4d-b2-protecao-e-savepoint` pararam no import de `django.db` nesta cópia. O mutante anunciou `FAIL=0 ERROR=1`, RC 2. A mordida em execução permanece [nao verificado]. Por inspeção, o mutante engole a exceção que o `assertRaises` exige. [app/ponto/tests/test_porta_batida.py:77–83] [app/ponto/tests/test_juiz_unico_eventos.py:318–360] [app/ponto/tests/juiz_unico_eventos_mutantes.py:126–134,225–232] [nao verificado]

**Contraexemplo adicional para B1.** T1 regulariza e mantém `FOR UPDATE` no colaborador C. T2 bate pelo app no mesmo instante que T1 vai registrar ou usar como novo horário de uma correção. T2 chega primeiro ao INSERT de Batida e reserva a chave única `(C, timestamp)`, depois pede `FOR KEY SHARE` de C pelo FK. T1 chega ao INSERT ou UPDATE sobre a mesma chave e aguarda T2. O código aceita a mesma chave temporal nas duas portas. O conflito `FOR UPDATE` × `KEY SHARE` está documentado no teste equivalente de Posto/FK. Execução do ciclo específico de Batida e ocorrência em produção [nao verificado]. [app/chamados/services/regularizacao_ext.py:73–83,122–123,144–157] [app/chamados/models.py:2939] [app/api/views_core.py:483–495,643–659] [app/api/hora_do_aparelho.py:135–146] [app/ponto/registro_batida.py:133–148,178–180] [app/ponto/models.py:22,102–108] [app/colaboradores/tests/test_feriado_matriz_p3_trava.py:5–10] [nao verificado]

## 26/11 17:36 · sessao 2d4c89ae68b2 · O30-F3-CONSTRUIR · verificador · origem=Code

**REPROVADO no pré-verificador c10.**

1. **Falta a prova com Pillow e PDF reais do item 1.** O verde e o mutante real pararam no `import PIL`. A queda do mutante com mock foi confirmada. O resultado com as dependências reais fica [nao verificado]. `test_atestado_f3_contrato.py:821-850` · `mutantes_atestado_f3.py:120-125,387-390` [nao verificado]
2. **A c10 perdeu o aviso em uma falha ocorrida dentro da própria reabertura.** Com upload já gravado, `KeyboardInterrupt` em `mudar_estado` sob `atomic` externo gerou o aviso na c9 reconstruída e saiu em silêncio na c10. Reproduzi em memória. Considerei essa saída excepcional parte de “falha loga”, porque ela ocorre antes do retorno e também pode deixar o anexo órfão. `ausencia.py:1413-1428,1434-1445` · `mutantes_atestado_f3.py:134-136`

Os quatro blobs da cópia conferem com o `delta.patch` fornecido. Os dois arquivos de serviço reconstruídos em memória também conferem com os blobs c9 do patch, e os dois REDs correspondentes falharam nas asserções esperadas. A confirmação dos hashes de **commit** e a ocorrência dos contraexemplos em produção ficam [nao verificado]. `mutantes_atestado_f3.py:92-93,391-395` · `test_atestado_f3_contrato.py:228-240,503-507` [nao verificado]

### 1. Os quatro itens do Opus

| Item | Sítio e teste que morde | Resultado | [nao verificado]
|---|---|---|
| 1. `load()` explícito e JPEG truncado | `atestados_lote.py:147-152` · `test_atestado_f3_contrato.py:801-815,821-850` | Código e teste real presentes. O mutante composto sem `load()` e com transposição fora do `try` caiu no mock com `OSError: truncado`. Retirar **só** o `load()` deixou o teste comportamental verde e derrubou o selo AST. A transposição do mock também chama `load()`. Mutante real [nao verificado]. `mutantes_atestado_f3.py:108-125` · `test_atestado_f3_contrato.py:239-240,763-765` | [nao verificado]
| 2. Corpo exato do `try` | `atestados_lote.py:147-158` · `test_atestado_f3_contrato.py:228-240` | Curado. `thumbnail` dentro do `try` derrubou a igualdade AST. `mutantes_atestado_f3.py:126-130` |
| 3. Uma marca e rodapé igual | `atestados_lote.py:201-214,225-232` · `test_atestado_f3_contrato.py:809-815,844-850` | Curado. A marca duplicada caiu com `2 != 1`. O mutante que zera o incremento também caiu, com rodapé `0` para uma marca. `mutantes_atestado_f3.py:131-133,140-142` |
| 4. Aviso na falha sob `atomic` externo | `ausencia.py:1434-1445` · `test_atestado_f3_contrato.py:503-538` | Curado para `Exception`. O mutante que repõe o `finally` caiu em `assertNoLogs`, e o que remove o aviso caiu em `assertLogs`. A saída por `BaseException` ficou sem aviso, conforme o bloqueio 2. `mutantes_atestado_f3.py:134-139` |

### 2. `_foto_no_pdf` e o rodapé

- O tratador cobre `Image.open`, a entrada e a saída do `with`, `original.load()` e `exif_transpose`, para `UnidentifiedImageError` e `OSError`. `thumbnail`, `save`, a construção da imagem do PDF e a abertura do documento no storage ficam fora. `atestados_lote.py:147-158,168-175`
- **Um anexo inacessível ainda pode derrubar o lote.** Contraexemplo reproduzido com mock: `exists()` devolve `True` e `documento.open('rb')` levanta `FileNotFoundError`, como na janela em que o arquivo some entre as chamadas. A exceção chega à montagem do PDF. Se `exists()` já devolve `False`, sai `Anexo indisponível`, com incremento zero. `atestados_lote.py:168-178,210-216` · `views_atestado.py:134-138`
- **Uma imagem decodificada pode virar `ILEGIVEL`.** No contraexemplo em memória, `load()` e a transposição completam, mas a saída do `with` levanta `OSError`. O resultado foi uma marca, rodapé `1` e nenhuma imagem embutida. Um anexo de tipo desconhecido segue como link, inclusive quando a leitura do cabeçalho falha e devolve `(None, None)`. `atestados_lote.py:147-152,171-178,211-214` · `ausencia.py:796-817`
- **O rodapé conta exatamente as marcas geradas pelo ramo `anexo is None`**, uma por passagem, e repete o total em cada página. Uma busca textual tem outro universo: `atestado_medico='ANEXO ILEGIVEL'` imprime a expressão no texto clínico mesmo com uma foto boa e rodapé `0`. `atestados_lote.py:201-214,225-232` · `forms.py:13-15`

### 3. `_reabrir` e o rollback do chamador

O aviso específico exige uma `Exception` que chegue ao tratador, upload detectado e `conexao.in_atomic_block` ainda verdadeiro. Sem atomic externo, o ramo tenta compensar e emite outro aviso. Em sucesso, `_reabrir` devolve a instância com `_criada=False` e `_reaberta=True`, sem emitir o aviso de órfão. `ausencia.py:1406-1421,1434-1445`

**Contraexemplo reproduzido em memória:** o chamador abre um atomic, recebe o retorno de `_reabrir` e só então levanta `RuntimeError`. O rollback restaura a referência anterior no banco simulado. A instância em memória ainda aponta para o nome novo, e o arquivo novo fica no storage. Sem outra referência ou compensação do chamador, sobra um órfão sem aviso de `_reabrir`. O teste de sucesso c10 para antes desse rollback. `ausencia.py:1413-1421,1434-1445` · `test_atestado_f3_contrato.py:409-431,503-513` [nao verificado]

**INSS tem os dois caminhos.** Se a falha vem depois do retorno, a view lê o nome restaurado e tenta compensar. Se `_reabrir` levanta antes de devolver, a atribuição de `ausencia` nunca completa, e a view pula essa limpeza. Para `Exception` com upload detectado, fica o aviso da porta. `views.py:2563-2579,2588-2601` · `ausencia.py:249-264,1434-1442`

### 4. Contagem AST c9 → c10

| Arquivo | `ExceptHandler` | `suppress` |
|---|---:|---:|
| `atestados_lote.py:151,263,271` | 3 → 3 | 0 → 0 |
| `ausencia.py:266,605,611,643,807,845,1132,1307,1345,1434,1511,1560,1818` | 13 → 13 | 0 → 0 |
| `mutantes_atestado_f3.py:101-139,192-196,350-370` | 0 → 0 | 0 → 0 |
| `test_atestado_f3_contrato.py:51-72,228-240,503-538,801-850` | 0 → 0 | 0 → 0 |

O `finally` externo saiu. O `finally` interno que registra a gravação do upload continua. Em `mutantes_atestado_f3.py`, `suppress` aparece como texto de mutante. `ausencia.py:1416-1421,1434-1445` · `mutantes_atestado_f3.py:192-196`

### 5. Classes já reprovadas

- **Sem ocorrência nova no delta** de segundo juiz, segundo escritor fora da porta, relógio fora da borda, derivação paralela, ordem de trava ou campo persistido com dois significados. A classificação segue em `tipo_real_documento`, a reabertura segue usando `estado_inicial` e `mudar_estado`, e a trava da gravação dos seis campos segue na mesma função. `atestados_lote.py:147-175` · `ausencia.py:301-324,796-817,1403-1433`
- **Selo/RED:** os mutantes pedidos que puderam rodar morderam. O selo novo tem uma lacuna verificável: trocar o tipo do handler por `except Exception` deixou verdes o selo do corpo, o censo de tratadores e o teste de decode com mock. O selo compara o corpo, e o censo conta tratadores. A c10 efetivamente entregue conserva o par de exceções. `atestados_lote.py:151` · `test_atestado_f3_contrato.py:51-72,222-240,801-815` [nao verificado]

### 6. Quem escreve e quem lê

| Campo ou valor | Quem escreve ou produz | Quem lê ou consome |
|---|---|---|
| `Ausencia.documento`, nome e bytes | Upload entra pela view. `criar_ausencia`, `anexar_documento` e `_reabrir` passam o documento ao `save` do `FileField`. `views.py:2525-2538` · `models.py:777-781` · `ausencia.py:571-572,638-642,1118-1120,1413-1421` | Histórico lê a presença. PDF lê nome, disponibilidade e bytes. ZIP lê o original. A rota de anexo abre o documento da raiz. Reabertura e compensação leem o nome e suas referências. `dia_decidido.py:679-687` · `atestados_lote.py:168-175,249-261` · `views.py:2146-2161` · `ausencia.py:249-264,1405` |
| `linha['ausencia']`, `linha['documento']`, `registro.pk`, `pode_ver_anexo` | O ORM fornece a ausência e o ID. `linha_do_tempo` monta a linha e a flag booleana. A view calcula a permissão. `atestados_lote.py:36-49,58-63` · `dia_decidido.py:679-687` · `views_atestado.py:99-105` | PDF usa o ID no resumo, link e marca, e as duas flags para decidir se lê o anexo. A reabertura usa o mesmo ID no aviso. `atestados_lote.py:176-178,190-216` · `ausencia.py:1437-1441` |
| `arquivo`, `tipo`, `original`, `foto`, retorno `None` | `_anexo_no_pdf` abre o stream. `tipo_real_documento` fornece o tipo pelos bytes. Pillow abre, carrega e transpõe. `_foto_no_pdf` produz a imagem ou `None`. `atestados_lote.py:147-158,168-175` · `ausencia.py:789-817` | `_foto_no_pdf` usa o tipo para escolher JPEG ou PNG. `_anexo_no_pdf` propaga `None`. `pdf_bytes` lê esse valor para gerar a marca e incrementar o contador. `atestados_lote.py:153-158,172-175,210-216` |
| Marca, `anexos_ilegiveis` e bytes do PDF | `pdf_bytes` produz a marca e incrementa no mesmo ramo. O rodapé imprime o total. `atestados_lote.py:201-214,225-234` | ReportLab consome os blocos e o callback. A view envia os bytes. Os testes leem a marca, a imagem e o rodapé. `atestados_lote.py:231-234` · `views_atestado.py:134-150` · `test_atestado_f3_contrato.py:809-815,844-850` | [nao verificado]
| `banco`, `in_atomic_block`, `_committed`, `_nome_antes`, `anexo_gravado` | O router fornece o alias. A conexão acompanha o atomic. O `save` do `FileField` grava o upload, e `_reabrir` captura o nome anterior e a dupla `(storage, nome gravado)`. `ausencia.py:1405-1421` · `views.py:2569-2579` | `_reabrir` lê a dupla e o estado da conexão para escolher compensação ou aviso. A compensação lê o nome anterior e consulta as referências ao nome gravado. `ausencia.py:249-264,1434-1442` |
| Aviso e flags de retorno | `_reabrir` emite o aviso pelo logger `ponto` e marca a instância no caminho de sucesso. `ausencia.py:49,1439-1445` | A configuração SaaS encaminha o logger ao handler. Os testes capturam o aviso e leem `_reaberta`. A API e `garantir_ausencia` leem `_criada`. `saas.py:160-166,181` · `test_atestado_f3_contrato.py:503-538` · `api/views.py:1852-1858` · `ausencia.py:690-691` |
| Seis campos `atestado_*` e `documento_referencia_id` | O formulário fornece os seis valores. A criação grava no INSERT. Edição, anexação e reabertura passam pelo gravador comum. O vínculo é declarado no modelo. `forms.py:13-22` · `views_atestado.py:31-50` · `ausencia.py:301-324,567,640-642,744-746,1114-1115,1409-1411` · `models.py:815-819` | O gravador lê os valores atuais e a referência para recusar a gravação na filha. PDF, CSV e painel leem os seis campos. O vínculo também é lido pela rota que serve o anexo. `ausencia.py:292-324` · `atestados_lote.py:104-108,122-138,205-209` · `painel_ausencia.html:100-131` · `views.py:2146-2149` |

## 26/11 17:35 · sessao 593de0114757 · - · - · origem=Ronald (tmux)

A quarta cura funciona para `Exception` dentro de `_reabrir` quando houve upload e a conexão continua em `atomic`. Os dois mutantes c10 do aviso caíram nos asserts de sucesso e falha, respectivamente. O `green_c10` passou os quatro testes. `app/ponto/services/ausencia.py:1413-1445`, `app/ponto/tests/test_atestado_f3_contrato.py:503-538`, `app/ponto/tests/mutantes_atestado_f3.py:134-139,387-388`.

**Regressão nova.** Injetei `KeyboardInterrupt` em `mudar_estado` após o upload, com rollback externo simulado. O novo arquivo ficou sem referência e sem aviso na c10. O mesmo caso, com o `finally` da c9 restaurado em memória, emitiu o aviso. A diferença vem do alcance de `except Exception`. Alcance em produção [nao verificado]. `app/ponto/services/ausencia.py:1417-1442`, `app/ponto/tests/test_atestado_f3_contrato.py:409-431`, `app/ponto/tests/mutantes_atestado_f3.py:134-136`.

**Rollback posterior.** Também reproduzi em memória o retorno bem-sucedido seguido de exceção no chamador. O rollback restaura a referência antiga. A instância devolvida ainda aponta ao nome novo e tem `_reaberta=True`. Sem compensação do chamador, o novo arquivo fica sem referência e sem aviso. No caminho INSS, o chamador tenta compensar se recebeu a instância antes de falhar. Se `_reabrir` falhou antes do retorno, `ausencia` continua `None` e essa compensação é pulada. `app/ponto/services/ausencia.py:1413-1445`, `app/ponto/views.py:2564-2601`.

**Selo do “só”.** Removi em memória apenas `and conexao.in_atomic_block` do aviso. Os oito testes V1 passaram mesmo com avisos falsos em falhas sem `atomic` externo. Os testes novos mordem o aviso no sucesso externo e sua falta na falha externa. `app/ponto/services/ausencia.py:1435-1441`, `app/ponto/tests/test_atestado_f3_contrato.py:433-451,503-556`, `app/ponto/tests/mutantes_atestado_f3.py:67-74`.

**Leituras e escritas.** O upload chega pela porta de criação ou reversão. `_reabrir` lê o nome antigo e o alias do router, obtém a conexão, grava `documento` e observa a mudança de `_committed` para guardar `(storage, name)`. O `except` lê a tupla, `in_atomic_block` e o `pk`. O auxiliar lê a referência SQL e usa o storage para compensar. O logger `ponto` emite o aviso, e o handler SaaS o direciona a `ponto.log`. `_criada` é lido por `garantir_ausencia` e pela API. `_reaberta` é lido pelos testes. `app/ponto/services/ausencia.py:49,241-264,470-476,511-512,690-691,1384-1445`, `app/ponto/models.py:777-780`, `app/config/settings/saas.py:160-166,181`, `app/api/views.py:1852`, `app/ponto/tests/test_atestado_f3_contrato.py:510,535-537`.

AST c9 → c10 conforme o delta informado e a contagem da cópia, em `ExceptHandler/suppress`. `atestados_lote.py` 3/0 → 3/0. `ausencia.py` 13/0 → 13/0. `mutantes_atestado_f3.py` e `test_atestado_f3_contrato.py` 0/0 → 0/0. O `finally` removido é o externo. `app/ponto/services/atestados_lote.py:151,263,271`, `app/ponto/services/ausencia.py:266,605,611,643,807,845,1132,1307,1345,1434,1511,1560,1818`, `app/ponto/tests/mutantes_atestado_f3.py:99-139,350-419`, `app/ponto/tests/test_atestado_f3_contrato.py:51-72,228-240,503-538,801-850`.

## 26/11 17:35 · sessao 867e29256908 · - · - · origem=Ronald (tmux)

- **Load explícito** em `app/ponto/services/atestados_lote.py:147–152`. O teste sem dependências `app/ponto/tests/test_atestado_f3_contrato.py:801–815` derrubou o mutante composto de `app/ponto/tests/mutantes_atestado_f3.py:108–125` com `OSError: truncado`. Remover só `load()` derrubou o selo estrutural na linha 239, enquanto o teste funcional ficou verde porque o mock de `exif_transpose` também chama `load()` na linha 764. O teste real e seu mutante pararam no import de PIL na linha 822. Comportamento com Pillow real [nao verificado].
- **Escopo do try** confirmado em `app/ponto/services/atestados_lote.py:147–158`. O `with` inclui a entrada e a saída do contexto, além de `Image.open`, `load` e `exif_transpose`. O teste em `app/ponto/tests/test_atestado_f3_contrato.py:228–240` derrubou `c10_pdf_thumbnail_no_try` de `app/ponto/tests/mutantes_atestado_f3.py:126–129`. Trocar o tipo da exceção por `Exception` sobreviveu a esse selo, ao censo de tratadores e ao PDF simulado. O selo confere o corpo e a quantidade de tratadores em `app/ponto/tests/test_atestado_f3_contrato.py:51–72,222–240`.
- **Paridade marcador e rodapé** em `app/ponto/services/atestados_lote.py:201,210–214,225–232`. O teste em `app/ponto/tests/test_atestado_f3_contrato.py:810–815` derrubou a marca duplicada e o incremento zero de `app/ponto/tests/mutantes_atestado_f3.py:131–142`. Cada parágrafo de falha tem um incremento. Um campo `atestado_medico` contendo `ANEXO ILEGIVEL` produz a ocorrência textual com total zero, como demonstrado no PDF simulado. O campo é texto livre em `app/ponto/forms.py:14` e entra em `app/ponto/services/atestados_lote.py:207–214`.
- **Contraexemplos do PDF** executados em memória com o helper de `app/ponto/tests/test_atestado_f3_contrato.py:747–790`. `exists=True`, seguido de `FileNotFoundError` no `documento.open`, aborta o lote em `app/ponto/services/atestados_lote.py:169–175,211`. Uma imagem cujo `load` e cuja transposição terminam com sucesso ainda vira `ILEGIVEL` se a saída do contexto de `Image.open` lançar `OSError` em `app/ponto/services/atestados_lote.py:147–152,174–175,211–214`. `exists=False` imprime `Anexo indisponível`, e tipo desconhecido segue para link em `app/ponto/services/atestados_lote.py:169–178`, com detecção em `app/ponto/services/ausencia.py:789–817`.
- **Aviso da reabertura** em `app/ponto/services/ausencia.py:1434–1445`. Os testes de `app/ponto/tests/test_atestado_f3_contrato.py:503–538` derrubaram os mutantes de aviso no `finally` e de aviso removido definidos em `app/ponto/tests/mutantes_atestado_f3.py:134–138`. Um chamador pode receber o objeto reaberto e depois reverter o banco, deixando o novo arquivo sem referência e sem aviso dessa função. A view INSS tem compensação própria para exceção após o retorno, mas não consegue fazê-la quando `_reabrir` levanta antes da atribuição. `app/ponto/services/ausencia.py:1412–1421,1434–1445`, `app/ponto/views.py:2564–2579,2588–2601`. O teste de integração preexistente cobre o segundo caso em `app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:164–186`, execução com banco [nao verificado].
- **AST** em c9→c10, pelo delta fornecido e AST da cópia. `app/ponto/services/atestados_lote.py:151,263,271` tem 3→3 handlers. `app/ponto/services/ausencia.py:266,605,611,643,807,845,1132,1307,1345,1434,1511,1560,1818` tem 13→13. `app/ponto/tests/mutantes_atestado_f3.py:119` e `app/ponto/tests/test_atestado_f3_contrato.py:221` têm 0→0. Zero `suppress` nos quatro.
- **Origem e leitura do PDF.** `Ausencia.documento` é `FileField` em `app/ponto/models.py:777`, escrito pela criação em `app/ponto/services/ausencia.py:571–572,608,640–641`, pelo anexo em `app/ponto/services/ausencia.py:1118–1120` e pela reabertura em `app/ponto/services/ausencia.py:1413–1421`. `dia_decidido` produz o booleano da linha em `app/ponto/services/dia_decidido.py:679–687`. O PDF lê o booleano, o `FileField`, seu nome, storage e bytes em `app/ponto/services/atestados_lote.py:168–175,210–214`. O tipo sai de `app/ponto/services/ausencia.py:789–817`, a foto sai de `app/ponto/services/atestados_lote.py:148–158`, e o renderizador consome foto, marca e contador em `app/ponto/services/atestados_lote.py:211–234`. O endpoint consome o PDF em `app/ponto/views_atestado.py:134–148`. A rota do link lê o arquivo em `app/ponto/views.py:2144–2161`.
- Os quatro testes de `green_c10` e os treze de `green_c9` passaram com o executor somente em memória de `app/ponto/tests/mutantes_atestado_f3.py:350–370,385–390,410–419`. O RED histórico autenticado por Git e a execução com Pillow real [nao verificado].

## 26/11 17:34 · sessao 0457b6427136 · DIA-DA-BATIDA-JUIZ-UNICO · construtor · origem=Code

**B2-chão fica como pergunta de lei.** A proteção do julgamento em `registrar` e `anotar` ficou fora da F4d. [registro_batida.py:207](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/registro_batida.py:207)

O diff está na cópia sobre `57aa0cb1b`. A execução local da suíte teve **25 testes sem falha e 1 erro de importação**. O RED→GREEN de B2 com o `atomic` real do Django e a conexão dublada ficou **[nao verificado]**. [test_juiz_unico_eventos.py:318](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/test_juiz_unico_eventos.py:318)

| Item | Cura | RED → GREEN e mutante |
|---|---|---|
| B1 | `trava_na_ordem` é a primeira instrução do `atomic`, antes de ler os itens e de corrigir ou remover a Batida. [regularizacao_ext.py:122](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/chamados/services/regularizacao_ext.py:122) | Faltou `call.travar_colaborador(1)` nos dois ramos da base. Ambos passaram na cópia. Mutante com RC 1, `FAIL=2 ERROR=0`. [teste:409](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/test_juiz_unico_eventos.py:409) | [nao verificado]
| B2 | A leitura viva roda no `atomic` externo, antes do `delete` e da trilha. A proteção do recálculo fica depois do `delete`. [registro_batida.py:328](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/registro_batida.py:328) | Teste invertido. Na base, na cópia e no mutante, o teste parou em `ModuleNotFoundError: No module named 'django.db'`. A sonda em memória com `sqlite3.DatabaseError` e `nullcontext`, sem conexão, observou na base `retorno=1, pks=[1], trilha=1` e na cópia `erro=DatabaseError, pks=[1, 2], trilha=0`. [teste:318](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/test_juiz_unico_eventos.py:318) | [nao verificado]
| B3 | O INFO registra a contagem devolvida pelo escritor. A prosa de K3 descreve a exceção que sobe ao callback. [fechamento.py:1817](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/services/fechamento.py:1817), [fechamento.py:1841](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/services/fechamento.py:1841) | `'processados=0' not found` na base. A cópia registrou `processados=0` em INFO com uma chamada ao escritor para dois dias da mesma competência. Mutante com RC 1, `FAIL=1 ERROR=0`. [teste:474](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/test_juiz_unico_eventos.py:474) | [nao verificado]
| E5 | Saiu do comentário a promessa de recálculo pelo cron. [registro_batida.py:192](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/registro_batida.py:192), [crons.py:1148](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/config/crons.py:1148) | `'cron da madrugada rega o gravado' unexpectedly found` na base. O teste passou na cópia. Mutante com RC 1, `FAIL=1 ERROR=0`. [teste:485](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/test_juiz_unico_eventos.py:485) | [nao verificado]

Comandos da suíte, do RED contra a base literal e dos mutantes, a partir de `app/`. O mutante histórico `f4b-a-sem-protecao` cobre a reintrodução apenas do savepoint e também parou no import de `django.db`. [juiz_unico_eventos_mutantes.py:121](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/juiz_unico_eventos_mutantes.py:121) [nao verificado]

```bash
export PYTHONDONTWRITEBYTECODE=1
python3 -m unittest ponto.tests.test_juiz_unico_eventos
python3 -m ponto.tests.juiz_unico_eventos_mutantes f4d-base
python3 -m ponto.tests.juiz_unico_eventos_mutantes f4d-b1-sem-trava
python3 -m ponto.tests.juiz_unico_eventos_mutantes f4d-b2-protecao-e-savepoint
python3 -m ponto.tests.juiz_unico_eventos_mutantes f4b-a-sem-protecao
python3 -m ponto.tests.juiz_unico_eventos_mutantes f4d-b3-info-antigo
python3 -m ponto.tests.juiz_unico_eventos_mutantes f4d-e5-comentario-antigo
```

**Contagem no diff contra a base:** `0` `except` novos e `0` `with suppress(...)` novos. A inversão de B2 removeu o `suppress` preexistente no teste. [test_juiz_unico_eventos.py:348](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/tests/test_juiz_unico_eventos.py:348) [nao verificado]

**Fora da volta:** a fila do opus sobre o escritor mensal, a corrida entre aprovar e rejeitar e a escrita direta de Batida pela regularização. Ocorrência e impacto em produção **[nao verificado]**. [fechamento.py:1326](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/services/fechamento.py:1326), [views.py:475](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/ponto/views.py:475), [regularizacao_ext.py:137](/home/ronald/wt-codex-dia-da-batida-juiz-unico-103044/app/chamados/services/regularizacao_ext.py:137)

**Mensagem sugerida**

```text
OBRA: DIA-DA-BATIDA-JUIZ-UNICO — F4d

LEI-AKITA: padrao=juiz unico, juizes novos=0
```

## 26/11 17:30 · sessao 0d996969111a · O30-F3-CONSTRUIR · construtor · origem=Code

A c10 está na cópia. `_foto_no_pdf` chama `original.load()` dentro do `with Image.open` e do `try` existente. `_reabrir` emite o aviso de possível anexo órfão quando há falha após gravar o anexo e a conexão ainda está em `atomic` externo. [atestados_lote.py:147] [ausencia.py:1434]

### RED → GREEN

Execute em `app/` com `PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 <caso>`. `green_c10` passou os quatro testes focados. O módulo de contrato passou 33 testes locais, excluído o PDF real. `selos` passou 14. A certificação fica com o Code e o opus final. [mutantes_atestado_f3.py:373] [test_atestado_f3_contrato.py:1] [nao verificado]

| Caso | Resultado local |
|---|---|
| `red_c9_pdf_try` | RED na base c9 pela ausência de `original.load()`. [test_atestado_f3_contrato.py:228] |
| `c10_pdf_sem_load_transpose_fora_do_try` | RED com `OSError: truncado` escapando no teste simulado. [mutantes_atestado_f3.py:120] [test_atestado_f3_contrato.py:801] | [nao verificado]
| `c10_pdf_thumbnail_no_try` | RED no selo AST quando `thumbnail` entra no `try`. [mutantes_atestado_f3.py:126] [test_atestado_f3_contrato.py:228] |
| `c10_pdf_marca_duplicada` e `c9_pdf_engole_sem_contar` | RED com duas marcas no primeiro mutante e rodapé `0` para uma marca no segundo. [mutantes_atestado_f3.py:131] [test_atestado_f3_contrato.py:809] |
| `red_c9_reabrir_aviso_sucesso` e `c10_reabrir_aviso_no_finally` | RED no `assertNoLogs` pelo aviso após sucesso. [mutantes_atestado_f3.py:93] [mutantes_atestado_f3.py:134] [test_atestado_f3_contrato.py:503] |
| `c10_reabrir_sem_aviso_na_falha` | RED no `assertLogs` pela ausência do aviso na falha. [mutantes_atestado_f3.py:137] [test_atestado_f3_contrato.py:515] |

**PDF real [nao verificado].** `green_c10_pdf_real` e `c10_pdf_real_sem_load_transpose_fora_do_try` pararam no import de `PIL` nesta cópia. Os dois comandos ficam para o container da suíte. O teste gera um JPEG, corta os bytes pela metade, exige que `Image.open` abra e que `load()` levante `OSError`, e confere uma marca `ANEXO ILEGIVEL #21`, rodapé `1` e a foto boa no PDF. [test_atestado_f3_contrato.py:821] [mutantes_atestado_f3.py:123] [mutantes_atestado_f3.py:389] [nao verificado]

### Diff e censo

`git diff --name-only 1e4723516 -- app/` listou os quatro arquivos abaixo. `git diff --check` passou. A contagem é por AST, de `1e4723516` para c10. [test_atestado_f3_contrato.py:49]

| Arquivo | `ExceptHandler` | `with suppress` |
|---|---:|---:|
| `app/ponto/services/atestados_lote.py` [atestados_lote.py:147] | 3 → 3 | 0 → 0 |
| `app/ponto/services/ausencia.py` [ausencia.py:1434] | 13 → 13 | 0 → 0 |
| `app/ponto/tests/mutantes_atestado_f3.py` [mutantes_atestado_f3.py:119] | 0 → 0 | 0 → 0 |
| `app/ponto/tests/test_atestado_f3_contrato.py` [test_atestado_f3_contrato.py:228] | 0 → 0 | 0 → 0 |

- **Leitores.** A rota do PDF chega a `views_atestado.lote_atestados`, que chama `pdf_bytes`. O lote lê o documento e os seis campos. `views.py` serve o documento da raiz, o painel exibe o anexo e os campos, e `anexar_atestado` coleta os valores do formulário. [relatorios/urls.py:17] [views_atestado.py:136] [atestados_lote.py:161] [atestados_lote.py:205] [views.py:2147] [painel_ausencia.html:71] [painel_ausencia.html:95] [views_atestado.py:29]
- **Escritores.** A catraca identifica `criar_ausencia` e `_gravar_dados_atestado` como os escritores dos seis campos. `_reabrir` grava documento, descrição e prazo e delega a transição de estado a `mudar_estado`. `anexar_documento` grava o documento e chama a porta dos seis campos. `_apagar_anexo_escrito_agora` executa a remoção. [test_atestado_f3_contrato.py:74] [test_atestado_f3_contrato.py:169] [ausencia.py:317] [ausencia.py:567] [ausencia.py:1114] [ausencia.py:1411] [ausencia.py:241]
- **Fora do escopo.** B1b, B2a, B2c, B5 e INSS seguem em fila, assim como o possível órfão no rollback posterior do `atomic` externo, o nome anterior defasado, a corrida entre `.exists()` e `storage.delete`, o erro em `documento.open('rb')` e a cópia via `DadosAtestadoForm.base_fields`. Banco e produção ficam [nao verificado]. [views.py:2569] [ausencia.py:256] [ausencia.py:262] [atestados_lote.py:171] [forms.py:13]

**Mensagem de commit sugerida** [atestados_lote.py:149] [ausencia.py:1434] [nao verificado]

```text
OBRA: O30-F3-CONSTRUIR
LEI-AKITA: padrao=escritor unico

Explicita a decodificação no PDF e limita o aviso de órfão à falha da reabertura.
```

## 26/11 17:26 · sessao 2ad7899ff274 · ABONO-X-SAIDA-ANTECIPADA · verificador · origem=Code

**Veredito da volta c1e — reprova.** O diff do motor respeita os sítios autorizados, mas a parcial e o pareador podem dar dias diferentes ao mesmo intervalo. A lista também pode publicar `0/30` para uma ausência que já abateu 30 minutos na jornada. A ordem de invalidação pode inverter a aquisição das travas de fechamento. `app/ponto/services/credito_parcial.py:100–114` · `app/ponto/turnos.py:257–260,1310–1314` · `app/ponto/motor_calculo_v2.py:1237–1293` · `app/ponto/services/ausencia.py:183–205` · `app/ponto/services/fechamento.py:1672–1680`

Os oito blobs da cópia conferem com os índices finais do diff anexado. A cópia veio sem metadados Git, portanto a identidade do objeto do commit e a sequência dos commits ficam **[nao verificado]**. Os contraexemplos abaixo são de código ou de execução sintética em memória. Incidência em produção **[nao verificado]**. [nao verificado]

### 1. Escopo do `!` — nada

No diff fornecido, o motor muda em dois sítios, `_minutos_da_ponta_cobertos` e o bloco da parcial em `_aplicar_teto_pontualidade`. `_parciais_do_dia` já consulta a porta cujo significado mudou. Seu corpo ficou igual no diff. `app/ponto/motor_calculo_v2.py:453–485,1222–1305,1344–1361`

### 2. Escritores, leitores e unidades — média

| Campo ou retorno | Quem escreve e quem lê |
|---|---|
| Chave de `parciais_com_ponta_por_dia` | `credito_parcial` primeiro reúne ausências pela data civil `a.data`, depois escreve `{dia_da_jornada: [Ausencia]}`. O motor lê `.get(dia)` e memoiza por `(colaborador, dia)`. `rubricas_por_dia` lê `.get(k)` e passa a lista ao contexto do teto. Os testes também leem a chave diretamente. `app/ponto/services/credito_parcial.py:15–32,62–81` · `app/ponto/motor_calculo_v2.py:1344–1361` · `app/ponto/calculador/alimentacao.py:510–539` · `app/ponto/calculador/regras.py:117–127` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:380–392` |
| `dia`, `ini_min`, `fim_min`, `adiante` | O teto agrupa pelo mapa recebido e passa `dia` como a **data da jornada**. As pontas são minutos de relógio desde a meia-noite local desse dia, com fração pelos segundos das batidas. O leitor `_minutos_da_ponta_cobertos` traduz cada minuto para a meia-noite civil de `a.data` e consulta o juiz do período por meio de `batida_dentro_da_parcial`. As chamadas efetivas usam `adiante=0`, por default. A simulação da lista usa `1`, em **dias civis**, traduzido em 1.440 minutos. Os demais chamadores diretos são os testes. `app/ponto/motor_calculo_v2.py:453–485,1132–1138,1233–1277` · `app/ponto/turnos.py:2683–2692,2714–2723` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:438–467` |
| `dia`, `causa` | O teto grava a data da jornada e o texto da causa. Nesse ramo a data também coincide com `a.data` por exigência da guarda. A deduplicação lê `dia`. E6 projeta o campo em conjunto de dias. A lavra grava `data` ISO, recorta a competência e conta `causa`. `janela_he` e o resumo do espelho filtram por causa. O fechamento concatena o item inteiro. Os diffs leem a lista, com o problema de classificação do item 6. `app/ponto/motor_calculo_v2.py:1267–1293` · `app/ponto/management/commands/e6_oraculo.py:162–172` · `app/ponto/services/cxr_na_lista.py:84–127,168–187` · `app/ponto/janela_he.py:324–329` · `app/ponto/services/espelho.py:748–755,1295–1297` · `app/ponto/services/fechamento.py:116–117,314–317` |
| `ausencia_id`, `ponta`, `periodo`, os dois `abate_*_min` | O teto grava o PK da ausência, a rubrica da ponta, `HH:MM-HH:MM` do cadastro e dois inteiros em minutos de relógio. A deduplicação lê PK e ponta. A lavra copia os cinco campos, preservando os inteiros, e `_cxr_do_motor` os converte em texto. HTML e PDF leem `detalhe`. O calculador também transporta o item em `Rubricas.detalhe['cadastro_x_realidade']`. `DiaPago.lavrar` consome as rubricas, sem copiar esse detalhe. Os testes leem os campos no resultado e na lavra e verificam o texto da C12. `app/ponto/motor_calculo_v2.py:1285–1293` · `app/ponto/services/cxr_na_lista.py:32–47,87–92` · `app/escala/services/cadastro_realidade.py:455–469,1323–1332` · `app/templates/escala/cadastro_x_realidade.html:31` · `app/ponto/calculador/regras.py:281–285,865–884` · `app/ponto/services/dia_pago.py:322–333` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:307–334` |
| Retorno de `vespera_do_cadastro` | A função extrai `(hi, hf)` do DNA da véspera ou das duas bordas do template, ou devolve `None`. São horários locais de cadastro. Os dois leitores são `_vespera`, que passa a resposta a `_mk`/`_data_do_turno`, e `dia_da_jornada_da_parcial`, que passa a resposta diretamente a `_data_do_turno`. O juiz transforma as bordas em comparações com instantes. `app/ponto/turnos.py:216–231,248–260,300–338,1310–1314,1501–1508` · `app/ponto/services/credito_parcial.py:102–114` |
| Retorno de `dia_da_jornada_da_parcial` | `_data_do_turno` escreve a resposta, uma `date` local igual ao dia civil do início ou à véspera. O mapa de parciais usa essa data como chave. `_acordar_projecao` usa a menor data entre ela e a borda civil para invalidar o intervalo. O teste da véspera lê o retorno diretamente. `app/ponto/turnos.py:300–324` · `app/ponto/services/credito_parcial.py:74–80,100–114` · `app/ponto/services/ausencia.py:192–205` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:380–392` |

**Fuso e significado.** A medição da ponta usa o fuso fixo de `settings.TIME_ZONE`. A construção dos instantes e o juiz usam o fuso corrente do Django. O padrão é `America/Sao_Paulo`. A igualdade depende do fuso ativo ser o mesmo. Os dois nomes de abatimento sugerem efeito monetário, mas os números medem sobreposição bruta, conforme o item 9. `app/ponto/motor_calculo_v2.py:28,73–79,1236–1255,1273–1292` · `app/ponto/services/credito_parcial.py:109–114` · `app/ponto/turnos.py:45–48,314` · `app/config/settings/base.py:93–95`

### 3. Juiz e carga — alta

**A decisão do dia é toda de `_data_do_turno`.** O adaptador monta início e fim e devolve diretamente a resposta do juiz. Ele próprio repete a conversão `HH:MM` e a virada `fim < início`, que já existem no juiz do período. A decisão de dia continua única. A derivação da virada tem duas implementações. `app/ponto/services/credito_parcial.py:108–114` · `app/ponto/turnos.py:300–324,2683–2689` · `app/core/juizes.py:216,266–277`

**A carga pode mudar a resposta.** A parcial carrega o vínculo do dia civil e lê o DNA direto numa janela em torno desse dia. `_turnos_do_juiz` usa o template recebido ou o da instância e o mapa de células da janela ou da fatia. `turnos_de_batidas` ainda copia as batidas e marca ecos. Com template `19:00–07:00`, sem DNA na janela curta e com DNA apenas em outro dia da janela longa, `01:00–03:00` pode cair na véspera pela carga curta e no dia civil pela longa. O mapa longo é não vazio e, sem a chave da véspera, devolve `None`. Uma troca de vínculo na borda também pode mudar o template e a decisão de consultar DNA. `app/ponto/services/credito_parcial.py:100–107` · `app/ponto/motor_calculo_v2.py:902–917` · `app/escala/utils.py:84–108` · `app/ponto/turnos.py:257–260,300–324,1529–1547,1846–1859` · `app/ponto/services/fechamento.py:291–311`

**A diferença de fatos também muda o dia.** O pareador veta a véspera quando já atribuiu a ela um turno encerrado naquele dia. O teste existente fixa `16:00–20:00` em D e `03:00–07:30` em D+1, com template `19:00–10:00`, e mantém o segundo par em D+1. Uma parcial sintética com os horários do segundo par, sob o mesmo template e sem DNA, recebe D pela consulta direta de cadastro. `app/ponto/turnos.py:216–231,314–324,1268–1278,1310–1314` · `app/ponto/services/credito_parcial.py:107–114` · `app/ponto/tests/test_turno_da_madrugada_pertence_a_vespera.py:33–36`

### 4. Extração de `_vespera` — nada

Para o pareador, a extração preserva referência pela entrada ou saída, cálculo da véspera local, veto pelo turno já pareado e os mesmos dois ramos de cadastro. `_bm_all` continua chegando com os valores vazios retirados. Mapa presente sem a véspera continua devolvendo `None`. `app/ponto/turnos.py:248–260,1233–1239,1268–1278,1310–1314` · `app/ponto/tests/test_turno_da_madrugada_pertence_a_vespera.py:33–45`

### 5. Chave dos leitores e bordas da competência — alta

- **O mapa novo resolve as duas bordas dentro do seu próprio contrato.** Lê datas civis até `fim+1`, depois filtra pela jornada. `ini` atribuída a `ini−1` sai do recorte corrente e pode entrar no anterior. `fim+1` atribuída a `fim` entra no recorte corrente. A parcial entra em uma chave, e o crédito de atraso exclui as parciais com período. Isso evita dupla contagem entre o teto e esse crédito. A aprovação invalida a competência da jornada e a civil, zerando o carimbo existente. `app/ponto/services/credito_parcial.py:46–50,62–81` · `app/ponto/turnos.py:300–324` · `app/ponto/services/ausencia.py:186–205,1133–1134` · `app/ponto/services/fechamento.py:1672–1680`
- **Motor e calculador pedem a mesma espécie de chave, mas o motor pode mudar seu valor entre passadas.** A base pode herdar o dia por um gap de até seis horas. Partido e comercial montam outro mapa a partir do juiz, e o calculador usa o mapa final. Se só o dia mudar, `_pontas_parcial` pode pular o novo julgamento porque guarda apenas atraso e saída. Assim, o abatimento pode ficar da primeira chave enquanto o período passa a outra. `app/ponto/motor_calculo_v2.py:1125–1127,1237–1241,1338–1342,2558–2567,3093–3104,3357,3488–3490,3671` · `app/ponto/calculador/alimentacao.py:239–262,510–539`
- **Grade, espelho e PDF ainda têm leituras civis da parcial com período.** A grade procura a cobertura por `a.data`, então pode classificar batidas e dispensar marcos na linha D+1 enquanto o teto abate a ponta em D. A ata recebe essa classificação. O ícone e a cobertura do espelho ficam na linha civil, e o E6 pula a linha marcada com ausência. No PDF, `efeitos_da_ausencia` soma `a.minutos` na data civil no resumo bruto, que pode ser substituído pelo gravado. O fechamento e o `DiaPago` também recebem o abono da grade, calculado como previsto menos coberto na linha de ausência. São efeitos que podem cair em duas competências. Isso, por si, não prova desconto abatido duas vezes. `app/ponto/turnos.py:2849–2851,2868–2879` · `app/escala/utils.py:1230–1233,1246–1252,1272–1295` · `app/ponto/services/cartorio.py:782–790` · `app/ponto/services/espelho.py:593–602,863–890` · `app/ponto/management/commands/e6_oraculo.py:339–341` · `app/ponto/services/efeito_ausencia.py:46–50` · `app/relatorios/pdf_espelho.py:332–336,479,628` · `app/relatorios/cartao_pela_celula.py:320–346` · `app/ponto/services/fechamento.py:1084–1088` · `app/ponto/services/dia_pago.py:386–401`
- **`DiaPago` pode gravar atraso residual fora da competência da jornada.** O fechamento recebe a pontualidade já abatida, mas `atraso_por_dia` agrupa pelo dia civil da entrada. `DiaPago` usa esse mapa para atraso e o dia da jornada para saída. Uma entrada de madrugada atribuída pelo motor à véspera pode, assim, deixar atraso residual em `fim+1` numa linha gravada para a competência de `fim`. A gravação aceita a chave sem recortar de novo. O oráculo de rubricas usa a jornada e substitui valores por suas próprias chaves. `app/ponto/services/fechamento.py:1018–1026,1224–1241,1293–1312` · `app/ponto/services/credito_parcial.py:117–131` · `app/ponto/services/dia_pago.py:173–195,322–330,340–360`

### 6. Publicação e leitores da lista — alta

- **Pode publicar parcial que já abateu.** `_pontas` nasce no laço de cada par. Reproduzi em memória uma jornada com dois pares e parcial D `06:30–19:30`. Ela eliminou 30 minutos de atraso do primeiro par e gerou `0/30` na saída antecipada do segundo. O filtro vê somente o segundo par quando decide publicar. Também há omissão pelo filtro: se o juiz mandar a parcial datada D para D−1, o laço de D nunca a examina para o relato de D. `app/ponto/motor_calculo_v2.py:1237–1293` · `app/ponto/services/credito_parcial.py:74–80` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:344–351,380–392`
- **A mesma instância deduplica por `(dia, ausência, ponta)`, mantendo o primeiro número.** Uma passada posterior pode reescrever a ponta, como faz a Zona 5, sem atualizar ou retirar a linha publicada na primeira. A composição concatena as listas das fatias. Na lavra, só dicionários inteiramente iguais são deduplicados. Duplicação efetiva entre fatias em produção **[nao verificado]**. O calculador usa seu próprio contexto e devolve o relato em `Rubricas.detalhe`. Já a rota de `Censo.cxr` chama a pontualidade sem `parciais=`. `app/ponto/motor_calculo_v2.py:1282–1293,2442,2877–2878,3235–3242,3357,3671` · `app/ponto/services/fechamento.py:314–317` · `app/ponto/services/cxr_na_lista.py:107–113` · `app/ponto/calculador/regras.py:281–285,865–876` · `app/ponto/calculador/periodo.py:474–477,786–793`
- **Os leitores aceitam a forma nova com efeitos diferentes.** `janela_he` e o resumo de HE do espelho excluem a nova causa corretamente. E6 e `cxr_lavrado` projetam presença por dia. O dono continua sendo decidido por dia, e `BATIDA` pode vencer a presença na lista. A C12 preserva os dois números, mas a legenda afirma que o horário real ficou longe do cadastrado e a ação encaminha para template ou vínculo, embora o item descreva a data da ausência. `app/ponto/janela_he.py:324–329` · `app/ponto/services/espelho.py:1295–1297,1330–1331` · `app/ponto/management/commands/e6_oraculo.py:98–110,162–172,182–195` · `app/ponto/services/cxr_na_lista.py:168–187` · `app/escala/services/cadastro_realidade.py:133–134,455–469,783–787,1105–1123`
- **Os dois diffs classificados como L-084 ficam semanticamente errados.** `diff_janela_he` conta qualquer item como fora da janela pela L-084. `diff_reclassificar_partido` conta qualquer item como dia-colab que deixou de gerar desconto pela L-084 e pode imprimir deltas `None` quando a amostra é o item novo. `app/ponto/management/commands/diff_janela_he.py:63–70,131–134` · `app/ponto/management/commands/diff_reclassificar_partido.py:186–190,259–273`

### 7. Classes de reprovação

| Classe | Resultado |
|---|---|
| Segundo juiz registrado | **Nada para o dia.** O adaptador chama `_data_do_turno`. A diferença está na carga. `app/docs/JUIZES.md:10–12,19–25` · `app/core/juizes.py:216` · `app/ponto/services/credito_parcial.py:100–114` |
| Segundo escritor fora da porta | **Nada.** O período segue escrito por `decidir`, a invalidação segue pela porta existente e a C12 segue gravada pela lavra existente. `app/ponto/services/ausencia.py:1061–1064,1089–1091,1111–1114,1133–1134` · `app/ponto/services/fechamento.py:1672–1680` · `app/ponto/services/cxr_na_lista.py:145–152` |
| Relógio fora da borda | **Nada novo.** A leitura de `localdate()` da linha em aberto antecede o diff. O comando continua lendo `agora` na borda. `app/ponto/services/ausencia.py:189–193` · `app/ponto/management/commands/lavrar_cxr_do_motor.py:27–32` |
| Derivação paralela | **Alta.** Há segunda carga de template/DNA e repetição da virada do período. A invalidação também usa um universo mais largo que o teto: basta ter as duas horas. Uma edição pode preservar essas horas ao mudar tipo, minutos ou vigência, e invalidar a véspera para linha que o teto já exclui. `app/ponto/services/credito_parcial.py:29–31,74–80,100–114` · `app/ponto/turnos.py:2683–2689` · `app/ponto/services/ausencia.py:193–205,656–680` |
| Selo/RED sem mordida | **Média.** A catraca morde composição, sítios e linhas, dentro de seus critérios léxicos. Ela não prova igualdade de carga nem a decisão publicada. Os testes de comportamento mordem a forma simples, com as lacunas do item 8. `app/core/tests/test_medida_turno_do_par.py:548–585` · `app/core/medida_turno_do_par.py:32–55` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:299–392` |
| Ordem de trava | **Alta.** Na edição de uma parcial de 21/10, de horário diurno para `01:00–03:00` da jornada de 20/10, o laço invalida primeiro a competência 11 pelo estado anterior, depois a 10 pelo novo intervalo, no mesmo `atomic`. O caminho de revisão de batida pode tomar as mesmas competências na ordem 10 → 11. Há inversão possível se ambos alcançarem as mesmas linhas. Ocorrência de deadlock **[nao verificado]**. `app/ponto/views.py:2835–2847` · `app/ponto/services/ausencia.py:183–205,1089–1091,1133–1134` · `app/ponto/services/fechamento.py:1623,1672–1680,1854–1876` · `app/ponto/services/dia_pago.py:702–719` |
| Pouso antes do pré-requisito | **[nao verificado].** A árvore declara suite inteira antes do opus e condições para reconstrução das projeções. O material fornecido não comprova execução, publicação do impacto ou pouso. `CLAUDE.md:392–402,418–437` · `app/docs/LEIS.md:142` |
| Campo com dois significados | **Alta.** Os campos `abate_*` medem sobreposição bruta por par, mas o texto da C12 os apresenta como abatimento. O atraso residual também conserva uma chave civil ao lado da chave de jornada usada para saída. `app/ponto/motor_calculo_v2.py:1237–1293,1306–1337` · `app/escala/services/cadastro_realidade.py:455–459` · `app/ponto/services/dia_pago.py:173–195` |
| `except`, `suppress` ou fallback | **Nada novo de captura.** A extração mantém a escolha anterior entre DNA e template no pareador. A parcial passa a usar esse ramo de template com a sua própria carga. A diferença observável é a do item 3. `app/ponto/turnos.py:248–260,1310–1314` · `app/ponto/services/credito_parcial.py:100–114` |

### 8. Mordida dos testes — média [nao verificado]

| Teste novo | O que pegaria | [nao verificado]
|---|---|
| D+1 `01:00–03:00` | Perda da parcial por recorte civil, chave errada ou falta da tradução de 1.440 minutos. Exige saída residual 240 nas quatro leituras e lista vazia para a parcial. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:79–112,136–141,299–302` |
| D `01:00–03:00` | Abatimento indevido em D+1, perda ou repetição do item nesse cenário, ou perda de dono, PK e números entre motor, lavra e formatador. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:304–334` |
| D `02:00–01:30` e D `06:30–19:30` | O primeiro pega publicação de parcial que já cobre a saída. O segundo pega uma guarda que olha a saída mas ignora abatimento do atraso **no mesmo par**. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:336–351` |
| Aprovação em 21/10 | Pega a volta da invalidação apenas civil. Verifica os dois carimbos nulos, sem verificar relavratura, rubricas nas duas competências ou concorrência. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:353–373` |
| Controle D `23:00–03:00` e véspera com turno | O primeiro pega quebra da virada e falso C12 no período que já cobre 120 minutos. O segundo pega chave fixa por `a.data` ou abandono da véspera do DNA. O segundo consulta juiz e mapa, sem comparar um pareamento real na mesma carga. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:375–392` |
| Dois testes puros novos | Isolam a tradução de D+1 e distinguem o `0` como datada do `120` com `adiante=1`. Os testes adjacentes verificam chamada e uso da resposta do juiz, bordas e segundos. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:446–501` |

**Contraexemplos faltantes.** Dois pares da mesma jornada, segunda passada que muda a ponta ou o dia, DNA fora da janela curta, troca de vínculo no corte, redatação que muda o dia no juiz, cobertura já dada por outra parcial e teto aritmético que reduz o ganho. Também falta cruzar a borda 20/21 pelas duas apurações e conferir a C12 na lista e na rota. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:299–423` · `app/ponto/motor_calculo_v2.py:1237–1337,3235–3242` · `app/ponto/services/credito_parcial.py:74–114` · `app/escala/views.py:521–537`

Em memória, passaram os **35 testes da catraca** com a adaptação documentada para `unittest`, e os **10 testes puros da ponta** com as funções reais extraídas da cópia. Os testes de integração com banco, a suite inteira e os REDs históricos ficam **[nao verificado]**. `app/core/tests/test_medida_turno_do_par.py:28–41,548–567` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:426–501`

### 9. Significado dos

[truncado em 20000 de 22099 caracteres]

## 26/11 17:25 · sessao 4d122d1eb77b · - · - · origem=Ronald (tmux)

1. **Escopo, nada.** O diff altera `_minutos_da_ponta_cobertos` e o bloco da parcial no teto. `_parciais_do_dia` permanece como leitor. Os oito blobs da cópia conferem com o `index` do diff fornecido. O objeto Git do commit está [nao verificado]. `app/ponto/motor_calculo_v2.py:453,477,1244,1273,1344`.

3. **Alta.** O retorno do dia vem exclusivamente de `_data_do_turno`. A carga difere por vínculo civil, janela pontual de DNA em vez da janela da apuração, consulta ao banco em vez de células alimentadas e ausência do veto de turno já encerrado na véspera. Em prova sem banco com cargadores simulados, parcial de 02/09 03:00–07:30 e template 19:00–10:00 retornou 01/09 com mapa pontual vazio. O par com o mesmo horário retornou 02/09 se o pareador tinha encerrado 01/09 ou possuía mapa maior com DNA somente em 07/09. `app/ponto/services/credito_parcial.py:97–114`, `app/ponto/motor_calculo_v2.py:902–917`, `app/ponto/turnos.py:248–260,300–324,1310–1314,1546–1547`.

4. **Extração, nada.** O pareador mantém a referência local, a filtragem das bordas e o veto. O helper executa os mesmos ramos para DNA não vazio e bordas do template. Os oito testes do BUG-145 passaram em memória com dublês de importação do Django. `app/ponto/turnos.py:248–260,1232–1239,1268–1278,1310–1314`, `app/ponto/tests/test_turno_da_madrugada_pertence_a_vespera.py:23–57`.

6. **Alta.** `_b0` consulta só as pontas de um `p`, embora o comentário diga jornada. O teto real, com dois pares sintéticos do mesmo dia, liquidou 30 min de atraso em um par e publicou `0/30` para a mesma parcial na saída do segundo. A Zona 5 pode alimentar pontas separadas. `app/ponto/motor_calculo_v2.py:1237–1244,1273–1293,3185–3209,3235–3242`. A causa de madrugada também foi emitida para `08:30–09:30`, porque as guardas medem data e sobreposição após o deslocamento. `app/ponto/motor_calculo_v2.py:1267–1293`. `InsumosDoDono` reduz a lista a um conjunto de dias, e o juiz ainda pode dar BATIDA antes de consultar o conjunto. `app/ponto/management/commands/e6_oraculo.py:98–110,162–172`.

7. **Alta.** Há derivação paralela da leitura dos limites e da virada do período registrado. A cópia de `hora:minuto` e `fim < início` mora na preparação da parcial. Não há segundo predicado de dia. `app/core/juizes.py:266–277`, `app/ponto/turnos.py:2683–2689,2710–2713`, `app/ponto/services/credito_parcial.py:108–114`. A edição de uma parcial na virada pode adquirir fechamento da competência civil primeiro pelo trecho antigo e da anterior depois pelo novo. A revisão C13 percorre ambas em ordem cronológica dentro do ato. Ocorrência concorrente em produção [nao verificado]. `app/ponto/views.py:2835–2848`, `app/ponto/services/ausencia.py:183–205,1089–1091,1133`, `app/ponto/services/fechamento.py:1168,1664–1680,1854–1876`. A invalidação usa presença dos dois horários como universo, enquanto o dinheiro exige tipo pago e parcial vigente. A edição ordinária pode preservar os horários e alterar a condição de parcial. `app/ponto/services/ausencia.py:193–205,656–680`, `app/ponto/services/credito_parcial.py:29–31,54–59`. Segundo escritor, nova leitura de relógio e novo `except/suppress`, nada no diff. A seleção do template na falta de mapa é preexistente, com alcance diferente na carga nova. `app/ponto/turnos.py:248–260,1310–1314`, `app/ponto/services/ausencia.py:190–205`. Pouso e prerequisitos em produção [nao verificado]. Cinco testes da catraca passaram em memória, incluindo composição, números, linhas e mutante B1. O detector declara limitações lexicais. `app/core/tests/test_medida_turno_do_par.py:28–41,510–571,846–852`, `app/core/medida_turno_do_par.py:29–55`.

9. **Alta.** A guarda real é `_b0 > 0` e depois `round(_b1) <= 0`. O primeiro é a soma da cobertura isolada da parcial nas pontas brutas do par atual e vale sempre zero nos itens publicados. O segundo é a cobertura bruta isolada da ponta após deslocar a data, arredondada. Em prova sintética, uma parcial corretamente datada já abateu os mesmos 120 min anunciados para mover a incorreta. O ganho marginal seria zero. Teto diário e demais parciais ficam fora do contrafactual. `app/ponto/motor_calculo_v2.py:1244–1258,1273–1293,1306–1331`. O teste percorre command, snapshot e `_cxr_do_motor`. O HTML/PDF passam por `lista()`, harmonização de dono e seleção da ação C12. A ação de CADASTRO aponta para o template ou vínculo, apesar de a divergência nova ser da data da ausência. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:318–329`, `app/escala/views.py:512–537`, `app/escala/services/cadastro_realidade.py:455–469,783–787,1040–1058,1105–1108`, `app/templates/escala/cadastro_x_realidade.html:31`.

## 26/11 17:23 · sessao 03ced07866e8 · - · - · origem=Ronald (tmux)

**Alta.** O mapa novo é escrito por jornada, e o recorte civil inclui `fim+1` antes de filtrar `ini…fim`. Seus dois consumidores diretos são o teto do motor e o calculador de rubricas. Contudo, o motor base ainda pode herdar por gap de 6 h, enquanto partido/comercial e calculador usam o mapa final do pareador. Um segundo passe que muda o dia e preserva os valores pula a parcial porque `_pontas_parcial` guarda só atraso e saída. [app/ponto/services/credito_parcial.py:74-80, app/ponto/motor_calculo_v2.py:1233-41,1340-61,2558-67,3093-3104,3357,3488-90,3671, app/ponto/calculador/alimentacao.py:239,259-62,511-39]

**Alta.** A grade e o espelho ainda leem cobertura pela data civil. Uma parcial D+1 pode reduzir a saída da jornada D, enquanto a grade classifica a parcial e dispensa marcos na linha civil D+1, o espelho põe a ausência ali e o E6 exclui essa linha. O abono da grade também segue essa linha no fechamento e no `DiaPago`. São efeitos distintos do abatimento, que pode ocorrer em competência diversa. O PDF soma `a.minutos` da data civil no resumo bruto, substituído pelos totais gravados quando disponíveis. [app/ponto/turnos.py:2849-51,2868-79, app/escala/utils.py:1230-33,1246-52,1272-95, app/ponto/services/espelho.py:593-602,863-68, app/ponto/management/commands/e6_oraculo.py:339-41, app/ponto/services/fechamento.py:1084-88, app/ponto/services/dia_pago.py:285-92,386-401, app/ponto/services/efeito_ausencia.py:46-50, app/relatorios/pdf_espelho.py:479,628, app/relatorios/cartao_pela_celula.py:320-46]

**Alta.** Crédito para desconto exclui as parciais com período. O fechamento lê saída e atraso já abatidos, porém o mapa do atraso residual usa a data civil da entrada. Com entrada na madrugada classificada na véspera, o `DiaPago` pode guardar atraso em D+1 e saída em D, inclusive com data fora da competência gravada. O calculador agrupa por jornada. [app/ponto/services/credito_parcial.py:47-50,117-131, app/ponto/services/fechamento.py:1018-26,1224-41,1293-1318, app/ponto/services/dia_pago.py:173-95,322-30,340-60]

**Alta.** `_b0` soma as pontas do par corrente. Assim uma ausência pode abater 30 min no atraso do primeiro par e ainda publicar `0/30` na saída do segundo. O teste de atraso e saída usa um só par. O cálculo deslocado de `_b1` não verifica se a nova data conservaria a jornada. Exemplo estático com turno D 19–07, saída D+1 01:00 e parcial D 06:00–11:01. O relato mede 60 min, mas a redatação efetiva ultrapassa às 11:01 o envelope da jornada D, que fecha às 11:00. [app/ponto/motor_calculo_v2.py:1237-57,1267-92, app/ponto/tests/test_abono_parcial_abate_a_ponta.py:346-54, app/ponto/turnos.py:80-82,229-31,314-24, app/ponto/services/credito_parcial.py:107-14]

**Média.** A invalidação expande para a jornada e conserva a competência civil. Ela só exige os dois horários, enquanto a folha exige parcial paga. Um registro editado para inteiro/range conserva horários e pode invalidar a competência anterior sem ser lido pelo teto. A porta invalida o carimbo. [app/ponto/services/ausencia.py:186-205,637-44,656-80, app/ponto/services/credito_parcial.py:29-31,74-80, app/ponto/services/fechamento.py:1672-80]

**Média.** A C12 real conserva o detalhe no HTML, mas continua dizendo que o horário real ficou longe e encaminha para corrigir o template/vínculo. O teste executa a lavra e o formatador, sem passar por `lista()`, pela consolidação do dono ou pela view e renderização. [app/escala/services/cadastro_realidade.py:129-34,783-87,1042-58,1105-08,1121-23, app/ponto/tests/test_abono_parcial_abate_a_ponta.py:315-334, app/escala/views.py:521-37, app/templates/escala/cadastro_x_realidade.html:31]

**Baixa.** A medição usa minutos fracionários de relógio em `settings.TIME_ZONE`, a parcial é montada em `HH:MM` no fuso ativo do Django, e os campos da C12 são minutos inteiros. Os fusos coincidem na configuração padrão, com divergência possível se o Django estiver com outro fuso ativo. [app/ponto/motor_calculo_v2.py:28,73-79,475-84,1236-55,1292, app/ponto/services/credito_parcial.py:109-14, app/ponto/turnos.py:45-48,314, app/config/settings/base.py:93-95]

## 26/11 17:23 · sessao eb543fa7c75e · - · - · origem=Ronald (tmux)

**6. Alta — o relato pode acusar uma parcial que já abate no mesmo dia.** `_pontas` é recriada por par, e `_b0` soma só as pontas desse par. Em um exemplo sintético com dois pares, a parcial D 06:30–19:30 eliminou 30 min de atraso do primeiro par e produziu `0/30` na saída antecipada do segundo. O teste correspondente usa um único par. `app/ponto/motor_calculo_v2.py:1237–1292` · `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:344–351`

**6. Média — a deduplicação conserva o primeiro número.** O motor impede repetição de `(dia, ausência, ponta)` na mesma lista, mas não atualiza a entrada se a Zona 5 mudar o bruto. Reexecutei o corpo do teto em memória com bruto 360 na primeira chamada e 30 na segunda. O desconto passou a 30, e a linha permaneceu `0/120`. O fechamento concatena as listas das fatias. A lavra remove apenas cópias integralmente iguais. Incidência em produção [nao verificado]. `app/ponto/motor_calculo_v2.py:1282–1293,2442,2877–2878,3235–3242,3357,3671` · `app/ponto/services/fechamento.py:116–117,306–317` · `app/ponto/services/cxr_na_lista.py:110–113`

**6. Média — os leitores aceitam o dicionário, mas alguns atribuem a ele outra causa.**

- `InsumosDoDono.cxr()` projeta qualquer item com `dia` para um conjunto de dias. `dono()` envia a presença nesse conjunto ao juiz. Batida fora do turno e contagem ímpar podem dar `BATIDA` antes da regra genérica de presença na lista. `app/ponto/management/commands/e6_oraculo.py:98–110,162–172,182–195`
- `janela_he` exige causa contendo `janela de HE`, e o resumo do espelho exige a causa exata de chegada fora da janela. O resultado bruto continua disponível no espelho. `app/ponto/janela_he.py:324–329` · `app/ponto/services/espelho.py:748–755,1295–1297,1330–1331`
- A lavra preserva os novos campos, converte `dia` para `data` ISO e recorta a competência. `cxr_lavrado` usa apenas as datas. A lista e o PDF exibem o detalhe, mas a legenda C12 afirma que o horário real ficou longe do cadastrado, e a ação CADASTRO aponta para corrigir o template ou abrir o vínculo. A ocorrência nova descreve a data da ausência. `app/ponto/services/cxr_na_lista.py:32–34,84–113,168–187` · `app/escala/services/cadastro_realidade.py:133–134,446–469,783–787,1105–1123,1331` · `app/templates/escala/cadastro_x_realidade.html:31`
- `diff_janela_he` conta todos os itens como fora da janela pela L-084. `diff_reclassificar_partido` conta todos como dia-colab L-084 e, se tomar o item novo de exemplo, imprime deltas `None`. `app/ponto/management/commands/diff_janela_he.py:63–70,131–134` · `app/ponto/management/commands/diff_reclassificar_partido.py:186–190,259–273`
- O calculador empresta o teto e pode produzir o item em seu contexto próprio. A rota que alimenta parciais leva esse retorno a `det['cadastro_x_realidade']`. A rota de `Censo.cxr` chama a pontualidade sem parciais. `app/ponto/calculador/alimentacao.py:511–515,537–539` · `app/ponto/calculador/regras.py:114–127,281–285,865–876` · `app/ponto/calculador/periodo.py:474–477,786–793`

**9. Alta — o segundo número é cobertura geométrica, e pode diferir do abatimento final.** O primeiro é a soma das interseções da parcial isolada com as pontas brutas do par. Para entrar na lista, essa soma precisa ser zero. A guarda efetiva pula `_b0 > 0` e `round(_b1) <= 0`. `_b1` é a interseção isolada com uma ponta bruta, deslocando a data em 1.440 min. O código não reatribui o dia hipotético pelo juiz, não mede o ganho além de outra parcial já cobrindo os minutos, nem simula o teto L-093 que vem depois. Em checagens sintéticas, o método publicou `0/120` tanto quando outra parcial já cobria os mesmos 120 min como quando o teto aritmético zerava todo o desconto. `app/ponto/motor_calculo_v2.py:475–485,1245–1258,1267–1293,1306–1337` · `app/ponto/services/credito_parcial.py:74–80,100–114`

**9. Média — o teste para no formatador C12.** Exercita o comando, o espelho consumido pela lavra, o juiz de dono, o snapshot e a formatação dos dois valores. Pula `lista()`, que agrega assinaturas, reconcilia donos, aplica visibilidade, escolhe a ação e passa o conteúdo à view, ao HTML e ao PDF. Também pula a orquestração do cron. Execução em produção [nao verificado]. `app/ponto/tests/test_abono_parcial_abate_a_ponta.py:304–334` · `app/ponto/management/commands/lavrar_cxr_do_motor.py:27–40` · `app/ponto/services/cxr_na_lista.py:80–106,145–165` · `app/escala/services/cadastro_realidade.py:1040–1058,1076–1079,1105–1126` · `app/escala/views.py:512–536` · `app/config/crons.py:1808–1810`

## 26/11 17:21 · sessao 7ee749aec0b8 · O30-F3-CONSTRUIR · verificador · origem=Code

## Veredito — REPROVADO

1. **O `except` do PDF excede o aval.** O `try` cobre `Image.open`, a entrada e a saída do `with` e `ImageOps.exif_transpose`. `thumbnail`, `foto.save` e a montagem da imagem do PDF ficam fora. O teste de decode exige justamente a captura fora de `Image.open`: a abertura retorna normalmente e o `OSError` vem de `original.load()`, chamado pelo `exif_transpose` simulado. [atestados_lote.py:147](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:147) · [test_atestado_f3_contrato.py:746](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:746) · [test_atestado_f3_contrato.py:784](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:784)
2. **V1 parcialmente curada.** A reabertura captura o `storage` e o nome efetivamente gravado quando o `FieldFile` passa de pendente a gravado. Fora de `atomic` externo, o helper preserva o arquivo se encontrar qualquer `Ausencia` com aquele nome no banco consultado. Isso cobre o `on_commit` que levanta após confirmar e o `FieldFile` antigo já gravado. **Contraexemplo:** um `atomic` externo reverte após o `save`. A reabertura emite um aviso e deixa o novo arquivo órfão. O teste sem banco e o teste de integração passaram a **exigir** essa sobra. [ausencia.py:1412](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1412) · [ausencia.py:262](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:262) · [ausencia.py:1434](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1434) · [test_atestado_f3_contrato.py:489](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:489) · [test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:164](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:164)
3. **Os selos deixam passar regressões no delta.** Em memória, pus `thumbnail` dentro do mesmo `try` e tanto o selo de tratadores quanto o teste de PDF ficaram verdes. Dupliquei o parágrafo `ANEXO ILEGIVEL` com o contador em `1` e o teste de PDF também ficou verde. No selo da folha, a nova isenção aceita `storage.delete(name=...)` com receptor sem modelo identificado, inclusive um parâmetro. O mesmo mutante com o parâmetro chamado `repositorio` foi apontado como escritor. O teste da isenção verifica que o helper real fica de fora, sem verificar a origem do receptor. [test_atestado_f3_contrato.py:222](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:222) · [test_atestado_f3_contrato.py:792](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:792) · [test_lote7_selo_escritor_acorda.py:282](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_lote7_selo_escritor_acorda.py:282) · [test_lote7_selo_escritor_acorda.py:315](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_lote7_selo_escritor_acorda.py:315)

## V1 e V2 — onde os testes mordem [nao verificado]

- **V1.** Retirar a consulta de referência fez falhar os testes de `on_commit` e de referência por outra ausência. Trocar `_committed` pela diferença de nome fez falhar o teste de nome defasado. Desligar a compensação fez falhar os testes de rollback na saída do `atomic` e de erro depois de gravar no storage, antes do `UPDATE`. O teste de falha dentro de `atomic` externo verifica explicitamente que o arquivo sobra. [mutantes_atestado_f3.py:107](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/mutantes_atestado_f3.py:107) · [test_atestado_f3_contrato.py:419](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:419) · [test_atestado_f3_contrato.py:439](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:439) · [test_atestado_f3_contrato.py:456](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:456) · [test_atestado_f3_contrato.py:474](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:474) · [test_atestado_f3_contrato.py:501](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:501) · [test_atestado_f3_contrato.py:523](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:523)
- **V1 também falha sem `atomic` externo.** Reproduzi em memória: o objeto traz o nome defasado `A`, a linha ainda referencia `B`, o upload grava no caminho livre `A` e o SQL reverte para `B`. Como `nome == nome_antes`, o helper sai antes de consultar referências e deixa `A` órfão. Se a consulta de referência falhar, o tratador também deixa o arquivo e a exceção original sobe. O arquivo antigo `B` ficou preservado nesses casos. [ausencia.py:1405](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1405) · [ausencia.py:255](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:255) · [ausencia.py:262](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:262)
- **V2 curada para o caso da triagem.** O censo usa `ast.parse` e `ast.walk`. Em `from ... import _CAMPOS_ATESTADO as campos`, registra `ast.alias.name` no sítio do import local. Em `import ... as porta`, registra o `ast.Attribute` de `porta._CAMPOS_ATESTADO`. Os dois mutantes falharam no censo. O RED que restaura o filtro `asname is None` falhou porque o primeiro mutante escapou. O limite é o fluxo de valores: também confirmei que o censo aceita uma escrita externa que copia os seis campos via `DadosAtestadoForm.base_fields` e usa essa coleção em `update_fields`. [test_atestado_f3_contrato.py:130](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:130) · [test_atestado_f3_contrato.py:165](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:165) · [test_atestado_f3_contrato.py:284](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:284) · [mutantes_atestado_f3.py:130](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/mutantes_atestado_f3.py:130) · [forms.py:13](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/forms.py:13)

## PDF — contagem e inventário do delta

Em um PDF concluído, há um incremento para cada parágrafo `ANEXO ILEGIVEL #<pk>` emitido pelo ramo `anexo is None`. O rodapé repete **o total do lote** em cada página. A cobertura é menor que o universo de arquivos ilegíveis. Por exemplo, se `storage.exists()` retorna `True` e `documento.open('rb')` levanta `OSError`, a geração para antes da marca e do rodapé. O mutante que troca `+= 1` por `+= 0` foi pego. O mutante que duplica a marca passou, como consta no bloqueio 3. [atestados_lote.py:167](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:167) · [atestados_lote.py:209](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:209) · [atestados_lote.py:224](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:224) · [mutantes_atestado_f3.py:101](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/mutantes_atestado_f3.py:101)

| Arquivo | `except` / `suppress` novos |
|---|---|
| `atestados_lote.py` | **1 / 0**. Um único handler com dois tipos. [atestados_lote.py:150](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:150) |
| `ausencia.py` | **0 / 0**. O `except` existente do helper passa a cobrir também a consulta. O `try/finally` interno captura o nome e o `finally` externo registra o aviso. [ausencia.py:258](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:258) · [ausencia.py:1416](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1416) · [ausencia.py:1434](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1434) |
| `mutantes_atestado_f3.py` | **0 / 0**. `with suppress` é texto do mutante histórico, que o selo rejeitou. [mutantes_atestado_f3.py:153](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/mutantes_atestado_f3.py:153) |
| `test_atestado_f3_contrato.py` | **0 / 0**. [test_atestado_f3_contrato.py:284](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:284) · [test_atestado_f3_contrato.py:506](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:506) |
| `test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py` | **0 / 0**. [test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:171](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:171) |
| `test_lote7_selo_escritor_acorda.py` | **0 / 0**. [test_lote7_selo_escritor_acorda.py:282](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_lote7_selo_escritor_acorda.py:282) |

## Classes de reprovação

- **Campo com dois significados e selo/RED sem mordida.** `_nome_antes` serve de fotografia do objeto e ainda veta a consulta como se provasse que o arquivo já existia. Os pontos cegos dos selos estão nos bloqueios 1 e 3. A consulta `.exists()` e a remoção são operações separadas. Uma referência criada entre elas fica fora da proteção da consulta. [ausencia.py:255](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:255) · [ausencia.py:262](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:262)
- **Segundo juiz, segundo escritor de produção fora da porta, relógio fora da borda, derivação paralela e inversão de trava.** Nenhum novo identificado no delta. O PDF usa `tipo_real_documento` para classificar e Pillow para abrir e processar a imagem. A reabertura continua chamando `estado_inicial` e `mudar_estado`. O cálculo do prazo permanece no mesmo trecho. Com os seis campos, o `select_for_update` existente roda antes da gravação do documento. [atestados_lote.py:165](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:165) · [ausencia.py:305](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:305) · [ausencia.py:1403](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1403) · [ausencia.py:1426](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1426)

## Quem escreve e quem lê

| Campo ou valor | Escritor e leitor |
|---|---|
| `Ausencia.documento`, `doc.name`, `doc.storage`, `doc._committed` | `criar_ausencia`, `anexar_documento` e `_reabrir` preenchem `documento`. O `FileField` grava o upload antes do SQL. `_reabrir` lê o estado e captura storage/nome. A compensação lê as referências pelo nome persistido e chama `storage.delete`. O histórico lê a presença, o PDF abre o arquivo e a view do link lê o documento da raiz. [models.py:777](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/models.py:777) · [ausencia.py:244](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:244) · [ausencia.py:571](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:571) · [ausencia.py:1118](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1118) · [ausencia.py:1412](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1412) · [ausencia.py:262](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:262) · [dia_decidido.py:685](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/dia_decidido.py:685) · [atestados_lote.py:167](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:167) · [views.py:2147](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/views.py:2147) |
| `_nome_antes`, `pendente`, `anexo_gravado`, `banco`, `in_atomic_block`, `_apagou` | `_reabrir` produz o snapshot, `pendente` e a tupla gravada. O router fornece `banco`, usado no `atomic`, na conexão e na consulta. A conexão fornece `in_atomic_block`, lido pelo `except` e pelo `finally`. O helper retorna `_apagou`, lido pelo aviso junto de `pk` e nome. [ausencia.py:1405](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1405) · [ausencia.py:1420](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1420) · [ausencia.py:255](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:255) · [ausencia.py:1434](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1434) |
| `atestado_medico`, `atestado_crm`, `atestado_crm_uf`, `atestado_cid`, `atestado_emissao`, `atestado_dias` | A view coleta os campos do formulário. A criação grava no `INSERT`. `_gravar_dados_atestado` relê sob trava, verifica `documento_referencia_id`, grava os seis e manda os nomes alterados para `core.observ.evento`. PDF, CSV e painel leem os valores. `_CAMPOS_ATESTADO` alimenta a porta e a exclusão no admin. `_COLUNAS_ATESTADO` alimenta a exportação. As duas coleções são lidas pelo selo. [views_atestado.py:31](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/views_atestado.py:31) · [ausencia.py:567](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:567) · [ausencia.py:641](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:641) · [ausencia.py:292](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:292) · [ausencia.py:301](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:301) · [core/observ.py:136](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/core/observ.py:136) · [atestados_lote.py:104](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:104) · [painel_ausencia.html:124](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/templates/ponto/partials/painel_ausencia.html:124) · [admin.py:50](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/admin.py:50) · [test_atestado_f3_contrato.py:165](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:165) |
| `tipo`, `data`, `data_fim`, `descricao`, `status`, `prazo_documento`, `_criada`, `_reaberta` | A criação grava os valores iniciais. A edição grava tipo, datas e descrição. Na reabertura, `estado_inicial` lê tipo/presença e devolve `novo`/`precisa_prazo`. `_reabrir` concatena a descrição, calcula o prazo e marca o retorno. `mudar_estado` lê/grava status, limpa o prazo quando cabível e manda o antes/depois para a trilha. O histórico lê datas, descrição, tipo e status. O vigia lê prazo e status. `garantir_ausencia` lê `_criada` e o teste de reabertura lê `_reaberta`. [ausencia.py:561](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:561) · [ausencia.py:691](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:691) · [ausencia.py:731](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:731) · [ausencia.py:1055](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1055) · [ausencia.py:1082](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1082) · [ausencia.py:1271](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1271) · [ausencia.py:1422](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:1422) · [dia_decidido.py:570](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/dia_decidido.py:570) · [dia_decidido.py:678](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/dia_decidido.py:678) · [test_atestado_f3_contrato.py:380](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:380) |
| Linhas do lote, `pode_ver_anexo`, `url_absoluta`, `tipo` do arquivo, `foto`, `anexo`, contador e bytes | A view fornece permissão e URL. O histórico produz as linhas, o lote acrescenta colaborador e exigência de documento, e o resumo lê `pk`, nome, datas, tipo e status. `tipo_real_documento` produz o tipo pelos bytes. `_foto_no_pdf` produz a imagem ou `None`, `_anexo_no_pdf` entrega o resultado ao laço, `pdf_bytes` grava marca e contador, o rodapé lê o contador, e a view entrega os bytes. [views_atestado.py:99](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/views_atestado.py:99) · [views_atestado.py:134](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/views_atestado.py:134) · [views_atestado.py:148](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/views_atestado.py:148) · [atestados_lote.py:58](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:58) · [ausencia.py:796](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/ausencia.py:796) · [atestados_lote.py:142](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:142) · [atestados_lote.py:170](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:170) · [atestados_lote.py:189](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:189) · [atestados_lote.py:209](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:209) · [atestados_lote.py:224](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/services/atestados_lote.py:224) |

**Execução somente leitura.** `green_c9` passou 13 testes e `selos` passou 14. Os 11 casos de mutação selecionados falharam nas asserções esperadas. O PDF real parou no import de Pillow. PDF real, integração com banco, comportamento em produção e REDs históricos que dependem de `git show` na cópia sem `.git` ficam **[nao verificado]**. [mutantes_atestado_f3.py:311](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/mutantes_atestado_f3.py:311) · [mutantes_atestado_f3.py:342](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/mutantes_atestado_f3.py:342) · [test_atestado_f3_contrato.py:801](/home/ronald/codex_ws/1e47235167e32694eadd2ffb4f8d1aca4b4db7ef/app/ponto/tests/test_atestado_f3_contrato.py:801)

## 26/11 17:20 · sessao f9bf31851912 · DIA-DA-BATIDA-JUIZ-UNICO · verificador · origem=Code

**REPROVADO — F4c.** K2 corrige a releitura da batida, mas a ordem de travas ainda admite deadlock. K3 corrige a retentativa quando a exceção chega ao callback, mas o escritor real pode devolver `0` após uma falha e o callback registra sucesso. K5 está curado para o caso de vínculo igual. `app/ponto/registro_batida.py:329–344`, `app/chamados/services/regularizacao_ext.py:122–156`, `app/ponto/services/fechamento.py:1326–1335,1833–1850`, `app/ponto/tests/test_juiz_unico_eventos.py:451–465`.

## Bloqueios

1. **K2 — a ordem de travas difere entre escritores.** `apagar` toma **Colaborador → Batida** e usa a linha relida para julgar e excluir. A correção por regularização externa entra em `atomic`, faz `UPDATE` direto de `tipo` e `timestamp` da **Batida**, e o `post_save` pode pedir a trava de **Colaborador** pelo reconciliador. A regularização aceita a batida-alvo por PK e colaborador, inclusive uma `offline_pendente`. O ciclo é possível: a regularização segura a Batida, `apagar` segura o Colaborador e espera a Batida, e o receiver da regularização espera o Colaborador. O ramo que pede essa trava exige uma pergunta candidata e pode alcançá-la mesmo com a disputa fechada. Ocorrência em produção `[nao verificado]`. `app/ponto/registro_batida.py:329–340`, `app/chamados/views_regularizacao.py:102–115`, `app/chamados/services/regularizacao_ext.py:57–66,122–156`, `app/ponto/signals.py:216–228`, `app/chamados/reconciliador.py:651–680,740–762,1580–1581`, `app/chamados/models.py:2892–2898,2933–2941`.
2. **A exclusão pode concluir com zero competências recalculadas.** Em memória, usei a fixture de corte 21, com batidas em 20 e 21/11, e fiz o juiz lançar `RuntimeError` nas três chamadas. `apagar` retornou `1`, deixou só a entrada e chamou a trilha. O callback foi registrado e executado, mas fez **zero recálculos**. A união dos dias ficou vazia e `[]` suprime o uso de `quando`. Houve um `WARNING` e dois `ERROR`, portanto a falha deixou log. A resposta da rejeição segue sendo de sucesso e nenhuma competência fica registrada para retomada por esse evento. Efeito em valores pagos `[nao verificado]`. `app/ponto/tests/test_juiz_unico_eventos.py:272–294,572–575`, `app/ponto/registro_batida.py:197–205,334–347`, `app/ponto/signals.py:19–35,90–98,113–122,148–150`, `app/ponto/services/fechamento.py:1830–1856`, `app/ponto/views.py:496–500`.
3. **K3 — retorno do escritor é tratado como sucesso.** Uma exceção dentro do `try` do colaborador, por exemplo no `get_or_create` do fechamento, cai no `except` mensal. Ele registra `ERROR`, segue e devolve a contagem de processados. Para um único colaborador, esse caminho devolve `0`. O callback ignora a contagem, adiciona a competência e emite `INFO` de sucesso. Com dois dias da mesma competência e retornos simulados `[0, 1]`, observei só a primeira chamada e o `INFO`. O teste oficial injeta uma exceção que sobe ao callback e deixa esse caso passar. A lavratura de `DiaPago` também tem capturas próprias que permitem incrementar `processados` após falha. `app/ponto/services/fechamento.py:637–639,879,918–921,1262–1265,1321–1335,1837–1850`, `app/ponto/tests/test_juiz_unico_eventos.py:154,410–420`.

## Caminho da falha do juiz e ordem dos outros escritores

- **Onde engole.** Em `apagar`, o `atomic` interno sai primeiro e desfaz o savepoint se a exceção do juiz sair do bloco. Em seguida, `_proteger_fechamento_por_evento` captura no `except Exception` da linha 201 e registra o traceback. Depois do `delete`, o núcleo tenta o mesmo juiz com a versão anterior e captura na linha 33 de `signals.py`. Se `_dias_exclusao` continuar ausente, o cartório tenta de novo na linha 117, dentro de `engolir`, cuja captura fica em `core/observ.py:90`. O segundo uso de `_proteger_fechamento_por_evento`, em `apagar`, também captura pelo mesmo `except` da linha 201. O teste existente cobre a falha **só da leitura viva**, seguida de recuperação pelo `post_delete` e recálculos de 11 e 12/2026. `app/ponto/registro_batida.py:197–205,336–344`, `app/ponto/signals.py:19–35,53–55,90–98,113–122,148–150`, `app/core/observ.py:76–91`, `app/ponto/tests/test_juiz_unico_eventos.py:313–334`.
- **Outros caminhos de Batida.** `registrar_batida` faz `INSERT` sem tomar a trava comum na porta. `anotar` faz `UPDATE` sem essa trava na porta. Flip, retratação, aprovação offline e foto passam por `anotar`. O veredito manual toma a trava do colaborador antes de chamar flip ou criar, e a aprovação de justificativa também a toma no início do ato. No lote de ensaio, `bulk_create` e `QuerySet.delete()` passam pela guarda de banco de ensaio. `app/ponto/registro_batida.py:120–183,265–297,301–315,350–365,370–402`, `app/ponto/services/flip_batida.py:25–33`, `app/ponto/views.py:490–499`, `app/ponto/services/veredito_celula.py:229–231,318–329`, `app/ponto/services/justificativa.py:126–142,187–202`.
- **Fechamento mensal.** O evento roda após o commit da exclusão. O escritor lê as batidas por `batidas_apuraveis`, sem `select_for_update`, e grava `FechamentoMensal` por `get_or_create`, `save` ou `update`. A lavratura de `DiaPago` usa `delete` e `bulk_create`. A sequência **Colaborador → Batida** é da exclusão, e o ciclo identificado acima passa pelo receiver de Batida. `app/ponto/registro_batida.py:329–345`, `app/ponto/services/fechamento.py:639–653,918–921,1161–1185,1215–1226,1852–1856`, `app/ponto/turnos.py:2871–2897`, `app/ponto/services/dia_pago.py:357–361`.
- **Quem retenta.** Só `_rodar` lê `competencias`, para pular uma chave já marcada naquele callback. Quando a exceção chega ao callback, ele avisa com dia, competência se resolvida e motivo. Outro dia presente na mesma lista pode tentar a mesma competência. Esgotada a lista, esse caminho não deixa tarefa pendente. O DP pode recalcular pela tela ou pelo comando `recalcular_fechamento --apply`. O comando está fora do cron, e o `pre_fechamento` noturno escreve a pauta de bloqueios. Outro evento da mesma competência pode disparar novo cálculo. A recuperação de uma falha específica em produção fica `[nao verificado]`. `app/ponto/services/fechamento.py:1830–1856`, `app/config/crons.py:1148–1155`, `app/ponto/views.py:1253–1267,1280–1281`, `app/ponto/management/commands/recalcular_fechamento.py:110–134`, `app/folha/management/commands/pre_fechamento.py:7–14,53–61`.

## Testes e mutantes [nao verificado]

A suíte da cópia passou com **23 testes**. Os quatro mutantes pontuais F4c deram **RC 1, `FAIL=1`, `ERROR=0`** cada. `app/ponto/tests/test_juiz_unico_eventos.py:85–161`, `app/ponto/tests/juiz_unico_eventos_mutantes.py:40–45,189–196`.

| Mutante | Asserção que mordeu |
|---|---|
| `f4c-k2-sem-releitura` | Saiu só 11/2026, faltou 12/2026. O teste também verifica atômico e trava do colaborador antes da releitura, a instância entregue ao juiz e o PK limpo. `app/ponto/tests/juiz_unico_eventos_mutantes.py:136–138`, `app/ponto/tests/test_juiz_unico_eventos.py:363–381`. |
| `f4c-k3-marcar-antes` | Houve uma tentativa de 10/2026, esperavam-se duas. `app/ponto/tests/juiz_unico_eventos_mutantes.py:139–142`, `app/ponto/tests/test_juiz_unico_eventos.py:410–420`. |
| `f4c-k3-warning-antigo` | Faltou `dia=2026-10-23`. A mesma rotina também verifica a ausência da promessa de cron. `app/ponto/tests/juiz_unico_eventos_mutantes.py:143–148`, `app/ponto/tests/test_juiz_unico_eventos.py:391–408`. |
| `f4c-k5-vinculo-sempre-diferente` | A sonda emitiu `vinculo_diferente` com os mesmos dois IDs `1`, onde se esperava `vinculo_igual`. O teste injeta a dupla reivindicação em `turnos_do_colab`. A sonda chama o **`vinculo_do_dia` real**, no ramo de vigência, com células e candidatos de fixture. A formação da dupla pelo pareador real fica fora da prova desse teste. `app/ponto/tests/juiz_unico_eventos_mutantes.py:149–150`, `app/ponto/tests/test_juiz_unico_eventos.py:155–161,209–215,451–465`, `app/ponto/tests/sonda_pk_multiplas_f4b.txt:25–43`, `app/escala/alimentacao.py:205–216,269–286`. |

O `f4c-base` literal parou antes dos testes porque a cópia não tem `.git`. Inverti em memória os trechos de produção do patch fornecido e rodei os mesmos três seletores. Resultado **RC 1, `FAIL=3`, `ERROR=0`**, nas mesmas asserções de releitura, retentativa e dia do aviso. Identidade com o objeto Git da base e hash da cópia `[nao verificado]`. `app/ponto/tests/juiz_unico_eventos_mutantes.py:57–59,110–111,181–196`, `app/ponto/tests/test_juiz_unico_eventos.py:377,406,418`.

## Classes do delta

- **`except`/`suppress` novos: 0.** O delta reutiliza as duas proteções de `apagar`, acrescenta um savepoint ao julgamento vivo e muda o conteúdo do aviso no `except` existente do callback. O `suppress(RuntimeError)` do teste já existia no trecho de contexto. `app/ponto/registro_batida.py:197–205,336–344`, `app/ponto/services/fechamento.py:1846–1856`, `app/ponto/tests/test_juiz_unico_eventos.py:324–329`.
- **Ordem e RED.** A inversão está no bloqueio 1. Retirei apenas o savepoint interno em memória e os testes de K2 e de falha da leitura viva continuaram verdes. O teste usa `MagicMock` para `atomic` e `RuntimeError` para a falha. O rollback de erro SQL real `[nao verificado]`. O segundo escritor direto de Batida é anterior ao delta e participa da inversão. `app/ponto/registro_batida.py:329–340`, `app/ponto/tests/test_juiz_unico_eventos.py:94–99,313–334,363–371`, `app/chamados/services/regularizacao_ext.py:132–156`, `app/core/portas.py:98–105`.
- **Juiz, derivação, relógio e dia.** As duas leituras de exclusão usam a mesma autoridade, `julgar_evento_da_batida`, em estados diferentes. O callback reaproveita `_so_a_data` e a cadeia de competência já existente. A normalização recebe a data ou o instante do fato e não lê a hora atual. `dias_da_jornada` explícito e `quando` têm entradas distintas, com `None` selecionando `quando` e lista vazia representando zero dias. O comentário antigo sobre o cron ainda ficou na porta de registro. `app/ponto/turnos.py:3037–3057`, `app/ponto/signals.py:23–25`, `app/ponto/registro_batida.py:192–195,337–344`, `app/ponto/services/fechamento.py:1582–1594,1618–1628,1821–1837`, `app/config/crons.py:1148–1155`.

## Quem escreve e quem lê os valores do delta

| Campo ou valor | Escritor → leitor |
|---|---|
| `Batida.pk`, `colaborador_id`, `colaborador`, `timestamp`, `tipo`, `retratada_em` | A criação ORM grava identidade, dono, instante e tipo. `anotar` grava tipo ou retratação, e a regularização também altera tipo e instante diretamente. `apagar` lê o dono da instância recebida para travar e o PK para reler. O juiz lê a identidade e o histórico apurável. O evento lê dono e instante da linha relida. Ao sair do atômico, `apagar` zera o PK da instância recebida. `app/ponto/models.py:22–24,63–67`, `app/ponto/registro_batida.py:172–182,233–236,285–288,322–346`, `app/chamados/services/regularizacao_ext.py:150–156`, `app/ponto/turnos.py:2890–2897,2972–2994,3037–3044`. |
| `origem`, `aprovada`, `_snap`, `acao`, `descricao`, `antes`, `depois` | A criação e a aprovação escrevem os estados. A view lê `origem` e `aprovada` para selecionar, monta `_snap` e passa os argumentos da trilha. A releitura de `apagar` filtra somente por PK. Logo, aprovação concluída entre a seleção da view e a releitura ainda pode ser seguida de exclusão. `_trilha` passa o PK guardado e `depois=None` ao escritor de `LogAuditoria`. O histórico de retratação consulta a trilha por modelo e PK. `app/ponto/views.py:475–500`, `app/ponto/registro_batida.py:172–180,255–262,331–345`, `app/core/auditoria.py:17–25`, `app/core/observ.py:133–138`, `app/ponto/services/retratar_humano.py:35–40`. |
| `dias_vivos`, `_dias_exclusao`, `dias_da_jornada` | O juiz produz `.dias`. `apagar` inicializa os dias vivos e limpa `_dias_exclusao` da linha relida. O núcleo escreve `_dias_exclusao` no `post_delete`, e o cartório lê ou preenche se ausente. `apagar` une e ordena as duas fontes e entrega os dias ao callback. `app/ponto/turnos.py:3034–3057`, `app/ponto/signals.py:23–32,113–122`, `app/ponto/registro_batida.py:334–344`, `app/ponto/services/fechamento.py:1830–1837`. |
| `quando`, `dia`, empresa/corte, `(mês, ano)`, `competencias`, `motivo` | As portas entregam o fato e o motivo, inclusive `exclusao`. O cadastro grava o corte da empresa. `_so_a_data` normaliza o fato, `janela_atual` aplica o corte e `competencias_do_intervalo` devolve mês e ano do fim. O callback mantém o conjunto local, entrega mês, ano e PK do colaborador ao escritor e usa dia, competência e motivo no log. `app/ponto/registro_batida.py:208–213,342–344`, `app/colaboradores/views.py:538–558`, `app/ponto/janelas.py:36–63`, `app/ponto/services/dia_pago.py:702–719`, `app/ponto/services/fechamento.py:1582–1594,1618–1628,1830–1856`. |
| Resultado gravado e rótulo da sonda | O escritor mensal grava `FechamentoMensal` e chama a lavratura de `DiaPago`. `classificar_export` consulta o mensal gravado quando recebe `fechamentos=None`. Na sonda, o produtor de `Turno` fornece `data_turno`, `entrada`, `saida`, `batidas` e `aberto`. O resolvedor fornece o vínculo. A sonda lê os PKs, imprime o rótulo de igualdade e o teste lê a linha `PK`. `app/ponto/services/fechamento.py:1161–1185,1215–1226`, `app/ponto/services/dia_pago.py:357–361`, `app/folha/export.py:782–788`, `app/ponto/turnos.py:313–324`, `app/ponto/tests/sonda_pk_multiplas_f4b.txt:16–43`, `app/ponto/tests/test_juiz_unico_eventos.py:459–465`. |

A triagem fornecida relata zero PKs com dupla reivindicação nas duas janelas. O stdout bruto da sonda, a ocorrência dos cenários de falha e seus efeitos em produção ficam `[nao verificado]`.

## 26/11 17:09 · sessao 2db3d8ae1bea · O30-F3-CONSTRUIR · construtor · origem=Code

**O30-F3-CONSTRUIR c9 entregue como diff local sobre `7ccdc8068`.** A regra da volta proíbe `git commit` e escrita em `.git`. O HEAD permanece na base.

- **PDF do lote.** O único `except` novo cobre `Image.open` e a decodificação. O bloco registra `ANEXO ILEGIVEL #<pk>`, o lote segue e o rodapé mostra `anexos ilegiveis: N`, inclusive zero. O chamador é `lote_atestados`. `app/ponto/services/atestados_lote.py:142`, `app/ponto/services/atestados_lote.py:209`, `app/ponto/services/atestados_lote.py:224`, `app/ponto/views_atestado.py:136`
- **Reabertura.** A porta captura o nome que o `FieldFile` gravou nesta chamada. Após a falha, a compensação exclui apenas esse nome, após consultar as referências de `Ausencia` fora do `atomic`. Em `atomic` externo, preserva o arquivo e registra o nome como possível órfão. Atualizei o teste de integração para esse contrato e o selo vizinho para reconhecer a exclusão no storage. `app/ponto/services/ausencia.py:241`, `app/ponto/services/ausencia.py:1413`, `app/ponto/services/ausencia.py:1435`, `app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:164`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:282`
- **Escritor único.** O censo passou a contar a importação da coleção por alias. Os dois mutantes em memória falharam por asserção no sítio injetado, um com `update(**dict.fromkeys(...))` e outro com `setattr` via alias de módulo. `app/ponto/tests/test_atestado_f3_contrato.py:130`, `app/ponto/tests/test_atestado_f3_contrato.py:165`, `app/ponto/tests/test_atestado_f3_contrato.py:284`

### RED → GREEN

Comandos a partir de `app/`. `green_c9` passou em **13 testes** e `selos` em **14 testes** locais. `app/ponto/tests/mutantes_atestado_f3.py:85`, `app/ponto/tests/mutantes_atestado_f3.py:342`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 red_c8_pdf_decode
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 red_c8_reabrir_fatos
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 red_c8_censo_alias
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 green_c9
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 selos
```

- **PDF:** a c8 propagou `OSError: truncado`. Na c9, o teste local verificou a linha com o ID, a inclusão da foto boa e o contador. O mutante que deixou o contador em zero caiu na asserção `0 != 1`. `app/ponto/tests/test_atestado_f3_contrato.py:784`
- **Reabertura:** na c8, falharam `on_commit` após confirmação, nome antigo defasado, rollback externo sem log, falha dentro de `atomic` externo e referência por outra ausência. A compensação antes do commit já passava e continuou passando. `app/ponto/tests/test_atestado_f3_contrato.py:419`, `app/ponto/tests/test_atestado_f3_contrato.py:439`, `app/ponto/tests/test_atestado_f3_contrato.py:456`, `app/ponto/tests/test_atestado_f3_contrato.py:474`, `app/ponto/tests/test_atestado_f3_contrato.py:489`, `app/ponto/tests/test_atestado_f3_contrato.py:501`
- **Alias:** na c8, o `from ... import ... as ...` escapava. O acesso por atributo do alias de módulo já era detectado. Na c9, ambos são rejeitados. `app/ponto/tests/test_atestado_f3_contrato.py:165`, `app/ponto/tests/test_atestado_f3_contrato.py:284`

Os nove mutantes abaixo retornaram **rc 1 por asserção**. `app/ponto/tests/mutantes_atestado_f3.py:62`, `app/ponto/tests/mutantes_atestado_f3.py:101`

```bash
for caso in \
  c9_pdf_engole_sem_contar \
  c9_reabrir_ignora_referencia \
  c9_reabrir_confunde_nome_com_upload \
  c9_reabrir_apaga_em_atomic_externo \
  c9_reabrir_nao_loga_orfao \
  c9_reabrir_nao_compensa \
  c9_selo_confunde_delete_do_storage \
  c9_import_alias_fromkeys \
  c9_modulo_alias_setattr
do
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 "$caso"
done
```

**PDF real [nao verificado].** O teste gera dois JPEGs de entrada, extrai o texto do PDF e confere a única imagem embutida. Os três comandos pararam na importação do Pillow no ambiente local. A integração com banco e a certificação da sessão ficam [nao verificado]. `app/ponto/tests/test_atestado_f3_contrato.py:801`, `app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:164`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 red_c8_pdf_real
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 green_c9_pdf_real
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 c9_pdf_engole_sem_contar_real
```

### Delta e contagem AST

Saída de `git -C .. diff --name-only 7ccdc8068 -- app/`, com a contagem de `ExceptHandler` anexada. `with suppress` ficou em **0 → 0** em todos os arquivos.

| Arquivo | c8 → c9 |
|---|---:|
| `app/ponto/services/atestados_lote.py` | 2 → 3 |
| `app/ponto/services/ausencia.py` | 13 → 13 |
| `app/ponto/tests/mutantes_atestado_f3.py` | 0 → 0 |
| `app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py` | 1 → 1 |
| `app/ponto/tests/test_atestado_f3_contrato.py` | 0 → 0 |
| `app/ponto/tests/test_lote7_selo_escritor_acorda.py` | 0 → 0 |

**Fora do delta:** MAPA, ARQUITETURA, máscara da sombra, RELATO e BACKLOG seguem com a sessão. B1b/B2c, B2a, B5 e INSS seguem em fila. `app/templates/ponto/partials/painel_ausencia.html:71`, `app/ponto/services/ausencia.py:694`, `app/ponto/services/ausencia.py:1861`, `app/ponto/views.py:2596`

```text
O30-F3 c9: PDF tolera imagem ilegível, reabertura compensa por referência e selo cobre aliases

OBRA: O30-F3-CONSTRUIR
LEI-AKITA: origem=ponto/services/ausencia.py:1388, testemunha=ponto/services/ausencia.py:262, RED=ponto/tests/mutantes_atestado_f3.py:85, quem-mais-le=ponto/views_atestado.py:136, juizes novos=0, padrao=escritor unico
```

## 26/11 16:58 · sessao 4a17fc90a9db · DIA-DA-BATIDA-JUIZ-UNICO · construtor · origem=Code

Implementei K2, K3 e K5 na cópia sobre `0d081f585`. O delta segue o padrão LEI-AKITA de juiz único, com **0 juízes novos e 0 `except` adicionados ou removidos**. O HEAD permaneceu na base pela restrição de commit do pedido. `ponto/registro_batida.py:329` `ponto/services/fechamento.py:1836` `ponto/tests/juiz_unico_eventos_mutantes.py:14`

- **K2.** `apagar` abre `atomic`, trava o colaborador via `PerguntaDisputa.trava_na_ordem` e relê a batida com `select_for_update`. Julga e exclui a mesma instância relida e agenda a união dos dias com os do `post_delete`. Um savepoint mantém a proteção do delete se o julgamento vivo falhar. `ponto/registro_batida.py:329–346`
- **K3.** A competência só entra no conjunto após o recálculo retornar com sucesso. O warning informa o dia local tentado e a competência quando resolvida. A agenda declarada deixa `recalcular_fechamento` fora do cron, e o warning agora relata a falha sem prometer rega. `ponto/services/fechamento.py:1833–1853` `config/crons.py:1148–1155`
- **K5.** O teste fornece à sonda dois turnos sintéticos que reivindicam a mesma PK e usa o resolvedor real de vínculo. Com o vínculo `1` nos dois dias, a sonda imprime `vinculo_igual`. `ponto/tests/test_juiz_unico_eventos.py:451–465` `ponto/tests/sonda_pk_multiplas_f4b.txt:28–43`

### RED → GREEN

| Caso | Saída vermelha | Com a correção |
|---|---|---|
| K2 | `[(11, 2026)] != [(11, 2026), (12, 2026)]` | Agenda novembro e dezembro após a troca simulada de `S` para `E`. `ponto/tests/test_juiz_unico_eventos.py:346–380` |
| K3, nova tentativa | Uma chamada para `10/2026` onde eram esperadas duas | O segundo dia tenta a mesma competência após a primeira falha. `ponto/tests/test_juiz_unico_eventos.py:410–420` |
| K3, warning | `AssertionError: 'dia=2026-10-23' not found` | Os avisos identificam os dias tentados e só incluem a competência resolvida. `ponto/tests/test_juiz_unico_eventos.py:391–408` |
| K5 | O mutante imprime `vinculo_diferente` onde o teste exige `vinculo_igual` | O mutante sobreviveu ao teste F4b e caiu no caso novo. A sonda original passou. `ponto/tests/test_juiz_unico_eventos.py:431–465` `ponto/tests/juiz_unico_eventos_mutantes.py:149` |

`f4c-base` retornou **RC=1, FAIL=3, ERROR=0**. Cada um dos quatro mutantes retornou **RC=1, FAIL=1, ERROR=0**. Passaram **23 testes em memória**, **35 da catraca de turno** e **10 verificações estáticas de trava e escrita**, com `SimpleTestCase` substituído em memória nas catracas. `ponto/tests/juiz_unico_eventos_mutantes.py:136–153` `ponto/tests/juiz_unico_eventos_mutantes.py:177–196` `chamados/tests/test_e1_v7_catracas.py:385` `ponto/tests/test_chokepoint_batida_gate.py:42`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v ponto.tests.test_juiz_unico_eventos
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.juiz_unico_eventos_mutantes f4c-base
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.juiz_unico_eventos_mutantes f4c-k2-sem-releitura
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.juiz_unico_eventos_mutantes f4c-k3-marcar-antes
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.juiz_unico_eventos_mutantes f4c-k3-warning-antigo
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.juiz_unico_eventos_mutantes f4c-k5-vinculo-sempre-diferente
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.juiz_unico_eventos_mutantes catraca
```

Ficaram fora o lote de ensaio, a decisão sobre o dono da PK e a rega durável. O comentário legado sobre rega está fora de `apagar`. O `flip_tipo` da cópia chama `anotar` após a guarda de competência, e a trava global desta volta foi aplicada no delete. `ponto/registro_batida.py:395` `ponto/turnos.py:3037` `config/crons.py:1148` `ponto/registro_batida.py:192` `ponto/services/flip_batida.py:21–33`

Concorrência em PostgreSQL, efeitos em produção e certificação pelo Code e pelo opus final [nao verificado].

## 26/11 16:56 · sessao 5ddadef41854 · RAMO2-B-EM-TURNO-QUE-CRUZA · verificador · origem=Code

REPROVA (P1 ainda perde pausa na entrada recortada. P2 e P3 deixam passar escritas fora da porta.) `app/ponto/calculador/alimentacao.py:136–178` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:519–600` · `app/ponto/tests/test_contract_juiz_batida.py:376–402`

### 1. P1 — caso original curado, equivalência ainda incompleta

Reproduzi em memória o 12x36 de **19:00 a 08:20 do dia seguinte**, com marco **07:00**, declarado **60 min** e previsto **660 min**, salvo onde indicado. Os minutos da madrugada ficam no dia de início do turno. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:428–505` · `app/ponto/calculador/nucleo.py:133–153`

| Pausa | Motor e oráculo, em minutos |
|---|---|
| **06:40–07:20**, cruza o marco | Gozo na base **20**, base líquida **700**, intra **40**. Sem o par sintético, o oráculo voltava a **0** de gozo e **60** de intra. `app/ponto/calculador/alimentacao.py:173–178` · `app/ponto/motor_calculo_v2.py:2875–2902` |
| **07:00–07:20**, começa no marco | Gozo **0**, base **720**, intra **60**. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:497–505` · `app/ponto/calculador/alimentacao.py:174–178` |
| **06:40–07:00**, termina no marco | Gozo **20**, base **700**, intra **40**. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:497–505` · `app/ponto/calculador/alimentacao.py:174–178` |
| **00:00–00:20 e 06:40–07:20**, dois vãos | Gozo **40**, base **680**, intra **20**. `app/ponto/calculador/alimentacao.py:136–178` · `app/ponto/calculador/nucleo.py:146–153` · `app/ponto/motor_calculo_v2.py:2875–2902` |
| **06:40–07:20**, previsto **0** | Gozo **20**, base líquida **700**, base bruta para o piso **720** em ambos. Sem o par sintético, o oráculo usava **700** para o piso. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:486–495` · `app/ponto/calculador/regras.py:765–783` |
| **07:20–07:40**, pausa toda na cauda | Caso do corte confirmado no código e na sonda: **HE 0, intra 60, cauda trabalhada 60**. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:301–334` |

**Contraexemplo restante.** Entrada batida **18:30**, recortada para **19:00**, com pausas marcadas **18:50–19:20** e **06:40–07:20**, saída **08:20** e marco **07:00**. O pareador e o adaptador entregaram um turno fechado com as duas pausas à sonda. O motor recortou a primeira para **19:00–19:20**. A alimentação descartou esses 20 min porque `_ini < _a` é falso quando ambos valem **19:00**. Resultado **motor × oráculo**, em minutos: gozo **40 × 20**, base líquida **680 × 700**, intra **20 × 40**, trabalhado integral **740 × 760**. Com previsto **0**, ambos ainda exibem **720** de base bruta, pois a perda na pausa é compensada pelo excesso no trabalhado. A guarda que perde a primeira pausa já estava antes do delta. `app/ponto/models.py:26` · `app/ponto/turnos.py:1192–1199` · `app/ponto/adaptador_motor.py:37–45` · `app/ponto/motor_calculo_v2.py:1744–1759,1977–1985,2875–2902` · `app/ponto/calculador/alimentacao.py:136–178` · `app/ponto/calculador/regras.py:765–783`

### 2. Leitores de `pares_base_da_he` — ESMERIL-SEMÂNTICA

- **Produção:** o único acesso consumidor à chave é `rubricas_por_dia`, que a passa a `minutos_do_oraculo`. Da resposta `vb`, só lê **minutos** e **pausas** nos dias com cauda. Fechamento e `diff_calculador` passam pela mesma função. `app/ponto/calculador/alimentacao.py:469–475,539–554` · `app/ponto/services/fechamento.py:1291–1300` · `app/ponto/management/commands/diff_calculador.py:414–424`
- **Teste:** `PausaQueCruzaOMarcoTest._apura` lê a chave diretamente e entrega a lista ao mesmo núcleo. Os acessos em `segmentos_do_motor` constroem a lista. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:460–469` · `app/ponto/calculador/alimentacao.py:168–179`
- **Efeito do par vazio:** soma **zero** ao trabalhado, acrescenta duas pontas iguais a `vb.horas` e fecha o vão em `vb.pausas` e `vb.envelope`. Nenhum consumidor de `vb` conta pares ou turnos, ou usa `horas` ou `envelope`. O trabalhado integral vem da lista `pares`, e o universo de dias vem de `v.minutos` mais os bloqueados. `app/ponto/calculador/nucleo.py:140–154` · `app/ponto/calculador/alimentacao.py:166–178,469–497,539–542`

### 3. P2 e P3 — mutações executadas no código real, em memória

- **P2, parcial.** O AST segue algumas atribuições locais e `from ... import ... as ...`. Detectou escrita direta e construtor com apelido. Escaparam `vars(periodo)['minutos_base_da_he'] = 1` e a sequência `periodo = copiar(periodo); setattr(periodo, campo, 1)`, com `copy` ou `deepcopy` importado como `copiar` e `campo` resolvido em execução. Inserida após `_gravar_base_da_he` no `Motor12x36`, a variante com `copy` devolveu **base 1 em vez de 700** e o selo continuou vendo apenas a porta. Na cópia sem mutação, **6 alvos em 4 linhas**, todos na porta, sem falso positivo observado. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:519–633,648–683` · `app/ponto/motor_calculo_v2.py:1838–1846,2901–2902`
- **P3, parcial.** O AST exige uma ocorrência por classe concreta, pega atribuição encadeada e anotada e acusa remoção, duplicação e mudança de escopo que preserve a forma reconhecida. Não resolve apelidos de importação. Acrescentei a `_escrever_noturnas` a escrita `periodo.minutos_trabalhados = teto(0, (s - e).total_seconds() / 60)`, com `from builtins import max as teto`. O motor devolveu **800 em vez de 760** e o selo passou. A escrita extra com `max` literal foi detectada. Na cópia sem mutação, os três acertos são as três impressões esperadas, sem falso positivo observado. `app/ponto/tests/test_contract_juiz_batida.py:376–450` · `app/ponto/motor_calculo_v2.py:1039–1050,2901–2909,3016,3387`

### 4. Classes de reprovação

| Classe | Resultado no escopo |
|---|---|
| Segundo juiz | **Ausente no delta.** A alimentação lê o dia, os intervalos e o marco do motor. A autoridade do par continua registrada em `MotorBase.calcular_periodo`. `app/ponto/calculador/alimentacao.py:78–88,136–145,168–178` · `app/core/juizes.py:1216` |
| Segundo escritor fora da porta | **Ausente para a tripla no código observado.** P2 deixa a porta desprotegida contra os mutantes acima. O R3 já grava trabalhado agregado no período aberto e passa a tripla pela porta. `app/ponto/motor_calculo_v2.py:1830–1846,2645–2668` |
| Relógio fora da borda | **Ausente no delta.** O `date.today()` de `intervalo_cadastrado` é preexistente. `app/ponto/calculador/alimentacao.py:173–178` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:436–460` · `app/ponto/juiz_batida.py:129` |
| Derivação paralela | **Presente.** O motor soma intervalos recortados. O oráculo reconstrói a pausa pelos vãos entre segmentos. A entrada recortada demonstra a divergência. `app/ponto/motor_calculo_v2.py:1977–2008,2875–2891` · `app/ponto/calculador/alimentacao.py:136–178` · `app/ponto/calculador/nucleo.py:146–153` |
| Selo/RED sem mordida | **Presente para as formas escapadas de P2/P3.** O RED de P1 mordeu ao retirar o par sintético em memória. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:471–495,558–633` · `app/ponto/tests/test_contract_juiz_batida.py:376–402` |
| Ordem de trava | **Ausente no delta.** A alteração de produção opera na lista em memória. `app/ponto/calculador/alimentacao.py:168–179` |
| Pouso antes do pré-requisito | **[nao verificado]**. O pré-requisito está em `CLAUDE.md:326–335,398–399`. | [nao verificado]
| Campo com dois significados | **Presente no formato.** `pares_base_da_he` carrega segmento trabalhado e ponta sintética de pausa com a mesma tupla. Os leitores atuais não transformam a ponta em turno ou em minuto trabalhado. `app/ponto/calculador/alimentacao.py:66–70,173–178,539–542` · `app/ponto/calculador/nucleo.py:140–154` |

### 5. `except` e `suppress`

**Zero novos** `ExceptHandler` e **zero** chamadas ou imports novos de `suppress` no AST dos trechos acrescentados. Os `assertRaises` dos selos verificam a falha esperada dos mutantes. `app/ponto/calculador/alimentacao.py:173–178` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:427–694` · `app/ponto/tests/test_contract_juiz_batida.py:44–47,56–65,374–457`

Usei o delta anexado e sondas em memória com cadastro injetado. Identidade Git da cópia, suíte Django completa e incidência em produção **[nao verificado]**. [nao verificado]

## 26/11 16:49 · sessao 7b5d45a6c76f · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

**P1, P2 e P3 entregues no diff local.** O `HEAD` da cópia é `deed2fe72c8eeb7790b4ef12e34f524bf75cf6e0`, conferido com `git rev-parse HEAD`. A proibição explícita de commit prevaleceu sobre o trecho do brief que pedia o commit. SHA de commit da cura [nao verificado].

| P | Commit | RED na base | GREEN local | [nao verificado]
|---|---|---|---|
| P1 | Diff local | Nos dois motores, a sonda encontrou intra de 40 min. O oráculo deu 60 min e, com previsto zero, usou 700 min brutos em vez de 720. | O oráculo mede 20 min de pausa até o marco, chega a 40 min de intra e 700 min líquidos. Com previsto zero, motor e oráculo usam 720 min brutos. [Cura](ponto/calculador/alimentacao.py:168) · [Testes](ponto/tests/test_ramo2b_c2_base_da_he.py:471) |
| P2 | Diff local | Com o selo da base, 33 casos de escrita em memória escaparam. | O selo detecta construtor, `replace`, `__dict__` e escrita reflexiva. Para nomes dinâmicos, segue anotações, construções, retornos de `calcular_periodo` e aliases locais. A varredura aponta a porta como escritora única. [Detector](ponto/tests/test_ramo2b_c2_base_da_he.py:511) · [Testes](ponto/tests/test_ramo2b_c2_base_da_he.py:627) · [Porta](ponto/motor_calculo_v2.py:1830) |
| P3 | Diff local | Com o selo da base, 10 subcasos de perda, escrita fora da porta ou aninhamento escaparam. | O selo exige uma impressão em cada um dos três `calcular_periodo` concretos. Reconhece atribuição encadeada e qualquer expressão no segundo argumento de `max(0, X)`. [Selo](ponto/tests/test_contract_juiz_batida.py:376) · [Mutantes](ponto/tests/test_contract_juiz_batida.py:404) |

**Verificação do construtor.** Os 25 testes focalizados passaram na sonda em memória, com dublês de Django, ORM e entrada da janela. A checagem adicional de linhas do censo reproduziu a mesma falha em `deed2fe72` e no diff atual. Por exemplo, ela espera a linha 530 e encontra 531 nas duas versões. Suite integral, opus final e incidência em produção [nao verificado]. [Sonda de P1](ponto/tests/test_ramo2b_c2_base_da_he.py:427) · [Checagem do censo](core/tests/test_medida_turno_do_par.py:566) · [Ocorrência](ponto/motor_calculo_v2.py:531)

**Diff conferido.** O comando `GIT_OPTIONAL_LOCKS=0 git -C .. diff --exit-code deed2fe72 -- app/ponto/motor_calculo_v2.py`, a partir de `app/`, saiu com código 0 e saída vazia. O AST do diff inteiro contou **0 `except` novos**, e o diff lista zero arquivos em `bin/`. LEI-AKITA `padrao=juiz unico`, `juizes novos=0`. SHA-256 do diff `1da2991e71f0cdc31aa759481afece1dfa0ed163db6b430c74716176a23c3579`. [Recorte](ponto/calculador/alimentacao.py:168) · [Selo da tripla](ponto/tests/test_ramo2b_c2_base_da_he.py:511) · [Selo da impressão](ponto/tests/test_contract_juiz_batida.py:376)

### Comandos dos mutantes

A partir de `app/`, os seletores abaixo executam todos os subcasos de cada forma em memória. O heredoc usa `unittest` e os mesmos dublês da sonda local. Em P1, cada `MUTANTE` deve sair com código 1 por asserção. Em P2 e P3, os próprios testes injetam as formas mutantes e exigem que o selo as detecte. [P1](ponto/tests/test_ramo2b_c2_base_da_he.py:471) · [P2](ponto/tests/test_ramo2b_c2_base_da_he.py:648) · [P3](ponto/tests/test_contract_juiz_batida.py:404)

```bash
ramo2() {
  GIT_OPTIONAL_LOCKS=0 PYTHONDONTWRITEBYTECODE=1 python3 - "$@" <<'PY'
import ast, importlib, os, pathlib, subprocess, sys, types, unittest
from zoneinfo import ZoneInfo

def modulo(nome, **atributos):
    pai, _, campo = nome.rpartition('.')
    novo = types.ModuleType(nome)
    vars(novo).update(atributos)
    sys.modules[nome] = novo
    if pai:
        setattr(importlib.import_module(pai), campo, novo)
    return novo

def extrair(caminho, destino, *nomes):
    arvore = ast.parse(pathlib.Path(caminho).read_text())
    arvore.body = [n for n in arvore.body if getattr(n, 'name', None) in nomes]
    exec(compile(arvore, caminho, 'exec'), vars(destino))

fuso = ZoneInfo('America/Sao_Paulo')
modulo('django.conf', settings=types.SimpleNamespace(TIME_ZONE=fuso.key))
modulo('django.test', SimpleTestCase=unittest.TestCase, TestCase=unittest.TestCase)
modulo('django.utils', timezone=types.SimpleNamespace(
    make_aware=lambda dt: dt.replace(tzinfo=fuso), localtime=lambda dt: dt.astimezone(fuso)))
modulo('freezegun', freeze_time=lambda *_: lambda caso: caso)
modelos = modulo('escala.models', EscalaColaborador=types.SimpleNamespace(
    objects=types.SimpleNamespace(filter=None)))
extrair('escala/models.py', modelos, 'piso_intervalo_art71')
utils = modulo('escala.utils', _esc_vigente_do_dia=None)
extrair('escala/utils.py', utils, 'minutos_previstos_do_dia', 'intervalo_declarado_do_dia')
modulo('escala.services.escala_certa', escala_certa_no_dia=None)
modulo('core.regua_cct', get_motor_cct=None)

mutante = os.getenv('MUTANTE')
if mutante:
    caminho = 'ponto/calculador/alimentacao.py'
    if mutante == 'recorte_antigo':
        fonte = subprocess.check_output(['git', 'show', f'deed2fe72:app/{caminho}'], text=True)
    else:
        antes, depois = {
            'inclui_cauda': ('append((_s_he, _s_he, _dia))',
                            'append((_segs[-1][0], _segs[-1][0], _dia))'),
            'perde_fim_no_marco': ('< _s_he <= seguinte[0]', '< _s_he < seguinte[0]'),
        }[mutante]
        fonte = pathlib.Path(caminho).read_text()
        assert fonte.count(antes) == 1
        fonte = fonte.replace(antes, depois)
    exec(compile(fonte, caminho, 'exec'), vars(modulo('ponto.calculador.alimentacao')))

unittest.main(module=None, argv=[sys.argv[0], '-v', *sys.argv[1:]])
PY
}

p1=ponto.tests.test_ramo2b_c2_base_da_he.PausaQueCruzaOMarcoTest
p2=ponto.tests.test_ramo2b_c2_base_da_he.EscritorUnicoDaBaseDaHeTest
p3=ponto.tests.test_contract_juiz_batida.JuizBatidaContratoTest

ramo2 "$p1" "$p2" "$p3"

MUTANTE=recorte_antigo ramo2 "$p1"
MUTANTE=inclui_cauda ramo2 "$p1.test_RED_pausa_que_cruza_o_marco_abate_so_a_parte_da_base"
MUTANTE=perde_fim_no_marco ramo2 "$p1.test_a_pausa_que_comeca_ou_termina_no_marco_fica_do_lado_certo"

ramo2 "$p2.test_MORDE_construtor_e_replace"
ramo2 "$p2.test_MORDE_dict"
ramo2 "$p2.test_MORDE_setattr_com_nome_dinamico"
ramo2 "$p2.test_MORDE_setattr_em_periodo_construido_ou_retornado"

ramo2 "$p3.test_MORDE_quando_perde_uma_das_tres_impressoes"
ramo2 "$p3.test_MORDE_impressao_fora_da_porta"
ramo2 "$p3.test_MORDE_impressao_aninhada_ou_duplicada_na_porta"
ramo2 "$p3.test_impressao_aceita_alvo_encadeado_e_expressao"
```

## 26/11 16:47 · sessao 60a1848e4da2 · E1-PREVISAO-24H · verificador · origem=Code

REPROVA (mudança em HE/DSR fora de previsto e falta, rateio que perde minutos, fontes divergentes para o mesmo dia e REDs que deixam regressões passar). `app/ponto/motor_calculo_v2.py:2854,3496` `app/folha/export.py:125–137` `app/escala/utils.py:1805–1813,2469–2495` `app/escala/tests/test_e1a_nucleo_sem_banco.py:93–160`

### 1. Motor e dinheiro

- **O motor é idêntico byte a byte ao de `558de6bd4`.** Comparei os arquivos das duas cópias locais com `cmp` e confirmei o blob Git `a9d1248f904db0fa22eaf49cc86a39883cb1de99` nos dois. O `except Exception` restaurado nesta volta já existia na base. `app/ponto/motor_calculo_v2.py:3415–3418`
- **As entradas do motor mudaram.** `escala/utils.py` entrega `0` quando a previsão é recusada. O motor lê esse valor e, no 12x36, usa a própria duração como limite se o previsto for zero. Nesse ramo, 665 min trabalhados, previsto anterior de 635 min e nenhum intervalo suprimido a acrescentar dão 30 min de HE50 na base e 0 na cópia. `app/escala/utils.py:2466–2475` `app/ponto/motor_calculo_v2.py:1254–1277,2849–2873`
- **O fechamento também muda DSR.** Executei em memória o cursor da base e o da cópia com o mesmo trecho de DSR do motor. Numa fatia de 21–22/09, com 30 min de HE em 21/09 e 22/09 recusado, a base deu uma semana com DSR perdido e reflexo zero. A cópia deu zero semanas perdidas e **0,5 h de reflexo**. O fechamento grava esse valor. O rateio de `folha/export.py` entra depois da apuração do motor e alimenta `horas_falta` e `DiaPago`. O TXT de falta consome a lista de datas por outra chamada. `app/ponto/services/fechamento.py:747–785,900–906,1139–1141,1238–1246` `app/ponto/motor_calculo_v2.py:3480–3524` `app/folha/export.py:784–788`

### 2. Quem escreve, quem lê e o que o valor significa

| Valor | Escritores, leitores e divergência |
|---|---|
| **Previsto do dia** | O leitor unitário calcula minutos líquidos para a data do turno. O builder calcula pela cadeia, o cartório grava o número na ata e `grade_da_celula` prefere o número lavrado. Motor e `Ausencia.save` leem o unitário. Rateio e fechamento leem a ata pela grade. Reproduzi com os leitores reais de DNA/vínculo em memória: célula com `dna.marcos=None`, 08h–20h e duas pausas válidas de 85 min dá **635 no unitário e 550 pela cadeia da grade**. Com pausa recusada, o unitário dá **0** no horizonte. `app/escala/models.py:1151–1160,1223–1235` `app/escala/utils.py:418–432,1805–1813,2469–2495` `app/ponto/services/cartorio.py:763–772` `app/escala/services/leitor_celula.py:397–412` `app/ponto/models.py:887–897` |
| **Cota de falta** | `folha/export.py` escreve o mapa de minutos por data, com frações possíveis. O juiz de ausência expande `data..data_fim` como **datas civis inclusivas**, e a grade fornece os pesos por data do turno. O rateio casa essas chaves diretamente. Fechamento e `DiaPago` convertem a cota para horas. O cartão ainda usa o total cru do registro ao montar o rótulo do dia, podendo mostrar **16h** onde a cota é **8h**. O TXT usa as datas, e a contagem de férias filtra pela data inicial do registro. `app/ponto/turnos.py:2851–2867` `app/folha/export.py:95–137,157–181,784–788` `app/ponto/services/fechamento.py:904–906` `app/ponto/services/dia_pago.py:274–277` `app/ponto/services/dia_decidido.py:254–256,554–567` `app/ferias/models.py:143–153` |
| **`Ausencia.minutos`** | Criação e edição recebem minutos informados. A aprovação persiste a derivação do `save`, que, para falta aprovada com `0`, soma o previsto unitário de cada data do intervalo. O rateio lê valor positivo como teto, enquanto um zero pode gerar desconto pelo previsto atual. O mesmo inteiro guarda duração informada ou total derivado, e o zero funciona como pedido de derivação. `app/ponto/models.py:755–759,887–897` `app/ponto/services/ausencia.py:483–502,654–659,975–988` `app/folha/export.py:133–137,176–179` |
| **Horizonte e datas do motor** | `REGRA_VERSOES` declara a regra 4 a partir da competência **10/2026**, inclusiva. `janela_atual` usa o corte da empresa. O cursor escreve `datas_previstas_trabalho` até o fim da fatia, inclusive, e o motor interpreta a data como início do turno. **18/09/2026**, com corte padrão 21, fica na regra 1 e pode entrar no conjunto pelo ramo anterior ao horizonte. Com corte 18, já fica na regra 4. O corte e a inclusão efetiva do dia da amostra são **[nao verificado]**. `app/ponto/services/cartorio.py:160,276–299` `app/ponto/janelas.py:10–33,36–63` `app/ponto/services/fechamento.py:753–771` `app/ponto/motor_calculo_v2.py:953–971` |

### 3. Conservação do rateio

**Não.** Com total gravado `M > 0`, cota anterior `A` e soma dos pesos elegíveis `P`, o código distribui `A + min(M − A, P)`. O recorte da janela vem depois. Estes casos foram executados em memória, com o rateio e a expansão reais e pesos e recusas fornecidos como entradas. A referência à L-110 exige conferir a propriedade pela regra. `app/folha/export.py:95–137,157–181` `app/ponto/turnos.py:2851–2867` `app/docs/LEIS.md:136`

| Caso sintético | Cotas em minutos | Soma |
|---|---:|---:|
| Gravado 960, pesos 480/480 e dia central recusado | 480 / 0 / 480 | 960 |
| Gravado 960, pesos 420/540 e dia central recusado | 420 / 0 / 540 | 960 |
| Gravado 720, pesos 420/540 e dia central recusado | 315 / 0 / 405 | 720 |
| Gravado 1440, pesos 420/540 e dia central recusado | 420 / 0 / 540 | **960** |
| Gravado 960, 20–22/09, corte 21, dia 21 recusado e peso 480 no dia 22 | 320 / 0 / 480 | **800** |

Em todos esses casos, `0` na tabela representa a ausência de cota para o dia recusado. A fonte dos pesos cria outra falha: um dia já corrigido pelo cadastro pode sair do conjunto de recusados e ainda carregar **peso zero da ata anterior**. O rateio usa esse zero até a nova lavra. `app/folha/export.py:129–137,157–174` `app/escala/utils.py:487–510` `app/escala/services/leitor_celula.py:397–412`

### 4–5. Juiz único e classes de reprovação

| Classe | Resultado |
|---|---|
| Segundo juiz que duplica juiz registrado | **Ausente no predicado geométrico.** `previsao_do_dia_valida` é o juiz registrado, chamado pelo cadastro, pela grade e pelo leitor do vínculo. **Presente na resposta operacional:** a grade também julga o teto escalar, a falta consulta só o mapa de células existentes, e o fechamento consulta o vínculo. A própria bateria espera que um dia sem célula possa estar inválido na grade e ainda descontar 480. O modo legado da grade também fica fora do caminho que carimba a recusa. `app/core/juizes.py:213–217` `app/docs/JUIZES.md:131–135` `app/escala/models.py:660–675` `app/escala/utils.py:435–446,487–510,2263–2271,2285–2291` `app/escala/tests/test_e1_v5_leitores.py:111–123` |
| Segundo escritor fora da porta | **Ausente nos campos auditados.** O rateio devolve um mapa, a ausência deriva no `save` e a ata passa pela porta da célula. Wizard, admin, cópia e derivação consultam a mesma validação de previsão. `app/folha/export.py:169–182` `app/ponto/models.py:878–918` `app/escala/models.py:1848–1856` `app/escala/services/cadastro_tipo.py:144–165,333–335,451–452,481–482` `app/escala/admin.py:19–24` |
| Relógio fora da borda | **Ausente nas leituras novas do horizonte.** O cálculo recebe a data e a empresa. O comando lê `hoje` e o passa ao contador. `app/ponto/services/cartorio.py:276–299` `app/escala/management/commands/lavrar_previsto_cego.py:35–39` `app/escala/services/previsao_invalida.py:22–34` |
| Derivação paralela de fonte única | **Presente.** Unitário e grade produzem 635/550 para o mesmo dia. O comercial preserva um terceiro cálculo do limite de HE pelos marcos. `previstas_do_periodo` e fechamento também divergem antes do horizonte: só o primeiro exige `responde_pelo_dia`. `app/escala/utils.py:1805–1813,2469–2495` `app/ponto/motor_calculo_v2.py:3404–3429` `app/escala/alimentacao.py:389–408` `app/ponto/services/fechamento.py:753–763` |
| Selo/RED sem mordida | **Presente nas integrações que os REDs substituem.** Os seis mutantes literais falham, mas quatro variantes comportamentais passaram, detalhadas abaixo. `app/escala/tests/test_e1a_nucleo_sem_banco.py:18–57,93–160` |
| Ordem de trava | **Ausente no delta auditado desta volta.** Os ramos novos consultam editabilidade, leem a grade e montam valores em memória, sem nova aquisição ou inversão de trava. `app/escala/services/cadastro_realidade.py:771–779` `app/folha/export.py:153–182` `app/ponto/services/fechamento.py:744–783` |
| Pouso antes do pré-requisito | **Pouso em produção [nao verificado].** A dependência em execução está **presente**: o rateio consulta a recusa vigente e aceita o peso da ata sem conferir se a correção já foi relavrada. `app/folha/export.py:157–174` `app/escala/services/leitor_celula.py:397–412` |
| Campo com dois significados | **Presente.** `Ausencia.minutos` é duração informada ou total derivado. `previsto=0` também significa recusa no novo leitor, enquanto o motor usa o mesmo zero para mudar o limite de HE e escolher a duração bruta no piso de intervalo. `app/ponto/models.py:755–759,887–897` `app/escala/utils.py:2466–2475` `app/ponto/motor_calculo_v2.py:1840–1847,2854–2860` |

### 6. REDs e C13

- Os **4 testes passaram** na cópia. Cada um dos **6 mutantes literais** falhou numa asserção: `previsto` deu 550, `aceita-invalido` deu 635, `rateio` somou 640, `cota-uniforme` deu 480/480, `c13` abriu resposta 400 e `horizonte` perdeu 18/09 do conjunto. `app/escala/tests/test_e1a_nucleo_sem_banco.py:18–34,65–160`
- **Passaram os quatro testes** com cada uma destas variantes em memória: horizonte real sempre verdadeiro, `<a>` incondicional no HTML do C13, pesos uniformes só quando o saldo é menor e rateio especial desativado só para registros que cruzam o horizonte. Os REDs injetam os leitores de DNA/vínculo e a janela, fornecem recusa e pesos prontos e exercitam a ação C13 sem renderizar o template. `app/escala/tests/test_e1a_nucleo_sem_banco.py:65–160` `app/escala/utils.py:407–410` `app/folha/export.py:135–137,170–179` `app/templates/escala/cadastro_x_realidade.html:43`
- **O C13 da cópia entrega texto sem link** para template de sistema com `editavel=False`. A lista por colaborador reutiliza a ação e as duas renderizações condicionam o link à URL. A tela com o registro real e o teste de integração que cria a ausência de 960 min ficam **[nao verificado]**. `app/escala/services/cadastro_realidade.py:747–752,771–779,1133–1147` `app/templates/escala/cadastro_x_realidade.html:32,43` `app/escala/tests/test_e1_v4_juiz_unico.py:381–391`

### 7. `except` e `suppress` no diff inteiro

**Zero novos e zero handlers modificados.** Comparei por AST os 43 arquivos Python diferentes entre as cópias de `558de6bd4` e `8d81e64c5`, incluindo testes e sondas. O `except` que aparece no anexo desta volta é restauração frente a `4a9862378` e já está na base solicitada. `app/ponto/motor_calculo_v2.py:3415–3418` `app/escala/tests/test_e1a_nucleo_sem_banco.py:38–57`

_8447 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
