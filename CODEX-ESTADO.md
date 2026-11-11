# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 20:55** · publicado em **10/11/2026 20:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 20:55 · sessao ce13aba8754d · BO-0611-05 · verificador · origem=Code

A retirada do bloqueio sem GPS está presente; os bloqueios por situação foram preservados antes da cobrança de afastamento e da porta de registro. Os dois kernels têm o mesmo código executável nesse trecho. (`app/api/views_core.py:402`, `app/api/views_core.py:435`, `app/api/views_core.py:658`; `app/api/views.py:415`, `app/api/views.py:448`, `app/api/views.py:666`)

A comparação com `6c0517eb9` abaixo usa o diff fornecido: esta cópia não contém histórico Git para conferir a base e a identidade do hash independentemente. **[nao verificado]**

1. **Recusas antes de `registrar_batida`**

   | Recusa atual | Core | UI | O que lê |
   |---|---|---|---|
   | 404, `Colaborador não encontrado.` | `app/api/views_core.py:391` | `app/api/views.py:404` | Acesso a `request.user.colaborador`; qualquer exceção nesse acesso cai no 404. |
   | 403, situação não permitida | `app/api/views_core.py:410` | `app/api/views.py:423` | `colab.situacao`; permite somente `ativo` e `afastado`. Escolhe mensagem/código para `desligado`, `ferias` ou outro valor. |
   | 400, `Tipo inválido.` | `app/api/views_core.py:426` | `app/api/views.py:439` | `request.data['tipo']`, obtido por `.get`; permite somente `E` e `S`. |

   Pelo diff da obra, os três retornos acima mantiveram posição, condição e texto da base. Saiu somente a guarda `if not latitude or not longitude`, com seu log `batida_recusada_sem_gps` e resposta `gps_obrigatorio`: na base fornecida, core condição em `app/api/views_core.py:440`, retorno em `:454`; UI condição em `app/api/views.py:449`, retorno em `:462`. O lugar passou a conter o comentário de encaminhamento à porta. (`app/api/views_core.py:435`; `app/api/views.py:448`; conferência independente da base **[nao verificado]**)

   Não houve deslocamento da guarda de situação em relação à cobrança: ela continua antes de `afastado_na_batida`. A volta 9 restaurou esse bloco em relação à volta 8; em relação à base fornecida, preservou-o. (`app/api/views_core.py:402`, `:411`; `app/api/views.py:415`, `:424`; comparação histórica independente **[nao verificado]**)

   Há também retorno antecipado `ja_registrada`, mas sem status de erro: ele não pertence ao conjunto solicitado de recusas ≥400. A colisão `ignorada_intervalo` acontece depois da chamada à porta. (`app/api/views_core.py:535`, `:658`, `:686`; `app/api/views.py:548`, `:666`)

   A proteção `IsAuthenticated` envolve as duas views; a tabela inventaria os retornos explícitos dentro delas, não as respostas produzidas pela autenticação do framework. (`app/api/views_core.py:385`; `app/api/views.py:398`)

2. **Ordem dos efeitos**

   | Ponto de saída | O que ocorreu antes |
   |---|---|
   | 404 | Tentativa de obter o colaborador; nenhum caminho de cobrança ou registro foi chamado pelo corpo da view. (`app/api/views_core.py:388`; `app/api/views.py:401`) |
   | 403 por situação | Obtenção do colaborador e leitura de situação; ainda não houve leitura do payload, cobrança de afastamento, detector de anomalias ou registro. (`app/api/views_core.py:402`, `:411`, `:419`; `app/api/views.py:415`, `:424`, `:432`) |
   | 400 por tipo | Já houve consulta ao juiz de afastamento e, quando positiva, tentativa de cobrar a supervisão; depois são lidos tipo, coordenadas, precisão e foto. (`app/api/views_core.py:411`, `:419`, `:425`; `app/api/views.py:424`, `:432`, `:438`) |

   A cobrança pode escrever **antes do 400**: procura Pauta viva; pode obter/criar o usuário sistema; cria Pauta, grava trilha e atualiza contador. Falha na cobrança é registrada e engolida. (`app/ponto/services/afastado_avisa.py:62`, `:71`, `:74`, `:80`; `app/core/utils.py:18`; `app/pautas/services.py:180`, `:186`, `:188`, `:352`; `app/core/observ.py:134`)

   Portanto, `situacao='ativo'` também pode gerar essa cobrança se uma ausência de afastamento cobrir o dia; `situacao='afastado'` sem ausência gera a divergência declarada. Coordenadas não participam dessa decisão. (`app/ponto/services/afastado_avisa.py:38`, `:41`; `app/ponto/turnos.py:2715`)

   Para tipo válido:

   | Situação | Com coordenadas | Sem coordenadas |
   |---|---|---|
   | `ativo` / `afastado` | Seguem cobrança, detector, demais preparações e porta. (`app/api/views_core.py:411`, `:433`, `:658`) | Agora seguem o mesmo percurso; o geofence é chamado depois da escrita. (`app/api/views_core.py:435`, `:658`, `:788`) |
   | `desligado` / `ferias` / outro | 403 antes desses efeitos. (`app/api/views_core.py:402`) | Mesmo 403; GPS nem é lido antes dele. (`app/api/views_core.py:410`, `:420`) |

   Pela base fornecida, a diferença está em `ativo`/`afastado` sem GPS: antes, cobrança e detector já tinham rodado; depois vinha o log da tentativa recusada e o 403. Agora esse log saiu e o fluxo continua. As demais ordens acima permaneceram. (`app/api/views_core.py:411`, `:433`, `:435`; `app/core/auditoria.py:22`; comparação histórica independente **[nao verificado]**)

3. **Quem escreve e quem lê**

   **Latitude/longitude da Batida:** as views capturam e encaminham; a porta sanitiza vazio/inválido para `None` e cria a Batida. Não inventa a coordenada faltante nem converte zero em uma coordenada alternativa. (`app/api/views_core.py:420`, `:664`; `app/api/views.py:433`, `:672`; `app/ponto/registro_batida.py:5`, `:105`, `:152`, `:157`)

   As escritas de coordenadas no geofence são de **AlertaGeofence**, não uma segunda escrita da Batida. O contexto do chamado também recebe cópias das coordenadas. (`app/ponto/services/geofence.py:271`, `:297`, `:304`, `:349`, `:391`)

   | Leitor afetado | Regra atual |
   |---|---|
   | Geofence | Consulta `tem_gps`, cujo resultado é `bool(latitude and longitude)`. (`app/ponto/services/geofence.py:145`, `:269`) |
   | Feed e offline | Filtro delega ao juiz. (`app/core/templatetags/hasner_filters.py:34`; `app/templates/core/_d2_feed.html:16`; `app/templates/ponto/batidas_offline_pendentes.html:96`) |
   | Relatório e modal | `par_tem_gps` delega ao mesmo juiz. (`app/core/templatetags/hasner_filters.py:41`; `app/templates/chamados/partials/_relatorio_geo.html:31`; `app/templates/chamados/partials/modal_fio.html:481`) |
   | Contador offline | Conta pares aprovados pelo juiz. (`app/ponto/views.py:485`) |
   | Três consultas de vínculo | Exigem não nulidade das duas coordenadas; **aceitam zero**, divergindo do juiz. (`app/colaboradores/views.py:337`, `:356`, `:395`) |
   | Detector de vínculo | Exige não nulidade e precisão; **aceita zero** como amostra. (`app/ponto/services/vinculo_na_lista.py:49`, `:56`; `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135`) |

   Restam também regra própria no JS do dashboard, rejeição apenas de NaN no mapa e conversão individual de zero para `None` no emissor do contexto. São comportamentos diferentes entre si; não são consultas ao juiz. (`app/templates/core/dashboard2.html:242`, `:287`; `app/static/js/geo-colab-map.js:39`; `app/ponto/services/geofence.py:349`, `:391`)

   Esses remanescentes estão expressamente fora da migração desta volta no corte registrado; sua inscrição efetiva na fila externa não está disponível nesta cópia. (`app/docs/CORTES.md:442`; registro externo **[nao verificado]**)

   **Situação:** os kernels leem para barrar; o detector de vínculo exclui desligados e seleciona colegas ativos. Essas funções não escrevem situação. (`app/api/views_core.py:402`, `:407`; `app/api/views.py:415`, `:420`; `app/ponto/services/vinculo_na_lista.py:71`, `:83`, `:152`)

   Considerando os arquivos tocados inteiros, há escritores anteriores: cadastro cria `ativo`; lançamento de afastamento escreve `afastado`, ambos pela porta de Colaborador. O reversor escreve a volta para ativo pela mesma porta. (`app/colaboradores/views.py:1182`, `:1205`; `app/ponto/views.py:2435`; `app/colaboradores/services/colaborador.py:55`, `:91`; `app/ponto/management/commands/reverter_situacao_afastado.py:73`)

   Outros leitores nesses arquivos incluem refresh/me, universo de vínculo/geo e filtros de telas. (`app/api/views_core.py:173`, `:344`; `app/api/views.py:116`, `:355`; `app/ponto/services/geofence.py:46`; `app/ponto/views.py:376`, `:1119`, `:2225`, `:2373`; `app/colaboradores/views.py:206`, `:602`, `:1043`, `:1414`, `:1748`, `:2610`)

   Nos testes novos, a situação é preparada na criação do colaborador ou por UPDATE explícito para férias/desligado; isso é fixture, não escritor novo de produção. (`app/api/tests/test_l030_batida_pela_porta.py:33`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:518`, `:679`, `:756`)

4. **Mordida dos testes da porta** [nao verificado]

   O analisador AST reconhece somente `if` cuja condição leia os campos nominais procurados e contenha `return Response` com status literal 403 ou `HTTP_403_FORBIDDEN`, antes da primeira chamada nominal `registrar_batida`. Não acompanha aliases nem chamadas auxiliares. (`app/api/tests/test_l030_selo_porta.py:30`, `:50`, `:66`)

   | Teste | Pega | Não pega | [nao verificado]
   |---|---|---|
   | `PortaSemRecusaTest.test_MORDE_os_dois_kernels` | Guarda GPS reconhecida, inclusive `return` dentro de `try`. | Outro campo, status 400/422, helper, alias `sem_gps`, recusa por exceção; `raise` no `if` com resposta em `except` separado. (`app/api/tests/test_l030_selo_porta.py:104`, `:50`, `:66`) |
   | `PortaSemRecusaTest.test_mutantes_em_memoria_com_controle_verde` | Cinco formas: nomes diretos, `.get`, subscrito, `getattr`, condição mista situação/GPS. | Não executa os ramos mutados; não prova alcançabilidade nem formas não enumeradas. (`app/api/tests/test_l030_selo_porta.py:109`) |
   | `PortaSemRecusaTest.test_prosa_nao_e_barreira` | Evita falso positivo de comentário/docstring. | Não testa comportamento da produção; esse não é seu objeto. (`app/api/tests/test_l030_selo_porta.py:122`) |
   | `BarreiraDeSituacaoDeclaradaTest.test_MORDE_exatamente_uma_guarda_declarada_em_cada_kernel` | Guarda reconhecida removida/duplicada ou expressão/conjunto alterado. | Segunda recusa em helper, outro status ou exceção; ordem frente à cobrança; semântica do corpo. Mover a única guarda para helper reprova por ausência; acrescentar helper mantendo-a pode escapar. (`app/api/tests/test_l030_selo_porta.py:136`, `:160`) |
   | `…test_MORDE_os_codigos_da_barreira_sao_os_tres_da_base` | Falta de literal esperado ou presença de `colab_afastado`. | Associação código/situação trocada; mensagens erradas; código novo fora do conjunto comparado; literais preservados sem uso. (`app/api/tests/test_l030_selo_porta.py:165`) |
   | `BarreiraDeSituacaoDeclaradaTest.test_mutantes_em_memoria_com_controle_verde` | Quatro guardas adicionais, conjunto menor/maior e remoção. | Mesmos limites do analisador; não executa produção mutada. (`app/api/tests/test_l030_selo_porta.py:176`) |

   O teste dos códigos pode continuar verde com os literais numa docstring ou em código morto dentro da guarda: ele coleta constantes de string, não respostas efetivas. Já o selo GPS não fica verde simplesmente porque perdeu a porta: existe uma asserção exigindo encontrá-la. (`app/api/tests/test_l030_selo_porta.py:170`, `:70`)

   | Teste HTTP de `BatidaPelaPortaTest` | Pega | Não pega | [nao verificado]
   |---|---|---|
   | Sem coordenadas, ativo/afastado | Recusa que atinja essas fixtures, por return/helper/try/exceção; fallback; porta ausente/duplicada; persistência errada. | Recusa condicionada a campos, horários ou cadastros não exercitados; ordem de Pauta/log. (`app/api/tests/test_l030_batida_pela_porta.py:47`, `:59`) |
   | Desligado/férias/outro barrados | Status/código errado, porta chamada ou Batida gravada, com e sem GPS. | Helper equivalente; Pauta/log indevidamente anteriores ao 403; outras situações não ensaiadas. (`app/api/tests/test_l030_batida_pela_porta.py:65`) |
   | Zero numérico | Zero barrado/reescrito e juiz passando a aceitá-lo. | Zero textual ou somente uma coordenada zero. (`app/api/tests/test_l030_batida_pela_porta.py:78`) |
   | Campos vazios | Recusa, conversão antecipada ou persistência diferente de nulos. | Outros inválidos; apesar do nome “formulário”, envia JSON. (`app/api/tests/test_l030_batida_pela_porta.py:44`, `:88`) |
   | Coordenada parcial | Latitude isolada barrada/modificada ou longitude inventada. | Longitude isolada; resultado posterior do geofence. (`app/api/tests/test_l030_batida_pela_porta.py:93`) |

   Os caminhos aceitos usam `wraps` na função real e conferem chamada, banco e ID; não passam por um mock que fabrica sucesso. A autenticação é forçada, portanto login/refresh ficam fora. (`app/api/tests/test_l030_batida_pela_porta.py:40`, `:42`, `:49`)

   Nesta revisão, os seis testes AST passaram sem banco; os testes HTTP não foram reexecutados. O OK de `api ponto` foi informado pelo solicitante, sem log disponível para conferência independente. (`app/api/tests/test_l030_selo_porta.py:99`, `:133`; execução HTTP e log dos módulos **[nao verificado]**)

5. **Bateria férias/desligado**

   | Caso/teste | Caminho e afirmação atual | [nao verificado]
   |---|---|
   | Relógio congelado | Só afirma a data congelada. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:691`) |
   | “Controle paga…” | Só compara constantes `MEDIDO`; isoladamente não demonstra que produção alcança dinheiro. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:694`) |
   | C0 | API, tabela inteira: janela ligada, 8 h e 120 min pendentes; desligada, 10 h e HE50 de 2 h. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:704`, `:82`, `:118`, `:663`) |
   | F1 | Ausência de férias com situação **ativo**, pela API; tabela inteira. Ligada: 8 h; desligada: 10 h, HE50 de 2 h e reflexo DSR de 12 h; sete dias/56 h de abono. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:711`, `:156`, `:193`, `:209`, `:216`) |
   | F2 | Ausência de férias com situação **ativo**, folga, pela API; tabela inteira. Ligada: pendência de 600 min e sem pagamento do dia; desligada: 10 h de folga trabalhada; oito dias/64 h de abono. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:721`, `:239`, `:246`, `:276`, `:292`) |
   | F1s/F2s | Situação alterada para férias: primeiro 403/zero Batida nos dois kernels; depois porta real, comparando APURACAO de F1/F2 e idempotência. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:731`, `:740`, `:567`, `:665`) |
   | D1 | Célula preservada após desligamento por UPDATE; API barra, depois porta real. APURACAO espera 10 h, célula concorde/600 min, sem HE. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:749`, `:311`, `:322`) |
   | D2 | Desligado dentro da competência, sem célula; API barra, depois porta real. APURACAO espera 10 h em DiaPago/fechamento. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:762`, `:373`, `:384`) |
   | D3 | Demissão anterior à competência; API barra, depois porta real. Quatro batidas/600 min de relógio, sem DiaPago ou fechamento. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:769`, `:428`, `:434`, `:440`) |

   As quatro classes cobrem os dois kernels e janela ligada/desligada. O plantio usa `registrar_batida` real; a apuração chama cartório, fechamento e espelho reais. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:576`, `:593`, `:603`, `:778`)

   **Deixaram de ser confrontados com MEDIDO:** `porta` e valores de `admin` para D1/D2/D3; também não são confrontados para as variantes F1s/F2s. F1/F2 ativos continuam conferindo esses valores. O `admin` ainda participa da idempotência entre duas apurações, mas isso não afirma seus valores históricos. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:52`, `:656`, `:663`, `:665`, `:674`)

   O 403 da bateria exige código e zero Batida, mas não exige porta sem chamadas nem ausência de outros efeitos. `_numeros` omite zeros/nulos; o resumo omite valores falsy. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:567`, `:480`, `:635`)

   Não encontrei número **atualmente afirmado** demonstravelmente impossível por análise estática: D1/D2 têm percurso compatível nos dias órfãos do fechamento. Reproduzir cada número nesta árvore permanece **[nao verificado]**. Os antigos HTTP 200 dos desligados continuam na tabela, mas não são mais expectativas executadas nesses casos. (`app/ponto/services/fechamento.py:838`, `:902`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:328`, `:665`)

6. **Sobras e textos contraditórios**

   - “Única exceção… desligado” é falso: férias e outro valor também barram. É comentário restaurado da base fornecida, não prova de retirada esquecida. (`app/api/views_core.py:431`, `:402`; `app/api/views.py:444`, `:415`)
   - O cabeçalho de homologação diz afastado bloqueado, contrariando o próprio teste que exige 200; a descrição “férias bate” também é imprecisa para o caso que prepara situação férias e exige recusa. (`app/api/tests/test_api_bater_ponto.py:6`, `:28`, `:67`, `:84`, `:88`)
   - Textos amplos “férias nunca recusa” precisam distinguir **ausência de férias** de **situação férias**. Não demonstram ramo restante que libere a situação barrada: há testes de ausência com cadastro ativo e de chamada direta à porta. (`app/ponto/tests/test_batida_sideeffects.py:173`; `app/ponto/services/aviso_da_batida.py:7`; `app/ponto/tests/test_ferias_dia21.py:38`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:712`)
   - LEIS está coerente com os 403 mantidos. MAPA e MAPA-JUIZES registram `tem_gps`; o lookup JUIZES ainda não o inclui. (`app/docs/LEIS.md:78`, `:165`; `app/docs/MAPA.md:246`; `app/docs/MAPA-JUIZES.md:19`; `app/docs/JUIZES.md:19`)
   - Não encontrei sobra executável da liberação de desligado/férias nos kernels; ambos têm a guarda restaurada. Exaustividade semântica de todo texto da árvore **[nao verificado]**. (`app/api/views_core.py:402`; `app/api/views.py:415`)

7. **Catraca de linhas**

   Bate: UI registrada **728**, ocorrência real **728**; core registrada **723**, ocorrência real **723**. O teste específico de igualdade da catraca também passou em memória. (`app/core/tests/test_medida_turno_do_par.py:256`, `:257`, `:539`; `app/api/views.py:728`; `app/api/views_core.py:723`)

8. **Classes que já reprovaram obra**

   | Classe | Resultado |
   |---|---|
   | Segundo juiz | Os leitores migrados delegam; os leitores próprios remanescentes estão discriminados no item 3 e adiados pelo corte. (`app/core/templatetags/hasner_filters.py:34`, `:41`; `app/docs/CORTES.md:442`) |
   | Segundo escritor | O censo executado encontrou zero escritas **atribuíveis** da família Batida fora da porta; isso conserva os limites de atribuição do próprio censo. (`app/core/censo_escritas.py:6`, `:43`; `app/ponto/tests/test_chokepoint_batida_gate.py:26`) |
   | Relógio fora da borda | Ainda existem leituras internas: `date.today()` no afastamento e relógio no detector. Não são introduções mostradas pelo diff fornecido. (`app/ponto/turnos.py:2730`; `app/chamados/detector_anomalias.py:73`; `app/ponto/utils_turno_orfao.py:22`) |
   | Derivação paralela | Permanece nos leitores de coordenada discriminados; não apareceu nos novos filtros delegadores. (`app/colaboradores/views.py:337`; `app/core/templatetags/hasner_filters.py:34`) |
   | Selo/RED sem mordida | Há mordida para formas e fixtures enumeradas; não há cobertura universal de recusa. O “anti-vacuidade” da bateria só lê constantes. (`app/api/tests/test_l030_selo_porta.py:66`; `app/api/tests/test_l030_batida_pela_porta.py:59`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:694`) |
   | Ordem de trava | Situação continua antes da cobrança; os testes HTTP não selam ausência de Pauta/log antes do 403. (`app/api/views_core.py:402`, `:411`; `app/api/tests/test_l030_batida_pela_porta.py:73`) |
   | Pouso antes de pré-requisito | A regra exige suite inteira verde e depois verificação final. Só os módulos foram informados como OK; cumprimento final **[nao verificado]**. (`CLAUDE.md:374`) | [nao verificado]
   | Campo com dois significados | Ausência de férias e `situacao='ferias'` têm efeitos distintos; a bateria agora o

[truncado em 20000 de 21968 caracteres]

## 10/11 20:53 · sessao 892be02b5445 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura B1–B4 entregue, com comandos, limites e pergunta h2 sem resposta no [relato](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/RELATO_BO_DISPUTA_VOLTA5B.txt:1).

[Execuções locais registradas](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/RELATO_BO_DISPUTA_VOLTA5B_VERIFICACOES.txt:1): 66 testes em memória, 35 da catraca e 20 mutantes mortos. Execução ORM e tetos SQL: [nao verificado]. [nao verificado]

perguntas de lei: 1

## 10/11 20:52 · sessao 67fdd29f7bf8 · - · - · origem=Ronald (tmux)

Itens 3, 6, 8 e 9 — achados e censo:

- Juiz único implementado: `app/ponto/services/geofence.py:126` define `tem_gps`; `:145` devolve `bool(latitude and longitude)`; `:269` é o consumidor no geofence. O registro vivo contém a autoridade em `app/core/juizes.py:1201`, com corte em `app/docs/CORTES.md:442`.
- Leitores migrados delegam: `app/core/templatetags/hasner_filters.py:34` e `:41`; templates `app/templates/core/_d2_feed.html:16`, `app/templates/ponto/batidas_offline_pendentes.html:96`, `app/templates/chamados/partials/_relatorio_geo.html:31`, `app/templates/chamados/partials/modal_fio.html:481`; contador em `app/ponto/views.py:485`.
- Permanecem leitores com regra própria: nulidade em `app/colaboradores/views.py:337`, `:356`, `:395` e `app/ponto/services/vinculo_na_lista.py:49`; também `app/ponto/services/geofence.py:52`. Aceitam zero numérico onde `tem_gps` nega. A autorização de manter leitores do censo fora da migração está documentada em `app/docs/CORTES.md:442`; a presença efetiva deles no BACKLOG externo é `[nao verificado]`.
- `app/ponto/services/geofence.py:349`, `:350`, `:391`, `:392` transformam cada zero em `None` ao formar contexto. `app/templates/core/dashboard2.html:242` e `:287` repetem truthiness; `app/static/js/geo-colab-map.js:39` só rejeita NaN, aceitando zero. São dívidas existentes, não alterações demonstradas no diff fornecido.
- Um teste novo fixa explicitamente zero como amostra numérica no detector: `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135`–`:140`. Isso diverge do juiz, mas o ramo de produção correspondente já aceitava zero pelo filtro de nulidade; o diff fornecido acrescenta apenas a exigência de longitude. Não atribuir introdução dessa regra à obra.
- Escritor de `Batida.latitude/longitude`: `app/ponto/registro_batida.py:105` sanitiza e `:152`/`:157` criam com `**campos`; a sanitização converte vazio/inválido em `None`, preservando zero em `:5`–`:18`. Kernels só capturam e encaminham: `app/api/views_core.py:420`, `:421`, `:664`, `:665`; `app/api/views.py:433`, `:434`, `:672`, `:673`. Escritas do geofence em `:275`, `:297`, `:304`, `:315`, `:324` são de **AlertaGeofence**, não Batida.
- Situação, nas funções alteradas: kernels leem em `app/api/views_core.py:402`, `:407`, `:408` e `app/api/views.py:415`, `:420`, `:421`; detector lê em `app/ponto/services/vinculo_na_lista.py:71`, `:83`, `:152`. Nenhuma dessas funções escreve situação.
- Situação, considerando arquivos inteiros tocados: `app/colaboradores/views.py:1182` cria colaborador com `situacao='ativo'` em `:1205`, pela porta `app/colaboradores/services/colaborador.py:91`–`:93`; `app/ponto/views.py:2435` escreve afastado pela porta `app/colaboradores/services/colaborador.py:55`–`:61`. A bateria escreve férias diretamente por UPDATE em `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:679` e desligado em `:758`; são preparação de teste.
- Leitores adicionais de situação nos arquivos tocados: refresh/me em `app/api/views_core.py:173`, `:344`, `app/api/views.py:116`, `:355`; geofence em `app/ponto/services/geofence.py:46`; filtros e contadores em `app/ponto/views.py:376`, `:1119`, `:1120`, `:1121`, `:2225`, `:2373`; lista/cadastro/vínculo em `app/colaboradores/views.py:33`, `:62`, `:66`, `:206`, `:602`, `:619`, `:884`, `:1043`, `:1045`, `:1414`, `:1448`, `:1540`, `:1689`, `:1748`, `:1770`, `:1831`, `:2610`, `:2846`, `:2856`, `:2896`, `:2912`.

Sobras:

- Comentário **falso, restaurado da base**: “Única exceção … desligado” em `app/api/views_core.py:431` e `app/api/views.py:444`; férias e outro valor também barram em `:402`/`:415`.
- Cabeçalhos antigos dizem afastado bloqueado em `app/api/tests/test_api_bater_ponto.py:6` e `:28`, contrariando o próprio teste `:67`–`:75`. `:84`–`:85` diz genericamente que férias bate, embora esse caso específico use situação férias e espere recusa em `:86`–`:90`.
- Textos amplos “nunca recusa” permanecem em `app/ponto/tests/test_batida_sideeffects.py:173`, `app/ponto/services/aviso_da_batida.py:7` e `app/ponto/tests/test_gate_ferias_vira_aviso.py:7`. Não demonstram remoção esquecida: férias como ausência com cadastro ativo continuam entrando (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:712`, `:722`); a porta direta também aceita (`app/ponto/tests/test_ferias_dia21.py:38`–`:42`).
- `app/docs/LEIS.md:78` e `:165` estão coerentes com as barreiras mantidas.
- `app/docs/JUIZES.md:19`–`:121` continua sem `tem_gps`, embora `app/docs/MAPA-JUIZES.md:19` e `app/docs/MAPA.md:246` já o registrem. O opus fornecido já condicionava corrigir esse lookup no pouso.
- MAPA-DA-FILA-BO e `logs/MAPA-FILA-BO/mapa.r` não estão disponíveis nesta cópia: registro do complemento `[nao verificado]`. `app/docs/BACKLOG.md:1873` ainda descreve a obra original em censo; isso não prova ausência do registro externo.

Proibidos: não observei introdução de fallback, nova recusa, mudança do classificador ou aceitação de zero no juiz; o comportamento visível é encaminhar coordenadas à porta e consultar o mesmo predicado (`app/api/views_core.py:664`, `app/api/views.py:672`, `app/ponto/services/geofence.py:145`, `:269`, `:292`). A integridade de `motor_calculo_v2.py` contra a base depende do diff fornecido, pois esta cópia não contém `.git`: comparação independente `[nao verificado]`.

## 10/11 20:52 · sessao 96e8efe466d2 · - · - · origem=Ronald (tmux)

Item 4 — leitura estática e execução sem banco:

| Teste | O que morde | O que não morde | [nao verificado]
|---|---|---|
| `PortaSemRecusaTest.test_MORDE_os_dois_kernels` | `if` anterior à porta que lê `latitude`/`longitude` e contém `return Response(...,status=403)`; inclusive dentro de `try`. | Outro campo, status400/422, status em variável, alias `sem_gps`, função auxiliar, `raise PermissionDenied`, ou return no `except` separado do `if`. O detector só reconhece essas formas nominais. (`app/api/tests/test_l030_selo_porta.py:50-79`, `:104-107`) |
| `PortaSemRecusaTest.test_mutantes_em_memoria_com_controle_verde` | Cinco mutantes AST por kernel: nomes diretos, `.get`, subscrito, `getattr`, condição mista férias/GPS. | Não executa a view mutada; prova a sensibilidade do analisador a essas cinco formas, sem provar alcançabilidade dos ramos. (`app/api/tests/test_l030_selo_porta.py:109-120`) |
| `PortaSemRecusaTest.test_prosa_nao_e_barreira` | Evita falso positivo em docstring/comentário do exemplo. | Não verifica produção nem comportamento de recusa. Não é vacuidade: seu objeto declarado é distinguir prosa de AST executável. (`app/api/tests/test_l030_selo_porta.py:122-130`) |
| `BarreiraDeSituacaoDeclaradaTest.test_MORDE_exatamente_uma_guarda_declarada_em_cada_kernel` | Remoção da guarda reconhecida, segunda guarda reconhecida ou mudança da expressão canônica/conjunto. | Segunda recusa escondida em helper, alias, exceção, status diferente; ordem relativa a cobrança/pauta; semântica do corpo. Mover toda a guarda para auxiliar faz faltar a guarda e reprova; acrescentar auxiliar mantendo a guarda pode escapar. (`app/api/tests/test_l030_selo_porta.py:66-90`, `:136-163`) |
| `...test_MORDE_os_codigos_da_barreira_sao_os_tres_da_base` | Ausência de qualquer um dos três literais esperados e presença de `colab_afastado` na guarda. | Troca da associação código/situação, mensagem errada, código novo fora do pequeno conjunto comparado, literais esperados conservados em código morto. Verifica conjunto de strings, não saída real. (`app/api/tests/test_l030_selo_porta.py:165-174`) |
| `BarreiraDeSituacaoDeclaradaTest.test_mutantes_em_memoria_com_controle_verde` | Quatro segundas guardas diretas, conjunto menor/maior, guarda removida. | Os mesmos escapes do detector; não executa os mutantes de produção. (`app/api/tests/test_l030_selo_porta.py:176-194`) |
| `BatidaPelaPortaTest.test_MORDE_sem_coordenadas_ativo_e_afastado_gravam_nos_dois_kernels` | Reintroduzir recusa que atinja ativo/afastado sem GPS, seja por return, helper, try ou exceção; fallback/coordenada alterada antes da porta; porta não chamada/duplicada; ausência ou duplicação da batida. | Condições de recusa que não incidam na fixture — aparelho, valores adicionais, foto presente, outro horário etc.; efeitos anteriores como Pauta/log. (`app/api/tests/test_l030_batida_pela_porta.py:31-63`) |
| `...test_MORDE_desligado_ferias_e_outro_seguem_barrados_antes_da_porta_nos_dois_kernels` | Remover barreira, trocar status/código, chamar porta antes de recusar, gravar Batida, com e sem GPS, nas três situações. | Mover a recusa para helper conservando resultados; criar Pauta/log antes do403; nova barreira para outra situação/fixture. (`app/api/tests/test_l030_batida_pela_porta.py:65-76`) |
| `...test_MORDE_zero_numerico_chega_intacto_ao_escritor` | Barrar/reescrever zero, perder persistência, juiz considerar zero válido. | Zero textual, só uma coordenada zero, outras combinações/condições não exercitadas. (`app/api/tests/test_l030_batida_pela_porta.py:78-86`) |
| `...test_MORDE_campos_vazios_do_formulario_sao_sanitizados_pela_porta` | Barrar vazios, convertê-los antes da porta, não gravá-los como `None`. | Outros valores inválidos e regras para outros formatos de request; envia JSON apesar do nome “formulário”. (`app/api/tests/test_l030_batida_pela_porta.py:44`, `:47-55`, `:88-91`) |
| `...test_caracteriza_coordenada_parcial_sem_inventar_a_outra` | Barrar latitude isolada ou inventar longitude; modificar latitude antes da porta. | Longitude isolada e geofence posterior; testa só ativo e uma orientação do par. (`app/api/tests/test_l030_batida_pela_porta.py:93-96`) |

Os cinco testes HTTP usam `wraps=registrar_batida`, POST real e checam persistência/ID nos caminhos aceitos; não passam por “mock que fabricou batida”. A autenticação é forçada, então login/refresh e middleware de autenticação não são cobertos. (`app/api/tests/test_l030_batida_pela_porta.py:39-56`)

O selo AST pode dar verde com barreira real escondida: demonstrei em memória os escapes acima. O próprio algoritmo explica a limitação: não acompanha fluxo entre funções nem aliases e só busca retorno403 dentro de guarda `if` cuja condição leia campo nominal. (`app/api/tests/test_l030_selo_porta.py:50-79`)

Executado: seis testes AST, `OK`, rc0; não executei os testes HTTP com banco nesta tarefa. `[nao verificado: execução HTTP nesta revisão]` (`app/api/tests/test_l030_selo_porta.py:99`, `:133`)

Item 7 — bate:

- `api/views.py`: registrada728, ocorrência real728. (`app/core/tests/test_medida_turno_do_par.py:256`; `app/api/views.py:728`)
- `api/views_core.py`: registrada723, ocorrência real723. (`app/core/tests/test_medida_turno_do_par.py:257`; `app/api/views_core.py:723`)
- Executei em memória o teste específico de igualdade de linhas da árvore inteira: um teste, `OK`, rc0. A asserção compara todo `LINHAS_REGISTRADAS` com o censo real. (`app/core/tests/test_medida_turno_do_par.py:20-28`, `:539-543`)

Item 8 — limite legado adicional: selo modal verifica apenas presença textual de `naoBloquear` quando encontra `hxConfirmar(`; comentário com a palavra já satisfaz essa parcela. Não demonstra que o fluxo efetivamente segue quando modal falha. (`app/core/tests/test_selo_modal_nao_nasce_vazio.py:135-143`)

## 10/11 20:52 · sessao efb82ef2f5e2 · - · - · origem=Ronald (tmux)

Item 5 — auditoria estática, sem banco e sem executar a bateria.

O arquivo declara caracterização do efeito de retirar as barreiras, não uma regra de pagamento; a tabela veio da volta 8 e os casos barrados agora usam a função real de registro. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:3`, `:11`, `:576`)

| Teste | Caminho e afirmações atuais | [nao verificado]
|---|---|
| `test_relogio_congelado` | Só verifica a data congelada; nenhuma batida ou cálculo. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:691`) |
| `test_MORDE_o_controle_paga_e_os_casos_nao_sao_o_controle` | Só compara constantes da própria tabela: C0 ligado = 8 h/120 min pendentes; desligado = 10 h/2 h HE50; casos diferentes do controle; flags históricas de idempotência verdadeiras. Não chama produção e sozinho não prova que a fixture alcança dinheiro. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:694`) |
| C0 | Ativo, sem ausência, célula de trabalho; quatro POSTs pela API e comparação integral com MEDIDO. Espera 8 h e pendência de 120 min com janela ligada; 10 h/HE50 2 h com janela desligada. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:704`, `:661`, `:82`, `:118`) |
| F1 | Ausência de férias lançada mantendo `situacao='ativo'`; célula existe; quatro POSTs pela API e tabela integral. Espera `fato_em_ausencia`, 8 h com janela ligada; 10 h/HE50 2 h e reflexo DSR 12 h com janela desligada; sete dias/56 h de abono. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:711`, `:156`, `:193`, `:209`, `:216`) |
| F2 | Mesmo cadastro ativo com ausência de férias, mas célula de folga; API e tabela integral. Ligada: pagamento do dia ausente, pendência `dia=600`; desligada: 10 h de folga trabalhada; oito dias/64 h de abono. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:721`, `:239`, `:246`, `:276`, `:292`) |
| F1s | F1 com `situacao='ferias'` por UPDATE: primeiro quatro respostas 403/`colab_ferias` e zero batidas em **cada** kernel; depois quatro registros pela porta real; compara APURACAO de F1 e idempotência. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:731`, `:676`, `:567`, `:665`) |
| F2s | Idem para folga, comparando APURACAO de F2. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:740`, `:665`) |
| D1 | Gera célula enquanto ativo; encerra vínculo e muda situação por UPDATE, preservando célula; API 403/zero, depois porta real. Espera célula concorde/600 min, DiaPago e fechamento com 10 h, sem HE nem pendência. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:749`, `:311`, `:322`) |
| D2 | Já nasce desligado, demissão dentro da competência; não tem célula no dia; API 403/zero, depois porta real. Espera DiaPago e fechamento com 10 h, célula e retenção ausentes. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:762`, `:373`, `:384`) |
| D3 | Demissão anterior à competência; sem célula; API 403/zero, depois porta real. Espera quatro batidas, relógio de 600 min no espelho, mas sem DiaPago/fechamento e zero trabalhadas. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:769`, `:428`, `:434`, `:440`) |

As quatro classes executam esses testes nos dois kernels e nas duas configurações da janela; os casos barrados ainda percorrem os dois kernels internamente, independentemente da classe. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:778`, `:786`, `:567`)

A porta real recebe os quatro horários congelados, `origem='app'`, canal PWA, sequência real e coordenadas nulas, e o teste exige quatro registros sem conflito. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:576`) A apuração chama cartório, fechamento e espelho reais; não há mock do motor nesse percurso. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:593`, `:603`)

**O que deixou de ser afirmado:** para D1/D2/D3, nenhum teste compara com MEDIDO os valores históricos de `porta` e `admin`: HTTP 200, disputa, motivos, contagens de alertas/perguntas/chamados/Pauta etc. F1/F2 ainda comparam esses valores pela API; F1s/F2s não os comparam para a situação férias. A exclusão é explícita e delimitada por APURACAO. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:52`, `:661`, `:665`, `:674`) Entretanto, `admin` ainda participa da comparação **entre as duas apurações**, porque `idempotente = um == dois` compara todo o dicionário medido. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:656`, `:673`)

**Limites dos testes:** `force_authenticate` contorna o login; o 403 verifica código e zero Batida, mas não verifica que a porta ficou sem chamadas nem que outros efeitos ficaram ausentes. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:547`, `:567`) `_numeros` omite zeros e nulos, e `resumo` omite valores falsy; nessas saídas, não distinguem zero de `None`. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:480`, `:635`)

**Número impossível:** não encontrei contradição estática que demonstre número **afirmado** impossível. Os 10 h de D1/D2 têm percurso de produção compatível: dias órfãos fora da vigência são somados às trabalhadas, e D3 fica fora do universo pela demissão anterior à janela. (`app/ponto/services/fechamento.py:792`, `:838`, `:902`, `:431`) Reproduzir numericamente cada tabela nesta árvore permanece **[nao verificado]**. As antigas respostas 200 dos casos desligados continuam armazenadas em MEDIDO, mas já não são expectativas executadas nesses casos. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:328`, `:665`)

## 10/11 20:37 · sessao a613d79df0b4 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Delta aplicado em cinco arquivos: [nao verificado]

- **Item 1:** `core/tests/test_medida_turno_do_par.py::LINHAS_REGISTRADAS`: somente duas entradas mudaram: `_orfao_resolvido_cross_meianoite`, `[809, 810] → [815, 816]`; `criar_disputa_a_partir_de_chamados_orfaos`, `[1293] → [1299]`. [Registro](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/core/tests/test_medida_turno_do_par.py:267)
- **Item 2:** `chamados/tests/test_bo_disputa_volta5.py::test_RED_d2_aberta_com_outros_motivos_e_canonica_em_fechada_duas_vezes`: `primeiro_estado` definido antes do laço; a comparação da segunda repetição permanece. [Código](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5.py:198)
- **Item 3:** `chamados/services/disputa_emissao.py::perguntar_entradas_pendentes` registra explicitamente a chamada ao gerador; `::criar_ou_atualizar_disputa` usa esse sinal para decidir a segunda releitura. [Escritor do sinal](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:294), [leitor e comentário corrigido](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:779)

Não provei que todas as cascatas sejam impossíveis quando o gerador devolve vazio. A cadeia concreta de fechamento passa por `reatar_pergunta_orfa` → `lavrar_sem_turno_aberto` → `p.disputa.fechar`; portanto, a dispensa agora depende da ausência de chamada, não do retorno vazio. [Gerador](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:497), [lavra](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/reconciliador.py:1619), [fechamento](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/reconciliador.py:411)

O RED novo executa pendentes real, simula fechamento por outra instância no gerador e retorno vazio, e exige **zero push**. O mutante que restaura a decisão da c8 falha justamente por emitir push. [Teste e mutantes](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta8_memoria.py:183) [nao verificado]

Mantidos **149/223**, com comentários atualizados em `ponto/tests/test_selo_chokepoint_escrita.py::TETOS`. Previsão: delta zero, pois a fábrica fornece data sem entradas pendentes. Nascimento pula pendentes; agregação sem chamada ao gerador conserva uma releitura. Medida ORM e identificação dos ramos efetivamente percorridos em cada cenário: **[nao verificado]**. [Fábrica](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/fabrica_da_celula.py:172), [tetos](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/ponto/tests/test_selo_chokepoint_escrita.py:186)

Controles locais passaram: volta8 **8**, volta5 **15**, sem_pergunta **14** e catraca **35** testes. Os sete mutantes da volta8, `sem_guarda_push` e o registro com linhas antigas morreram por asserção. [Runner c9](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta8_memoria.py:258), [runner volta5](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5_memoria.py:382), [catraca](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/core/tests/test_medida_turno_do_par.py:540) [nao verificado]

Comandos de memória e de cada mutante:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta8_memoria.py
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5_memoria.py
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_sem_pergunta_memoria.py

for mutante_c9 in \
  reconsulta_sem_pendentes ignora_fecho_gerador ignora_fecho_pendentes \
  estrangeira_em_a salva_fechada_d2 omite_sinal_gerador_vazio \
  retorno_vazio_dispensa_releitura
do
  PYTHONDONTWRITEBYTECODE=1 python3 -B \
    chamados/tests/test_bo_disputa_volta8_memoria.py --mutante "$mutante_c9" || exit 1
done

PYTHONDONTWRITEBYTECODE=1 python3 -B \
  chamados/tests/test_bo_disputa_volta5_memoria.py --mutante sem_guarda_push
```

Catraca completa e mutante das linhas antigas, sem Django/banco:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
from pathlib import Path
from types import ModuleType
import unittest

p = Path('core/tests/test_medida_turno_do_par.py').resolve()
suite = ModuleType('suite_turno') [nao verificado]
suite.__file__ = str(p) [nao verificado]
source = p.read_text().replace(
    'from django.test import SimpleTestCase',
    'from unittest import TestCase as SimpleTestCase')
exec(compile(source, str(p), 'exec'), suite.__dict__) [nao verificado]
runner = unittest.TextTestRunner(verbosity=2)
if not runner.run(unittest.defaultTestLoader.loadTestsFromModule(suite)).wasSuccessful(): [nao verificado]
    raise SystemExit(1)

linhas = suite.LINHAS_REGISTRADAS['derivacoes_de_dia'] [nao verificado]
linhas[('chamados/services/disputa_emissao.py::_orfao_resolvido_cross_meianoite', 'B1')] = [809, 810]
linhas[('chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos', 'B1')] = [1293]
resultado = runner.run(unittest.TestSuite([
    suite.CatracaTurnoDoParTest( [nao verificado]
        'test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit')]))
if resultado.errors or len(resultado.failures) != 1:
    raise SystemExit('mutante nao morreu por assertion')
print('MUTANTE MORTO: linhas antigas')
PY
```

Ruff indisponível no ambiente; lint, testes com banco, medidas ORM e certificação pela suite/opus ficam **[nao verificado]**. [nao verificado]

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

_3498 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
