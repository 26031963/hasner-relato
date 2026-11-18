# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 14:48** · publicado em **18/11/2026 14:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 14:48 · sessao 2ff881a349be · PRE-MAPA-E4D · leitura · origem=Code

A matriz abaixo é uma **leitura parcial**: onde não confirmei o juiz registrado, o ato ou um teste que exerça a porta, marquei `[nv]`. A numeração das decisões do item 2 não consta no dado fornecido; por isso, “decisão N” identifica uma dependência ainda sem número comprovado, sem escolher o destino do campo. O anexo situa A03, A05, A06, A17, A18 e A20 como exceções FATO posteriores à E4d; a abrangência exata de cada exceção por campo permanece `[nv]`. [app/docs/RELATO.md:2233](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/RELATO.md:2233)

As contagens na última coluna são **as do anexo `leitores_tabela.txt` fornecido na mensagem**, sem arquivo:linha local comprovado `[nv]`. São leitores **detectados**, não uma prova de completude; o próprio dado informa 67 `getattr` dinâmicos não atribuídos e possíveis correspondências de campo em outro modelo `[nv]`. Não somei contagens entre campos, pois um mesmo sítio pode ler mais de um campo `[nv]`.

Abreviações na tabela: `M` = `materializada_em`, `materializada_por`, `via_resolucao`; `V` = `validada_em`, `validada_por`; `R` = `respondida_em`, `resposta_colab`; `L` = `veredito_lavrado`, `julgado_em`, `janela_expira_em`; `F` = `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`. Os números de leitores são apresentados na mesma ordem dos campos dessas abreviações.

| Porta (função e fonte) | Estado de entrada | Juiz que decide | Operação | Grava hoje: ato + projeção | Depois da E4d | Retorno | Prova existente | Leitores NÃO cobertos dos campos gravados |
|---|---|---|---|---|---|---|---|---|
| A01 [`_gravar_estado`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:207) | Estado anterior e novo; terminal exige via válida e não admite pergunta viva ancorada. | `chamados.juizes::pergunta_viva` pela guarda; outros juízes `[nv]`. [models.py:228](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:228) | Transição do chamado. | Ato: evento `estado_chamado`; projeção: `status_local`, `resolvido_em`, `via_encerramento`, condicionalmente `respondido_por`; também urgência e relavra delegada. [models.py:234](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:234), [models.py:298](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:298) | Ato indicado; destino de cada campo e da relavra: decisão N `[nv]`. | `None` `[nv]`. | [`test_hx_b7_trilha.py::test_gravar_estado_emite`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_hx_b7_trilha.py:27) | `status_local` 454; `resolvido_em` 27; `via_encerramento` 19; `respondido_por` 8. |
| A02 [`fechar`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2017) | Disputa aberta, usuário; não pode haver resposta sem veredito. | `chamados.juizes::pergunta_viva` para perguntas vivas. [models.py:2040](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2040) | Fecha disputa; em exceções, encerra perguntas mudas. | Ato: evento `disputa_fechada` e, condicionalmente, `SILENCIO` por pergunta; projeção: `F` e carimbo delegado. [models.py:2082](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2082) | Atos indicados; `F` e campos delegados → decisão N `[nv]`. | `True`; `False` na repetição idêntica; entrada diferente pode levantar `ValueError`. [models.py:2045](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2045) | [`test_disputa_transicoes.py::test_01_fechar_grava_tres_campos`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_disputa_transicoes.py:60) | `F`: 190, 2, 24, 17; carimbo delegado: `M` 220, 1, 149. |
| A03 [`reabrir_admin`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2289) | Fechada, usuário presente e menos de duas reaberturas administrativas. | `[nv]` | Reabre administrativamente. | Ato próprio `[nv]`; limpa `F`, incrementa `reaberturas_admin`, altera `motivos` se fornecido. [models.py:2312](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2312) | FATO declarado; ato e destino de `F`, contador e `motivos` → decisão N `[nv]`. | `None`; condições inválidas levantam `ValueError`. [models.py:2301](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2301) | [`test_disputa_transicoes.py::test_01_reabrir_zera_fechada_em_e_incrementa`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_disputa_transicoes.py:131) | `F`: 190, 2, 24, 17; `reaberturas_admin` 6; `motivos` 26. |
| A04 [`reabrir_sistema`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2326) | Disputa fechada; aberta é no-op. | `[nv]` | Reabre pelo sistema. | Ato: ocorrência `disputa_reaberta_sistema`, se houver chamado; limpa `F`. [models.py:2336](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2336) | Ocorrência indicada; `F` → decisão N `[nv]`. | `True` se reabriu; `False` se já aberta. | `[nv]` | `F`: 190, 2, 24, 17. |
| A05 [`carimbar`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3299) | Via não identidade, pergunta dentro do fato; resposta opcional válida. | `chamados.juizes::pergunta_viva` e `texto_vazio`. [models.py:3313](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3313) | Carimba se a pergunta deixa de estar viva. | Ato próprio de decisão `[nv]`; `M`, condicionalmente `respondida_em`, `V` e `L`. [models.py:3324](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3324), [models.py:3345](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3345) | FATO declarado; ato e destino de `M`, `respondida_em`, `V`, `L` → decisão N `[nv]`. | `True` se gravou; `False` se recusou; pode levantar exceção. | `[nv]` | `M`: 220, 1, 149; `respondida_em` 68; `V`: 170, 7; `L`: 13, 5, 5. |
| A06 [`carimbar_identidade`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3364) | Via de identidade; fora do fato restringe mudança de via. | O código atribui o julgamento de identidade ao chamador; juiz registrado específico `[nv]`. [models.py:3365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3365) | Carimbo de identidade. | Ato próprio de decisão `[nv]`; `M` e, se fornecido, `recusa_motivo`. [models.py:3378](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3378) | FATO declarado; ato e destino de `M` e `recusa_motivo` → decisão N `[nv]`. | `True`/`False`; via inválida levanta `ValueError`. | [`test_e1_volta2.py::test_RED_retratada_nao_esta_no_dominio_do_lote`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e1_volta2.py:128) | `M`: 220, 1, 149; `recusa_motivo` 20. |
| A07 [`descarimbar`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3399) | Não estar fora do fato. | `[nv]` | Limpa carimbo; pode só preparar em memória. | Ato próprio `[nv]`; limpa `M`, persistindo se `salvar=True`. | Ato `[nv]`; `M` → decisão N `[nv]`. | Lista de campos ou `[]` se recusado. | [`test_e4c_f2_leitura_do_ato.py::test_MORDE_descarimbo_volta_com_resposta_so_na_projecao`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:403) | `M`: 220, 1, 149. |
| A08 [`descarimbar_em_lote`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3448) | Lote com colaboradores travados; exclui fora do fato e verifica par de resposta quando fornecido. | `[nv]` | Limpeza em lote com `**outros`. | Ato próprio `[nv]`; limpa `M` e grava os campos passados em `**outros`, incluindo `R`/`V` nos chamadores comprovados. [models.py:3485](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3485), [models.py:1702](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1702) | Ato `[nv]`; `M` e cada campo efetivo de `**outros` → decisão N `[nv]`. | Número de linhas atualizadas. | [`test_e3_10_cura_no_escritor.py`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e3_10_cura_no_escritor.py:207)::teste `[nv]`. | `M`: 220, 1, 149; `R`: 68, 135; `V`: 170, 7; demais dinâmicos `[nv]`. |
| A09 [`encerrar_por_fato_no_espelho`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3489) | Não estar fora do fato nem já na via `fato_materializado`. | `chamados.juizes::fato_do_marco_no_espelho`, chamado pelo chamador. [models.py:3491](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3491) | Troca a via. | Ato próprio `[nv]`; `via_resolucao`. | Ato `[nv]`; `via_resolucao` → decisão N `[nv]`. | `True`/`False`. | [`test_porta_chamado_e3.py::test_encerrar_por_fato_no_espelho_2x`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_porta_chamado_e3.py:70) | `via_resolucao` 149. |
| A10 [`_carimbar_validacao`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:161) | Pergunta e autor; condições de mérito vêm dos chamadores. | `[nv]` | Valida. | `DecisaoPergunta.VALIDACAO`, mais linhas legadas faltantes; `V`. [validacao.py:167](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:167) | Ato indicado; `V` → decisão N `[nv]`. | `None`. | [`test_e1_entradas.py::test_MORDE_validacao_e_linha_com_o_relogio_do_carimbo`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e1_entradas.py:94) | `V`: 170, 7. |
| A11 [`devolver_validacao_recusada`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:237) | Resposta de hora, não materializada, com motivo de recusa; validação pode já estar vazia. | `chamados.juizes::declarou_hora`, pela função auxiliar. [validacao.py:185](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:185) | Devolve validação; pode reabrir disputa. | `DEVOLUCAO` se havia validação e evento `validacao_devolvida` se houve mudança; limpa `V`, com possível A04 delegada. [validacao.py:269](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:269) | Atos indicados; `V` e campos delegados → decisão N `[nv]`. | `False`, `None` ou `True`, conforme guarda e mudança. | [`test_e1_volta2.py::test_MORDE_devolucao_guarda_a_validacao_legada`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e1_volta2.py:101) | `V`: 170, 7; se A04, `F`: 190, 2, 24, 17. |
| A12 [`responder_pergunta_disputa_core`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1439) | Resposta válida para o tipo; recebe indicador `ja_respondida`. | `[nv]` | Resposta humana e eventual descarimbo. | `RESPOSTA` e linhas legadas faltantes; `R`, possível limpeza de `V` e `M`, e `recusa_motivo`. [disputa_emissao.py:1481](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1481), [disputa_emissao.py:1532](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1532) | Ato indicado; `R`, `V`, `M` e `recusa_motivo` → decisão N `[nv]`. | `(True, None)` ou `(False, mensagem)`. | [`test_e4c_f2_leitura_do_ato.py::test_resposta_nova_depois_da_validacao_volta_ao_admin`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:239) | `R`: 68, 135; `V`: 170, 7; `M`: 220, 1, 149; `recusa_motivo` 20. |
| A13 [`reabrir_para_correcao`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1584) | Queryset de perguntas corrigíveis; ato deve aceitar foto. | `chamados.juizes::perguntas_corrigiveis` seleciona nos chamadores que o usam; não é chamado pela função em todos os casos comprovados `[nv]`. [disputa_emissao.py:1604](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1604) | Reabre em lote. | `REABERTURA` por padrão, ou ato recebido, para pergunta da qual algo é retirado; limpa `R`, `V`, `M` por A08. [disputa_emissao.py:1639](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1639) | Ato indicado; `R`, `V`, `M` → decisão N `[nv]`. | Número de linhas atualizadas. | [`test_e4c_f2_leitura_do_ato.py::test_resposta_validacao_devolucao_reabertura`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:215) | `R`: 68, 135; `V`: 170, 7; `M`: 220, 1, 149. |
| A14 [`responder_automaticamente`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1662) | Sem texto anterior; texto e instante devem formar par válido. | `[nv]` | Registra resposta automática. | `RESPOSTA` e linhas legadas faltantes; `R`. [disputa_emissao.py:1681](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1681) | Ato indicado; `R` → decisão N `[nv]`. | `True` se escreveu; `False` se já havia texto. | [`test_porta_chamado_e3.py::test_responder_automaticamente_2x`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_porta_chamado_e3.py:43) | `R`: 68, 135. |
| A15 [`carimbar_resposta_pela_trilha`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1692) | Texto presente e `respondida_em` vazio; instante fornecido pela trilha. | `[nv]` | Recupera instante da resposta. | `RESPOSTA` e evento `carimbar_resposta_pela_trilha`; `respondida_em`. [disputa_emissao.py:1705](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1705) | Ato indicado; `respondida_em` → decisão N `[nv]`. | `True`/`False`. | [`test_e4c_f2_leitura_do_ato.py::test_resposta_pela_trilha_e_validacao`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:295) | `respondida_em` 68. |
| A16 [`relavrar`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/relavra_via.py:106) | Via derivada e nova via diferente e não nula. | `chamados.juizes::via_pelo_fato` no ramo correspondente de `via_de_hoje`; outros ramos `[nv]`. [relavra_via.py:95](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/relavra_via.py:95) | Relavra via e veredito. | Evento de relavra por pergunta alterada; `via_resolucao` e `L`. [relavra_via.py:125](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/relavra_via.py:125) | Evento indicado; `via_resolucao` e `L` → decisão N `[nv]`. | Tupla `(n_vias, n_vereditos)`. | `[nv]` | `via_resolucao` 149; `L`: 13, 5, 5. |
| A17 [`_materializar_resposta_hora`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1310) | Ramo de hora; exige anuência e cálculo de plantio. | `[nv]` | Materializa ou registra pendência. | Ato de pendência `[nv]`; condicionalmente `pendencia_admin`, `recusa_motivo`; demais efeitos e atos do plantio `[nv]`. [materializacao.py:1355](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1355), [materializacao.py:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365) | FATO declarado; ato e destino dos dois campos → decisão N `[nv]`; demais efeitos `[nv]`. | `True` ou `None`, conforme ramo. | `[nv]` | `pendencia_admin` 5; `recusa_motivo` 20; delegados `[nv]`. |
| A18 [`_carimbar_recusa`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1683) | Motivo de recusa; colisão e ausência de carimbo determinam o ramo. | `[nv]` | Registra recusa; colisão delega carimbo de identidade. | Ocorrência `recusa_materializacao` quando aplicável; `recusa_motivo` e, na colisão, `M` por A06. [materializacao.py:1693](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1693) | FATO declarado; ocorrência indicada; `recusa_motivo` e `M` delegado → decisão N `[nv]`. | `None`. | `[nv]` | `recusa_motivo` 20; `M`: 220, 1, 149. |
| A19 [`recusar_pergunta`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:858) | Motivo, pergunta encontrada, respondida, não validada e dentro do fato. | `chamados.juizes::pergunta_respondida`. [validacao.py:901](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:901) | Recusa e devolve ao colaborador. | `RECUSA` via A13, ocorrência e auditoria; `recusa_motivo` e limpeza delegada de `R`, `V`, `M`. [validacao.py:916](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:916) | Atos indicados; campos citados → decisão N `[nv]`. | Dicionário `ok`, `estado`, `msg`. | [`test_e1_entradas.py::test_MORDE_recusa_e_linha_com_quem_e_o_texto_que_tirou`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e1_entradas.py:103) | `recusa_motivo` 20; `R`: 68, 135; `V`: 170, 7; `M`: 220, 1, 149. |
| A20 [`conferir_data_do_fato`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:957) | Pergunta encontrada, justificativa, data divergente e ainda não conferida. | Juiz registrado de `data_do_fato_de` `[nv]`. [validacao.py:990](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:990) | Confere data. | `LogAuditoria` `conferir_data`; `data_conferida_em`, `data_conferida_por`. [validacao.py:996](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:996) | FATO declarado; ato indicado; os dois campos → decisão N `[nv]`. | Dicionário `ok`, `estado`, `msg`. | [`test_data_do_fato_divergente.py::test_GOLDEN_conferir_carimba_e_deixa_trilha`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_data_do_fato_divergente.py:185) | `data_conferida_em` 5; `data_conferida_por` 1. |
| A21 [`remover_motivo`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2134) | Disputa aberta, motivo presente e ator; fato encerrado é julgado pelo evento chamador. | `[nv]` | Remove motivo e retrata perguntas correspondentes. | `RETRATACAO` por pergunta elegível; `motivos`, `contexto`, `M` delegado e possível fechamento A02. [models.py:2217](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2217) | Ato indicado; `motivos`, `M` e eventual `F` → decisão N `[nv]`; classificação de `contexto` `[nv]`. | Dicionário com `ok`, `estado`, `motivos`. [models.py:2229](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2229) | [`test_e1_entradas.py::test_MORDE_a_retratada_morre_e_tem_linha`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e1_entradas.py:142) | `motivos` 26; `M`: 220, 1, 149; possível `F`: 190, 2, 24, 17. |
| A22 [`_nascer`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3012) | Disputa, motivo, data e identidade válidos; aceita `**campos`. | `chamados.juizes::resolvedora_valida` em ramo de destino; demais julgamentos `[nv]`. [mod

[truncado em 20000 de 24659 caracteres]

## 18/11 14:47 · sessao baf79de3bdbc · PRE-MAPA-E4D · leitura · origem=Code

As etiquetas abaixo distinguem **LEI**, quando falta corte de Ronald, de **DESENHO**, quando o desenho ou corte escrito enquadra a pergunta. A escolha de implementação permanece formulada como pergunta em todos os itens. O censo de escritores admite escritas dinâmicas não resolvidas; sua completude é **[nao verificado]**. O resumo de leitores é um tamanho detectado, sujeito aos limites informados no anexo; cobertura real de cada leitor é **[nao verificado]**.

1. **DESENHO — `status_local`:** na A01, parar significa deixar de gravar, derivar de qual ato ou retirar o campo por migration? Como preservar seus leitores e o nascimento? Fonte: [models.py:234](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:234), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

2. **DESENHO — `resolvido_em`:** na A01, deixa de ser gravado, deriva de qual ato ou sai do modelo por migration? Fonte: [models.py:235](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:235), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

3. **DESENHO — `via_encerramento`:** na A01, deixa de ser gravada, deriva de qual ato ou sai do modelo por migration? Fonte: [models.py:234](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:234), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

4. **DESENHO — `respondido_por`:** na A01, deixa de ser gravado, deriva de qual ato ou sai do modelo por migration? Fonte: [models.py:237](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:237), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

5. **DESENHO — `fechada_em`:** nas A02–A04, deixa de ser gravada, deriva de qual ato ou sai do modelo por migration, considerando a exceção A03? Fonte: [models.py:2095](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2095), [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091).

6. **DESENHO — `fechada_por`:** nas A02–A04, deixa de ser gravado, deriva de qual ato ou sai do modelo por migration, considerando A03? Fonte: [models.py:2096](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2096), [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091).

7. **DESENHO — `resolucao`:** nas A02–A04 e A24, deixa de ser gravada, deriva de qual ato ou sai do modelo por migration, considerando A03? Fonte: [models.py:2097](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2097), [models.py:2428](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2428), [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091).

8. **DESENHO — `motivo_fechamento`:** nas A02–A04, deixa de ser gravado, deriva de qual ato ou sai do modelo por migration, considerando A03? Fonte: [models.py:2098](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2098), [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091).

9. **DESENHO — `reaberturas_admin`:** na A03, qual é o tratamento do contador até o fato próprio e depois dele: gravação, derivação de qual ato ou retirada por migration? Fonte: [models.py:2316](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2316), [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091).

10. **DESENHO — `reaberturas_colab`:** na A23, deixa de ser gravado, deriva de qual ato ou sai do modelo por migration? Fonte: [models.py:2402](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2402), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

11. **DESENHO — `motivos`:** na A03 e nos demais sítios, é projeção a parar, dado derivado de qual ato ou campo que permanece no modelo? Como incide a exceção A03? Fonte: [models.py:2319](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2319), [disputa_emissao.py:550](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:550), [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091).

12. **DESENHO — `materializada_em`:** nas portas que carimbam ou descarimbam, como passar da escrita dupla ao ato de conclusão e tratar as exceções A05/A06, inclusive os leitores ainda detectados? Fonte: [models.py:3324](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3324), [BACKLOG.md:2015](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2015), [BACKLOG.md:2092](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2092).

13. **DESENHO — `materializada_por`:** deixa de ser gravado, deriva de qual ato e autor, ou sai por migration, respeitadas A05/A06? Fonte: [models.py:3324](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3324), [BACKLOG.md:2015](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2015), [BACKLOG.md:2093](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2093).

14. **DESENHO — `via_resolucao`:** deixa de ser gravada, deriva de qual ato ou sai por migration, distinguindo vias de decisão e vias derivadas e as exceções A05/A06? Fonte: [models.py:3378](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3378), [relavrar_via_i6.py:58](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/management/commands/relavrar_via_i6.py:58), [BACKLOG.md:2092](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2092).

15. **DESENHO — `respondida_em`:** deixa de ser gravado, deriva de qual ato de resposta e de seus atos posteriores, ou sai por migration? Fonte: [disputa_emissao.py:1481](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1481), [models.py:4262](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:4262), [PLANO-PADROES.md:44](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/PLANO-PADROES.md:44).

16. **DESENHO — `resposta_colab`:** deixa de ser gravada, deriva de qual ato e texto, ou sai por migration, preservando os leitores e a exceção A05? Fonte: [disputa_emissao.py:1481](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1481), [CORTES.md:521](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:521), [BACKLOG.md:2092](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2092).

17. **DESENHO — `validada_em`:** deixa de ser gravado, deriva de qual ato de validação e de seus atos posteriores, ou sai por migration? Fonte: [validacao.py:168](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:168), [models.py:4267](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:4267), [CORTES.md:543](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:543).

18. **DESENHO — `validada_por`:** deixa de ser gravado, deriva de qual ato e autor, ou sai por migration? Fonte: [validacao.py:168](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:168), [models.py:4267](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:4267), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

19. **DESENHO — `recusa_motivo`:** quais escritas deixam de gravar, qual ato fornece o motivo e como ficam as exceções A06/A17/A18 até seus fatos próprios? Fonte: [validacao.py:916](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:916), [BACKLOG.md:2093](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2093), [BACKLOG.md:2094](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2094).

20. **DESENHO — `pendencia_admin`:** como permanece a fonte lida de A17 até o fato próprio e qual derivação ou migration fica para depois? Fonte: [materializacao.py:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365), [BACKLOG.md:2094](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2094).

21. **DESENHO — `data_conferida_em`:** como permanece a fonte de A20 até o fato próprio e de qual ato e par de dias será derivado o instante depois? Fonte: [validacao.py:996](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:996), [BACKLOG.md:2096](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2096).

22. **DESENHO — `data_conferida_por`:** como permanece a fonte de A20 até o fato próprio e de qual ato será derivado o autor depois? Fonte: [validacao.py:997](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:997), [BACKLOG.md:2096](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2096).

23. **DESENHO — `veredito_lavrado`:** o cache deixa de ser gravado, continua derivado de qual fonte, ou sai por migration; como seus leitores passam pelo Contract? Fonte: [veredito_lavrado.py:270](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/veredito_lavrado.py:270), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

24. **DESENHO — `julgado_em`:** o instante do cache deixa de ser gravado, deriva de qual fonte ou sai por migration? Fonte: [veredito_lavrado.py:281](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/veredito_lavrado.py:281), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

25. **DESENHO — `janela_expira_em`:** o prazo do cache deixa de ser gravado, deriva de qual fonte ou sai por migration? Fonte: [veredito_lavrado.py:228](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/veredito_lavrado.py:228), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

26. **DESENHO — competência exportada:** que tratamento recebe cada gravado alcançado pelo Contract e pelo backfill do ato de conclusão, distinguindo projeção, gravado de cálculo e TXT entregue, à luz da L-092 como alterada pela L-113 e do `!` declarado para backfill que alcance exportada? O estoque efetivamente atingido é **[nao verificado]**. Fonte: [LEIS.md:125](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/LEIS.md:125), [LEIS.md:155](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/LEIS.md:155), [BACKLOG.md:2015](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2015).

27. **DESENHO — CLASSE-D:** quais entregas da cura pela porta, da lista do admin e da leitura de empresas pelo cadastro precisam pousar antes da E4d, e como comprovar o cruzamento com B2 + delta e F2? O pouso atual é **[nao verificado]**. Fonte: [BACKLOG.md:2025](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2025), [CORTES.md:532](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:532), [CORTES.md:544](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:544).

28. **LEI — CLASSE-D:** qual dono responde por `FATO_EM_AUSENCIA`: CADASTRO, BATIDA ou ESTRUTURA? Falta o corte de Ronald sobre o dono dessa classe; o encaminhamento dia a dia à lista do admin já consta do corte. Fonte: [HANDOFF-SESSAO.md:25](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/HANDOFF-SESSAO.md:25), [CORTES.md:544](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:544).

29. **DESENHO — RAIZ-DA-DISPUTA F5:** qual fronteira e qual ordem de pouso unem a F5 da raiz à E4d, depois das fases anteriores, para que os escritores e leitores não sejam migrados duas vezes? Fonte: [BACKLOG.md:1904](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:1904), [BACKLOG.md:2109](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2109).

30. **DESENHO — D1:** como `devolver_paridade_bug_par_103` deixa de gravar os três campos de materialização e os dois de validação, usando a trilha `DEVOLUCAO` e preservando a operação? Fonte: [validacao.py:346](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:346), [CORTES.md:543](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:543).

31. **DESENHO — D2:** como `ressuscitar_perguntas` deixa de gravar os sete campos de resposta, validação e materialização, com a `REABERTURA` marcada e a fila conservada? Fonte: [models.py:1694](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1694), [models.py:1702](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1702), [CORTES.md:544](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:544).

32. **LEI — D2, efeito para o colaborador:** ressuscitar devolve a pergunta ao colaborador? Falta o corte **RESSUSCITAR-DEVOLVE-AO-COLABORADOR**; sua relação com a E4d e com a fila deve ser explicitada sem presumir resposta. Fonte: [CORTES.md:543](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:543), [CORTES.md:560](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:560).

33. **DESENHO — D3, sem ato:** como `devolver_instante_sem_texto_l130` deixará de limpar resposta e carimbo sem deixar mudança de estado sem ato, e como será tratado o estoque entregue sujeito a `!` próprio? Qual ato existente ou desenho autorizado representa essa operação é **[nao verificado]**. Fonte: [validacao.py:414](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:414), [validacao.py:440](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:440), [CORTES.md:560](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:560).

34. **DESENHO — D4:** como `desfazer_carimbo_sem_lastro` deixará de gravar os três campos por A07, e qual ato representa o desfazimento? A equivalência do ato é **[nao verificado]**. Fonte: [relavra_via.py:246](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/relavra_via.py:246), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

35. **DESENHO — L1:** que passa a fazer `lavrar`, inclusive quando chamado pelo comando de perguntas stale, se os três campos da lavra deixarem de ser gravados? Fonte: [veredito_lavrado.py:270](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/veredito_lavrado.py:270), [lavrar_perguntas_stale.py:89](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/management/commands/lavrar_perguntas_stale.py:89), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

36. **DESENHO — L2:** que passa a fazer o `bulk_update` de `gravar_lavras_em_lote` e seu comando de backfill? Fonte: [veredito_lavrado.py:250](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/veredito_lavrado.py:250), [backfill_veredito_lavrado.py:138](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/management/commands/backfill_veredito_lavrado.py:138), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

37. **DESENHO — L3:** como `reapontar_resolvedora` preserva a leitura após deixar de salvar os três campos da lavra? Fonte: [fio.py:975](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/fio.py:975), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

38. **DESENHO — L4:** como `rechavear_data_evento` preserva a leitura após deixar de salvar os três campos da lavra? Fonte: [fio.py:1091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/fio.py:1091), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

39. **DESENHO — L5:** como tratar a lavra no ramo de reuso de `_nascer`, inclusive sua fronteira com A22? A separação de L5 da porta é **[nao verificado]**. Fonte: [models.py:3195](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3195), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

40. **DESENHO — `motivos`, aparar:** como `aparar_motivo_respondido` deixa de gravar a lista e de qual ato ou fonte passa a obter seu efeito? Fonte: [models.py:2129](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2129), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

41. **DESENHO — `motivos`, abrir ou agregar:** como `_abrir_ou_agregar_disputa` passa a registrar e ler a inclusão de motivos? Fonte: [disputa_emissao.py:550](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:550), [BACKLOG.md:2109](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2109).

42. **DESENHO — `motivos`, órfãos:** como `criar_disputa_a_partir_de_chamados_orfaos` trata suas escritas de criação e atualização, e qual ato ou fonte as substitui? Fonte: [disputa_emissao.py:1097](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1097), [disputa_emissao.py:1269](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1269), [BACKLOG.md:2109](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2109).

43. **DESENHO — `motivos`, âncora:** como `criar_disputa_ancora_de_chamado` trata a gravação da lista e qual ato ou fonte a substitui? Fonte: [disputa_emissao.py:1372](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1372), [BACKLOG.md:2109](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2109).

44. **DESENHO — nascimento por `abrir`:** como tratar o default de `status_local` no nascimento e qual ato ou fonte sustentará a primeira leitura? Fonte: [models.py:1000](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1000), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

45. **DESENHO — nascimento por `criar`:** como tratar o default de `status_local` nesse sítio e qual ato ou fonte sustentará a primeira leitura? Fonte: [models.py:1154](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/mo

[truncado em 20000 de 22193 caracteres]

## 18/11 14:47 · sessao 5475c33f062c · ESPURIAS-PRE-MAPA · censo · origem=Code

O lote contém **29 linhas**, todas reproduzidas abaixo. O fechamento usa `[nv]` como categoria de incerteza: não consegui provar todas as classes, chamadores e fatias, portanto a lista final é **um mínimo confirmado**, não o tamanho real completo da obra. A unidade continua sendo a função do anexo, mesmo quando ela contém vários sítios. [app/core/medida_turno_do_par.py:10]

Convenções: **JP** = JUÍZO PARALELO; **EP** = ENTRADA DO PAR; **EX** = EXIBIÇÃO. Em (b), “cron” significa declarado em `config/crons.py`; a execução efetiva em produção é `[não verificado]`. Em (c), o efeito indicado é o alcance comprovado do caminho, sujeito às condições do código. Referências `ponto/...`, `escala/...` e `folha/...` são relativas a `app/`. [app/docs/BACKLOG.md:1846; app/config/crons.py:428]

| chave | linha(s) | (a) | (b) | (c) | (d) | (e) arquivo:linha conferido |
|---|---:|---|---|---|---|---|
| escala/regua_defesa.py::entradas_fora_do_inicio | 521 | JP | VIVO cron; chamador `escala/management/commands/termometro_regua.py:68`, cron `config/crons.py:428` | GRAVA lista de régua | DIA-DA-BATIDA-JUIZ-UNICO | [escala/regua_defesa.py:521](app/escala/regua_defesa.py#L521); [termometro_regua.py:93](app/escala/management/commands/termometro_regua.py#L93) |
| escala/utils.py::montar_grade_prevista | 807 | EX | VIVO cron, via `escala/utils.py:1027`; [nv] quanto à condição de ativação do caminho legacy | EXIBE órfãs | DIA-DA-BATIDA-JUIZ-UNICO | [escala/utils.py:807](app/escala/utils.py#L807); [reconciliar_grade.py:118](app/ponto/management/commands/reconciliar_grade.py#L118) |
| escala/utils.py::montar_grade_prevista_periodo_por_turno | 1491, 1511, 1521, 1534, 1545, 1831 | JP | VIVO cron, via `escala/utils.py:2098` e `reconciliar_grade.py:118`, condicionado à flag | [nv] | DIA-DA-BATIDA-JUIZ-UNICO | [escala/utils.py:1491](app/escala/utils.py#L1491); [escala/utils.py:1545](app/escala/utils.py#L1545); [escala/utils.py:1831](app/escala/utils.py#L1831) |
| folha/export.py::dias_ferias_com_batida | 500 | JP | VIVO request, via `folha/export.py:791`, `folha/views.py:214` | PAGA: classificação da exportação | DIA-DA-BATIDA-JUIZ-UNICO | [folha/export.py:500](app/folha/export.py#L500); [folha/export.py:791](app/folha/export.py#L791) |
| folha/porta_export.py::dias_fora_do_dia_pago | 104 | JP | [nv]; chamador `folha/porta_export.py:597` | EXIBE divergência para a porta | [nv] | [folha/porta_export.py:104](app/folha/porta_export.py#L104) |
| folha/porta_export.py::medir | 538 | JP | [nv] | EXIBE/mede contador da porta | [nv] | [folha/porta_export.py:538](app/folha/porta_export.py#L538) |
| ponto/calculador/nucleo.py::minutos_do_oraculo | 117, 141, 235 | JP | [nv]; chamadores `ponto/calculador/periodo.py:356` e `ponto/calculador/alimentacao.py:449` | [nv] | [nv]; o BACKLOG nomeia ESMERIL-ORÁCULO-NOTURNA, mas não provei que estes sítios sejam ele | [ponto/calculador/nucleo.py:141](app/ponto/calculador/nucleo.py#L141); [ponto/calculador/nucleo.py:235](app/ponto/calculador/nucleo.py#L235); [app/docs/BACKLOG.md:2051](app/docs/BACKLOG.md#L2051) |
| ponto/janelas.py::competencia_fechada | 84 | JP | VIVO request, via `ponto/registro_batida.py:159` | GRAVA: guarda de ajuste de batida | DIA-DA-BATIDA-JUIZ-UNICO | [ponto/janelas.py:84](app/ponto/janelas.py#L84); [ponto/registro_batida.py:159](app/ponto/registro_batida.py#L159) |
| ponto/management/commands/desvio_o68b.py::Command._autopsia | 112, 121, 136 | JP | INSTRUMENTO; chamador `desvio_o68b.py:181` | EXIBE autópsia | [nv] | [desvio_o68b.py:112](app/ponto/management/commands/desvio_o68b.py#L112) |
| ponto/management/commands/detectar_intervalo_ausente.py::furos_intervalo | 82 | EX | VIVO cron, chamador `detectar_intervalo_ausente.py:120`, com `--alarme` em `config/crons.py:491` | EXIBE/alarma; esse cron força dry | [nv] | [detectar_intervalo_ausente.py:82](app/ponto/management/commands/detectar_intervalo_ausente.py#L82); [detectar_intervalo_ausente.py:108](app/ponto/management/commands/detectar_intervalo_ausente.py#L108) |
| ponto/management/commands/detectar_par_relampago.py::varrer_pares_embutidos | 303 | EP | VIVO cron, chamador `detectar_par_relampago.py:392`, cron `config/crons.py:605` | GRAVA: retrata batidas candidatas | DIA-DA-BATIDA-JUIZ-UNICO | [detectar_par_relampago.py:303](app/ponto/management/commands/detectar_par_relampago.py#L303); [detectar_par_relampago.py:399](app/ponto/management/commands/detectar_par_relampago.py#L399) |
| ponto/management/commands/emitir_furo_retroativo.py::Command.handle | 75, 100 | JP | AVULSO, comando sob ordem `config/crons.py:1414` | GRAVA com `--apply` | DIA-DA-BATIDA-JUIZ-UNICO | [emitir_furo_retroativo.py:100](app/ponto/management/commands/emitir_furo_retroativo.py#L100); [emitir_furo_retroativo.py:111](app/ponto/management/commands/emitir_furo_retroativo.py#L111) |
| ponto/management/commands/flip_automatico.py::Command.handle | 64 | JP | VIVO cron `config/crons.py:707` | GRAVA batida e nota; o dia civil também seleciona chamados para a nota | DIA-DA-BATIDA-JUIZ-UNICO | [flip_automatico.py:64](app/ponto/management/commands/flip_automatico.py#L64); [flip_automatico.py:76](app/ponto/management/commands/flip_automatico.py#L76) |
| ponto/management/commands/marcar_abandono_provisorio.py::Command.handle | 56 | JP | AVULSO, comando sob ordem `config/crons.py:1422` | GRAVA ausências com `--apply` | DIA-DA-BATIDA-JUIZ-UNICO | [marcar_abandono_provisorio.py:56](app/ponto/management/commands/marcar_abandono_provisorio.py#L56); [marcar_abandono_provisorio.py:76](app/ponto/management/commands/marcar_abandono_provisorio.py#L76) |
| ponto/management/commands/plano_b_no_dinheiro.py::Command.handle._espiao._cm_espiao | 88 | EX | INSTRUMENTO; instalado no método em `plano_b_no_dinheiro.py:101` | EXIBE; ensaio revertido | [nv] | [plano_b_no_dinheiro.py:88](app/ponto/management/commands/plano_b_no_dinheiro.py#L88); [plano_b_no_dinheiro.py:110](app/ponto/management/commands/plano_b_no_dinheiro.py#L110) |
| ponto/management/commands/reconciliar_fantasmas.py::_parse_pendentes | 66 | [nv] | VIVO cron, chamador `reconciliar_fantasmas.py:184`, com `--alarme` em `config/crons.py:392` | EXIBE/alarma no cron; `--apply` manual existe | [nv] | [reconciliar_fantasmas.py:66](app/ponto/management/commands/reconciliar_fantasmas.py#L66); [reconciliar_fantasmas.py:27](app/ponto/management/commands/reconciliar_fantasmas.py#L27) |
| ponto/management/commands/recusar_ponta_pequena.py::Command.handle | 105 | [nv]: data de cálculo do retrato, não provei classe de dia de batida | VIVO cron encadeado `config/crons.py:1585` | GRAVA decisão de HE com `--apply`, condicionada à foto do dia | [nv] | [recusar_ponta_pequena.py:105](app/ponto/management/commands/recusar_ponta_pequena.py#L105); [recusar_ponta_pequena.py:170](app/ponto/management/commands/recusar_ponta_pequena.py#L170) |
| ponto/management/commands/supra_juiz.py::Command.handle | 135, 222 | JP | VIVO cron `config/crons.py:599` | EXIBE/alarma para esses sítios; a escrita lê a lavra da célula | DIA-DA-BATIDA-JUIZ-UNICO | [supra_juiz.py:222](app/ponto/management/commands/supra_juiz.py#L222); [supra_juiz.py:241](app/ponto/management/commands/supra_juiz.py#L241) |
| ponto/management/commands/vigia_plantio_overnight.py::batidas_fora_do_turno_da_pergunta | 86 | EP | VIVO cron, chamador `vigia_plantio_overnight.py:110`, cron `config/crons.py:646` | EXIBE/alarma | DIA-DA-BATIDA-JUIZ-UNICO | [vigia_plantio_overnight.py:86](app/ponto/management/commands/vigia_plantio_overnight.py#L86) |
| ponto/motor_calculo_v2.py::Motor12x36.calcular_periodo | 2796, 2809 | JP | VIVO request, via `motor_calculo_v2.py:2458`, fechamento `ponto/services/fechamento.py:286` e request `ponto/views.py:1255` | PAGA: feriado e HE | f2 DIA: feriado cruzando meia-noite | [motor_calculo_v2.py:2796](app/ponto/motor_calculo_v2.py#L2796); [app/docs/BACKLOG.md:2051](app/docs/BACKLOG.md#L2051) |
| ponto/motor_calculo_v2.py::MotorBase._intervalo_descontado_no_dia | 1313 | JP | [nv]; chamador `motor_calculo_v2.py:1803` | PAGA: limite de intervalo | [nv] | [motor_calculo_v2.py:1313](app/ponto/motor_calculo_v2.py#L1313); [motor_calculo_v2.py:1803](app/ponto/motor_calculo_v2.py#L1803) |
| ponto/motor_calculo_v2.py::MotorBase._janela_do_dia | 1528 | JP | [nv]; chamadores em `motor_calculo_v2.py:1691`, `:1733`, `:1760` | PAGA: janela do período | [nv] | [motor_calculo_v2.py:1528](app/ponto/motor_calculo_v2.py#L1528) |
| ponto/motor_calculo_v2.py::MotorBase._julgar_pontualidade | 2053, 2093, 2118, 2198, 2200, 2210 | JP | VIVO request, via `motor_calculo_v2.py:3306` e fechamento `ponto/services/fechamento.py:286` | PAGA: pontualidade | [nv] | [motor_calculo_v2.py:2053](app/ponto/motor_calculo_v2.py#L2053); [motor_calculo_v2.py:2198](app/ponto/motor_calculo_v2.py#L2198) |
| ponto/motor_calculo_v2.py::MotorBase._previsto_do_dia | 1227 | JP | [nv]; chamadores `motor_calculo_v2.py:1170`, `:1273`, `:1453`, `:1825` | PAGA: previsto do dia | [nv] | [motor_calculo_v2.py:1227](app/ponto/motor_calculo_v2.py#L1227) |
| ponto/motor_calculo_v2.py::MotorBase.calcular_mes | 2403, 2579, 2639, 2643, 2644 | JP | VIVO request, chamador `ponto/services/fechamento.py:286`, request `ponto/views.py:1255` | PAGA: jornada e reflexo DSR | f1 PAR **e** f2 DIA: a função também contém a herança por gap nomeada; não se atribui cada sítio automaticamente às duas | [motor_calculo_v2.py:2403](app/ponto/motor_calculo_v2.py#L2403); [motor_calculo_v2.py:2643](app/ponto/motor_calculo_v2.py#L2643); [app/docs/BACKLOG.md:2051](app/docs/BACKLOG.md#L2051) |
| ponto/motor_calculo_v2.py::MotorComercial.calcular_mes | 3326, 3447, 3465, 3490, 3494, 3495 | JP | VIVO request, chamador `ponto/services/fechamento.py:286`, request `ponto/views.py:1255` | PAGA: jornada, banco e DSR | [nv] | [motor_calculo_v2.py:3326](app/ponto/motor_calculo_v2.py#L3326); [motor_calculo_v2.py:3490](app/ponto/motor_calculo_v2.py#L3490) |
| ponto/motor_calculo_v2.py::MotorComercial.calcular_periodo | 3264, 3268 | JP | VIVO request, via `motor_calculo_v2.py:2458` e fechamento `ponto/services/fechamento.py:286` | PAGA: feriado e HE | f2 DIA: feriado cruzando meia-noite | [motor_calculo_v2.py:3264](app/ponto/motor_calculo_v2.py#L3264); [app/docs/BACKLOG.md:2051](app/docs/BACKLOG.md#L2051) |
| ponto/motor_calculo_v2.py::MotorTurnoPartido.calcular_mes | 2942, 2955 | JP | VIVO request, chamador `ponto/services/fechamento.py:286`, request `ponto/views.py:1255` | PAGA: jornada | f2 DIA: partido, sítio nomeado | [motor_calculo_v2.py:2942](app/ponto/motor_calculo_v2.py#L2942); [app/docs/BACKLOG.md:2051](app/docs/BACKLOG.md#L2051) |
| ponto/motor_calculo_v2.py::_periodos_pelo_marco | 578, 686, 687 | JP | [nv]; chamador `motor_calculo_v2.py:531` | PAGA: seleção de períodos | [nv] | [motor_calculo_v2.py:578](app/ponto/motor_calculo_v2.py#L578); [motor_calculo_v2.py:686](app/ponto/motor_calculo_v2.py#L686) |

A atribuição de f2 aos sítios de feriado identifica o tema registrado, **não prova que toda execução dessas linhas cruze meia-noite**. A classificação de efeito também não afirma que houve pagamento ou escrita em produção. [app/docs/BACKLOG.md:2051; app/ponto/motor_calculo_v2.py:2796; app/ponto/motor_calculo_v2.py:3264]

**Fecho (a) × (b), deste lote:**

| (a) | VIVO | AVULSO | INSTRUMENTO | SEM CHAMADOR | [nv] | Total |
|---|---:|---:|---:|---:|---:|---:|
| EXIBIÇÃO | 2 | 0 | 1 | 0 | 0 | 3 |
| ENTRADA DO PAR | 2 | 0 | 0 | 0 | 0 | 2 |
| JUÍZO PARALELO | 12 | 2 | 1 | 0 | 7 | 22 |
| [nv] | 2 | 0 | 0 | 0 | 0 | 2 |
| **Total** | **18** | **2** | **2** | **0** | **7** | **29** |

A soma é uma contagem das classificações da tabela acima; os sete `[nv]` em (b) não são prova de ausência de chamador. [app/core/medida_turno_do_par.py:33]

**JUÍZO PARALELO + VIVO + GRAVA/PAGA comprovados no lote:**

- `escala/regua_defesa.py::entradas_fora_do_inicio` — grava lista de régua. [termometro_regua.py:68](app/escala/management/commands/termometro_regua.py#L68); [termometro_regua.py:93](app/escala/management/commands/termometro_regua.py#L93)
- `folha/export.py::dias_ferias_com_batida` — participa da classificação da exportação. [folha/export.py:500](app/folha/export.py#L500); [folha/export.py:791](app/folha/export.py#L791)
- `ponto/janelas.py::competencia_fechada` — guarda usada no registro de batida. [ponto/janelas.py:84](app/ponto/janelas.py#L84); [ponto/registro_batida.py:159](app/ponto/registro_batida.py#L159)
- `ponto/management/commands/flip_automatico.py::Command.handle` — grava batida e seleciona chamados por dia para nota. [flip_automatico.py:64](app/ponto/management/commands/flip_automatico.py#L64)
- Motor: `Motor12x36.calcular_periodo`, `MotorBase._julgar_pontualidade`, `MotorBase.calcular_mes`, `MotorComercial.calcular_mes`, `MotorComercial.calcular_periodo` e `MotorTurnoPartido.calcular_mes`. [ponto/services/fechamento.py:286](app/ponto/services/fechamento.py#L286); [ponto/views.py:1255](app/ponto/views.py#L1255); [motor_calculo_v2.py:2458](app/ponto/motor_calculo_v2.py#L2458); [motor_calculo_v2.py:3306](app/ponto/motor_calculo_v2.py#L3306)

A função de grade por turno permanece fora dessa lista porque seu efeito de gravação ou pagamento ficou `[nv]`; as demais linhas com caminho `[nv]` também impedem tratar os **10 itens comprovados** como contagem final. [escala/utils.py:1545](app/escala/utils.py#L1545)

**Não visto pelo método:** outros pareadores por estado; outras regras de meia-noite e heranças; chamadas dinâmicas e parte da composição interprocedural; HTML/JS, código gerado e pastas excluídas. Não há número para esses pontos cegos. [app/core/medida_turno_do_par.py:33](app/core/medida_turno_do_par.py#L33); [app/core/medida_turno_do_par.py:44](app/core/medida_turno_do_par.py#L44); [app/core/medida_turno_do_par.py:46](app/core/medida_turno_do_par.py#L46)

## 18/11 14:46 · sessao 7be273638f9b · ESPURIAS-PRE-MAPA · censo · origem=Code

O lote contém **32 funções**. A conferência abaixo não fecha a classificação de todos os caminhos: **12 funções classificadas como JUÍZO PARALELO ficaram com o caminho de execução [nv]**. A lista curta, portanto, é um mínimo comprovado nesta leitura, não o tamanho final da obra. [anexo lote_00: não possui arquivo:linha; app/core/medida_turno_do_par.py:46]

Usei **J** = JUÍZO PARALELO, **E** = ENTRADA DO PAR, **X** = EXIBIÇÃO, **V** = VIVO, **A** = AVULSO e **I** = INSTRUMENTO. `[nv]` significa não verificado. Em (c), o efeito é o alcançado pelo caminho citado; uma função auxiliar pode apenas retornar um valor usado pela escrita. Em (d), `D` significa DIA-DA-BATIDA-JUIZ-UNICO. A atribuição de fatia que não consegui provar ficou [nv]. [app/docs/BACKLOG.md:1846] [app/docs/BACKLOG.md:2051]

| Chave | Linha(s) | (a) | (b) Caminho e chamador | (c) Efeito | (d) Fatia | (e) Conferido |
|---|---:|---|---|---|---|---|
| `escala/utils.py::_alinhar_por_papel` | 529 | J | [nv]; chamada interna em `escala/utils.py:623` | [nv] | [nv] | [app/escala/utils.py:511] |
| `escala/utils.py::_papeis_por_alternancia` | 496 | J | [nv]; chamada interna em `escala/utils.py:608` | [nv] | [nv] | [app/escala/utils.py:477] |
| `ponto/calculador/nucleo.py::minutos_do_oraculo` | 246 | J, no ramo que pareia por sequência | [nv]; também chamado por `ponto/calculador/alimentacao.py:449` | [nv] | [nv] | [app/ponto/calculador/nucleo.py:65] |
| `ponto/management/commands/e6_piso_autopsia.py::Command.handle` | 94 | J | I: autópsia | EXIBE/mede | [nv] | [app/ponto/management/commands/e6_piso_autopsia.py:80] |
| `ponto/management/commands/vigiar_motor_autoridade.py::Command.handle` | 85 | [nv] | I; cron em `config/crons.py:596` | EXIBE/mede | [nv] | [app/ponto/management/commands/vigiar_motor_autoridade.py:80] |
| `ponto/turnos.py::_pares_marcados` | 812 | J | [nv]; chamada interna em `ponto/turnos.py:545` | [nv] | [nv] | [app/ponto/turnos.py:798] |
| `ponto/motor_calculo_v2.py::turnos_via_autoridade` | 503 | E | [nv]; chamada interna em `ponto/motor_calculo_v2.py:530` | PAGA: integra cálculo do motor; caminho até fechamento em `ponto/services/fechamento.py:286` | f1 PAR | [app/ponto/motor_calculo_v2.py:503] |
| `ponto/motor_calculo_v2.py::MotorBase._turnos_do_juiz` | 882 | E | [nv]; chamada interna em `ponto/motor_calculo_v2.py:2346` | PAGA: integra cálculo do motor; fechamento em `ponto/services/fechamento.py:286` | f1 PAR | [app/ponto/motor_calculo_v2.py:867] |
| `ponto/motor_calculo_v2.py::MotorBase.calcular_mes` | 2408 | J | [nv]; fechamento chama o motor em `ponto/services/fechamento.py:286` | PAGA: cálculo do motor | f1 PAR e f2 DIA, conforme complemento do caso | [app/ponto/motor_calculo_v2.py:2403] [app/docs/BACKLOG.md:2051] |
| `api/views.py::api_bater_ponto` | 719 | J | V request: `api/urls.py:67` | GRAVA chamado com `data_turno` no contexto | D | [app/api/views.py:716] |
| `api/views_core.py::api_bater_ponto` | 713 | J | [nv]; rota desta cópia aponta para `views`, em `api/urls.py:2` e `:67` | GRAVA chamado com `data_turno` no contexto, se executada | D | [app/api/views_core.py:710] |
| `chamados/juizes.py::_dia_da_batida` | 54 | J | [nv]; chamada por `chamados/juizes.py:131` | [nv] | D | [app/chamados/juizes.py:51] |
| `chamados/juizes.py::_dia_do_contexto_fraco` | 170 | J | [nv]; chamada por `chamados/juizes.py:139` e `:230` | [nv] | D | [app/chamados/juizes.py:162] |
| `chamados/juizes.py::_plantio_na_hora_do_slot` | 275 | J | [nv]; chamada por `chamados/juizes.py:764` | [nv] | D | [app/chamados/juizes.py:254] |
| `chamados/juizes.py::dias_do_fato` | 204 | J | [nv]; há chamadores em `chamados/services/fila_por_causa.py:162` e `escala/services/propositor.py:633` | [nv] | D | [app/chamados/juizes.py:201] |
| `chamados/management/commands/auditar_disputas.py::Command.handle` | 111 | J | A: relatório sob demanda, `config/crons.py:1465` | EXIBE relatório | D | [app/chamados/management/commands/auditar_disputas.py:107] |
| `chamados/management/commands/carimbar_por_batida_coincidente.py::Command.handle` | 81 | J | A: comando, `config/crons.py:1321` | GRAVA contexto e vínculo de célula com `--apply` | D | [app/chamados/management/commands/carimbar_por_batida_coincidente.py:80] |
| `chamados/management/commands/migrar_data_evento_dia_do_turno.py::_fonte_viva` | 102 | E | A: migração; chamada por `migrar_data_evento_dia_do_turno.py:154` | GRAVA pergunta na migração com `--aplicar` [nv quanto a este ramo específico] | [nv] | [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:94] |
| `chamados/management/commands/migrar_data_evento_dia_do_turno.py::classificar` | 158 | E | A: migração; chamada por `migrar_data_evento_dia_do_turno.py:212` | GRAVA pergunta na migração com `--aplicar` [nv quanto a este ramo específico] | [nv] | [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:150] |
| `chamados/services/desvio_escala.py::medir_desvio` | 65 | J | V cron: `config/crons.py:707` → `ponto/management/commands/flip_automatico.py:66` → `desvio_escala.py:103` | GRAVA causa do flip de batida e nota em chamado | D | [app/chamados/services/desvio_escala.py:58] [app/ponto/management/commands/flip_automatico.py:64] |
| `chamados/services/disputa_emissao.py::_orfao_resolvido_cross_meianoite` | 729, 730 | J | V cron: `config/crons.py:362` → `ponto/management/commands/processar_alertas_turno.py:382` → `disputa_emissao.py:797` | GRAVA/PERGUNTA: a guarda interfere no universo da fábrica de disputa | D | [app/chamados/services/disputa_emissao.py:721] |
| `chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` | 1213 | J | V cron: `config/crons.py:362` → `ponto/management/commands/processar_alertas_turno.py:385` | GRAVA disputa/pergunta | D | [app/chamados/services/disputa_emissao.py:1207] |
| `chamados/services/explicador.py::_explicar_validada` | 70 | X | [nv]; chamada por `chamados/services/explicador.py:137` | EXIBE explicação; `_d0` é calculado nesse sítio | [nv] | [app/chamados/services/explicador.py:65] |
| `chamados/services/materializacao.py::_plantio_preenche_furo_na_data` | 434, 455 | J | [nv]; chamadas internas em `chamados/services/materializacao.py:220` e `:920` | GRAVA plantio indiretamente, conforme resultado da guarda; caminho vivo [nv] | D | [app/chamados/services/materializacao.py:404] |
| `chamados/services/passivo_validado.py::_dia_do_plantio` | 113 | J | A: `materializar_passivo_validado.py:113` → `passivo_validado.py:344`; não agendado conforme `config/crons.py:859` | GRAVA plantio no modo apply | D | [app/chamados/services/passivo_validado.py:98] |
| `chamados/services/passivo_validado.py::_plantar_o_dia` | 547, 549 | J | A: `materializar_passivo_validado.py:113` → `passivo_validado.py:443` | GRAVA plantio no modo apply; a guarda pode reverter o ato | D | [app/chamados/services/passivo_validado.py:537] |
| `chamados/services/passivo_validado.py::reparar` | 348 | X | A: `materializar_passivo_validado.py:113` | EXIBE/conta divergência de competência nesse sítio | [nv] | [app/chamados/services/passivo_validado.py:344] |
| `colaboradores/services/calendario.py::contexto_calendario` | 273 | X | V request: `colaboradores/views.py:1990` | EXIBE batidas retratadas por dia civil | [nv] | [app/colaboradores/services/calendario.py:258] |
| `core/esteira_vigia.py::instante_de_ontem` | 1082 | [nv] | I: chamado para congelar testes em `core/esteira_vigia.py:1358` | EXIBE/fornece instante para teste | [nv] | [app/core/esteira_vigia.py:1080] |
| `escala/detector_proposta.py::medir_padrao` | 58 | J | [nv]; chamada por `escala/detector_proposta.py:120` | EXIBE proposta no caminho read-only citado; outros efeitos [nv] | D | [app/escala/detector_proposta.py:49] [app/escala/detector_proposta.py:118] |
| `escala/management/commands/hub_smoke.py::Command.handle.win_map` | 95 | [nv] | I: smoke | EXIBE/mede [nv quanto ao consumo completo do mapa] | [nv] | [app/escala/management/commands/hub_smoke.py:91] |
| `escala/regua_defesa.py::diagnostico_regua` | 157 | J | V cron: `config/crons.py:428` → `escala/management/commands/termometro_regua.py:43` → `escala/regua_defesa.py:735` | GRAVA escore no vínculo | D | [app/escala/regua_defesa.py:153] [app/escala/regua_defesa.py:730] |

A contagem abaixo inclui **[nv] como balde de conferência**, não como nova classe do aval. Soma: **32**. Os rótulos e as classificações correspondem às linhas da tabela acima. [app/docs/BACKLOG.md:1846]

| (a) × (b) | VIVO | AVULSO | INSTRUMENTO | Caminho [nv] | Total |
|---|---:|---:|---:|---:|---:|
| JUÍZO PARALELO | 5 | 4 | 1 | 12 | 22 |
| ENTRADA DO PAR | 0 | 2 | 0 | 2 | 4 |
| EXIBIÇÃO | 1 | 1 | 0 | 1 | 3 |
| Classe [nv] | 0 | 0 | 3 | 0 | 3 |
| **Total** | **6** | **7** | **4** | **15** | **32** |

**JUÍZO PARALELO + VIVO + GRAVA/PAGA comprovados nesta leitura: 5 funções.** As duas primeiras influenciam uma escrita pela decisão que retornam; a terceira decide a emissão de pergunta; a quarta grava o contexto do chamado; a quinta influencia o escore gravado. [app/ponto/management/commands/flip_automatico.py:66] [app/ponto/management/commands/processar_alertas_turno.py:379] [app/api/views.py:717] [app/escala/regua_defesa.py:730]

- `chamados/services/desvio_escala.py::medir_desvio` — causa do flip e nota. [app/chamados/services/desvio_escala.py:65] [app/ponto/management/commands/flip_automatico.py:66]
- `chamados/services/disputa_emissao.py::_orfao_resolvido_cross_meianoite` — guarda da fábrica de pergunta. [app/chamados/services/disputa_emissao.py:729] [app/chamados/services/disputa_emissao.py:797] [app/ponto/management/commands/processar_alertas_turno.py:382]
- `chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` — emissão de disputa/pergunta. [app/chamados/services/disputa_emissao.py:1213] [app/ponto/management/commands/processar_alertas_turno.py:385]
- `api/views.py::api_bater_ponto` — chamado com data no contexto. [app/api/views.py:719] [app/api/urls.py:67]
- `escala/regua_defesa.py::diagnostico_regua` — escore gravado no vínculo. [app/escala/regua_defesa.py:157] [app/escala/regua_defesa.py:735] [app/config/crons.py:428]

O medidor declara como **não vistos pelo método**, entre outros, pareadores por estado fora das portas nomeadas, outras regras de meia-noite e heranças, além de certas chamadas dinâmicas e conteúdo em HTML/JS. Não há contagem desses pontos cegos no dado fornecido. [app/core/medida_turno_do_par.py:35] [app/core/medida_turno_do_par.py:44] [app/core/medida_turno_do_par.py:46]

## 18/11 14:46 · sessao 9dc0fd09c600 · ESPURIAS-PRE-MAPA · censo · origem=Code

A conferência abaixo cobre as **33 linhas fornecidas no anexo**. Ela não fecha todas as classificações: `[nv]` indica célula que não consegui provar. A linha do BACKLOG define as três classes e registra as fatias; ela nomeia `_dia_de` na fatia 2 e a herança por gap como caso das duas fatias. [app/docs/BACKLOG.md:1846; app/docs/BACKLOG.md:2051]

Na tabela, os caminhos são relativos a `app/`; **E** = ENTRADA DO PAR, **J** = JUÍZO PARALELO, **I** = INSTRUMENTO e **V** = caminho vivo. Em (c), o efeito é o do caminho indicado em (b), não necessariamente uma escrita feita pela própria função. A indicação de cron é a declaração na cópia; sua instalação ou execução efetiva em produção é [não verificado]. [app/config/crons.py:1; app/config/crons.py:5]

| chave | linha(s) | (a) | (b) caminho e chamador | (c) | (d) fatia | (e) conferido |
|---|---:|---|---|---|---|---|
| `ponto/motor_calculo_v2.py::minutos_noturnos` | 97,98 | [nv] | V, motor: `ponto/motor_calculo_v2.py:983` | PAGA | [nv] | `ponto/motor_calculo_v2.py:97` |
| `ponto/motor_calculo_v2.py::turnos_via_autoridade` | 470 | E | V, motor: `ponto/motor_calculo_v2.py:530` | PAGA | [nv] | `ponto/motor_calculo_v2.py:470` |
| `ponto/propositor.py::dia_posicional_completo` | 364 | J | [nv]; chamada em `chamados/reconciliador.py:113` | [nv] | [nv] | `ponto/propositor.py:364` |
| `ponto/registro_batida.py::_casa_marco_proprio` | 43 | J | V, porta de batida: `ponto/registro_batida.py:113`; request: `ponto/services/triagem_batida.py:700` | GRAVA batida, condicional | [nv] | `ponto/registro_batida.py:43` |
| `ponto/registro_batida.py::_janela_oposto` | 28 | J | V, triagem: `ponto/services/triagem_batida.py:715` | GRAVA batida, condicional | [nv] | `ponto/registro_batida.py:28` |
| `ponto/services/cartorio.py::_julgar_colab_corpo` | 1205,1358 | E | V, cron declarado `processar_cartorio --apply`: `config/crons.py:616`; chamada: `ponto/services/cartorio.py:996` | GRAVA [nv] | [nv] | `ponto/services/cartorio.py:1205`; `ponto/services/cartorio.py:1358` |
| `ponto/services/credito_parcial.py::dia_do_periodo` | 36 | J | V, fechamento: `ponto/services/credito_parcial.py:43`; `ponto/services/fechamento.py:998` | PAGA | [nv] | `ponto/services/credito_parcial.py:36` |
| `ponto/services/dia_pago.py::_dia_de` | 132 | J | V, lavratura: `ponto/services/dia_pago.py:174` | PAGA, DiaPago | f2 DIA | `ponto/services/dia_pago.py:132`; `docs/BACKLOG.md:2051` |
| `ponto/services/dias_em_aberto.py::passivo_da_competencia` | 123 | [nv] | AVULSO, comando: `pautas/management/commands/pauta_dia_em_aberto.py:98` | EXIBE | [nv] | `ponto/services/dias_em_aberto.py:123` |
| `ponto/services/esmeril_espelho.py::assinaturas_do_colab` | 170,177 | J | V, cron declarado: `config/crons.py:640`; chamada: `ponto/services/esmeril_espelho.py:195` | [nv] | [nv] | `ponto/services/esmeril_espelho.py:170` |
| `ponto/services/espelho.py::autoridade_do_periodo` | 1029 | E | V, espelho: `ponto/services/espelho.py:1227`; request: `ponto/views.py:161` | EXIBE | [nv] | `ponto/services/espelho.py:1029` |
| `ponto/services/espelho.py::montar_dias` | 426 | J | V, espelho: `ponto/services/espelho.py:1262`; request: `ponto/views.py:161` | EXIBE | [nv] | `ponto/services/espelho.py:426` |
| `ponto/services/espelho.py::somar_periodos` | 84 | J | V, espelho: `ponto/services/espelho.py:439`; request: `ponto/views.py:161` | EXIBE | [nv] | `ponto/services/espelho.py:84` |
| `ponto/services/fechamento.py::_so_a_data` | 1569 | [nv] | [nv]; chamada: `ponto/services/fechamento.py:1595` | [nv] | [nv] | `ponto/services/fechamento.py:1569` |
| `ponto/services/fechamento.py::apurar_por_fatia` | 204 | E | V, fechamento: `ponto/services/fechamento.py:754`; request: `ponto/views.py:1255` | PAGA | [nv] | `ponto/services/fechamento.py:204`; `ponto/services/fechamento.py:253` |
| `ponto/services/fechamento.py::recalcular_fechamento_mes` | 655,657,663,683,754,793,839,851,935,947 | J | V, request: `ponto/views.py:1255` | PAGA, fechamento | [nv] | `ponto/services/fechamento.py:793`; `ponto/services/fechamento.py:935` |
| `ponto/services/flip_auto.py::fila_flip_recusas` | 140 | J | V, cron declarado: `config/crons.py:707`; chamada: `ponto/services/flip_auto.py:148` | [nv] | [nv] | `ponto/services/flip_auto.py:140` |
| `ponto/services/regularizacao.py::regularizar_turno_orfao` | 60,73 | J | V, request: `ponto/views.py:3369` | GRAVA justificativa e ausência, condicional | [nv] | `ponto/services/regularizacao.py:73`; `ponto/services/regularizacao.py:91` |
| `ponto/services/triagem_batida.py::_bloqueio_horario_clt` | 424 | [nv] | V, triagem: `ponto/services/triagem_batida.py:657` | GRAVA chamado, condicional | [nv] | `ponto/services/triagem_batida.py:424`; `ponto/services/triagem_batida.py:459` |
| `ponto/services/triagem_batida.py::ramo_regularizar_saida` | 247,254,260 | J | V, request: `ponto/views.py:339` | GRAVA justificativa | [nv] | `ponto/services/triagem_batida.py:258` |
| `ponto/signals.py::_dias_batida` | 111 | E | V, signal: `ponto/signals.py:135` | GRAVA [nv] | [nv] | `ponto/signals.py:111` |
| `ponto/signals.py::_recompute_around` | 23 | E | V, signal: `ponto/signals.py:36` | GRAVA [nv] | [nv] | `ponto/signals.py:23` |
| `ponto/turnos.py::_dia_das_batidas_puro` | 2967 | J | V, espelho: `ponto/services/espelho.py:451`; chamada: `ponto/turnos.py:2927` | EXIBE; outros efeitos [nv] | DIA-DA-BATIDA-JUIZ-UNICO [nv] | `ponto/turnos.py:2967` |
| `ponto/turnos.py::dia_do_turno_da_batida` | 2945 | E | V, chamada: `chamados/services/disputa_emissao.py:283` | [nv] | [nv] | `ponto/turnos.py:2945` |
| `ponto/turnos.py::par_completo` | 2212 | E | V, cron declarado: `config/crons.py:605`; chamada: `ponto/management/commands/detectar_par_relampago.py:353` | GRAVA batida, condicional | [nv] | `ponto/turnos.py:2212`; `ponto/management/commands/detectar_par_relampago.py:362` |
| `ponto/views.py::regularizar_retorno` | 3386 | J | V, request: `ponto/urls.py:45` | EXIBE | [nv] | `ponto/views.py:3386` |
| `relatorios/management/commands/diff_extrato_parcial.py::Command._antes` | 45,51,58,61,67,68 | E | I, chamado pelo comando: `relatorios/management/commands/diff_extrato_parcial.py:117` | EXIBE | [nv] | `relatorios/management/commands/diff_extrato_parcial.py:68` |
| `relatorios/management/commands/diff_extrato_parcial.py::Command.handle` | 102,106,140 | [nv] | I, comando `diff_extrato_parcial` | EXIBE | [nv] | `relatorios/management/commands/diff_extrato_parcial.py:102`; `relatorios/management/commands/diff_extrato_parcial.py:140` |
| `relatorios/pdf_espelho.py::_coletar_dados_espelho` | 298 | E | V, PDF: `relatorios/views.py:1098`; chamada: `relatorios/pdf_espelho.py:1046` | EXIBE | [nv] | `relatorios/pdf_espelho.py:298` |
| `relatorios/views.py::extrato_parcial` | 486,593 | [nv] | V, request: `relatorios/urls.py:17` | EXIBE | [nv] | `relatorios/views.py:486`; `relatorios/views.py:593` |
| `ponto/motor_calculo_v2.py::turnos_via_autoridade` — porta nomeada | 503 | E | V, motor: `ponto/motor_calculo_v2.py:530` | PAGA | f1 PAR [nv] | `ponto/motor_calculo_v2.py:503` |
| `ponto/motor_calculo_v2.py::MotorBase._turnos_do_juiz` — porta nomeada | 882 | E | V, motor: `ponto/motor_calculo_v2.py:2346` | PAGA | f1 PAR [nv] | `ponto/motor_calculo_v2.py:882` |
| `ponto/motor_calculo_v2.py::MotorBase.calcular_mes` — herança por gap | 2408 | J | V, fechamento: `ponto/services/fechamento.py:754`; request: `ponto/views.py:1255` | PAGA | f1 PAR e f2 DIA | `ponto/motor_calculo_v2.py:2408`; `docs/BACKLOG.md:2051` |

**Ressalva da linha `par_completo`:** o chamador citado passa uma janela pronta; a derivação na linha 2212 só roda quando `janela is None`. Portanto, o cron prova o caminho da função, mas não prova que execute esse sítio específico. [app/ponto/turnos.py:2211; app/ponto/management/commands/detectar_par_relampago.py:353]

A contagem abaixo é da **classificação parcial desta tabela**, não um fechamento sem lacunas do censo. As células `[nv]` entram explicitamente na soma:

| (a) \ (b) | VIVO | AVULSO | INSTRUMENTO | [nv] | Total |
|---|---:|---:|---:|---:|---:|
| EXIBIÇÃO | 0 | 0 | 0 | 0 | 0 |
| ENTRADA DO PAR | 11 | 0 | 1 | 0 | 12 |
| JUÍZO PARALELO | 15 | 0 | 0 | 1 | 16 |
| [nv] | 3 | 1 | 1 | 1 | 6 |
| **Total** | **29** | **1** | **2** | **2** | **34** |

A soma acima revela uma inconsistência na própria consolidação: **não fecha as 33 linhas**. A contagem correta por inspeção da tabela é **[nv]**; não a apresento como censo fechado. [não verificado]

A lista **provisória**, pelas células J + V + GRAVA/PAGA, é:

- `_casa_marco_proprio` e `_janela_oposto`: caminho de registro de batida, condicionado às guardas. [app/ponto/registro_batida.py:105; app/ponto/registro_batida.py:113; app/ponto/services/triagem_batida.py:700]
- `dia_do_periodo`: participa do cálculo de atraso no fechamento. [app/ponto/services/credito_parcial.py:43; app/ponto/services/fechamento.py:998]
- `_dia_de`: atribuição de dia na lavratura de DiaPago, com fallback pela data local da entrada. [app/ponto/services/dia_pago.py:132; app/ponto/services/dia_pago.py:174]
- `recalcular_fechamento_mes`: caminho de fechamento acionado pela view. [app/ponto/services/fechamento.py:793; app/ponto/views.py:1255]
- `regularizar_turno_orfao` e `ramo_regularizar_saida`: gravam justificativa nos caminhos indicados. [app/ponto/services/regularizacao.py:91; app/ponto/services/triagem_batida.py:258]
- `MotorBase.calcular_mes`, na herança por gap: caso registrado nas duas fatias. [app/ponto/motor_calculo_v2.py:2408; app/docs/BACKLOG.md:2051]

Essa lista não é o tamanho real fechado da obra: há efeitos, chamadores e classes marcados `[nv]`. O método também declara pontos cegos, entre eles pareadores por estado, outras regras de meia-noite e heranças não reconhecidas, além das limitações da resolução estática; não há número para o que ele não viu. [app/core/medida_turno_do_par.py:40; app/core/medida_turno_do_par.py:46]

## 18/11 14:41 · sessao 0f40a97e3846 · PRE-MAPA-E4D · censo · origem=Code

A busca literal encontrou **52 hits de escrita**, correspondentes a **26 pares campo × função**. Destes, **um par está fora das portas**: duas atribuições de `reaberturas_colab` em `montar_chamados_fio`, em memória sobre `ChamadoColaborador`, e não sobre o campo de `DisputaSupervisao`. A classificação mecânica inclui essas atribuições; a distinção de modelo está documentada em [contratos_estruturais.py:719](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/contratos_estruturais.py:719).

A unidade das tabelas é **campo × função**; as linhas listadas incluem tanto atribuições quanto itens de listas usadas em `update_fields`. As portas seguem o mapeamento fornecido nos anexos e os respectivos corpos nesta cópia. A busca também encontrou uma atribuição múltipla de `pendencia_admin` ao examinar o contexto, embora a linha não seja um hit do padrão pedido: [materializacao.py:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365).

### `resolvido_em`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:_gravar_estado:235](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:235), [250](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:250), [254](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:254) | A01 |

**0 fora das portas (rg acima, 9 hits lidos).**

### `respondido_por`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:_gravar_estado:237](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:237), [238](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:238) | A01 |

**0 fora das portas (rg acima, 8 hits lidos).**

### `fechada_em`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:fechar:2095](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2095), [2099](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2099) | A02 |
| [models.py:reabrir_admin:2312](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2312), [2317](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2317) | A03 |
| [models.py:reabrir_sistema:2336](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2336), [2340](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2340) | A04 |

**0 fora das portas (rg acima, 23 hits lidos).**

### `fechada_por`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:fechar:2096](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2096), [2099](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2099) | A02 |
| [models.py:reabrir_admin:2313](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2313), [2317](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2317) | A03 |
| [models.py:reabrir_sistema:2337](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2337), [2340](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2340) | A04 |

**0 fora das portas (rg acima, 8 hits lidos).**

### `resolucao`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:fechar:2097](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2097), [2099](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2099) | A02 |
| [models.py:reabrir_admin:2314](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2314), [2317](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2317) | A03 |
| [models.py:reabrir_sistema:2338](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2338), [2340](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2340) | A04 |
| [models.py:limpar_resolucao_residual:2428](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2428), [2429](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2429) | A24 |

**0 fora das portas (rg acima, 42 hits lidos).**

### `motivo_fechamento`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:fechar:2098](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2098), [2099](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2099) | A02 |
| [models.py:reabrir_admin:2315](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2315), [2317](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2317) | A03 |
| [models.py:reabrir_sistema:2339](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2339), [2340](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2340) | A04 |

**0 fora das portas (rg acima, 11 hits lidos).**

### `reaberturas_admin`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:reabrir_admin:2316](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2316), [2317](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2317) | A03 |

**0 fora das portas (rg acima, 4 hits lidos).**

### `reaberturas_colab`

| Escrita: arquivo, função e linha | Porta ou tipo |
|---|---|
| [models.py:reabrir_por_colab:2402](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2402), [2403](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2403) | A23 |
| [fio.py:montar_chamados_fio:390](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/fio.py:390), [394](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/fio.py:394) | **Fora das portas; outro:** anotação em memória em outro modelo. [contratos_estruturais.py:719](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/contratos_estruturais.py:719) |

### `recusa_motivo`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:carimbar_identidade:3384](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3384), [3385](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3385) | A06 |
| [disputa_emissao.py:responder_pergunta_disputa_core:1511](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1511), [1513](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1513) | A12 |
| [materializacao.py:_materializar_resposta_hora:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365), [1366](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1366) | A17 |
| [materializacao.py:_carimbar_recusa:1702](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1702), [1703](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1703) | A18 |
| [validacao.py:recusar_pergunta:916](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:916), [922](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:922) | A19 |

**0 fora das portas (rg acima, 31 hits lidos).**

### `pendencia_admin`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [materializacao.py:_materializar_resposta_hora:1366](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1366), item de `update_fields` | A17 |
| [materializacao.py:_materializar_resposta_hora:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365), atribuição múltipla encontrada no contexto, **fora dos hits do padrão literal** | A17 |

**0 fora das portas (rg acima, 7 hits lidos).**

### `data_conferida_em`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [validacao.py:conferir_data_do_fato:996](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:996), [998](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:998) | A20 |

**0 fora das portas (rg acima, 4 hits lidos).**

### `data_conferida_por`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [validacao.py:conferir_data_do_fato:997](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:997), [998](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:998) | A20 |

**0 fora das portas (rg acima, 4 hits lidos).**

### Classificação dos demais hits

As definições de campo e migration são **DEFINIÇÃO**:

- `resolvido_em`: [models.py:66](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:66), [migration 0006:28](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0006_followup_sem_glpi.py:28); `respondido_por`: [models.py:64](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:64), [migration 0006:33](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0006_followup_sem_glpi.py:33).
- `fechada_em`: [models.py:1894](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1894), índice em [1943](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1943), campo e índice em [migration 0013:24](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0013_disputasupervisao.py:24) e [34](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0013_disputasupervisao.py:34).
- `fechada_por`: [models.py:1903](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1903), [migration 0013:28](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0013_disputasupervisao.py:28); `resolucao`: [models.py:1910](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1910), [migration 0013:25](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0013_disputasupervisao.py:25).
- `motivo_fechamento`: [models.py:1930](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1930), [migration 0049:14](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0049_disputa_motivo_fechamento.py:14).
- `reaberturas_admin`: [models.py:1899](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1899), [migration 0018:15](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0018_disputasupervisao_reaberturas_admin.py:15); `reaberturas_colab`: [models.py:1895](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1895), [migration 0015:13](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0015_disputasupervisao_reaberturas_colab.py:13).
- `recusa_motivo`: [models.py:2747](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2747), [migration 0040:15](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0040_a27_recusa_motivo.py:15); `pendencia_admin`: [models.py:2743](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2743), [migration 0043:15](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0043_hx_pendencia_admin.py:15).
- `data_conferida_em`: [models.py:2645](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2645), [migration 0044:18](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0044_perguntadisputa_data_conferida_em_and_more.py:18); `data_conferida_por`: [models.py:2646](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2646), [migration 0044:23](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0044_perguntadisputa_data_conferida_em_and_more.py:23).

Os demais hits compreendem **LEITURAS** e também texto que não é operação sobre o campo: comentários, documentação, parâmetros de função, variáveis locais e argumentos de chamadas comuns. Por exemplo, os `resolucao=` nos signals são argumentos de `remover_motivo`, não keywords de `update` ou `create`: [escala/signals.py:177](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/escala/signals.py:177), [colaboradores/signals.py:80](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/colaboradores/signals.py:80). **A classificação individual, com arquivo:linha, de todos esses demais hits é [nao verificado].**

**Contagens:** 160 hits lidos; 52 hits de escrita na busca, agrupados em 26 pares campo × função; **1 par fora das portas**, com 2 hits de atribuição em memória. A atribuição adicional de `pendencia_admin` em [materializacao.py:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365) não integra os 160 nem os 52 hits. Escritas não capturadas pelo padrão literal e execução em produção: **[nao verificado]**.

## 18/11 14:40 · sessao 08fcba64cfc5 · ESPURIAS-PRE-MAPA · censo · origem=Code

O censo identificou **quatro pontos de apresentação que afirmam texto sobre a HE sem ler a decisão do dia**: o aviso e o relógio do espelho, o relógio do calendário e a observação do PDF. Os demais consumidores de estado identificados usam `estado_por_dia` ou `DecisaoHE.objects.vigentes()`; a contagem e sua unidade estão no fecho. [app/templates/ponto/espelho.html:189; app/templates/ponto/espelho.html:237; app/templates/colaboradores/partials/_calendario_grade.html:127; app/templates/core/_relogio_riscado.html:15; app/relatorios/pdf_espelho.py:932; app/ponto/services/gestao_he.py:86; app/ponto/motor_calculo_v2.py:1416]

A tabela de juízes consultada se apresenta como lookup, aponta `app/core/juizes.py` como registro vivo e diz que o código prevalece em caso de divergência. Uma entrada específica para decisão de HE nessa tabela ou no registro: **[não verificado]**. Para a pergunta “a decisão vale hoje?”, o próprio modelo nomeia `DecisaoHE.objects.vigentes()`; `estado_por_dia` publica o mapa por colaborador e dia a partir dessa consulta. [app/docs/JUIZES.md:3; app/docs/JUIZES.md:9; app/ponto/models.py:1251; app/ponto/services/gestao_he.py:63]

Apenas `autorizado` e `nao` são estados armazenados; `nao` registra decisão e ciência, enquanto a ausência de decisão vigente é apresentada como `sem_decisao` pela Gestão. O retrato conserva dias e minutos, além do estado fotografado na lavratura; a Gestão sobrepõe a decisão viva para apresentar o estado atual. [app/ponto/models.py:1283; app/ponto/models.py:1287; app/ponto/services/he_pendente_lavrado.py:132; app/ponto/services/gestao_he.py:71; app/ponto/services/gestao_he.py:271]

| Sítio | O que imprime ou decide | Autoridade lida para o estado; texto fixo |
|---|---|---|
| [Espelho — montador](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/services/espelho.py:720) | Monta `he_fora_da_janela` a partir das pontas marcadas e de relatos do motor, inclusive dia sem previsto. É a entrada dos avisos da tela e do PDF. | Marcador de janela e relato do motor para **ocorrência e minutos**; não há leitura de decisão de HE nesse trecho. O comentário associa as ocorrências a “sem decisão”. [app/ponto/services/espelho.py:727; app/ponto/services/espelho.py:740; app/ponto/services/espelho.py:773] |
| [Espelho — aviso](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/templates/ponto/espelho.html:189) | Mostra minutos e “sem decisão” para cada ocorrência. | Lê `dia.he_fora_da_janela`, sem leitor de estado na linha. **Texto fixo:** “sem decisão”; o título também afirma que os minutos não viram HE ou não contam sem decisão. |
| [Espelho — relógio](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/templates/ponto/espelho.html:237) | Mostra relógio riscado nas células marcadas fora da janela. | Lê a marca da célula e inclui o componente com minutos, sentido e marco. **Texto fixo:** “não conta”, sem estado de HE no contexto passado ao componente. [app/templates/core/_relogio_riscado.html:15] |
| [Calendário — relógio](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/templates/colaboradores/partials/_calendario_grade.html:127) | Mostra o mesmo relógio riscado para batida marcada fora da janela. | Lê a marca da batida e inclui o mesmo componente. **Texto fixo:** “não conta”, sem estado de HE no contexto passado. [app/templates/core/_relogio_riscado.html:15] |
| [PDF do espelho](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/relatorios/pdf_espelho.py:927) | Imprime observação por ocorrência, com minutos e, quando existe, sentido e marco. | Lê `dia.he_fora_da_janela` do espelho. **Texto fixo:** “não conta sem decisão” para dia sem jornada prevista; “não conta” para ponta com marco. [app/relatorios/pdf_espelho.py:932; app/relatorios/pdf_espelho.py:937] |
| [Gestão de HE — tela](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/views.py:932) | Mostra e filtra dias sem decisão, autorizados e com ciência; apresenta também os totais. | `he_pendente_lavrado.ler` fornece o retrato; `enriquecer` sobrepõe `estado_por_dia` e consulta diretamente `DecisaoHE` vigente para os totais de minutos da decisão. Os rótulos são condicionados ao estado. [app/ponto/services/gestao_he.py:105; app/ponto/services/gestao_he.py:245; app/ponto/services/gestao_he.py:278; app/templates/ponto/gestao_he.html:259; app/templates/ponto/gestao_he.html:314] |
| [Gestão de HE — PDF](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/views.py:804) | Imprime por colaborador e em total dias sem decisão, minutos autorizados e minutos “não”. | Mesmo retrato e `enriquecer` da tela: `estado_por_dia` para estado dos dias e `DecisaoHE` vigente diretamente para totais da foto da decisão. [app/ponto/views.py:807; app/ponto/views.py:841; app/ponto/services/gestao_he.py:105; app/ponto/services/gestao_he.py:250] |
| [Gestão — seleção para atos em lote](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/services/gestao_he.py:370) | Seleciona dias ainda sem decisão para ciência ou autorização por período; `so_acima` restringe o segundo universo. | Retrato mais `enriquecer` e, portanto, `estado_por_dia`; a seleção testa `sem_decisao` e o limite do dia. [app/ponto/services/gestao_he.py:399; app/ponto/services/gestao_he.py:404] |
| [Central de relatórios](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/relatorios/views.py:37) | Mostra quantidade de dias sem decisão, indisponibilidade do contador e empresas sem retrato. | `pendentes_na_central` combina dias do retrato com `estado_por_dia`; o template condiciona as frases ao resultado. [app/ponto/services/gestao_he.py:461; app/ponto/services/gestao_he.py:475; app/templates/relatorios/index.html:125] |
| [Painel de chamados — atalho HE](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/views.py:761) | Mostra contador de dias aguardando ciência ou traço quando falta retrato. | Mesmo `pendentes_na_central`: retrato mais `estado_por_dia`. [app/ponto/services/gestao_he.py:466; app/ponto/services/gestao_he.py:475; app/templates/chamados/painel_gestao.html:105] |
| [Folha — portão de exportação](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/folha/porta_export.py:675) | Conta dias acima do limite e sem decisão para a trava configurável; a mensagem de recusa nomeia os dias. Qualquer estado presente, inclusive `nao`, sai dessa lista. | Ocorrências do espelho, `estado_por_dia` para decisão e `acima_do_limite` para o limite. A mensagem é condicionada à lista e à configuração da trava. [app/folha/porta_export.py:223; app/folha/porta_export.py:230; app/folha/porta_export.py:681; app/folha/porta_export.py:858] |
| [Motor — cálculo](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/motor_calculo_v2.py:1397) | Lê se o dia está autorizado para decidir o tratamento das pontas; também consulta o mapa para dia sem previsto. | `DecisaoHE.objects.vigentes()` diretamente, em mapa interno; a comparação é com `autorizado`. Não imprime rótulo de estado nesses pontos. [app/ponto/motor_calculo_v2.py:1416; app/ponto/motor_calculo_v2.py:1455; app/ponto/motor_calculo_v2.py:1653; app/ponto/motor_calculo_v2.py:1694; app/ponto/motor_calculo_v2.py:1739] |

No **app**, a API chama `espelho_do_colab`, mas o payload de dias examinado publica batidas, status, palavra do dia e horas; o resumo publica totais de extras. Não foi demonstrada ali a publicação do estado da decisão de HE ou de `he_fora_da_janela`; a apresentação pelo cliente nativo é **[não verificado]**. Os totais têm suas próprias fontes declaradas, incluindo folha ou ausência de apuração, e não provam o estado da decisão de cada HE. [app/api/views.py:1355; app/api/views.py:1535; app/api/views.py:1595; app/api/views.py:1648; app/api/views.py:1667]

**Fecho da contagem:** a unidade é o **sítio consumidor ou apresentador na tabela**, excluindo o montador, que é alimentação, e excluindo o app por não ter sido demonstrado leitor de estado. São **11 sítios**: seis leem `estado_por_dia` direta ou indiretamente, um lê `DecisaoHE` diretamente no motor e quatro apresentam texto sem leitor de estado. A tela e o PDF da Gestão também fazem leitura direta de `DecisaoHE` para os totais de minutos, portanto essa leitura é uma sobreposição e não acrescenta sítios à soma. [app/ponto/services/gestao_he.py:105; app/ponto/services/gestao_he.py:250; app/ponto/services/gestao_he.py:401; app/ponto/services/gestao_he.py:475; app/folha/porta_export.py:681; app/ponto/motor_calculo_v2.py:1416]

A lista de texto fixo é: **aviso do espelho**, **relógio do espelho**, **relógio do calendário** e **observação do PDF do espelho**. O componente de relógio é um único texto compartilhado por dois pontos de inclusão. [app/templates/ponto/espelho.html:189; app/templates/ponto/espelho.html:237; app/templates/colaboradores/partials/_calendario_grade.html:127; app/templates/core/_relogio_riscado.html:15; app/relatorios/pdf_espelho.py:932]

A completude fora dos caminhos examinados e qualquer ocorrência ou quantidade em produção permanecem **[não verificado]**. O arquivo de saída solicitado não foi criado, em respeito à instrução de somente leitura.

## 18/11 14:39 · sessao 498b353cf973 · ESPURIAS-PRE-MAPA · censo · origem=Code

O medidor retornou **9 pareadores e 85 derivações de dia: 94 parcelas**, correspondentes a **88 funções distintas**. A contagem soma os itens nomeados aos dois contadores; por isso, algumas funções aparecem mais de uma vez. A entrada usada foi `censo('app')`, com `python3 -I`, sem importar Django. ([medida_turno_do_par.py:10](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/medida_turno_do_par.py:10), [medida_turno_do_par.py:426](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/medida_turno_do_par.py:426), [medida_turno_do_par.py:448](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/medida_turno_do_par.py:448))

**O censo solicitado está incompleto:** não conferi classe, cadeia de chamada, efeito e fatia para cada função. Portanto, a tabela de totais por classe × caminho e a lista curta de juízos paralelos vivos que gravam ou pagam ficam **[não verificado]**. Não seria seguro preencher esses totais a partir apenas da detecção lexical, que o próprio medidor diz não ser prova semântica. ([medida_turno_do_par.py:18](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/medida_turno_do_par.py:18))

Um ponto conferido é `MotorBase.calcular_mes`: o medidor identifica a herança de dia por intervalo entre períodos; a função usa `data_jornada = _prev_dj` sob a condição de até seis horas. O fechamento chama `motor.calcular_mes`, e o BACKLOG registra essa herança na fatia 2 DIA de MOTOR-LE-O-TURNO. Sua classificação completa no recorte pedido e a prova do caminho vivo específico permanecem **[não verificado]**. ([motor_calculo_v2.py:2400](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/motor_calculo_v2.py:2400), [fechamento.py:286](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/services/fechamento.py:286), [BACKLOG.md:2051](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2051))

**Não visto pelo método**, sem número atribuído: outros pareadores por estado fora das formas reconhecidas; outras regras de meia-noite e heranças fora das formas reconhecidas; e novos sítios A1/B1 dentro de função já contada como aumento separado da catraca. A2 e B2 são aproximações para classificação, não prova de derivação da jornada. ([medida_turno_do_par.py:46](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/medida_turno_do_par.py:46))

## 18/11 14:39 · sessao 10dcfb4f1a51 · ESPURIAS-PRE-MAPA · leitura · origem=Code

A classificação mais sustentada pelo estado c9 é **FIXTURE nos dois testes identificados**, incluindo os dois subcasos de M8. A explicação causal é uma inferência a partir dos dubles atuais e do contrato: o diff c8→c9 e os erros da execução c8 não estão disponíveis para verificar a mudança e o traceback exatos. [não verificado] [nao verificado]

| Teste | Classificação e evidência | [nao verificado]
|---|---|
| `test_MORDE_quando_a_FOLHA_cobre_o_pendente_e_IGNORADO` | **FIXTURE.** O teste passa `_Aus` como cobertura e verifica que ela prevalece sobre o pedido pendente ([teste:93](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/tests/test_palavra_parcial_e_pendente.py:93)). O duble atual fornece `pk` ([teste:35](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/tests/test_palavra_parcial_e_pendente.py:35)); o ramo de cobertura de `do_dia` lê `cobertura.pk` para retornar `ausencia_id` ([dia_decidido.py:473](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:473)). Isso sustenta a leitura de duble defasado, sem demonstrar o erro histórico exato. [não verificado] |
| `test_nao_perguntado_intencional_preserva_o_juiz_da_oferta` | **FIXTURE.** O teste usa `calendario_inteiro` e espera oferta verdadeira com `em_aberto_perguntado=False` ([teste:788](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/test_e3_degrau_dono_e_porta.py:788)). O `do_dia` dublado agora devolve `quem`, `quando` e `ausencia_id` como `None` ([e3_lu3_apoio.py:100](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/e3_lu3_apoio.py:100)); o calendário acessa as três chaves diretamente ([calendario.py:748](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/colaboradores/services/calendario.py:748)). |
| `test_M8_carga_inteira_nao_engole_relogio`, **`aberto=False`** | **FIXTURE**, pela mesma costura: o subcaso usa `calendario_inteiro`, cujo retorno dublado precisa das três chaves lidas pelo calendário ([teste:752](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/test_e3_degrau_dono_e_porta.py:752); [duble:100](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/e3_lu3_apoio.py:100); [leitor:757](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/colaboradores/services/calendario.py:757)). |
| `test_M8_carga_inteira_nao_engole_relogio`, **`aberto=True`** | **FIXTURE**, pelo mesmo duble; o laço executa separadamente os estados de turno fechado e aberto ([teste:754](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/test_e3_degrau_dono_e_porta.py:754); [duble:100](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/e3_lu3_apoio.py:100)). |

O contrato atual de `do_dia` associa `quem`, `quando` e `ausencia_id` à ausência que deu a palavra; quando o dia não foi decidido por ausência, os três campos são `None` ([dia_decidido.py:434](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:434)). A mudança do contrato é o contexto da defasagem; as evidências atuais apontam para os dubles como causa, sem provar defeito no código de produção nem o histórico exato do vermelho. [não verificado]

Quanto ao merge, o anexo fornecido informa `rc 0`, uma árvore `cda458298117a0463f3b86addd90f5846cc1c7f2` e ausência de conflito entre as pontas c9 e F6F7 c7; pela informação relatada, **não houve conflito textual no merge, inclusive em `app/api/views.py`**. O merge não foi reproduzido nem suas entradas foram verificadas nesta leitura. [não verificado]

## 18/11 14:38 · sessao b89783392c74 · LAUDO-[nome] · sonda · origem=Code

Os campos de `TipoEscala` que descrevem o intervalo são:

- `hora_inicio_intervalo` e `hora_fim_intervalo`: par de horários da primeira pausa; `hora_inicio_intervalo2` e `hora_fim_intervalo2`: segundo par opcional. O código considera uma pausa cadastrada quando o par está completo. [app/escala/models.py:52](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/escala/models.py:52), [app/escala/models.py:259](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/escala/models.py:259)
- `intervalo_modo`: distingue horário fixo declarado de intervalo por duração; `intervalo_duracao_min` informa a duração para o segundo modo. [app/escala/models.py:94](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/escala/models.py:94)
- `intervalo_indenizavel`: flag distinta dos horários; o motor documenta que não a lê para esse cálculo. [app/escala/models.py:106](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/escala/models.py:106), [app/ponto/motor_calculo_v2.py:822](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/motor_calculo_v2.py:822)
- `horarios_por_dia` pode substituir os marcos da primeira pausa para um dia; no modo `duracao`, os marcos fixos de intervalo não são emitidos. [app/escala/models.py:326](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/escala/models.py:326)

Assim, o **horário pré-assinalado no cadastro** é declarado pelo par de início e fim da pausa, sujeito ao horário específico do dia quando houver substituição; a flag `False` não declara esse horário. [app/escala/models.py:259](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/escala/models.py:259), [app/escala/models.py:326](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/escala/models.py:326)

A sonda `sonda_laudo2_c2.py` não foi localizada na cópia e não pude executá-la; o ambiente informado também não dá acesso ao banco. **A tabela abaixo reproduz e soma a saída raspada fornecida**, cuja execução independente permanece **[não verificado]**. Para localizar essa fonte, conto `sonda_leitura` como linha 1 do bloco “DADO RASPADO”. [dado raspado:1]

| Grupo da saída | Batidas | Dias | Minutos | Templates no grupo |
|---|---:|---:|---:|---:|
| `duracao`, horas declaradas `False` | 2 | 106 | 6.360,00 | 10 |
| `duracao`, horas declaradas `False` | 3 | 1 | 60,00 | 10 |
| `duracao`, horas declaradas `False` | 4+ | 6 | 72,90 | 10 |
| `fixo`, horas declaradas `False` | 2 | 3 | 180,00 | 2 |
| `fixo`, horas declaradas `False` | 4+ | 2 | 120,00 | 2 |
| `fixo`, horas declaradas `True` | 1 | 1 | 60,00 | 97 |
| `fixo`, horas declaradas `True` | 2 | 226 | 13.724,00 | 97 |
| `fixo`, horas declaradas `True` | 3 | 168 | 10.384,00 | 97 |
| `fixo`, horas declaradas `True` | 4+ | 208 | 6.003,96 | 97 |
| **Total** | | **721** | **36.964,86** | |

Os templates são contagens **por grupo**, repetidas na tabela para cada faixa de batidas; não são contagens por faixa nem se pode comprovar que sejam distintos entre grupos. A soma dos dias e minutos fecha com as faixas `MOTOR_FALSE` da saída. [dado raspado:4–21]

A separação informada pela sonda é **603 dias e 30.171,96 minutos** em `fixo|horas_declaradas=True`, contra **118 dias e 6.792,90 minutos** nos demais grupos com flag `False`. O primeiro é o grupo que a saída identifica como tendo modo fixo e horas declaradas; chamar todos esses dias de pré-assinalados **de fato no dia** fica **[não verificado]**, pois o código da sonda e a aplicação de eventuais horários por dia não estão disponíveis no dado. A definição exata usada pela sonda para `horas_declaradas` também é **[não verificado]**. [dado raspado:10–21], [app/escala/models.py:298](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/escala/models.py:298)

- **col868, 14/11:** duas batidas, 60 minutos e flag `False`; grupo `GRUPO_PRE` específico **[não verificado]**. [dado raspado:24]
- **col934, 15/11:** duas batidas, 60 minutos e flag `False`; grupo `GRUPO_PRE` específico **[não verificado]**. [dado raspado:31]

As linhas individuais não informam `intervalo_modo` nem `horas_declaradas`; as mesmas duas batidas aparecem em todos os três grupos, portanto não identificam o grupo de cada caso. [dado raspado:10–18], [dado raspado:24], [dado raspado:31]

## 18/11 14:37 · sessao e99990621aef · PRE-MAPA-E4D · verificador · origem=Code

**Resultado: o contrato contém as cinco partes, mas o método da linha 4 não prova que os baldes correspondam à cobertura real nem que o universo esteja completo.** Há um furo demonstrável: o contrato limita a leitura a AST de `app/`, mas há templates em `app/` que leem diretamente os campos. A classificação efetiva de qualquer sítio por fatia permanece **[não verificado]**, pois os diffs e `fatias.txt` não foram fornecidos. ([contrato.txt:1–4, dado fornecido]; [modal_fio.html:219](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/templates/chamados/partials/modal_fio.html:219))

**Linha 1 — quem chama.** A parte existe. O comando declara envelope de memória, caminho em `logs/PRE-MAPA-E4D/` e leitura da cópia; o complemento restringe a exceção nominal a esse script e exige envio prévio do contrato. A linha não demonstra que ele foi mandado uma vez antes da construção, que a cópia corresponde ao HEAD indicado ou que havia memória livre: **[não verificado]**. O comando também não identifica onde grava `leitores.out`. ([contrato.txt:1, dado fornecido]; [complemento1_pre_mapa_e4d_1811_1440.txt:4, dado fornecido]; [contrato.txt:2, dado fornecido])

**Linha 2 — quem espera.** A parte existe e declara o item 4 como consumidor, além de excluir `bin/`, selo, hook, cron e pouso. O complemento também exclui construtor, Agent, pista e suíte; a linha 2 não os repete expressamente, embora a cláusula “só o item 4” restrinja o consumidor. O aval pede leitura **fora dos juízes** e a linha 2 não declara essa exclusão; tampouco ela aparece nas demais linhas. ([contrato.txt:1–5, dado fornecido]; [complemento1_pre_mapa_e4d_1811_1440.txt:4, dado fornecido]; [pre_mapa_e4d_1811_1425.txt:10, dado fornecido]) [nao verificado]

**Linha 3 — falha e limite.** A parte existe: declara 4 GB, 10 minutos, erro de parse como contagem incompleta e reconhecimento de apelidos, `getattr` e strings em `values()`/`filter()`. O comando da linha 1 estabelece o limite de memória, mas não mostra como os 10 minutos serão impostos; o comportamento em estouro de tempo ou memória também não está especificado. Strings em `order_by()` e nomes calculados para `getattr` não são explicitamente cobertos. ([contrato.txt:1,3, dado fornecido])

**Linha 4 — medida e baldes.** A parte existe e traz os três baldes, as fatias pendentes e a amostra. Há, porém, condições sem definição suficiente para aplicar a regra: uma função pode intersectar diffs de mais de uma fatia, e o contrato não diz como desempatar; também não define como tratar linhas removidas ou movidas, ou uma mudança na função que não migre aquele campo. Uma coincidência de função e diff, por si só, não demonstra cobertura semântica do campo. ([contrato.txt:4, dado fornecido])

Casos concretos que expõem o limite, sem afirmar um balde efetivamente errado:

- **String e filtro:** `respondida_sem_resposta` filtra `respondida_em__gte` e lê `resposta_colab` por `values_list`. Sua detecção depende de interpretar essas formas; a fatia que cobre esse sítio é **[não verificado]**. ([vigia_fio.py:738](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/vigia_fio.py:738), [vigia_fio.py:747](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/vigia_fio.py:747))
- **`getattr` dinâmico:** em `historico.py`, os nomes `respondida_em` e `validada_em` vêm de uma tupla e são passados pela variável `campo` a `getattr`. A promessa genérica de reconhecer `getattr` não explica se esse fluxo será resolvido. ([historico.py:96](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/historico.py:96))
- **Leitura indireta:** um consumidor acessa `p.estado`; a propriedade chama `DecisaoPergunta.leitura`, que chama `linhas_da_projecao_sem_linha`, onde há leituras diretas da projeção. Uma interseção feita apenas na função consumidora não demonstra a cobertura desse caminho. ([explicador.py:95](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/explicador.py:95), [models.py:3225](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3225), [models.py:4312](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:4312), [models.py:4251](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:4251))
- **Template:** `modal_fio.html` lê `resposta_colab`, `respondida_em` e `validada_em`. Essas leituras não são nós do AST Python da função que renderiza o template; a restrição da linha 1 deixa sua inclusão no censo sem demonstração. ([modal_fio.html:217](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/templates/chamados/partials/modal_fio.html:217); [contrato.txt:1, dado fornecido])
- **Resposta da API:** o código lê diretamente `p.resposta_colab` e também a propriedade `p.resposta_display`; esta, por sua vez, chama a leitura da sequência com o passivo da projeção. São caminhos diferentes no mesmo sítio consumidor, que a mera coincidência com um diff não distingue. ([views.py:1163](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/api/views.py:1163), [models.py:3811](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3811), [models.py:4295](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:4295))

Um caso concreto de **classificação efetivamente errada por fatia**, inclusive por apelido de import, é **[não verificado]**: não foram fornecidos os diffs dos commits nem o resultado do instrumento. A lista definitiva de campos do item 1 também é **[não verificado]**; os exemplos acima usam campos de projeção identificados no próprio código. ([models.py:4238](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:4238))

**Universo.** Sim, há furo: mesmo que a soma dos três baldes seja igual ao universo *detectado*, isso não prova que esse universo inclua os leitores em templates ou os caminhos indiretos citados. A igualdade também não garante exclusividade se um sítio puder entrar em mais de um balde; o contrato não define essa resolução. A amostra de cinco por balde, embora exigida pelo contrato, não demonstra completude dos leitores omitidos. ([contrato.txt:1,4, dado fornecido]; [modal_fio.html:219](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/templates/chamados/partials/modal_fio.html:219))

**Linha 5 — quem desliga.** A parte existe: a sessão encerra ao fechar o item 4, o arquivo permanece em `logs/` e a ida a `bin/` ou a selo exige vaga e pouso próprios. A aplicação dessa condição a um artefato construído é **[não verificado]**. ([contrato.txt:5, dado fornecido]; [complemento1_pre_mapa_e4d_1811_1440.txt:4, dado fornecido]) [nao verificado]

## 18/11 14:36 · sessao c52e48e0f93d · LAUDO-[nome] · sonda · origem=Code

**col868: o espelho mostra 21 min fora da janela, e o motor publica 21,209933 min para o período.** A regeneração mudou os marcos de 14/11 na mesma manhã da correção de fase e explica geometricamente as duas pontas; a autoria exata da regeneração pelo comando e o dono L-099 permanecem **[não verificado]**. [DADO RASPADO:12–14,29–31; autoria e dono: não verificado]

As referências `DADO RASPADO:linha` abaixo numeram as linhas do resultado fornecido na mensagem, começando em `sonda_leitura` como linha 1; não são números de linha verificados de um arquivo em disco. A sonda foi informada como já executada; não houve nova execução nem acesso ao banco nesta resposta. [DADO RASPADO:1; nova execução: não verificado]

1. **Minutos de 14/11.** No espelho, a entrada às **18:42** está **18 min antes** do marco das 19:00, e a saída às **07:03** está **3 min depois** do marco das 07:00: **21 min no total**. [DADO RASPADO:28–30]

   O motor retornou um período, com entrada em **14/11 às 18:42:28,063** e saída em **15/11 às 07:03:40,659**, horário local UTC−3, e **21,209933333333332 min** fora da janela. O motor soma os minutos retirados das duas pontas; o valor informado conserva os segundos. [DADO RASPADO:31; app/ponto/motor_calculo_v2.py:2759,2787]

2. **Células de col868.** Todas têm origem `gerada`, `editada_em=None` e uma regeneração; horários abaixo são locais UTC−3. [DADO RASPADO:9,12,15]

   | Data | `gerada_em` | `regenerada_em` | DNA atual × anterior |
   |---|---|---|---|
   | 13/11 | 21/10 05:50:08,771086 | 18/11 08:19:24,040195 | Atual: `trabalha=False`, sem marcos ou lâmpadas. Anterior: entrada 13/11 19:00, intervalo 14/11 01:00–02:00 e saída 14/11 07:00. [DADO RASPADO:9–11] |
   | 14/11 | 21/10 05:50:08,772390 | 18/11 08:19:24,041817 | Atual: `trabalha=True`, entrada 14/11 19:00, intervalo 15/11 01:00–02:00 e saída 15/11 07:00. Anterior: sem marcos ou lâmpadas. [DADO RASPADO:12–14] |
   | 15/11 | 21/10 05:50:08,773706 | 18/11 08:19:24,043138 | Atual: `trabalha=False`, sem marcos ou lâmpadas. Anterior: entrada 15/11 19:00, intervalo 16/11 01:00–02:00 e saída 16/11 07:00. [DADO RASPADO:15–17] |

   Nos três DNAs, o vínculo mudou de **1377 para 1603**; permaneceram `tipo_escala_id=380`, ciclo `12x36`, versão 2, intervalo não livre e jornada de 720 min. [DADO RASPADO:10–11,13–14,16–17]

3. **Gestão.** O retrato existe, cobre 21/10–20/11 e foi calculado em **18/11 às 06:43:43,226639 locais** — depois da regeneração das células. col868 não consta nele; portanto, 14/11 não consta sob col868 nesse retrato. A Gestão sem filtros também não retornou col868. A razão da ausência no retrato é **[não verificado]**. [DADO RASPADO:12,18–22,25; app/ponto/services/gestao_he.py:236; razão: não verificado]

   Não foram encontradas decisões para o dia, e o leitor da Gestão retornou `sem_decisao`. [DADO RASPADO:23–24]

4. **Relação com a fase e dono.** A célula de 14/11 foi regenerada às **08:19:24** e passou de DNA sem marcos para os marcos de 19:00–07:00; esses marcos, comparados às batidas informadas, explicam os **18 + 3 min** exibidos. Isso é consistente com a correção de fase relatada para cerca de 08:19, mas o dado não identifica o comando que produziu a regeneração nem apresenta uma medição de produção imediatamente anterior que, isoladamente, prove a origem causal dos minutos. [DADO RASPADO:12–14,29,31; autoria e medição anterior: não verificado] [nao verificado]

   **Dono L-099: [não verificado].** A mudança do DNA e a ausência no retrato estão demonstradas, mas o dado não estabelece por que a fase anterior ou a nova estava correta, nem por que o retrato posterior não incluiu col868; por isso não sustenta escolher entre ESTRUTURA, CADASTRO e BATIDA. [DADO RASPADO:10–14,20–22; correção da fase e causa da ausência: não verificado]

## 18/11 14:29 · sessao ea61c0add5dd · PRE-MAPA-E4D · censo · origem=Code

**Censo parcial: a exaustividade da parte B e as contagens de escritores permanecem [nao verificado].** A varredura encontrou os sítios abaixo, mas não terminou a resolução de todos os nomes dinâmicos, aliases, defaults e gravações indiretas em `app/` inteiro. A associação de cada escrita a um ato substituto também permanece [nao verificado], salvo a ausência explícita de ato no fluxo L-130. Não há dados de banco ou log para verificar execução em produção. [app/chamados/services/validacao.py:414](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:414)

A unidade é **campo × função**. Nos nomes abaixo, a referência aponta para a atribuição ou gravação; chamadas que delegam à porta são identificadas como tais. A classificação de exceção FATO dos escritores adicionais está **[nao verificado]**: o anexo declara seis portas como exceções, mas isso não basta para atribuir automaticamente essa classificação a cada chamador.

| Campo | Escritor comprovado fora das portas A01–A24/27 | Tipo | Ato substituto | Exceção FATO |
|---|---|---|---|---|
| `status_local` | [models.py:abrir:1000](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1000), defaults em `get_or_create`; [models.py:criar:1154](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1154); [0038:registrar:10](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0038_a3_registrado_backfill.py:10), [desfazer:15](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0038_a3_registrado_backfill.py:15); [0039:backfill:24](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0039_a3_via_backfill.py:24), [desfazer:33](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0039_a3_via_backfill.py:33) | outro; migration | [nao verificado] | [nao verificado] |
| `resolvido_em` | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| `via_encerramento` | [0039:backfill:24](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0039_a3_via_backfill.py:24); [desfazer:34](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0039_a3_via_backfill.py:34) | migration | [nao verificado] | [nao verificado] |
| `respondido_por` | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| `fechada_em` | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| `fechada_por` | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| `resolucao` | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| `motivo_fechamento` | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| `reaberturas_admin` | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| `reaberturas_colab` | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| `motivos` | [models.py:aparar_motivo_respondido:2129](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2129); [disputa_emissao.py:_abrir_ou_agregar_disputa:550](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:550); [criar_disputa_a_partir_de_chamados_orfaos:1097](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1097); [criar_disputa_ancora_de_chamado:1372](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1372) | outro / save | [nao verificado] | [nao verificado] |
| `materializada_em` | **Devoluções e ressuscitação:** sítios D1, D2, D3 e D4 definidos abaixo | outro, por A07/A08 | [nao verificado] | [nao verificado] |
| `materializada_por` | D1, D2, D3 e D4 | outro, por A07/A08 | [nao verificado] | [nao verificado] |
| `via_resolucao` | D1, D2, D3 e D4 | outro, por A07/A08 | [nao verificado] | [nao verificado] |
| `respondida_em` | D2 e D3 | outro, por A08 | D2: [nao verificado]; D3: nenhum ato de reabertura/devolução no fluxo | [nao verificado] |
| `resposta_colab` | D2 e D3 | outro, por A08 | D2: [nao verificado]; D3: nenhum ato de reabertura/devolução no fluxo | [nao verificado] |
| `validada_em` | D1 e D2 | save / outro, por A07/A08 | [nao verificado] | [nao verificado] |
| `validada_por` | D1 e D2 | save / outro, por A07/A08 | [nao verificado] | [nao verificado] |
| `recusa_motivo` | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| `pendencia_admin` | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| `data_conferida_em` | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| `data_conferida_por` | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| `veredito_lavrado` | **Lavra:** sítios L1–L5 definidos abaixo | save / cron-comando / outro | [nao verificado] | [nao verificado] |
| `julgado_em` | L1–L5 | save / cron-comando / outro | [nao verificado] | [nao verificado] |
| `janela_expira_em` | L1–L5 | save / cron-comando / outro | [nao verificado] | [nao verificado] |

Os sítios abreviados na tabela são:

- **D1:** [validacao.py:devolver_paridade_bug_par_103:346](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:346): descarimba por A07 e salva a limpeza da validação; registra `DEVOLUCAO`. A equivalência com um número A-NN é [nao verificado].
- **D2:** [models.py:ressuscitar_perguntas:1702](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1702): passa os quatro campos de resposta/validação a A08 e, nessa mesma operação, limpa os três campos de materialização.
- **D3:** [validacao.py:devolver_instante_sem_texto_l130:440](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:440): passa o par de resposta a A08 e limpa também os três campos de materialização. O próprio fluxo declara não registrar ato de reabertura/devolução. [validacao.py:414](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:414)
- **D4:** [relavra_via.py:desfazer_carimbo_sem_lastro:246](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/relavra_via.py:246): chama A07, que salva os três campos do carimbo. [models.py:3399](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3399)
- **L1:** [veredito_lavrado.py:lavrar:270](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/veredito_lavrado.py:270), com `save` quando solicitado; o comando [lavrar_perguntas_stale.py:handle:89](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/management/commands/lavrar_perguntas_stale.py:89) o chama.
- **L2:** [veredito_lavrado.py:gravar_lavras_em_lote:250](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/veredito_lavrado.py:250), `bulk_update`; [backfill_veredito_lavrado.py:handle:138](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/management/commands/backfill_veredito_lavrado.py:138) prepara os valores e chama o lote.
- **L3:** [fio.py:reapontar_resolvedora:975](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/fio.py:975), lavra em memória e salva os três campos.
- **L4:** [fio.py:rechavear_data_evento:1091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/fio.py:1091), lavra em memória e salva os três campos.
- **L5:** [models.py:_nascer:3195](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3195), no ramo de reuso; sua separação da porta A22 para a contagem pedida está **[nao verificado]**.

Em `motivos`, uma mesma função pode conter mais de um ponto de atribuição ou criação: a tabela usa a unidade campo × função, não campo × linha. Por exemplo, `criar_disputa_a_partir_de_chamados_orfaos` também grava em [disputa_emissao.py:1137](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1137), [1145](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1145), [1174](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1174) e [1269](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1269).

A busca textual executada sobre `app/` foi:

```bash
rg -l --glob '*.py' --glob '!**/tests/**' --glob '!**/test_*.py' '\b(status_local|resolvido_em|via_encerramento|respondido_por|fechada_em|fechada_por|resolucao|motivo_fechamento|reaberturas_admin|reaberturas_colab|motivos|materializada_em|materializada_por|via_resolucao|respondida_em|resposta_colab|validada_em|validada_por|recusa_motivo|pendencia_admin|data_conferida_em|data_conferida_por|veredito_lavrado|julgado_em|janela_expira_em)\b' app/
```

Também executei uma passagem AST em memória sobre arquivos Python de `app/`, procurando atribuições de atributo, palavras-chave em `update`/`create`/`get_or_create`/`update_or_create`, e literais de campos em `setattr`, `bulk_update` e `update`. A passagem não resolve sozinha nomes dinâmicos nem o fluxo completo de valores; por isso, ausência de resultado não prova ausência de escritor. A exclusão textual acima também não exclui todos os arquivos de teste com outros nomes; testes não foram contados. **[nao verificado]** [nao verificado]

**Contagens:** campos solicitados **25**; escritores fora das portas **[nao verificado]**; escritores sem ato substituto **[nao verificado]**; escritores em exceção FATO **[nao verificado]**.

## 18/11 14:26 · sessao 5f2a628261a9 · PRE-MAPA-E4D · censo · origem=Code

**Censo parcial: não consegui comprovar todos os escritores nem fechar as contagens.** A busca encontrou escritores posteriores ao inventário do anexo, inclusive uma devolução de paridade e a ressuscitação em lote; portanto, seria incorreto apresentar a tabela abaixo como exaustiva. A identidade Git da cópia também está **[nao verificado]**: o diretório fornecido não contém metadados Git.

A unidade pedida é **campo × função**. Nas linhas abaixo, agrupo campos quando compartilham o mesmo sítio para facilitar a leitura; isso não constitui uma contagem de unidades.

### A. Portas verificadas

Caminhos abreviados: `models.py` = [app/chamados/models.py](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py); os serviços ficam em `app/chamados/services/`.

| Porta | Escrita de estado identificada nesta cópia |
|---|---|
| A01 | `status_local`, `resolvido_em`, `via_encerramento`, `respondido_por` e urgência do chamado; a porta também relavra perguntas. A classificação do estado canônico do chamado como projeção a parar é **[nao verificado]**. ([models.py:234](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:234), [models.py:302](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:302)) |
| A02 | `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; há carimbo delegado de pergunta. ([models.py:2082](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2082), [models.py:2095](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2095)) |
| A03 | Os quatro campos de fechamento acima; também grava `reaberturas_admin` e, condicionalmente, `motivos`. A classificação desses dois últimos como projeção a parar é **[nao verificado]**. ([models.py:2312](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2312)) |
| A04 | Os quatro campos de fechamento. ([models.py:2336](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2336)) |
| A05 | `materializada_em`, `materializada_por`, `via_resolucao`; condicionalmente `respondida_em`, `validada_em`, `validada_por` e os três campos de lavra `veredito_lavrado`, `julgado_em`, `janela_expira_em`. A lavra é cache declarado no anexo; sua inclusão entre os campos a parar é **[nao verificado]**. ([models.py:3293](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3293), [models.py:3324](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3324), [models.py:3345](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3345)) |
| A06 | `materializada_em`, `materializada_por`, `via_resolucao` e, se fornecido, `recusa_motivo`. ([models.py:3378](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3378)) |
| A07 | Limpa `materializada_em`, `materializada_por`, `via_resolucao`; com `salvar=False`, prepara a alteração em memória para o chamador. ([models.py:3399](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3399)) |
| A08 | Limpa os três campos de A07 e grava os campos recebidos por `**outros`; os chamadores comprovados incluem R/V. ([models.py:3448](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3448), [models.py:3485](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3485), [models.py:1702](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1702)) |
| A09 | `via_resolucao`. ([models.py:3496](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3496)) |
| A10 | `validada_em`, `validada_por`. ([validacao.py:168](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:168)) |
| A11 | Limpa `validada_em`, `validada_por`. ([validacao.py:273](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:273)) |
| A12 | `resposta_colab`, `respondida_em`; pode limpar `validada_em`, `validada_por`, preparar descarimbo e gravar `recusa_motivo`. ([disputa_emissao.py:1481](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1481), [disputa_emissao.py:1494](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1494), [disputa_emissao.py:1511](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1511)) |
| A13 | Delega ao lote a limpeza de `respondida_em`, `resposta_colab`, `validada_em`, `validada_por` e dos três campos de carimbo. ([disputa_emissao.py:1617](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1617), [models.py:3485](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3485)) |
| A14 | `resposta_colab`, `respondida_em`. ([disputa_emissao.py:1682](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1682)) |
| A15 | `respondida_em`. ([disputa_emissao.py:1705](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1705)) |
| A16 | `via_resolucao` e lavra. ([relavra_via.py:125](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/relavra_via.py:125)) |
| A17 | `pendencia_admin`, `recusa_motivo`; efeitos e carimbos delegados requerem discriminação adicional **[nao verificado]**. ([materializacao.py:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365)) |
| A18 | `recusa_motivo`; no ramo de colisão, delega o carimbo de identidade. ([materializacao.py:1693](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1693)) |
| A19 | `recusa_motivo` e limpeza delegada por A13. ([validacao.py:916](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:916)) |
| A20 | `data_conferida_em`, `data_conferida_por`. ([validacao.py:996](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:996)) |
| A21 | Carimbo de identidade delegado; outros campos de estado próprios da disputa exigem discriminação adicional **[nao verificado]**. ([models.py:2218](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2218)) |
| A22 | Constrói pergunta com `**campos` e aceita os defaults do modelo; a enumeração dos campos efetivamente fornecidos por todos os chamadores está **[nao verificado]**. ([models.py:3012](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3012), [models.py:3206](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3206)) |
| A23 | Grava `reaberturas_colab` e delega correção; a classificação do contador como projeção a parar é **[nao verificado]**. ([models.py:2401](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2401)) |
| A24 | Limpa `resolucao`. ([models.py:2428](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2428)) |
| Porta 27 | `disparar_perguntas_competencia`: o corpo examinado conta e relata os alvos; escrita direta de projeção nesse caminho **[nao verificado]**. ([RELATO.md:2240](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/RELATO.md:2240), [disparar_perguntas_competencia.py:162](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/management/commands/disparar_perguntas_competencia.py:162)) |

### B–C. Escritores adicionais comprovados — tabela parcial

A coluna “ato” abaixo identifica apenas uma correspondência comprovada no sítio; **não determina** que o ato substitua integralmente todas as propriedades do campo. Onde isso não foi provado, permanece **[nao verificado]**.

| Campo | Escritor | Tipo | Ato da E4a que o substitui | Exceção FATO |
|---|---|---|---|---|
| `validada_em`, `validada_por` | [validacao.py:devolver_paridade_bug_par_103:349](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:349) | outro | A11 **[nao verificado]**; o próprio sítio registra `DEVOLUCAO` | [nao verificado] |
| `materializada_em`, `materializada_por`, `via_resolucao` | [validacao.py:devolver_paridade_bug_par_103:346](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:346), delegando A07 | outro | [nao verificado] | [nao verificado] |
| `respondida_em`, `resposta_colab`, `validada_em`, `validada_por`, `materializada_em`, `materializada_por`, `via_resolucao` | [models.py:ressuscitar_perguntas:1702](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1702), por A08 e `**outros` | outro | [nao verificado]; registra `REABERTURA` marcada ([models.py:1694](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1694)) | [nao verificado] |
| `respondida_em`, `resposta_colab`, `materializada_em`, `materializada_por`, `via_resolucao` | [validacao.py:devolver_instante_sem_texto_l130:440](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:440), por A08 e `**outros` | outro | nenhum ato de reabertura/devolução nesse fluxo, conforme o código ([validacao.py:414](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:414)); substituição **[nao verificado]** | [nao verificado] |
| `status_local` | [0038_a3_registrado_backfill.py:registrar:10](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0038_a3_registrado_backfill.py:10), também reversão em `:15` | migration | [nao verificado] | [nao verificado] |
| `status_local`, `via_encerramento` | [0039_a3_via_backfill.py:backfill:24](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0039_a3_via_backfill.py:24), também reversão em `:33`–`:34` | migration | [nao verificado] | [nao verificado] |

A varredura de **signals, comandos, outras migrations, registros de admin, aliases, `setattr`, `save()` e atualizações dinâmicas** não foi concluída; logo, “todo escritor fora das portas” permanece **[nao verificado]**. O arquivo [admin.py](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/admin.py) está vazio, o que por si só não prova ausência de registros ou formulários de admin em outros arquivos. **[nao verificado]**

**Contagens finais:** campos **[nao verificado]**; escritores **[nao verificado]**; escritores sem ato substituto **[nao verificado]**; destes, em exceção FATO **[nao verificado]**.

## 18/11 14:25 · sessao 6a8ed6edd114 · PRE-MAPA-E4D · verificador · origem=Code

**AVAL-DEVOLVIDO: itens 3 e 5.** Os demais itens conferem dentro dos limites abaixo. Os anexos foram fornecidos no enunciado sem numeração de linhas; por isso, suas referências de linha estão **[não verificado]**.

1. **CONFERE** — **Premissas:** E4d e os seis fatos conferem no [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089) e [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091); **ordem:** o censo não altera vagas nem abre Agent, conforme anexo PRE-MAPA, item 1 e proibição [linha não verificada]; **literal:** nenhum impedimento identificado; exaustividade dos escritores **[não verificado]**; **exportada/dinheiro/!:** o item pede leitura e registro, sem alteração declarada [anexo PRE-MAPA, item 1, linha não verificada].

2. **CONFERE** — **Premissas:** CLASSE-D está registrada e a RAIZ liga F5 à E4d ([BACKLOG.md:2025](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2025), [BACKLOG.md:2109](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2109)); **ordem:** perguntas sem construção não alteram as vagas nem abrem Agent [anexo PRE-MAPA, item 2, linha não verificada]; **literal:** pede perguntas sem respondê-las; **exportada/dinheiro/!:** consulta o destino da exportada, sem autorizar mudança no gravado ([CLAUDE.md:411](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/CLAUDE.md:411)).

3. **PROCEDE** — **Premissas:** B2b e as matrizes F2–F9 constam no [BACKLOG.md:826](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:826) e [BACKLOG.md:2117](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2117); OOM-0917 e o envelope concreto **[não verificado]**; **ordem e literal:** criar script reexecutável é construir instrumento, embora o cabeçalho diga “nenhuma vaga”, e a RAIA reserva instrumento à vaga 2 após esvaziar a fila; falta também explicitar o envio do contrato ao Codex antes da construção ([PISTA.md:170](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/PISTA.md:170); anexos PRE-MAPA item 3 e RAIA item 1, linhas **[não verificado]**); **exportada/dinheiro/!:** o censo AST declarado não prevê alteração desses dados [anexo PRE-MAPA, item 3, linha não verificada].

4. **CONFERE** — **Premissas:** a sequência E4a–E4d consta no [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089); **ordem:** a matriz depende dos itens 1 e 3 e não abre construtor ou Agent [anexo PRE-MAPA, item 4, linha não verificada]; **literal:** sua execução depende da resolução do item 3; **exportada/dinheiro/!:** descreve escritas atuais e futuras, sem autorizar executá-las [anexo PRE-MAPA, item 4, linha não verificada].

5. **PROCEDE** — **Premissas:** E3 consta “PARADA (aguarda lei)” na [GRADE-MESTRA.md:121](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/GRADE-MESTRA.md:121); a lei de 17/11 e seu literal de origem **[não verificado]**; **ordem:** registro não altera vagas nem abre Agent [anexo PRE-MAPA, item 5, linha não verificada]; **literal:** o próprio item determina AVAL-DEVOLVIDO se não encontrado o literal, e os `logs/AVAIS-*` não estão disponíveis nesta cópia **[não verificado quanto ao conteúdo]**; **exportada/dinheiro/!:** pede registro, sem alteração declarada desses dados [anexo PRE-MAPA, item 5, linha não verificada].

## 18/11 14:18 · sessao 459a4922e755 · DST-L2 · sonda · origem=Code

- **(b1)** A grade lê somente justificativas **aprovadas**; elas limpam os furos e marcam o dia como justificado, mas não mudam seu `tipo_dia`. A pendente não entra nesse filtro. Separadamente, uma Ausência pode mudar o tipo do dia. [app/escala/utils.py:1188](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1188), [app/escala/utils.py:1197](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1197), [app/escala/utils.py:1210](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1210), [app/escala/utils.py:1254](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1254).

- **(b2)** O efeito concreto da rejeição após o fecho — voltar a protestar e gerar cobrança por chamado reaberto ou novo — permanece **[não verificado]**: o dado não traz a prova em sombra pela porta real com `atomic+raise`. A porta aceita justificativa pendente, grava rejeitada e seu `save()` tenta acionar novo julgamento; esse julgamento pode não ocorrer. [app/ponto/services/justificativa.py:273](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:273), [app/ponto/signals.py:85](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:85), [app/ponto/signals.py:161](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:161). O candidato da S1 é **chamado 37191, col507, 2026-11-05**; sua justificativa já estar rejeitada não demonstra a transição pendente → rejeitada requerida para a prova. [DADO RASPADO: `candidato chamado 37191 col507 2026-11-05`; anexo `medida_b_leitura.r`, S1 e S2].

- **(b3)** Universo **35**, igual em quantidade aos **35** do anexo; diferença numérica **0**. Contagens: **pendente 0, aprovada 0, rejeitada 1, nenhuma 34**. Um chamado pode contar em mais de uma categoria de status. [DADO RASPADO: linhas `universo 35` a `nenhuma 34`; anexo `PERGUNTA-DE-DESENHO.md`, “Números”; anexo `medida_b_leitura.r`, S1]. A igualdade dos integrantes e a equivalência exata com a sonda original permanecem **[não verificado]**. A S1 lê a ata gravada pela implementação de `dia_sem_trabalho` disponível na árvore em que roda; já o cartório da cópia usa a ata do julgamento corrente. Uma diferença de código da árvore viva ou de ata poderia afetar a comparação, mas sua ocorrência aqui é **[não verificado]**. [anexo `medida_b_leitura.r`, S1; app/chamados/juizes.py:445](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:445), [app/ponto/services/cartorio.py:1457](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1457).

## 18/11 14:10 · sessao 3a71134a64be · DST-L2 · leitura · origem=Code

**A medida de produção ainda não está completa:** o dado raspado informa 35 chamados e zero justificativas pendentes pelas duas vias, mas não informa as quantidades de aprovadas, rejeitadas ou sem justificativa. [não verificado] A sonda original `sonda_impacto_dstl2.py` não está disponível nesta cópia; portanto, a equivalência exata de uma nova seleção com ela também está **[não verificado]**.

**(b1) Leitor e escritores.** O leitor da justificativa para a grade é `_marcar_ausencias`: ele filtra `Justificativa.status='aprovada'` em [app/escala/utils.py:1194](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1194). Uma justificativa aprovada marca `justificado`, limpa os furos e **não altera o `tipo_dia`**; pendente não entra nesse filtro. [app/escala/utils.py:1192](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1192), [app/escala/utils.py:1254](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1254)

Separadamente, a mesma grade consulta a cobertura de **Ausência** para folha e pode escrever `tipo_dia='ausencia'` ou `'falta'`; o filtro de status dessa via está em `_pelo_efeito`, que para folha aceita ausência aprovada. [app/escala/utils.py:1188](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1188), [app/escala/utils.py:1210](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1210), [app/ponto/turnos.py:2527](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/turnos.py:2527). A ata copia o `tipo_dia` da grade, e `_protesta` pergunta a `dia_sem_trabalho`, com exceção para cobertura parcial. [app/ponto/services/cartorio.py:730](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:730), [app/ponto/services/cartorio.py:1462](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1462), [app/chamados/juizes.py:454](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:454)

O status da justificativa nasce com padrão `pendente`; entre seus escritores estão a aprovação e rejeição individuais, a aprovação e rejeição em lote e a rejeição pelo reconciliador. [app/ponto/models.py:159](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:159), [app/ponto/services/justificativa.py:205](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:205), [app/ponto/services/justificativa.py:283](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:283), [app/ponto/views.py:1799](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1799), [app/ponto/views.py:1878](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1878), [app/chamados/reconciliador.py:576](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:576). Outro leitor da aprovada é `efeitos_da_ausencia`, usado para compor os dias justificados. [app/ponto/services/efeito_ausencia.py:28](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/efeito_ausencia.py:28), [app/ponto/services/efeito_ausencia.py:40](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/efeito_ausencia.py:40)

**(b2) Rejeição depois do fecho.** A porta individual real é `rejeitar_justificativa_dp`, chamada pela view `rejeitar_justificativa`; ela só decide uma justificativa pendente, grava `rejeitada` e chama `save()`. [app/ponto/views.py:1925](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1925), [app/ponto/services/justificativa.py:262](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:262), [app/ponto/services/justificativa.py:277](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:277)

Esse `save()` dispara o `post_save` da justificativa, que tenta rejulgar a célula; o handler pode parar se já estiver em julgamento ou se ocorrer erro engolido, e `julgar_celula` também retorna sem julgar em certas condições temporais ou sem célula. [app/ponto/signals.py:161](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:161), [app/ponto/signals.py:85](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:85), [app/ponto/services/cartorio.py:2033](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:2033)

A passagem de **pendente para rejeitada**, por si, não retira uma justificativa aprovada do filtro da grade: nenhuma das duas passa nele. [app/escala/utils.py:1194](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1194) Assim, se os demais fatos que produziram o dia sem trabalho continuarem iguais, essa troca não faz o dia voltar a protestar por essa via. [app/ponto/services/cartorio.py:1464](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1464), [app/chamados/juizes.py:454](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:454)

Se o novo julgamento encontrar furo e cumprir os portões de emissão, o cartório chama o emissor, que pode reabrir ou emitir conforme o caso; isso não está demonstrado para um dos 35. [app/ponto/services/cartorio.py:1712](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1712), [app/ponto/services/cartorio.py:1755](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1755), [app/ponto/services/cartorio.py:1773](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1773)

Depois do `save`, a porta individual **tenta criar um novo chamado interno de categoria `justificativa_rejeitada`** e criar follow-up; o gate de criação pode recusá-lo, e isso não prova a existência de cobrança do furo. [app/ponto/services/justificativa.py:289](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:289), [app/chamados/models.py:1101](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:1101), [app/ponto/services/justificativa.py:324](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:324). Pergunta nova e resultado concreto da cobrança: **[não verificado]**.

**(b3) S1 — leitura, texto não executado.** A seleção abaixo interpreta “só pela ata gravada” como `trabalha` diferente de `False`, com `dia_sem_trabalho(celula)` verdadeiro; ela usa o juiz real, inclusive a exceção de ausência parcial. [app/chamados/juizes.py:445](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:445) É uma reconstrução da descrição fornecida, não equivalência comprovada à sonda original **[não verificado]**. As vias são `Justificativa.chamado_origem` e `ChamadoColaborador.justificativa_relacionada`; um chamado com justificativas de status diferentes conta em mais de uma categoria. [app/ponto/models.py:173](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:173), [app/chamados/models.py:69](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:69)

```python
from collections import Counter
from django.db.models import Q
from chamados.catalogo.furo_modulos import FURO_MODULES
from chamados.catalogo.motor import VIVOS
from chamados.juizes import dia_sem_trabalho
from chamados.models import ChamadoColaborador
from ponto.models import Justificativa

totais = Counter()
candidato = None

qs = (
    ChamadoColaborador.objects
    .filter(modulo_origem__in=FURO_MODULES, status_local__in=VIVOS,
            celula__isnull=False)
    .select_related("celula")
    .order_by("pk")
)

for ch in qs.iterator():
    cel = ch.celula
    if cel.trabalha is False or not dia_sem_trabalho(cel):
        continue

    totais["universo"] += 1
    justs = list(
        Justificativa.objects.filter(
            Q(chamado_origem_id=ch.pk) |
            Q(pk=ch.justificativa_relacionada_id)
        ).values_list("pk", "status")
    )
    status = {s for _, s in justs}
    if not justs:
        totais["nenhuma"] += 1
    for s in ("pendente", "aprovada", "rejeitada"):
        if s in status:
            totais[s] += 1
    if candidato is None and justs:
        candidato = (ch.pk, ch.colaborador_id, cel.data)

for chave in ("universo", "pendente", "aprovada", "rejeitada", "nenhuma"):
    print(chave, totais[chave])

if candidato:
    pk, colab_id, dia = candidato
    print("candidato", "chamado", pk, "col" + str(colab_id), dia.isoformat())
else:
    print("candidato nenhum")
print("FIM_SONDA")
```

**S2 — não forneço como pronta para execução.** `_protesta` é uma função local de `julgar_colab`, não uma porta importável, e o fecho depende da ata gerada durante o julgamento. [app/ponto/services/cartorio.py:1435](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1435), [app/ponto/services/cartorio.py:1464](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1464) Além disso, a porta real tenta enviar push fora de seu `atomic` interno; um `raise` em transação externa desfaz escritas de banco, mas não desfaz uma notificação externa. [app/ponto/services/justificativa.py:273](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:273), [app/ponto/services/justificativa.py:308](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:308) Não verifiquei todos os efeitos externos possíveis do julgamento e do emissor, nem uma forma de capturar o resultado da função local sem alterar a observação; apresentar uma sonda como segura e fiel seria **[não verificado]**. A prova em sombra, portanto, permanece pendente.

## 18/11 14:09 · sessao 1b52d7327d1d · RAIA-ESPURIAS-1811-1410 · verificador · origem=Code

Os anexos foram fornecidos como texto, sem numeração de linhas; por isso, as referências a eles aparecem com **[não verificado]** quanto a arquivo:linha. A cópia disponível não contém metadados Git para conferir os hashes ou executar o merge-tree. [não verificado]

**Item 1 — CONFERE, condicionado às medições antes de cada abertura:** a ordem foi declarada e a exceção ao complemento é nominal; os cruzamentos informados ainda não foram verificados nesta cópia. [não verificado]

1. **Premissas:** as medições de DST-L2 e ESPURIA-TELA e o recorte de ESCALA não puderam ser conferidos por merge-tree. [não verificado]
2. **Aval vigente:** a nova ordem depois das 18h é uma exceção nominal ao item 1c do complemento; a restrição anterior a esse horário permanece. [não verificado]
3. **Literal:** a abertura depende de medir contra a ponta da espinha em voo; arquivo `.py` comum exige esperar o pouso ou entrar no mesmo comboio. A medição antiga não substitui essa condição. [não verificado] [nao verificado]
4. **Exportada, dinheiro ou `!`:** o item proíbe tocar exportada e declara o custo de parar instrumentos; efeito das obras sobre dinheiro ou outras matérias de `!` ainda depende dos diffs. [não verificado]

**Item 2 — CONFERE, condicionado ao pouso após a simulação e ao smoke:** o critério do “desde” e o marco de pouso repetem a resposta do aval vigente. [não verificado] [nao verificado]

1. **Premissas:** L-040 existe em [LEIS.md:88](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/LEIS.md:88). A referência do anexo a `esmeril_espelho.py:238` não corresponde ao cálculo do “desde” nesta cópia: ali está `p = r['proposta']`, em [esmeril_espelho.py:238](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/esmeril_espelho.py:238). O recorte da obra e seu censo ainda precisam ser verificados. [não verificado]
2. **Aval vigente:** a volta ao construtor, o menor “desde” ou INCOMPLETO e o pouso após a simulação com smoke constam da resposta anterior; a nova ordem é a exceção nominal ao complemento. [não verificado] [nao verificado]
3. **Literal:** comboio com a espinha, se pronta na mesma meia hora, continua sujeito ao pouso de ESCALA somente depois da simulação; a viabilidade temporal do comboio é [não verificado]. [nao verificado]
4. **Exportada, dinheiro ou `!`:** o aval proíbe tocar exportada; o diff ainda é necessário para verificar outros efeitos. Alterar dados de escala ou vínculo exige `!`, conforme [CLAUDE.md:265](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/CLAUDE.md:265). [não verificado]

**Item 3 — CONFERE para a decisão e a medição, sem considerar o pouso liberado:** a cópia confirma o precedente para `trabalha is False`; os resultados pedidos em b1–b3 ainda não estão completos. [cartorio.py:1451](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/cartorio.py:1451)

1. **Premissas:** `_protesta` exclui `trabalha is False` e o fechamento usa `reconciliar` em [cartorio.py:1458](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/cartorio.py:1458) e [cartorio.py:1533](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/cartorio.py:1533). O comportamento específico da cópia DST-L2, a reação à rejeição e a distribuição completa dos 35 são [não verificado]. O anexo informa zero pendentes por `chamado_origem` e por ponteiro, sem fornecer a contagem de aprovadas e nenhuma. [não verificado]
2. **Aval vigente:** não foi identificada colisão nos dois avais fornecidos; o item mantém a DST-L2 sem pouso até a medida. [não verificado] [nao verificado]
3. **Literal:** b2 exige prova pela porta real de rejeição em sombra com rollback; a existência da porta é visível em [justificativa.py:262](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/justificativa.py:262), mas seu resultado no caso pedido é [não verificado].
4. **Exportada, dinheiro ou `!`:** a medição pedida é em leitura e sombra; eventual cobrança ou cura após rejeição ainda não foi demonstrada. O tratamento de gravado exportado segue sujeito a `!` em [CLAUDE.md:265](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/CLAUDE.md:265). [não verificado]

**Item 4 — CONFERE para registrar o dry:** o apply permanece dependente do `!` declarado. [não verificado]

1. **Premissas:** o universo dos afetados, o número de órfãs e a situação da competência 11 não constam do dado fornecido. [não verificado]
2. **Aval vigente:** o dry foi marcado para depois da simulação, compatível com a proibição anterior de regenerar células antes dela. [não verificado]
3. **Literal:** reversão e contagem antes/depois são condições do dry; sua execução e seus resultados são [não verificado].
4. **Exportada, dinheiro ou `!`:** o registro não autoriza apply; o aval reserva o apply ao `!` com o número em mãos e proíbe tocar exportada. [não verificado]

## 18/11 13:53 · sessao 9e7ece3f7811 · LAUDO-[nome]-1811-1340 · sonda · origem=Code

O retrato de col830 foi calculado em **18/11/2026 às 04:37:31,642334, UTC−3**. A hora do pouso da S6 e a ordem entre os dois eventos estão **[não verificado]**. (DADO RASPADO, linha `col830 retrato_calculado_em`; hora da S6: [não verificado]) [nao verificado]

- **Retrato de 15/11:** `sem_decisao`, 713 minutos, com uma ponta às 06:50, `marco=None`, `sentido=dia` e 713 minutos. (DADO RASPADO, linha `col830 retrato_dia`)
- **DecisaoHE:** a consulta retornou lista vazia para o dia. (DADO RASPADO, linha `col830 decisoes`)
- **Batidas apuráveis na data local:** 06:50:03,757330 e 18:53:09,580000, ambas em UTC−3. (DADO RASPADO, linha `col830 batidas_apuraveis_data_local`)
- **Espelho:** `he_fora_da_janela=[]` e zero minutos. (DADO RASPADO, linha `col830 espelho_dia`)
- **Lavratura dry:** a sonda reproduziu o critério de inclusão e apontou `entra=False`, sem minutos; o estado do dry completo permanece **[não verificado]**. O comando sem `--apply` é dry, mas a sonda não executou o comando completo. (DADO RASPADO, linha `col830 dry_inclusao_dia`; anexo `laudo2_d_leitura.r`, seção 3; [lavrar_he_pendente.py:25](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/management/commands/lavrar_he_pendente.py:25); [he_pendente_lavrado.py:132](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/he_pendente_lavrado.py:132))

Se a Gestão enviar os **713 minutos** do retrato e as validações passarem, a porta gravará `DecisaoHE` autorizada com `minutos_na_decisao=713` e origem `admin`: ela recebe os minutos do formulário e não os recalcula. A lista vazia observada é a situação no momento da sonda; uma decisão vigente igual no momento do ato faria a porta retornar sem alterar a foto. ([gestao_he.py:270](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/gestao_he.py:270); [gestao_he.html:256](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/templates/ponto/gestao_he.html:256); [he.py:159](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/portas/he.py:159); [he.py:195](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/portas/he.py:195))

O motor carrega **data e estado**, não a foto de 713 minutos. A autorização pode alterar o cálculo quando as condições dos cortes ou do bloqueio de dia sem previsto se aplicam; **se ela mudaria o valor deste dia ou moveria dinheiro está [não verificado]** com o dado fornecido. ([motor_calculo_v2.py:1397](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1397); [motor_calculo_v2.py:1448](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1448); [motor_calculo_v2.py:1691](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1691); [motor_calculo_v2.py:1733](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1733))

**Dono L-099: [não verificado].** O material mostra a divergência entre retrato e espelho, mas não estabelece a hora da S6 nem a causa dessa divergência. (DADO RASPADO, linhas `col830 retrato_dia` e `col830 espelho_dia`; causa e hora da S6: [não verificado])

## 18/11 13:51 · sessao 266cc9c807c3 · LAUDO-[nome]-1811-1340 · sonda · origem=Code

A sonda terminou com os três marcadores esperados, `FIM_SONDA_ENVELOPE` e `rc_sombra=0`; não há traceback no dado fornecido. [dado raspado, linhas 11, 18, 25–27]

Para localizar as referências abaixo, conto as linhas a partir de `sonda_leitura` como linha 1, incluindo a linha em branco seguinte. [dado raspado, linhas 1–3]

A sonda recalculou o motor em leitura para a competência 11/2026 e agrupou os dias com minutos positivos de intrajornada e escala com `intervalo_indenizavel=False`, pela quantidade de batidas apuráveis. A flag `False`, por si só, não comprova que todos esses dias tenham intervalo **pre-assinalado**; a sonda não apresentou a modalidade da escala. [dado raspado, anexo `laudo2_c_leitura.r`: código de `linha_motor` e do censo; `pre-assinalado`: não verificado]

| Batidas apuráveis | Dias | Minutos do motor |
|---|---:|---:|
| 2 | 335 | 20.264,00 |
| 3 | 169 | 10.444,00 |
| 4 ou mais | 216 | 6.196,86 |
| **Subtotal pedido** | **720** | **36.904,86** |

Os valores da tabela são os emitidos pela sonda; o subtotal é a soma das três linhas. [dado raspado, linhas 6–8] A sonda encontrou ainda **1 dia com uma batida e 60,00 minutos**, nenhum com zero batidas, nenhum dia sem escala e zero erros de colaboradores; incluindo a classe de uma batida, são **721 dias e 36.964,86 minutos**. [dado raspado, linhas 4–10]

- **col868, 14/11:** confirmados 2 batidas apuráveis, 60 minutos no motor, flag `False` e 60 minutos em `DiaPago`. A pergunta consultada como 39550 retornou `mesma_pessoa=False`, com evento e célula em 03/10; portanto, não confirma associação dessa pergunta ao caso. [dado raspado, linhas 12–14; anexo `laudo2_c_leitura.r`, tupla de conferência]
- **col934, 15/11:** confirmados 2 batidas apuráveis, 60 minutos no motor, flag `False` e 60 minutos em `DiaPago`. A pergunta consultada como 39556 também retornou `mesma_pessoa=False`, com evento e célula em 03/10; portanto, não confirma associação dessa pergunta ao caso. [dado raspado, linhas 19–21; anexo `laudo2_c_leitura.r`, tupla de conferência]

O aval pede comparação com **151/152**, e a leitura anterior descreve 151 dias `False` em um recorte de 152 dias com marcação ímpar; o arquivo original `os152.r` e suas linhas não estão disponíveis para verificar esses números e filtros. [dado raspado, anexo `laudo_fernando_1811_1340.txt`, item 1(c); anexo `laudo2_c_leitura.r`, parágrafo sobre os152; não verificado] Sob essa descrição, o recorte ímpar deixava de fora os dias pares, incluindo a classe de **335 dias com duas batidas** medida agora; a classe `4+` mistura quantidades pares e ímpares, sem abertura suficiente para contar quantos ficaram fora. [dado raspado, linhas 6 e 8; não verificado: filtro original os152] Não é possível obter a interseção ou o acréscimo exato por subtração: a leitura anterior informa que os152 usava `DiaPago` gravado, enquanto esta sonda mede o motor recalculado. [dado raspado, anexo `laudo2_c_leitura.r`, parágrafo final; não verificado: execução e linhas originais de os152]

**Dono L-099: [não verificado].** O código declara que o motor não lê `intervalo_indenizavel` por decisão expressa; os resultados apresentados não demonstram, por si, uma divergência classificada pelo juiz de dono nem permitem atribuí-la a ESTRUTURA, CADASTRO ou BATIDA. [motor_calculo_v2.py:822](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:822), [CLAUDE.md:179](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/CLAUDE.md:179); [não verificado]

_6985 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
