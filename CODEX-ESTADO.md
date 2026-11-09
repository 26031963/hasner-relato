# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 18:23** · publicado em **09/11/2026 18:24** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 18:23 · sessao c723709f6c87 · - · - · origem=Ronald (tmux)

A1 — CONFORME: os três casos continuam na projeção; não foi criado fato equivalente nem migrado leitor no patch fornecido. A03 escreve `fechada_em/por`, `resolucao`, `motivo_fechamento`, `reaberturas_admin` e eventualmente `motivos` em `app/chamados/models.py:2149-2161`; a casca continua chamando essa porta em `app/chamados/services/acoes_disputa.py:255-259`, lê o contador em `:298`. A17 escreve conjuntamente `pendencia_admin` e `recusa_motivo` em `app/chamados/services/materializacao.py:850-851`; A18 escreve `recusa_motivo` em `:1102-1103`, ou pela porta de identidade em `:1099-1100` quando a colisão encerra por fato ocupado. O chamado recebe ocorrência em `:1105-1107`, mas isso não substitui a projeção como fonte dos leitores. O catálogo DP não contém atos próprios para esses três casos (`app/chamados/models.py:3853-3863`).

A2/D8 — CONFORME quanto à preservação antes/depois: os leitores continuam lendo a projeção. Leituras diretas de tela encontradas:

- Modal administrativo: `app/templates/chamados/partials/modal_fio.html:225`, sob `respondida_em` e `pendencia_admin` (`:216-221`); preserva advertência em `:222-225`.
- Atendimento do colaborador: `app/templates/chamados/partials/_card_atendimento.html:186`, com preferência anterior por `via_resolucao`; incluído por `app/templates/chamados/meu_atendimento.html:127` e `:144`.
- Explicador: `app/chamados/services/explicador.py:85-90` lê `pendencia_admin/recusa_motivo`; o fio o monta em `app/chamados/services/fio.py:302-314`. O modal renderiza seus itens em `app/templates/chamados/partials/modal_fio.html:433-442`.

Não afirmar visibilidade incondicional de toda recusa: o modal exige pendência para imprimir diretamente o campo (`app/templates/chamados/partials/modal_fio.html:221-225`), e o cartão só o mostra depois de esgotar as vias (`app/templates/chamados/partials/_card_atendimento.html:186`). Essas condições não foram alteradas pelo patch fornecido [nao verificado quanto ao diff completo ausente].

Outros leitores de `recusa_motivo`: `app/chamados/services/validacao.py:147`, `:188-208`; `app/chamados/services/passivo_validado.py:66`; `app/chamados/management/commands/auditar_invariantes_chamados.py:536`, `:1074-1075`, `:1111-1112`; `app/ponto/services/flip_auto.py:132` extrai identidade de batida pelo texto. Os campos de formulário em `modal_fio.html:60`, `:294` e `_pergunta_card.html:114` são entradas da recusa humana, não leituras da projeção.

B/modal — CONFORME: segue lendo `data_conferida_em/por`, nome completo ou username, em `app/templates/chamados/partials/modal_fio.html:169-170`; segue sendo escrito por `app/chamados/services/validacao.py:664-666`. O patch fornecido não altera esses sítios. A leitura do dia continua comparando a data da pergunta e a antiga do chamado (`app/templates/chamados/partials/modal_fio.html:161-167`).

C — sequência DP:

| Ato/campo | Escritor/chamador da porta | Leitor |
|---|---|---|
| Porta física DP | `app/chamados/models.py:3939-3948` | — |
| `resposta` | `app/chamados/services/disputa_emissao.py:1508-1509`, `:1653-1654`, `:1674-1675` | histórico `app/chamados/models.py:3504-3512`; tarefa `app/chamados/services/tarefa_do_dia.py:337-347` |
| `validacao` | `app/chamados/services/validacao.py:139-141` | tarefa `app/chamados/services/tarefa_do_dia.py:337-347` |
| `devolucao` | `app/chamados/services/validacao.py:205-208` | tarefa `app/chamados/services/tarefa_do_dia.py:337-347` |
| `reabertura/recusa` | `app/chamados/services/disputa_emissao.py:1612-1625`; recusa humana chama em `app/chamados/services/validacao.py:593-594` | histórico `app/chamados/models.py:3509-3512`; modal `app/templates/chamados/partials/modal_fio.html:204-211` |
| `silencio` | `app/chamados/models.py:1924-1926`; legado `app/chamados/management/commands/backfill_decisoes_pergunta.py:324-326` | tarefa `app/chamados/services/tarefa_do_dia.py:337-347`; seleção legado `app/chamados/management/commands/backfill_decisoes_pergunta.py:99-101`, `:315` |
| `retratacao` | `app/chamados/models.py:2056-2060` | tarefa `app/chamados/services/tarefa_do_dia.py:339-342` |
| `absorcao` | `app/chamados/services/materializacao.py:563-566` | carga com ausência `app/chamados/services/tarefa_do_dia.py:337-338` |
| Preservação R/V sem linha | `app/chamados/models.py:3910-3936`; nascimento `:3015`; ressuscitação `:1563-1564` | mesma função lê projeção; tarefa a usa em `app/chamados/services/tarefa_do_dia.py:343-347` |

O instante de resposta é o mesmo na projeção e DP (`app/chamados/services/disputa_emissao.py:1459-1465`, `:1508`); idem validação (`app/chamados/services/validacao.py:134-141`), silêncio (`app/chamados/models.py:1925`) e absorção (`app/chamados/services/materializacao.py:561-566`). A tarefa limita atos inclusivamente por `ato_em <= agora` (`app/chamados/services/tarefa_do_dia.py:339`); a ordem total DP é `ato_em, pk` (`app/chamados/models.py:3885`).

R/V/materialização mantêm leitura antiga: estado em `app/chamados/models.py:3037-3044`; carimbo físico único `_escrever_carimbo` em `:3093-3097`; `carimbar` em `:3120-3148`; volta avulsa em `:3200-3208`; volta em lote em `:3260-3262`; campos que acordam mantêm R/V/C em `:3216-3218`.

Observação útil sobre “campo com dois significados”: `recusa_motivo` já contém recusa técnica (`app/chamados/services/materializacao.py:1102`), advertência de tipo (`:850`), recusa humana (`app/chamados/services/validacao.py:584`), explicação de escape (`app/chamados/services/disputa_emissao.py:1494-1496`) e identidade da duplicata (`app/chamados/services/fio.py:1355`). O patch fornecido não cria nem altera esses usos; classificar como nova infração E4a seria incorreto [nao verificado quanto a exigência de sanar o legado].

Procedentes encontrados nesta subverificação: nenhum comprovado.

## 09/11 18:18 · sessao 87e4233cdf18 · FIO-E0-V4-E4-ESCRITORES · construtor · origem=Code

Entrega em [E4A_V3_ENTREGA.txt](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-180711/app/docs/E4A_V3_ENTREGA.txt:1), com parque integral, mapa de campos e comando ORM.

29 testes puros passaram; os sete mutantes morderam. [Resultados e comandos](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-180711/app/docs/E4A_V3_TESTES.txt:1). [nao verificado]

Não encontrei bypass no disparar; nenhuma cura produtiva foi necessária. [Análise](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-180711/app/docs/E4A_V3_ENTREGA.txt:90).

Resultados ORM e certificação final: [nao verificado].

## 09/11 18:11 · sessao 06071fff5ff7 · - · - · origem=Ronald (tmux)

Não encontrei caminho do `disparar` que ainda pergunte por DATA sem previsão fora da fábrica. Não recomendo alteração produtiva.

O universo célula lê `motivos_que_o_evento_deixou` em `chamados/management/commands/disparar_perguntas_competencia.py:112`, que passa por `motivos_a_perguntar` em `chamados/services/fabrica_da_celula.py:205`, calcula o teto em `:87` e entrega esse teto a `motivos_pela_ata` em `:107`. O universo FIO MUDO também passa explicitamente o teto em `chamados/management/commands/disparar_perguntas_competencia.py:158`. `vencidos=None` retorna `[]` em `chamados/services/fabrica_da_celula.py:38`. Só os motivos selecionados chegam ao escritor em `chamados/management/commands/disparar_perguntas_competencia.py:270`.

Ajustes identificados por leitura, sem executar ORM:

- `test_fabrica_pela_ata.py::FabricaPelaAta.test_05_sem_turno_previsto_vai_ao_cadastro_sem_pergunta` exige uma saída `CADASTRO-SEM-TURNO` ausente no comando HEAD; retirar essa exigência futura, conservando zero perguntas. `chamados/tests/test_fabrica_pela_ata.py:63`.
- `test_selo_emissor_fio_mudo.py::EmissorPedeDoChamadoMudoTest` tem seis casos afetados: `test_MORDE_o_emissor_ACHA_o_chamado_mudo`, `test_MORDE_desligado_e_barrado_e_NOMEADO`, `test_MORDE_competencia_exportada_VOLTA_a_ser_pedida`, `test_MORDE_pergunta_MORTA_do_MESMO_FATO_e_piso_F1t`, `test_MORDE_o_cron_leva_os_dois_caminhos_e_diz_por_porta`, `test_o_dry_nao_grava_nada`. As fixtures não têm vínculo nem DNA; o helper injeta ata, mas não previsão. `chamados/tests/test_selo_emissor_fio_mudo.py:36`, `:62`.
- No mesmo módulo, `FioMudoNaoSobreviveAUmCicloTest.test_MORDE_o_chamado_mudo_nao_sobrevive_a_um_ciclo` usa a mesma forma sem vínculo. `chamados/tests/test_selo_emissor_fio_mudo.py:256`, `:291`. Ajuste menor: dar previsão explícita no mock `marcos_vencidos` dentro de `_lei_e_ata`, acompanhado de comentário da LEI-PASSADO-SEM-MARCOS. Acrescentar vínculo antes de plantar o Chamado pode fazer o signal emitir antes do exercício; essa consequência precisa ser considerada. `chamados/signals.py:210`.
- `test_o261_julgamento_desfaz_carimbo.py::_Cenario._cenario` tem DNA, mas nenhum vínculo. Sem EC, `marcos_com_teto` retorna `[]`; a guarda nova manda a Pauta antes da coleta de carimbos. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:53`, `ponto/services/cartorio.py:856`, `chamados/services/fabrica_da_celula.py:164`.
- Quatro casos O261 inferidos vermelhos: `JulgamentoDesfazCarimboSemLastroTest.test_MORDE_o_julgamento_desfaz_o_carimbo_da_pergunta_viva`, `.test_julgar_duas_vezes_e_idempotente`, `VigiaMaterializadaVivaTest.test_MORDE_o_vigia_conta_e_zera_depois_do_julgamento`, `.test_MORDE_exportada_conta_no_balde_do_DP_e_o_julgamento_nao_tem_fronteira`. `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:88`, `:122`, `:208`, `:221`.
- O cenário O261 também alimenta `test_e1_v6_horizonte.py::FabricaLeOFatoTest.test_RED_pergunta_retratada_nao_cala_o_fato_que_volta_na_celula`, cuja expectativa passa de `['saida_sem_entrada']` a `[]`. `chamados/tests/test_e1_v6_horizonte.py:459`, `:467`. Acrescentar vínculo ao cenário compartilhado conserva a intenção desses testes sob a lei; existe `dar_vinculo` em `chamados/tests/_vinculo_do_dia.py:18`.

Nos outros apps com referências explícitas, não identifiquei expectativa alterada: F3 tem vínculo/DNA (`escala/tests/test_f3_beco1_cadastro_entrou.py:54`, `:72`); `test_cods_do_ato` e `test_classificar_falta_escala` substituem a fábrica (`ponto/tests/test_cods_do_ato.py:67`, `:95`; `ponto/tests/test_classificar_falta_escala.py:24`). Exaustividade sobre caminhos indiretos de todos os signals/cartório da árvore: [nao verificado]. Resultados ORM: [nao verificado].

## 09/11 18:10 · sessao 85fcb794b0e7 · - · - · origem=Ronald (tmux)

Inventário dos campos/valores do diff produtivo `f2962ab8d → 46d313581`:

| Campo/valor | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| DP `pergunta/pergunta_id`, `ato`, `ato_em`, `ato_por/id`, `ato_texto`, `ato_motivo` | `DecisaoPergunta.registrar`: `chamados/models.py:3945`; novas assinaturas de nascimento: `:3015`; conversão da projeção em linhas RESPOSTA/VALIDACAO: `:3929`, `:3933` | Correspondência para preservar legado: `chamados/models.py:3923`; histórico: `:3504`; tarefa por sequência/horizonte: `chamados/services/tarefa_do_dia.py:131`, `:337` |
| DP fotos `respondida_original_em`, `validada_original_em`, `validada_original_por_id` e vínculos de absorção | Portas existentes, gravadas pelo mesmo executor `chamados/models.py:3945`; campos e restrições: `:3873`, `:3894` | Histórico distingue atos com foto: `chamados/models.py:3509`; tarefa lê ato final: `chamados/services/tarefa_do_dia.py:136`; constraints: `chamados/models.py:3894` |
| Pergunta `respondida_em/resposta_colab` | Porta de resposta existente: `chamados/services/disputa_emissao.py:1464`; nascimento recebe `**campos`: `chamados/models.py:3010`; ressuscitação limpa instante via `:1566` | Nova conversão para DP: `chamados/models.py:3920`, `:3929`; C6 mantém projeção: `chamados/juizes.py:1339`; app: `chamados/services/pergunta_no_app.py:70` |
| Pergunta `validada_em/validada_por/id` | Validação existente: `chamados/services/validacao.py:134`; resposta invalida: `chamados/services/disputa_emissao.py:1477`; ressuscitação: `chamados/models.py:1566` | Conversão para DP: `chamados/models.py:3931`; C6: `chamados/juizes.py:1323` |
| Pergunta `materializada_em/materializada_por/id/via_resolucao` | Porta de carimbo: `chamados/models.py:3093`; limpeza por lote: `:3260`; ressuscitação usa a porta: `:1566` | C6: `chamados/juizes.py:1334`, `:1360`, `:1366`; fábrica busca candidatas com carimbo: `chamados/services/fabrica_da_celula.py:136` |
| Pergunta `pk`, `disputa_id`, `disputa__colaborador_id`, `celula/id`, `data_evento`, `chamado_destino/id`, `conversa`, `motivo` | Nascimento conserva campos: `chamados/models.py:3010`; reuso: `:2962`, `:2978`; reapontamento: `chamados/services/fio.py:925`; data canônica: `chamados/services/fio.py:790` | Captura de origem do evento: `chamados/models.py:3224`; reuso: `:2958`; ressuscitação: `:1565`; reapontamento: `chamados/services/fio.py:924`; fábrica/dedup: `chamados/services/fabrica_da_celula.py:113`, `:116` |
| Pergunta `veredito_lavrado/julgado_em/janela_expira_em` | Lavrador existente: `chamados/veredito_lavrado.py:270`; reuso salva os mesmos campos: `chamados/models.py:3001`; fio: `chamados/services/fio.py:927` | Validade temporal: `chamados/veredito_lavrado.py:228`; tarefa mantém seu leitor: `chamados/services/tarefa_do_dia.py:351` |
| Chamado `status_local`, `_b5_status_antes`, `modulo_origem`, `celula_id`, `pk`, `colaborador` | Estado pela porta existente: `chamados/models.py:228`; snapshot do status persistido: `chamados/signals.py:184` | Novo evento de volta a VIVOS: `chamados/signals.py:191`, `:201`; VIVOS segue catálogo: `chamados/catalogo/motor.py:24`; julgamento seguinte: `chamados/signals.py:214` |
| EventoCelula `celula_id/colaborador_id/data` | Escritor único: `ponto/portas/celula.py:892`; novos emissores: `chamados/models.py:1568`, `:3003`, `:3946`; `chamados/services/fio.py:929`; `chamados/signals.py:207`; reversão: `ponto/portas/celula.py:796` | Consumidor agrupa por colaborador e IDs: `chamados/reconciliador.py:1346`, `:1350`, `:1352`; fábrica lê datas do lote e relê célula atual: `chamados/services/fabrica_da_celula.py:260`, `:262` |
| EventoCelula `processado_em/tentativas/erro` | Consumidor existente, depois da nova fábrica: `chamados/reconciliador.py:1395`, `:1401` | Seleção pendente: `chamados/reconciliador.py:1343`, `:1393`; retenção: `:1408` |
| CelulaDia `pk/data/colaborador_id`, `trabalha`, `ata`, `veredito`, `julgada_em`; Colaborador `situacao='ativo'` | Portas existentes da célula/cartório: `ponto/portas/celula.py:900`, `:1002`; cadastro define situação: `colaboradores/models.py:323` | Fábrica mantém leitura da célula: `chamados/services/fabrica_da_celula.py:80`, `:88`, `:91`; novo encaminhamento CADASTRO: `:164`; consumidor relê atual por colaborador/data: `:262` |
| Retorno `vencidos=None`, `set()`, pares `(tipo, HH:MM)` | Adaptador do teto: `chamados/services/fabrica_da_celula.py:26`, `:28`; a autoridade continua `cartorio.marcos_vencidos` | `None`/vazio deixam de autorizar pergunta: `chamados/services/fabrica_da_celula.py:38`; `None` passado encaminha CADASTRO: `:166`; pares limitam motivos: `:49` |
| Pauta `ancora_tipo=ANCORA_COLAB`, `ancora_id`, `destinatario_departamento='dp'`, `pai=None`, marcador `CADASTRO: PASSADO SEM TURNO PREVISTO`, `autor_user` sistema `ti`, `remetente_informado='ti'`, `texto` | Novo chamador: `chamados/services/fabrica_da_celula.py:294`; escritor único efetivo: `pautas/services.py:180` | Guarda histórica por colaborador/cabeça/DP/marcador: `chamados/services/fabrica_da_celula.py:274`; `ANCORA_COLAB` é importado, sem literal inventado: `:273` |
| Celula `dna/dna_anterior/trabalha/escala_geradora_id/regenerada_em/regeneracoes` | Reversão existente conserva sua escrita: `ponto/portas/celula.py:786`, `:794`; diff acrescenta somente evento `:796` | Própria reversão lê DNA anterior: `ponto/portas/celula.py:777`, `:783`; leitor de reescrita: `escala/services/regeneracao.py:84`, `:104`; fábrica continua lendo ata/célula, `chamados/services/fabrica_da_celula.py:91`, `:107` |

O diff produtivo não altera escritores de cron, registro de juízes, campos de conferência, pendência administrativa ou recusa técnica; os novos testes podem compará-los, mas isso não acrescenta escrita produtiva. Evidência: `git diff f2962ab8d 46d313581 -- chamados/models.py chamados/reconciliador.py chamados/services/fabrica_da_celula.py chamados/services/fio.py chamados/signals.py ponto/portas/celula.py`; cada bloco acima referencia os fontes reais.

Comando ORM principal e vizinhos diretamente relacionados às funções alteradas, para a sessão executar, sem execução aqui:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings_test \
chamados.tests.test_e4_expand_orm \
chamados.tests.test_e1_entradas \
chamados.tests.test_e1_volta2 \
chamados.tests.test_e1_v12 \
chamados.tests.test_e1_v13_cadeia \
chamados.tests.test_e1_v15 \
chamados.tests.test_e1_v15b \
chamados.tests.test_e1_v15d_um_ponto_de_escrita \
chamados.tests.test_e1_v15f \
chamados.tests.test_e1_v15g_portas \
chamados.tests.test_e1_v5_ordem_trava \
chamados.tests.test_e1_v5_portas_backfill \
chamados.tests.test_e1_v6_horizonte \
chamados.tests.test_e1_v9 \
chamados.tests.test_fabrica_pela_ata \
chamados.tests.test_fabrica_por_evento \
chamados.tests.test_f1c4_3_emissores \
chamados.tests.test_cobparc5_parcial_sem_periodo_so_lista \
chamados.tests.test_pergunta_do_dia_em_curso \
chamados.tests.test_o261_julgamento_desfaz_carimbo \
chamados.tests.test_jv4a_disparar_vigia_da_celula \
chamados.tests.test_c20_juiz3_outbox \
chamados.tests.test_jv3_reconciliar_vira_vigia \
chamados.tests.test_orfas_pelo_evento \
chamados.tests.test_orfas_nao_morre_na_guarda_da_porta \
chamados.tests.test_porta_reapontamento_pergunta \
chamados.tests.test_o261e_f1_codex_v5 \
chamados.tests.test_o261e_f1_porta_encerramento \
chamados.tests.test_reconciliar_pergunta_a_premissa \
chamados.tests.test_e2_celula_regenerada_reavalia \
ponto.tests.test_porta_celula_dia \
ponto.tests.test_hx_cartorio_ressuscita \
ponto.tests.test_d3_impressao_cobre_insumos \
ponto.tests.test_c3_ordem_dos_receivers_da_batida \
ponto.tests.test_lastro_parte2_fecho_no_evento \
ponto.tests.test_lastro_parte3_fecho_na_pergunta \
ponto.tests.test_o_outbox_celula \
ponto.tests.test_bug114_fechador_de_dois_dias \
ponto.tests.test_bug115_lampada_do_marco \
ponto.tests.test_orfas_vigia
```

Evidência de vizinhança: imports da fábrica em `chamados/tests/test_fabrica_por_evento.py:35`, `test_f1c4_3_emissores.py:76`, `test_o261_julgamento_desfaz_carimbo.py:82`; consumidor em `test_c20_juiz3_outbox.py:241`, `test_jv3_reconciliar_vira_vigia.py:141`, `ponto/tests/test_o_outbox_celula.py:165`; fio em `chamados/tests/test_porta_reapontamento_pergunta.py:271`; signal em `chamados/tests/test_reconciliar_pergunta_a_premissa.py:269` e `ponto/tests/test_d3_impressao_cobre_insumos.py:369`; reversão em `ponto/tests/test_porta_celula_dia.py:184`; ressuscitação em `chamados/tests/test_e1_v13_cadeia.py:140`.

Todos os resultados ORM permanecem `[nao verificado]`.

## 09/11 17:58 · sessao 323aebf8a713 · FIO-E0-V4-E4-ESCRITORES · construtor · origem=Code

Entrega: [E4A_V2_ENTREGA.txt](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_V2_ENTREGA.txt:1).

- 35 testes sem banco passaram; oito mutantes morderam e foram restaurados. [Evidências](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_V2_TESTES.txt:29). [nao verificado]
- ORM não executado; comando preparado para a sessão. [Referência](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_V2_ENTREGA.txt:175).
- O pouso ainda depende dos hunks e das decisões explícitas sobre A05 e C11. [Pendências](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_V2_ENTREGA.txt:222). [nao verificado]

## 09/11 17:42 · sessao c84017ed8f71 · - · - · origem=Ronald (tmux)

C11 por ato:

| Ato | Evento demonstrável |
|---|---|
| Abertura `batida_ausente` | `abrir`: `chamados/models.py:977–980`; `criar`: `:1093–1099`. Ambos inserem evento no atomic do nascimento. |
| Reabrir chamado/transição para VIVOS | `chamados/signals.py:194–203`, antes do best-effort/cartório. `_gravar_estado` salva dentro de atomic em `chamados/models.py:280–296`. |
| Retratar duplicata | `chamados/services/fio.py:1354` → `carimbar_identidade` → save; receiver de pergunta em `chamados/signals.py:237–245`; save atômico em `chamados/models.py:3402–3413`. |
| Validação | `chamados/services/validacao.py:136–139` salva e registra DP no mesmo atomic; evento pelo receiver acima. |
| Morte pela leitura da célula | `ponto/portas/celula.py:894–902`: célula e evento no mesmo atomic. |
| Reapontar pela porta fio | Captura origem em `chamados/services/fio.py:924`; salva em `:927`; acorda origem em `:929`, no atomic aberto em `:881`. |

Lacunas adicionais:

1. **Tranca de competência:** C6 morre por leitura da tranca em `chamados/juizes.py:1325–1328`; `trancar_periodo` não emite evento nem possui atomic próprio (`ponto/services/fechamento.py:1400–1430`). Construção permitida sugerida: atomic envolvendo criação/retranca e evento por `CelulaDia` da empresa dentro de `janela_fechamento(mes, ano, empresa_id)`. Essa janela aceita ID e consulta o corte canônico (`ponto/janelas.py:36–57`). Manter `disparar_no_ato` fora do atomic: pode abrir subprocesso (`chamados/services/competencia_trancada.py:290–298`). Segunda chamada com `ato=False` não emite novamente (`ponto/services/fechamento.py:1411`).

2. **Reuso em `_nascer`:** muda célula/destino e salva, sem acordar origem (`chamados/models.py:2955–2998`). Hunk textual: capturar `cls.linhas_do_evento(cls.objects.filter(pk=ex.pk))` antes de `upd=[]`; após `ex.save(update_fields=upd)`, chamar `cls.acordar_celulas(linhas_antes)` no atomic existente (`chamados/models.py:2839`).

3. **Reversão de regeneração:** save cru em `ponto/portas/celula.py:794–795`; termina em `:808`, sem emissão e sem `_o_chao_do_dia_mudou`. Edição chama esse rabo em `:627`; regeneração em `:354`. Hunk mínimo textual: `emitir_evento_celula(cel)` depois desse save, ainda no atomic de `:785`. Entretanto, o INSERT sozinho não relavra ATA/veredito: fábrica lê os motivos da célula em `chamados/services/fabrica_da_celula.py:170`. Não afirmar cobertura completa da reversão com esse hunk isolado.

PERGUNTAS para sessão:

- Aceita-se que o erro de outbox impeça a transição para VIVOS? O caminho alterado é o `post_save` de **qualquer save existente com mudança de status para VIVOS**, módulo de turno e célula (`chamados/signals.py:194–203`). Pela porta `_gravar_estado`, participa do mesmo atomic (`chamados/models.py:280–296`). Saves diretos não ganham atomic genérico: `ChamadoColaborador` não contém wrapper equivalente ao de PerguntaDisputa (`chamados/models.py:3402–3413`).
- Se construir tranca+evento atomicamente, falha da outbox passa a desfazer a tranca. O comentário atual promete que a tranca nunca cai por causa da fila (`ponto/services/fechamento.py:1423`). Não resolver silenciosamente essa mudança.

H2 continua aplicável à implementação atual: `registrar` só executa `bulk_create` (`chamados/models.py:3932–3934`); o texto acrescenta atomic+outbox (`docs/E4A_HUNKS_SESSAO.txt:66`). H4 corresponde às três mudanças necessárias: remover `--apply` do ENC (`config/crons.py:1521`), retirar nome da lista de juízes (`:2048`) e declarar papel vigia (`:2164–2169`). Sem H4 aplicado, cron chama opção ausente no comando desta cópia (`docs/E4A_HUNKS_SESSAO.txt:123`).

## 09/11 17:42 · sessao 760a6e6cb3db · - · - · origem=Ronald (tmux)

Mapa de provas existentes, sem executar ORM:

| Porta | Prova aproveitável | Lacuna para selo EXPAND |
|---|---|---|
| A01 | Estado canônico + trilha: `chamados/tests/test_trilha_na_ordem_da_escrita.py:67`; porta válida: `chamados/tests/test_selo_e4_gates.py:59` | Comparar toda projeção das perguntas antes/depois, sem exigir DP para efeito Chamado. |
| A02 | SILENCIO com autor: `chamados/tests/test_e1_entradas.py:121` | Exigir F/C preservados, instante correspondente e 2x sem novo DP. |
| A04 | Exercício indireto na devolução: `chamados/tests/test_o261_recusa_fala.py:164` | Caso direto `reabrir_sistema`, F completo e 2x com uma nota; não possui DP próprio. |
| A05 efeito puro | Carimbo aceito: `chamados/tests/test_o261e_f2_porta_carimbo.py:676`; recusa restaura memória/banco: `:713` | Caracterizar C completo com `resposta=None`; assinatura segue exceção conflitante do pedido. |
| A07 | Três campos C limpos juntos: `chamados/tests/test_o261e_f2_porta_carimbo.py:754` | Exigir R/V intactos e nenhum DP inventado pelo efeito técnico. |
| A08 | Exercício por correção com absorção: `chamados/tests/test_e1_volta2.py:200`; 2x = 1 DP: `:217` | Caso direto do lote, incluindo `outros` e C/R/V completo. |
| A09 | 2x = mesmo estado e zero query na segunda: `chamados/tests/test_porta_chamado_e3.py:70` | Exigir demais projeções intactas e nenhum DP humano fabricado. |
| A10 | DP VALIDACAO com autor e mesmo instante de V: `chamados/tests/test_e1_entradas.py:94` | Asserção explícita da projeção completa. `_carimbar_validacao` não declara idempotência própria. |
| A11 | Conserva resposta/recusa, limpa V e reabre F: `chamados/tests/test_o261_recusa_fala.py:164`; conserva V legada em DP: `chamados/tests/test_e1_volta2.py:101` | Exigir exatamente um novo DP DEVOLUCAO e repetir a porta. |
| A12 | Cada resposta é uma linha; anterior imutável; mesmo instante R/DP: `chamados/tests/test_e1_entradas.py:64`; invalida V: `chamados/tests/test_porta_resposta_pergunta.py:224` | Comparar toda projeção. Repetição não é idempotente: a própria prova caracteriza duas trilhas em `test_porta_resposta_pergunta.py:216`. |
| A13 | Sem dados não fabrica DP: `chamados/tests/test_e1_entradas.py:115`; foto da absorção e 2x: `chamados/tests/test_e1_volta2.py:200`, `:217` | Reabertura ordinária com foto R/V, F intacto e exatamente 1 DP. |
| A14 | 2x sem query; voz não sobrescrita: `chamados/tests/test_porta_chamado_e3.py:43`, `:50` | Exigir 1 DP RESPOSTA, instante injetado e R completo. |
| A15 | Testemunha preservada e 2x sem mudar instante/trilha: `chamados/tests/test_porta_resposta_pergunta.py:241` | Exigir exatamente 1 DP RESPOSTA com texto/instante testemunhados. |
| A16 | Via e L no mesmo save: `chamados/tests/test_porta_veredito_pergunta.py:343`; 2x = uma trilha: `:372` | Exigir R/V/C restante preservado e nenhum DP de decisão inventado. |
| A19 | Uma RECUSA, autor/motivo e texto retirado: `chamados/tests/test_e1_entradas.py:103` | Exigir motivo projetado e R/V zerados; 2x = mesma projeção e um DP RECUSA. |
| A21 | RETRATACAO, via terminal, resposta preservada: `chamados/tests/test_e1_entradas.py:142` | Repetir remoção e comparar motivos/contexto/projeção e quantidade de DP. |
| A22 | 2x mesmo fato = uma pergunta e uma trilha: `chamados/tests/test_porta_pergunta_identidade.py:167`; reuso sem escrita: `chamados/tests/test_e1_v15.py:44` | Nascimento/defaults R/V/C e reuso conservando DP/projeções existentes. |
| A23 | Limpa resposta e conta: `chamados/tests/test_porta_disputa_ciclo.py:283`; 2x não conta/trilha de novo: `:297` | Exigir exatamente um DP REABERTURA com autor/foto, além do contador/projeção já provados. |

A fixture mais simples para um novo módulo ORM é `_Base` de `chamados/tests/test_e1_entradas.py:30`: cria Empresa/Colaborador/User; `_pergunta` nasce pela porta em `:37`; `_responder` executa o core e recarrega em `:44`. O relógio desse módulo é fixado por `RELOGIO` em `:26` e `freeze_time` em `:51`. Para efeitos que dependem de chamado/célula e lavra, a fixture de `chamados/tests/test_porta_veredito_pergunta.py:223` já prepara esse universo.

Não recomendo afirmar “1 DP” para A01/A04/A05 puro/A07/A08 direto/A09/A16/A22: o vocabulário atual representa decisões humanas ou atos definidos, enquanto esses corpos executam efeitos/identidade. O requisito permite “DP ou efeito canônico”; adicionar RESPOSTA/REABERTURA a efeitos técnicos alteraria significado. (`chamados/models.py:3846`; `chamados/services/relavra_via.py:106`; `chamados/models.py:3192`; `chamados/models.py:2822`)

Resultados dessas provas no banco desta cópia: **[nao verificado]**.

## 09/11 17:42 · sessao f9abd31ce653 · - · - · origem=Ronald (tmux)

Leitura concluída, sem escrita.

| Porta | Evidência / classificação |
|---|---|
| A01 `_gravar_estado` | Efeito canônico Chamado + trilha no mesmo atomic: `chamados/models.py:280`, `:292`, `:296`. Não corresponde a um ato DP por pergunta. |
| A02 `fechar` | Exceções já gravam `SILENCIO`: `chamados/models.py:1921`; F persiste no mesmo atomic: `:1934`. Fechamento normal conserva efeito/quitação, sem inventar decisão. |
| A04 `reabrir_sistema` | Efeito no continente e nota existente: `chamados/models.py:2174`, `:2177`. `REABERTURA` disponível significa correção da pergunta, não reabertura do continente: `:3853`. |
| A05 `carimbar`, ramo puro | Efeito/carimbo e L: `chamados/models.py:3118`, `:3140`, `:3141`. |
| A05 assinatura | **Lacuna confirmada**: atribui R/V em `chamados/models.py:3121`, `:3124`; salva sem DP em `:3141`. |
| A07 `descarimbar` | Limpeza do carimbo: `chamados/models.py:3197`, `:3201`; caller de lastro executa consequência do juiz: `chamados/services/relavra_via.py:242`, `:246`. |
| A08 `descarimbar_em_lote` | Executor de limpeza: `chamados/models.py:3253`; correção registra DP antes do lote: `chamados/services/disputa_emissao.py:1612`, `:1627`. Ressuscitação conserva decisões antigas antes da limpeza: `chamados/models.py:1563`, `:1565`. |
| A09 `encerrar_por_fato_no_espelho` | Efeito já existente do espelho, via idempotente: `chamados/models.py:3262`, `:3264`, `:3265`. |
| A10 `_carimbar_validacao` | Já grava `VALIDACAO` no atomic do save: `chamados/services/validacao.py:136`, `:139`. |
| A11 `devolver_validacao_recusada` | Já grava `DEVOLUCAO`: `chamados/services/validacao.py:201`, `:205`; segunda chamada não registra novamente quando V já nulo: `:195`. |
| A12 resposta core | Já grava `RESPOSTA` no atomic: `chamados/services/disputa_emissao.py:1500`, `:1505`, `:1508`. Advertência permanece em `recusa_motivo`: `:1494`. |
| A13 correção | Já grava `REABERTURA`/`RECUSA` antes da limpeza: `chamados/services/disputa_emissao.py:1602`, `:1612`, `:1624`, `:1629`. |
| A14 automática | Já grava `RESPOSTA`: `chamados/services/disputa_emissao.py:1651`, `:1653`; guarda do texto mantém idempotência existente: `:1644`. |
| A15 trilha | Já grava `RESPOSTA` no instante testemunhado: `chamados/services/disputa_emissao.py:1671`, `:1674`; guarda idempotente: `:1667`. |
| A16 relavra | Efeito derivado + L pelo juiz existente: `chamados/services/relavra_via.py:120`, `:125`, `:127`. Não representa nova decisão humana. |
| A19 recusa | Já delega DP `RECUSA` dentro do atomic da projeção: `chamados/services/validacao.py:588`, `:590`, `:593`; executor DP em `chamados/services/disputa_emissao.py:1612`. |
| A21 remover motivo | Já grava `RETRATACAO` e carimbo no atomic: `chamados/models.py:2023`, `:2053`, `:2059`. |
| A22 nascer/reuso | Reuso altera identidade/costura/L, sem nova escrita R/V/C: `chamados/models.py:2955`, `:2997`, `:2998`; nascimento aceita `campos` e persiste em `:3005`, `:3008`. |
| A23 colaborador | Já delega correção com DP dentro do atomic do contador: `chamados/models.py:2232`, `:2234`, `:2236`; executor DP em `chamados/services/disputa_emissao.py:1612`. |

A contradição precisa constar em PERGUNTAS: o pedido exclui expressamente A05 **ramo de assinatura**, mas manda começar pelo RED que exige DP precisamente nesse ramo. A falha é real (`chamados/models.py:3121`, `:3124`, `:3141`); qual instrução prevalece é **[nao verificado]**.

Se autorizado pela sessão, o hunk mínimo de A05 deve acumular `RESPOSTA` somente ao preencher R ausente, `VALIDACAO` somente ao preencher V ausente, com texto/instante/autor das atribuições existentes; depois envolver `save` + `DecisaoPergunta.registrar` no mesmo atomic dentro do `try` atual. O rollback em memória existente precisa continuar cobrindo falha do registrar (`chamados/models.py:3137`, `:3143`, `:3144`). Não inserir DP no ramo puro nem repetir assinatura já presente (`:3120`, `:3123`).

`registrar` não deduplica: faz apenas `bulk_create`, então a precondição deve continuar na porta (`chamados/models.py:3934`).

O vocabulário disponível é fechado em oito atos (`chamados/models.py:3846`, `:3847`); classificar indiscriminadamente limpeza técnica, relavra, nascimento ou reabertura de continente como `REABERTURA` alteraria o significado documentado em `:3853`.

Não encontrei `Parallel Change`/`Expand-Contract` nos dois catálogos consultados; resultado da busca local, não confirmação universal **[nao verificado]**. Termos efetivamente encontrados: `Single Writer Principle` em `docs/GLOSSARIO.md:12` e `Architecture Conformance Test` em `docs/GLOSSARIO.md:15`.

## 09/11 17:35 · sessao 98f00f1fa6ec · FIO-E0-V4-E4-ESCRITORES · construtor · origem=Code

**E4a não está pronta:** o RED final mantém 9 falhas, incluindo identidade, fatos ausentes e registro C20. [Resultado](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_TESTES.txt:784).

- [Entrega: diff, escritores/leitores, lacunas e pendências](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_ENTREGA.txt:1).
- [Hunks textuais para a sessão](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_HUNKS_SESSAO.txt:1).
- [Provas: 24 testes verdes e comandos dos 18 mutantes que falharam e foram restaurados](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_TESTES.txt:1). [nao verificado]

Sem commit, pela proibição de escrever em `.git*`. [Registro](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_ENTREGA.txt:292). [nao verificado]

## 09/11 17:13 · sessao af1c0c27e636 · - · - · origem=Ronald (tmux)

C20 precisa destes quatro movimentos coordenados:

1. O comando vira vigia puro: retirar `_rodar` e seus blocos de escrita, hoje em [disparar_perguntas_competencia.py:266](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/management/commands/disparar_perguntas_competencia.py:266), e fazer a leitura usar `motivos_que_o_evento_deixou`, cuja composição já reúne os gates da fábrica e a Pauta do limbo. [fabrica_da_celula.py:201](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fabrica_da_celula.py:201)

2. O consumidor passa a chamar `perguntar_pela_celula` depois do lastro. O `lote` já oferece as datas; carregar as células atuais por colaborador e `data__in={e.data for e in lote}` evita depender do identificador da célula anterior à regeneração. Reutilizar `agora` da rodada e manter as exceções dentro do bloco que deixa eventos pendentes. [reconciliador.py:1337](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/reconciliador.py:1337), [reconciliador.py:1348](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/reconciliador.py:1348), [reconciliador.py:1383](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/reconciliador.py:1383)

3. As fontes precisam permanecer completas após cessar os saves das projeções: retorno do chamado a VIVOS, registro de DP e reapontamento com origem e destino devem emitir o evento no mesmo atomic. Hoje o signal de pergunta depende do save e observa somente a situação posterior; `_gravar_estado` não emite explicitamente o evento. [signals.py:237](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/signals.py:237), [models.py:294](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/models.py:294), [fio.py:922](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fio.py:922)

4. Passado sem marcos já tem corte: não pergunta ao colaborador; cria pendência CADASTRO, uma por colaborador, com contador. O emissor antigo ainda retorna `None` por comparação de datas e alcança `classificar_falta`. [PROMPTS.md:2004](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/PROMPTS.md:2004), [fabrica_da_celula.py:29](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fabrica_da_celula.py:29), [fabrica_da_celula.py:55](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fabrica_da_celula.py:55)

A Pauta cabe pela porta existente `pautas.services.escrever`, usando `ANCORA_COLAB`, destinatário DP e marcador estável. Sob atomic e trava do colaborador, consultar toda a história das cabeças e escrever somente se nenhuma existir; o contador deve contar colaboradores, não dias. Essa implementação é proposta, ainda [nao verificado]. A porta e a âncora existem em [services.py:152](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/pautas/services.py:152) e [models.py:30](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/pautas/models.py:30).

O risco de loop vem do consumidor reler eventos novos na mesma execução. A fábrica deve escrever apenas diante de motivos efetivamente ausentes; sua seleção já exclui identidades existentes, e o nascimento da pergunta não aciona o outbox. Uma segunda passagem deve resultar em `motivos=[]`, sem save nem relavra. [reconciliador.py:1343](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/reconciliador.py:1343), [fabrica_da_celula.py:124](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fabrica_da_celula.py:124), [signals.py:239](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/signals.py:239)

Hunk proibido necessário em `config/crons.py`:

```diff
@@ ENC('disparar_perguntas_competencia', ...)
-    ENC('disparar_perguntas_competencia', gatilho='apurar_furos_diarios', args='--comp-corrente --apply',
+    ENC('disparar_perguntas_competencia', gatilho='apurar_furos_diarios', args='--comp-corrente --alarme',
@@ JUIZES_POR_VARREDURA = (
-    # 'disparar_perguntas_competencia' FICA, e so pelo FIO MUDO: no JUIZ-VARREDURA-4 parte A o caminho da CELULA virou
-    # VIGIA (conta o que o evento `fabrica_da_celula.perguntar_pela_celula` deixou, esperado 0, SystemExit(2)); o fio
-    # mudo (chamado vivo sem pergunta viva) segue ESCREVENDO -- nao ha evento dele e a lei esta aberta (CORTES.md,
-    # PAPEL-PRAZO-DISPARAR-PERGUNTAS 27/10).
-    'disparar_perguntas_competencia',
@@ PAPEL_DO_CRON = dict(
     **{n: 'vigia' for n in (
+                            'disparar_perguntas_competencia',
```

Âncoras reais: [crons.py:1521](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/config/crons.py:1521), [crons.py:2044](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/config/crons.py:2044), [crons.py:2169](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/config/crons.py:2169). Não encontrei registro desse comando em `core/juizes.py`; alteração ali para C20: [nao verificado].

A condição de saída permanece literalmente “fio mudo medido + C11 pousado”. Esta exploração não comprova nenhuma dessas duas condições, nem `20/20`; o contrato estrutural consulta as exceções reais e não deve ganhar `verde=True` artificial. [CORTES.md:305](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/CORTES.md:305), [contratos_estruturais.py:264](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/core/contratos_estruturais.py:264) [nao verificado]

## 09/11 17:13 · sessao e00ecb618eb2 · - · - · origem=Ronald (tmux)

Leitura concluída sem editar arquivos. O corte completo de identidade e continente permanece dependente de representação factual ausente; hunks apenas de leitura R/V podem ser aplicados separadamente. (`chamados/models.py:2752`, `chamados/models.py:3474`, `chamados/models.py:3480`, `chamados/models.py:3860`)

**Hunk de apresentação**, arquivo `chamados/models.py`, âncora imediatamente anterior a `PerguntaDisputa.estado` (`chamados/models.py:3022`):

```diff
@@
-    @property
-    def estado(self):
+    @property
+    def atos(self):
+        """Leitura mecanica da sequencia; nao julga tarefa nem efeito."""
+        from chamados.services.atos_pergunta import leitura
+        return leitura(self)
+
+    @property
+    def resposta_do_ato(self):
+        return self.atos.resposta
+
+    @property
+    def respondida_no_ato(self):
+        return self.atos.respondida_em
+
+    @property
+    def validada_no_ato(self):
+        return self.atos.validada_em
+
+    @property
+    def validada_pelo_ato(self):
+        return self.atos.validada_por
+
+    @property
+    def recusa_humana_do_ato(self):
+        return self.atos.recusa_humana
+
+    @property
+    def estado(self):
```

Esse bloco não corrige `estado`: a propriedade ainda contém julgamento por V/C/R e precisa ser substituída por apresentação da autoridade da tarefa. Não propor atribuições aos campos antigos para alimentar o C6: isso perpetuaria a projeção em memória. (`chamados/models.py:3030`, `chamados/models.py:3034`, `chamados/models.py:3035`; `chamados/juizes.py:1323`, `chamados/juizes.py:1339`)

**Hunk de presença da resposta**, arquivo `chamados/models.py`, âncora `PerguntaDisputa.respondida` (`chamados/models.py:3486`):

```diff
@@
     @property
     def respondida(self):
-        return self.respondida_em is not None
+        return self.atos.respondida_em is not None
```

**Hunk de conteúdo discriminado**, arquivo `chamados/models.py`, âncora `PerguntaDisputa.resposta_display` (`chamados/models.py:3508`):

```diff
@@
     def resposta_display(self):
         # S80 - resolve ID para nome legivel quando tipo_resposta e dropdown
-        if not self.resposta_colab:
+        resposta = self.atos.resposta
+        if not resposta:
             return ''
         if self.tipo_resposta == 'posto_dropdown':
             try:
                 from colaboradores.models import Posto
-                p = Posto.objects.filter(pk=int(self.resposta_colab)).first()
+                p = Posto.objects.filter(pk=int(resposta)).first()
@@
-                logger.warning('resposta_display: pk de posto invalido (%r)', self.resposta_colab)
+                logger.warning('resposta_display: pk de posto invalido (%r)', resposta)
@@
-                e = EscalaColaborador.objects.filter(pk=int(self.resposta_colab)).select_related('tipo_escala').first()
+                e = EscalaColaborador.objects.filter(pk=int(resposta)).select_related('tipo_escala').first()
@@
-                logger.warning('resposta_display: pk de escala invalido (%r)', self.resposta_colab)
+                logger.warning('resposta_display: pk de escala invalido (%r)', resposta)
@@
-                if _o.get('valor') == self.resposta_colab:
-                    return _o.get('rotulo', self.resposta_colab)
-        return self.resposta_colab
+                if _o.get('valor') == resposta:
+                    return _o.get('rotulo', resposta)
+        return resposta
```

**Falta fato — A03.** Falta o ato de reabertura do continente, com disputa, autor, instante, motivo e identidade da operação. `REABERTURA` atual representa correção da resposta; DP exige uma pergunta, portanto uma disputa vazia não pode receber esse ato sem inventar pergunta. Acrescentar uma opção em `ATOS` sozinho não resolve. (`chamados/models.py:2123`, `chamados/models.py:2193`, `chamados/models.py:3860`)

Proposta textual **somente para decisão da sessão**, arquivo `chamados/models.py`, âncora `DecisaoPergunta`:

```diff
@@
     REABERTURA, SILENCIO, RETRATACAO, ABSORCAO = 'reabertura', 'silencio', 'retratacao', 'absorcao'
+    REABRIR_DISPUTA = 'reabrir_disputa'
@@
         (ABSORCAO, 'Absorvida pela ausencia declarada'),
+        (REABRIR_DISPUTA, 'Continente reaberto pelo admin'),
```

Para esse ato ser utilizável em continente vazio ainda falta decidir o sujeito factual: tornar `pergunta` opcional e acrescentar `disputa`, com constraint “exatamente um sujeito”, implicaria schema. Não construir essa mudança silenciosamente nesta E4a. (`chamados/models.py:3860`, `chamados/models.py:3877`)

**Falta fato — A17/A18.** Faltam resultado técnico, código, motivo completo, pergunta, instante e autor operacional. Pendência nasce em campos; recusa técnica nasce em campos e nota do chamado. Nenhuma é DP `RECUSA` humana. (`chamados/services/materializacao.py:850`, `chamados/services/materializacao.py:1089`, `chamados/services/materializacao.py:1107`)

Proposta textual, arquivo `chamados/models.py`, âncora `DecisaoPergunta.ATOS`:

```diff
@@
     REABERTURA, SILENCIO, RETRATACAO, ABSORCAO = 'reabertura', 'silencio', 'retratacao', 'absorcao'
+    PENDENCIA_TECNICA, RECUSA_TECNICA = 'pendencia', 'recusa_tecnica'
@@
         (ABSORCAO, 'Absorvida pela ausencia declarada'),
+        (PENDENCIA_TECNICA, 'Materializacao exige atendimento tecnico'),
+        (RECUSA_TECNICA, 'Materializacao recusada tecnicamente'),
```

Só o enum não preserva todo o resultado técnico: `ato_motivo` admite 240 caracteres e `ato_texto` está documentado como resposta. A sessão precisa decidir representação dos dados técnicos; reutilizar texto da resposta para payload quebraria o significado declarado. (`chamados/models.py:3837`, `chamados/models.py:3864`, `chamados/models.py:3865`)

**Falta fato — A20.** Faltam fato durável da conferência, par dos dias, autor, instante e justificativa integral. O log não garante existência: `registrar_log` captura qualquer exceção. (`chamados/services/validacao.py:664`, `chamados/services/validacao.py:667`, `core/auditoria.py:17`, `core/auditoria.py:25`)

Proposta textual, arquivo `chamados/models.py`, âncora `DecisaoPergunta.ATOS`:

```diff
@@
     REABERTURA, SILENCIO, RETRATACAO, ABSORCAO = 'reabertura', 'silencio', 'retratacao', 'absorcao'
+    CONFERENCIA_DATA = 'conferencia_data'
@@
         (ABSORCAO, 'Absorvida pela ausencia declarada'),
+        (CONFERENCIA_DATA, 'Par de dias conferido pelo admin'),
```

A opção tem 16 caracteres e cabe no campo atual, mas não dá campos próprios ao par e à justificativa. Proposta adicional para a sessão decidir, sem aplicação: `ato_dados = models.JSONField(default=dict, blank=True)`, com validação obrigatória específica por ato; isso exige schema e não é hunk executável isoladamente nesta entrega. (`chamados/models.py:3861`, `chamados/models.py:3865`)

**Identidade — impedimento material para N=0 seguro.** As duas constraints parciais excluem linhas através de `via_resolucao`; a porta de retratação escreve essa via e a duplicata nem possui DP correspondente. Cessar a via deixa a linha retratada dentro do índice: um nascimento legítimo posterior colide com ela. Não remover a constraint, substituir por dedup apenas em Python ou fingir que subquery DP cabe na condição de índice. (`chamados/models.py:2053`, `chamados/models.py:2058`, `chamados/models.py:2752`, `chamados/models.py:3471`, `chamados/models.py:3477`)

O teste E5 deve permanecer RED enquanto não existir representação factual de terminalidade consultável pelo banco e tratamento das constraints pela sessão; retirar fisicamente campo não é necessário para demonstrar esse impedimento. (`chamados/models.py:3474`, `chamados/models.py:3480`)

**A23 — idempotência sob trava.** Além de substituir o filtro R em `perguntas_corrigiveis`, a sessão deve mover a seleção corrigível e leitura do contador para dentro do atomic, após trava C/D e releitura. Hoje a guarda e `usadas` são calculados antes da trava: duas chamadas concorrentes podem selecionar a mesma resposta antes de qualquer DP de correção. (`chamados/models.py:2220`, `chamados/models.py:2225`, `chamados/models.py:2232`, `chamados/models.py:2236`; `chamados/juizes.py:1592`)

**A05 — assinatura.** Não reduzir `carimbar` a “não salva”: os ramos `responder` e `validar` hoje completam assinaturas sem DP. O hunk deve registrar DP `RESPOSTA`/`VALIDACAO` no mesmo atomic e no instante `quando`, antes de produzir a consequência, conservando o gate da autoridade e a lavra L. Os testes E4/E9 precisam morder perda desse ato e troca de `quando` pelo relógio da reparação. (`chamados/models.py:3113`, `chamados/models.py:3119`, `chamados/models.py:3123`, `chamados/models.py:3138`)

Não produzi hunk integral de A01–A09/A21–A23 que alegasse fechar o corte: A03 e identidade acima impedem um patch seguro e aplicável isoladamente sem decisão de representação da sessão. [nao verificado]

## 09/11 17:12 · sessao 02db88593fc2 · - · - · origem=Ronald (tmux)

Exploração somente leitura concluída; nenhuma edição.

- Vizinho executado: `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest chamados.tests.test_e3_pendentes_app` → rc 0, `Ran 6 tests`, `OK`. O módulo usa `unittest`, AST e stubs de módulos; o relógio é explícito. (`chamados/tests/test_e3_pendentes_app.py:2`, `:16`, `:24`, `:118`)

- Infra segura para novos selos: reproduzir a extração AST de funções produtivas sem importar Django ou settings. `config/settings/ci.py` cria diretórios temporários fora de `app/` e configura PostgreSQL; portanto sua importação viola as restrições desta volta. (`config/settings/ci.py:6`, `:25`, `:29`, `:33`)

- E1/escritores: o detector existente usa contaminação por ponto fixo para atingir atributos, `setattr`, construtor com alias, dicionários/defaults, uniões e `bulk_update`. Sua varredura inclui `models.py`. Porém não resolve alias de métodos, como `gravar = qs.update`, e declara não alcançar `__dict__`; ampliar para o contrato novo. (`chamados/tests/test_o261e_f2_porta_carimbo.py:109`, `:133`, `:173`, `:192`, `:204`, `:294`)

- Resolução de alias aproveitável: `c17_callers` resolve imports de símbolo/módulo, atribuições locais, escopo lexical e `getattr`. Há contraprovas sobre `models.py` e registro. (`chamados/tests/test_e3_pendentes_app.py:33`, `:64`, `:75`, `:124`, `:149`)

- E9 puro: extrair `_hoje`, `relogio_virou` e `esta_stale`; fornecer timezone com `ZoneInfo('America/Sao_Paulo')` e pergunta `SimpleNamespace`. Casos relevantes matam `<= → <`, `> → >=`, data UTC em lugar da local, lavra ausente e lavra futura. (`chamados/veredito_lavrado.py:102`, `:107`, `:116`, `:213`, `:224`, `:232`)

- E2 existente depende de Django e `TestCase` com banco. Seus casos de referência para bordas são meia-noite local, instante futuro, relógio envenenado e horizonte de decisões. Escrever novos casos puros no arquivo e executar apenas a classe extraída por AST evita inicializar a infraestrutura proibida. (`chamados/tests/test_e2_tarefa_do_dia.py:59`, `:71`, `:640`, `:657`, `:667`, `:714`)

- E17 render real existente depende de banco: a matriz usa views inteiras, `Client`, autenticação e os dois kernels. Não declarar render admin/app certificado com mera inspeção AST. (`chamados/tests/test_e3_app_orm.py:1`, `:12`, `:28`, `:33`, `:38`)

- Conferência: os testes antigos medem apenas timestamp/autor e idempotência; não provam validade para par de dias alterado. O log é explicitamente best effort e pode devolver `None`; não serve como substituto obrigatório do fato. (`chamados/tests/test_data_do_fato_divergente.py:185`, `chamados/tests/test_porta_validacao_pergunta.py:317`, `core/observ.py:98`, `:139`) Falta ato de conferência em `ATOS`. (`chamados/models.py:3846`)

- Padrões existentes adequados à entrega: `Single Writer Principle`, `Architecture Conformance Test`, `Immutable event log + compensating event`; a linha `padrao=` deve refletir o diff realizado. (`docs/GLOSSARIO.md:12`, `:15`, `:25`; `docs/PLANO-PADROES.md:49`, `:53`)

Recomendação para mutantes: executar a função produtiva extraída de fonte com alteração em memória, mostrar a falha e descartar a fonte mutada. Informar explicitamente essa modalidade; os testes de snippets sintéticos provam o detector, mas não substituem os mutantes comportamentais de produção. (`chamados/tests/test_e3_pendentes_app.py:24`, `:149`; equivalência dessa modalidade ao requisito operacional: [nao verificado])

## 09/11 17:07 · sessao 08f5868560fb · FIO-E0-V4-E4-ESCRITORES · leitura · origem=Code

**É (b): leitor a migrar para o fato antes de cessar a escrita legada.** O template consome campos cuja escritora já está no censo; não acrescenta porta de escrita. (`app/templates/chamados/partials/modal_fio.html:169`, `:170`; `app/chamados/services/validacao.py:664`, `:665`; anexo `censo_v2.r:407`)

| Item | Resultado e evidência |
|---|---|
| **1 — Template: instante** | Dentro do ramo de divergência dos dias, `if perg.data_conferida_em` escolhe mostrar “Conferida em”, formatada `d/m/Y H:i`; sem instante, oferece formulário conforme permissão ou informa bloqueio. (`app/templates/chamados/partials/modal_fio.html:162`, `:169`, `:170`, `:171`, `:179`) |
| **1 — Template: autor** | `if perg.data_conferida_por` decide mostrar autoria; `get_full_name\|default:perg.data_conferida_por.username` apresenta nome completo ou username. (`app/templates/chamados/partials/modal_fio.html:170`) |
| **1 — Contexto/apelidos** | O objeto percorre `p → perguntas_por_chamado → ch.perguntas_disputa → perg`; os campos chegam pelo próprio objeto. (`app/chamados/services/fio.py:225`, `:243`, `:253`; `app/templates/chamados/partials/modal_fio.html:157`) |
| **1 — Views/contextos** | Render completo e parcial chamam o mesmo montador e passam `chamados`; o parcial também fornece `pode_conferir_data` pela permissão `gerir_chamados`. A view de conferência delega autor/justificativa ao serviço. (`app/chamados/views.py:889`, `:927`, `:931`, `:1312`; `app/chamados/services/fio.py:446`, `:465`, `:474`) |
| **1 — Python** | Fora da escritora, `_materializar_uma` lê somente `data_conferida_em`: divergência com instante nulo bloqueia materialização. Dentro da escritora, instante não nulo impede repetir a conferência. (`app/chamados/services/materializacao.py:1193`, `:1199`; `app/chamados/services/validacao.py:662`, `:663`) |
| **1 — JS estático** | Não localizei leitor literal desses valores **[nao verificado]**. O formulário encontrado aciona a rota por HTMX, sem transportar os campos de conferência. (`app/templates/chamados/partials/modal_fio.html:172`, `:174`) |
| **1 — Leituras de teste** | Conferem presença/ausência, autoria por `data_conferida_por_id` e preservação do instante no segundo pedido; `em1` é o apelido local. (`app/chamados/tests/test_data_do_fato_divergente.py:191`, `:192`, `:203`, `:211`; `app/chamados/tests/test_porta_validacao_pergunta.py:292`, `:293`, `:306`, `:315`, `:322`, `:327`) |
| **2 — Escritores** | Porta semântica produtiva: `conferir_data_do_fato`, que atribui instante/autor e persiste ambos; `save` é executor. Há escrita direta de preparação nos testes. A FK ainda declara `SET_NULL` quando o usuário é excluído. (`app/chamados/services/validacao.py:664`, `:665`, `:666`; `app/chamados/models.py:3402`, `:3412`, `:2457`; `app/chamados/tests/test_data_do_fato_divergente.py:155`, `:166`) |
| **2 — Ato correspondente** | A porta emite **`LogAuditoria`: ação `conferir_data`, modelo `PerguntaDisputa`, objeto `p.pk`**, com dia anterior, dia válido e justificativa; a trilha recebe usuário e tem `criado_em` próprio. **Nenhuma DP de conferência** consta em `ATOS`. (`app/chamados/services/validacao.py:667`, `:668`, `:669`, `:670`, `:671`, `:672`; `app/core/auditoria.py:22`, `:24`; `app/core/models.py:295`, `:307`; `app/chamados/models.py:3848`) |
| **3 — E4** | **(b), sem aumentar 26 (+27 informado):** migrar a leitura do instante/autoria para o ato correspondente antes de abandonar suas escritas. O candidato factual existente é o log acima; sua gravação admite falha sem impedir o fluxo. Cobertura histórica por pergunta: **[nao verificado]**. Onde não houver registro equivalente, **falta fato**: conferência daquele par de dias, autor, instante e justificativa. (`app/templates/chamados/partials/modal_fio.html:169`, `:170`; `app/chamados/services/validacao.py:664`, `:667`; `app/core/observ.py:98`, `:139`) |

| 4 — Outros leitores de template ausentes do `censo_v2` fornecido | Ocorrências confirmadas |
|---|---|
| Modal: R/V/F, incluindo indiretos | `app/templates/chamados/partials/modal_fio.html:184`, `:216`, `:218`, `:219`, `:227`, `:378`; `aberta`: `:75`, `:325`; `leitura_da_resposta`: `:249`; `todas_respondidas`: `:333`. |
| Card: R/via/estado/F | `app/templates/chamados/partials/_card_atendimento.html:94`, `:100`, `:108`, `:135`, `:137`, `:142`, `:148`, `:155`, `:176`, `:177`, `:384`, `:385`; via `:186`; estado `:183`; aberta `:10`, `:15`; correção `:190`. |
| Card de pergunta: R/V | `app/templates/chamados/partials/_pergunta_card.html:20`, `:23`, `:24`, `:29`, `:31`, `:32`, `:42`, `:43`. |
| Preview: R | `app/templates/chamados/partials/_lote_preview.html:24`. |
| F por contexto/propriedade | `app/templates/chamados/fio_colaborador.html:164`, `:169`; `app/templates/chamados/detalhe_local.html:6`, `:296`. |
| Correspondência dos indiretos | `aberta → fechada_em`: `app/chamados/models.py:1821`; `estado → V/C/R`: `:3030`, `:3032`, `:3034`, `:3037`; filtro → R: `app/chamados/templatetags/chamados_tags.py:128`; flags → R/F: `app/chamados/services/fio.py:340`, `:341`, `:342`; contexto → F: `app/chamados/services/acoes_chamado.py:336`. |

Exaustividade por reflexão, SQL ou nomes montados dinamicamente: **[nao verificado]**.

## 09/11 17:06 · sessao a086c7213e12 · - · - · origem=Ronald (tmux)

| Template | Leitores adicionais ao `censo_v2` fornecido |
|---|---|
| `app/templates/chamados/partials/modal_fio.html` | R: `:184`, `:216`, `:218`, `:219`; V: `:227`; F: `:378`, e por `disputa_obj.aberta`: `:75`, `:325`; R indireto por `leitura_da_resposta`: `:249`; R indireto por `todas_respondidas`: `:333`. |
| `app/templates/chamados/partials/_card_atendimento.html` | R: `:94`, `:100`, `:108`, `:135`, `:137`, `:142`, `:148`, `:155`, `:176`, `:177`, `:384`, `:385`; via: `:186`; V/C/R por `p.estado`: `:183`; F por `disputa_obj.aberta`: `:10`, `:15`; R/F por `pode_solicitar_correcao`: `:190`. |
| `app/templates/chamados/partials/_pergunta_card.html` | R: `:24`, `:29`, `:31`, `:32`; V: `:20`, `:23`, `:42`; R indireto por `leitura_da_resposta`: `:43`. |
| `app/templates/chamados/partials/_lote_preview.html` | R: `:24`. |
| `app/templates/chamados/fio_colaborador.html` | F por `disputa_obj.aberta`: `:164`, `:169`. |
| `app/templates/chamados/detalhe_local.html` | F por contexto `disputa_aberta`: `:6`, `:296`; seleção por `fechada_em__isnull=True` em `app/chamados/services/acoes_chamado.py:336`. |

| Comprovação dos aliases | Fonte |
|---|---|
| `aberta` lê `fechada_em`. | `app/chamados/models.py:1821` |
| `estado` lê V/C/R. | `app/chamados/models.py:3030`, `:3032`, `:3034`, `:3035`, `:3037` |
| `resposta_display` lê `resposta_colab`. | `app/chamados/models.py:3510`, `:3515`, `:3523`, `:3530`, `:3532` |
| `leitura_da_resposta` lê R por `getattr`. | `app/chamados/templatetags/chamados_tags.py:128` |
| `todas_respondidas` deriva de `all(p.respondida_em)`; `pode_solicitar_correcao` combina isso com `d.aberta`. | `app/chamados/services/fio.py:340`, `:341`, `:342` |

Correção da mensagem anterior: não há ocorrência de `pode_solicitar_correcao` no modal; a ocorrência encontrada é apenas `app/templates/chamados/partials/_card_atendimento.html:190`.

## 09/11 17:06 · sessao d2ea68ef7256 · - · - · origem=Ronald (tmux)

| Caminho | Evidência e uso |
|---|---|
| Transporte para template | `montar_chamados_fio` busca objetos `PerguntaDisputa` como `p`; agrupa em `perguntas_por_chamado`, anexa em `ch.perguntas_disputa`; template percorre como `perg`. Logo, `p → ch.perguntas_disputa → perg` é o apelido relevante; não há cópia separada dos dois campos. (`app/chamados/services/fio.py:225`, `:243`, `:253`; `app/templates/chamados/partials/modal_fio.html:157`) |
| Seleção/preload | Perguntas das disputas abertas; nas fechadas aplica `q_vivas` e depois `pergunta_viva`. `select_related` inclui disputa/célula/destino; prefetch inclui decisões com `validada_original_por`/`ato_por`; não inclui `data_conferida_por`. (`app/chamados/services/fio.py:221`, `:224`, `:225`, `:226`, `:227`, `:231`, `:238`) |
| Contexto completo | `_resp_modal_full` chama `montar_chamados_fio(..., com_explicacao=True)` e passa `'chamados': chamados` para `modal_fio.html`. Não calcula outro valor de conferência. (`app/chamados/views.py:889`, `:927`, `:931`) |
| Contexto parcial | `contexto_parcial_fio` chama o mesmo montador, passa `chamados` e define `pode_conferir_data` pela ação `gerir_chamados`. Consumido no re-render pós-ação de `processar_post_fio` e na reabertura de disputa. (`app/chamados/services/fio.py:446`, `:465`, `:474`, `:496`, `:497`; `app/chamados/views.py:1399`, `:1400`) |
| Leitura do par no modal | Dentro do ramo de divergência entre dias, `if perg.data_conferida_em` decide exibir conferência em vez do formulário/bloqueio. Mostra timestamp `d/m/Y H:i`; `if perg.data_conferida_por` controla a autoria; usa `get_full_name|default:...username`. Sem timestamp, oferece formulário se `pode_conferir_data`, senão informa bloqueio. (`app/templates/chamados/partials/modal_fio.html:162`, `:169`, `:170`, `:171`, `:172`, `:179`) |
| Casca HTTP | `conferir_data_pergunta` valida método/permissão e delega ao serviço com autor e justificativa; não lê os campos. (`app/chamados/views.py:1308`, `:1310`, `:1312`, `:1313`) |
| JS | Busca literal por `data_conferida`/`conferir_data` em `app/**/*.js` não encontrou ocorrência; cobertura dinâmica: **[nao verificado]**. O acionamento concreto está no atributo HTMX do template, `hx-post` para `conferir_data_pergunta`, `hx-swap="none"`. (`app/templates/chamados/partials/modal_fio.html:172`) |

Detalhe potencialmente útil: o contexto **completo** não fornece `pode_conferir_data`; o parcial fornece. Portanto a diferença comprovável é ausência/presença dessa chave nos dois construtores, sem inferir resultado produtivo. (`app/chamados/views.py:927`; `app/chamados/services/fio.py:474`; comportamento produtivo: **[nao verificado]**)

## 09/11 16:29 · sessao f6ee62474778 · FIO-E0-V4-E4-ESCRITORES · leitura · origem=Code

**O pré-desenho precisa incluir a troca dos leitores junto dos escritores.** Nesta cópia, a consulta central do app ainda depende de R/V/C/F; a tarefa ainda depende da via atual e consulta R/V para reconhecer passivo sem decisão. Cortar apenas as escritas deixaria consumidores usando projeções abandonadas. (`app/chamados/services/pergunta_no_app.py:48`, `:57`, `:70`; `app/chamados/services/tarefa_do_dia.py:227`, `:346`)

**Alvo declarado: N=0 portas de escrita de estado derivado da pergunta**, conservando decisões imutáveis, efeitos canônicos e L como cache declarado. Isso é alvo do desenho, ainda não resultado certificado. A própria definição de `DecisaoPergunta` declara transitórias as projeções até a E4. (`app/chamados/models.py:3832`; resultado alcançado: **[nao verificado]**)

Não cabe prometer uma única construção Codex: portas e leitores em `models.py`, migrations e eventuais ajustes em `bin/` são proibidos ao construtor. A quantidade de cópias/voltas necessária é **[nao verificado]**. (`bin/codex_pergunta.sh:108`, `:112`, `:113`, `:115`, `:330`)

Para compactar os endereços abaixo:

- `models.py` significa `app/chamados/models.py`.
- Arquivos de serviços sem prefixo significam `app/chamados/services/<arquivo>`.
- `juizes.py` significa `app/chamados/juizes.py`.
- Os demais caminhos aparecem completos.

Os números produtivos abaixo são exclusivamente os do anexo fornecido; sua reprodução por banco ou arquivo local é **[nao verificado]**. A identidade criptográfica independente da cópia também é **[nao verificado]**.

**A. MATRIZ DAS PORTAS**

R = resposta/texto/instante; V = validação/autor; C = materialização/autor/via; F = fechamento/autor/resolução/motivo; L = veredito/julgamento/expiração. Esses grupos não têm significado uniforme de “aconteceu”: isso está discriminado em B. (`models.py:2444`, `:3023`, `:3092`, `:3366`; `app/chamados/veredito_lavrado.py:213`)

As referências **H-R, H-V, H-C, H-F, H-S, H-L** remetem aos leitores discriminados na tabela H. “DIVERGE” significa dependência produtiva existente que a transição precisa retirar; não afirma uma ocorrência produtiva nova além do anexo. (`models.py:3315`; `pergunta_no_app.py:70`; `materializacao.py:1369`)

| Célula / porta em 05e6aa147 | Classificação e escrita atual | Fato substituto já existente | Leitores atuais / E3 | Resultado esperado |
|---|---|---|---|---|
| **A01 — `models.py:201 _gravar_estado`** | Efeito/decisão: `status_local`, `resolvido_em`, `via_encerramento`, `respondido_por`; prioridade/atualização; relavra perguntas. | **Chamado**, transição e trilha do ato. (`models.py:294`) | H-S; fila ainda lê status do chamado. (`painel.py:155`; `tarefa_do_dia.py:363`) | Conservar a porta do efeito chamado; retirar consequências que perpetuem estado derivado da pergunta. Estado canônico do chamado não entra no alvo zero da pergunta. (`models.py:201`; `models.py:3832`) |
| **A02 — `models.py:1852 fechar`** | Efeito no fechamento normal; decisão nas exceções. Grava F e delega C. | Exceção: DP `SILENCIO`. Normal: quitação pelos efeitos; F e trilha registram o fechamento atual. (`models.py:1921`, `:1929`, `:1937`) | H-F/H-C. App ainda particiona por F. **DIVERGE**. (`pergunta_no_app.py:70`) | Fechamento derivado deixa de comandar exigibilidade. Preservar o ato excepcional e a história antes de retirar projeções. (`tarefa_do_dia.py:149`; `models.py:3832`) |
| **A03 — `models.py:2123 reabrir_admin`** | Decisão: limpa F; incrementa contador/agrega motivos. | **Nenhum DP próprio dessa reabertura do continente**; há gesto, contador e trilha. (`models.py:2135`, `:2146`, `:2158`) | H-F. **DIVERGE**: DP `REABERTURA` da resposta não representa automaticamente esse gesto. (`disputa_emissao.py:1612`) | Preservar decisão, autor, limite e motivo; retirar F como cópia de exigibilidade somente com substituição factual do gesto. (`models.py:2123`; `app/docs/LEIS.md:65`) |
| **A04 — `models.py:2160 reabrir_sistema`** | Efeito: limpa F. | Pergunta voltou a viver; nota de sistema existente. (`models.py:2170`, `:2177`) | H-F. **DIVERGE**. (`models.py:1820`; `pergunta_no_app.py:70`) | Exigibilidade vem da tarefa; não depender de “reabrir F” para tornar ação alcançável. (`tarefa_do_dia.py:442`) |
| **A05 — `models.py:3092 carimbar`** | Efeito no carimbo; decisão nos ramos de assinatura. C; completa R/V; pode gravar L. | Batida/Ausência/chamado/lavra conforme ramo. Assinaturas R/V desta porta não ganham DP automaticamente. (`models.py:3119`, `:3123`, `:3140`) | H-R/H-V/H-C/H-L. **DIVERGE**. (`models.py:3023`; `juizes.py:1323`) | C deixa de ser escrito; assinaturas passam pelo fato de decisão; L permanece explicitamente cache. (`models.py:3832`; `app/chamados/veredito_lavrado.py:254`) |
| **A06 — `models.py:3157 carimbar_identidade`** | Efeito; decisão humana depende do chamador. C e recusa opcional. | Retratação de motivo tem DP; duplicata e outras vias não possuem representação factual uniforme equivalente. (`models.py:2053`; `tarefa_do_dia.py:220`) | H-C; **a própria tarefa lê via atual**. **DIVERGE de identidade**. (`tarefa_do_dia.py:227`; `models.py:2743`) | Não abandonar via enquanto dedup, índice e tarefa dependem dela. Factualizar terminalidade antes de substituir esse discriminador. (`models.py:3474`, `:3480`; `tarefa_do_dia.py:216`) |
| **A07 — `models.py:3192 descarimbar`** | Efeito: limpa C; também preparação em memória. | Fato/decisão que invalida carimbo, conforme chamador. A ausência de carimbo não é o fato. (`models.py:3192`; `materializacao.py:639`) | H-C. **DIVERGE**. (`models.py:3315`; `relavra_via.py:181`) | Retirar a escrita da projeção junto dos consumidores; manter a consequência factual e invalidação de L necessária. (`models.py:3832`; `app/chamados/veredito_lavrado.py:213`) |
| **A08 — `models.py:3241 descarimbar_em_lote`** | Efeito: limpa C e grava campos recebidos em `outros`, inclusive R/V. | DP da correção/recusa quando chamada por esse fluxo. (`disputa_emissao.py:1612`) | H-R/H-V/H-C. **DIVERGE**. (`models.py:3253`; `materializacao.py:1369`) | Nenhum campo derivado pode continuar entrando por `outros`; preservar atos e execução em lote. (`models.py:3241`; `models.py:3832`) |
| **A09 — `models.py:3257 encerrar_por_fato_no_espelho`** | Efeito: `via_resolucao=fato_materializado`. | Fato do marco no espelho/lavra. (`models.py:3258`; `juizes.py:645`) | H-C. **DIVERGE** nos consumidores de via. (`explicador.py:68`; `models.py:3182`) | Não escrever via derivada; ler confirmação do efeito pela autoridade. (`tarefa_do_dia.py:140`) |
| **A10 — `validacao.py:127 _carimbar_validacao`** | Decisão: V + DP `VALIDACAO`. | DP no mesmo instante da validação. (`validacao.py:134`, `:139`) | H-V. **DIVERGE**; tarefa já interpreta DP, mas outros consumidores ainda usam V. (`tarefa_do_dia.py:140`; `juizes.py:1323`) | V projetado deixa de ser escrito; o ato permanece. (`models.py:3832`) |
| **A11 — `validacao.py:163 devolver_validacao_recusada`** | Decisão: limpa V, preserva resposta; DP `DEVOLUCAO`. | DP `DEVOLUCAO`. (`validacao.py:205`) | H-V/H-R. **DIVERGE**. Tarefa já lê o ato. (`tarefa_do_dia.py:138`) | Nenhuma limpeza de V projetado; atos anteriores continuam imutáveis. (`models.py:3818`; `models.py:3832`) |
| **A12 — `disputa_emissao.py:1422 responder_pergunta_disputa_core`** | Decisão: R, invalida V/C, pode escrever recusa explicativa. | DP `RESPOSTA`, inclusive texto/escape. (`disputa_emissao.py:1494`, `:1508`) | H-R/H-V/H-C. **DIVERGE**. (`pergunta_no_app.py:70`; `materializacao.py:1370`) | Conteúdo e instante vêm da sequência; eliminar manutenção de R/V/C projetados. (`models.py:3832`) |
| **A13 — `disputa_emissao.py:1559 reabrir_para_correcao`** | Decisão: limpa R/V e opcionalmente C; **não escreve F**. | DP `REABERTURA` ou `RECUSA`, com foto anterior. (`disputa_emissao.py:1612`, `:1625`) | H-R/H-V/H-C. **DIVERGE**. (`models.py:3903`; `models.py:3315`) | Registrar ato seguinte sem apagar resposta anterior; consumidores deixam de depender da limpeza. (`models.py:3811`; `models.py:3832`) |
| **A14 — `disputa_emissao.py:1635 responder_automaticamente`** | Decisão de sistema: R sem sobrescrever texto existente. | DP `RESPOSTA`. (`disputa_emissao.py:1653`) | H-R; a própria guarda usa texto projetado. **DIVERGE**. (`disputa_emissao.py:1644`) | Preservar voz existente e idempotência pela sequência, sem escrever R. (`models.py:3832`) |
| **A15 — `disputa_emissao.py:1658 carimbar_resposta_pela_trilha`** | Decisão reconstruída: `respondida_em`. | Testemunha da resposta + DP `RESPOSTA`. (`disputa_emissao.py:1674`) | H-R; guarda ainda usa projeção. **DIVERGE**. (`disputa_emissao.py:1667`) | Conservar instante testemunhado; não substituí-lo pelo instante do reparo. (`disputa_emissao.py:1658`) |
| **A16 — `relavra_via.py:106 relavrar`** | Efeito: via derivada + L. | Autoridade do efeito e lavra. (`relavra_via.py:95`, `:118`) | H-C/H-L. **DIVERGE** na via. (`relavra_via.py:118`; `explicador.py:68`) | Encolher para manutenção declarada de L; retirar derivação de via sem consumidores sobreviventes. (`app/chamados/veredito_lavrado.py:254`; `app/docs/LEIS.md:137`) |
| **A17 — `materializacao.py:784 _materializar_resposta_hora`** | Efeito: `pendencia_admin`, `recusa_motivo`; plantio/carimbos delegados. | Sucesso: Batida. Pendência técnica: **nenhum DP próprio**. (`materializacao.py:850`, `:859`) | H-P/H-REC, além de R/C. Template também lê pendência. **DIVERGE**. (`explicador.py:85`; `app/templates/chamados/partials/modal_fio.html:221`) | Pendência deixa de ser estado paralelo; apresentar resultado técnico pela autoridade real, preservando trilha do insucesso. Não convertê-lo em recusa humana. (`app/docs/LEIS.md:60`, `:65`; `materializacao.py:1107`) |
| **A18 — `materializacao.py:1083 _carimbar_recusa`** | Efeito técnico: recusa; colisão delega C de identidade. | Colisão/resultado técnico + nota `recusa_materializacao`; **nenhum DP `RECUSA` humano**. (`materializacao.py:1099`, `:1107`) | H-REC/H-C; própria idempotência lê recusa/C. **DIVERGE**. (`materializacao.py:1089`, `:1092`) | Não usar string ou C como prova de decisão; conservar motivo técnico sem criar ato humano inexistente. (`models.py:3849`; `materializacao.py:1107`) |
| **A19 — `validacao.py:533 recusar_pergunta`** | Decisão: recusa, limpeza R/V delegada. | DP `RECUSA`. (`validacao.py:584`, `:593`; `disputa_emissao.py:1612`) | H-REC/H-R/H-V; guarda usa R. **DIVERGE**. (`validacao.py:576`) | Motivo e foto vêm do ato; nenhuma manutenção dos campos derivados. (`models.py:3840`) |
| **A20 — `validacao.py:625 conferir_data_do_fato`** | Decisão: `data_conferida_em/por`. | **Outro fato existente:** `LogAuditoria` com os dois dias e justificativa. Não há ato DP de conferência. (`validacao.py:664`, `:667`; `models.py:3846`) | H-DATA. Materializador exige timestamp; modal exibe instante **e autor**. **DIVERGE**. (`materializacao.py:1193`; `app/templates/chamados/partials/modal_fio.html:170`) | Substituir o sinalizador por leitura inequívoca do ato de conferência; preservar o par de dias, autor e justificativa. (`validacao.py:667`; `app/docs/LEIS.md:65`) |
| **A21 — `models.py:1968 remover_motivo`** | Decisão; motivos/contexto próprios; C delegado. | DP `RETRATACAO`. (`models.py:2053`) | Tarefa lê DP e ainda via; C6 lê via/C. **DIVERGE**. (`tarefa_do_dia.py:227`, `:230`; `juizes.py:1334`) | Entra apesar do “fica” parcial: retirar a dependência da consequência C, preservando retratação e metadados. (`models.py:2053`, `:2058`) |
| **A22 — `models.py:2822 _nascer`** | Nascimento/reuso: identidade, defaults projetados, costura e L. | Identidade factual e decisões já existentes no reuso. (`models.py:2822`, `:2997`) | H-R/H-V/H-C; identidade continua usando via. **DIVERGE** se defaults/reuso mantiverem campos abolidos. (`models.py:2743`, `:3474`) | Entra por obrigação do diff: nascimento e reuso não podem reintroduzir projeções retiradas. Identidade e L permanecem. (`models.py:3832`) |
| **A23 — `models.py:2179 reabrir_por_colab`** | Contador próprio; decisão delegada. | DP da correção + contador factual do pedido. (`models.py:2234`, `:2236`) | Gate lê F; idempotência atual depende de R ser limpo. **DIVERGE**. (`models.py:2201`, `:2221`) | Preservar teto/contador; migrar gate e idempotência para atos, sem repetir incremento na segunda chamada. (`models.py:2223`, `:2236`) |
| **A24 — `models.py:2247 limpar_resolucao_residual`** | Limpeza de `resolucao` em aberta. | Trilha com texto anterior. (`models.py:2259`, `:2266`) | H-F. **DIVERGE** se F/resolução forem retirados. (`models.py:2259`) | Entra por obrigação do diff; depois da conservação histórica, perde trabalho próprio e deve sair com seus consumidores. (`app/docs/LEIS.md:137`) |

As **20 portas centrais de alteração** conservam os endereços apresentados no censo; os seis nascimentos continuam em `models.py:833`, `:1054`, `:2822` e `disputa_emissao.py:474`, `:1024`, `:1299`. Isso não comprova identidade de commits sem metadados Git. (`models.py:201`, `:1852`, `:2123`, `:2160`, `:3092`, `:3157`, `:3192`, `:3241`, `:3257`; `validacao.py:127`, `:163`, `:533`, `:625`; `disputa_emissao.py:1422`, `:1559`, `:1635`, `:1658`; `relavra_via.py:106`; `materializacao.py:784`, `:1083`; identidade criptográfica: **[nao verificado]**) [nao verificado]

Duas referências do censo são **ocorrências de leitura**, não início de função: `cobranca.py:229` fica dentro de `_calados`, definido em `:221`; `fabrica_da_celula.py:128` fica dentro de `motivos_a_perguntar`, definido em `:74`. Não são evidência de deslocamento entre as árvores. (`cobranca.py:221`, `:229`; `fabrica_da_celula.py:74`, `:128`)

As referências antigas do aval a badge em `models.py:1729/1736` não apontam o leitor atual: há lápide em `models.py:1795`; os consumidores atuais chamam a consulta central em `app/api/views.py:325` e `app/api/views_core.py:314`. (`models.py:1729`, `:1736`, `:1795`; `app/api/views.py:325`; `app/api/views_core.py:314`)

**Conta:** o “16” é o inventário inicial de alterações; o censo corrigido conta **16 + 4 alterações adicionais = 20**, e depois **20 + 6 nascimentos = 26**. Não são o mesmo universo. As quatro adicionais são A17–A20; executores `save`/`registrar` não devem ser somados novamente às portas que executam. (`materializacao.py:784`, `:1083`; `validacao.py:533`, `:625`; `models.py:3402`, `:3932`; número histórico do censo anexado: **[nao verificado]**)

**RESULTADO 11, com unidade explícita: 16 → N=0 portas de escrita de estado derivado da pergunta; lista residual: vazia.** No censo corrigido, a comparação correspondente é **20 → 0**, fiscalizando também os nascimentos. Separadamente permanecem o executor factual `DecisaoPergunta.registrar`, as portas dos efeitos e os escritores declarados de L. Não seria comparável anunciar “16→4” somando três famílias de efeitos a um executor de decisões. (`models.py:3832`, `:3932`; `app/chamados/veredito_lavrado.py:240`, `:254`; realização desse alvo: **[nao verificado]**)

**B. CAMPO A CAMPO — ESMERIL-SEMÂNTICA**

| Célula | Significado e divergência comprovável | Onde morde A |
|---|---|---|
| **B01 — R: presença** | `respondida` testa instante não nulo; SQL combina instante/texto; C6 usa texto com `strip`; tarefa usa última DP e trata projeção sem linha como desconhecimento. São critérios distintos. (`models.py:3486`, `:3337`; `juizes.py:1339`; `tarefa_do_dia.py:127`, `:132`) | A05, A08, A12–A15, A22–A23. |
| **B02 — R: conteúdo** | Resposta pode ser hora, data/hora, escolha, identificador ou escape. Intervalo pode plantar pela hora do marco. Não transformar qualquer texto em instante. (`models.py:3508`; `materializacao.py:356`, `:400`, `:978`) | A05, A12–A15, A17. |
| **B03 — V** | V é assinatura, não confirmação de efeito. C6 encerra por V não nulo; tarefa com DP `VALIDACAO` exige L vigente e distingue materializar, rejulgar e encerrar. Ramo de carimbo também preenche V. (`juizes.py:1323`; `tarefa_do_dia.py:140`; `models.py:3123`) | A05, A10–A13. |
| **B04 — C** | `materializada_em` não significa uniformemente “Batida criada”: identidade/colisão também carimbam; identidade pode conservar instante e autor antigos. Nulo pode resultar de limpeza posterior. (`models.py:3157`, `:3171`, `:3192`; `materializacao.py:1099`) | A05–A09, A16–A18. |
| **B05 — via/identidade** | Via mistura resultado derivado com exclusão da identidade do fato. Retratação tem DP datada; duplicata não tem equivalente datado na tarefa atual. Índices e seleção ainda dependem da via. (`tarefa_do_dia.py:216`, `:220`, `:227`; `models.py:3474`, `:3480`) | A06–A08, A16, A21–A22. |
| **B06 — F** | F significa fechamento do continente, não inexistência de tarefa. App manda viva em fechada ao histórico; pedido de correção mantém gate de fechada. Reabertura da resposta não é reabertura do continente. (`pergunta_no_app.py:70`; `models.py:2221`; `disputa_emissao.py:1559`) | A02–A04, A13, A23–A24. |
| **B07 — pendência** | Pendência técnica precede o rótulo: resposta pode parecer validada/resolvida e ainda exigir admin. Explicador e templates dependem disso. (`materializacao.py:850`; `explicador.py:85`; `app/templates/chamados/partials/modal_fio.html:221`; `app/templates/chamados/partials/_card_atendimento.html:183`) | A17; saída dos leitores de A05/A10. |
| **B08 — recusa** | Humana gera DP `RECUSA`; técnica grava campo/nota; advertência de escape acompanha DP `RESPOSTA`. Não reconstruir recusa humana de qualquer string nesse campo. (`validacao.py:593`; `materializacao.py:1103`, `:1107`; `disputa_emissao.py:1494`, `:1508`) | A06, A12–A13, A17–A19. |
| **B09 — conferência** | `data_conferida_em/por` representa autorização humana diante de dois dias divergentes. Materializador usa apenas presença do timestamp; trilha guarda o par/justificativa; modal usa também autor. (`materializacao.py:1193`; `validacao.py:667`; `app/templates/chamados/partials/modal_fio.html:170`) | A20. Validade da conferência após alteração do par de dias: **[nao verificado]**. |
| **B10 — dia versus instante** | `data_evento` é dia do turno no fato e dia civil na conversa. Plantio combina dia/hora e ajusta overnight pelo envelope inclusivo. O instante da Batida não é esse dia. (`models.py:2791`; `juizes.py:296`, `:332`; `materializacao.py:413`) | A17, A20, A22; identidade de A06/A21. |
| **B11 — L e bordas** | `julgado_em` é instante; expiração é dia local. Expira por `<=`; marco posterior invalida por `>`. Ausente/indeterminado não equivale a “não materializado”. C6 ainda usa janela inclusiva e relógio que `--agora` não governa integralmente. (`app/chamados/veredito_lavrado.py:116`, `:224`, `:232`; `juizes.py:575`, `:601`; `app/chamados/management/commands/diff_e3_sombra.py:105`) | A05/A16; medidas C e leitores H-L. Divergência produtiva efetiva nessa borda: **[nao verificado]**. |
| **B12 — chamado** | `VIVOS`, `COBRANCA`, `ENCERRADOS`, `TERMINAIS` e `NAO_FECHADOS` respondem perguntas diferentes. “Tem tarefa” não substitui globalmente esses conjuntos. (`app/chamados/catalogo/motor.py:24`, `:29`, `:30`, `:31`, `:32`) | A01 e nascimentos do chamado; classificação C. |
| **B13 — dois silêncios** | Véu temporário vigora com `silenciado_ate > agora`, final exclusivo. DP `SILENCIO` é encerramento excepcional da pergunta. Não são o mesmo ato. (`models.py:511`, `:1921`; `arquivo.py:154`) | A02; filtros de fila usados por A01 e pela tarefa. |
| **B14 — instante histórico** | Reconstrução usa instante da testemunha; reparação ocorre depois. Ordenação das decisões é por `ato_em, pk`; carga considera atos `<= agora`. (`disputa_emissao.py:1658`; `models.py:3878`; `tarefa_do_dia.py:338`) | A15; backfill C3. |

Logo, **não**, escritores e leitores não supõem todos o mesmo significado dos campos. A retirada precisa conservar essas distinções na leitura factual, sem uma propriedade local voltar a ser segundo juiz. (`app/docs/LEIS.md:60`; evidências B01–B14)

**C. O FIO MORTO E O PASSIVO**

**C1 — contadores e universos**

| Contador | Definição efetiva |
|---|---|
| `fio_morto` | Células acusadoras alcançadas por chamados vivos de `FURO_MODULES

[truncado em 20000 de 59200 caracteres]

## 09/11 16:22 · sessao 1b6b5f41c939 · - · - · origem=Ronald (tmux)

Achado que impede retirar escritores isoladamente: o app ainda lê `R/V/C` por `q_vivas` e C6 e depois particiona por `R/F`; a tarefa lê DP, mas ainda depende de via atual, L, da projeção R/V para detectar passivo e de status do chamado pela fila. Isso é DIVERGE estrutural para qualquer célula de A que deixe esses campos sem manutenção antes da migração dos leitores. (`app/chamados/services/pergunta_no_app.py:48`; `app/chamados/services/pergunta_no_app.py:57`; `app/chamados/services/pergunta_no_app.py:70`; `app/chamados/services/tarefa_do_dia.py:227`; `app/chamados/services/tarefa_do_dia.py:346`; `app/chamados/services/tarefa_do_dia.py:351`; `app/chamados/services/tarefa_do_dia.py:365`)

**B — distinções semânticas verificadas**

| ID | Campo e significado | Onde diverge e célula afetada |
|---|---|---|
| B1 | `respondida_em`: instante do ato, reconstruível pela testemunha; `respondida` significa só não nulo. (`app/chamados/services/disputa_emissao.py:1658`; `app/chamados/models.py:3486`) | SQL exige instante + texto não vazio; C6 normaliza texto com `strip`; tarefa usa a última DP RESPOSTA e `texto_vazio`, e passivo sem linha produz desconhecimento. Timestamp sozinho, texto sozinho e ato não são equivalentes. A: responder/reabrir/carimbar/resposta automática/reconstrução. (`app/chamados/models.py:3337`; `app/chamados/juizes.py:1339`; `app/chamados/services/tarefa_do_dia.py:127`; `app/chamados/services/tarefa_do_dia.py:131`) |
| B2 | `resposta_colab`: conteúdo discriminado por `tipo_resposta`, opções e escapes; não representa sempre hora. (`app/chamados/models.py:3508`; `app/chamados/services/materializacao.py:356`; `app/chamados/services/materializacao.py:978`) | Plantio pode usar marco para intervalo; não converter todo texto em instante. Reabertura limpa texto e R/V juntos; limpar só R devolve resposta antiga por leitores de texto. A: responder/reabrir/materializar. (`app/chamados/services/materializacao.py:400`; `app/chamados/services/disputa_emissao.py:1564`; `app/chamados/services/disputa_emissao.py:1590`) |
| B3 | `validada_em/por`: assinatura da validação atual, não prova de efeito. Escritor humano carimba agora e registra DP no mesmo instante. (`app/chamados/services/validacao.py:134`; `app/chamados/services/validacao.py:139`) | C6 mata por V não nulo antes de conferir efeito; tarefa com DP VALIDACAO exige L vigente e distingue materializar/rejulgar/encerrada. Autor do V pode vir do ramo `carimbar(resposta='validar')`, portanto V não prova decisão humana independente. A: validar/devolver/carimbar. (`app/chamados/juizes.py:1323`; `app/chamados/services/tarefa_do_dia.py:140`; `app/chamados/models.py:3123`) |
| B4 | C = `materializada_em/por` e `via_resolucao`: carimbo de processamento, identidade ou encerramento; não é uniformemente Batida criada. (`app/chamados/models.py:3063`; `app/chamados/models.py:3157`) | Help text promete batida criada + chamado fechado, mas carimbo de identidade pode preservar instante/autor antigo; recusa COLISAO carimba sem plantar. A: carimbar/carimbar_identidade/_carimbar_recusa/relavrar. (`app/chamados/models.py:3384`; `app/chamados/models.py:3171`; `app/chamados/services/materializacao.py:1093`; `app/chamados/services/materializacao.py:1099`) |
| B5 | `via_resolucao` também delimita identidade do fato; duplicata/retratada são terminais. (`app/chamados/models.py:3182`; `app/chamados/models.py:3474`; `app/chamados/models.py:3480`) | Não pode simplesmente parar de escrevê-la: consulta SQL, índices parciais e tarefa ainda excluem por ela; DP RETRATACAO existe, mas duplicata não tem ato datado equivalente na tarefa atual. A: carimbar_identidade/remover_motivo/nascimento/reapontar. (`app/chamados/models.py:3341`; `app/chamados/services/tarefa_do_dia.py:220`; `app/chamados/services/tarefa_do_dia.py:227`) |
| B6 | F = fechamento do continente; `aberta` é `fechada_em is None`; fechamento excepcional registra DP SILENCIO por pergunta atingida. (`app/chamados/models.py:1820`; `app/chamados/models.py:1921`; `app/chamados/models.py:1929`) | App põe viva em disputa fechada no histórico; POST continua guardado. Reabertura administrativa/sistema limpa F sem ato DP próprio. Portanto DP atual não substitui integralmente F/contador/gesto do continente. A: fechar/reabrir_admin/reabrir_sistema. (`app/chamados/services/pergunta_no_app.py:70`; `app/chamados/models.py:2146`; `app/chamados/models.py:2170`; `app/chamados/models.py:2221`) |
| B7 | `pendencia_admin` e `recusa_motivo`: pendência técnica pode manter estado aparentemente encerrado; par é escrito junto. (`app/chamados/services/materializacao.py:848`; `app/chamados/services/materializacao.py:850`) | Explicador precisa ler pendência antes da prova do efeito para não esconder ação do admin. Não existe DP específica para essa pendência no universo ATOS atual. A: _materializar_resposta_hora. (`app/chamados/services/explicador.py:82`; `app/chamados/services/explicador.py:85`; `app/chamados/models.py:3846`) |
| B8 | `recusa_motivo` mistura recusa humana, técnica e aviso sobre escape. Humana gera DP RECUSA; técnica escreve campo/followup; escape gera DP RESPOSTA. (`app/chamados/services/validacao.py:584`; `app/chamados/services/validacao.py:593`; `app/chamados/services/materializacao.py:1103`; `app/chamados/services/materializacao.py:1107`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1508`) | Não reconstruir DP RECUSA de qualquer string de recusa: produziria uma decisão humana inexistente. A: recusar/_carimbar_recusa/responder/materializar. (`app/chamados/models.py:3849`; `app/chamados/services/tarefa_do_dia.py:135`) |
| B9 | `data_conferida_em/por`: ato humano que olhou dois dias divergentes e justificou qual vale; não significa simples coincidência das datas. (`app/chamados/services/validacao.py:641`; `app/chamados/services/validacao.py:664`; `app/chamados/services/validacao.py:667`) | Materializador lê somente timestamp como autorização; DP não contém ato de conferência atual, porém LogAuditoria recebe dois dias/justificativa. Fato substituto já existente = outro, trilha de conferência, cuja disponibilidade produtiva concreta não foi medida. A: conferir_data_do_fato. (`app/chamados/services/materializacao.py:1193`; `app/chamados/services/validacao.py:668`; `app/chamados/models.py:3846`) |
| B10 | `data_evento`: dia do turno para fato, dia civil de nascimento para conversa; não é timestamp da Batida. (`app/chamados/models.py:2780`; `app/chamados/models.py:2791`; `app/chamados/models.py:2798`) | Plantio combina dia/hora e ajusta overnight por cadeia/envelope inclusivo; não preencher default com dia civil nem escolher data do chamado em silêncio. A: nascimento/materialização/conferência. (`app/chamados/services/materializacao.py:413`; `app/chamados/juizes.py:296`; `app/chamados/services/materializacao.py:1193`) |
| B11 | L: `julgado_em` é instante; `janela_expira_em` é dia local; `sim/nao/indeterminado/ausente` não são dois estados booleanos. (`app/chamados/veredito_lavrado.py:107`; `app/chamados/veredito_lavrado.py:213`; `app/chamados/veredito_lavrado.py:257`) | Expiração `<= dia local`; marco posterior invalida por `>`; C6 janela é inclusiva e usa `date.today()`. `--agora` não governa C6. A: relavrar/carimbar e leitores L; C: medida em sombra. (`app/chamados/veredito_lavrado.py:116`; `app/chamados/veredito_lavrado.py:232`; `app/chamados/juizes.py:575`; `app/chamados/juizes.py:601`; `app/chamados/management/commands/diff_e3_sombra.py:105`) |
| B12 | `status_local`: VIVOS, COBRANCA, ENCERRADOS, TERMINAIS e NAO_FECHADOS são conjuntos distintos. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/chamados/catalogo/motor.py:30`; `app/chamados/catalogo/motor.py:31`; `app/chamados/catalogo/motor.py:32`) | `registrado` vive mas não cobra; `resolvido` pode ser não fechado. Não trocar qualquer teste de status por “tem tarefa”. Chamado é efeito permitido pelo aval; `status_local` dele não pode ser eliminado globalmente como se fosse só quarta cópia da pergunta. A: _gravar_estado e nascimentos. (`app/chamados/catalogo/motor.py:26`; `app/chamados/models.py:201`; `app/chamados/models.py:833`) |

Nulo não tem significado único transversal: em R/V/C é ausência de carimbo atual (inclusive porque foi limpo), em L pode ser desconhecimento/nunca lavrado, em `data_conferida_em` é ausência da autorização de conferência; não equiparar esses nulos. (`app/chamados/services/disputa_emissao.py:1590`; `app/chamados/models.py:3197`; `app/chamados/veredito_lavrado.py:224`; `app/chamados/services/materializacao.py:1193`)

**H — escritores e leitores, agrupamento para incorporar à tabela única**

Os leitores abaixo são referências produtivas sintáticas conferidas; são endereços de função quando agrupados. ORM, `getattr`, templates e operações por strings também precisam entrar no selo; só AST de `Attribute Load` não demonstra exaustividade e consumidores dinâmicos permanecem [nao verificado]. (`app/chamados/services/explicador.py:85`; `app/chamados/models.py:3337`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:109`)

| Campo | Escritores semânticos | Leitores produtivos a migrar/conservar |
|---|---|---|
| `resposta_colab/respondida_em` | `app/chamados/services/disputa_emissao.py:1422`, `:1559`, `:1635`, `:1658`; ramo R de `app/chamados/models.py:3092`; defaults/reuso `app/chamados/models.py:2822` | `app/chamados/models.py:1823`, `:1852`, `:3023`, `:3092`, `:3315`, `:3486`, `:3508`, `:3903`; `app/chamados/juizes.py:339`, `:1274`, `:1322`, `:1444`, `:1590`, `:1597`, `:1613`, `:1765`, `:1871`, `:1892`, `:1915`; `app/chamados/services/pergunta_no_app.py:60`; `app/chamados/services/cobranca.py:229`; `app/chamados/services/materializacao.py:244`, `:330`, `:425`, `:462`, `:496`, `:639`, `:784`, `:978`, `:1370`; `app/chamados/services/validacao.py:127`, `:163`, `:234`, `:373`, `:533`, `:845`, `:912`; `app/chamados/services/declaracao_texto.py:306`, `:641`; `app/chamados/services/disputa_emissao.py:1635`, `:1658`, `:1798`; `app/chamados/services/explicador.py:25`, `:68`; `app/chamados/services/fio.py:163`, `:485`, `:1216`, `:1227`; `app/chamados/services/acoes_disputa.py:118`, `:302`; `app/chamados/services/becos.py:25`; `app/chamados/services/historico.py:69`; `app/chamados/reconciliador.py:340`, `:1060`; `app/chamados/views.py:1228`; `app/chamados/views_cobrar.py:145`, `:275`; `app/api/views.py:991`, `:2549`, `:2572`, `:2607`; `app/api/views_mensageria.py:554`; `app/ponto/supra_juiz.py:539`; `app/ponto/services/cartorio.py:1597`; `app/ponto/services/veredito_celula.py:160`; `app/ponto/services/dia_do_colab.py:6`; `app/folha/previa.py:139`; `app/colaboradores/services/ficha.py:99`; diagnóstico `app/chamados/services/vigia_fio.py:542` |
| `validada_em/por` | `app/chamados/services/validacao.py:127`, `:163`; limpeza por `app/chamados/services/disputa_emissao.py:1422`, `:1559`; ramo V `app/chamados/models.py:3092`; defaults `app/chamados/models.py:2822` | `app/chamados/models.py:1823`, `:3023`, `:3092`, `:3315`, `:3903`; `app/chamados/juizes.py:1322`; `app/chamados/services/materializacao.py:639`, `:1371`; `app/chamados/services/validacao.py:127`, `:144`, `:163`, `:373`, `:533`, `:695`; `app/chamados/services/declaracao_texto.py:216`; `app/chamados/services/verbos.py:218`; `app/chamados/services/painel.py:268`; `app/chamados/services/disputa_emissao.py:1422`, `:1798`; `app/chamados/services/acoes_disputa.py:302`; `app/ponto/services/procedencia_batida.py:54`; consultas `app/ponto/services/flip_auto.py:130`, `app/chamados/services/vigia_carimbo.py:98`, `app/chamados/reconciliador.py:630`, `:1049` |
| `materializada_em/por` | `app/chamados/models.py:3092`, `:3157`, `:3192`, `:3241`; atribuição executora em `:3086`; restauração em `:3148` | `app/chamados/models.py:1823`, `:1852`, `:3023`, `:3039`, `:3092`, `:3157`, `:3268`, `:3278`, `:3290`, `:3315`; `app/chamados/juizes.py:282`, `:645`, `:1322`, `:1411`, `:1915`; `app/chamados/services/materializacao.py:425`, `:496`, `:603`, `:639`, `:1083`, `:1372`, `:1419`; `app/chamados/services/validacao.py:163`, `:373`; `app/chamados/services/disputa_emissao.py:1798`; `app/chamados/services/raiox.py:153`; `app/chamados/services/relavra_via.py:187`; `app/chamados/services/painel.py:67`; `app/chamados/reconciliador.py:340`, `:1112`, `:1049`, `:1500`; `app/chamados/views.py:1228`; `app/ponto/services/veredito_celula.py:160`; `app/ponto/services/procedencia_batida.py:54`; `app/chamados/services/fabrica_da_celula.py:148` |
| `via_resolucao` | C acima; adicional `app/chamados/models.py:3257`; `app/chamados/services/relavra_via.py:106` | C acima mais identidade: `app/chamados/models.py:2694`, `:2743`, `:3182`, `:3474`, `:3480`; `app/chamados/services/tarefa_do_dia.py:163`; `app/chamados/services/fio.py:1075`, `:1110`, `:1216`, `:1227`, `:1336`, `:1413`, `:1831`, `:1869`, `:1896`, `:1914`; `app/chamados/services/passivo_validado.py:131`, `:474`; `app/chamados/services/explicador.py:68`; `app/chamados/services/fabrica_da_celula.py:128`; `app/ponto/services/cartorio.py:1610`; `app/ponto/services/flip_auto.py:129` |
| `fechada_em/por/resolucao/motivo_fechamento` | `app/chamados/models.py:1852`, `:2123`, `:2160`; residual `:2247`; defaults em `app/chamados/services/disputa_emissao.py:474`, `:1024`, `:1299` | `app/chamados/models.py:1820`, `:1852`, `:1968`, `:2123`, `:2160`, `:2179`, `:2247`, `:2626`; `app/chamados/services/pergunta_no_app.py:70`; `app/chamados/juizes.py:2306`; `app/chamados/services/materializacao.py:496`, `:1302`; `app/chamados/services/validacao.py:144`, `:163`; `app/chamados/services/disputa_emissao.py:34`, `:474`, `:942`, `:1024`, `:1299`, `:1798`; `app/chamados/services/becos.py:25`, `:36`; `app/chamados/services/acoes_disputa.py:302`; `app/chamados/services/fio.py:485`; `app/chamados/services/relavra_via.py:187`; `app/chamados/signals.py:19`, `:121`, `:148`; `app/api/views.py:991`, `:2693`; consultas adicionais: `app/colaboradores/painel_cell.py:32`, `app/colaboradores/signals.py:73`, `app/escala/signals.py:157`, `app/api/foto_recebida.py:62`, `app/colaboradores/services/situacional.py:117`, `app/colaboradores/services/drawer.py:70`, `app/chamados/services/fila_por_causa.py:225`, `app/chamados/services/regularizacao_ext.py:59` |
| `pendencia_admin` | `app/chamados/services/materializacao.py:850` | `app/chamados/services/explicador.py:85`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1112` |
| `recusa_motivo` | `app/chamados/services/materializacao.py:850`, `:1102`; `app/chamados/services/validacao.py:584`; `app/chamados/services/disputa_emissao.py:1494`; opcional executor `app/chamados/models.py:3177` | `app/chamados/services/materializacao.py:1092`; `app/chamados/services/validacao.py:144`, `:163`, `:533`; `app/chamados/services/explicador.py:90`; `app/ponto/services/flip_auto.py:132`; leituras por strings `app/chamados/services/acoes_disputa.py:81`, `app/chamados/services/fio.py:518`, `:1351`; auditoria `app/chamados/management/commands/auditar_invariantes_chamados.py:536`, `:1075`, `:1112` |
| `data_conferida_em/por` | `app/chamados/services/validacao.py:664` | em: `app/chamados/services/validacao.py:662`, `app/chamados/services/materializacao.py:1193`; por: consumidor adicional [nao verificado] (`app/chamados/services/validacao.py:665` é escrita) |
| `veredito_lavrado/julgado_em/janela_expira_em` | `app/chamados/veredito_lavrado.py:240`, `:254`; composição de save `app/chamados/models.py:3140`, reuso `:2997`, `app/chamados/services/relavra_via.py:128`, `app/chamados/services/fio.py:1034` | `app/chamados/veredito_lavrado.py:213`, `:279`; `app/chamados/services/tarefa_do_dia.py:351`; `app/chamados/services/raiox.py:34`, `:35`, `:153`; `app/chamados/services/relavra_via.py:106`; snapshot `app/chamados/services/passivo_validado.py:65`; `app/chamados/management/commands/backfill_veredito_lavrado.py:82`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1173` |
| `resolvido_em/respondido_por/via_encerramento` | `app/chamados/models.py:201`; motor inicial pelos nascimentos `:833`, `:1054` | `app/chamados/models.py:663`, `:695`, `:983`, `:1318`, `:1362`; `app/chamados/services/verbos.py:218`; `app/chamados/services/painel.py:50`, `:247`; `app/chamados/services/fila.py:109`; `app/chamados/views_arquivar.py:52`, `:153`; `app/chamados/services/disputa_emissao.py:1779`; `app/chamados/services/competencia_trancada.py:163`, `:235`; `app/chamados/services/adesao.py:44`; `app/ponto/services/furos_sem_cobranca.py:69`; relatórios `app/relatorios/views.py:738`, `:914` |
| `DecisaoPergunta.*` | porta `app/chamados/models.py:3932`; `save` só inserção `:3892`; autores das linhas nos escritores R/V/recusa/reabertura, `:1921` SILENCIO, `:2053` RETRATACAO e `app/chamados/services/materializacao.py:563` ABSORCAO | `app/chamados/services/tarefa_do_dia.py:127`, `:131`, `:140`, `:149`, `:334`; `app/chamados/models.py:3489`, `:3903`; backfill `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `:163` |

O inventário completo de `status_local` é muito maior, conforme o censo já colado; o escritor canônico é `_gravar_estado` e os leitores incluem a própria fila que alimenta tarefa, sinais/cascatas, dedup, motor e guardas do chamado. A enumeração global por AST encontra também modelos homônimos, consultas ORM, snapshots e testes em `tests.py`; não transforma esse achado bruto em censo completo tipado sem conferência. (`app/chamados/models.py:201`; `app/chamados/services/painel.py:155`; `app/chamados/services/painel.py:189`; `app/chamados/services/tarefa_do_dia.py:365`; `app/chamados/catalogo/motor.py:125`)

**E — testemunhas existentes e contrato que falta**

1. Imutabilidade/mesmo relógio/recusa/reabertura/SILENCIO/RETRATACAO já têm casos em `app/chamados/tests/test_e1_entradas.py:64`, `:78`, `:94`, `:103`, `:115`, `:121`, `:142`; esses casos não provam que E4 parou de gravar os derivados. Mutante necessário para A/B1–B8: restaurar qualquer atribuição a R/V/C depois de migrar o leitor. (`app/chamados/tests/test_e1_entradas.py:64`; `app/chamados/tests/test_e1_entradas.py:217`)
2. AST com apelido deve cobrir atributos, `setattr`, `update/create`, dict espalhado, alias de modelo e alias de método; há detector exemplar e mutantes concretos, inclusive método novo dentro de models. Sua allowlist atual autoriza C nas portas e precisa virar ausência para campos abolidos, com exceção L declarada. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:109`; `:226`; `:270`; `:283`; `:296`; `:515`; `:583`)
3. Selo novo de “nenhum leitor produtivo lê campo abolido” precisa morder atributo, `getattr`, ORM, `values`, `order_by`, agregação, strings e template; catálogo de consumidor dinâmico fora disso permanece [nao verificado]. O detector E3 atual resolve imports e apelidos, mas só sua pergunta específica; não é esse selo geral. (`app/chamados/tests/test_e3_pendentes_app.py:34`; `:149`; `app/chamados/services/explicador.py:85`; `app/chamados/services/painel.py:67`)
4. Matriz nova app/admin/tarefa deve pôr projeção deliberadamente contraditória com DP/efeito e exigir saída igual pela regra; mutantes “consulta volta a R/F”, “C6 volta a V”, “tarefa volta a via” mordem B1/B3/B5/B6. Testes atuais preservam os três chamadores C17, e não certificam E4 completa. (`app/chamados/tests/test_e3_pendentes_app.py:144`; `:159`; `app/chamados/tests/test_e3_portas_tarefa.py:113`)
5. Caso B9: conferência divergente libera pelo ato humano; concordante não fabrica ato; duas datas mudadas depois não devem herdar conferência de outro par. Mutantes: nulo vira autorização, ou qualquer conferência antiga autoriza todo par. Os dois primeiros comportamentos estão no código; validade da conferência após mudança do par é risco técnico a medir, não lei nova inventada. (`app/chamados/services/validacao.py:658`; `:662`; `app/chamados/services/materializacao.py:1193`)
6. B10/B11: madrugada, envelope exatamente na borda, L expirando exatamente hoje, lavra futura, marco exatamente igual a

[truncado em 20000 de 21890 caracteres]

## 09/11 16:20 · sessao 34f39634760c · - · - · origem=Ronald (tmux)

**C — definições e passivo**

| Contador | Universo e incremento |
|---|---|
| `fio_morto` | Parte dos chamados `VIVOS` com célula, recorta `FURO_MODULES` e lê células com veredito acusador; tira fio vivo, hoje/futuro e quatro baldes com ação/dono; incrementa por célula restante. `app/chamados/services/vigia_fio.py:335`, `:349`, `:378`, `:418`, `:437`, `:449`. |
| `fio_morto_cobrando` | Subconjunto dos dias acima com algum chamado do vocabulário em `COBRANCA`; mapa em `app/chamados/services/vigia_fio.py:351`, incremento em `:451`. |
| `fio_morto_alcance` | Dia-colaborador que segura folha, nas competências enumeradas por `competencias_a_lavrar`, sem porta, fora de hoje/futuro e dos quatro baldes; universo em `app/chamados/services/vigia_fio.py:580`, `:586`, `:591`; porta em `:638`; exclusões em `:648`, `:654`; incremento em `:695`. Fontes reais: `app/folha/fora_da_folha.py:102`, `app/ponto/services/he_pendente_lavrado.py:209`. |
| `materializada_viva` | Quantidade de perguntas com carimbo elegível a desfazer, disputa aberta e julgadas vivas por C6; separa competência entregue de aberta. Universo em `app/chamados/services/vigia_fio.py:805`, juiz em `:813`, dono em `:816`, contador em `:847`; elegibilidade canônica em `app/chamados/services/relavra_via.py:171`, `:181`. |

A medida anexada permite classificar **377 = 2 + 0 + 17 + 284 + 74**, sem nova sonda. Os números vêm do anexo, sem arquivo local numerado: **[nao verificado]**. A interpretação das classes vem dos seguintes sítios:

| Classe | Número anexado | Significado efetivo e limite da conclusão |
|---|---:|---|
| a | 2 | Chamado vivo fora da fila; detalhes anexados `registrado`. Não equivale automaticamente a silêncio temporário. `app/chamados/services/vigia_fio.py:664`, `:669`. |
| b | 0 | Pergunta viva respondida sem texto. `app/chamados/services/vigia_fio.py:673`. |
| c | 17 | Pergunta viva respondida sob destino terminal. `app/chamados/services/vigia_fio.py:675`. |
| d | 284 | Sem pergunta viva, colaborador aparece na CxR, **dia não aparece** na lista. A classe não prova que o dia tem dono CADASTRO/BATIDA. `app/chamados/services/vigia_fio.py:677`; declaração explícita em `:520`. |
| e | 74 | 50 `pergunta_viva_sob_chamado_terminal` + 24 `sem_pergunta_viva`, conforme anexo **[nao verificado]**; regras em `app/chamados/services/vigia_fio.py:685`, `:687`. |

Os 170 `fio_do_admin`, 276 `decisao_do_dia_pendente`, 16 `aguarda_decisao_de_ausencia`, 2 `escala_nao_descreve_o_dia` e 3 `alcance_de_hoje_pra_frente` não compõem os 377, pelo controle de fluxo de `app/chamados/services/vigia_fio.py:648`, `:654`, `:657`, `:695`; seus valores são do anexo **[nao verificado]**.

Também não se pode somar os 2 `materializada_viva` aos 377: um contador conta perguntas, o outro conta dias, e a interseção não foi fornecida. `app/chamados/services/vigia_fio.py:591`, `:818`, `:851`.

Os cinco dias do universo antigo já vêm divididos em quatro com perguntas da porta sem alvo e um sem pergunta na porta; `fio_morto_com_alvo=0`, conforme anexo **[nao verificado]**. A porta `passivo_validado` não é solução universal: exclui `plantio_fora_do_slot` e `slot_nao_derivavel` em `app/chamados/services/passivo_validado.py:60`, aplicando esse veto em `:330`; o alvo do vigia deriva dessa declaração em `app/chamados/services/vigia_fio.py:152`.

**O que pode cair prospectivamente:** parar carimbos derivados indevidos e completar decisões E1 impede novas combinações incompatíveis; porém o efeito quantitativo de cada porta sobre cada contador é **[nao verificado]**. Há dependência real dos leitores antigos: `censo` ainda chama `fio_do_dia`/C6 em `app/chamados/services/vigia_fio.py:418`, e `censo_alcance` usa `alimentar_portas`/C17 em `:604`, `:638` e `dono_fora_do_fio` em `:654`. Só remover escritores não demonstra queda imediata nesses contadores.

Para separar o passivo por cura real, “**precisa medida na sombra**”: chamar `censo_alcance`, `carimbos_sem_lastro_por_dono`, `tarefa_do_dia` e as portas correspondentes; para os 284, ler a mesma `lista`/`dias_na_lista` carregada por `ListaDoAdmin._carregar`, sem inventar mapa classe→dono. `app/chamados/services/vigia_fio.py:212`, `:216`, `:217`, `:542`, `:789`; `app/chamados/services/tarefa_do_dia.py:442`. O próprio vigia proíbe deduzir dono de classe em `app/chamados/services/vigia_fio.py:99`.

**C3 — backfill único, desenho e limitações das portas existentes**

1. Inventário DRY pela fonte real, agrupado por competência e com todas as linhas sem porta; não truncar o passivo. O censo já conserva todo `sem_porta` em `app/chamados/services/vigia_fio.py:630`; competência real em `app/ponto/services/fechamento.py:1602`. Leis de origem: L-003 `app/docs/LEIS.md:61`, L-121 `:142`, L-124 `:145`.
2. Preservar a projeção legada como decisão antes de retirar o campo que a guarda: produtor canônico `DecisaoPergunta.linhas_da_projecao_sem_linha` em `app/chamados/models.py:3903`, inserção canônica `registrar` em `:3932`. Não fabricar texto de respostas antigas sobrescritas: limite declarado em `app/chamados/management/commands/backfill_decisoes_pergunta.py:12`.
3. **Não afirmar que o `--aplicar` E1 atual resolve essa etapa.** Ele mede as projeções R/V, mas explicitamente não as aplica; só escreve colisões e silêncio faltante. `app/chamados/management/commands/backfill_decisoes_pergunta.py:3`, `:6`, `:25`, `:426`.
4. Exportada fica fora com lista para o `!`, conforme pedido literal do usuário **[nao verificado]**; precedente mecânico real: lista de projeções por competência em `app/chamados/management/commands/backfill_decisoes_pergunta.py:455`, `:471`, `:480`; grupos exportados fora e rc 3 em `:620`. Não usar a permissividade geral de L-113 para alargar esse aval específico: L-004 `app/docs/LEIS.md:62`.
5. Escrever reversão antes de qualquer alteração, incluindo estado das perguntas, disputas, chamados e fatos atingidos. Precedente existente em `app/chamados/management/commands/reabrir_orfas_enterradas.py:123`, `:128`, `:137`; o comando E1 atual não fornece essa reversão em arquivo **[nao verificado]**.
6. DRY e APPLY devem atravessar a mesma porta, com rollback no DRY; molde existente em `app/chamados/management/commands/backfill_decisoes_pergunta.py:578`, `:582`, `:584`. Ordem de travas real em `app/chamados/models.py:2626`; uso no reparo em `app/chamados/management/commands/reabrir_orfas_enterradas.py:153`.
7. Por espécie de ato: decisão faltante por `registrar`; Batida pela materialização que chega a `registrar_batida` em `app/chamados/services/materializacao.py:926`, `:1006`; ausência por `app/chamados/services/materializacao.py:462`; ausência de pergunta pela fábrica `app/chamados/services/fabrica_da_celula.py:139`; carimbo sem lastro pela porta `app/chamados/services/relavra_via.py:187`, hoje chamando `descarimbar` em `:246`. Essas portas precisam da cura E4 antes de reutilização no backfill.
8. Idempotência não vem de `registrar`: ele faz `bulk_create` direto em `app/chamados/models.py:3934`. A seleção canônica deduplica por pergunta+ato+instante em `:3916`, `:3920`, `:3924`; deve ser relida sob trava. O requisito “duas execuções, mesmo estado, uma trilha por ato” ainda exige selo E4 **[nao verificado]**.
9. A porta de desfazer carimbo já evita segunda trilha quando não há ação: relê elegibilidade sob trava em `app/chamados/services/relavra_via.py:230`, `:238`, continua se não há candidato em `:240`, grava trilha só depois da mudança em `:248`.
10. O carimbo único é sobrescrito por `os.replace` em `app/chamados/management/commands/vigiar_fio_morto.py:134`; sozinho não prova três rodadas consecutivas. As três evidências precisam ser retidas pela sessão ou receber contrato próprio de sequência **[nao verificado]**.

**D — alarme**

- `ALARMA_EM_0` atual contém três chaves em `app/chamados/management/commands/vigiar_fio_morto.py:83`; `ESPERADO_DO_CARIMBO` já leva `materializada_viva=0` e `fio_morto_alcance=0` em `:96`. Os esperados não precisam virar 377/2 nem ganhar tolerância.
- **Armadilha concreta:** apenas acrescentar as duas chaves à tupla causa consulta inexistente: `gritou` lê `d[k]` em `:179`, enquanto alcance/mvv só são anexados ao dicionário do carimbo em `:221`. O desenho deve usar a composição dos contadores antes de julgar o alarme. Esse ajuste está dentro de `app/`.
- O texto/comentário “fora do alarme até zerar” está em `:93`; `materializada_viva` também se apresenta como “não alarma” em `:195`. Precisam acompanhar o contrato final.
- O cron já usa `--alarme --alarme-passivo`: `app/config/crons.py:1718`; não é necessário mudar esse arquivo proibido apenas para ativar as novas chaves.
- Com números não zero, o painel **já** acusa FORA DO ESPERADO: `bin/fio_estado.py:125`, `:180`. Ativar o alarme antes do backfill acrescenta `gritou` e rc 2: `app/chamados/management/commands/vigiar_fio_morto.py:204`, `:232`; o carimbo fica gravado antes da saída em `:207`, `:211`.
- Ordem de saída: portas/leitores coerentes → backfill permitido → nova medição → prova das três rodadas → alarme definitivo; essa ordem operacional cumpre o aval fornecido **[nao verificado]**.
- Leitor direto do JSON: `bin/fio_estado.py:98`, `:104`; compara todos os esperados em `:125`, lê chaves/alarme em `:146`, monta aviso condicionado em `:182`. Consumidor da representação: `bin/gerar_estado.py:260`. Outros leitores por acesso dinâmico: **[nao verificado]**.
- `bin/fio_estado.py:182` já deixa de imprimir o aviso quando a chave entra no carimbo; não exige hunk funcional para essa retirada. Docstring velha em `bin/fio_estado.py:50` é hunk de sessão, por estar fora de `app/`.

**Selos que precisam mudar / mutantes**

1. O selo atual exige que alcance **não** esteja no alarme: `app/chamados/tests/test_o261e_f5_alcance.py:459`, `:471`. Inverter para exigir inclusão após a etapa; mutante: retirar a chave.
2. O conjunto de chaves antigas é fixado em `app/chamados/tests/test_o261_vigia_fio.py:753`, `:759`, e exige presença no `censo` simples em `:767`. Adaptar ao mapa composto; mutantes: continuar usando `d[k]`; omitir alcance/mvv da composição.
3. Selo funcional: fio antigo zero com alcance=1 e depois mvv=1 deve alarmar e deixar JSON completo; mutantes: ler só `d`, calcular `gritou` antes da composição, sair antes de `_carimbar`. Arquivo E4 proposto ainda inexistente **[nao verificado]**.
4. Prova do carimbo como fotografia já existe em `app/chamados/tests/test_o261_vigia_fio.py:812`, esperados em `:819`.
5. Idempotência de desfazer carimbo já tem precedente em `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:122`; E4 precisa medir a operação inteira, sem supor que o precedente cobre todo o lote. Mutante: `registrar` incondicional na segunda corrida.
6. Exportada intocada precisa comparar hash de FM/TXT antes/depois e testar exportação surgindo entre DRY e trava; selo E4 ainda **[nao verificado]**. Precedente de lista/recusa atual em `app/chamados/management/commands/backfill_decisoes_pergunta.py:480`, `:620`.

**Proibidos para C/D**

A cura da porta `descarimbar` e das decisões de projeção exige hunk de sessão em `app/chamados/models.py:3192`, `:3903`; a composição/alarme fica com o construtor em `app/chamados/management/commands/vigiar_fio_morto.py:178`; representação/documentação de host fica com a sessão em `bin/fio_estado.py:50`; o cron existente já passa as opções em `app/config/crons.py:1718`. Migração só por este subescopo C/D não é demonstrada; remoção física de campos demanda etapa de sessão **[nao verificado]**.

## 09/11 16:20 · sessao 8e342f692882 · - · - · origem=Ronald (tmux)

Todas as **20 definições de alteração** abaixo estão nas mesmas linhas do censo fornecido. Os seis nascimentos também permanecem em `models.py:833,1054,2822` e `disputa_emissao.py:474,1024,1299`. Identidade criptográfica da cópia: **[nao verificado]**, pois `git rev-parse HEAD` encontrou diretório sem repositório Git.

Abreviações de leitores, todos de produção:

- **LC**: `app/chamados/juizes.py:645,796,1322,1411`; `app/chamados/models.py:3023,3182,3315`; `app/chamados/services/materializacao.py:425,496,1089,1369`; `app/chamados/services/relavra_via.py:118`; `app/chamados/services/raiox.py:51,153`. São leituras do campo C/via; C6 e pré-filtro permanecem ativos.
- **LR**: `app/chamados/models.py:3023,3315,3486,3508,3903`; `app/chamados/juizes.py:1339,1444,1590,1597,1765`; `app/chamados/services/pergunta_no_app.py:70`; `app/chamados/services/materializacao.py:829,1369`; `app/chamados/services/validacao.py:576,580,912`; `app/chamados/services/disputa_emissao.py:1644,1667`. Ainda leem R.
- **LV**: `app/chamados/models.py:3023,3315,3903`; `app/chamados/juizes.py:1323`; `app/chamados/services/materializacao.py:1369`; `app/chamados/services/validacao.py:144,373,573,695`; `app/ponto/services/procedencia_batida.py:54`. Ainda leem V.
- **LF**: `app/chamados/models.py:1820,1852,2123,2160,2179,2247,2626`; `app/chamados/services/pergunta_no_app.py:70`; `app/chamados/services/disputa_emissao.py:34,474,942,1024,1299`; `app/chamados/signals.py:19,121,148`. Ainda leem F.
- **LS**: `app/chamados/models.py:119,128,201,410,663,695,717,773,800,809,833,983,1318,1362,1602,1619`; `app/chamados/services/painel.py:96,135,158,198`; `app/chamados/reconciliador.py:238,761,961,1201,1441,1529`; `app/chamados/signals.py:54,75,91,179,188,270,337`. Ainda leem estado canônico do chamado.
- **LL**: `app/chamados/veredito_lavrado.py:107,151,213,279`; `app/chamados/services/tarefa_do_dia.py:349`; `app/chamados/services/raiox.py:10,51,153`. L é cache declarado; a tarefa já o lê em `tarefa_do_dia.py:351-352`.

| Célula/porta atual | Campo escrito | Fato disponível para substituir | Leitor / divergência e destino esperado |
|---|---|---|---|
| A01 `app/chamados/models.py:201 _gravar_estado` — efeito/decisão | `status_local,resolvido_em,via_encerramento,respondido_por`, urgência e L delegado | **Chamado**, transição e trilha no mesmo ato (`:294-300`) | LS; permanece porta canônica do efeito chamado. Não pode somar seu estado canônico como se fosse cópia derivada da pergunta. |
| A02 `models.py:1852 fechar` — efeito/exceção decisão | F; C delegado | DP `SILENCIO` nas exceções (`:1921`); fechamento normal só F/trilha (`:1930-1937`) | LF/LC. **DIVERGE**: fechamento normal não possui fato DP equivalente; exceção possui. Separar fato humano de fechamento derivado antes de retirar F. |
| A03 `models.py:2123 reabrir_admin` — decisão | limpa F, contador/motivos | Nenhuma linha DP desta reabertura; autor, contador e F apenas (`:2135-2158`) | LF. **DIVERGE**: não confundir com DP `REABERTURA` da resposta. Metadados/limite permanecem; decisão precisa fato antes de retirar F. |
| A04 `models.py:2160 reabrir_sistema` — efeito | limpa F | Pergunta voltou a viver, com nota no chamado (`:2174-2177`) | LF. **DIVERGE**: consumidores ainda exigem F aberto; retirar projeção apenas junto da troca. |
| A05 `models.py:3092 carimbar` — efeito/assinatura decisão | C, completa R/V, opcional L | Batida/Ausência/chamado/ata conforme ramo; DP apenas se outro chamador já o gravou | LC/LR/LV/LL. **DIVERGE**: a porta completa assinatura em `:3119-3125` sem registrar DP. Remover escrita C; assinatura não pode sumir; L declarado permanece. |
| A06 `models.py:3157 carimbar_identidade` — efeito/decisão delegada | C, `recusa_motivo` opcional | DP retratação quando chamado por `remover_motivo:2053`; outros ramos não têm um fato DP uniforme | LC; a própria tarefa lê via atual (`tarefa_do_dia.py:216-228`). **DIVERGE crítico**: duplicata não tem fato datado (`:220-221`); parar via quebra identidade/dedup, além da pendência. |
| A07 `models.py:3192 descarimbar` — efeito | limpa C | Retração/ausência rejeitada/resposta nova conforme chamador | LC. **DIVERGE**: remover limpeza junto de todos consumidores e do índice que usa via, nunca isoladamente. |
| A08 `models.py:3241 descarimbar_em_lote` — efeito | limpa C e `outros` R/V recebidos | DP da reabertura (`disputa_emissao.py:1612`) quando esse é chamador | LC/LR/LV. **DIVERGE**: filtro terminal por via ainda necessário (`models.py:3253-3255`); efeito deve ler decisões/identidade factual. |
| A09 `models.py:3257 encerrar_por_fato_no_espelho` — efeito | via | Fato do marco já lavrado, descrito em `:3258-3261` | LC; **DIVERGE** para leitores via. Via derivada deixa de ser escrita quando leitores conferirem efeito. |
| A10 `app/chamados/services/validacao.py:127 _carimbar_validacao` — decisão | V | DP `VALIDACAO` no mesmo instante (`:139-141`) | LV; **DIVERGE** nos leitores V. V sai; DP é fato. |
| A11 `validacao.py:163 devolver_validacao_recusada` — decisão | limpa V, preserva R | DP `DEVOLUCAO` (`:205`) | LV/LR; **DIVERGE**. Fatos anteriores preservados, tarefa lê ato em `tarefa_do_dia.py:138-139`. |
| A12 `app/chamados/services/disputa_emissao.py:1422 responder_pergunta_disputa_core` — decisão | R, limpa V/C, recusa explicativa | DP `RESPOSTA` (`:1508`); escape é `ato_texto`, não DP `RECUSA` | LR/LV/LC; **DIVERGE**. R/V/C derivados saem; resposta passa a ser conteúdo do ato vigente. |
| A13 `disputa_emissao.py:1559 reabrir_para_correcao` — decisão | limpa R/V e C opcional | DP `REABERTURA`/`RECUSA` com foto (`:1612-1625`) | LR/LV/LC; **DIVERGE**. Não escreve F hoje; não atribuir a esta porta reabertura da disputa. |
| A14 `disputa_emissao.py:1635 responder_automaticamente` — decisão sistema | R | DP `RESPOSTA` (`:1653-1654`) | LR; **DIVERGE**. Idempotência atual usa texto projetado (`:1644`); precisa consultar ato. |
| A15 `disputa_emissao.py:1658 carimbar_resposta_pela_trilha` — decisão reconstruída | `respondida_em` | DP `RESPOSTA`, instante da testemunha (`:1674-1675`) | LR; **DIVERGE**. Guarda atual R/texto (`:1667`) precisa ler sequência; não trocar instante por agora. |
| A16 `app/chamados/services/relavra_via.py:106 relavrar` — efeito | via + L | Via derivada do fato (`:95-103`), L da autoridade | LC/LL; **DIVERGE** na via. Encolhe para L, ou desaparece quando não tiver trabalho próprio; não deixar segundo juiz morto (L-111). |
| A17 `app/chamados/services/materializacao.py:784 _materializar_resposta_hora` — efeito | `pendencia_admin,recusa_motivo`, efeitos/carimbos delegados | Batida quando sucesso (`:859`); na pendência técnica o fato DP é **nenhum** (`:845-851`) | `app/chamados/services/explicador.py:68`; `materializacao.py:784`; auditorias. **DIVERGE**: mensagem técnica não vira decisão humana. Recalcular resultado técnico pela porta real ou guardar fato do insucesso, sem conservar estado derivado disfarçado. |
| A18 `materializacao.py:1083 _carimbar_recusa` — efeito técnico | `recusa_motivo`, C identidade delegado | Nenhum DP; nota `recusa_materializacao` (`:1107`) e colisão existente | `materializacao.py:1089-1094`, `explicador.py:68`; LC. **DIVERGE**: não tratar colisão como DP `RECUSA` humana. |
| A19 `validacao.py:533 recusar_pergunta` — decisão | recusa, R/V delegado | DP `RECUSA` (`:593-594` → `disputa_emissao.py:1612`) | `validacao.py:573-584`, `explicador.py:68`, LR/LV. **DIVERGE**: idempotência depende R (`:576`), motivo projetado pode sair após leitor do ato. |
| A20 `validacao.py:625 conferir_data_do_fato` — decisão | `data_conferida_em/por` | **Nenhum DP**; LogAuditoria com dias/justificativa (`:667-672`) | `validacao.py:662`; `materializacao.py:1193`. **DIVERGE**: remover sem substituto factual trava materialização; DP `ATOS` não tem conferência (`models.py:3846-3856`). |
| A21 condicional `models.py:1968 remover_motivo` — decisão + metadados próprios | motivos/contexto; C delegado | DP `RETRATACAO` (`:2053-2057`) | `tarefa_do_dia.py:230-232` lê ato, mas `:227` ainda lê via; C6 `juizes.py:1334`. **DIVERGE**: muda no diff de E4 por identidade/terminalidade, portanto deve entrar na matriz mesmo fora das 20 alterações contadas. |

**Nascimento que a E4 obriga a mudar:** `PerguntaDisputa._nascer` (`app/chamados/models.py:2822`) inicializa projeções e faz costura/L no reuso; a remoção física de campos ou a proibição de escrevê-los precisa contemplar essa porta. Não é permitida exclusão por estar rotulada “fica”. Defaults dos campos também exigem revisão no modelo (`app/chamados/models.py:2444`, `:3366`).

**16 versus 26:** a soma reconstruída do censo é **16 primeiras alterações + quatro alterações adicionais =20; + seis nascimentos =26**. `remover_motivo` tem decisão delegada/metadados próprios e não integra os 20 campos derivados escritos diretamente listados. A comparação literal **16 → N** não mede o mesmo universo de **26** (censo fornecido **[nao verificado]** como arquivo local; definições verificadas nos endereços acima).

**N defendível:** alvo **N=0 portas que escrevem estado DERIVADO da pergunta**, lista residual vazia; conservar separadamente o escritor factual `DecisaoPergunta.registrar` (`models.py:3932`), efeitos Batida/Ausência/chamado e cache L declarado. Isso não significa zero escritores do sistema. Não é honesto declarar “16→4” somando três famílias de efeitos com um executor DP: seria troca da unidade de contagem. `DecisaoPergunta` declara explicitamente que as projeções são transitórias até E4 (`models.py:3832-3833`).

**Bloqueio real da cópia:** E3-A centraliza a consulta do app, mas não a desliga dos campos. `consulta_do_app:69` chama `perguntas_do_app`, que usa `q_vivas:49` e C6 `:57`; depois particiona por R/F (`:70`). C6 ainda lê V (`juizes.py:1323`), C/via (`:1334,1360,1366`) e R/texto (`:1339`). A seleção materializadora ainda exige R/V/M (`materializacao.py:1369-1373`). A própria tarefa usa R/V para detectar passivo (`tarefa_do_dia.py:346` → `models.py:3903`) e via para identidade (`tarefa_do_dia.py:227`). Logo, cortar somente escritores deixa leitores de produção dependentes dos campos; **a E4 exige troca coordenada desses leitores**, inclusive modelos proibidos, antes de ser fechada.

## 09/11 16:14 · sessao 756b2c3a17e7 · FIO-E0-V4-E4-ESCRITORES · censo · origem=Code

**O total comparável é 26 portas: 20 de alteração + 6 de nascimento, diferença de +4 sobre o número anunciado no v1.** As quatro adicionais já estavam descritas no v1: gravam pendência, recusa e conferência de data. Esses campos interferem no processamento da pergunta; não são apenas apresentação ou cache. (`app/chamados/services/materializacao.py:851`; `app/chamados/services/materializacao.py:1103`; `app/chamados/services/materializacao.py:1193`; `app/chamados/services/validacao.py:590`; `app/chamados/services/validacao.py:666`)

A conferência foi estática por AST, sem importar a aplicação, executar testes, acessar banco/log ou escrever arquivos. Identidade criptográfica independente da cópia, execução produtiva, despacho inteiramente dinâmico, SQL dinâmico e consumidores por reflexão: **[nao verificado]**. [nao verificado]

Para a soma, cada função com escrita semântica própria conta uma vez, mesmo quando também delega. `PerguntaDisputa.save` e `DecisaoPergunta.registrar` ficam discriminados como executores das portas: somá-los novamente duplicaria as persistências que executam. A sequência E1 possui efetivamente sua porta única de inserção. (`app/chamados/models.py:3402`; `app/chamados/models.py:3836`; `app/chamados/models.py:3932`)

**Delta dos escritores.** Nenhum dos escritores do v1 foi eliminado ou teve seus campos/classificação alterados pelo diff fornecido; as definições das 26 portas permanecem nas linhas apresentadas no CENSO. O acréscimo de quatro ao total corrige a soma do v1; não representa quatro portas criadas pelo diff. (`app/chamados/models.py:201`; `app/chamados/models.py:2822`; `app/chamados/services/disputa_emissao.py:1422`; `app/chamados/services/materializacao.py:784`; `app/chamados/services/materializacao.py:1083`; `app/chamados/services/validacao.py:533`; `app/chamados/services/validacao.py:625`)

Omissões antigas do inventário de escritores, acrescentadas abaixo como **fica**, sem aumentar o N de estado da pergunta:

| Escritor omitido | Escrita própria | Chamadores encontrados |
|---|---|---|
| `app/chamados/models.py:488 rebaixar` | Urgência/original/atualização | T: `app/chamados/tests/test_selo_e4_gates.py:73`; P direto: **[nao verificado]** |
| `app/chamados/models.py:513 escalar_urgencia` | Urgência/atualização | P: `app/ponto/services/triagem_batida.py:292`; `app/ponto/management/commands/processar_alertas_avancados.py:176` |
| `app/chamados/models.py:618 _rebaixar_sem_post_save` | Urgência/original/atualização | P: `app/chamados/signals.py:72` |
| `app/chamados/models.py:573 definir_prazo_sla` | Prazo/atualização | P: `app/chamados/services/cobranca.py:178`; `app/chamados/services/disputa_emissao.py:1715` |
| `app/chamados/models.py:594 marcar_escalonado_dp` | Recibo de escalada/atualização | P: `app/ponto/management/commands/escalonar_chamados_supervisao.py:103` |
| `app/chamados/models.py:2072 anotar_contexto` | Contexto da disputa | P: `app/chamados/views.py:1343`; `app/ponto/management/commands/escalonar_chamados_supervisao.py:159` |
| `app/chamados/models.py:2105 renovar_contexto` | Contexto da disputa | P: `app/ponto/management/commands/alertar_disputas_pendentes.py:48` |
| `app/chamados/services/disputa_emissao.py:257 perguntar_entradas_pendentes` | Contexto da disputa | P: `app/chamados/services/disputa_emissao.py:685`; `app/chamados/services/disputa_emissao.py:1267` |

Chamadores de teste dessas omissões, salvo os expressamente apontados: **[nao verificado]**. [nao verificado]

Também faltava a linha de escritor de `DecisaoPergunta.registrar`: preserva os atos E1 por `bulk_create`. Chamadores P incluem fechamento, retratação, absorção, resposta, validação, devolução e reconstrução; há testemunhas T específicas. (`app/chamados/models.py:3932`; `app/chamados/models.py:1921`; `app/chamados/models.py:2053`; `app/chamados/services/materializacao.py:563`; `app/chamados/services/validacao.py:139`; `app/chamados/services/validacao.py:205`; `app/chamados/services/disputa_emissao.py:1508`; `app/chamados/services/disputa_emissao.py:1612`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`; `app/chamados/tests/test_e1_volta2.py:84`)

**Delta dos leitores.** As referências seguintes são as linhas atuais do lado `+` dos respectivos hunks do diff fornecido:

| Leitor do v1 | Mudança efetiva |
|---|---|
| `app/api/views.py:991 api_chamados_v2` | A seleção crua passa a `consulta_do_app`; percorre pendentes + histórico e conserva somente disputas abertas nessa representação. Histórico respondido continua disponível para correção. (`app/api/views.py:1023`; `app/api/views.py:1058`; `app/api/views.py:1059`) |
| `app/chamados/services/aviso_pendencia.py:7 perguntas_pendentes_do_colab` | Deixa de montar seleção própria; retorna a lista central de pendentes. (`app/chamados/services/aviso_pendencia.py:9`) |
| `app/chamados/services/pergunta_no_app.py:60 consulta_do_app` | Passa a chamar `perguntas_do_app(..., em_lote=True)`; preserva a partição por resposta e fechamento. (`app/chamados/services/pergunta_no_app.py:69`; `app/chamados/services/pergunta_no_app.py:70`) |
| `app/chamados/services/tarefa_do_dia.py:396 CargaDaTarefa.tarefa` | Troca `alimentar_portas` por `alimentar_fila`: a carga obtém fila/verbos sem rejulgar C6. (`app/chamados/services/tarefa_do_dia.py:408`; `app/chamados/services/tarefa_do_dia.py:409`) |
| `app/chamados/services/cobranca.py:221 _calados` | Deslocamento de linha decorrente da mudança de `_pendentes`; mantém a leitura de respostas para a seleção dos calados. (`app/chamados/services/cobranca.py:229`) |
| Leitores posteriores em `juizes.py` | Deslocamentos pelas inserções/refatoração; campos e significado dos leitores já listados permanecem, com endereços atuais no CENSO. (`app/chamados/juizes.py:1322`; `app/chamados/juizes.py:1590`; `app/chamados/juizes.py:1765`; `app/chamados/juizes.py:2306`) |

Duas correções do v1 não são mudanças criadas pelo diff: `_estado_da_decisao` era o nome errado para `_pela_decisao`; a ocorrência de `data_conferida_por` na linha 666 é persistência, não consumidor. A definição de `atualizado_em` também não constitui leitor. (`app/chamados/services/tarefa_do_dia.py:115`; `app/chamados/services/validacao.py:665`; `app/chamados/services/validacao.py:666`; `app/chamados/models.py:68`)

Leitores novos no diff ou ausentes do inventário v1, com seus caminhos de chamada principais:

| Leitor | Campos/significado e origem | Chamadores |
|---|---|---|
| `app/chamados/juizes.py:1084 tranca_da_consulta` | Novo no diff; dia pelo juiz, resultado da tranca e memo restrito à consulta; apenas memória | P: `app/chamados/services/pergunta_no_app.py:56`; T indireto: `app/chamados/tests/test_e3_v4_orm.py:108` |
| `app/chamados/juizes.py:1450 alimentar_fila` | Novo no diff; fila/célula/verbos do painel | P: `app/chamados/juizes.py:1495`; `app/chamados/services/tarefa_do_dia.py:409`; T: `app/chamados/tests/test_e3_portas_tarefa.py:119` |
| `app/chamados/services/pergunta_no_app.py:44 perguntas_do_app` | Omitido no v1; R/V/C por `q_vivas` + C6; o diff acrescenta o contexto de consulta | P: `app/chamados/services/pergunta_no_app.py:69`; `app/chamados/juizes.py:1519`; T: `app/chamados/tests/test_n1_perguntas_do_app.py:94` |
| `app/chamados/services/cobranca.py:45 _pendentes` | Omitido no v1; agora lê a mesma partição central | P: `app/chamados/services/cobranca.py:82`; `app/chamados/services/cobranca.py:163`; T: `app/chamados/tests/test_e3_v4_orm.py:31` |
| `app/chamados/services/tarefa_do_dia.py:418 _portas_da_tarefa` | Omitido no v1; ações exigíveis + fila/verbos produzem rota/fila/verbos/fio | P: `app/chamados/services/tarefa_do_dia.py:412`; T indireto: `app/chamados/tests/test_e3_portas_tarefa.py:121` |
| `app/chamados/services/painel.py:50 marcador_do_placar` | Omitido no v1; instantes de resolução/R/V/M invalidam apresentação | P: `app/chamados/services/painel.py:91`; T: `app/chamados/tests/test_selo_cadeia_placar.py:76` |
| `app/chamados/models.py:336 _ancoradas` | Omitido no v1; destino e validação das perguntas ancoradas | P: `app/chamados/models.py:223` |
| `app/chamados/models.py:374 _plano_pergunta_viva` | Omitido no v1; identidade/dia no plano de vivacidade | P: `app/chamados/models.py:419` |
| `app/chamados/models.py:983 _renascer_por_premissa_viva` | Omitido no v1; via anterior do chamado | P: `app/chamados/models.py:922`; `app/chamados/services/resposta_tardia.py:45` |
| `app/chamados/models.py:2626 trava_na_ordem` | Omitido no v1; fechamento/status durante aquisição das travas | P: `app/chamados/models.py:290` |
| `app/chamados/models.py:2694 do_fato`; `app/chamados/models.py:2743 q_dos_fatos` | Omitidos no v1; conversa/via/dia/instância delimitam identidade do fato | P: `app/chamados/models.py:2770`; `app/chamados/services/fabrica_da_celula.py:124` |
| `app/chamados/models.py:3039 fechar_por_admin` | Omitido no v1; via/M para idempotência do encerramento | P: `app/chamados/models.py:1918`; `app/ponto/services/veredito_celula.py:189` |
| `app/chamados/juizes.py:339 _slot_esperado` | Omitido no v1; resposta usada na identificação do slot | P: `app/chamados/juizes.py:732`; T: `app/chamados/tests/test_marco_por_motivo.py:67` |
| `app/chamados/juizes.py:604 _celulas_da_pergunta` | Omitido no v1; dia gravado e células pertinentes | P: `app/chamados/juizes.py:680`; `app/chamados/juizes.py:1392` |
| `app/chamados/juizes.py:2148 resolvedora_valida` | Omitido no v1; discriminador conversa e dia pelo juiz | P: `app/chamados/services/fio.py:907`; T: `app/chamados/tests/test_e1_v15.py:110` |
| `app/chamados/services/fio.py:929 _colisao_ao_datar`; `app/chamados/services/fio.py:1047 rechavear_no_reuso` | Omitidos no v1; dia/identidade no reuso | P: `app/chamados/services/fio.py:895`; `app/chamados/models.py:2949`; T: `app/chamados/tests/test_e1_v15f.py:408` |
| `app/chamados/services/fio.py:1110 violacao_de_saida` | Omitido no v1; dia e contrato de saída da identidade | P: `app/chamados/services/fio.py:1137`; `app/chamados/services/fio.py:1169` |
| `app/chamados/services/fio.py:1547 recusa_conversa` | Omitido no v1; conversa não recebe identidade de fato | P: `app/chamados/models.py:2769`; `app/chamados/services/fio.py:875` |
| `app/chamados/services/fio.py:1869 perguntas_sobre_batida_retratada`; `app/chamados/services/fio.py:1987 fora_do_indice` | Omitidos no v1; conversa/via/dia em diagnósticos de identidade | P: `app/chamados/management/commands/backfill_decisoes_pergunta.py:545`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:486`; T: `app/chamados/tests/test_e1_v15c.py:93`; `app/chamados/tests/test_e1_v6_horizonte.py:138` |
| `app/chamados/management/commands/diff_e3_sombra.py:42 candidata`; `app/chamados/management/commands/diff_e3_sombra.py:47 projecoes`; `app/chamados/management/commands/diff_e3_sombra.py:61 casos_do_par`; `app/chamados/management/commands/diff_e3_sombra.py:108 Command.comparar` | Omitidos no v1; comparação instrumental de C17/tarefa por campos, chamador e caso | P operacional: `app/chamados/management/commands/diff_e3_sombra.py:100`; `app/chamados/management/commands/diff_e3_sombra.py:126`; `app/chamados/management/commands/diff_e3_sombra.py:128`; T: `app/chamados/tests/test_e3_diff_sombra.py:20`; `app/chamados/tests/test_e3_diff_sombra.py:135` |

Testemunhas T dos demais leitores omitidos não indicadas na tabela: **[nao verificado]**. O instrumento está declarado fora do pipeline; isso não demonstra que tenha sido executado. (`app/config/crons.py:771`)

**D1–D15 não caíram.** D2 incorpora a partição pendentes/histórico e a retirada de C6 da alimentação da tarefa; D9 incorpora a limitação explícita de `--agora`; D11 precisa corrigir a atribuição do ramo: `reabrir_por_colab` recusa disputa fechada antes de delegar, enquanto `reabrir_para_correcao` é a porta que limpa respostas sem escrever F. (`app/chamados/services/pergunta_no_app.py:70`; `app/chamados/services/tarefa_do_dia.py:408`; `app/chamados/management/commands/diff_e3_sombra.py:105`; `app/chamados/models.py:2221`; `app/chamados/services/disputa_emissao.py:1559`)

As novas distinções D16–D17 constam integralmente abaixo. Unidade do app e destino de C17 já receberam cortes; não abrem pergunta de lei nova. A troca de C17 continua condicionada ao DIFF, e seus três chamadores diretos permanecem os mesmos. (`app/docs/PROMPTS.md:2002`; `app/docs/PROMPTS.md:2007`; `app/folha/fora_da_folha.py:67`; `app/chamados/services/vigia_fio.py:635`; `app/escala/services/cadastro_realidade.py:488`)

Leis/cortes checados: L-002, L-007, L-051, L-061, L-073, L-080, L-085, L-088, L-110, L-111, L-113, L-117 e L-124; FIO-E0-V4; LEI-TRANCA-E2-JUIZ; LEI-E3-UNIDADE-E-C17; LEI-C17-UM-JUIZ. **Perguntas de lei: 0.** (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:65`; `app/docs/LEIS.md:92`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:100`; `app/docs/LEIS.md:107`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:137`; `app/docs/LEIS.md:151`; `app/docs/LEIS.md:138`; `app/docs/LEIS.md:145`; `app/docs/CORTES.md:426`; `app/docs/CORTES.md:427`; `app/docs/PROMPTS.md:2002`; `app/docs/PROMPTS.md:2007`)

Abreviações do inventário: **C** = `materializada_em/por`, `via_resolucao`; **R** = `resposta_colab`, `respondida_em`; **V** = `validada_em/por`; **L** = `veredito_lavrado`, `julgado_em`, `janela_expira_em`; **F** = `fechada_em/por`, `resolucao`, `motivo_fechamento`; **DP** = sequência `DecisaoPergunta`. (`app/chamados/models.py:3115`; `app/chamados/models.py:2444`; `app/chamados/models.py:3366`; `app/chamados/veredito_lavrado.py:250`; `app/chamados/models.py:1929`; `app/chamados/models.py:3828`)

## CENSO

escritores:

- app/chamados/models.py:201 ChamadoColaborador._gravar_estado -- `status_local`, `resolvido_em`, `via_encerramento`, `respondido_por`, urgência/original, atualização; relavra perguntas atingidas -- efeito nos derivados; decisao na transição humana; fica atualização.
- app/chamados/models.py:1852 DisputaSupervisao.fechar -- F; encerramento excepcional delega carimbo das perguntas e registra DP `SILENCIO` -- efeito no fechamento normal; decisao nas exceções.
- app/chamados/models.py:2123 DisputaSupervisao.reabrir_admin -- limpa F, incrementa contador e pode agregar motivos -- decisao; fica contador/metadados.
- app/chamados/models.py:2160 DisputaSupervisao.reabrir_sistema -- limpa F -- efeito.
- app/chamados/models.py:3092 PerguntaDisputa.carimbar -- C; pode completar `respondida_em`, V e L -- efeito no carimbo; decisao nos ramos de assinatura; fica L.
- app/chamados/models.py:3157 PerguntaDisputa.carimbar_identidade -- C, preservação opcional de instante/autor, `recusa_motivo` opcional -- efeito; decisao humana determinada pelo chamador.
- app/chamados/models.py:3192 PerguntaDisputa.descarimbar -- limpa C; `salvar=False` prepara memória -- efeito.
- app/chamados/models.py:3241 PerguntaDisputa.descarimbar_em_lote -- limpa C e grava campos recebidos em `outros` -- efeito.
- app/chamados/models.py:3257 PerguntaDisputa.encerrar_por_fato_no_espelho -- `via_resolucao=fato_materializado` -- efeito.
- app/chamados/services/validacao.py:127 _carimbar_validacao -- V e DP `VALIDACAO` -- decisao.
- app/chamados/services/validacao.py:163 devolver_validacao_recusada -- limpa V, preserva resposta; DP `DEVOLUCAO` -- decisao.
- app/chamados/services/disputa_emissao.py:1422 responder_pergunta_disputa_core -- R, limpa V; pode limpar C e registrar recusa explicativa; DP `RESPOSTA` -- decisao.
- app/chamados/services/disputa_emissao.py:1559 reabrir_para_correcao -- limpa R/V e opcionalmente C; DP com foto anterior `REABERTURA` ou `RECUSA` -- decisao.
- app/chamados/services/disputa_emissao.py:1635 responder_automaticamente -- R sem sobrescrever texto existente; DP `RESPOSTA` -- decisao de sistema.
- app/chamados/services/disputa_emissao.py:1658 carimbar_resposta_pela_trilha -- `respondida_em` no instante da testemunha; DP `RESPOSTA` -- decisao reconstruída.
- app/chamados/services/relavra_via.py:106 relavrar -- via derivada e L no mesmo save -- efeito na via; fica L.
- app/chamados/services/materializacao.py:784 _materializar_resposta_hora -- `pendencia_admin`, `recusa_motivo`; plantio/carimbos delegados -- efeito.
- app/chamados/services/materializacao.py:1083 _carimbar_recusa -- `recusa_motivo`; colisão delega C de identidade -- efeito de recusa técnica.
- app/chamados/services/validacao.py:533 recusar_pergunta -- `recusa_motivo`; delega limpeza R/V e DP `RECUSA` -- decisao.
- app/chamados/services/validacao.py:625 conferir_data_do_fato -- `data_conferida_em/por`, com justificativa e dois dias na trilha -- decisao.
- app/chamados/models.py:833 ChamadoColaborador.abrir -- identidade/contexto e status inicial pelo motor -- fica nascimento.
- app/chamados/models.py:1054 ChamadoColaborador.criar -- identidade/contexto e status inicial pelo motor -- fica nascimento.
- app/chamados/services/disputa_emissao.py:474 _abrir_ou_agregar_disputa -- nascimento aberto, motivos/contexto -- fica.
- app/chamados/services/disputa_emissao.py:1024 criar_disputa_a_partir_de_chamados_orfaos -- nascimento aberto, identidade, motivos/contexto -- fica.
- app/chamados/services/disputa_emissao.py:1299 criar_disputa_ancora_de_chamado -- nascimento aberto, vínculo com chamado, motivos/contexto -- fica.
- app/chamados/models.py:2822 PerguntaDisputa._nascer -- identidade, dia, instância, célula, destino, conteúdo/tipo/opções, defaults R/V/C; costura e L no reuso -- fica.
- app/chamados/veredito_lavrado.py:240 gravar_lavras_em_lote -- L por `bulk_update` -- fica cache.
- app/chamados/veredito_lavrado.py:254 lavrar -- L; `salvar=False` prepara memória -- fica cache.
- app/chamados/services/arquivo.py:141 silenciar -- `silenciado_ate/por/motivo`, atualização -- fica prazo/metadados de apresentação.
- app/chamados/services/arquivo.py:177 levantar -- limpa silêncio; atualização e primeiro `resgatado_em` -- fica.
- app/chamados/services/fio.py:804 reapontar_resolvedora -- destino, célula/dia quando aplicável, L -- fica identidade/cache.
- app/chamados/services/fio.py:970 rechavear_data_evento -- dia/célula e L -- fica identidade/data/cache.
- app/chamados/models.py:1968 DisputaSupervisao.remover_motivo -- motivos/contexto próprios; DP `RETRATACAO` e C delegados -- decisao; fica metadados.
- app/chamados/models.py:1949 DisputaSupervisao.aparar_motivo_respondido -- motivos/contexto; não apaga pergunta -- fica.
- app/chamados/models.py:2179 DisputaSupervisao.reabrir_por_colab -- contador próprio; reabertura R/V delegada -- fica contador; decisao delegada.
- app/chamados/models.py:2247 DisputaSupervisao.limpar_resolucao_residual -- `resolucao=''` em disputa aberta -- fica.
- app/chamados/models.py:488 ChamadoColaborador.rebaixar -- urgência/original, atualização -- fica prioridade.
- app/chamados/models.py:513 ChamadoColaborador.escalar_urgencia -- urgência, atualização -- fica prioridade.
- app/chamados/models.py:618 ChamadoColaborador._rebaixar_sem_post_save -- urgência/original, atualização por update -- fica prioridade.
- app/chamados/models.py:573 ChamadoColaborador.definir_prazo_sla -- prazo SLA, atualização -- fica prazo.
- app/chamados/models.py:594 ChamadoColaborador.marcar_escalonado_dp -- recibo/instante da escalada, atualização -- fica.
- app/chamados/models.py:2072 DisputaSupervisao.anotar_contexto -- contexto -- fica.
- app/chamados/models.py:2105 DisputaSupervisao.renovar_contexto -- contexto -- fica.
- app/chamados/services/disputa_emissao.py:257 perguntar_entradas_pendentes -- contexto da disputa; emissão delegada -- fica na escrita própria.
- app/chamados/services/contexto.py:62 anotar_contexto -- `contexto_json`, atualização -- fica.
- app/chamados/services/contexto.py:82 renovar_contexto -- `contexto_json`, atualização -- fica.
- app/chamados/services/contexto.py:108 retirar_contexto -- `contexto_json`, atualização -- fica.
- app/chamados/services/contexto.py:131 fundir_contexto -- `contexto_json`, atualização -- fica.
- app/chamados/services/vinculos.py:81 apontar_justificativa -- 

[truncado em 20000 de 67950 caracteres]

_3217 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
