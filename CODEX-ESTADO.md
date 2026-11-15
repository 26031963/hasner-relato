# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 20:07** · publicado em **15/11/2026 20:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 20:07 · sessao fc9b4b42c98c · FIO-E0-V4-E1-ENTRADAS · leitura · origem=Code

**Veredito: ABERTA para baixa integral, com o núcleo já pousado.** A descrição “em construção na volta 15g” ficou superada pelo pouso e pelo “PRONTO / NO AR”; porém a documentação ainda mantém a obra parada e reúne complementos pendentes. (`app/docs/RELATO.md:1674–1675`; `app/docs/GRADE-MESTRA.md:106`, `:683–701`)

A cópia não contém `.git`: `git log --all --grep` não pôde ser executado. Ancestralidade no `main`, hashes completos e datas dos commits **[não verificado]**. Abaixo, hashes e datas são **registros documentais**, sem consulta a banco ou logs. [nao verificado]

**1. Rastro encontrado**

| Data registrada | Hash / etapa | Evidência |
|---|---|---|
| 06/11, aval 22:12; despacho 22:55 | Abertura em `wt-fioe1-225336`, base `19aeede1`; E1 absorve PLANTIO (i) | `app/docs/BACKLOG.md:1964`; `app/docs/RELATO.md:1835–1836` |
| 02:20–02:25; dia do commit **[não verificado]** | `c2833d85`; PLANTIO `742f6e3b` reescrito para `51ea1e37`; árvore anterior `e55c6a37` | Entrega ainda incompleta, expressamente **“NAO POUSA”**. `app/docs/RELATO.md:1813–1815` |
| 07/11, registro da v5 | `12740227` | Desenho v5 respondido; construção retomada. `app/docs/BACKLOG.md:1964`; `app/docs/RELATO.md:1781–1783` |
| 07/11 16:2x | `344a9ddd`, volta 8 | Suite 14.165 OK, mas parada por desenho; respostas posteriores sobre veto e ordem do sinal. `app/docs/BACKLOG.md:1964`; `app/docs/RELATO.md:1769–1770` |
| 08/11, volta 14b | `6695e972` | Suite 14.301 OK; opus final 2 reprovou; nada pousado nessa etapa. `app/docs/RELATO.md:1742`, `:1721` |
| 08/11 11:2x | `d41caaf6`, volta 15 | Entrega e verificação; posteriormente superada pela 15b. `app/docs/RELATO.md:1729`, `:1726` |
| 08/11, volta 15b | `21e7341c` | Suite 14.325 OK; correções adicionais encaminhadas à 15c. `app/docs/RELATO.md:1719`, `:1725` |
| 08/11, volta 15c | `746179d7`; merge `b74da998` | Suite da volta verde; merge apresentou três falhas na catraca e defeito no portão do apply. `app/docs/RELATO.md:1714–1718` |
| 08/11, volta 15c2 | `3d8ebe87`; merge `76644dc8` | Suite 14.404 OK; opus final 3 reprovou o reuso com data antiga. `app/docs/RELATO.md:1710–1713` |
| 08/11, volta 15d2 | `f16d4737`; merge `b0c4297c` | Helper único; suite 14.422 OK; opus final 4 reprovou o caso de data NULL. `app/docs/RELATO.md:1707–1708`, `:1688` |
| 08/11, volta 15e | `06efab7e`; merge `a42d5204` | Contrato de saída e sombra verde; verificador pediu nova volta. `app/docs/RELATO.md:1696–1697` |
| 08/11, volta 15f | `4ecedc95`; merge `e2f865f5` | Suite 14.474 OK; achados g1–g5 encaminhados à 15g. `app/docs/RELATO.md:1698–1700` |
| 08/11 18:1x | `297499b8`, volta 15g | RED 8/20, GREEN 313; aval **E1-MATRIZ-TODAS-AS-PORTAS**. `app/docs/RELATO.md:1701`; `app/docs/PROMPTS.md:1992` |
| 08/11, após esse aval | `5c875ee3` | **Squash de `297499b8`**, ampliado às portas P1–P7; GREEN 5.978. `app/docs/RELATO.md:1702` |
| 08/11, interrompido 19:23 | `eabbfc5a`, merge6 | **Não certificou**; suite interrompida pelo PARE-P4. `app/docs/RELATO.md:1702`, `:1704` |
| 08/11, volta 15h | `6dee2010`; merge7 `b0e948d8` | Recusa de destino de outro colaborador; suite **14.536 OK** e sombra verde. `app/docs/RELATO.md:1705–1706` |
| 08/11 21:15; deploy 22:46 | `a8a692f5` | **Pouso local**, seguido de **PRONTO E1 NO AR**. `app/docs/RELATO.md:1675`, `:1674` |
| 08/11 23:12 | `a5df8ed5` | Reescrita de mensagens: árvore declarada igual a `a8a692f5`; 19 hashes alterados. Mapeamento efetivo **[não verificado]**. `app/docs/RELATO.md:1673` |
| 09/11 00:1x–00:4x | E1-FILA: `602a7d73` → `06ee1af0` → `45cf0e53` | Voltas corretivas; última com opus APROVA e suite 14.570 OK. `app/docs/RELATO.md:1677`, `:1676`, `:1668` |
| 09/11 00:5x; no ar 01:5x | Merge de `45cf0e53`; commit `7aa48401` | Pouso e deploy da **E1-FILA**. `app/docs/RELATO.md:1679–1680`; `app/docs/BACKLOG.md:1965` |
| 09/11 03:3x | Merge de `e9bf5de2`; commit `89df8c52` | **E1-FILA-V2-2-TRAVA também pousou**, com suite 14.622 OK e opus APROVA. `app/docs/RELATO.md:1633`; `app/docs/BACKLOG.md:1972` |

As demais menções exatas no RELATO são despachos, perguntas respondidas ou referências de contexto: `app/docs/RELATO.md:1662–1664`, `:1672`, `:1686–1690`, `:1693`, `:1723`, `:1728`, `:1749`, `:1768`, `:1784`, `:1787–1788`, `:1811–1812`, `:1818–1819`, `:9028`. As menções posteriores no BACKLOG vinculam complementos à E1. (`app/docs/BACKLOG.md:2086–2087`)

Não localizei os termos solicitados em `app/docs/RELATO-ARQUIVO.md` nem em `app/docs/TICKETS.md` **[não verificado]**.

**2. Houve pouso? O que ficou fora?** [nao verificado]

**Sim, documentalmente:** E1 em `a8a692f5`, E1-FILA em `7aa48401` e V2-2 em `89df8c52`. A comprovação por grafo Git de merge no `main` permanece **[não verificado]**. (`app/docs/RELATO.md:1675`, `:1679–1680`, `:1633`)

Ficaram separados:

- **Cinco grupos exportados**, expressamente fora do apply; o aval autorizava pousar com os limpos. (`app/docs/BACKLOG.md:855`; `app/docs/RELATO.md:1731`)
- Recusas sem contador, origem sem dia, conversa/reabertura sobre retratada e duplicatas sobre retratada, em filas próprias. (`app/docs/BACKLOG.md:857–860`)
- Resíduos de leitores, relógios, travas, outras portas e custo. **V2-2-TRAVA não está inteira pendente: ela pousou.** (`app/docs/BACKLOG.md:1970–1974`)
- Complementos posteriores **BUG-PLANTIO-NOTURNO-MAIS-1** e **ABSOLVIDA-PELA-FOLGA**; o segundo consta explicitamente aberto em 12/11. (`app/docs/BACKLOG.md:2086–2087`)

Há registro de quatro migrações aplicadas por Ronald e DRY seguinte zerado; isso é prova documental, sem conferência do banco nesta auditoria. (`app/docs/RELATO.md:1682`)

**3. A implementação existe na cópia?**

| Elemento | Prova |
|---|---|
| Trava das quatro classes | `trava_na_ordem` adquire Colaborador → DisputaSupervisao → ChamadoColaborador → PerguntaDisputa, por pk. `app/chamados/models.py:2707`, `:2744`, `:2753`, `:2766`, `:2770` |
| Catraca AST | Enumera as quatro classes, fixa o dono autorizado e exige zero trava fora dele. `app/chamados/tests/test_e1_v7_catracas.py:71`, `:85`, `:387` |
| Append-only | `DecisaoPergunta`; bloqueios de update/delete/bulk_update e de sobrescrita por save. `app/chamados/models.py:3969–3979`, `:3982`, `:4046–4052` |
| Escritor e evento da célula | `registrar` insere decisões e acorda células no mesmo atomic; a emissão passa por `emitir_evento_celula`. `app/chamados/models.py:4086–4099`, `:3314–3331` |
| Lâmpada do efeito | O leitor usa `veredito_lavrado` vigente por `esta_stale`; vencido ou ausente resulta em `nao_sei`. `app/chamados/services/tarefa_do_dia.py:16–18`, `:422–425` |
| Dia do fato em ponto único | `gravar_dia_do_fato` concentra a atribuição de `data_evento`. `app/chamados/services/fio.py:829–839` |
| Backfill | Comando com `--aplicar`; registra silêncio, retrata duplicatas pela porta e desfaz o ato em caso de recusa. `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `:331`, `:344`, `:425` |
| Identidade no banco | Migration 0053 recusa colisões e cria unicidade parcial por MARCO e BATIDA. `app/chamados/migrations/0053_pergunta_unica_por_familia.py:36–66` |

**Limite do backfill:** esse comando não aplica as linhas antigas de resposta/validação da projeção; reserva esse passo a outro ato. A E4 registra o backfill posterior. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3–13`; `app/docs/BACKLOG.md:1991`) Resultados atuais dos testes **[não verificado]**.

**4. Decisão para a linha**

**ABERTA para baixa integral.** Há prova documental suficiente de que **o núcleo foi entregue**, mas não de quitação do agrupamento atual: a grade mantém “PARADA” e 16 complementos; há complemento explicitamente aberto. (`app/docs/RELATO.md:1674–1675`; `app/docs/GRADE-MESTRA.md:106`, `:683–701`; `app/docs/BACKLOG.md:2087`)

Também não há prova de a E1 ter sido absorvida por outra obra **[não verificado]**. A absorção documentada ocorre no sentido contrário: **E1 absorve PLANTIO (i)**. (`app/docs/RELATO.md:1835`)

O estado sustentado pela cópia é: **“núcleo pousado em 08/11; correções E1-FILA/V2-2 pousadas em 09/11; acervo exportado e complementos em linhas próprias”**. (`app/docs/RELATO.md:1675`, `:1679`, `:1633`; `app/docs/BACKLOG.md:855`; `app/docs/GRADE-MESTRA.md:683–701`)

## 15/11 20:07 · sessao ba2ae83313e6 · - · - · origem=Ronald (tmux)

Censo somente leitura. Convenção: **A** consulta autoridade de vigência/dia; **B** escolhe vínculo/tipo/fase/posto por `ativa`, sem vigência para a data; **C** outro uso, identificado abaixo. Chamar `eh_dia_trabalho` ou `marcos_do_dia` **depois de escolher EC por ativa** não torna a seleção A: exemplos `app/chamados/juizes.py:1852`/`:1857`, `app/escala/services/furos_diarios.py:301`/`:316`.

Autoridades: `tem_vigencia_declarada` implementa `ativa or data_fim` em `app/escala/alimentacao.py:196`; `vinculo_cobre_a_data` acrescenta intervalo em `:214`; SQL equivalente em `:219`; `vinculo_do_dia` lê geradora em `:269`, ou candidatos pelo juiz em `:285`, com desempate `ativa,data_inicio` em `:286`.

Inventário de consultas ORM explícitas, com prefixo `app/`:

| Arquivo | Sítios e classificação |
|---|---|
| `folha/export.py` | **C :806** — guarda sem âncora para classificar exportação; não escolhe EC do dia. |
| `folha/views_retrato.py` | **B :107** `_cadastro`. |
| `colaboradores/views_fase.py` | **B :45** — EC ativa mais recente para tipo/âncora; o início que cobre dia contraditório segue outra chamada, `inicio_do_vinculo`, em `:73`. |
| `colaboradores/views.py` | **B :48, :226, :268, :336, :355, :394, :927, :2206, :2864** — prefetch/lista, folga, painéis, posto, row, agenda e disponibilidade. |
| `colaboradores/services/calendario.py` | **B :258** `contexto_calendario`. |
| `colaboradores/services/drawer.py` | **B :22** `contexto_drawer`. |
| `colaboradores/services/detalhe.py` | **B :73** `contexto_detalhe`. |
| `colaboradores/services/troca_pelo_chat.py` | **B :54** — rótulo “de”; `julgar_vinculo` é chamado separadamente em `:58`. |
| `colaboradores/services/geofence_raios.py` | **C :63** — quantidade de vínculos ativos por posto. |
| `ponto/selecao_periodo.py` | **B :278** — jornada de escala ativa para teto do turno armazenado; ordena `data_inicio` ascendente e usa `setdefault` em `:280`. |
| `ponto/turnos.py` | **B :2464, :2483** — escala para tipo de próxima batida/intermitente/intervalo. |
| `ponto/worklist.py` | **B :49** — posto na fila DP. |
| `ponto/views.py` | **B :2064** — `_escala_rotulo_do_colab`. |
| `ponto/services/faltas_de_hoje.py` | **B :35** — escolhe EC ativa e depois pergunta trabalho em `:40`. |
| `ponto/services/abriu_nao_bateu.py` | **B :194** — escolhe EC ativa, depois marco para `dia` em `:198`. |
| `ponto/services/piso_intervalo.py` | **B :28** — usa template ativo para batidas de período. |
| `ponto/services/espelho.py` | **B :42, :201** — piso visual e resumo da escala. |
| `ponto/services/cluster_espurio.py` | **C :185** — universo de reconciliação: existência de ativo sem fim; não escolhe EC do dia. |
| `ponto/services/bordas_realizado.py` | **B :62** — usa intervalo livre do tipo do EC ativo em `:67`; grade diária é lida separadamente em `:68`. |
| `ponto/services/fechamento_lista.py` | **B :59** — resumo por tipo ativo. |
| `ponto/services/triagem_batida.py` | **B :92** — “sem escala” por existência de ativa; **B :332** — rótulo de anuência por tipo ativo. O mesmo arquivo usa `vinculo_do_dia` em outro trecho; isso não rege essas duas consultas. |
| `inteligencia/calculadores.py` | **B :327** — `_calc_escalas_desvio`. |
| `relatorios/furos.py` | **B :60** — escolhe vínculos ativos sem fim para verificar limbo de hoje/amanhã em `:67`. |
| `relatorios/views.py` | **B :492** extrato parcial; **C :830, :856** universo de filtros de busca por tipo/fase. |
| `relatorios/pendencias.py` | **B :74** — tipo ativo para direção. |
| `core/views.py` | **B :401, :454, :789** — supervisor, onboarding e painel TV. |
| `core/services/painel_op.py` | **B :109** — mapa de tipo ativo. |
| `holerite/views.py` | **B :282** — `_pessoas_da_celula`. |
| `escala/views_plano_folgas.py` | **C :43** — postos com vínculo ativo; **B :55** — ECs da grade mensal por ativa; **C :109, :176** — guarda de estado por PK para editar/aplicar folgas. |
| `escala/folgas.py` | **A :74, :79** — ativa limita universo da lavra; quem alcança mês vem de `escalas_do_periodo_por_colab` em `:78`. |
| `escala/utils.py` | **C :2115** — guarda de existência de âncora, função termina retornando zero em `:2123`. |
| `escala/regua_defesa.py` | **C :259** — universo/contagem de templates para sugerir destino, não EC do dia. |
| `escala/views.py` | **B :17** minha escala; **C :86, :124, :165, :171, :240, :262, :392** — contagens/listas, proteção de exclusão (inclui `ativa=False`), busca de ainda não vinculados, alvo do desvínculo, universo de aplicação de calendário. |
| `escala/services/escala_auto.py` | **B :172** — EC ativo sem fim mais recente por colaborador para proposta; **C :186** — contagem de compartilhamento de template. |
| `escala/services/fase_conflitante.py` | **B :71** — compara foto/fase no próprio vínculo ativo, sem selecionar vínculo do dia (`:78–80`). |
| `escala/services/impacto_troca.py` | **B :34** — EC ativo mais recente para “antes”; chama trabalho por dia em `:39`. |
| `escala/services/meio_periodo.py` | **C :80** — contagem por template; **B :152** — observado por tipo ativo. |
| `escala/services/cadastro_realidade.py` | **B :228** — mapa de tipo ativo em `:231`, usado contra células do período em `:233`. |
| `escala/services/furos_diarios.py` | **B :35** apuração; **C :158** universo do ranking por join de ativa; **B :301** classificação sem escala/limbo/sem dia. |
| `escala/services/cadastro_tipo.py` | **C :339** — universo de propagação de alteração do template; recorta início por `max(ec.data_inicio, janela_anterior)` em `:342`, não chama juiz da cobertura nesse laço. |
| `api/views_core.py` | **B :299** `api_me`. |
| `api/views.py` | **B :311, :908** — `api_me`, `api_escala_v2`. |
| `api/views_mensageria.py` | **C :1222** — pendência cadastral de folga em 6x1. |
| `chamados/reconciliador.py` | **C :233** — premissa “existe ativa sem âncora”. |
| `chamados/juizes.py` | **B :1852** — `_marco_esperado_da_grade`: data da pergunta não recorta o EC escolhido. |
| `chamados/views.py` | **B :442** — vínculo ativo mais recente do tipo. |
| `chamados/detector_anomalias.py` | **C :58** — conta colisão de ativas sem fim, `count()>1` em `:63`; não mede sobreposição temporal geral. |
| `chamados/services/limbo_folgas.py` | **C :65** — contagem de usuários de template. |
| `chamados/services/disputa_emissao.py` | **B :349** — opções de escala do colaborador por ativa/sem fim, sem data do fato. |
| `chamados/services/desvio_escala.py` | **B :44** — EC ativo mais recente; **C :106** — outros usuários ativos do template. |
| `folha/services/ranking_he.py` | **B :132** — tipo ativo para ranking. |
| `ferias/management/commands/alertar_retornos.py` | **B :28** — tipo ativo. |
| `relatorios/management/commands/diff_extrato_parcial.py` | **B :105** — mapa de tipo ativo. |
| `escala/management/commands/termometro_regua.py` | **B :35** — EC ativo para régua. |
| `escala/management/commands/celulas_contra_o_template.py` | **B :28** — mapa de vínculo ativo contra células. |

Consultas dos commands, prefixo `app/`:

| Arquivo | Sítios e classificação |
|---|---|
| `colaboradores/management/commands/tripwire_vinculo_orfao.py` | **C :45** — estado ativo de colaborador fora de situação ativa. |
| `colaboradores/management/commands/fechar_vinculo_de_desligado.py` | **C :34** — alvos de fechamento. |
| `colaboradores/management/commands/sincronizar_feriado_admin.py` | **C :42** — universo de atualização cadastral. |
| `ponto/management/commands/detectar_par_relampago.py` | **C :336** — universo ativo/sem fim para detector. |
| `ponto/management/commands/diff_espelho_alimentacao.py` | **B :94** — caminho legado do diff por ativa. |
| `ponto/management/commands/detectar_cluster_espurio.py` | **C :71** — universo ativo/sem fim para detector. |
| `ponto/management/commands/tripwire_celulas.py` | **B :17** — mapa de EC ativo contra célula; **C :57** — universo ativo, depois regra temporal própria `ec.data_inicio <= d <= teto` em `:67`, sem fim/juiz. |
| `ponto/management/commands/reconciliar_fantasmas.py` | **B :173** — EC ativo para diagnóstico. |
| `ponto/management/commands/checklist_golive.py` | **C :70** — conta pessoas sem join de ativa. |
| `ponto/management/commands/gerar_batidas_smoke_motor.py` | **C :73** — universo smoke, ativa mais `data_inicio__lte=PERIODO_FIM`, regra própria. |
| `ponto/management/commands/detectar_ausencias.py` | **C :185** — universo de colegas do template. |
| `ponto/management/commands/propor_reorganizacao.py` | **C :42** — universo ativo intermitente. |
| `ponto/management/commands/fechar_chamados_meio_periodo.py` | **B :60** — tipo ativo. |
| `ponto/management/commands/detectar_intervalo_ausente.py` | **B :115** — tipo ativo. |
| `ponto/management/commands/censo_fase_12x36.py` | **B :403** — `_curar` escolhe ativo mais recente. |
| `escala/management/commands/corrigir_vinculo_vigencia.py` | **C :100, :124** — listas Python de `x.ativa` com limites de início para planejar correção, regras da operação. |

No próprio escritor `app/colaboradores/services/vinculo.py`, todos são **C**, porque fotografam estado ou escolhem alvos da operação, em vez de responder qual vínculo cobre dia: **:119** guarda de no-op (`existente.ativa`), **:183** foto anterior, **:215** alvos ativos a fechar, **:242** inativos sobrepostos com limites próprios, **:322** EC recém-escrita por data inicial exata, **:405** EC para aceite, **:523** censo de sobreposição (compara intervalos próprios em `:537`), **:711** no-op da reabertura, **:788** absorção de inícios `>=D`, **:863** fechamento de ativos, **:990** invalidação de âncoras.

Outros critérios Python/ordenação, prefixo `app/`:

| Sítio | Classe e uso |
|---|---|
| `escala/alimentacao.py:150`, `:153`, `:196`, `:202`, `:286` | **C**, implementação da própria autoridade/carregador, não leitor externo. |
| `escala/folgas.py:459` | **A** — desempata candidato ativo depois de `q_vinculo_cobre_a_data` em `:451`. |
| `ponto/portas/celula.py:908` | **A** — guarda ativa após `vinculo_cobre_a_data` em `:906`, e consulta `vinculo_do_dia` em `:909`. |
| `ponto/turnos.py:1560` | **B** — `_escalas_da_ata`: ordena `ativa,data_inicio`, último vence, sem data; dívida explicitada em `:1541`. |
| `ponto/nucleo.py:15` | **A** como alimentação: candidatos vêm de `escalas_do_periodo`; a ordenação conserva precedência da autoridade. |
| `relatorios/views.py:1320` | **B** — retorna primeiro `ec.ativa` de `c.escalas.all()`. |
| `relatorios/management/commands/vinculo_do_dia_divergentes.py:118` | **C** — regra temporal própria legada de comparação (`:116`), contra juiz em `:115`. |
| `chamados/services/carga_pergunta_viva.py:97` | **C** — ordena vínculos da carga para leitores C6; não escolhe EC nessa linha. |
| `ponto/portas/regen_exportada.py:180` | **C** — `bool(_ec.ativa)` decide extensão de horizonte. |
| `escala/admin.py:28` | **C** — filtro administrativo por estado `ativa`. |
| `colaboradores/services/ficha.py:94` | **C** — serializa estado, sem filtro/decisão de cobertura. |
| `escala/services/vigencia_sem_trilha.py:78` | **C** — serializa estado, sem filtro/decisão de cobertura. |
| `colaboradores/services/vinculo.py:37`, `:111`, `:497`, `:639` | **C** — fotos/trilha/serialização de estado. |

Fora de `app/`: **C `bin/censo_folha.py:25`**, contagem `c.escalas.filter(ativa=True).count()`.

Leituras de ativa dentro de testes (não confundir com criação de fixtures): todas **C** como asserção/arranjo do teste, prefixo `app/`: [nao verificado]

- `colaboradores/tests/test_vincular_escala.py:110`; `test_fase_12x36_tela.py:105`; `test_ficha_fonte_dos_totais.py:68`; `test_o207_vinculo_inicio_declarado.py:157`, `:231`, `:266`, `:309`, `:324`; `test_vinculo_linha_do_tempo.py:73`, `:125`; `test_bug110_rota_da_ui_vincula.py:83`; `test_vincular_escala_selos.py:52`; `test_e4cal_l1_calendario_le_o_d4.py:88`; `test_escala_aceite.py:78`.
- `ponto/tests/test_alertas_nucleo.py:80`, `:154`, `:198`, `:241`; `test_fluxos.py:129`; `test_e4cal_l1_d4_natureza.py:84`.
- `core/tests/test_haiku_cirurgico.py:121`.
- `escala/tests/test_termometro_camada3.py:102`; `test_vigencia_um_escritor.py:79`, `:160`; `test_e4_limbo_pergunta_a_juiza.py:41`.
- `chamados/tests/test_e4b_c12_orm.py:82`.

Não executei testes nem usei banco/log; efeitos em produção e exaustividade além dos sítios encontrados por busca estática ficam [nao verificado]. [nao verificado]

## 15/11 20:06 · sessao ab3d0959c51a · - · - · origem=Ronald (tmux)

O número **19** documenta um ensaio de **UniqueConstraint por colaborador com `ativa=True`**, incluindo vínculos adjacentes; não prova 19 falhas de EXCLUDE por sobreposição. O relato nomeia somente oito testes. Identidade dos outros 11 e reprodução da contagem nesta cópia: **[nao verificado]**. (`app/docs/RELATO-ARQUIVO.md:3094`, `:3099`, `:3102`).

| Teste nomeado no relato | Construção comprovada | Propósito | [nao verificado]
|---|---|---|
| `test_01_colisao_escala_multiplas_ativas` | Dois ativos sem fim, iniciados hoje e 30 dias antes (`app/chamados/tests/test_detector_avancado.py:121`, `:123`, `:124`; helper `:79`) | Intencional: detectar colisão (`:122`, `:129`). |
| `test_MORDE_a_absorcao_deixa_trilha_nos_DOIS_lugares` | Dois ativos abertos em D e depois de D (`app/colaboradores/tests/test_vinculo_linha_do_tempo.py:95`, `:96`, `:97`; helper `:67`) | Intencional: absorção com histórico e auditoria (`:98`, `:99`, `:105`). |
| `test_MORDE_o_censo_acha_a_sobreposicao_e_a_cura_a_zera` | Mesma sobreposição (`app/colaboradores/tests/test_vinculo_linha_do_tempo.py:132`, `:133`, `:134`) | Intencional: censo antes/depois da cura (`:136`, `:139`). |
| `test_escala_vigente_vence_first` | **Adjacentes**, velha termina ontem e nova começa hoje; ambas ativas (`app/ponto/tests/test_turno_aberto_calc_janela.py:31`, `:46`, `:48`) | Intencional: escolher pela vigência (`:45`, `:51`). |
| `test_dia_sob_escala_antiga_usa_marcos_antigos` | **Adjacentes**, velha termina D−1 e nova começa D; ambas ativas (`app/ponto/tests/test_turnos_do_colab_troca_escala.py:38`, `:42`, `:43`, `:45`) | Intencional: marcos históricos (`:1`, `:51`). |
| `test_celula_da_vigencia_certa` | **Adjacentes**, antiga termina 04/08 e nova começa 05/08; ambas ativas (`app/ponto/tests/test_pareador_dna_multivig.py:81`, `:83`, `:85`, `:87`) | Intencional: célula nasce da vigência certa (`:62`, `:90`). |
| `test_dt_fim_por_te_vigente_na_data` | **Adjacentes**, anterior termina D−1 e nova começa D; ambas ativas (`app/ponto/tests/test_nucleo_dt_fim_vigencia.py:38`, `:42`, `:43`, `:45`) | Intencional: fim previsto por dia (`:1`, `:50`). |
| `test_dia_dentro_da_vigencia_do_novo_usa_o_novo` | Antigo aberto desde 01/06; novo ativo de 01/07 a 15/07: **sobreposição real** (`app/ponto/tests/test_vigencia_partida.py:29`, `:30`, `:34`, `:37`) | Intencional: seleção histórica e respeito a `data_fim` (`:1`, `:48`, `:55`). |

Outras construções atuais, sem prova de pertencerem aos 19 históricos:

- **Sobreposição entre ativos, intencional:** absorção, idempotência e fechamento do anterior (`app/colaboradores/tests/test_vinculo_linha_do_tempo.py:82`, `:108`, `:118`); fechamento/recusa, invariante e trilha de dois vínculos (`app/escala/tests/test_vigencia_um_escritor.py:72`, `:91`, `:152`).
- **Ativo aberto e futura ativa, intencional:** fixture que compara os dois juízes nas bordas (`app/escala/tests/test_e4_vinculo_cobre_a_data.py:25`, `:26`, `:29`, `:43`, `:51`).
- **Ativo e inativas com fim sobrepostos, intencional:** contador de inconsistências (`app/escala/tests/test_vinculos_inconsistentes.py:30`, `:41`, `:42`, `:43`, `:54`); desempate da ficha (`app/escala/tests/test_e4_parte4_cinco_copias.py:76`, `:80`, `:81`); desempate do tipo vigente (`app/escala/tests/test_e4_parte5_copias.py:116`, `:122`, `:126`); referência do fechamento (`app/ponto/tests/test_referencia_pergunta_a_juiza.py:29`, `:31`, `:33`, `:40`); calendário versus geradora (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:88`, `:91`, `:93`, `:100`).
- **Intervalos brutos sobrepostos com inativa sem fim, intencional:** seleção pela célula geradora (`app/ponto/tests/test_vinculo_do_dia_pela_celula.py:30`, `:67`, `:70`, `:93`). A inativa sem fim não é candidata pelo juiz atual (`app/escala/tests/test_e4_vinculo_cobre_a_data.py:35`, `:40`).
- **Dublês sem banco, sobreposição intencional:** domínio da geradora (`app/ponto/tests/test_cadastro_futuro_c5b.py:104`, `:122`, `:127`, `:141`); fechamento de inativa sobreposta no mesmo ato (`app/ponto/tests/test_cadastro_futuro_c5e.py:62`, `:68`, `:82`); feriado respeita geradora (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:688`); fatias alternadas A/B/A (`app/ponto/tests/test_composicao_de_fatias_e_total.py:291`, `:293`, `:294`, `:295`).

Não há evidência suficiente para chamar esses casos de “fixture descuidada”; o propósito é explícito nos testes acima. Classificação dos demais testes/fixtures dinâmicos e inventário completo de todos os estados produzidos por execução: **[nao verificado]**. [nao verificado]

Quanto às constraints:

- O modelo tem unicidade de `(colaborador, data_inicio)` e somente a CHECK `ec_vigencia_fim_nunca_antes_do_inicio` na lista de constraints (`app/escala/models.py:1004`, `:1037`, `:1041`).
- A migration 0041 declara essa CHECK por `SeparateDatabaseAndState` e SQL **NOT VALID** (`app/escala/migrations/0041_ec_vigencia_not_valid.py:31`, `:35`, `:43`, `:46`).
- A migration **0042 atual** adiciona campos de HE em `TipoEscala`; não é a unique descrita no ensaio (`app/escala/migrations/0042_o146_he_extra_declarada.py:14`, `:17`, `:26`).
- O modelo afirma que a unique e seu selo ficaram no scratchpad (`app/escala/models.py:1019`, `:1036`). O SQL com `btree_gist` e EXCLUDE aparece no relato (`app/docs/RELATO-ARQUIVO.md:21721`, `:21722`, `:21725`); instalação efetiva desse SQL em produção: **[nao verificado]**.
- A rejeição de EXCLUDE com NOT VALID está registrada como medição histórica no relato; não foi reproduzida nesta investigação (`app/docs/RELATO-ARQUIVO.md:21712`, `:21714`).

## 15/11 20:06 · sessao 1a00fab08cd3 · - · - · origem=Ronald (tmux)

Funções e escopo para a sonda, com base apenas no código:

- Conta mensal real: `ponto.services.fechamento.recalcular_fechamento_mes(mes, ano, empresa_id=..., colaborador_ids=..., somente_leitura=True, retornar_objeto=True, lavrar_dias_pagos=False)`. Retorna o próprio `FechamentoMensal` destacado, evitando reconstruir o objeto a partir do recorte de campos. `app/ponto/services/fechamento.py:448`, `app/ponto/services/fechamento.py:890`, `app/ponto/services/fechamento.py:1128`
- Essa conta monta o motor por `get_motor_cct` e chama `apurar_por_fatia`; portanto, chamar somente `parear_turnos` ou somar duração bruta não reproduz o fechamento mensal. `app/ponto/services/fechamento.py:735`, `app/ponto/services/fechamento.py:754`
- Universo canônico: `universo_da_competencia(mes, ano, empresa_id)`; inclui situações diferentes de ativo e aplica a janela da empresa aos desligados. Não substituir por filtro `situacao='ativo'`. `app/ponto/services/fechamento.py:382`, `app/ponto/services/fechamento.py:428`
- Janela real: `janela_fechamento(mes, ano, empresa)`. Competência não equivale necessariamente ao mês civil. `app/ponto/janelas.py:10`, `app/ponto/janelas.py:54`
- Excluir empresas/competências entregues por `folha.export.competencia_entregue(empresa, mes, ano)`: verifica TXT vigente não invalidado. O status `aberto` do fechamento não responde essa pergunta. `app/folha/export.py:556`, `app/folha/export.py:588`

Existe instrumento real: `ponto.services.diff_frota.medir`, com três colunas: gravado, cálculo atual intacto, cálculo após mutação; usa `atomic` e `_Rollback`. `somar` distingue efeito (`novo-hoje`), deriva (`hoje-gravado`) e alteração contra gravado (`novo-gravado`). `app/ponto/services/diff_frota.py:45`, `app/ponto/services/diff_frota.py:72`, `app/ponto/services/diff_frota.py:101`

Há duas limitações a declarar se reutilizado: nasce exclusivamente dos `FechamentoMensal` existentes e ignora universo sem fechamento; e sua mutação corre por empresa numa única transação, então monkeypatch global mantido após a primeira empresa contaminaria o “hoje” das seguintes. Usar uma empresa por chamada, restaurando o patch fora dela. `app/ponto/services/diff_frota.py:62`, `app/ponto/services/diff_frota.py:76`, `app/ponto/services/diff_frota.py:84`

Esboço integral, sem segundo cálculo:

```python
# Esboço: somente memória/stdout; não executar neste trabalho.
for empresa, mes, ano in competencias_declaradas:
    if competencia_entregue(empresa, mes, ano):
        continue

    ids = list(universo_da_competencia(
        mes, ano, empresa.pk).values_list("pk", flat=True))

    try:
        with transaction.atomic():
            antes = fotografar(empresa, mes, ano, ids)

            # Contexto entra e sai antes da próxima empresa.
            # Substitui em memória a função/cura candidata REAL,
            # sem reimplementar pareamento na sonda.
            with candidata_em_memoria:
                depois = fotografar(empresa, mes, ano, ids)

            raise RollbackSonda()
    except RollbackSonda:
        registrar_delta_em_memoria(antes, depois)

imprimir_apenas_colNN_e_agregados()
```

`fotografar` deve chamar a função real acima com objetos destacados; depois chamar `classificar_export(empresa,mes,ano,fechamentos=objetos)` uma vez para o lote, guardar apenas colaborador, status, motivo e travas, descartando `nome`, `codigo_dominio` e detalhes textuais pessoais. A função retorna esses campos pessoais, logo não imprimir o retorno cru. `app/folha/export.py:765`, `app/folha/export.py:770`, `app/folha/export.py:783`

Para as rubricas efetivamente emitíveis, chamar `montar_linhas_export(..., itens=itens_classificados, fechamentos=objetos)`, que recebe a mesma injeção e não grava exportação nem aprovação. Comparar com `agregar_export_dominio`, parser existente. `app/folha/export.py:690`, `app/folha/export.py:697`, `app/folha/export.py:714`, `app/folha/export.py:310`

Para rubricas por `colNNN` sem ambiguidade de matrícula repetida, a fotografia pode chamar a montagem por objeto (`fechamentos=[f]`, classificação do lote já calculada), agregar e somar seus valores por rubrica internamente; nunca imprimir as linhas TXT, que carregam matrícula. `app/folha/export.py:716`, `app/folha/export.py:725`, `app/folha/export.py:728`, `app/folha/export.py:738`

Manter duas vistas: eventos derivados mesmo para retidos (`eventos_do_fechamento`) e rubricas realmente emitíveis após classificação. A primeira não resolve código de rubrica; a segunda inclui o gate de exportação. `app/folha/export.py:402`, `app/folha/export.py:439`, `app/folha/export.py:730`

Não rotular todo valor de rubrica como horas: o parser preserva valor cru e o próprio código registra que horas versus dias depende do catálogo. `app/folha/export.py:241`

Saída sugerida: por competência, `colNNN`, campo, antes, depois, delta; rubrica, unidade, antes/depois/delta; transições de retenção; totais, quantidade que aumentou/diminuiu e distribuição por campo/rubrica. Incluir faltantes em cada retorno como “sem resposta”, nunca zero: o recalculador captura erro por colaborador, registra e continua. O logger desse erro inclui nome, portanto a futura sonda precisa interceptar/redigir esse canal antes de emitir somente o dado raspado. `app/ponto/services/fechamento.py:1303`

Para col727, a função apta a reproduzir o **fechamento mensal atual** é `recalcular_fechamento_mes(...colaborador_ids=[727],somente_leitura=True,retornar_objeto=True)`; a fachada individual `fechamento_lido(colaborador,mes,ano)` chama exatamente essa conta. `app/ponto/services/fechamento.py:1759`

Os números históricos do anexo não são reproduzíveis com os insumos fornecidos: faltam batidas completas, competência, vínculos/células e demais fatos de cálculo; reprodução histórica na cópia atual também não está demonstrada. `[não verificado]` Há inconsistência aritmética no próprio anexo: `159,49−135,47=24,02 h`, portanto esses dois totais não demonstram `38,02 h`; o referencial desse segundo número não foi fornecido. `[não verificado]`

## 15/11 20:06 · sessao e3f972574a41 · - · - · origem=Ronald (tmux)

Censo de escritores, somente leitura:

- **Porta única declarada = módulo `app/colaboradores/services/vinculo.py`, não apenas `executar_vinculo`**: `app/core/portas.py:470`; gate com allowlist/pêndencias vazias e proibição de escrita fora da porta: `app/escala/tests/test_chokepoint_escala_gate.py:27`, `:28`, `:55`.
- **Nenhum escritor de produção de `EscalaColaborador.data_inicio/data_fim/ativa` fora desse módulo encontrado** na busca textual, busca por SQL/tabela e execução puramente estática de `core.censo_escritas.varrer_arvore`; limites do detector (atribuição e balde não atribuível) documentados em `app/core/censo_escritas.py:6`, `:43`, `:54`, `:469`. Não executei Django/testes/banco.
- **`executar_vinculo` já absorve posteriores**, contrário ao trecho antigo do backlog: fecha anteriores em `app/colaboradores/services/vinculo.py:214`, inativas sobrepostas em `:241`, absorve posteriores em `:253`, reativa existente em `:276`/`:277`/`:283`, cria em `:287`/`:291`/`:292`. Atomic: `:169`.

Portas:

| Porta | Caminho constatado |
|---|---|
| Wizard fase | Só leitura/preenchimento: `app/colaboradores/views_fase.py:5`, `:73`, `:93`; está dentro do formulário que faz POST em `vincular_escala`: `app/templates/colaboradores/partials/painel_vinculo.html:136`, `:157`; view chama juiz/executor em `app/colaboradores/views.py:411`, `:423`. |
| Cadastro de escala/painel | Mesmo formulário/mesma view acima: `app/templates/colaboradores/partials/painel_vinculo.html:136`; `app/colaboradores/views.py:423`. |
| Admissão | **Não escreve EC e não chama executor**: cria `Colaborador` em `app/colaboradores/views.py:1184`; a função só faz `Colaborador.objects.create` em `app/colaboradores/services/colaborador.py:91`; registra `HistoricoVinculo` e encerra resposta em `app/colaboradores/views.py:1220`, `:1230`. |
| Edição de cadastro | Salva `Colaborador`, não EC: `app/colaboradores/views.py:1344`, `:1345`. |
| Django admin EC | Bloqueado, não atravessa executor porque é somente leitura: `app/escala/admin.py:19`; add/change/delete negados e todos campos readonly em `app/core/admin.py:59`, `:62`, `:65`, `:68`. |
| Desvincular da escala | `encerrar_vinculo` do módulo: `app/escala/views.py:267`, `:269`. |
| Desligamento | `fechar_vinculos_ativos` do módulo: `app/colaboradores/services/desligamento.py:147`, `:148`. |
| Chat | `julgar_vinculo` e `executar_vinculo`: `app/colaboradores/services/troca_pelo_chat.py:72`, `:75`. |
| Régua/transferência | Mesma dupla: `app/escala/regua_defesa.py:316`, `:321`. |
| Command censo fase | Mesma dupla: `app/ponto/management/commands/censo_fase_12x36.py:422`, `:423`. |
| Command corrigir vigência | Usa reabrir/fechar/absorver do módulo: `app/escala/management/commands/corrigir_vinculo_vigencia.py:86`, `:107`, `:120`. |
| Command corrigir retroativa | Usa criar trecho/corrigir vínculo do módulo: `app/colaboradores/management/commands/corrigir_escala_retroativa.py:195`, `:196`, `:211`, `:212`. |
| Command fechar desligados | Usa encerrar do módulo: `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:55`, `:56`. |
| Aceite recusado | Restauração pelo módulo: `app/chamados/services/acoes_chamado.py:147`, `:149`. |
| Ajuste automático/desfazer | Funções do módulo: `app/escala/services/escala_auto_executor.py:94`, `:103`, `:124`, `:145`. |
| Signals EC | Leem estado anterior e regeneram células, invalidam previsto, revalidam disputas/reparentam folgas; nenhum escreve os três campos: `app/escala/signals.py:18`, `:25`, `:34`, `:73`, `:86`, `:125`, `:137`, `:145`, `:154`, `:174`, `:185`, `:203`. |

**Outros escritores, todos DENTRO do módulo** (útil para evitar chamar `executar_vinculo` de função única):

| Escrita | Referência |
|---|---|
| Fechamento `(ativa=False,data_fim)` | `app/colaboradores/services/vinculo.py:641` |
| Reabertura `(ativa=True,data_fim)` | `app/colaboradores/services/vinculo.py:730` |
| Absorção `ativa=False`, fim preservado | `app/colaboradores/services/vinculo.py:799` |
| Encerramento por save | `app/colaboradores/services/vinculo.py:843`, `:844`, `:845` |
| Criar trecho retroativo inativo | `app/colaboradores/services/vinculo.py:970`, `:972`, `:973` |
| Correção retroativa de fim | `app/colaboradores/services/vinculo.py:982`, `:983` |
| Restaurar snapshot, inclusive início/fim/ativa | `app/colaboradores/services/vinculo.py:1031`, `:1032` |
| Ajuste automático: fechar e criar | `app/colaboradores/services/vinculo.py:1087`, `:1089`, `:1090` |
| Desfazer: reabrir ou fechar/criar | `app/colaboradores/services/vinculo.py:1106`, `:1108`, `:1110`, `:1111`, `:1112` |

Migrations encontradas de EC são estrutura/schema, não escritores diretos adicionais dos três campos; definição original e defaults: `app/escala/migrations/0001_initial.py:36`; alterações de constraints/unique: `app/escala/migrations/0004_alter_escalacolaborador_unique_together.py:15`; CHECK vigência: `app/escala/migrations/0041_ec_vigencia_not_valid.py:23`. Testes/fixtures escrevem EC diretamente e estão fora do escopo de produção do gate por desenho: `app/core/censo_escritas.py:54`, `:66`, `:81`.

## 15/11 20:05 · sessao 055c14243405 · - · - · origem=Ronald (tmux)

Censo comprovado por código, sem banco/log:

| Leitor | Caminho real e campos | Semântica / ressalva |
|---|---|---|
| Geometria compartilhada | `Turno = (data_turno, entrada, saida, batidas, aberto, cross_meianoite)` em `app/ponto/turnos.py:43`; `_turnos_do_colab_puro` chama `parear_turnos` com marcos e papel da ata em `app/ponto/turnos.py:1992`; filtra pelo `t.data_turno` em `app/ponto/turnos.py:1995`. | `entrada`/`saida` são Batidas, seus timestamps são instantes; `data_turno` é decisão do juiz, podendo ser véspera, não sinônimo de data civil (`app/ponto/turnos.py:248`, `app/ponto/turnos.py:285`). |
| Motor | `turnos_via_autoridade` → `parear_turnos` (`app/ponto/motor_calculo_v2.py:503`); `_periodos_via_autoridade` → `periodos_de_turnos` (`app/ponto/motor_calculo_v2.py:530`). Adaptador publica `(entrada.timestamp, saida.timestamp|None)` e intervalos julgados (`app/ponto/adaptador_motor.py:37`, `app/ponto/adaptador_motor.py:44`). | Aberto vira período com saída None + anomalia; S órfã vira só anomalia (`app/ponto/adaptador_motor.py:30`, `app/ponto/adaptador_motor.py:39`). O mapa de dia liga instante de entrada a `t.data_turno` (`app/ponto/motor_calculo_v2.py:884`, `app/ponto/motor_calculo_v2.py:894`), mas há segunda receita: `_turnos_do_juiz` usa `turnos_de_batidas` e diz que parâmetros AUT do motor podem diferir (`app/ponto/motor_calculo_v2.py:867`, `app/ponto/motor_calculo_v2.py:882`). |
| Núcleo materializado | `recompute_turnos` chama `turnos_do_colab` e copia literalmente `data_turno`, `entrada`, `saida`, `aberto`, `cross_meianoite` para `TurnoMaterializado` (`app/ponto/nucleo.py:47`, `app/ponto/nucleo.py:50`). | Preserva os significados da geometria, mas é estado persistido; reconstrução faz delete + bulk_create em atomic (`app/ponto/nucleo.py:57`). |
| Cartório / grade | Cartório chama `montar_grade_prevista_periodo` (`app/ponto/services/cartorio.py:1175`); grade pergunta `turnos_do_colab` antes do alinhamento, indexa por `data_turno` (`app/escala/utils.py:1581`), e pergunta novamente com cópias de células e lâmpadas desta montagem (`app/escala/utils.py:1886`, `app/escala/utils.py:1898`). Calcula o realizado via `realizado_do_dia(... turnos=_turnos_juiz)` (`app/escala/utils.py:2045`). | Ata grava `minutos_realizados`, lâmpadas e órfãs (`app/ponto/services/cartorio.py:736`, `app/ponto/services/cartorio.py:740`). Não é o mesmo conceito de horas pagas. Dia sem turno recebe zero explicitamente na grade (`app/escala/utils.py:2058`). O cartório também usa `dias_encerrados` para fim de dia por instante/agora (`app/ponto/services/cartorio.py:1153`). |
| Espelho tela | Lê períodos do motor e `p.turno_aberto` (`app/ponto/services/espelho.py:421`); apresenta batidas e cards via `dia_das_batidas` e ponte instante→pk→dia (`app/ponto/services/espelho.py:450`, `app/ponto/services/espelho.py:458`). Também chama `turnos_do_colab` para geometria e datas de S órfã (`app/ponto/services/espelho.py:614`). | Divergência concreta: contador de turnos recorta competência por **data civil da entrada** (`app/ponto/services/espelho.py:426`), enquanto `datas_turno_aberto` recorta por **dia do juiz** (`app/ponto/services/espelho.py:471`). Dinheiro da linha vem de DiaPago, não do realizado cru (`app/ponto/services/espelho.py:406`, `app/ponto/services/espelho.py:410`). |
| Espelho PDF | Chama `turnos_do_colab` na janela inteira (`app/relatorios/pdf_espelho.py:213`); contador é `len(turnos_do_colab(...))` (`app/relatorios/pdf_espelho.py:451`). | O contador PDF conta geometria da janela inteira; a tela conta períodos do motor. Diferença está documentada no próprio sítio (`app/relatorios/pdf_espelho.py:444`). |
| Chamados: reconciliação | `turnos_abertos_de` lê **TurnoMaterializado**, com entrada presente, saída ausente e dia em `{dia-1,dia}` (`app/chamados/reconciliador.py:96`). | Não consulta geometria viva neste ponto; aplica veto por `dia_posicional_completo` (`app/chamados/reconciliador.py:112`), portanto não é consumo puro do mesmo booleano aberto. |
| Chamados: juízes / detector | Juiz de disputa pergunta `turnos_do_colab` ou lote, filtra `data_turno==dia`, exige todos fechados (`app/chamados/juizes.py:401`); detector localiza batida por timestamp em `t.batidas`, usa `t.data_turno` para folga (`app/chamados/detector_anomalias.py:157`, `app/chamados/detector_anomalias.py:162`, `app/chamados/detector_anomalias.py:166`). | Timestamp como chave no detector pode colidir entre duplicadas; mapa de apresentação deliberadamente usa pk (`app/ponto/turnos.py:2904`). Comentário do detector chama data_turno de data civil da entrada (`app/chamados/detector_anomalias.py:154`), mas implementação lê a resposta do juiz (`app/chamados/detector_anomalias.py:163`). |
| Chamados: materialização | Origem de turno procura `t.aberto`, entrada com timestamp anterior à criação do chamado (`app/chamados/services/materializacao.py:124`, `app/chamados/services/materializacao.py:127`); contador `turnos_sem_par` conta `t.aberto or t.entrada is None` (`app/chamados/services/materializacao.py:221`). | Primeira função é consulta por instante de criação; segunda é geometria por janela de dias, incluindo órfãs, não só aberto (`app/chamados/services/materializacao.py:123`, `app/chamados/services/materializacao.py:219`). |
| Lotes | `CargaDaJanela.turnos` chama o mesmo `_turnos_do_colab_puro` com cópias de batidas (`app/ponto/turnos.py:3036`); carga C6 faz o mesmo por janela pedida (`app/chamados/services/carga_pergunta_viva.py:181`). | Mesmo corpo de geometria; caches têm janela e lifetime próprios (`app/ponto/turnos.py:3028`, `app/chamados/services/carga_pergunta_viva.py:168`). |
| Folha | `eventos_do_fechamento` traduz fechamento em eventos/horas (`app/folha/export.py:402`); `montar_linhas_export` aceita fechamentos injetados ou lê persistidos e usa `classificar_export` (`app/folha/export.py:714`, `app/folha/export.py:717`). | Exportação não reinterpreta Turno. O dinheiro vem do FechamentoMensal; dia pago usa mapa `id(periodo)→dia` se presente e **data civil da entrada se ausente** (`app/ponto/services/dia_pago.py:126`, `app/ponto/services/dia_pago.py:128`, `app/ponto/services/dia_pago.py:132`). Soma por competência arredonda campos a 2 casas (`app/ponto/services/dia_pago.py:559`, `app/ponto/services/dia_pago.py:563`). |

A regra de apresentação completa é: cada batida reclamada por turno recebe `t.data_turno`; qualquer batida remanescente recebe sua `data_local`, por pk (`app/ponto/turnos.py:2949`, `app/ponto/turnos.py:2954`). Portanto “mesmo campo” não autoriza igualar `data_turno`, dia civil, instante, minuto realizado e hora paga (`app/ponto/turnos.py:43`, `app/ponto/services/espelho.py:398`, `app/ponto/services/dia_pago.py:128`).

Chamadas diretas adicionais a `parear_turnos` com possibilidade de absorção por marco: `turnos_de_batidas` (`app/ponto/turnos.py:1535`), `_turno_aberto_puro` (`app/ponto/turnos.py:2278`), reconciliação de fantasmas (`app/ponto/management/commands/reconciliar_fantasmas.py:99`). Chamadas diretas sem marcos de intervalo, portanto sem essa absorção específica: `app/ponto/utils_turno_orfao.py:31`, `app/escala/services/semana_do_posto.py:120`, `app/escala/services/meio_periodo.py:168`, `app/chamados/services/fila_por_causa.py:197`, `app/ponto/services/diagnostico_escala.py:142`, `app/ponto/services/lastro.py:268`; piso de intervalo passa explicitamente `marcos_intervalo=None` (`app/ponto/services/piso_intervalo.py:48`).

Alerta para item 1: HEAD já menciona e executa um passo O95 posterior `_fechar_aberto_na_pausa_sem_volta`; os comentários o listam como terceiro sítio de fechamento (`app/ponto/turnos.py:121`). O anexo pode estar descrevendo o ramo ainda existente, sem contemplar a reparação posterior; conferir corpo e ordem desse pós-processamento (`app/ponto/turnos.py:121`).

## 15/11 20:05 · sessao 269ddcfaaf7d · - · - · origem=Ronald (tmux)

Classes encontradas além da absorção por marco de intervalo:

| Classe | Prova exigida e efeito |
|---|---|
| **Eco de cluster junto de luz acesa, inclusive bordas de entrada/saída** | Batida real dentro do vão da ata, a ≤15 min de alguma luz, recebe papel `X`; vira interna e não abre/fecha turno. Não exige uma volta própria, mas exige ata com entrada e saída, luzes ligadas a batidas reais únicas e minuto não ambíguo. `app/ponto/turnos.py:1655`, `app/ponto/turnos.py:1682`, `app/ponto/turnos.py:1731`, `app/ponto/turnos.py:1742`, `app/ponto/turnos.py:1804`; limite em `app/escala/orfa_classe.py:28`; consumo em `app/ponto/turnos.py:1316`, `app/ponto/turnos.py:1371`. |
| **`S,S` sem volta registrada, com segunda saída perto da borda** | `_marca_intra_duracao` marca a primeira `S` como interna se ela está longe das bordas e encontra `S` posterior dentro de `cont_max_s` que casa borda a ≤90 min. Não há prova `E` de volta, mas há prova de `S` posterior; portanto não é a ausência total de continuidade da O95. `app/ponto/turnos.py:476`, `app/ponto/turnos.py:481`, `app/ponto/turnos.py:485`. |
| **Atribuição de batida a marco de borda por alinhamento** | Na rota sem papel de turno, `_match_marcos` seleciona batidas no envelope `[primeiro marco−tol, último marco+tol]` e usa DP de distância. Batida única pode acender marco sem parceiro; o cluster guard só atua havendo mais de uma. É atribuição de luz, não fabricação de batida nem prova de turno fechado. `app/escala/utils.py:625`, `app/escala/utils.py:633`, `app/escala/utils.py:641`; custo do DP em `app/escala/utils.py:229`; `tol_min=90` em `app/escala/utils.py:668`. |
| **Recorte monetário das bordas pela janela de HE** | Entrada anterior/saída posterior ao limite cadastrado podem ser substituídas pelo marco efetivo para cálculo; minutos externos ficam fora. Parte de pontas reais já informadas, sem criar presença ou volta. Autorização positiva preserva a ponta real. Entrada: `app/ponto/motor_calculo_v2.py:1691`, `app/ponto/motor_calculo_v2.py:1697`, `app/ponto/motor_calculo_v2.py:1702`; regra pura em `app/ponto/janela_he.py:92`. Saída: `app/ponto/motor_calculo_v2.py:1733`, `app/ponto/motor_calculo_v2.py:1739`; regra em `app/ponto/janela_he.py:143`. |
| **Reclassificação de E/S pelo marco** | `tipo_por_marcos` devolve o tipo do único tipo de marco dentro de 30 min; `decidir_tipo_estrito` aceita tipo oposto a ≤10 min quando o próprio está a >30 min. Ambas classificam uma batida real sem exigir parceiro. Não são absorção como intervalo, mas pertencem à classe ampla de inferência de papel pelo marco. `app/ponto/turnos.py:2397`, `app/ponto/turnos.py:2414`, `app/ponto/turnos.py:2424`, `app/ponto/turnos.py:2435`. |

**Tolerância de pontualidade não é absorção de batida:** `aplicar_tolerancia` só retorna atraso/antecipada, comparando instantes reais aos previstos; a implementação usa `TOLERANCIA_CONFORMIDADE_MIN=10`, embora a docstring diga cinco. Não altera horas trabalhadas nessa função. `app/ponto/motor_calculo_v2.py:60`, `app/ponto/motor_calculo_v2.py:1952`, `app/ponto/motor_calculo_v2.py:1959`, `app/ponto/motor_calculo_v2.py:1973`, `app/ponto/motor_calculo_v2.py:2006`. O raio `tol_min` do matcher é atribuição, explicitamente distinto do desconto em folha. `app/escala/utils.py:653`.

Para ESMERIL-SEMÂNTICA:

- `papel_por_minuto` identifica **instante local truncado ao minuto, naive**, não dia civil nem dia do turno; timestamps crus continuam na aritmética. Emissão e consulta fazem a mesma transformação. `app/ponto/turnos.py:1604`, `app/ponto/turnos.py:893`, `app/ponto/turnos.py:904`.
- `papel_por_minuto_da_ata` delimita o **vão do turno por entrada/saída da ata**, não por contagem de batidas no dia civil; evita usar meia ata e colisões entre células vizinhas. `app/ponto/turnos.py:1619`, `app/ponto/turnos.py:1682`, `app/ponto/turnos.py:1730`, `app/ponto/turnos.py:1802`.
- A janela de saída recebe **datetime do marco já resolvido**, pois um `time` não determina o dia da saída em jornada que cruza meia-noite; a entrada recebe `time` e o coloca no dia do instante de entrada. `app/ponto/janela_he.py:20`, `app/ponto/janela_he.py:94`, `app/ponto/janela_he.py:126`.
- `em_intervalo` interpreta **estado**: turno aberto cuja última batida é `S`; não usa relógio nem exige que a volta já exista. Isso explica por que uma cura na absorção precisa preservar o almoço em curso. `app/ponto/turnos.py:2498`, `app/ponto/turnos.py:2503`, `app/ponto/turnos.py:2508`.

## 15/11 20:05 · sessao 5d92ac32c131 · O169-FECHADA · leitura · origem=Code

1. **A regra de idade já saiu do fechamento.** `--dias` limita os dias consultados por `dias_com_cluster`; `pendencias` usa essa janela na abertura, mas calcula `a_fechar` por `_vivos_que_a_premissa_matou(colab)`, sem janela (`app/ponto/services/cluster_espurio.py:40-49`, `:165-173`). O fecho lê a célula do próprio dia e pergunta `premissa_morta`; sem célula/insumos, o fato é `None`, que o catálogo conserva (`app/ponto/services/cluster_espurio.py:66-77`; `app/chamados/catalogo/premissa.py:127-134`). O ato conta somente os fechamentos aceitos pela porta (`app/ponto/services/cluster_espurio.py:187-191`).

2. **`fechados_por_idade` não está implementado com esse nome; existe outra medida.** O command usa `contador_cluster_encerrado_sem_lastro`, imprime seu JSON e retorna rc 2, em `--alarme`, quando há pendências ou divergência do esperado (`app/ponto/management/commands/detectar_cluster_espurio.py:92-104`).

   Esse contador seleciona cluster em `ENCERRADOS`, via `RECONCILIAR`, e consulta `_leitura_da_celula`, sem copiar a regra da assinatura; retorna `numero`, `esperado=0`, universo e exclusões (`app/ponto/services/cluster_espurio.py:88-98`, `:107-150`). **Ele não prova fechamento por idade**: inclui também decisão administrativa via reconciliar, e o próprio serviço declara essa limitação (`app/ponto/services/cluster_espurio.py:228-230`). A trilha de fechamento registra status/via, sem motivo idade versus lastro; não há função causal demonstrada que responda “foi fechado por idade” (`app/chamados/models.py:292-295`; `app/docs/RELATO.md:1439`).

   O delta de publicação permanece identificável: o consumo/alarme já está no command, mas esse trecho não produz carimbo persistente de negócio (`app/ponto/management/commands/detectar_cluster_espurio.py:92-104`). O canal existente do vigia do fio grava `censo` e `esperado`; o leitor compara as chaves do próprio carimbo, e o gerador do ESTADO publica suas divergências (`app/chamados/management/commands/vigiar_fio_morto.py:240-263`; `bin/fio_estado.py:121-126`, `:191-194`; `bin/gerar_estado.py:260-261`). Encaixe específico aprovado do cluster nesse carimbo: **[nao verificado]**.

   Os leitores atuais são:

   - `config/crons.py` agenda `--alarme` às 06:24 e envolve comandos em `cron_run.sh` (`app/config/crons.py:598`, `:1498-1509`).
   - O runner grava o rc em `placar.jsonl`, produz carimbo de conclusão para **0 ou 2** e propaga o rc; esse carimbo contém conclusão, não o contador (`bin/cron_run.sh:11-25`, `:38`). O encadeador espera esses arquivos (`bin/encadeado.sh:81-94`).
   - `pipeline_placar` lê rc 2 como `alarmes` e exibe `ALARME`, porém essa situação não entra na lista que provoca o rc 2 do próprio pipeline (`app/core/management/commands/pipeline_placar.py:61-81`, `:124-132`, `:158-161`). Seus leitores explícitos de contadores do carimbo cobrem `fio_mudo_alvos` e `nunca_bateu_sem_adesao`, sem cluster (`:85-106`, `:135-136`).

3. **O teste existente que morde a regressão é o RED dos 15 dias:** exige ausência em `a_fechar` e chamado ainda vivo com assinatura; o helper executa `pendencias` e `reconciliar_colab` reais (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:56-62`, `:74-77`). A contraprova exige fechamento fora da janela quando o lastro não tem assinatura (`:80-83`).

   O mutante literal `fechar_por_idade` **já existe** e aponta para `test_vivo_de_20_dias_com_assinatura_segue_vivo_apos_o_cron`; o harness exige falha de asserção e nenhum erro de infraestrutura (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:222-225`, `:180-192`, `:286-302`). Resultado executado nesta leitura: **[nao verificado]**.

4. **B3 aberto no registro documental:** o BACKLOG diz `B3 DESENHO` e o topo do RELATO ainda declara O169-FECHADA em B3 (`app/docs/BACKLOG.md:2121`; `app/docs/RELATO.md:4`). Estado efetivo do disjuntor nesta execução: **[nao verificado]**.

   O bloqueio implementado é **próximo verificador final e próxima volta Codex verificador**, até liberação por DESENHO/ERRO-BOBO válido; dispara na primeira reprovação final ou terceira volta Codex procedente (`bin/disjuntor.py:1007-1014`, `:1025-1037`). O desenho registrado trata da responsabilidade pelo legado, via de tranca/trilha e reabertura pela porta — assunto além do contador nominal (`app/docs/RELATO.md:1399`; `app/docs/BACKLOG.md:2121`).

## 15/11 20:05 · sessao ec64280c8cd0 · - · - · origem=Ronald (tmux)

Os registros demonstram pouso do núcleo da E1, mas não encerramento integral dos complementos; a grade ainda registra E1-ENTRADAS como `PARADA (último estado escrito)` e mantém 16 complementos. (`app/docs/RELATO.md:1675`; `app/docs/GRADE-MESTRA.md:106`; `app/docs/GRADE-MESTRA.md:683`)

| Data documental | Hash | Prova |
|---|---|---|
| 08/11 18:1x | `297499b8` | Volta 15g, aval E1-MATRIZ-TODAS-AS-PORTAS, RED 8/20, GREEN 313. `app/docs/RELATO.md:1701`; data em `app/docs/PROMPTS.md:1992`. |
| 08/11, depois do aval 18:1x | `5c875ee3` | 15g ampliada, **squash de `297499b8`**, censo P1–P7. `app/docs/RELATO.md:1702`; horário exato [não verificado]. |
| 08/11, antes de 19:23 | `eabbfc5a` | Merge6 = `e2f865f5 + 5c875ee3`; **não certificou**, suite interrompida 19:23 pelo PARE-P4. `app/docs/RELATO.md:1702`; `app/docs/RELATO.md:1704`. |
| 08/11, depois de 19:23 | `6dee2010` | Volta 15h, recusa de destino de outro colaborador. `app/docs/RELATO.md:1705`; horário exato [não verificado]. |
| 08/11, depois da 15h | `b0e948d8` | Merge7 = `e2f865f5 + 6dee2010`, certificado com 14.536 testes e sombra verde. `app/docs/RELATO.md:1705`; `app/docs/RELATO.md:1706`. |
| 08/11 21:15 | `a8a692f5` | **Pouso**, local sem push naquele momento. `app/docs/RELATO.md:1675`. |
| 08/11 22:46 | `a8a692f5` | **PRONTO FIO-E0-V4-E1 NO AR**, deploy e smoke documentados. `app/docs/RELATO.md:1674`. |
| 08/11 23:12 | `a5df8ed5` | Reescrita das mensagens: árvore igual a `a8a692f5`, 19 hashes alterados. Mapa citado está em logs, não validado nesta cópia. `app/docs/RELATO.md:1673`. |
| 09/11 00:5x / 01:5x | `45cf0e53` / `7aa48401` | Pouso da **E1-FILA** por merge `--no-ff 45cf0e53`, commit `7aa48401`; no ar 01:5x. `app/docs/RELATO.md:1679`; `app/docs/RELATO.md:1680`; `app/docs/BACKLOG.md:1965`. |
| 09/11 03:3x | `e9bf5de2` / `89df8c52` | **E1-FILA-V2-2-TRAVA também pousou**, merge `--no-ff e9bf5de2`, commit `89df8c52`, deploy e smoke documentados. `app/docs/RELATO.md:1633`; `app/docs/BACKLOG.md:1972`. |

O que ficou separado:

- Cinco grupos em competência exportada ficaram fora do apply, retidos pelo aval de 08/11. `app/docs/BACKLOG.md:855`.
- RETRATADA-CONVERSA-E-REABRIR; E1-A2-VAZIO-MEDIR; E1-RECUSAS-SEM-CONTADOR; E1-DUPLICATA-SOBRE-RETRATADA continuam em filas próprias. `app/docs/BACKLOG.md:857`; `app/docs/BACKLOG.md:858`; `app/docs/BACKLOG.md:859`; `app/docs/BACKLOG.md:860`.
- E1-FILA-V2-3-A-6, OPUS6-FILA, V2-2-OUTRAS-PORTAS e V2-2-RESIDUOS continuam registrados como resíduos; **V2-2-TRAVA não deve ser descrita como inteira pendente**, pois pousou. `app/docs/BACKLOG.md:1970`; `app/docs/BACKLOG.md:1971`; `app/docs/BACKLOG.md:1972`; `app/docs/BACKLOG.md:1973`; `app/docs/BACKLOG.md:1974`.
- BUG-PLANTIO-NOTURNO-MAIS-1 e ABSOLVIDA-PELA-FOLGA foram anexados posteriormente como **complementos** da E1-ENTRADAS; o segundo está `ABERTA, medido, nao construir` em 12/11. `app/docs/BACKLOG.md:2086`; `app/docs/BACKLOG.md:2087`.
- O apply limpo está documentado: Ronald aplicou quatro migrações, dry seguinte zero, sem exportadas, com reversão anterior. Isso é prova documental da cópia, sem verificação de produção. `app/docs/RELATO.md:1682`.

Não encontrei os termos exatos solicitados em RELATO-ARQUIVO/TICKETS [não verificado]. A linha ABERTA da E1-ENTRADAS permanece literalmente no BACKLOG, apesar dos pousos posteriores documentados. `app/docs/BACKLOG.md:1964`.

## 15/11 20:05 · sessao 5c62d999d000 · - · - · origem=Ronald (tmux)

- **1(e) PROCEDE — o delta literal está vencido.** O BACKLOG ainda pede `fechados_por_idade`, mas o RELATO explica que idade não fica registrada na fonte e que o contador possível mede encerrados com assinatura viva (`app/docs/BACKLOG.md:2121`; `app/docs/RELATO.md:1439`). Esse contador já existe como `cluster_encerrado_sem_lastro`, explicitamente “medida, não escritor nem novo juiz”, e já alimenta o alarme do vigia (`app/ponto/services/cluster_espurio.py:88`, `app/ponto/services/cluster_espurio.py:91`, `app/ponto/management/commands/detectar_cluster_espurio.py:92`, `app/ponto/management/commands/detectar_cluster_espurio.py:100`). O RELATO registra pouso v5, apply dos três e DRY seguinte vazio; situação atual de produção é [nao verificado] (`app/docs/RELATO.md:1164`).

- **1(e), classificação: CONFERE no sentido funcional de medição.** O contador apenas consulta chamados/células e devolve número, esperado e universo (`app/ponto/services/cluster_espurio.py:107`, `app/ponto/services/cluster_espurio.py:126`, `app/ponto/services/cluster_espurio.py:148`). Isso não comprova que exista delta novo a construir na “vaga de instrumento”: a implementação já está na cópia (`app/ponto/management/commands/detectar_cluster_espurio.py:92`). A regra de contrato para instrumento novo permanece escrita (`CLAUDE.md:394`).

- **3, O95 PROCEDE — falta definir o objeto atual do DIFF.** A candidata do BACKLOG é impedir absorção sem uma volta `E`, consta como revertida e movia outros colaboradores sem explicação (`app/docs/BACKLOG.md:1426`). O HEAD contém outra cura O95: preserva a absorção e fecha o turno quando há fato posterior, explicando que impedir absorção quebrava onze selos (`app/ponto/turnos.py:1106`, `app/ponto/turnos.py:1119`, `app/ponto/turnos.py:1126`). O corpo implementa a condição e o pareador chama esse passo (`app/ponto/turnos.py:1171`, `app/ponto/turnos.py:1186`, `app/ponto/turnos.py:1502`). O arquivo histórico também registra O95 já no código e gravado parcialmente atualizado (`app/docs/RELATO-ARQUIVO.md:4370`). Portanto “DIFF de frota” não determina se compara candidata revertida, cura presente ou deriva do gravado; o motor exige uma `mutacao` definida e distingue GRAVADO/HOJE/NOVO (`app/ponto/services/diff_frota.py:7`, `app/ponto/services/diff_frota.py:45`, `app/ponto/services/diff_frota.py:86`).

- **4, O95: CONFERE para censo/ensaio; PROCEDE se virar apply ou cura implicitamente autorizada.** É domínio de dinheiro: a cura muda fechamento do turno e o motor chama `parear_turnos` (`app/ponto/turnos.py:1186`; `app/ponto/motor_calculo_v2.py:503`). O DIFF real calcula em `somente_leitura=True`, dentro de transação desfeita; isso não é apply persistente (`app/ponto/services/diff_frota.py:72`, `app/ponto/services/diff_frota.py:84`, `app/ponto/services/diff_frota.py:89`). Apply de competência não exportada exige reversão anterior, exportada intacta e prova de frota; mover exportada exige `!` e porta própria (`CLAUDE.md:406`, `CLAUDE.md:411`). O aval fornecido só manda leitura/DIFF e proíbe exportada [nao verificado: anexo sem arquivo:linha nesta cópia].

- **4, O169: CONFERE se restrito ao contador; apply existe e é outra ação.** `--alarme` força `aplicar=False`; o caminho manual `--apply` chama o reconciliador, e `--legado --apply --esperados` chama tratamento que escreve (`app/ponto/management/commands/detectar_cluster_espurio.py:55`, `app/ponto/management/commands/detectar_cluster_espurio.py:69`, `app/ponto/management/commands/detectar_cluster_espurio.py:80`). “Só contador” não abrange esses caminhos.

- **4, VINCULO: CONFERE para censo de leitores; não autoriza aplicar a obra histórica.** A linha mantém dependência do censo antes do tripwire e contém autorização antiga específica, diferenças em exportada e outro caso dependente de `!` (`app/docs/BACKLOG.md:1182`). Apply retroativo continua no domínio sujeito a aval; exportada segue as condições da regra DINHEIRO (`CLAUDE.md:404`, `CLAUDE.md:411`).

## 15/11 20:05 · sessao 845fff82b2b5 · - · - · origem=Ronald (tmux)

A implementação pedida existe na cópia:

| Elemento | Prova no código |
|---|---|
| Catraca das quatro classes | `app/chamados/models.py:2707` define `trava_na_ordem`; trava Colaborador em `:2744`, DisputaSupervisao em `:2753`, ChamadoColaborador em `:2766`, PerguntaDisputa em `:2770`, cada grupo ordenado por pk. |
| Catraca AST, sem allowlist | `app/chamados/tests/test_e1_v7_catracas.py:71` enumera as quatro classes; `:85` fixa o único dono autorizado; `:387` exige zero aquisição de trava fora dele; `:392` contém teste de sanidade que renomeia o dono e verifica que detecta as quatro classes. |
| Decisão append-only | `app/chamados/models.py:3982` define `DecisaoPergunta`; `:3969` bloqueia update/delete/bulk_update; `:4046` bloqueia salvar linha existente; `:4051` bloqueia apagar. FK PROTECT em `:4014`. |
| Escritor único e lâmpada/evento | `app/chamados/models.py:4086` define `registrar`; `:4095` abre atomic; `:4096` insere novas decisões e `:4097` acorda células no mesmo ato. `:3314` implementa a emissão de EventoCelula, pelo escritor único `emitir_evento_celula` em `:3331`. |
| Preservação do legado antes de sobrescrever | `app/chamados/models.py:4057` define `linhas_da_projecao_sem_linha`; `:4074` e `:4078` verificam correspondência de ato/instante antes de gerar resposta/validação faltante. |
| Backfill | `app/chamados/management/commands/backfill_decisoes_pergunta.py:425` oferece `--aplicar`; `:210` atomic; `:217` trava todos os colaboradores antes das demais classes; `:231` trava disputas/perguntas; `:324` registra silêncio e `:331` retrata duplicatas pela porta. `:344` provoca rollback integral quando há recusa. |
| Guardas de identidade no banco | `app/chamados/migrations/0053_pergunta_unica_por_familia.py:36` recusa migração com colisões; `:57` cria índice parcial de MARCO e `:62` de BATIDA. Horizonte literal `2026-09-21` em `:28`. |
| Testes de preservação e imutabilidade | `app/chamados/tests/test_e1_entradas.py:55` resposta nova conserva anterior; `:64` verifica duas linhas e instante; `:78` verifica recusa de edição/exclusão; `:185` catraca de escritor único. |

Limites relevantes:

- O backfill de respostas/validações da projeção é **somente medido pela E1**, com apply reservado a outro ato junto da troca do leitor: `app/chamados/management/commands/backfill_decisoes_pergunta.py:3-13`. Há preservação oportunista nos escritores atuais: `app/chamados/models.py:4057-4082`.
- Exportada fica fora **por grupo inteiro**, sem bloquear os grupos limpos: `app/chamados/management/commands/backfill_decisoes_pergunta.py:169-178`; CLI lista o diff em `:442` e termina com código 3 se restarem grupos exportados em `:620-628`.
- A fila E4 explicita o passo posterior: “E4b BACKFILL C3.2-C3.9 não exportada” em `app/docs/BACKLOG.md:1991`. Isso evita tratar o apply integral da projeção como entrega presumida da E1.
- A catraca AST declara limites de cobertura, incluindo SQL dinâmico, certas escolhas por min/sorted e escrita via helpers: `app/chamados/tests/test_e1_v7_catracas.py:36-45`.
- Existência dos testes foi lida; execução e resultados atuais **[não verificado]**. Aplicação efetiva do backfill em produção **[não verificado]**. [nao verificado]

## 15/11 20:05 · sessao 250f79b241c5 · - · - · origem=Ronald (tmux)

**1(a) PROCEDE — a enumeração literal dos cinco estados não confere integralmente.** O topo repete BO-0611-05 em B3 e itens 3–5 presos no apply exportado (`app/docs/RELATO.md:4`), mas o próprio RELATO registra BO-0611-05 **já NO AR** em `fb9269ea9` (`app/docs/RELATO.md:197`, `app/docs/RELATO.md:1350`, `app/docs/RELATO.md:9037`). E4B também **POUSOU em 12/11**, `087fe758a` (`app/docs/BACKLOG.md:2000`); seu apply remanescente espera COMPETENCIA-PAGA (`app/docs/BACKLOG.md:1942`). BO-DISPUTA está efetivamente estacionada, B3 sem rearme (`app/docs/BACKLOG.md:774`, `app/docs/BACKLOG.md:2005`); F1 nova depende de predesenho e aval novo (`app/docs/BACKLOG.md:1945`); demais fatias F2–F9 não constroem antes do pouso F1 (`app/docs/BACKLOG.md:2018`). Ausência de item imediatamente liberado é compatível com esses registros; “os cinco estão nos estados citados” omite pousos comprovados.

**1(b) CONFERE quanto à frase; PROCEDE se ela fundamentar abertura imediata.** “Construtor na 1a vaga após E3, na frente da LASTRO” consta literalmente (`app/docs/BACKLOG.md:1891`, `app/docs/RELATO.md:813`). Entretanto, está **PARADA até corte do desenho**; predesenho entregue, aguardando corte (`app/docs/BACKLOG.md:1891`, `app/docs/RELATO.md:716`). O aval de 12:28 diz **“4a volta NAO abre”**, retira `orfas_instantes` e mantém a trava atrás de **E3, DESLIGADO, FERIADO, O168 e HJ-090** (`app/docs/RELATO.md:717`). Censo de 12/11: ATA 12 chaves, 95 acessos literais em 27 arquivos; DNA nove chaves, 41 acessos em 19 arquivos; orfas HH:MM sem data, DNA instante ISO sem fuso (`app/docs/RELATO.md:808`). Esse censo inicial não substitui o corte ainda pendente (`app/docs/RELATO.md:716`).

**1(d) PROCEDE — a linha ABERTA de 08/11 ficou vencida; há prova documental e código da entrega.** BACKLOG ainda marca ABERTA (`app/docs/BACKLOG.md:1964`), mas RELATO registra **PRONTO FIO-E0-V4-E1 NO AR 22:46**, deploy `a8a692f5`, três cascas/rotas e sondas (`app/docs/RELATO.md:1674`); pouso local de 08/11 21:15 aparece em `app/docs/RELATO.md:1675`. A merge7 certificada, suite 14.536 e sombra invariantes verdes constam em `app/docs/RELATO.md:1706`. Resíduos seguiram para E1-FILA, também pousada (`app/docs/RELATO.md:1679`) e NO AR em 09/11 (`app/docs/RELATO.md:1662`); E1-FILA-V2-2-TRAVA pousou posteriormente em `89df8c52` (`app/docs/RELATO.md:1633`). Na cópia, a migration identifica expressamente E1, sequência `DecisaoPergunta` e chaves (`app/chamados/migrations/0051_decisao_pergunta.py:1`); a classe existe (`app/chamados/models.py:3982`) e a cura de classe v15g/MATRIZ-TODAS-AS-PORTAS está incorporada (`app/chamados/models.py:2373`). Histórico Git efetivo: **[nao verificado]** — a cópia não contém `.git`. Não foi localizada ocorrência exata de E1-ENTRADAS em RELATO-ARQUIVO: **[nao verificado]**; a prova positiva está no RELATO citado.

## 15/11 20:05 · sessao 92d4dd127d30 · - · - · origem=Ronald (tmux)

- **Q2 — PROCEDE, se a nova lista autorizar abertura imediata.** TIPO continua parada por corte de desenho: LU2-03 está **ABERTA**, entre selo do valor e acessor tipado (`app/docs/RELATO.md:63`); a quarta volta “NAO abre” e há travas atrás de E3, DESLIGADO, FERIADO, O168 e HJ-090 (`app/docs/RELATO.md:717`, `app/docs/BACKLOG.md:1891`). Alterar somente a ordem não resolve desenho/B3: outras obras seguem, a obra para e a liberação depende de aval (`CLAUDE.md:274`, `CLAUDE.md:294`).
- **Q2 — PROCEDE na condição de abertura da F1 nova.** O estado vigente exige **“construcao so por aval novo citando a entrega”** (`app/docs/BACKLOG.md:1945`, `app/docs/RELATO.md:4`). O novo aval condiciona F1 somente à existência do corte e do pré-desenho; se isso valer como autorização automática, falta a exigência vigente. Para R3, a formulação nova conserva o aval posterior exigido; o pré-desenho já está **ENTREGUE**, e c17 continua “so por aval novo” (`app/docs/RELATO.md:22`).
- **Q3 — PROCEDE: cruzamento com F1 futura não certificável.** A F1 nova ainda está em pré-desenho, com padrão a definir (`app/docs/BACKLOG.md:1945`); portanto a afirmação de ausência de arquivo comum com essa construção futura permanece **[nao verificado]**. A regra vigente mede arquivo em comum, não família/app (`CLAUDE.md:309`).
- **Q2 — CONFERE quanto às leituras no Codex.** QUEIMA já foi encerrada por POS-REDEFINICAO-1935; voltaram MODO-PADRAO-POS-RESET, teto 2 e DESPACHO (`app/docs/RELATO.md:25`, `app/docs/PROMPTS.md:2362`). A regra vigente atribui leitura/censo ao Codex (`CLAUDE.md:358`).
- **Q2 — CONFERE quanto ao teto, sem vaga extra para instrumento.** O teto 2 é de construtores, com fila 1, negócio e instrumento nessa precedência (`CLAUDE.md:291`). Instrumento exige contrato de cinco linhas e pouso próprio (`CLAUDE.md:394`, `CLAUDE.md:327`). [nao verificado]
- **FERIADO — PROCEDE se “termina a medida” implicar obra andável.** A medida já registra ENTREGUE_FORA=1 e manda não construir o ramo (a) (`app/docs/RELATO.md:7`); há AVAL-DEVOLVIDO para itens 1, 2 e 4 da revisão (`app/docs/RELATO.md:8`). A prioridade antiga era “na frente, vaga da TIPO item 3” (`app/docs/BACKLOG.md:729`), mas a nova ordem a modifica expressamente; o impedimento permanece nos termos preservados pelo próprio aval (`app/docs/RELATO.md:7`).

## 15/11 20:04 · sessao 5f21eb683352 · - · - · origem=Ronald (tmux)

- `fechados_por_idade` não está implementado: o command importa `contador_cluster_encerrado_sem_lastro`, imprime seu JSON e compara `numero` com `esperado`, emitindo rc 2 em `--alarme` (`app/ponto/management/commands/detectar_cluster_espurio.py:26-29,92-104`). O pedido nominal consta no BACKLOG (`app/docs/BACKLOG.md:2121`).

- Não há função real que distinga “fechado por idade”: o registro de fechamento guarda instante/via e a trilha registra status/via (`app/chamados/models.py:242-245,292-294`). O relato reconhece explicitamente a ausência desse fato na fonte (`app/docs/RELATO.md:1439`). A função real disponível é `contador_cluster_encerrado_sem_lastro`: seleciona encerrados via `RECONCILIAR` e pergunta `_leitura_da_celula` para saber se a assinatura continua viva; não distingue nota, trilha ou motivo (`app/ponto/services/cluster_espurio.py:88-98,107-126`). Retorna nome, número e esperado 0 (`app/ponto/services/cluster_espurio.py:148-150`).

- Localização do delta de instrumento, pelo desenho existente: consumo/impressão/alarme no command (`app/ponto/management/commands/detectar_cluster_espurio.py:92-104`); carimbo de contadores e leitor do ESTADO ainda não aparecem ali. O modelo existente de fotografia usa `censo` + `esperado`, escrito antes do `SystemExit` (`app/chamados/management/commands/vigiar_fio_morto.py:240-270`). O leitor compara todas as chaves de `esperado` do próprio carimbo e publica divergências (`bin/fio_estado.py:121-126,191-194`); o ESTADO chama esse leitor (`bin/gerar_estado.py:260-261`). Encaixe específico aprovado para cluster: [nao verificado].

- `config/crons.py` agenda o cluster às 06:24 com `--alarme`, dependente de `reconciliar_grade` (`app/config/crons.py:598`); `_run` envolve comandos em `cron_run.sh` (`app/config/crons.py:1498-1509`). O shell captura rc e grava nome/empresa/início/fim/duração/exit; carimba conclusão para rc **0 ou 2**; somente outros códigos acionam cron quebrado; propaga o rc (`bin/cron_run.sh:11-13,22-25,32-38`).

- `pipeline_placar` lê `placar.jsonl`: rc 2 conta como alarme, demais rc não zero como erro (`app/core/management/commands/pipeline_placar.py:61-81`). Exibe situação `ALARME`, porém essa situação não entra na lista que provoca o próprio rc 2 do pipeline (`app/core/management/commands/pipeline_placar.py:124-132,158-161`). O leitor de carimbo de negócio desse pipeline consulta especificamente `fio_mudo_alvos` e `nunca_bateu_sem_adesao`, não cluster (`app/core/management/commands/pipeline_placar.py:85-106,135-136`). O timestamp de conclusão é consumido pelo encadeador para aguardar gatilhos (`bin/encadeado.sh:81-94`).

## 15/11 20:04 · sessao 8ac28b4acdb4 · - · - · origem=Ronald (tmux)

- `--dias` recorta a consulta de abertura em `dias_com_cluster` (`app/ponto/services/cluster_espurio.py:40-49`); `pendencias` usa essa consulta para `achados/a_abrir`, mas `a_fechar` chama `_vivos_que_a_premissa_matou(colab)` sem janela (`app/ponto/services/cluster_espurio.py:165-173`).
- O fechamento lê a célula do dia de cada chamado vivo, obtém `None` quando não medido e pergunta `premissa_morta` (`app/ponto/services/cluster_espurio.py:66-77`); o catálogo conserva `None` (`app/chamados/catalogo/premissa.py:127-134`). O ato só fecha os selecionados, contando sucesso do gate (`app/ponto/services/cluster_espurio.py:187-191`).
- Existe `contador_cluster_encerrado_sem_lastro`, esperado zero, que pergunta `_leitura_da_celula` em vez de copiar sua regra (`app/ponto/services/cluster_espurio.py:88-98`, `:126-150`). Seu universo é módulo + `ENCERRADOS` + via `EVENTO_VIA['RECONCILIAR']` (`:107-113`); ele explicitamente **não prova idade**, podendo incluir decisão administrativa via reconciliar (`:228-230`). Portanto, não há equivalência demonstrada entre esse contador e “fechados por idade” (`:228-230`).
- O comando já imprime esse contador e sai `2` quando `--alarme` encontra divergência do esperado (`app/ponto/management/commands/detectar_cluster_espurio.py:92-104`). Função real que responda especificamente “fechado por idade”: [nao verificado].
- O teste existente de 15 dias exige que a vigia não proponha fechamento e que a reconciliação conserve o chamado com assinatura (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:74-77`); o helper chama `pendencias` e `reconciliar_colab` reais (`:56-62`).
- O mutante literal `fechar_por_idade` **já existe**, altera `_fato` para `False` por data e aponta o teste do chamado de 20 dias após `--apply/--alarme` (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:222-225`, `:180-192`). O harness exige falha de asserção e ausência de erro de infraestrutura (`:286-302`). Resultado executado nesta auditoria: [nao verificado].

## 15/11 20:01 · sessao 6a0cf11c44b9 · - · - · origem=Ronald (tmux)

Auditoria estática concluída; nenhum teste foi executado e não houve acesso a banco/log. Evidência de execução e medidas novas de produção: **[nao verificado]**. [nao verificado]

**Escritores e guardas do par**

| Sítio atual | O que escreve/lê e relação com o corte |
|---|---|
| `app/chamados/models.py:2525`, `:2526`, `:3097`, `:3100` | Nascimento: defaults `('', None)` ou campos recebidos; salva inserção. Guarda anterior em `:2922`–`:2923` só consulta recusa de instante sem texto; trilha R/V no mesmo atomic de `:2924`, registro `:3102`. |
| `app/chamados/services/disputa_emissao.py:1446`, `:1476`, `:1481`, `:1482`, `:1522` | Resposta canônica: normaliza texto, exige não vazio, grava texto + instante. Atomic `:1517`; ato `RESPOSTA` `:1525`. Já produz par completo. |
| `app/chamados/services/disputa_emissao.py:1663`, `:1671`, `:1673`, `:1674`, `:1676` | Automática: no-op quando já existe texto; guarda atual só recusa `I∧¬T`; grava os dois campos e `RESPOSTA` (`:1675`, `:1677`). Entrada com texto e instante `None` ainda precisa da recusa simétrica definida no corte. |
| `app/api/foto_recebida.py:25`, `:42`, `:48` | Delegado automático: texto constante não vazio e instante `agora`; validação delegada em `:51`. |
| `app/chamados/services/disputa_emissao.py:1691`, `:1695`, `:1697` | Cura de limbo: lê `¬I∧T`, escreve somente instante fornecido; ato `RESPOSTA` no mesmo atomic (`:1696`, `:1698`). Não confundir com a nova recusa de **criar** limbo: pode continuar reparando legado, conforme finalidade já existente. |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`, `:69` | Dois delegados: carimba pelo instante da nota ou zera pela reabertura. |
| `app/chamados/models.py:3213`, `:3214`, `:3215`, `:3235` | `carimbar(resposta=...)`: só completa instante se texto não vazio e instante ausente; também pode completar validação. Guarda foto anterior em `:3209`–`:3210`; desfaz memória em `:3244`–`:3248`. |
| `app/chamados/services/materializacao.py:563`, `:567`, `:569` | Absorção delega `carimbar(resposta='validar')`; pode completar instante. Atomic inclui ato `ABSORCAO`. |
| `app/chamados/services/materializacao.py:1226`, `:1283`, `:1284` | Materialização granular delega `carimbar(resposta='validar')`; pode completar instante de texto legado. Esse corpo não registra ato próprio de resposta/validação. Garantia geral de decisão para todo chamador: **[nao verificado]**. |
| `app/chamados/services/disputa_emissao.py:2000`, `:2007`, `:2008` | Resolver por chamado: protege par completo sem materialização de fato; restante delega carimbo com `resposta='validar'`. |
| `app/chamados/services/disputa_emissao.py:2078`, `:2081`, `:2084` | Ramo sem credor: lê existência de texto, delega carimbo com `resposta='responder'`; auditoria em `:2084`, sem `DecisaoPergunta` próprio nesse trecho. |
| `app/chamados/models.py:3343`, `:3348`, `:3351`, `:3354`, `:3363`, `:3365` | Executor de lote: aceita par completo ou metade e consulta outra coluna. **Bypass atual:** se só recebe `respondida_em=None`, não lê texto nem valida resultado (`:3349`–`:3352`); permite produzir `¬I∧T`. |
| `app/chamados/services/disputa_emissao.py:1609`, `:1632`, `:1635`, `:1645`, `:1646`, `:1648` | Reabertura comum: já zera instante e texto; guarda texto, instante e validação anteriores no ato `reabertura`/`recusa`, dentro do atomic `:1621`; exclui fora do fato. |
| `app/chamados/models.py:1610`, `:1611`, `:1613` | Ressuscitação atual: preserva projeções R/V sem linha, **zera somente instante**, texto permanece. Não há `REABERTURA` obrigatória nesse bloco; `linhas_da_projecao_sem_linha` só gera atos condicionais em `:4074`, `:4078`. |
| `app/chamados/models.py:1531`, `:1533`, `:1553`, `:1554`, `:1555` | Universo atual da ressuscitação só recebe materializadas com texto SQL `NULL` ou `''`; simulação também só limpa instante. Ambos precisam representar a mesma zeragem integral quando a pergunta for efetivamente ressuscitada. Alterar seletividade do universo além disso não decorre automaticamente do corte. |
| `app/chamados/services/validacao.py:623`, `:628`, `:629` | Recusa delega zeragem integral à reabertura com ato `recusa`. |
| `app/chamados/services/materializacao.py:681`, `:688` | Devolução de absorvidas delega zeragem integral; fornece autor e motivo. |
| `app/chamados/models.py:2299`, `:2301` | Reabertura pelo colaborador delega zeragem integral com autor/motivo. |
| `app/chamados/services/acoes_disputa.py:103`; `app/chamados/views.py:1436`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69` | Outros delegados de reabertura; par e ato ficam na transação da porta `disputa_emissao.py:1621`, `:1632`, `:1648`. |
| `app/chamados/models.py:3529`, `:3534`, `:3559`, `:3560`, `:3563`, `:3566` | `save` valida transição do par e memoriza foto. Hoje permite par ilegal preexistente inalterado e escrita só em outros campos; não recusa `¬I∧T`, pois porta final só consulta instante sem texto (`app/chamados/services/fio.py:1595`, `:1602`). |
| `bin/sombra_regras_pessoais.py:85`; `bin/sombra_mascara_pessoal.py:10`, `:55`, `:57`, `:62`, `:64` | Escritor operacional de texto via tabela e `bulk_update`, restrito ao banco `sombra`; não é escritor de produção. |
| `app/chamados/services/validacao.py:224`, `:225`, `:227`, `:230` | Devolver validação conserva o par; só escreve validação e registra `DEVOLUCAO`. Não é zeragem de resposta. |

Portas adicionais de alcance que **hoje não escrevem o par**:

- `PerguntaDisputa.reabrir_por_competencia` promete voltar “com a resposta que tinha” e só descarimba (`app/chamados/models.py:3416`, `:3418`, `:3421`); merece constar da fronteira do item (2). `descarimbar` salva apenas materialização/via (`app/chamados/models.py:3286`, `:3293`, `:3295`).
- `reabrir_fantasma` também só descarimba (`app/chamados/models.py:3388`, `:3395`, `:3397`); seu chamador escolhe a pergunta, conforme docstring em `:3392`–`:3394`.
- `reabrir_por_resposta` tira carimbo por chegada de **resposta nova**, dentro da resposta canônica; zerar o par nesse ponto apagaria a resposta recém-recebida (`app/chamados/models.py:3400`, `:3412`, `:3414`; `app/chamados/services/disputa_emissao.py:1481`, `:1499`).
- `relavra_via.desfazer_carimbo_sem_lastro` só descarimba e pode reabrir o continente disputa; não escreve par (`app/chamados/services/relavra_via.py:246`, `:259`, `:261`).

**Testes: asserções que de fato mudam** [nao verificado]

| Causa | Asserção atual e mudança derivada |
|---|---|
| Juiz único `I∧T` | `test_e3_10_cura_no_escritor.py:132`, `:133`: `assertIn(vazia/espacos, lista)` passam a exclusão para legado `I∧¬T`; `:134` mantém inclusão do par completo. |
| Juiz único `I∧T` | `test_e3_10_cura_no_escritor.py:140`, `:147`: listas iguais a `[vazia.pk]` passam a `[]`. `:141` (`assertFalse(d.pode_fechar(...))`) **não deve ser invertida automaticamente**: `pode_fechar` também considera fio vivo; nova resposta completa do método requer examinar o restante da fixture e do juiz. |
| Juiz único `I∧T` | `test_o168_pai_com_conversa_no_gate.py:283`, `:285`: instante sem texto atualmente retém `[p.pk]`; passa a `[]`. `:287`: texto sem instante também atualmente retém `[p.pk]`; passa a `[]`. `:289` já exige `[]` após validação. |
| Partição do app/estoque | `test_o261e_f5_alcance.py:194`: espera pendentes `[]` para instante sem texto; pergunta viva em disputa aberta passa a pendente com o novo predicado. Consequências atuais `:196` `(fio_morto=1, pergunta=0, app=0)` e `:197` causa `b` precisam ser reavaliadas com a porta alcançável. `:199` razão `respondida_sem_texto` só desaparece pela **cura do estoque**; trocar leitor sozinho não limpa o dado. |
| Recusa simétrica | `test_contract_par_resposta.py:113` afirma que o detector não acusa trecho só instante, com mensagem “carimbo-sem-texto, que e legitimo”. Esse contrato unidirecional (`:81`) precisa cobrir também instante sem texto; a asserção atual não prova legalidade de valor algum em runtime. |

**Testes que precisam mudar preparação/cobertura, sem inverter automaticamente a asserção** [nao verificado]

- `test_pergunta_estado.py:57`–`:60`: comentário e fixture dizem reabertura mantém texto sem instante; o novo corte exige fixture `('', None)`, porém `estado == 'pendente'` e viva continuam expectativas compatíveis. Não há teste do escritor nessa classe (`:19`–`:20` só instancia o modelo). [nao verificado]
- `test_materializar_carimba_respondida.py:62`–`:63`: hoje persiste texto sem instante; esse `save` passa a ser recusado. Asserções de cura de legado `:76`–`:79` podem continuar se a fixture representar legado por escrita crua controlada; não é autorização para continuar criando meio-estado pela porta.
- `test_porta_resposta_pergunta.py:244`–`:245`: mesmo problema de fixture persistida só texto. Asserções `:247`, `:249`, `:251`, `:254`, `:255` conferem cura histórica/idempotência e não se tornam falsas só pela nova recusa.
- `test_e4b_c6_orm.py:72`–`:77`: fixture `limbo=True` usa `objects.create` com texto sem instante; deve passar a provar recusa da porta ou representar explicitamente legado. Asserções de diagnóstico de limbo `:130`, `:132`, `:142` permanecem possíveis para estoque legado; não há dado fornecido que prove estoque `¬I∧T` zerado pelo item (3). `test_e4b_c11_orm.py:121`–`:122` herda esse caso por `:99`–`:101`.
- `test_e3_10_cura_no_escritor.py:61`–`:69`, `:104`–`:107`, `:166`–`:170`, `:176`–`:184`, `:205`–`:212` já recusam instante sem texto; precisam de contrapartes texto sem instante e lote que só zera instante. Asserções atuais permanecem pertinentes.
- `test_e3_10_cura_no_escritor.py:192`–`:197` permite legado ilegal inalterado em outros campos e sua saída para pendente. Se o corte pretende rejeitar também `save()` do **mesmo** legado inalterado, essa asserção mudaria; essa extensão além de impedir gravar pares novos é **[nao verificado]**.
- `test_o261e_f2_porta_carimbo.py:720` usa `update` para montar legado texto sem instante; asserções `:725`–`:731` provam recusa/rollback do carimbo e podem continuar pertinentes.
- `test_o261_resposta_vazia.py:64` monta meio-estado legado por `update`; `:85`, `:95` já proíbem fabricar instante sem texto; `:109`, `:113` conferem recusa do carimbo pelo C6 e não são automaticamente invertidas pelo novo juiz.
- `test_o261e_f2_ramo_e_i20.py:265` exige preservação do legado `('07:55', None, None)`: o item (3) nomeia `I∧¬T`, portanto não implica limpar esse outro estoque.
- `test_reabrir_ressuscita.py:44`–`:49` exige que **pergunta não selecionada para ressuscitação** continue com resposta e materialização. Isso testa o universo atual SQL só texto vazio (`models.py:1533`), não uma ressuscitação que mantém texto. A seleção não deve ser ampliada sem distinguir essa questão.
- `test_tranca_de_verdade.py:61` e `test_competencia_trancada.py:155` preservam texto ao **trancar/encerrar**, não ao reabrir; não trocam de asserção pelo item (2). `test_tranca_de_verdade.py:74`–`:89` cobre reabrir competência, mas não confere zeragem do par nem foto anterior.

**Testes já conformes que devem permanecer como provas** [nao verificado]

- Reabrir zera par e guarda anterior: `test_reabrir_nao_destroi.py:49`, `:50`, `:52`, `:58`; pendente não fabrica ato em `:64`; repetição não duplica em `:72`.
- Porta única de reabertura e zeragem conjunta: `test_e3_porta_A_reabrir.py:38`, `:41`, `:42`; ato de carimbo sozinho `:51`; recusa comum zera par e registra `recusa` `:82`–`:83`.
- Contrato textual de zeragem já exige ambos: `test_reabertura_fantasma.py:63`–`:65`.
- `test_o261_ressuscita_respeita_o_marco.py:118`–`:121` já exige instante/validação/materialização limpos nos alvos; `:126`–`:128` preserva alvos excluídos pelo marco aceso. Falta nessa prova conferir texto e `REABERTURA` com foto anterior.
- Contratos estáticos texto+instante em `test_contract_texto_com_carimbo.py:119`, `:124`, `:136`, `:138`, `:166`–`:172` continuam úteis, porém não provam recusa runtime, atomicidade nem igualdade entre leitores.

Não localizei teste dedicado que aplique exatamente a cura das **143 vivas**, preserve todas validadas/materializadas e confira separação aberta/entregue; existência em arquivo não aberto: **[nao verificado]**. [nao verificado]

## 15/11 20:01 · sessao 130dfb79b02a · - · - · origem=Ronald (tmux)

Todos os sítios abaixo foram abertos na cópia; nenhuma execução de Django, banco ou log. Correspondência Git/HEAD: [nao verificado].

Use `J=I∧T`, com `T=bool((texto or '').strip())`, sem substituir vitalidade, validação, materialização, tranca, janela ou identidade; o código atual já possui esse significado na face SQL `q_respondida_com_texto`, mas ainda não a função `pergunta_respondida`. (`app/chamados/juizes.py:1523`; `app/chamados/juizes.py:1554`; `app/chamados/models.py:3436`.)

A tabela é por R do censo, com finalidade e destino. “J SQL” significa face SQL do juiz; “J” significa Python/property/contexto do template.

| Classe/R | Uso atual e finalidade | Destino e estado divergente |
|---|---|---|
| A/R13 | C6 usa `I∧T`, depois decide concordância/admin/vias/lâmpada. (`app/chamados/juizes.py:1403`; `app/chamados/juizes.py:1420`.) | J sobre projeção temporal, sem alteração por par. |
| A/R16 | Face SQL `I∧T`, com regex de todos os brancos Python. (`app/chamados/juizes.py:1539`; `app/chamados/juizes.py:1559`.) | Aproveitável como J SQL; sem alteração por par. |
| A/R19 | Fila global exige `q_vivas`, `q_respondida_com_texto`, C6 Python. (`app/chamados/juizes.py:1725`; `app/chamados/juizes.py:1728`.) | J SQL; sem alteração por par. |
| A/R29 | Auditor I21 usa I + `.exclude(resposta_colab='')`, sem trim, e exclui validada/materializada. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1572`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1575`.) | J SQL; diverge `I∧brancos`, classificados hoje como texto. |
| A/R52 | Degrau resposta de `q_vivas` delega à face E. (`app/chamados/models.py:3453`; `app/chamados/models.py:3454`; `app/chamados/models.py:3468`.) | J SQL; sem alteração por par. `q_vivas` segue continente do C6, não igualdade. (`app/chamados/models.py:3436`; `app/chamados/models.py:3439`.) |
| A/R79 | Resolvedor preserva pergunta com `I∧T` e sem materialização de fato. (`app/chamados/services/disputa_emissao.py:2000`; `app/chamados/services/disputa_emissao.py:2001`.) | J + juiz de materialização; sem alteração por par. |
| A/R96 | Janela visível herda degrau resposta do `q_vivas`. (`app/chamados/services/janela_colab.py:63`.) | Herda J SQL; sem alteração por par. |
| A/R110 | App pré-filtra `q_vivas`, confirma C6. (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/services/pergunta_no_app.py:61`.) | Herda J SQL/J no C6; sem alteração por par. |
| A/R112 | Crédito da mesa: tuplas com I e `not texto_vazio`, complemento explícito. (`app/chamados/services/raiox.py:138`; `app/chamados/services/raiox.py:143`; `app/chamados/services/raiox.py:144`.) | J/adaptador de tupla, sem reconstituir predicado; sem alteração por par. |
| A/R125 | Vigia viva em disputa fechada herda `q_vivas`. (`app/chamados/services/vigia_fio.py:769`.) | Herda J SQL; sem alteração por par. |
| A/R141 | Fecho do fio herda `q_vivas`. (`app/ponto/services/veredito_celula.py:181`.) | Herda J SQL; sem alteração por par. |

O corpo B é `sem_veredito_entre`: `¬validada ∧ ¬materializada ∧ (I∨T) ∧ (ignorar_tranca∨¬trancada)`. Trocar somente a cláusula `I∨T` por J muda ambas as metades (`I∧¬T`, `¬I∧T`), dentro dos demais filtros; os delegados abaixo herdam esse delta. (`app/chamados/models.py:1905`; `app/chamados/models.py:1909`.)

| R/B | Sítio e finalidade |
|---|---|
| 24 | Irmãs sem consumo; retenção/crédito. (`app/chamados/juizes.py:2079`.) |
| 27 | Auditor I12; retenção de irmãs. (`app/chamados/management/commands/auditar_invariantes_chamados.py:984`.) |
| 38 | Lote validar classe A; fila. (`app/chamados/management/commands/validar_classe_a.py:54`.) |
| 42 | Corpo do juiz sem veredito; retenção. (`app/chamados/models.py:1907`; `app/chamados/models.py:1908`.) |
| 43 | Lista da disputa; retenção. (`app/chamados/models.py:1889`.) |
| 44 | Pode fechar; ausência da lista também combinada com ausência de fio vivo. (`app/chamados/models.py:1917`.) |
| 45 | Fechar; lista não vazia recusa fechamento. (`app/chamados/models.py:1950`; `app/chamados/models.py:1952`.) |
| 60 | Retenção da retratação. (`app/chamados/reconciliador.py:448`.) |
| 62 | Encerrar órfãs pela lâmpada. (`app/chamados/reconciliador.py:1164`.) |
| 69 | Medir competência trancada; ignora tranca. (`app/chamados/services/competencia_trancada.py:126`.) |
| 80 | Resolvedor; irmãs protegem pai. (`app/chamados/services/disputa_emissao.py:2030`.) |
| 85 | Gaveta da fila por causa. (`app/chamados/services/fila_por_causa.py:226`.) |
| 104 | Materializador; irmãs impedem fecho. (`app/chamados/services/materializacao.py:768`.) |
| 133 | Desligamento; retenção. (`app/colaboradores/services/desligamento.py:165`.) |
| 134 | Ficha; interseção da lista sem veredito com vivas. (`app/colaboradores/services/ficha.py:168`; `app/colaboradores/services/ficha.py:169`.) |
| 142 | CSF pelos universos pai/destino, ignorando tranca. (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574`.) |

Todos os C abaixo trocam I por J — J SQL quando em `.filter`/contagem, J nos demais — e mudam `I∧¬T`; testes adicionais de domínio continuam podendo impedir diferença no resultado externo. (`app/chamados/models.py:3641`; `app/chamados/juizes.py:1559`.)

| R/C | Predicado e finalidade |
|---|---|
| 4 | I seleciona `respondidas` no histórico app. (`app/api/views.py:2577`.) |
| 5 | I informa `ja_respondida` à resposta/correção. (`app/api/views.py:2620`; `app/api/views.py:2627`.) |
| 18 | I + lastro/vitalidade para corrigíveis. (`app/chamados/juizes.py:1718`; `app/chamados/juizes.py:1719`.) |
| 25 | Falta de I integra contraprova I21a. (`app/chamados/management/commands/auditar_invariantes_chamados.py:177`.) |
| 26 | I em disputa aberta no auditor I2. (`app/chamados/management/commands/auditar_invariantes_chamados.py:592`.) |
| 30 | ¬I no auditor C2. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1603`.) |
| 49 | I distingue pendente/respondida após precedências de fase/C6. (`app/chamados/models.py:3124`; `app/chamados/models.py:3128`; `app/chamados/models.py:3131`.) |
| 56 | Property `respondida=I`. (`app/chamados/models.py:3641`.) |
| 58 | I gera linha RESPOSTA ao recompor trilha; texto apenas copiado. **Não descartar o fato legado vazio:** esta é preservação histórica, diferente da classificação corrente. (`app/chamados/models.py:4067`; `app/chamados/models.py:4074`; `app/chamados/models.py:4077`.) |
| 64 | Conta ¬I para reabrir disputa. (`app/chamados/services/acoes_disputa.py:95`.) |
| 65 | Alguma viva ¬I impede regularização. (`app/chamados/services/acoes_disputa.py:130`.) |
| 67 | ¬I e destino não resolvido em becos. (`app/chamados/services/becos.py:30`.) |
| 71 | `bool(I)` informa reenvio ao escritor de declaração. (`app/chamados/services/declaracao_texto.py:419`.) |
| 72 | ¬I dispara resposta na aplicação da declaração. (`app/chamados/services/declaracao_texto.py:687`.) |
| 76 | Conta ¬I para recobrar questionário. (`app/chamados/services/disputa_emissao.py:1734`.) |
| 77 | I exclui porta de reabrir credor; exige C6 viva. (`app/chamados/services/disputa_emissao.py:1842`.) |
| 89 | Viva ¬I impede regularização no POST do fio. (`app/chamados/services/fio.py:698`.) |
| 94 | I seleciona evento temporal de resposta, com `desde`; distinguir evento legado de estado corrente antes de migrar. (`app/chamados/services/historico.py:99`; `app/chamados/services/historico.py:105`; `app/chamados/services/historico.py:106`.) |
| 107 | I + validada + não materializada seleciona lote de materialização. (`app/chamados/services/materializacao.py:1375`; `app/chamados/services/materializacao.py:1379`.) |
| 111 | ¬I + disputa aberta => pendente app. (`app/chamados/services/pergunta_no_app.py:81`.) |
| 116 | ¬I => `sem_resposta` na entrada validar. (`app/chamados/services/validacao.py:418`.) |
| 117 | ¬I => `ja_pendente` na entrada recusar. (`app/chamados/services/validacao.py:605`.) |
| 121 | I nos últimos sete dias e posterior à cobrança => contador de aceite por pessoa. (`app/chamados/services/verbos.py:335`; `app/chamados/services/verbos.py:342`; `app/chamados/services/verbos.py:345`.) |
| 123 | I sob âncora terminal => causa c do alcance. (`app/chamados/services/vigia_fio.py:694`.) |
| 128 | I informa `ja_respondida` na view responder. (`app/chamados/views.py:1280`.) |
| 129 | ¬I + C6 para pendentes vivas na reabertura. (`app/chamados/views.py:1416`; `app/chamados/views.py:1417`.) |
| 130 | Vivas I para validar; ¬I para aguardando colaborador. (`app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:176`.) |
| 137 | I antes de fio vivo no reconciliador de grade. (`app/ponto/management/commands/reconciliar_grade.py:461`.) |
| 143 | Contadores folha ¬I/I, acrescidos de disputa/validação. (`bin/censo_folha.py:28`; `bin/censo_folha.py:29`.) |
| 148 | I governa rótulo e caixa da pergunta. (`app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29`.) |
| 150 | ¬I na pergunta cega; I governa caixa de resposta. (`app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217`.) |

D após strip diverge `¬I∧T` se a pergunta feita é “respondida?”. Para guardas de conteúdo (“não plantar sem texto”, parse/display, preservar texto histórico), conservar leitura de texto e pôr J na entrada onde o corte exige resposta; não trocar a necessidade de texto por uma afirmação sobre fase. (`app/chamados/services/materializacao.py:835`; `app/chamados/templatetags/chamados_tags.py:142`.)

| R/D | Uso e observação |
|---|---|
| 8 | SQL `texto=''` encontra pergunta muda com resposta no fio; finalidade diagnóstico. Ao usar `¬J`, passa a alcançar texto sem instante e brancos; exige dados do fio para resultado final. (`app/chamados/canal.py:53`; `app/chamados/canal.py:65`; `app/chamados/canal.py:72`.) |
| 31 | `any(T)` interrompe auditor F1. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1655`.) |
| 34 | `.only(texto)` alimenta eleição canônica, pelo mesmo corpo R90. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:504`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:506`.) |
| 37 | `T` elege com resposta na deduplicação. (`app/chamados/management/commands/dedup_perguntas_fato.py:75`.) |
| 40 | Universo de ressuscitação restringe texto NULL/`''`, sem trim, além de materializada e fora do fato. `¬J` ampliaria também perguntas com texto; isso é mudança de universo, não consequência automática de “reabrir zera par”. (`app/chamados/models.py:1530`; `app/chamados/models.py:1535`.) |
| 59 | T impede lavratura de órfã. (`app/chamados/reconciliador.py:365`.) |
| 61 | T impede veredito de órfã. (`app/chamados/reconciliador.py:1083`.) |
| 74 | T impede resposta automática. (`app/chamados/services/disputa_emissao.py:1663`.) |
| 81 | T só rotula auditoria “com resposta”; carimbo usa porta própria. (`app/chamados/services/disputa_emissao.py:2078`; `app/chamados/services/disputa_emissao.py:2081`; `app/chamados/services/disputa_emissao.py:2086`.) |
| 90 | T prefere canônica com resposta. (`app/chamados/services/fio.py:1271`.) |
| 91 | T + via admin protege mérito perdedor. (`app/chamados/services/fio.py:1282`; `app/chamados/services/fio.py:1283`.) |
| 99 | T + tipo hora governa estado de aplicação. (`app/chamados/services/materializacao.py:438`.) |
| 105 | ¬T recusa plantio e gera recusa; guarda de texto deve continuar além do gate J. (`app/chamados/services/materializacao.py:835`; `app/chamados/services/materializacao.py:842`.) |
| 126 | ¬T retorna leitura vazia; apresentação de conteúdo. (`app/chamados/templatetags/chamados_tags.py:142`.) |
| 132 | SQL `texto!=''` e C6 governa indicador de calendário; J SQL exclui `¬I∧T` e brancos com qualquer I. (`app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/calendario.py:476`.) |
| 135 | ¬T e fora do conjunto sem veredito => sem resposta na ficha; migrar em conjunto com B, pois o `falou` muda. (`app/colaboradores/services/ficha.py:169`; `app/colaboradores/services/ficha.py:171`.) |
| 136 | `any(T)` => evidência de fala na prévia; J se a descrição passar a ser “respondida”, distinguir preservação de evidência textual. (`app/folha/previa.py:139`; `app/folha/previa.py:142`.) |
| 138 | `any(T)` isenta resposta do cartório/dedup. (`app/ponto/services/cartorio.py:1637`; `app/ponto/services/cartorio.py:1641`; `app/ponto/services/cartorio.py:1648`.) |
| 145 | Verdade crua de texto governa ramos de exibição; se for rótulo “respondida”, J; se mero conteúdo, permanece apresentação. Diverge também brancos. (`app/templates/chamados/partials/_card_atendimento.html:141`; `app/templates/chamados/partials/_card_atendimento.html:182`.) |

F é leitura de conteúdo/data e não decisão de “respondida”; não converter payload/texto em booleano. Existem dependências dos gates e da garantia do par:

- R1/R3/R6 payload app, datas e texto. (`app/api/views.py:1163`; `app/api/views.py:1165`; `app/api/views.py:2561`; `app/api/views.py:2563`; `app/api/views.py:2637`.)
- R7 item da fila assume I e chama `.isoformat()`/subtração sem guarda; a fila nova E preserva essa premissa. (`app/api/views_mensageria.py:567`; `app/api/views_mensageria.py:569`; `app/api/views_mensageria.py:570`.)
- R39 amostra de resposta. (`app/chamados/management/commands/validar_classe_a.py:80`.)
- R57 resolve rótulo e devolve texto original. (`app/chamados/models.py:3664`; `app/chamados/models.py:3669`; `app/chamados/models.py:3677`; `app/chamados/models.py:3684`; `app/chamados/models.py:3686`.)
- R83/R84 texto na explicação. (`app/chamados/services/explicador.py:32`; `app/chamados/services/explicador.py:79`; `app/chamados/services/explicador.py:80`.)
- R95 trecho de texto no evento de linha do tempo. (`app/chamados/services/historico.py:109`.)
- R120 idade da fila. (`app/chamados/services/validacao.py:959`.)
- R131 amostra da proposta. (`app/chamados/views_cobrar.py:291`.)
- R139 resposta no dia do colaborador. (`app/ponto/services/dia_do_colab.py:70`.)
- R144 conteúdo/data/input. (`app/templates/chamados/partials/_card_atendimento.html:106`; `app/templates/chamados/partials/_card_atendimento.html:143`; `app/templates/chamados/partials/_card_atendimento.html:148`; `app/templates/chamados/partials/_card_atendimento.html:183`; `app/templates/chamados/partials/_card_atendimento.html:391`.)
- R147 preview de lote. (`app/templates/chamados/partials/_lote_preview.html:24`.)
- R149 resposta formatada/data depois do gate R148. (`app/templates/chamados/partials/_pergunta_card.html:31`; `app/templates/chamados/partials/_pergunta_card.html:32`.)
- R151 conteúdo/data depois do gate R150. (`app/templates/chamados/partials/modal_fio.html:219`; `app/templates/chamados/partials/modal_fio.html:220`.)

Complementos do censo:

- `_item` da API lê `p.estado`, serializa fase e calcula `aguarda_admin=RESPONDIDA`: migrar por autoridade de `estado`; delta `I∧¬T` somente onde as precedências não produzem VALIDADA/RESOLVIDA. (`app/api/views.py:2550`; `app/api/views.py:2552`; `app/api/views.py:2553`; `app/chamados/models.py:3124`; `app/chamados/models.py:3128`.)
- `explicar_pergunta` escolhe dono/ação pelo estado PENDENTE/RESPONDIDA, além da exibição R84: herda a mesma mudança. (`app/chamados/services/explicador.py:72`; `app/chamados/services/explicador.py:73`; `app/chamados/services/explicador.py:77`.)
- `tarefa_do_dia._pela_decisao` lê última DP: RESPOSTA com `ato_texto` vazio => responder; com texto => validar. É gate por ato/status, não o mesmo par da PerguntaDisputa; não confundir com autorização de trocar a autoridade temporal inteira. (`app/chamados/services/tarefa_do_dia.py:147`; `app/chamados/services/tarefa_do_dia.py:151`.)
- A carga da tarefa lê atos ordenados/cortados por horizonte e delega pendentes à mesma partição app. (`app/chamados/services/tarefa_do_dia.py:410`; `app/chamados/services/tarefa_do_dia.py:420`; `app/chamados/services/tarefa_do_dia.py:513`; `app/chamados/services/tarefa_do_dia.py:514`.)
- Comparações VALIDADA/RESOLVIDA não decidem “respondida”: fio registra ato após validação; vigia identifica aceite de folga validado; template rotula resolvida. Não migrar essas comparações de fase para J. (`app/chamados/services/fio.py:652`; `app/chamados/services/vigia_fio.py:311`; `app/templates/chamados/partials/_card_atendimento.html:189`.)

Sobre produção: não há no dado anexado uma contagem observada por estado/sítio. “143 vivas” é previsão literal do aval, não prova de que só 143 leituras mudam. Existem leitores C sobre consolidadas, por exemplo o lote de materialização com `validada_em!=NULL`, e `respondida` não filtra fase; logo o script deve enumerar também `I∧¬T` validada/materializada sem alterá-las. (`app/chamados/services/materializacao.py:1375`; `app/chamados/services/materializacao.py:1379`; `app/chamados/models.py:3641`; números em produção: [nao verificado].)

## 15/11 20:00 · sessao 022007b1addb · - · - · origem=Ronald (tmux)

Para a sonda, a competência deve vir de `competencia_do_dia_entregue`: ela compõe `janela_atual` com o corte da empresa e `competencia_entregue`; devolve `None` sem dia. “Entregue” significa existir `ExportacaoDominio` vigente e não revogada, sem depender de tranca ou de disputa aberta. (`app/chamados/services/vigia_fio.py:843`; `app/chamados/services/vigia_fio.py:859`; `app/folha/export.py:556`; `app/folha/export.py:588`.)

Vitalidade deve vir do C6 real. `q_vivas` garante somente continência; além disso, uma pergunta materializada pode continuar viva, portanto “viva” e “validada/materializada” precisam aparecer em eixos separados. (`app/chamados/models.py:3436`; `app/chamados/juizes.py:1420`; `app/chamados/juizes.py:1432`.)

O C6 engole algumas exceções e devolve um resultado conservador. A sonda precisa conferir o contador de engolidos e classificar aquela leitura como erro, evitando contar a falha como pergunta morta. (`app/chamados/juizes.py:1476`; `app/chamados/juizes.py:1489`; `app/core/observ.py:60`; `app/core/observ.py:73`.)

Script proposto, sem execução nesta cópia: **[nao verificado]**. Pressupõe um shell Django já configurado no tenant correto, iniciado com `PYTHONDONTWRITEBYTECODE=1`; não lê arquivos de log. **[nao verificado]**

```python
import io
import json
import logging
from collections import Counter
from contextlib import redirect_stdout, redirect_stderr

from django.db import connection, transaction
from django.utils import timezone

from chamados.models import PerguntaDisputa
from chamados.juizes import (
    pergunta_viva,
    q_respondida_com_texto,
    texto_vazio,
    um_colaborador_por_identidade,
)
from chamados.services.vigia_fio import competencia_do_dia_entregue
from core.observ import engolidos

# Fontes:
# campos: chamados/models.py:2525-2527
# vazio: chamados/juizes.py:1523-1525
# face SQL atual: chamados/juizes.py:1559
# vida exata: chamados/juizes.py:865
# competência: chamados/services/vigia_fio.py:843-865
# precedência V/M no estado: chamados/models.py:3124-3131
#
# As famílias abaixo medem diferenças do componente "respondida".
# Os demais filtros e agregações de cada sítio precisam ser considerados
# na interpretação; estes números não equivalem ao resultado final
# de todas as filas, portas ou contadores.
#
# O alvo de estoque segue o corte fornecido, não executa sua correção.

universo = 0
erros = []
estados = Counter()
deltas = Counter()
linhas = []
estoque = {"aberta": [], "entregue": [], "indecidivel": []}
memo_competencias = {}
fatal = None
nivel_anterior = logging.root.manager.disable

def emitir(obj):
    print(json.dumps(obj, ensure_ascii=False, sort_keys=True))

def medir(func, *args, **kwargs):
    antes = engolidos()["total"]
    try:
        # Impede impressão incidental de nomes/textos pelos leitores.
        with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
            valor = func(*args, **kwargs)
    except Exception as exc:
        return None, type(exc).__name__
    if engolidos()["total"] != antes:
        return None, "erro_engolido"
    return valor, None

try:
    if connection.vendor != "postgresql":
        raise RuntimeError("sonda_exige_postgresql")
    if connection.in_atomic_block:
        raise RuntimeError("iniciar_fora_de_atomic")
    if getattr(connection, "schema_name", None) in (None, "public"):
        raise RuntimeError("tenant_nao_selecionado")

    # Evita handlers de logging gravarem mensagens incidentais.
    # O contador de engolidos continua funcionando.
    logging.disable(logging.CRITICAL)

    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )

        agora = timezone.now()
        universo = PerguntaDisputa.objects.count()

        # Universo inteiro: não usar q_vivas como se fosse C6 exato.
        qs = (
            PerguntaDisputa.objects
            .select_related(
                "disputa__colaborador__empresa",
                "celula",
                "chamado_destino",
                "chamado_destino__celula",
            )
            .order_by("pk")
        )
        perguntas = list(um_colaborador_por_identidade(qs))

        ids_sql = set(
            PerguntaDisputa.objects.filter(q_respondida_com_texto())
            .values_list("pk", flat=True)
        )

        for p in perguntas:
            I = p.respondida_em is not None
            T = not texto_vazio(p.resposta_colab)
            novo = I and T
            bruto = bool(p.resposta_colab)
            diferente_de_vazio = (
                p.resposta_colab is not None and p.resposta_colab != ""
            )
            formato = (
                "NULL" if p.resposta_colab is None
                else "vazio_exato" if p.resposta_colab == ""
                else "brancos" if not T
                else "texto"
            )
            par = (
                "I_T" if I and T
                else "I_sem_T" if I
                else "T_sem_I" if T
                else "sem_I_sem_T"
            )
            protecao = (
                ("V" if p.validada_em is not None else "-")
                + ("M" if p.materializada_em is not None else "-")
            )
            disputa = (
                "aberta" if p.disputa.fechada_em is None else "fechada"
            )

            viva, erro_viva = medir(pergunta_viva, p, agora=agora)
            entregue, erro_comp = medir(
                competencia_do_dia_entregue,
                p.data_evento,
                p.disputa.colaborador.empresa,
                memo=memo_competencias,
            )
            comp = (
                "indecidivel" if erro_comp or entregue is None
                else "entregue" if entregue
                else "aberta"
            )
            vida = (
                "erro" if erro_viva
                else "viva" if viva
                else "morta"
            )

            for etapa, erro in (
                ("vitalidade", erro_viva),
                ("competencia", erro_comp),
            ):
                if erro:
                    erros.append({
                        "pergunta": p.pk,
                        "pessoa": f"col{p.disputa.colaborador_id:03d}",
                        "etapa": etapa,
                        "erro": erro,
                    })

            if (p.pk in ids_sql) != novo:
                erros.append({
                    "pergunta": p.pk,
                    "etapa": "equivalencia_face_sql",
                    "erro": "python_sql_divergentes",
                })

            chave = (comp, vida, protecao, disputa, par, formato)
            estados[chave] += 1

            antigos = {
                "A_instante_E_texto": I and T,
                "B_instante_OU_texto": I or T,
                "C_so_instante": I,
                "D_so_texto_strip": T,
                "D_texto_verdade_crua": bruto,
                "D_texto_diferente_vazio_sem_trim": diferente_de_vazio,
                "A_instante_E_texto_sem_trim": I and diferente_de_vazio,
            }
            familias = [
                nome for nome, antigo in antigos.items()
                if bool(antigo) != novo
            ]
            for familia in familias:
                deltas[(familia,) + chave] += 1

            alvo = (
                not erro_viva and bool(viva)
                and protecao == "--"
                and I and not T
            )
            if alvo:
                estoque[comp].append(p.pk)

            # Não imprime resposta, nome, CPF nem conteúdo de exceção.
            if familias or alvo:
                linhas.append({
                    "pergunta": p.pk,
                    "pessoa": f"col{p.disputa.colaborador_id:03d}",
                    "competencia": comp,
                    "vida": vida,
                    "protecao_VM": protecao,
                    "disputa": disputa,
                    "par": par,
                    "formato_texto": formato,
                    "familias_divergentes": familias,
                    "alvo_estoque": alvo,
                })

except Exception as exc:
    fatal = type(exc).__name__
    erros.append({"etapa": "fatal", "erro": fatal})
finally:
    logging.disable(nivel_anterior)

# Primeira linha da SAÍDA: universo e total de erros.
emitir({"universo": universo, "total_erros": len(erros), "fatal": fatal})

for chave, n in sorted(estados.items()):
    comp, vida, protecao, disputa, par, formato = chave
    emitir({
        "tipo": "estado", "competencia": comp, "vida": vida,
        "protecao_VM": protecao, "disputa": disputa,
        "par": par, "formato_texto": formato, "n": n,
    })

for chave, n in sorted(deltas.items()):
    familia, comp, vida, protecao, disputa, par, formato = chave
    emitir({
        "tipo": "divergencia_do_componente",
        "familia": familia, "competencia": comp, "vida": vida,
        "protecao_VM": protecao, "disputa": disputa,
        "par": par, "formato_texto": formato, "n": n,
    })

for linha in linhas:
    emitir({"tipo": "pergunta_divergente", **linha})
for erro in erros:
    emitir({"tipo": "erro", **erro})

emitir({
    "tipo": "confronto_com_corte_fornecido",
    "esperado": {
        "total": 143,
        "aberta_pks": [38251, 38252],
        "entregue_n": 141,
    },
    "medido": {
        "total": sum(map(len, estoque.values())),
        "aberta_pks": estoque["aberta"],
        "entregue_n": len(estoque["entregue"]),
        "entregue_pks": estoque["entregue"],
        "indecidivel_pks": estoque["indecidivel"],
    },
    "medicao_completa": fatal is None and not erros,
})
```

`ΔB` alcança as duas metades ilegais; `ΔC` alcança instante sem texto; `ΔD` com `strip` alcança texto sem instante; igualdade com `''` ou verdade crua também pode divergir em brancos. Esses são estados candidatos de mudança do componente; filtros de validação, materialização, vitalidade, prazo, agrupamento e existência ainda podem impedir mudança do resultado final. (`app/chamados/models.py:1905`; `app/chamados/models.py:3124`; `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/juizes.py:1523`.)

Quantidade efetivamente encontrada, igualdade aos 143 esperados e distribuição das 141 entregues: **[nao verificado]**.

## 15/11 20:00 · sessao e34ba78245e1 · FERIADO-USA-A-LEI-DE-1609 · sonda · origem=Code

Transcrição do dado fornecido [nao verificado]:

```text
UNIVERSO HOJE=2026-11-15 lote=15 pks=[328, 1111, 1208, 1247, 1264, 1287, 1303, 1349, 1350, 1355, 1358, 1398, 1409, 1425, 1430]
EC 1208 col940 no_lote=True gravado=True lei=False ciclo=5x2 ini=2026-09-11 fim=None
EC 1255 col945 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-09-11 fim=None
EC 1418 col985 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-10-14 fim=None
EC 1421 col987 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-10-13 fim=None
EC 1424 col990 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-10-14 fim=None
EC 1517 col1018 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-11-04 fim=None
EC 1522 col1023 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-11-03 fim=None
EC 1523 col1024 no_lote=False gravado=False lei=None ciclo=6x1 ini=2026-11-04 fim=None
total_erros=0
```

Só o **EC 1208 (col940)** está no lote (`no_lote=True`), segundo a saída fornecida. [nao verificado]

## 15/11 19:59 · sessao 1a301cb1c9f7 · FERIADO-USA-A-LEI-DE-1609 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 1: PROCEDE; item 2: PROCEDE; item 3: NÃO PROCEDE como escrito; item 4: PROCEDE**, pelos fundamentos abaixo (`app/escala/models.py:1096`; `app/ponto/services/fechamento.py:1607`; `CLAUDE.md:269`).

As referências abaixo são da cópia fornecida. A identidade Git dessa cópia com o HEAD `988df8bfb7b341382d5b84dea73b9b9c18a84032` é **[nao verificado]**: não há metadados Git disponíveis.

**1. CORTE — PROCEDE como candidata condicionada à medida.**

As premissas centrais conferem, com uma precisão: a flag não é lida incondicionalmente para toda data; o caminho aritmético só consulta `feriado_vira_folga` quando `base is True`, e o predicado só suprime trabalho nos ciclos declarados (`app/escala/models.py:1369`; `app/escala/models.py:1390`; `app/escala/models.py:1392`).

- Havendo célula, `eh_dia_trabalho` devolve `cel.trabalha`; sem célula, chama `eh_dia_trabalho_calculado`. Essa cadeia não testa a vigência antes da consulta à flag (`app/escala/models.py:1092`; `app/escala/models.py:1095`; `app/escala/models.py:1096`; `app/escala/models.py:1369`; `app/escala/models.py:1390`).
- Os endereços completos dos três leitores externos são **`app/ponto/services/fechamento.py:949`**, **`app/ponto/precedencia.py:377`** e **`app/ponto/calculador/alimentacao.py:521`**. Os nomes sem pasta do aval são abreviações; não identificam arquivos na raiz.
- Existe também o leitor interno, usado pelo gerador: `app/escala/models.py:1369`. Outros leitores diretos da **flag** aparecem no contador, na sincronização e nas cópias de trechos (`app/escala/models.py:929`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:48`; `app/colaboradores/services/vinculo.py:974`; `app/colaboradores/services/vinculo.py:1092`; `app/colaboradores/services/vinculo.py:1113`).
- O gerador atual restringe a geração à cobertura do vínculo antes de chamar a aritmética. Portanto, preservar seu comportamento **dentro** da vigência é compatível com o desenho; a preservação pela implementação futura é **[nao verificado]** (`app/ponto/portas/celula.py:906`; `app/ponto/portas/celula.py:911`).

Não identifiquei colisão de **“SEM célula e FORA da vigência não lê a flag”** com os contratos citados:

- L-020: “CELULA SOBERANA: dia + marcos sao DADO com DNA congelado”; o corte proposto se limita justamente à ausência dessa testemunha (`app/docs/LEIS.md:71`).
- Seção 4: a grade é fonte única; os métodos leem a célula, com fallback aritmético/template (`CLAUDE.md:148`; `CLAUDE.md:155`).
- Sem célula, o contrato de `vinculo_do_dia` declara “a aritmetica do vinculo que cobre a data”; E1 escreve “a geradora da celula so vale DENTRO da vigencia” e “Vigencia, nao o flag ativo” (`app/escala/alimentacao.py:244`; `app/escala/alimentacao.py:272`).
- Os cortes registram uma autoridade para vínculo do dia e outra para cobertura, ambas já existentes; não é necessário inventar outro juiz (`app/docs/CORTES.md:335`; `app/docs/CORTES.md:338`).
- A lei de 16/09 decide como um vínculo **NOVO** nasce: posto declarado vence; sem declaração, comerciais 5x2/6x1 folgam; vínculo existente não passa novamente pelo padrão. Ela não determina o uso desse flag antes do nascimento do vínculo (`app/escala/models.py:900`; `app/escala/models.py:906`; `app/escala/models.py:908`; `app/escala/models.py:916`).

Isso não decide que todo dia anterior ao vínculo deva deixar de contar no banco: essa é a pergunta distinta do item 4 (`app/ponto/motor_calculo_v2.py:3449`; `app/ponto/motor_calculo_v2.py:3451`).

**2. MEDIR ANTES — PROCEDE como medida de leitura; condição ainda não demonstrada.**

A medida deve alcançar os dias efetivamente consultados pelo fechamento, incluindo a semana completa usada pelo banco: o motor recua até segunda-feira e consulta os sete dias, mesmo quando parte deles está fora do período apurado (`app/ponto/motor_calculo_v2.py:3436`; `app/ponto/motor_calculo_v2.py:3441`; `app/ponto/motor_calculo_v2.py:3449`).

A população da frota inteira, sua partição ENTREGUE/aberta e o resultado “ENTREGUE fora do lote = 0” **não constam do dado fornecido [nao verificado]**. O resumo de 15 vínculos/31 colab-competências não demonstra essa condição de frota **[nao verificado]**.

Também não cabe substituir “todos os vínculos” por `ativa=True`: a cobertura inclui vínculo encerrado com fim declarado, e a escolha sem célula passa pelo juiz de cobertura (`app/escala/alimentacao.py:249`; `app/escala/alimentacao.py:285`).

**3. CONDIÇÃO LITERAL — NÃO PROCEDE como escrito.**

Há quatro pontos que impedem certificar o literal:

1. **“31/31 iguais” depende dos dois lados da comparação.** Comparar **antes do lote** com **lote + cura** pode fechar com col940 estabilizado. Comparar a sombra anterior, já com col940 alterado, com a sombra curada exige que esse próprio caso mude para recuperar o valor anterior. A referência escolhida pelo aval é **[nao verificado]**. O mecanismo que permite essa diferença está na consulta semanal ao fallback (`app/ponto/motor_calculo_v2.py:3449`; `app/escala/models.py:1096`; `app/escala/models.py:1369`).

2. **Não há prova de identidade do lote desde 13/11.** O histórico escrito menciona 16 vínculos, enquanto o impacto registrado menciona 15/31. Esses textos demonstram divergência entre registros; não demonstram quem saiu, por quê, nem que o universo mudou em produção (`app/docs/BACKLOG.md:729`; `app/docs/RELATO.md:4`). Identidade congelada do lote: **[nao verificado]**.

3. **“Qualquer campo gravado” inclui carimbos.** A porta salva a flag; o `post_save` chama `invalidar_previsto`; essa função zera `previsto_em` e muda `atualizado_em` sem filtro de competência/exportação (`app/colaboradores/services/vinculo.py:894`; `app/escala/signals.py:137`; `app/escala/signals.py:146`; `app/ponto/services/fechamento.py:1611`). Se algum fechamento ENTREGUE dos alvos tiver `previsto_em` preenchido, o literal falha mesmo com dinheiro estável. A existência desses registros é **[nao verificado]**. O achado já está registrado fora do escopo (`app/docs/BACKLOG.md:730`).

4. **A sombra padrão não prova todos os campos.** O retorno recortado omite `motivos_espelho`, `causa_espelho`, `previsto_em` e `atualizado_em`; além disso, `previsto_em` é produzido com relógio. Igualdade numérica e preservação do registro precisam ser verificadas separadamente; o retorno padrão sozinho não certifica o literal (`app/ponto/services/fechamento.py:454`; `app/ponto/services/fechamento.py:1086`; `app/ponto/services/fechamento.py:1128`).

O **caso oposto já está coberto no código de teste**: vínculo iniciado em 01/01, feriado em 21/04, flag False resulta em folga; True mantém trabalho (`app/escala/tests/test_escala_models.py:255`; `app/escala/tests/test_escala_models.py:268`; `app/escala/tests/test_escala_models.py:275`). Há ainda vínculo com vigência delimitada e feriado interno suprimido pela flag False (`app/ponto/tests/test_precedencia_vinculo_encerrado.py:43`; `app/ponto/tests/test_precedencia_vinculo_encerrado.py:68`). Execução desses testes nesta revisão: **[nao verificado]**.

Quanto à Q4, **não é possível afirmar que nenhuma outra competência exportada muda [nao verificado]**. Alterar `models.py` pode mudar dinheiro sem editar o motor: ele consulta o método do vínculo, usa a contagem como denominador e calcula o saldo; o fechamento persiste esse saldo (`app/ponto/motor_calculo_v2.py:3449`; `app/ponto/motor_calculo_v2.py:3451`; `app/ponto/motor_calculo_v2.py:3455`; `app/ponto/services/fechamento.py:1180`). O mesmo predicado também seleciona rubricas de feriado (`app/ponto/services/fechamento.py:949`; `app/ponto/services/fechamento.py:955`; `app/ponto/calculador/alimentacao.py:521`).

A cura isolada pode ser uma alteração de leitura; o lote, porém, escreve vínculo e regenera células, além da invalidação descrita acima (`app/colaboradores/services/vinculo.py:894`; `app/colaboradores/services/vinculo.py:904`; `app/colaboradores/services/vinculo.py:910`). “Motor intocado” não certifica “dinheiro intocado”; dado de vínculo/escala e gravado exportado continuam sujeitos ao escopo literal do `!` (`CLAUDE.md:265`; `CLAUDE.md:267`; `CLAUDE.md:411`).

**4. REGISTRAR `lei:` SEM DECIDIR — PROCEDE.**

Registrar a pergunta sobre os dias anteriores ao início do vínculo, sem implementar resposta, corresponde à L-117 e à PARADA 3 (`app/docs/LEIS.md:138`; `CLAUDE.md:269`).

E1 resolve **qual vínculo vale na data**; o banco atual pergunta ao mesmo vínculo pelos sete dias da semana. Logo, a candidata (a) não resolve por si toda a pergunta (b), e esta revisão não autoriza alteração do motor nem do gravado exportado (`app/escala/alimentacao.py:272`; `app/escala/alimentacao.py:285`; `app/ponto/motor_calculo_v2.py:3433`; `app/ponto/motor_calculo_v2.py:3449`; `CLAUDE.md:142`; `CLAUDE.md:411`).

**Quem escreve e quem lê `trabalha_em_feriado`.**

| Papel | Arquivo:linha |
|---|---|
| Declara o campo/default | `app/escala/models.py:896` |
| Decide o padrão de nascimento | `app/escala/models.py:905` |
| Cria vínculo com o padrão | `app/colaboradores/services/vinculo.py:296` |
| Altera flag existente | `app/colaboradores/services/vinculo.py:893`; `app/colaboradores/services/vinculo.py:894` |
| Lê e copia para trechos | `app/colaboradores/services/vinculo.py:974`; `app/colaboradores/services/vinculo.py:1092`; `app/colaboradores/services/vinculo.py:1113` |
| Lê para decidir feriado | `app/escala/models.py:1390` |
| Lê para contador | `app/escala/models.py:929` |
| Lê na sincronização; escreve pela porta | `app/colaboradores/management/commands/sincronizar_feriado_admin.py:48`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:56` |

Os quatro leitores executáveis encontrados de `feriado_vira_folga` são `app/escala/models.py:1369`, `app/ponto/services/fechamento.py:949`, `app/ponto/precedencia.py:377` e `app/ponto/calculador/alimentacao.py:521`. Leitores indiretos relevantes incluem o motor e os caminhos de previsto (`app/ponto/motor_calculo_v2.py:2448`; `app/ponto/motor_calculo_v2.py:3449`; `app/ponto/services/fechamento.py:731`; `app/ponto/calculador/alimentacao.py:445`; `app/ponto/precedencia.py:538`).

**Quem escreve `CelulaDia`.**

| Operação | Arquivo:linha |
|---|---|
| Geração/regeneração | `app/ponto/portas/celula.py:265`; `app/ponto/portas/celula.py:279`; `app/ponto/portas/celula.py:293`; `app/ponto/portas/celula.py:310`; `app/ponto/portas/celula.py:914`; `app/ponto/portas/celula.py:926` |
| Edição humana | `app/ponto/portas/celula.py:638`; `app/ponto/portas/celula.py:647` |
| Reversão | `app/ponto/portas/celula.py:836` |
| Veredito/ata/status | `app/ponto/portas/celula.py:1098` |
| Persistência comum e carimbos | `app/ponto/portas/celula.py:957`; `app/ponto/portas/celula.py:1201`; `app/ponto/portas/celula.py:1216`; `app/ponto/portas/celula.py:1237` |
| Efeito referencial ao remover vínculo | O próprio código reconhece escrita por `SET_NULL` e recusa referência humana antes da remoção: `app/colaboradores/services/vinculo.py:1023`; `app/colaboradores/services/vinculo.py:1029` |

**Quem lê `CelulaDia`.** Os leitores dos métodos do vínculo incluem previsto, marcos, pausas e extra declarada (`app/escala/models.py:1092`; `app/escala/models.py:1122`; `app/escala/models.py:1187`; `app/escala/models.py:1232`). A lista abaixo registra entradas de consulta ORM por arquivo, incluindo aliases; consumidores transitivos de mapas recebidos não constituem censo exaustivo **[nao verificado]**.

```text
app/chamados/juizes.py:411
app/chamados/models.py:1287
app/chamados/reconciliador.py:693
app/chamados/rejuncao.py:88
app/chamados/veredito_lavrado.py:191
app/chamados/services/adesao.py:33
app/chamados/services/celula_carimbada.py:96
app/chamados/services/disputa_emissao.py:951
app/chamados/services/fabrica_da_celula.py:299
app/chamados/services/fila_por_causa.py:202
app/chamados/services/fio.py:1073
app/chamados/services/foto_e4b.py:269
app/chamados/services/furo_sem_canal.py:78
app/chamados/services/materializacao.py:171
app/chamados/services/resposta_tardia.py:44
app/chamados/services/silencio_ausencia.py:41
app/chamados/services/vigia_fio.py:378
app/chamados/management/commands/auditar_invariantes_chamados.py:266
app/chamados/management/commands/backfill_fio_morto_e4b.py:315
app/chamados/management/commands/carimbar_por_batida_coincidente.py:82
app/chamados/management/commands/diff_e3_sombra.py:114
app/chamados/management/commands/disparar_perguntas_competencia.py:178
app/chamados/management/commands/materializar_passivo_validado.py:223
app/chamados/management/commands/nomear_marco_por_lampada.py:59

app/colaboradores/views.py:2874
app/colaboradores/services/calendario.py:411
app/colaboradores/services/feriado_do_posto.py:94
app/colaboradores/services/pos_vinculo.py:62
app/colaboradores/services/vinculo.py:1025
app/colaboradores/management/commands/corrigir_escala_retroativa.py:98
app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:46
app/core/views.py:396

app/escala/alimentacao.py:75
app/escala/folgas.py:621
app/escala/regua_defesa.py:687
app/escala/servico_jornada.py:47
app/escala/utils.py:49
app/escala/services/cadastro_realidade.py:166
app/escala/services/escala_auto_executor.py:57
app/escala/services/fase_12x36.py:188
app/escala/services/furos_diarios.py:313
app/escala/services/leitor_celula.py:362
app/escala/services/propositor.py:472
app/escala/services/regeneracao.py:101
app/escala/services/semana_do_posto.py:85
app/escala/management/commands/celulas_contra_o_template.py:29

app/folha/export.py:949
app/folha/porta_export.py:532
app/folha/previa.py:50
app/folha/validacao_pdf.py:44
app/folha/services/sem_furo.py:20
app/folha/services/vigia_pausa.py:73
app/pautas/services.py:423

app/ponto/marcos.py:155
app/ponto/nucleo.py:92
app/ponto/portas/celula.py:226
app/ponto/signals.py:127
app/ponto/turnos.py:1586
app/ponto/worklist.py:21
app/ponto/services/cadastro_futuro.py:31
app/ponto/services/cartorio.py:390
app/ponto/services/cluster_espurio.py:46
app/ponto/services/dia_do_colab.py:16
app/ponto/services/diagnostico_escala.py:135
app/ponto/services/esmeril_espelho.py:60
app/ponto/services/faltas_de_hoje.py:42
app/ponto/services/fechamento.py:605
app/ponto/services/flip_auto.py:211
app/ponto/services/furos_sem_cobranca.py:61
app/ponto/services/furos_vetados.py:61
app/ponto/services/lastro.py:193
app/ponto/services/passivo_s84.py:94
app/ponto/services/pautas_dos_discordantes.py:72
app/ponto/services/veredito_celula.py:30
app/ponto/services/vigia_ausencia_veredito.py:42
app/ponto/services/vigia_cadastro_futuro.py:31
app/ponto/services/vigia_de_hora.py:21
app/ponto/services/vigia_entrada.py:78
app/ponto/services/vigia_pareador_celula.py:69
app/ponto/management/commands/aplicar_09_corte_b.py:137
app/ponto/management/commands/aplicar_col900.py:163
app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:47
app/ponto/management/commands/backfill_b5_insumos.py:60
app/ponto/management/commands/celula_veredito_velho.py:86
app/ponto/management/commands/censo_col900.py:104
app/ponto/management/commands/censo_fase_12x36.py:393
app/ponto/management/commands/curar_residuo_bug115.py:81
app/ponto/management/commands/detectar_ausencias.py:469
app/ponto/management/commands/detectar_intervalo_ausente.py:46
app/ponto/management/commands/diff_calculador.py:274
app/ponto/management/commands/diff_espelho_alimentacao.py:99
app/ponto/management/commands/dna_x_batida_real.py:58
app/ponto/management/commands/emitir_furo_retroativo.py:69
app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86
app/ponto/management/commands/gerar_celulas.py:316
app/ponto/management/commands/lavrar_status_competencia.py:42
app/ponto/management/commands/plano_b_no_dinheiro.py:142
app/ponto/management/commands/processar_cartorio.py:49
app/ponto/management/commands/recusar_ponta_pequena.py:124
app/ponto/management/commands/supra_juiz.py:165
app/ponto/management/commands/tripwire_celulas.py:21
app/ponto/management/commands/vigia_b5_ausencia.py:54

app/relatorios/cartao_pela_celula.py:286
app/relatorios/dia_da_frota.py:34
```

Anexo existente: [JUIZES.md](/home/ronald/codex_ws/988df8bfb7b341382d5b84dea73b9b9c18a84032/app/docs/JUIZES.md:101). Ali constam feriado, célula/precedência e vínculo/cobertura; o próprio documento distingue sua tabela de consulta do registro vivo (`app/docs/JUIZES.md:101`; `app/docs/JUIZES.md:112`; `app/docs/JUIZES.md:130`; `app/docs/JUIZES.md:10`).

_5760 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
