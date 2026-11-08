# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 11:32** · publicado em **08/11/2026 11:35** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 11:32 · sessao ab592c50e326 · FIO-E0-V4-E1 · verificador · origem=Code

**REPROVADO no alcance universal do aval.** A correção atende aos casos originais com episódio cujo juiz responde um dia, mas o fallback ainda permite gravar o pedido e reproduzir 3a/3b. A catraca verifica a chamada ao helper, sem garantir essa semântica. (`app/chamados/services/fio.py:1258`, `app/chamados/services/fio.py:1262`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:59`)

1. **CENSO — NÃO PROCEDENTE a afirmação de que toda escrita grava exclusivamente o dia do juiz.**

   Os sítios de atribuição e persistência encontrados são:

   | Sítio | Valor e classificação |
   |---|---|
   | `PerguntaDisputa._nascer`: `app/chamados/models.py:2840` | `create(data_evento=_dia_fato)`. Obtém `_dia_fato` pelo helper em `app/chamados/models.py:2749`; o helper pode devolver o pedido, conforme `app/chamados/services/fio.py:1258`–`1262`. |
   | `reapontar_resolvedora`: `app/chamados/services/fio.py:843` | Atribui `_dia`, calculado pelo mesmo helper em `app/chamados/services/fio.py:842`. Acrescenta o campo à lista em `app/chamados/services/fio.py:844` e persiste em `app/chamados/services/fio.py:873`. Mesma ressalva de fallback. |
   | `rechavear_data_evento`: `app/chamados/services/fio.py:962` | Atribui o parâmetro `novo_dia` e persiste em `app/chamados/services/fio.py:968`; não consulta o juiz nessa porta. |
   | Chamador de rechaveamento, apply | O valor vem de `dia_do_turno_da_batida`, passa por `ln['novo']` e chega à porta. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:154`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:358`) |
   | Chamador de rechaveamento, reversão | Grava a data recuperada do arquivo de reversão, sem recalculá-la pelo juiz. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:412`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:423`) |

   Há ainda três atribuições **sem persistência própria**: o proxy da validação inicial e os dois desfazimentos em memória antes de retornar por recusa. (`app/chamados/models.py:2756`; `app/chamados/services/fio.py:855`, `app/chamados/services/fio.py:864`)

   Não encontrei escritor adicional de `data_evento` por `update`, bulk, `setattr` ou construtor com apelido. O bulk existente de Pergunta grava somente os três campos da lavra; o `save` do modelo delega a persistência sem normalizar `data_evento`. (`app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:207`; `app/chamados/models.py:3242`–`3245`) Exaustividade contra formas dinâmicas arbitrárias: **[nao verificado]**.

   **Chamado incluído:** nesta cópia, a classe é `ChamadoColaborador` e **não possui campo `data_evento`**; possui `contexto_json`, `celula` e timestamps. Suas portas de INSERT são `abrir`, com `get_or_create`, e `criar`, com `create`. (`app/chamados/models.py:38`–`101`, `app/chamados/models.py:971`, `app/chamados/models.py:1095`)

   Essas duas portas preservam o contexto recebido e resolvem a célula por `_celula_de`, que consulta `juizes.dia_do_fato(fortes=True)`. Não são escritoras literais de `data_evento`, nem normalizam o contexto pelo novo helper da Pergunta. (`app/chamados/models.py:939`–`940`, `app/chamados/models.py:958`, `app/chamados/models.py:1067`, `app/chamados/models.py:1234`–`1240`)

2. **CATRACA — NÃO PROCEDENTE como garantia completa; PROCEDENTE a mordida dos cinco mutantes oficiais.**

   O P13 identifica os escritores explícitos atuais. Reconhece apelidos de import e atribuições simples de alias, `update(data_evento=...)`, `setattr` com nome literal e bulk com campo literal. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:37`–`54`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:84`–`108`)

   A V15 fiscaliza valores somente em `_nascer` e `reapontar_resolvedora`. Para rechaveamento, verifica o **arquivo chamador**, não a procedência do valor. Isoladamente, também não reprova escritores novos encontrados pelo censo: apenas exige `assertTrue(censo())`; a rejeição de sítios externos pertence a outro teste, no P13. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:24`–`30`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:118`–`134`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:143`–`146`)

   Escapes da fiscalização:

   - **Dentro de porta permitida:** trocar `p.data_evento = _dia` por `setattr(p, 'data_evento', data_evento)` escapa da V15. Ela examina `ast.Assign` e keywords diretos; o P13 aceita a localização. Bulk com campos posicionais também não recebe fiscalização de valor pela V15. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:74`–`88`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:117`–`119`)
   - **Fluxo por variável:** `kw={'data_evento': d}; qs.update(**kw)` e `campos=['data_evento']; qs.bulk_update(ps, campos)` escapam do P13, que procura constantes nos argumentos da própria chamada. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:105`–`108`)
   - **Outra classe:** construtor direto só é reconhecido quando pertence aos aliases de `PerguntaDisputa`; chamadas ORM com keyword literal são reconhecidas independentemente da classe. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:29`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:94`–`97`)
   - **Semântica do helper:** chamar uma função chamada `dia_do_turno_da_linha` basta para ligar o valor ao “juiz”; seu fallback não é examinado. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:45`–`66`)

   Os cinco mutantes oficiais detectam regressões específicas das atribuições e da costura. Isso não cobre os escapes acima. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:138`–`154`) Execução da suíte Django/BD: **[nao verificado]**.

3. **FALLBACK — PROCEDENTE: reabre o defeito em caminho de produção alcançável.**

   Sem episódio, o helper devolve pedido/gravado diretamente; com episódio e resposta `None`, faz o mesmo. Já o universo consulta A2 para decidir o dia dessas linhas. Batida retratada pode produzir `None`, e `_nascer` aceita episódio explícito sem verificar retratação. (`app/chamados/services/fio.py:1258`–`1262`, `app/chamados/services/fio.py:1401`–`1408`; `app/ponto/turnos.py:2917`–`2926`; `app/chamados/models.py:2745`–`2747`)

   **Contraexemplo construído de 3a**, com `D0=10/09` e `DIA=18/10`: K é acervo sem episódio gravado em D0; A tem episódio vivo `a`, também gravado em D0, mas o juiz responde DIA; `b` é episódio retratado ainda sem Pergunta. O vínculo entre acervo e episódios pela data gravada é exatamente o de `do_fato`. (`app/chamados/models.py:2627`–`2635`; datas da base: `app/chamados/tests/test_e1_v13.py:40`, `app/chamados/tests/test_e1_v13.py:60`–`63`)

   - Primeira `abrir(b, D0)`: helper responde D0. O universo encontra K; A2(K) alcança A, cujo dia é DIA, e recusa K. Não existe recusada do episódio `b`; nasce L com episódio `b` e data D0. (`app/chamados/services/fio.py:1258`–`1262`, `app/chamados/services/fio.py:1300`–`1308`, `app/chamados/services/fio.py:1406`; `app/chamados/models.py:2781`, `app/chamados/models.py:2840`)
   - Repetição idêntica: encontra K e L. A2(L) passa por K e alcança A=DIA; ambas são recusadas para o pedido D0. Agora há recusada do episódio `b`, portanto levanta **`FatoEmOutroDia`**. (`app/chamados/services/fio.py:1300`–`1308`, `app/chamados/services/fio.py:1406`–`1409`; `app/chamados/models.py:2781`–`2792`)

   Assim, a primeira chamada cria e a segunda idêntica recusa. Ocorrência desse estado na frota: **[nao verificado]**.

   **Conversa é diferente:** tem contrato explícito de nascer sempre, gravar dia civil e dispensar comparação de turno. Sua falta de dedup não constitui esse 3a. (`app/chamados/models.py:2700`–`2707`, `app/chamados/models.py:2765`; `app/chamados/juizes.py:1991`–`1992`)

4. **MEMO — NÃO PROCEDENTE a hipótese de colisão entre os usos indicados.**

   | Uso | Chave | Escritas |
   |---|---|---|
   | Dia da batida | `('dia_do_turno', batida_id)` | `app/chamados/services/fio.py:1233`, `app/chamados/services/fio.py:1238` |
   | Colaborador | `('colab', colaborador_id)` | `app/chamados/services/fio.py:1268`, `app/chamados/services/fio.py:1271`, `app/chamados/services/fio.py:1274` |
   | Vizinhos | `(pk, chave_do_dia, batida_id)` | `app/chamados/services/fio.py:1280`, `app/chamados/services/fio.py:1287`, `app/chamados/services/fio.py:1390` |

   As duas primeiras famílias têm duas posições e marcadores distintos; a terceira tem três posições. Os acessos usam essas chaves completas. A resolvedora apenas repassa o dict ao helper: não introduz outra escrita nele. (`app/chamados/services/fio.py:1234`–`1239`, `app/chamados/services/fio.py:1269`–`1288`; `app/chamados/juizes.py:1994`)

5. **3a/3b ORIGINAIS — PROCEDENTE a cura com juiz não nulo; NÃO PROCEDENTE a garantia universal.**

   **3a, episódio vivo:** pedidos D0 e DIA viram o mesmo `_dia_fato`; busca, célula e criação usam esse valor. A repetição encontra a canônica e retorna sem novo nascimento. Esse é também o comportamento exigido pelo teste novo. (`app/chamados/models.py:2749`–`2751`, `app/chamados/models.py:2768`, `app/chamados/models.py:2779`, `app/chamados/models.py:2837`, `app/chamados/models.py:2840`; `app/chamados/tests/test_e1_v15.py:52`–`66`)

   **3b, episódio vivo:** linha gravada em D0, juiz=DIA, destino=DIA passa a ser aceita, porque a resolvedora lê DIA pelo helper. O reuso preserva D0 no banco. (`app/chamados/juizes.py:1994`, `app/chamados/juizes.py:2013`–`2014`; `app/chamados/services/fio.py:1261`–`1262`; `app/chamados/tests/test_e1_v15.py:90`–`99`)

   **Ainda pode levantar `ResolvedoraCruzada`:** tome K sem episódio, gravada em D0 e canônica por resposta/menor pk, mais A do episódio vivo `a`, gravada em D0 e juiz=DIA. `abrir(a, DIA, destino=DIA)` passa no proxy, semeia A, alcança K por A2={DIA} e elege K. Na segunda validação, o helper de K devolve D0; o gap de 38 dias produz recusa e exceção. (`app/chamados/models.py:2756`–`2762`, `app/chamados/models.py:2802`–`2804`; `app/chamados/services/fio.py:998`–`1000`, `app/chamados/services/fio.py:1259`–`1260`, `app/chamados/services/fio.py:1361`–`1368`; `app/chamados/juizes.py:2022`–`2029`)

   **Variante com episódio na própria linha recusada:** acrescente R respondida, de episódio retratado, gravada em D0 e ligada por K a A. O fecho admite R por A2 e a canônica passa a ser R; seu juiz responde `None`, o helper devolve D0 e ocorre a mesma `ResolvedoraCruzada`. Portanto a resposta à pergunta sobre linha **com episódio** ainda é sim, quando ele está retratado. (`app/chamados/services/fio.py:1238`, `app/chamados/services/fio.py:1262`, `app/chamados/services/fio.py:1363`–`1368`, `app/chamados/services/fio.py:999`; `app/chamados/models.py:2804`) Ocorrência em produção: **[nao verificado]**.

6. **ESCRITORES/LEITORES — PROCEDENTE a divergência residual de significado.**

   | Campo | Quem escreve nos caminhos examinados | Quem lê |
   |---|---|---|
   | `data_evento` | Nascimento, datação e rechaveamento censados acima. (`app/chamados/models.py:2840`; `app/chamados/services/fio.py:843`, `app/chamados/services/fio.py:962`) | Costura pelo helper; fecho pelo juiz/A2; busca pela chave gravada; C6/tranca/plantio pela gravada. (`app/chamados/juizes.py:1994`; `app/chamados/services/fio.py:1196`, `app/chamados/services/fio.py:1232`; `app/chamados/juizes.py:336`, `app/chamados/juizes.py:948`; `app/chamados/services/materializacao.py:345`) |
   | `celula` | Nascimento/reuso pela célula de `_dia_fato`; rechaveamento pela célula de `novo_dia`. (`app/chamados/models.py:2768`, `app/chamados/models.py:2798`, `app/chamados/models.py:2841`; `app/chamados/services/fio.py:959`–`963`) | C6 lê a célula; alimentação da falta exige igualdade com `data_evento`. (`app/chamados/juizes.py:634`–`642`, `app/chamados/juizes.py:1199`–`1214`) |
   | `batida_origem_id` | Explícita ou derivada no nascimento; o `save` usa o mesmo derivador quando ausente. (`app/chamados/models.py:2745`–`2747`, `app/chamados/models.py:3270`–`3289`) | Busca do episódio, juiz do turno e seleção do fallback. (`app/chamados/models.py:2632`; `app/chamados/services/fio.py:1232`–`1239`, `app/chamados/services/fio.py:1259`) |
   | `chamado_destino` | Nascimento, reuso após guardas e reapontamento. (`app/chamados/models.py:2841`, `app/chamados/models.py:2812`; `app/chamados/services/fio.py:871`–`874`) | Validação de turno, destino da listagem, guarda de terminal e marcos do plantio. (`app/chamados/juizes.py:2004`; `app/chamados/services/fio.py:117`, `app/chamados/services/fio.py:778`–`783`; `app/chamados/services/materializacao.py:339`, `app/chamados/services/materializacao.py:409`) |

   O estado residual é explícito: reuso pode associar célula/destino de DIA e manter `data_evento=D0`. A costura julga DIA, enquanto tranca calcula competência por D0; C6 descarta a célula de DIA como alimentação para D0. (`app/chamados/models.py:2797`–`2813`; `app/chamados/tests/test_e1_v15.py:99`; `app/chamados/juizes.py:1994`, `app/chamados/juizes.py:948`–`961`, `app/chamados/juizes.py:638`)

   O derivador de plantio, quando alcançado, continua combinando a hora com a data gravada e ajustando overnight a partir dela. Isso identifica a divergência de leitura; plantio efetivamente executado nesses estados: **[nao verificado]**. (`app/chamados/services/materializacao.py:344`–`345`, `app/chamados/services/materializacao.py:414`–`421`)

   Os leitores permanecerem intactos atende ao recorte literal do aval. Isso, porém, não comprova a premissa “gravada == juiz”: o próprio teste conserva a divergência, e o fallback pode criá-la. Migração integral do acervo em produção: **[nao verificado]**. (`app/chamados/tests/test_e1_v15.py:98`–`99`; `app/chamados/services/fio.py:1262`)

7. **CLASSES DE REPROVA — presentes as seguintes:**

   - **Derivação alternativa à fonte única: PROCEDENTE.** O juiz documenta que `None` significa “sem resposta [...] não há dia a gravar”; o helper substitui essa ausência pelo pedido/gravado e não consulta A2. (`app/ponto/turnos.py:2919`–`2920`; `app/chamados/services/fio.py:1258`–`1262`)
   - **Selo insuficiente: PROCEDENTE quanto à abrangência.** Os mutantes oficiais mordem, mas `setattr`/bulk dentro da porta e a semântica interna do helper permanecem descobertos. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:59`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:74`–`88`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:138`–`154`)
   - **Campo com significados divergentes: PROCEDENTE no acervo/fallback.** Identidade pelo juiz/A2 e chave gravada continuam podendo divergir, com consumidores decisórios lendo a gravada. (`app/chamados/services/fio.py:1222`–`1231`; `app/chamados/tests/test_e1_v15.py:99`; `app/chamados/juizes.py:336`)
   - **Segundo juiz geométrico duplicando o registrado: NÃO PROCEDENTE neste trajeto.** O helper chega a `_T1`, cuja implementação consulta `turnos_do_colab`, autoridade registrada. (`app/core/juizes.py:196`; `app/chamados/services/fio.py:1236`–`1238`; `app/ponto/turnos.py:2902`–`2903`, `app/ponto/turnos.py:2925`)
   - **Segundo escritor explícito fora da porta: NÃO PROCEDENTE no censo encontrado.** Os escritores concretos estão nos três sítios listados; rechaveamento é uma porta declarada, embora aceite valor fornecido. (`app/chamados/models.py:2840`; `app/chamados/services/fio.py:843`, `app/chamados/services/fio.py:962`)
   - **Relógio novo fora da borda: NÃO PROCEDENTE nas alterações examinadas.** O relógio identificado está na porta explícita de conversa. Auditoria transitiva integral de relógios: **[nao verificado]**. (`app/chamados/models.py:2706`–`2707`)
   - **Inversão nova de trava: NÃO PROCEDENTE nos caminhos examinados.** `_nascer` trava Colaborador antes das consultas; reapontamento adquire essa trava por `fato_livre_para` ou diretamente antes da guarda de Chamado. Todos os chamadores/transações externos: **[nao verificado]**. (`app/chamados/models.py:2724`, `app/chamados/models.py:2677`; `app/chamados/services/fio.py:852`–`861`, `app/chamados/services/fio.py:768`)

## 08/11 11:29 · sessao d1aba136826f · - · - · origem=Ronald (tmux)

Checagem adicional concluída: nenhum escritor real extra encontrado fora das portas já listadas. A busca incluiu todos os `.py` da árvore, aliases de import/atribuição e referências fora de `app/`. [nao verificado: artefato persistido do censo]

Os candidatos dinâmicos existentes não escrevem `data_evento`:

- `descarimbar_em_lote(**outros)` recebe campos de resposta/validação (`app/chamados/models.py:1555`; `app/chamados/services/disputa_emissao.py:1494`, `:1531`).
- Atualização dinâmica do Chamado usa somente `urgencia` e `urgencia_original` (`app/chamados/models.py:473`, `:631`, `:636`).
- Bulk de Pergunta grava somente os três campos da lavra (`app/chamados/veredito_lavrado.py:194`, `:207`).
- `CC = apps.get_model(...)` em `bin/r5_idempotencia_frota.py:30` somente consulta chamados em `bin/r5_idempotencia_frota.py:60`.

Os únicos INSERTs diretos de Chamado fora dos gates encontrados são testes em arquivos soltos: `app/tenants/tests.py:62` (`FastTenantTestCase`, `:5`) e `app/relatorios/tests_pdf_chamados.py:34` (`TestCase`, `:18`).

Não encontrei construtor aliased de Pergunta seguido de save em produção, nem bypass real de INSERT de Chamado; permanecem `abrir` e `criar` (`app/chamados/models.py:971`, `:1095`). [nao verificado: prova negativa exaustiva contra formas dinâmicas arbitrárias]

## 08/11 11:29 · sessao cc28a568237e · - · - · origem=Ronald (tmux)

**3. PROCEDENTE: o fallback mantém caminhos que reproduzem 3a/3b.** Sem episódio, devolve diretamente pedido/data gravada; com episódio cujo juiz responde `None`, também devolve esse valor. A ponte A2 usada pelo universo não é consultada pelo helper (`app/chamados/services/fio.py:1258`, `app/chamados/services/fio.py:1262`, `app/chamados/services/fio.py:1406`). Batida retratada pode produzir `None`; a porta aceita `batida_origem_id` explícita sem conferir retratação (`app/ponto/turnos.py:2917`, `app/ponto/turnos.py:2925`, `app/chamados/models.py:2745`).

Contraexemplo concreto de **3a**, por trace:

- D0=10/09; DIA=18/10. Estado inicial: K sem episódio gravada em D0; A com episódio vivo `a`, gravada em D0, juiz=DIA; episódio `b` retratado sem pergunta. Esses estados são representáveis pelo modelo; `do_fato` conecta acervo do mesmo dia aos episódios (`app/chamados/models.py:2630`, `app/chamados/models.py:2632`).
- Primeiro `abrir(b,D0)`: helper=D0; universo encontra K; A2(K) alcança A=DIA, logo recusa K. Não há recusada de episódio `b`, portanto nasce L(b,D0) (`app/chamados/services/fio.py:1300`, `app/chamados/services/fio.py:1304`, `app/chamados/services/fio.py:1406`, `app/chamados/models.py:2781`, `app/chamados/models.py:2840`).
- Repetição idêntica: universo encontra K e L; A2(L) passa por K e alcança A=DIA. Ambas ficam recusadas; agora existe recusada de episódio `b`, disparando `FatoEmOutroDia` (`app/chamados/services/fio.py:1300`, `app/chamados/services/fio.py:1406`, `app/chamados/models.py:2781`, `app/chamados/models.py:2789`).

Contraexemplo concreto de **3b**:

- K sem episódio, menor pk ou respondida, gravada em D0; A de episódio vivo, gravada em D0, juiz=DIA. `abrir(a,DIA,destino=DIA)` semeia A, alcança K pela ponte A2={DIA} e elege K (`app/chamados/services/fio.py:1401`, `app/chamados/services/fio.py:1421`, `app/chamados/services/fio.py:1358`, `app/chamados/services/fio.py:1363`, `app/chamados/services/fio.py:998`).
- A validação inicial do proxy passa usando DIA. No reuso, K sem episódio devolve D0; o gap de 38 dias termina em recusa e `ResolvedoraCruzada` (`app/chamados/models.py:2756`, `app/chamados/models.py:2760`, `app/chamados/models.py:2802`, `app/chamados/models.py:2804`, `app/chamados/services/fio.py:1259`, `app/chamados/juizes.py:2022`, `app/chamados/juizes.py:2029`).
- Variante com episódio: acrescente R respondida, de episódio retratado, gravada em D0, ligada por K ao mesmo A. O fecho admite R por A2 e a elege; o helper recebe `None` do juiz e volta a D0, produzindo a mesma rejeição. Portanto ainda pode rejeitar uma linha **com episódio**, quando ele está retratado (`app/chamados/services/fio.py:1238`, `app/chamados/services/fio.py:1262`, `app/chamados/services/fio.py:1363`, `app/chamados/services/fio.py:999`, `app/chamados/models.py:2804`).

A conversa tem tratamento explícito: nasce sempre, grava dia civil e não passa pela comparação de turno. Isso não reproduz o defeito contratual de dedup de pergunta datada (`app/chamados/models.py:2700`, `app/chamados/models.py:2707`, `app/chamados/models.py:2765`, `app/chamados/juizes.py:1991`).

**4. NÃO PROCEDENTE: não há colisão entre as chaves dos usos descritos.** Dia usa tupla de dois elementos `('dia_do_turno', id)` e escreve em 1238; colaborador usa `('colab', id)` e escreve em 1271/1274; vizinhança usa tupla de três elementos `(pk, chave_do_dia, batida_id)`, escrita em 1287 e 1390. Tamanho ou marcador distinguem esses espaços (`app/chamados/services/fio.py:1233`, `app/chamados/services/fio.py:1238`, `app/chamados/services/fio.py:1268`, `app/chamados/services/fio.py:1271`, `app/chamados/services/fio.py:1274`, `app/chamados/services/fio.py:1280`, `app/chamados/services/fio.py:1287`, `app/chamados/services/fio.py:1390`). A resolvedora apenas repassa o memo ao helper (`app/chamados/juizes.py:1994`).

**5. Cura dos casos originais com episódio vivo: PROCEDENTE; garantia universal: NÃO PROCEDENTE.** Com resposta não nula do juiz, pedidos D0/DIA viram o mesmo `_dia_fato`, usado na busca, célula e criação; a repetição retorna a canônica existente. A resolvedora lê esse mesmo juiz, independentemente da data gravada (`app/chamados/models.py:2749`, `app/chamados/models.py:2768`, `app/chamados/models.py:2779`, `app/chamados/models.py:2837`, `app/chamados/models.py:2840`, `app/chamados/services/fio.py:1261`, `app/chamados/juizes.py:1994`, `app/chamados/juizes.py:2013`). Os contraexemplos acima impedem estender a conclusão aos caminhos de fallback (`app/chamados/services/fio.py:1262`).

Executei uma reprodução somente em memória dos corpos AST reais de helper/A2/fecho/universo/canônica, adaptando o ORM para listas e pré-carregando respostas do juiz: primeira chamada do cenário 3a teve universo vazio/recusada K; repetição teve universo vazio/recusadas K,L; os dois cenários 3b elegeram a linha cujo helper retornou D0. Execução Django/BD e ocorrência em produção: **[nao verificado]**. [nao verificado]

## 08/11 11:28 · sessao d1aba136826f · - · - · origem=Ronald (tmux)

Censo literal de `data_evento`:

| Sítio persistente | Valor gravado |
|---|---|
| `app/chamados/models.py:2840` (`PerguntaDisputa._nascer`) | `_dia_fato`, obtido por `dia_do_turno_da_linha(..., pedido=data_evento)` em `app/chamados/models.py:2749`. |
| `app/chamados/services/fio.py:843`, persistido em `app/chamados/services/fio.py:873` | `_dia`, obtido pelo helper em `app/chamados/services/fio.py:842`; campo acrescentado à lista em `app/chamados/services/fio.py:844`. |
| `app/chamados/services/fio.py:962`, persistido em `app/chamados/services/fio.py:968` | Parâmetro `novo_dia`, sem consulta local ao juiz; apply passa o resultado de `dia_do_turno_da_batida` (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `:154`, `:358`); reversão passa a data lida do arquivo (`:412`, `:423`). |

`_px.data_evento = _dia_fato` é proxy em memória (`app/chamados/models.py:2756`); `p.data_evento = None` desfaz em memória duas recusas que retornam antes do save (`app/chamados/services/fio.py:855`, `:864`). Nenhum escritor explícito adicional em produção foi encontrado; exaustividade além das formas pesquisadas permanece [nao verificado].

**Chamado incluído:** `ChamadoColaborador` não declara campo `data_evento`; declara `contexto_json`, `celula` e timestamps (`app/chamados/models.py:38`–`:101`). Suas duas portas concretas de INSERT são `abrir` (`app/chamados/models.py:971`) e `criar` (`app/chamados/models.py:1095`). Elas gravam contexto recebido por `defaults`/`campos` (`app/chamados/models.py:939`, `:1095`) e resolvem célula em `app/chamados/models.py:958`/`:1067`; `_celula_de` pergunta a `chamados.juizes.dia_do_fato(fortes=True)` (`app/chamados/models.py:1234`), não ao helper de data da Pergunta. Portanto não se deve classificá-las como escritores de um campo inexistente.

**Catraca: PROCEDENTE para os escritores explícitos atuais; NÃO PROCEDENTE como garantia geral.** P13 identifica esses sete registros sintáticos: proxy 2756, create 2840, atribuições fio 843/855/864/962 e save explícito fio 968; seus detectores estão em `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:84`–`:108`. V15 verifica os valores de duas portas (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:24`); rechaveamento recebe somente fiscalização dos arquivos chamadores (`:97`, `:133`).

- Apelidos de import e aliases simples de `PerguntaDisputa` são cobertos pelo P13 (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:37`).
- `update(data_evento=...)`, bulk com campo literal e `setattr(...,'data_evento',...)` são detectados fora da porta (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:94`, `:98`, `:107`).
- Fluxo por variável escapa: `campos={'data_evento':d}; qs.update(**campos)` e `campos=['data_evento']; qs.bulk_update(ps,campos)`. Os detectores só procuram constantes nos argumentos locais da chamada (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `:105`, `:107`).
- Construtor de outra classe não entra na lista de aliases de Pergunta (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:29`, `:96`).
- Dentro de porta permitida, V15 ignora `setattr`, atribuição anotada/aumentada e bulk por campo posicional: inspeciona somente `ast.Assign` e keywords diretos (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:74`–`:88`); P13 aceita a localização (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:117`). Contra-mutante concreto, validado somente em memória: substituir fio:843 por `setattr(p, 'data_evento', data_evento)` mantém `violacoes_do_escritor=[]`; nenhum arquivo foi alterado [nao verificado: registro persistido da execução].
- Os cinco mutantes oficiais mordem nos analisadores AST (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:138`–`:154`); extraí e executei as funções puras via `python3 -B`, sem importar Django nem acessar banco. Resultado da execução sem artefato persistido: [nao verificado]. A limitação do mutante alternativo decorre diretamente de `:74`–`:88`.

## 08/11 11:28 · sessao d5d52281b203 · - · - · origem=Ronald (tmux)

**Item 6 — PROCEDENTE: os leitores continuam atribuindo significado de turno ao valor gravado, embora o reuso deixe o acervo com outra data.** A própria prova da volta espera `data_evento=D0` depois de costurar a linha no chamado de `DIA`; o reuso atualiza célula/destino, sem atualizar `data_evento`. `app/chamados/tests/test_e1_v15.py:90-99`; `app/chamados/models.py:2797-2813`; `app/chamados/models.py:2836`.

| Campo/valor | Quem escreve nos caminhos examinados | Quem lê e para quê |
|---|---|---|
| `data_evento` | `_nascer`: `_dia_fato` obtido em `dia_do_turno_da_linha`, depois `create`; `reapontar_resolvedora`: data pergunta sem dia pelo mesmo wrapper; `rechavear_data_evento`: grava parâmetro `novo_dia`. `app/chamados/models.py:2749-2751`; `app/chamados/models.py:2840`; `app/chamados/services/fio.py:842-844`; `app/chamados/services/fio.py:962-968`. | Costura: wrapper/juiz; fecho: `_dia_do_fato`; identidade de busca: valor gravado; C6/tranca/plantio: valor gravado. `app/chamados/juizes.py:1993-1994`; `app/chamados/services/fio.py:1217-1243`; `app/chamados/services/fio.py:1196`; `app/chamados/juizes.py:332-336`; `app/chamados/services/materializacao.py:344-345`. |
| `celula` | `_nascer` busca por `_dia_fato`, atribui ao existente ou cria com ela; rechaveamento busca célula de `novo_dia`. `app/chamados/models.py:2768`; `app/chamados/models.py:2797-2799`; `app/chamados/models.py:2841`; `app/chamados/services/fio.py:959-963`. | C6 lê veredito da célula; alimentação de falta exige que `celula.data == data_evento`, descartando a FK divergente; chamado deriva seu dia pela célula antes dos outros degraus. `app/chamados/juizes.py:1166-1168`; `app/chamados/juizes.py:1199-1214`; `app/chamados/juizes.py:634-642`; `app/chamados/juizes.py:98-99`. |
| `batida_origem_id` | `_nascer` recebe campo explícito ou deriva pelo mesmo `batida_origem_do_nascimento` utilizado no `save` de inserção; deriva de `disputa.contexto.ultima_batida.id`, validando dono quando a derivação ocorre. `app/chamados/models.py:2745-2747`; `app/chamados/models.py:3270-3271`; `app/chamados/models.py:3282-3289`. | Wrapper decide se consulta juiz; `_dia_do_fato` carrega Batida e consulta `_T1`; busca/dedup usa episódio. `app/chamados/services/fio.py:1259-1262`; `app/chamados/services/fio.py:1232-1239`; `app/chamados/models.py:2781`; `app/chamados/models.py:2680`. |
| `chamado_destino` | Nascimento cria com destino; reuso atribui após guardas; reapontamento atribui e salva com lavra. `app/chamados/models.py:2841`; `app/chamados/models.py:2800-2813`; `app/chamados/services/fio.py:859-874`. | Costura compara dia; `chamado_que_lista` escolhe destino antes do pai; plantio usa destino para marcos; guarda terminal julga C6 no pós estado. `app/chamados/juizes.py:1994-2029`; `app/chamados/services/fio.py:116-117`; `app/chamados/services/materializacao.py:339`; `app/chamados/services/materializacao.py:409`; `app/chamados/services/fio.py:778-783`. |

Contraexemplo de leitura mantido pelo código: linha com episódio cujo juiz diz `DIA`, gravada em `D0`; `abrir(..., DIA)` elege a linha, associa célula de `DIA`, aceita costura pelo juiz e preserva `D0`. Então:

- A costura compara `DIA`, enquanto `_dia_de_turno_da_pergunta` devolve `D0`. `app/chamados/models.py:2749-2751`; `app/chamados/models.py:2768`; `app/chamados/models.py:2802`; `app/chamados/juizes.py:1994`; `app/chamados/juizes.py:336`.
- A tranca calcula competência por `D0`. `app/chamados/juizes.py:948-961`.
- C6, quando alcança falta do dia, consulta `D0`; a célula de `DIA` não serve para alimentar essa consulta porque a igualdade falha. `app/chamados/juizes.py:1216-1222`; `app/chamados/juizes.py:638`.
- Plantio tipo hora combina resposta com `D0`, depois faz ajuste overnight a partir de `D0`. Não consulta o novo wrapper nesse caminho. `app/chamados/services/materializacao.py:344-345`; `app/chamados/services/materializacao.py:414-421`.
- A guarda de costura em chamado terminal decide competência entregue por `p.data_evento`, portanto também pelo gravado. `app/chamados/services/fio.py:776`.

Isso demonstra discrepância sem exigir migração de leitores, cuja preservação está expressamente no escopo dado; comprovar que a migração já eliminou o estado em produção é **[nao verificado]**. `app/chamados/tests/test_e1_v15.py:98-99`.

**Item 7 — classes verificadas.**

- **Campo com dois significados: PROCEDENTE.** O código declara expressamente data gravada como chave e `_dia_do_fato` como identidade distinta; o reuso conserva divergência observável entre leitores acima. `app/chamados/services/fio.py:1220-1227`; `app/chamados/tests/test_e1_v15.py:98-99`.
- **Segundo juiz geométrico duplicando `_T1`: não demonstrado.** O registro aponta `turnos_do_colab`; o wrapper chama `_dia_do_fato` → `dia_do_turno_da_batida` → `dia_das_batidas` → `turnos_do_colab`. Não há outro pareador nesse trajeto. `app/core/juizes.py:196`; `app/chamados/services/fio.py:1236-1238`; `app/ponto/turnos.py:2923-2926`; `app/ponto/turnos.py:2902-2903`.
- **Derivação paralela de fonte: PROCEDENTE no fallback sem resposta, não na geometria.** O juiz documenta `None` como “sem resposta do juiz não há dia a gravar”, mas `dia_do_turno_da_linha` devolve pedido/gravado nessa situação. `app/ponto/turnos.py:2919-2920`; `app/chamados/services/fio.py:1258-1262`.
- **Derivação duplicada de episódio: não demonstrada.** Porta e inserção chamam a mesma função. `app/chamados/models.py:2745`; `app/chamados/models.py:3271`.
- **Relógio fora da borda: não demonstrado nas linhas alteradas.** O relógio identificado é a porta própria de conversa, que captura `localdate()`; conversa é explicitamente civil e excluída de confronto de turno. `app/chamados/models.py:2700-2707`; `app/chamados/juizes.py:1991-1992`. Auditoria transitiva integral de relógios: **[nao verificado]**.
- **Ordem de trava nova incorreta: não demonstrada nas linhas alteradas.** `_nascer` toma Colaborador antes das novas consultas e da costura; reapontamento toma Colaborador por `fato_livre_para` ou diretamente antes da guarda que trava Chamado. `app/chamados/models.py:2720-2724`; `app/chamados/services/fio.py:852-861`; `app/chamados/services/fio.py:768`. Auditoria de todos os chamadores/transações externos: **[nao verificado]**.
- Segundo escritor fora da porta e alcance/mordida do selo pertencem ao censo do outro explorer: **[nao verificado]**.

## 08/11 11:23 · sessao c343206cd790 · FIO-E0-V4-E1 · censo · origem=Code

**As classes propostas não cobrem três dos cinco achados:** o Codex viu, e a triagem confirmou sem curar. Chamá-los de **(b)** falsearia o registro, porque a triagem manteve 3a/3b como bloqueadores. — `triagem_e1_v14b.txt:3,5-6`

Numeração abaixo: primeira linha após cada marcador `== anexo: ... ==` = linha 1, contando linhas vazias.

1. **3a — segunda chamada idêntica → `FatoEmOutroDia`: sem classe aplicável entre (a–e).** Foi visto e mantido como bloqueador. — `codex_e1_v14b.r:39`; `triagem_e1_v14b.txt:5`

   **Pergunta:** “o comportamento novo (FatoEmOutroDia ou reaproveitar a linha de outra data gravada e lhe dar a celula de data_evento) esta certo pela lei do aval?” — `codex_e1_v14b.q:8`

   **Resposta:** “Portanto `abrir(D0, batida cujo juiz diz DIA)` pode criar em D0; uma chamada direta posterior igual pode recusar a própria linha pelo juiz.” — `codex_e1_v14b.r:39`

   **Triagem:** “NAO curado: PERGUNTA DE DESENHO (duas curas, ver relato). Bloqueia pouso.” **Motivo escrito de descarte: nao ha trecho.** — `triagem_e1_v14b.txt:5` [nao verificado]

2. **3b — `_resolvedora_valida` compara a data gravada: sem classe aplicável entre (a–e).** Foi visto e mantido como bloqueador. — `codex_e1_v14b.r:43`; `triagem_e1_v14b.txt:6`

   **Pergunta:** “Todos os escritores e leitores supoem o MESMO significado (dia civil gravado x dia do turno x instante)?” — `codex_e1_v14b.q:6`

   **Resposta:** “A segunda validação lê essa data antiga e lança `ResolvedoraCruzada`, pois a diferença supera um dia.” O mesmo parágrafo cita `app/chamados/models.py:2786` e `app/chamados/juizes.py:1988`. — `codex_e1_v14b.r:43`

   **Triagem:** “PROCEDENTE, provado. NAO curado: mesma PERGUNTA DE DESENHO. Bloqueia pouso.” **Motivo escrito de descarte: nao ha trecho.** — `triagem_e1_v14b.txt:6` [nao verificado]

3. **`data_evento` com dois significados: sem classe aplicável entre (a–e).** Foi visto e confirmado como causa raiz, sem cura naquela volta. — `codex_e1_v14b.r:15,91`; `triagem_e1_v14b.txt:3`

   **Pergunta:** “campo com dois significados (data_evento: chave de busca x identidade -- esta declarado e respeitado em todo leitor?)” — `codex_e1_v14b.q:12`

   **Resposta:** “**Campo com dois significados não reconciliados — procedente, gravidade alta:** a identidade nova usa o juiz, enquanto destino, C6 e plantio continuam usando a data gravada como dia do fato.” — `codex_e1_v14b.r:91`

   **Triagem:** “PROCEDENTE como causa raiz de 3a/3b; vai junto da PERGUNTA DE DESENHO (nao curado nesta volta).” **Motivo escrito de descarte: nao ha trecho.** — `triagem_e1_v14b.txt:3`

4. **Outro componente fica fora sem recusa: (c), fora do alcance da árvore perguntada.** A pergunta identifica `5e55c211`; a eleição de um único componente aparece na triagem como cura da volta 14b. O laudo posterior aponta o residual dessa cura. — `codex_e1_v14b.q:1`; `triagem_e1_v14b.txt:4`; `opus_final2_e1_6695e972.md:8`

   **Pergunta relacionada:** “e o universo que a porta (universo_do_fato) elege e exatamente o universo_da_linha de seus membros?” — `codex_e1_v14b.q:7`

   **Resposta sobre a implementação anterior:** “o fecho preserva todas as sementes, sem exigir conectividade entre elas.” — `codex_e1_v14b.r:29`

   **Registro da mudança:** “CURADO na volta 14b: fio.py::universo_do_fato elege UM componente (o da canonica_do_grupo das sementes)”. — `triagem_e1_v14b.txt:4`

   **Pergunta ou resposta sobre o residual específico da nova escolha — componente omitido sem recusa: nao ha trecho.** Esse residual consta do laudo posterior: “o outro nao entra em recusadas nem fecho_cruza_dias, nenhum contador o ve”. — `opus_final2_e1_6695e972.md:8`

5. **Custo dos juízes: (b), visto e descartado como bloqueio, mantido em fila.** — `codex_e1_v14b.r:59,66,72`; `triagem_e1_v14b.txt:9`

   **Pergunta:** “quantas chamadas ao juiz por abrir, por fecho, por fecho_cruza_dias? Memo por batida basta?” — `codex_e1_v14b.q:10`

   **Resposta:** “Zero com busca vazia; caso contrário, B no universo/ponte/fecho compartilhado.” Também: “**Memo por batida não basta para compartilhar trabalho por turno/janela**”. — `codex_e1_v14b.r:66,72`

   **Motivo escrito na triagem:** “descartado como bloqueio -- nenhum selo de teto vermelho na suite de 14300; fila (medir na frota).” A medição na frota permanece **[nao verificado]**. — `triagem_e1_v14b.txt:9` [nao verificado]

## 08/11 11:04 · sessao b5a11839aaae · PERGUNTA-SEM-SELO · censo · origem=Code

**A medida antiga precisa de correção:** S1 recebe hoje a contagem como parâmetro; S3 mantém consulta crua para idempotência, mas o julgamento já verifica a retratação no retorno. Permanecem consultas cruas em S2, S4, S5 e S6. (`app/ponto/supra_juiz.py:207`; `app/chamados/juizes.py:235`; `app/chamados/juizes.py:775`; `app/ponto/services/veredito_celula.py:215`; `app/chamados/services/materializacao.py:94`; `app/ponto/services/flip_auto.py:35`; `app/ponto/utils.py:244`.)

**1. S1–S6 nesta cópia**

| Sítio | Universo atual e consequência |
|---|---|
| **S1 — `classificar_dia`** | Não monta universo: recebe `batidas_no_dia` e usa essa contagem no fallback. O comando `supra_juiz` carrega cru, conta e entrega esse insumo; `emitir_furo_retroativo` faz o mesmo. O cartório usa células da grade e órfãs. (`app/ponto/supra_juiz.py:207`; `app/ponto/supra_juiz.py:513`; `app/ponto/management/commands/supra_juiz.py:113`; `app/ponto/management/commands/supra_juiz.py:222`; `app/ponto/management/commands/supra_juiz.py:229`; `app/ponto/management/commands/emitir_furo_retroativo.py:53`; `app/ponto/management/commands/emitir_furo_retroativo.py:100`; `app/ponto/services/cartorio.py:1373`; `app/ponto/services/cartorio.py:1419`.) |
| **S2 — `veredito_celula`** | **Cru-JULGAMENTO.** `reais` entra em `grade_dia` e decide se o dia já está completo. `grade_dia` encaminha a lista recebida à grade. (`app/ponto/services/veredito_celula.py:215`; `app/ponto/services/veredito_celula.py:220`; `app/ponto/services/veredito_celula.py:223`; `app/ponto/services/simular_dia.py:5`.) |
| **S3 — `_batida_existente_na_janela`** | **Consulta crua de idempotência.** O julgamento `_pergunta_materializada_de_fato` já exige `retratada_em is None` no retorno. A justificativa para preservar o cru no plantio está escrita: impedir replantio da batida retratada. A cura é local ao julgamento, não ao helper inteiro. (`app/chamados/juizes.py:227`; `app/chamados/juizes.py:235`; `app/chamados/juizes.py:768`; `app/chamados/juizes.py:774`; `app/chamados/juizes.py:775`.) |
| **S4 — `_vizinha_mesmo_tipo`** | O código correspondente está em **`chamados/services/materializacao.py`**: duas consultas cruas decidem bloqueio de materialização por paridade. (`app/chamados/services/materializacao.py:85`; `app/chamados/services/materializacao.py:94`; `app/chamados/services/materializacao.py:100`; `app/chamados/services/materializacao.py:451`; `app/chamados/services/materializacao.py:850`.) |
| **S5 — `pares_mesmo_tipo`** | **Cru-JULGAMENTO.** A consulta crua define adjacência e pares que alimentam a fila de flip. A consulta posterior a `batidas_do_dia_turno` não altera esse universo inicial. (`app/ponto/services/flip_auto.py:35`; `app/ponto/services/flip_auto.py:41`; `app/ponto/services/flip_auto.py:104`; `app/ponto/services/flip_auto.py:146`.) |
| **S6 — `proxima_batida_de`** | **Cru-JULGAMENTO na função:** última marca crua determina o próximo tipo. A autoridade registrada para próximo tipo é `turnos.proximo_tipo_de`. Chamadores dinâmicos de `proxima_batida_de`: **[não verificado]**. (`app/ponto/utils.py:240`; `app/ponto/utils.py:244`; `app/ponto/utils.py:246`; `app/core/juizes.py:197`.) |

Commit de cura: **[não verificado]**, pois esta cópia não oferece histórico Git consultável. A prova atual de cura do julgamento de S3 é `app/chamados/juizes.py:775`.

Há outro uso de S3 que merece separação: a apresentação declara `ja_satisfeita` verificando somente `aprovada`; o plantio reutiliza qualquer existente. Isso não equivale à verificação explícita de retratação feita pelo juiz. (`app/chamados/services/materializacao.py:446`; `app/chamados/services/materializacao.py:447`; `app/chamados/services/materializacao.py:834`; `app/chamados/services/materializacao.py:840`; `app/chamados/juizes.py:775`.)

**2. Censo AST**

Unidade do inventário: uma raiz de consulta persistente ou uma chamada à porta; encadeamentos contam uma vez. São **154 raízes de consulta + 47 chamadas à porta = 201 sítios**, discriminados abaixo. A implementação da própria porta entra em **(a)**. Os três acessos por manager relacionado estão em `app/chamados/juizes.py:720`, `app/chamados/services/vigia_carimbo.py:113` e `app/ponto/management/commands/reconciliar_grade.py:118`.

| Classe | Total do inventário abaixo |
|---|---:|
| **a — porta**, incluindo sua implementação | **48** |
| **b — `retratada_em` explícito**, inclusive verificação em memória | **51** |
| **c — cru-exibição** de marcação, histórico ou indicadores da marcação | **22** |
| **d — cru-JULGAMENTO** | **28** |
| **e — cru-lookup**, identidade ou idempotência de escrita | **27** |
| **f — indeterminado na taxonomia**, com motivo indicado | **25** |
| **Total** | **201** |

Ocorrências executáveis de `.raw()` ou SQL direto de `ponto_batida` nesse recorte: **nenhuma encontrada pela varredura**; SQL construído dinamicamente e acessos externos à cópia: **[não verificado]**.

**(a) Porta — 48 sítios**

```text
app/api/views.py:2026 api_justificativas
app/chamados/juizes.py:1462 via_pelo_fato
app/chamados/services/validacao.py:828 classe_do_lote
app/colaboradores/services/detalhe.py:49 contexto_detalhe
app/core/services/painel_op.py:83 montar_painel_operacional
app/core/services/painel_op.py:269 montar_painel_operacional
app/core/views.py:114 _batidas_hoje
app/escala/detector_proposta.py:56 medir_padrao
app/escala/regua_defesa.py:153 diagnostico_regua
app/escala/regua_defesa.py:520 entradas_fora_do_inicio
app/escala/services/fase_na_tela.py:47 _trabalhados
app/escala/services/previsto_cego.py:159 medir
app/escala/services/semana_do_posto.py:117 _bateram
app/escala/utils.py:1505 montar_grade_prevista_periodo_por_turno
app/folha/export.py:499 dias_ferias_com_batida
app/folha/export.py:906 classificar_export
app/folha/previa.py:58 montar_previa
app/pautas/services.py:420 contexto_do_dia_texto
app/ponto/management/commands/aplicar_09_corte_b.py:126 _tem_saida_real_em_turno_aberto
app/ponto/management/commands/censo_col900.py:128 handle
app/ponto/management/commands/diff_espelho_alimentacao.py:92 handle
app/ponto/management/commands/e6_oraculo.py:152 __init__
app/ponto/management/commands/e6_oraculo.py:212 batida_fora_de_turno
app/ponto/management/commands/mapa_divergencia.py:96 handle
app/ponto/management/commands/reconciliar_fantasmas.py:96 _completude
app/ponto/management/commands/tripwire_celulas.py:283 medir_sobra_com_tipo_trocado
app/ponto/management/commands/vigiar_motor_autoridade.py:54 handle
app/ponto/services/abriu_nao_bateu.py:174 _bateu_no_dia
app/ponto/services/cartorio.py:1123 _julgar_colab_corpo
app/ponto/services/cobertura.py:299 declarar_cobertura_passada
app/ponto/services/deslogados.py:172 apurar
app/ponto/services/dia_do_colab.py:60 dia_do_colab
app/ponto/services/diagnostico_escala.py:142 diagnostico_escala
app/ponto/services/dias_em_aberto.py:122 passivo_da_competencia
app/ponto/services/esmeril_espelho.py:156 assinaturas_do_colab
app/ponto/services/espelho.py:1022 autoridade_do_periodo
app/ponto/services/faltas_de_hoje.py:57 faltas_de_hoje_sem_cobranca
app/ponto/services/fechamento.py:633 recalcular_fechamento_mes
app/ponto/services/fechamento.py:811 recalcular_fechamento_mes
app/ponto/services/triagem_batida.py:175 triagem_bater_ponto
app/ponto/services/vigia_de_hora.py:76 _acesas_de_sobra
app/ponto/turnos.py:2864 batidas_apuraveis — implementação
app/ponto/turnos.py:2962 _ultima_de
app/ponto/turnos.py:2982 turnos
app/ponto/utils_turno_orfao.py:28 detectar_bloqueio_turno_orfao
app/relatorios/management/commands/diff_extrato_parcial.py:37 _antes
app/relatorios/pdf_espelho.py:272 _coletar_dados_espelho
app/relatorios/views.py:502 extrato_parcial
```

**(b) Retratação explícita — 51 sítios**

```text
app/api/views.py:703 api_bater_ponto
app/api/views_core.py:700 api_bater_ponto
app/chamados/juizes.py:720 _pergunta_materializada_de_fato
app/chamados/management/commands/carimbar_por_batida_coincidente.py:70 handle
app/chamados/reconciliador.py:206 _par_integro — rejeição em memória na linha 216
app/chamados/services/adesao.py:60 sincronizar_adesao
app/chamados/services/adesao.py:144 sincronizar_adesao
app/chamados/services/desvio_escala.py:60 medir_desvio
app/chamados/services/fila_por_causa.py:190 particionar
app/chamados/services/limbo_folgas.py:35 sugerir_folga
app/chamados/services/limbo_folgas.py:155 semana_muda
app/chamados/services/materializacao.py:616 perguntas_absorvidas_por
app/chamados/services/vigia_carimbo.py:113 classificar
app/chamados/veredito_lavrado.py:140 marcos_do_lote
app/colaboradores/management/commands/corrigir_escala_retroativa.py:94 _rodar
app/colaboradores/services/calendario.py:239 contexto_calendario
app/colaboradores/services/ficha.py:145 ficha_do_colab
app/colaboradores/services/pos_vinculo.py:38 fechar_adesao_com_batida
app/colaboradores/services/senha_provisoria.py:24 senhas_provisorias
app/core/services/painel_op.py:92 montar_painel_operacional
app/core/views.py:173 dashboard
app/core/views.py:371 dashboard_supervisor
app/core/views.py:775 painel_tv
app/core/views.py:1177 dashboard_dados
app/escala/services/meio_periodo.py:158 observado_meio_periodo
app/escala/services/propositor.py:477 propostas
app/ferias/situacao.py:277 situacao_de_ferias
app/ponto/management/commands/diff_calculador.py:270 handle
app/ponto/management/commands/diff_janela_he.py:93 handle
app/ponto/management/commands/e6_piso_autopsia.py:71 handle
app/ponto/management/commands/marcar_foto_ausente_retro.py:66 handle
app/ponto/registro_batida.py:107 registrar_batida
app/ponto/registro_batida.py:139 registrar_batida
app/ponto/services/adesao.py:34 primeira_batida
app/ponto/services/adesao.py:58 ids_com_adesao
app/ponto/services/cartorio.py:1154 _julgar_colab_corpo
app/ponto/services/deslogados.py:161 apurar
app/ponto/services/deslogados.py:174 apurar
app/ponto/services/faltas_de_hoje.py:55 faltas_de_hoje_sem_cobranca
app/ponto/services/flip_auto.py:226 etiquetas_pela_celula — exclusão em memória na linha 229
app/ponto/services/lastro.py:261 julgar
app/ponto/services/passivo_s84.py:53 vivas
app/ponto/services/piso_intervalo.py:39 pares_abaixo_do_piso
app/ponto/turnos.py:1599 papel_por_minuto_da_ata
app/ponto/turnos.py:1861 turnos_do_colab
app/ponto/turnos.py:1899 _ultima_bat
app/ponto/turnos.py:2217 _turno_aberto_calc
app/ponto/turnos.py:2350 turnos_abertos_de
app/ponto/turnos.py:2382 sequencia_relogio_x_jornada
app/ponto/turnos.py:2466 proximo_tipo_de
app/ponto/turnos.py:2903 dia_das_batidas
```

**Duas ressalvas sobre (b):** calendário seleciona explicitamente as **retratadas para exibição**; `marcos_do_lote` seleciona o instante de retratação para invalidar lavra. Portanto, “tem `retratada_em` explícito” não significa automaticamente “consulta de apuráveis”. (`app/colaboradores/services/calendario.py:239`; `app/colaboradores/services/calendario.py:619`; `app/chamados/veredito_lavrado.py:140`.)

**(c) Cru-exibição — 22 sítios**

```text
app/api/views.py:297 api_me
app/api/views_core.py:286 api_me
app/chamados/services/acoes_chamado.py:377 montar_contexto_detalhe
app/chamados/views.py:692 fio_colaborador
app/colaboradores/painel_cell.py:33 montar_linha_colab
app/colaboradores/services/detalhe.py:20 contexto_detalhe
app/colaboradores/services/drawer.py:34 contexto_drawer
app/colaboradores/services/situacional.py:137 montar_situacional
app/colaboradores/views.py:53 vincular_colaboradores — indicador de marcação com GPS
app/colaboradores/views.py:337 vincular_posto — indicador tem_geo
app/colaboradores/views.py:356 vincular_posto — indicador tem_geo
app/colaboradores/views.py:395 _render_row — indicador tem_geo
app/colaboradores/views.py:928 painel_colaborador — total de registros
app/colaboradores/views.py:931 painel_colaborador — registros do mês
app/core/cell.py:10 cell_ultimas_batidas
app/core/views.py:194 dashboard — últimas marcações
app/core/views.py:219 dashboard — total de registros
app/core/views.py:991 batida_detalhe_json — histórico
app/core/views.py:1163 dashboard_dados — feed de marcações
app/ponto/views.py:191 espelho_ponto — últimas seis
app/ponto/views.py:441 batidas_offline_pendentes — fila de marcações
app/relatorios/services.py:230 gerar_pdf_espelho_lote_bytes — informação de movimento
```

O drawer calcula presença usando turnos, separadamente da lista crua exibida; o situacional também separa a última marca exibida do julgamento de uso/adesão. (`app/colaboradores/services/drawer.py:34`; `app/colaboradores/services/drawer.py:45`; `app/colaboradores/services/drawer.py:58`; `app/colaboradores/services/situacional.py:146`; `app/colaboradores/services/situacional.py:150`.)

**(d) Cru-JULGAMENTO — 28 sítios**

| Arquivo:linha | Função | O que o resultado decide |
|---|---|---|
| `app/chamados/management/commands/auditar_disputas.py:103` | `handle` | Qualificação de tipo invertido no relatório. |
| `app/chamados/services/explicador.py:40` | `_explicar_validada` | Existência/tipo da prova de materialização. |
| `app/chamados/services/explicador.py:53` | `_explicar_validada` | Diagnóstico e encaminhamento para cadastro. |
| `app/chamados/services/explicador.py:132` | `por_que_nao_fecha` | Alimentação desses diagnósticos em lote. |
| `app/chamados/services/materializacao.py:94` | `_vizinha_mesmo_tipo` | Vizinha anterior que barra plantio. |
| `app/chamados/services/materializacao.py:100` | `_vizinha_mesmo_tipo` | Vizinha posterior que barra plantio. |
| `app/core/views.py:360` | `dashboard_supervisor` | Entrada usada para excluir alguém dos ausentes. |
| `app/core/views.py:793` | `painel_tv` | Atividade que entra na classificação de cobertura. |
| `app/escala/services/furos_diarios.py:261` | `universo_empresa` | Nunca bateu/sumiu e última atividade. |
| `app/escala/services/furos_diarios.py:267` | `universo_empresa` | Escala versus realidade no período. |
| `app/ponto/management/commands/detectar_par_relampago.py:205` | `_alvo_retratavel` | Guardas e escolha da retratação. |
| `app/ponto/management/commands/detectar_par_relampago.py:290` | `varrer_pares_embutidos` | Adjacência e pares candidatos. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:53` | `handle` | Universo contado para classificar dia. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:74` | `handle` | Piso de adesão usado no julgamento. |
| `app/ponto/management/commands/reconciliar_grade.py:84` | `handle` | Seleção do universo de colaboradores. |
| `app/ponto/management/commands/reconciliar_grade.py:88` | `handle` | Piso de adesão que suprime dias. |
| `app/ponto/management/commands/reconciliar_grade.py:118` | `handle` | `c.batidas.all()` alimenta grade/missing. |
| `app/ponto/management/commands/supra_juiz.py:113` | `handle` | Universo contado para classificar dia. |
| `app/ponto/management/commands/supra_juiz.py:132` | `handle` | Adesão entregue ao classificador. |
| `app/ponto/propositor.py:346` | `dia_posicional_completo` | Completude posicional do dia. |
| `app/ponto/services/flip_auto.py:35` | `pares_mesmo_tipo` | Pares que alimentam fila de flip. |
| `app/ponto/services/flip_auto.py:291` | `avaliar_fila` | Dia perfeito antes/depois do flip. |
| `app/ponto/services/geofence.py:51` | `vinculo_contagem` | Evidência que julga divergência de vínculo. |
| `app/ponto/services/veredito_celula.py:215` | `veredito_celula` | Completude, flips e materialização. |
| `app/ponto/services/vinculo_na_lista.py:49` | `achar` | Classificação de evidência de vínculo/posto. |
| `app/ponto/utils.py:186` | `colabs_disponiveis_para_cobertura` | Exclusão de candidatos por suposto turno aberto. |
| `app/ponto/utils.py:244` | `proxima_batida_de` | Próximo tipo de batida. |
| `app/ponto/views.py:1598` | `_abrir_justificativa_fluxo` | Entrada vinculada à justificativa. |

Em `avaliar_fila`, excluir retratadas somente da lista de flips **não protege o julgamento**: `reais` continua inteira em `grade_dia`. (`app/ponto/services/flip_auto.py:291`; `app/ponto/services/flip_auto.py:298`; `app/ponto/services/flip_auto.py:303`.)

Os detectores de par filtram `aprovada=True`, mas isso não é o filtro legal de retratação: o modelo define `aprovada` como crivo do DP. Efeito concreto disso nos registros de produção: **[não verificado]**. (`app/ponto/management/commands/detectar_par_relampago.py:205`; `app/ponto/management/commands/detectar_par_relampago.py:290`; `app/ponto/models.py:66`.)

**(e) Cru-lookup/identidade/idempotência — 27 sítios**

```text
app/api/views.py:559 api_bater_ponto — idempotência
app/api/views.py:856 api_upload_foto — pk
app/api/views_core.py:551 api_bater_ponto — idempotência
app/api/views_core.py:846 api_upload_foto — pk
app/chamados/detector_anomalias.py:43 detectar_anomalias — pk
app/chamados/juizes.py:49 _dia_da_batida — pk
app/chamados/juizes.py:194 dias_do_fato — pk__in
app/chamados/juizes.py:235 _batida_existente_na_janela — idempotência compartilhada
app/chamados/services/acoes_chamado.py:262 acao_recusar_geofence — pk
app/chamados/services/passivo_validado.py:513 _plantar_o_dia — marcador de pk anterior à escrita
app/chamados/services/passivo_validado.py:527 _plantar_o_dia — identificação do que a escrita criou
app/chamados/services/regularizacao_ext.py:73 _cria_batida — chave colaborador/instante
app/chamados/views_regularizacao.py:109 regularizacao_add_item — pk
app/core/views.py:978 batida_detalhe_json — pk
app/ponto/management/commands/diff_passivo_s84.py:79 _mutacao — pk
app/ponto/management/commands/flip_automatico.py:44 handle — pk__in
app/ponto/management/commands/passivo_disputa_retro.py:158 handle — pk
app/ponto/registro_batida.py:120 registrar_batida — chave colaborador/instante
app/ponto/services/flip_auto.py:135 fila_flip_recusas — pk
app/ponto/services/flip_auto.py:272 avaliar_fila — pk
app/ponto/services/geofence.py:406 reavaliar_chamados_geofence — pk__in
app/ponto/services/regularizacao.py:52 regularizar_turno_orfao — pk
app/ponto/services/triagem_batida.py:257 ramo_regularizar_saida — pk
app/ponto/services/triagem_batida.py:670 ramo_registrar_es — idempotência
app/ponto/services/triagem_batida.py:722 ramo_registrar_es — identificação da oposta na resposta
app/ponto/views.py:456 batidas_offline_pendentes — pk
app/ponto/views.py:2899 salvar_foto_batida — id
```

O cru por chave exata em `registrar_batida` é deliberado: reenviar a mesma chave deve recuperar inclusive a marca retratada, evitando colisão com a constraint. (`app/ponto/registro_batida.py:116`; `app/ponto/registro_batida.py:120`.)

**(f) Indeterminado na taxonomia — 25 sítios, com motivo**

Aqui “indeterminado” significa que a leitura não cabe integralmente em exibição de marcação, julgamento, porta ou lookup de escrita; o uso observado está declarado em cada linha.

| Arquivo:linha | Função | Motivo |
|---|---|---|
| `app/api/hora_do_aparelho.py:173` | `descartadas` | Auditoria da diferença entre relógio do aparelho e timestamp gravado. |
| `app/chamados/veredito_lavrado.py:137` | `marcos_do_lote` | Carimbo de criação para invalidar lavra; não conta trabalho. |
| `app/core/media_protegida.py:31` | `servir_media_protegida` | Autorização de acesso à foto. |
| `app/core/views_config.py:528` | `saude_sistema` | Contagem física de saúde do sistema. |
| `app/escala/management/commands/hub_smoke.py:98` | `win_map` | Foto antes/depois para detectar mutação durante smoke. |
| `app/ponto/management/commands/censo_col900.py:133` | `handle` | Diagnóstico de existência crua quando a porta não encontrou marca. |
| `app/ponto/management/commands/checklist_golive.py:100` | `handle` | Contagem física global. |
| `app/ponto/management/commands/checklist_golive.py:101` | `handle` | Contagem para alerta sobre dados de teste. |
| `app/ponto/management/commands/gerar_batidas_smoke_motor.py:61` | `_limpar` | Seleção para limpeza de ensaio. |
| `app/ponto/management/commands/mapa_divergencia.py:62` | `handle` | Seleção ampla de candidatos; comparação carrega pela porta. |
| `app/ponto/management/commands/processar_alertas_turno.py:59` | `handle` | Seleção para disparar `recompute_turnos`; não entreg

[truncado em 20000 de 32584 caracteres]

## 08/11 11:04 · sessao 92c5caf6b911 · COBPARC-AUSENCIA-CALA-DOIS-JUIZES · sonda · origem=Code

1. Contagens por classe, competência e empresa:

| Classe | Competência | Empresa | Contagem | Entregue K6 | Trancada K4 | Referência |
|---|---|---:|---:|---|---|---|
| carimbo_sem_periodo | 10/2026 | 2 | 14 | False | False | [nao verificado] |
| carimbo_sem_periodo | 11/2026 | 2 | 4 | False | False | [nao verificado] |
| carimbo_sem_periodo | 10/2026 | 4 | 2 | False | False | [nao verificado] |
| carimbo_sem_periodo | 11/2026 | 4 | 1 | False | False | [nao verificado] |
| sem_carimbo | 08/2026 | 2 | 18 | True | True | [nao verificado] |
| sem_carimbo | 09/2026 | 2 | 27 | True | False | [nao verificado] |
| sem_carimbo | 08/2026 | 3 | 1 | False | True | [nao verificado] |
| sem_carimbo | 09/2026 | 3 | 5 | True | False | [nao verificado] |
| sem_carimbo | 08/2026 | 4 | 5 | False | True | [nao verificado] |
| sem_carimbo | 09/2026 | 4 | 5 | True | False | [nao verificado] |
| sem_celula | 07/2026 | 2 | 1 | False | False | [nao verificado] |

2. Colab-dias por classe, considerando “aberta” como `entregue=False` em K6. [nao verificado]

| Classe | Entregue/exportada | Aberta | Total | Referência |
|---|---:|---:|---:|---|
| carimbo_sem_periodo | 0 | 21 | 21 | [nao verificado] |
| sem_carimbo | 55 | 6 | 61 | [nao verificado] |
| sem_celula | 0 | 1 | 1 | [nao verificado] |
| **Total** | **55** | **28** | **83** | [nao verificado] |

3. Nenhuma linha indicando erro ou traceback no trecho fornecido. [nao verificado]

As referências estão marcadas assim porque o dado foi fornecido no texto, sem arquivo:linha. [nao verificado]

## 08/11 10:58 · sessao ce0c1ff64080 · - · - · origem=Ronald (tmux)

Censo AST de `app/**/management/commands/*.py`: **47 sítios** — **a=10, b=6, c=0, d=10, e=3, f=18**. Unidade: chamada à porta ou raiz de consulta; encadeamentos contam uma vez. Foram resolvidos aliases importados (`_B`, `_bap`); `FotoBatida` excluída. As linhas abaixo constituem o inventário e sua evidência.

Prefixos usados na tabela:

- `P/` = `app/ponto/management/commands/`
- `C/` = `app/chamados/management/commands/`

| Classe | Arquivo:linha | Função | Motivo/contexto |
|---|---|---|---|
| a | `P/aplicar_09_corte_b.py:126` | `_tem_saida_real_em_turno_aberto` | `batidas_apuraveis` alimenta guarda de proteção de horas. |
| a | `P/censo_col900.py:128` | `handle` | Primeira batida pela porta. |
| a | `P/diff_espelho_alimentacao.py:92` | `handle` | Alimentação do motor pela porta. |
| a | `P/e6_oraculo.py:152` | `__init__` | Timestamps do oráculo pela porta. |
| a | `P/e6_oraculo.py:212` | `batida_fora_de_turno` | Existência de órfã confirmada na porta. |
| a | `P/mapa_divergencia.py:96` | `handle` | Batidas comparadas nas grades pela porta. |
| a | `P/reconciliar_fantasmas.py:96` | `_completude` | Universo carregado pela porta. |
| a | `P/tripwire_celulas.py:283` | `medir_sobra_com_tipo_trocado` | Evidência em torno do marco pela porta. |
| a | `P/vigiar_motor_autoridade.py:54` | `handle` | Alimentação pela porta. |
| a | `app/relatorios/management/commands/diff_extrato_parcial.py:37` | `_antes` | Alias `_bap` da porta. |
| b | `C/carimbar_por_batida_coincidente.py:70` | `handle` | `retratada_em__isnull=True`, aplicado na linha 73. |
| b | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:94` | `_rodar` | Filtro explícito no universo de dias batidos. |
| b | `P/diff_calculador.py:270` | `handle` | Filtro explícito. |
| b | `P/diff_janela_he.py:93` | `handle` | Filtro explícito. |
| b | `P/e6_piso_autopsia.py:71` | `handle` | Filtro explícito. |
| b | `P/marcar_foto_ausente_retro.py:66` | `handle` | Filtro explícito na linha 68. |
| d | `C/auditar_disputas.py:103` | `handle` | Universo cru usado para qualificar tipo invertido; decisão de inversão nas linhas 119–123. É relatório, sem abrir/fechar chamado (comentário linha 107). |
| d | `P/detectar_par_relampago.py:205` | `_alvo_retratavel` | Filtra `aprovada=True`, não retratação; lista decide guardas e escolha de retratação nas linhas 208–214. |
| d | `P/detectar_par_relampago.py:290` | `varrer_pares_embutidos` | Filtra `aprovada=True`, não retratação; adjacência e pares nas linhas 294–310. |
| d | `P/emitir_furo_retroativo.py:53` | `handle` | Lista crua gera `n_bat` na linha 100 e entra em `classificar_dia` nas linhas 101–106. |
| d | `P/emitir_furo_retroativo.py:74` | `handle` | Primeira crua decide `adesao`, enviada ao julgamento na linha 105. |
| d | `P/reconciliar_grade.py:84` | `handle` | Universo cru de colaboradores, utilizado na seleção da linha 106. |
| d | `P/reconciliar_grade.py:88` | `handle` | `Min(timestamp)` cru define piso de adesão; piso suprime dia nas linhas 136–138. |
| d | `P/reconciliar_grade.py:118` | `handle` | Related `c.batidas.all()` cru alimenta montagem da grade e detecção de missing nas linhas 151–155. |
| d | `P/supra_juiz.py:113` | `handle` | Lista crua contada na linha 222 e enviada a `classificar_dia` nas linhas 223–229. |
| d | `P/supra_juiz.py:132` | `handle` | Primeira crua decide adesão na linha 135 e julgamento na linha 227. |
| e | `P/diff_passivo_s84.py:79` | `handle._mutacao` | Lookup por `pk`, depois guarda de retratação na linha 80. |
| e | `P/flip_automatico.py:44` | `handle` | Alias `_B`, busca por `pk__in` de flips já propostos. |
| e | `P/passivo_disputa_retro.py:158` | `handle` | Lookup por `pk` do passivo já selecionado. |
| f | `app/escala/management/commands/hub_smoke.py:98` | `handle.win_map` | Mapa cru de antes/depois para detectar mutação e excluir instabilidade do smoke, linhas 112–131; não é exibição nem apuração. |
| f | `P/censo_col900.py:133` | `handle` | Contador diagnóstico “retratadas mudariam”, somente quando a porta não achou batida; motivo explícito nas linhas 129–135. |
| f | `P/checklist_golive.py:100` | `handle` | Contagem física global exibida na linha 103; não é marcação nem apuração. |
| f | `P/checklist_golive.py:101` | `handle` | Contagem de colaboradores de teste para alerta de limpeza, linhas 104–105. |
| f | `P/gerar_batidas_smoke_motor.py:61` | `_limpar` | Seleção por marcador para limpeza em banco de ensaio; não decide apuração. |
| f | `P/mapa_divergencia.py:62` | `handle` | Alias `_B`: seleção ampla de colaboradores para auditoria; batidas efetivamente comparadas vêm da porta na linha 96. |
| f | `P/processar_alertas_turno.py:59` | `handle` | Seleção ampla de colaboradores para disparar `recompute_turnos` nas linhas 65–67; não alimenta pareamento cru. Caso se adote “seleciona materialização” como suficiente para (d), mover este sítio a d. |
| f | `P/testar_pwa.py:169` | `t05_batida_entrada` | Limpeza preparatória de ensaio. |
| f | `P/testar_pwa.py:178` | `t05_batida_entrada` | Confirma persistência de POST no smoke. |
| f | `P/testar_pwa.py:194` | `t05_batida_entrada` | Confirma persistência após repetir POST. |
| f | `P/testar_pwa.py:199` | `t05_batida_entrada` | Verifica resultado de smoke. |
| f | `P/testar_pwa.py:213` | `t06_batida_saida` | Verifica persistência de saída no smoke. |
| f | `P/testar_pwa.py:226` | `t07_batida_duplicada` | Limpeza preparatória do teste de idempotência. |
| f | `P/testar_pwa.py:232` | `t07_batida_duplicada` | Limpeza final do ensaio. |
| f | `P/vigia_plantio_overnight.py:30` | `batidas_plantadas_no_dia_errado` | Seleciona colaboradores por cru aprovado; evidência julgada vem de `turnos_do_colab` e `t.batidas`, linhas 38–39. |
| f | `P/zerar_dados_operacionais.py:43` | `handle` | Contagem de objetos antes de limpeza. |
| f | `P/zerar_dados_operacionais.py:96` | `handle` | Seleção de todos os objetos para limpeza de ensaio. |
| f | `app/tenants/management/commands/cron_healthcheck.py:17` | `handle.check` | Contagem física para healthcheck. |

Para eliminar ambiguidade na consolidação: **f aqui significa “uso operacional/diagnóstico fora das outras cinco classes”, com motivo verificado, não desconhecimento do fluxo**. Se o critério do usuário “resultado decide materialização” incluir disparo amplo de recompute, `processar_alertas_turno.py:59` deve ser **d**, dando **d=11, f=17**, demais totais iguais.

O caminho de `reconciliar_grade.py:118` tem bifurcação pela configuração: `app/escala/utils.py:2107` decide builder por turno ou legacy; o wrapper repassa a lista crua nas linhas 2108–2111. Portanto não foi classificado como porta, ainda que um builder possa voltar a buscar outra fonte (`app/escala/utils.py:2105`).

Interseção com E1, limitada a este inventário: **`app/escala/management/commands/hub_smoke.py:98`** e **`app/ponto/management/commands/vigia_plantio_overnight.py:30`**; pertença à lista E1 vem do anexo do usuário [não verificado por arquivo local].

## 08/11 10:58 · sessao 87740fa7e361 · - · - · origem=Ronald (tmux)

S1..S6 no código atual:

| Sítio | Resultado e prova |
|---|---|
| S1 | `classificar_dia` é pura: recebe `batidas_no_dia`, sem construir universo (`app/ponto/supra_juiz.py:204`, `:207`). Usa a quantidade como fallback quando `dia` não tem `orfas` (`:513`, `:521`). O produtor do command segue **cru**: query `app/ponto/management/commands/supra_juiz.py:113`, contagem `:222`, passagem `:229`. Também há o produtor cru em `app/ponto/management/commands/emitir_furo_retroativo.py:53`, contagem `:100`, passagem `:106`. Já o cartório calcula sobre células da grade e órfãs (`app/ponto/services/cartorio.py:1373`), passando o resultado em `:1419`. Portanto a localização antiga não representa o sítio de leitura atual. |
| S2 | **Cru-JULGAMENTO**: `veredito_celula` carrega `Batida.objects.filter` sem marca em `app/ponto/services/veredito_celula.py:215`; passa as reais para `grade_dia` em `:220`; o resultado decide “nada a corrigir” em `:223`. `grade_dia` apenas encaminha as batidas à grade (`app/ponto/services/simular_dia.py:5`); a grade monta `locs` sem excluir retratação (`app/escala/utils.py:1344`). |
| S3 | `_batida_existente_na_janela` é **crua**, documentada como idempotência (`app/chamados/juizes.py:227`, query `:235`). O chamador de julgamento já exige a marca nula **no objeto retornado** (`:774`, `:775`): cura explícita local. A razão de manter a função crua para não replantar retratadas está escrita em `:768`. Materialização reutiliza a existente em `app/chamados/services/materializacao.py:834`, `:840`; apresentação também decide `ja_satisfeita` lendo apenas `aprovada`, sem checar retratação, em `:446`, `:447`. Não é lookup por chave, e sim por janela; separar seu uso de idempotência do uso de satisfação. |
| S4 | O caminho `app/ponto/services/materializacao.py` não existe nesta cópia [não verificado por arquivo:linha]. O código correspondente está em `app/chamados/services/materializacao.py:85`, `_vizinha_mesmo_tipo`, com duas queries **cruas**, `:94` e `:100`. Decide bloqueio/recusa de materialização em `:451`, `:452`, `:850`, `:851`. |
| S5 | `pares_mesmo_tipo` continua **cru-JULGAMENTO**, query em `app/ponto/services/flip_auto.py:35`; essa sequência define os pares em `:41`–`:46` e alimenta a fila em `:146`. Há cura separada da contagem de dia: `classificar_pares` usa `batidas_do_dia_turno` em `:104`, autoridade que exclui retratação por `turnos_do_colab` (`app/ponto/turnos.py:3024`, `:3033`). Isso não cura a query de formação dos pares. |
| S6 | **Cru-JULGAMENTO** na própria função: `proxima_batida_de` lê a última batida sem marca em `app/ponto/utils.py:244` e decide o tipo em `:246`. Não encontrei chamada dessa função em produção por busca AST/textual [não verificado quanto a chamadas dinâmicas]. A autoridade registrada para a próxima batida é outra: `ponto/turnos.py::proximo_tipo_de`, em `app/core/juizes.py:197`. |

Não é possível provar commits pela cópia sem metadados Git [não verificado]. As curas acima têm prova nas linhas atuais, não em comentários históricos. [nao verificado]

Selos:

- **B5-j** vigia chamadas a `parear_turnos` e `_turnos_do_colab_puro`, incluindo aliases de importação (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:63`, `:66`, `:118`). Busca apenas `Batida.objects` literal dentro do escopo que chama essas funções (`:89`, `:93`). Considera qualquer query cuja representação contenha `retratada_em` conforme/copiadora; aceita referência à porta ou lookup `pk/id` (`:125`–`:131`). Não resolve fluxo entre funções nem managers relacionados; os limites declarados estão em `:26`–`:34`. Executando apenas suas funções AST em memória, o resultado foi **0 crus e 6 copiadores** [não verificado por artefato persistido; execução somente leitura].
- **Bocais** limita-se a seis arquivos: `ponto/services/fechamento.py`, `folha/export.py`, `relatorios/views.py`, `relatorios/pdf_espelho.py`, `ponto/views.py`, `api/views.py` (`app/ponto/tests/test_tripwire_bocais_apuracao.py:13`). Conta texto que casa `(?:Batida|_Bat)\.objects\.filter`, sem AST nem inspeção do predicado (`:25`, `:29`, `:30`). Permite 3 ocorrências em `ponto/views.py` e 7 em `api/views.py` (`:18`, `:20`).
- **Todos os sítios S1..S6 e os dois commands produtores crus de S1 ficam fora dos dois selos**, pelas restrições acima; executei `universos` sobre esses oito arquivos e todos retornaram `([], [])` [não verificado por artefato persistido]. Nenhum desses arquivos está no `ALLOW` dos bocais (`app/ponto/tests/test_tripwire_bocais_apuracao.py:13`).

Juiz:

- **Não há autoridade registrada para “esta batida conta?”**: a família `batida` registra períodos/intervalo, horas do par, geofence e selfie (`app/core/juizes.py:1173`–`:1183`); `turno/marcos` registra outras perguntas, não o universo apurável (`:195`–`:237`).
- **Não precisa criar função/lei nova para o universo**: `batidas_apuraveis` já se declara autoridade única, exclui retratadas e preserva a distinção de exibição (`app/ponto/turnos.py:2844`–`:2850`, filtro `:2864`); a lei a nomeia em `CLAUDE.md:155`.
- Registrar agora essa autoridade no registro vivo exige observar a regra de nascimento de autoridade, que pede corte nominal (`CLAUDE.md:74`–`:76`). Isso é decisão de registro; não demonstra lacuna na lei que define o universo (`CLAUDE.md:155`).

Decisões técnicas extraídas:

1. Migrar os universos de S1 nos dois commands para a porta; `classificar_dia` recebe insumo e não deve ganhar query (`app/ponto/management/commands/supra_juiz.py:113`, `app/ponto/management/commands/emitir_furo_retroativo.py:53`, `app/ponto/supra_juiz.py:207`).
2. S2, S4 e a formação de pares de S5 devem consultar universo apurável, pois seus resultados escolhem materialização/paridade/flip (`app/ponto/services/veredito_celula.py:215`, `app/chamados/services/materializacao.py:94`, `app/ponto/services/flip_auto.py:35`).
3. Manter o uso cru de S3 para impedir repetição de escrita; separar o julgamento de satisfação, que precisa de prova viva (`app/chamados/juizes.py:768`–`:775`; apresentação ainda sem essa marca em `app/chamados/services/materializacao.py:447`).
4. Para S6, escolher remoção de código sem chamadas localizadas ou delegação à autoridade já registrada da próxima batida (`app/ponto/utils.py:240`, `app/core/juizes.py:197`); chamadas dinâmicas não comprovadas [não verificado].
5. Ampliar selo para as leituras de julgamento que não chamam pareador e para aliases/managers relacionados; hoje os limites são explícitos (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:26`, `:89`; bocais `app/ponto/tests/test_tripwire_bocais_apuracao.py:13`, `:25`).

Nenhuma dessas mudanças de universo exige lei nova: a distinção apuração/exibição já está escrita (`CLAUDE.md:155`, `app/ponto/turnos.py:2847`). A exceção de idempotência tem justificativa técnica expressa no código (`app/chamados/juizes.py:768`).

## 08/11 10:58 · sessao 5a08669ba673 · - · - · origem=Ronald (tmux)

Lei e semântica: `CLAUDE.md:155-164`, `app/ponto/models.py:64-67` e `app/ponto/turnos.py:2844-2871` concordam: `retratada_em != NULL` exclui geometria/apuração, preserva exibição; `aprovada` significa crivo do DP, e pendente do DP ainda conta.

**Escritores encontrados fora de tests/migrations:**

- Escritor físico genérico: `app/ponto/registro_batida.py:235`, `:252-254` (`anotar`, `setattr` e `save(update_fields)`); campo permitido em `:205`.
- Retratar: `app/ponto/registro_batida.py:267`, `:271-274`: idempotência, grava `aprovada=False` e `retratada_em=timezone.now()`. Agenda relavra financeira em `:281`.
- Desfazer: `app/ponto/services/retratar_humano.py:190`, `:197-209`: grava `aprovada=True` e `retratada_em=None` pela mesma `anotar`.
- Chamadores que mandam retratar: `app/chamados/services/acoes_chamado.py:269`; `app/ponto/management/commands/detectar_par_relampago.py:224`; `app/ponto/management/commands/diff_passivo_s84.py:82` (ensaio, conforme `:75-76`, `:91`); `app/ponto/management/commands/passivo_disputa_retro.py:164`; `app/ponto/services/retratar_humano.py:179`.
- `app/ponto/services/retratar_humano.py:9-12` é histórico e já não serve como lista completa: afirma dois chamadores, mas os cinco acima existem.
- `app/ponto/registro_batida.py:152-158` aceita `**campos` na criação; não há passagem literal atual de `retratada_em` identificada nessas chamadas. Conteúdo arbitrário recebido dinamicamente: [não verificado].

**Leitores executáveis de `retratada_em`, levantados por AST (linhas; funções):**

| Arquivo | Leitores |
|---|---|
| `app/api/views.py` | `705 api_bater_ponto` |
| `app/api/views_core.py` | `702 api_bater_ponto` |
| `app/chamados/juizes.py` | `720,775 _pergunta_materializada_de_fato` |
| `app/chamados/management/commands/carimbar_por_batida_coincidente.py` | `73 handle` |
| `app/chamados/reconciliador.py` | `216 _par_integro` |
| `app/chamados/services/adesao.py` | `60,145 sincronizar_adesao` |
| `app/chamados/services/desvio_escala.py` | `60 medir_desvio` |
| `app/chamados/services/fila_por_causa.py` | `191 particionar` |
| `app/chamados/services/limbo_folgas.py` | `37 sugerir_folga`; `156 semana_muda` |
| `app/chamados/services/materializacao.py` | `616 perguntas_absorvidas_por` |
| `app/chamados/services/vigia_carimbo.py` | `113 classificar` |
| `app/chamados/veredito_lavrado.py` | `140-141 marcos_do_lote` |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py` | `95 _rodar` |
| `app/colaboradores/services/calendario.py` | `240,621 contexto_calendario` |
| `app/colaboradores/services/ficha.py` | `145 ficha_do_colab` |
| `app/colaboradores/services/pos_vinculo.py` | `38 fechar_adesao_com_batida` |
| `app/colaboradores/services/senha_provisoria.py` | `25 senhas_provisorias` |
| `app/core/services/painel_op.py` | `93 montar_painel_operacional` |
| `app/core/views.py` | `173 dashboard`; `372 dashboard_supervisor`; `775 painel_tv`; `1177 dashboard_dados` |
| `app/escala/services/meio_periodo.py` | `160 observado_meio_periodo` |
| `app/escala/services/propositor.py` | `478 propostas` |
| `app/ferias/situacao.py` | `278 situacao_de_ferias` |
| `app/ponto/management/commands/diff_calculador.py` | `271 handle` |
| `app/ponto/management/commands/diff_janela_he.py` | `94 handle` |
| `app/ponto/management/commands/diff_passivo_s84.py` | `80 _mutacao` |
| `app/ponto/management/commands/e6_piso_autopsia.py` | `72 handle` |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py` | `68 handle` |
| `app/ponto/management/commands/passivo_disputa_retro.py` | `161 handle` |
| `app/ponto/motor_calculo_v2.py` | `671 _periodos_pelo_marco` |
| `app/ponto/registro_batida.py` | `109,141 registrar_batida`; `271 retratar_batida`; leitura genérica dos valores anteriores em `244` |
| `app/ponto/services/adesao.py` | `35 primeira_batida`; `58 ids_com_adesao` |
| `app/ponto/services/cartorio.py` | `353 houve_fato_novo`; `1155 _julgar_colab_corpo` |
| `app/ponto/services/deslogados.py` | `161,174 apurar` |
| `app/ponto/services/faltas_de_hoje.py` | `55 faltas_de_hoje_sem_cobranca` |
| `app/ponto/services/flip_auto.py` | `229 etiquetas_pela_celula`; `287,298 avaliar_fila` |
| `app/ponto/services/flip_batida.py` | `15 flip_tipo` |
| `app/ponto/services/lastro.py` | `262 julgar` |
| `app/ponto/services/passivo_s84.py` | `54 vivas` |
| `app/ponto/services/piso_intervalo.py` | `41 pares_abaixo_do_piso` |
| `app/ponto/services/retratar_humano.py` | `48,53 prazo_de_desfazer`; `67,75 prazos_de_desfazer`; `88 decisao_de`; `173 retratar`; `197 desfazer` |
| `app/ponto/turnos.py` | `1600,1612 papel_por_minuto_da_ata`; `1862 turnos_do_colab`; `1900 _ultima_bat`; `2219 _turno_aberto_calc`; `2351 turnos_abertos_de`; `2382 sequencia_relogio_x_jornada`; `2467 proximo_tipo_de`; `2864 batidas_apuraveis`; `2903 dia_das_batidas` |

Essas referências cobrem atributos, `getattr`, filtros ORM e campo temporal dinâmico; não incluem comentários/docstrings nem snapshots `antes` de auditoria como se fossem leitura da coluna. Exemplos de snapshots: `app/ponto/management/commands/diff_passivo_s84.py:84`, `app/ponto/management/commands/passivo_disputa_retro.py:146,170`.

**Interpretações especiais que preservam o significado:**

- Exibição busca deliberadamente somente retratadas em `app/colaboradores/services/calendario.py:239-244`, organiza o render separado da apuração em `:619-621`, e o template desenha riscado e informa “fora do cálculo” em `app/templates/colaboradores/partials/_calendario_grade.html:121`.
- Invalidação usa o instante da retratação como fato novo, sem contar a marca como trabalho: `app/chamados/veredito_lavrado.py:137-142`; `app/ponto/services/cartorio.py:329-353`.
- Prazo/desfazer e decisão humana leem data para ações e explicação: `app/ponto/services/retratar_humano.py:42-53,66-77,80-101,190-210`.
- `_par_integro` consulta cru em `app/chamados/reconciliador.py:206`, mas recusa se membro selecionado foi retratado em `:216-217`: o resultado final tem filtro explícito em memória.
- `_batida_existente_na_janela` é propositalmente compartilhado com idempotência de escrita; justificativa atual em `app/chamados/juizes.py:768-773`; o leitor que prova materialização exige `retratada_em is None` em `:774-775`. Mudar o helper inteiro sem separar as duas perguntas pode replantar aquilo que o cron retratou, conforme `:770-772`.

**Falha semântica adicional que exige atenção no censo principal:** `app/ponto/services/flip_auto.py:291-292` carrega `reais` cru; `:298` exclui retratadas apenas da lista de flips, mas `:303` passa a lista crua inteira para `grade_dia` no cálculo de `p0`. Portanto, a presença de um leitor correto de `retratada_em` nessa função não prova que todo o julgamento usa o universo correto.

Todos os **leitores diretos identificados** reconhecem retratação como exclusão, apresentação ou instante de mudança pelos exemplos citados; o sistema inteiro não aplica uniformemente a exclusão, como demonstra `flip_auto.py:291-303`. Garantia sobre estado efetivo em produção: [não verificado].

## 08/11 10:52 · sessao d2eb00cbbb9c · SELOS-DO-CANON-0811 · leitura · origem=Code

As referências abaixo são da cópia fornecida. Ela não contém `.git`; não consegui revalidar sua identidade como HEAD `80df6a5dc22bd4868696699ab8bdb51ce033198d`. [nao verificado]

**I-1 — CENSO-ANTES-DA-VAGA**

**(a) Quem chama hoje.** Existe o ponto de interceptação: `.claude/settings.json:62` configura `PreToolUse`, chamando `disjuntor.py pretool` em `.claude/settings.json:68`. Porém, o ramo `Agent` atual lê o prompt e aplica B3/B4; não abre brief nem valida CENSO. Portanto, **o hook geral existe; a guarda de abertura do construtor seria nova**. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`)

O protocolo também difere: o disjuntor comunica negação por JSON `permissionDecision=deny`, e seu `main_pretool` retorna zero. O contrato novo de validador com `rc 1` ainda não corresponde a esse protocolo existente. (`bin/disjuntor.py:1395`, `bin/disjuntor.py:1493`, `bin/disjuntor.py:1507`)

Os briefs aparecem como artefatos preparados e despachados pela sessão: E1 “brief pronto” e depois abertura do construtor; AKMED e PLANTIO também têm caminhos registrados. **Não há nomenclatura uniforme:** `logs/fio_e0_v4/e1_brief.md` não casa com `logs/<obra>/brief*.md`; `logs/v2bugs/i2/brief_v6.md` tem um nível adicional. (`app/docs/RELATO.md:95`, `app/docs/RELATO.md:91`, `app/docs/RELATO.md:80`, `app/docs/RELATO.md:75`, `app/docs/RELATO.md:205`)

No inventário desta cópia: **zero arquivos `logs/*/brief*.md`, zero obras observáveis por esse glob**; `logs/` não foi fornecido. Quantas obras os possuem em produção: **[nao verificado]**. O diretório está ignorado pelo Git, portanto sua ausência na exportação não prova ausência no vivo. (`.gitignore:53`)

**(b) Colisão com PAREI.** A linha contratada **não satisfaz nenhum dos dois caminhos atuais**:

- PAREI precisa conter `!`; pergunta de lei tampouco devolve turno por esse caminho. Além disso, deve travar o item em curso, sem outros itens andáveis. (`bin/hook_stop_fila1.py:1109`, `bin/hook_stop_fila1.py:1119`, `bin/hook_stop_fila1.py:1155`, `bin/hook_stop_fila1.py:1076`)
- Espera provada precisa começar literalmente por `ESPERANDO:` e declarar `pid=` ou `arquivo=`. `PAREI: censo-<obra> | espera Codex` não declara nenhum deles. (`bin/hook_stop_fila1.py:656`, `bin/hook_stop_fila1.py:665`, `bin/hook_stop_fila1.py:673`)

Os sítios envolvidos na compatibilização são o reconhecedor de espera, sua memoização por Stop, o orçamento que fica pendente durante espera provada e a liberação final. Mudar apenas a decisão final deixaria o orçamento tratando o censo por outro caminho. (`bin/hook_stop_fila1.py:645`, `bin/hook_stop_fila1.py:688`, `bin/hook_stop_fila1.py:984`, `bin/hook_stop_fila1.py:1231`)

| Leitor/selo solicitado | O que faz hoje e a colisão |
|---|---|
| `test_hook_parei_so_lei.sh` | Exige `_SO_BANG`; rejeita decisão técnica e lei como portas; aceita `!`. Relaxar todo PAREI contrariaria esses casos. (`bin/tests/test_hook_parei_so_lei.sh:25`, `bin/tests/test_hook_parei_so_lei.sh:41`, `bin/tests/test_hook_parei_so_lei.sh:52`, `bin/tests/test_hook_parei_so_lei.sh:60`) |
| `test_hook_esperando_com_prova.sh` | Distingue produtor vivo, morto, outro programa e declaração sem prova; verifica que a liberação ocorreu especificamente por espera provada. (`bin/tests/test_hook_esperando_com_prova.sh:67`, `bin/tests/test_hook_esperando_com_prova.sh:93`) |
| `test_disjuntor.sh` | Exercita PAREI CLEAR, DIFF em curso, agente sem entrega e COTA com handoff; também impede usar arquivo escrito pelo próprio hook como prova. São interações a preservar, sem um caso censo demonstrado. (`bin/tests/test_disjuntor.sh:192`, `bin/tests/test_disjuntor.sh:200`, `bin/tests/test_disjuntor.sh:215`, `bin/tests/test_disjuntor.sh:477`, `bin/tests/test_disjuntor.sh:578`) |
| `handoff_sessao.sh` | Já exibiria a linha censo, mas diria “item em curso ANDA”/“não devolve turno”: consulta somente os predicados de PAREI genérico. (`bin/handoff_sessao.sh:108`, `bin/handoff_sessao.sh:120`) |
| `alarme_sessao_ociosa.py` | Já desarma com qualquer substring `PAREI:` nas primeiras 40 linhas, sem prova. Pode silenciar enquanto o Stop continua mandando seguir. (`bin/alarme_sessao_ociosa.py:119`, `bin/alarme_sessao_ociosa.py:127`, `bin/alarme_sessao_ociosa.py:42`) |

A prova PID atual verifica processo vivo, identidade do executável/script e estabilidade do PID durante a leitura; **não consulta `CODEX_OBRA` nem `CODEX_PAPEL`**. A alternativa por arquivo exige avanço de mtime; a primeira observação não prova progresso. (`bin/hook_stop_fila1.py:523`, `bin/hook_stop_fila1.py:535`, `bin/hook_stop_fila1.py:548`, `bin/hook_stop_fila1.py:640`)

O wrapper exige obra e papel, mas isso não estabelece vínculo entre o novo PAREI e um censo vivo daquela obra. Sua emissão explícita de `ESPERANDO` com PID/JSONL ocorre no ramo **construtor**, não no papel censo. (`bin/codex_pergunta.sh:81`, `bin/codex_pergunta.sh:85`, `bin/codex_pergunta.sh:375`)

Assim, o lastro específico “produtor vivo do censo desta obra” permanece **[nao verificado]**. Aceitar somente o literal dispensaria o requisito de prova; aceitar qualquer Codex vivo não distinguiria obra nem papel, campos que o wrapper já separa. (`bin/hook_stop_fila1.py:673`, `bin/codex_pergunta.sh:81`)

**(f) Furos.** O ramo atual não fotografa versão/conteúdo do brief; portanto não estabelece identidade abertura→brief. Também depende da passagem pelo hook configurado, e a chave `DISJUNTOR_DESLIGADO` pula suas verificações; exceções fora do ramo B3 liberam a ferramenta. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1487`, `bin/disjuntor.py:1496`)

Cobertura de agente aberto por outra via, allowlist vazia efetivamente guardada e proteção contra brief alterado após abertura: **[nao verificado]**. Existe ainda outra abertura de construtor, pelo wrapper Codex, com vaga própria e execução própria; cobri-la não decorre automaticamente do ramo `Agent`. (`bin/codex_pergunta.sh:233`, `bin/codex_pergunta.sh:394`)

**I-2 — UNIDADE-NO-AVAL**

**(a) Quem registra hoje.** A responsabilidade declarada é do **Code/sessão**: prompt recebido vira linha em `PROMPTS.md`, e pedido de obra vira BACKLOG no mesmo turno. Ronald aparece como autor dos avais registrados. Não localizei uma porta executável única responsável por registrar e despachar esses avais. (`CLAUDE.md:433`, `app/docs/PROMPTS.md:3`, `app/docs/PROMPTS.md:1931`; registrador único: **[nao verificado]**)

Os registros apontam arquivos privados como `logs/plantio_mais1/aval_i.txt`; quem efetivamente escreveu esses arquivos, e por qual ferramenta, é **[nao verificado]**. (`app/docs/PROMPTS.md:1931`, `.gitignore:53`)

`gerar_avais.py` não resolve essa identidade: ele transforma `PENDENTES_RONALD.json` em `AVAIS.md`, a lista de pedidos abertos para Ronald responder. Não é o escritor de `PROMPTS.md`/`aval_*.txt`. (`bin/gerar_avais.py:3`, `bin/gerar_avais.py:30`, `bin/gerar_avais.py:105`)

**(d) Marcadores detectáveis nos registros disponíveis.** Há exemplos textuais das seis classes; isso comprova vocabulário, não um classificador formal. (`app/docs/PROMPTS.md:1477`, `app/docs/PROMPTS.md:1931`, `app/docs/PROMPTS.md:1933`, `app/docs/PROMPTS.md:1966`, `app/docs/PROMPTS.md:1967`)

| Classe | Exemplo no registro |
|---|---|
| Backfill | “backfill de data_evento”. (`app/docs/PROMPTS.md:1931`) |
| Migração | “migracao e backfill chamam a MESMA porta”. (`app/docs/PROMPTS.md:1967`) |
| Dedup | “oraculo E6 perde o dedup < 10 min”. (`app/docs/PROMPTS.md:1477`) |
| Juiz | “juiz tarefa_do_dia nasce”. (`app/docs/PROMPTS.md:1933`) |
| Veto | “MESMO veto de merito”. (`app/docs/PROMPTS.md:1967`) |
| Catraca | Contagem de `select_for_update` sobre modelos nomeados. (`app/docs/PROMPTS.md:1966`) |

**Falso positivo concreto:** `dedup` aparece em instrumento de medição de consumo do Code; `catraca`/`juizes` aparecem numa ordem de gestão da noite. Casar essas palavras classificaria também esses textos como DADO. (`app/docs/PROMPTS.md:1872`, `app/docs/PROMPTS.md:1839`)

**Falso negativo no resumo disponível:** E1-FATO-E-O-TURNO muda o critério dos selos que recusam por `data_evento`, sem empregar nenhuma das seis palavras. O texto integral privado desse aval não foi fornecido. (`app/docs/PROMPTS.md:1977`; integral: **[nao verificado]**)

A isenção por título também falha: TOAST-E-NOME-CALENDARIO reúne veto de criação/salvamento e apresentação de toast no mesmo registro. (`app/docs/PROMPTS.md:1828`)

A memória citada tem exemplos explícitos: grep contou 13 arquivos contra nove por AST porque prosa citava a palavra; na O118, identidade por texto `HH:MM` produziu falsos positivos, corrigidos consultando o predicado de batidas apuráveis. (`bin/atomicidade_da_porta.py:41`, `app/docs/BACKLOG.md:1199`)

**(f) Furos.** Autoridade para classificar DADO versus gestão/instrumento/UI, tratamento de aval misto, campos vazios e caminho obrigatório antes do despacho não estão estabelecidos por um mecanismo localizado nesta cópia. **[nao verificado]**

Aval apenas no chat fica fora do alcance de um fiscal que só examina arquivo. O selo atual parte de promessas **já registradas** em PROMPTS e verifica IDs no BACKLOG; não comprova que todo aval recebido entrou no registro. (`bin/tests/test_prompt_virou_item.sh:21`, `bin/tests/test_prompt_virou_item.sh:30`)

**I-3 — ETAPAS-NO-BRIEF**

**(a/c) Abertura, opus final e B3.** A abertura compartilha o sítio do I-1, atualmente sem leitura do brief. O verificador recebe ID da obra, diff e log RED; seu checklist não exige brief, RELATO do pouso ou correspondência entre etapas e resultados. (`bin/disjuntor.py:1451`, `.claude/agents/verificador.md:3`, `.claude/agents/verificador.md:8`)

O **B3 atual não faz essa conferência**. Ele identifica obra/leitura final pelo prompt e extrai `APROVADO`/`REPROVADO` da primeira linha do `SubagentHandback`, filtrando agentes do tipo `verificador`. (`bin/disjuntor.py:546`, `bin/disjuntor.py:561`, `bin/disjuntor.py:742`)

Ele lê o RELATO para fotografar duas linhas úteis do topo e reconhecer `DESENHO`/`ERRO-BOBO` que liberam a obra. Conta reprovações e voltas Codex procedentes, não etapas entregues. (`bin/disjuntor.py:662`, `bin/disjuntor.py:626`, `bin/disjuntor.py:910`)

Há uma lacuna temporal: **o opus final precede o pouso**, enquanto a linha de medida é exigida no RELATO **do pouso**. O artefato e o instante da conferência de etapas antes de autorizar esse pouso não estão definidos no código atual. (`CLAUDE.md:374`, `CLAUDE.md:379`, `.claude/agents/verificador.md:3`)

**Como contaria hoje:** não há contador demonstrado. Pela letra do anexo, seria necessário comparar as etapas declaradas no brief com as etapas que têm resultado medido no RELATO; contar ocorrências de “etapa K” não comprova essa relação. Identidade de versão, resultado antigo versus atual e resultado esperado versus medido permanecem sem reconhecedor verificado. **[nao verificado]**

O checador de prova existente também não supre isso: procura `PROVA:` próximo de certos atos em título/negrito, sem comparar etapa, comando ou valor medido. (`bin/relato_afirma_com_prova.py:36`, `bin/relato_afirma_com_prova.py:47`)

**(f) Furos.** Brief editado depois da abertura perde uma referência estável para o denominador das etapas; o ramo atual não guarda essa referência. Além disso, B3 só reconhece os tipos/prompts finais previstos, e o verificador manda retornar apenas problemas — até 15 linhas —, sem relatório positivo N/N. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:555`, `bin/disjuntor.py:742`, `.claude/agents/verificador.md:18`)

Proteção específica contra reescopo, uso de brief anterior ou aval de alteração mantido apenas no chat: **[nao verificado]**.

**(e) MEDIDA — comum aos três instrumentos**

`ESTADO.md` é gerado por `bin/gerar_estado.py`, chamado por `relato.sh` para `$TMP/ESTADO.md`, e publicado pela raspagem. O gerador grava o destino recebido; não depende de existir `app/docs/ESTADO.md`. (`bin/relato.sh:38`, `bin/gerar_estado.py:390`, `bin/relato.sh:105`)

**Já existe molde de janela móvel de 24 h:** `disjuntor.ler` usa `agora - 86400`, lê eventos datados e conta disparos, negações e liberações. O ESTADO consome esse medidor por `ler/linha`. (`bin/disjuntor.py:1546`, `bin/disjuntor.py:1568`, `bin/disjuntor.py:1570`, `bin/gerar_estado.py:277`)

Há também contagem de chamadas Codex por papel em 24 h, baseada em eventos `ini`; e média de espera CLEAR baseada em `espera=Ns`. **Essa média não mede horas de vaga ociosa esperando censo.** (`bin/codex_limite_estado.py:24`, `bin/codex_limite_estado.py:71`, `bin/disjuntor.py:1580`, `bin/disjuntor.py:1688`)

Os contadores atuais não demonstram as novas medidas de I-1/I-2/I-3; zero construtores sem censo, 100% de avais conformes e 100% de pousos N/N em produção permanecem **[nao verificado]**. Os campos hoje publicados são os explicitados em `bin/disjuntor.py:1615` e `bin/disjuntor.py:1691`.

## 08/11 10:50 · sessao 3977fc5450fa · - · - · origem=Ronald (tmux)

**I-2 — quem registra.** A obrigação atual é do Code/sessão: prompt recebido ganha linha em `PROMPTS.md` e obra ganha item no BACKLOG no mesmo turno (`CLAUDE.md:433-435`; `app/docs/PROMPTS.md:3-9`). Ronald aparece como autor do aval; os registros dizem “recebido” e “repassado ao construtor” (`app/docs/PROMPTS.md:1948-1950`). Não localizei registrador central executável, validador `UNIDADE=`, diretório `avais_recusados` ou selo I-2 na cópia **[não verificado]**. O executor físico das escritas privadas em `logs/*/aval_*.txt` é **[não verificado]**; `logs/` está ignorado (`.gitignore:53`).

`gerar_cortes.py` é outro instrumento: lê `CORTES.json` e gera `CORTES.md`/tabela do RELATO; não registra o recebimento do aval (`bin/gerar_cortes.py:3-10`, `:23-25`). Handoff e placar **leem** PROMPTS (`bin/handoff_sessao.sh:128-140`; `bin/placar_code.sh:395-396`).

| Classe | Marcador textual já encontrado em registro de aval |
|---|---|
| Backfill | “backfill de data_evento” — `app/docs/PROMPTS.md:1931` |
| Migração | “migracao e backfill chamam a MESMA porta” — `app/docs/PROMPTS.md:1967` |
| Dedup | “oraculo E6 perde o dedup < 10 min” — `app/docs/PROMPTS.md:1477` |
| Juiz | “juiz tarefa_do_dia nasce” — `app/docs/PROMPTS.md:1933` |
| Veto | “MESMO veto de merito” — `app/docs/PROMPTS.md:1967` |
| Catraca | “conta so select_for_update sobre” quatro modelos — `app/docs/PROMPTS.md:1966` |

**Palavra não comprova classe.** “Dedup” aparece em medição de consumo de agentes, instrumento isento pelo contrato fornecido; usá-la como classe DADO recusaria esse instrumento (`app/docs/PROMPTS.md:1872`). “Catraca” e “juízes” aparecem em aval de ordem/teto da noite (`app/docs/PROMPTS.md:1839`). Título UI também não determina isenção: TOAST-E-NOME-CALENDARIO contém uma parte de veto de criação/salvamento e outra de apresentação (`app/docs/PROMPTS.md:1828`).

**Falso negativo lexical demonstrável:** o resumo do aval E1-FATO-E-O-TURNO-NAO-A-DATA-GRAVADA trata da recusa por `data_evento` sem nenhuma das seis palavras; classificador dessas palavras não o selecionaria (`app/docs/PROMPTS.md:1977`). O texto integral desse aval privado permanece **[não verificado]**.

A memória tem fundamento concreto: o varredor de atomicidade documenta grep contando 13 arquivos contra 9 por AST porque prosa citava a palavra, e proximidade textual de decorador não provava cobertura (`bin/atomicidade_da_porta.py:8-12`, `:41-44`). A memória O118 apontada pelo pai está em `app/docs/BACKLOG.md:1199`.

**Furo de chat/arquivo.** O selo atual considera somente IDs no idioma literal `BACKLOG **Oxx/Fxx**` já escritos em PROMPTS e verifica sua existência no BACKLOG; não estabelece que todo aval entrou no registro (`bin/tests/test_prompt_virou_item.sh:21-35`). A lacuna está explicitamente documentada no histórico (`app/docs/RELATO-ARQUIVO.md:30931`). Portanto, fiscalização restrita ao registro não observa aval que ficou apenas no chat; a ocorrência atual disso é **[não verificado]**.

**Allowlist/classificação.** O anexo I-2 isenta gestão/instrumento/UI, mas autoridade executável que decide essa classe, tratamento de aval misto e caminho único obrigatório de despacho permanecem **[não verificado]**. Não há evidência para chamar essas seis palavras de marcador formal existente **[não verificado]**.

## 08/11 10:49 · sessao fa5c2257202c · - · - · origem=Ronald (tmux)

I-3 — pontos comprovados na cópia:

- O verificador recebe **id da obra, diff e log do RED**; lê `CLAUDE.md`/`LEIS.md`, verifica nove itens e responde com primeira linha `APROVADO` ou `REPROVADO`. O arquivo não determina leitura de brief nem do RELATO do pouso. `.claude/agents/verificador.md:3`, `.claude/agents/verificador.md:8`, `.claude/agents/verificador.md:18`.
- O B3 lê os **transcripts dos verificadores**: identifica obra/leitura final no primeiro prompt e extrai o veredito da primeira linha de `SubagentHandback`; filtra `agentType=verificador`. Não extrai etapas ou resultados dessas respostas. `bin/disjuntor.py:546`, `bin/disjuntor.py:551`, `bin/disjuntor.py:561`, `bin/disjuntor.py:736`.
- O B3 lê o **RELATO**, mas o uso atual é fotografar duas linhas úteis do topo e reconhecer `DESENHO`/`ERRO-BOBO` para liberar a obra. Não é leitura do marco do pouso para conferir execução integral. `bin/disjuntor.py:626`, `bin/disjuntor.py:662`, `bin/disjuntor.py:686`, `bin/disjuntor.py:930`.
- O disparo atual conta reprovações do opus final e voltas Codex triadas procedentes; limiares são 1 e 3, respectivamente. `bin/disjuntor.py:76`, `bin/disjuntor.py:77`, `bin/disjuntor.py:896`, `bin/disjuntor.py:910`.
- O opus final ocorre **antes do pouso**, depois da suite verde; a linha de medida é exigida no RELATO **do pouso**. Portanto o I-3 deixa sem endereço/instante definido o relato que o opus precisaria conferir antes de autorizar esse pouso. `CLAUDE.md:374`, `CLAUDE.md:379`; definição desse endereço/instante no anexo: [não verificado]. [nao verificado]
- Contador existente de “etapa K citada com resultado”, frase `pela metade, faltam N`, parser `RESULTADO OBSERVAVEL` ou selo `test_etapas_no_brief.sh`: [não verificado]. O anexo não fixa vínculo entre obra/versão do brief e marco do RELATO, nem critério para distinguir resultado medido de resultado esperado, citado ou antigo. [não verificado]
- O checador de prova já existente só procura `PROVA:` nas seis linhas próximas de certos atos em título/negrito; sua implementação não compara comando, número, etapas ou medição. `bin/relato_afirma_com_prova.py:30`, `bin/relato_afirma_com_prova.py:36`, `bin/relato_afirma_com_prova.py:47`.
- Furo de rebrief: o ramo atual de abertura recebe `prompt` do `Agent` e faz B3/B4; não lê nem fotografa brief. Nenhum vínculo imutável abertura→brief→etapas fica estabelecido nesse ramo. `bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`. Existência de outra proteção específica: [não verificado].
- Furo de identificação: B3 só captura como final prompts que correspondam ao reconhecedor `_eh_final` e verificadores com o tipo esperado; a existência de outros agentes/verificadores efetivamente lançados fora disso não pode ser inferida. `bin/disjuntor.py:499`, `bin/disjuntor.py:555`, `bin/disjuntor.py:742`; uso real fora desses filtros: [não verificado].

MEDIDA — gerador e moldes existentes:

- `ESTADO.md` é produzido por `gerar(raiz,dst)` e gravado em `dst`; `relato.sh` chama o gerador para `$TMP/ESTADO.md` e publica esse arquivo como `ESTADO.md`. `bin/gerar_estado.py:178`, `bin/gerar_estado.py:390`, `bin/relato.sh:38`, `bin/relato.sh:105`.
- O gerador **consome** medidores: importa `disjuntor.ler/linha` dentro de `try`; não reimplementa a contagem. `bin/gerar_estado.py:277`.
- Molde mais direto: `disjuntor.ler` usa `corte=agora-86400`, lê trilhas com carimbo, conta disparos/negações/liberações e acumula `espera=Ns` de eventos `ATENDIDO`. Trilhas ilegíveis tornam contadores `None`; a ausência de arquivo é tratada separadamente. `bin/disjuntor.py:1546`, `bin/disjuntor.py:1554`, `bin/disjuntor.py:1568`, `bin/disjuntor.py:1580`, `bin/disjuntor.py:1584`, `bin/disjuntor.py:1590`.
- Outro molde: `codex_limite_estado` conta recusas em 24 h e chamadas por papel, contando apenas eventos `ini`; o escritor declarado é `codex_pergunta.sh`. `bin/codex_limite_estado.py:4`, `bin/codex_limite_estado.py:24`, `bin/codex_limite_estado.py:27`, `bin/codex_limite_estado.py:71`.
- Esses moldes demonstram janela e integração ao painel; dados atuais para “construtor sem censo”, “horas de vaga esperando censo”, “aval recusado por UNIDADE” e “pouso N/N etapas” não aparecem nesses contadores. `bin/disjuntor.py:1615`, `bin/disjuntor.py:1691`, `bin/codex_limite_estado.py:45`; existência das novas trilhas e suas medidas em produção: [não verificado].

## 08/11 10:49 · sessao 2c9c1db42771 · - · - · origem=Ronald (tmux)

**I-1 / colisão e leitores**

- A linha proposta `PAREI: censo-<obra> | espera Codex` **não libera hoje por nenhum dos dois caminhos**: `_parei_do_em_curso` exige `PAREI:` e `!`; `_esperando_com_prova` exige prefixo literal `ESPERANDO:` e ao menos `pid=` ou `arquivo=`. (`bin/hook_stop_fila1.py:1104`, `bin/hook_stop_fila1.py:1119`, `bin/hook_stop_fila1.py:656`, `bin/hook_stop_fila1.py:673`.)
- O PAREI genérico ainda exige que a linha nomeie o item em curso e que não haja outros andáveis; acrescentar apenas `!` tampouco equivale ao contrato do censo. (`bin/hook_stop_fila1.py:1087`, `bin/hook_stop_fila1.py:1142`, `bin/hook_stop_fila1.py:1155`.)
- Os sítios dependentes da espera são o reconhecedor, a memoização por Stop, o orçamento que fica pendente durante espera provada e a decisão principal do Stop; mudar só o ramo final deixa o orçamento tratar a espera de censo diferentemente. (`bin/hook_stop_fila1.py:645`, `bin/hook_stop_fila1.py:688`, `bin/hook_stop_fila1.py:984`, `bin/hook_stop_fila1.py:1231`.)
- Antes desses caminhos, a checagem de afirmações do RELATO e o B4 têm precedência; a exceção do censo não pode ser presumida como passagem por eles. (`bin/hook_stop_fila1.py:1194`, `bin/hook_stop_fila1.py:1204`, `bin/hook_stop_fila1.py:1215`.)
- O handoff já **exibiria** a linha censo, pois coleta `PAREI` até a primeira seção, mas continuaria imprimindo “item em curso ANDA” / “não devolve turno”, por consultar exclusivamente os predicados de PAREI genérico. (`bin/handoff_sessao.sh:99`, `bin/handoff_sessao.sh:108`, `bin/handoff_sessao.sh:120`.)
- O alarme já aceita qualquer substring `PAREI:` nas primeiras 40 linhas e desarma, sem consultar prova ou `_SO_BANG`: com censo inexistente/morto ele já pode ficar silencioso enquanto o Stop manda seguir. (`bin/alarme_sessao_ociosa.py:119`, `bin/alarme_sessao_ociosa.py:127`, `bin/alarme_sessao_ociosa.py:42`.)

**Selos que precisam ser confrontados, sem execução**

- `test_hook_parei_so_lei.sh` preserva o filtro `_SO_BANG`, rejeita decisão técnica e pergunta de lei como passagem e aceita `!`; liberar todo PAREI violaria esses casos. Não contém caso censo. (`bin/tests/test_hook_parei_so_lei.sh:25`, `bin/tests/test_hook_parei_so_lei.sh:41`, `bin/tests/test_hook_parei_so_lei.sh:52`, `bin/tests/test_hook_parei_so_lei.sh:60`.)
- `test_hook_esperando_com_prova.sh` distingue vivo/morto/outro produtor/sem prova e exige trilha `ALLOW ESPERANDO com prova` ou `NOPROVA`; censo precisaria entrar com essa mesma distinção, sem transformar o literal em prova. (`bin/tests/test_hook_esperando_com_prova.sh:67`, `bin/tests/test_hook_esperando_com_prova.sh:79`, `bin/tests/test_hook_esperando_com_prova.sh:93`.)
- `test_disjuntor.sh` protege PAREI CLEAR com handoff, validade temporal, DIFF e agente sem entrega; B4 COTA também exige handoff; protege ainda contra `pisos.log` como prova escrita pelo próprio hook. São interações a preservar, não regras hoje alteradas pelo literal censo. (`bin/tests/test_disjuntor.sh:104`, `bin/tests/test_disjuntor.sh:192`, `bin/tests/test_disjuntor.sh:200`, `bin/tests/test_disjuntor.sh:215`, `bin/tests/test_disjuntor.sh:477`, `bin/tests/test_disjuntor.sh:578`.)

**Prova de vida e furo**

- A prova PID atual lê `/proc/<pid>/stat`, executable e cmdline, rejeita ancestrais, zumbi, troca de processo e produtor divergente; ela **não lê `CODEX_OBRA`**. (`bin/hook_stop_fila1.py:523`, `bin/hook_stop_fila1.py:535`, `bin/hook_stop_fila1.py:544`, `bin/hook_stop_fila1.py:548`.)
- A prova por arquivo exige arquivo regular e avanço de mtime desde avaliação anterior, sob trava; primeira observação não prova e arquivos do próprio painel/hook são excluídos. (`bin/hook_stop_fila1.py:563`, `bin/hook_stop_fila1.py:583`, `bin/hook_stop_fila1.py:604`, `bin/hook_stop_fila1.py:640`.)
- O wrapper exige `CODEX_OBRA` e `CODEX_PAPEL`, e o ramo construtor emite uma linha ESPERANDO com PID do wrapper e JSONL da chamada. Isso oferece identidade declarada da chamada, mas não há vínculo dela com o novo literal censo no Stop. (`bin/codex_pergunta.sh:81`, `bin/codex_pergunta.sh:85`, `bin/codex_pergunta.sh:374`.)
- Uma prova específica precisaria vincular processo vivo/chamada ao **mesmo censo e obra**, por exemplo identidade `CODEX_OBRA` conferível no produtor ou sinal próprio com progresso. A existência desse mecanismo é **[não verificado]**; aceitar somente “espera Codex” abre espera sem produtor, e aceitar qualquer processo Codex permite que outra obra justifique a parada. A insuficiência do código atual se apoia em `bin/hook_stop_fila1.py:656`, `bin/hook_stop_fila1.py:673`, `bin/hook_stop_fila1.py:535`.
- Processo Codex real ativo, acesso a `/proc` em produção e resultado dos selos nesta auditoria: **[não verificado]**.

## 08/11 10:41 · sessao 16d8ac42b6cf · CENSO-ANTES-DA-VAGA-HOOK · leitura · origem=Code

**O contrato ainda não está implementado nesta árvore:** `ESPERANDO: censo <OBRA> | Codex` retorna **“produtor nao declarado em 1 palavra”**; o parser atual exige produtor único e campos `pid=`/`arquivo=`. (`bin/hook_stop_fila1.py:658–674`)

**(i) Identidade em `/proc`**

Depois da passagem pelo shebang, já executando o corpo do wrapper:

| Chamada | `/proc/<pid>/exe` | `argv[0]` | `argv[1]` |
|---|---|---|---|
| `bin/codex_pergunta.sh ...` | Executável do Bash | `bash` | `bin/codex_pergunta.sh` |
| `bash bin/codex_pergunta.sh ...` | Executável do Bash | `bash` | `bin/codex_pergunta.sh` |

A primeira forma usa `#!/usr/bin/env bash`; a segunda explicita esse interpretador. O caminho absoluto concreto de `exe` nesta execução é **[não verificado]**. (`bin/codex_pergunta.sh:1`)

Consequência para o hook: sendo `exe` um interpretador, `_processo_casa` examina **`argv[1]`**, não exige que `exe` seja o `.sh`. Produtor declarado com caminho absoluto compara o caminho resolvido usando o `cwd` do processo; produtor sem barra compara apenas o basename. (`bin/hook_stop_fila1.py:511–520`)

**(ii) `CODEX_OBRA`**

**É entrada do wrapper, não um export posterior:** o script lê `CODEX_OBRA` para `OBRA` e recusa valor vazio. O selo fornece `CODEX_OBRA` pelo `env` antes de executar o wrapper; nessa chamada, portanto, a variável já entra no ambiente inicial do processo. (`bin/codex_pergunta.sh:80–85`; `bin/tests/test_codex_pergunta.sh:111–113`)

O hook atual não lê esse ambiente: `_prova_pid` consulta `stat`, `exe` e `cmdline`, sem conferir `CODEX_OBRA`. (`bin/hook_stop_fila1.py:534–550`)

**(iii) Furos e limites**

- **Outro usuário:** a exigência de mesmo UID ainda não existe em `_prova_pid`. Falha de leitura bloqueia aquela prova, mas não há comparação explícita de UID. (`bin/hook_stop_fila1.py:523–550`)
- **Wrapper esperando vaga/flock:** processo vivo não prova Codex iniciado. O wrapper espera no gate, nos slots e na trava da resposta antes da chamada ao Codex. Já a vaga exclusiva do construtor usa `flock -n`: ocupada, recusa imediatamente. (`bin/codex_pergunta.sh:341–369`; `bin/codex_pergunta.sh:401–402`; `bin/codex_pergunta.sh:232–233`)
- **Wrapper de outra sessão:** excluir ancestrais não vincula o produtor à sessão principal. `_ancestrais` monta somente a cadeia de pais do hook; `_prova_pid` não compara sessão nem exige descendência do chamador. (`bin/hook_stop_fila1.py:474–491`; `bin/hook_stop_fila1.py:529–550`)
- **Obra com prefixo comum:** a igualdade exata exigida pelo contrato elimina esse casamento por prefixo; sua implementação é **[não verificado]**. Hoje não há comparação de obra nessa prova. (`bin/hook_stop_fila1.py:534–550`)
- **Papel e fase:** o wrapper admite cinco papéis; conferir somente wrapper e obra não distingue `censo` dos demais. Além disso, ele continua vivo fazendo pós-processamento depois que o Codex retorna. (`bin/codex_pergunta.sh:81–85`; `bin/codex_pergunta.sh:404–448`)
- **“Terminou → volta a bloquear” tem limite:** perder a prova apenas continua o fluxo normal do Stop; fila vazia, teto, B2 e exceção ainda possuem caminhos de liberação. (`bin/hook_stop_fila1.py:1231–1246`; `bin/hook_stop_fila1.py:1284–1306`)

**(iv) Outros leitores do topo do RELATO**

Primeiro, **“linha 2” não é posição física para o hook**: ele pula título e brancos e usa a primeira linha útil. A segunda linha útil integra o bloco destinado ao B3. (`bin/hook_stop_fila1.py:400–408`; `bin/hook_stop_fila1.py:415–440`)

| Leitor | Comportamento |
|---|---|
| **Disjuntor B3** | Lê fotografia consistente do documento e usa o mesmo `_bloco_do_topo`, com duas linhas úteis. (`bin/disjuntor.py:662–689`) |
| **`handoff_sessao.sh`** | Percorre o topo até `##`, mas seleciona somente `PAREI`, `lei:` e `!:`; omite ESPERANDO e consulta apenas os vereditos PAREI. (`bin/handoff_sessao.sh:92–123`) |
| **`relato.sh`** | Submete o documento ao checador de afirmações e ao raspador/publicador; não julga prova de ESPERANDO nessas etapas. (`bin/relato.sh:54–61`; `bin/relato.sh:103–107`) |
| **Alarme de sessão ociosa** | Procura `PAREI:` nas primeiras 40 linhas; ESPERANDO não satisfaz esse reconhecimento. (`bin/alarme_sessao_ociosa.py:119–127`) |
| **Selo ESPERANDO** | Monta estado na linha física **3** e verifica ALLOW/NOPROVA; cobre PID vivo, morto, outro programa e ausência de prova. (`bin/tests/test_hook_esperando_com_prova.sh:62–105`) |
| **Selo de orçamento** | Exige que ESPERANDO provado suspenda o disparo do orçamento; morto o produtor, cobra novamente o orçamento. (`bin/tests/test_orcamento_contexto.sh:220–233`) |
| **Selo do disjuntor** | Exercita o recorte compartilhado e inclui mutante que conserva somente a primeira linha útil. (`bin/tests/test_disjuntor.sh:845–865`; `bin/tests/test_disjuntor.sh:1658`) |

## 08/11 10:40 · sessao 55f94ed382ed · - · - · origem=Ronald (tmux)

Adicionais:

- O selo espiona chamadas a `H._bloco_do_topo`, verificando leitura sob a trava: `bin/tests/test_disjuntor.sh:845-865`.
- O mutante que conserva só a primeira linha útil deve reprovar: `bin/tests/test_disjuntor.sh:1658`. Há mutantes de fotografia parcial: `bin/tests/test_disjuntor.sh:1659-1663`.
- `test_no_relato_tem_secao_pinada.py` conserva o topo inteiro por comparação `startswith(TOPO)`; não interpreta a linha 2: `app/core/tests/test_no_relato_tem_secao_pinada.py:31-35`, `app/core/tests/test_no_relato_tem_secao_pinada.py:51-60`. Sua leitura posicional verifica a última linha útil: `app/core/tests/test_no_relato_tem_secao_pinada.py:68-69`.

Nenhum leitor adicional da linha física 2 identificado na varredura, incluindo `.claude`; ausência absoluta: [não verificado].

## 08/11 10:39 · sessao 55f94ed382ed · - · - · origem=Ronald (tmux)

Leitores e efeitos confirmados:

- **O hook não lê “linha física 2”**: `_linha_do_topo()` usa a primeira linha útil; `_bloco_do_topo()` pula brancos e `# RELATO`, tira crases iniciais e retorna até duas linhas úteis. A segunda útil existe para B3. (`bin/hook_stop_fila1.py:400-408`, `bin/hook_stop_fila1.py:415-440`)

- **Disjuntor B3** lê o RELATO inteiro duas vezes, confere identidade/mtime/tamanho e bytes; recorta as duas linhas úteis pelo mesmo `H._bloco_do_topo()`. Portanto continua sendo consumidor do painel, sem uma regra paralela de recorte. (`bin/disjuntor.py:662-689`)

- **Handoff** lê o topo até a primeira seção `## `, mas seleciona somente `PAREI`, `lei:` e `!:`. `ESPERANDO: censo ... | Codex` fica fora da lista; os vereditos mostrados chamam `_parei_do_em_curso()` e `_relato_parou()`, não a prova ESPERANDO. (`bin/handoff_sessao.sh:92-123`)

- **`relato.sh`** entrega o documento ao checador de afirmações de ato e depois ao raspador/publicador; nesse fluxo não interpreta o estado ESPERANDO. O checador percorre linhas procurando títulos/negritos com verbos de ato e prova próxima. (`bin/relato.sh:54-61`, `bin/relato.sh:103-107`, `bin/relato_afirma_com_prova.py:30-50`)

- **Alarme de sessão ociosa** lê as primeiras 40 linhas e só procura a substring `PAREI:`. A linha ESPERANDO não desarma esse reconhecimento. (`bin/alarme_sessao_ociosa.py:119-127`)

Selos relevantes:

- `test_hook_esperando_com_prova.sh` constrói RELATO com cabeçalho na linha 1, branco na 2 e estado na 3; exige liberação pelo motivo `ALLOW ESPERANDO com prova`, ou bloqueio com `siga: ITEM-VIVO` e motivo `NOPROVA`. (`bin/tests/test_hook_esperando_com_prova.sh:62-89`)
- O selo atual cobre produtor vivo, PID morto, outro programa e ausência de prova, usando a sintaxe `produtor | pid=`. (`bin/tests/test_hook_esperando_com_prova.sh:93-105`)
- Também exige que prova de ato seja conferida antes de ESPERANDO e que matar o produtor revogue a liberação. (`bin/tests/test_hook_esperando_com_prova.sh:186-192`)
- `test_orcamento_contexto.sh` exige que ESPERANDO com produtor vivo suspenda o disparo de orçamento e libere pelo motivo correto. (`bin/tests/test_orcamento_contexto.sh:220-230`)
- `test_disjuntor.sh` testa diretamente os juízes compartilhados de leitura do topo, inclusive leitura parcial após a primeira linha útil. (`bin/tests/test_disjuntor.sh:748-765`)
- O selo do handoff exercita perguntas de lei depois de 14 linhas RESPONDIDA, protegendo o alcance até a seção, sem testar censo. (`bin/tests/test_handoff_sessao.sh:139-148`)

## 08/11 10:35 · sessao 950169722c2a · FIO-E0-V4-E3 · censo · origem=Code

**O aval não confere como lista completa.** Dois pontos objetivos: não encontrei chamadas de produção de `o_que_segura`; e `explicador` e `folha/previa` são respondedores, embora estejam entre os nove nomes tratados como simples consumidores de C15. (`app/folha/fora_da_folha.py:203`, `app/chamados/services/explicador.py:163`, `app/folha/previa.py:95`)

| Base da conferência | Limite |
|---|---|
| AST dos arquivos Python da cópia, fora de testes, com conferência dos corpos, imports e aliases; chamadas concretas discriminadas abaixo. | Uso dinâmico externo à cópia: **[nao verificado]**. | [nao verificado]
| Aval e e2.r comparados pelo conteúdo colado no pedido. | Arquivos locais dos anexos e correspondência desta cópia com HEAD `94d41ea`: **[nao verificado]**. |
| `JUIZES.md` é lookup; o próprio documento manda conferir o vivo e aponta `core/juizes.py` como registro. | `app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:11`, `app/docs/JUIZES.md:14`. |
| Resultados abaixo descrevem código existente. | Quantidades atuais, ocorrências individuais em produção e comportamento visual do APK: **[nao verificado]**. |

**1. Conferência do aval**

| Item do aval | Resultado contra o vivo | Chamadores, unidade e fonte |
|---|---|---|
| `vigia_fio.fio_do_dia:156` | **Confere.** | Chamado por `censo` em `app/chamados/services/vigia_fio.py:414` e `app/chamados/services/vigia_fio.py:417`. Devolve booleano sobre a coleção recebida; **lê C15**, sem derivar vitalidade própria. `app/chamados/services/vigia_fio.py:166`. |
| `vigia_fio.censo_alcance` | **Confere.** Definição em `app/chamados/services/vigia_fio.py:542`. | Chamado pelo command em `app/chamados/management/commands/vigiar_fio_morto.py:173`. Responde por **(colab,dia)** retido, com rota, papel e causa; lê retenção, C17 e dono fora do fio, depois deriva contadores/causas. `app/chamados/services/vigia_fio.py:585`, `app/chamados/services/vigia_fio.py:635`, `app/chamados/services/vigia_fio.py:654`, `app/chamados/services/vigia_fio.py:699`. |
| C17 `porta_alcancavel_do_dia:1341` | **Confere.** | Três chamadores diretos: `_porta`, `censo_alcance`, `beco_em_lote`. Devolve `None` ou `{rota,fila,verbos,fio}` por **célula/dia**. Compõe fila, perguntas C6 vivas presentes no app e verbos do painel. `app/folha/fora_da_folha.py:67`, `app/chamados/services/vigia_fio.py:635`, `app/escala/services/cadastro_realidade.py:488`, `app/chamados/juizes.py:1386`. |
| `folha._fio_do_dia:70` | **Confere.** | Três chamadas: `fora_da_folha`, `o_que_segura`, `dias_sem_fio`. Devolve `admin/colab/chamado/nada` por célula; **lê `fio` de C17**. `app/folha/fora_da_folha.py:77`, `app/folha/fora_da_folha.py:178`, `app/folha/fora_da_folha.py:221`, `app/ponto/services/pautas_dos_discordantes.py:77`. |
| `o_que_segura:203`, “6 chamadores” | **Não confere quanto aos seis.** O AST não encontrou chamada de produção da função nesta cópia. | O corpo existe e responde colaborador/competência com dias retidos, fio, destrava e chamados; compõe export, retenção e C17. Definição e retorno: `app/folha/fora_da_folha.py:203`, `app/folha/fora_da_folha.py:215`, `app/folha/fora_da_folha.py:229`. Chamador dinâmico externo: **[nao verificado]**. |
| `resolver_dia_oferecido:97` e `STATUS_QUE_PEDEM_DECISAO:94` | **Confere.** | Única chamada direta: `contexto_calendario`, em `app/colaboradores/services/calendario.py:769`. Booleano por dia; **deriva** último recurso + passado + status em `falta/irregular/aberto/sem_certidao`. `app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`. |
| `perguntas_do_app:42 ← api/views:2552` | **Confere, mas faltam quatro chamadores.** | Também leem: `_no_app`, ficha, `medir`, `do_dia`. Unidade **pergunta**; lê `q_vivas` e C6 exato. `app/chamados/juizes.py:1319`, `app/colaboradores/services/ficha.py:166`, `app/chamados/services/pergunta_no_app.py:139`, `app/chamados/services/pergunta_no_app.py:162`, `app/chamados/services/pergunta_no_app.py:53`. |
| `pendentes_do_colaborador:1729 ← dois kernels` | **Confere, mas falta dashboard web.** | Três chamadas: `app/api/views.py:325`, `app/api/views_core.py:314`, `app/core/views.py:253`. Retorna **disputas abertas distintas** com pergunta C6 viva sem resposta; deriva o recorte da disputa e lê C6. `app/chamados/models.py:1736`, `app/chamados/models.py:1740`. |
| C11 `cadastro_realidade.beco_em_lote:442` | **Confere.** A chamada C17 está em **488**, não em 460. | Chamado em `app/escala/services/cadastro_realidade.py:439` e `app/escala/services/cadastro_realidade.py:573`. Devolve assinatura C11 por **(colab,dia)**; compõe célula indefinida, perguntas vivas, ausência de porta, âncora terminal, cadastro e dono. `app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:488`, `app/escala/services/cadastro_realidade.py:515`, `app/escala/services/cadastro_realidade.py:530`. A linha 460 é docstring. |

**Censo complementar: respondedores que faltaram na lista**

Na coluna “natureza”, **lê** identifica delegação ao juiz; **compõe/deriva** identifica seleção, agregação ou resposta construída no próprio sítio. As fontes e chamadas estão discriminadas em cada linha.

| Sítio/função | Quem chama | Unidade e resposta | Natureza |
|---|---|---|---|
| `vigia_fio.censo` — `app/chamados/services/vigia_fio.py:318` | `vigiar_fio_morto:172`; `materializar_passivo_validado:203`, em `app/chamados/management/commands/` | Células/dias com fio vivo, morto ou dono fora do fio. | Compõe universo de chamados/células, lê C15 e dono; agrega. `app/chamados/services/vigia_fio.py:334`, `app/chamados/services/vigia_fio.py:417`, `app/chamados/services/vigia_fio.py:431`. |
| `dono_fora_do_fio` — `app/chamados/services/vigia_fio.py:274` | `censo:431`, `censo_alcance:654`, mesmo arquivo | **(colab,dia)** → decisão pendente, ausência aberta, escala não descreve dia, fio do admin ou `None`; opcional papel. | Deriva precedência entre oferta do calendário, ausência, estado da pergunta e lista administrativa. `app/chamados/services/vigia_fio.py:306`. |
| `ListaDoAdmin.dia_do_admin/dia_na_lista/papel_do_dia` — `app/chamados/services/vigia_fio.py:222`, `app/chamados/services/vigia_fio.py:230`, `app/chamados/services/vigia_fio.py:238` | Dono em `app/chamados/services/vigia_fio.py:312`; alcance em `app/chamados/services/vigia_fio.py:661` | Presença e papel por **(colab,dia)**. | Lê lista Cadastro×Realidade e reduz seus donos. `app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:246`. |
| `resolver_dia_oferecido_nas_datas` — `app/colaboradores/services/calendario.py:117` | Vigia em `app/chamados/services/vigia_fio.py:271` | `{dia:bool}` para um colaborador. | Lê campo do calendário; dia ausente recebe falso. `app/colaboradores/services/calendario.py:132`, `app/colaboradores/services/calendario.py:135`. |
| `contexto_calendario` — `app/colaboradores/services/calendario.py:159` | `app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160`; plural em `app/colaboradores/services/calendario.py:132` | Dias com estado, pendência e oferta de decisão. | Compõe status/último recurso, lê C6 e escreve `resolver_dia_oferecido`. `app/colaboradores/services/calendario.py:476`, `app/colaboradores/services/calendario.py:769`. |
| `cadastro_realidade.lista/dias_na_lista/um_dono_por_dia` — `app/escala/services/cadastro_realidade.py:787`, `app/escala/services/cadastro_realidade.py:1044`, `app/escala/services/cadastro_realidade.py:985` | Tela `app/escala/views.py:487`; vigia `app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:217`; lista chama uniformização em `app/escala/services/cadastro_realidade.py:885` | Lista administrativa e dono por **(colab,dia)**. | Compõe assinaturas, incorpora C11 e lê `InsumosDoDono.dono`. `app/escala/services/cadastro_realidade.py:864`, `app/escala/services/cadastro_realidade.py:1009`, `app/escala/services/cadastro_realidade.py:1026`, `app/escala/services/cadastro_realidade.py:1064`. |
| `montar_chamados_fio` — `app/chamados/services/fio.py:160` | Colab/admin/modal em `app/chamados/views.py:569`, `app/chamados/views.py:677`, `app/chamados/views.py:889`; parcial em `app/chamados/services/fio.py:442` | **Chamados**, com perguntas e indicador de ação pendente. | Lê janela, C6/`.viva` e prova de ação possível. **`tem_pergunta_viva` não é apenas C15:** exige `.viva` e ausência de clique morto. `app/chamados/services/fio.py:171`, `app/chamados/services/fio.py:234`, `app/chamados/services/fio.py:279`. |
| `chamados_visiveis_colab` — `app/chamados/services/janela_colab.py:49` | API `app/api/views.py:1016`; fio `app/chamados/services/fio.py:171`; arquivamento `app/chamados/views_arquivar.py:195` | QuerySet de **chamados** da lista do colaborador. | Deriva janela/destinos/silêncio/módulos; lê **pré-filtro SQL `q_vivas`**, sem C6 exato nesse corpo. `app/chamados/services/janela_colab.py:63`, `app/chamados/services/janela_colab.py:67`. |
| `explicar_pergunta/por_que_nao_fecha` — `app/chamados/services/explicador.py:68`, `app/chamados/services/explicador.py:108` | Explicador em `app/chamados/services/explicador.py:133`; montador em `app/chamados/services/fio.py:308`; modal mostra em `app/templates/chamados/partials/modal_fio.html:432` | **Pergunta:** estado/dono/ação; **disputa:** `pode_fechar/resumo/itens`. | Lê `.estado`, materialização de fato e C15; deriva descrição e ações. `app/chamados/services/explicador.py:72`, `app/chamados/services/explicador.py:93`, `app/chamados/services/explicador.py:137`, `app/chamados/services/explicador.py:163`. |
| `desenlace` — `app/chamados/services/desenlace.py:44` | Fio em `app/chamados/services/fio.py:399`; modal em `app/templates/chamados/partials/modal_fio.html:118` | **Chamado** → verbo, rótulo, estado, ação ou `None`. | Compõe natureza, explicação, filhos e porta própria. `app/chamados/services/desenlace.py:50`, `app/chamados/services/desenlace.py:57`, `app/chamados/services/desenlace.py:62`. |
| `montar_contexto_detalhe` — `app/chamados/services/acoes_chamado.py:327` | `app/chamados/views.py:551` | Detalhe do **chamado** e perguntas vivas. | Lê C6 para filtrar perguntas da disputa aberta. `app/chamados/services/acoes_chamado.py:335`, `app/chamados/services/acoes_chamado.py:348`. |
| `contexto_drawer` — `app/colaboradores/services/drawer.py:17` | `app/colaboradores/views.py:2943` | Banner de motivos pendentes; fallback “N chamados de furo aguardando colaborador”. | Primeiro lê perguntas vivas; fallback **deriva por `VIVOS` e quatro módulos**, sem conferir C6. `app/colaboradores/services/drawer.py:78`, `app/colaboradores/services/drawer.py:88`, `app/colaboradores/services/drawer.py:92`. |
| `relatorios_disputa.classificar/coletar` — `app/chamados/relatorios_disputa.py:11`, `app/chamados/relatorios_disputa.py:26` | Coleta em `app/chamados/relatorios_disputa.py:47`; tela `app/colaboradores/views.py:2997`; command `app/chamados/management/commands/relatorio_pendencias_disputa.py:15` | **Disputa** → sem pergunta, pronta fechar, aguarda colab ou supervisão. | Lê C6; deriva categoria pela existência de resposta. `app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:21`. |
| `perguntas_pendentes_do_colab/aviso_texto_livre` — `app/chamados/services/aviso_pendencia.py:7`, `app/chamados/services/aviso_pendencia.py:16` | Cobrança em `app/chamados/services/cobranca.py:48`; API `app/api/views.py:997`, `app/api/views.py:2220`; web `app/chamados/views.py:1120` | Lista de **perguntas** e aviso com datas que continuam aguardando resposta. | Lê `q_vivas` + C6 + ausência de `respondida_em`; deriva texto. `app/chamados/services/aviso_pendencia.py:13`, `app/chamados/services/aviso_pendencia.py:20`. |
| `checar_cobranca` — `app/chamados/services/cobranca.py:75` | `cobrar_colaborador`, em `app/chamados/services/cobranca.py:155`; referência de intake em `app/core/intake.py:68` | Quantidade de **perguntas**, datas, disputas, prazo e canal. | Lê aviso de pendência; deriva agregados/prazo. `app/chamados/services/cobranca.py:82`, `app/chamados/services/cobranca.py:87`. |
| `_calados/linhas_para_a_pauta` — `app/chamados/services/cobranca.py:221`, `app/chamados/services/cobranca.py:251` | Pauta em `app/escala/services/fase_12x36.py:272`; redução em `app/chamados/services/cobranca.py:257` | **Pessoas** cobradas/caladas e sem canal. | Deriva com `q_vivas`, disputa aberta e pergunta sem resposta; **não chama C6 exato** nesse corpo. `app/chamados/services/cobranca.py:229`, `app/chamados/services/cobranca.py:248`. |
| `pergunta_no_app.retencao/do_dia/medir` — `app/chamados/services/pergunta_no_app.py:65`, `app/chamados/services/pergunta_no_app.py:152`, `app/chamados/services/pergunta_no_app.py:119` | `dia_do_colab:109`; commands `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:14`, `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:21` | Por **chamado/dia**: pergunta no app, causa e dono da retenção. | Lê lista do app, fábrica, ata, cobertura e lista admin; deriva taxonomia. `app/chamados/services/pergunta_no_app.py:77`, `app/chamados/services/pergunta_no_app.py:88`, `app/chamados/services/pergunta_no_app.py:139`, `app/chamados/services/pergunta_no_app.py:165`. |
| `dia_do_colab/_veredito_do_dia` — `app/ponto/services/dia_do_colab.py:6`, `app/ponto/services/dia_do_colab.py:112` | API `app/api/views_mensageria.py:1970`; ficha `app/colaboradores/services/ficha.py:280`; frase em `app/ponto/services/dia_do_colab.py:102` | **(colab,dia)**: marcos, perguntas e frases “Faltou…”/“esperando resposta ou decisão”. | Lê ata, C6 e pergunta no app; **deriva** frase pelos marcos apagados e perguntas vivas. `app/ponto/services/dia_do_colab.py:30`, `app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/dia_do_colab.py:144`, `app/ponto/services/dia_do_colab.py:150`. |
| `ficha_do_colab` — `app/colaboradores/services/ficha.py:99` | Ficha lazy em `app/colaboradores/services/ficha.py:269`; copiloto em `app/colaboradores/services/ficha.py:275`, servido por `app/api/views_mensageria.py:1131` | **Chamados/perguntas** por pessoa, com dono e ações quantificadas. | Lê verbos, perguntas do app e respostas sem veredito; deriva seleção por texto e ações. `app/colaboradores/services/ficha.py:155`, `app/colaboradores/services/ficha.py:166`, `app/colaboradores/services/ficha.py:171`, `app/colaboradores/services/ficha.py:190`. |
| `fila_validar_classificada` — `app/chamados/services/validacao.py:865` | `app/chamados/views_cobrar.py:288`, `app/chamados/views_cobrar.py:349`, `app/chamados/views_cobrar.py:361` | **Perguntas** e **chamados distintos** por classe, idade e total. | Lê fila C6 de respondidas a validar; deriva classes/contagens. `app/chamados/services/validacao.py:874`, `app/chamados/services/validacao.py:882`, `app/chamados/services/validacao.py:884`. |
| `becos.classificar` — `app/chamados/services/becos.py:25` | Command `app/chamados/management/commands/fechar_becos_disputa.py:39`; evento em `app/chamados/services/becos.py:69` | **Disputa** → `None/fechavel/barrada`. | Deriva recorte de beco por abertura, resposta/destino e pai terminal; lê C7 para fechar. `app/chamados/services/becos.py:27`, `app/chamados/services/becos.py:33`. |
| `api_chamados_v2` — `app/api/views.py:975` | Rota `app/api/urls.py:73` | **Chamados** com perguntas da disputa, marcos e flag de regularização. | Monta perguntas a partir de **disputas abertas**, sem filtrar essa coleção por C6; lê projeção por marco separadamente. `app/api/views.py:1029`, `app/api/views.py:1043`, `app/api/views.py:1103`, `app/api/views.py:1158`. |
| `projecao_por_marco/dia_em_regularizacao/marco_aceito_unico` — `app/chamados/services/declaracao_texto.py:376`, `app/chamados/services/declaracao_texto.py:444`, `app/chamados/services/declaracao_texto.py:431` | Fio `app/chamados/services/fio.py:371`; API `app/api/views.py:1103`; wrapper em `app/chamados/services/declaracao_texto.py:501` | **Marco** → aceita/recusa; **dia** → todos recusados; marco único oferecido. | Lê faltantes da célula e identidade/guarda da porta; deriva projeção e reduções. `app/chamados/services/declaracao_texto.py:414`, `app/chamados/services/declaracao_texto.py:423`, `app/chamados/services/declaracao_texto.py:427`, `app/chamados/services/declaracao_texto.py:454`. |
| `montar_contexto_painel` — `app/chamados/services/painel.py:196` | `app/chamados/views.py:727`, `app/chamados/views.py:852`; lavrador `app/core/management/commands/lavrar_badge_navbar.py:39` | Lista/pílulas/contadores de **chamados**. | Lê `qs_na_fila` e partição de verbos; agrega por estado/verbo. `app/chamados/services/painel.py:210`, `app/chamados/services/painel.py:228`. |
| `qs_na_fila/particionar_verbos/creditos_perguntas` — `app/chamados/services/painel.py:158`, `app/chamados/services/verbos.py:30`, `app/chamados/services/raiox.py:49` | Painel acima; alimentação C17 em `app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1290`; consumidores abaixo | **Chamado** → pertença à fila/verbo; crédito de pergunta ao chamado. | Fila deriva `COBRANCA` e silêncio; verbo lê crédito/natureza; crédito usa `q_vivas` e `veredito_lavrado`, **não C6 exato nesse corpo**. `app/chamados/services/painel.py:187`, `app/chamados/services/verbos.py:34`, `app/chamados/services/verbos.py:47`, `app/chamados/services/raiox.py:133`. |
| `coletar_lente` — `app/chamados/services/verbos.py:164` | `app/chamados/views.py:1480` | **Chamados** agrupados da pílula ativa. | Lê partição de verbos e agrega. `app/chamados/services/verbos.py:171`, `app/chamados/services/verbos.py:185`. |
| Navbar `_badge_staff/_badge_registrados/_badge_colab` — `app/core/context_processors.py:59`, `app/core/context_processors.py:131`, `app/core/context_processors.py:185` | `navbar_context`, que publica as lazies em `app/core/context_processors.py:154`, `app/core/context_processors.py:189` | Staff: **chamados que exigem admin**; segundo número: **fila**; colab: **chamados na fila dele**. | Staff lê lavrado, fallback partição e fallback fila; colab lê fila. `app/core/context_processors.py:84`, `app/core/context_processors.py:90`, `app/core/context_processors.py:93`, `app/core/context_processors.py:150`, `app/core/context_processors.py:187`. |
| Dashboard web — `app/core/views.py:253` | Render em `app/core/views.py:271`; badge em `app/templates/core/dashboard_colaborador_v2.html:79` | **Disputas** pendentes do colaborador. | Lê `pendentes_do_colaborador`. `app/core/views.py:253`. |
| `montar_situacional/_led` e `montar_linha_colab` — `app/colaboradores/services/situacional.py:65`, `app/colaboradores/services/situacional.py:56`, `app/colaboradores/painel_cell.py:11` | Telas em `app/colaboradores/views.py:2664`, `app/colaboradores/views.py:2801`; live-cell em `app/colaboradores/painel_cell.py:81` | **Colaborador** → LED vermelho se existe disputa aberta. | **Deriva existência de disputa aberta**, sem exigir pergunta C6 viva. Lote: `app/colaboradores/services/situacional.py:117`; pontual: `app/colaboradores/painel_cell.py:31`; cor: `app/colaboradores/services/situacional.py:58`, `app/colaboradores/painel_cell.py:68`. |
| `montar_painel_operacional` — `app/core/services/painel_op.py:38` | `app/core/views.py:607`, `app/core/views.py:632` | Número de **chamados por colaborador**. | Lê `qs_na_fila` e agrega. `app/core/services/painel_op.py:119`. |
| `api_mensageria_pendencias/sinais` — `app/api/views_mensageria.py:390`, `app/api/views_mensageria.py:427` | Rotas `app/api/urls.py:10`, `app/api/urls.py:11` | **Chamados** com verbo; totais por verbo. | Leem partição e derivam serialização/agregados. `app/api/views_mensageria.py:395`, `app/api/views_mensageria.py:421`, `app/api/views_mensageria.py:453`, `app/api/views_mensageria.py:460`. |
| `fila.por_verbo/idade_e_vazao/gestao` — `app/chamados/services/fila.py:53`, `app/chamados/services/fila.py:88`, `app/chamados/services/fila.py:145` | Redução em `app/chamados/services/fila.py:141`; APIs em `app/api/views_mensageria.py:668`, `app/api

[truncado em 20000 de 48406 caracteres]

## 08/11 10:28 · sessao 02956b4fd4e0 · - · - · origem=Ronald (tmux)

| Sítio confirmado fora de `tests/` | Chamadores de produção confirmados | Unidade e resposta | Deriva ou lê |
|---|---|---|---|
| `app/chamados/services/vigia_fio.py:156` `fio_do_dia` | `censo`, `app/chamados/services/vigia_fio.py:414`, `:417` | Booleano sobre perguntas recebidas: dia tem fio vivo. | Só lê C15 `fio_vivo`, `app/chamados/services/vigia_fio.py:165`. |
| `app/chamados/services/vigia_fio.py:274` `dono_fora_do_fio` | `censo :431`; `censo_alcance :654`, mesmo arquivo | Por `(colab,data)`: decisão pendente, ausência aberta, escala não descreve dia, fio do admin ou `None`; opcional papel. | Compõe, nesta ordem, `oferecido`, cobertura de ausência/ABERTAS, `.estado` + aceite de folga, presença/dono na lista; `app/chamados/services/vigia_fio.py:306`. |
| `app/chamados/services/vigia_fio.py:318` `censo` | `app/chamados/management/commands/vigiar_fio_morto.py:172`; `app/chamados/management/commands/materializar_passivo_validado.py:203` | Contadores e listas de células/dias; universo começa em chamados vivos ligados à célula e módulos de furo, `app/chamados/services/vigia_fio.py:334`; exclui hoje/futuro do alarmado, `:420`. | Lê C15 via `fio_do_dia`, `:417`; lê `dono_fora_do_fio`, `:431`; deriva contadores. |
| `app/chamados/services/vigia_fio.py:542` `censo_alcance` | `app/chamados/management/commands/vigiar_fio_morto.py:173` | Por `(colab,data)` retido: rota, causa, detalhe e papel; `app/chamados/services/vigia_fio.py:625`, `:699`. | Lê `dias_que_seguram :585`, C17 `:635`, dono `:654`; deriva causas sem porta `:667`. |
| `app/chamados/services/vigia_fio.py:222` `ListaDoAdmin.dia_do_admin`; `:230` `dia_na_lista`; `:238` `papel_do_dia` | `dono_fora_do_fio :312`, `:313`; `censo_alcance :661` | Booleano presença com dono BATIDA/CADASTRO; presença+dono; papel do dia. | Lê `cadastro_realidade.lista` e `dias_na_lista`, `app/chamados/services/vigia_fio.py:216`; filtra/traduz `:228`, `:246`. |
| `app/colaboradores/services/calendario.py:97` `resolver_dia_oferecido` | `contexto_calendario`, mesmo arquivo `:769` | Booleano por dia: último recurso, passado, status em `STATUS_QUE_PEDEM_DECISAO`, `:94`, `:103`. | Deriva da projeção do calendário; não considera permissão do observador, `:101`. |
| `app/colaboradores/services/calendario.py:117` `resolver_dia_oferecido_nas_datas` | `app/chamados/services/vigia_fio.py:271` | `{date:bool}`; dia ausente recebe falso, `app/colaboradores/services/calendario.py:135`. | Lê os campos produzidos pelo calendário; `:132`. |
| `app/colaboradores/services/calendario.py:159` `contexto_calendario` | `app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160`; plural `app/colaboradores/services/calendario.py:132` | Lista de dias, incluindo `resolver_dia_oferecido`, `:769`. | Lê C6 para perguntas respondidas vivas, `:476`; compõe status e último recurso e escreve projeção do juiz `:769`. |
| `app/escala/services/cadastro_realidade.py:442` `beco_em_lote` | `dias_folga_nao_definida_sem_porta :439`; `_folga_nao_definida_sem_porta :573`, mesmo arquivo | Lista `(colab_id,assinatura C11)`, com data, dono, perguntas, pai limbo e detalhe; `:530`. | Compõe lâmpada indefinida/protesto `:477`, vivas/C17 `:487`, âncora terminal `:515`, cadastro `:518`, dono `:531`. |
| `app/escala/services/cadastro_realidade.py:787` `lista`; `:1044` `dias_na_lista`; `:985` `um_dono_por_dia` | Tela `app/escala/views.py:487`; vigia `app/chamados/services/vigia_fio.py:216`, `:217`; `lista → um_dono_por_dia :885` | Lista administrativa; mapa `(colab,data) → dono`; uniformização do dono do dia. | Lista incorpora C11 `:864`; uniformização reaproveita C11 `:1009` ou lê `InsumosDoDono.dono :1026`; mapa reduz donos `:1064`. |
| `app/chamados/services/fio.py:160` `montar_chamados_fio` | `app/chamados/views.py:569` colab, `:677` admin, `:889` modal; `contexto_parcial_fio`, `app/chamados/services/fio.py:442` | Lista de chamados, com perguntas e `tem_pergunta_viva`, `:249`, `:279`. | Colab: janela e disputas abertas `:169`, `:213`; admin inclui vivas em fechadas via C6 `:234`; flag combina `.viva` e `raiox.pergunta_sem_acao_possivel`, `:279`. |
| `app/chamados/services/janela_colab.py:49` `chamados_visiveis_colab` | `app/api/views.py:1016`; `app/chamados/services/fio.py:171`; `app/chamados/views_arquivar.py:195` | Queryset de chamados da lista de trabalho. | Deriva recorte janela OU destinos de `q_vivas`, silêncio nulo e módulos não internos; `app/chamados/services/janela_colab.py:61`, `:67`, `:70`. Usa pré-filtro SQL, não C6 exato nesse corpo, `:63`. |
| `app/chamados/services/explicador.py:68` `explicar_pergunta`; `:108` `por_que_nao_fecha` | `por_que_nao_fecha → explicar_pergunta :133`; montador `app/chamados/services/fio.py:308`; exibição `app/templates/chamados/partials/modal_fio.html:432` | Por pergunta: estado, dono e ação, `explicador.py:74`; por disputa: `pode_fechar`, resumo, itens, `:163`. | Lê `.estado :72`, materialização real `:93`; C15 decide `pode`, `:137`; deriva descrição e ações. **É respondedor administrativo**, comprovado pelo template `modal_fio.html:435`–`:437`. |
| `app/chamados/services/desenlace.py:44` `desenlace` | `app/chamados/services/fio.py:399`; exibição `app/templates/chamados/partials/modal_fio.html:118` | Por chamado: `{verbo,rotulo,estado,acao}` ou `None`, `desenlace.py:47`. | Compõe `catalogo.natureza.dono_do_chamado :50`, explicação `:62`, filhos `:57`, módulos/porta própria `:78`. |
| `app/chamados/services/acoes_chamado.py:327` `montar_contexto_detalhe` | `app/chamados/views.py:551` | Detalhe de chamado, perguntas vivas da disputa aberta e evidência, `acoes_chamado.py:335`, `:348`. | Lê C6 `:347` e filtra; não usa C17 nesse percurso. |
| `app/colaboradores/services/drawer.py:15` `contexto_drawer` — confirmar linha de definição antes de usar; trechos verificados `:69`, `:78`, `:88` | `app/colaboradores/views.py:2943` | Banner `falha_texto`: motivos das vivas na última disputa aberta; fallback número de chamados de furo “aguardando colaborador”, `drawer.py:82`, `:92`. | Primeiro lê `perguntas_vivas_de :78`; fallback **deriva independentemente** de `VIVOS` e quatro módulos, sem C6, `:88`–`:92`. |
| `app/chamados/relatorios_disputa.py:11` `classificar`; `:26` `coletar` | `coletar → classificar :47`; tela `app/colaboradores/views.py:2997`; command `app/chamados/management/commands/relatorio_pendencias_disputa.py:15` | Por disputa: SEM_PERGUNTA/PRONTA_FECHAR/AGUARDA_COLAB/AGUARDA_SUPERVISAO, `relatorios_disputa.py:13`; buckets de disputas, `:47`. | Lê C6 `:18`; deriva categoria pela existência de resposta `:21`. |
| `app/chamados/services/aviso_pendencia.py:7` `perguntas_pendentes_do_colab`; `:16` `aviso_texto_livre` | `_pendentes`, `app/chamados/services/cobranca.py:48`; aviso `app/api/views.py:997`, `:2220`; `app/chamados/views.py:1120` | Lista de perguntas vivas sem resposta; aviso por conjunto de datas, `aviso_pendencia.py:13`, `:20`. | Lê `q_vivas` e C6, `:11`, `:13`; deriva texto. |
| `app/chamados/services/cobranca.py:75` `checar_cobranca` | `cobrar`, mesmo arquivo `:155`; também referência dinâmica de intake `app/core/intake.py:68` | Pendentes em perguntas, datas distintas e disputas distintas, `cobranca.py:87`–`:94`. | Lê aviso via `_pendentes :82`; deriva prazo/canal/contagens. |
| `app/chamados/services/cobranca.py:221` `_calados`; `:251` `linhas_para_a_pauta` | `linhas_para_a_pauta → _calados :257`; pauta `app/escala/services/fase_12x36.py:272` | Pessoas cobradas com prazo vencido e pessoas sem canal, `cobranca.py:248`; frases de pendência `:261`. | **Deriva com `q_vivas` SQL**, sem C6 exato nesse corpo, exigindo disputa aberta e pergunta sem resposta `:229`; lê juiz prazo `:246`. |
| `app/ponto/services/dia_do_colab.py:6` `dia_do_colab`; `:112` `_veredito_do_dia` | API `app/api/views_mensageria.py:1970`; ficha `app/colaboradores/services/ficha.py:280`; corpo chama frase `dia_do_colab.py:102` | Um `(colab,data)`: marcos/batidas/perguntas e frase “Faltou…” `:144`, vivas esperando resposta/decisão `:150`. | Lê ata `:30`, C6 `:71`, pergunta_no_app `:109`; frase deriva contagens de marcos/perguntas `:139`–`:154`. |
| `app/chamados/services/pergunta_no_app.py:65` `retencao`; `:152` `do_dia`; `:119` `medir` | `medir → retencao :142`; `do_dia → retencao :164`; `app/ponto/services/dia_do_colab.py:109`; commands `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:14`, `perguntar_chamados_sem_pergunta.py:21`, `:30` | Causa/quem reteve; por dia chamados vivos, perguntas no app, dono da retenção `pergunta_no_app.py:165`; censo por chamado `:145`. | Lê perguntas_do_app `:139`, `:162`, ata `:80`, cobertura `:90`, lista admin `:88`, C6 em `_marco_ja_perguntado :111`; deriva taxonomia de retenção. |
| `app/chamados/services/validacao.py:865` `fila_validar_classificada` | `app/chamados/views_cobrar.py:288`, `:349`, `:361` | Perguntas e chamados distintos por classe, idade, total e rótulo `validacao.py:884`–`:887`. | Lê `perguntas_respondidas_a_validar :874`; deriva classes e contadores. |
| `app/chamados/services/becos.py:25` `classificar` | command `app/chamados/management/commands/fechar_becos_disputa.py:39`; evento `becos.py:69` | Por disputa: `None`, fechável ou barrada, `becos.py:26`. | Deriva aberta, destino e pai terminal `:27`–`:32`; lê C7 `pode_fechar :33`. |

Juízes e leitores que o e2.r incluía, mas o aval não nomeou:

| Função | Chamadores | Resposta e natureza |
|---|---|---|
| `app/chamados/juizes.py:1886` `cobranca_viva_do_dia` | `app/chamados/models.py:373`, `:711`; `app/ponto/services/cartorio.py:1604`; `entrada_sincronizada.py:28`; `triagem_batida.py:589` | Chamado mais novo ou `None`; deriva `VIVOS`, módulo, dia forte e desempate, `juizes.py:1897`–`:1902`. Estes chamadores usam o resultado para planejar/fechar/reconciliar, não são cinco telas novas. |
| `app/chamados/juizes.py:822` `pergunta_viva`, corpo `:1135` | Censo direto extenso é igual ao colado no e2.r; as chamadas adicionais de apresentação estão nas tabelas acima, `explicador.py:72`, `fio.py:280`, `models.py:2502`, `:2809`. | Booleano por pergunta; juiz que deriva vitalidade. |
| `app/chamados/models.py:2492` `.estado`; `:2799` `.viva` | API `app/api/views.py:2569`; explicador `app/chamados/services/explicador.py:72`; vigia `vigia_fio.py:311`; fio `fio.py:280`; também templates `app/templates/chamados/partials/_card_atendimento.html:77`, `:183` | `.viva` delega totalmente a C6, `models.py:2809`; `.estado` lê C6 mas também traduz carimbos/resposta com fallback, `:2499`–`:2506`. |
| `app/chamados/services/fabrica_da_celula.py:74` `motivos_a_perguntar` | `perguntar_pela_celula :164`; `motivos_que_o_evento_deixou :199` | Motivos de perguntas ainda a emitir por célula; deriva deduplicação por motivo `:118`–`:126`, lendo dia_sem_trabalho `:91`, teto `:98`, parcial_so_lista `:102`. Não devolve tarefa completa do dia. |
| `app/chamados/services/fabrica_da_celula.py:191` `motivos_que_o_evento_deixou` | `app/chamados/management/commands/disparar_perguntas_competencia.py:112` | Leitor/vigia dos motivos que evento deixou; lê fábrica `:199`. |
| `app/chamados/models.py:1789` `DisputaSupervisao.pode_fechar` | `becos.py:33`, `:51`; reconciliador `:411`, `:601`, `:1159`; materializacao `:567`, `:1291`; competencia_trancada `:153`, `:202`; disputa_emissao `:1112`, `:1717`; entrada_sincronizada `:58` | Booleano por disputa; compõe C15 e `respostas_sem_veredito`, `models.py:1794`; regra adicional de respostas sem veredito `:1783`–`:1787`. |

Comparação com aval:

| Item do aval | Conferência |
|---|---|
| `vigia_fio.fio_do_dia:156` e `censo_alcance` | Confere; `censo_alcance` está em `app/chamados/services/vigia_fio.py:542`; faltou explicitar `censo:318`, dono `:274` e leitores da lista `:222`. |
| `resolver_dia_oferecido:97` + STATUS `:94` | Confere; único chamador direto é `contexto_calendario:769`; plural é leitura do campo, `calendario.py:135`. |
| C11 `beco_em_lote:442` | Confere função; chamada C17 real é `app/escala/services/cadastro_realidade.py:488`, **não `:460` como colado no e2.r**. A linha 460 é docstring. |
| Nove consumidores C15 “não são respondedores” | Não confere para **explicador**: produz `pode_fechar`, resumo e itens `explicador.py:163`, expostos ao admin `modal_fio.html:432`–`:437`. |
| “TODOS” os leitores na lista | Não confere: montador de fio, painel, relatório, aviso, drawer, pergunta_no_app.do_dia/retenção, dia_do_colab, desenlace, fila de validação e beco de disputa estão documentados acima com seus chamadores. |

O aval acrescenta explicitamente, frente à seção “leitores E3” do e2.r, `resolver_dia_oferecido` e C11, mas ambos já apareciam noutras seções do próprio e2.r colado: juiz “dono fora do fio” e chamadores C17. A identidade da versão da cópia com o HEAD `94d41ea` mencionado no aval é **[nao verificado]** nesta subanálise.

_2869 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
