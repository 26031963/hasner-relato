# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 19:13** · publicado em **10/11/2026 19:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 19:13 · sessao ebd73717303d · BO-DISPUTA-SEM-PERGUNTA · leitura · origem=Code

**A cópia ainda permite nascimento persistido de DS sem pergunta própria:** o núcleo aceita geração vazia, o agrupador pode fechar a recém-criada como `'vazia'`, e os consumidores de declaração não desfazem o nascimento quando recebem pergunta estrangeira e terminam normalmente. A guarda de visitadas desta volta também pode encerrar o redirecionamento conservando o motivo na DS corrente. (`app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa`:618–630, 651–680; `::criar_disputa_a_partir_de_chamados_orfaos`:1226–1231, 1385–1391; `app/chamados/services/declaracao_texto.py::declarar_hora_colab`:376–426)

As referências abaixo são desta cópia. Para abreviar: `disputa_emissao.py`, `declaracao_texto.py`, `fio.py`, `materializacao.py`, `regularizacao_ext.py` e `vigia_fio.py` ficam em `app/chamados/services/`; `models.py` e `signals.py`, em `app/chamados/`; os testes sem caminho completo, em `app/chamados/tests/`. Certificação por execução, banco, logs, concorrência e entrega de push: **[nao verificado]**. [nao verificado]

**1. `app/ponto/management/commands/reconciliar_grade.py::Command._disputa`.**

O único chamador concreto encontrado no repositório é `test_porta_chamado_e3.py::IdempotenciaDaFamiliaChamado.test_MORDE_a_disputa_da_grade_nasce_UMA_vez`: chama o método duas vezes e exige mesmo PK, uma DS aberta, pai e SLA. Não foi encontrado chamador produtivo, `getattr` ou apelido desse método; o fluxo do comando emite por `_emitir_furo`. Chamadas por reflexão construída em execução: **[nao verificado]**. (`app/chamados/tests/test_porta_chamado_e3.py:91–98`; `app/ponto/management/commands/reconciliar_grade.py::Command.handle`:250–254)

O método chama diretamente `_abrir_ou_agregar_disputa` com `motivos=[]`, origem/contexto de grade, e retorna a DS. Portanto, quando não encontra aberta, seu próprio corpo permite pai/DS sem pergunta. (`app/ponto/management/commands/reconciliar_grade.py::Command._disputa`:412–428; `disputa_emissao.py::_abrir_ou_agregar_disputa`:651–680)

Os testes que o sustentam são: [nao verificado]

| Teste | O que sustenta | [nao verificado]
|---|---|
| `test_porta_chamado_e3.py::IdempotenciaDaFamiliaChamado.test_MORDE_a_disputa_da_grade_nasce_UMA_vez` | Chama o método real; verifica reuso, pai e SLA. (`:91–98`) |
| `test_sla_pelo_juiz.py::OsEmissoresCarimbamOPrazoTest.test_MORDE_a_grade` | Inspeção de fonte: delegação ao núcleo, sem criação direta de DS. (`:115–123`) |
| `test_porta_chamado_e3.py::OsSitiosChamamOAto.test_cada_sitio_chama_o_seu_ato` | Selo AST dos sítios delegantes, incluindo a grade. (`:101–121`) |
| `test_bo_disputa_sem_pergunta_memoria.py::ProvasMemoria.test_chamada_grade_motivos_vazios_conta_estrutura` | Chama **o núcleo diretamente**, com motivos vazios; espera I7a=1 em doubles. Não chama `_disputa`. (`:416–420`) |

**O comando conserva caminhos escritores com `--apply`:**

- `Command.handle` chama `_emitir_furo` sob `apply and not o['so_retratar']`; este abre chamado e pode reabri-lo. (`app/ponto/management/commands/reconciliar_grade.py:250–254`; `::Command._emitir_furo`:507–532)
- `Command.handle`, sob `if apply:`, chama `reconciliador.retratar`. (`app/ponto/management/commands/reconciliar_grade.py:305–316`)
- `Command._relavrar_orfa`, com aplicação, retira `orfa_regeneracao` ou renova contexto com `marcos_faltantes`/`orfa_regeneracao_curada`, além da trilha. (`app/ponto/management/commands/reconciliar_grade.py:452–454, 472–485`)

O cron declarado chama sem `--apply`; os selos de sombra provam esse contrato restrito, não ausência de escritor no comando. (`app/config/crons.py:460–462`; `test_hx_um_emissor.py::UmEmissorTest.test_reconciliar_grade_sem_apply_no_cron`:5–9; `app/ponto/tests/test_reconciliar_grade_sombra.py::ReconciliarGradeNaoEscreveTest.test_GOLDEN_sem_apply_nao_escreve_NADA`:58–63)

**2. `disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos`.**

**(i) Ordem exata.** Antes do nascimento local, pode complementar perguntas em DS preexistentes. Depois filtra os chamados, procura uma DS aberta e, não encontrando, abre/reutiliza pai e cria a DS em **1226–1231**. Os laços locais de perguntas começam depois: intervalo, ata, episódio, sem origem e pendentes. Logo, cria a DS antes de conhecer o resultado desses laços. (`disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos`:1168–1197, 1199–1231, 1241–1281, 1296–1355, 1373)

**(ii) Fecho com zero perguntas.** O agrupador consulta `disputa.pode_fechar()`, não uma contagem SQL de perguntas. Se houve abstenção por ata e poderia fechar, acende luz e conserva aberta; no outro ramo permitido, chama `fechar` e retorna antes do push. (`disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos`:1381–1393) [nao verificado]

Com zero linhas de pergunta, `DisputaSupervisao.fechar` escolhe `FECHOU_VAZIA` e grava:

- `fechada_em`;
- `fechada_por`;
- `resolucao`;
- `motivo_fechamento='vazia'`;
- evento `disputa_fechada`.

A DS, seus motivos/contexto e seu pai não são apagados por esse ramo. (`models.py::DisputaSupervisao.fechar`:1895–1903, 1932–1940)

O ramo de fecho não marca diretamente os chamados órfãos. Nos laços anteriores, `analisar()` depende do booleano de criação/complemento; dia completo pode fazer `continue` antes disso. Zero perguntas **locais**, porém, não prova que nenhum órfão foi analisado: o helper pode retornar verdadeiro após reutilizar pergunta estrangeira. (`disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos`:1261–1262, 1280–1281, 1304–1305, 1323–1326, 1341–1355; `::_criar_pergunta_se_couber`:940–950)

O pai fica sujeito aos receivers descritos no item 4; alterações efetivamente produzidas nos órfãos/pai por cascatas: **[nao verificado]**. (`signals.py::disputa_fechada_fecha_chamado_pai`:28–45)

Os testes caracterizam expressamente DS fechada com zero perguntas e ausência de notificação. (`test_hx_disputa_muda_sem_ata.py::DisputaMudaSemAtaTest.test_com_ata_e_dia_completo_fecha_como_antes`:63–79; `test_hx_disputa_muda.py::test_sem_nada_a_perguntar_nao_fica_aberta`:24–33; `::test_nao_notifica_disputa_muda`:44–50) [nao verificado]

**(iii) Leitores de `'vazia'`.** A busca pelas referências ao campo, constante e contador encontrou estes consumidores:

| Arquivo::função | Leitura e decisão |
|---|---|
| `vigia_fio.py::disputa_vazia` | Conta `motivo_fechamento=DisputaSupervisao.FECHOU_VAZIA`. Não reconta perguntas, não filtra abertura e não marca órfãos. É o consumidor funcional específico encontrado. (`:727–734`) |
| `test_c7_red_minimos.py::C7_04_DisputaVaziaTest.test_RED_vazia_fecha_com_motivo_vazia_e_o_contador_conta` | Exige `'vazia'` e incremento de um no contador. (`:174–183`) |
| `test_c7_red_minimos.py::C7_04_DisputaVaziaTest.test_RED_o_contador_MORDE_disputa_com_pergunta_nao_conta` | Exige que disputa com pergunta não incremente o contador. (`:185–192`) |
| `test_c7_red_minimos.py::test_MORDE_o_update_em_lote` | Usa uma consulta textual por `'vazia'` como entrada do detector AST; não executa essa consulta ORM. (`:613–616`) |
| `models.py::DisputaSupervisao.fechar` | Leitura genérica do motivo para idempotência: entrada compatível retorna falso; fechamento diferente recusa. Não contém tratamento especial de reagrupamento para `'vazia'`. (`:1879–1883`) |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle` | Serializa o campo no registro de reversão; não decide pela igualdade com `'vazia'`. (`:128–138`) |

Não foi encontrado chamador produtivo de `vigia_fio.disputa_vazia`; seus chamadores executáveis encontrados são os testes acima. Os registros de inventário apenas classificam/declararam o campo ou contador. (`test_c7_red_minimos.py:178–192`; `bin/sombra_regras_pessoais.py:177`; `bin/esmeril_chamado_censo.py:134`)

**(iv) Seleção seguinte e idempotência.** O filtro do agrupador não consulta `motivo_fechamento`, nem os IDs guardados no contexto de DS/pai. Exclui dia na lista do admin; depois exige regularizabilidade e, salvo o caminho por ata, ausência de pergunta não materializada pelo destino. A seleção de DS considera somente abertas. Assim, **a DS fechada vazia também não funciona como selo de tentativa anterior**. (`disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos`:1150–1152, 1189–1201, 1219–1229)

`chamado_orfao_e_regularizavel` delega a `motivo_fora_da_fabrica`: categoria/status, situação da célula/pergunta, turno resolvido e mapa. Não há marcador externo de “conjunto já tentado” nesse contrato. Se continuarem elegíveis e forem fornecidos novamente, os mesmos chamados podem voltar; a próxima passada real é **[nao verificado]**. (`disputa_emissao.py::motivo_fora_da_fabrica`:850–890; `::chamado_orfao_e_regularizavel`:893–899)

Os chamadores produtivos encontrados e seus filtros são:

| Arquivo::função | Universo fornecido |
|---|---|
| `app/chamados/services/acoes_disputa.py::acao_iniciar_disputa_orfaos` | Chamados do colaborador filtrados por `chamado_orfao_e_regularizavel`. (`:210–224`) |
| `app/chamados/services/pergunta_no_app.py::perguntar` | Itens com causa `'fabrica_nao_rodou'`, agrupados por colaborador. A medição antecedente usa COBRANCA, módulos de furo e colaborador ativo. (`:197–217`; `::medir`:144–153) |
| `app/chamados/services/cobrar_dia.py::cobrar_dia` | IDs do dia cuja causa é `'fabrica_nao_rodou'`, com notificação. (`:45–49`) |
| `app/ponto/management/commands/detectar_ausencias.py::Command._abrir_chamado` | Chamados do colaborador em COBRANCA e regularizáveis. (`:790–806`) |
| `app/ponto/management/commands/processar_alertas_turno.py::Command._abrir_chamado_critico` | Quando criou o alerta: COBRANCA e regularizáveis. (`:372–389`) |
| `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle` | No ramo de criação: COBRANCA e regularizáveis. (`:147–163`) |

Há também reexportação de compatibilidade em `models.py`, sem outro implementador. (`models.py:4093`)

**(v) Retorno estrangeiro ignorado.** `disputa_emissao.py::_criar_pergunta_se_couber` acrescenta o motivo antes dos vetos; chama `PerguntaDisputa.abrir` como expressão, sem capturar `(pergunta, criada)`; depois devolve `(motivos, True)` incondicionalmente. (`:931–950`)

A porta elege a canônica no universo do fato e pode devolver `ex` de outra DS, preservando sua dona. Portanto aquele `True` não prova criação nem pertencimento local; `_complementar_marcos_faltantes` apenas acumula esse booleano. (`models.py::PerguntaDisputa._nascer`:2905–2921, 3008; `disputa_emissao.py::_complementar_marcos_faltantes`:984–987)

**3. Saídas de `gerar_perguntas_disputa` e `_resolver_pergunta_por_motivo`.**

Na tabela, “sem emissão” inclui dedup que já encontrou pergunta local. Isso não significa necessariamente zero linhas locais na DS. O gerador devolve apenas a lista acumulada; os motivos omitidos não levam código de causa ao núcleo. (`disputa_emissao.py::gerar_perguntas_disputa`:402–419, 483–484)

| Arquivo::função e saída | Condição literal | Classe pelo código | O núcleo distingue pelo retorno? |
|---|---|---|---|
| `disputa_emissao.py::gerar_perguntas_disputa`, `continue` 406 | `pergunta_obj is None` | **D — catálogo:** `pergunta_para_motivo` não encontrou pergunta. (`:404–406`; `app/chamados/catalogo/query.py::pergunta_para_motivo`:16–18) | Não: motivo ausente da lista. |
| Mesma função, `continue` 419 | `motivo not in MOTIVOS_DATADOS_SET and PerguntaDisputa.batida_origem_do_nascimento(disputa, motivo) is None and PerguntaDisputa.objects.filter(disputa=disputa, motivo=motivo).exclude(via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO).exists()` | **B — pergunta já na própria DS**, no domínio desse dedup. Não exige resposta nem resolução. (`:415–419`) | Não: motivo ausente da lista. |
| `disputa_emissao.py::_resolver_pergunta_por_motivo`, `return None` 329 | Segundo `if texto is None:`, após tentar `_data_furo_da_grade` | **D — texto/data:** continua impossível obter texto. Emite warning; não chama a luz HX. (`:309–329`) | Não: helper devolve `None`; gerador faz `continue` em 435–436. |
| Mesma função, primeiro lado do `return None` 364 | Dentro de `motivo in MOTIVOS_DATADOS_SET and motivo not in MOTIVOS_QUE_PRECISAM_CHAMADO_DESTINO_SET`: `_data_fallback is None` | **D — dia ausente.** (`:358–364`) | Não: o mesmo `None`. |
| Mesma função, segundo lado do mesmo `return None` 364 | `PerguntaDisputa.objects.filter(disputa=disputa, motivo=motivo, data_evento=_data_fallback, conversa=False).exclude(via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO).exists()` | **B — pergunta local do motivo/dia**, fora conversa e vias excluídas. (`:361–364`) | Não: indistinguível da falta de dia nesse retorno. |
| Mesma função, `return None` 383 | Sob `motivo in MOTIVOS_QUE_PRECISAM_CHAMADO_DESTINO_SET`: `PerguntaDisputa.objects.filter(disputa=disputa, motivo=motivo, data_evento=_data_ev, conversa=False).exclude(via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO).exists()` | **B — pergunta local no dia derivado**; `_data_ev` pode ser `None`. (`:373–383`) | Não: o mesmo `None`. |
| `disputa_emissao.py::gerar_perguntas_disputa`, `continue` 436 | `resolvida is None` | Propaga as quatro causas anteriores; não acrescenta outra classificação. (`:434–436`) | Não. |
| Mesma função, `continue` 443 | `_dia_p is None and pergunta_obj.tipo_resposta == 'hora'` | **D — hora sem dia do turno.** Só registra `logger.info`. (`:438–443`) | Não: motivo ausente da lista. |
| Mesma função, `continue` 451 | Sob `_dia_p is None`: `PerguntaDisputa.objects.filter(disputa=disputa, motivo=motivo, conversa=True).exclude(via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO).exists()` | **B — conversa já na própria DS**; conversa não possui identidade de fato. (`:444–451`; `models.py::PerguntaDisputa.abrir_conversa`:2794–2798) | Não: motivo ausente da lista. |

Há ainda uma saída sem pergunta **nova local**, sem `continue`: `abrir` pode devolver canônica estrangeira; o gerador acrescenta essa pergunta à lista, descartando `_criada` no retorno público. Aqui o núcleo consegue identificar a dona por `pergunta.disputa_id`. Nesta volta, porém, só redireciona se ela também não estiver em `_donas_visitadas`; se já visitada, pode retornar a DS corrente normalmente. (**B** quanto à pergunta existente.) (`disputa_emissao.py::gerar_perguntas_disputa`:457–465, 483–484; `::_abrir_ou_agregar_disputa`:618–630)

**Não há saída A explícita nessas duas funções.** Nem o warning de texto nem a hora sem dia chamam `acender_sem_ata`. A abstenção HX está em `_ata_do_dia` e `_complementar_marcos_faltantes`, que chamam `declaracao_texto.py::acender_sem_ata`. (`disputa_emissao.py::_resolver_pergunta_por_motivo`:323–329; `::gerar_perguntas_disputa`:438–443; `::_ata_do_dia`:1005–1015; `::_complementar_marcos_faltantes`:963–970; `declaracao_texto.py::acender_sem_ata`:63–72)

**Também não há saída C explícita por ausência cobrindo o fato ou resposta resolvida nessas duas funções.** Os dedups acima perguntam existência, sem provar resolução. A guarda `_coberto_por_ausencia` está em `_criar_pergunta_se_couber`, fora delas. (`disputa_emissao.py::gerar_perguntas_disputa`:415–451; `::_resolver_pergunta_por_motivo`:358–383; `::_criar_pergunta_se_couber`:933–936)

Lista de motivos vazia percorre zero iterações e devolve `[]`; o núcleo não exige uma pergunta local antes de confirmar o nascimento. (**E — nenhum motivo fornecido**.) (`disputa_emissao.py::gerar_perguntas_disputa`:402–403, 484; `::_abrir_ou_agregar_disputa`:651–680)

**4. Save da dona fechada, receivers e push.** [nao verificado]

A dona explícita é buscada por PK sem filtro de abertura; o núcleo pode salvar motivos/contexto em `:614` sem escrever reabertura. Nenhum dos três receivers diretos de DS examina `update_fields` ou a transição de `fechada_em`. (`disputa_emissao.py::_abrir_ou_agregar_disputa`:575–614; `signals.py:18–45, 120–163`)

| `signals.py::função` | Guarda literal | Escritas/ações possíveis |
|---|---|---|
| `disputa_fechada_fecha_chamado_pai` | `if created or not instance.fechada_em or not instance.chamado_id:`; depois `if not ch or ch.status_local in ENCERRADOS:` | Chama `ch.reconciliar()` em atomic; somente se retornar verdadeiro anexa ocorrência ao pai. Captura/loga exceção. (`:28–50`) |
| `retoma_regularizacoes_ao_fechar_disputa` | `if raw or created or not instance.fechada_em:` | Registra `transaction.on_commit(_retomar)`. Callback usa `fechada_por` ou sistema e chama retomada das regularizações do colaborador. Captura/loga exceção. (`:129–144`) |
| `materializa_perguntas_ao_fechar_disputa` | `if raw or created or not instance.fechada_em:` | Chama sincronamente `materializar_pendentes_aprovadas(instance, instance.fechada_por)`. Captura/loga exceção. (`:153–165`) |

As escritas delegadas são:

- **Reconciliação do pai:** decisão com `registrar=True`, possível aplicação de reata/dedup, escrita de `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`, urgência/urgência original, evento de estado e possível relavra de perguntas apontadas ao pai. A ocorrência do receiver cria followup. (`models.py::ChamadoColaborador.reconciliar`:641–655; `::_decisao_do_fechamento`:682–692; `::_gravar_estado`:228–253, 292–310; `app/chamados/services/nota.py::anexar_ocorrencia`:103)
- **Retomada no commit:** seleciona regularizações `status='confirmada', materializada_em__isnull=True`, pelo colaborador. O materializador ainda recusa se houver outra DS aberta. (`regularizacao_ext.py::retomar_regularizacoes_confirmadas`:218–224; `::materializar_regularizacao_externa`:58–65) [nao verificado]
- **Materialização da regularização:** pode criar batidas e auditoria; remover batida e gravar `batida_alvo/aplicada_em`; corrigir `tipo/timestamp` e marcar aplicação; gravar referências `batida_entrada/batida_saida`; gravar `status/materializada_em/materializada_por` da regularização; anexar nota e encerrar seu chamado. (`regularizacao_ext.py::materializar_regularizacao_externa`:76–102, 131–164, 166–205) [nao verificado]
- **Pendentes aprovadas:** seleciona perguntas locais não materializadas, pula as em voo e as reprovadas pelo juiz, depois chama `carimbar(VIA_JUIZ_APROVOU, ...)`. Essa via não fabrica batida. (`materializacao.py::materializar_pendentes_aprovadas`:1408–1411, 1418–1432)
- **Carimbo:** pode escrever `materializada_em`, `materializada_por`, `via_resolucao`; neste caller não são pedidos resposta, validação ou nova lavra. O save de pergunta pode agendar fechamento de beco e emitir evento de célula. (`models.py::PerguntaDisputa.carimbar`:3120–3152; `signals.py::_becos_post_save_pergunta`:227–238; `::_outbox_post_save_pergunta`:251–257)

Se o pai for salvo, entram também os receivers de Chamado: urgência, resolução de perguntas, cartório, silêncio de isentos e despertar do pai pelo filho. Quais alterações eles efetivamente produzem nesta situação dependem do acervo: **[nao verificado]**. (`signals.py::rebaixa_urgencia_ao_resolver`:54–72; `::resolve_perguntas_ao_resolver`:91–115; `::_cartorio_pre_save_chamado`/`::_cartorio_post_save_chamado`:178–216; `signals.py:283–306, 350–365`)

**Nenhum dos três receivers de DS é no-op provado apenas por esse save ser de motivos/contexto.** O primeiro possui saídas condicionais por pai ausente/encerrado; o segundo registra callback; o terceiro consulta e tenta carimbar pendentes. Existência de linhas aptas e persistido final: **[nao verificado]**. (`signals.py:28–45, 129–144, 153–163`; `materializacao.py:1418–1432`)

Quanto ao push de `disputa_emissao.py::criar_ou_atualizar_disputa`: [nao verificado]

- Se `nascida=True`, chama push com todos os motivos, sem conferir pergunta local. (`:774–776`) [nao verificado]
- Se `nascida=False`, executa pendentes e depois chama push se `novos`; não confere fechamento nem pergunta local. (`:788–791`) [nao verificado]
- A recuperação pode devolver uma DS fechada e retorna apenas a primeira dona, com união dos motivos novos das várias donas. (`::_abrir_ou_agregar_disputa`:578, 695–707)
- `_enviar_push_disputa_colab` só possui a guarda contextual de silêncio; usa o colaborador da DS recebida, me

[truncado em 20000 de 29457 caracteres]

## 10/11 19:12 · sessao 461234e6110a · BO-0611-05 · verificador · origem=Code

**NAO LIBERA a obra inteira para pouso neste estado de evidência.** A correção dos dois leitores está coerente com o juiz registrado, mas persistem pendências da classe e faltam as provas de runner/RED/smoke indicadas abaixo. (`app/core/templatetags/hasner_filters.py:38`; `app/core/juizes.py:1201`; `app/docs/LEIS.md:165`; `app/docs/CORTES.md:440`)

A comparação histórica abaixo usa os diffs fornecidos. A cópia não disponibiliza Git para conferir os hashes, executar o checkout da base ou autenticar `7547c8367`: **[nao verificado]**.

1. **O que significa “tem coordenada” agora e na base**

Ambos perguntam a `geofence.tem_gps(latitude, longitude)`, que retorna `bool(latitude and longitude)`. O modal exige também `modulo_origem == "gps_fora_geofence"`. (`app/ponto/services/geofence.py:145`; `app/templates/chamados/partials/_relatorio_geo.html:31`; `app/templates/chamados/partials/modal_fio.html:481`)

Para `x` e `y` numéricos diferentes de zero:

| Par no contexto | Agora, ambos | Base da obra, ambos |
|---|---:|---:|
| `(0.0, 0.0)` | Não | Não |
| `(None, None)` | Não | Não |
| `(x, None)` | Não | **Sim** |
| `(None, y)` | Não | Não |
| `(x, y)` | Sim | Sim |
| `('0', '0')` | **Sim** | **Sim** |

A coluna atual decorre do corpo do juiz; a base testava apenas a latitude, conforme os hunks fornecidos para `6c0517eb9`. A condição adicional do módulo vale para toda a coluna do modal. (`app/ponto/services/geofence.py:145`; `6c0517eb9:app/templates/chamados/partials/_relatorio_geo.html:30`; `6c0517eb9:app/templates/chamados/partials/modal_fio.html:480`)

Assim, a volta 8 recupera a rejeição do zero numérico que a nulidade da volta 3 havia perdido e também elimina o mapa com latitude preenchida e longitude nula. (`app/templates/chamados/partials/_relatorio_geo.html:31`; `app/templates/chamados/partials/modal_fio.html:481`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:44`)

Com chave ausente — latitude, longitude ou ambas — o resultado esperado é condição falsa. Longitude ausente é particularmente relevante porque é argumento do filtro e sua resolução pode produzir `VariableDoesNotExist`, tratado pelo `if`. A execução desse caso no Django desta obra permanece **[nao verificado]**; a fixture sempre fornece ambas as chaves. (`app/ponto/tests/test_bo0611_05_templates_geo.py:132`)

2. **Escritores, cópias, leitores e tipos**

Os produtores explícitos do par no aplicativo são:

| Escritor/cópia | Destino e tipo |
|---|---|
| `app/ponto/services/geofence.py:349` e `:350` | `_occ`: `float(x) if x else None`. |
| `app/ponto/services/geofence.py:391` e `:392` | Par escalar de `contexto_json`, com a mesma conversão. |
| `app/ponto/services/geofence.py:375` | Nascimento do chamado guarda o par escalar e `ocorrencias: [_occ]`. A persistência passa por `ChamadoColaborador.criar`. (`app/chamados/models.py:1095`) |
| `app/ponto/services/geofence.py:359` | Ocorrência adicional passa por `nota.anexar_ocorrencia`, delega à porta de contexto e une listas sem converter os valores. (`app/chamados/services/nota.py:110`; `app/chamados/services/nota.py:123`; `app/chamados/services/contexto.py:147`; `app/chamados/services/contexto.py:163`) |
| `app/chamados/services/acoes_chamado.py:203` e `:204` | Lê o contexto e copia o par, sem conversão, para **`Advertencia.detalhe_json`**; não é outro escritor de `contexto_json`. (`app/chamados/services/acoes_chamado.py:193`; `app/chamados/services/acoes_chamado.py:197`) |

Consequência dos escritores: zero **numérico** vira `None`; texto `"0"` é truthy e vira `0.0`. Isso vale tanto na ocorrência quanto no par escalar. (`app/ponto/services/geofence.py:349`; `app/ponto/services/geofence.py:350`; `app/ponto/services/geofence.py:391`; `app/ponto/services/geofence.py:392`)

Os leitores diretos são as guardas, atributos, texto e URL dos dois templates; a cópia para advertência; a extração de ocorrências/legado; e a reconciliação dessas ocorrências. (`app/templates/chamados/partials/modal_fio.html:481`; `app/templates/chamados/partials/modal_fio.html:485`; `app/templates/chamados/partials/_relatorio_geo.html:31`; `app/chamados/services/acoes_chamado.py:203`; `app/ponto/services/geofence.py:405`; `app/ponto/services/geofence.py:406`; `app/ponto/services/geofence.py:425`)

O leitor JS indireto recebe `data-blat/data-blon`, encaminha as strings pelo wrapper, aplica `parseFloat` e usa `isNaN` como guarda. Essa regra aceita zero; a obra mudou sua alimentação pelo botão do modal, que agora não nasce para zero numérico. (`app/static/js/geo-colab-map.js:97`; `app/static/js/geo-colab-map.js:101`; `app/static/js/geo-colab-map.js:29`; `app/static/js/geo-colab-map.js:39`; `app/templates/chamados/partials/modal_fio.html:481`)

Entre os leitores de **latitude/longitude tocados pela obra inteira**, ainda existem regras próprias de nulidade: os três `tem_geo` e o seletor do detector de vínculo. Eles aceitam zero numérico, diferentemente do juiz. O tratamento do zero já era preexistente nesses caminhos; a obra acrescentou a exigência de longitude. (`app/colaboradores/views.py:337`; `app/colaboradores/views.py:356`; `app/colaboradores/views.py:395`; `app/ponto/services/vinculo_na_lista.py:49`; `app/ponto/services/geofence.py:145`)

As fixtures também escrevem o par: a desta volta produz `float/None`, preservando zero; há contextos explícitos nos demais testes de geofence. (`app/ponto/tests/test_bo0611_05_templates_geo.py:132`; `app/chamados/tests/test_s127_fluxo_geofence.py:26`; `app/chamados/tests/test_geofence_volta_no_fio.py:145`; `app/chamados/tests/test_geofence_validar_e_recusar.py:47`; `app/core/tests/test_smoke_chromium_fio_geofence.py:119`; `app/ponto/tests/test_geofence_catalogo_premissa.py:64`; `app/ponto/tests/test_jz1_autoridade_unica.py:194`; `app/ponto/tests/test_reconciliar_geofence.py:42`)

Fora do fluxo do aplicativo, o instrumento de raspagem da sombra também transforma esses JSONs: percorre dicionários/listas e substitui coordenadas numéricas por `None`. Isso não demonstra que tenha sido aplicado ao dado fornecido. (`bin/sombra_regras_pessoais.py:36`; `bin/sombra_regras_pessoais.py:43`; `bin/sombra_regras_pessoais.py:145`) Aplicação efetiva: **[nao verificado]**.

3. **Filtro, `load`, contexto isolado e exceções**

`par_tem_gps` não possui predicado próprio, conversão ou captura de exceção: resolve o módulo no momento da chamada e devolve o resultado do juiz. (`app/core/templatetags/hasner_filters.py:38`; `app/core/templatetags/hasner_filters.py:40`; `app/core/templatetags/hasner_filters.py:41`)

Os `load` estão em posição válida nos partials, antes do uso. Nenhum desses partials usa `extends`; a página consumidora conserva `extends` como primeira tag. (`app/templates/chamados/partials/_relatorio_geo.html:1`; `app/templates/chamados/partials/modal_fio.html:6`; `app/templates/chamados/detalhe_local.html:1`)

As duas inclusões reais do relatório passam `chamado=chamado`, sem `only`: detalhe e modal. O modal também é renderizado diretamente pelas entradas de view/serviço, encontrando seu próprio `load`. (`app/templates/chamados/detalhe_local.html:278`; `app/templates/chamados/partials/modal_fio.html:498`; `app/chamados/views.py:927`; `app/chamados/views.py:1399`; `app/chamados/services/fio.py:496`)

Um include com `only` continuaria precisando fornecer `chamado`; o carregamento da biblioteca pertence ao próprio partial. Não há esse caminho real localizado para os dois partials, nem ensaio dele nesta volta: **[nao verificado]**.

**`str`, `dict` e lista comuns não fazem o juiz atual levantar por tipo.** Valores vazios são falsos; preenchidos são verdadeiros, inclusive pares de dicionários/listas preenchidos. O juiz verifica truthiness, sem validar natureza numérica. (`app/ponto/services/geofence.py:145`)

Se o juiz efetivamente levantar uma exceção, o filtro não a intercepta. Há uma diferença estrutural: relatório usa filtro isolado; modal o usa dentro de `and`. (`app/core/templatetags/hasner_filters.py:41`; `app/templates/chamados/partials/_relatorio_geo.html:31`; `app/templates/chamados/partials/modal_fio.html:481`)

O comportamento esperado do Django é: `TypeError`/`ValueError` propaga no relatório isolado, enquanto o operador `and` do modal transforma a falha em falso. Fonte/runtime do Django não disponíveis nesta cópia para confirmar esse detalhe: **[nao verificado]**. Na base, esses templates não chamavam o juiz; portanto essa origem de exceção não existia. (`6c0517eb9:app/templates/chamados/partials/_relatorio_geo.html:30`; `6c0517eb9:app/templates/chamados/partials/modal_fio.html:480`)

4. **Valores dentro e fora das guardas**

Nenhuma leitura direta do par escapa da guarda nos dois templates: o relatório guarda texto e link; o modal guarda os atributos e a inclusão do relatório. (`app/templates/chamados/partials/_relatorio_geo.html:31`; `app/templates/chamados/partials/_relatorio_geo.html:32`; `app/templates/chamados/partials/_relatorio_geo.html:33`; `app/templates/chamados/partials/_relatorio_geo.html:36`; `app/templates/chamados/partials/modal_fio.html:481`; `app/templates/chamados/partials/modal_fio.html:485`; `app/templates/chamados/partials/modal_fio.html:498`; `app/templates/chamados/partials/modal_fio.html:543`)

Para pares numéricos nulos, parciais ou com zero, o juiz impede esses controles. Para `('0','0')`, permite-os; para tipos preenchidos inesperados, também pode permitir texto/atributos inadequados. Isso decorre do juiz atual, não de uma saída do bloco guardado. (`app/ponto/services/geofence.py:145`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:79`)

5. **Mordida dos testes e vacuidade** [nao verificado]

Não é correto afirmar que **todo teste novo** falharia por asserção sobre zero contra a árvore anterior. A análise é: [nao verificado]

| Teste novo | Com a guarda anterior de nulidade | [nao verificado]
|---|---|
| `ContextoJsonLeOJuizTest.test_MORDE_relatorio` e `test_MORDE_modal` | Falhariam na asserção de HTML do zero: `morder` exige primeiro mapa para o completo, depois ausência para zero/parciais. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:200`; `:214`; `:218`; `:223`; `:227`) |
| Mutante de nulidade com controle verde | Falharia já no controle original com zero; no código atual, renderiza o mutante antes de esperar a falha específica. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:233`; `:237`; `:238`) |
| Consulta ao juiz durante render | Falharia primeiro em `assert_any_call` do par completo, porque a guarda antiga não chamava o juiz; não chegaria ao zero. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:247`; `:249`) |
| Delegação de `par_tem_gps` | Na árvore sem o filtro, daria `AttributeError`, não RED por asserção de zero. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:120`; `:126`) |
| Caracterização de texto zero | Deve passar também com o juiz anterior à volta 8: documenta comportamento preservado. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:79`) |

Identidade da árvore histórica e RED realmente executado contra `7547c8367`: **[nao verificado]**.

Não encontrei as três vacuidades propostas:

- A fixture usa `is not None`, preservando `0.0`; o teste exige chamada com `(0.0, 0.0)`. (`app/ponto/tests/test_bo0611_05_templates_geo.py:132`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:253`)
- O patch precisa ser chamado e alterar a saída; patch inerte falha em `assert_any_call`. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:249`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:250`)
- Render e controle ficam fora de `assertRaisesRegex`; dentro fica a asserção específica sobre controles geográficos. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:237`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:238`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:200`)

A mudança de `_CTX/_PAR` preserva o mutante “sem longitude”: a troca exige uma âncora única e substitui a pergunta ao juiz por latitude não nula. Os mutantes da volta 7 continuam independentes dessa mudança. (`app/ponto/tests/test_bo0611_05_templates_geo.py:37`; `app/ponto/tests/test_bo0611_05_templates_geo.py:55`; `app/ponto/tests/test_bo0611_05_templates_geo.py:201`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:156`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:171`)

`TemplatesGeoTest.morder` continua sem caso zero; a cobertura numérica adicional está na classe nova. Chaves ausentes, JSON textual original, listas/dicionários e juiz levantando não são cobertos por essa fixture. (`app/ponto/tests/test_bo0611_05_templates_geo.py:132`; `app/ponto/tests/test_bo0611_05_templates_geo.py:178`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:218`)

6. **Classes estruturais já reprovadas**

Os filtros não criam segundo juiz; o juiz está registrado e o próprio emissor o chama. As criações de Batida examinadas continuam na porta, antes da avaliação de geofence. (`app/core/templatetags/hasner_filters.py:29`; `app/core/templatetags/hasner_filters.py:38`; `app/core/juizes.py:1201`; `app/ponto/services/geofence.py:269`; `app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:157`; `app/api/views_core.py:640`; `app/api/views_core.py:770`)

O juiz e os filtros não leem relógio, banco ou trava. Concorrência, ordem efetiva das travas e sequência real de pouso não foram exercitadas: **[nao verificado]**. (`app/ponto/services/geofence.py:126`; `app/core/templatetags/hasner_filters.py:29`)

Há diferença de **tipo**, com efeito semântico: PWA transmite por FormData; o sanitizador valida com `float(v)` sem substituir o valor; o campo persistido é DecimalField; o emissor converte para float no JSON; o HTML vira texto e o JS aplica `parseFloat`. Texto `"0"` pode ser aceito antes da conversão e zero numérico rejeitado depois. (`app/static/pwa.js:145`; `app/ponto/registro_batida.py:15`; `app/ponto/models.py:58`; `app/ponto/services/geofence.py:391`; `app/templates/chamados/partials/modal_fio.html:485`; `app/static/js/geo-colab-map.js:29`)

O selo AST da porta tem alcance limitado: procura `Response` 403 dentro de `If` anterior à porta que leia geo/situação. Não prova ausência de qualquer barreira. O selo do modal ficou condicional à presença de `hxConfirmar(`. (`app/api/tests/test_l030_selo_porta.py:37`; `app/api/tests/test_l030_selo_porta.py:48`; `app/api/tests/test_l030_selo_porta.py:60`; `app/core/tests/test_selo_modal_nao_nasce_vazio.py:141`)

7. **Proibidos e fechamento**

Não identifiquei coordenada inventada nos caminhos examinados: ausência permanece ausência; coordenada parcial não ganha a outra componente. O geofence conserva o predicado original, agora nomeado. (`app/ponto/registro_batida.py:11`; `app/ponto/registro_batida.py:17`; `app/api/tests/test_l030_batida_pela_porta.py:71`; `app/ponto/services/geofence.py:145`; `app/ponto/services/geofence.py:269`)

A volta 8 não passa a aceitar `0.0` nos dois leitores. Outros caminhos tocados pela obra inteira continuam aceitando zero, inclusive com teste exigindo-o como evidência no detector; isso é resíduo preexistente, não introdução desta volta. (`app/templates/chamados/partials/modal_fio.html:481`; `app/templates/chamados/partials/_relatorio_geo.html:31`; `app/ponto/services/vinculo_na_lista.py:49`; `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135`)

Pelo diff integral fornecido, não há alteração de `motor_calculo_v2`; comparação independente com a base por Git: **[nao verificado]**.

**Achados e pendências para liberação:**

- **Classe “nunca barrar” ainda incompleta:** colisão de tipo oposto continua sem materializar, respondendo `ignorada_intervalo`; selfie continua interrompendo no cliente. São resíduos anteriores, mas impedem declarar a obra inteira como cura completa da classe. (`app/ponto/registro_batida.py:145`; `app/api/views_core.py:655`; `app/templates/ponto/bater_ponto_v2.html:287`; `app/docs/LEIS.md:165`)
- **Dívida de leitores/tipos sem a linha específica localizada no BACKLOG da cópia:** registro e corte prometem essa fila; o BACKLOG apresenta a entrada genérica da obra, e as antigas posições 1884/1885 contêm outros assuntos. Existência no commit de pouso: **[nao verificado]**. (`app/core/juizes.py:1199`; `app/docs/CORTES.md:442`; `app/docs/BACKLOG.md:1873`; `app/docs/BACKLOG.md:1884`; `app/docs/BACKLOG.md:1885`)
- **Provas pendentes:** runner Django, RED histórico, render de chave ausente/tipos inesperados/exceção e smoke humano nas duas cascas: **[nao verificado]**. O smoke é requisito explícito do pouso do front. (`app/docs/CORTES.md:440`)

**NAO LIBERA.** Os ensaios locais sem Django não substituem essas provas nem autorizam presumir o runner verde. **[nao verificado: resultado final do runner]** [nao verificado]

## 10/11 19:10 · sessao 09f9e0049feb · - · - · origem=Ronald (tmux)

Q6 — leitura estática, sem testes executados; resultado em banco/concorrência: [nao verificado]. [nao verificado]

- Para **universo e dona**, já existem `app/chamados/services/fio.py::universo_do_fato` (`:1784–1837`) e `::canonica_do_grupo` (`:1225–1233`). O primeiro usa `PerguntaDisputa.do_fato`, filtra pelo dia real/instância, exclui episódios retratados e pontes entre dias e fecha **um componente**; a segunda escolhe resposta não vazia e depois menor PK. É exatamente a composição usada por `app/chamados/models.py::PerguntaDisputa._nascer` (`:2905–2921`).
- `app/chamados/models.py::PerguntaDisputa.do_fato` (`:2697–2728`), `::linhas_dos_fatos` (`:2731–2743`) e `::q_dos_fatos` (`:2746–2755`) são leitores puros, mas o primeiro conjunto sozinho **não é o universo transitivo completo**. O predicado usa colaborador, motivo, dia, instância, `conversa=False` e exclusão de `VIAS_FORA_DO_FATO`; BATIDA com episódio também inclui acervo sem episódio (`:2717–2725`).
- Para fixar a identidade que a porta efetivamente julga, existem `app/chamados/models.py::PerguntaDisputa.batida_origem_do_nascimento` (`:3452`) e `app/chamados/services/fio.py::dia_do_turno_da_linha` (`:1608–1650`), mais `::recusa_destino_de_outro_colaborador` (`:1572–1588`) e `::recusa_batida_retratada` (`:1591–1605`), todos sem escrita persistente. O próprio nascimento monta uma instância **não salva** com instância/episódio e consulta o dia antes de escrever (`app/chamados/models.py::PerguntaDisputa._nascer`, `:2866–2878`).
- `app/chamados/services/declaracao_texto.py::_candidata_reusavel` (`:230–254`) não substitui a eleição global: primeiro elege no universo e depois exige `can.disputa_id == disputa.pk and _reusavel(can)` (`:254`), podendo devolver `None` mesmo havendo dona estrangeira.

Para **“a porta aceitaria?”**, há respostas parciais já puras, de domínios distintos:

| Domínio | Leitura existente e limite |
|---|---|
| Declaração: recusa pelo carimbo | `declaracao_texto.py::_resposta_validada` (`:216–227`), `::_segura_da_lista` (`:257–275`) e `::_porta_recusaria_por_carimbo` (`:278–285`). A última responde exatamente `alvo is not None and _resposta_validada(alvo)`. Não certifica ausência de outras recusas do nascimento. |
| Declaração por marco | `declaracao_texto.py::projecao_por_marco` (`:427–479`), documentada sem escrita em `:449`; devolve `aceita = not _porta_recusaria_por_carimbo(linhas)` em `:477–478`. Atenção: obtém `linhas` por `do_fato` em `:474`, não pelo fecho `universo_do_fato`. |
| Pai Chamado: célula absolveria | `models.py::ChamadoColaborador.celula_absolveria` (`:1118–1123`) resolve célula e chama `::_celula_que_absolve` (`:1103–1115`) sobre instância não gravada. A porta real consulta esse predicado em `::abrir`, `:962–965`. |
| Pai Chamado: veto da régua | `models.py::ChamadoColaborador.veto_no_nascimento` (`:1148–1152`) chama `::_veto_da_regua` (`:1126–1145`), sem escrita; porta usa em `::abrir`, `:967–970`. |
| Texto/dia/destino da pergunta | `disputa_emissao.py::_resolver_pergunta_por_motivo` (`:300–388`) é leitura sem save, mas mistura resolução e dedup local e não incorpora catálogo, dedup inicial, hora sem dia e conversa repetida de `::gerar_perguntas_disputa` (`:405–451`). |

`declaracao_texto.py::propostas_do_chamado` também usa a guarda de carimbo (`:633–634`), mas sem ata chama `acender_sem_ata` (`:618`); portanto sua leitura pode produzir o warning, ainda que não grave DS/P. (`:594–638`)

**Não existe hoje uma leitura única que certifique o nascimento integral da DS/P ou todo o reuso/rechaveamento.** O aceite final ainda inclui as guardas internas de `_nascer`, salvamento, lavra/costura e contrato de saída (`models.py::PerguntaDisputa._nascer`, `:2888–2890`, `:2909–2926`, `:2940–3008`, `:3014–3019`; `::_sob_contrato`, `:2819–2822`). O `dry` existente **não é leitura pura**: `fio.py::rechavear_no_reuso(dry=True)` chama a porta real dentro de atomic e provoca rollback (`:1077–1081`); a porta salva e emite efeitos em `::rechavear_data_evento`, `:1037–1048`.

A ordem exigida é **Colaborador → todas as Disputas → todos os Chamados → todas as Perguntas**, cada grupo por PK crescente, antecipando o universo da transação. Só `models.py::PerguntaDisputa.trava_na_ordem` adquire essas travas (`:2629–2694`; aquisições `:2666`, `:2675`, `:2688`, `:2692`). Com vários colaboradores, todos os C por PK vêm primeiro (`:2654–2666`). O universo automático adiciona **DS abertas e pais delas + Chamados vivos**, não todos os destinos terminais de DS fechadas (`:2677–2683`); estes precisam ser fornecidos explicitamente.

Há um molde existente em `declaracao_texto.py::_trava_do_ato`: trava C, lê universo, depois fornece DS abertas/fechadas, destinos, chamado da declaração e perguntas (`:288–303`). Para o lote completo, deve-se unir esses conjuntos antes de adquirir D/Ch/P; fazer a sequência separada por item pode introduzir nova linha de tabela anterior depois de P. (`models.py::PerguntaDisputa.trava_na_ordem`, `:2631–2647`; reuso que solicita destinos e pergunta em `::_nascer`, `:2947–2950`)

Selos lidos, execução [nao verificado]:

- `test_e1_v13.py::OpusUniversoUnicoTest.test_RED_abrir_alfa_segura_a_canonica_do_fecho` (`:88`) e `::test_RED_candidata_reusavel_segura_a_canonica_do_fecho` (`:98`); um componente em `::PortaUmComponenteTest.test_RED_porta_sem_episodio_e_um_componente` (`:346`).
- `test_e1_identidade_corrida.py::CorridaDoNascerTest.test_MORDE_duas_na_janela_nascem_UMA_pergunta` (`:74`) e `::test_MORDE_a_segunda_recebe_criada_False` (`:81`).
- `test_e1_v5_ordem_trava.py::OrdemDeTravaTest`: analisar (`:100`), nascer costurando (`:107`), reapontar (`:115`), rechavear (`:124`), mutante invertido (`:133`).
- `test_e1_v15f.py::OrdemDeTravaAbrirTest.test_RED_reuso_com_rechave_e_costura_trava_na_ordem` (`:117–131`) registra **linhas**, exige Ch/P efetivamente travados e zero violações.
- `test_e1_v9.py::OrdemDoSinalTest.test_MORDE_o_rastreador_ve_a_segunda_linha_da_mesma_tabela` (`:326`) e `::test_MORDE_o_rastreador_ve_o_update` (`:336`) cobrem a limitação do observador só por tabela.
- `test_e1_v7_catracas.py::CatracaTravaTest.test_zero_trava_fora_de_trava_na_ordem` (`:387`) e anti-vacuidade (`:392`); `::test_zero_canonica_fora_do_universo` (`:447`).
- `test_bo_disputa_volta2_memoria.py::TravasVolta2Test.test_leitura_so_depois_de_c_e_dona_fechada_antes_de_ch` (`:149`) e `test_bo_disputa_volta3_memoria.py::Volta3Test.test_recuperacao_trava_fechada_adicional_antes_de_ch_com_gerador_real` (`:266`) sustentam ordem em doubles, não SQL real.

## 10/11 19:10 · sessao 64fe6e1e1d2c · - · - · origem=Ronald (tmux)

Leitura apenas; nenhuma execução de testes ou acesso a banco/log. Execução efetiva dos efeitos descritos: [nao verificado]. [nao verificado]

**4. Save da dona FECHADA**

`app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa` seleciona a dona explícita sem filtro de abertura (`:578`), une motivos (`:585-591`), funde contexto (`:592-612`) e chama `save(update_fields=campos)` em `:614`. O núcleo não escreve reabertura nesse ramo (`:578-630`).

Há exatamente três receivers diretos de `DisputaSupervisao` em `app/chamados/signals.py`, declarados em `:18`, `:120`, `:147`. Nenhum testa `update_fields` nem mudança de `fechada_em`.

| Função | Guarda literal de saída | Efeitos possíveis |
|---|---|---|
| `signals.py::disputa_fechada_fecha_chamado_pai` (`:19`) | `if created or not instance.fechada_em or not instance.chamado_id:` (`:28`); depois `if not ch or ch.status_local in ENCERRADOS:` (`:33`) | Busca pai (`:32`), abre atomic (`:41`), chama `ch.reconciliar()` (`:42`); se True, insere Followup pela porta de nota (`:45`). Erros são capturados/logados (`:46-50`). |
| `signals.py::retoma_regularizacoes_ao_fechar_disputa` (`:121`) | `if raw or created or not instance.fechada_em:` (`:129`) | Sempre registra callback `transaction.on_commit(_retomar)` quando passa na guarda (`:144`). Callback escolhe `instance.fechada_por or get_user_sistema()` (`:136`) e chama `retomar_regularizacoes_confirmadas(instance.colaborador, user)` (`:137`); erros são capturados/logados (`:141-142`). |
| `signals.py::materializa_perguntas_ao_fechar_disputa` (`:148`) | `if raw or created or not instance.fechada_em:` (`:153`) | Chama sincronamente `materializar_pendentes_aprovadas(instance, instance.fechada_por)` (`:163`); erros são capturados/logados (`:164-165`). |

**Nenhum dos três é no-op provado somente por ser um save de motivos/contexto numa DS fechada.** O primeiro pode parar se não houver pai ou ele já estiver encerrado; os dois outros passam sua guarda no save normal de instância existente/fechada. O segundo registra callback mesmo que depois não encontre regularização; o terceiro consulta pendentes mesmo que não encontre nenhuma. (`signals.py:28`, `:33`, `:129`, `:144`, `:153`, `:163`; `materializacao.py:1418`; `regularizacao_ext.py:218`)

Detalhes das escritas delegadas:

- `app/chamados/models.py::ChamadoColaborador.reconciliar`: decisão com `registrar=True` em `:651`; se permitida, `_gravar_estado` em `:654`. `_decisao_do_fechamento` pode aplicar plano de reata e registrar dedup (`:682-692`). `_gravar_estado` grava `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`, urgência e eventualmente `urgencia_original`; registra evento de estado e salva (`:228-253`, `:292-296`). Pode relavrar perguntas apontadas ao pai (`:303-310`). Followup é `FollowupChamado.objects.create` em `app/chamados/services/nota.py::anexar_ocorrencia:103` (sem objeto_ref/chave nesse caller).
- O save do pai pode acionar os receivers de **Chamado**, não diretamente pelo save DS: rebaixamento de urgência (`signals.py:54-72`), S143 (`:91-115`), cartório (`:178-216`), silêncio de isentos (`:283-306`) e filho morto acordando pai (`:350-365`). O conjunto efetivo depende do estado e acervo: [nao verificado].
- `app/chamados/services/regularizacao_ext.py::retomar_regularizacoes_confirmadas` filtra `status='confirmada', materializada_em__isnull=True` (`:218-219`), pelo colaborador (`:220-221`), e chama materializador (`:224`). `::materializar_regularizacao_externa` ainda recusa se houver qualquer DS aberta (`:58-65`). Pode criar Batida pelo `registrar_batida` (`:76-82`), inserir auditoria (`:90-102`), remover Batida e salvar item `batida_alvo/aplicada_em` (`:131-139`), corrigir `tipo/timestamp` e item `aplicada_em` (`:142-164`), salvar referências `batida_entrada/batida_saida` (`:166-188`), gravar RegularizacaoExterna `status/materializada_em/materializada_por` (`:197-200`), anexar nota e encerrar seu chamado (`:201-205`). Esses são efeitos possíveis do callback, não prova de que ocorram neste caso.
- `app/chamados/services/materializacao.py::materializar_pendentes_aprovadas` consulta P da DS com `materializada_em__isnull=True` (`:1418-1419`), pula `pergunta_em_voo(p.pk)` (`:1425-1426`), pula `not _pergunta_materializada_de_fato(p)` (`:1427-1428`), chama `p.carimbar(VIA_JUIZ_APROVOU, user, quando=agora)` (`:1431`). A porta não fabrica Batida nesse caminho (`:1408-1411`).
- `app/chamados/models.py::PerguntaDisputa.carimbar` pode gravar `materializada_em/materializada_por/via_resolucao` (`:3128-3129`, `:3152`); aqui não recebe `resposta` nem `lavrar_veredito`, portanto não solicita os campos condicionais de resposta/validação/lavra (`:3130-3137`, `:3149-3151`). Recusa via fora do fato (`:3120-3121`) ou pergunta ainda viva pelo C6 (`:3139-3145`). Save de P registra callback para fechar beco (`signals.py::_becos_post_save_pergunta:227-238`) e, pelos campos que acordam, pode emitir evento de célula (`signals.py::_outbox_post_save_pergunta:251-257`). Efeitos finais dessas cascatas: [nao verificado].

**Push do wrapper** [nao verificado]

`app/chamados/services/disputa_emissao.py::criar_ou_atualizar_disputa`:

1. Se `not motivos`, retorna `None` antes do núcleo (`:761-762`).
2. Se núcleo retorna `nascida=True`, chama push com todos os motivos e `nova=True` (`:774-776`), sem conferir pergunta local. [nao verificado]
3. Se `nascida=False`, primeiro executa `perguntar_entradas_pendentes` (`:788`), depois chama push se `novos` (`:789-790`), sem conferir fechamento/pergunta local. [nao verificado]
4. Recuperação retorna a primeira DS processada e união dos motivos novos (`::_abrir_ou_agregar_disputa:704-707`). Portanto a DS passada ao push pode ser fechada, e os novos podem ter sido agregados a várias donas (`:578`, `:697-707`, `:789-790`). [nao verificado]
5. A guarda de visitadas da volta4 permite concluir agregação recebendo pergunta estrangeira já visitada, pois a condição exige também `pergunta.disputa_id not in _donas_visitadas` (`:619-621`); o núcleo então pode retornar a DS local sem ter acrescentado pergunta local (`:630`).

`::_enviar_push_disputa_colab` só tem a guarda `_PUSH_SILENCIADO.get()` (`:1856-1859`). Não confere `fechada_em`, existência de P nem dona. Envia WebPush para `disputa.colaborador` com URL genérica `/meu-atendimento/` (`:1870-1875`), depois tenta FCM (`:1877-1880`); erros são capturados (`:1881-1882`). Entrega efetiva: [nao verificado].

**5. Declaração — sequência e todas as saídas após nascimento**

Os dois consumidores de `disputa_para_declaracao` neste módulo são `app/chamados/services/declaracao_texto.py::declarar_hora_colab` e `::aplicar_declaracao` (`:318`, `:360`; `:647`, `:659`).

O helper `app/chamados/services/disputa_emissao.py::disputa_para_declaracao` chama núcleo com `emitir_perguntas=False`, `merge_contexto=None`, urgência normal e contexto de origem (`:731-735`), e retorna DS (`:736`). Assim, ele pode criar DS/pai antes de o consumidor abrir P.

`declaracao_texto.py::_candidata_reusavel` elege canônica do universo inteiro (`:250-253`), mas só devolve se `can.disputa_id == disputa.pk and _reusavel(can)` (`:254`). **Essa conferência ocorre antes de `abrir`; não há conferência equivalente da dona da P devolvida por `abrir`** (`:376-421`, `:666-695`).

`::declarar_hora_colab`:

- Valida módulo/hora/motivo/dia/ata/casamento antes de criar DS (`:320-344`). Retornos `nao_furo`, `hora_invalida`, `motivo_invalido`, `sem_data`, `indeterminado`, `nao_casa` não fazem o nascimento deste caminho (`:321`, `:323`, `:329`, `:333`, `:339`, `:343`).
- Atomic externo (`:348`) → `_trava_do_ato` (`:352`) → DS pelo helper (`:360-362`) → candidata local (`:363`) → eventualmente reata candidata órfã (`:364-372`) → se nenhuma, `PerguntaDisputa.abrir(disputa=d, ..., chamado_destino=ch, data_evento=...)` (`:373-377`).
- Após `abrir`, todos estes retornos normais podem confirmar uma DS recém-nascida sem P sua, **se `abrir` devolveu P estrangeira e não surgiu outra P local por cascata**:
  - `_resposta_validada(perg)` → `validada_sem_registro` (`:378`, `:412-416`);
  - `perg.respondida_em and (perg.resposta_colab or '') == hora` → `ja_declarada` (`:417-418`);
  - `responder_pergunta_disputa_core` devolve `not ok` → `resposta_invalida` (`:419-421`);
  - resposta aceita, nota anexada, fim do atomic → `declarada` (`:422-426`).
- Não há rollback explícito nesses returns. Se exceção propaga, é outra saída e o atomic externo pode desfazer o nascimento (`:348-426`). Estado final de casos reais: [nao verificado].

`::aplicar_declaracao`:

- `not props` → `sem_proposta` antes de nascer DS (`:649-651`).
- Atomic (`:654`) → `_trava_do_ato` (`:656`) → DS pelo helper (`:659-661`) → candidata local (`:662`) → `abrir` se nenhuma (`:663-667`).
- Saídas normais que podem deixar DS recém-nascida sem P sua, sob a mesma condição de retorno estrangeiro:
  - `_resposta_validada(perg)` → `validada_sem_registro` (`:668`, `:683-686`);
  - pergunta sem resposta e core devolve `not _ok` → `resposta_invalida` (`:687-695`);
  - pergunta já respondida, ou resposta aceita → sai do atomic e chama `validar_pergunta(..., disputa=d)` (`:696`), retorna seu resultado (`:697`).
- Aqui existe **conferência tardia da dona** no validador: `app/chamados/services/validacao.py::validar_pergunta` inclui `filtro['disputa'] = disputa` (`:385-387`), busca (`:389`) e, se P estrangeira não pertence a `d`, retorna normalmente `nao_encontrada` (`:390-392`). Esse retorno vem **depois do atomic de nascimento já concluído**, portanto não desfaz DS/pai local nem eventual resposta escrita na P estrangeira (`declaracao_texto.py:654-697`).
- Não é correto dizer simplesmente “não há nenhuma conferência de dona”: há a conferência anterior em `_candidata_reusavel` para ambos e a conferência posterior do administrador em `validar_pergunta`; falta conferir o retorno de `abrir` antes de prosseguir/confirmar o nascimento (`declaracao_texto.py:254`, `:376`, `:666`, `:696`; `validacao.py:385-392`).

Nenhuma dessas composições com canônica estrangeira foi executada aqui; persistido efetivo, demais efeitos de sinais e existência desse estado em produção: [nao verificado].

## 10/11 19:09 · sessao efb9acd35f45 · - · - · origem=Ronald (tmux)

1. Os dois leitores perguntam a `geofence.tem_gps`, cujo corpo é `bool(latitude and longitude)`; o modal exige também `modulo_origem == "gps_fora_geofence"` (`app/core/templatetags/hasner_filters.py:38`, `app/ponto/services/geofence.py:145`, `app/templates/chamados/partials/modal_fio.html:481`, `app/templates/chamados/partials/_relatorio_geo.html:31`).

Assumindo `x` e `y` numéricos diferentes de zero, a tabela vale para ambos, com a condição adicional do módulo no modal (`app/ponto/services/geofence.py:145`, `app/templates/chamados/partials/modal_fio.html:481`):

| Par | Agora | Base, guarda apenas latitude |
|---|---|---|
| `0.0, 0.0` | Não | Não |
| `None, None` | Não | Não |
| `x, None` | Não | Sim |
| `None, y` | Não | Não |
| `x, y` | Sim | Sim |
| `'0', '0'` | Sim | Sim |

A coluna atual foi confirmada executando em memória a função extraída por AST; a base decorre do `diff_obra_v8.patch` fornecido, com guardas anteriores em `app/templates/chamados/partials/_relatorio_geo.html:30` e `app/templates/chamados/partials/modal_fio.html:480`; checkout da base indisponível [nao verificado].

Chave ausente deve produzir condição falsa: latitude ausente pode chegar como `None` ao filtro; longitude ausente, por ser argumento do filtro, pode gerar `VariableDoesNotExist`, tratado pelo `{% if %}`. Render efetivo dessa situação não foi executado porque Django não está instalado no `python3` disponível [nao verificado].

2. O filtro não tem lógica própria, conversão ou tratamento de exceção: importa o módulo no momento da chamada e devolve seu resultado (`app/core/templatetags/hasner_filters.py:38`).

Os dois partials carregam sua própria biblioteca, antes do uso; nenhum contém `extends` (`app/templates/chamados/partials/_relatorio_geo.html:1`, `app/templates/chamados/partials/modal_fio.html:6`). A página de detalhe tem `extends` como primeira tag e inclui o relatório passando `chamado`; o modal também passa `chamado`. Ambos os includes preservam contexto, sem `only` (`app/templates/chamados/detalhe_local.html:1`, `app/templates/chamados/detalhe_local.html:278`, `app/templates/chamados/partials/modal_fio.html:498`).

As entradas diretas do modal renderizam o próprio partial; portanto encontram seu `load` local (`app/chamados/views.py:927`, `app/chamados/views.py:1399`, `app/chamados/services/fio.py:496`). O ensaio acrescenta um include sintético dentro de uma página que estende a casca (`app/ponto/tests/test_bo0611_05_templates_geo.py:105`). Não há include `only` desses partials no código localizado; funcionamento hipotético com contexto isolado não foi executado [nao verificado].

3. **`str`, `dict` e lista comuns não fazem este juiz levantar por tipo.** Vazios são falsos; não vazios são verdadeiros. O ensaio em memória confirmou inclusive `('0','0')`, dicionários preenchidos e listas preenchidas como `True` (`app/ponto/services/geofence.py:145`). Se o próprio `contexto_json` tiver tipo que não oferece aquelas chaves, aplica-se o caso de variável ausente; render não executado [nao verificado].

Se o juiz for forçado a levantar outra exceção, há uma possível assimetria relevante: o relatório usa filtro como condição isolada, enquanto o modal o coloca no operador `and` (`app/templates/chamados/partials/_relatorio_geo.html:31`, `app/templates/chamados/partials/modal_fio.html:481`). Pelo comportamento conhecido de `django.template.smartif`, o `and` captura exceções e produz falso, enquanto uma condição isolada pode propagá-las; fonte do Django e execução não disponíveis nesta cópia [nao verificado]. A base não chamava o juiz nessas condições, conforme o diff fornecido; não haveria essa nova origem de exceção, mas render da base não foi executado [nao verificado].

4. Nenhuma leitura direta de `batida_lat/batida_lon` localizada nesses dois templates escapa da guarda: texto e URL do relatório ficam entre `if` e `endif`; atributos `data-blat/data-blon` ficam no bloco externo do modal (`app/templates/chamados/partials/_relatorio_geo.html:31`, `app/templates/chamados/partials/_relatorio_geo.html:36`, `app/templates/chamados/partials/modal_fio.html:481`, `app/templates/chamados/partials/modal_fio.html:485`, `app/templates/chamados/partials/modal_fio.html:543`). O relatório incluído no modal também está dentro desse bloco (`app/templates/chamados/partials/modal_fio.html:498`).

O JS recebe os atributos como texto, passa-os pelo wrapper e faz `parseFloat`; sua guarda própria é `isNaN`, que aceita zero numérico (`app/static/js/geo-colab-map.js:97`, `app/static/js/geo-colab-map.js:101`, `app/static/js/geo-colab-map.js:106`, `app/static/js/geo-colab-map.js:29`, `app/static/js/geo-colab-map.js:39`). Essa regra JS não mudou na obra pelo diff fornecido; o caminho do botão passou a receber somente pares aceitos pela nova guarda do template (`app/templates/chamados/partials/modal_fio.html:481`).

## 10/11 19:09 · sessao 4bce3e235d1e · - · - · origem=Ronald (tmux)

Q5 — revisão por leitura concluída; render Django e RED real contra `7547c8367` permanecem **[nao verificado]**. Django não está instalado no Python local; não executei runner nem importei settings CI, que criariam diretórios (`app/config/settings/ci.py:26`, `:29`, `:33`). [nao verificado]

- **Os dois testes diretos mordem o zero:** `test_MORDE_relatorio` e `test_MORDE_modal` passam pela mesma `morder`, primeiro exigem mapa com coordenada real, depois ausência com `0.0/0.0`, parciais, ausência total e zero em um componente. Contra os guards antigos de nulidade, o primeiro zero produziria controles e falharia na asserção de HTML (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:200`, `:214`, `:218`, `:223`, `:227`). Correspondência exata com o commit `7547c8367`: **[nao verificado]**.

- **O teste de mutante também morde:** começa com `self.morder(sitio)` como controle; substitui apenas a guarda do sítio selecionado; exige mapa com coordenada real; renderiza zero antes do `assertRaisesRegex`; verifica que o outro sítio continua sem mapa. Na versão antiga de nulidade, já falharia por asserção no controle zero (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:231`, `:233`, `:235`, `:237`, `:238`, `:240`).

- **O teste do patch não passa com patch inerte:** exige chamada ao juiz com os valores completos e com `(0.0, 0.0)`; exige que o juiz negando retire o mapa e que o juiz de nulidade produza mapa no zero (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:243`, `:247`, `:249`, `:250`, `:251`, `:253`, `:254`). Com guarda antiga, falharia primeiro no `assert_any_call` do par completo; portanto não seria correto dizer que esse teste chega ao zero no RED antigo (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:249`).

- **Nem todo teste novo é RED por asserção no zero:** a caracterização de texto zero deve passar também no juiz anterior à volta 8; ela documenta comportamento preservado (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:79`). O teste novo do filtro, numa árvore sem `par_tem_gps`, daria `AttributeError`, não falha de asserção sobre `0.0` (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:120`, `:126`).

- **Fixture não troca zero por `None`:** aplica `float(lat)` somente após teste `lat is not None`, preservando `0.0`; longitude idem (`app/ponto/tests/test_bo0611_05_templates_geo.py:132`, `:133`). Contudo ela converte texto numérico para float e sempre inclui ambas as chaves: não cobre JSON textual original, chave ausente nem `dict/list` (`app/ponto/tests/test_bo0611_05_templates_geo.py:132`, `:133`).

- **Regex não está absorvendo falha de setup:** render e controle estão fora do `assertRaisesRegex`; dentro fica apenas `afirmar_sem_gps`, cuja primeira asserção usa a mensagem específica esperada. As asserções seguintes do relatório não usam essa mensagem (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:200`, `:203`, `:205`, `:237`, `:238`, `:252`, `:254`). Há ponto não escapado em `base.html`, mas não identifiquei outra asserção nesse bloco que produza o restante da mensagem esperada (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:238`).

- **A mudança em `TemplatesGeoTest` não acrescenta caso zero:** sua `morder` continua exercitando apenas completo e três pares com `None`; o zero foi coberto na classe nova (`app/ponto/tests/test_bo0611_05_templates_geo.py:174`, `:178`; `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:218`).

- **Mutante antigo “sem longitude” continua alcançando os quatro sítios:** `_CTX` agora é apenas o caminho; `_PAR` representa o guard real; o substituto continua sendo `latitude is not None`. O código exige ocorrência única antes da troca e os testes exigem controle completo válido e exatamente um controle para `(LATITUDE, None)` antes de esperar a falha da asserção (`app/ponto/tests/test_bo0611_05_templates_geo.py:37`, `:38`, `:40`, `:55`, `:194`, `:201`, `:204`, `:207`). Executei extração AST em memória: quatro âncoras únicas e quatro substituições efetivas. Render dos mutantes: **[nao verificado]**.

- **Mutantes volta 7 continuam estruturalmente independentes de `_CTX/_PAR`:** feeds usam guardas próprias de `SITIOS`; nulidade por sítio e patch no juiz são verificados pelo HTML; contador extrai a expressão Python por AST (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:156`, `:171`, `:261`, `:284`). Execução Django: **[nao verificado]**.

- **Execução local apenas sem Django:** `test_bo0611_05_leitores_geo.py` passou 4 testes, incluindo mutantes dos três leitores; `test_bo0611_05_vinculo_geo_parcial.py` passou 5 testes com `PYTHONPATH=app`, incluindo mutante sem longitude (`app/ponto/tests/test_bo0611_05_leitores_geo.py:133`, `:142`; `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:142`). Resultado não equivale a runner Django verde. [nao verificado]

## 10/11 19:09 · sessao d88972d64298 · - · - · origem=Ronald (tmux)

Censo e tipos:

- `app/ponto/services/geofence.py:349-350` produz o par em `_occ`; `:391-392` produz o par escalar no contexto legado. Ambos usam `float(x) if x else None`: valor numérico truthy vira `float`; zero numérico vira `None`; texto `"0"` vira `0.0`. O nascimento grava também `ocorrencias: [_occ]` (`app/ponto/services/geofence.py:375-394`).
- Ocorrência posterior entra por `anexar_ocorrencia(... contexto_novo={'ocorrencias': [_occ]})` (`app/ponto/services/geofence.py:359`), delega à porta de contexto (`app/chamados/services/nota.py:110-123`), une listas sem converter os itens e salva pela porta (`app/chamados/services/contexto.py:144-163`).
- A cópia explícita adicional **não escreve contexto_json**: lê o par do chamado e o copia sem conversão para `Advertencia.detalhe_json` (`app/chamados/services/acoes_chamado.py:193-205`).
- Escritor sintético da volta: fixture preserva zero e monta `float/None` por nulidade (`app/ponto/tests/test_bo0611_05_templates_geo.py:131-134`). Demais fixtures explícitas: `app/chamados/tests/test_s127_fluxo_geofence.py:26`, `app/chamados/tests/test_geofence_volta_no_fio.py:145`, `app/chamados/tests/test_geofence_validar_e_recusar.py:47`, `app/core/tests/test_smoke_chromium_fio_geofence.py:119`, `app/ponto/tests/test_geofence_catalogo_premissa.py:64`, `app/ponto/tests/test_jz1_autoridade_unica.py:194`, `app/ponto/tests/test_reconciliar_geofence.py:42-66`.

Leitores do par:

- Templates: guarda e atributos do modal (`app/templates/chamados/partials/modal_fio.html:481`, `:485`); guarda, texto e URL do relatório (`app/templates/chamados/partials/_relatorio_geo.html:31-33`).
- Python: cópia para advertência acima; extração de ocorrências/legado (`app/ponto/services/geofence.py:403-406`); reconciliação filtra por `is not None` e usa distância/classificador (`app/ponto/services/geofence.py:425-432`).
- JS indireto do modal: wrapper transfere os atributos (`app/static/js/geo-colab-map.js:97-106`), `parseFloat` lê as duas strings (`:29`) e `isNaN` decide ausência (`:39`). A nova guarda impede o botão para zero numérico; o JS mantém regra própria que aceitaria zero se recebido (`app/templates/chamados/partials/modal_fio.html:481-485`, `app/static/js/geo-colab-map.js:39`).
- A guarda própria da reconciliação continua divergindo para zero; o diff da obra fornecido não altera essa expressão, apenas a extração do juiz no mesmo arquivo. Alteração histórica adicional nessa reconciliação: [nao verificado].

Classes estruturais e proibidos:

- Os dois filtros delegam ao módulo no ato da chamada; não duplicam o predicado (`app/core/templatetags/hasner_filters.py:29-41`). Juiz registrado em `app/core/juizes.py:1201`, implementação `bool(latitude and longitude)` em `app/ponto/services/geofence.py:145`, usada pelo emissor em `:269`.
- Não há novo escritor de Batida nas mudanças fornecidas: ambas as criações ficam na porta (`app/ponto/registro_batida.py:152-159`); API encaminha o par (`app/api/views_core.py:639-654`). Geofence vem depois da gravação e sua falha é tratada (`app/api/views_core.py:766-773`).
- O juiz novo não lê relógio, escreve ou deriva outra fonte (`app/ponto/services/geofence.py:126-145`); os filtros também não (`app/core/templatetags/hasner_filters.py:29-41`). Ordem de travas/deploy efetivamente executada: [nao verificado].
- O risco de tipo permanece: PWA envia os componentes por FormData (`app/static/pwa.js:145-146`), sanitizador só valida com `float(v)`, preservando o valor original (`app/ponto/registro_batida.py:10-17`), enquanto o banco declara DecimalField (`app/ponto/models.py:58-59`). Assim, o objeto recém-criado pode conservar texto `"0"` truthy; o JSON vira zero float e o template passa a rejeitá-lo. Não há mudança de unidade nas novas linhas; a divergência de tipo é real no código, preexistente no sanitizador (`app/ponto/registro_batida.py:15`, `app/ponto/services/geofence.py:145`, `:391-392`, `app/templates/chamados/partials/modal_fio.html:481`).
- O teste novo caracteriza explicitamente texto zero como aceito, enquanto float zero é rejeitado (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:79-84`). Não é correção do tipo na porta (`app/ponto/registro_batida.py:15`).
- Outros leitores de latitude/longitude tocados pela obra mantêm nulidade própria: vínculo em três retornos (`app/colaboradores/views.py:337`, `:356`, `:395`) e detector (`app/ponto/services/vinculo_na_lista.py:49-57`). O detector inclusive tem teste exigindo zero como evidência (`app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135-140`). Esse comportamento de zero já aparece no diff/base fornecido como preexistente; incidência em produção: [nao verificado].
- Restam barreiras anteriores: selfie no cliente (`app/templates/ponto/bater_ponto_v2.html:285-290`, `:312`) e colisão de tipo oposto na porta (`app/ponto/registro_batida.py:145-151`), respondida como `ignorada_intervalo` (`app/api/views_core.py:655-677`). A incompletude da classe está declarada em `app/docs/LEIS.md:165`.
- O selo AST só detecta `Response` 403 em `If` anterior à porta que leia latitude/longitude/situação; não certifica qualquer barreira possível (`app/api/tests/test_l030_selo_porta.py:37-64`). O selo de modal tornou a exigência de `naoBloquear` condicional à presença de `hxConfirmar(` (`app/core/tests/test_selo_modal_nao_nasce_vazio.py:139-143`).

Achado documental forte:

- O registro e corte dizem que leitores restantes entram no BACKLOG (`app/core/juizes.py:1199-1200`, `app/docs/CORTES.md:442`). Na cópia, não localizei a linha específica `BO-0611-05-TEM-GPS-LEITORES`, nem a dívida de tipo texto zero no BACKLOG; há apenas a entrada genérica BO (`app/docs/BACKLOG.md:1873`). `:1884` é outra obra. Existência dessas linhas na árvore viva/commit: [nao verificado].
- O corte exige smoke nas duas cascas antes do front (`app/docs/CORTES.md:440`). Smoke, suite final, RED histórico, pouso e comparação física com `motor_calculo_v2.py` da base: [nao verificado]. Pelo diff fornecido, não há alteração de motor nem coordenada inventada; confirmação por Git independente: [nao verificado].

## 10/11 19:05 · sessao 49d9c6780845 · - · - · origem=Ronald (tmux)

Auditoria estática; não executei testes nem banco. Execução efetiva em produção e chamadas construídas dinamicamente fora da cópia: [nao verificado]. [nao verificado]

**1. `app/ponto/management/commands/reconciliar_grade.py::Command._disputa`**

- Corpo em `:412`; delega ao núcleo em `:424`, passando `motivos=[]`, `origem='grade'`, contexto `{'origem':'grade'}`; retorna DS em `:428`. Não há chamada a `_disputa` dentro de `Command.handle` nem dos demais métodos do comando. A emissão efetiva do handle usa `_emitir_furo`, em `:250–254`.
- Único chamador concreto desse método encontrado no repositório: `app/chamados/tests/test_porta_chamado_e3.py::PortasDeCampoTest.test_MORDE_a_disputa_da_grade_nasce_UMA_vez` (confirmar nome da classe caso necessário), definição `:91`, chamadas `Command()._disputa(self.colab)` em `:93` e `:94`. Exige mesmo PK, uma DS aberta, pai e prazo SLA em `:95–98`.
- Pesquisei repo inteiro, inclusive arquivos ocultos fora `.git`, por `_disputa`, referências/importações de `reconciliar_grade`, aliases dos imports encontrados e referência textual para reflexão. Não apareceu `getattr(..., '_disputa')`, apelido ou chamada produtiva desse método. A inexistência universal de reflexão arbitrária construída em execução é [nao verificado].
- Outros selos sustentam partes do método sem chamá-lo:
  - `app/chamados/tests/test_sla_pelo_juiz.py::OPrazo...test_MORDE_a_grade`, `:115–123`: inspeção de fonte exige delegação ao núcleo e ausência de `DS.objects.create`.
  - `app/chamados/tests/test_porta_chamado_e3.py::OsSitiosChamamOAto.test_cada_sitio_chama_o_seu_ato`, `:101–121`: catálogo AST inclui o arquivo com `_abrir_ou_agregar_disputa`.
  - `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py::ProvasMemoria.test_chamada_grade_motivos_vazios_conta_estrutura`, `:416–420`: chama diretamente o núcleo com lista vazia, não `Command._disputa`; espera `(I7a,I7b)=(1,0)` em doubles.

**Sim, `--apply` ainda escreve.** O texto “SEM ESCRITOR” do cabeçalho descreve o uso shadow, não a impossibilidade do ramo:

- `Command.add_arguments`: flag presente, `:67`; handle lê em `:76`.
- `Command.handle`: com `apply and not o['so_retratar']`, chama `_emitir_furo`, `:250–254`.
- `Command._emitir_furo`: `ChamadoColaborador.abrir`, `:507–520`, e `ch.reabrir()` para terminal, `:529–530`; esse caminho termina sem chamar `_disputa`, `:531–532`.
- `Command.handle`: `if apply:` chama `chamados.reconciliador.retratar`, `:305–316`.
- `Command._relavrar_orfa`: com apply, retira `orfa_regeneracao`, `:452–454`; no ramo de relavra renova `marcos_faltantes`/`orfa_regeneracao_curada` e retira a marca em atomic, `:472–474`, além da trilha `:475–485`.
- Cron configurado sem apply em `app/config/crons.py:460–462`. Selo `app/chamados/tests/test_hx_um_emissor.py::UmEmissorTest.test_reconciliar_grade_sem_apply_no_cron`, `:5–9`.
- `app/ponto/tests/test_reconciliar_grade_sombra.py::ReconciliarGradeNaoEscreveTest.test_GOLDEN_sem_apply_nao_escreve_NADA`, `:58–63`, só cobre sem apply; anti-vacuidade/placar em `:65–88`.

**2. Agrupador de órfãos**

Função: `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos`, definição `:1130`.

**(i) Nascimento em relação às perguntas**

- Filtro inicial de lista do admin em `:1150–1152`; atomic `:1155`; trava do repasse `:1162`.
- Há complemento de perguntas em DS **preexistentes**, antes do filtro/nascimento, `:1168–1188`.
- Filtro dos chamados a converter em `:1189–1197`; seleção de DS aberta em `:1199–1201`.
- Se nenhuma DS aberta, abre pai em `:1207–1222`, anexa ocorrência se reutilizado em `:1223–1225`, cria DS em **`:1226–1231`** com colaborador, motivos, contexto `manual_orfaos`/IDs e pai.
- Os laços locais que perguntam vêm **depois**: intervalo `:1241–1264`; ata `:1269–1281`; episódio `:1296–1326`; sem origem `:1334–1355`; pendentes `:1373`.
- Portanto a DS local é criada antes de saber se haverá pergunta sua; o complemento inicial em outra DS não muda essa ordem. (`:1184`, `:1226`, `:1256`)

**(ii) Ramo zero perguntas**

- Não testa SQL “zero perguntas”; usa `disputa.pode_fechar()` em `:1381`.
- Se absteve por ata e poderia fechar: acende `acender_sem_ata('auto_fecho_disputa',...)`, `:1382–1384`, preservando aberta.
- Caso contrário, se pode fechar: chama `disputa.fechar(user_sistema, resolucao=...)` em `:1385–1390`, retorna em `:1391`, antes do push `:1392–1393`. [nao verificado]
- `app/chamados/models.py::DisputaSupervisao.pode_fechar`, `:1848–1853`: ausência de respostas sem veredito e de fio vivo.
- `::fechar`: com zero linhas `perguntas`, deriva `motivo=FECHOU_VAZIA`, `:1895–1903`; `'vazia'` definido em `:1781`.
- Escritas diretas da DS: `fechada_em=now`, `fechada_por=user`, `resolucao=texto`, `motivo_fechamento='vazia'`; save desses quatro campos, `:1932–1936`; evento `disputa_fechada`, `:1937–1940`. Não apaga DS/pai nem zera seus IDs/contexto/motivos.
- O ramo de fecho **não altera diretamente os chamados órfãos**. Nos laços anteriores, `analisar()` só é chamado quando helpers retornaram criação/complemento: `:1261–1262`, `:1280–1281`, `:1323–1326`, `:1354–1355`. Dia completo faz `continue` antes disso em `:1304–1305`/`:1341–1342`.
- Atenção: **zero perguntas locais não prova que nenhum órfão foi analisado**, porque o helper ignora a dona retornada e pode devolver `_criou=True` para reuso estrangeiro (`:940–950`).
- Pai passa pelo receiver de DS fechada, `app/chamados/signals.py::disputa_fechada_fecha_chamado_pai`, condição `:28`, `ch.reconciliar()` `:42`, nota `:45`; efeitos completos das cascatas dependem do acervo [nao verificado].
- Testes que caracterizam vazio fechado: `app/chamados/tests/test_hx_disputa_muda_sem_ata.py::DisputaMudaSemAtaTest.test_com_ata_e_dia_completo_fecha_como_antes`, `:63–79`; `app/chamados/tests/test_hx_disputa_muda.py::...test_sem_nada_a_perguntar_nao_fica_aberta`, `:24–33`; `::test_nao_notifica_disputa_muda`, `:44–50`.

**(iii) TODOS leitores de `'vazia'` encontrados**

Pesquisa repo-wide por `motivo_fechamento`, `FECHOU_VAZIA`, `disputa_vazia`:

1. **Único consumidor funcional específico**: `app/chamados/services/vigia_fio.py::disputa_vazia`, `:727–734`. Conta DS com `motivo_fechamento=DisputaSupervisao.FECHOU_VAZIA`. Sem filtro de abertura/data/empresa e sem recontar perguntas. Não seleciona nem marca órfãos. Não encontrei chamador produtivo desse contador; referências executáveis encontradas são testes.
2. `app/chamados/tests/test_c7_red_minimos.py::C7_04_DisputaVaziaTest.test_RED_vazia_fecha_com_motivo_vazia_e_o_contador_conta`, `:174–183`: exige motivo `'vazia'` e delta 1 do contador.
3. `::test_RED_o_contador_MORDE_disputa_com_pergunta_nao_conta`, `:185–192`: DS com pergunta fecha sem incrementar contador.
4. `app/chamados/tests/test_c7_red_minimos.py::...test_MORDE_o_update_em_lote`, `:613–616`: fonte artificial `filter(motivo_fechamento='vazia').count()`, lida pelo detector AST para provar que filter não é escrita; não executa consulta ORM.
5. Leitores **genéricos**, sem decisão específica por `'vazia'`: `app/chamados/models.py::DisputaSupervisao.fechar`, `:1879–1883`, compara o motivo recebido para idempotência; `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle`, `:128–138`, serializa campo em arquivo de reversão. Não funcionam como bloqueio de reagrupamento.
6. Registro em `bin/sombra_regras_pessoais.py:177` classifica o campo como código choices; não é consumidor decisório de `'vazia'`.

**(iv) Chamadores/filtros e idempotência**

Se não nascesse a DS vazia, **não há marcador externo de “este conjunto já foi tentado” no agrupador**. Os IDs são guardados no contexto da DS/pai (`:1219`, `:1229`), mas o filtro de entrada não consulta esses IDs nem `motivo_fechamento`. O filtro usa regularizabilidade e pergunta existente por destino (`:1189–1194`); a regularizabilidade vem de `motivo_fora_da_fabrica`, `:850–899`, com status/categoria/célula/pergunta materializada/turno resolvido. O dedup local `_ja_perguntou` consulta motivo/dia/conversa dentro da própria DS, `:1518–1525`.

Se os chamados continuam elegíveis, sem pergunta que os exclua e são novamente fornecidos pelo chamador, podem voltar; o que ocorrerá na próxima execução real é [nao verificado]. A própria DS fechada vazia **também não é consultada como selo de tentativa** (`:1199–1201` só procura abertas).

Seis chamadores produtivos diretos:

- `app/chamados/services/acoes_disputa.py::acao_iniciar_disputa_orfaos`, `:210–224`: todos chamados do colaborador (`:218`) filtrados por `chamado_orfao_e_regularizavel` (`:219`), passa request_user.
- `app/chamados/services/pergunta_no_app.py::perguntar`, `:197–217`: só itens com causa `'fabrica_nao_rodou'` (`:206–208`), busca PKs e agrupa por colaborador (`:210–212`), `notificar` recebido, default False. Universo anterior de `::medir`: status COBRANCA, módulos CALADOS_POR_ISENCAO, colaborador ativo em `:144–153`; `::retencao` decide causa em `:90–119`.
- `app/chamados/services/cobrar_dia.py::cobrar_dia`, `:45–49`: depois do julgamento, lê `do_dia`; somente IDs cuja causa é `'fabrica_nao_rodou'`; notificar True.
- `app/ponto/management/commands/detectar_ausencias.py::Command._abrir_chamado`, `:796–802`: todos chamados do colab em COBRANCA e regularizáveis; best-effort `:790–806`.
- `app/ponto/management/commands/processar_alertas_turno.py::Command._abrir_chamado_critico`, `:372–385`: só quando `criado`; COBRANCA + regularizáveis; best-effort `:373–389`.
- `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle`, `:153–159`: no ramo de criação do alerta, COBRANCA + regularizáveis; best-effort `:147–163`.

Reexport de compatibilidade do agrupador em `app/chamados/models.py:4093`; não é outro implementador.

**(v) Retorno estrangeiro ignorado**

`app/chamados/services/disputa_emissao.py::_criar_pergunta_se_couber`:

- Acrescenta motivo **antes** dos vetos, `:931–932`.
- Ausência cobre: retorna `(motivos,False)`, `:933–934`; já perguntou local: idem `:935–936`.
- Chama `PerguntaDisputa.abrir(...)` como expressão, sem atribuir/inspecionar `(pergunta, criada)`, `:940–949`.
- Retorna sempre `(motivos, True)` depois da chamada, `:950`.
- `app/chamados/models.py::PerguntaDisputa._nascer` elege canônica do universo global do fato em `:2905–2921`, sem exigir a DS recebida; retorna `ex, False, dia` em `:3008`, preservando dona. Logo esse True não prova pergunta nova nem pergunta local.
- `::_complementar_marcos_faltantes`, `:984–986`, apenas acumula esse booleano. O agrupador pode analisar o órfão e mais tarde fechar a DS local zero perguntas, sem encaminhar motivos à dona efetiva (`:1323–1326`, `:1385–1391`).

## 10/11 19:05 · sessao b5dd34352e46 · BO-0611-05 · construtor · origem=Code

Cura e testes escritos; comandos, alterações e pendências na [entrega](/home/ronald/wt-codex-bo-0611-05-185609/app/ponto/tests/ENTREGA_BO_0611_05_VOLTA8.txt:9). [nao verificado]

Sintaxe e mutante isolado do filtro passaram ([resultado](/home/ronald/wt-codex-bo-0611-05-185609/app/ponto/tests/ENTREGA_BO_0611_05_VOLTA8.txt:125)). RED/GREEN e mutantes de render seguem **[nao verificado]** por falta de Django; certificação permanece pendente ([FALTA](/home/ronald/wt-codex-bo-0611-05-185609/app/ponto/tests/ENTREGA_BO_0611_05_VOLTA8.txt:147)).

## 10/11 18:59 · sessao 30c755e4344e · BO-DISPUTA-SEM-PERGUNTA · leitura · origem=Code

**A cópia protege o nascimento no núcleo contra exceção do gerador e tenta encaminhar o motivo à dona da canônica, mas ainda permite disputa persistida sem pergunta.** Há caminhos por recusa silenciosa do gerador, grade, órfãos e declaração; a recuperação também continua recursiva. (`app/chamados/services/disputa_emissao.py:435`, `:643`, `:650`, `:690`, `:726`, `:1217`; `app/ponto/management/commands/reconciliar_grade.py:424`)

A alternativa **cabe sem alterar `chamados/models.py` para retirar exceção de controle e reentrada**; exige planejar pela identidade completa da porta, sob trava, e conservar esse fato durante a escrita. Ausência absoluta de exceções não cabe nos contratos atuais da pergunta. (`app/chamados/models.py:2866`, `:2875`, `:2907`, `:2954`; `app/chamados/services/fio.py:1784`, `:1225`)

Confirmação do hash por Git, execução dos testes, SQL efetivo e estado de produção: **[nao verificado]**. As provas abaixo são cobertura **lida**, não certificação de execução. [nao verificado]

Para compactar as referências, `models.py` e `signals.py` abaixo são os de `app/chamados`; `disputa_emissao.py`, `fio.py`, `declaracao_texto.py` e `fabrica_da_celula.py` ficam em `app/chamados/services`; os testes citados ficam em `app/chamados/tests`. Arquivos principais: [emissor](/home/ronald/codex_ws/5bdf48b9a2115875419e75de9a5614c8bb43a787/app/chamados/services/disputa_emissao.py:495), [porta da pergunta](/home/ronald/codex_ws/5bdf48b9a2115875419e75de9a5614c8bb43a787/app/chamados/models.py:2629), [fio](/home/ronald/codex_ws/5bdf48b9a2115875419e75de9a5614c8bb43a787/app/chamados/services/fio.py:1784).

**Parte 1 — matriz da porta.**

DS = `DisputaSupervisao`; P = `PerguntaDisputa`; Ch = `ChamadoColaborador`. No núcleo, o nascimento grava DS com `colaborador`, `motivos`, `contexto` e `chamado`; o pai recebe categoria/módulo, textos, classificação, urgência, contexto e SLA. Na agregação, grava união de motivos e, havendo merge, contexto com `qtd`/`ultima_em` por motivo. (`disputa_emissao.py:578`, `:590`, `:599`, `:624`, `:643`)

P nova recebe dona, motivo, texto/tipo/opções, destino, célula, discriminador conversa e dia; colaborador, instância e episódio seguem a porta de identidade. P reaproveitada conserva sua dona e pode sofrer rechaveamento, costura e nova lavra. (`disputa_emissao.py:452`, `:457`; `models.py:2866`, `:2940`, `:2965`, `:3003`, `:3008`, `:3014`, `:3448`)

| Estado | Juiz — quem decide | Operação, em ordem | Persistido ao fim | Retorno ao chamador | Prova | “Nasce com pergunta ou não nasce” |
|---|---|---|---|---|---|---|
| **a1. Fato novo, emissão local aceita.** (`disputa_emissao.py:650`) | Resolução de texto/dia/destino no emissor; identidade e nascimento em `PerguntaDisputa.abrir`/`abrir_conversa`. (`disputa_emissao.py:434`, `:452`, `:457`; `models.py:2907`) | Atomic → trava → seleciona aberta; agrega nela ou abre pai e DS → gera P → retorna. (`disputa_emissao.py:547`, `:566`, `:571`, `:609`, `:624`, `:650`) | **DS:** nova ou motivos/contexto agregados. **P:** local. **Ch:** pai novo/reutilizado; ocorrência no reuso. **Contexto:** nascimento ou merge. (`disputa_emissao.py:583`, `:604`, `:636`, `:642`, `:643`, `:650`) | Núcleo: `(DS, True, motivos)` no nascimento; `(DS, False, novos)` na agregação. Wrapper: DS. (`disputa_emissao.py:622`, `:672`, `:767`, `:782`) | `test_bo_disputa_sem_pergunta_memoria.py::test_nascimento_bom_e_agregacao_geram_uma_vez_no_atomic`, linha 387; fronteiras ORM são doubles. (`test_bo_disputa_sem_pergunta_memoria.py:73`, `:387`) | **SIM**, quando efetivamente há P local no retorno. (`disputa_emissao.py:650`; `models.py:3014`) |
| **a2. Gerador termina normalmente sem emitir nenhuma P.** Catálogo ausente, texto/dia impossível, hora sem dia ou outros `continue`. (`disputa_emissao.py:404`, `:435`, `:438`) | Guardas do gerador/helper; nenhuma guarda final exige uma P local. (`disputa_emissao.py:329`, `:364`, `:383`, `:443`, `:655`) | Cria pai/DS → gerador devolve lista vazia → não levanta → confirma nascimento. (`disputa_emissao.py:624`, `:643`, `:650`, `:655`, `:672`) | **DS:** nova, aberta, motivos/contexto recebidos. **P:** zero. **Ch:** pai persiste. **Contexto:** persiste mesmo sem pergunta. (`disputa_emissao.py:636`, `:643`, `:650`, `:672`; `models.py:1757`) | Retorno normal de nascimento; wrapper tenta push. (`disputa_emissao.py:672`, `:765`, `:766`) | **AUSENTE** para cada recusa nessa composição; o caso concreto de lista vazia está em `test_bo_disputa_sem_pergunta_memoria.py::test_chamada_grade_motivos_vazios_conta_estrutura`, linha 416. (`test_bo_disputa_sem_pergunta_memoria.py:416`) | **NAO** quando houve nascimento. (`disputa_emissao.py:643`, `:655`, `:672`) | [nao verificado]
| **b1. Canônica em outra DS aberta B, diferente da A selecionada.** Se B já for a primeira aberta, a agregação é direta. (`disputa_emissao.py:571`, `:612`) | Porta da P elege canônica; núcleo compara `pergunta.disputa_id` com A. (`models.py:2907`, `:2921`; `disputa_emissao.py:612`) | Ensaia em A → recebe P de B → levanta → desfaz ensaio → reentra explicitamente em B. (`disputa_emissao.py:607`, `:621`, `:673`, `:690`) | **DS:** nenhuma nova; B recebe motivo/contexto. A preexistente conserva estado anterior ao ensaio. **P:** continua em B. **Ch:** pai/ocorrências do ensaio não se consolidam. (`disputa_emissao.py:575`, `:583`, `:604`, `:621`, `:676`; `models.py:3008`) | Núcleo devolve B, `False`, motivos novos em B; wrapper devolve B. (`disputa_emissao.py:695`, `:698`, `:782`) | **AUSENTE** para integração do núcleo com duas abertas e porta real; a eleição da porta tem `test_e1_v13.py::test_RED_abrir_alfa_segura_a_canonica_do_fecho`, linha 88. (`test_e1_v13.py:88`) | **SIM para este nascimento:** não nasce outra DS. Não sana eventual A já vazia. (`disputa_emissao.py:621`, `:690`; `test_bo_disputa_volta3_memoria.py:252`) |
| **b2. Lote misto, com aberta A: alguns motivos pertencem a B; restantes ficam em A.** (`disputa_emissao.py:615`) | Detecção por P retornada e particionamento por dona. (`disputa_emissao.py:612`, `:616`, `:620`) | Desfaz ensaio inteiro → reentra por dona → filtra contexto de motivos; chaves globais acompanham todas. (`disputa_emissao.py:621`, `:687`, `:688`, `:690`) | **DS:** motivos reusados agregados às respectivas donas; restantes em A. **P:** perguntas locais do ensaio são desfeitas e podem nascer novamente na recuperação. **Ch:** sem pai novo pelo ensaio. **Contexto:** merge por partição. (`disputa_emissao.py:620`, `:676`, `:688`, `:694`) | Somente **primeira dona processada**, com união de `novos` das várias donas. (`disputa_emissao.py:695`, `:697`, `:698`) | `test_bo_disputa_volta3_memoria.py::test_agregacao_reuso_move_so_motivo_reusado_e_rollback_contexto`, linha 227; dona simulada fechada. (`test_bo_disputa_volta3_memoria.py:227`, `:228`) | **DEPENDE** da emissão dos restantes e do estado anterior de A; não há saneamento geral de DS vazia. (`disputa_emissao.py:609`, `:620`; `test_bo_disputa_volta3_memoria.py:252`) |
| **b3. Lote misto, sem aberta: há canônicas estrangeiras e motivos ainda sem dona.** (`disputa_emissao.py:655`) | Núcleo escolhe a primeira dona encontrada para receber também os restantes. (`disputa_emissao.py:658`, `:659`, `:660`) | Cria ensaio de pai/DS/P → detecta estrangeiras → desfaz tudo → reparte reusados; restantes seguem à primeira dona. (`disputa_emissao.py:643`, `:650`, `:661`, `:690`) | **DS:** ensaio não persiste; donas existentes recebem motivos. **P:** próprias do ensaio são desfeitas; restantes podem gerar P na primeira dona. **Ch:** pai do ensaio não persiste. **Contexto:** merge filtrado. (`disputa_emissao.py:660`, `:676`, `:688`, `:694`) | Primeira dona e união de novos; não retorna coleção de donas. (`disputa_emissao.py:695`, `:698`) | `test_bo_disputa_sem_pergunta_memoria.py::test_reuso_lote_agrega_cada_motivo_na_sua_dona_e_desfaz_pergunta_propria`, linha 343. (`test_bo_disputa_sem_pergunta_memoria.py:343`) | **SIM quanto à ausência de nova DS vazia**; não garante uma dona aberta para os restantes. (`disputa_emissao.py:575`, `:660`, `:698`) |
| **c. Canônica em DS fechada B.** (`disputa_emissao.py:575`) | Porta elege a P sem exigir DS aberta; recuperação seleciona B por PK. (`models.py:2907`, `:2921`; `disputa_emissao.py:575`) | Rollback do ensaio → agrega em B fechada → `save` de motivos/contexto → receivers → gerador. (`disputa_emissao.py:583`, `:604`, `:607`, `:609`, `:690`) | **DS:** B recebe motivos/contexto; o núcleo não escreve seu fechamento. **P:** FK permanece B; alterações adicionais dependem de dedup, costura e receivers. **Ch:** pai pode ser reconciliado. Efeitos finais adicionais: **[nao verificado]** sem esses insumos. (`disputa_emissao.py:575`, `:607`; `models.py:3008`; `signals.py:42`, `:144`, `:163`) | B fechada, `False`, novos; push continua possível. (`disputa_emissao.py:698`, `:779`, `:781`, `:1845`) | `test_bo_disputa_sem_pergunta.py::test_porta_devolve_outra_disputa_nova_nao_nasce_agrega_na_dona`, linha 68, **mocka `abrir_conversa` e push**; não prova esse reuso pela porta real. (`test_bo_disputa_sem_pergunta.py:68`, `:81`, `:82`) | **SIM para não nascer outra DS**; isso não garante ausência de efeitos sobre o agregado fechado. (`disputa_emissao.py:575`, `:607`; `signals.py:129`, `:153`) | [nao verificado]
| **d. Ciclo A↔B: cada reentrada resolve o dia cuja canônica está na outra.** (`disputa_emissao.py:335`, `:690`) | Cada chamada recalcula pelo contexto da DS corrente; compara apenas dona atual com P retornada. (`disputa_emissao.py:335`, `:434`, `:612`) | A→rollback→B→rollback→A; não há visitadas nem limite no corpo. Se o ciclo continuar, termina por erro de recursão, não por resultado de negócio. (`disputa_emissao.py:621`, `:673`, `:690`) | Sob erro propagado, escritas dos atomics da emissão são desfeitas; DS/P/Ch anteriores permanecem. Reprodução com ORM real: **[nao verificado]**. (`disputa_emissao.py:547`, `:676`, `:690`) | Sem retorno normal enquanto o ciclo persiste; wrapper não alcança seu push. (`disputa_emissao.py:690`, `:758`, `:765`) | **AUSENTE** nesta cópia para A↔B com porta ORM real; recuperação existente continua sendo a chamada de `:690`. (`disputa_emissao.py:690`) | **SIM por rollback se o erro propagar**, mas não há garantia de conclusão da operação. (`disputa_emissao.py:547`, `:676`) | [nao verificado]
| **e1. Gerador levanta depois da batida gravada, dentro do núcleo.** (`api/views_core.py:681`, `:794`) | Atomic do núcleo; captura específica somente para reuso estrangeiro. (`disputa_emissao.py:547`, `:650`, `:673`) | Batida já gravada → emissão → falha → rollback de pai/DS/P/alterações do ensaio. (`api/views_core.py:681`, `:794`; `disputa_emissao.py:547`, `:650`) | **DS/P/Ch/contexto:** sem alterações consolidadas desse ato, inclusive se P nasceu antes da falha. **Batida:** anterior à emissão, preservada nessa fronteira. (`test_bo_disputa_sem_pergunta.py:39`, `:49`, `:52`, `:55`, `:66`) | Exceção propaga; sem push do wrapper. Reemissão do mesmo fato e seu instante: **[nao verificado]**. (`disputa_emissao.py:673`, `:765`; `test_bo_disputa_sem_pergunta.py:53`) | `test_bo_disputa_sem_pergunta.py::test_gerador_levanta_desfaz_disputa_pai_e_pergunta_mas_preserva_batida`, linha 32; `::test_gerador_falha_apos_pergunta_desfaz_as_duas`, linha 55. (`test_bo_disputa_sem_pergunta.py:32`, `:55`) | **SIM nessa fronteira transacional.** (`disputa_emissao.py:547`, `:650`) |
| **e2. Falha na geração posterior ao núcleo: pendentes, fábrica ou smoke.** (`disputa_emissao.py:779`; `fabrica_da_celula.py:174`; `smoke_chamados.py:102`) | Atomic externo, se houver; esses chamados estão depois do retorno do núcleo. (`disputa_emissao.py:779`; `fabrica_da_celula.py:172`, `:174`) | Núcleo conclui → outra geração escreve/chama portas → pode falhar. Pendentes só salva seu contexto ao terminar com perguntas retornadas. (`disputa_emissao.py:294`, `:296`, `:779`) | **DEPENDE do atomic externo:** sem ele, estado já confirmado pelo núcleo não é desfeito; perguntas anteriores da geração posterior também podem permanecer. Contexto em memória e gravado podem divergir após falha. (`disputa_emissao.py:291`, `:294`, `:296`; `fabrica_da_celula.py:174`) | Pendentes pode impedir retorno/push do wrapper; fábrica propaga; smoke contabiliza erro. (`disputa_emissao.py:779`, `:781`; `fabrica_da_celula.py:174`; `smoke_chamados.py:114`) | **AUSENTE** para rollback integral dessas gerações posteriores. (`disputa_emissao.py:294`; `fabrica_da_celula.py:174`) | **DEPENDE** do estado produzido pelo núcleo e da transação externa; a proteção nova não abrange automaticamente esses trechos. (`disputa_emissao.py:547`, `:779`) | [nao verificado]
| **f1. HX sem ata: agrupador abstém e não fecha.** (`disputa_emissao.py:1265`, `:1373`) | `_marcos_faltantes` → `_ata_do_dia`; fecho por `DS.pode_fechar`, com veto da abstenção. (`declaracao_texto.py:86`; `disputa_emissao.py:990`, `:1372`) | Pai/DS nascem antes → ata responde `sem_celula` → abstém → conserva aberta → acende warning. (`disputa_emissao.py:1217`, `:1267`, `:1373`, `:1375`) | **DS:** aberta, motivos e IDs dos órfãos. **P:** zero na cena HX. **Ch:** pai persiste. **Contexto:** `manual_orfaos`/IDs; auditor publica I7b nessa cena. (`disputa_emissao.py:1220`; `test_hx_disputa_muda_sem_ata.py:58`, `:59`; `test_bo_disputa_sem_pergunta.py:95`, `:98`) | DS; com `notificar=True`, tenta push mesmo sem P. O teste HX usa `False`. (`disputa_emissao.py:1383`, `:1385`; `test_hx_disputa_muda_sem_ata.py:55`) | `test_hx_disputa_muda_sem_ata.py::test_sem_ata_nao_fecha_e_acende`, linha 51; BO incorpora a cena na linha 95. (`test_hx_disputa_muda_sem_ata.py:51`; `test_bo_disputa_sem_pergunta.py:95`) | **NAO na frase literal**, sendo a exceção HX preservada no dado e no teste. (`test_hx_disputa_muda_sem_ata.py:56`, `:58`, `:59`) | [nao verificado]
| **f2. “Sem ata” fora dessa abstenção específica.** (`disputa_emissao.py:995`, `:1003`) | Não existe veto universal de ata no gerador genérico; `_ata_do_dia` também permite seguir sem escala vigente ou após erro de leitura. (`disputa_emissao.py:434`, `:995`, `:1003`) | Pode gerar P com dia conhecido e célula ausente; ou cair nas recusas de a2. (`disputa_emissao.py:437`, `:457`; `models.py:2896`, `:3014`) | **DS/P/Ch/contexto:** conforme geração efetiva; “sem ata” sozinho não determina zero P em todos os emissores. (`disputa_emissao.py:650`; `models.py:2896`, `:3014`) | Conforme a1/a2; não há retorno HX universal. (`disputa_emissao.py:672`, `:767`) | **AUSENTE** para uma garantia universal; a cena existente cobre o agrupador específico. (`test_hx_disputa_muda_sem_ata.py:55`) | **DEPENDE** de nascer P local; não se pode estender a exceção HX a toda recusa. (`disputa_emissao.py:443`, `:650`) |

Os demais caminhos de nascimento e o estado de motivo repetido completam a matriz:

| Estado | Juiz — quem decide | Operação, em ordem | Persistido ao fim | Retorno ao chamador | Prova | “Nasce com pergunta ou não nasce” |
|---|---|---|---|---|---|---|
| **g1. Grade, helper `_disputa`: chamada de `reconciliar_grade.py:424`.** (`reconciliar_grade.py:412`) | Núcleo, recebendo `motivos=[]`. (`reconciliar_grade.py:424`, `:425`) | Chama núcleo → pode abrir pai/DS → laço de perguntas vazio. Invocação pelo fluxo efetivo do comando: **[nao verificado]**; o sítio é um helper. (`reconciliar_grade.py:424`; `disputa_emissao.py:403`, `:643`) | **Se nova:** DS aberta, `motivos=[]`, contexto `{'origem':'grade'}`, pai; **P zero**. Se já aberta, retorna a existente sem merge. (`reconciliar_grade.py:425`, `:427`; `disputa_emissao.py:585`, `:643`, `:650`) | DS; esse helper não chama push. (`reconciliar_grade.py:428`) | `test_bo_disputa_sem_pergunta_memoria.py::test_chamada_grade_motivos_vazios_conta_estrutura`, linha 416, chama núcleo diretamente. (`test_bo_disputa_sem_pergunta_memoria.py:416`) | **NAO se nasce DS; SIM quanto a não haver novo nascimento se reutiliza existente.** (`disputa_emissao.py:622`, `:643`) | [nao verificado]
| **g2. Âncora: `criar_disputa_ancora_de_chamado`, nascimento de `:1452`.** (`disputa_emissao.py:1396`, `:1452`) | Guardas categoria/status/pergunta por destino; nascimento por `abrir_conversa`. (`disputa_emissao.py:1410`, `:1417`, `:1465`) | Atomic → verifica P existente → trava → abre/reusa pai/DS → cria conversa → analisa limbo → push → retorno. (`disputa_emissao.py:1416`, `:1425`, `:1433`, `:1465`, `:1473`, `:1475`) | **DS:** nova ou motivo agregado. **P:** conversa local `ancora_ausente`, `data_hora`, destino limbo, dia civil. **Ch:** pai e transição do limbo. **Contexto novo:** origem/id do limbo. (`disputa_emissao.py:1454`, `:1455`, `:1465`, `:1473`; `models.py:2801`, `:3014`) | Guardas retornam `None`; sucesso retorna DS. Exceção propagada desfaz o atomic. (`disputa_emissao.py:1411`, `:1422`, `:1416`, `:1476`) | `test_fio_ancora.py::test_01_disparo_gera_pergunta_sem_batida`, linha 69; `::test_02_idempotente`, linha 88. (`test_fio_ancora.py:69`, `:88`) | **SIM no sucesso:** conversa nasce na DS recebida. Retorno estrangeiro não é estado real desta porta de conversa. (`models.py:2893`, `:3014`, `:3017`) | [nao verificado]
| **h1. Órfãos elegíveis, perguntas locais emitidas.** (`disputa_emissao.py:1121`, `:1269`) | Elegibilidade do chamado; marcos pela ata; falta pela jornada; dedup pelo fio; identidade pela P. (`disputa_emissao.py:1182`, `:1265`, `:1294`, `:1509`) | Trava repasse → complementa existentes → filtra → abre/reusa DS → percorre intervalo/ata/origem/contexto → pendentes → decide fecho. (`disputa_emissao.py:1153`, `:1175`, `:1190`, `:1232`, `:1262`, `:1288`, `:1325`, `:1364`) | **DS:** motivos atualizados; contexto novo com origem/IDs. **P:** locais por marco/fato. **Ch:** pai manual e chamadas a `analisar` dos órfãos atendidos. Na reutilização, não há merge equivalente ao do núcleo para os novos IDs. (`disputa_emissao.py:1210`, `:1217`, `:1225`, `:1272`, `:1317`, `:1349`) | DS; push condicionado a `notificar`, se não retornou antes pelo fecho. (`disputa_emissao.py:1382`, `:1383`, `:1385`) | `test_pergunta_no_app.py::test_MORDE_col709_dia_mais_velho_ganha_a_pergunta_do_marco_apagado`, linha 89; `::test_duas_passadas_uma_pergunta`, linha 103. (`test_pergunta_no_app.py:89`, `:103`) | **SIM quando alguma P fica local; DEPENDE nos demais ramos abaixo.** (`disputa_emissao.py:931`, `:1372`) | [nao verificado]
| **h2. Órfãos, zero P, sem abstenção HX e `pode_fechar=True`.** (`disputa_emissao.py:1376`) | Guardas podem calar por dia ausente, sem base ou sem falta; fechamento pertence a `DS.pode_fechar`/`fechar`. (`disputa_emissao.py:1300`, `:1327`, `:1329`, `:1332`; `models.py:1848`) | Nasce DS/pai → não nasce P → esgota pendentes → fecha DS; não desfaz nascimento. (`disputa_emissao.py:1217`, `:1364`, `:1378`) | **DS:** persiste fechada, com `fechada_em`, autor, resolução e `motivo_fechamento=vazia`. **P:** zero. **Ch:** pai persiste, sujeito à reconciliação do receiver. **Contexto:** origem/IDs permanece. (`models.py:1903`, `:1932`, `:1936`; `signals.py:42`; `disputa_emissao.py:1220`) | DS fechada; retorna antes do push. (`disputa_emissao.py:1382`) | `test_hx_disputa_muda_sem_ata.py::test_com_ata_e_dia_completo_fecha_como_antes`, linha 63, exige DS com zero P e fechada; push: `test_hx_disputa_muda.py::test_nao_notifica_disputa_muda`, linha 44. (`test_hx_disputa_muda_sem_ata.py:63`, `:77`, `:78`; `test_hx_disputa_muda.py:44`) | **NAO.** Fechar a DS vazia não equivale a não a fazer nascer. (`disputa_emissao.py:1217`, `:1378`) | [nao verificado]
| **h3. Órfãos, porta devolve P de outra dona.** (`disputa_emissao.py:931`) | Identidade da P decide a dona; `_criar_pergunta_se_couber` ignora o retorno. (`models.py:2921`, `:3008`; `disputa_emissao.py:931`, `:941`) | Acrescenta motivo antes dos vetos → chama `abrir` → devolve `True` sem conferir dona → pode analisar chamados → decide fecho da DS local. (`disputa_emissao.py:922`, `:931`, `:941`, `:1317`, `:1372`) | **DS:** local mantém motivos; não há encaminhamento do motivo à dona como no núcleo. **P:** estrangeira conserva FK, poden

[truncado em 20000 de 44262 caracteres]

## 10/11 18:52 · sessao 28e142e5e8be · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Corrigidos os dois pontos: [guarda de donas visitadas](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:619) e [dia indeterminado classificado como I7a](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/management/commands/auditar_invariantes_chamados.py:69).

A [entrega VOLTA 4](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:136) registra comandos, RED contra a base, três mutantes mortos, 37 testes de memória e 35 da catraca verdes. O deslocamento de nove linhas foi [registrado com justificativa](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/core/tests/test_medida_turno_do_par.py:267). [nao verificado]

FALTA certificação ORM/SQL pelo Code e revisão final Opus [nao verificado].

## 10/11 18:51 · sessao d1cb93410c4e · - · - · origem=Ronald (tmux)

Correção da linha âncora: retorno estrangeiro não é estado real nesta cópia. `abrir_conversa` sempre insere pergunta na DS recebida; o dedup canônico só roda para `conversa=False`. Portanto, sucesso da âncora persiste P local; exceção da porta desfaz pai/DS no atomic externo. Push e retorno estão dentro desse atomic. (`app/chamados/models.py:2794`, `:2801`, `:2893`, `:3008`, `:3014`, `:3017`; `app/chamados/services/disputa_emissao.py:1416`, `:1465`, `:1475`, `:1476`)

## 10/11 18:50 · sessao d1cb93410c4e · - · - · origem=Ronald (tmux)

Censo de nascimento: três `DisputaSupervisao.objects.create` no código de aplicação fora de testes/migrations: núcleo, órfãos e âncora; as rotas de declaração e grade delegam ao núcleo. (`app/chamados/services/disputa_emissao.py:643`, `:1217`, `:1452`, `:722`; `app/ponto/management/commands/reconciliar_grade.py:424`)

| Caminho | Operação e persistido | Retorno/push | Prova | [nao verificado]
|---|---|---|---|
| Grade `_disputa` | Chama núcleo com `motivos=[]`, contexto `{'origem':'grade'}`; nascendo, pai e DS ficam sem P porque laço do gerador recebe lista vazia. (`app/ponto/management/commands/reconciliar_grade.py:424`; `app/chamados/services/disputa_emissao.py:643`, `:650`, `:403`) | Retorna `d`; helper não chama push. Não há caller de `_disputa` neste comando; fluxo de `handle` chama `_emitir_furo`, que cria/reabre apenas Chamado e retorna. (`app/ponto/management/commands/reconciliar_grade.py:428`, `:254`, `:488`, `:507`, `:529`, `:531`) | Memória testa núcleo diretamente: `test_bo_disputa_sem_pergunta_memoria.py::test_chamada_grade_motivos_vazios_conta_estrutura`; comando inteiro para este nascimento: AUSENTE. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:421`) |
| Órfãos — elegibilidade | Remove dias na lista do admin antes atomic; vazio retorna None. Dentro atomic trava fecho C/D/Ch/P. Pode complementar perguntas em dona preexistente e analisar Chamado antes de retornar None no segundo filtro. (`app/chamados/services/disputa_emissao.py:1141`, `:1143`, `:1153`, `:1164`, `:1175`, `:1177`, `:1179`, `:1187`) | None pode coexistir com complementos persistidos; não implica ausência de escrita. (`app/chamados/services/disputa_emissao.py:1175`, `:1188`) | Prova específica retorno None com complemento: AUSENTE [nao verificado]. |
| Órfãos — nascimento normal | Seleciona primeira DS aberta; se ausente abre pai manual e cria DS com motivos existentes ou `['orfao_14h']`, contexto origem/IDs e FK pai. Ramo intervalo pergunta por marcos com data parseável; demais por ata, batida-origem ou contexto. (`app/chamados/services/disputa_emissao.py:1190`, `:1194`, `:1198`, `:1217`, `:1220`, `:1232`, `:1242`, `:1262`, `:1288`, `:1325`) | Ao fim pergunta pendentes, consulta `pode_fechar`; permanece aberta se não pode fechar; push se `notificar`, dentro atomic, com DS local/motivos atualizados. (`app/chamados/services/disputa_emissao.py:1364`, `:1372`, `:1383`, `:1385`) | `test_pergunta_no_app.py::test_MORDE_col709_dia_mais_velho_ganha_a_pergunta_do_marco_apagado`, `::test_duas_passadas_uma_pergunta`. (`app/chamados/tests/test_pergunta_no_app.py:89`, `:103`) |
| Órfãos — zero P fora HX | Continua sem pergunta quando marco intervalo inválido, dia do fato ausente, falta de base, juiz sem falta ou veto ausência/fio; motivo pode entrar antes do veto. Se `pode_fechar` e não houve abstenção por ata, persiste DS FECHADA e pai; não desfaz nascimento. (`app/chamados/services/disputa_emissao.py:1244`, `:1300`, `:1327`, `:1329`, `:1332`, `:922`, `:924`, `:926`, `:1376`, `:1378`) | Retorna DS fechada antes do push. Literal “nasce com pergunta ou não nasce”: NÃO. (`app/chamados/services/disputa_emissao.py:1382`, `:1384`) | `test_hx_disputa_muda_sem_ata.py::test_com_ata_e_dia_completo_fecha_como_antes` exige zero P e DS fechada; `test_hx_disputa_muda.py::test_nao_notifica_disputa_muda` exige ausência de push. (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:63`, `:77`, `:78`; `app/chamados/tests/test_hx_disputa_muda.py:44`, `:50`) |
| Órfãos — sem ata HX | Pai/DS já existem; `_ata_do_dia` chama `_marcos_faltantes`; estado sem_celula com escala vigente acende e recusa perguntas. Mantém DS aberta zero P, acende auto-fecho. Sem escala vigente, gate permite seguir; exceção de leitura também permite seguir. (`app/chamados/services/disputa_emissao.py:1217`, `:990`, `:995`, `:996`, `:1003`, `:1005`, `:1267`, `:1305`, `:1336`, `:1373`, `:1375`) | Se `notificar=True`, tenta push mesmo zero P; teste HX usa `notificar=False`. Literal invariante: NÃO, exceção explicitamente aprovada pelo dado do usuário. (`app/chamados/services/disputa_emissao.py:1383`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:55`) | `test_hx_disputa_muda_sem_ata.py::test_sem_ata_nao_fecha_e_acende` exige zero P/aberta/warning. (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51`, `:58`, `:59`, `:61`) |
| Órfãos — retorno estrangeiro | `_criar_pergunta_se_couber` chama `PerguntaDisputa.abrir` e ignora `(pergunta,criada)`, devolvendo `True`; não transfere motivo nem troca DS local pela dona. Chamados podem ser analisados como se pergunta local tivesse nascido. (`app/chamados/services/disputa_emissao.py:931`, `:941`, `:1252`, `:1271`, `:1314`, `:1317`, `:1345`) | Resultado/push referem-se à DS local; pode fechar local zero P ou conservá-la conforme `pode_fechar`/HX. Literal invariante DEPENDE de outra P local. (`app/chamados/services/disputa_emissao.py:1372`, `:1373`, `:1376`, `:1384`) | Combinação específica com dona estrangeira: AUSENTE [nao verificado]. |
| Órfãos — universo mudou | `_trava_do_repasse` recusa crescimento do conjunto; atomic desfaz tentativa; captura fora grava evento `repasse_recusado`. (`app/chamados/services/disputa_emissao.py:1097`, `:1100`, `:1108`, `:1110`, `:1387`, `:1390`) | `REPASSE_RECUSADO`; outras exceções não são capturadas nesse handler. (`app/chamados/services/disputa_emissao.py:1393`) | Provas específicas podem estar em testes de repasse; não auditadas [nao verificado]. |
| Âncora — nova | Guarda categoria/status e P não materializada por destino; trava universo aberto; abre/reusa pai manual, cria DS com `ancora_ausente`, contexto origem/id do limbo; abre conversa `data_hora` com destino limbo, analisa limbo. (`app/chamados/services/disputa_emissao.py:1410`, `:1412`, `:1417`, `:1425`, `:1433`, `:1452`, `:1455`, `:1465`, `:1469`, `:1471`, `:1473`) | Push da DS local dentro atomic, depois retorna DS. Literal invariante SIM se P ficou nela. (`app/chamados/services/disputa_emissao.py:1475`, `:1476`) | `test_fio_ancora.py::test_01_disparo_gera_pergunta_sem_batida`, `::test_02_idempotente`. (`app/chamados/tests/test_fio_ancora.py:69`, `:78`, `:88`, `:95`) |
| Âncora — estrangeira | Ignora retorno de `abrir_conversa`; DS/pai local persistem mesmo que conversa pertença a outra DS, salvo exceção externa. Analisa limbo e notifica pela local. (`app/chamados/services/disputa_emissao.py:1452`, `:1465`, `:1473`, `:1475`) | Retorna local; literal invariante NÃO se local sem outra P. (`app/chamados/services/disputa_emissao.py:1476`) | AUSENTE [nao verificado]. |
| Declaração helper isolado | `disputa_para_declaracao` chama núcleo com `emitir_perguntas=False`, contexto origem, merge None. Pode persistir pai/DS sem P; não gera nem envia push. (`app/chamados/services/disputa_emissao.py:709`, `:712`, `:714`, `:722`, `:725`, `:726`) | Retorna DS local. Literal invariante NÃO no limite desse helper. (`app/chamados/services/disputa_emissao.py:727`) | `test_porta_disputa_emissao.py::test_declaracao_sem_disputa_aberta_faz_nascer_uma_com_pai_e_trilha`, `::test_MORDE_a_declaracao_nao_manda_push_nem_varre_pergunta`. (`app/chamados/tests/test_porta_disputa_emissao.py:185`, `:189`, `:223`, `:230`, `:231`) |
| Declaração consumidores | Ambos têm atomic externo, trava antes do helper e abrem P depois. `_candidata_reusavel` só retorna canônica na própria DS; estrangeira resulta None e chamada de `abrir` pode devolvê-la. Consumidores não conferem dona nem eliminam DS local. Retornos normais de erro dentro atomic também confirmam escrita anterior. (`app/chamados/services/declaracao_texto.py:348`, `:352`, `:360`, `:250`, `:254`, `:376`, `:412`, `:421`, `:654`, `:656`, `:659`, `:666`, `:683`, `:695`) | Declaração colab retorna dicionário; admin valida depois do atomic usando `disputa=d` local. Sem push do helper. Invariante DEPENDE de P retornada pertencer a d. (`app/chamados/services/declaracao_texto.py:426`, `:696`, `:697`; `app/chamados/services/disputa_emissao.py:712`) | Helper coberto acima; combinação consumidor+canônica estrangeira: AUSENTE [nao verificado]. |
| Fábrica | `perguntar` desfaz carimbo, chama wrapper e chama gerador novamente depois do núcleo; não é outro create direto. Julgamento envolve chamada em silêncio de push. (`app/chamados/services/fabrica_da_celula.py:169`, `:172`, `:174`, `:218`, `:219`) | `perguntar` retorna contagem de desfeitos; pós-geração pode falhar após núcleo já concluído, salvo atomic externo. (`app/chamados/services/fabrica_da_celula.py:175`) | Cobertura específica da segunda geração com falha: AUSENTE [nao verificado]. |
| Smoke | Comando chama núcleo e gera outra vez depois; não há create DS separado. (`app/chamados/management/commands/smoke_chamados.py:99`, `:102`) | Exceção vira contador/problema no comando. (`app/chamados/management/commands/smoke_chamados.py:114`, `:116`) | É bancada do próprio comando; prova da atomicidade pós-núcleo AUSENTE [nao verificado]. |

Push sempre usa `disputa.colaborador`, URL `/meu-atendimento/`, sem FK/ID da dona na URL; não consulta fechamento nem existência de P. `_PUSH_SILENCIADO` suprime antes das chamadas WebPush/FCM; exceptions são registradas e engolidas. Entrega efetiva [nao verificado]. (`app/chamados/services/disputa_emissao.py:1845`, `:1847`, `:1861`, `:1862`, `:1865`, `:1869`, `:1870`, `:1872`)

O código não realiza push por dona em lote: o wrapper usa a DS única retornada pelo núcleo; nascimento manda `nova=True`; agregação chama pendentes e manda somente quando `novos` não vazio. (`app/chamados/services/disputa_emissao.py:765`, `:766`, `:779`, `:780`, `:781`)

## 10/11 18:50 · sessao 5c96eb64a4b6 · - · - · origem=Ronald (tmux)

**Efeitos da dona fechada.** O núcleo salva `motivos`/`contexto` antes de chamar o gerador, preservando diretamente `fechada_em`; o `save` aciona os receivers mesmo sem transição de estado. (`app/chamados/services/disputa_emissao.py:575`, `:583`, `:604`, `:607`, `:609`)

- O receiver tenta reconciliar o pai quando ele existe e não está em `ENCERRADOS`; pode acrescentar ocorrência de fechamento. (`app/chamados/signals.py:28`, `:33`, `:42`, `:45`)
- Outro receiver agenda `retomar_regularizacoes_confirmadas` em `on_commit`, selecionando regularizações confirmadas sem materialização daquele colaborador. (`app/chamados/signals.py:129`, `:137`, `:144`; `app/chamados/services/regularizacao_ext.py:218`, `:221`, `:224`)
- Outro chama `materializar_pendentes_aprovadas`, que percorre perguntas não materializadas e carimba somente as aprovadas pelo juiz e pela porta. (`app/chamados/signals.py:153`, `:163`; `app/chamados/services/materializacao.py:1418`, `:1427`, `:1431`)
- O persistido adicional exato depende de pai, perguntas e regularizações preexistentes: [nao verificado]. O teste de reuso fechado substitui `abrir_conversa` e o push; não demonstra todos esses efeitos. (`app/chamados/tests/test_bo_disputa_sem_pergunta.py:68`, `:81`, `:82`)

**Push e retorno com várias donas.** A recuperação retorna somente a primeira dona processada e a união dos motivos novos das diversas agregações. (`app/chamados/services/disputa_emissao.py:687`, `:695`, `:697`, `:698`)

- Se nasceu: wrapper chama push com todos os motivos e `nova=True`. (`app/chamados/services/disputa_emissao.py:765`, `:766`)
- Se agregou/reusou: wrapper chama primeiro `perguntar_entradas_pendentes` sobre essa única dona; depois envia push se a união `novos` não for vazia. Assim, motivos novos de outras donas podem compor o aviso associado à primeira. (`app/chamados/services/disputa_emissao.py:779`, `:780`, `:781`; `:695`, `:698`)
- O helper de push não verifica `fechada_em`; tenta web push e FCM para o colaborador e usa URL genérica `/meu-atendimento/`, sem ID de dona no endereço. Silêncio contextual suprime ambos. Entrega real: [nao verificado]. (`app/chamados/services/disputa_emissao.py:1847`, `:1855`, `:1861`, `:1865`, `:1869`)
- Órfãos enviam somente com `notificar`, após a tentativa de fecho automático; quando esse fecho ocorre, retornam antes do push. (`app/chamados/services/disputa_emissao.py:1376`, `:1382`, `:1383`, `:1384`)
- A fábrica de perguntas por célula usa `push_disputa_silenciado`; ainda chama o gerador novamente depois do wrapper. (`app/chamados/services/fabrica_da_celula.py:172`, `:174`, `:217`, `:218`, `:219`)

**Exceção após batida.** A API grava a batida por `registrar_batida` antes de chamar a emissão. O núcleo captura somente `_PerguntaDeOutraDisputa`; exceções do gerador atravessam o `atomic` e impedem o wrapper de chegar ao push. (`app/api/views_core.py:681`, `:794`; `app/chamados/services/disputa_emissao.py:546`, `:547`, `:650`, `:673`, `:765`)

A prova ORM existente cria batida antes da emissão, exige preservação dela, igualdade das contagens de DS/Chamado/Pergunta/log e ausência de push; outro teste falha depois de criar pergunta e exige rollback. Execução ORM nesta sessão: [nao verificado]. (`app/chamados/tests/test_bo_disputa_sem_pergunta.py::test_gerador_levanta_desfaz_disputa_pai_e_pergunta_mas_preserva_batida`, linhas `32–53`; `::test_gerador_falha_apos_pergunta_desfaz_as_duas`, linhas `55–66`)

Não há retry genérico nessa captura: quem/quando reemitirá **o mesmo episódio** é [nao verificado]. Existem entradas futuras condicionais: nova chamada API reexecuta detector e emissão; cartório pergunta somente pelas células julgadas naquele ato; cron de foto percorre batidas ainda sem selfie cobrável às 07:26. Isso não demonstra recuperação universal do episódio perdido. (`app/api/views_core.py:433`, `:794`; `app/ponto/services/cartorio.py:992`, `:1007`, `:1008`; `app/chamados/services/fabrica_da_celula.py:207`, `:219`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:95`, `:96`; `app/config/crons.py:686`)

**Ciclo.** A cópia mantém recursão na recuperação e não contém guarda de visitadas no corpo; o teste “dona diferente” compara apenas a pergunta retornada com a DS da tentativa atual. Um ciclo A→B→A não encontra limite explícito; persistência final depende da saída/erro e dos `atomic` envolventes. Reprodução com ORM: [nao verificado]. (`app/chamados/services/disputa_emissao.py:612`, `:621`, `:676`, `:690`, `:698`)

**Auditor.** I7a/I7b contam somente DS abertas com zero linhas de perguntas; pergunta morta também conta como linha e exclui a DS desses números. (`app/chamados/management/commands/auditar_invariantes_chamados.py:61`, `:63`; `app/chamados/tests/test_bo_disputa_sem_pergunta.py:100`, `:111`, `:112`)

O auditor descarta motivos sem resolução/dia; portanto um motivo indeterminado mais outro dia `sem_celula` pode acabar em I7b. `bool(dias)` protege apenas o conjunto totalmente vazio. (`app/chamados/management/commands/auditar_invariantes_chamados.py:49`, `:51`, `:53`, `:69`, `:70`)

A cena HX exige DS existente, zero perguntas, aberta e warning; BO a incorpora e exige `(I7a,I7b)=(0,1)`. (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:55`, `:56`, `:58`, `:59`, `:61`; `app/chamados/tests/test_bo_disputa_sem_pergunta.py:95`, `:97`, `:98`)

**Limite das provas.** As suites de memória declaram que não certificam PostgreSQL, signals nem a suite Django. O arquivo de teste `test_e1_ordem_de_trava.py` citado no comentário do núcleo está AUSENTE nesta cópia; não o usar como prova executável. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:1`, `:3`; `app/chamados/services/disputa_emissao.py:553`; presença do arquivo: [nao verificado])

## 10/11 18:50 · sessao 0425b48d5ffb · - · - · origem=Ronald (tmux)

**Conclusão:** a substituição da exceção de controle e da reentrada cabe no serviço de emissão, sem alterar `chamados/models.py`, desde que o planejamento use a identidade completa da porta e congele cada fato antes da escrita. “Nenhuma exceção possível” não cabe: a porta mantém recusas deliberadas e transacionais. (`app/chamados/services/disputa_emissao.py:673`, `app/chamados/models.py:2840`, `app/chamados/models.py:2917`, `app/chamados/models.py:2954`, `app/chamados/models.py:2819`)

| Questão | Resposta fundamentada |
|---|---|
| Quem responde a dona sem escrever? | `fio.universo_do_fato(colaborador_id, motivo, dia, batida_origem_id=...)`, seguido de `fio.canonica_do_grupo(linhas).disputa`; lista vazia significa nenhuma canônica naquele universo. O universo é explicitamente o mesmo usado pela porta, e a canônica prefere resposta do colaborador, depois menor PK. (`app/chamados/services/fio.py:1784`, `app/chamados/services/fio.py:1794`, `app/chamados/services/fio.py:1831`, `app/chamados/services/fio.py:1225`) |
| `PerguntaDisputa.do_fato` basta? | Não: fornece as linhas iniciais da chave; a porta usa o fecho limitado pelo dia e elege um componente do fato. Há identidade por episódio e acervo sem episódio. (`app/chamados/models.py:2717`, `app/chamados/models.py:2900`, `app/chamados/models.py:2907`, `app/chamados/services/fio.py:1826`) |
| `_candidata_reusavel` responde dona global? | Não: elege no universo completo, mas só devolve a canônica se pertence à disputa recebida e satisfaz a reusabilidade. Pode retornar `None` com canônica existente noutra disputa. (`app/chamados/services/declaracao_texto.py:230`, `app/chamados/services/declaracao_texto.py:250`, `app/chamados/services/declaracao_texto.py:254`) |
| `(colaborador,motivo,dia)` sempre identifica sozinho? | Não para a família BATIDA: episódio conhecido participa da consulta; dois componentes distintos podem existir no mesmo dia. O planejamento deve levar episódio, instância e dia normalizado pela mesma autoridade da porta. (`app/chamados/models.py:2712`, `app/chamados/models.py:2866`, `app/chamados/models.py:2875`, `app/chamados/services/fio.py:1826`) |
| O helper atual de resolução basta? | Não: retorna texto/destino/dia e recusa alguns casos, mas não consulta a canônica global; contém dedup local que retorna `None`. Outras guardas continuam no gerador: catálogo, dedup não datado, hora sem dia e conversa repetida. (`app/chamados/services/disputa_emissao.py:300`, `app/chamados/services/disputa_emissao.py:361`, `app/chamados/services/disputa_emissao.py:380`, `app/chamados/services/disputa_emissao.py:404`, `app/chamados/services/disputa_emissao.py:415`, `app/chamados/services/disputa_emissao.py:438`, `app/chamados/services/disputa_emissao.py:449`) |
| Leitura prévia sem trava equivale à porta? | Não oferece a proteção que a porta usa: ela trava o colaborador antes do dedup; o teste de corrida exige uma única pergunta e retornos `criada=True/False` para duas entradas concorrentes. (`app/chamados/models.py:2845`, `app/chamados/tests/test_e1_identidade_corrida.py:74`, `app/chamados/tests/test_e1_identidade_corrida.py:81`) |
| Leitura sob C no mesmo atomic resolve tudo? | Protege contra escritores que seguem a mesma trava, mas o plano precisa reproduzir a identidade completa e manter os insumos: a porta recalcula episódio/dia, pode rechavear a canônica e pode recusar. Equivalência integral numa implementação ainda inexistente: [nao verificado]. (`app/chamados/models.py:2866`, `app/chamados/models.py:2875`, `app/chamados/models.py:2940`, `app/chamados/models.py:2954`) |
| Pergunta morta segura identidade? | “Morta” não basta como filtro: a identidade exclui especificamente `duplicata` e `motivo_retratado`; fechamento da disputa e demais vias não estão nessa exclusão. Filtrar previamente por pergunta viva ou disputa aberta divergiria da porta. (`app/chamados/models.py:2393`, `app/chamados/models.py:2754`, `app/chamados/models.py:2921`) |
| Rechaveamento pode desaparecer do desenho? | Não: reuso com dia gravado diferente passa pela porta de rechaveamento; competência desconhecida/exportada, colisão e outras recusas continuam. O “dry” existente roda escrita real em atomic desfeito, portanto não é uma leitura pura. (`app/chamados/models.py:2940`, `app/chamados/services/fio.py:1056`, `app/chamados/services/fio.py:1068`, `app/chamados/services/fio.py:1077`) |

A ordem exige **C → todas as DS necessárias → todos os Chamados necessários → todas as Perguntas necessárias**, com PK crescente por grupo, antes da escrita que possa disparar cascatas; usar apenas chamadas individuais aparentemente ordenadas não certifica a transação inteira. O topo atual inclui DS abertas, DS explicitamente pedidas, pais das abertas e Chamados vivos, mas não inclui automaticamente destino terminal de canônica fechada. No rechaveamento, `_nascer` pede tanto o destino recebido quanto o destino anterior e a pergunta. Num lote, é necessário antecipar esse universo antes da primeira Pergunta. (`app/chamados/models.py:2631`, `app/chamados/models.py:2675`, `app/chamados/models.py:2677`, `app/chamados/models.py:2679`, `app/chamados/models.py:2683`, `app/chamados/models.py:2692`, `app/chamados/models.py:2947`)

Mudanças de desenho necessárias, sem código de produção:

- `disputa_emissao.py::_abrir_ou_agregar_disputa`: planejar cada fato sob C, particionar pelas donas e escrever uma vez; retirar `_PerguntaDeOutraDisputa` como mecanismo de descoberta/reentrada. Hoje a descoberta ocorre depois de salvar a DS e gerar perguntas, seguida de rollback e chamada recursiva. (`app/chamados/services/disputa_emissao.py:607`, `app/chamados/services/disputa_emissao.py:609`, `app/chamados/services/disputa_emissao.py:621`, `app/chamados/services/disputa_emissao.py:673`, `app/chamados/services/disputa_emissao.py:690`)
- `disputa_emissao.py::_resolver_pergunta_por_motivo` e `::gerar_perguntas_disputa`: separar resolução de insumos da decisão de emissão/dedup e consumir o plano congelado; recalcular a data no contexto de cada dona reproduz a condição do ciclo A↔B. Hoje a data vem do contexto da DS usada em cada entrada. (`app/chamados/services/disputa_emissao.py:335`, `app/chamados/services/disputa_emissao.py:387`, `app/chamados/services/disputa_emissao.py:434`, `app/chamados/services/disputa_emissao.py:585`, `app/chamados/services/disputa_emissao.py:690`)
- `disputa_emissao.py::criar_ou_atualizar_disputa`: tratar o resultado de várias donas para pendentes e push; hoje o núcleo devolve somente a primeira dona e o conjunto agregado de motivos novos. (`app/chamados/services/disputa_emissao.py:695`, `app/chamados/services/disputa_emissao.py:698`, `app/chamados/services/disputa_emissao.py:779`, `app/chamados/services/disputa_emissao.py:781`)
- `auditar_invariantes_chamados.py::_dias_da_disputa_sem_pergunta`: continuar consumindo a fonte comum de resolução caso a assinatura/resultado do helper mude; hoje lê diretamente a posição `[2]`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:49`, `app/chamados/management/commands/auditar_invariantes_chamados.py:50`, `app/chamados/management/commands/auditar_invariantes_chamados.py:52`)
- Não mudar `models.py` para contornar identidade, rechaveamento, contrato de saída ou recusas: a API pública já aceita a dona escolhida como `disputa`, preserva a FK da canônica reusada e mantém esses controles. (`app/chamados/models.py:2776`, `app/chamados/models.py:2789`, `app/chamados/models.py:3008`, `app/chamados/models.py:3014`)

Os testes existentes exigem preservar: [nao verificado]

| Exigência | Teste | [nao verificado]
|---|---|
| Canônica do fecho, não filtro local | `app/chamados/tests/test_e1_v13.py:88::test_RED_abrir_alfa_segura_a_canonica_do_fecho`; `app/chamados/tests/test_e1_v13.py:98::test_RED_candidata_reusavel_segura_a_canonica_do_fecho`. |
| Um componente por fato, inclusive episódios/acervo | `app/chamados/tests/test_e1_v13.py:346::test_RED_porta_sem_episodio_e_um_componente`. |
| Rechavear no mesmo ato e idempotência | `app/chamados/tests/test_e1_v15d.py:60::test_RED_R1_reuso_rechaveia_no_mesmo_ato_e_2x_e_o_mesmo_estado`. |
| Recusa sem escrita: sem célula, colisão, competência exportada | `app/chamados/tests/test_e1_v15d.py:84::test_RED_R2_dia_novo_sem_celula_recusa_declarada_sem_escrever`; `:101::test_RED_R3_colisao_com_outra_linha_do_fato_recusa`; `:124::test_RED_R4_competencia_exportada_no_dia_velho_ou_novo_recusa`. |
| Corrida produz uma pergunta e mesmo PK | `app/chamados/tests/test_e1_identidade_corrida.py:74::test_MORDE_duas_na_janela_nascem_UMA_pergunta`; `:81::test_MORDE_a_segunda_recebe_criada_False`. |
| Ordem real linha a linha | `app/chamados/tests/test_e1_v15f.py:117::test_RED_reuso_com_rechave_e_costura_trava_na_ordem`; definição de violação em `app/chamados/tests/test_e1_v9.py:96`. |
| Só `trava_na_ordem` adquire travas | `app/chamados/tests/test_e1_v7_catracas.py:387::test_zero_trava_fora_de_trava_na_ordem`. |
| Canônica eleita sobre universo autorizado | `app/chamados/tests/test_e1_v7_catracas.py:447::test_zero_canonica_fora_do_universo`. |
| Declaração não envia push nem chama gerador | `app/chamados/tests/test_porta_disputa_emissao.py:223::test_MORDE_a_declaracao_nao_manda_push_nem_varre_pergunta`. |
| Urgência normal e contexto preexistente conservado na declaração | `app/chamados/tests/test_porta_disputa_emissao.py:194::test_MORDE_a_urgencia_da_declaracao_e_normal_nao_emergencia`; `:199::test_disputa_aberta_agrega_o_motivo_e_NAO_toca_o_contexto`. |

Não cabe prometer ausência absoluta de exceções, usar `do_fato` sem o universo, tomar `_candidata_reusavel` como dona global, filtrar canônicas por vitalidade, nem usar `rechavear_no_reuso(dry=True)` como leitura pura. Esses atalhos conflitam com os contratos já escritos. (`app/chamados/models.py:2905`, `app/chamados/services/declaracao_texto.py:254`, `app/chamados/models.py:2754`, `app/chamados/services/fio.py:1077`)

Certificação ORM, corrida efetivamente executada, SQL total, efeitos de receivers e validade de uma implementação nova permanecem [nao verificado].

## 10/11 18:41 · sessao cec9bd295d6b · BO-DISPUTA-SEM-PERGUNTA · verificador · origem=Code

**Não liberaria ainda:** a reentrada pode ciclar entre donas; o auditor pode publicar I7b descartando um motivo sem dia; e continuam gerações posteriores à transação do núcleo. (`app/chamados/services/disputa_emissao.py:690`, `:779`, `:294`; `app/chamados/management/commands/auditar_invariantes_chamados.py:51`, `:69`; `app/chamados/services/fabrica_da_celula.py:174`)

1. **Estado dos achados da `preverif1.r`**

   | Achado anterior | Estado atual | Linha que decide |
   |---|---|---|
   | 1. Geração posterior ao núcleo | **Igual.** Principal protegida; pendentes e fábrica continuam depois. | `app/chamados/services/disputa_emissao.py:609`, `:650`, `:779`, `:294`; `app/chamados/services/fabrica_da_celula.py:174` |
   | 2. Retorno estrangeiro ignorado na agregação | **Curado nesse ponto.** Agora detecta e levanta; persistem limitações de reentrada descritas abaixo. | `app/chamados/services/disputa_emissao.py:612`, `:621` |
   | 3. Batida anterior ao atomic; resposta HTTP não garantida | **Igual.** A disputa continua posterior à gravação; o núcleo captura somente a exceção específica de reuso. | `app/api/views_core.py:681`, `:794`; `app/api/views.py:684`, `:794`; `app/chamados/services/disputa_emissao.py:673` |
   | 4. HX preservada | **Igual, corretamente.** Abstenção sem ata e impedimento de fecho continuam. | `app/chamados/services/disputa_emissao.py:1267`, `:1304`, `:1335`, `:1373`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58` |
   | 5. Derivação paralela do dia por motivo | **Curado quanto à divergência anterior.** Auditor chama o helper por motivo; há outra insuficiência com `None`. | `app/chamados/management/commands/auditar_invariantes_chamados.py:49`, `:50`, `:51` |
   | 6. Fechada adicional travada depois de Chamado na recuperação | **Curado para o universo de DS.** Recuperação agora antecipa todas as DS. A ordem completa de Ch/P tem ressalva abaixo. | `app/chamados/services/disputa_emissao.py:681`, `:683` |
   | 7. C8 sem certificação dos totais SQL | **Igual.** Justificativas presentes; 145/219 efetivos são [nao verificado]. | `app/ponto/tests/test_selo_chokepoint_escrita.py:180`, `:185`, `:193`, `:198` |
   | 8. RED/testes sem mordida suficiente | **Parcial.** Declaração e `all` vazio foram fortalecidos; execução histórica da base nesta cópia é [nao verificado]. | `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:330`, `:406`, `:411`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:465` |
   | 9. Conflito de leitores do dia | **Curado no trecho antigo por motivo.** A distinção entre zero perguntas e perguntas mortas continua coerente. | `app/chamados/management/commands/auditar_invariantes_chamados.py:50`, `:63`; `app/chamados/relatorios_disputa.py:13`, `:18` |
   | 10. Classes estruturais/metadados | **Parcial.** Saíram a derivação antiga e a lacuna específica de DS; continuam as ressalvas desta revisão. Pouso/pré-requisito: [nao verificado]. | `app/chamados/management/commands/auditar_invariantes_chamados.py:50`; `app/chamados/services/disputa_emissao.py:681`, `:690` |

   A grade ainda pode criar DS sem pergunta passando motivos vazios; a âncora continua tendo nascimento próprio e não examina o retorno de `abrir_conversa`. Esses dois pontos anteriores permanecem. (`app/ponto/management/commands/reconciliar_grade.py:424`, `:425`; `app/chamados/services/disputa_emissao.py:403`, `:1452`, `:1465`)

2. **Agregação, rollback, destino do motivo e reentrada**

   Na agregação, motivos/contexto são salvos e o gerador roda dentro do mesmo `atomic`. Se alguma pergunta devolvida pertence a outra DS, a exceção sai desse bloco: desfaz os updates do ensaio, perguntas novas e alterações transacionais feitas no reuso. A DS aberta preexistente e as perguntas preexistentes permanecem. (`app/chamados/services/disputa_emissao.py:547`, `:607`, `:609`, `:621`, `:673`)

   Depois, a recuperação reentra **na mesma porta** para cada dona. Motivos reusados seguem suas canônicas; os restantes voltam à aberta original. O contexto por motivo é filtrado por dona; chaves globais acompanham todas. A pergunta não troca de FK. (`app/chamados/services/disputa_emissao.py:616`, `:620`, `:688`, `:690`; `app/chamados/models.py:3008`)

   **Pode ficar em duas `DS.motivos`:** se a aberta já continha M antes da tentativa, o rollback restaura esse M; a recuperação acrescenta M à dona e não remove o anterior. Portanto isso não implementa transferência exclusiva de um motivo preexistente. Como `motivos` resume cenários, sem identificar dia/episódio, duas ocorrências do mesmo ID também não provam, isoladamente, duplicação do mesmo fato. (`app/chamados/services/disputa_emissao.py:578`, `:580`, `:621`, `:690`; `app/chamados/models.py:1743`)

   Em retorno normal, o particionamento conserva os motivos recebidos. Se a recuperação levantar, seu `atomic` desfaz as agregações: um motivo novo pode terminar em nenhuma DS porque o ato falhou. O retorno normal representa somente a primeira dona processada, mesmo quando outras também receberam motivos. (`app/chamados/services/disputa_emissao.py:676`, `:687`, `:695`, `:698`)

   **Pode ciclar até `RecursionError`.** O teste “a pergunta é da própria dona” impede apenas nova recuperação nessa situação; não há conjunto de visitadas nem limite para A→B→A. (`app/chamados/services/disputa_emissao.py:612`, `:621`, `:690`)

   Reprodução somente em memória, com núcleo/helper/gerador reais: `intervalo_saida`; A resolve D1 e possui pergunta local de D2; B resolve D2 e possui a canônica de D1; contexto recebido vazio preserva as datas distintas. A porta double, estável por dia, devolve B para D1 e A para D2. O dedup local não barra nenhum dos lados, e a execução termina em `RecursionError`. Reprodução com a porta ORM real: [nao verificado]. (`app/chamados/services/disputa_emissao.py:335`, `:361`, `:387`, `:585`, `:612`, `:690`; infraestrutura em `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:73`)

   **Dona fechada:** é selecionada explicitamente, sem filtro de abertura; recebe motivos/contexto, preservando `fechada_em` e a FK da pergunta. Porém seu `save` também aciona receivers: reconciliação do pai, retomada de regularizações e materialização de pendentes podem ocorrer. Os doubles não certificam esses efeitos. (`app/chamados/services/disputa_emissao.py:575`, `:583`, `:604`, `:607`; `app/chamados/signals.py:28`, `:42`, `:129`, `:144`, `:153`, `:163`)

3. **Ordem de trava**

   Nascimento, agregação e reentrada passam pela chamada de `:566`; recuperação passa por `:683`. Com emissão habilitada, ambas fornecem todas as DS do colaborador, inclusive fechadas. (`app/chamados/services/disputa_emissao.py:564`, `:566`, `:681`, `:683`)

   A primeira tabela efetivamente travada é **Colaborador**, depois DS, Chamado e perguntas explicitamente pedidas, cada grupo ordenado por PK. (`app/chamados/models.py:2666`, `:2675`, `:2688`, `:2692`)

   O QuerySet lazy dos IDs é avaliado em `:2671`, **depois de C e antes de D**. Nesse caminho `_universo` é verdadeiro, logo `:2674` faz curto circuito e não reitera `_pks(disputas)`. A reentrada cria outra consulta sob C já retido; isso não é dupla avaliação daquele mesmo QuerySet. (`app/chamados/models.py:2669`, `:2671`, `:2674`; `app/chamados/services/disputa_emissao.py:564`, `:690`)

   A declaração desliga a emissão e mantém sua trava externa antes da porta; agrupador e âncora continuam com suas entradas próprias. (`app/chamados/services/disputa_emissao.py:726`, `:1153`, `:1425`; `app/chamados/services/declaracao_texto.py:348`, `:352`, `:654`, `:656`)

   **A lacuna antiga D depois de Ch foi corrigida para as DS selecionadas, mas isso não prova toda a ordem por transação.** O topo inclui pais de DS abertas e chamados vivos; não inclui automaticamente destinos terminais de canônicas fechadas. No rechaveamento, `_nascer` pode pedir esse destino e a pergunta posteriormente. (`app/chamados/models.py:2677`, `:2679`, `:2683`, `:2940`, `:2947`, `:2949`, `:2950`)

   Assim, há caminho estático em que dois reusos/rechaveamentos no mesmo laço pedem uma nova linha de Chamado depois da pergunta do primeiro motivo — ou um Chamado de PK menor depois dos já adquiridos. Reprodução completa e deadlock efetivo são [nao verificado]; não declararia a ordem global inteira certificada. (`app/chamados/services/disputa_emissao.py:403`, `:609`, `:650`; `app/chamados/models.py:2947`, `:2950`)

4. **Fidelidade de `_resolver_pergunta_por_motivo`**

   Pelos hunks fornecidos da obra, o bloco extraído preserva os ramos anteriores: texto pelo catálogo; recuperação pela grade; data declarada do motivo; batida pelo turno; intervalo; derivação de destino; dedup; preferência de `_data_ev` sobre fallback. Comparação independente com o objeto Git `6c0517eb9`: [nao verificado]. (`app/chamados/services/disputa_emissao.py:308`, `:313`, `:335`, `:354`, `:358`, `:373`, `:387`)

   Os três `continue` internos tornaram-se `return None`: texto ainda impossível, intervalo sem dia/duplicado e destino duplicado. O gerador converte esse retorno novamente em `continue`. (`app/chamados/services/disputa_emissao.py:329`, `:364`, `:383`, `:435`, `:436`)

   Permanecem fora do helper as guardas de catálogo, dedup não datado, hora sem dia e conversa repetida. Portanto o helper não é, sozinho, uma resposta completa a “essa pergunta nascerá?”. Não identifiquei mudança de motivos emitidos causada pela extração isolada nos hunks apresentados. (`app/chamados/services/disputa_emissao.py:404`, `:415`, `:438`, `:449`)

   O comportamento anterior de dedup com `_data_ev=None` antes de aplicar fallback também foi conservado; a própria paridade espera repetição no cenário sem candidato. (`app/chamados/services/disputa_emissao.py:381`, `:387`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:439`, `:441`)

   O outro consumidor operacional do helper é o auditor: aceita `None`, lê `tupla[2]` e descarta dia `None`. O dia retornado é o do fato/turno resolvido; o dia civil da conversa só é escrito depois, por `abrir_conversa`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:50`, `:51`, `:52`, `:53`; `app/chamados/models.py:2801`)

5. **Fonte do dia e resultados de I7a/I7b**

   Sem IDs de órfãos, cada motivo consulta o mesmo helper do gerador. Saiu a precedência antiga “alguma data explícita → ultima_batida → pai/contexto”. (`app/chamados/management/commands/auditar_invariantes_chamados.py:49`, `:50`)

   **Sobra o ramo próprio dos IDs:** contexto da DS prevalece sobre o pai; IDs não vazios causam retorno antecipado, mesmo se nenhum chamado correspondente for encontrado. Nesse ramo, as fontes continuam sendo `data_do_chamado`, origem com `resolver_data_evento` e contexto sem origem. (`app/chamados/management/commands/auditar_invariantes_chamados.py:34`, `:38`, `:42`, `:44`, `:45`, `:46`)

   | Situação da DS aberta sem nenhuma pergunta | Resultado |
   |---|---|
   | Conjunto de dias vazio | **I7a**, sem chamar `_marcos_faltantes`. |
   | Um dia `sem_celula`, outro `lido` ou `fora_vinculo` | **I7a**. |
   | Conjunto não vazio, todos `sem_celula` | **I7b**. |
   | Helper retorna `None` para todos os motivos | **I7a**. |
   | Helper retorna `None` para um motivo e dia `sem_celula` para outro | **Pode dar I7b**, pois o motivo indeterminado desapareceu do conjunto. |

   As decisões da tabela vêm do descarte de `None` e de `bool(dias) and all(...)`; a última linha é uma insuficiência residual para afirmar que todos os motivos foram explicados pela falta de ata. (`app/chamados/management/commands/auditar_invariantes_chamados.py:51`, `:53`, `:69`, `:70`, `:72`)

   “SEM ata” reutiliza `sem_celula`, que também inclui ata sem lâmpadas. I7a é um grupo residual de responsabilidade ESTRUTURA: seu rótulo “COM ata” não prova existência de ata nos casos sem dia. (`app/chamados/services/declaracao_texto.py:93`, `:123`; `app/chamados/management/commands/auditar_invariantes_chamados.py:65`, `:69`, `:73`)

6. **Selo de derivação reescrito**

   Antes, conforme o diff fornecido, exigia somente a substring `data_pergunta=_data_fallback` no gerador. Agora exige a delegação literal ao helper e exatamente duas chamadas AST a `_derivar_chamado_destino`, ambas com esse argumento. O novo verifica as duas derivações, enquanto uma única substring bastava ao antigo. (`app/chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:138`, `:139`, `:143`, `:146`, `:148`; versão histórica local: [nao verificado])

   Continua sendo selo estrutural: não demonstra que o gerador utiliza o resultado corretamente. Por exemplo, delegar e depois sobrescrever destino/dia por uma terceira derivação sem data pode passar, porque a inspeção das chamadas ocorre somente dentro do helper. (`app/chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:139`, `:143`, `:145`; `app/chamados/services/disputa_emissao.py:437`)

   **Não demonstraria “antigo mata, novo deixa passar” usando literalmente o selo antigo sobre esta extração:** ele já reprovaria o controle correto, que deixou de conter aquela substring no gerador. Um mutante semanticamente equivalente antes/depois, com ambos os controles verdes e perda comprovada de mordida, é [nao verificado].

   A sonda chamada “selo original” executa o método **atual** lido do arquivo, não a versão histórica anterior à reescrita. (`app/chamados/tests/test_bo_disputa_volta3_memoria.py:346`, `:347`, `:349`, `:355`)

7. **Catraca de linhas do turno**

   O delta fornecido altera somente números dos mesmos dois sítios B1: `[778,779]→[809,810]` e `[1262]→[1293]`. Não acrescenta sítio permitido nem altera o critério; a cópia registra as novas linhas nos mesmos identificadores. (`app/core/tests/test_medida_turno_do_par.py:267`, `:269`, `:270`)

   A catraca verifica separadamente identidade dos sítios e linhas exatas. Os 35 testes passaram nesta sessão com a substituição de `SimpleTestCase` descrita na entrega; isso não certifica o runner Django. (`app/core/tests/test_medida_turno_do_par.py:20`, `:535`, `:541`; certificação Django: [nao verificado])

8. **Testes reais, `--base`, RED e vacuidade** [nao verificado]

   Executam os corpos extraídos dos arquivos por AST, sem reimplementação manual do núcleo/gerador/auditor. São funções recompiladas em outro escopo: ORM, transação, catálogo, juiz da ata e porta de pergunta são doubles. A sonda de trava extrai o método real de `models.py`. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:37`, `:70`, `:73`, `:89`, `:183`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:197`)

   **`--base` pertence à suite anterior:** busca emissor `6c0517eb9`, acrescenta helper atual quando ausente e usa auditor atual como sonda. A suite nova oferece `--antes`, com base `f0b2c84fa`, não `--base`. Seus testes de paridade também comparam com f0b, não diretamente com 6c. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:23`, `:29`, `:209`, `:210`, `:214`, `:473`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:26`, `:358`, `:457`)

   Para o caminho **(a)**:

   - `test_a_excecao_do_gerador_desfaz_agregado_e_preserva_batida`: a assertion de `:295` espera `(0,0)`; no fluxo antigo, DS/pai já concluídos antes da exceção produzem `(1,0)`. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:290`, `:295`)
   - `test_a_excecao_apos_inserir_pergunta_desfaz_tambem_a_pergunta`: exige positivamente pergunta antes da exceção; o contador pode continuar zero por essa pergunta existir, mas a contagem direta de tabelas em `:312` deve reprovar o resíduo antigo. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:300`, `:305`, `:311`, `:312`)

   Essa é a mordida indicada pelo código e pelo fluxo anterior anexado; execução dessas assertions contra o objeto Git real da base nesta cópia é [nao verificado].

   A declaração deixou de passar como no-op: exige retorno e gravações positivas. O `all` das travas agora tem guarda de conjunto não vazio. A paridade comportamental exige resultados esperados por cenário antes de comparar os dois geradores; as recusas vazias são deliberadas. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:330`, `:406`, `:411`, `:412`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:380`, `:433`, `:434`)

   Rodei 25 controles de memória nesta sessão, verdes. Os dois testes restantes de paridade histórica, `--base` e a certificação ORM ficaram [nao verificado]. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:284`; `app/chamados/tests/test_bo_disputa_volta2_memoria.py:141`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:221`, `:345`, `:357`, `:377`)

9. **Quem escreve, quem lê e significado dos campos**

   | Campo | Escritores | Leitores relevantes |
   |---|---|---|
   | `DS.motivos` | Núcleo; agrupador/âncora; portas de remoção/reabertura. | Gerador, auditor e relatório. (`app/chamados/services/disputa_emissao.py:583`, `:645`, `:1219`, `:1349`, `:1460`; `app/chamados/models.py:1966`, `:2063`, `:2159`; `app/chamados/management/commands/auditar_invariantes_chamados.py:49`; `app/chamados/relatorios_disputa.py:52`) |
   | `DS.contexto`, `motivo.data`, `qtd`, `ultima_em` | Núcleo; pendentes; portas de contexto. Recuperação filtra, mas reentra para gravar. | Texto/data do helper, grade e auditor. (`app/chamados/services/disputa_emissao.py:293`, `:296`, `:599`, `:600`, `:604`, `:646`, `:688`; `app/chamados/models.py:2104`, `:2122`; `app/chamados/services/disputa_emissao.py:308`, `:335`; `app/chamados/management/commands/auditar_invariantes_chamados.py:30`) |
   | `DS.chamado` e contexto do pai | Nascimentos e porta de ocorrência/contexto do chamado. | Grade, auditor, S127 e receivers. (`app/chamados/services/disputa_emissao.py:636`, `:642`, `:647`, `:1221`, `:1456`, `:55`; `app/chamados/management/commands/auditar_invariantes_chamados.py:31`; `app/chamados/juizes.py:2316`; `app/chamados/signals.py:32`) |
   | `DS.fechada_em` | Fecho e reaberturas existentes; esta cura não o escreve diretamente. | Seleção da aberta/dona, auditor e receivers. (`app/chamados/models.py:1932`, `:2149`, `:2173`; `app/chamados/services/disputa_emissao.py:571`, `:575`; `app/chamados/management/commands/auditar_invariantes_chamados.py:63`; `app/chamados/signals.py:129`) |
   | `Pergunta.disputa`, `motivo`, `data_evento`, `chamado_destino`, `conversa`, via | Porta de nascimento; dia pelo ponto canônico; reuso pode rechavear/costurar preservando dona. | Dedup, detector estrangeiro, auditor e leitores do fio. (`app/chamados/models.py:2947`, `:2982`, `:3008`, `:3014`, `:3016`; `app/chamados/services/disputa_emissao.py:361`, `:381`, `:612`, `:613`; `app/chamados/management/commands/auditar_invariantes_chamados.py:63`) |
   | I7a/I7b | Auditor calcula listas e `add` produz número/amostra. | stdout, HTML e execução programada. (`app/chamados/management/commands/auditar_invariantes_chamados.py:73`, `:74`, `:632`, `:1911`, `:1918`; `app/config/crons.py:542`) |

   Os insumos de origem continuam vindo dos produtores existentes: API grava `entrada_sem_saida`/`ultima_batida`; PWA e fábrica fornecem data por motivo; agrupador grava IDs de órfãos; detector transporta entradas pendentes. Censo exaustivo de todos os produtores: [nao verificado]. (`app/api/views_core.py:669`, `:788`; `app/ponto/services/triagem_batida.py:738`; `app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/services/disputa_emissao.py:1220`; `app/chamados/detector_anomalias.py:50`)

   **Leitores com pressupostos relevantes:** o wrapper recebe uma única DS, mas ela agora pode ser fechada ou apenas a primeira dona de um lote. Mesmo assim chama pendentes e, havendo motivos novos, envia “Nova pendência… Toque para responder”, sem guarda de `fechada_em` nessa função de push. Entrega efetiva de notificação é [nao verificado]. (`app/chamados/services/disputa_emissao.py:695`, `:698`, `:779`, `:781`, `:1845`, `:1859`, `:1861`)

   `data_evento` continua tendo domínio explicitado por `conversa`: turno para fato e dia civil para conversa. O helper não deve ser lido com

[truncado em 20000 de 22660 caracteres]

## 10/11 18:36 · sessao 40458ebcdfa0 · - · - · origem=Ronald (tmux)

**Agregação estrangeira está curada para motivos novos, mas há duas ressalvas reproduzidas: motivo preexistente fica em duas DS; reentrada pode ciclar.** (`app/chamados/services/disputa_emissao.py:578`, `:612`, `:620`, `:690`)

- O ensaio salva motivos/contexto e executa todo o gerador dentro do `atomic`; só depois examina as perguntas devolvidas. A exceção estrangeira desfaz essas gravações, as perguntas novas e alterações transacionais do reuso. A DS aberta preexistente e as perguntas preexistentes permanecem. (`app/chamados/services/disputa_emissao.py:547`, `:607`, `:609`, `:621`, `:673`)
- A recuperação filtra as chaves por motivo, reentra na mesma porta para cada dona e mantém os motivos não reusados na aberta original. As chaves globais do contexto acompanham todas as donas; `contexto_nascimento` é passado inteiro, mas a reentrada seleciona uma DS existente. (`app/chamados/services/disputa_emissao.py:616`, `:620`, `:688`, `:690`, `:694`, `:575`)
- A primeira dona estrangeira vira o retorno da função; outras donas também recebem seus motivos. Portanto o retorno não representa necessariamente todo o lote. (`app/chamados/services/disputa_emissao.py:687`, `:695`, `:698`)
- **Duas DS.motivos:** se a aberta já possuía M antes da chamada, o rollback restaura esse M; a recuperação agrega M à dona e não remove o M antigo. Reproduzi com `Ambiente` atual e gerador real: ambas as DS terminam com `['batida_fora_escala_grave']`. A demonstração usa doubles; ocorrência real em banco [nao verificado]. (`app/chamados/services/disputa_emissao.py:578`, `:580`, `:621`, `:690`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:73`)
- **Ciclo:** não existe conjunto de visitadas, limite nem estabilização na recursão. Dona própria encerra porque não satisfaz `pergunta.disputa_id != disputa_aberta.pk`; isso não impede A→B→A quando cada dona resolve outro dia. (`app/chamados/services/disputa_emissao.py:612`, `:621`, `:690`)
- Reproduzi ciclo com **intervalo_saida**, sem episódio, helper/gerador/núcleo reais: A aberta tem contexto M.data=D1 e pergunta local de D2; B fechada tem contexto M.data=D2 e pergunta local de D1. Porta double estável por dia devolve a canônica de D1/B ou D2/A; `_derivar_chamado_destino` devolve `(None,None)`, sem alterar o dia intervalar. `emitir([M], contexto={})` termina em `RecursionError` após 139 chamadas com limite 160. O dedup não barra: cada DS só possui pergunta do dia oposto ao pedido. PostgreSQL [nao verificado]. (`app/chamados/services/disputa_emissao.py:334`, `:360`, `:370`, `:387`, `:434`, `:612`, `:690`)
- A dona fechada é selecionada sem filtro de abertura e recebe apenas `motivos`/`contexto`; seu `fechada_em` e a FK da pergunta ficam preservados. Entretanto salvar DS fechada dispara receivers que não exigem alteração de `fechada_em`: S88 pode reconciliar o pai e a retomada de regularizações agenda callback. Esses efeitos não existem nos doubles da sonda. (`app/chamados/services/disputa_emissao.py:575`, `:583`, `:604`, `:607`; `app/chamados/signals.py:28`, `:42`, `:129`, `:144`)
- Se a operação termina normalmente, os motivos novos ficam na dona ou na aberta original conforme o particionamento. Exceção posterior na recuperação desfaz sua transação; motivo novo pode terminar em nenhuma DS porque o ato inteiro falhou. Não há garantia de sucesso no ciclo. (`app/chamados/services/disputa_emissao.py:676`, `:687`, `:690`, `:698`)

**Travas do núcleo/reentrada/recuperação:**

- Entrada normal e reentrada passam todas as DS do colaborador, abertas e fechadas, por QuerySet lazy; recuperação agora passa o mesmo universo antes de recursar. A lacuna anterior “fechada adicional adquirida depois de Ch” foi curada para essas DS. (`app/chamados/services/disputa_emissao.py:564`, `:566`, `:681`, `:683`, `:690`)
- Primeira tabela é **Colaborador**, depois avaliação dos IDs, depois DS por PK, depois Ch por PK, depois perguntas explicitamente pedidas. (`app/chamados/models.py:2666`, `:2671`, `:2675`, `:2688`, `:2692`)
- Com `abertas_do_colaborador=True` e colaborador válido, `_universo` é verdadeiro; a condição em 2674 faz curto-circuito e não reitera `_pks(disputas)`. O QuerySet é avaliado em 2671, uma vez nessa chamada, entre C e D. A reentrada cria outro QuerySet sob a mesma transação, mas C já está retido; releitura adicional não é dupla avaliação do mesmo QuerySet entre duas travas. (`app/chamados/models.py:2669`, `:2671`, `:2674`; `app/chamados/services/disputa_emissao.py:564`, `:690`)
- `emitir_perguntas=False` passa a dona explícita, se houver, e abertas; não toma todas as fechadas. Declaração tem trava externa própria antes da porta e da pergunta. (`app/chamados/services/disputa_emissao.py:556`, `:557`, `:726`; `app/chamados/services/declaracao_texto.py:348`, `:352`, `:360`, `:376`)
- **Ressalva da ordem global completa:** todas as DS estão previamente travadas, porém Ch inclui apenas pais das DS **abertas** e Ch **VIVOS**. Destino terminal de canônica fechada não entra automaticamente. (`app/chamados/models.py:2677`, `:2679`, `:2681`, `:2683`)
- No reuso com rechave, `_nascer` pode adquirir `ex.chamado_destino_id` terminal e a pergunta. Como o atomic novo engloba o laço inteiro do gerador, dois reusos/rechaves sucessivos podem pedir Ch terminal novo depois da P do primeiro motivo. Isso é caminho estático plausível; reprodução completa com ORM/portas reais [nao verificado]. Não afirmar ausência global de Ch após P. (`app/chamados/models.py:2940`, `:2947`, `:2949`, `:2950`; `app/chamados/services/disputa_emissao.py:403`, `:609`)
- Reuso sem rechave costura o destino pedido por `_costura_nao_mata_o_fio`; esse destino vem do derivador de Ch vivos, já coberto pelo topo. (`app/chamados/services/disputa_emissao.py:198`, `:200`; `app/chamados/models.py:2980`; `app/chamados/services/fio.py:768`)
- Permanecem gerações posteriores ao núcleo em pendentes e fábrica, sem participação no novo detector estrangeiro. (`app/chamados/services/disputa_emissao.py:779`, `:294`; `app/chamados/services/fabrica_da_celula.py:172`, `:174`)

## 10/11 18:35 · sessao e9fc4319af12 · - · - · origem=Ronald (tmux)

4. **Extração fiel no trecho mostrado do diff da obra.** O antigo bloco da base 6c `disputa_emissao.py:344–424` corresponde ao helper atual `:307–387`; as recusas por texto sem data, intervalo sem dia/duplicado e destino duplicado viraram `return None` em `:329`, `:364`, `:383`. A seleção de `_data_ev` antes do fallback continua em `:387`; o retorno inclui texto/destino/dia em `:388`. A comparação independente com o objeto Git 6c é [nao verificado].

As guardas do gerador continuam fora do helper: motivo ausente do catálogo (`app/chamados/services/disputa_emissao.py:404`), dedup não datado (`:415`), hora sem dia (`:438`) e conversa repetida (`:449`). Portanto o helper não representa sozinho todas as condições de nascimento; a chamada do gerador conserva essas guardas e transforma `None` em `continue` (`:434–443`).

O outro consumidor operacional é o auditor: aceita `None`, acrescenta apenas `resolvida[2]`, e elimina dia `None` (`app/chamados/management/commands/auditar_invariantes_chamados.py:49–53`). O dia é o turno/fato fornecido pelas fontes; conversas recebem dia civil posteriormente na porta `abrir_conversa` (`app/chamados/models.py:2801`), não no helper.

5. **A precedência paralela dos motivos saiu; sobra o ramo próprio dos IDs.** IDs do contexto da DS prevalecem sobre os do pai; IDs não vazios causam retorno antecipado, mesmo sem registros encontrados (`app/chamados/management/commands/auditar_invariantes_chamados.py:34`, `:45`). Nesse ramo os dias vêm de `data_do_chamado`, `resolver_data_evento` com origem ou `data_evento_do_contexto` (`:37–44`). Fora dele, cada motivo chama o helper (`:49–52`).

Resultados por DS aberta sem nenhuma linha de pergunta:

- Dias vazios: I7a; juiz não é chamado pelo curto circuito de `bool(dias)` (`app/chamados/management/commands/auditar_invariantes_chamados.py:69`).
- Dias mistos, um `sem_celula` e outro `lido`/`fora_vinculo`: I7a (`:69–72`).
- Todos os dias retornados `sem_celula`, com conjunto não vazio: I7b (`:69–74`).
- Helper `None` em todos os motivos: I7a; `None` é descartado (`:51–53`).
- **Helper `None` em um motivo e dia `sem_celula` em outro: pode dar I7b.** O motivo recusado não deixa marca de indeterminação no conjunto (`:51–53`, `:69`). Reproduzido localmente com funções atuais e doubles; runtime/produção [nao verificado].

`sem_celula` inclui ata sem lâmpadas, enquanto `fora_vinculo` tem significado distinto; o auditor reutiliza essa autoridade (`app/chamados/services/declaracao_texto.py:91–96`, `:123–126`; `app/chamados/management/commands/auditar_invariantes_chamados.py:70`).

9. Campos e consumidores relevantes:

| Campo | Escritores | Leitores/semântica |
|---|---|---|
| DS.motivos | Núcleo `app/chamados/services/disputa_emissao.py:583`, `:645`; agrupador `:1219`, `:1225`, `:1254`, `:1349`; remoção/reabertura `app/chamados/models.py:1966`, `:2063`, `:2159` | Catálogo define “cenários ativos” em `app/chamados/models.py:1743`; auditor `app/chamados/management/commands/auditar_invariantes_chamados.py:49`; relatório `app/chamados/relatorios_disputa.py:52`. |
| DS.contexto, motivo.data, qtd, ultima_em | Núcleo `app/chamados/services/disputa_emissao.py:599–607`, `:646`; pendentes `:293–296`; outras portas `app/chamados/models.py:1967`, `:2064`, `:2104`, `:2122` | Helper texto/data `app/chamados/services/disputa_emissao.py:308`, `:335`; grade funde contexto do pai `:54–58`; auditor `app/chamados/management/commands/auditar_invariantes_chamados.py:30–34`. Helper só monta cópia local em `app/chamados/services/disputa_emissao.py:315–316`. |
| DS.chamado / contexto_json do pai | Núcleo `app/chamados/services/disputa_emissao.py:636`, `:647`; FK aceita SET_NULL `app/chamados/models.py:1749–1752` | Grade `app/chamados/services/disputa_emissao.py:55–58`; auditor `app/chamados/management/commands/auditar_invariantes_chamados.py:31–34`; S127 `app/chamados/juizes.py:2316`. |
| DS.fechada_em | Fecho/reaberturas `app/chamados/models.py:1932`, `:2149`, `:2173` | Núcleo seleciona aberta em `app/chamados/services/disputa_emissao.py:571` e dona sem filtro em `:575`; auditor só abertas `app/chamados/management/commands/auditar_invariantes_chamados.py:63`. |
| Pergunta.disputa/motivo/destino/data | `_nascer` cria em `app/chamados/models.py:3014`; dia passa por `gravar_dia_do_fato` em `:3016`; reuso retorna existente em `:3008` | Detector lê dona/motivo `app/chamados/services/disputa_emissao.py:612–613`; helper dedup lê dia/motivo/via/conversa `:361–382`; auditor conta ausência de qualquer linha `app/chamados/management/commands/auditar_invariantes_chamados.py:63`. |

“Sem pergunta” significa zero linhas, consistente com relatório (zero em `app/chamados/relatorios_disputa.py:12–14`; só mortas em `:18–20`), fechamento VAZIA/VEREDITO (`app/chamados/models.py:1903`) e bloqueio S127 (`app/chamados/juizes.py:2319–2320`).

10. **Relógio indireto merece ressalva.** O auditor chama `cobra_marcos_da_ata` sem `hoje` (`app/chamados/management/commands/auditar_invariantes_chamados.py:37`); esse predicado lê `_tz.localdate()` (`app/chamados/services/disputa_emissao.py:838`). Já existia na volta 2, portanto não é introdução do delta v3; integra o caminho adicionado pela obra inteira. Não afirmar classificação I7 inteiramente independente do relógio.

Não localizei autoridade duplicada pela extração: o helper conserva os leitores, e o auditor reutiliza `_marcos_faltantes` (`app/chamados/services/disputa_emissao.py:313`, `:354`, `:372`, `:375`; `app/chamados/management/commands/auditar_invariantes_chamados.py:70`). MAPA continua atribuindo dia do chamado a `data_do_chamado` e fechamento a `pode_fechar` (`app/docs/MAPA.md:163`, `:165`); não há divergência específica desses endereços causada pelo helper. Pouso, pré-requisito e certificação são [nao verificado].

## 10/11 18:35 · sessao f6ffe75e9181 · - · - · origem=Ronald (tmux)

6. O selo anterior exigia só a substring `data_pergunta=_data_fallback` dentro do gerador; isso consta no hunk fornecido de `diff_v3.patch` [nao verificado quanto ao arquivo histórico local]. O atual exige a delegação literal ao helper e exatamente duas chamadas AST a `_derivar_chamado_destino`, ambas com argumento `data_pergunta` cujo valor é `_data_fallback`. (`app/chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:138`, `:143`, `:146`, `:148`)

   Para os dois sítios extraídos, a exigência de encaminhamento ficou mais estrita: o antigo podia deixar uma chamada sem data enquanto outra preservava a substring; o atual verifica ambas. Entretanto continua sendo teste estrutural: pode passar se o gerador mantiver a delegação e depois sobrescrever destino/dia com uma terceira chamada a `_derivar_chamado_destino(disputa, motivo)` sem data. O selo só inspeciona chamadas dentro do helper. (`app/chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:139`, `:143`, `:145`; desempacotamento real em `app/chamados/services/disputa_emissao.py:436`)

   **Cuidado na comparação pedida:** executar literalmente o selo antigo sobre o código extraído já reprova o controle correto; logo não é uma prova pareada de “controle verde e mutante RED”. Um mutante semanticamente equivalente antes/depois da extração, morto apenas pelo antigo, não foi demonstrado nesta cópia. [nao verificado] [nao verificado]

7. O delta fornecido muda somente os valores das linhas dos mesmos dois identificadores B1: `_orfao_resolvido_cross_meianoite` e `criar_disputa_a_partir_de_chamados_orfaos`; não acrescenta sítio permitido. A cópia registra `[809,810]` e `[1293]`. A catraca mantém verificações separadas da identidade e das linhas. (`app/core/tests/test_medida_turno_do_par.py:269`, `:270`, `:535`, `:541`)

8. Os testes recompilam por AST os corpos lidos dos arquivos, em vez de reimplementar manualmente núcleo/gerador/auditor. Fronteiras ORM, atomic, catálogo, ata e porta de pergunta são doubles. A trava da volta 3 é extraída do método real de `PerguntaDisputa`. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:37`, `:70`, `:73`, `:89`, `:183`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:197`)

   `--base` pertence à suite anterior: emissor obtido por `git show` da base 6c, acrescido do helper atual quando ausente, com auditor atual como sonda. A suite nova possui `--antes`, referindo-se a f0b; não possui `--base`. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:23`, `:29`, `:209`, `:210`, `:214`, `:473`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:26`, `:457`)

   No caminho (a), a primeira exceção confronta I7a/I7b com `(0,0)` em `:295`; no emissor antigo que já concluiu núcleo antes da exceção, espera-se falha por `(1,0)`. A segunda exceção exige positivamente uma pergunta antes de levantar (`:305`); como a pergunta existente exclui a DS do contador, a assertion decisiva é a contagem direta vazia em `:312`. Execução dessas assertions contra o histórico real 6c nesta cópia: [nao verificado]. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:290`, `:295`, `:300`, `:305`, `:311`, `:312`)

   A vacuidade apontada na declaração foi curada: exige retorno, DS/pai gravados, motivo/contexto/descrição e contagens positivas, além de ausência de geração. A assertion `all(...)` de travas agora tem guarda de não vazio. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:330`, `:331`, `:406`, `:408`, `:410`, `:411`, `:414`)

   A paridade não passa simplesmente por ambos os geradores devolverem vazio: confronta antes o resultado com dia/pergunta esperado por cenário; três casos de recusa esperam vazio deliberadamente. O teste do selo extraído apenas inspeciona código, sem executar geração. (`app/chamados/tests/test_bo_disputa_volta3_memoria.py:380`, `:433`, `:434`, `:345`, `:355`)

   Os modos de mutação exigem controle verde, pelo menos uma falha por assertion e nenhum erro de montagem. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:477`, `:481`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:462`, `:465`)

Execução somente em memória nesta revisão: 23 testes verdes, abrangendo suite anterior, `Volta3Test` e selo extraído; sem banco/Git histórico. [nao verificado quanto à certificação Django/PostgreSQL] [nao verificado]

## 10/11 18:29 · sessao 1510caea65c6 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

VOLTA 3 implementada; cura, comandos e rastreio na [entrega](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:3).

- [27 testes de memória verdes](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:136) e [35 da catraca verdes](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:184). [nao verificado]
- [RED contra a base: 6 assertions; sete mutantes novos mortos](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:142).
- [Registro de linhas ajustado; tetos preservados](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:85).

SQL/ORM pelo Code e certificação final pelo Opus: **[não verificado]**. [FALTA](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:244).

_3455 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
