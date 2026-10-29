# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 18:17** · publicado em **29/10/2026 18:17** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 18:17 · sessao 6bc00e83b369 · - · - · origem=Ronald (tmux)

**Resultado:** chamado `entrada_bloqueada_turno_aberto` vivo não é uma guarda de recusa na API: `api_bater_ponto` coleta anomalias, chama `registrar_batida`, depois abre/atualiza disputa. No formulário web, o chamado vivo é procurado para renovar seu contexto; o fluxo continua para registrar. (`app/api/views_core.py:432-433`, `app/api/views_core.py:679-694`, `app/api/views_core.py:769-781`; `app/ponto/views.py:274-314`, `app/ponto/views.py:345-353`.)

**Rotas e canais**

- Proxy declarado: `/api/ponto/*` → `core:8000` (`Caddyfile:8-9`); URLconf core inclui `api.urls_core` (`app/config/urls_core.py:6`), que importa `views_core` e liga `ponto/bater/` a `api_bater_ponto` (`app/api/urls_core.py:5`, `app/api/urls_core.py:14`). A outra casca inclui `api.urls` (`app/config/urls.py:37`), cuja rota aponta `api.views.api_bater_ponto` (`app/api/urls.py:2`, `app/api/urls.py:67`).
- `/ponto/bater/` e `/ponto/bater-v2/` → `ponto.views.bater_ponto` (`app/config/urls.py:29`; `app/ponto/urls.py:11-12`). V2 renderiza `bater_ponto_v2.html` (`app/ponto/views.py:355-357`), cujo JS faz POST `/api/ponto/bater/` (`app/templates/ponto/bater_ponto_v2.html:328-338`).
- PWA offline grava IndexedDB local, sem criar `Batida` no servidor (`app/static/pwa.js:107-122`); sincroniza por POST `/api/ponto/bater/` (`app/static/pwa.js:126-157`). HTTP OK remove o item local (`app/static/pwa.js:158-165`); HTTP400 também remove, como erro definitivo (`app/static/pwa.js:166-175`).
- Formulário web antigo faz POST à própria URL (`app/templates/ponto/triagem/apto.html:86-91`, `app/templates/ponto/triagem/apto.html:210-235`), passando por `ponto.views.bater_ponto` (`app/ponto/views.py:267-272`).
- O próprio código chama `origem='app'` de relógio: `DO_RELOGIO = 'app'` (`app/ponto/services/procedencia_batida.py:30`). App e web passam essa origem ao chokepoint (`app/api/views_core.py:680-682`; `app/ponto/services/triagem_batida.py:708-710`). `canal_da_batida(device_id)` distingue APK/PWA (`app/ponto/registro_batida.py:53-61`). Integração atual de um relógio físico separado: **[não verificado]**. A porta AEJ cadastrada (`app/ponto/urls.py:9`) apenas retorna HTTP410 (`app/ponto/views.py:56-71`).

**Passos API até chokepoint**

1. POST autenticado, obtém colaborador; sem colaborador →404 (`app/api/views_core.py:385-391`).
2. Situação diferente de ativo/afastado →403; inclui desligado e ferias (`app/api/views_core.py:402-410`).
3. Tipo fora E/S →400 (`app/api/views_core.py:419-426`).
4. Detector consulta geometria de turno órfão, acrescentando `orfao_14h` quando há bloqueio e não há regularização pendente (`app/api/views_core.py:432-433`; `app/chamados/detector_anomalias.py:29-51`). Não consulta EB vivo para recusar (`app/chamados/detector_anomalias.py:19-121`).
5. GPS ausente →403 `gps_obrigatorio`, sem chamar chokepoint (`app/api/views_core.py:440-458`).
6. Sem foto usa juiz de selfie para adicionar anomalia, sem recusa (`app/api/views_core.py:459-483`; `app/ponto/registro_batida.py:80-91`).
7. Timestamp vem de `hora_do_aparelho` (`app/api/views_core.py:536-539` — **confirmar faixa: nesta casca o bloco começa antes; referência exata desta chamada não preservada no output**).
8. Mesmo tipo em ±5min → HTTP200 `ja_registrada`, devolve existente, sem criar (`app/api/views_core.py:549-568`).
9. Tipo ao vivo pode ser corrigido pelo `proximo_tipo_de`; retroativo pode ser corrigido por marcos (`app/api/views_core.py:592-623`).
10. Saída sem entrada ou entrada sobre turno aberto adicionam motivo/contexto; não retornam erro (`app/api/views_core.py:632-672`).
11. `registrar_batida(colab, ts_efetivo, tipo, 'app', autoridade_humana=False, canal=...)` (`app/api/views_core.py:679-694`).
12. Retorno `batida is None` →HTTP200 `ignorada_intervalo`, devolvendo oposta existente e `sucesso=True`, sem criar nova (`app/api/views_core.py:695-721`).
13. Após gravação, anomalias alimentam `criar_ou_atualizar_disputa`, `em_disputa=True` (`app/api/views_core.py:769-781`); resposta normal `status='ok'`, ID e sucesso (`app/api/views_core.py:823-834`).

Clone UI possui decisões equivalentes em `app/api/views.py:398-439`, `app/api/views.py:449-466`, `app/api/views.py:557-576`, `app/api/views.py:600-631`, `app/api/views.py:683-721`, `app/api/views.py:769-781`, `app/api/views.py:823-834`.

**Formulário web e EB**

- Triagem reconstrói entradas sem saída por `batidas_apuraveis`/`parear_turnos` (`app/ponto/services/triagem_batida.py:137`, `app/ponto/services/triagem_batida.py:175-192`); `tem_bloqueio_turno` usa `14 <= horas < 96` (`app/ponto/services/triagem_batida.py:17-30`, `app/ponto/services/triagem_batida.py:206`).
- Regularização pendente depende de justificativa pendente com saída informada, não de chamado EB aberto (`app/ponto/services/triagem_batida.py:208-223`).
- POST E + bloqueio + sem regularização: procura EB vivo (`app/ponto/views.py:274-284`); se existe renova contexto (`app/ponto/views.py:285-288`), senão cria (`app/ponto/views.py:290-302`); apenas produz sinal e continua (`app/ponto/views.py:311-314`).
- **Ramo que desvia sem gravar:** POST E + `context['turno_aberto']` + sem confirmar: salva confirmação e retorna tela (`app/ponto/views.py:315-329`). O formulário dessa tela reenvia E com `confirmar=sim` (`app/templates/ponto/triagem/turno_confirmar.html:8-17`). Este `elif` também pode ser alcançado quando existe regularização pendente, pois o primeiro `if` exige ausência dela (`app/ponto/views.py:274`, `app/ponto/views.py:315`).
- POST E/S → `ramo_registrar_es` (`app/ponto/views.py:345-353`); duplica mesmo tipo nos 5min anteriores →`ja_registrada`, sem criar (`app/ponto/services/triagem_batida.py:669-677`).
- Sinal de acesso/bloqueio torna `aprovada=False` e escreve justificativa de revisão (`app/ponto/services/triagem_batida.py:692-705`); chama chokepoint (`app/ponto/services/triagem_batida.py:707-720`).
- Chokepoint retorna None →JSON `ignorada_intervalo`, sem nova batida (`app/ponto/services/triagem_batida.py:721-732`). Se retorna batida, define sucesso no contexto (`app/ponto/services/triagem_batida.py:745-753`).
- Fora de horário cria sinal e continua, sem impedir gravação (`app/ponto/services/triagem_batida.py:478-496`).

**Chokepoint: criação não é incondicional**

- Mesmo tipo não retratado em ±60s → devolve existente (`app/ponto/registro_batida.py:106-115`).
- Mesma chave colaborador/timestamp → devolve existente (`app/ponto/registro_batida.py:120-122`).
- Competência fechada recusa somente quando `fato_de_chao=False`; chamadas app/web acima omitem a flag e passam autoridade humana false, então deriva `fato_de_chao=True` (`app/ponto/registro_batida.py:130-137`).
- Oposta não retratada na janela e sem casar marco do próprio tipo, com autoridade humana false →`None, conflito`, sem criar (`app/ponto/registro_batida.py:138-151`). A janela é 300s com escala e marcos, senão60s (`app/ponto/registro_batida.py:21-32`); exceção de marco próprio está em `app/ponto/registro_batida.py:35-50`.
- Fora dessas saídas, cria `Batida` em `objects.create` (`app/ponto/registro_batida.py:157-159`), chama fechamento por evento e retorna (`app/ponto/registro_batida.py:160-161`); exceção do agendamento de fechamento é capturada (`app/ponto/registro_batida.py:175-183`).

**Desvios reais da UI v2 antes de POST**

- `bloqueio_horario` ou `aguardando_confirmacao` substitui formulário por aviso/link para tela antiga (`app/templates/ponto/bater_ponto_v2.html:64-75`).
- Sem selfie, `garantirRequisitos` retorna false (`app/templates/ponto/bater_ponto_v2.html:285-290`); sem GPS também (`app/templates/ponto/bater_ponto_v2.html:292-300`); confirmação aborta envio (`app/templates/ponto/bater_ponto_v2.html:325`).
- Qualquer HTTP OK é mostrado como sucesso sem ler o corpo; isso inclui `ja_registrada` e `ignorada_intervalo` (`app/templates/ponto/bater_ponto_v2.html:335-340`; `app/api/views_core.py:555-568`, `app/api/views_core.py:695-721`).
- Onboarding incompleto desvia antes da batida web (`app/ponto/views.py:240-243`); comunicado desvia somente GET (`app/ponto/views.py:249-254`).

## 29/10 18:17 · sessao 86a7408a2cd1 · - · - · origem=Ronald (tmux)

Achados dos testes, por leitura e AST em memória, sem banco: [nao verificado]

- **Selo tem mordida estrutural limitada.** Removendo em memória as chamadas reais, ele acusa `aprovar_justificativa_dp`, `materializar_saida_dp` e `aprovar_lote_justificativas`; o reconhecimento e a checagem ficam em `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:47` e `:94`. Baseline dos dois arquivos: nenhuma violação. Execução funcional das mutações: [nao verificado].
- **Selo aceita fechamento sem efeito ou falha engolida.** Provei com fontes sintéticos: `if False: encerrar_chamado_de_origem(j)`, `try: encerrar... except Exception: pass` e fechamento antes de `registrar_batida(j)` retornam `[]`; `_chama` apenas compara posição textual, receptor e ancestral `atomic`, em `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:99`. O ramo morto é limitação declarada em `:8`; captura da falha e pré-requisito não são verificados em `:102`.
- **Ausência de sinal não reprova o teste da batida.** `test_J2_nao_e_rejeitada_pela_BATIDA_do_proprio_ato` exige `batida_gerada_id`, J2 pendente e chamado vivo, mas não exige invocação do receiver/reconciliador, em `app/ponto/tests/test_justificativa_encerra_origem.py:413`. Sem `_reconciliador_post_save_batida`, a chamada explícita ainda retém o chamado, em `app/ponto/services/justificativa.py:194` e `app/chamados/reconciliador.py:453`. Portanto esse teste pode passar com o caminho anunciado ausente; comparação executada contra tal mutante: [nao verificado].
- **O caso de falha repetida das perguntas não prova o savepoint S143.** Ele exige rollback completo em `app/ponto/tests/test_justificativa_encerra_origem.py:256`; o resolver sem captura da unidade S128 já produz esse rollback em `app/chamados/reconciliador.py:521`. O teste específico que mira a cura do savepoint é `test_erro_de_banco_engolido_no_receiver_S143_nao_mente_o_fechamento`, em `app/ponto/tests/test_justificativa_encerra_origem.py:231`.
- **Tetos não provam a existência do savepoint.** `_cobrar` aceita qualquer contagem menor ou igual, em `app/ponto/tests/test_selo_chokepoint_escrita.py:213`; retirar o savepoint diminui queries e permanece dentro dos novos tetos de `:114` e `:157`. É comportamento adequado de teto, mas não é prova funcional da cura. As medições alegadas em `:150` dependem de logs não fornecidos: [nao verificado].
- **Sem ensaio novo de concorrência ou pós-commit.** Os novos funcionais usam `TestCase`, em `app/ponto/tests/test_justificativa_encerra_origem.py:19` e `:38`; não há teste de threads, `TransactionTestCase` ou execução explícita de callbacks no módulo. Segurança da ordem de locks e propagação do contexto em callbacks, por teste: [nao verificado].
- **Há sentinelas úteis contra vacuidade.** Rollback do fechamento exige `m.assert_called_once()` em `app/ponto/tests/test_justificativa_encerra_origem.py:170`; falha da saída idem em `:197`; recusa do coletor exige trilha em `:159`; retenção por irmã exige trilha em `:356`; o caso J2 direto exige chamada do helper em `:290`.
- **O positivo da materialização com wrapper é bem escolhido contra fechamento prévio pelo reconciliador da batida.** A fixture usa `justificativa_colab`, em `app/ponto/tests/test_justificativa_encerra_origem.py:213`; esse módulo não aparece no catálogo de premissas, em `app/chamados/catalogo/premissa.py:45`, e o reconciliador retorna `None` para classe fora do catálogo, em `app/chamados/reconciliador.py:257`. Assim o caminho comum da batida não substitui ali a nova chamada da origem.
- **O selo não cobre todos os escritores possíveis.** Ele reconhece somente `ast.Assign` com literal `'aprovada'`, em `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:47`; `update`, `create` e admin são exclusões declaradas em `:13`. Não usá-lo como prova de inexistência desses escritores.

## 29/10 18:17 · sessao 439554232df8 · - · - · origem=Ronald (tmux)

- `VIVOS = ('aberto', 'em_analise', 'registrado')`; `ENCERRADOS = ('resolvido', 'fechado')`. Fonte: `app/chamados/catalogo/motor.py:24`, `:30`.

- `Batida.objects.create` dispara o receiver `_nucleo_post_save_batida`, que recompõe os turnos; `_recompute_around` captura exceções. Fonte: `app/ponto/signals.py:19`, `:29`, `:34`.

- Em seguida `_cartorio_post_save_batida` chama `_julgar_dias` para véspera e dia da batida; este chama `cartorio.julgar_celula` dentro de `engolir`, que captura exceções. Fonte: `app/ponto/signals.py:85`, `:88`, `:93`, `:108`, `:131`; captura em `app/core/observ.py:85`.

- `_reconciliador_post_save_batida` chama `reconciliar_chamados_colab(instance.colaborador)` e captura exceções; esse reconciliador percorre todos os chamados VIVOS do colaborador. Fonte: `app/ponto/signals.py:206`, `:213`; `app/chamados/reconciliador.py:1203`.

- O módulo `entrada_bloqueada_turno_aberto` pertence à premissa `FATO_TURNO_ABERTO`. `_veredito_premissa` coleta o turno específico do chamado quando há dia; sem dia, consulta globalmente `TurnoMaterializado` com entrada e sem saída. Premissa viva segue no laço; morta chama `_retratar`. Fontes: `app/chamados/catalogo/premissa.py:48`; `app/chamados/reconciliador.py:264`, `:286`, `:139`, `:170`, `:1208`.

- `_retratar` pode **recusar o fechamento do chamado** se houver irmãs respondidas sem consumo; senão abre `transaction.atomic`, chama `ch.reconciliar()`, rejeita justificativa vinculada ainda pendente, resolve perguntas e anexa ocorrência. Isso é fechamento de chamado após o evento da Batida, não uma consulta prévia que barre a criação da Batida. Fontes: `app/chamados/reconciliador.py:435`, `:446`, `:455`, `:462`, `:469`, `:471`; chamada pelo receiver em `app/ponto/signals.py:215`.

- `ChamadoColaborador.reconciliar()` consulta `_decisao_do_fechamento`; o motor transforma VIVOS em `resolvido`, e `_gravar_estado` grava com via `sistema`. Fontes: `app/chamados/models.py:434`, `:440`, `:444`; `app/chamados/catalogo/motor.py:95`, `:111`; escritor em `app/chamados/models.py:185`, `:226`.

- O gate de premissa do fechamento pode recusar: se é chamado de furo com dia e o dia segue furado para folha, sem pai agrupador ou sobrevivente vivo, devolve `False` e registra `fechamento_recusado`. Sem dia, permite e registra `fechamento_sem_dia_do_fato`; também permite premissa regularizada ou dedup com outra cobrança viva do dia. Fonte: `app/chamados/models.py:484`, `:488`, `:495`, `:498`, `:522`, `:525`.

- Outra porta automática é `retratar_chamados_absolvidos_por_celula`: inclui EB em `MODULOS_DE_TURNO`, exige célula com veredito que não acusa e sem marcos pendentes, e chama a mesma `_retratar`. Não roda inline no signal: consumidor do outbox chama essa função em `app/chamados/reconciliador.py:1145`. Fontes: `app/chamados/reconciliador.py:336`, `:757`, `:792`, `:800`, `:814`, `:823`, `:825`.

- `reconciliar_chamados_colab` também é chamado pelo comando `reconciliar_chamados`, em `app/chamados/management/commands/reconciliar_chamados.py:52`.

- Não generalizar que todos os receivers são incapazes de propagar erro: `batida_marca_onboarding` não envolve a chamada de gravação em `try/except`; ele chama a porta do colaborador, que executa `colab.save`. Não há consulta ao chamado EB nesse receiver. Fontes: `app/colaboradores/signals.py:86`, `:94`, `:101`; `app/colaboradores/services/colaborador.py:47`, `:53`. Ocorrência concreta de exceção/rollback: **[não verificado]**.

## 29/10 17:58 · sessao 23e552545147 · grupo1p1 · verificador · origem=Code

- **Classe 3 — instante perdido no resumo.** `espelho_do_colab` e o coletor PDF recebem `agora`, mas chamam `_folha_manda` sem propagá-lo. Essa cadeia relê o relógio em `ate_ontem`, em `turno_aberto_de` quando há turno aberto e no fechamento online quando há folha. Referências: `app/ponto/services/espelho.py:1288`, `app/relatorios/pdf_espelho.py:624`, `app/relatorios/cartao_pela_celula.py:412`, `app/relatorios/cartao_pela_celula.py:40`, `app/relatorios/cartao_pela_celula.py:398`, `app/ponto/turnos.py:2255`, `app/relatorios/cartao_pela_celula.py:73`, `app/ponto/services/fechamento.py:1718`, `app/ponto/services/fechamento.py:595`.

- **Classe 3 — releitura na classificação do export.** Os atos capturam seu instante, mas continuam chamando `classificar_export` sem ele; a classificação chega a `motivos_retencao_celula` sem `hoje`, provocando outra leitura. Sítios: `app/folha/porta_export.py:469`, `app/ponto/services/dias_em_aberto.py:104`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:53`, `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:58` e `app/ponto/management/commands/e6_oraculo.py:269`. Cadeia: `app/folha/export.py:942`, `app/folha/export.py:1249`, `app/folha/export.py:1179`.

- **Classe 3 — ficha com vários instantes.** Depois da captura própria, `ficha_do_colab` chama `particionar_verbos`, que lê outro relógio; com propostas habilitadas, também alcança a leitura de `_recusadas_recentes`. Referências: `app/colaboradores/services/ficha.py:117`, `app/colaboradores/services/ficha.py:155`, `app/chamados/services/verbos.py:32`, `app/colaboradores/services/ficha.py:176`, `app/escala/services/propositor.py:538`, `app/escala/services/propositor.py:622`.

- **Classe 3 — avaliação de flips com três leituras.** `avaliar_fila` captura `agora`, mas `fila_flip` consulta duas fontes que leem seus próprios relógios para os cortes da fila. Referências: `app/ponto/services/flip_auto.py:263`, `app/ponto/services/flip_auto.py:268`, `app/ponto/services/flip_auto.py:146`, `app/ponto/services/flip_auto.py:148`, `app/ponto/services/flip_auto.py:34`, `app/ponto/services/flip_auto.py:126`.

- **Classe 3 — PDF auditável perde o instante.** O gerador captura o instante para os dados, mas chama `gerar_pdf_auditavel` sem propagá-lo; o pipeline lê outro instante para o rodapé. Referências: `app/relatorios/pdf_espelho.py:1088`, `app/relatorios/pdf_espelho.py:1099`, `app/relatorios/pdf_base.py:133`, `app/relatorios/pdf_base.py:139`.

- **Classe 3 — reconciliação consulta outro relógio no julgamento.** O comando captura `_agora_do_ato`, mas chama `classificar_falta` sem esse instante; o juiz compara o fim previsto com um novo `now()`. Referências: `app/ponto/management/commands/reconciliar_grade.py:75`, `app/ponto/management/commands/reconciliar_grade.py:243`, `app/escala/servico_jornada.py:376`.

- **Classe 3 — duas leituras no teste do contrato.** A janela usa `timezone.localdate()` e a grade recebe um `timezone.now()` separado. Referências: `app/escala/tests/test_f1c_contrato.py:33`, `app/escala/tests/test_f1c_contrato.py:35`.

- **Classe 3 — duas leituras em `_pares`.** A mesma chamada obtém a referência por `localdate()` e o instante por `now()`, separadamente, mesmo sob congelamento. Referência: `app/ponto/tests/test_par_relampago_pelo_turno.py:55`.

- **Classe 3 — teste do cartão usa relógios separados.** O coletor recebe `now()`, mas a expectativa chama `ate_ontem` sem `hoje` e depois lê `localdate()` novamente. Referências: `app/relatorios/tests/test_cartao_pela_celula.py:58`, `app/relatorios/tests/test_cartao_pela_celula.py:61`, `app/relatorios/tests/test_cartao_pela_celula.py:62`, `app/relatorios/cartao_pela_celula.py:40`.

- **Classe 5 — selo deixa voltar o relógio interno do builder.** Repor `ref = ref or timezone.localdate()` no builder por turno mantém vazios os resultados dos três verificadores AST: a inspeção de relógios só examina funções que chamam um `ALVO`, e esse builder não chama nenhum. A verificação de assinatura permanece satisfeita. Referências: `app/escala/utils.py:1234`, `app/escala/tests/test_selo_grade_pede_o_instante.py:103`, `app/escala/tests/test_selo_grade_pede_o_instante.py:106`, `app/escala/tests/test_selo_grade_pede_o_instante.py:191`.

Total por classe: 1=0; 2=0; 3=9; 4=0; 5=1; 6=0; 7=0; 8=0.

## 29/10 17:53 · sessao 0710e4f763be · grupo1p1 · verificador · origem=Code

1. **Classe 3 — espelho/PDF perdem o instante antes de `folha_manda`.** As chamadas em `app/ponto/services/espelho.py:1288` e `app/relatorios/pdf_espelho.py:624` não repassam `agora`. `folha_manda` chama `ate_ontem` sem referência temporal (`app/relatorios/cartao_pela_celula.py:412`), provocando outra leitura em `app/relatorios/cartao_pela_celula.py:40`. Havendo turno aberto, também consulta o juiz sem instante (`app/relatorios/cartao_pela_celula.py:398`), acionando `timezone.now()` em `app/ponto/turnos.py:2255`.

2. **Classe 3 — ficha lê vários relógios no mesmo ato.** Captura `agora` em `app/colaboradores/services/ficha.py:117`, mas chama `particionar_verbos` em `app/colaboradores/services/ficha.py:155`, que relê o relógio em `app/chamados/services/verbos.py:32`. Com propostas habilitadas, a chamada de `app/colaboradores/services/ficha.py:176` chega a outra leitura em `app/escala/services/propositor.py:622`, via `app/escala/services/propositor.py:538`.

3. **Classe 3 — fila e simulações usam instantes distintos.** `avaliar_fila` captura `agora` em `app/ponto/services/flip_auto.py:263`, mas chama `fila_flip` em `app/ponto/services/flip_auto.py:268`. Esta consulta duas fontes (`app/ponto/services/flip_auto.py:146`, `app/ponto/services/flip_auto.py:148`) que releem o relógio em `app/ponto/services/flip_auto.py:34` e `app/ponto/services/flip_auto.py:126`.

4. **Classe 3 — classificação do universo relê o dia.** `medir` captura o instante em `app/folha/porta_export.py:318`, mas chama `classificar_export` sem transmiti-lo em `app/folha/porta_export.py:469`. A cadeia `app/folha/export.py:942` → `app/folha/export.py:1249` chega a `localdate()` em `app/folha/export.py:1179`. A mesma falha alcança os chamadores tocados em `app/ponto/services/dias_em_aberto.py:104`, `app/ponto/management/commands/e6_oraculo.py:269`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:53` e `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:58`.

5. **Classe 3 — PDF auditável ganha outro instante.** O instante capturado em `app/relatorios/pdf_espelho.py:1088` alimenta os dados e o texto de geração, mas não segue na chamada de `app/relatorios/pdf_espelho.py:1099`. O pipeline relê o relógio em `app/relatorios/pdf_base.py:133` e usa esse segundo valor no rodapé em `app/relatorios/pdf_base.py:139`.

6. **Classe 5 — selo deixa voltar a segunda leitura da API.** O verificador ignora funções sem parâmetro `agora` (`app/escala/tests/test_selo_grade_pede_o_instante.py:73`) e examina apenas chamadas aos alvos enumerados (`app/escala/tests/test_selo_grade_pede_o_instante.py:80`). Restaurar `timezone.now() < _fim_hj` em `app/api/views.py:1635`, após a captura de `app/api/views.py:1350`, continua produzindo listas vazias nos dois verificadores usados pelo selo (`app/escala/tests/test_selo_grade_pede_o_instante.py:102`, `app/escala/tests/test_selo_grade_pede_o_instante.py:108`).

Total por classe, agrupado por causa: **1=0; 2=0; 3=5; 4=0; 5=1; 6=0; 7=0; 8=0.**

## 29/10 17:52 · sessao 6c4ab6f050bb · - · - · origem=Ronald (tmux)

Achados verificáveis da classe 3, agrupados por causa:

- `app/ponto/services/espelho.py:1288` e `app/relatorios/pdf_espelho.py:624`: ambos recebem `agora`, mas chamam `folha_manda` sem propagá-lo. Ela chama `ate_ontem` sem `hoje` (`app/relatorios/cartao_pela_celula.py:412`), que lê `timezone.localdate()` (`:40`). Havendo turno aberto, também chama `turno_aberto_de` sem instante (`:398`), acionando `timezone.now()` em `app/ponto/turnos.py:2255`. Havendo fechamento, chama `totais_da_folha` (`:323`), depois `fechamento_lido` (`:73`), que chama o recalculador (`app/ponto/services/fechamento.py:1718`), cujo instante é novamente lido em `:595`. Portanto, o instante injetado não governa integralmente a mesma leitura.

- `app/colaboradores/services/ficha.py:117`: captura `agora`, mas `:155` chama `particionar_verbos`, que relê em `app/chamados/services/verbos.py:32`. Com propostas habilitadas, `ficha.py:176` chama `propostas`; esta chama `_recusadas_recentes` em `app/escala/services/propositor.py:538`, que relê em `:622`. A ficha pode usar três instantes no mesmo ato.

- `app/ponto/services/flip_auto.py:263`: captura `agora`, mas `:268` chama `fila_flip`. Esta chama `pares_mesmo_tipo` em `:146`, que lê outro instante em `:34`, e `fila_flip_recusas` em `:148`, que lê novamente em `:126`. São três leituras no ato que fornece `agora` às simulações tocadas em `:303` e `:304`.

- `app/folha/porta_export.py:469`: depois de capturar o instante em `:318`, chama `classificar_export` sem referência temporal. Este chama `motivos_retencao_espelho` em `app/folha/export.py:942`, que delega a `motivos_retencao_celula` em `:1249`; ela lê `localdate()` em `:1179`. A mesma causa aparece nos sítios tocados `app/ponto/services/dias_em_aberto.py:104`, `app/ponto/management/commands/e6_oraculo.py:269`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:53` e `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:58`.

- `app/relatorios/pdf_espelho.py:1099`: após capturar o instante em `:1088` e usá-lo para dados e texto de geração (`:1089`, `:1090`), chama `gerar_pdf_auditavel` sem esse instante. O pipeline relê o relógio em `app/relatorios/pdf_base.py:133` e usa o segundo valor no rodapé (`:139`).

Adicional, se o escopo incluir o chamador intocado da função tocada: `app/colaboradores/services/ficha.py:275` chama `ficha_do_colab`, que captura `agora` em `:117`; depois `ficha_para_o_copiloto` relê `localdate()` em `:280` quando `hoje` não é informado.

Não incluiria como achado de leitor a releitura de `app/ponto/management/commands/reconciliar_grade.py:458`: é o carimbo de uma escrita, chamado em `:228`, e não o instante empregado para julgar a grade.

## 29/10 17:51 · sessao 054f4626c827 · justif · verificador · origem=Code

**Não aprovaria o v6.** Há guardas alcançadas depois do fechamento e um ciclo de travas ainda possível. (`app/ponto/services/justificativa.py:176`, `app/ponto/services/cartorio.py:1163`, `app/ponto/services/veredito_celula.py:307`)

Os contraexemplos abaixo são inferências da árvore. Ocorrência em produção, reprodução concorrente, testes funcionais e números citados nos comentários: **[nao verificado]**. [nao verificado]

**1. Defeitos encontrados**

- **F1 — A guarda das irmãs pode chegar depois do fechamento. Classe: pouso antes do pré-requisito.** A aprovação salva J antes do helper; esse `save()` dispara o cartório, que pode chamar `reconciliar()` diretamente, sem passar pela guarda S128. Depois, o helper encontra C encerrado e retorna. (`app/ponto/services/justificativa.py:167`, `app/ponto/services/justificativa.py:176`, `app/ponto/signals.py:165`, `app/ponto/services/cartorio.py:1163`, `app/ponto/services/justificativa.py:38`)

  **Entrada → erro:** escala E07/S19, E07 real, C vivo cobrando S19, J própria pendente, pergunta S19 respondida e ainda não consumida; célula de trabalho com `julgada_em=None` antes da aprovação. J aprovada faz a grade marcar o dia justificado e a ata dispensar S19; `marcos_acesos` permite o fechamento direto. Resultado: C terminal com resposta aguardando consumo. A célula não é pulada porque ainda não foi julgada. (`app/escala/utils.py:1198`, `app/ponto/services/cartorio.py:608`, `app/ponto/services/lastro.py:170`, `app/ponto/services/cartorio.py:1092`, `app/chamados/services/disputa_emissao.py:1558`)

- **F2 — A guarda contra decidir J2 também chega tarde. Classe: pouso antes do pré-requisito.** Na aprovação individual e na materialização, registrar a saída antecede o helper. O signal da batida chama o reconciliador, cuja unidade auto-rejeita a justificativa relacionada pendente. (`app/ponto/services/justificativa.py:147`, `app/ponto/services/justificativa.py:328`, `app/ponto/signals.py:214`, `app/chamados/reconciliador.py:1214`, `app/chamados/reconciliador.py:468`)

  **Entrada → erro:** C `turno_aberto` de D aponta J2 pendente de D+1; J1 pendente de D aponta C e informa E/S que fecham o turno; sem célula e sem irmãs. A batida fecha C pelo fallback dos turnos e auto-rejeita J2. Depois J1 é aprovada, e o helper retorna pelo estado terminal antes de consultar a guarda nova. Resultado: aprovar J1 decidiu J2. (`app/chamados/reconciliador.py:264`, `app/chamados/models.py:978`, `app/chamados/reconciliador.py:462`, `app/ponto/services/justificativa.py:38`, `app/ponto/services/justificativa.py:44`)

  A cascata também contorna essa guarda: se C é alvo da cascata e aponta outra pendente, `encerrar()` força seu fechamento antes do helper. Nesse ramo J2 pode continuar pendente sob C fechado. (`app/ponto/services/justificativa.py:369`, `app/ponto/services/justificativa.py:377`, `app/chamados/models.py:1192`)

- **F3 — Permanece o ciclo chamado ↔ `TurnoMaterializado`. Classe: ordem de trava.** A aprovação segura C e J, depois registra batida e tenta reconstruir TM. `veredito_celula` pode segurar TM na sua transação e depois tentar fechar C. Escalonamento: B segura TM; A segura C; A espera TM; B espera C. (`app/ponto/services/justificativa.py:89`, `app/ponto/services/justificativa.py:101`, `app/ponto/services/justificativa.py:147`, `app/ponto/signals.py:36`, `app/ponto/nucleo.py:59`, `app/ponto/services/veredito_celula.py:203`, `app/ponto/services/veredito_celula.py:307`, `app/ponto/services/veredito_celula.py:324`)

- **F4 — A pré-trava não garante o conjunto após nascimento ou vinculação concorrente. Classe: ordem de trava.** Ela lê o ponteiro sem travar J e depois trava os chamados encontrados naquela consulta. A API pode criar C e preencher a origem anteriormente nula entre essa consulta e a trava de J. Assim, a promessa de cobrir nascimento concorrente não é garantida. (`app/ponto/services/justificativa.py:74`, `app/ponto/services/justificativa.py:80`, `app/ponto/services/justificativa.py:89`, `app/api/views.py:2033`, `app/api/views.py:2055`)

  **Interleaving → erro:** A termina a pré-trava; B cria C, liga J nos dois sentidos e conclui; A trava J; R começa a retratar C, lê J ainda pendente e espera sua escrita; A espera C no helper. Existe entrada pública de retratação por módulo, além da unidade que escreve C antes de J. Esse ciclo específico não foi reproduzido em banco **[nao verificado]**. (`app/api/views.py:2069`, `app/ponto/services/justificativa.py:101`, `app/chamados/reconciliador.py:455`, `app/chamados/reconciliador.py:468`, `app/ponto/services/justificativa.py:37`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:24`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`)

- **F5 — O lote ainda aprova quando a batida pertence a outra justificativa. Classe: outra, inconsistência do ato. Remanescente no trecho tocado.** `_outra is not None` apenas evita vincular a batida; o lote continua, fecha a origem e conta aprovação. (`app/ponto/views.py:1825`, `app/ponto/views.py:1833`, `app/ponto/views.py:1834`)

  **Entrada → erro:** J1 pendente com saída informada; a porta deduplica para S já vinculada a J2 aprovada. Resultado: J1 aprovada, origem encerrada, `batida_gerada` de J1 nula e resposta de sucesso. A porta individual recusa precisamente essa situação. (`app/ponto/registro_batida.py:113`, `app/ponto/views.py:1826`, `app/ponto/services/justificativa.py:157`)

- **F6 — A materialização pode produzir duas transições de estado. Classe: outra, violação da trilha única. Remanescente anterior ao v6.** A saída pode resolver C pelo signal; a cascata o seleciona novamente porque `NAO_FECHADOS` inclui `resolvido`, e força `resolvido → fechado`. Cada passagem grava `estado_chamado`. (`app/ponto/signals.py:214`, `app/chamados/catalogo/motor.py:32`, `app/ponto/services/justificativa.py:61`, `app/ponto/services/justificativa.py:369`, `app/chamados/catalogo/motor.py:115`, `app/chamados/models.py:227`)

  **Entrada → erro:** origem `turno_aberto`, J própria, E existente, S materializada, sem irmãs. Resultado: duas linhas de estado no primeiro ato; o helper posterior ser idempotente não elimina essas duas linhas. (`app/ponto/services/justificativa.py:328`, `app/chamados/reconciliador.py:456`, `app/ponto/services/justificativa.py:377`)

- **F7 — O fecho legado continua aplicando uma regra própria a outro chamado. Classe: outra, leitor com regra própria de fechar. Remanescente no sítio tocado.** O `exclude` protege a origem, mas a busca continua sem filtro de dia e fecha outro `entrada_bloqueada_turno_aberto`, fora da transação da aprovação e sem guarda das irmãs. (`app/ponto/services/justificativa.py:203`, `app/ponto/services/justificativa.py:207`, `app/ponto/services/justificativa.py:209`)

  **Entrada → erro:** aprovar J de D1 enquanto o chamado mais recente desse módulo pertence a D2, ainda irregular ou com irmã respondida. Resultado: C de D2 é forçado a fechado pelo ato sobre D1. (`app/ponto/services/justificativa.py:204`, `app/ponto/services/justificativa.py:209`, `app/chamados/models.py:1193`)

**2. Escritores não alcançados**

Sim: **FORA-DO-ESCOPO**, o Django admin continua editável para Justificativa, incluindo `status='aprovada'`, sem costura explícita de fechamento. O próprio selo declara essa lacuna. (`app/ponto/admin.py:18`, `app/ponto/models.py:159`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:13`)

As três atribuições explícitas encontradas estão alcançadas: individual, materialização e lote. (`app/ponto/services/justificativa.py:164`, `app/ponto/services/justificativa.py:350`, `app/ponto/views.py:1795`)

Não encontrei escritor explícito de `status_local` fora das portas nas formas cobertas pelo censo AST; isso não prova ausência de escrita dinâmica ou genérica. As portas autorizadas e o universo da varredura estão no selo. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:17`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:37`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:322`)

**3. Mordida dos testes e tetos** [nao verificado]

- Os positivos de individual, lote e materialização verificam aprovação e fechamento; o de falha do fechamento exige que a porta tenha sido chamada antes de conferir rollback. Portanto há cobertura da costura nesses cenários. (`app/ponto/tests/test_justificativa_encerra_origem.py:73`, `app/ponto/tests/test_justificativa_encerra_origem.py:85`, `app/ponto/tests/test_justificativa_encerra_origem.py:171`, `app/ponto/tests/test_justificativa_encerra_origem.py:226`)

- **Ausência de sinal:** `test_aprovar_J1_nao_rejeita_a_J2_pendente_que_o_chamado_aponta` passaria sem chamar o fechamento: J1 aprovada, J2 pendente e C aberto são também o resultado de nunca tentar fechar. Ele cobre a guarda mantendo a costura, mas não demonstra sua execução nesse cenário. **Classe: selo/RED sem mordida da costura.** (`app/ponto/tests/test_justificativa_encerra_origem.py:272`, `app/ponto/tests/test_justificativa_encerra_origem.py:289`)

- O teste da irmã na aprovação exige a trilha de retenção, portanto não passa apenas porque ninguém tentou fechar. Porém, os novos casos de irmãs não criam célula e deixam de exercitar F1. O teste da cascata também passaria retirando todo o fechamento; retirando somente sua guarda, deve falhar. (`app/ponto/tests/test_justificativa_encerra_origem.py:352`, `app/ponto/tests/test_justificativa_encerra_origem.py:299`, `app/ponto/tests/test_justificativa_encerra_origem.py:340`, `app/ponto/services/cartorio.py:1548`, `app/ponto/tests/test_justificativa_encerra_origem.py:315`)

- O RED direto do S143 verifica chamada, retorno verdadeiro, estado persistido e uma trilha: está desenhado para detectar o rollback silencioso sem seu savepoint. Já o teste de erro repetido continuaria passando sem esse savepoint, pois exige rollback integral. (`app/ponto/tests/test_justificativa_encerra_origem.py:250`, `app/ponto/tests/test_justificativa_encerra_origem.py:254`, `app/ponto/tests/test_justificativa_encerra_origem.py:256`, `app/ponto/tests/test_justificativa_encerra_origem.py:270`)

- Os testes de erro de banco por `tipo='x'*40` dependem de o backend rejeitar esse valor. O teste do UPDATE injeta uma data inválida; não demonstra uma falha emitida pelo servidor nem verifica explicitamente o estado de rollback. A mordida real desses testes no backend instalado ficou **[nao verificado]**. (`app/ponto/tests/test_justificativa_encerra_origem.py:242`, `app/ponto/tests/test_justificativa_encerra_origem.py:362`, `app/ponto/tests/test_justificativa_encerra_origem.py:380`, `app/ponto/tests/test_justificativa_encerra_origem.py:382`)

- **Tetos:** o custo acrescentado é por execução do receiver em `post_save` terminal, inclusive saves que não mudam estado; “por chamado que fecha” é impreciso. A aritmética declarada é compatível com dois comandos por execução, mas as contagens medidas permanecem **[nao verificado]**. Como os testes cobram `<= teto`, retirar o savepoint não os reprova; essa responsabilidade é do RED funcional. (`app/chamados/signals.py:81`, `app/chamados/signals.py:91`, `app/ponto/tests/test_selo_chokepoint_escrita.py:150`, `app/ponto/tests/test_selo_chokepoint_escrita.py:210`)

- **F7 anterior corrigido:** o selo compara o receptor da atribuição, do `save()` e do argumento do fechamento; existe teste explícito com outro objeto. A limitação de não provar execução do ramo continua declarada. (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:102`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:103`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:207`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:9`)

**4. Ordem das travas**

No conjunto já existente, individual, rejeição individual, materialização e lote agora pretravam chamados antes de J. Isso corrige a inversão direta apontada na rejeição individual. (`app/ponto/services/justificativa.py:100`, `app/ponto/services/justificativa.py:244`, `app/ponto/services/justificativa.py:310`, `app/ponto/views.py:1792`)

O reconciliador percorre os vivos por PK, mas não os trava antecipadamente: a escrita de C ocorre em `reconciliar()`, seguida da possível escrita de J pendente. Portanto a ordem C→J continua existindo do outro lado; F3 e F4 impedem concluir que todos os ciclos foram eliminados. (`app/chamados/reconciliador.py:1203`, `app/chamados/reconciliador.py:456`, `app/chamados/reconciliador.py:468`)

**5. Efeitos dentro da transação**

**Sim, podem desfazer a aprovação.** S143 agora captura a falha após sair de seu savepoint, incluindo a obtenção do usuário sistema. Entretanto, S128 chama o resolver novamente sem captura; uma falha persistente, ou falha da nota subsequente, atravessa a transação da aprovação. Nesse caminho, desfazer tudo atende à exigência de atomicidade. (`app/chamados/signals.py:91`, `app/chamados/signals.py:93`, `app/chamados/reconciliador.py:469`, `app/chamados/reconciliador.py:471`, `app/ponto/services/justificativa.py:99`, `app/ponto/services/justificativa.py:177`)

O rebaixamento também não captura exceções. Normalmente o núcleo já normalizou a urgência antes do signal, tornando o segundo rebaixamento um no-op. (`app/chamados/signals.py:72`, `app/chamados/models.py:210`, `app/chamados/models.py:424`)

O cartório captura exceções com `engolir`, que não cria savepoint por si. Logo não há garantia geral demonstrada de isolamento de todo erro de banco desse receiver; um sucesso falso por esse caminho específico ficou **[nao verificado]**. (`app/chamados/signals.py:172`, `app/core/observ.py:85`)

Ler a própria J aprovada ainda não commitada é necessário para o gate. O erro concreto encontrado é a **ordem dos efeitos antes das guardas**, descrita em F1/F2. Os novos savepoints de `invalidar_fechamento` envolvem corretamente a coleta e cada UPDATE, com captura externa; não identifiquei ali um novo meio-estado da aprovação. (`app/chamados/models.py:937`, `app/ponto/services/fechamento.py:1616`, `app/ponto/services/fechamento.py:1619`, `app/ponto/services/fechamento.py:1629`)

## 29/10 17:51 · sessao 643d6293abcc · - · - · origem=Ronald (tmux)

- **Classe 5 — selo não detecta a volta da segunda leitura de relógio.** Em `app/escala/tests/test_selo_grade_pede_o_instante.py:73`, o verificador ignora funções sem parâmetro `agora`; em `:80`, examina somente chamadas aos `ALVOS`. Restaurar em `app/api/views.py:1635` o trecho anterior `timezone.now() < _fim_hj`, após a leitura de `:1350`, produz `[]` nos dois verificadores do selo (`test_selo_grade_pede_o_instante.py:102` e `:108`). Confirmei essa mutação em memória, executando os auxiliares AST, sem gravar arquivos. A assinatura permanece igual, portanto as verificações de assinatura em `:139` e `:145` também não detectam a regressão.

## 29/10 17:50 · sessao 5ecb2ed6553f · - · - · origem=Ronald (tmux)

Nenhum achado verificável de classe 7 no censo dos chamadores das assinaturas alteradas.

O aparente chamador sem `agora=` em `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:53` recebe o argumento via `**kw`, preenchido em `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:52`.

Os chamadores de `Command.apurar` passam `agora` em `app/ponto/management/commands/pautas_discordantes.py:112` e `app/ponto/tests/test_o320_pauta_dos_discordantes.py:189`.

## 29/10 17:48 · sessao edaf1101b86e · - · - · origem=Ronald (tmux)

Refinamento confirmado: a célula do contraexemplo deve estar elegível para julgamento, por exemplo `julgada_em=None` imediatamente antes do POST; assim o pulo por impressão igual não ocorre (`app/ponto/services/cartorio.py:1092`).

Entrada concreta: escala E07/S19, E07 real, C vivo cobrando S19, J própria pendente e pergunta S19 respondida sem validação/materialização; célula de trabalho criada após os saves preparatórios. Ao aprovar, o signal de J julga (`app/ponto/signals.py:161`–`:165`), a grade marca justificado (`app/escala/utils.py:1198`–`:1203`), a ata acende S19 por dispensa (`app/ponto/services/cartorio.py:602`–`:610`) e o cartório chama `reconciliar()` diretamente (`:1163`). O helper encontra C terminal e não consulta S128 (`app/ponto/services/justificativa.py:37`–`:39`). A proteção das irmãs fica contornada.

O savepoint S143 inclui corretamente `get_user_sistema()` (`app/chamados/signals.py:91`–`:92`), com captura após a saída do atomic (`:93`–`:94`).

Existe risco estrutural residual de banco engolido no cartório: o receiver usa `engolir` sem savepoint (`app/chamados/signals.py:171`–`:180`; `app/core/observ.py:85`–`:91`), e pode alcançar `ressuscitar_perguntas` (`app/ponto/services/cartorio.py:1271`), cujo `update` não tem atomic próprio (`app/chamados/models.py:1162`–`:1163`). Uma reprodução de aprovação retornando sucesso com C vivo nesse caminho específico ficou [nao verificado]; não acrescentaria como defeito demonstrado do diff.

Não identifiquei segundo juiz autônomo na guarda J2: ela impede decidir outra justificativa (`app/ponto/services/justificativa.py:40`–`:45`), enquanto S128 usa “pendente” para executar auto-rejeição (`app/chamados/reconciliador.py:462`–`:468`). Não há no dado contrato exigindo trilha para esse veto [nao verificado].

## 29/10 17:47 · sessao 1e7f75cfae7d · grupo1p1 · verificador · origem=Code

- **Classe 3 — espelho perde o instante na fonte dos totais.** `app/ponto/services/espelho.py:1288` chama `folha_manda` sem repassar `agora`. Ela consulta turno vivo e véspera com novos relógios em `app/relatorios/cartao_pela_celula.py:398` e `app/relatorios/cartao_pela_celula.py:412`, acionando `app/ponto/turnos.py:2255` e `app/relatorios/cartao_pela_celula.py:40`.

- **Classe 3 — fila e simulação usam relógios diferentes.** `app/ponto/services/flip_auto.py:263` captura `agora`, mas a fila chamada em `app/ponto/services/flip_auto.py:268` lê novamente em `app/ponto/services/flip_auto.py:34` e `app/ponto/services/flip_auto.py:126`.

- **Classe 3 — ficha ainda lê novamente para classificar pendências.** Após capturar o instante em `app/colaboradores/services/ficha.py:117`, chama `particionar_verbos` em `app/colaboradores/services/ficha.py:155`; essa função lê outro instante em `app/chamados/services/verbos.py:32`, usado para silêncio e idade em `app/chamados/services/verbos.py:48` e `app/chamados/services/verbos.py:54`.

- **Classe 3 — PDF auditável mantém segundo relógio.** O instante capturado em `app/relatorios/pdf_espelho.py:1088` não acompanha a chamada em `app/relatorios/pdf_espelho.py:1099`. O pipeline captura outro em `app/relatorios/pdf_base.py:133`, usado no rodapé em `app/relatorios/pdf_base.py:139`.

- **Classe 3 — reconciliação relê no ramo de aplicação.** O comando captura o instante em `app/ponto/management/commands/reconciliar_grade.py:75`, mas chama `_relavrar_orfa` em `app/ponto/management/commands/reconciliar_grade.py:228`, que lê novamente para `relavrada_em` em `app/ponto/management/commands/reconciliar_grade.py:458`.

- **Classe 5 — selo aceita restaurar leitura interna.** O detector em `app/escala/tests/test_selo_grade_pede_o_instante.py:57` apenas exige `agora=` diferente de literal `None`. Trocar a propagação em `app/escala/utils.py:2277` por `agora=timezone.now()` mantém essa verificação aprovada; a asserção em `app/escala/tests/test_selo_grade_pede_o_instante.py:78` não distingue os dois casos.

- **Classe 8 — migração muda os cenários temporais dos testes.** `app/escala/tests/test_f1c_contrato.py:36` injeta 2100 numa janela construída em torno do hoje real em `app/escala/tests/test_f1c_contrato.py:34`; o contrato usa 2100 como hoje em `app/escala/utils.py:2272`, retirando o cenário futuro que a asserção pretende verificar. Também `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:206` injeta 2100 no cenário congelado em julho/2026 de `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:328`: o teto do coletor em `app/relatorios/pdf_espelho.py:287` passa a admitir a competência inteira, embora `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:330` exija excluir o dia corrente.

Total por classe: 1=0; 2=0; 3=5; 4=0; 5=1; 6=0; 7=0; 8=1.

## 29/10 17:47 · sessao b48a37d3b9a3 · - · - · origem=Ronald (tmux)

Achados estáticos para consolidar; execução concorrente em banco `[nao verificado]`.

1. **F3 permanece — ordem de trava C → TurnoMaterializado contra TurnoMaterializado → C.** A aprovação trava chamados em `app/ponto/services/justificativa.py:89`, trava J em `:101`, e registra saída em `:147`; o signal recomputa turnos em `app/ponto/signals.py:34`, com DELETE/INSERT de `TurnoMaterializado` em `app/ponto/nucleo.py:57`. O concorrente `veredito_celula` mantém atomic em `app/ponto/services/veredito_celula.py:203`, registra batidas em `:300`, recomputa em `:307`, e depois fecha chamados em `:324`, via `:182`. Escalonamento: B segura TM; A segura C; A espera TM; B espera C. A pretrava nova não abrange TM (`app/ponto/services/justificativa.py:84`).

2. **F5 permanece no ramo que registra saída — guarda J2 chega depois do efeito.** Entrada concreta: C `turno_aberto` do dia D, sem CelulaDia, aponta J2 pendente de D+1; J1 de D aponta C e informa entrada/saída. A saída registrada em `app/ponto/services/justificativa.py:147` recomputa TM e dispara reconciliador (`app/ponto/signals.py:214`); sem célula, cartório retorna em `app/ponto/services/cartorio.py:1547`; o reconciliador verifica turno fechado em `app/chamados/reconciliador.py:264`, gate aceita pelo fallback dos turnos em `app/chamados/models.py:978`, e `_retratar` rejeita J2 em `app/chamados/reconciliador.py:462`. A guarda nova só aparece depois, em `app/ponto/services/justificativa.py:44`, mas C já encerrado retorna antes em `:38`. Resultado: aprovar J1 rejeita J2. O teste novo usa wrapper sem batida e não cobre esse ramo (`app/ponto/tests/test_justificativa_encerra_origem.py:277`, `:284`).

3. **A pretrava não elimina novos chamados ou novo ponteiro depois da consulta.** O ponteiro é lido sem trava em `app/ponto/services/justificativa.py:80`; o conjunto é travado em `:89`. Produção pode preencher origem nula depois: `app/ponto/views.py:1631` ou `app/api/views.py:2055`; a API pode criar wrapper em `app/api/views.py:2032` e apontar reverso em `:2067`. Escalonamento possível: A conclui pretrava; B cria C e liga J; A trava J; R retrata C e espera J (`app/chamados/reconciliador.py:455`, `:468`); A espera C (`app/ponto/services/justificativa.py:37`). O comentário que promete cobrir nascimento concorrente não corresponde ao conjunto fixado pela consulta (`app/ponto/services/justificativa.py:74`). Reprodução `[nao verificado]`.

4. **Possível bypass adicional das irmãs, para confirmação no consolidado.** O `save()` de J precede helper (`app/ponto/services/justificativa.py:167`, `:353`), dispara cartório (`app/ponto/signals.py:161`) e esse chama `reconciliar()` diretamente em `app/ponto/services/cartorio.py:1163`, `:1175`, `:1196`, sem unidade S128. Os testes novos de irmãs não criam CelulaDia (`app/ponto/tests/test_justificativa_encerra_origem.py:299`, `:340`); portanto não medem esse fechamento anterior. Contraexemplo completo com os protestos calculados `[nao verificado]`.

`rejeitar_lote_justificativas` não usa atomic próprio (`app/ponto/views.py:1868`), então não afirmo inversão persistente J → C ali em autocommit. `JustificativaAdmin` continua ModelAdmin editável (`app/ponto/admin.py:18`); atomic interno do framework não está na cópia consultada `[nao verificado]`.

## 29/10 17:47 · sessao 1842696154ae · - · - · origem=Ronald (tmux)

- **Escritor não alcançado: Django admin, FORA-DO-ESCOPO.** `JustificativaAdmin` é um `admin.ModelAdmin` genérico, sem bloqueio/override do save; `status` é editável e aceita `aprovada`. Entrada concreta: admin altera J pendente para aprovada → save genérico sem `encerrar_chamado_de_origem`. O selo admite expressamente essa lacuna. (`app/ponto/admin.py:18`, `app/ponto/models.py:130`, `app/ponto/models.py:159`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:13`)

- **Censo de atribuições de aprovação:** os escritores de Justificativa encontrados são um a um, materialização e lote; os três chamam o fechamento dentro do atomic. A atribuição de férias é outro modelo. (`app/ponto/services/justificativa.py:164`, `app/ponto/services/justificativa.py:176`, `app/ponto/services/justificativa.py:350`, `app/ponto/services/justificativa.py:377`, `app/ponto/views.py:1795`, `app/ponto/views.py:1838`, `app/ferias/services.py:419`)

- **`status_local`:** executei em memória o censo AST do selo, que percorreu 739 arquivos e encontrou zero violações; isso cobre as formas declaradas pelo próprio selo, com os limites de tipagem/dinamismo registrados nele. As portas permitidas estão na allowlist. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:10`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:37`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:322`)

- **Ausência de sinal no teste de J1/J2:** `test_aprovar_J1_nao_rejeita_a_J2_pendente_que_o_chamado_aponta` passaria retirando a chamada do fechamento da aprovação: ele só exige J1 aprovada, J2 pendente e chamado aberto, precisamente o resultado de nunca tentar fechar. Como teste específico da guarda, morde removendo a guarda e mantendo a chamada; como teste da costura, não morde. Classe: **selo/RED sem mordida**, limitada a esse cenário. (`app/ponto/tests/test_justificativa_encerra_origem.py:272`, `app/ponto/tests/test_justificativa_encerra_origem.py:286`, `app/ponto/tests/test_justificativa_encerra_origem.py:289`)

- **Irmãs:** o teste da aprovação exige a trilha `retratacao_retida_irma_orfa`, portanto não passa simplesmente por ausência do fechamento. O teste da cascata exige aprovação efetiva e chamado vivo; removendo só a guarda da cascata, o `encerrar()` passa a fechar e contraria a asserção. Ele ainda passaria removendo todo o fechamento da cascata e da origem. (`app/ponto/tests/test_justificativa_encerra_origem.py:313`, `app/ponto/tests/test_justificativa_encerra_origem.py:315`, `app/ponto/tests/test_justificativa_encerra_origem.py:349`, `app/ponto/tests/test_justificativa_encerra_origem.py:352`, `app/ponto/services/justificativa.py:367`, `app/ponto/services/justificativa.py:369`)

- **Erro de banco S143:** o teste direto exige que o mock seja chamado, retorno verdadeiro, chamado encerrado e uma trilha; está desenhado para distinguir o rollback silencioso. O teste da falha repetida exige chamada e rollback integral, mas, isoladamente, também passaria sem o savepoint S143, pois esse resultado continuaria sendo rollback integral. (`app/ponto/tests/test_justificativa_encerra_origem.py:241`, `app/ponto/tests/test_justificativa_encerra_origem.py:250`, `app/ponto/tests/test_justificativa_encerra_origem.py:254`, `app/ponto/tests/test_justificativa_encerra_origem.py:261`, `app/ponto/tests/test_justificativa_encerra_origem.py:267`)

- **Teste UPDATE do carimbo:** injeta uma data inválida, não uma falha emitida pelo servidor; só exige que `timezone.now` seja chamado e o ato sobreviva. Ausência de `FechamentoMensal` não basta para declarar vacuidade, pois o UPDATE precisa preparar os valores mesmo sem linhas correspondentes. Se a preparação ocorre dentro de `mark_for_rollback_on_error` na versão instalada, o teste morde sem savepoint; essa implementação Django e a execução com banco ficaram **[nao verificado]**. (`app/ponto/tests/test_justificativa_encerra_origem.py:379`, `app/ponto/tests/test_justificativa_encerra_origem.py:382`, `app/ponto/tests/test_justificativa_encerra_origem.py:385`)

- **Tetos:** o custo acrescentado é por execução do receiver em `post_save` de chamado terminal, pois ele não verifica transição nem `update_fields`; portanto “por chamado que fecha” é uma descrição incompleta. O próprio comentário prevê dois disparos no cenário de um chamado. A alteração é compatível com SAVEPOINT+RELEASE por execução; os números medidos permanecem **[nao verificado]** sem banco. Os tetos usam `<=`, portanto remover o savepoint não os reprovaria; a garantia funcional depende do RED separado. (`app/chamados/signals.py:75`, `app/chamados/signals.py:81`, `app/chamados/signals.py:91`, `app/ponto/tests/test_selo_chokepoint_escrita.py:148`, `app/ponto/tests/test_selo_chokepoint_escrita.py:150`, `app/ponto/tests/test_selo_chokepoint_escrita.py:210`)

- **F7 corrigido:** o selo agora compara o receptor da atribuição, do save e do primeiro argumento do fechamento; há mutação explícita para outro objeto. Executei os dois selos em memória, substituindo somente `SimpleTestCase` por `unittest.TestCase`: 22 testes passaram, sem Django nem banco; execução Django funcional **[nao verificado]**. (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:81`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:102`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:103`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:207`)

## 29/10 17:46 · sessao edaf1101b86e · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

1. **Guarda das irmãs pode chegar tarde.** `j.save()` ocorre antes de `encerrar_chamado_de_origem` (`app/ponto/services/justificativa.py:167`, `:176`; lote `app/ponto/views.py:1798`, `:1833`). O signal da justificativa julga a célula imediatamente (`app/ponto/signals.py:161`–`:165`, `:88`–`:93`). A grade reconhece a justificativa aprovada e marca o dia justificado (`app/escala/utils.py:1157`–`:1159`, `:1198`–`:1203`); o cartório dispensa as lâmpadas (`app/ponto/services/cartorio.py:602`–`:610`) e fecha chamados diretamente por `reconciliar`, sem a guarda S128 (`:1163`, `:1175`, `:1196`).

   Contraexemplo: célula de trabalho com saída S19 apagada; chamado vivo com `marcos_faltantes=[{'hora':'19:00','tipo':'S'}]`; justificativa própria pendente; pergunta respondida “19:00”, não validada/materializada. Aprovar a justificativa acende a lâmpada por dispensa; `marcos_acesos` retorna verdadeiro (`app/ponto/services/lastro.py:170`–`:181`), o gate aceita a justificativa própria aprovada (`app/chamados/models.py:937`–`:938`) e fecha antes da guarda. O helper encontra chamado terminal e retorna (`app/ponto/services/justificativa.py:37`–`:39`). O resolver preserva a resposta ainda não consumida (`app/chamados/services/disputa_emissao.py:1558`–`:1560`). **Classe: pouso antes do pré-requisito / guarda contornada.**

2. **Guarda J2 pendente também pode chegar tarde.** As portas unitária e materialização registram a saída antes de chamar o helper (`app/ponto/services/justificativa.py:147`–`:153`, `:328`–`:335`, `:176`, `:377`). O signal da batida chama o reconciliador (`app/ponto/signals.py:206`–`:215`); ele encontra a premissa de turno morta e chama `_retratar` (`app/chamados/reconciliador.py:264`–`:289`, `:1203`–`:1214`), que auto-rejeita a justificativa relacionada pendente (`:462`–`:468`).

   Contraexemplo: J1 aprovada com saída que fecha o turno; origem C aponta J2 pendente; sem irmãs. A batida resolve C e rejeita J2 antes da guarda `app/ponto/services/justificativa.py:44`. Depois o helper retorna porque C já está encerrado (`:38`). **Classe: pouso antes do pré-requisito / outra: decisão colateral de J2.**

3. **A cascata antecede a guarda J2.** Ela chama `encerrar()` em cada alvo (`app/ponto/services/justificativa.py:361`–`:369`), antes do helper (`:377`). `encerrar` força fechamento sem gate da premissa (`app/chamados/models.py:1192`–`:1202`). Se a origem pertence à cascata e aponta outra justificativa pendente, termina fechada antes da guarda de `:44`; a cascata não auto-rejeita J2, mas contorna o resultado prometido pela guarda. **Classe: pouso antes do pré-requisito.**

4. **Residual de duas trilhas na materialização:** `NAO_FECHADOS` contém `resolvido` (`app/chamados/catalogo/motor.py:32`). A saída pode fazer o signal resolver C antes da cascata; `_cascata_do_turno` volta a selecionar esse C (`app/ponto/services/justificativa.py:61`, `:361`) e `encerrar()` faz `resolvido → fechado` (`app/chamados/catalogo/motor.py:114`–`:115`). São duas transições e duas linhas `estado_chamado` (`app/chamados/models.py:227`–`:231`). Esse mecanismo da cascata já existia no trecho fornecido; apontar como residual preexistente, não atribuir sua introdução ao v6.  

Sobre os efeitos:

- Rebaixamento não captura exceções (`app/chamados/signals.py:68`–`:72`). No caminho normal `_gravar_estado` já normaliza a urgência antes do save (`app/chamados/models.py:210`), de modo que `_rebaixar_sem_post_save` retorna sem update (`:424`–`:426`). Uma exceção propagada nessa execução atravessa o atomic da aprovação (`app/ponto/services/justificativa.py:99`–`:181`).
- S143 agora tem savepoint próprio e captura fora dele (`app/chamados/signals.py:88`–`:94`), portanto o erro direto do resolver fica isolado. S128 chama o resolver novamente sem captura (`app/chamados/reconciliador.py:455`–`:473`); erro repetido desfaz a aprovação inteira.
- O cartório usa `engolir` (`app/chamados/signals.py:171`–`:180`), cujo corpo somente captura, sem savepoint próprio (`app/core/observ.py:85`–`:91`). Suas portas de persistência da célula têm atomic próprio (`app/ponto/portas/celula.py:634`–`:642`). Uma garantia geral de isolamento de todo erro de banco no cartório não foi demonstrada [nao verificado].
- O risco concreto demonstrável pela leitura é **ordem de efeitos síncronos** antes das guardas, acima; ler as próprias alterações ainda não commitadas é necessário para que o gate veja a justificativa aprovada (`app/chamados/models.py:937`–`:938`), não constitui sozinho erro.
- `invalidar_fechamento` protege a coleta da competência e cada update com atomic separado (`app/ponto/services/fechamento.py:1616`–`:1624`), capturando fora (`:1629`–`:1633`). Se falhar na segunda competência, a primeira permanece invalidada e a função devolve zero; é o comportamento documentado no novo comentário (`:1614`–`:1615`), não evidência de aprovação parcialmente gravada.

Não executei banco nem testes; ocorrências reais desses contraexemplos em produção estão [nao verificado]. [nao verificado]

## 29/10 17:46 · sessao c8fc8330748e · - · - · origem=Ronald (tmux)

Achados de classe 3:

- `app/ponto/services/flip_auto.py:263`: `avaliar_fila` captura `agora`, mas `fila_flip` chamado em `:268` provoca outras duas leituras: `pares_mesmo_tipo` em `:34` e `fila_flip_recusas` em `:126`, pelas chamadas em `:146` e `:148`. O universo da fila e as simulações em `:303-304` usam instantes diferentes.
- `app/ponto/services/espelho.py:1288`: não repassa o instante obrigatório a `folha_manda`. Essa fonte chama `turno_aberto_de` sem instante em `app/relatorios/cartao_pela_celula.py:398` e `ate_ontem` sem data em `:412`, acionando relógios em `app/ponto/turnos.py:2255` e `app/relatorios/cartao_pela_celula.py:40`.
- `app/relatorios/pdf_espelho.py:1088`: captura o instante do PDF, mas chama `gerar_pdf_auditavel` em `:1099` sem repassá-lo; o pipeline lê novamente em `app/relatorios/pdf_base.py:133` e usa esse segundo instante nos rodapés em `:139` e `:170`.
- `app/colaboradores/services/ficha.py:117`: captura o instante da ficha, mas a chamada `particionar_verbos(vivos)` em `:155` faz outra leitura em `app/chamados/services/verbos.py:32`, usada para classificar silêncio e idade em `:48` e `:54`.
- `app/colaboradores/services/ficha.py:275`: `ficha_para_o_copiloto` chama a ficha, cuja leitura ocorre em `:117`, e depois lê novamente por `timezone.localdate()` em `:280`.
- `app/ponto/management/commands/reconciliar_grade.py:75`: captura instante do comando, mas `_relavrar_orfa`, chamado em `:228`, lê novamente em `:458` no ramo `apply`. Esse caso afeta o carimbo `relavrada_em`.

Possível classe 5, comprovada em memória:

- `app/escala/tests/test_selo_grade_pede_o_instante.py:58`: o detector só exige a presença de `agora=` e rejeita literal `None` (`:59-60`). `chamadas_sem_instante('grade_espelho_janela(c, bs, i, f, agora=timezone.now())')` retorna `[]`; portanto trocar uma propagação por nova leitura do relógio mantém esse selo aprovado. A execução foi apenas da função AST extraída, com `python3 -B`, sem Django ou escrita.

## 29/10 17:46 · sessao c22e52762040 · - · - · origem=Ronald (tmux)

- **Classe 8 — migração do teste remove seu cenário futuro.** `app/escala/tests/test_f1c_contrato.py:36` passa `DEPOIS_DE_TUDO`, definido como 01/01/2100 em `app/escala/tests/instante.py:12`, embora o dia examinado seja o hoje real + cinco dias (`app/escala/tests/test_f1c_contrato.py:34`). O contrato passa a considerar hoje=2100 (`app/escala/utils.py:2272`), portanto esse dia deixa de ser futuro para o clip (`app/escala/utils.py:942`), contrariando a asserção do teste (`app/escala/tests/test_f1c_contrato.py:39`). Na data fornecida, 03/11/2026 é terça, dia de trabalho da fixture (`app/escala/tests/test_f1c_contrato.py:27`); a montagem mantém as células (`app/escala/utils.py:1749`).

- **Classe 8 — migração do selo corrente muda o período perguntado.** `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:206` passa 2100 ao cartão, inclusive no cenário congelado em 23/07/2026 (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:328`), que exige excluir o dia corrente (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:330`). O coletor usa esse argumento para determinar a véspera (`app/relatorios/pdf_espelho.py:287`) e entrega 31/12/2099 como hoje ao espelho (`app/relatorios/pdf_espelho.py:594`); o espelho monta até essa data (`app/ponto/services/espelho.py:1231`, `app/ponto/services/espelho.py:1266`). O cenário deixou de medir um cartão emitido em 23/07/2026.

Não identifiquei introdução verificável das classes 1, 2, 4 ou 6 nas alterações examinadas. [não verificado]

## 29/10 17:46 · sessao 7aa4ace3aa4f · - · - · origem=Ronald (tmux)

- **Classe 5 (candidato):** `app/escala/tests/test_selo_grade_pede_o_instante.py:57-59` aceita qualquer expressão não literal `None` como instante. Uma chamada com `agora=timezone.now()` passa no selo mesmo após outra leitura de relógio no ato; reprodução em memória de `chamadas_sem_instante` retornou `[]`. O teste de produção apenas exige essa lista vazia (`:74-78`), e os demais verificam assinatura (`:100-111`). Isso permite restaurar relógio interno, por exemplo em `app/escala/utils.py:2277`, sem este selo detectar.
- **Classe 3, se o escopo inclui testes:** `app/ponto/tests/test_par_relampago_pelo_turno.py:55` combina `timezone.localdate()` e `timezone.now()` na mesma chamada. Os testes chamadores congelam o relógio (`:58`), mas o helper continua lendo duas vezes.
- **Problema de migração temporal de teste:** `app/escala/tests/test_f1c_contrato.py:34-36` monta janela em torno do relógio real e injeta o ano 2100. O contrato deriva `hoje` do argumento (`app/escala/utils.py:2272`) e o entrega ao clip (`:2278`); portanto a janela denominada futura (`test_f1c_contrato.py:38-40`) é passada para o clip como passada.

Não encontrei chamadores Python sem migração de `agora`: varredura AST de `app` e `bin` incluiu aliases e testes. A única chamada sem keyword explícita usa `**kw`, previamente preenchido com `agora` (`app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:52-53`).

## 29/10 17:43 · sessao cc944dd21722 · grupo1p1 · verificador · origem=Code

- **Classe 3** — A sonda calcula `HOJE` com `localtime()` sem argumento e depois lê outro instante para `agora`, passando ambos ao mesmo espelho. `bin/r3_turno_aberto_alcanca.py:34`, `bin/r3_turno_aberto_alcanca.py:42`, `bin/r3_turno_aberto_alcanca.py:48`.

- **Classe 3** — O comando captura o instante, mas chama `_ate_ontem` sem `hoje=`, provocando outra leitura de relógio. `app/relatorios/management/commands/diff_pdf_x_espelho.py:50`, `app/relatorios/management/commands/diff_pdf_x_espelho.py:75`; releitura em `app/relatorios/cartao_pela_celula.py:40`.

- **Classe 3** — `avaliar_fila` captura `agora`, mas `fila_flip` consulta duas fontes que leem seus próprios relógios. `app/ponto/services/flip_auto.py:263`, `app/ponto/services/flip_auto.py:268`; chamadas em `app/ponto/services/flip_auto.py:146`, `app/ponto/services/flip_auto.py:148`; releituras em `app/ponto/services/flip_auto.py:34`, `app/ponto/services/flip_auto.py:126`.

- **Classe 3** — A ficha captura `agora`, mas a partição das pendências lê outro instante para vencimento e idade. `app/colaboradores/services/ficha.py:117`, `app/colaboradores/services/ficha.py:155`; releitura e usos em `app/chamados/services/verbos.py:32`, `app/chamados/services/verbos.py:48`, `app/chamados/services/verbos.py:54`.

- **Classe 3** — O PDF formal captura o instante para coleta e corpo, mas não o repassa ao pipeline, que lê outro para o rodapé. `app/relatorios/pdf_espelho.py:1088`, `app/relatorios/pdf_espelho.py:1099`; releitura em `app/relatorios/pdf_base.py:133` e uso em `app/relatorios/pdf_base.py:139`.

- **Classe 3** — O espelho recebe `agora`, mas não o repassa a `folha_manda`; esta consulta o teto e o turno vivo com relógios próprios. `app/ponto/services/espelho.py:1198`, `app/ponto/services/espelho.py:1288`; chamadas em `app/relatorios/cartao_pela_celula.py:398`, `app/relatorios/cartao_pela_celula.py:412`; releituras em `app/ponto/turnos.py:2255`, `app/relatorios/cartao_pela_celula.py:40`.

- **Classe 7** — `Command.apurar` passou a exigir `agora`, mas o chamador de teste continua sem o argumento, causando `TypeError` antes das verificações. `app/ponto/management/commands/pautas_discordantes.py:56`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:188`.

- **Classe 5** — O selo omite `Command.apurar` de `ALVOS`. Retirar `agora=` do chamador de produção não seria detectado pelo censo nem pela inspeção das assinaturas. `app/escala/tests/test_selo_grade_pede_o_instante.py:25`, `app/escala/tests/test_selo_grade_pede_o_instante.py:51`, `app/escala/tests/test_selo_grade_pede_o_instante.py:92`; chamador em `app/ponto/management/commands/pautas_discordantes.py:112`.

Total por classe: 1=0; 2=0; 3=6; 4=0; 5=1; 6=0; 7=1; 8=0.

## 29/10 17:42 · sessao 5694306ca989 · - · - · origem=Ronald (tmux)

- **Classe 5 — censo do selo incompleto:** `app/escala/tests/test_selo_grade_pede_o_instante.py:25` omite `pautas_discordantes.Command.apurar`, embora `app/ponto/management/commands/pautas_discordantes.py:56` torne `agora` obrigatório. Retirar `agora=` do chamador em `app/ponto/management/commands/pautas_discordantes.py:112` passa pelo filtro do selo (`app/escala/tests/test_selo_grade_pede_o_instante.py:51`) e pela inspeção de assinaturas (`app/escala/tests/test_selo_grade_pede_o_instante.py:92`). Confirmei executando somente o AST em memória: `chamadas_sem_instante('self.apurar(mes, ano, empresas, hoje)')` retorna `[]`.

Os casos F1c (`app/escala/tests/test_f1c_contrato.py:36`) e O197 (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:206`) passam 2100 para cenários de dia corrente. Isso contradiz as expectativas de recorte em `app/escala/tests/test_f1c_contrato.py:39` e `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:330`; são testes quebrados pela migração, não exemplos fortes de classe 5 (“passaria com o defeito”).

## 29/10 17:42 · sessao 846b1fa92892 · - · - · origem=Ronald (tmux)

Achados de classe 3:

- `bin/r3_turno_aberto_alcanca.py:34,42,48`: `HOJE` vem de `timezone.localtime()` sem argumento; depois outro relógio fornece `_AGORA_DO_ATO`. A mesma chamada de espelho recebe os dois.
- `app/relatorios/management/commands/diff_pdf_x_espelho.py:50,75`: captura `_agora_do_ato`, mas chama `_ate_ontem` sem `hoje=`. O helper relê `timezone.localdate()` em `app/relatorios/cartao_pela_celula.py:40`.
- `app/ponto/services/flip_auto.py:263,268`: captura `agora`, depois chama `fila_flip`; esta chama `pares_mesmo_tipo` e `fila_flip_recusas` (`:146,148`), que releem relógio (`:34,126`).
- `app/relatorios/pdf_espelho.py:1088,1099`: captura instante para coleta/corpo, depois chama `gerar_pdf_auditavel` sem repasse. O pipeline lê outro instante em `app/relatorios/pdf_base.py:133`, usado no rodapé (`:139,170`).
- `app/colaboradores/services/ficha.py:117,155`: captura `agora`, depois chama `particionar_verbos`. Esta relê relógio em `app/chamados/services/verbos.py:32`, usado em vencimento e idade (`:48,54`).

Cadeia adicional compartilhada, para avaliar o alcance do escopo:

- `app/ponto/services/espelho.py:1288` chama `folha_manda` sem repassar `agora`; `app/relatorios/cartao_pela_celula.py:323,73` chama `totais_da_folha → fechamento_lido`; `app/ponto/services/fechamento.py:1718` chama `recalcular_fechamento_mes`, que lê relógio em `:595`, utilizado no teto dos furos em `:1085`. Isso continua acontecendo dentro do espelho que agora recebe instante obrigatório (`app/ponto/services/espelho.py:1198`).

Um carimbo adicional:

- `app/ponto/management/commands/reconciliar_grade.py:75,228` captura instante e chama `_relavrar_orfa`, que relê relógio em `:458`. É carimbo de escrita; a relevância para o teto do julgamento é [não verificado].

_199 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
