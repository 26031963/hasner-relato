# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 18:42** · publicado em **14/11/2026 18:42** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 18:42 · sessao 988e591b1cf0 · O50-DESLIGAR-ENCERRA-TUDO · verificador · origem=Code

**23 CONFERE, 2 DIVERGE, 0 SUMIU**, conforme as linhas abaixo. **F05 e F06 caem como provas do caminho RET→TXT/vigia**: a saída por rescisão ocorre antes da retenção. Os campos crus continuam presentes. (`app/folha/export.py:866`, `app/folha/export.py:871`, `app/folha/export.py:968`, `app/folha/export.py:1241`, `app/folha/export.py:1269`)

F01 aparece uma vez na tabela, pois foi repetido na amostra fornecida. A identidade Git do HEAD informado é **[não verificado]**.

Nos contraexemplos, **S** significa situação atual discordante de vínculo válido; **A**, EC inativa com vigência cobrindo o dia ou ativa com início futuro; **B**, igualdade entre dia, demissão e fim do vínculo. A autoridade aceita a igualdade e vínculos inativos datados, sem consultar situação ou demissão do colaborador. São possibilidades de código; ocorrência em produção é **[não verificado]**. (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:282`, `app/escala/alimentacao.py:285`)

| ID | Arquivo:linha na cópia | Cru lido? | Predicado confere? | Alcance folha/fio e caminho comprovado | CE procede? | Veredito |
|---|---|---|---|---|---|---|
| **F01** | `app/folha/export.py:60` | **Sim**, D | **Sim:** D não nula e D≤fim. | **Folha sim.** `gerar_dominio_txt` (`app/folha/views.py:212`) → `classificar_export` (`app/folha/export.py:866`) → `sai_pela_rescisao` (`app/folha/export.py:60`). Retorna `fora/rescisao` e continua (`app/folha/export.py:867`, `app/folha/export.py:871`); a montagem recebe esses itens (`app/folha/views.py:268`) e exclui quem não está em `entra` (`app/folha/export.py:716`, `app/folha/export.py:726`). | **Sim**, como divergência entre rota financeira e vínculo válido, conforme autoridade acima. | **CONFERE** |
| **F02** | `app/folha/export.py:807`; `app/folha/export.py:810` | **Sim**, A+S | **Sim:** A=True, 12x36, âncora nula, S=ativo, participante dos fechamentos (`app/folha/export.py:808`, `app/folha/export.py:809`, `app/folha/export.py:811`). | **Folha sim.** `aprovar_fechamento` → `classificar_export` (`app/ponto/views.py:1299`) → seleção crua (`app/folha/export.py:806`). Produz `sem_ancora_12x36` e `travas_aprovacao` (`app/folha/export.py:835`, `app/folha/export.py:848`); o consumidor lê a trava, exclui os PKs e aprova somente os restantes (`app/ponto/views.py:1301`, `app/ponto/views.py:1311`, `app/ponto/views.py:1315`, `app/ponto/views.py:1365`). | **Sim**, S/A, conforme autoridade acima. | **CONFERE** |
| **F05** | `app/folha/export.py:1241`, agora em `_dias_sem_decisao` | **Sim**, D | **Sim localmente:** corta fim até D inclusive; janela colapsada retorna vazio (`app/folha/export.py:1242`, `app/folha/export.py:1243`, `app/folha/export.py:1246`). **Há guarda anterior nova:** desligado não chega ao helper pelo retentor (`app/folha/export.py:1207`, `app/folha/export.py:1212`). | **Não alcança TXT/vigia pelo caminho alegado.** Corte efetivo exige D<fim; F01 já dá `continue` antes do retentor (`app/folha/export.py:866`, `app/folha/export.py:871`, `app/folha/export.py:968`). `dias_que_seguram` chama `_dias_que_retem` somente para classificados `furo_espelho` (`app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`). Persiste diagnóstico: `recalcular_fechamento_mes` → `motivos_retencao_espelho` (`app/ponto/services/fechamento.py:1067`) → `motivos_retencao_celula` (`app/folha/export.py:1307`) → helper (`app/folha/export.py:1212`), gravado em `motivos_espelho` (`app/ponto/services/fechamento.py:1186`). | **Sim localmente**, para S≠desligado; **não demonstra a trava alegada**. Para desligado, o helper serve ao informativo (`app/folha/export.py:1226`, `app/folha/export.py:1228`). | **DIVERGE — alcance RET/TXT/vigia e guarda anterior.** |
| **F06** | `app/folha/export.py:1269`; `app/folha/export.py:1271` | **Sim**, D | **Sim localmente:** célula ausente só gera `sem_celula` com d<hoje e d≠D; também há exceção para célula sem veredito (`app/folha/export.py:1272`, `app/folha/export.py:1295`). Mesma guarda anterior de desligado (`app/folha/export.py:1207`). | **Não alcança TXT/vigia pelo caminho alegado.** A igualdade precisa ocorrer no laço d≤fim (`app/folha/export.py:1265`, `app/folha/export.py:1269`), portanto F01 já excluiu antes da retenção (`app/folha/export.py:866`, `app/folha/export.py:871`, `app/folha/export.py:968`). Restam diagnóstico de fechamento e leitura individual: `o_que_segura` → `_dias_que_retem` → retentor (`app/folha/fora_da_folha.py:224`, `app/folha/fora_da_folha.py:98`), seguida de leitura do fio (`app/folha/fora_da_folha.py:228`). | **Sim localmente**, B com S≠desligado; **não como prova de trava TXT/vigia**, pelo bloqueio anterior citado. | **DIVERGE — alcance RET/TXT/vigia e guarda anterior.** |
| **P34** | `app/ponto/services/fechamento.py:432`; `app/ponto/services/fechamento.py:438`; `app/ponto/services/fechamento.py:440` | **Sim**, S+D | **Sim:** S≠desligado OU desligado com D≥início da janela da empresa; sem teto superior (`app/ponto/services/fechamento.py:441`). | **Folha sim.** `recalcular_fechamento_mes` → `universo_da_competencia` (`app/ponto/services/fechamento.py:554`) → queryset retornado (`app/ponto/services/fechamento.py:445`) → laço (`app/ponto/services/fechamento.py:619`) → gravação do fechamento (`app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1160`). | **Sim:** desligado com D nula fica fora mesmo com EC válida, pelo predicado citado e autoridade acima. | **CONFERE** |
| **C35** | `app/escala/services/leitor_celula.py:365`; `app/escala/services/leitor_celula.py:389` | **Sim**, D | **Sim:** d>D OU d=D sem fato; também ramo anterior à admissão. Fato = minutos realizados OU lâmpadas não missing (`app/escala/services/leitor_celula.py:385`, `app/escala/services/leitor_celula.py:386`). | **Folha e fio sim.** Folha: `recalcular_fechamento_mes` → `grade_do_fechamento` (`app/ponto/services/fechamento.py:1014`) → `grade_da_celula` (`app/folha/export.py:338`) → `fora_vinculo` (`app/escala/services/leitor_celula.py:393`), excluído dos dias internos e trabalho/ausência (`app/ponto/services/fechamento.py:1043`, `app/ponto/services/fechamento.py:1046`), alterando previsto (`app/ponto/services/fechamento.py:1083`). Fio: `gerar_perguntas_disputa` → `_data_furo_da_grade` (`app/chamados/services/disputa_emissao.py:350`) → grade (`app/chamados/services/disputa_emissao.py:83`); fora de trabalho retorna None (`app/chamados/services/disputa_emissao.py:85`, `app/chamados/services/disputa_emissao.py:87`), podendo causar `continue` antes de abrir pergunta (`app/chamados/services/disputa_emissao.py:366`, `app/chamados/services/disputa_emissao.py:444`). | **Sim**, B, conforme autoridade inclusiva acima. | **CONFERE** |
| **P39** | `app/ponto/services/he_pendente_lavrado.py:92` | **Sim**, S | **Sim:** universo da empresa com S=ativo; produz retrato (`app/ponto/services/he_pendente_lavrado.py:160`, `app/ponto/services/he_pendente_lavrado.py:169`). | **Folha sim, condicional.** `lavrar_he_pendente.handle` injeta revogador e chama `apurar` (`app/ponto/management/commands/lavrar_he_pendente.py:56`, `app/ponto/management/commands/lavrar_he_pendente.py:62`) → callback por dia (`app/ponto/services/he_pendente_lavrado.py:143`) → `revogar_recusa_do_sistema` → marcação revogada (`app/ponto/portas/he.py:472`, `app/ponto/portas/he.py:375`) → decisões vigentes em `estado_por_dia` (`app/ponto/services/gestao_he.py:86`) → `medir` (`app/folha/porta_export.py:684`, `app/folha/porta_export.py:686`) → trava condicional e recusa (`app/folha/porta_export.py:230`, `app/folha/porta_export.py:853`). A recusa automática de ponta pequena estável, isoladamente, não comprova trava: ela rejeita acima do limite, faixa que o portão verifica (`app/ponto/portas/he.py:313`, `app/folha/porta_export.py:230`). | **Sim**, S pode omitir dia e impedir revogação pelo callback citado. | **CONFERE — alcance condicional.** |
| **P44** | `app/ponto/services/vigia_de_hora.py:117` | **Sim**, S | **Sim:** pula desligado; também pula isento (`app/ponto/services/vigia_de_hora.py:119`). | **Fio sim.** `rodar` → `julgar_celula` (`app/ponto/services/vigia_de_hora.py:131`) → `julgar_colab` (`app/ponto/services/cartorio.py:2040`) → emissão sob aplicação e demais guardas (`app/ponto/services/cartorio.py:1791`, `app/ponto/services/cartorio.py:1829`) → `ChamadoColaborador.abrir` (`app/ponto/management/commands/detectar_ausencias.py:694`). | **Sim**, S; inclui candidatos de hoje/ontem (`app/ponto/services/vigia_de_hora.py:114`, `app/ponto/services/vigia_de_hora.py:115`). | **CONFERE** |
| **P66** | `app/chamados/services/pergunta_no_app.py:186` | **Sim**, S | **Sim:** chamados em cobrança, módulos selecionados e S=ativo (`app/chamados/services/pergunta_no_app.py:185`). | **Fio sim, sob apply.** `Command.handle` → `medir` (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:21`) → achados (`app/chamados/services/pergunta_no_app.py:201`) → `perguntar` (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29`) → `criar_disputa_a_partir_de_chamados_orfaos` (`app/chamados/services/pergunta_no_app.py:246`) → `_complementar_marcos_faltantes` (`app/chamados/services/disputa_emissao.py:1175`) → `_criar_pergunta_se_couber` (`app/chamados/services/disputa_emissao.py:881`) → `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:837`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |
| **P65** | `app/chamados/services/limbo_folgas.py:65` | **Sim**, A | **Sim:** conta vínculos A=True do template. | **Não alcança decisão de folha/fio.** `aplicar_sugestao` → `texto_seco` → `reescrever_texto` (`app/chamados/services/limbo_folgas.py:70`); a contagem forma apenas sufixo textual (`app/chamados/services/limbo_folgas.py:51`). Aceitação usa outros dados (`app/chamados/services/limbo_folgas.py:82`, `app/chamados/services/limbo_folgas.py:85`). | **Sim**, A para o universo contado; efeito é textual, como citado. | **CONFERE** |
| **P22** | `app/ponto/selecao_periodo.py:278` | **Sim**, A | **Sim:** jornada de EC ativa; fallback 24h (`app/ponto/selecao_periodo.py:287`). | **Não alcança no fluxo indicado.** `turnos_em_turno_stored` retorna mapa (`app/ponto/selecao_periodo.py:293`) → `colabs_em_turno_stored` (`app/ponto/selecao_periodo.py:253`) → dashboard usa tamanho (`app/core/views.py:1160`, `app/core/views.py:1161`). | **Sim**, A, conforme autoridade acima. | **CONFERE** |
| **C64** | `app/ferias/management/commands/alertar_retornos.py:30` | **Sim**, S | **Sim:** S=ativo, hoje folga e amanhã trabalha (`app/ferias/management/commands/alertar_retornos.py:34`). | **Não alcança folha/fio decisório.** `handle` monta retornos (`app/ferias/management/commands/alertar_retornos.py:35`) → `push_dp`, aviso de disponibilidade (`app/ferias/management/commands/alertar_retornos.py:45`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |
| **T13** | `app/colaboradores/views.py:619` | **Sim**, S | **Sim:** S=ativo, em outro posto. | **Não alcança.** `buscar_colabs_posto` termina em JSON limitado a 30 (`app/colaboradores/views.py:622`, `app/colaboradores/views.py:626`); o POST resolve o colaborador separadamente (`app/colaboradores/views.py:640`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |
| **T26** | `app/colaboradores/views.py:2900` | **Sim**, S | **Sim:** após juiz de férias, exclui S=desligado. | **Não alcança.** `disponibilidade` acrescenta em `parados` (`app/colaboradores/views.py:2905`) e renderiza painel (`app/colaboradores/views.py:2933`). | **Sim**, S, havendo férias cobrindo hoje no fluxo citado. | **CONFERE** |
| **T39** | `app/colaboradores/views.py:48` | **Sim**, A | **Sim:** prefetch somente A=True. | **Não alcança.** `vincular_colaboradores` usa o prefetch para `sem_escala` (`app/colaboradores/views.py:71`, `app/colaboradores/views.py:73`), filtros (`app/colaboradores/views.py:97`) e render (`app/colaboradores/views.py:126`). | **Sim**, A, conforme autoridade acima. | **CONFERE** |
| **T52** | `app/colaboradores/views_fase.py:45` | **Sim**, A | **Sim:** última A=True por início. | **Não alcança.** `fase_12x36` usa template/âncora (`app/colaboradores/views_fase.py:47`, `app/colaboradores/views_fase.py:49`), passa EC aos previews (`app/colaboradores/views_fase.py:98`) e renderiza (`app/colaboradores/views_fase.py:115`). | **Sim**, A, conforme autoridade acima. | **CONFERE** |
| **P51** | `app/ponto/worklist.py:49` | **Sim**, A | **Sim:** primeira A=True fornece posto. | **Não alcança.** `coletar_gaps_dp` inclui posto no resultado (`app/ponto/worklist.py:50`, `app/ponto/worklist.py:66`) → `painel_inconsistencias_posto` agrupa e renderiza (`app/colaboradores/views.py:3025`, `app/colaboradores/views.py:3029`). | **Sim**, A para agrupamento, conforme autoridade acima. | **CONFERE** |
| **C11** | `app/escala/views_plano_folgas.py:176` | **Sim**, A | **Sim:** exige A=True; ausência gera erro `vinculo` e pula item (`app/escala/views_plano_folgas.py:178`, `app/escala/views_plano_folgas.py:179`). | **Folha e fio sim.** `plano_folgas_aplicar` → `aplicar_folgas_em_lote` (`app/escala/views_plano_folgas.py:185`) → `relavrar_celulas_da_foto` (`app/escala/folgas.py:418`) → `regenerar_celulas_vinculo` (`app/escala/folgas.py:293`) → `_o_chao_do_dia_mudou` (`app/ponto/portas/celula.py:392`) → julgamento (`app/ponto/portas/celula.py:472`), recálculo (`app/ponto/portas/celula.py:496`) e reavaliação/rejunção (`app/ponto/portas/celula.py:536`, `app/ponto/portas/celula.py:539`). | **Sim**, A, para vínculo inativo cobrindo dia editável. | **CONFERE** |
| **C26** | `app/escala/services/cadastro_realidade.py:575` | **Sim**, S | **Sim:** exclui S=desligado. | **Não alcança decisão de folha/fio por esse corte.** `_afastado_sem_registro` produz C4 sem data (`app/escala/services/cadastro_realidade.py:600`) → `lista` (`app/escala/services/cadastro_realidade.py:794`). Existe consumo estrutural pelo vigia: `ListaDoAdmin._carregar` (`app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:218`) → `colab_na_lista` (`app/chamados/services/vigia_fio.py:251`) → causa d/e (`app/chamados/services/vigia_fio.py:682`). Porém desligados já ficam sem dias nesse universo pelo retentor (`app/chamados/services/vigia_fio.py:586`, `app/folha/export.py:1207`); C4 sem data também não entra no mapa de dias (`app/escala/services/cadastro_realidade.py:1033`). | **Sim**, S quanto à lista C4; não comprova alteração da trava/vigia, pelos bloqueios citados. | **CONFERE** |
| **C40** | `app/ponto/management/commands/reconciliar_grade.py:106` | **Sim**, S | **Sim:** S=ativo e universo de batidas/chamados/operação (`app/ponto/management/commands/reconciliar_grade.py:84`, `app/ponto/management/commands/reconciliar_grade.py:95`, `app/ponto/management/commands/reconciliar_grade.py:105`). | **Fio sim sob apply.** `handle` → `_emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:250`, `app/ponto/management/commands/reconciliar_grade.py:254`) → `ChamadoColaborador.abrir` ou `reabrir` (`app/ponto/management/commands/reconciliar_grade.py:507`, `app/ponto/management/commands/reconciliar_grade.py:530`); outro ramo chama `retratar` (`app/ponto/management/commands/reconciliar_grade.py:309`). Cron está sem apply (`app/config/crons.py:462`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |
| **C53** | `app/ponto/management/commands/reconciliar_fantasmas.py:174` | **Sim**, A | **Sim:** primeira A=True fornece diagnóstico. | **Não decide a transição.** EC → `_completude` → `detalhe` (`app/ponto/management/commands/reconciliar_fantasmas.py:205`, `app/ponto/management/commands/reconciliar_fantasmas.py:206`). Decisão usa núcleo e premissa separadamente (`app/ponto/management/commands/reconciliar_fantasmas.py:248`, `app/ponto/management/commands/reconciliar_fantasmas.py:256`); detalhe entra apenas no motivo passado à retratação (`app/ponto/management/commands/reconciliar_fantasmas.py:278`, `app/ponto/management/commands/reconciliar_fantasmas.py:280`). | **Sim**, A no diagnóstico, conforme autoridade acima. | **CONFERE** |
| **C70** | `app/chamados/management/commands/auditar_invariantes_chamados.py:1476` | **Sim**, S | **Sim:** S≠ativo é pulado em A4. | **Não alcança emissão/reabertura.** O corpo alimenta resultado A4 (`app/chamados/management/commands/auditar_invariantes_chamados.py:1485`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1505`), stdout e HTML opcional (`app/chamados/management/commands/auditar_invariantes_chamados.py:1841`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1853`). | **Sim**, S no universo auditado, conforme autoridade acima. | **CONFERE** |
| **A24** | `app/relatorios/furos.py:117` | **Sim**, S | **Sim:** S∈{ativo,afastado,ferias}. | **Não alcança.** `dataset_furos` monta e retorna cards (`app/relatorios/furos.py:142`, `app/relatorios/furos.py:167`) → view (`app/relatorios/views.py:1541`) → PDF ou render (`app/relatorios/views.py:1562`, `app/relatorios/views.py:1566`). | **Sim**, S em consulta histórica, conforme autoridade acima. | **CONFERE** |
| **A37** | `app/inteligencia/calculadores.py:139` | **Sim**, S | **Sim:** S∈{ativo,afastado,ferias}. | **Não alcança.** `preenchimento_marcos` calcula e retorna linhas (`app/inteligencia/calculadores.py:162`, `app/inteligencia/calculadores.py:177`) → `metricas.calcular` (`app/inteligencia/metricas.py:42`) → grava `MetricaSnapshot` (`app/inteligencia/metricas.py:47`). | **Sim**, S em janela histórica, conforme autoridade acima. | **CONFERE** |
| **A50** | `app/inteligencia/views.py:209` | **Sim**, S | **Sim:** S∈{ativo,ferias,afastado}, com empresa opcional. | **Não alcança.** A base de `api_kpi` alimenta ramos de JSON: ativos, sem escala, sem ausência, aniversariantes e disponíveis (`app/inteligencia/views.py:222`, `app/inteligencia/views.py:228`, `app/inteligencia/views.py:235`, `app/inteligencia/views.py:263`, `app/inteligencia/views.py:295`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |

**Consolidação das 25 linhas: 23 conferem, 2 divergem, 0 sumiram.** Dos seis “folha diretamente”, **F05 e F06 caem na fundamentação RET→TXT/vigia**, enquanto **F01, F02, P34 e C35 mantêm os caminhos diretos documentados acima**. A consequência exata sobre antecipação/prazo depende do aval não fornecido: **[não verificado]**.

## 14/11 18:42 · sessao 89d24e9eeb22 · GRADE-RESOLVE · censo · origem=Code

**O PASSO 0 fica incompleto:** há **18 casos individualizados com dono ESTRUTURA escrito**, mas faltam os IDs de 63 unidades da ingestão e o rol autônomo das atas do Fernando. Portanto, **18 é o subtotal documental recuperável; o total integral para o cruzamento é [nao verificado]**. (app/docs/RELATO.md:8390, :8398; app/docs/BACKLOG.md:1102; app/docs/RELATO.md:52)

**(A) Fontes, unidades e contagens**

| Fonte | Onde mora o registro | O que contei como um caso | Contagem |
|---|---|---|---:|
| GRADE-MESTRA — SEGUE | app/docs/GRADE-MESTRA.md:33 até :522 | Uma linha de dados das sete tabelas por balde; excluí cabeçalhos e separadores | **456 obras** |
| GRADE-MESTRA — complementos | app/docs/GRADE-MESTRA.md:525 até :1551 | Uma linha de dados das tabelas `complemento`; excluí cabeçalhos e separadores | **548 complementos**, sob **119 mães** |
| Linhas HJ- do BACKLOG | app/docs/BACKLOG.md:734, :744–749, :784, :786, :1868, :1917, :2547–2555 | Um identificador-raiz HJ-NNN representado nas linhas; derivados agrupados na raiz | **11 raízes**; quantidade de casos pessoa+dia: **[nao verificado]** |
| Fernando — ingestão localizada | app/docs/BACKLOG.md:1868; app/docs/RELATO.md:8368, :8386 | Uma unidade pessoa+dia HJ-NNN-Uk | **80 unidades declaradas; 17 IDs recuperáveis** |
| Atas do Fernando — coleção autônoma | **NAO ACHADA**; há remissão a ATA-1111 em app/docs/PROMPTS.md:2125 | Rol original necessário para estabelecer a unidade | **[nao verificado]** |
| BO-0611 | app/docs/BACKLOG.md:1911–1912; app/docs/RELATO.md:1471, :1533–1534 | Um item numerado 01–14; 11 e 12 permanecem separados | **14** |
| BO-1311-ROTEIRO | Nome encontrado: BO-[nome]-1311-ROTEIRO, app/docs/BACKLOG.md:783; app/docs/RELATO.md:222, :226 | Uma ficha 01–10; perguntas vivas são subunidades | **10** |
| BO2910-ESTRUTURA | app/docs/BACKLOG.md:1102 | Uma das classes explicitamente enumeradas: 01, 02, 05, 06, 08 | **5** |
| BO-ADMIN-2710 | app/docs/CORTES.md:330, :332; app/docs/RELATO.md:3421–3429 | Um item 1–8, mais os complementos 8.1, 8.2 e 8.3 | **11**, confirmado em app/docs/RELATO.md:52 |

A recontagem da grade deu **75+5+155+177+39+1+4 = 456**. Os **548 complementos** foram contados diretamente nas linhas de dados; a soma dos números dos 119 subtítulos também deu 548. Os dois números do aval conferem. (app/docs/GRADE-MESTRA.md:15–22, :525–1551)

Nas linhas HJ-, encontrei **20 linhas: dez IDs HJ-NNN diretos, nove derivados e um instrumento de ingestão**. Os derivados representam HJ-091, HJ-075 e HJ-058; o TSV agrupa-os em suas raízes para evitar contar achados de opus e complementos como novos BOs originais. A granularidade original desses casos permanece **[nao verificado]**. (app/docs/BACKLOG.md:734, :744–749, :784, :786, :1868, :1917, :2547–2555)

O mapa integral da ingestão é remetido a `logs/hj_0511/mapa.md`, **NAO ACHADO nesta cópia**. A publicação permite recuperar 17 IDs dos 80; os outros **63 IDs ficam [nao verificado]**. O vínculo entre essa ingestão e a totalidade das “atas do Fernando” também é **[nao verificado]**. (app/docs/RELATO.md:8386, :8390–8398)

No BO-0611, o aval V2 **substitui o quadro anterior**. Por isso, BATIDA para 01/04 e CADASTRO para 06, escritos no quadro antigo, não foram transportados como donos vigentes; a V2 escreve CADASTRO para 11/12. (app/docs/PROMPTS.md:1925; app/docs/RELATO.md:1533, :1471)

As buscas abrangeram a árvore inteira, incluindo arquivos ocultos/ignorados e os documentos RELATO-ARQUIVO, LAPIDES, DOSSIES, CORTES e TICKETS. Os comandos executados seguem abaixo; a execução e a ausência física dos arquivos não possuem referência arquivo:linha. **[nao verificado]**

```sh
rg -n --hidden --no-ignore -i 'BO[-_]?0611|BO[-_]?1311[-_]?ROTEIRO|BO2910[-_]?ESTRUTURA|BO[-_]?ADMIN[-_]?2710' .
rg -n --hidden --no-ignore -i 'bo[-_]mauricio[-_]0611|bo[-_]mauricio[-_]1311|bo2910|bo2710' .
rg -n --hidden --no-ignore -i 'atas.{0,60}fernando|fernando.{0,60}atas|ata.{0,40}ingerid|ingerid.{0,40}ata' .
rg -n --hidden --no-ignore -i 'fernando|ingestao|ingestão|ingeridas|ingeridos|ATA-1111' .
rg -n --hidden --no-ignore 'HJ-' .
rg -n --hidden --no-ignore 'HJ-[0-9]{3}-U[0-9]+' .
rg --files --hidden --no-ignore | rg -i '0611|1311|2910|2710|fernando|ata|relato|lapides|dossies|cortes|tickets'
git grep -n -I -E 'BO[-_]?0611|BO[-_]?1311[-_]?ROTEIRO|BO2910-ESTRUTURA|BO-ADMIN-2710|Fernando|HJ-'
```

O `git grep` falhou porque a cópia não contém metadados Git; a identidade HEAD informada no pedido não pôde ser conferida. As buscas equivalentes usaram `rg`. **[nao verificado]**

Os testes encontrados com BO-0611 no nome documentam o caso 05 e cenários sintéticos; não fornecem o rol original dos 14 itens. (app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:1, :13; app/ponto/tests/test_bo0611_05_vinculo_geo_parcial_django.py:9)

**(B) TSV recuperável — 68 linhas**

Os donos da ingestão abaixo preservam a classificação escrita em **05/11**. A publicação posterior registra cinco mudanças de estado, porém marca a própria remedida como **[nao verificado]**; não fornece novo dono. Assim, os seis ESTRUTURA históricos não comprovam seis divergências atuais. (app/docs/RELATO.md:8390–8394, :52)

```tsv
fonte	id_do_caso	arquivo:linha	dono_L099	prova_do_dono
BACKLOG-HJ	HJ-058	app/docs/BACKLOG.md:2554	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-066	app/docs/BACKLOG.md:2547	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-068	app/docs/BACKLOG.md:2548	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-071	app/docs/BACKLOG.md:2549	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-073	app/docs/BACKLOG.md:2550	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-074	app/docs/BACKLOG.md:2551	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-075	app/docs/BACKLOG.md:2552	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-090	app/docs/BACKLOG.md:1917	SEM_DONO	[nao verificado] dono único fechado; linha pede classificação dos dias
BACKLOG-HJ	HJ-091	app/docs/BACKLOG.md:1917	SEM_DONO	[nao verificado] dono único fechado; derivados em BACKLOG:734 e :744-749
BACKLOG-HJ	HJ-092	app/docs/BACKLOG.md:784	BATIDA	app/docs/BACKLOG.md:784: FECHADA como BATIDA; confirmado em app/docs/RELATO.md:271
BACKLOG-HJ	HJ-093	app/docs/BACKLOG.md:786	SEM_DONO	[nao verificado] dono único fechado para o caso composto
HJ-INGESTAO-0511	HJ-001-U1	app/docs/RELATO.md:8391	CADASTRO	app/docs/RELATO.md:8391: divergente com dono CADASTRO
HJ-INGESTAO-0511	HJ-001-U2	app/docs/RELATO.md:8391	CADASTRO	app/docs/RELATO.md:8391: divergente com dono CADASTRO
HJ-INGESTAO-0511	HJ-002-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392: divergente com dono BATIDA; mudança de estado ressalvada em :52
HJ-INGESTAO-0511	HJ-003-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392: divergente com dono BATIDA
HJ-INGESTAO-0511	HJ-005-U1	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390: divergente com dono ESTRUTURA; mudança de estado ressalvada em :52
HJ-INGESTAO-0511	HJ-005-U2	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390: divergente com dono ESTRUTURA; mudança de estado ressalvada em :52
HJ-INGESTAO-0511	HJ-005-U4	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390: divergente com dono ESTRUTURA; mudança de estado ressalvada em :52
HJ-INGESTAO-0511	HJ-005-U5	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392: divergente com dono BATIDA; mudança de estado ressalvada em :52
HJ-INGESTAO-0511	HJ-012-U1	app/docs/RELATO.md:8394	SEM_DONO	app/docs/RELATO.md:8394: indeterminado; dono [nao verificado]
HJ-INGESTAO-0511	HJ-012-U2	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390: divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-015-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392: divergente com dono BATIDA
HJ-INGESTAO-0511	HJ-016-U1	app/docs/RELATO.md:8391	CADASTRO	app/docs/RELATO.md:8391: divergente com dono CADASTRO
HJ-INGESTAO-0511	HJ-017-U1	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390: divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-027-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392: divergente com dono BATIDA
HJ-INGESTAO-0511	HJ-030-U1	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390: divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-031-U1	app/docs/RELATO.md:8394	SEM_DONO	app/docs/RELATO.md:8394: indeterminado; dono [nao verificado]
HJ-INGESTAO-0511	HJ-050-U2	app/docs/RELATO.md:8394	SEM_DONO	app/docs/RELATO.md:8394: indeterminado; dono [nao verificado]
BO-0611	01	app/docs/RELATO.md:1533	SEM_DONO	[nao verificado] dono vigente; quadro substituído por app/docs/PROMPTS.md:1925
BO-0611	02	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] dono fechado; queixa não identificada
BO-0611	03	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono fechado
BO-0611	04	app/docs/RELATO.md:1533	SEM_DONO	[nao verificado] dono vigente; quadro substituído por app/docs/PROMPTS.md:1925
BO-0611	05	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono fechado
BO-0611	06	app/docs/RELATO.md:1533	SEM_DONO	[nao verificado] dono vigente; quadro substituído por app/docs/PROMPTS.md:1925
BO-0611	07	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono fechado
BO-0611	08	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] dono fechado; registro diz sem divergência
BO-0611	09	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono fechado
BO-0611	10	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono fechado
BO-0611	11	app/docs/RELATO.md:1471	CADASTRO	app/docs/RELATO.md:1471: 11/12 col921 CADASTRO -> lista do admin
BO-0611	12	app/docs/RELATO.md:1471	CADASTRO	app/docs/RELATO.md:1471: 11/12 col921 CADASTRO -> lista do admin
BO-0611	13	app/docs/RELATO.md:1534	SEM_DONO	[nao verificado] frase vigente de dono; item preservado em app/docs/BACKLOG.md:2025
BO-0611	14	app/docs/RELATO.md:1534	SEM_DONO	[nao verificado] frase vigente de dono; item preservado em app/docs/BACKLOG.md:2025
BO-1311-ROTEIRO	01	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	02	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	03	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	04	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	05	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	06	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	07	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	08	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	09	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	10	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO2910-ESTRUTURA	01	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA, cada uma como linha, item 01
BO2910-ESTRUTURA	02	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA, cada uma como linha, item 02
BO2910-ESTRUTURA	05	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA, cada uma como linha, item 05
BO2910-ESTRUTURA	06	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA, cada uma como linha, item 06
BO2910-ESTRUTURA	08	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA, cada uma como linha, item 08
BO-ADMIN-2710	1	app/docs/RELATO.md:3422	ESTRUTURA	app/docs/RELATO.md:3422: Dono ESTRUTURA
BO-ADMIN-2710	2	app/docs/RELATO.md:3423	ESTRUTURA	app/docs/CORTES.md:334: ESTRUTURA dentro do item 2
BO-ADMIN-2710	3	app/docs/RELATO.md:3424	ESTRUTURA	app/docs/RELATO.md:3424: ESTRUTURA
BO-ADMIN-2710	4	app/docs/RELATO.md:3425	CADASTRO	app/docs/RELATO.md:3425: CADASTRO; confirmado em app/docs/CORTES.md:337
BO-ADMIN-2710	5	app/docs/RELATO.md:3426	ESTRUTURA	app/docs/RELATO.md:3426: Dono ESTRUTURA
BO-ADMIN-2710	6	app/docs/RELATO.md:3427	ESTRUTURA	app/docs/RELATO.md:3427: Dono ESTRUTURA
BO-ADMIN-2710	7	app/docs/RELATO.md:3428	ESTRUTURA	app/docs/RELATO.md:3428: Dono ESTRUTURA
BO-ADMIN-2710	8	app/docs/RELATO.md:3429	CADASTRO	app/docs/RELATO.md:3429: CADASTRO, ativa=False sem data_fim
BO-ADMIN-2710	8.1	app/docs/RELATO.md:3429	ESTRUTURA	app/docs/RELATO.md:3429: item 8.1 Dono ESTRUTURA
BO-ADMIN-2710	8.2	app/docs/RELATO.md:3429	SEM_DONO	app/docs/RELATO.md:52: não fechado, 8.2 e 8.3
BO-ADMIN-2710	8.3	app/docs/RELATO.md:3429	SEM_DONO	app/docs/RELATO.md:52: não fechado, 8.2 e 8.3
```

**(C) Totais por fonte × dono**

Contagem das **68 linhas efetivamente individualizadas no TSV**, conforme as provas indicadas em cada linha:

| Fonte | ESTRUTURA | CADASTRO | BATIDA | DECISAO_ADMIN | SEM_DONO | Total | Prova |
|---|---:|---:|---:|---:|---:|---:|---|
| BACKLOG-HJ, por raiz | 0 | 0 | 1 | 0 | 10 | 11 | app/docs/BACKLOG.md:784, :1917, :2547–2555; demais registros acima |
| HJ-INGESTAO, IDs recuperáveis | 6 | 3 | 5 | 0 | 3 | 17 | app/docs/RELATO.md:8390–8394 |
| BO-0611 | 0 | 2 | 0 | 0 | 12 | 14 | app/docs/BACKLOG.md:1911–1912; app/docs/RELATO.md:1471 |
| BO-1311-ROTEIRO | 0 | 0 | 0 | 0 | 10 | 10 | app/docs/BACKLOG.md:783; dono por ficha [nao verificado] |
| BO2910-ESTRUTURA | 5 | 0 | 0 | 0 | 0 | 5 | app/docs/BACKLOG.md:1102 |
| BO-ADMIN-2710 | 7 | 2 | 0 | 0 | 2 | 11 | app/docs/RELATO.md:52, :3422–3429 |
| **Subtotal individualizado** | **18** | **7** | **6** | **0** | **37** | **68** | Soma das linhas acima |

Para a **ingestão inteira declarada**, a publicação histórica dá **80 = 6 ESTRUTURA + 3 CADASTRO + 5 BATIDA + 66 sem dono publicado**. Esses 66 são os 33 “sem divergência hoje” e 33 “indeterminados”; somente três têm ID exposto. As 63 unidades restantes não foram inventadas no TSV. (app/docs/RELATO.md:8390–8395, :8398)

A lista individualizada **“falta triagem” contém os 37 SEM_DONO do TSV**. Na ingestão há ainda 63 unidades sem ID recuperável e sem dono publicado; o conteúdo integral das atas permanece **[nao verificado]**. (provas do TSV; app/docs/RELATO.md:8393–8398)

**Entrada documental ESTRUTURA recuperável: 18 = 6 da ingestão histórica + 5 BO2910 + 7 BO-ADMIN-2710.** O universo integral e a situação atual das seis unidades históricas não estão certificados; por isso, o contrato completo de entrada ainda fica **[nao verificado]**. (app/docs/RELATO.md:8390, :52; app/docs/BACKLOG.md:1102)

## 14/11 18:39 · sessao 67ed0ab09ba7 · - · - · origem=Ronald (tmux)

Fonte recuperável das atas/relatos ingeridos: **HJ-INGESTAO-0511**, 56 TXT do Fernando → **80 unidades pessoa+dia HJ-NNN-Uk**; regra escrita em `app/docs/BACKLOG.md:1868`, ingestão em `app/docs/RELATO.md:8368`, contagem em `app/docs/RELATO.md:8386`.

O rol completo mora no **mapa privado `logs/hj_0511/mapa.md`**, remissão em `app/docs/RELATO.md:8398`; arquivo **NÃO ACHADO na cópia**, portanto os IDs das outras **63 unidades são [nao verificado]**. A árvore publica individualmente apenas estes **17 IDs**, expandindo abreviações `/U2/U4` da própria tabela (`app/docs/RELATO.md:8390`–`:8394`):

```tsv
fonte	id_do_caso	arquivo:linha	dono_L099	prova_do_dono
HJ-INGESTAO-0511	HJ-001-U1	app/docs/RELATO.md:8391	CADASTRO	app/docs/RELATO.md:8391 — divergente com dono CADASTRO; lista do admin
HJ-INGESTAO-0511	HJ-001-U2	app/docs/RELATO.md:8391	CADASTRO	app/docs/RELATO.md:8391 — divergente com dono CADASTRO; lista do admin
HJ-INGESTAO-0511	HJ-002-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392 — divergente com dono BATIDA; lista do admin
HJ-INGESTAO-0511	HJ-003-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392 — divergente com dono BATIDA; lista do admin
HJ-INGESTAO-0511	HJ-005-U1	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390 — divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-005-U2	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390 — divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-005-U4	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390 — divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-005-U5	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392 — divergente com dono BATIDA; lista do admin
HJ-INGESTAO-0511	HJ-012-U1	app/docs/RELATO.md:8394	SEM_DONO	app/docs/RELATO.md:8394 — indeterminado; lavrado irregular sem segurar
HJ-INGESTAO-0511	HJ-012-U2	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390 — divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-015-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392 — divergente com dono BATIDA; lista do admin
HJ-INGESTAO-0511	HJ-016-U1	app/docs/RELATO.md:8391	CADASTRO	app/docs/RELATO.md:8391 — divergente com dono CADASTRO; lista do admin
HJ-INGESTAO-0511	HJ-017-U1	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390 — divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-027-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392 — divergente com dono BATIDA; lista do admin
HJ-INGESTAO-0511	HJ-030-U1	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390 — divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-031-U1	app/docs/RELATO.md:8394	SEM_DONO	app/docs/RELATO.md:8394 — indeterminado; lavrado irregular sem segurar
HJ-INGESTAO-0511	HJ-050-U2	app/docs/RELATO.md:8394	SEM_DONO	app/docs/RELATO.md:8394 — indeterminado; dia futuro
```

A publicação de **05/11** soma **6 ESTRUTURA + 3 CADASTRO + 5 BATIDA + 33 sem divergência + 33 indeterminados = 80** (`app/docs/RELATO.md:8390`, `:8391`, `:8392`, `:8393`, `:8394`, `:8395`). Para vocabulário solicitado, há **66 sem dono publicado**, mas somente três deles têm ID exposto (`app/docs/RELATO.md:8393`, `:8394`). O TSV recuperável soma **17 = 6 E + 3 C + 5 B + 3 SEM_DONO**; **63 sem ID**, sem inventar placeholders.

**Ressalva atual:** a linha mais recente registra mudanças de **estado**, não escreve novos donos: HJ-002-U1 D-B→S; HJ-005-U1 D-E→I; HJ-005-U2/U4 D-E→S; HJ-005-U5 D-B→S. A própria linha marca a remedida **[nao verificado]** por faltar arquivo:linha da saída (`app/docs/RELATO.md:52`). Portanto os seis ESTRUTURA acima são **donos escritos em 05/11**, e não prova de seis divergências atuais. Não retirei dono a partir de S/I por inferência.

**Atas como coleção autônoma:** **NÃO ACHADA**. Existe referência à **ATA-1111**, em aval que lista cinco itens e inclui HJ-066/068/071/073, mas ela não prova autor Fernando nem universo completo: `app/docs/PROMPTS.md:2125`, `app/docs/BACKLOG.md:2544` a `:2550`. O arquivo apontado `logs/CASO-0711/aval_caso_0711_hj003_ata_1111.txt` não está na cópia [nao verificado: conteúdo].

Greps feitos na árvore inteira, incluindo ocultos e ignorados:

```text
rg --files --hidden -g AGENTS.md
rg -n --hidden --no-ignore -i 'atas.{0,60}fernando|fernando.{0,60}atas|ata.{0,40}ingerid|ingerid.{0,40}ata' .
rg -n --hidden --no-ignore -i 'fernando' .
rg -n --hidden --no-ignore -i '\b(ingestao|ingestão|ingeridas|ingerida|ingerido|ingeridos|ingest)\b' .
rg -n --hidden --no-ignore -i '\batas\b' .
rg -n --hidden --no-ignore -i 'ATA-1111|ata 11/11|ata.{0,30}1111' .
rg -n --hidden --no-ignore 'HJ-[0-9]{3}-U[0-9]+' .
rg --files --hidden --no-ignore | rg -i 'fernando|ata|auditoria|laudo|bo[-_]'
```

Não somar os **80 ingeridos** como se fossem os **19 IDs de linhas de BACKLOG**: são granularidades diferentes, sem mapa completo para deduplicação (`app/docs/BACKLOG.md:1868`; `app/docs/RELATO.md:8398`).

## 14/11 18:39 · sessao d6c942a36ee4 · - · - · origem=Ronald (tmux)

(A) Fontes e contagem:

- **BO-0611:** encontrada como `BO-[nome]-0611` e V2, `app/docs/BACKLOG.md:1911`, `app/docs/BACKLOG.md:1912`; registro resumido vigente em `app/docs/RELATO.md:1471`. Unidade = número de item 01–14; **14 casos**, preservando 11 e 12 separados embora juntos no resumo. O quadro anterior está em `app/docs/RELATO.md:1533` e `app/docs/RELATO.md:1534`, mas foi **substituído explicitamente**, conforme `app/docs/PROMPTS.md:1925`. Assim, não usei seus donos históricos de 01/04/06 como donos vigentes. V2 escreve CADASTRO para 11/12; encaminhar outro item a uma obra não escreve seu dono. (`app/docs/RELATO.md:1471`)
- **BO-1311-ROTEIRO:** encontrada como `BO-[nome]-1311-ROTEIRO`; `app/docs/BACKLOG.md:783` enumera casos 01–10 e dez colaboradores; `app/docs/RELATO.md:226` diz que a numeração segue a ordem do UNIVERSO e é contestável. Unidade = ficha numerada, não pergunta viva; **10 casos**. Totais a/b/c/d são por pergunta, sem fechamento de dono L-099 por ficha. (`app/docs/RELATO.md:222`)
- **BO2910-ESTRUTURA:** registro completo das **cinco classes** 01,02,05,06,08 em `app/docs/BACKLOG.md:1102`, literalmente “classes ESTRUTURA ... cada uma como linha”. Unidade = classe enumerada nessa linha, não todos os BOs do dia 29/10. As queixas originais podem conter outros donos; não expandi a fonte. (`app/docs/RELATO.md:2829`, `app/docs/RELATO.md:2848`, `app/docs/RELATO.md:2849`, `app/docs/RELATO.md:2851`)
- **BO-ADMIN-2710:** oito itens originais em `app/docs/CORTES.md:330`; três complementos do item 8 em `app/docs/CORTES.md:332`; resultado em `app/docs/RELATO.md:3422` a `app/docs/RELATO.md:3429`. Unidade = item 1–8 mais 8.1,8.2,8.3; **11 casos**, expressamente confirmado por `app/docs/RELATO.md:52`. Item 2 teve CADASTRO preliminar corrigido: “ESTRUTURA dentro do item 2”, `app/docs/CORTES.md:334`. Resumo mais recente fecha **7 ESTRUTURA, 2 CADASTRO, 2 não fechados**, identificando estes como 8.2/8.3. (`app/docs/RELATO.md:52`)

Os registros privados `logs/bo_mauricio_0611/`, `logs/BO-[nome]-1311/`, `logs/bo2910/`, `logs/bo2710/` **NÃO ACHADOS nesta cópia**; `ls -ld logs .git` devolveu inexistência. [nao verificado por arquivo:linha; verificação de árvore nesta exploração]

Greps executados em toda a árvore, incluindo ocultos/ignorados:

```sh
rg --files --hidden --no-ignore -g 'AGENTS.md'
rg -n -i --hidden --no-ignore 'BO[-_]?0611|BO[-_]?1311[-_]?ROTEIRO|BO2910[-_]?ESTRUTURA|BO[-_]?ADMIN[-_]?2710' .
rg -n -i --hidden --no-ignore '1311.*ROTEIRO|ROTEIRO.*1311|BO.?06.?11|BO.?0611|BO.?1311' app/docs/RELATO-ARQUIVO.md app/docs/LAPIDES.md app/docs/DOSSIES.md app/docs/TICKETS.md app/docs/RELATO.md app/docs/BACKLOG.md app/docs/CORTES.md
rg -n -i --hidden --no-ignore 'bo_mauricio_0611|BO-[nome]-0611|BO-[nome]-1311|bo_mauricio_1311|bo2910|BO2910' .
rg --files --hidden --no-ignore | rg -i '0611|1311|2910|2710|relato|lapides|dossies|tickets'
```

Não existe metadado `.git` nesta cópia; portanto `git grep` não executável aqui, e buscas equivalentes foram feitas com `rg`. [nao verificado por arquivo:linha; verificação de árvore nesta exploração]

Testes com BO no nome encontrados: sete `app/ponto/tests/test_*bo0611*`; eles tratam o caso 05 e variantes sintéticas, sem fornecer registro numerado dos 14 casos nem dono L-099 dos outros casos. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:1`, `app/ponto/tests/test_bo0611_05_templates_geo.py:1`, `app/ponto/tests/test_bo0611_05_leitores_geo.py:1`, `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:1`, `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial_django.py:9`, `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:1`)

(B) TSV:

```tsv
fonte	id_do_caso	arquivo:linha	dono_L099	prova_do_dono
BO-0611	01	app/docs/RELATO.md:1533	SEM_DONO	[nao verificado] dono vigente; quadro 18:58 substituído por app/docs/PROMPTS.md:1925
BO-0611	02	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono; V2: queixa não identificada no dado
BO-0611	03	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono
BO-0611	04	app/docs/RELATO.md:1533	SEM_DONO	[nao verificado] dono vigente; quadro 18:58 substituído por app/docs/PROMPTS.md:1925
BO-0611	05	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono
BO-0611	06	app/docs/RELATO.md:1533	SEM_DONO	[nao verificado] dono vigente; quadro 18:58 substituído por app/docs/PROMPTS.md:1925
BO-0611	07	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono
BO-0611	08	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono; sem divergência não é categoria L-099
BO-0611	09	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono
BO-0611	10	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono
BO-0611	11	app/docs/RELATO.md:1471	CADASTRO	app/docs/RELATO.md:1471: 11/12 col921 CADASTRO #37165 -> lista do admin
BO-0611	12	app/docs/RELATO.md:1471	CADASTRO	app/docs/RELATO.md:1471: 11/12 col921 CADASTRO #37165 -> lista do admin
BO-0611	13	app/docs/RELATO.md:1534	SEM_DONO	[nao verificado] frase vigente de dono; vínculo ao item13 também em app/docs/BACKLOG.md:2025
BO-0611	14	app/docs/RELATO.md:1534	SEM_DONO	[nao verificado] frase vigente de dono; vínculo ao item14 também em app/docs/BACKLOG.md:2025
BO-1311-ROTEIRO	01	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	02	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	03	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	04	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	05	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	06	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	07	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	08	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	09	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	10	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO2910-ESTRUTURA	01	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA ... cada uma como linha ... (01)
BO2910-ESTRUTURA	02	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA ... cada uma como linha ... (02)
BO2910-ESTRUTURA	05	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA ... cada uma como linha ... (05)
BO2910-ESTRUTURA	06	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA ... cada uma como linha ... (06)
BO2910-ESTRUTURA	08	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA ... cada uma como linha ... (08)
BO-ADMIN-2710	1	app/docs/RELATO.md:3422	ESTRUTURA	app/docs/RELATO.md:3422: Dono ESTRUTURA
BO-ADMIN-2710	2	app/docs/RELATO.md:3423	ESTRUTURA	app/docs/CORTES.md:334: ESTRUTURA dentro do item 2; corrige CADASTRO preliminar
BO-ADMIN-2710	3	app/docs/RELATO.md:3424	ESTRUTURA	app/docs/RELATO.md:3424: ESTRUTURA
BO-ADMIN-2710	4	app/docs/RELATO.md:3425	CADASTRO	app/docs/RELATO.md:3425: CADASTRO; confirmado app/docs/CORTES.md:337
BO-ADMIN-2710	5	app/docs/RELATO.md:3426	ESTRUTURA	app/docs/RELATO.md:3426: Dono ESTRUTURA
BO-ADMIN-2710	6	app/docs/RELATO.md:3427	ESTRUTURA	app/docs/RELATO.md:3427: Dono ESTRUTURA
BO-ADMIN-2710	7	app/docs/RELATO.md:3428	ESTRUTURA	app/docs/RELATO.md:3428: Dono ESTRUTURA
BO-ADMIN-2710	8	app/docs/RELATO.md:3429	CADASTRO	app/docs/RELATO.md:3429: CADASTRO (ativa=False sem data_fim)
BO-ADMIN-2710	8.1	app/docs/RELATO.md:3429	ESTRUTURA	app/docs/RELATO.md:3429: (8.1) ... Dono ESTRUTURA
BO-ADMIN-2710	8.2	app/docs/RELATO.md:3429	SEM_DONO	app/docs/RELATO.md:52: nao fechado 2 (8.2, 8.3)
BO-ADMIN-2710	8.3	app/docs/RELATO.md:3429	SEM_DONO	app/docs/RELATO.md:52: nao fechado 2 (8.2, 8.3)
```

(C) Totais desta exploração, derivados das linhas acima:

| Fonte | ESTRUTURA | CADASTRO | BATIDA | DECISAO_ADMIN | SEM_DONO | Total |
|---|---:|---:|---:|---:|---:|---:|
| BO-0611 | 0 | 2 | 0 | 0 | 12 | 14 |
| BO-1311-ROTEIRO | 0 | 0 | 0 | 0 | 10 | 10 |
| BO2910-ESTRUTURA | 5 | 0 | 0 | 0 | 0 | 5 |
| BO-ADMIN-2710 | 7 | 2 | 0 | 0 | 2 | 11 |
| **Subtotal** | **12** | **4** | **0** | **0** | **24** | **40** |

Provas das contagens e categorias: `app/docs/BACKLOG.md:1911`, `app/docs/PROMPTS.md:1925`, `app/docs/RELATO.md:1471`, `app/docs/BACKLOG.md:783`, `app/docs/BACKLOG.md:1102`, `app/docs/RELATO.md:52`.

## 14/11 18:39 · sessao 7f90b3c24142 · - · - · origem=Ronald (tmux)

Referência comum dos CE: `vinculo_cobre_a_data` aceita vínculo inativo datado quando cobre `d` (`app/escala/alimentacao.py:196`, `:214`, `:215`); `vinculo_do_dia` não consulta S e escolhe geradora/vigência (`app/escala/alimentacao.py:269`, `:279`, `:282`, `:285`, `:286`). CE abaixo são possibilidades do código; ocorrência em produção [não verificado].

| ID | Sítio HEAD | Cru? | Predicado | Folha/fio e caminho lido | CE | Veredito |
|---|---|---|---|---|---|---|
| T13 | `app/colaboradores/views.py:619` | sim, S | sim: S=ativo e posto diferente do alvo | Não alcança: `buscar_colabs_posto` termina em JSON com até 30 resultados (`app/colaboradores/views.py:622`, `:626`). O POST resolve colaborador independentemente, sem reutilizar QS (`:640`). | sim, CE S | CONFERE |
| T26 | `app/colaboradores/views.py:2900` | sim, S | sim: após `em_gozo_hoje`, exclui S=desligado | Não alcança: `disponibilidade` acrescenta em `parados` (`app/colaboradores/views.py:2905`) e renderiza painel (`:2933`). | sim, CE S, supondo férias cobrindo hoje | CONFERE |
| T39 | `app/colaboradores/views.py:48` | sim, A | sim: prefetch apenas A=True | Não alcança: `vincular_colaboradores` usa primeira prefetch para `sem_escala` (`app/colaboradores/views.py:71`, `:73`), filtros da tela (`:97`, `:101`) e render (`:117`, `:126`). | sim, CE A | CONFERE |
| T52 | `app/colaboradores/views_fase.py:45` | sim, A | sim: A=True, primeira por `-data_inicio` | Não alcança: `fase_12x36` usa template/âncora (`app/colaboradores/views_fase.py:47`, `:49`), passa EC para previews (`:98`) e renderiza (`:115`). | sim, CE A | CONFERE |
| P51 | `app/ponto/worklist.py:49` | sim, A | sim: primeira A=True fornece posto | Não alcança: `coletar_gaps_dp` transporta posto no resultado (`app/ponto/worklist.py:50`, `:66`, `:69`) → `painel_inconsistencias_posto` agrupa (`app/colaboradores/views.py:3025`, `:3026`) e renderiza (`:3029`). | sim, CE A | CONFERE |
| C26 | `app/escala/services/cadastro_realidade.py:575` | sim, S | sim: exclui S=desligado | Lista C4: `_afastado_sem_registro` retorna assinatura sem data (`app/escala/services/cadastro_realidade.py:600`, `:606`) → `lista` incorpora (`:794`, `:898`, `:917`, `:942`). Existe caminho **diagnóstico** até vigia: `ListaDoAdmin._carregar` (`app/chamados/services/vigia_fio.py:216`, `:218`) → `colab_na_lista` (`:248`, `:251`) → causa d/e de `censo_alcance` (`:682`, `:701`). Não altera total (`:700`); C4 não entra em `_dias`, pois não tem `data` (`app/escala/services/cadastro_realidade.py:1033`). Atenção: verificar redundância do filtro frente ao novo universo que já exclui desligados antes de classificar C26. | sim, CE S quanto à lista | Predicado CONFERE; impacto exige consolidação descrita abaixo |
| C53 | `app/ponto/management/commands/reconciliar_fantasmas.py:174` | sim, A | sim: primeira A=True fornece diagnóstico do chamado antigo | Não alcança decisão: EC → `_completude` (`app/ponto/management/commands/reconciliar_fantasmas.py:205`) → `detalhe` (`:206`). Decisão é `_tta_s141(colab,d)` (`:248`) e `premissa_avaliar` com fato turno fechado (`:256`). `detalhe` só entra no motivo (`:278`), passado a `retratar` (`:280`), e na auditoria (`:299`). | sim, CE A no diagnóstico | CONFERE |
| C70 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1476` | sim, S | sim: S!=ativo incrementa piso e pula A4 | Não alcança: candidato entra em `_a4` (`app/chamados/management/commands/auditar_invariantes_chamados.py:1485`), depois resultado do auditor (`:1486`, `:1505`), stdout (`:1841`) e HTML opcional (`:1853`). Não executa reabertura. | sim, CE S | CONFERE |
| A24 | `app/relatorios/furos.py:117` | sim, S | sim: S∈{ativo,afastado,ferias} | Não alcança: `dataset_furos` monta/retorna cards (`app/relatorios/furos.py:142`, `:167`) → view consome (`app/relatorios/views.py:1541`) → PDF (`:1562`) ou render (`:1566`). | sim, CE S histórico | CONFERE |
| A37 | `app/inteligencia/calculadores.py:139` | sim, S | sim: mesmo conjunto de S | Não alcança: `preenchimento_marcos` monta numerador/denominador (`app/inteligencia/calculadores.py:162`, `:174`) e retorna linhas (`:177`) → `metricas.calcular` (`app/inteligencia/metricas.py:42`) grava `MetricaSnapshot` (`:47`). | sim, CE S histórico | CONFERE |
| A50 | `app/inteligencia/views.py:209` | sim, S | sim: S∈{ativo,ferias,afastado}, empresa opcional | Não alcança: base de `api_kpi`, consumida em ramos que retornam JSON (`app/inteligencia/views.py:220`, `:222`, `:226`, `:228`, `:232`, `:235`, `:260`, `:263`, `:293`, `:295`). | sim, CE S | CONFERE |

C26: o caminho estrutural até diagnóstico do vigia está comprovado. Para classificá-lo DIVERGE por “alcança fio”, considere que o filtro exclui apenas S=desligado; no HEAD `esta_desligado` é exatamente S=desligado (`app/colaboradores/services/desligamento.py:23`, `:32`), e `censo_alcance` recebe o universo de `dias_que_seguram` (`app/chamados/services/vigia_fio.py:586`). Se esse produtor já exclui desligados em todos os casos, o efeito de C26 na causa d/e é redundante nesse universo e a afirmação funcional “não/não” do censo continua sustentável. A definição do censo inclui universos/roteamento de vigia, portanto sugiro explicitar esse caminho e a redundância na consolidação, em vez de omiti-lo.

C53 está confirmado como independente: trocar/remover a EC selecionada altera diagnóstico/motivo, mas não os argumentos nem o resultado do núcleo e da premissa que autorizam retratação (`app/ponto/management/commands/reconciliar_fantasmas.py:204`, `:248`, `:256`, `:278`).

## 14/11 18:39 · sessao a198a5bfff52 · - · - · origem=Ronald (tmux)

Verificação dos seis: **4 CONFERE, 2 DIVERGE, 0 SUMIU**. F05/F06 permanecem como leitores de D, mas o caminho causal de retenção/TXT/vigia indicado no censo não se sustenta: D ≤ fim já causa saída por rescisão antes do retentor (`app/folha/export.py:866`, `app/folha/export.py:871`, `app/folha/export.py:968`).

| ID | Sítio atual; campo cru | Predicado | Alcance comprovado | CE | Veredito |
|---|---|---|---|---|---|
| F01 | `app/folha/export.py:60`; **sim**, D | **Sim**: D não nula e D ≤ fim. | `gerar_dominio_txt` (`app/folha/views.py:212`) → `classificar_export` (`app/folha/export.py:866`) → `sai_pela_rescisao` (`app/folha/export.py:60`); resultado `fora` e `continue` (`app/folha/export.py:867`, `app/folha/export.py:871`). O gerador passa os itens à montagem (`app/folha/views.py:268`), cujo conjunto `entra` exclui o colaborador (`app/folha/export.py:716`, `app/folha/export.py:726`). TXT sai em `app/folha/views.py:281`, `app/folha/views.py:285`. | **Sim**, como diferença entre rota financeira e existência do vínculo: a autoridade não consulta D (`app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:285`); ocorrência [não verificado]. | **CONFERE** |
| F02 | `app/folha/export.py:807`, `app/folha/export.py:810`; **sim**, A+S | **Sim**: A=True, 12x36, âncora nula, S=ativo, participante dos fechamentos (`app/folha/export.py:807`, `app/folha/export.py:808`, `app/folha/export.py:809`, `app/folha/export.py:810`, `app/folha/export.py:811`). | `aprovar_fechamento` → `classificar_export` (`app/ponto/views.py:1299`) → query (`app/folha/export.py:806`); produz trava (`app/folha/export.py:835`, `app/folha/export.py:836`, `app/folha/export.py:848`), lida em `app/ponto/views.py:1301`; `trava_do_grupo` (`app/ponto/views.py:1311`) impede inclusão em `_pks` (`app/ponto/views.py:1312`, `app/ponto/views.py:1315`), usados na aprovação (`app/ponto/views.py:1365`). Também impede carimbo na geração, sem impedir por si só a linha TXT (`app/folha/views.py:244`, `app/folha/views.py:247`, `app/folha/views.py:252`). | **Sim**: S não é consultada pela autoridade; EC inativa com fim cobrindo d continua candidata (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:285`). | **CONFERE** |
| F05 | Movido a `app/folha/export.py:1241`, `app/folha/export.py:1242`, no helper `_dias_sem_decisao`; **sim**, D | **Sim localmente**: D < fim reduz fim a D; janela colapsada devolve vazio (`app/folha/export.py:1241`, `app/folha/export.py:1243`, `app/folha/export.py:1246`). Mas agora o retentor só chega ao helper depois de excluir isento/desligado (`app/folha/export.py:1207`, `app/folha/export.py:1212`). | **Não alcança a trava TXT pelo RET descrito**: truncamento efetivo implica D < fim, já capturado por F01 e seu `continue` (`app/folha/export.py:866`, `app/folha/export.py:871`), antes da chamada do retentor (`app/folha/export.py:968`). O vigia usa somente classificados `furo_espelho` (`app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`), portanto também fica mascarado. Há alcance diagnóstico: `recalcular_fechamento_mes` → `_mre` (`app/ponto/services/fechamento.py:1067`) → `motivos_retencao_celula` (`app/folha/export.py:1307`) → helper (`app/folha/export.py:1212`) e persistência de `motivos_espelho` (`app/ponto/services/fechamento.py:1186`). Há leitura individual de fio via `o_que_segura` (`app/folha/fora_da_folha.py:224`, `app/folha/fora_da_folha.py:228`), para não desligado; isso não comprova o vigia alegado. | **Sim no helper para S≠desligado; não como CE causal de trava TXT/vigia**. Para desligado, retorna vazio antes do campo (`app/folha/export.py:1207`, `app/folha/export.py:1211`); o mesmo helper alimenta apenas informativo (`app/folha/export.py:1226`, `app/folha/export.py:1228`). | **DIVERGE: alcance e guarda anterior omitida** |
| F06 | Movido a `app/folha/export.py:1269`, `app/folha/export.py:1271`; **sim**, D | **Sim localmente**: `d==D`; célula ausente só gera `sem_celula` se d<hoje e d≠D (`app/folha/export.py:1269`, `app/folha/export.py:1270`, `app/folha/export.py:1271`, `app/folha/export.py:1272`). Exceção semelhante para célula sem veredito (`app/folha/export.py:1295`). Mesma guarda nova de F05 (`app/folha/export.py:1207`). | **Não alcança a trava TXT/vigia pelo caminho alegado**: para igualdade afetar o laço é preciso D=d≤fim (`app/folha/export.py:1265`, `app/folha/export.py:1269`); F01 já excluiu esse colaborador (`app/folha/export.py:866`, `app/folha/export.py:871`). Restam os caminhos diagnósticos/individuais descritos em F05 (`app/ponto/services/fechamento.py:1067`, `app/folha/fora_da_folha.py:224`). | **Sim localmente**, escolhendo S≠desligado, d passado, EC cobrindo D=data_fim, sem célula: autoridade inclui igualdade (`app/escala/alimentacao.py:215`, `app/escala/alimentacao.py:285`), helper dispensa (`app/folha/export.py:1271`). **Não demonstra a trava alegada** (`app/folha/export.py:866`). | **DIVERGE: alcance e guarda anterior omitida** |
| P34 | `app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:438`, `app/ponto/services/fechamento.py:440`; **sim**, S+D | **Sim**: S≠desligado OU S=desligado e D≥início por empresa; sem teto superior (`app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:438`, `app/ponto/services/fechamento.py:440`, `app/ponto/services/fechamento.py:441`). | `recalcular_fechamento_mes` → `universo_da_competencia` (`app/ponto/services/fechamento.py:554`) → queryset retornado (`app/ponto/services/fechamento.py:445`) → laço de colaboradores (`app/ponto/services/fechamento.py:619`) → grava fechamento (`app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1160`). | **Sim**: desligado com D nula é excluído pelo predicado, mesmo havendo EC válida; autoridade considera EC sem consultar S/D (`app/ponto/services/fechamento.py:440`, `app/ponto/services/fechamento.py:441`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:285`). | **CONFERE** |
| C35 | `app/escala/services/leitor_celula.py:365`, `app/escala/services/leitor_celula.py:389`; **sim**, D | **Sim**: d>D OU d=D e sem fato, além do ramo admissão; fato é minutos realizados OU n_celulas−n_missing>0 (`app/escala/services/leitor_celula.py:385`, `app/escala/services/leitor_celula.py:386`, `app/escala/services/leitor_celula.py:389`). | **Folha**: `recalcular_fechamento_mes` → `grade_do_fechamento` (`app/ponto/services/fechamento.py:1014`) → `grade_da_celula` (`app/folha/export.py:338`) → corte/`fora_vinculo` (`app/escala/services/leitor_celula.py:389`, `app/escala/services/leitor_celula.py:393`); excluído dos dias internos (`app/ponto/services/fechamento.py:1043`) e dos dias trabalho/ausência (`app/ponto/services/fechamento.py:1046`), com campos apurados (`app/ponto/services/fechamento.py:1048`, `app/ponto/services/fechamento.py:1083`). **Fio**: `gerar_perguntas_disputa` → `_data_furo_da_grade` (`app/chamados/services/disputa_emissao.py:350`) → `grade_da_celula` (`app/chamados/services/disputa_emissao.py:83`); só aceita `trabalho`, senão None (`app/chamados/services/disputa_emissao.py:85`, `app/chamados/services/disputa_emissao.py:87`); texto ausente causa `continue` (`app/chamados/services/disputa_emissao.py:360`, `app/chamados/services/disputa_emissao.py:366`) antes de `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:444`). | **Sim**: D=data_fim, sem fato; igualdade incluída pela autoridade (`app/escala/alimentacao.py:215`, `app/escala/alimentacao.py:282`), excluída pela grade (`app/escala/services/leitor_celula.py:389`). | **CONFERE** |

Para o fechamento solicitado: **F05 e F06 caem como provas dos caminhos RET→TXT e dias da folha→vigia apresentados no censo**, embora os sítios crus não tenham sumido e ainda alcancem diagnóstico/informativo/leitura individual (`app/folha/export.py:1241`, `app/folha/export.py:1269`, `app/ponto/services/fechamento.py:1067`, `app/folha/fora_da_folha.py:224`, `app/folha/export.py:1228`).

## 14/11 18:39 · sessao 8520b811ee6b · - · - · origem=Ronald (tmux)

Todos os oito sítios permanecem; predicados conferem. Recomendo **8 CONFERE**, com ressalva explícita sobre o alcance condicional de P39.

A autoridade aceita EC inativa com fim declarado cobrindo a data (`app/escala/alimentacao.py:196`, `:214`, `:215`); escolhe pela geradora ou pelos vínculos cobertos e não lê `situacao` (`app/escala/alimentacao.py:269`, `:279`, `:282`, `:285`, `:286`). Portanto CE S/A procedem como hipóteses de código nos oito casos; ocorrência de produção é [não verificado].

| ID | Sítio HEAD / leitura | Predicado | Caminho comprovado / alcance | CE | Veredito |
|---|---|---|---|---|---|
| P39 | `app/ponto/services/he_pendente_lavrado.py:92`, S: sim | Sim: empresa e `situacao='ativo'`; fornece dias/retrato em `:160`, `:169` | **Folha sim, condicional; fio não demonstrado.** Caminho original: `apurar` (`he_pendente_lavrado.py:92`, `:160`) → `lavrar` (`app/ponto/management/commands/lavrar_he_pendente.py:62`, `:71`; grava em `he_pendente_lavrado.py:182`) → `ler` (`app/ponto/management/commands/recusar_ponta_pequena.py:98`) → alvos (`:113`, `:156`) → `recusar_ponta_pequena` (`:170`) → `_gravar` (`app/ponto/portas/he.py:341`) → estado vivo (`app/ponto/services/gestao_he.py:86`) → `medir` (`app/folha/porta_export.py:684`, `:686`) → teste de trava (`:230`, `:232`, `:690`) e recusa (`:853`, `:865`). A pequena estável não trava: a recusa automática rejeita acima do limite (`he.py:313`) e TXT só trava acima (`porta_export.py:230`). Ainda pode alcançar com foto pequena e vivo posterior grande; comando exige apenas foto do mesmo dia (`recusar_ponta_pequena.py:105`, `:106`). **Caminho adicional mais direto:** lavrador injeta `_revogar` (`lavrar_he_pendente.py:56`, `:59`, `:62`) → `apurar` chama callback somente para os S ativos (`he_pendente_lavrado.py:92`, `:143`) → `revogar_recusa_do_sistema` marca revogada (`he.py:472`) → `_marcar_revogada.save` (`he.py:373`, `:375`) → `estado_por_dia` lê só vigentes (`gestao_he.py:86`, `:87`) → portão supracitado. | Sim, S atual pode omitir vínculo histórico válido e impedir a revogação (`he_pendente_lavrado.py:92`, `:143`; autoridade acima) | CONFERE, alcance condicional precisa da ressalva |
| P44 | `app/ponto/services/vigia_de_hora.py:117`, S: sim | Sim: pula desligado **ou isento** (`:117`, `:119`) | **Fio sim**, `rodar` → `julgar_celula` (`vigia_de_hora.py:131`) → `julgar_colab` (`app/ponto/services/cartorio.py:2040`) → `emissor._abrir_chamado` sob apply e demais gates (`cartorio.py:1791`, `:1829`) → `ChamadoColaborador.abrir` (`app/ponto/management/commands/detectar_ausencias.py:694`). Raw gate também impede julgamento/reconciliação, como alegado. Folha não demonstrada neste fluxo. | Sim S, inclusive ontem (candidatas de hoje/ontem em `vigia_de_hora.py:114`, `:115`) | CONFERE |
| P66 | `app/chamados/services/pergunta_no_app.py:186`, S: sim | Sim: COBRANCA, módulos `CALADOS_POR_ISENCAO`, colaborador ativo (`:185`, `:186`) | **Fio sim, sob apply**. `Command.handle` → `medir` (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:21`) → filtro/achados (`pergunta_no_app.py:186`, `:201`, `:203`) → `perguntar` (`command:29`; dry retorna em `:26`, `:28`) → `criar_disputa_a_partir_de_chamados_orfaos` (`pergunta_no_app.py:246`) → pai/disputa (`app/chamados/services/disputa_emissao.py:1104`, `:1123`) → `_complementar_marcos_faltantes` (`:1175`) → `_criar_pergunta_se_couber` (`:881`) → `PerguntaDisputa.abrir` (`:837`). Folha não demonstrada. | Sim S | CONFERE |
| P65 | `app/chamados/services/limbo_folgas.py:65`, A: sim | Sim: conta A=True do template, ou usa 1 sem tid (`:64`, `:65`) | **Não alcança folha/fio decisório neste sítio.** `aplicar_sugestao` → `texto_seco(sug,n_tpl)` → `reescrever_texto` (`:70`); n_tpl só forma sufixo textual (`:49`, `:51`, `:52`). Aceitação decide pelos dias/classe/tid (`:82`, `:85`), não pelo contador. | Sim A para contagem do alcance; não prova mudança de emissão (`:51`, `:65`) | CONFERE |
| P22 | `app/ponto/selecao_periodo.py:278`, A: sim | Sim: mapa de jornada por A=True; fallback 24h (`:269`, `:278`, `:280`, `:287`). Nota: `order_by(data_inicio)` + `setdefault` escolhe a **primeira mais antiga**, apesar do comentário dizer mais recente (`:276`, `:279`, `:280`); o censo não alegou ordem. | **Não alcança** no fluxo encontrado. `turnos_em_turno_stored` retorna mapa (`:292`, `:293`) → wrapper `colabs_em_turno_stored` (`:253`) → endpoint de dashboard usa tamanho (`app/core/views.py:1160`, `:1161`). | Sim A: pode usar jornada atual diferente daquela do turno histórico, ou fallback em EC inativa válida (`:278`, `:287`) | CONFERE |
| C64 | `app/ferias/management/commands/alertar_retornos.py:30`, S: sim | Sim: ativo; EC do mapa ativo (`:28`, `:31`), hoje False e amanhã True (`:34`) | **Não alcança folha/fio decisório.** `handle` → lista retornam (`:35`) → `push_dp` (`:45`), aviso para disponibilidade. | Sim S | CONFERE |
| C11 | `app/escala/views_plano_folgas.py:176`, A: sim | Sim: pk e A=True; ausente erro `vinculo` e continue (`:176`, `:178`, `:179`) | **Folha e fio sim.** `plano_folgas_aplicar` → `aplicar_folgas_em_lote` (`:185`) → `relavrar_celulas_da_foto` (`app/escala/folgas.py:418`) → `regenerar_celulas_vinculo` (`folgas.py:293`; reexport em `app/ponto/management/commands/gerar_celulas.py:13`) → `_o_chao_do_dia_mudou` (`app/ponto/portas/celula.py:392`) → `julgar_celula` (`:472`), `recalcular_por_evento` (`:496`), callback de rejunção/reavaliação (`:531`, `:536`, `:539`, `:540`). | Sim A; vínculo inativo cobrindo um dia editável é barrado antes da porta (`views_plano_folgas.py:176`, `:179`) | CONFERE |
| C40 | `app/ponto/management/commands/reconciliar_grade.py:106`, S: sim | Sim: ativo, empresa, IDs na união de batidas/chamados/em operação (`:84`, `:95`, `:105`, `:106`) | **Fio sim sob apply**. `handle` filtro → `_emitir_furo` (`:250`, `:254`) → `ChamadoColaborador.abrir` (`:507`), eventual `reabrir` (`:530`); ramo de retratação → `retratar` (`:305`, `:309`). Cron cadastrado **sem apply** (`app/config/crons.py:462`), como censo diz. Folha não demonstrada neste fluxo. | Sim S | CONFERE |

Não há metadado Git disponível nesta cópia para autenticar o hash por `git rev-parse`; identidade do HEAD informado pelo usuário é [não verificado]. Nenhuma conclusão acima depende de banco/log.

## 14/11 18:39 · sessao 260c338b86e1 · - · - · origem=Ronald (tmux)

Contagem reproduzível: `re.match(r'^\| (HJ-[^|]+?) \|', linha)` em `app/docs/BACKLOG.md` encontra **20 linhas**: **10 IDs exatos HJ-NNN**, **9 derivados**, **1 instrumento de ingestão**. Os intervalos comprobatórios são BACKLOG:734,744–749,784,786,1868,1917,2547–2555.

**Não equiparar automaticamente as 19 linhas sem instrumento a 19 BO originais.** HJ-091-FILA-1..4 são achados do opus; HJ-091-OPUS3-FILA reúne seis achados; HJ-091-LISTA registra complemento de pouso. BACKLOG:744–749. Uma regra de caso por **raiz HJ-NNN representada nas linhas** produz **11 casos**: os 10 IDs diretos abaixo mais HJ-091, documentado pelos derivados e pela menção expressa em BACKLOG:1917. O registro individual original dos 11 e sua granularidade pessoa+dia não estão completos na cópia. [nao verificado] [nao verificado]

TSV das **19 ocorrências de registro**, preservando cada ID literal; a classificação de obras da GRADE não foi usada como dono do caso. O único dono inequívoco de linha HJ encontrado é HJ-092, “FECHADA como BATIDA”. BACKLOG:784.

```tsv
fonte	id_do_caso	arquivo:linha	dono_L099	prova_do_dono
BACKLOG-HJ	HJ-091-REJEITADA-SOBREVIVE-A-JANELA	app/docs/BACKLOG.md:734	SEM_DONO	[nao verificado]; lei pendente na linha; sem dono único L-099 fechado localizado
BACKLOG-HJ	HJ-091-FILA-1	app/docs/BACKLOG.md:744	SEM_DONO	[nao verificado]; achado do opus sem dono L-099 fechado localizado
BACKLOG-HJ	HJ-091-FILA-2	app/docs/BACKLOG.md:745	SEM_DONO	[nao verificado]; achado do opus sem dono L-099 fechado localizado
BACKLOG-HJ	HJ-091-FILA-3	app/docs/BACKLOG.md:746	SEM_DONO	[nao verificado]; achado do opus sem dono L-099 fechado localizado
BACKLOG-HJ	HJ-091-FILA-4	app/docs/BACKLOG.md:747	SEM_DONO	[nao verificado]; achado do opus sem dono L-099 fechado localizado
BACKLOG-HJ	HJ-091-LISTA	app/docs/BACKLOG.md:748	SEM_DONO	[nao verificado]; lista DP/passivo não fecha dono único do caso HJ
BACKLOG-HJ	HJ-091-OPUS3-FILA	app/docs/BACKLOG.md:749	SEM_DONO	[nao verificado]; linha reúne seis achados sem dono único L-099 fechado localizado
BACKLOG-HJ	HJ-092	app/docs/BACKLOG.md:784	BATIDA	app/docs/BACKLOG.md:784: FECHADA como BATIDA; app/docs/RELATO.md:271 confirma
BACKLOG-HJ	HJ-093	app/docs/BACKLOG.md:786	SEM_DONO	[nao verificado]; linha mistura pergunta de decisão humana e realizado zerado, sem dono único fechado
BACKLOG-HJ	HJ-090	app/docs/BACKLOG.md:1917	SEM_DONO	[nao verificado]; exige dono L-099 dos dias com batida, mas não fornece dono único fechado
BACKLOG-HJ	HJ-066	app/docs/BACKLOG.md:2547	SEM_DONO	[nao verificado]; fila UI não equivale a dono L-099 fechado
BACKLOG-HJ	HJ-068	app/docs/BACKLOG.md:2548	SEM_DONO	[nao verificado]; fila UI não equivale a dono L-099 fechado
BACKLOG-HJ	HJ-071	app/docs/BACKLOG.md:2549	SEM_DONO	[nao verificado]; fila UI não equivale a dono L-099 fechado
BACKLOG-HJ	HJ-073	app/docs/BACKLOG.md:2550	SEM_DONO	[nao verificado]; censo pendente sem dono L-099 fechado localizado
BACKLOG-HJ	HJ-074	app/docs/BACKLOG.md:2551	SEM_DONO	[nao verificado]; vínculo com obra não equivale a dono do caso fechado
BACKLOG-HJ	HJ-075	app/docs/BACKLOG.md:2552	SEM_DONO	[nao verificado]; aguarda print; sem dono fechado localizado
BACKLOG-HJ	HJ-075-EC-FIRST	app/docs/BACKLOG.md:2553	SEM_DONO	[nao verificado]; diz fila 1 estrutural, sem frase fechando dono L-099 do caso
BACKLOG-HJ	HJ-058	app/docs/BACKLOG.md:2554	SEM_DONO	[nao verificado]; vínculo com obra não equivale a dono do caso fechado
BACKLOG-HJ	HJ-058-MEDIA-AUSENCIAS	app/docs/BACKLOG.md:2555	SEM_DONO	[nao verificado]; origem não verificada; censo antes da cura
```

No inventário literal das 19 linhas: ESTRUTURA 0 / CADASTRO 0 / BATIDA 1 / DECISAO_ADMIN 0 / SEM_DONO 18. Pela regra conservadora de **11 raízes**, BATIDA 1 / SEM_DONO 10; demais 0. A soma é derivada dos registros acima. BACKLOG:734,744–749,784,786,1917,2547–2555.

Para o TSV principal de **11 raízes**, use os dez HJ-NNN diretos e acrescente:

```tsv
BACKLOG-HJ	HJ-091	app/docs/BACKLOG.md:1917	SEM_DONO	[nao verificado]; menção expressa +HJ-091; derivados em app/docs/BACKLOG.md:734 e :744-749; nenhum dono único L-099 fechado localizado
```

Exclusões e cuidados:

- **HJ-INGESTAO-0511** é instrumento “56 HJ -> unidades pessoa+dia”, não outro BO. BACKLOG:1868.
- **HJ-057: NAO ACHADA como registro individual**; única menção localizada: “HJ-057 sem linha na copia”. RELATO:52. Grep executado: `rg -n --hidden --no-ignore 'HJ-057'`.
- **HJ-067/HJ-069/HJ-095** aparecem como referências, sem primeiro campo HJ próprio no BACKLOG: HJ-067 em BACKLOG:2544; HJ-069 em BACKLOG:2536; HJ-095 em BACKLOG:2153.
- **HJ-058** menciona “1 caso desde 30/09 (06/11), 4 no total”; não transformar isso em quatro linhas de caso sem IDs e registros recuperáveis. BACKLOG:2554.
- **HJ-093** diz “decisao humana” para a pergunta #50873, mas também relata a célula zerada e confirma a classe de pagamento. Atribuir DECISAO_ADMIN ao HJ inteiro apagaria essa mistura; falta dono único escrito. BACKLOG:786; RELATO:266.
- **HJ-091-G** consta como “obra ESTRUTURA”; isso prova tipo da obra, sem necessariamente fechar o dono único de todos os casos HJ-091 e passivo. RELATO:66.
- Coordenei com `/root/atas`: as unidades históricas HJ-001..031 identificadas no RELATO não coincidem com os IDs diretos HJ-058..093. O acervo de 80 unidades segue separado, sem inventar IDs faltantes. RELATO:8386,8390–8394.

Buscas realizadas: `rg -n '^\| HJ-' app/docs/BACKLOG.md`; `rg -n '\bHJ-[0-9]{3}\b' app/docs/BACKLOG.md`; `rg -n --hidden --no-ignore 'HJ-'` nos RELATO, RELATO-ARQUIVO, LAPIDES, DOSSIES, TICKETS e CORTES; busca ampla por HJ e dono/ESTRUTURA/CADASTRO/BATIDA na árvore. Nenhum arquivo foi criado, editado ou apagado. [nao verificado por diff: cópia sem .git]

## 14/11 18:37 · sessao 17c379e7a817 · - · - · origem=Ronald (tmux)

Censo por AST + buscas textuais em produção; não alterei arquivos. As linhas de `fio.py` e `disputa_emissao.py` precisam ser atualizadas após a cura do pai. [nao verificado]

**Escritores**

- `chamado_destino`: `chamados/models.py:367` (`ChamadoColaborador._vivas_entre`, simulação); `:3043` (`PerguntaDisputa._nascer`, reuso); `:3075` (`_nascer`, construtor). Os sítios `:2846` e `:2858` encaminham o argumento para `_sob_contrato`, sem atribuição própria. Na base, `chamados/services/fio.py:825`, `:829` (`_costura_nao_mata_o_fio`, simulação/restauro) e `:971` (`reapontar_resolvedora`). A cura em andamento já concentrou essas atribuições em `chamados/services/fio.py:790`, `:793`, `:796`.
- `DS.fechada_em`: `chamados/models.py:1988` (`fechar`, save `:1992`), `:2205` (`reabrir_admin`, save `:2210`), `:2229` (`reabrir_sistema`, save `:2233`).
- `DS.motivos`: `chamados/models.py:2022` (`aparar_motivo_respondido`), `:2119` (`remover_motivo`), `:2215` (`reabrir_admin`); `chamados/services/disputa_emissao.py:601` (`_agregar_motivos_na_dona`, save `:606`), e nascimentos `:712`, `:1354`, `:1625`.
- `Pergunta.respondida_em`: `chamados/models.py:1555` (`_universo_da_ressuscitacao`, simulação), `:3193` (`carimbar`), `:3224` (`_desfazer_em_memoria`), `:3323` (`descarimbar_em_lote`, recebe `**outros` de `:1613` e `disputa_emissao.py:1857`); `chamados/services/disputa_emissao.py:1732` (`responder_pergunta_disputa_core`), `:1917` (`responder_automaticamente`), `:1938` (`carimbar_resposta_pela_trilha`).

**DS.motivos — todos os leitores encontrados e pressuposto**

| Sítio | Uso / pressuposto |
|---|---|
| `chamados/models.py:2017` | `aparar_motivo_respondido`: lista de motivos restantes |
| `chamados/models.py:2088` | `remover_motivo`: lista completa decide remoção e fecho |
| `chamados/models.py:2212` | `reabrir_admin`: acrescenta ao agregado |
| `chamados/services/disputa_emissao.py:579` | `_contexto_do_ato`: presença determina contador de recorrência |
| `chamados/services/disputa_emissao.py:597` | `_agregar_motivos_na_dona`: diferença/união com agregado |
| `chamados/services/disputa_emissao.py:1306` | repasse: entrada do complemento |
| `chamados/services/disputa_emissao.py:1327` | repasse: seed e motivos existentes |
| `chamados/services/descarte_disputa.py:9` | fotografa agregado fechado |
| `chamados/services/descarte_disputa.py:10` | calcula somente motivos ainda não agregados |
| `chamados/services/fio.py:680` | corpo do push de regularização |
| `chamados/services/acoes_disputa.py:149` | corpo do push de regularização |
| `chamados/relatorios_disputa.py:52` | exibe labels do agregado |
| `chamados/management/commands/relatorio_pendencias_disputa.py:39` | exibe labels recebidos do relatório |
| `chamados/management/commands/auditar_disputas.py:88` | verifica contexto de cada motivo agregado |
| `chamados/management/commands/auditar_disputas.py:142` | relatório de disputa parada |
| `chamados/management/commands/auditar_disputas.py:169` | compara todos os motivos com todas as perguntas; supõe agregado completo |
| `chamados/management/commands/auditar_invariantes_chamados.py:50` | universo dos dias na disputa sem pergunta; supõe todos os motivos agregados |
| `ponto/management/commands/alertar_disputas_pendentes.py:82` | log de disputa velha |
| `api/foto_recebida.py:63` | filtro exige `foto_ausente` agregado |
| `api/foto_recebida.py:70` | restantes decidem aparar ou fechar; supõe agregado completo |
| `escala/signals.py:171` | filtro exige `ancora_ausente` agregado |
| `colaboradores/signals.py:74` | filtro exige `posto_desativado` agregado |
| `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49` | descoberta exige `device_comprometido` agregado |
| `templates/chamados/partials/modal_fio.html:327` | fluxo de âncora depende de motivo agregado |
| `templates/chamados/detalhe_local.html:301` | exibe agregado |

Omitir um motivo sem pergunta local pode excluir a disputa dos três filtros de evento — foto, âncora e posto — e da descoberta de aparelhos compartilhados (`api/foto_recebida.py:63`, `escala/signals.py:171`, `colaboradores/signals.py:74`, `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49`). O drawer explicitamente usa perguntas pendentes em vez de `DS.motivos` (`colaboradores/services/drawer.py:75`).

**DS.fechada_em — leitores**

- `chamados/models.py`: `transicionar:190`; `ressuscitar_perguntas:1621`; `aberta:1871`; `fechar:1935`; `remover_motivo:2087`; `reabrir_admin:2194`; `reabrir_sistema:2223`; `reabrir_por_colab:2280`; `limpar_resolucao_residual:2318`; `trava_na_ordem:2729`.
- `chamados/signals.py`: `disputa_fechada_fecha_chamado_pai:29`; `retoma_regularizacoes_ao_fechar_disputa:131`; `materializa_perguntas_ao_fechar_disputa:155`.
- `chamados/juizes.py:2396` (`disputa_viva_do_chamado`).
- `chamados/reconciliador.py`: `lavrar_sem_turno_aberto:412`; `retratar_perguntas_absolvidas_por_celula:600,611,612`; `_orfas_abertas:1050`; `encerrar_orfas_pela_lampada:1161,1162`; `perguntas_orfas_reatavies:1504`.
- `chamados/services/disputa_emissao.py`: `colaborador_em_disputa:42`; `_agregar_motivos_na_dona:593`; `agregar_estrangeiras:647`; `_abrir_ou_agregar_disputa:671`; `criar_ou_atualizar_disputa:874,884`; `fecho_do_repasse:1192`; `notificar_complementos:1282`; agrupador `:1324,1554`; âncora `:1598`; resposta `:1786`; `_resolver_perguntas_por_chamado_resolvido:2173,2268,2292,2294,2353,2355`.
- `chamados/services/fio.py`: `_disputa_do_chamado:131,135`; `montar_chamados_fio:187`; `processar_post_fio:600,633`.
- `chamados/services/acoes_disputa.py:255,347`; `acoes_chamado.py:346`; `becos.py:27,56,59`; `cobranca.py:230`; `competencia_trancada.py:176,221`; `descarte_disputa.py:9`; `fila_por_causa.py:225,259,262`; `foto_e4b.py:182`; `materializacao.py:591,1322`; `pergunta_no_app.py:71`; `regularizacao_ext.py:60`; `relavra_via.py:183,260`; `validacao.py:171,237,240,242`; `vigia_fio.py:753`.
- `chamados/relatorios_disputa.py:41`; `chamados/views.py:471,495,512,530,911,1329`; `chamados/views_cobrar.py:161,164`.
- Comandos de `chamados/management/commands/`: `auditar_chamados_orfaos.py:41`; `auditar_disputas.py:72,87,136,152,167`; `auditar_invariantes_chamados.py:63,620,676,890,1851`; `encerrar_perguntas_com_fato.py:82`; `fechar_becos_disputa.py:32`; `reabrir_orfas_enterradas.py:96,131,164,165`.
- `api/foto_recebida.py:62`; `api/views.py:1013,1029,2671`; `escala/signals.py:170`; `colaboradores/signals.py:73`; `colaboradores/painel_cell.py:32`; `colaboradores/services/desligamento.py:164`; `drawer.py:70`; `situacional.py:117`.
- `ponto/management/commands/alertar_disputas_pendentes.py:34`; `escalonar_chamados_supervisao.py:58,149`; `reconciliar_fantasmas.py:214`; `ponto/services/entrada_sincronizada.py:57`.
- Template: `templates/chamados/partials/modal_fio.html:379`.

**Pergunta.respondida_em — leitores**

- `chamados/models.py`: `sem_veredito_entre:1900`; `estado:3106,3109`; `carimbar:3188,3192`; `q_vivas:3409`; `respondida:3559`; `DecisaoPergunta.linhas_da_projecao_sem_linha:3985,3992,3993,3994`.
- `chamados/juizes.py`: `_slot_esperado:348`; `_chave_memo_pv:852`; `projecao_temporal_da_pergunta:1385`; `respondida_sem_texto:1528`; `perguntas_corrigiveis:1673`; `perguntas_respondidas_a_validar:1680,1683`.
- `chamados/veredito_lavrado.py:207,208`; `chamados/relatorios_disputa.py:21`; `chamados/views.py:1280,1416`; `chamados/views_cobrar.py:169,176`.
- `chamados/services/fio.py:340,660`; `acoes_disputa.py:95,130`; `becos.py:30`; `cobranca.py:229`; `declaracao_texto.py:406,408,674`; `disputa_emissao.py:1934,1977,2085,2103,2243,2316,2333`; `foto_e4b.py:180`; `furo_sem_canal.py:81`; `historico.py:99`; `materializacao.py:675,1376`; `painel.py:67`; `passivo_validado.py:66`; `pergunta_no_app.py:71`; `raiox.py:138`; `validacao.py:416,603,608,632,951`; `verbos.py:335,336,342,345`; `vigia_fio.py:680,788`.
- Comandos: `chamados/management/commands/auditar_invariantes_chamados.py:261,676,1191,1658,1689`; `backfill_decisoes_pergunta.py:56,60`; `curar_perguntas_em_limbo.py:46,69`; `ponto/management/commands/reconciliar_grade.py:445`.
- `api/views.py:1160,1172,2556,2570,2613,2630`; `api/views_mensageria.py:569,570`.
- Templates: `modal_fio.html:185,217,220`; `_card_atendimento.html:94,100,137,155,384,385`; `_pergunta_card.html:24,29,32`, todos em `templates/chamados/partials/`.

**chamado_destino — leitores**

- `chamados/models.py`: `_gravar_estado:304,310,311`; `_ancoradas:345`; `_vivas_entre:361,362,366`; `_gate_pergunta_viva:436,443,444`; `fechar:1967`; `remover_motivo:2083`; `_nascer:3010,3029,3051,3057`.
- `chamados/juizes.py`: `_pergunta_materializada_de_fato:779`; `_chave_memo_pv:843,857`; `delta_da_resposta:1860`; `motivo_implausivel_validacao:1987`; `irmas_respondidas_sem_consumo:2034`; `_dias_das_perguntas_vivas:2346`.
- `chamados/reconciliador.py:652,1052,1502`; `chamados/relatorios_disputa.py:43`; `chamados/views_cobrar.py:165`.
- `chamados/services/fio.py`: `data_do_fato_de:80,81`; `chamado_que_lista:117`; `_disputa_do_chamado:135`; `montar_chamados_fio:226,227,331,396,397`; guarda, rechaveamento e censo de colaborador cruzado nas linhas em atualização pelo pai. [nao verificado]
- `chamados/services/acoes_chamado.py:367`; `acoes_disputa.py:354`; `becos.py:30,46,47`; `competencia_trancada.py:121,132,165`; `declaracao_texto.py:302`; `disputa_emissao.py:540,812,1183,1184,1295,1316,1520,1589,1686,2063,2171,2300`; `explicador.py:122`; `foto_e4b.py:259`; `historico.py:96`; `janela_colab.py:64,65`; `limbo_folgas.py:205,240,241`; `materializacao.py:342,533,587,588,717,800,1000,1082,1111,1113,1248,1269,1271`; `pergunta_no_app.py:199,222,225`; `raiox.py:57,62,137`; `relavra_via.py:101,103,156`; `tarefa_do_dia.py:377`; `validacao.py:309,341,455,869`; `vigia_carimbo.py:103`.
- Comandos: `chamados/management/commands/auditar_disputas.py:151`; `auditar_invariantes_chamados.py:610,616,621,672,673,679,942,978,979,982,1247,1662`; `backfill_veredito_lavrado.py:101`; `lavrar_perguntas_stale.py:58,88`; `migrar_data_evento_dia_do_turno.py:377,472`; `reabrir_orfas_enterradas.py:69`; `reatar_beco_folga_ao_limbo.py:87,122,124`; `relavrar_via_i6.py:56`; `ponto/management/commands/reconciliar_fantasmas.py:212`; `reconciliar_grade.py:444`.
- `api/views.py:1031,2548,2567,2568`; `colaboradores/services/calendario.py:474`; `ponto/services/veredito_celula.py:187`.

Além das portas de cobrança já destacadas, há dois pushes de resultado de disputa ao colaborador: `chamados/services/fio.py:681` e `chamados/services/acoes_disputa.py:150`, ambos com “Sua situacao foi regularizada”. Precisam constar do censo e ter tratamento explicitado sob a exigência literal “todos” (`chamados/services/fio.py:680`, `chamados/services/acoes_disputa.py:149`).

## 14/11 18:36 · sessao 15bbc60ce588 · - · - · origem=Ronald (tmux)

Autoridade recomendada: `fio.dia_do_turno_da_linha` em `chamados/services/fio.py:1654`, já usada pelo nascimento em `chamados/models.py:2936`. Ela delega episódio ao juiz do turno (`ponto/turnos.py:2927`) e marco ao dia da célula (`ponto/turnos.py:2939`).

A substituição pode montar a mesma instância não salva usada pelo nascimento, com `instancia_do_motivo` e `batida_origem_do_nascimento`, e chamar essa autoridade. A data explícita do contexto entra somente como chave `pedido`; retire a cascata grade → contexto → batida. Sem chave nem episódio, a autoridade devolve `None` (`chamados/services/fio.py:1687`). Motivo datado/hora deve retornar `InsumoAusente`, com trilha; não deve ganhar data civil por conversa (`chamados/services/disputa_emissao.py:505`). O chamador já publica causas de recusa (`chamados/services/disputa_emissao.py:661`).

Cuidados da eleição:

- `_marcos_de_intervalo` fornece os fatos explícitos do intervalo (`chamados/services/disputa_emissao.py:263`). `motivos_atendidos` **não inclui** `intervalo_ausente`; usar somente esse juiz perderia esses candidatos (`chamados/juizes.py:2147`).
- O S160 exclui chamado com pergunta não materializada (`chamados/services/disputa_emissao.py:224`); o eleitor de intervalo inclui chamados com pergunta existente (`chamados/services/fio.py:750`). A unificação precisa conservar essa inclusão para ser idempotente.
- O `None` do S160 permite HIST/c1/c2 no reconciliador (`chamados/reconciliador.py:1559`) e reatamento imediato de pergunta recém-criada (`chamados/services/disputa_emissao.py:541`). Com menor PK na ambiguidade, esses caminhos deixam de executar para múltiplos candidatos; continuam necessários quando não há candidato.
- As duas portas de declaração passam o chamado de origem para a emissão (`chamados/services/declaracao_texto.py:365`, `chamados/services/declaracao_texto.py:653`). A resposta deve ler o destino de `perg.chamado_destino`, sem nova eleição.

Casos úteis sem banco: autoridade devolver dia diferente da chave e texto/dedup usarem o dia devolvido; autoridade devolver `None` apesar de grade/contexto/batida disponíveis e nenhum fallback ser consultado; intervalo explícito vencer chamado genérico com PK menor; permutação de pedidos conservar destino. Execução desses testes: [nao verificado]. [nao verificado]

## 14/11 18:36 · sessao 96181d5931e8 · - · - · origem=Ronald (tmux)

Censo somente leitura, linhas da base antes da cura:

| Sítio | Predicado atual |
|---|---|
| `chamados/services/disputa_emissao.py:885` — `criar_ou_atualizar_disputa` | motivos novos; DS aberta relida; não exige pergunta viva pendente |
| `chamados/services/disputa_emissao.py:1283` — `notificar_complementos` | `notificar`, motivos, dona diferente da principal e DS aberta; sem C6 |
| `chamados/services/disputa_emissao.py:1555` — órfãos | `notificar`, motivos, qualquer pergunta existente, DS aberta; inclui respondida/morta |
| `chamados/services/disputa_emissao.py:1645` — âncora | após criar conversa/analisar; sem C6 |
| `chamados/services/disputa_emissao.py:1992` — recobrança | conta todas sem resposta, mas envia mesmo com zero |
| `chamados/services/disputa_emissao.py:2041` / `:2049` — driver | só ContextVar de silêncio; envia WebPush e FCM |
| `ponto/management/commands/alertar_disputas_pendentes.py:59` | DS aberta e `renovar_contexto` concedeu carimbo diário; sem pergunta |
| `chamados/services/acoes_disputa.py:109` — reabrir | existe pergunta corrigível e `_reabrir(qs)` rodou; sem C6 após ato |
| `chamados/services/acoes_disputa.py:288` — reabrir admin | admin autorizado, DS fechada reaberta e conversa criada; sem C6 |
| `chamados/views.py:1447` — reabrir questionário | `_reabrir2(qs)` rodou; sem C6 após ato |
| `chamados/services/validacao.py:640` — recusar resposta | motivo presente, pergunta existente, respondida e não validada; reabertura rodou; sem C6 |
| `chamados/services/cobranca.py:107` / `:112`, chamado em `:187` | `checar_cobranca` e `_pendentes` já usam `consulta_do_app`; prazo não correndo; envio fora do atomic local, mas pode herdar atomic externo |
| `chamados/views_cobrar.py:44` — individual | permissão, POST, área, throttle diário; nem vitalidade nem DS aberta |
| `chamados/views_cobrar.py:123` — massa | permissão, `particionar_verbos == cobrar`, vivos, área, folha alterável, throttle; sem consulta direta à lista do app |

Avisos de conclusão de disputa também encontrados: `chamados/services/acoes_disputa.py:150`, `:156` e `chamados/services/fio.py:681`, `:688`. São posteriores à materialização e anteriores até à verificação de resultado vazio; não exigem conclusão bem-sucedida nem C6. A regra literal “todo push ... por causa de disputa” os alcança; semanticamente são avisos de conclusão, não cobrança. [Interpretação; texto literal fornecido pelo usuário.]

A fonte adequada já existe: `consulta_do_app` em `chamados/services/pergunta_no_app.py:61`. Ela toma `perguntas_do_app` (`:45`), que chama o C6 `pergunta_viva` (`:58`), e admite pendentes somente com `respondida_em is None` e disputa aberta (`:71`). Para uma DS, filtrar essa lista por `p.disputa_id == disputa.pk` conserva a mesma decisão do app.

A cura deve envolver os envios de disputa num callback de `transaction.on_commit`, relendo `consulta_do_app` dentro do callback; guardar só um booleano antes do commit pode cobrar pergunta respondida/fechada no restante da transação. `push_colaborador` só decide existência de usuário (`chamados/push_utils.py:114`) e FCM só existência de token (`chamados/fcm_utils.py:101`): o gate pertence à porta da disputa.

Atenção ao driver compartilhado: `cobranca._tocar` também é usado para abertura genérica pelo DP em `chamados/services/abertura.py:204`; pôr gate de disputa diretamente nele perderia esse caso. O `cobrar_colaborador` é o chamador de disputa e já usa a lista correta (`chamados/services/cobranca.py:155`, `:163`, `:187`).

Exclusões identificadas pelo código: mensagens DP (`chamados/views.py:989`, `chamados/services/fio.py:726`, `chamados/services/acoes_chamado.py:310`), exceção autorizada (`chamados/services/acoes_chamado.py:72`), lote de regularização externa (`chamados/views_regularizacao.py:171`, `:178`), alertas de turno (`ponto/management/commands/processar_alertas_turno.py:267`), ausência/justificativa/cobertura (`ponto/services/ausencia.py:1088`, `ponto/services/justificativa.py:242`, `:310`, `ponto/services/cobertura.py:160`, `:171`). Não são envios causados por `DisputaSupervisao` nesses sítios. `push_supervisao`/`push_dp` têm outro destinatário, inclusive alerta agregado em `ponto/management/commands/alertar_disputas_pendentes.py:103`.

O texto falso de quantidade está em `chamados/services/acoes_disputa.py:232` (`len(orfaos)`, que conta chamados). Outro “Colaborador notificado” incondicional aparece em `:299`; ambos precisam depender do resultado real do envio, distinguindo agendamento de entrega.

## 14/11 18:21 · sessao ca67df423b4b · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Cura escrita: após a união dos períodos, soma acima do previsto retira todos os marcos do dia, zera os minutos e registra `pago_mais_trabalhado_passa_do_previsto`. Igualdade continua pagando (`ponto/services/efeito_ausencia.py:141–150`).

**Q1.** O previsto lavrado vem da ata (`ponto/services/cartorio.py:739`). Extraí a leitura existente da grade para `minutos_previstos_da_celula`: ata primeiro, inclusive zero; fallback em `minutos_previstos_do_dia`, sem cálculo paralelo (`escala/services/leitor_celula.py:354–366`). Grade e guarda compartilham essa leitura (`escala/services/leitor_celula.py:431`; `ponto/services/efeito_ausencia.py:145`). Isso evita chamar a grade inteira, que já lê os efeitos da justificativa (`escala/services/leitor_celula.py:379–380`).

O trabalhado usado na soma é `realizado_do_dia`, sobre todos os turnos do dia; pares fechados, pausas e E solta seguem a autoridade existente (`ponto/turnos.py:707–795`). `trabalhado_do_turno` permanece na DOBRA 2 (`ponto/services/efeito_ausencia.py:129`).

**Q2.** A fila anterior não consumia cobertura nem `conserva`. Acrescentei `justificativas_para_revisao`, reunindo recusa por soma, cobertura vazia da DOBRA e motivos de conservação (`ponto/services/efeito_ausencia.py:154–178`). A fila agora oferece **Revisar cobertura** e mostra o motivo (`ponto/views.py:1738–1743`; `templates/ponto/fila_justificativas.html:60,117`).

**Q3.** Não existe tolerância cadastrada **com juiz aplicável** encontrada: os campos existem (`core/models.py:59–62`), mas estão declarados sem efeito no cálculo (`core/regua_cct.py:72–75`). A tolerância de HE é constante (`ponto/motor_calculo_v2.py:51`). **TOLERANCIA-NA-JUSTIFICATIVA: sem construção**; +1 minuto recusa (`ponto/tests/test_justificativa_soma_sem_banco.py:104`).

O diff e seus escritores/leitores ficaram assim:

| Arquivo::função / campo | Escrita ou mudança | Leitores |
|---|---|---|
| `efeito_ausencia.py::periodos_justificados` — `cobre`, `minutos` | União e recusa integral: `ponto/services/efeito_ausencia.py:138–150` | Efeito financeiro: `:46–47`; grade: `escala/utils.py:1251–1276` |
| Mesmo serviço — `conserva`, `motivo` | Conservação em `:134`; motivo da soma em `:150` | Revisão em `:172–176`; fila em `ponto/views.py:1740`; template em `templates/ponto/fila_justificativas.html:117` |
| `leitor_celula.py::minutos_previstos_da_celula` — previsto | Extração da leitura: `escala/services/leitor_celula.py:354–366`; ata escrita por `ponto/services/cartorio.py:739` | Grade em `:431`; guarda em `ponto/services/efeito_ausencia.py:145` |
| Trabalhado/realizado | Autoridade existente: `ponto/turnos.py:707–795`; transporte à ata: `ponto/services/cartorio.py:740` | Guarda em `ponto/services/efeito_ausencia.py:147–148`; grade em `escala/utils.py:2064` |
| Testes | S1–S6, propriedade, fontes e fila: `ponto/tests/test_justificativa_soma_sem_banco.py:83–272`; adaptação das bordas v2: `ponto/tests/test_justificativa_periodo_v2_sem_banco.py:135–183` | Runner de mutantes: `ponto/tests/mutantes_justificativa_soma.py:73` |

O crédito continua chegando ao motor pelo mapa existente (`ponto/services/fechamento.py:755`; `ponto/services/espelho.py:1134`; `relatorios/pdf_espelho.py:407`), lido em `ponto/motor_calculo_v2.py:2628–2634`.

Resultados locais, para revisão pelo Code/suite e opus final: [nao verificado]

- **RED antes da cura:** 14 testes, 10 falhas de asserção — [saída preservada](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_red.txt:127). [nao verificado]
- **GREEN:** 32 testes sem banco — [saída](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_green.txt:35). [nao verificado]
- **Mutantes novos:** 16/16 mortos por asserção; o comando individual de cada um está na [saída](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_mutantes.txt:1).
- **Regressão v2:** 21/21 mutantes mortos — [saída](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_regressao_mutantes.txt:22).

Comandos do RED, GREEN e dos três mutantes exigidos, respectivamente (`ponto/tests/justificativa_soma_red.txt:127`; `ponto/tests/justificativa_soma_green.txt:35`; `ponto/tests/justificativa_soma_mutantes.txt:1–6`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco -v
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco ponto.tests.test_justificativa_periodo_v2_sem_banco -v
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma sem_guarda
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma limite_exclusivo
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma so_turno_da_ancora
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma --todos
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_periodo_v2 --todos
```

Três testes ORM foram escritos para a sessão executar; resultado **[nao verificado]** (`ponto/tests/test_justificativa_paga_o_periodo.py:325–369`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto.tests.test_justificativa_paga_o_periodo
```

Sem teto, desconto parcial ou tolerância nova na guarda (`ponto/services/efeito_ausencia.py:148–150`). Nenhum caminho proibido aparece entre os alterados; `git diff --check` passou (`ponto/tests/justificativa_soma_verificacao.txt:3,17`). Frota, DSR, TXT, banco produtivo e aplicação em competência: **[nao verificado]**.

`LEI-AKITA | origem=anexo justificativa_pela_soma.txt, Ronald 14/11 18:08 [nao verificado na copia] | testemunha=S1–S6 (ponto/tests/test_justificativa_soma_sem_banco.py:83–139) | RED=justificativa_soma_red.txt:129 | quem-mais-le=efeito/grade/fila (ponto/services/efeito_ausencia.py:46,172; escala/utils.py:1257; ponto/views.py:1740) | juizes novos=0 | padrao=Single Source of Truth / Aggregate Root (docs/GLOSSARIO.md:11; escala/services/leitor_celula.py:354–366)`

## 14/11 18:01 · sessao b161216b6cea · E3-V10-DEGRAU-DONO-E-PORTA · leitura · origem=Code

1. **No teste isolado de classe e, a leitura da v2 dá `fio='colab'` e `rota='porta_app'`.** A fixture cria pergunta sem resposta, sob chamado `resolvido`; sem decisões, a tarefa exige `responder/colaborador`. A porta do colaborador depende da pendência no app, sem exigir chamado na fila. ([test_e4_passivo.py:67](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:67), [tarefa_do_dia.py:142](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:142), [tarefa_do_dia.py:601](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:601), [tarefa_do_dia.py:625](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:625))

   Os relógios são diferentes:

   - A classe de testes congela **25/09/2026 às 12:00 UTC**, relógio sob o qual cria a disputa e a pergunta. `DisputaSupervisao.aberta_em` usa `auto_now_add`; `data_evento` da pergunta é explicitamente `DIA=01/09/2026`, não a data de criação. ([test_e4_passivo.py:26](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:26), [test_e4_passivo.py:34](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:34), [models.py:1803](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/models.py:1803), [test_e4_backfill.py:28](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_backfill.py:28))
   - `_rodar(aplicar=True)` congela **15/09/2026 às 12:00 UTC** durante o comando. O comando captura `timezone.localtime()` e, em apply, usa `agora.date()`, ignorando `--hoje`. Isso corresponde ao cabeçalho `15/09 09:00 −03` anexado. ([test_e4_backfill.py:98](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_backfill.py:98), [backfill_fio_morto_e4b.py:218](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:218))

   A pergunta criada sob o relógio de 25/09 continua carregada na consulta de 15/09: a carga seleciona perguntas por colaborador, `conversa=False` e `data_evento`, sem cortar pelo nascimento da disputa. O próprio contrato declara pergunta e identidade como estado atual. ([tarefa_do_dia.py:367](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:367), [tarefa_do_dia.py:392](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:392))

   **O predicado exato que o exclui do plano normal é `x[4] == 'sem_porta'`.** Com `porta_app`, o vigia contabiliza a porta e executa `continue`, antes de incrementar `fio_morto_alcance`; `_alcance` retém somente as linhas `sem_porta`. Portanto ele sai do subconjunto do backfill, não necessariamente de `alcance_universo`. O complemento de exportadas também não inclui competência cuja entrega seja `False`. ([vigia_fio.py:658](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:658), [vigia_fio.py:714](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:714), [backfill_fio_morto_e4b.py:76](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:76), [backfill_fio_morto_e4b.py:173](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:173))

   O mutante falha antes de testar a reabertura: ele espera erro contendo `ato=reabrir_credor`, mas a caracterização primeiro exige encontrar a linha do dia. ([test_e4_passivo.py:73](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:73), [test_e4_passivo.py:87](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:87))

2. **Os dois dias são 01/09 e 02/09/2026.** O primeiro é a célula-base, inicialmente sem pergunta, com entrada apagada e saída acesa. O segundo é `DIA+1`, com lâmpadas equivalentes e pergunta sem resposta sob chamado `resolvido`. ([test_e4_backfill.py:77](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_backfill.py:77), [test_e4_passivo.py:97](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:97))

   **Qual deles produziu `segue_sem_porta` na execução anexada: [não verificado].** O anexo fornece o agregado, sem as linhas `DIA` desse teste. O comando atribui `segue_sem_porta` quando o `cel_id` executado permanece no conjunto `sem_porta` do censo posterior; o agregado não identifica essa célula. ([backfill_fio_morto_e4b.py:368](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:368))

   Há uma incompatibilidade que a diferença de relógios, sozinha, **não explica**: pela leitura estática da fixture, a pergunta de 02/09 deve ter porta no app também em 25/09. C6 retorna viva pela célula `furo`; a partição do app considera pendente a pergunta sem resposta em disputa aberta; sem decisões, a tarefa exige responder e publica a porta. ([juizes.py:1448](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/juizes.py:1448), [pergunta_no_app.py:78](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/pergunta_no_app.py:78), [tarefa_do_dia.py:142](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:142), [tarefa_do_dia.py:601](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:601))

   Assim, não consigo derivar dessa cópia o contador inicial **2**, nem atribuir causalmente o residual a 02/09. O teste exige esse contador em 25/09 e depois compara o plano com ele; o estado adicional que faria essa sequência produzir o agregado anexado é **[não verificado]**. ([test_e4_passivo.py:110](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:110))

3. **Usam o mesmo juiz de porta, mas não a mesma carga nem sempre o mesmo instante.** O vigia abre uma carga em lote e lê `porta_da_tarefa`. O backfill obtém seu universo chamando esse vigia com o `agora` da borda; depois, para escolher atos, chama `tarefa_do_dia` sem fornecer carga, abrindo outra carga, individual. ([vigia_fio.py:647](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:647), [backfill_fio_morto_e4b.py:78](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:78), [backfill_fio_morto_e4b.py:87](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:87), [tarefa_do_dia.py:650](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:650))

   O censo posterior recebe **novo** `timezone.localtime()`, para enxergar os atos escritos. No teste misto, o censo externo usa 25/09, enquanto o comando roda em 15/09. ([backfill_fio_morto_e4b.py:350](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:350), [test_e4_passivo.py:110](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:110), [test_e4_backfill.py:103](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_backfill.py:103))

4. **A caracterização isolada “sem resposta sob terminal ⇒ precisa reabrir para ter alcance” perdeu essa premissa na v2:** quando a tarefa exige responder e a pergunta está pendente no app, já existe dono `colab` e porta `porta_app`. O vigia aceita essa porta antes de classificar qualquer causa e. ([tarefa_do_dia.py:577](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:577), [tarefa_do_dia.py:601](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:601), [vigia_fio.py:658](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:658))

   O ramo `e/pergunta_viva_sob_chamado_terminal` continua no código, mas só é alcançado depois de faltar porta e de não haver outro dono pelas leis consultadas. Sua presença no código não demonstra ocorrência no dado fornecido. ([vigia_fio.py:670](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:670), [vigia_fio.py:698](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:698))

   **Existe uma segunda avaliação no backfill para escolher reabertura.** Além de ler a tarefa, `ato_do_dia` percorre as perguntas e consulta `reabrir_credor_de_pergunta_viva`, independentemente das ações publicadas. Esse helper exige ausência de resposta, C6 vivo e âncora fora de `VIVOS`; não consulta `pendentes_app` e não recebe `agora`. Portanto são critérios diferentes: a tarefa pergunta por alcance atual; o helper pergunta pela elegibilidade de reabrir a âncora. Ambos usam C6, mas o helper o chama sem o horizonte temporal da tarefa. ([backfill_fio_morto_e4b.py:87](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:87), [disputa_emissao.py:1812](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/disputa_emissao.py:1812), [disputa_emissao.py:1828](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/disputa_emissao.py:1828))

Quanto aos campos, são resultados e variáveis locais nesses caminhos:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `fio`, `rota` | `_portas_da_tarefa`, no dicionário retornado. ([tarefa_do_dia.py:626](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:626)) | `porta_da_tarefa`; vigia conta rota e publica fio como papel. ([tarefa_do_dia.py:636](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:636), [vigia_fio.py:662](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:662)) |
| `fio_morto_alcance` | Vigia inicializa em zero e incrementa no caminho final sem porta. ([vigia_fio.py:578](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:578), [vigia_fio.py:714](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:714)) | Backfill lê antes/depois; teste compara ao tamanho do plano. ([backfill_fio_morto_e4b.py:239](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:239), [backfill_fio_morto_e4b.py:415](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:415), [test_e4_passivo.py:114](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:114)) |
| `plano` | Comando cria a lista, acrescenta os dias e atribui resultados após execução/medição. ([backfill_fio_morto_e4b.py:251](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:251), [backfill_fio_morto_e4b.py:371](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:371)) | O comando executa/lista os itens e imprime `len(plano)` no resumo. ([backfill_fio_morto_e4b.py:262](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:262), [backfill_fio_morto_e4b.py:409](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:409)) |
| `partes` | `_resumo` do teste interpreta o texto `RESUMO`, remove `plano` e converte contagens em inteiros. O comando produz esse texto contando `p['res']`. ([test_e4_passivo.py:43](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:43), [backfill_fio_morto_e4b.py:405](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:405)) | O teste verifica soma e distribuição esperada. ([test_e4_passivo.py:113](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:113)) |

## 14/11 18:00 · sessao 37c906b22221 · - · - · origem=Ronald (tmux)

O vigia lê a porta publicada pela tarefa, sem refazer o predicado: abre `carga_da_tarefa(..., agora)`, chama `.tarefa` e `porta_da_tarefa` em `app/chamados/services/vigia_fio.py:647`, `:648`, `:649`. A alimentação anterior `alimentar_portas(..., agora=agora)` serve às causas do sem porta; é outra carga, embora receba o mesmo instante (`vigia_fio.py:611`, `:615`, `:621`).

Com porta, o vigia incrementa `alcance_<rota>`, registra papel=`porta['fio']` e encerra o processamento daquele dia (`vigia_fio.py:658`, `:662`, `:664`, `:665`). Sem porta, só depois dos baldes de lei calcula a causa e incrementa `fio_morto_alcance` (`vigia_fio.py:670`, `:672`, `:679`, `:714`). O detalhe `pergunta_viva_sob_chamado_terminal` é atribuído apenas nesse ramo já sem porta (`vigia_fio.py:706`, `:707`).

Na v2, a tarefa escreve `fio` e `rota` em `_portas_da_tarefa`; pergunta pendente no app com ação `responder/colaborador` dá dono `colab`, independentemente de âncora e prazo (`app/chamados/services/tarefa_do_dia.py:601`, `:602`, `:607`, `:608`, `:613`). Sem chamado vivo com dono, publica `porta_app` (`tarefa_do_dia.py:619`, `:624`, `:625`); a escrita final dos dois campos está em `:626`. `porta_da_tarefa` apenas copia campos da tarefa, retornando `None` quando não há rota (`:629`, `:636`). A matriz tem um caso explícito de só app → `(colab, porta_app, (), {})` (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:71`, `:73`).

O backfill usa o próprio vigia e filtra literalmente `x[4] == 'sem_porta'` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`, `:78`, `:79`). Assim, uma pergunta sem resposta sob terminal que publique `porta_app` deixa o conjunto de reparo, embora permaneça no universo de dias retidos (`vigia_fio.py:599`, `:658`).

Há outra seleção de elegibilidade no backfill: `ato_do_dia` lê `tarefa_do_dia(colab, dia, agora)` (`backfill_fio_morto_e4b.py:87`), mas também chama `reabrir_credor_de_pergunta_viva(p, hoje=hoje, executar=False)` para todas as perguntas do dia (`:89`, `:90`, `:96`). Esse helper não recebe `agora`, não consulta `porta_da_tarefa` nem pendentes do app: testa resposta crua, C6 sem instante, âncora e status fora de VIVOS (`app/chamados/services/disputa_emissao.py:1812`, `:1828`, `:1830`, `:1834`). Portanto há a mesma autoridade de **porta** no vigia/tarefa, e uma seleção adicional de **reabertura de credor** com horizonte diferente no backfill (mesmas linhas).

A hipótese de que simplesmente mudar 15/09 para 25/09 retire `porta_app` da mesma pergunta **não foi demonstrada**. Pelo código visível, pergunta sem resposta, disputa aberta e célula `furo` é viva em ambos os instantes (`app/chamados/juizes.py:1447`, `:1450`, `:1451`); a partição do app exige apenas `respondida_em is None` e `disputa.fechada_em is None` (`app/chamados/services/pergunta_no_app.py:78`, `:82`, `:83`); sem decisões E1 a tarefa exige responder (`tarefa_do_dia.py:142`, `:143`). A carga declara expressamente que lê a pergunta/identidade atual e não recorta sua criação (`tarefa_do_dia.py:367`, `:371`, `:372`). A explicação exata do `v0=2` naquele anexo depende de outro estado/efeito ainda não identificado. [não verificado]

Escritores e leitores:

- `fio`, `rota`: escritos no resultado transitório pela tarefa (`tarefa_do_dia.py:626`); copiados por `porta_da_tarefa` (`:636`); lidos pelo vigia (`vigia_fio.py:662`, `:663`, `:664`).
- `fio_morto_alcance`: inicializado e incrementado pelo vigia (`vigia_fio.py:578`, `:714`); lido pelo backfill (`backfill_fio_morto_e4b.py:239`) e pelo teste (`app/chamados/tests/test_e4_passivo.py:111`, `:114`).
- `plano`: criado/apendido pelo comando (`backfill_fio_morto_e4b.py:251`, `:257`); `RESUMO plano` imprime `len(plano)` (`:409`).
- `partes`: o comando escreve o agregado `conta` por prefixo de resultado (`backfill_fio_morto_e4b.py:405`, `:407`, `:408`, `:409`); o teste lê a linha RESUMO, retira `plano` e chama o restante de partes (`test_e4_passivo.py:43`, `:46`, `:47`, `:113`, `:116`). [nao verificado]

## 14/11 17:59 · sessao c82c8fd8b7e8 · - · - · origem=Ronald (tmux)

Achados de fixture, todos só por leitura:

- `DIA = 2026-09-01`; `HOJE = 2026-09-25`; `AGORA_E4B = 2026-09-25 12:00 UTC`. Base cria competência 09/2026 aberta e uma célula de trabalho em `DIA`, `furo`, E 07:00 apagada/S 16:00 acesa. `app/chamados/tests/test_e4_backfill.py:27`, `:28`, `:30`, `:63`, `:77`.
- `PassivoE4bTest` inteiro fica congelado em 25/09 12:00; classe e cria chamado `resolvido`, sem célula, e pergunta sem resposta, motivo `saida_sem_entrada`, data 01/09, vinculada à célula-base e ao chamado terminal. `app/chamados/tests/test_e4_passivo.py:26`, `:29`, `:34`, `:67`.
- Disputa ganha `aberta_em` automaticamente; pergunta não tem `criada_em`. O horizonte da carga declara expressamente que pergunta/identidade/ata/status/fila são estado atual, não reconstrução histórica. `app/chamados/models.py:1803`; `app/chamados/services/tarefa_do_dia.py:367`.
- `_rodar` coloca `hoje=25/09` por padrão, mas quando `aplicar=True` congela apenas a execução do comando em 15/09 12:00 UTC. O comando usa `timezone.localtime()` e, em aplicar, ignora `--hoje` para obter `hoje=agora.date()`. Portanto cabeçalho 15/09 09:00 −03 do anexo é compatível. `app/chamados/tests/test_e4_backfill.py:98`, `:100`, `:103`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:218`.
- A exceção de classe c ganha congelamento próprio em 15/09 para resposta e apply coincidirem. Classe e e classes universos não têm esse congelamento próprio. `app/chamados/tests/test_e4_passivo.py:49`, `:51`, `:67`, `:93`.

Dois dias em `test_classes_e_universos_nao_somem_para_zerar`:

1. **01/09:** célula-base de fábrica, sem pergunta inicial, E apagada/S acesa. `app/chamados/tests/test_e4_backfill.py:77`.
2. **02/09:** `DIA+1`, célula análoga, pergunta sem resposta sob chamado resolvido, criada na montagem do teste. `app/chamados/tests/test_e4_passivo.py:97`, `:99`, `:101`, `:105`, `:107`.

O teste chama `censo_alcance` explicitamente em 25/09 12:00 UTC, exige `fio_morto_alcance=2`, depois chama `_rodar(aplicar=True)` em 15/09 e exige plano igual ao contador, soma de partes igual ao plano, e `{'resolvido':2}`. `app/chamados/tests/test_e4_passivo.py:110`, `:111`, `:112`, `:114`, `:115`, `:116`.

**Limite material encontrado:** não consigo derivar da cópia o sucesso de `v0['fio_morto_alcance']==2` no instante 25/09. A pergunta de 02/09 aparenta obter `responder/colaborador` e `porta_app` tanto em 15/09 quanto em 25/09: sem decisões, `_pela_decisao` retorna responder; C6 retorna viva por célula `furo`; consulta do app inclui pergunta e a partição pendente pede apenas resposta ausente e disputa aberta; `_portas_da_tarefa` dá dono colab/porta_app sem prazo. `app/chamados/services/tarefa_do_dia.py:142`, `:601`, `:607`, `:625`; `app/chamados/juizes.py:1448`; `app/chamados/services/pergunta_no_app.py:54`, `:63`, `:82`.

Não existe filtro por `Disputa.aberta_em` na seleção da tarefa nem na consulta do app; portanto o nascimento da disputa em 25/09 não impede sua leitura no horizonte de 15/09. `app/chamados/services/tarefa_do_dia.py:392`; `app/chamados/services/pergunta_no_app.py:54`.

Assim, **identificar categoricamente qual dia produziu `segue_sem_porta` na execução anexada exige linhas `DIA` ou estado não fornecidos — [não verificado]**. O anexo só dá o agregado e não mostra essas linhas. A hipótese “02/09 ficou sem porta” não fica sustentada apenas pelo fluxo visível da cópia. O `segue_sem_porta` é calculado comparando `cel_id` ao conjunto de linhas sem porta do censo posterior. `app/chamados/management/commands/backfill_fio_morto_e4b.py:352`, `:368`, `:371`.

`partes` é variável só do teste: `_resumo` lê a linha textual `RESUMO`, separa `plano` e converte contagens para inteiros. O comando produz as contagens a partir de `p['res']`, normalizando sufixos, e imprime `len(plano)`. `app/chamados/tests/test_e4_passivo.py:43`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:405`.

## 14/11 17:59 · sessao ffca45bbba11 · - · - · origem=Ronald (tmux)

- O relógio do comando é `agora = timezone.localtime()` uma vez na borda; em `--aplicar`, `hoje=agora.date()` mesmo que venha `--hoje`. O `_rodar` de `BaseE4b` injeta freeze em **15/09/2026 12:00 UTC** para apply; fora dele, `PassivoE4bTest` fica congelado em **25/09/2026 12:00 UTC**, inclusive na criação de pergunta/disputa. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:218`; `:220`; `app/chamados/tests/test_e4_backfill.py:94`; `:103`; `app/chamados/tests/test_e4_passivo.py:26`; `:34`)

- `_alcance` chama `censo_alcance` passando o mesmo `agora`, depois seleciona **apenas** `x[4] == 'sem_porta'`; esse é o filtro que tira um dia com `porta_app` do plano normal. `handle` soma apenas o complemento entregue/indeterminado da classe e, filtra competência e então constrói o plano. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`; `:79`; `:237`; `:250`; `:257`)

- O complemento `_exportadas_classe_e` não resgata o dia de competência aberta: ele dá `continue` quando `competencia_do_dia_entregue(...) is False`. Só inclui entregues/indeterminadas entre hoje−45 e hoje cuja porta de reabertura planeje algo. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:166`; `:173`; `:175`; `:178`)

- O censo consulta a tarefa **em lote**, abrindo `carga_da_tarefa(..., agora)` e produzindo tarefas/portas antes do laço. Ao encontrar porta, soma sua rota e lista seu fio, sem chegar à classificação e nem a `fio_morto_alcance`. (`app/chamados/services/vigia_fio.py:647`; `:648`; `:649`; `:658`; `:662`; `:665`; `:714`)

- O backfill não reutiliza a carga do censo: `ato_do_dia` chama `T.tarefa_do_dia(colab,dia,agora)` sem `carga`, portanto a casca abre uma carga individual nova. Na execução, repete `ato_do_dia` sob a trava. O censo posterior ganha outro instante, explicitamente `timezone.localtime()`, para enxergar fatos escritos pelos atos. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:87`; `:309`; `:350`; `:352`; `app/chamados/services/tarefa_do_dia.py:650`; `:654`)

- Há uma consulta adicional de elegibilidade no backfill: ele percorre todas as perguntas atuais do colaborador/data e chama `reabrir_credor_de_pergunta_viva(p,hoje=hoje,executar=False)` **independentemente das ações retornadas por tarefa_do_dia**. O helper exige `respondida_em is None`, `pergunta_viva(p,hoje=hoje)` e âncora existente fora de `VIVOS`; não recebe `agora`. Assim, o ato “reabrir” não nasce exclusivamente da tarefa, embora o universo normal venha da porta dela. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:89`; `:90`; `:96`; `app/chamados/services/disputa_emissao.py:1828`; `:1830`; `:1834`)

- A reabertura efetiva passa por `credor.reabrir()` e anexa nota; recusa também anexa nota. (`app/chamados/services/disputa_emissao.py:1858`; `:1859`; `:1864`)

- O horizonte da tarefa é parcial: pergunta/identidade e âncora são estado atual, sem corte por nascimento; decisões são cortadas por `ato_em`, passivo pelo instante da projeção e efeitos por `Batida.criado_em`. Portanto pergunta criada no freeze de 25/09 pode existir na carga julgada em 15/09. (`app/chamados/services/tarefa_do_dia.py:367`; `:372`; `:392`; `:409`; `:416`; `:428`)

- App: `consultas_do_app` passa `agora` ao C6, mas `_particao` lê diretamente `respondida_em` e `disputa.fechada_em` atuais para pendentes/histórico. Não projeta esses dois campos para a partição. (`app/chamados/services/pergunta_no_app.py:63`; `:78`; `:82`; `:89`; `:96`; `:98`)

- Escritores/leitores:
  - `fio` e `rota`: escritos no dicionário por `_portas_da_tarefa`; `porta_da_tarefa` lê e devolve se há rota; censo lê para contagens/listagem. (`app/chamados/services/tarefa_do_dia.py:626`; `:636`; `app/chamados/services/vigia_fio.py:662`; `:664`)
  - `fio_morto_alcance`: inicializado em zero e incrementado somente ao fim do caminho sem porta/sem balde; backfill lê antes/depois. (`app/chamados/services/vigia_fio.py:578`; `:714`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:239`; `:415`)
  - `plano`: lista local criada e preenchida pelo comando; atos/pids são revalidados; resultado é `SEGUE` se o `cel_id` ainda está no sem-porta medido depois, senão `RESOLVIDO`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:251`; `:257`; `:310`; `:368`; `:371`)
  - `partes`: só existe como dicionário local do teste, derivado da linha `RESUMO`; o comando imprime essa linha contando `p['res']`. (`app/chamados/tests/test_e4_passivo.py:43`; `:46`; `:47`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:405`; `:407`; `:409`)

Não consegui fechar estaticamente qual dos dois dias do teste misto permaneceu em `segue_sem_porta`; a saída fornecida só traz as contagens, sem linhas `DIA`. [não verificado] [nao verificado]

## 14/11 17:46 · sessao 68b2829022d2 · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

lei: falta regra escrita que determine o abono quando o período justificado 13h–18h30 se sobrepõe ao trabalho 08h–17h; L-088/L-089 não respondem essa conta, e o corte define o marco coberto, sem definir o pagamento dessa interseção (`docs/LEIS.md:120–121`; `docs/CORTES.md:342`).

**(a) Origem, escritor e intervalo**

O escritor do valor é `periodos_justificados`: consulta justificativas aprovadas por `data_turno`, reúne seus intervalos e mede a união. `efeitos_da_ausencia` monta `min_justificados` e remove o parcial quando existe Ausencia ABONA inteira (`ponto/services/efeito_ausencia.py:99–102`, `137–154`, `47`, `63`).

O intervalo depende do marco:

- `esqueci_saida`: de `max(ponta_real, hi)` até `hf`; a ponta considera a última batida do turno e eventual `saida_informada` (`ponto/turnos.py:2737–2738`; `ponto/services/efeito_ausencia.py:197–202`).
- `esqueci_entrada`: de `hi` até `min(primeira_batida, hf)` (`ponto/turnos.py:2739`; `ponto/services/efeito_ausencia.py:203–204`).

Desconta pausas previstas e une intervalos sobrepostos de justificativas. **Não subtrai o trabalho do período.** Antes da medição, um filtro retira o marco inteiro se seu trecho cruza algum par fechado identificado; portanto, rejeição de cobertura e desconto de minutos trabalhados são comportamentos distintos (`ponto/turnos.py:2751–2766`, `2792–2805`; `ponto/services/efeito_ausencia.py:205–206`, `144–154`).

A grade transporta o mapa para a linha; `por_dia_da_grade` soma esse valor sem outra medição (`escala/services/leitor_celula.py:365`, `435–436`; `ponto/services/dia_pago.py:398–399`).

Para **col82 em 31/10** e **col592 em 10/09**, os totais fornecidos não identificam início/fim do período nem por que o filtro anterior permitiu crédito. Reprodução individual: **[nao verificado]**. Para decidir, faltam, em cada dia: justificativas aprovadas, tipo, âncora, `saida_informada`, marcos/pausas do vínculo, turnos pareados com papéis e timestamps, além da ata que forneceu realizado/previsto (`ponto/services/efeito_ausencia.py:99–129`, `172–205`; `escala/services/leitor_celula.py:385`, `403–429`).

**(b) Motor versus gravado**

No ramo com dias previstos informados, o motor soma integralmente `_mj[d] / 60` para os dias obrigatórios justificados. Não desconta realizado nessa soma (`ponto/motor_calculo_v2.py:2628–2634`).

Assim, **com o mesmo mapa e o mesmo enquadramento do dia, motor e gravado recebem o mesmo abono parcial**, inclusive quando há trabalho: o motor soma `_mj[d]`; a grade soma `min_justificados`. A igualdade efetiva dos resultados completos nos 41 dias: **[nao verificado]** (`ponto/motor_calculo_v2.py:2633–2634`; `ponto/services/dia_pago.py:398–399`).

A obra manda ambos seguirem **`efeito_ausencia`**, não escolher um resultado divergente como autoridade. O fechamento injeta esse mapa no motor e grava o abono retornado pela grade (`docs/CORTES.md:375`; `ponto/services/fechamento.py:665–667`, `755`, `1066–1067`).

A divergência A2 do anexo é outro ramo: Ausencia inteira com trabalho. Ela não demonstra divergência nestes casos de crédito parcial (`ponto/services/dia_pago.py:396–399`; `ponto/motor_calculo_v2.py:2611–2614`).

**(c) Caso antes de qualquer código**

**CASO:** período justificado 13h–18h30; trabalho 08h–17h. **Resposta legal numérica: [nao verificado].** As regras encontradas não determinam se deve haver subtração da interseção ou outro tratamento (`docs/LEIS.md:120–121`; `docs/CORTES.md:342`, `352`, `375`).

Se esses intervalos forem apresentados diretamente ao filtro atual, ele rejeita o marco inteiro por cruzamento. Isso descreve o código existente; não fornece fundamento para instituir outra conta (`ponto/turnos.py:2762–2766`).

**(d) Classificação dos 41**

Pela função medida, os 41 pertencem ao ramo **trabalho com crédito justificado positivo**: no ramo `ausencia`, abono é `previsto − realizado_capado`, logo sua soma com realizado não produziria o excesso observado. A grade emite uma linha por data (`ponto/services/dia_pago.py:394–404`; `escala/services/leitor_celula.py:383`, `451–452`).

Os totais `pv/abono/real` **não distinguem a causa temporal dos 41**. Não permitem atribuí-los individualmente a pausa fora de ordem ou sobreposição com trabalho. A classificação causal permanece **[nao verificado]**.

O encadeamento realmente desloca para D+1 um marco que não sucede o anterior; isso comprova o mecanismo possível, não sua ocorrência nesses registros (`escala/utils.py:345–347`). A indicação de sobreposição para col82 veio da triagem fornecida; sua reconstrução pelos instantes permanece **[nao verificado]**.

**Censo de escritores e leitores**

| Papel | Arquivo:linha |
|---|---|
| Calcula período e escreve mapa | `ponto/services/efeito_ausencia.py:140`, `47`, `68` |
| Escreve `min_justificados` nas grades | `escala/utils.py:1276`; `escala/services/leitor_celula.py:436` |
| Lê crédito na grade | `ponto/services/dia_pago.py:398–399` |
| Alimenta motor | `ponto/services/fechamento.py:755`, `831`; `ponto/services/espelho.py:1134`; `relatorios/pdf_espelho.py:407` |
| Alimenta caminhos de comparação | `ponto/management/commands/diff_espelho_alimentacao.py:56`, `124`; `relatorios/management/commands/diff_extrato_parcial.py:62`; `ponto/services/esmeril_espelho.py:166` |
| Escreve `FechamentoMensal.minutos_abonados` | `ponto/services/fechamento.py:1067` |
| Escreve abono em DiaPago: lavratura/retrato | `ponto/services/dia_pago.py:288–292`, `479`, `484` |
| Lê abono: prontidão, conferência, cartão e contador | `folha/export.py:1078`, `1104`; `folha/porta_export.py:139`; `relatorios/cartao_pela_celula.py:118`, `186`; `ponto/services/dia_pago.py:702` |

Comando do censo executado:

```bash
git grep -n -E 'min_justificados|minutos_justificados|minutos_abonados|por_dia_da_grade' \
  -- ponto escala folha relatorios ':!**/models.py' ':!**/migrations/**'
```

Vizinhos identificados: transporte e cálculo parcial (`ponto/tests/test_justificativa_periodo_v2_sem_banco.py:196`, `285`, `307`), percurso ORM até fechamento/DiaPago (`ponto/tests/test_justificativa_paga_o_periodo.py:395–408`), realizado da grade (`escala/tests/test_montador_realizado_pela_autoridade.py:479`), conferência da folha (`folha/tests/test_gravado_discorda_da_propria_grade.py:50–58`) e leitores do cartão (`relatorios/cartao_pela_celula.py:118`, `186`).

Executei **15 testes existentes, todos passando**, com: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  ponto.tests.test_justificativa_periodo_v2_sem_banco -v
```

Esse conjunto declara que não prova ORM, frota, DSR produtivo nem hashes TXT (`ponto/tests/test_justificativa_periodo_v2_sem_banco.py:6–8`).

**Diff por arquivo::função: vazio.** `git diff --stat` e `git status --short` terminaram sem saída. Não construí cura, RED novo nem mutantes novos porque a condição (c) não foi satisfeita. Não executei ORM, banco ou nova medição de frota; resultados desses percursos permanecem **[nao verificado]**. A certificação pelo Code e pelo opus final permanece pendente, conforme `docs/LEIS.md:147`.

## 14/11 17:43 · sessao b1bc8dd9fd69 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura entregue na cópia. A lista dos oito arquivos tocados, motivo por item, censo e comandos está na [entrega c24](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/docs/BO_DISPUTA_C24_ENTREGA.txt:6).

- **Catraca:** o `IfExp` contaminava `_bo` e, transitivamente, `_linhas_do_fato`. A cura usa `if/else` somente no hunk P3, preservando `None` explícito e a entrega integral do universo à canônica. A catraca permanece intacta. [models.py:2923](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/models.py:2923), [explicação:26](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/docs/BO_DISPUTA_C24_ENTREGA.txt:26).
- **(a)** A mudança relevante da c22 consulta a ata **antes** do classificador silencioso no ramo `sem_origem`. Isso marca a abstenção e impede o autofecho. [disputa_emissao.py:1465](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/services/disputa_emissao.py:1465).
- **(b)** A fixture original não lavra ata nem cria batida-origem; cria vínculo, chamado e disputa vazia. [test_b2d_resposta_visivel.py:29](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/tests/test_b2d_resposta_visivel.py:29).
- **(c)** Para essa fixture original, a resposta correta é **acender e esperar**. Os casos antigos ganharam ata pelas portas reais; o caso sem ata ganhou teste próprio, mantendo abertura, luz e silêncio em duas passadas. [regra:1532](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/services/disputa_emissao.py:1532), [fixture e contraste:16](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/tests/test_hx_disputa_muda.py:16). [nao verificado]
- **(d)** Sem nenhuma pergunta, não pode notificar. O envio observado é da **principal**, não de `complementadas`: esse dicionário exige pergunta anterior e complemento admitido. A guarda nova exige pergunta na própria disputa. [inclusão:1295](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/services/disputa_emissao.py:1295), [push:1552](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/services/disputa_emissao.py:1552). [nao verificado]

O [censo completo](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/docs/BO_DISPUTA_C24_ENTREGA.txt:90) registra todas as escritas e leituras das três variáveis. Os [seis chamadores produtivos](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/docs/BO_DISPUTA_C24_ENTREGA.txt:112) passam a suprimir o push da principal vazia quando a notificação está habilitada; retorno e autofecho conservam seus caminhos. [nao verificado]

Resultados locais: cinco testes c24 passaram; a base reproduziu três falhas por asserção; os cinco mutantes novos morreram, sem errors. A catraca original executada em memória passou oito testes, e as regressões passaram 8 + 12 + 6. Cada comando está [registrado na entrega](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/docs/BO_DISPUTA_C24_ENTREGA.txt:130). [nao verificado]

Certificação pela suíte real, ORM e execução da fixture nova: **[nao verificado]**. [nao verificado]

## 14/11 17:38 · sessao 190a94933cb1 · - · - · origem=Ronald (tmux)

Não encontrei defeito concreto que exija ajuste na fixture atual; execução ORM continua [nao verificado].

- O isolamento intercepta o nome importado dinamicamente pelo cartório e permanece ativo durante o `post_save` do órfão (`chamados/tests/test_hx_disputa_muda.py:28`, `ponto/services/cartorio.py:1033`, `:1045`, `chamados/signals.py:190-218`).
- O piso impede que o julgamento de 20/05 espelhe marcos ou abra cobranças pela emissão automática (`ponto/services/cartorio.py:298`, `:340-354`, `:1727`, `:1813`). A entrada real mantém a premissa acusadora; a guarda de nascimento só absolve diante de veredito conhecido fora de `VEREDITOS_ACUSAM` (`chamados/reconciliador.py:1027-1031`).
- O preparo exige resposta real `lido`; o novo caso exige `sem_celula` e origem ausente antes do repasse, depois verifica duas passadas abertas, sem pergunta, com luz e sem push (`chamados/tests/test_hx_disputa_muda.py:34-35`, `:80-94`).

Precisão para a entrega: esses dois casos não variam apenas a ata. O preparo com ata também cria batida-origem; o caso sem ata mantém origem ausente (`chamados/tests/test_hx_disputa_muda.py:29-31`, `:81`). Assim exercitam os dois ramos silenciosos distintos (`chamados/services/disputa_emissao.py:1420-1429`, `:1459-1471`).

## 14/11 17:31 · sessao 190a94933cb1 · - · - · origem=Ronald (tmux)

Censo por leitura da cópia; execução ORM e efeitos reais de signals: [nao verificado].

| Variável | Escrita | Leitura |
|---|---|---|
| `_absteve_por_ata` | Inicializada em `chamados/services/disputa_emissao.py:1289`; marcada nos complementos `:1304`, caminho de ata `:1392`, batida com classificador silencioso `:1428`, batida classificada `:1438` e sem origem `:1467`. | Veto de auto-fecho em `:1540`; acende em `:1542`, preservando a disputa aberta. |
| `complementadas` | Criada em `:1277`; única inclusão em `:1310`, seguida de atualização de motivos em `:1311`. Exige pergunta anterior `:1295`, ata disponível `:1302` e complemento admitido `:1307`. | Percorrida em `notificar_complementos`, `:1280`; push em `:1283`. Chamadas em `:1320`, `:1527`, `:1531`; esta última exclui a principal pela condição `:1281`. | [nao verificado]
| `motivos_atualizados` | Inicializada pelos motivos da principal em `:1327-1328`; recebe resultados de pergunta/complemento em `:1373`, `:1394`, `:1440`, `:1446`, `:1472`, `:1477`; seed `orfao_14h` em `:1486`. | Criação da disputa `:1354`; agregação `:1360`, `:1380`, `:1487`; insumo das mesmas portas de pergunta/complemento; entradas pendentes `:1501`; condição do push `:1550` e argumentos em `:1552`. Persistência dos motivos pertence a `_agregar_motivos_na_dona`, `:587`, `:601`, `:607`. | [nao verificado]

A fixture HX **não entra em `complementadas`**: o preparo cria disputa vazia com `motivos=['orfao_14h']`, sem perguntas (`chamados/tests/test_b2d_resposta_visivel.py:44-50`); a inclusão no dicionário exige pergunta anterior e complemento admitido (`chamados/services/disputa_emissao.py:1295`, `:1307`, `:1310`). O push `nova=False` do anexo corresponde ao push principal em `:1552`, após selecionar a disputa aberta existente em `:1323-1325` e copiar seus motivos em `:1327-1328`.

Também não há batida-origem criada pelo preparo (`chamados/tests/test_b2d_resposta_visivel.py:29-50`). Sem candidatas, `_achar_batida_origem` devolve `None` (`chamados/services/materializacao.py:140`); portanto a reordenação da c22 que alcança esse cenário é a consulta à ata antes do classificador no ramo sem origem (`chamados/services/disputa_emissao.py:1465-1471`). O ramo silencioso de batida existe separadamente em `:1420-1429`.

Os seis chamadores produtivos encontrados são:

- `ponto/management/commands/processar_alertas_turno.py:385`.
- `ponto/management/commands/processar_alertas_avancados.py:159`.
- `ponto/management/commands/detectar_ausencias.py:803`.
- `chamados/services/acoes_disputa.py:222`.
- `chamados/services/cobrar_dia.py:49`.
- `chamados/services/pergunta_no_app.py:246`, cujo parâmetro padrão é `notificar=False` em `:231`.

Os quatro primeiros importam pela reexportação de `chamados/models.py:4151`; os dois últimos importam diretamente do emissor (`cobrar_dia.py:20`, `pergunta_no_app.py:238`).

Adicionar existência de pergunta apenas à condição do push principal suprime, para esses chamadores, a notificação da principal aberta e vazia que passa pela condição atual (`chamados/services/disputa_emissao.py:1550-1552`). O retorno e a decisão de auto-fecho ficam nas linhas anteriores (`:1539-1549`); complementos continuam pelo caminho próprio (`:1279-1283`).

Há dois contratos de teste anteriores incompatíveis com essa supressão: ambos exigem push após afirmar zero perguntas sem ata (`chamados/tests/test_bo_disputa_volta5b_orfaos.py:191-199`, `chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:187-193`). A resposta textual administrativa também afirma “Colaborador notificado” incondicionalmente após retorno de disputa (`chamados/services/acoes_disputa.py:231-232`); o efeito real dessa mensagem na execução corrigida é [nao verificado].

_5216 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
