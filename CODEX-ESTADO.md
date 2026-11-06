# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 07:49** · publicado em **06/11/2026 07:51** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 07:49 · sessao ef717d1946a3 · LASTRO-PARTE-3 · leitura · origem=Code

**Os cinco atos admitem montagem em TestCase, mas nem todo ramo deve fechar o chamado.** Em particular, uma resposta discordante continua prevalecendo sobre `carimbar_identidade`; esse é um controle legítimo. (`app/chamados/models.py:2625`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`)

Os vermelhos abaixo são **previstos pela leitura da cópia**; execução dos novos casos e confirmação independente do hash: **[nao verificado]**. Ocorrência ou quantidade desses estados em produção: **[nao verificado]**.

**Peças de fixture já existentes**

Nenhum dos helpers abaixo, isoladamente, entrega toda a combinação pedida; é necessário compor chamado, âncora e célula. O A5 de LASTRO-PARTE-2 simula a recusa com mock, portanto não demonstra retenção por C6 real. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:63`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:144`)

| Peça | Helper existente | Teste que o usa | [nao verificado]
|---|---|---|
| Cadastro com colaborador, usuário e admin; chamados e disputa com pai/destino separados | `_Base.setUpTestData`, `_ch`, `_disputa`, `_pergunta`, em `app/chamados/tests/test_o261e_f1_porta_encerramento.py:49`, `:59`, `:65`, `:69` | Retenção real por destino: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82` |
| Pergunta de hora já ancorada; aceita sobrescrever motivo, resposta, data e célula por `**kw` | `PortaValidacaoPerguntaTest._pergunta`, `app/chamados/tests/test_porta_validacao_pergunta.py:175` | Recusa pública: `app/chamados/tests/test_porta_validacao_pergunta.py:233` |
| Pergunta discordante, DNA, lâmpada e dia coerente | `PerguntaRespondidaEMarcoAcesoTest._caso('19:06')`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:36` | Confirma C6 viva aos seis minutos: `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:59` |
| Célula acusando outro marco; pergunta muda com materialização opcional | `_Base._celula` e `_pergunta`, `app/chamados/tests/test_bug117_marco_no_espelho.py:55`, `:64` | Marco aceso sem materialização ainda deixa pergunta viva: `app/chamados/tests/test_bug117_marco_no_espelho.py:137` |
| Chamado cobrando marcos nomeados e ata plantada depois dos sinais | `_Base._caso`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:63` | `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:97` |
| Vínculo válido no dia | `dar_vinculo`, `app/chamados/tests/_vinculo_do_dia.py:15` | `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:61` |

Para os casos, a composição concreta é esta:

- Colaborador ativo, com vínculo, usuário e autor administrativo; competência sem entrega/tranca. Usar os cadastros F1 e o vínculo acima; o gate só considera perguntas da competência aberta. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:49`, `app/chamados/tests/_vinculo_do_dia.py:15`, `app/chamados/models.py:347`)
- Alvo `batida_ausente`, `status_local='em_analise'`, sem silêncio; dia do chamado igual ao da célula e da pergunta; `contexto_json.marcos_faltantes` contendo **somente S/19:00**. A receita Q6 entrega chamado e âncora; a LP2 mostra como nomear o marco cobrado. (`app/chamados/tests/test_porta_validacao_pergunta.py:179`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:68`)
- Pergunta `orfao_14h`, `tipo_resposta='hora'`, `chamado_destino=alvo`, `celula=cel`; DNA com `hf='19:00'`, lâmpada S/19:00 acesa e `luz='19:00'`. Esse motivo cobra precisamente S/`hf`. (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:21`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:36`, `app/chamados/juizes.py:869`)
- Plantar a ata final depois das criações que acionam o cartório, seguindo a ordem LP2; não deixar outra cobrança viva apta a receber reata do mesmo colaborador/dia. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:64`, `app/chamados/models.py:373`)
- Antes do ato, exigir **C6 viva**, `marcos_acesos(alvo, cel) is True`, alvo na fila e `alvo.reconciliaria() is False`. Assim o caso prova retenção real, não apenas a presença de uma pergunta. (`app/chamados/juizes.py:1135`, `app/ponto/services/lastro.py:153`, `app/chamados/services/painel.py:186`, `app/chamados/models.py:623`)

Em cada ato com `save`, executar os callbacks com `captureOnCommitCallbacks(execute=True)`, padrão já usado na suíte; o evento da pergunta é registrado em `on_commit`. Relê-se pergunta, célula e chamado antes das asserções. (`app/chamados/tests/test_varredura_becos_evento_pergunta.py:55`, `app/chamados/signals.py:191`, `app/chamados/signals.py:202`)

A ordem abaixo considera primeiro o aproveitamento direto da fixture discordante, depois os acréscimos necessários. **[nao verificado]**

1. **Corrigir a resposta para a hora da luz — RED previsto**

   **Fixture:** composição acima, pai da disputa igual ao alvo vivo; resposta anterior `19:06`, `respondida_em` preenchido, `validada_em=None`, `materializada_em=None`; célula `concorde`, via `cartorio`, demais lâmpadas acesas. A discordância mantém C6 viva mesmo com o marco aceso. (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:36`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:59`)

   **Ato exato:** `responder_pergunta_disputa_core(p, '19:00', colab.usuario, True)`. A função grava texto e instante da nova resposta. (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1282`)

   **Pela regra:** C6 morre pela concordância com a luz em dia coerente; o alvo passa a `reconciliaria()=True` e deve sair da fila por `reconciliar()`, cujo destino é `resolvido`, via sistema. (`app/chamados/juizes.py:1130`, `app/chamados/juizes.py:1162`, `app/chamados/models.py:623`, `app/chamados/catalogo/motor.py:95`, `app/chamados/catalogo/motor.py:111`)

   **Asserção vermelha:** exigir que o `status_local` relido esteja fora de `VIVOS`, ao final do ato e dos callbacks. A leitura prevê que continue `em_analise`: o ramo HH:MM salva a resposta e acrescenta nota, sem reconciliar o destino; o resgate não atua sem silêncio, e o beco não atua com pai vivo. (`app/chamados/services/disputa_emissao.py:1301`, `app/chamados/services/disputa_emissao.py:1326`, `app/chamados/services/disputa_emissao.py:1355`, `app/chamados/services/materializacao.py:687`, `app/chamados/services/becos.py:31`)

   **Controle:** trocar o veredito para `furo` faz a concordância com a luz deixar de absolver a pergunta; o chamado deve continuar vivo. (`app/chamados/juizes.py:1132`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:62`)

2. **Recusar a resposta / `reabrir_para_correcao` — RED previsto**

   **Fixture:** a mesma do caso 1, ainda com resposta discordante `19:06` e célula coerente. A receita da recusa pública já existe na suíte. (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:59`, `app/chamados/tests/test_porta_validacao_pergunta.py:224`)

   **Ato público exato:** `recusar_pergunta(p.pk, admin, 'hora incorreta')`. O ato interno relevante é `reabrir_para_correcao(PerguntaDisputa.objects.filter(pk=p.pk))`, com `desfazer_absorcao=False`, o padrão. (`app/chamados/services/validacao.py:545`, `app/chamados/services/validacao.py:547`, `app/chamados/services/disputa_emissao.py:1362`)

   **Pela regra:** apagada a resposta discordante, C6 morre pelo veredito coerente da célula; o alvo torna-se reconciliável e deve sair da fila pelo mesmo gate. (`app/chamados/services/disputa_emissao.py:1393`, `app/chamados/juizes.py:1195`, `app/chamados/models.py:623`)

   **Asserção vermelha:** novamente, `status_local` relido fora de `VIVOS`. A recusa salva primeiro o motivo, enquanto a resposta ainda retém; depois o `qs.update` remove resposta e validação, sem `post_save` dessa mudança e sem reconciliação na função. A nota posterior também não chama o gate nesse caminho. (`app/chamados/services/validacao.py:545`, `app/chamados/services/disputa_emissao.py:1413`, `app/chamados/services/validacao.py:551`)

   **Controle:** com célula `furo`, pergunta muda e sem materialização continua viva pelo veredito acusador; nesse ramo, reabrir não autoriza fechar. (`app/chamados/juizes.py:1183`, `app/chamados/tests/test_bug117_marco_no_espelho.py:137`)

3. **`carimbar_identidade` — um RED e um controle obrigatório**

   **Fixture do RED:** pai vivo; pergunta **muda**, sem materialização ou validação; célula `furo`, via `cartorio`, porque S/12:00 está apagada; o alvo cobra exclusivamente S/19:00, que está acesa. A receita BUG117 já demonstra pergunta viva com outro marco apagado e o próprio marco aceso, desde que ainda sem materialização. (`app/chamados/tests/test_bug117_marco_no_espelho.py:77`, `app/chamados/tests/test_bug117_marco_no_espelho.py:137`)

   **Ato exato:** `p.carimbar_identidade(PerguntaDisputa.VIA_ADMIN_DECIDIU, admin)`. Essa é uma via usada pelo próprio encerramento administrativo da pergunta. (`app/chamados/models.py:2522`, `app/chamados/models.py:2625`)

   **Pela regra:** C6 morre pela identidade; o furo vizinho não impede regularizar a premissa do chamado que cobra somente o marco aceso. Portanto, `reconciliaria()=True` e o alvo deve sair por `reconciliar()`. (`app/chamados/juizes.py:1171`, `app/chamados/models.py:1247`, `app/chamados/models.py:623`)

   **Asserção vermelha:** `status_local` relido fora de `VIVOS`. O carimbador termina salvando somente a pergunta; seu callback não encontra beco com pai vivo. (`app/chamados/models.py:2640`, `app/chamados/models.py:2644`, `app/chamados/services/becos.py:31`)

   **Controle obrigatório:** aplicar o mesmo carimbo à fixture discordante do caso 1. A pergunta continua viva porque a resposta é julgada **antes** da identidade; sem reata, `reconciliaria()` continua falso e o chamado deve permanecer vivo. Exigir fechamento aqui produziria um teste contrário à regra. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1162`, `app/chamados/models.py:2630`, `app/chamados/models.py:405`)

4. **Encerrar administrativamente a disputa com pai terminal e destino distinto — RED previsto**

   **Fixture:** a pergunta muda e a célula acusadora do caso 3. Acrescentar um pai `disputa_supervisao` já `resolvido`, criado **antes das perguntas**, e manter `chamado_destino=alvo` vivo, diferente do pai. Os helpers F1 permitem essa separação; o teste administrativo existente cobre o outro ramo, com pai vivo. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:59`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:65`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:69`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:220`)

   **Ato exato:** `d.admin_encerra(admin, 'decisao no teste')`, que chama `d.fechar(admin, resolucao='decisao no teste', motivo=d.FECHOU_ADMIN_ENCERRA)`. (`app/chamados/models.py:1872`, `app/chamados/models.py:1879`)

   **Pela regra:** a porta carimba a muda antes de fechar a disputa; C6 morre e o destino com marco aceso torna-se reconciliável. É o **destino** que deve sair da fila pelo gate. (`app/chamados/models.py:1852`, `app/chamados/juizes.py:1171`, `app/chamados/models.py:1249`, `app/chamados/models.py:623`)

   **Asserção vermelha:** `status_local` relido do **alvo**, fora de `VIVOS`. O receiver consulta somente o pai e retorna porque ele já está encerrado; o materializador posterior exclui a pergunta recém-carimbada. O beco também retorna quando a disputa já está fechada. (`app/chamados/signals.py:32`, `app/chamados/signals.py:33`, `app/chamados/services/materializacao.py:1343`, `app/chamados/services/becos.py:27`)

5. **Validar pergunta já materializada — RED previsto, montagem mais exigente**

   **Fixture:** resposta discordante `19:06`, célula coerente e luz `19:00`, como no caso 1; acrescentar `materializada_em` preenchido e `validada_em=None`. O helper BUG117 já monta materialização, e seu teste demonstra a precedência da resposta sobre ela. (`app/chamados/tests/test_bug117_marco_no_espelho.py:64`, `app/chamados/tests/test_bug117_marco_no_espelho.py:145`)

   Usar pai vivo **diferente do alvo**, com módulo `disputa_supervisao`; acrescentar uma irmã muda, não materializada, de outro dia, ancorada no pai e sem célula. A irmã permanece viva e impede quitar a disputa, sem reter o alvo; esse pai também não pertence aos módulos usados para procurar uma cobrança sobrevivente. (`app/chamados/juizes.py:1207`, `app/chamados/models.py:328`, `app/chamados/models.py:1794`, `app/chamados/catalogo/modulos.py:235`, `app/chamados/models.py:373`)

   **Ato exato:** `validar_pergunta(p.pk, admin, confirmar_implausivel=True)`, executando também os callbacks. O argumento explícito evita que a guarda de plausibilidade impeça alcançar o ramo examinado. (`app/chamados/services/validacao.py:343`, `app/chamados/services/validacao.py:374`)

   **Pela regra:** `validada_em` mata C6 imediatamente; o alvo passa a reconciliável e deve sair da fila pelo gate, mesmo que a disputa continue aberta por outra pergunta. (`app/chamados/juizes.py:1136`, `app/chamados/models.py:327`, `app/chamados/models.py:623`)

   **Asserção vermelha:** `status_local` relido do alvo fora de `VIVOS`. A validação carimba, mas pula `_materializar_uma` porque a pergunta já está materializada; o lote também a exclui. A irmã impede fechar a disputa, e o pai vivo impede o beco. (`app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:422`, `app/chamados/services/materializacao.py:1297`, `app/chamados/services/materializacao.py:1241`, `app/chamados/services/becos.py:31`)

   O recálculo mensal agendado precisa ser executado no teste; sua chamada não equivale, por si, a uma chamada ao gate do chamado ou à emissão de `EventoCelula`. (`app/ponto/services/fechamento.py:1791`, `app/ponto/services/fechamento.py:1794`, `app/ponto/services/fechamento.py:1808`)

**Quem escreve e quem lê os campos usados nessas verificações**

A tabela delimita os escritores da fixture/atos e os leitores relevantes aos casos; não é um censo de todos os leitores do projeto. **[nao verificado]**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `Chamado.status_local` — campo da asserção RED | Fixture `_ch`; em produção, `reconciliar()` chama `_gravar_estado`, que atribui e salva. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:59`, `app/chamados/models.py:620`, `app/chamados/models.py:222`, `app/chamados/models.py:279`) | Asserção sobre objeto relido; motor do fechamento; fila. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:77`, `app/chamados/models.py:639`, `app/chamados/services/painel.py:187`) |
| `silenciado_ate` — pertinência à fila | Nasce nulo; portas de arquivo atribuem ou limpam. (`app/chamados/models.py:84`, `app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:201`) | Fila e resgate por resposta. (`app/chamados/services/painel.py:188`, `app/chamados/services/materializacao.py:687`) |
| `contexto_json.data_turno`, `marcos_faltantes`, `modulo_origem` | Helpers de chamado e fixture LP2. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:59`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:68`) | Seleção de `batida_ausente`, extração dos marcos e premissa regularizada. (`app/ponto/services/lastro.py:240`, `app/ponto/services/lastro.py:74`, `app/chamados/models.py:1247`) |
| `Pergunta.chamado_destino`, `disputa`; `Disputa.chamado` | Helpers de âncora Q6/F1. (`app/chamados/tests/test_porta_validacao_pergunta.py:184`, `app/chamados/tests/test_porta_validacao_pergunta.py:186`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:65`) | Coletor de ancoradas e julgamento no pós-estado; receiver do pai. (`app/chamados/models.py:328`, `app/chamados/models.py:344`, `app/chamados/signals.py:32`) |
| `motivo`, `tipo_resposta`, `data_evento`, `celula` da pergunta | Helper Q6 aceita esses dados; PRAM/BUG117 montam motivo, dia e célula. (`app/chamados/tests/test_porta_validacao_pergunta.py:186`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:47`, `app/chamados/tests/test_bug117_marco_no_espelho.py:68`) | Mapeamento do marco, comparação com a luz e cerca da competência. (`app/chamados/juizes.py:886`, `app/chamados/juizes.py:1099`, `app/chamados/models.py:347`) |
| `resposta_colab`, `respondida_em` | Core da resposta; reabertura zera ambos por `update`. (`app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1413`) | C6 dá precedência à resposta e compara seu texto com a luz. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1113`) |
| `validada_em` | `_carimbar_validacao`; nova resposta e reabertura podem limpar. (`app/chamados/services/validacao.py:131`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1393`) | Primeiro degrau de C6 e filtro das ancoradas. (`app/chamados/juizes.py:1136`, `app/chamados/models.py:327`) |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, persistido pelo carimbador de identidade; BUG117 também os prepara como fixture. (`app/chamados/models.py:2556`, `app/chamados/models.py:2644`, `app/chamados/tests/test_bug117_marco_no_espelho.py:71`) | C6; validação e materializadores leem a presença da materialização. (`app/chamados/juizes.py:1171`, `app/chamados/services/validacao.py:422`, `app/chamados/services/materializacao.py:1297`) |
| `Celula.colaborador`, `data`, `trabalha`, `dna.marcos` | Fixture `_celula`; porta de célula também grava DNA/trabalho. (`app/chamados/tests/test_bug117_marco_no_espelho.py:55`, `app/ponto/portas/celula.py:650`) | Localização da célula do chamado, premissa e identificação do marco da pergunta. (`app/ponto/services/lastro.py:393`, `app/chamados/models.py:1245`, `app/chamados/juizes.py:893`) |
| `ata.lampadas.{tipo,hora,acesa,luz}` | Fixtures PRAM/LP2; cartório constrói tipo, hora, acendimento e luz. (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:39`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:72`, `app/ponto/services/cartorio.py:677`) | `marcos_acesos`, fato do marco e concordância resposta/luz. (`app/ponto/services/lastro.py:174`, `app/chamados/juizes.py:910`, `app/chamados/juizes.py:1111`) |
| `veredito`, `veredito_via` | Fixture `_celula`; produção pela lavra da célula. (`app/chamados/tests/test_bug117_marco_no_espelho.py:60`, `app/ponto/portas/celula.py:812`) | C6 distingue decisão administrativa, dia coerente e dia acusador. (`app/chamados/juizes.py:1147`, `app/chamados/juizes.py:1132`, `app/chamados/juizes.py:1183`) |
| `Disputa.fechada_em` | `Disputa.fechar`. (`app/chamados/models.py:1861`, `app/chamados/models.py:1865`) | Receiver do pai, classificador de beco e cauda da materialização. (`app/chamados/signals.py:28`, `app/chamados/services/becos.py:27`, `app/chamados/services/materializacao.py:1241`) |

Para reproduzir especificamente o escape do evento, a fixture deve começar sem evento pendente do colaborador; há helper de limpeza já existente. Após o ato, o diagnóstico esperado por leitura é alvo ainda vivo, agora em `com_lastro`, e contável em `com_lastro_sem_evento`. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:75`, `app/ponto/services/lastro.py:318`, `app/ponto/services/lastro.py:421`)

**Não chamar `reconciliar()` antes da asserção vermelha:** isso executaria justamente o fechamento cuja reação está faltando. `reconciliaria()` é a consulta adequada para demonstrar que o juiz já permitiria a saída. (`app/chamados/models.py:617`, `app/chamados/models.py:620`, `app/chamados/models.py:623`)

## 06/11 07:45 · sessao 66bbc807ce84 · - · - · origem=Ronald (tmux)

**Resposta HH:MM — montável e candidato RED.** Base concreta: pergunta `orfao_14h`, `tipo_resposta='hora'`, resposta anterior `19:06`, `respondida_em` preenchido, célula com DNA `hf='19:00'`, lâmpada S/19:00 acesa com `luz='19:00'`, veredito `concorde`, via `cartorio`; esse conjunto já existe em `PerguntaRespondidaEMarcoAcesoTest._caso` e seu teste discordante (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:36`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:59`). Falta complementá-lo com chamado `batida_ausente` aberto, cobrando exclusivamente S/19:00, `chamado_destino` da pergunta igual a esse chamado, pai da disputa vivo e ausência de outro chamado apto à reata; o helper LP2 `_caso` fornece chamado+ata e o helper F1 `_disputa`/`_pergunta` fornece a montagem da âncora, mas nenhum desses helpers isoladamente entrega todo o estado (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:63`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:65`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:69`).

Ato: `responder_pergunta_disputa_core(p, '19:00', colab.usuario, True)`. O core grava resposta e carimbo, só materializa escapes de ausência, acrescenta followup e retorna; HH:MM não chama reconciliação nem emite evento nesse corpo (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1327`, `app/chamados/services/disputa_emissao.py:1343`, `app/chamados/services/disputa_emissao.py:1359`). C6 passa a morta porque resposta concorda com a luz em dia coerente (`app/chamados/juizes.py:1099`, `app/chamados/juizes.py:1130`, `app/chamados/juizes.py:1158`). O gate, antes retido por pergunta sem reata, passa a permitir o fechamento (`app/chamados/models.py:373`, `app/chamados/models.py:405`, `app/chamados/models.py:623`). O RED esperado é a permanência em `qs_na_fila` depois do ato; execução efetiva desse teste: **[nao verificado]**. O contrato existente considera defeito lastro elegível sem evento pendente (`app/ponto/services/lastro.py:411`).

**Recusa/reabertura — montável e candidato RED no mesmo estado coerente.** Ato público: `recusar_pergunta(p.pk, admin, 'hora incorreta')`; ato interno equivalente relevante: `reabrir_para_correcao(PerguntaDisputa.objects.filter(pk=p.pk))` (`app/chamados/services/validacao.py:494`, `app/chamados/services/validacao.py:545`, `app/chamados/services/validacao.py:547`). Há helper/teste real dessa recusa, `_pergunta` e `test_MORDE_recusar_SEM_request_carimba_reabre_e_deixa_trilha`, mas ele precisa ganhar célula, DNA, lâmpada e marco cobrado para este caso (`app/chamados/tests/test_porta_validacao_pergunta.py:175`, `app/chamados/tests/test_porta_validacao_pergunta.py:224`).

A recusa primeiro salva o motivo enquanto a resposta discordante ainda retém; depois `qs.update` apaga resposta e carimbos sem novo `post_save` da pergunta (`app/chamados/services/validacao.py:545`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1413`). Sem resposta e com veredito coerente, C6 morre, permitindo `reconciliaria=True`, mas nenhum fechamento aparece nesse caminho; a mesma asserção de saída da fila é o RED previsto, ainda **[nao verificado]** por execução (`app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1195`, `app/chamados/models.py:623`, `app/chamados/services/validacao.py:548`).

**Controle necessário:** se a célula ainda acusa outro marco (`veredito='furo'`), recusar/apagar a resposta deixa a pergunta viva; nesse cenário não cabe exigir fechamento só porque o marco cobrado está aceso (`app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1183`). Concordar com a luz também não mata a pergunta enquanto o dia acusa (`app/chamados/juizes.py:1132`, `app/chamados/juizes.py:1162`; teste existente `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:62`).

**Não deixar o TestCase esconder callbacks:** o `post_save` de pergunta agenda o beco via `on_commit`; convém executar os callbacks capturados ao redor do ato. Mantendo o pai vivo, o beco continua sem agir; mantendo `silenciado_ate=None`, o resgate também não interfere (`app/chamados/signals.py:191`, `app/chamados/signals.py:202`, `app/chamados/services/becos.py:31`, `app/chamados/services/materializacao.py:687`).

**Limite do helper LP2:** A5 anuncia retenção por pergunta viva, mas efetivamente usa mock de `reconciliar=False`; não serve como prova de fixture com C6 real (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:137`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:144`).

## 06/11 07:45 · sessao 5408b8736f70 · - · - · origem=Ronald (tmux)

Os dois atos são montáveis; `carimbar_identidade` exige dois casos, pois resposta discordante continua viva por precedência. (`app/chamados/models.py:2625-2632`; `app/chamados/juizes.py:1150-1172`)

1. **Controle: identidade sobre resposta discordante — permanece vivo, sem RED legítimo.** Partindo da pergunta ancorada respondida e discordante, executar `p.carimbar_identidade(PerguntaDisputa.VIA_DUPLICATA, admin)`. O carimbo grava `materializada_em`, autor e via, mas C6 decide pela resposta antes de consultar identidade; consequentemente `pergunta_viva(p)=True`, `ch.reconciliaria()=False` sem sobrevivente, chamado permanece vivo. (`app/chamados/models.py:2633-2644`; `app/chamados/juizes.py:1150-1172`; `app/chamados/models.py:393-414`)

2. **Identidade sobre pergunta muda — RED possível.** Montar célula `veredito='furo', veredito_via='cartorio'`, lâmpada cobrada pelo chamado acesa e outro marco apagado; pergunta muda, sem materialização, ancorada no chamado. Antes, C6 viva pelo veredito acusador; depois de `p.carimbar_identidade(PerguntaDisputa.VIA_DUPLICATA, admin)`, C6 morta pela via. `marcos_acesos=True` regulariza a premissa independentemente do outro furo; portanto `ch.reconciliaria()=True`. O ato somente salva a pergunta e seu callback trata beco, sem reconciliar destino; com pai vivo, nem beco existe. RED: chamado deve sair da fila ao final do ato/callbacks, mas permanece. (`app/chamados/juizes.py:1181-1184`; `app/chamados/juizes.py:1170-1172`; `app/chamados/models.py:1247-1250`; `app/chamados/models.py:2644`; `app/chamados/signals.py:191-202`; `app/chamados/services/becos.py:31`)

3. **Admin encerra disputa, pai terminal e destino distinto — RED possível.** Usar estado anterior de pergunta muda acima; criar pai terminal **antes** das perguntas e apontar a pergunta para outro chamado vivo. Executar `d.admin_encerra(admin, 'decisao no teste')`, equivalente a `d.fechar(admin, resolucao='decisao no teste', motivo=d.FECHOU_ADMIN_ENCERRA)`. A porta carimba a muda antes de fechar disputa, C6 morre e destino fica reconciliável; receiver ignora pai terminal e nunca percorre destinos. Materializador posterior exclui pergunta já carimbada. RED: destino deve sair da fila, mas permanece vivo. (`app/chamados/models.py:1872-1879`; `app/chamados/models.py:1852-1865`; `app/chamados/signals.py:28-42`; `app/chamados/services/materializacao.py:1343-1344`)

**Helpers existentes para composição:** `_Base._ch(modulo, status, dia)`, `_disputa(pai)` e `_pergunta(d, destino, **campos)` permitem criar pai terminal, destino distinto e pergunta ligada à célula; uso conjunto no teste de retenção por destino e no teste administrativo de pai vivo. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:59-71`; usos `:82-87`, `:220-226`) O helper `_Base._caso(marcos,lampadas)` planta a célula e o chamado com ata acesa depois dos signals; teste A1 o usa e confirma fecho pelo evento. Ele **não** cria pergunta viva, e A5 apenas simula recusa com mock: não apresentá-lo como fixture pronta de retenção real. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:63-73`; `:97-109`; `:137-150`)

**Cuidados da montagem:** pergunta muda com célula `concorde` já está morta, logo não demonstra desbloqueio por identidade. (`app/chamados/juizes.py:1195-1196`) Executar os callbacks com `captureOnCommitCallbacks(execute=True)` evita atribuir ao produto um efeito da transação do TestCase; o callback é registrado por `transaction.on_commit`. (`app/chamados/signals.py:202`) A execução efetiva dos novos casos e o resultado observado do runner: [nao verificado].

**Campos lidos pelas asserções:**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `status_local` | `_gravar_estado`, atribuição e save. (`app/chamados/models.py:222`; `:279`) | `reconciliaria` → `_decisao_do_fechamento`; fila. (`app/chamados/models.py:623-651`; `app/chamados/services/painel.py:186-188`) |
| `materializada_em`, `materializada_por_id`, `via_resolucao` | `_escrever_carimbo`, persistido por `carimbar_identidade`. (`app/chamados/models.py:2556-2560`; `:2644`) | C6 lê instante/via; materializador lê instante. (`app/chamados/juizes.py:1170-1172`; `app/chamados/services/materializacao.py:1343-1344`) |
| `respondida_em`, `resposta_colab` | Fixture existente `_pergunta`; os atos de identidade/admin não alteram resposta. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:629-637`; `app/chamados/models.py:2640-2644`) | C6 lê antes da identidade. (`app/chamados/juizes.py:1150-1162`) |
| Âncora `chamado_destino`, `disputa.chamado`, `validada_em` | Helpers `_pergunta` e `_disputa`. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:65-71`) | `_ancoradas` e `_vivas_entre`. (`app/chamados/models.py:327-351`) |
| `contexto_json.marcos_faltantes`, `ata.lampadas` | Helper `_caso`. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:68-72`) | `_marcos_cobrados`, `_lampada`, premissa. (`app/ponto/services/lastro.py:62-94`; `app/chamados/models.py:1247-1250`) |
| `veredito`, `veredito_via` | Produção: `lavrar_veredito`; fixture pode usar padrão de `update` já existente. (`app/ponto/portas/celula.py:751`; `:812-815`; `app/chamados/tests/test_o261e_f1_porta_encerramento.py:181-186`) | C6. (`app/chamados/juizes.py:1147-1149`; `:1181-1184`) |
| `Disputa.fechada_em` | `Disputa.fechar`. (`app/chamados/models.py:1861-1865`) | Receiver do pai e classificador de beco. (`app/chamados/signals.py:28`; `app/chamados/services/becos.py:27-28`) |

## 06/11 07:44 · sessao 7bdef93e5b55 · - · - · origem=Ronald (tmux)

`validar_pergunta` com pergunta já materializada é montável; o RED exige impedir que o fechamento da disputa reconcilie o pai incidentalmente. (`app/chamados/services/validacao.py:419`, `app/chamados/services/materializacao.py:1241`, `app/chamados/signals.py:42`)

- **Helpers existentes:** `_Base._celula(lampadas, veredito='furo')`, `_Base._pergunta(motivo, cel, materializada=True)` montam célula com DNA/ata e pergunta materializada; usados conjuntamente no teste da precedência da resposta. (`app/chamados/tests/test_bug117_marco_no_espelho.py:55`, `app/chamados/tests/test_bug117_marco_no_espelho.py:64`, `app/chamados/tests/test_bug117_marco_no_espelho.py:145`)
- **Helper para pergunta respondida discordante e dia coerente:** `PerguntaRespondidaEMarcoAcesoTest._caso('19:06')` produz pergunta `orfao_14h`, luz `19:00`, veredito `concorde`; o teste confirma C6 viva aos seis minutos. Ele ainda não cria chamado/âncora. (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:36`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:59`)
- **Helpers para âncora:** `_Base._ch`, `_disputa(pai=...)`, `_pergunta(d, destino=...)` e teste que confirma recusa real de `reconciliar` por pergunta viva, sem sobrevivente. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:59`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:65`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:69`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`)

**Caso concreto:** alvo `batida_ausente` aberto, cobrando exclusivamente `S 19:00`, célula com DNA `hf=19:00`, lâmpada acesa com luz `19:00`, veredito `concorde` via cartório; pergunta `orfao_14h` ancorada no alvo, resposta `19:06`, `respondida_em` preenchido, `materializada_em` preenchido e `validada_em=None`. C6 permanece viva pela discordância antes de ler o carimbo. Acrescentar pai vivo da disputa e uma segunda pergunta viva de **outro dia**, ancorada no pai, para não reter o alvo nem oferecer sobrevivente do mesmo dia. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1162`, `app/chamados/models.py:373`)

**Ato:** `validar_pergunta(p.pk, admin, confirmar_implausivel=True)`, dentro de `captureOnCommitCallbacks(execute=True)`; esse mecanismo já é usado para testar os eventos da pergunta. (`app/chamados/services/validacao.py:343`, `app/chamados/tests/test_varredura_becos_evento_pergunta.py:55`)

**Antes:** confirmar `pergunta_viva(p)=True`, `alvo.reconciliaria()=False` e alvo na fila; ausência de sobrevivente é necessária para que não ocorra reata legítima. (`app/chamados/models.py:357`, `app/chamados/models.py:375`, `app/chamados/models.py:405`)

**Depois pela regra:** C6 morta por `validada_em`; o chamado agora é reconciliável, e deveria sair da fila pelo gate `reconciliar()`, não por escrita direta de estado. (`app/chamados/juizes.py:1136`, `app/chamados/models.py:607`, `app/chamados/models.py:623`)

**Asserção RED proposta:** após ato e callbacks, `assertFalse(qs_fila(_base=ChamadoColaborador.objects.filter(pk=alvo.pk)).exists())`, ou equivalente `assertNotIn(status_recarregado, VIVOS)`. O hash mantém o alvo aberto: carimba validação, pula `_materializar_uma` por já materializada, exclui a pergunta do lote, e a irmã impede fechar a disputa; beco tampouco atua com pai vivo. (`app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:422`, `app/chamados/services/materializacao.py:1294`, `app/chamados/services/materializacao.py:1241`, `app/chamados/services/becos.py:31`, `app/chamados/services/painel.py:186`)

**Não suprimir callbacks:** `recalcular_por_evento` agenda o cálculo mensal em `on_commit`; o corpo chama `recalcular_fechamento_mes`, sem chamada explícita a `reconciliar` ou emissão de `EventoCelula` nesse trecho. (`app/ponto/services/fechamento.py:1782`, `app/ponto/services/fechamento.py:1791`, `app/ponto/services/fechamento.py:1808`)

**Escritores/leitores principais:** validação escreve `validada_em/por` em `_carimbar_validacao`; C6 lê `validada_em`, e o coletor de âncoras filtra esse campo. O carimbo `materializada_em/por/via` é atribuído por `_escrever_carimbo`; validação/lote leem `materializada_em`. `status_local` é escrito por `_gravar_estado`, através de `reconciliar`, e lido pela fila. (`app/chamados/services/validacao.py:127`, `app/chamados/juizes.py:1136`, `app/chamados/models.py:327`, `app/chamados/models.py:2556`, `app/chamados/services/validacao.py:422`, `app/chamados/services/materializacao.py:1297`, `app/chamados/models.py:222`, `app/chamados/models.py:620`, `app/chamados/services/painel.py:187`)

Execução deste caso no hash: **[nao verificado]**.

## 06/11 07:41 · sessao 5a79ef418565 · LASTRO-PARTE-2-FILA-OPUS · leitura · origem=Code

**(1) O `or hora` aceita uma lâmpada acesa sem luz, mas os escritores atuais preservam a distinção entre batida, isenção e dispensa.** `como_acendeu()` lê somente `dispensada`, `isencao` e `luz`; sem os dois marcadores, devolve `('batida', lamp.get('luz') or hora)`. (`app/ponto/services/lastro.py:336`)

É preciso separar a **célula da grade**, que tem `missing` e `hora` real, da **lâmpada da ata**, que tem `acesa`, `hora` prevista e `luz`. O cartório transforma uma célula não missing em lâmpada acesa e copia sua `hora` para `luz`. (`app/ponto/services/cartorio.py:647`, `app/ponto/services/cartorio.py:677`, `app/ponto/services/cartorio.py:687`)

| Caminho/escritor | O que escreve e o que `como_acendeu()` devolve |
|---|---|
| Batida alinhada | `_celula_do_marco` escreve a hora do timestamp; a lâmpada copia essa hora para `luz`. Portanto não chega sem hora: devolve `('batida', hora_real)`. (`app/escala/utils.py:325`, `app/ponto/services/cartorio.py:678`, `app/ponto/services/lastro.py:340`) |
| Intermitente, intervalo explícito e reconciliação da ponta | Também escrevem hora extraída da batida. Não produzem o branco sem marcador. (`app/escala/utils.py:1569`, `app/escala/utils.py:1849`, `app/escala/utils.py:2061`) |
| Ausência parcial | Escreve `missing=False, hora=None`, **com `dispensada`**; o cartório propaga o marcador. Devolve `('dispensa', tipo)`. (`app/escala/utils.py:1266`, `app/ponto/services/cartorio.py:561`, `app/ponto/services/lastro.py:336`) |
| Ausência inteira ou justificativa aprovada | `dispensar_por_ausencia` escreve `acesa=True, luz=None`, **com `dispensada`**. Devolve `('dispensa', motivo)`. (`app/ponto/services/cartorio.py:766`, `app/ponto/services/cartorio.py:773`, `app/ponto/services/lastro.py:336`) |
| Isenção | Escreve hora planejada, `missing=False, isencao=True`; o cartório propaga `isencao`. Devolve `('isencao', None)`, mesmo se a hora planejada for `00:00`. (`app/escala/utils.py:857`, `app/ponto/services/cartorio.py:556`, `app/ponto/services/lastro.py:338`) |
| Marco do DNA sem posição na régua | Acende somente encontrando uma hora disponível por proximidade, gravando-a em `luz`; sem ela, escreve `acesa=False` ou `None`. Não produz acesa sem luz. (`app/ponto/services/cartorio.py:632`, `app/ponto/services/cartorio.py:638`, `app/ponto/services/cartorio.py:641`) |

Há **dois produtores de células de apresentação** não missing e potencialmente sem hora/marcador: o leitor de lâmpada com `acesa=None` e o leitor de ata agregada antiga. Eles não constituem caminho para essa nota: o cartório constrói a ata pelo builder, não por essa saída do leitor. (`app/escala/services/leitor_celula.py:119`, `app/escala/services/leitor_celula.py:167`, `app/ponto/services/cartorio.py:1123`)

Isso vale também para o builder legado: ele usa o mesmo alinhamento e os mesmos overlays; sua união rearranja as células. As duas lavras persistentes com `ata=` vêm de `ata_do_dia`, e a outra operação sobre a ata acrescenta `precedencia`. (`app/escala/utils.py:776`, `app/escala/utils.py:806`, `app/escala/utils.py:1037`, `app/escala/utils.py:1058`, `app/ponto/services/cartorio.py:1210`, `app/ponto/services/cartorio.py:1800`, `app/ponto/portas/celula.py:860`)

**Conclusão sobre o branco sem marcador:** o construtor aceita esse formato — inclusive há teste injetando `{'status': 'ok'}` e obtendo lâmpada acesa sem luz —, mas os escritores de produção acima não o fornecem. O teste demonstra aceitação do formato, não sua produção por um fluxo normal. (`app/ponto/tests/test_t40_ata_lampadas.py:10`, `app/ponto/tests/test_t40_ata_lampadas.py:15`, `app/ponto/services/cartorio.py:677`)

**“Acendeu as 00:00” sem batida às 00:00 tem duas hipóteses diferentes:**

- Se já houver ata com lâmpada acesa, sem marcadores, `luz=None` e marco cobrado `00:00`, o fallback devolve `('batida', '00:00')` e a nota usa essa hora. **Nesse caso, `lamp['luz']` seria `None`.** Um escritor atual produzindo esse estado, ou sua existência em produção: **[nao verificado]**. (`app/ponto/services/lastro.py:340`, `app/ponto/services/lastro.py:347`, `app/ponto/services/lastro.py:355`)
- Uma ata antiga de isenção **sem marcador**, contendo hora planejada `00:00` em `luz`, também seria anunciada como batida. **Nesse caso, `lamp['luz']` seria `'00:00'`; o `or hora` nem entraria**, porque essa string é truthy. O código mantém a leitura de atas antigas ambíguas; a existência desse registro específico no dado fornecido é **[nao verificado]**. (`app/ponto/services/cartorio.py:527`, `app/ponto/services/cartorio.py:547`, `app/ponto/services/lastro.py:336`)

**(2) `batida_ausente` não recebe a emissão explícita do nascimento nesses trechos; isso, sozinho, não prova escape.** Os pontos são `app/chamados/models.py`, não `app/ponto/models.py`: ambas as emissões exigem `MODULOS_DE_TURNO`, conjunto que não contém `batida_ausente`. (`app/chamados/models.py:938`, `app/chamados/models.py:1057`, `app/chamados/reconciliador.py:336`)

O nascimento dispara `post_save`, que chama o cartório, salvo julgamento já em curso. O cartório pode reconciliar o chamado pelo marco naquele ato; uma alteração de ata/veredito também emite evento. Já uma lavra idêntica não emite. Portanto, **ausência da emissão direta não basta para concluir que o chamado ficará aberto**. (`app/chamados/signals.py:165`, `app/ponto/services/cartorio.py:1461`, `app/ponto/services/cartorio.py:1488`, `app/ponto/portas/celula.py:676`, `app/ponto/portas/celula.py:795`)

Para delimitar os escritores: com os marcos cobrados acesos, a premissa já está regularizada. `aberto` e `em_analise` já permitem `RECONCILIAR`; trocar entre esses estados não remove a retenção. O desbloqueio relevante passa pela pergunta viva, sua âncora ou sua possibilidade de reata. (`app/chamados/models.py:1247`, `app/chamados/catalogo/motor.py:111`, `app/chamados/models.py:648`, `app/chamados/models.py:651`, `app/chamados/models.py:357`)

O inventário das portas relevantes é:

| Escritor/ato | Emite `EventoCelula` ou reconcilia no mesmo caminho? |
|---|---|
| **`_gravar_estado` — `status_local` do chamado** | Salva o estado e dispara o cartório quando ele muda. Entre `aberto`/`em_analise`, não destrava por si; encerrá-lo retira-o da fila. Não possui emissão direta própria. (`app/chamados/models.py:222`, `app/chamados/models.py:279`, `app/chamados/signals.py:168`, `app/chamados/services/painel.py:186`) |
| **`validar_pergunta` → `_carimbar_validacao`** | Grava `validada_em/por`, fazendo C6 responder morta. O carimbo não emite evento; o caminho tenta materializar, mas pula a unidade quando a pergunta já está materializada. Logo não há reconciliação garantida em todos os ramos. (`app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:422`, `app/chamados/juizes.py:1136`) |
| **Aceite de folga** | Usa o mesmo carimbo, mas chama `fechar_o_fio_do_dia` ou `declarar_dia`; o fechamento do fio chama `reconciliar()` para os chamados vivos do dia. (`app/chamados/services/validacao.py:263`, `app/chamados/services/validacao.py:270`, `app/chamados/services/validacao.py:290`, `app/chamados/services/validacao.py:291`, `app/ponto/services/veredito_celula.py:182`) |
| **`PerguntaDisputa.carimbar(..., resposta='validar')`** | É o outro escritor de validação positiva: grava materialização/via e validação quando cabível. A porta apenas salva a pergunta. Seus chamadores de hora/data-hora/escolha tentam reconciliar destino/clones; ausência decidida tenta reconciliar cada destino. (`app/chamados/models.py:2587`, `app/chamados/models.py:2609`, `app/chamados/services/materializacao.py:944`, `app/chamados/services/materializacao.py:1029`, `app/chamados/services/materializacao.py:1049`, `app/chamados/services/materializacao.py:549`, `app/chamados/services/materializacao.py:562`) |
| **`resolver_perguntas_por_chamado_resolvido`** | Também usa `resposta='validar'`, mas exige C6 **já morta** antes de carimbar. Não é um caminho que mata a pergunta viva responsável pela retenção. (`app/chamados/services/disputa_emissao.py:1585`, `app/chamados/services/disputa_emissao.py:1603`) |
| **`responder_pergunta_disputa_core`** | Escreve resposta/`respondida_em` e pode substituir discordância por concordância com a luz. Para resposta HH:MM, não emite evento nem reconcilia; os escapes de ausência têm tratamento separado. (`app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1301`, `app/chamados/services/disputa_emissao.py:1325`, `app/chamados/services/disputa_emissao.py:1359`) |
| **`reabrir_para_correcao`**, incluindo **recusar resposta** | Zera texto e carimbos por `update`; pode remover a precedência da resposta discordante. Não dispara `post_save`, não emite evento nem reconcilia nessa função. A recusa chama essa porta. (`app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1413`, `app/chamados/services/validacao.py:547`) |
| **`carimbar_identidade`** — admin, duplicata, paridade, colisão | Apenas salva materialização/via. Pode matar C6, mas uma resposta discordante ainda tem precedência. Não possui evento/reconciliação embutidos; os chamadores são admin, dedup, paridade e colisão. (`app/chamados/models.py:2644`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1170`, `app/chamados/models.py:2522`, `app/chamados/services/fio.py:891`, `app/chamados/services/materializacao.py:894`, `app/chamados/services/materializacao.py:1069`) |
| **`Disputa.fechar` por `admin_encerra`/desligamento** | Carimba as mudas antes de fechar a disputa. O signal tenta `reconciliar()` **somente no pai** e ignora pai já encerrado; não garante reconciliação de outro `chamado_destino`. `fechada_em`, sozinho, não mata a pergunta pelo C6. (`app/chamados/models.py:1852`, `app/chamados/models.py:1865`, `app/chamados/signals.py:28`, `app/chamados/signals.py:33`, `app/chamados/juizes.py:1135`) |
| **Decisão administrativa na célula** | Remove a precedência da resposta discordante pelo veredito/via admin. A escrita de veredito/ata emite evento, e o fechamento do fio também tenta reconciliar os chamados do dia. (`app/chamados/juizes.py:1147`, `app/ponto/portas/celula.py:815`, `app/ponto/portas/celula.py:680`, `app/ponto/services/veredito_celula.py:183`) |
| **Trancar competência** | C6 deixa de considerar a pergunta viva por leitura da tranca. A porta dispara encerramento da fila no ato ou em processo separado; não depende de `EventoCelula`. (`app/chamados/juizes.py:1140`, `app/ponto/services/fechamento.py:1404`, `app/chamados/services/competencia_trancada.py:184`, `app/chamados/services/competencia_trancada.py:276`) |

Os carimbadores auxiliares de célula/turno/fecho não acrescentam outro escritor positivo de `validada_em`: alguns exigem C6 já morta; os executados pelo consumidor são seguidos de `fechar_pelo_marco_colab` no mesmo consumo. `materializar_pendentes_aprovadas` e `encerrar_fantasma` usam a mesma porta de carimbo, sem emissão própria. (`app/chamados/reconciliador.py:397`, `app/chamados/reconciliador.py:586`, `app/chamados/services/disputa_emissao.py:1663`, `app/chamados/services/materializacao.py:1349`, `app/chamados/models.py:2685`, `app/chamados/reconciliador.py:1370`)

**Cenário concreto de N>0, sem depender de falha:**

1. **Antes:** chamado na fila, marco cobrado aceso, célula sem acusação, pergunta ancorada respondida com hora discordante e não validada, sem outra cobrança apta a receber reata. A resposta mantém C6 viva e o gate retém o chamado. O consumo anterior pode terminar normalmente e marcar o evento processado apesar dessa recusa legítima. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1162`, `app/chamados/models.py:375`, `app/chamados/models.py:405`, `app/chamados/reconciliador.py:1375`)
2. **Ato A — colaborador corrige para a hora da luz:** a porta aceita a pergunta ainda viva e salva a nova resposta. C6 passa a morta por concordância, sem novo evento nem reconciliação para HH:MM. Com pai da disputa vivo e chamado sem silêncio, nem o beco nem o resgate alteram esse resultado. (`app/chamados/views.py:1242`, `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/juizes.py:1099`, `app/chamados/juizes.py:1132`, `app/chamados/services/becos.py:31`, `app/chamados/services/materializacao.py:687`)
3. **Ato B — alternativamente, admin recusa:** salva o motivo enquanto a resposta ainda discorda; depois apaga resposta/carimbos por `update`. Sem resposta discordante e com célula coerente, C6 passa a morta; não há signal posterior desse `update` para fechar o chamado. (`app/chamados/services/validacao.py:545`, `app/chamados/services/validacao.py:547`, `app/chamados/services/disputa_emissao.py:1413`, `app/chamados/juizes.py:1195`)
4. **Depois:** o chamado continua vivo, agora com `reconciliaria()=True`. Sem evento pendente do colaborador, entra em `com_lastro_sem_evento`; **N≥1**, e `--alarme` sai 2. (`app/ponto/services/lastro.py:316`, `app/ponto/services/lastro.py:421`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:77`)

Há também escape pelo **encerramento administrativo da disputa**: pai já terminal, destino diferente ainda retido, pergunta muda sem materialização e célula acusando outro marco. O carimbo `admin_decidiu` mata a pergunta; o receiver ignora o pai encerrado, e a materialização posterior pula a pergunta já carimbada. Sem evento pendente, o destino fica contável pelo vigia. (`app/chamados/juizes.py:1177`, `app/chamados/models.py:1852`, `app/chamados/signals.py:33`, `app/chamados/services/materializacao.py:1343`, `app/ponto/services/lastro.py:421`)

A validação também tem ramo sem garantia de fechamento: pergunta já materializada, mas viva pela resposta discordante; `validar_pergunta` grava a validação e pula sua materialização. Se outra pergunta mantém a disputa aberta e o pai segue vivo, nem a cauda nem o beco reconciliam esse destino. Recalcular o fechamento mensal não equivale a emitir `EventoCelula`. (`app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:422`, `app/chamados/services/materializacao.py:1294`, `app/chamados/services/materializacao.py:1241`, `app/chamados/services/becos.py:31`, `app/ponto/services/fechamento.py:1791`)

**Esses são caminhos demonstrados pelo código; ocorrência desses estados e valor real de N em produção: [nao verificado].**

## 06/11 07:40 · sessao e416ea4f87a3 · - · - · origem=Ronald (tmux)

Escritores que podem liberar a guarda da pergunta viva:

| Escritor | Campos/efeito | EventoCelula ou reconciliar no mesmo caminho |
|---|---|---|
| `validar_pergunta` → `_carimbar_validacao` | Grava `validada_em/por`; C6 devolve `False` imediatamente. | Carimbo não emite evento. Materializador normalmente reconcilia destino/clones; pergunta já materializada é pulada, portanto não há garantia geral. `app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:422`, `app/chamados/juizes.py:1136`, `app/chamados/services/materializacao.py:1294`. |
| Aceite de folga | Mesmo carimbo, nas duas ramificações. | Chama `fechar_o_fio_do_dia`/`declarar_dia`; o fechamento do fio chama `reconciliar()` para os chamados vivos daquele dia. `app/chamados/services/validacao.py:263`, `app/chamados/services/validacao.py:270`, `app/chamados/services/validacao.py:290`, `app/chamados/services/validacao.py:291`, `app/ponto/services/veredito_celula.py:182`. |
| `PerguntaDisputa.carimbar(..., resposta='validar')` | Escreve materialização/via e, havendo texto, validação nula. | A porta só salva a pergunta, sem outbox/reconcile próprio. Chamadores abaixo. `app/chamados/models.py:2556`, `app/chamados/models.py:2587`, `app/chamados/models.py:2609`. |
| `resolver_perguntas_por_chamado_resolvido` | Chama `carimbar(..., resposta='validar')`. | Exige pergunta **já morta** antes do carimbo; portanto não transforma uma pergunta viva em morta por esse carimbo. Pode fechar disputa; o signal reconcilia o pai. `app/chamados/services/disputa_emissao.py:1585`, `app/chamados/services/disputa_emissao.py:1603`, `app/chamados/services/disputa_emissao.py:1695`, `app/chamados/signals.py:42`. |
| `_materializar_ausencia_de_resposta` | Chama `carimbar(..., resposta='validar')` nas irmãs. | Reconcilia cada destino se ausência decidida; ausência aberta mantém cobrança. `app/chamados/services/materializacao.py:549`, `app/chamados/services/materializacao.py:562`. |
| `_materializar_uma` | Chama `carimbar(..., resposta='validar')`. | Ramos hora/data_hora/escolha chamam `_reconciliar_destino_e_clones`; helper consulta irmãs e chama `alvo.reconciliar()`. `app/chamados/services/materializacao.py:1202`, `app/chamados/services/materializacao.py:944`, `app/chamados/services/materializacao.py:1029`, `app/chamados/services/materializacao.py:1049`, `app/chamados/services/materializacao.py:731`. |
| `responder_pergunta_disputa_core` | Troca resposta/carimbo de resposta; remove validação anterior; pode trocar discordância por concordância com a luz. | Para HH:MM, não emite outbox nem reconcilia; a exceção explícita são escapes que criam ausência. `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1301`, `app/chamados/services/disputa_emissao.py:1325`, `app/chamados/services/disputa_emissao.py:1330`, `app/chamados/services/disputa_emissao.py:1359`. |
| `reabrir_para_correcao` | Zera texto, `respondida_em`, `validada_em/por` via `QuerySet.update`; pode remover a precedência da resposta discordante. | Sem `post_save`, sem evento/reconcile nessa função. Chamadores incluem recusa, reabertura do questionário por admin e colaborador. `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1413`, `app/chamados/services/validacao.py:547`, `app/chamados/services/acoes_disputa.py:103`, `app/chamados/views.py:1436`, `app/chamados/models.py:2146`. |
| `carimbar_identidade` | Grava materializada/via; identidade mata C6 depois da precedência da resposta. | Porta não emite evento/reconcile. Chamadores: admin, duplicata, paridade e colisão. `app/chamados/models.py:2633`, `app/chamados/models.py:2644`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1170`, `app/chamados/models.py:2522`, `app/chamados/services/fio.py:891`, `app/chamados/services/materializacao.py:894`, `app/chamados/services/materializacao.py:1069`. |
| `fechar_por_admin` pelas exceções da disputa | Mata perguntas mudas antes de gravar disputa fechada. | Signal da disputa tenta reconciliar **somente o pai**, e ignora pai já encerrado. Destino diferente não é garantidamente reconciliado. `app/chamados/models.py:1852`, `app/chamados/models.py:1865`, `app/chamados/signals.py:28`, `app/chamados/signals.py:33`, `app/chamados/signals.py:42`. |
| Dedup de perguntas | `retratar_duplicata` chama identidade. | Command `--apply` não reconcilia nem emite outbox; migração de resolvedoras também não garante isso. `app/chamados/services/fio.py:891`, `app/chamados/management/commands/dedup_perguntas_fato.py:86`, `app/chamados/services/fio.py:943`, `app/chamados/services/fio.py:960`. |

Demais carimbadores sem validação: `VIA_SEM_TURNO_ABERTO`, `VIA_CELULA_ABSOLVEU`, fecho sem credor, `VIA_JUIZ_APROVOU`, fantasma. Os dois primeiros e o fecho sem credor exigem pergunta já morta; os executados pelo consumidor são seguidos de `fechar_pelo_marco_colab` no mesmo consumo. `app/chamados/reconciliador.py:397`, `app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:586`, `app/chamados/services/disputa_emissao.py:1663`, `app/chamados/services/disputa_emissao.py:1675`, `app/chamados/services/materializacao.py:1349`, `app/chamados/models.py:2685`, `app/chamados/reconciliador.py:1353`, `app/chamados/reconciliador.py:1370`.

Cenário mais simples, sem depender de pergunta materializada: chamado na fila, marco cobrado aceso, célula coerente, pergunta respondida com hora discordante e não validada. A resposta discordante mantém C6 viva e retém o fechamento, mesmo com luz. O colaborador corrige para a hora da luz; a porta permite porque a pergunta estava viva, o core salva a resposta e C6 passa a morta por concordância. O core não reconcilia nem emite EventoCelula para HH:MM. Com pai da disputa vivo, o receiver da pergunta só chama o beco, que exige pai terminal e não age. Sem outro evento pendente, `com_lastro_sem_evento` conta esse chamado: N≥1. `app/chamados/juizes.py:1099`, `app/chamados/juizes.py:1132`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1162`, `app/chamados/views.py:1242`, `app/api/views.py:2631`, `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/signals.py:191`, `app/chamados/services/becos.py:29`, `app/ponto/services/lastro.py:420`.

Outra execução concreta do mesmo estado: admin recusa a resposta. O primeiro save de `recusa_motivo` ainda vê resposta discordante; depois `reabrir_para_correcao` apaga resposta/carimbos por update sem signal. Com célula coerente e marco aceso, C6 agora devolve falsa, chamado continua vivo e sem evento pendente, produzindo N≥1. `app/chamados/services/validacao.py:545`, `app/chamados/services/validacao.py:547`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1413`, `app/chamados/juizes.py:1195`, `app/ponto/services/lastro.py:422`.

Recalcular fechamento não equivale a EventoCelula: o callback chama `recalcular_fechamento_mes`; outbox nasce por `emitir_evento_celula`, normalmente condicionado a campos relevantes da lavra. Lavra idêntica não salva nem emite. `app/ponto/services/fechamento.py:1791`, `app/ponto/services/fechamento.py:1808`, `app/ponto/portas/celula.py:673`, `app/ponto/portas/celula.py:680`, `app/ponto/portas/celula.py:795`.

Ocorrência desses estados em produção: [nao verificado].

## 06/11 07:39 · sessao 4295aa63f985 · - · - · origem=Ronald (tmux)

Delimitação pelos leitores: com `marcos_acesos(...) is True`, a premissa já está regularizada; `Justificativa.status` e `Ausencia.status` não precisam mudar para liberar esse retido. (`app/chamados/models.py:1247–1250`) A decisão restante passa pelo motor de `status_local` e pelo C6/âncora/reata. (`app/chamados/models.py:637–655`, `app/chamados/models.py:327–378`)

| Campo/ato adicional | Efeito e reação |
|---|---|
| `status_local` do próprio retido | `aberto` e `em_analise` já permitem `RECONCILIAR`; mudar entre eles não remove a recusa. Terminal sai da fila. (`app/chamados/catalogo/motor.py:111`, `app/chamados/services/painel.py:186–188`) |
| `CelulaDia.veredito/veredito_via` por decisão admin | Remove a precedência da resposta discordante no C6; a porta grava veredito/ata com emissão atômica de `EventoCelula`. (`app/chamados/juizes.py:1147–1162`, `app/ponto/portas/celula.py:809–817`, `app/ponto/portas/celula.py:676–684`) |
| Fechar `DisputaSupervisao` | `fechada_em` não é lido por `_pergunta_viva_calc`; fechar a disputa sozinho não mata pergunta. (`app/chamados/juizes.py:1135–1218`) |
| `Disputa.admin_encerra` / fechamento por desligamento | Carimba perguntas mudas com `admin_decidiu` antes de salvar a disputa. O `post_save` tenta `reconciliar()` somente no chamado-pai. (`app/chamados/models.py:1852–1865`, `app/chamados/signals.py:28–45`) |

**Outro cenário concreto de escape sem falha:** pai da disputa já terminal; pergunta muda cujo `chamado_destino` é outro chamado `batida_ausente` vivo; marco cobrado aceso, mas célula acusa outro furo; pergunta ainda sem `materializada_em`, portanto continua viva pelo C6 e segura o destino. Essa disputa pode continuar aberta, pois pergunta muda com destino vivo impede a classificação como beco. (`app/chamados/juizes.py:1177–1184`, `app/chamados/models.py:327–328`, `app/chamados/services/becos.py:25–33`)

O admin usa `admin_encerra`: a muda recebe carimbo de identidade, passa a morta, e o destino fica fechável. O receiver ignora o pai já encerrado; não reconcilia o destino nem emite evento. (`app/chamados/models.py:1852–1865`, `app/chamados/models.py:2508–2523`, `app/chamados/juizes.py:1170–1172`, `app/chamados/signals.py:28–34`) Os demais receivers não desfazem esse cenário: sem regularização externa confirmada não há retomada; a materialização filtra apenas perguntas sem carimbo; o beco ignora disputa já fechada. (`app/chamados/signals.py:97–121`, `app/chamados/services/materializacao.py:1343–1350`, `app/chamados/services/becos.py:27–28`) Sem evento pendente, o destino entra em N do vigia. (`app/ponto/services/lastro.py:421–423`) Ocorrência em produção: [nao verificado].

## 06/11 07:39 · sessao 26487d3ddb3e · - · - · origem=Ronald (tmux)

Completude confirmada por escrita:

- Os únicos caminhos persistentes que fornecem `ata=` à lavra são o ramo nunca bateu e o ramo normal do cartório, ambos construindo por `ata_do_dia`: `app/ponto/services/cartorio.py:1210`, `app/ponto/services/cartorio.py:1242`, `app/ponto/services/cartorio.py:1800`, `app/ponto/services/cartorio.py:1802`. A porta copia a ata recebida e preserva apenas `precedencia` da antiga: `app/ponto/portas/celula.py:768`.
- A outra atribuição persistente a `cel.ata` só acrescenta `precedencia`: `app/ponto/portas/celula.py:860`. As atribuições em `celula_como_o_julgamento_deixa` e no builder são em cópias em memória: `app/ponto/portas/celula.py:719`, `app/escala/utils.py:1907`.
- Não há caminho do leitor com `missing=False,hora=None` para essas lavras: as células agregadas são fabricadas para a saída do leitor, em `app/escala/services/leitor_celula.py:167`, enquanto o cartório usa o builder, em `app/ponto/services/cartorio.py:1126`.
- A migração da ata só adiciona um JSONField nulo; não escreve lâmpadas ou remove marcadores: `app/escala/migrations/0037_hx_ata_celula.py:12`.
- `_carimbar_isencao` propaga os marcadores truthy e não possui ramo que os apague: `app/ponto/services/cartorio.py:556`, `app/ponto/services/cartorio.py:561`.

Nuance do ramo por proximidade: `_reais` inclui horas sintéticas de isenção, mas as horas dos marcos DNA que têm posição são previamente reservadas em `_usadas`, e o ramo sem posição exclui esses minutos: `app/ponto/services/cartorio.py:593`, `app/ponto/services/cartorio.py:615`, `app/ponto/services/cartorio.py:632`. Na montagem coerente da mesma foto DNA, isso não fornece um segundo marco sem marcador com a hora sintética de um marco casado; concorrência ou dados inconsistentes que produzam outra foto são **[nao verificado]**.

Assim, sustento: **nenhum escritor de produção atual rastreado produz o caso vazio e sem marcador; legado ambíguo pode ser consumido, mas sua presença no dado fornecido é [nao verificado]**. O retorno para legado vazio seria hora cobrada por fallback; para legado de isenção com `luz='00:00'`, o retorno usaria a própria string, sem fallback: `app/ponto/services/lastro.py:340`.

## 06/11 07:37 · sessao 26487d3ddb3e · - · - · origem=Ronald (tmux)

Na cópia, **não encontrei caminho de produção que escreva uma célula de grade não missing, sem hora e sem `isencao`/`dispensada`, e a lave como lâmpada acesa**. O construtor aceita esse formato, mas os escritores atuais não o produzem: `app/ponto/services/cartorio.py:647`, `app/ponto/services/cartorio.py:677`, `app/ponto/services/cartorio.py:687`.

- `como_acendeu` não lê `missing`, `acesa`, `pendente`, `tipo_real` nem a hora prevista da própria lâmpada. Lê somente `dispensada`, `isencao` e `luz`; sem marcadores, retorna `('batida', lamp.get('luz') or hora)`: `app/ponto/services/lastro.py:336`.
- Os dois chamadores efetivos só chegam à função com lâmpada acesa: `julgar` exige `lamp.get('acesa')`, em `app/ponto/services/lastro.py:289`; o fechador exige `marcos_acesos(...) is True`, em `app/ponto/services/lastro.py:394`, cujo teste recusa qualquer `acesa` falsy, em `app/ponto/services/lastro.py:179`. Portanto `acesa=None` e `pendente=True` não chegam à nota por esse caminho: `app/ponto/services/cartorio.py:869`, `app/ponto/services/lastro.py:394`.

Prova pelos escritores:

| Caminho | Escrita e resultado |
|---|---|
| Batida alinhada normal | `_match_marcos` escreve missing ou chama `_celula_do_marco`, que sempre escreve `hora=instante.strftime('%H:%M')`: `app/escala/utils.py:618`, `app/escala/utils.py:659`, `app/escala/utils.py:325`. A lâmpada copia essa hora para `luz`: `app/ponto/services/cartorio.py:677`. Retorno: `('batida', hora_real)`: `app/ponto/services/lastro.py:340`. |
| Batida intermitente / intervalo explícito / reconciliação de ponta | Hora vem igualmente do timestamp: `app/escala/utils.py:1569`, `app/escala/utils.py:1849`, `app/escala/utils.py:2061`. O modelo intermitente `00:00–23:59` fica fora da régua por hora: `app/escala/utils.py:1377`. |
| Dispensa parcial | A grade escreve `missing=False,hora=None,dispensada=_slug`: `app/escala/utils.py:1266`. Cartório copia `dispensada`: `app/ponto/services/cartorio.py:561`. Retorno: `('dispensa', slug)`: `app/ponto/services/lastro.py:336`. |
| Ausência inteira / justificativa | A grade remove células por `None` ou remove só missing: `app/escala/utils.py:1272`, `app/escala/utils.py:1275`. O cartório acende as lâmpadas sem luz **com** `dispensada`: `app/ponto/services/cartorio.py:766`, `app/ponto/services/cartorio.py:773`. Retorno: `('dispensa', motivo)`: `app/ponto/services/lastro.py:336`. |
| Isenção | A grade escreve hora planejada, `missing=False,isencao=True`: `app/escala/utils.py:857`. O cartório copia o marcador: `app/ponto/services/cartorio.py:556`. Retorno: `('isencao', None)`: `app/ponto/services/lastro.py:338`. |
| Lâmpada do DNA sem posição na régua | Se acende por proximidade, escreve uma hora real formatada em `luz`; sem hora real, fica False ou None: `app/ponto/services/cartorio.py:632`. Não produz acesa True sem luz. |
| Ata agregada antiga lida | O leitor produz células `missing=False,hora=None` sem marcador para a parte não missing: `app/escala/services/leitor_celula.py:167`. Isso é saída do leitor, não lâmpada gravada: seu ramo agregador devolve `marcos=[]`, em `app/escala/services/leitor_celula.py:171`. O cartório remonta a grade pelas batidas, em `app/ponto/services/cartorio.py:1123`; dispatcher em `app/escala/utils.py:2105`. |
| Ata com `acesa=None` lida | O leitor traduz para `missing=False`, com hora recebida de `luz` possivelmente None: `app/escala/services/leitor_celula.py:119`. Esse dict também não é fonte da lavratura atual: `app/ponto/services/cartorio.py:1126`. |
| Builder legacy ativado pela flag | Chama o single, que usa o mesmo alinhamento e overlays: `app/escala/utils.py:1037`, `app/escala/utils.py:776`, `app/escala/utils.py:806`. A união apenas rearranja dicts: `app/escala/utils.py:1058`. Logo não introduz o branco sem marcador. |

Há teste que **injeta** `{'status': 'ok'}` sem hora e confirma que `ata_do_dia` gera `acesa=True,luz=None`: `app/ponto/tests/test_t40_ata_lampadas.py:10`, `app/ponto/tests/test_t40_ata_lampadas.py:15`. Isso demonstra que o construtor aceita o formato, não um escritor de produção que o fornece: `app/ponto/services/cartorio.py:647`, `app/ponto/services/cartorio.py:677`.

Sobre **“acendeu as 00:00” sem batida**:

- Uma lâmpada acesa sem marcador e com `luz=None`, se já estivesse gravada e cobrasse `hora='00:00'`, produziria `('batida','00:00')` pelo fallback e a nota afirmativa: `app/ponto/services/lastro.py:340`, `app/ponto/services/lastro.py:347`, `app/ponto/services/lastro.py:355`. Que algum escritor real desta cópia produza esse estado: **[nao verificado]**; os escritores rastreados acima não produzem.
- Uma isenção **legada sem marcador**, com hora planejada `00:00` em `luz`, também seria lida como batida; nesse caso **`lamp['luz']=='00:00'`**, e o `or hora` nem seria acionado: `app/ponto/services/lastro.py:336`, `app/ponto/services/lastro.py:340`. O próprio código documenta atas antigas ambíguas e decisão de não inventar o marcador retrospectivo: `app/ponto/services/cartorio.py:547`, `app/escala/services/leitor_celula.py:134`. Existir tal registro no dado fornecido, ou ele ter hora `00:00`: **[nao verificado]**.
- A isenção **escrita hoje** recebe marcador e devolve `isencao`, portanto não usa a frase de batida: `app/ponto/services/cartorio.py:556`, `app/ponto/services/lastro.py:338`, `app/ponto/services/lastro.py:352`.

## 06/11 07:37 · sessao 4295aa63f985 · - · - · origem=Ronald (tmux)

Os pontos 938–941 e 1057–1060 são de **`app/chamados/models.py`**. Eles não emitem diretamente `EventoCelula` para `batida_ausente`: ambos exigem módulo em `MODULOS_DE_TURNO`; a tupla contém somente `turno_aberto`, `turno_aberto_24h` e `entrada_bloqueada_turno_aberto`. (`app/chamados/models.py:938`, `app/chamados/models.py:1054`, `app/chamados/reconciliador.py:336`)

Isso não prova nascimento sem qualquer evento indireto: o `post_save` do chamado julga a célula no nascimento ou mudança de status, salvo julgamento já em curso. (`app/chamados/signals.py:164–180`) O julgamento pode reconciliar os marcos já satisfeitos no próprio ato. (`app/ponto/services/cartorio.py:1461–1495`) Só lavra com alteração de `veredito`/`ata` acorda o consumidor; lavra idêntica retorna sem save/evento. (`app/ponto/portas/celula.py:662–684`, `app/ponto/portas/celula.py:795–802`) Portanto não usar “nasceu sem emissão direta” como prova suficiente de escape.

Critérios relevantes:

- `julgar()` considera apenas fila de cobrança do módulo `batida_ausente`; exige marcos e tipos conferíveis. (`app/ponto/services/lastro.py:239–241`, `app/ponto/services/lastro.py:273–282`)
- Aceita lâmpada acesa ou, na ausência dela, batida casada por tipo e tolerância de horário; depois só põe em `com_lastro` se `reconciliaria()` permitir fechamento. (`app/ponto/services/lastro.py:287–319`)
- O fechador do evento exige célula gravada e `marcos_acesos(...) is True`; escreve a nota somente após `reconciliar()` retornar verdadeiro, no mesmo `atomic`. (`app/ponto/services/lastro.py:393–402`)
- `reconciliaria()` e `reconciliar()` consultam a mesma `_decisao_do_fechamento`; ela consulta motor, guarda da premissa e guarda da pergunta viva. (`app/chamados/models.py:607–659`)
- A guarda da pergunta viva recusa quando há viva ancorada sem plano de reata para outra cobrança; sem viva, deixa fechar. (`app/chamados/models.py:393–428`)
- O consumidor chama o fechador e, sem falha/adiamento, marca os eventos como processados mesmo quando o fechamento foi legitimamente recusado. (`app/chamados/reconciliador.py:1367–1385`)

**N do vigia:** de `com_lastro`, conta chamado cujo colaborador não tenha evento pendente **ou** cujos marcos na célula não estejam todos acesos. Assim, lastro obtido pelo recuo de batidas, sem lâmpada, conta mesmo havendo evento pendente. (`app/ponto/services/lastro.py:420–423`) Evento “pendente” significa `processado_em IS NULL` e tentativas abaixo do teto. (`app/chamados/reconciliador.py:1235–1238`) Com `--alarme` e escapes, o comando lista e sai com código 2. (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:77–83`)

Há testes explícitos para lâmpada acesa sem evento → `sem_evento=1`/saída 2; com evento → zero; recuo sem lâmpada mesmo com evento → um. São fixtures que plantam a ata/deletam eventos, portanto não demonstram sozinhas um caminho normal de produção. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:63–83`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:214–260`)

## 06/11 05:22 · sessao c611628a56e6 · OTIMIZA-3 · censo · origem=Code

**BLOQUEIO 1: classe (e) — NÃO PERGUNTADO.** Nenhuma das três perguntas trouxe loader/import dinâmico ou leitura de símbolo por atributo de módulo carregado. Referência: anexos q1–q3 fornecidos; arquivo:linha na cópia [nao verificado].

| achado | classe | trecho pergunta | trecho resposta | trecho triagem |
|---|---|---|---|---|
| Loader de string deixa a faceta símbolo-de-topo imprimir zero falso. O caso por caminho existe na cópia: carregamento e chamada em `bin/gerar_estado.py:142` e `bin/gerar_estado.py:145`; resolução do atributo depende de `apelidos` em `bin/quem_chama.py:473`. | **(e) NÃO PERGUNTADO** — pelas perguntas fornecidas [nao verificado]. | q1/P1: “Existe forma de Python embutido em .sh, config ou string de .py que IMPORTA um modulo-alvo e que o novo `_rx_embutido` nao casa”. q1/P7: “Qualquer outro defeito que voce veja no diff”. q2/P6 e q3/P5: “Qualquer outro defeito no delta”. Nenhum desses trechos explicita o mecanismo de loader. [nao verificado] | r1/6: “há escapes reais da regex, mas não demonstrei uma faceta com zero falso causada por eles”. r2/6: “nenhum dos antigos achados 5, 6, 8 e 9 ganhou falso zero demonstrado com o delta”. Tratam dos escapes anteriores; nenhuma das três respostas relata o bloqueio de loader. [nao verificado] | v5/6: “regex embutida nao resolve `from .ci import *` dentro de heredoc nem `from pacote.sub import` contra o __init__ do pacote”; motivo escrito: “falso 0 real nao demonstrado”. **Esse encaminhamento para FILA trata de outro mecanismo**, não de um loader identificado pelo Codex e descartado. [nao verificado] |

**O pedido genérico de procurar zero falso não conta, sozinho, como tema PRESENTE nesta classificação.** Caso contrário, “qualquer outro defeito” tornaria impossível a classe (e). Aqui, P1 delimita Python embutido e `_rx_embutido`; P2 pergunta por “faceta de um modulo rebaixado”, pressupondo rebaixamento. Nenhum deles pergunta pelo vínculo criado por loader que não foi reconhecido. Referência: q1/P1–P2/P7 fornecidos [nao verificado].

A distinção tem fundamento no código: os vínculos de `apelidos` vêm de `ast.Import`/`ast.ImportFrom`; o carregamento real usa `spec_from_file_location` seguido de `mod.placar()`. (`bin/quem_chama.py:448`, `bin/quem_chama.py:454`, `bin/gerar_estado.py:142`, `bin/gerar_estado.py:145`)

## 06/11 05:22 · sessao 78075a70af27 · LASTRO-PARTE-2 · verificador · origem=Code

**D1. O `continue` não provoca rollback, mas pode confirmar efeitos do gate.** Quando `reconciliar()` retorna `False`, a nota e o incremento de `n` são pulados; a saída normal do `with` preserva eventuais escritas anteriores, inclusive a trilha `fechamento_recusado`. Portanto, “sem efeito colateral” seria incorreto. (`app/ponto/services/lastro.py:399`; `app/chamados/models.py:617`; `app/chamados/models.py:407`; `app/chamados/models.py:391`)

O `atomic(savepoint=False)` continua envolvendo a decisão e a gravação; agora participa da transação externa até a nota. O `select_for_update()` já era usado porque a leitura ocorria dentro daquele atomic; a mudança prolonga a retenção da trava até terminar o bloco novo. (`app/chamados/models.py:616`; `app/chamados/models.py:673`; `app/ponto/services/lastro.py:402`)

A premissa de que os signals **passaram agora** a rodar em transação está errada: `_gravar_estado()` já chamava `save()` dentro de outro atomic, ainda sob o atomic de `reconciliar()`. O delta amplia a unidade transacional para incluir a nota. (`app/chamados/models.py:274`; `app/chamados/models.py:279`; `app/chamados/models.py:616`; `app/ponto/services/lastro.py:399`)

Há efeitos relevantes nessa ampliação:

| Efeito | Consequência do bloco novo |
|---|---|
| `on_commit` do filho que acorda o pai; fechamento de becos por pergunta; retomada de regularizações por disputa | Quando acionados pela cascata, aguardam também a nota; rollback anterior ao commit descarta os callbacks. (`app/chamados/signals.py:314`; `app/chamados/signals.py:202`; `app/chamados/signals.py:121`; `app/ponto/services/lastro.py:402`) |
| Lavra da célula e `EventoCelula` | São escritas síncronas pelo ORM, na transação compartilhada; uma falha posterior da nota também pode desfazê-las. (`app/ponto/services/cartorio.py:1802`; `app/ponto/services/cartorio.py:1822`; `app/ponto/portas/celula.py:681`; `app/ponto/portas/celula.py:673`) |
| Push da reemissão | **Escapa ao rollback:** o cartório pode chamar o emissor, que envia push síncrono à supervisão antes da nota. Se ela falhar, as escritas podem voltar depois de o aviso já ter saído; o retry pode reenviá-lo. (`app/ponto/services/cartorio.py:1778`; `app/ponto/management/commands/detectar_ausencias.py:778`; `app/chamados/push_utils.py:40`; `app/ponto/services/lastro.py:402`) |

No caminho de persistência rastreado, as operações usam o ORM sem conexão de banco separada declarada; o envio de push é uma chamada externa imediata. Configuração e entrega efetiva em produção: **[nao verificado]**. (`app/ponto/portas/celula.py:673`; `app/chamados/services/nota.py:103`; `app/chamados/push_utils.py:40`)

**D2. Exceção do cartório não passou genericamente a desfazer o fecho.** O receiver captura `Exception` por `engolir()`, que registra e não relança. Uma exceção Python capturada ali não chega automaticamente à lista `falhas` do lastro. (`app/chamados/signals.py:172`; `app/chamados/signals.py:180`; `app/core/observ.py:85`)

`engolir()` sozinho não cria savepoint; entretanto, o cartório já executava dentro dos atomics antigos. Logo, eventual transação comprometida por erro SQL engolido não deve ser atribuída como novidade deste delta. A gravação célula/evento tem seu próprio savepoint. Comportamento efetivamente observado desse erro de banco: **[nao verificado]**. (`app/chamados/signals.py:172`; `app/chamados/models.py:274`; `app/chamados/models.py:616`; `app/ponto/portas/celula.py:681`)

A mudança intencional é a **falha da nota posterior ao gate**: agora ela desfaz fecho e escritas transacionais da cascata; o handler registra a falha, e o consumidor incrementa as tentativas dos eventos daquele lote do colaborador. O teste A6 descreve uma falha e posterior recuperação, mas sua execução nesta verificação é **[nao verificado]**. (`app/ponto/services/lastro.py:399`; `app/ponto/services/lastro.py:404`; `app/chamados/reconciliador.py:1377`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:163`)

Quem escreve e quem lê:

| Objeto | Escritor e leitor |
|---|---|
| Estado do chamado | `reconciliar()` → `_gravar_estado()`; o lastro seleciona os chamados pela fila de cobrança. (`app/chamados/models.py:620`; `app/ponto/services/lastro.py:387`; `app/chamados/services/painel.py:187`) |
| Nota | `anexar_ocorrencia()` cria `FollowupChamado`, agora antes do commit do bloco externo. (`app/chamados/services/nota.py:103`; `app/ponto/services/lastro.py:402`) |
| Tentativas/erro/processamento | O consumidor atualiza o lote após o trabalho; `eventos_pendentes()` exclui os esgotados e `eventos_mortos()` os conta. (`app/chamados/reconciliador.py:1377`; `app/chamados/reconciliador.py:1238`; `app/chamados/reconciliador.py:1244`) |

**Pode ficar parado indefinidamente, sob condições.** Depois de cinco falhas, aquele evento sai do consumo automático. Outro evento do mesmo colaborador permite nova passagem pelo lastro, mas, sem ele, não existe retry daquele evento morto nesse laço; o comando antigo agora apenas alarma. Isso não prova um chamado preso em produção. (`app/chamados/reconciliador.py:1198`; `app/chamados/reconciliador.py:1238`; `app/chamados/reconciliador.py:1371`; `app/ponto/management/commands/fechar_cobranca_com_lastro.py:77`)

Quem vê o morto é o comando consumidor, que imprime a contagem, e `reconciliar_chamados --alarme`, que soma mortos ao alarme e sai com código 2. O registro declara esses comandos a cada minuto e a cada quinze minutos, respectivamente; instalação real, destinatário humano e recebimento do alarme: **[nao verificado]**. (`app/chamados/management/commands/consumir_eventos_celula.py:37`; `app/chamados/management/commands/reconciliar_chamados.py:118`; `app/config/crons.py:394`)

Também não contaria com `--semear` como recuperação geral de `batida_ausente`: seu primeiro universo contém somente os módulos de turno enumerados; o outro ramo trata perguntas órfãs. (`app/chamados/reconciliador.py:1278`; `app/chamados/reconciliador.py:1193`; `app/chamados/reconciliador.py:336`; `app/chamados/reconciliador.py:1291`)

**D3. Os descartes têm sustentação funcional, com uma correção na justificativa de cobertura de V10.**

| Item | Conferência |
|---|---|
| **V4** | Sustentado para a cascata descrita: o save aciona o cartório, que pode reemitir pelos marcos apagados; estes impedem novo fechamento pelo lastro. A equivalência histórica com o comando antigo: **[nao verificado]**. (`app/chamados/signals.py:180`; `app/ponto/services/cartorio.py:1768`; `app/ponto/services/cartorio.py:1778`; `app/ponto/services/lastro.py:179`) |
| **V5** | Coerente como descarte por escopo, não como ausência de lacuna: a regeneração muda DNA sem substituir a ata e `recalculo=False` pula o julgamento. Só ata/veredito acionam a outbox nessa porta. O signal de isenção agenda silêncio dos chamados. (`app/ponto/portas/celula.py:270`; `app/ponto/portas/celula.py:390`; `app/ponto/portas/celula.py:663`; `app/chamados/signals.py:224`) |
| **V6** | Sustentado: retidos saem em `(a2)` e entram no TOTAL do placar. O aval está documentado na cópia. (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:61`; `bin/placar_code.sh:121`; `bin/placar_code.sh:549`; `app/docs/PROMPTS.md:1859`) |
| **V7** | O defeito do produtor existe: `_reais` admite células sintéticas de isenção; o ramo `j is None` acende por proximidade sem copiar o marcador. Sem marcador, a nota pode dizer `EXISTE`. Portanto, descarte por origem/escopo não elimina o defeito. Medição de sombra e número publicado: **[nao verificado]**. (`app/escala/utils.py:858`; `app/ponto/services/cartorio.py:593`; `app/ponto/services/cartorio.py:638`; `app/ponto/services/cartorio.py:645`; `app/ponto/services/lastro.py:340`; `app/ponto/services/lastro.py:356`) |
| **V10** | Correto que o comando virou vigia e o escritor exige lâmpada acesa. **Exagerada a alegação de cobertura:** A5 mocka `reconciliar=False`, sem testar o gate real; C5 testa o vigia, sem executar o consumidor. A3/A4 exercem apagado/metade. “M1–M5 morderam”: **[nao verificado]**. (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:53`; `app/ponto/services/lastro.py:394`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:144`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:258`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:122`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:130`) |

**D4. Sim: o `.mmd` inteiro conferiu com a recomposição em memória do gerador.** O gerador compõe os blocos e deriva as contagens do registro: **40 vigias, 12 prazos, 1 juiz**, `disparar_perguntas_competencia`. `fechar_cobranca_com_lastro` está declarado como vigia e aparece no bloco correspondente. (`app/core/management/commands/gerar_diagrama.py:912`; `app/core/management/commands/gerar_diagrama.py:242`; `app/docs/ARQUITETURA.mmd:207`; `app/docs/ARQUITETURA.mmd:217`; `app/config/crons.py:2155`; `app/docs/ARQUITETURA.mmd:391`)

Execução da suíte e resultados observados em produção: **[nao verificado]**. [nao verificado]

## 06/11 05:21 · sessao 6e30e659ccea · - · - · origem=Ronald (tmux)

D1/D2 — evidências consolidadas:

- `continue` sai normalmente do `with`; não pede rollback, não anexa nota e não incrementa `n`. Porém **não significa ausência de efeitos**: `reconciliar(registrar=True)` pode gravar `fechamento_recusado` antes de retornar `False`; essa auditoria permanece na saída normal. (`app/ponto/services/lastro.py:399-403`; `app/chamados/models.py:617-619`, `:730-734`, `:380-391`, `:407-414`.)
- O `atomic(savepoint=False)` e o `select_for_update` já existiam e continuam operando: `reconciliar` abre atomic e `_recarregar_estado_se_mudou` trava quando está em atomic. O novo envoltório estende a transação/trava até a nota. (`app/chamados/models.py:616-620`, `:638`, `:671-675`; `app/ponto/services/lastro.py:399-402`.)
- **Os signals já rodavam dentro de transação antes do delta**: `_gravar_estado` tem seu próprio `atomic` e executa `save` dentro dele, chamado pelo atomic de `reconciliar`. Portanto não procede a premissa de que uma exceção do cartório passou agora, genericamente, a poder desfazer o fecho. (`app/chamados/models.py:274-279`, `:616-620`.)
- O receiver do cartório é síncrono e captura `Exception` via `engolir`; esse contexto somente registra e suprime, não cria savepoint. Erro Python ali não vira automaticamente falha do consumidor. SQL que deixe a transação abortada fora de savepoints é risco preexistente; manifestação específica de `needs_rollback`, sem execução/driver aberto: [nao verificado]. (`app/chamados/signals.py:171-180`; `app/core/observ.py:85-91`.)
- Há isolamento próprio em efeitos que prometem best-effort: resolução de perguntas, auditoria e escrita de célula/evento usam atomics internos. (`app/chamados/signals.py:88-94`; `app/core/observ.py:133-138`; `app/ponto/portas/celula.py:681-684`.)

Callbacks que passam a aguardar também a nota e são descartados se ela provocar rollback:

- Chamado filho morto → reconciliar pai: `on_commit`, condicionado à saída de VIVOS e existência de pai vivo. (`app/chamados/signals.py:295-314`.)
- Pergunta salva → fechar beco: `on_commit`. A resolução síncrona pode carimbar perguntas. (`app/chamados/signals.py:192-202`; `app/chamados/services/disputa_emissao.py:1603-1604`.)
- Disputa fechada → retomar regularizações: `on_commit`; a resolução síncrona pode fechar disputa. (`app/chamados/signals.py:98-121`; `app/chamados/services/disputa_emissao.py:1695-1696`.)
- Isentos: o receiver direto retorna para chamado encerrado; pode agendar na reabertura em cascata. (`app/chamados/signals.py:228-251`.)

Escrita de evento na cascata:

- Cartório lavra e carimba → `carimbar` → `salvar_e_emitir` → `EventoCelula.objects.create`; a emissão é síncrona, no mesmo banco/transação, não `on_commit`. Pode ser desfeita pela falha posterior da nota. Não encontrei conexão própria nos arquivos examinados; o caminho demonstrado usa ORM comum. (`app/ponto/services/cartorio.py:1800-1823`; `app/ponto/portas/celula.py:829-849`, `:676-684`, `:666-673`.)

**Risco novo relevante: push externo antes do commit.** [nao verificado]

- Cartório pode emitir/reabrir cobrança pelos marcos restantes e chamar `_abrir_chamado`. Esse emissor chama imediatamente `push_supervisao` para perfil não flexível. O utilitário efetivamente chama `webpush` durante a execução. (`app/ponto/services/cartorio.py:1740-1742`, `:1777-1780`; `app/ponto/management/commands/detectar_ausencias.py:774-783`; `app/chamados/push_utils.py:89-111`, `:38-48`.)
- O emissor também pode criar o fio de disputa; a porta pode enviar WebPush e FCM imediatamente, com `messaging.send(msg)` síncrono. A supressão de push usada pela fábrica da célula não envolve genericamente esse emissor. (`app/ponto/management/commands/detectar_ausencias.py:790-801`; `app/chamados/services/disputa_emissao.py:1123-1124`, `:1535-1543`; `app/chamados/fcm_utils.py:85`, `:101-106`; `app/chamados/services/fabrica_da_celula.py:175`.)
- Inferência: se a nota falhar **depois** desses envios, o novo atomic desfaz fecho/reabertura/lavra/eventos, mas não recolhe notificações já enviadas; outra tentativa pode repetir os avisos. Antes, a nota falhava depois do commit do gate. Ocorrência em produção e destinatários configurados: [nao verificado]. (`app/ponto/services/lastro.py:399-407`; caminhos de envio acima.)

Fila e morte:

- Falha propagada da nota é capturada pelo fechador, que acrescenta `falhas`; consumidor incrementa tentativas/erro dos eventos originalmente carregados e conserva `processado_em=NULL`. (`app/ponto/services/lastro.py:404-407`; `app/chamados/reconciliador.py:1376-1385`.)
- Após cinco falhas, o evento sai da seleção viva; pode ficar chamado aberto sem retry automático desse evento. Evento novo do mesmo colaborador pode provocar nova passagem, pois o fechador percorre a fila inteira do colaborador. (`app/chamados/reconciliador.py:1198`, `:1235-1244`, `:1336-1371`; `app/ponto/services/lastro.py:387-388`.)
- `--semear` não é recuperação genérica de `batida_ausente`: semeia chamados dos módulos de turno e perguntas órfãs, não toda cobrança viva. (`app/chamados/reconciliador.py:1190-1193`, `:1278-1299`; conforme definição de `MODULOS_DE_TURNO` conferida pelo agente principal.)
- Quem vê mortos: consumidor imprime a contagem; `reconciliar_chamados --alarme` inclui mortos na soma e sai 2. Config declara consumidor a cada minuto e vigia a cada quinze minutos. Destinatário humano efetivo: [nao verificado]. (`app/chamados/management/commands/consumir_eventos_celula.py:36-37`; `app/chamados/management/commands/reconciliar_chamados.py:100-124`; `app/config/crons.py:394-395`.)
- Vigia de lastro também considera morto como ausência de evento vivo e pode listar o chamado elegível. (`app/ponto/services/lastro.py:420-423`; `app/ponto/management/commands/fechar_cobranca_com_lastro.py:77-83`.)

## 06/11 05:19 · sessao ba4b19cc5a2f · - · - · origem=Ronald (tmux)

D3, leitura estática:

- **V4: motivo sustentado.** O save do estado chama o cartório (`app/chamados/models.py:279`, `app/chamados/signals.py:180`). O cartório emite cobrança parcial pelos marcos apagados (`app/ponto/services/cartorio.py:1768`, `app/ponto/services/cartorio.py:1778`); estes vêm de `marcos_apagados` (`app/ponto/services/cartorio.py:1878`) e substituem o contexto da cobrança (`app/ponto/management/commands/detectar_ausencias.py:558`). Um marco apagado impede novo fechamento (`app/ponto/services/lastro.py:179`). O comentário também documenta reabertura síncrona pelo receiver (`app/chamados/models.py:269`). A equivalência histórica com o comando antigo, sem abrir sua versão anterior, fica **[nao verificado]**.

- **V5: descarte coerente como limite de escopo; a lacuna existe.** O regenerador muda DNA e preserva `ata` no `update_fields` (`app/ponto/portas/celula.py:265`, `app/ponto/portas/celula.py:270`); `recalculo=False` pula o julgamento (`app/ponto/portas/celula.py:390`). Ainda agenda rejunção e reavaliação de feriado (`app/ponto/portas/celula.py:461`, `app/ponto/portas/celula.py:473`). Portanto é insumo novo sem relavra, e não ata nova sem evento: somente ata/veredito acordam a outbox nessa porta (`app/ponto/portas/celula.py:663`, `app/ponto/portas/celula.py:680`). Cadastro de isenção altera o insumo (`app/colaboradores/services/colaborador.py:34`, `app/colaboradores/services/colaborador.py:61`); o signal aberto agenda silêncio dos chamados, sem relavrar a célula (`app/chamados/signals.py:213`, `app/chamados/signals.py:224`).

- **V6: motivo sustentado.** O gate separa `com_lastro` e retidos (`app/ponto/services/lastro.py:318`); o comando imprime os retidos em `(a2)` (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:61`); o placar raspa esse número e soma ao TOTAL (`bin/placar_code.sh:121`, `bin/placar_code.sh:549`). O aval consta da cópia (`app/docs/PROMPTS.md:1859`).

- **V7: defeito do produtor confirmado; descarte não significa inexistência do problema.** A isenção fabrica a hora planejada com `missing=False` (`app/escala/utils.py:858`). `_reais` aceita essas células sem excluir isenção (`app/ponto/services/cartorio.py:593`); o ramo `j is None` acende por proximidade e sai sem carimbar (`app/ponto/services/cartorio.py:632`, `app/ponto/services/cartorio.py:638`, `app/ponto/services/cartorio.py:645`), enquanto os demais ramos carimbam (`app/ponto/services/cartorio.py:680`, `app/ponto/services/cartorio.py:691`). Sem marcador, `como_acendeu` devolve batida e a nota pode dizer EXISTE (`app/ponto/services/lastro.py:338`, `app/ponto/services/lastro.py:340`, `app/ponto/services/lastro.py:356`). Os outros leitores também dependem do marcador (`app/escala/services/leitor_celula.py:139`, `app/ponto/supra_juiz.py:90`, `app/ponto/status_dia.py:186`). A ambiguidade das atas antigas é declarada (`app/ponto/services/cartorio.py:547`). Medição da sombra e número publicado na fila: **[nao verificado]**.

- **V10: conclusão funcional coerente, justificativa de cobertura exagerada.** O comando atual somente julga e alarma (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:53`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:77`). O escritor exige lâmpada acesa (`app/ponto/services/lastro.py:394`). A3/A4 exercem apagado e metade resolvida (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:122`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:130`); **A5 mocka** `reconciliar=False`, não testa o gate real (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:144`); **C5 testa o vigia**, não executa o consumidor (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:255`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:258`). “M1–M5 morderam”: **[nao verificado]**.

## 06/11 05:19 · sessao 2414262bf15d · - · - · origem=Ronald (tmux)

D4: **sim, bate integralmente**. Comparei em memória o `.mmd` commitado com as oito funções e o cabeçalho que `gerar()` usa: igualdade byte a byte, 427 linhas. A composição está em `app/core/management/commands/gerar_diagrama.py:911-919`; o arquivo termina em `app/docs/ARQUITETURA.mmd:427`.

- **40 vigias, 12 prazos e 1 juiz**: o tabuleiro deriva os conjuntos por papel sobre `todas_as_entradas()` (`app/core/management/commands/gerar_diagrama.py:233-256`), que reúne `CRONS` e `ENCADEADOS` (`app/config/crons.py:1752-1763`). Os números estão corretos em `app/docs/ARQUITETURA.mmd:207-209`.
- O único juiz derivado é `disparar_perguntas_competencia`: registro em `app/config/crons.py:2009-2013`, derivação em `app/config/crons.py:2313-2322`, desenho em `app/docs/ARQUITETURA.mmd:217`.
- `fechar_cobranca_com_lastro` está declarado como **vigia** (`app/config/crons.py:2155-2157`), com agenda `40 * * * *`, argumento `--alarme`, log `lastro.log`, estágio `continuo` e dependências vazias (`app/config/crons.py:731-732`). O novo nó aparece em `app/docs/ARQUITETURA.mmd:391`.
- O bloco `(h)` continua derivando somente de `CRONS`, enquanto o contador do tabuleiro inclui também os encadeados; isso é comportamento do gerador, não divergência introduzida pelo delta (`app/core/management/commands/gerar_diagrama.py:396-401`; `app/core/management/commands/gerar_diagrama.py:242-243`).

Não executei a suíte: ela inclui teste que escreve no `MAPA.md`, incompatível com a restrição de leitura (`app/core/tests/test_selo_diagrama_do_codigo.py:49-60`). A comparação em memória executou as funções originais do gerador, removendo apenas o import de Django e a classe de comando do módulo carregado; execução da suíte e ambiente Django: [nao verificado].

## 06/11 04:45 · sessao d58c4d563f58 · OTIMIZA-3 · verificador · origem=Code

**Parecer: bloqueia pelo achado 1. Os bloqueios anteriores estão corrigidos, mas permanece outro falso zero com registros empilhados** (`bin/quem_chama.py:256`, `bin/quem_chama.py:284`, `bin/quem_chama.py:353`, `bin/quem_chama.py:714`).

1. **BLOQUEIA — P1/P5: remove todos os decoradores de registro, mas guarda somente o primeiro.**

   `_registro()` retorna no primeiro decorador reconhecido; a limpeza nova retira todos os registros reconhecidos de `decor` (`bin/quem_chama.py:242`, `bin/quem_chama.py:256`, `bin/quem_chama.py:282`).

   Contraexemplo, alterando somente o corpo de `f` em uma biblioteca `m_tags.py`:

   ```python
   register = template.Library()

   @register.filter(name="a")
   @register.filter(name="b")
   def f(x):
       return x
   ```

   Com `{% load m_tags %}` e `{{ x|b }}`, o fluxo conserva `decor=[]` e `registros=[('filtro', 'a')]`. A busca pelo nome `b` desaparece, e a faceta imprime **`simbolo f [filtro/tag]: 0 chamador(es)`**; os decoradores também não produzem leitura própria de `f` (`bin/quem_chama.py:284`, `bin/quem_chama.py:357`, `bin/quem_chama.py:521`, `bin/quem_chama.py:611`, `bin/quem_chama.py:771`).

   Esse falso zero já era possível pela precedência antiga mostrada no anexo; não é regressão exclusiva desta volta. Contudo, a limpeza nova impede que o segundo registro funcione como motivo opaco depois da inversão. O contrato anunciado inclui filtros registrados com nomes constantes (`bin/quem_chama.py:61`, `bin/quem_chama.py:256`, `bin/quem_chama.py:284`). Ocorrência desse empilhamento na produção: **[nao verificado]**.

2. **FILA — P1: `@stringfilter` rebaixa um filtro conhecido conservadoramente.**

   Contraexemplo: `@register.filter` empilhado com `@stringfilter`. O registro sai de `decor`, mas `stringfilter` permanece e não integra `DECORADORES_TRANSPARENTES`; o resultado passa a ser `decorado @stringfilter: o decorador pode registrar` (`bin/quem_chama.py:65`, `bin/quem_chama.py:69`, `bin/quem_chama.py:284`, `bin/quem_chama.py:353`).

   É perda de precisão, sem falso zero: o tipo sai da lista de contagem e os usos encontrados pelos registros continuam sendo emitidos como avulsos (`bin/quem_chama.py:611`, `bin/quem_chama.py:776`). Ocorrência real de `@stringfilter` nesta cópia: **[nao verificado]**.

Nos demais casos de **P1**, a proteção permanece:

| Caso concreto | Resultado |
|---|---|
| `@outra.filter`, com `outra` ausente de `_libs_de_template` | Não reconhece registro, conserva o decorador e classifica como opaco (`bin/quem_chama.py:245`, `bin/quem_chama.py:282`, `bin/quem_chama.py:353`). |
| `simple_tag(takes_context=True)` | Conserva o nome da função e classifica como `filtro/tag`; exemplo existente: `next_para` (`app/core/templatetags/nav_extras.py:49`, `bin/quem_chama.py:247`, `bin/quem_chama.py:253`). |
| `inclusion_tag(..., takes_context=True)` | Mesmo resultado; exemplo existente: `voltar` (`app/core/templatetags/nav_extras.py:36`, `bin/quem_chama.py:247`). |
| `register.filter('x', f)` fora de decorador | `f` fica `simbolo-de-topo`, mas a chamada conta como leitura própria de `f`, evitando zero nesse exemplo (`bin/quem_chama.py:360`, `bin/quem_chama.py:521`, `bin/quem_chama.py:529`). |
| `C.f` decorado dentro de classe | Não recebe registro; prevalece atributo de classe, não varrido (`bin/quem_chama.py:280`, `bin/quem_chama.py:345`). |

**P2:** não encontrei leitor operacional dependente da semântica antiga de `decor`; classificação e varredura recebem os campos depois da soma (`bin/quem_chama.py:712`, `bin/quem_chama.py:716`, `bin/quem_chama.py:737`).

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `decor` | Coleta, limpeza e gravação: `bin/quem_chama.py:281`, `bin/quem_chama.py:283`, `bin/quem_chama.py:285`; inicialização/união: `bin/quem_chama.py:710`, `bin/quem_chama.py:712` | União/deduplicação: `bin/quem_chama.py:712`; classificação: `bin/quem_chama.py:353` |
| `registro` singular | Extração: `bin/quem_chama.py:247`, `bin/quem_chama.py:251`, `bin/quem_chama.py:255`; gravação: `bin/quem_chama.py:285` | Condição da limpeza: `bin/quem_chama.py:282`; exclusão da cópia e transferência: `bin/quem_chama.py:709`, `bin/quem_chama.py:713`, `bin/quem_chama.py:714` |
| `registros` plural | Inicialização/acréscimo: `bin/quem_chama.py:710`, `bin/quem_chama.py:714` | Deduplicação: `bin/quem_chama.py:713`; classificação: `bin/quem_chama.py:356`, `bin/quem_chama.py:357`; templates: `bin/quem_chama.py:611`, `bin/quem_chama.py:612` |

**P3:** as asserções novas distinguem os mutantes apontados anteriormente; não identifiquei verde acidental por texto de outra faceta nos casos abaixo (`bin/tests/test_quem_chama.sh:197`, `bin/tests/test_quem_chama.sh:202`).

| Asserção nova | Por que distingue o defeito |
|---|---|
| Lista nominal sob arquivo | Exige texto específico do cabeçalho EMBUTIDO dentro da faceta arquivo (`bin/tests/test_quem_chama.sh:329`, `bin/quem_chama.py:766`). |
| Autoimport sob arquivo | Exige cabeçalho rebaixado **e** sítio `selfmod.py:5` nessa faceta (`bin/tests/test_quem_chama.sh:344`, `bin/tests/test_quem_chama.sh:345`). |
| Registro versus opaco | Exige `decorado @receiver`; `filtro/tag` não satisfaz o positivo (`bin/tests/test_quem_chama.sh:347`, `bin/tests/test_quem_chama.sh:348`). |
| Opaco removido | O `receiver` existe somente na base; conservar só a ponta perde o positivo (`bin/tests/test_quem_chama.sh:150`, `bin/tests/test_quem_chama.sh:183`, `bin/tests/test_quem_chama.sh:350`). |
| Nome antigo do filtro | O template específico usa `velho_f`; conservar somente o novo perde o achado (`bin/tests/test_quem_chama.sh:153`, `bin/tests/test_quem_chama.sh:352`). |
| Include entre comentários | A remoção gulosa apaga o include exigido na linha 2 (`bin/tests/test_quem_chama.sh:151`, `bin/tests/test_quem_chama.sh:354`). |
| Argumento de outra tag | Generalizar `filter` para qualquer tag produz exatamente o falso uso proibido (`bin/tests/test_quem_chama.sh:152`, `bin/tests/test_quem_chama.sh:356`). |

`_so_na_fac` agora detecta duplicação de linhas. Os fixtures atuais escolhem achados exclusivos; a troca de nomes entre dois filtros ainda poderia gerar duplicação legítima entre facetas, mas esse cenário não está nos fixtures atuais (`bin/tests/test_quem_chama.sh:205`, `bin/tests/test_quem_chama.sh:143`, `bin/tests/test_quem_chama.sh:180`, `bin/quem_chama.py:714`, `bin/quem_chama.py:620`).

**P4:** os bloqueios anteriores estão fechados: decorador acrescentado ou removido permanece na união; nomes velho e novo são buscados; autoimport rebaixa arquivo e símbolos; opaco prevalece sobre registro (`bin/quem_chama.py:712`, `bin/quem_chama.py:714`, `bin/quem_chama.py:611`, `bin/quem_chama.py:743`, `bin/quem_chama.py:757`, `bin/quem_chama.py:353`).

Execução integral do selo, mutantes integrais e ranges históricos: **[nao verificado]**.

## 06/11 04:44 · sessao 5d2f2b28a4fe · - · - · origem=Ronald (tmux)

P1/P5, conferidos por inspeção e execução em memória:

1. **BLOQUEIA — múltiplos registros são apagados de `decor`, mas somente um entra em `registros`.** `_registro` retorna no primeiro decorador reconhecido (`bin/quem_chama.py:256`), enquanto o delta remove todos os decoradores de registro (`bin/quem_chama.py:282`, `bin/quem_chama.py:284`).

   Contraexemplo:

   ```python
   register = template.Library()

   @register.filter(name="a")
   @register.filter(name="b")
   def f(x):
       return x
   ```

   Um template `{% load m_tags %}\n{{ x|b }}` não é associado ao símbolo: o metadado contém apenas `('filtro', 'a')`, e o varredor busca exclusivamente `info['registros']` (`bin/quem_chama.py:611`, `bin/quem_chama.py:614`). A reprodução em memória emitiu `simbolo f [filtro/tag]: 0 chamador(es)`; a leitura própria também ficou vazia (`bin/quem_chama.py:492`, `bin/quem_chama.py:529`, `bin/quem_chama.py:770`).

   O falso zero já era possível na v6 anexada, pela precedência antiga; a remoção indiscriminada nova conserva esse defeito mesmo depois da inversão de precedência (`bin/quem_chama.py:353`, `bin/quem_chama.py:356`). Ocorrência concreta desse empilhamento na cópia: **[nao verificado]**.

2. **FILA — `@stringfilter` passa a rebaixar filtro conhecido.** Com `@register.filter` empilhado com `@stringfilter`, o primeiro sai de `decor`, mas `stringfilter` permanece e não integra `DECORADORES_TRANSPARENTES` (`bin/quem_chama.py:65`, `bin/quem_chama.py:69`, `bin/quem_chama.py:284`). A reprodução devolveu `decorado @stringfilter: o decorador pode registrar`, antes de considerar o registro conhecido (`bin/quem_chama.py:353`, `bin/quem_chama.py:356`). É perda conservadora de precisão; não produz zero contado (`bin/quem_chama.py:774`). Ocorrência real desse decorador na cópia: **[nao verificado]**.

3. **FILA — os demais casos pedidos conservam a proteção.**

   - Biblioteca desconhecida de `_libs_de_template`: `_registro` não reconhece, o decorador permanece e vira motivo opaco (`bin/quem_chama.py:245`, `bin/quem_chama.py:282`, `bin/quem_chama.py:353`).
   - `simple_tag(takes_context=True)`: conserva o nome da função; a lógica especial de argumentos posicionais só vale para `filter` e `tag` (`bin/quem_chama.py:247`, `bin/quem_chama.py:253`). A reprodução sobre `nav_extras.next_para` devolveu `filtro/tag`; o caso existe em `app/core/templatetags/nav_extras.py:49`.
   - `inclusion_tag(..., takes_context=True)`: igualmente preservado; caso real `voltar` em `app/core/templatetags/nav_extras.py:36`.
   - `register.filter("x", f)` fora de decorador: `f` fica `simbolo-de-topo`, mas a chamada gera leitura própria de `f`, evitando zero no exemplo (`bin/quem_chama.py:360`, `bin/quem_chama.py:521`, `bin/quem_chama.py:529`).
   - Membro de classe: não recebe registro e prevalece o tipo não varrido de atributo de classe (`bin/quem_chama.py:280`, `bin/quem_chama.py:345`).

## 06/11 04:43 · sessao 16c879ef65ef · - · - · origem=Ronald (tmux)

P2: fluxo consistente; nenhum leitor encontrado exige o decorador de registro dentro de `decor` (`bin/quem_chama.py:280`, `bin/quem_chama.py:353`, `bin/quem_chama.py:712`).

| Campo | Escritores | Leitores |
|---|---|---|
| `decor` cru | Coleta e filtragem: `bin/quem_chama.py:281`, `bin/quem_chama.py:283`; gravação: `bin/quem_chama.py:285` | União: `bin/quem_chama.py:712` |
| `decor` somado | Inicialização: `bin/quem_chama.py:710`; união: `bin/quem_chama.py:712` | Deduplicação: `bin/quem_chama.py:712`; classificação: `bin/quem_chama.py:353` |
| `registro` singular | `_registro`: `bin/quem_chama.py:247`, `bin/quem_chama.py:251`, `bin/quem_chama.py:255`; dict cru: `bin/quem_chama.py:285` | Condição da filtragem: `bin/quem_chama.py:282`; exclusão da cópia: `bin/quem_chama.py:709`; união: `bin/quem_chama.py:713`, `bin/quem_chama.py:714` |
| `registros` plural | Inicialização: `bin/quem_chama.py:710`; acréscimo: `bin/quem_chama.py:714` | Deduplicação: `bin/quem_chama.py:713`; classificação: `bin/quem_chama.py:356`, `bin/quem_chama.py:357`; usos em template: `bin/quem_chama.py:611`, `bin/quem_chama.py:612` |

A classificação ocorre depois da soma e a varredura de templates ocorre depois da construção dos alvos (`bin/quem_chama.py:715`, `bin/quem_chama.py:716`, `bin/quem_chama.py:721`, `bin/quem_chama.py:737`). A busca textual em código de `app/` e `bin/` não encontrou consumidor adicional dessas funções; consumidor fora desse universo: **[nao verificado]**.

P4: os bloqueios identificados nas voltas anteriores estão fechados pelo fluxo atual:

- Decorador acrescentado na ponta: a união conserva `receiver`; o tipo fica “decorado”, sem contagem (`bin/quem_chama.py:712`, `bin/quem_chama.py:353`, `bin/quem_chama.py:355`).
- Registro renomeado: os nomes velho e novo ficam em `registros` e ambos geram usos em template (`bin/quem_chama.py:713`, `bin/quem_chama.py:714`, `bin/quem_chama.py:611`, `bin/quem_chama.py:620`).
- Autoimport em string executada: é processado antes da guarda do próprio arquivo; arquivo e símbolo recebem o sítio e são rebaixados (`bin/quem_chama.py:743`, `bin/quem_chama.py:746`, `bin/quem_chama.py:757`, `bin/quem_chama.py:760`).
- Registro na base e opaco na ponta: o registro reconhecido sai de `decor`, mas `receiver` permanece; opaco vence o registro (`bin/quem_chama.py:280`, `bin/quem_chama.py:284`, `bin/quem_chama.py:353`, `bin/quem_chama.py:356`).

Esses três primeiros fluxos também foram exercitados com `quem_chama()` e revisões substituídas por dicionários em memória; os resultados corresponderam aos ramos citados (`bin/quem_chama.py:705`, `bin/quem_chama.py:716`, `bin/quem_chama.py:743`, `bin/quem_chama.py:760`). Execução de ranges históricos e do selo integral: **[nao verificado]**.

Não identifiquei defeito adicional no delta relativo aos campos ou ao fechamento dos bloqueios; contraexemplo adicional: **[nao verificado]**.

## 06/11 04:43 · sessao 9b3c092840e4 · - · - · origem=Ronald (tmux)

P3: **nenhum defeito demonstrado nas asserções novas ou falha espúria nos fixtures atuais** (`bin/tests/test_quem_chama.sh:149`, `bin/tests/test_quem_chama.sh:205`, `bin/tests/test_quem_chama.sh:329`, `bin/tests/test_quem_chama.sh:344`).

| Asserção nova | Conferência |
|---|---|
| Lista nominal sob arquivo, linha 329 | Exige texto do cabeçalho EMBUTIDO dentro da faceta arquivo; o achado comum não contém esse texto (`bin/quem_chama.py:766`, `bin/quem_chama.py:778`). |
| Autoimport sob arquivo, linhas 344–345 | Exige tanto cabeçalho rebaixado quanto sítio específico `selfmod.py:5`; omitir o arquivo reprova ambas (`bin/tests/test_quem_chama.sh:181`, `bin/quem_chama.py:743`, `bin/quem_chama.py:757`). |
| Registro versus opaco, linhas 347–348 | Fixture conserva registro da base e acrescenta receiver na ponta; tipo `filtro/tag` não satisfaz o positivo (`bin/tests/test_quem_chama.sh:149`, `bin/tests/test_quem_chama.sh:182`, `bin/quem_chama.py:351`). |
| Opaco removido, linha 350 | O receiver existe somente na base; conservar apenas decoradores da ponta perde o texto exigido (`bin/tests/test_quem_chama.sh:150`, `bin/tests/test_quem_chama.sh:183`). |
| Nome antigo do filtro, linha 352 | O template específico usa somente `velho_f`; manter somente registro novo perde o achado exigido (`bin/tests/test_quem_chama.sh:153`, `bin/quem_chama.py:611`). |
| Include entre comentários, linha 354 | Exercício em memória: atual emite include na linha 2; mutante guloso `.*` não emite (`bin/tests/test_quem_chama.sh:151`, `bin/quem_chama.py:577`). |
| Argumento de outra tag, linha 356 | Exercício em memória: atual emite load e tag `tg`; mutante que generaliza `filter` para qualquer tag acrescenta exatamente o uso falso proibido (`bin/tests/test_quem_chama.sh:152`, `bin/quem_chama.py:614`). |

A exigência `tot=1, sob=1` detecta duplicação de linhas; os usos atuais do helper escolhem achados únicos nos respectivos fixtures (`bin/tests/test_quem_chama.sh:205`, `bin/tests/test_quem_chama.sh:216`, `bin/tests/test_quem_chama.sh:223`, `bin/tests/test_quem_chama.sh:228`, `bin/tests/test_quem_chama.sh:229`, `bin/tests/test_quem_chama.sh:243`, `bin/tests/test_quem_chama.sh:284`, `bin/tests/test_quem_chama.sh:340`, `bin/tests/test_quem_chama.sh:352`, `bin/tests/test_quem_chama.sh:354`).

Execução integral do selo e mutantes integrais: **[nao verificado]**.

## 06/11 04:34 · sessao b4f914dd4fbb · LASTRO-PARTE-2 · verificador · origem=Code

**Volta 1: há pendências verificáveis.** O diagrama diverge do registro dos crons, e a nova nota ainda pode afirmar “a batida EXISTE” quando a lâmpada perdeu o marcador de isenção. (`app/docs/ARQUITETURA.mmd:218`; `app/config/crons.py:2157`; `app/ponto/services/cartorio.py:593`; `app/ponto/services/cartorio.py:638`; `app/ponto/services/lastro.py:340`.) Execução da suíte, RED, sombra em produção e instalação do cron: **[nao verificado]**.

**V1. Chamadores que quebram**

Não encontrei, na busca textual e por AST da cópia, chamador de produção das funções apagadas nem execução residual de `fechar_cobranca_com_lastro --apply`. Os consumidores encontrados usam a interface nova: consumidor → `fechar_pelo_marco_colab`; comando → `julgar`/`com_lastro_sem_evento`; cron → `--alarme`; placar → comando sem `--apply`. (`app/chamados/reconciliador.py:1370`; `app/ponto/management/commands/fechar_cobranca_com_lastro.py:52`; `app/config/crons.py:731`; `bin/placar_code.sh:117`.)

Crontab efetivamente instalado, scripts externos à cópia e processos carregando código antigo: **[nao verificado]**.

**V2. Escritores e leitores**

| Campo/interface | Quem escreve | Quem lê |
|---|---|---|
| `status_local`, neste caminho | `reconciliar()` decide pelo gate e chama `_gravar_estado`, que atribui e salva. (`app/chamados/models.py:616`; `app/chamados/models.py:222`; `app/chamados/models.py:279`.) | O próprio gate recarrega o estado; fila, API e sinal do cartório também o leem. (`app/chamados/models.py:638`; `app/chamados/services/painel.py:187`; `app/api/views.py:1145`; `app/chamados/signals.py:169`.) |
| Nota `lastro_encontrado` | Lastro chama `anexar_ocorrencia` somente após retorno verdadeiro do gate; a porta cria `FollowupChamado`. (`app/ponto/services/lastro.py:395`; `app/chamados/services/nota.py:103`.) | Histórico, templates e API leem o conteúdo; a API filtra notas de sistema pela palavra `justificativa`. (`app/chamados/services/historico.py:84`; `app/templates/chamados/detalhe_local.html:135`; `app/api/views.py:1129`; `app/api/views.py:62`.) |
| `EventoCelula.processado_em` / `tentativas` | Nascimento usa `NULL`/zero; consumidor incrementa tentativa ou marca processado. (`app/escala/models.py:1859`; `app/chamados/reconciliador.py:1379`; `app/chamados/reconciliador.py:1385`.) | Seleção dos pendentes, contador de mortos, semeadura, vigias e retenção. (`app/chamados/reconciliador.py:1238`; `app/chamados/reconciliador.py:1244`; `app/chamados/reconciliador.py:1284`; `app/ponto/services/lastro.py:415`; `app/chamados/reconciliador.py:1392`.) |
| Chave `lastro` do resultado | Inicializada e acumulada pelo consumidor. (`app/chamados/reconciliador.py:1327`; `app/chamados/reconciliador.py:1371`.) | Testes a verificam; o comando de produção formata outras chaves e **não imprime `lastro`**. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:103`; `app/chamados/management/commands/consumir_eventos_celula.py:32`.) |

`lastro_encontrado` **não é coluna `tipo` do Followup**: o modelo não possui esse campo. O argumento participa da identidade quando existe `objeto_ref`; esta chamada não fornece referência, portanto cria nota sem chave de deduplicação. (`app/chamados/models.py:1598`; `app/chamados/models.py:1618`; `app/chamados/services/nota.py:88`; `app/chamados/services/nota.py:95`; `app/ponto/services/lastro.py:396`.)

Não encontrei leitor de produção comparando o resultado inteiro, somando suas chaves ou exigindo o texto antigo. O leitor de produção encontrado usa formatação por nomes; os leitores de notas examinados trabalham com apresentação, autoria ou outros padrões, como `SLA` e `[COBRANCA]`. (`app/chamados/management/commands/consumir_eventos_celula.py:32`; `app/chamados/services/sla.py:45`; `app/chamados/services/painel.py:353`.) Consumidores externos: **[nao verificado]**.

**V3. Transação, exceção e `adiar`**

Não há transação do consumidor envolvendo o colaborador inteiro. Cada `reconciliar()` abre seu próprio `atomic(savepoint=False)`; a nota vem depois. A atualização dos eventos ocorre em outro `atomic`, posterior e curto. (`app/chamados/reconciliador.py:1343`; `app/chamados/models.py:616`; `app/ponto/services/lastro.py:395`; `app/chamados/reconciliador.py:1376`.)

- Exceção no lastro entra em `falhas`: mantém os eventos pendentes e incrementa uma tentativa. IDs já lidos não voltam na mesma execução. (`app/ponto/services/lastro.py:398`; `app/chamados/reconciliador.py:1334`; `app/chamados/reconciliador.py:1379`.)
- Recusa normal do gate não é falha e permite marcar o evento processado. (`app/ponto/services/lastro.py:395`; `app/chamados/reconciliador.py:1385`.)
- `adiar=True` **não pula o lastro**: a chamada acontece depois de definir a flag; posteriormente o evento permanece pendente sem gastar tentativa. (`app/chamados/reconciliador.py:1363`; `app/chamados/reconciliador.py:1371`; `app/chamados/reconciliador.py:1380`.)
- Exceção anterior ao lastro pula sua execução, mas preserva o evento com tentativa incrementada. Colaborador inexistente pula o trabalho e permite processamento do evento. (`app/chamados/reconciliador.py:1344`; `app/chamados/reconciliador.py:1372`; `app/chamados/reconciliador.py:1379`.)

**Risco de nota perdida:** o fechamento pode confirmar e a inserção da nota falhar depois. No retry, o chamado terminal sai da fila e a nota não é recuperada. Possibilidade pelo código; ocorrência em produção **[nao verificado]**. (`app/chamados/models.py:616`; `app/ponto/services/lastro.py:385`; `app/ponto/services/lastro.py:395`; `app/chamados/services/nota.py:103`.)

**V4. Ordem e fecha/reabre**

A ordem explícita é: **rejunção → perguntas absolvidas → chamados absolvidos → premissa morta → órfãs → lastro**. Nenhum desses passos é chamado explicitamente depois do lastro. (`app/chamados/reconciliador.py:1351`; `app/chamados/reconciliador.py:1371`.)

Entretanto, salvar o fechamento dispara o cartório sincronamente. O cartório pode reemitir por `FURO_PARCIAL` ou `FURO_COM_COBRANCA_MORTA`, encontrando e reabrindo o mesmo chamado do dia. (`app/chamados/models.py:279`; `app/chamados/signals.py:164`; `app/chamados/signals.py:180`; `app/ponto/services/cartorio.py:1689`; `app/ponto/services/cartorio.py:1777`; `app/ponto/management/commands/detectar_ausencias.py:693`.)

**Existe fecha/reabre no mesmo evento:** entrada acesa encerra a cobrança antiga; saída apagada permite renascimento pelos marcos novos, posteriormente espelhados no chamado. A guarda considera os marcos **do pedido**, portanto não barra essa troca. O próprio fixture migrado acrescentou saída para evitar essa reabertura. (`app/chamados/models.py:1223`; `app/chamados/models.py:1248`; `app/chamados/models.py:1310`; `app/ponto/management/commands/detectar_ausencias.py:743`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:223`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:259`.)

Consequentemente, `lastro=1` e nota “Encerrado” não garantem estado final terminal. Isso não demonstra loop infinito pelo mesmo marco: após espelhamento, o marco apagado não passa pelo lastro; perguntas do marco aceso também são excluídas da ressuscitação. (`app/ponto/services/lastro.py:392`; `app/ponto/services/lastro.py:395`; `app/chamados/models.py:1452`.)

**V5. Produtores de evento e escapes**

As escritas persistentes de ata encontradas passam por `salvar_e_emitir`, que salva e emite no mesmo `atomic` quando `ata`/`veredito` foi lavrado. Não encontrei alteração persistente de ata sem emissão nas vias rastreadas. (`app/ponto/portas/celula.py:662`; `app/ponto/portas/celula.py:676`; `app/ponto/portas/celula.py:785`; `app/ponto/portas/celula.py:817`; `app/ponto/portas/celula.py:864`.)

- **Cartório:** tanto o ramo normal quanto `nunca_bateu` passam os campos lavrados a `carimbar`, que salva e emite. (`app/ponto/services/cartorio.py:1242`; `app/ponto/services/cartorio.py:1264`; `app/ponto/services/cartorio.py:1802`; `app/ponto/services/cartorio.py:1822`; `app/ponto/portas/celula.py:849`.)
- **Ausência/atestado e justificativa:** sinais chamam o cartório; a dispensa integra a ata posteriormente lavrada. (`app/ponto/signals.py:143`; `app/ponto/signals.py:150`; `app/ponto/signals.py:161`; `app/ponto/services/cartorio.py:766`; `app/ponto/services/cartorio.py:1800`.)
- **Validação de pergunta:** materialização de batida/ausência aciona seus sinais. O carimbo de validação sozinho não altera ata nem emite diretamente esse evento. (`app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:430`; `app/chamados/services/materializacao.py:484`; `app/chamados/services/materializacao.py:897`; `app/ponto/signals.py:131`.)
- **Regeneração:** com recálculo, rejulga dias tocados, passados e não humanos. Frota usa `recalculo=False`; reversão grava DNA sem rejulgar naquele ato. São mudanças de **insumo sem relavra imediata**, não atas modificadas sem evento. (`app/ponto/portas/celula.py:390`; `app/escala/services/cadastro_tipo.py:348`; `app/ponto/portas/celula.py:569`.)
- **Isenção:** ligar a flag pelo cadastro aciona silenciamento, sem relavra imediata da ata nesse caminho. Também é lacuna de atualização do insumo, anterior à existência de uma lâmpada nova gravada. (`app/colaboradores/views.py:173`; `app/colaboradores/services/colaborador.py:55`; `app/chamados/signals.py:213`.)

Essas lacunas de insumo **não são necessariamente detectadas pelo vigia**, pois ele consulta a ata gravada; se ela ainda não acendeu, falta a condição examinada. (`app/ponto/services/lastro.py:289`; `app/ponto/services/lastro.py:417`.)

**V6. Zero do vigia e permanência**

`sem_evento=0` não significa ausência de chamado vivo com marco aceso: retidos ficam fora de `com_lastro`; registrados e silenciados ficam fora da fila. O retido continua no TOTAL do placar pela linha `(a2)`. (`app/ponto/services/lastro.py:318`; `app/ponto/services/lastro.py:416`; `app/chamados/services/painel.py:187`; `bin/placar_code.sh:121`; `bin/placar_code.sh:549`.)

Os três exemplos não demonstram, isoladamente, invisibilidade permanente:

- Retido liberado sem evento passa a `com_lastro` na próxima medição e será acusado se não houver pendente vivo. (`app/ponto/services/lastro.py:318`; `app/ponto/services/lastro.py:415`.)
- Eventos com `tentativas>=5` não entram em `eventos_pendentes`, portanto não mascaram esse vigia; há contador separado de mortos. (`app/chamados/reconciliador.py:1198`; `app/chamados/reconciliador.py:1238`; `app/chamados/management/commands/consumir_eventos_celula.py:37`.)
- Evento de outro dia serve para o colaborador inteiro: o lastro percorre todos os seus chamados elegíveis. (`app/chamados/reconciliador.py:1337`; `app/ponto/services/lastro.py:385`.)

Falhas antes do lastro podem manter temporariamente o vigia em zero enquanto existir evento vivo. Permanência por fluxo contínuo de falhas/eventos: **[nao verificado]**. (`app/chamados/reconciliador.py:1351`; `app/chamados/reconciliador.py:1372`; `app/chamados/reconciliador.py:1379`; `app/ponto/services/lastro.py:415`.)

**V7. Forma das lâmpadas e verdade da nota**

Nos caminhos normais, as chaves conferem: isenção usa hora prevista com `isencao=True`; dispensa inteira/parcial usa `dispensada`; o cartório propaga ambas. Não encontrei terceira chave equivalente nesses produtores. (`app/escala/utils.py:858`; `app/escala/utils.py:1267`; `app/ponto/services/cartorio.py:556`; `app/ponto/services/cartorio.py:680`; `app/ponto/services/cartorio.py:691`; `app/ponto/services/cartorio.py:774`.)

**A garantia universal da nota falha:**

1. Atas antigas sem marcador são explicitamente documentadas como ambíguas; `como_acendeu` interpreta ausência dos marcadores como batida. Presença atual desses registros em produção: **[nao verificado]**. (`app/ponto/services/cartorio.py:547`; `app/ponto/services/lastro.py:336`; `app/ponto/services/lastro.py:340`.)
2. Há caminho atual que perde o marcador: `_reais` inclui horas de isenção; o ramo de marco fora da régua acende por proximidade sem copiar `isencao`. Na reprodução em memória da função real, uma célula isenta às `07:00`, com DNA `hi=07:20, hii=07:00`, gerou E07:20 acesa sem marcador. A nova nota a classifica como batida. Atingibilidade desse cadastro em produção: **[nao verificado]**. (`app/ponto/services/cartorio.py:593`; `app/ponto/services/cartorio.py:624`; `app/ponto/services/cartorio.py:638`; `app/ponto/services/lastro.py:340`; `app/ponto/services/lastro.py:355`.)

**V8. Crons, contadores e selos**

No diff fornecido, as mudanças de `crons.py` correspondem à entrada com `--alarme`, saída dos juízes e inclusão como vigia. Esses destinos conferem na cópia; comparação independente com a base `a37c673d`: **[nao verificado]**. (`app/config/crons.py:731`; `app/config/crons.py:2018`; `app/config/crons.py:2157`.)

Placar, contrato estrutural e duração não exigem manter `--apply` ou o papel juiz: o placar preserva `(a)/(a2)`, o contador deriva o papel declarado e o JSON guarda somente duração. O selo de host exige justamente o comando sem `--apply`. (`bin/placar_code.sh:114`; `bin/placar_code.sh:549`; `app/core/contratos_estruturais.py:263`; `app/config/crons.py:2322`; `app/config/crons_duracao.json:32`; `bin/tests/test_placar_conta_o_retido_do_lastro.sh:594`.)

**Há divergência certa no diagrama:** ainda mostra 39 vigias, dois juízes e este comando ligado a juiz. O gerador deriva essas informações do registro atual, e o selo exige igualdade. A extração em memória gerou 40 vigias e um juiz; execução da suíte completa: **[nao verificado]**. (`app/docs/ARQUITETURA.mmd:207`; `app/docs/ARQUITETURA.mmd:209`; `app/docs/ARQUITETURA.mmd:218`; `app/core/management/commands/gerar_diagrama.py:242`; `app/core/management/commands/gerar_diagrama.py:264`; `app/core/tests/test_selo_diagrama_do_codigo.py:44`.)

**V9. Classes de reprovação**

Não identifiquei novo juiz de fechamento, escritor manual de status ou leitura explícita nova de relógio no corpo acrescentado: ele reutiliza `marcos_acesos`, `reconciliar` e a porta de nota. A trava do chamado continua dentro do gate; não foi acrescentada transação envolvendo o colaborador inteiro. (`app/ponto/services/lastro.py:392`; `app/ponto/services/lastro.py:395`; `app/ponto/services/lastro.py:396`; `app/chamados/models.py:616`; `app/chamados/models.py:673`; `app/chamados/reconciliador.py:1376`.)

As ressalvas concretas são a testemunha sem prova suficiente de batida, a nota fora da transação do fechamento, o fecha/reabre síncrono e o artefato gerado divergente. (`app/ponto/services/lastro.py:340`; `app/ponto/services/lastro.py:395`; `app/chamados/signals.py:180`; `app/docs/ARQUITETURA.mmd:218`.) RED executado, sombra N=0, pré-requisitos de pouso e instalação: **[nao verificado]**.

**V10. Mordida dos testes migrados** [nao verificado]

- **BUG114:** o teste de 30 dias sem lâmpada mudou deliberadamente de “fecha” para “permanece vivo e alarma”; não preserva literalmente a expectativa anterior de fecho do degrau 2. Os negativos sem batida/longe/sem marco/agora só classificam com `julgar`, sem testar o novo escritor, que percorre a fila diretamente. Esses negativos isolados passam mesmo com defeito no novo fechamento. (`app/ponto/tests/test_bug114_fechador_de_dois_dias.py:144`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:162`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:171`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:182`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:193`; `app/ponto/services/lastro.py:385`.)
- **BUG115:** mantém o positivo “marco aceso com furo vizinho → `lastro=1` e terminal” e o negativo “marco apagado → vivo”. O teste de recuo por outro tipo passou a cobrir somente classificação. (`app/ponto/tests/test_bug115_lampada_do_marco.py:185`; `app/ponto/tests/test_bug115_lampada_do_marco.py:207`; `app/ponto/tests/test_bug115_lampada_do_marco.py:167`.)
- **O231:** mantém medições discordantes, exige uma única chamada a `julgar` e relaciona quadro impresso ao número do vigia. (`app/ponto/tests/test_o231_lastro_mede_uma_vez.py:54`; `app/ponto/tests/test_o231_lastro_mede_uma_vez.py:64`.)
- **lastro_rele:** preserva o par objeto obsoleto → zero/sem nota e chamado vivo → um/com nota. O fixture efetivamente planta o marco cobrado aceso. (`app/ponto/tests/test_lastro_rele_o_chamado.py:46`; `app/chamados/tests/_dia_regularizado.py:8`; `app/ponto/tests/test_lastro_rele_o_chamado.py:59`; `app/ponto/tests/test_lastro_rele_o_chamado.py:69`.)

Remover a chamada nova do consumidor é distinguido pelos positivos que exigem `lastro=1`; não são selos inteiramente vazios. Execução com cura revertida e resultados dinâmicos: **[nao verificado]**. (`app/ponto/tests/test_bug115_lampada_do_marco.py:204`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:103`.)

_2123 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
