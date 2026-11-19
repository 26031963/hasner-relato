# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 01:51** · publicado em **19/11/2026 01:51** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 01:51 · sessao 9e6c148debe0 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| chamados/management/commands/curar_perguntas_em_limbo.py:32 | nota_da_resposta | resposta_colab | PerguntaDisputa | v | `p.resposta_colab or ''` |
| chamados/management/commands/curar_perguntas_em_limbo.py:47 | handle | respondida_em | PerguntaDisputa | vi | `respondida_em__isnull=True` |
| chamados/management/commands/curar_perguntas_em_limbo.py:47 | handle | resposta_colab | PerguntaDisputa | vi | `exclude(resposta_colab='')` |
| chamados/management/commands/curar_perguntas_em_limbo.py:70 | handle | respondida_em | PerguntaDisputa | vi | `respondida_em__isnull=True` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:33 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:34 | handle | via_resolucao | PerguntaDisputa | vi | `exclude(via_resolucao=...` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:43 | handle | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao in SEM_HISTORIA` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:50 | handle | via_resolucao | PerguntaDisputa | v | `Counter(p.via_resolucao` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:67 | handle | via_resolucao | PerguntaDisputa | iv | `antes = p.via_resolucao` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:77 | handle | via_resolucao | PerguntaDisputa | iv | `depois={'via_resolucao': p.via_resolucao}` |
| chamados/management/commands/estoque_l130_instante_sem_texto.py:58 | handle | respondida_em | PerguntaDisputa | iv | `str(p.respondida_em)` |
| chamados/management/commands/estoque_l130_instante_sem_texto.py:58 | handle | resposta_colab | PerguntaDisputa | iv | `'resposta_colab': p.resposta_colab` |
| chamados/management/commands/estoque_l130_instante_sem_texto.py:59 | handle | via_resolucao | PerguntaDisputa | iv | `'via_resolucao': p.via_resolucao` |
| chamados/management/commands/migrar_data_evento_dia_do_turno.py:70 | _fonte_materializada | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao in (...)` |
| chamados/management/commands/migrar_data_evento_dia_do_turno.py:71 | _fonte_materializada | via_resolucao | PerguntaDisputa | v | `'absorcao (via=%s` |
| chamados/management/commands/migrar_data_evento_dia_do_turno.py:72 | _fonte_materializada | via_resolucao | PerguntaDisputa | v | `'sem_fonte (materializada` |
| chamados/management/commands/migrar_data_evento_dia_do_turno.py:147 | classificar | materializada_em | PerguntaDisputa | iv | `'materializada' if p.materializada_em else 'viva'` |
| chamados/management/commands/relavrar_via_i6.py:58 | planejar | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao not in VIAS_DERIVADAS` |
| chamados/management/commands/relavrar_via_i6.py:59 | planejar | via_resolucao | PerguntaDisputa | v | `_fora(p.via_resolucao, 'via de DECISAO` |
| chamados/management/commands/relavrar_via_i6.py:62 | planejar | via_resolucao | PerguntaDisputa | v | `_fora(p.via_resolucao, 'excluido` |
| chamados/management/commands/relavrar_via_i6.py:66 | planejar | via_resolucao | PerguntaDisputa | v | `_fora(p.via_resolucao, 'sem destino` |
| chamados/management/commands/relavrar_via_i6.py:68 | planejar | via_resolucao | PerguntaDisputa | ii | `nova != p.via_resolucao` |
| chamados/management/commands/relavrar_via_i6.py:88 | handle | via_resolucao | PerguntaDisputa | v | `pares[(p.via_resolucao, nova)]` |
| chamados/management/commands/validar_classe_a.py:63 | handle | resposta_colab | PerguntaDisputa **e** DecisaoPergunta/ato | iii | `DecisaoPergunta.leitura(q).resposta_colab != q.resposta_colab` |
| chamados/management/commands/validar_classe_a.py:83 | handle | resposta_colab | DecisaoPergunta/ato | — | `DecisaoPergunta.leitura(q).resposta_colab` |
| chamados/migrations/0053_pergunta_unica_por_familia.py:32 | _no_fato | via_resolucao | PerguntaDisputa | vi | `~Q(via_resolucao__in=VIAS_FORA_DO_FATO)` |
| chamados/models.py:1543 | _universo_da_ressuscitacao | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| chamados/models.py:1547 | _universo_da_ressuscitacao | via_resolucao | PerguntaDisputa | vi | `via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO` |
| chamados/models.py:1558 | _universo_da_ressuscitacao | respondida_em | PerguntaDisputa | ii | `p.respondida_em is None` |
| chamados/models.py:1558 | _universo_da_ressuscitacao | resposta_colab | PerguntaDisputa | ii | `not texto_vazio(p.resposta_colab)` |
| chamados/models.py:1558 | _universo_da_ressuscitacao | validada_em | PerguntaDisputa | ii | `p.validada_em is None` |
| chamados/models.py:1650 | ressuscitar_perguntas | materializada_em | PerguntaDisputa | vi | `filter(materializada_em__isnull=False)` |
| chamados/models.py:1651 | ressuscitar_perguntas | via_resolucao | PerguntaDisputa | vi | `exclude(via_resolucao__in=...` |
| chamados/models.py:1655 | ressuscitar_perguntas | respondida_em | PerguntaDisputa | vi | `only('pk', 'respondida_em'` |
| chamados/models.py:1655 | ressuscitar_perguntas | resposta_colab | PerguntaDisputa | vi | `'validada_por_id', 'resposta_colab'` |
| chamados/models.py:1655 | ressuscitar_perguntas | validada_em | PerguntaDisputa | vi | `'respondida_em', 'validada_em'` |
| chamados/models.py:1686 | ressuscitar_perguntas | via_resolucao | PerguntaDisputa | vi | `alvo.exclude(via_resolucao__in=...` |
| chamados/models.py:1692 | ressuscitar_perguntas | validada_em | DecisaoPergunta/ato | — | `d.ato_em == _lido.validada_em` |
| chamados/models.py:1696 | ressuscitar_perguntas | respondida_em | DecisaoPergunta/ato | — | `respondida_original_em=_lido.respondida_em` |
| chamados/models.py:1696 | ressuscitar_perguntas | resposta_colab | DecisaoPergunta/ato | — | `ato_texto=_lido.resposta_colab or ''` |
| chamados/models.py:1697 | ressuscitar_perguntas | validada_em | DecisaoPergunta/ato | — | `validada_original_em=_lido.validada_em` |
| chamados/models.py:1698 | ressuscitar_perguntas | validada_em | DecisaoPergunta/ato | — | `if _lido.validada_em is not None` |
| chamados/models.py:1702 | ressuscitar_perguntas | validada_em | PerguntaDisputa | ii | `descarimbar_em_lote(... validada_em=None` |
| chamados/models.py:1703 | ressuscitar_perguntas | respondida_em | PerguntaDisputa | ii | `respondida_em=None` |
| chamados/models.py:1703 | ressuscitar_perguntas | resposta_colab | PerguntaDisputa | ii | `resposta_colab=''` |

linhas: 45 de 45

Totais por classe: i = 0; ii = 10; iii = 1; iv = 6; v = 8; vi = 14. Seis linhas têm receptor de ato e não recebem classe; a linha de `validar_classe_a.py:63` compara ato e projeção.

perguntas de lei: 0. [app/docs/CORTES.md:545](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/CORTES.md:545); [app/docs/CORTES.md:560](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/CORTES.md:560); [app/docs/LEIS.md:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/LEIS.md:151).

## 19/11 01:51 · sessao 70b9c08a266b · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| api/views.py:1163 | api_chamados_v2 | resposta_colab | PerguntaDisputa | v | `p.resposta_colab` |
| api/views.py:1165 | api_chamados_v2 | respondida_em | PerguntaDisputa | v | `p.respondida_em.isoformat()` |
| api/views.py:2567 | api_disputa_perguntas._item | resposta_colab | PerguntaDisputa | v | `p.resposta_colab` |
| api/views.py:2569 | api_disputa_perguntas._item | respondida_em | PerguntaDisputa | v | `p.respondida_em.isoformat()` |
| api/views.py:2645 | api_disputa_responder | respondida_em | PerguntaDisputa | v | `p.respondida_em.isoformat()` |
| api/views_mensageria.py:569 | _item_validar | resposta_colab | PerguntaDisputa | v | `p.resposta_colab` |
| api/views_mensageria.py:571 | _item_validar | respondida_em | PerguntaDisputa | v | `p.respondida_em.isoformat()` |
| api/views_mensageria.py:572 | _item_validar | respondida_em | PerguntaDisputa | v | `(agora - p.respondida_em).days` |
| chamados/juizes.py:292 | _retratada_com_trilha | materializada_em | PerguntaDisputa | iii | `p.materializada_em is None` |
| chamados/juizes.py:352 | _slot_esperado | respondida_em | PerguntaDisputa | iv | `respondida is None and p.respondida_em is not None` |
| chamados/juizes.py:364 | _slot_esperado | resposta_colab | PerguntaDisputa | iv | `(p.resposta_colab or '').strip()` |
| chamados/juizes.py:683 | _pergunta_materializada_de_fato | materializada_em | PerguntaDisputa | iii | `p.materializada_em is not None` |
| chamados/juizes.py:683 | _pergunta_materializada_de_fato | via_resolucao | PerguntaDisputa | iii | `p.via_resolucao in VIAS_SEM_TESTEMUNHA` |
| chamados/juizes.py:695 | _pergunta_materializada_de_fato | via_resolucao | PerguntaDisputa | iii | `p.via_resolucao in _ABSOLVEM_SEMPRE` |
| chamados/juizes.py:711 | _pergunta_materializada_de_fato | materializada_em | PerguntaDisputa | iii | `return p.materializada_em is not None` |
| chamados/juizes.py:856 | _chave_memo_pv | respondida_em | PerguntaDisputa | iii | `getattr(p, 'respondida_em', None)` |
| chamados/juizes.py:856 | _chave_memo_pv | validada_em | PerguntaDisputa | iii | `getattr(p, 'validada_em', None)` |
| chamados/juizes.py:857 | _chave_memo_pv | resposta_colab | PerguntaDisputa | iii | `getattr(p, 'resposta_colab', None)` |
| chamados/juizes.py:857 | _chave_memo_pv | via_resolucao | PerguntaDisputa | iii | `getattr(p, 'via_resolucao', None)` |
| chamados/juizes.py:858 | _chave_memo_pv | materializada_em | PerguntaDisputa | iii | `getattr(p, 'materializada_em', None)` |
| chamados/juizes.py:1360 | _resposta_concorda_com_a_luz | resposta_colab | PerguntaDisputa | iv | `(p.resposta_colab or '').strip()` |
| chamados/juizes.py:1389 | projecao_temporal_da_pergunta | respondida_em | PerguntaDisputa | i | `respondida = ... p.respondida_em` |
| chamados/juizes.py:1389 | projecao_temporal_da_pergunta | resposta_colab | PerguntaDisputa | i | `resposta = ... p.resposta_colab` |
| chamados/juizes.py:1389 | projecao_temporal_da_pergunta | validada_em | PerguntaDisputa | i | `validada = ... p.validada_em` |
| chamados/juizes.py:1390 | projecao_temporal_da_pergunta | materializada_em | PerguntaDisputa | iv | `materializada, via = p.materializada_em, p.via_resolucao` |
| chamados/juizes.py:1390 | projecao_temporal_da_pergunta | via_resolucao | PerguntaDisputa | iv | `materializada, via = p.materializada_em, p.via_resolucao` |
| chamados/juizes.py:1423 | _pergunta_viva_calc | respondida_em | indeterminado: `SimpleNamespace` projetado | — | `SimpleNamespace(respondida_em=respondida` |
| chamados/juizes.py:1423 | _pergunta_viva_calc | resposta_colab | indeterminado: `SimpleNamespace` projetado | — | `resposta_colab=resposta)` |
| chamados/juizes.py:1502 | pergunta_sem_lastro_ou_viva | materializada_em | PerguntaDisputa | ii | `p.materializada_em is None or pergunta_viva(p)` |
| chamados/juizes.py:1534 | respondida_sem_texto | respondida_em | PerguntaDisputa; também sondador | iii | `p.respondida_em is not None` |
| chamados/juizes.py:1534 | respondida_sem_texto | resposta_colab | PerguntaDisputa; também sondador | iii | `texto_vazio(p.resposta_colab)` |
| chamados/juizes.py:1541 | texto_sem_carimbo | respondida_em | PerguntaDisputa; também sondador | iii | `p.respondida_em is None` |
| chamados/juizes.py:1541 | texto_sem_carimbo | resposta_colab | PerguntaDisputa; também sondador | iii | `not texto_vazio(p.resposta_colab)` |
| chamados/juizes.py:1552 | pergunta_respondida | respondida_em | PerguntaDisputa; também LeituraDoAto | i | `p.respondida_em is not None` |
| chamados/juizes.py:1552 | pergunta_respondida | resposta_colab | PerguntaDisputa; também LeituraDoAto | i | `not texto_vazio(p.resposta_colab)` |
| chamados/juizes.py:1572 | q_respondida_sem_texto | respondida_em | PerguntaDisputa | vi | `Q(respondida_em__isnull=False)` |
| chamados/juizes.py:1572 | q_respondida_sem_texto | resposta_colab | PerguntaDisputa | vi | `Q(resposta_colab__regex=_RE_SO_BRANCOS)` |
| chamados/juizes.py:1580 | q_respondida_com_texto | respondida_em | PerguntaDisputa | vi | `Q(respondida_em__isnull=False)` |
| chamados/juizes.py:1580 | q_respondida_com_texto | resposta_colab | PerguntaDisputa | vi | `Q(resposta_colab__regex=_RE_TEM_TEXTO)` |
| chamados/juizes.py:1591 | q_nao_respondida | respondida_em | PerguntaDisputa | vi | `Q(respondida_em__isnull=True)` |
| chamados/juizes.py:1765 | perguntas_respondidas_a_validar | respondida_em | PerguntaDisputa | vi | `.order_by('respondida_em')` |
| chamados/juizes.py:1791 | declarou_hora | resposta_colab | PerguntaDisputa | iv | `(pergunta.resposta_colab or '').strip()` |
| chamados/juizes.py:1938 | delta_da_resposta | resposta_colab | PerguntaDisputa | iv | `(perg.resposta_colab or '').strip()` |
| chamados/juizes.py:2042 | classificar_resposta_validacao | resposta_colab | PerguntaDisputa | iv | `(perg.resposta_colab or '').strip().lower()` |
| chamados/juizes.py:2047 | classificar_resposta_validacao | resposta_colab | PerguntaDisputa | iv | `'resp': perg.resposta_colab` |

linhas: 45 de 45.

Totais por classe: i **5**; ii **1**; iii **14**; iv **9**; v **8**; vi **6**; sem classe **2**.

- `juizes.py:1389` — respondida_em: chamada em `chamados/juizes.py:351` e `:1404`; alimenta o julgamento de vitalidade, que é usado como guarda de porta em `api/views.py:2622`.
- `juizes.py:1389` — resposta_colab: chamada em `chamados/juizes.py:351` e `:1404`; alimenta o julgamento de vitalidade usado na guarda em `api/views.py:2622`.
- `juizes.py:1389` — validada_em: chamada em `chamados/juizes.py:351` e `:1404`; alimenta o julgamento de vitalidade usado na guarda em `api/views.py:2622`.
- `juizes.py:1552` — respondida_em: entre os chamadores, `api/views.py:2628` passa o resultado à porta em `:2635`; `chamados/services/materializacao.py:1300` condiciona a guarda de aplicação.
- `juizes.py:1552` — resposta_colab: entre os chamadores, `api/views.py:2628` passa o resultado à porta em `:2635`; `chamados/services/materializacao.py:1300` condiciona a guarda de aplicação.

perguntas de lei: **0**.

## 19/11 01:51 · sessao b473b8de65f0 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| chamados/juizes.py:2064 | motivo_implausivel_validacao | resposta_colab | PerguntaDisputa | ii | `(perg.resposta_colab or '').strip()` |
| chamados/management/commands/auditar_disputas.py:71 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/auditar_disputas.py:150 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/auditar_invariantes_chamados.py:203 | _contraprova_f1t | via_resolucao | PerguntaDisputa | iii | `not (_p.via_resolucao or '').strip()` |
| chamados/management/commands/auditar_invariantes_chamados.py:206 | _contraprova_f1t | validada_em | DecisaoPergunta/ato | — | `DecisaoPergunta.leitura(_p).validada_em` |
| chamados/management/commands/auditar_invariantes_chamados.py:207 | _contraprova_f1t | via_resolucao | PerguntaDisputa | iii | `_p.via_resolucao in _IDENT` |
| chamados/management/commands/auditar_invariantes_chamados.py:434 | contraprova_piso | via_resolucao | PerguntaDisputa | iii | `por_via[p.via_resolucao]` |
| chamados/management/commands/auditar_invariantes_chamados.py:472 | universo_i6 | materializada_em | PerguntaDisputa | vi | `.values_list('pk', 'materializada_em')` |
| chamados/management/commands/auditar_invariantes_chamados.py:541 | _i6_pares | materializada_em | PerguntaDisputa | iii | `p.materializada_em is None or not PMF(p)` |
| chamados/management/commands/auditar_invariantes_chamados.py:545 | _i6_pares | via_resolucao | PerguntaDisputa | iii | `_i6.append((p.pk, p.via_resolucao))` |
| chamados/management/commands/auditar_invariantes_chamados.py:592 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/auditar_invariantes_chamados.py:911 | handle | materializada_em | PerguntaDisputa | iii | `_p8b.materializada_em is not None` |
| chamados/management/commands/auditar_invariantes_chamados.py:962 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| chamados/management/commands/auditar_invariantes_chamados.py:962 | handle | via_resolucao | PerguntaDisputa | vi | `via_resolucao=''` |
| chamados/management/commands/auditar_invariantes_chamados.py:1084 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/auditar_invariantes_chamados.py:1084 | handle | validada_em | PerguntaDisputa | vi | `validada_em__isnull=False` |
| chamados/management/commands/auditar_invariantes_chamados.py:1115 | handle | respondida_em | PerguntaDisputa | vi | `filter(respondida_em__isnull=True)` |
| chamados/management/commands/auditar_invariantes_chamados.py:1116 | handle | resposta_colab | PerguntaDisputa | vi | `resposta_colab=''` |
| chamados/management/commands/auditar_invariantes_chamados.py:1167 | handle | materializada_em | PerguntaDisputa | vi | `filter(materializada_em__isnull=False)` |
| chamados/management/commands/auditar_invariantes_chamados.py:1588 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/auditar_invariantes_chamados.py:1617 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/auditar_invariantes_chamados.py:1726 | handle | via_resolucao | PerguntaDisputa | iii | `(p.via_resolucao or '').strip() and not _viva(p)` |
| chamados/management/commands/auditar_invariantes_chamados.py:1783 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/management/commands/backfill_decisoes_pergunta.py:57 | linhas_a_criar | respondida_em | PerguntaDisputa | vi | `Q(respondida_em__isnull=False)` |
| chamados/management/commands/backfill_decisoes_pergunta.py:57 | linhas_a_criar | validada_em | PerguntaDisputa | vi | `Q(validada_em__isnull=False)` |
| chamados/management/commands/backfill_decisoes_pergunta.py:61 | linhas_a_criar | respondida_em | PerguntaDisputa | vi | `'respondida_em'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:61 | linhas_a_criar | validada_em | PerguntaDisputa | vi | `'validada_em'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:62 | linhas_a_criar | resposta_colab | PerguntaDisputa | vi | `'resposta_colab'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:105 | contador_validada_projetada_sem_linha | respondida_em | PerguntaDisputa | vi | `'respondida_em'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:105 | contador_validada_projetada_sem_linha | resposta_colab | PerguntaDisputa | vi | `'resposta_colab'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:105 | contador_validada_projetada_sem_linha | validada_em | PerguntaDisputa | vi | `'validada_em'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:128 | admin_decidiu_sem_ato | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| chamados/management/commands/backfill_decisoes_pergunta.py:128 | admin_decidiu_sem_ato | via_resolucao | PerguntaDisputa | vi | `via_resolucao=PerguntaDisputa.VIA_ADMIN_DECIDIU` |
| chamados/management/commands/backfill_decisoes_pergunta.py:129 | admin_decidiu_sem_ato | validada_em | PerguntaDisputa | vi | `validada_em__isnull=True` |
| chamados/management/commands/backfill_decisoes_pergunta.py:341 | aplicar | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao != PerguntaDisputa.VIA_ADMIN_DECIDIU` |
| chamados/management/commands/backfill_decisoes_pergunta.py:352 | aplicar | materializada_em | PerguntaDisputa | iv | `ato_em=p.materializada_em` |
| chamados/management/commands/backfill_decisoes_pergunta.py:353 | aplicar | resposta_colab | PerguntaDisputa | iv | `ato_texto=p.resposta_colab or ''` |
| chamados/management/commands/backfill_decisoes_pergunta.py:357 | aplicar | via_resolucao | PerguntaDisputa | iv | `via_antes = {p.pk: p.via_resolucao for p in escreve}` |
| chamados/management/commands/backfill_decisoes_pergunta.py:425 | _diff_do_grupo | via_resolucao | PerguntaDisputa | vi | `.values_list('pk', 'via_resolucao')` |
| chamados/management/commands/backfill_decisoes_pergunta.py:531 | handle | respondida_em | PerguntaDisputa | vi | `'respondida_em'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:531 | handle | resposta_colab | PerguntaDisputa | vi | `'resposta_colab'` em `.only()` |
| chamados/management/commands/backfill_decisoes_pergunta.py:531 | handle | via_resolucao | PerguntaDisputa | vi | `'via_resolucao'` em `.only()` |
| chamados/management/commands/bug_par_103.py:67 | handle | materializada_em | PerguntaDisputa | v | `'materializada_em': str(p.materializada_em)` |
| chamados/management/commands/bug_par_103.py:67 | handle | via_resolucao | PerguntaDisputa | v | `'via_resolucao': p.via_resolucao` |
| chamados/management/commands/bug_par_103.py:69 | handle | validada_em | PerguntaDisputa | v | `p.validada_em and str(p.validada_em)` |

linhas: 45 de 45

Totais por classe: i **0**; ii **2**; iii **7**; iv **3**; v **3**; vi **29**. Uma leitura é de ato, sem classe: chamados/management/commands/auditar_invariantes_chamados.py:206.

perguntas de lei: 0

## 19/11 01:51 · sessao b7be1208f6f3 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| chamados/models.py:2005 | sem_veredito_entre | materializada_em | PerguntaDisputa | iv | `q.materializada_em is None` |
| chamados/models.py:2005 | sem_veredito_entre | validada_em | DecisaoPergunta/ato | — | `_lido.validada_em is None` |
| chamados/models.py:2082 | fechar | via_resolucao | PerguntaDisputa | v | `_via_antes = _q.via_resolucao` |
| chamados/models.py:2088 | fechar | materializada_em | PerguntaDisputa | iv | `ato_em=_q.materializada_em` |
| chamados/models.py:2089 | fechar | resposta_colab | DecisaoPergunta/ato | — | `leitura(_q).resposta_colab` |
| chamados/models.py:2218 | remover_motivo | via_resolucao | PerguntaDisputa | vi | `exclude(via_resolucao=...)` |
| chamados/models.py:2221 | remover_motivo | resposta_colab | DecisaoPergunta/ato | — | `leitura(_q).resposta_colab` |
| chamados/models.py:2223 | remover_motivo | via_resolucao | PerguntaDisputa | v | `_q.via_resolucao or '-'` |
| chamados/models.py:2911 | do_fato | via_resolucao | PerguntaDisputa | vi | `exclude(via_resolucao__in=...)` |
| chamados/models.py:2942 | q_dos_fatos | via_resolucao | PerguntaDisputa | vi | `~models.Q(via_resolucao__in=...)` |
| chamados/models.py:3236 | estado | validada_em | DecisaoPergunta/ato | — | `_lido.validada_em is not None` |
| chamados/models.py:3238 | estado | materializada_em | PerguntaDisputa | iv | `self.materializada_em is not None` |
| chamados/models.py:3242 | estado | materializada_em | PerguntaDisputa | iv | `self.materializada_em is not None` |
| chamados/models.py:3251 | fechar_por_admin | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao == self.VIA_ADMIN_DECIDIU` |
| chamados/models.py:3252 | fechar_por_admin | materializada_em | PerguntaDisputa | ii | `self.materializada_em is not None` |
| chamados/models.py:3322 | carimbar | materializada_em | PerguntaDisputa | ii | `antes = (self.materializada_em, ...` |
| chamados/models.py:3322 | carimbar | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao` |
| chamados/models.py:3323 | carimbar | respondida_em | PerguntaDisputa | ii | `self.respondida_em` |
| chamados/models.py:3323 | carimbar | validada_em | PerguntaDisputa | ii | `self.validada_em` |
| chamados/models.py:3326 | carimbar | resposta_colab | PerguntaDisputa | ii | `not texto_vazio(self.resposta_colab)` |
| chamados/models.py:3327 | carimbar | respondida_em | PerguntaDisputa | ii | `self.respondida_em is None` |
| chamados/models.py:3330 | carimbar | validada_em | PerguntaDisputa | ii | `self.validada_em is None` |
| chamados/models.py:3378 | carimbar_identidade | materializada_em | PerguntaDisputa | ii | `manter_carimbo and self.materializada_em is not None` |
| chamados/models.py:3379 | carimbar_identidade | materializada_em | PerguntaDisputa | ii | `_escrever_carimbo(self.materializada_em, ...` |
| chamados/models.py:3397 | fora_do_fato | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao in self.VIAS_FORA_DO_FATO` |
| chamados/models.py:3468 | descarimbar_em_lote | resposta_colab | PerguntaDisputa | vi | `qs.values_list('resposta_colab', flat=True)` |
| chamados/models.py:3471 | descarimbar_em_lote | respondida_em | PerguntaDisputa | vi | `qs.values_list('respondida_em', flat=True)` |
| chamados/models.py:3480 | descarimbar_em_lote | via_resolucao | PerguntaDisputa | vi | `exclude(via_resolucao__in=...)` |
| chamados/models.py:3486 | descarimbar_em_lote | via_resolucao | PerguntaDisputa | vi | `exclude(via_resolucao__in=...)` |
| chamados/models.py:3494 | encerrar_por_fato_no_espelho | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao == self.VIA_FATO_MATERIALIZADO` |
| chamados/models.py:3505 | encerrar_fantasma | materializada_em | PerguntaDisputa | ii | `self.materializada_em is not None` |
| chamados/models.py:3505 | encerrar_fantasma | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao == self.VIA_FANTASMA` |
| chamados/models.py:3526 | reabrir_por_resposta | materializada_em | PerguntaDisputa | ii | `self.materializada_em is None` |
| chamados/models.py:3526 | reabrir_por_resposta | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao not in VIAS_SEM_LASTRO` |
| chamados/models.py:3533 | reabrir_por_competencia | via_resolucao | PerguntaDisputa | ii | `self.via_resolucao != self.VIA_COMPETENCIA_TRANCADA` |
| chamados/models.py:3560 | q_validada_pelo_ato | validada_em | PerguntaDisputa | vi | `Q(validada_em__isnull=False)` |
| chamados/models.py:3599 | q_vivas | materializada_em | PerguntaDisputa | vi | `Q(materializada_em__isnull=False)` |
| chamados/models.py:3599 | q_vivas | via_resolucao | PerguntaDisputa | vi | `Q(via_resolucao__in=_IDENT)` |
| chamados/models.py:3601 | q_vivas | materializada_em | PerguntaDisputa | vi | `Q(materializada_em__isnull=False)` |
| chamados/models.py:3601 | q_vivas | via_resolucao | PerguntaDisputa | vi | `Q(via_resolucao__in=cls.VIAS_QUE_VENCEM_A_RESPOSTA)` |
| chamados/models.py:3706 | _recusa_par_ilegal | respondida_em | PerguntaDisputa | vi | `values_list('respondida_em', 'resposta_colab')` |
| chamados/models.py:3706 | _recusa_par_ilegal | resposta_colab | PerguntaDisputa | vi | `values_list('respondida_em', 'resposta_colab')` |
| chamados/models.py:3707 | _recusa_par_ilegal | respondida_em | PerguntaDisputa | ii | `self.respondida_em if ... else _banco[0]` |
| chamados/models.py:3708 | _recusa_par_ilegal | resposta_colab | PerguntaDisputa | ii | `self.resposta_colab if ... else _banco[1]` |

linhas: 44 de 44

Totais por classe: i **0**; ii **20**; iii **0**; iv **4**; v **2**; vi **14**. As outras **4** leituras têm receptor DecisaoPergunta/ato e não recebem classe. [chamados/models.py:2004](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2004), [chamados/models.py:2089](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2089), [chamados/models.py:2221](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2221), [chamados/models.py:3235](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3235).

perguntas de lei: 0

## 19/11 01:51 · sessao be0f05997da0 · E4C-F5 · censo · origem=Code

| arquivo:linha | função | campo | receptor | classe | trecho literal curto |
|---|---|---|---|---|---|
| chamados/models.py:3774 | `<modulo>` | via_resolucao | PerguntaDisputa | vi | `~models.Q(via_resolucao__in=VIAS_FORA_DO_FATO)` |
| chamados/models.py:3780 | `<modulo>` | via_resolucao | PerguntaDisputa | vi | `~models.Q(via_resolucao__in=VIAS_FORA_DO_FATO)` |
| chamados/models.py:3815 | resposta_display | resposta_colab | outro modelo: LeituraDoAto | — | `DecisaoPergunta.leitura(self).resposta_colab` |
| chamados/models.py:4251 | linhas_da_projecao_sem_linha | respondida_em | PerguntaDisputa | ii | `p.respondida_em is not None` |
| chamados/models.py:4251 | linhas_da_projecao_sem_linha | validada_em | PerguntaDisputa | ii | `p.validada_em is not None` |
| chamados/models.py:4260 | linhas_da_projecao_sem_linha | respondida_em | PerguntaDisputa | ii | `p.respondida_em is not None` |
| chamados/models.py:4261 | linhas_da_projecao_sem_linha | respondida_em | PerguntaDisputa | ii | `ja.add((p.pk, cls.RESPOSTA, p.respondida_em))` |
| chamados/models.py:4262 | linhas_da_projecao_sem_linha | respondida_em | PerguntaDisputa | ii | `ato_em=p.respondida_em` |
| chamados/models.py:4263 | linhas_da_projecao_sem_linha | resposta_colab | PerguntaDisputa | ii | `ato_texto=p.resposta_colab or ''` |
| chamados/models.py:4265 | linhas_da_projecao_sem_linha | validada_em | PerguntaDisputa | ii | `p.validada_em is not None` |
| chamados/models.py:4266 | linhas_da_projecao_sem_linha | validada_em | PerguntaDisputa | ii | `ja.add((p.pk, cls.VALIDACAO, p.validada_em))` |
| chamados/models.py:4267 | linhas_da_projecao_sem_linha | validada_em | PerguntaDisputa | ii | `ato_em=p.validada_em` |
| chamados/models.py:4268 | linhas_da_projecao_sem_linha | resposta_colab | PerguntaDisputa | ii | `ato_texto=p.resposta_colab or ''` |
| chamados/reconciliador.py:368 | lavrar_sem_turno_aberto | materializada_em | PerguntaDisputa | ii | `if p.materializada_em is not None` |
| chamados/reconciliador.py:673 | qs_perguntas_absolviveis | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/reconciliador.py:674 | qs_perguntas_absolviveis | validada_em | PerguntaDisputa | vi | `validada_em__isnull=True` |
| chamados/reconciliador.py:1093 | _orfas_abertas | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/reconciliador.py:1093 | _orfas_abertas | validada_em | PerguntaDisputa | vi | `validada_em__isnull=True` |
| chamados/reconciliador.py:1188 | encerrar_orfas_pela_lampada | via_resolucao | PerguntaDisputa | iv | `'via_resolucao': p.via_resolucao or ''` |
| chamados/reconciliador.py:1189 | encerrar_orfas_pela_lampada | materializada_em | PerguntaDisputa | iv | `p.materializada_em.isoformat()` |
| chamados/reconciliador.py:1199 | encerrar_orfas_pela_lampada | materializada_em | PerguntaDisputa | iv | `'materializada_em': p.materializada_em.isoformat()` |
| chamados/reconciliador.py:1199 | encerrar_orfas_pela_lampada | via_resolucao | PerguntaDisputa | iv | `'via_resolucao': p.via_resolucao` |
| chamados/reconciliador.py:1549 | perguntas_orfas_reatavies | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/services/acoes_disputa.py:349 | acao_corrigir_resposta | validada_em | PerguntaDisputa | i | `if perg.validada_em` |
| chamados/services/acoes_disputa.py:357 | acao_corrigir_resposta | resposta_colab | PerguntaDisputa | v | `antes = (perg.resposta_colab or '').strip()` |
| chamados/services/competencia_trancada.py:276 | reabrir | via_resolucao | PerguntaDisputa | vi | `via_resolucao=VIA` |
| chamados/services/declaracao_texto.py:228 | _resposta_validada | validada_em | PerguntaDisputa | i | `return perg.validada_em is not None` |
| chamados/services/declaracao_texto.py:418 | declarar_hora_colab | resposta_colab | PerguntaDisputa | ii | `(perg.resposta_colab or '') == hora` |
| chamados/services/disputa_emissao.py:228 | _derivar_chamado_destino | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True).exists()` |
| chamados/services/disputa_emissao.py:337 | gerar_perguntas_disputa | via_resolucao | PerguntaDisputa | vi | `via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO` |
| chamados/services/disputa_emissao.py:409 | gerar_perguntas_disputa | via_resolucao | PerguntaDisputa | vi | `via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO` |
| chamados/services/disputa_emissao.py:428 | gerar_perguntas_disputa | via_resolucao | PerguntaDisputa | vi | `via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO` |
| chamados/services/disputa_emissao.py:446 | gerar_perguntas_disputa | via_resolucao | PerguntaDisputa | vi | `via_resolucao__in=PerguntaDisputa.VIAS_FORA_DO_FATO` |
| chamados/services/disputa_emissao.py:776 | motivo_fora_da_fabrica | materializada_em | PerguntaDisputa | vi | `filter(materializada_em__isnull=False).exists()` |
| chamados/services/disputa_emissao.py:1084 | criar_disputa_a_partir_de_chamados_orfaos | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/services/disputa_emissao.py:1104 | criar_disputa_a_partir_de_chamados_orfaos | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/services/disputa_emissao.py:1340 | criar_disputa_ancora_de_chamado | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| chamados/services/disputa_emissao.py:1484 | responder_pergunta_disputa_core | validada_em | PerguntaDisputa | ii | `_validada_antes = (p.validada_em, p.validada_por_id)` |
| chamados/services/disputa_emissao.py:1485 | responder_pergunta_disputa_core | validada_em | PerguntaDisputa | ii | `if p.validada_em is not None` |

linhas: 39 de 39

Totais: i = 2; ii = 14; iii = 0; iv = 4; v = 1; vi = 17; outro receptor = 1.

- `acoes_disputa.py:349`: chamada por `chamados/views.py:536` e `chamados/services/fio.py:686`; **sim**, a guarda impede a correção antes da chamada à porta escritora em `acoes_disputa.py:375`.
- `declaracao_texto.py:228`: chamada em `declaracao_texto.py:286`, `:379` e `:669`; **sim**, as duas últimas guardam as portas que chamam o escritor em `:420` e `:694`; `:286` consulta a condição para prever a recusa.

perguntas de lei: 0 — L-130 e os cortes F5 constam em `app/docs/LEIS.md:151` e `app/docs/CORTES.md:560–561`.

## 19/11 01:50 · sessao 301527d19ae3 · E4C-F3-LAVRA-FOTO · construtor · origem=Code

Atualizei o detector em [test_f3_lavra_foto_sem_decisao.py](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:41): ele percorre chamadas transitivas a funções do módulo, reconhece reexportações e acessos dinâmicos, e verifica caminhos ORM e SQL usando os relacionamentos lidos do modelo por AST. [teste:16](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:16) [nao verificado]

A sintaxe foi verificada por parse AST. **Os testes não foram executados**, conforme pedido; resultado dos controles e mordida dos mutantes: **[não verificado]**. [nao verificado]

Para executar cada mutante separadamente, no diretório `app`:

```bash
for teste in \ [nao verificado]
  test_MORDE_m7_helper_transitivo \
  test_MORDE_m8_reexportacao \
  test_MORDE_m9_getattr_dinamico \
  test_MORDE_m10_prefetch \
  test_MORDE_m11_lookup_dict \
  test_MORDE_m12_sql
do
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "chamados.tests.test_f3_lavra_foto_sem_decisao.LavraFotoSemDecisaoTest.$teste" [nao verificado]
done
```

Testes novos: `test_related_names_do_modelo_nao_vazios`, `test_MORDE_m7_helper_transitivo`, `test_MORDE_m8_reexportacao`, `test_MORDE_m9_getattr_dinamico`, `test_MORDE_m10_prefetch`, `test_MORDE_m11_lookup_dict`, `test_MORDE_m12_sql` e `test_controle_helper_nao_chamado_nao_conta`. [teste:212](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:212) [nao verificado]

## 19/11 01:50 · sessao ab0b5e747455 · - · - · origem=Ronald (tmux)

Classificação enviada ao agente principal, com receptores, classes e trechos das 20 linhas de `auditar_invariantes_chamados.py`.

## 19/11 01:50 · sessao ea3987f5fad5 · - · - · origem=Ronald (tmux)

Classificação do backfill enviada ao agente principal, com receptores, trechos e referências de linha. Nenhuma das linhas examinadas foi classificada como classe i.

## 19/11 01:47 · sessao e5128e733da5 · E4C-F5 · censo · origem=Code

O censo deste lote **não está fechado**. A transcrição do anexo contém **139 linhas pela minha contagem manual [não verificado]**; não concluí a classificação linha a linha nem a reconciliação dos totais. Portanto, os totais por receptor e por classe e a igualdade `contador == universo` permanecem **[não verificado]**.

A conferência já mostra que o nome do campo, sozinho, não identifica o receptor. Por exemplo, `_lido.validada_em` em `ressuscitar_perguntas` vem de `DecisaoPergunta.leitura(_q)`, enquanto `p.validada_em` em `linhas_da_projecao_sem_linha` vem da projeção da pergunta. ([models.py:1688](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1688), [models.py:4238](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4238))

As linhas abaixo foram conferidas diretamente. Os números entre parênteses são as classes solicitadas; `ato` indica `LeituraDoAto`, que tem os três campos declarados em [models.py:17](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:17).

| Arquivo:linha | Função | Campo | Receptor | Classe | Trecho literal curto |
|---|---|---|---|---|---|
| [api/views.py:1163](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/api/views.py:1163) | `api_chamados_v2` | `resposta_colab` | PerguntaDisputa | (v) exibição | `'resposta_colab': p.resposta_colab` |
| [api/views.py:1165](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/api/views.py:1165) | `api_chamados_v2` | `respondida_em` | PerguntaDisputa | (v) exibição | `'respondida_em': p.respondida_em.isoformat()` |
| [api/views.py:2567](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/api/views.py:2567) | `api_disputa_perguntas._item` | `resposta_colab` | PerguntaDisputa | (v) exibição | `'resposta_colab': p.resposta_colab` |
| [api/views.py:2569](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/api/views.py:2569) | `api_disputa_perguntas._item` | `respondida_em` | PerguntaDisputa | (v) exibição | `'respondida_em': p.respondida_em.isoformat()` |
| [api/views.py:2645](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/api/views.py:2645) | `api_disputa_responder` | `respondida_em` | PerguntaDisputa | (v) resposta da API | `'respondida_em': p.respondida_em.isoformat()` |
| [auditar_disputas.py:71](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/auditar_disputas.py:71) | `handle` | `materializada_em` | PerguntaDisputa | (vi) filtro em lote | `materializada_em__isnull=True` |
| [auditar_disputas.py:150](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/auditar_disputas.py:150) | `handle` | `materializada_em` | PerguntaDisputa | (vi) filtro em lote | `materializada_em__isnull=True` |
| [curar_perguntas_em_limbo.py:47](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/curar_perguntas_em_limbo.py:47) | `handle` | `respondida_em` | PerguntaDisputa | (iii) reparo da projeção declarado | `ALLOW-iii` |
| [curar_perguntas_em_limbo.py:47](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/curar_perguntas_em_limbo.py:47) | `handle` | `resposta_colab` | PerguntaDisputa | (iii) reparo da projeção declarado | `exclude(resposta_colab='')` |
| [models.py:1558](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1558) | `_universo_da_ressuscitacao` | `respondida_em` | PerguntaDisputa | (ii) conservação do passivo | `p.respondida_em is None` |
| [models.py:1558](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1558) | `_universo_da_ressuscitacao` | `validada_em` | PerguntaDisputa | (ii) conservação do passivo | `p.validada_em is None` |
| [models.py:1558](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1558) | `_universo_da_ressuscitacao` | `resposta_colab` | PerguntaDisputa | (ii) conservação do passivo | `not texto_vazio(p.resposta_colab)` |
| [models.py:1692](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1692) | `ressuscitar_perguntas` | `validada_em` | DecisaoPergunta/ato | — | `d.ato_em == _lido.validada_em` |
| [models.py:1696](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1696) | `ressuscitar_perguntas` | `respondida_em` | DecisaoPergunta/ato | — | `respondida_original_em=_lido.respondida_em` |
| [models.py:1696](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1696) | `ressuscitar_perguntas` | `resposta_colab` | DecisaoPergunta/ato | — | `ato_texto=_lido.resposta_colab` |
| [models.py:1697](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1697) | `ressuscitar_perguntas` | `validada_em` | DecisaoPergunta/ato | — | `validada_original_em=_lido.validada_em` |
| [models.py:1698](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1698) | `ressuscitar_perguntas` | `validada_em` | DecisaoPergunta/ato | — | `if _lido.validada_em is not None` |
| [models.py:2005](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2005) | `sem_veredito_entre` | `validada_em` | DecisaoPergunta/ato | — | `_lido.validada_em is None` |
| [models.py:2005](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2005) | `sem_veredito_entre` | `materializada_em` | PerguntaDisputa | (i) julgamento de estado | `q.materializada_em is None` |
| [models.py:3236](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3236) | `estado` | `validada_em` | DecisaoPergunta/ato | — | `_lido.validada_em is not None` |
| [models.py:3295](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3295) | `_escrever_carimbo` | `materializada_em` | PerguntaDisputa | (ii) escritor | `self.materializada_em = quando` |
| [models.py:3297](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3297) | `_escrever_carimbo` | `via_resolucao` | PerguntaDisputa | (ii) escritor | `self.via_resolucao = via` |
| [acoes_disputa.py:349](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/acoes_disputa.py:349) | `acao_corrigir_resposta` | `validada_em` | PerguntaDisputa | (i) julga validação | `if perg.validada_em` |
| [acoes_disputa.py:357](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/acoes_disputa.py:357) | `acao_corrigir_resposta` | `resposta_colab` | PerguntaDisputa | (iv) fotografia anterior à escrita | `antes = (perg.resposta_colab or '')` |
| [fio.py:188](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:188) | `espera_o_texto_do_colaborador` | `respondida_em` | PerguntaDisputa | (i) julga resposta | `p.respondida_em is None` |
| [fio.py:1709](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1709) | `recusa_par_ilegal` | `respondida_em` | outro objeto: `SimpleNamespace` | — | `SimpleNamespace(respondida_em=respondida_em` |
| [fio.py:1709](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1709) | `recusa_par_ilegal` | `resposta_colab` | outro objeto: `SimpleNamespace` | — | `resposta_colab=resposta_colab)` |
| [regularizacao_ext.py:199](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/regularizacao_ext.py:199) | `materializar_regularizacao_externa` | `materializada_em` | outro modelo: RegularizacaoExterna | — | `reg.materializada_em = _tz.now()` |
| [regularizacao_ext.py:220](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/regularizacao_ext.py:220) | `retomar_regularizacoes_confirmadas` | `materializada_em` | outro modelo: RegularizacaoExterna | — | `materializada_em__isnull=True` |
| [veredito_lavrado.py:203](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:203) | `marcos_do_lote` | `materializada_em` | PerguntaDisputa | (iv) marco da lavra | `materializada_em__isnull=False` |
| [veredito_lavrado.py:207](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:207) | `marcos_do_lote` | `respondida_em` | PerguntaDisputa | (iv) marco da lavra | `respondida_em__isnull=False` |

A classificação de `materializada_em` em `sem_veredito_entre` como (i) identifica sua participação no julgamento; esse campo **não existe em `LeituraDoAto`**, portanto a linha não autoriza substituí-lo por `leitura.materializada_em`. ([models.py:17](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:17), [models.py:1996](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1996))

Para as linhas da classe (i) acima, a conferência de chamadores também está incompleta:

- `acao_corrigir_resposta` é chamada em [views.py:536](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/views.py:536) e [fio.py:686](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:686). **Sim:** a guarda de validação retorna antes de chamar `responder_pergunta_disputa_core` e `validar_pergunta`, podendo impedir essas escritas. ([acoes_disputa.py:349](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/acoes_disputa.py:349), [acoes_disputa.py:375](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/acoes_disputa.py:375), [acoes_disputa.py:383](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/acoes_disputa.py:383))
- `espera_o_texto_do_colaborador` é chamada por `aguarda_o_colaborador`, `montar_chamados_fio` e `_calados`. O efeito do resultado sobre o momento de uma escrita de porta existente permanece **[não verificado]**. ([fio.py:175](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:175), [fio.py:360](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:360), [cobranca.py:241](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/cobranca.py:241))
- Chamadores e efeito de `sem_veredito_entre` sobre quando uma porta escreve: **[não verificado]**.

Há uma questão de lei apontada pelo próprio código sobre a trilha de **texto legado sem instante e sem validação**, que hoje fica fora do universo de ressuscitação para evitar perda silenciosa. O conjunto de opções e se os documentos a respondem integralmente permanecem **[não verificado]**; não há base para fixar o total de perguntas do lote. ([models.py:1553](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1553))

perguntas de lei: [não verificado]

## 19/11 01:47 · sessao 50ddc07b8c74 · E4C-F5 · censo · origem=Code

O censo do lote **não está fechado**. Consegui consolidar com segurança as 39 ocorrências abaixo; as demais ocorrências, os totais do lote inteiro e a igualdade entre contador e universo permanecem **[não verificado]**. Os caminhos são relativos a `app/`. O texto da coluna final reproduz um fragmento da própria linha citada.

Nas 35 primeiras linhas, o receptor é `PerguntaDisputa`, inclusive o modelo histórico da migração. As classes são: **ii** conservação do passivo ou porta; **iv** fotografia da lavra; **v** exibição; **vi** face SQL. ([backfill_decisoes_pergunta.py:44](app/chamados/management/commands/backfill_decisoes_pergunta.py:44), [encerrar_perguntas_com_fato.py:27](app/chamados/management/commands/encerrar_perguntas_com_fato.py:27), [relavrar_via_i6.py:44](app/chamados/management/commands/relavrar_via_i6.py:44), [0053_pergunta_unica_por_familia.py:37](app/chamados/migrations/0053_pergunta_unica_por_familia.py:37), [reconciliar_fantasmas.py:210](app/ponto/management/commands/reconciliar_fantasmas.py:210), [flip_auto.py:125](app/ponto/services/flip_auto.py:125))

| Arquivo:linha | Função | Campo | Receptor | Classe | Trecho literal |
|---|---|---|---|---|---|
| chamados/management/commands/backfill_decisoes_pergunta.py:57 | linhas_a_criar | respondida_em | PerguntaDisputa | vi | `Q(respondida_em__isnull=False)` |
| chamados/management/commands/backfill_decisoes_pergunta.py:57 | linhas_a_criar | validada_em | PerguntaDisputa | vi | `Q(validada_em__isnull=False)` |
| chamados/management/commands/backfill_decisoes_pergunta.py:61 | linhas_a_criar | respondida_em | PerguntaDisputa | vi | `.only(... 'respondida_em'` |
| chamados/management/commands/backfill_decisoes_pergunta.py:61 | linhas_a_criar | validada_em | PerguntaDisputa | vi | `'validada_em'` |
| chamados/management/commands/backfill_decisoes_pergunta.py:62 | linhas_a_criar | resposta_colab | PerguntaDisputa | vi | `'resposta_colab'` |
| chamados/management/commands/backfill_decisoes_pergunta.py:105 | contador_validada_projetada_sem_linha | respondida_em | PerguntaDisputa | vi | `qs.only(... 'respondida_em'` |
| chamados/management/commands/backfill_decisoes_pergunta.py:105 | contador_validada_projetada_sem_linha | resposta_colab | PerguntaDisputa | vi | `'resposta_colab'` |
| chamados/management/commands/backfill_decisoes_pergunta.py:105 | contador_validada_projetada_sem_linha | validada_em | PerguntaDisputa | vi | `'validada_em'` |
| chamados/management/commands/backfill_decisoes_pergunta.py:128 | admin_decidiu_sem_ato | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| chamados/management/commands/backfill_decisoes_pergunta.py:128 | admin_decidiu_sem_ato | via_resolucao | PerguntaDisputa | vi | `via_resolucao=PerguntaDisputa.VIA_ADMIN_DECIDIU` |
| chamados/management/commands/backfill_decisoes_pergunta.py:129 | admin_decidiu_sem_ato | validada_em | PerguntaDisputa | vi | `validada_em__isnull=True` |
| chamados/management/commands/backfill_decisoes_pergunta.py:341 | aplicar | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao != PerguntaDisputa.VIA_ADMIN_DECIDIU` |
| chamados/management/commands/backfill_decisoes_pergunta.py:352 | aplicar | materializada_em | PerguntaDisputa | ii | `ato_em=p.materializada_em` |
| chamados/management/commands/backfill_decisoes_pergunta.py:353 | aplicar | resposta_colab | PerguntaDisputa | ii | `ato_texto=p.resposta_colab or ''` |
| chamados/management/commands/backfill_decisoes_pergunta.py:357 | aplicar | via_resolucao | PerguntaDisputa | iv | `via_antes = {p.pk: p.via_resolucao` |
| chamados/management/commands/backfill_decisoes_pergunta.py:425 | _diff_do_grupo | via_resolucao | PerguntaDisputa | vi | `values_list('pk', 'via_resolucao')` |
| chamados/management/commands/backfill_decisoes_pergunta.py:531 | handle | respondida_em | PerguntaDisputa | vi | `.only(... 'respondida_em'` |
| chamados/management/commands/backfill_decisoes_pergunta.py:531 | handle | resposta_colab | PerguntaDisputa | vi | `.only(... 'resposta_colab'` |
| chamados/management/commands/backfill_decisoes_pergunta.py:531 | handle | via_resolucao | PerguntaDisputa | vi | `'via_resolucao'` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:33 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=False` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:34 | handle | via_resolucao | PerguntaDisputa | vi | `.exclude(via_resolucao=...)` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:43 | handle | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao in SEM_HISTORIA` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:50 | handle | via_resolucao | PerguntaDisputa | v | `Counter(p.via_resolucao` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:67 | handle | via_resolucao | PerguntaDisputa | iv | `antes = p.via_resolucao or ''` |
| chamados/management/commands/encerrar_perguntas_com_fato.py:77 | handle | via_resolucao | PerguntaDisputa | iv | `depois={'via_resolucao': p.via_resolucao}` |
| chamados/management/commands/relavrar_via_i6.py:58 | planejar | via_resolucao | PerguntaDisputa | ii | `p.via_resolucao not in VIAS_DERIVADAS` |
| chamados/management/commands/relavrar_via_i6.py:59 | planejar | via_resolucao | PerguntaDisputa | ii | `_fora(p.via_resolucao` |
| chamados/management/commands/relavrar_via_i6.py:62 | planejar | via_resolucao | PerguntaDisputa | ii | `_fora(p.via_resolucao, 'excluido por ordem')` |
| chamados/management/commands/relavrar_via_i6.py:66 | planejar | via_resolucao | PerguntaDisputa | ii | `_fora(p.via_resolucao, 'sem destino: nao julgavel')` |
| chamados/management/commands/relavrar_via_i6.py:68 | planejar | via_resolucao | PerguntaDisputa | ii | `nova != p.via_resolucao` |
| chamados/management/commands/relavrar_via_i6.py:88 | handle | via_resolucao | PerguntaDisputa | v | `pares[(p.via_resolucao, nova)]` |
| chamados/migrations/0053_pergunta_unica_por_familia.py:32 | _no_fato | via_resolucao | PerguntaDisputa histórico | vi | `Q(via_resolucao__in=VIAS_FORA_DO_FATO)` |
| ponto/management/commands/reconciliar_fantasmas.py:213 | handle | materializada_em | PerguntaDisputa | vi | `materializada_em__isnull=True` |
| ponto/services/flip_auto.py:129 | fila_flip_recusas | via_resolucao | PerguntaDisputa | vi | `via_resolucao=PerguntaDisputa.VIA_PARIDADE_RECUSADA` |
| ponto/services/flip_auto.py:130 | fila_flip_recusas | validada_em | PerguntaDisputa | vi | `validada_em__gte=corte` |

Também confirmei estas quatro ocorrências da **classe i**, cujo julgamento de validação pede atenção à `LeituraDoAto`. ([models.py:17](app/chamados/models.py:17), [declaracao_texto.py:217](app/chamados/services/declaracao_texto.py:217), [materializacao.py:194](app/chamados/services/materializacao.py:194), [materializacao.py:2117](app/chamados/services/materializacao.py:2117))

| Arquivo:linha | Função | Campo | Receptor | Classe | Trecho literal |
|---|---|---|---|---|---|
| chamados/services/declaracao_texto.py:228 | _resposta_validada | validada_em | PerguntaDisputa | i | `perg.validada_em is not None` |
| chamados/services/materializacao.py:199 | par_do_intervalo | validada_em | PerguntaDisputa | i | `irma_validada and q.validada_em is None` |
| chamados/services/materializacao.py:201 | par_do_intervalo | validada_em | PerguntaDisputa | i | `q.validada_em is None` |
| chamados/services/materializacao.py:2123 | materializar_perguntas_validadas_da_disputa | validada_em | PerguntaDisputa | i | `p.validada_em is None` |

Para essas quatro ocorrências, os chamadores de produção identificados a um nível são:

- `_resposta_validada`: `declaracao_texto.py:286`, `:379` e `:669`. Nas duas portas de declaração, o resultado participa de uma guarda que pode impedir o avanço até a escrita. ([declaracao_texto.py:286](app/chamados/services/declaracao_texto.py:286), [declaracao_texto.py:379](app/chamados/services/declaracao_texto.py:379), [declaracao_texto.py:669](app/chamados/services/declaracao_texto.py:669))
- `par_do_intervalo`: `validacao.py:632`, `materializacao.py:923` e `materializacao.py:1732`. Nos caminhos de validação do par e de gravação da irmã, o resultado pode impedir a carimbagem ou a materialização; o efeito da chamada em `:923` sobre o momento de uma escrita é **[não verificado]**. ([validacao.py:632](app/chamados/services/validacao.py:632), [materializacao.py:923](app/chamados/services/materializacao.py:923), [materializacao.py:1732](app/chamados/services/materializacao.py:1732))
- `materializar_perguntas_validadas_da_disputa`: `fio.py:718`, `acoes_disputa.py:143`, `validacao.py:786` e `backfill_fio_morto_e4b.py:147`. O teste da linha 2123 pode impedir a chamada seguinte a `_materializar_uma`. ([fio.py:718](app/chamados/services/fio.py:718), [acoes_disputa.py:143](app/chamados/services/acoes_disputa.py:143), [validacao.py:786](app/chamados/services/validacao.py:786), [backfill_fio_morto_e4b.py:147](app/chamados/management/commands/backfill_fio_morto_e4b.py:147), [materializacao.py:2123](app/chamados/services/materializacao.py:2123))

**Contagem apenas das linhas apresentadas:** 39 receptores `PerguntaDisputa`, dos quais um é o modelo histórico; classe i = 4, ii = 9, iii = 0, iv = 3, v = 2 e vi = 21. Soma = 39. Isso **não é o contador do universo do lote**, que permanece **[não verificado]**.

Há pelo menos uma ocorrência adicional que não deve ser contada como leitura direta de `PerguntaDisputa`: em `tarefa_do_dia.py:163`, o receptor é `leitura.validada_em`, já proveniente do ato. A classificação e a contagem de todas as demais ocorrências ainda precisam ser consolidadas. ([tarefa_do_dia.py:159](app/chamados/services/tarefa_do_dia.py:159))

perguntas de lei: **[não verificado]** — a revisão incompleta do lote e das leis não permite determinar N nem apresentar opções exaustivas.

## 19/11 01:46 · sessao 33d88961b2b8 · E4C-F5 · censo · origem=Code

**O censo do lote 3 permanece incompleto.** Conferi os sítios abaixo, mas não classifiquei todas as linhas do anexo nem fechei o contador do universo. Os totais por receptor, por classe e a igualdade contador == universo permanecem **[não verificado]**.

| Arquivo:linha | Função | Campo | Receptor | Classe | Trecho literal curto |
|---|---|---|---|---|---|
| [auditar_invariantes_chamados.py:203](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/auditar_invariantes_chamados.py:203) | `_contraprova_f1t` | `via_resolucao` | PerguntaDisputa | (iii) | `not (_p.via_resolucao or '').strip()` |
| [auditar_invariantes_chamados.py:206](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/auditar_invariantes_chamados.py:206) | `_contraprova_f1t` | `validada_em` | DecisaoPergunta/ato: `LeituraDoAto` | não se aplica | `DecisaoPergunta.leitura(_p).validada_em` |
| [auditar_invariantes_chamados.py:207](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/auditar_invariantes_chamados.py:207) | `_contraprova_f1t` | `via_resolucao` | PerguntaDisputa | (iii) | `_p.via_resolucao in _IDENT` |
| [auditar_invariantes_chamados.py:472](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/auditar_invariantes_chamados.py:472) | `universo_i6` | `materializada_em` | PerguntaDisputa | (vi) | `.values_list('pk', 'materializada_em')` |
| [auditar_invariantes_chamados.py:1115](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/auditar_invariantes_chamados.py:1115) | `handle` | `respondida_em` | PerguntaDisputa | (iii) | `filter(respondida_em__isnull=True)` |
| [auditar_invariantes_chamados.py:1116](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/auditar_invariantes_chamados.py:1116) | `handle` | `resposta_colab` | PerguntaDisputa | (iii) | `exclude(resposta_colab='')` |
| [estoque_l130_instante_sem_texto.py:58](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:58) | `handle` | `respondida_em` | PerguntaDisputa | (iv) | `str(p.respondida_em)` |
| [estoque_l130_instante_sem_texto.py:58](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:58) | `handle` | `resposta_colab` | PerguntaDisputa | (iv) | `'resposta_colab': p.resposta_colab` |
| [estoque_l130_instante_sem_texto.py:59](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:59) | `handle` | `via_resolucao` | PerguntaDisputa | (iv) | `'via_resolucao': p.via_resolucao` |
| [competencia_trancada.py:276](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/competencia_trancada.py:276) | `reabrir` | `via_resolucao` | PerguntaDisputa | (vi) | `PerguntaDisputa.objects.filter(..., via_resolucao=VIA` |
| [disputa_emissao.py:1481](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1481) | `responder_pergunta_disputa_core` | `resposta_colab` | PerguntaDisputa | (ii) | `p.resposta_colab = resposta` |
| [disputa_emissao.py:1482](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1482) | `responder_pergunta_disputa_core` | `respondida_em` | PerguntaDisputa | (ii) | `p.respondida_em = _agora` |
| [disputa_emissao.py:1484](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1484) | `responder_pergunta_disputa_core` | `validada_em` | PerguntaDisputa | (ii) | `_validada_antes = (p.validada_em, ...)` |
| [disputa_emissao.py:1485](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1485) | `responder_pergunta_disputa_core` | `validada_em` | PerguntaDisputa | (ii) | `if p.validada_em is not None` |
| [disputa_emissao.py:1494](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1494) | `responder_pergunta_disputa_core` | `validada_em` | PerguntaDisputa | (ii) | `p.validada_em = None` |
| [disputa_emissao.py:1528](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1528) | `responder_pergunta_disputa_core` | `respondida_em` | PerguntaDisputa | (ii) | `p.respondida_em = _agora` |
| [validacao.py:392](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:392) | `impedimento_estoque_l130` | `validada_em` | PerguntaDisputa | (i) | `if perg.validada_em is not None` |
| [validacao.py:640](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:640) | `_validar_o_par` | `validada_em` | PerguntaDisputa | (i) | `if q.validada_em is None` |
| [validacao.py:685](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:685) | `validar_pergunta` | `validada_em` | PerguntaDisputa | (i) | `if perg.validada_em` |
| [validacao.py:898](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:898) | `recusar_pergunta` | `validada_em` | PerguntaDisputa | (i) | `if perg.validada_em` |
| [validacao.py:1064](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:1064) | `validar_lote_classe_a` | `validada_em` | PerguntaDisputa | (i) | `if perg.validada_em` |
| [verbos.py:264](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/verbos.py:264) | `placar_equipe` | `validada_em` | PerguntaDisputa | (vi) | `validada_em__gte=d7` |
| [verbos.py:270](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/verbos.py:270) | `placar_equipe` | `validada_em` | PerguntaDisputa | (v) | `if pq.validada_em >= hoje_ini` |
| [procedencia_batida.py:74](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/services/procedencia_batida.py:74) | `procedencia` | `materializada_em` | PerguntaDisputa | (v) | `p.validada_em or p.materializada_em` |
| [procedencia_batida.py:74](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/services/procedencia_batida.py:74) | `procedencia` | `validada_em` | PerguntaDisputa | (v) | `p.validada_em or p.materializada_em` |

A classificação (iii) nas linhas 1115–1116 segue a finalidade declarada de medir meio-estado da própria projeção. A fotografia das linhas 58–59 integra o arquivo de reversão do comando. ([auditar_invariantes_chamados.py:1113](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/auditar_invariantes_chamados.py:1113), [estoque_l130_instante_sem_texto.py:26](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:26))

Dos sítios **(i) acima**, os vínculos de chamada e escrita que consegui conferir são:

- `impedimento_estoque_l130`: chamada por `devolver_instante_sem_texto_l130`; um impedimento faz a porta retornar antes de limpar os campos. O comando de estoque também a chama para selecionar e reconferir elegíveis antes de executar a devolução. ([validacao.py:432](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:432), [validacao.py:440](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:440), [estoque_l130_instante_sem_texto.py:42](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:42), [estoque_l130_instante_sem_texto.py:99](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/estoque_l130_instante_sem_texto.py:99))
- `_validar_o_par`: chamada por `validar_pergunta`; a guarda decide se chama `_carimbar_validacao(q, autor)`. ([validacao.py:771](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:771), [validacao.py:640](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:640))
- `validar_pergunta`: chamadas comprovadas incluem `acoes_disputa` e `aplicar_declaracao`; a guarda retorna `ja_validada` antes das escritas posteriores da porta. Lista completa de chamadores: **[não verificado]**. ([acoes_disputa.py:19](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/acoes_disputa.py:19), [declaracao_texto.py:697](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:697), [validacao.py:685](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:685))
- `recusar_pergunta`: chamada por `acoes_disputa`; a guarda retorna `ja_validada` antes de salvar o motivo e chamar a porta de reabertura. ([acoes_disputa.py:82](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/acoes_disputa.py:82), [validacao.py:898](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:898), [validacao.py:920](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:920))
- `validar_lote_classe_a`: chamada por `views_cobrar`; a guarda pula a pergunta já validada antes de chamar a ação de validação. ([views_cobrar.py:263](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/views_cobrar.py:263), [views_cobrar.py:383](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/views_cobrar.py:383), [validacao.py:1064](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:1064), [validacao.py:1078](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:1078))

Os demais sítios do lote, seus receptores, classes e chamadores permanecem **[não verificado]**. Também não concluí o confronto com `LEIS.md` e `CORTES.md`, necessário para enumerar perguntas de lei e opções.

perguntas de lei: [não verificado]

## 19/11 01:45 · sessao 778b831b7b28 · - · - · origem=Ronald (tmux)

A análise das ocorrências de `materializacao.py`, incluindo as classificações, os trechos e os chamadores comprovados, foi enviada ao agente responsável pela consolidação. A leitura da cópia foi apenas em memória; não houve escrita em arquivos. [não verificado]

## 19/11 01:45 · sessao 3b84bf5df64f · - · - · origem=Ronald (tmux)

Classificação do subconjunto de comandos, migração e serviços enviada ao agente principal: **35 linhas**, todas com receptor `PerguntaDisputa`; classes **(ii) 9, (iv) 3, (v) 2 e (vi) 21**. As evidências e os trechos foram enviados junto à classificação. ([backfill_decisoes_pergunta.py:44](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/backfill_decisoes_pergunta.py:44), [encerrar_perguntas_com_fato.py:27](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/encerrar_perguntas_com_fato.py:27), [relavrar_via_i6.py:44](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/relavrar_via_i6.py:44), [0053_pergunta_unica_por_familia.py:31](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/migrations/0053_pergunta_unica_por_familia.py:31), [reconciliar_fantasmas.py:210](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/management/commands/reconciliar_fantasmas.py:210), [flip_auto.py:116](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/services/flip_auto.py:116))

## 19/11 01:45 · sessao 85eb35a46c3f · - · - · origem=Ronald (tmux)

Resultados e ressalvas enviados ao agente principal, com referências ao código e aos chamadores.

## 19/11 01:45 · sessao c61a08d6dac7 · E4C-F3-LAVRA-FOTO · verificador · origem=Code

**BLOQUEIA — o selo morde os mutantes declarados, mas não garante a ausência de leituras de `DecisaoPergunta`.** A busca percorre apenas o AST do corpo de `marcos_do_lote` e reconhece um conjunto específico de nomes, atributos, palavras-chave e strings; ela não acompanha chamadas nem resolve todas as formas de acesso ao modelo. [app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:14](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:14)

**1. Mordida e lacunas**

Os testes M1–M4 apresentam construções que o detector reconhece: nome ou import direto, apelido direto, atributo `DecisaoPergunta` e string literal contendo esse nome. M5, M6, função aninhada e lookup `decisoes__...` também têm padrões reconhecidos. Isso é uma conclusão por inspeção; execução dos testes e logs RED/controle: **[não verificado]**. [app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:88](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:88)

As formas abaixo escapam. A lista descreve classes de acesso, não uma enumeração finita de todos os programas Python possíveis. Em cada linha, a cobertura indicada é trabalho necessário, não uma proteção existente verificada.

| Forma que escaparia | Alcançável no corpo atual? | Quem cobriria |
|---|---|---|
| Chamar helper do módulo ou importado que faz a leitura; o detector não entra no corpo do helper. | Não há chamada a helper próprio com essa leitura no corpo visível; comportamento interno de bibliotecas chamadas: **[não verificado]**. | Selo com análise transitiva dos chamados ou contrato separado que vigie o helper. [teste:39](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:39), [produção:151](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/veredito_lavrado.py:151) |
| Importar no módulo outro símbolo que já represente o modelo, por exemplo reexportação `from outro_modulo import DP`, e usar `DP.objects`; só o import direto de `chamados.models` registra apelido. O conjunto `modulos` é preenchido, mas não é consultado. | Esse import e uso não aparecem hoje no corpo ou imports mostrados. | Resolução de símbolos e reexportações. [teste:24](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:24), [produção:88](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/veredito_lavrado.py:88) |
| `getattr` ou `get_model` com nome montado, por exemplo `'Decisao' + 'Pergunta'`; aliases obtidos por atribuição ou acesso dinâmico também podem escapar. | Não há essa construção no corpo atual. | Análise de fluxo e avaliação conservadora de strings; acesso dinâmico arbitrário exige política explícita. [teste:46](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:46), [produção:151](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/veredito_lavrado.py:151) |
| `prefetch_related('decisoes')`, lookup passado como string ou via `**dict`, e consultas pelo outro relacionamento `absorcoes_feitas`. A string `decisoes` não contém `DecisaoPergunta`, e esse segundo relacionamento não consta nos atributos procurados. | Nenhuma dessas construções aparece no corpo atual. | Detector de caminhos ORM em strings e argumentos dinâmicos, considerando os relacionamentos reais do modelo. [teste:52](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:52), [modelo:4176](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/models.py:4176) |
| `Subquery`/`OuterRef` ou consulta sobre outro modelo que alcance a tabela por caminho de relacionamento em string; SQL bruto ou abstração que consulte a tabela também pode escapar. | Não aparecem essas construções no corpo atual. A execução interna do ORM: **[não verificado]**. | Análise dos caminhos ORM e SQL, ou restrição verificável às formas permitidas de consulta. [produção:177](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/veredito_lavrado.py:177), [teste:59](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:59) |

`select_related('decisoes')` não é, por si, um exemplo comprovado de leitura válida desse relacionamento reverso de múltiplas linhas; `prefetch_related('decisoes')` é a forma relevante aqui. Validação em execução: **[não verificado]**. [modelo:4166](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/models.py:4166)

**2. Valores assumidos pelo selo**

- **NÃO BLOQUEIA — relacionamento:** `decisoes` confere: é o `related_name` de `DecisaoPergunta.pergunta`, FK para `PerguntaDisputa`. O modelo também declara `absorcoes_feitas` para outra FK à pergunta, não vigiada pelo selo. [app/chamados/models.py:4166](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/models.py:4166), [app/chamados/models.py:4176](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/models.py:4176)
- **NÃO BLOQUEIA — escrita e leitura:** o modelo declara `registrar` como porta de escrita; há chamada concreta no backfill e leitura concreta por `self.decisoes.all()` em `PerguntaDisputa`. Censo exaustivo de todos os escritores e leitores: **[não verificado]**. [app/chamados/models.py:4141](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/models.py:4141), [backfill_decisoes_pergunta.py:351](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/management/commands/backfill_decisoes_pergunta.py:351), [app/chamados/models.py:3801](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/models.py:3801)
- **NÃO BLOQUEIA — nome e caminho:** o teste forma o caminho para `chamados/veredito_lavrado.py` e procura exatamente uma função de topo chamada `marcos_do_lote`; ambos conferem nesta cópia. O nome e o caminho são declarações de código, não valores escritos em banco. [teste:8](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:8), [teste:17](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:17), [produção:151](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/veredito_lavrado.py:151)

**3. Classes de reprovação**

- **BLOQUEIA — selo/RED com mordida parcial:** os mutantes exercitam padrões que o detector reconhece, mas as leituras indiretas e por caminhos ORM acima permanecem sem cobertura demonstrada. É o mesmo bloqueio contado na seção 1. [teste:46](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:46)
- **NÃO BLOQUEIA — demais classes na obra apresentada:** o diff fornecido contém apenas um arquivo de teste; nele não identifiquei novo juiz de domínio, escritor de produção, leitura de relógio, derivação paralela ou campo de produção com dois significados. A ausência dessas classes no restante do sistema é **[não verificada]**. [teste:1](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:1)

**4. Dependências e falsos verdes**

- **NÃO BLOQUEIA — relógio, banco e ordem:** por inspeção, os testes leem o arquivo, fazem parse e analisam strings em memória; não invocam `marcos_do_lote`, banco nem relógio. Cada mutante parte de `self.fonte`, sem depender de alteração feita por teste anterior. Execução da suíte: **[não verificado]**. [teste:75](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:75)
- **NÃO BLOQUEIA — vacuidade e âncoras nesta cópia:** erro de parse levanta exceção; função ausente ou duplicada também. As três âncoras ocorrem uma vez, e a âncora de corpo está dentro do alvo. A lista vazia pode, contudo, ser falso verde para as formas de leitura que o detector não reconhece, já contadas no bloqueio. [teste:16](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:16), [teste:70](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:70), [produção:160](/home/ronald/codex_ws/141757fc8bd7b8b1008458478683d71a0cc5787c/app/chamados/veredito_lavrado.py:160)

A cópia acessível não possui metadados Git no diretório consultado; identidade do HEAD com o hash informado: **[não verificado]**.

BLOQUEIA: 1  
perguntas de lei: 0

## 19/11 01:44 · sessao 0f3fd24bbc1d · CADEIA-20 · sonda · origem=Code

**1. Marcador da pausa.** A informação que distingue os caminhos existe nas condições do pareador; o resultado comum `_intra_dur=True` não registra qual delas foi satisfeita.

- No caminho **S→E**, a próxima batida é lida como `tn` e o intervalo em segundos como `gap`: `tn = (getattr(nx, 'tipo', '') or '').upper()[:1]` e `gap = (nx.timestamp - b.timestamp).total_seconds()` ([turnos.py:433](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:433)). A distinção existe na condição `tn == 'E' and 0 <= gap <= intra_max_s` ([turnos.py:453](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:453)); a origem deixa de ser distinguível pelo marcador quando ambas as pontas recebem `b._intra_dur = True` e `nx._intra_dur = True` ([turnos.py:456](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:456)). Há outro caminho S→E sem bordas, com condição de S posterior e o mesmo marcador ([turnos.py:407](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:407)), além do caminho com marcos em [turnos.py:1468](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1468).
- No caminho **S,S**, a informação existe em `if tn == 'S' and (ds is None or ds > 90):` ([turnos.py:476](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:476)). Ao encontrar a S posterior próxima da borda, a primeira recebe o mesmo `b._intra_dur = True` ([turnos.py:484](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:484)).
- O laço também usa esse atributo para papel vindo da ata e para absorção por marco: `b._intra_dur = True` ([turnos.py:1316](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1316), [turnos.py:1364](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1364)). O consumo reúne as origens em `_iv = getattr(b, "is_intervalo", False) or getattr(b, "_intra_dur", False)` ([turnos.py:1326](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1326)).

A batida gravada tem `tipo` e `is_intervalo` ([models.py:23](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/models.py:23)); a célula grava `ata` em JSON ([models.py:1774](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/models.py:1774)). O período do motor expõe `intervalos_do_motor`, isto é, intervalos já julgados, e o adaptador os entrega como pares de instantes ([motor_calculo_v2.py:239](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/motor_calculo_v2.py:239), [adaptador_motor.py:13](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/adaptador_motor.py:13)). **Não foi verificado um campo gravado que registre qual daqueles dois ramos do pareador produziu `_intra_dur`**; os tipos e dados da ata podem conter fatos relacionados, mas não provam por si o caminho executado. [não verificado]

**2. ATA_DESALINHADA.** No anexo `sonda3.py`, o ramo é literalmente `if resultado.get("desalinhado"):` seguido de `nao_medida[(emp, "ATA_DESALINHADA")] += 1` e `continue`; o número da linha do anexo está [não verificado]. A condição vem de `periodos_do_dia(cel, None)` e conta uma ocorrência por célula cuja ata é um `dict` e cujo resultado tem `desalinhado` verdadeiro. [não verificado: linha do anexo]

O motor retorna esse valor em quatro causas exclusivas pela precedência do código: sem marcos esperados após a tentativa posicional; sem lâmpadas, se há marcos; quantidade diferente, se ambos existem; ou discordância de horário de um marco, após passar pelas guardas anteriores ([juiz_batida.py:190](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/juiz_batida.py:190), [juiz_batida.py:195](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/juiz_batida.py:195), [juiz_batida.py:227](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/juiz_batida.py:227)). Os contadores por causa e exemplos ainda são [não verificado].

A sonda abaixo usa o mesmo recorte e guarda do anexo, classifica pelo `porque` devolvido pelo próprio motor e compara a soma por causa com o universo contado diretamente pelo ramo. Cada exemplo é uma célula; a hora é a primeira batida apurável **na data local da célula**, quando existir. A ausência de batida é explicitada. A execução e seus resultados são [não verificado].

```python
# Executar pelo mesmo envelope de leitura da sombra, tenant juliani.
# Não cria, altera ou apaga arquivos. Não imprime nome nem CPF.

import collections
import datetime
import os
import sys

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from escala.models import CelulaDia
from ponto.juiz_batida import periodos_do_dia
from ponto.turnos import batidas_apuraveis


INI = datetime.date(2026, 10, 21)
FIM = datetime.date(2026, 11, 20)
UM_DIA = datetime.timedelta(days=1)

CAUSAS = (
    "SEM_MARCOS",
    "SEM_LAMPADAS",
    "QUANTIDADE_DIFERENTE",
    "HORARIO_DIVERGENTE",
)


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


def hora_valida(hora):
    if not isinstance(hora, str) or len(hora) != 5 or hora[2] != ":":
        return False
    try:
        return datetime.datetime.strptime(hora, "%H:%M").strftime("%H:%M") == hora
    except ValueError:
        return False


def causa_do_resultado(resultado):
    porque = resultado.get("porque")
    if porque == "sem marcos no dna":
        return "SEM_MARCOS"
    if porque == "sem lampadas na ata":
        return "SEM_LAMPADAS"
    if isinstance(porque, str) and porque.startswith("ata com "):
        return "QUANTIDADE_DIFERENTE"
    if isinstance(porque, str) and porque.startswith("marco "):
        return "HORARIO_DIVERGENTE"
    raise RuntimeError("causa_desconhecida")


def main():
    etapa = "GUARDA_SOMBRA"
    try:
        with transaction.atomic():
            guarda_sombra()

            etapa = "CELULAS"
            celulas = list(
                CelulaDia.objects.filter(data__gte=INI, data__lte=FIM)
                .select_related("colaborador", "colaborador__empresa")
                .order_by("colaborador_id", "data")
            )
            por_pessoa = collections.defaultdict(list)
            empresas = set()
            for cel in celulas:
                por_pessoa[cel.colaborador_id].append(cel)
                empresas.add(cel.colaborador.empresa_id)

            universo = collections.Counter()
            contagem = collections.Counter()
            # Primeiro guarda exemplos com hora; depois completa com células
            # sem batida no dia, se necessário.
            com_hora = collections.defaultdict(list)
            sem_hora = collections.defaultdict(list)

            for cid, cels in sorted(por_pessoa.items()):
                colab = cels[0].colaborador
                emp = colab.empresa_id
                classificadas = []

                for cel in cels:
                    etapa = "ATA"
                    if not isinstance(cel.ata, dict):
                        continue

                    orfas = cel.ata.get("orfas")
                    if orfas is not None and not isinstance(orfas, list):
                        raise RuntimeError("orfas_formato")
                    if orfas is not None:
                        for hora in orfas:
                            if not hora_valida(hora):
                                raise RuntimeError("hora_orfa")

                    etapa = "INVERSA_ATA"
                    resultado = periodos_do_dia(cel, None)
                    if not resultado.get("desalinhado"):
                        continue

                    # Mesmo ponto de contagem da sonda3, antes da partição.
                    universo[emp] += 1
                    causa = causa_do_resultado(resultado)
                    contagem[(emp, causa)] += 1
                    classificadas.append((cel.data, causa))

                if not classificadas:
                    continue

                # Só procura horas enquanto ainda faltam exemplos com hora
                # para alguma causa desta pessoa.
                precisa_hora = any(
                    len(com_hora[causa]) < 2
                    for _, causa in classificadas
                )
                por_dia = collections.defaultdict(list)
                if precisa_hora:
                    etapa = "BATIDAS_EXEMPLOS"
                    lo = timezone.make_aware(
                        datetime.datetime.combine(INI, datetime.time.min)
                    )
                    hi = timezone.make_aware(
                        datetime.datetime.combine(FIM + UM_DIA, datetime.time.min)
                    )
                    for batida in batidas_apuraveis(colab, lo, hi):
                        local = timezone.localtime(batida.timestamp)
                        if INI <= local.date() <= FIM:
                            por_dia[local.date()].append(local)

                for dia, causa in classificadas:
                    if len(com_hora[causa]) >= 2:
                        continue
                    instantes = por_dia.get(dia, [])
                    if instantes:
                        hora = min(instantes).strftime("%H:%M")
                        com_hora[causa].append((cid, dia, hora))
                    elif len(sem_hora[causa]) < 2:
                        sem_hora[causa].append((cid, dia, None))

            etapa = "SAIDA"
            linhas = []
            soma_geral = 0
            universo_geral = 0

            for emp in sorted(empresas, key=lambda valor: str(valor)):
                soma = sum(contagem[(emp, causa)] for causa in CAUSAS)
                total = universo[emp]
                soma_geral += soma
                universo_geral += total
                for causa in CAUSAS:
                    linhas.append(
                        "CAUSA emp=%s causa=%s ocorrencias=%d"
                        % (emp, causa, contagem[(emp, causa)])
                    )
                linhas.append(
                    "CONFERENCIA emp=%s contador=%d universo=%d contador==universo=%s"
                    % (emp, soma, total, "s" if soma == total else "n")
                )
                if soma != total:
                    raise RuntimeError("soma_empresa")

            linhas.append(
                "CONFERENCIA_TOTAL contador=%d universo=%d contador==universo=%s"
                % (
                    soma_geral,
                    universo_geral,
                    "s" if soma_geral == universo_geral else "n",
                )
            )
            if soma_geral != universo_geral:
                raise RuntimeError("soma_total")

            # Referência da saída fornecida; pode divergir se a sombra mudou.
            linhas.append(
                "REFERENCIA_SONDA3 universo=6454 atual=%d confere=%s"
                % (universo_geral, "s" if universo_geral == 6454 else "n")
            )

            for causa in CAUSAS:
                exemplos = (com_hora[causa] + sem_hora[causa])[:2]
                for numero, (cid, dia, hora) in enumerate(exemplos, 1):
                    linhas.append(
                        "EXEMPLO causa=%s numero=%d col%d dia_celula=%s hora_local=%s"
                        % (
                            causa,
                            numero,
                            cid,
                            dia.strftime("%d/%m/%Y"),
                            hora if hora is not None else "sem_batida_no_dia",
                        )
                    )
                for numero in range(len(exemplos) + 1, 3):
                    linhas.append(
                        "EXEMPLO causa=%s numero=%d indisponivel"
                        % (causa, numero)
                    )

        for linha in linhas:
            print(linha)
        print("ERROS=0")
        print("FIM_SONDA_PASSO2")
        return 0

    except Exception:
        print("ERRO " + etapa)
        print("ERROS=1")
        print("FIM_SONDA_PASSO2")
        return 1


sys.exit(main())
```

perguntas de lei: 0

## 19/11 01:44 · sessao 7975ea5f169d · QUEIMA-B4 · sonda · origem=Code

**A. Código e limite da identificação**

O caminho histórico de entrada das 14 continua **[não verificado]**. A classe atual informada para elas é `b_conversa`; isso, por si só, não identifica a porta que atuou no passado. [não verificado: saída histórica independente não fornecida; app/chamados/services/validacao.py:1067]

Na cópia, as portas abaixo compartilham o materializador. A assinatura permite reconhecer **compatibilidade com um ramo**, mas não distinguir todas as portas que o chamaram:

| Caminho | Assinatura e trilha gravada |
|---|---|
| Resposta `hora`: `_materializar_resposta_hora` | Ao criar: `origem='disputa_s84_retro'`, `aprovada=True`, `aprovada_por=request_user`, `aprovada_em=now()`; depois vincula `pergunta_origem` e tenta log `materializar_batida`, com `batida_pk` e `pergunta_pk`. Não informa justificativa à criação. [app/chamados/services/materializacao.py:1487] [app/chamados/services/materializacao.py:1542] |
| Resposta `data_hora`: `_materializar_resposta_data_hora` | `origem='disputa_ancora'`, tipo `E`, `sequencia=1`, justificativa sobre âncora, `aprovada=True`, `aprovada_por=request_user`, `aprovada_em=now()`; tenta log `materializar_ancora`, com `batida_pk` e `pergunta_pk`. Esse trecho não atribui `pergunta_origem`. [app/chamados/services/materializacao.py:1604] [app/chamados/services/materializacao.py:1637] |
| Validação individual e par | A validação grava `validada_em`, `validada_por` e decisão `ato='validacao'`; para hora, chama `_materializar_uma` ou o caminho do par, e depois a fila da disputa. A assinatura da batida é a do ramo acima; não há, nesses campos, marca exclusiva de validação individual. [app/chamados/services/validacao.py:161] [app/chamados/services/validacao.py:769] |
| Lote da tela/chat | Reclassifica cada pergunta e chama `acao_validar_pergunta` para classe A. Compartilha a trilha da validação; uma assinatura exclusiva do lote **[não verificado]**. [app/chamados/services/validacao.py:1027] [app/chamados/services/validacao.py:1067] |
| Comando `validar_classe_a` | Seleciona pela classificação usada pelo comando e chama `validar_pergunta(q.pk, autor, disputa=d)`. Compartilha a trilha de validação; assinatura exclusiva do comando **[não verificado]**. [app/chamados/management/commands/validar_classe_a.py:60] [app/chamados/management/commands/validar_classe_a.py:97] |
| Regularização da disputa, inclusive pelo fio | Ambas chamam `materializar_perguntas_validadas_da_disputa` com o usuário da requisição; a primeira registra também `regularizar_disputa_granular` na disputa. Compartilham a assinatura do ramo materializado. [app/chamados/services/acoes_disputa.py:121] [app/chamados/services/acoes_disputa.py:143] [app/chamados/services/fio.py:711] |
| Passivo validado e comando `materializar_passivo_validado` | O reparo chama `_materializar_uma` com `user`; o comando chama `reparar` com o autor. Compartilha a assinatura do ramo materializado; marca individual exclusiva de criação pelo reparo **[não verificado]**. [app/chamados/services/passivo_validado.py:529] [app/chamados/services/materializacao.py:1846] [app/chamados/management/commands/materializar_passivo_validado.py:113] |
| `backfill_fio_morto_e4b` | No ato `materializar`, chama a mesma fila da disputa com `user`; no ato separado `registrar_decisao`, registra linhas de projeção. Uma linha de decisão, portanto, não prova sozinha qual porta criou a batida. [app/chamados/management/commands/backfill_fio_morto_e4b.py:138] [app/chamados/management/commands/backfill_fio_morto_e4b.py:144] |
| Relavra de via e carimbo de pendentes aprovadas | `relavrar` altera `via_resolucao` e registra `relavra_via_i6`; `materializar_pendentes_aprovadas` carimba perguntas aprovadas e declara que não fabrica batida. São modificadores de trilha, não uma assinatura de criação de batida nesses trechos. [app/chamados/services/relavra_via.py:106] [app/chamados/services/relavra_via.py:125] [app/chamados/services/materializacao.py:2139] |

Trechos literais decisivos:

- Criação de hora: `disputa.colaborador, ts, _tipo, 'disputa_s84_retro'` e `aprovada_por=request_user`. [app/chamados/services/materializacao.py:1488]
- Criação de âncora: `disputa.colaborador, ts, 'E', 'disputa_ancora'`. [app/chamados/services/materializacao.py:1605]
- Reuso: `if mesma is not None: return mesma, None`; o materializador pode depois fazer `anotar(... {'pergunta_origem': p} ...)`. Assim, vínculo e log não provam, isoladamente, que aquele toque criou a linha. [app/ponto/registro_batida.py:137] [app/chamados/services/materializacao.py:1542]
- Carimbo: `p.carimbar(PerguntaDisputa.VIA_DECLARACAO if _tem_resp_via else _via_sem_decl, request_user, ...)`; a relavra pode alterar a via depois. [app/chamados/services/materializacao.py:1981] [app/chamados/services/relavra_via.py:125]
- Auditoria da materialização: `evento(acao, 'Batida', batida_pk, ... depois=valor_depois, ator=request_user)`; o próprio auxiliar declara que falha do log não derruba a materialização. [app/chamados/services/materializacao.py:1223]

A busca acima localizou as portas explicitadas; a exaustividade de **todos** os possíveis curadores e chamadores históricos permanece **[não verificado]**. O modelo de `Batida` expõe aprovador, justificativa e criação, mas não um campo de criador no trecho do modelo. [app/ponto/models.py:8] [app/ponto/models.py:56] [app/ponto/models.py:63] [app/ponto/models.py:79]

**B. Sonda proposta — não executada [não verificado].**

A sonda abaixo retorna assinaturas **compatíveis**, sem atribuir uma porta específica. Para evitar imprimir nomes ou CPF que possam estar em texto livre, ela mostra presença e SHA-256 de justificativa e motivos, em vez do texto. Isso limita a inspeção literal desses campos. A busca de auditoria inclui referências numéricas em descrição e JSON: são **candidatas**, pois um número isolado pode coincidir com outro identificador; também não prova ausência de referências escritas sem o número. [app/core/models.py:295] [app/core/models.py:299] [não verificado]

```python
# Executar dentro do Django pelo envelope de leitura, no tenant juliani.
# Não chama materializadores, validadores ou escritores.
# Não imprime nomes, CPF, textos livres, respostas ou conteúdo dos logs.

import hashlib
import os
import re

from django.conf import settings
from django.db import connection, transaction
from django.db.models import Q, TextField
from django.db.models.functions import Cast
from django.utils import timezone

from core.models import LogAuditoria
from ponto.models import Batida


CASOS = (
    (40, 139931), (60, 152873), (200, 146569),
    (263, 146777), (301, 151816), (444, 140817),
    (446, 144666), (549, 151894), (727, 140816),
    (881, 154095), (899, 151735), (909, 149856),
    (996, 153862), (996, 153847),
)
ERROS = 0


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("SETTINGS")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("CONFIGURACAO")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("CONEXAO")


def local(valor):
    if valor is None:
        return None
    if not timezone.is_aware(valor):
        raise ValueError("INSTANTE_SEM_FUSO")
    return timezone.localtime(valor).isoformat()


def usuario(pk):
    return None if pk is None else "u%d" % pk


def texto_protegido(valor):
    if valor is None:
        return None
    valor = str(valor)
    return {
        "vazio": not bool(valor),
        "sha256": hashlib.sha256(valor.encode("utf-8")).hexdigest(),
    }


def codigo(valor):
    # Os campos de código devem conter identificadores, não texto livre.
    # Valor inesperado é protegido para evitar exposição acidental.
    if valor is None:
        return None
    valor = str(valor)
    if re.fullmatch(r"[a-z0-9_]{1,64}", valor):
        return valor
    return {"valor_protegido": texto_protegido(valor)}


def campo_texto(obj, nome):
    if not any(f.name == nome for f in obj._meta.concrete_fields):
        return "CAMPO_INEXISTENTE"
    return texto_protegido(getattr(obj, nome))


def logs_candidatos(batida_pk, pergunta_pk):
    # Busca referência numérica em texto/JSON e referência estruturada.
    # As referências numéricas são candidatas, não identificação inequívoca.
    filtro = Q(modelo="Batida", objeto_id=batida_pk)
    numeros = [batida_pk]
    if pergunta_pk is not None:
        filtro |= Q(modelo="PerguntaDisputa", objeto_id=pergunta_pk)
        numeros.append(pergunta_pk)

    for numero in numeros:
        padrao = r"(^|[^0-9])%d([^0-9]|$)" % numero
        filtro |= (
            Q(descricao__regex=padrao)
            | Q(antes_texto__regex=padrao)
            | Q(depois_texto__regex=padrao)
        )

    return (
        LogAuditoria.objects.annotate(
            antes_texto=Cast("valor_antes", output_field=TextField()),
            depois_texto=Cast("valor_depois", output_field=TextField()),
        )
        .filter(filtro)
        .order_by("criado_em", "pk")
    )


def medir(cid, pk):
    b = Batida.objects.select_related("pergunta_origem").get(
        pk=pk, colaborador_id=cid
    )
    p = b.pergunta_origem

    print(
        "BATIDA",
        {
            "colaborador": "col%d" % cid,
            "pk": b.pk,
            "instante_local": local(b.timestamp),
            "tipo": codigo(b.tipo),
            "origem": codigo(b.origem),
            "aprovada": b.aprovada,
            "aprovada_por": usuario(b.aprovada_por_id),
            "aprovada_em_local": local(b.aprovada_em),
            "justificativa": campo_texto(b, "justificativa"),
            "pergunta_origem_id": b.pergunta_origem_id,
            "retratada_em_local": local(b.retratada_em),
            "criado_em_local": local(b.criado_em),
            "criador": "CAMPO_INEXISTENTE",
        },
    )

    pergunta_pk = None
    if p is None:
        print("PERGUNTA", {"batida_pk": pk, "pergunta_origem": None})
    else:
        pergunta_pk = p.pk
        print(
            "PERGUNTA",
            {
                "batida_pk": pk,
                "pk": p.pk,
                "motivo": campo_texto(p, "motivo"),
                "recusa_motivo": campo_texto(p, "recusa_motivo"),
                "tipo_resposta": codigo(p.tipo_resposta),
                "via_resolucao": codigo(p.via_resolucao),
                "respondida_em_local": local(p.respondida_em),
                "validada_em_local": local(p.validada_em),
                "validada_por": usuario(p.validada_por_id),
                "materializada_em_local": local(p.materializada_em),
                "materializada_por": usuario(p.materializada_por_id),
            },
        )
        decisoes = []
        for d in p.decisoes.order_by("ato_em", "pk"):
            decisoes.append(
                {
                    "pk": d.pk,
                    "ato": codigo(d.ato),
                    "ato_em_local": local(d.ato_em),
                    "autor": usuario(d.ato_por_id),
                    "ato_motivo": campo_texto(d, "ato_motivo"),
                }
            )
        print("DECISOES", {"pergunta_pk": p.pk, "sequencia": decisoes})

    logs = []
    for linha in logs_candidatos(pk, pergunta_pk):
        referencia_direta = (
            (linha.modelo == "Batida" and linha.objeto_id == pk)
            or (
                pergunta_pk is not None
                and linha.modelo == "PerguntaDisputa"
                and linha.objeto_id == pergunta_pk
            )
        )
        logs.append(
            {
                "pk": linha.pk,
                "acao": codigo(linha.acao),
                "quando_local": local(linha.criado_em),
                "usuario": usuario(linha.usuario_id),
                "referencia": (
                    "OBJETO_DIRETO" if referencia_direta
                    else "NUMERO_EM_TEXTO_OU_JSON_CANDIDATO"
                ),
            }
        )
    print("LOGS_CANDIDATOS", {"batida_pk": pk, "linhas": logs})

    # Compatibilidade com o ramo escritor atual, não prova de autoria:
    # a porta pode devolver uma batida preexistente e o vínculo pode mudar.
    if b.origem == "disputa_s84_retro":
        assinatura = "COMPATIVEL_RAMO_HORA_ORIGEM"
    elif b.origem == "disputa_ancora" and b.tipo == "E":
        assinatura = "COMPATIVEL_RAMO_DATA_HORA_ORIGEM_E"
    else:
        assinatura = "SEM_CASA"
    print(
        "ASSINATURA",
        {
            "batida_pk": pk,
            "resultado": assinatura,
            "porta_especifica": "NAO_VERIFICADA",
            "criacao_pelo_ato": "NAO_VERIFICADA",
        },
    )


def main():
    global ERROS
    try:
        with transaction.atomic():
            guarda_sombra()
            for cid, pk in CASOS:
                try:
                    # O savepoint permite continuar após erro de SQL.
                    with transaction.atomic():
                        medir(cid, pk)
                except Exception as exc:
                    ERROS += 1
                    print(
                        "ERRO_CASO",
                        "col%d" % cid,
                        "batida_pk=%d" % pk,
                        type(exc).__name__,
                    )
    except Exception as exc:
        ERROS += 1
        print("ERRO_GUARDA_OU_TRANSACAO", type(exc).__name__)

    print("ERROS=%d" % ERROS)
    print("FIM_SONDA_B4V3")


main()
```

`ERROS=0` indicaria apenas que a sonda terminou sem exceções capturadas; não converteria candidatos de log em prova nem identificaria uma porta específica compartilhada. [não verificado: sonda não executada]

perguntas de lei: 0

## 19/11 01:43 · sessao 5fd4cbef7389 · E4C-F5 · censo · origem=Code

O censo **não está fechado**: não classifiquei as 415 linhas do TSV por receptor e finalidade. Por isso, os totais por classe, a lista final exaustiva da classe (i) e o número de perguntas de lei permanecem **[não verificado]**. Abaixo estão os sítios e os limites que consegui conferir diretamente na cópia.

O diretório recebido não contém metadados Git; portanto, a identidade da cópia pelo hash `5bad81419` está **[não verificado]**. Não criei, alterei ou apaguei arquivos.

### Classificações comprovadas e candidatos à migração

`LeituraDoAto` tem exatamente os campos `validada_em`, `respondida_em` e `resposta_colab`; não oferece `materializada_em` nem `via_resolucao`. O código declara: `LeituraDoAto = namedtuple('LeituraDoAto', 'validada_em respondida_em resposta_colab')`. ([models.py:17](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:17))

| Sítio do TSV | Classificação verificada e consequência |
|---|---|
| [declaracao_texto.py:228](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:228), `_resposta_validada` | **PerguntaDisputa, classe (i):** `return perg.validada_em is not None`. A leitura correspondente é `leitura.validada_em`. Chamam-na, entre outros, `declarar_hora_colab` e `aplicar_declaracao`; a guarda pode impedir que essas portas avancem até a escrita. ([declaracao_texto.py:379](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:379), [declaracao_texto.py:669](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:669)) |
| [juizes.py:1552](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1552), `pergunta_respondida` | **Receptor polimórfico:** o literal é `p.respondida_em is not None and not texto_vazio(p.resposta_colab)`. A função aceita tanto a projeção quanto `LeituraDoAto`; a migração deve ser examinada nos chamadores, pois o próprio modelo declara que o juiz lê a leitura. `aplicar_declaracao` usa o resultado para decidir se chama o escritor de resposta. ([models.py:17](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:17), [declaracao_texto.py:688](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:688)) |
| [fio.py:188](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:188), `espera_o_texto_do_colaborador` | **PerguntaDisputa, classe (i):** `p.respondida_em is None or respondida_sem_texto(p)`. Para julgar pelo ato, o instante e o par usado pelo predicado precisam vir da mesma `LeituraDoAto`. Chamadores comprovados incluem `_calados` e `montar_chamados_fio`; efeito desses resultados sobre o momento de uma escrita **[não verificado]**. ([cobranca.py:241](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/cobranca.py:241), [fio.py:360](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:360)) |
| [validacao.py:685](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:685), `validar_pergunta` | **PerguntaDisputa, classe (i):** `if perg.validada_em:` decide o retorno `ja_validada`; o campo correspondente é `leitura.validada_em`. A própria função é uma porta de validação; seus chamadores completos e o efeito concreto da troca sobre as escritas **[não verificado]**. |
| [materializacao.py:199](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:199), `par_do_intervalo` | **PerguntaDisputa, candidato classe (i):** `if irma_validada and q.validada_em is None`; a linha 201 também testa `q.validada_em`. O campo correspondente é `leitura.validada_em`. Um chamador comprovado é `_validar_o_par`, que pode carimbar e materializar; a classificação final dos dois testes e seus demais chamadores está **[não verificado]**. ([validacao.py:632](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:632)) |
| [vigia_fio.py:747](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/vigia_fio.py:747), `respondida_sem_resposta` | **PerguntaDisputa, classe (iii):** a finalidade declarada é ler “os dois campos que a porta escreve” e “não julga a pergunta”; a query filtra `respondida_em` e lê `resposta_colab`. ([vigia_fio.py:738](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/vigia_fio.py:738)) |
| [veredito_lavrado.py:203](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:203), `marcos_do_lote` | **PerguntaDisputa, classe (iv):** lê marcos `materializada_em` e `respondida_em`; o comentário os identifica como “escritores da PROPRIA pergunta” usados para o veredito. ([veredito_lavrado.py:197](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:197)) |
| [painel.py:270](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/painel.py:270), `_jan_val` | **PerguntaDisputa, classe (v):** conta atividade humana de validação por janela, usando o carimbo projetado. ([painel.py:256](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/painel.py:256)) |
| [raiox.py:138](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/raiox.py:138), `creditos_perguntas` | **PerguntaDisputa, classe (vi), com julgamento posterior:** a consulta em lote extrai o par e depois chama `pergunta_respondida` sobre um `SimpleNamespace`. A fonte do par para esse julgamento precisa de desenho em lote; a substituição direta por uma propriedade em cada linha não está demonstrada. ([raiox.py:135](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/raiox.py:135), [raiox.py:147](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/raiox.py:147)) |

A guarda de declaração mostra por que uma mudança de leitura pode mudar **quando uma porta existente escreve**: `if not _pergunta_respondida(perg)` antecede `responder_pergunta_disputa_core(...)`; depois a função chama `validar_pergunta(...)`. ([declaracao_texto.py:688](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:688))

Não tratei `projecao_temporal_da_pergunta` como substituição mecânica: ela declara ler valores do C6 “no instante”, corta carimbos posteriores a `agora` e também transporta materialização e via, que não estão na leitura de três campos. Sua classificação e o desenho temporal de migração ficam **[não verificado]**. ([juizes.py:1382](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1382))

Os demais receptores homônimos, as demais linhas e os totais das seis classes: **[não verificado]**.

### Silêncio ressuscitado → dono admin

O corte fornecido determina admin na lista para a **ressuscitada** cujo silêncio decidido hoje a deixa sem dono, com dia do fato em competência aberta; para competência entregue pelo K6, determina lista textual do DP. Essa redação vem do anexo `respostas_1711_1120.txt` fornecido no pedido; sua existência como arquivo nesta cópia está **[não verificado]**. A cópia registra o corte anterior em [CORTES.md:545](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/CORTES.md:545).

O caminho atual tem três obstáculos concretos:

1. `_pela_decisao` retorna literalmente `return 'silencio_decidido', []`. ([tarefa_do_dia.py:182](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:182))
2. A carga retira a reabertura marcada como ressuscitação **antes** de escolher a última decisão. Logo, mudar indistintamente o ramo `SILENCIO` alcançaria também silêncios para os quais a ressuscitação não foi demonstrada; como transportar essa distinção ao ponto escolhido é parte do desenho. ([tarefa_do_dia.py:390](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:390))
3. `_portas_da_tarefa` inclui `silencio_decidido` em `perguntas_encerradas` e aceita como ações administrativas alcançáveis `DECIDIR_AUSENCIA`, `resolver_dia` ou `VALIDAR` nas condições de porta/âncora ali expressas. ([tarefa_do_dia.py:739](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:739), [tarefa_do_dia.py:746](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:746))

**Encaixes para examinar, sem escolher:**

- **Ação `resolver_dia` no caso identificado.** É um nome de ação administrativa já reconhecido pela composição, e a própria composição já o cria quando a autoridade do calendário oferece essa porta. A disponibilidade dessa porta para cada silêncio ressuscitado e sua adequação para levar a pergunta à lista estão **[não verificado]**. Seria necessário distinguir o caso ressuscitado e ajustar o tratamento de encerrada conforme a rota escolhida. ([tarefa_do_dia.py:746](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:746), [tarefa_do_dia.py:776](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:776))
- **Ação ligada à âncora da pergunta, reconhecida pela composição.** `VALIDAR` já existe e tem verificação de âncora, mas a porta de validação recusa pergunta sem resposta: `estado: 'sem_resposta'`. Portanto, sua adequação ao silêncio ressuscitado **[não verificado]**; não basta rotular a ação como `VALIDAR`. O botão `cobrar` existe, mas o código o exclui dos verbos residuais que dão dono. ([tarefa_do_dia.py:746](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:746), [validacao.py:681](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:681), [tarefa_do_dia.py:757](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:757), [views_cobrar.py:44](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/views_cobrar.py:44))
- **Composição de dono a partir de um verbo residual existente.** A composição já deriva `admin` de verbos residuais, e publica o nome do verbo; porém a existência de um verbo residual apropriado para cada caso e de uma porta que cumpra a lista determinada pelo corte está **[não verificado]**. Retirar apenas `silencio_decidido` da tupla de encerradas não cria, por si, uma ação nem prova o dono. ([tarefa_do_dia.py:757](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:757), [tarefa_do_dia.py:790](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:790))

Para qualquer encaixe, já existe carga em lote que consulta `competencia_entregue` sobre a empresa e a janela do dia. Porém o filtro `exportada` atual é explicitamente limitado a **novas exposições administrativas daquela obra**, preservando tarefas E2; não prova sozinho que bloqueará o novo caminho de silêncio. ([tarefa_do_dia.py:207](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:207), [tarefa_do_dia.py:558](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:558), [tarefa_do_dia.py:273](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:273))

O teste existente compara a tarefa antes e depois da ressuscitação para os dois casos. O caso sem decisão deve conservar sua expectativa; o caso com silêncio precisa passar a verificar o resultado exigido para competência aberta, incluindo dono e lista, e o recorte de competência entregue precisa de prova própria. O fixture e a asserção atuais estão em [test_f1nova_b1_juiz_respondida.py:328](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:328).

**`chamados/models.py` por desenho:** para os três encaixes acima, a necessidade de alterar o arquivo permanece **[não verificado]**. A leitura já existe e a composição examinada fica em `tarefa_do_dia.py`, mas ainda falta demonstrar como a marca de ressuscitação chega ao encaixe e como todos os consumidores do censo são atendidos. ([models.py:4295](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4295), [tarefa_do_dia.py:390](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:390))

Não executei testes. A classificação completa, a lista final de sítios e a análise de todas as leis exigem continuação da leitura; não há base para declarar ausência de perguntas de lei. [nao verificado]

perguntas de lei: [não verificado]

## 19/11 01:42 · sessao daadceabae56 · QUEIMA-B6 · leitura · origem=Code

**Não consigo recuperar quais eram as três perguntas anunciadas em `r.txt`: a matriz termina com a contagem, mas não as enumera.** Também não provei que o número correto seja zero. [não verificado]

A releitura encontrou três conflitos possíveis que **já têm resposta**. Eles não podem ser apresentados como as três perguntas originais, pois essa identificação não está no anexo. [não verificado]

1. **Admin `trabalhou` versus fato do cartório.** O código responde à precedência na célula: `if cel.veredito_via == 'admin':` impede que a lavra do cartório substitua o veredito administrativo. [app/ponto/portas/celula.py:1060] A L-007 trata de escritor único por estado e trilha; não é necessário supor uma escolha nova para essa precedência. [app/docs/LEIS.md:71]

   A sonda não mede colisões entre esses dois escritores: conta o **estado atual** da célula, não as tentativas anteriores de escrita. O predicado necessário seria: mesmo colaborador e data, decisão administrativa vigente e tentativa posterior do cartório com veredito diferente, incluindo tentativas que não alteraram a célula. A disponibilidade de trilha suficiente para medi-lo é [não verificado]. [app/ponto/portas/celula.py:1060]

2. **Trabalho versus ausência ou justificativa.** O corte já responde que a justificativa cobre seu tipo e período e que, havendo trabalho, paga-se o realizado mais o abonado do período; para ausência parcial, há regras específicas sobre a batida dentro do período e a falta de horário declarado. [app/docs/CORTES.md:411] [app/docs/CORTES.md:418] A L-089 trata do efeito da ausência em dia previsto de trabalho. [app/docs/LEIS.md:121]

   A sonda mede uma sobreposição mais ampla, sem demonstrar valores de realizado ou abono nem se os períodos coincidem. As linhas literais fornecidas são:

   ```text
   PAR|empresa=2|categoria1=CELULA_cartorio_fato_em_ausencia|categoria2=DIAPAGO_motor_TRABALHO|dias=48
   PAR|empresa=3|categoria1=CELULA_cartorio_fato_em_ausencia|categoria2=DIAPAGO_motor_TRABALHO|dias=16
   PAR|empresa=4|categoria1=CELULA_cartorio_fato_em_ausencia|categoria2=DIAPAGO_motor_TRABALHO|dias=4
   ```

   Linha numerada de `sonda.out`: [não verificado]. O predicado para medir a aplicação financeira da regra seria: mesmo colaborador e data, ausência ou justificativa aprovada com seu período, trabalho realizado e `DiaPago` da versão motor, comparando os minutos realizados e abonados com os respectivos períodos. [não medido]

3. **Folga trabalhada versus hora trabalhada sem escala certa.** O código já explicita que, sem escala certa, a hora entra em `horas_trabalhadas` sem o adicional de 100%; com escala certa, a folga é encaminhada à rubrica própria, com separação para feriado. [app/ponto/services/fechamento.py:925] [app/ponto/services/fechamento.py:936] [app/ponto/services/fechamento.py:965] A L-086 chega perto ao tratar da dobra em domingo e feriado, com ressalva sobre o texto de feriado indicada na própria lei. [app/docs/LEIS.md:118]

   A sonda conta rubrica positiva de folga, mas não mede a alternativa sem escala certa nem o impacto de uma mudança de regra. [não verificado] Linhas literais:

   ```text
   CENSO|empresa=2|categoria=DIAPAGO_motor_FOLGA|atuais=186|vigentes_HE=0|revogados=NA|dias_sobrepostos=184
   CENSO|empresa=3|categoria=DIAPAGO_motor_FOLGA|atuais=35|vigentes_HE=0|revogados=NA|dias_sobrepostos=35
   CENSO|empresa=4|categoria=DIAPAGO_motor_FOLGA|atuais=2|vigentes_HE=0|revogados=NA|dias_sobrepostos=2
   ```

   Linha numerada de `sonda.out`: [não verificado]. O predicado para medir o outro ramo seria: períodos de `resultado.periodos_ft` cujo julgamento de `escala_certa_no_dia` seja falso, por empresa e dia de jornada, comparados à lavratura correspondente em `DiaPago`; para medir dinheiro alterado, seria necessário comparar as rubricas antes e depois de uma regra definida. [app/ponto/services/fechamento.py:933] [app/ponto/services/fechamento.py:967]

A sobreposição entre categorias da sonda, por si só, não estabelece pagamento duplicado nem responde quantos dias mudariam sob uma opção de regra. [não verificado] Sem a enumeração anterior ou três lacunas demonstradas na releitura, atribuir um número corrigido seria uma hipótese. [não verificado]

perguntas de lei: [não verificado]

## 19/11 01:41 · sessao 2d8feb0d356d · E4C-F3-LAVRA-FOTO · construtor · origem=Code

Criado [test_f3_lavra_foto_sem_decisao.py](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:1). Os testes não foram executados, conforme solicitado; os resultados são [não verificado]. [nao verificado]

Os mutantes usam a âncora `    ids = list(colaborador_ids)\n`, exceto M6, que também usa `import datetime\n` para inserir o apelido no módulo. As inserções verificam que cada âncora ocorre uma única vez ([arquivo:9](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:9), [arquivo:70](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:70)).

Para executar cada mutante individualmente, a partir de `app/`:

```bash
T=chamados.tests.test_f3_lavra_foto_sem_decisao.LavraFotoSemDecisaoTest
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$T.test_MORDE_m1_import_direto"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$T.test_MORDE_m2_import_apelidado"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$T.test_MORDE_m3_modulo_apelidado"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$T.test_MORDE_m4_get_model"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$T.test_MORDE_m5_related_manager"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$T.test_MORDE_m6_apelido_no_modulo"
```

O arquivo também contém os métodos `test_fonte_real_sem_leitura`, `test_alvo_ausente_falha`, `test_controle_leitura_na_vizinha_nao_conta`, `test_MORDE_funcao_aninhada`, `test_MORDE_lookup_relacionado` e `test_MORDE_get_model_string_unica` ([arquivo:81](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:81)). O controle usa a âncora `def esta_stale(p, marcos, agora):\n`; os três mutantes adicionais usam a âncora do corpo ([arquivo:122](/home/ronald/wt-codex-e4c-f3-lavra-foto-013922/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:122)).

_7242 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
