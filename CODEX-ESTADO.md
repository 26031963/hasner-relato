# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 23:35** · publicado em **07/11/2026 23:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 23:35 · sessao 5f4f8b5530e6 · - · - · origem=Ronald (tmux)

Confirmei a cadeia literal **CH antes de D**:

| Passo | Evidência |
|---|---|
| Criação da batida | `app/ponto/registro_batida.py:152` ou `:157` |
| Receiver síncrono do cartório | `app/ponto/signals.py:132` → `:135` → `_julgar_dias`, que chama cartório em `:93` |
| Entrada no julgamento | `app/ponto/services/cartorio.py:1989` → corpo em `:948` |
| Espelhamento do chamado vivo | `app/ponto/services/cartorio.py:1789` ou `:1798` |
| Porta de contexto | `app/ponto/management/commands/detectar_ausencias.py:558` → `app/chamados/services/contexto.py:101` atomic, `:103` trava, `:104` save |
| Aquisição C + CH | `app/chamados/models.py:486`: `trava_na_ordem(self.colaborador_id, chamados=(self.pk,))`; C em `:2547`, CH em `:2567`; essa chamada não pede universo de D |
| Fábrica posterior ao corpo | `app/ponto/services/cartorio.py:997` → `app/chamados/services/fabrica_da_celula.py:186` → `:149` |
| Aquisição posterior de D abertas | `app/chamados/services/disputa_emissao.py:530` pede `abertas_do_colaborador=True`; D em `app/chamados/models.py:2554` |

Sem atomic externo herdado, a transação de `contexto.py:101` termina antes de o fluxo chegar à fábrica em `cartorio.py:997`; com atomic externo, os locks C/CH continuam na transação que depois pede D. A identificação desse percurso com o nome **“2a instância”** é [nao verificado].

Correção do sítio `app/chamados/veredito_lavrado.py:231`: pelo command `lavrar_perguntas_stale.py:87`, é **SEM-TRAVA**, pois o próprio `PerguntaDisputa.save` abre atomic em `app/chamados/models.py:3173`. Pela chamada `app/chamados/models.py:312`, é **TRAVA-ANTES**, com trava em `:290`. Assim, o sítio tem caminhos mistos; não recebe um único rótulo universal sem registrar essa distinção.

## 07/11 23:34 · sessao 5f4f8b5530e6 · - · - · origem=Ronald (tmux)

Área lida: fora de `chamados/models.py`, `chamados/signals.py`, `chamados/services/` e comandos de chamados, o censo por AST aponta dois escritores diretos atribuídos às três entidades; a leitura confirmou ambos em `app/chamados/veredito_lavrado.py:207` e `:231`. A exaustividade além dos sítios atribuídos pelo varredor é [nao verificado].

| Sítio | Função | Entidade/operação | Classificação por caminho lido |
|---|---|---|---|
| `app/chamados/veredito_lavrado.py:207` | `gravar_lavras_em_lote` | P `bulk_update` | **SEM-ATOMIC** no caminho do backfill: chamadas `app/chamados/management/commands/backfill_veredito_lavrado.py:143` e `:146`; a função não abre atomic nem trava (`app/chamados/veredito_lavrado.py:197`). |
| `app/chamados/veredito_lavrado.py:231` | `lavrar` | P `save` | **INDETERMINADO como rótulo único: caminhos mistos.** Pela chamada `app/chamados/models.py:312`, **TRAVA-ANTES** em `app/chamados/models.py:290`, dentro do atomic `:280`. Pela chamada `app/chamados/management/commands/lavrar_perguntas_stale.py:87`, **SEM-ATOMIC** (command lido; não há atomic/lock no caminho). |

As chamadas `models.py:2767`, `models.py:2905`, `services/fio.py:869`, `services/fio.py:964` e `services/relavra_via.py:126` passam `salvar=False`; não executam o save de `veredito_lavrado.py:231` nesses caminhos.

Laços multicolaborador confirmados fora da sua área:

| Atomic externo / laço | Escrita alcançada |
|---|---|
| `app/chamados/views_arquivar.py:134` / `:135` | `arquivo.silenciar` em `:140`, sobre queryset sem restrição obrigatória a um colaborador (`:115`, filtro opcional `:118`). |
| `app/escala/views.py:407` / `:408` (`aplicar_folga_calendario`) | `trocar_calendario_de_folga` em `:409`; relavra em `:416` → `app/escala/folgas.py:272` → regeneração → marcador de CH em `app/ponto/portas/celula.py:289` e julgamento em `:434` (pode alcançar portas D/CH/P). |
| `app/ponto/portas/celula.py:652` / `:673` (`trocar_plantao`) | Dois colaboradores obrigatoriamente distintos (`:662`), ordenados por colaborador/data (`:674`), chama `editar_dia` em `:675` → marcador de CH em `:626`; julgamento indireto pelo `_o_chao_do_dia_mudou` em `:627`, que chama cartório em `:434`. |
| `app/ponto/management/commands/marcar_abandono_provisorio.py:22` / `:43`, interno `:70` | `garantir_ausencia` em `:76` → `app/ponto/services/ausencia.py:417-418` salva Ausência; receiver síncrono `app/ponto/signals.py:147` → `_julgar_dias`/cartório `:93` (portas D/CH/P indiretas). Callback `ponto/signals.py:272` roda depois do commit e não integra essa transação. |
| `app/ponto/management/commands/gerar_celulas.py:309` / `app/ponto/portas/celula.py:837`, interno `:839` | O corpo escreve **CelulaDia**, com `create :849`, saves `:861`, `:871`. Não escreve D/CH/P diretamente nesse corpo (`:811-873`). Produzir D/CH/P por algum gancho não lido é [nao verificado]. |

`app/ponto/portas/regen_exportada.py:171` não é multicolaborador: o plano foi construído para um único `colaborador` em `:140-143`.

`app/escala/services/cadastro_tipo.py:334` é laço de frota, mas **não encontrei atomic envolvendo esse laço no caminho local**: `salvar_tipo` começa em `:140`, chama propagação em `:320`; cada regeneração abre seu atomic próprio em `app/ponto/portas/celula.py:218`. O decorador `cadastro_tipo.py:41` pertence ao helper `_hpd_pares :43`, não a `salvar_tipo`. Chamador externo que imponha atomic à frota é [nao verificado].

Batida, passos literais lidos:

1. `app/ponto/registro_batida.py:152` e `:157`: `Batida.objects.create`.
2. Receiver de cartório: `app/ponto/signals.py:132`, chama `_julgar_dias :135`; este chama `cartorio.julgar_celula :93`.
3. `app/ponto/services/cartorio.py:1989` chama `julgar_colab`; corpo inicial em `:948`.
4. Chamados podem passar por `reconciliar()` em `cartorio.py:1488`, `:1501`, `:1600`, ou pela passada adiada `:963`.
5. `app/chamados/models.py:650-654`: atomic de `reconciliar` → decisão → `_gravar_estado`; `_recarregar_estado_se_mudou` trava em `:711`, `_gravar_estado` trava em `:290`.
6. Esses dois locks pedem **Disputas abertas antes de CH**. A implementação seleciona D abertas em `models.py:2552-2555`, depois CH em `:2567`.
7. A fábrica ocorre depois do corpo em `cartorio.py:997` → `app/chamados/services/fabrica_da_celula.py:186` → `:149` criar/atualizar disputa; porta adquire universo em `app/chamados/services/disputa_emissao.py:530`.
8. Segundo receiver da batida: `app/ponto/signals.py:207-215` → `app/chamados/reconciliador.py:1418`, `:1419`, `:1427`. Retração: `reconciliador.py:522-523` → mesmo `reconciliar` acima.
9. Onde **CH já pode estar seguro antes de adquirir D fechada**: `models.py:1432` `_gravar_estado` → `:1441` `ressuscitar_perguntas` → trava explícita das D selecionadas em `:1552` → `:1582-1583` `reabrir_sistema`. C já foi adquirido em `:290`; a primeira aquisição do CH está em `:2567`, e o primeiro universo exclui D fechadas por `:2552`.
10. Outro lock que pede somente C+CH: `app/chamados/services/fio.py:768` (`_costura_nao_mata_o_fio`); sua chegada pela batida e posterior D na mesma transação não foram fechadas nesta leitura [nao verificado].

Não identifiquei com segurança qual desses percursos corresponde ao rótulo externo **“2a instância da batida”** [nao verificado].

## 07/11 23:34 · sessao 2783e9e9098d · - · - · origem=Ronald (tmux)

Nenhum novo laço multicolaborador escritor de D/CH/P confirmado nos candidatos desta rodada.

| Candidato | Resultado com evidência |
|---|---|
| `aplicar_09_corte_b.py:197–213` | Há atomic externo e loop de empresas. O serviço chamado percorre colaboradores (`fechamento.py:619`) e grava FechamentoMensal (`898,1137,1153`) e DiaPago (`1193`); não confirmei caminho D/CH/P. Demais efeitos transitivos dos cálculos: [nao verificado]. |
| `reconciliar_grade.py:472` | Atomic por um `ch`, para renovar/retirar contexto (`473–474`). O loop de colaboradores está fora (`115`), assim como a chamada (`228`). Não é transação multicolaborador. |
| `curar_residuo_bug115.py:120` | Atomic de `_ambiguo` por um `ch` (`121–127`). Loops externos (`95–106`) não estão dentro de atomic. |
| `gerar_batidas_smoke_motor.py:106–107` | Chama `semear`, que faz Batida.bulk_create e trilha (`registro_batida.py:331–338`), sem signals de save. Guarda de ensaio em `333→326–328`. Não confirmei escrita D/CH/P. |
| `lavrar_placar_situacional.py:101–111` | Loop escreve somente PlacarSituacional (`105–106`) e apaga placares (`110–111`); busca nos arquivos `*signals.py` não encontrou receiver desse modelo. |
| `escala/folgas.py:162,309,354,382,490` | Cada ato recebe um único vínculo/colaborador. O loop de `384` percorre dias do mesmo `ec`; a relavra também recebe esse `ec` (`395–398`). `reparentar_folgas_do_colaborador` filtra um colaborador (`512–515`). |
| `escala/folgas.py:555,585` | Atos escrevem calendário e seus tamanhos (`560–572`, `587–593`); nenhum receiver desses modelos encontrado nos arquivos `*signals.py`. Não confirmei escrita D/CH/P. |

A relavra de folgas pode chegar às portas de chamado, mas mantém um único vínculo no ato: `folgas.py:272–276` chama `regenerar_celulas_vinculo(ec, ...)`. Um chamador externo que envolva vários vínculos em atomic precisa ser contado no próprio chamador; esses chamadores adicionais não foram auditados nesta rodada. [nao verificado]

## 07/11 23:32 · sessao 2e5bb51989dd · - · - · origem=Ronald (tmux)

Censo de `app/chamados/services`: **36 sítios diretos** de escrita nas três entidades, conforme os endereços da tabela. P = `PerguntaDisputa`, D = `DisputaSupervisao`, CH = `ChamadoColaborador`.

| Arquivo:linha | Função | Entidade/operação | Classificação e fundamento |
|---|---|---|---|
| `services/vinculos.py:89` | `apontar_justificativa` | CH/save | TRAVA-ANTES `:88`, atomic `:86` |
| `services/vinculos.py:101` | `apontar_celula` | CH/save | TRAVA-ANTES `:100`, atomic `:98` |
| `services/vinculos.py:117` | `apontar_ticket_glpi` | CH/save | TRAVA-ANTES `:116`, atomic `:114` |
| `services/vinculos.py:135` | `assumir_responsavel` | CH/save | TRAVA-ANTES `:134`, atomic `:131` |
| `services/vinculos.py:147` | `liberar_responsavel` | CH/save | TRAVA-ANTES `:146`, atomic `:143` |
| `services/contexto.py:78` | `anotar_contexto` | CH/save | TRAVA-ANTES `:77`, atomic `:75` |
| `services/contexto.py:104` | `renovar_contexto` | CH/save | TRAVA-ANTES `:103`, atomic `:101` |
| `services/contexto.py:127` | `retirar_contexto` | CH/save | TRAVA-ANTES `:126`, atomic `:124` |
| `services/contexto.py:163` | `fundir_contexto` | CH/save | TRAVA-ANTES `:162`, atomic `:160` |
| `services/texto.py:36` | `reescrever_texto` | CH/save | TRAVA-ANTES `:35`, atomic `:31` |
| `services/arquivo.py:171` | `silenciar` | CH/save | TRAVA-ANTES `:170` **se** atomic externo; helper não trava fora de atomic (`models.py:484`). Nos chamadores sem atomic, SEM-ATOMIC, sujeito à leitura do override CH feita pelo agente models. |
| `services/arquivo.py:210` | `levantar` | CH/save | TRAVA-ANTES `:209` **se** atomic externo; mesma guarda `models.py:484` |
| `services/disputa_emissao.py:296` | `perguntar_entradas_pendentes` | D/save | TRAVA-ANTES `:968` pelo chamador `:1179`; pelo chamador `:685`, atomic de `_abrir_ou_agregar` já terminou (`:521`, retorno `:604`), portanto classificação depende do atomic externo de `criar_ou_atualizar_disputa` |
| `services/disputa_emissao.py:565` | `_abrir_ou_agregar_disputa` | D/save | TRAVA-ANTES `:530`, atomic `:521` |
| `services/disputa_emissao.py:587` | `_abrir_ou_agregar_disputa` | D/create | TRAVA-ANTES `:530`, atomic `:521` |
| `services/disputa_emissao.py:993` | `criar_disputa_a_partir_de_chamados_orfaos` | D/save | TRAVA-ANTES `:968`, atomic `:964` |
| `services/disputa_emissao.py:1032` | mesma | D/create | TRAVA-ANTES `:968` |
| `services/disputa_emissao.py:1041` | mesma | D/save | TRAVA-ANTES `:968` |
| `services/disputa_emissao.py:1070` | mesma | D/save | TRAVA-ANTES `:968` |
| `services/disputa_emissao.py:1165` | mesma | D/save | TRAVA-ANTES `:968` |
| `services/disputa_emissao.py:1259` | `criar_disputa_ancora_de_chamado` | D/create | TRAVA-ANTES `:1232`, atomic `:1223` |
| `services/disputa_emissao.py:1268` | mesma | D/save | TRAVA-ANTES `:1232` |
| `services/relavra_via.py:127` | `relavrar` | P/save | SEM-TRAVA; command `relavrar_via_i6.py:101-102` abre atomic sem C; fallback de P.save também tem atomic `models.py:3173` |
| `services/validacao.py:137` | `_carimbar_validacao` | P/save | TRAVA-ANTES pelos chamadores `validacao.py:285`, `:317`, `:431`; chamadas `:288`, `:320`, `:435` |
| `services/validacao.py:202` | `devolver_validacao_recusada` | P/save | TRAVA-ANTES `validacao.py:431` pelo chamador único da app `:464`, ainda no atomic `:425` |
| `services/validacao.py:585` | `recusar_pergunta` | P/save | SEM-TRAVA no caminho `acoes_disputa.py:80`; P.save abre atomic `models.py:3173` |
| `services/validacao.py:661` | `conferir_data_do_fato` | P/save | SEM-TRAVA no caminho `views.py:1312`; P.save abre atomic `models.py:3173` |
| `services/materializacao.py:848` | `_materializar_resposta_hora` | P/save | TRAVA-ANTES quando via `validacao.py:431/463`; SEM-TRAVA via `acoes_disputa.py:139`, `fio.py:670`, ou `validacao.py:469` (fora do atomic `:425`). Atomic local `materializacao.py:1217`, chamada `:1219`. |
| `services/materializacao.py:1100` | `_carimbar_recusa` | P/save | TRAVA-ANTES via `validacao.py:431/463`; SEM-TRAVA no gate anterior ao atomic (`materializacao.py:1210`) e no finally depois dele (`:1294`) quando os chamadores HTTP não têm atomic; P.save garante atomic `models.py:3173`. Também alcançado sob atomic do passivo (`passivo_validado.py:371`, `:511`, `:545`) sem trava C prévia garantida. |
| `services/disputa_emissao.py:1389` | `responder_pergunta_disputa_core` | P/save | SEM-TRAVA via `views.py:1283`, `api/views.py:2659`, `acoes_disputa.py:368`; atomic local `:1388`. TRAVA-ANTES via `declaracao_texto.py:322/330/389` ou `:624/627/661`, que mantêm vivo o lock de `_abrir_ou_agregar_disputa:530`. |
| `services/disputa_emissao.py:1434` | mesma | P/save | Mesmos caminhos acima: SEM-TRAVA nos HTTP diretos, TRAVA-ANTES nos de declaração; atomic próprio P.save `models.py:3173`. |
| `services/disputa_emissao.py:1518` | `reabrir_para_correcao` | P/QuerySet.update | SEM-TRAVA via `acoes_disputa.py:103`, `validacao.py:588`, `curar_perguntas_em_limbo.py:69`; atomic próprio `:1496`. TRAVA-ANTES via `materializacao.py:672/676/679` (devolução da ausência rejeitada). |
| `services/disputa_emissao.py:1541` | `responder_automaticamente` | P/save | SEM-TRAVA local; atomic `:1540`; chamada `api/foto_recebida.py:48`, antes de validar (`:51`). Atomic/lock ancestral dessa entrada precisa ser consolidado pelo agente responsável por API. |
| `services/disputa_emissao.py:1562` | `carimbar_resposta_pela_trilha` | P/save | SEM-TRAVA; atomic `:1561`, command chamador `curar_perguntas_em_limbo.py:67` sem lock |
| `services/fio.py:870` | `reapontar_resolvedora` | P/save | TRAVA-ANTES `:855` no ramo sem datar; ramo datando usa `fato_livre_para` em `:849`, que trava C em `models.py:2646`; atomic `fio.py:843` |
| `services/fio.py:965` | `rechavear_data_evento` | P/save | TRAVA-ANTES `:947`, atomic `:943` |

Todos os caminhos P sem trava continuam sendo **SEM-TRAVA**, e não SEM-ATOMIC: o override abre atomic em `app/chamados/models.py:3173` e escreve em `:3174`.

Laços multicolaborador confirmados nesta área:

- `app/chamados/services/silencio_isentos.py:41-43`, atomic envolvendo queryset inteiro; `alvos(None)` pega vários colaboradores (`:24-28`).
- `app/chamados/services/relavra_via.py:117-127`, sob atomic único em `app/chamados/management/commands/relavrar_via_i6.py:101-102`; seleção sem restrição por colaborador em `:55`.

O exemplo `cobranca.py:176-178` é laço de vários pais, com perguntas recortadas pelo colaborador de entrada (`cobranca.py:140`, `:163`; `aviso_pendencia.py:10`). O pai é obtido por `p.disputa.chamado` (`cobranca.py:60-66`); o código não confirma aqui que o dono CH seja o mesmo da disputa. Mais de um dono efetivo nesse laço: [nao verificado].

Outros laços com atomic por unidade, não abrangendo todos os colaboradores: `relavra_via.py:215/219`, `passivo_validado.py:353/371` (por colaborador/competência), `passivo_validado.py:403/404` (por disputa), `limbo_folgas.py:316` → `agrupar_furo` com atomic em `:270`.

## 07/11 23:32 · sessao 2783e9e9098d · - · - · origem=Ronald (tmux)

Inventário completo dos sítios explícitos em `app/chamados/models.py`; não encontrei escrita SQL direta em `app/chamados/signals.py`, mas os receivers delegam aos escritores indicados abaixo (`signals.py:42,72,115,163,203,224,265,292,321,355`).

Abreviações: CH = ChamadoColaborador; D = DisputaSupervisao; P = PerguntaDisputa. Classificações mistas exigem desdobramento por caminho; não inventei uma garantia universal.

| Sítio (`app/chamados/models.py`) | Função | Entidade/operação | Classificação/evidência |
|---|---|---|---|
| 296 | `_gravar_estado` | CH.save | TRAVA-ANTES, `290`, dentro do atomic `280` |
| 498 | `rebaixar` | CH.save | INDETERMINADO: chama porta `497`, que só trava com atomic preexistente (`484–486`); busca por chamadas de produção não encontrou chamador |
| 553 | `escalar_urgencia` | CH.save | Misto: TRAVA-ANTES `552→486` quando atomic externo, como command `escalonar_chamados_supervisao.py:131–138`; SEM-ATOMIC no command `processar_alertas_avancados.py:173` |
| 571 | `apagar_em_ensaio` | CH.delete | Não alcança banco de produção segundo guarda chamada `569–570`; caller `zerar_dados_operacionais.py:80–84`, atomic externo sem trava C. Guarda concreta `registro_batida.py:316` precisa leitura complementar |
| 591 | `definir_prazo_sla` | CH.save | Misto: TRAVA-ANTES `590→486`, cobrança com atomic `cobranca.py:176–178`; `disputa_emissao.py:1604` não abre atomic nessa função, chamadores externos ainda necessários |
| 610 | `marcar_escalonado_dp` | CH.save | SEM-ATOMIC no único caller encontrado, `escalonar_chamados_supervisao.py:103`; porta `609→484` retorna sem travar em autocommit |
| 636 | `_rebaixar_sem_post_save` | CH.QuerySet.update | INDETERMINADO por caminho originador do save; receiver `signals.py:54–72` não abre atomic nem trava; herda a transação do save |
| 971 | `abrir` | CH.get_or_create | TRAVA-ANTES `905`, atomic `902` |
| 1095 | `criar` | CH.create | SEM-TRAVA no caminho API `api/views.py:2201`; atomic próprio `1094`, nenhum C antes do create na função |
| 1928 | `DisputaSupervisao.fechar` | D.save | TRAVA-ANTES `1902`, atomic `1897` |
| 1960 | `aparar_motivo_respondido` | D.save | SEM-ATOMIC no caminho `api/views.py:887→api/foto_recebida.py:72` e gêmeo `api/views_core.py:877`; aparo precede resposta/validação `foto_recebida.py:73` |
| 2057 | `remover_motivo` | D.save | TRAVA-ANTES `2018/2020`, atomic `2017` |
| 2097 | `anotar_contexto` | D.save | SEM-ATOMIC nos callers `chamados/views.py:1343` e `escalonar_chamados_supervisao.py:159` |
| 2115 | `renovar_contexto` | D.save | SEM-ATOMIC no único caller encontrado `alertar_disputas_pendentes.py:48`, loop `42`, handle `30` |
| 2153 | `reabrir_admin` | D.save | SEM-ATOMIC: `acoes_disputa.py:256`, vindo `fio.py:583`; factory conversa `260` e reabertura CH `267` vêm depois |
| 2169 | `reabrir_sistema` | D.save | Misto. TRAVA-ANTES em `relavra_via.py:230→261`, `reabrir_orfas_enterradas.py:153→165`, `materializacao.py:676→683`. Via `models.py:1583` ocorre depois da saída do atomic próprio `1547–1555`; depende de atomic externo do caller |
| 2227 | `reabrir_por_colab` | D.save | SEM-ATOMIC nas views `chamados/views.py:1334` / `api/views.py:2718`: `_reabrir` termina seu atomic antes de voltar (`disputa_emissao.py:1496–1520`) |
| 2253 | `limpar_resolucao_residual` | D.save | INDETERMINADO: método sem atomic/trava, nenhum caller de produção encontrado |
| 2769 | `_nascer` | P.save existente | TRAVA-ANTES `2691`; ramo só existe sob `if not conversa` (`2713`) |
| 2773 | `_nascer` | P.create | Misto: datada TRAVA-ANTES `2691`; conversa SEM-TRAVA no caminho `acoes_disputa.py:260`, atomic da factory `2689` e guarda da trava `2690` |
| 2907 | `carimbar` | P.save | SEM-TRAVA comprovado pelo consumidor: `reconciliador.py:1355→582/586`; não há atomic externo nessa etapa, save abre atomic `3173`. Outro caminho `1365→1136/1137→encerrar_fantasma→carimbar` também sem C |
| 2945 | `carimbar_identidade` | P.save | Misto/depende do caller: coberto por D.fechar `1902→1913→2819`; remover motivo `2018→2053`; restante em fio/materialização precisa consolidação do outro explorer |
| 2967 | `descarimbar` | P.save | Misto: TRAVA-ANTES em `relavra_via.py:230→246`, `reabrir_orfas_enterradas.py:153→157`; SEM-TRAVA em `competencia_trancada.py:253–254→models.py:3073`, salvo trava de outer caller ainda não lido |
| 3015–3017 | `descarimbar_em_lote` | P.QuerySet.update | Misto: TRAVA-ANTES via `models.py:1552→1555`; sem C próprio via `disputa_emissao.py:1496→1516` |
| 3027 | `encerrar_por_fato_no_espelho` | P.save | SEM-TRAVA: único caller encontrado command `encerrar_perguntas_com_fato.py:67–68`, atomic por pergunta, antes de fechamento D em outro atomic `87–88` |
| 3174 | `save` override | P.super.save | INDETERMINADO por caller; sempre abre atomic `3173`, portanto não classificar esse sítio como SEM-ATOMIC |

Precisões para consolidação:

- A porta de campo retorna sem travar em autocommit (`models.py:483–486`). Não se pode transformar todo `trava_a_ordem_para_escrever()` em TRAVA-ANTES sem subir ao caller.
- `ressuscitar_perguntas` trava C em `1552`, atualiza P em `1555`, sai do próprio atomic e depois reabre D em `1582–1583`. Callers encontrados: `models.py:1441`, `views_arquivar.py:86`, `cartorio.py:1622`. O `reabrir()` de CH também não mantém um atomic próprio entre `_gravar_estado` (`1432`) e ressuscitação (`1441`).
- `cobranca.py:176–178` percorre `pais` obtidos pelos FKs `p.disputa.chamado` (`51–67`), partindo de pendências de um colaborador (`163–164`). Dado fornecido não prova se existem pais de dono diferente; multiplicidade de donos nesse laço é [nao verificado].
- O consumidor chama os carimbos de P sem atomic externo: `reconciliador.py:1345–1373`; só a marcação de EventoCelula abre atomic em `1378`. Travas obtidas e já commitadas pela etapa anterior não cobrem esses saves. [nao verificado]

Signals que disparam escrita D/CH/P, ainda que por delegação:

| Receiver | Passo de escrita |
|---|---|
| `signals.py:19` disputa fechada | atomic `41`, CH.reconciliar `42` → CH._gravar_estado e trava `models.py:290` |
| `signals.py:54` urgência ao resolver | chama CH._rebaixar_sem_post_save `72` → update `models.py:636`; sem atomic/trava próprios |
| `signals.py:91` resolver perguntas | atomic `114`, resolver `115` → serviço de disputa |
| `signals.py:121` retomar regularizações | callback `132–144`, chama `retomar_regularizacoes_confirmadas` `137` |
| `signals.py:148` materializar pendentes | serviço `163`, na transação originadora |
| `signals.py:188` cartório CH | `203`, na transação originadora |
| `signals.py:215` becos P | callback `220–225`, serviço `224` |
| `signals.py:256` isento Colaborador | callback `261–266`, silenciar `265` |
| `signals.py:270` isento CH | callback `288–293`, silenciar `292` |
| `signals.py:312` feriado | callback `317–322`, reavaliar `321` |
| `signals.py:337` filho morto | callback `350–356`, reconciliador `355` |

`_cartorio_pre_save_chamado` apenas lê (`signals.py:179–184`); `_outbox_post_save_pergunta` grava EventoCelula, não uma das três entidades (`signals.py:238–244`).

Configurações lidas: `config/settings/saas.py:83–92` e `prod.py:9–18` não ativam ATOMIC_REQUESTS; busca em `app/config` não achou ATOMIC_REQUESTS/AUTOCOMMIT. Perfil/config efetivamente carregado em produção: [nao verificado].

## 07/11 21:33 · sessao 348e377151bc · FIO-E0-V4-E1 · verificador · origem=Code

**1. C8 — PROCEDENTE o +2 por save; NÃO PROCEDE tratá-lo como explicação completa do caminho.** O helper de campo pede C e aquele Chamado; isso executa dois SELECTs, mesmo quando as travas são reentrantes. `renovar_contexto` chama o helper antes do save e pode retornar antes dele por igualdade. (`app/chamados/models.py:484`, `app/chamados/models.py:486`, `app/chamados/models.py:2547`, `app/chamados/models.py:2567`, `app/chamados/services/contexto.py:98`, `app/chamados/services/contexto.py:103`)

Há outro custo que essa justificativa não discrimina: na emissão, `trava_na_ordem(...abertas_do_colaborador=True)` consulta **C, Disputas abertas e Chamados vivos/pais**; depois ocorre uma leitura simples separada da Disputa. São quatro SELECTs nesse trecho, inclusive quando os conjuntos de Disputas/Chamados estão vazios. A fábrica do cartório alcança esse núcleo. (`app/chamados/services/disputa_emissao.py:530`, `app/chamados/services/disputa_emissao.py:531`, `app/chamados/models.py:2547`, `app/chamados/models.py:2553`, `app/chamados/models.py:2566`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/disputa_emissao.py:662`)

Portanto, **a busca dos Chamados vivos e a separação entre travar e reler a Disputa ficam fora da decomposição escrita dos tetos**. O comentário histórico descreve apenas +1 de C nesse portão; os comentários novos atribuem +2/+6 às portas de campo. (`app/ponto/tests/test_selo_chokepoint_escrita.py:174`, `app/ponto/tests/test_selo_chokepoint_escrita.py:180`, `app/ponto/tests/test_selo_chokepoint_escrita.py:187`)

**Que essas consultas adicionais tenham sido introduzidas exatamente em 12b/12c, ou expliquem determinado delta contra versões anteriores: [nao verificado].** O delta fornecido só muda tetos/comentários; não fornece aquela implementação anterior. Assim, o +2/+6 está explicado localmente, mas a exclusividade desse acréscimo não está provada. **[nao verificado]**

**2. Triagem v14, itens 1 a 7.**

| Item | Veredito da conferência |
|---|---|
| **1 — Ordem global** | **PROCEDENTE o residual.** O comando envolve várias células em um atomic; o cartório percorre células/chamados e ressuscita; cada ressuscitação antecipa apenas suas Disputas, seu Chamado e suas Perguntas. Com duas candidatas distintas, D2/CH2 podem ser aquisições novas após P1. Chamá-lo “pergunta de desenho” não fecha a violação da ordem. (`app/ponto/management/commands/celula_veredito_velho.py:133`, `app/ponto/management/commands/celula_veredito_velho.py:141`, `app/ponto/services/cartorio.py:1303`, `app/ponto/services/cartorio.py:1622`, `app/chamados/models.py:1552`) |
| **2 — Porta fora de transação** | **NÃO PROCEDE como inversão sob autocommit.** A porta de arquivo não abre atomic; o helper retorna fora de atomic. A triagem preserva a ressalva que v14 já fazia. Configuração efetiva de autocommit em produção: **[nao verificado]**. (`app/chamados/services/arquivo.py:141`, `app/chamados/services/arquivo.py:170`, `app/chamados/services/arquivo.py:171`, `app/chamados/models.py:484`) |
| **3 — Ressuscitação** | **PROCEDENTES a janela e a seleção anterior à trava.** O atomic interno termina após descarimbar, e a reabertura vem depois; o universo é escolhido antes da aquisição de C. Classificar como preexistente não corrige esses fatos, mas também não contradiz a ressalva de v14. Comparação histórica independente: **[nao verificado]**. (`app/chamados/models.py:1533`, `app/chamados/models.py:1547`, `app/chamados/models.py:1552`, `app/chamados/models.py:1555`, `app/chamados/models.py:1582`) |
| **4 — Campos/comportamento** | **NÃO PROCEDE alteração explícita de valores no delta dos tetos. PROCEDENTE a diferença de disparo do S143 na volta inteira:** saves de campo retornam pela guarda. Declarar essa mudança não a torna apenas ordem de travas. (`app/ponto/tests/test_selo_chokepoint_escrita.py:180`, `app/ponto/tests/test_selo_chokepoint_escrita.py:187`, `app/chamados/signals.py:98`, `app/chamados/signals.py:104`) |
| **5 — Catraca** | **PROCEDENTE-LIMITE.** Ela procura `.save`, aceita chamada de ordem por posição textual e não verifica receptor, ramo ou fluxo; seu universo é limitado. Conferência somente em memória reproduziu **0 acusações atuais e 16 sem o helper**. Isso confirma mordida local, sem cobrir o lote residual. (`app/chamados/tests/test_e1_v12.py:212`, `app/chamados/tests/test_e1_v12.py:228`, `app/chamados/tests/test_e1_v12.py:234`, `app/chamados/tests/test_e1_v12.py:242`, `app/chamados/tests/test_e1_v12.py:137`) |
| **6 — Custo/selo** | **PROCEDENTE a multiplicação por save.** A justificativa é incompleta como censo do caminho, conforme item 1. Atribuir a diferença entre execuções a “memo/relógio” como causa estabelecida é **[nao verificado]**. (`app/chamados/models.py:486`, `app/chamados/models.py:2547`, `app/chamados/models.py:2567`, `app/chamados/services/disputa_emissao.py:530`) |
| **7 — Classes anteriores** | **Ordem permanece PROCEDENTE**, pelo item 1. **Dois significados permanecem PROCEDENTES:** coluna nomeada com `update_fields`, valor diferente no save inteiro; “declarado” reconhece, não elimina a diferença. “Pré-requisito fechado pela sombra no hash final”: **[nao verificado]**, sem prova dessa execução no material. (`app/chamados/models.py:1552`, `app/chamados/signals.py:79`, `app/chamados/signals.py:85`, `app/chamados/signals.py:87`) |

As duas afirmações sobre as curas candidatas conferem:

- **“Topo com abertas não alcança a FECHADA” — PROCEDENTE.** A flag filtra `fechada_em__isnull=True`; a ressuscitação procura expressamente Disputas fechadas para reabri-las. A aquisição local da segunda ressuscitação não antecipa D2 antes de P1. (`app/chamados/models.py:2552`, `app/chamados/models.py:1582`, `app/chamados/models.py:1552`)
- **“C primeiro basta exige censo” — PROCEDENTE.** Existem escritores sem aquisição local prévia de C: `descarimbar_em_lote` faz UPDATE direto; `reabrir_para_correcao` o chama ou usa `qs.update`; a resposta HTML chega ao save da Pergunta sem essa aquisição na cadeia examinada. O override de `Pergunta.save` acrescenta atomic, sem adquirir C. (`app/chamados/models.py:3015`, `app/chamados/services/disputa_emissao.py:1496`, `app/chamados/services/disputa_emissao.py:1516`, `app/chamados/services/disputa_emissao.py:1518`, `app/chamados/views.py:1283`, `app/chamados/services/disputa_emissao.py:1388`, `app/chamados/models.py:3173`)

**A triagem não refutou os achados que reconheceu como desenho, preexistência ou limite. Se esses rótulos forem usados como baixa, o motivo é insuficiente; especialmente nos itens 1 e 7.** A ordem residual e os dois predicados continuam presentes. Deadlock efetivo e ocorrência das duas candidatas em produção: **[nao verificado]**. (`app/chamados/models.py:1552`, `app/chamados/signals.py:85`, `app/chamados/signals.py:87`)

**3. Estado que explica 211 versus 217 — [nao verificado].** Passar sob teto 211 demonstra **≤211**, sem revelar a contagem exata: o selo usa `assertLessEqual`. (`app/ponto/tests/test_selo_chokepoint_escrita.py:240`, `app/ponto/tests/test_selo_chokepoint_escrita.py:241`)

O código oferece fontes de variação, mas não identifica qual ocorreu nas duas rodadas:

- **Relógio:** `setUpTestData` calcula `dia = localdate()-2`; o cartório consulta novamente data/hora; o runner permite congelamento por `CONGELAR_EM`. Valores efetivos nas duas execuções: **[nao verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:229`, `app/ponto/services/cartorio.py:1965`, `app/ponto/services/cartorio.py:1989`, `app/core/runner_quarentena.py:94`)
- **Memo:** empresa e tranca são memorizadas no request ou na instância do colaborador. Os FICAM não fazem limpeza explícita; a limpeza existente pertence à comparação com rollback em outro teste. Isso não prova aquecimento pelo `setUpTestData` nem transmissão entre métodos. Estado efetivo do memo: **[nao verificado]**. (`app/chamados/juizes.py:957`, `app/chamados/juizes.py:963`, `app/chamados/juizes.py:993`, `app/ponto/tests/test_selo_chokepoint_escrita.py:393`, `app/ponto/tests/test_selo_chokepoint_escrita.py:440`)
- **Percurso:** o espelho pode retornar sem save; o 3-FICAM exige somente **pelo menos um** chamado vivo, não três. Portanto, o passe não certifica três saves de espelho. (`app/ponto/management/commands/detectar_ausencias.py:556`, `app/ponto/management/commands/detectar_ausencias.py:558`, `app/ponto/tests/test_selo_chokepoint_escrita.py:425`)

**Não há no dado fotografia de memo, relógio e consultas das duas execuções que permita atribuir as consultas ausentes a um desses estados. A explicação causal afirmada pela triagem permanece [nao verificado].**

## 07/11 21:31 · sessao 1e8be6e76dc3 · - · - · origem=Ronald (tmux)

Aprofundamento: **não há prova de que o memo foi aquecido por `setUpTestData`**, nem de que o memo passou de um método do selo ao seguinte; a limpeza dentro de uma comparação com rollback não demonstra essa herança (`app/ponto/tests/test_selo_chokepoint_escrita.py:435-447`).

- Colaborador nasce antes de `cls.dia`; seu `save` normaliza campos e gera matrícula, sem chamar o memo do C6 (`app/ponto/tests/test_selo_chokepoint_escrita.py:226-229`; `app/colaboradores/models.py:415-439`).
- Escala nasce depois de `cls.dia`; seu receiver recompõe turnos numa janela calculada pelo relógio, enquanto os receivers de regeneração retornam quando `created=True` (`app/ponto/tests/test_selo_chokepoint_escrita.py:230-233`; `app/ponto/signals.py:53-74`; `app/escala/signals.py:54-55`; `app/escala/signals.py:104-105`). Aquecimento indireto do memo por esse percurso: **[nao verificado]**.
- `get_user_sistema` não possui cache próprio: executa `get_or_create` (`app/core/utils.py:6-18`).
- O runner permite congelamento pelo ambiente `CONGELAR_EM`; a utilização e o instante nas rodadas citadas são **[nao verificado]** (`app/core/runner_quarentena.py:93-100`).

**Conclusão mantida:** a causa das consultas ausentes continua **[nao verificado]**. O dado fornece um passe sob teto 211, não contagem exata nem fotografia de memo, relógio ou consultas (`app/ponto/tests/test_selo_chokepoint_escrita.py:240-246`).

## 07/11 21:31 · sessao 69aba3bfc04d · - · - · origem=Ronald (tmux)

**4 — NÃO PROCEDE alteração explícita dos valores pelo delta fornecido; PROCEDENTE a mudança de comportamento na volta inteira.** A guarda do S143 retorna para save que não nomeia `status_local`; portanto há mudança de disparo, declarada no próprio código. (`app/chamados/signals.py:98`, `app/chamados/signals.py:104`) O delta anexado altera somente tetos/comentários; demais diferenças históricas para o pai são **[nao verificado]**.

**5 — PROCEDENTE-LIMITE.** A catraca procura apenas `ast.Call` cujo método é um `Attribute.save`, aceitando qualquer chamada de ordem em linha anterior, sem conferir receptor, ramo ou fluxo. Alias do método não é visto; alias do objeto continua sendo `.save`. (`app/chamados/tests/test_e1_v12.py:228`, `app/chamados/tests/test_e1_v12.py:231`, `app/chamados/tests/test_e1_v12.py:234`) O universo é limitado a quatro módulos e à classe `ChamadoColaborador`; helpers externos ficam fora. (`app/chamados/tests/test_e1_v12.py:212`, `app/chamados/tests/test_e1_v12.py:242`) Os testes de mordida existem; execução e contagem efetiva de 16 acusações nesta revisão são **[nao verificado]**. (`app/chamados/tests/test_e1_v12.py:253`, `app/chamados/tests/test_e1_v12.py:259`)

**6 — PROCEDENTE o custo de duas queries por helper executado; NÃO PROCEDE usá-lo como descrição completa do caminho.** O helper retorna fora de atomic ou sem PK e chama `trava_na_ordem` somente com C e CH; isso executa os SELECTs de C e CH. (`app/chamados/models.py:484`, `app/chamados/models.py:486`, `app/chamados/models.py:2547`, `app/chamados/models.py:2567`) `renovar_contexto` pode retornar antes do helper. (`app/chamados/services/contexto.py:98`)

Além disso, no núcleo da emissão, `trava_na_ordem(...abertas_do_colaborador=True)` executa C, Disputas e Chamados vivos; depois há uma leitura simples separada da Disputa. São **quatro SELECTs** nesse trecho, mesmo se as coleções de D/CH retornarem vazias. (`app/chamados/services/disputa_emissao.py:530`, `app/chamados/services/disputa_emissao.py:531`, `app/chamados/models.py:2547`, `app/chamados/models.py:2553`, `app/chamados/models.py:2566`) Esse núcleo é alcançado pela fábrica do cartório. (`app/chamados/services/fabrica_da_celula.py:184`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/disputa_emissao.py:662`) C8 descreve o +1 histórico de C e o +2/+6 das portas, mas não explica nessa justificativa a busca dos vivos e a separação entre trava e leitura do núcleo. (`app/ponto/tests/test_selo_chokepoint_escrita.py:174`, `app/ponto/tests/test_selo_chokepoint_escrita.py:180`, `app/ponto/tests/test_selo_chokepoint_escrita.py:187`) Quantificação desse delta contra implementação anterior não disponível: **[nao verificado]**.

**7 — PROCEDENTE a duplicidade de predicados, mesmo declarada.** Com `update_fields`, `_salvou_o_status` pergunta se a coluna foi nomeada; no save inteiro pergunta se o valor difere da leitura anterior. Declarar não transforma os dois critérios num só. (`app/chamados/signals.py:79`, `app/chamados/signals.py:85`, `app/chamados/signals.py:87`) “Pouso antes do pré-requisito fecha com sombra no hash final”: **[nao verificado]**; o dado apenas afirma o fechamento, sem prova da sombra executada.

## 07/11 21:30 · sessao a70fe2b95732 · - · - · origem=Ronald (tmux)

- **Triagem 1: PROCEDENTE quanto ao residual; “é desenho” não o fecha.** O comando mantém várias células sob um atomic (`app/ponto/management/commands/celula_veredito_velho.py:133`, `:141`, `:144`); o cartório percorre células e chamados e ressuscita (`app/ponto/services/cartorio.py:1303`, `:1591`, `:1622`). Cada ressuscitação trava apenas suas disputas, seu chamado e seus IDs (`app/chamados/models.py:1541`, `:1552`): a segunda pode adquirir D2/CH2 novos depois de P1; o rastreador rejeita precisamente aquisição nova de rank menor (`app/chamados/tests/test_e1_v9.py:86`, `:106`). Existência das duas candidatas em produção e deadlock efetivo: **[nao verificado]**.

- **Candidata “topo com abertas”: insuficiência PROCEDENTE.** A flag adiciona somente disputas `fechada_em__isnull=True` (`app/chamados/models.py:2552`), enquanto ressuscitação alcança expressamente as fechadas e as reabre (`app/chamados/models.py:1582`, `:1583`). A trava local pode pegá-las antes da própria Pergunta, mas não antecipa a fechada da segunda ressuscitação antes de P1 (`app/chamados/models.py:1552`).

- **Candidata “C primeiro basta”: garantia universal NÃO demonstrada; exigência de censo PROCEDENTE.** Há escritores sem aquisição local de C: `descarimbar_em_lote` faz UPDATE direto (`app/chamados/models.py:3007`, `:3015`); `reabrir_para_correcao` entra em atomic e faz esse UPDATE ou `qs.update`, sem chamar a trava (`app/chamados/services/disputa_emissao.py:1496`, `:1516`, `:1518`). Há caminho produtivo de resposta HTML até `p.save()` sem C nessa cadeia: view seleciona e chama core (`app/chamados/views.py:1228`, `:1239`, `:1283`), core salva dentro de atomic (`app/chamados/services/disputa_emissao.py:1388`, `:1389`), e o override apenas envolve `super().save()` em atomic (`app/chamados/models.py:3173`, `:3174`). A API também chama o core diretamente (`app/api/views.py:2644`, `:2659`). Esses exemplos derrubam a premissa “todos entram por C”; não demonstram, sozinhos, um ciclo de deadlock **[nao verificado]**.

- **Triagem 2: NÃO PROCEDE como inversão sob autocommit; não descartou o que v14 afirmava.** View chama `silenciar` (`app/chamados/views_arquivar.py:37`), porta não abre atomic e salva (`app/chamados/services/arquivo.py:141`, `:170`, `:171`), helper retorna fora de atomic (`app/chamados/models.py:484`). Receiver pode chamar cartório quando status da instância diverge da leitura (`app/chamados/signals.py:184`, `:192`, `:203`). Configuração e estado efetivos de autocommit na execução: **[nao verificado]**.

- **Triagem 3: janela PROCEDENTE; classificá-la como preexistente não a corrige.** O bloco interno termina após descarimbar (`app/chamados/models.py:1547`, `:1555`); reabertura vem depois (`app/chamados/models.py:1559`, `:1582`). Seleção ocorre antes da trava (`app/chamados/models.py:1533`, `:1541`, `:1552`). Comparação histórica independente com pai indisponível nesta cópia sem `.git`: **[nao verificado]**.

- **Triagem 7: “declarado” não elimina dois significados.** O código declara e implementa coluna nomeada quando há `update_fields` e valor divergente no save inteiro (`app/chamados/signals.py:79`, `:85`, `:87`). Ordem permanece o residual do item 1 (`app/chamados/models.py:1552`). “Fecha com prova da sombra no hash final” não pode ser confirmado pelo material fornecido: **[nao verificado]**.

## 07/11 21:30 · sessao 1e8be6e76dc3 · - · - · origem=Ronald (tmux)

**Causa exata da diferença: [nao verificado].** O dado relata que o 3-FICAM passou com teto 211; isso demonstra **≤211**, não medição exata de 211. O selo usa `assertLessEqual`, sem imprimir contagem quando passa (`app/ponto/tests/test_selo_chokepoint_escrita.py:240-246`).

O código comprova estas fontes de variação:

- **Data e relógio:** `setUpTestData` calcula `cls.dia = localdate()-2`, sem data fixa; o cartório consulta novamente `localdate()` e `now()` ao julgar (`app/ponto/tests/test_selo_chokepoint_escrita.py:229`; `app/ponto/services/cartorio.py:1965`; `app/ponto/services/cartorio.py:1989`).
- **Memo:** o leitor guarda empresa e resultado de `periodo_trancado` por chave de empresa/mês/ano; o dono é o request ou a instância do colaborador (`app/chamados/juizes.py:957-965`; `app/chamados/juizes.py:983-996`). Os testes FICAM não limpam esse memo; a limpeza explícita existente pertence a outro teste, de comparação do gate (`app/ponto/tests/test_selo_chokepoint_escrita.py:393-426`; `app/ponto/tests/test_selo_chokepoint_escrita.py:435-440`).
- **Caminho efetivamente executado:** `_espelhar_marcos` retorna sem save se a lista já coincide; o selo 3-FICAM exige apenas **pelo menos um** dos três chamados vivo (`app/ponto/management/commands/detectar_ausencias.py:555-559`; `app/ponto/tests/test_selo_chokepoint_escrita.py:423-426`).

Portanto, “dependência de ordem por memo/relógio do setUpTestData” é uma hipótese compatível com essas portas, mas a execução anterior que aqueceu o memo, os valores do memo, as datas/horas de ambas as rodadas e as consultas ausentes não constam do dado: **[nao verificado]**. Não atribuir as seis consultas especificamente ao memo, ao relógio ou a três saves sem dump comparativo: **[nao verificado]**.

## 07/11 21:11 · sessao c09619854497 · FIO-E0-V4-E1 · verificador · origem=Code

**PROCEDENTE: ainda existe aquisição de Disputa/Chamado depois de Pergunta do mesmo colaborador, na mesma transação. A cura unitária não fecha a ordem global do lote.** O contraexemplo estático é o cartório ressuscitando perguntas de duas células do mesmo colaborador sob atomic externo. Ocorrência desse estado na frota e deadlock efetivo: **[nao verificado]**. (`app/ponto/management/commands/celula_veredito_velho.py:133`, `app/ponto/services/cartorio.py:1303`, `app/ponto/services/cartorio.py:1622`, `app/chamados/models.py:1552`)

**1. Ordem global — PROCEDENTE o residual; NÃO PROCEDE repetir como intactas as inversões unitárias corrigidas.**

A cadeia residual é:

| Passo | Evidência |
|---|---|
| `celula_veredito_velho`, inclusive com `--aplicar`, envolve a janela em um atomic e entrega várias células do mesmo colaborador ao cartório. | `app/ponto/management/commands/celula_veredito_velho.py:44`, `app/ponto/management/commands/celula_veredito_velho.py:133`, `app/ponto/management/commands/celula_veredito_velho.py:141`, `app/ponto/management/commands/celula_veredito_velho.py:156` |
| `julgar_colab → _julgar_colab_corpo` percorre as células; cada chamado elegível pode ressuscitar suas perguntas. | `app/ponto/services/cartorio.py:948`, `app/ponto/services/cartorio.py:1303`, `app/ponto/services/cartorio.py:1591`, `app/ponto/services/cartorio.py:1622` |
| Primeira ressuscitação: adquire `C → D1 → CH1 → P1`. Seu universo contém apenas as disputas das perguntas escolhidas, `self.pk` e os IDs escolhidos. | `app/chamados/models.py:1541`, `app/chamados/models.py:1552` |
| Segunda célula elegível, com D2/CH2 distintos: C é reentrante; **D2 e CH2 são aquisições novas depois de P1**, ainda no atomic externo. | `app/chamados/models.py:1552`, `app/chamados/models.py:2548`, `app/chamados/models.py:2554`, `app/chamados/models.py:2567` |

Esse contraexemplo pressupõe duas ressuscitações elegíveis e nenhuma aquisição anterior cobrindo D2/CH2. Os reconciliadores anteriores são condicionais; não garantem antecipadamente esse universo. Existência concreta das candidatas: **[nao verificado]**. (`app/ponto/services/cartorio.py:1488`, `app/ponto/services/cartorio.py:1501`, `app/ponto/services/cartorio.py:1514`)

**Isso demonstra quebra da ordem exigida, não um deadlock demonstrado:** C já está retido e serializa outra transação que também entre por C. Reentrância de C não torna D2/CH2 reentrantes. O próprio rastreador denuncia a aquisição nova de rank menor depois de Pergunta. (`app/chamados/models.py:2547`, `app/chamados/tests/test_e1_v9.py:86`, `app/chamados/tests/test_e1_v9.py:106`)

As **16 portas** efetivamente chamam o helper antes do save:

| Grupo | Portas e localização da trava |
|---|---|
| Texto — 1 | `reescrever_texto`: `app/chamados/services/texto.py:35` |
| Contexto — 4 | `anotar_contexto`: `app/chamados/services/contexto.py:77`; `renovar_contexto`: `app/chamados/services/contexto.py:103`; `retirar_contexto`: `app/chamados/services/contexto.py:126`; `fundir_contexto`: `app/chamados/services/contexto.py:162` |
| Vínculos — 5 | `apontar_justificativa`: `app/chamados/services/vinculos.py:88`; `apontar_celula`: `app/chamados/services/vinculos.py:100`; `apontar_ticket_glpi`: `app/chamados/services/vinculos.py:116`; `assumir_responsavel`: `app/chamados/services/vinculos.py:134`; `liberar_responsavel`: `app/chamados/services/vinculos.py:146` |
| Arquivo — 2 | `silenciar`: `app/chamados/services/arquivo.py:170`; `levantar`: `app/chamados/services/arquivo.py:209` |
| Modelo — 4 | `rebaixar`: `app/chamados/models.py:497`; `escalar_urgencia`: `app/chamados/models.py:552`; `definir_prazo_sla`: `app/chamados/models.py:590`; `marcar_escalonado_dp`: `app/chamados/models.py:609` |

O helper adquire **Colaborador e aquele Chamado**, sem antecipar o universo de Disputas. Ele corrige a aquisição tardia de C nessas portas, mas não prova sozinho a ordem de toda continuação possível. (`app/chamados/models.py:486`, `app/chamados/models.py:2553`, `app/chamados/models.py:2566`)

Em `agrupar_furo`, a cadeia unitária anterior foi corrigida: o topo adquire C, Disputas abertas, pais/vivos e furo/pai antes de contexto, reapontamento e reconciliação. A solicitação posterior dessas mesmas linhas é reentrante. (`app/chamados/services/limbo_folgas.py:274`, `app/chamados/services/limbo_folgas.py:275`, `app/chamados/services/limbo_folgas.py:282`, `app/chamados/services/limbo_folgas.py:283`, `app/chamados/services/fio.py:855`)

Os receivers relevantes ficam assim:

| Receiver | Cadeia e efeito sobre a ordem |
|---|---|
| Chamado: rebaixamento | Faz UPDATE de urgência no mesmo PK, pelo ritual do modelo. (`app/chamados/signals.py:72`, `app/chamados/models.py:636`) |
| Chamado: S143 | Save de campo retorna pela guarda; save canônico de status já adquiriu o universo antes. Depois, o resolver solicita C → Disputas → Chamados e pode carimbar Perguntas. (`app/chamados/signals.py:104`, `app/chamados/models.py:290`, `app/chamados/services/disputa_emissao.py:1683`, `app/chamados/services/disputa_emissao.py:1749`) |
| Chamado: cartório | Compara banco/instância e pode julgar sincronamente, inclusive com instância desatualizada. A fábrica pode alcançar nova aquisição de universo. (`app/chamados/signals.py:191`, `app/chamados/signals.py:203`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/disputa_emissao.py:530`) |
| Chamado: isentos e filho morto | Registram trabalho em `on_commit`; esse trabalho não permanece dentro da transação anterior. (`app/chamados/signals.py:293`, `app/chamados/signals.py:356`) |
| Disputa: S88 | Ao salvar fechada, pode chamar `pai.reconciliar → _gravar_estado → trava_na_ordem`. O fechamento canônico antecipa o universo; na reabertura, S88 retorna. (`app/chamados/signals.py:28`, `app/chamados/signals.py:42`, `app/chamados/models.py:1902`, `app/chamados/models.py:290`) |
| Disputa: retomada | Trabalho em `on_commit`. (`app/chamados/signals.py:144`) |
| Disputa: S144 | Ao fechar, materializa pendentes pela porta de materialização; na reabertura retorna. (`app/chamados/signals.py:153`, `app/chamados/signals.py:163`, `app/chamados/services/materializacao.py:1428`) |

**2. Porta fora de transação — PROCEDENTE o caminho; NÃO PROCEDE chamá-lo inversão pelo UPDATE já confirmado.**

Existe caminho condicional: `arquivar_chamado → arquivo.silenciar`, sem atomic próprio; o helper retorna. Se o status da instância estiver desatualizado, o post_save do cartório pode prosseguir e alcançar, por exemplo, `ressuscitar_perguntas`, que abre atomic e pede C. S143 não faz isso pelo save de campo, pois `update_fields` não contém status. (`app/chamados/views_arquivar.py:37`, `app/chamados/services/arquivo.py:170`, `app/chamados/models.py:484`, `app/chamados/signals.py:86`, `app/chamados/signals.py:191`, `app/ponto/services/cartorio.py:1622`, `app/chamados/models.py:1547`)

**Sob a premissa de autocommit**, a transação do UPDATE já terminou: ela não continua segurando o Chamado enquanto o receiver pede C em outra transação. Portanto, essa sequência temporal não constitui aquela inversão de travas retidas. Configuração efetiva e execução concorrente em produção: **[nao verificado]**. (`app/chamados/models.py:480`, `app/chamados/models.py:484`, `app/chamados/services/arquivo.py:171`)

Texto, contexto e vínculos abrem atomic antes do helper; entrar nessas funções sem transação externa não faz seu save ocorrer em autocommit. As duas portas de arquivo e os quatro métodos de campo do modelo não abrem atomic próprio. (`app/chamados/services/texto.py:31`, `app/chamados/services/contexto.py:101`, `app/chamados/services/vinculos.py:86`, `app/chamados/services/arquivo.py:141`, `app/chamados/models.py:488`, `app/chamados/models.py:573`)

**3. Ressuscitação — PROCEDENTE a janela; NÃO PROCEDE atribuí-la como novidade ao delta.**

Sem atomic externo, o bloco termina depois de descarimbar as Perguntas, antes do log e de `reabrir_sistema`. Pode ficar confirmado o estado intermediário “Pergunta ressuscitada / Disputa fechada”; erro ou concorrência nessa separação continua possível. A trava nova não ampliou essa fronteira transacional. Ocorrência efetiva: **[nao verificado]**. (`app/chamados/models.py:1547`, `app/chamados/models.py:1555`, `app/chamados/models.py:1559`, `app/chamados/models.py:1582`)

Com atomic externo, as travas sobrevivem à saída do atomic interno. Reabrir **as mesmas Disputas** é reentrante; S88/S144 retornam por estarem abertas, e a nota de reabertura cria Followup sem passar contexto novo. Isso não cobre outra ressuscitação do lote, como demonstrado no item 1. (`app/chamados/models.py:1541`, `app/chamados/models.py:1552`, `app/chamados/models.py:1583`, `app/chamados/models.py:2172`, `app/chamados/signals.py:28`, `app/chamados/signals.py:153`, `app/chamados/services/nota.py:110`)

Também permanece uma seleção anterior à trava: IDs, julgamento e disputas são obtidos antes de adquirir C; a porta não refaz o julgamento depois de eventual espera. Portanto, a adição não prova ausência de seleção envelhecida. Dano concorrente concreto: **[nao verificado]**. (`app/chamados/models.py:1533`, `app/chamados/models.py:1541`, `app/chamados/models.py:1552`, `app/chamados/models.py:1555`)

**4. Campos — NÃO PROCEDE mudança explícita de valores no delta 12b/12c; a volta inteira não é somente ordem de travas.**

Os escritores canônicos e consumidores relevantes às cadeias revisadas são:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `status_local` | Transição em `_gravar_estado`; nascimento em `abrir/criar`. (`app/chamados/models.py:228`, `app/chamados/models.py:296`, `app/chamados/models.py:971`, `app/chamados/models.py:1095`) | S143, cartório e C6. (`app/chamados/signals.py:96`, `app/chamados/signals.py:191`, `app/chamados/juizes.py:758`) |
| `contexto_json` do Chamado | As quatro portas de contexto. (`app/chamados/services/contexto.py:76`, `app/chamados/services/contexto.py:102`, `app/chamados/services/contexto.py:125`, `app/chamados/services/contexto.py:161`) | Cartório lê dia; espelho lê marcos; reconciliador lê filhos. (`app/ponto/services/cartorio.py:126`, `app/ponto/management/commands/detectar_ausencias.py:555`, `app/chamados/reconciliador.py:242`) |
| `contexto` da Disputa | Emissão, remoção de motivo, anotar/renovar. (`app/chamados/services/disputa_emissao.py:544`, `app/chamados/models.py:2057`, `app/chamados/models.py:2097`, `app/chamados/models.py:2115`) | Merge e guardas das próprias portas. (`app/chamados/services/disputa_emissao.py:544`, `app/chamados/models.py:2093`, `app/chamados/models.py:2111`) |
| Justificativa relacionada | `apontar_justificativa`. (`app/chamados/services/vinculos.py:87`) | Reconciliação. (`app/chamados/reconciliador.py:451`, `app/chamados/reconciliador.py:529`) |
| Célula | `apontar_celula`. (`app/chamados/services/vinculos.py:99`) | Juiz do dia. (`app/chamados/juizes.py:100`) |
| Ticket GLPI | `apontar_ticket_glpi`. (`app/chamados/services/vinculos.py:115`) | Encaminhamento da view. (`app/chamados/views.py:220`) |
| Responsável/instante | Assumir/liberar. (`app/chamados/services/vinculos.py:132`, `app/chamados/services/vinculos.py:144`) | Resposta da atribuição e fila. (`app/chamados/views.py:1036`, `app/chamados/services/fila.py:154`) |
| Arquivo/véu | `silenciado_ate/por/motivo`, opcionalmente `resgatado_em`, pelas portas de arquivo. Não existe choice de status “arquivado”. (`app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:202`, `app/chamados/models.py:28`, `app/chamados/models.py:87`) | Predicado temporal, reconciliação da ausência, isentos e prescrição. (`app/chamados/models.py:511`, `app/chamados/services/silencio_ausencia.py:66`, `app/chamados/signals.py:284`, `app/chamados/services/arquivo.py:182`) |
| Urgência/original | Ritual de rebaixamento, escalar, transição e UPDATE do receiver. (`app/chamados/models.py:468`, `app/chamados/models.py:553`, `app/chamados/models.py:246`, `app/chamados/models.py:636`) | Guarda de escalada pelo juiz de SLA. (`app/chamados/models.py:546`) |
| `prazo_sla` | Porta de SLA e nascimento na emissão. (`app/chamados/models.py:588`, `app/chamados/services/disputa_emissao.py:581`) | Juiz do prazo e cobrança. (`app/chamados/catalogo/motor.py:54`, `app/chamados/catalogo/motor.py:68`, `app/chamados/services/cobranca.py:246`) |
| Escalonado DP/em | `marcar_escalonado_dp`. (`app/chamados/models.py:606`) | Comando lê flag; fila lê instante. (`app/ponto/management/commands/escalonar_chamados_supervisao.py:96`, `app/chamados/services/fila.py:181`) |
| Texto do Chamado | `reescrever_texto`, além do nascimento. (`app/chamados/services/texto.py:18`, `app/chamados/services/texto.py:33`) | A própria porta compara o valor anterior. (`app/chamados/services/texto.py:28`) |
| `atualizado_em` | Listado nos saves das portas. (`app/chamados/services/contexto.py:52`, `app/chamados/services/vinculos.py:65`, `app/chamados/services/texto.py:36`) | Marco de atualização do painel. (`app/chamados/services/painel.py:65`) |
| Materialização/autor/via da Pergunta | Atribuição central e portas de carimbar/descarimbar, inclusive lote. (`app/chamados/models.py:2852`, `app/chamados/models.py:2907`, `app/chamados/models.py:2938`, `app/chamados/models.py:3015`) | Estado, C6, seleção S143; a própria porta preserva/restaura autor. (`app/chamados/models.py:2796`, `app/chamados/juizes.py:827`, `app/chamados/services/disputa_emissao.py:1711`, `app/chamados/models.py:2881`) |
| Resposta/respondida/validação/autor | Resposta, validação/devolução e ressuscitação. (`app/chamados/services/disputa_emissao.py:1368`, `app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:199`, `app/chamados/models.py:1555`) | Estado/C6, marco do painel e preservação da decisão histórica. (`app/chamados/models.py:2796`, `app/chamados/juizes.py:827`, `app/chamados/services/painel.py:67`, `app/chamados/models.py:3689`) |
| Lavra/julgamento/expiração da Pergunta | `veredito_lavrado.lavrar`. (`app/chamados/veredito_lavrado.py:227`) | `esta_stale`. (`app/chamados/veredito_lavrado.py:179`) |
| `fechada_em` da Disputa | Fechar e reabrir admin/sistema. (`app/chamados/models.py:1924`, `app/chamados/models.py:2141`, `app/chamados/models.py:2165`) | Receivers, ressuscitação e universo de travas. (`app/chamados/signals.py:28`, `app/chamados/signals.py:129`, `app/chamados/signals.py:153`, `app/chamados/models.py:1582`, `app/chamados/models.py:2552`) |

No delta fornecido, permanecem as atribuições, os filtros e os `update_fields`; entram aquisições de trava. Isso não garante equivalência entre entrelaçamentos concorrentes nem igualdade de timestamps após espera. Contexto/vínculos continuam decidindo pela instância antes da trava, sem refresh. (`app/chamados/models.py:486`, `app/chamados/models.py:1552`, `app/chamados/services/contexto.py:97`, `app/chamados/services/contexto.py:103`, `app/chamados/services/vinculos.py:84`)

Já a **volta inteira** altera o disparo do S143: saves de campo e saves inteiros sem diferença detectada deixam de tentar o resolver. Logo, “só mudou ordem” é falso para `afb67623..f67b36a6`. (`app/chamados/signals.py:83`, `app/chamados/signals.py:104`)

**5. Catraca — PROCEDENTE o alcance limitado; NÃO localizei outro escritor produtivo por save fora desse conjunto.**

A varredura de escritores da árvore e a inspeção complementar não localizaram `ChamadoColaborador.save()` produtivo adicional fora do modelo e das quatro portas. Escrita dinâmica não demonstrada: **[nao verificado]**. (`app/core/censo_escritas.py:469`, `app/chamados/models.py:296`, `app/chamados/services/texto.py:36`, `app/chamados/services/contexto.py:78`, `app/chamados/services/vinculos.py:89`, `app/chamados/services/arquivo.py:171`)

A catraca não vê `QuerySet.update/bulk_update`, chamada por alias de método (`salvar = ch.save; salvar()`), nem segue helpers externos. Outro módulo fica fora. Alias do **objeto**, como `outro.save()`, continua visível. (`app/chamados/tests/test_e1_v12.py:212`, `app/chamados/tests/test_e1_v12.py:231`, `app/chamados/tests/test_e1_v12.py:234`, `app/chamados/tests/test_e1_v12.py:242`)

Ela aceita uma chamada anterior à “ordem” sem conferir receptor, argumentos, ramo executado ou fluxo; `ast.walk` inclui funções internas e só trata `FunctionDef`. Portanto, uma trava em ramo morto ou sobre objeto errado pode liberar o save. (`app/chamados/tests/test_e1_v12.py:229`, `app/chamados/tests/test_e1_v12.py:231`, `app/chamados/tests/test_e1_v12.py:232`)

Na extração executada somente em memória, a árvore atual deu zero acusações; retirar as chamadas ao helper produziu **16 acusações**. Há mordida para a mudança local, mas ela não mede a ordem acumulada entre duas ressuscitações. O RED novo exercita apenas uma ressuscitação seguida de contexto no mesmo Chamado. (`app/chamados/tests/test_e1_v12.py:217`, `app/chamados/tests/test_e1_v12.py:137`, `app/chamados/tests/test_e1_v12.py:140`)

**6. Custo — PROCEDENTE a multiplicação; relevância operacional medida é [nao verificado].**

São duas consultas adicionais por helper executado dentro de atomic: C e Chamado. Reentrância não elimina essas consultas; fora de atomic ou em no-op anterior ao helper, esse acréscimo não ocorre. (`app/chamados/models.py:484`, `app/chamados/models.py:486`, `app/chamados/models.py:2547`, `app/chamados/models.py:2567`, `app/chamados/services/contexto.py:98`)

| Caminho | Multiplicação verificável |
|---|---|
| Escalonamento a cada cinco minutos | N2 chama escalar e anotar contexto no mesmo atomic: até **+4 por candidato alterado**. N1 chama marcar DP fora de atomic: +0 pelo helper; demais anotações acrescentam +2 quando escrevem. (`app/config/crons.py:369`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:51`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:103`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:131`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:138`) |
| Espelho do cartório | +2 por contexto alterado no laço de chamados vivos. (`app/ponto/services/cartorio.py:1787`, `app/ponto/management/commands/detectar_ausencias.py:558`) |
| Rejunção, no caminho da batida/outbox/vigia | +2 por vínculo efetivamente escrito. (`app/chamados/rejuncao.py:73`, `app/chamados/rejuncao.py:107`) |
| Agrupamento de furos | Trava superior de universo, mais duas consultas do helper de contexto, repetidas por furo. (`app/chamados/services/limbo_folgas.py:274`, `app/chamados/services/limbo_folgas.py:275`, `app/chamados/services/limbo_folgas.py:331`) |
| Cobrança | +2 por pai com SLA alterado no lote. (`app/chamados/services/cobranca.py:176`, `app/chamados/services/cobranca.py:178`) |

Os testes contêm asserts de **7 consultas na escalada** e **6 no vínculo**. Execução desses testes nesta revisão, volume real, contenção e latência: **[nao verificado]**. (`app/chamados/tests/test_porta_chamado_ciclo.py:109`, `app/chamados/tests/test_porta_chamado_ciclo.py:265`, `app/chamados/tests/test_porta_vinculos.py:77`, `app/chamados/tests/test_porta_vinculos.py:339`)

**7. Classes anteriores de reprovação.**

| Classe | Veredito neste recorte |
|---|---|
| Segundo juiz | **NÃO PROCEDE como introdução do delta:** o helper usa a porta existente. (`app/chamados/models.py:486`, `app/chamados/models.py:1552`) |
| Segundo escritor fora da porta | **NÃO PROCEDE como introdução do delta:** permanecem os saves nas portas; o helper só solicita travas. (`app/chamados/services/contexto.py:103`, `app/chamados/services/contexto.py:104`, `app/chamados/models.py:486`) |
| Relógio fora da borda | **NÃO PROCEDE como novidade do delta:** as leituras de relógio dessas portas permanecem anteriores ao helper. (`app/chamados/models.py:551`, `app/chamados/services/texto.py:34`) |
| Derivação paralela | **NÃO PROCEDE como novidade do delta:** a seleção da ressuscitação continua na mesma função. (`app/chamados/models.py:1533`) |
| Selo/RED sem mordida | **PROCEDENTE quanto ao alcance insuficiente; NÃO PROCEDE dizer que é vazio.** A catraca 

[truncado em 20000 de 20886 caracteres]

## 07/11 21:08 · sessao 50a9c5887032 · - · - · origem=Ronald (tmux)

**Q2 — PROCEDENTE o caminho; NÃO PROCEDE chamá-lo inversão de trava retida pelo UPDATE anterior.** `arquivar_chamado → arquivo.silenciar` não abre `atomic`; o helper retorna quando `in_atomic_block=False`. Se a instância traz status velho, o cartório compara status banco/instância, ignora `update_fields` e pode julgar depois do save. Em autocommit, a transação desse UPDATE já acabou; execução concreta e configuração efetiva de produção são **[nao verificado]**. (`app/chamados/views_arquivar.py:37`, `app/chamados/services/arquivo.py:170`, `app/chamados/models.py:484`, `app/chamados/signals.py:184`, `app/chamados/signals.py:191`, `app/chamados/signals.py:203`)

Texto/contexto/vínculos abrem `atomic` antes de chamar o helper; portanto “chamada externa à transação” não significa que seu save ocorra fora de transação. Os quatro métodos de campo do modelo e as duas portas de arquivo não abrem atomic próprio. (`app/chamados/services/texto.py:31`, `app/chamados/services/contexto.py:101`, `app/chamados/services/vinculos.py:86`, `app/chamados/models.py:488`, `app/chamados/models.py:513`, `app/chamados/models.py:573`, `app/chamados/models.py:594`, `app/chamados/services/arquivo.py:141`, `app/chamados/services/arquivo.py:177`)

**Q4 — mapa de escritores e leitores concretos:**

| Campo | Escritor | Leitor |
|---|---|---|
| `status_local` | `_gravar_estado` atribui e salva; rebaixa/restaura urgência junto. `app/chamados/models.py:228`, `app/chamados/models.py:246`, `app/chamados/models.py:250`, `app/chamados/models.py:296` | S143, cartório e C6. `app/chamados/signals.py:96`, `app/chamados/signals.py:191`, `app/chamados/juizes.py:758` |
| `contexto_json` do Chamado | Quatro portas de contexto. `app/chamados/services/contexto.py:76`, `app/chamados/services/contexto.py:102`, `app/chamados/services/contexto.py:125`, `app/chamados/services/contexto.py:161` | Juiz de dia/marcos/agrupamento e reconciliador de filhos. `app/chamados/juizes.py:100`, `app/chamados/juizes.py:534`, `app/chamados/juizes.py:1462`, `app/chamados/reconciliador.py:242` |
| `contexto` da Disputa | Remover motivo, anotar e renovar. `app/chamados/models.py:2032`, `app/chamados/models.py:2057`, `app/chamados/models.py:2097`, `app/chamados/models.py:2115` | Guardas das próprias portas. `app/chamados/models.py:2093`, `app/chamados/models.py:2111` |
| Justificativa, célula, ticket, responsável | Cinco portas de vínculos. `app/chamados/services/vinculos.py:87`, `app/chamados/services/vinculos.py:99`, `app/chamados/services/vinculos.py:115`, `app/chamados/services/vinculos.py:132`, `app/chamados/services/vinculos.py:144` | Reconciliação, juiz do dia, view GLPI e fila. `app/chamados/reconciliador.py:451`, `app/chamados/reconciliador.py:529`, `app/chamados/juizes.py:100`, `app/chamados/views.py:220`, `app/chamados/services/fila.py:154` |
| Arquivo/véu | `silenciado_ate/por/motivo` e `resgatado_em`, pelas portas de arquivo; arquivo não é choice de status. `app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:202`, `app/chamados/models.py:28`, `app/chamados/models.py:87` | Predicado temporal, dono do silêncio, receiver de isentos. `app/chamados/models.py:507`, `app/chamados/services/silencio_ausencia.py:66`, `app/chamados/services/silencio_ausencia.py:80`, `app/chamados/signals.py:278` |
| Urgência/original | Ritual de rebaixamento, escalar, transição e UPDATE do receiver. `app/chamados/models.py:468`, `app/chamados/models.py:498`, `app/chamados/models.py:553`, `app/chamados/models.py:246`, `app/chamados/models.py:636` | Guarda por `sla_limite_horas`. `app/chamados/models.py:546` |
| SLA | Porta e nascimento dos chamados de disputa. `app/chamados/models.py:588`, `app/chamados/services/disputa_emissao.py:581`, `app/chamados/services/disputa_emissao.py:1026`, `app/chamados/services/disputa_emissao.py:1253` | Juiz do prazo e cobrança. `app/chamados/catalogo/motor.py:54`, `app/chamados/catalogo/motor.py:68`, `app/chamados/services/cobranca.py:246` |
| Escalonado DP/em | Porta carimba após push. `app/chamados/models.py:606`, `app/chamados/models.py:610` | Comando lê flag; gestão lê instante. `app/ponto/management/commands/escalonar_chamados_supervisao.py:96`, `app/chamados/services/fila.py:181` |
| Materialização/autor/via da Pergunta | Atribuição central; portas carimbar, identidade, descarimbar e lote. `app/chamados/models.py:2852`, `app/chamados/models.py:2907`, `app/chamados/models.py:2937`, `app/chamados/models.py:2963`, `app/chamados/models.py:3015` | Estado, memo C6 e seleção da materialização/S143. `app/chamados/models.py:2796`, `app/chamados/juizes.py:827`, `app/chamados/services/materializacao.py:434`, `app/chamados/services/disputa_emissao.py:1711` |
| Resposta/respondida/validação/autor | Resposta, validação, devolução e ressuscitação. `app/chamados/services/disputa_emissao.py:1368`, `app/chamados/services/disputa_emissao.py:1381`, `app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:199`, `app/chamados/models.py:1555` | Estado e memo do C6; ressuscitação registra projeção antes de apagar. `app/chamados/models.py:2796`, `app/chamados/juizes.py:827`, `app/chamados/models.py:1553` |
| `fechada_em` da Disputa | Fechar, reabrir admin, reabrir sistema. `app/chamados/models.py:1924`, `app/chamados/models.py:2141`, `app/chamados/models.py:2165` | S88, retomada, S144, ressuscitação e universo de travas. `app/chamados/signals.py:28`, `app/chamados/signals.py:129`, `app/chamados/signals.py:153`, `app/chamados/models.py:1582`, `app/chamados/models.py:2552` |

**NÃO PROCEDE alteração explícita dos valores no delta 12b/12c:** as chamadas novas precedem as atribuições persistidas pelos mesmos `update_fields`; helper só adquire travas. Porém a volta inteira inclui mudança de disparo do S143; afirmar “só ordem” para `afb..HEAD` seria falso. (`app/chamados/models.py:475`, `app/chamados/models.py:497`, `app/chamados/models.py:1552`, `app/chamados/services/contexto.py:103`, `app/chamados/signals.py:104`)

**Limite concorrente preservado:** contexto/vínculos continuam lendo a instância e decidindo antes da trava, sem releitura; a adição da trava não demonstra cura de sobrescrita por fotografia velha. (`app/chamados/services/contexto.py:97`, `app/chamados/services/contexto.py:103`, `app/chamados/services/vinculos.py:84`, `app/chamados/services/vinculos.py:88`)

## 07/11 21:07 · sessao 92e78a3613dc · - · - · origem=Ronald (tmux)

**PROCEDENTE: a volta 12c corrige a cadeia unitária, mas não demonstra a ordem global durante todo o atomic externo.** Há contraexemplo estático por duas ressuscitações do mesmo colaborador, sem depender de duplicados no mesmo dia. (`app/chamados/models.py:1552`, `app/ponto/services/cartorio.py:1303`, `app/ponto/services/cartorio.py:1622`)

Cadeia:

1. `celula_veredito_velho` abre **um atomic envolvendo a janela inteira**; carrega várias células do mesmo colaborador e chama `julgar_colab`. (`app/ponto/management/commands/celula_veredito_velho.py:133`, `app/ponto/management/commands/celula_veredito_velho.py:139`, `app/ponto/management/commands/celula_veredito_velho.py:141`, `app/ponto/management/commands/celula_veredito_velho.py:144`)
2. `julgar_colab → _julgar_colab_corpo` percorre as células e chama `ressuscitar_perguntas` para o chamado elegível. (`app/ponto/services/cartorio.py:948`, `app/ponto/services/cartorio.py:1303`, `app/ponto/services/cartorio.py:1591`, `app/ponto/services/cartorio.py:1622`)
3. Na primeira célula: `C → D1 → CH1 → P1`. O topo da ressuscitação pede somente suas disputas, `self.pk` e seus IDs de perguntas; não pede `abertas_do_colaborador=True`. (`app/chamados/models.py:1541`, `app/chamados/models.py:1552`)
4. Na segunda célula elegível do mesmo C, com D2/CH2 distintos: C é reentrante, **D2 e CH2 são aquisições novas depois de P1**, na mesma transação externa. A implementação não amplia o universo sem a flag. (`app/chamados/models.py:1552`, `app/chamados/models.py:2548`, `app/chamados/models.py:2551`, `app/chamados/models.py:2560`)

O caminho pressupõe duas ressuscitações elegíveis e ausência de uma trava anterior que já cobrisse D2/CH2; os reconciliadores anteriores são condicionais, portanto não constituem pré-requisito incondicional de aquisição desse universo. Estado correspondente na frota e deadlock observado: **[nao verificado]**. (`app/ponto/services/cartorio.py:1488`, `app/ponto/services/cartorio.py:1501`, `app/ponto/services/cartorio.py:1514`, `app/ponto/services/cartorio.py:1522`)

Também há continuação potencial pela fábrica: após o corpo/ressuscitações, `julgar_colab` chama a fábrica, que pode chegar à emissão e adquirir o universo aberto do colaborador — linhas que a ressuscitação não necessariamente adquiriu. Elegibilidade concreta dessa continuação: **[nao verificado]**. (`app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:184`, `app/chamados/services/fabrica_da_celula.py:186`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/disputa_emissao.py:530`)

**Não confundir violação de rank com deadlock demonstrado:** no contraexemplo a trava C já está retida; outra transação que também obedeça C primeiro será serializada. O fato demonstrável é aquisição nova D2/CH2 após P1, contrariando a ordem global expressa e o critério do rastreador; ciclo concorrente efetivo é **[nao verificado]**. (`app/chamados/models.py:2547`, `app/chamados/tests/test_e1_v9.py:94`, `app/chamados/tests/test_e1_v9.py:106`)

**NAO PROCEDE como inversão nova a cadeia unitária corrigida:** uma ressuscitação sob atomic externo retém C, suas DS, seu Chamado e suas Perguntas. Reabrir as mesmas DS depois do atomic interno é reentrante; o SELECT de reabertura usa os mesmos IDs recolhidos antes. (`app/chamados/models.py:1541`, `app/chamados/models.py:1552`, `app/chamados/models.py:1582`, `app/chamados/models.py:1583`)

Os receivers de fechamento da Disputa retornam quando a instância está aberta; reabrir não aciona reconciliação S88 nem materialização S144. A nota de `reabrir_sistema` cria Followup e não passa contexto novo para salvar o Chamado. (`app/chamados/signals.py:28`, `app/chamados/signals.py:129`, `app/chamados/signals.py:153`, `app/chamados/models.py:2169`, `app/chamados/models.py:2172`, `app/chamados/services/nota.py:103`, `app/chamados/services/nota.py:110`)

**PROCEDENTE a janela sem atomic externo; NAO PROCEDE chamá-la novidade do delta:** o bloco que altera Perguntas termina antes do log e da reabertura das Disputas. Assim, pode haver estado intermediário confirmado “Pergunta ressuscitada / Disputa ainda fechada”; a adição da trava não estende a transação até a reabertura. O diff fornecido acrescenta a trava, mantendo essa fronteira. Ocorrência concorrente real é **[nao verificado]**. (`app/chamados/models.py:1547`, `app/chamados/models.py:1555`, `app/chamados/models.py:1559`, `app/chamados/models.py:1582`; delta anexado `codex_e1_v14.diff` **[nao verificado como arquivo local]**)

Os IDs e as disputas também são coletados **antes** de adquirir C e as linhas; a porta não refaz o julgamento do universo depois de esperar pelas travas. Portanto, a nova trava não prova ausência de seleção envelhecida. Entrelaçamento efetivo e alteração incorreta observada são **[nao verificado]**. (`app/chamados/models.py:1533`, `app/chamados/models.py:1541`, `app/chamados/models.py:1552`, `app/chamados/models.py:1555`)

**NAO PROCEDE repetir a inversão antiga no `agrupar_furo` unitário:** agora o topo adquire C, abertas, pais/vivos e furo/pai antes de contexto, reapontamento e reconciliação. O reapontamento pede C e o destino antes de salvar P; essas linhas já estão cobertas no caso corrigido. (`app/chamados/services/limbo_folgas.py:274`, `app/chamados/services/limbo_folgas.py:275`, `app/chamados/services/limbo_folgas.py:282`, `app/chamados/services/limbo_folgas.py:283`, `app/chamados/services/fio.py:855`, `app/chamados/services/fio.py:858`, `app/chamados/services/fio.py:768`, `app/chamados/services/fio.py:870`)

## 07/11 21:07 · sessao 155a5efe7473 · - · - · origem=Ronald (tmux)

**5. PROCEDENTE o alcance limitado; não localizei escritor produtivo adicional de `ChamadoColaborador.save()`.** O censo AST da árvore, executado somente em memória, retorna os cinco saves do modelo e os doze das quatro portas; sua implementação varre código produtivo, inclusive `management/`. A busca adicional pelos saves em módulos que referenciam o modelo não encontrou outro escritor de Chamado. Escrita dinâmica não demonstrada: **[nao verificado]**. (`app/core/censo_escritas.py:469`, `app/core/censo_escritas.py:54`, `app/chamados/models.py:296`, `app/chamados/services/texto.py:36`, `app/chamados/services/contexto.py:78`, `app/chamados/services/vinculos.py:89`, `app/chamados/services/arquivo.py:171`)

A catraca:

- Examina apenas quatro arquivos e `ChamadoColaborador` no modelo; outro módulo fica fora. (`app/chamados/tests/test_e1_v12.py:212`, `app/chamados/tests/test_e1_v12.py:242`)
- Só reconhece `Call` com `Attribute` literal `save`; `QuerySet.update`, `bulk_update`, `salvar = ch.save; salvar()` ficam fora. Alias do objeto, como `outro.save()`, continua visível. (`app/chamados/tests/test_e1_v12.py:231`, `app/chamados/tests/test_e1_v12.py:234`)
- Não segue helper; se o helper estiver fora dos arquivos/classe examinados, seu save desaparece desta catraca. (`app/chamados/tests/test_e1_v12.py:242`)
- Aceita chamada anterior à ordem independentemente de receptor, argumentos, ramo executado e domínio do fluxo; `ast.walk(fn)` também inclui funções internas. Só trata `FunctionDef`, não `AsyncFunctionDef`. (`app/chamados/tests/test_e1_v12.py:229`, `app/chamados/tests/test_e1_v12.py:231`)
- O `update` conhecido de `_rebaixar_sem_post_save` permanece no próprio modelo, sobre o mesmo PK, chamado pelo receiver. (`app/chamados/models.py:625`, `app/chamados/models.py:636`)

**6. PROCEDENTE a multiplicação linear de consultas; relevância operacional medida na frota é [nao verificado].** O helper acrescenta duas consultas quando realmente chamado dentro de atomic: Colaborador e Chamado; não consulta Disputa sem parâmetros/universo. Fora de atomic retorna; guardas idempotentes das portas podem retornar antes dele. (`app/chamados/models.py:484`, `app/chamados/models.py:486`, `app/chamados/models.py:2547`, `app/chamados/models.py:2553`, `app/chamados/models.py:2567`, `app/chamados/services/contexto.py:98`)

| Caminho | Multiplicação verificável |
|---|---|
| Cron de escalonamento, a cada cinco minutos | Laço sobre candidatos; N2 faz `escalar_urgencia` e `anotar_contexto` no mesmo atomic: até **+4** por candidato alterado. N1 chama `marcar_escalonado_dp` fora de atomic: **+0** pelo helper. Outros carimbos de contexto: **+2** cada alteração. (`app/config/crons.py:369`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:51`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:103`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:131`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:138`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:163`) |
| Cartório por empresa | Laço de colaboradores e, depois, chamados vivos: **+2** por contexto realmente modificado pelo espelho. (`app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/services/cartorio.py:1787`, `app/ponto/management/commands/detectar_ausencias.py:558`) |
| Rejunção no caminho da batida/outbox/vigia | **+2** por vínculo de célula modificado; o próprio código identifica a frequência por linha deste laço. (`app/chamados/rejuncao.py:73`, `app/chamados/rejuncao.py:107`) |
| Agrupamento de furos | Laço de furos; trava superior de universo pode fazer três consultas, seguida das duas do helper de contexto. Reentrância não elimina a nova consulta. (`app/chamados/services/limbo_folgas.py:274`, `app/chamados/services/limbo_folgas.py:275`, `app/chamados/services/limbo_folgas.py:316`, `app/chamados/services/limbo_folgas.py:331`) |
| Cobrança | **+2** por chamado pai com SLA alterado no atomic do lote. (`app/chamados/services/cobranca.py:176`, `app/chamados/services/cobranca.py:178`) |

Os números declarados no diff estão presos por verificações efetivas de contagem: escalada **7**, vínculo **6**. Execução desses testes com banco nesta revisão: **[nao verificado]**. (`app/chamados/tests/test_porta_chamado_ciclo.py:109`, `app/chamados/tests/test_porta_chamado_ciclo.py:265`, `app/chamados/tests/test_porta_vinculos.py:77`, `app/chamados/tests/test_porta_vinculos.py:339`)

## 07/11 21:00 · sessao 72546ee75f51 · FIO-E0-V4-E1 · censo · origem=Code

O censo abaixo distingue caminhos da cópia; ocorrência efetiva desses caminhos em produção é **[nao verificado]**. Os perfis locais não configuram `ATOMIC_REQUESTS`; configuração efetivamente carregada em produção é **[nao verificado]**. — `app/config/settings/saas.py:83`, `app/config/settings/prod.py:9`

Em **(b)**, “sim” significa transação já aberta **antes de entrar na porta**. O atomic interno da porta pode, mesmo com (b)=não, manter a escrita e seus receivers na mesma transação. — `app/chamados/services/contexto.py:100`, `app/chamados/services/texto.py:31`, `app/chamados/services/vinculos.py:86`

A notação **R** significa **(e)=sim, condicionalmente pelo receiver**: o status lido do banco difere do status da instância; as guardas permitem julgamento e a fábrica chega à trava de Colaborador/Disputa. A ocorrência é **[nao verificado]**, mas o caminho existe no código. O bloqueio do S143 para saves de campo não elimina esse outro receiver. — `app/chamados/signals.py:100`, `app/chamados/signals.py:180`, `app/chamados/signals.py:188`, `app/chamados/signals.py:193`, `app/chamados/signals.py:199`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/disputa_emissao.py:530`

**R anterior** significa que uma chamada/iteração anterior completou esse caminho na mesma transação: (c)=sim pela disputa travada/escrita; (d)=sim somente se o Colaborador travado foi o mesmo. Callbacks `on_commit` ficam fora da transação anterior. — `app/chamados/services/disputa_emissao.py:530`, `app/chamados/services/disputa_emissao.py:565`, `app/chamados/services/disputa_emissao.py:587`, `app/chamados/signals.py:221`, `app/chamados/signals.py:289`

| (a) Chamada — função chamadora | (b) Transação anterior | (c) PD/DS anterior | (d) Colaborador anterior | (e) Trava posterior na mesma transação |
|---|---|---|---|---|
| `app/api/views.py:2033` — `api_justificativas` → `apontar_justificativa` | Não | Não; J salva antes, sem atomic envolvendo ambos | Não | **Sim, R**; J pode desatualizar a instância carregada antes. — `app/api/views.py:1986`, `app/api/views.py:2021`, `app/ponto/signals.py:165` |
| `app/api/views.py:3159` — `api_ausencia_documento` → `renovar_contexto` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:100` |
| `app/api/views.py:3160` — `api_ausencia_documento` → `reescrever_texto` | Não | Não; atomic da chamada anterior terminou | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/texto.py:31` |
| `app/ponto/views.py:288` — `bater_ponto` → `renovar_contexto` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:100` |
| `app/ponto/views.py:1648` — `_abrir_justificativa_fluxo` → `apontar_justificativa` | Não | Não; J salva antes, sem atomic envolvendo ambos | Não | **Sim, R**; CC foi carregado antes do signal de J. — `app/ponto/views.py:1615`, `app/ponto/views.py:1633`, `app/ponto/signals.py:165` |
| `app/chamados/views.py:1025` — `atribuir_chamado` → `liberar_responsavel` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/vinculos.py:139` |
| `app/chamados/views.py:1029` — `atribuir_chamado` → `assumir_responsavel` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/vinculos.py:128` |
| `app/chamados/views_arquivar.py:37` — `arquivar_chamado` → `silenciar` | Não | Não | Não | Não na mesma transação; porta sem atomic interno. — `app/chamados/services/arquivo.py:170` |
| `app/chamados/views_arquivar.py:77` — `desarquivar_chamado` → `levantar` | Não | Não; eventual `reabrir` anterior já retornou | Não na mesma transação | Não na mesma transação; ressuscitação posterior também está fora. — `app/chamados/views_arquivar.py:70`, `app/chamados/views_arquivar.py:86`, `app/chamados/services/arquivo.py:208` |
| `app/chamados/views_arquivar.py:140` — `arquivar_lote` → `silenciar` | **Sim**, atomic `app/chamados/views_arquivar.py:134` | Não na primeira passagem; **sim se R anterior** no laço | Não inicialmente; sim se R anterior do mesmo colaborador | **Sim, R**. — `app/chamados/views_arquivar.py:135` |
| `app/ponto/services/triagem_batida.py:292` — `ramo_regularizar_saida` → `escalar_urgencia` | Não | Não; `update_or_create` de J já terminou | Não | Não na mesma transação. — `app/ponto/services/triagem_batida.py:258`, `app/chamados/models.py:538` |
| `app/ponto/services/triagem_batida.py:369` — `ramo_solicitar_anuencia` → `escalar_urgencia` | **Sim**, atomic `app/ponto/services/triagem_batida.py:368` | Não; primeiro ato do bloco | Não | **Sim, R** |
| `app/ponto/services/triagem_batida.py:371` — `ramo_solicitar_anuencia` → alias `_renovar_contexto` | **Sim**, atomic `app/ponto/services/triagem_batida.py:368` | Não; **sim se R anterior em `app/ponto/services/triagem_batida.py:369`** | Não; sim nesse R anterior | **Sim, R**. — `app/ponto/services/triagem_batida.py:367` |

| (a) Chamada — função chamadora | (b) Transação anterior | (c) PD/DS anterior | (d) Colaborador anterior | (e) Trava posterior na mesma transação |
|---|---|---|---|---|
| `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:94` — `Command.handle` → `silenciar` | Não | Não | Não | Não; porta sem atomic interno. — `app/chamados/services/arquivo.py:170` |
| `app/chamados/management/commands/arquivar_competencia_encerrada.py:288` — `Command._aplicar` → `silenciar` | **Sim**, atomic `app/chamados/management/commands/arquivar_competencia_encerrada.py:285` | Não | Não | **Sim, R** |
| `app/chamados/management/commands/arquivar_prescritos.py:94` — `Command.handle` → `silenciar` | **Sim**, atomic `app/chamados/management/commands/arquivar_prescritos.py:91` | Não inicialmente; **sim se R anterior** no laço | Não inicialmente; sim se R anterior do mesmo colaborador | **Sim, R**. — `app/chamados/management/commands/arquivar_prescritos.py:92` |
| `app/chamados/management/commands/nomear_marco_por_lampada.py:109` — `Command.handle` → `renovar_contexto` | **Sim**, atomic por item `app/chamados/management/commands/nomear_marco_por_lampada.py:107` | Não | Não | **Sim, R** |
| `app/chamados/management/commands/carimbar_por_batida_coincidente.py:98` — `Command.handle` → `renovar_contexto` | **Sim**, atomic `app/chamados/management/commands/carimbar_por_batida_coincidente.py:97` | Não | Não | **Sim, R** |
| `app/chamados/management/commands/carimbar_por_batida_coincidente.py:100` — `Command.handle` → `apontar_celula` | **Sim**, mesmo atomic | Não; **sim se R anterior em `app/chamados/management/commands/carimbar_por_batida_coincidente.py:98`** | Não; sim nesse R anterior | **Sim, R** |
| `app/ponto/management/commands/reconciliar_grade.py:454` — `Command._relavrar_orfa` → `retirar_contexto` | Não; antes do atomic posterior | Não | Não | **Sim, R**, sob atomic interno. — `app/ponto/management/commands/reconciliar_grade.py:472`, `app/chamados/services/contexto.py:122` |
| `app/ponto/management/commands/reconciliar_grade.py:473` — `Command._relavrar_orfa` → `renovar_contexto` | **Sim**, atomic `app/ponto/management/commands/reconciliar_grade.py:472` | Não | Não | **Sim, R** |
| `app/ponto/management/commands/reconciliar_grade.py:474` — `Command._relavrar_orfa` → `retirar_contexto` | **Sim**, mesmo atomic | Não; **sim se R anterior em `app/ponto/management/commands/reconciliar_grade.py:473`** | Não; sim nesse R anterior | **Sim, R** |
| `app/ponto/management/commands/curar_residuo_bug115.py:121` — `Command._ambiguo` → `renovar_contexto` | **Sim**, atomic `app/ponto/management/commands/curar_residuo_bug115.py:120` | Não | Não | **Sim, R** |
| `app/ponto/management/commands/processar_alertas_avancados.py:173` — `Command.handle` → `escalar_urgencia` | Não | Não | Não | Não na mesma transação; método sem atomic interno. — `app/chamados/models.py:538` |
| `app/ponto/management/commands/processar_alertas_turno.py:423` — `Command._abrir_justificativa_automatica` → `apontar_justificativa` | Não | Não; atomic de `get_or_create` já terminou | Não | **Sim, R**, sob atomic interno. — `app/ponto/management/commands/processar_alertas_turno.py:391`, `app/chamados/services/vinculos.py:86` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:64` — `Command.handle` → `anotar_contexto` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:75` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:103` — `Command.handle` → `marcar_escalonado_dp` | Não | Não | Não | Não na mesma transação; método sem atomic interno. — `app/chamados/models.py:593` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:132` — `Command.handle` → `escalar_urgencia` | **Sim**, atomic `app/ponto/management/commands/escalonar_chamados_supervisao.py:131` | Não | Não | **Sim, R** |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:138` — `Command.handle` → `anotar_contexto` | **Sim**, mesmo atomic | Não; **sim se R anterior em `app/ponto/management/commands/escalonar_chamados_supervisao.py:132`** | Não; sim nesse R anterior | **Sim, R** |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:163` — `Command.handle` → `anotar_contexto` | Não | Não na mesma transação; anotação da DS anterior já retornou | Não | **Sim, R**. — `app/ponto/management/commands/escalonar_chamados_supervisao.py:159`, `app/chamados/services/contexto.py:75` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:179` — `Command.handle` → `anotar_contexto` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:75` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:195` — `Command.handle` → `anotar_contexto` | Não | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:75` |

Os três sítios seguintes precisam de variantes: são funções também chamadas por outros módulos. **P anterior** abaixo significa ressuscitação ou reconciliação anterior do cartório: UPDATE de PD, save de DS ou trava de DS pela porta do estado. — `app/ponto/services/cartorio.py:1622`, `app/chamados/models.py:1533`, `app/chamados/models.py:2994`, `app/chamados/models.py:1561`, `app/chamados/models.py:2147`, `app/ponto/services/cartorio.py:1488`, `app/chamados/models.py:694`

| (a) Chamada — função/caminho | (b) Transação anterior | (c) PD/DS anterior | (d) Colaborador anterior | (e) Trava posterior na mesma transação |
|---|---|---|---|---|
| `app/ponto/management/commands/gerar_celulas.py:255` — `_marcar_cobrancas_orfas`, regeneração/edição isolada | **Sim**, atomic da regeneração/edição | Não inicialmente; **sim se R anterior** no laço | Não inicialmente; sim nesse R anterior | **Sim, R**; o julgamento normal posterior fica fora do atomic próprio. — `app/ponto/portas/celula.py:218`, `app/ponto/portas/celula.py:289`, `app/ponto/portas/celula.py:592`, `app/ponto/portas/celula.py:626`, `app/ponto/portas/celula.py:354` |
| `app/ponto/management/commands/gerar_celulas.py:255` — primeiro receiver de âncora durante `executar_vinculo` | **Sim**, atomic externo | Não no caminho sem atos anteriores; sim se R/P anterior | Não nesse caminho inicial | **Sim**: receiver posterior chama `remover_motivo`, que trava C e DS; também pode haver julgamento no rabo. — `app/colaboradores/services/vinculo.py:187`, `app/colaboradores/services/vinculo.py:274`, `app/escala/signals.py:117`, `app/escala/signals.py:161`, `app/chamados/models.py:1996`, `app/chamados/models.py:1998`, `app/ponto/portas/celula.py:434` |
| `app/ponto/management/commands/gerar_celulas.py:255` — foto de folga após receiver de âncora/remover motivo | **Sim**, mesmo atomic externo | **Sim**, quando aquele receiver atuou: PD carimbada e DS salva antes | **Sim**, pelo receiver anterior | **Sim/pode**, pelo julgamento posterior. — `app/colaboradores/services/vinculo.py:275`, `app/colaboradores/services/vinculo.py:163`, `app/escala/folgas.py:272`, `app/chamados/models.py:2031`, `app/chamados/models.py:2035`, `app/chamados/models.py:1996`, `app/ponto/portas/celula.py:434` |
| `app/ponto/management/commands/detectar_ausencias.py:558` — `_espelhar_marcos`, comando sem atomic externo | Não | Não na mesma transação | Não na mesma transação | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:100` |
| `app/ponto/management/commands/detectar_ausencias.py:558` — via `_abrir_chamado`, sob atomic externo do cartório | **Sim**, herdado | Não sem P anterior; **sim com P anterior** | **Sim**, `Chamado.abrir` anterior | **Sim/pode**, pelo fio posterior ou fábrica. — `app/ponto/management/commands/detectar_ausencias.py:693`, `app/chamados/models.py:888`, `app/ponto/management/commands/detectar_ausencias.py:744`, `app/ponto/management/commands/detectar_ausencias.py:801`, `app/chamados/services/disputa_emissao.py:968`, `app/ponto/services/cartorio.py:997` |
| `app/ponto/management/commands/detectar_ausencias.py:558` — chamada direta do cartório, sob atomic externo | **Sim**, por exemplo o comando de veredito velho | Não sem P anterior; **sim com P anterior** | Não sem trava anterior; **sim** se houve reconciliação ou wrapper que trava C | **Sim/pode**, pela fábrica posterior. — `app/ponto/services/cartorio.py:1789`, `app/ponto/services/cartorio.py:1798`, `app/ponto/management/commands/celula_veredito_velho.py:133`, `app/ponto/management/commands/celula_veredito_velho.py:144`, `app/ponto/services/veredito_celula.py:225`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/disputa_emissao.py:530` |
| `app/ponto/management/commands/detectar_ausencias.py:765` — `_abrir_chamado`, comando sem atomic externo | Não | Não na mesma transação | Não na mesma transação | **Sim, R**, sob atomic interno. — `app/chamados/services/contexto.py:100` |
| `app/ponto/management/commands/detectar_ausencias.py:765` — `_abrir_chamado`, sob atomic externo | **Sim**, herdado | Não sem P anterior; **sim com P anterior** | **Sim**, `Chamado.abrir` anterior | **Sim/pode**, pelo fio/fábrica posterior. — `app/ponto/management/commands/detectar_ausencias.py:693`, `app/chamados/models.py:888`, `app/ponto/management/commands/detectar_ausencias.py:801`, `app/chamados/services/disputa_emissao.py:968`, `app/ponto/services/cartorio.py:997` |

| (a) Chamada — função chamadora | (b) Transação anterior | (c) PD/DS anterior | (d) Colaborador anterior | (e) Trava posterior na mesma transação |
|---|---|---|---|---|
| `app/chamados/services/abertura.py:115` — `abrir_chamado_local` → `apontar_ticket_glpi` | Não no caminho da view | Não | Não | **Sim, R**. — `app/chamados/views.py:197`, `app/chamados/services/vinculos.py:112` |
| `app/chamados/services/abertura.py:116` — `abrir_chamado_local` → `renovar_contexto` | Não no caminho da view | Não; transação anterior terminou | Não | **Sim, R**. — `app/chamados/views.py:197`, `app/chamados/services/contexto.py:100` |
| `app/chamados/services/adesao.py:112` — `sincronizar_adesao` → `renovar_contexto` | Não no comando chamador | Não na mesma transação | Não; `abrir/reabrir` anteriores já retornaram | **Sim, R**; `reconciliar` seguinte tem transação própria. — `app/escala/management/commands/apurar_furos_diarios.py:17`, `app/chamados/services/adesao.py:74`, `app/chamados/services/adesao.py:92`, `app/chamados/services/adesao.py:115` |
| `app/chamados/services/celula_carimbada.py:109` — `backfill` → `apontar_celula` | Não no corpo; transação de entrada **[nao verificado]** | Não no corpo; contexto externo **[nao verificado]** | Não no corpo; contexto externo **[nao verificado]** | **Sim, R**, se executado; entrada de produção **[nao verificado]**. — `app/chamados/services/celula_carimbada.py:74`, `app/chamados/services/vinculos.py:97` |
| `app/chamados/services/declaracao_texto.py:202` — `declarar_nao_trabalhou` → `renovar_contexto` | Não no caminho da view | Não | Não | **Sim, R**. — `app/chamados/views.py:1066`, `app/chamados/services/contexto.py:100` |
| `app/chamados/services/cobranca.py:178` — `cobrar_colaborador` → `definir_prazo_sla` | **Sim**, atomic `app/chamados/services/cobranca.py:176` | Não inicialmente; **sim se R anterior** no laço | Não inicialmente; sim se R anterior do mesmo colaborador | **Sim, R**. — `app/chamados/services/cobranca.py:177` |
| `app/chamados/services/disputa_emissao.py:1604` — `recobrar_questionario` → `definir_prazo_sla` | Não no caminho da view | Não | Não | Não na mesma transação. — `app/chamados/views.py:1425`, `app/chamados/models.py:575` |
| `app/chamados/services/silencio_isentos.py:43` — `silenciar` → `arquivo.silenciar` | **Sim**, atomic `app/chamados/services/silencio_isentos.py:41` | Não inicialmente; **sim se R anterior** no laço | Não inicialmente; sim se R anterior do mesmo colaborador | **Sim, R**. — `app/chamados/services/silencio_isentos.py:42` |
| `app/chamados/services/silencio_ausencia.py:86` — `reconciliar_por_ausencia` → `silenciar` | Não no comando/callback de entrada | Não | Não | Não na mesma transação. — `app/ponto/management/commands/reavaliar_ausencias_lancadas.py:54`, `app/ponto/signals.py:272`, `app/chamados/services/arquivo.py:170` |
| `app/chamados/services/silencio_ausencia.py:72` — closure `_levantar` → `levantar` | Não no comando/callback de entrada | Não na mesma transação; eventual reconciliação anterior já terminou | Não na mesma transação | Não na mesma transação. — `app/chamados/services/silencio_ausencia.py:101`, `app/chamados/services/silencio_ausencia.py:107`, `app/ponto/signals.py:272`, `app/chamados/services/arquivo.py:208` |
| `app/chamados/services/limbo_folgas.py:67` — `aplicar_sugestao` → `renovar_contexto` | Não no comando chamador | Não | Não | **Sim, R**. — `app/ponto/management/commands/detectar_ausencias.py:236`, `app/chamados/services/contexto.py:100` |
| `app/chamados/services/limbo_folgas.py:69` — `aplicar_sugestao` → `reescrever_texto` | Não no comando chamador | Não; atomic anterior terminou | Não | **Sim, R**. — `app/ponto/management/commands/detectar_ausencias.py:236`, `app/chamados/services/texto.py:31` |
| `app/chamados/services/limbo_folgas.py:270` — `agrupar_furo`, sem atomic externo | **Sim**, atomic local `app/chamados/services/limbo_folgas.py:269` | Não; primeiro ato do bloco | Não | **Sim**: reapontamento/reconciliação posteriores. — `app/chamados/services/limbo_folgas.py:277`, `app/chamados/services/fio.py:855`, `app/chamados/services/limbo_folgas.py:278`, `app/chamados/models.py:290` |
| `app/chamados/services/limbo_folgas.py:270` — `agrupar_furo`, herdando atomic por `_abrir_chamado` | **Sim**, herdado e local | Não sem P/R anterior; **sim com P anterior ou R anterior de `_espelhar_marcos`** | **Sim**, `Chamado.abrir` anterior | **Sim**, pelos mesmos atos posteriores. — `app/ponto/management/commands/detectar_ausencias.py:693`, `app/chamados/models.py:888`, `app/ponto/management/commands/detectar_ausencias.py:744`, `app/ponto/management/commands/detectar_ausencias.py:761`, `app/chamados/services/limbo_folgas.py:277` |
| `app/chamados/rejuncao.py:107` — `rejuntar_celula_dos_vivos` → `apontar_celula` | Não no consumidor; regeneração entra por `on_commit` | Não | Não | **Sim, R**, sob atomic interno. — `app/chamados/reconciliador.py:1354`, `app/chamados/reconciliador.py:1378`, `app/ponto/portas/celula.py:498`, `app/ponto/portas/celula.py:502`, `app/chamados/services/vinculos.py:97` |
| `app/chamados/services/materializacao.py:715` — `_ressuscitar_por_resposta`, respostas diretas/correção administrativa | Não; atomic da resposta termina antes | Não na mesma transação | Não na mesma transação | Não na mesma transação. — `app/chamados/services/disputa_emissao.py:1388`, `app/chamados/services/disputa_emissao.py:1418`, `app/chamados/views.py:1283`, `app/api/views.py:2659`, `app/chamados/services/acoes_disputa.py:368` |
| `app/chamados/services/materializacao.py:715` — `_ressuscitar_por_resposta`, declarações com atomic externo | **Sim**, atomic da declaração | **Sim**, save anterior de PD | **Sim**, trava anterior da emissão | **Sim, R**; validação de `aplicar

[truncado em 20000 de 29894 caracteres]

## 07/11 20:56 · sessao 00393fa21d8e · - · - · origem=Ronald (tmux)

Concordo: **(e)=sim condicional** é a leitura estrita de “pode travar”, desde que se explicite a condição e não se afirme ocorrência em produção. O `pre_save` lê o status atual do banco, mas o `post_save` compara esse valor com o status da instância; saves de campo não renovam esse status em memória. (`app/chamados/signals.py:180`, `app/chamados/signals.py:188`; `app/chamados/services/contexto.py:102`)

Se houver divergência e as demais guardas permitirem julgamento/emissão, o receiver chega à fábrica e à trava de Colaborador/Disputa; sob atomic interno ou externo, isso ocorre com a escrita de CC ainda na mesma transação. (`app/chamados/signals.py:199`; `app/ponto/services/cartorio.py:997`; `app/chamados/services/disputa_emissao.py:530`; `app/chamados/services/texto.py:31`; `app/chamados/services/contexto.py:100`; `app/chamados/services/vinculos.py:86`)

Correção do meu recorte:

- **e=sim condicional:** `app/api/views.py:2033`, `:3159`, `:3160`; `app/ponto/views.py:288`, `:1648`; `app/chamados/views.py:1025`, `:1029`; `app/ponto/services/triagem_batida.py:369`, `:371`; `app/chamados/views_arquivar.py:140`. A condição é o caminho do receiver acima; os dois últimos blocos têm atomic externo em `app/ponto/services/triagem_batida.py:368` e `app/chamados/views_arquivar.py:134`.
- **e=não na mesma transação:** `app/chamados/views_arquivar.py:37`, `:77` e `app/ponto/services/triagem_batida.py:292`, porque estes caminhos não têm atomic externo nem atomic interno nessas portas. (`app/chamados/services/arquivo.py:170`, `:208`; `app/chamados/models.py:538`)
- **Propagação anterior:** em `app/ponto/services/triagem_batida.py:371`, se o save de `:369` percorreu esse caminho, (c) e (d) passam a sim; ambos estão sob `:368`. No lote `app/chamados/views_arquivar.py:140`, iteração anterior pode igualmente fornecer (c)=sim, e (d)=sim quando for o mesmo colaborador, sob `:134`.

Há inclusive mecanismo local para instância desatualizada nas duas views de justificativa: CC é carregado antes do save de J, cujo receiver julga o dia antes da porta de vínculo; a instância original não é recarregada nesse intervalo. Isso demonstra possibilidade pelo código, sem demonstrar ocorrência. (`app/api/views.py:1986`, `:2021`, `:2033`; `app/ponto/views.py:1615`, `:1633`, `:1648`; `app/ponto/signals.py:165`)

## 07/11 20:55 · sessao 130dcd99d6c6 · - · - · origem=Ronald (tmux)

Confirmada a classificação proposta para os três contextos de `nota._fundir_contexto`. app/chamados/services/nota.py:123.

- Via `disputa_emissao.py:586`: b=sim; c=não localmente, salvo atos anteriores herdados; d=sim; e=não sem atomic externo, podendo ser sim nas declarações externas. app/chamados/services/disputa_emissao.py:521; app/chamados/services/disputa_emissao.py:530; app/chamados/signals.py:28; app/chamados/services/declaracao_texto.py:346; app/chamados/services/declaracao_texto.py:634.
- Via `disputa_emissao.py:1031`: b=sim; c=pode ser sim pelo complemento anterior e pelo save da disputa; d=sim; e=sim pelas posteriores aberturas de perguntas ou chamadas a `analisar`. app/chamados/services/disputa_emissao.py:964; app/chamados/services/disputa_emissao.py:968; app/chamados/services/disputa_emissao.py:990; app/chamados/services/disputa_emissao.py:993; app/chamados/services/disputa_emissao.py:1062; app/chamados/services/disputa_emissao.py:1068.
- Via `disputa_emissao.py:1258`: b=sim; c=não localmente; d=sim; e=sim por `chamado_limbo.analisar`. app/chamados/services/disputa_emissao.py:1223; app/chamados/services/disputa_emissao.py:1232; app/chamados/services/disputa_emissao.py:1280; app/chamados/models.py:290.

A conversa criada em `:1272` não serve como evidência da trava posterior de Colaborador: `_nascer` só o trava quando `not conversa`; a evidência posterior adequada é `analisar` em `:1280`. app/chamados/services/disputa_emissao.py:1272; app/chamados/models.py:2668; app/chamados/services/disputa_emissao.py:1280.

## 07/11 20:55 · sessao cc06e3cd7ec5 · - · - · origem=Ronald (tmux)

| Chamada / função | b | c | d | e |
|---|---|---|---|---|
| `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:94` — `Command.handle`, `arquivo.silenciar` | não; fluxo sem atomic em `:43/:90` | não | não | não |
| `app/chamados/management/commands/arquivar_competencia_encerrada.py:288` — `Command._aplicar`, `arquivo.silenciar` | sim, `:285` | não | não | não; depois somente nota em `:296` |
| `app/chamados/management/commands/arquivar_prescritos.py:94` — `Command.handle`, `arquivo.silenciar` | sim, `:91` | não | não | não |
| `app/chamados/management/commands/nomear_marco_por_lampada.py:109` — `Command.handle`, `renovar_contexto` | sim, `:107` | não | não | não |
| `app/chamados/management/commands/carimbar_por_batida_coincidente.py:98` — `Command.handle`, `renovar_contexto` | sim, `:97` | não | não | não |
| `app/chamados/management/commands/carimbar_por_batida_coincidente.py:100` — `Command.handle`, `apontar_celula` | sim, `:97` | não; anterior `:98` grava contexto | não | não |
| `app/ponto/management/commands/reconciliar_grade.py:454` — `Command._relavrar_orfa`, `retirar_contexto` | não; chamada anterior ao atomic de `:472` | não | não | não |
| `app/ponto/management/commands/reconciliar_grade.py:473` — `Command._relavrar_orfa`, `renovar_contexto` | sim, `:472` | não | não | não |
| `app/ponto/management/commands/reconciliar_grade.py:474` — `Command._relavrar_orfa`, `retirar_contexto` | sim, `:472` | não; anterior `:473` grava contexto | não | não |
| `app/ponto/management/commands/curar_residuo_bug115.py:121` — `Command._ambiguo`, `renovar_contexto` | sim, `:120` | não | não | não |
| `app/ponto/management/commands/processar_alertas_avancados.py:173` — `Command.handle`, `escalar_urgencia` | não; ramo `:165` | não | não | não |
| `app/ponto/management/commands/processar_alertas_turno.py:423` — `Command._abrir_justificativa_automatica`, `apontar_justificativa` | não; `get_or_create` de `:391` já retornou | não | não | não |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:64` — `Command.handle`, `anotar_contexto` | não | não | não | não |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:103` — `Command.handle`, `marcar_escalonado_dp` | não | não | não | não |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:132` — `Command.handle`, `escalar_urgencia` | sim, `:131` | não | não | não |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:138` — `Command.handle`, `anotar_contexto` | sim, `:131` | não; anterior `:132` grava urgência | não | não |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:163` — `Command.handle`, `anotar_contexto` | não; método da disputa em `:159` retornou antes | não na mesma transação | não | não |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:179` — `Command.handle`, `anotar_contexto` | não | não | não | não |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:195` — `Command.handle`, `anotar_contexto` | não | não | não | não |

Os três sítios restantes exigem desdobrar caminhos, pois são funções importadas por outros módulos; seguem as evidências completas. `app/ponto/management/commands/gerar_celulas.py:223`; `app/ponto/management/commands/detectar_ausencias.py:543`; `app/ponto/management/commands/detectar_ausencias.py:570`.

| Chamada / caminho | b | c | d | e |
|---|---|---|---|---|
| `app/ponto/management/commands/gerar_celulas.py:255` — `_marcar_cobrancas_orfas`, regeneração/edição sem atomic exterior | sim: `app/ponto/portas/celula.py:218/:289` ou `:592/:626` | não no primeiro ato isolado | não | não: julgamento posterior fica após atomic próprio, `app/ponto/portas/celula.py:354/:627` |
| Mesmo sítio, receiver de âncora durante `executar_vinculo`, antes de qualquer outro julgamento/trava no caminho mínimo | sim: `app/colaboradores/services/vinculo.py:187/:274` → `app/escala/signals.py:117` | não nesse caminho mínimo | não | sim: receiver posterior `app/escala/signals.py:161` → trava C `app/chamados/models.py:1996` e DS `:1998` |
| Mesmo sítio, foto de folga copiada após receiver de âncora/remover motivo | sim: `app/colaboradores/services/vinculo.py:187/:275/:163` → `app/escala/folgas.py:162/:203/:272` | sim: DS salva antes em `app/chamados/models.py:2035`, PD em `:2031` | sim: `app/chamados/models.py:1996/:1998` | pode: rabo chama cartório em `app/ponto/portas/celula.py:354/:434`, fábrica posterior em `app/ponto/services/cartorio.py:997` |
| `app/ponto/management/commands/detectar_ausencias.py:558` — `_espelhar_marcos`, comando avulso | não: `app/ponto/management/commands/detectar_ausencias.py:61/:744` | não na mesma transação | não na mesma transação | não na mesma transação |
| Mesmo sítio via `_abrir_chamado` sob atomic exterior | sim, por exemplo `app/ponto/services/veredito_celula.py:223` → batida `:322` → receiver `app/ponto/signals.py:135/:93` → cartório `app/ponto/services/cartorio.py:1740/:1778` | sim se houve ressuscitação/reconciliação anterior: `app/ponto/services/cartorio.py:1622/:1488/:1501/:1522`; UPDATE PD em `app/chamados/models.py:2994`, trava DS em `:694` | sim: `Chamado.abrir` anterior, `app/ponto/management/commands/detectar_ausencias.py:693` → `app/chamados/models.py:888` | sim/pode: `app/ponto/management/commands/detectar_ausencias.py:801` → `app/chamados/services/disputa_emissao.py:968`; fábrica `app/ponto/services/cartorio.py:997` → `app/chamados/services/disputa_emissao.py:530` |
| Mesmo sítio via chamadas diretas do cartório `app/ponto/services/cartorio.py:1789/:1798`, sob atomic de `app/ponto/management/commands/celula_veredito_velho.py:133/:144` | sim | sim se ressuscitou antes: `app/ponto/services/cartorio.py:1622` → `app/chamados/models.py:1533/:2994`; DS reaberta salva em `:2147` | não se não houve reconciliação/trava anterior; ressuscitação acima só faz UPDATE PD e save DS | sim/pode: fábrica posterior `app/ponto/services/cartorio.py:997` → `app/chamados/services/fabrica_da_celula.py:149` → `app/chamados/services/disputa_emissao.py:627/:530` |
| `app/ponto/management/commands/detectar_ausencias.py:765` — `_abrir_chamado`, comando avulso | não: método `:570`, nascimento `:693` já retornou | não na mesma transação | não na mesma transação | não na mesma transação |
| Mesmo sítio sob atomic exterior do cartório | sim, caminhos acima: `app/ponto/services/veredito_celula.py:223`; `app/ponto/management/commands/celula_veredito_velho.py:133/:144` | sim se ressuscitação/reconciliação ocorreu antes: `app/ponto/services/cartorio.py:1622/:1488/:1501/:1522` → PD UPDATE `app/chamados/models.py:2994`, DS lock `:694` | sim sempre nessa variante: `app/ponto/management/commands/detectar_ausencias.py:693` → `app/chamados/models.py:888` | sim/pode: `app/ponto/management/commands/detectar_ausencias.py:801` → `app/chamados/services/disputa_emissao.py:968`; fábrica posterior `app/ponto/services/cartorio.py:997` → `app/chamados/services/disputa_emissao.py:530` |

Para `(c)=sim`: sítios `gerar_celulas.py:255`, `detectar_ausencias.py:558` e `detectar_ausencias.py:765`, exclusivamente nas variantes transacionais acima; não significa ocorrência demonstrada em produção. `app/chamados/models.py:2035/:2994/:694`; [nao verificado].

Para `(e)=sim,(d)=não`: sítios `gerar_celulas.py:255` no primeiro receiver de regeneração antes de remover motivo; `detectar_ausencias.py:558` via cartório direto sem trava C anterior, com fábrica posterior. `app/escala/signals.py:117/:161`; `app/chamados/models.py:1996/:1998`; `app/ponto/services/cartorio.py:1789/:1798/:997`; `app/chamados/services/disputa_emissao.py:530`.

## 07/11 20:54 · sessao 130dcd99d6c6 · - · - · origem=Ronald (tmux)

Censo dos serviços; `não` em b considera o ponto da chamada, sem contar o atomic interno da porta. Evidência dos atomics internos: app/chamados/services/contexto.py:100, app/chamados/services/contexto.py:157.

| Chamada / função | b | c | d | e |
|---|---|---|---|---|
| app/chamados/services/abertura.py:115 `abrir_chamado_local` → `apontar_ticket_glpi` | não; entrada app/chamados/views.py:197 | não | não | não; retorno app/chamados/services/abertura.py:119 |
| app/chamados/services/abertura.py:116 `abrir_chamado_local` → `renovar_contexto` | não; entrada app/chamados/views.py:197 | não | não | não; retorno app/chamados/services/abertura.py:119 |
| app/chamados/services/adesao.py:112 `sincronizar_adesao` → `renovar_contexto` | não; entrada app/escala/management/commands/apurar_furos_diarios.py:17 | não | não; atomic anterior de `abrir` não envolve esta chamada app/chamados/services/adesao.py:74 | não na mesma transação; reconciliação seguinte app/chamados/services/adesao.py:115 abre a própria |
| app/chamados/services/celula_carimbada.py:109 `backfill` → `apontar_celula` | não no corpo app/chamados/services/celula_carimbada.py:74; entrada de produção [nao verificado] | não no corpo app/chamados/services/celula_carimbada.py:87 | não no corpo app/chamados/services/celula_carimbada.py:87 | não; sequência app/chamados/services/celula_carimbada.py:110 |
| app/chamados/services/declaracao_texto.py:202 `declarar_nao_trabalhou` → `renovar_contexto` | não; entrada app/chamados/views.py:1066 | não | não | não; só nota e retorno app/chamados/services/declaracao_texto.py:205 |
| app/chamados/services/cobranca.py:178 `cobrar_colaborador` → `definir_prazo_sla` | sim app/chamados/services/cobranca.py:176 | não; antes apenas leitura app/chamados/services/cobranca.py:155 | não | não; só nota até saída do atomic app/chamados/services/cobranca.py:181 |
| app/chamados/services/disputa_emissao.py:1604 `recobrar_questionario` → `definir_prazo_sla` | não; entrada app/chamados/views.py:1425 | não | não | não; sequência app/chamados/services/disputa_emissao.py:1607 |
| app/chamados/services/silencio_isentos.py:43 `silenciar` → `arquivo.silenciar` | sim app/chamados/services/silencio_isentos.py:41 | não | não | não; loop somente arquivo e contador app/chamados/services/silencio_isentos.py:43 |
| app/chamados/services/silencio_ausencia.py:86 `reconciliar_por_ausencia` → `arquivo.silenciar` | não; comando app/ponto/management/commands/reavaliar_ausencias_lancadas.py:54 ou callback app/ponto/signals.py:269 registrado em app/ponto/signals.py:272 | não | não | não; retorno app/chamados/services/silencio_ausencia.py:92 |
| app/chamados/services/silencio_ausencia.py:72 `_levantar`, interna de `reconciliar_por_ausencia` → `arquivo.levantar` | não; mesmas entradas acima app/ponto/signals.py:272, app/ponto/management/commands/reavaliar_ausencias_lancadas.py:54 | não; `ch.reconciliar` anterior no ramo app/chamados/services/silencio_ausencia.py:101 encerra atomic próprio antes do levantamento app/chamados/services/silencio_ausencia.py:107 | não na mesma transação | não; sequência app/chamados/services/silencio_ausencia.py:108 |
| app/chamados/services/limbo_folgas.py:67 `aplicar_sugestao` → `renovar_contexto` | não; caller comando app/ponto/management/commands/detectar_ausencias.py:236 | não | não | não; chamada seguinte texto app/chamados/services/limbo_folgas.py:69 não trava |
| app/chamados/services/limbo_folgas.py:69 `aplicar_sugestao` → `reescrever_texto` | não; caller comando app/ponto/management/commands/detectar_ausencias.py:236 | não | não | não; retorno app/chamados/services/limbo_folgas.py:70 |
| app/chamados/services/limbo_folgas.py:270 `agrupar_furo` → `renovar_contexto`, caminho comando puro | sim app/chamados/services/limbo_folgas.py:269 | não; primeiro ato do atomic app/chamados/services/limbo_folgas.py:270 | não; primeiro ato do atomic app/chamados/services/limbo_folgas.py:270 | sim: reapontamento app/chamados/services/limbo_folgas.py:277 → trava Colaborador app/chamados/services/fio.py:855; reconciliação app/chamados/services/limbo_folgas.py:278 → app/chamados/models.py:290 |
| app/chamados/services/limbo_folgas.py:270 `agrupar_furo`, herdando atomic do cartório por `detectar_ausencias._abrir_chamado` | sim app/chamados/services/limbo_folgas.py:269; entrada cartório app/ponto/services/cartorio.py:1740 ou app/ponto/services/cartorio.py:1778 | depende de atos anteriores do cartório/signal; agente commands aprofundando [nao verificado] | sim quando houver atomic externo, pois `Chamado.abrir` anterior no mesmo `_abrir_chamado` mantém a trava; localização exata dessa chamada [nao verificado] | sim app/chamados/services/limbo_folgas.py:277, app/chamados/services/limbo_folgas.py:278 |
| app/chamados/rejuncao.py:107 `rejuntar_celula_dos_vivos` → `apontar_celula` | não; consumidor app/chamados/reconciliador.py:1354 sem atomic envolvente, atomic posterior começa app/chamados/reconciliador.py:1378; regeneração via callback app/ponto/portas/celula.py:498 registrado app/ponto/portas/celula.py:502 | não | não | não na mesma transação; rotina retorna app/chamados/rejuncao.py:111 |
| app/chamados/services/materializacao.py:715 `_ressuscitar_por_resposta` → `arquivo.levantar`, via views de resposta direta | não; core fecha atomic app/chamados/services/disputa_emissao.py:1393 antes de chamar ressuscitação app/chamados/services/disputa_emissao.py:1418; entradas app/chamados/views.py:1283, app/api/views.py:2659 | não na mesma transação | não na mesma transação | não na mesma transação app/chamados/services/disputa_emissao.py:1418 |
| app/chamados/services/materializacao.py:715 `_ressuscitar_por_resposta`, via `declarar_hora_colab` ou `aplicar_declaracao` | sim app/chamados/services/declaracao_texto.py:322 ou app/chamados/services/declaracao_texto.py:624 | sim: save Pergunta app/chamados/services/disputa_emissao.py:1389 antes de app/chamados/services/disputa_emissao.py:1418 | sim: núcleo trava app/chamados/services/disputa_emissao.py:530, chamado antes em app/chamados/services/declaracao_texto.py:330 ou app/chamados/services/declaracao_texto.py:627 | não: resposta transmitida é hora app/chamados/services/declaracao_texto.py:389 ou app/chamados/services/declaracao_texto.py:661; validação posterior está fora do atomic app/chamados/services/declaracao_texto.py:664 |

Para a lista **c=sim**, incluir `materializacao.py:715` nos dois caminhos de declaração com atomic externo e, conforme a análise do agente commands, os caminhos transacionais do cartório para `limbo_folgas.py:270`; a classificação definitiva deste último depende dos atos anteriores identificados. app/chamados/services/disputa_emissao.py:1389; app/ponto/services/cartorio.py:1740.

Para a lista **e=sim e d=não**, incluir `limbo_folgas.py:270` no comando puro/varredura, pelos callers `agrupar_furos_sob_limbo` e `detectar_ausencias` executado como comando. app/chamados/services/limbo_folgas.py:326; app/ponto/management/commands/detectar_ausencias.py:761.

`nota.py:123`/`fundir_contexto` ficou com o root, por coordenação. app/chamados/services/nota.py:123.

## 07/11 20:53 · sessao 00393fa21d8e · - · - · origem=Ronald (tmux)

| Chamada / função chamadora | (b) atomic externo | (c) PD/DS anterior | (d) Colaborador anterior | (e) trava posterior C/DS |
|---|---|---|---|---|
| `app/api/views.py:2033` — `api_justificativas` → `apontar_justificativa` | Não; função sem atomic (`app/api/views.py:1960`) | Não na mesma transação; `Justificativa.save/create` em `app/api/views.py:2021`/`:2023` dispara receiver `app/ponto/signals.py:161`, porém sem atomic externo | Não (`app/api/views.py:1960`) | Não; receiver de campo retorna (`app/chamados/signals.py:100`, `:188`) |
| `app/api/views.py:3159` — `api_ausencia_documento` → `renovar_contexto` | Não (`app/api/views.py:3091`) | Não na mesma transação; `_anexar` anterior em `app/api/views.py:3147` não está embrulhado com esta chamada | Não (`app/api/views.py:3091`) | Não (`app/chamados/signals.py:100`, `:188`) |
| `app/api/views.py:3160` — `api_ausencia_documento` → `reescrever_texto` | Não (`app/api/views.py:3091`) | Não (`app/api/views.py:3159`) | Não (`app/api/views.py:3091`) | Não (`app/chamados/signals.py:100`, `:188`) |
| `app/ponto/views.py:288` — `bater_ponto` → `renovar_contexto` | Não (`app/ponto/views.py:242`) | Não (`app/ponto/views.py:275`) | Não (`app/ponto/views.py:281`) | Não; registro de batida posterior pertence a outra transação (`app/ponto/views.py:350`) |
| `app/ponto/views.py:1648` — `_abrir_justificativa_fluxo` → `apontar_justificativa` | Não; wrapper apenas delega (`app/ponto/views.py:1565`, `:1568`) | Não na mesma transação; J salva/cria antes (`app/ponto/views.py:1633`, `:1635`) e dispara cartório (`app/ponto/signals.py:165`) | Não (`app/ponto/views.py:1568`) | Não (`app/chamados/signals.py:100`, `:188`) |
| `app/chamados/views.py:1025` — `atribuir_chamado` → `liberar_responsavel` | Não (`app/chamados/views.py:1008`) | Não (`app/chamados/views.py:1012`) | Não (`app/chamados/views.py:1012`) | Não (`app/chamados/signals.py:100`, `:188`) |
| `app/chamados/views.py:1029` — `atribuir_chamado` → `assumir_responsavel` | Não (`app/chamados/views.py:1008`) | Não (`app/chamados/views.py:1012`) | Não (`app/chamados/views.py:1012`) | Não (`app/chamados/signals.py:100`, `:188`) |
| `app/chamados/views_arquivar.py:37` — `arquivar_chamado` → `silenciar` | Não (`app/chamados/views_arquivar.py:24`) | Não (`app/chamados/views_arquivar.py:30`) | Não (`app/chamados/views_arquivar.py:30`) | Não (`app/chamados/signals.py:100`, `:188`) |
| `app/chamados/views_arquivar.py:77` — `desarquivar_chamado` → `levantar` | Não (`app/chamados/views_arquivar.py:52`) | Não na mesma transação; eventual `reabrir` anterior já retornou (`app/chamados/views_arquivar.py:70`) | Não na mesma transação (`app/chamados/views_arquivar.py:70`) | Não na mesma transação; eventual ressuscitação é posterior, fora da transação da porta (`app/chamados/views_arquivar.py:86`) |
| `app/chamados/views_arquivar.py:140` — `arquivar_lote` → `silenciar` | Sim (`app/chamados/views_arquivar.py:134`) | Não; laço apenas seleciona/verifica permissão/silencia (`app/chamados/views_arquivar.py:135`, `:137`, `:140`) | Não (`app/chamados/views_arquivar.py:134`) | Não; nenhuma trava no restante do bloco (`app/chamados/views_arquivar.py:147`) |
| `app/ponto/services/triagem_batida.py:292` — `ramo_regularizar_saida` → `escalar_urgencia` | Não; chamador direto sem atomic (`app/ponto/views.py:336`) | Não na mesma transação; `Justificativa.update_or_create` ocorreu antes (`app/ponto/services/triagem_batida.py:258`) | Não (`app/ponto/services/triagem_batida.py:227`) | Não (`app/chamados/signals.py:100`, `:188`) |
| `app/ponto/services/triagem_batida.py:369` — `ramo_solicitar_anuencia` → `escalar_urgencia` | Sim (`app/ponto/services/triagem_batida.py:368`) | Não; primeiro ato do bloco (`app/ponto/services/triagem_batida.py:369`) | Não; primeiro ato do bloco (`app/ponto/services/triagem_batida.py:369`) | Não; só renova contexto antes de sair (`app/ponto/services/triagem_batida.py:371`) |
| `app/ponto/services/triagem_batida.py:371` — `ramo_solicitar_anuencia` → `renovar_contexto` pelo alias `_renovar_contexto` | Sim; import alias e atomic (`app/ponto/services/triagem_batida.py:367`, `:368`) | Não; anterior foi só urgência de CC (`app/ponto/services/triagem_batida.py:369`) | Não (`app/ponto/services/triagem_batida.py:369`) | Não; fim do bloco (`app/ponto/services/triagem_batida.py:371`) |

Os perfis locais SaaS e prod não declaram `ATOMIC_REQUESTS`; WSGI/manage usam SaaS como padrão, mas configuração efetivamente executada em produção é [nao verificado]. (`app/config/settings/saas.py:83`, `app/config/settings/prod.py:9`, `app/config/wsgi.py:14`, `app/manage.py:9`)

O receiver de `Justificativa` chama o cartório sincronicamente; isso foi considerado, mas os três sítios envolvidos não mantêm uma transação externa aberta até a chamada da porta. (`app/ponto/signals.py:161`, `:165`, `:85`, `:93`; `app/api/views.py:2033`; `app/ponto/views.py:1648`; `app/ponto/services/triagem_batida.py:292`)

Recorte sem linhas com (c)=sim e sem linhas com (e)=sim/(d)=não, conforme os blocos e guards acima. (`app/chamados/views_arquivar.py:134`; `app/ponto/services/triagem_batida.py:368`; `app/chamados/signals.py:100`, `:188`)

_2814 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
