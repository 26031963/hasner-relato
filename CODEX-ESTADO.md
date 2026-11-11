# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 10:16** · publicado em **11/11/2026 10:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 10:16 · sessao ee78a9807660 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos apenas da cópia:

- **Sonda retrospectiva do tamanho do lote já tem fonte no código:** `validou_lote_tela` registra horário da auditoria, quantidade bruta de refs, validadas e recusadas; o registro ocorre depois do serviço. Existência e cobertura dessas linhas em prod são [nao verificado]. (`app/chamados/views_cobrar.py:380-385`; `app/core/models.py:295-307`; auditoria best-effort: `app/core/auditoria.py:17-28`, `app/core/observ.py:105-138`)
- **N bruto difere de N processado:** refs são convertidas para int e deduplicadas; cada ref restante acaba validada ou recusada. Logo, `distintos_processados = validadas + recusadas`; `nao_gravados` já integra validadas e não deve ser somado novamente. (`app/chamados/services/validacao.py:713-727`, `:729-761`)
- **Sonda sugerida, sem nomes:** extrair somente `criado_em` e os três grupos numéricos do sufixo `: N perguntas pedidas, N validadas, N recusadas`, filtrando `acao='validou_lote_tela'`; não projetar descrição, usuário ou IP. Correspondência do timestamp do access log com início/fim da requisição permanece [nao verificado]. (`app/chamados/views_cobrar.py:383-385`; `app/core/models.py:295-307`)

```sql
-- Confirmar schema/tabela da instalação: [nao verificado].
SELECT criado_em,
       m[1]::int AS solicitados_brutos,
       m[2]::int AS validadas,
       m[3]::int AS recusadas,
       m[2]::int + m[3]::int AS distintos_processados
FROM (
  SELECT criado_em,
         regexp_match(
           descricao,
           ': ([0-9]+) perguntas pedidas, ([0-9]+) validadas, ([0-9]+) recusadas$'
         ) AS m
  FROM core_logauditoria
  WHERE acao = 'validou_lote_tela'
    AND criado_em >= TIMESTAMPTZ '2026-10-28 00:00:00-03'
    AND criado_em <  TIMESTAMPTZ '2026-11-07 00:00:00-03'
) a
ORDER BY criado_em;
```

Cron e concorrência:

- Há programação de `alertar_disputas_pendentes` às 08:00, `gerar_alertas` às 08:00/18:00 e `suporte_hasner` às 08:05. Há tarefas contínuas de 1, 5, 10, 15 e 30 minutos também às 16:xx. Estarem instaladas, executando e disputando recursos nesses POSTs é [nao verificado]. (`app/config/crons.py:362-398`, `:745-753`)
- Para separar concorrência: obter apenas intervalos início/fim dos comandos, quantidades de processos ativos e contagens de sessões PostgreSQL esperando lock/IO durante o POST. A presença dessas medições existentes em prod é [nao verificado]; horário agendado sozinho não comprova execução nem contenção. (`app/config/crons.py:362-398`, `:745-753`)

Campos e leitores:

| Entidade/campos | Escritor | Leitor comprovado |
|---|---|---|
| Pergunta: `validada_em`, `validada_por` | `app/chamados/services/validacao.py:133-141` | Estado: `app/chamados/models.py:3041-3047`; vitalidade: `app/chamados/juizes.py:1325`; placar humano: `app/chamados/services/painel.py:261-273` |
| Pergunta: `materializada_em`, `materializada_por`, `via_resolucao` | `app/chamados/models.py:3099-3101`, `:3128-3137`, `:3186-3189` | Vitalidade: `app/chamados/juizes.py:1336`, `:1362-1369`; autoria: `app/ponto/services/procedencia_batida.py:71-72` |
| Pergunta: `pendencia_admin`, `recusa_motivo` | `app/chamados/services/materializacao.py:850-851`, `:1102-1103` | `app/templates/chamados/partials/modal_fio.html:222-226` |
| Pergunta: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `app/chamados/veredito_lavrado.py:270-275` | `app/chamados/services/raiox.py:175-187`; validade: `app/chamados/veredito_lavrado.py:224-232` |
| Decisão: `pergunta`, `ato`, `ato_em`, `ato_por`, `ato_texto`, `ato_motivo` | `app/chamados/services/validacao.py:139-141`; bulk: `app/chamados/models.py:3953` | Sequência: `app/chamados/models.py:3500-3516`; autores e conteúdo: `app/templates/chamados/partials/modal_fio.html:205-212` |
| Disputa: `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | `app/chamados/models.py:1932-1936` | Idempotência: `app/chamados/models.py:1880-1881`; fila aberta: `app/chamados/services/fio.py:131-135`; motivo: `app/chamados/services/vigia_fio.py:734` |
| Turno: `colaborador`, `data_turno`, `entrada`, `saida`, `aberto`, `cross_meianoite`, `dt_fim_previsto` | `app/ponto/nucleo.py:48-59` | Todos: `app/ponto/management/commands/verificar_nucleo.py:30`; alertas: `app/ponto/management/commands/processar_alertas_turno.py:76-100` |
| Colaborador: `onboarding_completo` | `app/colaboradores/signals.py:94-102` | `app/colaboradores/queries.py:48` |
| Nota: `chamado_id`, `autor_tipo`, `autor_user`, `conteudo`, `anexo_hash`, `evento_chave`; `criado_em` automático | `app/chamados/services/nota.py:88-107`; automático: `app/chamados/models.py:1674` | Relação/autor/conteúdo/data: `app/templates/chamados/partials/modal_fio.html:461-473`; chave: `app/chamados/services/nota.py:109`; leitor de `anexo_hash` [nao verificado] |
| Auditoria: `usuario`, `acao`, `modelo`, `objeto_id`, `descricao`, `valor_antes`, `valor_depois`, `ip`, `ator`; `criado_em` automático | `app/core/observ.py:134-138`; automático: `app/core/models.py:307` | UI: `app/templates/core/log_auditoria.html:56-74`; leitores específicos de `valor_antes`, `valor_depois`, `ator` [nao verificado] |

FM: grava `atualizado_em`, `horas_trabalhadas`, `horas_noturnas`, `horas_extras`, `horas_extras_50`, `horas_extras_50_noturna`, `horas_extras_100`, `horas_extras_100_feriado`, `horas_extras_100_noturna`, `horas_folga_trabalhada`, `horas_atraso`, `horas_saida_antecipada`, `horas_falta`, `horas_intra_indenizada`, `turnos_abertos`, `inconsistencias`, `semanas_dsr_ok`, `semanas_dsr_perdido`, `horas_reflexo_dsr`, `saldo_banco_horas`, `minutos_previstos`, `dias_previstos`, `dias_incertos`, `minutos_abonados`, `minutos_realizados`, `motivos_espelho`, `causa_espelho`, `previsto_em`, `derivado_em`. Persistência por `save()` quando aberto ou `update()` dos campos calculados quando aprovado. (`app/ponto/services/fechamento.py:1143-1189`)

Leitores FM: resumo do cartão lê trabalhadas/extras/atraso/antecipada/abertos/inconsistências/banco/falta/folga/previstos/abono; rubricas folha leem noturnas/HE/folga/intra; export lê `previsto_em`, realizados e abonados; retrato lê causa; contador lê derivado; indicadores leem DSR perdido. Leitores individuais restantes não foram censados integralmente [nao verificado]. (`app/relatorios/cartao_pela_celula.py:94-120`; `app/folha/export.py:420-452`, `:1037-1070`; `app/folha/retrato_folha.py:1115`; `app/ponto/services/fechamento.py:1695-1696`; `app/inteligencia/views.py:119`)

DiaPago: grava `colaborador`, `mes`, `ano`, `data`, `tipo`, `detalhe`, `furo_apurado`, `versao`, `versao_motor`, `versao_dna`, `evento_origem`, `lavrado_em` e rubricas: `horas_trabalhadas`, `horas_noturnas`, `horas_extras_50`, `horas_extras_50_noturna`, `horas_extras_100`, `horas_extras_100_feriado`, `horas_extras_100_noturna`, `horas_folga_trabalhada`, `horas_atraso`, `horas_saida_antecipada`, `horas_falta`, `horas_intra_indenizada`, `minutos_abonados`, `minutos_previstos`, `minutos_realizados`, `horas_reflexo_dsr`, `saldo_banco_horas`; substitui todas as linhas de colaborador/competência/versão. (`app/ponto/services/dia_pago.py:34-44`, `:335-360`; automático: `app/ponto/models.py:380`)

Leitores DiaPago: soma todas as rubricas; seleciona identidade por colaborador/versão/tipo/data; lê detalhe para hora paga; lê furo e relógio da lavra. Leitores específicos de `versao_dna` e `evento_origem` fora de reversões [nao verificado]. (`app/ponto/services/dia_pago.py:559-563`, `:855-856`, `:897-898`, `:588-589`, `:649`)

## 11/11 10:16 · sessao 54a8712f0a4e · RAIZ-DA-DISPUTA · leitura · origem=Code

**Não pode abrir como raia disjunta:** F3/F4 alcança `materializacao.py`, já listado na E3, e `disputa_emissao.py`, listado na E4B e na c21; a regra de paralelismo é por **arquivo**, independentemente de conflito entre hunks. (`app/chamados/services/materializacao.py:1375`; `app/chamados/services/disputa_emissao.py:1964`; anexos `arquivos_e3.txt:3`, `arquivos_e4b.txt:3`, `arquivos_raia_c21_9bba8132b.txt:4`; `CLAUDE.md:309`.)

Nas referências aos anexos, linha 1 significa a primeira linha de conteúdo depois do marcador `== anexo… ==`.

**1. Arquivos e sítios da cura**

O quadro é um **mapa de intervenção**, usado nas interseções abaixo. O inventário definitivo de edições depende do patch, ainda **[nao verificado]**.

| Arquivo | Sítios que a cura precisa abranger |
|---|---|
| `app/chamados/models.py` | F3: C7 e porta de fechamento, autoria e trilha (`:1848`, `:1853`, `:1933`, `:1937`); fonte do carimbo e autor (`:3097`, `:3100`, `:3152`, `:3190`). F4: agrupamento na raiz e estado derivado; estado atual em `:3034`, identidade em `:3437`. |
| `app/chamados/services/materializacao.py` | F4: unidade e transação individuais (`:1158`, `:1220`), aplicação individual no lote (`:1375`), estado de aplicação (`:425`). F3: cauda que consulta C7 e fecha (`:1316`, `:1333`). |
| `app/chamados/services/validacao.py` | Valida uma pergunta (`:435`), materializa essa pergunta antes do lote (`:461`, `:463`), devolve validação recusada (`:464`) e chama o lote (`:469`). |
| `app/chamados/services/passivo_validado.py` | Reparador também chama diretamente a unidade individual (`:514`, `:516`); precisa ser abrangido pelo novo contrato, seja nessa chamada, seja dentro da unidade. |
| `app/chamados/signals.py` | O evento recebe a pergunta, mas encaminha somente `disputa_id`, depois do commit (`:228`, `:231`, `:237`, `:238`): sítio de propagação da autoria F3. |
| `app/chamados/services/becos.py` | Recorte exige pai terminal (`:29`, `:31`); entrada do evento fecha usando usuário sistema (`:78`), enquanto a porta aceita o usuário recebido (`:57`). |
| `app/chamados/reconciliador.py` | Caudas de carimbo/fechamento a integrar: `:405`, `:411`, `:412`; `:595`, `:611`, `:612`; `:1147`, `:1170`, `:1171`. |
| `app/chamados/services/disputa_emissao.py` | Fechamento do agrupador (`:1278`, `:1284`) e do resolvedor (`:1961`, `:1963`, `:1964`); resposta/reversão também entram no censo (`:1508`, `:1632`). |
| `app/chamados/services/explicador.py` | Calcula `pode_fechar` só pela ausência de fio vivo (`:137`, `:163`), enquanto C7 também exige ausência de resposta sem veredito (`app/chamados/models.py:1853`). |
| `app/chamados/relatorios_disputa.py` | Publica `PRONTA_FECHAR` pela ausência de vivas (`:18`, `:20`), outra projeção a alinhar ao C7 (`app/chamados/models.py:1853`). |
| `app/chamados/services/declaracao_texto.py` | Catálogo pergunta→posição do marco (`:42`, `:55`); limitação expressa de identidade entre duas pausas (`:49`, `:50`). |
| `app/chamados/services/fio.py` | Identidade/canônica e plano de retratação abrangidos pelo censo (`:1232`, `:1259`, `:1290`); não são a autoridade do par, que está na célula (`app/escala/models.py:1652`). |
| `app/chamados/services/relavra_via.py` | Escrita direta de `via_resolucao` a abranger na consolidação de escritor prevista no censo (`:125`, `:127`; `app/chamados/models.py:3101`). |
| `app/chamados/services/historico.py` | Testemunha da materialização, abrangida pela revisão de significado do carimbo (`:103`, `:105`; `app/chamados/services/materializacao.py:924`, `:1099`). |

A autoridade F4 **já existe como dado**: `CelulaDia.PARES_PAUSA_DNA` declara `hii/hfi` e `hii2/hfi2`; `marcos_presentes()` seleciona os marcos presentes. `pausas_do_dia()` lê esses pares e devolve vazio quando a célula existente não declara marcos. Isso fundamenta leitura da autoridade, não necessidade de editá-la. (`app/escala/models.py:1652`, `:1672`, `:1679`, `:1197`, `:1202`, `:1207`.)

Não foi demonstrada uma função existente que receba duas perguntas e certifique “mesma pausa”: **[nao verificado]**. As funções encontradas respondem outras perguntas: posição pelo motivo, instâncias do turno e marco do chamado. (`app/chamados/services/declaracao_texto.py:55`; `app/chamados/services/tarefa_do_dia.py:91`; `app/chamados/juizes.py:524`.)

A catraca também poderá precisar de edição **se as ocorrências registradas mudarem de linha**; isso depende do patch. (`app/core/tests/test_medida_turno_do_par.py:267`, `:268`, `:269`, `:270`, `:271`, `:272`; anexo `construtor22.q:12`.)

**2. Interseções exatas do mapa acima**

| Par | Arquivos em comum |
|---|---|
| **F3/F4 × E3** | `app/chamados/services/materializacao.py` — sítios `:1158`, `:1375`; anexo `arquivos_e3.txt:3`. |
| **F3/F4 × E4B** | `app/chamados/services/disputa_emissao.py` — sítios `:1284`, `:1964`; anexo `arquivos_e4b.txt:3`. |
| **F3/F4 × c21** | `app/chamados/services/declaracao_texto.py` — sítios `:49`, `:55`; anexo `arquivos_raia_c21_9bba8132b.txt:2`. **E** `app/chamados/services/disputa_emissao.py` — sítios `:1284`, `:1964`; mesmo anexo `:4`. |

Se houver atualização da catraca, acrescenta-se **aos três pares** `app/core/tests/test_medida_turno_do_par.py`. (`app/core/tests/test_medida_turno_do_par.py:268`; anexos `arquivos_e3.txt:15`, `arquivos_e4b.txt:22`, `arquivos_raia_c21_9bba8132b.txt:33`.)

Mudanças adicionais em `juizes.py` ampliariam a interseção com E3 e E4B; necessidade efetiva dessas edições: **[nao verificado]**. Os sítios sensíveis são a agregação de pausas e a morte pela validação. (`app/chamados/juizes.py:919`, `:1296`, `:1325`; anexos `arquivos_e3.txt:1`, `arquivos_e4b.txt:1`.)

Não transformei todos os leitores do censo em edições obrigatórias; interseção definitiva do futuro diff: **[nao verificado]**.

**3. F1/F2 são pré-requisito? E o merge posterior?**

**Pelo critério literal pedido — F3/F4 lê algo que F1/F2 muda — sim, há dependência pelo destino.** Os dois lados são:

| Contrato compartilhado | Lado F1/F2 | Lado F3/F4 |
|---|---|---|
| `PerguntaDisputa.chamado_destino` | F1 concentra a escrita atualmente situada em `models.py:2982`, `:3014` e `fio.py:925`. (Anexo `aval_raiz_f1_um_eleitor_1111.txt:1`.) | Materialização lê o destino (`app/chamados/services/materializacao.py:339`) e busca nele a hora fixa do intervalo (`:408`, `:409`); validação usa o destino na trava (`app/chamados/services/validacao.py:432`). |
| Eleição do destino | F1/c22 substituem as eleições divergentes do derivador, agrupador e declaração. (`app/chamados/services/disputa_emissao.py:238`, `:1197`; `app/chamados/services/declaracao_texto.py:377`; anexos `aval_raiz_f1_um_eleitor_1111.txt:1`, `construtor22.q:8`.) | O marco consumido vem de `chamado.contexto_json['marcos_faltantes']`, casado por motivo. (`app/chamados/juizes.py:524`, `:534`, `:536`; `app/chamados/services/materializacao.py:409`.) |
| Autofecho do agrupador | c22 altera a condição/veto por ata nesse caminho. (Anexo `construtor22.q:10`, `:11`; correspondente atual `app/chamados/services/disputa_emissao.py:1278`, `:1279`.) | F3 precisa preservar essa condição ao integrar autoria e reação da raiz; fechamento atual em `app/chamados/services/disputa_emissao.py:1284`. |

Isso **não equivale a depender de um campo novo para compilar**: destino, autoria do carimbo, autoria do fecho e C7 já existem. (`app/chamados/models.py:3385`, `:3397`, `:1933`, `:1848`.)

Um merge posterior da c22 **pode mudar o destino eleito e, por consequência, o marco/hora consumidos pela materialização**. O significado da FK continua “chamado que a pergunta resolve”; muda o valor eleito. (`app/chamados/models.py:3391`; `app/chamados/juizes.py:534`, `:536`; `app/chamados/services/materializacao.py:409`; anexo `construtor22.q:8`.) Conflito textual real entre os patches finais: **[nao verificado]**.

Há ainda um cuidado independente da c22: `instancia_marco` hoje deriva do motivo, e o nascimento recusa valor diferente. Reutilizá-lo silenciosamente como chave DNA ou índice da pausa quebraria esse contrato já na cópia atual. (`app/chamados/services/declaracao_texto.py:55`; `app/chamados/models.py:3437`, `:3441`, `:3446`.) O par deve ser lido da célula conforme o aval, sem converter `chamado_destino` em identificador de pausa. (Anexo `aval_f4_par_pela_celula_1111.txt:2`, `:6`; `app/escala/models.py:1652`; `app/chamados/models.py:3391`.)

**4. NAO PODE — motivo: interseção com E3 em `materializacao.py`, com E4B/c21 em `disputa_emissao.py`, suficiente pela trava por arquivo; há também leitura do destino alterado por F1/F2.** (`app/chamados/services/materializacao.py:1375`, `:339`; `app/chamados/services/disputa_emissao.py:1964`; anexos `arquivos_e3.txt:3`, `arquivos_e4b.txt:3`, `arquivos_raia_c21_9bba8132b.txt:4`; `CLAUDE.md:309`.)

## 11/11 10:14 · sessao d9f4e17b69b7 · - · - · origem=Ronald (tmux)

Mapa comprovado no HEAD, somente leitura.

1. Entrada e laço:
- Rota `chamados/urls.py:37` → `views_cobrar.validar_lote_tela` (`app/chamados/views_cobrar.py:372`); autorização `:376`, lê `refs` `:380`, chama lote `:381`, registra auditoria por POST `:383`, renderiza `:386`.
- Lote normaliza/deduplica refs em lista (`app/chamados/services/validacao.py:713`), faz um SELECT inicial com disputa (`:724`), percorre serialmente (`:727`), reclassifica cada pergunta (`:735`), chama ação (`:746`), faz `refresh_from_db` (`:752`).
- Classe A resolve delta e slot (`app/chamados/services/validacao.py:864`, `:872`) e consulta existência de batida apurável na janela ±60 min (`:875`). Não varre toda fila de validação nesse POST; recebe refs explícitas (`app/chamados/views_cobrar.py:380`; `app/chamados/services/validacao.py:724`).

2. Uma pergunta aprovada:
- `acao_validar_pergunta` → `validar_pergunta` (`app/chamados/services/acoes_disputa.py:19`).
- Novo SELECT da pergunta (`app/chamados/services/validacao.py:389`), juiz de implausibilidade (`:404`), foto da disputa (`:416`), atomic (`:425`), trava na ordem colaborador/disputas/chamados (`:431`).
- Carimbo `validada_em/por` (`:435` → `:134`) salvo em `:137`; registra `DecisaoPergunta` em `:139` → bulk INSERT (`app/chamados/models.py:3953`), e acorda células (`:3954`).
- Agenda recálculo da competência do colaborador (`app/chamados/services/validacao.py:446` → `app/ponto/services/fechamento.py:1809`, `:1810`, `:1824`).
- Materializa unidade desta pergunta (`app/chamados/services/validacao.py:463` → `app/chamados/services/materializacao.py:1158`), tenta devolver validação recusada (`app/chamados/services/validacao.py:464`).
- Depois disso, por CADA item do lote, varre todas irmãs respondidas/validadas/não materializadas da disputa (`app/chamados/services/validacao.py:469` → `app/chamados/services/materializacao.py:1369`, `:1375`, `:1376`); conta vivas/C7 e pode fechar disputa (`:1378` → `:1314`, `:1316`, `:1333`). Assim, um item não implica somente uma tentativa de materialização.
- Refresh próprio e auditoria de resultado (`app/chamados/services/validacao.py:472`, `:479`, `:494`, `:507`); refresh adicional do lote (`:752`).

3. Plantio de hora e multiplicação:
- Unidade verifica data do fato (`app/chamados/services/materializacao.py:1192`), passa por atomic e guarda “em voo” (`:1220`), ramo hora (`:1222`).
- Ramo hora calcula instante/tipo (`:846`), pode gravar `pendencia_admin/recusa_motivo` (`:850`, `:851`), busca existente (`:859`), testa paridade (`:875`), chama `registrar_batida` (`:927`).
- `registrar_batida`: SELECT mesmo tipo (`app/ponto/registro_batida.py:113`), SELECT mesma chave (`:120`), guarda competência (`:135`), janela oposta/exists (`:138`, `:145`), INSERT de Batida (`:157`), agenda fechamento por evento (`:160` → `:178`).
- Após inserir, liga `pergunta_origem` (`app/chamados/services/materializacao.py:946`, `:948`) pela porta `anotar`, que faz OUTRO `Batida.save` (`app/ponto/registro_batida.py:254`).
- Portanto, quando cria batida e liga origem, são dois saves de Batida, com as mesmas cascatas abaixo, exceto NOTIFY que só ocorre na criação (`app/chamados/services/materializacao.py:927`, `:946`; `app/ponto/registro_batida.py:157`, `:254`; `app/ponto/signals.py:42`).
- Reconciliam-se destino e clones (`app/chamados/services/materializacao.py:974` → `:762`, `:771`, `:777`, `:780`).
- Juiz confirma lastro (`:1233`), porta `carimbar` (`:1277`) aplica materializada/via e pergunta ao C6 (`app/chamados/models.py:3129`, `:3139`), relavra S2 (`:3150`), salva (`:3152`). Recusa pode gravar motivo e nota (`app/chamados/services/materializacao.py:1297` → `:1089`, `:1099`, `:1103`, `:1107`).

4. Receivers de cada save de Batida:
- Recompute `TurnoMaterializado` em quatro datas, d−2..d+1 (`app/ponto/signals.py:19`, `:26`, `:34`); reconstrói linhas e DELETE+bulk INSERT (`app/ponto/nucleo.py:47`, `:48`, `:58`, `:59`). HEAD também emite eventos de turnos fechados (`:64`, `:66`).
- Só criação: `publicar_batida` (`app/ponto/signals.py:39`, `:45`) → um SQL com dois `pg_notify`, condicionado a posto (`app/ponto/realtime.py:29`, `:42`).
- Cartório julga d−1 e d (`app/ponto/signals.py:108`, `:112`, `:131`, `:135` → `:93`), salvo reentrada `em_julgamento` (`:90`).
- Reconciliador do colaborador (`app/ponto/signals.py:206`, `:215`) → retrata perguntas absolvidas (`app/chamados/reconciliador.py:1432`), reconcilia órfãs (`:1433`), percorre chamados vivos/testa premissa/retrata (`:1441`, `:1453`, `:1456`, `:1458`, `:1464`).
- Fecha chamado de adesão com batida (`app/ponto/signals.py:234`).

**Custo estrutural para novo plantio+vínculo:** dois recomputes, quatro chamadas de julgamento de célula antes das guardas, duas reconciliações do universo vivo do colaborador, duas verificações de adesão; dois recálculos mensais agendados, um pela validação e um pela criação da batida (`app/ponto/signals.py:26`, `:93`, `:112`, `:215`, `:234`; `app/ponto/registro_batida.py:160`; `app/chamados/services/validacao.py:446`). Número total de queries e tempo em produção: [nao verificado]. Os comentários com “122 ms”, “20 queries”, etc. são relatos históricos do código, não medição do lote fornecido (`app/chamados/services/validacao.py:439`; `app/ponto/signals.py:96`).

5. Receivers de PerguntaDisputa:
- Todo save passa por atomic do modelo (`app/chamados/models.py:3413`, `:3422`).
- Beco: agenda `_fechar` em `on_commit` (`app/chamados/signals.py:227`, `:237`, `:238`); callback consulta disputa+perguntas (`app/chamados/services/becos.py:72`), classifica percorrendo perguntas (`:29`), pode resolver perguntas por chamados e fechar disputa (`:54`, `:57`).
- HEAD, alteração de campo relevante: outbox síncrono (`app/chamados/signals.py:250`, `:255`, `:257`); SELECT linhas de evento (`app/chamados/models.py:3228`), SELECT célula por FK ou par (`:3242`, `:3244`), emite evento (`:3248`). Esses caminhos HEAD são posteriores a 03/11 pelos próprios comentários de 06/11; não atribuir a eles o início do salto (`app/chamados/signals.py:242`).
- Lavra de `veredito_lavrado/julgado_em/janela_expira_em` também dispara callback de beco; filtro do outbox exclui esses campos (`app/chamados/veredito_lavrado.py:275`; `app/chamados/signals.py:255`; `app/chamados/models.py:3220`).

6. Fechamento de Chamado e novo F1:
- `reconciliar` chama decisão, premissa e gate de pergunta viva (`app/chamados/models.py:651`, `:682`, `:685`).
- F1 busca todas não validadas ancoradas no chamado, sem filtro de competência no SELECT (`:344`), julga entrega/C6 (`:364`, `:368`), busca C8 por pergunta viva (`:390`), pode reapontar (`:437`) ou registrar recusa (`:425`).
- `_gravar_estado` repete julgamento na lista guardada (`:221`, `:224`), grava estado (`:296`), relavra perguntas destino não validadas na travessia (`:303`, `:309`).
- Pre-save consulta status anterior (`app/chamados/signals.py:178`, `:184`).
- Post-save urgência (`:53`, `:72` → `app/chamados/models.py:636`); S143 resolver perguntas (`app/chamados/signals.py:90`, `:115`); cartório se status mudou (`:187`, `:192`, `:216`); silêncio isento condicional (`:282`, `:293`, `:305`); HEAD evento filho morto condicional (`:349`, `:357`, `:368`).
- F1 portanto acrescenta trabalho por chamado afetado e por suas perguntas ancoradas, não simplesmente por POST (`app/chamados/models.py:344`, `:360`, `:390`).

7. Fechamento de Disputa:
- C7 lê respostas sem veredito e fio vivo (`app/chamados/models.py:1853`); `fechar` verifica novamente (`:1886`, `:1895`), trava ordem (`:1910`), grava `fechada_em/por`, `resolucao`, `motivo_fechamento` (`:1932`–`:1936`).
- S88 busca pai e reconcilia, depois nota (`app/chamados/signals.py:18`, `:32`, `:42`, `:45`).
- Retoma regularizações confirmadas em on_commit (`:120`, `:137`, `:144`) → SELECT confirmadas não materializadas do colaborador e materialização de cada (`app/chamados/services/regularizacao_ext.py:218`, `:221`, `:223`, `:224`).
- S144 percorre perguntas ainda não materializadas e carimba as aprovadas (`app/chamados/signals.py:147`, `:163` → `app/chamados/services/materializacao.py:1418`, `:1427`, `:1431`).

Callbacks `on_commit` aqui não são evidência de job: a porta apenas registra funções com `transaction.on_commit`, e estas chamam diretamente recálculo/fecho/materialização (`app/ponto/services/fechamento.py:1807`, `:1810`, `:1824`; `app/chamados/signals.py:132`, `:137`, `:144`, `:233`, `:237`, `:238`). Tempo real consumido por cada uma no POST histórico: [nao verificado].

## 11/11 10:14 · sessao 2f4f18edb999 · - · - · origem=Ronald (tmux)

Q3 — Há leitura de campo compartilhado, mas nenhuma dependência demonstrada de interface nova da F1/F2. A FK `chamado_destino` já existe e significa “chamado que esta pergunta resolve”; a F1/c22 altera quem a escreve/elege. A materialização que a F4 precisa curar já lê esse destino para buscar o marco. (`app/chamados/models.py:3385`, `:3391`; `app/chamados/services/materializacao.py:339`, `:408`, `:409`.)

| Vínculo | Lado F1/F2 na cópia | Lado F3/F4 na cópia |
|---|---|---|
| Destino gravado | Reuso escreve `chamado_destino`; nascimento o recebe; reapontamento o escreve. (`app/chamados/models.py:2982`, `:3014`; `app/chamados/services/fio.py:925`.) | Materializador lê destino e busca o marco nele; validação também o usa na trava. (`app/chamados/services/materializacao.py:339`, `:794`, `:408`, `:409`; `app/chamados/services/validacao.py:432`.) |
| Eleição do destino | Derivador exige candidato único; agrupador escolhe primeiro do lote; declaração pede chamado do ato. (`app/chamados/services/disputa_emissao.py:238`, `:1197`; `app/chamados/services/declaracao_texto.py:377`.) | `entrada_de_marco` lê `destino.contexto_json['marcos_faltantes']`, portanto trocar eleição pode trocar o marco fonte consumido pelo materializador. (`app/chamados/juizes.py:524`, `:534`, `:536`, `:537`; `app/chamados/services/materializacao.py:409`.) |
| Identidade do par | A eleição do chamado resolve um destino, não expressa o índice de uma pausa. A identidade atual agrega duas pausas na mesma posição. (`app/chamados/models.py:3391`; `app/chamados/services/declaracao_texto.py:49`, `:50`.) | A célula já declara os pares `hii/hfi`, `hii2/hfi2`; o leitor `pausas_do_dia` retorna somente pares completos e retorna vazio quando a célula não declara marcos. (`app/escala/models.py:1652`, `:1197`, `:1199`, `:1201`, `:1202`, `:1207`.) |
| Autoria F3 | O destino não é a autoria do carimbo ou do fecho. São campos distintos já existentes. (`app/chamados/models.py:3385`, `:3397`, `:1766`.) | Carimbo grava `materializada_por_id`; fecho recebe usuário e grava `fechada_por`. (`app/chamados/models.py:3100`, `:3125`, `:1933`, `:1936`.) |
| C7 e autofecho | O agrupador já consulta C7 e distingue abstenção por ata; esse é o ponto que o brief c22 manda modificar/restaurar — aplicação futura do brief: [nao verificado]. (`app/chamados/services/disputa_emissao.py:1278`, `:1279`, `:1282`.) | O materializador consulta C7 e fecha; outras caudas fazem o mesmo. (`app/chamados/services/materializacao.py:1316`, `:1333`; `app/chamados/services/becos.py:56`, `:57`; `app/chamados/reconciliador.py:411`, `:611`, `:1170`.) |

Pelo critério literal do usuário — “F3/F4 lê algo que F1/F2 muda” — **sim, existe acoplamento pelo destino**: escritor/eleitores de um lado, leitor do marco e plantio do outro. Isso não demonstra necessidade de aguardar um campo ou função ainda inexistente para começar a implementar a cura. (`app/chamados/models.py:2982`, `:3014`; `app/chamados/services/fio.py:925`; `app/chamados/services/disputa_emissao.py:238`, `:1197`; `app/chamados/services/materializacao.py:339`, `:409`.)

A identificação do par deve consumir os pares da célula; usar `chamado_destino` como identificador do par criaria um segundo significado para uma FK cujo contrato é outro. Não há evidência de que os avais exijam esse reaproveitamento; implementação futura: [nao verificado]. (`app/escala/models.py:1652`; `app/chamados/models.py:3391`.)

Um merge posterior da c22 pode mudar o valor escolhido para destino e, por consequência, o marco consumido, preservando o significado da FK. Conflito textual real e resultado semântico dos patches finais: [nao verificado]. (`app/chamados/services/disputa_emissao.py:238`, `:1197`; `app/chamados/juizes.py:534`, `:536`; `app/chamados/models.py:3391`.)

A trava de paralelismo local é por **arquivo em comum**, suficiente mesmo sem dependência de interface nova; não exige provar conflito de hunks. (`CLAUDE.md:309`, `:310`, `:311`.)

## 11/11 10:14 · sessao e7ab94268b20 · - · - · origem=Ronald (tmux)

O par já existe como **dado da célula**: `CelulaDia.PARES_PAUSA_DNA` declara `(hii,hfi)` e `(hii2,hfi2)`; `marcos_presentes()` fornece as chaves efetivamente presentes e sua ordem. Não exige mudar essa autoridade para F4 lê-la. (`app/escala/models.py:1652`, `app/escala/models.py:1659`, `app/escala/models.py:1672`, `app/escala/models.py:1679`.)

`EscalaColaborador.pausas_do_dia()` fornece todos os pares declarados no DNA; célula existente sem marcos devolve `()`. `intervalo_do_dia()` é só o leitor da primeira pausa. O fallback para template ocorre quando não há célula; logo, a F4 literal “pela célula” precisa considerar essa diferença ao escolher como consumir a autoridade. (`app/escala/models.py:1163`, `app/escala/models.py:1166`, `app/escala/models.py:1197`, `app/escala/models.py:1202`, `app/escala/models.py:1208`.)

A enumeração AST em memória encontrou consumidores desses pares, mas não identificou função que recebe duas perguntas e responde “mesma pausa”. Completude contra lógica construída/reflexão e execução do enumerador como evidência `arquivo:linha`: [nao verificado]. Exemplos conferidos de funções existentes que **não** respondem isso:

- `fato_do_marco_no_espelho(p)` agrega **todas** as pausas do dia com AND para um motivo; não distingue duas perguntas de pausas diferentes. (`app/chamados/juizes.py:892`, `app/chamados/juizes.py:911`, `app/chamados/juizes.py:919`, `app/chamados/juizes.py:933`.)
- `_resposta_concorda_com_a_luz(p)` devolve `None` quando duas pausas compartilham o motivo. (`app/chamados/juizes.py:1276`, `app/chamados/juizes.py:1296`, `app/chamados/juizes.py:1298`.)
- `instancias_do_turno()`/`_posicoes()` já separam a segunda repetição como `int_ida:2`, mas são projeções por posição, sem casamento entre duas perguntas. (`app/chamados/services/tarefa_do_dia.py:57`, `app/chamados/services/tarefa_do_dia.py:65`, `app/chamados/services/tarefa_do_dia.py:67`, `app/chamados/services/tarefa_do_dia.py:91`.)

O gargalo de identidade merece constar no brief: o catálogo de pergunta só deriva `int_ida/int_volta`, e comenta expressamente que os emissores não carregam índice da pausa. O nascimento veta `instancia_marco` diferente da derivação do motivo. Portanto, usar `instancia_marco` como “hii/hfi” ou “int_ida:2” sem alterar o contrato introduziria incompatibilidade **já com a cópia atual**, independentemente de c22. (`app/chamados/services/declaracao_texto.py:45`, `app/chamados/services/declaracao_texto.py:49`, `app/chamados/services/declaracao_texto.py:55`, `app/chamados/models.py:3437`, `app/chamados/models.py:3441`, `app/chamados/models.py:3446`.)

Sobre arquivos da cura efetiva:

| Arquivo / sítio | Motivo concreto |
|---|---|
| `app/chamados/models.py:3034` | Estado da pergunta hoje diz VALIDADA antes de consultar C6; candidato para estado derivado de espera. Agrupamento na DS exigido pelo aval: implementação futura [nao verificado]. |
| `app/chamados/services/materializacao.py:1158`, `:1220`, `:1375` | Unidade e atomic são por pergunta; o lote itera individualmente. O par num ato precisa atuar nessa fronteira. |
| `app/chamados/services/validacao.py:435`, `:461`, `:463`, `:469` | Valida uma e materializa essa uma antes do lote; precisa respeitar a espera/unidade nova. |
| `app/chamados/services/passivo_validado.py:514`, `:516` | Reparador chama a unidade individual diretamente; precisa migrar ou estar coberto por mudança na própria unidade. |
| `app/chamados/services/declaracao_texto.py:49`, `app/chamados/models.py:3441` | Só são mudanças obrigatórias se a implementação ampliar a identidade para distinguir duas pausas; escolha desse desenho [nao verificado]. |
| `app/chamados/juizes.py:919`, `:1296` | Precisam entrar na análise de impacto se a pergunta passar a identificar uma pausa específica; suas leituras atuais agregam/abstêm diante de duas pausas. Mudança efetiva necessária para a implementação escolhida [nao verificado]. |

Não é correto pôr `escala/models.py`, `escala/servico_jornada.py`, `ponto/turnos.py` ou `ponto/services/cartorio.py` no diff só porque são autoridades/chamadores. O dado do par já está em `escala/models.py:1652`; o materializador já consome autoridades de turno e plantio. Necessidade de alteração nesses outros arquivos: [nao verificado]. (`app/chamados/services/materializacao.py:421`, `app/chamados/services/materializacao.py:1222`, `app/chamados/services/passivo_validado.py:506`.)

Há dependência **de leitura do valor existente de destino** no materializador: lê `p.chamado_destino`, usa-o para tipo e para substituir a hora do intervalo pelos `marcos_faltantes`. Isso não demonstra pré-requisito da função nova de F1 ou do eleitor de c22; demonstra que mudanças na eleição do destino podem mudar o marco consumido pelo plantio. (`app/chamados/services/materializacao.py:339`, `app/chamados/services/materializacao.py:386`, `app/chamados/services/materializacao.py:408`, `app/chamados/services/materializacao.py:409`; autoridade do casamento por motivo: `app/chamados/juizes.py:524`, `app/chamados/juizes.py:534`, `app/chamados/juizes.py:536`.)

F4 não deve usar `chamado_destino` como identidade do par: o par está nas chaves do DNA, enquanto destino é FK de chamado e os marcos consumidos por ele vêm de contexto do chamado. Essa separação é inferência de desenho sustentada pelos sítios; patch futuro: [nao verificado]. (`app/escala/models.py:1652`, `app/chamados/models.py:3385`, `app/chamados/juizes.py:534`.)

## 11/11 10:13 · sessao 6563a0ff60ea · - · - · origem=Ronald (tmux)

A cura F3 tem estes sítios demonstráveis; o diff final e a escolha entre centralizar o disparo na porta ou no evento são [nao verificado].

| Arquivo | Sítios relevantes para F3 |
|---|---|
| `app/chamados/models.py` | C7 completo em `:1848`, `:1853`; porta de fecho recebe usuário em `:1855` e persiste autor/motivo/resolução com evento em `:1932`, `:1933`, `:1934`, `:1935`, `:1936`, `:1937`, `:1940`; escritor do carimbo e autor em `:3097`, `:3099`, `:3100`; portas persistem em `:3152`, `:3190`. |
| `app/chamados/signals.py` | Evento da pergunta recebe a instância em `:228`, mas passa somente `disputa_id` ao consumidor em `:231`, `:237`; dispara depois do commit em `:238`. |
| `app/chamados/services/becos.py` | Recorte exige pai terminal em `:29`, `:31`; consulta C7 em `:33`; fecha com usuário recebido em `:57`; entrada do evento substitui autoria por usuário sistema em `:78`. |
| `app/chamados/services/materializacao.py` | Cauda consulta C7 em `:1316` e fecha com `request_user` em `:1333`; lote chama essa cauda em `:1378`. |
| `app/chamados/reconciliador.py` | Caudas existentes consultam C7/fecham em `:411`, `:412`, `:611`, `:612`, `:1170`, `:1171`; carimbam antes em `:405`, `:595`, `:1147`. |
| `app/chamados/services/disputa_emissao.py` | Agrupador fecha em `:1284`; resolvedor fecha em `:1963`, `:1964`, com guarda de pergunta em voo em `:1961`. |
| `app/chamados/services/explicador.py` | Deriva `pode_fechar` apenas de `fio_vivo` em `:136`, `:137`, publica em `:163`; diverge do C7 completo em `app/chamados/models.py:1853`. |
| `app/chamados/relatorios_disputa.py` | Publica `PRONTA_FECHAR` pela ausência de vivas em `:18`, `:19`, `:20`; não consulta `respostas_sem_veredito`, exigido pelo C7 em `app/chamados/models.py:1853`. |

O autor do último ato de carimbo pode ser preservado pelo argumento da porta (`app/chamados/models.py:3125`, `:3185`) e já tem campo próprio (`app/chamados/models.py:3100`, `:3397`). Eleger o autor por “maior timestamp encontrado” não está demonstrado como implementação correta: `manter_carimbo` preserva instante e autor anteriores ao trocar a via (`app/chamados/models.py:3182`, `:3183`).

F1 não cria o campo consumido por F3: `chamado_destino` já existe e significa chamado resolvido pela pergunta (`app/chamados/models.py:3385`, `:3391`); autoria de carimbo é outro campo (`app/chamados/models.py:3397`). A cura F3 lê destino em travas/recorte já existentes (`app/chamados/models.py:1911`; `app/chamados/services/becos.py:30`); dependência obrigatória da nova função proposta por F1 permanece [nao verificado].

Para interseção por arquivo do núcleo F3 acima: E3 contém `app/chamados/services/materializacao.py`; E4B contém `app/chamados/services/disputa_emissao.py`; c21 contém `app/chamados/services/disputa_emissao.py` — pertencimento às listas fornecidas: [nao verificado por arquivo:linha, dado textual recebido]. Os sítios F3 desses dois arquivos são `app/chamados/services/materializacao.py:1316`, `:1333` e `app/chamados/services/disputa_emissao.py:1284`, `:1964`.

A catraca de endereços também registra materialização/explicador/emissão em `app/core/tests/test_medida_turno_do_par.py:267`, `:268`, `:269`, `:270`; necessidade de modificá-la depende do deslocamento efetivo dessas ocorrências, ainda [nao verificado].

## 11/11 10:12 · sessao 88613664e51b · UI-PAINEL-MOSTRA-O-APP · censo · origem=Code

Censo estático da cópia, somente leitura. Identidade do HEAD/hash informado: **[nao verificado]** — a cópia não contém metadados Git. Execução dos testes, situação em produção e smoke: **[nao verificado]**. [nao verificado]

1. **Regra do rótulo: duas cópias completas e uma variante no template.**

   | Local | Leitura/escrita |
   |---|---|
   | `app/api/views_core.py:307` | Conta chamados não lidos no recorte `VIVOS`. |
   | `app/api/views_core.py:314` | Conta perguntas de `consulta_do_app()['pendentes']`. |
   | `app/api/views_core.py:315` | Escreve a regra completa: pendentes → `{count, cor: '#f59e0b', rotulo: 'Responder DP'}`; senão não lidos → vermelho `'#ef4444'`, `"Chamados"`; senão `None`. |
   | `app/api/views_core.py:359` | Expõe os dois contadores e `chamados_badge` na resposta. |
   | `app/api/views.py:318` | Repete a consulta de não lidos. |
   | `app/api/views.py:325` | Repete contagem e regra completa. |
   | `app/api/views.py:370` | Expõe os mesmos três campos. |
   | `app/core/views.py:253` | Lê pendentes para o dashboard web. |
   | `app/templates/core/dashboard_colaborador_v2.html:78` | Variante: pendentes mudam destino do link, badge amarelo/count e texto `"Responder DP"`; sem pendentes escreve `"Chamados"`, sem ramo vermelho de não lidos. |
   | `app/templates/core/dashboard_colaborador.html:90` | Template anterior lê `chamados_nao_lidos` e mostra “novo(s)”; não contém a regra completa. |

   Portanto, **duas cópias da decisão ternária da API**; contando qualquer escrita manual da escolha `"Responder DP"/"Chamados"`, há também a variante do dashboard. (`app/api/views_core.py:315`; `app/api/views.py:326`; `app/templates/core/dashboard_colaborador_v2.html:80`)

   Os dois kernels têm rotas próprias para `/me/`; nenhum deles pode ser tratado como cópia morta. (`app/api/urls.py:64`; `app/api/urls_core.py:5`; `app/api/urls_core.py:13`)

   Leitores de teste da regra: contrato do badge da API, matriz dos dois kernels e testes do dashboard; os mutantes também dependem dos trechos literais atuais nos dois kernels. (`app/api/tests/test_api_me_badge.py:54`; `app/chamados/tests/test_e3_app_orm.py:28`; `app/core/tests/test_dashboard_badge.py:52`; `app/chamados/tests/e3_mutantes.py:23`)

   A busca textual incluiu templates e `static/js`; outras implementações JavaScript dessa regra não foram encontradas. Ausência absoluta de implementação equivalente com outros nomes: **[nao verificado]**.

2. **Fontes, leitores e escritores dos contadores.**

   **Pendentes de `consulta_do_app`:** são listas derivadas, não um contador persistido. A função lê perguntas vivas e separa como pendente cada pergunta com `respondida_em is None` **e** disputa com `fechada_em is None`; as demais vivas ficam no histórico, em ordem por ID. (`app/chamados/services/pergunta_no_app.py:45`; `app/chamados/services/pergunta_no_app.py:61`)

   Há **sete sítios de consumo em produção**:

   | Consumidor | Referência |
   |---|---|
   | Badge do kernel core | `app/api/views_core.py:314` |
   | Badge do kernel UI | `app/api/views.py:325` |
   | GET de chamados nativos | `app/api/views.py:1005` |
   | GET de perguntas da disputa | `app/api/views.py:2543` |
   | Dashboard web | `app/core/views.py:253` |
   | Aviso de questionário ainda pendente | `app/chamados/services/aviso_pendencia.py:9` |
   | Cobrança | `app/chamados/services/cobranca.py:48` |

   Os dois últimos alimentam, respectivamente, `aviso_texto_livre` e as operações de cobrança; continuam usando a mesma lista. (`app/chamados/services/aviso_pendencia.py:12`; `app/chamados/services/cobranca.py:82`; `app/chamados/services/cobranca.py:163`)

   Escritores diretos dos campos que separam pendentes/histórico:

   | Ato | Escritor |
   |---|---|
   | Nascimento da pergunta | Campo nullable; `_nascer` constrói com `**campos` e insere. (`app/chamados/models.py:2448`; `app/chamados/models.py:3014`) |
   | Resposta do colaborador | `responder_pergunta_disputa_core` atribui e salva `respondida_em`. (`app/chamados/services/disputa_emissao.py:1468`; `app/chamados/services/disputa_emissao.py:1508`) |
   | Resposta automática | `responder_automaticamente`. (`app/chamados/services/disputa_emissao.py:1653`) |
   | Recuperação pela trilha | `carimbar_resposta_pela_trilha`. (`app/chamados/services/disputa_emissao.py:1674`) |
   | Carimbo com resposta textual | `PerguntaDisputa.carimbar`. (`app/chamados/models.py:3130`; `app/chamados/models.py:3152`) |
   | Reabertura para correção | Monta `respondida_em=None` e atualiza diretamente ou pelo descarimbo em lote. (`app/chamados/services/disputa_emissao.py:1593`; `app/chamados/services/disputa_emissao.py:1629`) |
   | Ressuscitação | Delega `respondida_em=None` a `descarimbar_em_lote`. (`app/chamados/models.py:1566`; `app/chamados/models.py:3264`) |
   | Nascimento da disputa aberta | Campo nullable e criações do emissor. (`app/chamados/models.py:1757`; `app/chamados/services/disputa_emissao.py:587`; `app/chamados/services/disputa_emissao.py:1123`; `app/chamados/services/disputa_emissao.py:1358`) |
   | Fechamento da disputa | `DisputaSupervisao.fechar` carimba `fechada_em`. (`app/chamados/models.py:1932`) |
   | Reabertura da disputa | `reabrir_admin` e `reabrir_sistema` zeram `fechada_em`. (`app/chamados/models.py:2149`; `app/chamados/models.py:2173`) |

   As atribuições em simulação de ressuscitação e restauração após recusa de carimbo são apenas em memória. (`app/chamados/models.py:1508`; `app/chamados/models.py:3163`)

   **Limite do censo:** a vitalidade anterior à partição também depende de C6, célula/ata/veredito, validação, materialização, resposta e tranca da competência. O fechamento transitivo de **todos** os escritores desses insumos é **[nao verificado]**; não seria correto reduzir a fonte somente aos dois campos acima. (`app/chamados/models.py:3326`; `app/chamados/juizes.py:1324`; `app/core/portas.py:393`)

   **Não lido:** o campo efetivo é `FollowupChamado.lido_colaborador_em`, nullable; não é `lido_dp_em`. (`app/chamados/models.py:1672`; `app/chamados/models.py:1673`)

   - **Leitores:** os dois badges; serialização nativa de `novo`/`nao_lidos`; predicado web `_nao_lido`; duas posições no card de atendimento. (`app/api/views_core.py:311`; `app/api/views.py:322`; `app/api/views.py:1128`; `app/chamados/views.py:603`; `app/templates/chamados/partials/_card_atendimento.html:51`; `app/templates/chamados/partials/_card_atendimento.html:59`)
   - **Escritor do carimbo:** `nota.marcar_lidas`, via mapa dinâmico `LADOS`, atualizando somente nulos. (`app/chamados/services/nota.py:59`; `app/chamados/services/nota.py:126`)
   - **Três chamadores para o lado colaborador:** GET nativo de chamados, endpoint web de um chamado e endpoint web em lote. (`app/api/views.py:1050`; `app/chamados/views.py:1135`; `app/chamados/views.py:1150`)
   - **Nascimento não lido:** `nota.anexar_ocorrencia` cria a nota sem preencher o carimbo; a porta está registrada em `core/portas`. (`app/chamados/services/nota.py:88`; `app/chamados/services/nota.py:103`; `app/core/portas.py:241`)
   - **Remoção da fonte:** apagar notas em ensaio ou excluir o chamado por cascade elimina followups. (`app/chamados/services/nota.py:136`; `app/chamados/models.py:1668`)

   O vermelho conta **chamados distintos com pelo menos um followup DP não lido**, não quantidade de followups. A serialização nativa tem outro contador, por followup. (`app/api/views.py:318`; `app/api/views.py:1128`; `app/chamados/tests/test_e3_app_orm.py:62`)

   **`status_local` no recorte da contagem:** `VIVOS = ('aberto', 'em_analise', 'registrado')`; `registrado` entra nesse badge. `COBRANCA` é outro conjunto. A própria `consulta_do_app` não contém esse filtro de chamados. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/api/views_core.py:309`; `app/api/views.py:320`; `app/chamados/services/pergunta_no_app.py:69`)

   Escritores de status que podem alterar entrada/saída desse recorte:

   - Default de nascimento: `aberto`; nascimentos pelas portas `abrir` e `criar`. (`app/chamados/models.py:52`; `app/chamados/models.py:938`; `app/chamados/models.py:971`; `app/chamados/models.py:1095`)
   - Transição persistente: `_gravar_estado`, atribuição seguida de `save(update_fields=campos)`. (`app/chamados/models.py:228`; `app/chamados/models.py:296`)
   - Gates que delegam a essa escrita: `_aplicar`, `reconciliar`, `cancelar`, `analisar`, `reabrir`, `encerrar`, `encerrar_por_competencia_trancada`. (`app/chamados/models.py:450`; `app/chamados/models.py:654`; `app/chamados/models.py:788`; `app/chamados/models.py:806`; `app/chamados/models.py:1442`; `app/chamados/models.py:1619`; `app/chamados/models.py:1639`)
   - `_vivas_entre` troca/restaura status somente em memória. (`app/chamados/models.py:356`; `app/chamados/models.py:371`)
   - Migrações de dados alteram `aberto↔registrado` e `cancelado↔superado`, fora do runtime normal. (`app/chamados/migrations/0038_a3_registrado_backfill.py:10`; `app/chamados/migrations/0038_a3_registrado_backfill.py:15`; `app/chamados/migrations/0039_a3_via_backfill.py:24`; `app/chamados/migrations/0039_a3_via_backfill.py:33`)

   Existem selos de escritores de status e da porta de notas; sua aprovação nesta cópia é **[nao verificado]**. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:37`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:449`; `app/chamados/tests/test_porta_nota_do_fio.py:44`; `app/chamados/tests/test_porta_nota_do_fio.py:207`)

3. **View do painel, contexto atual e contador de Mensagens.**

   A view é `painel_colaborador(request, cid)`, com login e permissão `ver_cadastro`; renderiza diretamente o partial indicado. (`app/colaboradores/views.py:914`; `app/colaboradores/views.py:917`; `app/colaboradores/views.py:988`; `app/colaboradores/urls.py:48`)

   Para mensagens, ela passa apenas `chamados`: últimos cinco chamados do colaborador, ordenados por criação, com followups prefetched. Apesar do comentário “abertos”, **não há filtro de status**. Não passa perguntas, histórico, rótulo nem contagem de pendentes. (`app/colaboradores/views.py:936`; `app/colaboradores/views.py:937`; `app/colaboradores/views.py:988`)

   Os demais dados explícitos são colaborador, escala, bloco/portas/legenda do retrato, batidas, mês e CSRF. (`app/colaboradores/views.py:988`)

   O contador do botão é hoje **`{{ chamados|length }}`**, portanto entre zero e cinco chamados carregados; não mede perguntas nem mensagens não lidas. (`app/templates/colaboradores/partials/painel_colaborador.html:39`; `app/colaboradores/views.py:937`)

   A aba contém formulário HTMX e `msgs_lista.html`; a lista mostra os chamados e até cinco followups por chamado, truncados. O envio atualiza somente `#msgs-lista`, não o botão da aba. (`app/templates/colaboradores/partials/painel_colaborador.html:188`; `app/templates/colaboradores/partials/painel_colaborador.html:198`; `app/templates/colaboradores/partials/msgs_lista.html:4`; `app/templates/colaboradores/partials/msgs_lista.html:7`; `app/chamados/views.py:997`)

4. **As chamadas em `api/views.py:1004` e `:2542`.**

   - **`:1004`, `api_chamados_v2`:** consulta pendentes e histórico, agrupa perguntas por chamado, usa destino vivo ou chamado pai, serializa a conversa e **marca followups do DP como lidos**. Não calcula o badge ternário. (`app/api/views.py:973`; `app/api/views.py:1004`; `app/api/views.py:1040`; `app/api/views.py:1044`; `app/api/views.py:1050`; `app/api/views.py:1155`)
   - **`:2542`, `api_disputa_perguntas`:** serve pendentes, histórico, subconjunto `respondidas`, `em_disputa` e IDs de ancoragem. Não calcula rótulo/cor/count do badge. (`app/api/views.py:2531`; `app/api/views.py:2542`; `app/api/views.py:2571`; `app/api/views.py:2573`)

   **Extrair somente a regra para uma função separada não exige mudar essas duas chamadas.** Preservar `consulta_do_app` mantém seus contratos atuais; há teste AST exigindo essa delegação. (`app/chamados/services/pergunta_no_app.py:61`; `app/chamados/tests/test_contract_pergunta_viva.py:68`)

   O selo deve comparar o painel com **`/me/`**; chamar antes o GET de chamados pode consumir o estado não lido e alterar o resultado seguinte. (`app/api/urls.py:64`; `app/api/views.py:318`; `app/api/views.py:1050`)

5. **Cascas e grafo de carregamento.**

   A premissa precisa de um ajuste: o GET do painel entra por **`fetch → innerHTML → htmx.process`**, e não por `hx-get`; HTMX passa a processar os controles do conteúdo injetado. (`app/templates/core/_drawer_generico.html:19`; `app/templates/core/_drawer_generico.html:26`; `app/templates/core/_drawer_generico.html:33`)

   | Caminho | Evidência |
   |---|---|
   | `base.html → core/_drawer_generico.html` | `app/templates/base.html:399` |
   | `base_app.html → core/_drawer_generico.html` | `app/templates/base_app.html:154` |
   | Lista: `colaboradores/lista.html → _lista_tabela.html → abrirPainelColab → abrirDrawer → /colaboradores/<cid>/painel/` | `app/templates/colaboradores/lista.html:1`; `app/templates/colaboradores/lista.html:57`; `app/templates/colaboradores/partials/_lista_tabela.html:16`; `app/templates/core/_drawer_generico.html:89`; `app/templates/core/_drawer_generico.html:104` |
   | Retrato: `folha/retrato_folha.html → _retrato_lista.html → mesma rota`, com mês/ano | `app/templates/folha/retrato_folha.html:1`; `app/templates/folha/retrato_folha.html:53`; `app/templates/folha/_retrato_lista.html:61` |
   | Retrato parcial → mesma lista | `app/templates/folha/_retrato_parcial.html:7` |
   | Raio-X → `_retrato_raiox.html` → mesma rota | `app/templates/folha/_retrato_barra.html:73`; `app/folha/views_retrato.py:510`; `app/templates/folha/_retrato_raiox.html:68` |
   | Painel → `_cabecalho_drawer.html`, condicional `_retrato_o_que_fazer.html`, `msgs_lista.html` | `app/templates/colaboradores/partials/painel_colaborador.html:8`; `app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/templates/colaboradores/partials/painel_colaborador.html:198` |
   | Bloco do retrato → `_led.html`, `_retrato_legenda.html` | `app/templates/folha/_retrato_o_que_fazer.html:36`; `app/templates/folha/_retrato_o_que_fazer.html:71` |

   O partial não estende casca. Os hosts encontrados usam `base.html` fixo; ambas as cascas oferecem a infraestrutura do drawer. A variável global `base_tpl` não substitui esses `extends` fixos. (`app/templates/colaboradores/partials/painel_colaborador.html:1`; `app/templates/colaboradores/lista.html:1`; `app/templates/folha/retrato_folha.html:1`; `app/core/context_processors.py:205`; `app/templates/base_app.html:154`)

   Consequência para o selo: pedir o partial duas vezes não demonstra as duas cascas; é necessário renderizar a integração com cada uma. Já existe exemplo de smoke que verifica os templates efetivamente usados nas duas cascas. (`app/colaboradores/views.py:988`; `app/core/tests/test_smoke_chromium.py:151`; `app/core/tests/test_smoke_chromium.py:172`)

6. **Juiz existente e autoridade da leitura.**

   Não há entrada específica para o rótulo/count “o que o app mostra agora” no registro de chamados nem no lookup documental; há autoridades próximas para vitalidade, retenção, fio e tarefa do dia. (`app/core/juizes.py:272`; `app/core/juizes.py:278`; `app/core/juizes.py:285`; `app/core/juizes.py:287`; `app/core/juizes.py:291`; `app/docs/JUIZES.md:59`)

   **`consulta_do_app` já é a autoridade única da seleção pendentes/histórico apresentada nesses consumidores; ainda não é autoridade do badge inteiro.** Ela delega vitalidade ao C6, enquanto a decisão de badge continua nos dois kernels. (`app/chamados/services/pergunta_no_app.py:3`; `app/chamados/services/pergunta_no_app.py:45`; `app/chamados/services/pergunta_no_app.py:61`; `app/api/views_core.py:315`; `app/api/views.py:326`)

   O nome `estado_app` encontrado no situacional mede adesão/uso do app, não esse rótulo de mensagens/perguntas. (`app/colaboradores/services/situacional.py:191`)

7. **Decisões técnicas para o construtor, sem lei nova.**

   | Candidata | Pró e consequência |
   |---|---|
   | **Função de projeção ao lado de `consulta_do_app`, no mesmo serviço — recomendação** | Reúne lista, consulta de não lidos e decisão do badge, sem fazer o painel importar uma view HTTP. Preserva a consulta existente. (`app/chamados/services/pergunta_no_app.py:61`; `app/api/views_core.py:307`; `app/api/views.py:318`) |
   | Acrescentar tudo à própria `consulta_do_app` | Menos funções, mas muda o retorno e acrescenta consulta a todos os consumidores; há igualdade exata do dicionário e exigência de nove queries em cenário existente. (`app/chamados/tests/test_e3_v4_orm.py:85`; `app/chamados/tests/test_e3_v4_orm.py:91`; `app/chamados/tests/test_e3_v4_orm.py:98`) |
   | Novo módulo em `chamados/services/` | Dá nome próprio à projeção e preserva a seleção; acrescenta arquivo/indireção. Admissibilidade desse arquivo no escopo literal: **[nao verificado]**, pois o limite vem do anexo. |
   | Helper puro recebendo duas contagens | Centraliza prioridade/cor, mas sozinho deixa a produção da consulta de não lidos nos leitores; precisaria de um montador compartilhado para o painel. (`app/api/views_core.py:307`; `app/api/views_core.py:315`) |
   | Helper dentro de uma view da API | Extração local simples, mas cria dependência do painel em camada HTTP e escolhe um kernel como dono. (`app/api/views_core.py:273`; `app/api/views.py:284`; `app/colaboradores/views.py:915`) |

   **Retorno sugerido da projeção:** listas `pendentes`/`historico`, inteiros `questionarios_pendentes`/`chamados_nao_lidos` e `chamados_badge` no formato atual `{count, cor, rotulo} | None`. Assim os três leitores recebem dados prontos; a decisão não reaparece no template. É proposta técnica, apoiada nos contratos existentes. (`app/chamados/services/pergunta_no_app.py:75`; `app/api/views_core.py:314`; `app/api/views_core.py:359`)

   Para a lista visual, os campos já existem: texto `pergunta`, dia `data_evento`, marco `instancia_marco` e motivo. Usar esses dados evita reconstruir identidade a partir do texto. (`app/chamados/models.py:2421`; `app/chamados/models.py:2423`; `app/chamados/models.py:2434`)

   O contador solicitado da aba é uma soma de apresentação sobre o contador atual; não deve ser confundido com o count do badge, que aplica prioridade entre pendentes e não lidos. A soma está fixada no anexo **[nao verificado]**; as duas medidas atuais estão em `app/templates/colaboradores/partials/painel_colaborador.html:39` e `app/api/views_core.py:315`.

   Há quatro cuidados concretos para a construção:

   - Atualizar os dois kernels de forma equivalente: existe contrato de paridade AST. (`app/api/tests/test_contract_paridade_kernels.py:25`)
   - Preservar a mordida dos mutantes que hoje procuram expressões literais nos kernels; a extração muda seus alvos. (`app/chamados/tests/e3_mutantes.py:23`)
   - Acrescentar o selo painel × API e integração das duas cascas; a matriz atual compara endpoint, dashboard e kernels, não este painel. (`app/chamados/tests/test_e3_app_orm.py:28`; `app/colaboradores/tests/test_retrato_no_drawer.py:151`; `app/core/tests/test_smoke_chromium.py:151`)
   - Tratar explicitamente a variante do dashboard no relatório de escopo: ela é outra escrita manual do rótulo, embora não seja a terceira cópia da regra completa. (`app/templates/core/dashboard_colaborador_v2.html:78`)

   O golden atual contém perguntas sobre presença de pergunta no app por colaborador/dia; o caso novo de rótulo/count “agora” permanece requisito do anexo, **[nao verificado]** como implementação. (`eval/golden.py:35`; `eval/golden.py:184`; `eval/golden.py:187`)

8. **Perguntas de lei: N = 0.**

   O anexo fixa fonte, prioridade, apresentação, ausência de badge e proibições, além de declarar “Nenhuma lei nova”: **[nao verificado]** como registro em arquivo desta cópia. As escolhas restantes são técnicas; seleção, vitalidade e regra atual já têm fontes identificadas. (`app/chamados/services/pergunta_no_app.py:61`; `app/core/juize

[truncado em 20000 de 20039 caracteres]

## 11/11 10:10 · sessao 1a4869192285 · - · - · origem=Ronald (tmux)

Censo direto, somente leitura; testes não executados, estado verde atual [nao verificado]. [nao verificado]

**Consulta e leitores**

- `consulta_do_app` deriva listas novas, preserva ordem por ID e separa `pendentes` por `respondida_em is None` **e** `disputa.fechada_em is None`; demais perguntas vivas vão ao histórico. Fonte anterior: `perguntas_do_app → q_vivas + pergunta_viva(C6)`. `app/chamados/services/pergunta_no_app.py:45`, `app/chamados/services/pergunta_no_app.py:61`, `app/chamados/models.py:3326`, `app/chamados/juizes.py:839`.
- **7 sítios de consumo em produção**: dois badges (`app/api/views_core.py:314`, `app/api/views.py:325`), GET chamados (`app/api/views.py:1005`), GET perguntas (`app/api/views.py:2543`), dashboard (`app/core/views.py:253`), aviso de pendência (`app/chamados/services/aviso_pendencia.py:9`), cobrança (`app/chamados/services/cobranca.py:48`).
- A consulta não contém `status_local__in=VIVOS`; esse filtro pertence à contagem de **chamados com followup DP não lido**, nos badges. `VIVOS = aberto, em_analise, registrado`, diferente de `COBRANCA`. `app/api/views_core.py:309`, `app/api/views.py:320`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`.

**Escritores diretos dos campos que separam pendentes/histórico**

| Campo/operação | Sítios |
|---|---|
| Resposta do colaborador | `responder_pergunta_disputa_core`: atribui e salva `respondida_em`. `app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1508` |
| Resposta automática | `responder_automaticamente`. `app/chamados/services/disputa_emissao.py:1653` |
| Recuperação de carimbo pela trilha | `carimbar_resposta_pela_trilha`. `app/chamados/services/disputa_emissao.py:1674` |
| Carimbo com resposta já textual | `PerguntaDisputa.carimbar`, quando `resposta` solicitado e texto presente. `app/chamados/models.py:3130`, `app/chamados/models.py:3152` |
| Voltar a pendente | `reabrir_para_correcao`: monta `respondida_em=None`, executa `qs.update(**campos)` ou delega ao descarimbo em lote. `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/services/disputa_emissao.py:1629` |
| Ressuscitar perguntas | `ChamadoColaborador.ressuscitar_perguntas` delega a `descarimbar_em_lote(...respondida_em=None)`; este faz update dinâmico. `app/chamados/models.py:1566`, `app/chamados/models.py:3264` |
| Nascimento da pergunta | `_nascer` recebe `**campos`, constrói e insere; inclui assinaturas recebidas. Campo nullable por definição. `app/chamados/models.py:2448`, `app/chamados/models.py:3014` |
| Fechar disputa | `DisputaSupervisao.fechar`: `fechada_em=now`, seguido de save. `app/chamados/models.py:1932` |
| Reabrir disputa | `reabrir_admin` e `reabrir_sistema`: `fechada_em=None`, seguido de save. `app/chamados/models.py:2149`, `app/chamados/models.py:2173` |
| Nascer disputa aberta | Default nullable; criações do emissor. `app/chamados/models.py:1757`, `app/chamados/services/disputa_emissao.py:587`, `app/chamados/services/disputa_emissao.py:1123`, `app/chamados/services/disputa_emissao.py:1358` |

Atribuições de `respondida_em` que **não persistem**: simulação em cópia (`_universo_da_ressuscitacao`) e restauração da memória quando carimbar recusa. `app/chamados/models.py:1508`, `app/chamados/models.py:3163`.

Vitalidade C6 também depende de `validada_em`, tranca da competência, `via_resolucao`, `materializada_em`, texto da resposta, célula/ata/veredito e testemunha do fato; o conjunto de escritores transitivos de todos esses insumos não foi encerrado neste recorte [nao verificado]. As dependências aparecem em `app/chamados/juizes.py:1324`. O registro existente reconhece oito módulos-porta de `PerguntaDisputa`, incluindo modelo, emissor, materialização, fio, reconciliador, lavra, relavra e validação. `app/core/portas.py:393`.

**Não lido do colaborador**

- Campo real: `FollowupChamado.lido_colaborador_em`, nullable. Criar nota sem carimbo faz nascer uma fonte não lida. `app/chamados/models.py:1673`, `app/chamados/services/nota.py:88`, `app/chamados/services/nota.py:103`.
- Escritor do carimbo: `nota.marcar_lidas`, usando mapa dinâmico `LADOS`; atualiza apenas registros ainda nulos. `app/chamados/services/nota.py:59`, `app/chamados/services/nota.py:126`.
- **3 chamadores com lado colaborador**: GET nativo marca **todos** followups DP daquele colaborador; endpoint web de um chamado; endpoint web em lote. `app/api/views.py:1050`, `app/chamados/views.py:1135`, `app/chamados/views.py:1150`.
- Chamador com lado DP modifica **outro campo**, `lido_dp_em`. `app/chamados/views.py:880`, `app/chamados/services/nota.py:59`.
- Leitores em produção: dois badges; serialização nativa `novo`/contador por followup; `_nao_lido` web/contador por chamado; duas posições no card de atendimento. `app/api/views_core.py:311`, `app/api/views.py:322`, `app/api/views.py:1128`, `app/chamados/views.py:603`, `app/templates/chamados/partials/_card_atendimento.html:51`, `app/templates/chamados/partials/_card_atendimento.html:59`.
- Outros atos que eliminam fonte: `nota.apagar_em_ensaio` apaga notas sob guarda de banco de ensaio; excluir o chamado faz cascade da FK. `app/chamados/services/nota.py:136`, `app/chamados/models.py:1668`.
- Registro da porta de criação/leitura/apagamento: `app/core/portas.py:239`.

**`status_local`: escritores que alteram entrada/saída de VIVOS**

- Campo/default `'aberto'`. `app/chamados/models.py:52`.
- **Escritor persistente de transição**: `_gravar_estado`, atribuição em `:228`, `save(update_fields=campos)` em `:296`. `app/chamados/models.py:228`, `app/chamados/models.py:296`.
- Gates locais que delegam ao núcleo: `_aplicar`, `reconciliar`, `cancelar`, `analisar`, `reabrir`, `encerrar`, `encerrar_por_competencia_trancada`. `app/chamados/models.py:450`, `app/chamados/models.py:654`, `app/chamados/models.py:788`, `app/chamados/models.py:806`, `app/chamados/models.py:1442`, `app/chamados/models.py:1619`, `app/chamados/models.py:1639`.
- **Nascimento idempotente**: `abrir` monta `_def['status_local']`, agrega defaults e passa ao `get_or_create`. `app/chamados/models.py:938`, `app/chamados/models.py:971`.
- **Nascimento direto**: `criar` passa `status_local=status` ao `objects.create`. A construção anterior `_nascente` é só memória. `app/chamados/models.py:1070`, `app/chamados/models.py:1095`.
- **Simulação sem persistência**: `_vivas_entre` aplica e restaura o status em memória. `app/chamados/models.py:356`, `app/chamados/models.py:371`.
- Registro existente da porta de ciclo: `app/core/portas.py:218`.
- Migrações de dados, fora do runtime: `aberto↔registrado` e `cancelado↔superado`. `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:33`.

**Contratos já presentes, sem executar**

- Selo AST de escritores de status permite precisamente `_gravar_estado`, `_vivas_entre`, `criar`, `abrir`; distingue memória e persistência, examina update/create/defaults/bulk/setattr/SQL e declara limites de resolução de tipos. `app/chamados/tests/test_selo_status_so_pelas_portas.py:10`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:37`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:340`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:449`.
- Selo da nota: nenhum escritor fora da porta e idempotência do marcar lidas. `app/chamados/tests/test_porta_nota_do_fio.py:44`, `app/chamados/tests/test_porta_nota_do_fio.py:207`.
- Matriz ORM compara endpoint de perguntas, dashboard e **ambos** kernels, incluindo dicionário completo do badge; prova que vermelho conta chamados e não followups. `app/chamados/tests/test_e3_app_orm.py:28`, `app/chamados/tests/test_e3_app_orm.py:62`.
- Contratos de leitores: aviso/cobrança/badges mesma pendente; lista nativa pendentes + histórico; frescor após tranca/toque/reabertura. `app/chamados/tests/test_e3_v4_orm.py:20`, `app/chamados/tests/test_e3_v4_orm.py:42`, `app/chamados/tests/test_e3_v4_orm.py:101`.
- Outros leitores de teste explícitos da consulta: `app/chamados/tests/test_esmeril2_left_join.py:19`, `app/chamados/tests/test_fio_le_o_juiz.py:267`, `app/core/tests/test_dashboard_badge.py:45`; contratos AST em `app/chamados/tests/test_contract_pergunta_viva.py:68`, `app/chamados/tests/test_pergunta_no_app.py:168`; ferramentas de mutação em `app/chamados/tests/e3_mutantes.py:23`.

## 11/11 10:08 · sessao 984214e9a557 · - · - · origem=Ronald (tmux)

Censo de autoridades e decisões, somente leitura:

1. **Não há juiz registrado para o rótulo/count “o que o app mostra ao colaborador agora”.** O registro de chamados contém C6 `pergunta_viva`, C13 `retencao`, C15 `fio_vivo`, C19 `tarefa_do_dia`, C20 `dia_da_pergunta_trancado`, mas nenhum para o badge ou `consulta_do_app`. `app/core/juizes.py:272`, `app/core/juizes.py:278`, `app/core/juizes.py:285`, `app/core/juizes.py:287`, `app/core/juizes.py:291`, `app/core/juizes.py:292`.

2. **A tabela `JUIZES.md` também não declara esse juiz.** Ela se define como lookup, aponta o registro vivo em `core/juizes.py`, declara que o código vivo manda e lista os juízes de chamados sem badge. `app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:11`, `app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:59`, `app/docs/JUIZES.md:78`.

3. **Autoridade existente da seleção apresentada:** `consulta_do_app(colab)` retorna exatamente `{'pendentes': [pergunta], 'historico': [pergunta]}`; lê `perguntas_do_app(colab, em_lote=True)` e separa pendentes por `respondida_em is None and disputa.fechada_em is None`, preservando ordem por id. O serviço declara que API, badge dos dois kernels e dashboard contam a mesma lista. `app/chamados/services/pergunta_no_app.py:3`, `app/chamados/services/pergunta_no_app.py:61`, `app/chamados/services/pergunta_no_app.py:64`, `app/chamados/services/pergunta_no_app.py:69`, `app/chamados/services/pergunta_no_app.py:75`.

4. **A vitalidade continua pertencendo ao C6:** `perguntas_do_app` usa `PerguntaDisputa.q_vivas()`, identidade do colaborador e `pergunta_viva`; `consulta_do_app` apenas particiona a apresentação. `app/chamados/services/pergunta_no_app.py:45`, `app/chamados/services/pergunta_no_app.py:49`, `app/chamados/services/pergunta_no_app.py:56`, `app/chamados/services/pergunta_no_app.py:58`, `app/core/juizes.py:278`.

5. **`estado_app` não é este problema:** no situacional significa “sem device ativo”, “instalou, nunca bateu”, “bateu e parou” ou “ativo”; não descreve perguntas, mensagens ou badge. `app/colaboradores/services/situacional.py:191`.

Decisões técnicas sugeridas para o construtor — propostas, não implementação:

| Candidata | Pró e limite com evidência |
|---|---|
| **Wrapper de projeção junto de `consulta_do_app`, em `chamados/services/pergunta_no_app.py` — recomendação** | Mantém seleção e apresentação próximas, sem depender de uma view HTTP; pode receber `colab`, chamar uma vez `consulta_do_app`, mover para si a query não lidos e devolver listas + números + badge. A seleção já mora nesse serviço por ser o leitor do universo do app; os kernels hoje repetem query e regra. `app/chamados/services/pergunta_no_app.py:64`, `app/api/views_core.py:305`, `app/api/views_core.py:313`, `app/api/views.py:316`, `app/api/views.py:324`. |
| Acrescentar badge à própria `consulta_do_app` | Menor número de funções, mas altera contrato de retorno e adiciona query de não lidos a todos os consumidores. Há testes de igualdade **exata** com as duas chaves e teto de nove queries; portanto é candidata mais invasiva. `app/chamados/tests/test_e3_v4_orm.py:85`, `app/chamados/tests/test_e3_v4_orm.py:91`, `app/chamados/tests/test_e3_v4_orm.py:98`. |
| Novo módulo de apresentação em `chamados/services/` | Nome próprio separa a projeção da seleção e pode conservar o contrato atual; acrescenta arquivo e indireção. Existência/aceitação desse arquivo no escopo literal do anexo precisa ser tratada como decisão técnica de implementação, não lei nova. Base: `app/chamados/services/pergunta_no_app.py:61`; restrição de escopo do anexo: **[nao verificado]**. |
| Helper puro que recebe duas contagens | Facilita testar prioridade/cor, mas sozinho não atende o painel sem alguém produzir a query não lidos. Para evitar query própria no painel, precisaria de outro montador, ou da função pública aceitando `colab`. Query atual: `app/api/views_core.py:305`; proibição do anexo: **[nao verificado]**. |
| Função em `api/views_core.py` ou `api/views.py` | Permite extrair a regra no local atual, mas o painel passaria a importar camada HTTP e ainda seria preciso escolher um dos kernels como dono; os dois possuem hoje a regra. `app/api/views_core.py:313`, `app/api/views.py:324`. |

**Forma recomendada do retorno do wrapper**, como proposta compatível: `{'pendentes': [...], 'historico': [...], 'questionarios_pendentes': int, 'chamados_nao_lidos': int, 'chamados_badge': {'count': int, 'cor': str, 'rotulo': str} | None}`. Não calcular novamente rótulo nem count nos leitores; `None` continua representando ausência de badge. Contrato atual dos kernels: `app/api/views_core.py:314`, `app/api/views_core.py:316`, `app/api/views_core.py:318`, `app/api/views_core.py:320`; contrato de listas: `app/chamados/services/pergunta_no_app.py:75`. O nome da função e o shape interno são decisão técnica, não comprovados como já escolhidos: **[nao verificado]**.

**Cuidados concretos com os selos existentes:**

- Matriz E3 testa os dois kernels, dashboard, endpoint, duas perguntas na mesma disputa e prioridade do amarelo sobre vermelho; vermelho conta chamados, não followups. `app/chamados/tests/test_e3_app_orm.py:28`, `app/chamados/tests/test_e3_app_orm.py:38`, `app/chamados/tests/test_e3_app_orm.py:43`, `app/chamados/tests/test_e3_app_orm.py:47`, `app/chamados/tests/test_e3_app_orm.py:62`.
- Mutantes atuais procuram literalmente `len(consulta_do_app(... )['pendentes'])` nos dois kernels; a extração deve atualizar esses alvos, preservando a mordida contra contar disputas. `app/chamados/tests/e3_mutantes.py:23`, `app/chamados/tests/e3_mutantes.py:25`.
- Contrato AST exige que `api_disputa_perguntas` continue lendo `consulta_do_app`, e que a consulta continue delegando a `perguntas_do_app`/C6. Mantendo wrapper separado e endpoints de perguntas intactos, esse contrato pode permanecer. `app/chamados/tests/test_contract_pergunta_viva.py:62`, `app/chamados/tests/test_contract_pergunta_viva.py:68`, `app/chamados/tests/test_contract_pergunta_viva.py:75`.
- A chamada `api/views.py:1004` alimenta o agrupamento por chamado usando pendentes + histórico, depois marca notas do DP como lidas; a chamada `:2542` alimenta perguntas, histórico e `em_disputa`, sem calcular badge. Extração apenas da regra não exige alterá-las. `app/api/views.py:1004`, `app/api/views.py:1040`, `app/api/views.py:1050`, `app/api/views.py:2542`, `app/api/views.py:2571`, `app/api/views.py:2573`, `app/api/views.py:2581`.

**Golden atual:** existem três perguntas por colaborador/dia sobre presença de pergunta no app, ligadas a `dia_do_colab`; não existe nesse arquivo caso do rótulo/count “agora”. `eval/golden.py:35`, `eval/golden.py:184`, `eval/golden.py:187`, `eval/oraculo.py:93`. A inclusão de uma pergunta “agora” é requisito do anexo, não execução nesta cópia: **[nao verificado]**.

**Perguntas de lei: 0**, conforme o anexo que já fixa fonte, prioridade, cores existentes, contagem por pergunta e ausência de badge; a autoridade de vitalidade continua C6. `app/chamados/services/pergunta_no_app.py:61`, `app/api/views_core.py:315`, `app/api/views_core.py:317`, `app/core/juizes.py:278`; aval/“Nenhuma lei nova” do anexo: **[nao verificado]**.

## 11/11 10:08 · sessao 950c845836fc · - · - · origem=Ronald (tmux)

3. View/contexto/contador:

- Única view encontrada renderizando o partial: `painel_colaborador(request, cid)`, protegida por login e ação `ver_cadastro`; rota `<int:cid>/painel/`. (`app/colaboradores/views.py:914`, `app/colaboradores/views.py:917`, `app/colaboradores/views.py:988`, `app/colaboradores/urls.py:48`)
- Mensagens chegam como `chamados`: `ChamadoColaborador.objects.filter(colaborador=colab).prefetch_related('followups').order_by('-criado_em')[:5]`. O comentário diz “abertos”, mas o filtro executável não restringe status. (`app/colaboradores/views.py:936`, `app/colaboradores/views.py:937`)
- A view passa `chamados`; não passa `consulta_do_app`, `pendentes`, `historico`, contagem de perguntas ou rótulo do app. Os outros dados explícitos são colaborador, escala, retrato/portas/legenda, total de batidas, batidas do mês, mês atual e CSRF. (`app/colaboradores/views.py:988`)
- Badge atual da aba Mensagens é `{{ chamados|length }}`, condicionado a haver chamados; portanto conta os até cinco chamados carregados, sem distinguir leitura, perguntas ou status. (`app/templates/colaboradores/partials/painel_colaborador.html:39`, `app/colaboradores/views.py:937`)
- Aba Mensagens contém formulário HTMX de mensagem rápida e include `msgs_lista.html`; a lista mostra assunto/data do chamado e até cinco followups por chamado, com conteúdo truncado em 80 caracteres. (`app/templates/colaboradores/partials/painel_colaborador.html:188`, `app/templates/colaboradores/partials/painel_colaborador.html:198`, `app/templates/colaboradores/partials/msgs_lista.html:4`, `app/templates/colaboradores/partials/msgs_lista.html:7`, `app/templates/colaboradores/partials/msgs_lista.html:10`)
- Após enviar, `abrir_chamado_rapido` cria/reusa chamado `mensagem_dp` em `VIVOS`, anexa nota, envia push e devolve somente `msgs_lista.html`, com a mesma consulta limitada a cinco. O `hx-target="#msgs-lista"` não atualiza o botão da aba. (`app/chamados/views.py:973`, `app/chamados/views.py:976`, `app/chamados/views.py:986`, `app/chamados/views.py:989`, `app/chamados/views.py:997`, `app/chamados/views.py:1000`, `app/templates/colaboradores/partials/painel_colaborador.html:189`)

5. Cascas e grafo:

- Correção da premissa: o GET do painel usa **fetch + innerHTML**, depois `htmx.process(corpo)`; HTMX processa os controles injetados. (`app/templates/core/_drawer_generico.html:19`, `app/templates/core/_drawer_generico.html:26`, `app/templates/core/_drawer_generico.html:33`)
- `base.html → core/_drawer_generico.html` e `base_app.html → core/_drawer_generico.html`; ambas carregam HTMX. (`app/templates/base.html:399`, `app/templates/base_app.html:154`, `app/templates/base.html:19`, `app/templates/base_app.html:19`)
- Fluxo da lista: `colaboradores/lista.html` estende `base.html` e inclui `_lista_tabela.html`; clique da linha chama `abrirPainelColab`, que chama `abrirDrawer(..., '/colaboradores/'+cid+'/painel/', 1100)`. (`app/templates/colaboradores/lista.html:1`, `app/templates/colaboradores/lista.html:57`, `app/templates/colaboradores/partials/_lista_tabela.html:16`, `app/templates/core/_drawer_generico.html:89`, `app/templates/core/_drawer_generico.html:104`)
- Há ramo legado `#col-painel`, também por fetch, com inserção in-place. (`app/templates/core/_drawer_generico.html:95`, `app/templates/core/_drawer_generico.html:99`)
- Fluxo do retrato: `folha/retrato_folha.html` estende `base.html`, inclui `_retrato_lista.html`; o nome abre a mesma rota, levando `mes/ano`. A versão parcial do retrato também inclui a lista. (`app/templates/folha/retrato_folha.html:1`, `app/templates/folha/retrato_folha.html:53`, `app/templates/folha/_retrato_lista.html:61`, `app/templates/folha/_retrato_parcial.html:7`)
- Fluxo Raio-X: barra abre drawer com `raiox_href`; `retrato_raiox` renderiza `_retrato_raiox.html`, cujas linhas abrem a mesma rota do painel com `mes/ano`. (`app/templates/folha/_retrato_barra.html:71`, `app/templates/folha/_retrato_barra.html:73`, `app/folha/views_retrato.py:510`, `app/templates/folha/_retrato_raiox.html:68`, `app/templates/folha/_retrato_raiox.html:70`)
- O partial do painel não estende casca; inclui `_cabecalho_drawer.html`, condicionalmente `_retrato_o_que_fazer.html`, e `msgs_lista.html`. O bloco do retrato inclui `_led.html` e `_retrato_legenda.html`. (`app/templates/colaboradores/partials/painel_colaborador.html:1`, `app/templates/colaboradores/partials/painel_colaborador.html:8`, `app/templates/colaboradores/partials/painel_colaborador.html:26`, `app/templates/colaboradores/partials/painel_colaborador.html:198`, `app/templates/folha/_retrato_o_que_fazer.html:36`, `app/templates/folha/_retrato_o_que_fazer.html:71`)
- Os hosts encontrados do painel usam `base.html` fixo; a infraestrutura de drawer existe nas duas cascas. A seleção global `base_tpl` depende de `modo_app`, mas não altera `{% extends 'base.html' %}` fixo desses hosts. (`app/templates/colaboradores/lista.html:1`, `app/templates/folha/retrato_folha.html:1`, `app/templates/base_app.html:154`, `app/core/context_processors.py:205`, `app/core/context_processors.py:210`)

Selo existente/duas cascas:

- Há testes do painel real via client GET, do bloco do retrato e do cabeçalho compartilhado; não constituem selo API × painel. (`app/colaboradores/tests/test_retrato_no_drawer.py:151`, `app/colaboradores/tests/test_retrato_no_drawer.py:167`, `app/colaboradores/tests/test_retrato_no_drawer.py:182`, `app/ponto/tests/test_drawer_ausencia_no_molde.py:81`)
- A casa já tem padrão de smoke renderizando as duas cascas, com verificação dos templates realmente usados e sessão `via_app/plataforma_sessao`; esse exemplo mede o espelho, não o painel. (`app/core/tests/test_smoke_chromium.py:151`, `app/core/tests/test_smoke_chromium.py:155`, `app/core/tests/test_smoke_chromium.py:160`, `app/core/tests/test_smoke_chromium.py:172`)
- Para o construtor: renderizar somente a resposta de `/colaboradores/<cid>/painel/` duas vezes não demonstra duas cascas, pois é partial sem extends; o selo deve medir a integração com cada casca que inclui o drawer. (`app/colaboradores/views.py:988`, `app/templates/colaboradores/partials/painel_colaborador.html:1`, `app/templates/base.html:399`, `app/templates/base_app.html:154`)

## 11/11 10:05 · sessao f575c8ee2b9b · O169-FECHADA · verificador · origem=Code

**A volta 5 ainda não cumpre integralmente o aval:** o ramo trancado fica parado sem relavrar a via, e o apply usa uma instância carregada antes da trava. O aval exige relavra no primeiro caso; a função apenas pula essas linhas. (`app/docs/CORTES.md:435`; `app/ponto/services/cluster_espurio.py:256`; `app/ponto/services/cluster_espurio.py:264`; `app/ponto/services/cluster_espurio.py:267`)

1. **Escritores e leitores dos campos.**

   | Campo/valor | Quem escreve | Quem lê |
   |---|---|---|
   | `status_local` | O caminho novo é tratamento → `_renascer_por_premissa_viva` → `reabrir` → `_gravar_estado`. O núcleo atribui e salva; `models.py` é porta declarada. (`app/ponto/services/cluster_espurio.py:273`; `app/chamados/models.py:1019`; `app/chamados/models.py:1442`; `app/chamados/models.py:228`; `app/chamados/models.py:296`; `app/core/portas.py:218`) | Contador seleciona encerrados; relatório mostra o estado; painel conta estados; vigia compara estado persistido com auditoria. (`app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:251`; `app/chamados/services/painel.py:213`; `app/chamados/services/vigia_loop.py:117`) |
   | `via_encerramento` | Mesmo núcleo: terminal recebe a via; renascimento limpa para `''`. (`app/chamados/models.py:245`; `app/chamados/models.py:249`) | Contador seleciona `RECONCILIAR`; renascimento recusa cascata; desarquivamento e reabertura de competência distinguem a via de tranca. (`app/ponto/services/cluster_espurio.py:112`; `app/chamados/models.py:1007`; `app/chamados/views_arquivar.py:64`; `app/chamados/services/competencia_trancada.py:259`) |
   | `resolvido_em` | `_gravar_estado` carimba encerramento, preserva encerrado→encerrado e limpa no renascimento. (`app/chamados/models.py:242`; `app/chamados/models.py:248`) | Guarda contra desfazer decisão administrativa sem fato novo; estatísticas de resolvidos. (`app/chamados/models.py:1342`; `app/chamados/services/fila.py:109`) |
   | Followup/nota | Renascimento chama `nota.anexar_ocorrencia`, porta declarada de `FollowupChamado`, com tipo `renascido`. (`app/chamados/models.py:1021`; `app/chamados/services/nota.py:75`; `app/chamados/services/nota.py:103`; `app/core/portas.py:241`) | Histórico lê conteúdo e autor; detalhe/painel também consomem followups. (`app/chamados/services/historico.py:80`; `app/chamados/views.py:352`; `app/chamados/services/painel.py:362`) |
   | `LogAuditoria`: `estado_chamado` e ação nova | Núcleo emite `estado_chamado`; tratamento emite `reabrir_legado_cluster_o169`; ambos usam o escritor `core.observ.evento`. (`app/chamados/models.py:292`; `app/ponto/services/cluster_espurio.py:218`; `app/ponto/services/cluster_espurio.py:277`; `app/core/observ.py:94`; `app/core/observ.py:134`) | Histórico e vigia leem `estado_chamado`. O teste lê especificamente a ação nova; o histórico filtra apenas suas ações declaradas. (`app/chamados/services/historico.py:44`; `app/chamados/services/historico.py:86`; `app/chamados/services/vigia_loop.py:114`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`) |
   | `CelulaDia.ata` | Cartório produz a ata; persistência passa pela porta da célula, inclusive lavra e precedência. O tratamento novo apenas lê. (`app/ponto/services/cartorio.py:1811`; `app/ponto/portas/celula.py:974`; `app/ponto/portas/celula.py:1033`; `app/ponto/portas/celula.py:1076`; `app/core/portas.py:112`) | `_leitura_da_celula`, compartilhada pelo contador, abertura e fechamento, lê órfãs/lâmpadas. (`app/ponto/services/cluster_espurio.py:27`; `app/ponto/services/cluster_espurio.py:47`; `app/ponto/services/cluster_espurio.py:74`; `app/ponto/services/cluster_espurio.py:126`) |
   | `PeriodoFechado` | `trancar_periodo` e `reabrir_periodo`, no módulo declarado. O diff não o escreve. (`app/ponto/services/fechamento.py:1408`; `app/ponto/services/fechamento.py:1437`; `app/core/portas.py:501`) | A casca do chamado consulta `competencia_trancada`; C20 consulta `periodo_trancado`. Ambos reconhecem a tranca por `reaberto_em IS NULL`; `ativo` é documental. (`app/chamados/models.py:1044`; `app/ponto/services/fechamento.py:1464`; `app/ponto/services/fechamento.py:1480`) |

   **Não há segundo escritor introduzido pelo diff para esses estados:** as alterações passam pelos escritores acima. O teste que escreve diretamente `CelulaDia.ata` é preparação de fixture, não o tratamento produtivo. (`app/ponto/services/cluster_espurio.py:273`; `app/ponto/services/cluster_espurio.py:277`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:56`)

   Os valores persistidos são `RECONCILIAR → 'sistema'` e tranca → `'competencia_trancada'`; `relavrar_via_tranca` é ação do relatório. (`app/chamados/catalogo/motor.py:88`; `app/chamados/catalogo/motor.py:92`; `app/chamados/catalogo/motor.py:95`; `app/ponto/services/cluster_espurio.py:219`)

2. **Juízes e derivações.**

   A função nova não implementa outro predicado de assinatura, universo, dia, competência ou recusa: usa contador, `dia_do_fato`, casca de tranca e `recusa_da_reabertura`. A porta continua responsável pelo veto e demais guardas. (`app/ponto/services/cluster_espurio.py:245`; `app/ponto/services/cluster_espurio.py:249`; `app/ponto/services/cluster_espurio.py:251`; `app/ponto/services/cluster_espurio.py:254`; `app/chamados/models.py:1015`)

   **Há duas semânticas preexistentes que precisam permanecer explícitas:** C20, registrado como “dia trancado”, desconta toque humano posterior; `_dia_em_competencia_trancada`, usada pelo relatório e renascimento, consulta a competência trancada sem esse desconto. Portanto a coluna não significa exatamente o C20 de `JUIZES.md`. (`app/docs/JUIZES.md:86`; `app/core/juizes.py:292`; `app/chamados/juizes.py:1033`; `app/chamados/juizes.py:1041`; `app/chamados/models.py:1038`)

   Também pode haver diferença entre **o dia cuja assinatura foi contada** e **a competência usada na tranca**: o primeiro usa `dia_do_fato(fortes=True)`; `competencia_do_chamado` escolhe o fato mais recente, podendo incluir ausência e perguntas vivas. Isso é reaproveitamento de autoridade existente, mas pode classificar outra competência daquela sugerida pela coluna `dia`. (`app/ponto/services/cluster_espurio.py:114`; `app/ponto/services/cluster_espurio.py:251`; `app/chamados/juizes.py:2275`; `app/chamados/juizes.py:2283`; `app/chamados/juizes.py:2290`)

3. **Relógio: existe leitura decisória indireta fora da borda.**

   Não há `now/today` novo diretamente no tratamento. Contudo, inclusive no DRY, há caminho alcançável: tranca → competência → perguntas vivas → C6 sem `hoje` → `_dentro_da_janela_viva` → `date.today()`. A consulta de perguntas não restringe o módulo do chamado. (`app/ponto/services/cluster_espurio.py:249`; `app/chamados/models.py:1043`; `app/chamados/juizes.py:2286`; `app/chamados/juizes.py:2264`; `app/chamados/juizes.py:2267`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:577`)

   O apply também pode alcançar esse caminho ao ressuscitar perguntas. Os carimbos automáticos de persistência são outra coisa, preexistente. A ocorrência dessas configurações nos três chamados de produção é **[nao verificado]**. (`app/chamados/models.py:1451`; `app/chamados/models.py:1509`; `app/chamados/models.py:68`; `app/chamados/models.py:1674`)

4. **Travas, deadlock e TOCTOU.**

   - `abrir`: `atomic → trava C → leitura → renascer`. O novo tratamento: `atomic → leitura de chamado+colaborador → trava C → contador → renascer`. Portanto não são sequências equivalentes: o objeto usado pela porta nasceu antes da trava. O contrato de `trava_na_ordem` manda reler depois. (`app/chamados/models.py:902`; `app/chamados/models.py:905`; `app/chamados/models.py:906`; `app/chamados/models.py:922`; `app/ponto/services/cluster_espurio.py:266`; `app/ponto/services/cluster_espurio.py:267`; `app/chamados/models.py:2638`)
   - `resposta_tardia` também não possui a sequência proposta: lê estado/célula e chama renascimento diretamente; o escritor adquire as travas posteriormente. (`app/chamados/services/resposta_tardia.py:35`; `app/chamados/services/resposta_tardia.py:44`; `app/chamados/services/resposta_tardia.py:46`; `app/chamados/models.py:290`)
   - **TOCTOU concreto:** enquanto espera C, o colaborador pode passar de ativo para desligado. A releitura do contador não filtra situação nem substitui o colaborador em cache; o renascimento pode continuar vendo `ativo`. `reabrir` só atualiza determinados campos quando detecta mudança de status, sem renovar esse colaborador. (`app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:267`; `app/ponto/services/cluster_espurio.py:270`; `app/chamados/models.py:1005`; `app/chamados/models.py:1373`)
   - `--esperados` valida somente a fotografia inicial. Sob trava verifica pertinência individual, não igualdade do universo inteiro; entradas desaparecidas são puladas e novas entradas não abortam. Cada linha tem sua transação, portanto falha posterior pode deixar aplicações anteriores commitadas. (`app/ponto/services/cluster_espurio.py:259`; `app/ponto/services/cluster_espurio.py:263`; `app/ponto/services/cluster_espurio.py:266`; `app/ponto/services/cluster_espurio.py:270`)
   - Não encontrei inversão nova demonstrada nesse corpo: a leitura anterior é comum, e o núcleo adquire C→D→Chamado. Ausência universal de deadlock é **[nao verificado]**. A trava C também não torna atômica a leitura de `PeriodoFechado`, escrito antes dos ganchos que tratam chamados. (`app/ponto/services/cluster_espurio.py:267`; `app/chamados/models.py:290`; `app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/ponto/services/fechamento.py:1408`; `app/ponto/services/fechamento.py:1428`)

5. **Campos com dois significados.**

   - **`pks`/`numero`: coerentes nos leitores encontrados.** `numero=len(pks)`; tratamento usa os PKs e alarme usa o número. `universo.total` possui outro denominador: todos os encerrados pela via selecionada, antes de exigir assinatura. (`app/ponto/services/cluster_espurio.py:126`; `app/ponto/services/cluster_espurio.py:127`; `app/ponto/services/cluster_espurio.py:149`; `app/ponto/services/cluster_espurio.py:246`; `app/ponto/management/commands/detectar_cluster_espurio.py:100`)
   - **`trancada`: mesma função da guarda de renascimento, em dados estáveis.** Pode ficar temporalmente velha, pois define a ação antes do apply; linhas inicialmente trancadas sequer são reavaliadas dentro dele. (`app/ponto/services/cluster_espurio.py:249`; `app/ponto/services/cluster_espurio.py:264`; `app/chamados/models.py:1009`)
   - **`aplicado/aplicadas` contam retornos verdadeiros do renascimento**, não tentativas nem todas as escritas. Uma recusa pode escrever auditoria; a recusa por irmão vivo pode migrar perguntas e anexar nota, retornando `False`. (`app/ponto/services/cluster_espurio.py:273`; `app/ponto/services/cluster_espurio.py:282`; `app/chamados/models.py:1017`; `app/chamados/models.py:1415`; `app/chamados/models.py:1427`; `app/chamados/models.py:1436`)
   - **`recusa` é incompleta e pode ficar desatualizada.** Consulta só guardas de `reabrir`; desligado/cascata/tranca são guardas adicionais de renascimento. Após `False`, o tratamento não atualiza o motivo. Assim, desligado pode sair `ativo=False`, `recusa=None`, `aplicado=False`. (`app/ponto/services/cluster_espurio.py:254`; `app/ponto/services/cluster_espurio.py:255`; `app/ponto/services/cluster_espurio.py:274`; `app/chamados/models.py:1005`; `app/chamados/models.py:1007`)

6. **Selos e mutantes.**

   Os dois harnesses exigem `errors == []` e alguma falha de asserção; erro de infraestrutura registrado em `errors` não é aceito como morte, e execução sem falha também não passa. O legado acrescenta controle da fonte intacta. Contudo, aceitam qualquer asserção falhada no caso, inclusive de fixture, sem identificar qual asserção matou. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:286`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:287`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:295`; `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:143`; `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:144`)

   As mordidas dos oito mutantes do legado são concretas:

   | Mutante | Asserção |
   |---|---|
   | `fechar_por_idade` | Chamado continua VIVO após cron. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:175`) |
   | `relavrar_via_em_nao_trancada` | `aplicadas == 1`. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:109`) |
   | `reabre_trancada` | Ação e classificação devem ser `relavrar_via_tranca, True`. **Morde o relatório; a porta real ainda barraria reabrir.** (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:132`; `app/chamados/models.py:1009`) |
   | `reabrir_fora_da_porta` | Exige mais uma linha `estado_chamado`. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:113`) |
   | `apply_sem_esperados` | Exige aborto para expectativa ausente/diferente. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:145`) |
   | `dry_escreve` | Exige linha não aplicada, contador zero e foto intacta. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:95`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:98`) |
   | `universo_proprio` | Só o chamado assinado pode entrar. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:154`) |
   | `forca_desligado` | Exige não aplicação e foto intacta. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:164`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:166`) |

   Os 36 mutantes do contador/comando também têm asserções correspondentes:

   | Mutante(s) | Asserção |
   |---|---|
   | `contador_cego`, `recorta_por_idade` | `numero == 1`. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:173`) |
   | `pks_divergem_do_numero` | `pks == [1]`. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:174`) |
   | `assinatura_invertida` | Sem assinatura conta zero. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:179`) |
   | `nao_medido_vira_assinatura` | Não medido conta zero. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:333`) |
   | `sem_filtro_via` | Vias excluídas contam zero. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:201`) |
   | `via_digitada` | Constante substituída por via sintética continua contando um. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:323`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:326`) |
   | `status_errado`, `sem_filtro_modulo` | Conta exatamente `len(ENCERRADOS)`. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:209`) |
   | `sem_filtro_colab` | Recorte do colaborador conta dois. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:352`) |
   | `conta_dias_em_vez_de_chamados` | Três chamados contam três. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:351`) |
   | `mistura_colaboradores` | Assinatura do outro colaborador não serve. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:340`) |
   | `legado_nao_contado`, `vazia_nao_contada`, `nula_nao_contada`, `via_nova_nao_contada` | Igualdade integral de `fora_do_universo`. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:225`) |
   | `rotulo_desconhecido_vazio` | Rótulos precisam estar preenchidos. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:232`) |
   | `fora_sem_filtro_colab` | Exclusões respeitam colaborador. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:257`) |
   | `sem_guarda_colaborador_fk` | Assinatura vem do colaborador correto. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:305`) |
   | `consulta_por_par` | Exige `k` consultas. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:319`) |
   | `troca_rotulos_humana_cascata`, `cascata_omite_ajuste_retroativo`, `cancelado_rotulado_desconhecido`, `superado_rotulado_desconhecido`, `legado_rotulado_desconhecido`, `vazia_rotulada_desconhecida`, `nula_rotulada_desconhecida` | Igualdade integral dos rótulos. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:250`) |
   | `produto_cartesiano` | Só células dos pares pedidos são lidas. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:273`) |
   | `recarrega_fk`, `reconsulta_par_da_fk` | Consulta somente o par faltante. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:284`) |
   | `esconde_universo_vazio` | Saída precisa declarar universo vazio. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:377`) |
   | `zero_vira_universo_vazio` | Zero achados não pode anunciar universo vazio. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:385`) |
   | `silencia_contador` | Saída contém o número medido. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:401`) |
   | `alarme_ignora_contador`, `apply_vence_alarme` | Exige `SystemExit`. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:391`) |
   | `alarme_so_maior` | Divergência para menos também exige alarme. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:419`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:422`) |

   **Lacunas relevantes:** remover a trava/releitura; usar colaborador desatualizado; trocar a classificação de tranca por uma que desconta toque humano; perder assinatura ou mudar tranca entre medida/apply; falhar após o primeiro PK; ignorar `--colab` no caminho legado; adulterar `recusa`, ator ou motivo da auditoria; tornar a comparação de esperados dependente da ordem. Os cenários novos não instrumentam concorrência, o teste de comando prepara um colaborador, as verificações da trilha são parciais e os applies felizes usam um PK. (`app/ponto/services/cluster_espurio.py:249`; `app/ponto/services/cluster_espurio.py:259`; `app/ponto/services/cluster_espurio.py:268`; `app/ponto/services/cluster_espurio.py:270`; `app/ponto/management/commands/detectar_cluster_espurio.py:55`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:106`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:158`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:184`)

   A execução da suíte Django do legado permanece **[nao verificado]**; as mordidas acima são demonstradas pela estrutura dos testes, não por execução contra banco. [nao verificado]

7. **Existe porta que troca via de chamado já encerrado sem reabrir? Sim, em sentido geral.**

   `transicionar('fechado', user)` permite `resolvido → fechado`, passa por `_aplicar` e grava via `admin`; ambos os estados pertencem a `ENCERRADOS`, sem passagem por vivo. O núcleo preserva `resolvido_em`. Portanto a frase absoluta de `PAROU_SEM_PORTA` é excessiva. (`app/chamados/catalogo/motor.py:30`; `app/chamados/catalogo/motor.py:102`; `app/chamados/models.py:128`; `app/chamados/models.py:199`; `app/chamados/models.py:450`; `app/chamados/models.py:242`; `app/ponto/services/cluster_espurio.py:220`)

   **Isso não entrega a operação exigida pelo aval:** relavrar somente a via para `competencia_trancada`, preservando o encerramento. O gate dedicado recusa fora de `VIVOS`; o núcleo tecnicamente aceita escrever via terminal, mas não há no caminho examinado um gate específico dessa relavra. (`app/chamados/models.py:1630`; `app/chamados/models.py:1639`; `app/chamados/models.py:240`; `app/chamados/models.py:245`)

8. **Outros defeitos materiais.**

   - **Trilha obrigatória não é garantida.** `evento` é best-effort, isola e engole falhas, retornando `None`; o tratamento ignora o retorno e marca aplicação. Pode haver chamado reaberto sem a auditoria nova; `estado_chamado` usa o mesmo emissor. (`app/core/observ.py:98`; `app/core/observ.py:133`; `app/core/observ.py:139`; `app/ponto/services/cluster_espurio.py:277`; `app/ponto/services/cluster_espurio.py:282`; `app/chamados/models.py:292`)
   - **“Nada se escreve” não é universal no apply parado/vazio.** `get_user_sistema()` roda antes de pular trancada

[truncado em 20000 de 21272 caracteres]

## 11/11 10:04 · sessao 0693a9df1757 · FIO-E0-V4-E4B · construtor · origem=Code

O leitor **já existia nesta base**; sua ausência não explica o log fornecido. Causa histórica do `OK`: **[nao verificado]**. [test_e4_backfill.py:209](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4_backfill.py:209)

- Alinhei a fonte a `fonte_real`, preservando tupla única, unicidade e patch condicional. [test_e4_backfill.py:212](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4_backfill.py:212)
- Acrescentei selo que exige a assertion de entrega, sem `precondicao:`. [protocolo.py:69](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm_protocolo.py:69)
- Memória: protocolo **12 OK**, c4v7 **19 OK**; três mutantes novos caíram por assertion, `rc=1`, sem errors. [recibo:50](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v12.txt:50)

Arquivos, comandos com rc e comandos GREEN/`--orm` para a sessão estão no [recibo](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v12.txt:110). ORM **[nao verificado]**.

LEI-AKITA com `padrao=n/a (instrumento)` registrada. Sem commit, pela proibição inicial. [recibo:130](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v12.txt:130) [nao verificado]

## 11/11 10:02 · sessao e62921812cba · FIO-E0-V4-E3 · sonda · origem=Code

[nao verificado]
```text
sonda_leitura: sonda_col1005_base.py [94d2db28b6538739] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

PERG 56283 mat=sim via=celula_absolveu cel_ver=concorde data=2026-11-10
PERG 56384 mat=sim via=celula_absolveu cel_ver=concorde data=2026-11-10
GERADOR_BASE col1005 []
gerador.n n 0
```

## 11/11 10:02 · sessao 021c9279df7a · - · - · origem=Ronald (tmux)

Correção da conclusão sobre relógio: **não cabe afirmar ausência de leitura indireta fora da borda**. O tratamento não lê relógio diretamente, mas pode alcançar `date.today()` em um juiz herdado — `app/ponto/services/cluster_espurio.py:249`, `app/chamados/juizes.py:577`.

Cadeia verificável, inclusive no DRY: `_dia_em_competencia_trancada → competencia_do_chamado → _dias_das_perguntas_vivas → pergunta_viva(p)` sem `hoje → _pergunta_viva_calc → _dentro_da_janela_viva → date.today()` — `app/chamados/models.py:1043`, `app/chamados/juizes.py:2286`, `app/chamados/juizes.py:2264`, `app/chamados/juizes.py:2267`, `app/chamados/juizes.py:839`, `app/chamados/juizes.py:856`, `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:577`.

A consulta de perguntas não restringe módulo, portanto alcança chamado de cluster que tenha disputa e perguntas correspondentes; o relógio é lido se a pergunta ultrapassar os retornos anteriores do C6 — `app/chamados/juizes.py:2264`, `app/chamados/juizes.py:1325`, `app/chamados/juizes.py:1329`, `app/chamados/juizes.py:1372`, `app/chamados/juizes.py:1389`. Existência dessa configuração nos chamados de produção: [nao verificado].

Outra cadeia possível no apply é `reabrir → ressuscitar_perguntas → _universo_da_ressuscitacao → pergunta_viva(_q)` sem `hoje` — `app/chamados/models.py:1451`, `app/chamados/models.py:1543`, `app/chamados/models.py:1509`.

Sem perguntas relacionadas, os caminhos diretos de agrupamento/irmão não mostram relógio; o veto retorna antes do juiz para esse módulo, porque cluster não pertence ao universo declarado — `app/chamados/models.py:815`, `app/chamados/models.py:822`, `app/chamados/juizes.py:2133`, `app/chamados/juizes.py:2137`, `app/chamados/models.py:1137`, `app/ponto/services/furos_vetados.py:79`, `app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/reconciliador.py:336`.

Os carimbos `auto_now/auto_now_add` devem ser citados separadamente como persistência preexistente, não confundidos com o relógio decisório do C6 — `app/chamados/models.py:68`, `app/chamados/models.py:1674`.

## 11/11 10:02 · sessao a47458d8e632 · - · - · origem=Ronald (tmux)

A tabela cobre os 36 nomes declarados; todas as linhas abaixo apontam para as asserções em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py`. Os pares nome→teste são declarados em `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:13`.

| Mutante(s) | Teste / asserção que morde | [nao verificado]
|---|---|
| `contador_cego`, `recorta_por_idade` | `test_RED_resolvido_reconciliar_com_assinatura_conta_um`: `numero == 1` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:173`. |
| `pks_divergem_do_numero` | Mesmo teste: `pks == [1]` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:174`. |
| `assinatura_invertida` | `test_mesmo_encerramento_sem_assinatura_conta_zero`: `numero == 0` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:179`. |
| `nao_medido_vira_assinatura` | `test_sem_dia_sem_celula_e_ata_nao_medida_nao_sao_assinatura`: `numero == 0` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:333`. |
| `sem_filtro_via` | `test_tres_vias_excluidas_mesmo_com_assinatura`: `numero == 0` para cada via excluída — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:201`. |
| `via_digitada` | `test_via_vem_da_constante_do_evento`: constante alterada para via sintética, `numero == 1` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:323`, `:326`. |
| `status_errado`, `sem_filtro_modulo` | `test_filtro_exige_modulo_encerrados_e_via`: `numero == len(ENCERRADOS)` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:209`. |
| `sem_filtro_colab` | `test_conta_chamados_e_respeita_colab_opcional`: filtro colab 1 deve contar 2 — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:352`. |
| `conta_dias_em_vez_de_chamados` | Mesmo teste: três chamados devem contar 3, apesar dos pares repetidos — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:351`. |
| `mistura_colaboradores` | `test_mesmo_dia_de_outro_colaborador_nao_prova_assinatura`: `numero == 1`; também exige 2 após assinar ambos — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:340`, `:343`. |
| `legado_nao_contado`, `vazia_nao_contada`, `nula_nao_contada`, `via_nova_nao_contada` | `test_RED_toda_via_fora_do_universo_contada_e_rotulada`: dicionário completo `fora_do_universo == esperado` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:225`. |
| `rotulo_desconhecido_vazio` | Mesmo teste: rótulo de cada via deve ser verdadeiro/preenchido — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:232`. |
| `fora_sem_filtro_colab` | `test_fora_do_universo_respeita_colab_e_universo_vazio`: `{'legado': 1}` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:257`. |
| `sem_guarda_colaborador_fk` | `test_RED_fk_cruzada_busca_colaborador_do_chamado_na_data_da_fk`: `numero == int(assinatura_correta)` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:305`. |
| `consulta_por_par` | `test_RED_k_colaboradores_varios_pares_faltantes_k_consultas`: número de consultas deve ser `k`, não número de pares — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:319`. |
| `troca_rotulos_humana_cascata`, `cascata_omite_ajuste_retroativo`, `cancelado_rotulado_desconhecido`, `superado_rotulado_desconhecido`, `legado_rotulado_desconhecido`, `vazia_rotulada_desconhecida`, `nula_rotulada_desconhecida` | `test_RED_rotulo_fiel_por_via_exata`: igualdade integral do dicionário `excluidos` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:250`. |
| `produto_cartesiano` | `test_RED_celulas_lidas_so_dos_pares_pedidos`: só duas células lidas — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:273`. |
| `recarrega_fk`, `reconsulta_par_da_fk` | `test_RED_reaproveita_fk_carregada_sem_buscar_o_mesmo_par`: lista de células consultadas exatamente `[(2, DIA)]` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:284`. |
| `esconde_universo_vazio` | `test_universo_vazio_imprime_sempre_inclusive_apply_e_alarme`: saída contém `universo vazio` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:377`. |
| `zero_vira_universo_vazio` | `test_zero_achados_com_universo_preenchido_nao_diz_vazio`: saída não contém `universo vazio` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:385`. |
| `silencia_contador` | `test_dry_informa_um_sem_sair_e_sem_depender_de_escala_ativa`: saída contém `"numero": 1` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:401`. |
| `alarme_ignora_contador`, `apply_vence_alarme` | `test_RED_alarme_com_um_sai_dois_sem_escrever_mesmo_com_apply`: precisa levantar `SystemExit` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:391`. |
| `alarme_so_maior` | `test_alarme_compara_numero_com_esperado`: cenário `numero=0, esperado=1` precisa levantar `SystemExit` — `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:419`, `:422`. |

**Infraestrutura/vacuidade:** não são aceitas como morte pelo harness: alvo deve existir uma única vez, o caso roda, `errors` precisa ser vazio e `failures` precisa ser não vazio. Portanto erro de infraestrutura faz o selo ficar vermelho; ausência de falha também. O harness não distingue qual asserção falhou, mas os alvos acima têm asserções diretas sobre o comportamento mutado. (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:137`, `:142`, `:143`, `:144`.)

## 11/11 10:01 · sessao a47458d8e632 · - · - · origem=Ronald (tmux)

**Selos/mutantes — conclusão estática:** os dois harnesses recusam erro de infraestrutura e exigem `resultado.failures`; uma exceção de import/sintaxe/banco não conta como mutante morto. O legado também roda controle da fonte intacta. Não há caminho explícito de sucesso com zero falhas nos mutantes. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:278`, `:286`, `:287`, `:295`; `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:137`, `:143`, `:144`.)

Os **oito mutantes do legado** têm mordidas concretas:

| Mutante | Asserção que o mata |
|---|---|
| `fechar_por_idade` | O chamado de 20 dias deve continuar VIVO após cron (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:175`). |
| `relavrar_via_em_nao_trancada` | `aplicadas == 1`, depois status VIVO (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:109`). |
| `reabre_trancada` | O relatório precisa trazer `relavrar_via_tranca, True` (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:132`). **Aqui a mordida é na classificação do relatório; a porta real continua recusando a reabertura** (`app/chamados/models.py:1009`). |
| `reabrir_fora_da_porta` | Exatamente mais uma linha `estado_chamado` (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:113`). O `QuerySet.update` do mutante pula `_gravar_estado` (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:221`). |
| `apply_sem_esperados` | `abortado` verdadeiro para `None`, vazio e conjunto diferente (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:142`, `:145`). |
| `dry_escreve` | Linha deve ficar `aplicado=False`, `aplicadas=0`, foto intacta (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:95`, `:97`, `:98`). |
| `universo_proprio` | Só o chamado com assinatura entra nos PKs/linhas (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:150`, `:154`, `:155`). |
| `forca_desligado` | `aplicado=False`, `aplicadas=0`, foto intacta (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:164`, `:165`, `:166`). |

**Execução:** rodei apenas os dois módulos puros `test_o169_cluster_encerrado_sem_lastro` e `test_o169_cluster_encerrado_mutantes`, com bytecode desabilitado: 62 testes, OK, incluindo todos os 36 mutantes. Nenhum arquivo de resultado foi criado; a execução não tem evidência arquivo:linha própria [nao verificado]. O contrato declara não usar Django/banco/rede/temporários e executa fontes em namespaces isolados (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:1`, `:31`, `:103`). A suite Django do legado não foi executada [nao verificado].

**Limites reais dos selos:**

- O harness aceita **qualquer** `AssertionError` no caso alvo, inclusive de fixture; não exige uma asserção específica. No legado, controle intacto reduz esse risco; no contador, cada caso normal também existe, mas não há controle intacto dentro da classe de mutantes. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:65`, `:72`, `:287`, `:295`; `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:133`, `:148`.)
- O comentário “cada caso abre seu savepoint (`_fixture_setup`)" não corresponde ao mecanismo visível: chama diretamente `.run(resultado)`, dentro de `transaction.atomic()` próprio e rollback `_Desfaz`. O isolamento visível é esse atomic externo; não há chamada explícita `_fixture_setup`. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:245`, `:258`, `:260`, `:261`.)
- As notas só são verificadas por **contagem maior**, sem conteúdo/tipo/exatamente uma; a trilha extra só confere via anterior e status posterior, sem ator/motivo completos. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`, `:116`, `:117`, `:118`.)

**Mutações relevantes do diff sem mordida nos testes perguntados:** [nao verificado]

1. Remover/mover a trava ou remover a releitura do contador: todos os casos são sequenciais, sem escritor concorrente/instrumentação de ordem. (`app/ponto/services/cluster_espurio.py:266`, `:268`, `:270`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:89` a `:205`.)
2. Remover `colaborador_id=o['colab']` do comando legado: o teste de comando prepara apenas um colaborador; a verificação existente do filtro no contrato puro passa pelo caminho normal, sem `legado`. (`app/ponto/management/commands/detectar_cluster_espurio.py:55`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:184`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:164`, `:404`.)
3. Trocar `recusa` por `None` sempre, ou não atualizar a recusa após falha da porta: não há asserção sobre essa chave, inclusive no desligado. (`app/ponto/services/cluster_espurio.py:254`, `:274`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:158`.)
4. Alterar ator/motivo da auditoria ou texto/tipo da nota mantendo contagem: os testes perguntados não mordem esses valores. (`app/ponto/services/cluster_espurio.py:277`, `:281`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`, `:118`.)
5. Trocar aceitação de conjuntos esperados por dependência da ordem: não há APPLY feliz com dois ou mais PKs, em ordem invertida/duplicados. (`app/ponto/services/cluster_espurio.py:259`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:106`, `:131`, `:142`.)
6. Não há cenário de tranca surgindo/desaparecendo entre medição e apply, perda de assinatura no intervalo, ou erro depois de primeiro PK de lote: os cenários perguntados usam dados estáveis e APPLY por um PK. (`app/ponto/services/cluster_espurio.py:249`, `:263`, `:270`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:100`, `:127`.)

**Outro defeito concreto:** APPLY com todos os itens trancados, com porta recusando tudo, ou mesmo `esperados=[]`/universo vazio ainda chama `get_user_sistema()` antes do loop; esse helper faz `get_or_create` e pode gravar User se faltar. Logo “NADA se escreve” não é universal. `_foto` não olha User e não captura isso. (`app/ponto/services/cluster_espurio.py:262`, `:264`; `app/core/utils.py:18`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75`.)

## 11/11 10:00 · sessao 021c9279df7a · - · - · origem=Ronald (tmux)

2. **Sem juiz novo no corpo do diff:** universo vem de `contador_cluster_encerrado_sem_lastro()['pks']`; assinatura continua em `_leitura_da_celula`; dia usa `dia_do_fato`; tranca usa a mesma casca de `renascer`; recusa delega a `recusa_da_reabertura` — `app/ponto/services/cluster_espurio.py:245`, `app/ponto/services/cluster_espurio.py:249`, `app/ponto/services/cluster_espurio.py:251`, `app/ponto/services/cluster_espurio.py:254`, `app/ponto/services/cluster_espurio.py:270`, `app/ponto/services/cluster_espurio.py:126`.

Há uma diferença **preexistente, agora exposta pelo relatório**: `_dia_em_competencia_trancada` usa a competência do chamado e a existência de `PeriodoFechado` vivo; não desconta toque do admin — `app/chamados/models.py:1038`, `app/chamados/models.py:1044`. C20, declarado como autoridade da tranca do dia, desconta explicitamente toque humano posterior — `app/core/juizes.py:156`, `app/core/juizes.py:161`, `app/chamados/juizes.py:1033`, `app/chamados/juizes.py:1041`. Não chamar essa casca de C20 sem distinguir as perguntas.

Outra diferença relevante: o dia mostrado/medido vem de `dia_do_fato(fortes=True)`, mas a competência usada na tranca vem do fato **mais recente**, incluindo ausência e perguntas vivas; a classificação pode, portanto, tratar outra competência que a do dia cuja assinatura foi contada — `app/ponto/services/cluster_espurio.py:114`, `app/ponto/services/cluster_espurio.py:251`, `app/chamados/models.py:1043`, `app/chamados/juizes.py:2275`, `app/chamados/juizes.py:2283`, `app/chamados/juizes.py:2290`.

3. **Nenhuma leitura explícita nova de relógio** no contador ou tratamento; o comando normal lê `timezone.localdate()` na borda, e `--legado` retorna antes dela — `app/ponto/services/cluster_espurio.py:88`, `app/ponto/services/cluster_espurio.py:224`, `app/ponto/management/commands/detectar_cluster_espurio.py:67`, `app/ponto/management/commands/detectar_cluster_espurio.py:70`. Os carimbos automáticos de nota/atualização são leituras de relógio de persistência preexistentes, não uma decisão nova por idade — `app/chamados/models.py:68`, `app/chamados/models.py:1674`. Não confundir `timezone.localdate(b.timestamp)` com leitura do relógio atual: ela converte um instante dado — `app/chamados/juizes.py:53`.

4. **Ordem de trava:** `abrir` faz `atomic → trava do colaborador → primeira leitura → renascer` — `app/chamados/models.py:902`, `app/chamados/models.py:905`, `app/chamados/models.py:906`, `app/chamados/models.py:922`. O novo tratamento faz `atomic → leitura de ch+colaborador → trava → contador → renascer`; a primeira leitura ocorre antes da trava — `app/ponto/services/cluster_espurio.py:266`, `app/ponto/services/cluster_espurio.py:267`, `app/ponto/services/cluster_espurio.py:268`, `app/ponto/services/cluster_espurio.py:270`, `app/ponto/services/cluster_espurio.py:273`. Isso viola a orientação explícita de reler a instância depois da aquisição — `app/chamados/models.py:2638`.

`resposta_tardia` não tem essa sequência: consulta estado e célula e chama diretamente o renascimento; a trava vem posteriormente, quando o escritor grava estado — `app/chamados/services/resposta_tardia.py:35`, `app/chamados/services/resposta_tardia.py:44`, `app/chamados/services/resposta_tardia.py:46`, `app/chamados/models.py:280`, `app/chamados/models.py:290`.

Não identifiquei uma inversão nova comprovada: antes de adquirir C, o novo corpo só faz leitura comum; o escritor adquire C→D→Ch e a porta de ressuscitação C→D→Ch→P — `app/ponto/services/cluster_espurio.py:267`, `app/chamados/models.py:289`, `app/chamados/models.py:1562`, `app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`, `app/chamados/models.py:2692`. Ausência universal de deadlock é [nao verificado].

**TOCTOU concreto:** a releitura do contador não atualiza `ch` nem seu colaborador já carregado. Se outro ato alterar `situacao`, contexto ou via enquanto espera a trava, `renascer` pode usar o objeto anterior; `reabrir` só refresca campos quando detecta mudança de status — `app/ponto/services/cluster_espurio.py:267`, `app/ponto/services/cluster_espurio.py:270`, `app/chamados/models.py:1005`, `app/chamados/models.py:1007`, `app/chamados/models.py:1373`, `app/chamados/models.py:1375`.

`esperados` compara apenas a fotografia inicial; sob a trava só verifica se cada pk ainda pertence ao contador, sem conferir igualdade de conjuntos. Novos candidatos não abortam o lote; candidatos desaparecidos são pulados; a classificação inicial `trancada` também decide quais linhas sequer entram no apply — `app/ponto/services/cluster_espurio.py:259`, `app/ponto/services/cluster_espurio.py:264`, `app/ponto/services/cluster_espurio.py:270`. Não há transação do lote: falha posterior deixa anteriores já commitados — `app/ponto/services/cluster_espurio.py:263`, `app/ponto/services/cluster_espurio.py:266`.

A trava C não serializa necessariamente a mudança de `PeriodoFechado`: `trancar_periodo` grava a tranca antes do gancho que trata os chamados, e `reabrir_periodo` também grava antes do gancho — `app/ponto/services/fechamento.py:1408`, `app/ponto/services/fechamento.py:1419`, `app/ponto/services/fechamento.py:1428`, `app/ponto/services/fechamento.py:1441`, `app/ponto/services/fechamento.py:1448`.

5. **`pks` e `numero`: coerentes na implementação:** `pks` enumera chamados com assinatura e `numero=len(pks)`; o alarme compara `numero` ao esperado e o tratamento usa os pks. `universo.total` é outro denominador, anterior ao filtro da assinatura — `app/ponto/services/cluster_espurio.py:126`, `app/ponto/services/cluster_espurio.py:127`, `app/ponto/services/cluster_espurio.py:149`, `app/ponto/services/cluster_espurio.py:246`, `app/ponto/management/commands/detectar_cluster_espurio.py:100`.

`trancada` significa a mesma chamada que a guarda de `renascer`, em estado estável; pode divergir temporalmente porque a linha foi montada antes do apply — `app/ponto/services/cluster_espurio.py:249`, `app/chamados/models.py:1009`, `app/ponto/services/cluster_espurio.py:263`.

`aplicado/aplicadas` contam somente retorno verdadeiro do renascimento, não efeitos totais ou tentativas — `app/ponto/services/cluster_espurio.py:273`, `app/ponto/services/cluster_espurio.py:274`, `app/ponto/services/cluster_espurio.py:282`. Porta recusada pode mesmo assim escrever trilha de veto ou migrar perguntas/anexar nota e retornar falso — `app/chamados/models.py:1017`, `app/chamados/models.py:1415`, `app/chamados/models.py:1427`, `app/chamados/models.py:1436`.

**`recusa` é incompleta:** relatório consulta só as guardas de `reabrir`; desligado/cascade/tranca pertencem às guardas adicionais de `renascer`; após retorno falso nada atualiza o motivo. Assim desligado pode aparecer `ativo=False`, `recusa=None`, `aplicado=False` — `app/ponto/services/cluster_espurio.py:254`, `app/ponto/services/cluster_espurio.py:255`, `app/ponto/services/cluster_espurio.py:274`, `app/chamados/models.py:1005`, `app/chamados/models.py:1007`, `app/chamados/models.py:1009`.

Outros defeitos concretos:

- `get_user_sistema()` roda antes de pular linhas trancadas e fora dos atomics; usa `get_or_create`. Até lote vazio ou totalmente parado pode criar `auth_user` se o sistema faltar — `app/ponto/services/cluster_espurio.py:262`, `app/ponto/services/cluster_espurio.py:264`, `app/core/utils.py:18`.
- A nova trilha obrigatória usa escritor best-effort, cujo retorno pode ser `None`; o tratamento ignora o retorno e conta aplicação mesmo sem a trilha persistida — `app/core/observ.py:96`, `app/core/observ.py:105`, `app/core/observ.py:139`, `app/ponto/services/cluster_espurio.py:277`, `app/ponto/services/cluster_espurio.py:282`.
- Descrição da auditoria afirma “encerrado por idade pelo cron velho”, mas o próprio contador declara não distinguir nota/trilha/motivo e incluir decisões administrativas por reconciliar; proveniência por idade de cada pk é [nao verificado] — `app/ponto/services/cluster_espurio.py:93`, `app/ponto/services/cluster_espurio.py:94`, `app/ponto/services/cluster_espurio.py:278`.


## 11/11 10:00 · sessao 5604be92552c · - · - · origem=Ronald (tmux)

O diff respeita os escritores declarados dos campos principais:

- `status_local`, `via_encerramento` e `resolvido_em`: tratamento chama `_renascer_por_premissa_viva`; esta chama `reabrir`, que chama `_gravar_estado`. O núcleo altera e salva os três campos; `models.py` é porta declarada. Não há escrita direta nova desses campos no serviço. `app/ponto/services/cluster_espurio.py:273`; `app/chamados/models.py:1019`; `app/chamados/models.py:1442`; `app/chamados/models.py:228`; `app/chamados/models.py:244`; `app/chamados/models.py:248`; `app/chamados/models.py:296`; `app/core/portas.py:218`.
- Nota de renascimento: `_renascer_por_premissa_viva` usa `nota.anexar_ocorrencia`, escritor declarado de `FollowupChamado`; a porta cria a nota. Leitores incluem histórico, detalhe e painel. `app/chamados/models.py:1021`; `app/chamados/services/nota.py:75`; `app/chamados/services/nota.py:103`; `app/core/portas.py:241`; `app/chamados/services/historico.py:80`; `app/chamados/views.py:352`; `app/chamados/services/painel.py:362`.
- `LogAuditoria`: núcleo emite `estado_chamado`; serviço emite `reabrir_legado_cluster_o169`; ambos passam por `core.observ.evento`. O histórico e os vigias leem `estado_chamado`; a ação nova tem leitor específico no teste. `app/chamados/models.py:292`; `app/ponto/services/cluster_espurio.py:277`; `app/core/observ.py:94`; `app/core/observ.py:134`; `app/chamados/services/historico.py:86`; `app/chamados/services/vigia_loop.py:28`; `app/chamados/services/vigia_loop.py:114`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`.
- `CelulaDia.ata`: tratamento apenas lê via contador e `_leitura_da_celula`; produtor é cartório, persistência passa pela porta da célula. `app/ponto/services/cluster_espurio.py:245`; `app/ponto/services/cluster_espurio.py:126`; `app/ponto/services/cluster_espurio.py:27`; `app/ponto/services/cartorio.py:696`; `app/ponto/services/cartorio.py:1811`; `app/ponto/portas/celula.py:974`; `app/ponto/portas/celula.py:1033`; `app/core/portas.py:112`.
- `PeriodoFechado`: tratamento apenas consulta indiretamente; escritores de tranca/reabertura são `trancar_periodo`/`reabrir_periodo` em fechamento, módulo declarado. `_dia_em_competencia_trancada` lê `competencia_trancada`, que consulta `reaberto_em__isnull=True`. `app/ponto/services/fechamento.py:1408`; `app/ponto/services/fechamento.py:1437`; `app/core/portas.py:501`; `app/chamados/models.py:1033`; `app/ponto/services/fechamento.py:1473`.

Leitores relevantes dos valores alterados:

- Contador filtra `ENCERRADOS` e via `RECONCILIAR`; relatório lê status/via antes e depois. `app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:112`; `app/ponto/services/cluster_espurio.py:251`; `app/ponto/services/cluster_espurio.py:272`; `app/ponto/services/cluster_espurio.py:280`.
- Via de tranca determina desarquivamento e devolução à fila quando período reabre. `app/chamados/views_arquivar.py:64`; `app/chamados/services/competencia_trancada.py:259`.
- `resolvido_em` participa da recusa por fechamento administrativo sem fato novo e das estatísticas. `app/chamados/models.py:1342`; `app/chamados/services/painel.py:245`; `app/chamados/services/fila.py:109`.
- Os valores reais são `RECONCILIAR → 'sistema'` e tranca → `'competencia_trancada'`. `app/chamados/catalogo/motor.py:88`; `app/chamados/catalogo/motor.py:92`; `app/chamados/catalogo/motor.py:95`.

Defeitos e nuance do ramo parado:

- A trilha não é garantida atomicamente: `evento` é best-effort, engole falha e retorna `None`; o tratamento ignora o retorno e marca aplicado. Pode reabrir sem a linha obrigatória do legado; o núcleo usa o mesmo emissor para `estado_chamado`. `app/core/observ.py:98`; `app/core/observ.py:105`; `app/core/observ.py:133`; `app/core/observ.py:139`; `app/ponto/services/cluster_espurio.py:277`; `app/ponto/services/cluster_espurio.py:282`; `app/chamados/models.py:292`.
- `get_user_sistema()` pode criar usuário antes do laço, mesmo universo vazio ou composto somente de linhas trancadas. Logo “nada se escreve” no ramo parado não vale para qualquer entidade do banco se faltar esse usuário. `app/ponto/services/cluster_espurio.py:262`; `app/core/utils.py:18`.
- Não encontrei gate que relavre **somente a via para tranca, preservando o status encerrado**: o gate dedicado recusa qualquer estado fora de `VIVOS`. `app/chamados/models.py:1622`; `app/chamados/models.py:1630`.
- A alegação literal “nenhuma porta troca a via de chamado já encerrado sem reabrir” é excessiva: `resolvido → fechado` é permitido; `transicionar → _aplicar → _gravar_estado` troca via para `admin`, preservando `resolvido_em`, sem estado vivo intermediário. Isso não resolve a relavra solicitada para tranca. `app/ponto/services/cluster_espurio.py:220`; `app/chamados/catalogo/motor.py:30`; `app/chamados/catalogo/motor.py:102`; `app/chamados/models.py:128`; `app/chamados/models.py:199`; `app/chamados/models.py:447`; `app/chamados/models.py:242`.

## 11/11 10:00 · sessao e11873e160bf · FIO-E0-V4-E3 · sonda · origem=Code

Não reexecutei a sonda. O envelope reescreve credenciais, cria/remove arquivos temporários e acrescenta log, contrariando sua regra de não escrever arquivos. [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/bin/sonda_leitura.sh:126), [bin/sonda_leitura.sh:216](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/bin/sonda_leitura.sh:216), [bin/sonda_leitura.sh:248](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/bin/sonda_leitura.sh:248) A identificação desta cópia como HEAD do hash informado não pôde ser confirmada: não há repositório Git reconhecido no diretório. [nao verificado]

Transcrição integral da saída fornecida, sem nova execução. [nao verificado]

```text
sonda_leitura: sonda_x1_comp11_v2.py [c1ce461ce6ce3acb] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

PERG pk 54831 col237 2026-11-07 liga=celula data_ev=2026-11-07 mat=sim valid=nao via=celula_absolveu cel_ver=fato_em_ausencia disp_fechada=sim pai=nenhum viva=True motivo=batida_fora_escala_grave
celula.acusa n 76
celula.nao_acusa n 1
pares n 77
pergunta.fora_universo n 1
pergunta.total n 1
pergunta.viva n 1
```

Na saída fornecida não consta traceback, `sonda_nao_verificada` nem linha `CHAMADO_VIVO_X1`. Não há contadores adicionais transcritos; não os completei com zeros. [nao verificado]

Não existe linha PERG com `mat=nao valid=nao` no dado fornecido; portanto, não há caso individual dessa classe para mapear. [nao verificado] O caminho condicional de absolvição seria `qs_perguntas_absolviveis` → `perguntas_absolvidas_pela_celula` → `retratar_perguntas_absolvidas_por_celula`: exige materialização e validação nulas, célula com veredito preenchido que não acusa, pergunta morta pelo juiz e dia não trancado; então chama `carimbar(VIA_CELULA_ABSOLVEU, ...)`. Deixar de acusar sozinho não basta. [app/chamados/reconciliador.py:627](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:627), [app/chamados/reconciliador.py:661](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:661), [app/chamados/reconciliador.py:595](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:595)

Para a **PERG 54831**, com via preenchida e `viva=True` no dado: [nao verificado]

- **O carimbo não garante morte.** `fato_em_ausencia` pertence a `VEREDITOS_ACUSAM`; quando alcança esse ramo, o juiz retorna `True`, mesmo havendo materialização. `celula_absolveu` não integra as vias de identidade que encerram independentemente do fato. [app/escala/models.py:1767](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/escala/models.py:1767), [app/chamados/juizes.py:1372](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/juizes.py:1372), [app/chamados/models.py:2510](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/models.py:2510)

- **O reconciliador não a recarimba nesse estado**, nem se a célula passar a não acusar: sua seleção exige `materializada_em IS NULL`. O reatador de órfãs também exige materialização nula e disputa aberta. [app/chamados/reconciliador.py:629](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:629), [app/chamados/reconciliador.py:1500](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/reconciliador.py:1500)

- **A cascata S143/S128 também não a recarimba:** o receiver chama o resolvedor, cujo primeiro conjunto exige materialização nula e disputa aberta; o ramo sem credor pula disputas fechadas e exige materialização e validação nulas. [app/chamados/signals.py:115](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/signals.py:115), [app/chamados/services/disputa_emissao.py:1823](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/disputa_emissao.py:1823), [app/chamados/services/disputa_emissao.py:1907](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/disputa_emissao.py:1907), [app/chamados/services/disputa_emissao.py:1916](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/disputa_emissao.py:1916)

- **S144 também a exclui:** delega a `materializar_pendentes_aprovadas`, que seleciona somente materialização nula. [app/chamados/signals.py:162](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/signals.py:162), [app/chamados/services/materializacao.py:1418](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/materializacao.py:1418)

- **Existe outro escritor capaz de desfazer o carimbo**, se receber essa pergunta e confirmar sua elegibilidade/vitalidade: `desfazer_carimbo_sem_lastro`, cujo padrão permite reabrir disputa fechada, limpa os três campos do carimbo e reabre a disputa. Porém, o julgamento automático chama essa porta com `reabrir_fechada=False`, excluindo disputas fechadas. Isso impede concluir que “nenhum escritor existe”, mas esses caminhos automáticos não corrigem essa linha no estado informado. [app/chamados/services/relavra_via.py:187](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/relavra_via.py:187), [app/chamados/services/relavra_via.py:238](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/relavra_via.py:238), [app/chamados/services/relavra_via.py:246](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/relavra_via.py:246), [app/chamados/services/relavra_via.py:260](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/relavra_via.py:260), [app/chamados/services/fabrica_da_celula.py:209](/home/ronald/codex_ws/9ca563bb3e0470eae1586b080303b2b026c85455/app/chamados/services/fabrica_da_celula.py:209)

O dado não comprova quem gravou o carimbo original, quando o veredito mudou ou se alguma dessas portas foi executada para a PK 54831. No anexo, `pai=nenhum` representa `chamado_destino`, não o chamado-pai da disputa. [nao verificado]

## 11/11 09:55 · sessao eacf68e547f2 · FIO-E0-V4-E3 · sonda · origem=Code

1) Transcrição de todos os contadores e linhas `VIVA`/`CHAMADO_VIVO` fornecidos; execução em produção sem verificação independente: [nao verificado]

```text
VIVA pk 54831 col237 2026-11-07
chamado.total n 1
pares n 77
pergunta.total n 1
pergunta.via.celula_absolveu n 1
pergunta.viva n 1
```

Não aparece linha `CHAMADO_VIVO`, nem contador `chamado.vivo` impresso na saída anexada. [nao verificado]

2) Não aparece `sonda_nao_verificada` nem traceback no dado fornecido. [nao verificado]

3) **Não: os recortes não são equivalentes.** O alcance possível pelo código é maior, pelos seguintes caminhos. (`app/chamados/reconciliador.py:627`, `app/chamados/reconciliador.py:608`)

- **Perguntas com data ausente ou divergente.** O reconciliador seleciona pelo colaborador da disputa, exige `materializada_em` e `validada_em` nulos e célula vinculada com veredito preenchido que não acusa. Não restringe `data_evento`, `celula__data` nem competência. Assim, perguntas vinculadas às células desses dias, mas com `data_evento=NULL` ou fora dos dias listados para aquele colaborador, ficam fora da sonda e podem entrar como candidatas no reconciliador. (`app/chamados/reconciliador.py:627–642`)

- **Candidata não significa pergunta efetivamente absolvida.** O gerador só entrega a pergunta quando `pergunta_viva(p)` é falsa e o dia não está trancado; o escritor ainda depende do retorno de `carimbar`. Portanto, a diferença de filtros demonstra alcance possível, sem provar que existam registros omitidos elegíveis em produção. (`app/chamados/reconciliador.py:661–672`, `app/chamados/reconciliador.py:595–597`)

- **Chamado-pai sem célula no dia sondado.** Após carimbar, o escritor pode fechar a disputa; `pode_fechar()` consulta todas as perguntas dela, sem recorte de dia. O signal busca o chamado-pai pelo seu PK e tenta reconciliá-lo, sem exigir `celula__data=dia`. Um pai sem célula ou com célula fora dos dias sondados escapa da consulta de chamados da sonda, embora possa ser alcançado por esse caminho. (`app/chamados/reconciliador.py:598–614`, `app/chamados/models.py:1848–1853`, `app/chamados/signals.py:28–42`)

- **Perguntas adicionais pela cascata do pai.** Se o chamado for encerrado, o signal pode chamar o resolvedor, cujo filtro usa `chamado_destino`, materialização nula e disputa aberta, sem limitar `data_evento`. Essas perguntas também podem ficar fora da sonda; a cascata aplica suas próprias guardas antes de carimbar. (`app/chamados/signals.py:96–115`, `app/chamados/services/disputa_emissao.py:1823–1827`, `app/chamados/services/disputa_emissao.py:1837–1865`)

Separadamente das duas funções perguntadas, a rotina irmã `retratar_chamados_absolvidos_por_celula` também alcança chamados vivos de turno sem exigir célula vinculada: determina o dia por `dia_do_fato` e busca a célula por colaborador/data. Esse dia pode vir do contexto, da batida ou da disputa. (`app/chamados/reconciliador.py:996–1004`, `app/chamados/reconciliador.py:1023–1026`, `app/chamados/juizes.py:98–138`)

Existência e quantidade de registros nessas lacunas, causa da pergunta viva e resultado de eventual reconciliação em produção: [nao verificado].

_3773 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
