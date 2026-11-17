# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 07:32** · publicado em **17/11/2026 07:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 07:32 · sessao c72a3ef3dbe3 · CORTE-B2-MEIA-RESPOSTA · leitura · origem=Code

**nao existe**, entre as portas da cópia examinada, uma que reúna: decidir a meia-resposta de competência **ENTREGUE**, registrar a decisão com quem/quando/antes/depois/motivo e encaminhar a pergunta à lista do DP. As portas de validação não aceitam essa resposta; a correção L-130 recusa entregue; os escritores de Pauta não decidem a pergunta. (`app/chamados/services/validacao.py:681`, `app/chamados/services/validacao.py:901`, `app/chamados/services/validacao.py:403`, `app/pautas/services.py:180`.)

**Há uma diferença essencial entre a premissa de produção e esta cópia B2:** aqui, a meia-resposta **já não integra** `respostas_sem_veredito`: o corpo exige `pergunta_respondida`, isto é, instante **E** texto após trim. A retenção da retratação delega ao mesmo corpo. Essa saída acontece por leitura, sem gravar decisão ou Pauta. (`app/chamados/models.py:1933`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:2117`.)

Os chamadores abaixo são referências executáveis encontradas no código; sua execução efetiva em produção é **[nao verificado]**. PKs, quantidades e efeitos atuais em CH23072/CH33525 são **[nao verificado]**. A anotação de sombra existente em `app/chamados/juizes.py:2107` não fornece registros de produção verificáveis nesta leitura.

**Portas da validação e L-130**

| Porta e chamadores no código | Escrita e trilha | Entregue, destino e reversão |
|---|---|---|
| **`validar_pergunta`**, `app/chamados/services/validacao.py:661`. Chamada pela casca HTTP, foto, declaração textual e comando classe A: `app/chamados/services/acoes_disputa.py:19`, `app/api/foto_recebida.py:51`, `app/chamados/services/declaracao_texto.py:697`, `app/chamados/management/commands/validar_classe_a.py:100`. | Para meia-resposta, retorna `sem_resposta` antes de escrever. Nos casos aceitos, grava `validada_em`, `validada_por`, decisão `VALIDACAO`, aciona materialização e auditoria. (`app/chamados/services/validacao.py:681`, `app/chamados/services/validacao.py:168`, `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:769`, `app/chamados/services/validacao.py:796`.) | A guarda K6 específica consulta metades de intervalo **fora U163**; não é veto universal de toda pergunta entregue. A frase “preservada para Pauta DP” é retorno textual, sem criação de Pauta. Devolução da validação recusada ocorre pela porta seguinte; restauração exata posterior **[nao verificado]**. (`app/chamados/services/materializacao.py:792`, `app/chamados/services/materializacao.py:812`, `app/chamados/services/validacao.py:781`.) |
| **`devolver_validacao_recusada`**, `app/chamados/services/validacao.py:237`. Chamada por `validar_pergunta`, em `app/chamados/services/validacao.py:781`. | Exige resposta de hora declarada, não materializada e motivo preenchido; texto vazio não satisfaz esse escopo. Zera `validada_em/por`, preserva resposta, registra `DEVOLUCAO` e evento `validacao_devolvida` com antes/depois/ator. Pode reabrir disputa. (`app/chamados/services/validacao.py:193`, `app/chamados/services/validacao.py:264`, `app/chamados/services/validacao.py:273`, `app/chamados/services/validacao.py:279`, `app/chamados/services/validacao.py:291`, `app/chamados/services/validacao.py:297`.) | Não tem guarda K6 própria; o chamador consulta a entrega antes e depois da materialização. Destino é o admin, sem Pauta. Nova validação passa novamente por `validar_pergunta`; inversa de restauração exata **[nao verificado]**. (`app/chamados/services/validacao.py:289`, `app/chamados/services/validacao.py:740`, `app/chamados/services/validacao.py:777`, `app/chamados/services/validacao.py:661`.) |
| **`recusar_pergunta`**, `app/chamados/services/validacao.py:858`. Casca em `app/chamados/services/acoes_disputa.py:82`, chamada pelos cards e fio: `app/chamados/views.py:518`, `app/chamados/services/fio.py:684`. | Para meia-resposta, retorna `ja_pendente` antes de escrever. Para resposta completa, grava `recusa_motivo`, chama reabertura com ato `RECUSA`, registra foto anterior na sequência, nota e auditoria com antes/depois. (`app/chamados/services/validacao.py:901`, `app/chamados/services/validacao.py:916`, `app/chamados/services/validacao.py:925`, `app/chamados/services/disputa_emissao.py:1631`, `app/chamados/services/validacao.py:930`, `app/chamados/services/validacao.py:934`.) | O corpo não consulta K6; o destino declarado é **colaborador**, com push para responder novamente. Não cria Pauta DP. Inversa dedicada **[nao verificado]**. (`app/chamados/services/validacao.py:883`, `app/chamados/services/validacao.py:945`, `app/chamados/services/validacao.py:953`.) |
| **`devolver_paridade_bug_par_103`**, `app/chamados/services/validacao.py:308`. Chamada pelo comando `app/chamados/management/commands/bug_par_103.py:106`. | Exige o caso específico de paridade absolvida. Limpa `materializada_em/por`, `via_resolucao`, `validada_em/por`; conserva resposta e motivo. Grava `DEVOLUCAO` e evento com antes/depois/ator; falha da auditoria desfaz o ato. (`app/chamados/services/validacao.py:334`, `app/chamados/services/validacao.py:346`, `app/chamados/services/validacao.py:351`, `app/chamados/services/validacao.py:354`, `app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:366`.) | **Recusa entregue e sem dia do fato**. Volta ao admin para validar o par, sem reabrir disputa nem criar Pauta. O comando salva JSON anterior; restaurador dedicado **[nao verificado]**. (`app/chamados/services/validacao.py:338`, `app/chamados/services/validacao.py:315`, `app/chamados/management/commands/bug_par_103.py:64`, `app/chamados/management/commands/bug_par_103.py:95`.) |
| **`impedimento_estoque_l130`**, `app/chamados/services/validacao.py:381`. Lido pelo comando na seleção/releitura e pela porta: `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:42`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:99`, `app/chamados/services/validacao.py:432`. | É predicado: retorna impedimento, sem escrever pergunta, trilha ou Pauta. Exclui validada, materializada, fora do fato, morta pelo C6 e sem dia. (`app/chamados/services/validacao.py:390`.) | **Retorna `competencia_entregue`** quando K6 confirma entrega. Não há ato para reverter. (`app/chamados/services/validacao.py:400`.) |
| **`devolver_instante_sem_texto_l130`**, `app/chamados/services/validacao.py:408`. Chamada pelo comando em `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:105`. | Preserva projeção anterior em `DecisaoPergunta`; zera `respondida_em/resposta_colab` pela porta de lote, que também limpa `materializada_em/por/via_resolucao`; acorda células. Grava `l130_instante_sem_texto_devolvida` com antes/depois/ator. Falta de trilha desfaz o atomic. Não cria ato `REABERTURA/DEVOLUCAO`. (`app/chamados/services/validacao.py:417`, `app/chamados/services/validacao.py:439`, `app/chamados/services/validacao.py:440`, `app/chamados/models.py:3402`, `app/chamados/services/validacao.py:446`, `app/chamados/services/validacao.py:449`, `app/chamados/services/validacao.py:455`.) | **Recusa entregue**, relendo o impedimento sob trava. Destino é pendente de competência aberta; não escreve Pauta. Reposição literal da meia-resposta pela porta seria recusada como par ilegal. (`app/chamados/services/validacao.py:428`, `app/chamados/services/validacao.py:432`, `app/chamados/services/validacao.py:450`, `app/chamados/models.py:3386`.) |
| **Comando `estoque_l130_instante_sem_texto`**, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:30`. Invocação operacional efetiva **[nao verificado]**. | DRY imprime elegíveis/impedimentos. APPLY exige exatamente os alvos **38251/38252**, salva JSON anterior e chama a porta; divergência na quantidade desfaz o lote. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:47`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:54`, `app/chamados/services/validacao.py:378`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:65`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:111`.) | Herda a recusa de entregue; não envia ao DP. O JSON contém foto anterior, mas os argumentos disponíveis são autor/pasta/apply, sem restauração. (`app/chamados/services/validacao.py:403`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:57`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:25`.) |

A conferência de data também está em `validacao.py`: grava apenas `data_conferida_em/por` e auditoria da divergência; é chamada pela view e não decide o par nem cria Pauta. (`app/chamados/services/validacao.py:957`, `app/chamados/services/validacao.py:996`, `app/chamados/services/validacao.py:999`, `app/chamados/views.py:1313`.)

A trilha comum escreve `LogAuditoria` com usuário, descrição, antes/depois, IP e ator; `criado_em` registra quando. `evento` é best effort; as portas L-130 e BUG-PAR verificam seu retorno e abortam sem trilha. (`app/core/observ.py:94`, `app/core/observ.py:134`, `app/core/models.py:295`, `app/core/models.py:307`, `app/chamados/services/validacao.py:455`, `app/chamados/services/validacao.py:366`.)

**Outras candidatas e as listas do DP existentes**

| Candidata/lista | O que faz e o que falta |
|---|---|
| **`reabrir_para_correcao`**, `app/chamados/services/disputa_emissao.py:1576`. | Zera resposta, instante, validação e carimbo; grava decisão com foto, autor/motivo quando fornecidos. Não contém veto K6 nem encaminhamento DP. Chamadores incluem recusa, ausência rejeitada e reabertura do questionário. A última devolve ao colaborador. Restaurador exato **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1609`, `app/chamados/services/disputa_emissao.py:1631`, `app/chamados/services/disputa_emissao.py:1648`, `app/chamados/services/validacao.py:925`, `app/chamados/services/materializacao.py:1167`, `app/chamados/services/acoes_disputa.py:106`, `app/chamados/services/acoes_disputa.py:109`.) |
| **`fechar_por_admin` / Resolver dia**. | `fechar_por_admin` grava `materializada_em/por` e `via_resolucao=admin_decidiu`, sem corrigir o par ou consultar K6. Resolver dia chama essa porta e acrescenta `SILENCIO` com autor/instante/motivo; sua auditoria é do dia. Não há encaminhamento obrigatório da pergunta ao DP. Reversão técnica do trio é `descarimbar`; inversa completa do dia **[nao verificado]**. (`app/chamados/models.py:3171`, `app/chamados/models.py:3306`, `app/ponto/services/veredito_celula.py:190`, `app/ponto/services/veredito_celula.py:195`, `app/ponto/services/veredito_celula.py:356`, `app/chamados/models.py:3324`.) |
| **`DisputaSupervisao.fechar` / `admin_encerra`**. | `fechar` lê respostas sem veredito; nas exceções desligamento/admin, carimba perguntas vivas, grava `SILENCIO`, eventos e `fechada_em/por/resolucao/motivo_fechamento`. Não corrige o par nem cria Pauta. Desligamento chama esse fluxo; caller efetivo de `admin_encerra` em produção **[nao verificado]**. Há reabertura da disputa, sem inversa integral demonstrada dos efeitos **[nao verificado]**. (`app/chamados/models.py:1977`, `app/chamados/models.py:2009`, `app/chamados/models.py:2023`, `app/chamados/models.py:2034`, `app/colaboradores/services/desligamento.py:165`, `app/chamados/models.py:2217`.) |
| **Pauta genérica, aba `dp`**. | `escrever` cria remetente/destinatário, âncora, texto, prazo, pai e autor; registra trilha e contador. O DP lê pelo filtro departamental da aba. Não altera `PerguntaDisputa`; marcar feita também só altera a Pauta. Reversão: autor pode cancelar pauta ainda não feita, com trilha; isso não reverte pergunta. (`app/pautas/services.py:153`, `app/pautas/services.py:180`, `app/pautas/services.py:304`, `app/pautas/services.py:311`, `app/pautas/services.py:247`, `app/pautas/services.py:281`, `app/pautas/services.py:380`.) |
| **Pauta `[TRANCA]`, empresa × competência**. | `competencia_trancada.encerrar` exige competência **trancada**, seleciona `decidir_pelo_dp` por `respostas_sem_veredito(ignorar_tranca=True)` e escreve Pauta DP. Nesta B2, meia-resposta não entra na seleção. Não escreve a pergunta; a retenção da retratação ignora tranca. Chamadores: fechamento e comando. Reabertura restaura efeitos da tranca; não cancela sua Pauta. (`app/chamados/services/competencia_trancada.py:95`, `app/chamados/services/competencia_trancada.py:126`, `app/chamados/models.py:1933`, `app/chamados/services/competencia_trancada.py:203`, `app/chamados/services/competencia_trancada.py:256`, `app/chamados/juizes.py:2117`, `app/ponto/services/fechamento.py:1427`, `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`, `app/chamados/services/competencia_trancada.py:260`.) |
| **Pauta `[FIO-E0-V4-E4B-S143]`**. | `transicionar_com_pauta` coleta recusas de reabertura do credor durante fechamento, agrupa por competência e cria Pauta DP. É chamado, por exemplo, por `_retratar` e Resolver dia. Não é decisão da meia-resposta nem correção do par. Cancelamento segue a porta genérica da Pauta; inversa integral da transição **[nao verificado]**. (`app/chamados/services/fecho_runtime.py:16`, `app/chamados/services/fecho_runtime.py:119`, `app/chamados/services/fecho_runtime.py:139`, `app/chamados/services/fecho_runtime.py:146`, `app/chamados/reconciliador.py:525`, `app/ponto/services/veredito_celula.py:203`, `app/pautas/services.py:281`.) |
| **“PAUTA DP” do comando BUG-PAR**. | É lista Python impressa no stdout, inclusive entregues/sem dia; entregues são preservadas. Não cria `Pauta` nem decide essas perguntas. Sem escrita dessas linhas, não há ato sobre elas para reverter. (`app/chamados/management/commands/bug_par_103.py:63`, `app/chamados/management/commands/bug_par_103.py:70`, `app/chamados/management/commands/bug_par_103.py:85`, `app/chamados/management/commands/bug_par_103.py:122`.) |
| **Worklist DP**. | Somente coleta gaps e imprime por posto/colaborador/dia; não grava decisão ou encaminhamento de pergunta. (`app/ponto/management/commands/worklist_dp.py:5`, `app/ponto/management/commands/worklist_dp.py:12`, `app/ponto/management/commands/worklist_dp.py:22`.) |

Outros produtores de Pauta DP têm recortes próprios: limbo exportado por dia e cadastro sem turno por colaborador; resposta tardia exige texto livre novo; reabertura de órfãs exportadas e passivo validado produzem pautas específicas. Nenhum desses trechos constitui decisão da meia-resposta examinada. (`app/chamados/services/fabrica_da_celula.py:279`, `app/chamados/services/fabrica_da_celula.py:369`, `app/chamados/services/resposta_tardia.py:59`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:208`, `app/chamados/services/passivo_validado.py:149`.)

Também existem pautas de competência para pago × devido, folga não definida exportada e dia em aberto; são escritas pelo serviço de Pautas e não corrigem o par da pergunta nesses pontos. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:134`, `app/pautas/management/commands/pauta_dia_em_aberto.py:139`.)

**Quem escreve os dois campos**

| Escritor | Campos e origem |
|---|---|
| Resposta normal, `responder_pergunta_disputa_core` | Escreve ambos; salva e registra `RESPOSTA` com autor/instante. Chamam web, API, correção administrativa e declaração textual. (`app/chamados/services/disputa_emissao.py:1481`, `app/chamados/services/disputa_emissao.py:1522`, `app/chamados/services/disputa_emissao.py:1525`, `app/chamados/views.py:1284`, `app/api/views.py:2629`, `app/chamados/services/acoes_disputa.py:375`, `app/chamados/services/declaracao_texto.py:694`.) |
| Resposta automática | Escreve ambos, após validar integridade; chamada pelo recebimento de foto. (`app/chamados/services/disputa_emissao.py:1672`, `app/chamados/services/disputa_emissao.py:1674`, `app/api/foto_recebida.py:48`.) |
| `carimbar_resposta_pela_trilha` | Só preenche `respondida_em` quando já existe texto; comando de cura fornece instante da nota. (`app/chamados/services/disputa_emissao.py:1692`, `app/chamados/services/disputa_emissao.py:1696`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.) |
| `PerguntaDisputa.carimbar` | Preenche instante ausente somente com texto, nas modalidades responder/validar; usado na materialização. (`app/chamados/models.py:3251`, `app/chamados/models.py:3253`, `app/chamados/models.py:3273`, `app/chamados/services/materializacao.py:1046`.) |
| Reabertura, ressuscitação e L-130 | Zeram ambos pela porta de lote; a escrita efetiva é o `update` de `descarimbar_em_lote`. (`app/chamados/services/disputa_emissao.py:1609`, `app/chamados/services/disputa_emissao.py:1648`, `app/chamados/models.py:1639`, `app/chamados/services/validacao.py:440`, `app/chamados/models.py:3402`.) |
| Nascimento | Aceita os campos e verifica o par; defaults são texto `''` e instante nulo. (`app/chamados/models.py:2562`, `app/chamados/models.py:2959`.) |

**Quem lê os campos — principais caminhos identificados**

- **Integridade e estado:** C21, faces SQL, projeção temporal/C6, property `respondida` e guarda de `save`. (`app/chamados/juizes.py:1552`, `app/chamados/juizes.py:1572`, `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1423`, `app/chamados/models.py:3678`, `app/chamados/models.py:3594`.)
- **Validação/correção e app/cobrança:** filas de corrigíveis/respondidas, validação, recusa, partição do app e espera pelo texto. (`app/chamados/juizes.py:1753`, `app/chamados/juizes.py:1761`, `app/chamados/services/validacao.py:681`, `app/chamados/services/validacao.py:901`, `app/chamados/services/pergunta_no_app.py:79`, `app/chamados/services/fio.py:187`, `app/chamados/services/cobranca.py:241`.)
- **Materialização:** interpreta o texto como hora/tipo/ausência e lê os dois campos na foto anterior à devolução. (`app/chamados/services/materializacao.py:546`, `app/chamados/services/materializacao.py:625`, `app/chamados/services/materializacao.py:953`, `app/chamados/services/materializacao.py:1153`.)
- **Histórico, apresentação e métricas:** sequência de decisões, histórico, APIs, display, painel, lavra e vigia de instante sem texto. (`app/chamados/models.py:4105`, `app/chamados/services/historico.py:99`, `app/api/views.py:1163`, `app/api/views_mensageria.py:567`, `app/chamados/models.py:3702`, `app/chamados/services/painel.py:67`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/services/vigia_fio.py:747`.)

**Quem produz e quem lê a retenção**

A lista operacional é **calculada**, não uma coluna a limpar: `irmas_respondidas_sem_consumo` seleciona o universo de crédito — destino, ou pai quando o destino não está vivo — e devolve PKs pelo corpo de `respostas_sem_veredito`, com `ignorar_tranca=True`. (`app/chamados/juizes.py:2115`, `app/chamados/juizes.py:2117`.)

`retencao_da_retratacao` consome esses PKs e retorna `('irma_orfa', pks)`. Leem esse resultado `_retratar`, `encerrar_com_retencao` e o dry do reconciliador. (`app/chamados/reconciliador.py:448`, `app/chamados/reconciliador.py:494`, `app/chamados/reconciliador.py:520`, `app/chamados/management/commands/reconciliar_chamados.py:79`.)

**`registrar_retencao` escreve a trilha**, ação `retratacao_retida_irma_orfa`, em `LogAuditoria.valor_depois.perguntas_pendentes`; guarda somente **os primeiros oito PKs** e lê a última trilha para evitar duplicação. Essa foto histórica não é a lista operacional integral. (`app/chamados/reconciliador.py:459`, `app/chamados/reconciliador.py:470`, `app/chamados/reconciliador.py:480`, `app/chamados/reconciliador.py:484`.)

Outros consumidores do juiz/lista são fechamento da disputa, desligamento, reconciliador, ficha, fila por causa, tranca, materializador e CSF/gate do pai. (`app/chamados/models.py:1944`, `app/chamados/models.py:1977`, `app/colaboradores/services/desligamento.py:165`, `app/chamados/reconciliador.py:1166`, `app/colaboradores/services/ficha.py:169`, `app/chamados/services/fila_por_causa.py:226`, `app/chamados/services/competencia_trancada.py

[truncado em 20000 de 21148 caracteres]

## 17/11 07:30 · sessao fdfa23b1ae21 · CORTE-B2-MEIA-RESPOSTA · verificador · origem=Code

**AVAL-DEVOLVIDO — item 6:** a condição nova elimina o reuso de mesmo tipo, mas ainda permite reuso pela chave `(colaborador, timestamp)`, sem criação nem `+1`. (`app/chamados/juizes.py:248`; `app/ponto/registro_batida.py:144`)

**Item 1**

1. **Premissas:** conferem quanto à mudança das travas: B2 exige instante **e** texto em `respostas_sem_veredito`, e a retenção usa esse mesmo predicado. A docstring registra CH23072/CH33525 perdendo retenção; os números de produção permanecem os fornecidos, sem nova verificação. (`app/chamados/models.py:1926`; `app/chamados/juizes.py:2104`; produção: [nao verificado])

2. **Colisão:** não identificada com L-130: estoque aberto segue a devolução com trilha; entregue continua condicionado a lista publicada e `!` próprio. A existência de uma porta adequada para decidir as ENTREGUE ainda precisa ser demonstrada. (`app/docs/LEIS.md:151`; `app/chamados/services/validacao.py:403`)

3. **Literal:** o bloqueio do pouso é coerente, mas sua condição ainda não está comprovada. Na B2, validar meia-resposta retorna `sem_resposta`; recusar retorna `ja_pendente`; a devolução L-130 exclui ENTREGUE. A Pauta da tranca também depende de `respostas_sem_veredito`, portanto não recolhe automaticamente essas meia-respostas. O ramo de pré-desenho previsto no anexo evita presumir uma porta existente; igualdade posterior com main: **[nao verificado]**. (`app/chamados/services/validacao.py:682`; `app/chamados/services/validacao.py:902`; `app/chamados/services/validacao.py:403`; `app/chamados/services/competencia_trancada.py:126`)

4. **Exportada/dinheiro/`!`:** o alcance declarado exige preservar o `!` próprio; decidir o estoque não está autorizado como apply por esta revisão. Efeito monetário posterior: **[nao verificado]**. (`app/docs/LEIS.md:151`; `CLAUDE.md:265`; apply condicionado no anexo: [nao verificado])

**Item 5**

1. **Premissas:** construir em cópia e preservar cruzamento por arquivo e teto dois conferem. Identidade da cópia aprovada, lei zero e vaga efetivamente livre não foram medidas aqui. (`CLAUDE.md:309`; `CLAUDE.md:291`; condições atuais: [nao verificado])

2. **Colisão:** a antecipação substitui nominalmente o requisito “HEAD com B2 pousada” do COMPLEMENTO-1 fornecido; esse complemento está apenas no anexo. A exceção não afasta a regra vigente de certificar a árvore que pousará. (COMPLEMENTO-1: [nao verificado]; `app/docs/PISTA.md:35`)

3. **Literal:** “refaz a base” fecha **mantendo a certificação da árvore resultante**. Se incorporar o main alterar a árvore, a certificação anterior não vale; código da B2 permanecer igual não prova igualdade da árvore inteira. Não há dispensa literal de suíte no item 5. (`app/docs/PISTA.md:38`; `bin/arvore_hash.sh:18`; dispensa no anexo: [nao verificado])

4. **Exportada/dinheiro/`!`:** abrir uma cópia é rotina autorizada; a exceção de base não amplia autorização para gravado exportado, dinheiro ou pouso sem os portões vigentes. (`CLAUDE.md:282`; `CLAUDE.md:265`; `CLAUDE.md:325`) [nao verificado]

**Item 6**

1. **Premissas:** conferem para a busca apontada em r.r: mesmo colaborador, mesmo tipo, limites inclusivos de ±20 minutos, sem filtro de aprovação **nem de retratação**; o materializador reutiliza o resultado. (`app/chamados/juizes.py:239`; `app/chamados/juizes.py:247`; `app/chamados/services/materializacao.py:1361`)

2. **Colisão:** a condição atende à correção de r.r sobre essa busca, mas não basta para cumprir o RED `+1` de RESPOSTAS-DEVOLVIDOS. A criação passa também por `registrar_batida`, que possui outro reuso. (RED do anexo: [nao verificado]; `app/chamados/services/materializacao.py:1475`; `app/ponto/registro_batida.py:144`)

3. **Literal — não fecha:** sem irmã, uma batida **oposta exatamente em `ts`** satisfaz a ausência de mesmo tipo em ±20 minutos. A vizinhança usa limites estritos; depois, o escritor devolve a batida pela chave exata, com conflito `None`, antes da guarda de colisão. Portanto, a contagem pode continuar igual. A condição precisa excluir também qualquer batida do colaborador exatamente em `ts`. Ocorrência em produção: **[nao verificado]**. (`app/chamados/services/materializacao.py:113`; `app/chamados/services/materializacao.py:116`; `app/ponto/registro_batida.py:144`; `app/ponto/registro_batida.py:162`)

4. **Exportada/dinheiro/`!`:** o RED exercita escrita de batida e seu evento de recálculo; executá-lo em produção ultrapassaria uma revisão/ensaio. A construção posterior na F4 consta apenas no anexo; efeito monetário real: **[nao verificado]**. (`app/ponto/registro_batida.py:176`; `app/ponto/registro_batida.py:180`; escopo F4: [nao verificado])

**VEREDITO: PROCEDE (item 6: ausência de mesmo tipo em ±20 minutos ainda permite reuso de batida oposta exatamente em `ts`, sem garantir `+1`; `app/ponto/registro_batida.py:144`).**

## 17/11 07:30 · sessao 316bd3de1bae · - · - · origem=Ronald (tmux)

Nesta cópia, a meia-resposta **já não segura** `respostas_sem_veredito`, CSF ou retenção da retratação: `sem_veredito_entre` exige C21, isto é, instante **E** texto após trim. Isso é comportamento da cópia B2, não medição de produção. (`app/chamados/models.py:1926`, `app/chamados/models.py:1933`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:2117`, `app/ponto/supra_juiz.py:573`)

O comentário que diz “15 chamados, 113 perguntas a menos; CH23072/CH33525 perdem retenção” existe na fonte, mas valores/efeitos reais em produção permanecem **[nao verificado]**; não são linhas de dado raspado. (`app/chamados/juizes.py:2107`)

**Escritores encontrados dos campos:**

| Escritor | O que escreve e quem chama |
|---|---|
| `responder_pergunta_disputa_core` | Escreve `resposta_colab=resposta` e `respondida_em=agora`, limpa validação anterior e reabre carimbo quando cabível; salva no atomic e acrescenta `DecisaoPergunta.RESPOSTA` com mesmo instante e autor. Chamadores: formulário web, API, correção pelo admin, declaração de hora avulsa e lote de declaração. (`app/chamados/services/disputa_emissao.py:1481`, `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/services/disputa_emissao.py:1499`, `app/chamados/services/disputa_emissao.py:1522`, `app/chamados/services/disputa_emissao.py:1525`; callers `app/chamados/views.py:1284`, `app/api/views.py:2629`, `app/chamados/services/acoes_disputa.py:375`, `app/chamados/services/declaracao_texto.py:420`, `app/chamados/services/declaracao_texto.py:694`) |
| `responder_automaticamente` | Escreve ambos, sem sobrescrever texto já existente; valida o par antes e grava sequência `RESPOSTA`. Caller localizado: recebimento de foto, `_validar_pelo_dono`. (`app/chamados/services/disputa_emissao.py:1663`, `app/chamados/services/disputa_emissao.py:1672`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/services/disputa_emissao.py:1678`, `app/api/foto_recebida.py:48`) |
| `carimbar_resposta_pela_trilha` | Só preenche `respondida_em`, preservando texto existente; exige meio-par texto sem instante; grava decisão e evento. Comando `curar_perguntas_em_limbo` chama usando instante da nota. (`app/chamados/services/disputa_emissao.py:1692`, `app/chamados/services/disputa_emissao.py:1696`, `app/chamados/services/disputa_emissao.py:1699`, `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`) |
| `PerguntaDisputa.carimbar(...resposta='responder'/'validar')` | Preenche `respondida_em` ausente somente com texto; `'validar'` também preenche validação ausente. Exemplos de chamadores: absorção da ausência e resolução das perguntas de chamado resolvido. (`app/chamados/models.py:3251`, `app/chamados/models.py:3253`, `app/chamados/models.py:3255`, `app/chamados/models.py:3273`, `app/chamados/services/materializacao.py:1046`, `app/chamados/services/disputa_emissao.py:2082`) |
| `reabrir_para_correcao` | Zera ambos e validação pela porta de lote; antes grava decisão com foto, autor/motivo quando fornecidos. Chamadores localizados: recusa humana, ausência rejeitada e comando de cura de limbo. (`app/chamados/services/disputa_emissao.py:1609`, `app/chamados/services/disputa_emissao.py:1631`, `app/chamados/services/disputa_emissao.py:1648`, `app/chamados/services/validacao.py:925`, `app/chamados/services/materializacao.py:1167`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`) |
| `ChamadoColaborador.ressuscitar_perguntas` | Zera ambos e validação pela porta de lote; conserva projeção anterior na sequência, sem fabricar ato `REABERTURA/DEVOLUCAO`. Chamadores localizados: reabrir chamado, desarquivar e cartório. (`app/chamados/models.py:1621`, `app/chamados/models.py:1633`, `app/chamados/models.py:1639`, `app/chamados/models.py:1498`, `app/chamados/views_arquivar.py:86`, `app/ponto/services/cartorio.py:1674`) |
| `devolver_instante_sem_texto_l130` | Zera ambos pela porta de lote, preserva projeção anterior na sequência e grava evento com antes/depois. Recusa entregue por seu impedimento. (`app/chamados/services/validacao.py:403`, `app/chamados/services/validacao.py:432`, `app/chamados/services/validacao.py:439`, `app/chamados/services/validacao.py:440`, `app/chamados/services/validacao.py:449`) |
| Executor `PerguntaDisputa.descarimbar_em_lote` | O `QuerySet.update` efetivo zera carimbo e escreve os campos recebidos; valida o par antes. Os escritores acima delegam a ele. (`app/chamados/models.py:3381`, `app/chamados/models.py:3386`, `app/chamados/models.py:3402`) |
| Nascimento genérico | A porta de nascimento aceita campos e chama a guarda do par antes de criar/reusar/costurar; campos têm defaults `''`/`NULL`. (`app/chamados/models.py:2562`, `app/chamados/models.py:2563`, `app/chamados/models.py:2959`) |

Não foi identificado comando com SQL cru escrevendo esses dois campos nesta busca; os comandos de estoque/limbo delegam às portas acima; uma declaração de inventário absoluto de todos mecanismos dinâmicos seria **[nao verificado]**. O `update` concreto identificado está em `descarimbar_em_lote`. (`app/chamados/models.py:3402`; comandos `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`, `app/chamados/services/validacao.py:440`)

**Leitores principais, agrupados — não declarar como inventário exaustivo:**

- Integridade: `respondida_sem_texto`, `texto_sem_carimbo`, C21 e suas faces SQL; guarda `save` lê o par que efetivamente ficará gravado, combinando `update_fields` com banco. (`app/chamados/juizes.py:1534`, `app/chamados/juizes.py:1541`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:1572`, `app/chamados/juizes.py:1580`, `app/chamados/models.py:3594`, `app/chamados/models.py:3596`)
- Vida C6: projeção temporal lê os dois campos e C6 consulta C21 sobre o par projetado; property `PerguntaDisputa.respondida` delega a C21. (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1394`, `app/chamados/juizes.py:1423`, `app/chamados/models.py:3678`)
- Fila humana e correção: `perguntas_corrigiveis`, `perguntas_respondidas_a_validar`, recusa humana e classificação do lote; o comando estoque mostra o par. (`app/chamados/juizes.py:1756`, `app/chamados/juizes.py:1763`, `app/chamados/services/validacao.py:901`, `app/chamados/services/validacao.py:1183`, `app/chamados/services/validacao.py:1256`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:58`)
- App/cobrança: `particao_do_app` usa C21; `espera_o_texto_do_colaborador` lê instante ou meio-par; cobrança lê esse helper e C6. (`app/chamados/services/pergunta_no_app.py:79`, `app/chamados/services/pergunta_no_app.py:82`, `app/chamados/services/fio.py:187`, `app/chamados/services/cobranca.py:241`)
- Materialização: lê texto para tipo/hora/ausência e os dois campos para foto de devolução; fila de materialização separa respondida completa e meio-par sem texto para recusa. (`app/chamados/services/materializacao.py:546`, `app/chamados/services/materializacao.py:625`, `app/chamados/services/materializacao.py:953`, `app/chamados/services/materializacao.py:1153`, `app/chamados/services/materializacao.py:1580`, `app/chamados/services/materializacao.py:2091`)
- Interpretação do texto: slot esperado, declaração de hora, delta e plausibilidade. (`app/chamados/juizes.py:364`, `app/chamados/juizes.py:1791`, `app/chamados/juizes.py:1938`, `app/chamados/juizes.py:2042`, `app/chamados/juizes.py:2064`)
- Sequência/trilha: conversão da projeção sem linha, histórico, fechamento de silêncio, retirada de motivo e fechamento do fio do dia. (`app/chamados/models.py:4105`, `app/chamados/models.py:4114`, `app/chamados/models.py:4119`, `app/chamados/services/historico.py:99`, `app/chamados/models.py:2017`, `app/chamados/models.py:2149`, `app/ponto/services/veredito_celula.py:197`)
- Apresentação/API: representação do texto, respostas da API, mensageria, explicador, dia do colaborador e templates. (`app/chamados/models.py:3702`, `app/api/views.py:1163`, `app/api/views.py:2561`, `app/api/views_mensageria.py:567`, `app/chamados/services/explicador.py:102`, `app/ponto/services/dia_do_colab.py:70`, `app/templates/chamados/partials/modal_fio.html:219`, `app/templates/chamados/partials/_card_atendimento.html:106`)
- Idade/vigia/projeção de painel: `verbos` usa instante filtrando C21, painel calcula `Max(respondida_em)`, lavra calcula máximo de resposta, vigia conta instante com texto vazio. (`app/chamados/services/verbos.py:333`, `app/chamados/services/verbos.py:337`, `app/chamados/services/painel.py:67`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/services/vigia_fio.py:747`, `app/chamados/services/vigia_fio.py:750`)

**Quem escreve/lê a retenção:**

A lista vigente não é campo persistido: `irmas_respondidas_sem_consumo` calcula PKs sobre universo do crédito — destino ou pai quando destino não está vivo — delegando ao mesmo `sem_veredito_entre`, com `ignorar_tranca=True`. (`app/chamados/juizes.py:2115`, `app/chamados/juizes.py:2117`)

`retencao_da_retratacao` lê essa lista e retorna `('irma_orfa', pks)`; também pode retornar retenção por justificativa alheia durante aprovação. `_retratar`, `encerrar_com_retencao` e o dry do reconciliador leem esse resultado. (`app/chamados/reconciliador.py:448`, `app/chamados/reconciliador.py:453`, `app/chamados/reconciliador.py:494`, `app/chamados/reconciliador.py:520`, `app/chamados/management/commands/reconciliar_chamados.py:79`)

`registrar_retencao` é escritor da **trilha**, não de lista operacional: grava `LogAuditoria` via `evento`, ação `retratacao_retida_irma_orfa`, `valor_depois.perguntas_pendentes=detalhe[:8]`; lê a última trilha para evitar duplicação. Portanto a trilha guarda apenas os primeiros oito PKs, não a lista integral. (`app/chamados/reconciliador.py:459`, `app/chamados/reconciliador.py:470`, `app/chamados/reconciliador.py:480`, `app/chamados/reconciliador.py:484`)

Outros consumidores do mesmo corpo/lista: `pode_fechar`, `fechar`, desligamento, reconciliador de disputa, ficha e fila por causa, fechamento por competência; materializador lê as irmãs para preservar destino/clones. CSF produz lista irmã sobre universo pai+destino e o gate a registra no fio. (`app/chamados/models.py:1944`, `app/chamados/models.py:1977`, `app/colaboradores/services/desligamento.py:165`, `app/chamados/reconciliador.py:1166`, `app/colaboradores/services/ficha.py:169`, `app/chamados/services/fila_por_causa.py:226`, `app/chamados/services/competencia_trancada.py:126`, `app/chamados/services/materializacao.py:1247`, `app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:574`, `app/chamados/models.py:719`, `app/chamados/models.py:739`)

**Reversão relevante:** restaurar a foto de meia-resposta por `save`, nascimento ou porta do lote introduziria novamente instante sem texto e será recusado pela guarda; o bypass “par legado intocado” só vale quando o par já é igual ao banco, não depois de ter sido corrigido. A reposição literal de meia-resposta por uma porta autorizada de reversão existente é **[nao verificado]**. (`app/chamados/services/fio.py:1607`, `app/chamados/services/fio.py:1620`, `app/chamados/models.py:2959`, `app/chamados/models.py:3386`, `app/chamados/models.py:3598`, `app/chamados/models.py:3601`)

## 17/11 07:29 · sessao 8ae71a11d1e4 · - · - · origem=Ronald (tmux)

Conclusão: nenhuma porta auditada reúne decidir a meia-resposta ENTREGUE, corrigir o par, trilhar a decisão e encaminhar à lista DP. Os caminhos abaixo têm partes desse comportamento, sem a combinação completa. (`app/chamados/models.py:1933`; `app/chamados/services/competencia_trancada.py:126`; `app/chamados/services/fecho_runtime.py:139`; `app/pautas/services.py:180`)

| Candidata/destino | Evidência e limite |
|---|---|
| **Pauta genérica DP** | `pautas.services.escrever` cria Pauta com remetente, destinatário, âncora, texto, prazo, pai e autor; chama trilha e lavra contador. Não altera `PerguntaDisputa`. (`app/pautas/services.py:153`; `app/pautas/services.py:180`) |
| **Onde o DP lê Pauta** | Aba departamental filtra `destinatario_departamento=aba`, normalmente só vivas; feitas aparecem por opção. View chama esse leitor, URL `aba/<str:aba>/`. (`app/pautas/services.py:304`; `app/pautas/services.py:311`; `app/pautas/views.py:29`; `app/pautas/urls.py:8`) |
| **Trilha da Pauta** | `_trilha` registra evento modelo `Pauta`, descrição com âncora, extra em `depois` e ator; a criação registra remetente e indicação de remetente informado. Não é uma trilha antes/depois da pergunta; `evento` é best effort. (`app/pautas/services.py:186`; `app/pautas/services.py:380`; `app/core/observ.py:94`; `app/core/observ.py:134`) |
| **Reversão da Pauta** | `cancelar` exige autor original, recusa pauta feita, grava `cancelado_em`, trilha e relavra contador. Mantém histórico; não desfaz pergunta. (`app/pautas/services.py:281`) |
| **`competencia_trancada.encerrar` / `_pauta_do_dp`** | Exige `periodo_trancado`; competência apenas exportada continua viva. Seleção `decidir_pelo_dp` lê `respostas_sem_veredito(ignorar_tranca=True)`, que nesta B2 exige C21: meia-resposta não entra. Não escreve pergunta; fecha chamados e disputas e cria `[TRANCA]` para DP por competência. (`app/chamados/services/competencia_trancada.py:5`; `app/chamados/services/competencia_trancada.py:95`; `app/chamados/services/competencia_trancada.py:126`; `app/chamados/models.py:1933`; `app/chamados/services/competencia_trancada.py:203`; `app/chamados/services/competencia_trancada.py:232`; `app/chamados/services/competencia_trancada.py:237`) |
| **Chamadores/reversão da tranca** | Gancho `trancar_periodo` chama disparo; até 100 ações chama `encerrar`, acima dispara comando; comando também chama porta diretamente. Reabertura da competência chama `reabrir`, que restaura perguntas antigas pela via da tranca e chamados, com evento. Não cancela a Pauta `[TRANCA]`. (`app/ponto/services/fechamento.py:1427`; `app/chamados/services/competencia_trancada.py:301`; `app/chamados/services/competencia_trancada.py:315`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`; `app/ponto/services/fechamento.py:1447`; `app/chamados/services/competencia_trancada.py:260`) |
| **`fecho_runtime.transicionar_com_pauta`** | Coleta recusas S143 de reabrir credor exportado/limbo; agrupa por competência e cria `[FIO-E0-V4-E4B-S143]` para DP, deduplicando lote vivo. Não decide meia-resposta nem corrige seu par. (`app/chamados/services/disputa_emissao.py:1969`; `app/chamados/services/fecho_runtime.py:95`; `app/chamados/services/fecho_runtime.py:119`; `app/chamados/services/fecho_runtime.py:139`; `app/chamados/services/fecho_runtime.py:146`) |
| **`fechar_por_admin`** | Grava apenas `materializada_em/materializada_por/via_resolucao=admin_decidiu`, por `carimbar_identidade`; não consulta competência entregue, não corrige par e não cria Pauta. Reversão técnica pelo trio em `descarimbar`, salvo linha fora do fato. A porta isolada não grava `DecisaoPergunta`; os chamadores a gravam. (`app/chamados/models.py:3171`; `app/chamados/models.py:3289`; `app/chamados/models.py:3324`; `app/chamados/models.py:2015`; `app/ponto/services/veredito_celula.py:195`) |
| **`fechar_o_fio_do_dia` / `veredito_celula`** | Resolve o dia inteiro, seleciona perguntas vivas do fato, chama `fechar_por_admin` e grava `DecisaoPergunta.SILENCIO` com autor, instante do carimbo, texto e motivo `dia_decidido_pelo_admin`. Não conserta par. Pode criar Pauta somente pelas recusas S143 do fechamento dos chamados. View POST do veredito chama serviço; URL `celula/veredito/`. (`app/ponto/services/veredito_celula.py:160`; `app/ponto/services/veredito_celula.py:180`; `app/ponto/services/veredito_celula.py:190`; `app/ponto/services/veredito_celula.py:195`; `app/ponto/services/veredito_celula.py:203`; `app/ponto/views.py:3391`; `app/ponto/views.py:3419`; `app/ponto/urls.py:58`) |
| **`DisputaSupervisao.admin_encerra`** | Exige motivo e fecha disputa inteira por porta de exceção. `fechar` recusa respostas sem veredito; na B2 a meia não integra essa lista. Carimba vivas e grava SILENCIO mais eventos antes/depois; não conserta par, não cria Pauta. Nenhuma tela oferece a porta, conforme backlog; caller efetivo de produção [nao verificado]. (`app/chamados/models.py:2034`; `app/chamados/models.py:1977`; `app/chamados/models.py:2009`; `app/docs/BACKLOG.md:1587`) |
| **`bug_par_103` “PAUTA DP”** | Lista em memória recebe entregues/sem dia e imprime no stdout; não cria `Pauta`. Entregues são preservadas; apply chama `devolver_paridade_bug_par_103` nas linhas elegíveis, que é outro recorte. (`app/chamados/management/commands/bug_par_103.py:63`; `app/chamados/management/commands/bug_par_103.py:70`; `app/chamados/management/commands/bug_par_103.py:85`; `app/chamados/management/commands/bug_par_103.py:106`; `app/chamados/management/commands/bug_par_103.py:122`) |
| **Pauta do limbo exportado** | Julgamento da célula encaminha limbo `trabalha=None` entregue para Pauta DP do DIA, sem pergunta no app. Não trata meia-resposta existente. Dedup considera pautas vivas, feitas e canceladas. (`app/chamados/services/fabrica_da_celula.py:214`; `app/chamados/services/fabrica_da_celula.py:223`; `app/chamados/services/fabrica_da_celula.py:251`; `app/chamados/services/fabrica_da_celula.py:268`) |
| **Pauta cadastro sem turno** | Destinatário DP, âncora colaborador e marcador `CADASTRO: PASSADO SEM TURNO PREVISTO`; entregue significa viva ou fechada no/apos dia relevante. Não trata o par da pergunta. (`app/chamados/services/fabrica_da_celula.py:305`; `app/chamados/services/fabrica_da_celula.py:308`; `app/chamados/services/fabrica_da_celula.py:317`) |
| **Resposta tardia** | Recebe nova manifestação em chamado encerrado; se não renasce, grava nota/log e somente texto livre não vazio cria Pauta DP ancorada no chamado. Não decide meia-resposta armazenada. (`app/chamados/services/resposta_tardia.py:31`; `app/chamados/services/resposta_tardia.py:53`; `app/chamados/services/resposta_tardia.py:59`) |
| **Worklist DP** | Comando somente leitura: coleta gaps de turno/entrada e imprime por posto/colaborador/dia. Não grava decisão nem encaminha perguntas. (`app/ponto/management/commands/worklist_dp.py:5`; `app/ponto/management/commands/worklist_dp.py:12`; `app/ponto/management/commands/worklist_dp.py:22`) |

`DecisaoPergunta.registrar` também não é porta de decisão de domínio: recebe linhas, faz `bulk_create` e acorda células; não muda projeção da pergunta, não consulta entrega e não cria Pauta. Os atos são imutáveis; reversão exige ato seguinte, não update/delete. (`app/chamados/models.py:4124`; `app/chamados/models.py:4084`; `app/chamados/models.py:4089`)

## 17/11 07:29 · sessao 3ff40bea6d64 · - · - · origem=Ronald (tmux)

Leitura estática; execução efetiva dos chamadores em produção é [nao verificado].

| Candidata | Resultado para instante + texto vazio em ENTREGUE | Escrita/trilha/destino/reversão |
|---|---|---|
| `validar_pergunta` | Recusa antes de escrever: `sem_resposta`, pois consulta `pergunta_respondida` (`app/chamados/services/validacao.py:681`). | Nos casos válidos grava `validada_em/por` e `DecisaoPergunta.VALIDACAO` (`validacao.py:168`, `validacao.py:173`), materializa (`validacao.py:769`), registra LogAuditoria (`validacao.py:796`). K6 da validação só consulta metades de intervalo fora U163 (`app/chamados/services/materializacao.py:792`, `materializacao.py:805`); entregue gera mensagem “preservada para Pauta DP”, sem escrever Pauta (`materializacao.py:812`). Desfaz carimbo da validação pela devolução se materialização recusar (`validacao.py:781`); reversão geral posterior não verificada [nao verificado]. |
| `recusar_pergunta` | Recusa como `ja_pendente`, antes de escrever (`validacao.py:901`). | Nos casos válidos escreve motivo (`validacao.py:916`, `validacao.py:922`), chama reabertura com ato `recusa` (`validacao.py:925`), que zera resposta/instante/validação (`app/chamados/services/disputa_emissao.py:1609`) e carimbo materialização (`app/chamados/models.py:3402`). Trilha DecisaoPergunta com foto (`disputa_emissao.py:1631`), nota (`validacao.py:930`), LogAuditoria com antes/depois (`validacao.py:934`), push ao colaborador (`validacao.py:945`). Não consulta K6 no corpo (`validacao.py:883`–`954`); destino é colaborador, não DP (`validacao.py:953`). Inversa dedicada [nao verificado]. |
| `devolver_validacao_recusada` | Exige resposta HORA declarada, não materializada, motivo preenchido (`validacao.py:264`, `validacao.py:266`; `_resposta_de_hora` em `validacao.py:193`). Não resolve meia-resposta vazia por esse escopo. | Zera somente `validada_em/por` (`validacao.py:273`), conserva resposta (`validacao.py:281`), grava DEVOLUCAO (`validacao.py:279`) e evento com antes/depois/ator (`validacao.py:297`). Pode reabrir disputa (`validacao.py:291`); `reabrir=False` conserva fechada e expõe ao admin (`validacao.py:289`). Sem guarda K6 própria no corpo (`validacao.py:261`–`305`); chamador validar consulta K6 antes e depois (`validacao.py:740`, `validacao.py:777`). Inversa funcional seria validar novamente, se elegível (`validacao.py:661`); restauração exata [nao verificado]. |
| `devolver_paridade_bug_par_103` | Específica de paridade absolvida U103 e recusa competência entregue ou sem dia (`validacao.py:334`, `validacao.py:338`). | Descarimba materialização e zera validação (`validacao.py:346`, `validacao.py:349`), conserva resposta/motivo (`validacao.py:356`, `validacao.py:364`), grava DEVOLUCAO e LogAuditoria antes/depois/ator (`validacao.py:354`, `validacao.py:359`). Falha de trilha desfaz atomic (`validacao.py:366`). Não reabre disputa; destino admin para validar par (`validacao.py:315`). Comando grava JSON de reversão com carimbos/validação/motivo (`app/chamados/management/commands/bug_par_103.py:64`, `bug_par_103.py:95`); não possui argumento de restauração (`bug_par_103.py:31`). |
| `impedimento_estoque_l130` | Predicado sem escrita. Retorna `competencia_entregue` (`validacao.py:400`, `validacao.py:403`), além de excluir validada, materializada, fora do fato, morta C6 e sem dia (`validacao.py:390`). | Sem trilha, decisão ou destino; retorna impedimento (`validacao.py:381`–`405`). |
| `devolver_instante_sem_texto_l130` | Recusa ENTREGUE por reler o predicado sob trava (`validacao.py:428`, `validacao.py:432`). | Nos elegíveis abertos preserva linha antiga da projeção (`validacao.py:436`, `validacao.py:439`), zera `respondida_em/resposta_colab` (`validacao.py:440`) e carimbo materialização pela porta de lote (`models.py:3402`), acorda células (`validacao.py:446`). Evento com antes/depois/ator (`validacao.py:449`), atomic desfeito sem trilha (`validacao.py:455`). Não cria ato REABERTURA/DEVOLUCAO (`validacao.py:417`), não escreve Pauta nem oferece inversa no corpo (`validacao.py:421`–`458`). Restaurar exatamente a meia-resposta contrariaria o guardião do par ilegal da porta de lote (`models.py:3383`). |
| comando `estoque_l130_instante_sem_texto` | Só chama porta anterior; apply exige exatamente alvos 38251/38252 (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:54`; alvos em `validacao.py:378`). | DRY default e stdout dos impedimentos (`estoque_l130_instante_sem_texto.py:47`, `:51`); grava JSON anterior antes de aplicar (`:57`, `:65`), chama porta (`:105`), desfaz lista inteira se contagem divergir (`:111`). Só argumentos autor/pasta/apply, sem restaurar (`:25`). Não encaminha à lista DP (`:30`–`115`). |

Chamadores existentes na árvore:

- `validar_pergunta`: casca HTTP `app/chamados/services/acoes_disputa.py:19`; aprovação inline `acoes_disputa.py:383`; foto API `app/api/foto_recebida.py:51`; declaração textual `app/chamados/services/declaracao_texto.py:697`; comando classe A `app/chamados/management/commands/validar_classe_a.py:100`. Casca chamada por cards `app/chamados/views.py:501`, fio `app/chamados/services/fio.py:655`, cobrança `app/chamados/views_cobrar.py:210` e lote `validacao.py:1078`.
- `recusar_pergunta`: casca `acoes_disputa.py:82`; esta é chamada pelo card `views.py:518` e fio `fio.py:684`.
- `devolver_validacao_recusada`: chamada concreta de código em `validacao.py:781`; docstring menciona `logs/recusa_fala/acervo.py` (`validacao.py:242`), arquivo ausente na árvore consultada [nao verificado].
- `devolver_paridade_bug_par_103`: comando `bug_par_103.py:106`.
- `impedimento_estoque_l130`: seleção/releitura no comando `estoque_l130_instante_sem_texto.py:42`, `:99` e porta `validacao.py:432`.
- `devolver_instante_sem_texto_l130`: comando `estoque_l130_instante_sem_texto.py:105`.

“Pauta DP” adjacente: `bug_par_103` acumula lista Python de U103 entregues/sem dia (`bug_par_103.py:56`, `:62`), acrescenta metades fora U163 **respondidas com texto** (`:71`) entregues/sem dia (`:75`), e só imprime `PAUTA DP` no stdout (`:85`, `:122`). Não persiste Pauta nem decide aquelas perguntas; a própria lista preserva exportadas (`:70`). A mensagem K6 “preservada para Pauta DP” é retorno textual (`materializacao.py:813`).

Trilha comum: `evento` escreve `LogAuditoria` com usuário, ação, descrição, antes/depois e IP (`app/core/observ.py:133`), ator de processo quando sem request (`observ.py:116`); o modelo carimba `criado_em` (`app/core/models.py:295`, `:307`). `DecisaoPergunta` guarda ato/instante/autor/texto/motivo (`app/chamados/models.py:4052`); `registrar` é a porta de criação (`models.py:4124`).

## 17/11 05:47 · sessao 35e187ebaa3c · O10-D-PERMUTA-RECIPROCA · verificador · origem=Code

| Ponto | Veredito | Prova |
|---|---|---|
| Delta corrige “o inverso deadlocka” sem exceder o selo? | **PROCEDENTE** | O texto passa a dizer possibilidade e explicita “sem concorrência”, limitada à ordem SQL desta porta (`app/ponto/tests/test_o10d_permuta_reciproca.py:404`). O helper verifica a sequência carta → célula → colaborador antes da primeira escrita; a execução usa captura de consultas (`app/ponto/tests/test_o10d_permuta_reciproca.py:380`, `app/ponto/tests/test_o10d_permuta_reciproca.py:394`, `app/ponto/tests/test_o10d_permuta_reciproca.py:420`). |
| Ressalva sobre `veredito_celula` | **PROCEDENTE** | O caminho trava colaborador primeiro e depois chama a gravação do veredito da célula (`app/ponto/services/veredito_celula.py:226`, `app/ponto/services/veredito_celula.py:352`). A permuta trava células antes dos colaboradores (`app/ponto/portas/celula.py:1016`, `app/ponto/portas/celula.py:1019`). Deadlock executado: **[nao verificado]**. |
| A — `date.max`: DESCARTADO | **NAO PROCEDE** como descarte integral | O juiz compartilha o limite, pois incrementa depois de incluir o último dia (`app/ponto/turnos.py:2813`). Isso não elimina o limite da porta: ela calcula `a.data + timedelta(days=1)` antes de chamar o juiz (`app/ponto/portas/celula.py:1036`). A alegação “não existe como carta” em produção é **[nao verificado]**; portanto não sustenta o descarte. |
| C — R3 removida: “declarado”; invertida como mutante de asserção | **PROCEDENTE** quanto ao mecanismo | O teste deixa SUB sem vínculo em 04/11 e exige a frase exclusiva dessa pessoa/data (`app/ponto/tests/test_o10d_permuta_reciproca.py:227`, `app/ponto/tests/test_o10d_permuta_reciproca.py:231`). A inversão acusa vínculos existentes e retorna pela lista de recusas (`app/ponto/portas/celula.py:888`, `app/ponto/portas/celula.py:890`). A remoção permite desreferenciar `None`; os handlers não capturam `AttributeError` (`app/ponto/portas/celula.py:909`, `app/ponto/portas/celula.py:959`, `app/ponto/portas/celula.py:1091`). Execução/publicação histórica de M06/M06b: **[nao verificado]**. |
| C — Três cartas com `KeyError`: “declarado”; M09 dirigido à quinta | **PROCEDENTE** quanto ao mecanismo | Três cartas deixam uma chave ausente; retirar a guarda expõe o acesso direto ao mapa (`app/ponto/tests/test_o10d_permuta_reciproca.py:248`, `app/ponto/portas/celula.py:1029`). O caso da quinta mantém as quatro necessárias, declara M09 e mocka a troca; o helper exige `'recusada'` por asserção (`app/ponto/tests/test_o10d_permuta_reciproca.py:255`, `app/ponto/tests/test_o10d_permuta_reciproca.py:259`, `app/ponto/tests/test_o10d_permuta_reciproca.py:261`, `app/ponto/tests/test_o10d_permuta_reciproca.py:106`). Execução histórica do alvo M09: **[nao verificado]**. |
| E — Relógio: DESCARTADO por estar fora deste delta | **PROCEDENTE**, somente como delimitação de escopo | O delta fornecido altera a docstring do teste (`app/ponto/tests/test_o10d_permuta_reciproca.py:401`). O problema permanece: tanto edição isolada quanto callback composto alcançam o helper que lê `localdate()` internamente (`app/ponto/portas/celula.py:676`, `app/ponto/portas/celula.py:698`, `app/ponto/portas/celula.py:465`). O descarte desta rodada não comprova cura. |
| Campo/valor novo: quem escreve e quem lê? | **SEM ACHADO** de campo/valor de domínio novo | No delta fornecido, muda somente o texto da docstring; não há novo escritor ou leitor de domínio a mapear (`app/ponto/tests/test_o10d_permuta_reciproca.py:404`). |

Identidade Git da cópia, completude do intervalo entre hashes e execução dos selos/mutantes nesta volta: **[nao verificado]**.

## 17/11 05:45 · sessao 597e5f6fd18a · O10-D-PERMUTA-RECIPROCA · verificador · origem=Code

A correção temporal fecha o escape de período aberto, mas a mordida por asserção continua incompleta no R3, e os problemas de tipo/status e idempotência permanecem (`app/ponto/portas/celula.py:1035`, `app/ponto/portas/celula.py:909`, `app/ponto/portas/celula.py:1043`, `app/ponto/portas/celula.py:1058`). Execução integral da suíte, mutantes Django, identidade Git da cópia e ocorrência em produção: **[nao verificado]**.

| A — Carta do dia exato | Veredito | Prova |
|---|---|---|
| Ainda aceita carta que cobre vários dias ou outro dia? | **NAO PROCEDE** para intervalos válidos e janela representável. | `_cobertura_pura` e `dias_da_ausencia` usam os mesmos três casos: fim preenchido, aberto por tipo, ou dia único (`app/ponto/turnos.py:2783`, `app/ponto/turnos.py:2803`). Na janela inclusiva `[data,data+1]`, fim posterior ou aberto produz dois dias e a guarda recusa (`app/ponto/portas/celula.py:1035`). |
| Recusa carta legítima de um dia? | **SEM ACHADO** na distinção temporal ordinária. | Fim igual ao início, ou fim nulo de tipo não aberto, produz `[data]`, aceito pela guarda (`app/ponto/turnos.py:2803`, `app/ponto/portas/celula.py:1037`). Fim anterior produz lista vazia, mas o próprio modelo proíbe esse intervalo (`app/ponto/models.py:904`). |
| Os outros juízes discordam dos limites? | **SEM ACHADO** para intervalos válidos. | `_q_cobertura`, `afastamentos_no_dia` e `ausencias_sobrepostas` incluem início e fim; nenhum desloca a data por horário (`app/ponto/turnos.py:2773`, `app/ponto/turnos.py:2730`, `app/ponto/turnos.py:2843`). |
| Ausência fora do dia exato ainda sai da regra 2 por `excluir_pks`? | **NAO PROCEDE** nessa passagem. | A cardinalidade e o mapeamento exigem quatro cartas das quatro células; todas passam pela guarda temporal antes das barreiras (`app/ponto/portas/celula.py:1022`, `app/ponto/portas/celula.py:1035`, `app/ponto/portas/celula.py:1065`). A exclusão ocorre depois, em `app/ponto/portas/celula.py:922`. |
| Atestado aprovado continua podendo ser carta? | **PROCEDENTE**, como problema do item 2. | Um atestado aprovado **de um dia** passa: não há restrição de tipo e só `rejeitada` é recusada; seu PK é excluído da sobreposição (`app/ponto/portas/celula.py:1042`, `app/ponto/portas/celula.py:1043`, `app/ponto/portas/celula.py:922`). |
| A janela de um dia além pertence ao juiz? | **NAO PROCEDE** como janela prescrita pelo juiz; **SEM ACHADO** de expansão paralela. | A porta escolhe `data+1` como testemunha de um segundo dia; o juiz recebe limites arbitrários e executa a expansão existente (`app/ponto/portas/celula.py:1032`, `app/ponto/turnos.py:2798`). O registro já reconhece `dias_da_ausencia` como juiz (`app/core/juizes.py:454`). |
| Limite extremo do calendário | **PROCEDENTE** como limite técnico, sem ocorrência demonstrada. | `data+1` não é representável em `date.max`; os próprios expansores também incrementam após incluir o último dia (`app/ponto/portas/celula.py:1036`, `app/ponto/turnos.py:2794`, `app/ponto/turnos.py:2813`). Existência de carta nessa borda: **[nao verificado]**. |

| B — Ordem de trava | Veredito | Prova |
|---|---|---|
| Comentários/docstring corrigidos afirmam ordem universal? | **NAO PROCEDE**. | Agora dizem “desta porta”; a execução efetivamente adquire cartas → células existentes → colaboradores (`app/ponto/portas/celula.py:744`, `app/ponto/portas/celula.py:971`, `app/ponto/portas/celula.py:1014`, `app/ponto/portas/celula.py:1016`, `app/ponto/portas/celula.py:1019`). |
| Coerência com os escritores citados | **SEM ACHADO**, com precisão sobre travas implícitas. | `editar_dia` trava célula antes do marcador; regeneração escreve célula antes do marcador, adquirindo trava pelo UPDATE (`app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:672`, `app/ponto/portas/celula.py:311`, `app/ponto/portas/celula.py:328`). O marcador chega à trava do colaborador por `renovar_contexto` (`app/ponto/management/commands/gerar_celulas.py:264`, `app/chamados/services/contexto.py:103`, `app/chamados/models.py:487`). `decidir` escreve Ausencia dentro do atomic e seu signal chama o cartório (`app/ponto/services/ausencia.py:1079`, `app/ponto/services/ausencia.py:988`, `app/ponto/signals.py:147`). |
| A chamada inicial também trava Disputa/Chamado/Pergunta? | **NAO PROCEDE**. | A permuta passa somente colaboradores; as demais tabelas dependem de argumentos adicionais (`app/ponto/portas/celula.py:1019`, `app/chamados/models.py:2771`, `app/chamados/models.py:2779`, `app/chamados/models.py:2792`, `app/chamados/models.py:2796`). |
| Restou linguagem universal? | **SEM ACHADO** no comentário `GLOBAL`; **PROCEDENTE** quanto à garantia excessiva do teste. | `GLOBAL` trata da ordenação **entre células**, executada pelo `sorted` (`app/ponto/portas/celula.py:770`, `app/ponto/portas/celula.py:773`). Já “o inverso deadlocka” excede o que o teste mede: captura SQL e ordem, sem concorrência (`app/ponto/tests/test_o10d_permuta_reciproca.py:404`, `app/ponto/tests/test_o10d_permuta_reciproca.py:418`). |
| A correção textual resolve compatibilidade com toda a casa? | **NAO PROCEDE**. | Existe caminho inverso: `veredito_celula` trava colaborador e depois escreve célula (`app/ponto/services/veredito_celula.py:226`, `app/ponto/services/veredito_celula.py:352`, `app/ponto/portas/celula.py:1457`). Deadlock efetivamente executado: **[nao verificado]**. |

| C — Mordida dos testes | Veredito | Prova | [nao verificado]
|---|---|---|
| R3 com guarda invertida: `is None` → `is not None` | **NAO PROCEDE** a crítica de falhar somente por exceção nessa mutação. | A inversão produz frases para os três vínculos existentes e retorna; a igualdade exata exige exclusivamente SUB em 04/11 e falha por asserção (`app/ponto/portas/celula.py:888`, `app/ponto/portas/celula.py:890`, `app/ponto/tests/test_o10d_permuta_reciproca.py:231`). |
| R3 com guarda removida/desativada | **PROCEDENTE**: continua falhando por exceção. | O teste encerra SUB em 03/11. Sem a guarda, o caminho chega a `.eh_dia_trabalho` sobre `None` no dia cedido 04/11 (`app/ponto/tests/test_o10d_permuta_reciproca.py:227`, `app/ponto/portas/celula.py:909`). Os handlers só capturam `_NaoSei` e `_PermutaRecusada`, deixando escapar `AttributeError` antes da asserção (`app/ponto/portas/celula.py:959`, `app/ponto/portas/celula.py:1091`). |
| R5 QUINTA com guarda inteira removida/invertida | **NAO PROCEDE** a crítica de exceção nesse teste novo. | As quatro chaves necessárias existem, evitando `KeyError`; a quinta só viola cardinalidade (`app/ponto/tests/test_o10d_permuta_reciproca.py:259`, `app/ponto/portas/celula.py:1029`). Com troca mockada, o fluxo alcança `'permutada'`, contrariando a asserção `'recusada'` do helper (`app/ponto/tests/test_o10d_permuta_reciproca.py:261`, `app/ponto/portas/celula.py:1097`, `app/ponto/tests/test_o10d_permuta_reciproca.py:106`). |
| Há forma que ainda morde por exceção? | **PROCEDENTE**. | Além do R3 acima, o teste antigo de três cartas continua produzindo `KeyError` ao remover/inverter a guarda inteira, porque falta `a_assume` na compreensão (`app/ponto/tests/test_o10d_permuta_reciproca.py:248`, `app/ponto/portas/celula.py:1026`, `app/ponto/portas/celula.py:1029`). |

| D — Campo/valor | Veredito | Quem escreve, quem lê e significado comprovado |
|---|---|---|
| `Ausencia.tipo` | **PROCEDENTE**: papel duplo. | Escrevem nascimento e edição (`app/ponto/services/ausencia.py:484`, `app/ponto/services/ausencia.py:648`). Permuta lê na expansão e aceita qualquer tipo como carta (`app/ponto/portas/celula.py:1036`, `app/ponto/portas/celula.py:1043`). Precedência continua lendo a mesma linha como ausência, enquanto a permuta a exclui como “próprio pedido” (`app/ponto/precedencia.py:273`, `app/ponto/portas/celula.py:914`, `app/ponto/portas/celula.py:922`). |
| `Ausencia.data` | **SEM ACHADO** de deslocamento nesses caminhos. | Nascimento/edição escrevem; permuta associa à célula; troca edita essa mesma data (`app/ponto/services/ausencia.py:484`, `app/ponto/services/ausencia.py:649`, `app/ponto/portas/celula.py:1022`, `app/ponto/portas/celula.py:775`). Cobertura a usa como início inclusivo; parcial noturna pertence ao dia inicial do turno (`app/ponto/turnos.py:2809`, `app/ponto/models.py:765`). |
| `Ausencia.data_fim` | **SEM ACHADO** de divergência inclusivo/exclusivo. | Porta deriva fim inclusivo; nascimento/edição gravam; modelo valida e deriva duração (`app/ponto/services/ausencia.py:198`, `app/ponto/services/ausencia.py:485`, `app/ponto/services/ausencia.py:650`, `app/ponto/models.py:904`). Permuta e cobertura leem fim inclusivo; nulo é aberto somente pelos tipos correspondentes (`app/ponto/portas/celula.py:1036`, `app/ponto/turnos.py:2803`). O help text generaliza “NULL = aberto”, sem essa ressalva (`app/ponto/models.py:751`). |
| `Ausencia.status` | **PROCEDENTE** quanto à diferença de efeito. | Nascimento grava estado inicial; `mudar_estado` escreve transições; espelho de férias tem rejeição direta declarada (`app/ponto/services/ausencia.py:486`, `app/ponto/services/ausencia.py:976`, `app/ponto/services/ausencia.py:1496`). Folha exige aprovada; cobrança aceita não rejeitada; permuta só recusa rejeitada e exclui inclusive aprovada da regra 2 (`app/ponto/turnos.py:2523`, `app/ponto/portas/celula.py:1043`, `app/ponto/portas/celula.py:922`). |
| `Ausencia.documento` | **PROCEDENTE** quanto à equivalência presumida com carta. | Escrevem criação, anexo posterior e reabertura (`app/ponto/services/ausencia.py:493`, `app/ponto/services/ausencia.py:1017`, `app/ponto/services/ausencia.py:1307`). Modelo o define como comprovante da ausência; permuta testa presença e troca copia o nome à trilha (`app/ponto/models.py:777`, `app/ponto/portas/celula.py:1042`, `app/ponto/portas/celula.py:784`). Essa guarda não verifica conteúdo nem existência no storage (`app/ponto/portas/celula.py:1042`). |
| `CelulaDia.trabalha` | **SEM ACHADO** de novo escritor fora da porta. | Permuta compõe trocas, que chamam `editar_dia` com False/True; este cria/atualiza (`app/ponto/portas/celula.py:1073`, `app/ponto/portas/celula.py:773`, `app/ponto/portas/celula.py:645`, `app/ponto/portas/celula.py:648`). Gerador também escreve dentro da porta (`app/ponto/portas/celula.py:294`, `app/ponto/portas/celula.py:307`). Autoridade de trabalho lê a célula antes da aritmética do vínculo (`app/escala/models.py:1092`). |
| `CelulaDia.dna` | **PROCEDENTE** na leitura incompleta de igualdade. | Produtor é `montar_dna`; edição pede e grava o resultado (`app/ponto/management/commands/gerar_celulas.py:73`, `app/ponto/portas/celula.py:636`, `app/ponto/portas/celula.py:649`). Leitores extraem marcos congelados e os encadeiam desde o dia inicial, avançando pela meia-noite (`app/escala/models.py:1124`, `app/escala/servico_jornada.py:69`). Permuta ignora DNA na igualdade, embora edição o compare (`app/ponto/portas/celula.py:1058`, `app/ponto/portas/celula.py:639`). |
| `CelulaDia.origem` | **SEM ACHADO** de significado divergente. | Gerador escreve `gerada`; edição escreve `editada`; regeneração preserva a edição; permuta lê origem na igualdade e na trilha (`app/ponto/portas/celula.py:295`, `app/ponto/portas/celula.py:651`, `app/ponto/portas/celula.py:298`, `app/ponto/portas/celula.py:1058`, `app/ponto/portas/celula.py:1078`). |
| `CelulaDia.editada_em` | **SEM ACHADO** de significado divergente nesses leitores. | Edição escreve instante do ato, serializado na trilha (`app/ponto/portas/celula.py:643`, `app/ponto/portas/celula.py:652`, `app/ponto/portas/celula.py:569`). Leem cartório, validade do veredito, fase e cobrança órfã (`app/ponto/services/cartorio.py:393`, `app/chamados/veredito_lavrado.py:191`, `app/escala/services/fase_12x36.py:188`, `app/escala/services/regeneracao.py:84`). Cartório compara em `(desde,ate]`; é instante, não data do turno (`app/ponto/services/cartorio.py:358`). |
| `LogAuditoria` da permuta | **SEM ACHADO** de escritor paralelo; leitura funcional limitada. | Porta gera UUID, propaga às edições/trocas e grava ação, células, cartas, pedido e motivo pelo escritor `core.observ.evento` (`app/ponto/portas/celula.py:1069`, `app/ponto/portas/celula.py:664`, `app/ponto/portas/celula.py:792`, `app/ponto/portas/celula.py:1081`, `app/core/observ.py:134`). Idempotência lê somente ação/cartas; testes leem vínculos e demais valores (`app/ponto/portas/celula.py:1060`, `app/ponto/tests/test_o10d_permuta_reciproca.py:140`). Datas das células são ISO; criação da trilha é instante separado (`app/ponto/portas/celula.py:1079`, `app/core/models.py:307`). |

Garantia universal sobre todos os emissores históricos de “dia civil versus dia do turno”, ou consumidores externos desses valores: **[nao verificado]**. Nos caminhos examinados, ranges de ausência são inclusivos e marcos noturnos pertencem ao dia inicial do turno (`app/ponto/turnos.py:2811`, `app/escala/servico_jornada.py:70`).

| E — Classe do checklist | Veredito | Presença e prova |
|---|---|---|
| Segundo juiz duplicando registrado | **SEM ACHADO**. | Cobertura, vínculo, trabalho e marcos são consultados nas autoridades existentes (`app/ponto/portas/celula.py:1034`, `app/ponto/portas/celula.py:888`, `app/ponto/portas/celula.py:909`, `app/ponto/portas/celula.py:833`). Os novos predicados de interjornada/DSR permanecem questão distinta do item 4 (`app/ponto/portas/celula.py:940`, `app/ponto/portas/celula.py:956`). |
| Segundo escritor fora da porta | **NAO PROCEDE** no diff. | Composição passa por `trocar_plantao` → `editar_dia`; trilha passa por `evento_kw` → `evento` (`app/ponto/portas/celula.py:1074`, `app/ponto/portas/celula.py:775`, `app/core/observ.py:142`). |
| Relógio fora da borda | **PROCEDENTE** no caminho alcançado, em leitura preexistente. | O novo callback chama `_o_chao_do_dia_mudou`, que lê `localdate()` internamente para decidir quais dias julgar, sem receber o relógio do ato (`app/ponto/portas/celula.py:698`, `app/ponto/portas/celula.py:465`). Isso diverge da regra de injeção para julgamento (`app/docs/PLANO-PADROES.md:101`). |
| Derivação paralela de fonte única | **PROCEDENTE** para o critério de igualdade; **NAO PROCEDE** para a expansão temporal. | Permuta declara igualdade por origem/trabalha/cartas; edição exige também DNA/geradora (`app/ponto/portas/celula.py:1058`, `app/ponto/portas/celula.py:639`). A expansão temporal delega ao helper existente (`app/ponto/portas/celula.py:1034`). |
| Selo/RED sem mordida adequada | **PROCEDENTE**, parcialmente. | Remover R3 ainda gera exceção antes da asserção; quinta carta oferece caminho de asserção (`app/ponto/portas/celula.py:909`, `app/ponto/tests/test_o10d_permuta_reciproca.py:231`, `app/ponto/tests/test_o10d_permuta_reciproca.py:254`). Evidência dos placares/RED históricos citados pelo Opus: **[nao verificado]**. |
| Ordem de trava | **PROCEDENTE** como pendência de compatibilidade. | Porta faz célula → colaborador; outro caminho faz colaborador → escrita de célula (`app/ponto/portas/celula.py:1016`, `app/ponto/portas/celula.py:1019`, `app/ponto/services/veredito_celula.py:226`, `app/ponto/services/veredito_celula.py:352`). |
| Pouso antes do pré-requisito | **SEM ACHADO** de pouso ocorrido. | Pouso real: **[nao verificado]**. Há pré-requisitos ainda abertos: teste declara tipo/aprovação como pergunta e porta recusa datas iguais apesar do contrato declarado (`app/ponto/tests/test_o10d_permuta_reciproca.py:12`, `app/ponto/portas/celula.py:976`, `app/ponto/portas/celula.py:1002`). O corte condiciona construção a perguntas de lei zeradas (`app/docs/CORTES.md:231`). |
| Campo com dois significados | **PROCEDENTE**. | A linha/anexo de ausência vira pedido/carta excluído da regra 2, enquanto precedência/cobertura continuam lendo ausência (`app/ponto/portas/celula.py:914`, `app/ponto/portas/celula.py:922`, `app/ponto/models.py:780`, `app/ponto/precedencia.py:273`). |

| F — Itens não endereçados | Veredito | Permanência e agravamento |
|---|---|---|
| Laudo 2: tipo/status e datas iguais | **PROCEDENTE**. | Continua aceitando qualquer tipo não rejeitado com documento e recusando `d1=d2` (`app/ponto/portas/celula.py:1042`, `app/ponto/portas/celula.py:1043`, `app/ponto/portas/celula.py:1002`). **Sem agravamento identificado pelo delta**: a guarda temporal restringe períodos; não resolve essas escolhas (`app/ponto/portas/celula.py:1035`). |
| Laudo 4: concorrência, plantão cedido, interjornada/DSR | **PROCEDENTE**. | Criação de célula continua sem tradução de `IntegrityError` para recusa; os handlers externos capturam somente recusas próprias (`app/ponto/portas/celula.py:645`, `app/ponto/portas/celula.py:798`, `app/ponto/portas/celula.py:1091`); existe unicidade por colaborador/data (`app/escala/models.py:1833`). Exigência de plantão cedido e predicados próprios permanecem (`app/ponto/portas/celula.py:909`, `app/ponto/portas/celula.py:940`, `app/ponto/portas/celula.py:956`). **Sem agravamento identificado pelo delta**; ocorrência concorrente e aval desses predicados: **[nao verificado]**. |
| Laudo 5: idempotência incompleta | **PROCEDENTE**. | `igual` continua ignorando DNA/geradora e retorna antes das barreiras, enquanto `editar_dia` compara ambos (`app/ponto/portas/celula.py:1058`, `app/ponto/portas/celula.py:1061`, `app/ponto/portas/celula.py:1065`, `app/ponto/portas/celula.py:639`). **Sem agravamento identificado pelo delta**. Justificativa documental do descarte anterior: **[nao verificado]**. |

## 17/11 05:42 · sessao 0d6b5650b349 · - · - · origem=Ronald (tmux)

| Item | Veredito | Prova |
|---|---|---|
| B — comentários novos descrevem a porta | NAO PROCEDE a acusação de ordem universal nos trechos corrigidos | A troca adquire carta → células → colaboradores (`app/ponto/portas/celula.py:735`, `:754`, `:758`); a permuta faz o mesmo (`:1014`, `:1016`, `:1019`). Comentários restringem a afirmação à porta (`:744`, `:1006`) e a docstring também (`:971`). |
| B — `GLOBAL` residual | SEM ACHADO de universalidade entre tabelas nesse trecho | `app/ponto/portas/celula.py:770` fala especificamente da ordenação **entre células** por colaborador/data, executada pelo `sorted` em `:773`. |
| B — relação com os outros escritores citados | NAO PROCEDE como descrição de precedência local; ressalvar aquisição implícita | `editar_dia` faz SELECT FOR UPDATE da célula (`app/ponto/portas/celula.py:638`) antes do marcador (`:672`). Regeneração escreve célula (`:307`, `:311`) antes do marcador (`:328`), sem SELECT FOR UPDATE nesse corpo. O marcador chama `renovar_contexto` (`app/ponto/management/commands/gerar_celulas.py:264`), que chama a porta de trava antes do save (`app/chamados/services/contexto.py:103` → `app/chamados/models.py:487`). `decidir` abre atomic (`app/ponto/services/ausencia.py:1079`), altera Ausencia (`:1088` → `:988`); seu post_save chama o cartório (`app/ponto/signals.py:143`, `:147`, `:93`). |
| B — compatibilidade global / garantia contra deadlock | PROCEDENTE como pendência; execução concorrente [nao verificado] | Existe o caminho inverso na casa: `veredito_celula` trava colaborador dentro do atomic (`app/ponto/services/veredito_celula.py:224`, `:226`) e depois lavra a célula (`:351`, `:352`; escrita em `app/ponto/portas/celula.py:1456`). O teste declara “o inverso deadlocka” (`app/ponto/tests/test_o10d_permuta_reciproca.py:404`), mas apenas captura SQL e confere ordem (`:418`, `:421`); não mede concorrência. |
| B — cauda Disputa/Chamado/Pergunta | SEM ACHADO, com precisão de escopo | A chamada da permuta em `app/ponto/portas/celula.py:1019` só solicita colaboradores. `trava_na_ordem` trava C em `app/chamados/models.py:2771`; D, Ch e P dependem dos argumentos/flags (`:2779`, `:2792`, `:2796`). Não afirmar que a chamada inicial já trava as quatro tabelas. |
| C — R3, inversão `is None` → `is not None` | NAO PROCEDE a crítica de mordida somente por exceção para essa mutação | Inversão em `app/ponto/portas/celula.py:888` produz frases para os três vínculos existentes e retorna em `:890`; a nova igualdade exata em `app/ponto/tests/test_o10d_permuta_reciproca.py:231` falha por asserção. Confirmação adicional: AST real executada em memória, com fontes/imports substituídos por stubs, deu `AssertionError`; suíte Django integral [nao verificado]. |
| C — R3, remoção/desativação da guarda | PROCEDENTE: continua mordendo por exceção | O teste encerra o vínculo SUB em 03/11 (`app/ponto/tests/test_o10d_permuta_reciproca.py:227`); sem a guarda de `app/ponto/portas/celula.py:888`, chega a `.eh_dia_trabalho` sobre None em `:909`. O except local captura apenas `_NaoSei` (`:959`), e o externo apenas `_PermutaRecusada` (`:1091`). AST em memória confirmou `AttributeError`; a assertiva nova não é alcançada. |
| C — R5 quinta carta, remoção/inversão da guarda inteira | NAO PROCEDE a crítica de exceção para esse teste novo | Há quatro chaves certas mais a quinta (`app/ponto/tests/test_o10d_permuta_reciproca.py:259`, `:260`), portanto construir `_c` não perde chave (`app/ponto/portas/celula.py:1029`). Removida/invertida a guarda `:1026`, a troca está mockada (`app/ponto/tests/test_o10d_permuta_reciproca.py:261`); o fluxo alcança retorno `'permutada'` (`app/ponto/portas/celula.py:1097`) e a igualdade com `'recusada'` do helper falha (`app/ponto/tests/test_o10d_permuta_reciproca.py:106`). Execução integral [nao verificado]. |
| C — outra forma ainda produz exceção | PROCEDENTE | O teste antigo fornece somente três cartas (`app/ponto/tests/test_o10d_permuta_reciproca.py:248`). Remover/inverter a guarda inteira de `app/ponto/portas/celula.py:1026` nesse caso produz `KeyError` em `_por_celula[chave]` (`:1029`), antes da assertiva de retorno (`app/ponto/tests/test_o10d_permuta_reciproca.py:250`). |

## 17/11 05:42 · sessao 6d62fa42aad5 · - · - · origem=Ronald (tmux)

| Campo | Escritores e leitores comprovados | Significado / achado |
|---|---|---|
| `Ausencia.tipo` | Nascimento por `criar_ausencia`: `app/ponto/services/ausencia.py:483`; edição por `editar_ausencia_registro`: `app/ponto/services/ausencia.py:648`. Catálogo determina efeitos e tipos em aberto: `app/ponto/models.py:680`, `app/ponto/models.py:689`. Permuta lê para expansão: `app/ponto/portas/celula.py:1036`; barreira lê para descrever sobreposição: `app/ponto/portas/celula.py:923`. | **PROCEDENTE: papel duplo de ausência e carta.** A permuta chama suas cartas de “próprio pedido” e as exclui da regra 2: `app/ponto/portas/celula.py:914`, `app/ponto/portas/celula.py:922`; os leitores de precedência continuam lendo essas linhas como ausências: `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:335`. O fixture usa `declaracao`, não tipo permuta: `app/ponto/tests/test_o10b_editar_dia_troca.py:106`. |
| `Ausencia.data` | Nascimento: `app/ponto/services/ausencia.py:484`; edição: `app/ponto/services/ausencia.py:649`. Permuta associa carta a `(colaborador_id, data)`: `app/ponto/portas/celula.py:1020`; troca usa essa mesma data para editar a célula: `app/ponto/portas/celula.py:775`. Cobertura lê início de intervalo: `app/ponto/turnos.py:2798`, `app/ponto/turnos.py:2809`. | A data é chave do dia e início do intervalo; a expansão trabalha com datas inclusivas: `app/ponto/turnos.py:2809`, `app/ponto/turnos.py:2811`. Na ausência parcial noturna, esse dia é explicitamente o início do turno: `app/ponto/models.py:765`, `app/ponto/turnos.py:2639`. Uma garantia geral de que todo emissor histórico entenda sempre “dia civil” ou sempre “dia de início do turno” é **[nao verificado]**. |
| `Ausencia.data_fim` | Derivador da porta declara fim inclusivo: `app/ponto/services/ausencia.py:198`. Nascimento: `app/ponto/services/ausencia.py:485`; edição: `app/ponto/services/ausencia.py:650`; `save` valida fim e deriva `dias_corridos`/fim: `app/ponto/models.py:904`, `app/ponto/models.py:917`. Permuta lê: `app/ponto/portas/celula.py:1036`; juízes leem: `app/ponto/turnos.py:2784`, `app/ponto/turnos.py:2803`, `app/ponto/turnos.py:2844`. | **SEM ACHADO de exclusividade divergente nesses caminhos:** é inclusivo; `None` significa aberto só nos tipos em aberto, senão dia único: `app/ponto/turnos.py:2803`, `app/ponto/turnos.py:2811`. O help text generaliza `NULL = indeterminado/em aberto`, sem a ressalva de tipo: `app/ponto/models.py:751`; o código vivo faz a distinção: `app/ponto/turnos.py:2805`. |
| `Ausencia.status` | Nascimento: `app/ponto/services/ausencia.py:486`; transição por `mudar_estado`: `app/ponto/services/ausencia.py:955`, `app/ponto/services/ausencia.py:976`; decisão chama essa porta: `app/ponto/services/ausencia.py:1088`. Exceção declarada para espelhos de férias escreve rejeição diretamente: `app/ponto/services/ausencia.py:1496`. Permuta só rejeita `rejeitada`: `app/ponto/portas/celula.py:1043`. | **PROCEDENTE quanto à diferença de efeito:** folha só lê aprovadas; cobrança lê todas as não rejeitadas: `app/ponto/turnos.py:2523`. A permuta aceita uma aprovada ou aguardando decisão como carta sem distinguir seu efeito: `app/ponto/portas/celula.py:1043`; continua excluindo a aprovada do dia exato da regra 2: `app/ponto/portas/celula.py:922`. |
| `Ausencia.documento` | Nascimento: `app/ponto/services/ausencia.py:492`; anexo posterior: `app/ponto/services/ausencia.py:1017`; reabertura: `app/ponto/services/ausencia.py:1307`. Estado inicial lê presença: `app/ponto/services/ausencia.py:926`; propriedade `tipo_real` abre arquivo: `app/ponto/models.py:855`. Permuta testa somente presença: `app/ponto/portas/celula.py:1042`; trilha da troca copia nome: `app/ponto/portas/celula.py:784`. | Modelo o descreve como comprovante da ausência, preenchido no primeiro dia do intervalo: `app/ponto/models.py:777`. Permuta toma sua presença como carta da permuta: `app/ponto/portas/celula.py:1042`. Existência/conteúdo do arquivo não são comprovados por essa guarda; a leitura efetiva do arquivo está em outro leitor: `app/ponto/models.py:861`. |
| `CelulaDia.trabalha` | Fluxo da obra escreve via duas trocas: `app/ponto/portas/celula.py:1073`; troca chama `editar_dia` com False/True: `app/ponto/portas/celula.py:773`; escritor cria/atualiza: `app/ponto/portas/celula.py:645`, `app/ponto/portas/celula.py:648`. Gerador também escreve: `app/ponto/portas/celula.py:294`, `app/ponto/portas/celula.py:307`. Leitor autoridade: `app/escala/models.py:1092`, `app/escala/models.py:1095`; barreiras: `app/ponto/portas/celula.py:909`, `app/ponto/portas/celula.py:916`. | True trabalho, False folga, None indeterminado: `app/escala/models.py:1686`. A permuta assume dia normal do próprio template: `app/ponto/portas/celula.py:709`. **SEM ACHADO de escritor novo fora da porta nesse fluxo:** composição chama o escritor existente: `app/ponto/portas/celula.py:775`. |
| `CelulaDia.dna` | Produtor único `montar_dna`: `app/ponto/management/commands/gerar_celulas.py:73`; só congela marcos quando True: `app/ponto/management/commands/gerar_celulas.py:88`. `editar_dia` o solicita: `app/ponto/portas/celula.py:636`; grava: `app/ponto/portas/celula.py:649`. `EscalaColaborador.marcos_do_dia` lê DNA: `app/escala/models.py:1124`; `marcos_turno` lê essa autoridade e encadeia meia-noite: `app/escala/servico_jornada.py:69`, `app/escala/servico_jornada.py:90`. | Horários congelados do dia de início, capazes de avançar para D+1: `app/escala/servico_jornada.py:69`, `app/escala/servico_jornada.py:89`. **PROCEDENTE no item 5:** idempotência da permuta ignora DNA, enquanto a edição compara DNA e geradora: `app/ponto/portas/celula.py:1058` versus `app/ponto/portas/celula.py:639`. |
| `CelulaDia.origem` | Gerador cria `gerada`: `app/ponto/portas/celula.py:295`; edição escreve `editada`: `app/ponto/portas/celula.py:651`. Regeneração condiciona escrita à origem gerada: `app/ponto/portas/celula.py:298`; permuta lê origem na idempotência: `app/ponto/portas/celula.py:1058`; trilha copia: `app/ponto/portas/celula.py:1078`. | Procedência humana versus geração: `app/escala/models.py:1638`, `app/escala/models.py:1690`. **SEM ACHADO de significado temporal/civil divergente:** campo representa proveniência, conforme modelo e escritores: `app/escala/models.py:1690`, `app/ponto/portas/celula.py:651`. |
| `CelulaDia.editada_em` | Edição obtém relógio e escreve: `app/ponto/portas/celula.py:643`, `app/ponto/portas/celula.py:652`. Trilha serializa: `app/ponto/portas/celula.py:569`. Leitores: fato novo do cartório `app/ponto/services/cartorio.py:393`; validade de veredito `app/chamados/veredito_lavrado.py:191`; fase 12x36 `app/escala/services/fase_12x36.py:188`; cobrança órfã usa máximo dos carimbos `app/escala/services/regeneracao.py:72`. | Instante do ato de edição, não data do turno; comparação do cartório é `(desde, ate]`: `app/ponto/services/cartorio.py:393`. Comparação de reescrita usa instante mais novo: `app/escala/services/regeneracao.py:84`. **SEM ACHADO de significados divergentes nesses leitores.** |
| `LogAuditoria` da permuta | UUID nasce em `app/ponto/portas/celula.py:1069`; é passado às trocas/edições: `app/ponto/portas/celula.py:1071`, `app/ponto/portas/celula.py:664`, `app/ponto/portas/celula.py:792`. Evento da permuta escreve id, quatro células, cartas, pedido e motivo: `app/ponto/portas/celula.py:1081`. Adaptador leva ao escritor real: `app/core/observ.py:142`, `app/core/observ.py:134`; instante de criação automático: `app/core/models.py:307`. Leitor funcional específico comprovado é idempotência por ação/cartas: `app/ponto/portas/celula.py:1060`; testes leem os demais valores: `app/ponto/tests/test_o10d_permuta_reciproca.py:140`. | Datas de células são ISO de `d1`/`d2`: `app/ponto/portas/celula.py:1079`; instante do evento é separado: `app/core/models.py:307`. O leitor de idempotência não verifica DNA/geradora nem o estado completo registrado: `app/ponto/portas/celula.py:1058`. Outros consumidores funcionais específicos de `permuta_id`/`pedido_id` fora desses caminhos são **[nao verificado]**. |

O delta não converte cartas em tipo próprio nem altera seu status; a obra as lê e exclui como pedido, preservando registros que os leitores normais seguem tratando como ausência: `app/ponto/portas/celula.py:1035`, `app/ponto/portas/celula.py:1043`, `app/ponto/portas/celula.py:922`, `app/ponto/precedencia.py:273`. Portanto o conserto de intervalo não encerra o problema de papel/efeito das cartas apontado no item 2: `app/ponto/portas/celula.py:1035`, `app/ponto/portas/celula.py:1043`.

## 17/11 05:41 · sessao 33f0533d9377 · - · - · origem=Ronald (tmux)

| Item A | Veredito | Prova |
|---|---|---|
| Carta de vários dias aceita pela guarda | **NAO PROCEDE**, para datas válidas representáveis pela janela | `dias_da_ausencia` usa os mesmos ramos do expansor de cobertura: fim preenchido, fim aberto por catálogo, ou dia único (`app/ponto/turnos.py:2783-2794`, `app/ponto/turnos.py:2803-2813`). Na janela inclusiva `[data,data+1]`, fim posterior ou aberto retorna dois dias; fim igual ou ausência sem fim de tipo não aberto retorna `[data]` (`app/ponto/portas/celula.py:1034-1039`). |
| Carta legítima de um dia recusada por essa distinção | **SEM ACHADO** | Fim igual ao início e fim nulo de tipo não aberto retornam `[data]`; o filtro aceita ambos (`app/ponto/turnos.py:2803-2813`, `app/ponto/portas/celula.py:1035-1039`). |
| Ausência fora do dia exato escapando pelo `excluir_pks` | **NAO PROCEDE**, nessa passagem | As quatro cartas precisam corresponder às quatro células e nenhuma quinta linha existente cabe na cardinalidade; todas passam pela guarda antes de seus pks chegarem às barreiras (`app/ponto/portas/celula.py:1022-1039`, `app/ponto/portas/celula.py:1065`). A exclusão acontece em `app/ponto/portas/celula.py:922`, implementada por `app/ponto/turnos.py:2839-2847`. |
| Atestado aprovado do dia tratado como carta | **PROCEDENTE**, permanece questão de tipo/status do item 2 | A porta exige documento e ausência de rejeição, sem exigir tipo específico; a carta aceita sai da sobreposição (`app/ponto/portas/celula.py:1042-1052`, `app/ponto/portas/celula.py:922`). Isso permite ausência legítima de **um dia**, mas não constitui escape de ausência fora do dia exato (`app/ponto/turnos.py:2803-2813`). |
| Janela `+1` como regra do juiz | **NAO PROCEDE** se apresentada como janela prescrita pelo juiz; **SEM ACHADO** de duplicação da expansão | O juiz recebe limites arbitrários; a porta escolhe `data+1` para observar um eventual segundo dia (`app/ponto/turnos.py:2798-2809`, `app/ponto/portas/celula.py:1032-1037`). A expansão é delegada ao helper existente, reconhecido como o mesmo juiz no registro (`app/core/juizes.py:452-455`). |
| Dia civil versus turno noturno | **SEM ACHADO** de divergência entre os juízes pedidos | Os filtros consultados usam `data`/`data_fim` inclusivos, sem deslocamento de data por horário (`app/ponto/turnos.py:2730-2733`, `app/ponto/turnos.py:2773-2774`, `app/ponto/turnos.py:2843-2847`). A ausência parcial noturna pertence explicitamente ao dia de início do turno (`app/ponto/models.py:765-771`, `app/ponto/turnos.py:2639-2644`). |
| Dados malformados e extremo de calendário | **SEM ACHADO** de escape; limites merecem ressalva | `data_fim<data` produz expansão vazia e recusa, embora filtros SQL ainda selecionem a data inicial; `save` proíbe essa inversão (`app/ponto/turnos.py:2790-2794`, `app/ponto/turnos.py:2731`, `app/ponto/models.py:904-906`). Em `date.max`, `data+1` estoura; os próprios expansores também incrementam após incluir o último dia e estouram nesse extremo (`app/ponto/portas/celula.py:1036`, `app/ponto/turnos.py:2794`, `app/ponto/turnos.py:2813`). Existência dessas bordas em produção: **[nao verificado]**. |

## 17/11 05:40 · sessao 41cf8ec1af63 · O10-D-PERMUTA-RECIPROCA · censo · origem=Code

A classificação resulta em **2 partes (a), 6 partes (b), 3 partes (c), nenhuma (d) e 1 parte (e)**. A unidade dessa contagem é cada parte discriminada abaixo; itens compostos aparecem em mais de uma linha.

As referências dos anexos contam desde a primeira linha após o delimitador, incluindo linhas vazias. “Não achado” significa ausência de trecho específico nos anexos fornecidos.

**1. Ausência aberta aceita como carta e excluída da sobreposição — (a), falha do Codex.**

- **Pergunta:** “(2) sobreposicao com turno previsto ou ausencia barra;” (`verif_v1.q:10`); “Ausencia.status/documento/data/data_fim” (`verif_v1.q:25`); “significado (dia civil x dia do turno, inclusivo x exclusivo, fuso)?” (`verif_v1.q:27`).
- **Resposta:** “Cobertura e sobreposição usam intervalo fechado; a permuta confere posição e fim igual ao início ou nulo.” (`verif_v1.r:60`). Também reconheceu: “nulo significa aberto somente para determinados tipos.” (`verif_v1.r:73`).
- **O que não viu:** o contraexemplo que combina esses fatos: o intervalo aberto passa em `_largas` e seu próprio ID é excluído da barreira de ausência. O código confirma as duas condições. (`app/ponto/portas/celula.py:1027`; `app/ponto/portas/celula.py:920`)
- **Triagem desse contraexemplo:** não achada. A entrega reconhece o conflito geral entre carta e ausência, mas não esse escape específico. (`entrega.md:65`)

A resposta descreveu os ingredientes sem identificar a violação resultante; isso sustenta **(a)**, inclusive para a ausência real aprovada reutilizada como carta nesse mesmo mecanismo. (`verif_v1.r:60`; `verif_v1.r:71`; `verif_v1.r:73`)

**2. Tipo/status permissivos e recusa de `d1=d2` — duas partes (b), falha da triagem.**

**2.1. Tipo e aprovação das cartas — (b).**

- **Pergunta:** “tipo permuta, aprovada, dia exato, ligada ao id da permuta” (`verif_v1.q:12`).
- **Resposta:** “**Não exige tipo nem aprovação e não grava vínculo na Ausencia.**” (`verif_v1.r:13`). A última volta reiterou: “**Não rejeitada não significa aprovada.**” (`verif_v3.r:91`).
- **Triagem:** “Nao curados (TOPO): d1 = d2 (1), tipo/aprovacao da carta (2, 8)” (`entrega.md:150`).
- **Motivo dado:** exigir aprovação com o tipo atual produziria abono indevido; seria necessário tipo `permuta` sem efeito, tratado como outra fatia. (`entrega.md:18`; `entrega.md:20`; `entrega.md:21`)

**2.2. `d1=d2` recusado — (b).**

- **Pergunta:** “d1 = d2 permitido” (`verif_v1.q:11`).
- **Resposta:** “**proíbe expressamente a igualdade**.” (`verif_v1.r:12`).
- **Triagem:** “**AVAL item 2 literal NAO cumprido: "d1 = d2 aceito".**” (`entrega.md:5`); mantido entre os não curados. (`entrega.md:150`)
- **Motivo dado:** o DNA usa o template do próprio vínculo; representar o turno do outro exigiria outro desenho. A entrega manteve a recusa e declarou que não a selou para não congelar o contrário do contrato. (`entrega.md:8`; `entrega.md:14`)

**3. Ordem congelada e afirmação de ordem universal — partes (a), (c) e (b).**

**3.1. Selos TRAVA consolidando a escolha ainda contestada — (a).**

- **Pergunta:** “TABELA DE ORDEM DE TRAVA” (`verif_v3.q:11`); “Os achados da sua volta 2 [...] quais [...] continuam” (`verif_v3.q:20`); “Os testes novos/reescritos falhariam com o codigo da volta 2” (`verif_v3.q:21`). [nao verificado]
- **Resposta:** os testes de ordem são descritos como “**Falharia**” contra a v2, enquanto a própria resposta afirma que o problema de ordem “**Continua globalmente**”. (`verif_v3.r:77`; `verif_v3.r:78`; `verif_v3.r:69`) [nao verificado]
- **Crítica específica de que os selos congelavam escolha sem aval:** não achada na resposta. A asserção exige precisamente `['carta', 'celula', 'colaborador']`. (`app/ponto/tests/test_o10d_permuta_reciproca.py:358`)
- **Entrega:** reconhece explicitamente “**Selo congela a escolha:**” e informa que os selos inverteriam se o corte fosse C primeiro. (`entrega.md:55`; `entrega.md:56`)

O Codex examinou conjuntamente a ordem ainda problemática e os testes que a fixavam, mas não apontou esse defeito dos selos; não atribuo à resposta uma crítica que ela não escreveu. (`verif_v3.r:69`; `verif_v3.r:77`; `verif_v3.r:78`) [nao verificado]

**3.2. M24/M25 — (c), fora do alcance.**

- **Pergunta específica sobre M24/M25:** não achada.
- **Resposta específica sobre M24/M25:** não achada.
- **Entrega:** identifica os dois mutantes como instrumentos que “cravam X antes de C”. (`entrega.md:55`)

Não conto a avaliação desses mutantes como falha do Codex: mutantes pertencem à exclusão **(c)** determinada no pedido. A falha estática dos selos visíveis foi contabilizada separadamente em 3.1. (`entrega.md:55`; `app/ponto/tests/test_o10d_permuta_reciproca.py:358`)

**3.3. Comentários/docstring afirmando ordem universal — (b).**

- **Pergunta:** inventariar os caminhos de trava e os ciclos entre eles. (`verif_v3.q:11`; `verif_v3.q:15`)
- **Resposta literal:** “A alegada ‘ordem da casa’ não corresponde a todas as transações existentes.” — no original, aspas tipográficas duplas em “ordem da casa”. (`verif_v3.r:1`)
- **Triagem:** reconhece que comentários e docstring “dizem mais do que o vivo” e determina “ajustar o texto no pouso”. (`entrega.md:57`; `entrega.md:58`) [nao verificado]
- **Motivo dado para adiar:** “nao mexi para nao descertificar a suite”. (`entrega.md:58`) [nao verificado]

**4. TOPO incompleto — partes (b), (e) e (b).**

**4.1. `IntegrityError` na criação concorrente — (b).**

- **Pergunta:** “CELULA AUSENTE” e concorrência “criando a MESMA celula”. (`verif_v3.q:17`; `verif_v3.q:18`)
- **Resposta:** “**Corrida de criação também pode resultar em erro de unicidade**, mesmo sem deadlock: os criadores não recuperam a célula concorrente, e troca/permuta capturam apenas suas exceções de recusa.” (`verif_v3.r:104`)
- **Triagem:** “corrida de criacao pode dar IntegrityError de unicidade em vez / de deadlock”. (`entrega.md:158`; `entrega.md:159`)
- **Motivo dado:** “PROCEDENTE e NAO curavel aqui”, enquadrado como o mesmo desenho de concorrência. (`entrega.md:157`; `entrega.md:159`)

A remissão “TOPO 7” está errada: esse tópico trata de “ambos ativos”, não do erro de unicidade. Assim, a triagem reconheceu o achado, mas não o colocou corretamente no TOPO. (`entrega.md:159`; `entrega.md:61`)

**4.2. Regra adicional “dia cedido tem de ser plantão” — (e), não perguntado.**

- **Pergunta específica sobre a legitimidade desse predicado:** não achada. O trecho próximo é “(6) compensacao, nao folga trabalhada; sem DSR na semana barra.” (`verif_v1.q:12`)
- **Resposta específica sobre esse predicado adicional:** não achada. O tratamento próximo descreve o cedido como folga e a verificação semanal, sem questionar essa exigência. (`verif_v1.r:14`)
- **Linha de triagem/pergunta ao TOPO:** não achada.
- **Reconhecimento pelo construtor:** o código justifica expressamente “o dia cedido e PLANTAO de quem cede”; a entrega registra a recusa “nao tem plantao previsto 14/11”. (`app/ponto/portas/celula.py:905`; `entrega.md:119`)

Classifico como **(e)** porque não foi perguntada a legitimidade dessa regra adicional. Não cabe **(d)**: o construtor conhecia e justificava o predicado, embora não o tivesse apresentado como pergunta. (`app/ponto/portas/celula.py:905`; `entrega.md:119`)

**4.3. Interjornada/DSR como decisões novas sem juiz — (b).**

- **Pergunta:** “a interjornada e o DSR locais sao juiz novo?” (`verif_v1.q:29`).
- **Resposta:** “As guardas locais **são decisões novas**, com piso de11h e semana segunda–domingo. Registro/corte específico delas: **não achei** [nao verificado].” (`verif_v1.r:79`)
- **Triagem:** admite interjornada/DSR “sem juiz registrado”, alegando composição de guardas locais e proibição de registrar autoridade nova. (`entrega.md:22`; `entrega.md:23`)
- **Encaminhamento dado:** confirmação com o DP. (`entrega.md:24`)

O Codex apontou a natureza de decisão nova; a triagem manteve a implementação sob essa justificativa. (`verif_v1.r:79`; `entrega.md:23`)

**5. Descarte da idempotência sem DNA/geradora — (b), falha da triagem.**

- **Pergunta:** “A idempotencia ('igual') pode dar falso positivo” por células editadas por outra operação. (`verif_v1.q:32`)
- **Resposta:** “Entretanto, não compara DNA, geradora, autor, carimbo, pedido, papéis ou se a última edição ainda pertence àquela permuta.” (`verif_v1.r:93`). Reiterado na última volta. (`verif_v3.r:103`)
- **Triagem:** “Descartado com motivo: idempotencia `igual` nao / compara DNA”. (`entrega.md:153`; `entrega.md:154`)
- **Motivo literal:** “o DNA e funcao de `trabalha` + vinculo pelo produtor unico P13; repetir com cadastro mudado e / regeneracao, nao permuta”. (`entrega.md:154`; `entrega.md:155`)

O descarte não elimina a diferença constatável: `editar_dia` compara DNA e geradora; o retorno `igual` da permuta compara origem/trabalho e existência de trilha com as cartas. (`app/ponto/portas/celula.py:639`; `app/ponto/portas/celula.py:1048`; `app/ponto/portas/celula.py:1050`)

**6. M06/M09, placar contaminado por ERROR e RED por módulo inexistente — (c), fora do alcance.**

- **Pergunta próxima:** “selo sem mordida (algum teste / passaria com o codigo da base ou com a regra removida?)”. (`verif_v1.q:30`; `verif_v1.q:31`) [nao verificado]
- **Resposta:** “Revisão somente por leitura; testes e concorrência executados: [nao verificado].” (`verif_v1.r:3`) [nao verificado]
- **Tratamento específico dos resultados M06/M09, `GREEN_v3 errors=1` e desse RED:** não achado nas respostas.
- **Entrega:** declara “RED = modulo inexistente” e reconhece fixture que “dava ERROR em todo run”. (`entrega.md:95`; `entrega.md:104`; `entrega.md:105`)

São defeitos da evidência de execução relatados pelo opus; não contam como falha do Codex neste censo. M06/M09 morrerem especificamente por exceção consta do laudo, sem resultado correspondente nas `.r`. (`opus_final.md:7`)

**7. Sombra: drenagem dentro do atomic e FM de dois colaboradores — (c), fora do alcance.**

- **Pergunta próxima:** “Existe caminho em que algo saia da casa [...] ANTES / do commit do ato inteiro”. (`verif_v1.q:18`; `verif_v1.q:19`) [nao verificado]
- **Resposta próxima:** “o rabo normal fica em `on_commit`, registrado dentro do atomic correspondente”. Isso examina a porta, não certifica o instrumento de sombra. (`verif_v1.r:24`)
- **Pergunta/resposta específica sobre `o10d_permuta_diff.py`, drenagem manual dentro do atomic ou universo do MD5:** não achada.
- **Entrega:** apresenta a sombra como `atomic+raise`, menciona callbacks “drenados” e declara “FM intacto True”; não registra as duas limitações apontadas pelo opus. (`entrega.md:113`; `entrega.md:128`; `entrega.md:121`)

A drenagem dentro do atomic e o MD5 restrito a dois colaboradores são achados do laudo sobre o instrumento e sua medição; ficam em **(c)**. Sua confirmação independente pelo script/resultado original é **[nao verificado]**. (`opus_final.md:8`)

| Item / parte | Classe |
|---|---|
| 1 — ausência real aceita e excluída da barreira | **(a)** |
| 2.1 — tipo/aprovação | **(b)** |
| 2.2 — `d1=d2` | **(b)** |
| 3.1 — selos TRAVA congelam escolha | **(a)** |
| 3.2 — M24/M25 | **(c)** |
| 3.3 — comentários/docstring universais | **(b)** |
| 4.1 — `IntegrityError` | **(b)** |
| 4.2 — plantão obrigatório no dia cedido | **(e)** |
| 4.3 — decisões locais sem juiz | **(b)** |
| 5 — idempotência | **(b)** |
| 6 — RED/mutantes/ERROR | **(c)** |
| 7 — instrumento e universo da sombra | **(c)** |
| **Contagem das 12 partes** | **(a) 2 · (b) 6 · (c) 3 · (d) 0 · (e) 1** |

**Não há caso (d), MODO-COMUM, demonstrado pelos anexos.** O caso não perguntado do item 4.2 era conhecido pelo construtor; os demais ficaram classificados pelas evidências acima. (`app/ponto/portas/celula.py:905`; `entrega.md:119`)

## 17/11 05:38 · sessao 948a32929507 · - · - · origem=Ronald (tmux)

Numeração contada desde a primeira linha de cada anexo, incluindo vazias.

| Anexo | Linha | Trecho literal |
|---|---:|---|
| verif_v1.q | 10 | “(2) sobreposicao com turno previsto ou ausencia barra;” |
| verif_v1.q | 11 | “tipos de escala diferentes ok; (4) qualquer intervalo, d1 = d2 permitido, competencia nao exportada; (5) carta = Ausencia” |
| verif_v1.q | 12 | “tipo permuta, aprovada, dia exato, ligada ao id da permuta; (6) compensacao, nao folga trabalhada; sem DSR na semana barra.” |
| verif_v1.q | 16 | “1. Para cada regra (1)-(6) e cada item do contrato: o codigo a cumpre, cumpre em parte, ou nao cumpre? Contra-exemplo” |
| verif_v1.q | 18 | “2. Existe caminho em que algo saia da casa (push, julgamento gravado, recalculo de dinheiro, chamado, log externo) ANTES” | [nao verificado]
| verif_v1.q | 21 | “3. Ordem de travas: permuta x permuta cruzada, permuta x trocar_plantao, permuta x regenerar_celulas_vinculo/” |
| verif_v1.q | 25 | “LogAuditoria.valor_depois permuta_id/cartas, Ausencia.status/documento/data/data_fim): arquivo:linha de cada” |
| verif_v1.q | 27 | “significado (dia civil x dia do turno, inclusivo x exclusivo, fuso)?” |
| verif_v1.q | 29 | “(a interjornada e o DSR locais sao juiz novo?);” |
| verif_v1.q | 30 | “selo sem mordida (algum teste” | [nao verificado]
| verif_v1.q | 31 | “passaria com o codigo da base ou com a regra removida?); ordem de trava; campo com dois significados.” |
| verif_v1.q | 32 | “6. A idempotencia ('igual') pode dar falso positivo (celulas editadas por OUTRA operacao com o mesmo estado, trilha de” |
| verif_v2.q | 14 | “2. A trava nova cria ORDEM INVERTIDA com algum caminho que ja trava CelulaDia, Ausencia, Chamado ou Pergunta ANTES de” |
| verif_v2.q | 21 | “5. Os testes novos do delta mordem (passariam com o codigo do hash b85315b2b)? Algum e vacuo?” |
| verif_v3.q | 11 | “1. TABELA DE ORDEM DE TRAVA. Para cada caminho -- permutar_plantoes, trocar_plantao isolada, editar_dia direto,” |
| verif_v3.q | 17 | “2. CELULA AUSENTE: quando uma das celulas nao existe (fora do horizonte gerado), ha ciclo entre a permuta/troca e” |
| verif_v3.q | 18 | “editar_dia direto ou a geracao/regeneracao criando a MESMA celula? Compare com o codigo do hash (sem o delta): o delta” |
| verif_v3.q | 21 | “4. Os testes novos/reescritos falhariam com o codigo da volta 2 (colaborador travado antes das cartas; guarda de dono” | [nao verificado]

Respostas relevantes:

- **verif_v1.r:12:** “**proíbe expressamente a igualdade**.”
- **verif_v1.r:13:** “**Não exige tipo nem aprovação e não grava vínculo na Ausencia.**”
- **verif_v1.r:60:** “Cobertura e sobreposição usam intervalo fechado; a permuta confere posição e fim igual ao início ou nulo.”
- **verif_v1.r:71:** “A exclusão só existe na guarda da própria permuta.”
- **verif_v1.r:73:** “`data_fim` é **inclusiva**; nulo significa aberto somente para determinados tipos.”
- **verif_v1.r:79:** “As guardas locais **são decisões novas**, com piso de11h e semana segunda–domingo. Registro/corte específico delas: **não achei** [nao verificado].”
- **verif_v1.r:93:** “Entretanto, não compara DNA, geradora, autor, carimbo, pedido, papéis ou se a última edição ainda pertence àquela permuta.”
- **verif_v3.r:1:** “A alegada ‘ordem da casa’ não corresponde a todas as transações existentes.” Atenção: as aspas no original são tipográficas duplas: `“ordem da casa”`.
- **verif_v3.r:69:** linha da tabela sobre ordem invertida: “**Continua globalmente:** outra ausência, desligamento, INSS, Chamado e transações de várias etapas têm C antes de algum X posterior.”
- **verif_v3.r:70:** “**Continua sem demonstrar concorrência ou célula ausente.**”
- **verif_v3.r:77–78:** testes de ordem na permuta e troca “**Falharia**” contra v2. [nao verificado]
- **verif_v3.r:84:** “o helper reconhece a presença do SQL `FOR UPDATE`, não a quantidade de linhas retornadas; uma consulta vazia também acrescentaria ‘celula’ à ordem.” Original usa aspas duplas tipográficas em `“celula”`.
- **verif_v3.r:91:** “**Não rejeitada não significa aprovada.**”
- **verif_v3.r:103:** “`igual` compara origem/trabalha e existência de trilha com as cartas; não compara DNA, geradora ou última edição, e retorna antes das barreiras atuais.”
- **verif_v3.r:104:** “**Corrida de criação também pode resultar em erro de unicidade**, mesmo sem deadlock: os criadores não recuperam a célula concorrente, e troca/permuta capturam apenas suas exceções de recusa.”
- **verif_v3.r:105:** “**Tipo/aprovação das cartas e mesmo dia continuam pendentes.**”

Os pontos `verif_v1.r:60`, `:73` e `verif_v3.r:104` coincidem com as referências fornecidas pelo opus.

## 17/11 05:37 · sessao bddc04b98043 · - · - · origem=Ronald (tmux)

Numeração de `entrega.md` contando linhas vazias, com `# O10-D...` como linha 1; texto total: 180 linhas.

| Tema | Linhas e trecho literal |
|---|---|
| `d1=d2` | `entrega.md:5`: `1. **AVAL item 2 literal NAO cumprido: "d1 = d2 aceito".** PERGUNTA DE DESENHO. A porta RECUSA d1 = d2 com frase` |
| Motivo para adiamento de `d1=d2` | `entrega.md:8`: `os marcos sao os do template do PROPRIO vinculo. Com d1 = d2 os dois ja trabalham no dia; "trocar o turno" so` — complementa `:9-14`; `:14`: `muda). A recusa NAO tem selo nem mutante de proposito, para nao travar o oposto do contrato.` |
| Tipo/status | `entrega.md:15`: `2. **Regra (5) em parte: carta "tipo permuta, aprovada".** Nao existe `tipo='permuta'` em `ponto/models.py`/` |
| Qualquer ausência | `entrega.md:16`: `` `ponto/catalogo/ausencias.py`; a porta aceita qualquer `Ausencia` com `documento`, do dia exato, da celula certa,`` |
| Motivo tipo/status | `entrega.md:18`: ``conferido: exigir `aprovada` com o tipo de hoje (ex. `declaracao`, que e ABONA em``; `:20`: `pelo consumidor da ausencia o dia em que a pessoa trabalha -- efeito de dinheiro errado. Precisa do tipo`; `:21`: `` `permuta` sem efeito no catalogo (migration + catalogo congelado), que e fatia propria. Pergunta: corte do tipo.`` |
| Interjornada/DSR sem juiz | `entrega.md:22`: ``3. **Interjornada (1) e DSR (6) sem juiz registrado** em `core/juizes.py`: a porta compoe guardas LOCAIS sobre juizes``; `:23`: ``registrados (`vinculo_do_dia`, `eh_dia_trabalho`, `marcos_turno`). Nao registrei juiz (PROIBIDO autoridade nova).``; `:24`: `Pergunta de lei: confirmar com o DP (o aval ja pede homologacao de (1), (3), (6)).` |
| Ordem global adiada | `entrega.md:29`: `5. **PERGUNTA DE DESENHO -- ORDEM GLOBAL DE TRAVA (Codex v3, procedente, NAO curavel nesta obra).** A casa NAO tem` |
| Motivo estrutural | `entrega.md:39`: `as barreiras liam vizinho e desligado sem trava (Codex v1, procedente). Opcoes, todas fora desta obra: (a) corte da`; `:40`: `ORDEM DA CASA (ex. C sempre primeiro) migrando editar_dia/regeneracao/cartorio -- obra ESTRUTURA propria;` |
| Selos/M24/M25 congelam escolha | `entrega.md:55`: ``**Selo congela a escolha:** `test_TRAVA_carta_celula_COLABORADOR_*` e os mutantes M24/M25 cravam X antes de C; se o``; `:56`: `corte for (a) C primeiro, esses selos INVERTEM (mesmo motivo de d1 = d2 nao ter selo). E o comentario` |
| Comentários/docstring | `entrega.md:57`: `` `ORDEM DA CASA` em `celula.py:744` e `:1003` e a docstring `:969` (linhas de bd590178e) dizem mais do que o vivo: e a ordem dos caminhos``; `:58`: `X -> C, nao uma ordem universal -- ajustar o texto no pouso (nao mexi para nao descertificar a suite).` |
| Dois significados | `entrega.md:65`: `8. **Carta = Ausencia tem DOIS significados (Codex v1 (4)/(5), procedente, nao curado: e o item 2).** A carta`; restante `:66-68`. |
| Status pendente | `entrega.md:70`: ``9. **"Nao rejeitada" != "aprovada"** (Codex v3 item 5): a porta exclui so carta `rejeitada`; carta pendente vale.`` |
| Triagem tipo/status e mesmo dia | `entrega.md:150`: `isolada aceitava rejeitada. Nao curados (TOPO): d1 = d2 (1), tipo/aprovacao da carta (2, 8), regeneracao sem trava` |
| Idempotência descartada | `entrega.md:153`: ``comparado por CONJUNTO (A/B -> B/A passava). Notas ao TOPO: 6 e 9. Descartado com motivo: idempotencia `igual` nao``; `:154`: ``compara DNA (o DNA e funcao de `trabalha` + vinculo pelo produtor unico P13; repetir com cadastro mudado e``; `:155`: `regeneracao, nao permuta) -- vai ao opus.` |
| IntegrityError reconhecido/adiado | `entrega.md:157`: `e o A<->C da mesma carta, e que os testes novos discriminam a v2 (3 de 4). PROCEDENTE e NAO curavel aqui: a casa nao`; `:158`: `tem ordem global X/C (TOPO 5), celula ausente (TOPO 5), corrida de criacao pode dar IntegrityError de unicidade em vez`; `:159`: `de deadlock (mesmo desenho, TOPO 7). Repetidos: barreiras na troca isolada (6), idempotencia (descarte acima),` | [nao verificado]
| RED módulo inexistente | `entrega.md:95`: `- Volta 1: arquivo de teste novo (RED = modulo inexistente na base fbc5c76ce).` |
| Placar contaminado por ERROR | `entrega.md:104`: ``MORDEM**, MEDIDA POR CAMADA: 21 contra os testes da volta 3 (`placar3.txt`, com o defeito de fixture do teste do``; `:105`: ``dono que dava ERROR em todo run); M19/M26 contra test.v4 (`placar4.txt`); M08/M18/M24/M25 contra o codigo e os testes`` | [nao verificado]
| Motivo não rerodar mutantes | `entrega.md:106`: ``FINAIS (`placar6.txt`). Os outros 20 (so no placar3) nao foram rerodados sobre v5 -- mutar `celula.py` depois do veredito descertificaria a arvore (o delta v3->v5 so muda o texto da regra 4 e`` |
| Sombra se declara atomic+raise | `entrega.md:113`: ``## DIFF NA SOMBRA (atomic+raise; `o10d_permuta_diff.py`, log `sombra_diff_final.log`; codigo da volta 1)`` |
| Dois colaboradores no FM futuro | `entrega.md:116`: `(uma por colab, {d1,d2}); FechamentoMensal movido: (128,12/2026), (136,12/2026), nenhum em competencia exportada.` |
| FM intacto alegado | `entrega.md:121`: `- (2c) competencia exportada 21/08-20/09: `recusada` com 4 frases LAVRADA; FM intacto True; frota `editada` +0.` |
| Drenagem manual mencionada | `entrega.md:128`: ``linhas: `on_commit registrados` 1 -> 3 e `drenados` 5 -> 7. Os +2 sao os sentinelas `_TravaDoColaborador` `` |
| Alegação dos efeitos | `entrega.md:131`: `` `permutada`, 4 celulas, frota +4, trilhas 1/2/4, rabo 0 dentro / 2 no commit, FM movidos so na competencia aberta,`` | [nao verificado]

Não achados específicos na entrega:

- Bug da aceitação de **range aberto** como carta e exclusão indevida da guarda: **não achado**; `:65-68` tratam do conflito geral dos significados.
- Pergunta sobre **dia cedido ter de ser plantão**: **não achado**; `:119` apenas relata a recusa `"nao tem plantao previsto 14/11"`.
- `M06/M09` morrerem por exceção: **não achado**.
- Literal `GREEN_v3 errors=1`: **não achado**; defeito geral de ERROR em todo run consta de `:104-105`.
- MD5 de somente dois colaboradores: **não achado**.
- Aviso explícito de que a drenagem aconteceu **dentro do atomic**: **não achado**; `:113` dá contexto atomic+raise e `:128` menciona drenagem, sem essa crítica.

A remissão de `entrega.md:159` para “TOPO 7” é textual, mas o TOPO 7 em `:61-64` trata de “ambos ativos”, não de `IntegrityError`.

## 17/11 05:37 · sessao 7d588557d67a · - · - · origem=Ronald (tmux)

Auditoria recomendada:

- **Item 1: (a).** A pergunta cobria ausência, dia exato e semântica de `data_fim` (`verif_v1.q:10–12`, `:24–27`). A resposta descreveu intervalo fechado/nulo aberto, mas não identificou o contraexemplo composto de aceitar afastamento como carta e excluí-lo da barreira (`verif_v1.r:60`, `:73`).
- **Item 2: (b).** Tipo/aprovação e `d1=d2` foram apontados e conscientemente adiados (`verif_v1.r`, seção 1; `entrega.md`, TOPO 1, 2, 8, 9).
- **Item 3:** comentários/docstring de ordem universal **(b)**: a abertura de `verif_v3.r` contesta expressamente a “ordem da casa”; TOPO 5 admite o exagero e adia sua correção. Para o defeito específico de **selos congelarem escolha sem aval**, não há crítica literal na resposta: **(a)** é mais rigoroso que atribuir reconhecimento explícito ao Codex. A resposta identifica o comportamento dos testes, mas não esse defeito (`verif_v3.r`, seção 4). Resultados particulares de M24/M25 ficam **(c)**. [nao verificado]
- **Item 4:** IntegrityError **(b)**; interjornada/DSR como decisões sem juiz **(b)**; exigência adicional de plantão no dia cedido **(e)**. Para esta última, trecho específico nas perguntas/respostas: **não achado**. O construtor conhecia o predicado, pois o implementou e justificou (`app/ponto/portas/celula.py:905–911`) e registrou sua recusa na sombra (`entrega.md`, seção DIFF NA SOMBRA, caso 2a). Portanto não cabe **(d)**.
- **Item 5: (b).** DNA/geradora ignorados foram vistos; houve descarte expresso por P13 (`verif_v1.r`, seção 6; `verif_v3.r`, seção 6; `entrega.md`, TRIAGEM CODEX v2).
- **Itens 6–7: (c).** Os defeitos concretos dependem de RED, execuções de mutantes e instrumento/medição da sombra fornecidos posteriormente (`opus_final.md:7–8`, contando título como linha 1).

Não encontrei fundamento para **(d)**: nos casos ausentes da crítica do Codex, existe reconhecimento do construtor ou o material está fora do alcance declarado. [nao verificado]

## 17/11 03:42 · sessao 170bd6b657ed · O10-D-PERMUTA-RECIPROCA · verificador · origem=Code

**Parecer: não aprovar como fechamento integral.** O delta corrige a inversão local contra edição/regeneração de células existentes, mas mantém ciclos com células ausentes e com caminhos que começam pelo Colaborador. A alegada “ordem da casa” não corresponde a todas as transações existentes. (`delta_v3.patch:+744`; `delta_v3.patch:+1000`; `app/colaboradores/services/desligamento.py:128`; `app/ponto/services/cadastro_futuro.py:31`.)

Referências `app/...:N` apontam à cópia original; `delta_v3.patch:+N`, à numeração de destino do anexo aplicado mentalmente. Execução de testes, reprodução concorrente, DDL efetivo e ocorrência em produção: **[nao verificado]**. [nao verificado]

**1. Tabela de ordem de trava**

Notação: **A** = Ausencia; **X** = CelulaDia; **C** = Colaborador; **D/H/P** = Disputa/Chamado/Pergunta. A autoridade ordena C e depois D/H/P por PK; ordenar uma chamada não ordena todas as aquisições da transação. (`app/chamados/models.py:2757`; `app/chamados/models.py:2771`; `app/chamados/models.py:2780`; `app/chamados/models.py:2793`; `app/chamados/models.py:2797`.)

| Caminho | Sequência efetiva |
|---|---|
| `permutar_plantoes`, com delta | **A por PK → X existentes por colaborador/data → C por PK → trocas internas → criação/edição de X → H**, quando o marcador encontra cobrança elegível. As trocas internas retomam travas já adquiridas; X ausente só nasce depois de C. (`delta_v3.patch:+1000`; `delta_v3.patch:+1013`; `app/ponto/portas/celula.py:1018`; `app/ponto/portas/celula.py:645`; `app/ponto/portas/celula.py:672`.) |
| `trocar_plantao` isolada, com delta | **A por PK → X existentes por colaborador/data → C por PK → criação/edição dos dois X → H**, condicional ao marcador. (`delta_v3.patch:+734`; `delta_v3.patch:+752`; `delta_v3.patch:+756`; `app/ponto/portas/celula.py:751`; `app/ponto/portas/celula.py:672`.) |
| `editar_dia` direto | **X existente por `FOR UPDATE`, ou INSERT de X ausente → C → H**, estes dois últimos somente se houver cobrança órfã elegível. O rabo fica depois do atomic próprio; atomic externo pode conservá-lo na mesma transação. (`app/ponto/portas/celula.py:637`; `app/ponto/portas/celula.py:645`; `app/ponto/portas/celula.py:672`; `app/ponto/portas/celula.py:676`; `app/chamados/models.py:487`.) |
| `regenerar_celulas_vinculo` isolada | Leitura inicial **sem trava → INSERT/UPDATE dos X em ordem de data → C → H**, se o marcador for elegível. O marcador recebe dias **reescritos**, não os meramente criados. O rabo fica depois do atomic próprio. (`app/ponto/portas/celula.py:226`; `app/ponto/portas/celula.py:227`; `app/ponto/portas/celula.py:294`; `app/ponto/portas/celula.py:311`; `app/ponto/portas/celula.py:328`; `app/ponto/portas/celula.py:393`.) |
| Command `gerar_celulas` | Um atomic envolve **regeneração de hoje por colaborador crescente → geração do período**. Pode conservar **X de A → C de A → X de B → C de B → mais X da geração**. Portanto não é simplesmente “todos os X antes de todos os C”. (`app/ponto/management/commands/gerar_celulas.py:319`; `app/ponto/management/commands/gerar_celulas.py:323`; `app/ponto/management/commands/gerar_celulas.py:345`; `app/ponto/management/commands/gerar_celulas.py:349`.) |
| `gerar_celulas_periodo` | Leitura sem trava → **INSERT de X ausente / UPDATE de DNA antigo nulo**. Atomic próprio e pedido explícito de C: **não achei**. Se chamado pelo command, herda as travas anteriores dele. (`app/ponto/portas/celula.py:1201`; `app/ponto/portas/celula.py:1218`; `app/ponto/portas/celula.py:1230`; `app/ponto/management/commands/gerar_celulas.py:345`.) |
| `decidir(Ausencia)` | Atomic → **UPDATE de A → post_save síncrono → cartório**. O cartório pode fazer **C/H → UPDATE de X** durante emissão; também pode escrever X antes de uma reconciliação que pede C. Não tem uma sequência única A→X→C. (`app/ponto/services/ausencia.py:1079`; `app/ponto/services/ausencia.py:1088`; `app/ponto/services/ausencia.py:988`; `app/ponto/signals.py:147`; `app/ponto/services/cartorio.py:1791`; `app/ponto/services/cartorio.py:1873`; `app/ponto/services/cartorio.py:1007`.) |
| `mudar_estado` direto | Não abre atomic próprio. **A → cartório → demais travas na mesma transação** exige atomic externo. O consumidor `on_commit` é outro momento, distinto do cartório síncrono. (`app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:988`; `app/ponto/signals.py:147`; `app/ponto/signals.py:272`.) |
| Desligamento | **C → UPDATE de vínculo → X futuros gerados por data**, conservados no atomic do desligamento. (`app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/desligamento.py:133`; `app/colaboradores/services/desligamento.py:148`; `app/colaboradores/services/vinculo.py:641`; `app/colaboradores/services/vinculo.py:662`; `app/ponto/services/cadastro_futuro.py:31`.) |
| Lançamento INSS | **C → criação/reabertura de A → post_save/cartório → X**, dentro do atomic da view. (`app/ponto/views.py:2544`; `app/ponto/views.py:2546`; `app/ponto/views.py:2547`; `app/ponto/views.py:2564`.) |
| Escrita/transição de Chamado | **C → D/H/P conforme a operação → cartório síncrono → X**, quando a mudança de estado exige julgamento. (`app/chamados/models.py:290`; `app/chamados/models.py:487`; `app/chamados/signals.py:189`; `app/chamados/signals.py:218`; `app/ponto/portas/celula.py:1267`.) |
| `regenerar_futuras_do_cadastro` | **X futuros existentes por data → regeneração de cada dia → C/H**, se o marcador alcançar cobrança elegível. (`app/ponto/services/cadastro_futuro.py:30`; `app/ponto/services/cadastro_futuro.py:36`; `app/ponto/services/cadastro_futuro.py:44`; `app/ponto/portas/celula.py:328`.) |
| Foto futura de folgas | **X futuros → FolgaDia → regeneração futura**, conservando as travas de X. (`app/escala/folgas.py:620`; `app/escala/folgas.py:621`; `app/escala/folgas.py:639`; `app/escala/folgas.py:640`.) |
| Restauração de snapshot | **EscalaColaborador por PK → X que referenciam vínculos removidos → DELETE/UPDATE dos vínculos → regeneração futura**. O chamador ainda pode escrever nota/transição de Chamado no atomic externo. (`app/colaboradores/services/vinculo.py:1017`; `app/colaboradores/services/vinculo.py:1020`; `app/colaboradores/services/vinculo.py:1025`; `app/colaboradores/services/vinculo.py:1035`; `app/chamados/services/acoes_chamado.py:142`; `app/chamados/services/acoes_chamado.py:153`.) |
| Salvamento de template | Atomic envolvendo várias regenerações: pode acumular **X/C de A antes de pedir X de B**, como o command. (`app/escala/services/cadastro_tipo.py:140`; `app/escala/services/cadastro_tipo.py:323`; `app/escala/services/cadastro_tipo.py:339`; `app/escala/services/cadastro_tipo.py:353`.) |
| Cancelamento de férias | **A por PK → saves → cartório síncrono**, com os sufixos condicionais já descritos. (`app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1488`; `app/ponto/services/ausencia.py:1498`.) |
| Reversão de regeneração / lavra / carimbo | UPDATE de X; reversão tem rabo depois do atomic próprio. Lavra e carimbo herdam as travas do chamador; `salvar_e_emitir` não impõe ordem anterior de C. (`app/ponto/portas/celula.py:1130`; `app/ponto/portas/celula.py:1140`; `app/ponto/portas/celula.py:1162`; `app/ponto/portas/celula.py:1266`.) |

**Pares envolvendo permuta/troca**, consideradas suas invocações isoladas, sem travas prévias do chamador:

| Par | Resultado com o delta |
|---|---|
| Permuta×permuta; permuta×troca; troca×troca | Com conjuntos estáveis e apenas essas portas, ciclo: **não achei**. Os prefixos são ordenados e a criação ocorre depois da trava comum de C. Isso não demonstra segurança perante os outros caminhos. (`delta_v3.patch:+752`; `delta_v3.patch:+756`; `delta_v3.patch:+1000`; `delta_v3.patch:+1013`.) |
| Permuta/troca×edição direta, X existente | A inversão simples da v2 fecha: ambas chegam a X antes de C. Ciclo nesse prefixo isolado: **não achei**. X ausente muda o resultado, abaixo. (`app/ponto/portas/celula.py:638`; `app/ponto/portas/celula.py:672`; `delta_v3.patch:+752`; `delta_v3.patch:+1013`.) |
| Permuta/troca×regeneração isolada, todos os X existentes | No prefixo de um colaborador, as células são adquiridas antes de C e por data; ciclo local: **não achei**. Atomic externo, criação e acumulação de colaboradores precisam da análise separada abaixo. (`app/ponto/portas/celula.py:228`; `app/ponto/portas/celula.py:311`; `app/ponto/portas/celula.py:328`.) |
| Permuta/troca×command ou salvamento de template | **Ciclo possível:** G reescreve A/hoje e segura C_A; P segura A/futuro e B/hoje e espera C_A; G avança e espera B/hoje. O bloqueio de P ocorre antes das barreiras. (`app/ponto/management/commands/gerar_celulas.py:321`; `app/ponto/management/commands/gerar_celulas.py:345`; `app/ponto/portas/celula.py:328`; `delta_v3.patch:+1010`; `delta_v3.patch:+1013`.) |
| Permuta/troca×decisão da **mesma carta** | O ciclo simples A↔C da v2 fica serializado pela carta antes de X/C. Outros recursos anteriores do chamador: **[nao verificado]**. (`delta_v3.patch:+735`; `delta_v3.patch:+1008`; `app/ponto/services/ausencia.py:988`.) |
| Permuta/troca×decisão de **outra ausência** do mesmo colaborador | **Ciclo possível:** P segura X; decisão de outra A emite cobrança e segura C; P espera C; decisão espera X para carimbar. (`delta_v3.patch:+1013`; `app/ponto/services/cartorio.py:1791`; `app/chamados/models.py:952`; `app/ponto/services/cartorio.py:1873`.) |
| Permuta/troca×desligamento | **Ciclo possível:** P segura X futuro gerado; desligamento segura C; desligamento espera X; P espera C. Não exige célula ausente nem marcador elegível na permuta. (`delta_v3.patch:+1010`; `delta_v3.patch:+1013`; `app/colaboradores/services/desligamento.py:133`; `app/colaboradores/services/desligamento.py:148`; `app/ponto/services/cadastro_futuro.py:31`.) |
| Permuta/troca×INSS ou transição de Chamado com cartório | **Ciclo possível pelo mesmo X↔C:** o outro caminho pode possuir C antes de pedir X. Na reabertura de A também pode haver A↔C se for uma carta já retida pela troca. (`app/ponto/views.py:2546`; `app/ponto/views.py:2547`; `app/chamados/models.py:290`; `app/chamados/signals.py:218`; `delta_v3.patch:+756`.) |
| Permuta/troca×escrita somente de C | Sem pedido posterior de A/X, ciclo: **não achei**; há espera unilateral. (`app/colaboradores/services/colaborador.py:131`; `app/colaboradores/services/colaborador.py:136`; `delta_v3.patch:+1013`.) |
| Permuta/troca×lavra/carimbo somente de X | Sem pedido posterior de C ou outro recurso retido pela permuta, ciclo: **não achei**. Se o chamador já detém C, é o caso anterior de X↔C. (`app/ponto/portas/celula.py:1266`; `app/chamados/signals.py:218`.) |

Para os demais pares, o mesmo cruzamento está presente em **edição ou regeneração×desligamento/Chamado/decisão emissora**: E/R segura X e pede C; o outro segura C e pede X. Em **edição/regeneração×command/template**, o command pode reter C de uma etapa e pedir X que E/R retém. São ciclos condicionados aos marcadores/emissões indicados, não observações de produção. (`app/ponto/portas/celula.py:672`; `app/ponto/portas/celula.py:328`; `app/ponto/management/commands/gerar_celulas.py:321`; `app/chamados/models.py:952`; `app/ponto/services/cadastro_futuro.py:31`.)

Entre duas edições diretas de uma célula cada, duas regenerações isoladas do mesmo colaborador em ordem de data, ou duas operações que adquirem somente C: ciclo concreto **não achei**. Para as demais combinações dos caminhos compostos do inventário, **não achei demonstração completa de ausência de ciclos**; os atomics externos impedem concluir segurança apenas pela ordem de uma função interna. (`app/ponto/portas/celula.py:638`; `app/ponto/portas/celula.py:228`; `app/ponto/portas/celula.py:393`; `app/chamados/models.py:2771`.)

**2. Célula ausente: o delta não fecha o problema global**

`SELECT FOR UPDATE` das existentes não reserva a chave da célula ausente; os três criadores usam `create`, e o modelo declara unicidade por colaborador/data. A trava de C só serializa os escritores que a adquirem **antes** de criar. (`app/ponto/portas/celula.py:638`; `app/ponto/portas/celula.py:645`; `app/ponto/portas/celula.py:294`; `app/ponto/portas/celula.py:1218`; `app/escala/models.py:1833`; `delta_v3.patch:+1013`.)

| Par e interleaving concreto | Comparação com o hash |
|---|---|
| **P/T×edição:** P consulta X ausente; E insere X; P trava C; E encontra cobrança órfã e espera C; P tenta inserir X e espera a transação de E pela unicidade. | **Piora esse interleaving.** No hash, sem aquisição anterior de C pelo marcador de outra célula, P esperaria X enquanto E obteria C e terminaria. O delta passa a exigir C antes dessa criação. (`app/ponto/portas/celula.py:645`; `app/ponto/portas/celula.py:672`; `app/chamados/models.py:487`; `delta_v3.patch:+1013`; `app/escala/models.py:1833`.) |
| **P/T×regeneração:** R insere X e reescreve outro dia Y; P não vê X, trava C e tenta criar X; R chega ao marcador de Y e espera C. | **Piora esse interleaving**, quando o hash ainda não tinha adquirido C por edição anterior. A criação de X, sozinha, não entra em `_reescritos`; Y é necessário para esse exemplo pelo marcador. (`app/ponto/portas/celula.py:294`; `app/ponto/portas/celula.py:317`; `app/ponto/portas/celula.py:328`; `delta_v3.patch:+1013`.) |
| **P/T×geração/regeneração em atomic com vários X:** G insere X; P pretrava outro X existente Y e depois pede/cria X; G tenta atualizar Y. | **Empata quanto à existência desse ciclo:** a base já pretravava Y antes da criação de X. Acrescentar C não elimina X↔Y. No gerador de período, a atualização possível é o enriquecimento de DNA antigo nulo. (`app/ponto/portas/celula.py:984`; `app/ponto/portas/celula.py:1218`; `app/ponto/portas/celula.py:1230`; `app/ponto/portas/celula.py:311`.) |
| **Permuta×permuta / permuta×troca**, criando células em etapas diferentes | **Melhora:** a trava comum de C agora antecede todas as criações dessas portas. Na base, a permuta compunha dois pares sucessivos e podia criar o segundo par depois de outra operação ter criado parte dele. (`app/ponto/portas/celula.py:1018`; `app/ponto/portas/celula.py:751`; `delta_v3.patch:+756`; `delta_v3.patch:+1013`.) |
| **Troca isolada×troca isolada** | No caminho simples, ciclo original por criação em ordem oposta: **não achei**; a base já ordenava os dois lados por colaborador/data. Portanto não atribuo ao delta uma correção demonstrada desse par. (`app/ponto/portas/celula.py:751`.) |

Há ainda uma hipótese independente do marcador: G insere X; P trava C e espera o INSERT de G pela unicidade; G precisa validar a FK de X para C e espera P. O modelo declara FK e unicidade, mas **DDL efetivo e momento dessa validação: [nao verificado]**. Não afirmo que o INSERT sempre peça essa trava imediatamente. Sob essa condição, o delta piora o par simples contra qualquer criador; a v2 já tinha o mesmo risco. (`app/escala/models.py:1681`; `app/escala/models.py:1833`; `delta_v3.patch:+756`; `delta_v3.patch:+1013`.)

O hash também podia chegar a C por um marcador anterior; portanto “piora” acima significa **ampliar os interleavings que ciclam**, não afirmar que a base era livre de deadlocks. (`app/ponto/portas/celula.py:672`; `app/ponto/portas/celula.py:1018`.)

**3. Situação dos itens 1, 2, 5 e 7 da volta 2**

| Item | Veredito |
|---|---|
| **1 — validações e criação concorrente** | Mantém as correções de releitura do desligado, quatro documentos e rejeição. Conserva serialização entre permutas/trocas. **Não fecha** a criação ausente contra edição/geração/regeneração. A troca isolada continua sem barreiras de descanso. (`delta_v3.patch:+1013`; `delta_v3.patch:+1027`; `delta_v3.patch:+757`; `app/ponto/portas/celula.py:753`; `app/ponto/portas/celula.py:902`.) |
| **2 — ordem invertida** | **Fecha o contraexemplo local com X existente** de edição/regeneração e o ciclo simples com decisão da mesma carta. **Continua globalmente:** outra ausência, desligamento, INSS, Chamado e transações de várias etapas têm C antes de algum X posterior. Contra caminhos C→X, a v3 perde a serialização que C primeiro proporcionava na v2. (`delta_v3.patch:+752`; `delta_v3.patch:+1013`; `app/ponto/services/cartorio.py:1791`; `app/colaboradores/services/desligamento.py:148`; `app/ponto/management/commands/gerar_celulas.py:345`.) |
| **5 — testes** | Melhora a discriminação da ordem v3 e acrescenta o caso A/B→B/A. **Continua sem demonstrar concorrência ou célula ausente.** A fragilidade de contar callbacks brutos também permanece: a trava registra sentinelas `on_commit`. (`delta_v3.patch:+333`; `delta_v3.patch:+380`; `app/chamados/services/trava_do_colaborador.py:47`; `app/ponto/tests/test_o10d_permuta_reciproca.py:305`.) |
| **7 — outros defeitos** | **Fecha a comparação de donos por conjunto.** Permanecem tipo/aprovação sem exigência, cartas como fatos de ausência, leitura antiga da regeneração, recusa de mesmo dia e idempotência insuficiente. (`delta_v3.patch:+742`; `app/ponto/portas/celula.py:227`; `app/ponto/portas/celula.py:897`; `app/ponto/portas/celula.py:975`; `app/ponto/portas/celula.py:1003`.) |

**4. Os testes distinguem a volta 2?** [nao verificado]

| Teste novo/reescrito | Contra o código da volta 2 | [nao verificado]
|---|---|
| Ordem carta→célula→Colaborador na permuta | **Falharia:** a v2 começava por Colaborador. (`delta_v3.patch:+352`; `delta_v3.patch:+359`.) |
| Mesma ordem na troca isolada | **Falharia**, pelo mesmo motivo. (`delta_v3.patch:+352`; `delta_v3.patch:+371`.) |
| Donos A/B→B/A | **Falharia:** conjuntos iguais não detectam papéis invertidos. (`delta_v3.patch:+380`; `delta_v3.patch:+742`.) |
| Rejeitada na troca isolada | **Passaria:** a rejeição já era verificada na v2. É regressão adicional, não discriminador da v3. (`delta_v3.patch:+757`; `delta_v3.patch:+396`.) |
| Desligado no banco, rejeitada na permuta, quatro documentos antes da primeira troca | **Passariam:** são correções acumuladas da v2. (`delta_v3.patch:+219`; `delta_v3.patch:+256`; `delta_v3.patch:+266`.) |
| Rollback com segunda recusa forjada | **Passaria:** o atomic e a conversão da segunda recusa já existiam. O teste executa a primeira troca real e exige trilha antes de forjar a segunda recusa. (`delta_v3.patch:+290`; `app/ponto/portas/celula.py:972`; `app/ponto/portas/celula.py:1019`.) |

**Não passam vazios por falta de células na fixture:** a base gera as células de setembro até dezembro, e os testes de ordem usam novembro. Contudo, o helper reconhece a presença do SQL `FOR UPDATE`, não a quantidade de linhas retornadas; uma consulta vazia também acrescentaria “celula” à ordem. Teste concorrente com segunda conexão/célula ausente: **não achei no delta**. (`app/ponto/tests/test_o10b_editar_dia_troca.py:91`; `app/ponto/tests/test_o10b_editar_dia_troca.py:93`; `app/ponto/tests/test_o10d_permuta_reciproca.py:48`; `delta_v3.patch:+333`; `delta_v3.patch:+352`.)

**5. Quem escreve, quem lê e qual significado**

| Campo/valor | Escritores encontrados | Leitores e divergência |
|---|---|---|
| `Ausencia.colaborador` | A criação atribui o dono e salva. Escritor de produção que **transfira** uma ausência existente: **não achei**; o editor normal não altera esse campo. (`app/ponto/services/ausencia.py:484`; `app/ponto/services/ausencia.py:562`; `app/ponto/services/ausencia.py:648`.) | Troca lê dono como papel cedente/substituto; cobertura e sobreposição leem como pessoa ausente. O par ordenado corrige a identidade dos papéis recebidos. (`delta_v3.patch:+734`; `delta_v3.patch:+742`; `app/ponto/turnos.py:2543`; `app/ponto/turnos.py:2839`.) |
| `Ausencia.status` | Estado inicial na criação; `mudar_estado`; decisão; cancelamento de férias grava rejeitada. (`app/ponto/services/ausencia.py:475`; `app/ponto/services/ausencia.py:486`; `app/ponto/services/ausencia.py:976`; `app/ponto/services/ausencia.py:1496`.) | Delta exclui só rejeitada; folha exige aprovada; cobrança/sobreposição consideram não rejeitadas. **Não rejeitada não significa aprovada.** (`delta_v3.patch:+757`; `delta_v3.patch:+

[truncado em 20000 de 24725 caracteres]

## 17/11 03:37 · sessao 466bb7bb03cb · - · - · origem=Ronald (tmux)

Achado adicional decisivo: o delta v3 mantém inversão contra **desligamento**, mesmo com células existentes. `executar_desligamento` segura Colaborador pelo `salvar_inteiro`, fecha vínculos e trava células futuras na mesma transação. A permuta/troca agora segura essas células antes de pedir Colaborador. Interleaving: P segura célula futura; D segura Colaborador; D espera célula; P espera Colaborador. (`app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/desligamento.py:133`, `app/colaboradores/services/desligamento.py:148`; `app/colaboradores/services/colaborador.py:90`, `app/colaboradores/services/colaborador.py:158`; `app/colaboradores/services/vinculo.py:640`, `app/colaboradores/services/vinculo.py:662`; `app/ponto/services/cadastro_futuro.py:30`, `app/ponto/services/cadastro_futuro.py:31`; `delta_v3.patch:+997`, `delta_v3.patch:+1009`.)

O command também compõe outra ordem: regenera A/hoje, podendo reter Colaborador A pelo marcador; depois regenera B/hoje. P pode segurar A/futuro e B/hoje, esperando Colaborador A, enquanto G espera B/hoje. Bloqueia antes das barreiras da permuta. O mesmo padrão existe no salvamento de template em atomic envolvendo vários colaboradores. (`app/ponto/management/commands/gerar_celulas.py:319`, `app/ponto/management/commands/gerar_celulas.py:323`, `app/ponto/management/commands/gerar_celulas.py:345`; `app/ponto/portas/celula.py:311`, `app/ponto/portas/celula.py:328`; `app/escala/services/cadastro_tipo.py:140`, `app/escala/services/cadastro_tipo.py:323`, `app/escala/services/cadastro_tipo.py:339`, `app/escala/services/cadastro_tipo.py:353`.)

Inventário adicional de caminhos:

| Caminho | Sequência observada |
|---|---|
| `regenerar_futuras_do_cadastro` | Células futuras geradas por data → regeneração de cada dia → Colaborador/Chamado se marcador elegível. (`app/ponto/services/cadastro_futuro.py:30`, `app/ponto/services/cadastro_futuro.py:36`, `app/ponto/services/cadastro_futuro.py:44`; `app/ponto/portas/celula.py:328`.) |
| `sincronizar_foto_futura_do_calendario` | Células futuras → FolgaDia → serviço anterior. (`app/escala/folgas.py:620`, `app/escala/folgas.py:621`, `app/escala/folgas.py:639`, `app/escala/folgas.py:640`.) |
| `reverter_para_snapshot` | EscalaColaborador por PK → células referenciando vínculos removidos → DELETE/UPDATE vínculos → futuras. No chamador, nota/transição do Chamado ainda dentro do atomic externo. (`app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1020`, `app/colaboradores/services/vinculo.py:1025`, `app/colaboradores/services/vinculo.py:1035`; `app/chamados/services/acoes_chamado.py:142`, `app/chamados/services/acoes_chamado.py:149`, `app/chamados/services/acoes_chamado.py:153`, `app/chamados/services/acoes_chamado.py:155`.) |
| `reverter_regeneracao_dia` | UPDATE célula sem releitura sob trava → evento; rabo após atomic próprio, ainda sob transação externa quando houver. (`app/ponto/portas/celula.py:1121`, `app/ponto/portas/celula.py:1130`, `app/ponto/portas/celula.py:1140`, `app/ponto/portas/celula.py:1162`.) |
| `salvar_e_emitir` | UPDATE célula → INSERT EventoCelula no atomic. (`app/ponto/portas/celula.py:1266`, `app/ponto/portas/celula.py:1267`, `app/ponto/portas/celula.py:1269`.) |
| Lançamento INSS | Colaborador → criação/reabertura Ausencia → post_save/cartório → atualização situação, mesmo atomic. (`app/ponto/views.py:2544`, `app/ponto/views.py:2546`, `app/ponto/views.py:2547`, `app/ponto/views.py:2564`.) |

Para célula ausente há também ciclo **condicionado à FK efetiva**: G insere X; P não vê INSERT não confirmado, trava Colaborador e tenta inserir X, esperando a UNIQUE; G valida FK no INSERT/commit e pede chave compartilhada no Colaborador, esperando P. Modelo declara FK e UNIQUE, backend declarado é PostgreSQL; DDL vivo e momento efetivo da validação **[nao verificado]**. Não afirmar que todo INSERT trava Colaborador imediatamente. (`app/escala/models.py:1681`, `app/escala/models.py:1833`; `app/ponto/portas/celula.py:645`, `app/ponto/portas/celula.py:294`, `app/ponto/portas/celula.py:1218`; `app/config/settings/prod.py:11`.)

Entre apenas duas permutas/trocas, com conjuntos de donos/cartas/células estáveis, **não achei** ciclo novo: prefixos ordenados e trava comum antecedem criação; isso não permite concluir ordem global segura diante dos caminhos acima. (`delta_v3.patch:+749`, `delta_v3.patch:+1003`; `app/chamados/models.py:2771`.)

## 17/11 03:36 · sessao 90e72400e96b · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura; concorrência executada/produção: **[nao verificado]**.

| Caminho | Ordem efetiva e limites |
|---|---|
| `decidir` | `atomic` → UPDATE Ausencia → `post_save` síncrono → cartório. (`app/ponto/services/ausencia.py:1079`, `app/ponto/services/ausencia.py:1088`, `app/ponto/services/ausencia.py:988`; `app/ponto/signals.py:147`, `app/ponto/signals.py:93`.) |
| `mudar_estado` direto | Não abre atomic próprio: sobreposição da trava de Ausencia com as seguintes exige atomic externo. (`app/ponto/services/ausencia.py:955`, `app/ponto/services/ausencia.py:988`, `app/ponto/services/ausencia.py:1002`.) |
| Cartório, ramo emissor | Pode adquirir Colaborador/Chamado antes de escrever CelulaDia: emissão em `cartorio.py:1791` → `detectar_ausencias.py:694` → `models.py:949`, `models.py:952`; carimbo posterior em `cartorio.py:1873` → `celula.py:1505`, `celula.py:1267`. |
| Cartório, outros ramos | Sem adesão, escreve célula e retorna, sem emissão. Já a primeira passada pode escrever célula e a reconciliação seguinte pedir Colaborador. Portanto não há ordem universal “Ausencia→célula→Colaborador”. (`app/ponto/services/cartorio.py:1292`, `app/ponto/services/cartorio.py:1314`, `app/ponto/services/cartorio.py:1318`, `app/ponto/services/cartorio.py:1007`, `app/ponto/services/cartorio.py:1011`.) |
| Consumidor de Ausencia | É `on_commit`; suas travas não pertencem ao atomic original. Não confundir com o cartório síncrono. (`app/ponto/signals.py:250`, `app/ponto/signals.py:272`.) |
| INSS pela view | Colaborador → criação/reabertura de Ausencia → cartório/célula → situação `afastado`, no mesmo atomic. (`app/ponto/views.py:2544`, `app/ponto/views.py:2546`, `app/ponto/views.py:2547`, `app/ponto/views.py:2564`; `app/colaboradores/services/colaborador.py:90`, `app/colaboradores/services/colaborador.py:81`.) |
| Desligamento | Colaborador → UPDATE vínculo → células futuras geradas por data, no mesmo atomic. (`app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/desligamento.py:133`, `app/colaboradores/services/desligamento.py:148`; `app/colaboradores/services/vinculo.py:862`, `app/colaboradores/services/vinculo.py:641`, `app/colaboradores/services/vinculo.py:662`; `app/ponto/services/cadastro_futuro.py:30`, `app/ponto/services/cadastro_futuro.py:31`, `app/ponto/services/cadastro_futuro.py:36`.) |
| Cadastro futuro | Trava células existentes por data e chama regeneração mantendo as travas. (`app/ponto/services/cadastro_futuro.py:30`, `app/ponto/services/cadastro_futuro.py:36`, `app/ponto/services/cadastro_futuro.py:44`.) |
| Cancelamento de férias | Ausencias por PK → saves síncronos → cartório, dentro do atomic; pode ter os mesmos sufixos anteriores. (`app/ponto/services/ausencia.py:1484`, `app/ponto/services/ausencia.py:1488`, `app/ponto/services/ausencia.py:1498`.) |
| Backfill origem férias | AgendamentoFerias (+ período pelo `select_related`) → Ausencias por PK → UPDATE origem; Colaborador explícito posterior não achei. (`app/ponto/services/ausencia.py:1671`, `app/ponto/services/ausencia.py:1673`, `app/ponto/services/ausencia.py:1679`, `app/ponto/services/ausencia.py:1696`.) |

**Ciclo preservado pela volta 3:** permuta/troca segura célula X, antes da nova trava de Colaborador; uma decisão de **outra Ausencia** do mesmo colaborador consegue segurar Colaborador durante emissão, depois pede X para carimbar; a permuta pede Colaborador: ciclo X↔Colaborador. Se for exatamente a mesma carta, o pedido inicial de carta da permuta a faz esperar antes de possuir X; esse interleaving simples fica serializado. (`delta_v3.patch:+754`, `delta_v3.patch:+1013`; `app/ponto/services/cartorio.py:1791`, `app/ponto/services/cartorio.py:1873`; `app/chamados/models.py:952`.)

**Outro ciclo concreto:** P segura célula futura gerada X e pede Colaborador; desligamento segura Colaborador e pede X pelo suplemento futuro. A ordem célula→Colaborador já era alcançável na base pelo marcador de órfãs; o delta antecipa o ponto dessa espera, sem eliminar o ciclo. (`delta_v3.patch:+1013`; `app/ponto/portas/celula.py:672`; `app/colaboradores/services/desligamento.py:133`, `app/colaboradores/services/desligamento.py:148`; `app/ponto/services/cadastro_futuro.py:31`.)

| Campo | Escritores e leitores |
|---|---|
| `Ausencia.colaborador` | Criação escreve em `ausencia.py:484`, persiste em `ausencia.py:562`; edição normal altera tipo/data/minutos, sem dono em `ausencia.py:648`, `ausencia.py:659`. **Escritor de produção que transfere dono: não achei.** Leitores de cobertura e sobreposição usam esse FK como pessoa ausente: `turnos.py:2543`, `turnos.py:2839`. |
| `Ausencia.documento` | Criação: `ausencia.py:492`; anexar: `ausencia.py:1017`, `ausencia.py:1019`; reabrir: `ausencia.py:1307`, `ausencia.py:1309`. Modelo define comprovante, preenchido só no primeiro dia do intervalo: `models.py:777`, `models.py:780`. Leitores: estado inicial `ausencia.py:938`; gate de aprovação `ausencia.py:754`, `ausencia.py:774`; aviso `ausencia.py:796`, `ausencia.py:813`. |
| `Ausencia.status` | Criação deriva estado: `ausencia.py:475`, `ausencia.py:486`; transição: `ausencia.py:976`, `ausencia.py:988`; cancelamento férias: `ausencia.py:1496`, `ausencia.py:1498`. Folha exige aprovada, cobrança aceita todas não rejeitadas: `turnos.py:2528`, `turnos.py:2531`; sobreposição exclui rejeitadas: `turnos.py:2839`. |
| `Colaborador.situacao` | Portas guardadas: `colaborador.py:126`, `colaborador.py:140`, `colaborador.py:152`, `colaborador.py:164`; desligar: `desligamento.py:129`, `desligamento.py:133`; reativar: `desligamento.py:242`, `desligamento.py:247`; INSS: `ponto/views.py:2564`. `esta_desligado` significa somente estado `desligado`; agendamento futuro e afastamento não satisfazem: `desligamento.py:23`, `desligamento.py:29`, `desligamento.py:32`. Choices incluem ativo, afastado, férias, desligado: `colaboradores/models.py:295`. |
| `CelulaDia` no cartório | Cartório lê célula existente: `cartorio.py:2027`; futura/inexistente retorna: `cartorio.py:2025`, `cartorio.py:2028`; escreve veredito/ata/carimbos pela porta: `cartorio.py:1853`, `cartorio.py:1873`, `celula.py:1505`, `celula.py:1267`. |

**Divergência semântica que continua:** não rejeitada não significa aprovada; documento presente normalmente coloca a linha em `aguardando_decisao`. As cartas aceitas na permuta continuam sendo fatos de ausência para cobrança e, quando aprovadas, para folha. Não desligado também não significa ativo. (`app/ponto/services/ausencia.py:938`, `app/ponto/services/ausencia.py:912`; `app/ponto/turnos.py:2528`, `app/ponto/turnos.py:2531`; `app/colaboradores/services/desligamento.py:32`; `app/colaboradores/models.py:295`.)

## 17/11 03:35 · sessao 4aabe714545d · - · - · origem=Ronald (tmux)

Auditoria dos testes por leitura; execução e concorrência real: **[nao verificado]**. [nao verificado]

| Teste do delta acumulado | Contra a volta 2 | [nao verificado]
|---|---|
| Ordem na permuta e na troca isolada | **Falhariam:** exigem `carta → celula → colaborador`; a v2 começava por Colaborador. (`delta_v3.patch`, hunk `+331`, métodos `test_TRAVA_carta_celula_COLABORADOR_*`; descrição da v2 no anexo `verif_v2.r`, item 2.) |
| Donos A/B → B/A | **Falharia:** a comparação por conjunto aceita a inversão; a troca continua e edita os papéis relidos. (`delta_v3.patch`, hunk `+331`, método `test_TROCA_carta_que_TROCOU_de_dono_A_B_para_B_A_e_recusada`; `app/ponto/portas/celula.py:736`, `app/ponto/portas/celula.py:751`, `app/ponto/portas/celula.py:753`.) |
| Rejeitada na troca isolada | **Passaria:** a v2 já verificava rejeição antes de chamar `editar_dia`. (`verif_v2.r`, item 1, linha “Carta rejeitada”; `delta_v3.patch`, hunk `+331`, método `test_TROCA_isolada_recusa_carta_REJEITADA`.) |
| Instância velha/desligado, rejeitada na permuta e quatro documentos antes da primeira troca | **Passariam:** são correções acumuladas da v2, não discriminadores da v3. (`verif_v2.r`, itens 1 e 5; `delta_v3.patch`, hunks `+216` e `+253`.) |
| Rollback com segunda recusa forjada | **Passaria:** preserva o mesmo atomic e conversão de recusa da base; executa a primeira troca real e exige trilha antes da segunda chamada. (`app/ponto/portas/celula.py:972`, `app/ponto/portas/celula.py:1018`, `app/ponto/portas/celula.py:1020`; `delta_v3.patch`, hunk `+290`.) |

**Os testes de ordem não são vazios por falta de células na fixture.** A base gera CED/SUB/CTL/C61 de 01/09 até 31/12; os testes novos usam 03/11 e 04/11. A composição da fixture chama essa base, e `_cel` usa `.get`, portanto a ausência de uma célula também derrubaria as fotografias dos testes que as usam. (`app/ponto/tests/test_o10b_editar_dia_troca.py:91`, `app/ponto/tests/test_o10b_editar_dia_troca.py:93`, `app/ponto/tests/test_o10b_editar_dia_troca.py:103`; `app/ponto/tests/test_o10d_permuta_reciproca.py:48`, `app/ponto/tests/test_o10d_permuta_reciproca.py:89`.)

**O helper continua insuficiente para provar proteção de célula ausente.** Ele reconhece a instrução `SELECT … FOR UPDATE` pelo SQL e coleta nomes distintos; não verifica quantas linhas foram travadas. Um SELECT que devolva zero linhas também satisfaz essa coleta. Não há segunda conexão ou interleaving concorrente nesses testes. (`delta_v3.patch`, hunk `+331`, métodos `_ordem_das_travas` e `_confere_ordem_da_casa`; concorrência executada: **[nao verificado]**.) [nao verificado]

Outros defeitos que permanecem:

- A idempotência da permuta compara apenas `origem`, `trabalha` e existência de trilha com as mesmas cartas; não compara DNA, vínculo gerador ou autoria da última edição, e retorna antes das barreiras. (`app/ponto/portas/celula.py:1003`, `app/ponto/portas/celula.py:1005`, `app/ponto/portas/celula.py:1006`, `app/ponto/portas/celula.py:1010`.)
- A troca isolada continua sem chamar as barreiras de interjornada/DSR da permuta. (`app/ponto/portas/celula.py:751`, `app/ponto/portas/celula.py:753`; barreiras em `app/ponto/portas/celula.py:902`, `app/ponto/portas/celula.py:927`.)
- Tipo específico e aprovação das cartas continuam sem exigência; a fixture positiva usa `declaracao/aguardando_decisao`. (`app/ponto/tests/test_o10b_editar_dia_troca.py:106`, `app/ponto/tests/test_o10b_editar_dia_troca.py:108`; o próprio contrato do teste declara a lacuna em `app/ponto/tests/test_o10d_permuta_reciproca.py:12`.)
- A carta permanece também um fato de ausência: a guarda exclui seus IDs, mas a grade consulta cobertura aprovada e pode apagar os marcos de ausência integral. (`app/ponto/portas/celula.py:897`; `app/escala/utils.py:1188`, `app/escala/utils.py:1220`, `app/escala/utils.py:1251`.)
- `d1 == d2` continua recusado apesar de o contrato documentado dizer permitido. (`app/ponto/portas/celula.py:975`; `app/ponto/tests/test_o10d_permuta_reciproca.py:259`.)

## 17/11 03:26 · sessao 919afaf4b8c8 · O10-D-PERMUTA-RECIPROCA · verificador · origem=Code

**Parecer: não aprovar como fechamento integral.** O delta corrige validações locais, mas introduz conflito de ordem com caminhos existentes: a permuta passa a fazer **Colaborador → célula**, enquanto edição e regeneração podem fazer **célula → Colaborador**. (`delta_v2.patch:+998`; `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:672`, `app/chamados/models.py:487`.)

Referências ao código são da cópia original; `delta_v2.patch:+N` indica a numeração de destino do anexo. Execução de testes, reprodução concorrente e ocorrência em produção: **[nao verificado]**. [nao verificado]

**1. O que fecha da volta 1**

| Achado | Parecer | Fundamento e limite |
|---|---|---|
| Deadlock na criação de célula ausente | **Fecha em parte.** | Entre permutas/trocas que compartilham colaboradores, ambas passam a adquirir os mesmos Colaborador por PK antes das cartas/células; isso elimina o interleaving original de criação cruzada. Entretanto, permanece possibilidade de deadlock contra edição/regeneração, detalhada abaixo. (`delta_v2.patch:+738`, `delta_v2.patch:+998`; `app/chamados/models.py:2771`; `app/ponto/portas/celula.py:672`, `app/ponto/portas/celula.py:328`.) |
| TOCTOU de interjornada/DSR entre permutas/trocas | **Fecha a janela concorrente entre essas portas; fecha em parte a garantia de descanso.** | A trava comum serializa essas operações, mas `trocar_plantao` continua escrevendo sem chamar `_barreiras_da_permuta`. Assim, uma troca executada depois da permuta pode produzir descanso inválido. (`delta_v2.patch:+738`, `delta_v2.patch:+998`; `app/ponto/portas/celula.py:751`, `app/ponto/portas/celula.py:753`, `app/ponto/portas/celula.py:1010`.) |
| `esta_desligado` sobre instância velha | **Fecha para a validação da permuta.** | Os dois colaboradores são relidos depois da trava; a guarda recebe essas instâncias. Os escritores guardados de situação também usam a trava de Colaborador. Isso não transforma “não desligado” em “ativo”. (`delta_v2.patch:+1000`; `app/ponto/portas/celula.py:1010`; `app/colaboradores/services/colaborador.py:80`, `app/colaboradores/services/colaborador.py:130`; `app/colaboradores/services/desligamento.py:32`.) |
| Documento conferido somente na vez do par | **Fecha.** | A permuta verifica os quatro documentos antes de alcançar qualquer chamada interna de troca, inclusive antes do retorno `igual`. É preenchimento do campo; existência/conteúdo do arquivo: **[nao verificado]**. (`delta_v2.patch:+1020`; `app/ponto/portas/celula.py:998`, `app/ponto/portas/celula.py:1018`.) |
| Carta rejeitada | **Fecha a aceitação de `status='rejeitada'` nessas portas.** | A permuta verifica as quatro; a troca isolada verifica seu par, sobre cartas relidas e travadas. Exigir aprovação ou tipo específico continua ausente. (`delta_v2.patch:+749`, `delta_v2.patch:+1021`; `app/ponto/portas/celula.py:734`, `app/ponto/portas/celula.py:987`.) |

Contraexemplo para a garantia de descanso: A, 24x48 07–07, cede 09/11 e recebe 10/11 numa permuta válida. Depois, uma troca isolada faz A assumir 11/11. A termina o primeiro turno em 11/11 às 07h e começa o seguinte nesse mesmo instante; a troca não consulta interjornada. O equivalente para DSR é receber sábado numa permuta e domingo numa troca posterior. São contraexemplos deduzidos dos caminhos; execução: **[nao verificado]**. (`app/ponto/portas/celula.py:818`, `app/ponto/portas/celula.py:915`, `app/ponto/portas/celula.py:931`, `app/ponto/portas/celula.py:753`; `app/ponto/tests/test_o10d_permuta_reciproca.py:48`.)

**2. Ordem invertida**

**Sim, há caminho concreto no código.** A ordem nova é **Colaborador → Ausencia → CelulaDia**. (`delta_v2.patch:+738`, `delta_v2.patch:+998`; `app/ponto/portas/celula.py:734`, `app/ponto/portas/celula.py:982`, `app/ponto/portas/celula.py:984`.)

- **Edição direta: CelulaDia → Colaborador → Chamado.** `editar_dia` trava a célula e, ainda no atomic, chama `_marcar_cobrancas_orfas`; este chama `renovar_contexto`, que pede a trava de Colaborador e Chamado antes do save. (`app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:672`; `app/ponto/management/commands/gerar_celulas.py:264`; `app/chamados/services/contexto.py:103`; `app/chamados/models.py:487`.)

  Interleaving: E segura a célula; P segura seu Colaborador e espera a célula; E encontra cobrança órfã elegível e espera Colaborador. Há ciclo de espera. Exige cobrança que atravesse as guardas do marcador; reprodução: **[nao verificado]**. (`app/ponto/management/commands/gerar_celulas.py:257`, `app/ponto/management/commands/gerar_celulas.py:261`; referências anteriores.)

- **Regeneração: CelulaDia → Colaborador → Chamado.** O UPDATE da célula ocorre antes do mesmo marcador. O command gerador também alcança esse caminho dentro de seu atomic, pela regeneração das células de hoje. (`app/ponto/portas/celula.py:311`, `app/ponto/portas/celula.py:328`; `app/ponto/management/commands/gerar_celulas.py:323`, `app/ponto/management/commands/gerar_celulas.py:345`, `app/ponto/management/commands/gerar_celulas.py:347`.)

- **Porta de ausência: Ausencia → Colaborador.** `decidir` abre atomic e chega ao save de Ausencia. Seu `post_save` chama o cartório sincronamente; havendo emissão de cobrança, a abertura de Chamado pede Colaborador. Portanto uma decisão pode segurar a carta e esperar Colaborador enquanto a permuta segura Colaborador e espera essa carta. Exemplo deduzido: rejeição de atestado passado deixa furo que exige emissão. Execução: **[nao verificado]**. (`app/ponto/services/ausencia.py:1079`, `app/ponto/services/ausencia.py:1088`, `app/ponto/services/ausencia.py:988`; `app/ponto/signals.py:147`, `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:1791`; `app/ponto/management/commands/detectar_ausencias.py:694`; `app/chamados/models.py:952`.)

- **`gerar_celulas_periodo` isoladamente:** pedido explícito posterior de Colaborador **não achei**. Há leitura sem trava, INSERT e atualização de DNA; o caminho composto do command tem a inversão da regeneração acima. (`app/ponto/portas/celula.py:1201`, `app/ponto/portas/celula.py:1218`, `app/ponto/portas/celula.py:1230`; `app/ponto/management/commands/gerar_celulas.py:347`.)

- **`abertas_do_colaborador`:** dentro da autoridade, a sequência continua Colaborador → Disputa → Chamado → Pergunta. Não achei inversão causada apenas por essa flag; ela tampouco verifica as travas que o chamador já adquiriu. (`app/chamados/models.py:2771`, `app/chamados/models.py:2780`, `app/chamados/models.py:2793`, `app/chamados/models.py:2797`.)

**3. Reentrância e precondições**

**Sim, para as mesmas linhas na mesma transação.** A autoridade refaz o `SELECT FOR UPDATE`; o registro evita adicionar sentinelas duplicadas. Na composição normal, a permuta já possui os dois Colaborador quando as duas trocas internas os pedem novamente. (`app/chamados/models.py:2771`, `app/chamados/models.py:2773`; `app/chamados/services/trava_do_colaborador.py:42`; `delta_v2.patch:+738`, `delta_v2.patch:+998`.)

O delta cumpre as precondições indicadas: as chamadas ficam dentro dos atomics existentes e passam listas, tipo aceito pelo primeiro argumento. A restrição de `abertas_do_colaborador=True` a no máximo um colaborador não se aplica, pois o delta não passa essa flag. (`app/ponto/portas/celula.py:729`, `app/ponto/portas/celula.py:972`; `app/chamados/models.py:2742`, `app/chamados/models.py:2763`, `app/chamados/models.py:2765`; `delta_v2.patch:+738`, `delta_v2.patch:+998`.)

**Reentrância não garante ordem por transação:** a função ordena os IDs daquela chamada; não impede acrescentar outro Colaborador depois de já possuir células, Chamados ou Perguntas. Chamador de produção envolvendo a permuta dessa maneira: **não achei [nao verificado]**. (`app/chamados/models.py:2757`, `app/chamados/models.py:2763`, `app/chamados/models.py:2771`.)

**4. Catraças**

**Não achei catraca que fique vermelha pelas adições mostradas; execução: [nao verificado].**

- A catraca de travas proíbe `select_for_update` das quatro entidades fora da autoridade. O delta chama a autoridade, sem criar outro escritor direto de trava de Colaborador. (`app/chamados/tests/test_e1_v7_catracas.py:85`, `app/chamados/tests/test_e1_v7_catracas.py:116`, `app/chamados/tests/test_e1_v7_catracas.py:131`; `delta_v2.patch:+738`, `delta_v2.patch:+998`.)
- A16 exclui imports dentro de funções; os novos imports de `chamados.models` são locais. (`app/core/tests/test_contract_direcao_a16.py:88`, `app/core/tests/test_contract_direcao_a16.py:95`, `app/core/tests/test_contract_direcao_a16.py:123`; `delta_v2.patch:+736`, `delta_v2.patch:+996`.)
- O selo Disputa→Colaborador examina travas diretas dentro da função; não demonstra a ordem dos caminhos indiretos encontrados acima. (`app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:90`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:110`.)

Lista AST fechada de **quem pode chamar** `trava_na_ordem`: **não achei [nao verificado]**.

**5. Os testes mordem?** [nao verificado]

Por leitura, **os cinco testes novos falhariam no hash**; execução efetiva: **[nao verificado]**. [nao verificado]

| Teste novo | Por que distingue a base | [nao verificado]
|---|---|
| Desligado no banco, instância velha | A base pergunta à instância recebida. (`delta_v2.patch:+219`; `app/ponto/portas/celula.py:863`.) |
| Carta rejeitada | A base não verifica rejeição e alcança a troca mockada, contrariando a recusa exigida. (`delta_v2.patch:+256`; `app/ponto/portas/celula.py:994`, `app/ponto/portas/celula.py:1018`.) |
| Documento antes da primeira troca | A base só confere documento dentro da troca, que o teste substitui. (`delta_v2.patch:+266`; `app/ponto/portas/celula.py:741`, `app/ponto/portas/celula.py:1018`.) |
| Primeira trava na permuta | Na base, a primeira trava é de Ausencia. (`delta_v2.patch:+331`; `app/ponto/portas/celula.py:982`.) |
| Primeira trava na troca isolada | Na base, a primeira trava também é de Ausencia. (`delta_v2.patch:+331`; `app/ponto/portas/celula.py:734`.) |

O **rollback reescrito passaria na base**, pois o atomic e a conversão da segunda recusa já existem. **Não é vazio:** executa a primeira troca real, exige sua trilha e exige duas chamadas. (`delta_v2.patch:+290`; `app/ponto/portas/celula.py:972`, `app/ponto/portas/celula.py:1019`.)

Os testes de SQL comprovam a primeira instrução de trava, mas não demonstram concorrência ou proteção de célula ausente: a fixture gera as células previamente. Testes novos com segunda conexão, mudança de dono da carta ou rejeição na troca isolada: **não achei no delta [nao verificado]**. (`app/ponto/tests/test_o10b_editar_dia_troca.py:91`; `app/ponto/tests/test_o10d_permuta_reciproca.py:70`; `delta_v2.patch:+331`.)

Há uma fragilidade adicional no teste antigo de callbacks: ele conta a fila bruta, mas a trava registra sentinelas `on_commit`. A fixture pode já trazer os colaboradores registrados pela geração de matrícula; portanto não concluo que surgirão exatamente três callbacks nesse teste. O repo já possui contador que exclui esses sentinelas. (`app/ponto/tests/test_o10d_permuta_reciproca.py:305`; `app/chamados/services/trava_do_colaborador.py:47`; `app/colaboradores/models.py:436`; `app/chamados/tests/test_e1_v13_ordem.py:8`; `app/ponto/tests/test_selo_chokepoint_escrita.py:258`.)

**6. Quem escreve, quem lê e os significados**

| Campo | Escritores encontrados | Leitores e significado |
|---|---|---|
| `Colaborador.situacao` | Portas `gravar`, `salvar_campos`, `salvar_inteiro`, `atualizar_em_lote`; desligamento atribui `desligado`, reativação atribui `ativo`. (`app/colaboradores/services/colaborador.py:126`, `app/colaboradores/services/colaborador.py:140`, `app/colaboradores/services/colaborador.py:152`, `app/colaboradores/services/colaborador.py:164`; `app/colaboradores/services/desligamento.py:129`, `app/colaboradores/services/desligamento.py:242`.) | A permuta lê `esta_desligado`, que reconhece somente esse estado. Afastado não é desligado; desligamento futuro agendado ainda não altera a situação. (`app/ponto/portas/celula.py:863`; `app/colaboradores/services/desligamento.py:29`, `app/colaboradores/services/desligamento.py:32`.) |
| `Ausencia.status` | Criação escolhe estado inicial; `mudar_estado` persiste transições; `decidir` aceita aprovação/rejeição. Cancelamento de férias também grava `rejeitada`. (`app/ponto/services/ausencia.py:475`, `app/ponto/services/ausencia.py:486`, `app/ponto/services/ausencia.py:976`, `app/ponto/services/ausencia.py:1057`, `app/ponto/services/ausencia.py:1496`.) | Folha exige aprovada; cobrança e sobreposição excluem rejeitada. (`app/ponto/turnos.py:2528`, `app/ponto/turnos.py:2531`, `app/ponto/turnos.py:2839`.) |

Não achei significado incompatível de **desligado** nesses caminhos. Porém **não desligado ≠ ativo**: existem `afastado` e `ferias`, e essa diferença continua na guarda. (`app/colaboradores/services/desligamento.py:30`; `app/colaboradores/models.py:295`; `app/ponto/portas/celula.py:863`.)

**Não rejeitada ≠ aprovada:** documento presente normalmente produz `aguardando_decisao`, ainda aberto. Além disso, rejeitada também representa férias canceladas; não permite concluir falsidade do documento. (`app/ponto/services/ausencia.py:912`, `app/ponto/services/ausencia.py:938`, `app/ponto/services/ausencia.py:1496`.)

Permanece o conflito das **cartas como comprovantes e fatos de ausência**: a permuta exclui seus IDs da própria guarda, mas a grade continua consultando cobertura aprovada; uma carta integral aprovada pode transformar o dia assumido em ausência e apagar seus marcos. (`app/ponto/portas/celula.py:897`; `app/escala/utils.py:1188`, `app/escala/utils.py:1220`, `app/escala/utils.py:1251`.)

**7. Outros defeitos e limites**

- **A guarda de mudança de colaborador é incompleta.** Compara conjuntos: se as duas cartas passam de donos A/B para B/A, o conjunto permanece `{A,B}` e a troca executa os papéis invertidos. As travas ainda cobrem os dois donos, mas a promessa de recusar carta cujo dono mudou não é cumprida. Escritor de produção que altere `Ausencia.colaborador`: **não achei [nao verificado]**; a falha lógica está na comparação. (`delta_v2.patch:+737`, `delta_v2.patch:+747`; `app/ponto/portas/celula.py:751`, `app/ponto/portas/celula.py:753`; `app/ponto/services/ausencia.py:648`.)

- **Tipo e aprovação continuam sem exigência.** O caso positivo usa `declaracao/aguardando_decisao`; excluir rejeitada não resolve esse achado da volta 1. (`app/ponto/tests/test_o10b_editar_dia_troca.py:106`; `app/ponto/tests/test_o10d_permuta_reciproca.py:130`; `delta_v2.patch:+1021`.)

- **A regeneração ainda pode sobrescrever com leitura antiga.** Ela carrega células sem trava e decide pela origem dessa instância; a nova trava da permuta não é adquirida antes dessa leitura/escrita. (`app/ponto/portas/celula.py:227`, `app/ponto/portas/celula.py:298`, `app/ponto/portas/celula.py:311`.)

- **Persistem `d1=d2` recusado e idempotência insuficiente.** O delta não altera a recusa, nem faz `igual` comparar DNA ou conferir que a última edição ainda pertence à permuta; esse retorno continua anterior à guarda de desligamento. (`app/ponto/portas/celula.py:975`, `app/ponto/portas/celula.py:1003`, `app/ponto/portas/celula.py:1005`, `app/ponto/portas/celula.py:1010`.)

## 17/11 03:23 · sessao e39e9a130d99 · - · - · origem=Ronald (tmux)

**4. Catraças: não achei uma que fique vermelha pelas mudanças mostradas; execução efetiva: [nao verificado].**

- A catraca de travas proíbe `select_for_update` de Colaborador/Disputa/Chamado/Pergunta fora da autoridade; não proíbe chamar a autoridade. O delta adiciona chamadas a `PerguntaDisputa.trava_na_ordem`, sem novo `Colaborador.select_for_update`. (`app/chamados/tests/test_e1_v7_catracas.py:71`, `app/chamados/tests/test_e1_v7_catracas.py:85`, `app/chamados/tests/test_e1_v7_catracas.py:116`, `app/chamados/tests/test_e1_v7_catracas.py:131`; `delta_v2.patch`, hunks `@@ -730` e `@@ -976`)
- A16 considera imports de topo e exclui os que estão dentro de funções; os imports novos de `chamados.models` são locais. Portanto essas adições não infringem essa catraca. (`app/core/tests/test_contract_direcao_a16.py:88`, `app/core/tests/test_contract_direcao_a16.py:95`, `app/core/tests/test_contract_direcao_a16.py:123`; `delta_v2.patch`, hunks `@@ -730` e `@@ -976`)
- O selo Disputa→Colaborador examina `select_for_update` direto na mesma função e não segue chamadas indiretas; as adições não introduzem esse padrão. (`app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:12`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:20`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:110`)
- Catraça AST que fixe uma lista de **quem pode chamar** `trava_na_ordem`: **não achei** [nao verificado].

**5. Mordida dos testes, por leitura; execução: [nao verificado].** [nao verificado]

| Teste do delta | Passaria no hash? | Evidência | [nao verificado]
|---|---|---|
| `test_R3_desligado_le_o_BANCO_e_nao_a_instancia` | **Não**, por leitura: o hash consulta `esta_desligado` sobre a instância recebida. | `app/ponto/portas/celula.py:863`; delta `@@ -216` |
| `test_R5_carta_REJEITADA_nao_e_carta` | **Não**: o hash não verifica rejeição e alcança o mock de troca; falta a recusa anterior exigida. | `app/ponto/portas/celula.py:994`, `app/ponto/portas/celula.py:1018`, `app/ponto/portas/celula.py:1042`; delta `@@ -246` |
| `test_R5_carta_sem_documento_barra_ANTES_da_1a_escrita` | **Não**: a conferência do hash está na troca, que o teste substitui; a permuta não recusa antes dela. | `app/ponto/portas/celula.py:741`, `app/ponto/portas/celula.py:1018`; delta `@@ -246` |
| Primeira trava de Colaborador na permuta | **Não**: a primeira trava explícita do hash é Ausencia. | `app/ponto/portas/celula.py:982`; delta `@@ -291` |
| Primeira trava de Colaborador na troca isolada | **Não**: a primeira trava explícita do hash é Ausencia. | `app/ponto/portas/celula.py:734`; delta `@@ -291` |
| Rollback reescrito, segunda recusa forjada | **Sim, por leitura**: a rede de rollback já existe no hash. Não é vazio: executa a primeira troca real, exige trilha e exige duas chamadas. | `app/ponto/portas/celula.py:972`, `app/ponto/portas/celula.py:1019`; delta `@@ -265` |

Os testes novos de SQL provam a posição da primeira instrução, mas **não demonstram ausência de deadlock nem proteção de células ausentes**: a fixture gera as células previamente e os testes mostrados usam uma única execução, sem segunda conexão concorrente. (`app/ponto/tests/test_o10b_editar_dia_troca.py:91`, `app/ponto/tests/test_o10d_permuta_reciproca.py:70`; delta `@@ -291`)

Testes novos de concorrência interjornada/DSR, de carta cujo colaborador mudou e de rejeição na troca isolada: **não achei no delta** [nao verificado]. [nao verificado]

**Cuidado com a contagem de callbacks:** `trava_na_ordem` registra um sentinela `on_commit` por colaborador ainda não registrado; logo callback não equivale necessariamente a rabo. (`app/chamados/models.py:2773`, `app/chamados/services/trava_do_colaborador.py:37`, `app/chamados/services/trava_do_colaborador.py:47`)

Não afirmaria que o teste antigo `len(cbs)==1` passa a falhar com três callbacks: sua fixture cria colaboradores sem matrícula, e cada criação trava o colaborador anterior para gerar matrícula; assim os colaboradores usados podem estar registrados desde o atomic da classe. Essa contaminação de fixture já é documentada por outro selo. (`app/ponto/tests/test_o10d_permuta_reciproca.py:305`, `app/ponto/tests/test_o10b_editar_dia_troca.py:95`, `app/colaboradores/models.py:427`, `app/colaboradores/models.py:436`, `app/chamados/tests/test_e1_v13_ordem.py:8`)

Já existe no repo uma contagem que exclui explicitamente esses sentinelas. (`app/ponto/tests/test_selo_chokepoint_escrita.py:258`)

_6330 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
