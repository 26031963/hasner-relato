# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 08:56** · publicado em **12/11/2026 08:56** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 08:56 · sessao a53ca1c97fa3 · - · - · origem=Ronald (tmux)

Censo de leitores concluído por AST em memória (`python3 -B`, sem importar Django ou escrever arquivo). O inventário completo de chamadas e o cruzamento LASTRO foram enviados nas mensagens anteriores. A execução de cada leitor em produção permanece [não verificado].

- A autoridade já existe: `app/core/juizes.py:1304-1305`. A célula com geradora fora da vigência devolve `None`, sem escolher o vínculo novo: `app/escala/alimentacao.py:269-286`.
- Há leitores que admitem futuro: calendário aceita mês, semana, quinzena, competência e intervalo customizado sem teto em hoje (`app/colaboradores/services/calendario.py:159-212`), carrega células/geradoras (`:314-316`) e chama a autoridade (`:389`); `grade_da_celula` percorre toda a janela recebida (`app/escala/services/leitor_celula.py:350-412`); semana do posto pode incluir dias futuros e lê `CelulaDia.trabalha` (`app/escala/services/semana_do_posto.py:51-56`, `:85-87`).
- O cron não tem horizonte fixo de N dias. Sua janela é a união das competências correntes das empresas ativas: `app/ponto/management/commands/gerar_celulas.py:281-291`. Roda às 05:50 sem argumentos: `app/config/crons.py:426`. O corte vem de `Empresa.dia_inicio_competencia`, default 21 (`app/colaboradores/models.py:46`), interpretado por `app/ponto/janelas.py:10-63`. Horizonte real = `(fim - hoje).days` [aguarda sonda].
- Interseção direta com LASTRO: `app/api/views_mensageria.py:976`; `app/colaboradores/services/calendario.py:315,389`; `app/escala/utils.py:988,1311,2183,2192,2194,2212,2260`; `app/folha/export.py:359`; `app/ponto/precedencia.py:313,320,349,510,537`; `app/ponto/services/cartorio.py:875,1768,1903,2035`; `app/ponto/services/espelho.py:286,625,724,1054,1065`; `app/ponto/services/fechamento.py:125,190,191,656,948`; `app/relatorios/pdf_espelho.py:307,358,571`. A identificação desses caminhos como LASTRO provém do anexo fornecido [não verificado quanto ao estado externo da obra].
- MAPA-20: `app/docs/BACKLOG.md:1916` nomeia `bin/mapa20_montar.py`; `:1900` cita publicação por `bin/relato.sh`. O publicador desta cópia não inclui MAPA-20 nos pares publicados (`bin/relato.sh:103-117`) nem no `git add` (`:121`). Estado externo da obra [não verificado].
- Lei já resolve evento imediato, cron vigia, escritor único, célula irretroativa e exceção formal: `CLAUDE.md:149-156`, `:171-178`. Não identifiquei necessidade de juiz novo; as perguntas do domínio já têm autoridades registradas (`app/core/juizes.py:1304-1305`). O aval fornecido já determina alcance apenas de futuros existentes na janela, preservação de editadas e proibição de ampliar passado [não verificado fora do anexo].

Perguntas de lei sugeridas para consolidação: **0**.

## 12/11 08:56 · sessao 563c7d44c06f · - · - · origem=Ronald (tmux)

Censo executado por AST em memória, com o script integral já enviado; não importou Django nem código da cópia. Resultado: 13 definições, 403 ocorrências de chamada — 94 em produção e 309 em 97 arquivos de teste; zero erros de parse. Essas contagens são da execução do script enviado, sem arquivo persistido [nao verificado]. [nao verificado]

`models.py` reexporta `pergunta_viva`; desconsiderar esse reexport perderia leitores reais. `app/chamados/models.py:4083`.

As 94 chamadas de produção seguem abaixo. Cada linha conserva o alvo e o sítio `arquivo:linha::função`.

```text
pergunta_viva:
app/api/views.py:2608::api_disputa_responder
app/chamados/juizes.py:1486::pergunta_sem_lastro_ou_viva
app/chamados/juizes.py:1491::perguntas_vivas_de
app/chamados/juizes.py:1503::fio_vivo
app/chamados/juizes.py:1573::alimentar_portas
app/chamados/juizes.py:1674::perguntas_respondidas_a_validar
app/chamados/juizes.py:2336::_dias_das_perguntas_vivas
app/chamados/management/commands/auditar_invariantes_chamados.py:202::_contraprova_f1t
app/chamados/management/commands/auditar_invariantes_chamados.py:540::_i6_pares
app/chamados/management/commands/auditar_invariantes_chamados.py:858::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:894::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:1653::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:1710::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:1769::Command.handle
app/chamados/management/commands/auditar_invariantes_chamados.py:1782::Command.handle
app/chamados/management/commands/disparar_perguntas_competencia.py:109::contador_fio_mudo
app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:449::Command._descarimbados
app/chamados/management/commands/reabrir_orfas_enterradas.py:83::Command.handle
app/chamados/models.py:368::ChamadoColaborador._vivas_entre
app/chamados/models.py:1510::ChamadoColaborador._universo_da_ressuscitacao
app/chamados/models.py:1897::DisputaSupervisao.fechar
app/chamados/models.py:3045::PerguntaDisputa.estado
app/chamados/models.py:3140::PerguntaDisputa.carimbar
app/chamados/models.py:3376::PerguntaDisputa.viva
app/chamados/reconciliador.py:399::lavrar_sem_turno_aberto
app/chamados/reconciliador.py:662::perguntas_absolvidas_pela_celula
app/chamados/reconciliador.py:759::_viva_em_competencia_aberta
app/chamados/reconciliador.py:1087::veredito_orfa
app/chamados/reconciliador.py:1595::reatar_pergunta_orfa
app/chamados/relatorios_disputa.py:18::classificar
app/chamados/services/acoes_chamado.py:355::montar_contexto_detalhe
app/chamados/services/competencia_trancada.py:123::medir
app/chamados/services/competencia_trancada.py:133::medir
app/chamados/services/competencia_trancada.py:143::medir
app/chamados/services/disputa_emissao.py:1821::reabrir_credor_de_pergunta_viva
app/chamados/services/disputa_emissao.py:1966::_resolver_perguntas_por_chamado_resolvido
app/chamados/services/disputa_emissao.py:2048::_resolver_perguntas_por_chamado_resolvido
app/chamados/services/fio.py:238::montar_chamados_fio
app/chamados/services/fio.py:606::processar_post_fio
app/chamados/services/fio.py:782::_costura_nao_mata_o_fio
app/chamados/services/fio.py:961::_colisao_ao_datar
app/chamados/services/fio.py:2129::_migrar_sob_trava
app/chamados/services/fio.py:2134::_migrar_sob_trava
app/chamados/services/pergunta_no_app.py:58::perguntas_do_app
app/chamados/services/pergunta_no_app.py:170::_marco_ja_perguntado
app/chamados/services/relavra_via.py:242::desfazer_carimbo_sem_lastro
app/chamados/services/tarefa_do_dia.py:491::_carga_da_tarefa_cls.CargaDaTarefa._ler_vida
app/chamados/services/vigia_fio.py:755::viva_em_disputa_fechada
app/chamados/services/vigia_fio.py:813::carimbos_sem_lastro_por_dono
app/chamados/views.py:1242::responder_pergunta_disputa
app/chamados/views.py:1257::responder_pergunta_disputa
app/chamados/views.py:1417::reabrir_disputa_supervisao
app/colaboradores/services/calendario.py:476::contexto_calendario
app/ponto/services/dia_do_colab.py:71::dia_do_colab

_pergunta_viva_calc:
app/chamados/juizes.py:889::pergunta_viva
app/chamados/juizes.py:894::pergunta_viva

fio_vivo:
app/chamados/juizes.py:2389::disputa_viva_do_chamado
app/chamados/management/commands/encerrar_perguntas_com_fato.py:83::Command.handle
app/chamados/models.py:1854::DisputaSupervisao.pode_fechar
app/chamados/services/disputa_emissao.py:1422::_ja_perguntou
app/chamados/services/explicador.py:137::por_que_nao_fecha
app/chamados/services/limbo_folgas.py:203::filhos_no_defeito
app/chamados/services/vigia_fio.py:166::fio_do_dia
app/folha/previa.py:144::_material
app/ponto/management/commands/emitir_furo_retroativo.py:98::Command.handle
app/ponto/management/commands/reconciliar_grade.py:460::Command._relavrar_orfa
app/ponto/management/commands/supra_juiz.py:215::Command.handle
app/ponto/services/cartorio.py:1389::_julgar_colab_corpo
app/ponto/services/cartorio.py:1646::_julgar_colab_corpo

porta_alcancavel_do_dia:
app/chamados/services/vigia_fio.py:635::censo_alcance
app/escala/services/cadastro_realidade.py:488::beco_em_lote
app/folha/fora_da_folha.py:67::_porta

tarefa_do_dia:
app/chamados/management/commands/backfill_fio_morto_e4b.py:87::ato_do_dia
app/chamados/management/commands/diff_e3_sombra.py:125::Command.comparar

carga_da_tarefa:
app/chamados/management/commands/diff_e3_sombra.py:115::Command.comparar
app/chamados/services/tarefa_do_dia.py:569::tarefa_do_dia

CargaDaTarefa.tarefa:
app/chamados/services/tarefa_do_dia.py:572::tarefa_do_dia

cobranca_viva_do_dia:
app/chamados/models.py:390::ChamadoColaborador._plano_pergunta_viva
app/chamados/models.py:749::ChamadoColaborador._gate_premissa_do_fechamento
app/ponto/services/cartorio.py:1655::_julgar_colab_corpo
app/ponto/services/entrada_sincronizada.py:29::resolver_cobranca_da_entrada
app/ponto/services/triagem_batida.py:590::_autoresolve_f2_f4

motivos_a_perguntar:
app/chamados/management/commands/disparar_perguntas_competencia.py:115::contador_fio_mudo
app/chamados/services/fabrica_da_celula.py:207::perguntar_pela_celula
app/chamados/services/fabrica_da_celula.py:242::motivos_que_o_evento_deixou

montar_chamados_fio:
app/chamados/services/fio.py:446::contexto_parcial_fio
app/chamados/views.py:569::meu_atendimento
app/chamados/views.py:677::fio_colaborador
app/chamados/views.py:889::modal_fio_colaborador._resp_modal_full

creditos_perguntas:
app/chamados/services/fio.py:390::montar_chamados_fio
app/chamados/services/verbos.py:34::particionar_verbos

pergunta_sem_acao_possivel:
app/chamados/services/fio.py:284::montar_chamados_fio

relatorios_disputa.classificar:
app/chamados/relatorios_disputa.py:47::coletar

cobranca._calados:
app/chamados/services/cobranca.py:257::linhas_para_a_pauta
```

Escritores ORM dentro de `tarefa_do_dia.py`: nenhum candidato encontrado pelos nomes `save/create/update/delete/bulk_create/bulk_update/get_or_create/update_or_create`, salvo seis `.update` de dicionários em memória. `app/chamados/services/tarefa_do_dia.py:205`, `:214`, `:249`, `:252`, `:333`, `:553`. O carregador monta os insumos, e a tarefa retorna o resultado: `app/chamados/services/tarefa_do_dia.py:361`, `:523`, `:572`.

Candidatos relevantes a leitura indireta C6:

```text
app/api/views.py:2543::api_disputa_perguntas._item -> p.estado
app/chamados/management/commands/arquivar_competencia_encerrada.py:158::Command._disputa_viva -> p.viva
app/chamados/services/explicador.py:72::explicar_pergunta -> p.estado
app/chamados/services/fio.py:614::processar_post_fio -> _p.estado
app/chamados/services/vigia_fio.py:311::dono_fora_do_fio -> p.estado
app/chamados/services/cobranca.py:229::_calados -> PerguntaDisputa.q_vivas
app/chamados/services/fio.py:224::montar_chamados_fio -> PerguntaDisputa.q_vivas
app/chamados/services/janela_colab.py:63::chamados_visiveis_colab -> PerguntaDisputa.q_vivas
app/chamados/services/pergunta_no_app.py:50::perguntas_do_app -> PerguntaDisputa.q_vivas
app/chamados/services/raiox.py:74::creditos_perguntas -> PerguntaDisputa.q_vivas
app/chamados/services/vigia_fio.py:748::viva_em_disputa_fechada -> PerguntaDisputa.q_vivas
app/ponto/services/veredito_celula.py:181::fechar_o_fio_do_dia -> PerguntaDisputa.q_vivas
```

`q_vivas` declara contenção generosa, não igualdade com o C6, e encaminha quem precisa da resposta exata a `pergunta_viva`/`.viva`. `app/chamados/models.py:3328`, `:3340`, `:3343`. O template lê `.viva`, mas também carimbos crus nos ramos de resposta. `app/templates/chamados/partials/_card_atendimento.html:77`, `:94`, `:155`.

O gate **ampliado**, incluindo todos os consumidores de C6/fio_vivo, cruza o anexo LASTRO nestes três arquivos: `app/colaboradores/services/calendario.py:476`, `app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/cartorio.py:1389`. A presença dos três no anexo é dado fornecido pelo usuário [nao verificado]. Isso precisa ficar separado do gate estrito dos arquivos MUDA+respondedores+testes; não faz desses três automaticamente arquivos a alterar.

Os 309 sítios de chamada em testes, agrupados por arquivo, abaixo. As funções completas são impressas pelo `TEST_FILE` do script integral enviado. [nao verificado]

```text
app/api/tests/test_api_mensageria_pendencias.py:78 [1]
app/chamados/tests/test_adesao_por_colab.py:61 [1]
app/chamados/tests/test_b2d_resposta_visivel.py:76,99 [2]
app/chamados/tests/test_bug117_marco_no_espelho.py:128,135,143,153 [4]
app/chamados/tests/test_c2_endpoints_janela.py:37,45 [2]
app/chamados/tests/test_c7_red_minimos.py:124 [1]
app/chamados/tests/test_clique_morto.py:67,76,91,92,99,109 [6]
app/chamados/tests/test_competencia_trancada.py:127,134,156,201 [4]
app/chamados/tests/test_contract_pergunta_viva.py:56 [1]
app/chamados/tests/test_cq_c8.py:37,39,40 [3]
app/chamados/tests/test_e1_entradas.py:152 [1]
app/chamados/tests/test_e1_v10.py:229,239 [2]
app/chamados/tests/test_e1_v15c2_descarimbo_efeito.py:44,65,79 [3]
app/chamados/tests/test_e1_v6_horizonte.py:470,476,538,550 [4]
app/chamados/tests/test_e1_v9.py:171,202 [2]
app/chamados/tests/test_e2_pergunta_so_ao_c6.py:95,103 [2]
app/chamados/tests/test_e2_tarefa_do_dia.py:139,321,443,446,523,547,611,613,613,683,684,684,883,884,895,907,908,909,910,933,936,1134,1169,1170 [24]
app/chamados/tests/test_e3_c8c_orm.py:40,45,46,57,65,66,66,76,90 [9]
app/chamados/tests/test_e3_portas_tarefa.py:19,109,120,121,122,124 [6]
app/chamados/tests/test_e4a2_nunca_bateu.py:158 [1]
app/chamados/tests/test_e4a_relogio_injetado.py:34,35 [2]
app/chamados/tests/test_e4b_c11_orm.py:145,169 [2]
app/chamados/tests/test_e4b_c12_orm.py:61,93 [2]
app/chamados/tests/test_e4b_c4_orm.py:473 [1]
app/chamados/tests/test_e4b_c6_orm.py:80,81,82,83 [4]
app/chamados/tests/test_exibicao_so_viva.py:76 [1]
app/chamados/tests/test_f1c4_3_emissores.py:78 [1]
app/chamados/tests/test_fechar3_retorno_do_gate.py:100,108,139 [3]
app/chamados/tests/test_fio_ancora_destino.py:60,68,79 [3]
app/chamados/tests/test_fio_explicacao.py:33 [1]
app/chamados/tests/test_fio_le_o_juiz.py:103 [1]
app/chamados/tests/test_hx_pergunta_viva_topo.py:43 [1]
app/chamados/tests/test_hx_tipo_pelo_marco.py:161,163 [2]
app/chamados/tests/test_invariantes_disputa.py:145 [1]
app/chamados/tests/test_jv3_reconciliar_vira_vigia.py:151 [1]
app/chamados/tests/test_limbo_nao_fecha_com_filho_no_defeito.py:85,127 [2]
app/chamados/tests/test_migrar_data_evento_dia_do_turno.py:104,119 [2]
app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:150,238,265 [3]
app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:98,98 [2]
app/chamados/tests/test_o261_fabrica_le_a_lampada.py:57 [1]
app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:98,99,104,111,184 [5]
app/chamados/tests/test_o261_lavra_le_a_lampada.py:138,163 [2]
app/chamados/tests/test_o261_pergunta_viva_teto.py:55,60,65,89 [4]
app/chamados/tests/test_o261_recusa_fala.py:105,127,132,177,183 [5]
app/chamados/tests/test_o261_resposta_vazia.py:113 [1]
app/chamados/tests/test_o261_vigia_fio.py:134,135,238,241 [4]
app/chamados/tests/test_o261e_f1_porta_encerramento.py:75,436,446 [3]
app/chamados/tests/test_o261e_f2_fila.py:246 [1]
app/chamados/tests/test_o261e_f2_porta_carimbo.py:668,731 [2]
app/chamados/tests/test_o261e_f2_ramo_e_i20.py:186,202,237,260 [4]
app/chamados/tests/test_o261e_f5b_juiz_porta.py:81,106,188,189 [4]
app/chamados/tests/test_o318_card_nao_oferece_o_que_a_porta_recusa.py:102,222,237 [3]
app/chamados/tests/test_o318_dia_misto_projecao_por_marco.py:123,219,230 [3]
app/chamados/tests/test_o44_tranca_nao_barra_o_toque.py:177,183 [2]
app/chamados/tests/test_orfa_sem_candidato.py:70 [1]
app/chamados/tests/test_orfas_pelo_evento.py:102,110,354,378 [4]
app/chamados/tests/test_particionar_verbos.py:39 [1]
app/chamados/tests/test_pergunta_do_dia_em_curso.py:89,90,92,99,289,316,457,507 [8]
app/chamados/tests/test_pergunta_no_app.py:85 [1]
app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:54 [1]
app/chamados/tests/test_pergunta_viva.py:18,22,27,36,42,47,51,60,67,72,77 [11]
app/chamados/tests/test_porta_veredito_pergunta.py:427,428,438,454 [4]
app/chamados/tests/test_profile_e53.py:86 [1]
app/chamados/tests/test_pvmuda_carimbo_em_voo.py:76,90,116,125,127,275,281,282 [8]
app/chamados/tests/test_reabrir_orfas_enterradas.py:103 [1]
app/chamados/tests/test_reabrir_pendente_pelo_juiz.py:67,68 [2]
app/chamados/tests/test_reconciliador_c1_escopo.py:48 [1]
app/chamados/tests/test_relatorio_pendencias.py:35,39,43,47,53,59 [6]
app/chamados/tests/test_relatorio_pendencias_lei.py:22,31 [2]
app/chamados/tests/test_resposta_nao_e_fantasma.py:86 [1]
app/chamados/tests/test_selo_celula_absolveu.py:72,85,117 [3]
app/chamados/tests/test_selo_defer_json_creditos.py:80,94 [2]
app/chamados/tests/test_selo_emissor_fio_mudo.py:269 [1]
app/chamados/tests/test_selo_get_nao_escreve.py:88,101 [2]
app/chamados/tests/test_selo_i6_ouve_a_lei.py:114,130 [2]
app/chamados/tests/test_selo_i8_fio_solto.py:95,119,167,179 [4]
app/chamados/tests/test_selo_i8b_resolvedora_cruzada.py:83 [1]
app/chamados/tests/test_selo_identidade_fio_explicacao.py:78,82 [2]
app/chamados/tests/test_selo_juizes_orfaos.py:73,79 [2]
app/chamados/tests/test_selo_memo_pergunta_viva.py:86,87,88,108,109,110,112,124,126,129,132,137,140,148,150,153,156,164,166,174,176,177 [22]
app/chamados/tests/test_snc2_toque_em_lote.py:85,95,98,144,146,148,161,163,179,181,195,197,220,234,238 [15]
app/chamados/tests/test_t34_furo_sob_limbo.py:55 [1]
app/chamados/tests/test_tranca_de_verdade.py:52,59,82,87 [4]
app/chamados/tests/test_viva_delega_ao_juiz.py:42 [1]
app/colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py:56 [1]
app/core/tests/test_selo_performance.py:480,482 [2]
app/escala/tests/test_f3_beco1_folga_na_lista.py:80,148,173,191,193 [5]
app/ponto/tests/test_bug114_fechador_de_dois_dias.py:239 [1]
app/ponto/tests/test_cobparc_marco_no_periodo.py:386 [1]
app/ponto/tests/test_cods_do_ato.py:124 [1]
app/ponto/tests/test_cxr_tela_le_o_motor.py:367,368 [2]
app/ponto/tests/test_fechar3_troca_escala_gate.py:152,203,204,253,254 [5]
app/ponto/tests/test_hx_cartorio_ressuscita.py:56,65 [2]
app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:102 [1]
app/ponto/tests/test_o261_cobrado_le_o_fio.py:134,147,161,195,309,325,349,419 [8]
app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:84,91,150,175,184 [5]
app/ponto/tests/test_x1_folga_sem_adesao.py:325,391 [2]
```

O script não encontrou chamadas desses alvos em arquivos `bin/`; isso é resultado da execução sem relatório persistido [nao verificado]. Referências textuais em shell/documentação não equivalem a arquivo compartilhado nem a chamada Python provada.

Limites: `.viva/.estado` de modelos diferentes aparecem entre os 214 candidatos; não declarar 214 leitores C6. Dispatch dinâmico, atribuições em tupla e consumidores de ordem superior não estão provados [nao verificado]. AST Python não julga HTML/JS: o ramo do template requer leitura textual; o script imprime o restante para inspeção.

## 12/11 08:55 · sessao 66539aafd139 · - · - · origem=Ronald (tmux)

Censo escala/folgas/humano concluído em leitura; AST rodou com `python3 -B - <<'PY'`, sem importar Django. Evidência de chamadas abaixo. [nao verificado]

| Porta / chamador | Janela pedida e guarda | Futuro já gerado |
|---|---|---|
| `salvar_tipo` → `_propagar_regeneracao_template`; wizard `app/escala/views_wizard.py:108`, aceite do limbo `app/chamados/services/limbo_folgas.py:92` | Edição chama propagação em `app/escala/services/cadastro_tipo.py:307`; somente EC `ativa=True`, início `max(ec.data_inicio,início competência anterior)`, guarda **`desde <= hoje`**, chamada `[desde,hoje]` em `app/escala/services/cadastro_tipo.py:334` e `app/escala/services/cadastro_tipo.py:338` e `app/escala/services/cadastro_tipo.py:348`. | Se passa guarda, a porta estende até fim da competência atual; EC que começa amanhã é pulada. Dinheiro permanece `recalculo=False`, invalidando fechamento depois. `app/escala/services/cadastro_tipo.py:350`, `app/escala/services/cadastro_tipo.py:358`, `app/ponto/portas/celula.py:187`. |
| Signal `_ec_vigencia_regenera_celulas` (`post_save` EC) | Ignora criação/raw; quando início/fim mudou: desde menor início anterior/atual, até maior fim anterior/atual/hoje. `app/escala/signals.py:34`, `app/escala/signals.py:54`, `app/escala/signals.py:60`, `app/escala/signals.py:67`, `app/escala/signals.py:71`. | Regenera geradas dentro da janela efetiva; sem guarda `<=hoje` nesse signal. `app/escala/signals.py:71`, `app/ponto/portas/celula.py:263`. |
| Signal `_ec_ancora_regenera_celulas` | Ignora criação/raw/âncora nula/igual; desde `max(início competência atual,ec.data_inicio)`; até `max(fim competência atual,ec.data_fim,hoje)`. `app/escala/signals.py:79`, `app/escala/signals.py:104`, `app/escala/signals.py:114`, `app/escala/signals.py:117`. | Alcança futuro da janela; sem guarda `<=hoje`. `app/escala/signals.py:117`, `app/ponto/portas/celula.py:263`. |
| `gerar_foto_folgas` → `_gerar_foto_folgas_no_ato` | Foto + relavra em mesmo atomic; pede mês civil inteiro. `app/escala/folgas.py:150`, `app/escala/folgas.py:162`, `app/escala/folgas.py:200`, `app/escala/folgas.py:203`. | Regenera geradas, ampliando ao horizonte atual. `app/escala/folgas.py:272`, `app/ponto/portas/celula.py:187`, `app/ponto/portas/celula.py:263`. |
| `lancar_folga` / `alterar_folga` | Pedem `[dia,dia]` via `relavrar_celulas_da_foto`; essa usa extensão de horizonte por padrão. `app/escala/folgas.py:314`, `app/escala/folgas.py:357`, `app/escala/folgas.py:276`. | Futuro gerado dentro da janela efetiva é regenerado; também podem criar células inexistentes até horizonte. `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:263`. |
| `aplicar_folgas_em_lote` | Recusa dias anteriores ao início da competência atual; pede `[min(gravados),max(gravados)]`, com extensão padrão. `app/escala/folgas.py:379`, `app/escala/folgas.py:385`, `app/escala/folgas.py:395`. | Futuro alcançado é regenerado. `app/escala/folgas.py:395`, `app/ponto/portas/celula.py:263`. |
| `reparentar_folga` | `[fd.data,fd.data]`; se dia anterior à competência corrente, `so_rejulgar=True` produz intervalo vazio e desliga extensão. `app/escala/folgas.py:500`, `app/escala/folgas.py:502`, `app/escala/folgas.py:267`, `app/escala/folgas.py:276`. | Futuro da competência corrente em diante relavra com extensão. Escolha de alvo depende de `vinculo_do_dia_carregado`, podendo sofrer a célula velha. `app/escala/folgas.py:470`, `app/escala/folgas.py:500`. |
| `aplicar_folga_calendario` view | EC ativas do tipo; gera fotos mês corrente + próximo e relavra explícita `[1º corrente,último próximo]`. `app/escala/views.py:392`, `app/escala/views.py:409`, `app/escala/views.py:411`, `app/escala/views.py:413`, `app/escala/views.py:416`. | Regenera futuro gerado e cria faltantes nessa janela ampla. `app/escala/views.py:416`, `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:263`. |
| `aplicar_no_vinculo` / `desfazer` ESCALA-AUTO | Aplicação exige início `>hoje`; desfazer volta `max(início,amanhã)`; até última célula já gerada desde início, no mínimo início. `app/escala/services/escala_auto_executor.py:52`, `app/escala/services/escala_auto_executor.py:57`, `app/escala/services/escala_auto_executor.py:100`, `app/escala/services/escala_auto_executor.py:104`, `app/escala/services/escala_auto_executor.py:140`, `app/escala/services/escala_auto_executor.py:143`. | Alcança última célula futura existente (mais extensão padrão); command chama aplicação/desfazer. `app/escala/management/commands/ajustar_escalas_auto.py:39`, `app/escala/management/commands/ajustar_escalas_auto.py:95`. |

Chamadores adicionais das portas folga:

- Plano individual → lançar/alterar: `app/escala/views_plano_folgas.py:131`, `app/escala/views_plano_folgas.py:136`; lote → aplicar lote: `app/escala/views_plano_folgas.py:185`; declaração do dia → lançar: `app/ponto/services/declarar_dia.py:191`.
- Vínculo escala-folgas: troca calendário e gera fotos corrente/próximo: `app/colaboradores/views.py:239`, `app/colaboradores/views.py:245`.
- Herança de calendário na criação/troca: `app/colaboradores/services/vinculo.py:164`, `app/colaboradores/services/vinculo.py:165`.
- Signal de criação/mudança vigência → reparentamento: `app/escala/signals.py:172`, `app/escala/signals.py:183`, `app/escala/signals.py:190`; command manual → mesma porta: `app/escala/management/commands/reparentar_folgas.py:34`.
- Command `lavrar_foto_folgas --apply` → foto: `app/escala/management/commands/lavrar_foto_folgas.py:159`, `app/escala/management/commands/lavrar_foto_folgas.py:170`; meses das competências atual + seguinte: `app/escala/management/commands/lavrar_foto_folgas.py:74`.

Lacunas de portas que não regeneram:

- Editor de calendário salva calendário/tamanhos e retorna, sem materializar fotos nem regenerar: `app/escala/folgas.py:548`, `app/escala/folgas.py:566`, `app/escala/folgas.py:574`; view chama somente esse serviço: `app/escala/views.py:356`. Wizard cria calendário também sem propagação: `app/escala/folgas.py:577`, `app/escala/views_wizard.py:171`, `app/escala/views_wizard.py:192`.
- Admin de TipoEscala é `admin.ModelAdmin` direto, sem override e sem chamar `salvar_tipo`: `app/escala/admin.py:7`; EC no admin é `SoLeitura`: `app/escala/admin.py:13`. Portanto não contar admin TipoEscala como porta comprovadamente curada pela propagação. `app/escala/admin.py:8`.

Porta comum, diferenças essenciais:

- `regenerar_celulas_vinculo` usa janela `[desde,max(ate,fim competência atual)]` por padrão, não limita laço a hoje: `app/ponto/portas/celula.py:187`, `app/ponto/portas/celula.py:219`, `app/ponto/portas/celula.py:221`.
- Dentro da vigência cria célula inexistente e reescreve somente `origem='gerada'` se trabalha/geradora/DNA diferiu, guardando DNA anterior/carimbos: `app/ponto/portas/celula.py:258`, `app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:271`.
- Fora da vigência **não apaga**: se a geradora é esse EC, gerada, ainda acesa, sem lavra e sem outro vínculo cobrindo, zera trabalho/DNA; outro vínculo cobrindo faz preservar até a regeneração desse outro. `app/ponto/portas/celula.py:238`.
- Guarda de futuro `if _d > _hoje_j` em `app/ponto/portas/celula.py:431` limita **julgamento**, não escrita de DNA. Guarda equivalente do `rejulgar` em `app/ponto/portas/celula.py:300`.
- Docstring `CelulaDia` diz regeneração só `data>=hoje`, porém o corpo vivo admite passado pela exceção formal: comentário `app/escala/models.py:1641` versus código `app/ponto/portas/celula.py:219` e documentação formal `app/escala/models.py:1705`.

Proteção humana:

- `editar_dia` exige autor/motivo, grava `origem='editada'`, `editada_em`, `editada_por`, e trilha; idempotência não move carimbo: `app/ponto/portas/celula.py:559`, `app/ponto/portas/celula.py:594`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:606`, `app/ponto/portas/celula.py:611`.
- Regeneração só reescreve gerada; gerador pula editada; reversão só gerada: `app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:859`, `app/ponto/portas/celula.py:777`.
- `trocar_plantao` chama `editar_dia` para cada participante/dia: `app/ponto/portas/celula.py:675`. AST de runtime não encontrou outro chamador de `editar_dia`/`trocar_plantao`; tela concreta de edição humana [não verificado].

Outros escritores/correções (separar de cadastro cotidiano):

- Correção retroativa command pede piso declarado/default desde, topo `min(fim,hoje)`, guarda piso<=topo, mas porta estende depois: `app/colaboradores/management/commands/corrigir_escala_retroativa.py:225`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:227`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:230`.
- Retificação exportada segmenta intervalo por vínculo do cadastro; extensão apenas se EC ativa: `app/ponto/portas/regen_exportada.py:145`, `app/ponto/portas/regen_exportada.py:174`.
- Regenerar um dia desliga extensão: `app/ponto/portas/celula.py:734`; callers de manutenção `app/ponto/management/commands/aplicar_col900.py:232`, `app/ponto/management/commands/diff_col900.py:69`; reversão em `app/ponto/management/commands/aplicar_col900.py:184`.
- Correção resíduo BUG115 chama gerador `[d,d]`: `app/ponto/management/commands/curar_residuo_bug115.py:135`.
- Operações restantes da porta são veredito/ata, carimbos/insumos/furos, não propagadores de cadastro DNA: registro `app/ponto/portas/celula.py:74`; saves veredito `app/ponto/portas/celula.py:905`; bulk insumos `app/ponto/portas/celula.py:1108`.

Contradição importante com descrição do cron no aval: gerador **atualiza célula futura existente**, além de criar inexistente; condição `d<hoje` separa passado, ramo else reconstrói trabalha/geradora/DNA: `app/ponto/portas/celula.py:853`, `app/ponto/portas/celula.py:861`, `app/ponto/portas/celula.py:870`, `app/ponto/portas/celula.py:876`.

## 12/11 08:54 · sessao f0253d30c824 · - · - · origem=Ronald (tmux)

Achado decisivo para não repetir a hipótese do aval como fato do HEAD: `regenerar_celulas_vinculo` estende `ate` até o fim da competência corrente por padrão; portanto `ate=hoje` não significa, sozinho, que o futuro fica velho. As guardas que impedem entrar na porta e os filtros de universo são o problema demonstrável por código. (`app/ponto/portas/celula.py:186`, `app/ponto/portas/celula.py:190`, `app/ponto/portas/celula.py:192`)

| Porta / origem | Janela e guarda | Efeito sobre futuro existente |
|---|---|---|
| `executar_vinculo`, troca de escala/template/âncora no vínculo (`app/colaboradores/services/vinculo.py:168`) | Pede `[data_inicio, hoje]`, **somente** se `data_inicio <= hoje` (`:315`, `:316`, `:321`). Depois a porta estende ao fim da competência (`app/ponto/portas/celula.py:190`). | Início amanhã: não chama regeneração explícita; criação é ignorada pelos signals de regeneração, que retornam em `created` (`app/escala/signals.py:54`, `:104`). Futuro já gerado pode permanecer com a geradora antiga. Início até hoje: células futuras na extensão são regeneradas se `origem='gerada'` e houver diferença (`app/ponto/portas/celula.py:263`). |
| Signal `_ec_vigencia_regenera_celulas`, `post_save EscalaColaborador` (`app/escala/signals.py:34`) | Ignora criação/raw (`:54`); alteração de início/fim: desde `min(início anterior, novo)` (`:60`), até `max(fim anterior, novo fim, hoje)` (`:67`), com extensão padrão (`:71`). | Reescreve futuras existentes alcançadas, preservando humanas. Fora da vigência, não apaga: zera trabalho/DNA somente se a célula era desse vínculo e nenhum outro cobre (`app/ponto/portas/celula.py:238`, `:242`, `:243`). |
| Signal `_ec_ancora_regenera_celulas`, `post_save EscalaColaborador` (`app/escala/signals.py:79`) | Ignora criação/raw (`:104`) e âncora nova nula/igual (`:108`); desde `max(início competência atual, início vínculo)` até `max(fim competência, fim vínculo, hoje)` (`:114`, `:115`, `:116`, `:117`). | Atualiza futuras geradas na janela; não há guarda `<=hoje` no signal. |
| `marcar_trabalha_em_feriado` (`app/colaboradores/services/vinculo.py:823`) | Idempotente (`:844`); desde `max(início vínculo, início competência anterior)` (`:849`, `:853`, `:854`), **somente** se `desde <= hoje` (`:855`), pede até hoje (`:859`) e recebe extensão padrão. | Vínculo já iniciado alcança futuro da competência. Vínculo começando depois de hoje é salvo (`:848`) mas a regeneração é pulada (`:855`). |
| `reavaliar_pela_troca_de_municipio` (`app/colaboradores/services/vinculo.py:871`) | Universo = vínculos do posto (`:900`) que **cobrem hoje** (`:902`). Pede `[hoje, hoje]` (`:904`), com extensão padrão. | Vínculos futuros são excluídos; vínculo corrente atualiza futuro até o horizonte, sem recalcular dinheiro por pessoa (`:905`); invalida fechamento de hoje (`:906`). |
| Comando `corrigir_escala_retroativa::_aplicar` (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:164`) | `piso = regenerar_desde ou desde` (`:225`); `topo=min(novo_fim ou hoje, hoje)` (`:227`); guarda `piso <= topo` (`:228`); chama regeneração com extensão padrão (`:230`). | Pode atualizar futuro até fim da competência, apesar de a mensagem imprimir só piso/topo (`:234`). Piso futuro é pulado (`:240`). |
| Troca de `Colaborador.posto` | Evento de célula removido expressamente: o lugar é do posto do vínculo do dia (`app/colaboradores/services/colaborador.py:47`, `:49`, `:50`); `gravar` apenas salva (`:61`). | Não regenera; isso é comportamento documentado, não o mesmo evento da mudança do município do posto (`app/colaboradores/services/colaborador.py:50`). |
| Cadastro de `Feriado` | View edição `.save()` (`app/core/views_config.py:68`), criação `.create()` (`:81`), importação `.get_or_create()` (`:141`). Signal só agenda `reavaliar_dia(data)` (`app/chamados/signals.py:326`, `:335`, `:336`, `:337`). | O evento encontrado fecha chamados, não regenera células: `reavaliar_dias` filtra chamados (`app/chamados/services/feriado_reavaliar.py:77`) e chama `encerrar` (`:84`). Não há atualização de futuro materializado por esse caminho. |
| Exclusão de `Feriado` | `.delete()` (`app/core/views_config.py:198`); ausência deliberada de `post_delete` (`app/chamados/signals.py:322`, `:323`). | Nenhuma regeneração nesse caminho; reabertura dos chamados também não ocorre por esse evento (`app/chamados/signals.py:323`). |

Chamadores das portas:

- `executar_vinculo`: view `vincular_escala` (`app/colaboradores/views.py:373`, `:423`); serviço de chat `trocar_escala_porta` (`app/colaboradores/services/troca_pelo_chat.py:64`, `:75`); realocação pela régua `realocar_vinculo_para_destino` (`app/escala/regua_defesa.py:291`, `:321`); comando `censo_fase_12x36` (`app/ponto/management/commands/censo_fase_12x36.py:423`).
- `marcar_trabalha_em_feriado`: comando `sincronizar_feriado_admin`, somente `--apply`; percorre vínculos ativos da empresa (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:42`, `:53`, `:56`). Não há chamada direta encontrada em view; exhaustividade além do censo estático deve ser marcada [não verificado].
- Município: `_evento_do_municipio` compara antes/depois e chama reavaliação (`app/colaboradores/services/estrutura.py:52`, `:58`, `:60`); acionado por `atualizar_posto` (`:64`, `:69`) na view `form_posto` (`app/colaboradores/views.py:714`, `:862`), e `definir_municipio_posto` (`app/colaboradores/services/estrutura.py:86`, `:93`) no comando `backfill_municipio_posto` (`app/colaboradores/management/commands/backfill_municipio_posto.py:93`).
- Troca calendário do vínculo: `trocar_calendario_de_folga` apenas salva calendário (`app/colaboradores/services/vinculo.py:434`, `:446`); view `vincular_folga` chama essa porta e materializa mês corrente e próximo (`app/colaboradores/views.py:212`, `:239`, `:243`, `:245`, `:246`); outro caller em lote é `app/escala/views.py:409`. A regeneração indireta está em `escala/folgas.py`, já fora deste recorte delegado.
- `encerrar_vinculo` salva `ativa/data_fim` e atravessa signal de vigência (`app/colaboradores/services/vinculo.py:781`, `:800`); `fechar_vigencia` usa `update`, sem esses signals por construção (`app/colaboradores/services/vinculo.py:562`, `:590` e comentário da família `:431`).
- Admin Django: `EscalaColaboradorAdmin` é `SoLeitura` (`app/escala/admin.py:13`); `PostoAdmin` (`app/colaboradores/admin.py:33`) e `ColaboradorAdmin` (`:48`) também. `TipoEscalaAdmin` continua `ModelAdmin` simples (`app/escala/admin.py:8`); verificar evento desse caminho junto ao censo de template.

## 12/11 08:52 · sessao 51063f8f4d10 · - · - · origem=Ronald (tmux)

Os leitores atuais continuam fora de `tarefa_do_dia`; os três chamadores C17 são `_porta`, `beco_em_lote` e `censo_alcance`, confirmados por execução em memória do detector AST existente. A catraca atual exige exatamente esses três chamadores e precisará mudar no diff. (`app/chamados/tests/test_e3_pendentes_app.py:19`, `app/chamados/tests/test_e3_pendentes_app.py:33`, `app/chamados/tests/test_e3_pendentes_app.py:145`)

| Caminho | Sítio e resposta atual | Divergência observável no código |
|---|---|---|
| Fio | `app/chamados/services/fio.py:163::montar_chamados_fio`: monta cartões, portas, perguntas, rótulos e explicação | Perguntas vivas pelo C6, ação possível por `raiox`, regularização por `declaracao_texto` e desenlace por outro serviço; não consulta tarefa. (`app/chamados/services/fio.py:237`, `app/chamados/services/fio.py:279`, `app/chamados/services/fio.py:283`, `app/chamados/services/fio.py:375`, `app/chamados/services/fio.py:403`) |
| Tela admin — fio completo | `app/chamados/views.py:659::fio_colaborador` | Chama `montar_chamados_fio` sem explicação. (`app/chamados/views.py:676`) |
| Tela admin — modal | `app/chamados/views.py:869::modal_fio_colaborador`, função interna `_resp_modal_full` em `:882` | Chama `montar_chamados_fio(..., com_explicacao=True)`, logo alcança o cache defeituoso do item 11. (`app/chamados/views.py:889`, `app/chamados/services/fio.py:312`) |
| Tela admin — drawer | `app/colaboradores/views.py:915::painel_colaborador` | “No app dele agora” vem de `projecao_do_app`, não tarefa. (`app/colaboradores/views.py:940`, `app/templates/colaboradores/partials/painel_colaborador.html:190`) |
| Tela admin — ficha | `app/colaboradores/services/ficha.py:99::ficha_do_colab` | Monta verbos/dono pelos chamados e separa perguntas sem resposta por texto/`respostas_sem_veredito`. (`app/colaboradores/services/ficha.py:154`, `app/colaboradores/services/ficha.py:159`, `app/colaboradores/services/ficha.py:166`, `app/colaboradores/services/ficha.py:169`) |
| API/app — lista de chamados | `app/api/views.py:961::api_chamados_v2` | Consulta `consulta_do_app`; não tarefa. (`app/api/views.py:992`) |
| API/app — perguntas | `app/api/views.py:2519::api_disputa_perguntas` | Consulta `consulta_do_app`; não tarefa. (`app/api/views.py:2530`) |
| Badge/API, kernel 1 | `app/api/views.py:284::api_me` | Badge vem de `projecao_do_app`. (`app/api/views.py:315`) |
| Badge/API, kernel 2 | `app/api/views_core.py:272::api_me` | Mesma projeção compartilhada. (`app/api/views_core.py:303`) |
| Badge/PWA | `app/core/views.py:118::dashboard` | Conta `len(consulta_do_app(...)[pendentes])`; template só desenha número e “Responder DP”. (`app/core/views.py:252`, `app/templates/core/dashboard_colaborador_v2.html:78`) |
| Fonte compartilhada API/badge | `app/chamados/services/pergunta_no_app.py:45::perguntas_do_app`, `:61::consulta_do_app`, `:78::projecao_do_app` | Universo vem de C6; pendência exige `respondida_em is None` **e disputa aberta**. Projeção conta essas pendentes e, separadamente, chamados com nota DP não lida. (`app/chamados/services/pergunta_no_app.py:58`, `app/chamados/services/pergunta_no_app.py:71`, `app/chamados/services/pergunta_no_app.py:90`, `app/chamados/services/pergunta_no_app.py:98`) |
| `o_que_segura` | `app/folha/fora_da_folha.py:203::o_que_segura` | Para cada dia retido lê `_fio_do_dia` e `_porta`; `_porta` chama C17. (`app/folha/fora_da_folha.py:221`, `app/folha/fora_da_folha.py:225`, `app/folha/fora_da_folha.py:65`) |
| Fora da folha/contador | `app/folha/fora_da_folha.py:142::fora_da_folha` | Classifica dias via `_fio_do_dia`; fonte permanece C17. (`app/folha/fora_da_folha.py:178`, `app/folha/fora_da_folha.py:77`) |
| Cadastro realidade | `app/escala/services/cadastro_realidade.py:442::beco_em_lote` | Seleciona dia indefinido acusado, exige vivas e ausência de porta C17, depois examina âncoras e cadastro. (`app/escala/services/cadastro_realidade.py:476`, `app/escala/services/cadastro_realidade.py:487`, `app/escala/services/cadastro_realidade.py:488`, `app/escala/services/cadastro_realidade.py:493`) |
| Vigia/contador | `app/chamados/services/vigia_fio.py:542::censo_alcance` | Universos por dias retidos; C17 em `:635`; após ausência de porta decide futuro, baldes legais e causas próprias até contar `fio_morto_alcance`. (`app/chamados/services/vigia_fio.py:585`, `app/chamados/services/vigia_fio.py:635`, `app/chamados/services/vigia_fio.py:648`, `app/chamados/services/vigia_fio.py:654`, `app/chamados/services/vigia_fio.py:695`) |

Contraste com o juiz que fica: tarefa decide por ato E1, efeito/lavra, cobertura, instância e teto; isenção/tranca eliminam exigências, ausência de célula/lâmpada produz desconhecimento e marco aceso em dia de trabalho prevalece sobre pergunta. (`app/chamados/services/tarefa_do_dia.py:128`, `app/chamados/services/tarefa_do_dia.py:141`, `app/chamados/services/tarefa_do_dia.py:188`, `app/chamados/services/tarefa_do_dia.py:195`, `app/chamados/services/tarefa_do_dia.py:204`, `app/chamados/services/tarefa_do_dia.py:260`, `app/chamados/services/tarefa_do_dia.py:281`)

As quatro divergências históricas já possuem expectativas escritas nos testes: fila sem pergunta → sistema/perguntar; fila A com pergunta B → porta B; verbo B lido do universo do painel; isento com fila residual → nenhuma ação. Esses testes não provam incidência nem tamanho das classes atuais de produção. (`app/chamados/tests/test_e3_portas_tarefa.py:1`, `app/chamados/tests/test_e3_portas_tarefa.py:32`, `app/chamados/tests/test_e3_portas_tarefa.py:40`, `app/chamados/tests/test_e3_portas_tarefa.py:49`, `app/chamados/tests/test_e3_portas_tarefa.py:57`)

Para o portão, os arquivos de implementação acima e os testes diretamente pertinentes abaixo têm **0 caminhos em comum** com a lista LASTRO fornecida; essa comparação usa a lista do enunciado, sem inspeção de obra em voo. [não verificado: existência/estado da obra LASTRO] [nao verificado]

Testes candidatos do diff, com ponto verificável: [nao verificado]

- `app/chamados/tests/test_e3_pendentes_app.py:145`: substituir expectativa de três C17.
- `app/chamados/tests/test_e3_app_orm.py:28`: endpoint, dashboard e dois kernels; casos em `:47`, `:52`, `:57`, `:76`.
- `app/chamados/tests/test_e3_v4_orm.py:20`: avisos/cobrança/badges; lista nativa em `:42`.
- `app/chamados/tests/test_fio_explicacao.py:24`, `app/chamados/tests/test_selo_identidade_fio_explicacao.py:72`: explicação do fio.
- `app/chamados/tests/test_fio_ancora_destino.py:58`, `app/chamados/tests/test_fio_le_o_juiz.py:102`: montagem do fio.
- `app/chamados/tests/test_n1_alimentacao_unica.py:77`, `:111`, `:125`, `:144`: vigia, fora da folha e o_que_segura.
- `app/chamados/tests/test_o261e_f5_alcance.py:100`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:84`, `app/chamados/tests/test_o261_vigia_fio.py:799`: alcance/vigia.
- `app/folha/tests/test_haiku_fora_da_folha.py:65`, `:75`, `:100`, `:292`: consumidores da folha.
- `app/escala/tests/test_f3_beco1_folga_na_lista.py:163`, `:177`, `:205`, `:218`: beco/C17/lista/vigia.
- `app/core/tests/test_dashboard_badge.py:40`, `app/api/tests/test_api_me_badge.py:36`: badge.
- `app/colaboradores/tests/test_selo_situacao_e_lampada.py:118`, `app/colaboradores/tests/test_ficha_do_colab.py:39`: drawer/ficha.

Há consumidor adicional indireto em `app/ponto/services/dia_do_colab.py:108::_pna_do_dia`, chamando `pergunta_no_app.do_dia`; esse arquivo **está** na lista LASTRO. Ele não é alteração necessária demonstrada para os seis caminhos exigidos, mas ampliar o diff a esse consumidor mudaria o portão. (`app/ponto/services/dia_do_colab.py:107`; presença LASTRO: dado do enunciado [não verificado na obra em voo])

PWA tem service worker no caminho: `pwa.js` registra `/sw.js`; SW intercepta GET e usa rede/cache, sem decidir pendências. `badge` em `sw.js` é ícone de notificação. Não encontrei derivação do dia em `app/static/js`; necessidade de tocar esses arquivos no futuro diff = [não verificado]. (`app/static/pwa.js:14`, `app/static/sw.js:33`, `app/static/sw.js:45`, `app/static/sw.js:82`, `app/static/sw.js:94`)

`bin/` só apresentou referências descritivas ao JSON de alcance em `bin/fio_estado.py:44` e `bin/tests/test_fio_estado.sh:38`; não há arquivo de `bin/` entre os candidatos acima, portanto **0 em comum por caminho**. (`bin/fio_estado.py:44`, `bin/tests/test_fio_estado.sh:38`)

## 12/11 08:52 · sessao cc66d70d4171 · - · - · origem=Ronald (tmux)

Confirmado o defeito de universo do item 11: `app/chamados/services/fio.py::montar_chamados_fio` agrupa perguntas por cartão em `:240-244`, cacheia a explicação por disputa em `:305-306` e fornece somente `perguntas_por_chamado.get(ch.pk)` em `:312-313`. O explicador aceita essa lista sem completar a disputa (`app/chamados/services/explicador.py:121-124`), consulta `fio_vivo(ps)` e pode declarar “Tudo resolvido” (`:136-140`). A correção deve preservar um conjunto completo de perguntas por disputa e compartilhar suas instâncias; o cache por disputa precisa corresponder ao universo da disputa (`app/chamados/juizes.py:1494-1503`).

Confirmado o item 12: `app/chamados/services/fio.py:340-342` calcula `todas_respondidas` com `all(p.respondida_em ...)`, e `app/templates/chamados/partials/modal_fio.html:334-341` usa o resultado para acusar ausência de resposta e impedir fechamento sem anuência. O juiz individual é C6 `pergunta_viva`; a redução da coleção é C15 `fio_vivo`, que delega ao C6 (`app/core/juizes.py:112`, `:125`, `:278`, `:287`; `app/chamados/juizes.py:1494-1503`). A pendência do colaborador precisa excluir perguntas mortas pelo juiz; a operação de fechamento continua sob C7, cujo corpo também guarda respostas sem veredito (`app/chamados/models.py:1850-1854`).

`JUIZES.md` registra C7 em `app/docs/JUIZES.md:62`, C19 em `:78-85`, e menciona C6 em `:90`; não contém verbete independente com os endereços literais de C6/C15. O registro vivo é `app/core/juizes.py:278`, `:287`, e prevalece segundo `app/docs/JUIZES.md:14-17`. Não apresentar um verbete inexistente como se estivesse no anexo.

| Sítio | Resposta atual e chamadores | Destino para a certidão |
|---|---|---|
| `app/chamados/services/fio.py::montar_chamados_fio:163` | Flags de pergunta viva `:283-285`, timestamp cru `:340-347`, projeção de regularização `:375-378`, crédito para desenlace `:387-390`; chamado por `app/chamados/views.py:569`, `:677`, `:889`, e `app/chamados/services/fio.py:446`. | Consumir a projeção C19 para o que fazer com o dia; corrigir separadamente universo C15 da explicação e aviso C6/C15 do complemento. C19 especificado em `app/docs/JUIZES.md:78-85`. |
| `app/chamados/services/raiox.py::creditos_perguntas:51` | Pré-filtra `q_vivas` em `:74`, exclui materializada com `VL_SIM` em `:135-136`, separa respondida/não respondida pelo timestamp em `:138-140`, credita destino vivo ou pai em `:144-149`. Chamadores: `app/chamados/services/verbos.py::particionar_verbos:34` e fio `:387-390`. | Remover esse cálculo como respondedor do dia; encaminhar consumidores aos donos/ações/portas do C19 (`app/chamados/services/tarefa_do_dia.py:32-37`). |
| `app/chamados/relatorios_disputa.py::classificar:11` | Consulta C6 em `:18`, deriva categorias por `respondida_em` em `:21-23`; `coletar` chama em `:47`. Consumidores de `coletar`: `app/colaboradores/views.py:3000` e `app/chamados/management/commands/relatorio_pendencias_disputa.py:5`. | Categorias referentes a tarefa devem projetar o C19; fechamento permanece C7 (`app/docs/JUIZES.md:62`, `:78-85`). |
| `app/chamados/services/cobranca.py::_calados:221` | SQL `q_vivas + respondida NULL + disputa aberta` em `:228-230`; deriva contagens por pessoa, canal e SLA em `:232-248`. Chamador `linhas_para_a_pauta:257`, integrado em `app/escala/services/fase_12x36.py:272`. | Pendência do colaborador deve vir das ações C19; preservar canal e SLA como perguntas próprias (`app/chamados/services/tarefa_do_dia.py:32-37`; `app/docs/JUIZES.md:63`). |
| `app/templates/chamados/partials/_card_atendimento.html` | Deriva documento em `:65`, formulário por `p.viva` em `:77`, aviso de resposta em `:94`, porta por status/furo/pergunta em `:203-205`, regularização em `:212`, data pelo contexto cru em `:232-235`. Incluído nas duas listas de `app/templates/chamados/meu_atendimento.html:127`, `:144`. | Receber decisão/projeção C19 do servidor; deixar a apresentação depender dessa resposta (`app/docs/JUIZES.md:78-85`). |

Diferenças semânticas verificáveis que exigem DECISÕES técnicas, sem juiz novo:

1. Timestamp cru não representa a última decisão: C19 usa `DecisaoPergunta`, trata texto vazio e recusa/reabertura (`app/chamados/services/tarefa_do_dia.py:128-140`), enquanto os respondedores acima usam `respondida_em` (`fio.py:340`; `raiox.py:138-140`; `relatorios_disputa.py:21`; `cobranca.py:229`). Lei da lâmpada e juiz único já orientam a migração (`CLAUDE.md:173`; `app/docs/JUIZES.md:78-85`).
2. Efeito da validação precisa de lâmpada vigente; ausente, C19 devolve desconhecimento (`app/chamados/services/tarefa_do_dia.py:141-149`). O crédito atual aceita a exclusão por `VL_SIM` sem verificar validade ali (`app/chamados/services/raiox.py:135-136`).
3. Ausência aguardando documento pertence ao colaborador e outras pendências podem pertencer ao admin (`app/chamados/services/tarefa_do_dia.py:152-160`, `:281-289`); o template decide documento pelo contexto do chamado (`app/templates/chamados/partials/_card_atendimento.html:65-70`).
4. Dia isento e trancado recebem ações vazias em C19 (`app/chamados/services/tarefa_do_dia.py:188-203`); os critérios SQL dos respondedores não são uma leitura dessas respostas (`app/chamados/services/cobranca.py:228-230`; `app/chamados/services/raiox.py:135-140`).
5. Universo do cache precisa acompanhar sua chave; explicação de disputa não pode usar o primeiro cartão (`app/chamados/services/fio.py:305-314`; `app/chamados/services/explicador.py:121-124`). O contrato C15 declara explicitamente que o universo pertence ao chamador (`app/chamados/juizes.py:1500-1501`).

Não confundir os cinco sítios enumerados no aval com os cinco nomes históricos da declaração C19: o registro enumera `cobranca_viva_do_dia`, `pergunta_viva`, `motivos_a_perguntar`, `porta_alcancavel_do_dia`, `dono_fora_do_fio` (`app/core/juizes.py:153-154`). A obra não autoriza eliminar C6 globalmente; ele continua registrado e alimenta o C19 (`app/core/juizes.py:278`; `app/chamados/services/tarefa_do_dia.py:174-175`, `:241-245`).

Testes prioritários identificados: [nao verificado]

- `app/chamados/tests/test_fio_explicacao.py:22-37`: cobre uma pergunta em um cartão; não demonstra universo multidia.
- `app/chamados/tests/test_selo_identidade_fio_explicacao.py:51-60`: cria uma disputa por cartão; `:93-106` compara explicador alimentado com lista completa. Preservar identidade ao corrigir o cache.
- `app/chamados/tests/test_relatorio_pendencias.py:35-59`: categorias atuais.
- `app/chamados/tests/test_o318_card_nao_oferece_o_que_a_porta_recusa.py:103`, `:163`, `:265-271`: render e guarda atual do cartão.
- `app/chamados/tests/test_o318_dia_misto_projecao_por_marco.py:124`, `:276`: render/projeção do cartão.
- `app/chamados/tests/test_o261_fio_tem_uma_porta.py:100-127`: catraca de redução C6 fora de C15.
- `app/docs/LEIS.md:98-99`: L-071/L-072; `:137`: L-111; `:148`: L-127.

Os REDs reais 34390 e 35602/pergunta 51407 estão apenas no dado fornecido; reexecução e fixture completa estão [nao verificado]. Os demais REDs reais por leitor e por classe V/T precisam da sonda [aguarda sonda]; o dado fornecido não contém seus identificadores [nao verificado].

## 12/11 08:51 · sessao 8e729553ce73 · - · - · origem=Ronald (tmux)

Mapa dos leitores MUDA nesta cópia:

| Sítio | Resposta/cadeia atual | Deriva ou lê tarefa |
|---|---|---|
| `app/chamados/services/fio.py::montar_chamados_fio` | Fio do colaborador e admin: perguntas agrupadas por chamado; `tem_pergunta_viva` usa `.viva` + `raiox.pergunta_sem_acao_possivel`; `todas_respondidas` usa carimbo cru; explicação cacheada por disputa recebe perguntas do cartão. | Não lê tarefa; decisões próprias em `app/chamados/services/fio.py:243`, `:283`, `:312`, `:340`. |
| `app/chamados/views.py::meu_atendimento` | Tela colaborador chama `montar_chamados_fio(..., so_janela=True)`. | Casca de fio existente, `app/chamados/views.py:559`, `:566`, `:569`. |
| `app/chamados/views.py::fio_colaborador` | Tela admin chama `montar_chamados_fio(colaborador)`; modal chama com explicação. | Casca de fio existente, `app/chamados/views.py:659`, `:676`, `:869`, `:889`. |
| `app/chamados/services/fio.py::contexto_parcial_fio` | Re-render do modal chama montagem com explicação. | Mesmo leitor após ação, `app/chamados/services/fio.py:437`, `:446`. |
| `app/colaboradores/views.py::painel_colaborador` | “No app dele agora” usa `projecao_do_app`, exibida em partial admin. | Não lê tarefa, `app/colaboradores/views.py:915`, `:940`; `app/templates/colaboradores/partials/painel_colaborador.html:190`. |
| `app/api/views.py::api_me` e `app/api/views_core.py::api_me` | Badge/API usam `projecao_do_app`. | Não leem tarefa, `app/api/views.py:315`; `app/api/views_core.py:303`. |
| `app/api/views.py::api_chamados_v2` | GET lê `consulta_do_app`. | Não lê tarefa, `app/api/views.py:991`. |
| `app/api/views.py::api_disputa_perguntas` | Pendentes/histórico vêm de `consulta_do_app`. | Não lê tarefa, `app/api/views.py:2530`. |
| `app/chamados/services/pergunta_no_app.py::perguntas_do_app` | Pré-filtro `q_vivas`, juiz C6 por pergunta. | Lê C6; não tarefa, `app/chamados/services/pergunta_no_app.py:45`, `:58`. |
| `app/chamados/services/pergunta_no_app.py::consulta_do_app` | Separa pendentes/histórico por `respondida_em is None` e disputa aberta. | Derivação própria de apresentação, `app/chamados/services/pergunta_no_app.py:61`, `:70`. |
| `app/chamados/services/pergunta_no_app.py::projecao_do_app` | Conta perguntas pendentes; combina com chamados não lidos para badge. | Derivação própria de contagem/prioridade, `app/chamados/services/pergunta_no_app.py:78`, `:90`, `:98`. |
| `app/core/views.py::dashboard` | Badge/PWA conta `len(consulta_do_app(... )['pendentes'])`. | Não lê tarefa, `app/core/views.py:252`; render `:270`; badge em `app/templates/core/dashboard_colaborador_v2.html:78`. |
| `app/core/context_processors.py::navbar_context._badge_colab` | Badge navbar conta `qs_na_fila(f_colab=...).count()`. | Outro sítio contador, não tarefa, `app/core/context_processors.py:185`. |
| `app/folha/fora_da_folha.py::_porta` | Consulta C17. | Não tarefa, `app/folha/fora_da_folha.py:65`. |
| `app/folha/fora_da_folha.py::_fio_do_dia` | Traduz ausência de porta para `FIO_SEM_PORTA`, senão lê `porta['fio']`. | Leitor de C17, `app/folha/fora_da_folha.py:70`. |
| `app/folha/fora_da_folha.py::fora_da_folha` | Agrupa colaboradores pelo fio de seus dias retidos. | Leitura indireta de C17, `app/folha/fora_da_folha.py:142`, `:178`. |
| `app/folha/fora_da_folha.py::o_que_segura` | Dias retidos, “destrava” e chamados da fila. | Leitura indireta de C17 e frase local `DESTRAVA`, `app/folha/fora_da_folha.py:203`, `:221`, `:225`. |
| `app/escala/services/cadastro_realidade.py::beco_em_lote` | C11 filtra células acusadoras com perguntas vivas e ausência de porta C17. | Não tarefa, `app/escala/services/cadastro_realidade.py:442`, `:476`, `:488`. |
| `app/chamados/services/vigia_fio.py::censo_alcance` | Universo dos dias retidos; porta C17, depois baldes e causas próprias para contador. | Não tarefa, `app/chamados/services/vigia_fio.py:542`, `:585`, `:635`, `:654`, `:695`. |
| `app/chamados/services/vigia_fio.py::censo` | Outro universo do vigia, com fio/perguntas e depois mesmos baldes. | Não tarefa, `app/chamados/services/vigia_fio.py:318`, `:417`, `:431`. |
| `app/chamados/services/vigia_fio.py::dono_fora_do_fio` | Cadeia local: resolver dia, ausência aberta, folga validada, lista admin. | Composição própria do “quem deve agir”; `app/chamados/services/vigia_fio.py:274`, `:306`. |

O caminho UI carrega JavaScript e service worker: ambas bases carregam `pwa.js`, que registra `/sw.js`; ambas também carregam `static/js/popover-acao.js`. O SW intercepta GET e trata cache/push; nessas linhas ele não decide a tarefa do dia. A classificação CLASSE A depende do diff tocar os arquivos indicados pelo aval, e não apenas de existirem na cadeia. Fontes: `app/templates/base.html:20`, `:440`; `app/templates/base_app.html:20`, `:194`; `app/static/pwa.js:14`; `app/static/sw.js:33`, `:88`; condição CLASSE A fornecida no anexo `[nao verificado]` quanto ao diff futuro.

A tela de atendimento escolhe as duas cascas por `base_tpl`; o dashboard do colaborador atualmente estende `base.html` diretamente. Fontes: `app/templates/chamados/meu_atendimento.html:1`; `app/core/context_processors.py:194`; `app/templates/core/dashboard_colaborador_v2.html:1`.

Testes relevantes para o portão e adaptação: [nao verificado]

- `app/chamados/tests/test_e3_pendentes_app.py`: catraca AST resolve imports, aliases, atribuições e `getattr`; permite hoje exatamente os três chamadores C17 de folha, vigia e cadastro. Fontes: `app/chamados/tests/test_e3_pendentes_app.py:19`, `:33`, `:145`.
- `app/chamados/tests/test_e3_app_orm.py`: matriz API, dashboard, dois kernels, perguntas pendentes/histórico, badge vermelho. Fontes: `app/chamados/tests/test_e3_app_orm.py:28`, `:47`, `:52`, `:57`, `:62`, `:76`.
- `app/chamados/tests/test_e3_v4_orm.py`: avisos/cobrança/badge na mesma pendente e leitura C6; fontes `app/chamados/tests/test_e3_v4_orm.py:20`, `:42`.
- `app/chamados/tests/test_e3_portas_tarefa.py`: fila sem pergunta, A/B, verbos, isento, relógio e alimentação em lote; fontes `app/chamados/tests/test_e3_portas_tarefa.py:32`, `:40`, `:49`, `:57`, `:84`, `:113`.
- `app/chamados/tests/test_fio_le_o_juiz.py`: admin/colaborador, viva em disputa fechada, exclusão de morta, vigia e badge; fontes `app/chamados/tests/test_fio_le_o_juiz.py:105`, `:115`, `:121`, `:129`, `:262`.
- `app/chamados/tests/test_o261e_f5b_juiz_porta.py`: folha/vigia, A/B, fila sem pergunta, âncora fora da fila, verbos e registro C17; fontes `app/chamados/tests/test_o261e_f5b_juiz_porta.py:119`, `:125`, `:138`, `:226`, `:258`, `:278`.
- `app/folha/tests/test_haiku_fora_da_folha.py`: leitura C17, fonte única, isento, grupos e datas; fontes `app/folha/tests/test_haiku_fora_da_folha.py:93`, `:175`, `:240`, `:475`, `:531`.
- `app/ponto/tests/test_cxr_tela_le_o_motor.py`: C11 lê lista lavrada e não chama motor, sem resposta fora da lavra, relógio; fontes `app/ponto/tests/test_cxr_tela_le_o_motor.py:390`, `:410`, `:422`, `:444`.
- `app/core/tests/test_dashboard_badge.py`: badge igual à consulta do app; fonte `app/core/tests/test_dashboard_badge.py:31`.
- `app/api/tests/test_api_me_badge.py`: badge API zero/pendente/laranja; fontes `app/api/tests/test_api_me_badge.py:30`, `:36`, `:59`.

Nenhum teste foi executado neste subtrabalho; estado RED/GREEN atual `[nao verificado]`. [nao verificado]

## 12/11 08:51 · sessao b6b0d043e1b6 · - · - · origem=Ronald (tmux)

1. **DECISÃO — tarefa é o juiz que fica.** Os quatro desacordos antigos (rota só fila, pergunta sob outra âncora, verbos A/B e isento com fila residual) já estão decididos por LEI-C17-UM-JUIZ: vale a tarefa, que deve responder rota/fila/verbos; migrar chamadores somente depois do DIFF na sombra. Não há pergunta nova de lei nesses quatro campos. `app/docs/RELATO.md:515`, `app/docs/RELATO.md:518`.

2. **DECISÃO — consumidor e vigia leem a resposta.** Não recriar vida, exigibilidade nem julgamento por leitor; a lâmpada 4a manda o consumidor ler e o cron somente alarmar. `CLAUDE.md:169`, `CLAUDE.md:172`, `CLAUDE.md:173`. A tarefa já recebe a vida de C6 e não deve duplicá-la. `app/chamados/services/tarefa_do_dia.py:174`, `app/chamados/services/tarefa_do_dia.py:242`, `app/chamados/services/tarefa_do_dia.py:245`.

3. **DECISÃO — migração em fases, Contract após censo.** L-127 exige que Expand não troque leitor de fonte e que Contract só apague depois que nenhum leitor use o antigo. `app/docs/LEIS.md:148`, `app/docs/PLANO-PADROES.md:79`, `app/docs/PLANO-PADROES.md:83`, `app/docs/PLANO-PADROES.md:85`. O registro antigo ainda diz “os 5 respondedores […] seguem vivos”, mas ali os cinco nomes são `cobranca_viva_do_dia`, `pergunta_viva`, `motivos_a_perguntar`, `porta_alcancavel_do_dia`, `dono_fora_do_fio`; não confundir essa lista documental com os cinco sítios do aval de agora e apagar C6 indevidamente. `app/core/juizes.py:150`, `app/core/juizes.py:153`, `app/docs/JUIZES.md:85`.

4. **DECISÃO — não transportar o diagnóstico V antigo sem medir a árvore atual.** A tarefa atual já expõe ação administrativa em dia sem trabalho, motivos fora do mapa de marcos e `fato_em_ausencia` sem pergunta viva. `app/chamados/services/tarefa_do_dia.py:241`, `app/chamados/services/tarefa_do_dia.py:260`, `app/chamados/services/tarefa_do_dia.py:314`, `app/chamados/services/tarefa_do_dia.py:327`. A persistência das classes V/T/X nesta árvore depende da sonda [nao verificado]. A diferença entre os denominadores 230 e 232 é uma lacuna de identificação do universo, não uma nova lei [nao verificado].

5. **Item 11 confirmado: chave e universo incompatíveis.** `montar_chamados_fio` mantém cache por `d.pk`, porém calcula sua primeira entrada com `perguntas_por_chamado.get(ch.pk)`. `app/chamados/services/fio.py:306`, `app/chamados/services/fio.py:312`, `app/chamados/services/fio.py:313`, `app/chamados/services/fio.py:314`. `por_que_nao_fecha` usa a lista recebida quando presente e só busca toda a disputa quando `perguntas is None`; depois decide fechamento por `not fio_vivo(ps)`. `app/chamados/services/explicador.py:121`, `app/chamados/services/explicador.py:124`, `app/chamados/services/explicador.py:134`, `app/chamados/services/explicador.py:137`. Portanto a correção deve alinhar o universo da lista ao universo da chave de cache e preservar as instâncias já carregadas, exigência do selo de identidade. `app/chamados/tests/test_selo_identidade_fio_explicacao.py:1`, `app/chamados/tests/test_selo_identidade_fio_explicacao.py:13`, `app/chamados/tests/test_selo_identidade_fio_explicacao.py:16`.

6. **Item 12 confirmado: carimbo cru dirige aviso.** `todas_resp = all(p.respondida_em for p in ch.perguntas_disputa)` alimenta `ch.todas_respondidas`; o modal usa sua negação para escrever “não respondeu” e impedir fechamento sem anuência. `app/chamados/services/fio.py:340`, `app/chamados/services/fio.py:341`, `app/templates/chamados/partials/modal_fio.html:334`, `app/templates/chamados/partials/modal_fio.html:341`. Deve considerar a vida pelo C6 (`pergunta_viva`) e a redução da coleção por C15 (`fio_vivo`), preservando o significado da pendência do colaborador. C6 distingue explicitamente perguntas mortas por validação, tranca, vias vencedoras e lâmpada. `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1395`, `app/chamados/juizes.py:1402`, `app/chamados/juizes.py:1452`. A definição de `fio_vivo` é a redução `any(pergunta_viva(p) …)` e o universo cabe ao chamador. `app/chamados/juizes.py:1494`, `app/chamados/juizes.py:1500`, `app/chamados/juizes.py:1503`.

7. **Registro de juízes: ressalva necessária no brief.** `JUIZES.md` não enumera diretamente os endereços de `pergunta_viva` e `fio_vivo`: registra C7 em `DisputaSupervisao.pode_fechar` e cita C6 como leitor da tranca; declara que o registro vivo é `app/core/juizes.py` e que o vivo prevalece. `app/docs/JUIZES.md:10`, `app/docs/JUIZES.md:12`, `app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:62`, `app/docs/JUIZES.md:90`. A localização verificável é C6 → `chamados/juizes.py::pergunta_viva` e C15 → `chamados/juizes.py::fio_vivo`. `app/core/juizes.py:112`, `app/core/juizes.py:125`, `app/core/juizes.py:278`, `app/core/juizes.py:287`. Corrigir o lookup no mesmo commit segue sua própria instrução. `app/docs/JUIZES.md:14`.

8. **REDs reais e PRONTO literal: limites.** Os números 34390, 35602 e 51407 e a disputa 7445 não foram encontrados em código/testes de `app` e `bin`; a fonte disponível é somente o anexo do usuário [nao verificado]. Ele fornece sintomas, não uma fixture completa; RED executável com fixture copiada permanece [aguarda sonda]. `logs/MAPA-20/pronto_xn.r` e a árvore `logs/` não estão disponíveis nesta cópia [nao verificado]. Copiar os dez itens literalmente do anexo dado, sem substituir pela lista resumida do BACKLOG [nao verificado]. Selo existente relacionado ao item 11: `app/chamados/tests/test_fio_explicacao.py:22`, `app/chamados/tests/test_fio_explicacao.py:36`.

9. **Diferenças concretas dos respondedores para embasar decisões.** `raiox.creditos_perguntas` recorta por lavra e separa resposta pelo carimbo, atribuindo crédito ao destino vivo ou pai. `app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:139`, `app/chamados/services/raiox.py:145`. `relatorios_disputa.classificar` usa C6, mas escolhe colaborador/supervisão por `respondida_em`. `app/chamados/relatorios_disputa.py:11`, `app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:21`. `cobranca._calados` parte de `q_vivas`, carimbo sem resposta e disputa aberta, agrupando por pessoa. `app/chamados/services/cobranca.py:221`, `app/chamados/services/cobranca.py:228`, `app/chamados/services/cobranca.py:234`. O cartão oferece formulário por `p.viva` e diz “aguardando a supervisão” por `p.respondida_em`. `app/templates/chamados/partials/_card_atendimento.html:73`, `app/templates/chamados/partials/_card_atendimento.html:77`, `app/templates/chamados/partials/_card_atendimento.html:94`, `app/templates/chamados/partials/_card_atendimento.html:101`.

10. **Sem pergunta nova de lei identificada neste recorte.** O que falta para decidir o DIFF é prova de frota e identificação do universo; as autoridades e a ordem de migração já estão escritas. `app/docs/RELATO.md:518`, `CLAUDE.md:173`, `app/docs/PLANO-PADROES.md:83`. Perguntas de lei deste subcenso: 0.

## 12/11 08:33 · sessao 944391eae9b5 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVA c11:** a semana do posto transforma a recusa de julgamento em **dia explicado**, retirando-o dos buracos e podendo esconder o posto do contador. O próprio teste exige essa classificação (`app/escala/services/semana_do_posto.py:152`, `app/escala/services/semana_do_posto.py:237`, `app/ponto/tests/test_lastro_c11_sem_banco.py:270`). O par financeiro da competência 11 exigido pelo aval continua **[nao verificado]**.

1. **Achados da c10 e mordida**

| Achado anterior | Conclusão na c11 | Teste e mutante | [nao verificado]
|---|---|---|
| `montar_semanas` relançava a recusa | **Curado no caminho reproduzido:** os dois ramos D4 consultam A2 com `recusas=`; o ramo alimentado lê fatos e recusas recebidos; devolve `erro.rotulo` (`app/ponto/status_dia.py:302`, `app/ponto/status_dia.py:307`, `app/ponto/status_dia.py:314`, `app/ponto/status_dia.py:318`). | O teste passa; retirar `recusas=` nos mutantes `d4_lote` e `d4_unitario` produz falha por asserção, reproduzida nesta verificação (`app/ponto/tests/test_lastro_c11_sem_banco.py:47`, `app/ponto/tests/test_lastro_c11_sem_banco.py:121`, `app/ponto/tests/test_lastro_c11_sem_banco.py:136`). |
| Porta aceitava `codigo` em `campos` | **Curado:** rejeita antes de qualquer `setattr`/`save`, inclusive código repetido; a guarda de faixa consulta a identidade final (`app/ponto/services/ausencia.py:1306`, `app/ponto/services/ausencia.py:1309`, `app/ponto/services/ausencia.py:1314`). | Os pares passam; `porta_codigo` e `porta_final` falharam por asserção nesta execução (`app/ponto/tests/test_lastro_c11_sem_banco.py:53`, `app/ponto/tests/test_lastro_c11_sem_banco.py:152`, `app/ponto/tests/test_lastro_c11_sem_banco.py:162`). |
| Contador com duas unidades | **Curado:** cartório declara/incrementa/imprime `colabs_isolados_tipo_sem_faixa`; vigia conserva o contador de códigos ativos (`app/ponto/services/cartorio.py:1954`, `app/ponto/management/commands/processar_cartorio.py:73`, `app/ponto/services/vigia_ausencia.py:168`). | O teste passa; `contador` restabelece o nome antigo e falhou por asserção (`app/ponto/tests/test_lastro_c11_sem_banco.py:62`, `app/ponto/tests/test_lastro_c11_sem_banco.py:167`). |
| Paridade das órfãs PDF/tela | **Esclarecido e coberto no caso pedido:** tela publica `orfas_grade`; PDF lê essa saída. Sem célula, o caso ensaiado entrega órfãs vazias (`app/ponto/services/espelho.py:522`, `app/ponto/services/espelho.py:853`, `app/relatorios/pdf_espelho.py:477`, `app/ponto/tests/test_lastro_c11_sem_banco.py:299`). | O coletor ensaiado passa; `pdf_builder` injeta `07:02` e falhou em `assertFalse(batida.orfa)`. Esse mutante adultera o mapa, **não executa o builder antigo restaurado** (`app/ponto/tests/test_lastro_c11_sem_banco.py:68`, `app/ponto/tests/test_lastro_c11_sem_banco.py:354`). |

Executei os quatro harnesses: **55 testes, OK, RC 0**; executei os dez mutantes C11 individualmente: todos terminaram **RC 1 por asserção**. São ensaios AST com alimentação externa substituída, não integração HTTP/ORM (`app/ponto/tests/test_lastro_c5_sem_banco.py:1`, `app/ponto/tests/test_lastro_c11_sem_banco.py:1`, `app/ponto/tests/test_lastro_c11_sem_banco.py:42`). Reexecução histórica contra o commit c10: **[nao verificado]**; o carregador depende de `git show` (`app/ponto/tests/test_lastro_c11_sem_banco.py:33`).

O teste PDF substitui o espelho completo, a apuração e `folha_manda`; portanto não certifica renderização binária nem paridade financeira integral (`app/ponto/tests/test_lastro_c11_sem_banco.py:317`, `app/ponto/tests/test_lastro_c11_sem_banco.py:343`, `app/ponto/tests/test_lastro_c11_sem_banco.py:347`).

2. **Censo dos chamadores**

Na tabela: **a** preserva a recusa nomeada; **b** propaga quando alcança a consulta; **c** absorve, omite ou reclassifica. As condições anteriores à consulta importam: ter o tipo legado no cadastro, sozinho, não garante que todos esses ramos sejam alcançados (`app/ponto/precedencia.py:417`, `app/ponto/status_dia.py:283`, `app/ponto/services/faltas_de_hoje.py:59`).

| Chamador direto, fora de tests/migrations | Destino |
|---|---|
| `precedencia.fatos_do_dia` → período | **b**: forma unitária sem `recusas=` (`app/ponto/precedencia.py:286`). |
| `escala.utils._marcar_ausencias` → período | **b**: builder consulta sem destinatário de recusas (`app/escala/utils.py:1209`). |
| `folha.services.sem_furo.dias_com_furo` → período | **b**, se houver marcos apagados; command `colabs_sem_furo` (`app/folha/services/sem_furo.py:23`, `app/folha/services/sem_furo.py:25`, `app/folha/management/commands/colabs_sem_furo.py:13`). |
| Calendário → período | **a**: transporta as duas respostas da A2 ao D4/compositor (`app/colaboradores/services/calendario.py:510`, `app/colaboradores/services/calendario.py:607`, `app/colaboradores/services/calendario.py:719`). |
| Espelho, alias `_fatos_periodo` → período | **a**: recebe recusas e transporta seu rótulo (`app/ponto/services/espelho.py:653`, `app/ponto/services/espelho.py:880`). |
| `dia_do_colab` → período | **a**: rótulo preservado, natureza desconhecida nula (`app/ponto/services/dia_do_colab.py:16`, `app/ponto/services/dia_do_colab.py:94`). |
| D4 → período, sem carga/com carga | **a**, quando alcança A2 (`app/ponto/status_dia.py:302`, `app/ponto/status_dia.py:311`, `app/ponto/status_dia.py:318`). |
| `fatos_do_status` → dia | **b**, somente para veredito `indefinida`; insumo da lavra (`app/ponto/status_dia.py:177`, `app/ponto/status_dia.py:181`, `app/ponto/services/cartorio.py:932`). |
| `feriado_reavaliar.espurios` → dia | **b** no command `reavaliar_ausencias_feriado`; **c** nos eventos protegidos por captura (`app/chamados/services/feriado_reavaliar.py:33`, `app/ponto/management/commands/reavaliar_ausencias_feriado.py:39`, `app/chamados/signals.py:332`, `app/ponto/portas/celula.py:499`). |
| Semana do posto → dia | Rótulo chega, mas **c**: vira explicação válida para classificação/contagem (`app/escala/services/semana_do_posto.py:179`, `app/escala/services/semana_do_posto.py:152`). |
| `detectar_ausencias` → dia | **b**, ao alcançar o gate de feriado (`app/ponto/management/commands/detectar_ausencias.py:616`). |
| `deslogados` → dia | **b**: `lavrar_deslogados` e apuração de pautas (`app/ponto/services/deslogados.py:166`, `app/ponto/management/commands/lavrar_deslogados.py:38`, `app/ponto/services/pautas_dos_deslogados.py:62`). |
| `abriu_nao_bateu` → dia | **b**, com filtro de previsão ativo; command `lavrar_abriu_nao_bateu` (`app/ponto/services/abriu_nao_bateu.py:123`, `app/ponto/management/commands/lavrar_abriu_nao_bateu.py:30`). |
| `faltas_de_hoje` → dia | **b**, se passar pelas guardas anteriores; command `faltas_de_hoje_sem_cobranca` (`app/ponto/services/faltas_de_hoje.py:59`, `app/ponto/services/faltas_de_hoje.py:61`, `app/ponto/management/commands/faltas_de_hoje_sem_cobranca.py:16`). |
| Cartório, `aplicar_precedencia_na_celula` → dia | Levanta localmente; **c** na consulta tardia protegida de `julgar_celula`, que loga e retorna sem novo carimbo (`app/ponto/services/cartorio.py:1998`, `app/ponto/services/cartorio.py:2047`, `app/ponto/services/cartorio.py:2053`). |
| Férias, `_repouso` → dia | Aviso nomeado, retorno `None`, sem concluir repouso (`app/ferias/situacao.py:95`, `app/ferias/situacao.py:98`). |
| `status_do_dia_lavrado` → lote | Delega ao mesmo corpo (`app/ponto/status_dia.py:218`). |
| `montar_semanas` → D4 | **a** no caso reprovado na c10; copia status/rótulo (`app/ponto/services/espelho.py:170`, `app/ponto/services/espelho.py:174`). |
| Calendário → D4 | **a**, com fatos/recusas alimentados (`app/colaboradores/services/calendario.py:606`). |
| API espelho v2 → D4 | Lê somente status; **c** fora da janela da palavra do espelho, detalhado abaixo (`app/api/views.py:1483`, `app/api/views.py:1584`, `app/api/views.py:1586`). |
| Auditoria de furos → D4 | **c no relatório filtrado**: conserva status, cobra apenas `irregular`; recusa não aparece como furo (`app/relatorios/furos.py:123`, `app/relatorios/furos.py:128`, `app/relatorios/furos.py:141`). |
| Painel operacional → D4 | Consulta pares encerrados com entrada; normalmente recebe lavra ou não certificação genérica, sem consultar A2 (`app/core/services/painel_op.py:329`, `app/core/services/painel_op.py:336`, `app/ponto/status_dia.py:295`). |
| Compositor → `dia_decidido.do_dia` | **a**: recusa tem ramo anterior e não entra no tradutor de precedência (`app/ponto/services/espelho.py:339`, `app/ponto/services/espelho.py:343`). |
| API/ficha → `dia_do_colab` | **a**: serializam/transportam o resultado (`app/api/views_mensageria.py:1970`, `app/colaboradores/services/ficha.py:280`). |

Os outros `do_dia` encontrados pertencem ao calculador, vigia de entrada e pergunta no app; são contratos distintos do tradutor de precedência (`app/ponto/calculador/regras.py:576`, `app/ponto/services/vigia_entrada.py:111`, `app/chamados/services/pergunta_no_app.py:211`).

**Destinos indiretos b/c relevantes pelo builder:**

- **b — `/ponto/celula/veredito/`**, ramo `trabalhou`: view → serviço → `grade_dia` → builder sem recusas. É ação que necessita julgamento, não evidência de que devesse fabricar resultado tolerante (`app/ponto/urls.py:58`, `app/ponto/views.py:3340`, `app/ponto/services/veredito_celula.py:241`, `app/ponto/services/simular_dia.py:7`).
- **b — `/ponto/flip-fila/` e command `flip_automatico`**: avaliação usa a mesma grade (`app/ponto/urls.py:63`, `app/ponto/views.py:3305`, `app/ponto/services/flip_auto.py:303`, `app/ponto/management/commands/flip_automatico.py:32`).
- **b — command `detectar_par_relampago`**: usa builder no score (`app/ponto/management/commands/detectar_par_relampago.py:136`).
- **c — command `reconciliar_grade`**: captura, incrementa `GRADE_FAIL` e pula colaborador (`app/ponto/management/commands/reconciliar_grade.py:118`, `app/ponto/management/commands/reconciliar_grade.py:120`).
- **c — grade de apresentação do espelho**: falha pode virar grade vazia; consulta A2 independente ainda preserva a palavra da recusa (`app/ponto/services/espelho.py:572`, `app/ponto/services/espelho.py:653`).
- **Isolamento autorizado — `processar_cartorio`**: captura por colaborador, conta e loga; demais colaboradores seguem (`app/ponto/management/commands/processar_cartorio.py:64`, `app/ponto/management/commands/processar_cartorio.py:72`).

**Achado — fila: rótulo perdido no app futuro.** Contraexemplo: hoje 12/11, ausência legada `fantasma_xyz` em 13/11, sem batida. D4 devolve a recusa; a API `/api/espelho/v2/` descarta `rotulo`. Como o espelho monta dias somente até hoje, a palavra de 13/11 cai no vazio: `status='neutro'`, `status_dia='sem_certidao'`, sem o nome da recusa (`app/ponto/status_dia.py:318`, `app/ponto/services/espelho.py:1272`, `app/api/views.py:1390`, `app/api/views.py:1584`, `app/api/views.py:1594`, `app/api/urls.py:81`). Ocorrência produtiva: **[nao verificado]**.

Não sustento quebra natural pelo fechamento apenas porque seus handlers relançam: a grade financeira identificada lê a célula, sem chamar A2. Propagação se ocorrer é demonstrada; geração natural dessa exceção nesse caminho permanece **[nao verificado]** (`app/folha/export.py:335`, `app/ponto/services/fechamento.py:1089`, `app/ponto/services/fechamento.py:1308`).

3. **`sem_certidao`: não encontrei falsificação semântica por esse valor**

A definição vigente já significa situação não certificada por evidência/decisão insuficiente. Uma faixa que não pode ser julgada cabe nessa definição; não significa exclusivamente “não existe ata” (`app/ponto/status_dia.py:45`).

| Leitor | Sentido observado |
|---|---|
| Contador/calendário | Soma no balde de não certificados, separado de faltas e ausências (`app/colaboradores/services/calendario.py:773`, `app/colaboradores/services/calendario.py:785`, `app/colaboradores/services/calendario.py:789`). |
| Badges/tooltips | Rodapé usa a definição geral; dia pode mostrar rótulo específico (`app/templates/colaboradores/partials/_calendario_grade.html:107`, `app/templates/colaboradores/partials/_calendario_grade.html:177`, `app/templates/ponto/espelho.html:151`). |
| App | Traduz para neutro, não para falta ou irregularidade; há a perda de rótulo futuro já apontada (`app/ponto/status_dia.py:84`, `app/api/views.py:1604`, `app/api/views.py:1607`). |
| Auditoria | Não cobra como furo (`app/relatorios/furos.py:128`). |
| Chamados | Pode oferecer “Resolver dia” no passado, com último recurso; vigia do fio lê essa oferta (`app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`, `app/chamados/services/vigia_fio.py:271`). |
| Folha/cartório | Não localizei consumo financeiro desse retorno transitório D4. A escrita do status usa os contratos próprios da lavra/admin (`app/ponto/status_dia.py:198`, `app/ponto/portas/celula.py:958`, `app/ponto/portas/celula.py:970`). |

D4 continua priorizando andamento e status lavrado antes de consultar A2. Portanto, **seu próprio rótulo não é universalmente o da recusa**; no espelho, a palavra separada pode preservá-la (`app/ponto/status_dia.py:283`, `app/ponto/status_dia.py:290`, `app/ponto/services/espelho.py:339`).

4. **Calendário válido: transporte equivalente à chamada da base**

Não encontrei contraexemplo no delta: conserva presença, cobertura, decisão, marcos faltantes e pertinência a `_em_aberto`; o compositor repassa esses argumentos (`app/colaboradores/services/calendario.py:713`, `app/ponto/services/espelho.py:343`).

`pendente` continua `None`: antes era omitido; agora `_pendente_do_dia` também é omitido e lido por `.get`. `ata_sem_turno` retorna inteiro positivo ou zero, preservado pelo `or 0` do compositor (`app/ponto/services/dia_decidido.py:399`, `app/ponto/services/espelho.py:334`, `app/ponto/services/dia_decidido.py:179`, `app/ponto/services/espelho.py:345`). Palavra/cor/veredito são copiados da mesma resposta de `do_dia` (`app/ponto/services/espelho.py:347`, `app/colaboradores/services/calendario.py:723`).

O teste C11 sela a recusa, com tradutor substituído; não sela paridade integral de dias válidos (`app/ponto/tests/test_lastro_c11_sem_banco.py:219`, `app/ponto/tests/test_lastro_c11_sem_banco.py:230`).

5. **Semana do posto: bloqueia — recusa vira explicação**

O consumidor aceita qualquer string diferente de `'trabalho'` como explicação. Assim, `erro.rotulo` ocupa o mesmo canal de uma faixa reconhecida, entra em `explicados`, sai de `buracos` e deixa de participar das propostas (`app/escala/services/semana_do_posto.py:152`, `app/escala/services/semana_do_posto.py:154`, `app/escala/services/semana_do_posto.py:179`, `app/escala/services/semana_do_posto.py:197`).

**Contraexemplo:** posto com um único colaborador previsto, sem batida, com `fantasma_xyz`: `buracos=[]`, `explicados=[recusa]`; contador sem posto omite esse posto; consulta específica publica `dias_sem_explicacao=0`. Recusar o julgamento passou a valer como explicar o dia (`app/escala/services/semana_do_posto.py:237`, `app/api/views_mensageria.py:2083`, `app/api/views_mensageria.py:2121`).

O teste exige precisamente `buracos=[]`: morde o transporte do rótulo, mas **sela a mistura semântica** (`app/ponto/tests/test_lastro_c11_sem_banco.py:270`).

`explicados` tem consumidor por transporte integral até o contexto do copiloto; não encontrei leitura tipada específica nem template/JS dessa chave. A interpretação final pelo modelo é **[nao verificado]** (`mensageria/nucleo/core_client.py:308`, `mensageria/nucleo/ferramentas.py:884`, `mensageria/nucleo/ferramentas.py:422`, `mensageria/nucleo/copiloto.py:379`).

6. **`dia_do_colab`: sem leitor identificado que quebre com esses `None`**

O ramo recusado entrega rótulo, natureza nula e evita `_veredito_do_dia` (`app/ponto/services/dia_do_colab.py:94`, `app/ponto/services/dia_do_colab.py:108`). Os leitores encontrados serializam o dicionário inteiro ou o incluem na ficha; `_json` preserva `None` (`app/api/views_mensageria.py:1970`, `app/colaboradores/services/ficha.py:253`, `app/colaboradores/services/ficha.py:280`). A mensageria transporta o resultado, sem iterar `escala.ausencias` nem desreferenciar `escala.tipo` (`mensageria/nucleo/ferramentas.py:817`, `mensageria/nucleo/ferramentas.py:1081`).

HTTP completo e consumidores externos à cópia: **[nao verificado]**.

7. **Rename: sem órfão operacional localizado na árvore**

Cartório declara/incrementa/imprime exclusivamente o contador de colaboradores; teste exige ausência do alias antigo (`app/ponto/services/cartorio.py:1954`, `app/ponto/management/commands/processar_cartorio.py:73`, `app/ponto/management/commands/processar_cartorio.py:80`, `app/ponto/tests/test_lastro_c11_sem_banco.py:169`).

O nome antigo preservado pertence ao vigia: códigos ativos. API e revisão semanal leem explicitamente esse vigia; não passaram a ler o contador do cartório (`app/ponto/services/vigia_ausencia.py:160`, `app/api/views_mensageria.py:1416`, `app/api/views_mensageria.py:1422`, `app/ponto/services/vigia_ausencia.py:259`). A descrição documental encontrada também se refere ao vigia; cron não interpreta esse campo (`app/docs/RELATO.md:37`, `app/config/crons.py:616`). Parser externo à cópia: **[nao verificado]**.

8. **Classes de reprovação**

- **Campo com dois significados — bloqueia:** faixa reconhecida e recusa compartilham o canal que significa “dia explicado”; contraexemplo do posto acima (`app/escala/services/semana_do_posto.py:152`, `app/escala/services/semana_do_posto.py:179`).
- **Rótulo descartado — fila:** app futuro converte recusa nomeada em neutro sem palavra (`app/api/views.py:1584`, `app/api/views.py:1586`).
- **Fallback — fila, preexistente:** catálogo troca falha/vazio por catálogo congelado. Contraexemplo: `abono` cadastrado para descontar pode voltar ao efeito congelado após falha de leitura; ficou expressamente fora desta volta (`app/ponto/catalogo/ausencias.py:254`, `app/ponto/catalogo/ausencias.py:283`, `app/ponto/catalogo/ausencias.py:298`).
- **Except que engole — fila:** semana converte outros erros em `'trabalho'`; reconciliação converte em `GRADE_FAIL`; cartório captura falha na consulta tardia. Para esta última, alcançar a recusa apenas na segunda consulta exige mudança de alimentação entre consultas, **[nao verificado]** (`app/escala/services/semana_do_posto.py:180`, `app/ponto/management/commands/reconciliar_grade.py:119`, `app/ponto/services/cartorio.py:2048`).
- **Selo sem mordida:** não procede como acusação geral aos testes novos; os mutantes falharam. Permanecem os limites do PDF substituído e o teste da semana que exige o resultado semanticamente errado (`app/ponto/tests/test_lastro_c11_sem_banco.py:42`, `app/ponto/tests/test_lastro_c11_sem_banco.py:270`, `app/ponto/tests/test_lastro_c11_sem_banco.py:347`).
- **Segundo juiz/segundo escritor/relógio novo/derivação paralela:** sem achado novo nos sítios lidos. Faixa continua por indexação na autoridade; cadastro passa pela porta; calendário usa compositor comum; PDF lê órfãs da tela e deriva `_hj` do instante recebido (`app/ponto/precedencia.py:108`, `app/ponto/views_tipos_ausencia.py:87`, `app/ponto/views_tipos_ausencia.py:98`, `app/colaboradores/services/calendario.py:719`, `app/relatorios/pdf_espelho.py:475`, `app/relatorios/pdf_espelho.py:477`).

## 12/11 08:30 · sessao 18c53079f346 · - · - · origem=Ronald (tmux)

Retiro o achado de quebra natural por `folha_manda`: não localizei ligação fechamento → A2. O fechamento chama `grade_do_fechamento`, que apenas lê a célula (`app/ponto/services/fechamento.py:1014`, `app/folha/export.py:335`, `app/folha/export.py:338`).

As capturas comprovam propagação **se** `TipoSemFaixa` ocorrer; não comprovam que o tipo legado provoque essa exceção nesse caminho (`app/ponto/services/fechamento.py:1089`, `app/ponto/services/fechamento.py:1308`). Alcance natural: [nao verificado].

## 12/11 08:30 · sessao 18c53079f346 · - · - · origem=Ronald (tmux)

Censo direto concluído, fora tests/migrations; os homônimos de `do_dia` no calculador, vigia de entrada e pergunta no app têm contratos distintos (`app/ponto/calculador/regras.py:576`, `app/ponto/services/vigia_entrada.py:111`, `app/chamados/services/pergunta_no_app.py:211`).

**Achado principal confirmado — bloqueia a promessa espelho/PDF rotular:** a chamada posterior a `folha_manda` reentra no fechamento online e levanta. Cadeia: tela chama espelho (`app/ponto/views.py:158`); espelho chama `_folha_manda` sem captura (`app/ponto/services/espelho.py:1294`); competência exata chama totais (`app/relatorios/cartao_pela_celula.py:321`); existindo fechamento gravado, totais chama `fechamento_lido` (`app/relatorios/cartao_pela_celula.py:71`, `app/relatorios/cartao_pela_celula.py:73`); leitura recalcula e propaga `TipoSemFaixa` (`app/ponto/services/fechamento.py:1766`, `app/ponto/services/fechamento.py:1089`, `app/ponto/services/fechamento.py:1308`). PDF entra no próprio espelho e ainda chama folha novamente (`app/relatorios/pdf_espelho.py:475`, `app/relatorios/pdf_espelho.py:587`). URL da tela `ponto/espelho/`, app `api/espelho/v2/`, PDF avulso `relatorios/espelho/avulso/` (`app/ponto/urls.py:10`, `app/api/urls.py:81`, `app/relatorios/urls.py:28`). No PDF informacional, geração sem captura está em `app/relatorios/views.py:1098`; o caminho mensal passa pelo serviço (`app/relatorios/services.py:70`). Lote tenta auditável, depois informacional e finalmente registra colaborador pulado: não garante PDF com rótulo (`app/relatorios/services.py:244`, `app/relatorios/services.py:250`, `app/relatorios/services.py:254`).

**Outro achado concreto — fila, recusa some no app fora da janela do espelho.** Hoje 12/11, ausência legada aprovada `fantasma_xyz` em 13/11 sem batida: D4 obtém a recusa, mas API descarta seu `rotulo`, serializando só status (`app/ponto/status_dia.py:318`, `app/api/views.py:1584`). Palavra vem do espelho, que monta somente até hoje (`app/ponto/services/espelho.py:1272`). Para 13/11, `.get` usa palavra vazia e resposta vira `status='neutro'`, `status_dia='sem_certidao'`, sem nome da recusa (`app/api/views.py:1390`, `app/api/views.py:1586`, `app/api/views.py:1594`, `app/api/views.py:1604`). Endpoint: `app/api/urls.py:81`. Ocorrência produtiva [nao verificado].

Censo direto:

| Chamador | Resultado ao alcançar o ramo |
|---|---|
| `fatos_do_dia` → período | Propaga por definição unitária (`app/ponto/precedencia.py:286`). |
| `_marcar_ausencias` → período | **b**, consulta sem `recusas` (`app/escala/utils.py:1209`). Alimenta todos os builders (`app/escala/utils.py:807`, `app/escala/utils.py:1008`, `app/escala/utils.py:1863`). |
| `dias_com_furo` → período | **b**, command `colabs_sem_furo`, condicionado a ter marco apagado (`app/folha/services/sem_furo.py:23`, `app/folha/services/sem_furo.py:25`, `app/folha/management/commands/colabs_sem_furo.py:13`). |
| calendário → período | **a**, transporta recusas; outras exceções logadas e relançadas (`app/colaboradores/services/calendario.py:507`, `app/colaboradores/services/calendario.py:510`, `app/colaboradores/services/calendario.py:515`, `app/colaboradores/services/calendario.py:719`). |
| ficha/API `dia_do_colab` → período | **a**, campos desconhecidos nulos, rótulo preservado (`app/ponto/services/dia_do_colab.py:16`, `app/ponto/services/dia_do_colab.py:85`, `app/ponto/services/dia_do_colab.py:105`). |
| espelho `_fatos_periodo` → período | **a no miolo**; chamada posterior à folha pode quebrar, como acima (`app/ponto/services/espelho.py:653`, `app/ponto/services/espelho.py:880`, `app/ponto/services/espelho.py:1294`). |
| D4, sem carga → período | **a**, quando alcança A2 (`app/ponto/status_dia.py:302`, `app/ponto/status_dia.py:318`). |
| D4, lote com carga sem fatos → período | **a**, quando alcança A2 (`app/ponto/status_dia.py:311`, `app/ponto/status_dia.py:318`). |
| `feriado_reavaliar.espurios` → dia | **b** no command `reavaliar_ausencias_feriado`; em eventos é **c**, loga e deixa efeito pendente (`app/chamados/services/feriado_reavaliar.py:33`, `app/ponto/management/commands/reavaliar_ausencias_feriado.py:39`, `app/chamados/signals.py:332`, `app/ponto/portas/celula.py:499`). |
| semana do posto → dia | **a** devolvida como string de fato; recusa não vira `'trabalho'`; outros erros seguem fallback (`app/escala/services/semana_do_posto.py:177`, `app/escala/services/semana_do_posto.py:178`, `app/escala/services/semana_do_posto.py:180`). |
| `fatos_do_status` → dia | **b**, só para veredito `indefinida`; usado na lavra do cartório (`app/ponto/status_dia.py:177`, `app/ponto/status_dia.py:181`, `app/ponto/services/cartorio.py:932`, `app/ponto/services/cartorio.py:1292`, `app/ponto/services/cartorio.py:1853`). |
| `detectar_ausencias` → dia | **b** ao chegar ao gate de feriado (`app/ponto/management/commands/detectar_ausencias.py:616`). |
| deslogados → dia | **b**, commands `lavrar_deslogados` e serviço de pautas; API lê snapshot (`app/ponto/services/deslogados.py:166`, `app/ponto/management/commands/lavrar_deslogados.py:38`, `app/ponto/services/pautas_dos_deslogados.py:62`). |
| abriu/não bateu → dia | **b**, command `lavrar_abriu_nao_bateu`, salvo `--todos` desativar filtro (`app/ponto/services/abriu_nao_bateu.py:123`, `app/ponto/management/commands/lavrar_abriu_nao_bateu.py:30`). |
| faltas de hoje → dia | **b**, command `faltas_de_hoje_sem_cobranca`, só se sobreviver às guardas anteriores (`app/ponto/services/faltas_de_hoje.py:59`, `app/ponto/services/faltas_de_hoje.py:61`, `app/ponto/management/commands/faltas_de_hoje_sem_cobranca.py:16`). |
| aplicar precedência na célula → dia | Levanta localmente; `julgar_celula` captura/loga a consulta tardia e retorna out, **c** sem carimbo novo (`app/ponto/services/cartorio.py:1998`, `app/ponto/services/cartorio.py:2046`, `app/ponto/services/cartorio.py:2053`). No dado estável, recusa normalmente ocorre antes no builder; para alcançar captura tardia seria alimentação diferente entre consultas [nao verificado]. |
| férias `_repouso` → dia | **a** aviso, devolve `None` e não conclui repouso (`app/ferias/situacao.py:95`, `app/ferias/situacao.py:96`, `app/ferias/situacao.py:98`). |
| forma unitária D4 → lote D4 | Delega, sem nova regra (`app/ponto/status_dia.py:218`). |
| espelho `montar_semanas` → D4 | **a** no ramo antes reprovado; atualiza status/rótulo (`app/ponto/services/espelho.py:170`, `app/ponto/services/espelho.py:174`). |
| calendário → D4 | **a** com fatos e recusas alimentados (`app/colaboradores/services/calendario.py:606`). |
| API espelho v2 → D4 | lê status, traduz neutro; palavra separada preserva recusa somente onde espelho fornece dia (`app/api/views.py:1483`, `app/api/views.py:1500`, `app/api/views.py:1534`, `app/api/views.py:1586`). |
| auditoria furos → D4 | **c no relatório principal**, retém só status e cobra só irregular; recusa não é furo e colaborador pode sair do relatório filtrado (`app/relatorios/furos.py:123`, `app/relatorios/furos.py:127`, `app/relatorios/furos.py:128`, `app/relatorios/furos.py:141`). Calendário anexado preserva rótulo quando card incluído (`app/relatorios/furos.py:160`). |
| painel op → D4 | Só pares encerrados com entrada; normalmente não alcança A2, mostra lavrado ou sem-certidão normal (`app/core/services/painel_op.py:329`, `app/core/services/painel_op.py:334`, `app/core/services/painel_op.py:336`, `app/ponto/status_dia.py:295`). |
| compositor espelho → `dia_decidido.do_dia` | Não chama para recusa; **a** (`app/ponto/services/espelho.py:339`, `app/ponto/services/espelho.py:343`). |

Outros **b/c indiretos pelo builder**, relevantes à completude do censo:

- **b** POST `ponto/celula/veredito/`, veredito `trabalhou`: view chama serviço sem captura → `_grade(reais)` → `simular_dia.grade_dia` → `grade_espelho_janela` → builder sem recusas (`app/ponto/urls.py:58`, `app/ponto/views.py:3340`, `app/ponto/services/veredito_celula.py:241`, `app/ponto/services/simular_dia.py:7`, `app/escala/utils.py:2352`). É avaliação de ação, não deve inventar julgamento tolerante.
- **b** GET `ponto/flip-fila/` e command `flip_automatico`, candidato com marcos e tipo legado desconhecido: mesma grade sem captura (`app/ponto/urls.py:63`, `app/ponto/views.py:3305`, `app/ponto/services/flip_auto.py:303`, `app/ponto/management/commands/flip_automatico.py:32`).
- **c** command `reconciliar_grade`: recusa vira `GRADE_FAIL` e pula colaborador (`app/ponto/management/commands/reconciliar_grade.py:118`, `app/ponto/management/commands/reconciliar_grade.py:119`).
- **c** grade de apresentação do espelho: recusa do builder pode virar grade vazia, mas consulta independente posterior A2 preserva rótulo; não vira faixa válida (`app/ponto/services/espelho.py:534`, `app/ponto/services/espelho.py:572`, `app/ponto/services/espelho.py:653`).
- **b** command `detectar_par_relampago` usa builder no score (`app/ponto/management/commands/detectar_par_relampago.py:136`).
- **c** eventos de feriado e regeneração: recusa é log/contador de engolidos e reconciliação pendente, não uma nova natureza (`app/chamados/signals.py:332`, `app/ponto/portas/celula.py:499`, `app/core/observ.py:85`, `app/core/observ.py:90`).
- Chamado sem célula/ata conhecida chega indiretamente à A2 pela premissa regularizada; propagação depende do chamador da ação (`app/chamados/models.py:1305`, `app/chamados/services/feriado_reavaliar.py:33`). Usos na resolução, fechamento e reabertura: `app/chamados/models.py:183`, `app/chamados/models.py:738`, `app/chamados/models.py:1349`.

Sobre **`sem_certidao` e ESMERIL**: não vi prova de campo mentindo “falta de ata” versus “tipo sem faixa”. A definição vigente é ampla: sem evidência/decisão suficiente para afirmar cumprimento/falta/irregularidade; inclui impossibilidade de julgar a faixa (`app/ponto/status_dia.py:45`). Consumos:

- Contador calendário soma recusados junto com demais dias não certificados; não soma como falta/ausência (`app/colaboradores/services/calendario.py:773`, `app/colaboradores/services/calendario.py:785`, `app/colaboradores/services/calendario.py:789`).
- Badge/rodapé diz não certificado pela tag da autoridade, tooltip do dia mostra rótulo específico (`app/templates/colaboradores/partials/_calendario_grade.html:107`, `app/templates/colaboradores/partials/_calendario_grade.html:177`, `app/templates/ponto/espelho.html:151`).
- App traduz `sem_certidao` para `neutro`; não acusa falta/irregularidade (`app/ponto/status_dia.py:84`, `app/api/views.py:1501`, `app/api/views.py:1607`). Perde nome fora da janela conforme achado.
- Auditoria de furos não cobra nem conta `sem_certidao` como furo (`app/relatorios/furos.py:128`).
- “Resolver dia” é oferecido se passado + último recurso + `sem_certidao`; o vigia do fio lê esse mesmo campo (`app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`, `app/chamados/services/vigia_fio.py:271`). A ação `trabalhou` pode então levantar conforme cadeia acima.
- Não encontrei leitor desse **valor transitório** no domínio financeiro ou cartório: o D4 retorna dict em memória, a escrita de status é a porta, usando `status_da_lavra`/`status_do_admin`; folha não consulta D4 nesse caminho (`app/ponto/status_dia.py:198`, `app/ponto/portas/celula.py:958`, `app/ponto/portas/celula.py:970`). Persistência do estado de produção [nao verificado].
- D4 dá prioridade a turno aberto, batida em andamento e status lavrado antes de consultar A2; logo não promete que o próprio `status_rotulo` sempre seja recusa (`app/ponto/status_dia.py:283`, `app/ponto/status_dia.py:286`, `app/ponto/status_dia.py:290`). No espelho/app a palavra separada pode preservar a recusa; o painel restrito a dias com entrada pode mostrar só o estado lavrado, não o tipo desconhecido (`app/core/services/painel_op.py:329`, `app/core/services/painel_op.py:338`).

## 12/11 08:29 · sessao 736c4747c31e · - · - · origem=Ronald (tmux)

Itens 4 e 6: sem achado específico no delta, com os limites abaixo.

**4. Calendário sem recusa preserva os argumentos e a saída.**

| Argumento | Transporte atual e equivalência |
|---|---|
| `fatos`, `houve_trabalho`, `cobertura`, `decisao`, `falta_marcos` | O dicionário conserva os valores e o compositor os entrega a `do_dia` (`app/colaboradores/services/calendario.py:713`, `app/ponto/services/espelho.py:331`, `app/ponto/services/espelho.py:343`). |
| `pendente` | A chamada antiga omitia o argumento; o atual dicionário também omite `_pendente_do_dia`. O compositor lê `None`, igual ao default de `do_dia` (`app/ponto/services/espelho.py:334`, `app/ponto/services/dia_decidido.py:399`). |
| `ata_sem_turno` | O calendário conserva a mesma chamada ao helper; este devolve inteiro positivo ou zero. O `or 0` do compositor não altera esses resultados (`app/colaboradores/services/calendario.py:717`, `app/ponto/services/dia_decidido.py:179`, `app/ponto/services/espelho.py:345`). |
| `em_aberto` | Continua sendo pertinência da mesma data ao mesmo conjunto; `_em_aberto` já é `set` antes da chamada (`app/colaboradores/services/calendario.py:563`, `app/colaboradores/services/calendario.py:584`, `app/ponto/services/espelho.py:329`, `app/ponto/services/espelho.py:343`). |
| palavra/cor/veredito | O compositor apenas copia os três valores devolvidos por `do_dia`; o calendário lê essas novas chaves (`app/ponto/services/espelho.py:347`, `app/colaboradores/services/calendario.py:723`). |

Executei em memória 216 combinações de dias válidos — trabalho/folga/feriado, presença, aberto, ata, pareamento e marcos faltantes — comparando a chamada direta informada da base com o dicionário/compositor reais atuais; todas iguais. Não existe arquivo do ensaio: [nao verificado]. A equivalência estrutural está nos pontos acima. [nao verificado]

O teste C11 do calendário cobre **recusa**, substitui `do_dia` por `Mock` e exige que ele não seja chamado; portanto não sela a paridade de dias válidos (`app/ponto/tests/test_lastro_c11_sem_banco.py:189`, `app/ponto/tests/test_lastro_c11_sem_banco.py:219`, `app/ponto/tests/test_lastro_c11_sem_banco.py:230`).

**6. Não encontrei leitor da árvore que quebre especificamente com os campos `None`.**

O serviço entrega rótulo e campos de natureza nulos e evita `_veredito_do_dia` no dia recusado (`app/ponto/services/dia_do_colab.py:94`, `app/ponto/services/dia_do_colab.py:108`). Os leitores encontrados são:

- `/api/mensageria/dia-do-colab/`: serializa o dicionário inteiro, sem operar sobre os campos nulos (`app/api/urls.py:42`, `app/api/views_mensageria.py:1970`).
- `/api/mensageria/ficha-do-colab/`: inclui o dicionário em `hoje`; `_json` recorre por listas/dicionários e devolve `None` intacto (`app/api/urls.py:27`, `app/api/views_mensageria.py:1131`, `app/colaboradores/services/ficha.py:280`, `app/colaboradores/services/ficha.py:253`).
- Mensageria: transporta o JSON completo, envolve-o em contexto e serializa o contexto; não itera `escala.ausencias` nem desreferencia `escala.tipo` (`mensageria/nucleo/core_client.py:352`, `mensageria/nucleo/ferramentas.py:817`, `mensageria/nucleo/ferramentas.py:1081`, `mensageria/nucleo/ferramentas.py:1771`, `mensageria/nucleo/ferramentas.py:1800`, `mensageria/nucleo/copiloto.py:379`).

O teste existente verifica o rótulo, `tipo is None` e ausência de chamada ao veredito, com ORM substituído; HTTP completo e comportamento de consumidores externos: [nao verificado] (`app/ponto/tests/test_lastro_c11_sem_banco.py:245`, `app/ponto/tests/test_lastro_c11_sem_banco.py:251`).

Captura ampla adicional para o censo: o calendário captura falha da consulta de dias em aberto, loga e entrega conjunto vazio com `em_aberto_perguntado=False`; alcançar esse `except` especificamente com `TipoSemFaixa` precisa confirmação da cadeia do motor/grade (`app/colaboradores/services/calendario.py:568`, `app/colaboradores/services/calendario.py:585`, `app/colaboradores/services/calendario.py:592`, `app/ponto/services/espelho.py:1174`).

## 12/11 08:28 · sessao 0c94d4952dc7 · - · - · origem=Ronald (tmux)

**Item 5 — bloqueia: recusa vira “dia explicado”.** `_fato_do_dia` devolve `erro.rotulo`; o único teste do consumidor é `fato != 'trabalho'`. Assim, a recusa entra em `explicados`/`explicado_por`, deixa de entrar em `buracos`, não participa das propostas e pode fazer o posto desaparecer do contador. Há dois significados no mesmo canal: faixa reconhecida e impossibilidade de reconhecer a faixa (`app/escala/services/semana_do_posto.py:152`, `app/escala/services/semana_do_posto.py:154`, `app/escala/services/semana_do_posto.py:179`, `app/escala/services/semana_do_posto.py:197`, `app/escala/services/semana_do_posto.py:237`).

Contraexemplo reproduzido em memória, com A2, `grade_do_posto`, `propostas_cruzadas` e `contador_por_posto` reais: um único `col17` previsto, sem batida, com ausência `fantasma_xyz` → `buracos=[]`, `explicados=[recusa]`, `propostas=[]`, `contador=[]`. A API com posto publica `dias_sem_explicacao=0`; sem posto, o contador omite esse posto (`app/api/views_mensageria.py:2083`, `app/api/views_mensageria.py:2121`, `app/api/views_mensageria.py:2123`). O teste atual exige `buracos=[]`, portanto certifica esse comportamento semântico problemático; seu mutante comprova transporte do rótulo, não distinção entre recusa e explicação válida (`app/ponto/tests/test_lastro_c11_sem_banco.py:270`, `app/ponto/tests/test_lastro_c11_sem_banco.py:271`, `app/ponto/tests/test_lastro_c11_sem_banco.py:80`).

A chave nova `explicados` **tem consumidor por transporte integral**: cliente retorna todo JSON; ferramenta devolve o bloco inteiro; montagem agrega esse bloco ao contexto; copiloto serializa todo contexto. Não encontrei leitura tipada de `explicados`, template ou JavaScript específico; a fala efetiva do modelo sobre a recusa é [nao verificado] (`mensageria/nucleo/core_client.py:308`, `mensageria/nucleo/ferramentas.py:879`, `mensageria/nucleo/ferramentas.py:884`, `mensageria/nucleo/ferramentas.py:422`, `mensageria/nucleo/copiloto.py:379`). A instrução específica continua pedindo número de dias sem explicação e pares prováveis, sem tratamento específico dessa recusa (`mensageria/nucleo/copiloto.py:280`).

Existe ainda fallback preexistente para outros erros: `Exception` vira `'trabalho'`, portanto `RuntimeError` do juiz converte um dia desconhecido em buraco. A captura específica impede isso para `TipoSemFaixa` nesta volta (`app/escala/services/semana_do_posto.py:178`, `app/escala/services/semana_do_posto.py:180`).

**Item 7 — cura consistente no universo pesquisado; sem órfão operacional localizado.** Cartório declara, incrementa e imprime `colabs_isolados_tipo_sem_faixa`; sua unidade é colaborador isolado por passada, sem filtro de ativo (`app/ponto/services/cartorio.py:1954`, `app/ponto/management/commands/processar_cartorio.py:64`, `app/ponto/management/commands/processar_cartorio.py:73`, `app/ponto/management/commands/processar_cartorio.py:75`, `app/ponto/management/commands/processar_cartorio.py:80`).

O nome antigo operacional permanece somente no vigia e no rótulo da pauta: conta códigos ativos, publica esse universo na API e na revisão semanal. O consumidor da API lê explicitamente `vigia_ausencia.contadores`, não `cartorio.contadores`; não passou a ler outro universo (`app/ponto/services/vigia_ausencia.py:160`, `app/ponto/services/vigia_ausencia.py:168`, `app/api/views_mensageria.py:1416`, `app/api/views_mensageria.py:1422`, `app/ponto/services/vigia_ausencia.py:259`, `app/pautas/semanal.py:64`).

Os contratos do cartório foram migrados; o teste novo exige ausência do alias antigo tanto no dicionário como no stdout. Os testes que preservam o nome antigo pertencem ao vigia (`app/ponto/tests/test_lastro_c10_sem_banco.py:218`, `app/ponto/tests/test_lastro_c10_sem_banco.py:220`, `app/ponto/tests/test_lastro_c11_sem_banco.py:169`, `app/ponto/tests/test_lastro_c11_sem_banco.py:172`, `app/ponto/tests/test_lastro_faixa_sem_default.py:90`, `app/ponto/tests/test_a6_vigia_familia_ausencia.py:128`).

A ocorrência documental atual do nome antigo descreve o vigia, não o contador do cartório; o cron cadastra a execução e o arquivo de saída sem analisar esse campo. Buscas globais pelos dois nomes não localizaram parser adicional em bin, placar ou crons. Leitor externo à cópia: [nao verificado] (`app/docs/RELATO.md:37`, `app/config/crons.py:616`).

## 12/11 08:22 · sessao b901d43194d7 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Cura deixada na árvore, com censos e resposta do PDF em [LASTRO_C11.txt:6](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C11.txt:6).

- 55 testes sem banco passaram; Ruff passou ([registro:243](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C11.txt:243)). [nao verificado]
- RED contra c10 reproduzido; 10 mutantes falharam por asserção, com comandos registrados ([registro:209](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C11.txt:209)).
- Par financeiro da competência 11: **[nao verificado]** ([limites:334](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/LASTRO_C11.txt:334)).

## 12/11 08:12 · sessao ee40839f9d72 · - · - · origem=Ronald (tmux)

Censo na base c10, somente leitura:

| Chamador direto da A2/faixa | Destino e tratamento |
|---|---|
| `ponto/status_dia.py:298`, `:304` | Leitura D4 sem `recusas=`; levanta em dia sem batida/lavra. Corrigir os dois ramos. |
| `colaboradores/services/calendario.py:507` | Tela consulta A2 sem `recusas=` e propaga (`:515`); indexa `_fatos[data]` (`:661`) e chama `_do_dia` (`:716`). Precisa transportar a recusa ao D4 (`:606`) e à palavra, preservando `KeyError` para fato ausente sem recusa. |
| `ponto/services/dia_do_colab.py:15` | Leitor da API (`api/views_mensageria.py:1970`) e ficha (`colaboradores/services/ficha.py:280`). Levanta; deve devolver rótulo da A2 sem concluir precedência. |
| `escala/services/semana_do_posto.py:177` | API chama grade (`api/views_mensageria.py:2113`) e propostas (`:2114`). Relança em `semana_do_posto.py:178`; precisa resposta nomeada sem afirmar faixa válida. |
| `ponto/services/espelho.py:653` | Já consulta com `recusas=` (`:654`); palavra recebe `erro.rotulo` (`:880`, `:341`) sem chamar `do_dia` no recusado. |
| `ferias/situacao.py:95` | Captura específica (`:96`) e devolve aviso nomeado sem concluir descanso (`:98`). |
| `ponto/services/vigia_ausencia.py:162` | Captura específica (`:163`) e conta códigos ativos (`:160`, `:168`); manter. |
| `escala/utils.py:1209` | Builder compartilhado; julgamento/grade que alimenta dinheiro. Manter recusa, sem inventar fato válido. |
| `ponto/status_dia.py:181` | Insumo da lavra, chamado pelo cartório (`cartorio.py:932`, `:1292`, `:1853`); manter recusa. |
| `ponto/services/cartorio.py:1998` | Escrita de precedência na célula (`:1999`); manter recusa. |
| `chamados/services/feriado_reavaliar.py:33` | Julga premissa antes de reconciliar (`:43`, `:94`); manter recusa. |
| `ponto/management/commands/detectar_ausencias.py:616` | Julgamento do emissor; manter recusa. |
| `ponto/services/ausencia.py:1310` | Porta converte em `AusenciaInvalida` (`:1312`); corrigir brecha de `campos` conforme pedido. |
| `ponto/precedencia.py:544`, `:565` | Veto operacional; manter recusa. Chamadores: `ausencia.py:380`, `declarar_dia.py:120`, `escala/folgas.py:310`, `:336`, `ferias/views.py:254`. |
| `folha/services/sem_furo.py:25` | Apuração de comando (`folha/management/commands/colabs_sem_furo.py:13`); nova semântica numérica da recusa não autorizada. |
| `ponto/services/faltas_de_hoje.py:61` | Apuração de comando (`ponto/management/commands/faltas_de_hoje_sem_cobranca.py:16`); não é chamada de request identificada. |
| `ponto/services/deslogados.py:166` | Apuração/lavra (`ponto/management/commands/lavrar_deslogados.py:38`, `ponto/services/pautas_dos_deslogados.py:62`). |
| `ponto/services/abriu_nao_bateu.py:123` | Apuração/lavra (`ponto/management/commands/lavrar_abriu_nao_bateu.py:30`); API lê snapshot (`api/views_mensageria.py:139`), não executa A2 no request. |

D4 chega a leitores por `ponto/services/espelho.py:170`, `api/views.py:1483`, `colaboradores/services/calendario.py:606`, `relatorios/furos.py:123` e `core/services/painel_op.py:336`. A forma unitária delega ao lote (`ponto/status_dia.py:218`).

Fechamento continua propagando nas três capturas (`ponto/services/fechamento.py:1081`, `:1089`, `:1308`); TXT chama a leitura antes de gerar (`folha/views.py:208`), e simulação chama fechamento (`folha/management/commands/simular_folha.py:123`, `:130`).

`dia_decidido.do_dia` tem dois chamadores operacionais: espelho (`ponto/services/espelho.py:343`) e calendário (`colaboradores/services/calendario.py:716`). Os `do_dia` de calculador, pergunta no app e vigia são homônimos distintos (`ponto/calculador/regras.py:576`, `chamados/services/pergunta_no_app.py:211`, `ponto/services/vigia_entrada.py:111`).

Execução HTTP/ORM e ocorrência produtiva: [nao verificado].

## 12/11 08:05 · sessao 553a6d49936c · FIO-E0-V4-E4B · construtor · origem=Code

Cura aplicada. A [entrega completa](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_entrega.txt:7) contém as 69 classificações R/C, escritores/leitores e comandos individuais. [nao verificado]

- GREEN: [158 testes de memória](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_memoria.txt:59) e [47 contratos estáticos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_selos.txt:6). [nao verificado]
- [21 mutantes c14](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_entrega.txt:123) morderam por assertion, sem ERROR.

Suíte inteira, ORM e medição SQL do C8: **[nao verificado]**; Django está ausente e banco foi vedado. Sem commit, conforme a proibição explícita das REGRAS. [Pendências para o Code](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c14_entrega.txt:194). [nao verificado]

## 12/11 08:02 · sessao f2aeed7d1ba0 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVA c10:** o rótulo da recusa não garante que a tela renderize: uma consulta posterior volta a levantar `TipoSemFaixa` (`app/ponto/views.py:184`, `app/ponto/services/espelho.py:170`, `app/ponto/status_dia.py:304`). O par financeiro da competência 11 exigido pelo aval não está demonstrado no dado fornecido [nao verificado].

1. **PORTA e escritores**

   A criação, inclusive inativa, e a ativação explícita consultam `faixa_de` antes da escrita; a recusa usa `AusenciaInvalida(codigo='tipo_sem_faixa')` (`app/ponto/services/ausencia.py:1308`, `app/ponto/services/ausencia.py:1312`).

   Não encontrei segundo escritor operacional de `TipoAusencia` nas views, commands ou importadores pesquisados. O admin Django não registra esse modelo; os dois ramos da view passam pela porta. A migration 0052 escreve diretamente código/efeito/ativo, mas suas sementes têm faixa; 0062/0063 alteram flags de tipos existentes (`app/ponto/admin.py:11`, `app/ponto/admin.py:37`, `app/ponto/views_tipos_ausencia.py:87`, `app/ponto/views_tipos_ausencia.py:98`, `app/ponto/migrations/0052_seed_tipoausencia.py:33`, `app/ponto/precedencia.py:52`, `app/ponto/migrations/0062_fim_dos_17_tipos.py:51`, `app/ponto/migrations/0063_atestado_aceita_horas.py:20`).

   **Achado — fila, brecha da própria porta.** Ela valida `tipo.codigo` antes de aplicar indiscriminadamente `campos`. Contraexemplo reproduzido em memória: um objeto `abono` aceita `campos={'codigo':'fantasma_xyz','ativo':True}` e chama `save()` com o código desconhecido ativo. A view atual não fornece `codigo` nesse dicionário; exploração por essa tela não foi demonstrada (`app/ponto/services/ausencia.py:1310`, `app/ponto/services/ausencia.py:1314`, `app/ponto/views_tipos_ausencia.py:67`).

   A view não lê `.codigo`: entrega `str(erro)` ao feedback. Para `fantasma_xyz`, o texto é **“Tipo sem faixa de precedencia: fantasma_xyz”**; o JavaScript escreve na caixa de erro e no toast (`app/ponto/precedencia.py:100`, `app/ponto/views_tipos_ausencia.py:89`, `app/ponto/views_tipos_ausencia.py:100`, `app/core/respostas.py:65`, `app/templates/ponto/tipos_ausencia.html:73`, `app/templates/ponto/tipos_ausencia.html:86`).

   Os leitores gerais de `AusenciaInvalida.codigo` encontrados são API, formulário de ausência e comando de rederivação; eles não são os chamadores do cadastro de tipos. O novo valor tem assert específico no ensaio c10 (`app/api/views.py:1819`, `app/ponto/views.py:2806`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:172`, `app/ponto/tests/test_lastro_c10_sem_banco.py:86`).

2. **Cartório: isolamento, persistência e contadores**

   O `except` envolve **`julgar_colab` inteiro**, não cada dia nem a empresa inteira. Captura somente `TipoSemFaixa`, incrementa uma vez por colaborador recusado, loga pk/código e continua (`app/ponto/management/commands/processar_cartorio.py:64`, `app/ponto/management/commands/processar_cartorio.py:70`, `app/ponto/management/commands/processar_cartorio.py:72`).

   No caminho normal identificado, a grade do período consulta a A2 antes da primeira lavra. Um tipo desconhecido interrompe essa preparação: nenhuma ata nova daquele colaborador é escrita por esse caminho; as antigas permanecem (`app/ponto/services/cartorio.py:1168`, `app/escala/utils.py:1209`, `app/ponto/precedencia.py:419`, `app/ponto/services/cartorio.py:1288`).

   Isso **não constitui rollback por colaborador**. Se a exceção surgir depois de uma gravação, ela pode permanecer: o `finally` limpa contexto/filas, sem desfazer persistência. Há `atomic()` por salvamento de célula/evento, não envolvendo o colaborador inteiro (`app/ponto/services/cartorio.py:1093`, `app/ponto/services/cartorio.py:1116`, `app/ponto/portas/celula.py:899`, `app/ponto/portas/celula.py:904`). Nesse eventual estado parcial, as atas já salvas continuam disponíveis ao leitor da célula (`app/escala/services/leitor_celula.py:418`).

   Não há fundamento para atribuir conexão SQL quebrada à recusa normal: `TipoSemFaixa` é `ValueError` levantado pelo mapa. Continuidade com ORM e verificação de `TransactionManagementError` não foram executadas [nao verificado] (`app/ponto/precedencia.py:96`, `app/ponto/precedencia.py:108`).

   **Achado — fila, campo com dois significados.** `tipos_ativos_sem_faixa` conta colaboradores recusados no cartório e códigos ativos no vigia; o cartório sequer filtra `ativo`. Contraexemplos: um código em três colaboradores pode produzir **3/1**; um código inativo com ausência pode produzir **1/0** (`app/ponto/management/commands/processar_cartorio.py:73`, `app/ponto/services/cartorio.py:1952`, `app/ponto/services/vigia_ausencia.py:160`, `app/ponto/services/vigia_ausencia.py:168`, `app/ponto/precedencia.py:419`).

   Ambos chegam à mesma função real `faixa_de`: diretamente no vigia e pela grade/A2 no cartório. Portanto, são universos diferentes, sem evidência de segundo juiz de faixa (`app/ponto/services/vigia_ausencia.py:162`, `app/escala/utils.py:1209`, `app/ponto/precedencia.py:419`). O cartório publica seu contador no stdout; API/revisão semanal leem o contador do vigia (`app/ponto/management/commands/processar_cartorio.py:80`, `app/api/views_mensageria.py:1422`, `app/ponto/services/vigia_ausencia.py:259`).

3. **Espelho/PDF**

   A A2 recebe `recusas=`, exclui o dia recusado dos fatos e não ordena sua precedência; o compositor publica o rótulo pronto sem chamar `do_dia` para ele. O PDF reutiliza esse compositor (`app/ponto/precedencia.py:420`, `app/ponto/precedencia.py:425`, `app/ponto/services/espelho.py:339`, `app/relatorios/pdf_espelho.py:597`).

   **Achado — bloqueia, recusa tratada no miolo e relançada na tela.** Após obter o espelho, a view chama `montar_semanas`; este consulta `status_dos_dias_lavrados` sem fornecer fatos/recusas. Quando chega a um dia sem batida e sem status lavrado, consulta novamente a A2, agora sem `recusas=` (`app/ponto/views.py:158`, `app/ponto/views.py:184`, `app/ponto/services/espelho.py:170`, `app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:296`, `app/ponto/status_dia.py:304`).

   **Contraexemplo:** ausência aprovada `fantasma_xyz` em dia passado sem batida e sem lavra. A primeira passagem produz “Tipo sem faixa: fantasma_xyz”; `montar_semanas` levanta novamente antes do `render`. Reproduzi essa sequência em memória com A2, D4 e `montar_semanas` reais; a execução HTTP completa permanece [nao verificado] (`app/ponto/precedencia.py:421`, `app/ponto/views.py:193`).

   **Achado — fila, paridade do PDF para dias válidos não demonstrada.** A troca de órfãs do builder por `orfas_grade` troca efetivamente a fonte: a tela normalmente lê a ata. Sem célula, esse leitor entrega órfãs vazias; o builder conserva batidas não consumidas como órfãs (`app/relatorios/pdf_espelho.py:477`, `app/ponto/services/espelho.py:522`, `app/escala/services/leitor_celula.py:394`, `app/escala/services/leitor_celula.py:420`, `app/escala/utils.py:1849`).

   **Contraexemplo de código:** builder com órfã `07:02`, ata/tela sem ela, período aberto iniciado às `07:02`. A substituição altera sua inclusão em `turnos_abertos`, depois `datas_turno_aberto`, podendo alterar dias em aberto e palavra. Nenhum tipo sem faixa é necessário; ocorrência em produção e magnitude são [nao verificado] (`app/relatorios/pdf_espelho.py:178`, `app/relatorios/pdf_espelho.py:515`, `app/relatorios/pdf_espelho.py:576`, `app/relatorios/cartao_pela_celula.py:418`, `app/relatorios/pdf_espelho.py:597`).

   Órfãs e palavra passam pela autoridade da tela, mas o PDF ainda apura/soma seu resumo e calcula saldo antes de `folha_manda`; essas derivações já existiam no recorte da base (`app/relatorios/pdf_espelho.py:418`, `app/relatorios/pdf_espelho.py:504`, `app/relatorios/pdf_espelho.py:531`, `app/ponto/tests/lastro_c9_recortes.json:27`). Paridade integral de horas/saldo/totais com a base: [nao verificado].

   Sem achado específico na remoção de `_realizado`: era helper local sem chamada no recorte fornecido da base; sua remoção isolada não explica mudança de saída (`app/ponto/tests/lastro_c9_recortes.json:27`).

4. **Fechamento e competência**

   As três capturas alteradas relançam `TipoSemFaixa`. Se ela ocorrer, interrompe o restante da chamada — empresa inteira ou subconjunto escolhido —, sem representar uma tranca formal da competência (`app/ponto/services/fechamento.py:555`, `app/ponto/services/fechamento.py:558`, `app/ponto/services/fechamento.py:1081`, `app/ponto/services/fechamento.py:1089`, `app/ponto/services/fechamento.py:1308`).

   Não há atomicidade de lote: A pode estar salvo, B pode ganhar linha `aberto` antes da falha, e C não ser processado. Isso é consequência de propagação, não uma decisão nova de pagar/abonar/zerar B (`app/ponto/services/fechamento.py:899`, `app/ponto/services/fechamento.py:1015`, `app/ponto/services/fechamento.py:1149`, `app/ponto/services/fechamento.py:1308`).

   Os chamadores relevantes têm estes destinos:

   - View e command de recalcular deixam a recusa subir (`app/ponto/views.py:1252`, `app/ponto/management/commands/recalcular_fechamento.py:118`).
   - Exportação chama leitura da empresa antes de classificar/gerar TXT; uma recusa interrompe esse fluxo (`app/folha/views.py:208`, `app/folha/views.py:212`).
   - `simular_folha` chama o fechamento antes do seu `try` posterior (`app/folha/management/commands/simular_folha.py:123`, `app/folha/management/commands/simular_folha.py:130`, `app/folha/management/commands/simular_folha.py:132`).
   - O callback por evento captura `Exception` e loga após o fato gravado: nesse chamador a recusa não chega ao usuário (`app/ponto/services/fechamento.py:1817`, `app/ponto/services/fechamento.py:1825`).
   - O cadastro declara ausência de cron ordinário de recalcular (`app/config/crons.py:1130`).

   Deixar levantar corresponde à instrução D do pedido; o isolamento por colaborador foi exigido especificamente para o cartório. A condição financeira de pouso, item 6, continua sem par medido no dado fornecido [nao verificado]. [nao verificado]

5. **Fonte única e defaults**

   Sem achado de default de **faixa** para tipo desconhecido: o mapa é consultado por indexação em `faixa_de`; A2/veto passam por essa função. O fallback `'trabalho'` da semana do posto relança expressamente `TipoSemFaixa` (`app/ponto/precedencia.py:108`, `app/ponto/precedencia.py:110`, `app/ponto/precedencia.py:419`, `app/ponto/precedencia.py:544`, `app/escala/services/semana_do_posto.py:178`).

   **Achado — fila, fallback de efeito preexistente.** `_do_banco` captura qualquer exceção e `vigente` substitui falha/resultado vazio pelo catálogo congelado; `efeito_vigente` lê esse resultado. Contraexemplo: `abono` alterado para descontar, seguido de falha na leitura, pode voltar a responder `ABONA`. Isso é fallback de efeito, não de faixa (`app/ponto/catalogo/ausencias.py:102`, `app/ponto/catalogo/ausencias.py:254`, `app/ponto/catalogo/ausencias.py:283`, `app/ponto/catalogo/ausencias.py:298`).

6. **Mordida dos testes** [nao verificado]

   Executei os três harnesses juntos: **43 testes, OK, RC 0**; seus mecanismos executam AST do fonte com alimentação substituída (`app/ponto/tests/test_lastro_c5_sem_banco.py:108`, `app/ponto/tests/test_lastro_c10_sem_banco.py:75`, `app/ponto/tests/test_lastro_c7_sem_banco.py:70`).

   Os mutantes executados ficaram vermelhos por asserção: retirar a guarda da porta, relançar no isolamento do cartório, restaurar o default `'atestado'` e retirar o rótulo. Portanto, há mordida de comportamento, além de inspeção textual (`app/ponto/tests/test_lastro_c10_sem_banco.py:31`, `app/ponto/tests/test_lastro_c10_sem_banco.py:36`, `app/ponto/tests/test_lastro_c10_sem_banco.py:43`, `app/ponto/tests/test_lastro_c10_sem_banco.py:280`, `app/ponto/tests/test_lastro_c5_sem_banco.py:50`).

   **Limite que bloqueia a conclusão de integração:** o teste B substitui `julgar_colab`; o teste PDF executa somente um prefixo com espelho mockado; o teste D injeta a exceção nas capturas, sem motor/ORM. Eles não certificam persistência, PDF completo nem o caminho posterior de `montar_semanas` que falha (`app/ponto/tests/test_lastro_c10_sem_banco.py:180`, `app/ponto/tests/test_lastro_c10_sem_banco.py:311`, `app/ponto/tests/test_lastro_c10_sem_banco.py:319`, `app/ponto/tests/test_lastro_c10_sem_banco.py:349`).

   O `test_05` cria `reciclagem` pela porta, aprova ausência e confere RD; os testes 07/08 são o par de criação/ativação recusadas. O código escolhido tem faixa e não está na semente 0052 (`app/relatorios/tests/test_rotulo_ausencia_pdf.py:52`, `app/relatorios/tests/test_rotulo_ausencia_pdf.py:64`, `app/relatorios/tests/test_rotulo_ausencia_pdf.py:73`, `app/ponto/precedencia.py:71`, `app/ponto/migrations/0052_seed_tipoausencia.py:10`). Execução ORM desses testes: [nao verificado].

   Os RED B/C também falharam com `--red-c9`, mas esse mecanismo substitui recortes congelados; não executa um checkout independente da c9 (`app/ponto/tests/test_lastro_c10_sem_banco.py:22`, `app/ponto/tests/test_lastro_c10_sem_banco.py:28`).

7. **Classes de reprovação**

   Os achados concretos são: **integração da recusa incompleta — bloqueia**; **contador com duas unidades, brecha interna da porta e fallback de efeito — fila**. Não encontrei novo juiz de faixa nem segundo escritor operacional de tipos; a consulta duplicada na tela usa a própria A2, mas perde o tratamento de recusa (`app/ponto/status_dia.py:304`, `app/ponto/management/commands/processar_cartorio.py:73`, `app/ponto/services/vigia_ausencia.py:168`, `app/ponto/services/ausencia.py:1314`, `app/ponto/catalogo/ausencias.py:283`).

   Sem achado de relógio novo no delta do PDF: `_hj` deriva do `agora` recebido. Os `except` que ainda absorvem falhas foram identificados no catálogo e no callback financeiro; as três capturas c10 do fechamento propagam a exceção (`app/relatorios/pdf_espelho.py:475`, `app/ponto/catalogo/ausencias.py:254`, `app/ponto/services/fechamento.py:1825`, `app/ponto/services/fechamento.py:1089`).

## 12/11 07:59 · sessao c2fbd7863c76 · - · - · origem=Ronald (tmux)

1. **Bloqueia — tela admin ainda pode levantar `TipoSemFaixa` antes de renderizar.** A view obtém o espelho com a recusa rotulada (`app/ponto/views.py:158`), mas depois chama `montar_semanas` (`app/ponto/views.py:184`). Esta chama `status_dos_dias_lavrados` sem fornecer fatos ou recusas (`app/ponto/services/espelho.py:170`). Ao alcançar dia sem batida e sem status lavrado, o leitor pergunta novamente à A2 pela janela inteira, sem `recusas=` (`app/ponto/status_dia.py:296`, `app/ponto/status_dia.py:304`). A A2 relança nessa modalidade (`app/ponto/precedencia.py:421`). Contraexemplo: ausência aprovada `legado_rd` em dia passado sem batida e célula sem `status_dia`; a palavra pode estar pronta no miolo, mas a view falha antes do `render` (`app/ponto/views.py:193`). O PDF não chama `montar_semanas`; chama somente o miolo (`app/relatorios/pdf_espelho.py:476`).

2. **Fila — paridade PDF sem tipo inválido não está provada; há mudança concreta de fonte.** Agora órfãs vêm da tela (`app/relatorios/pdf_espelho.py:477`), que normalmente lê a ATA (`app/ponto/services/espelho.py:522`, `app/escala/services/leitor_celula.py:420`). Antes vinham diretamente do builder, conforme o diff fornecido `[nao verificado: checkout independente da base]`. Essas listas não têm contrato de igualdade: sem célula, o leitor entrega dia sem órfãs (`app/escala/services/leitor_celula.py:390`, `app/escala/services/leitor_celula.py:394`); o builder conserva batidas não consumidas como órfãs (`app/escala/utils.py:1832`, `app/escala/utils.py:1849`).

   Contraexemplo condicional: builder classifica `07:02` como órfã e a ATA lida pela tela não; um período aberto iniciado às `07:02` passa a entrar em `_abertos_ts` (`app/relatorios/pdf_espelho.py:515`), muda `datas_turno_aberto` (`app/relatorios/pdf_espelho.py:576`) e pode mudar `dias_em_aberto`, seu badge e a palavra da linha (`app/relatorios/cartao_pela_celula.py:418`, `app/relatorios/pdf_espelho.py:750`, `app/relatorios/pdf_espelho.py:597`). Nenhum tipo sem faixa é necessário. Existência desse estado em produção e delta de horas/saldo/dinheiro: `[nao verificado]`.

3. **O selo de PDF não mede essa paridade.** Executa somente o prefixo até `_orfas_hhmm` (`app/ponto/tests/test_lastro_c10_sem_banco.py:302`, `app/ponto/tests/test_lastro_c10_sem_banco.py:319`), com `espelho_do_colab` substituído por `Mock` (`app/ponto/tests/test_lastro_c10_sem_banco.py:311`). Não executa os consumidores de órfãs, o resumo final nem a renderização; portanto o verde desse teste não certifica os efeitos descritos acima (`app/ponto/tests/test_lastro_c10_sem_banco.py:324`).

4. **Fonte única melhorou para órfãs e palavra, mas o PDF ainda calcula seu próprio resumo.** Lê os dias do espelho (`app/relatorios/pdf_espelho.py:561`), reutiliza a mesma porta de composição da palavra (`app/relatorios/pdf_espelho.py:597`) e apenas imprime `palavra_dia` (`app/relatorios/pdf_espelho.py:882`). Ainda executa `apurar_por_fatia` (`app/relatorios/pdf_espelho.py:418`), soma períodos (`app/relatorios/pdf_espelho.py:504`) e calcula saldo (`app/relatorios/pdf_espelho.py:531`) antes de `folha_manda` (`app/relatorios/pdf_espelho.py:587`). Esses caminhos já aparecem na base congelada (`app/ponto/tests/lastro_c9_recortes.json:27`); não são novos no delta.

5. **Sem achado na remoção de `_realizado`.** Era função local sem chamadas no recorte da base congelada (`app/ponto/tests/lastro_c9_recortes.json:27`); o PDF atual recebe minutos e pago pelo dict da tela (`app/ponto/services/espelho.py:792`, `app/ponto/services/espelho.py:820`). A remoção do helper, isoladamente, não muda saída; fidelidade do JSON ao commit real `[nao verificado]`.

6. **Sem relógio novo nesse delta.** `_hj` deriva do `agora` recebido e a chamada continua terminando em ontem (`app/relatorios/pdf_espelho.py:475`, `app/relatorios/pdf_espelho.py:476`). A mesma chamada foi apenas antecipada segundo o diff fornecido `[nao verificado: checkout independente da base]`.

## 12/11 07:59 · sessao 0214359824a2 · - · - · origem=Ronald (tmux)

2. O isolamento envolve **o colaborador inteiro**, na chamada única de `julgar_colab`; captura exclusivamente `TipoSemFaixa`, incrementa uma vez e registra pk/código antes de continuar (`app/ponto/management/commands/processar_cartorio.py:64`, `:70`, `:72`–`:80`).

No caminho normal desse erro, a grade inteira é montada antes de qualquer lavra (`app/ponto/services/cartorio.py:1168`); ela chama A2 (`app/escala/utils.py:1209`), que levanta em `faixa_de` (`app/ponto/precedencia.py:419`–`:422`). Portanto, nesse caminho, **nenhuma ata nova daquele colaborador é gravada**, e as anteriores permanecem; a primeira lavra/carimbo vem depois (`app/ponto/services/cartorio.py:1288`, `:1310`). Não existe rollback de colaborador: se a exceção ocorrer numa fase posterior, escritas anteriores podem permanecer; o `finally` limpa filas e contexto, mas não desfaz persistência (`app/ponto/services/cartorio.py:1093`–`:1118`).

Não há `atomic()` envolvendo o comando nem `julgar_colab`; existe atomicidade menor na porta que salva célula e evento (`app/ponto/portas/celula.py:899`–`:907`). `TipoSemFaixa` é `ValueError` puro (`app/ponto/precedencia.py:96`), levantado pela consulta ao mapa (`:105`–`:110`): esse caminho não evidencia SQL abortado ou `TransactionManagementError`. Ensaio ORM real da continuidade: **[nao verificado]**.

Os contadores são diferentes: cartório mede **colaboradores recusados na passada** (`app/ponto/services/cartorio.py:1952`–`:1954`; comando `:73`), vigia mede **códigos ativos sem faixa** (`app/ponto/services/vigia_ausencia.py:159`–`:168`). Ambos chegam à autoridade `faixa_de`: diretamente no vigia (`:162`), por grade/A2 no cartório (`app/escala/utils.py:1209`; `app/ponto/precedencia.py:419`). Leitores: resumo stdout do comando (`:75`–`:80`); API do vigia (`app/api/views_mensageria.py:1416`–`:1424`) e revisão semanal (`app/ponto/services/vigia_ausencia.py:259`–`:262`). Consumo automático do stdout em produção: **[nao verificado]**.

**Achado, fila — campo com dois significados:** `tipos_ativos_sem_faixa` designa unidades diferentes e o cartório nem exige que o cadastro esteja ativo. Contraexemplo: um código X usado por três colaboradores produz cartório=3 e vigia=1; X inativo, mas com ausência aprovada, pode produzir cartório=1 e vigia=0, porque a A2 consulta o código sem consultar `.ativo` (`app/ponto/management/commands/processar_cartorio.py:73`; `app/ponto/services/vigia_ausencia.py:160`; `app/ponto/precedencia.py:419`). Isso não introduz segundo juiz de faixa, mas cria ambiguidade semântica no nome compartilhado.

4. A recusa sobe pelas três capturas alteradas (`app/ponto/services/fechamento.py:1081`, `:1089`, `:1308`), interrompendo **todo o restante do universo daquela chamada**, que pode ser empresa inteira ou subconjunto (`:555`–`:558`, `:620`). Não é uma tranca de competência: são interrompidos o cálculo e o retorno, sem alteração específica de status por essa recusa (`:1308`–`:1318`).

**Persistência parcial concreta:** não existe atomicidade de lote. A pode ser salvo (`app/ponto/services/fechamento.py:1149`), B pode ganhar uma linha `status='aberto'` pelo `get_or_create` (`:899`–`:902`) e só depois falhar ao montar sua grade (`:1015`); C não será processado (`:1308`–`:1309`). Sem fotografia anterior para B, pode sobrar linha com defaults; com fotografia anterior, ela pode permanecer velha. Não houve regra de dinheiro nova para resolver B (`:1089`–`:1104`).

Chamadores relevantes:

- Botão de recalcular: chamada sem captura da recusa; deixa de retornar sucesso (`app/ponto/views.py:1252`–`:1253`). Resposta HTTP efetivamente observada: **[nao verificado]**.
- Gerar TXT: recalcula empresa inteira em leitura antes de classificar/aprovar; a recusa impede chegar à geração e ao carimbo (`app/folha/views.py:207`–`:212`, `:250`–`:252`).
- Comando `recalcular_fechamento --apply`: propaga antes de fotografar o depois (`app/ponto/management/commands/recalcular_fechamento.py:118`–`:122`).
- `simular_folha`, modos recalcular/lido: chamadas fora do `try` posterior; propaga (`app/folha/management/commands/simular_folha.py:121`–`:135`).
- Evento de batida/célula/HE: callback **captura qualquer Exception e loga**, preservando o fato já gravado; a recusa não sobe ao usuário por esse chamador (`app/ponto/services/fechamento.py:1814`–`:1828`).
- Regeneração em exportada: chama dentro de `transaction.atomic()`, portanto a recusa propagada desfaz o ato envolvente (`app/ponto/portas/regen_exportada.py:169`–`:180`).

Não encontrei cron ordinário de `recalcular_fechamento`; o cadastro de crons declara explicitamente que não há cron dessa operação (`app/config/crons.py:1130`–`:1137`). O comentário do callback dizendo que “o cron rega” não demonstra um agendamento real (`app/ponto/services/fechamento.py:1827`).

A propagação financeira corresponde à instrução D do pedido fornecido — deixar levantar quando seria necessário inventar regra. O item 4 do aval exige isolamento especificamente no **cartório**, não no fechamento; o item 6 exige par medido da competência 11. O dado fornecido não contém esse par: **[nao verificado]**.

## 12/11 07:59 · sessao 7bde65f716ea · - · - · origem=Ronald (tmux)

Itens 1 e 5, com achados separados de limites:

- **Fila — brecha na própria porta, anterior à c10.** `gravar_tipo_ausencia` valida o código atual e depois aplica qualquer chave de `campos` por `setattr`. Contraexemplo: `gravar_tipo_ausencia(tipo_com_codigo_abono, campos={'codigo': 'fantasma_xyz', 'ativo': True})` valida `abono` e salva `fantasma_xyz` ativo, sem faixa. A view atual não fornece `codigo` em `campos`, portanto não demonstrei exploração pelo HTTP. Evidências: `app/ponto/services/ausencia.py:1310`, `app/ponto/services/ausencia.py:1314`, `app/ponto/views_tipos_ausencia.py:67`.

- **Não encontrei segundo escritor runtime de `TipoAusencia` fora da porta.** O admin Django registra outros cinco modelos, sem `TipoAusencia`; a view de cadastro chama a porta nos dois ramos. A migration 0052 escreve diretamente código, efeito e ativo, mas seus 17 códigos estão todos no mapa. As migrations 0062 e 0063 alteram flags de tipos existentes, sem criar/ativar código novo. Evidências: `app/ponto/admin.py:11`, `app/ponto/admin.py:37`, `app/ponto/views_tipos_ausencia.py:87`, `app/ponto/views_tipos_ausencia.py:98`, `app/ponto/migrations/0052_seed_tipoausencia.py:10`, `app/ponto/migrations/0052_seed_tipoausencia.py:33`, `app/ponto/precedencia.py:52`, `app/ponto/migrations/0062_fim_dos_17_tipos.py:51`, `app/ponto/migrations/0063_atestado_aceita_horas.py:20`. Censo literal global não encontrou comando/importador/seed adicional que escreva esse modelo; caminhos dinâmicos externos à cópia: [nao verificado].

- **A recusa chega nomeada à tela.** A porta levanta `AusenciaInvalida(..., codigo='tipo_sem_faixa')`; a view lê somente `str(erro)`, não `.codigo`. `resposta_acao` envia `showToast` com tipo erro; o JavaScript escreve o texto na caixa `ta-erro` e no toast. Contraexemplo esperado: código `fantasma_xyz` exibe `Tipo sem faixa de precedencia: fantasma_xyz`. Evidências: `app/ponto/services/ausencia.py:1312`, `app/ponto/views_tipos_ausencia.py:89`, `app/ponto/views_tipos_ausencia.py:100`, `app/core/respostas.py:51`, `app/core/respostas.py:65`, `app/templates/ponto/tipos_ausencia.html:72`, `app/templates/ponto/tipos_ausencia.html:83`, `app/templates/ponto/tipos_ausencia.html:86`.

- **Leitores de `AusenciaInvalida.codigo`.** A API distingue `sobreposicao` → 409, demais códigos → 400; edição de ausência escolhe campo do formulário pelo código; um comando de rederivação registra o código. Esses consumidores não chamam `gravar_tipo_ausencia`; o novo valor `tipo_sem_faixa` tem assert específico no ensaio c10. Evidências: `app/api/views.py:1819`, `app/api/views.py:1823`, `app/ponto/views.py:2806`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:172`, `app/ponto/tests/test_lastro_c10_sem_banco.py:86`.

- **Fonte da faixa preservada.** `_FAIXA_DE_TIPO` só é acessado em `faixa_de`, que faz indexação e levanta `TipoSemFaixa` quando falta a chave. A2 e veto consultam essa função; nenhum default executável `'atestado'` substitui tipo desconhecido. `ROTULOS.get` é fallback de texto depois da classificação; `ICONE_PADRAO='atestado'` é desenho. Evidências: `app/ponto/precedencia.py:52`, `app/ponto/precedencia.py:108`, `app/ponto/precedencia.py:110`, `app/ponto/precedencia.py:419`, `app/ponto/precedencia.py:544`, `app/ponto/precedencia.py:565`, `app/ponto/catalogo/ausencias.py:119`, `app/ponto/catalogo/ausencias.py:181`.

- **Fila — fallback de efeito/cadastro sobrevive, anterior à c10.** `_do_banco` engole qualquer exceção e `vigente` troca resultado vazio/falha pelo `CATALOGO` congelado; `efeito_vigente` lê isso. Contraexemplo: `abono` configurado como `desconta`, seguido de falha de leitura do cadastro, pode voltar a responder `ABONA` do congelado. É fallback de efeito, não de faixa, e não foi introduzido pela c10. Evidências: `app/ponto/catalogo/ausencias.py:254`, `app/ponto/catalogo/ausencias.py:283`, `app/ponto/catalogo/ausencias.py:298`, `app/ponto/catalogo/ausencias.py:102`.

- **Outro fallback encontrado, sem engolir `TipoSemFaixa`.** Semana do posto usa `'trabalho'` quando não recebe faixa ou ocorre outra exceção, mas relança expressamente `TipoSemFaixa`. Portanto não demonstra sobrevivência do default para tipo sem faixa. Evidências: `app/escala/services/semana_do_posto.py:177`, `app/escala/services/semana_do_posto.py:178`, `app/escala/services/semana_do_posto.py:180`.

- **ESMERIL-SEMANTICA: mesmo nome, universos distintos.** O vigia conta códigos ativos de cadastro sem faixa; cartório conta colaboradores recusados na passada. Contraexemplo estático: um código ativo usado por três colaboradores pode resultar em vigia=1/cartório=3; código ativo sem lançamento pode resultar em vigia=1/cartório=0. Ambos consultam a mesma autoridade, portanto isso não prova segundo juiz. Evidências: `app/ponto/services/vigia_ausencia.py:155`, `app/ponto/services/vigia_ausencia.py:162`, `app/ponto/services/vigia_ausencia.py:168`, `app/ponto/services/cartorio.py:1952`, `app/ponto/management/commands/processar_cartorio.py:73`.

`TipoSemFaixa.codigo` significa identificador do tipo; `AusenciaInvalida.codigo` significa categoria da recusa. São classes distintas, e seus leitores encontrados respeitam esses sentidos. Evidências: `app/ponto/precedencia.py:101`, `app/ponto/services/ausencia.py:63`, `app/ponto/management/commands/processar_cartorio.py:74`, `app/api/views.py:1819`.

_4104 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
