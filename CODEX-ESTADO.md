# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 17:59** · publicado em **10/11/2026 18:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 17:59 · sessao 3960e7546cc4 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Volta 2 entregue: [ENTREGA, com comandos, RED e mutantes](chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:9).

- Linhas registradas: **778, 779 e 1262**. [Registro](core/tests/test_medida_turno_do_par.py:267)
- Trava duplicada removida; tetos **145/219**, com justificativa C8. [Cura](chamados/services/disputa_emissao.py:548), [tetos](ponto/tests/test_selo_chokepoint_escrita.py:180)
- Sonda isolada: **6 → 5 consultas**, igualmente com 1 e 3 furos. [Medida](chamados/tests/ENTREGA_BO_DISPUTA_SEM_PERGUNTA.txt:54)

SQL real e suite ORM completa: **[nao verificado]**. [nao verificado]

## 10/11 17:53 · sessao 33840901b286 · - · - · origem=Ronald (tmux)

As duas queries vêm de [disputa_emissao.py:548](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:548) e [:549](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:549):

- `SELECT ... FOR UPDATE` do Colaborador pela chamada separada de `trava_na_ordem`; implementação em [models.py:2666](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/models.py:2666).
- `SELECT pk` de todas as disputas do colaborador, incluindo fechadas.

O `atomic` já existia na BASE; a comparação `git diff 6c0517eb9 69f1a8b4 -- chamados/services/disputa_emissao.py` mostra que o acréscimo está nessas leituras. Quantidades ORM locais: [nao verificado].

É possível remover **uma** query: passe `donas` como QuerySet lazy e retire a chamada separada de `trava_na_ordem(colaborador)`. A própria porta trava o Colaborador em [models.py:2665](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/models.py:2665) antes de avaliar os IDs em [:2671](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/models.py:2671). O QuerySet guarda o resultado das avaliações seguintes; runtime com ORM: [nao verificado].

A query restante lê o conjunto de donas necessário para incluir as disputas fechadas na trava antes de Chamado/Pergunta: [disputa_emissao.py:545](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:545). Ela fica fora dos loops de motivos/furos: [:543](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:543). Portanto, o acréscimo estrutural passa de **+2 para +1 por invocação do núcleo**; comparação ORM efetiva de 1 versus 3 furos: [nao verificado].

Nenhum arquivo alterado.

## 10/11 17:16 · sessao af554e746a14 · BO-0611-05 · censo · origem=Code

O geofence decide pela **truthiness das duas coordenadas**:

```python
if not batida.latitude or not batida.longitude:
```

Essa é a expressão exata em `app/ponto/services/geofence.py:247`. Havendo posto, o ramo cria alerta `tipo='sem_gps'` e grava coordenadas **do alerta** como `None`; não altera latitude/longitude da Batida (`app/ponto/services/geofence.py:248`, `app/ponto/services/geofence.py:252`, `app/ponto/services/geofence.py:253`).

O censo abaixo reúne **37 sítios diretos de Batida/contexto**, mais **4 sobre coordenadas copiadas em AlertaGeofence**, totalizando **41**, conforme as referências das tabelas. Incidência desses casos em produção: **[nao verificado]**.

Convenção de contagem: cada expressão independente conta um sítio; cada filtro conjunto das duas coordenadas conta um. Mantive **N/N1 na sintaxe solicitada**: guardas inversas com `is None` e cadeias `.exclude(...=None)` aparecem como **O**, embora sejam testes de nulidade equivalentes. [nao verificado]

**(i) Leitores de Batida gravada ou contexto de chamado — Python**

| Arquivo:linha | Expressão literal | Forma | O que decide |
|---|---|---|---|
| `app/ponto/services/geofence.py:247` | `not batida.latitude or not batida.longitude` | T | Ramo de ausência que pode emitir `sem_gps`. |
| `app/colaboradores/services/drawer.py:63` | `bool(b.latitude and b.longitude)` | T | Resultado `tem_gps` do drawer. |
| `app/ponto/services/geofence.py:327` | `float(batida.latitude) if batida.latitude else None` | T1 | Latitude ou ausência na ocorrência do chamado. |
| `app/ponto/services/geofence.py:328` | `float(batida.longitude) if batida.longitude else None` | T1 | Longitude ou ausência na ocorrência. |
| `app/ponto/services/geofence.py:369` | `float(batida.latitude) if batida.latitude else None` | T1 | Latitude ou ausência no contexto legado. |
| `app/ponto/services/geofence.py:370` | `float(batida.longitude) if batida.longitude else None` | T1 | Longitude ou ausência no contexto legado. |
| `app/core/services/painel_op.py:227` | `entrada.latitude if entrada and entrada.latitude else None` | T1 | Latitude disponível no contexto do painel. |
| `app/core/services/painel_op.py:228` | `entrada.longitude if entrada and entrada.longitude else None` | T1 | Longitude disponível no contexto do painel. |
| `app/colaboradores/painel_cell.py:54` | `str(coord[0] or '')` | T1 | Latitude ou vazio no atributo da linha do painel. |
| `app/colaboradores/painel_cell.py:55` | `str(coord[1] or '')` | T1 | Longitude ou vazio. |
| `app/colaboradores/services/situacional.py:199` | `str((batida_coord.get(c.pk) or (None, None))[0] or '')` | T1 | Latitude ou vazio no painel em lote. |
| `app/colaboradores/services/situacional.py:200` | `str((batida_coord.get(c.pk) or (None, None))[1] or '')` | T1 | Longitude ou vazio. |
| `app/chamados/views.py:692` | `_BatidaGeo.objects.filter(colaborador=colaborador, latitude__isnull=False, longitude__isnull=False)` | N | Seleciona última localização e histórico para o fio. |
| `app/ponto/views.py:485` | `pendentes.filter(latitude__isnull=False, longitude__isnull=False).count()` | N | Conta `com_gps`; `sem_gps` é o complemento em `app/ponto/views.py:486`. |
| `app/ponto/services/geofence.py:52` | `colaborador=colaborador, latitude__isnull=False, longitude__isnull=False` | N | Seleciona batidas consideradas “COM GPS” por `vinculo_contagem`. |
| `app/ponto/services/vinculo_na_lista.py:49` | `Batida.objects.filter(timestamp__date__gte=ini, latitude__isnull=False, longitude__isnull=False, gps_accuracy__isnull=False)` | N | Seleciona evidências geográficas para avaliar vínculo. |
| `app/ponto/services/geofence.py:403` | `la is not None and lo is not None` | N | Seleciona ocorrências do contexto para reavaliar chamados. |
| `app/ponto/views.py:468` | `str(batida.latitude) if batida.latitude is not None else None` | N1 | Latitude ou ausência no snapshot de auditoria offline. |
| `app/ponto/views.py:469` | `str(batida.longitude) if batida.longitude is not None else None` | N1 | Longitude ou ausência no snapshot. |
| `app/core/views.py:1024` | `float(b.latitude) if b.latitude is not None else None` | N1 | Latitude ou ausência no JSON do detalhe. |
| `app/core/views.py:1025` | `float(b.longitude) if b.longitude is not None else None` | N1 | Longitude ou ausência no JSON. |
| `app/colaboradores/views.py:53` | `Batida.objects.exclude(latitude=None).exclude(longitude=None)` | O | Conjunto de colaboradores com geo para vínculo. |
| `app/colaboradores/views.py:337` | `Batida.objects.filter(colaborador=colaborador).exclude(latitude=None).exclude(longitude=None).exists()` | O | `tem_geo` no retorno HTMX de erro ao vincular posto. |
| `app/colaboradores/views.py:356` | `Batida.objects.filter(colaborador=colaborador).exclude(latitude=None).exclude(longitude=None).exists()` | O | `tem_geo` no retorno HTMX de sucesso ao vincular posto. |
| `app/colaboradores/views.py:395` | `Batida.objects.filter(colaborador=colaborador).exclude(latitude=None).exclude(longitude=None).exists()` | O | `tem_geo` ao renderizar linha de vínculo de escala. |
| `app/colaboradores/services/detalhe.py:22` | `.exclude(latitude=None).exclude(longitude=None)` | O | Histórico geo e pontos do mapa. |
| `app/colaboradores/services/detalhe.py:53` | `_todas.exclude(latitude=None).exclude(longitude=None).count()` | O | Contador total de batidas com geo. |
| `app/colaboradores/views.py:2474` | `not coord or coord[0] is None or coord[1] is None` | O | Retorna pino `sem_gps`, cinza, para coordenadas da última Batida. |

No último helper, uma falha de cálculo/conversão também retorna `sem_gps` pelo `except (TypeError, ValueError)`; é tratamento de exceção, separado da expressão direta de presença contabilizada acima (`app/colaboradores/views.py:2479`, `app/colaboradores/views.py:2481`).

**(i) Leitores — templates e JavaScript**

| Arquivo:linha | Expressão literal | Forma | O que decide |
|---|---|---|---|
| `app/templates/core/_d2_feed.html:16` | `b.latitude is not None and b.longitude is not None` | N | Exibe pino/link Google Maps. |
| `app/templates/ponto/batidas_offline_pendentes.html:96` | `batida.latitude is not None and batida.longitude is not None` | N | Exibe link Google Maps. |
| `app/templates/chamados/partials/modal_fio.html:480` | `chamado.modulo_origem == "gps_fora_geofence" and chamado.contexto_json.batida_lat is not None and chamado.contexto_json.batida_lon is not None` | N | Exibe bloco GPS e botão de mapa do chamado. |
| `app/templates/chamados/partials/_relatorio_geo.html:30` | `chamado.contexto_json.batida_lat is not None and chamado.contexto_json.batida_lon is not None` | N | Exibe coordenadas e rota posto→batida. |
| `app/templates/core/dashboard2.html:242` | `(b.latitude && b.longitude)` | T | Escolhe conteúdo GPS ou “Sem localizacao”. |
| `app/templates/core/dashboard2.html:287` | `b.latitude && b.longitude` | T | Inicializa mapa e pino do drawer. |
| `app/static/js/geo-colab-map.js:39` | `isNaN(blat) || isNaN(blon)` | O | Mostra ausência e retorna, ou prossegue com mapa. |
| `app/templates/chamados/fio_colaborador.html:313` | `!window.L||isNaN(blat)||isNaN(blon)` | O | Mostra “Sem coordenada de GPS.” e retorna; inclui indisponibilidade da biblioteca. |
| `app/templates/chamados/painel_gestao.html:583` | `isNaN(blat) || isNaN(blon)` | O | Mostra “Sem coordenada de GPS nesta batida.” e retorna, ou prossegue com mapa. |

**(i) Equivalentes: coordenadas copiadas em AlertaGeofence**

| Arquivo:linha | Expressão literal | Forma | O que decide |
|---|---|---|---|
| `app/ponto/models.py:263` | `self.latitude is None or self.longitude is None` | O | Em `ainda_vale`, ausência conserva a validade do alerta. |
| `app/ponto/management/commands/reconciliar_alertas_geofence.py:42` | `lat is None or lon is None or not alvo` | O | Conta alerta como sem dado e pula reconciliação. |
| `app/templates/colaboradores/detalhe.html:142` | `a.latitude` | T1 | Exibe link de mapa do alerta. |
| `app/templates/core/dashboard_supervisor.html:90` | `a.latitude` | T1 | Exibe link de mapa do alerta ou “—”. |

As coordenadas do alerta vêm da Batida nos ramos geográficos, por exemplo em `app/ponto/services/geofence.py:275` e `app/ponto/services/geofence.py:302`; no ramo `sem_gps`, recebem `None` (`app/ponto/services/geofence.py:253`).

A soma dos sítios documentados nessas tabelas é:

| Universo | T | T1 | N | N1 | O | Total |
|---|---:|---:|---:|---:|---:|---:|
| Batida/contexto | 4 | 10 | 9 | 4 | 10 | **37** |
| Cópias em alerta | 0 | 2 | 0 | 0 | 2 | **4** |
| **Total operacional** | **4** | **12** | **9** | **4** | **12** | **41** |

**Divergem da forma T do geofence todos os 37 sítios classificados T1, N, N1 ou O nas tabelas.** Os quatro T são `app/ponto/services/geofence.py:247`, `app/colaboradores/services/drawer.py:63`, `app/templates/core/dashboard2.html:242` e `app/templates/core/dashboard2.html:287`.

A divergência de **forma** não implica o mesmo efeito em todos: T1 serializa/testa um componente; N/N1 e os O de nulidade preservam zero; os O com `isNaN` aceitam zero quando ele chega como número parseável (`app/ponto/services/geofence.py:327`, `app/core/views.py:1024`, `app/colaboradores/views.py:2474`, `app/static/js/geo-colab-map.js:29`, `app/static/js/geo-colab-map.js:39`). Se os nove O de nulidade forem agrupados semanticamente como N, o total equivalente fica **T=4, T1=12, N=18, N1=4, O=3** — os mesmos sítios, apenas outra convenção.

Há consumidores de resultados prontos, sem novo cálculo sobre coordenadas; ficam fora dos 41:

| Arquivo:linha | Expressão | Uso |
|---|---|---|
| `app/templates/colaboradores/partials/_drawer_diagnostico.html:49` | `b.tem_gps`, duas vezes | Cor e texto `ok/sem`; produtor em `app/colaboradores/services/drawer.py:63`. |
| `app/templates/colaboradores/partials/vinculo_row.html:12` | `d.tem_geo` | Selo 📍; produtor em `app/colaboradores/views.py:84`. |
| `app/templates/colaboradores/partials/_ps_linha.html:16` | `l.geo == 'verde'` | Apresentação do pino. |
| `app/templates/colaboradores/partials/_ps_linha.html:17` | `l.geo == 'vermelho'` | Apresentação do pino. |
| `app/templates/chamados/fio_colaborador.html:112` | `ultima_geo` | Bloco de última localização; seleção por coordenadas em `app/chamados/views.py:692`. |
| `app/templates/colaboradores/detalhe.html:226` | `!pontos.length` | Ausência de pontos no mapa; seleção por coordenadas em `app/colaboradores/services/detalhe.py:22`. |

Quanto a um nome reutilizável para “tem geo”: existe a **chave de dicionário** `'tem_gps': bool(b.latitude and b.longitude)`; não é método/propriedade da Batida (`app/colaboradores/services/drawer.py:63`). Existem variáveis locais `tem_geo` (`app/colaboradores/views.py:337`, `app/colaboradores/views.py:356`, `app/colaboradores/views.py:395`) e a função classificadora `_geo_do_pino(coord, posto)`, cujo guard de presença é próprio (`app/colaboradores/views.py:2454`, `app/colaboradores/views.py:2474`).

O registro declara `classificar_posicao` para **“a batida esta dentro do geofence?”**, não um predicado de presença (`app/core/juizes.py:1191`). Essa função recebe distância, accuracy e raio (`app/ponto/services/geofence.py:95`). Não localizei função/propriedade/método booleano específico `tem_gps`, `tem_geo` ou `tem_coordenada`; inexistência absoluta sob qualquer nome: **[nao verificado]**.

**(ii) Captura no navegador antes do POST — somente listagem**

| Arquivo:linha | Captura/teste | [nao verificado]
|---|---|
| `app/templates/ponto/bater_ponto.html:293` | Geolocation e preenchimento das coordenadas. |
| `app/templates/ponto/bater_ponto.html:482` | Captura com timeout. |
| `app/templates/ponto/bater_ponto.html:503` | `!lat || !gpsCapturedAt || (Date.now() - gpsCapturedAt) > 60000` |
| `app/templates/ponto/bater_ponto.html:505` | `if (gps)` |
| `app/templates/ponto/bater_ponto.html:509` | `!lat || !lng` |
| `app/templates/ponto/triagem/base_triagem.html:56` | Geolocation e preenchimento de `.campo-lat/.campo-lng`. |
| `app/templates/ponto/triagem/apto.html:200` | Captura com timeout. |
| `app/templates/ponto/triagem/apto.html:217` | `!lat || !gpsCapturedAt || (Date.now() - gpsCapturedAt) > 60000` |
| `app/templates/ponto/triagem/apto.html:219` | `if (gps)` |
| `app/templates/ponto/triagem/apto.html:221` | `lat && lng` |
| `app/templates/ponto/triagem/apto.html:276` | `gpsLat == null || gpsLng == null` |
| `app/templates/ponto/bater_ponto_v2.html:177` | `gpsLat!=null && gpsAccuracy!=null && acc>gpsAccuracy && (Date.now()-gpsCapturedAt)<120000` |
| `app/templates/ponto/bater_ponto_v2.html:184` | Suporte/API de geolocation. |
| `app/templates/ponto/bater_ponto_v2.html:189` | `gpsLat!=null` |
| `app/templates/ponto/bater_ponto_v2.html:204` | `gpsLat==null` |
| `app/templates/ponto/bater_ponto_v2.html:273` | Captura com timeout. |
| `app/templates/ponto/bater_ponto_v2.html:292` | `gpsLat==null || gpsLng==null || !gpsCapturedAt || (Date.now()-gpsCapturedAt)>60000` |
| `app/templates/ponto/bater_ponto_v2.html:294` | `!(g && g.lat!=null)` |
| `app/templates/ponto/bater_ponto_v2.html:295` | `g && g.lat!=null` |
| `app/static/pwa.js:289` | `capturarGPS`, coordenadas ou `null`. |
| `app/static/pwa.js:394` | Captura para POST de ping geográfico. |
| `app/templates/core/onboarding.html:264` | Geolocation para permissão, sem leitura de lat/lon no callback. |

Também antes do POST, mas lendo uma batida **já guardada localmente no IndexedDB**, há dois sítios adicionais, fora dos 41 do servidor:

| Arquivo:linha | Expressão literal | Forma | Decisão |
|---|---|---|---|
| `app/static/pwa.js:145` | `b.latitude != null` | O | Inclui latitude no POST de sincronização. |
| `app/static/pwa.js:146` | `b.longitude != null` | O | Inclui longitude no POST de sincronização. |

A origem é `batidas_pendentes` do IndexedDB (`app/static/pwa.js:116`, `app/static/pwa.js:131`). São dois testes individuais de não nulidade em JS, classificados O pela legenda literal.

**Escritores de latitude/longitude da Batida**

| Arquivo:linha | Escrita/origem | Tratamento |
|---|---|---|
| `app/ponto/registro_batida.py:152` | `Batida.objects.create(..., **campos)` | Caminho com conflito autorizado; sanitização anterior em `app/ponto/registro_batida.py:105`. |
| `app/ponto/registro_batida.py:157` | `Batida.objects.create(..., **campos)` | Caminho normal; mesma sanitização. |
| `app/api/views.py:654` | `latitude=latitude`, seguido de longitude | Encaminha `request.data` à porta; leitura em `app/api/views.py:416`, chamada em `app/api/views.py:648`. |
| `app/api/views_core.py:646` | `latitude=latitude`, seguido de longitude | Encaminha `request.data`; leitura em `app/api/views_core.py:403`, chamada em `app/api/views_core.py:640`. |
| `app/ponto/services/triagem_batida.py:707` | `latitude=latitude`, seguido de longitude | Encaminha POST/sessão à porta; chamada em `app/ponto/services/triagem_batida.py:701`. |
| `app/ponto/registro_batida.py:335` | `Batida.objects.bulk_create([Batida(**d) for d in linhas], batch_size=500)` | Semeadura de ensaio, sem sanitização; guard em `app/ponto/registro_batida.py:333`. |

A sanitização percorre latitude/longitude (`app/ponto/registro_batida.py:3`, `app/ponto/registro_batida.py:8`):

- **`0.0` é preservado:** `float(v)` apenas valida, sem substituir o valor (`app/ponto/registro_batida.py:15`).
- **`''` e `None` viram `None`**, assim como valores não numéricos (`app/ponto/registro_batida.py:11`, `app/ponto/registro_batida.py:17`).
- A entrada web já aplica `request.POST.get('latitude') or None` e equivalente de longitude (`app/ponto/views.py:270`, `app/ponto/views.py:271`).
- O chamador localizado da semeadura omite geo no dicionário; os campos são nullable (`app/ponto/management/commands/gerar_batidas_smoke_motor.py:107`, `app/ponto/management/commands/gerar_batidas_smoke_motor.py:182`, `app/ponto/models.py:58`, `app/ponto/models.py:59`).
- A porta de anotação não inclui latitude/longitude nos campos permitidos e rejeita campos externos (`app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`).

Estes produtores chamam `registrar_batida` **sem geo**; numa criação nova, latitude/longitude ficam nulas pelos campos do modelo (`app/ponto/models.py:58`, `app/ponto/models.py:59`):

| Arquivo:linha | Produtor |
|---|---|
| `app/ponto/views.py:1815` | Saída aprovada em lote. |
| `app/ponto/services/justificativa.py:188` | Saída após aprovação. |
| `app/ponto/services/justificativa.py:361` | Ajuste retroativo. |
| `app/ponto/services/veredito_celula.py:322` | Materialização manual. |
| `app/chamados/services/regularizacao_ext.py:77` | Regularização externa. |
| `app/chamados/services/materializacao.py:927` | `disputa_s84_retro`. |
| `app/chamados/services/materializacao.py:1007` | `disputa_ancora`. |
| `app/chamados/services/acoes_chamado.py:44` | `excecao_autorizada`. |

Há teste existente que espera persistência de `0.0/0.0`, conversão de `''/''` para `None/None` e preservação de coordenada parcial (`app/api/tests/test_l030_batida_pela_porta.py:58`, `app/api/tests/test_l030_batida_pela_porta.py:63`, `app/api/tests/test_l030_batida_pela_porta.py:68`). Resultado de execução: **[nao verificado]**.

Para delimitar a contagem operacional: fixtures também contêm decisões de nulidade ao montar dados sintéticos, fora dos 41 — `lat is None` e `lon is None` em `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:101` e `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:102` (**O**, dois); `lat is not None` e `lon is not None` em `app/ponto/tests/test_bo0611_05_templates_geo.py:115` e `app/ponto/tests/test_bo0611_05_templates_geo.py:116` (**N1**, dois).

Há ainda leituras de entrada no servidor, anteriores à gravação: normalização do POST (`app/ponto/views.py:270`, `app/ponto/views.py:271`), truthiness individual para assinatura (`app/api/views.py:464`, `app/api/views.py:465`, `app/api/views_core.py:451`, `app/api/views_core.py:452`), recuperação individual da sessão (`app/ponto/services/triagem_batida.py:507`, `app/ponto/services/triagem_batida.py:509`) e sanitização (`app/ponto/registro_batida.py:11`). Elas não entram no subtotal de leitores de Batida gravada.

Os testes de coordenadas **do Posto** e os pings de outro modelo também ficam fora desse subtotal; exemplos concretos são `app/ponto/services/geofence.py:259`, `app/templates/ponto/bater_ponto_v2.html:139`, `app/ponto/views.py:2941`, `app/api/views.py:2331` e `app/api/views_core.py:865`.

## 10/11 17:13 · sessao 0ca338b4e83a · - · - · origem=Ronald (tmux)

Censo Python: **28 sítios diretos** de Batida/contexto, por condição/expressão, agrupando cada cadeia de exclusão das duas coordenadas em um sítio. Na convenção estrita solicitada pelo pai: **T=2, T1=10, N=5, N1=4, O=7**. Os sete O usam exclusão de `None` ou guarda inversa `is None`; semanticamente são nulidade das duas coordenadas.

| Arquivo:linha | Expressão literal | Forma | Decisão |
|---|---|---|---|
| `app/ponto/services/geofence.py:247` | `not batida.latitude or not batida.longitude` | T | Entra em ausência GPS; com posto cria alerta `sem_gps` em :249–256. |
| `app/colaboradores/services/drawer.py:63` | `bool(b.latitude and b.longitude)` | T | Produz chave `tem_gps` para batida do drawer. |
| `app/ponto/services/geofence.py:327` | `float(batida.latitude) if batida.latitude else None` | T1 | Latitude ou ausência no contexto da ocorrência do chamado. |
| `app/ponto/services/geofence.py:328` | `float(batida.longitude) if batida.longitude else None` | T1 | Longitude ou ausência no contexto da ocorrência. |
| `app/ponto/services/geofence.py:369` | `float(batida.latitude) if batida.latitude else None` | T1 | Latitude ou ausência no contexto legado do chamado. |
| `app/ponto/services/geofence.py:370` | `float(batida.longitude) if batida.longitude else None` | T1 | Longitude ou ausência no contexto legado. |
| `app/core/services/painel_op.py:227` | `entrada.latitude if entrada and entrada.latitude else None` | T1 | Coordenada disponível no contexto do painel. |
| `app/core/services/painel_op.py:228` | `entrada.longitude if entrada and entrada.longitude else None` | T1 | Idem longitude. |
| `app/colaboradores/painel_cell.py:54` | `str(coord[0] or '')` | T1 | Latitude ou string vazia na linha do painel. |
| `app/colaboradores/painel_cell.py:55` | `str(coord[1] or '')` | T1 | Longitude ou string vazia. |
| `app/colaboradores/services/situacional.py:199` | `str((batida_coord.get(c.pk) or (None, None))[0] or '')` | T1 | Latitude ou string vazia na linha do painel em lote. |
| `app/colaboradores/services/situacional.py:200` | `str((batida_coord.get(c.pk) or (None, None))[1] or '')` | T1 | Longitude ou string vazia. |
| `app/chamados/views.py:692` | `_BatidaGeo.objects.filter(colaborador=colaborador, latitude__isnull=False, longitude__isnull=False)` | N | Seleciona última geolocalização e histórico para fio. |
| `app/ponto/views.py:485` | `pendentes.filter(latitude__isnull=False, longitude__isnull=False).count()` | N | Contador `com_gps`; `sem_gps` é complemento em :486. |
| `app/ponto/services/geofence.py:52` | `colaborador=colaborador, latitude__isnull=False, longitude__isnull=False` | N | Seleciona últimas batidas COM GPS em `vinculo_contagem`. |
| `app/ponto/services/vinculo_na_lista.py:49` | `Batida.objects.filter(timestamp__date__gte=ini, latitude__isnull=False, longitude__isnull=False, gps_accuracy__isnull=False)` | N | Seleciona evidência geo de cadastro versus realidade. |
| `app/ponto/services/geofence.py:403` | `la is not None and lo is not None` | N | Seleciona ocorrências do contexto de chamado que entram na reavaliação. |
| `app/ponto/views.py:468` | `str(batida.latitude) if batida.latitude is not None else None` | N1 | Snapshot de auditoria antes de aprovar/apagar offline. |
| `app/ponto/views.py:469` | `str(batida.longitude) if batida.longitude is not None else None` | N1 | Snapshot longitude. |
| `app/core/views.py:1024` | `float(b.latitude) if b.latitude is not None else None` | N1 | JSON do detalhe da batida. |
| `app/core/views.py:1025` | `float(b.longitude) if b.longitude is not None else None` | N1 | JSON longitude. |
| `app/colaboradores/views.py:53` | `Batida.objects.exclude(latitude=None).exclude(longitude=None)` | O | Conjunto de colaboradores com geo para vínculo. |
| `app/colaboradores/views.py:337` | `Batida.objects.filter(colaborador=colaborador).exclude(latitude=None).exclude(longitude=None).exists()` | O | `tem_geo` no retorno de erro HTMX ao vincular posto. |
| `app/colaboradores/views.py:356` | mesma expressão de :337 | O | `tem_geo` no retorno de sucesso HTMX ao vincular posto. |
| `app/colaboradores/views.py:395` | mesma expressão de :337 | O | `tem_geo` na linha ao vincular escala. |
| `app/colaboradores/services/detalhe.py:22` | `).exclude(latitude=None).exclude(longitude=None)` | O | Histórico geo e pontos do mapa. |
| `app/colaboradores/services/detalhe.py:53` | `_todas.exclude(latitude=None).exclude(longitude=None).count()` | O | Contador total geo. |
| `app/colaboradores/views.py:2474` | `not coord or coord[0] is None or coord[1] is None` | O | Pino `sem_gps`, cinza; coordenada vem da última Batida em `painel_cell.py:33–40` e `situacional.py:137–145,179`. |

**Equivalentes em coordenadas copiadas de alerta**, se incluídos pelo escopo, acrescentam **O=2**, total Python **30**:

| Arquivo:linha | Expressão literal | Forma | Decisão |
|---|---|---|---|
| `app/ponto/models.py:263` | `self.latitude is None or self.longitude is None` | O | `AlertaGeofence.ainda_vale`: ausência conserva o alerta. |
| `app/ponto/management/commands/reconciliar_alertas_geofence.py:42` | `lat is None or lon is None or not alvo` | O | Classifica alerta sem dado e pula reconciliação. |

Não contei guardas da mera existência da tupla como novo juiz de latitude/longitude: `b_co or (None, None)` em `painel_cell.py:39`, e os `batida_coord.get(c.pk) or (None, None)` internos em `situacional.py:199–200`.

Não achei função/propriedade/método Python de produção nomeado `tem_gps`, `tem_geo` ou `tem_coordenada` que responda essa pergunta. Existem a chave `tem_gps` em `drawer.py:63` e variáveis `tem_geo` em `colaboradores/views.py:337,356,395`; `_geo_do_pino` existe em `colaboradores/views.py:2454`, mas retorna classificação/cor e testa nulidade em :2474. `classificar_posicao`, em `geofence.py:95`, recebe distância, accuracy e raio, não latitude/longitude. Busca em `core/juizes.py` por geo/gps/coordenada não encontrou registro; ausência global de implementação com nome arbitrário: **[nao verificado]**.

A varredura AST de produção feita por este explorador terminou sem erro de parse; o pai informou parse total de 3299/3299 Python.

Leituras Python de **entrada anterior à gravação**, separadas dos 28:

- `app/ponto/views.py:270–271`: `request.POST.get('latitude') or None` e longitude.
- `app/api/views.py:464–465`, `app/api/views_core.py:451–452`: `latitude or ''` e `longitude or ''`, assinatura.
- `app/ponto/services/triagem_batida.py:507,509`: `not latitude` / `not longitude`, recuperação de confirmação na sessão.
- `app/ponto/registro_batida.py:11`: `v in ('', None)`, com campos latitude/longitude declarados em :3; sanitização antes de criação.
- Pings de outro modelo: `lat is None or lng is None` em `app/ponto/views.py:2941`, `app/api/views.py:2331` e `app/api/views_core.py:865`.

As formas Python N/N1 e as sete O de nulidade diferem da forma T do geofence; os T1 consultam apenas um campo e não constituem o mesmo predicado conjunto de `geofence.py:247`.

## 10/11 17:12 · sessao e2df1f633553 · - · - · origem=Ronald (tmux)

Censo de escritores por AST (`3299` Python lidos, sem erro de parse) e grep; nenhum arquivo alterado. Execução de testes e persistência real em produção: [nao verificado]. [nao verificado]

| Sítio | Expressão/origem | Valores especiais |
|---|---|---|
| `app/ponto/registro_batida.py:152` e `:157` | `Batida.objects.create(..., **campos)` | Os dois caminhos recebem `_sanitiza_geo(campos)` em `:105`. |
| `app/ponto/registro_batida.py:11` | `if v in ('', None): campos[k] = None` | `''` e `None` viram `None`. |
| `app/ponto/registro_batida.py:15` | `float(v)` | Só valida; não altera o valor numérico, preservando `0.0`. Não numérico vira `None` em `:16-17`. |
| `app/api/views.py:648` | `registrar_batida(... latitude=latitude, longitude=longitude ...)` (`:654-655`) | Origem `request.data.get(...)` em `:416-417`; zero segue à porta; vazio/ausência seguem à sanitização. |
| `app/api/views_core.py:640` | Mesmo encaminhamento (`:646-647`) | Origem `request.data.get(...)` em `:403-404`; mesma sanitização. |
| `app/ponto/services/triagem_batida.py:701` | Mesmo encaminhamento (`:707-708`) | Origem POST em `app/ponto/views.py:270-271`: `request.POST.get(...) or None`; recuperação de sessão em `triagem_batida.py:507-510`, chamada em `:659`. String `'0'` é preservada; `''` vira `None`. |
| `app/ponto/registro_batida.py:335` | `Batida.objects.bulk_create([Batida(**d) for d in linhas], batch_size=500)` | Caminho `semear`, guardado para banco de ensaio em `:333`; não chama `_sanitiza_geo`. Único chamador localizado: `app/ponto/management/commands/gerar_batidas_smoke_motor.py:107`; seu dict omite geo em `:182-192`, produzindo campos nulos (`app/ponto/models.py:58-59`). |

Outros produtores chamam a porta **sem** latitude/longitude; quando criam uma nova Batida, os campos são nulos (`app/ponto/models.py:58-59`):

- `app/ponto/views.py:1815`: saída aprovada em lote.
- `app/ponto/services/justificativa.py:188`: saída após aprovação.
- `app/ponto/services/justificativa.py:361`: ajuste retroativo.
- `app/ponto/services/veredito_celula.py:322`: materialização manual.
- `app/chamados/services/regularizacao_ext.py:77`: regularização externa.
- `app/chamados/services/materializacao.py:927`: `disputa_s84_retro`.
- `app/chamados/services/materializacao.py:1007`: `disputa_ancora`.
- `app/chamados/services/acoes_chamado.py:44`: `excecao_autorizada`.

O caminho de anotação não permite alterar geo: `CAMPOS_ANOTAVEIS` omite latitude/longitude em `app/ponto/registro_batida.py:203`, e o guard rejeita campos externos em `:241-243`.

O teste existente caracteriza explicitamente `0.0/0.0` persistido em `app/api/tests/test_l030_batida_pela_porta.py:58`, `''/'' → None/None` em `:63`, e coordenada parcial em `:68`. Resultado de execução desses testes: [nao verificado].

Expressão exata do geofence: `if not batida.latitude or not batida.longitude:` em `app/ponto/services/geofence.py:247`. Se houver posto (`:248`), cria **AlertaGeofence**, não altera Batida: `tipo='sem_gps'` em `:252`, latitude/longitude do alerta `None` em `:253-254`. O contexto do chamado normaliza cada componente separadamente: `float(batida.latitude) if batida.latitude else None` em `:327` e `:369`; equivalente de longitude em `:328` e `:370`.

Não localizei função/propriedade/método booleano nomeado que seja a autoridade de “Batida tem coordenadas”; inexistência absoluta: [nao verificado]. Os candidatos concretos são:

- `app/colaboradores/services/drawer.py:63`: chave de dict `'tem_gps': bool(b.latitude and b.longitude)`, não método/propriedade.
- `app/colaboradores/views.py:337`, `:356`, `:395`: variáveis locais `tem_geo` derivadas de queryset.
- `app/colaboradores/views.py:2454`: função classificadora `_geo_do_pino(coord, posto)`, com presença por `if not coord or coord[0] is None or coord[1] is None:` em `:2474`.
- `app/core/juizes.py:1191`: autoridade registrada é `classificar_posicao` para “a batida esta dentro do geofence?”, não para presença de coordenadas.

## 10/11 17:12 · sessao ca2040610eb0 · - · - · origem=Ronald (tmux)

Censo HTML/JS direto de presença: **11 sítios** para Batida/contexto/cópia em Alerta: **N=4, T=2, T1=2, O=3, N1=0** (linhas da tabela).

| arquivo:linha | expressão literal | forma | decisão |
|---|---|---|---|
| `app/templates/core/_d2_feed.html:16` | `b.latitude is not None and b.longitude is not None` | N | Exibe pino/link Google Maps; link em :17. |
| `app/templates/ponto/batidas_offline_pendentes.html:96` | `batida.latitude is not None and batida.longitude is not None` | N | Exibe link Google Maps; link em :97. |
| `app/templates/chamados/partials/modal_fio.html:480` | `chamado.modulo_origem == "gps_fora_geofence" and chamado.contexto_json.batida_lat is not None and chamado.contexto_json.batida_lon is not None` | N | Habilita bloco GPS e botão mapa do chamado; dados do botão em :484. |
| `app/templates/chamados/partials/_relatorio_geo.html:30` | `chamado.contexto_json.batida_lat is not None and chamado.contexto_json.batida_lon is not None` | N | Exibe coordenadas e rota posto→batida; :31–32. |
| `app/templates/core/dashboard2.html:242` | `(b.latitude && b.longitude)` | T | Define `gpsBlock`: mapa/coordenadas/link ou “Sem localizacao”; :242–246. |
| `app/templates/core/dashboard2.html:287` | `b.latitude && b.longitude` | T | Inicializa mapa e pino do drawer; :288–301. |
| `app/templates/colaboradores/detalhe.html:142` | `a.latitude` | T1 | Exibe link mapa da cópia em Alerta; :128, :135, :143. |
| `app/templates/core/dashboard_supervisor.html:90` | `a.latitude` | T1 | Exibe link mapa do Alerta, senão “—”; :77, :84, :91–94. |
| `app/static/js/geo-colab-map.js:39` | `isNaN(blat) || isNaN(blon)` | O | Dados de batida recente: informa ausência e retorna ou desenha mapa/pino; parse em :29, efeito :40–41, pino :65. Reutilizado pelo chamado via `abrirGeoBatida`, :94–106. |
| `app/templates/chamados/fio_colaborador.html:313` | `!window.L||isNaN(blat)||isNaN(blon)` | O | Informa “Sem coordenada de GPS.” e retorna ou desenha mapa/pino; parse :301, pino :318. Mistura disponibilidade da biblioteca com validade numérica. |
| `app/templates/chamados/painel_gestao.html:583` | `isNaN(blat) || isNaN(blon)` | O | Informa “Sem coordenada de GPS nesta batida.” e retorna ou desenha mapa/pino; parse :571, pino :592. |

Os quatro N, dois T1 e três O divergem da expressão de truthiness das duas coordenadas do geofence. O significado exato para cada entrada Python/JSON precisa acompanhar tipagem/conversão: JS `isNaN(parseFloat(...))` aceita zero numérico; a origem do parse está nas linhas citadas.

**Armazenamento local no navegador, antes da sincronização**, separado de Batida já gravada no banco:

| arquivo:linha | expressão literal | forma | decisão |
|---|---|---|---|
| `app/static/pwa.js:145` | `b.latitude != null` | O pela legenda literal do pedido; equivalente JS a teste não nulo de uma coordenada | Inclui latitude no POST de sincronização. |
| `app/static/pwa.js:146` | `b.longitude != null` | O pela legenda literal do pedido; equivalente JS a teste não nulo de uma coordenada | Inclui longitude no POST de sincronização. |

`b` vem de `batidas_pendentes` do IndexedDB (`app/static/pwa.js:130–138`); essas coordenadas são guardadas em `app/static/pwa.js:116`. Se o consolidado decidir agrupar equivalentes JS `!= null` como N1, serão **dois N1 locais**, explicitando a convenção.

Há consumidores de resultados já calculados, sem ler/refazer latitude/longitude; **não somar aos 11 juízos diretos**:

- `app/templates/colaboradores/partials/_drawer_diagnostico.html:49`: `{% if b.tem_gps %}` aparece **duas vezes**; cor e texto `ok/sem`. Produtor é chave de dict, `'tem_gps': bool(b.latitude and b.longitude)`, em `app/colaboradores/services/drawer.py:63`; não prova método/propriedade de Batida.
- `app/templates/colaboradores/partials/vinculo_row.html:12`: `{% if d.tem_geo %}` controla selo 📍; chave produzida em `app/colaboradores/views.py:84`.
- `app/templates/colaboradores/partials/_ps_linha.html:16–18`: `l.geo == 'verde'` / `l.geo == 'vermelho'` / else escolhe selo; coordenadas chegam como `l.b_lat/l.b_lon` na :1. Há decisões `or ''` nos produtores Python `app/colaboradores/painel_cell.py:54–55` e `app/colaboradores/services/situacional.py:199–200`, a consolidar pelo explorador Python.
- `app/templates/chamados/fio_colaborador.html:112`: `ultima_geo` controla bloco de última localização; histórico em :119. A escolha por coordenadas ocorre no produtor Python, não nesses testes de objetos.
- `app/templates/colaboradores/detalhe.html:165` e :222 usam `batidas_geo`; JS :226 testa `!pontos.length`. A filtragem por coordenadas ocorre no produtor Python.

**Captura antes do POST — somente listar:**

- `app/templates/ponto/bater_ponto.html:293–305`: suporte/API de geolocation, captura e preenchimento; timeout em :482–487; `!lat || !gpsCapturedAt || (Date.now() - gpsCapturedAt) > 60000` em :503; `if (gps)` em :505; `!lat || !lng` em :509.
- `app/templates/ponto/triagem/base_triagem.html:56–68`: suporte/API e captura para `.campo-lat/.campo-lng`.
- `app/templates/ponto/triagem/apto.html:200–205`: suporte/API e captura com timeout; `!lat || !gpsCapturedAt || (Date.now() - gpsCapturedAt) > 60000` em :217; `if (gps)` em :219; `lat && lng` em :221; `gpsLat == null || gpsLng == null` em :276.
- `app/templates/ponto/bater_ponto_v2.html:177`: `gpsLat!=null && gpsAccuracy!=null && acc>gpsAccuracy && (Date.now()-gpsCapturedAt)<120000`; suporte/API em :184–195; `gpsLat!=null` em :189; `gpsLat==null` em :204; suporte/timeout em :273–278; `gpsLat==null || gpsLng==null || !gpsCapturedAt || (Date.now()-gpsCapturedAt)>60000` em :292; `!(g && g.lat!=null)` em :294; `g && g.lat!=null` em :295.
- `app/static/pwa.js:289–301`: `capturarGPS`, suporte/API, resultado com coordenadas ou `null`.
- `app/static/pwa.js:394–411`: suporte/API e captura para POST `/ponto/ping-geo/`.
- `app/templates/core/onboarding.html:264–268`: teste de suporte e chamada geolocation para permissão; callback não lê latitude/longitude nessa chamada.

Excluídos das contagens Batida: testes de coordenadas **do Posto**, por exemplo `app/templates/ponto/bater_ponto_v2.html:139`, `app/templates/colaboradores/posto_form.html:103/:107/:211/:214`, `app/templates/colaboradores/postos.html:71`, `app/templates/colaboradores/partials/painel_posto.html:28`, `_postos_tabela.html:23`, `app/templates/core/painel_tv.html:154`; e testes `plat/plon` nos mapas (`geo-colab-map.js:66`, `fio_colaborador.html:319`, `painel_gestao.html:594`).

## 10/11 17:05 · sessao e54c7972b4a6 · - · - · origem=Ronald (tmux)

Encontrei quatro pontos materiais:

- **I7b aparece como violação.** O renderizador marca qualquer contador positivo com `VIOL` e vermelho, contrariando “número publicado sem esperado”. A descrição sozinha não altera essa classificação. `auditar_invariantes_chamados.py:74`, `:1901-1915`. [nao verificado]

- **União de dias pode atribuir HX ao dia errado.** O helper reúne datas do contexto, pai e turno da batida; basta qualquer uma responder `sem_celula`. Num conflito entre dia civil D+1 e turno D, a ausência de ata em D+1 pode esconder a violação de D com ata. O teste atual cobre somente `ultima_batida`, sem conflito entre fontes. `auditar_invariantes_chamados.py:32-43`, `:69-71`; `test_bo_disputa_sem_pergunta_memoria.py:366-370`. [nao verificado]

- **Vários motivos com donas diferentes vão todos para a primeira dona.** A primeira pergunta estrangeira interrompe a seleção; a recursão passa `motivos_lista` inteira à dona encontrada, e a agregação ignora os retornos seguintes do gerador. O teste cobre um único motivo. `disputa_emissao.py:555-586`, `:616-637`; `test_bo_disputa_sem_pergunta.py:68-93`. [nao verificado]

- **A tentativa inicial pode adquirir D depois de Ch.** O núcleo segura chamados vivos antes de gerar. Uma canônica de disputa fechada que precise rechaveamento adquire a dona em `models.py:2947-2950`, antes de o núcleo detectar o reuso e desfazer o ensaio. A segunda tentativa trava a dona corretamente, mas isso não corrige a ordem da primeira. Os testes substituem a porta e não exercitam esse caminho. `disputa_emissao.py:543-545`, `:615-618`; `test_bo_disputa_sem_pergunta.py:81`. [nao verificado]

A prova AST da API é razoável para a fronteira estática solicitada: verifica ordem das chamadas e ausência de `atomic` envolvendo o registro, além de `ATOMIC_REQUESTS` nos settings. Não é prova de execução da API. `test_bo_disputa_sem_pergunta_memoria.py:374-389`.

## 10/11 17:00 · sessao f212df288509 · BO-0611-05 · censo · origem=Code

Numeração dos anexos: primeira linha de conteúdo = 1, contando linhas vazias.

1. **(b) Falha da triagem.** O tema estava perguntado: “supoe outro significado (None x 0.0 x '', Decimal x float” (`preverif1.q:23`). O Codex viu: “geofence usa truthiness, enquanto os leitores alterados usam `is not None`; zero permanece na exclusão declarada” (`preverif1.r:37`). A triagem descartou literalmente: **“- 0.0 truthiness x is not None: fora do escopo declarado (brief volta 3); anexo ao opus.”** Motivo registrado: exclusão de escopo (`triagem_preverif1.txt:13`).

2. **(c) Fora do alcance — não conta.** O achado cobra evidência do RED **executado contra a base**, não apenas mutantes ou inspeção (`opus_final1.txt:5`). A pergunta exigia: “RED/GREEN exigidos (aval): POST /api/ponto/bater/ sem coordenadas, colab ativo, nos DOIS kernels -> HOJE 403 e 0 Batida” (`construtor1.q:33`). A resposta delimitou: “POSTs reais, renderizações Django e os 100/100 relatados não foram reexecutados aqui: **[nao verificado]**” (`preverif1.r:66`). Não há trecho de resposta comprovando esse RED na base; RED executado pertence expressamente à classe (c) (`CLAUDE.md:383`).

3. **(e) NÃO PERGUNTADO — falha de quem perguntou.** Não há trecho de pergunta solicitando conferir `LEI-AKITA`/`L-030` nas mensagens dos commits. O trecho próximo é “pouso antes do pre-requisito”, dentro da pergunta sobre classes de defeitos; isso pergunta por ordem do pouso, sem formular a conferência das marcações legais (`preverif1.q:24–26`; tema do achado em `opus_final1.txt:6`). Não há trecho correspondente nas `.r` fornecidas nem descarte correspondente na triagem (`triagem_preverif1.txt:1–13`). Respostas `construtor1.r`–`construtor6.r`: **[nao verificado]**, não fornecidas. [nao verificado]

4. **(e) NÃO PERGUNTADO — falha de quem perguntou.** A pergunta cita “PELA-METADE em app/docs/LEIS.md:78 e :165” como contexto do aval; não pergunta se esses registros ficariam falsos após a cura (`censo1.q:1–3`). A construção também usa “L-030 PELA-METADE” como especificação (`construtor1.q:2`). Não há trecho `.q` pedindo atualização documental por L-106, trecho `.r` apontando essa desatualização ou descarte correspondente na triagem (`opus_final1.txt:7`; `triagem_preverif1.txt:1–13`).

Evidência de avaliação por opus anterior, necessária para atribuir **(d)**: **[nao verificado]**, não fornecida.

| achado | classe | trecho .q | trecho .r | linha da triagem |
|---|---|---|---|---|
| 1 — zero/nulo | **(b)** falha da triagem | “supoe outro significado (None x 0.0 x ''” — `preverif1.q:23` | “geofence usa truthiness, enquanto os leitores alterados usam `is not None`” — `preverif1.r:37` | “- 0.0 truthiness x is not None: fora do escopo declarado (brief volta 3); anexo ao opus.” — `triagem_preverif1.txt:13` |
| 2 — RED na base | **(c)** não conta | “RED/GREEN exigidos […] HOJE 403 e 0 Batida” — `construtor1.q:33` | “POSTs reais […] não foram reexecutados aqui” — `preverif1.r:66`; não há comprovação do RED na base | Não há trecho correspondente — `triagem_preverif1.txt:1–13` |
| 3 — marcações legais dos commits | **(e)** não perguntado | Não há trecho específico; “pouso antes do pre-requisito” — `preverif1.q:25` — não formula essa conferência | Não há trecho correspondente nas respostas fornecidas | Não há trecho correspondente — `triagem_preverif1.txt:1–13` | [nao verificado]
| 4 — atualização de LEIS.md | **(e)** não perguntado | Não há pergunta de atualização; “PELA-METADE em app/docs/LEIS.md:78 e :165” é contexto — `censo1.q:2` | Não há trecho correspondente nas respostas fornecidas | Não há trecho correspondente — `triagem_preverif1.txt:1–13` |

## 10/11 16:58 · sessao e54c7972b4a6 · - · - · origem=Ronald (tmux)

Para HX :58, o dia vem do chamado órfão: o teste grava `contexto_json.data_turno`; o agrupador preserva seu ID em `DS.contexto.chamados_orfaos_ids` e no contexto do pai. O leitor exato do ramo sem origem é `data_evento_do_contexto`. ([teste](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_hx_disputa_muda_sem_ata.py:46), [IDs](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:1126), [leitura](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:1232)) [nao verificado]

Os outros leitores já usados para decidir a abstenção são `data_do_chamado`, no ramo por ata, e `resolver_data_evento`, no ramo com batida de origem. Este último devolve o dia do turno pela autoridade `_T1`. ([por ata](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:1169), [por origem](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:1205), [resolver](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/escala/servico_jornada.py:474))

Evite `dia_do_fato(..., fortes=False)` como substituto universal: seus últimos degraus aceitam o dia civil do instante observado. `fortes=True` encerra antes desses degraus. ([guarda](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/juizes.py:127), [significado civil](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/juizes.py:141))

O reuso conserva `ex.disputa_id`, mas pode costurar destino/célula antes de retornar. Portanto, o rollback precisa abranger toda a geração, inclusive perguntas próprias emitidas anteriormente na mesma lista. ([costura e retorno](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/models.py:2958))

A agregação na dona exige seleção explícita: o ramo atual consulta apenas disputas abertas. Uma dona fechada deve receber o motivo sem reabertura paralela; a necessidade dessa seleção decorre do filtro atual. ([seleção](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:532))

A dona fechada também deve entrar na trava antes de chamado/pergunta. A ordem declarada é Colaborador → Disputa → Chamado → Pergunta; após rollback do nascimento, reentrar na porta com a dona permite respeitá-la. ([contrato da trava](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/models.py:2629))

Nenhum arquivo escrito, banco ou rede acessado nesta exploração. [nao verificado]

## 10/11 16:57 · sessao e439e679001b · - · - · origem=Ronald (tmux)

**Achado 3: (e) NÃO PERGUNTADO.** Nenhuma `.q` solicita auditar mensagens de commit por `LEI-AKITA`/`L-030`, nem conferir `test_lei_protege_sitio` para esse pouso. O trecho mais próximo, em `preverif1.q`, é “**pouso antes do pre-requisito**”: pergunta sobre ordem/dependência do pouso, sem perguntar pelas marcações legais dos commits. Não há trecho `.r` que identifique a falta dessas marcações. A resposta apenas declara: “**Ordem efetiva das travas, liberação da vaga, pouso, smoke nas duas cascas com col677 e certificação independente do hash/base: [nao verificado]**” (`preverif1.r`, último parágrafo). Não há descarte correspondente na triagem fornecida. As linhas devem ser numeradas pelo texto do anexo; não conferi numeração. [nao verificado]

**Achado 4: (e) NÃO PERGUNTADO.** Citar o conteúdo de `LEIS.md` como especificação não equivale a perguntar se a documentação deveria ser atualizada após a obra. `censo1.q` abre com “**L-030 PELA-METADE em app/docs/LEIS.md:78 e :165**”; `construtor1.q` diz “**lei CLAUDE.md secao 4 / L-030 (app/docs/LEIS.md, linha L-030 PELA-METADE)**”. Nenhum desses trechos pede conferir a veracidade final de “BARRA em 4 casos”/“PELA-METADE” ou atualizar `LEIS.md` por L-106. Não há trecho `.r` diagnosticando documentação desatualizada, nem descarte desse diagnóstico na triagem fornecida.

**Ressalva para a consolidação:** a menção ao selo “VERMELHO” no achado 3 é uma alegação do opus anexado; sua execução não foi demonstrada por log nos anexos. Isso não torna o tema específico de marcação legal dos commits uma pergunta previamente feita. Não há evidência de opus anterior suficiente para sustentar classe (d). [nao verificado]

## 10/11 16:25 · sessao c29c5b84bc96 · DISPUTA-ZUMBI-1110 · censo · origem=Code

**A cópia contradiz a ausência de leitor após `sem_turno_aberto`:** o carimbo ocorre em `app/chamados/reconciliador.py:405`; depois, a mesma função consulta `pode_fechar()` em `:411` e chama `fechar()` em `:412`. O chamador indicado está em `app/chamados/reconciliador.py:1619`. A execução histórica desses trechos nas 18 disputas do anexo é [nao verificado].

Os números **18 abertas fecháveis, 10 com última resolução por `sem_turno_aberto`, pais 7 abertos/7 resolvidos/4 fechados** são os dados fornecidos pelo Ronald; não houve conferência independente em banco ou logs. [nao verificado]

C7 significa **nenhuma resposta sem veredito e nenhuma pergunta viva pelo C6**; não exige pai terminal nem disputa aberta. Essas condições adicionais pertencem aos chamadores. A disputa está aberta quando `fechada_em is None`; `estado` da pergunta é propriedade calculada, não coluna escrita. (`app/chamados/models.py:1842–1853`, `app/chamados/models.py:1822–1824`, `app/chamados/models.py:3033–3048`)

Para ler as tabelas, **B** significa o caminho comum de fechamento condicionado: `PerguntaDisputa.save()` → `post_save` → `on_commit` → `fechar_beco_da_disputa`. Ele **só chega a C7** se a disputa estiver aberta, o pai estiver em `TERMINAIS` e não existir pergunta com `respondida_em=None` cujo destino não seja terminal; há também guarda de reentrada. Portanto, B não significa consulta garantida a C7 após cada carimbo. (`app/chamados/signals.py:227–238`, `app/chamados/services/becos.py:25–33`, `app/chamados/services/becos.py:66–78`)

**1–2. Escritores persistentes dos campos da pergunta e fechamento posterior**

| Escritor físico | O que grava | Depois da escrita |
|---|---|---|
| `_carimbar_validacao` — `app/chamados/services/validacao.py:134–137` | `validada_em/por` | **B**, mais o fluxo normal de validação/materialização que termina em C7, detalhado abaixo. (`app/chamados/services/validacao.py:435–469`, `app/chamados/services/materializacao.py:1378`) |
| `devolver_validacao_recusada` — `app/chamados/services/validacao.py:199–202` | Zera `validada_em/por` | **B**. Nenhum fechamento direto na função; pode reabrir a disputa. O chamador normal depois materializa a disputa. (`app/chamados/services/validacao.py:209–230`, `app/chamados/services/validacao.py:464–469`) |
| `responder_pergunta_disputa_core` — `app/chamados/services/disputa_emissao.py:1467–1508` | Resposta, `respondida_em`, desfaz validação anterior e carimbo obsoleto | **B**. Nenhum C7 direto até o retorno; resposta de ausência chama materialização que consulta C7. (`app/chamados/services/disputa_emissao.py:1538–1559`, `app/chamados/services/materializacao.py:585–586`) |
| `responder_automaticamente` — `app/chamados/services/disputa_emissao.py:1652–1655` | Resposta e `respondida_em` | **B**. Chamador da foto valida pelo dono logo depois, alcançando a materialização. (`app/api/foto_recebida.py:43–51`) |
| `carimbar_resposta_pela_trilha` — `app/chamados/services/disputa_emissao.py:1674–1676` | `respondida_em` | **B**. Nenhum C7 direto na função ou no chamador `curar_perguntas_em_limbo`, que carimba/reabre e imprime o resultado. (`app/chamados/services/disputa_emissao.py:1677–1682`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:64–70`) |
| `PerguntaDisputa.carimbar` — `app/chamados/models.py:3128–3152` | `materializada_em/por`, `via_resolucao`; opcionalmente `respondida_em` e `validada_em/por` | Consulta **C6 antes de salvar**, não C7. Depois: **B**; fechamento explícito depende do chamador, discriminado na próxima tabela. (`app/chamados/models.py:3139–3157`) |
| `carimbar_identidade` — `app/chamados/models.py:3182–3190` | Carimbo/via; opcionalmente `recusa_motivo` | **B**. Nenhum C7 direto no corpo; fechamento depende do chamador. (`app/chamados/models.py:3168–3191`) |
| `descarimbar` — `app/chamados/models.py:3208–3212` | Zera materialização/autoria/via | **B**, quando `salvar=True`. Nenhum C7 direto no corpo; `salvar=False` apenas devolve campos para o chamador persistir. (`app/chamados/models.py:3203–3213`) |
| `descarimbar_em_lote` — `app/chamados/models.py:3264–3266` | `QuerySet.update`: zera materialização/autoria/via e recebe outros campos | **Nenhum** no corpo; retorna diretamente o `update`. Chamadores atuais: ressuscitação e reabertura com desfazimento de absorção. (`app/chamados/models.py:1566`, `app/chamados/services/disputa_emissao.py:1630`) |
| `reabrir_para_correcao` — `app/chamados/services/disputa_emissao.py:1593`, `:1629–1634` | `QuerySet.update`: zera resposta, resposta-em, validação; opcionalmente descarimba absorção pela porta anterior | **Nenhum C7/B próprio**: a escrita não passa por `post_save`; emite evento de célula explicitamente. (`app/chamados/services/disputa_emissao.py:1607–1609`, `:1629–1634`) |
| `encerrar_por_fato_no_espelho` — `app/chamados/models.py:3275–3276` | Troca a via para `fato_materializado` | **B** e fechamento explícito pelo comando chamador, após consultar `fio_vivo`. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`, `:80–93`) |
| `relavrar` — `app/chamados/services/relavra_via.py:125–128` | Troca `via_resolucao` e atualiza lavra no mesmo `save` | **B**. Nenhum C7 direto na função nem no comando chamador. (`app/chamados/services/relavra_via.py:129–139`, `app/chamados/management/commands/relavrar_via_i6.py:100–104`) |
| `_nascer`, reuso — `app/chamados/models.py:2965–3006` | Pode trocar célula e destino; atualiza lavra | **B**. Após salvar, acorda célula anterior e retorna; nenhum C7 direto nessa cauda. (`app/chamados/models.py:3006–3008`) |
| `_nascer`, inserção — `app/chamados/models.py:3014–3017` | Identidade, disputa, motivo, destino, célula, conversa, dia e `**campos`, que podem conter assinaturas recebidas | **B**, inclusive no nascimento. Nenhum C7 direto antes do retorno. (`app/chamados/models.py:3018–3031`, `app/chamados/signals.py:228–229`) |
| `reapontar_resolvedora` — `app/chamados/services/fio.py:879`, `:925–928` | Destino e, ao datar linha sem dia, `data_evento`; atualiza lavra | **B**. Nenhum C7 direto na cauda; acorda também a célula anterior. (`app/chamados/services/fio.py:929–935`) |
| `rechavear_data_evento` — `app/chamados/services/fio.py:1037–1043` | Dia, célula e lavra | **B**. Nenhum C7 direto na cauda; acorda célula anterior e registra trilha. (`app/chamados/services/fio.py:1044–1049`) |

O ponto que atribui `data_evento` é `gravar_dia_do_fato`; ele apenas modifica memória, e os três fluxos persistentes estão na tabela acima. `_escrever_carimbo` também apenas atribui em memória; quem persiste são `carimbar`, `carimbar_identidade` e `descarimbar`. (`app/chamados/services/fio.py:790–801`, `app/chamados/models.py:3097–3101`)

Abaixo estão os **gatilhos de carimbo**, distinguindo os caminhos que compartilham o mesmo escritor físico:

| Carimbador/gatilho | Consulta C7 ou chama fechamento depois? Onde e condições |
|---|---|
| `lavrar_sem_turno_aberto` → `carimbar` — `app/chamados/reconciliador.py:405` | **Sim, direto:** aberta + C7 em `:411`; `fechar` em `:412`. Sem condição adicional de status do pai, flag ou tranca nesse fechamento. (`app/chamados/reconciliador.py:411–414`) |
| `retratar_perguntas_absolvidas_por_celula` → `carimbar(celula_absolveu)` — `app/chamados/reconciliador.py:595` | **Sim:** após o laço, relê `fechada_em`, consulta C7 e fecha. (`app/chamados/reconciliador.py:608–616`) |
| `encerrar_orfas_pela_lampada` → `encerrar_fantasma` → `carimbar` — `app/chamados/reconciliador.py:1147`, `app/chamados/models.py:3287` | **Sim:** após o laço; barra fechada e resposta sem veredito antes de C7/`fechar`. (`app/chamados/reconciliador.py:1158–1174`) |
| Validação normal → `_carimbar_validacao` — `app/chamados/services/validacao.py:435` | **Sim pelo fluxo:** unidade individual, eventual devolução da validação e materialização da disputa; esta chama `_fechar_disputa_se_quitada`, que consulta C7 e fecha. (`app/chamados/services/validacao.py:461–469`, `app/chamados/services/materializacao.py:1375–1379`, `:1316–1333`) |
| Aceite de folga → validação + `fechar_por_admin` — `app/chamados/services/validacao.py:288–296`, `:320–329` | **B**; também fecha o fio do dia, reconcilia chamados e, quando há transição a encerrado, alcança S143. Não há consulta direta a C7 nesse ramo de aceite. (`app/ponto/services/veredito_celula.py:179–205`, `app/chamados/signals.py:90–115`) |
| `_materializar_ausencia_de_resposta` → carimbo das irmãs — `app/chamados/services/materializacao.py:549–566` | **Sim, direto:** C7/`fechar` em `:585–586`, sem exigir pai terminal. A reconciliação dos destinos tem condição separada: ausência fora de `ABERTAS`. (`app/chamados/services/materializacao.py:580–586`) |
| `_materializar_uma` → declaração/via do fato — `app/chamados/services/materializacao.py:1277–1278` | **Sim pela cauda dos chamadores:** laço normal em `:1378`; validação em `app/chamados/services/validacao.py:469`; passivo em `app/chamados/services/passivo_validado.py:405`. Todos chegam a C7 em `app/chamados/services/materializacao.py:1316`. |
| Recusa de paridade → identidade `paridade_recusada` — `app/chamados/services/materializacao.py:924` | **B** e cauda da materialização nos chamadores anteriores; a unidade não consulta C7 por si. (`app/chamados/services/materializacao.py:1299`, `:1375–1379`) |
| `_carimbar_recusa`, colisão → identidade `fato_ocupado` — `app/chamados/services/materializacao.py:1093–1100` | **B**. Nenhum C7 dentro da função; integra a materialização. Há também gravação de recusa pelo passivo após desfazer o grupo por geometria. (`app/chamados/services/materializacao.py:1101–1107`, `:1296–1299`, `app/chamados/services/passivo_validado.py:543–546`) |
| `materializar_pendentes_aprovadas` → `juiz_aprovou` — `app/chamados/services/materializacao.py:1431` | **B**, nenhum C7 direto na função. Seu gatilho é o signal de disputa **já fechada**; pula pergunta em voo e lastro reprovado. (`app/chamados/signals.py:147–165`, `app/chamados/services/materializacao.py:1425–1433`) |
| S143, `_resolver_perguntas_por_chamado_resolvido`, ramo por destino — `app/chamados/services/disputa_emissao.py:1863–1864` | **Sim pela cauda:** C7/`fechar` em `:1963–1964`; pula fechamento se houver pergunta em voo. O encerramento pela tranca retorna antes desse trabalho. (`app/chamados/services/disputa_emissao.py:1818–1819`, `:1961–1964`) |
| Mesmo S143, ramo sem credor — `app/chamados/services/disputa_emissao.py:1935–1937` | **Sim, mesma cauda.** Esse ramo de carimbo exige pai e destinos fora de `VIVOS`. (`app/chamados/services/disputa_emissao.py:1909–1915`, `:1961–1964`) |
| `DisputaSupervisao.fechar`, exceções admin/desligamento → `fechar_por_admin` — `app/chamados/models.py:1918–1921` | **Já está na porta de fechamento:** após carimbar perguntas, grava o fechamento da disputa. As exceções permitem pergunta viva, mas continuam recusando resposta sem veredito. (`app/chamados/models.py:1886–1897`, `:1932–1936`) |
| `remover_motivo` → identidade `motivo_retratado` — `app/chamados/models.py:2061–2062` | **B**; diretamente, só chama `fechar` quando não resta motivo. Se restarem, retorna sem fechamento direto. (`app/chamados/models.py:2066–2072`) |
| `retratar_duplicata` → identidade `duplicata` — `app/chamados/services/fio.py:1359–1362` | **B**. Nenhum C7/fechamento direto na função nem em seu chamador imediato `retratar_para_canonica`. (`app/chamados/services/fio.py:1336–1342`, `:1345–1363`) |
| `ressuscitar_perguntas` → descarimbo em lote — `app/chamados/models.py:1566` | **Nenhum fechamento nesse caminho lido:** faz `update`, emite evento e reabre disputas fechadas; não passa por B. (`app/chamados/models.py:1565–1575`) |
| `desfazer_carimbo_sem_lastro` → `descarimbar` — `app/chamados/services/relavra_via.py:246` | **B**. Nenhum fechamento direto; reabre disputa fechada. (`app/chamados/services/relavra_via.py:259–263`) |
| `reabrir_fantasma` → `descarimbar` — `app/chamados/models.py:3298` | **B**. Nenhum fechamento direto; o comando chamador reabre disputa e pai. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:157–172`) |
| `reabrir_por_competencia` → `descarimbar` — `app/chamados/models.py:3322` | **B**. Nenhum fechamento direto no caminho lido; o serviço chama essa porta e reabre chamados da competência. (`app/chamados/services/competencia_trancada.py:251–273`) |
| `encerrar_perguntas_com_fato` → troca de via — `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68` | **Sim, porta diretamente:** sob `--apply`, consulta `fio_vivo` e chama `fechar`; contabiliza recusa da porta. Não chama nominalmente C7. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:53–55`, `:80–93`) |

Há ainda estes saves auxiliares, para não confundi-los com coluna de estado ou deixar `bulk_update` fora do inventário:

| Escritor auxiliar | Campos e consequência |
|---|---|
| `_materializar_resposta_hora` — `app/chamados/services/materializacao.py:850–851` | `pendencia_admin/recusa_motivo`; **B**, mais o fluxo da unidade/materialização. (`app/chamados/services/materializacao.py:1375–1379`) |
| `_carimbar_recusa`, sem colisão — `app/chamados/services/materializacao.py:1101–1103` | Só motivo; **B**, nenhum C7 na função. (`app/chamados/services/materializacao.py:1104–1107`) |
| `recusar_pergunta` — `app/chamados/services/validacao.py:584–594` | Salva motivo e reabre resposta por `update`; **B** do save do motivo observa o estado final no commit. Nenhum fechamento direto nessa cauda. (`app/chamados/services/validacao.py:595–610`) |
| `conferir_data` — `app/chamados/services/validacao.py:664–666` | Assinatura da conferência; **B**, sem C7 direto até retornar. (`app/chamados/services/validacao.py:667–675`) |
| `veredito_lavrado.lavrar` — `app/chamados/veredito_lavrado.py:270–275` | Cache `veredito_lavrado/julgado_em/janela_expira_em`; save individual dispara **B**. C6 consulta o juiz do fato, não esse cache. (`app/chamados/juizes.py:1405–1406`) |
| `gravar_lavras_em_lote` — `app/chamados/veredito_lavrado.py:237–250` | **`bulk_update`** dos três campos do cache; nenhum C7/fechamento/signal nessa porta. (`app/chamados/veredito_lavrado.py:240–251`) |
| Migração `_preencher_chave` — `app/chamados/migrations/0051_decisao_pergunta.py:25–27` | Dois `QuerySet.update`: colaborador e instância de marco; nenhum C7 ou fechamento no corpo. (`app/chamados/migrations/0051_decisao_pergunta.py:22–27`) |

O outro signal da pergunta emite `EventoCelula` para os campos relevantes; **seu corpo não consulta C7 nem fecha disputa**. A lista inclui resposta, validação, materialização, via, dia e FKs. (`app/chamados/signals.py:250–257`, `app/chamados/models.py:3220–3222`)

**3. Escritores do fechamento da disputa**

O **único escritor físico de fechamento encontrado é `DisputaSupervisao.fechar`**: atribui `fechada_em`, autoria, resolução e motivo, salvando os quatro juntos. Não há coluna `status` de disputa sendo fechada por outra porta. (`app/chamados/models.py:1932–1936`, `app/chamados/models.py:1822–1824`)

Os chamadores encontrados dessa porta são:

| Origem | Sítios |
|---|---|
| Reconciliador | `app/chamados/reconciliador.py:412`, `:612`, `:1171` |
| Materialização | `app/chamados/services/materializacao.py:586`, `:1333` |
| Emissão/S143 | `app/chamados/services/disputa_emissao.py:1284`, `:1964` |
| Beco | `app/chamados/services/becos.py:57` |
| Competência trancada | `app/chamados/services/competencia_trancada.py:204` |
| Entrada sincronizada | `app/ponto/services/entrada_sincronizada.py:59` |
| Encerramento administrativo e retirada do último motivo | `app/chamados/models.py:1950`, `:2072` |
| Foto recebida, após validar pelo dono | `app/api/foto_recebida.py:79–80` |
| Desligamento, por exceção nomeada | `app/colaboradores/services/desligamento.py:139–142` |
| Comando de perguntas com fato | `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` |

`fechar()` **não chama literalmente `pode_fechar()`**: consulta respostas sem veredito e julga perguntas por C6, aplicando as exceções nomeadas. Exige usuário e toma as travas antes de persistir. (`app/chamados/models.py:1876–1912`)

Os outros escritores de `fechada_em` são **reabridores**, que gravam `None`: `reabrir_admin` e `reabrir_sistema`. (`app/chamados/models.py:2149–2161`, `app/chamados/models.py:2173–2177`)

**4. Cron/vigia**

Existe **vigia parcial**, `fechar_becos_disputa --alarme`, cadastrado para 07:10. Ele percorre abertas, mas usa o recorte de beco — pai terminal e nenhuma pergunta visível pelo filtro — antes de consultar C7. (`app/config/crons.py:663`, `app/chamados/management/commands/fechar_becos_disputa.py:32–43`, `app/chamados/services/becos.py:25–33`)

Com `--alarme`, **conta/lista/alerta, sem fechar**. Com `--apply`, o mesmo comando chama a porta de fechamento de beco. (`app/chamados/management/commands/fechar_becos_disputa.py:30`, `:54–70`)

Não encontrei varredor do universo completo **“disputa aberta com C7=True”**, sem aquele recorte. O comando antigo `reconciliar_disputa_zumbi` consta como removido; `disputa_vazia` conta fechadas pelo motivo `vazia`, e a auditoria de abertas sem motivo também não é esse contador. (`app/config/crons.py:840`, `app/chamados/services/vigia_fio.py:727–734`, `app/chamados/management/commands/auditar_disputas.py:166–177`)

**5. Todos os chamadores nominais de `pode_fechar()` encontrados fora de `tests/`**

| Chamador | Leitura e uso |
|---|---|
| `lavrar_sem_turno_aberto` | Fecha se aberta e C7. (`app/chamados/reconciliador.py:411–412`) |
| Retratação por célula | Fecha após o laço. (`app/chamados/reconciliador.py:611–612`) |
| Órfãs pela lâmpada | Fecha, após barrar resposta sem veredito. (`app/chamados/reconciliador.py:1163–1171`) |
| Ausência da resposta | Fecha. (`app/chamados/services/materializacao.py:585–586`) |
| Cauda da materialização | Fecha. (`app/chamados/services/materializacao.py:1316–1333`) |
| Emissão a partir de órfãos | Se houve abstenção por falta de ata, só acende aviso; caso contrário, fecha. (`app/chamados/services/disputa_emissao.py:1278–1284`) |
| S143 | Fecha, salvo pergunta em voo. (`app/chamados/services/disputa_emissao.py:1961–1964`) |
| `competencia_trancada.medir` | Seleciona; pai vivo fora do conjunto a encerrar impede inclusão. (`app/chamados/services/competencia_trancada.py:153–158`) |
| `competencia_trancada.encerrar` | Fecha sob `aplicar`, competência trancada e ações existentes. (`app/chamados/services/competencia_trancada.py:167–168`, `:202–205`) |
| `becos.classificar` | Só classifica, condicionado ao recorte. (`app/chamados/services/becos.py:25–33`) |
| `becos.fechar_beco` | Resolve perguntas dos chamados terminais, relê e fecha. (`app/chamados/services/becos.py:41–57`) |
| Entrada sincronizada | Fecha disputa do pai manual que agrupa somente aquele órfão. (`app/ponto/services/entrada_sincronizada.py:47–61`) |

Há **outro significado publicado com o nome `pode_fechar`**: o explicador calcula somente `not fio_vivo(ps)` e devolve essa chave, sem a trava adicional `respostas_sem_veredito()` do C7. Seus consumidores dizem “Tudo validado e gravado” e oferecem “Fechar/regularizar”. (`app/chamados/services/explicador.py:134–163`, `app/chamados/models.py:1853`, `app/chamados/services/desenlace.py:63–65`, `app/templates/chamados/partials/modal_fio.html:434–445`)

**6. Chamado-pai**

Quem reage ao fechamento da disputa é `disputa_fechada_fecha_chamado_pai`: busca o pai e chama **`ch.reconciliar()`**, salvo criação, ausência de pai, ausência de fechamento ou pai já em `ENCERRADOS`. O receiver engole falhas; também aceita que o gate recuse a reconciliação. (`app/chamados/signals.py:18–50`)

O fluxo normal leva o pai a **`resolvido`**, pelo motor e por `_gravar_estado`, que atribui e salva `status_local`. A reconciliação pode recusar por premissa ou pergunta viva. (`app/chamados/catalogo/motor.py:111`, `app/chamados/models.py:641–655`, `app/chamados/models.py:682–689`, `app/chamados/models.py:228`, `app/chamados/models.py:296`)

**A relação inversa não exige disputa fechada como condição u

[truncado em 20000 de 21254 caracteres]

## 10/11 16:22 · sessao a50c10442bff · - · - · origem=Ronald (tmux)

Censo complementar, fora de `models.py`/`reconciliador.py`, para consolidação:

**Regra comum aos `save()` abaixo:** o `post_save` de PerguntaDisputa agenda `fechar_beco_da_disputa` em `on_commit` (`app/chamados/signals.py:227-238`). Essa porta **só consulta C7 se** a disputa estiver aberta, não houver pergunta com `respondida_em=None` e destino não terminal, e o pai estiver em `TERMINAIS` (`app/chamados/services/becos.py:21-33`). Tem guarda de reentrada (`app/chamados/services/becos.py:70-78`). Portanto, “tem signal” não equivale a “sempre pergunta C7” (`app/chamados/services/becos.py:29-33`).

| Carimbador / campos | Depois do carimbo, fechamento no fluxo |
|---|---|
| `_carimbar_validacao`: `validada_em/por`, save (`app/chamados/services/validacao.py:134-141`) | Fluxo normal: `validar_pergunta` chama materialização (`app/chamados/services/validacao.py:435-469`), cujo laço termina na cauda (`app/chamados/services/materializacao.py:1375-1379`) que pergunta C7 e fecha (`app/chamados/services/materializacao.py:1316-1333`); também signal condicionado acima. |
| Aceite de folga usa o mesmo carimbo e `fechar_por_admin` (`app/chamados/services/validacao.py:288-296`, `:320-329`) | Chama `fechar_o_fio_do_dia` diretamente ou por `declarar_dia`; essa porta fecha perguntas por admin e reconcilia chamados (`app/ponto/services/veredito_celula.py:179-205`), acionando S143 quando status é encerrado (`app/chamados/signals.py:90-115`); também signal condicionado. |
| `devolver_validacao_recusada`: zera `validada_em/por` (`app/chamados/services/validacao.py:195-208`) | Nenhum C7 direto na função; pode reabrir a disputa (`app/chamados/services/validacao.py:209-230`). Seu chamador normal depois materializa a disputa (`app/chamados/services/validacao.py:464-469`); também signal condicionado. |
| `responder_pergunta_disputa_core`: `resposta_colab`, `respondida_em`, zera validação e descarimba carimbo obsoleto (`app/chamados/services/disputa_emissao.py:1467-1508`) | Nenhum C7 direto até retorno (`app/chamados/services/disputa_emissao.py:1513-1559`); signal condicionado. Se resposta vira ausência, chama `_materializar_ausencia_de_resposta` (`app/chamados/services/disputa_emissao.py:1538-1541`), que pergunta C7/fecha (`app/chamados/services/materializacao.py:585-586`). Chamador de correção administrativa também chama `validar_pergunta` após responder (`app/chamados/services/acoes_disputa.py:370-380`). |
| `responder_automaticamente`: `resposta_colab/respondida_em` (`app/chamados/services/disputa_emissao.py:1647-1658`) | Signal condicionado; chamador direto da foto depois invoca `validar_pergunta` (`app/api/foto_recebida.py:43-51`) e alcança cauda C7 da materialização. |
| `carimbar_resposta_pela_trilha`: `respondida_em` (`app/chamados/services/disputa_emissao.py:1670-1682`) | Nenhum C7 direto; signal condicionado. Chamador `curar_perguntas_em_limbo` só executa essa porta/reabertura e imprime resultado (`app/chamados/management/commands/curar_perguntas_em_limbo.py:64-70`). |
| `reabrir_para_correcao`: `respondida_em=None`, `validada_em/por=None`, `resposta_colab=''`, opcionalmente descarimba absorção (`app/chamados/services/disputa_emissao.py:1593`, `:1629-1634`) | **QuerySet.update**, ou `descarimbar_em_lote`: não passa pelo signal de `save`; não consulta C7, emite evento de célula explicitamente (`app/chamados/services/disputa_emissao.py:1607-1609`, `:1630-1634`). Chamadores de UI seguem com nota/push/log (`app/chamados/views.py:1435-1457`; `app/chamados/services/acoes_disputa.py:102-114`). |
| `recusar_pergunta`: grava `recusa_motivo`, depois usa reabertura acima (`app/chamados/services/validacao.py:584-594`) | O save do motivo agenda signal condicionado; no commit ele observa o estado após a reabertura. Nenhum C7 direto até retorno (`app/chamados/services/validacao.py:595-610`). |
| `_materializar_ausencia_de_resposta`: carimba irmãs, via declaração/ausência, `resposta='validar'` (`app/chamados/services/materializacao.py:549-566`) | C7 e `fechar` explícitos (`app/chamados/services/materializacao.py:585-586`), sem exigir pai terminal. Destino só reconcilia se ausência não está em `ABERTAS` (`app/chamados/services/materializacao.py:580-582`). |
| `_materializar_uma`: carimbo declaração/via do fato, resposta validada, lavra de veredito (`app/chamados/services/materializacao.py:1277-1282`); ramo paridade usa identidade (`app/chamados/services/materializacao.py:924`) | Chamadores: laço normal chama cauda C7 (`app/chamados/services/materializacao.py:1375-1379`); validação individual depois chama o laço (`app/chamados/services/validacao.py:463-469`); passivo chama unidade (`app/chamados/services/passivo_validado.py:516`) e cauda uma vez por disputa (`app/chamados/services/passivo_validado.py:401-406`). Também signal condicionado. |
| `_carimbar_recusa`: colisão → identidade `fato_ocupado`; demais → `recusa_motivo` (`app/chamados/services/materializacao.py:1089-1107`) | Nenhum C7 direto dentro da função; integra unidade/materialização acima; também signal condicionado. Passivo pode gravar recusa de geometria após rollback (`app/chamados/services/passivo_validado.py:543-546`). |
| `_materializar_resposta_hora`: `pendencia_admin/recusa_motivo` (`app/chamados/services/materializacao.py:845-851`) | Nenhum C7 direto nesse sítio; pertence à unidade/laço/cauda acima; signal condicionado. |
| `materializar_pendentes_aprovadas`: carimbo `juiz_aprovou` (`app/chamados/services/materializacao.py:1418-1433`) | Nenhum C7 direto na função. Chamador é signal de disputa **já fechada**, condicionado a `fechada_em` preenchido (`app/chamados/signals.py:147-165`); seus saves também agendam Beco. Guarda pergunta em voo e lastro (`app/chamados/services/materializacao.py:1425-1431`). |
| `_resolver_perguntas_por_chamado_resolvido`: carimbo por fato com `resposta='validar'` (`app/chamados/services/disputa_emissao.py:1863-1867`); ramo sem credor carimba com `resposta='responder'` (`:1935-1947`) | Cauda explícita C7/fechar (`app/chamados/services/disputa_emissao.py:1963-1964`). Não roda para encerramento por tranca (`:1818-1819`); não fecha disputa com pergunta em voo (`:1961-1962`). Ramo sem credor exige pai e destinos fora de `VIVOS` (`:1909-1915`). Também signal condicionado. |
| `relavrar`: troca `via_resolucao` e lavra veredito no mesmo save (`app/chamados/services/relavra_via.py:125-139`) | Nenhum C7 direto; signal condicionado. Chamador command só chama `relavrar` e imprime (`app/chamados/management/commands/relavrar_via_i6.py:100-104`). |
| `desfazer_carimbo_sem_lastro`: `p.descarimbar()` (`app/chamados/services/relavra_via.py:242-247`) | Nenhum C7 direto; se disputa fechada reabre (`app/chamados/services/relavra_via.py:259-263`); signal condicionado do descarimbo via save. |
| `retratar_duplicata`: identidade `duplicata`, conserva carimbo anterior, grava motivo (`app/chamados/services/fio.py:1357-1363`) | Nenhum C7 direto na função ou chamador imediato `retratar_para_canonica` (`app/chamados/services/fio.py:1336-1342`); signal condicionado. |
| Command `encerrar_perguntas_com_fato`: `encerrar_por_fato_no_espelho` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:65-76`) | Após carimbos, testa `fio_vivo`, então chama `d.fechar()`; sem chamada nominal a C7 no command (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:80-93`). |
| `conferir_data`: `data_conferida_em/por` (`app/chamados/services/validacao.py:664-666`) | Nenhum C7 direto até retorno (`app/chamados/services/validacao.py:667-674`); signal condicionado. |

Outros gatilhos de reversão: ausência rejeitada chama reabertura com `desfazer_absorcao=True` e reabre disputas (`app/chamados/services/materializacao.py:679-690`); reabrir órfãs usa `reabrir_fantasma` e reabre disputa/pai (`app/chamados/management/commands/reabrir_orfas_enterradas.py:157-172`); competência reaberta chama `reabrir_por_competencia` e reabre chamados (`app/chamados/services/competencia_trancada.py:251-265`).

`veredito_lavrado.lavrar` tem save individual (`app/chamados/veredito_lavrado.py:270-275`), enquanto `gravar_lavras_em_lote` usa `bulk_update` (`:237-250`): o segundo não tem signal/C7 dentro da porta (`:240-251`). O cron de stale chama `lavrar(p)` individualmente (`app/chamados/management/commands/lavrar_perguntas_stale.py:87-92`).

Verificação estática adicional: executei em memória o próprio censo AST, sem importar Django e com `python3 -B`; seus critérios de atribuição e limites estão documentados em `app/core/censo_escritas.py:6-51`. Ele reconhece os saves de PD em `models.py:3006,3152,3190,3212,3276`, update em `:3264`, e os sítios externos da tabela; não deve ser apresentado como prova sem limites porque seu balde não atribuível é explícito (`app/core/censo_escritas.py:43-51`).

Não encontrei escritor de PerguntaDisputa em `flip_auto.py`: as referências ao modelo nesse arquivo apenas consultam recusas de paridade (`app/ponto/services/flip_auto.py:125-140`).

## 10/11 16:22 · sessao 8ca827f1088e · - · - · origem=Ronald (tmux)

Censo de fechamento/leitores concluído sobre arquivos acessíveis, sem executar Django, banco ou logs. As contagens e situações de produção permanecem exclusivamente as fornecidas pelo usuário. [não verificado]

**Escritores físicos do estado da disputa**

| Ato | Escritor |
|---|---|
| Fechar | **Único sítio encontrado:** `DisputaSupervisao.fechar`: atribui `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` e salva os quatro campos — `app/chamados/models.py:1932-1936`. |
| Reabrir administrativamente | Zera os quatro campos; salva em `app/chamados/models.py:2149-2161`. |
| Reabrir pelo sistema | Zera os quatro campos; salva em `app/chamados/models.py:2173-2177`. |
| Definição do estado | A disputa não usa campo `status`: `aberta` deriva de `fechada_em is None` — `app/chamados/models.py:1822-1824`; timestamp declarado em `:1757`. |

A porta canônica exige usuário, recusa resposta sem veredito em qualquer modalidade e, sem exceção nomeada, recusa perguntas vivas; exceções são desligamento, admin encerra e competência trancada. Ela toma a ordem global de travas antes de gravar. `app/chamados/models.py:1876-1912`.

O antigo bulk update de desligamento **já saiu**: o código atual chama a porta `fechar(..., motivo=FECHOU_DESLIGAMENTO)` e pula disputa com respostas sem veredito. `app/colaboradores/services/desligamento.py:132-142`. O comentário de exceção no instrumento `bin/esmeril_chamado_censo.py:64-71` está desatualizado frente a esse corpo.

**Todos os 12 chamadores reais de `.pode_fechar()` fora de `tests/` encontrados por AST**

| Função | Leitura / consequência |
|---|---|
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:411` → `fechar` em `:412`. **Há chamada explícita nesta cópia**; distinguir os retornos anteriores, com o agente específico. |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:611` → `fechar` em `:612`. |
| `encerrar_orfas_pela_lampada` | `app/chamados/reconciliador.py:1170` → `fechar` em `:1171`; respostas sem veredito são barradas previamente em `:1163-1169`. |
| `_materializar_ausencia_de_resposta` | `app/chamados/services/materializacao.py:585` → `fechar` em `:586`. |
| `_fechar_disputa_se_quitada` | `app/chamados/services/materializacao.py:1316` → `fechar` em `:1333`. |
| `criar_disputa_a_partir_de_chamados_orfaos` | `app/chamados/services/disputa_emissao.py:1278`; se `_absteve_por_ata`, apenas acende aviso `:1279-1281`; caso contrário fecha em `:1284`. |
| `_resolver_perguntas_por_chamado_resolvido` | `app/chamados/services/disputa_emissao.py:1963` → `fechar` em `:1964`; pergunta em voo impede a leitura/fecho em `:1961-1962`. |
| `competencia_trancada.medir` | `app/chamados/services/competencia_trancada.py:153`: apenas seleciona; pai vivo fora do conjunto a encerrar impede inclusão em `:155-158`. |
| `competencia_trancada.encerrar` | `app/chamados/services/competencia_trancada.py:202` → porta de exceção em `:204-205`; depende de `aplicar`, competência trancada e ações em `:167-168`. |
| `becos.classificar` | `app/chamados/services/becos.py:33`: apenas classifica; **só chega ao C7 se pai terminal e nenhuma pergunta “visível”** em `:27-32`. |
| `becos.fechar_beco` | `app/chamados/services/becos.py:56` → `fechar` em `:57`; toma travas, resolve perguntas dos chamados terminais e relê disputa em `:41-55`. |
| `resolver_cobranca_da_entrada` | `app/ponto/services/entrada_sincronizada.py:58` → `fechar` em `:59`; recorte é pai manual com somente o órfão atual em `:47-56`. |

`fechar()` não chama literalmente `.pode_fechar()`: aplica a mesma composição por `respostas_sem_veredito()` e perguntas julgadas por `pergunta_viva`, com as exceções nomeadas. `app/chamados/models.py:1886-1903`. O método C7 reúne resposta sem veredito e fio vivo. `app/chamados/models.py:1848-1853`.

**Leitor indireto que deve constar na tabela de TODOS os carimbadores por `save()`**

Cada `PerguntaDisputa.save()` alcança `_becos_post_save_pergunta`, que agenda `on_commit` → `fechar_beco_da_disputa`. `app/chamados/signals.py:227-238`. Essa entrada relê a disputa e exige classificação `FECHAVEL`. `app/chamados/services/becos.py:66-78`.

Logo, para carimbador por `save()`, escrever “nenhum” absoluto seria incorreto: existe leitor indireto **condicional**. O classificador sai antes do C7 se houver qualquer pergunta com `respondida_em is None` e destino não terminal, ou se o pai não for terminal; não considera `materializada_em`/`validada_em` nesse pré-filtro. `app/chamados/services/becos.py:25-33`. O próprio comentário declara que `update()` direto nasce sem signal. `app/chamados/management/commands/fechar_becos_disputa.py:9-11`.

Há também post-save síncrono emitindo evento da célula quando os campos acordadores mudam, mas seu corpo não chama C7 nem fecha a disputa: chama `PerguntaDisputa.acordar_celulas(...)`. `app/chamados/signals.py:250-257`.

**Cron/vigia**

Existe o vigia parcial de becos: a configuração agenda `fechar_becos_disputa --alarme` às 07:10. `app/config/crons.py:663`. Ele varre disputas abertas, porém classifica pelo recorte de beco acima. `app/chamados/management/commands/fechar_becos_disputa.py:32-43`; `app/chamados/services/becos.py:25-33`.

Com `--alarme`, apenas lista/conta e termina com erro se achar candidato; **não fecha**. `app/chamados/management/commands/fechar_becos_disputa.py:30`, `:54-70`. O comando preserva `--apply` e aí chama `becos.fechar_beco`. `:23`, `:57-64`.

**Não encontrei varredor de todo o universo “aberta + C7 True”, sem o recorte de beco.** A lista exaustiva de chamadas acima contém somente o classificador parcial como leitura usada pelo cron. O antigo comando `reconciliar_disputa_zumbi` está registrado como removido. `app/config/crons.py:840`. O contador `disputa_vazia` mede **fechadas com motivo vazia**, não abertas fecháveis. `app/chamados/services/vigia_fio.py:727-734`. `auditar_disputas` conta abertas sem motivos, não C7. `app/chamados/management/commands/auditar_disputas.py:166-177`.

**Outro significado chamado “pode_fechar”**

O explicador **não chama o método C7**: calcula somente `not fio_vivo(ps)` e devolve chave `pode_fechar`; não acrescenta a lista `respostas_sem_veredito()` do C7. `app/chamados/services/explicador.py:134-140`, `:163`; comparar `app/chamados/models.py:1853`.

Essa chave alimenta a afirmação “Tudo validado e gravado” / “Fechar/regularizar” em `app/chamados/services/desenlace.py:63-65` e os textos/cores do modal em `app/templates/chamados/partials/modal_fio.html:434`, `:445`. O fio monta esse explicador em `app/chamados/services/fio.py:304-312`.

A guarda S127 também usa outro recorte: disputa aberta sem nenhuma pergunta, ou `fio_vivo(perguntas)`; não chama C7. `app/chamados/juizes.py:2308-2322`.

**Todos os chamadores de fechamento encontrados (delegam ao escritor único)**

- `app/chamados/models.py:1950` (`admin_encerra`), `:2072` (`remover_motivo`, apenas se não sobra motivo, `:2066`).
- `app/chamados/reconciliador.py:412`, `:612`, `:1171`.
- `app/chamados/services/materializacao.py:586`, `:1333`.
- `app/chamados/services/disputa_emissao.py:1284`, `:1964`.
- `app/chamados/services/becos.py:57`.
- `app/chamados/services/competencia_trancada.py:204`.
- `app/ponto/services/entrada_sincronizada.py:59`.
- `app/api/foto_recebida.py:80`: valida pelo dono antes; fecha diretamente quando não restam motivos além de foto (`:70-80`), sem consulta explícita ao C7.
- `app/colaboradores/services/desligamento.py:142`.
- `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`: sob `--apply` (`:53-55`), consulta `fio_vivo` em `:83`, chama porta e contabiliza recusa `ValueError` em `:92-93`.

**Chamado-pai**

Quando a disputa salva fechada, `disputa_fechada_fecha_chamado_pai` procura o pai e chama `ch.reconciliar()`, em atomic; não atua se criado, sem pai, sem timestamp ou pai já resolvido/fechado; falhas são engolidas com registro. `app/chamados/signals.py:18-50`.

`reconciliar()` resolve pelo motor e `_gravar_estado`; o destino normal é `resolvido`, não `fechado`. `app/chamados/models.py:641-655`; `app/chamados/catalogo/motor.py:111`. Pode recusar por premissa ou pergunta viva. `app/chamados/models.py:682-689`.

O escritor físico do pai é `_gravar_estado`: atribui `status_local` em `app/chamados/models.py:228`, salva em `:296`. O fechamento da disputa **não garante** fechamento do pai, por essas recusas e pelo `except` do receiver. `app/chamados/signals.py:42-50`.

O inverso também não é uma trava universal: o humano bloqueia encerramento usando S127 (`fio_vivo`), sem exigir que `fechada_em` esteja preenchida. `app/chamados/models.py:148-171`; `app/chamados/juizes.py:2316-2322`. Resolver o pai dispara a resolução de perguntas em best effort; falha desse trabalho não desfaz o estado do pai. `app/chamados/signals.py:90-117`. O código tem tratamento explícito da combinação disputa aberta/pai terminal no serviço de becos. `app/chamados/services/becos.py:25-33`. O anexo fornecido afirma 7 pais resolvidos e 4 fechados entre as 18, sem confirmação independente. [não verificado]

Não encontrei arquivo inacessível entre os caminhos rastreados; não li banco nem logs. Execução efetiva dos crons, callbacks e código em produção não é verificável nesta cópia. [não verificado]

## 10/11 16:21 · sessao afd64bbd10f1 · - · - · origem=Ronald (tmux)

Achado principal: o dado do anexo e esta cópia divergem. `lavrar_sem_turno_aberto` carimba em `app/chamados/reconciliador.py:405` e imediatamente consulta `p.disputa.pode_fechar()` em `app/chamados/reconciliador.py:411`, chamando `fechar()` em `app/chamados/reconciliador.py:412`. Portanto, “não se achou chamada depois do carimbo” não descreve esse corpo da cópia. A execução efetiva em produção, possíveis exceções e o código carregado pelo processo são [não verificado].

**Mecanismo transversal para todas as linhas com `save`:** `PerguntaDisputa.save` apenas envolve a persistência em `atomic` (`app/chamados/models.py:3413`, `app/chamados/models.py:3422`). O `post_save` `_becos_post_save_pergunta` agenda `on_commit -> fechar_beco_da_disputa` (`app/chamados/signals.py:227`, `app/chamados/signals.py:233`, `app/chamados/signals.py:238`). Essa porta só chega a C7 se disputa aberta, nenhuma pergunta “visível” segundo seu filtro e pai em `TERMINAIS` (`app/chamados/services/becos.py:21`, `app/chamados/services/becos.py:27`, `app/chamados/services/becos.py:29`, `app/chamados/services/becos.py:31`, `app/chamados/services/becos.py:33`), além de guarda de reentrada (`app/chamados/services/becos.py:70`). A porta relê e fecha sob C7 em `app/chamados/services/becos.py:56`. Logo, uma linha sem chamada direta não equivale a “nenhum fechamento”: os saves têm essa via condicional.

| Carimbador no escopo models/reconciliador | Escrita | Leitura/fechamento posterior |
|---|---|---|
| `lavrar_sem_turno_aberto` | Chama `carimbar(VIA_SEM_TURNO_ABERTO)` (`app/chamados/reconciliador.py:405`); save real em `app/chamados/models.py:3152` | **Sim, direto:** C7 `app/chamados/reconciliador.py:411`, `fechar` `app/chamados/reconciliador.py:412`; só exige disputa aberta e C7. Também signal Beco acima. |
| `retratar_perguntas_absolvidas_por_celula` | `carimbar(VIA_CELULA_ABSOLVEU)` (`app/chamados/reconciliador.py:595`), save `app/chamados/models.py:3152` | **Sim, após laço:** `refresh` + aberta + C7 em `app/chamados/reconciliador.py:610` e `app/chamados/reconciliador.py:611`; `fechar` em `app/chamados/reconciliador.py:612`. Guardas de tranca/vitalidade já na seleção `app/chamados/reconciliador.py:650`, `app/chamados/reconciliador.py:661`, `app/chamados/reconciliador.py:663`. Também signal Beco. |
| `fantasma_pela_lampada` / `encerrar_fantasma` | `p.encerrar_fantasma` (`app/chamados/reconciliador.py:1147`) -> `carimbar(VIA_FANTASMA)` (`app/chamados/models.py:3287`) -> save `app/chamados/models.py:3152` | **Sim, após laço:** pula fechada `app/chamados/reconciliador.py:1161`; barra resposta sem veredito `app/chamados/reconciliador.py:1163`; C7 `app/chamados/reconciliador.py:1170`; fecha `app/chamados/reconciliador.py:1171`. Também signal Beco. |
| `DisputaSupervisao.fechar`, portas desligamento/admin | `q.fechar_por_admin` (`app/chamados/models.py:1921`) -> `carimbar_identidade(VIA_ADMIN_DECIDIU)` (`app/chamados/models.py:3064`), save `app/chamados/models.py:3190` | **Sim, já dentro da porta de fechamento:** grava `fechada_em` em `app/chamados/models.py:1932`, save `app/chamados/models.py:1936`. Não chama nominalmente C7: valida resposta sem veredito `app/chamados/models.py:1886`, pergunta viva `app/chamados/models.py:1896`; exceções permitem viva `app/chamados/models.py:1897`, `app/chamados/models.py:1918`. |
| `DisputaSupervisao.remover_motivo` / retratação | `carimbar_identidade(VIA_MOTIVO_RETRATADO, manter_carimbo=True)` (`app/chamados/models.py:2061`), save `app/chamados/models.py:3190` | **Condicional direto:** se restarem motivos retorna sem fechar (`app/chamados/models.py:2066`); se nenhum, chama `self.fechar` (`app/chamados/models.py:2072`). O save de cada pergunta também dispara Beco condicional. |
| `ChamadoColaborador.ressuscitar_perguntas` | `descarimbar_em_lote(...validada_em=None, validada_por=None, respondida_em=None)` (`app/chamados/models.py:1566`); `QuerySet.update` real `app/chamados/models.py:3264` | **Nenhum C7/fechar nesse corpo:** o caminho lido retorna perguntas à fila e reabre disputa fechada (`app/chamados/models.py:1574`, `app/chamados/models.py:1575`). Por `update`, não passa `post_save`; chama explicitamente `acordar_celulas` em `app/chamados/models.py:1568`. |
| Porta `carimbar` (infraestrutura, não gatilho específico) | `_escrever_carimbo` atribui materializada/via (`app/chamados/models.py:3099`); opcionalmente `respondida_em` (`app/chamados/models.py:3132`) e `validada_em` (`app/chamados/models.py:3135`); save `app/chamados/models.py:3152` | **Nenhum direto no corpo** `app/chamados/models.py:3103`–`app/chamados/models.py:3157`; consulta C6 antes de persistir (`app/chamados/models.py:3139`). Tem signal Beco. Fechamento direto depende do chamador (tabela de gatilhos). |
| Porta `carimbar_identidade` | Atualiza carimbo e via (`app/chamados/models.py:3182`, `app/chamados/models.py:3185`); save `app/chamados/models.py:3190` | **Nenhum direto no corpo** `app/chamados/models.py:3168`–`app/chamados/models.py:3191`; tem signal Beco. Exemplos de chamadores acima e nos outros arquivos do censo. |
| Porta `descarimbar` | Carimbo nulo (`app/chamados/models.py:3208`); save opcional `app/chamados/models.py:3212` | **Nenhum direto no corpo**, `app/chamados/models.py:3203`–`app/chamados/models.py:3213`; se salvar, tem signal Beco. `salvar=False` exige analisar save no chamador. |
| `encerrar_por_fato_no_espelho` | Apenas via `fato_materializado` (`app/chamados/models.py:3275`), save `app/chamados/models.py:3276` | **Sim pelo chamador comando**, mediante `--apply` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:53`), que chama a porta em `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`, filtra `fio_vivo` em `app/chamados/management/commands/encerrar_perguntas_com_fato.py:83` e chama `d.fechar` em `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`. Também signal Beco. |
| `reabrir_fantasma` | `self.descarimbar()` (`app/chamados/models.py:3298`) | **Nenhum fecho direto:** chamador comando reabre disputa em `app/chamados/management/commands/reabrir_orfas_enterradas.py:165` e pai em `app/chamados/management/commands/reabrir_orfas_enterradas.py:172`; signal Beco permanece aplicável ao save da pergunta. |
| `reabrir_por_competencia` | `self.descarimbar()` (`app/chamados/models.py:3322`) | **Nenhum fecho direto:** `competencia_trancada.reabrir` chama em `app/chamados/services/competencia_trancada.py:254` e reabre chamados em `app/chamados/services/competencia_trancada.py:265`; signal Beco no save da pergunta. |
| `reabrir_por_resposta` | Só muda memória, retornando campos de `descarimbar(salvar=False)` (`app/chamados/models.py:3315`) | Persistência/fluxo pertencem aos escritores de resposta, sob censo do outro agente; por si não é escritor persistente (`app/chamados/models.py:3311`, `app/chamados/models.py:3315`). |
| `_nascer`, reuso | Pode trocar `celula` (`app/chamados/models.py:2965`, `app/chamados/models.py:2966`) e `chamado_destino` (`app/chamados/models.py:2982`); save em `app/chamados/models.py:3006` | **Nenhum C7/fechar direto no trecho lido:** após save acorda célula antiga e retorna (`app/chamados/models.py:3007`, `app/chamados/models.py:3008`). Tem signal Beco. |
| `_nascer`, inserção | Instância recebe `disputa`, motivo, destino, célula, conversa, `**campos` (`app/chamados/models.py:3014`); `gravar_dia_do_fato` (`app/chamados/models.py:3016`); insert (`app/chamados/models.py:3017`) | Nenhum C7/fechar direto antes de retornar (`app/chamados/models.py:3018`, `app/chamados/models.py:3031`); `post_save` Beco também roda para `created` pois o receiver não o exclui (`app/chamados/signals.py:228`, `app/chamados/signals.py:229`). |

C6/C7 têm estes significados na cópia:

- C7 retorna **ausência de resposta sem veredito e ausência de pergunta viva**; não exige que a disputa esteja aberta nem lê status do chamado-pai (`app/chamados/models.py:1848`, `app/chamados/models.py:1853`). A condição “aberta” é dos chamadores, e `aberta` deriva exclusivamente de `fechada_em is None` (`app/chamados/models.py:1822`, `app/chamados/models.py:1824`).
- `respostas_sem_veredito` inclui pergunta com validada/materializada nulas e timestamp **ou** texto; exclui dia trancado (`app/chamados/models.py:1842`, `app/chamados/models.py:1846`). Assim C6 dar uma pergunta por morta não basta necessariamente para C7: essa lista é uma trava adicional (`app/chamados/models.py:1853`).
- C6 lê validação, tranca, vias terminais, resposta, decisão da célula, marco, data/janela e lastro, nessa sequência (`app/chamados/juizes.py:1324`, `app/chamados/juizes.py:1329`, `app/chamados/juizes.py:1336`, `app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1362`, `app/chamados/juizes.py:1368`, `app/chamados/juizes.py:1388`, `app/chamados/juizes.py:1406`).
- `estado` da pergunta é propriedade de leitura, não coluna escrita (`app/chamados/models.py:3033`, `app/chamados/models.py:3041`).
- Há campos equivalentes relevantes além dos carimbos: data, tipo de resposta, FK célula, FK destino e conversa influenciam a leitura do juiz (`app/chamados/juizes.py:828`, `app/chamados/juizes.py:832`, `app/chamados/juizes.py:835`, `app/chamados/juizes.py:334`, `app/chamados/juizes.py:647`, `app/chamados/juizes.py:688`).

Signals e fluxo adicional:

- `_outbox_post_save_pergunta` emite evento de célula para saves que toquem campos relevantes; não fecha diretamente (`app/chamados/signals.py:250`, `app/chamados/signals.py:255`, `app/chamados/signals.py:257`). A lista está em `app/chamados/models.py:3220`.
- A porta de fechamento grava `fechada_em` e dispara signal para o pai (`app/chamados/models.py:1932`, `app/chamados/models.py:1936`, `app/chamados/signals.py:18`). O pai é reconciliado em `app/chamados/signals.py:42`, se existir e ainda não estiver em `ENCERRADOS` (`app/chamados/signals.py:28`, `app/chamados/signals.py:33`); falhas são engolidas em `app/chamados/signals.py:46`.
- O signal S143, quando chamado termina, tenta resolver perguntas pelo serviço compartilhado (`app/chamados/signals.py:90`, `app/chamados/signals.py:115`) e engole falhas (`app/chamados/signals.py:116`), logo o sinal não garante que todas as perguntas/disputa fechem antes de permitir chamado terminal (`app/chamados/signals.py:111`, `app/chamados/signals.py:117`).
- Migração 0051 grava apenas chave (`colaborador_id`, `instancia_marco`), sem campos de resposta/carimbo (`app/chamados/migrations/0051_decisao_pergunta.py:25`, `app/chamados/migrations/0051_decisao_pergunta.py:27`); a 0053 só conta colisões/recusa, sem carimbar (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:36`, `app/chamados/migrations/0053_pergunta_unica_por_familia.py:45`).

Não li banco/log nem executei as funções de domínio; execução histórica e estado em produção permanecem [não verificado]. [nao verificado]

## 10/11 16:06 · sessao 9e89a5dceeb9 · BO-0611-05 · construtor · origem=Code

Alteradas somente as duas entradas em `core/tests/test_medida_turno_do_par.py:256-257`:

```diff
- 'derivacoes_de_dia': {('api/views.py::api_bater_ponto', 'B1'): [746],
-                       ('api/views_core.py::api_bater_ponto', 'B1'): [746],
+ 'derivacoes_de_dia': {('api/views.py::api_bater_ponto', 'B1'): [710],
+                       ('api/views_core.py::api_bater_ponto', 'B1'): [705],
```

Texto idêntico, incluindo indentação, nas quatro posições: base `da829e9f6:app/api/views.py:746`, base `da829e9f6:app/api/views_core.py:746`, WT `api/views.py:710` e WT `api/views_core.py:705`:

```python
                          data_turno=_tzc9.localdate(batida.timestamp).isoformat())
```

Resultados observados:

- **RED antes da edição:** 1 teste, 1 falha, rc **1**, com “linhas mudaram: registrar no MESMO commit” (`core/tests/test_medida_turno_do_par.py:539`).
- **Unittest do docstring após edição:** 35 testes, **OK**, rc **0** (`core/tests/test_medida_turno_do_par.py:20`).
- **MUTANTE=linhas:** 1 teste, 3 falhas de asserção, rc **1**; mutante mordido (`core/tests/test_medida_turno_do_par.py:585`, `:592`).

Comandos, em `app/`, usando este executor do heredoc original (`core/tests/test_medida_turno_do_par.py:20`):

```bash
rodar() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B - "$@" <<'PY'
import pathlib, textwrap
p = pathlib.Path('core/tests/test_medida_turno_do_par.py')
code = p.read_text().split("  PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'\n", 1)[1].split('\n  PY\n', 1)[0]
exec(compile(textwrap.dedent(code), str(p) + ':docstring', 'exec'))
PY
}

# RED: executado antes da edição
rodar CatracaTurnoDoParTest.test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit

# Após a edição
rodar

# Mutante em memória
MUTANTE=linhas rodar CatracaTurnoDoParTest.test_MORDE_linhas_a1_b1_publicam_ocorrencia_e_nao_def
```

## 10/11 15:37 · sessao b14693f6a721 · BO-0611-05 · construtor · origem=Code

Alterado somente [app/ponto/tests/test_l030_forma_js.py](/home/ronald/wt-codex-bo-0611-05-153526/app/ponto/tests/test_l030_forma_js.py:58): guarda de `hxConfirmar` antes do POST e de cada fila. O `return false` da selfie continua permitido (app/templates/ponto/bater_ponto_v2.html:287).

Censo dos tokens:

| Token | Quem emite/define | Quem lê |
|---|---|---|
| `garantirRequisitos` | v2 define em app/templates/ponto/bater_ponto_v2.html:285 | v2 chama em :312; oráculo localiza em app/ponto/tests/test_l030_forma_js.py:33 |
| `fetch` | v2 emite POST em app/templates/ponto/bater_ponto_v2.html:322 | oráculo em app/ponto/tests/test_l030_forma_js.py:58 |
| `registrarBatidaOffline` | app/static/pwa.js:107 define; :310 exporta | v2 chama em app/templates/ponto/bater_ponto_v2.html:333 e :346; triagem em app/templates/ponto/triagem/apto.html:282; oráculo em app/ponto/tests/test_l030_forma_js.py:66 |
| `hxConfirmar` | app/templates/core/_dialogo.html:172 define | guardas em app/ponto/tests/test_l030_forma_js.py:55, :58 e :66 |

Nenhum dos dois templates atuais emite `hxConfirmar` nos intervalos protegidos; o controle das duas fontes passou (app/ponto/tests/test_l030_forma_js.py:72).

Comandos executados, todos a partir de `app/`:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v ponto.tests.test_l030_forma_js
```

Antes da guarda: **rc 1**, oito mutantes escaparam. Depois: **rc 0**, dois testes passaram. Os novos casos cobrem começo de `garantirRequisitos`, POST e duas filas, com/sem comentário; cada caso exige controle verde, âncora única e exceção contendo `confirmacao` (app/ponto/tests/test_l030_forma_js.py:95).

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v ponto.tests.test_l030_templates_renderizados
```

**rc 1**, import interrompido por `ModuleNotFoundError: django`. Renderização das quatro combinações: **[nao verificado]**. A regra nova é alcançada pelo import do mesmo oráculo e sua chamada nesse teste (app/ponto/tests/test_l030_templates_renderizados.py:10, :37).

Diff:

```diff
@@ -55,12 +55,15 @@
         teste.assertNotIn('hxConfirmar', prefixo, 'confirmacao de GPS condiciona a fila') [nao verificado]
     else:
         teste.assertIn('fetch', tokens, 'POST ausente no JS emitido') [nao verificado]
+        teste.assertNotIn('hxConfirmar', tokens[:tokens.index('fetch')], [nao verificado]
+                          'confirmacao antes do POST')
         teste.assertEqual(tokens.count('registrarBatidaOffline'), 2, 'faltou fila offline ou de erro de rede') [nao verificado]
         # Antes da primeira fila (fetch falhou) e da segunda (offline),
         # nenhum retorno pode impedir o enfileiramento.
         for indice, nome in enumerate(tokens):
             if nome != 'registrarBatidaOffline':
                 continue
+            teste.assertNotIn('hxConfirmar', tokens[:indice], 'confirmacao antes da fila') [nao verificado]
             abertura = max(i for i in range(indice) if tokens[i] == 'try')
             teste.assertNotIn('return', tokens[abertura:indice], 'retorno antes da fila') [nao verificado]
@@ -88,3 +91,22 @@
                 self.assertIn(alvo, original, 'mutante nao foi aplicado') [nao verificado]
                 with self.assertRaisesRegex(AssertionError, 'GPS interrompe|confirmacao interrompe'):
                     afirmar_sem_barreira(self, original.replace(alvo, mutacao, 1), triagem)
+
+        original = (TPL / 'ponto/bater_ponto_v2.html').read_text()
+        for comentario in ('', ' /* naoBloquear */'):
+            portao = "if (!await hxConfirmar('GPS ausente', 'Continuar')) {return false;}" + comentario
+            chamada = "await hxConfirmar('GPS ausente', 'Continuar');" + comentario
+            for alvo, mutacao in (
+                ('async function garantirRequisitos(){',
+                 'async function garantirRequisitos(){\n    ' + portao),
+                ("var r=await fetch('/api/ponto/bater/',", chamada + "\n        var r=await fetch('/api/ponto/bater/',"),
+                ('await window.JulianiPWA.registrarBatidaOffline(tipo, lat, lng,',
+                 chamada + '\n          await window.JulianiPWA.registrarBatidaOffline(tipo, lat, lng,'),
+                ('await window.JulianiPWA.registrarBatidaOffline(tipo, gpsLat, gpsLng,',
+                 chamada + '\n        await window.JulianiPWA.registrarBatidaOffline(tipo, gpsLat, gpsLng,'),
+            ):
+                with self.subTest(alvo=alvo, comentario=comentario):
+                    afirmar_sem_barreira(self, original)
+                    self.assertEqual(original.count(alvo), 1, 'mutante exige ancora unica')
+                    with self.assertRaisesRegex(AssertionError, 'confirmacao'):
+                        afirmar_sem_barreira(self, original.replace(alvo, mutacao, 1))
```

## 10/11 15:34 · sessao a91f3b33f6ad · BO-0611-05 · verificador · origem=Code

1. **As recusas explícitas por GPS e situação foram removidas; “toda barreira”, literalmente, não.** Os dois kernels recebem coordenadas ausentes e seguem para `registrar_batida`; os templates seguem para POST/fila sem confirmação de GPS. (`app/api/views_core.py:393`, `app/api/views_core.py:403`, `app/api/views_core.py:640`; `app/api/views.py:406`, `app/api/views.py:648`; `app/templates/ponto/bater_ponto_v2.html:292`, `app/templates/ponto/bater_ponto_v2.html:333`, `app/templates/ponto/bater_ponto_v2.html:346`; `app/templates/ponto/triagem/apto.html:275`)

   Permanecem caminhos que também alcançam uma tentativa sem GPS:

   - Dedup retorna antes da porta; colisão oposta pode chegar à porta e não materializar outra batida. (`app/api/views_core.py:514`; `app/api/views.py:527`; `app/ponto/registro_batida.py:145`)
   - Triagem online, recebendo `E` com turno aberto e sem `confirmar`, retorna uma confirmação antes do registro. Ela oferece cancelar. (`app/ponto/views.py:316`, `app/ponto/views.py:330`; `app/templates/ponto/triagem/turno_confirmar.html:15`, `app/templates/ponto/triagem/turno_confirmar.html:19`)
   - Selfie ausente ainda interrompe a v2; câmera indisponível esconde o botão de confirmação offline da triagem. (`app/templates/ponto/bater_ponto_v2.html:287`, `app/templates/ponto/bater_ponto_v2.html:312`; `app/templates/ponto/triagem/apto.html:253`)

   A fila PWA aceita nulos, omite esses campos no envio e não descarta por 403; descarta em HTTP OK ou 400. Não encontrei descarte condicionado à ausência de GPS nesse caminho. (`app/static/pwa.js:107`, `app/static/pwa.js:145`, `app/static/pwa.js:158`, `app/static/pwa.js:166`)

2. **Sim, no caminho que cria uma batida nova, sem coordenadas grava `None/None` pela porta.** Os kernels passam os valores recebidos; a porta transforma `''`, `None` e valores não numéricos em `None` antes do `create`. Não há fallback para zero ou coordenada do posto nesse trajeto. (`app/api/views_core.py:403`, `app/api/views_core.py:646`; `app/api/views.py:654`; `app/ponto/registro_batida.py:5`, `app/ponto/registro_batida.py:105`, `app/ponto/registro_batida.py:157`)

   Coordenada parcial permanece parcial; zero recebido permanece zero. Os testes distinguem expressamente esses contratos. (`app/api/tests/test_l030_batida_pela_porta.py:55`, `app/api/tests/test_l030_batida_pela_porta.py:60`, `app/api/tests/test_l030_batida_pela_porta.py:65`)

   Não identifiquei escritor novo fora da porta nos caminhos alterados; a triagem web também usa `registrar_batida`. Isso não significa que toda tentativa produza uma linha nova: dedup devolve existente e colisão pode devolver `None`. (`app/ponto/services/triagem_batida.py:700`; `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`, `app/ponto/registro_batida.py:150`)

3. **Não identifiquei regra nova de raio, precisão ou classificação de geofence nos sítios alterados.** A mudança do detector exige o par completo e continua alimentando sua classificação existente; o emissor continua chamando `classificar_posicao`, autoridade registrada. (`app/ponto/services/vinculo_na_lista.py:49`, `app/ponto/services/vinculo_na_lista.py:87`; `app/ponto/services/geofence.py:270`; `app/core/juizes.py:1191`)

   As barreiras adicionais citadas no item 1 não foram introduzidas pelo diff fornecido. Alteração de `motor_calculo_v2.py` não aparece nesse diff; comparação independente com a base/hash, sem metadados Git nesta cópia: **[nao verificado]**.

4. **Todos os leitores declarados passaram a rejeitar coordenada parcial:**

   | Leitor | Guarda do par |
   |---|---|
   | Detector | `app/ponto/services/vinculo_na_lista.py:49` |
   | Vínculo, três consultas | `app/colaboradores/views.py:337`, `app/colaboradores/views.py:356`, `app/colaboradores/views.py:395` |
   | Contador offline | `app/ponto/views.py:485` |
   | Feed | `app/templates/core/_d2_feed.html:16` |
   | Tabela offline | `app/templates/ponto/batidas_offline_pendentes.html:96` |
   | Relatório | `app/templates/chamados/partials/_relatorio_geo.html:30` |
   | Modal | `app/templates/chamados/partials/modal_fio.html:480` |

   Não encontrei outro leitor decisório de **Batida** aceitando latitude presente com longitude ausente: listagem, detalhe, drawer, dashboard, fio e pino já exigem ambos. (`app/colaboradores/views.py:53`; `app/colaboradores/services/detalhe.py:22`; `app/colaboradores/services/drawer.py:63`; `app/templates/core/dashboard2.html:242`; `app/chamados/views.py:692`; `app/colaboradores/views.py:2474`)

   Há transmissores que preservam componentes separadamente, sem decidir “tem geo”: painel operacional e endpoint de detalhe. Há também testes somente de latitude no JS de captura/mapa: `desenharMapa`, aceitação da tentativa de fix e reaproveitamento do GPS na triagem. Portanto, a afirmação ampla “todo leitor de latitude exige o par” seria excessiva. (`app/core/services/painel_op.py:227`; `app/core/views.py:1024`; `app/templates/ponto/bater_ponto_v2.html:204`, `app/templates/ponto/bater_ponto_v2.html:294`; `app/templates/ponto/triagem/apto.html:217`)

   A equivalência com a autoridade vale para **parcial com `None`**. Ela não é total: geofence usa truthiness, enquanto os leitores alterados usam `is not None`; zero permanece na exclusão declarada. (`app/ponto/services/geofence.py:247`; `app/templates/core/_d2_feed.html:16`)

5. **As alterações de comportamento nos testes são devidas ao aval, com uma ressalva no selo modal:** [nao verificado]

   | Teste alterado | Avaliação | [nao verificado]
   |---|---|
   | `test_gps_obrigatorio` | Inversão devida: exige 200, persistência com nulos e ausência do antigo log de recusa. (`app/api/tests/test_gps_obrigatorio.py:19`, `:24`, `:34`) |
   | `test_api_bater_ponto` | Inversão devida para desligado/férias; exige 200 e uma batida. (`app/api/tests/test_api_bater_ponto.py:62`, `:84`) |
   | `test_afastado_na_batida` | Inversão devida para férias; reforça 200 para afastado e mantém cobrança. (`app/ponto/tests/test_afastado_na_batida.py:109`, `:117`) |
   | `test_afastado_nunca_bloqueia` | Troca a exigência de bloqueio por inspeção estrutural dos dois kernels; cobertura funcional complementar está no POST pela porta. (`app/ponto/tests/test_afastado_nunca_bloqueia.py:30`; `app/api/tests/test_l030_batida_pela_porta.py:49`) |
   | `test_batida_sideeffects` | Inversão com reforço: exige batida nula, alerta `sem_gps` e chamado. (`app/ponto/tests/test_batida_sideeffects.py:125`) |
   | `test_invariantes_disputa` | Mais estrito: remove a exceção que aceitava 403 por GPS. (`app/chamados/tests/test_invariantes_disputa.py:291`) |

   **O selo modal condicionado não garante que o fluxo continue livre.** A condição acomoda a retirada das chamadas, mas só procura os textos `hxConfirmar(` e `naoBloquear`, sem conferir argumentos ou fluxo. (`app/core/tests/test_selo_modal_nao_nasce_vazio.py:138`, `app/core/tests/test_selo_modal_nao_nasce_vazio.py:141`)

   Contraprova executada somente em memória: inserir no começo de `garantirRequisitos()` o trecho abaixo passou tanto nesse selo quanto no oráculo JS novo; cancelar faria o chamador interromper antes do POST. (`app/templates/ponto/bater_ponto_v2.html:285`, `app/templates/ponto/bater_ponto_v2.html:312`; `app/ponto/tests/test_l030_forma_js.py:40`, `app/ponto/tests/test_l030_forma_js.py:55`)

   ```js
   if (!await hxConfirmar('GPS ausente', 'Continuar')) {return false;} /* naoBloquear */
   ```

6. **Os mutantes declarados mordem; a cobertura não é universal.**

   - **403:** controle verde e mutantes nos dois kernels exercitam a mesma asserção estrutural. Ela encontra `Response(status=403)` sob condições que leem coordenadas/situação; não cobre qualquer forma possível de interrupção. (`app/api/tests/test_l030_selo_porta.py:37`, `app/api/tests/test_l030_selo_porta.py:60`, `app/api/tests/test_l030_selo_porta.py:78`)
   - **Quatro consultas:** executei os quatro mutantes individuais; todos retornaram exit 1 por coordenada parcial contada como geo. A extração exige a quantidade esperada de consultas e a mutação exige exatamente uma guarda removida. (`app/ponto/tests/test_bo0611_05_leitores_geo.py:71`, `app/ponto/tests/test_bo0611_05_leitores_geo.py:95`, `app/ponto/tests/test_bo0611_05_leitores_geo.py:123`)
   - **Detector:** controle verde ignora parcial; retirar a longitude faz a execução real chegar a `TypeError`. (`app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:142`)
   - **JS:** os mutantes exigem âncora existente e foram detectados; permanece a contraprova do item 5. (`app/ponto/tests/test_l030_forma_js.py:74`, `app/ponto/tests/test_l030_forma_js.py:88`)
   - **Quatro templates:** por inspeção, exigem âncora única, controle completo e mapa efetivamente criado pelo mutante antes de exigir a falha da mesma asserção. Não há `subTest` interno engolindo essa falha. Execução Django aqui: **[nao verificado]**. (`app/ponto/tests/test_bo0611_05_templates_geo.py:44`, `app/ponto/tests/test_bo0611_05_templates_geo.py:177`, `app/ponto/tests/test_bo0611_05_templates_geo.py:190`)

   Os quatro módulos puros rodaram com **15/15 verdes**, incluindo suas verificações de mutação. POSTs reais, renderizações Django e os 100/100 relatados não foram reexecutados aqui: **[nao verificado]**. (`app/api/tests/test_l030_selo_porta.py:68`; `app/ponto/tests/test_l030_forma_js.py:68`; `app/ponto/tests/test_bo0611_05_leitores_geo.py:111`; `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:84`)

7. **Censo de escrita, leitura e significado:**

   | Campo/contrato | Quem escreve e quem lê |
   |---|---|
   | `Batida.latitude/longitude` | Captura JS → request/fila → kernels/triagem → sanitização e criação pela porta. Persistência em `DecimalField` nullable. (`app/templates/ponto/bater_ponto_v2.html:178`; `app/static/pwa.js:116`; `app/api/views_core.py:403`; `app/ponto/services/triagem_batida.py:700`; `app/ponto/registro_batida.py:105`; `app/ponto/models.py:58`) |
   | `contexto_json.batida_lat/lon` | Geofence escreve `float` ou `None`, nos escalares e ocorrências. Relatório/modal leem o par; reavaliador filtra pares completos; advertência copia os valores. (`app/ponto/services/geofence.py:327`, `:369`, `:403`; `app/templates/chamados/partials/_relatorio_geo.html:30`; `app/templates/chamados/partials/modal_fio.html:480`; `app/chamados/services/acoes_chamado.py:203`) |
   | `situacao` | Porta cadastral admite a operação; afastamento escreve `afastado`, reversor escreve `ativo`, desligamento escreve `desligado`. Na batida, o campo alimenta divergência/cobrança pelo juiz. (`app/colaboradores/services/colaborador.py:32`; `app/ponto/views.py:2435`; `app/ponto/management/commands/reverter_situacao_afastado.py:73`; `app/colaboradores/services/desligamento.py:104`; `app/ponto/services/afastado_avisa.py:38`) |
   | Resposta HTTP | API devolve sucesso normal, dedup ou colisão ignorada. V2 aceita qualquer HTTP OK; fila apaga em OK. Logo, 200/`sucesso=True` não implica criação nova. (`app/api/views_core.py:517`, `:668`, `:798`; `app/templates/ponto/bater_ponto_v2.html:327`; `app/static/pwa.js:158`) |
   | Flags JS | `gpsLat/gpsLng` recebem fix; `_enviando` evita concorrência local; `_fotoBlob` controla selfie; `gps_aviso_visto` evita repetir aviso. (`app/templates/ponto/bater_ponto_v2.html:145`, `:178`, `:287`, `:308`; `app/templates/ponto/triagem/apto.html:277`) |

   `''` é normalizado pela porta; `Decimal` é persistência, `float` é representação no JSON. Zero continua com significado divergente preexistente: geofence o considera sem GPS e o serializa como `None`. (`app/ponto/registro_batida.py:5`; `app/ponto/models.py:58`; `app/ponto/services/geofence.py:247`, `app/ponto/services/geofence.py:327`)

   **Situação ainda bloqueia login e refresh**, embora não bloqueie o corpo da batida autenticada. Os testes novos usam `force_authenticate`, portanto não demonstram acesso real nessas situações. (`app/api/login_helpers.py:141`; `app/api/views_core.py:172`; `app/api/views.py:115`; `app/api/tests/test_l030_batida_pela_porta.py:35`)

   Nos sinais inspecionados, núcleo/cartório trabalham com timestamp; realtime publica IDs/timestamp; reconciliador recebe colaborador. Geofence ocorre depois da porta, com exceção capturada; sem posto, não cria alerta `sem_gps`. (`app/ponto/signals.py:19`, `:108`, `:131`, `:213`; `app/ponto/realtime.py:32`; `app/api/views_core.py:768`; `app/ponto/services/geofence.py:248`)

   Bloqueio local, fila e interpretação da resposta pelo Android/iOS efetivamente instalado: **[nao verificado]**.

8. **Há duas classes relevantes presentes: barreira residual e lacuna concreta de selo.** A confirmação da triagem pode retornar antes da porta; o selo modal e o oráculo JS admitem a confirmação bloqueante demonstrada no item 5. (`app/ponto/views.py:316`; `app/core/tests/test_selo_modal_nao_nasce_vazio.py:141`; `app/ponto/tests/test_l030_forma_js.py:40`)

   Também permanece a resposta de sucesso para colisão sem nova materialização, consumida como sucesso pelo PWA. Isso exige preservar a distinção entre “requisição aceita” e “nova batida criada”. (`app/ponto/registro_batida.py:150`; `app/api/views_core.py:669`; `app/static/pwa.js:158`)

   Não identifiquei novo juiz duplicado, segundo escritor ou derivação paralela de classificação nos sítios alterados; eles continuam usando a porta e as autoridades existentes. (`app/api/views_core.py:640`; `app/api/views.py:648`; `app/ponto/services/vinculo_na_lista.py:87`; `app/core/juizes.py:1191`)

   Ordem efetiva das travas, liberação da vaga, pouso, smoke nas duas cascas com col677 e certificação independente do hash/base: **[nao verificado]**. O smoke consta como pré-requisito do aval registrado. (`app/docs/CORTES.md:440`)

## 10/11 15:32 · sessao f27b4a22fdea · - · - · origem=Ronald (tmux)

1. **GPS/situação nos kernels: removidos os gates explícitos; não há nova recusa por GPS nesses corpos.** Ambos leem coordenadas sem fallback e passam à porta (`app/api/views_core.py:403`, `app/api/views_core.py:640`, `app/api/views.py:416`, `app/api/views.py:648`). Permanecem autenticação, colaborador ausente e tipo inválido (`app/api/views_core.py:385`, `app/api/views_core.py:391`, `app/api/views_core.py:408`; `app/api/views.py:398`, `app/api/views.py:404`, `app/api/views.py:421`).

   **“Toda barreira” não pode ser aprovado literalmente:** dedup responde antes da porta (`app/api/views_core.py:510`, `app/api/views_core.py:514`, `app/api/views.py:523`, `app/api/views.py:527`); colisão oposta pode não materializar (`app/ponto/registro_batida.py:145`, `app/ponto/registro_batida.py:150`). Estes mecanismos são preexistentes no diff fornecido [nao verificado quanto ao histórico git].

2. **Sem coordenadas, os caminhos que criam Batida passam pela porta e sanitizam para None; não encontrei fallback para posto ou zero nesse trajeto.** Request ausente produz None (`app/api/views_core.py:403`, `app/api/views_core.py:404`); kwargs seguem intactos (`app/api/views_core.py:646`, `app/api/views_core.py:647`; irmão `app/api/views.py:654`, `app/api/views.py:655`); `''`/None/não numérico viram None antes dos únicos `objects.create` da porta (`app/ponto/registro_batida.py:5`, `app/ponto/registro_batida.py:105`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`). Campos persistidos são DecimalField nullable (`app/ponto/models.py:58`, `app/ponto/models.py:59`). Coordenada parcial permanece parcial, sem fabricação da outra (`app/api/tests/test_l030_batida_pela_porta.py:65`).

3. **Templates e fila não interrompem especificamente por GPS ausente.** V2 tenta obter GPS e retorna true (`app/templates/ponto/bater_ponto_v2.html:292`, `app/templates/ponto/bater_ponto_v2.html:297`), envia online ou enfileira nos dois caminhos (`app/templates/ponto/bater_ponto_v2.html:322`, `app/templates/ponto/bater_ponto_v2.html:333`, `app/templates/ponto/bater_ponto_v2.html:346`). Triagem offline avisa sem confirmação e enfileira (`app/templates/ponto/triagem/apto.html:275`, `app/templates/ponto/triagem/apto.html:282`). IndexedDB aceita os valores sem gate, sync omite apenas os nulos e não descarta 403; apaga em HTTP OK ou 400 (`app/static/pwa.js:107`, `app/static/pwa.js:116`, `app/static/pwa.js:145`, `app/static/pwa.js:158`, `app/static/pwa.js:166`).

   **Há barreiras preexistentes por outros motivos:** selfie ausente impede v2 (`app/templates/ponto/bater_ponto_v2.html:287`, `app/templates/ponto/bater_ponto_v2.html:312`); falha de câmera offline esconde confirmar na triagem (`app/templates/ponto/triagem/apto.html:253`, `app/templates/ponto/triagem/apto.html:255`).

   **Retorno antes do registro na triagem online:** form sem action faz submit na rota atual (`app/templates/ponto/triagem/apto.html:86`, `app/templates/ponto/triagem/apto.html:235`), view recalcula triagem e, para POST E com turno aberto e sem confirmar, salva contexto e retorna confirmação antes de `ramo_registrar_es` (`app/ponto/views.py:259`, `app/ponto/views.py:316`, `app/ponto/views.py:330`, `app/ponto/views.py:346`). A confirmação oferece registrar ou cancelar (`app/templates/ponto/triagem/turno_confirmar.html:15`, `app/templates/ponto/triagem/turno_confirmar.html:19`). Tela E obsoleta quando um turno aparece antes do POST pode entrar nesse ramo — inferência dessas condições, sem ocorrência demonstrada [nao verificado]. Não é condicional à ausência de GPS (`app/ponto/views.py:316`).

4. **Censo escritor/leitor e semântica:**

   - **Batida.latitude/longitude:** geolocalização escreve flags JS e campos ocultos (`app/templates/ponto/bater_ponto_v2.html:178`, `app/templates/ponto/bater_ponto_v2.html:180`; `app/templates/ponto/triagem/base_triagem.html:59`); API lê e porta sanitiza/persiste (`app/api/views_core.py:403`, `app/ponto/registro_batida.py:105`, `app/ponto/registro_batida.py:157`). Triagem web também chama essa porta (`app/ponto/services/triagem_batida.py:700`, `app/ponto/services/triagem_batida.py:707`). Recuperação da confirmação usa somente coordenadas anteriormente recebidas na sessão (`app/ponto/services/triagem_batida.py:505`).
   - **contexto_json.batida_lat/lon:** geofence escreve float ou None tanto no escalar quanto em ocorrencias (`app/ponto/services/geofence.py:327`, `app/ponto/services/geofence.py:369`); relatório/modal leem os dois campos (`app/templates/chamados/partials/_relatorio_geo.html:30`, `app/templates/chamados/partials/modal_fio.html:480`). Reavaliador extrai formatos novo/legado e filtra pares completos (`app/ponto/services/geofence.py:379`, `app/ponto/services/geofence.py:403`). Advertência apenas copia os valores (`app/chamados/services/acoes_chamado.py:203`).
   - **None × zero:** Batida admite zero numérico; geofence usa truthiness e trata zero como sem GPS, e serialização do contexto converte zero a None; divergência preexistente, fora do escopo zero declarado (`app/ponto/models.py:58`, `app/ponto/services/geofence.py:247`, `app/ponto/services/geofence.py:327`, `app/ponto/services/geofence.py:369`).
   - **situacao:** afastamento escreve afastado pela porta cadastral (`app/ponto/views.py:2433`); reversor escreve ativo (`app/ponto/management/commands/reverter_situacao_afastado.py:73`); desligamento escreve desligado e desativa acesso (`app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:111`); operação cadastral declara campo (`app/colaboradores/services/colaborador.py:32`). Batida lê para divergência/cobrança pelo juiz, sem gate (`app/ponto/services/afastado_avisa.py:38`, `app/ponto/services/afastado_avisa.py:41`, `app/api/views_core.py:394`).
   - **Situação ainda barra LOGIN/REFRESH:** helper nega qualquer situação !=ativo com 403 (`app/api/login_helpers.py:38`, `app/api/login_helpers.py:141`); refresh nega com 401 nos dois kernels (`app/api/views_core.py:172`, `app/api/views.py:115`). Teste novo usa force_authenticate e não prova esses trajetos (`app/api/tests/test_l030_batida_pela_porta.py:35`).
   - **HTTP:** API produz sucesso/ID também para dedup e colisão sem nova escrita (`app/api/views_core.py:517`, `app/api/views_core.py:668`), além do sucesso normal (`app/api/views_core.py:798`). V2 considera qualquer HTTP OK sucesso; fila apaga em OK (`app/templates/ponto/bater_ponto_v2.html:327`, `app/static/pwa.js:158`). Logo “200/sucesso” significa aceitação/idempotência, não necessariamente nova Batida (`app/api/views_core.py:514`, `app/api/views_core.py:655`).
   - **Flags JS:** gpsLat/gpsLng começam null e recebem fix real; `_enviando` é trava concorrente local; `_fotoBlob` é pré-requisito v2; `gps_aviso_visto` somente evita aviso repetido (`app/templates/ponto/bater_ponto_v2.html:145`, `app/templates/ponto/bater_ponto_v2.html:178`, `app/templates/ponto/bater_ponto_v2.html:287`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/templates/ponto/triagem/apto.html:277`).
   - **Sinais:** núcleo/cartório usam timestamp; realtime publica IDs e timestamp; reconciliador recebe colaborador — não encontrei latitude/longitude nesses receivers (`app/ponto/signals.py:19`, `app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/realtime.py:32`, `app/ponto/signals.py:213`). Geofence é chamada depois da porta e sua exceção é capturada na API (`app/api/views_core.py:640`, `app/api/views_core.py:768`); sem posto, não cria alerta sem_gps (`app/ponto/services/geofence.py:247`, `app/ponto/services/geofence.py:248`).
   - **Cliente nativo real:** código-fonte Android/iOS não encontrado no censo de arquivos realizado; compreensão do novo HTTP pelo cliente, bloqueio local GPS e comportamento de sua fila [nao verificado].

5. **Limite adicional:** middleware isenta `/api/`, mas pode retornar 403 nas páginas web por plataforma; não é gate GPS e não mudou no diff fornecido (`app/core/middleware.py:247`, `app/core/middleware.py:254`, `app/core/middleware.py:268`, `app/core/middleware.py:283`).

## 10/11 15:32 · sessao 296f7a425d16 · - · - · origem=Ronald (tmux)

**5. Testes alterados.** As inversões de GPS/situação correspondem ao aval: passam a exigir 200 **e persistência**, preservam cobrança de afastamento e verificam alertas/chamados sem GPS; a exceção permissiva de 403 foi retirada do teste de invariantes. Referências: `app/api/tests/test_gps_obrigatorio.py:19`, `:24`, `:34`; `app/api/tests/test_api_bater_ponto.py:62`, `:84`; `app/ponto/tests/test_afastado_na_batida.py:109`, `:117`; `app/ponto/tests/test_afastado_nunca_bloqueia.py:30`; `app/ponto/tests/test_batida_sideeffects.py:119`; `app/chamados/tests/test_invariantes_disputa.py:291`.

**Ressalva comprovada no selo modal:** a condição nova acomoda a retirada das chamadas, mas **não garante que o fluxo não volte a prender**. Ela só procura as strings `hxConfirmar(` e `naoBloquear`; não verifica argumentos ou fluxo. `app/core/tests/test_selo_modal_nao_nasce_vazio.py:138`, `:141`.

Prova somente em memória: inseri no início de `garantirRequisitos()`:

```js
if (!await hxConfirmar('GPS ausente', 'Continuar')) {return false;} /* naoBloquear */
```

Tanto o método extraído do selo modal quanto `afirmar_sem_barreira()` passaram. A nova confirmação bloqueante seria cortada pelo chamador antes do envio. Motivo: no v2, o oráculo só examina condições com `gpsLat/gpsLng/lat/lng`; a proibição genérica de confirmação é exclusiva da triagem. `app/templates/ponto/bater_ponto_v2.html:285`, `:312`; `app/ponto/tests/test_l030_forma_js.py:40`, `:50`, `:55`.

**6. Mutantes declarados mordem; sem âncora vazia identificada.**

- API: controle verde e sete condições mutantes em cada kernel; a mesma asserção rejeita o 403. O `assertRaisesRegex` fica dentro do `subTest`, portanto a falha esperada não é engolida por subteste interno. Limite: é detector estrutural de `Response(status=403)` condicionado diretamente aos campos, não prova universal de toda barreira. `app/api/tests/test_l030_selo_porta.py:37`, `:48`, `:60`, `:78`, `:92`.
- JS: quatro mutantes; exige que a âncora exista antes de substituir e usa o mesmo oráculo. Todos passaram no teste de mordida. Continua a lacuna da confirmação genérica no v2 descrita acima. `app/ponto/tests/test_l030_forma_js.py:74`, `:88`, `:89`.
- Consultas dos quatro leitores: seleção AST exige quantidade esperada de consultas; mutação exige exatamente uma guarda removida; `morder()` não contém `subTest`. Executei os quatro `--mutante` individualmente: **todos exit 1**, com falha em coordenada parcial na asserção real. `app/ponto/tests/test_bo0611_05_leitores_geo.py:71`, `:95`, `:112`, `:123`, `:143`.
- Detector: mutação exige um filtro removido; controle parcial retorna `[]`; mutante executa o detector e produz `TypeError` por longitude `None`. `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:67`, `:78`, `:142`.
- Quatro templates: âncora exata com ocorrência única; controle completo; confirmação de que mutante efetivamente criou mapa; depois exige falha da mesma asserção sem `subTest`. Estrutura correta por inspeção; execução aqui **[nao verificado]**. `app/ponto/tests/test_bo0611_05_templates_geo.py:44`, `:136`, `:177`, `:187`, `:190`.
- Testes POST reais verificam porta chamada uma vez, parâmetros recebidos intactos, uma batida, coordenadas persistidas e ID da resposta. Executados aqui **[nao verificado]**. `app/api/tests/test_l030_batida_pela_porta.py:36`, `:40`, `:49`, `:60`, `:65`.

Executei `python3 -B -m unittest -v` nos quatro módulos puros: **15/15 verdes**, incluindo mutantes em memória. Não houve escrita de arquivo. Django/freezegun não estão disponíveis neste ambiente; suíte 100/100, renderizações reais e runner informado permanecem **[nao verificado]** nesta exploração. Os contratos executados estão em `app/api/tests/test_l030_selo_porta.py:68`, `app/ponto/tests/test_l030_forma_js.py:68`, `app/ponto/tests/test_bo0611_05_leitores_geo.py:111`, `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:84`.

**8. Classe relevante:** há uma **lacuna concreta de selo** para confirmação bloqueante genérica no v2; isso não invalida a mordida dos mutantes declarados, mas impede afirmar que o selo modal garante a classe inteira de fluxo que prende colaborador. `app/core/tests/test_selo_modal_nao_nasce_vazio.py:141`; `app/ponto/tests/test_l030_forma_js.py:40`, `:55`.

## 10/11 15:31 · sessao 4ef31c947e0c · - · - · origem=Ronald (tmux)

3. No diff fornecido, a mudança de geo restringe leitores ao par completo; o detector continua chamando o juiz `classificar_evidencia`, sem nova decisão de distância ou raio (`app/ponto/services/vinculo_na_lista.py:49`, `:87`). A autoridade registrada permanece `classificar_posicao` (`app/core/juizes.py:1191`), chamada pelo emissor (`app/ponto/services/geofence.py:270`). Mudança em `motor_calculo_v2` não consta do diff fornecido; comparação independente contra a base indisponível porque a cópia não contém repositório Git [nao verificado].

4. Todos os leitores declarados corrigidos rejeitam coordenada parcial:
- Detector: `app/ponto/services/vinculo_na_lista.py:49`.
- Três consultas de vínculo: `app/colaboradores/views.py:337`, `:356`, `:395`.
- Contador offline: `app/ponto/views.py:485`.
- Quatro templates: `app/templates/core/_d2_feed.html:16`, `app/templates/ponto/batidas_offline_pendentes.html:96`, `app/templates/chamados/partials/_relatorio_geo.html:30`, `app/templates/chamados/partials/modal_fio.html:480`.

Não encontrei outro leitor **decisório de Batida** que aceite latitude presente com longitude ausente: listagem geral já exige o par (`app/colaboradores/views.py:53`); detalhe/histórico exige o par (`app/colaboradores/services/detalhe.py:22`, `:53`); fio exige o par na consulta (`app/chamados/views.py:692`); pino retorna sem GPS para qualquer componente ausente (`app/colaboradores/views.py:2474`); drawer exige ambos (`app/colaboradores/services/drawer.py:63`); dashboard exige ambos (`app/templates/core/dashboard2.html:242`, `:287`); reavaliação de contexto filtra ambos (`app/ponto/services/geofence.py:403`).

Há leitores/transmissores independentes por componente, sem decisão “tem geo”: painel operacional conserva latitude e longitude separadamente (`app/core/services/painel_op.py:227`, `:228`), endpoint do detalhe serializa cada campo (`app/core/views.py:1024`, `:1025`), painel situacional conserva o par original (`app/colaboradores/services/situacional.py:145`) e o entrega em atributos (`app/colaboradores/services/situacional.py:199`, `:200`; `app/templates/colaboradores/partials/_ps_linha.html:1`). Não são equivalentes a um leitor latitude-só decidindo geo (`app/colaboradores/views.py:2474`; `app/templates/core/dashboard2.html:242`).

Latitude-só encontrada nas exclusões expressas: alerta geofence (`app/templates/core/dashboard_supervisor.html:90`; `app/templates/colaboradores/detalhe.html:142`) e Posto (`app/ponto/services/vinculo_na_lista.py:48`, `:66`; `app/templates/colaboradores/partials/_postos_tabela.html:23`; `app/templates/colaboradores/partials/painel_posto.html:28`; `app/templates/core/painel_tv.html:154`). A autoridade ainda usa truthiness, portanto zero é sem GPS nela, enquanto leitores alterados usam `None`; isso fica na exclusão 0.0 declarada (`app/ponto/services/geofence.py:247`; `app/templates/core/_d2_feed.html:16`).

8. Há ressalva **preexistente**, sem mudança no diff: colisão de tipo oposto ainda pode devolver `None` antes de criar (`app/ponto/registro_batida.py:138`, `:145`, `:150`), e API responde sucesso com `ignorada_intervalo` (`app/api/views_core.py:655`, `:669`, `:675`). Portanto “qualquer tentativa sempre materializa uma batida nova” não é contrato atendido pela cópia (`app/ponto/registro_batida.py:113`, `:120`, `:150`).

Não identifiquei novo juiz duplicado ou derivação paralela na alteração do detector: seleção do par completo alimenta o juiz existente (`app/ponto/services/vinculo_na_lista.py:49`, `:87`). O relógio lido pelo detector é `timezone.localdate()` já existente no trecho anterior à mudança fornecida (`app/ponto/services/vinculo_na_lista.py:46`). Comprovação de ordem de pouso, pré-requisitos e execução real das travas depende de evidência ausente no dado [nao verificado].

_3426 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
