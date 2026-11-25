# CODEX-ESTADO

> Ultima resposta processada: **25/11/2026 08:28** · publicado em **25/11/2026 08:28** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 25/11 08:28 · sessao 1250193dc50b · A-TELA-DO-DIA-LE-UM-JUIZ · construtor · origem=Code

Implementei a opção A sobre `7f9a20167`. Na competência entregue, K6 omite o sem par da leitura do juiz e da impressão. A entrega sozinha rejulga a célula cuja impressão carregava o sinal. Fato novo também volta a entrar pelo caminho normal. A fórmula da impressão e os escritores existentes atendem aos dois casos. `ponto/services/cartorio.py:1234`, `:1506`, `:1523`, `:1595`, `:2029`.

### Efeito da cura

| Item | Resultado |
|---|---|
| **(i) Guarda e avulso** | Apaguei `_foto_entregue`, os dois pulos, `pulados_foto_entregue`, seu leitor no avulso e sua coluna no resumo do comando. Alterei `_julgar_colab_corpo`, `contadores`, `julgar_celula` e `processar_cartorio.Command.handle`. Em `apply_`, o avulso consulta `aplicar_precedencia_na_celula` mesmo quando a impressão pula o julgamento. Se a precedência falta ou mudou, a porta salva `ata` e emite `EventoCelula`. Se é igual, a porta retorna sem save. `ponto/services/cartorio.py:1258`, `:1372`, `:1523`, `:2124`, `:2216` · `ponto/management/commands/processar_cartorio.py:66` · `ponto/portas/celula.py:1242`. |
| **(ii) Três inversões da v8** | Os dois casos com acusação aberta exigem uma relavra, um save, `ata.sem_par=[]`, `concorde/ok` e repetição sem save, inclusive em dois avulsos quando a precedência já estava gravada. O caso `nunca_bateu` tem impressão sem sinal desde a origem. Na entrega, o primeiro avulso pula a lavra e salva só a precedência faltante. Lote e avulso seguintes fazem zero saves, e D5 completa o status em uma passada. `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:126`, `:148`, `:155`, `:299` · `ponto/services/cartorio.py:1369`. |
| **(iii) Discriminante** | A impressão que nunca carregou sinal faz zero saves na entrega e conserva a célula inteira. O teste exige também zero julgamentos e carimbos. Assim ele pega o mutante que força o julgamento mesmo quando a porta de gravação continua idempotente. `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:163` · `ponto/portas/celula.py:1227`. |
| **(iv) Sinal inerte** | Sim. Na aberta, o teste prova que `E` entrou nos bytes mesmo com `ata.sem_par=[]`. Na entrega, só a impressão diverge. O save inclui exatamente `julgada_em`, `impressao`, `ausencia`, `tem_entrada`, `tem_saida` e `insumos_em`. Dispara o `post_save` do Django. Nesse caso, a lavra devolve `[]`, então o save não cria `EventoCelula` nem trilha `veredito_celula`. O teste confere também chamados, `FechamentoMensal`, `DiaPago` e hash do TXT antes e depois. `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:180`, `:196` · `ponto/portas/celula.py:978`, `:994`, `:1113`, `:1236`. |
| **(v) Batida nova** | Os testes passam pelos serviços reais `julgar_colab` e `julgar_celula`, com `ExportacaoDominio` real. Conferem que o pareador ainda sinaliza `E`, a batida nova entra na impressão e o sem par fica fora dela. Para isolar o save da célula, duplei a abertura do chamado apenas na lavra inicial em aberta. `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:86`, `:93`, `:106`, `:227`. |
| **(vi) Chamado aberto** | Com lâmpadas presentes e sem pendência, a ata sem o PK dá o marco `batida_sem_par` como aceso. O cartório tenta reconciliar pelo gate existente. Se houver fio vivo, a ordem é lavra, E2 e tentativa de fecho. O outbox também tem consumidor de lastro. O estado final depende do gate e, se houver outros marcos cobrados, do lastro deles. `ponto/services/lastro.py:91`, `:219`, `:468` · `ponto/services/cartorio.py:1667`, `:1054` · `chamados/models.py:661`, `:702`, `:1354` · `chamados/reconciliador.py:1522`. |
| **(vii) Retroativo** | A lista vazia faz o comando seguir pelo ramo comum, com `entrada_sem_par=False` e `saida_orfa=False`. Só `FURO_SEM_COBRANCA` segue como candidato nesse ramo, sujeito às guardas existentes. A emissão por PK continua tendo leitor e uso para atas que ainda acusam. `ponto/management/commands/emitir_furo_retroativo.py:98`, `:111`, `:130`, `:157`. |
| **(viii) Procedentes 2 e 3** | Completei o dublê S2/S3 com `ata={}`, restaurei a prova do sinal inerte no hash e deixei as lápides L-111 no sítio da guarda e junto à classificação, onde saíram o filtro tardio e `posicoes_sem_par`. Os testes v3, v6 e v7 passaram a cobrar o pulo pela impressão. O teste v7 exige uma única impressão e nenhuma classificação na repetição. `chamados/tests/test_e3_pos_suite_sem_banco.py:139` · `ponto/services/cartorio.py:1258`, `:1597` · `ponto/tests/test_a_tela_v3_sem_par.py:183` · `ponto/tests/test_a_tela_v6_sem_par_memoria.py:137` · `ponto/tests/test_a_tela_v7_impressao_um_significado.py:38` · `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:180`. |

### Escritores e leitores

- **`CelulaDia.impressao`**. `carimbar` compara e grava. `desfazer_julgamento_da_futura` lê para trilha e limpa. O cartório compara no normal e em `nunca_bateu`; D5 reaproveita as comparações. A auditoria e as fotos leem o valor gravado. `ponto/portas/celula.py:1186`, `:1202`, `:1218` · `ponto/services/cartorio.py:1372`, `:1523` · `ponto/management/commands/celula_veredito_velho.py:110` · `colaboradores/services/feriado_do_posto.py:428`, `:453` · `chamados/services/foto_e4b.py:247`, `:269` · `bin/snapshot_o118_col221.py:18`.
- **`ata.sem_par`**. O cartório monta a lista em curso e `ata_do_dia` a leva à porta `lavrar_veredito`, que grava a ata. A limpeza do julgamento futuro pode apagá-la junto com a ata. `sem_par_da_ata` valida a chave para o lastro, a emissão parcial, o retroativo e o comando `supra_juiz`. O lastro alimenta a premissa do chamado, a entrada sincronizada e o fechador. A premissa pode ler a ata em curso. `ponto/services/cartorio.py:818`, `:1612`, `:2006`, `:2101` · `ponto/portas/celula.py:1127`, `:1202` · `ponto/services/lastro.py:81`, `:91` · `ponto/management/commands/emitir_furo_retroativo.py:99` · `ponto/management/commands/supra_juiz.py:209`, `:263` · `chamados/juizes.py:452` · `chamados/models.py:1346` · `ponto/services/entrada_sincronizada.py:26` · `ponto/management/commands/detectar_ausencias.py:405`.
- **`ata.precedencia`**. `aplicar_precedencia_na_celula` prepara o valor. `carimbar_precedencia` lê para comparar e grava a chave. `lavrar_veredito` lê a chave anterior para carregá-la na ata nova. As fotos de auditoria também incluem a ata. `ponto/services/cartorio.py:2156` · `ponto/portas/celula.py:106`, `:1086`, `:1242` · `colaboradores/services/feriado_do_posto.py:428` · `chamados/services/foto_e4b.py:247`, `:269`.

### RED e mutantes para o Code

Na base, a falha esperada nos dois casos de entrega com acusação e nos dois de batida nova é `(julgadas, carimbadas, emitidos) = (0, 0, 0)` em vez de `(1, 1, 0)`. O inerte cai na mesma asserção. O caso `nunca_bateu` cai na presença de `ata.precedencia`. O discriminante é controle verde nas duas versões. Execução ORM **[nao verificado]**. `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:53`, `:129`, `:163`, `:214`, `:313`.

```bash
export PYTHONDONTWRITEBYTECODE=1
alvo=ponto.tests.test_a_tela_v8_entregue_nao_le_sem_par.EntregueNaoLeSemParTest

python3 manage.py test --settings=config.settings_test "$alvo"

A_TELA_V9_VARIANTE=base python3 manage.py test --settings=config.settings_test \
  "$alvo.test_RED_entrega_sozinha_relavra_acusacao_uma_vez_no_lote" \
  "$alvo.test_RED_lote_entregue_relavra_e_dois_avulsos_conservam_precedencia" \
  "$alvo.test_RED_nunca_bateu_entregue_carimba_precedencia_uma_vez_e_completa_status" \
  "$alvo.test_entrega_sem_sinal_na_impressao_pula_sem_save" \
  "$alvo.test_RED_entrega_com_sinal_inerte_grava_so_o_carimbo" \
  "$alvo.test_RED_fato_novo_entregue_rejulga_no_lote" \
  "$alvo.test_RED_fato_novo_entregue_rejulga_no_avulso"

A_TELA_V9_VARIANTE=congela_acusacao python3 manage.py test --settings=config.settings_test "$alvo.test_RED_entrega_sozinha_relavra_acusacao_uma_vez_no_lote"
A_TELA_V9_VARIANTE=le_sem_par_entregue python3 manage.py test --settings=config.settings_test "$alvo.test_RED_entrega_sozinha_relavra_acusacao_uma_vez_no_lote"
A_TELA_V9_VARIANTE=forca_entregue python3 manage.py test --settings=config.settings_test "$alvo.test_entrega_sem_sinal_na_impressao_pula_sem_save"
A_TELA_V9_VARIANTE=ignora_sinal_inerte python3 manage.py test --settings=config.settings_test "$alvo.test_RED_entrega_com_sinal_inerte_grava_so_o_carimbo"
A_TELA_V9_VARIANTE=nb_forca_entregue python3 manage.py test --settings=config.settings_test "$alvo.test_RED_nunca_bateu_entregue_carimba_precedencia_uma_vez_e_completa_status"
A_TELA_V9_VARIANTE=avulso_pula_precedencia python3 manage.py test --settings=config.settings_test "$alvo.test_RED_nunca_bateu_entregue_carimba_precedencia_uma_vez_e_completa_status"
```

O mutante `forca_entregue` liga `forcar` para todo o lote quando há uma célula entregue. O discriminante cobra zero classificações pelo contador `julgadas`, mesmo quando `carimbar` evita o save. Os demais mutantes exercitam o congelamento da acusação, a leitura indevida na entregue, a perda do sinal no hash aberto, a passagem forçada em `nunca_bateu` e o avulso que suprime a precedência após um pulo. `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:33`, `:177`.

**Verificação local.** O censo real da árvore final deu **9 pareadores e 81 derivações**, com B1 em **`[1316, 1469]`** e o restante do registro conferido. Os oito fontes e os seis mutantes compilam, `git diff --check` passou e o diff acrescenta **0 `except`**. A suíte, os seis selos de AST e os resultados ORM ficam para o Code **[nao verificado]**. `core/medida_turno_do_par.py:448` · `core/tests/test_medida_turno_do_par.py:321`, `:546`, `:562` · `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:33`.

**Condição de pouso.** Estoque por empresa e competência, igualdade dos hashes exportados na sombra e impacto a encaminhar como Pauta DP **[nao verificado]**. O caso (iv) cobre uma fixture de banco. `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:196` · `docs/LEIS.md:125`.

**Pergunta de lei.** A queda da acusação só pela entrega deve bastar para encerrar o chamado aberto de `batida_sem_par` pelo lastro atual? A L-061 manda consumir a ata e o corte da A-TELA trata da acusação. A regra específica de destino desse chamado na entrega fica **[nao verificado]**. `docs/LEIS.md:95` · `docs/CORTES.md:640`.

## 25/11 08:24 · sessao 3c49b25d7d72 · PAR-RELAMPAGO-A-LISTA-DO-ADMIN · leitura · origem=Code

# PAR-RELAMPAGO-A-LISTA-DO-ADMIN — plano de construção

A cópia contém `detectar_par_relampago --dias 2 --apply --retratar`. O plano é trocar a escrita do cron pela lavra dos dois ramos na lista do admin, levar as 20 batidas abertas à decisão humana e encaminhar as 51 exportadas ao DP. Identidade Git do HEAD, execução dos testes e valores de produção do DIFF [nao verificado]. A contagem de 71 vem do anexo fornecido. `app/config/crons.py:605` · `app/docs/BACKLOG.md:2308` · `respostas_2511_0635.txt: item 4` · `respostas_2511_0815.txt: item 2` · `DADO RASPADO:3–14`

## 1. Pauta DP

**A porta única de criação é `pautas.services.escrever`. Não existe uma porta única de negócio para “fato em exportada com pago × devido”.** `escrever` recebe autor, destinatário, tipo e ID da âncora, texto e, quando houver, pauta pai. Faz `Pauta.objects.create` em cada chamada. Dois cliques nessa função sozinha geram duas pautas. O modelo já tem âncora `dia` (`colab:AAAA-MM-DD`) e `competencia` (`empresa:AAAA-MM`). Tipo do fato e números entram no texto, pois a Pauta não tem colunas para eles. `app/pautas/services.py:152–189` · `app/pautas/models.py:27–41,67–108`

| Caminho na cópia | O que emite e como deduplica |
|---|---|
| `pauta_pago_x_devido_exportada` | É o precedente direto **sem alterar o gravado**. `Command._medir` lê K6, o `FechamentoMensal` gravado e o cálculo atual. `Command._escrever` usa âncora de competência, marcador e PK do colaborador na cabeça, e pares pago/devido por campo nas filhas. A trava da pessoa serializa a consulta e a criação. A cabeça feita pelo DP conta para dedup, a cancelada não. Só emite se houver diferença, e a entrada é uma lista de colaboradores, sem dia ou PK de batida. `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:107–148,169–213,239–252` · `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:244–269` |
| `regen_exportada` | `regenerar_em_exportada` **altera** células, `FechamentoMensal` e `DiaPago`. Seu consumidor `_trabalhar_o_par`, em `passivo_validado`, chama `_pauta_da_competencia` quando o gravado muda. Esse emissor recebe os números antes/depois, usa âncora de competência e procura uma cabeça pelo marcador e pelo colaborador. É um fluxo de retificação, inaplicável à restauração bloqueada desta obra. `app/ponto/portas/regen_exportada.py:174–225` · `app/chamados/services/passivo_validado.py:157–175,231–245,490–516` |
| `pautas_dos_discordantes`, `esmeril_espelho` e limbo | `sincronizar` dos discordantes chama `escrever` para o DP por dono, colaborador e competência, com datas e dedup entre pautas vivas. `esmeril_espelho --apply` lavra o retrato. O sincronizador separado `pautas_do_esmeril` chama `escrever` para **Supervisão**, por posto. `pauta_dp_do_limbo_exportado` é o precedente de âncora **dia**, com dedup por marcador e motivo, inclusive em pautas feitas ou canceladas. Esses três fluxos não fornecem o par pago/devido desta obra. `app/ponto/services/pautas_dos_discordantes.py:101–120,168–213` · `app/ponto/management/commands/esmeril_espelho.py:12–17` · `app/ponto/services/pautas_do_esmeril.py:52–79` · `app/chamados/services/fabrica_da_celula.py:248–285` |

**Para as 51**, proponho um emissor específico do par que chame `escrever`. A entrada leva tipo do fato, empresa, competência, colaborador, dias, PKs das batidas e da trilha de origem, e pago/devido por rubrica, com unidade e referência da medição. A cabeça pode agrupar por colaborador e competência, desde que as filhas identifiquem todos os fatos cobertos e um pedido posterior por outro fato não seja absorvido pelo dedup. O dia identifica a ocorrência. O par de números medido pelo fechamento é da competência. O histórico de pagamento efetivo no Domínio [nao verificado]. O precedente chama de “pago” o gravado da folha e pede conferência no Domínio. `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:53–67,169–177,239–252` · `app/ponto/portas/regen_exportada.py:62–84` · `CLAUDE.md:418–429` · `DADO RASPADO:15–86`

**Quem calcula** é `fechamento.recalcular_fechamento_mes(..., colaborador_ids=[pk], somente_leitura=True)`, com `lavrar_dias_pagos` desativado. Para medir **se restaurar**, o cenário precisa repor os PKs na sombra pela porta de Batida, deixar o `post_save` reconstituir os insumos e só então rodar o fechamento real. O fechamento carrega as apuráveis e a ata da célula. Rodar o comando atual sobre o estoque como está calcula com as retratações ainda vigentes. O cenário de sombra permite medir sem restaurar em produção. O lote precisa prestar contas dos 51 PKs mesmo com delta zero ou fonte indisponível. Zero medido vai como igualdade, e valor ausente fica explícito como tal. Os valores e a disponibilidade das fontes por caso [nao verificado]. `app/ponto/services/fechamento.py:457–477,628–642,751–765,1204–1209` · `app/ponto/turnos.py:2877–2904` · `app/ponto/signals.py:85–112,131–135` · `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130–144`

## 2. Escritor e leitor da lista

**C8 hoje** segue `detectar_vinculo_divergente.Command.handle` → `vinculo_na_lista.lavrar` → `MetricaSnapshot` → `vinculo_na_lista.colabs_em_revisao` → `cadastro_realidade._vinculo_divergente` → `lista` → tela. A chave C8 é `('vinculo_divergente', 'frota', None, hoje)` com `update_or_create`. `colabs_em_revisao` tira da leitura a acusação cujo posto mudou ou cujo colaborador saiu do recorte. C8 não apaga a lavra ao decidir. `app/ponto/management/commands/detectar_vinculo_divergente.py:32–52` · `app/ponto/services/vinculo_na_lista.py:21,112–154` · `app/escala/services/cadastro_realidade.py:393–402,976–982` · `app/escala/views.py:511–529`

**Para o par**, proponho `ponto/services/par_relampago_na_lista.py::lavrar` e `pendentes`, lidos por uma nova `_par_relampago` em `cadastro_realidade.lista`. Chave do snapshot proposta: `('par_relampago', 'colab', colab_pk, dia_do_fato)`. No `detalhe`, par novo usa os dois PKs ordenados, e retratação histórica usa PK da batida mais PK da trilha do cron. O identificador da lavra inclui um `escopo_id` preenchido, coberto pela restrição existente. A leitura consulta os pendentes de todo o recorte visível, incluindo dias fora da janela do cron. O fechamento do sinal é a decisão em trilha, lida pela tela e pelo escritor. O snapshot continua sendo a evidência da detecção. `app/inteligencia/models.py:81–109` · `app/core/models.py:285–316` · `app/escala/services/cadastro_realidade.py:1023–1043,1053–1088,1203–1223`

**Os ramos publicam juntos hoje? Não.** `pares_relampago` chega a `_retratar` ou a `ChamadoColaborador.abrir`. `varrer_pares_embutidos` só roda sob `--retratar` e, no apply, retrata os dois membros. Na obra, ambos entregam candidatos ao mesmo `lavrar`, com dedup pelos PKs e o mesmo `par_completo`. Conservaria o critério atual do embutido para preservar sua cobertura de detecção. `app/ponto/management/commands/detectar_par_relampago.py:275–314,344–401` · `app/config/crons.py:2152–2156`

## 3. Os três atos do admin

**As três portas de negócio específicas ainda não existem.** A proposta é um POST da lista que revalide identidade do sinal, empresa visível, permissão `regularizar_batida`, estado atual e motivo obrigatório. O ato é transacional e só confirma com trilha gravada. Há precedente para exigir o retorno de `core.observ.evento` dentro do `atomic`, já que a trilha padrão é best-effort. `CLAUDE.md:32–36` · `app/ponto/views.py:3439–3473` · `app/ponto/services/retratar_humano.py:122–171` · `app/core/observ.py:94–137` · `app/ponto/portas/celula.py:1178–1212`

| Ato | Porta e resultado propostos |
|---|---|
| **(a) Ratificar** | Registrar `ratificar_retratacao_cron` no PK da `Batida`, ligando à trilha original. Antes: retratada, atribuída ao cron, decisão pendente. Depois: mesma batida e mesmo carimbo, ratificada pelo usuário, com motivo. `retratar_humano.retratar` retorna cedo se já estiver retratada e `registro_batida.anotar` pula alterações sem efeito. A ratificação precisa de ato próprio de auditoria. Para a autoria aparecer também no calendário, ampliar `retratar_humano.decisao_de`, que hoje só lê `retratar_batida_admin`. `app/ponto/services/retratar_humano.py:80–102,150–185` · `app/ponto/registro_batida.py:255–283` · `respostas_2511_0635.txt: item 4` |
| **(b) Restaurar** | Porta separada `restaurar_retratacao_cron`, proposta, mantendo a trava de `desfazer`. Em aberta, usa `registro_batida.anotar` na batida original com `aprovada=True`, `retratada_em=None` e trilha própria. O `post_save` chama o cartório e o núcleo. A porta chama também `fechamento.recalcular_por_evento` uma vez, que agenda no commit o fechamento e, pelo recálculo, a lavra de `DiaPago`. `anotar` sozinho não agenda esse recálculo. Em exportada, consultar `competencia_do_fato` e `competencia_entregue` antes da mutação, recusar a restauração e encaminhar pela Pauta do item 1. Se a medição em sombra ainda faltar, o pedido auditado fica pendente de encaminhamento até a pauta com os valores ser gravada. A porta genérica de evento hoje pode recalcular exportada. `app/ponto/services/retratar_humano.py:30–53,190–209` · `app/ponto/registro_batida.py:255–302` · `app/ponto/signals.py:19–36,85–112,131–135` · `app/ponto/services/fechamento.py:1204–1254,1615–1617,1796–1840` · `app/folha/export.py:556–594` |
| **(c) Manter as duas** | Registrar `manter_par_relampago` com a identidade dos dois PKs, estado das marcas antes/depois, usuário, instante e motivo. Proponho indexar a linha pelo menor PK sob o modelo de auditoria `ParRelampago` e conferir **os dois** PKs no JSON. A mesma consulta de decisões, em lote, serve ao leitor e ao detector. Assim, fechar A/B não fecha A/C nem o dia inteiro, e a próxima varredura pula A/B. `LogAuditoria` já tem os campos necessários. Nenhum campo ou modelo novo é necessário para esse desenho. `app/core/models.py:285–316` · `app/core/observ.py:94–137` · `app/ponto/management/commands/detectar_par_relampago.py:86–99,290–314` · `respostas_2511_0635.txt: item 4` |

O RED da restauração deve cobrir a corrida com a **primeira entrega**. K6 é uma consulta e a geração atual lê a folha antes de registrar a entrega. Uma trava só na linha de `Batida` não serializa os dois atos. O plano inclui coordenar a geração e a restauração pela mesma empresa/competência, cobrindo a relavra financeira, com teste da ordem dos commits. `app/folha/export.py:580–594` · `app/folha/views.py:209–214,264–302` · `app/folha/models.py:173–188` · `app/ponto/services/fechamento.py:1820–1840`

## 4. Desligamento e censo dos órfãos

- **Sai do cron** `--retratar` em `app/config/crons.py:605`. No comando, saem `_melhora_estrita` (`154–155`), `_alvo_retratavel` (`199–214`), `_retratar` (`217–226`), `escolher_retratavel` (`229–248`), as duas flags (`322–323`), a implicação de `--so-retratar` (`333–334`), a seleção e o desvio de escrita do primeiro ramo (`355–367`) e as duas chamadas de escrita do embutido (`399–400`). A condição `if o['retratar']` e os contadores/mensagens de retratação dão lugar à lavra. Saem também o import, `MODULO_ORIGEM` e a emissão de chamado do primeiro ramo. `app/ponto/management/commands/detectar_par_relampago.py:17,154–155,199–248,317–404` · `CLAUDE.md:192`
- **Têm leitor no desenho escolhido** `varrer_pares_embutidos`, `par_embutido_retratavel`, `_par_embutido_retratavel_geometria`, `_score_geometria` e a guarda de procedência/teto hoje chamada `motivo_para_nao_retratar`. Passam a sustentar a seleção do **sinal embutido**. A seleção de `pares_relampago` também tem leitor em `tripwire_celulas`. O piso de 60 s tem leitor em `turnos`; o teto de 360 s tem leitor no motor de pontualidade. `app/ponto/management/commands/detectar_par_relampago.py:63–196,251–314` · `app/ponto/management/commands/tripwire_celulas.py:251,287–296` · `app/ponto/turnos.py:117–131` · `app/ponto/motor_calculo_v2.py:1154–1168`
- **Apagar ou substituir os testes do retratador extinto.** `test_eco_retratavel` e `test_empate_conservador_vira_none`, em `test_par_relampago_v3.py`, só testam `escolher_retratavel`, inclusive por herança de `V4ParEmbutido`. Reaproveitar as duas situações no RED do comando, verificando sinal e batidas preservadas. `test_o_comando_CONSULTA_a_guarda_antes_de_retratar` perde seu alvo. `test_o_skip_nao_e_calado` só fica se continuar cobrindo a guarda do embutido. Os testes puros de procedência e teto ficam para o embutido, com as mensagens de asserção voltadas à candidatura. O teste isolado que só subtrai dois horários fixos pode sair quando o caso de 120 s estiver coberto pelo cenário do comando. `app/ponto/tests/test_par_relampago_v3.py:48–92` · `app/ponto/tests/test_guardas_par_relampago.py:48–107,161–183,213–246`
- **Inverter a caracterização do comando.** Em `test_par_relampago_pelo_turno.py:66–74`, trocar `[DRY-RETRATA-PAR]` por previsão do sinal. No apply, provar uma lavra, duas marcas preservadas, zero trilha de retratação do cron e zero chamado novo desse emissor. Ficam o teste de candidato só após encerramento, o papel `prazo`, o juiz registrado e os positivos/negativos do embutido. Atualizar o mapa do selo de relógio e as linhas congeladas do censo AST que se deslocarem. `app/ponto/tests/test_par_relampago_pelo_turno.py:53–92` · `app/ponto/tests/test_par_relampago_v3.py:75–92` · `app/escala/tests/test_selo_grade_pede_o_instante.py:25–35` · `app/core/tests/test_medida_turno_do_par.py:199,292,562–566`
- **Ficam as regressões independentes e o acervo.** Piso, pontualidade, oráculo, exclusão das retratadas do juízo, relavra da retratação humana e recusa do `desfazer` para o cron continuam sendo contratos. Os chamados antigos ainda têm leitor no catálogo e no reconciliador. O `--so-retratar` de `reconciliar_grade` atua em chamados e segue fora da retirada. `app/ponto/tests/test_o316_par_de_um_minuto_nao_e_par.py:59–155,171–249` · `app/ponto/tests/test_par_relampago_nao_se_julga.py:43–92` · `app/ponto/tests/test_e6_oraculo_le_a_retratacao.py:33–61` · `app/ponto/tests/test_selo_b5d_retratada_fora_do_juizo.py:75–106` · `app/ponto/tests/test_retratacao_relavra_o_dia.py:33–46` · `app/ponto/tests/test_porta_retratar_batida.py:157–169` · `app/chamados/catalogo/premissa.py:54–56` · `app/chamados/reconciliador.py:182–224` · `app/ponto/management/commands/reconciliar_grade.py:72,298–316`

## 5. Estoque aberto e ordem segura

**Sim. As 20 entram como retratações históricas com decisão pendente.** O anexo dá empresa 2: `202607=3`, `202610=11`. Empresa 3: `202606=1`, `202608=3`, `202611=2`. São 20 batidas, e a trilha antiga identifica a batida retratada sem gravar os dois PKs do par. O comando não deve reconstruir o parceiro por proximidade. `DADO RASPADO:3–14,15–86` · `app/ponto/management/commands/detectar_par_relampago.py:217–226` · `respostas_2511_0635.txt: item 4` · `respostas_2511_0815.txt: item 2`

O comando proposto `lavrar_estoque_par_relampago`, com `--dry` por padrão e `--apply` explícito, recebe o recorte fechado, confere trilha, decisão posterior e K6 atual, e chama o mesmo `par_relampago_na_lista.lavrar` para as abertas. A prova exigida é dois applies, inclusive em datas de execução diferentes, com os mesmos IDs de snapshot, mesmas chaves e conteúdo, zero novos sinais no segundo apply e zero alteração de `Batida`. Depois de um ato humano, repetir o comando e rodar a varredura deve manter o sinal fechado. Essa prova da obra ainda está [nao verificado]. `app/ponto/services/vinculo_na_lista.py:112–123` · `app/inteligencia/models.py:95–105` · `app/ponto/services/retratar_humano.py:35–53` · `CLAUDE.md:179`

## 6. Dinheiro daqui em diante

A função real é **`recalcular_fechamento_mes(..., somente_leitura=True)`**. O instrumento existente é **`diff_frota.medir`**, que devolve `gravado`, `hoje` e `novo` e chama o fechamento real antes/depois do cenário com rollback. Antes do apply, medir na sombra o mesmo recorte e relógio sob a política antiga e sob a política que conserva as marcas, por empresa, competência e rubrica. O `medir` parte dos colaboradores com `FechamentoMensal` gravado. Casos fora desse universo precisam aparecer no placar de cobertura. O efeito observado [nao verificado]. `app/ponto/services/fechamento.py:457–477` · `app/ponto/services/diff_frota.py:45–98` · `CLAUDE.md:35–36,413–417`

| Leitor | O que passa a receber quando as duas marcas ficam apuráveis |
|---|---|
| Cartório | `batidas_apuraveis` alimenta a grade que monta a ata e o veredito. Cada marca ainda pode ocupar marco ou ficar órfã conforme a geometria. `app/ponto/services/cartorio.py:1184–1207,1459–1468,1898–1906` |
| `flip_automatico` | `avaliar_fila` deixa de excluir essas marcas pelo carimbo de retratação. Se forem candidatas e passarem pelas demais guardas, o comando ainda pode inverter o tipo via `flip_tipo`. `app/ponto/services/flip_auto.py:268–313` · `app/ponto/management/commands/flip_automatico.py:32–68` |
| `verificar_nucleo` | Compara `turnos_do_colab` ao `TurnoMaterializado`, reconstruído pela mesma função. As duas marcas entram na geometria. O efeito depende do turno resultante. `app/ponto/turnos.py:1850–1871` · `app/ponto/nucleo.py:40–59` · `app/ponto/management/commands/verificar_nucleo.py:16–41` |
| Export | A geração do TXT já usa o fechamento calculado em leitura e passa os mesmos objetos a `classificar_export` e `montar_linhas_export`. `classificar_export` também lê apuráveis diretamente nas guardas de férias com batida e fechamento zerado. `app/folha/views.py:209–214,264–270` · `app/folha/export.py:485–504,902–924` |

## 7. Censo para a obra A-TELA e sequência de construção

**A obra não precisa editar `app/ponto/services/cartorio.py` nem `test_a_tela_*`.** A restauração aberta usa o `post_save` existente e a comparação em sombra pode usar o mesmo caminho. O comentário histórico do cartório sobre o cron fica fora do patch funcional. Prova de merge com a outra obra [nao verificado]. `app/ponto/registro_batida.py:264–283` · `app/ponto/signals.py:85–112,131–135` · `app/ponto/services/cartorio.py:1184–1207` · `app/docs/BACKLOG.md:1985`

1. **RED primeiro.** Usar `app/ponto/tests/test_par_relampago_pelo_turno.py` para os dois ramos e acrescentar testes da lista, dos três atos e do estoque em `app/ponto/tests/` e `app/escala/tests/`. Cobrir replay, par com um PK em comum, ausência de motivo, falha de trilha, validação de empresa e export concorrente. Usar `app/pautas/tests/test_pauta_pago_x_devido_exportada.py` como precedente para zero, dedup, falha entre cabeça e filhas e integridade do gravado. `CLAUDE.md:28–36` · `app/ponto/tests/test_par_relampago_pelo_turno.py:53–92` · `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:130–144,185–269,285–302`
2. **Lavra e tela.** Criar `app/ponto/services/par_relampago_na_lista.py` e ligar `app/escala/services/cadastro_realidade.py` e `app/templates/escala/cadastro_x_realidade.html`. Implementar o POST em `app/ponto/views.py` e `app/ponto/urls.py`, com as portas humanas em `app/ponto/services/retratar_humano.py` e a restauração pela escrita de `app/ponto/registro_batida.py`. Exigir trilha no ato humano pelo escritor existente. `app/ponto/services/vinculo_na_lista.py:112–154` · `app/escala/services/cadastro_realidade.py:976–982,1053–1088` · `app/templates/escala/cadastro_x_realidade.html:31–32` · `app/ponto/services/retratar_humano.py:122–209`
3. **Pauta, comando de estoque e sombra.** Acrescentar o emissor em `app/pautas/` usando `services.escrever`, e o comando avulso em `app/ponto/management/commands/`. Exercitar a fronteira de entrega em `app/folha/views.py` e a porta de evento em `app/ponto/services/fechamento.py`. Medir usando `app/ponto/services/diff_frota.py` e provar integridade das três tabelas, inclusive `DiaPago` e os registros e hashes de `ExportacaoDominio`. `app/pautas/services.py:152–189` · `app/ponto/services/diff_frota.py:45–98` · `app/ponto/services/fechamento.py:1204–1254,1811–1840` · `app/folha/views.py:209–214,297–302` · `app/folha/models.py:173–188`
4. **Virada única do cron e dos leitores.** Depois do DIFF, pousar a publicação dos dois ramos junto com a reti

[truncado em 20000 de 21089 caracteres]

## 25/11 07:12 · sessao e84c66e51e83 · A-TELA-DO-DIA-LE-UM-JUIZ · verificador · origem=Code

**Parecer v8 em `7f9a20167`: PROCEDENTE, reprova.** A opção (b) retira o sem par dos insumos usados para julgar o dia entregue. A guarda vai além: congela toda célula com veredito no horizonte e TXT vigente, inclusive quando nunca houve sem par. Bloqueia correções que mudariam a impressão e pode interromper a releitura que o próprio cartório faz depois de emitir ou reconciliar um chamado. Há ainda uma regressão reproduzida no selo S2/S3, fora do delta. [Guarda](/home/ronald/codex_ws/7f9a201673976c4d96cd39d21436c26e73beca3e/app/ponto/services/cartorio.py:1258) · `app/ponto/services/cartorio.py:1083,1148,1515` · `app/chamados/tests/test_e3_pos_suite_sem_banco.py:139`

Os oito blobs alterados da cópia conferem com o delta anexado. Rodei 32 testes de memória, 35 da catraca e sondas em memória com os corpos extraídos do código. Ancestralidade Git, suíte ORM integral, linhas persistidas e incidência em produção **[nao verificado]**. `app/ponto/tests/test_e3_nb_impressao_memoria.py:56` · `app/core/tests/test_medida_turno_do_par.py:29` · `app/config/settings_test.py:4`

### 1. O que a opção (b) entrega

| Ramo | Leitura do sem par e gravação |
|---|---|
| **Lote normal, aberta** | A partir da competência `2026-11`, pela janela da empresa, passa `E`, `S` ou `ES` do pareador ao hash e ao classificador se o turno encerrou. Sinal inerte também entra no hash. A ata só lista os pks que o classificador acusou como furo pelo sem par. Turno encerrado sem acusação grava `[]`. Turno pendente ou julgado até o teto fica sem a chave. Se a lavra prossegue, grava veredito, protestos, ata, status/rótulo e carimbo. `app/ponto/services/cartorio.py:316,332,1229,1524,1596,1620,2015,2039` · `app/ponto/supra_juiz.py:547` |
| **Lote normal, entregue** | Com veredito no horizonte, incrementa `pulados_foto_entregue` e sai antes de ler o sinal para o dia, comparar o hash ou lavrar. Sem veredito, julga os demais insumos com os dois sinais falsos. Pode gravar a primeira lavra completa e emitir furo comum. No turno encerrado, a primeira lavra grava `ata.sem_par=[]` e hash sem `sem_par:`. `app/ponto/services/cartorio.py:1234,1258,1515,1530,1620,1978,2015,2039` |
| **Avulso** | Chama o mesmo lote com uma célula. Se a guarda contou a foto, também deixa de gravar `ata.precedencia`. Uma primeira lavra em avulso pode gravá-la. Há uma saída anterior à guarda: com `teto_de_hoje` e zero marcos vencidos, o corpo conta `cedo` e o avulso ainda pode gravar precedência. O vigia pré-filtra esse caso. `app/ponto/services/cartorio.py:1482,1487,2221,2229` · `app/ponto/portas/celula.py:1242` · `app/ponto/services/vigia_de_hora.py:172` |
| **`nunca_bateu`** | `imp0` usa batidas e chamados vazios e nenhum sem par. Em `apply`, na aberta dentro do horizonte, `_sem_par` pode ler a projeção vazia. Na entregue, devolve falso/falso sem consultar essa projeção pelo dia. A primeira lavra grava ata sem a chave, status e carimbo, com protestos vazios e sem emissão. O veredito é `nunca_bateu` ou o derivado para dia sem trabalho/cobertura integral. Na repetição entregue, a guarda precede o pulo por igualdade. `app/ponto/services/cartorio.py:1319,1360,1374,1381,1389,1414,1439` |
| **D5 / `_completar_status`** | Passa pela comparação do carimbo sem a guarda da foto. No normal, usa o sinal da aberta encerrada no horizonte e omite o da entregue. Em `nunca_bateu`, compara `imp0` sem ler sem par. Status presente conta `ja_lavrado`. Status ausente com carimbo atual e grade disponível ganha, em `apply`, só status e rótulo pela porta. Divergência fica em `status_fila_cartorio`. A entrada D5 encerra antes da fábrica e das segundas passadas. `app/ponto/services/cartorio.py:973,1050,1374,1515,1535` · `app/ponto/portas/celula.py:1113` |
| **Retroativo** | Antes do horizonte, a lavra omite o sinal. Numa entregue anterior ao horizonte, a guarda protege se há veredito e acusação gravada. O comando `emitir_furo_retroativo` lê a acusação da ata quando há `furo`. Na ausência dela, procura furo comum com falso/falso. Pode acionar o emissor sem relavrar a célula. Assim, uma acusação antiga preservada na ata continua legível pelo retroativo. `app/ponto/services/cartorio.py:332,1234,1258` · `app/ponto/management/commands/emitir_furo_retroativo.py:98,111,130,157` |
| **TXT invalidado sem substituto** | K6 fica falso. Uma foto lavrada já pela v8, sem sinal no hash, volta a julgar se agora entra `E`/`S` ou outro insumo muda. Se o hash continua igual, a passada comum pula. Foto v7 com hash que já contém `E`, `concorde` e `ata.sem_par=[]` pode continuar pulada com o mesmo sinal após a revogação. Reproduzi essa diferença em memória. `app/folha/models.py:143` · `app/folha/export.py:588` · `app/ponto/services/cartorio.py:1234,1535,1539` · `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:96` |

Em **lote misto**, uma célula aberta pode disparar a projeção dos turnos da janela inteira. Os mapas de sinais e de batidas acusáveis podem incluir datas entregues, pois o produtor filtra pelo horizonte. O que K6 impede é usar o sinal como insumo do hash e do classificador **daquele dia entregue**. `app/ponto/services/cartorio.py:1194,1214,1234,1265,1624` · `app/ponto/supra_juiz.py:101,116,129`

### 2. Quais fotos a guarda passou a congelar [nao verificado]

O conjunto novo é **TXT vigente + competência no horizonte + qualquer veredito preenchido + nenhuma acusação em `ata.sem_par`**. Inclui ata sem chave, ata vazia, dia sem sobra, sobra inerte, primeira lavra feita já na entregue e `nunca_bateu`. Na v7, essas células podiam relavrar se o hash divergisse ou a passada fosse forçada e o sem par atual não acionasse a proteção tardia. Na v8, a guarda vence até `forcar=True` e `toque=True`. `app/ponto/services/cartorio.py:1258,1377,1381,1515,1535` · `app/chamados/services/cobrar_dia.py:41`

| Fato que chega após a entrega | Efeito na célula com veredito dentro do horizonte |
|---|---|
| **Batida regularizada, nova ou retratada** | O fato pode ser registrado e acordar o cartório. A lavra anterior fica, inclusive se a primeira batida encerra a condição `nunca_bateu`. A guarda vem antes do teste que descartaria esse veredito como atual. `app/ponto/signals.py:85,131` · `app/ponto/services/cartorio.py:1321,1515,1535` |
| **Chamado, mudança de estado ou fio** | Chamado e fio podem mudar pelas próprias portas. O cartório não atualiza veredito, protestos, ata, status ou impressão por causa dessa mudança. A primeira lavra entregue também pode emitir ou reconciliar um furo comum e depois ter a sua **própria** segunda passada bloqueada pelo veredito que acabou de gravar. `app/chamados/signals.py:189,218` · `app/ponto/services/cartorio.py:1079,1135,1503,1515,1685,1978,2019` |
| **Cadastro, vínculo ou DNA** | A porta pode gravar DNA corrigido e o avulso forçado deixar a ata anterior. A porta de regeneração em exportada abre a escrita do cadastro e chama o recálculo, mas o avulso chamado pela regeneração cai na guarda. O chamador pode até registrar o dia em `julgados` apesar do pulo. Impacto financeiro efetivo **[nao verificado]**. `app/ponto/portas/regen_exportada.py:174` · `app/ponto/portas/celula.py:390,456,479` · `app/escala/services/leitor_celula.py:379` |
| **Cobertura ou justificativa** | A cobertura nova pode existir na origem e na grade. A guarda impede atualizar `ausencia`, ata, carimbo e status na célula, inclusive em `nunca_bateu`. `app/ponto/signals.py:130` · `app/ponto/services/cartorio.py:1344,1374,1381,1439,1475,1515` |
| **Status do dia** | Status presente permanece, mesmo se ficou velho. Status ausente entra pelo D5 quando o carimbo casa. Se insumos mudaram ou a foto foi lavrada com `E`/`S` que K6 agora omite, o D5 manda à fila e a guarda impede o cartório de drená-la enquanto houver TXT vigente. A decisão direta de admin tem sua própria porta. `app/ponto/services/cartorio.py:980,1377,1515,1535` · `app/ponto/services/veredito_celula.py:357` |

Na sonda com os corpos v7 e v8, fiz mudanças que divergiam do hash anterior em batida, primeira adesão, chamado, DNA e cobertura. A v7 relavrou os cinco casos. A v8 preservou as cinco fotos e o D5 enfileirou as cinco quando o status estava ausente. A L-124 manda fato novo rejulgar normalmente em qualquer competência. A L-113 e **DINHEIRO item 3** preservam a entrada da correção provada. O corte da A-TELA restringe a acusação pelo sem par. A guarda da v8 bloqueia também os outros fatos, antes de verificar se mudaram. `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:16,96` · `app/ponto/services/cartorio.py:1258,1381,1515` · `app/docs/CORTES.md:640` · `app/docs/LEIS.md:145,155` · `CLAUDE.md:425`

### 3. Escritores, leitores e significado

| Campo | Quem escreve e quem lê |
|---|---|
| **`CelulaDia.impressao`** | `carimbar` é o único gravador de hash e compara o valor anterior. Recebe `imp0` de `nunca_bateu` e `imp` do normal. `desfazer_julgamento_da_futura` lê para trilha e limpa a coluna na mesma porta. Os dois ramos do cartório são os únicos comparadores de produção com hash recalculado. D5 usa essas comparações. `celula_veredito_velho` e `snapshot_o118_col221` copiam o valor. `feriado_do_posto` e `foto_e4b` comparam fotos que o incluem. `processar_cartorio` reexporta a função e delega ao cartório, sem cálculo próprio. `app/ponto/portas/celula.py:1149,1186,1202,1218` · `app/ponto/services/cartorio.py:1374,1439,1524,1535,2039` · `app/ponto/management/commands/celula_veredito_velho.py:110,144` · `bin/snapshot_o118_col221.py:18` · `app/colaboradores/services/feriado_do_posto.py:428,451` · `app/chamados/services/foto_e4b.py:116,145,247,269` · `app/ponto/management/commands/processar_cartorio.py:20,65` |
| **`ata.sem_par`** | O cartório reúne os pks na ata em curso e `ata_do_dia` monta a chave para a gravação por `lavrar_veredito`. A substituição da ata inteira pode retirar a chave, assim como a limpeza do julgamento futuro. O carimbo de precedência preserva as demais chaves. A projeção do julgamento em curso leva a ata aos leitores de premissa. Leitura direta da chave: guarda do cartório, por valor truthy, e `lastro.sem_par_da_ata`, que valida a lista. Consumidores do leitor: emissão parcial do cartório, emissão retroativa, comando `supra_juiz` e lastro. Pelo lastro, chega à premissa de chamados, à entrada sincronizada e ao fechador de `detectar_ausencias`. `app/ponto/services/cartorio.py:67,777,818,1620,2016,2111` · `app/ponto/portas/celula.py:1027,1086,1127,1202,1249` · `app/chamados/juizes.py:452` · `app/ponto/services/lastro.py:81,91,219,239,329,468,497` · `app/ponto/management/commands/emitir_furo_retroativo.py:99` · `app/ponto/management/commands/supra_juiz.py:209,263` · `app/chamados/models.py:1346` · `app/ponto/services/entrada_sincronizada.py:26` · `app/ponto/management/commands/detectar_ausencias.py:405` |
| **`pulados_foto_entregue`** | `contadores()` inicializa. Só os ramos normal e `nunca_bateu` incrementam. O avulso lê para decidir a precedência e `processar_cartorio` imprime no resumo. Os testes v3/v6/v7/v8 conferem o valor. A segunda passada forçada usa o `out` principal. A releitura pós-emissão tem outro `out` e seu incremento fica fora do total publicado. O contador mede a passagem pela guarda, inclusive para dias sem sinal. `app/ponto/services/cartorio.py:1083,1147,1382,1516,2138,2229` · `app/ponto/management/commands/processar_cartorio.py:66` · `app/ponto/tests/test_a_tela_v3_sem_par.py:262` · `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:145` · `app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:44` · `app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:210` |

Há **uma fórmula** de impressão. Em novas lavras do mesmo ramo, a comparação e a porta usam os mesmos bytes. O estoque ainda pode carregar a semântica v7, que incluía o sinal bruto na entregue, e a v8 compara pela leitura sujeita ao K6 atual. Na ata, `[]` continua dizendo que a lavra não acusa nenhum pk. Para o lastro, com lâmpadas e sem pendência, isso também pode dar o marco cobrado como aceso **quando o sinal foi omitido por K6**. A guarda aceita qualquer valor truthy na chave, inclusive um valor que o leitor do lastro rejeitaria como malformado. `app/ponto/services/cartorio.py:513,601,1234,1262,1530,1622,2039` · `app/ponto/services/lastro.py:81,91`

### 4. Destino dos achados da v7

- **Avulso comum após lote entregue: curado.** A guarda conta a foto antes da igualdade e o avulso preserva a precedência. A saída `cedo` continua sendo a exceção descrita acima. `app/ponto/services/cartorio.py:1482,1515,2229` · `app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:123`
- **Revogação do TXT: curada para nova lavra v8 cujo hash muda.** Segue aberta para o estoque v7 com sinal bruto já gravado e acusação vazia. A sonda confirmou pulo por igualdade nessa combinação. Estoque em produção **[nao verificado]**. `app/ponto/services/cartorio.py:1234,1535,1539` · `app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:146`
- **Sinal bruto e filtro tardio por posição: superados por (b).** A fila D5 continua possível e ganha um caso: foto aberta com `E`/`S`, ou foto bruta v7, seguida de K6 vigente e status ausente. O hash em aberto deixa de casar com o hash que D5 exige na entregue. `app/docs/RELATO.md:10` · `app/ponto/services/cartorio.py:980,1234,1262,1524,1535`
- **Catraca: curada. Mutante legado E3 sem alcançar a asserção: segue aberto.** `app/core/tests/test_medida_turno_do_par.py:321,562` · `app/ponto/tests/test_e3_nb_impressao_memoria.py:76`

### 5. Idempotência, linha de trilha e duplicatas

- **Dois lotes estáveis.** Em memória, com corpo e portas reais e persistência/trilha dubladas, normal e `nunca_bateu` conservaram o estado após a primeira lavra, com uma chamada de save e uma de `veredito_celula`. O segundo lote deu `pulados=1` na aberta e `pulados_foto_entregue=1` na entregue no horizonte. O D5 aplicado a status ausente acrescentou sua gravação e trilha próprias na sonda e depois deu `status_ja_lavrado=1`. `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:96` · `app/ponto/services/cartorio.py:980,1381,1515,1539` · `app/ponto/portas/celula.py:1113,1136,1227`
- **Lote seguido de avulsos.** Na entregue no horizonte, o caminho comum mantém a foto. Na aberta, o primeiro avulso pode adicionar precedência com save/outbox, mesmo quando o hash pulou o julgamento. A avulsa seguinte para de salvar se a precedência é igual. A ordem da primeira chamada também importa: começar por avulso pode produzir foto com precedência onde começar por lote deixa a chave ausente. `app/ponto/services/cartorio.py:1539,2182,2221,2229` · `app/ponto/portas/celula.py:994,1242`
- **Uma linha e zero duplicatas como garantia geral: não.** Reconciliação e releitura pós-emissão podem relavrar dentro do ato e gerar outra linha quando o resultado muda. A trilha comum é best-effort. Para chamado, a abertura consulta a chave de colaborador, módulo e dia sob a trava do colaborador, reutilizando o existente. Total persistido, concorrência e zero duplicatas em banco **[nao verificado]**. `app/ponto/services/cartorio.py:1083,1148` · `app/ponto/portas/celula.py:1136` · `app/core/observ.py:94` · `app/ponto/management/commands/detectar_ausencias.py:697` · `app/chamados/models.py:968`

### 6. L-111 e asserções adaptadas

- A busca integral na cópia deu **zero ocorrências** de `_retirar_sem_par_entregue` e `posicoes_sem_par` em código, testes, docs e selos. A assinatura e o filtro por posição saíram. Também não ficou lápide específica, embora a conduta da L-111 peça registro no sítio. `app/ponto/supra_juiz.py:280,547` · `app/ponto/services/cartorio.py:1258,1596` · `app/docs/LEIS.md:137` · `CLAUDE.md:192`
- A v3 manteve as asserções de igualdade da foto e mudou o contador esperado. A v7 trocou as expectativas de sinal bruto por sinal omitido e passou a exigir pulo pela foto. E3/NB manteve as asserções e recebeu dependências. A v6 perdeu uma prova ainda pertinente: **sinal inerte na aberta entra no hash**. O teste v8 verifica sinal inerte e hash presente, mas não que o hash incorpora aquele sinal. A antiga expectativa de só consultar K6 diante de protesto foi superada pela opção (b). `app/ponto/tests/test_a_tela_v3_sem_par.py:244` · `app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:13` · `app/ponto/tests/test_e3_nb_impressao_memoria.py:98,181` · `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:129` · `app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:98,108`
- **Quebra nova fora do delta.** Rodei o selo S2/S3 com os mesmos dois dublês de importação nas duas cópias. Na v7, passa. Na v8, os dois subcasos dão `AttributeError` por falta de `ata` no dublê antes de alcançar as asserções. Alcançada a guarda, o teste também precisa fornecer os insumos de empresa/horizonte que ela usa. O mutante E3 `classificacao_antes_guarda`, por sua vez, continua parando na montagem com `ValueError: substring not found`. `app/chamados/tests/test_e3_pos_suite_sem_banco.py:122,139,146,174,182` · `app/ponto/services/cartorio.py:1262,1381,1391` · `app/ponto/tests/test_e3_nb_impressao_memoria.py:76`

### 7. RED e mutantes

O RED v8 chama `julgar_colab` com ORM e emissor real, `julgar_celula` pelo avulso e `julgar_colab(completar_status=True)` pelo D5. Cobre a entrada de serviço do D5. O teste v3 é quem chama o comando Django. O cenário de lote entregue seguido de dois avulsos exige `emitidos=0`, deixando de fora a releitura depois de emissão real. `app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:74,81,123,131,138,168,186` · `app/ponto/tests/test_a_tela_v3_sem_par.py:265`

Os **cinco mutantes** falharam por `AssertionError` na sonda em memória, com controles verdes e um alvo por mutante. As falhas foram acusar sem par na entregue, perder o contador de foto, calar o sinal na aberta, deixar D5 sem completar e perder o contador de foto em `nunca_bateu`. A execução ORM desses cinco é **[nao verificado]**. `app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:27,108,119,132,177,210` · `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:118,129,137` · `app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:24`

A variante `base` fica inerte na suíte padrão, com `A_TELA_V8_VARIANTE` ausente. Quando selecionada, carrega por `git show` e troca as funções durante a classe. A cópia não tem `.git`, portanto esse caminho falha na montagem aqui. Pela leitura direta dos dois fontes v7, os equivalentes em memória dos REDs de foto e revogação falharam por asserção. Isso deixa a execução do `git show` e do RED ORM **[nao verificado]**. `app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:44,59,65,142,162`

### 8. Classes de reprovação e `except`

O delta tem **0 `except` adicionados e 0 removidos**. O `except` do avulso já existia. Produção continua com o mesmo classificador, o mesmo pareador, K6 e as mesmas portas. O delta não acrescenta segundo juiz, segundo escritor, relógio de produção, derivação paralela do sinal, trava de banco ou fallback. A mudança de ordem é da **guarda de negócio** em relação ao hash e à relavra. `app/ponto/services/cartorio.py:1214,1234,1248,1381,1515,1596,2039,2232` · `app/ponto/supra_juiz.py:129,280,547`

As classes observadas são **selo que deixa de alcançar a asserção**, **cobertura insuficiente do RED para correções e releitura entregue** e **hashes de semânticas diferentes convivendo no estoque**. A L-061 mantém o consumidor lendo a ata. Isso torna a ata congelada relevante mesmo com juiz e escritor únicos. L-073 e L-115 não ganham alteração de relógio nem de geometria no delta. A ordem real de pouso frente ao pré-requisito registrado no corte **[nao verificado]**. `app/chamados/tests/test_e3_pos_suite_sem_banco.py:139,182` · `app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:131` · `app/ponto/services/cartorio.py:1234,1482,1515,1535` · `app/docs/LEIS.md:95,100,153` · `app/docs/CORTES.md:640`

### 9. Catraca das linhas

**Sim.** `[1321, 1477]` bate com a adesão e o recorte de batidas em `_julgar_colab_corpo`. Os 35 testes da catraca passaram pela receita sem banco do próprio arquivo. O censo enc

[truncado em 20000 de 20231 caracteres]

## 25/11 07:10 · sessao 93986b17ca54 · - · - · origem=Ronald (tmux)

**Q7.** O RED entra por `julgar_colab` com o emissor real, `julgar_celula` no avulso e `julgar_colab(completar_status=True)` no D5. A fixture cria o TXT pelo ORM e a invalidação passa por `invalidar`. As asserções conferem a foto, os chamados de furo e os saves. O caso entregue testa emissão zero e a suíte não consulta `LogAuditoria`. [test_a_tela_v8_entregue_nao_le_sem_par.py:74](app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:74) · [test_a_tela_v8_entregue_nao_le_sem_par.py:123](app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:123) · [test_a_tela_v8_entregue_nao_le_sem_par.py:146](app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:146) · [test_a_tela_v8_entregue_nao_le_sem_par.py:168](app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:168)

Apliquei cada uma das cinco mutações reais uma vez ao corpo extraído. Os controles passaram e as cinco falharam por `AssertionError`, respectivamente no veredito entregue, no contador da foto normal, no protesto aberta, na elegibilidade D5 e no contador da foto `nunca_bateu`. A sonda usa os dublês v6/v7. As asserções ORM correspondentes estão nas linhas 132, 119/140, 108, 177 e 209–210. Execução ORM dos cinco mutantes **[nao verificado]**. [test_a_tela_v8_entregue_nao_le_sem_par.py:27](app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:27) · [test_a_tela_v6_sem_par_memoria.py:96](app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:96) · [test_a_tela_v7_impressao_um_significado.py:24](app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:24)

Sem `A_TELA_V8_VARIANTE`, a variante `base` é inerte. Com a variável, tenta `git show` e substitui as duas funções só no contexto da classe. Nesta cópia sem `.git` aquele comando falha antes das asserções. Carregando diretamente o corpo e o classificador da cópia v7 em memória, as asserções equivalentes da foto e da invalidação falharam contra a v7 e passaram contra a v8. A variante ORM completa **[nao verificado]**. [test_a_tela_v8_entregue_nao_le_sem_par.py:42](app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:42) · [test_a_tela_v8_entregue_nao_le_sem_par.py:65](app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:65) · [test_a_tela_v8_entregue_nao_le_sem_par.py:142](app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:142) · [test_a_tela_v8_entregue_nao_le_sem_par.py:162](app/ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:162)

**Q9.** A catraca bate `[1321, 1477]` para adesão e recorte. Rodei os 35 testes pela receita em memória do próprio arquivo. Todos passaram. O censo encontrou 9 pareadores e 81 derivações, com composição, identidade, mapa de linhas e `CLASSIFICAR` iguais ao registro. [test_medida_turno_do_par.py:29](app/core/tests/test_medida_turno_do_par.py:29) · [test_medida_turno_do_par.py:321](app/core/tests/test_medida_turno_do_par.py:321) · [test_medida_turno_do_par.py:562](app/core/tests/test_medida_turno_do_par.py:562) · [cartorio.py:1321](app/ponto/services/cartorio.py:1321) · [cartorio.py:1477](app/ponto/services/cartorio.py:1477)

Passaram ainda 32 testes de memória v6/v7/E3 e 10 asserções estruturais extraídas dos selos O219, FND, F1C4, O261 e E3. O mutante E3 `classificacao_antes_guarda` continua falhando na montagem com `ValueError`, porque procura uma compreensão de lista que já saiu. [test_a_tela_v6_sem_par_memoria.py:113](app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:113) · [test_a_tela_v7_impressao_um_significado.py:8](app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:8) · [test_e3_nb_impressao_memoria.py:76](app/ponto/tests/test_e3_nb_impressao_memoria.py:76) · [cartorio.py:1391](app/ponto/services/cartorio.py:1391) · [test_o219_versao_da_regra.py:224](app/ponto/tests/test_o219_versao_da_regra.py:224) · [test_fnd_pergunta_a_juiza.py:137](app/ponto/tests/test_fnd_pergunta_a_juiza.py:137) · [test_f1c4_3_supra_pergunta_ao_juiz.py:215](app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:215) · [test_tripwire_o261_gate_le_o_fio.py:61](app/ponto/tests/test_tripwire_o261_gate_le_o_fio.py:61) · [test_e3_isento_na_impressao.py:122](app/ponto/tests/test_e3_isento_na_impressao.py:122)

**Regressão adicional.** Na entrega, primeira lavra ainda pode emitir furo comum ou reconciliar chamado. A foto gravada nessa mesma passada já satisfaz `_foto_entregue`, bloqueando a segunda passada forçada e a releitura pós-emissão O222. Esta última deveria reler chamado/fio e atualizar a própria decisão recém-lavrada. Seu pulo vai ao contador temporário `_conv` e fica fora do resumo do ato. [cartorio.py:1083](app/ponto/services/cartorio.py:1083) · [cartorio.py:1111](app/ponto/services/cartorio.py:1111) · [cartorio.py:1147](app/ponto/services/cartorio.py:1147) · [cartorio.py:1515](app/ponto/services/cartorio.py:1515) · [cartorio.py:1956](app/ponto/services/cartorio.py:1956) · [cartorio.py:2015](app/ponto/services/cartorio.py:2015)

Em lote misto, a célula entregue recebe sinais falsos na impressão e na classificação. Uma aberta no mesmo lote ainda pode materializar o mapa de toda a janela, incluindo sinais de dias entregues, porque o leitor do pareador filtra só pelo horizonte. [cartorio.py:1214](app/ponto/services/cartorio.py:1214) · [cartorio.py:1229](app/ponto/services/cartorio.py:1229) · [supra_juiz.py:129](app/ponto/supra_juiz.py:129)

## 25/11 07:09 · sessao f6f7a21468a6 · - · - · origem=Ronald (tmux)

**PROCEDENTE.** A opção (b) exclui o sem par do hash e do classificador da entregue. A guarda acrescentada também congela toda célula do horizonte `2026-11+` com veredito e TXT vigente, mesmo quando o dia nunca teve sem par. A trava antecede a comparação de insumos e vale com `forcar` e `toque`. Fato novo poderia rejulgar a célula na v7. `app/ponto/services/cartorio.py:316,332,1229,1258,1381,1515,1535,1892` · `CLAUDE.md:425–429` · `app/docs/LEIS.md:145,155`

- **Normal.** Na aberta, o turno encerrado alimenta o hash com E/S/ES e o classificador pode gravar acusação com os pks em `ata.sem_par`. Sinal inerte entra no hash e deixa `[]`. Na entregue sem veredito, o caminho normal julga os outros fatos, grava veredito, protestos, ata, status e carimbo sem o sinal, podendo emitir cobrança dos furos comuns. Em turno encerrado grava `[]`, que o lastro interpreta como aceso para uma cobrança de sem par existente se há lâmpadas sem pendência. Na entregue com veredito, a guarda pula. No lote misto, a abertura carrega o mapa da janela inteira e chega a projetar sem par da entregue, embora `hash` e classificador do dia entregue nunca o recebam. `app/ponto/services/cartorio.py:1214,1229,1265,1515,1524,1596,1620,1912,1978,2015,2039` · `app/ponto/supra_juiz.py:101,129,547` · `app/ponto/services/lastro.py:81–102,219`
- **Avulso.** Delega ao lote de uma célula. Quando recebe `pulados_foto_entregue`, evita inclusive o carimbo da precedência. Com teto e zero marcos vencidos, `cedo` antecede a guarda, de modo que a entrada direta ainda pode gravar precedência. O vigia normal pré-filtra essa situação. `app/ponto/services/cartorio.py:1482–1493,2221–2237` · `app/ponto/services/vigia_de_hora.py:172–182` · `app/ponto/portas/celula.py:1242–1254`
- **Nunca bateu.** Hash sem batidas/chamados/sem par. Aberta dentro do horizonte pode executar a projeção vazia no apply. Entregue recebe sinais falsos, sem ler o mapa por esse ramo. Primeira lavra sem veredito grava ata sem a chave `sem_par`, veredito de adesão ou o derivado se cobertura integral/dia sem trabalho, status e carimbo; sem protestos gravados nem emissão. A guarda vem antes do pulo por igualdade, mas depois do cálculo do hash e do D5. Ao surgir a primeira batida, o normal deveria rejeitar `nunca_bateu` como atual e a guarda bloqueia a transição. `app/ponto/services/cartorio.py:1319,1360–1443,1515,1535`
- **D5.** A guarda da foto não se aplica. Sem sinais na entregue, com sinais da aberta encerrada no horizonte. Status preenchido sai como já lavrado. Status ausente com carimbo atual e grade, se exigida, grava só status/rótulo; divergência vai à fila. A guarda impede a drenagem no cartório enquanto persistir a entrega, inclusive para foto aberta com E/S seguida de entrega ou foto v7 entregue com E/S no hash. `app/ponto/services/cartorio.py:973–990,1050–1052,1373–1386,1515–1538` · `app/ponto/portas/celula.py:1097–1125`
- **Retroativo e revogação.** Antes do horizonte os sinais do pareador não alimentam a lavra; a proteção fora dele só alcança entregue com acusação gravada. `emitir_furo_retroativo` lê acusação já lavrada no veredito `furo` ou classifica com sinais falsos para achar furo comum, e aciona o emissor sem lavar a célula. Invalidar o TXT sem substituto desliga K6; se foto v8 sem sinal agora recebe E/S, hash muda e rejulga. Uma foto legada da v7 com `concorde`, `ata.sem_par=[]` e E já no hash ainda pula após revogação com os mesmos fatos. Ocorrência na frota [nao verificado]. `app/ponto/services/cartorio.py:332,1234,1258,1524,1535–1541` · `app/ponto/management/commands/emitir_furo_retroativo.py:98–118,130–166` · `app/folha/export.py:588–594` · `app/folha/models.py:143–164`

**Fatos novos.** Batida regularizada/retratada e cobertura/justificativa disparam o avulso, chamado novo ou mudança de estado também. A guarda conserva veredito, ata, impressão, marcas de ausência/batida e status anteriores mesmo se os fatos que alimentam o hash mudarem. Cadastro pode efetivamente gravar DNA novo e manter a ata antiga. A porta expressa `regen_exportada` regenera com `apesar_da_lavra`, mas a regeneração chama o avulso do cartório com `forcar=True`, e o chamador pode registrar o dia no vetor `julgados` apesar do pulo. A grade de dinheiro lê minutos previstos da ata, se presentes; valor de folha afetado [nao verificado]. Status existente deixa de ser atualizado; D5 só supre status ausente se o carimbo for atual. O alcance excede o sem par do corte e conflita com rejulgar fato novo pela L-124 e deixar entrar correção provada em qualquer competência pelo item 3 de DINHEIRO. `app/ponto/signals.py:78–93,130–165` · `app/chamados/signals.py:189–218` · `app/ponto/services/cartorio.py:973–990,1258,1373–1386,1515` · `app/ponto/portas/regen_exportada.py:174–193` · `app/ponto/portas/celula.py:390–394,456–503` · `app/escala/services/leitor_celula.py:379,397–423` · `app/docs/CORTES.md:640` · `app/docs/LEIS.md:145,155` · `CLAUDE.md:425–429`

**V7 e idempotência.** O avulso após lote entregue do laudo anterior fica curado para o caminho comum. A v7 exigia hash com sinal bruto e filtro tardio; esse desenho foi superado por (b). A revogação foi curada para novas lavras v8 com E/S atual e segue aberta para estoque v7 de hash bruto; D5 agora também enfileira esses carimbos ainda entregues. Confirmei em memória, com corpo/hash/classificador reais extraídos e portas dubladas, que lote repetido normal ou `nunca_bateu` preserva a foto em aberta e entregue, e que D5 enfileira entregue após batida nova. Repetição aberta dá pulo por igualdade, entregue dá pulo por foto. Lote seguido de avulso aberto ainda pode gravar precedência e um outbox na primeira avulsa, embora sem nova trilha `veredito_celula`. Primeira lavra com chamada real ou reconciliação pode lavrar mais de uma vez no mesmo ato aberto, então uma linha de trilha não é invariante. Na entregue, a nova guarda pode impedir a segunda lavra necessária **do próprio ato**, após emissão de furo comum ou reconciliação. A porta do chamado deduplica sob trava do colaborador, mas zero duplicatas, total de linhas persistidas e concorrência ORM [nao verificado]. `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:16–27,68–109` · `app/ponto/services/cartorio.py:1079,1135–1154,1377,1515–1541,2221–2237` · `app/ponto/portas/celula.py:994–1002,1113–1141,1227–1238,1242–1254` · `app/ponto/management/commands/detectar_ausencias.py:697–717` · `app/chamados/models.py:939–971`

## 25/11 07:02 · sessao ee1e060c5ed2 · A-TELA-DO-DIA-LE-UM-JUIZ · construtor · origem=Code

Implementei a opção **(b)**. No horizonte do sem par, o mesmo K6 corta o sinal antes da leitura do pareador e preserva a foto que já tem veredito. A primeira lavra entregue classifica os demais insumos. A competência aberta continua levando o sinal ao classificador e à impressão. `ponto/services/cartorio.py:1229`, `:1248`, `:1258`, `:1515`, `:1607`.

| Propriedade | Resultado pelo código |
|---|---|
| 1. Momento da entrega | A guarda preserva a célula com acusação e a célula com sinal inerte. **Zero saves de `impressao`**. No ramo normal, a guarda sai antes de calcular o hash e antes da lavra. O teste compara as linhas inteiras e vigia `CelulaDia.save`. `ponto/services/cartorio.py:1515`, `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:96`. |
| 2. Impressão com um significado | O mesmo `_sp_julga` alimenta o hash e o classificador. O selo v7 verifica a leitura vazia na primeira lavra e no D5, mantém a igualdade da foto e exige que a repetição saia pela guarda, sem classificar, calcular hash ou consultar o leitor do sem par. `ponto/services/cartorio.py:1518`, `:1530`, `:1607`, `ponto/tests/test_a_tela_v7_impressao_um_significado.py:13`. |
| 3. Lote seguido de dois avulsos | O teste exige zero saves de célula, ata idêntica, ausência de `precedencia` e zero chamados. A guarda incrementa `pulados_foto_entregue`, que barra a escrita de precedência no avulso. `ponto/services/cartorio.py:1515`, `:2229`, `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:123`. |
| 4. TXT invalidado | Para a primeira lavra feita pela v8, a invalidação sem substituto desliga K6. O sinal volta à impressão, abre o julgamento e produz a acusação com o PK da batida. `folha/export.py:588`, `ponto/services/cartorio.py:1234`, `:1530`, `:1622`, `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:146`. |
| 5. D5 | Com status ausente e carimbo v8 em dia, o teste exige `completaveis=1`, `completados=1` e fila zero. Na repetição exige `ja_lavrado=1`, fila zero e zero saves. A foto inteira volta ao estado original depois de repor o status. `ponto/services/cartorio.py:973`, `:1535`, `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:168`. |
| 6. `nunca_bateu` | A primeira lavra usa o mesmo portão de leitura. A guarda da foto vem antes do pulo por hash no julgamento comum, e o D5 usa `imp0`. `ponto/services/cartorio.py:1374`, `:1381`, `:1389`, `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:198`. |

**Limite para o Code.** O TXT guarda geração e invalidação. A célula guarda `julgada_em` e o hash dos insumos. O fato “estava entregue quando lavrou” fica sem registro explícito na célula. Para uma foto **já lavrada pela v7** com sinal bruto no hash e ata vazia, a propriedade **4** ainda pode cair por igualdade do hash após a invalidação. A propriedade **1** preserva essa foto enquanto K6 estiver vigente. Um status ausente num carimbo legado divergente também pode continuar na fila pelo contrato do D5. O diff preserva os carimbos legados. Incidência em produção **[nao verificado]**. `folha/models.py:82`, `:96`, `ponto/portas/celula.py:1227`, `ponto/services/cartorio.py:980`, `:1515`, `:1535`.

**Censo de código**

- Alterei `_julgar_colab_corpo`, com `_sem_par` e `_foto_entregue`, e retirei de `classificar_dia` o parâmetro `posicoes_sem_par`. Os únicos dois chamadores de `_retirar_sem_par_entregue` estavam no cartório. Apaguei o helper, as listas de posições, o teste `test_sinal_inerte_mantem_impressao_sem_consultar_entrega` e os adaptadores de base e mutantes v6/v7. Os casos de preservação dos demais protestos e de `nunca_bateu` seguem com asserções sobre a leitura anterior à classificação. `7e266931b:app/ponto/services/cartorio.py:1264`, `:1407`, `:1621`, `7e266931b:app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:188`, `ponto/supra_juiz.py:278`, `ponto/tests/test_a_tela_v6_sem_par_memoria.py:148`.
- **`CelulaDia.impressao`** é gravada e comparada na porta `carimbar`, chamada pelas duas lavras. `desfazer_julgamento_da_futura` lê o campo para a trilha e o limpa. O cartório compara os hashes para o pulo e o D5. Outros leitores copiam ou comparam o valor gravado nos snapshots. `ponto/portas/celula.py:1149`, `:1186`, `:1202`, `:1227`, `ponto/services/cartorio.py:1377`, `:1439`, `:1535`, `:2039`, `ponto/management/commands/celula_veredito_velho.py:110`, `colaboradores/services/feriado_do_posto.py:428`, `chamados/services/foto_e4b.py:251`.
- **`ata.sem_par`** é montada no cartório para a ata em curso e para a ata persistida pela porta. Leem a chave a guarda da foto e `lastro.sem_par_da_ata`, usado pelo lastro, pela emissão do cartório, pelo supra e pelo retroativo. `ponto/services/cartorio.py:818`, `:1262`, `:1629`, `:2017`, `:2111`, `ponto/portas/celula.py:1128`, `ponto/services/lastro.py:81`, `:221`, `ponto/management/commands/supra_juiz.py:209`, `ponto/management/commands/emitir_furo_retroativo.py:99`.

O censo real confirmou **`[1321, 1477]`**, totais **9/81**, identidades, composição e `CLASSIFICAR`. Conferi a compilação em memória das fontes e dos cinco mutantes, com **zero `except` adicionados**. As duas chamadas de impressão e as duas de classificação conservaram os argumentos dos selos, salvo a retirada do parâmetro órfão. Execução ORM, morte dos mutantes e resultado dos seis selos **[nao verificado]**, para certificação pelo Code e pelo opus final. `core/medida_turno_do_par.py:448`, `core/tests/test_medida_turno_do_par.py:108`, `:321`, `ponto/services/cartorio.py:1374`, `:1390`, `:1524`, `:1596`.

**Comandos para o Code**

Na base, o caso 3 deve cair em `assertNotIn('precedencia', depois['ata'])` no primeiro avulso. O caso 4 deve cair em `assertGreater(avulso['julgadas'], 0)`. O seletor `base` carrega as funções do hash pedido em memória, com as entradas reais de lote e avulso. `ponto/tests/test_a_tela_v8_entregue_nao_le_sem_par.py:42`, `:81`, `:140`, `:162`, `7e266931b:app/ponto/services/cartorio.py:1550`, `:2248`.

```bash
alvo=ponto.tests.test_a_tela_v8_entregue_nao_le_sem_par.EntregueNaoLeSemParTest
avulso="$alvo.test_RED_lote_entregue_e_dois_avulsos_preservam_foto_sem_precedencia"
txt="$alvo.test_RED_txt_invalidado_sem_substituto_reabre_o_julgamento"

PYTHONDONTWRITEBYTECODE=1 python -B manage.py test "$alvo"
A_TELA_V8_VARIANTE=base PYTHONDONTWRITEBYTECODE=1 python -B manage.py test "$avulso" "$txt"

A_TELA_V8_VARIANTE=le_sem_par_entregue PYTHONDONTWRITEBYTECODE=1 python -B manage.py test "$avulso"
A_TELA_V8_VARIANTE=foto_so_com_acusacao PYTHONDONTWRITEBYTECODE=1 python -B manage.py test "$avulso"
A_TELA_V8_VARIANTE=aberta_sem_sinal PYTHONDONTWRITEBYTECODE=1 python -B manage.py test "$txt"
A_TELA_V8_VARIANTE=d5_pula_foto PYTHONDONTWRITEBYTECODE=1 python -B manage.py test "$alvo.test_d5_entregue_com_sinal_lavra_um_status_ausente"
A_TELA_V8_VARIANTE=nb_hash_antes_da_foto PYTHONDONTWRITEBYTECODE=1 python -B manage.py test "$alvo.test_nunca_bateu_entregue_preserva_foto_no_avulso_e_completa_status"
```

## 25/11 07:02 · sessao 0d0075cf508f · PAR-RELAMPAGO-A-LISTA-DO-ADMIN · sonda · origem=Code

## Saída raspada fornecida

```text
sonda_leitura: sonda_estoque.py [7610d93fbc9e8e83] no cpuset 4-7, lendo sombra como leitor (LEITURA)

TOTAL_ESTOQUE_RETRATADO_PELO_CRON 71
empresa competencia exportada total sem_decisao
2 202607 False 3 3
2 202608 True 17 17
2 202609 True 4 4
2 202610 False 11 11
3 202606 False 1 1
3 202607 True 26 26
3 202608 False 3 3
3 202611 False 2 2
4 202606 True 3 3
4 202607 True 1 1
pk colab empresa data_local retratada_em(local) tem_decisao exportada
44398 29 4 2026-06-17 30/07 00:13 False True
44399 29 4 2026-06-17 30/07 00:13 False True
60707 39 4 2026-06-19 09/08 23:56 False True
44650 146 3 2026-06-20 30/07 00:13 False False
45516 146 3 2026-06-22 30/07 00:13 False True
44839 146 3 2026-06-22 30/07 00:13 False True
45113 93 3 2026-06-23 30/07 00:13 False True
45114 93 3 2026-06-23 30/07 00:13 False True
45314 642 3 2026-06-24 09/08 23:56 False True
45403 111 3 2026-06-24 30/07 00:01 False True
45410 157 3 2026-06-24 30/07 00:01 False True
45349 81 3 2026-06-24 09/08 23:56 False True
45312 642 3 2026-06-24 09/08 23:56 False True
45539 172 3 2026-06-25 30/07 00:13 False True
45540 172 3 2026-06-25 30/07 00:13 False True
45684 95 3 2026-06-25 30/07 00:01 False True
45745 146 3 2026-06-26 30/07 00:13 False True
45746 146 3 2026-06-26 30/07 00:13 False True
45830 90 3 2026-06-26 30/07 00:13 False True
45832 90 3 2026-06-26 30/07 00:13 False True
46308 170 3 2026-06-27 30/07 00:01 False True
46288 170 3 2026-06-29 30/07 00:01 False True
47225 174 2 2026-07-01 09/08 23:56 False False
47490 82 3 2026-07-02 30/07 00:01 False True
48141 761 3 2026-07-06 30/07 00:13 False True
48143 761 3 2026-07-06 30/07 00:13 False True
48323 148 3 2026-07-07 30/07 00:01 False True
59632 747 3 2026-07-13 09/08 23:56 False True
49572 51 4 2026-07-14 30/07 00:01 False True
50779 152 3 2026-07-17 30/07 00:13 False True
50267 152 3 2026-07-17 30/07 00:13 False True
50291 114 3 2026-07-17 09/08 23:56 False True
50875 663 2 2026-07-20 30/07 00:13 False False
50876 663 2 2026-07-20 30/07 00:13 False False
51193 465 2 2026-07-21 30/07 00:13 False True
51197 465 2 2026-07-21 30/07 00:13 False True
51418 450 2 2026-07-21 29/07 23:44 False True
51551 398 2 2026-07-21 29/07 23:44 False True
53304 451 2 2026-07-23 30/07 00:13 False True
53317 451 2 2026-07-23 30/07 00:13 False True
53799 238 2 2026-07-24 30/07 00:13 False True
53805 238 2 2026-07-24 30/07 00:13 False True
53813 239 2 2026-07-24 30/07 00:13 False True
53821 239 2 2026-07-24 30/07 00:13 False True
54959 492 2 2026-07-25 29/07 23:44 False True
58216 512 2 2026-07-29 29/07 23:44 False True
59233 868 3 2026-07-30 31/07 06:55 False False
59229 868 3 2026-07-30 31/07 06:55 False False
60942 363 2 2026-08-01 02/08 06:55 False True
60943 363 2 2026-08-01 02/08 06:55 False True
61042 668 2 2026-08-01 03/08 06:55 False True
61548 169 3 2026-08-02 09/08 23:56 False False
65277 860 2 2026-08-05 06/08 06:55 False True
65273 860 2 2026-08-05 06/08 06:55 False True
97993 936 2 2026-09-11 12/09 06:26 False True
98000 936 2 2026-09-11 12/09 06:26 False True
104398 473 2 2026-09-18 19/09 06:26 False True
104386 473 2 2026-09-18 19/09 06:26 False True
111295 190 2 2026-09-26 15/10 02:23 False False
112983 948 2 2026-09-28 29/09 06:26 False False
112970 948 2 2026-09-28 29/09 06:26 False False
113226 398 2 2026-09-29 30/09 06:26 False False
113225 398 2 2026-09-29 30/09 06:26 False False
115732 358 2 2026-10-01 03/10 06:26 False False
115731 358 2 2026-10-01 03/10 06:26 False False
123806 934 2 2026-10-10 11/10 06:26 False False
128118 992 2 2026-10-15 16/10 06:26 False False
128150 992 2 2026-10-15 16/10 06:26 False False
129040 741 2 2026-10-16 17/10 06:26 False False
141249 109 3 2026-10-29 30/10 06:26 False False
141250 109 3 2026-10-29 30/10 06:26 False False
rc_sombra=0
```

## Respostas

Não encontrei `sonda_estoque.py` na cópia local e não reexecutei a sonda. A transação `READ ONLY` e o rollback da execução reportada ficam [nao verificado]. As referências `DADO RASPADO:linha` apontam para o bloco acima.

1. **71 batidas**, conforme `TOTAL_ESTOQUE_RETRATADO_PELO_CRON`. `DADO RASPADO:3` · `app/ponto/management/commands/detectar_par_relampago.py:224`
2. **51 em competências exportadas e 20 em abertas**, usando o mês do fim da janela de 21 a 20. `DADO RASPADO:5–14` · `app/ponto/janelas.py:10` · `app/folha/export.py:594`

   | Empresa | Competência | Exportada | Aberta |
   |---:|---:|---:|---:|
   | 2 | 202607 | 0 | 3 |
   | 2 | 202608 | 17 | 0 |
   | 2 | 202609 | 4 | 0 |
   | 2 | 202610 | 0 | 11 |
   | 3 | 202606 | 0 | 1 |
   | 3 | 202607 | 26 | 0 |
   | 3 | 202608 | 0 | 3 |
   | 3 | 202611 | 0 | 2 |
   | 4 | 202606 | 3 | 0 |
   | 4 | 202607 | 1 | 0 |
   | **Total** | | **51** | **20** |

3. **0 batidas com ato humano auditado posterior.** Todas as linhas detalhadas têm `tem_decisao=False`. `DADO RASPADO:15–86`
4. **Nenhum `ERRO` ou traceback aparece no dado raspado.** A última linha é `rc_sombra=0`. `DADO RASPADO:1–87`

## 25/11 06:49 · sessao b2c670b7bee4 · REGUA-TICKETS-SO-PADRAO-DE-FATIA · leitura · origem=Code

**O contrato está incompleto.** Há um segundo script de pre-push que chama a `regua_tickets`, e o placar tem uma definição própria de citação. O filtro novo ainda aceita marcadores de prosa com dígito. A cópia veio sem `.git`, então o censo histórico e o resultado em `origin/main..HEAD` ficam como [nao verificado]. `app/core/hooks/pre-push.sh:7`, `app/core/placar_tickets.py:175–191`, `bin/regua_tickets.sh:81–85`.

### 1. Quem chama e quem lê a saída

- `bin/pre-push.sh` e `app/core/hooks/pre-push.sh` chamam `~/saas-hasner/bin/regua_tickets.sh` e convertem qualquer retorno diferente de zero em `exit 1`, com a saída textual seguindo para quem acompanha o push. A instalação prevista em `bin/hooks.sh` delega para `bin/pre-push.sh` da árvore do push, que por sua vez aponta a régua de tickets para `~/saas-hasner`. Qual wrapper estava instalado no host é [nao verificado]. `bin/pre-push.sh:5–11`, `app/core/hooks/pre-push.sh:5–8`, `bin/hooks.sh:63–70`.
- `bin/regua.sh` chega à régua de tickets **pelo selo novo**, quando alcança o laço dos selos de host. O atalho `JA VERDE` sai antes desse laço. O selo compara o `rc` e usa a última linha da saída para diagnóstico quando há divergência. `bin/regua.sh:84–95`, `bin/regua.sh:128–145`, `bin/tests/test_regua_tickets_padrao_de_fatia.sh:25–30`.
- Na cópia, não encontrei consumidor de `CITADOS` nem de `N citacao(oes)` fora da própria régua. O número é de **IDs distintos**. O contador que `bin/placar_code.sh` extrai é `parado_citado_em_commit_posterior`, produzido pelo placar a partir de uma leitura própria do Git. O `rc=1` da régua também pode significar tabela ou base ausente. O placar reprovado retorna `2` na régua e vira `1` no hook. `bin/regua_tickets.sh:28–38`, `bin/regua_tickets.sh:81–104`, `bin/tickets_placar.sh:54–55`, `bin/tickets_placar.sh:88–99`, `bin/placar_code.sh:44–53`, `bin/pre-push.sh:10`.

### 2. O que o filtro ainda confunde com fatia

- **Há dois casos históricos documentados** de prosa com dígito que a regra nova ainda extrai: `[FIO-C6-MORTA]` e `[FIO-DUP-C8]`. Eram marcadores de log citados no corpo de commits, e ambos já ganharam linha na tabela. São exemplos do falso positivo, mas ficam fora do conjunto **sem linha** da cópia. `app/docs/RELATO.md:5772`, `app/docs/TICKETS.md:148`, `app/docs/TICKETS.md:150`, `bin/regua_tickets.sh:81–96`.
- No ensaio em memória, um literal solto como `[OK2]` ou um hash em maiúsculas como `[A1B2C3D]` foi cobrado. Um índice numérico como `[0]`, um acesso colado como `x[A2]` e uma lista com vírgula como `[OK2, AGORA]` ficaram de fora. A regra distingue essas formas pelo regex, sem identificar o sentido da citação. `bin/regua_tickets.sh:81–85`.
- **Conferi 983 IDs distintos na tabela, 565 com dígito e 418 sem.** Os números históricos relatados implicam `730 − 376 = 354` IDs citados com dígito, antes de cruzar com a tabela. O total **com dígito e sem linha**, sua lista e a classificação entre prosa e fatias esquecidas ficam [nao verificado]. `bin/regua_tickets.sh:16–18`, `bin/regua_tickets.sh:81–96`.

### 3. O que o selo prova

- O selo copia a **régua real daquela árvore** a cada caso e a executa no repositório descartável. O placar é substituído por um stub verde. No ensaio em memória com a régua real e entradas simuladas, os três `MORDE` retornaram `1` por falta de linha, e `fatia_com_linha` retornou `0` com **2 citações**. A execução integral do selo com Git fica [nao verificado]. `bin/tests/test_regua_tickets_padrao_de_fatia.sh:12–30`, `bin/tests/test_regua_tickets_padrao_de_fatia.sh:37–43`, `bin/regua_tickets.sh:23–38`, `bin/regua_tickets.sh:93–104`.
- **A asserção aceita a causa errada.** Base inexistente também satisfaz cada `MORDE` isoladamente. Se a base faltar em todos os casos, os positivos derrubam o selo. Já `fatia_com_linha` também fica verde com **zero citações extraídas**, pois só compara o `rc`. Uma falha de `git log` sem saída pode produzir esse verde. Os comandos de preparação e de criação do commit também seguem sem checagem explícita de retorno. `bin/tests/test_regua_tickets_padrao_de_fatia.sh:11–30`, `bin/tests/test_regua_tickets_padrao_de_fatia.sh:38–43`, `bin/regua_tickets.sh:23`, `bin/regua_tickets.sh:37–38`, `bin/regua_tickets.sh:81–104`.

### 4. Onde a regra diverge

- **O placar diz que usa o mesmo padrão, mas aplica outro.** Procura o ID de uma linha parada no assunto, sem exigir dígito e sem distinguir maiúsculas de minúsculas. A régua procura no assunto e no corpo e exige dígito. O próprio selo do placar exige que `[ALFA]` no assunto conte. Como o placar roda antes da extração da régua, uma divergência dele ainda pode barrar o push. `app/core/placar_tickets.py:175–191`, `app/core/tests/test_selo_placar_tickets.py:111–119`, `bin/regua_tickets.sh:28–38`, `bin/regua_tickets.sh:81–85`.
- `CLAUDE.md` e a regra principal do `TICKETS.md` dão exemplos compatíveis com o filtro, sem declarar a exigência de dígito. O próprio `TICKETS.md` registra que a régua passou a cobrar `[SELO-VE-O-CASO-COMUM]` e `[GEOFENCE-VALIDAR-VOLTA]`, ambos agora descartados. O `BACKLOG.md` afirma que colchete de prosa não é fatia, distinção que o regex ainda não faz. `CLAUDE.md:3–6`, `app/docs/TICKETS.md:98–104`, `app/docs/TICKETS.md:591–594`, `app/docs/BACKLOG.md:1988`, `bin/regua_tickets.sh:81–85`.
- `bin/regua.sh` apenas executa o selo pelo laço de host. `bin/tickets_rodape.sh` confere `origin/main`, o carimbo e a série T. O handoff chama de “rodapé” a verificação do topo feita pela régua de tickets, embora o hook confira o rodapé em uma chamada separada. `bin/regua.sh:128–145`, `bin/tickets_rodape.sh:20–35`, `bin/tickets_rodape.sh:84–95`, `app/docs/HANDOFF_SESSAO.md:66–67`, `bin/regua_tickets.sh:28–35`, `bin/pre-push.sh:10–11`.

## 25/11 06:46 · sessao 1c654b1dd0de · A-TELA-DO-DIA-LE-UM-JUIZ · verificador · origem=Code

**Parecer da v7 em `7e266931b`: PROCEDENTE, reprova.** A primeira lavra em lote passou a usar a mesma impressão bruta das leituras seguintes. A mudança abriu uma regressão no **avulso**: o pulo por impressão igual deixa `pulados_foto_entregue=0`, e `julgar_celula` pode escrever `ata.precedencia` numa foto entregue que a v6 preservava. Há também um pulo indevido após revogação do TXT e uma linha desatualizada na catraca do merge. [cartorio.py:1546](/home/ronald/codex_ws/7e266931b89d634a1d7262cc32d3ad9568d748b8/app/ponto/services/cartorio.py:1546) · [cartorio.py:2248](/home/ronald/codex_ws/7e266931b89d634a1d7262cc32d3ad9568d748b8/app/ponto/services/cartorio.py:2248) · [export.py:588](/home/ronald/codex_ws/7e266931b89d634a1d7262cc32d3ad9568d748b8/app/folha/export.py:588) · [test_medida_turno_do_par.py:321](/home/ronald/codex_ws/7e266931b89d634a1d7262cc32d3ad9568d748b8/app/core/tests/test_medida_turno_do_par.py:321)

### 1. Escritores, leitores e significado

| Sítio | Uso de `CelulaDia.impressao` |
|---|---|
| `carimbar` | Único gravador de um hash encontrado. Compara o valor anterior com o argumento para evitar um save e persiste o valor recebido. Os dois chamadores são a lavra normal e `nunca_bateu`. `app/ponto/portas/celula.py:1218`, `app/ponto/portas/celula.py:1227`, `app/ponto/services/cartorio.py:1449`, `app/ponto/services/cartorio.py:2056` |
| `desfazer_julgamento_da_futura` | Lê o valor para a trilha e o limpa para `None` na mesma porta, na operação de desfazer o julgamento futuro. Preserva o julgamento de admin. `app/ponto/portas/celula.py:1149`, `app/ponto/portas/celula.py:1186`, `app/ponto/portas/celula.py:1192`, `app/ponto/portas/celula.py:1202` |
| Cartório e D5 | As duas comparações com hash recalculado estão no corpo do cartório, uma por ramo. `_completar_status` recebe o dia se o carimbo estiver em dia; lê o status e o veredito, sem recalcular a impressão. `app/ponto/services/cartorio.py:1381`, `app/ponto/services/cartorio.py:1535`, `app/ponto/services/cartorio.py:1546`, `app/ponto/services/cartorio.py:973` |
| `processar_cartorio` e `lavrar_status_competencia` | Delegam a `julgar_colab` e publicam os contadores. A importação de `impressao_insumos` em `processar_cartorio` fica sem chamada própria. `app/ponto/management/commands/processar_cartorio.py:20`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/management/commands/lavrar_status_competencia.py:56` |
| `celula_veredito_velho` e `snapshot_o118_col221` | Copiam o valor gravado para o snapshot de reversão. O primeiro também chama `julgar_colab(forcar=True)` quando percorre as células para relavrar. O segundo só descreve a reversão no JSON. `app/ponto/management/commands/celula_veredito_velho.py:104`, `app/ponto/management/commands/celula_veredito_velho.py:144`, `bin/snapshot_o118_col221.py:15`, `bin/snapshot_o118_col221.py:25` |
| `feriado_do_posto` e `foto_e4b` | Comparam fotos antes/depois pelo valor gravado. O primeiro inclui `impressao` na lista explícita de campos. O segundo inclui todos os campos concretos e usa a diferença na cerca da competência. `app/colaboradores/services/feriado_do_posto.py:428`, `app/colaboradores/services/feriado_do_posto.py:451`, `app/chamados/services/foto_e4b.py:116`, `app/chamados/services/foto_e4b.py:145`, `app/chamados/services/foto_e4b.py:247`, `app/chamados/services/foto_e4b.py:269` |

A impressão normal é SHA-256 das batidas apuráveis da janela de três dias (`pk`, tipo, instante), status da cobertura, chamados (`pk`, `status_local`), versão do DNA e marcos, mais as parcelas aplicáveis de veto, teto, versão da regra, turno em curso, quantidade de cobranças com fio, cobertura parcial, isenção e sem par. `nunca_bateu` usa listas vazias de batidas e chamados, cobertura/DNA/regra/isenção e, quando cabe, a marca de sem adesão. O estado de K6 fica fora do hash. `app/ponto/services/cartorio.py:513`, `app/ponto/services/cartorio.py:522`, `app/ponto/services/cartorio.py:566`, `app/ponto/services/cartorio.py:574`, `app/ponto/services/cartorio.py:590`, `app/ponto/services/cartorio.py:1381`, `app/ponto/services/cartorio.py:1484`, `app/ponto/services/cartorio.py:1535`

Para **novas gravações**, o sem par é bruto em relação a K6: `E`, `S` ou `ES` do pareador, já recortado pelo horizonte e pelo turno encerrado. O mesmo `imp` é conferido e entregue à porta. Fora do cartório, encontrei zero comparações com um hash reconstruído por outra fórmula. A ressalva é o estoque: um carimbo filtrado da v6 continua gravado com os bytes antigos enquanto a guarda impedir sua relavra. Existência e volume desse estoque em produção **[nao verificado]**. `app/ponto/supra_juiz.py:101`, `app/ponto/supra_juiz.py:129`, `app/ponto/services/cartorio.py:1229`, `app/ponto/services/cartorio.py:1535`, `app/ponto/services/cartorio.py:1621`, `app/ponto/services/cartorio.py:2056`

### 2. Segunda e terceira passadas com os mesmos fatos

| Ramo | Resultado na 2ª e na 3ª passada |
|---|---|
| Normal em lote, aberta ou entregue, primeira lavra pela v7 | Se a primeira lavra completou e os insumos ficaram iguais, ambas dão `pulados=1`, sem classificar ou carimbar. Na entregue recém-lavrada com o sem par barrado, `pulados_foto_entregue=0` nas duas. Com `--forcar`, o caso que gera protesto de sem par volta à guarda e conta a foto. `app/ponto/services/cartorio.py:1546`, `app/ponto/services/cartorio.py:1608`, `app/ponto/services/cartorio.py:1621`, `app/ponto/tests/test_a_tela_v3_sem_par.py:247` |
| Entregue, foto anterior com acusação em `ata.sem_par` | Ambas preservam a foto. No normal, a guarda vem antes do hash. Em `nunca_bateu`, a igualdade pode dar `pulados=1` antes de chegar à guarda. `app/ponto/services/cartorio.py:1255`, `app/ponto/services/cartorio.py:1384`, `app/ponto/services/cartorio.py:1526` |
| Entregue, foto sem acusação, impressão divergente e protesto atual de sem par | Ambas classificam, K6 protege a foto, e ambas dão `pulados_foto_entregue=1`. Como o hash fica antigo, D5 com status ausente dá `status_fila_cartorio=1` nas duas. Vale para a impressão v6 filtrada e também para uma foto anterior a um sinal novo que a guarda barrou. **A fila permanente ainda é possível.** `app/ponto/services/cartorio.py:983`, `app/ponto/services/cartorio.py:1546`, `app/ponto/services/cartorio.py:1621`, `app/ponto/services/cartorio.py:2056` |
| `nunca_bateu`, aberta ou entregue | Sem batida apurável, o pareador tem zero sinais. A primeira lavra usa `imp0`; ambas as repetições pulam com o mesmo `imp0`. A primeira batida apurável desloca a célula para o ramo normal, que recusa considerar em dia um veredito `nunca_bateu`. `app/ponto/turnos.py:1867`, `app/ponto/services/cartorio.py:1326`, `app/ponto/services/cartorio.py:1381`, `app/ponto/services/cartorio.py:1449`, `app/ponto/services/cartorio.py:1546` |
| D5, aberta ou entregue | Status já presente dá `status_ja_lavrado=1`, mesmo com hash velho. Status ausente e carimbo atual dá `status_completaveis=1` em cada dry-run. Com `apply`, a primeira conclusão bem-sucedida conta `completaveis=1` e `completados=1`, e as seguintes contam `ja_lavrado=1`. Status ausente com carimbo velho, ou sem dia de grade no ramo normal, continua na fila. D5 sai antes do classificador e das passadas internas. `app/ponto/services/cartorio.py:980`, `app/ponto/services/cartorio.py:1050`, `app/ponto/services/cartorio.py:1385`, `app/ponto/services/cartorio.py:1547` |
| Retroativo | Antes do horizonte `2026-11`, o sem par fica fora do hash e o pulo segue a comparação dos demais insumos. Já `emitir_furo_retroativo` é outro fluxo: relê a acusação da ata ou procura o furo comum com ambos os sinais em `False`. Em cada repetição, se o chamado da acusação está vivo, conta `ja_existia`; se a primeira tentativa foi vetada e os fatos seguiram iguais, pode tentar novamente. O comando não carimba a célula. `app/ponto/services/cartorio.py:316`, `app/ponto/services/cartorio.py:332`, `app/ponto/management/commands/emitir_furo_retroativo.py:98`, `app/ponto/management/commands/emitir_furo_retroativo.py:111`, `app/ponto/management/commands/emitir_furo_retroativo.py:157` |

A segunda passada interna de reconciliação é forçada. A releitura pós-emissão volta ao mesmo corpo sem `forcar` e pode julgar de novo se chamado ou fio mudou. O RED novo chama o corpo diretamente e deixa essas duas costuras fora da execução. `app/ponto/services/cartorio.py:1079`, `app/ponto/services/cartorio.py:1135`, `app/ponto/services/cartorio.py:1148`, `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:145`

### 3. K6 e integridade da foto entregue

- **K6 continua barrando o protesto quando o julgamento é alcançado.** O classificador marca a posição que produziu; K6 retira esse item. Sem foto prévia, os demais códigos seguem à lavra e a ata fica sem a acusação retirada. Com foto prévia e protesto atual acionável, o corpo preserva a foto. Um sinal que passa de vazio para `E`, `S` ou `ES` muda o hash e chega a esse ponto, salvo proteção anterior pela própria foto. Sinal inerte, como sobra num dia já cumprido, não gera posição. `app/ponto/supra_juiz.py:550`, `app/ponto/services/cartorio.py:1255`, `app/ponto/services/cartorio.py:1264`, `app/ponto/services/cartorio.py:1535`, `app/ponto/services/cartorio.py:1621`, `app/ponto/services/cartorio.py:1637`
- **Igualdade pode esconder mudança de K6.** Primeira lavra entregue com `E` acionável grava `concorde`, `ata.sem_par=[]` e hash com `E`. Se o TXT vigente é invalidado sem substituto, a mesma entrada continua gerando o mesmo hash e a foto vazia de acusação passa pela primeira guarda. A passada normal pula antes de consultar K6. Na sonda, obtive `pulados=1`, zero classificações e zero consultas a K6 após trocar a resposta para aberta. A mesma entrada, aberta desde a primeira lavra, deu `furo` com **o mesmo hash**. `app/folha/models.py:143`, `app/folha/export.py:588`, `app/ponto/services/cartorio.py:1260`, `app/ponto/services/cartorio.py:1546`, `app/ponto/services/cartorio.py:1621`
- **A foto entregue pode mudar no avulso, com os mesmos fatos e o TXT ainda vigente.** Reproduzi primeira lavra em lote seguida de duas chamadas avulsas. Na v6, ambas contaram `pulados_foto_entregue=1`, com zero saves adicionais. Na v7, ambas deram `pulados=1` e `pulados_foto_entregue=0`. A primeira avulsa acrescentou `ata.precedencia` e pediu um save; a segunda manteve essa alteração. A porta real de precedência dispara o caminho do outbox quando a ata muda. O teste ORM existente cobre uma foto **com** acusação e chamada **forçada**, caso em que a guarda ainda funciona. `app/ponto/services/cartorio.py:1546`, `app/ponto/services/cartorio.py:2200`, `app/ponto/services/cartorio.py:2248`, `app/ponto/portas/celula.py:978`, `app/ponto/portas/celula.py:994`, `app/ponto/portas/celula.py:1242`, `app/ponto/tests/test_a_tela_v3_sem_par.py:282`

### 4. Idempotência, trilha e duplicatas

Na sonda sequencial do corpo, usei as funções de lavra, carimbo e status extraídas da cópia, com persistência, trilha, outbox e bordas dublados. Em normal e `nunca_bateu`, tanto aberta quanto entregue, três passadas deram **o mesmo estado após a primeira, um save, uma chamada de trilha `veredito_celula` e uma chamada de outbox**. Para a entregue nova com protesto barrado, `pulados_foto_entregue` caiu de `0,1,1` na v6 para `0,0,0` na v7. Na sonda de D5 com status removido, a primeira aplicação gravou só status/rótulo, com trilha própria, e a seguinte contou `ja_lavrado`. O avulso após o lote é a exceção de estado descrita acima. Total de linhas persistidas, zero duplicatas de chamado e comportamento concorrente em banco **[nao verificado]**. `app/ponto/services/cartorio.py:980`, `app/ponto/services/cartorio.py:1388`, `app/ponto/services/cartorio.py:1550`, `app/ponto/portas/celula.py:994`, `app/ponto/portas/celula.py:1113`, `app/ponto/portas/celula.py:1136`, `app/ponto/portas/celula.py:1227`

### 5. RED, mutantes e testes adaptados [nao verificado]

- **33 testes de memória passaram.** O RED v7 e os testes de memória v6/E3 executam corpos reais extraídos por AST com dublês. A v7 usa D5 real em dry-run, com pareador, K6, grade, lavra, persistência e emissão fora do caminho real de integração. O teste adaptado em `test_a_tela_v3_sem_par.py` importa o cartório real e entra pelo comando Django, com o emissor dublado. Sua execução ORM **[nao verificado]**. `app/ponto/tests/test_e3_nb_impressao_memoria.py:56`, `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:87`, `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:120`, `app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:50`, `app/ponto/tests/test_a_tela_v3_sem_par.py:6`, `app/ponto/tests/test_a_tela_v3_sem_par.py:246`
- Os dois REDs falharam por `AssertionError` contra o `cartorio.py` da cópia v6, carregado diretamente. Os **3 mutantes v7** falharam por asserção, em `2/1/1` casos respectivamente. Os **6 mutantes v6 adaptados** falharam por asserção. No E3, **7 mutantes** falharam por asserção e o legado `classificacao_antes_guarda` ainda falha na montagem com `ValueError: substring not found`. O caso v6 de sem par em `nunca_bateu` injeta essa combinação pelo dublê. `app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:13`, `app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:50`, `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:19`, `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:202`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:76`
- Houve **troca de expectativa**. A v6 exigia hash filtrado; a adaptação exige `E` e preserva as asserções de veredito, ata, protestos e consulta a K6. O teste ORM trocou `pulados_foto_entregue=1` na segunda rodada normal por `pulados=1` e foto `0`, acrescentou uma rodada forçada com foto `1` e confere a igualdade do estado depois de cada uma. O E3 adicionou dependências ao dublê e preservou as asserções. A lacuna é a sequência **lote → avulso sem força**, além da mudança de vigência de K6. `app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:160`, `app/ponto/tests/test_a_tela_v3_sem_par.py:247`, `app/ponto/tests/test_a_tela_v3_sem_par.py:261`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:130`, `app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:61`

### 6. Classes de reprovação

No delta fornecido, contei **0 `except` adicionados e 0 removidos**. Encontrei zero segundo juiz, segundo escritor fora da porta, relógio novo fora da borda, derivação paralela do sinal, mudança de ordem de trava ou fallback novo. O delta mantém o sinal do pareador, K6 e a porta existentes, e retira o adiamento local de `imp`. O `except` do avulso já existia. A ordem real de pouso em relação ao pré-requisito do corte **[nao verificado]**. `app/ponto/services/cartorio.py:1229`, `app/ponto/services/cartorio.py:1245`, `app/ponto/services/cartorio.py:1535`, `app/ponto/services/cartorio.py:1621`, `app/ponto/services/cartorio.py:2056`, `app/ponto/services/cartorio.py:2248`, `app/docs/CORTES.md:640`

A incompatibilidade do campo persiste para carimbos legados. Para novas gravações, a falha é usar igualdade de **insumos brutos** como prova de que ainda vale uma decisão que dependeu de K6. Os selos estruturais O219, FND, F1C4-3, O261 e E3 passaram nas asserções extraídas. O mutante legado E3 continua sem atingir a asserção, e os REDs da v7 deixam passar a regressão avulsa. `app/ponto/services/cartorio.py:1535`, `app/ponto/services/cartorio.py:1621`, `app/ponto/tests/test_o219_versao_da_regra.py:224`, `app/ponto/tests/test_fnd_pergunta_a_juiza.py:137`, `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:218`, `app/ponto/tests/test_tripwire_o261_gate_le_o_fio.py:61`, `app/ponto/tests/test_e3_isento_na_impressao.py:122`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:76`

### 7. Resolução do conflito no merge

**Incoerente.** Rodei o censo real sobre a cópia. A catraca registra `[1328, 1492]` para B1 em `_julgar_colab_corpo`, mas a adesão está na `1328` e o recorte de batidas na **`1487`**. A `1492` hoje abre o ramo `teto_de_hoje`. Essa foi a única divergência no mapa de linhas. As identidades, as contagens `9/81`, a composição e `CLASSIFICAR` conferiram. [test_medida_turno_do_par.py:321](/home/ronald/codex_ws/7e266931b89d634a1d7262cc32d3ad9568d748b8/app/core/tests/test_medida_turno_do_par.py:321) · [test_medida_turno_do_par.py:562](/home/ronald/codex_ws/7e266931b89d634a1d7262cc32d3ad9568d748b8/app/core/tests/test_medida_turno_do_par.py:562) · [cartorio.py:1487](/home/ronald/codex_ws/7e266931b89d634a1d7262cc32d3ad9568d748b8/app/ponto/services/cartorio.py:1487) · [medida_turno_do_par.py:448](/home/ronald/codex_ws/7e266931b89d634a1d7262cc32d3ad9568d748b8/app/core/medida_turno_do_par.py:448)

Os quatro arquivos do delta têm os hashes de blob do anexo na cópia. A ancestralidade Git do merge, a suíte Django integral e a incidência em produção dos casos reproduzidos ficam **[nao verificado]**. [nao verificado]

## 25/11 05:21 · sessao 7293b1f4cbd9 · PAR-RELAMPAGO-A-LISTA-DO-ADMIN · sonda · origem=Code

Transcrição integral do trecho fornecido [nao verificado] em produção.

```text
sonda_leitura: sonda_retratadas.py [754713b38be06df3] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

2 202609 4 4
2 202610 11 11
3 202611 2 2
```

Soma das três linhas fornecidas (`4 + 11 + 2`) → **`total = 17`** e **`sem_decisao = 17`**. [nao verificado] em produção.

## 25/11 05:20 · sessao e7dc0e4eaea1 · TIPO-NA-FONTE · sonda · origem=Code

### Transcrição integral

`DADO RASPADO:1–66` do texto enviado, contando o cabeçalho da execução como linha 1. O formato desse cabeçalho consta em [bin/sonda_leitura.sh:221](/home/ronald/codex_ws/71efa33d78925687bd2f64b4c368b61e2f72837d/bin/sonda_leitura.sh:221).

```text
sonda_leitura: sonda_estoque.py [6b7d1ffa767635de] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

empresa_pk=1 competencia=2026-09 ata_none=2 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=1 ambas_descasadas=0 total=3
empresa_pk=1 competencia=2026-10 ata_none=21 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=69 ambas_descasadas=0 total=90
empresa_pk=1 competencia=2026-11 ata_none=4 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=58 ambas_descasadas=0 total=62
empresa_pk=1 competencia=2026-12 ata_none=57 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=3 ambas_descasadas=0 total=60
empresa_pk=1 competencia=2027-01 ata_none=11 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=0 ambas_descasadas=0 total=11
empresa_pk=2 competencia=2024-01 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=1600 ambas_descasadas=0 total=1600
empresa_pk=2 competencia=2024-02 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2480 ambas_descasadas=0 total=2480
empresa_pk=2 competencia=2024-03 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2320 ambas_descasadas=0 total=2320
empresa_pk=2 competencia=2024-04 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2480 ambas_descasadas=0 total=2480
empresa_pk=2 competencia=2024-05 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2400 ambas_descasadas=0 total=2400
empresa_pk=2 competencia=2024-06 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2480 ambas_descasadas=0 total=2480
empresa_pk=2 competencia=2024-07 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2400 ambas_descasadas=0 total=2400
empresa_pk=2 competencia=2024-08 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2480 ambas_descasadas=0 total=2480
empresa_pk=2 competencia=2024-09 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2480 ambas_descasadas=0 total=2480
empresa_pk=2 competencia=2024-10 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2400 ambas_descasadas=0 total=2400
empresa_pk=2 competencia=2024-11 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2480 ambas_descasadas=0 total=2480
empresa_pk=2 competencia=2024-12 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2400 ambas_descasadas=0 total=2400
empresa_pk=2 competencia=2025-01 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2480 ambas_descasadas=0 total=2480
empresa_pk=2 competencia=2025-02 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2480 ambas_descasadas=0 total=2480
empresa_pk=2 competencia=2025-03 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2240 ambas_descasadas=0 total=2240
empresa_pk=2 competencia=2025-04 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2480 ambas_descasadas=0 total=2480
empresa_pk=2 competencia=2025-05 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2400 ambas_descasadas=0 total=2400
empresa_pk=2 competencia=2025-06 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2480 ambas_descasadas=0 total=2480
empresa_pk=2 competencia=2025-07 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2380 ambas_descasadas=0 total=2380
empresa_pk=2 competencia=2025-08 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2449 ambas_descasadas=0 total=2449
empresa_pk=2 competencia=2025-09 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2449 ambas_descasadas=0 total=2449
empresa_pk=2 competencia=2025-10 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2370 ambas_descasadas=0 total=2370
empresa_pk=2 competencia=2025-11 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2449 ambas_descasadas=0 total=2449
empresa_pk=2 competencia=2025-12 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2370 ambas_descasadas=0 total=2370
empresa_pk=2 competencia=2026-01 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2449 ambas_descasadas=0 total=2449
empresa_pk=2 competencia=2026-02 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2449 ambas_descasadas=0 total=2449
empresa_pk=2 competencia=2026-03 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2212 ambas_descasadas=0 total=2212
empresa_pk=2 competencia=2026-04 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=2588 ambas_descasadas=0 total=2588
empresa_pk=2 competencia=2026-05 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=1793 ambas_descasadas=0 total=1793
empresa_pk=2 competencia=2026-06 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=995 ambas_descasadas=0 total=995
empresa_pk=2 competencia=2026-07 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=160 ambas_descasadas=0 total=160
empresa_pk=2 competencia=2026-08 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=12691 ambas_descasadas=0 total=12691
empresa_pk=2 competencia=2026-09 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=13179 ambas_descasadas=0 total=13179
empresa_pk=2 competencia=2026-10 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=13466 ambas_descasadas=0 total=13466
empresa_pk=2 competencia=2026-11 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=14096 ambas_descasadas=0 total=14096
empresa_pk=2 competencia=2026-12 ata_none=11676 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=1628 ambas_descasadas=0 total=13304
empresa_pk=2 competencia=2027-01 ata_none=779 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=0 ambas_descasadas=0 total=779
empresa_pk=2 competencia=2027-02 ata_none=31 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=0 ambas_descasadas=0 total=31
empresa_pk=2 competencia=2027-03 ata_none=28 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=0 ambas_descasadas=0 total=28
empresa_pk=2 competencia=2027-04 ata_none=31 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=0 ambas_descasadas=0 total=31
empresa_pk=2 competencia=2027-05 ata_none=30 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=0 ambas_descasadas=0 total=30
empresa_pk=2 competencia=2027-06 ata_none=31 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=0 ambas_descasadas=0 total=31
empresa_pk=2 competencia=2027-07 ata_none=16 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=0 ambas_descasadas=0 total=16
empresa_pk=3 competencia=2026-06 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=4 ambas_descasadas=0 total=4
empresa_pk=3 competencia=2026-07 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=57 ambas_descasadas=0 total=57
empresa_pk=3 competencia=2026-08 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=3552 ambas_descasadas=0 total=3552
empresa_pk=3 competencia=2026-09 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=3530 ambas_descasadas=0 total=3530
empresa_pk=3 competencia=2026-10 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=3418 ambas_descasadas=0 total=3418
empresa_pk=3 competencia=2026-11 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=3565 ambas_descasadas=0 total=3565
empresa_pk=3 competencia=2026-12 ata_none=2767 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=375 ambas_descasadas=0 total=3142
empresa_pk=3 competencia=2027-01 ata_none=187 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=0 ambas_descasadas=0 total=187
empresa_pk=4 competencia=2026-04 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=1 ambas_descasadas=0 total=1
empresa_pk=4 competencia=2026-05 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=30 ambas_descasadas=0 total=30
empresa_pk=4 competencia=2026-08 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=651 ambas_descasadas=0 total=651
empresa_pk=4 competencia=2026-09 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=620 ambas_descasadas=0 total=620
empresa_pk=4 competencia=2026-10 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=660 ambas_descasadas=0 total=660
empresa_pk=4 competencia=2026-11 ata_none=0 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=682 ambas_descasadas=0 total=682
empresa_pk=4 competencia=2026-12 ata_none=554 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=76 ambas_descasadas=0 total=630
empresa_pk=4 competencia=2027-01 ata_none=11 sem_chaves=0 so_orfas=0 so_n_orfas=0 ambas_casadas=0 ambas_descasadas=0 total=11
```

### Soma por forma na frota do dado raspado

| Forma | Soma | Fonte |
|---|---:|---|
| `ata_none` | 16.236 | `DADO RASPADO:3–66` |
| `sem_chaves` | 0 | `DADO RASPADO:3–66` |
| `so_orfas` | 0 | `DADO RASPADO:3–66` |
| `so_n_orfas` | 0 | `DADO RASPADO:3–66` |
| `ambas_casadas` | 142.485 | `DADO RASPADO:3–66` |
| `ambas_descasadas` | 0 | `DADO RASPADO:3–66` |
| **Total** | **158.721** | `DADO RASPADO:3–66` |

As 64 linhas de contagem fecham individualmente, e a soma por forma coincide com a soma da coluna `total`. `DADO RASPADO:3–66`.

A regra de competência 21–20 em `sonda_estoque.py`, o código de saída da execução e a cobertura da frota em produção ficam [nao verificado].

## 25/11 05:17 · sessao f7f8eae2d55d · TIPO-NA-FONTE · leitura · origem=Code

# TIPO-NA-FONTE item 3 — retomada 2

Na cópia identificada como `71efa33d7`, encontrei **14 leituras cruas em 10 arquivos de `app/`**, **uma leitura adicional em `bin/`** e **zero chamadas aos acessores da volta 5c**. A grade tem **34 leituras** pelo critério do pré-desenho, uma a mais que as 33 da volta 5c. A leitura adicional calcula `n_orfas` a partir da lista da grade no cartório. (`app/escala/services/leitor_celula.py:187`, `app/ponto/services/cartorio.py:760`, `bin/snapshot_o118_col221.py:32`, `12e424d5f/app/ponto/services/cartorio.py:772`)

O corte vigente fixa duas formas, **NAO LAVRADA** e **LAVRADA com lista e contagem juntas**, nesta ordem de implantação: **escritor único → estoque migrado → leitores pela porta → tripwire**. O registro de 18/11 ainda lista os estados legados do desenho anterior e exige DIFF de frota por leitor igual a zero. Estoque atual e DIFF de frota: **[nao verificado]**. (`app/docs/PROMPTS.md:2929`, `app/docs/PLANO-PADROES.md:50`, `app/docs/BACKLOG.md:1974`)

A comparação abaixo usa os snapshots locais `78994522d`, `12e424d5f` e `7621a973f`. Os hashes dos blobs da base e da volta 5c conferem com os IDs do diff anexado. A cópia recebida veio sem `.git`, portanto a identidade do commit por `git rev-parse` e a ancestralidade são **[nao verificado]**.

## 1. Censo AST e aplicação da volta 5c

O censo usa o AST de `bin/leitores_de_chave.py`, que segue aliases, desembrulha `values_list` e aceita parâmetros com proveniência declarada. Conferi também os literais em Python fora de `app/`. A unidade é a operação AST, inclusive quando duas operações ocupam a mesma linha. Chaves dinâmicas, fluxo de ramos e leituras implícitas dentro de cópias de dict têm limites de cobertura documentados. Completude além do reconhecimento estático: **[nao verificado]**. (`bin/leitores_de_chave.py:20`, `bin/leitores_de_chave.py:53`, `bin/leitores_de_chave.py:65`)

| Leitor | Receptor e leituras cruas hoje |
|---|---|
| `_celula_furada` | `ata.get('orfas')` e `ata.get('n_orfas')`, sobre ata gravada ou projetada do julgamento em curso. `app/chamados/juizes.py:473`, `app/chamados/juizes.py:559` |
| `_plantio_preenche_furo_na_data` | `ata.get('orfas')`, a partir de `cel.ata`. `app/chamados/services/materializacao.py:454`, `app/chamados/services/materializacao.py:463` |
| `_orfas_da_ata` | `ata.get('orfas')` e `ata.get('n_orfas')`, sobre o dict recebido de `grade_da_celula`. `app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:190`, `app/escala/services/leitor_celula.py:420` |
| `periodos_do_dia` | `ata.get('orfas')`, sobre ata gravada ou célula sintética da grade. `app/ponto/juiz_batida.py:165`, `app/escala/utils.py:1909`, `app/ponto/turnos.py:1633` |
| `medir_sobra_com_tipo_trocado` | `ata.get('orfas')`, repassada a `sobra_na_borda`. `app/ponto/management/commands/tripwire_celulas.py:264`, `app/ponto/management/commands/tripwire_celulas.py:165` |
| `insumos_da_ata` | `a.get('n_orfas')`, com `a` ligado ao parâmetro `ata`. `app/ponto/services/cartorio.py:851`, `app/ponto/services/cartorio.py:857` |
| `_leitura_da_celula` | `ata.get('orfas')` para validar a lista e `ata['orfas']` para iterar. `app/ponto/services/cluster_espurio.py:29`, `app/ponto/services/cluster_espurio.py:31` |
| `assinaturas_do_colab` | `(x.ata or {}).get('orfas')`, usada na carga e na assinatura A5. `app/ponto/services/esmeril_espelho.py:130`, `app/ponto/services/esmeril_espelho.py:143` |
| `censo` do vigia | `a.get('n_orfas')` decide se lê `a.get('orfas')`. `ata` vem da tupla de `values_list` e `a` é seu alias normalizado. `app/ponto/services/vigia_pareador_celula.py:69`, `app/ponto/services/vigia_pareador_celula.py:74` |
| `orfas_e_isencao_da_ata` | `ata.get('orfas')`, para o conjunto por dia. `app/relatorios/cartao_pela_celula.py:287`, `app/relatorios/cartao_pela_celula.py:288` |
| Módulo `snapshot_o118_col221` | `(c['ata'] or {}).get('orfas')`. A ata foi transportada para cada item de `cels`. O módulo também grava o arquivo do snapshot, por isso a conferência foi pelo código. `bin/snapshot_o118_col221.py:18`, `bin/snapshot_o118_col221.py:30`, `bin/snapshot_o118_col221.py:32` |

**Comparação com o pré-desenho.** Em `app/`, são **10 leituras de `orfas` e 4 de `n_orfas`**, em 12 posições. Na volta 5c eram **12 chamadas aos acessores em 10 arquivos**. As duas diferenças de unidade estão no cluster, onde duas leituras cruas viravam uma chamada, e em `_celula_furada`, onde duas leituras viravam uma. O script de `bin/` já existia na volta 5c e ficava fora do universo de `app/`. (`app/ponto/services/cluster_espurio.py:29`, `app/chamados/juizes.py:559`, `12e424d5f/app/ponto/services/cluster_espurio.py:31`, `12e424d5f/app/chamados/juizes.py:511`, `12e424d5f/bin/snapshot_o118_col221.py:32`, `app/docs/RELATO.md:3749`)

**Grade e saída-dia.** O total de 34 conta `.get` e subscript em leitura. Contando também o teste de presença `'orfas' in dia` no supra, são **35 operações AST no HEAD e 34 na volta 5c**. Os dois usos do contador agregado do Esmeril e o mapa lido por `cadastro_realidade` pertencem ao retrato do Esmeril. (`app/ponto/supra_juiz.py:527`, `12e424d5f/app/ponto/supra_juiz.py:514`, `app/ponto/services/esmeril_espelho.py:236`, `app/escala/services/cadastro_realidade.py:1032`)

| Arquivo | Leituras da grade no HEAD |
|---|---|
| `app/colaboradores/services/calendario.py` | `:409`, `:410` |
| `app/escala/orfa_classe.py` | `:58` |
| `app/escala/services/furos_diarios.py` | `:22` |
| `app/escala/utils.py` | `:810`, `:813`, `:814`, `:1054`, `:1121`, `:1843`, `:1845`, `:1848`, `:1849`, `:1905`, `:1906`, `:1979`, `:1984` |
| `app/folha/causas.py` | `:43`, `:100` |
| `app/ponto/management/commands/detectar_par_relampago.py` | `:140` |
| `app/ponto/management/commands/propor_reorganizacao.py` | `:104` |
| `app/ponto/propositor.py` | `:39` |
| `app/ponto/services/bordas_realizado.py` | `:49` |
| `app/ponto/services/cartorio.py` | `:754`, `:760`, `:763`, `:1454` |
| `app/ponto/services/simular_dia.py` | `:12` |
| `app/ponto/status_dia.py` | `:188` |
| `app/ponto/supra_juiz.py` | `:392`, `:527` |
| `app/ponto/turnos.py` | `:2684` |
| `app/relatorios/pdf_espelho.py` | `:477`, `:552` |

### Os 23 arquivos do diff

**Limpo** abaixo significa que o hunk passou na checagem textual em memória. Na inserção de import de `chamados/juizes.py`, o patch textual falha e a comparação de três vias resolve. Em materialização, esmeril e no teste de medida, a comparação de três vias também acusa conflito. As referências abaixo identificam os trechos comparados. (`app/chamados/juizes.py:1`, `app/chamados/services/materializacao.py:463`, `app/ponto/services/esmeril_espelho.py:130`, `app/core/tests/test_medida_turno_do_par.py:248`)

| Arquivo | Delta `78994522d` → HEAD e encaixe dos hunks |
|---|---|
| `app/chamados/juizes.py` | O módulo ganhou imports e o tipo de resultado de slot. Mudaram `_dia_do_contexto_fraco`, `_slot_esperado` e `_pergunta_materializada_de_fato`. Mudaram também janela viva, chave de memo, tranca avulsa e em lote, memos por estampa, projeção temporal, vitalidade, juiz de resposta e suas faces SQL, alimentadores de fila/porta, consultas de irmãs e disputas, e o contador O168. `_celula_furada` conserva o corpo da base. **Import inicial colide textualmente. Hunk do leitor limpo.** `app/chamados/juizes.py:2`, `:143`, `:347`, `:360`, `:633`, `:712`, `:869`, `:1023`, `:1054`, `:1155`, `:1221`, `:1471`, `:1494`, `:1604`, `:1639`, `:1661`, `:1690`, `:1723`, `:1787`, `:1850`, `:2175`, `:2518`, `:2567`, `:2690`, `:559` |
| `app/chamados/services/materializacao.py` | `_plantio_preenche_furo_na_data` ganhou `pergunta=`, eleição da célula pelo dono/data do fato e conversão de slots e marcas para o eixo do turno. No restante, o delta agrupa vizinhas/irmãs/BUG-PAR, declarado × gravado e K6, estado de aplicação, materialização de ausência e resposta, anuência, plantio da irmã e travas/releitura do lote. **Import limpo. Hunk do leitor colide** no contexto que hoje faz o cálculo do eixo. `app/chamados/services/materializacao.py:95`, `:139`, `:182`, `:262`, `:309`, `:418`, `:447`, `:463`, `:465`, `:738`, `:802`, `:879`, `:1009`, `:1211`, `:1374`, `:1403`, `:1667`, `:1754`, `:1817`, `:1951`, `:2179`, `:2198`, `:2234`, `:2305` |
| `app/config/settings/ci.py` | Igual à base. **Limpo** para a troca do runner e a configuração da catraca daquela volta. `app/config/settings/ci.py:40` |
| `app/core/tests/test_medida_turno_do_par.py` | A composição e os registros do módulo avançaram. **Inclusão dos seis nomes da porta em `COMPOSICAO_REGISTRADA` limpa. Cinco hunks de `LINHAS_REGISTRADAS` colidem**, cobrindo pareadores, juízes de chamados, materialização, grade, cartório e esmeril. `app/core/tests/test_medida_turno_do_par.py:104`, `:248`, `:257`, `:272`, `:282`, `:319`, `:323` |
| `app/core/tests/test_quarentena.py` | `PodarSuiteTest` ganhou prova do log dos podados. **Limpo** o hunk do teste de herança do runner. `app/core/tests/test_quarentena.py:61`, `:145` |
| `app/escala/apps.py` | Igual à base. **Limpo** em `EscalaConfig.ready`. `app/escala/apps.py:8` |
| `app/escala/runner_ata_ci.py` | Arquivo novo na volta 5c, caminho livre no HEAD. **Adição limpa** de `RunnerAtaVigiada.run_suite`. `12e424d5f/app/escala/runner_ata_ci.py:9` |
| `app/escala/services/ata.py` | Arquivo novo na volta 5c, caminho livre no HEAD. **Adição limpa** da porta antiga, dos acessores, do censo e da catraca. O corte vigente define o contrato a implementar. `12e424d5f/app/escala/services/ata.py:108`, `:220`, `:266`, `:347`, `app/docs/PROMPTS.md:2929` |
| `app/escala/services/ata_ci.py` | Arquivo novo na volta 5c, caminho livre no HEAD. **Adição limpa** do descriptor e do hook da suíte. `12e424d5f/app/escala/services/ata_ci.py:13`, `:31`, `:49` |
| `app/escala/services/leitor_celula.py` | Igual à base. **Três hunks limpos**, no import, em `n_ok_lavrado` e em `_orfas_da_ata`. `app/escala/services/leitor_celula.py:11`, `:85`, `:180` |
| `app/escala/tests/test_tipo_item3_selo_classe_fallback.py` | Arquivo novo na volta 5c, caminho livre no HEAD. **Adição limpa** do selo de fallback da volta reprovada. `12e424d5f/app/escala/tests/test_tipo_item3_selo_classe_fallback.py:116`, `app/docs/RELATO.md:3757` |
| `app/escala/tests/test_tipo_item3_v5_expand.py` | Arquivo novo na volta 5c, caminho livre no HEAD. **Adição limpa** dos testes de Expand, inclusive expectativas do campo paralelo retirado pelo aval A. `12e424d5f/app/escala/tests/test_tipo_item3_v5_expand.py:78`, `:205`, `app/docs/RELATO.md:3750` |
| `app/escala/utils.py` | Entrou `_protegida_da_guarda`, usada por `_alinhar_por_papel` e `_match_marcos` para preservar o marco próprio fora da rajada. **Seis hunks limpos**, no import, em `_celula_do_marco`, nos dois ramos de `_match_marcos` e nos dois montadores da grade. A ata sintética só com lista continua em `:1906`. `app/escala/utils.py:502`, `:589`, `:660`, `:810`, `:1843`, `:1906` |
| `app/folha/previa.py` | `montar_previa` carrega decisões e `_material` consulta o juiz de resposta. **Dois hunks limpos**, no import e no teste `dict` de `montar_previa`. `app/folha/previa.py:66`, `:83`, `:139` |
| `app/folha/validacao_pdf.py` | Igual à base. **Dois hunks limpos**, no import e em `referencia_do_espelho`. `app/folha/validacao_pdf.py:19`, `:47` |
| `app/ponto/juiz_batida.py` | `intervalo_cadastrado` ganhou o dia do turno e `periodos_do_dia` ganhou os ramos para duração prevista, DNA sem modo e dia sem pausa. **Dois hunks limpos**, no import e na leitura de órfãs. `app/ponto/juiz_batida.py:106`, `:129`, `:165`, `:307`, `:325` |
| `app/ponto/management/commands/tripwire_celulas.py` | Igual à base. **Cinco hunks limpos**, no import, na peneira, na extração de `_hoje` e nos dois trechos de `Command.handle`. `app/ponto/management/commands/tripwire_celulas.py:8`, `:264`, `:350`, `:357`, `:411` |
| `app/ponto/portas/celula.py` | Desde a base, regenerações passaram a limpar julgamento de futura com trilha. Mudaram também edição, troca, reversão, geração de período e a documentação de emissão. **Três hunks limpos**, em `celula_como_o_julgamento_deixa`, `lavrar_veredito` e `carimbar_precedencia`. `app/ponto/portas/celula.py:112`, `:404`, `:579`, `:688`, `:765`, `:803`, `:899`, `:981`, `:1040`, `:1090`, `:1161`, `:1249` |
| `app/ponto/services/cartorio.py` | `chamados_que_falam_de_batida` passou a carregar decisões. `julgar_colab` e `_julgar_colab_corpo` juntaram fecho, rejeição e ocorrência no mesmo ato. `_protesta` passou a consultar o juiz de dia sem trabalho com a ata desta lavra, e `_tem_resposta_do_colab` passou ao juiz L-130. **Quatro hunks limpos**, no import, nos dois trechos de `ata_do_dia` e em `insumos_da_ata`. `app/ponto/services/cartorio.py:48`, `:144`, `:748`, `:834`, `:1030`, `:1492`, `:1574`, `:1687` |
| `app/ponto/services/cluster_espurio.py` | Igual à base. **Limpo** em `_leitura_da_celula`. `app/ponto/services/cluster_espurio.py:27` |
| `app/ponto/services/esmeril_espelho.py` | `assinaturas_do_colab` passou a medir `len(_orfas)` na mesma passagem de A5. `anomalias_da_frota` publica a carga e o início da janela. **Import limpo. Hunk do leitor colide** na atribuição e iteração novas. `app/ponto/services/esmeril_espelho.py:79`, `:130`, `:143`, `:224`, `:236` |
| `app/ponto/services/vigia_pareador_celula.py` | Igual à base. **Dois hunks limpos** em `censo`. `app/ponto/services/vigia_pareador_celula.py:54`, `:73` |
| `app/relatorios/cartao_pela_celula.py` | `folha_manda` passou a aceitar `agora` da borda e repassá-lo ao turno vivo e ao corte do dia. **Dois hunks limpos**, no import e em `orfas_e_isencao_da_ata`. `app/relatorios/cartao_pela_celula.py:288`, `:296`, `:398`, `:412` |

**`ponto/portas/celula.py` desde `7621a973f`.** O snapshot já contém `desfazer_julgamento_da_futura`. O delta até a cópia atual passa `hoje` de `regenerar_celulas_vinculo` a `_o_chao_do_dia_mudou`, acrescenta a guarda `so_se_folga` sob trava em `editar_dia`, compartilha `agora` entre os dois lados de `trocar_plantao`, condiciona o `UPDATE` de `reverter_regeneracao_dia` à foto original e documenta o emissor do vigia. Os três trechos de transporte tocados pela volta 5c ficaram iguais aos de `7621a973f`. (`7621a973f/app/ponto/portas/celula.py:1124`, `app/ponto/portas/celula.py:392`, `:404`, `:644`, `:688`, `:855`, `:988`, `:1040`, `:1090`, `:1249`)

## 2. Escritores e montagens de ata

| Sítio | Forma que produz |
|---|---|
| `ata_do_dia` | **Ambas casadas**, inclusive `[]` e `0` quando a lista da grade está vazia. É a origem dos dois valores na ata do cartório. `app/ponto/services/cartorio.py:748`, `:760`, `:763` |
| `montar_grade_prevista_periodo_por_turno` | **Só lista**, inclusive vazia, na cópia de `CelulaDia` entregue ao pareador. Para dias de padding fora da montagem, reutiliza a célula com a ata que veio do banco. `app/escala/utils.py:1892`, `:1895`, `:1898`, `:1906`, `:1909` |
| `_julgar_colab_corpo` | No julgamento corrente e no ramo normal de gravação, monta **ambas** via `ata_do_dia`. No ramo `nunca_bateu`, monta **ambas** se tem dia da grade ou passa `None` à lavra. O `None` de entrada deixa a ata anterior sem substituição. `app/ponto/services/cartorio.py:1279`, `:1320`, `:1464`, `:1899`, `app/ponto/portas/celula.py:1085` |
| `dispensar_por_ausencia` e `_ata_ate_o_teto` | Preservam a forma de entrada. O primeiro altera lâmpadas e `n_missing` na mesma ata. O segundo pode montar uma cópia com lâmpadas e `n_missing` atualizados. `app/ponto/services/cartorio.py:820`, `:830`, `:925`, `:935` |
| `celula_como_o_julgamento_deixa` e `lavrar_veredito` | Copiam a forma recebida. A primeira operação projeta em memória. A segunda grava quando a ata muda e preserva a chave `precedencia` de outra operação. `app/ponto/portas/celula.py:1035`, `:1040`, `:1090`, `:1101`, `:1128` |
| `carimbar_precedencia` | Preserva as chaves presentes. Com `ata=None`, monta um dict de precedência com **nenhuma** das duas chaves. `app/ponto/portas/celula.py:1247`, `:1249`, `:1251` |
| `desfazer_julgamento_da_futura` | Grava **`None`** para a ata entre os campos limpos. Conserva o julgamento quando a via é de admin. `app/ponto/portas/celula.py:1149`, `:1192`, `:1202` |

O AST reconheceu **dois produtores dos valores de órfãs** no HEAD, o cartório e a grade, com três escritas literais. Não apareceu produtor de **só contagem** nesse censo. Os demais sítios acima transportam ou limpam o envelope. (`bin/leitores_de_chave.py:22`, `app/ponto/services/cartorio.py:760`, `app/ponto/services/cartorio.py:763`, `app/escala/utils.py:1906`, `app/ponto/portas/celula.py:1090`, `app/ponto/portas/celula.py:1202`)

## 3. Matriz dos leitores no HEAD

`L` é a lista fornecida e `n` a contagem fornecida. A coluna `{}` é o dict literalmente vazio. Uma ata sem as duas chaves que contenha lâmpadas pode passar por guardas que `{}` não passa. Nas colunas com uma ou duas chaves, valem as guardas da última coluna. **↓** marca quando `None` vira vazio, zero ou uma resposta negativa equivalente para órfãs. (`app/chamados/juizes.py:542`, `:554`, `app/chamados/services/materializacao.py:454`, `:457`, `app/ponto/management/commands/tripwire_celulas.py:258`, `:261`)

| Leitor | `None` | `{}` | Só lista | Só contagem | Ambas descasadas | Ambas casadas | Guardas e efeito |
|---|---|---|---|---|---|---|---|
| `_celula_furada` | `None` | `None` | Testa verdade de `L` | Testa verdade de `n` | Testa `L or n` | Testa `L or n` | Primeiro julga ausência e dia sem trabalho. Sem `lampadas`, retorna `None`. Uma lâmpada apagada basta. Órfã só prende com régua confiável. `app/chamados/juizes.py:525`, `:542`, `:554`, `:559`, `:565` |
| Materialização | Pula a célula | Pula a célula | Acrescenta as `HH:MM` válidas à lista de marcas | Sem marcas de órfãs | Usa `L` | Usa `L` | Exige dia de trabalho e lâmpadas. Sem outra célula que comprove o furo, sai `False`. A captura de erro também retorna `False`. `app/chamados/services/materializacao.py:454`, `:457`, `:463`, `:474`, `:477` |
| `_orfas_da_ata` | **↓ `[]` via grade** | `[]` | Converte elementos verdadeiros em `str` | `['?'] * int(n or 0)` | Prefere a lista filtrada, usa `n` se ela esvaziar | Prefere a lista filtrada, usa `n` se ela esvaziar | Chamado diretamente com `None`, levanta em `.get`. `grade_da_celula` entrega `{}`. `app/escala/services/leitor_celula.py:187`, `:190`, `:379`, `:420` |
| Juiz de batida | **↓ `[]`** | `[]` | `[str(h) for h in L]` | `[]` | Usa `L` | Usa `L` | Qualquer ata que não seja `dict` vira `{}`. As órfãs entram separadas no resultado. `app/ponto/juiz_batida.py:161`, `:165`, `:168` |
| Tripwire de bordas | Pula, fora do universo | Pula, fora do universo | Peneira `L` | Peneira recebe `None` e usa `[]` | Peneira usa `L or []` | Peneira usa `L or []` | Exige ata verdadeira e dois marcos. A peneira filtra horas e a etapa posterior confirma pelo instante da batida. `app/ponto/management/commands/tripwire_celulas.py:165`, `:258`, `:261`, `:264`, `:283` |
| `insumos_da_ata` | **↓ `(False, False)`** | `(False, False)` | `(False, False)` | `n` verdadeiro → `(None, None)`, senão `(False, False)` | Mesma regra de `n` | Mesma regra de `n` | As quatro últimas colunas descrevem o ramo sem lâmpadas. Com lâmpadas, resolve pelos marcos do DNA e pelo estado das lâmpadas. `app/ponto/services/cartorio.py:851`, `:857`, `:860`, `:873` |
| Cluster | `None`, não medido | `None`, não medido | Mede se `L` e `lampadas` forem listas | `None`, não medido | Mesma regra de tipos da lista | Mesma regra de tipos da lista | Dia sem trabalho retorna `()` antes. Com insumo, retorna assinatura quando há rajada de até 15 min e lâmpada apagada. Demais dias medidos retornam `()`. `app/ponto/services/cluster_espurio.py:25`, `:29`, `:31`, `:36`, `:37` |
| Esmeril | **↓ zero na carga e nada para A5** | Zero na carga e nada para A5 | Soma `len(L)` e classifica as horas | Zero na carga e nada para A5 | Usa `L` | Usa `L` | A via `so_escala` sai antes. A5 conta horas em faixas de meia hora e publica faixas com três ou mais ocorrências. `app/ponto/services

[truncado em 20000 de 31146 caracteres]

## 25/11 05:15 · sessao 86f69f846fb0 · PAR-RELAMPAGO-A-LISTA-DO-ADMIN · leitura · origem=Code

## Pré-desenho — PAR-RELAMPAGO-A-LISTA-DO-ADMIN

**Tirar `--retratar` do cron interrompe a retratação automática, mas entrega apenas parte dos sinais a outra tela.** Com `--apply` sozinho, o detector abre chamados de supervisão para os pares do primeiro ramo. A varredura dos pares embutidos fica desligada. A entrega à lista **Cadastro x Realidade** precisa de um escritor e de um leitor explícitos. `app/config/crons.py:605` · `app/ponto/management/commands/detectar_par_relampago.py:352–401` · `app/escala/views.py:511–529`

A análise usa os arquivos da cópia e os anexos transcritos na mensagem. A identidade Git do HEAD, a execução dos crons em produção, a publicação das 142 na UI e os números da sonda proposta ficam [nao verificado].

### 1. Retratação por código hoje

| Caminho | O que faz | Prova |
|---|---|---|
| `detectar_par_relampago --apply --retratar`, primeiro ramo | `pares_relampago` recolhe turnos curtos e pares sob o carimbo de degenerado. `handle` exige `par_completo`. `_alvo_retratavel` carrega aprovadas de D−1 a D+1, aplica a guarda de procedência e de 360 s e chama `escolher_retratavel`. A exclusão simulada de **uma** batida tem de ser a única a melhorar o vetor e deixá-lo em `(0,0,0,0)`. Havendo alvo, `_retratar` grava e o ramo pula a abertura de chamado. | `app/ponto/management/commands/detectar_par_relampago.py:63–126,158–248,352–368` |
| Mesmo comando, segundo ramo | `varrer_pares_embutidos` examina adjacentes aprovadas de tipos opostos, exige `par_completo` e simula retirar **as duas**. Se mais de um par passa no mesmo dia, escolhe o mais tardio pelo timestamp da primeira batida. As duas chamadas de `_retratar` estão em `399–400`. | `app/ponto/management/commands/detectar_par_relampago.py:251–314,389–401` |
| Flags | Sem `--apply`, ambos os ramos ficam em DRY. `--so-retratar` implica `--retratar` e, com `--apply`, suprime a abertura de chamado quando falta alvo individual. `--apply` sozinho percorre o primeiro ramo e abre ou recupera chamado. | `app/ponto/management/commands/detectar_par_relampago.py:320–334,352–401` |
| Cron | Diário, `06:26`, `--dias 2 --apply --retratar`, log `par_relampago.log`, estágio `cobranca`, dependência declarada `reconciliar_grade`. | `app/config/crons.py:605` |

`_retratar` chama `registro_batida.retratar_batida` com `acao='retratar_par_relampago'` e sem `request` ou ator humano. A porta retorna cedo se a batida já estiver retratada. Caso contrário, grava `aprovada=False` e `retratada_em`, anota a trilha de `Batida` pelo PK e chama a porta de recálculo por evento. `app/ponto/management/commands/detectar_par_relampago.py:217–226` · `app/ponto/registro_batida.py:245–252,272–302` · `app/ponto/services/fechamento.py:1798–1827`

A busca por chamadas de `retratar_batida`, escritas de `retratada_em` e ações de auditoria encontrou também estes caminhos. `app/ponto/registro_batida.py:287–302` · `app/ponto/services/retratar_humano.py:179–209` · `app/chamados/services/acoes_chamado.py:251–289` · `app/ponto/management/commands/passivo_disputa_retro.py:154–172` · `app/ponto/management/commands/diff_passivo_s84.py:75–88`

- **`passivo_disputa_retro --apply`** seleciona em lote batidas plantadas pelo fluxo S84, sem pergunta de origem, em folga e dentro do escopo. Chama a porta com `retratar_passivo_s84`, sem decisão individual, por ato avulso sob aval documentado. `app/ponto/management/commands/passivo_disputa_retro.py:60–76,116–172` · `app/config/crons.py:852–868`
- **`diff_passivo_s84`** chama a mesma porta com a mesma ação, dentro do ensaio que força rollback. É uma escrita durante a transação de medição. `app/ponto/management/commands/diff_passivo_s84.py:75–91` · `app/ponto/services/diff_frota.py:70–92`
- **Retratação pelo admin e recusa de geofence** chegam por decisões humanas com motivo. O `--so-retratar` homônimo de `reconciliar_grade` atua sobre `ChamadoColaborador`. O serviço de cluster também abre e fecha chamados. `app/ponto/views.py:3453–3462` · `app/ponto/services/retratar_humano.py:160–185` · `app/chamados/views.py:544–545` · `app/chamados/services/fio.py:669–682` · `app/chamados/services/acoes_chamado.py:251–286` · `app/ponto/management/commands/reconciliar_grade.py:72,250–254,298–316` · `app/ponto/services/cluster_espurio.py:176–216`

### 2. Lista do admin e destino das 142

- **A lista em código é Cadastro x Realidade.** A rota `cadastro-x-realidade/` exige `ver_escala`, chama `cadastro_realidade.lista` com as empresas visíveis e renderiza `escala/cadastro_x_realidade.html`. O vigia do fio também chama essa composição em `ListaDoAdmin`. A tela reúne dados de cadastro lidos na hora e lavras em `MetricaSnapshot`. O cron `esmeril_espelho --apply` escreve a lavra principal por `gravar_lavra`, lida por `ler_lavra`. C8 é um precedente de ponta a ponta para a obra: `detectar_vinculo_divergente --lavrar` → `vinculo_na_lista.lavrar` → `MetricaSnapshot` → `_vinculo_divergente` → tela. `app/escala/urls.py:14–19` · `app/escala/views.py:511–529` · `app/chamados/services/vigia_fio.py:166–195` · `app/config/crons.py:640` · `app/ponto/management/commands/esmeril_espelho.py:15–17` · `app/ponto/services/esmeril_espelho.py:321–329,342–375` · `app/ponto/management/commands/detectar_vinculo_divergente.py:32–52` · `app/ponto/services/vinculo_na_lista.py:112–136` · `app/escala/services/cadastro_realidade.py:393–402,910–935`
- **A ação segue o dono.** Nas assinaturas por dia, BATIDA leva à regularização da batida e CADASTRO à porta da assinatura, como vínculo, template, fio ou plano de folgas. O calendário oferece inversão e retratação com motivo por marcação. A aplicação de proposta de escala é um POST de gestor e usa a proposta guardada na lavra. `app/escala/services/cadastro_realidade.py:744–805,1053–1081` · `app/templates/colaboradores/partials/_calendario_grade.html:127` · `app/ponto/views.py:3439–3462` · `app/escala/views.py:475–508`
- **O destino atual do detector é o Painel de Gestão de Chamados.** `ChamadoColaborador.abrir` grava ou recupera um chamado por colaborador, módulo `par_relampago` e dia, com categoria `batida_espuria`, área `supervisao` e JSON com `data_turno`, duas horas `HH:MM` e `duracao_min`. O painel do DP/gestor lê o modelo e aplica os filtros da fila. O chamado guarda horas, sem os PKs das batidas. O reconciliador devolve indeterminado quando a hora falta ou é ambígua, e considera morta a premissa se encontra membro retratado ou os dois tipos iguais. O mapa de disputa deixa `par_relampago` sem motivo atendido. `app/ponto/management/commands/detectar_par_relampago.py:368–386` · `app/chamados/models.py:899–909,1037,4127–4133` · `app/chamados/views.py:720–737,768` · `app/chamados/services/painel.py:96–132,300–331` · `app/templates/chamados/partials/_lista_chamados.html:30–65` · `app/chamados/reconciliador.py:183–224,295–296`
- **As 142 são `PerguntaDisputa`.** O destino registrado pelo aval é a lista do admin sob o rótulo “respondida pelo sistema sem o colab”, com `respondida_em` preservado, sem reabertura no app e com `col331/#22394` fora das 142. O RELATO aponta o inventário nominal em `logs/E3-LEITORES/e3_10/app/lista_142_historico_para_pendentes.txt`. A publicação na tela e o arquivo apontado estão [nao verificado]. A cura de novas escritas está nas portas `responder_automaticamente` e `_nascer`, que recusam o par ilegal. `app/docs/BACKLOG.md:2294` · `app/docs/PROMPTS.md:2253` · `app/docs/RELATO.md:3869` · `app/chamados/models.py:2678,3114–3118` · `app/chamados/services/disputa_emissao.py:1720–1738`

**Desenho proposto:** publicar o sinal BATIDA em Cadastro x Realidade, com os dois PKs e a data do fato, ligado à regularização. Conferir a cobertura dos dois ramos antes de retirar as flags. O segundo ramo hoje só existe com retratação habilitada, e o primeiro só publica chamado quando não toma o atalho de escrita. O anexo S1 informa 24 pares E/S até 60 s, com 20 replays `E` e quatro `S`. Esse recorte traz horários e replays. O tipo enviado no primeiro toque, o ramo original, o cruzamento por PK com as retratações e a decisão humana por batida ficam [nao verificado]. `app/docs/BACKLOG.md:2297` · `app/escala/services/cadastro_realidade.py:792–803` · `app/ponto/management/commands/detectar_par_relampago.py:352–401`

### 3. Órfãos pela L-111

| Símbolo ou trecho | Chamadores de produção encontrados e efeito da retirada |
|---|---|
| `_retratar` e o par de chamadas `399–400` | `Command.handle:363,399–400` são os três sítios. A função e as três chamadas saem com a escrita automática. `app/ponto/management/commands/detectar_par_relampago.py:217–226,362–365,389–401` |
| `_alvo_retratavel` e `escolher_retratavel` | `handle:355` é o chamador de `_alvo_retratavel`, que chama `escolher_retratavel` em `214`. `_melhora_estrita` só é chamada por `escolher_retratavel`. `app/ponto/management/commands/detectar_par_relampago.py:154–155,199–214,229–248,352–355` |
| `motivo_para_nao_retratar`, `_score_geometria` e o ramo embutido | A guarda tem dois chamadores, `_alvo_retratavel` e `par_embutido_retratavel`. O score atende `escolher_retratavel` e `_par_embutido_retratavel_geometria`. A cadeia embutida só é acionada em produção por `handle:392`. Se a publicação reaproveitar parte da detecção embutida, o critério que tiver leitor continua vivo. `app/ponto/management/commands/detectar_par_relampago.py:129–155,199–214,229–314,389–392` |
| `--so-retratar` | No detector, entra nas guardas de `333–334` e `366–367`. A entrada de cron encontrada usa `--retratar`. A outra flag homônima pertence ao reconciliador de chamados. `app/ponto/management/commands/detectar_par_relampago.py:320–334,366–367` · `app/config/crons.py:605` · `app/ponto/management/commands/reconciliar_grade.py:72,250–254,303–316` |

**Testes.** Os de seleção v3/v4 e os de procedência, teto e costura do retratador dependem das funções acima. O teste de par pelo turno chama a varredura e o DRY de `--retratar`, mas também cobre separadamente o juiz e o papel do cron. A retirada pela L-111 alcança os testes dos sítios extintos. Há cobertura independente para piso de 60 s, pontualidade de 360 s, exclusão de retratadas do cartório, relavratura da porta e recusa de desfazer estoque do cron. `app/docs/LEIS.md:137` · `app/ponto/tests/test_par_relampago_v3.py:48–91` · `app/ponto/tests/test_guardas_par_relampago.py:48–107,129–245` · `app/ponto/tests/test_par_relampago_pelo_turno.py:53–92` · `app/ponto/tests/test_o316_par_de_um_minuto_nao_e_par.py:59–155,171–249` · `app/ponto/tests/test_par_relampago_nao_se_julga.py:43–92` · `app/ponto/tests/test_selo_b5d_retratada_fora_do_juizo.py:75–106` · `app/ponto/tests/test_retratacao_relavra_o_dia.py:24–40` · `app/ponto/tests/test_porta_retratar_batida.py:157–169`

**Papel e referências vivas.** `crons.py:1997–2021` conserva o histórico da escrita e da migração incompleta. A declaração efetiva já está em `PRAZO_DELEGA_A`, com `par_completo`, e o comando já saiu de `JUIZES_POR_VARREDURA`. O contador lê o papel efetivo. A nota de `SEM_JUIZ['batida']`, o texto do placar e a descrição dos dois ramos pedem atualização. O registro de **par completo** tem leitor no detector, `pares_relampago` também é chamado por `tripwire_celulas`, e as constantes de 60 e 360 s têm leitores em `turnos.py` e no motor. `app/config/crons.py:1997–2021,2065–2073,2152–2156,2197–2200,2382–2391` · `app/core/juizes.py:244–248,1223–1226` · `app/core/placar_estrutural.py:529–540` · `app/ponto/management/commands/tripwire_celulas.py:251,287–296` · `app/ponto/turnos.py:117–131` · `app/ponto/motor_calculo_v2.py:1146–1159`

### 4. Dependentes do cron

| Cron | Dependência no corpo |
|---|---|
| `reconciliar_vinculo` | O cron usa `--alarme`. O serviço lê as últimas batidas com coordenadas do colaborador e compara com o posto. A consulta inclui retratadas e independe da retirada do par. `app/config/crons.py:489` · `app/ponto/management/commands/reconciliar_vinculo.py:25–62` · `app/ponto/services/geofence.py:40–67` |
| `verificar_nucleo` | Compara `TurnoMaterializado` com a geometria ao vivo. A consulta ao vivo exclui retratadas, e a reconstrução da tabela materializada usa o mesmo `turnos_do_colab`. Conservar batidas pode mudar os turnos comparados. O verificador testa a igualdade das duas leituras. `app/config/crons.py:544` · `app/ponto/management/commands/verificar_nucleo.py:16–41` · `app/ponto/turnos.py:1850–1871` · `app/ponto/nucleo.py:40–59` |
| `detectar_vinculo_divergente` | Lê GPS por janela e calcula evidência de vínculo. A consulta inclui retratadas. `--lavrar` publica C8. `app/config/crons.py:597` · `app/ponto/management/commands/detectar_vinculo_divergente.py:32–52` · `app/ponto/services/vinculo_na_lista.py:32–57,112–123` |
| `flip_automatico` | É o dependente que pode voltar a **agir** sobre um membro preservado. `avaliar_fila` consulta dia encerrado, pula retratadas no alvo e nos candidatos à simulação e, se o veredito permitir, o cron chama `flip_tipo`. A porta também recusa retratada. O efeito concreto depende do candidato, da grade e das demais guardas. `app/config/crons.py:707` · `app/ponto/services/flip_auto.py:223–245,268–310` · `app/ponto/management/commands/flip_automatico.py:32–68` · `app/ponto/services/flip_batida.py:10–33` |

### 5. Apuração, dinheiro e sonda

`batidas_apuraveis` seleciona por `retratada_em IS NULL`. Ao cessar a escrita automática, o membro que seria excluído permanece disponível na respectiva janela. O estoque já carimbado continua fora por esse mesmo predicado. `app/ponto/turnos.py:2877–2904` · `app/ponto/registro_batida.py:287–301`

- **Motor e fechamento:** o fechamento carrega as apuráveis, passa as batidas ao motor por fatia e grava os resultados. A retratação hoje também dispara o recálculo daquele colaborador na competência do fato. `app/ponto/services/fechamento.py:279–290,628–633,749–755,1137–1165,1798–1824`
- **Cartório:** as apuráveis alimentam grade, ata e veredito. A batida preservada pode ocupar um marco ou ficar órfã, alterando as lâmpadas e o julgamento. `app/ponto/services/cartorio.py:1184–1207,1498–1504,1899–1906` · `app/ponto/tests/test_selo_b5d_retratada_fora_do_juizo.py:75–106`
- **Espelho e PDF:** a apuração ao vivo usa as apuráveis. A linha de pago lê `DiaPago`. No calendário, as marcas retratadas já aparecem em separado, riscadas. `app/ponto/services/espelho.py:819–831,1055–1058,1198–1202` · `app/relatorios/pdf_espelho.py:267–272,418–420,901–910` · `app/colaboradores/services/calendario.py:268–284,652–673` · `app/templates/colaboradores/partials/_calendario_grade.html:127`
- **Export:** `classificar_export` lê apuráveis diretamente nas guardas de férias com batida e de fechamento zerado com duas ou mais marcas. A porta compara espelho e `DiaPago` com fechamento. O TXT monta as rubricas a partir dos fechamentos selecionados. O efeito no arquivo depende da nova apuração, da elegibilidade e da geração. `app/folha/export.py:485–504,902–924,439–451,711–739` · `app/folha/porta_export.py:326–335,455–476` · `app/folha/views.py:264–270`

**O delta diário depende do novo pareamento.** Um E→S de até 60 s que tentaria fechar um turno cai na guarda de par degenerado. No ramo principal, a S vira órfã e a E segue aberta para uma saída posterior. Se as marcas mudarem uma borda ou pausa de outro turno, muda o intervalo líquido daquele turno. Acima do piso, um microturno fechado pode contribuir com sua duração efetiva. O motor aplica a janela de HE às pontas, desconta pausas e calcula o excedente conforme previsto, ciclo, folga e feriado. Um período aberto ou de até 360 s também pode zerar **atraso e saída antecipada** do dia pelo teto de pontualidade. O valor e o sinal do delta dos 24 pares estão [nao verificado]. `app/ponto/turnos.py:117–131,1482–1502,707–768` · `app/ponto/motor_calculo_v2.py:1131–1159,1752–1763,2470–2510,2750–2787,2807–2854` · `app/ponto/janela_he.py:76–103,117–153`

**Sonda em texto, para shell Django no tenant com credencial leitora.** Conta o estoque ainda retratado cuja última ação de retratação é `retratar_par_relampago`, com `retratada_em` nos últimos 90 dias. Agrupa pela data local da **batida**, no recorte 21→20 pedido, e pelo PK da empresa atual do colaborador. `sem_decisao` significa sem ato humano posterior auditado diretamente no PK da batida. Decisões só no dia, no chamado ou sem trilha por batida ficam [nao verificado]. Em particular, o clique de retratar uma batida já retratada retorna antes de gravar novo ato. Saída numérica em ordem `empresa_pk AAAAMM total sem_decisao`. `app/ponto/janelas.py:10–33,60–63` · `app/ponto/turnos.py:45–48` · `app/ponto/models.py:22–24,62–71` · `app/colaboradores/models.py:321` · `app/core/models.py:285–307` · `app/ponto/services/retratar_humano.py:30–32,173–185,207–209` · `app/ponto/services/flip_batida.py:10–33` · `app/ponto/services/veredito_celula.py:322` · `app/chamados/services/acoes_chamado.py:224–231,277–288`

```python
from collections import defaultdict
from datetime import timedelta

from django.db.models import Exists, OuterRef, Subquery
from django.utils import timezone

from core.models import LogAuditoria
from ponto.janelas import janela_atual
from ponto.models import Batida
from ponto.services.retratar_humano import ACAO, ACAO_DESFAZER
from ponto.turnos import data_local

agora = timezone.now()
acao_par = "retratar_par_relampago"
acao_geo = "recusar_batida_fora_do_posto"

trilha = LogAuditoria.objects.filter(
    modelo="Batida",
    objeto_id=OuterRef("pk"),
    criado_em__lte=agora,
)
ultima_retratacao = (
    trilha.filter(acao__in=(acao_par, "retratar_passivo_s84", ACAO, acao_geo))
    .order_by("-criado_em", "-pk")
    .values("acao")[:1]
)
ato_humano = trilha.filter(
    criado_em__gte=OuterRef("retratada_em"),
    usuario__isnull=False,
    acao__in=(ACAO, ACAO_DESFAZER, acao_geo, "flip_tipo_batida", "veredito_flip"),
)

estoque = (
    Batida.objects.filter(
        retratada_em__gte=agora - timedelta(days=90),
        retratada_em__lte=agora,
    )
    .alias(ultima_retratacao=Subquery(ultima_retratacao))
    .filter(ultima_retratacao=acao_par)
    .annotate(tem_decisao=Exists(ato_humano))
    .order_by()
    .values_list("colaborador__empresa_id", "timestamp", "tem_decisao")
)

grupos = defaultdict(lambda: [0, 0])
for empresa_pk, timestamp, tem_decisao in estoque.iterator(chunk_size=2000):
    _, fim = janela_atual(data_local(timestamp))
    contagens = grupos[empresa_pk, fim.year * 100 + fim.month]
    contagens[0] += 1
    contagens[1] += int(not tem_decisao)

for (empresa_pk, competencia), (total, sem_decisao) in sorted(grupos.items()):
    print(empresa_pk, competencia, total, sem_decisao)
```

### 6. Estoque já retratado

**O admin não consegue desfazer pela tela a retratação do cron.** O calendário mostra o botão quando há permissão e prazo. A rota POST chama `retratar_humano.desfazer`, que exige que a **última trilha da batida** tenha ação `retratar_batida_admin` e que a marca esteja dentro dos sete dias. O teste injeta `retratar_par_relampago` e verifica a recusa e a preservação do carimbo. O Django Admin de `Batida` é somente leitura. `app/colaboradores/services/calendario.py:268–284` · `app/templates/colaboradores/partials/_calendario_grade.html:127` · `app/ponto/urls.py:62–63` · `app/ponto/views.py:3465–3473` · `app/ponto/services/retratar_humano.py:30–53,190–209` · `app/ponto/tests/test_porta_retratar_batida.py:157–169` · `app/ponto/admin.py:9–15`

### 7. Arquivos em comum

| Arquivo | Cruzamento com a obra |
|---|---|
| `app/ponto/services/cartorio.py` | Comentário da sequência detector → cartório a atualizar. Regressão em grade, ata e veredito com a marca preservada. `app/ponto/services/cartorio.py:1184–1207,1498–1504,1899–1906` |
| `app/chamados/models.py` | Porta do chamado do ramo atual e mapa de motivos que documenta o par. A eventual retirada desse emissor pede revisão da referência e preservação do tratamento do acervo de chamados. `app/chamados/models.py:899–909,1037,4127–4133` · `app/chamados/reconciliador.py:183–224` |
| `app/ponto/supra_juiz.py` | Regressão indireta. `class

[truncado em 20000 de 22279 caracteres]

## 25/11 05:00 · sessao cfc9ed5ea57c · A-TELA-DO-DIA-LE-UM-JUIZ · construtor · origem=Code

A v7 dá a `CelulaDia.impressao` um único significado: **hash dos insumos do julgamento, com o sinal bruto de sem par que chega ao classificador**. Na primeira lavra entregue, K6 retira o protesto antes de compor a ata e o veredito. Com os mesmos fatos, o D5 conta `status_completaveis=1` e a passada normal conta `pulados=1`, sem chamar o classificador. [escala/models.py:1752](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/escala/models.py:1752) · [cartorio.py:1535](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:1535) · [cartorio.py:1621](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:1621) · [test v7:44](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:44)

**Escrita e leitura**

- A função de produção alterada foi `_julgar_colab_corpo`. Incorporei a chamada de `_impressao_normal` no ramo normal. `_sem_par_da_impressao` codifica o mesmo par de sinais que vai ao classificador. O valor calculado antes do julgamento serve à comparação normal, ao D5 e ao `carimbar` da lavra. Essa ordem permite verificar o carimbo antes de classificar. [cartorio.py:1239](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:1239) · [cartorio.py:1535](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:1535) · [cartorio.py:1546](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:1546) · [cartorio.py:1619](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:1619) · [cartorio.py:2056](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:2056)
- `nunca_bateu` entra quando a busca pela primeira batida apurável da vida volta vazia. Nesse ramo o sinal bruto está ausente, e `imp0` serve tanto à comparação normal e ao D5 quanto à lavra. `_completar_status` recebe a grade quando a comparação indica carimbo em dia. [cartorio.py:1326](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:1326) · [cartorio.py:1380](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:1380) · [cartorio.py:1449](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:1449) · [cartorio.py:973](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:973)
- A porta `carimbar` compara e persiste o valor. Os leitores de diagnóstico e de fotografia usam o campo gravado em seus instantâneos. [celula.py:1227](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/portas/celula.py:1227) · [celula.py:1231](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/portas/celula.py:1231) · [celula_veredito_velho.py:110](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/management/commands/celula_veredito_velho.py:110) · [feriado_do_posto.py:428](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/colaboradores/services/feriado_do_posto.py:428) · [foto_e4b.py:247](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/chamados/services/foto_e4b.py:247)

**RED e mutantes**

- Na v6, o primeiro RED cai em `status_completaveis == 1`: obteve `0`, com `status_fila_cartorio=1`. O segundo cai em `pulados == 1`: obteve `0`, com `pulados_foto_entregue=1`. Ambos passaram na v7. [test v7:56](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:56) · [test v7:66](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:66)
- Os três mutantes da v7 falharam nessas asserções. Os dois mutantes adaptados da v6 falharam na asserção do sinal bruto e na igualdade da foto, respectivamente. [test v7:13](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:13) · [test v6:26](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:26) · [test v6:165](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:165) · [test v6:184](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:184)

```bash
export PYTHONDONTWRITEBYTECODE=1
v7=ponto.tests.test_a_tela_v7_impressao_um_significado
v6=ponto.tests.test_a_tela_v6_sem_par_memoria.SemParUmaVez

A_TELA_V7_BASE=1 python3 -B -m unittest "$v7" -v
python3 -B -m unittest "$v7" -v
A_TELA_V7_MUTANTE=lavra_sem_sinal python3 -B -m unittest "$v7.ImpressaoUmSignificadoTest.test_d5_sem_status_com_carimbo_em_dia" -v
A_TELA_V7_MUTANTE=d5_sem_sinal python3 -B -m unittest "$v7.ImpressaoUmSignificadoTest.test_d5_sem_status_com_carimbo_em_dia" -v
A_TELA_V7_MUTANTE=pulo_normal_sem_sinal python3 -B -m unittest "$v7.ImpressaoUmSignificadoTest.test_segunda_passada_pula_sem_classificar" -v
A_TELA_V6_MUTANTE=omite_sinal_bruto python3 -B -m unittest "$v6.test_primeira_lavra_imprime_o_sinal_bruto" -v
A_TELA_V6_MUTANTE=perde_foto python3 -B -m unittest "$v6.test_foto_anterior_continua_com_contador_proprio" -v
```

**Seis selos e cobertura preservada**

| Selo | Por que passa |
|---|---|
| O219 | O AST contém duas chamadas de `impressao_insumos`, ambas com `regra=_regra_do_dia(...)`. [test_o219:224](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_o219_versao_da_regra.py:224) |
| FND | O censo encontra quatro chamadas de produção, todas com `**_cadastro(...)`. [test_fnd:137](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_fnd_pergunta_a_juiza.py:137) |
| F1C4-3 | Os dois ramos passam `com_insumo_dia_sem_trabalho(...)` como primeiro argumento. Os dois comandos usam `grade_da_celula`. [test_f1c4_3:215](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:215) |
| O261 | Todas as chamadas de produção passam `cobrancas_vivas=`. [test_o261:61](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_tripwire_o261_gate_le_o_fio.py:61) |
| E3 | As duas chamadas de hash passam `isento=_isento`. [test_e3:122](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_e3_isento_na_impressao.py:122) |
| E3 `nunca_bateu` | O pulo e o D5 saem antes de classificar ou carregar. A passagem forçada classifica uma vez. Completei os dublês de importação do teste em memória e as asserções originais selecionadas passaram. [test_e3_nb:153](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_e3_nb_impressao_memoria.py:153) · [test_e3_nb:250](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_e3_nb_impressao_memoria.py:250) | [nao verificado]

Os testes B4 da v5 cobrem foto intacta e contador próprio. No caso da primeira lavra entregue, acrescentei a passagem forçada para manter a asserção `pulados_foto_entregue=1` quando a guarda atua, e a segunda passagem comum agora exige `pulados=1`. Os controles em memória da v6 passaram e cobrem primeira lavra sem acusação, competência aberta, foto anterior, sinal inerte e preservação dos demais protestos. [test v5:183](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_a_tela_v3_sem_par.py:183) · [test v5:233](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_a_tela_v3_sem_par.py:233) · [test v6:156](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:156) [nao verificado]

**Limite verificado em memória.** Uma foto gravada pela v6 com hash filtrado ainda diverge na leitura pela v7. Nessa sonda, o D5 contou `status_fila_cartorio=1` e a passada normal preservou a foto com `pulados_foto_entregue=1`. A existência de fotos assim em produção e a certificação pelo Code e pelo Opus estão [nao verificado]. [test v7:18](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/tests/test_a_tela_v7_impressao_um_significado.py:18) · [cartorio.py:1546](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:1546) · [cartorio.py:1621](/home/ronald/wt-codex-a-tela-do-dia-le-um-juiz-045353/app/ponto/services/cartorio.py:1621)

## 25/11 04:57 · sessao 1321c4345948 · WRAPPER-N-VAGAS · verificador · origem=Code

BLOQUEIA

D1 ainda tem modos de troca do registro que escapam da recusa controlada, e a barra final continua fazendo o provisionador consultar a gêmea errada. D3 corrige os dois caminhos, mas o selo só detecta regressão no CENSO. `bin/codex_admissao.py:142-190,260-263` · `bin/codex_pergunta.sh:493,499` · `bin/tests/test_codex_pergunta.sh:276-300,680-685`

## 1–2. D1 — releitura sob as travas

**NOVO — cura parcial da classe.** Qualquer diferença de bytes detectada sob os flocks impede o `os.replace`, inclusive se a releitura devolver `None`. O selo `prov_relida` injeta um registro v2 antes do primeiro flock e cobra rc 2, conteúdo novo ainda no nome e histórico vazio. O mutante que desliga a comparação alcança o arquivamento. O selo cobre a troca de conteúdo regular e deixa as falhas de abertura sem prova. `bin/codex_admissao.py:160-190` · `bin/tests/test_codex_n_vagas.sh:768-788,845`

| Estado no momento da releitura, com as travas tomadas | Retorno, mensagem e estado |
|---|---|
| **NOVO — o helper devolve `None`** | **rc 2**, `ERRO <registro> mudou entre a leitura e as travas da vaga <k>: nao movido`. O JSON continua ausente, o histórico daquele registro fica intacto e os descritores da vaga e da gêmea, quando aberta, são fechados. `bin/codex_admissao.py:142-148,166-190` |
| **NOVO — o nome virou symlink antes do `open`** | **rc 1 no CLI**, com traceback `OSError: [Errno 40] Too many levels of symbolic links`. O link permanece no nome e o alvo e o histórico ficam intactos. A exceção pula `os.close` das leases. O kernel as solta na saída do CLI. Se um chamador Python capturar a exceção e continuar vivo, os descritores ficam abertos. `bin/codex_admissao.py:142-148,166-190,260-271` |
| **NOVO na releitura — o nome some entre `lexists` e `open`** | **rc 1 no CLI**, com traceback `FileNotFoundError`. O JSON fica ausente, sem arquivamento, e vale a mesma distinção entre saída do processo e exceção capturada. Se `None` vier já na **primeira** leitura, `b.strip()` levanta `AttributeError` antes de abrir as leases. `bin/codex_admissao.py:142-148,155-170,187-190,260-271` |

**Desenho falho.** **NOVO**, a releitura pode bloquear com os flocks presos se o nome virar FIFO: a abertura carece de `O_NONBLOCK` e de validação do tipo pelo descritor. **PREEXISTENTE e ainda aberto no D1**, passar `…/logs/vagas/` faz `dirname(vdir)` procurar `…/logs/vagas/construtor.vaga`. Com `vaga.1` livre e a gêmea real presa, o provisionador pode arquivar o registro sem tomar a trava da gêmea. O delta também deixa `os.replace` operar pelo nome depois da comparação. Para escritores que respeitam a lease, a releitura fecha a janela original. Uma troca posterior por quem ignora a trava ainda pode mover o nome trocado. A ocorrência desses estados em produção é [não verificado]. `bin/codex_admissao.py:142-148,151-183,260-263` · `bin/tests/test_codex_n_vagas.sh:742-788`

## 3. D2 — decisão sobre a gêmea legada

**NOVO — cura a classe no retrato da admissão.** O titular emite `legado=1` quando conseguiu tomar `vaga.1` e encontrou a gêmea presa. O wrapper exclui k=1 de `_OUTRAS` nesse estado e recusa pela guarda própria antes de executar `_COMUM_PY`. O selo cobre gêmea vazia e gêmea com JSON copiado de uma obra disjunta. `bin/codex_admissao.py:207-233` · `bin/codex_pergunta.sh:529-559` · `bin/tests/test_codex_n_vagas.sh:790-805,842,874`

| Estado na admissão | Resultado |
|---|---|
| **Só a gêmea presa**, com ou sem `vaga.1.json` residual de outro dono | Com capacidade disponível, **2 `PORTAO`** nos dois estados. Se os predicados anteriores passarem, sai o `COMUM-NAO-SEI` próprio da gêmea. Uma falha anterior de carga, janela, desligador ou saúde dá **2 `PORTAO`** com seu próprio motivo. Capacidade esgotada dá **75 `SEM_VAGA`** antes do portão. `bin/codex_admissao.py:209-233` · `bin/codex_pergunta.sh:524-559` |
| **k=1 livre, gêmea livre, k=2 viva** | O titular escolhe k=1 e emite `legado=0`. Com N suficiente, o portão consulta a projeção da k=2 e admite se todos os predicados passarem e `_COMUM_PY` devolver `COMUM-NAO`. Caso contrário, **2 `PORTAO`**. Com **N=1**, dá **75 `SEM_VAGA`** pelo N+1. `bin/codex_admissao.py:207-233` · `bin/codex_pergunta.sh:535-559` · `bin/tests/test_codex_n_vagas.sh:251-257,362-367` |
| **N=1, gêmea legada presa** | **PREEXISTENTE — 75 `SEM_VAGA`**. O titular fica sem k elegível e o wrapper sai antes de chamar `_portao`. `bin/codex_admissao.py:209-232` · `bin/codex_pergunta.sh:524-527` · `bin/tests/test_codex_n_vagas.sh:530-535` |

A guarda roda **depois** do CENSO, que já pode gravar trilha, da criação de diretórios e da escolha da lease. Ela roda **antes** do JSON da vaga e de `abriu`. Na recusa, o trap fecha o pipe do titular, que libera a lease ao sair. **PREEXISTENTE, vizinho ainda aberto**: se k=1 troca de dono entre M4 e M6, `CONFERE` verifica a lease própria e a contagem corrente. A decisão do portão sobre o dono anterior segue valendo. `bin/disjuntor.py:2316-2319,2335-2348` · `bin/codex_pergunta.sh:425-428,486-493,508-526,555-575,771-793` · `bin/codex_admissao.py:95-120,234-239`

## 4. D3 — leitores e significado de `PERGUNTA_R`

**NOVO — cura a classe de releitura do alias original.** O único escritor da variável é `realpath -e "$PERGUNTA"` na linha 90. O argumento original ainda é usado no pré-teste `-r`. O valor resolvido tem o mesmo significado em todos os consumidores: caminho absoluto da pergunta. `bin/codex_pergunta.sh:86-90`

- **CENSO e `_ARQS_PY` — NOVO.** Ambos recebem esse caminho e os anexos resolvidos em `_ANX`. O CENSO lê com abertura de arquivo regular, sem seguir symlink final. `_ARQS_PY` usa `open` comum para pergunta e anexos. `bin/codex_pergunta.sh:124-127,223-232,493,499` · `bin/disjuntor.py:191-194,2320-2330` · `bin/orcamento_contexto.py:68-81`
- **Registro de insumos, log e prompt — PREEXISTENTE.** M4 recebe `PERGUNTA_R` e `_ANX` e grava caminho e SHA. M6 relê os caminhos do JSON. O log abre `PERGUNTA_R` para raspar a pergunta, e o prompt abre `PERGUNTA_R` e recebe o conteúdo raspado dos anexos lidos de `_ANX`. Dos argumentos originais dos anexos sai o rótulo pelo `basename`. O inventário da worktree recebe somente `WT` e `HASH` nos três pontos. `bin/codex_pergunta.sh:145-147,368-382,563,571,611-628,642-654,677-679,780,822`

**Desenho falho PREEXISTENTE.** Caminho resolvido e bytes congelados são garantias distintas. CENSO, `_ARQS_PY` e M4 fazem leituras independentes, e o prompt é montado antes da conferência M6. Uma edição no próprio caminho resolvido pode fazer o CENSO julgar bytes diferentes dos registrados em M4. M6 só compara com o SHA de M4. **Lacuna no selo NOVO**: o mutante de `insumo_link` reverte apenas o argumento de CENSO e o fake só grava argv. No mesmo selo, importar o fake sai antes de oferecer `_briefs_do_censo`, `_ARQS_PY` pode ficar sem projeção e N=1 dispensa o portão no caso positivo. Reverter apenas `_ARQS_PY` a `$PERGUNTA` fica sem testemunha. `bin/codex_pergunta.sh:227-234,368-382,493-500,559,571,677-681,787-792` · `bin/tests/test_codex_pergunta.sh:79-107,276-300,680-685`

## 5. Selos e achados menores

- **PREEXISTENTE — `#V:comum` conserva mutante próprio.** A linha `comum|_recusa_portao|true|comum|comum:` testa colisão de arquivo real do cenário, com um vizinho disjunto que deve entrar. O `estado_alheio` passou para `#V:gemea_nao_sei`. `bin/tests/test_codex_n_vagas.sh:315-324,354-359,873-874`
- **NOVO — o mutante de CENSO morre por argv.** A asserção nova detecta o caminho errado mesmo com rc 0 e Codex falso chamado. Os mutantes de D1 e D2 removem decisões. O executor de mutantes `#V` aceita qualquer falha com o rótulo esperado, inclusive a de texto, mas a neutralização da guarda D2 também permite prosseguir para a comparação sem a gêmea. A primeira asserção que matou cada mutante na execução informada é [não verificado]. `bin/tests/test_codex_pergunta.sh:291-300,685,703-705` · `bin/tests/test_codex_n_vagas.sh:197-201,842-845,874,910-915` · `bin/codex_pergunta.sh:535-557`
- **Achados menores NOVOS de vacuidade parcial.** `prov_relida` exige rc e conteúdo, mas `ls ... 2>/dev/null` aceita histórico ilegível como vazio. O subcaso `gemea_so_json` pode passar pela recusa esperada mesmo se o `cp` falhar e a gêmea continuar vazia. `insumo_link` exige troca e captura, mas o novo teste de argv pode reaproveitar `censo_argv` de rodada anterior, pois o caso não o limpa. O selo já tem um mutante separado que cobra a execução do CENSO. `bin/tests/test_codex_n_vagas.sh:784-787,795-805` · `bin/tests/test_codex_pergunta.sh:83-85,238-248,276-300,670-671`
- **M1 — NOVO, fecha a documentação pedida.** O cabeçalho agora descreve N pelo blob de `main`, lease, gêmea e prazo da meta. O caso `vagas_n_ambiente` já sela a precedência do blob. Vizinho **PREEXISTENTE**: o cabeçalho diz que nenhum caso lê relógio real, mas o shim só fixa `+%H%M` e delega os demais formatos a `/usr/bin/date`. `bin/tests/test_codex_n_vagas.sh:3-14,114-119,266-275`

## 6. Checklist do delta

| Item | Resultado |
|---|---|
| **Except e fallback** | **0** `except` explícitos novos e **0** `\|\| true` de execução novos. Há um retorno novo `None` quando `lexists` dá falso e um ramo novo de recusa explícita que reaproveita `livre=None`. São **0** novos `2>/dev/null` líquidos na produção e **4** nos selos: `grep`, `ls` e dois `sed`. O redirecionamento de `_ARQS_PY` é **PREEXISTENTE**. `bin/codex_admissao.py:142-148,170-190` · `bin/codex_pergunta.sh:499-500` · `bin/tests/test_codex_n_vagas.sh:785-787` · `bin/tests/test_codex_pergunta.sh:299-300` |
| **Segundo escritor** | **PREEXISTENTE.** `_PROJ_PY` publica, inicia e fecha com a lease. `--provisiona` é o segundo caminho que muda o nome do JSON, fora de `meta.lock`, sob o flock de k e o da gêmea se encontrada. O delta altera a validação desse caminho. A unicidade operacional do pouso é [não verificado]. `bin/codex_pergunta.sh:328-387,414-427,502-503,571,787` · `bin/codex_admissao.py:123-139,166-190` |
| **Relógio e ordem de trava** | **PREEXISTENTE.** A hora que decide o portão é lida sob meta e lease. M4 toma meta → vaga e libera meta antes do gate. M6 retoma meta com lease, slot e resposta, depois de liberar o gate. O provisionador usa vaga → gêmea com tentativas não bloqueantes e sem gate. O delta acrescenta I/O sob a lease no provisionador. `bin/codex_pergunta.sh:510-521,549-559,571-575,693-726,736-750,768-793` · `bin/codex_admissao.py:72-77,166-170` |
| **Campo com dois significados** | **NOVO — `_OUTRAS` deixa de misturar** projeção e arquivo de lock. `legado=1` mantém o sentido de k=1 presa só pela gêmea no retrato M4. `PERGUNTA_R` mantém o sentido de caminho. O triestado local de `livre` é **PREEXISTENTE**, com um novo ramo usando `None` para recusar. `bin/codex_admissao.py:170-190,207-233` · `bin/codex_pergunta.sh:86-90,529-557` |

A comparação usa o patch anexado e as linhas da cópia. As exceções de D1 foram conferidas em memória com I/O simulado. Os verdes informados e o hash Git da cópia são [não verificado].

## 25/11 02:57 · sessao fcf764eba105 · PONTA-PEQUENA-NA-BORDA · verificador · origem=Code

**Parecer v5b: o delta corrige o achado 5b.** A asserção nova isola a guarda da decisão vigente no cenário do teste. [app/ponto/tests/test_ponta_pequena_prazo_evento.py:250](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/tests/test_ponta_pequena_prazo_evento.py:250) · [app/ponto/services/vigia_de_hora.py:113](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/services/vigia_de_hora.py:113)

1. **O que segura o vigia.** Às 19h10, a `DecisaoHE` vigente do mesmo colaborador e dia faz `motivo_de_nao_decidir()` devolver `MOTIVO_JA_DECIDIDO`. O vigia pula a célula antes de consultar o último evento. Retirada só essa guarda, a célula continua candidata: é de hoje, o colaborador está ativo, o limite é 15, há veredito, a competência está aberta e o turno terminou às 18h. O atalho das batidas completas vem depois de incluir a célula na lista do prazo. Com os eventos do par apagados, o vigia chega ao `INSERT` e soma 1. A falha `1 != 0` na linha 261 é o resultado esperado pela leitura do código. A execução relatada com `rc 1` fica **[nao verificado]**. [teste:63](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/tests/test_ponta_pequena_prazo_evento.py:63) · [teste:124](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/tests/test_ponta_pequena_prazo_evento.py:124) · [ponta_pequena.py:49](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/services/ponta_pequena.py:49) · [vigia_de_hora.py:107](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/services/vigia_de_hora.py:107) · [vigia_de_hora.py:172](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/services/vigia_de_hora.py:172)

   Há uma saída antecipada que a asserção aceitaria em caso de contenção: `rodar()` devolve `{'em_curso': 1}` se a trava estiver ocupada, e `.get('prazo_eventos', 0)` lê isso como zero. O teste faz chamadas sequenciais e o vigia libera a trava ao fim de cada passada. [vigia_de_hora.py:145](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/services/vigia_de_hora.py:145) · [vigia_de_hora.py:186](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/services/vigia_de_hora.py:186) · [teste:250](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/tests/test_ponta_pequena_prazo_evento.py:250)

2. **Exclusão e isolamento.** Não encontrei violação. `EventoCelula` guarda os IDs da célula e do colaborador como inteiros, sem FK, e a decisão tem vínculo próprio com o colaborador. `_eventos()` limita a exclusão ao par do teste. `setUpTestData` cria empresa, colaborador e escala; cada método que precisa dos eventos cria seu próprio cenário, e o rollback de `TestCase` isola as contagens. A ressalva é o comentário “limpeza dos baixados”: o laço apaga todos os eventos do par sem conferir `processado_em`. Isso serve para isolar a guarda, enquanto a retenção de produção filtra pela data de processamento. [escala/models.py:1868](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/escala/models.py:1868) · [ponto/models.py:1300](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/models.py:1300) · [teste:58](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/tests/test_ponta_pequena_prazo_evento.py:58) · [teste:112](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/tests/test_ponta_pequena_prazo_evento.py:112) · [teste:258](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/tests/test_ponta_pequena_prazo_evento.py:258) · [rollback:5](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/core/tests/test_haiku_contador_ordem.py:5) · [reconciliador.py:1556](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/chamados/reconciliador.py:1556)

3. **Escopo do delta.** O anexo acrescenta quatro linhas a um teste existente, sem alteração de produção. Não há `except` novo, relógio solto, `timezone.now()` no acréscimo ou selo sem mordida nesse cenário. A chamada das 19h10 usa `_vigia()`, que congela o relógio e passa `agora` explicitamente. O arquivo confere com os hashes de blob do anexo; a identidade do commit-base `aac786def` fica **[nao verificado]**. [teste:91](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/tests/test_ponta_pequena_prazo_evento.py:91) · [teste:258](/home/ronald/codex_ws/5dfaa69b15b8db999910a4a8249722e9e859c11d/app/ponto/tests/test_ponta_pequena_prazo_evento.py:258)

## 25/11 02:46 · sessao cf26dada28c3 · A-TELA-DO-DIA-LE-UM-JUIZ · verificador · origem=Code

**Parecer da v6.** O cartório voltou a chamar `classificar_dia` uma vez por ramo, por passada do corpo. Os seis selos de AST passaram isoladamente. O achado que ficou é na impressão da primeira lavra entregue: grava-se o hash com o sinal filtrado, mas a conferência do status usa o sinal bruto. Reproduzi em memória uma célula sem status que cai na fila do cartório e volta a ser pulada pela guarda da foto. [cartorio.py:1394](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1394) · [cartorio.py:1543](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1543) · [cartorio.py:1627](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1627)

### 1. Quem escreve e quem lê as posições

- O cartório cria uma lista vazia em cada chamada. `classificar_dia` é o único escritor da posição. Faz `append(len(out))` imediatamente antes de acrescentar **uma** tupla, `PREVISTO_COM_DIA_INDEFINIDO` ou `FURO_PARCIAL`. `_retirar_sem_par_entregue` é o único consumidor e apaga `classificacao[posicoes_sem_par[0]]` se K6 confirmar a entrega. [cartorio.py:1395](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1395) · [cartorio.py:1613](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1613) · [supra_juiz.py:551](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/supra_juiz.py:551) · [cartorio.py:1264](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1264)
- Qualquer quantidade de itens acrescentados **ao fim por outro ramo** mantém o índice válido. Na sonda, `REALIZADO_ZERO_COM_TURNO` veio antes, o protesto marcado ficou na posição `1`, e `BATIDA_ORFA_FORA_TOLERANCIA` veio depois. A retirada preservou os dois vizinhos. Se o **próprio ramo de sem par** passar a emitir dois protestos, o consumidor atual só retira o primeiro, mesmo que o juiz registre as duas posições. Inserir, remover ou reordenar itens antes do índice depois da marcação também quebraria o contrato. [supra_juiz.py:359](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/supra_juiz.py:359) · [supra_juiz.py:554](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/supra_juiz.py:554) · [supra_juiz.py:618](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/supra_juiz.py:618) · [cartorio.py:1269](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1269)
- No ramo normal, a lista que sobra alimenta `cods`, `furo_do_sem_par`, a ata, a emissão e a porta da lavra. A porta escreve `CelulaDia.protestos`, lido, entre outros, pela folha e pelo comando `supra_juiz`. No `nunca_bateu`, a lista filtrada alimenta `derivar_veredito`, e a lavra recebe `codigos=[]`. [cartorio.py:1406](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1406) · [cartorio.py:1423](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1423) · [cartorio.py:1635](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1635) · [cartorio.py:2004](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:2004) · [celula.py:1126](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/portas/celula.py:1126) · [export.py:1285](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/folha/export.py:1285)

### 2. Classificação e retratação

Para os mesmos insumos, a lista retornada pelo supra-juiz é idêntica com ou sem `posicoes_sem_par`. A única diferença é o índice escrito na lista auxiliar. Os dois ramos do cartório passam a lista. Os comandos `supra_juiz` e `emitir_furo_retroativo` consultam a ata lavrada e, quando chamam o classificador, passam os dois sinais como `False`. A diferença entre o sinal atual e a acusação lavrada já existe nessa costura. O argumento novo só permite aplicar K6 sobre o protesto identificado pelo juiz. [supra_juiz.py:278](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/supra_juiz.py:278) · [supra_juiz.py:551](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/supra_juiz.py:551) · [comando supra_juiz.py:205](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/management/commands/supra_juiz.py:205) · [emitir_furo_retroativo.py:98](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/management/commands/emitir_furo_retroativo.py:98)

O cron `--executar` decide pela lâmpada lavrada `cobranca_indevida` e pelo código em `cel.protestos`. A classificação do próprio comando alimenta o diagnóstico e o alarme. Pelo código, o parâmetro novo não altera a decisão de retratar. Dias e retratações efetivamente observados em produção **[nao verificado]**. [crons.py:599](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/config/crons.py:599) · [comando supra_juiz.py:236](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/management/commands/supra_juiz.py:236) · [comando supra_juiz.py:284](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/management/commands/supra_juiz.py:284)

### 3. Os seis selos

Executei as asserções de AST extraídas dos testes contra os arquivos da cópia. O classificador tem dois pontos de chamada no cartório, um por ramo. A impressão normal tem dois pontos de chamada ao helper local, com caminhos mutuamente exclusivos. O `ast.walk` enxerga o `impressao_insumos` dentro do helper. [cartorio.py:1398](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1398) · [cartorio.py:1465](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1465) · [cartorio.py:1545](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1545) · [cartorio.py:1614](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1614) · [cartorio.py:1633](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1633)

| Selo | O que exige e por que passa |
|---|---|
| O219, quantidade | Exatamente dois nós `impressao_insumos` dentro do corpo, ambos com `regra=` literal. Conta `imp0` e a chamada dentro de `_impressao_normal`. [test_o219_versao_da_regra.py:224](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_o219_versao_da_regra.py:224) |
| O219, fonte | O valor de cada `regra=` tem de ser uma chamada a `_regra_do_dia`. Os dois nós passam essa chamada diretamente. [test_o219_versao_da_regra.py:232](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_o219_versao_da_regra.py:232) |
| FND | Quatro chamadas de produção, com `**_cadastro(...)` visível em cada uma. São duas no cartório e uma em cada comando. [test_fnd_pergunta_a_juiza.py:137](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_fnd_pergunta_a_juiza.py:137) |
| F1C4-3 | Exige `com_insumo_dia_sem_trabalho(...)` literalmente no primeiro argumento das chamadas do cartório. Nos comandos, verifica a presença de `grade_da_celula` no arquivo. A v6 põe a chamada diretamente no argumento. O `nunca_bateu` ainda chama o mesmo produtor antes, para a guarda. [test_f1c4_3_supra_pergunta_ao_juiz.py:218](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:218) · [cartorio.py:1373](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1373) |
| O261 | Exige `cobrancas_vivas=` literal em cada chamada encontrada. O cartório passa `0` no `nunca_bateu` e `len(_com_fio)` no normal. [test_tripwire_o261_gate_le_o_fio.py:27](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_tripwire_o261_gate_le_o_fio.py:27) · [cartorio.py:1398](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1398) · [cartorio.py:1614](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1614) |
| E3 | Exige dois nós `impressao_insumos`, ambos com `isento=` apontando para o mesmo nome. `imp0` e o helper passam `_isento` literalmente. [test_e3_isento_na_impressao.py:118](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_e3_isento_na_impressao.py:118) |

### 4. Idempotência

Na sonda sequencial, com os mesmos fatos, executei o corpo e as funções reais da porta extraídos da árvore, com persistência e trilha dubladas. Em ambos os casos houve **um save, uma chamada de trilha `veredito_celula` e estado igual nas repetições**. Na aberta, as três passadas deram `pulados = 0, 1, 1`. Na entregue sem foto inicial, deram `pulados_foto_entregue = 0, 1, 1`. O contador vale por rodada. A segunda passada entregue reclassifica porque compara a impressão bruta com a filtrada e então preserva a foto. Trilha persistida e duplicatas de chamados no banco **[nao verificado]**. [cartorio.py:1543](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1543) · [cartorio.py:1627](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1627) · [celula.py:1112](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/portas/celula.py:1112) · [processar_cartorio.py:38](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/management/commands/processar_cartorio.py:38)

**Falha no D5.** Com status ausente e os mesmos fatos, a sonda reproduziu `status_fila_cartorio=1` na entregue. O D5 calcula o hash bruto, considera o carimbo velho e manda à fila. O julgamento normal volta a preservar a foto pelo mesmo sinal. O teste existente para completar status na entregue usa uma foto já lavrada com acusação e espera `ja_lavrado=1`, portanto deixa esse caso descoberto. O fluxo bruto/filtrado já aparece no lado removido do delta da v5. [cartorio.py:973](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:973) · [cartorio.py:1546](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1546) · [cartorio.py:1627](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1627) · [test_a_tela_v3_sem_par.py:262](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_a_tela_v3_sem_par.py:262)

### 5. Teste novo e mutantes [nao verificado]

Os **7 testes novos passaram no modo padrão**. Eles leem a fonte da cópia, selecionam nós por AST e executam os corpos em namespaces montados pelo teste. Exercitam as instruções reais desses corpos, com dependências diferentes do import de produção. Pareador, grade, K6, lavra, carimbo e emissão são dublados ou desativados. `git show` só roda com `A_TELA_V6_BASE=1`. O RED contra o commit v5 e a suíte Django integral **[nao verificado]**. [test_a_tela_v6_sem_par_memoria.py:41](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:41) · [test_e3_nb_impressao_memoria.py:56](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_e3_nb_impressao_memoria.py:56) · [test_a_tela_v6_sem_par_memoria.py:84](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:84)

**Os seis mutantes novos mordem.** Cada controle passou e cada mutante falhou por asserção no teste indicado. Há dois limites: o caso `nunca_bateu` força um sinal positivo sem batida apurável, combinação produzida pelos dublês, e o mutante legado `classificacao_antes_guarda` do E3 quebra com `ValueError: substring not found` antes de alcançar qualquer asserção. [test_a_tela_v6_sem_par_memoria.py:19](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:19) · [test_a_tela_v6_sem_par_memoria.py:88](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:88) · [test_a_tela_v6_sem_par_memoria.py:195](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_a_tela_v6_sem_par_memoria.py:195) · [turnos.py:1867](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/turnos.py:1867) · [test_e3_nb_impressao_memoria.py:74](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/tests/test_e3_nb_impressao_memoria.py:74)

### 6. Classes de reprovação

No delta de produção, encontrei **zero segundo juiz, segundo escritor, relógio novo fora da borda, derivação paralela do sem par, mudança de ordem de trava, `except` novo ou fallback de decisão**. A posição vem do próprio classificador, a entrega vem de K6, o relógio segue por `agora` e a gravação segue pela porta. O `None` local de `imp` apenas adia a chamada ao mesmo gerador de hash. O problema semântico remanescente está em `CelulaDia.impressao`, usada como hash do sinal efetivo na escrita e conferida como hash do sinal bruto na leitura. Os seis mutantes novos cobrem suas respectivas costuras, com a lacuna do D5 descrita acima. [supra_juiz.py:551](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/supra_juiz.py:551) · [cartorio.py:1222](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1222) · [cartorio.py:1245](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1245) · [cartorio.py:1543](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:1543) · [cartorio.py:2041](/home/ronald/codex_ws/004a0b7f858584f8cbb25079d57810137bbcfe57/app/ponto/services/cartorio.py:2041)

## 25/11 02:43 · sessao c999c0505dcf · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · verificador · origem=Code

**PAREI na v10.** O ramo recupera o trabalhado na saída de D+1, mas a HE comum passa a usar esse tempo na apuração. Ao mesmo tempo, a grade ainda pode marcar a saída como bloqueada. São duas divergências com a regra do delta, que manda contar trabalho e noturno e classificar a HE no recortado/Gestão. `app/ponto/janela_he.py:182`, `app/ponto/motor_calculo_v2.py:3350`, `app/ponto/motor_calculo_v2.py:3429`, `app/ponto/janela_he.py:251`, `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:320`

### 1. Quem escreve e quem lê o dia

- O juiz entrega `Turno.data_turno`. `_dia_do_turno` indexa essa resposta pela entrada crua localizada e `calcular_mes` guarda o mapa. `_dia_do_turno_do_par` devolve `(dia de trabalho da janela, dia do juiz ou None)`. `_janela_do_dia` escreve `j['dia']` com o primeiro valor e passa o segundo, `_dia_t`, a `marco_de_saida`. A entrada lê `j['dia']` para ancorar `hi` e para o relato acima do teto. A saída agora o lê para construir a virada e já o lia para o relato acima do teto. `app/ponto/turnos.py:318`, `app/ponto/motor_calculo_v2.py:884`, `app/ponto/motor_calculo_v2.py:2413`, `app/ponto/motor_calculo_v2.py:949`, `app/ponto/motor_calculo_v2.py:1567`, `app/ponto/motor_calculo_v2.py:1661`, `app/ponto/motor_calculo_v2.py:1722`, `app/ponto/motor_calculo_v2.py:1760`
- **Com juiz**, os dois valores são o mesmo D. O marco de saída fica em D para turno que não cruza e em D+1 para turno que cruza. No segundo caso, `marco_saida >= virada`, inclusive quando `hf` é `00:00`, e o ramo fica fora. **Sem juiz**, `j['dia']` vem da data local da entrada. O marco legado fica na data civil da saída ou um dia depois. Se a saída alcançou a virada, esse marco já está na virada ou depois dela. Se a saída ficou antes, falha a outra metade da guarda. O fallback chega ao argumento novo, mas não dispara o ramo. `app/ponto/motor_calculo_v2.py:949`, `app/ponto/janela_he.py:54`, `app/ponto/janela_he.py:182`

### 2. Fuso da saída

A saída chega **localizada em `America/Sao_Paulo`, UTC−3 nas datas do anexo**. O adaptador extrai `t.saida.timestamp`. Se a ata substitui o par, o motor recupera o instante da batida correspondente. `calcular_mes` entrega o par a `calcular_periodo`, e as três implementações chamam `_janela_do_par` com `localizar(saida)`. Para um timestamp aware, `localizar` chama `.astimezone(TZ_LOCAL)`, com `TZ_LOCAL` vindo de `settings.TIME_ZONE`. O `.replace` da janela opera sobre esse instante local. A virada nesse caminho é `00:00` local. `app/ponto/adaptador_motor.py:37`, `app/ponto/motor_calculo_v2.py:636`, `app/ponto/motor_calculo_v2.py:2474`, `app/ponto/motor_calculo_v2.py:2775`, `app/ponto/motor_calculo_v2.py:2905`, `app/ponto/motor_calculo_v2.py:3243`, `app/ponto/motor_calculo_v2.py:28`, `app/ponto/motor_calculo_v2.py:73`, `app/config/settings/base.py:93`, `app/ponto/janela_he.py:36`

### 3. Paridade da tela

**Sim, se a saída estiver casada à última célula S e a sobra superar o piso.** Reproduzi em memória com marco `23:30`, saída `00:10`, piso zero e extra declarada zero. O motor devolveu `250 trabalhados / 0 fora`, e o marcador devolveu `40 min depois`, com `fora_da_janela=True`. `montar_realizado_grade` só repassa `delta` e `hora_marco`. O delta vem da diferença entre instantes no builder ou da diferença normalizada entre as horas no leitor da ata. `marcar_pontas_fora` aplica sinal, extra e piso sobre esse delta. Divide auxiliares com a janela do motor, mas toma sua própria decisão sobre o riscado. `app/ponto/janela_he.py:182`, `app/ponto/janela_he.py:244`, `app/escala/utils.py:325`, `app/escala/utils.py:1118`, `app/escala/services/leitor_celula.py:29`, `app/escala/services/leitor_celula.py:121`, `app/docs/LEIS.md:60`

| Leitor | Fonte do aviso ou riscado |
|---|---|
| Espelho web | O montador chama `marcar_pontas_fora` sobre a grade. O template lê a marca para o relógio riscado e a lista para o aviso. `app/ponto/services/espelho.py:741`, `app/templates/ponto/espelho.html:189`, `app/templates/ponto/espelho.html:239` |
| Calendário do colaborador | Chama o mesmo marcador sobre as células da ata e transfere o resultado à batida do turno por hora. O template imprime a marca. `app/colaboradores/services/calendario.py:413`, `app/colaboradores/services/calendario.py:643`, `app/colaboradores/services/calendario.py:655`, `app/templates/colaboradores/partials/_calendario_grade.html:127` |
| PDF do espelho | Lê a lista do montador e imprime “não conta”. Herda o falso positivo da grade. `app/relatorios/pdf_espelho.py:932`, `app/relatorios/pdf_espelho.py:944` |
| Gestão de HE, PDF da Gestão e contador da Central | Leem o retrato lavrado a partir da lista do espelho. A Gestão risca conforme a decisão vigente, com os minutos vindos desse retrato. `app/ponto/services/he_pendente_lavrado.py:132`, `app/ponto/views.py:950`, `app/templates/ponto/gestao_he.html:276`, `app/ponto/views.py:820`, `app/ponto/views.py:857`, `app/ponto/services/gestao_he.py:493`, `app/relatorios/views.py:43` |
| Portão do TXT | Lê a lista do espelho. O falso positivo pode barrar o TXT se o dia estiver acima do limite, sem decisão e com a trava ligada. `app/folha/porta_export.py:190`, `app/folha/porta_export.py:690`, `app/folha/porta_export.py:864` |

Quando a célula está `missing`, o marcador pula a ponta. Nesse caminho, o espelho complementa a lista com o relato do motor, e o ramo novo devolve zero antes de publicar qualquer acusação de saída. O caso de dia sem previsto também vem do motor. A lista Cadastro × Realidade lê a lavra do relato. Se a saída do col327 ficou casada na ata e apareceu riscada em produção, **[nao verificado]**. `app/ponto/janela_he.py:246`, `app/ponto/janela_he.py:338`, `app/ponto/services/espelho.py:754`, `app/ponto/services/espelho.py:760`, `app/ponto/motor_calculo_v2.py:1761`, `app/escala/services/cadastro_realidade.py:129`

### 4. L-084 e entrada

A **L-084 do par** recebe o mesmo `m_saida` e a saída crua. Ela mede as duas distâncias antes do novo ramo e exige ambas acima de 180 min em módulo. A L-084 do dia também roda antes, sobre as pontas cruas. `app/ponto/motor_calculo_v2.py:1567`, `app/ponto/motor_calculo_v2.py:1576`, `app/ponto/motor_calculo_v2.py:1617`, `app/ponto/motor_calculo_v2.py:1962`

**A função de entrada fica assimétrica.** Se receber D e `hi=00:05` de D, uma entrada às `23:55` da véspera continua sujeita ao corte de 10 min com piso zero. O juiz de geometria, para essa batida como abertura do turno, resolve seu dia civil ou a véspera dele. Ele não promove essa entrada ao dia seguinte por proximidade do `hi`. `app/ponto/janela_he.py:125`, `app/ponto/motor_calculo_v2.py:1715`, `app/ponto/turnos.py:285`, `app/ponto/turnos.py:299`, `app/ponto/turnos.py:318`

### 5. Mordida das mutações

Executei as mutações **em memória**, nas funções da cópia, com cadastro e dia do juiz fornecidos pela sonda. A coluna de testes indica quais asserções caem pela leitura do código. Execução integral da suíte Django e RED histórico **[nao verificado]**. `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:355`, `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:392`, `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:404`

| Mutação | Testes que pegam | [nao verificado]
|---|---|
| **(a) Tirar o ramo** | Caem os três REDs de saída D+1, sem feriado, com feriado em D e com feriado só em D+1. A janela volta a `210/40`. Cai também `test_RED_par_inteiro_depois_do_marco_conta_inteiro`, que volta a `0/50` com efetivo invertido. O controle no próprio D passa. `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:338`, `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:423`, `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:435` |
| **(b) Trocar só `marco_saida < _virada` por `True`** | Os cinco casos da classe do delta passam. O controle sai antes da virada. Pegam a mutação os testes anteriores de noturno sem cabeça, entrada atrasada depois da meia-noite e marco em D+1 no par do turno. Eles exigem cortar respectivamente 2, 1 e 26 min na saída de turnos que cruzam. `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:64`, `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:73`, `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:102`, `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:174`, `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:186` |
| **(c) Passar `dia=None` só na chamada a `saida_efetiva`** | Caem os mesmos quatro de **(a)**. O marco permanece ancorado em D, a virada fica desativada e o recorte volta. `app/ponto/motor_calculo_v2.py:1567`, `app/ponto/motor_calculo_v2.py:1758`, `app/ponto/janela_he.py:182`, `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:423` |

### 6. Classes de reprovação e escopo

- **HE comum sem cobertura no RED.** Reproduzi o trecho de apuração comercial em memória para escala `20:00–23:30`, entrada `20:20`, saída `00:10`, sem feriado nem intervalo. O recortado dava `190 min trabalhados / HE50 0`. A v10 dá `230 min trabalhados / HE50 20`, pois o excedente usa o novo trabalhado contra o limite de 210 min. A mesma saída pode ir à Gestão como bloqueada. Os REDs novos conferem a HE100 de feriado, mas a asserção não cobre HE50/100 comum, noturno nem paridade da tela. `app/ponto/motor_calculo_v2.py:3243`, `app/ponto/motor_calculo_v2.py:3350`, `app/ponto/motor_calculo_v2.py:3421`, `app/ponto/motor_calculo_v2.py:3429`, `app/ponto/services/espelho.py:741`, `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:402`
- **Duas respostas para a janela.** A derivação da grade já existia e diverge do motor neste ramo. Há também uma ambiguidade de procedência: o `dia` novo é documentado como resposta do juiz, mas recebe `j['dia']`, que admite a data civil da entrada. É um reaproveitamento do fallback existente e a guarda o torna inerte neste caminho. O delta concentra a nova decisão na função existente, sem novo escritor, `except`, consulta ao relógio corrente ou nova derivação do dia da saída. `app/ponto/janela_he.py:173`, `app/ponto/janela_he.py:182`, `app/ponto/janela_he.py:244`, `app/ponto/motor_calculo_v2.py:949`, `app/ponto/motor_calculo_v2.py:1760`
- **09/10 e cauda.** O ramo vale em qualquer data que passe pela vigência da janela da empresa. O recálculo direto pode dar o resultado novo também em 09/10. A guarda existente protege a escrita do fechamento para as empresas que exportaram essas competências e permite leitura. Quanto ao noturno, o delta muda o `s` entregue ao escritor existente. A gravação efetiva ou o impacto em 09/10 em produção são **[nao verificado]**. `app/ponto/motor_calculo_v2.py:1406`, `app/ponto/janela_he.py:182`, `app/ponto/services/fechamento.py:31`, `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:569`, `app/ponto/motor_calculo_v2.py:3279`

## 25/11 02:38 · sessao 2d04f1524f51 · PONTA-PEQUENA-NA-BORDA · verificador · origem=Code

**BLOQUEIA a v5.** A supressão resolve a reemissão com cadastro e batidas fixos, mas também bloqueia o prazo de um dia que o consumidor baixou sem medir a ponta. Além disso, a consulta com `agora=criado_em` usa a escala e as batidas atuais e pode classificar como pós-encerramento um evento que nasceu e foi baixado antes do fim previsto na época. `app/ponto/services/vigia_de_hora.py:107–124`, `app/ponto/services/ponta_pequena.py:94–118`, `app/ponto/turnos.py:1867–1893,2101–2105`.

### 1. Quem escreve, quem lê e o que é morto

- **Escrita.** `emitir_evento_celula` faz o INSERT e o ORM preenche `criado_em` por `auto_now_add`. O consumidor grava `processado_em` na baixa, incrementa `tentativas` e grava `erro` se o lote do colaborador falhar, e deixa a linha como está se adiar. `app/ponto/portas/celula.py:981–1002`, `app/escala/models.py:1869–1875`, `app/chamados/reconciliador.py:1539–1549`.
- **Leitura.** O consumidor ordena por `criado_em` e a ponta pequena usa o campo para fixar o instante por dia. O monitor da fila lê o mais antigo, o vigia da v5 lê o máximo por par e a foto E4B captura todos os campos. `eventos_pendentes`, `eventos_mortos` e o vigia da v5 leem o estado da fila. `app/chamados/reconciliador.py:1382–1404,1480–1486`, `app/ponto/services/ponta_pequena.py:110–118`, `app/ponto/services/vigia_de_hora.py:100–109`, `app/chamados/services/foto_e4b.py:247–252,269–271`.
- **Morto** é especificamente `processado_em IS NULL` com `tentativas >= 5`. Esse evento fica fora dos próximos lotes. Um **baixado** também fica fora dos lotes, mas passa no `exclude(filtro_evento_morto())`. A quinta falha pode fazer o vigia reemitir se o dia ainda for elegível e só restarem mortos ou eventos anteriores ao encerramento. Se a falha persistir, o ciclo pode se repetir com novos IDs. A baixa normal preserva a linha. A limpeza apaga baixados após sete dias, quando o dia já está fora da janela usual do vigia, que consulta hoje e ontem. `app/chamados/reconciliador.py:1343–1347,1382–1397,1540–1556`, `app/ponto/services/vigia_de_hora.py:22–30,107–124`.

### 2. O que o consumidor efetivamente julga

Com limite ligado, **sim**. Para cada data do lote, `decidir_eventos` usa `max(agora_do_início, maior criado_em daquela data)` e passa esse instante à elegibilidade e à medição. A origem do evento não filtra essa chamada. Lavra, nascimento de chamado de turno, recompute e pergunta chegam pela mesma porta. `app/chamados/reconciliador.py:1480–1495,1529–1535`, `app/ponto/services/ponta_pequena.py:83–118`, `app/ponto/portas/celula.py:981–991`.

**Contraexemplo com o consumidor atual.** Um evento de lavra nasce às 18:40, com o dia encerrado e `limite_decisao_he_min=0`. `decidir_eventos` retorna antes de olhar as datas do lote e o consumidor pode dar baixa normalmente. Se o admin liga o limite para 15 antes da próxima rodada, uma ponta de 9 minutos fica elegível. O save da empresa segue o caminho normal do cadastro, e, sem outro sinal para o par, a v5 conta `prazo_ja_emitido` pelo evento baixado e pula a emissão. `app/ponto/services/ponta_pequena.py:49–62,110–118`, `app/chamados/reconciliador.py:1534–1549`, `app/colaboradores/admin.py:7–19`, `app/colaboradores/models.py:82–89,172–175`, `app/ponto/services/vigia_de_hora.py:107–124`.

A mesma regressão aparece no cenário que o delta retirou do RED da v4: **18 minutos com limite 15**, baixa sem decisão, depois **limite 20**. A v4 exigia nova emissão e recusa. A v5 suprime pelo evento anterior. `app/ponto/tests/test_ponta_pequena_prazo_evento.py:247–253` **na v4**, trecho removido no anexo, `app/ponto/services/ponta_pequena.py:98–106`, `app/ponto/services/vigia_de_hora.py:113–124`.

### 3. Monotonicidade e leitura do passado

**Com os insumos fixos, sim.** Avançar `agora` só pode expirar uma aberta e ultrapassar o fim previsto. **Entre leituras do banco, a resposta pode mudar para o mesmo `u`.** O juiz carrega as batidas apuráveis da janela sem corte em `u` e lê vínculo e DNA atuais. A edição humana pode substituir o DNA, e a regeneração pode reescrevê-lo para uma célula gerada. `app/ponto/turnos.py:1867–1893,2005–2018,2101–2130`, `app/escala/models.py:1128–1144`, `app/ponto/portas/celula.py:289–311,641–664`.

Exemplo pelo código, com as batidas já pareadas: às **17:30**, o fim previsto é **18:00**, então o consumidor pode baixar o evento pelo ramo de dia aberto sem medir HE. Depois, o cadastro encurta o fim para **17:00**. A mesma chamada com `u=17:30` agora dá encerrado. A propagação de template tem caminho com `recalculo=False`, que grava o DNA e pula o rejulgamento imediato. Se não houver nova lavra ou outro sinal para o par, o vigia pode suprimir pelo evento antigo. `app/ponto/turnos.py:2121–2130`, `app/ponto/services/ponta_pequena.py:60–61,93–98`, `app/escala/services/cadastro_tipo.py:339–364`, `app/ponto/portas/celula.py:305–311,467–479,535–547`, `app/ponto/services/vigia_de_hora.py:118–124`.

### 4. Qual teste cai [nao verificado]

| Variante | Teste e primeira asserção que a distingue | [nao verificado]
|---|---|
| **v4**, supressão só por pendente | `test_RED_baixa_SEM_decisao_nascida_com_o_dia_encerrado_nao_reemite`. Às 18:50 espera `(0, 1)` e a v4 volta a emitir, `(1, 0)`. `app/ponto/tests/test_ponta_pequena_prazo_evento.py:234–248`. |
| Incluir **mortos**, mantendo o corte por encerramento | `test_RED_evento_morto_pos_encerramento_nao_suprime_prazo`. Espera `(1, 0)` e o mutante suprime, `(0, 1)`. Esse teste já existia no contexto do delta. `app/ponto/tests/test_ponta_pequena_prazo_evento.py:200–216`. |
| Aceitar evento **anterior ao encerramento** | `test_RED_baixa_nascida_ANTES_do_encerramento_reemite_e_decide`. O evento das 17:58 já foi baixado às 17:59. Às 18:40 o teste exige uma emissão e o mutante dá zero. Um mutante que aceite literalmente qualquer evento também cai aqui. `app/ponto/tests/test_ponta_pequena_prazo_evento.py:218–232`. |

### 5. Classes de reprovação no delta

- **Campo com dois significados.** `criado_em` registra o INSERT e a v5 usa esse mesmo carimbo como evidência de que o prazo foi ou será julgado sob os insumos relevantes. Os retornos antecipados do consumidor e a releitura do cadastro quebram essa inferência. `app/escala/models.py:1869–1875`, `app/ponto/services/vigia_de_hora.py:90–96,107–121`, `app/ponto/services/ponta_pequena.py:93–118`.
- **RED sem mordida na guarda que anuncia.** `test_RED_decisao_vigente_suprime` mantém um evento pós-encerramento já baixado. Se só a guarda de decisão vigente do vigia for ignorada, a deduplicação pelo evento ainda produz o zero que o teste cobra. O RED da baixa sem decisão também não exige que o espelho tenha sido chamado naquela passada. O teste separado de acima do limite exige essa chamada. `app/ponto/tests/test_ponta_pequena_prazo_evento.py:234–257,294–299`, `app/ponto/services/vigia_de_hora.py:113–121`.
- O delta reutiliza o juiz registrado e o filtro do outbox. O INSERT continua na mesma porta e no mesmo `atomic`. A troca da consulta não acrescenta outro juiz, escritor, leitura de relógio, derivação dos marcos, aquisição explícita de trava ou `except`. `app/core/juizes.py:243`, `app/chamados/reconciliador.py:1388–1391`, `app/ponto/services/vigia_de_hora.py:97–124,145–149`.

Conferi os dois arquivos pelos hashes de conteúdo do anexo e exercitei as variantes em memória com dependências simuladas. Execução da suíte Django, confirmação do commit-base `b8f12a692` e ocorrência dos contraexemplos em produção: [nao verificado].

## 25/11 02:37 · sessao 3f247a62fe22 · A-TELA-DO-DIA-LE-UM-JUIZ · construtor · origem=Code

Implementei a volta 6 com **uma classificação por ramo do cartório**. `classificar_dia` identifica a posição do protesto que o próprio ramo do sem par acrescentou, inclusive quando o código é `PREVISTO_COM_DIA_INDEFINIDO`. `_julgar_colab_corpo`, por `_retirar_sem_par_entregue`, consulta a entrega só quando existe esse protesto e retira exatamente aquela ocorrência antes da lavra. Isso preserva os demais protestos e dispensa o segundo julgamento da v5. [`ponto/supra_juiz.py:551`] [`ponto/services/cartorio.py:1264`] [`ponto/services/cartorio.py:1398`] [`ponto/services/cartorio.py:1614`]

Na primeira lavra com sinal, `_impressao_normal` imprime uma vez, com o sinal efetivo. Para uma foto existente, o cartório conserva a checagem de idempotência e incrementa `pulados_foto_entregue` quando a guarda impede a relavra. As linhas 1328 e 1492 do censo ficaram nas posições exigidas pela catraca. [`ponto/services/cartorio.py:1465`] [`ponto/services/cartorio.py:1531`] [`ponto/services/cartorio.py:1546`] [`ponto/services/cartorio.py:1627`] [`core/tests/test_medida_turno_do_par.py:319`]

**Seis selos**

As seis asserções passaram em checagem de AST isolada, com os métodos originais dos selos. A certificação da suite pelo Code e pelo Opus final fica **[nao verificado]**. [`ponto/tests/test_o219_versao_da_regra.py:224`] [`ponto/tests/test_e3_isento_na_impressao.py:122`] [`ponto/tests/test_fnd_pergunta_a_juiza.py:137`] [`ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:218`] [`ponto/tests/test_tripwire_o261_gate_le_o_fio.py:61`]

- **O219, dois selos, e E3**. Há duas chamadas de `impressao_insumos` no corpo. Ambas passam `regra=_regra_do_dia(...)` e `isento=_isento` explicitamente. [`ponto/services/cartorio.py:1380`] [`ponto/services/cartorio.py:1466`]
- **FND, F1C4-3 e O261**. As duas chamadas do cartório passam `**_cadastro(...)` literal, recebem o dia diretamente de `com_insumo_dia_sem_trabalho(...)` e declaram `cobrancas_vivas=`. O censo FND mantém quatro chamadas de produção. [`ponto/services/cartorio.py:1398`] [`ponto/services/cartorio.py:1614`] [`ponto/tests/test_fnd_pergunta_a_juiza.py:171`]

**RED e mutantes**

Na base `650392522`, o RED falhou com `2 != 1`. Na cópia, os sete casos em memória passaram e cada um dos seis mutantes falhou na asserção prevista. Os comandos carregam a base e os mutantes em memória. [`ponto/tests/test_a_tela_v6_sem_par_memoria.py:19`] [`ponto/tests/test_a_tela_v6_sem_par_memoria.py:41`] [`ponto/tests/test_a_tela_v6_sem_par_memoria.py:150`]

```bash
alvo=ponto.tests.test_a_tela_v6_sem_par_memoria.SemParUmaVez

PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest "$alvo"
A_TELA_V6_BASE=1 PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest \
  "$alvo.test_RED_primeira_lavra_classifica_uma_vez"

mutante() {
  A_TELA_V6_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest "$alvo.$2"
}

mutante ignora_entrega test_aberta_lavra_o_sem_par_e_imprime_o_sinal
mutante consulta_sinal_inerte test_sinal_inerte_mantem_impressao_sem_consultar_entrega
mutante imprime_sinal_barrado test_primeira_lavra_imprime_o_sinal_efetivo
mutante perde_foto test_foto_anterior_continua_com_contador_proprio
mutante marca_posicao_errada test_entregue_preserva_outro_protesto_e_retira_o_do_sem_par
mutante pula_guarda_no_nunca_bateu test_nunca_bateu_filtra_antes_de_derivar_veredito
```

**Cobertura da v5**

- Os casos B4 cobrem foto intacta, foto anterior à regra, primeira lavra sem acusação, contador na segunda passada, complemento de status, chamada avulsa e desaparecimento do sinal. [`ponto/tests/test_a_tela_v3_sem_par.py:183`] [`ponto/tests/test_a_tela_v3_sem_par.py:210`] [`ponto/tests/test_a_tela_v3_sem_par.py:233`] [`ponto/tests/test_a_tela_v3_sem_par.py:262`] [`ponto/tests/test_a_tela_v3_sem_par.py:277`] [`ponto/tests/test_a_tela_v3_sem_par.py:289`]
- Os controles anteriores cobrem dia cheio, impressão com sinal e precedência do marco apagado. A execução Django/ORM desses testes fica **[nao verificado]**. [`ponto/tests/test_a_tela_sem_par_acusa.py:194`] [`ponto/tests/test_a_tela_sem_par_acusa.py:202`] [`ponto/tests/test_a_tela_sem_par_v2.py:143`]

_8140 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
