# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 20:27** · publicado em **10/11/2026 20:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 20:27 · sessao 99c28c41e23e · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura c8 preparada. A execução ORM e a nova contagem SQL ficam para a sessão: **[nao verificado]**.

Arquivos e funções alterados, por item:

1. [test_3b_reuso_746.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_3b_reuso_746.py:15)::`test_reusa_orfao_nao_duplica` e `test_sem_orfao_cria_normal`: chamadas usam `gps_mock`. As asserções e a subcategoria do órfão foram preservadas. (`:19–35`)

2. [test_bo_disputa_volta5.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5.py:32)::`_dona`: cria a outra aberta depois da dona, com instante fixo posterior. O comentário registra a ordenação pressuposta; a ordenação de produção permanece `-aberta_em`. (`chamados/models.py:1804`)

3. No mesmo arquivo:
   - `test_replay_porta_real_conserva_dia_d_e_linha_da_dona_fechada_duas_vezes`: usa `bulk_create` para montar a batida sem `post_save` e afirma ausência de disputa aberta antes da medição. Preserva as exigências de quatro chamadas à porta e conservação do estado. (`:141–182`)
   - `test_RED_d2_aberta_com_outros_motivos_e_canonica_em_fechada_duas_vezes`: acrescentado; exige retorno A, `values()` de B idêntico, FK da pergunta em B, motivo ausente de A, nenhum push e estado idêntico na segunda chamada. (`:186–218`) [nao verificado]
   - **Não alterei D2**: o código já restringe A aos motivos locais, conserva B fechada e retorna A. (`chamados/services/disputa_emissao.py:592–597,619–633`)

4. [disputa_emissao.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:773)::`criar_ou_atualizar_disputa`: elimina a segunda releitura quando pendentes não devolve perguntas. Mantém a primeira e mantém a segunda quando pendentes pode ter provocado fechamento. (`:774–786`)

A conta proposta dos cinco acréscimos é:

| Acréscimo | Decisão |
|---|---|
| SAVEPOINT + RELEASE externos | Ficam: protegem o atomic comum e o rollback da agregação em erro. (`disputa_emissao.py:606–633`) |
| SELECT de perguntas locais | Fica: determina os motivos admitidos em A. (`:561–564,621`) |
| Primeiro EXISTS | Fica: a cascata do gerador pode alcançar fechamento por outra instância. (`:495–501`; `chamados/reconciliador.py:411–413`) |
| Segundo EXISTS sem perguntas pendentes | Sai. (`disputa_emissao.py:777–785`) |

O savepoint exclusivo de nascimento já está depois do retorno de D2. (`disputa_emissao.py:633–636`)

**Sai 1; ficam 4. Tetos propostos: 149 e 223.** As duas linhas receberam justificativa C8 e `MEDIDO <a preencher pela sessao>`; nenhum outro teto foi alterado. [test_selo_chokepoint_escrita.py:186](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/ponto/tests/test_selo_chokepoint_escrita.py:186), `:199`. Totais SQL: **[nao verificado]**.

Acrescentei [test_bo_disputa_volta8_memoria.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta8_memoria.py:28)::`fonte`, `Ambiente8.__init__`, `Delta8Test.*` e `rodar`. O RED registrou `2 != 1` antes da cura; os controles atuais terminaram sem falhas, e os cinco mutantes produziram falhas por asserção. São provas com doubles, sem certificação ORM. (`:1–5,71–206`)

Comandos do controle e de cada mutante:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta8_memoria.py
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta8_memoria.py --mutante reconsulta_sem_pendentes
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta8_memoria.py --mutante ignora_fecho_gerador
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta8_memoria.py --mutante ignora_fecho_pendentes
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta8_memoria.py --mutante estrangeira_em_a
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta8_memoria.py --mutante salva_fechada_d2
```

A rodada conjunta sem banco registrou 52 testes, sem falhas: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -q \
  chamados.tests.test_bo_disputa_sem_pergunta_memoria \
  chamados.tests.test_bo_disputa_volta2_memoria \
  chamados.tests.test_bo_disputa_volta3_memoria \
  chamados.tests.test_bo_disputa_volta4_memoria \
  chamados.tests.test_bo_disputa_volta5_memoria \
  chamados.tests.test_bo_disputa_volta5_replay_red_memoria \
  chamados.tests.test_bo_disputa_volta8_memoria
```

Quanto aos leitores de `motivos`, c8 preserva a seleção e a persistência anteriores de D2. (`disputa_emissao.py:619–633`) A leitura de cada sítio continua sendo:

| Leitor | Resultado usado |
|---|---|
| `api/foto_recebida.py:63,70` | Seleciona a dona aberta de foto e calcula motivos restantes. |
| `colaboradores/signals.py:74` | Seleciona dona aberta de `posto_desativado`. |
| `escala/signals.py:158` | Seleciona dona aberta de `ancora_ausente`. |
| `chamados/models.py:2031–2037` | Revalida estado aberto e presença do motivo antes da remoção. |
| `auditar_disputas.py:88,142,169,182–194` | Audita contexto, lista motivos e compara com perguntas locais. |
| `auditar_invariantes_chamados.py:50` | Resolve os dias dos motivos registrados na disputa. |
| `chamados/relatorios_disputa.py:52` | Exibe os motivos registrados. |
| `chamados/services/acoes_disputa.py:149` | Compõe texto de regularização. |
| `chamados/services/fio.py:680` | Compõe texto de regularização. |
| `pauta_aparelhos_compartilhados.py:49–57` | Filtra motivo e lê seu contexto. |
| `alertar_disputas_pendentes.py:82` | Registra os motivos no texto de auditoria. |
| `templates/chamados/detalhe_local.html:301` | Exibe os motivos da aberta. |
| `templates/chamados/partials/modal_fio.html:326` | Escolhe a orientação para âncora. |
| `colaboradores/services/drawer.py:75–82` | Usa perguntas vivas para o banner. |

Efeitos desses leitores sobre dados reais: **[nao verificado]**.

Não executei testes com banco. Ruff também ficou pendente: executável e módulo indisponíveis no ambiente **[nao verificado quanto ao lint]**. Comandos para a sessão: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_3b_reuso_746 \
  chamados.tests.test_bo_disputa_volta5 \
  ponto.tests.test_selo_chokepoint_escrita

ruff check --no-cache \
  chamados/services/disputa_emissao.py \
  chamados/tests/test_3b_reuso_746.py \
  chamados/tests/test_bo_disputa_volta5.py \
  chamados/tests/test_bo_disputa_volta8_memoria.py \
  ponto/tests/test_selo_chokepoint_escrita.py
```

## 10/11 20:20 · sessao 81bdf4d4b274 · BO-DISPUTA-SEM-PERGUNTA · sonda · origem=Code

| Contagem | Valor | Referência |
|---|---:|---|
| disputas_total | 7009 | [nao verificado] |
| fechadas_vazia_total | 300 | [nao verificado] |
| fechadas_vazia_desde_1110 | 300 | [nao verificado] |
| fechadas_vazia_desde_0311 | 93 | [nao verificado] |
| fechadas_vazia_colabs_distintos_desde_1110 | 107 | [nao verificado] |
| fechadas_vazia_com_zero_pergunta_desde_1110 | 300 | [nao verificado] |
| fechadas_vazia_no_mesmo_minuto_do_nascimento_desde_1110 | 237 | [nao verificado] |
| abertas_total | 489 | [nao verificado] |
| abertas_com_zero_pergunta | 8 | [nao verificado] |
| nascidas_desde_1110 | 1387 | [nao verificado] |

O trecho fornecido não contém linhas de erro nem indicação de contagens que a sonda não conseguiu fazer. [nao verificado]

## 10/11 20:20 · sessao e7138677bd9d · MAPA-FILA-BO · censo · origem=Code

## BO-0611-05-TEM-GPS-LEITORES (lote 4)

| Achado | Conferência nesta árvore |
|---|---|
| A1 | **confere**. Os três sítios perguntam por nulidade, usando `.exclude(latitude=None).exclude(longitude=None).exists()`, sem chamar `tem_gps`; zero não é excluído. `app/colaboradores/views.py:337`, `app/colaboradores/views.py:356`, `app/colaboradores/views.py:395`. |
| A2 | **parcial**. O filtro exige latitude, longitude e precisão não nulas; admite coordenada zero. Isso confere no código. “Já contava zero na base” depende de dado: quantidade de amostras zero efetivamente selecionadas; essa contagem não foi fornecida. `app/ponto/services/vinculo_na_lista.py:49`, `app/ponto/services/vinculo_na_lista.py:50`. [nao verificado: contagem na base anterior] |
| A3 | **confere**. O JS usa truthiness conjunta, `b.latitude && b.longitude`, nos dois pontos. `app/templates/core/dashboard2.html:242`, `app/templates/core/dashboard2.html:287`. |
| A4 | **confere**. Os escritores do contexto usam `float(coordenada) if coordenada else None`; zero numérico vira `None`, tanto na ocorrência quanto no contexto escalar. `app/ponto/services/geofence.py:349`, `app/ponto/services/geofence.py:350`, `app/ponto/services/geofence.py:391`, `app/ponto/services/geofence.py:392`. |
| A5 | **confere**. O mapa faz `parseFloat` e rejeita `isNaN`; zero passa por essa forma. `app/static/js/geo-colab-map.js:29`, `app/static/js/geo-colab-map.js:39`. |
| A6 | **parcial**. `tem_gps` devolve `bool(latitude and longitude)`: texto `"0"` é verdadeiro e `Decimal('0')` é falso. `_sanitiza_geo` testa `float(v)`, mas não atribui a conversão ao campo; o envio PWA usa `FormData.append`. Entretanto, **o teste atual já cobre texto zero**, caracterizando a divergência. `app/ponto/services/geofence.py:145`, `app/ponto/registro_batida.py:15`, `app/static/pwa.js:145`, `app/static/pwa.js:146`, `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:79`. |
| A7 | **confere**. O ramo sem GPS pergunta ao juiz, mas a criação de `AlertaGeofence(tipo='sem_gps')` está dentro de `if posto`. Isso prova a condição de nascimento do alerta, não uma obrigação de removê-la. `app/ponto/services/geofence.py:269`, `app/ponto/services/geofence.py:270`, `app/ponto/services/geofence.py:271`. |
| A8 | **parcial**. Os leitores atuais de contexto já chamam `par_tem_gps`; a fixture fornece expressamente as duas chaves. Chave ausente não equivale ao caso testado com chave presente e `None`. A consequência de execução com chave ausente não foi demonstrada. `app/templates/chamados/partials/_relatorio_geo.html:31`, `app/templates/chamados/partials/modal_fio.html:481`, `app/ponto/tests/test_bo0611_05_templates_geo.py:132`. [nao verificado: renderização com chaves ausentes] |
| A9 | **parcial**. O módulo `templates_geo` monta contexto por `is not None` e seu laço não contém zero; o teste do detector exige zero como evidência. Porém, o módulo atual do juiz já renderiza contextos com zero. Os testes de forma JS e de templates renderizados existem; sua presença não comprova o RED histórico contra a base. `app/ponto/tests/test_bo0611_05_templates_geo.py:132`, `app/ponto/tests/test_bo0611_05_templates_geo.py:178`, `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135`, `app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:218`, `app/ponto/tests/test_l030_forma_js.py:31`, `app/ponto/tests/test_l030_templates_renderizados.py:14`. [nao verificado: RED das voltas 2–5 contra a base] |

**(a) Lei, juiz e portas.** A lei é testemunha lendo a autoridade, escritor único e batida nunca barrada: L-002, L-007 e L-030; o corte específico manda consultar o mesmo juiz, conserva a semântica de zero e proíbe mudar o veredito do geofence nessa migração. `app/docs/LEIS.md:60`, `app/docs/LEIS.md:65`, `app/docs/LEIS.md:78`, `CLAUDE.md:20`, `CLAUDE.md:140`, `app/docs/CORTES.md:442`.

O juiz **já está registrado**: `ponto/services/geofence.py::tem_gps`. Os filtros de template delegam a ele. O lookup `JUIZES.md` fornecido não traz essa entrada, mas determina que o registro vivo prevaleça. `app/core/juizes.py:1201`, `app/core/templatetags/hasner_filters.py:34`, `app/core/templatetags/hasner_filters.py:41`, `app/docs/JUIZES.md:10`, `app/docs/JUIZES.md:14`.

A cura dos leitores toca `tem_gps` e seus consumidores; coerência de tipo na escrita toca `_sanitiza_geo`/`registrar_batida`. Se preservar coordenadas brutas no contexto, toca também o emissor e as portas de nascimento/fusão do chamado. `app/ponto/registro_batida.py:5`, `app/ponto/registro_batida.py:94`, `app/core/portas.py:103`, `app/ponto/services/geofence.py:345`, `app/chamados/models.py:1095`, `app/chamados/services/contexto.py:163`.

**(b) Estado e decisor.**

| Pergunta | Resposta |
|---|---|
| Cria campo? | **Não**: latitude/longitude nullable e tipo `sem_gps` já existem. `app/ponto/models.py:58`, `app/ponto/models.py:59`, `app/ponto/models.py:233`. |
| Cria escolha nova de campo? | **Não**, para migração ao juiz e normalização numérica; os módulos emitidos já existem. `app/ponto/services/geofence.py:344`, `app/chamados/catalogo/modulos.py:160`, `app/chamados/catalogo/modulos.py:162`. |
| Pode produzir estado/linhas que esses caminhos hoje não produzem? | **Sim**. Normalizar texto zero pode trocar o ramo executado e o tipo de alerta/chamado. Preservar zero bruto em A4 também muda o JSON que hoje escreve `None`. Isso é alcance por caminho; não prova que tais valores nunca existiram em todo o banco. `app/ponto/services/geofence.py:145`, `app/ponto/services/geofence.py:269`, `app/ponto/services/geofence.py:344`, `app/ponto/services/geofence.py:349`. [nao verificado: inexistência global no banco] |
| Muda quem decide? | **Sim**: os predicados locais passam a perguntar ao juiz registrado. A normalização também pode mudar a natureza/fila do chamado: `gps_ausente` é registro; `gps_fora_geofence` vai à validação administrativa. `app/colaboradores/services/drawer.py:63`, `app/core/juizes.py:1201`, `app/chamados/catalogo/natureza.py:18`, `app/chamados/catalogo/natureza.py:29`, `app/chamados/catalogo/natureza.py:44`. |

Remover a guarda de posto de A7 seria uma ampliação distinta: criaria o alerta sem posto que hoje não nasce nesse ramo; o corte atual não autoriza alterar esse veredito. `app/ponto/services/geofence.py:270`, `app/docs/CORTES.md:442`.

**(c) Censo.** Foram identificadas **78 unidades**: função Python, classe de admin, template, arquivo JS ou script, deduplicados por unidade; ocorrências repetidas dentro da mesma função não aumentam o total. A união compreende par de coordenadas, cópias no JSON, booleano derivado, alertas, classificação `gps_ausente`, entrada/transporte, sombra e exclusão dos registros. Os sítios e formas são os seguintes.

| Grupo | Unidades e formas |
|---|---|
| Python direto — **24** | `app/ponto/registro_batida.py:94` — `registrar_batida`, `create` em `:152`/`:157`; `app/ponto/registro_batida.py:331` — `semear`, construtor por `**d` e `bulk_create`, restrito a ensaio; `app/ponto/services/geofence.py:145` — juiz, truthiness; `app/ponto/services/geofence.py:269` — `verificar_geofence`, juiz, criação de alertas e ternários de contexto; `app/ponto/services/geofence.py:52` — `vinculo_contagem`, queryset `isnull=False`; `app/ponto/services/geofence.py:405` — `ocorrencias_do_chamado`, `.get`; `app/ponto/services/geofence.py:425` — `reavaliar_chamados_geofence`, `is not None`; `app/core/templatetags/hasner_filters.py:34` e `app/core/templatetags/hasner_filters.py:41` — dois filtros delegando ao juiz; `app/colaboradores/views.py:53` — exclusão de nulos; `app/colaboradores/views.py:337` — `vincular_posto`, exclusão de nulos/`.exists()`; `app/colaboradores/views.py:395` — `_render_row`, mesma forma; `app/colaboradores/views.py:2474` — `_geo_do_pino`, ausência de tupla/`is None`; `app/colaboradores/services/detalhe.py:22` — exclusão de nulos, contagem em `:53`, `float` em `:82`; `app/colaboradores/services/drawer.py:63` — `bool(latitude and longitude)`; `app/colaboradores/painel_cell.py:37` — leitura da tupla, `or ''` em `:54`; `app/colaboradores/services/situacional.py:141` — `values_list`, `or ''` em `:199`; `app/chamados/views.py:692` — queryset com alias `_BatidaGeo`; `app/core/views.py:1024` — `is not None`; `app/core/services/painel_op.py:227` — truthiness; `app/ponto/views.py:468` — serialização por `is not None` e contador pelo juiz em `:485`; `app/ponto/services/vinculo_na_lista.py:49` — filtro de nulidade e leitura em `:56`; `app/chamados/services/acoes_chamado.py:203` — `.get` e cópia para `Advertencia.detalhe_json`; `app/ponto/admin.py:12` — leitura reflexiva por `SoLeitura`, expandindo campos em `app/core/admin.py:74`. |
| Templates/JS diretos — **12** | `app/templates/core/_d2_feed.html:16` e `app/templates/ponto/batidas_offline_pendentes.html:96` — filtro juiz; `app/templates/chamados/partials/_relatorio_geo.html:31` e `app/templates/chamados/partials/modal_fio.html:481` — filtro do par; `app/templates/colaboradores/partials/_drawer_diagnostico.html:49` — truthiness de `b.tem_gps`; `app/templates/colaboradores/detalhe.html:188` — interpolação direta no link; `app/templates/core/dashboard2.html:242` — JS `&&`, repetido em `:287`; `app/static/js/geo-colab-map.js:29` — `parseFloat`/`isNaN`; `app/templates/chamados/fio_colaborador.html:116` — interpolação, `parseFloat`/`isNaN` em `:313`; `app/templates/chamados/painel_gestao.html:583` — `isNaN`; `app/templates/colaboradores/partials/_ps_linha.html:1` — interpolação em dataset; `app/static/pwa.js:145` — `!= null` e `FormData.append`. |
| Ingresso/transporte — **11** | `app/ponto/registro_batida.py:5` — sanitizador, valida com `float` sem converter; `app/api/views_core.py:403` e `app/api/views.py:416` — `request.data.get` e passagem à porta; `app/ponto/views.py:270` — POST `or None`, gravação na sessão em `:320`; `app/ponto/services/triagem_batida.py:507` — recuperação da sessão por truthiness; `app/ponto/services/triagem_batida.py:707` — kwargs da porta; `app/templates/ponto/bater_ponto.html:296` — aquisição e atribuição a campos; `app/templates/ponto/bater_ponto_v2.html:178` — aquisição, campos em `:105`; `app/templates/ponto/triagem/base_triagem.html:59` — aquisição; `app/templates/ponto/triagem/apto.html:223` — atribuição aos campos; `app/templates/ponto/triagem/turno_confirmar.html:12` — campos ocultos. |
| AlertaGeofence — **11** | `app/ponto/models.py:258` — `ainda_vale`, `is None` conserva alerta; `app/ponto/admin.py:31` — filtro por tipo e campos readonly; `app/core/cell.py:15` — queryset ativo para conjunto; `app/core/views.py:199` — conjunto por batida; `app/core/views.py:407` — comparação `tipo='ponto_fora'`; `app/core/views.py:783` — conjunto por colaborador; `app/core/views.py:1167` — conjunto por batida; `app/ponto/management/commands/reconciliar_alertas_geofence.py:33` — queryset, `is None` em `:42`, `update(obsoleto_em=...)` em `:52`; `app/ponto/management/commands/testar_pwa.py:291` — ensaio, `.exists()` em `:294`; `app/ponto/management/commands/zerar_dados_operacionais.py:47` — ensaio, `count` e `delete` em `:85`; `app/templates/core/dashboard_supervisor.html:90` — truthiness da latitude do alerta. |
| Módulo/rótulo — **9** | `app/chamados/catalogo/natureza.py:38` — `not in MODULOS_AUDITORIA`; `app/chamados/catalogo/natureza.py:44` — escolha da pilha; `app/chamados/services/desenlace.py:38` — membership/lookup; `app/chamados/services/desenlace.py:68` — comparação de pilha/lookup; `app/chamados/services/verbos.py:158` — lookup por subcategoria; `app/chamados/services/verbos.py:186` — módulo e lookups; `app/api/views.py:60` — `.get` do rótulo; `app/api/views_mensageria.py:1161` — lookup por subcategoria; `app/chamados/management/commands/auditar_invariantes_chamados.py:1068` — filtro `modulo_origem__in=MODULOS_AUDITORIA`. |
| Template de módulo — **1** | `app/templates/chamados/detalhe_local.html:274` — comparação explícita com `gps_ausente` e inclusão do relatório. |
| Escrita do chamado — **5** | `app/chamados/models.py:1051` — natureza do nascimento; `app/chamados/models.py:1095` — INSERT por `criar`; `app/chamados/services/nota.py:110` — delegação da fusão; `app/chamados/services/nota.py:123` — alias `_ctx`; `app/chamados/services/contexto.py:144` — união de listas, ausência por `k not in ctx` em `:155`, `save` em `:163`. |
| Sombra — **3** | `bin/sombra_mascara_pessoal.py:12` — alias `R`, `R.NULO`/`objects.update` em `:17`/`:20`, JSON/`bulk_update` em `:44`/`:62`; `bin/sombra_regras_pessoais.py:36` — recursão, coordenada numérica vira `None`; `bin/sombra_selo_pessoal.py:55` — SQL `IS NOT NULL`, JSONPath em `:90`. |
| Exclusão do registro — **2** | `app/ponto/registro_batida.py:291` — `apagar`, `delete`; `app/ponto/registro_batida.py:345` — `apagar_em_ensaio`, `queryset.delete`, com guarda em `:343`. |

`anotar` não é escritor de coordenadas: a allowlist não contém latitude/longitude e campos externos são recusados. O escritor potencial de ensaio `semear` aceita dicionários genéricos, mas exige banco de ensaio. `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`, `app/ponto/registro_batida.py:326`, `app/ponto/registro_batida.py:335`.

Os delegadores de reavaliação também foram conferidos: command chama o serviço; signal de `Posto` chama o mesmo serviço após commit; o cron executa `reconciliar_geofence --alarme`, enquanto a reconciliação de alertas fica sob demanda. Não são novas leituras diretas de coordenadas além das unidades acima. `app/ponto/management/commands/reconciliar_geofence.py:33`, `app/ponto/signals.py:306`, `app/config/crons.py:486`, `app/config/crons.py:1452`.

**Limite do fechamento:** 78 é o inventário nominal acima, não uma certificação de todos os consumidores genéricos de `ChamadoColaborador`, `PerguntaDisputa`, `FollowupChamado` e leitura dinâmica de JSON. Esse universo integral permanece **[nao verificado]**. A cópia em `Advertencia.detalhe_json` existe; outro leitor nominal dessas chaves não foi localizado. `app/chamados/services/acoes_chamado.py:197`. [nao verificado: leitura dinâmica/reflexiva desse JSON]

**(d) Casa.** **COMPLEMENTO de O254 — CONTRATO-BATIDA-GANHA-LEITOR**, ligado ao BO pai; não é BO simples de um sítio. A casa está no BACKLOG e o contrato vivo aponta para o teste da família. `app/docs/BACKLOG.md:1458`, `app/docs/BACKLOG.md:1873`, `app/core/contratos_estruturais.py:375`.

A descrição do BO pai ainda relata 403 sem coordenadas e interrupção por GPS: **não confere nesta árvore**. Os kernels passam as coordenadas à porta e o requisito GPS do v2 termina permitindo prosseguir. A afirmação histórica de três perguntas sem juiz na descrição de O254 também não descreve o registro atual, que lista duas. `app/docs/BACKLOG.md:1873`, `app/api/views_core.py:640`, `app/api/views.py:648`, `app/templates/ponto/bater_ponto_v2.html:292`, `app/core/juizes.py:1203`.

O registro separado desse filho na árvore anterior não foi disponibilizado na cópia atual. [nao verificado: descrição anterior do filho em 6c0517eb9]

**(e) Padrão.** **Single Writer Principle / juiz único**, acompanhado de **Architecture Conformance Test** para cobrar o significado da pergunta. Ambos são termos existentes. `app/docs/GLOSSARIO.md:12`, `app/docs/GLOSSARIO.md:15`.

**(f)**

perguntas de lei: 1

1. **Batida sem GPS e sem posto deve gerar `AlertaGeofence.sem_gps` e chamado `gps_ausente`?** Hoje o escritor exige posto; o corte da migração manda conservar o veredito. Essa pergunta condiciona uma eventual cura de A7, não a migração dos leitores para o juiz existente. `app/ponto/services/geofence.py:270`, `app/ponto/services/geofence.py:344`, `app/docs/CORTES.md:442`.

**Depende de dado:** contagens de zero textual/numérico por canal; batidas parciais; contextos sem chaves; batidas sem posto; divergências entre cada leitor e o juiz; alertas/chamados que mudariam de classificação. [nao verificado: contagens não fornecidas]

## BO-0611-05-RESTO-DA-CLASSE (lote 4)

| Achado | Conferência nesta árvore |
|---|---|
| B1 | **confere**. `if(!_fotoBlob)` retorna `false`, e o chamador encerra antes do POST. A API já trata foto ausente como sinal. `app/templates/ponto/bater_ponto_v2.html:287`, `app/templates/ponto/bater_ponto_v2.html:290`, `app/templates/ponto/bater_ponto_v2.html:312`, `app/api/views_core.py:418`. |
| B2 | **parcial**. As APIs procuram mesmo tipo em intervalo inclusivo **±5 minutos**, usam `.first()` e truthiness, devolvendo `ja_registrada` antes da porta. A web usa **[agora−5 minutos, agora]**, não ±5. Isso não demonstra que cada tentativa retida seja fato distinto. `app/api/views_core.py:508`, `app/api/views_core.py:510`, `app/api/views_core.py:517`, `app/api/views.py:523`, `app/ponto/services/triagem_batida.py:662`, `app/ponto/services/triagem_batida.py:663`, `app/ponto/services/triagem_batida.py:670`. |
| B3 | **parcial**. A tentativa chega a `registrar_batida`; a própria porta pode devolver `(None, conflito)`. A janela é 300 s com escala/marcos, senão 60 s, com exceção por marco próprio. As APIs reutilizam `_janela_oposto as _jop`; a web procura a oposta em ±60 s fixos, sem excluir retratadas, podendo não encontrar a batida que motivou uma recusa de 300 s. `app/ponto/registro_batida.py:30`, `app/ponto/registro_batida.py:145`, `app/ponto/registro_batida.py:151`, `app/api/views_core.py:658`, `app/api/views.py:666`, `app/ponto/services/triagem_batida.py:714`. |
| B4 | **confere**. A conjunção `tipo == 'E'`, turno aberto truthy e ausência de `confirmar` grava sessão e retorna renderização antes do registro. O turno aberto é alimentado pelo juiz, com alias `_tad`. `app/ponto/views.py:316`, `app/ponto/views.py:319`, `app/ponto/views.py:329`, `app/ponto/views.py:330`, `app/ponto/services/triagem_batida.py:198`. |
| B5 | **confere**, e há duas cópias adicionais no refresh. Login e ambos os kernels de refresh recusam `situacao != 'ativo'`; a guarda alcança férias e os demais valores diferentes de ativo. São barreiras de autenticação anteriores ao endpoint. `app/api/login_helpers.py:141`, `app/api/views_core.py:173`, `app/api/views.py:116`. |
| B6 | **confere**. O sincronizador aceita `resp.ok`, lê o JSON e apaga a linha sem comparar `data.status`; também apaga em HTTP 400. O v2 online igualmente aceita `r.ok` sem interpretar o status do payload. `app/static/pwa.js:158`, `app/static/pwa.js:162`, `app/static/pwa.js:166`, `app/static/pwa.js:172`, `app/templates/ponto/bater_ponto_v2.html:327`. |
| B7 | **confere**. O selo modal condiciona a exigência à presença textual de `hxConfirmar(`. O selo API procura somente `If` anteriores à primeira chamada nominal da porta, com latitude/longitude/situação na condição e `Response(status=403)`. Não cobre a classe inteira de B1–B6. Há teste que ainda exige a recusa da colisão. `app/core/tests/test_selo_modal_nao_nasce_vazio.py:141`, `app/api/tests/test_l030_selo_porta.py:39`, `app/api/tests/test_l030_selo_porta.py:48`, `app/api/tests/test_l030_selo_porta.py:60`, `app/ponto/tests/test_f9d_t1_caracterizacao_ramo_quente.py:105`, `app/ponto/tests/test_f9d_t1_caracterizacao_ramo_quente.py:120`. |

**(a) Lei, juiz e porta.** L-030 manda conservar a batida; o corte da obra exige censo de toda barreira e proíbe substituí-la por outra. Selfie ausente é cobrança, não condição para nascer a batida. `app/docs/LEIS.md:78`, `CLAUDE.md:140`, `app/docs/CORTES.md:440`, `app/docs/CORTES.md:94`.

**Sem juiz registrado** para admissibilidade da tentativa, identidade de reenvio ou situação autorizada a autenticar. Hoje decidem os filtros e guardas discriminados em B1–B5. O registro da família contém período/par, geofence, selfie e GPS; selfie e geometria são auxiliares existentes, não um juiz de admissibilidade. `app/core/juizes.py:1186`, `app/core/juizes.py:1195`, `app/core/juizes.py:1201`, `app/core/juizes.py:208`.

A porta canônica é `registrar_batida`, declarada em `core/portas.py`, chamada pelos dois kernels e pela web. A cura da classe precisa alcançar a recusa interna da porta, além das cascas. `app/core/portas.py:103`, `app/ponto/registro_batida.py:9

[truncado em 20000 de 57506 caracteres]

## 10/11 20:17 · sessao f379adfa3ffe · BO-0611-05 · leitura · origem=Code

1. **Portas de autenticação e chegada à batida**

| Porta | `Colaborador.situacao` | `data_demissao` | `usuario.is_active` |
|---|---|---|---|
| PWA `/login/` | Exige exatamente `ativo`. | Não achei conferência no helper. | Delega a `authenticate()` do Django; sem teste explícito local. (`app/core/views.py:50`; `app/api/login_helpers.py:122`; `app/api/login_helpers.py:141`) |
| App `/api/auth/login/` | Usa o mesmo helper: exige `ativo`. | Não achei conferência. | Mesma delegação ao Django. (`app/api/urls.py:60`; `app/api/views.py:209`; `app/api/login_helpers.py:122`; `app/api/login_helpers.py:141`) |
| `/api/auth/refresh/` | Exige `ativo` quando há colaborador. | Não achei conferência. | Testa explicitamente: falso retorna 401. Também confere a versão da credencial. (`app/api/views.py:96`; `app/api/views.py:110`; `app/api/views.py:115`) |
| JWT access já emitido | Não achei teste na classe local. | Não achei teste na classe local. | Delega a `JWTAuthentication.get_user()`; depois confere a credencial da senha. (`app/api/credencial.py:177`; `app/api/credencial.py:186`) |
| Sessão Django existente | Não achei nova exigência de `ativo` na porta da batida. | Não achei conferência nessa porta. | Autenticação delegada ao Django; na API também há `SessionAuthentication`. (`app/ponto/views.py:241`; `app/config/settings/base.py:39`; `app/config/settings/base.py:59`) |
| `/api/webview/token/` → `/webview/entrar/` | Emissão exige autenticação existente; consumo não revalida situação. | Não achei conferência. | Consumo não testa explicitamente: valida uso único/validade e chama `login()`. (`app/api/views.py:2995`; `app/api/views.py:3019`; `app/core/views.py:1270`; `app/core/views.py:1276`) |
| `/api/auth/trocar-senha/` | Não achei conferência adicional. | Não achei conferência. | Exige autenticação existente e senha atual; reemite JWTs. (`app/api/views.py:2863`; `app/api/views.py:2880`; `app/api/views.py:2928`) |
| Sessão pelo `/admin/` | Conferência interna da autenticação: não achei na árvore. | Não achei. | Implementação delegada ao Django. (`app/config/urls.py:24`; `app/core/signals.py:4`) |

A implementação interna das bibliotecas para comprovar os testes de `is_active` delegados acima: **não achei**. Não considero comprovado apenas pela chamada à biblioteca. (`app/api/login_helpers.py:122`; `app/api/credencial.py:188`; [nao verificado])

A PWA v2 envia a batida para `/api/ponto/bater/` com sessão/CSRF; permanece também o POST direto em `/ponto/bater/`, que chega a `registrar_batida` pela triagem. (`app/templates/ponto/bater_ponto_v2.html:322`; `app/ponto/views.py:346`; `app/ponto/services/triagem_batida.py:700`)

Na API de batida, há `IsAuthenticated` e exigência de vínculo com Colaborador; não achei veto por `situacao` ou `data_demissao`. A função final `registrar_batida` também não confere esses campos nem `usuario.is_active`. (`app/api/views.py:398`; `app/api/views.py:402`; `app/ponto/registro_batida.py:94`)

O kernel possui as mesmas rotas de login, refresh e batida, com as verificações correspondentes. (`app/api/urls_core.py:9`; `app/api/urls_core.py:12`; `app/api/urls_core.py:14`; `app/api/views_core.py:173`; `app/api/views_core.py:386`)

2. **Desligamento: escritores e efeitos**

- **Porta HTTP:** `POST colaboradores/<cid>/desligar/`, com login e permissão `editar_cadastro`, chama o juiz e o executor de desligamento. (`app/colaboradores/urls.py:52`; `app/colaboradores/views.py:1419`; `app/colaboradores/views.py:1425`; `app/colaboradores/views.py:1430`)
- **Porta agendada:** `efetivar_desligamentos_agendados --apply` seleciona `data_demissao <= hoje`, exclui quem já está desligado e chama o mesmo executor. (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:46`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`)
- **Agendamento futuro:** grava data, motivo e histórico; retorna antes de alterar situação, usuário ou vínculos. Portanto, preencher uma demissão futura não desativa o acesso naquele ato. (`app/colaboradores/services/desligamento.py:82`; `app/colaboradores/services/desligamento.py:85`; `app/colaboradores/services/desligamento.py:101`)
- **Efetivação:** grava `situacao='desligado'`, data e motivo; chama `definir_acesso(..., False)`. Essa função mantém o usuário e altera somente `usuario.is_active`, com auditoria; se não houver usuário, retorna. (`app/colaboradores/services/desligamento.py:104`; `app/colaboradores/services/desligamento.py:111`; `app/core/credenciais.py:107`; `app/core/credenciais.py:113`)
- **Tokens, sessões, bridge e FCM:** exclusão, blacklist, revogação explícita ou limpeza desses registros no desligamento: **não achei**. A operação de credencial chamada salva exclusivamente `is_active`; limpeza de FCM/dispositivos existe separadamente em `resetar_onboarding`. (`app/core/credenciais.py:114`; `app/core/credenciais.py:150`; `app/core/credenciais.py:165`; `app/core/credenciais.py:172`)
- **Outros efeitos:** fecha vínculos; tenta fechar disputas e chamados, preservando disputas com respostas sem veredito; cria chamado `desligamento_revisao` e envia `push_dp`. Esse chamado nasce do desligamento. (`app/colaboradores/services/desligamento.py:122`; `app/colaboradores/services/desligamento.py:139`; `app/colaboradores/services/desligamento.py:154`; `app/colaboradores/services/desligamento.py:163`; `app/colaboradores/services/desligamento.py:187`)

Outra porta de produção que atribua desligado/data de demissão: **não achei**. O admin de Colaborador é somente leitura. Existe a operação inversa: reativação limpa `data_demissao`, grava `ativo` e reativa o usuário. (`app/colaboradores/admin.py:47`; `app/core/admin.py:59`; `app/colaboradores/services/desligamento.py:210`; `app/colaboradores/services/desligamento.py:216`; [nao verificado])

3. **Início de férias e acesso**

O alinhamento pelas datas muda o `AgendamentoFerias.status`; o `save()` espelha o agendamento em ausências aprovadas por dia. Não achei, nesse fluxo, alteração do usuário, sessões, JWT ou FCM. (`app/ferias/services.py:598`; `app/ferias/services.py:604`; `app/ferias/models.py:446`; `app/ponto/services/ausencia.py:1202`)

Escritor de `Colaborador.situacao='ferias'`: **não achei**; o caminho PWA também documenta a retirada desse ramo. Caso esse valor esteja no cadastro, login e refresh o recusam porque exigem exatamente `ativo`. (`app/ponto/services/triagem_batida.py:68`; `app/api/login_helpers.py:141`; `app/api/views.py:115`)

A condição operacional “está de férias hoje” usa as datas do agendamento, excluindo cancelados; não exige exclusivamente `status='em_gozo'`. (`app/ferias/services.py:47`; `app/ferias/services.py:51`; `app/ferias/services.py:57`)

4. **Chamados, avisos e apuração depois da batida**

**Férias: existe tratamento, mas parte dele ocorre antes da gravação.** Na API, o detector chama `bloquear_se_em_ferias` antes de `registrar_batida`; o helper cria chamado DP `colaborador_em_ferias`, deduplicado por colaborador/dia, e tenta enviar `push_dp`. Ele consulta **hoje**, não a data efetiva da batida retroativa. (`app/api/views.py:427`; `app/api/views.py:648`; `app/chamados/detector_anomalias.py:113`; `app/ponto/utils.py:85`; `app/ponto/utils.py:94`; `app/ponto/utils.py:103`; `app/ponto/utils.py:127`)

Depois da gravação, a API passa os motivos — incluindo `em_ferias` — à criação/atualização da disputa e chama o gerador de perguntas. Também devolve aviso ao colaborador dizendo que a batida foi registrada. A descrição do chamado anterior ainda diz “batida recusada”, divergindo desse caminho executado. (`app/api/views.py:749`; `app/chamados/services/disputa_emissao.py:662`; `app/chamados/services/disputa_emissao.py:669`; `app/api/views.py:801`; `app/ponto/services/aviso_da_batida.py:35`; `app/ponto/utils.py:112`)

No POST direto da PWA, o pós-registro consulta `aviso_do_dia`; esse serviço apenas lê férias e devolve a frase, sem criar chamado. (`app/ponto/services/triagem_batida.py:764`; `app/ponto/services/aviso_da_batida.py:55`; `app/ponto/services/aviso_da_batida.py:73`)

**Batida posterior à demissão:** chamado, Pauta DP, pendência ou aviso específico disparado por essa condição: **não achei**. Não há esse motivo no detector lido nem teste de demissão na porta de gravação. (`app/chamados/detector_anomalias.py:19`; `app/chamados/detector_anomalias.py:123`; `app/ponto/registro_batida.py:94`; [nao verificado])

Os demais sítios relevantes do evento e da apuração são:

| Sítio | Leitura/efeito |
|---|---|
| Signals da batida | Recomputam turnos, julgam ontem/hoje e reconciliam chamados; o registrador também pede recálculo do fechamento. (`app/ponto/signals.py:34`; `app/ponto/signals.py:108`; `app/ponto/signals.py:131`; `app/ponto/signals.py:206`; `app/ponto/registro_batida.py:160`) |
| Cobertura e grade | Leem ausências; para folha, somente aprovadas. Férias integrais marcam o dia como ausência e limpam células/órfãs da grade. (`app/ponto/turnos.py:2516`; `app/escala/utils.py:1188`; `app/escala/utils.py:1220`; `app/escala/utils.py:1251`) |
| Precedência | Férias precedem afastamento, atestado, feriado, folga e trabalho. (`app/ponto/precedencia.py:46`; `app/ponto/precedencia.py:396`) |
| Classificador do dia | Quando recebe fato realizado/órfão em dia encerrado e coberto por ausência aprovada, produz `FATO_EM_AUSENCIA`; é uma condição de apuração, não garantia de chamado para qualquer batida isolada. (`app/ponto/supra_juiz.py:379`; `app/ponto/supra_juiz.py:400`) |
| Cadastro do dia | Lê `data_demissao` para não acusar “sem vínculo em dia de trabalho” após a demissão. (`app/ponto/services/cartorio.py:1107`; `app/ponto/supra_juiz.py:193`) |
| Leitor da célula | Dias posteriores à demissão viram `fora_vinculo`; no próprio dia, considera se houve fato. (`app/escala/services/leitor_celula.py:365`; `app/escala/services/leitor_celula.py:386`; `app/escala/services/leitor_celula.py:392`) |
| Emissão do cartório | Exclui `situacao='desligado'` dos portões de cobrança de furo e furo parcial. (`app/ponto/services/cartorio.py:1708`; `app/ponto/services/cartorio.py:1773`) |

Há ainda aviso administrativo `ferias_com_batida` na leitura da situação de férias. A Pauta DP encontrada pertence à **recusa de materialização das férias no save do agendamento**, não a um gancho pós-batida. (`app/ferias/situacao.py:269`; `app/ferias/situacao.py:283`; `app/ponto/services/ausencia.py:1215`; `app/ponto/services/ausencia.py:1245`)

5. **Retenção do fechamento e exportação TXT**

**Sim, existem cortes explícitos no TXT:**

- **Demissão:** `data_demissao <= fim_da_janela` classifica como `fora / rescisao_modulo_proprio`, independentemente de situação ou batida. Demissão posterior ao fim não satisfaz esse corte. (`app/folha/export.py:54`; `app/folha/export.py:60`; `app/folha/export.py:858`)
- **Férias com batida:** cruza agendamentos não cancelados com batidas apuráveis pelo dia local; havendo interseção, classifica `fora / ferias_com_batida`. Não depende de `situacao='ferias'` nem de status exclusivamente `em_gozo`. (`app/ferias/services.py:51`; `app/folha/export.py:491`; `app/folha/export.py:499`; `app/folha/export.py:868`)
- **Aplicação:** a montagem do TXT aceita somente itens classificados como `entra`. (`app/folha/export.py:706`; `app/folha/export.py:718`)

No `FechamentoMensal`, a retenção materializada está em `motivos_espelho`. Seu cálculo consulta o juiz de retenção, cuja varredura corta o fim na demissão: portanto, não audita dias posteriores; no próprio dia da demissão, falta de célula/veredito não gera a retenção `sem_celula`. (`app/ponto/models.py:451`; `app/ponto/services/fechamento.py:1066`; `app/folha/export.py:1200`; `app/folha/export.py:1229`; `app/folha/export.py:1255`)

perguntas de lei: 0

## 10/11 20:15 · sessao 939d1201216d · - · - · origem=Ronald (tmux)

2. **Desligamento**

- Porta HTTP: `POST colaboradores/<cid>/desligar/`, exige login e ação `editar_cadastro`, passa por `julgar_desligamento` e `executar_desligamento` — `app/colaboradores/urls.py:52`, `app/colaboradores/views.py:1419`, `app/colaboradores/views.py:1425`, `app/colaboradores/views.py:1430`.
- Porta agendada: comando seleciona `data_demissao <= hoje` e situação diferente de desligado; com `--apply`, chama o mesmo juiz/executor — `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:46`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`.
- Há declaração do cron às 06:06, com `--apply`; instalação/execução real não verificada — `app/config/crons.py:440`.
- Futuro dentro do teto de 90 dias somente agenda: grava `data_demissao` e motivo, histórico e auditoria; retorna antes de modificar usuário, situação ou vínculos — `app/colaboradores/services/desligamento.py:39`, `app/colaboradores/services/desligamento.py:57`, `app/colaboradores/services/desligamento.py:82`, `app/colaboradores/services/desligamento.py:85`, `app/colaboradores/services/desligamento.py:101`.
- Efetivação grava `situacao='desligado'`, data e motivo, depois chama `credenciais.definir_acesso(..., False)` — `app/colaboradores/services/desligamento.py:103`, `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:111`.
- `definir_acesso` mantém o registro e vínculo de usuário: altera apenas `usuario.is_active`, salva esse campo e registra auditoria; usuário ausente é ignorado — `app/core/credenciais.py:107`, `app/core/credenciais.py:108`, `app/core/credenciais.py:113`, `app/core/credenciais.py:114`, `app/core/credenciais.py:115`.
- Exclusão/revogação explícita de tokens, sessões, dispositivos ou FCM no desligamento: **não achei**; executor usa somente gravação da situação e `definir_acesso`, cuja implementação salva exclusivamente `is_active` — `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:108`, `app/colaboradores/services/desligamento.py:111`, `app/core/credenciais.py:107`.
- Limpeza de FCM/dispositivos existe em outra operação, `resetar_onboarding`, que o desligamento não chama — `app/core/credenciais.py:150`, `app/core/credenciais.py:165`, `app/core/credenciais.py:172`; `app/colaboradores/services/desligamento.py:103`.
- Demais efeitos: fecha vínculos ativos; fecha disputas abertas, preservando respostas sem veredito; tenta encerrar chamados; cria chamado DP de auditoria e faz `push_dp` — `app/colaboradores/services/desligamento.py:122`, `app/colaboradores/services/desligamento.py:139`, `app/colaboradores/services/desligamento.py:140`, `app/colaboradores/services/desligamento.py:154`, `app/colaboradores/services/desligamento.py:163`, `app/colaboradores/services/desligamento.py:187`.
- Django admin não é outra porta atual: `ColaboradorAdmin` herda `SoLeitura`, que nega inclusão, alteração e exclusão — `app/colaboradores/admin.py:47`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`.
- Literalmente, existe também porta que limpa `data_demissao`: reativação HTTP chama executor que grava ativo/data nula e reativa usuário — `app/colaboradores/urls.py:53`, `app/colaboradores/views.py:1405`, `app/colaboradores/views.py:1417`, `app/colaboradores/services/desligamento.py:210`, `app/colaboradores/services/desligamento.py:211`, `app/colaboradores/services/desligamento.py:216`.
- Outra porta de produção que atribua desligado/data de demissão: **não achei**. [nao verificado]

3. **Férias**

- `ferias` é opção de `Colaborador.situacao`, mas escritor de `situacao='ferias'`: **não achei**; comentário do caminho PWA também documenta zero escritores e remoção do ramo antigo — `app/colaboradores/models.py:295`, `app/colaboradores/models.py:298`, `app/ponto/services/triagem_batida.py:68`.
- Início pelas datas deriva `AgendamentoFerias.status`; o cron chama `alinhar_status_por_data`, que trava/recarrega o agendamento e salva apenas `status` — `app/ferias/management/commands/sincronizar_status_ferias.py:37`, `app/ferias/management/commands/sincronizar_status_ferias.py:48`, `app/ferias/services.py:598`, `app/ferias/services.py:600`, `app/ferias/services.py:604`.
- Há declaração de cron diário às 06:15 com `--apply`; execução real não verificada — `app/config/crons.py:442`.
- `AgendamentoFerias.save()` espelha férias em `Ausencia`, inclusive quando muda só status — `app/ferias/models.py:444`, `app/ferias/models.py:446`, `app/ferias/models.py:450`, `app/ferias/models.py:386`.
- O espelhamento considera agendado/em_gozo/concluído e cria ausências aprovadas por dia; não altera usuário, tokens, sessões ou FCM — `app/ponto/services/ausencia.py:1178`, `app/ponto/services/ausencia.py:1180`, `app/ponto/services/ausencia.py:1198`, `app/ponto/services/ausencia.py:1202`.
- O juiz de férias atuais usa cobertura de datas, excluindo cancelados; não depende somente de status `em_gozo` — `app/ferias/services.py:47`, `app/ferias/services.py:51`, `app/ferias/services.py:57`, `app/ferias/services.py:63`.
- Bloqueio automático de credencial no início das férias: **não achei**; `alinhar_status_por_data` somente grava status e o `save` chama espelhamento — `app/ferias/services.py:598`, `app/ferias/services.py:604`, `app/ferias/models.py:446`.

Achados adicionais úteis à pergunta 4:

- `detectar_anomalias` chama `bloquear_se_em_ferias`, acrescentando motivo `em_ferias`; apesar do nome/documentação antiga, conferir consumidor para concluir sobre recusa — `app/chamados/detector_anomalias.py:113`, `app/chamados/detector_anomalias.py:115`, `app/chamados/detector_anomalias.py:118`.
- Esse helper consulta férias de hoje, cria chamado `colaborador_em_ferias` idempotente por colaborador/dia e envia push DP — `app/ponto/utils.py:85`, `app/ponto/utils.py:89`, `app/ponto/utils.py:94`, `app/ponto/utils.py:103`, `app/ponto/utils.py:118`, `app/ponto/utils.py:127`.
- `aviso_do_dia` devolve frase explícita “batida registrada”; recebe motivos do detector ou consulta férias pelas datas — `app/ponto/services/aviso_da_batida.py:35`, `app/ponto/services/aviso_da_batida.py:55`, `app/ponto/services/aviso_da_batida.py:69`, `app/ponto/services/aviso_da_batida.py:73`.
- Pauta DP do espelhamento ocorre por recusa de materialização de férias, durante save do agendamento, e não é por si evento pós-batida — `app/ponto/services/ausencia.py:1209`, `app/ponto/services/ausencia.py:1215`, `app/ponto/services/ausencia.py:1220`, `app/ponto/services/ausencia.py:1245`.
- Leitura administrativa `situacao_de_ferias` detecta batidas dentro do período e acrescenta aviso `ferias_com_batida`; não escreve — `app/ferias/situacao.py:269`, `app/ferias/situacao.py:277`, `app/ferias/situacao.py:283`, `app/ferias/situacao.py:285`.

## 10/11 20:15 · sessao e014fb4f63fb · - · - · origem=Ronald (tmux)

Portas encontradas, com foco nas três condições:

| Porta | `situacao` | `data_demissao` | `usuario.is_active` |
|---|---|---|---|
| PWA `/login/` | Exige exatamente `ativo`; portanto `ferias`, `afastado`, `desligado` impedem novo login. `app/core/urls.py:20`; `app/core/views.py:50`; `app/api/login_helpers.py:141` | Não achei leitura no helper. `app/api/login_helpers.py:107` | Delega a `django.contrib.auth.authenticate`; não achei implementação da biblioteca nesta árvore. `app/api/login_helpers.py:13`; `app/api/login_helpers.py:122` |
| API `/api/auth/login/` | Mesmo helper, exige `ativo`. `app/api/urls.py:60`; `app/api/views.py:209`; `app/api/login_helpers.py:141` | Não achei leitura. `app/api/login_helpers.py:107` | Mesma delegação a `authenticate`; checagem interna da biblioteca [nao verificado]. `app/api/login_helpers.py:122` |
| Renovação `/api/auth/refresh/` | Exige `ativo`, quando há Colaborador. `app/api/urls.py:63`; `app/api/views.py:115` | Não achei leitura nessa renovação. `app/api/views.py:76` | Confere explicitamente e devolve 401 se falso. `app/api/views.py:96` |
| JWT access já emitido | Não achei checagem de situação na classe local; chama `super().get_user()` e confere versão da senha. `app/api/credencial.py:186` | Não achei checagem. `app/api/credencial.py:186` | Delega a `JWTAuthentication`; checagem interna da biblioteca [nao verificado]. `app/api/credencial.py:177`; `app/api/credencial.py:188` |
| Sessão Django já existente | Não achei gate local por situação; `/ponto/bater/` usa `login_required`. `app/ponto/views.py:241` | Não achei checagem nessa porta. `app/ponto/views.py:241` | Delega à autenticação/sessão Django e, na API, `SessionAuthentication`; biblioteca [nao verificado]. `app/config/settings/base.py:39`; `app/config/settings/base.py:56`; `app/config/settings/base.py:59` |
| Bridge `/api/webview/token/` → `/webview/entrar/` | Emissão exige autenticação existente; consumo não revalida situação. `app/api/views.py:2995`; `app/api/views.py:3019`; `app/core/views.py:1271` | Não achei leitura nas duas funções. `app/api/views.py:2997`; `app/core/views.py:1256` | Não achei checagem explícita no consumo: após validar token, chama `login(request,t.usuario)`. `app/core/views.py:1271`; `app/core/views.py:1276` |
| `/api/auth/trocar-senha/`, que também emite novos JWTs | Não achei checagem; exige autenticação existente e senha atual. `app/api/urls.py:61`; `app/api/views.py:2863`; `app/api/views.py:2880`; `app/api/views.py:2928` | Não achei leitura nessa função. `app/api/views.py:2865` | Sem checagem explícita adicional, usa autenticação da API. `app/api/views.py:2864` |
| Sessão pelo `/admin/`, se o usuário puder autenticar pelo admin | Não achei implementação local da autenticação do admin. Montagem em `app/config/urls.py:24`; existência dessa porta documentada em `app/core/signals.py:4` | Não achei. `app/config/urls.py:24` | Implementação da biblioteca [nao verificado]. `app/config/urls.py:24` |

A configuração da API aceita JWT **ou sessão Django**; o access dura 12h e refresh 30 dias, com rotação. `app/config/settings/base.py:32`; `app/config/settings/base.py:47`.

A PWA v2 posta efetivamente em `/api/ponto/bater/`, com CSRF da sessão. Também permanece o POST direto em `/ponto/bater/`, que chama `ramo_registrar_es` e daí `registrar_batida`. `app/templates/ponto/bater_ponto_v2.html:322`; `app/ponto/views.py:346`; `app/ponto/services/triagem_batida.py:700`.

Na entrada da API de batida, exige `IsAuthenticated` e vínculo com Colaborador; situação cadastral não veta registro nessa view. A triagem PWA também não tem mais ramo de férias, e a porta final `registrar_batida` não verifica os três campos. `app/api/views.py:398`; `app/api/views.py:406`; `app/ponto/services/triagem_batida.py:68`; `app/ponto/registro_batida.py:94`.

Há um segundo URLconf do kernel com as mesmas portas login/refresh/batida e mesmas verificações locais: `app/config/urls_core.py:6`; `app/api/urls_core.py:9`; `app/api/urls_core.py:12`; `app/api/urls_core.py:14`; `app/api/views_core.py:153`; `app/api/views_core.py:173`; `app/api/views_core.py:196`; `app/api/views_core.py:386`.

O bridge valida uso único e TTL de 120s, mas não reconsulta os três campos no consumo. `app/core/models.py:327`; `app/core/views.py:1270`; `app/core/views.py:1271`; `app/core/views.py:1276`.

As versões declaradas são Django 5.2.14, DRF 3.17.1 e simplejwt 5.5.1; não achei o código dessas bibliotecas nem `AUTHENTICATION_BACKENDS` custom nesta árvore, então não há arquivo:linha local para comprovar o comportamento interno de `is_active`. `app/requirements.txt:13`; `app/requirements.txt:14`; `app/requirements.txt:15`; [nao verificado].

Não achei porta servidor de biometria/código de login. [nao verificado]

## 10/11 20:14 · sessao f0ed2325bcc3 · BO-0611-05 · sonda · origem=Code

Competência informada: **2026-10-21 a 2026-11-20**. [não verificado]

| Campo da sonda | Empresa 1 | Empresa 2 | Empresa 3 | Empresa 4 | Empresa 29 | Referência |
|---|---:|---:|---:|---:|---:|---|
| colabs | 7 | 732 | 163 | 35 | 2 | [não verificado] |
| desligados | 4 | 302 | 58 | 11 | 2 | [não verificado] |
| desligados_com_demissao_na_competencia | 0 | 11 | 7 | 0 | 0 | [não verificado] |
| desligados_sem_data_demissao | 0 | 0 | 0 | 1 | 0 | [não verificado] |
| desligados_com_usuario_ativo | 0 | 131 | 9 | 3 | 0 | [não verificado] |
| situacao_ferias | 0 | 0 | 0 | 0 | 0 | [não verificado] |
| agendamentos_ferias_tocando_competencia | 1 | 9 | 6 | 0 | 0 | [não verificado] |

| Empresa | janela_he, conforme saída | Referência |
|---|---|---|
| 1 | `(True, '2026-09-21', False)` | [não verificado] |
| 2 | `(True, '2026-08-21', True)` | [não verificado] |
| 3 | `(True, '2026-08-21', True)` | [não verificado] |
| 4 | `(True, '2026-08-21', True)` | [não verificado] |
| 29 | `(False, 'None', False)` | [não verificado] |

Contagens apresentadas sem divisão por empresa: [não verificado]

| Campo da sonda | Contagem | Referência |
|---|---:|---|
| batidas_desde_2108_com_data_local_apos_demissao | 20 | [não verificado] |
| batidas_dentro_de_ferias_agendadas_desde_2108 | 17 | [não verificado] |

A saída fornecida não contém linhas explícitas de erro nem indicação de contagens que a sonda não conseguiu realizar. [não verificado]

## 10/11 20:14 · sessao 8ce5f87320ca · - · - · origem=Ronald (tmux)

5. **Sim, o TXT exclui esses casos por regras explícitas.** O veredito vem de `classificar_export`; a montagem só aceita colaboradores com `status='entra'`. (`app/folha/export.py:757`, `app/folha/export.py:706`, `app/folha/export.py:718`)

- **Demissão:** `sai_pela_rescisao` testa `data_demissao is not None` e `data_demissao <= fim_janela`; o resultado vira `fora / rescisao_modulo_proprio`, independentemente de `situacao` ou existência de batida. Demissão posterior ao fim da janela não satisfaz esse teste. (`app/folha/export.py:54`, `app/folha/export.py:60`, `app/folha/export.py:858`)
- **Férias com batida:** cruza as datas de `AgendamentoFerias` com as batidas apuráveis pelo dia local; ignora agendamento cancelado, mas não exige `status='em_gozo'` nem `Colaborador.situacao='ferias'`. Encontrando dias, classifica `fora / ferias_com_batida` e registra a lista em `dias_ferias_com_batida`. (`app/ferias/services.py:47`, `app/ferias/services.py:51`, `app/folha/export.py:491`, `app/folha/export.py:499`, `app/folha/export.py:868`)
- **Aplicação efetiva:** a porta HTTP deriva os fechamentos em leitura, chama `classificar_export` e passa os mesmos objetos e itens para montar o TXT. (`app/folha/views.py:207`, `app/folha/views.py:212`, `app/folha/views.py:268`)
- **Retenção do espelho:** o campo materializado chama-se `motivos_espelho`; o recalculador o alimenta com `motivos_retencao_espelho`. Esse caminho corta a janela na demissão; assim, sua varredura não alcança dias posteriores à demissão. No próprio dia da demissão, ausência de célula/veredito não gera `sem_celula`. (`app/ponto/models.py:451`, `app/ponto/services/fechamento.py:1066`, `app/folha/export.py:1200`, `app/folha/export.py:1229`, `app/folha/export.py:1255`)
- **Universo do fechamento:** inclui qualquer situação diferente de `desligado`; para `desligado`, exige `data_demissao >= início da janela`. Assim, férias entram no universo; desligado com demissão anterior à janela ou nula fica fora desse universo. (`app/ponto/services/fechamento.py:431`, `app/ponto/services/fechamento.py:439`)
- **Visibilidade administrativa:** o painel lê o mesmo classificador e exibe contadores “Bateu em ferias” e “† Rescisao”; esse trecho somente monta a resposta HTML. (`app/folha/views.py:589`, `app/folha/views.py:593`, `app/folha/views.py:638`, `app/folha/views.py:641`)
- **Distinção:** as `travas_aprovacao` são `conta_vazia`, `turno_aberto` e `sem_ancora_12x36`; a exclusão por rescisão/férias ocorre na classificação de entrada do TXT. (`app/folha/export.py:822`, `app/folha/export.py:825`, `app/folha/export.py:827`, `app/folha/export.py:858`, `app/folha/export.py:870`)

## 10/11 20:11 · sessao b76f94c08b6a · - · - · origem=Ronald (tmux)

Lista nominal Python foi entregue nas três mensagens: **353 funções, 146 arquivos de `app/`**, pelo método sintático definido; consumidores indiretos de projeções e execução dinâmica não estão demonstrados por esse número [nao verificado].

Os **13 templates leitores diretos**, contando o JS embutido como parte do template:

| Arquivo:linha | Forma |
|---|---|
| `app/templates/colaboradores/detalhe.html:178` | Iteração `batidas_geo`; `b.timestamp`, `tipo`, `origem` em :180–184. |
| `app/templates/ponto/batidas_offline_pendentes.html:53` | Iteração de Batida; comparação `tipo == 'E'` :60; `timestamp_dispositivo` truthiness :71; `timestamp_gap_ms is not None` :78, comparação ±300000 :79; filtro juiz `tem_gps` :96. |
| `app/templates/ponto/materializar_saida_retroativa.html:25` | Truthiness das relações `batida_entrada` e `batida_gerada`; timestamp :27; pk :32; negação da relação :37. |
| `app/templates/ponto/regularizar_retorno.html:18` | `batida_entrada.timestamp` em template. |
| `app/templates/ponto/espelho.html:76` | Iteração `ultimas_batidas`; timestamp :77–78; listas `dia.batidas` :244 e comparação de tipo :246. |
| `app/templates/core/dashboard_supervisor.html:40` | Iteração de entradas `turnos_abertos`; `b.timestamp` :44. |
| `app/templates/core/_d2_feed.html:3` | Iteração; foto por truthiness :6; comparação `tipo == 'E'` :14; filtro `tem_gps` :16. |
| `app/templates/core/_d2_alertas.html:23` | Iteração `turnos_abertos`; `t.timestamp` :26. |
| `app/templates/chamados/fio_colaborador.html:116` | Timestamp/coordenadas de `ultima_geo`; iteração de históricos em :125–126. |
| `app/templates/colaboradores/partials/_calendario_grade.html:121` | Iteração `dia.batidas`, timestamp/tipo/pk, links para portas e procedência. |
| `app/templates/ponto/partials/geo_detalhe_turno.html:4` | `batida.colaborador` e `batida.timestamp`; amostras em :29 são PingGeo, não novas Batida. |
| `app/templates/ponto/partials/painel_justificativa.html:129` | Truthiness de relações `batida_entrada`/`batida_gerada` e timestamps :129–130. |
| `app/templates/core/dashboard2.html:236` | JS lê `d.batida`; truthiness conjunta de coordenadas :242/:287; histórico `h.timestamp` :260; timestamp da batida :277. |

**Um leitor adicional em static:** `app/static/js/geo-colab-map.js:29`, função `abrirGeoColab`: `parseFloat` dos dados de latitude/longitude e recusa por `isNaN(...) || isNaN(...)` em :39. A fila PWA continua no censo separado do estado IndexedDB: `app/static/pwa.js:113`, :132, :162, :172, :190.

**Escritores confirmados:** quatro funções primitivas, cinco operações, todas em `app/ponto/registro_batida.py`:

- `registrar_batida`: `create` em :152 e :157.
- `anotar`: `save` em :254.
- `apagar`: `delete` em :291.
- `semear`: `bulk_create` em :335.

`registrar_batida`, `anotar` e `apagar` já estão nas 353 funções leitoras; `semear` foi excluída dessa lista por ser somente escritora. `anexar_foto` escreve Batida indiretamente por `anotar` (:307), não constitui outra operação primitiva de escrita.

**Admin adicional confirmado:** `app/ponto/admin.py:11–15` registra `BatidaAdmin`, com leitura reflexiva de colaborador/timestamp/tipo/origem/aprovação e filtros. Não entrou nas 353 porque a leitura ocorre pelo framework. `AlertaGeofenceAdmin` também expõe a relação `batida` em `readonly_fields`, em `app/ponto/admin.py:30–34`; convém declarar esse leitor reflexivo separado, sem apresentá-lo como descoberta pelo AST.

Os programas fora de `app/` identificados pelo pai ficam separados do subtotal 353: os dois bins leitores e os programas de máscara/selo. Confirmei que a máscara é **escritora efetiva**, não mero registro: alias `R` em `bin/sombra_mascara_pessoal.py:12`, mapa `R.NULO` em :17 e `M.objects.update(...)` em :20; o selo lê por SQL `is not null` em `bin/sombra_selo_pessoal.py:55–58`.

## 10/11 20:08 · sessao 129941409e00 · - · - · origem=Ronald (tmux)

União nominal consolidada: **76 unidades identificadas = 47 anteriores + 29 adicionais**, deduplicadas por função Python, classe de admin, template, arquivo JS ou script. O número cobre os sítios listados; **fechamento de todos os consumidores genéricos de Chamado/Pergunta/Followup: [nao verificado]**.

As 29 adicionais são:

| Grupo | Unidades adicionais e forma |
|---|---|
| AlertaGeofence — 11 | `app/ponto/models.py:258` (`ainda_vale`: `is None`, conserva alerta); `app/ponto/admin.py:31` (filtro por tipo, campos readonly); `app/core/cell.py:15` (queryset ativo → conjunto); `app/core/views.py:199` (`dashboard`, conjunto por batida); `app/core/views.py:407` (`dashboard_supervisor`, comparação `tipo='ponto_fora'`); `app/core/views.py:783` (`painel_tv`, conjunto por colaborador); `app/core/views.py:1167` (`dashboard_dados`, conjunto por batida); `app/ponto/management/commands/reconciliar_alertas_geofence.py:33` (queryset, `is None` em `:42`, escrita `obsoleto_em` em `:52`); `app/ponto/management/commands/testar_pwa.py:291` (ensaio, `.exists()` em `:294`); `app/ponto/management/commands/zerar_dados_operacionais.py:47` (ensaio, `count`, exclusão total em `:85`); `app/templates/core/dashboard_supervisor.html:90` (truthiness da latitude do alerta). |
| Sombra — 3 | `bin/sombra_mascara_pessoal.py:12` (alias `R`, `R.NULO` → `objects.update` em `:17`/`:20`; JSON → `bulk_update` em `:44`/`:62`); `bin/sombra_regras_pessoais.py:36` (`json_`, recursão e regex de coordenada numérica → `None` em `:43`); `bin/sombra_selo_pessoal.py:55` (SQL `IS NOT NULL`; JSONPath por `R.CHAVES_GEO` em `:90`). As regras abrangem Batida/Alerta em `bin/sombra_regras_pessoais.py:75` e `:77`, e contexto de Chamado em `:145`. |
| Template — 1 | `app/templates/chamados/detalhe_local.html:274`: comparação explícita `modulo_origem == "gps_ausente"` e inclusão do relatório. |
| Leitores do módulo/rótulo — 9 | `app/chamados/catalogo/natureza.py:38` (`eh_acionavel`, `not in`); `:44` (`dono_do_chamado`, registro versus admin); `app/chamados/services/desenlace.py:38` (`rotulo`, membership/lookup); `:68` (`desenlace`, comparação de pilha e lookup); `app/chamados/services/verbos.py:158` (`_descritiva`, lookup por subcategoria); `:186` (`coletar_lente`, módulo e lookups em `:206`); `app/api/views.py:60` (`_motivo_label`, `.get`); `app/api/views_mensageria.py:1161` (`api_mensageria_decidir`, lookup por subcategoria); `app/chamados/management/commands/auditar_invariantes_chamados.py:1068` (queryset `modulo_origem__in=MODULOS_AUDITORIA`). |
| Cadeia efetiva de escrita de Chamado — 5 | `app/chamados/models.py:1051` (`_evento_nascimento`, natureza → EMITIR/REGISTRAR); `:1060`/`:1064`/`:1095` (`criar`, motor, estado inicial e INSERT); `app/chamados/services/nota.py:110` (`anexar_ocorrencia`, delega fusão); `:123` (`_fundir_contexto`, alias `_ctx`); `app/chamados/services/contexto.py:144` (`fundir_contexto`, união de listas, ausência por `k not in ctx` em `:155`, `save` em `:163`). |

A normalização de texto zero pode mudar também o responsável pelo chamado: `gps_ausente` pertence a `MODULOS_AUDITORIA`, enquanto `gps_fora_geofence` pertence aos módulos de validação administrativa; o nascimento segue essa natureza (`app/chamados/catalogo/natureza.py:18`; `app/chamados/catalogo/natureza.py:29`; `app/chamados/models.py:1051`).

Declarações auditadas, **sem somar como leitores de instância**: módulo `app/chamados/catalogo/modulos.py:162`; pergunta `app/chamados/catalogo/perguntas.py:88`; motivo `app/chamados/catalogo/motivos.py:42`; rótulo/comentário `app/chamados/services/verbos.py:77` e `:150`; rótulo `app/chamados/services/desenlace.py:21`; pergunta legada `app/chamados/models.py:3598`; mapas API `app/api/views.py:47` e `app/api/views_core.py:47`; opção do formulário `app/templates/chamados/partials/modal_fio.html:402`. `describe._catalogo` apenas inspeciona essas definições e fica fora do total (`app/core/management/commands/describe.py:114`).

`Advertencia.detalhe_json` recebe a cópia das coordenadas (`app/chamados/services/acoes_chamado.py:197`); outro leitor nominal dessas chaves não foi localizado. **Leitura dinâmica/reflexiva desse JSON: [nao verificado]**.

## 10/11 20:05 · sessao 2f185fdf9cd5 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Delta implementado: o replay consome os seis argumentos fixados no ensaio e aceita somente `criada=False` com o mesmo PK. Resultado divergente desfaz seu próprio savepoint e registra `replay-divergente` pelo observador existente. (`chamados/services/disputa_emissao.py:484`, `:688`, `:694`, `:698`, `:702`)

Dona fechada não recebe save; a defesa para dona aberta usa exclusivamente `agregar_estrangeiras`, sem contexto. `_replay` saiu dos dois helpers. O ramo de agregação permaneceu com seu fluxo anterior. (`chamados/services/disputa_emissao.py:315`, `:413`, `:592`, `:615`, `:701`)

Arquivos tocados nesta volta:

- [disputa_emissao.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:484): coletor e replay fixado.
- [test_bo_disputa_volta5_replay_red_memoria.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5_replay_red_memoria.py:25): caracterização invertida e renomeada.
- [test_bo_disputa_volta5_memoria.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5_memoria.py:156): argumentos exatos, divergências, defesa da aberta e mutantes.
- [test_bo_disputa_volta5.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5.py:135): cenário ORM solicitado.
- [test_bo_disputa_volta4_memoria.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta4_memoria.py:44): expectativas anteriores de replay ajustadas.
- [test_bo_disputa_sem_pergunta_memoria.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:45): callbacks aceitam o coletor; mutante acompanha três atomics.
- [test_bo_disputa_sem_pergunta.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_sem_pergunta.py:58): callback de falha encaminha o coletor.
- [test_medida_turno_do_par.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/core/tests/test_medida_turno_do_par.py:267): registro das linhas deslocadas.

F1–F3 e destino do ensaio:

| Item | Conferência |
|---|---|
| F1 | Confirmado antes da alteração: o RED terminou em `2 != 1`. A precedência permanece data do motivo antes de `ultima_batida`; o replay agora não chama resolvedor nem gerador. (`disputa_emissao.py:355`, `:361`, `:370`, `:693`; `test_bo_disputa_volta5_replay_red_memoria.py:25`) |
| F2 | Não encontrei contraexemplo nesse fluxo: qualquer aberta conduz à agregação e retorna antes do nascimento, inclusive duas abertas incoerentes. A eleição da pergunta usa o colaborador da disputa; a recuperação também exige esse colaborador. (`disputa_emissao.py:612`, `:615`, `:633`, `:687`; `chamados/models.py:2846`, `:2907`) |
| F3 | Confere quanto às escritas: reuso conserva a dona e altera a pergunta; inserção usa `disputa=disputa`. Precisão: a porta também **lê** a disputa antes disso para derivar episódio/identidade. (`chamados/models.py:2866`, `:2875`, `:2965`, `:2982`, `:3008`, `:3014`) |
| R2: pai desfeito | Não pode ser destino devolvido pelo derivador: nasce com categoria `disputa_supervisao`, excluída literalmente da consulta. Portanto não foi necessário substituir esse destino por `None`. (`disputa_emissao.py:201`, `:639`) |

A porta ainda deriva episódio BATIDA do contexto da dona. Se isso produzir outra identidade, a guarda rejeita o resultado; intervalo é MARCO e não recebe esse episódio. (`chamados/models.py:2866`, `:3460`; `disputa_emissao.py:694`)

T1 passou com chamadas `[(2, DIA), (dona.pk, DIA)]`, zero reatar, DS/chamados `[1, 0]`, uma pergunta total, reaponte preservado e zero save na dona. (`test_bo_disputa_volta5_replay_red_memoria.py:59`) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_replay_red_memoria.py
```

```text
Ran 1 test
OK
```

Os seis módulos em memória passaram, incluindo os testes anteriores ajustados. (`test_bo_disputa_sem_pergunta_memoria.py:291`, `:378`; `test_bo_disputa_volta4_memoria.py:44`, `:85`; `test_bo_disputa_volta5_memoria.py:382`) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  chamados.tests.test_bo_disputa_sem_pergunta_memoria \
  chamados.tests.test_bo_disputa_volta2_memoria \
  chamados.tests.test_bo_disputa_volta3_memoria \
  chamados.tests.test_bo_disputa_volta4_memoria \
  chamados.tests.test_bo_disputa_volta5_memoria \
  chamados.tests.test_bo_disputa_volta5_replay_red_memoria
```

```text
Ran 47 tests
OK
```

T2: todos os oito mutantes tiveram controle verde com 15 testes e morreram por assertion, sem erro de montagem. Os dois novos reproduzem resolução no replay e aceitação de inserção. (`test_bo_disputa_volta5_memoria.py:26`, `:54`, `:75`, `:394`) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante reentrada
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante salva_fechada
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante sem_guarda_push
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante nasce_sem_pergunta
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante eleicao_antes_trava
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante dedup_cala_replay
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante replay_re_resolve
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante replay_insere
```

Saída final de cada comando, emitida pelo harness: (`test_bo_disputa_volta5_memoria.py:401`)

```text
MUTANTE MORTO: reentrada assertions RED: 1
MUTANTE MORTO: salva_fechada assertions RED: 5
MUTANTE MORTO: sem_guarda_push assertions RED: 3
MUTANTE MORTO: nasce_sem_pergunta assertions RED: 4
MUTANTE MORTO: eleicao_antes_trava assertions RED: 1
MUTANTE MORTO: dedup_cala_replay assertions RED: 6
MUTANTE MORTO: replay_re_resolve assertions RED: 2
MUTANTE MORTO: replay_insere assertions RED: 1
```

T3 foi escrito com relógio fixo, porta real, dona fechada com data D+1, `qtd`/`ultima_em`, batida D e dois atos idênticos. Exige igualdade da linha inteira da dona, das linhas DS/chamados/perguntas, pertencimento da pergunta, zero push e zero reatar. **Execução ORM: [nao verificado].** (`test_bo_disputa_volta5.py:135`, `:142`, `:160`, `:173`) [nao verificado]

T4: `[814, 815] → [809, 810]`; `[1298] → [1293]`. A catraca em memória passou com 35 testes. (`core/tests/test_medida_turno_do_par.py:267`)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import pathlib, types, unittest
p = pathlib.Path('core/tests/test_medida_turno_do_par.py')
suite = types.ModuleType('suite_turno') [nao verificado]
suite.__file__ = str(p.resolve()) [nao verificado]
source = p.read_text().replace(
    'from django.test import SimpleTestCase',
    'from unittest import TestCase as SimpleTestCase')
exec(compile(source, str(p), 'exec'), suite.__dict__) [nao verificado]
unittest.main(module=suite, verbosity=1) [nao verificado]
PY
```

```text
Ran 35 tests
OK
```

Quem escreve e quem lê os campos relevantes ao delta:

| Campo | Escritor → leitor |
|---|---|
| Argumentos da pergunta | Catálogo/resolvedor/gerador produzem texto, tipo, opções, destino e dia; coletor conserva exatamente os seis argumentos e PK; replay os consome. (`disputa_emissao.py:426`, `:444`, `:458`, `:484`, `:488`, `:688`) |
| `P.pk`, `P.disputa_id`, `criada` | Porta insere ou devolve existente; núcleo identifica dona; replay exige mesmo PK e falso; wrapper distingue perguntas criadas de retornadas. (`chamados/models.py:3008`, `:3014`; `disputa_emissao.py:569`, `:694`, `:778`) |
| `P.data_evento`, célula, destino | Porta consulta identidade, rechaveia/costura e relavra; replay conserva o dia pedido no ensaio. (`chamados/models.py:2907`, `:2940`, `:2965`, `:2982`, `:3003`; `disputa_emissao.py:693`) |
| `DS.motivos` | Núcleo cria/agrega; modelo remove/reabre acrescentando; núcleo consulta perguntas locais para admissão. (`disputa_emissao.py:561`, `:583`, `:654`; `chamados/models.py:2031`, `:2063`, `:2156`) |
| `DS.contexto`, `qtd`, `ultima_em` | Merge escreve metadados e persistência salva contexto; resolvedor lê data/batida; porta BATIDA lê episódio. Replay não faz esse merge. (`disputa_emissao.py:526`, `:534`, `:535`, `:586`, `:355`, `:370`, `:688`; `chamados/models.py:3463`) |
| `DS.fechada_em` | Modelo fecha/reabre; núcleo, helper e wrapper leem abertura; receivers leem valor, sem exigir transição. (`chamados/models.py:1932`, `:2149`; `disputa_emissao.py:575`, `:595`, `:773`; `chamados/signals.py:28`, `:129`, `:153`) |
| Coletor/causas | Gerador escreve `reusos`; replay lê e registra divergência; helper escreve `agregada`; wrapper lê; observador recebe causas. (`disputa_emissao.py:488`, `:558`, `:597`, `:602`, `:688`, `:698`, `:770`) |

Leitor a leitor de `motivos`, para a política de motivos locais já presente na cópia:

| Leitor | Efeito/pressuposto |
|---|---|
| `api/foto_recebida.py:63`, `:70` | Seleção acompanha dona aberta, mas também exige `ultima_batida` correspondente; transferir só motivo não garante passar essa guarda. (`:67`) |
| `colaboradores/signals.py:74` | Remoção acompanha a dona aberta selecionada. (`:77`) |
| `escala/signals.py:158` | Remoção de âncora acompanha a dona aberta selecionada. (`:161`) |
| `chamados/models.py:1997` | Referência documental; execução exige abertura e motivo presente. (`:2031`, `:2036`) |
| `auditar_disputas.py:88` | Exige contexto dos motivos mantidos. |
| `auditar_disputas.py:142` | Exibe lista mantida. |
| `auditar_disputas.py:169` | Compara com perguntas locais, com exceções contextuais. (`:182`, `:188`) |
| `auditar_invariantes_chamados.py:50` | Resolve dias dos motivos mantidos; seleciona abertas sem nenhuma linha de pergunta. (`:62`) |
| `chamados/relatorios_disputa.py:52` | Rótulos da lista mantida. |
| `chamados/services/acoes_disputa.py:149` | Mensagem da lista mantida. |
| `chamados/services/fio.py:680` | Mensagem da lista mantida. |
| `pauta_aparelhos_compartilhados.py:49` | Cobertura também exige contexto de dispositivo; motivo sozinho não transfere essa evidência. (`:52`, `:55`) |
| `alertar_disputas_pendentes.py:82` | Log da lista mantida. |
| `templates/chamados/detalhe_local.html:301` | Exibição da lista mantida. |
| `templates/chamados/partials/modal_fio.html:326` | Aviso depende de âncora na lista. |
| `colaboradores/services/drawer.py:75` | Comentário; execução usa perguntas vivas. (`:78`) |

Neste delta, a dona fechada conserva motivos/contexto; portanto esses leitores não mudam por escrita nesses campos. Efeitos reais da alteração da pergunta: **[nao verificado]**. (`disputa_emissao.py:595`, `:701`; `test_bo_disputa_volta5_replay_red_memoria.py:65`)

A resposta da leitura na **cópia** é **D+1**: `_contexto_do_ato` começa pelo contexto guardado; receber somente `ultima_batida` preserva `motivo.data`. A agregação faz esse merge antes do gerador; o resolvedor escolhe a data explícita e não entra no fallback da batida. Não curei esse comportamento. (`disputa_emissao.py:526`, `:527`, `:538`, `:617`, `:620`, `:355`, `:361`, `:370`)

Atribuir esse resultado especificamente à base histórica `6c0517eb9`: **[nao verificado]**. Não consultei Git.

Para a sessão executar, além dos seis módulos em memória acima, ficam estes módulos; a integração ORM permanece **[nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_bo_disputa_sem_pergunta \
  chamados.tests.test_bo_disputa_volta5 \
  chamados.tests.test_porta_disputa_emissao \
  chamados.tests.test_porta_chamado_e3 \
  chamados.tests.test_sla_pelo_juiz \
  chamados.tests.test_selo_derivacao_pela_data_da_pergunta \
  chamados.tests.test_disputa_s127 \
  chamados.tests.test_c7_red_minimos \
  chamados.tests.test_invariantes_disputa \
  chamados.tests.test_hx_disputa_muda_sem_ata \
  chamados.tests.test_hx_disputa_muda \
  chamados.tests.test_e1_v15f \
  chamados.tests.test_e1_v7_catracas \
  core.tests.test_medida_turno_do_par
```

Não rodei Django/PostgreSQL, receivers reais, concorrência nem ruff; certificação pela suite e revisão final: **[nao verificado]**. [nao verificado]

## 10/11 20:03 · sessao 129941409e00 · - · - · origem=Ronald (tmux)

Consolidado A enviado com os sítios e o inventário contado. Pontos que precisam aparecer no resultado:

- **A6 é parcial:** a divergência texto/número existe (`app/ponto/services/geofence.py:145`; `app/ponto/registro_batida.py:15`), mas o teste atual **já caracteriza** `'0'`/`'0.0'` como truthy (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:79`).
- **A8 não comprova falha:** os dois leitores atuais delegam ao juiz (`app/templates/chamados/partials/_relatorio_geo.html:31`; `app/templates/chamados/partials/modal_fio.html:481`); a fixture sempre fornece as chaves (`app/ponto/tests/test_bo0611_05_templates_geo.py:132`). Consequência de chave ausente em execução: **[nao verificado]**.
- **A9 é parcial:** o módulo `templates_geo` individual não inclui zero no laço (`app/ponto/tests/test_bo0611_05_templates_geo.py:178`), mas o teste do juiz já exercita zeros nos contextos (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:218`). O detector fixa zero como evidência (`app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135`). RED histórico contra a base: **[nao verificado]**.
- `anotar` **não escreve coordenadas**: sua allowlist não as contém e campos externos são recusados (`app/ponto/registro_batida.py:203`; `app/ponto/registro_batida.py:241`). `semear` pode recebê-las genericamente, mas exige banco de ensaio (`app/ponto/registro_batida.py:326`; `app/ponto/registro_batida.py:335`).
- O leitor reflexivo do admin deve constar: `BatidaAdmin` usa `SoLeitura`, que expande todos os campos do modelo (`app/ponto/admin.py:12`; `app/core/admin.py:74`). O realtime não transporta geo: o payload explícito tem somente tipo, identificadores e timestamp (`app/ponto/realtime.py:32`).
- **Contagem consolidada:** 35 unidades diretas em produção + 1 escritor potencial de ensaio + 11 unidades de ingresso/transporte = **47 unidades identificadas**, antes dos delegadores command/signal. A lista nominal e as linhas foram enviadas nas mensagens anteriores; não apresentar esse número como censo de todos os consumidores genéricos de `AlertaGeofence`/`ChamadoColaborador`, cujo alcance depende de incluir a expansão de A7. **[nao verificado: fechamento desse universo genérico]**
- Casa verificável: **COMPLEMENTO de O254 / contrato batida–juiz**, `app/docs/BACKLOG.md:1458`, e BO pai `app/docs/BACKLOG.md:1873`. Os identificadores filhos pedidos não aparecem literalmente no BACKLOG desta cópia. **[nao verificado: casas registradas apenas em 6c0517eb9]**
- Lei/juiz/padrão: L-002/L-007/L-030 (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:65`; `app/docs/LEIS.md:78`); juiz vivo `app/core/juizes.py:1201`; corte de migração e zero inválido `app/docs/CORTES.md:442`; padrão **Single Writer Principle**, `app/docs/GLOSSARIO.md:12`.
- **perguntas de lei: 0**, para migração e coerência de tipo já cobertas pelo corte. Se a cura incluir remover a guarda de posto, há **1** pergunta adicional: “Batida sem GPS e sem posto deve gerar `AlertaGeofence.sem_gps` e chamado `gps_ausente`?” Hoje o escritor exige posto (`app/ponto/services/geofence.py:270`), e o corte proíbe alterar o veredito nessa migração (`app/docs/CORTES.md:442`).

## 10/11 20:03 · sessao b76f94c08b6a · - · - · origem=Ronald (tmux)

Bloco B consolidado, somente leitura.

**Conferência B1–B7**

| Achado | Veredito e forma |
|---|---|
| B1 | **confere**: `app/templates/ponto/bater_ponto_v2.html:287` faz `if(!_fotoBlob)` e retorna `false` em :290; `v2Confirmar` encerra antes do POST em :312. A API já trata selfie ausente como sinal: `app/api/views_core.py:418`, :430–434; outra casca `app/api/views.py:431`, :443. |
| B2 | **parcial**: API usa queryset de mesmo tipo no intervalo inclusivo **±5 min**, `.first()` e truthiness `if dupla`, retornando HTTP 200 sem chamar a porta (`app/api/views_core.py:508`, :510, :514, :517; réplica `app/api/views.py:521`, :523, :527, :530). Web usa **[agora−5 min, agora]**, não ±5 min (`app/ponto/services/triagem_batida.py:662`, :663, :669–670). Não é demonstrado que cada tentativa seja um fato distinto: a idempotência é exigida em `CLAUDE.md:175`; a chave exata existe em `app/ponto/models.py:106` e `app/ponto/registro_batida.py:120`. |
| B3 | **parcial**: não impede “chegar a registrar_batida”: chega e a **própria porta** recusa com `(None, conflito)` (`app/ponto/registro_batida.py:145`, :150–151). Janela 300 s com escala/marcos, senão 60 s (`app/ponto/registro_batida.py:21`, :30–32); ainda há exceção quando `_casa_marco_proprio` responde True (:35–49, :145). As APIs consultam a mesma janela `_janela_oposto as _jop`, queryset excluindo tipo e retratadas, e respondem 200 `ignorada_intervalo` (`app/api/views_core.py:658–678`; réplica `app/api/views.py:666–686`). A web consulta oposta em **±60 s fixos**, sem `retratada_em` no filtro (`app/ponto/services/triagem_batida.py:714–725`), portanto pode devolver id nulo quando a porta recusou no envelope de 300 s. |
| B4 | **confere**: `tipo == 'E'`, truthiness de `context.get('turno_aberto')` e ausência de `request.POST.get('confirmar')` gravam sessão, marcam `aguardando_confirmacao=True` e fazem `return render` antes do ramo registrador (`app/ponto/views.py:316–330`; ramo em :346–350). A presença do turno é alimentada perguntando a `turno_aberto_de as _tad`, não somente pela última marcação (`app/ponto/services/triagem_batida.py:198–204`). |
| B5 | **confere**, com complemento: login lê `getattr(colab, 'situacao', 'ativo') != 'ativo'` em `app/api/login_helpers.py:141` e retorna erro :142. Refresh também tem cópia em **ambos** os kernels: `app/api/views_core.py:173–177` e `app/api/views.py:116–120`. Logo inclui férias e qualquer outro valor diferente de ativo; a mensagem do refresh chama tudo de “desligado”. São guardas de autenticação anteriores ao endpoint de bater, não 403 internos ao kernel da batida. |
| B6 | **confere**: `resp.ok` em `app/static/pwa.js:158` aceita o 200; lê JSON em :159 e não consulta `data.status`, apagando IndexedDB em :162. Também apaga 400 em :166–174. Online v2 faz `if(r.ok) v2Sucesso(...)` sem ler payload (`app/templates/ponto/bater_ponto_v2.html:327`). |
| B7 | **confere**: exigência do selo modal depende da presença textual de `hxConfirmar(` (`app/core/tests/test_selo_modal_nao_nasce_vazio.py:141–143`). Selo API só visita `If` anteriores à primeira chamada nominal `registrar_batida`, cuja condição mencione latitude/longitude/situacao e contenha `Response(status=403)` (`app/api/tests/test_l030_selo_porta.py:39–64`). Não detecta selfie JS, sessão/render, `ja_registrada` 200, recusa interna da porta, login nem refresh. Os próprios testes ainda exigem a colisão: `app/ponto/tests/test_f9d_t1_caracterizacao_ramo_quente.py:105`, :115–120. |

**(a) Lei, juiz e porta**

- L-030: `app/docs/LEIS.md:78`, `CLAUDE.md:140–141`; corte da obra em `app/docs/CORTES.md:440` manda censo de **toda** barreira e proíbe barrar por outro motivo.
- **Sem juiz registrado para “esta tentativa pode ser materializada?”**, “qual identidade de reenvio?” ou “qual situação pode autenticar?”. O registro da família batida contém período/par, geofence, selfie e GPS (`app/core/juizes.py:1186–1201`), não essas perguntas. Hoje decidem os `If` e filtros acima.
- Juízes auxiliares existentes: selfie ausente em `app/core/juizes.py:1195` → `app/ponto/registro_batida.py:80`; geometria/tipo em `app/core/juizes.py:208–209`. Selfie ausente é **cobrança**, explicitamente no corte `app/docs/CORTES.md:94`, não autorização para impedir registrar.
- Porta canônica: `app/core/portas.py:103` → `app/ponto/registro_batida.py:94`. Web chama em `app/ponto/services/triagem_batida.py:700–713`; APIs em `app/api/views_core.py:639–654` e `app/api/views.py:647–662`.
- `JUIZES.md` é lookup e manda prevalecer o vivo (`app/docs/JUIZES.md:10–15`); sua tabela de geometria está em :20–25.

**(b) Estado e decisor**

- Campo novo: **não**; escolhas novas de campo: **não**, para uma cura que apenas encaminhe o fato à porta existente. Selfie já é opcional (`app/ponto/models.py:78`); foto/GPS ausentes já seguem API (`app/api/views_core.py:417–434`, :639–654).
- Linhas que hoje não nascem **por esses caminhos**: **sim**. Exemplos são a tentativa interceptada pelo retorno de :517, pelo render de `app/ponto/views.py:330` ou pela recusa de `app/ponto/registro_batida.py:151`. Não se deve dizer que a combinação é impossível **globalmente**: autoridade humana já pode criar a colisão (`app/ponto/registro_batida.py:150–156`).
- Muda quem decide admissibilidade: **sim**, ao retirar o veto dos clientes/cascas/guardas para encaminhar à porta; não requer inventar juiz. A definição exata da cura do login geral permanece [nao verificado], porque o corte da batida não especifica permissões para todas as outras operações autenticadas (`app/docs/CORTES.md:440`).
- Existência e quantidade real de batidas hoje perdidas: **depende de dado: contagem de tentativas distintas retidas em cada barreira; colisões por 60/300 s; fila descartada por status; recusas de login/refresh por situação**. O dado fornecido não contém essas contagens [nao verificado].

**(c) Censo fechado de estados locais; limite do censo global**

Método: grep em todos os `.py`, `.html` e `.js` da aplicação para nomes dos retornos, sessão/confirmação e store IndexedDB; AST de produção preservando `ImportFrom.asname`, incluindo `management/`; verificadas buscas específicas em `config/crons.py` e `*/signals*.py`. Não há comparação literal de `ja_registrada`/`ignorada_intervalo` em leitor runtime achado: os usos literais runtime são os seis escritores abaixo; os consumidores de sucesso leem `Response.ok`. Testes são separados, não contados como produção (`app/api/tests/test_l030_selo_porta.py:37` mostra por que a forma precisa ser distinguida do significado).

**28 sítios sintáticos distintos nos três estados locais abaixo**, contados uma vez por `arquivo:linha`; `pop` conta como leitura/escrita no mesmo sítio:

1. **Retornos — 8 sítios**:
   - Escritores `ja_registrada`: `app/api/views_core.py:518`, `app/api/views.py:531`, `app/ponto/services/triagem_batida.py:670` — literal em dict de Response/JsonResponse.
   - Escritores `ignorada_intervalo`: `app/api/views_core.py:669`, `app/api/views.py:677`, `app/ponto/services/triagem_batida.py:721` — literal em dict.
   - Leitores genéricos de sucesso: `app/static/pwa.js:158` e `app/templates/ponto/bater_ponto_v2.html:327` — truthiness de `.ok`, sem comparação de status do payload.

2. **Sessão/confirmação — 15 sítios**:
   - Escritores: `app/ponto/views.py:319` (`session['confirmacao_pendente'] = dict`), :327 (`confirmacao_foto_b64 = ...`), :328 (`confirmacao_foto_nome = ...`), :329 (`context['aguardando_confirmacao']=True`).
   - Leitor contextual: `app/ponto/views.py:233` — truthiness de `.get`.
   - Leitores/deletores: `app/ponto/services/triagem_batida.py:505` (`confirmar and session.get`), :506 (`pop`), :508 e :510 (`cp.get`), :512 (`confirmar and session.get`), :513 e :514 (`pop`, último com default).
   - Templates: `app/templates/ponto/bater_ponto_v2.html:64` (`elif ... or aguardando_confirmacao`); `app/templates/ponto/bater_ponto.html:161` (`and not aguardando_confirmacao`) e :174 (`if aguardando_confirmacao`).
   - Formulário confirmado nasce em `app/templates/ponto/triagem/turno_confirmar.html:11` e `app/templates/ponto/bater_ponto.html:183` e é lido em `app/ponto/views.py:273`, :316; estes são **quatro sítios adicionais se incluir também o campo POST `confirmar`**, total local então **32**. O total 28 acima deliberadamente cobre estados de retorno, sessão/contexto e fila; não incorpora esse campo de entrada.

3. **Fila — 5 sítios**:
   - `app/static/pwa.js:113` — `.add` da linha.
   - :132 — `.getAll` para envio.
   - :162 e :172 — `.delete` por sucesso/400.
   - :190 — `.count` para indicador.
   - Definição do store em :54 é DDL local, separada das cinco operações sobre linhas.

**Guardas/decisores adicionais que a cura toca**, separados da contagem de estados acima:
- selfie: `app/templates/ponto/bater_ponto_v2.html:287` (nega truthiness do blob) e :312 (nega retorno da função);
- situação/autenticação: `app/api/login_helpers.py:141`, `app/api/views_core.py:173`, `app/api/views.py:116` — comparação `!= 'ativo'`;
- duplicidade: querysets `.first()` em `app/api/views_core.py:510`, `app/api/views.py:523`, `app/ponto/services/triagem_batida.py:663`, seguidos de truthiness;
- colisão da porta: `app/ponto/registro_batida.py:107` (janela), :113–115 (`mesma is not None`), :120–122 (mesma chave), :139–145 (`exclude(tipo=tipo)`, `.exists()`), :150 (`not autoridade_humana`);
- confirmação: `app/ponto/views.py:316` — conjunção tipo/truthiness/ausência de confirmar.

**Todos os escritores Batida sob o instrumento existente**: cinco operações do AST `core/censo_escritas`:
`app/ponto/registro_batida.py:152` create; :157 create; :254 save; :291 delete; :335 bulk_create. Fontes do instrumento: `app/core/censo_escritas.py:6–46`, :451, :469; porta declarada `app/core/portas.py:103`. O próprio censo tem inferência por nome e operações não atribuíveis, portanto não é prova absoluta de fluxo dinâmico (`app/core/censo_escritas.py:38–46`).

**Não fechar como “28/32 leitores+escritores de Batida”**: esse é apenas o inventário dos estados locais. Novas linhas também alimentam apuração, tela, comandos, signals e dinheiro. Meu AST amplo achou **218 unidades candidatas em 128 arquivos**, incluindo aliases, mas contém metadados/constantes e não acompanha listas recebidas por parâmetro; portanto **total global de leitores de novas Batida: [nao verificado]**. Exemplos comprovados adicionais:
- `app/ponto/signals.py:35`, :40, :49, :132, :139 — receivers de criação/remoção; `instance` e `created/raw`.
- `app/ponto/signals.py:19`, :108 — timestamp/colaborador por parâmetro.
- `app/ponto/turnos.py:2876` — queryset do universo apurável.
- `app/ponto/services/fechamento.py:633`, :811 — `_bap`, alias de `batidas_apuraveis`.
- `app/folha/export.py:499` — `batidas_apuraveis`.
- `app/ponto/management/commands/marcar_foto_ausente_retro.py:66`, :74 — queryset e juiz da selfie.
- `app/ponto/registro_batida.py:177–178` — chama recálculo.
- Uso de universo por parâmetro é limite que a própria documentação admite (`CLAUDE.md:160–163`), portanto não declarar o AST candidato como censo completo.

**(d) Casa**

**COMPLEMENTO de BO-0611-05**, `app/docs/BACKLOG.md:1873`, pelo corte `app/docs/CORTES.md:440`; não “BO simples”: já há guardas distribuídas por JS/template, três caminhos de gravação e autenticação. Não encontrei os IDs `TEM-GPS-LEITORES` ou `RESTO-DA-CLASSE` no BACKLOG desta cópia [nao verificado: resultado negativo de grep, sem linha citável]. O usuário forneceu que foram mapeados antes em outra árvore; não transformar isso em linha inexistente nesta.

A descrição do pai em `BACKLOG.md:1873` ainda diz API 403 sem coordenada e template interrompendo por GPS: **não confere no vivo desta árvore**. APIs chegam à porta com latitude/longitude (`app/api/views_core.py:639–654`; `app/api/views.py:647–662`); o requisito GPS do v2 termina em `return true` mesmo sem obter coordenadas (`app/templates/ponto/bater_ponto_v2.html:292–297`). A selfie continua barrando, conforme B1.

Não encontrei motivo para classificar como RAIZ-DA-DISPUTA/E/FIO/MOTOR: estas obras têm objetos diferentes explicitados em `app/docs/BACKLOG.md:1839`, :1862–1864; a casa direta já é BO-0611-05.

**(e) Padrão**

`Single Writer Principle`, termo literal em `app/docs/GLOSSARIO.md:12`, com `Architecture Conformance Test` em :15 para selar a **classe** das barreiras. A cura tem de preservar idempotência e a porta, não apagar indiscriminadamente qualquer retorno de reenvio (`CLAUDE.md:175`; `app/ponto/registro_batida.py:120–122`).

**(f) Perguntas de lei candidatas**

`perguntas de lei: 2`

1. **Qual identidade distingue um reenvio do mesmo fato de uma nova marcação do mesmo tipo em instante distinto dentro dos cinco minutos?** A lei exige zero barreira e idempotência (`app/docs/LEIS.md:78`; `CLAUDE.md:175`), mas o envelope temporal vem de decisão do código/comentário (`app/api/views_core.py:491–513`), enquanto a unicidade armazenada é por instante exato (`app/ponto/models.py:106`). Não perguntar novamente se selfie, GPS ou tipo oposto podem barrar: L-030 já responde.
2. **Para quem tem situação diferente de ativo, a L-030 exige permitir login/refresh geral, ou garantir somente acesso à porta de registrar uma batida, conservando as outras restrições da conta?** O corte existente disciplina gravação da batida (`app/docs/CORTES.md:440`) e o corte de credencial disciplina validade/revogação (`app/docs/CORTES.md:184`); nenhum dos dois explicita essa extensão de permissões. Hoje os três `!= 'ativo'` decidem (`app/api/login_helpers.py:141`; `app/api/views_core.py:173`; `app/api/views.py:116`).

## 10/11 20:01 · sessao a12c0edbce44 · - · - · origem=Ronald (tmux)

## BO-0611-05-DINHEIRO-DESLIGADO-FERIAS

**C1: parcial.** Os dois caminhos chegam a `registrar_batida`: `app/api/views_core.py:640` e `app/ponto/services/triagem_batida.py:701`. O primeiro já não bloqueia pela situação (`app/api/views_core.py:393`); o segundo transforma o sinal em justificativa/aprovação para revisão, continuando a gravar (`app/ponto/services/triagem_batida.py:693`). A porta cria `Batida` e dispara fechamento (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:160`).

A função de evento calcula a competência do fato e chama o escritor sem recusa genérica por exportação (`app/ponto/services/fechamento.py:1809`, `app/ponto/services/fechamento.py:1810`), por `on_commit` (`app/ponto/services/fechamento.py:1824`). **Mas não se pode afirmar que toda batida produz dinheiro novo:** o universo exclui desligado com demissão anterior à janela ou sem data (`app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:439`); a cadeia conserva guarda temporária de **09/2026** (`app/ponto/services/fechamento.py:36`), aplicada mediante exclusão dos colaboradores de empresas exportadas (`app/ponto/services/fechamento.py:560`, `app/ponto/services/fechamento.py:564`).

O teste fornecido no código cobre ambos os kernels e situações, verifica uma Batida e o retorno de seu id; não afirma nenhum valor de fechamento (`app/api/tests/test_l030_batida_pela_porta.py:39`, `app/api/tests/test_l030_batida_pela_porta.py:44`, `app/api/tests/test_l030_batida_pela_porta.py:50`). Situação textual `ferias` no teste não comprova férias efetivas em `Ausencia`/agendamento (`app/api/tests/test_l030_batida_pela_porta.py:28`, `app/api/tests/test_l030_batida_pela_porta.py:52`).

**(a)** Leis: L-030 manda conservar a batida (`app/docs/LEIS.md:78`); L-113 revoga a recusa de recálculo em exportada, preserva TXT anterior/trilha/DIFF de frota e altera expressamente L-092/L-052 (`app/docs/LEIS.md:153`). Logo a falta de recusa genérica não é defeito por si; a função trata exportada como carimbo `warning`, não como veto (`app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:541`).

Juiz registrado do valor do par: `MotorBase.calcular_periodo` (`app/core/juizes.py:1188`); efeitos e cobertura da ausência: `efeito_vigente` e `cobertura_ausencia_periodo` (`app/core/juizes.py:252`, `app/core/juizes.py:253`); férias hoje: `em_gozo_hoje` (`app/core/juizes.py:262`); entregue/trancado: `competencia_entregue` e `periodo_trancado` (`app/core/juizes.py:327`, `app/core/juizes.py:328`). A tabela anexa local chama cálculo de `motor_calculo_v2` (`app/docs/JUIZES.md:29`).

**Sem juiz registrado específico para “uma batida de desligado/em férias deve produzir qual dinheiro?”** O caminho atual seleciona o universo, lê a cobertura para folha e monta o motor com a régua existente (`app/ponto/services/fechamento.py:439`, `app/ponto/services/fechamento.py:683`, `app/ponto/services/fechamento.py:735`). Não substituir essa cadeia por uma política presumida. [nao verificado]

Portas: batida `registrar_batida`; evento `_fechamento_por_evento` → `recalcular_por_evento`; escritor financeiro `recalcular_fechamento_mes` (`app/ponto/registro_batida.py:94`, `app/ponto/registro_batida.py:175`, `app/ponto/services/fechamento.py:1798`, `app/ponto/services/fechamento.py:448`). FM nasce por `get_or_create`, salva aberto e atualiza os demais status; `DiaPago` é reconstruído por exclusão e `bulk_create` (`app/ponto/services/fechamento.py:898`, `app/ponto/services/fechamento.py:1137`, `app/ponto/services/fechamento.py:1160`, `app/ponto/services/dia_pago.py:359`).

**(b)** Cura não especificada pelo C1; não cabe inventá-la. Para o **censo solicitado**, não há alteração proposta de estado ou decisor. O estado Batida e os efeitos de fechamento já existem nesta árvore (`app/ponto/registro_batida.py:157`, `app/ponto/services/fechamento.py:1810`). Uma cura futura que passe a recusar, excluir ou mudar o valor dessas batidas pode alterar decisor/combinação de estados, mas seu escopo permanece [nao verificado].

**(c)** Censo auditável de **chamadas diretas**, não um total de todos os leitores/escritores das tabelas: AST em memória de `app/**/*.py`, excluindo `tests/` e `migrations/`, resolvendo `ImportFrom`, `as` e import de módulo; grep complementar. Resultado: **5 chamadas de evento, 21 do escritor mensal, 3 do universo e 5 da competência do fato**. A cifra completa de leitores/escritores financeiros, incluindo templates/JS, permanece **[nao verificado]**; não somar essas chamadas e rotular o resultado de censo de estado.

As cinco chamadas de evento:

- `app/ponto/registro_batida.py:178`: evento da batida.
- `app/ponto/portas/celula.py:458`: evento do ato de célula.
- `app/ponto/portas/he.py:202`: decisão de HE.
- `app/chamados/services/validacao.py:361`: folga aceita.
- `app/chamados/services/validacao.py:446`: resposta validada.

As 21 chamadas do escritor, com sua forma:

- Escrita padrão: `app/folha/management/commands/simular_folha.py:123`; `app/ponto/management/commands/aplicar_09_corte_b.py:212`; `app/ponto/management/commands/celula_veredito_velho.py:149`; `app/ponto/management/commands/desvio_o68b.py:94`; `app/ponto/management/commands/diff_reclassificar_partido.py:161` e `:206`; `app/ponto/management/commands/folga_que_sumiu.py:70`; `app/ponto/management/commands/plano_b_no_dinheiro.py:110` (**alias de módulo `_fech`**); `app/ponto/management/commands/recalcular_fechamento.py:118`; `app/ponto/portas/regen_exportada.py:179`; `app/ponto/services/fechamento.py:1810`; `app/ponto/views.py:1252`.
- Leitura `somente_leitura=True`: `app/folha/management/commands/simular_folha.py:130`; `app/folha/views.py:208` (**alias `_recalc`**); `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135`; `app/ponto/management/commands/apurar_furo_da_exportada.py:111`; `app/ponto/services/diff_frota.py:84` e `:87`; `app/ponto/services/fechamento.py:1759`.
- Leitura de FM **com escrita de DiaPago**, `somente_leitura=True, lavrar_dias_pagos=True`: `app/ponto/management/commands/diff_calculador.py:140` (**alias `_recalc`**); `app/ponto/management/commands/lavrar_dias_pagos.py:106`.

Leitores do universo: escritor mensal e duas telas (`app/ponto/services/fechamento.py:554`, `app/ponto/views.py:1078`, `app/ponto/views.py:1114`). A forma decisiva é queryset `~Q(situacao='desligado') | (Q(situacao='desligado') & data_demissao__gte=inicio_da_janela)` (`app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:439`).

`post_save` de Batida também recomputa turnos e publica tempo real (`app/ponto/signals.py:34`, `app/ponto/signals.py:39`); julgamento da célula passa por `cartorio.julgar_celula` (`app/ponto/signals.py:85`, `app/ponto/signals.py:93`). Não há chamada direta adicional de `recalcular_por_evento` nesses signals pelo AST/grep. [nao verificado]

**Não usar como fato a promessa “cron da madrugada rega”** dos comentários de `recalcular_por_evento`: o cadastro declara expressamente que `recalcular_fechamento` **não tem cron**, por desenho (`app/config/crons.py:1130`). O comentário do command que promete recusa de exportada também está desatualizado em relação ao escritor (`app/ponto/management/commands/recalcular_fechamento.py:83`, `app/ponto/services/fechamento.py:539`).

**(d)** Encaixe sugerido: **COMPLEMENTO de O-PROJECOES**, cuja descrição cobre fonte, versão e reconstrução de espelho/DiaPago/fechamento (`app/docs/BACKLOG.md:737`). Não há prova de que este C1 trate de pareamento/dia para encaixá-lo em MOTOR-LE-O-TURNO (`app/docs/BACKLOG.md:1839`). A questão distinta de custo inline já possui casa própria (`app/docs/BACKLOG.md:734`).

**(e)** Padrão: **CQRS com projeções reconstruíveis** (`app/docs/GLOSSARIO.md:24`), mantendo **Single Writer Principle** (`app/docs/GLOSSARIO.md:12`).

**(f)** `perguntas de lei: 0`

O C1 não formula cura nem cenário financeiro sem resposta; a ausência de medição não é lacuna de lei. L-113 já responde a exportada; L-030 responde a conservação da batida (`app/docs/LEIS.md:153`, `app/docs/LEIS.md:78`).

**Depende de dado:** contagem de Batidas efetivamente novas por kernel/situação factual/competência; quantas caem fora do universo, na guarda de 09 exportada, ou produzem alterações em FM/DiaPago; DIFF por campo e competência contra gravado. Nenhuma dessas contagens consta do dado fornecido. [nao verificado]

## 10/11 19:59 · sessao 10354500077f · - · - · origem=Ronald (tmux)

Leitura concluída, sem editar arquivos.

- **F2 confere:** trava e seleção de aberta antecedem os ramos; havendo qualquer aberta, inclusive duas incoerentes, o código entra em agregação e retorna antes do nascimento. Assim, no ramo de nascimento, a única aberta criada pelo próprio ensaio não pode ser dona estrangeira. (`chamados/services/disputa_emissao.py:613`, `:615`, `:618`, `:636`, `:656`)
- **F3 confere quanto à FK da dona:** o reuso escreve célula, destino e relavratura da pergunta existente; retorna a mesma dona. A inserção usa `disputa=disputa`. Ressalva literal: `disputa` também participa das **leituras** de identidade/episódio anteriores; não entra exclusivamente na inserção como insumo de leitura. (`chamados/models.py:2866`, `:2875`, `:2965`, `:2982`, `:3003`, `:3008`, `:3014`)
- **R2, pai desfeito:** `_derivar_chamado_destino` exclui `categoria='disputa_supervisao'`; o pai criado pelo ensaio tem precisamente essa categoria. Portanto, esse pai não pode ser destino devolvido pelo derivador; não é necessário substituir esse destino por `None` nesse cenário. (`chamados/services/disputa_emissao.py:198`, `:201`, `:640`, `:642`)

O resolvedor ainda pode derivar episódio BATIDA do contexto da dona dentro da porta. A guarda de mesmo PK e `criada=False` deve desfazer qualquer resultado divergente. Intervalo pertence à família MARCO e não recebe esse episódio. (`chamados/models.py:2866`, `:3452`, `:3460`; `chamados/services/declaracao_texto.py:42`)

Na **cópia atual**, agregação simples escolhe **D+1**: o merge começa pelo contexto guardado, recebe somente `ultima_batida` e preserva `motivo.data`; o resolvedor consome essa data antes do fallback pela batida. O comportamento antecede o delta solicitado. A base histórica `6c0517eb9` não foi consultada: **[nao verificado]**. (`chamados/services/disputa_emissao.py:530`, `:531`, `:541`, `:619`, `:623`, `:328`, `:354`, `:358`, `:361`, `:370`)

| Campo | Escritores e leitores relevantes |
|---|---|
| `DS.motivos` | Núcleo cria/agrega; modelo poda e reabre acrescentando motivo. Núcleo admite pela existência de perguntas locais. (`chamados/services/disputa_emissao.py:564`, `:576`, `:595`, `:656`; `chamados/models.py:1961`, `:2031`, `:2063`, `:2156`) |
| `DS.contexto`, `qtd`, `ultima_em` | Merge escreve metadados; persistência grava contexto. Resolvedor lê data/batida; derivador lê data; porta BATIDA lê episódio. Busca de acessos específicos encontrou leitura de `qtd` no próprio merge e teste API; nenhum consumidor específico de `ultima_em` nessa busca. (`chamados/services/disputa_emissao.py:191`, `:355`, `:370`, `:530`, `:538`, `:539`, `:588`; `chamados/models.py:3463`; `api/tests/test_api_bater_pwa.py:91`) |
| `DS.fechada_em` | Modelo fecha/reabre; núcleo e wrapper distinguem aberta/fechada; os três receivers verificam valor, sem exigir transição. (`chamados/models.py:1932`, `:2149`; `chamados/services/disputa_emissao.py:578`, `:598`, `:616`, `:778`; `chamados/signals.py:28`, `:129`, `:153`) |
| Pergunta: PK/FK, dia, célula, destino | Porta elege pergunta e distingue reuso/inserção; reuso conserva dona. Escritas de dia também passam pelas portas de fio. (`chamados/models.py:2907`, `:2921`, `:2940`, `:2965`, `:2982`, `:3008`, `:3014`; `chamados/services/fio.py:790`, `:804`, `:979`) |
| `criada` e pergunta devolvida | Gerador separa pergunta retornada de realmente criada; wrapper só notifica motivos de perguntas realmente criadas e locais. (`chamados/services/disputa_emissao.py:484`, `:499`, `:510`, `:511`, `:514`, `:783`) |

Impacto leitor a leitor da política de motivos locais:

| Leitor | Consequência |
|---|---|
| `api/foto_recebida.py:63`, `:70` | Seleção acompanha a dona aberta, mas ainda exige `contexto.ultima_batida.id` correspondente; transferência só de motivo não garante passar essa guarda. (`:67`, `:68`, `:73`) |
| `colaboradores/signals.py:74` | Remoção de `posto_desativado` acompanha a dona aberta selecionada. (`:71`, `:77`) |
| `escala/signals.py:158` | Remoção de `ancora_ausente` acompanha a dona aberta selecionada. (`:155`, `:161`) |
| `chamados/models.py:1997` | É documentação; guardas executáveis verificam fechamento e motivo presente. (`:2031`, `:2033`, `:2036`) |
| `auditar_disputas.py:88` | Auditoria passa a exigir contexto dos motivos mantidos. (`:89`, `:91`) |
| `auditar_disputas.py:142` | Exibe a lista mantida. |
| `auditar_disputas.py:169` | Compara lista com perguntas locais; a interpretação já corresponde ao pertencimento local, com exceção contextual para emissão impossível. (`:182`, `:188`, `:191`) |
| `auditar_invariantes_chamados.py:50` | Resolve dias dos motivos mantidos; I7 continua selecionando abertas sem nenhuma linha de pergunta. (`:51`, `:62`, `:63`) |
| `chamados/relatorios_disputa.py:52` | Exibe rótulos dos motivos mantidos. |
| `chamados/services/acoes_disputa.py:149` | Mensagem usa os motivos mantidos. |
| `chamados/services/fio.py:680` | Mensagem usa os motivos mantidos. |
| `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49` | Cobertura pode mudar; também exige contexto com motivo/prefixo, que não acompanha a transferência somente de motivos. (`:52`, `:53`, `:55`) |
| `ponto/management/commands/alertar_disputas_pendentes.py:82` | Log usa lista mantida. |
| `templates/chamados/detalhe_local.html:301` | Exibe lista mantida. |
| `templates/chamados/partials/modal_fio.html:326` | Aviso depende da presença local de âncora. |
| `colaboradores/services/drawer.py:75` | Comentário descreve agregado que não poda; execução usa perguntas vivas, sem leitura direta da lista. (`:77`, `:78`) |

Para o **delta de replay com dona fechada**, esses leitores não devem mudar por escrita em motivos/contexto da dona, pois a decisão é persistir nada nela. A alteração da pergunta pode ter efeitos próprios pelos leitores da pergunta; execução desses efeitos permanece **[nao verificado]**. (`chamados/services/disputa_emissao.py:578`; `chamados/models.py:3006`, `:3007`)

## 10/11 19:56 · sessao 51de6528d488 · MAPA-FILA-BO · censo · origem=Code

O vínculo desta cópia ao hash informado não pôde ser conferido por Git: a árvore disponível não tem repositório Git reconhecido. [não verificado]

Em **(b)**, “não cria estado” significa que a cura usa campos, valores e combinações já admitidos pelas portas existentes; a ocorrência efetiva dessas combinações em produção continua [não verificado]. Distingo mudança do decisor de apresentação da mudança do juiz de negócio, conforme a separação entre autoridade e escritor registrada em `app/core/juizes.py:3`.

## DISPUTA-ZUMBI-1110

**(a) Lei, juiz e porta.** Aplicam-se **L-002**, testemunha lê a mesma autoridade, e **L-007**, escritor único com trilha (`app/docs/LEIS.md:60`, `app/docs/LEIS.md:65`). O corte C7 declara **`DisputaSupervisao.pode_fechar`**, registrado no código e no lookup (`app/docs/CORTES.md:272`, `app/core/juizes.py:279`, `app/docs/JUIZES.md:57`).

C7 exige **nenhuma resposta sem veredito e nenhum fio vivo**; a porta canônica é **`DisputaSupervisao.fechar`**, que grava conjuntamente fechamento, autor, resolução e motivo (`app/chamados/models.py:1853`, `app/chamados/models.py:1932`). Os carimbos das perguntas passam por `carimbar` e `carimbar_identidade` (`app/chamados/models.py:3103`, `app/chamados/models.py:3168`).

**Conferência do relato:**

- **Confere:** `lavrar_sem_turno_aberto` já consulta C7 e chama `fechar` (`app/chamados/reconciliador.py:411`, `app/chamados/reconciliador.py:412`).
- **Confere:** o explicador calcula “pode fechar” somente por `not _fio_vivo(ps)`, omitindo a guarda adicional de respostas sem veredito de C7 (`app/chamados/services/explicador.py:136`, `app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).
- **“Fechamento só por beco” não confere globalmente:** existem caudas de fechamento na reconciliação e na materialização (`app/chamados/reconciliador.py:1170`, `app/chamados/services/materializacao.py:1316`).
- **“7 carimbadores sem C7” não está demonstrado como sete caminhos sem fechamento:** `encerrar_fantasma` alcança C7 pelo reconciliador; materialização tem caudas compartilhadas; `remover_motivo` chama diretamente `fechar` (`app/chamados/reconciliador.py:1147`, `app/chamados/reconciliador.py:1170`, `app/chamados/services/materializacao.py:1378`, `app/chamados/services/passivo_validado.py:405`, `app/chamados/models.py:2072`).

**(b) Cria estado: não. Muda quem decide: sim, no explicador.** Os campos e motivos de fechamento já existem, e a porta já produz a combinação fechada/resolução/motivo (`app/chamados/models.py:1757`, `app/chamados/models.py:1773`, `app/chamados/models.py:1780`, `app/chamados/models.py:1932`). Porém o bool apresentado deixaria de ser decidido pela negação local de `fio_vivo` e passaria a consumir C7; o juiz legal de fechamento permaneceria o existente (`app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).

**(c) Censo do bool cujo decisor muda: 7 ocorrências = 1 escritor + 6 leitores.**

| Papel e forma | Sítio |
|---|---|
| Escritor: atribuição `pode = not _fio_vivo(ps)` | `app/chamados/services/explicador.py:137` |
| Leitor: condição de ternário | `app/chamados/services/explicador.py:138` |
| Leitor: truthiness no `if` | `app/chamados/services/explicador.py:139` |
| Leitor: exportação na chave `pode_fechar` | `app/chamados/services/explicador.py:163` |
| Leitor: truthiness de `dict.get` | `app/chamados/services/desenlace.py:63` |
| Leitor: truthiness em template, cor | `app/templates/chamados/partials/modal_fio.html:434` |
| Leitor: truthiness em template, mensagem | `app/templates/chamados/partials/modal_fio.html:445` |

O transporte preserva o dict: cache, atribuição a `ch.explicacao` e passagem ao alias `desenlace as _desen`; não o contei novamente como decisão (`app/chamados/services/fio.py:312`, `app/chamados/services/fio.py:314`, `app/chamados/services/fio.py:386`, `app/chamados/services/fio.py:403`).

Método: grep global de `pode_fechar`, `por_que_nao_fecha` e transportes de `explicacao`, seguido dos imports, aliases e usos locais de `pode`; as sete ocorrências estão enumeradas acima. Não apareceram consumidores adicionais em commands, crons, signals ou JS de `static/` para esse bool calculado. [não verificado por referência negativa arquivo:linha]

**Censo complementar da costura**, separado dos sete acima: AST de chamadas de método encontrou **13 chamadas de carimbo em 12 funções e 12 chamadas diretas a C7**; a unidade é `ast.Call`, independentemente do nome do receptor:

| Forma | Sítios |
|---|---|
| `.carimbar(...)`: 8 | `app/chamados/models.py:3287`; `app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:595`; `app/chamados/services/disputa_emissao.py:1863`; `app/chamados/services/disputa_emissao.py:1937`; `app/chamados/services/materializacao.py:561`; `app/chamados/services/materializacao.py:1277`; `app/chamados/services/materializacao.py:1431` |
| `.carimbar_identidade(...)`: 5 | `app/chamados/models.py:2061`; `app/chamados/models.py:3064`; `app/chamados/services/fio.py:1359`; `app/chamados/services/materializacao.py:924`; `app/chamados/services/materializacao.py:1099` |
| `fechada_em is None and …pode_fechar()`: 6 | `app/chamados/reconciliador.py:411`; `app/chamados/reconciliador.py:611`; `app/chamados/services/becos.py:56`; `app/chamados/services/disputa_emissao.py:1963`; `app/chamados/services/materializacao.py:585`; `app/chamados/services/materializacao.py:1316` |
| `elif …pode_fechar()` | `app/chamados/reconciliador.py:1170` |
| Ternário sobre C7 | `app/chamados/services/becos.py:33` |
| `if not …pode_fechar()`: 2 | `app/chamados/services/competencia_trancada.py:153`; `app/chamados/services/competencia_trancada.py:202` |
| Atribuição seguida de truthiness | `app/chamados/services/disputa_emissao.py:1278`, `app/chamados/services/disputa_emissao.py:1282` |
| `if disp and disp.pode_fechar()` | `app/ponto/services/entrada_sincronizada.py:58` |

O signal genérico de salvamento da pergunta agenda a porta de becos após o commit; esse classificador exige pai resolvido e ausência de perguntas visíveis antes de consultar C7 (`app/chamados/signals.py:227`, `app/chamados/signals.py:238`, `app/chamados/services/becos.py:29`, `app/chamados/services/becos.py:31`, `app/chamados/services/becos.py:33`). O cron correspondente usa `--alarme`; isso não equivale a um fechamento automático geral (`app/config/crons.py:663`).

**(d) Casa:** **COMPLEMENTO de C7-DISPUTA-FECHA-POR-UM-JUIZ**, com interseção na série E3/E4; a casa já manda migrar decisões espalhadas para C6/C7 (`app/docs/BACKLOG.md:1468`, `app/docs/BACKLOG.md:1862`, `app/docs/BACKLOG.md:1863`). Os múltiplos sítios enumerados impedem classificá-lo como BO de sítio único.

**(e) Padrão:** **Single Writer Principle / juiz único**, termo existente, realizado ao retirar a decisão paralela do explicador e consumir C7 (`app/docs/GLOSSARIO.md:12`, `app/chamados/services/explicador.py:137`, `app/core/juizes.py:279`).

**Depende de dado:** contagem dos dez casos por instante do carimbo antes/depois de 22/10, via, estado do pai, respostas sem veredito e resultado atual de C7; também contagem de tentativas de fechamento recusadas. A atribuição histórica ao commit indicado no anexo não foi conferida. [não verificado] [nao verificado]

perguntas de lei: 0

O critério de fechamento e a guarda de resposta sem veredito já estão escritos; a separação histórica dos dez casos é pergunta de dado (`app/docs/CORTES.md:272`, `app/chamados/models.py:1853`).

## PERGUNTA-DUPLICATA-1110

**(a) Lei, juiz e porta.** Aplicam-se **L-001/L-002/L-003/L-007**: origem, autoridade comum, medição pela função real e escritor com trilha; a idempotência integra o contrato estrutural (`app/docs/LEIS.md:59`, `app/docs/LEIS.md:60`, `app/docs/LEIS.md:61`, `app/docs/LEIS.md:65`, `CLAUDE.md:175`).

**Sem juiz registrado para eleger a pergunta canônica.** C8 pergunta pela duplicidade de **chamado** e aponta para `ChamadoColaborador.abrir`; não registra a eleição de `PerguntaDisputa` (`app/core/juizes.py:114`, `app/core/juizes.py:280`). O lookup de fio aponta para `_disputa_do_chamado`, outra pergunta (`app/docs/JUIZES.md:40`).

Quem decide hoje:

- **Identidade:** MARCO por colaborador/dia/motivo/instância; BATIDA por episódio/motivo (`app/chamados/services/fio.py:1372`).
- **Canônica:** menor pk com resposta textual não vazia; na ausência dela, menor pk (`app/chamados/services/fio.py:1225`, `app/chamados/services/fio.py:1232`).
- **Retratabilidade:** `plano_de_retratacao` preserva mérito, episódios distintos e acervo sem episódio (`app/chamados/services/fio.py:1259`, `app/chamados/services/fio.py:1287`, `app/chamados/services/fio.py:1290`, `app/chamados/services/fio.py:1294`).
- **Porta/escritor:** `retratar_para_canonica` → `retratar_duplicata` → `carimbar_identidade`; a gravação física ocorre no modelo (`app/chamados/services/fio.py:1319`, `app/chamados/services/fio.py:1345`, `app/chamados/services/fio.py:1359`, `app/chamados/models.py:3190`).

**Conferência do relato.** Os **486 grupos/848 excedentes** são dados fornecidos sem chave de agrupamento nem pks; sua equivalência a duplicatas segundo o código não está demonstrada. [não verificado] O agrupador exclui conversa e vias fora do fato, distingue famílias e aplica horizonte; o plano também conserva perdedoras não retratáveis (`app/chamados/services/fio.py:1431`, `app/chamados/services/fio.py:1945`, `app/chamados/services/fio.py:1949`, `app/chamados/services/fio.py:1290`).

“Disputa aberta” não basta para demonstrar inflação: o app primeiro consulta `q_vivas`, confirma C6 e só depois classifica pendência por resposta ausente e disputa aberta (`app/chamados/services/pergunta_no_app.py:50`, `app/chamados/services/pergunta_no_app.py:58`, `app/chamados/services/pergunta_no_app.py:71`).

Há ainda uma limitação na proposta de “medir idade”: a pergunta dispõe de data do fato e carimbos posteriores, mas não de carimbo próprio de nascimento; essa ausência está registrada em **O326** (`app/chamados/models.py:2423`, `app/chamados/models.py:2448`, `app/chamados/models.py:2449`, `app/docs/BACKLOG.md:1633`). Idade exata de criação não pode ser inferida desses outros carimbos. [não verificado]

**(b) Cria estado: não. Muda quem decide: não**, no escopo de medir pelo universo existente e retratar apenas os excedentes aceitos pela porta atual. `duplicata` já é valor existente, e a porta já grava via/carimbo/autor/motivo; o nascimento também consulta a eleição existente pelo alias `_canonica` (`app/chamados/models.py:2495`, `app/chamados/models.py:3182`, `app/chamados/models.py:3186`, `app/chamados/models.py:3190`, `app/chamados/models.py:2905`, `app/chamados/models.py:2921`).

Adicionar nascimento próprio ou fato novo de identidade ampliaria esse escopo: são obras já separadas, **O326** e **FATO-A06** (`app/docs/BACKLOG.md:1633`, `app/docs/BACKLOG.md:1867`).

**(c) A condição de censo global dos campos não é acionada por não/não.** Como inventário complementar, contei por AST o perímetro de cinco símbolos — `canonica_do_grupo`, `plano_de_retratacao`, `grupos_de_colisao`, `retratar_para_canonica`, `retratar_duplicata` — resolvendo imports com `as`, inclusive `_canonica`. São **23 chamadas: 19 leituras de decisão e 4 delegações de escrita**, enumeradas abaixo:

| Arquivo | Sítios e forma |
|---|---|
| `app/chamados/management/commands/backfill_decisoes_pergunta.py` | `:276` eleição; `:290` plano/truthiness das categorias em `:295`; `:331` escrita; `:393` eleição após `len>=2` em `:392`; `:396` leitura `['retrata']`; `:485` grupos; `:506` eleição/comparação de pk em `:507`; `:510` plano/`len` em `:512`. |
| `app/chamados/management/commands/dedup_perguntas_fato.py` | `:58` grupos; `:76` eleição; `:81` plano, leitura `['retrata']` em `:88` e truthiness em `:89`. |
| `app/chamados/models.py` | `:2921` eleição pelo alias `_canonica`, condicionada à lista; `is not None` em `:2922`. |
| `app/chamados/services/declaracao_texto.py` | `:253` eleição/comparação de disputa e `_reusavel` em `:254`; `:275` eleição após truthiness da lista em `:271`. |
| `app/chamados/services/fio.py` | `:921` escrita condicionada à lista em `:920`; `:956` eleição/comparação de pk em `:957`; `:967` plano/`any` em `:968`; `:1340` plano; `:1341` escrita em compreensão com truthiness; `:1832` eleição condicionada às sementes em `:1833`; `:2155` eleição/comparação de pk em `:2165`; `:2158` plano/truthiness em `:2166`; `:2180` escrita/leitura `['retratadas']`. |

A unidade é chamada dos cinco símbolos, excluindo testes/migrations; o save físico subsequente não foi somado novamente (`app/chamados/services/fio.py:1359`, `app/chamados/models.py:3190`). Nenhuma referência adicional desses cinco símbolos apareceu em templates, signals ou JS; isso não constitui censo global dos campos. [não verificado por referência negativa arquivo:linha] A menção ao backfill em crons é declaração de papel de ato único (`app/config/crons.py:780`, `app/config/crons.py:783`).

**(d) Casa:** **COMPLEMENTO de FIO-E0-V4 E1**, articulado à E4; o fato próprio de duplicata/identidade fica depois da E4d (`app/docs/BACKLOG.md:1848`, `app/docs/BACKLOG.md:1863`, `app/docs/BACKLOG.md:1867`). Há um resíduo específico sobre episódio retratado, mas não se pode atribuir os 486 grupos a ele sem dados (`app/docs/BACKLOG.md:786`). [não verificado quanto ao alcance]

**(e) Padrão:** **idempotência pela chave do fato**, termo existente; a cura deve consumir a identidade compartilhada pelo nascimento e pelo dedup (`app/docs/GLOSSARIO.md:29`, `app/chamados/models.py:2905`, `app/chamados/services/fio.py:1372`).

**Depende de dado:** reclassificação dos 486 grupos pela chave real; quantidades retratáveis, recusadas por mérito, episódios unidos e acervo sem episódio; distribuição por horizonte/competência; quantos excedentes passam por C6 e entram efetivamente nos contadores. Os 848 não demonstram, isoladamente, inflação. [não verificado]

perguntas de lei: 1

**Quando uma linha BATIDA do acervo sem episódio e uma linha com episódio podem representar o mesmo fato, permitindo que uma retrate a outra?** O código declara essa lei aberta e hoje conserva ambas (`app/chamados/services/fio.py:1274`, `app/chamados/services/fio.py:1275`, `app/chamados/services/fio.py:1294`). Não foi localizada resposta posterior nas leis/cortes pesquisados. [não verificado quanto à ausência global] Depende de dado: quantos grupos fornecidos pertencem a essa classe. [não verificado]

## BO-ROTULO-ENCERRADO

**(a) Lei, juiz e porta.** Aplicam-se **L-002**, testemunha lê a autoridade; **LEIS-UI L2**, tela e ação consultam a mesma função; **L7** trata especificamente de ajuda proveniente do mapa do domínio, não estabelece literalmente “um rótulo por estado” (`app/docs/LEIS.md:60`, `app/docs/LEIS-UI.md:23`, `app/docs/LEIS-UI.md:77`).

**Sem juiz registrado para o texto das ações do seletor.** O registro chamado não contém essa pergunta; hoje os textos são decididos por dois blocos literais de template (`app/core/juizes.py:272`, `app/core/juizes.py:293`, `app/templates/chamados/partials/modal_fio.html:667`, `app/templates/chamados/partials/painel_chamado_turno.html:76`).

Para espera do colaborador, decide `rotulo_para_pessoa`, através do filtro `espera`; para transição de negócio, o juiz registrado é `motor.decidir`, acessado por `transicionar`/`pode_transicionar`, com escritor `_gravar_estado` (`app/chamados/services/verbos.py:138`, `app/chamados/templatetags/chamados_tags.py:63`, `app/core/juizes.py:274`, `app/docs/JUIZES.md:37`, `app/chamados/models.py:119`, `app/chamados/models.py:128`, `app/chamados/models.py:201`).

**Conferência do relato:**

- **Confere:** os dois detalhes usam `espera|first` e mostram “Encerrado” para `resolvido` e `fechado`, conforme o mapa (`app/templates/chamados/detalhe.html:133`, `app/templates/chamados/detalhe.html:134`, `app/templates/chamados/detalhe_local.html:396`, `app/templates/chamados/detalhe_local.html:397`, `app/chamados/services/verbos.py:118`, `app/chamados/services/verbos.py:119`).
- O defeito também alcança **um terceiro seletor**, no fio do colaborador (`app/templates/chamados/fio_colaborador.html:269`, `app/templates/chamados/fio_colaborador.html:270`).
- **Confere:** o modal distingue “Encerrar” de “Encerrar e arquivar” (`app/templates/chamados/partials/modal_fio.html:668`, `app/templates/chamados/partials/modal_fio.html:669`).
- **Não existe a fonte comum pressuposta pela cura descrita:** o modal contém literais, repetidos no painel do turno. Seu comentário distingue explicitamente verbo da ação e valor enviado à view (`app/templates/chamados/partials/modal_fio.html:660`, `app/templates/chamados/partials/modal_fio.html:667`, `app/templates/chamados/partials/painel_chamado_turno.html:76`).

**(b) Cria estado: não. Muda quem decide: sim, na apresentação.** Os valores enviados já existem; a cura centralizaria quem fornece o texto, preservando os `value` e a decisão de negócio (`app/chamados/models.py:31`, `app/chamados/models.py:32`, `app/templates/chamados/detalhe_local.html:396`, `app/templates/chamados/detalhe_local.html:397`). O servidor recebe `novo_status` e chama a mesma porta, que consulta o motor (`app/chamados/services/acoes_chamado.py:287`, `app/chamados/services/acoes_chamado.py:315`, `app/chamados/models.py:123`).

**(c) Censo do texto de espera/seletor: 13 sítios = 3 definidores/renderizadores + 10 leitores.** A unidade é função ou bloco de template; opções irmãs contam como um bloco.

| Papel | Sítio | Forma |
|---|---|---|
| Definidor | `app/chamados/services/verbos.py:114` | Dict `ESPERA_COLAB`, comparação por chave de estado |
| Definidor/renderizador | `app/templates/chamados/partials/modal_fio.html:667` | Texto literal das opções |
| Definidor/renderizador | `app/templates/chamados/partials/painel_chamado_turno.html:76` | Texto literal das opções |
| Leitor | `app/chamados/services/verbos.py:138` | Truthiness/normalização em `:140`, `.get` em `:141` |
| Leitor | `app/chamados/templatetags/chamados_tags.py:53` | Filtro registrado, import e delegação em `:63`/`:64` |
| Leitor | `app/templates/chamados/detalhe.html:36` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/detalhe_local.html:68` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/fio_colaborador.html:188` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/partials/painel_chamado_turno.html:18` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/partials/modal_fio.html:101` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/detalhe.html:132` | Constantes → `espera|first`, opções |
| Leitor | `app/templates/chamados/detalhe_local.html:395` | Constantes → `espera|first`, opções |
| Leitor | `app/templates/chamados/fio_colaborador.html:268` | Constantes → `espera|first`, opções |

Método: grep de símbolos/filtro/opções em Python, HTML e JS, mais AST dos Python com imports apelidados. O carregamento do mapa e a chamada da função estão em `app/chamados/services/verbos.py:141` e `app/chamados/templatetags/chamados_tags.py:64`. Não apareceram outros aliases, leitores em commands/crons/signals ou leituras textuais desses seletores no JS. [não verificado por referência negativa arquivo:linha]

`rotulo_de_linha` responde outra pergunta — palavra da fila administrativa — e não foi contado como fornecedor do texto desses seletores (`app/chamados/services/verbos.py:96`, `app/chamados/services/verbos.py:125`).

**(d) Casa:** **COMPLEMENTO de O137 — ESMERIL-NO-RASTRO**, classe “testemunha recalcula/rótulo não migrou”; a ligação está explicitada no filtro e no teste dessa família (`app/docs/BACKLOG.md:1363`, `app/chamados/templatetags/chamados_tags.py:55`, `app/chamados/tests/test_o137_esmeril_chamado.py:11`). Não é BO de sítio único: são três seletores e dois blocos literais de referência, enumerados em (c).

**(e) Padrão:** **Single Source of Truth / fonte única**, termo existente, aplicado ao vocabulário das ações; VERBO e ESPERA cont

[truncado em 20000 de 22188 caracteres]

## 10/11 19:54 · sessao 2f5571a09655 · - · - · origem=Ronald (tmux)

Confirmo **estado novo: não / muda decisor local: sim**: o explicador decide hoje por `not _fio_vivo(ps)`; a cura faria esse bool consumir C7, que também exige ausência de respostas sem veredito (`app/chamados/services/explicador.py:136`, `app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).

Para contar **todas as ocorrências semânticas do bool**, use **7 = 1 escritor + 6 leitores**:

| Papel/forma | Sítio |
|---|---|
| Escritor: atribuição de `pode` | `app/chamados/services/explicador.py:137` |
| Leitor interno: condição do ternário | `app/chamados/services/explicador.py:138` |
| Leitor interno: truthiness no `if` | `app/chamados/services/explicador.py:139` |
| Leitor interno: valor exportado na chave `pode_fechar` | `app/chamados/services/explicador.py:163` |
| Leitor externo: `dict.get`, truthiness | `app/chamados/services/desenlace.py:63` |
| Leitor externo: truthiness em template | `app/templates/chamados/partials/modal_fio.html:434` |
| Leitor externo: truthiness em template | `app/templates/chamados/partials/modal_fio.html:445` |

A contagem **4** seria somente **1 produtor + 3 consumidores externos**, omitindo as leituras locais; prefiro **7** para atender literalmente ao censo (`app/chamados/services/explicador.py:137`, `app/chamados/services/explicador.py:138`, `app/chamados/services/explicador.py:139`, `app/chamados/services/explicador.py:163`).

O transporte não cria decisão: o retorno entra no cache e depois em `ch.explicacao`; o alias `desenlace as _desen` recebe o mesmo dict (`app/chamados/services/fio.py:312`, `app/chamados/services/fio.py:314`, `app/chamados/services/fio.py:386`, `app/chamados/services/fio.py:403`, `app/chamados/services/fio.py:404`). Os caminhos completo e parcial passam listas com explicação ao modal (`app/chamados/views.py:889`, `app/chamados/views.py:927`, `app/chamados/services/fio.py:446`, `app/chamados/services/fio.py:495`).

Grep global de `pode_fechar`, `por_que_nao_fecha`, `.explicacao` e `explicacao=`, excluindo testes/migrations, seguido dos aliases e transportes acima, não revelou outros consumidores do bool; a busca específica em `app/static/**/*.js` retornou zero ocorrências [não verificado por arquivo:linha negativo]. Os 25 sítios de chamadas de carimbo/C7 constituem outro recorte e não devem ser somados ao total **7** de (c) (`app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).

## 10/11 19:54 · sessao 235eacb48a70 · - · - · origem=Ronald (tmux)

Confirmado: **não cria estado / sim, troca quem fornece o rótulo**, preservando o juiz da transição (`app/chamados/models.py:123`; `app/chamados/services/acoes_chamado.py:315`).

Os **13 sítios** abrangem o texto visível de espera e dos seletores. Os dois blocos literais contam uma vez cada, como definidor/renderizador (`app/templates/chamados/partials/modal_fio.html:667`; `app/templates/chamados/partials/painel_chamado_turno.html:76`). `rotulo_de_linha` responde outra pergunta — palavra da fila do administrador — e não fornece texto aos seletores (`app/chamados/services/verbos.py:125`).

Grep de JS estático e embutido não encontrou leitura textual desses seletores. Os candidatos examinados leem outros controles: `select[name=escape]` e `data-doc` (`app/templates/chamados/meu_atendimento.html:373`), ou `para-colab-tipo` e `data-pedeDia` (`app/templates/chamados/form_abertura.html:237`). O servidor recebe o `value` pelo nome `novo_status` (`app/chamados/services/acoes_chamado.py:287`). A negativa global, sem artefato gravado da busca, deve carregar **[não verificado]** no relatório.

## 10/11 19:53 · sessao 67ea159aafa1 · - · - · origem=Ronald (tmux)

Correção de **(f)**: para o item amplo, use:

perguntas de lei: 1

**“Quando uma linha BATIDA do acervo sem episódio e uma linha com episódio podem representar o mesmo fato, permitindo que uma retrate a outra?”** O código declara expressamente essa lei aberta em `app/chamados/services/fio.py:1274` e `:1275`; hoje preserva ambas, pelo ramo `sem_episodio_intocada` (`:1294`).

Busquei `sem_episodio_intocada`, `canonica_sem_episodio`, “sem episódio”, “acervo” combinado com “episódio/retrata/identidade” em LEIS, CORTES, CLAUDE e docs. Não encontrei corte que responda essa pergunta; a ausência global de resposta escrita é **[não verificado]**, mas a declaração explícita de pendência está no código (`app/chamados/services/fio.py:1275`).

**Alcance:** depende de dado: quantos dos 486 grupos/848 excedentes contêm essa combinação. O anexo não fornece chave nem pks **[não verificado]**. Essa pergunta não impede medir ou retratar os grupos que o plano já aceita em `app/chamados/services/fio.py:1297`; impede chamar toda perdedora de excedente curável sem classificar as recusas (`:1280`, `:1294`).

O N=0 anterior era válido somente para o subconjunto já aceito pela porta, não para todo o item genérico.

## 10/11 19:53 · sessao 235eacb48a70 · - · - · origem=Ronald (tmux)

## BO-ROTULO-ENCERRADO

**(a) Lei, juiz e porta.** A regra aplicável é “testemunha lê, não recalcula” (`CLAUDE.md:20`), acompanhada de tela e ação consultarem a mesma função (`app/docs/LEIS-UI.md:23`) e textos explicativos saírem do mapa do domínio (`app/docs/LEIS-UI.md:77`). Não encontrei uma lei literal “rótulo único por estado”; a fonte atual distingue duas perguntas: ação do administrador e espera do colaborador (`app/chamados/services/verbos.py:96`).

Para **rótulo de ação**, **sem juiz registrado**: o registro da família chamado enumera transição, premissa, permissão, dia, perguntas e demais autoridades, sem essa pergunta (`app/core/juizes.py:272` até `:293`). Hoje decidem dois blocos literais: `app/templates/chamados/partials/modal_fio.html:667` e `app/templates/chamados/partials/painel_chamado_turno.html:76`. Para o rótulo de espera, decide `rotulo_para_pessoa`, através do filtro `espera` (`app/chamados/templatetags/chamados_tags.py:53`, `app/chamados/services/verbos.py:138`).

A transição permanece com o juiz registrado `motor.decidir` (`app/core/juizes.py:274`, `app/docs/JUIZES.md:37`), acessado por `ChamadoColaborador.transicionar`/`pode_transicionar` (`app/chamados/models.py:119`, `:128`); seu escritor canônico é `_gravar_estado` (`app/chamados/models.py:201`).

**Conferência do relato.** Confere: os dois detalhes mostram “Encerrado” nas opções `resolvido` e `fechado`, porque usam `espera|first` (`app/templates/chamados/detalhe.html:133`, `:134`; `app/templates/chamados/detalhe_local.html:396`, `:397`) e o mapa traduz ambos assim (`app/chamados/services/verbos.py:118`, `:119`). O mesmo defeito aparece também em um terceiro seletor, não mencionado no relato (`app/templates/chamados/fio_colaborador.html:269`, `:270`). Confere que o modal distingue “Encerrar”/“Encerrar e arquivar” (`app/templates/chamados/partials/modal_fio.html:668`, `:669`).

Não confere a sugestão de uma **fonte comum já existente no modal**: os rótulos são literais no template; outro template repete os mesmos literais (`app/templates/chamados/partials/modal_fio.html:667`, `app/templates/chamados/partials/painel_chamado_turno.html:76`). O próprio comentário esclarece que ali se mostra **ação**, mantendo `value` como contrato com a view (`app/templates/chamados/partials/modal_fio.html:660`). O chamado de exemplo não pode ser conferido: depende de dado — contagem de ocorrências e renderização do pk citado em cada rota. [não verificado]

**(b)** Cria estado: **não**, para a cura limitada a rótulos: `resolvido` e `fechado` já são valores do campo existente (`app/chamados/models.py:31`, `:32`, `:52`), e os seletores já enviam esses mesmos valores (`app/templates/chamados/detalhe_local.html:396`, `:397`). Muda decisor de negócio: **não**, nesse escopo; a ação recebe `novo_status` e chama a mesma porta (`app/chamados/services/acoes_chamado.py:287`, `:313`, `:315`), cuja validação delega ao juiz atual (`app/chamados/models.py:123`). Centralizar o vocabulário muda sua fonte de apresentação; não autoriza mudar o julgamento da transição (`app/chamados/catalogo/motor.py:139`).

**(c) Censo relevante ao rótulo; censo global do estado dispensado por (b).** Unidade: função ou bloco de template; opções irmãs contam como um bloco. **13 sítios = 3 definidores + 10 leitores**:

| Tipo | Sítio | Forma |
|---|---|---|
| Definidor | `app/chamados/services/verbos.py:114` | dicionário `ESPERA_COLAB`; valores `resolvido`/`fechado` iguais |
| Definidor/renderizador | `app/templates/chamados/partials/modal_fio.html:667` | opções com texto literal e `value` de estado |
| Definidor/renderizador | `app/templates/chamados/partials/painel_chamado_turno.html:76` | opções com texto literal e `value` de estado |
| Leitor | `app/chamados/services/verbos.py:138` | normalização por truthiness em `:140`; `.get(st, fallback)` em `:141` |
| Leitor | `app/chamados/templatetags/chamados_tags.py:53` | filtro registrado; import local e delegação em `:63`/`:64` |
| Leitor | `app/templates/chamados/detalhe.html:36` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/detalhe_local.html:68` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/fio_colaborador.html:188` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/partials/painel_chamado_turno.html:18` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/partials/modal_fio.html:101` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/detalhe.html:132` | constantes de estado → `espera|first` |
| Leitor | `app/templates/chamados/detalhe_local.html:395` | constantes de estado → `espera|first` |
| Leitor | `app/templates/chamados/fio_colaborador.html:268` | constantes de estado → `espera|first` |

Método executado: grep global em `.py`, `.html`, `.js` por `rotulo_para_pessoa`, `ESPERA_COLAB`, `|espera` e opções; AST dos Python de produção, com resolução de imports `as`. Encontrados um carregamento Python do mapa (`app/chamados/services/verbos.py:141`) e uma chamada da função (`app/chamados/templatetags/chamados_tags.py:64`). Nenhum outro alias/chamador foi encontrado; commands, `config/crons.py`, signals e JS de static não produziram ocorrências desses símbolos. Como prova negativa de execução sem artefato gravado, o resultado das buscas fica [não verificado] para quem só disponha deste relatório.

**(d)** **COMPLEMENTO de O137 — ESMERIL-NO-RASTRO**, especificamente a classe testemunha/rótulo que não migrou, já descrita nessa casa (`app/docs/BACKLOG.md:1363`). A ligação é explícita no teste da família chamado: a classe B centraliza a pílula de espera em `rotulo_para_pessoa` (`app/chamados/tests/test_o137_esmeril_chamado.py:11`), e o próprio filtro identifica O137 (`app/chamados/templatetags/chamados_tags.py:55`). Não há prova de BO de sítio único: são três seletores afetados e dois modelos literais de ação (`app/templates/chamados/detalhe.html:132`, `app/templates/chamados/detalhe_local.html:395`, `app/templates/chamados/fio_colaborador.html:268`, `app/templates/chamados/partials/modal_fio.html:667`, `app/templates/chamados/partials/painel_chamado_turno.html:76`). Esse encaixe não afirma que a condição atual de execução da O137 esteja satisfeita; a casa exige família fechada (`app/docs/BACKLOG.md:1363`).

**(e)** **Single Source of Truth / Aggregate Root — fonte única**, termo existente (`app/docs/GLOSSARIO.md:11`), aplicado ao vocabulário da ação, mantendo separado o vocabulário de espera já declarado (`app/chamados/services/verbos.py:96`).

**(f)** perguntas de lei: 0

No escopo limitado de apresentar os verbos já existentes e preservar `value`, a distinção ação/espera já está documentada (`app/templates/chamados/partials/modal_fio.html:660`, `app/chamados/services/verbos.py:96`); a obrigação de fonte única já existe (`CLAUDE.md:20`). Se se pretender registrar uma nova autoridade de domínio, aplica-se a regra de declaração prévia, assunto que este censo não autoriza construir (`CLAUDE.md:311`).

**Linha para tabela consolidada:**  
`BO-ROTULO-ENCERRADO | CLAUDE:20; LEIS-UI L2/L7; sem juiz registrado do rótulo; motor.decidir/transicionar/_gravar_estado | não/não | 13 (perímetro do rótulo; estado global n/a) | COMPLEMENTO de O137 (BACKLOG:1363) | Single Source of Truth (GLOSSARIO:11) | 0`

_3492 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
