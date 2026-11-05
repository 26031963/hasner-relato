# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 15:49** · publicado em **05/11/2026 15:49** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 15:49 · sessao afae41774832 · - · - · origem=Ronald (tmux)

Oito itens consolidados, todos por leitura do código; resultados de execução e ocorrência em produção são [não verificado].

1. **a — Botão recalcula competência exportada sem degraus.** Texto: `app/ponto/views.py:1244-1252`; chamada: `app/ponto/views.py:1253`. **CONTRADIZ; certeza alta.** O serviço mantém guarda específica para setembro/2026 (`app/ponto/services/fechamento.py:36-43`) e exclui do recálculo colaboradores das empresas exportadas (`app/ponto/services/fechamento.py:549-555`). Contraexemplo por caminho de código: POST para recalcular 09/2026 de empresa que exportou encontra essa exclusão; a existência de tal empresa em produção é [não verificado].

2. **a — Todo colaborador que entra no TXT recebe “aprovado”.** Texto: `app/folha/views.py:213-214`. **CONTRADIZ; certeza alta.** A aprovação exclui quem conserva travas (`app/folha/views.py:243-248`); somente `conta_vazia` é dispensada (`app/folha/export.py:743`). A montagem do TXT continua usando todos os itens classificados para entrada (`app/folha/views.py:268`). Contraexemplo concreto codificado: fixture B tem `status='entra'` e trava `sem_ancora_12x36` (`app/folha/tests/test_txt_le_a_celula_e_o_juiz.py:303-304`), conserva linha 0150 no arquivo (`app/folha/tests/test_txt_le_a_celula_e_o_juiz.py:294`) e fica aberto (`app/folha/tests/test_txt_le_a_celula_e_o_juiz.py:248-249`). Teste executado neste trabalho: [não verificado]. O comentário posterior reconhece a exceção (`app/folha/views.py:216-222`), mas a frase anterior permanece absoluta (`app/folha/views.py:213-214`).

3. **a — Totais do espelho seguem a competência navegada e o corte da empresa.** Texto: `app/ponto/views.py:146-152`. **AINDA VALE; certeza alta.** A view passa `mes`, `ano` e empresa para `janela_fechamento`, e entrega a janela ao espelho comum (`app/ponto/views.py:153-160`); a autoridade aplica o corte da empresa (`app/ponto/janelas.py:54-57`).

4. **a — Alterar passe livre exige motivo de dez caracteres e trilha.** Texto: `app/colaboradores/views.py:158-162`. **AINDA VALE; certeza alta.** Mudança efetiva com motivo menor que dez retorna recusa (`app/colaboradores/views.py:163-172`); a mudança usa a porta de cadastro e registra motivo e antes/depois (`app/colaboradores/views.py:173-185`). A exigência está condicionada a mudança efetiva, conforme `app/colaboradores/views.py:164`.

5. **a — Extrato parcial soma dinheiro lavrado, sem reconstruir motor próprio para dinheiro.** Texto: `app/relatorios/views.py:473-480`. **AINDA VALE; certeza alta.** A view chama `soma_do_periodo` (`app/relatorios/views.py:481-487`); o serviço filtra `DiaPago` por período e agrega seus campos com `Sum` (`app/ponto/services/dia_pago.py:782-787`). A geometria ainda é consultada separadamente para turnos abertos, como o próprio texto delimita (`app/relatorios/views.py:488-514`).

6. **a — Prévia de autorização de HE mostra DIFF por rubrica e confirmação recusa divergência.** Texto da regra L-081: `app/ponto/views.py:680-683`. **AINDA VALE; certeza alta**, entendendo “antes de qualquer escrita” como nenhuma escrita persistida: o ensaio faz o ato dentro de transação e provoca rollback (`app/ponto/services/autorizacao_he_periodo.py:320-327`); a tela publica os valores recebidos e serializa o DIFF (`app/ponto/views.py:717-720`). Confirmar compara o DIFF dentro da transação e levanta recusa se divergir (`app/ponto/services/autorizacao_he_periodo.py:343-355`). A redação literal “antes de qualquer escrita” em `app/ponto/views.py:678` é imprecisa: há escrita transacional desfeita (`app/ponto/services/autorizacao_he_periodo.py:322-323`).

7. **e — “O LOTE SO RECUSA”, no escopo da multisseleção de recusa.** Docstring: `app/ponto/tests/test_tela_gestao_he.py:350`. **AINDA VALE; certeza alta nesse escopo.** A view chama `recusar_em_lote` (`app/ponto/views.py:614`); essa função chama a porta individual fixando `DecisaoHE.NAO` (`app/ponto/portas/he.py:234-238`). A frase não deve ser generalizada para toda autorização em lote: a porta de período existe (`app/ponto/views.py:725-751`), distinção documentada também em `app/ponto/views.py:549-554`.

8. **e — Espelho web “NUNCA” desvia para template bloqueado por disputa.** Docstring: `app/ponto/tests/test_disputa_sinaliza.py:55`. **AINDA VALE; certeza alta.** A disputa é consultada e guardada (`app/ponto/views.py:137-141`), o espelho é montado independentemente dela (`app/ponto/views.py:143-160`) e a resposta usa `ponto/espelho.html`, levando `disputa_aberta` no contexto (`app/ponto/views.py:192-204`). O próprio comentário ainda existente “Bloqueado = bypass total” é obsoleto (`app/ponto/views.py:137`), mas não contém um dos marcadores exigidos para tipo a; a explicação posterior o identifica como antigo (`app/ponto/views.py:177-180`).

| item | tipo | texto:linha | código:linha | VEREDITO | certeza |
|---|---|---|---|---|---|
| 1 | a | `app/ponto/views.py:1244` | `app/ponto/services/fechamento.py:552` | CONTRADIZ | alta |
| 2 | a | `app/folha/views.py:213` | `app/folha/views.py:247` | CONTRADIZ | alta |
| 3 | a | `app/ponto/views.py:146` | `app/ponto/views.py:154` | AINDA VALE | alta |
| 4 | a | `app/colaboradores/views.py:158` | `app/colaboradores/views.py:164` | AINDA VALE | alta |
| 5 | a | `app/relatorios/views.py:473` | `app/ponto/services/dia_pago.py:782` | AINDA VALE | alta |
| 6 | a | `app/ponto/views.py:680` | `app/ponto/services/autorizacao_he_periodo.py:346` | AINDA VALE | alta, ressalva literal acima |
| 7 | e | `app/ponto/tests/test_tela_gestao_he.py:350` | `app/ponto/portas/he.py:237` | AINDA VALE | alta, escopo recusa |
| 8 | e | `app/ponto/tests/test_disputa_sinaliza.py:55` | `app/ponto/views.py:193` | AINDA VALE | alta |

## 05/11 15:49 · sessao 7455beb04fc3 · - · - · origem=Ronald (tmux)

B1–B4 **não estão individualizados** nas definições encontradas: a L-120 fala em “becos”, enquanto o backlog pede coletivamente estados vivos por motivo/status/idade; B2/B3 incluem conferência humana na tela. Portanto, atribuir um significado particular a cada número seria [não verificado] (`app/docs/LEIS.md:141`; `app/docs/BACKLOG.md:1494`).

Existem medidores adjacentes, mas não equivalem aos quatro contadores L-120:

| Medida real | Evidência | Limite |
|---|---|---|
| Beco fechável/barrado | `app/chamados/services/becos.py:25`; `app/chamados/management/commands/fechar_becos_disputa.py:32` | O alarme só considera **fecháveis**, sem idade; barradas apenas aparecem na saída (`:42`, `:67`). |
| Estoque por idade e categoria | `app/chamados/services/fila.py:88` | Universo = `qs_na_fila`, que exclui silenciados; categorias limitadas às oito maiores (`fila.py:92`, `:139`; `painel.py:186`). |
| Respostas aguardando validação por classe/idade | `app/chamados/services/validacao.py:865` | Mediana/máximo são calculados, sem meta zero ou sequência de rodadas nessa função (`:878-887`). |
| SLA vencido sem aviso | `app/chamados/services/sla.py:56`; `app/chamados/management/commands/sla_vencido_sem_aviso.py:15` | Conta ausência de aviso, não ausência de saída; fica fora do TOTAL (`bin/placar_code.sh:283`). |

Contraexemplos abaixo são **sintéticos ou já escritos como fixture de teste; não são constatações em produção** [não verificado]. [nao verificado]

| Achado | arquivo:linha | Contraexemplo concreto | Certeza | Dono L-099 |
|---|---|---|---|---|
| **Beco barrado não acende o alarme** | `app/chamados/management/commands/fechar_becos_disputa.py:67`; `app/chamados/services/becos.py:29`; `app/chamados/models.py:1772` | Pai terminal; disputa aberta; pergunta respondida `19:00`, sem validação/materialização. A resposta bloqueia C7, `classificar` devolve BARRADA, mas `--alarme` só falha com FECHAVEL. A fixture cobre esse estado em `app/chamados/tests/test_varredura_becos_evento_pergunta.py:60`. | Alta, leitura; não executado | ESTRUTURA **da cobertura do sinal**; espera humana não é automaticamente bug |
| **Idade da fila não cobre envelhecimento de todos os vivos** | `app/chamados/services/fila.py:92`; `app/chamados/services/painel.py:186` | Único chamado vivo há 90 dias com `silenciado_ate` amanhã: todas as faixas etárias da fila dão zero. `arquivados=1` continua disponível, mas sem idade desse arquivado (`fila.py:122`). Usar isso como censo completo B seria falso verde. | Alta, condicional ao uso como B | ESTRUTURA do instrumento |
| **Aviso identificado por substring pode mascarar ausência do aviso verdadeiro** | `app/chamados/services/sla.py:42`; `:66` | Chamado vencido, sem aviso emitido, contendo apenas followup de sistema `"SLA indisponível"`. A substring faz `ja_avisado=True`, movendo o chamado para `avisados`. Alcançabilidade desse texto por escritor real: [não verificado]. | Alta para predicado; média para cenário alcançável | ESTRUTURA |
| **Auditor pode sair com sucesso apesar de erro/violação fora de F1/F2** | `app/chamados/management/commands/auditar_invariantes_chamados.py:994`; `:1833`; `:1853` | Consulta de I12 falha: ele imprime `ERR` com total `-1`; F1/F2 ficam zero; não há `SystemExit(2)`. Também I20 positivo sozinho não determina saída de erro. Interpretar rc=0 como invariantes saudáveis mente. | Alta, leitura; não executado | ESTRUTURA |
| **TOTAL verde não significa todos os contadores impressos zerados** | `bin/placar_code.sh:186`; `:285`; `:549`; `:551` | `_loop=1` ou `_sla=1`, demais termos somados zero: o script pode anunciar “nenhum contador do Code aceso”. `_sla` é explicitamente humano e excluído; `_loop` também não é somado, independentemente da data mencionada no comentário. | Alta | ESTRUTURA quanto à interpretação do TOTAL; SLA pendente = CADASTRO/BATIDA/ação humana a medir |

O selo de becos usa modelos reais, verifica não fechar pergunta respondida sem veredito e testa alarme com beco FECHAVEL (`app/chamados/tests/test_varredura_becos_evento_pergunta.py:32`, `:60`, `:94`). Ele **não demonstra alarme para BARRADA**: o comando preserva expressamente essa diferença (`app/chamados/management/commands/fechar_becos_disputa.py:42`, `:67`).

Não verifiquei execução dos testes, valores dos contadores, telas B2/B3, rodadas limpas nem existência desses cenários na produção [não verificado]. [nao verificado]

## 05/11 15:49 · sessao bdf5c627bc8b · - · - · origem=Ronald (tmux)

Levantamento c–e concluído por leitura estática; testes e contadores de produção [nao verificado]. [nao verificado]

| ID sugerido | Tipo | Texto auditado | O código faz | Veredito | Certeza |
|---|---|---|---|---|---|
| C1 | c | Ausência parcial: tipo parcial, um único dia, minutos abaixo de 480 — `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:8` e `:24`. | O contador exclui `cobre_parte_do_dia` em `:49`; a autoridade usa `MINUTOS_DIA_INTEIRO=480`, exige tipo parcial, único dia e **0 < minutos < 480** — `app/ponto/turnos.py:2580`, `:2598`. | AINDA VALE | Alta, estática |
| D1 | d | `wk_em_operacao`: crons de falta/score/emergência só atuam se True — `app/colaboradores/models.py:370`. | `detectar_ausencias` usa `colaboradores_em_operacao()` — `app/ponto/management/commands/detectar_ausencias.py:98`; o helper implementa empresa em rollout + vínculo vigente, sem consultar a flag, e declara sua migração — `app/colaboradores/queries.py:50`, `:64`. | NAO VALE MAIS | Alta |
| D2 | d | `tolerante_offline` adia falta até fim do turno + folga, em vez de 15 minutos — `app/colaboradores/models.py:228`. | O gate só pergunta `marcos_vencidos` e `dia_encerrado` — `app/ponto/management/commands/detectar_ausencias.py:460`, `:470`; a retirada do leitor está registrada em `app/core/configuracao_efeito.py:123`. | NAO VALE MAIS | Alta |
| D3 | d | Até o limite cadastrado de HE, o sistema recusa com origem `sistema` — `app/colaboradores/models.py:84`. | O cron lê `limite_decisao_he_min`, respeita decisão vigente e chama a porta — `app/ponto/management/commands/recusar_ponta_pequena.py:92`, `:120`, `:170`; a porta rejeita acima do limite, preserva decisão vigente e grava `NAO`/`ORIGEM_SISTEMA` — `app/ponto/portas/he.py:313`, `:318`, `:341`. | AINDA VALE, com as guardas explicitadas | Alta |
| E1 | e | Vigia do intervalo **SO alarma**; `--alarme` não escreve e sai 2 quando acharia o que fechar — `app/ponto/tests/test_vigia_intervalo_ausente.py:4`. | Cron passa `--alarme` — `app/config/crons.py:491`; command força `dry`, usa `reconciliaria` e sai 2 se houver fechamento possível — `app/ponto/management/commands/detectar_intervalo_ausente.py:107`, `:138`, `:160`. | AINDA VALE | Alta |
| E2 | e | Cabeçalho da docstring que contém **SO** afirma “o papel segue JUIZ” para `reconciliar_vinculo` — `app/ponto/tests/test_vigia_reconciliar_vinculo.py:1`, `:5`. | O mesmo teste registra inversão em 26/10 — `:18`; configuração atual classifica `reconciliar_vinculo` como vigia, cron `--alarme` — `app/config/crons.py:489`, `:2138`. | NAO VALE MAIS | Alta |
| E3 | e | Órfãs contam **SO pelo predicado do evento**, sem escrita e com alarme — `app/ponto/tests/test_orfas_vigia.py:4`, `:8`. | Command apenas consulta `orfas_pela_lampada_sem_evento`, imprime e sai 2 quando a lista não está vazia — `app/ponto/management/commands/reconciliar_perguntas_orfas.py:29`. | AINDA VALE | Alta |
| E4 | e | Docstring com **DENTRO** explica que `vigia_de_hora` saiu dos juízes e ganhou papel `prazo` — `app/ponto/tests/test_vigia_de_hora.py:222`. | `PRAZO_DELEGA_A` registra a autoridade `cartorio::marcos_vencidos` — `app/config/crons.py:2073`; `PAPEL_DO_CRON` deriva `prazo` desse cadastro — `app/config/crons.py:2114`. | AINDA VALE | Alta |
| E5 | e | Helper do selo fornece `hoje` **SEMPRE por parâmetro** — `app/chamados/tests/test_o261_vigia_fio.py:116`. | `_censo` define default fixo relativo ao cenário e repassa-o — `:122`; produção só consulta `timezone.localdate()` se `hoje` não vier — `app/chamados/services/vigia_fio.py:329`. | AINDA VALE para esse helper | Alta |

Exemplo estático D1: colaborador com `wk_em_operacao=False`, ativo, username numérico válido, empresa ativa/em rollout e vínculo cobrindo hoje entra no universo de `detectar_ausencias`; filtros em `app/colaboradores/queries.py:25`, `:64`, consumidor em `app/ponto/management/commands/detectar_ausencias.py:98`. Isso demonstra o texto antigo; ocorrência em produção [nao verificado].

Cuidados para a consolidação:

- `420` em `app/config/crons.py:138` pertence a `_DUR_DECLARADA`/`DUR_MAX_S`, portanto segundos; `240` em `:673` e `:1974` também aparece explicitamente em segundos.
- O `720` operacional de `app/ponto/management/commands/dna_x_batida_real.py:104` normaliza distância circular, mas esse comando está expressamente declarado **“NAO e cron”** em `app/config/crons.py:1029`; sugiro excluí-lo do lote.
- As medições históricas citadas nas docstrings E1/E2 não comprovam o valor atual: produção [nao verificado].

## 05/11 15:49 · sessao 6ff699cc82f6 · - · - · origem=Ronald (tmux)

A1 está especificada: célula que acusa em dia passado deve ter chamado aberto, pergunta viva com destino ou Resolver dia; exige frota da competência aberta, contador esperado zero e caso que morde (`app/docs/BACKLOG.md:1493`). A2, A3, A4 e A5 aparecem apenas agrupadas como “espelho x chamado”, sem cláusula individual nesses registros; associar-lhes predicados específicos seria [não verificado] (`app/docs/BACKLOG.md:1494`; `app/docs/LEIS.md:141`).

| Achado | Arquivo:linha | Contraexemplo sintético, não observado em produção | Certeza | Dono L-099 |
|---|---|---|---|---|
| A1: `fio_morto` legado começa no chamado vivo; célula sem chamado fica invisível | `app/chamados/services/vigia_fio.py:333`; `app/chamados/services/vigia_fio.py:377` | Célula de ontem `furo`, sem chamado nem pergunta nem Resolver; outro dia possui fio vivo para manter `cego=False`. O legado pode publicar `fio_morto=0` | Alta; o teste caracteriza 0 legado contra 1 alcance em `app/chamados/tests/test_o261e_f5_alcance.py:446` | ESTRUTURA, lacuna do instrumento |
| A1: alcance amplia o universo, mas mede somente quem tem fechamento e motivo primário `furo_espelho` | `app/chamados/services/vigia_fio.py:581`; `app/folha/fora_da_folha.py:122`; `app/folha/export.py:763` | Mesmo dia acusador sem canal, porém sem `FechamentoMensal`; alternativamente, fechamento existente e `codigo_dominio=''`: o export sai antes por cadastro e o dia não entra no alcance (`app/folha/export.py:846`) | Alta sobre a exclusão; ocorrência em produção [não verificado] | ESTRUTURA na cobertura da medição; código ausente é CADASTRO |
| A1: alcance positivo não torna vermelho o comando de alarme | `app/chamados/management/commands/vigiar_fio_morto.py:81`; `app/chamados/management/commands/vigiar_fio_morto.py:170`; `app/chamados/management/commands/vigiar_fio_morto.py:227` | Um dia com fio vivo mantém legado saudável; outro dia retém folha sem chamado e sem canal. `fio_morto_alcance=1`, mas `gritou` examina apenas `d` e não o alcance | Alta; exclusão do alarme é exigida pelo teste `app/chamados/tests/test_o261e_f5_alcance.py:470` | ESTRUTURA |
| Selo do alcance substitui o juiz real do Resolver dia | `app/chamados/tests/test_o261e_f5_alcance.py:64` | Regressão hipotética no oferecimento real do Resolver pode escapar dos casos que sempre recebem False do mock | Alta sobre mock; escape específico é **especulação rotulada**, não demonstrado | ESTRUTURA |

Há medição real parcial: `censo_alcance` chama `dias_que_seguram`, alimenta o juiz da porta e consulta o Resolver em lote (`app/chamados/services/vigia_fio.py:584`; `app/chamados/services/vigia_fio.py:603`; `app/chamados/services/vigia_fio.py:634`). O carimbo guarda alcance e esperado zero (`app/chamados/management/commands/vigiar_fio_morto.py:95`; `app/chamados/management/commands/vigiar_fio_morto.py:216`). Portanto, o achado do alarme não significa que o número desapareça do carimbo (`app/chamados/management/commands/vigiar_fio_morto.py:218`).

Não identifiquei exceção silenciosa no carregamento da lista do admin: ela registra o erro e retorna False, mantendo o dia sujeito ao alarme (`app/chamados/services/vigia_fio.py:218`; `app/chamados/services/vigia_fio.py:224`).

Valores atuais, rodadas limpas, execução dos selos, existência dos contraexemplos na frota e definições individuais de A2–A5 permanecem [não verificado].

## 05/11 15:49 · sessao 975b343a0f63 · - · - · origem=Ronald (tmux)

Seleção de 8 itens; evidência estática, sem execução de testes. Os números históricos de produção citados nos comentários permanecem **[não verificado]**. [nao verificado]

1. **Férias viram aviso, nunca recusa — CONTRADIZ; certeza alta.** O comentário promete gravar a batida e devolver aviso (`app/api/views.py:815-820`). O endpoint recusa `situacao='ferias'` com HTTP 403 antes da gravação (`app/api/views.py:415-423`). **Contraexemplo alcançável:** usuário autenticado com colaborador em `situacao='ferias'` recebe `codigo='colab_ferias'` no retorno antecipado (`app/api/views.py:418-423`); não alcança `registrar_batida` nem o aviso (`app/api/views.py:683`, `app/api/views.py:821-833`). A mesma contradição aparece no kernel core (`app/api/views_core.py:815-820`, `app/api/views_core.py:402-410`).

2. **Fonte dos totais tem dois estados conforme existência de fechamento — NÃO VALE MAIS; certeza alta.** O comentário afirma `'folha'` quando existe fechamento e `'sem apuracao ainda'` quando não existe (`app/api/views.py:1662-1667`). O código tem o terceiro estado `'lavratura'`: sem totais da folha, havendo lavratura, escolhe seus números e esse rótulo (`app/relatorios/cartao_pela_celula.py:340-346`). A API repassa o valor sem derivá-lo (`app/api/views.py:1675`). A evolução para três estados está explicitamente documentada no teste (`app/api/tests/test_espelho_app_fonte_e_geometria.py:135-149`).

3. **Segunda entrada distante aceita outra E e gera disputa — NÃO VALE MAIS; certeza alta.** A docstring ainda exige aceitar a E e gerar `orfao_14h` (`app/api/tests/test_api_bater_ponto.py:533-534`). O endpoint consulta `proximo_tipo_de` para batida ao vivo e substitui o tipo enviado (`app/api/views.py:600-608`). O próprio corpo do teste registra a mudança e exige uma E e uma S, encerrando o turno (`app/api/tests/test_api_bater_ponto.py:564-572`).

4. **Dia sem certidão nunca vira “ok” — AINDA VALE; certeza alta.** A docstring exige `neutro` para `sem_certidao` e ausência de comparação própria contra `'ok'` (`app/api/tests/test_t42_espelho_v2_celula.py:49-54`). A tabela do núcleo mapeia `sem_certidao` e `sem_dados` para `neutro`, e o adaptador apenas consulta essa tabela (`app/ponto/status_dia.py:79-91`). A API consulta o D4 e usa esse adaptador nos cards com e sem batida (`app/api/views.py:1495-1499`, `app/api/views.py:1515-1516`, `app/api/views.py:1600`).

5. **Total trabalhado da lavratura contém só trabalhadas; folga fica separada — AINDA VALE; certeza alta.** A docstring afirma essa separação especificamente em `_totais_da_lavratura` (`app/api/tests/test_espelho_app_fonte_e_geometria.py:167-176`). A implementação atribui `horas_trabalhadas` a `total_trabalhadas` e `horas_folga_trabalhada` a `trab_folga`; a folga também integra `total_extras` (`app/relatorios/cartao_pela_celula.py:174-177`).

6. **Lavratura zero prevalece sobre o motor no dinheiro do dia — AINDA VALE; certeza alta.** O comentário denuncia a antiga queda no motor para linha lavrada zerada e declara que a escolha cabe ao núcleo (`app/api/views.py:1533-1538`). A API entrega linha e período a `horas_do_dia` e publica o número e o indicador retornados (`app/api/views.py:1539-1540`, `app/api/views.py:1554-1558`). O helper testa existência com `is not None`: linha existente retorna seu número ou zero com `True`; somente ausência da linha usa o motor com `False` (`app/ponto/services/espelho.py:132-136`).

7. **Turno aberto vem da geometria, nunca da paridade E/S — AINDA VALE; certeza alta.** O comentário declara essa regra e cita L-085 (`app/api/views.py:1506-1513`). A API carrega `turnos_do_colab`, agrupa pelo `data_turno` e calcula abertura com `any(_t.aberto ...)` (`app/api/views.py:1445-1451`, `app/api/views.py:1514`).

8. **Entrada sincronizada só fecha cobrança quando os marcos acenderam — AINDA VALE; certeza alta.** O comentário declara o requisito e a migração das duas cópias para serviço único (`app/api/views.py:800-802`). O endpoint chama o serviço para entradas (`app/api/views.py:803-805`). O serviço retorna sem fechar quando não existe cobrança ou quando `marcos_acesos_do_dia(...) is not True`; só depois tenta reconciliar (`app/ponto/services/entrada_sincronizada.py:28-32`).

| Item | Tipo | Texto:linha | Código:linha | VEREDITO | Certeza |
|---|---|---|---|---|---|
| 1 | a | `app/api/views.py:815` | `app/api/views.py:415` | CONTRADIZ | Alta |
| 2 | a | `app/api/views.py:1662` | `app/relatorios/cartao_pela_celula.py:340`; `app/api/views.py:1675` | NÃO VALE MAIS | Alta |
| 3 | e | `app/api/tests/test_api_bater_ponto.py:533` | `app/api/views.py:600`; `app/api/tests/test_api_bater_ponto.py:564` | NÃO VALE MAIS | Alta |
| 4 | e | `app/api/tests/test_t42_espelho_v2_celula.py:49` | `app/ponto/status_dia.py:84`; `app/api/views.py:1515` | AINDA VALE | Alta |
| 5 | e | `app/api/tests/test_espelho_app_fonte_e_geometria.py:169` | `app/relatorios/cartao_pela_celula.py:174` | AINDA VALE | Alta |
| 6 | a | `app/api/views.py:1533` | `app/ponto/services/espelho.py:133`; `app/api/views.py:1540` | AINDA VALE | Alta |
| 7 | a | `app/api/views.py:1506` | `app/api/views.py:1449`; `app/api/views.py:1514` | AINDA VALE | Alta |
| 8 | a | `app/api/views.py:800` | `app/ponto/services/entrada_sincronizada.py:29`; `app/api/views.py:804` | AINDA VALE | Alta |

## 05/11 15:49 · sessao 32a263dea867 · - · - · origem=Ronald (tmux)

Candidatos tipo **a**, todos de comentários de produção em `app/config/crons.py`. Não executei comandos Django nem testes. O anexo não informa contadores de execução; zero observado em produção permanece **[nao verificado]**.

| Item | Texto afirma | Código faz | VEREDITO | Certeza |
|---|---|---|---|---|
| V1 | `app/config/crons.py:1614-1615`: campo `log` de encadeado “não tem leitor”; repetido em `:1669-1670` e `:1701-1702` (lei 12). | `app/config/crons.py:1801-1802` lê `_achou['log']`; `app/core/management/commands/crons.py:40-41` o serializa; `bin/encadeado.sh:58-64` transmite `ENC_LOG`; `bin/encadeado.sh:100-102` redireciona a saída. A mudança também está documentada em `app/config/crons.py:1581-1585`. | **NAO VALE MAIS** | Alta |
| V2 | `app/config/crons.py:2116-2120` (L-113): vigia TXT lê tanto `classificar_export` como `dono_da_divergencia`. | `app/folha/services/vigia_txt.py:231` consulta `classificar_export`; `:367-368` agrega apenas Q1/Q2. O serviço declara em `:358-365` que deixou de carimbar dono e entrega essa decisão ao juiz. | **NAO VALE MAIS**, especificamente “lê os dois” | Alta |
| V3 | `app/config/crons.py:1533-1537` (L-113): vigia TXT é o “único elo da corrente que não escreve nada”. | Há também `vigiar_pausa_export` na corrente (`app/config/crons.py:1618-1620`), cujo comando só consulta censo, imprime e eventualmente levanta alarme (`app/folha/management/commands/vigiar_pausa_export.py:43-51`); seu censo consulta e conta (`app/folha/services/vigia_pausa.py:109-143`). | **NAO VALE MAIS**, especificamente “único” | Alta na estrutura visível |
| V4 | `app/config/crons.py:1598-1603` (L-033): dia isento por pausa não entra nos dias retidos; contador esperado zero por construção. | Retenção exige `not nao_retem_pela_pausa(c)` (`app/folha/export.py:1235-1236`). Vigia lê os dias devolvidos pela mesma porta e conta os que satisfazem essa mesma função (`app/folha/services/vigia_pausa.py:125-132`). | **AINDA VALE** como regra de construção; valor atual em produção **[nao verificado]** | Alta |
| V5 | `app/config/crons.py:1609-1611` (L-118): `--alarme` também termina com código 2 quando `dias_so_pausa == 0`. | Censo define `cego = dias_so_pausa == 0` (`app/folha/services/vigia_pausa.py:142`); comando inclui `cego` em `ALARMA_POR` (`app/folha/management/commands/vigiar_pausa_export.py:25`) e levanta `SystemExit(2)` (`:49-51`). | **AINDA VALE** | Alta |
| V6 | `app/config/crons.py:1721-1724` (L-039): ausência × veredito, esperado zero; `--alarme` só por cegueira e discordância só com `--alarme-passivo`. | Agenda usa apenas `--alarme` (`app/config/crons.py:1725`); comando separa `cego` e `discordancia` (`app/ponto/management/commands/vigiar_ausencia_x_veredito.py:21-22`) e só inclui a segunda quando recebe a flag (`:51-56`). | **AINDA VALE** quanto à configuração e ao alarme. Condição futura de ativação depende de medição **[nao verificado]** | Alta |
| V7 | `app/config/crons.py:2172-2176` (lei 25/10): vigia ausência × veredito consulta autoridades da acusação e cobertura; não corrige o veredito. | Usa `CelulaDia.VEREDITOS_ACUSAM`, retirando `fato_em_ausencia` (`app/ponto/services/vigia_ausencia_veredito.py:41-42`), consulta `ausencia_cobre(..., para='cobranca')` e exclui cobertura parcial (`:53-55`); devolve contagem/casos (`:56-59`). | **AINDA VALE**, com a exceção explícita `fato_em_ausencia` | Alta |
| V8 | `app/config/crons.py:2178-2180` (LEI-AKITA 2): pareador × célula lê `turnos_do_colab` e ata, sem regra paralela do fechamento. | Consulta juiz na janela (`app/ponto/services/vigia_pareador_celula.py:97`), lê primeira/última lâmpada (`:106-107`) e órfã na data civil da borda (`:109-112`). Casos sem posição são contados e excluídos (`:102-104`). | **AINDA VALE** para as autoridades consultadas | Alta |

Ponto para a consolidação: “esperado zero” e “alarme pelo contador ativo” são coisas verificavelmente distintas nesta cópia. `vigiar_retido_calado`, `vigiar_ausencia_x_veredito` e `vigia_pareador_celula` estão agendados somente com `--alarme` (`app/config/crons.py:1624`, `:1725`, `:1731`). Seus contadores entram no alarme apenas com `--alarme-passivo` (`app/folha/management/commands/vigiar_retido_calado.py:34-39`; `app/ponto/management/commands/vigiar_ausencia_x_veredito.py:51-56`; `app/ponto/management/commands/vigia_pareador_celula.py:69-74`). Isso confirma os comentários específicos; não demonstra que os respectivos passivos estejam zerados em produção **[nao verificado]**.

## 05/11 15:49 · sessao 1a58832745ae · CONTRATO-20-CENSO · censo · origem=Code

**Conclusão — certeza alta:** a guarda de O168 continua localizada no executor do supra; o cartório solicita fechamentos sem consultá-la. Existem outras guardas nas portas, portanto **“não perguntou CSF” não implica “enterrou resposta”** (`app/ponto/management/commands/supra_juiz.py:261`; `app/ponto/services/cartorio.py:1522`; `app/chamados/models.py:648`; `app/chamados/models.py:651`).

**Os dois totais históricos pedidos são [não verificado].** `CSF_RETRATADO` é um contador de execução, não um estado/motivo persistido; a trilha de estado não registra a passagem pela guarda (`app/ponto/management/commands/supra_juiz.py:288`; `app/chamados/models.py:275`). A sonda abaixo explicita essa limitação e mede as evidências disponíveis.

**(a) Portas e caminhos**

Uso `COBRANCA` conforme o sistema: `aberto` e `em_analise`. Isso inclui avisos de cadastro nesses estados, além dos chamados de furo (`app/chamados/catalogo/motor.py:29`). **Certeza alta para cada linha das tabelas: leitura estática da cópia; incidência em juliani [não verificado].**

Estas são as portas de fechamento encontradas. Os códigos da primeira coluna identificam seus efeitos nas tabelas seguintes.

| Código / porta | Resultado quando aceita | Pergunta `pode_executar_csf`? |
|---|---|---|
| **H** — `transicionar`, também por `fechar` | Destino humano permitido; grava via `admin` (`app/chamados/models.py:122`; `app/chamados/models.py:430`; `app/chamados/models.py:435`) | **Não**; aplica as guardas próprias antes de `_aplicar` (`app/chamados/models.py:129`; `app/chamados/models.py:142`; `app/chamados/models.py:175`; `app/chamados/models.py:193`) |
| **R** — `reconciliar` | `resolvido`, via `sistema` (`app/chamados/catalogo/motor.py:111`; `app/chamados/models.py:620`) | **Não**; pergunta premissa e pergunta viva (`app/chamados/models.py:648`; `app/chamados/models.py:651`) |
| **E** — `encerrar` | `fechado`, via `cascade` (`app/chamados/models.py:1533`; `app/chamados/models.py:1538`; `app/chamados/catalogo/motor.py:95`) | **Não**; pergunta a guarda de pergunta viva (`app/chamados/models.py:1536`) |
| **K** — `cancelar` | `cancelado`, via `cancelado`, com ocorrência quando há motivo (`app/chamados/models.py:746`; `app/chamados/models.py:751`; `app/chamados/models.py:754`) | **Não**; pergunta a guarda de pergunta viva (`app/chamados/models.py:749`) |
| **CT** — `encerrar_por_competencia_trancada` | `fechado`, via `competencia_trancada` (`app/chamados/models.py:1551`; `app/chamados/models.py:1558`) | **Não**; pergunta a guarda de pergunta viva (`app/chamados/models.py:1556`) |

**T**, abaixo, significa `retratar → _retratar → R`: acrescenta retenção, tratamento da justificativa, resolução das perguntas e nota de retratação. **ER** significa `encerrar_com_retencao → E`: acrescenta retenção e nota do ato. Nenhuma dessas duas unidades consulta CSF (`app/chamados/reconciliador.py:492`; `app/chamados/reconciliador.py:496`; `app/chamados/reconciliador.py:518`; `app/chamados/reconciliador.py:523`; `app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:540`; `app/chamados/reconciliador.py:546`).

| Entrada / ramo | Porta e local da solicitação | CSF antes do fecho? |
|---|---|---|
| Supra, cobrança indevida lavrada | **T**, `app/ponto/management/commands/supra_juiz.py:286` | **Sim**, `app/ponto/management/commands/supra_juiz.py:261` |
| Cartório, adiados após lavra/E2 | **R**, `app/ponto/services/cartorio.py:963` | **Não**, mesmo sítio + porta R |
| Cartório, chamados sem protesto | **R**, `app/ponto/services/cartorio.py:1488` | **Não**, mesmo sítio + porta R |
| Cartório, marcos inexistentes | **R**, `app/ponto/services/cartorio.py:1501` | **Não**, mesmo sítio + porta R |
| Cartório, cobrança sem furo/dia sem trabalho | **R**, `app/ponto/services/cartorio.py:1522` | **Não**, mesmo sítio + porta R |
| Cartório, FIO-DUP e FIO-DUP-C8 | **R**, `app/ponto/services/cartorio.py:1600`; `app/ponto/services/cartorio.py:1615` | **Não**; há guarda própria de texto (`app/ponto/services/cartorio.py:1590`; `app/ponto/services/cartorio.py:1614`) |
| Reconciliador, célula absolveu / premissa morta | **T**, `app/chamados/reconciliador.py:972`; `app/chamados/reconciliador.py:1395` | **Não**, mesmos sítios + unidade T |
| Disputa fechada, chamado-pai | **R**, `app/chamados/signals.py:42` | **Não**, mesmo sítio + porta R |
| Agrupar filhos sob adesão / resolver pai após batida | **R**, `app/chamados/services/adesao.py:115`; `app/chamados/services/adesao.py:150` | **Não**, mesmos sítios + porta R |
| Reavaliar feriado/folga | **R**, `app/chamados/services/feriado_reavaliar.py:47` | **Não**, mesmo sítio + porta R |
| Aceitar sugestão de folga / agrupar furo sob limbo | **R**, `app/chamados/services/limbo_folgas.py:101`; `app/chamados/services/limbo_folgas.py:243`; `app/chamados/services/limbo_folgas.py:246` | **Não**, mesmos sítios + porta R |
| Ausência aprovada no fluxo de silêncio | **R**, `app/chamados/services/silencio_ausencia.py:101` | **Não**, mesmo sítio + porta R |
| Materialização: ausência decidida / destino e clones | **R**, `app/chamados/services/materializacao.py:561`; `app/chamados/services/materializacao.py:738` | **Não**, mesmos sítios + porta R |
| Regularização externa materializada | **E**, `app/chamados/services/regularizacao_ext.py:205` | **Não**, mesmo sítio + porta E |
| Competência trancada | **CT**, `app/chamados/services/competencia_trancada.py:184` | **Não**, mesmo sítio + porta CT |
| Auditoria de órfãos `--fechar` | **R**, `app/chamados/management/commands/auditar_chamados_orfaos.py:67` | **Não**, mesmo sítio + porta R |
| Retratar por lei que cala | **T**, `app/chamados/management/commands/retratar_chamados_lei_cala.py:59` | **Não**, mesmo sítio + unidade T |
| Lastro | **R**, `app/ponto/services/lastro.py:345` | **Não**, mesmo sítio + porta R |
| Entrada sincronizada / triagem de batida | **R**, `app/ponto/services/entrada_sincronizada.py:31`; `app/ponto/services/triagem_batida.py:597` | **Não**, mesmos sítios + porta R |
| Decisão administrativa do dia | **R**, `app/ponto/services/veredito_celula.py:183` | **Não**; antes encerra perguntas por admin (`app/ponto/services/veredito_celula.py:177`) |
| Designar cobertura / declarar cobertura passada | **R**, `app/ponto/services/cobertura.py:122`; `app/ponto/services/cobertura.py:317` | **Não**, mesmos sítios + porta R |
| Cluster espúrio / ausência decidida | **R**, `app/ponto/services/cluster_espurio.py:98`; `app/ponto/services/ausencia.py:678` | **Não**, mesmos sítios + porta R |
| Detector de ausências: limbo, afastamento, cobertura, entrada | **R**, `app/ponto/management/commands/detectar_ausencias.py:158`; `app/ponto/management/commands/detectar_ausencias.py:821`; `app/ponto/management/commands/detectar_ausencias.py:831`; `app/ponto/management/commands/detectar_ausencias.py:842` | **Não**, mesmos sítios + porta R |
| Detector de intervalo / cura BUG115 | **R**, `app/ponto/management/commands/detectar_intervalo_ausente.py:138`; `app/ponto/management/commands/curar_residuo_bug115.py:151` | **Não**, mesmos sítios + porta R |
| Fechar chamados de meio período | **H**, por `fechar`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109` | **Não**, mesmo sítio + porta H |
| Justificativa aprovada, origem | **T**, `app/ponto/services/justificativa.py:42` | **Não**, mesmo sítio + unidade T |
| Justificativa, bloqueio de entrada / cascata retroativa | **ER**, `app/ponto/services/justificativa.py:79`; `app/ponto/services/justificativa.py:385` | **Não**, mesmos sítios + unidade ER |
| Reconciliação geofence / grade | **T**, `app/ponto/services/geofence.py:419`; `app/ponto/management/commands/reconciliar_grade.py:309` | **Não**, mesmos sítios + unidade T |
| Reconciliação fantasmas / vínculo / alertas de turno | **T**, `app/ponto/management/commands/reconciliar_fantasmas.py:279`; `app/ponto/management/commands/reconciliar_vinculo.py:68`; `app/ponto/management/commands/processar_alertas_turno.py:216` | **Não**, mesmos sítios + unidade T |
| Pós-vínculo / marcar feriado do posto | **R**, `app/colaboradores/services/pos_vinculo.py:42`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:140` | **Não**, mesmos sítios + porta R |
| Desligamento / substituição de escala aceita | **E**, `app/colaboradores/services/desligamento.py:155`; `app/colaboradores/services/vinculo.py:387` | **Não**, mesmos sítios + porta E |
| Aprovar exceção / responder agenda / responder escala | **H**, `app/chamados/services/acoes_chamado.py:70`; `app/chamados/services/acoes_chamado.py:108`; `app/chamados/services/acoes_chamado.py:149` | **Não**, mesmos sítios + porta H |
| Autorizar / recusar geofence | **H**, `app/chamados/services/acoes_chamado.py:222`; `app/chamados/services/acoes_chamado.py:277` | **Não**, mesmos sítios + porta H |
| Resposta livre com mudança de status | **H**, `app/chamados/services/acoes_chamado.py:315` | **Não**, mesmo sítio + porta H |
| Resposta rápida no fio | **H**, `app/chamados/services/fio.py:717` | **Não**, mesmo sítio + porta H |
| Avaliar benefício | **H**, `app/colaboradores/views.py:2068` | **Não**, mesmo sítio + porta H |
| Aposentar revisão de vínculo / aviso de régua | **K**, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57`; `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47` | **Não**, mesmos sítios + porta K |
| Instrumentação `smoke_chamados` | **H**, `app/chamados/management/commands/smoke_chamados.py:134`; `app/chamados/management/commands/smoke_chamados.py:147`; `app/chamados/management/commands/smoke_chamados.py:163` | **Não**, mesmos sítios + porta H |

**Fechamentos indiretos por disputa — certeza alta:** todos convergem no receiver acima, que solicita **R**, sem CSF (`app/chamados/models.py:1801`; `app/chamados/signals.py:42`). Os sítios que fecham a disputa são:

| Entrada indireta | Sítios |
|---|---|
| Admin encerra / remoção do último motivo | `app/chamados/models.py:1868`; `app/chamados/models.py:1975` |
| Materialização | `app/chamados/services/materializacao.py:565`; `app/chamados/services/materializacao.py:1255` |
| Becos / competência trancada | `app/chamados/services/becos.py:52`; `app/chamados/services/competencia_trancada.py:204` |
| Emissão/ciclo da disputa | `app/chamados/services/disputa_emissao.py:1118`; `app/chamados/services/disputa_emissao.py:1696` |
| Reconciliador | `app/chamados/reconciliador.py:412`; `app/chamados/reconciliador.py:636`; `app/chamados/reconciliador.py:1139` |
| Encerrar perguntas com fato | `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` |
| Entrada sincronizada / foto recebida / desligamento | `app/ponto/services/entrada_sincronizada.py:59`; `app/api/foto_recebida.py:80`; `app/colaboradores/services/desligamento.py:142` |

**(b) Efeito e contraexemplos**

**Certeza alta:** a guarda CSF protege perguntas com `respondida_em`, pelas relações de origem e destino. Ela **não lê `FollowupChamado`**. Contraexemplo concreto de código: chamado com mensagem livre do colaborador e nenhuma pergunta respondida recebe `True` dessa guarda (`app/ponto/supra_juiz.py:543`; `app/ponto/supra_juiz.py:547`; `app/ponto/supra_juiz.py:550`; `app/chamados/models.py:1587`; `app/chamados/models.py:1588`).

| Família dos caminhos acima | Efeito sem consultar CSF; certeza alta | Contraexemplo / limite concreto |
|---|---|---|
| **R**, inclusive cartório | Se as outras guardas aceitarem, resolve o chamado. O núcleo grava estado/via; o receiver tenta resolver perguntas associadas. Isso não exige resposta textual do DP (`app/chamados/models.py:620`; `app/chamados/models.py:275`; `app/chamados/signals.py:92`). | Pergunta viva pode impedir o fechamento: teste exige `reconciliar=False` e chamado aberto (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:86`). Outra possibilidade é fechar a duplicata **transferindo** a pergunta ao sobrevivente, sem perder o fio (`app/ponto/tests/test_fechar3_troca_escala_gate.py:323`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:325`). |
| **T** | Resolve pela porta R e acrescenta nota automática `retratado`; pode rejeitar justificativa pendente vinculada. Isso é retratação canônica, mas não incrementa, por si, o contador `CSF_RETRATADO` do supra (`app/chamados/reconciliador.py:523`; `app/chamados/reconciliador.py:531`; `app/chamados/reconciliador.py:540`; `app/ponto/management/commands/supra_juiz.py:288`). | Teste concreto: resposta **“23:00”**, premissa morta, `_retratar=False`, chamado permanece em análise por retenção de resposta sem consumo (`app/chamados/tests/test_selo_retratador_turno_do_chamado.py:128`; `app/chamados/tests/test_selo_retratador_turno_do_chamado.py:134`; `app/chamados/tests/test_selo_retratador_turno_do_chamado.py:136`). |
| **E / ER** | Fecha pela via `cascade`; ER primeiro consulta a retenção e depois acrescenta nota do ato. Nenhum exige nova resposta textual do DP (`app/chamados/models.py:1538`; `app/chamados/reconciliador.py:492`; `app/chamados/reconciliador.py:499`). | No desligamento, o teste com resposta **“08:05”** sem veredito exige que esse chamado continue aberto (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:236`; `app/chamados/tests/test_o261e_f1_porta_encerramento.py:247`). |
| **CT** | Fecha pela via de competência trancada; a ocorrência declara o fio consultável, e respostas sem veredito são encaminhadas à pauta (`app/chamados/models.py:1558`; `app/chamados/services/competencia_trancada.py:187`; `app/chamados/services/competencia_trancada.py:223`). | A guarda geral não protege indiscriminadamente toda competência: o teste de competência entregue permite reconciliar (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:117`). Visualização efetiva dessa conversa em produção: **[não verificado]**. |
| **K** | Cancela e registra o motivo como ocorrência; não é o contador CSF nem exige resposta do DP (`app/chamados/models.py:751`; `app/chamados/models.py:754`). | Pergunta viva pode impedir o cancelamento: teste exige retorno falso e estado aberto (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:104`; `app/chamados/tests/test_o261e_f1_porta_encerramento.py:105`). |
| **H**, resposta livre | Pode fechar sem nova mensagem: grava conteúdo somente se preenchido, mas transiciona independentemente (`app/chamados/services/acoes_chamado.py:289`; `app/chamados/services/acoes_chamado.py:313`). | Cenário concreto construível: há mensagem anterior do colaborador; gestor envia `novo_status=fechado`, `conteudo=''`; sem disputa/pergunta viva impeditiva, o fluxo não exige resposta textual. Ocorrência em produção: **[não verificado]** (`app/chamados/models.py:142`; `app/chamados/models.py:159`; `app/chamados/services/acoes_chamado.py:315`). |
| **H**, resposta rápida no fio | Este caminho exige conteúdo e grava `mensagem_dp` antes da transição (`app/chamados/services/fio.py:705`; `app/chamados/services/fio.py:714`; `app/chamados/services/fio.py:717`). | É contraexemplo à generalização “todo fechamento sem CSF ocorre sem resposta do DP”, pelos mesmos sítios. |
| **Disputa → R** | A disputa fechada solicita resolução do pai e acrescenta ocorrência com sua resolução (`app/chamados/signals.py:42`; `app/chamados/signals.py:45`). | A própria porta da disputa recusa respostas sem veredito; portanto a ausência de CSF não torna esse caminho incondicional (`app/chamados/models.py:1816`; `app/chamados/models.py:1818`). |

**Cartório, especificamente — certeza alta:** o ramo FIO-DUP tem proteção própria de texto. O teste com resposta **“17:00”** exige preservar chamado e âncora; portanto esse ramo é contraexemplo concreto a “cartório sem CSF sempre fecha conversa” (`app/ponto/services/cartorio.py:1590`; `app/ponto/services/cartorio.py:1597`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:339`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:343`).

**(c) Sonda e limite da medição**

**Certeza alta:** os dois números exatos não podem ser reconstruídos dessas evidências:

- `CSF_RETRATADO` é incrementado em memória e impresso no placar. O dado raspado menciona cinco linhas históricas, mas não fornece a série dos últimos 30 dias (`app/ponto/management/commands/supra_juiz.py:288`; `app/ponto/management/commands/supra_juiz.py:297`; `app/docs/BACKLOG.md:1381`).
- `estado_chamado` guarda estados/via, sem chamada da guarda ou pilha. O ator identifica o processo, não qual ramo interno executou (`app/chamados/models.py:275`; `app/core/observ.py:22`; `app/core/observ.py:117`).
- Aplicar a guarda hoje não reconstrói seu resultado histórico: há reapontamento de perguntas e ressuscitação que limpa `respondida_em` (`app/chamados/models.py:420`; `app/chamados/models.py:1493`).

A sonda retorna **`null`, nunca zero**, para esses dois totais, e entrega eventos auditados, chamados distintos, mensagens anteriores ao evento e avaliação **atual** da função real. Execução e resultados em juliani: **[não verificado]**.

O bloco é para colar em `manage.py shell` já aberto com backend tenant. O bootstrap SaaS possui criação de diretório e handlers de arquivo; iniciar esse ambiente não equivale, por si, a não tocar arquivos (`app/config/settings/saas.py:118`; `app/config/settings/saas.py:138`; `app/config/settings/saas.py:162`).

===SONDA===
```python
import sys
sys.dont_write_bytecode = True

import json
from collections import Counter, defaultdict
from datetime import timedelta

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from chamados.catalogo.motor import COBRANCA, TERMINAIS
from chamados.models import ChamadoColaborador
from core.models import LogAuditoria
from ponto.supra_juiz import pode_executar_csf

# Fontes:
# estados: app/chamados/catalogo/motor.py:29
# auditoria: app/core/models.py:285
# payload estado_chamado: app/chamados/models.py:275
# guarda real, somente leitura: app/ponto/supra_juiz.py:539
# mensagens e datas: app/chamados/models.py:1587

if connection.in_atomic_block:
    raise RuntimeError("Execute fora de outra transacao.")

with transaction.atomic():
    with connection.cursor() as cursor:
        cursor.execute(
            "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ READ ONLY"
        )

    with schema_context("juliani"):
        agora = timezone.now()
        desde = agora - timedelta(days=30)

        # Mede transicoes auditadas, inclusive ciclos de fechar/reabrir.
        # Nao usa resolvido_em como historico:
        # reabertura limpa o campo (app/chamados/models.py:242).
        eventos = list(
            LogAuditoria.objects.filter(
                modelo="ChamadoColaborador",
                acao="estado_chamado",
                criado_em__gte=desde,
                criado_em__lt=agora,
                valor_antes__status_local__in=COBRANCA,
                valor_depois__status_local__in=TERMINAIS,
            ).order_by("criado_em", "pk")
        )

        ids = {
            e.objeto_id for e in eventos
            if e.objeto_id is not None
        }
        chamados = {
            ch.pk: ch
            for ch in ChamadoColaborador.objects.filter(
                pk__in=ids
            ).prefetch_related(
                "disputa_origem__perguntas",
                "perguntas_que_resolvem",
            )
        }

        # Funcao real; avaliacao de HOJE, sem fingir replay historico.
        bloqueia_agora = {
            pk: not pode_executar_csf(ch)
            for pk, ch in chamados.items()
        }

        grupos = defaultdict(Counter)
        distintos = defaultdict(set)
        com_mensagem_previa = set()
        evidencias = []

        for e in eventos:
            via = (e.valor_depois or {}).get("via", "")
            ator = e.ator or "[request/ou ator ausente]"
            chave = (ator, via)

            grupos[chave]["eventos"] += 1
            distintos[chave].add(e.objeto_id)
            ch = chamados.get(e.objeto_id)

            mensagem_previa = None
            guarda_atual = None
            if ch

[truncado em 20000 de 22886 caracteres]

## 05/11 15:48 · sessao e49e34c06e9f · - · - · origem=Ronald (tmux)

Levantamento tipo **b**, somente por leitura da cópia. Não executei Django nem testes; a contagem abaixo veio de AST com `python3 -B`. Operação no host e resultados em produção: [nao verificado]. [nao verificado]

| Item | Texto e afirmação | Código e comportamento | VEREDITO | Certeza |
|---|---|---|---|---|
| B1 | `app/docs/LEIS.md:94`: cron de varredura “nunca juiz”, “só alarma”. | `app/config/crons.py:1999`, `:2004`, `:2018` mantêm **3** nomes em `JUIZES_POR_VARREDURA`; `:2111` dá papel `juiz`. Contraexemplo: `:1483` encadeia `disparar_perguntas_competencia` com `--apply`; `app/chamados/management/commands/disparar_perguntas_competencia.py:287` entra na escrita transacional e `:280` chama `perguntar(...)` para fio mudo. | **CONTRADIZ** | Alta, contradição estática; existência de alvos em prod [nao verificado]. |
| B2 | `app/docs/LEIS.md:94`: papel declarado em `config/crons.py` e contador esperado zero. | `app/config/crons.py:2110` define cadastro, `:2280` resolve papel, `:2297` calcula juízes; `bin/placar_code.sh:477` lê `len(juizes_por_varredura())` e `:479` publica `esperado 0`. O zero é alvo; a lista desta cópia tem 3 membros (`app/config/crons.py:1999`, `:2004`, `:2018`). | **AINDA VALE**, nesta cláusula específica | Alta. |
| B3 | `app/docs/LEIS.md:127`, cláusula normativa: prazo tem papel próprio e deve delegar ao juiz da casa. | `app/config/crons.py:1955` inclui `prazo`; `:2051` cadastra **12** delegações; `:2114` deriva seus papéis desse cadastro. `bin/tests/test_papel_prazo_nao_deriva.sh:247` compara cadastro/papel, `:277` examina comando, `:287` examina corpo e `:295` exige citação à autoridade. A abrangência é comando e corpo cadastrado; o próprio selo limita escopo em `:26`. | **AINDA VALE**, cadastro e guarda existentes | Alta para existência; média para garantia semântica universal, pois a guarda é AST e tem escopo declarado. |
| B4 | `app/docs/LEIS.md:127`, diagnóstico: “NENHUM”, `PAPEIS` tem 7 papéis, nenhum `prazo`; 27 crons continuam juiz. | `app/config/crons.py:1955` tem **8** papéis incluindo `prazo`; `:2051` possui cadastro com 12 comandos; `:2114` atribui papel `prazo`; a dívida nominal tem **3** membros em `:1999`, `:2004`, `:2018`. | **NAO VALE MAIS** | Alta. |
| B5 | `app/docs/LEIS.md:191`: 27 crons continuam juiz, incluindo cartório, supra, par-relâmpago e flip. | A lista atual tem os três nomes citados em B1 (`app/config/crons.py:1999`, `:2004`, `:2018`). Os quatro exemplos antigos estão no cadastro `prazo`: cartório `:2062`, par `:2069`, supra `:2071`, flip `:2084`; papel atribuído em `:2114`. | **NAO VALE MAIS** | Alta. |
| B6 | `app/docs/LEIS.md:194`: ainda falta criar papel `prazo` e guarda de delegação. | Papel existe em `app/config/crons.py:1955`; cadastro em `:2051`; atribuição em `:2114`. Guarda existe em `bin/tests/test_papel_prazo_nao_deriva.sh:247`, `:277`, `:287`, `:295`. | **NAO VALE MAIS** | Alta; execução atual do selo [nao verificado]. |
| B7, opcional monetário | `app/docs/LEIS.md:93`, regra absoluta: competência com TXT não é tocada, “nem para corrigir”. A própria linha admite superação parcial. | `app/ponto/portas/celula.py:196` aceita `apesar_da_lavra` e põe `_exp=[]`; sem esse motivo `:199` carrega proteção de `_competencias_exportadas`, e `:238` pula datas protegidas. Com motivo, pode chegar à reescrita `:260` e `:264`. `app/ponto/management/commands/gerar_celulas.py:173` pergunta ao juiz de competência fechada antes de incluir a janela protegida. | **NAO VALE MAIS**, cláusula “nem para corrigir”; proteção padrão permanece | Alta. |

Para B1, o contraexemplo é um caminho explícito do código, não alegação de que ele rodou ou escreveu algum registro em produção. A configuração conserva a emissão do fio mudo e restringe o caminho de célula a alarme: `app/chamados/management/commands/disparar_perguntas_competencia.py:245`, `:251`, `:268`, `:287`.

Nenhum `AGENTS.md` apareceu em `rg --files --hidden -g AGENTS.md`; instruções locais adicionais [nao verificado].

## 05/11 15:48 · sessao 68b87afddb72 · - · - · origem=Ronald (tmux)

Candidatos tipo **b**, somente por leitura:

| ID | Texto afirma | Código faz / evidência | VEREDITO | Certeza |
|---|---|---|---|---|
| B01 | Há cadastro pela UI para regime da empresa e aplicação de convenção. `app/docs/LEIS.md:64` | A view aceita regime válido e chama o escritor: `app/colaboradores/views.py:512`, `app/colaboradores/views.py:520`. Aplicação entra pelo serviço, inclusive desativação: `app/core/views_config.py:391`, `app/core/views_config.py:404`, `app/core/views_config.py:410`. | AINDA VALE **para essas portas expressamente citadas** | Alta, estática |
| B02 | “A BATIDA DE CHÃO NUNCA É BARRADA EM RUNTIME”. `app/docs/LEIS.md:78` | Férias/desligado retornam 403: `app/api/views_core.py:402`, `app/api/views_core.py:410`. GPS ausente também retorna 403: `app/api/views_core.py:440`, `app/api/views_core.py:454`. Rota ativa nessa casca: `app/api/urls_core.py:14`; equivalente full: `app/api/urls.py:67`, `app/api/views.py:415`. | CONTRADIZ | Alta, estática |
| B03 | Cartão PDF usa os dias da tela; piso visual não corta o período solicitado. `app/docs/LEIS.md:85` | PDF chama `espelho_do_colab` e filtra seus dias pelo período: `app/relatorios/pdf_espelho.py:596`, `app/relatorios/pdf_espelho.py:598`, `app/relatorios/pdf_espelho.py:602`. Serviço usa `min(piso_visual(...), apur_ini)`: `app/ponto/services/espelho.py:1225`; vínculo por dia: `app/ponto/services/espelho.py:286`. | AINDA VALE **para a integração e remoção do corte pelo piso citadas** | Alta, estática |
| B04 | Lista Cadastro × Realidade tem agrupamento, ação/destino; PDF é a mesma lista. `app/docs/LEIS.md:88` | A view calcula `dados` uma vez e entrega ao PDF ou template: `app/escala/views.py:487`, `app/escala/views.py:489`, `app/escala/views.py:492`. Serviço agrega assinaturas e ações por vínculo/posto: `app/escala/services/cadastro_realidade.py:650`, `app/escala/services/cadastro_realidade.py:680`, `app/escala/services/cadastro_realidade.py:685`. PDF percorre os mesmos postos/assinaturas/destinos: `app/escala/services/cadastro_realidade.py:789`, `app/escala/services/cadastro_realidade.py:793`, `app/escala/services/cadastro_realidade.py:800`. | AINDA VALE **para a montagem comum citada** | Alta, estática |
| B05 | Mudanças no front não sobem sem smoke de clique do Ronald nas duas cascas. `app/docs/LEIS.md:99` | Há piso automatizado de cascata/console, explicitamente limitado a uma página por casca: `app/core/tests/test_smoke_chromium.py:35`, `app/core/tests/test_smoke_chromium.py:65`, `app/core/tests/test_smoke_chromium.py:124`. Régua chama verificação sintática JS: `bin/regua.sh:146`. Isso não demonstra clique humano nem autorização do envio. | NAO SE SABE — faltam registros do clique e do envio correspondente; anexo não os fornece. [não verificado] | Alta quanto à insuficiência |
| B06 | Universo do holerite usa mês civil; lista de ausências filtra vigência. `app/docs/LEIS.md:130` | Universo usa `_limites`, do dia 1 ao último: `app/holerite/matriz.py:51`, `app/holerite/matriz.py:89`. Ausências calcula intervalo e chama `ausencias_vigentes`: `app/ponto/views.py:2177`, `app/ponto/views.py:2180`. Consulta explícita por número de registro substitui esse filtro: `app/ponto/views.py:2181`. | AINDA VALE **no universo e filtro normal declarados** | Alta, estática |
| B07 | Admin pode declarar início; derivação por batida é sugestão, não limite. `app/docs/LEIS.md:150` | Existem três modos: `app/colaboradores/views_fase.py:23`. Data declarada vira início e âncora: `app/colaboradores/views_fase.py:81`, `app/colaboradores/views_fase.py:89`, `app/colaboradores/views_fase.py:93`. Template oferece rádio/data e transmite início ao formulário: `app/templates/colaboradores/partials/_fase_12x36.html:31`, `app/templates/colaboradores/partials/_fase_12x36.html:35`, `app/templates/colaboradores/partials/_fase_12x36.html:41`; submissão usa juiz/executor: `app/colaboradores/views.py:411`, `app/colaboradores/views.py:423`. | AINDA VALE **para a possibilidade de declaração na tela** | Alta, estática |
| B08 | Na mensageria de ausências/férias, mês civil é fallback rotulado **só quando os cortes divergem**. `app/docs/LEIS.md:174` | O teste é `len(cortes) == 1`; todo outro tamanho cai em mês civil, inclusive zero: `app/api/views_mensageria.py:723`, `app/api/views_mensageria.py:731`, `app/api/views_mensageria.py:734`; férias repete: `app/api/views_mensageria.py:883`, `app/api/views_mensageria.py:891`, `app/api/views_mensageria.py:894`. | CONTRADIZ **a exclusividade “quando divergem”** | Alta, estática |
| B09 | Implementação de L-112 está pronta, mas faltam merge, deploy e print de tela. `app/docs/LEIS.md:198` | Implementação está na cópia: `app/colaboradores/views_fase.py:23`, `app/templates/colaboradores/partials/_fase_12x36.html:31`. Presença do código não prova estado de merge/deploy/print. | NAO SE SABE — faltam evidências desses atos; anexo não as traz. [não verificado] | Alta quanto à insuficiência |

**Contraexemplos estáticos:**

- **B02:** colaborador autenticado com `situacao='ferias'` recebe `403 colab_ferias` antes de registrar batida, mesmo com tipo/GPS preenchidos: `app/api/views_core.py:402`, `app/api/views_core.py:405`, `app/api/views_core.py:410`. Alternativamente, colaborador ativo sem latitude/longitude recebe `403 gps_obrigatorio`: `app/api/views_core.py:440`, `app/api/views_core.py:457`. Não são casos de produção observados. [não verificado]
- **B08:** sem `empresa_id` e sem empresa ativa com corte não nulo, o conjunto `cortes` fica vazio; `len(cortes)==1` falha, e o endpoint escolhe mês civil com rótulo “empresas usam cortes diferentes”, apesar de nenhum corte estar presente: `app/api/views_mensageria.py:720`, `app/api/views_mensageria.py:723`, `app/api/views_mensageria.py:732`, `app/api/views_mensageria.py:734`. Ocorrência desse cadastro em produção: [não verificado].

Observação para consolidação: `app/docs/LEIS.md:161` **já admite** as barreiras de L-030; não é necessário contá-la como outro achado. A regra absoluta em `app/docs/LEIS.md:78` continua escrita.

## 05/11 15:48 · sessao 42408f193864 · - · - · origem=Ronald (tmux)

Nove itens, apenas pela cópia. Ocorrência dos cenários em produção: `[nao verificado]`.

1. **S1 — tipo b — NAO VALE MAIS; certeza alta.**  
   **AFIRMA:** supra tem teto apenas por data civil; turno de ontem que atravessa meia-noite fica sem guarda de instante (`app/docs/LEIS.md:167`).  
   **FAZ:** `turno_encerrado` é obrigatório e governa os ramos de furo/fato; o comando fornece o resultado de `dias_encerrados` (`app/ponto/supra_juiz.py:209`, `app/ponto/supra_juiz.py:278`, `app/ponto/supra_juiz.py:316`, `app/ponto/supra_juiz.py:380`, `app/ponto/management/commands/supra_juiz.py:120`, `app/ponto/management/commands/supra_juiz.py:232`). O juiz verifica turno aberto e compara `agora` ao fim previsto, independentemente de ser ontem (`app/ponto/turnos.py:2114`, `app/ponto/turnos.py:2119`, `app/ponto/turnos.py:2122`). A exceção por data permanece no ramo sem vínculo (`app/ponto/supra_juiz.py:454`).

2. **S2 — tipo a — NAO VALE MAIS; certeza alta.**  
   **AFIRMA:** cobertura parcial entra “SO NO PORTAO DO FURO”; `COBRANCA_EM_DIA_COBERTO` continua lendo aprovação “CRU” (`app/ponto/supra_juiz.py:260`, `app/ponto/supra_juiz.py:264`).  
   **FAZ:** parcial com marco missing e período diferente de `sem_periodo` calcula `_resto_cobrado=True` e impede `COBRANCA_EM_DIA_COBERTO`; portanto a parcial também altera esse ramo (`app/ponto/supra_juiz.py:530`, `app/ponto/supra_juiz.py:531`). O comentário posterior já descreve essa mudança (`app/ponto/supra_juiz.py:525`).

3. **S3 — tipo a — AINDA VALE; certeza alta.**  
   **AFIRMA:** `FURO_PRE_ADOCAO` declara ausência de dívida cobrável ao colaborador; a lista é consumida na retenção e afeta TXT/dinheiro (`app/ponto/supra_juiz.py:16`, `app/ponto/supra_juiz.py:22`).  
   **FAZ:** a lista contém `FURO_PRE_ADOCAO`; o retentor isenta quando há protestos e todos pertencem a essa lista. Mistura com protesto cobrável continua sujeita à retenção (`app/ponto/supra_juiz.py:34`, `app/folha/export.py:1227`, `app/folha/export.py:1235`). A regra não estabelece isenção geral de qualquer dia que contenha esse código (`app/folha/export.py:1235`).

4. **S4 — tipo a — AINDA VALE; certeza alta.**  
   **AFIRMA:** batida casada prova turno pela hora; atraso não elimina essa prova; marcador sintético de isenção não conta (`app/ponto/supra_juiz.py:93`, `app/ponto/supra_juiz.py:99`).  
   **FAZ:** `_celulas_com_hora` exige dicionário com `hora`, sem `missing` e sem `isencao`; não exige status `ok`; `_celulas_ok` aponta para essa função (`app/ponto/supra_juiz.py:88`, `app/ponto/supra_juiz.py:102`).

5. **S5 — tipo a — AINDA VALE; certeza alta.**  
   **AFIRMA:** usa `n_ok` lavrado quando fornecido, preservando zero; recontagem é fallback apenas quando não fornecido (`app/ponto/supra_juiz.py:225`).  
   **FAZ:** escolhe o valor lavrado com `is not None`; zero não dispara recontagem (`app/ponto/supra_juiz.py:229`, `app/ponto/supra_juiz.py:230`).

6. **S6 — tipo a — CONTRADIZ; certeza alta no classificador; ocorrência produtiva `[nao verificado]`.**  
   **AFIRMA:** justificativa aprovada cala furo do colaborador, “NUNCA a acusacao de CADASTRO do mesmo ramo” (`app/ponto/supra_juiz.py:327`, `app/ponto/supra_juiz.py:329`).  
   **FAZ:** o ramo de régua degenerada emite `FURO_COM_REGUA_DEGENERADA`, descrito como “divida de cadastro” e “cobra o CADASTRO”; depois o filtro de justificativa remove qualquer código iniciado por `FURO_`, incluindo esse (`app/ponto/supra_juiz.py:348`, `app/ponto/supra_juiz.py:352`, `app/ponto/supra_juiz.py:354`, `app/ponto/supra_juiz.py:371`).  
   **Contraexemplo:** `dia={'tipo_dia':'trabalho','minutos_previstos':480,'minutos_realizados':0,'celulas':[]}`, com `regua_degenerada=True, turno_encerrado=True`, produz `FURO_COM_REGUA_DEGENERADA`; acrescentar `justificado=True` produz `[]`. O resultado foi confirmado executando a função em memória, sem Django e sem escrita; os ramos determinantes são `app/ponto/supra_juiz.py:313`, `app/ponto/supra_juiz.py:348` e `app/ponto/supra_juiz.py:371`.

7. **S7 — tipo a — AINDA VALE; certeza alta.**  
   **AFIRMA:** quem decide retratação é a lâmpada `cobranca_indevida` lavrada, com código dos protestos; classificação do comando alimenta placar/alarme; ausência de lavra não retrata (`app/ponto/management/commands/supra_juiz.py:239`).  
   **FAZ:** lê veredito/protestos persistidos, conta `CSF_SEM_LAVRA`, exige `cobranca_indevida` e busca código nos protestos antes de considerar retratação (`app/ponto/management/commands/supra_juiz.py:165`, `app/ponto/management/commands/supra_juiz.py:246`, `app/ponto/management/commands/supra_juiz.py:250`). A chamada de escrita depende também de `--executar` e da trava desativada (`app/ponto/management/commands/supra_juiz.py:45`, `app/ponto/management/commands/supra_juiz.py:279`).

8. **S8 — tipo a — AINDA VALE; certeza alta.**  
   **AFIRMA:** cobrança órfã de regeneração é segurada e contada pelo supra, sem retratação automática que negaria o marco existente na origem (`app/ponto/management/commands/supra_juiz.py:264`).  
   **FAZ:** consulta `cobranca_orfa`; se verdadeiro, incrementa `COBRANCA_ORFA_REGENERACAO` e executa `continue`, antes da chamada ao retratador (`app/ponto/management/commands/supra_juiz.py:275`, `app/ponto/management/commands/supra_juiz.py:277`, `app/ponto/management/commands/supra_juiz.py:286`). A ocorrência histórica col439 citada no comentário não está demonstrada no anexo fornecido: `[nao verificado]`.

9. **S9 — tipo e — AINDA VALE; certeza alta.**  
   **AFIRMA:** chamado com conversa — pergunta respondida por disputa ou relação direta — “NUNCA e morto no ato” pelo executor (`app/ponto/tests/test_supra_juiz_executor.py:6`).  
   **FAZ:** `pode_executar_csf` devolve `False` ao encontrar `respondida_em is not None` em qualquer das duas relações; o comando preserva e pula o chamado quando recebe esse resultado (`app/ponto/supra_juiz.py:543`, `app/ponto/supra_juiz.py:547`, `app/ponto/management/commands/supra_juiz.py:261`). Os testes cobrem resposta em disputa e resposta direta (`app/ponto/tests/test_supra_juiz_executor.py:42`, `app/ponto/tests/test_supra_juiz_executor.py:45`).

| Item | Tipo | Texto:linha | Código:linha | VEREDITO | Certeza |
|---|---|---|---|---|---|
| S1 | b | `app/docs/LEIS.md:167` | `app/ponto/supra_juiz.py:278`; `app/ponto/turnos.py:2114` | NAO VALE MAIS | Alta |
| S2 | a | `app/ponto/supra_juiz.py:264` | `app/ponto/supra_juiz.py:530` | NAO VALE MAIS | Alta |
| S3 | a | `app/ponto/supra_juiz.py:16` | `app/ponto/supra_juiz.py:34`; `app/folha/export.py:1235` | AINDA VALE | Alta |
| S4 | a | `app/ponto/supra_juiz.py:93` | `app/ponto/supra_juiz.py:88` | AINDA VALE | Alta |
| S5 | a | `app/ponto/supra_juiz.py:225` | `app/ponto/supra_juiz.py:230` | AINDA VALE | Alta |
| S6 | a | `app/ponto/supra_juiz.py:329` | `app/ponto/supra_juiz.py:354`; `app/ponto/supra_juiz.py:372` | CONTRADIZ | Alta |
| S7 | a | `app/ponto/management/commands/supra_juiz.py:239` | `app/ponto/management/commands/supra_juiz.py:246` | AINDA VALE | Alta |
| S8 | a | `app/ponto/management/commands/supra_juiz.py:264` | `app/ponto/management/commands/supra_juiz.py:275` | AINDA VALE | Alta |
| S9 | e | `app/ponto/tests/test_supra_juiz_executor.py:6` | `app/ponto/supra_juiz.py:543`; `app/ponto/management/commands/supra_juiz.py:261` | AINDA VALE | Alta |

## 05/11 15:48 · sessao 4ac00f2229ea · - · - · origem=Ronald (tmux)

Dez candidatos, sem repetir L-020/L-052 nem signals. Certeza abaixo é sobre leitura estática; execução e alcance em produção estão `[nao verificado]`.

1. **Tipo e — `--forcar` “SO sombra”.** Texto: `app/ponto/tests/test_carimbo_isencao_lampada.py:27`–`:29`. **AFIRMA:** exportada só seria alcançada por `--forcar`, restrito à sombra. **FAZ:** o comando aceita `--apply` e `--forcar` independentemente (`app/ponto/management/commands/processar_cartorio.py:27`, `:29`) e passa ambos ao juiz (`:65`); o pulo por impressão depende de `not forcar` (`app/ponto/services/cartorio.py:1362`). O próprio docstring atual registra a superação (`app/ponto/services/cartorio.py:900`). **NAO VALE MAIS; alta.** Não usar os números históricos desse docstring como dado atual.

2. **Tipo b — L-073 descreve teto por instante somente hoje.** Texto: `app/docs/LEIS.md:100`, repetido em `:167`. **AFIRMA:** cartório só compara instante no dia corrente e deixa ontem cross-meia-noite sem essa guarda. **FAZ:** consulta `dias_encerrados(... agora=agora ...)` (`app/ponto/services/cartorio.py:1102`–`:1106`) e passa o resultado ao classificador (`:1423`); a vigia também trata ontem explicitamente (`:1318`–`:1333`). **NAO VALE MAIS; alta**, quanto ao retrato da implementação no índice. A lei do teto permanece compatível com esse código; não confundir com invalidar a lei.

3. **Tipo b — L-033 ainda contém “retém todo veredito furo”.** Texto: coluna PROTEGE de `app/docs/LEIS.md:81`: “NENHUM: folha/export.py::motivos_retencao_celula retem todo veredito furo”. **FAZ:** existe exceção explícita para protesto exclusivamente `FURO_PARCIAL` e falta somente na pausa (`app/folha/export.py:1147`); essa exceção impede `_retem` (`:1235`–`:1236`). O leitor exige resposta conhecida e coerente dos períodos (`:1124`–`:1129`). **NAO VALE MAIS; alta.** A própria coluna ESTADO dessa mesma linha já reconhece a implementação; o trecho envelhecido é PROTEGE, não a regra principal.

4. **Tipo b — L-124, horizonte da isenção na impressão.** Texto: `app/docs/LEIS.md:145`. **AFIRMA:** regra nova alcança horizonte para frente; passado fica foto até fato novo/backfill. **FAZ, no uso concretamente declarado:** versões 2/3/4 começam em competência `2026-10` (`app/ponto/services/cartorio.py:132`), `versao_para` seleciona a maior versão aplicável (`:263`–`:267`), e `isento:1` só entra com `regra >= 4` (`:520`–`:521`). **AINDA VALE; alta nesse uso específico.** A execução de BACKFILL-UNICO ou sua ausência em produção é `[nao verificado]`.

5. **Tipo a — batida retratada não julga.** Texto: comentário com corte em `app/ponto/services/cartorio.py:1109`. **AFIRMA:** batidas retratadas ficam fora da apuração do cartório. **FAZ:** obtém `bs` por `batidas_apuraveis` (`:1121`–`:1123`) e entrega esse conjunto à montagem da grade (`:1126`); o juiz filtra `retratada_em__isnull=True` (`app/ponto/turnos.py:2864`). **AINDA VALE; alta.**

6. **Tipo a — lâmpada `cobrado` não se sustenta com marcos satisfeitos.** Texto: corte em `app/ponto/services/cartorio.py:1447`–`:1448`. **AFIRMA:** cobrança de marcos todos acesos na ata deste julgamento não sustenta `cobrado`. **FAZ:** `_protesta` exige `trabalha is not False` e resultado de `_acesos` diferente de `True` (`:1407`–`:1408`); `cobrado` só é atribuído se existir chamado com fio que ainda proteste (`:1451`–`:1452`). **AINDA VALE; alta.** A limitação declarada no anexo continua visível: `_protesta` usa `trabalha`, sem incorporar a segunda cláusula de `dia_sem_trabalho` (`:1408`).

7. **Tipo a — parcial sem período vai à lista, sem chamado novo.** Texto: corte em `app/ponto/services/cartorio.py:379`–`:382`. **AFIRMA:** `cobertura_parcial` com `periodo_parcial='sem_periodo'` nunca gera chamado pelo julgamento, independentemente de fato novo/primeiro julgamento. **FAZ:** `parcial_so_lista` retorna `True` antes da consulta de fato novo (`:395`–`:400`); o cartório usa o predicado (`:1706`–`:1707`) e os dois pontos de abertura exigem `not _parcial_so_lista` (`:1725`, `:1777`). **AINDA VALE; alta para as duas portas de abertura deste cartório.** Isso não prova inexistência de chamado anterior no banco `[nao verificado]`.

8. **Tipo a — carimbo de isenção é propagação.** Texto: docstring com corte em `app/ponto/services/cartorio.py:528`, afirmação em `:539`–`:542`. **AFIRMA:** o cartório copia a marca que recebeu, sem inferir isenção pela coincidência de horários. **FAZ:** `_carimbar_isencao` só escreve `isencao=True` quando a célula recebida traz marca verdadeira (`:556`–`:557`); os dois ramos produtores chamam esse propagador (`:680`, `:691`). **AINDA VALE; alta.** As medições históricas do docstring não são confirmação de produção neste lote `[nao verificado]`.

9. **Tipo e — noturno ainda DENTRO do turno não acusa furo.** Texto: `app/ponto/tests/test_c1_cartorio_pergunta_a_juiza.py:13`–`:16`: turno 19h–07h, agora 06h do dia seguinte. **FAZ:** o teste chama o cartório real e exige ausência de códigos `FURO` (`:31`–`:34`); a implementação fornece o resultado de `dias_encerrados` ao classificador (`app/ponto/services/cartorio.py:1105`, `:1423`), que usa `turno_encerrado` (`app/ponto/supra_juiz.py:278`). **AINDA VALE; alta por leitura; teste executado `[nao verificado]`.**

10. **Tipo a — vigia não apaga marco ainda não vencido.** Texto: corte em `app/ponto/services/cartorio.py:1309`–`:1310`. **AFIRMA:** hoje só entram marcos vencidos; pendentes são “não sei”, nunca apagados. **FAZ:** sem marco vencido o julgamento é pulado (`:1313`–`:1317`); colunas missing pendentes saem do dia julgado (`:855`–`:859`); lâmpadas apagadas pendentes viram `acesa=None, pendente=True`, e `n_missing` é recalculado (`:868`–`:872`). **AINDA VALE; alta.** A cifra de 30 minutos exige também a leitura de `marcos_vencidos` e do juiz de vencimento; nesta conclusão credito apenas a semântica vencido/pendente, confirmada nas linhas citadas.

| Item | Tipo | Texto:linha | Código:linha | VEREDITO | Certeza |
|---|---|---|---|---|---|
| `--forcar` só sombra | e | `app/ponto/tests/test_carimbo_isencao_lampada.py:29` | `app/ponto/management/commands/processar_cartorio.py:65` | NAO VALE MAIS | Alta |
| Teto de instante só hoje | b | `app/docs/LEIS.md:167` | `app/ponto/services/cartorio.py:1423` | NAO VALE MAIS | Alta |
| L-033 retém todo furo | b | `app/docs/LEIS.md:81` | `app/folha/export.py:1235` | NAO VALE MAIS | Alta |
| Horizonte da isenção | b | `app/docs/LEIS.md:145` | `app/ponto/services/cartorio.py:520` | AINDA VALE | Alta, escopo indicado |
| Retratada não julga | a | `app/ponto/services/cartorio.py:1109` | `app/ponto/turnos.py:2864` | AINDA VALE | Alta |
| `cobrado` lê marcos atuais | a | `app/ponto/services/cartorio.py:1447` | `app/ponto/services/cartorio.py:1451` | AINDA VALE | Alta |
| Parcial sem período, só lista | a | `app/ponto/services/cartorio.py:379` | `app/ponto/services/cartorio.py:398` | AINDA VALE | Alta, cartório |
| Propagação da isenção | a | `app/ponto/services/cartorio.py:539` | `app/ponto/services/cartorio.py:556` | AINDA VALE | Alta |
| DENTRO do noturno | e | `app/ponto/tests/test_c1_cartorio_pergunta_a_juiza.py:15` | `app/ponto/services/cartorio.py:1423` | AINDA VALE | Alta estática |
| Pendente é “não sei” | a | `app/ponto/services/cartorio.py:1309` | `app/ponto/services/cartorio.py:870` | AINDA VALE | Alta |

## 05/11 15:48 · sessao 5aefd047a3fe · CONTRATO-20-CENSO · censo · origem=Code

**Números atuais de `juliani`, inclusive últimos 30 dias: [não verificado].** Abaixo, “sem pergunta viva” usa as resolvedoras de `perguntas_que_resolvem`, como o ramo examinado. **Certeza alta.** (`app/chamados/management/commands/disparar_perguntas_competencia.py:145–149`)

**(a) Caminhos que produzem ou conservam o estado**

As possibilidades abaixo são demonstradas pelo código; sua incidência atual em produção é **[não verificado]**.

| Caminho | Ato ou ausência de ato | Certeza e contraexemplo |
|---|---|---|
| **Nascimento sem emissão de pergunta** | `abrir()` e `criar()` não criam diretamente pergunta. A grade abre o chamado e retorna expressamente sem fio. (`app/chamados/models.py:932–942`; `app/chamados/models.py:1048–1050`; `app/ponto/management/commands/reconciliar_grade.py:507–532`) | **Alta.** Contraexemplo: o cartório emite perguntas quando a célula foi julgada naquela passada. (`app/ponto/services/cartorio.py:981–997`) |
| **Falha depois de criar o chamado** | `detectar_ausencias` abre primeiro; a tentativa posterior de construir o fio é `best-effort`, com exceção absorvida. Portanto, falhar essa tentativa não desfaz necessariamente o nascimento. (`app/ponto/management/commands/detectar_ausencias.py:693–711`; `:787–803`) | **Alta.** Contraexemplo: quando a construção do fio sucede, esse caminho pode entregar pergunta. (`app/ponto/management/commands/detectar_ausencias.py:795–801`) |
| **Reabertura sem pergunta restaurável** | O dedup pode renascer um encerrado; `reabrir()` grava estado vivo e chama a ressuscitação, sem fabricar pergunta nova. A ressuscitação considera somente perguntas materializadas e sem texto, consulta marco/C6 e pode retornar zero. (`app/chamados/models.py:867–885`; `:1379–1392`; `:1423–1447`; `:1481–1487`) | **Alta.** Contraexemplo: havendo candidata elegível, a porta efetivamente descarimba perguntas. (`app/chamados/models.py:1488–1493`) |
| **Encerramento da última pergunta por identidade/duplicata** | `retratar_duplicata()` carimba a pergunta; não encerra diretamente seu chamado. Se era a última resolvedora viva e a canônica está em outro chamado, o antigo pode ficar mudo. (`app/chamados/services/fio.py:878–895`; `app/chamados/juizes.py:1105–1114`) | **Alta para o mecanismo; distribuição concreta [não verificado].** Contraexemplo: resposta ainda pendente de decisão pode manter viva até uma duplicata, pois precede a identidade no juiz. (`app/chamados/juizes.py:1092–1114`) |
| **Validação/materialização com outro furo residual** | A validação torna a pergunta morta para o C6. A materialização tenta reconciliar o destino, mas o chamado sobrevivente pode permanecer vivo porque o dia ainda está furado para a folha. (`app/chamados/services/validacao.py:393–396`; `app/chamados/juizes.py:1077–1079`; `app/chamados/services/materializacao.py:728–748`; `app/chamados/models.py:701–734`) | **Alta.** Contraexemplo: premissa regularizada ou outra cobrança sobrevivente pode autorizar o fechamento. (`app/chamados/models.py:701–724`) |
| **Morte por leitura, sem novo carimbo na pergunta** | Tranca e determinadas condições da célula fazem o juiz devolver morta. Se o chamado continuar vivo e não houver outra resolvedora viva, surge a condição bruta de fio mudo. (`app/chamados/juizes.py:1080–1088`; `:1122–1138`) | **Alta.** Contraexemplos: resposta discordante ainda por decidir ou marco pendente podem conservar a pergunta viva. (`app/chamados/juizes.py:1092–1104`; `:1127–1138`) |
| **Transferência da última resolvedora** | `reapontar_resolvedora()` pode trocar `chamado_destino`; seu corpo não encerra o destino anterior. Se ele continuar vivo e perder sua última resolvedora viva, fica mudo na relação examinada. (`app/chamados/services/fio.py:829–849`) | **Média como causa concreta.** Contraexemplo: destino anterior já terminal ou com outra pergunta viva não produz esse estado. (`app/chamados/catalogo/motor.py:24–31`; `app/chamados/juizes.py:1175–1184`) |

**O contador bruto não equivale a defeito estrutural.** `registrado` pertence a `VIVOS`, embora não peça ação. Ressuscitar por resposta em chamado silenciado também pode apenas levantar o véu: o silêncio não alterava o estado vivo. **Certeza alta.** (`app/chamados/catalogo/motor.py:24–29`; `app/chamados/models.py:81–86`; `app/chamados/services/materializacao.py:682–694`)

**(b) Avaliação das duas alternativas**

| Alternativa | Cobertura e lacunas | Certeza / contraexemplo |
|---|---|---|
| **(i) Pergunta no evento de abertura de `batida_ausente`** | Cobre prospectivamente os nascimentos que atravessem a porta instrumentada; corresponde ao aval documentado. Não basta para perda posterior da última pergunta, reabertura sem nascimento ou outros tipos de chamado. (`app/docs/PROMPTS.md:1853`; `app/chamados/models.py:1379–1392`; `app/chamados/services/fio.py:878–895`) | **Alta para o alcance causal; implantação [não verificado].** Contraexemplo: a última resolvedora morre por duplicata depois da abertura; não ocorre novo nascimento. (`app/chamados/services/fio.py:878–895`) |
| **(ii) Emissor com papel próprio, preservando o ramo atual** | A varredura alcança estoque existente e reabertos, mas somente com célula na janela. Ainda exclui parcial da lista, ausência de motivo elegível, desligado e qualquer pergunta histórica do fato. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141–175`; `:211–217`) | **Alta para o comportamento atual.** Contraexemplo: existe pergunta morta do mesmo fato; o chamado continua sem viva, mas o ramo o barra em `fato_ja_existe`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:211–215`) |

**O evento já existente não demonstra cobertura de (i).** O outbox explícito de `abrir()` vale para `MODULOS_DE_TURNO`, conjunto que não contém `batida_ausente`; seu consumidor executa retratação e encerramento de órfãs. A emissão de perguntas fica no cartório. **Certeza alta.** (`app/chamados/models.py:935–941`; `app/chamados/reconciliador.py:334–337`; `:1322–1346`; `app/ponto/services/cartorio.py:981–997`)

**Limite normativo de (ii):** o cadastro não declara papel `emissor`; o comando continua juiz, e a lei exige varredura como vigia. Reclassificar papel para obter verde é expressamente proibido. Para legitimar um papel autônomo de emissor dessa varredura de estado, faltaria corte que o definisse e autorizasse; tal autorização é **[não verificado]**. A avaliação normativa para aqui. **Certeza alta sobre o cadastro e a restrição existentes.** (`app/config/crons.py:1955`; `:1995–1999`; `CLAUDE.md:168–170`; `app/core/contratos_estruturais.py:269`)

**(c) O que há sobre C11**

- **Texto operacional, sim — certeza alta:** “FIO-MUDO = C11 antes” manda medir se as 12 células têm furo; sem furo, morrer por lastro; com furo, opção **(i), evento de abertura**. O aval posterior especifica abertura de `batida_ausente`. (`app/docs/PROMPTS.md:1519`; `:1853`)
- **CORTES:305 não define C11:** registra a condição de saída “fio mudo medido e C11 pousar”; o selo repete essa condição. **Certeza alta.** (`app/docs/CORTES.md:305`; `bin/tests/test_papel_prazo_nao_deriva.sh:302–307`)
- **Contraexemplo à identificação apenas pelo nome:** `_C11` no registro significa “a ausência do dia suspende, fecha ou levanta a cobrança viva?”, com autoridade em `silencio_ausencia`. Sua equivalência com o rótulo de CORTES é **[não verificado]**. (`app/core/juizes.py:117`; `:271`)
- **Commit que defina ou implemente esse C11: [não verificado].** [nao verificado]

**(d) Sonda**

A sonda usa `VIVOS` e o juiz `fio_vivo`, que delega a `pergunta_viva`; não substitui o juiz por `q_vivas()`. Conta o universo amplo de chamados, inclusive sem célula e fora da competência. Origem significa `modulo_origem`; tipo significa `categoria/subcategoria`. **Certeza alta sobre essas definições.** (`app/chamados/catalogo/motor.py:24`; `app/chamados/juizes.py:1175–1184`; `app/chamados/models.py:2753–2762`; `:38–40`)

“Criados em 30 dias” usa `criado_em`; a condição de mudo é avaliada agora, sem reconstruir seu estado no nascimento. Execução e resultados em Django: **[não verificado]**. (`app/chamados/models.py:61`; `app/chamados/management/commands/disparar_perguntas_competencia.py:148–149`)

===SONDA===
```python
exec(r'''
import sys
sys.dont_write_bytecode = True
import logging
from collections import defaultdict
from datetime import timedelta

from django.contrib.auth import get_user_model
from django.db import connection, transaction
from django.db.models import Prefetch
from django.utils import timezone
from django_tenants.utils import schema_context

# app/chamados/catalogo/motor.py:24
# app/chamados/juizes.py:1175-1184
from chamados.catalogo.motor import VIVOS
from chamados.juizes import fio_vivo, um_colaborador_por_identidade
from chamados.models import ChamadoColaborador, PerguntaDisputa
from core.observ import engolidos


def medir_somente_leitura():
    if connection.vendor != "postgresql" or connection.in_atomic_block:
        raise RuntimeError("Requer PostgreSQL e shell fora de atomic.")
    if not connection.get_autocommit():
        raise RuntimeError("Requer autocommit na entrada.")

    with schema_context("juliani"), transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cursor.execute(
                "SELECT current_schema(), "
                "current_setting('transaction_read_only')"
            )
            if cursor.fetchone() != ("juliani", "on"):
                raise RuntimeError("Tenant ou modo somente leitura incorreto.")

        # O juiz pode chamar get_user_sistema(), que usa get_or_create.
        # app/chamados/juizes.py:1014-1017; app/core/utils.py:18
        if not get_user_model().objects.filter(username="sistema").exists():
            raise RuntimeError("Usuario sistema ausente; abortado sem criar.")

        agora = timezone.now()
        hoje = timezone.localdate(agora)
        inicio = agora - timedelta(days=30)
        recentes = dict(criado_em__gte=inicio, criado_em__lte=agora)

        base = ChamadoColaborador.objects.all()
        universo = base.count()
        universo_30d = base.filter(**recentes).count()
        vivos = base.filter(status_local__in=VIVOS).order_by("pk")

        perguntas = PerguntaDisputa.objects.select_related(
            "celula",
            "chamado_destino__celula",
            "disputa__colaborador__empresa",
        )
        # Mesma relacao do ramo: disparar_perguntas_competencia.py:147-149
        vivos = vivos.prefetch_related(Prefetch(
            "perguntas_que_resolvem",
            queryset=perguntas,
            to_attr="_sonda_ps",
        ))

        # vivos, mudos, vivos_criados_30d, mudos_criados_30d
        grupos = defaultdict(lambda: [0, 0, 0, 0])
        totais = [0, 0, 0, 0]
        identidade = {}
        falhas_antes = engolidos()["total"]

        for chamado in vivos.iterator(chunk_size=200):
            mudo = not fio_vivo(
                um_colaborador_por_identidade(
                    chamado._sonda_ps, identidade
                )
            )
            recente = inicio <= chamado.criado_em <= agora
            chave = (
                chamado.modulo_origem,
                chamado.categoria,
                chamado.subcategoria,
            )
            valores = (
                1, int(mudo), int(recente), int(mudo and recente)
            )
            for i, numero in enumerate(valores):
                grupos[chave][i] += numero
                totais[i] += numero

        if engolidos()["total"] != falhas_antes:
            raise RuntimeError("Juiz registrou falha; contagem nao publicada.") [nao verificado]
        if timezone.localdate() != hoje:
            raise RuntimeError("Virou o dia durante a sonda; repetir.")

        return (
            agora, hoje, inicio, universo, universo_30d, totais, grupos
        )


# Evita que caminhos de erro do juiz escrevam em handlers de arquivo.
# app/core/observ.py:73-82
nivel_anterior = logging.root.manager.disable
logging.disable(logging.CRITICAL)
try:
    resultado = medir_somente_leitura()
finally:
    logging.disable(nivel_anterior)

agora, hoje, inicio, universo, universo_30d, totais, grupos = resultado
print("tenant=juliani; hoje=%s; instante=%s" % (
    hoje, agora.isoformat()
))
print("criados_30d: [%s, %s]" % (
    inicio.isoformat(), agora.isoformat()
))
print("universo_todos_chamados=%d" % universo)
print("universo_todos_chamados_criados_30d=%d" % universo_30d)

nomes = (
    "universo_vivos",
    "vivos_sem_pergunta_viva",
    "universo_vivos_criados_30d",
    "vivos_criados_30d_sem_pergunta_viva",
)
for nome, numero in zip(nomes, totais):
    print("%s=%d" % (nome, numero))

for chave, valores in sorted(grupos.items()):
    print("origem=%r categoria=%r subcategoria=%r" % chave)
    for nome, numero in zip(nomes, valores):
        print("  %s=%d" % (nome, numero))
''')
```
===FIM===

## 05/11 15:48 · sessao 26be018ea890 · - · - · origem=Ronald (tmux)

Levantamento de 12 candidatos, sem executar testes nem escrever arquivos; os contraexemplos abaixo são entradas deduzidas do fluxo, não observações de produção. [nao verificado] [nao verificado]

1. **DNA documentado como versão 1 — tipo d.** AFIRMA: o snapshot tem `dna_versao 1` (`app/escala/models.py:1699`). FAZ: o produtor define `DNA_VERSAO = 2` e grava essa constante no snapshot (`app/ponto/management/commands/gerar_celulas.py:16`, `app/ponto/management/commands/gerar_celulas.py:96`). **NAO VALE MAIS; certeza alta.**

2. **Toda reescrita denuncia o ato — tipo a.** AFIRMA: a célula denuncia a própria reescrita, guardando DNA anterior e contador (`app/escala/models.py:1704`, `app/escala/models.py:1714`). FAZ: o ramo regular atualiza os três campos (`app/ponto/portas/celula.py:259`); o ramo fora da vigência altera `trabalha` e `dna`, salvando somente esses dois campos (`app/ponto/portas/celula.py:225`, `app/ponto/portas/celula.py:233`). Contraexemplo: célula gerada, `trabalha=True`, geradora igual ao vínculo, dia sem cobertura desse vínculo nem de outro e fora de competência exportada; seu DNA é substituído sem incrementar `regeneracoes` nem guardar `dna_anterior` (`app/ponto/portas/celula.py:226`). **CONTRADIZ; certeza alta.**

3. **L-020: denúncia da exceção ao passado imutável — tipo b.** AFIRMA: correção por cadastro reescreve o passado, mas a célula denuncia o ato (`app/docs/LEIS.md:71`). FAZ: a mesma exceção de denúncia do item 2 permanece no ramo fora da vigência (`app/ponto/portas/celula.py:225`, `app/ponto/portas/celula.py:233`). Contraexemplo e certeza iguais ao item 2. **CONTRADIZ; certeza alta.** É duplicação documental útil; pode ser omitido para poupar universo.

4. **L-052: exportada não se toca, nem para corrigir — tipo b.** AFIRMA essa proibição no enunciado, embora a própria linha reconheça superação parcial (`app/docs/LEIS.md:93`). FAZ: `apesar_da_lavra` preenchido torna vazia a lista de competências protegidas e permite alcançar a reescrita (`app/ponto/portas/celula.py:196`, `app/ponto/portas/celula.py:251`). Contraexemplo: chamar regeneração com motivo em `apesar_da_lavra`, célula gerada em competência exportada e DNA diferente (`app/ponto/portas/celula.py:196`, `app/ponto/portas/celula.py:251`). **CONTRADIZ no enunciado; certeza alta.** A própria documentação já registra a exceção (`app/docs/LEIS.md:166`).

5. **L-113: revogação da guarda de exportada como trava — tipo b.** AFIRMA que a guarda é revogada como trava; o diagnóstico ao final da própria linha reconhece que essa cláusula ainda falta (`app/docs/LEIS.md:149`). FAZ: sem `apesar_da_lavra`, consulta as competências exportadas e pula os respectivos dias (`app/ponto/portas/celula.py:198`, `app/ponto/portas/celula.py:238`). Contraexemplo: correção ordinária sem esse argumento em dia exportado retorna sem reescrever o dia e registra bloqueio (`app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:442`). **CONTRADIZ a cláusula normativa; certeza alta.** Não tratar a linha inteira como mentira: ela declara a lacuna (`app/docs/LEIS.md:149`).

6. **720 minutos: menor distância assinada — tipo c.** AFIRMA: o leitor normaliza a diferença de relógios para a menor distância assinada (`app/escala/services/leitor_celula.py:15`). FAZ: subtrai 1440 quando `d > 720` e soma 1440 quando `d < -720`; exatamente ±720 ficam preservados (`app/escala/services/leitor_celula.py:29`, `app/escala/services/leitor_celula.py:33`). **AINDA VALE; certeza alta.** A regra calcula desvio da lâmpada, não duração remunerada (`app/escala/services/leitor_celula.py:121`, `app/escala/services/leitor_celula.py:132`).

7. **240 minutos: piso do intervalo — tipo c.** AFIRMA: até quatro horas o piso é zero; acima de quatro até seis, quinze minutos; acima de seis, sessenta (`app/escala/models.py:7`). FAZ: compara primeiro com 360, depois com 240 e retorna respectivamente 60, 15 ou 0 (`app/escala/models.py:17`). **AINDA VALE no algoritmo; certeza alta.** É confronto interno do texto com código, sem conclusão jurídica externa.

8. **Lei da lâmpada: status pelo delta — tipo a.** AFIRMA: o status sai do desvio, não apenas do fato de estar acesa (`app/escala/services/leitor_celula.py:130`). FAZ: a lâmpada acesa chama `_status_da_luz(_d)`, que delega à mesma `status_do_delta` do builder (`app/escala/services/leitor_celula.py:132`, `app/escala/services/leitor_celula.py:56`, `app/escala/utils.py:299`). Delta inexistente produz `None` (`app/escala/services/leitor_celula.py:56`). **AINDA VALE; certeza alta.**

9. **Ausência parcial transportada, não recalculada — tipo a.** AFIRMA: os carimbos da parcial vêm da ata; a antiga consulta da ausência atual foi retirada (`app/escala/services/leitor_celula.py:400`, `app/escala/services/leitor_celula.py:419`). FAZ: copia `cobertura_parcial` e `periodo_parcial` somente quando presentes na ata (`app/escala/services/leitor_celula.py:404`). **AINDA VALE nesse leitor; certeza alta.** A ausência de função com o nome antigo pode ser conferida pelo arquivo completo; o corpo atual termina no transporte (`app/escala/services/leitor_celula.py:404`, `app/escala/services/leitor_celula.py:416`).

10. **Extra fixa passada não herda template atual — tipo a.** AFIRMA: célula existente sem a chave responde `(0,0)`; template somente sem célula (`app/escala/models.py:1213`, `app/escala/models.py:1217`). FAZ: lê `extra_declarada` do DNA; chave ausente ou valor inválido resulta em zero; só ausência de célula consulta template (`app/escala/models.py:1234`). **AINDA VALE; certeza alta.**

11. **HE autorizada antes/depois do marco — tipo d.** AFIRMA: os campos declaram minutos de HE autorizada antes e depois dos marcos (`app/escala/models.py:80`, `app/escala/models.py:85`). FAZ: congela os números no DNA para dia de trabalho (`app/ponto/management/commands/gerar_celulas.py:119`); o leitor do dia extrai os números congelados (`app/escala/models.py:1234`); a janela desconta a autorização antes de decidir quanto fica fora (`app/ponto/janela_he.py:73`, `app/ponto/janela_he.py:99`). **AINDA VALE para autorização; certeza alta.** A autorização é teto, não pagamento automático, explicitamente documentado em `app/ponto/janela_he.py:68`.

12. **Toda lâmpada emitida tem `tipo_real` — tipo e.** AFIRMA: a chave existe SEMPRE, com `None` quando desconhecida (`app/ponto/tests/test_hx_tipo_real_na_ata.py:41`). FAZ: todos os pontos de construção de lâmpadas incluem a chave, tanto proximidade/desconhecimento quanto atribuição e forma posicional (`app/ponto/services/cartorio.py:638`, `app/ponto/services/cartorio.py:642`, `app/ponto/services/cartorio.py:677`, `app/ponto/services/cartorio.py:688`). **AINDA VALE para lâmpadas construídas pelo produtor atual; certeza alta.** Não estender a todas as atas históricas existentes: o leitor aceita ausência da chave e a trata como desconhecimento (`app/escala/services/leitor_celula.py:124`).

Achado auxiliar: `_overlay_isencao` ainda documenta que só toca dia de trabalho, preserva julgamento de batida real e não cobre célula sem `hora_prevista` (`app/escala/services/leitor_celula.py:204`). Sua execução delega à regra central (`app/escala/services/leitor_celula.py:212`), que muda status de batida real para `ok` e cobre também células sem marco, sem guarda por tipo do dia (`app/escala/utils.py:847`, `app/escala/utils.py:857`). Contraexemplo: dia `folga` com célula `{'hora':'08:30','missing':False,'status':'atraso'}` passa a `status='ok'` (`app/escala/utils.py:850`). **CONTRADIZ; certeza alta.** Para o universo estrito, o docstring não traz literalmente LAPIDE/LEI/corte/L-NNN; o comentário imediatamente seguinte traz `corte` e declara delegação (`app/escala/services/leitor_celula.py:209`). Recomendo usar como evidência auxiliar, não contar automaticamente como item a.

## 05/11 15:47 · sessao c063f3f2ca11 · CONTRATO-20-CENSO · censo · origem=Code

**A O169 ainda tem objeto: o fechamento por janela permanece no serviço chamado pelo evento e pelo comando manual. Certeza alta.** (`app/ponto/services/cluster_espurio.py:21-23,45-50,95-99`; `app/ponto/services/cartorio.py:990-994`)

**(a) Sim — fechamento por janela, certeza alta.**

O mecanismo é:

- A janela padrão tem **14 dias**; somente células desse intervalo são consultadas. (`app/ponto/services/cluster_espurio.py:12,21-23`)
- `datas` contém somente os dias com assinatura encontrada, mas a comparação percorre **todos os chamados vivos de cluster**, sem recorte temporal. Dia fora de `datas` recebe `FATO_CLUSTER_ESPURIO=False`. (`app/ponto/services/cluster_espurio.py:45-50,73-82`)
- O catálogo interpreta esse `False` como **premissa morta**, e o serviço chama `ch.reconciliar()`. Assim, um dia anterior à janela pode perder o chamado sem leitura da sua célula, mesmo que conserve a assinatura. (`app/chamados/catalogo/premissa.py:127-134`; `app/ponto/services/cluster_espurio.py:95-99`)

Há dois disparadores dessa decisão:

| Caminho | Onde decide/escreve |
|---|---|
| Evento do cartório, havendo células julgadas | Chama `_cluster(c, hoje)`, usando os 14 dias padrão. (`app/ponto/services/cartorio.py:981-994`) |
| Comando manual `detectar_cluster_espurio --apply` | Chama o mesmo serviço com `--dias`, cujo padrão é 14. (`app/ponto/management/commands/detectar_cluster_espurio.py:26-29,34-46`) |

**O fechamento é efetivável, não apenas candidato — certeza alta.** O cluster canônico nasce com categoria `batida_espuria`; seu módulo/categoria não pertencem ao universo da guarda de furo. Por isso essa guarda autoriza sem consultar o dia. Sem pergunta viva impeditiva, `reconciliar()` grava `resolvido`, pela via sistema. (`app/ponto/services/cluster_espurio.py:109`; `app/chamados/catalogo/modulos.py:235-241`; `app/chamados/models.py:2955-2972,1194-1200,690-691,402-404,617-620`; `app/chamados/catalogo/motor.py:95,111`)

**Contraexemplos:** pergunta viva sem destino que a receba pode impedir o fechamento; chamado sem dia forte é ignorado. O cron configurado usa `--alarme` e não escreve. (`app/chamados/models.py:405-414`; `app/ponto/services/cluster_espurio.py:47-49`; `app/config/crons.py:598`; `app/ponto/management/commands/detectar_cluster_espurio.py:34-50`)

**Existe também encerramento por competência explicitamente trancada — certeza alta.** A seleção abrange cluster e o gate encerra sem consultar sua assinatura. As entradas são o ato de trancar e o comando manual. Essa via já está declarada na nota da célula. **Contraexemplo:** competência apenas passada, sem tranca, não entra; resgatados e chamados com perguntas vivas preservadas ficam. (`app/chamados/catalogo/modulos.py:115-116,253-272`; `app/chamados/services/competencia_trancada.py:82-103,135-146,181-184`; `app/chamados/models.py:1547-1558`; `app/ponto/services/fechamento.py:1404-1412`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`; `app/core/contratos_estruturais.py:268-269`)

**(b) Sim — dia não medido vira premissa morta do cluster, certeza alta.**

Sem célula, o dia não aparece na consulta; com célula sem ata, `ata={}` produz nenhuma assinatura. Nos dois casos, o dia fica fora de `datas` e vira `False`, seguindo o fechamento descrito acima. Isso prova “premissa morta/nada a cobrar pelo cluster”; gravação de rótulo “falta” ou “dia encerrado” por esse caminho: **[não verificado]**. (`app/ponto/services/cluster_espurio.py:23-35,45-50,73-82`; `app/chamados/catalogo/premissa.py:127-134`)

**Contraexemplo:** o próprio catálogo conserva a premissa quando recebe `None`; a leitura genérica `_celula_furada` retorna `None` sem célula ou sem lâmpadas na ata. Portanto, a conversão indevida está no coletor de cluster. (`app/chamados/catalogo/premissa.py:127-130`; `app/chamados/juizes.py:457-462,471-476`; `app/ponto/services/cluster_espurio.py:50`)

**(c) Ainda existe — certeza alta.** O sítio é `_vivos_que_a_premissa_matou`: decide morte pela ausência no conjunto recortado; `reconciliar_colab` executa o fechamento. Não se aplica a alternativa de apontar commit que o retirou. (`app/ponto/services/cluster_espurio.py:39-52,95-99`)

**(d) Sim — certeza alta.** A nota condiciona a célula à O169, e o mecanismo permanece. A lei já escolheu **(b)**: cluster não fecha por idade; fechamento por janela é dívida a remover. Logo, há objeto técnico comprovado e não falta decisão de negócio sobre essa alternativa. (`app/core/contratos_estruturais.py:269`; `app/docs/BACKLOG.md:424-426`; `app/ponto/services/cluster_espurio.py:21-23,45-50,95-99`)

## 05/11 15:46 · sessao 73834736df25 · CRIVO-AINDA-VALE · censo · origem=Code

A promessa de preservar o gravado após exportação encontra um caminho de regravação no código; o `help_text` de materialização também promete efeitos que o campo não garante. (`app/docs/LEIS.md:125`; `app/ponto/services/fechamento.py:1144`; `app/chamados/models.py:2782`; `app/chamados/models.py:2628`)

A certeza abaixo se refere à leitura estática. Ocorrência, quantidade afetada e efeito financeiro em produção: **[não verificado]**. Correspondência independente desta cópia ao hash informado: **[não verificado]**.

Os contraexemplos de **CONTRADIZ** são caminhos construídos a partir do código, sem afirmar ocorrência em produção:

- **A3:** pergunta respondida, não validada, sem tranca e sem decisão administrativa: se resposta e batida concordam e o dia não acusa, a pergunta morre mesmo sem decisão do admin. (`app/chamados/juizes.py:1074`; `app/chamados/juizes.py:1078`; `app/chamados/juizes.py:1104`)
- **B1:** competência 10/2026 exportada, colaborador elegível, recálculo com escrita e valores novos: a guarda específica cobre apenas 09/2026; nos demais casos o escritor pode atualizar o fechamento. (`app/ponto/services/fechamento.py:37`; `app/ponto/services/fechamento.py:551`; `app/ponto/services/fechamento.py:1144`)
- **C4:** marcos 08:00/17:00 e duas batidas 13:00/22:00: o ramo de dia completo associa por posição, admitindo distâncias de 300 minutos antes de alcançar o teto de 240. (`app/chamados/services/desvio_escala.py:74`; `app/chamados/services/desvio_escala.py:77`; `app/chamados/services/desvio_escala.py:84`)
- **D3:** retratar uma pergunta duplicada ainda sem carimbo preenche `materializada_em` pela porta de identidade, sem exigir criação de batida. (`app/chamados/services/fio.py:891`; `app/chamados/models.py:2547`; `app/chamados/models.py:2628`; `app/chamados/models.py:2633`)
- **D4:** clone vivo com irmã respondida sem consumo permanece aberto: `_fecha` retorna antes da reconciliação. (`app/chamados/services/materializacao.py:729`; `app/chamados/services/materializacao.py:737`; `app/chamados/services/materializacao.py:748`)
- **E1:** colaborador ativo, autenticado, tipo `E`, sem latitude: a API devolve `403/gps_obrigatorio`. (`app/api/views.py:415`; `app/api/views.py:438`; `app/api/views.py:449`; `app/api/views.py:466`)
- **E2:** `respondida_em` preenchido com `resposta_colab=''`: o texto admite o carimbo como anuência, mas a materialização recusa por ausência de resposta. (`app/chamados/tests/test_sem_resposta_nao_planta_batida.py:14`; `app/chamados/services/materializacao.py:796`; `app/chamados/services/materializacao.py:808`)
- **E3:** uma linha acesa com frase de 600 caracteres e URL final entra inteira no montador, mas a persistência corta em 500 caracteres; a linha não sai inteira nem vira excedente contado. (`app/pautas/semanal.py:168`; `app/pautas/semanal.py:180`; `app/pautas/semanal.py:278`; `app/pautas/services.py:185`)

| item | tipo a-e | texto:linha — o que AFIRMA | codigo:linha — o que FAZ | VEREDITO | certeza |
|---|---|---|---|---|---|
| A1 | a | `app/chamados/services/validacao.py:731`: envelope do lote desce para a tolerância da folha. | `app/chamados/services/validacao.py:751` importa a constante do motor; `app/ponto/motor_calculo_v2.py:60` fixa 10; `app/chamados/services/validacao.py:822` aplica o limite. | AINDA VALE | Alta |
| A2 | a | `app/chamados/services/validacao.py:720`: fila dividida em **quatro** classes. | `app/chamados/services/validacao.py:730` declara cinco classes; `app/chamados/services/validacao.py:821` separa “sem marco”; `app/chamados/services/validacao.py:873` monta cinco grupos. | NAO VALE MAIS | Alta |
| A3 | a | `app/chamados/juizes.py:1093`: resposta do colaborador vence tudo, exceto decisão explícita do admin. | `app/chamados/juizes.py:1104` também mata a pergunta quando a resposta concorda com a luz e o dia não acusa, conforme `app/chamados/juizes.py:1074`. Contraexemplo A3 acima. | CONTRADIZ | Alta |
| A4 | a | `app/chamados/juizes.py:467`: consulta da véspera preserva o efeito pedido, inclusive `para='folha'`. | `app/chamados/juizes.py:469` repassa `para=para` à chamada recursiva; `app/chamados/juizes.py:446` usa esse parâmetro na consulta de ausência. | AINDA VALE | Alta |
| A5 | a | `app/chamados/services/vigia_fio.py:420`: hoje e futuro ficam fora do contador alarmado de fio morto. | `app/chamados/services/vigia_fio.py:419` testa `data >= hoje`; `app/chamados/services/vigia_fio.py:422` contabiliza separadamente e interrompe esse caminho. | AINDA VALE | Alta |
| A6 | a | `app/pautas/services.py:324`: enviadas incluem feitas; canceladas não voltam. | `app/pautas/services.py:327` usa `nao_canceladas()`, cujo filtro em `app/pautas/models.py:64` exclui cancelamento sem excluir conclusão. | AINDA VALE | Alta |
| A7 | a | `app/chamados/reconciliador.py:401`: a lápide diz que só entra pergunta sem resposta. | `app/chamados/reconciliador.py:364` recusa texto não vazio; `app/chamados/reconciliador.py:405` encaminha o carimbo pela porta. “Sem resposta” aqui significa sem texto. | AINDA VALE | Alta |
| A8 | a | `app/chamados/catalogo/motor.py:84`: “com 30d a folha de um jeito ou de outro fechou”. | `app/chamados/services/arquivo.py:118` apenas calcula silêncio por 30 dias; não comprova fechamento da folha. Faltam datas e estados dos fechamentos correspondentes. | NAO SE SABE | Alta sobre insuficiência; fechamento real [não verificado] |
| B1 | b | `app/docs/LEIS.md:125`: exportada não muda o gravado; diferença vira Pauta DP. | `app/ponto/services/fechamento.py:532` registra aviso de exportada; `app/ponto/services/fechamento.py:551` aplica guarda específica; `app/ponto/services/fechamento.py:1144` permite atualizar os valores. Contraexemplo B1 acima. | CONTRADIZ | Alta |
| B2 | b | `app/docs/LEIS.md:189`: `motivos_a_perguntar` ainda cala emissão em dia exportado. | `app/chamados/services/fabrica_da_celula.py:91` inicia as guardas e `app/chamados/services/fabrica_da_celula.py:126` devolve motivos sem essa recusa. O desvio para pauta em exportada fica restrito ao limbo por `app/chamados/services/fabrica_da_celula.py:185`. | NAO VALE MAIS | Alta |
| B3 | b | `app/docs/LEIS.md:189`: a tranca recusa cobrança em `cobrar_dia.py`. | `app/chamados/services/cobrar_dia.py:37` registra toque humano; `app/chamados/services/cobrar_dia.py:39` julga com `toque=True`; `app/chamados/juizes.py:949` deixa de calar o dia tocado após a tranca. | NAO VALE MAIS | Alta |
| B4 | b | `app/docs/LEIS.md:92`: a implementação de `encerrar` ainda carimba perguntas com a via da tranca. | `app/chamados/services/competencia_trancada.py:77` inicia a lista de perguntas vazia; `app/chamados/services/competencia_trancada.py:125` passa a contar por leitura; os laços de escrita atingem chamados em `app/chamados/services/competencia_trancada.py:181` e disputas em `app/chamados/services/competencia_trancada.py:196`. | NAO VALE MAIS | Alta |
| B5 | b | `app/docs/LEIS.md:92`: nenhuma pergunta nova deve nascer com `competencia_trancada`. | `app/chamados/models.py:2514` registra a remoção do escritor antigo; `app/chamados/models.py:2567` recusa a via na porta; `app/chamados/juizes.py:1082` lê a tranca sem novo carimbo. | AINDA VALE | Alta |
| B6 | b | `app/docs/LEIS.md:90`: proposta é recalculada a cada corrida. | `app/chamados/services/proposta_no_fio.py:67` consulta propostas para exibição; `app/chamados/services/proposta_no_fio.py:106` recalcula antes da decisão. | AINDA VALE | Alta, no fluxo do fio |
| B7 | b | `app/docs/LEIS.md:92`: existem 1.994 perguntas históricas que ficam intocadas. | `app/chamados/juizes.py:1087` mantém leitura da via histórica, mas não prova quantidade ou conservação dos registros. Faltam fotografia histórica, dados atuais e trilha de alterações. | NAO SE SABE | Alta sobre insuficiência; acervo [não verificado] |
| C1 | c | `app/chamados/models.py:3004` e `app/chamados/models.py:3005`: `FALTA_DIA` e `ATESTADO` usam 720 minutos. | `app/chamados/services/materializacao.py:468` lê o mapa e `app/chamados/services/materializacao.py:487` repassa os minutos à criação. O caminho também permite reutilizar ausência existente em `app/chamados/services/materializacao.py:488`. | AINDA VALE | Alta, como parâmetro de criação |
| C2 | c | `app/chamados/services/materializacao.py:521`: ausência derivada da resposta recebe 720 minutos. | `app/chamados/services/materializacao.py:519` chama a criação com esse valor e `app/chamados/services/materializacao.py:523` desliga a checagem de parcial nesse ponto. | AINDA VALE | Alta |
| C3 | c | `app/chamados/services/declaracao_texto.py:25`: hora declarada precisa estar a até 240 minutos de marco faltante. | `app/chamados/services/declaracao_texto.py:138` rejeita distância maior; `app/chamados/services/declaracao_texto.py:140` também rejeita ambiguidade. Estar dentro do teto é necessário, mas não suficiente. | AINDA VALE | Alta |
| C4 | c | `app/chamados/services/desvio_escala.py:12`: batida casa com o marco mais próximo até quatro horas. | `app/chamados/services/desvio_escala.py:74` casa dias completos por posição, sem o teto; somente o ramo incompleto aplica proximidade e 240 em `app/chamados/services/desvio_escala.py:84`. Contraexemplo C4 acima. | CONTRADIZ | Alta |
| C5 | c | `app/chamados/catalogo/motor.py:79`: limiar N3 é 240 minutos. | `app/ponto/management/commands/escalonar_chamados_supervisao.py:145` usa a constante para incidente; `app/ponto/management/commands/escalonar_chamados_supervisao.py:170` reutiliza o limiar para aviso condicionado à janela comercial. | AINDA VALE | Alta |
| C6 | c | `app/chamados/juizes.py:1451`: distância circular a partir de 660 minutos é classificada como AM/PM. | `app/chamados/juizes.py:1455` devolve a classificação; `app/chamados/juizes.py:1553` mantém o mesmo limiar no delta da resposta; `app/chamados/services/validacao.py:822` encaminha AM/PM ao lote B. | AINDA VALE | Alta |
| D1 | d | `app/chamados/models.py:1679`: colaborador tem máximo de duas correções das próprias respostas. | `app/chamados/models.py:2129` recusa contador ≥2; `app/chamados/models.py:2132` exige resposta corrigível; `app/chamados/models.py:2136` incrementa a operação aceita. | AINDA VALE | Alta |
| D2 | d | `app/chamados/models.py:1683`: reabertura administrativa tem máximo de duas. | `app/chamados/models.py:2043` impõe o limite; `app/chamados/models.py:2056` incrementa. A porta do sistema não consome esse contador, conforme `app/chamados/models.py:2066` e `app/chamados/models.py:2080`. | AINDA VALE | Alta |
| D3 | d | `app/chamados/models.py:2782`: `materializada_em` significa “batida criada + chamado fechado”. | `app/chamados/models.py:2628` e `app/chamados/models.py:2633` também preenchem e gravam o campo por identidade. Há ainda materialização de ausência em `app/chamados/services/materializacao.py:519`. Contraexemplo D3 acima. | CONTRADIZ | Alta |
| D4 | d | `app/chamados/models.py:2797`: clones são fechados junto quando a pergunta é materializada. | `app/chamados/services/materializacao.py:748` visita clones, mas `app/chamados/services/materializacao.py:729` retém quem tem irmã respondida sem consumo; os demais ainda dependem da reconciliação em `app/chamados/services/materializacao.py:738`. Contraexemplo D4 acima. | CONTRADIZ | Alta |
| E1 | e | `app/chamados/tests/test_invariantes_disputa.py:193`: ativo + tipo válido implica batida “NUNCA bloqueada”; GPS ausente seria apenas sinal. | `app/api/views.py:449` exige coordenadas e `app/api/views.py:466` devolve 403. O próprio teste já excepciona esse resultado em `app/chamados/tests/test_invariantes_disputa.py:288`. Contraexemplo E1 acima. | CONTRADIZ | Alta |
| E2 | e | `app/chamados/tests/test_sem_resposta_nao_planta_batida.py:14`: anuência pode ser texto **ou** `respondida_em`, na docstring que cita “motor NUNCA fabrica batida” em `app/chamados/tests/test_sem_resposta_nao_planta_batida.py:9`. | `app/chamados/services/materializacao.py:796` exige texto; `app/chamados/services/materializacao.py:803` registra recusa e `app/chamados/services/materializacao.py:808` retorna. Contraexemplo E2 acima. | CONTRADIZ | Alta |
| E3 | e | `app/pautas/tests/test_pauta_texto.py:80`: linha “NUNCA” cortada; quem não cabe sai inteiro e contado. | `app/pautas/semanal.py:178` limita quantidade, sem orçamento de caracteres; `app/pautas/services.py:185` persiste apenas os primeiros 500. Contraexemplo E3 acima. | CONTRADIZ | Alta |
| E4 | e | `app/chamados/tests/test_o261_vespera_nao_absolve.py:2`: furo de hoje só absolve plantio de hoje; véspera não vale nessa avaliação. | `app/chamados/services/materializacao.py:156` obtém a data local do plantio; `app/chamados/services/materializacao.py:170` consulta somente essa data antes da decisão em `app/chamados/services/materializacao.py:181`. | AINDA VALE | Alta |
| E5 | e | `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:3`: parcial sem período vai à lista, nunca a chamado; inclui expressamente a fábrica do pai em `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:4`. | `app/ponto/services/cartorio.py:398` identifica o caso; `app/chamados/services/fabrica_da_celula.py:102` consulta o predicado e retorna antes da emissão em `app/chamados/services/fabrica_da_celula.py:104`. | AINDA VALE | Alta para a fábrica explicitamente citada |
| E6 | e | `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:3`: E2 pergunta só ao C6 se a pergunta vive. | `app/chamados/reconciliador.py:610` preserva a pergunta viva pelo C6; a consulta adicional em `app/chamados/reconciliador.py:616` impede carimbo permanente por tranca, sem substituir o julgamento de vitalidade. | AINDA VALE | Alta |
| E7 | e | `app/chamados/tests/test_cobrar_so_o_que_muda_a_folha.py:128`, docstring com “DENTRO” em `app/chamados/tests/test_cobrar_so_o_que_muda_a_folha.py:131`: prévia e envio usam o mesmo `por_colab`. | `app/chamados/views_cobrar.py:97` monta o mapa antes da bifurcação; `app/chamados/views_cobrar.py:109` conta e `app/chamados/views_cobrar.py:116` percorre o mesmo mapa. Entregas podem falhar, contabilizadas em `app/chamados/views_cobrar.py:138`. | AINDA VALE | Alta sobre o universo selecionado |
| E8 | e | `app/chamados/tests/test_invariantes_disputa.py:303`: retro “NUNCA” tem tipo invertido por escala; servidor grava o tipo enviado. | `app/api/views.py:432` lê o tipo; `app/api/views.py:684` o encaminha à porta; `app/ponto/registro_batida.py:153` e `app/ponto/registro_batida.py:158` criam com esse tipo. Dedup pode devolver existente em `app/ponto/registro_batida.py:122`, sem criar nova batida invertida. | AINDA VALE | Alta sobre a criação |

## 05/11 15:46 · sessao ec6967a32fdd · - · - · origem=Ronald (tmux)

Certeza alta: `arquivar_prescritos` e `arquivar_competencia_encerrada` apenas silenciam; não fecham cluster nem alteram `status_local`. Ambos delegam a `arquivo.silenciar`, cuja escrita inclui somente campos de silêncio e atualização. (`app/chamados/management/commands/arquivar_prescritos.py:94-100`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:288-298`; `app/chamados/services/arquivo.py:165-172`)

Contraexemplo à equivalência “arquivo = fechamento”: chamado prescrito por idade conserva o estado; a própria operação registra “Status e apuração inalterados”. (`app/chamados/management/commands/arquivar_prescritos.py:94-100`)

Certeza alta: há fechamento adicional por **competência efetivamente trancada**, abrangendo cluster, independentemente da assinatura atual do cluster. A seleção exige tranca, identifica a competência do chamado, exclui módulos de cadastro/atos DP e guarda resgatados/perguntas vivas; depois chama o gate que grava `via=competencia_trancada`. Cluster tem dono `supervisao`, portanto não cai na exclusão por cadastro. (`app/chamados/services/competencia_trancada.py:82-103,134-146,181-184`; `app/chamados/catalogo/modulos.py:115-116,253-272`; `app/chamados/models.py:1547-1558`)

Entradas desse fechamento: ato `trancar_periodo`, execução imediata ou comando em processo, e comando manual. (`app/ponto/services/fechamento.py:1404-1412`; `app/chamados/services/competencia_trancada.py:276-297`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:19-29`)

Contraexemplos: competência apenas passada/exportada sem tranca não entra; resgatado depois do fechamento ou chamado com pergunta viva preservada permanece. (`app/chamados/services/competencia_trancada.py:82-85,104-105,135-145`)

## 05/11 15:46 · sessao 8e958ce1789d · CONTRATO-20-CENSO · censo · origem=Code

A cópia ainda mantém `reconciliar_chamados` como juiz por varredura e o teto executável de `_gravar_estado (sem pergunta)` em **17**. **Certeza alta** — `app/config/crons.py:2018`; `app/ponto/tests/test_selo_chokepoint_escrita.py:112`.

`TURNO_ABERTO` aparece no relato como família que reúne `turno_aberto`, `turno_aberto_24h` e `disputa_supervisao/orfao_14h`; abaixo individualizo essas três classes concretas. O catálogo sustenta todas pelo fato `FATO_TURNO_ABERTO`. **Certeza alta** — `app/docs/RELATO.md:63`; `app/chamados/catalogo/premissa.py:46`.

Há dois estados distintos: `recompute_turnos` escreve a **geometria materializada**; `abrir`/`criar` escrevem o **estado inicial do chamado**, e `_gravar_estado` escreve suas transições. **Certeza alta** — `app/ponto/nucleo.py:48`; `app/chamados/models.py:899`; `app/chamados/models.py:1048`; `app/chamados/models.py:222`.

**Classe `turno_aberto`**

| Aspecto | Conclusão e evidência | Certeza |
|---|---|---|
| Ato produtor | `processar_alertas_turno` chama `_marco_critico` no lembrete `pos_20`; `_abrir_chamado_critico` usa `ChamadoColaborador.abrir`, com módulo `turno_aberto`, subcategoria `saida_pendente`, dia e entrada no contexto. `app/ponto/management/commands/processar_alertas_turno.py:140`, `:316`, `:335`. | Alta |
| Escritores e leitores | `abrir` persiste o nascimento; transições passam por `_gravar_estado`. O cron específico lê `tm.aberto` e chamados vivos; o reconciliador geral lê vivos e verifica a premissa. O N5 também lê essa classe para reaproveitá-la. `app/chamados/models.py:932`, `:195`; `app/ponto/management/commands/processar_alertas_turno.py:88`, `:162`; `app/chamados/reconciliador.py:1384`; `app/ponto/management/commands/processar_alertas_avancados.py:100`. | Alta |
| Deploy/reload | O estado é persistido; a inicialização desses apps registra signals, sem replay de fechamento. Depois, os comandos voltam a consultar o materializado. Atribuir uma rajada ao reload, porém, permanece **[nao verificado]**. `app/ponto/models.py:1196`; `app/ponto/apps.py:7`; `app/chamados/apps.py:8`; `app/ponto/management/commands/processar_alertas_turno.py:76`. | Alta para o mecanismo |
| Virada do dia | Meia-noite não altera automaticamente `status_local`. O lembrete depende do instante relativo ao fim previsto, com tolerância de 240 segundos; “vivo agora” não é materializado. `app/ponto/turnos.py:3158`; `app/ponto/models.py:1199`. | Alta |
| Lote sem batida nem `EventoCelula` | O cron de cinco minutos pode recomputar a janela de quem tem batida nas últimas 36 horas e retratar `turno_aberto` encerrado. O cron geral de quinze minutos também procura premissas mortas, sem exigir evento. O consumidor, sem pendentes, para. `app/ponto/management/commands/processar_alertas_turno.py:59`, `:67`, `:216`; `app/chamados/management/commands/reconciliar_chamados.py:36`, `:52`; `app/chamados/reconciliador.py:1309`. | Alta, condicionada à execução |
| Contraexemplo | Indisponibilidade durante toda a janela `pos_20` pode perder esse nascimento: fora da tolerância, `_na_hora` fica vazio e o comando pula. Não há replay nesse ramo. `app/ponto/turnos.py:3164`; `app/ponto/management/commands/processar_alertas_turno.py:120`. | Alta |

**Classe `turno_aberto_24h`**

| Aspecto | Conclusão e evidência | Certeza |
|---|---|---|
| Ato produtor | `processar_alertas_avancados` recompõe e examina **ontem**. N5 significa pelo menos 24 horas; primeiro reaproveita chamado existente, senão usa `ChamadoColaborador.criar`. Existe ainda emissão retroativa explícita por `abrir`. `app/ponto/management/commands/processar_alertas_avancados.py:22`, `:57`, `:100`, `:121`; `app/ponto/turnos.py:3136`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`. | Alta |
| Escritores e leitores | `criar` escreve o estado inicial; `_gravar_estado` escreve transições. O emissor lê o materializado e o estado dos chamados existentes; o reconciliador lê a premissa pelo turno específico. `app/chamados/models.py:1048`, `:195`; `app/ponto/management/commands/processar_alertas_avancados.py:58`, `:103`; `app/chamados/reconciliador.py:117`, `:286`. | Alta |
| Deploy/reload | Preserva chamado e materializado. A execução posterior do emissor recompõe antes de calcular idade; isso não demonstra que o reload tenha causado qualquer fechamento observado. `app/chamados/models.py:46`; `app/ponto/models.py:1196`; `app/ponto/management/commands/processar_alertas_avancados.py:57`. Causalidade produtiva **[nao verificado]**. | Alta para o mecanismo |
| Virada do dia | A virada muda o universo `ontem`; não encerra o chamado. O emissor lê somente `data_turno=ontem`. `app/ponto/management/commands/processar_alertas_avancados.py:20`, `:58`. | Alta |
| Lote sem batida nem `EventoCelula` | O comando configurado a cada trinta minutos pode recompor, emitir ou escalar sem batida nova. O fechamento por premissa continua alcançável pelo reconciliador geral. O fechador específico de cinco minutos não inclui `_24h` no seu filtro. `app/config/crons.py:363`; `app/ponto/management/commands/processar_alertas_avancados.py:57`, `:173`; `app/chamados/reconciliador.py:1389`; `app/ponto/management/commands/processar_alertas_turno.py:164`. | Alta |
| Contraexemplos | Ter 24 horas não obriga nascer `_24h`: pode apenas escalar `turno_aberto`. Além disso, uma entrada às 23h pode ainda estar abaixo de 24 horas na última execução do dia seguinte e sair do universo “ontem” na virada seguinte. `app/ponto/management/commands/processar_alertas_avancados.py:100`, `:173`, `:22`, `:58`; `app/ponto/turnos.py:3136`. | Alta; segundo caso é cenário deduzido |

**Classe `disputa_supervisao/orfao_14h`**

| Aspecto | Conclusão e evidência | Certeza |
|---|---|---|
| Ato produtor | Na API, a anomalia é detectada antes da batida e a disputa é criada/atualizada depois. O detector exige bloqueio e ausência de regularização pendente; o bloqueio recente usa `14 ≤ horas < 96`. Julgamento/lote também pode criar pela fábrica da célula. O pai nasce por `abrir`. `app/api/views_core.py:432`, `:777`; `app/chamados/detector_anomalias.py:31`; `app/ponto/services/triagem_batida.py:17`; `app/chamados/services/fabrica_da_celula.py:129`; `app/chamados/services/disputa_emissao.py:504`. | Alta |
| Escritores e leitores | `abrir` escreve o nascimento do pai; `_gravar_estado` escreve suas transições. O reconciliador identifica o **par exato** módulo/subcategoria. O fechamento da disputa também lê o estado do pai e tenta `ch.reconciliar()`. Motivos/contexto da disputa são escritos separadamente pela agregação. `app/chamados/models.py:932`, `:195`; `app/chamados/catalogo/premissa.py:49`; `app/chamados/signals.py:32`, `:42`; `app/chamados/services/disputa_emissao.py:471`. | Alta |
| Deploy/reload e virada | Inicialização registra signals; não produz uma tentativa de batida nem executa o detector. A premissa do chamado consulta materializado, dia e entrada; esse teste não contém extinção por meia-noite ou idade. `app/chamados/apps.py:8`; `app/api/views_core.py:432`; `app/chamados/reconciliador.py:117`, `:264`. Causa das rajadas **[nao verificado]**. | Alta para o mecanismo |
| Lote sem batida nem `EventoCelula` | A fábrica pode abrir a disputa por outro ato; o reconciliador de quinze minutos continua alcançando o pai vivo. O outbox atual não retrata **diretamente** esse módulo pelo ramo de chamados absolvidos. Pode alcançá-lo indiretamente se encerrar órfãs, fechar a disputa e o gate do pai permitir. `app/chamados/services/fabrica_da_celula.py:139`; `app/chamados/management/commands/reconciliar_chamados.py:37`, `:52`; `app/chamados/reconciliador.py:336`, `:964`, `:1138`; `app/chamados/signals.py:42`. | Alta |
| Contraexemplos | Acrescentar motivo `orfao_14h` a disputa existente não troca a subcategoria do pai; pai novo usa o primeiro motivo ordenado. Portanto, conter esse motivo não prova pertencer à classe exata. `disputa_supervisao_manual/orfao_14h` também é outra classe: sua premissa é **filhos vivos**. `app/chamados/services/disputa_emissao.py:471`, `:595`, `:601`; `app/chamados/catalogo/premissa.py:50`. | Alta |

Para as três classes, perceber premissa morta não garante fechamento: `_retratar` pode reter o chamado, e o gate ainda verifica premissa da folha e pergunta viva. Sem dia apurável, a coleta volta ao `exists` global, podendo outro turno aberto sustentar um chamado antigo. **Certeza alta; contraexemplos ao fechamento automático** — `app/chamados/reconciliador.py:518`, `:523`, `:286`; `app/chamados/models.py:648`, `:651`.

**Candidata (a): evento em `recompute_turnos`**

| Avaliação | Conclusão e evidência | Certeza |
|---|---|---|
| Mudança exigida | Registrar o fato junto da reconstrução, no mesmo `atomic`, e consumi-lo pelo reconciliador existente. Hoje o corpo apenas apaga e recria materializado. A cópia registra a escolha “JUIZ-3 (a) outbox na transação”; isso é decisão documental, não implementação demonstrada. `app/ponto/nucleo.py:57`; `app/chamados/reconciliador.py:1357`; `app/docs/PROMPTS.md:1853`. | Alta |
| Custo em sítios | **Mínimo: dois pontos funcionais**, produtor e consumidor; para rebaixar o cron, também command e cadastro de papéis, além dos selos. Número final de alterações e custo em queries **[nao verificado]**. `app/ponto/nucleo.py:40`; `app/chamados/reconciliador.py:1326`; `app/chamados/management/commands/reconciliar_chamados.py:51`; `app/config/crons.py:2018`. | Média para o mínimo de integração |
| Contraexemplo | Se nenhum ato chama recompute, reload/virada não produz evento por esse mecanismo. Emitir em todo rebuild também cria eventos sem mudança lógica; emitir somente por mudança exige comparar antes/depois, pois o procedimento recria as linhas. `app/ponto/nucleo.py:40`, `:58`; `app/ponto/management/commands/processar_alertas_turno.py:67`. | Alta |

**Candidata (b): consumidor O-OUTBOX executando o reconciliador inteiro**

| Avaliação | Conclusão e evidência | Certeza |
|---|---|---|
| Mudança exigida | Acrescentar `reconciliar_chamados_colab` ao consumidor. Atualmente ele chama apenas retratação pela célula e encerramento de órfãs pela lâmpada. A ampliação precisa preservar gates, retenções e o tratamento de falhas antes de marcar eventos processados. `app/chamados/reconciliador.py:1326`, `:1329`, `:1337`, `:1357`, `:518`. | Alta |
| Custo em sítios | **Um ponto funcional no consumidor**, mais produtores que cubram as alterações faltantes; command/cadastro/vigia para rebaixamento. Quantidade de produtores faltantes **[nao verificado]**. Há assimetria concreta: `abrir` emite evento no nascimento de módulo de turno com célula; `criar`, usado pelo `_24h`, retorna após criação/trilha. `app/chamados/models.py:935`, `:1048`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/config/crons.py:2018`. | Alta para a assimetria; média para custo |
| Contraexemplo | Fila vazia encerra o consumidor antes de visitar qualquer colaborador: ampliar somente seu corpo não cobre lote sem evento. Eventos já pendentes podem sobreviver ao relógio por órfãs adiadas, mas essa garantia não cria eventos ausentes. `app/chamados/reconciliador.py:1309`, `:1341`. | Alta |

**Candidata (c): `JUIZES_DO_PRAZO`**

| Avaliação | Conclusão e evidência | Certeza |
|---|---|---|
| Autoridade existente | Abertura temporal já delega: `processar_alertas_avancados → turno_aberto_ha`; `processar_alertas_turno → lembrete_fim_turno`. A REGRA-DE-PRAZO permite reutilização somente para a **mesma pergunta**. `app/config/crons.py:2098`, `:2100`; `CLAUDE.md:427`. | Alta |
| Mudança/custo | Reclassificar o reconciliador inteiro exigiria demonstrar equivalência e separar seus trabalhos que não são apenas prazo: retratação de premissas, reata de órfãs e rejunção. Há pelo menos esses três trabalhos; um diff válido não é determinável por simples troca de cadastro. `app/chamados/reconciliador.py:1357`; `app/chamados/management/commands/reconciliar_chamados.py:56`; `app/config/crons.py:1957`. | Alta; custo final **[nao verificado]** |
| Contraexemplo e limite de lei | Aos 24/72h, o juiz aumenta nível/urgência; não demonstra fim do turno. `dia_encerrado` também não equivale à morte dessa premissa: uma aberta pode expirar para o relógio e continuar com entrada sem saída no materializado. Para **extinguir essas classes por idade**, falta lei específica de extinção temporal; paro nessa decisão. `app/ponto/turnos.py:3136`, `:3143`, `:2008`; `app/chamados/reconciliador.py:97`, `:286`. | Alta |

**Selo `_gravar_estado`: 17 → 18**

| Pergunta | Resultado e evidência | Certeza |
|---|---|---|
| Qual teste? | `SeloChokepointEscritaTest.test_gravar_estado_sem_pergunta`. A fixture cria `turno_aberto`; a captura executa **`ch.reconciliar()`**, não `_gravar_estado` isoladamente. `app/ponto/tests/test_selo_chokepoint_escrita.py:225`, `:286`. | Alta |
| O teto passou? | No executável desta cópia, continua **17**. O RELATO descreve proposta com “1 EXISTS por transição”; PROMPTS registra aval posterior para 17→18. Aval registrado não comprova alteração presente nem medição. `app/ponto/tests/test_selo_chokepoint_escrita.py:112`; `app/docs/RELATO.md:63`; `app/docs/PROMPTS.md:1853`. | Alta |
| Qual query nova e origem? | SQL, tabela, filtro e ponto exato da nova EXISTS: **[nao verificado]**. Não se deve identificá-la com o evento disponível: `emitir_evento_celula` faz **INSERT**. `app/ponto/portas/celula.py:634`, `:639`. | Alta sobre o INSERT; delta não verificado |
| Mede o que diz? | Mede um **teto da pilha síncrona de `reconciliar()` nessa fixture**, incluindo gates e efeitos do save. Não isola o escritor nem atribui queries. `app/chamados/models.py:617`, `:620`, `:279`; `app/ponto/tests/test_selo_chokepoint_escrita.py:217`, `:289`. | Alta |
| Contraexemplos do selo | Se `reconciliar()` recusar e retornar `False`, o teste pode passar gastando menos: ele não confere retorno/estado final. Deslocar trabalho para `on_commit` também pode ocultar seu custo: `TestCase` não executa callbacks, e essa chave não integra a contagem específica da batida. `app/chamados/models.py:618`; `app/ponto/tests/test_selo_chokepoint_escrita.py:286`, `:174`, `:177`. | Alta |

Pontos sem resposta:

- **Quais atos produziram cada rajada após deploy, virada e lote:** **[nao verificado]**. O RELATO declara a associação, mas não fornece nesta evidência a sequência causal por chamado — `app/docs/RELATO.md:63`.
- **Se o materializado estava fresco e qual gate permitiu ou reteve cada fechamento produtivo:** **[nao verificado]**; são condições efetivas do caminho — `app/chamados/reconciliador.py:95`, `:518`; `app/chamados/models.py:648`.
- **Implementação e SQL do delta 17→18, número realmente executado e custo após commit:** **[nao verificado]**; a cópia mostra teto 17 e captura síncrona — `app/ponto/tests/test_selo_chokepoint_escrita.py:112`, `:174`, `:289`.
- **Cobertura completa, vazão e zero residual necessários ao rebaixamento:** **[nao verificado]**. O corte exige cobertura do ato produtor e medição; a célula continua explicitamente PARCIAL — `app/docs/CORTES.md:358`; `app/docs/CORTES.md:340`; `app/core/contratos_estruturais.py:269`.
- **Extinção por idade dessas três classes:** falta a lei específica; não há conclusão de limiar ou fechamento temporal. Os limiares existentes governam emissão/urgência — `app/ponto/turnos.py:3110`, `:3136`, `:3143`; `CLAUDE.md:427`.

## 05/11 15:46 · sessao 585398b8b414 · - · - · origem=Ronald (tmux)

**O169 ainda existe — certeza alta.** A migração ao evento preservou a decisão por janela:

- `app/ponto/services/cluster_espurio.py:12` define 14 dias; `:21-23` consulta somente células desse intervalo; `:73-74` monta `datas` apenas com assinaturas encontradas.
- `app/ponto/services/cluster_espurio.py:45-50` consulta **todos** os chamados vivos do módulo, sem recorte de data, e envia `False` ao catálogo quando o dia não está em `datas`. Portanto, um dia anterior à janela é tratado como premissa morta sem consultar sua célula.
- `app/chamados/catalogo/premissa.py:52` liga cluster ao fato recebido; `:127-134` transforma `False` em premissa morta; `app/ponto/services/cluster_espurio.py:95-99` executa `ch.reconciliar()`.

**Dois disparadores escrevem essa decisão — certeza alta:**

1. Evento do cartório: `app/ponto/services/cartorio.py:981-994`, após haver células julgadas, chama `_cluster(c, hoje)` com os 14 dias padrão.
2. Comando manual `detectar_cluster_espurio --apply`: `app/ponto/management/commands/detectar_cluster_espurio.py:29` e `:34-46`, usando `--dias`.

O cron agendado usa `--alarme` (`app/config/crons.py:598`); esse modo não executa o fechamento (`app/ponto/management/commands/detectar_cluster_espurio.py:34-50`). É contraexemplo à alegação de que a varredura agendada ainda escreve.

**A guarda do modelo não elimina o defeito no chamado canônico — certeza alta.** O emissor usa categoria `batida_espuria` (`app/ponto/services/cluster_espurio.py:109`). `e_chamado_de_furo` só reconhece módulo em `CALADOS_POR_ISENCAO` ou categorias/subcategorias dos mapas (`app/chamados/models.py:1194-1200`); cluster não consta no conjunto (`app/chamados/catalogo/modulos.py:235-241`) nem `batida_espuria` nos mapas (`app/chamados/models.py:2955-2972`). Assim, a guarda da premissa retorna `True` antes de consultar o dia (`app/chamados/models.py:690-691`).

Contraexemplo: perguntas vivas ancoradas podem impedir o fechamento (`app/chamados/models.py:393-414`). Sem perguntas vivas, a guarda permite (`:402-404`).

**Dia não medido também vira fato morto nesse coletor — certeza alta.** Sem célula, a consulta não devolve o dia (`app/ponto/services/cluster_espurio.py:23`); com célula sem ata, `ata={}` produz nenhuma órfã/missing e nenhuma assinatura (`:24-35`). Ambos deixam o dia fora de `datas`, convertido em `False` em `:50`. Isso decide “premissa morta do cluster”; não prova uma classificação explícita como “falta” ou “dia encerrado” (`app/chamados/catalogo/premissa.py:127-134`).

Contraexemplos: chamado sem dia forte é ignorado (`app/ponto/services/cluster_espurio.py:47-49`); dia medido dentro da janela com assinatura permanece em `datas` (`:32-35`, `:73-74`).

**Outro caminho por competência existe, mas exige tranca explícita — certeza alta.** O serviço exige período trancado (`app/chamados/services/competencia_trancada.py:82-85`), seleciona vivos daquela competência (`:88-103`) e, sem perguntas vivas, os inclui para encerramento (`:135-146`). Cluster está no catálogo como supervisão (`app/chamados/catalogo/modulos.py:115-116`), enquanto as exceções da tranca são cadastro/atos DP (`:267-272`). O serviço chama o gate (`app/chamados/services/competencia_trancada.py:181-184`), que grava `competencia_trancada` sem consultar a assinatura do cluster (`app/chamados/models.py:1541-1559`). Pode ser disparado pelo ato de trancar (`app/ponto/services/fechamento.py:1404-1412`) ou pelo comando (`app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`).

Contraexemplos: competência meramente passada sem tranca não entra (`app/chamados/services/competencia_trancada.py:82-84`); resgatados e chamados com perguntas vivas ficam (`:135-145`).

**A nota ainda tem objeto — certeza alta.** A nota exige O169 para superar PARCIAL (`app/core/contratos_estruturais.py:269`), e o mecanismo de ausência fora da janela permanece em `app/ponto/services/cluster_espurio.py:21-23`, `:45-50`, `:95-99`. Não cabe indicar commit que o removeu, pois o caminho atual permanece.

## 05/11 15:46 · sessao 2124c9a2aa12 · - · - · origem=Ronald (tmux)

**(b) Sim — certeza alta, no coletor de cluster.** `dias_com_cluster` consulta apenas células da janela; dia sem célula não entra, e célula sem ata recebe `{}`, produzindo zero órfãs/missing e nenhum achado (`app/ponto/services/cluster_espurio.py:21-35`). `pendencias` transforma somente os achados em `datas`, mas compara esse conjunto com **todos** os chamados vivos de cluster: dia ausente vira explicitamente `FATO_CLUSTER_ESPURIO=False` (`app/ponto/services/cluster_espurio.py:45-50`, `:73-82`). O catálogo interpreta `False` como “premissa morta”, enquanto `None` seria “não coletado — conservar” (`app/chamados/catalogo/premissa.py:127-134`).

Esse resultado chega ao fechamento por `ch.reconciliar()` (`app/ponto/services/cluster_espurio.py:95-99`). Para o cluster normal, a guarda de furo não relê o dia: o emissor usa categoria `batida_espuria` (`app/ponto/services/cluster_espurio.py:109`), ausente dos mapas de furo (`app/chamados/models.py:2955-2972`); o módulo também não está em `CALADOS_POR_ISENCAO` (`app/chamados/catalogo/modulos.py:235-241`). Assim, `e_chamado_de_furo()` resulta falso e a guarda autoriza (`app/chamados/models.py:1194-1200`, `:690-691`); sem pergunta viva impeditiva, `reconciliar` grava o novo estado (`app/chamados/models.py:402-404`, `:617-620`).

**Contraexemplos/limites — certeza alta:** pergunta viva sem destino legítimo pode impedir o fechamento (`app/chamados/models.py:405-414`). A leitura genérica de furo já distingue ignorância: sem célula ou sem `lampadas`, retorna `None` (`app/chamados/juizes.py:457-462`, `:471-476`); o defeito está na transformação feita pelo coletor de cluster, antes do catálogo (`app/ponto/services/cluster_espurio.py:50`).

**Alcance:** essa prova mostra classificação indevida como premissa morta; não prova que o caminho também grave rótulo “falta” ou “dia encerrado”. [não verificado]

## 05/11 15:45 · sessao 3180af4789fa · - · - · origem=Ronald (tmux)

**Certeza alta:** `CSF_RETRATADO` é contador de execução, incrementado após `retratar=True`; não é estado ou motivo persistido (`app/ponto/management/commands/supra_juiz.py:279-288`, `app/chamados/models.py:22-30`). Sem stdout/logs, sua soma histórica exata é **[não verificado]**.

**Certeza alta:** `estado_chamado` persiste status anterior, posterior e via, sem caminho da pilha ou resultado da guarda (`app/chamados/models.py:275-278`). O ator identifica comando ou request; não distingue um receptor chamado dentro de outro comando (`app/core/observ.py:22-32`, `app/core/observ.py:108-123`). Portanto `ator=sistema:supra_juiz` é evidência de atribuição, não uma contagem autônoma do placar.

**Certeza alta:** a guarda real só consulta perguntas respondidas em duas relações; mensagem livre não participa (`app/ponto/supra_juiz.py:539-550`). Mensagens têm autor e timestamp próprios (`app/chamados/models.py:1580-1593`). Contraexemplo concreto de classe: chamado com mensagem livre do colaborador e nenhuma pergunta respondida recebe `True` da guarda, embora tenha conversa (`app/ponto/supra_juiz.py:543-550`, `app/chamados/models.py:1588-1593`).

**Certeza alta:** guarda hoje não reconstrói guarda no fechamento: reabertura pode apagar `respondida_em` (`app/chamados/models.py:1493`), enquanto reabertura do chamado limpa `resolvido_em` e via (`app/chamados/models.py:241-243`). A sonda usa eventos auditados, preservando ciclos de fechamento e separando mensagens anteriores ao evento da guarda atual (`app/chamados/models.py:275-278`).

O backend tenant é PostgreSQL (`app/config/settings/saas.py:83-95`); há precedente de `schema_context` com padrão juliani (`app/colaboradores/management/commands/backfill_municipio_posto.py:37-42`).

===SONDA===
```python
# Executar como Python em manage.py shell, com settings saas.
# Somente SELECT; a transacao PostgreSQL recusa escrita.
# Backend tenant: app/config/settings/saas.py:83-95.
import json
from collections import Counter, defaultdict
from datetime import timedelta

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context
from chamados.models import ChamadoColaborador
from chamados.catalogo.motor import COBRANCA, TERMINAIS
from ponto.supra_juiz import pode_executar_csf
from core.models import LogAuditoria

# Estados reais: app/chamados/catalogo/motor.py:24-31.
# Trilha real: app/chamados/models.py:275-278.
# Campos auditados: app/core/models.py:285-307.
# Guarda real, somente leitura: app/ponto/supra_juiz.py:539-550.
with schema_context("juliani"), transaction.atomic():
    with connection.cursor() as cursor:
        cursor.execute(
            "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ READ ONLY"
        )

    agora = timezone.now()
    desde = agora - timedelta(days=30)
    eventos = list(
        LogAuditoria.objects.filter(
            modelo__in=("Chamado", "ChamadoColaborador"),
            acao="estado_chamado",
            criado_em__gte=desde,
            criado_em__lt=agora,
            valor_antes__status_local__in=COBRANCA,
            valor_depois__status_local__in=TERMINAIS,
        ).order_by("criado_em", "pk")
    )
    ids = {e.objeto_id for e in eventos if e.objeto_id is not None}
    chamados = ChamadoColaborador.objects.in_bulk(ids)

    # Avaliacao ATUAL, sem inferencia retrospectiva.
    guarda_agora = {
        pk: not pode_executar_csf(ch)
        for pk, ch in chamados.items()
    }
    grupos = defaultdict(Counter)
    distintos = defaultdict(set)
    com_mensagem_previa = set()
    linhas = []

    for e in eventos:
        ch = chamados.get(e.objeto_id)
        via = (e.valor_depois or {}).get("via", "")
        ator = e.ator or "[request/ou ator ausente]"
        chave = (ator, via)
        grupos[chave]["eventos"] += 1
        distintos[chave].add(e.objeto_id)

        if ch is None:
            grupos[chave]["objeto_ausente"] += 1
            continue

        # Fato persistido, nao duplicacao da logica da guarda.
        # Followup: app/chamados/models.py:1587-1593.
        mensagem_antes = ch.followups.filter(
            autor_tipo="colaborador",
            criado_em__lte=e.criado_em,
        ).exists()
        grupos[chave]["eventos_com_mensagem_previa"] += int(mensagem_antes)
        grupos[chave]["eventos_de_chamado_bloqueado_pela_guarda_agora"] += int(
            guarda_agora[ch.pk]
        )
        if mensagem_antes:
            com_mensagem_previa.add(ch.pk)

        linhas.append({
            "evento": e.pk,
            "chamado": ch.pk,
            "em": e.criado_em.isoformat(),
            "ator": ator,
            "via": via,
            "mensagem_colab_antes_do_evento": mensagem_antes,
            "guarda_real_bloqueia_agora": guarda_agora[ch.pk],
            "passou_pela_guarda_no_evento": None,
        })

    resultado = {
        "tenant": "juliani",
        "desde_inclusivo": desde.isoformat(),
        "ate_exclusivo": agora.isoformat(),
        "CSF_RETRATADO_30d": None,
        "CSF_RETRATADO_limitacao":
            "[nao verificado] Contador de stdout; nao e estado/motivo persistido "
            "(app/ponto/management/commands/supra_juiz.py:279-288).",
        "fechados_com_conversa_por_caminho_sem_guarda_30d": None,
        "caminho_limitacao":
            "[nao verificado] Trilha registra status/via, nao passagem na guarda; "
            "guarda atual nao reconstrói historico "
            "(app/chamados/models.py:275-278;1493).",
        "eventos_auditados_cobranca_para_terminal": len(eventos),
        "chamados_distintos_nesses_eventos": len(ids),
        "objetos_ausentes": sorted(ids - chamados.keys()),
        "chamados_com_mensagem_anterior_a_algum_fechamento_auditado":
            len(com_mensagem_previa),
        "por_atribuicao_auditada": [
            {
                "ator": ator,
                "via": via,
                **dict(contagens),
                "chamados_distintos": len(distintos[(ator, via)]),
            }
            for (ator, via), contagens in sorted(grupos.items())
        ],
        "evidencias": linhas,
    }
    print(json.dumps(resultado, ensure_ascii=False, indent=2))
```
===FIM===

_1707 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
