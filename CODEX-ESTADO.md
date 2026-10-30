# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 23:22** · publicado em **29/10/2026 23:25** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 23:22 · sessao 0a65ecea0174 · - · - · origem=Ronald (tmux)

Censo fora `models.py`: **11 pontos que atribuem carimbo não nulo**, em quatro módulos; há também a limpeza por `QuerySet.update` em `disputa_emissao.py:1360,1377`. Os pontos e respectivos chamadores estão abaixo. (`app/chamados/reconciliador.py:402,617`; `app/chamados/services/materializacao.py:509,858,1043,1149,1339`; `app/chamados/services/disputa_emissao.py:1562,1644`; `app/chamados/services/fio.py:846`.) **Correção da conta: são 10 pontos não nulos**, pois materialização tem cinco, reconciliador dois, emissão dois, fio um; o décimo primeiro é o zerador `disputa_emissao.py:1360,1377`.

| Sítio | Via / resposta / juiz antes | Caminho de produção |
|---|---|---|
| `app/chamados/reconciliador.py:402`, save `:407` | `sem_turno_aberto` (`:406`); recusa texto de resposta (`:365`); exige ausência de turno aberto (`:372`) e `not pergunta_viva(p)` (`:398–400`). | `reconciliar_chamados_colab:1265` → órfãs → chamada `:1411`; signal Batida `app/ponto/signals.py:206–215`; command `app/chamados/management/commands/reconciliar_chamados.py:51–52`; cron `app/config/crons.py:376`, `*/15`. |
| `app/chamados/reconciliador.py:617`, save `:620` | `celula_absolveu` (`:619`); resposta não é requisito (`:584–599`); célula julgada sem acusação no SQL (`:594–598`), marcos sem pendência (`:607–612`), depois `not pergunta_viva(p)` (`:613–614`). | `reconciliar_chamados_colab:1264`; mesmos signal/command/cron da linha anterior. |
| `app/chamados/services/materializacao.py:509`, save `:528` | `declaracao` se `declarou_hora(q)`, senão `ausencia_dia` (`:525–527`); **irmãs podem não ter resposta** (`:507–515`). Não consulta `pergunta_viva`, `fio_vivo` ou veredito da célula nesse escritor (`:492–530`). | Resposta escape em `responder_pergunta_disputa_core`, chamada `app/chamados/services/disputa_emissao.py:1292–1295`; o core exige resposta não vazia (`:1220–1221`) e salva resposta (`:1247–1248`). View PWA `app/chamados/views.py:1283`; API `app/api/views.py:2694–2695`. |
| `app/chamados/services/materializacao.py:858`, save `:868` | `paridade_recusada` (`:859`); ramo hora exige texto antes (`:769–781`); consulta **lâmpada do marco** `fato_do_marco_no_espelho(p) is True` (`:840–856`), não `pergunta_viva`/`fio_vivo`. | `_materializar_uma` chama ramo hora (`:1125–1126`); chamadores humanos e lote listados abaixo. |
| `app/chamados/services/materializacao.py:1043`, save `:1053` | `fato_ocupado` (`:1044`), somente código traduzido `COLISAO` e carimbo nulo (`:1035–1042`). **A própria `_carimbar_recusa` não exige resposta nem consulta juiz de vitalidade/célula** (`:1028–1053`); o caminho hora normal tem guarda de texto (`:769`). | `_materializar_uma` chama na recusa DATA_EM_DISPUTA (`:1118`) e no `finally` (`:1214`); passivo também chama `_carimbar_recusa` em `app/chamados/services/passivo_validado.py:545`. |
| `app/chamados/services/materializacao.py:1149`, save `:1198` | `declaracao` se `declarou_hora`, senão `via_pelo_fato`, ou `chamado_encerrado` sem destino (`:1167–1178`). Antes do carimbo, hora/data_hora exigem `_pergunta_materializada_de_fato`; **escolha dispensa esse juiz** (`:1136–1138`). Seleção pública requer `respondida_em` e `validada_em` (`:1286–1293`); ramo hora exige texto (`:769`), data_hora interpreta texto (`:940`); escolha não tem guarda de texto local (`:1008–1025`). | Detalhado abaixo. |
| `app/chamados/services/materializacao.py:1339`, save `:1342` | `juiz_aprovou` (`:1341`); resposta não é requisito do laço (`:1335–1336`); exige `_pergunta_materializada_de_fato(p)` antes (`:1337–1338`), não chama `pergunta_viva`/`fio_vivo`. | Signal `post_save DisputaSupervisao`, se fechada: `app/chamados/signals.py:124–140`. |
| `app/chamados/services/disputa_emissao.py:1562`, save `:1583` | `via_pelo_fato` (`:1564`); **aceita sem resposta**, só preenche respondida/validada se houver texto (`:1570–1577`). Consulta `not pergunta_viva` (`:1547–1548`) e barra dia trancado (`:1551–1552`); resposta existente sem lastro é poupada (`:1558–1560`). | `resolver_perguntas_por_chamado_resolvido`; caminhos abaixo. |
| `app/chamados/services/disputa_emissao.py:1644`, save `:1659` | `via_pelo_fato(pai)` ou `chamado_encerrado` (`:1646–1647`); **aceita sem resposta** (`:1655–1658`). Exige pai e destinos fora de VIVOS (`:1627–1633`) e `not pergunta_viva(q)` (`:1641–1642`). | Segundo ramo do mesmo resolver, mesmos caminhos abaixo. |
| `app/chamados/services/fio.py:846`, save `:848` | `duplicata` (`:844`); resposta não é requisito (`:830–849`). **Não consulta vitalidade no próprio escritor**. No chamador `migrar_resolvedoras_vivas`, só entram perguntas julgadas **vivas**, antes de deduplicar (`:882–892`); isso não é guarda “viva → não carimba”. | Command `dedup_perguntas_fato.py:88`, somente `--apply` (`:84–86`), declarado executor sob ordem em `app/config/crons.py:1330`; ou `ChamadoColaborador.reabrir` → `migrar_resolvedoras_vivas` (`app/chamados/models.py:1068–1072`) → `fio.py:892`. |

Chamadores dos ramos da materialização:

- View de regularização → `acao_regularizar_disputa` (`app/chamados/views.py:467–469`) → `materializar_perguntas_validadas_da_disputa` (`app/chamados/services/acoes_disputa.py:139`); o painel do fio chama diretamente a mesma função (`app/chamados/services/fio.py:664`). Ambos bloqueiam perguntas vivas sem `respondida_em` (`acoes_disputa.py:129–131`; `fio.py:653–655`).
- Validação humana → `validar_pergunta`; exige `respondida_em` (`app/chamados/services/validacao.py:356–358`), chama `_materializar_uma` diretamente para resposta de hora (`:415–417`) e depois o laço da disputa (`:423`). View da validação chama `acao_validar_pergunta` (`app/chamados/views.py:483–501`), que delega a `validar_pergunta` (`app/chamados/services/acoes_disputa.py:18–19`); painel do fio também (`app/chamados/services/fio.py:602–603`).
- **Passivo também chega à mesma unidade**, sem novo sítio de gravação: command `materializar_passivo_validado.py:110` → `passivo_validado.reparar` → guarda `apply` (`passivo_validado.py:359–364`) → `_trabalhar_o_par` (`:372`) → `_plantar_o_dia` (`:427–428`) → `_materializar_uma` (`:516`). **Não é cron agendado**: pertence a `FORA_DE_PIPELINE` (`app/config/crons.py:743`) e a entrada o descreve como ato sob aval (`:771–776`).

Chamadores do resolver S128, portanto dos dois pontos `disputa_emissao.py:1562` e `:1644`:

- Signal de chamado encerrado: `app/chamados/signals.py:75–92`.
- Reconciliador `_retratar`: `app/chamados/reconciliador.py:538`; entrada do reconciliador por colaborador chama `_retratar` em `:1283`; órfã reapontada chama resolver em `:1439`.
- Evento da célula: consumidor chama `retratar_chamados_absolvidos_por_celula` (`app/chamados/reconciliador.py:1214`), que chama `_retratar` (`:869`), que chama resolver (`:538`). Command consumidor chama serviço (`app/chamados/management/commands/consumir_eventos_celula.py:31`), agendado a cada minuto (`app/config/crons.py:375`); reconciliar `*/15` também drena consumidor (`app/chamados/management/commands/reconciliar_chamados.py:62`).
- Beco: `app/chamados/services/becos.py:49`; evento PerguntaDisputa (`app/chamados/signals.py:191–202`) → `becos.fechar_beco_da_disputa` → `fechar_beco` (`becos.py:73`). Command `fechar_becos_disputa.py:57` só grava em `--apply`; **cron `app/config/crons.py:639` usa `--alarme`, portanto apenas vigia**, conforme branch dry `fechar_becos_disputa.py:54–56,66–70`.
- Command manual de fantasmas: `app/ponto/management/commands/reconciliar_fantasmas.py:280`.

`via_pelo_fato` pode retornar todas as vias solicitadas: `cadastro_folga` (`app/chamados/juizes.py:1192`), `adesao` (`:1193`), `ausencia_dia` (`:1201`), `completude_turno` (`:1209`) e `chamado_encerrado` (`:1210`); não consulta `pergunta_viva` nessa função (`:1158–1210`).

**Legado:** não encontrei escritor vivo fora migrations para criar carimbo `legado_sem_lastro`; a constante declara “histórico, não se refaz” (`app/chamados/models.py:1915`) e o auditor declara que não tem escritor vivo (`app/chamados/management/commands/auditar_invariantes_chamados.py:438–449`). A migração histórica fica fora do escopo pedido (`.../auditar_invariantes_chamados.py:442–443`).

Zerador adicional fora modelo/relavra: `reabrir_para_correcao` acrescenta `materializada_em=None`, autor nulo e via vazia quando `desfazer_absorcao=True` (`app/chamados/services/disputa_emissao.py:1358–1360`), aplica `qs.update(**campos)` (`:1377`). Chamado pela devolução de ausência absorvente em `app/chamados/services/materializacao.py:628`; quem seleciona é `perguntas_absorvidas_por`, conforme contrato do próprio zerador (`disputa_emissao.py:1351–1356`).

Não achei `setattr` ou `bulk_update` de `materializada_em` para PerguntaDisputa: os `setattr` de restores em `bin/restore_relavratura_10_2026.py:74–81` e `bin/restore_o263_comp10.py:43–50` escrevem FechamentoMensal; o `bulk_update` PerguntaDisputa em `app/chamados/veredito_lavrado.py:207` usa `CAMPOS_DA_LAVRA`, não esse campo `[nao verificado: composição da constante não foi relida]`.

## 29/10 23:21 · sessao 9e3e4d17d91a · - · - · origem=Ronald (tmux)

Auditoria só leitura, com seis escritores que retiram `materializada_em` identificados:

| Escritor | Escrita e julgamento anterior | Caminho confirmado |
|---|---|---|
| `desfazer_carimbo_sem_lastro` | Exige carimbo presente, exclui `admin_decidiu`, `duplicata`, `fato_ocupado`, `paridade_recusada`; **consulta `pergunta_viva(p)` e só limpa se VIVA**. Zera materializada/autor/via e salva. Reabre disputa fechada. `app/chamados/services/relavra_via.py:166`, `:196`, `:200`, `:204`, `:207`, `:220` | `fabrica_da_celula.perguntar` chama para cada motivo/dia/colaborador carimbado (`app/chamados/services/fabrica_da_celula.py:102`, `:105`). Chamado pelo julgamento `cartorio.julgar_colab` → `perguntar_pela_celula` → `perguntar` (`app/ponto/services/cartorio.py:758`, `:770`; `app/chamados/services/fabrica_da_celula.py:123`) e pelo comando `disparar_perguntas_competencia` (`app/chamados/management/commands/disparar_perguntas_competencia.py:277`, `:288`). Cron encadeado executa esse comando com `--comp-corrente --apply`: `app/config/crons.py:1456`. |
| `ChamadoColaborador.ressuscitar_perguntas` | `QuerySet.update(materializada_em=None, validada_em=None, validada_por=None, respondida_em=None, via_resolucao='')`. Universo somente carimbadas sem texto; **não devolve marco cujo `fato_do_marco_no_espelho(p) is True`**. `app/chamados/models.py:1111`, `:1113`, `:1115`, `:1119`, `:1162` | Chamado por `ChamadoColaborador.reabrir` (`app/chamados/models.py:1098`) e cartório (`app/ponto/services/cartorio.py:1271`); no cartório, primeiro exige fio morto via `_fio_vivo` e ausência de texto (`app/ponto/services/cartorio.py:1261`, `:1263`). |
| `PerguntaDisputa.reabrir_fantasma` | Só desfaz via `fantasma` carimbada; zera materializada/autor/via e salva, **sem juiz dentro do método**. `app/chamados/models.py:2183`, `:2185`, `:2188` | Comando `reabrir_orfas_enterradas` chama (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`); antes seleciona `fato_do_marco_no_espelho is False` **e** `pergunta_viva=True`, recusa competência exportada e outras condições (`:77`, `:82`, `:94`, `:98`, `:105`). |
| `PerguntaDisputa.reabrir_por_resposta` | Só limpa carimbo se via em `VIAS_SEM_LASTRO`; retorna campos para salvar, **sem chamar juiz**. `app/chamados/models.py:2203`, `:2205`, `:2208` | Escritor da resposta nova atribui texto/respondida, chama o método e salva campos: `app/chamados/services/disputa_emissao.py:1247`, `:1248`, `:1265`, `:1266`. |
| `PerguntaDisputa.reabrir_por_competencia` | Só desfaz `competencia_trancada`; zera materializada/autor/via e salva, **sem juiz de vitalidade**. `app/chamados/models.py:2213`, `:2215`, `:2218` | `competencia_trancada.reabrir` percorre perguntas daquela competência e chama método (`app/chamados/services/competencia_trancada.py:244`, `:247`). View reabre período → serviço fechamento → serviço competência (`app/ponto/views.py:1544`; `app/ponto/services/fechamento.py:1415`, `:1416`). |
| `reabrir_para_correcao(..., desfazer_absorcao=True)` | Adiciona materializada/autor/via zerados ao dicionário e faz `qs.update(**campos)`; **não chama `pergunta_viva`, `fio_vivo` nem célula**. `app/chamados/services/disputa_emissao.py:1358`, `:1359`, `:1360`, `:1377` | Único caller com flag encontrada: `devolver_absorvidas_da_ausencia_rejeitada`, guarda ausência rejeitada e nenhuma outra `ausencia_cobre(..., para='cobranca')`, seleciona grupo de absorção `ausencia_dia/declaracao` sem batida plantada viva e chama clear (`app/chamados/services/materializacao.py:608`, `:615`, `:575`, `:578`, `:579`, `:586`, `:588`, `:628`). Gatilho post_save de Ausencia/on_commit: `app/ponto/signals.py:250`, `:261`, `:262`, `:272`, `:275`, `:279`. |

Regra da casa confirmada no código: `lavrar_sem_turno_aberto` recusa texto de resposta (`app/chamados/reconciliador.py:365`), carimbo existente (`:367`), turno aberto (`:372`) e **pergunta VIVA** (`:398`, `:399`), antes de gravar `sem_turno_aberto` (`:402`, `:406`, `:407`). Caminho batida → signal → reconciliador: `app/ponto/signals.py:206`, `:214`, `:215`; chamada da lavra no reconciliador: `app/chamados/reconciliador.py:1411`.

Selo presente: caso com célula acusando e lâmpada de entrada apagada exige `pergunta_viva=True`, lavra False e ausência de materializada/via (`app/chamados/tests/test_o261_lavra_le_a_lampada.py:129`, `:138`, `:139`, `:141`, `:143`). Caso com validação anterior permite lavra e mantém `respondida_em=None` (`:159`, `:163`, `:164`, `:167`, `:168`). Caso célula absolvendo exige nome `celula_absolveu` (`:172`, `:188`, `:191`).

`fio_vivo` é apenas `any(pergunta_viva(p) for p in perguntas)` (`app/chamados/juizes.py:1102`, `:1111`). `pergunta_viva` delega para `_pergunta_viva_calc` (`:778`, `:795`), cuja ordem considera validação/tranca (`:1006`, `:1010`), resposta (`:1020`), identidade (`:1044`), marco aceso com carimbo (`:1050`), veredito da célula (`:1054`, `:1056`), marcos pendentes (`:1063`, `:1064`) e lastro final (`:1083`).

**Não há porta única de primeiro carimbo**: a própria implementação declara os primeiros escritores em `materializacao.py`, `reconciliador.py`, `models.py`, `disputa_emissao.py`, enquanto `relavra_via` só troca via já carimbada (`app/chamados/services/relavra_via.py:58`, `:59`, `:60`). Evidência executável suficiente: lavra direta no reconciliador (`app/chamados/reconciliador.py:402`, `:407`) e lavra direta distinta em `materializar_pendentes_aprovadas` (`app/chamados/services/materializacao.py:1339`, `:1341`, `:1342`). Este último consulta `_pergunta_materializada_de_fato` (`:1337`), não a guarda `pergunta_viva`.

## 29/10 23:21 · sessao 858e08c08f4d · - · - · origem=Ronald (tmux)

Censo do modelo e chamadores, sem alteração de arquivos:

| Escritor | Via e resposta | Juiz antes da escrita / caminho |
|---|---|---|
| `app/chamados/models.py:2117`, atribuição `:2132`, `save :2134` — `fechar_por_admin` | `admin_decidiu` (`:2131`); método não exige resposta nem consulta juiz (`:2121-2134`). | Chamadores abaixo. |
| `app/chamados/models.py:2137`, atribuição `:2146`, `save :2148` — `fechar_por_competencia_trancada` | `competencia_trancada` (`:2145`); só impede validada ou via já trancada (`:2143`), não exige resposta nem consulta juiz (`:2142-2148`). | Nenhum chamador de produção encontrado `[nao verificado]`. Serviço atual diz e implementa que não carimba perguntas: `app/chamados/services/competencia_trancada.py:122-130`, `:171-173`. Incluir como escritor definido sem caminho ativo confirmado. |
| `app/chamados/models.py:2162`, atribuição `:2171`, `save :2173` — `encerrar_fantasma` | `fantasma` (`:2170`); método não exige resposta nem consulta juiz (`:2167-2173`). | Chamado por `app/chamados/reconciliador.py:1009`, após `veredito_orfa` (`:997-1002`), detalhado abaixo. |

**Chamadores de `fechar_por_admin`:**

1. `DisputaSupervisao.fechar`: consulta `pergunta_viva` e monta a lista de **vivas** em `app/chamados/models.py:1481-1482`; nas exceções `desligamento` e `admin_encerra`, carimba justamente essas vivas em `:1499-1502`. Recusa respostas sem veredito em `:1472-1480`. Portanto **consulta antes, mas não veta VIVA**; a consulta seleciona o que será carimbado.
   - View de desligamento chama executor: `app/colaboradores/views.py:1430-1434`; executor chama `fechar(... FECHOU_DESLIGAMENTO)` após pular respostas sem veredito: `app/colaboradores/services/desligamento.py:147-150`.
   - Cron 06:06: `app/config/crons.py:421`; comando chama mesmo executor: `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`.
   - `admin_encerra` exige motivo e delega a `fechar`: `app/chamados/models.py:1510-1517`. Chamador externo ativo dessa porta: `[nao verificado]`.

2. `fechar_o_fio_do_dia`: `app/ponto/services/veredito_celula.py:174-177`. Filtra por `q_vivas`, sem exigir resposta; não chama `pergunta_viva`, `fio_vivo` ou consulta veredito para veto nesse laço (`:174-177`). `q_vivas` é explicitamente pré-filtro generoso, não equivalente ao juiz (`app/chamados/models.py:2232-2239`).
   - View Resolver dia chama serviço: `app/ponto/views.py:3330-3331`; serviço chama `fechar_o_fio_do_dia` em `app/ponto/services/veredito_celula.py:325-328`.
   - Nesse serviço, **veredito da célula é lavrado depois** do fechamento/carimbo (`:329-330`).
   - Outro chamador é declaração de dia: `app/ponto/services/declarar_dia.py:197-198`; falta delega a `veredito_celula` em `:206-207`.

3. Aceite da resposta `FOLGA_CONTESTA`: `app/chamados/services/validacao.py:264` e `:292`. Aqui **exige resposta**: porta `validar_pergunta` recusa ausência de `respondida_em` (`:356-358`) e só entra no aceite quando código corresponde (`:375-376`; comparação `:216-218`). Antes do primeiro carimbo verifica `o_dia_ja_e_folga` (`:261`); segundo vem após `declarar_dia` bem-sucedido (`:283-292`). Não consulta `pergunta_viva`/`fio_vivo` como veto nesse trecho.
   - View/modal -> `acao_validar_pergunta` -> `validar_pergunta`: `app/chamados/views.py:483-501`, `app/chamados/services/acoes_disputa.py:18-22`.
   - Fio também chama a ação em `app/chamados/services/fio.py:602-603`.

**Fantasma merece destaque:** `veredito_orfa` exige texto vazio (`app/chamados/reconciliador.py:947-948`), lâmpada do próprio marco acesa por `fato_do_marco_no_espelho is True` (`:949-950`) e **pergunta VIVA**: `if not pergunta_viva(p): return None` (`:951-952`). Depois respeita teto/dia encerrado (`:953-957`). Assim, há consulta anterior, **com sinal contrário ao veto da regra descrita**: só carimba VIVA cuja lâmpada acendeu e cujo dia encerrou. `encerrar_orfas_pela_lampada` aplica esse veredito (`:996-1009`), chamado no consumidor de eventos (`:1217`); cron a cada minuto: `app/config/crons.py:375`; comando chama consumidor em `app/chamados/management/commands/consumir_eventos_celula.py:31`.

**Escritas que desfazem carimbo no modelo:**

| Escritor | Efeito e juiz |
|---|---|
| `ChamadoColaborador.ressuscitar_perguntas`, `app/chamados/models.py:1162-1163` | `QuerySet.update(materializada_em=None, ... via_resolucao='')`; universo só materializadas sem texto (`:1112-1116`) e pula `fato_do_marco_no_espelho is True` (`:1118-1119`), sem consultar `pergunta_viva`. Chamado por reabertura do chamado (`:1098`) e cartório (`app/ponto/services/cartorio.py:1271`). |
| `reabrir_fantasma`, `app/chamados/models.py:2185-2188` | Limpa via/carimbo/autor; método só checa via e existência de carimbo (`:2183`). Comando chama em `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`; antes exige lâmpada **False** e `pergunta_viva` **True** (`:77-83`), aplica restrições da janela (`:87-91`) e exportação (`:93-102`). |
| `reabrir_por_resposta`, `app/chamados/models.py:2205-2208` | Limpa em memória quando tem carimbo e via em `VIAS_SEM_LASTRO` (`:2203-2204`); retorna campos para salvar. Core exige resposta não vazia (`app/chamados/services/disputa_emissao.py:1219-1221`), chama método (`:1265`) e salva (`:1266`). Entradas web (`app/chamados/views.py:1281-1283`) e API (`app/api/views.py:2694-2695`). Sem juiz explícito nessa operação. |
| `reabrir_por_competencia`, `app/chamados/models.py:2215-2218` | Limpa somente via `competencia_trancada` (`:2213-2214`), sem juiz. Chamado em `app/chamados/services/competencia_trancada.py:244-247`, por reabertura de período (`app/ponto/services/fechamento.py:1413-1416`), acionada na view (`app/ponto/views.py:1544`). |

`encerrar_por_fato_no_espelho` **não escreve `materializada_em`**: somente `via_resolucao=fato_materializado` (`app/chamados/models.py:2156-2159`); chamador `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`.

`desfazer_carimbo_sem_lastro` usa exatamente `pergunta_viva` (`app/chamados/services/relavra_via.py:192`, `:200-201`); pula vias de identidade definidas em `:166-167` (`:198-199`) e limpa carimbo/autor/via com `save` (`:204-208`). Também reabre disputa fechada (`:220-222`).

## 29/10 23:16 · sessao 0019299f2950 · o261c2 · leitura · origem=Code

As referências abaixo usam a árvore-base `e84a9463…`, sob `app/`. A conclusão central é: **já existe costura de órfãs, mas ela não cobre toda pergunta viva; os retornos à vida também não passam por uma porta única de escolha de destino.** ([reconciliador.py:1317](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:1317), [models.py:2176](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2176), [disputa_emissao.py:1358](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:1358).)

**(1) Os seis emissores**

| Emissor | Motivo e `data_evento` enviados a `abrir` | `chamado_destino` |
|---|---|---|
| `gerar_perguntas_disputa`, ~389 | Itera `motivos_novos` com template. Obtém data da grade quando o texto não consegue resolver a data; depois tenta contexto `[motivo]['data']` e `ultima_batida.timestamp` em data local. Envia a data do derivador quando disponível, senão esse fallback. ([disputa_emissao.py:292](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:292), [:316](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:316), [:343](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:343), [:389](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:389).) | Pode receber `_ch_dest`: deriva para motivos que exigem destino e para intervalos pertencentes a `MOTIVOS_DO_DIA_SEM_FECHAR`. Pode continuar `None`. ([disputa_emissao.py:367](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:367).) |
| `_criar_pergunta_se_couber`, ~770 | Repassa os argumentos `motivo` e `data_evento`; verifica ausência e pergunta anterior. ([disputa_emissao.py:756](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:756).) | Repassa explicitamente o chamado escolhido pelo agrupador; admite chamados relacionados. ([disputa_emissao.py:767](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:767).) |
| `criar_disputa_ancora_de_chamado`, ~1160 | `ancora_ausente`; omite data. `abrir` grava o dia local da abertura. ([disputa_emissao.py:1160](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:1160), [models.py:2054](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2054).) | O próprio `chamado_limbo`, cuja categoria deve ser `ancora_limbo` e cujo estado não pode estar em `ENCERRADOS`. ([disputa_emissao.py:1107](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:1107), [:1166](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:1166).) |
| `acao_reabrir_disputa_admin`, ~265 | Motivo do POST, validado em `MOTIVOS_CHOICES`; omite data, portanto recebe o dia local da reabertura. ([acoes_disputa.py:245](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/acoes_disputa.py:245), [:259](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/acoes_disputa.py:259), [models.py:2054](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2054).) | Omite destino. Depois tenta reabrir o chamado da disputa se encerrado. ([acoes_disputa.py:259](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/acoes_disputa.py:259), [:265](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/acoes_disputa.py:265).) |
| `declarar_hora_colab`, ~322 | Motivo e dia do marco faltante que casou com a hora declarada; parte de `dia_do_fato(ch)`. ([declaracao_texto.py:281](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/declaracao_texto.py:281), [:296](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/declaracao_texto.py:296).) | O próprio `ch` onde o colaborador declara. Também costura explicitamente uma candidata reutilizada sem destino. ([declaracao_texto.py:310](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/declaracao_texto.py:310).) |
| `aplicar_declaracao`, ~611 | `p['motivo']` e `p['dia']` da proposta extraída do followup e casada com marco faltante. ([declaracao_texto.py:572](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/declaracao_texto.py:572), [:600](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/declaracao_texto.py:600).) | O próprio `ch` quando chama `abrir`. Se encontra candidata reutilizável, esse ramo não contém a costura explícita da porta anterior. ([declaracao_texto.py:608](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/declaracao_texto.py:608).) |

Nos dois emissores de declaração, o mapa é início/E → `saida_sem_entrada`; fim/S → `orfao_14h`; ida/S → `intervalo_saida`; volta/E → `intervalo_volta`. ([declaracao_texto.py:42](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/declaracao_texto.py:42).)

No agrupador que alimenta ~770, existem quatro escolhas concretas:

- Intervalo dedicado: motivo pelo tipo E/S, dia de `marcos_faltantes[].data`, destino no próprio chamado de intervalo. ([disputa_emissao.py:939](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:939).)
- Pela ata: dia pela chave declarada do módulo; perguntas dos marcos faltantes apontam para aquele chamado. ([disputa_emissao.py:966](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:966).)
- Com batida de origem: motivo de `classificar_falta`, dia de `resolver_data_evento(...) or data_entrada`, destino no primeiro chamado do grupo. ([disputa_emissao.py:996](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:996).)
- Sem origem: dia extraído do contexto, motivo de `classificar_falta`, destino no próprio chamado; exige base e ata. ([disputa_emissao.py:1030](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:1030).)

**Qual chamado cobra esse colab-dia:** o código não permite deduzir um único módulo pelo motivo. Há emissão de `batida_ausente` com `contexto_json.data_turno`, emissão de `turno_aberto` nessa mesma chave e módulos declarados `turno_aberto_24h` e `intervalo_pendente`; a emissão específica de intervalo está aposentada, mantendo reconciliação do legado. ([detectar_ausencias.py:688](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/ponto/management/commands/detectar_ausencias.py:688), [processar_alertas_turno.py:335](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/ponto/management/commands/processar_alertas_turno.py:335), [catalogo/modulos.py:105](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/catalogo/modulos.py:105), [detectar_intervalo_ausente.py:145](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/ponto/management/commands/detectar_intervalo_ausente.py:145).)

A célula do chamado é procurada no nascimento por `(colaborador, dia forte)`. Esse dia prefere `chamado.celula.data`, depois a chave declarada no catálogo e o fallback `data_turno`; a pergunta datada procura sua própria célula por `(colaborador, data_evento)`. Não se deve presumir igualdade dessas datas sem conferir o turno. ([models.py:716](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:716), [:895](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:895), [:2058](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2058), [juizes.py:78](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/juizes.py:78), [:1730](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/juizes.py:1730).)

**O mapa escrito é categoria/subcategoria → motivos atendidos**, em `MAPA_CATEGORIA_PARA_MOTIVO_DISPUTA`, `MAPA_SUBCATEGORIA_PARA_MOTIVO_DISPUTA` e `motivos_atendidos`. Para categorias/subcategorias reconhecidas, este último devolve os quatro motivos do dia sem fechar. Não é um mapa motivo → `modulo_origem`; o derivador sequer filtra módulo. ([models.py:2466](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2466), [:2502](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2502), [juizes.py:1589](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/juizes.py:1589), [disputa_emissao.py:181](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:181).)

Os PKs, módulos, JSONs e células que efetivamente cobram cada pergunta hoje dependem dos registros do banco. **[nao verificado]**

**(2) Quem escolhe destino, e por qual juiz**

| Componente | Responsabilidade efetiva |
|---|---|
| `PerguntaDisputa.abrir` | Recebe destino do emissor; verifica `resolvedora_valida`. No dedup de fato, pode reapontar a pergunta existente ao destino recebido. Não busca cobrança viva. ([models.py:2028](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2028), [:2064](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2064).) |
| `cobranca_viva_do_dia` | Recebe módulo(s), busca chamados `VIVOS` com JSON ou célula no dia, confirma `dia_do_fato(fortes=True)` e escolhe o mais novo por criação/PK. Não escreve destino nem infere módulo pelo motivo. ([juizes.py:1610](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/juizes.py:1610).) |
| `resolvedora_valida` | Confere compatibilidade temporal; não escolhe candidato, não exige chamado vivo nem verifica motivo. Admite datas vizinhas para turno que cruza meia-noite ou condição desconhecida. ([juizes.py:1687](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/juizes.py:1687).) |
| `_derivar_chamado_destino` | Busca vivos por categoria/subcategoria e dia; exclui chamado já ocupado por pergunta daquele motivo com materialização NULL; exige candidato único após desempate pelo turno. Não usa C8. ([disputa_emissao.py:174](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:174), [:209](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:209).) |
| `reapontar_resolvedora` | Recebe o chamado escolhido, consulta `resolvedora_valida` e grava destino e lavra juntos. Não procura candidato. ([fio.py:785](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/fio.py:785).) |
| `reemitir_chamado_do_dia` | Para `orfao_14h`/`saida_sem_entrada`, procura/abre a chave fixa `batida_ausente + data_turno`; aplica seus gates e só costura se o resultado estiver vivo. ([reconciliador.py:332](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:332), [:712](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:712), [:737](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:737), [:802](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:802).) |

**Sim, já existe costura de pergunta sem destino:** `reatar_pergunta_orfa`. Primeiro deriva vivo; sem candidato, tenta histórico encerrado; sem histórico, tenta `lavrar_sem_turno_aberto` e depois c2. Histórico único pode levar a um destino encerrado e ao resolvedor correspondente; não é exclusivamente uma busca por cobrança viva. ([reconciliador.py:1369](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:1369), [:1384](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:1384), [:1411](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:1411), [:1433](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:1433).)

Ela é chamada pelo emissor principal **somente para pergunta recém-criada**, sem destino e de um dos quatro motivos; também pelo reconciliador, cujo universo exige destino NULL, disputa aberta, materialização NULL e motivo reatável. O comentário “HOJE SO O CRON A CHAMA” está desatualizado diante da chamada no emissor. ([disputa_emissao.py:404](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:404), [reconciliador.py:1317](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:1317), [:1352](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:1352).)

Isso deixa lacunas demonstráveis:

- Pergunta existente devolvida por `abrir` tem `_criada=False`, portanto não passa pela costura pós-nascimento. ([models.py:2084](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2084), [disputa_emissao.py:404](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:404).)
- Pergunta viva pode ter materialização preenchida; disputa fechada ou motivo fora do conjunto também a excluem do reconciliador. ([juizes.py:1050](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/juizes.py:1050), [reconciliador.py:1317](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:1317).)
- O derivador pode excluir o vivo por ocupação do mesmo motivo ou permanecer ambíguo; c2 cobre apenas dois motivos e um módulo. ([disputa_emissao.py:209](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:209), [reconciliador.py:712](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:712), [:738](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/reconciliador.py:738).)
- Sem destino, `chamado_que_lista` continua escolhendo o pai, sem examinar seu status. ([fio.py:116](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/fio.py:116).)

**Não está demonstrado qual dessas barreiras explica cada uma das 807**, nem se o reconciliador executou sobre elas: faltam os registros e a evidência de execução. **[nao verificado]**

**(3) Pergunta existente que volta a viver**

**Não convergem numa porta única de reabertura ou costura.** As portas efetivas são distintas: ([disputa_emissao.py:1358](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:1358), [models.py:2176](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2176), [:2191](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2191), [:2210](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2210).)

| Caminho | Porta |
|---|---|
| Correção do questionário / recusa de resposta | `reabrir_para_correcao`; zera texto, resposta e validação, preservando versão anterior. ([models.py:1780](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:1780), [validacao.py:540](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/validacao.py:540), [disputa_emissao.py:1358](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:1358).) |
| Ausência rejeitada | `devolver_absorvidas_da_ausencia_rejeitada` → `reabrir_para_correcao(..., desfazer_absorcao=True)` e `reabrir_sistema` nas disputas. ([materializacao.py:628](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/materializacao.py:628).) |
| Fantasma | `PerguntaDisputa.reabrir_fantasma`; desfaz via e materialização. ([models.py:2183](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2183).) |
| `reabrir_orfas_enterradas` | Chama `reabrir_fantasma`, depois reabre disputa e, quando necessário, o próprio pai. Não procura cobrança de outro chamado. ([reabrir_orfas_enterradas.py:146](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/management/commands/reabrir_orfas_enterradas.py:146), [:154](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/management/commands/reabrir_orfas_enterradas.py:154), [:161](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/management/commands/reabrir_orfas_enterradas.py:161).) |
| Resposta nova | O core desfaz validação anterior e chama `reabrir_por_resposta`, que desfaz apenas carimbo das vias sem lastro. ([disputa_emissao.py:1260](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:1260), [models.py:2203](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2203).) |
| Competência reaberta | `competencia_trancada.reabrir` → `reabrir_por_competencia`; desfaz aquela via, conservando resposta. ([competencia_trancada.py:247](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/competencia_trancada.py:247), [models.py:2210](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2210).) |
| Ressurreição pelo chamado | `ressuscitar_perguntas`; atua nas perguntas que já apontam para aquele chamado, excluindo marco comprovadamente aceso. ([models.py:1112](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:1112), [:1153](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:1153).) |

Nem a persistência converge: correção e ressurreição usam `QuerySet.update`; fantasma e competência usam `save`. O receiver de pergunta agenda fechamento de beco, sem escolher destino. ([disputa_emissao.py:1377](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:1377), [models.py:1162](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:1162), [:2188](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2188), [signals.py:191](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/signals.py:191).)

Há ainda vida que volta **por leitura**: removida a tranca, ou mudando a célula/classificação do dia, o juiz pode voltar a dar viva sem nenhuma dessas escritas. O denominador comum é `pergunta_viva`, uma autoridade de leitura; não uma porta que costura destino. ([juizes.py:1005](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/juizes.py:1005), [:1054](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/juizes.py:1054), [:1066](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/juizes.py:1066).)

**(4) Conversa: qual “dia” e qual chamado lista**

Ao receber `data_evento=None`, `abrir` grava `timezone.localdate()`: **o dia em que a conversa foi aberta**. Esse ramo pula dedup de fato e busca de célula; a verificação inicial do destino ocorre antes dessa normalização e exige data originalmente explícita. ([models.py:2043](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2043), [:2052](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2052).)

- **Reabertura administrativa:** data da conversa, destino NULL, listagem no pai da disputa existente. A ação tenta reabrir esse pai; não consulta a cobrança do dia da conversa. ([acoes_disputa.py:259](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/acoes_disputa.py:259), [:265](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/acoes_disputa.py:265), [fio.py:117](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/fio.py:117).)
- **`ancora_ausente` no emissor próprio:** data da conversa, destino explícito no `chamado_limbo`; é ele que lista. `ancora_limbo` declara escopo PERÍODO e chave `data_inicio`, não `data_turno`. ([disputa_emissao.py:1160](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/disputa_emissao.py:1160), [catalogo/modulos.py:131](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/catalogo/modulos.py:131), [fio.py:117](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/fio.py:117).)
- **Dia real da primeira entrada:** vem da data/hora respondida pelo colaborador, que o materializador transforma no timestamp da batida E. O `data_evento` da conversa não demonstra esse dia. ([models.py:2461](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/models.py:2461), [materializacao.py:939](../e84a9463bfb582ed01e420ee31c3b3f1a97f1c61/app/chamados/services/materializacao.py:939).)

## 29/10 23:14 · sessao b8d86e930c33 · - · - · origem=Ronald (tmux)

Todas as referências abaixo são da base `e84a9463`, sob `app/`.

**(3) Não há uma porta única pela qual todos esses retornos à vida passem.** Há portas distintas para atos distintos; nenhuma das portas abaixo escolhe `chamado_destino`. A reabertura do questionário faz `QuerySet.update`, enquanto fantasma, resposta e competência usam `save`, portanto nem a persistência converge num `save` comum. (`chamados/services/disputa_emissao.py:1358`, `chamados/services/disputa_emissao.py:1377`, `chamados/models.py:2183`, `chamados/models.py:2203`, `chamados/models.py:2213`.)

| Caminho | Porta efetiva e o que faz |
|---|---|
| Correção pelo colaborador | `DisputaSupervisao.reabrir_por_colab` pede o conjunto ao juiz `perguntas_corrigiveis` e chama `reabrir_para_correcao`; a disputa permanece aberta. (`chamados/models.py:1745`, `chamados/models.py:1770`, `chamados/models.py:1780`, `chamados/models.py:1784`.) |
| Correção pela supervisão | `acao_reabrir_disputa` também lê `perguntas_corrigiveis` e chama `reabrir_para_correcao`. (`chamados/services/acoes_disputa.py:90`, `chamados/services/acoes_disputa.py:96`, `chamados/services/acoes_disputa.py:103`.) |
| Resposta recusada | `validacao.recusar_pergunta` registra o motivo e chama `reabrir_para_correcao`. (`chamados/services/validacao.py:538`, `chamados/services/validacao.py:540`.) |
| `reabrir_para_correcao` | Zera texto, resposta e validação; com `desfazer_absorcao=True`, também materialização e via. Guarda `RespostaAnterior` no mesmo `atomic`, depois executa `qs.update`. (`chamados/services/disputa_emissao.py:1358`, `chamados/services/disputa_emissao.py:1368`, `chamados/services/disputa_emissao.py:1370`, `chamados/services/disputa_emissao.py:1377`.) |
| Ausência rejeitada | `devolver_absorvidas_da_ausencia_rejeitada` exige ausência rejeitada e nenhuma outra ausência viva cobrindo o dia; chama `reabrir_para_correcao(..., desfazer_absorcao=True)` e reabre as disputas por `reabrir_sistema`. (`chamados/services/materializacao.py:608`, `chamados/services/materializacao.py:615`, `chamados/services/materializacao.py:628`, `chamados/services/materializacao.py:631`.) |
| Fantasma | `PerguntaDisputa.reabrir_fantasma` exige via `fantasma` e materialização; zera via e carimbos de materialização, preservando resposta e validação. (`chamados/models.py:2183`, `chamados/models.py:2185`, `chamados/models.py:2188`.) |
| `reabrir_orfas_enterradas` | O comando seleciona fantasmas carimbadas, não validadas e datadas; exige lâmpada explicitamente apagada e `pergunta_viva`; separa competência exportada para pauta, restringe janela do carimbo e verifica recusa/irmãs do pai terminal. No apply chama `p.reabrir_fantasma`, depois `d.reabrir_sistema` e, se necessário, `pai.reabrir`. Não chama `abrir` nem reaponta destino. (`chamados/management/commands/reabrir_orfas_enterradas.py:67`, `chamados/management/commands/reabrir_orfas_enterradas.py:77`, `chamados/management/commands/reabrir_orfas_enterradas.py:82`, `chamados/management/commands/reabrir_orfas_enterradas.py:87`, `chamados/management/commands/reabrir_orfas_enterradas.py:98`, `chamados/management/commands/reabrir_orfas_enterradas.py:104`, `chamados/management/commands/reabrir_orfas_enterradas.py:146`, `chamados/management/commands/reabrir_orfas_enterradas.py:154`, `chamados/management/commands/reabrir_orfas_enterradas.py:161`.) |
| Resposta nova | `responder_pergunta_disputa_core` grava texto/carimbo, desfaz validação anterior e chama `p.reabrir_por_resposta`; esta só desfaz materialização das `VIAS_SEM_LASTRO`. O core salva tudo em seguida. (`chamados/services/disputa_emissao.py:1247`, `chamados/services/disputa_emissao.py:1260`, `chamados/services/disputa_emissao.py:1265`, `chamados/services/disputa_emissao.py:1266`, `chamados/models.py:2203`.) |
| Competência reaberta | `services.competencia_trancada.reabrir` chama `q.reabrir_por_competencia`, que só desfaz a via `competencia_trancada` e materialização, conservando resposta. O serviço também tenta reabrir chamados encerrados pela tranca. (`chamados/services/competencia_trancada.py:244`, `chamados/services/competencia_trancada.py:247`, `chamados/services/competencia_trancada.py:252`, `chamados/services/competencia_trancada.py:258`, `chamados/models.py:2210`, `chamados/models.py:2213`.) |

Outro caminho é `ChamadoColaborador.ressuscitar_perguntas`: lê perguntas que apontam para **aquele próprio chamado** por `perguntas_que_resolvem`, com materialização e sem texto; exclui marco comprovadamente aceso; zera carimbos por `update` e reabre disputas fechadas. Essa porta também não recebe perguntas sem destino nem procura chamado do dia. (`chamados/models.py:1112`, `chamados/models.py:1119`, `chamados/models.py:1153`, `chamados/models.py:1160`, `chamados/models.py:1162`, `chamados/models.py:1188`.)

`desfazer_carimbo_sem_lastro` é outra porta: seleciona pergunta que o juiz **já dá por viva**, remove somente materialização/via e reabre a disputa, sem modificar resposta/validação/destino. (`chamados/services/relavra_via.py:195`, `chamados/services/relavra_via.py:198`, `chamados/services/relavra_via.py:200`, `chamados/services/relavra_via.py:204`, `chamados/services/relavra_via.py:220`.)

Há retorno à vida **apenas por leitura**, sem porta de escrita: destrancar remove a causa lida por `dia_da_pergunta_trancado`; célula que passa a acusar retorna `True`; classificação de falta na janela também pode retornar `True`, mesmo com materialização anterior. O denominador comum é o juiz `pergunta_viva`, não um gate de reabertura/costura. (`chamados/juizes.py:875`, `chamados/juizes.py:881`, `chamados/juizes.py:1005`, `chamados/juizes.py:1054`, `chamados/juizes.py:1066`, `chamados/juizes.py:1070`, `chamados/juizes.py:1072`.)

O único receiver `post_save` de pergunta na base agenda **fechamento de beco**, não escolhe destino nem reabre chamados. E `reabrir_para_correcao` usa `update`, portanto não passa por esse receiver. (`chamados/signals.py:191`, `chamados/signals.py:197`, `chamados/signals.py:202`, `chamados/services/becos.py:61`, `chamados/services/becos.py:69`, `chamados/services/disputa_emissao.py:1377`.)

**(4) Conversa tem dois conceitos de “dia” que não devem ser confundidos.** Ao receber `data_evento=None`, `abrir` marca `_conversa=True` e grava `timezone.localdate()`: o dia em que a pergunta foi feita. Esse ramo não consulta `CelulaDia` nem faz dedup de fato. A verificação inicial de `resolvedora_valida` só ocorre se o argumento original `data_evento` não for `None`, antes dessa normalização. (`chamados/models.py:2043`, `chamados/models.py:2044`, `chamados/models.py:2052`, `chamados/models.py:2054`, `chamados/models.py:2057`, `chamados/models.py:2058`, `chamados/models.py:2060`.)

- **Reabertura administrativa:** o emissor passa motivo escolhido pelo admin e não passa data nem destino. A pergunta recebe o dia atual da conversa e lista no **pai da disputa existente**. Em seguida a ação tenta reabrir esse chamado se terminal; não procura cobrança desse colab-dia. (`chamados/services/acoes_disputa.py:245`, `chamados/services/acoes_disputa.py:256`, `chamados/services/acoes_disputa.py:259`, `chamados/services/acoes_disputa.py:264`, `chamados/services/acoes_disputa.py:265`, `chamados/services/fio.py:116`, `chamados/services/fio.py:117`.)
- **`ancora_ausente` pelo emissor próprio:** não passa data, mas passa explicitamente `chamado_destino=chamado_limbo`; portanto recebe o dia atual como data da conversa e lista no **chamado limbo recebido**, e não no pai. (`chamados/services/disputa_emissao.py:1160`, `chamados/services/disputa_emissao.py:1162`, `chamados/services/disputa_emissao.py:1166`, `chamados/services/fio.py:117`.)
- **Dia da primeira entrada da âncora:** vem da data/hora escrita na resposta do colaborador. O materializador parseia a data da resposta, constrói `ts` e registra batida `E` nesse instante; `MOTIVOS_DATA_EVENTO_NA_RESPOSTA` contém explicitamente `ancora_ausente`. Logo o dia atual gravado em `data_evento` não prova o dia da primeira entrada. (`chamados/models.py:2461`, `chamados/models.py:2464`, `chamados/services/materializacao.py:939`, `chamados/services/materializacao.py:944`, `chamados/services/materializacao.py:947`, `chamados/services/materializacao.py:952`.)

A lei da listagem é literalmente `p.chamado_destino_id or d.chamado_id`; não há busca do dia nessa função. Para mostrar divergência, `data_do_fato_de` compara `p.data_evento` ao dia forte do chamado que lista; havendo divergência sem conferência humana, `_materializar_uma` bloqueia antes de despachar inclusive o ramo `data_hora`. (`chamados/services/fio.py:116`, `chamados/services/fio.py:117`, `chamados/services/fio.py:77`, `chamados/services/fio.py:85`, `chamados/services/fio.py:87`, `chamados/services/materializacao.py:1096`, `chamados/services/materializacao.py:1098`, `chamados/services/materializacao.py:1119`, `chamados/services/materializacao.py:1127`.)

## 29/10 23:14 · sessao 19964796909d · - · - · origem=Ronald (tmux)

Leitura na base `/home/ronald/codex_ws/e84a9463bfb582ed01e420ee31c3b3f1a97f1c61`; referências abaixo são relativas a essa raiz. [não verificado: identificação da árvore por Git, pois o snapshot não contém `.git`]

**Semântica comum do `abrir`**

- Assinatura: `abrir(disputa, motivo, data_evento=None, chamado_destino=None, **campos)`. Pergunta datada deduplica por `(colaborador, motivo, data_evento)` e pode reapontar uma linha existente de qualquer disputa; `data_evento=None` é conversa e não entra nesse dedup. `app/chamados/models.py:2028`
- Atenção: `None` recebido no `abrir` **não fica NULL**: `_conversa=True` substitui por `timezone.localdate()`, pula busca de célula e dedup, e cria com `celula=None`. Portanto “dia da conversa” gravado é o dia local da abertura. `app/chamados/models.py:2052`, `app/chamados/models.py:2058`, `app/chamados/models.py:2085`
- Destino datado fornecido é conferido por `resolvedora_valida`; divergência comprovada levanta `ResolvedoraCruzada`. No reapontamento via dedup há nova validação. `app/chamados/models.py:2043`, `app/chamados/models.py:2064`

**Os seis sítios**

| Sítio | Motivo e dia recebidos | Destino recebido |
|---|---|---|
| `gerar_perguntas_disputa` | `motivo` iterado em `motivos_novos`, desde que exista template; dia prefere fallback da grade quando necessário para texto, depois contexto `[motivo]['data']`, depois data local de `ultima_batida.timestamp`; usa dia devolvido pelo derivador quando presente. `app/chamados/services/disputa_emissao.py:292`, `app/chamados/services/disputa_emissao.py:316`, `app/chamados/services/disputa_emissao.py:343`, `app/chamados/services/disputa_emissao.py:350`, `app/chamados/services/disputa_emissao.py:389` | `_ch_dest`, inicialmente `None`; deriva para motivos de destino obrigatório e para intervalo pertencente a `MOTIVOS_DO_DIA_SEM_FECHAR`. `app/chamados/services/disputa_emissao.py:367`, `app/chamados/services/disputa_emissao.py:379` |
| `_criar_pergunta_se_couber` | Repassa literalmente argumentos `motivo`, `data_evento`; tipo `hora`; guardas ausência e `_ja_perguntou`. `app/chamados/services/disputa_emissao.py:756`, `app/chamados/services/disputa_emissao.py:763`, `app/chamados/services/disputa_emissao.py:770` | Repassa argumento `chamado_destino`; pode levar `chamados_relacionados_ids`. `app/chamados/services/disputa_emissao.py:767` |
| `criar_disputa_ancora_de_chamado` | `motivo='ancora_ausente'`, tipo `data_hora`, omite `data_evento`, logo conversa datada com `localdate()` por `abrir`. `app/chamados/services/disputa_emissao.py:1160`, `app/chamados/models.py:2054` | Exatamente `chamado_limbo` recebido, exigido categoria `ancora_limbo` e estado não terminal. `app/chamados/services/disputa_emissao.py:1107`, `app/chamados/services/disputa_emissao.py:1166` |
| `acao_reabrir_disputa_admin` | `motivo_reabertura` do POST, validado em `DisputaSupervisao.MOTIVOS_CHOICES`; pergunta escolha; omite data, logo dia local da reabertura. `app/chamados/services/acoes_disputa.py:245`, `app/chamados/services/acoes_disputa.py:259`, `app/chamados/models.py:2054` | Omite destino: `None`; em seguida reabre o próprio chamado da disputa, se terminal. `app/chamados/services/acoes_disputa.py:259`, `app/chamados/services/acoes_disputa.py:265` |
| `declarar_hora_colab` | `motivo=_MOTIVO_POR_MARCO[(pos,tipo)]`, dia do marco faltante casado com hora; busca começa pelo `dia_do_fato(ch)` e lê marcos. `app/chamados/services/declaracao_texto.py:281`, `app/chamados/services/declaracao_texto.py:292`, `app/chamados/services/declaracao_texto.py:296`, `app/chamados/services/declaracao_texto.py:322` | O próprio `ch` sobre o qual colaborador declara; aceita só módulo em `CALADOS_POR_ISENCAO`. Se reusa pergunta órfã, há costura explícita por `reapontar_resolvedora(perg,ch,data_evento=dia)` antes da eventual abertura. `app/chamados/services/declaracao_texto.py:266`, `app/chamados/services/declaracao_texto.py:271`, `app/chamados/services/declaracao_texto.py:310` |
| `aplicar_declaracao` | `p['motivo']` e `p['dia']` da proposta; proposta extrai hora do followup, casa marco faltante e usa `_MOTIVO_POR_MARCO`. `app/chamados/services/declaracao_texto.py:572`, `app/chamados/services/declaracao_texto.py:576`, `app/chamados/services/declaracao_texto.py:600`, `app/chamados/services/declaracao_texto.py:611` | O próprio `ch`; ramo reusável não contém a costura explícita existente em `declarar_hora_colab`, apenas usa candidata e chama `abrir` quando não existe candidata. `app/chamados/services/declaracao_texto.py:608` |

O mapa de marcos é: início E → `saida_sem_entrada`; fim S → `orfao_14h`; intervalo ida S → `intervalo_saida`; intervalo volta E → `intervalo_volta`. `app/chamados/services/declaracao_texto.py:42`

Detalhamento dos argumentos do sítio `~770`:

- Intervalo dedicado: motivo S→`intervalo_saida`, demais→`intervalo_volta`, dia de cada `contexto_json.marcos_faltantes[].data`, destino `_chi` que tem categoria `intervalo_ausente`. `app/chamados/services/disputa_emissao.py:939`
- Perguntas pela ata: dia pela chave de catálogo do chamado; complementa todos marcos faltantes usando o mesmo chamado como destino. `app/chamados/services/disputa_emissao.py:966`
- Grupo por batida de origem: motivo dado por `classificar_falta`; data por `resolver_data_evento(...) or data_entrada`; destino primeiro chamado do grupo e restantes relacionados. `app/chamados/services/disputa_emissao.py:996`
- Sem batida origem: dia por `data_evento_do_contexto(ch.contexto_json)`; exige base; motivo por `classificar_falta`; destino `ch`; complementa os marcos do mesmo dia. `app/chamados/services/disputa_emissao.py:1030`
- Complementar marcos produz os quatro motivos do mapa e mantém destino recebido, aceitando apenas marco cujo dia coincide com `data_evento`. `app/chamados/services/disputa_emissao.py:805`

**Qual chamado cobra o mesmo dia**

Não existe nesses sítios uma função geral “motivo de pergunta → `modulo_origem` da cobrança”; o mecanismo escrito de compatibilidade usa **categoria/subcategoria do chamado → motivos atendidos**, e o derivador não filtra `modulo_origem`. `app/chamados/services/disputa_emissao.py:181`, `app/chamados/services/disputa_emissao.py:186`, `app/chamados/juizes.py:1589`

Os mapas escritos são:

- `MAPA_CATEGORIA_PARA_MOTIVO_DISPUTA`: `saida_sem_entrada`→mesmo motivo; `batida_ausente`, `turno_aberto_24h`, `turno_aberto_14h`, `turno_aberto`, `saida_nao_registrada`, `ausencia`→`orfao_14h`. `app/chamados/models.py:2466`
- `MAPA_SUBCATEGORIA_PARA_MOTIVO_DISPUTA`: `batida_orfan`, `batida_orfa`, `batida nao realizada`, `saida_pendente`→`orfao_14h`. `app/chamados/models.py:2478`
- `motivos_atendidos` transforma qualquer categoria/subcategoria pertencente a esses mapas em **todos quatro** motivos `MOTIVOS_DO_DIA_SEM_FECHAR`; quem não pertence atende nenhum. `app/chamados/juizes.py:1589`, `app/chamados/models.py:2502`
- O próprio comentário diferencia “que motivo o chamado É” dos mapas 1:1 de “que motivos ele RESPONDE”; chamado de turno cobre dia inteiro. `app/chamados/models.py:2486`

A cobrança viva do dia pelo juiz já existente é `cobranca_viva_do_dia(colaborador, modulo_origem, dia)`: módulo(s) precisa ser fornecido pelo chamador; filtra vivo + `(contexto_json.data_turno=dia OR celula.data=dia)`, ordena mais novo por `criado_em,pk` e confirma `dia_do_fato(fortes=True)==dia`. **Não infere módulo a partir do motivo da pergunta.** `app/chamados/juizes.py:1610`

Os módulos declarados como cobrança de dia incluem `batida_ausente`, `turno_aberto`, `turno_aberto_24h`, `intervalo_pendente`, todos com chave `data_turno`. `app/chamados/catalogo/modulos.py:103`

- Emissão canônica de ausência/furo usa `MODULO_ORIGEM='batida_ausente'`, lookup `colaborador + contexto_json.data_turno`, defaults categoria `Ausencia`, subcategoria `Batida nao realizada`, JSON com `data_turno` e marcos faltantes. `app/ponto/management/commands/detectar_ausencias.py:29`, `app/ponto/management/commands/detectar_ausencias.py:688`
- Emissor de turno crítico usa `modulo_origem='turno_aberto'` e JSON `data_turno`. `app/ponto/management/commands/processar_alertas_turno.py:324`
- Emissão `intervalo_pendente` foi aposentada: detector mantém reconciliação dos vivos legados e afirma que grade é autoridade; corpo termina com `continue`, sem emissão. `app/ponto/management/commands/detectar_intervalo_ausente.py:145`
- Reemissor `c2` é explicitamente `batida_ausente + contexto_json.data_turno`; costura num chamado existente vivo passa por gates diferentes de nascimento. `app/chamados/reconciliador.py:737`
- A ligação `chamado.celula` nasce pelo gate `ChamadoColaborador.abrir` usando `_celula_de`; esta consulta `CelulaDia(colaborador,data)` cujo dia vem de `dia_do_fato(...,fortes=True)`. `app/chamados/models.py:716`, `app/chamados/models.py:895`
- Dia forte prefere `chamado.celula.data`, depois chave declarada no catálogo, depois JSON `data_turno`; com `fortes=True` para antes dos fallbacks de batida/disputa/carimbo. `app/chamados/juizes.py:78`

`ancora_limbo` tem escopo **PERIODO**, chave `data_inicio`, dono cadastro; portanto não é uma cobrança de um colab-dia equivalente aos quatro motivos de marco. `app/chamados/catalogo/modulos.py:128`

Qual PK ou módulo efetivamente cobra **cada pergunta hoje**, ou se existe candidato para cada uma das 807, exige estado do banco, que não foi consultado. [não verificado]

## 29/10 23:13 · sessao 0c98fb6252a9 · - · - · origem=Ronald (tmux)

Leitura realizada na base `/home/ronald/codex_ws/e84a9463bfb582ed01e420ee31c3b3f1a97f1c61`; referências abaixo relativas a essa árvore.

**Quem escolhe e quem escreve o destino**

- No nascimento, `PerguntaDisputa.abrir` recebe o destino do chamador; não procura o chamado vivo. Se há destino e dia explícitos, consulta `resolvedora_valida`. Na identidade já existente `(colaborador,motivo,data_evento)`, pode reapontar diretamente para o destino recebido, consultar novamente o juiz e relavrar no mesmo `save`. Se não recebeu destino, conserva o destino existente; para uma nova pergunta, grava `None`. `app/chamados/models.py:2028-2086`.
- Para pergunta existente fora desse dedup de `abrir`, a porta de escrita é `reapontar_resolvedora`: recebe o chamado escolhido, preenche dia somente se ainda ausente, consulta `resolvedora_valida`, desfaz o dia em memória se recusado e salva destino + lavra juntos. Não escolhe candidato nem exige destino vivo por conta própria. `app/chamados/services/fio.py:740-801`.
- `declarar_hora_colab` já reaponta uma pergunta reutilizada que estava sem destino para o chamado `ch` no qual o colaborador está declarando; usa `reapontar_resolvedora(perg,ch,data_evento=dia)`. `app/chamados/services/declaracao_texto.py:310-323`.
- A migração por irmão vivo acontece em `ChamadoColaborador.reabrir`: se `recusa_da_reabertura` devolve `irmao_vivo`, chama `migrar_resolvedoras_vivas(self,irmao)`. Essa função percorre somente `velho.perguntas_que_resolvem`, seleciona as vivas, resolve duplicatas e chama `reapontar_resolvedora`. Portanto não busca perguntas sem destino listadas incidentalmente sob o pai. `app/chamados/models.py:1065-1083`; `app/chamados/services/fio.py:881-900`.

**Os juízes fazem perguntas diferentes**

- `cobranca_viva_do_dia(colaborador,modulo_origem,dia)` procura módulo(s) informado(s), status `VIVOS`, `contexto_json.data_turno == dia` **ou** `celula.data == dia`; confirma `dia_do_fato(...,fortes=True)` e devolve o mais novo por `-criado_em,-pk`. Não escreve destino. `app/chamados/juizes.py:1610-1626`.
- `resolvedora_valida` não escolhe chamado nem verifica vida ou compatibilidade de motivo. Compara o dia da pergunta ao dia declarado pelo módulo no catálogo; permite conversa sem dia, chamado sem escopo DIA ou dia desconhecido; para datas vizinhas permite se o turno cruza meia-noite ou essa condição é desconhecida. Recusa somente divergência demonstrada. `app/chamados/juizes.py:1687-1738`.
- `_derivar_chamado_destino` usa outro recorte: chamados `VIVOS` do colaborador, exclui categoria `disputa_supervisao`, exige motivo atendido pela categoria/subcategoria, exclui candidato que já tenha pergunta daquele motivo com `materializada_em IS NULL`, compara `dia_do_fato(ch)` com `data_pergunta` ou contexto e só aceita um candidato após desempate. Não chama C8. `app/chamados/services/disputa_emissao.py:174-237`.
- Seu desempate procura entrada de turno aberto e, se dois candidatos representam a mesma entrada, prefere estágio `turno_aberto_24h` a `turno_aberto`. Sem unicidade, conserva ambiguidade e devolve `None`. `app/chamados/services/disputa_emissao.py:75-146`; `app/chamados/services/disputa_emissao.py:219-227`.

**Já existe costura de órfã para o chamado vivo do dia**

Sim, por `reatar_pergunta_orfa`, alcançada por dois caminhos:

1. Evento: após `abrir` em `gerar_perguntas_disputa`, **somente** se `_criada`, destino NULL e motivo em `MOTIVOS_DO_DIA_SEM_FECHAR`. Executa em savepoint; falha é registrada e a pergunta continua órfã. `app/chamados/services/disputa_emissao.py:398-414`.
2. Reconciliação: `reconciliar_chamados_colab` chama `reconciliar_perguntas_orfas_colab`; este seleciona destino NULL, disputa aberta, `materializada_em IS NULL` e motivo reatável. `app/chamados/reconciliador.py:1264-1265`; `app/chamados/reconciliador.py:1302-1343`.

A docstring de `reatar_pergunta_orfa` ainda diz “HOJE SO O CRON A CHAMA”, mas o código do emissor acima já a chama; é comentário desatualizado. `app/chamados/reconciliador.py:1350-1354`; `app/chamados/services/disputa_emissao.py:404-409`.

A ordem efetiva dessa porta é:

- Derivar candidato vivo usando **dia da pergunta**, depois reapontar pela porta. `app/chamados/reconciliador.py:1365-1370`; `app/chamados/reconciliador.py:1441-1449`.
- Sem candidato vivo: procurar chamados encerrados **pelo mesmo dia**, categoria/subcategoria mapeando **exatamente** ao motivo; um histórico único é reapontado e passa pelo resolvedor de chamado resolvido. `app/chamados/reconciliador.py:1380-1394`; `app/chamados/reconciliador.py:1415-1440`.
- Sem nenhum histórico: tentar `lavrar_sem_turno_aberto`; se não lavrou, tentar `reemitir_chamado_do_dia`. Havendo dois históricos, retorna sem costurar; nem chega ao c2. `app/chamados/reconciliador.py:1395-1416`.

No c2:

- Só aceita `orfao_14h` e `saida_sem_entrada`; exige colaborador ativo e dia válido. `app/chamados/reconciliador.py:332`; `app/chamados/reconciliador.py:712-724`.
- A chave do chamado é fixa: `modulo_origem='batida_ausente'`, `contexto_json__data_turno=str(dia)`. Se já há um correspondente vivo, dispensa piso e protesto, mas ainda aplica isenção, ausência, veto da célula e motor. Não consulta `cobranca_viva_do_dia`. `app/chamados/reconciliador.py:737-778`.
- Chama `ChamadoColaborador.abrir` nessa chave; só costura se o resultado está `VIVOS`; termina em `reapontar_resolvedora`. `app/chamados/reconciliador.py:779-823`.

**Por que isso não garante costura das 807**

O número **807**, seus IDs e a classificação individual das recusas não foram confirmados nesta árvore: **[não verificado]**.

O código demonstra estas lacunas concretas, que impedem afirmar que “pergunta viva sob pai terminal” pertence automaticamente ao universo costurado:

- Vida não equivale à elegibilidade do cron: `materializada_em != NULL`, disputa fechada ou motivo fora das quatro opções excluem a pergunta antes de qualquer tentativa. O filtro não pergunta se o pai é terminal. `app/chamados/reconciliador.py:1311-1323`; `app/chamados/models.py:2595-2596`.
- Ao reencontrar uma pergunta existente, `_criada=False`; a costura pós-nascimento do emissor não executa. `app/chamados/models.py:2084`; `app/chamados/services/disputa_emissao.py:404`.
- Um chamado vivo existente pode ser excluído pelo derivador porque já tem pergunta do mesmo motivo sem carimbo; também pode faltar categoria/subcategoria reconhecida ou sobrar ambiguidade. `app/chamados/services/disputa_emissao.py:194-227`.
- O c2 cobre apenas dois motivos e o módulo `batida_ausente`; não é busca geral do chamado vivo de qualquer módulo que cobra o dia. `app/chamados/reconciliador.py:332`; `app/chamados/reconciliador.py:712`; `app/chamados/reconciliador.py:737-742`.
- Quando não há destino, a listagem continua no pai, sem testar seu status: `return p.chamado_destino_id or d.chamado_id`. `app/chamados/services/fio.py:97-117`.

Assim, o mecanismo já existe, mas tem universo e seletores mais estreitos que “toda pergunta viva listada sob pai terminal”; determinar quais dessas barreiras explicam **cada uma das 807** exige os registros dessas perguntas: **[não verificado]**.

## 29/10 23:08 · sessao 30188e4fd539 · o261c2 · leitura · origem=Code

**Religar o pai não separa competências:** o método obtém um pai VIVO e altera somente `DisputaSupervisao.chamado`; todas as perguntas sem destino passam a ancorar nele pelo mesmo fallback. Não há seleção por `data_evento` ou exportação nessa operação. (`app/chamados/models.py:1741–1782`; `app/chamados/services/fio.py:116–117`)

**Exportada e trancada têm efeitos diferentes nesta árvore.** A tranca pode calar uma pergunta; competência somente exportada continua viva. A antiga `dia_em_competencia_exportada` foi removida. (`app/chamados/services/competencia_trancada.py:3–11`; `app/chamados/juizes.py:1504–1528`)

Para ler as tabelas:

- **C6** significa `pergunta_viva`: verifica a competência de **`p.data_evento`** por `janela_atual` e `periodo_trancado`; cala a pergunta se houver tranca, salvo toque humano posterior. **Não consulta exportação vigente.** (`app/chamados/juizes.py:887–907`; `app/chamados/juizes.py:1005–1016`)
- **`q_vivas()` não equivale ao C6:** é um pré-filtro deliberadamente mais amplo. Exclui a via antiga `competencia_trancada` carimbada, mas **não aplica a tranca dinâmica por leitura**. (`app/chamados/models.py:2325–2332`; `app/chamados/models.py:2334–2353`)

Na fila do admin, os caminhos são estes:

| Caminho | Como inclui/lista/conta | Filtro de competência |
|---|---|---|
| `qs_vivos` → `qs_na_fila` | VIVOS; depois somente `COBRANCA`, excluindo `silenciado_ate > now`. Portanto, VIVO sozinho não garante presença na fila. (`app/chamados/services/painel.py:153–155`; `app/chamados/services/painel.py:186–188`) | **Nenhum** por pergunta, exportação ou tranca nesses métodos. |
| Lista principal, resumo e totais | A lista padrão corta por `COBRANCA` e verbos arquivado/histórico; totais usam `qs_na_fila`. (`app/chamados/services/painel.py:210–223`; `app/chamados/services/painel.py:314–338`; `app/chamados/services/painel.py:462–475`) | **Nenhum adicional** por competência. |
| Crédito da pergunta ao pai; verbos cobrar/validar | `creditos_perguntas` busca perguntas por destino **ou pai vivo**; aplica `q_vivas()`, exclui materializada com `VL_SIM` e, sem destino, credita o pai. A partição por verbos consome esses créditos. (`app/chamados/services/raiox.py:54–72`; `app/chamados/services/raiox.py:133–149`; `app/chamados/services/verbos.py:31–59`) | **Não exportação; não tranca dinâmica pelo C6.** |
| Placar, navbar e filas vizinhas | Placar/navbar usam contexto e partição; fila copiloto, inteligência e fila por causa usam `qs_vivos`, `qs_na_fila` ou a mesma partição. (`app/chamados/views.py:840–861`; `app/core/management/commands/lavrar_badge_navbar.py:39–42`; `app/chamados/services/fila.py:53–55`; `app/chamados/services/fila.py:88–100`; `app/chamados/services/na_fila.py:15–24`; `app/chamados/services/fila_por_causa.py:158–161`) | **Nenhum corte adicional** por exportação; herdam as seleções acima. |
| Fio/modal admin | Carrega perguntas das disputas **abertas sem filtrar C6**; nas fechadas, exige C6. Agrupa por `chamado_que_lista`; o modal percorre as anexadas. (`app/chamados/services/fio.py:218–240`; `app/templates/chamados/partials/modal_fio.html:154–155`) | **Não exportação.** Tranca via C6 somente no filtro das disputas fechadas. |
| Destaque e ordenação do fio | `tem_pergunta_viva` exige `.viva` e ação possível; os cards correspondentes sobem. (`app/chamados/services/fio.py:279–281`; `app/chamados/services/fio.py:409–415`) | **Não exportação; tranca via C6.** |
| Detalhe local do pai | Busca disputa aberta do chamado e filtra perguntas pelo juiz exato. (`app/chamados/services/acoes_chamado.py:335–348`) | **Não exportação; tranca via C6.** |
| Contador do modal | Ativos por `qs_na_fila`; disputas por `fechada_em IS NULL`. (`app/chamados/views.py:900–912`) | **Não** pela competência das perguntas. |

Na cobrança e nas portas do colaborador:

| Caminho | Seleção e efeito | Filtro de competência |
|---|---|---|
| Lembrete diário | Seleciona **disputas abertas**, carimba o aviso diário e envia push ao colaborador e agregado à supervisão. (`app/ponto/management/commands/alertar_disputas_pendentes.py:33–35`; `app/ponto/management/commands/alertar_disputas_pendentes.py:48–108`) | **Não exportação nem tranca:** não seleciona pelas perguntas. |
| Push de abrir/agregar disputa | `_enviar_push_disputa_colab` envia webpush/FCM; sua guarda é o contexto de silêncio do push. (`app/chamados/services/disputa_emissao.py:1483–1507`) | **Não** por competência. |
| Prazo do pai religado | Os defaults do novo pai recebem `prazo_sla=agora+20min`. O próprio corpo do religamento salva o vínculo e registra evento, sem chamada de push. (`app/chamados/models.py:1749–1782`; `app/chamados/catalogo/motor.py:45–51`) | **Não** por competência. |
| Cobrança por pessoa/chat | Pendentes = `q_vivas()` + C6 + sem resposta. Conta perguntas/dias, grava prazo padrão de 48h nos pais e envia FCM/webpush. (`app/chamados/services/aviso_pendencia.py:7–13`; `app/chamados/services/cobranca.py:75–95`; `app/chamados/services/cobranca.py:163–187`) | **Não exportação; tranca via C6.** |
| Aviso após texto livre | Enumera essas pendentes e informa que o questionário continua aguardando resposta. (`app/chamados/services/aviso_pendencia.py:16–24`) | **Não exportação; tranca via C6.** |
| Recobrança do questionário | A view exige alguma pergunta sem resposta viva; o serviço conta **todas sem resposta**, renova prazo de 20min e envia push. (`app/chamados/views.py:1413–1425`; `app/chamados/services/disputa_emissao.py:1444–1464`) | **Não exportação.** C6 na decisão da view; o contador do serviço não separa competências nem vitalidade individual. |
| Botão Cobrar individual | Valida chamado/área, limita aviso diário por pessoa e envia FCM. (`app/chamados/views_cobrar.py:24–57`) | **Não:** não consulta a competência das perguntas. |
| Cobrar em massa e prévia | Aplica `muda_a_folha(ch)` antes de agrupar por pessoa. (`app/chamados/views_cobrar.py:68–114`) | **Sim, mas pelo dia do CHAMADO**, obtido do módulo/contexto; **não por `p.data_evento`**. Sem dia, exclui o chamado inteiro. (`app/chamados/services/cobranca_folha.py:29–35`; `app/chamados/services/cobranca_folha.py:54–71`) |
| Escalonamento supervisão | Seleciona pai `disputa_supervisao` em `COBRANCA`, não silenciado e com disputa aberta; cobra por prazo vencido e idade. (`app/ponto/management/commands/escalonar_chamados_supervisao.py:42–59`; `app/ponto/management/commands/escalonar_chamados_supervisao.py:93–198`) | **Não exportação nem tranca das perguntas.** |
| Alerta genérico de SLA | Percorre `qs_na_fila` e aplica regras de prazo/alerta. (`app/chamados/management/commands/alertar_chamados_sla.py:24–52`) | **Nenhum filtro próprio** por competência. |
| `perguntas_do_app` / API GET perguntas | Busca perguntas do colaborador por `q_vivas()` e confirma C6; a API usa essa lista e o pai como fallback da âncora. (`app/chamados/services/pergunta_no_app.py:39–44`; `app/api/views.py:2603–2604`; `app/api/views.py:2620–2646`) | **Não exportação; tranca via C6.** A seleção do helper não exige pai vivo nem disputa aberta. |
| PWA: formulário e POST | Monta fio com `so_janela=True`; formulário exige `p.viva`; POST exige dono e C6. (`app/chamados/views.py:566–569`; `app/templates/chamados/partials/_card_atendimento.html:73–78`; `app/chamados/views.py:1238–1246`) | **Não exportação; tranca via C6.** |
| API POST resposta | Busca por dono, exige C6 e delega ao core. (`app/api/views.py:2680–2698`) | **Não exportação; tranca via C6.** |
| API chamados v2 | Usa janela do colaborador, mas anexa e serializa **todas as perguntas das disputas abertas**, usando destino vivo ou pai. (`app/api/views.py:1015–1016`; `app/api/views.py:1028–1049`; `app/api/views.py:1158–1178`) | **Não exportação; não C6 na seleção dessas perguntas.** |
| Pauta de pessoas cobradas e caladas | `_calados` seleciona `q_vivas()`, sem resposta, disputa aberta, e verifica prazo dos pais; alimenta `linhas_para_a_pauta`. (`app/chamados/services/cobranca.py:228–257`) | **Não exportação; não confirma tranca dinâmica pelo C6.** |

A janela do colaborador **não é uma proteção por competência de `data_evento`**: usa `criado_em` do chamado, com exceção por perguntas `q_vivas()` com destino não nulo. Assim, um pai criado agora pode carregar perguntas antigas anexadas pelo fio. (`app/chamados/services/janela_colab.py:49–74`; `app/chamados/services/fio.py:218–239`)

Na cobrança em massa, o contexto novo do religamento contém apenas `origem`, `disputa` e `pai_anterior`; portanto esse pai pode ser recusado inteiro por `sem_data`. Isso não constitui exclusão seletiva das perguntas exportadas de uma disputa mista. (`app/chamados/models.py:1762`; `app/chamados/services/cobranca_folha.py:54–61`)

Nos caminhos que julgam ou contam:

| Caminho | Julgamento/contagem e filtro |
|---|---|
| Vitalidade, `.viva`, `perguntas_vivas_de`, `fio_vivo` | Usam C6: **tranca por `data_evento`, sem exportação**. (`app/chamados/models.py:2355–2366`; `app/chamados/juizes.py:1097–1111`) |
| `estado` | Chama C6, mas pode retornar `PENDENTE`/`RESPONDIDA` mesmo quando a pergunta está morta sem carimbo; não substitui `.viva`. Sem consulta de exportação. (`app/chamados/models.py:2201–2208`) |
| `respostas_sem_veredito` / `pode_fechar` | A lista exclui dias trancados, salvo `ignorar_tranca=True`; fechar exige nenhuma resposta sem veredito e nenhum fio vivo. **Sem exportação.** (`app/chamados/models.py:1425–1439`) |
| Fila global de validação; classificação/lote/API mensageria | Respondidas com texto, `q_vivas()` e confirmação C6; classificação e contagens consomem essa fila. **Sem filtro adicional de exportação; tranca via C6.** (`app/chamados/juizes.py:1121–1127`; `app/chamados/services/validacao.py:858–880`; `app/api/views_mensageria.py:539–550`) |
| Validação individual e materialização | **Não barram por competência exportada/trancada** nessas portas; a recusa por tranca foi removida. Materialização seleciona respondida + validada + não materializada. (`app/chamados/services/validacao.py:348–376`; `app/chamados/services/materializacao.py:1063–1073`; `app/chamados/services/materializacao.py:1120–1122`; `app/chamados/services/materializacao.py:1286–1296`) |
| Pendentes após materializar | Conta `perguntas_vivas_de` e consulta `pode_fechar`: **C6/tranca, sem exportação**. (`app/chamados/services/materializacao.py:1230–1233`) |
| Juiz do lastro / `veredito_lavrado` | Lavra `_pergunta_materializada_de_fato`; reconhece a **via carimbada** de tranca, mas não é o filtro dinâmico do C6. **Sem exportação.** (`app/chamados/veredito_lavrado.py:219–231`; `app/chamados/juizes.py:603–617`) |
| Cron de lavra stale | Seu universo exige **destino vivo**; a pergunta sem destino ancorada somente no pai não entra por esse filtro. **Sem exportação.** (`app/chamados/management/commands/lavrar_perguntas_stale.py:56–58`) |
| Relatórios de disputa | Classifica disputas abertas pelas perguntas C6: **tranca, sem exportação**. (`app/chamados/relatorios_disputa.py:10–22`; `app/chamados/relatorios_disputa.py:39–50`) |
| `fora_da_folha` / `o_que_segura` | Recortam a competência consultada e classificam fio por perguntas ligadas à célula, usando `.viva`. **Não excluem por exportação; herdam tranca.** (`app/folha/fora_da_folha.py:46–73`; `app/folha/fora_da_folha.py:119–173`; `app/folha/fora_da_folha.py:188–213`) |
| Prévia da folha | Carrega por `data_evento` na competência pedida; prioriza qualquer texto de resposta e, sem texto, usa `fio_vivo`. **Sem exportação; tranca via C6 somente nesse segundo ramo.** (`app/folha/previa.py:64–67`; `app/folha/previa.py:139–146`) |
| Vigias de fio | `viva_em_disputa_fechada` e `viva_sob_pai_terminal` usam `q_vivas()` + C6, **sem exportação**. Após religar, sai especificamente do segundo universo porque o pai está VIVO. (`app/chamados/services/vigia_fio.py:352–362`; `app/chamados/services/vigia_fio.py:401–413`) |

O recorte `_dentro_da_janela_viva`, usado em degraus de vitalidade/lastro, é temporal; não consulta exportação e não equivale a excluir competência exportada. (`app/chamados/juizes.py:517–544`; `app/chamados/juizes.py:616–617`; `app/chamados/juizes.py:1066–1077`)

Quanto ao mecanismo pedido: **não há, no religamento e nos leitores acima, uma máscara por exportação que mantenha fora da fila e da cobrança somente essas perguntas já vivas.** O wrapper exige existir **alguma** viva sem destino e religa a disputa inteira; não particiona as demais. (`app/chamados/models.py:1788–1795`; `app/chamados/services/fio.py:116–117`)

Os mecanismos próximos têm limites concretos:

- **Tranca:** atua individualmente pelo `data_evento`, mas exportada não significa trancada; além disso, os caminhos que usam apenas `q_vivas()` ou a disputa aberta não aplicam uniformemente essa leitura. (`app/ponto/services/fechamento.py:1432–1448`; `app/chamados/juizes.py:887–907`; `app/chamados/models.py:2325–2332`)
- **Encerramento individual:** existe `fechar_por_competencia_trancada(autor)`, que escreve via/carimbo na pergunta; seu corpo não verifica exportação. É alteração do estado da pergunta, não máscara de exportadas vivas; o serviço atual de tranca deixou de carimbá-las. (`app/chamados/models.py:2230–2242`; `app/chamados/services/competencia_trancada.py:9–11`)
- **Silêncio do chamado:** `silenciado_ate` retira o chamado inteiro de `qs_na_fila`; não distingue irmãs de competências diferentes. (`app/chamados/services/painel.py:186–188`)
- **Separação no passivo O261:** a sonda identifica exportadas por exportação vigente, deriva a competência de `data_evento` e mede disputas mistas, mas não instala exclusão em pergunta ou leitor. (`logs/o261/passivo_pai_terminal.py:35–44`; `logs/o261/passivo_pai_terminal.py:69–88`)
- **Lote de órfãs enterradas:** separa exportadas para pauta antes de reabrir, mas seu universo é de perguntas carimbadas `fantasma`; não protege irmãs já vivas ao religar uma disputa. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:67–69`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:92–102`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:140–156`)

## 29/10 23:08 · sessao e8ed830a5577 · o261c2 · verificador · origem=Code

**Encontrei falhas funcionais e lacunas de cobertura; o diff não garante o nascimento sob chamado vivo do dia.** [models.py:1749](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1749), [models.py:2176](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2176)

A análise foi estática; testes, interleavings concorrentes e presença atual dos estados no banco **[não verificado]**. Nenhum arquivo foi alterado **[não verificado]**. [nao verificado]

1. **A porta procura um container da pessoa, sem perguntar pelo chamado vivo do dia.**  
   Entrada: pergunta sem destino de 15/10, disputa fechada, pai resolvido e cobrança `batida_ausente` já viva daquele colaborador/dia. Ao desfazer o carimbo sem lastro, a cadeia reabre a disputa e procura/cria outro `disputa_supervisao`, mantendo a pergunta sem destino. A consulta nova não recebe dia nem célula; ignora a cobrança existente. Resultado: outro chamado vivo, em vez de costura na cobrança do fato. [relavra_via.py:222](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/relavra_via.py:222), [models.py:1819](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1819), [models.py:1749](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1749)

   A autoridade existente escolhe a cobrança pelo dia forte e pelo desempate “mais nova”; a costura de pergunta existente tem porta própria. O pai criado pela cura recebe contexto sem `data_turno`, portanto tampouco é reconhecido pelo juiz forte como chamado daquele dia. **Classe: derivação paralela de fonte única; pedido (C) incompleto.** [juizes.py:1610](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/juizes.py:1610), [fio.py:740](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/fio.py:740), [models.py:1762](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1762), [juizes.py:118](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/juizes.py:118)

2. **O nascimento continua quando o religamento foi recusado.**  
   Entrada: disputa aberta sob pai terminal; existe chamado da mesma chave, terminal, agrupado sob outro pai vivo. `ChamadoColaborador.abrir` devolve esse filho pelo ramo AGRUPADO-EM. `religar_pai_vivo` recusa e retorna `False`; `PerguntaDisputa.abrir` ignora o resultado e cria a pergunta sem destino sob o pai terminal original. **Classe: pré-requisito não imposto pelo gate.** [models.py:689](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:689), [models.py:1766](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1766), [models.py:2176](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2176)

3. **O dedup retorna antes do gate novo.**  
   Entrada: fato existente sem destino, disputa aberta e pai resolvido. A célula correta encontrada para o colaborador/dia acusa; o gate reaponta a célula, relavra e devolve a pergunta existente antes de religar o pai. Resultado: a pergunta pode voltar a viver sob pai terminal. Mesmo uma existente já viva continua sendo devolvida nessa condição. **Classe: cura incompleta na origem; caminho que escapa do gate.** [models.py:2147](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2147), [models.py:2158](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2158), [models.py:2171](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2171)

4. **Reabrir a pergunta numa disputa já aberta não assegura pai vivo.**  
   Entrada: disputa aberta sob pai resolvido, pergunta sem destino absorvida por ausência; a ausência é rejeitada e nenhuma outra cobre o dia. O materializador devolve a pergunta a pendente e chama `reabrir_sistema`; essa porta retorna imediatamente porque a disputa já está aberta, sem alcançar o religamento. Resultado: pergunta viva, pai ainda terminal. **Classe: cobertura incompleta das reaberturas.** [materializacao.py:615](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/materializacao.py:615), [materializacao.py:628](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/materializacao.py:628), [models.py:1805](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1805)

5. **A trava do religamento termina antes do nascimento.**  
   Cenário concorrente: D está aberta, suas perguntas antigas estão validadas e o pai é terminal. T1 religa D ao novo pai N e sai do `atomic`; antes do INSERT da pergunta nova, T2 fecha N. A guarda S127 permite esse fechamento porque existem perguntas antigas, mas nenhuma está viva. T1 então insere a pergunta viva sob N já fechado. O emissor comum gera perguntas depois de retornar do `atomic` do núcleo da disputa. **Classe: ordem de trava/transação; pré-requisito não preservado até a escrita.** [models.py:1782](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1782), [models.py:2178](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2178), [juizes.py:1817](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/juizes.py:1817), [disputa_emissao.py:618](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:618)

6. **Uma falha no religamento deixa reabertura parcial e impede o retry.**  
   Entrada concreta possível no legado: pai terminal com `urgencia_original='critica'`. A nova porta copia esse valor; `ChamadoColaborador.abrir` o rejeita antes do dedup. Entretanto, `reabrir_sistema` já salvou a disputa aberta. Uma segunda tentativa retorna antes de religar, porque ela agora está aberta. O estoque histórico desse valor é documentado; sua presença atual **[não verificado]**. **Classe: transação incompleta; retry não recupera a operação.** [models.py:1761](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1761), [models.py:659](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:659), [juizes.py:1850](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/juizes.py:1850), [models.py:1815](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1815), [models.py:1805](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1805)

7. **Há cópia da redução registrada como C15.**  
   Para o queryset de perguntas sem destino, a porta calcula `any(pergunta_viva(...))` localmente, repetindo exatamente `fio_vivo`, cuja documentação atribui o universo ao chamador. O booleano coincide hoje; a incompletude é estrutural: a mesma pergunta passa a ter dois sítios de decisão. **Classe: segundo juiz que duplica autoridade registrada; LEI-AKITA 2.** [models.py:1793](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1793), [juizes.py:1102](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/juizes.py:1102), [core/juizes.py:241](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/core/juizes.py:241)

8. **O teste do carimbo não morde contador desconectado.**   [nao verificado]
   A fixture desse teste cria disputas sem pai. Logo `viva_sob_pai_terminal` vale zero, e trocar o valor escrito pelo comando por uma constante `0` preservaria o verde. O teste unitário do contador não cobre essa desconexão no comando. **Classe: selo sem mordida na integração.** [test_o261_vigia_fio.py:93](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_vigia_fio.py:93), [test_o261_vigia_fio.py:690](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_vigia_fio.py:690), [test_o261_vigia_fio.py:701](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_vigia_fio.py:701)

**Censo dos caminhos**

- **Nascimento de pergunta:** seis emissores chegam a `PerguntaDisputa.abrir`: [disputa_emissao.py:389](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:389), [disputa_emissao.py:770](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:770), [disputa_emissao.py:1160](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:1160), [acoes_disputa.py:265](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/acoes_disputa.py:265), [declaracao_texto.py:322](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/declaracao_texto.py:322), [declaracao_texto.py:611](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/declaracao_texto.py:611). O escape está dentro do gate, no retorno de dedup e na recusa ignorada descritos acima. [models.py:2171](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2171)
- **Nascimento da disputa/vínculo:** núcleo comum e dois ancoradores manuais criam a disputa com chamado previamente obtido pela porta. [disputa_emissao.py:523](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:523), [disputa_emissao.py:927](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:927), [disputa_emissao.py:1147](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:1147). O novo escritor posterior ao nascimento é `religar_pai_vivo`. [models.py:1774](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1774)
- **Reabertura da disputa:** `reabrir_admin` e `reabrir_sistema`; a ação administrativa alcança o gate pelo nascimento da confirmação. [models.py:1678](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1678), [models.py:1797](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1797), [acoes_disputa.py:256](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/acoes_disputa.py:256).
- **Chamadores da reabertura sistêmica:** ressuscitação, desfazer carimbo, devolução de validação, rejeição de ausência e comando de órfãs. [models.py:1189](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1189), [relavra_via.py:222](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/relavra_via.py:222), [validacao.py:200](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/validacao.py:200), [materializacao.py:631](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/materializacao.py:631), [reabrir_orfas_enterradas.py:154](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/management/commands/reabrir_orfas_enterradas.py:154).
- **Retorno da pergunta à vida sem nascimento:** correção de questionário, fantasma, resposta nova e competência reaberta não passam pelo novo gate. Há caminhos que tentam reabrir o chamado separadamente, mas isso não cobre o caso da disputa já aberta do achado 4. [disputa_emissao.py:1327](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:1327), [models.py:2269](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:2269), [disputa_emissao.py:1265](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/disputa_emissao.py:1265), [competencia_trancada.py:247](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/competencia_trancada.py:247).

**RED dos testes — conclusão por inspeção; execução [não verificado]** [nao verificado]

| Teste | Sem a mudança correspondente | [nao verificado]
|---|---|
| Nascimento sem destino | Falha na âncora viva; não exige chamado do dia. [linha 74](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_pai_terminal.py:74) |
| Pai vivo permanece | Passa sem a cura; controle negativo. [linha 85](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_pai_terminal.py:85) |
| Destino vivo não religa | Passa sem a cura; controle negativo. [linha 95](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_pai_terminal.py:95) |
| Idempotência | Falha pela trilha ausente; cobre duas chamadas sequenciais de motivos diferentes. [linha 111](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_pai_terminal.py:111) |
| Reabertura sistêmica | Falha sem religamento; só cobre disputa inicialmente fechada. [linha 129](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_pai_terminal.py:129) |
| Contador do vigia | Morde contador sempre zero, com transição esperada `1 → 0`. [linha 142](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_pai_terminal.py:142) |
| Carimbo | Morde ausência da chave; não morde valor constante zero no comando. [linha 701](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/tests/test_o261_vigia_fio.py:701) |

Dois religamentos sobre a mesma disputa são serializados pela nova trava; essa proteção termina antes do nascimento, conforme o achado 5. [models.py:1743](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/models.py:1743)

O vigia lê `pergunta_viva` e o conjunto oficial `VIVOS`, publica esperado zero e chega à comparação genérica do painel; não identifiquei defeito nessa cobrança do pedido (D). [vigia_fio.py:398](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/services/vigia_fio.py:398), [vigiar_fio_morto.py:203](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/chamados/management/commands/vigiar_fio_morto.py:203), [fio_estado.py:71](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/bin/fio_estado.py:71)

C7 permanece registrado em `DisputaSupervisao.pode_fechar`; a duplicação encontrada é C15. Não há registro novo em `core/juizes.py` neste diff que acione JUIZ-NOVO. [core/juizes.py:233](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/core/juizes.py:233), [core/juizes.py:241](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/app/core/juizes.py:241), [CLAUDE.md:86](/home/ronald/codex_ws/1ae59d8c11d75477f8d934220af74194990678fe/CLAUDE.md:86). Violação funcional adicional por relógio, alteração de gravado exportado/L-092 ou pouso sem pré-requisito **[não verificado]**.

## 29/10 23:06 · sessao 057bb4011ce7 · - · - · origem=Ronald (tmux)

Julgamento e contagem — evidências para consolidação:

| Caminho | Exportação da competência | Tranca/fechamento |
|---|---|---|
| `pergunta_viva` / `_pergunta_viva_calc` | Não consulta exportação. O antigo `dia_em_competencia_exportada` foi removido, com justificativa expressa. `app/chamados/juizes.py:1005`, `app/chamados/juizes.py:1504` | Sim: `dia_da_pergunta_trancado` mata antes de resposta/carimbo; deriva competência de `data_evento`, lê `periodo_trancado` e preserva dias tocados por humano após a tranca. `app/chamados/juizes.py:1010`, `app/chamados/juizes.py:887`, `app/chamados/juizes.py:897`, `app/chamados/juizes.py:907`, `app/chamados/juizes.py:945` |
| `q_vivas` | Não consulta exportação. `app/chamados/models.py:2334` | Exclui via `competencia_trancada` materializada; **não implementa a tranca por leitura**: é superset deliberado do C6. `app/chamados/models.py:2325`, `app/chamados/models.py:2343`, `app/chamados/models.py:2350` |
| `PerguntaDisputa.viva` | Herda ausência de filtro de exportação do C6. `app/chamados/models.py:2366` | Delega ao C6, portanto herda tranca por leitura. `app/chamados/models.py:2366` |
| `PerguntaDisputa.estado` | Sem consulta de exportação. `app/chamados/models.py:2194` | Chama C6, mas pode devolver `PENDENTE`/`RESPONDIDA` mesmo quando C6 diz morta sem carimbo; usar `estado` sozinho não equivale a usar `.viva`. `app/chamados/models.py:2201`, `app/chamados/models.py:2208` |
| `perguntas_vivas_de` e `fio_vivo` | Sem filtro adicional de exportação. `app/chamados/juizes.py:1097`, `app/chamados/juizes.py:1102` | Reduzem `pergunta_viva`, herdando tranca. `app/chamados/juizes.py:1099`, `app/chamados/juizes.py:1111` |
| `respostas_sem_veredito` / `pode_fechar` / fechamento da disputa | Não consultam exportação. `app/chamados/models.py:1428`, `app/chamados/models.py:1439` | A lista exclui dias trancados, salvo `ignorar_tranca=True`; `pode_fechar` exige nenhuma resposta sem veredito e nenhum fio vivo. `app/chamados/models.py:1432`, `app/chamados/models.py:1439` |
| Fila global de validação `perguntas_respondidas_a_validar` | Não consulta exportação; seleciona respondidas com texto via `q_vivas` e confirma C6. `app/chamados/juizes.py:1121`, `app/chamados/juizes.py:1127` | Herda tranca pelo C6. `app/chamados/juizes.py:1127` |
| `fila_validar_classificada`, contagens por classe/chamado e API mensageria de validação | Não acrescentam filtro de exportação; usam fila global e classificam resposta/delta/conflito de batida. `app/chamados/services/validacao.py:791`, `app/chamados/services/validacao.py:817`, `app/chamados/services/validacao.py:867`, `app/chamados/services/validacao.py:875`, `app/api/views_mensageria.py:539` | Herdam tranca quando usam a fila global; `fila_validar_classificada(qs=...)` aceita universo fornecido pelo chamador. `app/chamados/services/validacao.py:867` |
| Validação individual/lote | Individual não barra por competência exportada/trancada; guarda de tranca foi expressamente removida. Lote reclassifica por resposta e chama a mesma ação individual. `app/chamados/services/validacao.py:348`, `app/chamados/services/validacao.py:362`, `app/chamados/services/validacao.py:681`, `app/chamados/services/validacao.py:692` | Ato humano não é barrado pela tranca nesse caminho. `app/chamados/services/validacao.py:362` |
| Materialização após validação | Seleção é respondida + validada + não materializada; a porta declara não ter portão de competência. `app/chamados/services/materializacao.py:1071`, `app/chamados/services/materializacao.py:1286` | Recusa por tranca foi removida. A cauda conta perguntas vivas por C6 e usa `pode_fechar`. `app/chamados/services/materializacao.py:1120`, `app/chamados/services/materializacao.py:1230` |
| Juiz do lastro `_pergunta_materializada_de_fato` / cache `veredito_lavrado` | Não consultam exportação; lavra chama o juiz do lastro. `app/chamados/juizes.py:588`, `app/chamados/veredito_lavrado.py:219` | Reconhece a **via carimbada** `VIA_COMPETENCIA_TRANCADA` como absolvição; não é o mesmo filtro de tranca por leitura do C6. `app/chamados/juizes.py:603`, `app/chamados/juizes.py:613` |
| `creditos_perguntas` do raio X/placar | Seleciona perguntas por destino **ou pai vivo**, aplica `q_vivas` e exclui materializadas com cache `VL_SIM`; não consulta exportação nem confirma C6. `app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133` | Não executa tranca por leitura; só exclusões presentes em `q_vivas` e cache. `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133` |
| Cron `lavrar_perguntas_stale` | Sem filtro de exportação, mas universo exige **chamado_destino vivo**: pergunta sem destino ancorada no pai não entra por esse filtro. `app/chamados/management/commands/lavrar_perguntas_stale.py:56` | Mesmo universo; lavra juiz do lastro. `app/chamados/management/commands/lavrar_perguntas_stale.py:85` |
| `relatorios_disputa.classificar/coletar` | Sem filtro de exportação; disputas abertas são classificadas pelas perguntas vivas. `app/chamados/relatorios_disputa.py:17`, `app/chamados/relatorios_disputa.py:39` | Herda C6. `app/chamados/relatorios_disputa.py:17` |
| `folha/fora_da_folha` e `o_que_segura` | Recortam competência solicitada e dias retidos, mas não excluem por exportação vigente. Perguntas são carregadas por célula. `app/folha/fora_da_folha.py:51`, `app/folha/fora_da_folha.py:119`, `app/folha/fora_da_folha.py:188` | Fio usa `.viva` e `estado`, herdando tranca C6; somas por fio/grupo usam essa classificação. `app/folha/fora_da_folha.py:66`, `app/folha/fora_da_folha.py:154`, `app/folha/fora_da_folha.py:204` |
| `folha/previa` | Carrega perguntas cujo `data_evento` está na competência pedida, sem filtro de exportação. `app/folha/previa.py:64` | `_material` prioriza qualquer texto de resposta, inclusive de pergunta morta; só sem texto usa `fio_vivo` e herda tranca. `app/folha/previa.py:139` |
| Vigias `viva_em_disputa_fechada`, `viva_sob_pai_terminal` | Usam `q_vivas` + C6, sem exportação. `app/chamados/services/vigia_fio.py:352`, `app/chamados/services/vigia_fio.py:358`, `app/chamados/services/vigia_fio.py:401`, `app/chamados/services/vigia_fio.py:409` | Herdam tranca C6; após religar em pai vivo, a pergunta deixa especificamente o universo de `viva_sob_pai_terminal`. `app/chamados/services/vigia_fio.py:403` |
| Vigia `censo` de fio morto | Universo: chamados vivos com célula, células acusadoras e perguntas dessas células; sem filtro de exportação. `app/chamados/services/vigia_fio.py:186`, `app/chamados/services/vigia_fio.py:228`, `app/chamados/services/vigia_fio.py:244` | `fio_do_dia` delega `fio_vivo`; herda C6. `app/chamados/services/vigia_fio.py:164`, `app/chamados/services/vigia_fio.py:260` |

`_dentro_da_janela_viva` é um recorte **temporal**, formado pela janela anterior e atual de hoje; não consulta exportação ou tranca. Ele participa do juiz do lastro e de um degrau da vitalidade, sem equivaler a “competência exportada”. `app/chamados/juizes.py:517`, `app/chamados/juizes.py:538`, `app/chamados/juizes.py:544`, `app/chamados/juizes.py:616`, `app/chamados/juizes.py:1066`.

Sobre exclusão seletiva: há mecanismo real por **tranca**, que pode calar perguntas de certas competências da mesma disputa por leitura de `data_evento`; porém exportada e trancada não são equivalentes: o juiz de fechamento declara que aprovada/exportada/paga são marcos e “fechada = trancada”. `app/chamados/juizes.py:887`, `app/chamados/juizes.py:907`, `app/ponto/services/fechamento.py:1441`.

Existe seleção por exportação no lote específico `reabrir_orfas_enterradas`: exportadas vão à pauta em vez de serem reabertas. Isso não instala uma máscara de fila/cobrança sobre irmãs já vivas de uma disputa religada; o universo é restrito a perguntas carimbadas como `fantasma` e o APPLY reabre apenas as selecionadas. `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:92`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:98`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:140`.

A via `fechar_por_competencia_trancada` existe por pergunta, mas declara competência **trancada**, não exportada, e escreve via/carimbo; `pendencia_admin` é explicitamente pendência de admin e não encerramento. Nenhum desses é máscara seletiva por exportação. `app/chamados/models.py:2230`, `app/chamados/models.py:2238`, `app/chamados/models.py:2022`, `app/chamados/models.py:2034`.

## 29/10 23:06 · sessao a5260489adb0 · - · - · origem=Ronald (tmux)

Consolidado de cobrança (somente leitura):

| Caminho | Seleção/efeito | Competência exportada de `p.data_evento`? |
|---|---|---|
| Lembrete diário | Disputas com `fechada_em IS NULL`; carimbo diário e push colaborador; agregado à supervisão. `app/ponto/management/commands/alertar_disputas_pendentes.py:33-35`, `:48-70`, `:87-108`; cron `app/config/crons.py:719`. | **Não**. Não consulta perguntas, exportação ou tranca. |
| Cobrança por pessoa / chat | `_pendentes` delega a `perguntas_pendentes_do_colab`, que aplica `q_vivas`, `pergunta_viva` e sem resposta. `app/chamados/services/cobranca.py:45-48`; `app/chamados/services/aviso_pendencia.py:7-13`. Preview conta datas, perguntas e pais, sem filtro adicional: `cobranca.py:75-95`. Escreve prazo 48h nos pais e envia FCM/webpush: `:163-187`, `:98-119`. Chat registra serviço em `app/core/intake.py:65-68` e executa em `app/core/views_ia.py:828-846`. | **Não exportada**. **Sim tranca**, herdada de `pergunta_viva`; detalhes abaixo. |
| Aviso após texto livre | `aviso_texto_livre` enumera as perguntas pendentes e dias e informa que seguem aguardando resposta. `app/chamados/services/aviso_pendencia.py:16-24`. Usado no web em `app/chamados/views.py:1119` e API em `app/api/views.py:996`, `:2271`. | **Não exportada**; tranca via juiz. |
| Recobrança no fio | View distingue perguntas sem resposta vivas por `pergunta_viva`, depois chama serviço: `app/chamados/views.py:1413-1425`. Serviço conta **todas sem resposta**, renova prazo 20min no chamado recebido e envia push: `app/chamados/services/disputa_emissao.py:1444-1464`; regra 20min em `app/chamados/catalogo/motor.py:45-51`. | **Não exportada**. View exige ao menos uma pergunta viva; serviço não separa competências nem reavalia vitalidade de cada pergunta no contador. |
| Botão Cobrar individual | Busca chamado, valida área, limita um aviso/dia por pessoa e envia FCM. `app/chamados/views_cobrar.py:24-57`. | **Não**: sequer consulta perguntas/competência. |
| Cobrar em massa e prévia | Parte da fila/verbos, chamados VIVOS; aplica `muda_a_folha(ch)` antes de agrupar por pessoa: `app/chamados/views_cobrar.py:68-97`; prévia `:102-114`; envio `:116-133`. | **Filtra exportação do dia do CHAMADO**, não de cada pergunta: `app/chamados/services/cobranca_folha.py:29-35`, `:54-71`. Não separa perguntas de competências diferentes. Sem data, exclui chamado inteiro: `:54-61`. |
| Prazo ao religar | Defaults do novo pai recebem `prazo_sla=agora+20min`: `app/chamados/models.py:1749-1764`; regra `app/chamados/catalogo/motor.py:45-51`. | **Não**. Defaults do novo pai só têm contexto `origem/disputa/pai_anterior`, sem dia: `models.py:1762`; logo filtro em massa pode recusá-lo inteiro como `sem_data`, não separar perguntas. |
| Escalonamento do pai | Seleciona pai de módulo `disputa_supervisao`, em `COBRANCA`, não silenciado, com disputa aberta; prazo vencido gera push DP; níveis seguintes por idade. `app/ponto/management/commands/escalonar_chamados_supervisao.py:42-59`, `:93-104`, `:109-198`. | **Não**. Nenhuma seleção por pergunta/data/competência. |
| SLA genérico | Itera `qs_na_fila`, usa juiz de SLA, corte de passivo e teto de push DP. `app/chamados/management/commands/alertar_chamados_sla.py:24-52`. | Sem filtro próprio; herda universo da fila. |
| `perguntas_do_app` | Busca perguntas do colaborador com `q_vivas`, ordena e aplica `pergunta_viva`. `app/chamados/services/pergunta_no_app.py:39-44`. | **Não exportada**; tranca via juiz. |
| API GET perguntas | `api_disputa_perguntas` chama `perguntas_do_app`, separa respondidas e usa pai como fallback de ancoragem. `app/api/views.py:2603-2604`, `:2620-2646`. | **Não exportada**; tranca via juiz. |
| API POST resposta | Busca pergunta por dono e exige `pergunta_viva`; depois delega ao core. `app/api/views.py:2680-2698`. | **Não exportada**; tranca via juiz. |
| PWA formulário | `views.py:566-569` chama `montar_chamados_fio(so_janela=True)`; perguntas de disputas abertas são anexadas pela âncora em `app/chamados/services/fio.py:218-239`; formulário exige `p.viva` em `app/templates/chamados/partials/_card_atendimento.html:73-78`. | **Não exportada**; tranca via juiz. |
| PWA POST resposta | Mesma verificação por dono e `pergunta_viva`. `app/chamados/views.py:1238-1246`. | **Não exportada**; tranca via juiz. |
| API chamados v2 | Usa janela do colaborador; carrega **todas** as perguntas das disputas abertas, indexando destino VIVO ou pai; serializa sem filtro de vitalidade. `app/api/views.py:1015-1016`, `:1028-1049`, `:1158-1178`. | **Não**, nem C6 nessa seleção de perguntas. |

A distinção decisiva: `pergunta_viva` delega ao cálculo em `app/chamados/juizes.py:778-804`; este rejeita dia trancado e via de resolução por tranca em `:1005-1016`. A tranca é determinada pelo **`p.data_evento`**, janela da empresa e `periodo_trancado`, salvo toque humano posterior: `:875-907`. Esse caminho não consulta exportação vigente. A remoção de `dia_em_competencia_exportada` está expressamente documentada em `:1504-1528`.

A janela do colaborador não resolve a mistura de competências: seleciona `criado_em` na janela corrente e abre exceção por `q_vivas` **com destino não nulo**; não testa exportação nem `p.data_evento`. `app/chamados/services/janela_colab.py:49-74`. Pai recém-criado entra pela sua data atual, carregando perguntas antigas via `fio.py:218-239`.

Sobre push no próprio religamento: o método salva a troca de pai e registra evento, sem chamar notificações (`app/chamados/models.py:1774-1782`). O gate `abrir` cria, registra nascimento e eventualmente emite evento de célula (`:730-739`); `_trilha_do_nascimento` só grava auditoria (`:880-892`). Os receivers de chamado inspecionados tratam terminais, julgamento por dia forte e silêncio de isentos (`app/chamados/signals.py:53-82`, `:164-180`, `:227-251`), e os receivers de disputa relevantes condicionam o efeito ao fechamento (`:18-29`, `:97-107`, `:124-140`). Não há push automático nessas rotas inspecionadas.

Outros pushes de cobrança/reabertura são explícitos, sem filtro de exportação: abrir/agregar disputa usa `_enviar_push_disputa_colab`, com guarda global de ContextVar, em `app/chamados/services/disputa_emissao.py:1483-1507` (chamadores `:607`, `:623`, `:1089`, `:1170`); reabrir questionário notifica em `app/chamados/services/acoes_disputa.py:90-110`; reabrir administrativamente notifica em `:265-293`; recusar resposta pede resposta nova em `app/chamados/services/validacao.py:540-561`.

## 29/10 23:05 · sessao 9ff2e22e4a8d · - · - · origem=Ronald (tmux)

Achados comprováveis por leitura; testes não executados para preservar a árvore. [nao verificado]

1. **Recusa não impede nascimento — gate sem mordida.** Em `app/chamados/models.py:1766-1773`, `religar_pai_vivo()` recusa chamado terminal devolvido pelo ramo AGRUPADO-EM, mas `PerguntaDisputa.abrir()` ignora o resultado e cria a pergunta em `:2176-2179`. Cenário: disputa aberta sob pai terminal; existe chamado da mesma chave agrupado sob outro pai vivo. `ChamadoColaborador.abrir()` devolve esse filho terminal em `:689-696`; a disputa permanece no pai terminal e a pergunta sem destino nasce viva ali. O agrupamento aceita pai vivo do mesmo colaborador (`:575-596`). Classe: **pouso antes do pré-requisito / gate sem mordida**.

2. **Trava termina antes do nascimento — corrida com fechamento.** O atomic de religamento termina em `app/chamados/models.py:1782`; o INSERT ocorre depois, em `:2178`. Um caminho real gera perguntas após sair do atomic de `_abrir_ou_agregar_disputa` (`app/chamados/services/disputa_emissao.py:598-606,618`). Cenário: disputa aberta com perguntas antigas validadas e pai terminal; T1 religa a pai novo vivo e libera trava; T2 fecha esse pai antes do INSERT; T1 cria pergunta viva sob pai já fechado. T2 passa S127 porque perguntas antigas existem e nenhuma está viva (`app/chamados/juizes.py:1006-1007,1817-1823`; fechamento em `app/chamados/models.py:142-155,183,262-264`). O resolver não altera antigas já materializadas/validadas (`app/chamados/services/disputa_emissao.py:1634-1637`). Classe: **ordem de trava/transação / pré-requisito não preservado**. O teste de idempotência só faz duas chamadas sequenciais (`app/chamados/tests/test_o261_pai_terminal.py:116-120`).

3. **Reabertura parcialmente gravada e retry inútil.** `reabrir_sistema()` salva abertura e nota antes do atomic do religamento (`app/chamados/models.py:1811-1819`). Cenário: pai terminal legado com `urgencia_original='critica'`; a nova porta copia esse valor (`:1761`), e `ChamadoColaborador.abrir()` rejeita antes do dedup (`:659-661`; validação em `app/chamados/juizes.py:1880-1889`). A disputa fica aberta com pai terminal; retry retorna imediatamente porque já está aberta (`app/chamados/models.py:1805-1806`). O estoque histórico desses valores é documentado em `app/chamados/juizes.py:1850-1855`; sua presença atual no banco não foi verificada. Classe: **transação incompleta / idempotência de retry**.

Matriz dos testes, por inspeção: [nao verificado]

| Teste | Sem mudança correspondente | [nao verificado]
|---|---|
| Nascimento sem destino | Falha na âncora viva (`test_o261_pai_terminal.py:74-82`). |
| Pai vivo permanece | **Passa sem cura**; controle negativo (`:85-92`). |
| Destino vivo não religa | **Passa sem cura**; controle negativo (`:95-108`). |
| Idempotência | Falha pela trilha ausente; não testa concorrência (`:111-126`). |
| Reabrir sistema | Falha sem novo religamento (`:129-139`). |
| Vigia conta estado | Falha com função ausente/contador zero (`:142-153`). |
| Carimbo alterado | Falha sem chave nova, mas constante zero no comando passa: fixture não planta pai terminal (`app/chamados/tests/test_o261_vigia_fio.py:689-710`). |

A serialização de dois religamentos sobre a mesma disputa está presente: leitura sob trava da disputa e do colaborador, seguida do gate que trava colaborador (`app/chamados/models.py:1743-1749,666-670`). Isso não protege o intervalo até a criação da pergunta nem coordena o fechador (`:2177-2179,122-183`).

## 29/10 23:05 · sessao f7e20b14288d · - · - · origem=Ronald (tmux)

Fila admin — evidências consolidadas:

| Caminho | Seleção/efeito | Exportada? Trancada/fechada? |
|---|---|---|
| `qs_vivos` → `qs_na_fila` | Universo `status_local ∈ VIVOS`; fila `status_local ∈ COBRANCA` menos `silenciado_ate > now` (`app/chamados/services/painel.py:153-155`, `186-188`). | Nenhum filtro por `data_evento`, exportação ou tranca nesses métodos (`app/chamados/services/painel.py:153-155`, `186-188`). |
| Lista principal / resumo | View chama `montar_contexto_painel`; lista padrão corta por `COBRANCA`, depois exclui verbos arquivado/histórico; resumo agrupa essa mesma lista (`app/chamados/views.py:726-738`; `app/chamados/services/painel.py:314-329`, `337-338`, `462-475`). | Nenhum filtro de exportação/tranca nesses cortes (`app/chamados/services/painel.py:314-329`). |
| Totais `abertos`, `em_analise`, `ativos`, `emergencias` | Contam `qs_na_fila`; `ativos=abertos+em_analise` (`app/chamados/services/painel.py:210-223`). | Sem corte adicional por competência (`app/chamados/services/painel.py:210-223`). |
| Crédito da pergunta ao pai | Busca `Q(chamado_destino__in=vivo) | Q(disputa__chamado__in=vivo)`; sem destino, credita `disputa.chamado`; filtra `q_vivas()` e exclui materializada com `VL_SIM` (`app/chamados/services/raiox.py:54-55`, `72`, `133-149`). | Exportação: não. Tranca dinâmica: **não chama juiz exato**. `q_vivas` exclui legado carimbado com `VIA_COMPETENCIA_TRANCADA`, mas declara não expressar a tranca por leitura (`app/chamados/models.py:2325-2332`, `2342-2353`). |
| Pilhas / verbos / pilulas | `particionar_verbos` usa esses créditos; respondida → validar, não respondida → cobrar, salvo precedência auditoria/módulo; aplica arquivado/histórico conforme silêncio (`app/chamados/services/verbos.py:31-59`; `app/chamados/catalogo/natureza.py:41-63`, `72-75`). | Sem filtro adicional de exportação ou tranca; herda limitações de `creditos_perguntas` (`app/chamados/services/verbos.py:31-59`). |
| Placar JSON / navbar | JSON lê lavrado sem filtros ou recalcula contexto; cron lavra contexto; badge admin soma validar+decidir; fallback navbar recalcula mesma partição (`app/chamados/views.py:840-861`; `app/core/management/commands/lavrar_badge_navbar.py:39-42`, `68-83`; `app/core/context_processors.py:88-93`; `app/chamados/services/verbos.py:19-24`). | Sem corte adicional; herda métodos acima (`app/core/management/commands/lavrar_badge_navbar.py:39-42`, `72-83`). |
| Fila copiloto / idade / gestão | `por_verbo` usa `particionar_verbos(qs_vivos())`; estoque/gestão usam `qs_na_fila` (`app/chamados/services/fila.py:53-55`, `88-100`, `117-141`, `145-170`). | Sem corte adicional por exportação/tranca (`app/chamados/services/fila.py:53-55`, `88-100`, `145-170`). |
| Inteligência `na_fila` | Usa vivos e conta chamados cujo verbo pertence a `VERBOS` (`app/chamados/services/na_fila.py:10-26`). | Sem corte adicional; herda crédito/partição (`app/chamados/services/na_fila.py:15-24`). |
| Fila por causa / toques | Universo `qs_na_fila`; classifica cada chamado; resposta sem veredito ganha gaveta própria (`app/chamados/services/fila_por_causa.py:158-161`, `195-200`, `210-216`). | Não exclui competência exportada ou trancada no universo; `respostas_sem_veredito` é outra leitura cuja implementação cabe à investigação de julgamento (`app/chamados/services/fila_por_causa.py:158-161`, `210-216`). |
| Modal/fio admin | Admin carrega todos chamados; busca perguntas de disputas abertas e vivas de fechadas; agrupa por `chamado_que_lista` (`app/chamados/services/fio.py:173-184`, `218-240`). | Exportação: não. Em disputa **aberta**, anexa todas sem juiz; em disputa **fechada**, exige `pergunta_viva` (`app/chamados/services/fio.py:229-239`). Modal desenha cada pergunta anexada (`app/templates/chamados/partials/modal_fio.html:154-155`). |
| Destaque/ordenação do fio | `tem_pergunta_viva` usa property `.viva` + `pergunta_sem_acao_possivel`; cards com ela sobem (`app/chamados/services/fio.py:279-281`, `409-415`). | `.viva` delega ao juiz, logo tranca dinâmica conta; exportação não é condição desse trecho (`app/chamados/models.py:2355-2360`; `app/chamados/juizes.py:1005-1016`). |
| Detalhe local do pai | Busca disputa aberta por `chamado`, carrega perguntas dela e filtra `_viva(_p)` (`app/chamados/services/acoes_chamado.py:335-348`). | Exportação: não; tranca dinâmica: sim, pelo C6 (`app/chamados/juizes.py:1005-1016`). |
| Contador modal | Ativos via `qs_na_fila`, disputas via `fechada_em IS NULL` (`app/chamados/views.py:900-912`). | Não corta pela competência das perguntas (`app/chamados/views.py:900-912`). |

Distinção importante: **trancada não é equivalente a somente exportada na árvore atual**. A própria regra diz “NUNCA exportada: competência só exportada segue viva”; a antiga `dia_em_competencia_exportada` foi removida e há lápide explícita (`app/chamados/services/competencia_trancada.py:3-11`; `app/chamados/juizes.py:1504-1526`). O juiz exato mata pela tranca **do `data_evento`**, salvo toque humano posterior à tranca (`app/chamados/juizes.py:887-907`, `1005-1016`).

Mecanismos vizinhos que poderiam confundir a conclusão:

- `arquivar_competencia_encerrada` silencia **chamados**, usando dia do chamado/criação; pula chamado com disputa contendo qualquer pergunta viva. Não separa perguntas por exportação (`app/chamados/management/commands/arquivar_competencia_encerrada.py:100-128`, `148-162`).
- Serviço de tranca seleciona perguntas por `data_evento` na janela; hoje não as carimba, pois C6 lê a tranca. Preserva chamado com pergunta viva fora da competência e disputa cujo pai vivo fica fora (`app/chamados/services/competencia_trancada.py:106-149`, `171-198`). Isso atende **tranca**, não “apenas exportada” (`app/chamados/services/competencia_trancada.py:3-11`).
- `religar_pai_vivo` não lê nenhuma pergunta/competência/exportação; cria/obtém pai vivo com prazo e escreve apenas `d.chamado` (`app/chamados/models.py:1741-1782`). Wrapper exige existir **alguma** viva sem destino, sem particionar demais perguntas (`app/chamados/models.py:1788-1795`). Todas sem destino passam a usar pai novo pelo `or` da ancoragem (`app/chamados/services/fio.py:116-117`).

Não encontrei AGENTS na busca `rg --files -g AGENTS.md`; existência fora desse universo [nao verificado].

## 29/10 23:05 · sessao dfa06780a531 · - · - · origem=Ronald (tmux)

Achado principal: `religar_pai_vivo` substitui “chamado vivo do dia” por “container vivo da pessoa”, contrariando o pedido literal de `app/docs/CORTES.md:381`. A busca nova não recebe dia/célula nem consulta a cobrança existente; filtra apenas colaborador, categoria, módulo e `VIVOS`. `app/chamados/models.py:1749`

Cenário concreto: existe pergunta viva sem destino de 15/10, disputa fechada com pai resolvido e cobrança `batida_ausente` aberta daquele colaborador/dia. `desfazer_carimbo_sem_lastro` reabre essa disputa; a nova cadeia cria outro `disputa_supervisao` e mantém a pergunta sem destino, embora a cobrança correta já exista. Caminho: `app/chamados/services/relavra_via.py:195`, `app/chamados/services/relavra_via.py:222`, `app/chamados/models.py:1819`, `app/chamados/models.py:1795`, `app/chamados/models.py:1749`. Classe: derivação/chave paralela e cura fora da origem do vínculo.

A autoridade do dia seleciona cobrança por dia forte e desempate “mais nova”; a porta canônica de costura valida, aponta destino e lavra no mesmo ato. `app/chamados/juizes.py:1610`, `app/chamados/juizes.py:1621`, `app/chamados/services/fio.py:740`, `app/chamados/services/fio.py:792`

O pai novo também nasce sem célula e sem `data_turno`; portanto o juiz forte do dia não o identifica como cobrança de 15/10. `app/chamados/models.py:1762`, `app/chamados/models.py:905`, `app/chamados/juizes.py:118`

Há uma incompletude estrutural adicional: o novo `any(pergunta_viva(q) for q in qs)` copia literalmente a redução de `fio_vivo`, autoridade registrada C15. Hoje os booleanos coincidem; não há diferença comportamental demonstrada, mas resta segundo escritor da redução. `app/chamados/models.py:1793`, `app/chamados/juizes.py:1102`, `app/core/juizes.py:241`

Não identifiquei cópia nova de C7 nem registro novo que dispare JUIZ-NOVO; `pai_terminal` consulta o `VIVOS` oficial. `app/chamados/models.py:1434`, `app/chamados/models.py:1715`, `app/core/juizes.py:233`, `CLAUDE.md:86`

Não elevaria o `_tz.now()` do SLA a achado: encontrei a classe proibida citada, mas nenhuma regra concreta de borda aplicável que sustentasse a reprovação. `app/chamados/models.py:1763`, `CLAUDE.md:592`

## 29/10 23:04 · sessao 842bc2ead565 · - · - · origem=Ronald (tmux)

Censo de caminhos, por leitura:

- Nascimento: seis chamadas a `PerguntaDisputa.abrir`: `app/chamados/services/disputa_emissao.py:389`, `:770`, `:1160`; `app/chamados/services/acoes_disputa.py:265`; `app/chamados/services/declaracao_texto.py:322`, `:611`. A criação efetiva fica em `app/chamados/models.py:2178`.
- Nascimento da disputa: núcleo comum em `app/chamados/services/disputa_emissao.py:523`, agrupador manual em `:927` e âncora manual em `:1147`; usam `ChamadoColaborador.abrir` antes de criar, respectivamente em `:504`, `:908`, `:1128`.
- Reabertura da disputa: duas portas escrevem `fechada_em=None`, `reabrir_admin` em `app/chamados/models.py:1701` e `reabrir_sistema` em `:1811`. A ação administrativa cria pergunta depois, alcançando o gate novo (`app/chamados/services/acoes_disputa.py:256`, `:265`).
- Chamadores de `reabrir_sistema`: ressuscitação em `app/chamados/models.py:1189`; desfazer carimbo em `app/chamados/services/relavra_via.py:222`; devolver validação em `app/chamados/services/validacao.py:200`; rejeição de ausência em `app/chamados/services/materializacao.py:631`; comando de órfãs em `app/chamados/management/commands/reabrir_orfas_enterradas.py:154`.
- Reabertura de pergunta também ocorre em `app/chamados/models.py:2269`, `:2284`, `:2303`, e na porta `app/chamados/services/disputa_emissao.py:1327`; esta é chamada pelo colaborador em `app/chamados/models.py:1871`, pela recusa administrativa em `app/chamados/services/validacao.py:540`, por ausência rejeitada em `app/chamados/services/materializacao.py:628` e pelo comando de limbo em `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`.

Achados para consolidar:

1. **Gate novo não cobre a pergunta existente que ganha vida ao ser reapontada.** `PerguntaDisputa.abrir` troca `celula`, relavra, salva e retorna a existente antes da chamada nova a `religar_pai_vivo` (`app/chamados/models.py:2154`, `:2168`, `:2171`, `:2177`). Cenário: fato existente sem destino, disputa aberta e pai resolvido; sua célula histórica absolve, mas a célula correta encontrada para o colaborador/dia acusa. O reapontamento torna a pergunta viva e a devolve ainda sob pai terminal. Classe: **cura incompleta na origem / caminho que escapa do gate**. A vida é decidida com os insumos da pergunta, inclusive célula, conforme a documentação do prefiltro em `app/chamados/models.py:2317`.

2. **Reabrir pergunta em disputa já aberta continua sem garantir pai vivo.** A rejeição da ausência limpa os carimbos das perguntas e chama `reabrir_sistema` (`app/chamados/services/materializacao.py:628`, `:631`). Porém essa porta retorna imediatamente quando a disputa já está aberta, antes do religamento recém-adicionado (`app/chamados/models.py:1805`, `:1819`). Cenário: disputa aberta sob pai resolvido, pergunta sem destino absorvida por ausência; ausência rejeitada e nenhuma outra ausência cobrindo o dia → pergunta volta a pendente, pai permanece resolvido. Classe: **cura incompleta / pré-requisito da reabertura não assegurado**. A condição de rejeição e ausência sem cobertura está em `app/chamados/services/materializacao.py:608`, `:615`.

Observações de autoridade:

- O diff não acrescenta outra implementação de `pode_fechar`; a autoridade registrada continua `DisputaSupervisao.pode_fechar` em `app/core/juizes.py:233`.
- O novo `any(pergunta_viva(q)...)` em `app/chamados/models.py:1793` repete a redução já existente em `fio_vivo`, `app/chamados/juizes.py:1102`–`:1111`. Há duplicação estrutural da redução C15, mas divergência funcional atual entre as duas expressões **[não verificado]**.
- A nova escrita do vínculo fica em `app/chamados/models.py:1774`; os outros três locais identificados são os nascimentos citados acima. Segundo escritor posterior ao nascimento **[não verificado]**.

Não executei testes ou operações de escrita. **[não verificado]** [nao verificado]

## 29/10 23:03 · sessao 96578cc4f702 · o261c2 · sonda · origem=Code

- MISTAS 18 disputas | perguntas EXPORTADAS nelas 201 | ABERTAS nelas 212 | colabs 18. [nao verificado]
- EXPORTADAS nas mistas por competencia: `{'2:09/2026': 192, '3:09/2026': 9}`. [nao verificado]
- SO-EXPORTADA 22 disputas | perguntas 269. [nao verificado]
- SO-ABERTA 33 disputas | perguntas 125. [nao verificado]
- universo repartido 807 (tem de ser == vigia 807) | disputas 73 | colabs 66. Universo == VIGIA: 807 == 807. [nao verificado]
- ERROS=0. [nao verificado]

As duas somas fecham: ABERTA + EXPORTADA + sem data = 337 + 470 + 0 = 807; mistas + so-exportada + so-aberta = 18 + 22 + 33 = 73. [nao verificado]

## 29/10 22:09 · sessao 61d5e68cd5c8 · - · - · origem=Ronald (tmux)

Achado confirmado: novo `any(pergunta_viva(q) for q in qs)` duplica C15, registrado como `fio_vivo` em `app/core/juizes.py:123` e `:241`; autoridade em `app/chamados/juizes.py:1102`. O contrato existente detecta exatamente essa forma: `app/chamados/tests/test_o261_fio_tem_uma_porta.py:99`, `:127`, `:142`. Executei o detector AST real contra o trecho novo em memória: acusou `religar_se_fio_sob_pai_morto`.

Outros pontos encaminhados:

- Admin conserva `ch` antigo: nota e push usam esse objeto mesmo se `PerguntaDisputa.abrir` mudar o pai; base `app/chamados/services/acoes_disputa.py:270`, `:275`.
- Novo pai manual perde `contexto_json.chamados_orfaos_ids`, necessário para dissolução imediata por entrada sincronizada: nascimento em `app/chamados/services/disputa_emissao.py:920`; leitor em `app/ponto/services/entrada_sincronizada.py:47`.
- Reparentar muda a âncora de todas as perguntas sem destino, inclusive históricas, enquanto followups ficam no pai anterior: `app/chamados/services/fio.py:117`; novas respostas escrevem no pai atual em `app/chamados/services/disputa_emissao.py:1309`.
- Não encontrei segundo escritor de `DisputaSupervisao.chamado` pós-nascimento na base; escritores de nascimento em `app/chamados/services/disputa_emissao.py:527`, `:931`, `:1151`.
- Não classificaria a suíte inteira como sem mordida: os testes positivos de pai terminal verificam mudança ausente na base; os controles de pai vivo/destino vivo passam também sem cura. O teste de idempotência usa motivos diferentes e não demonstra duas entradas iguais, conforme anexo `app/chamados/tests/test_o261_pai_terminal.py:111`.

Censo ampliado de consumidores foi enviado nas mensagens anteriores, com linhas.

## 29/10 22:09 · sessao 6d3f808735cb · - · - · origem=Ronald (tmux)

Achados de escopo e selo:

- **P2 — Escopo de testes diverge do corte.** O corte permite “testes de tela”; o wrapper exige testes **novos**, por nome/caminho. Assim, alteração legítima de teste existente é contada fora, enquanto teste novo sem relação com tela passa. O selo exercita somente teste novo. Referências: `app/docs/CORTES.md:394`; `bin/codex_pergunta.sh:116`; `bin/codex_pergunta.sh:117`; `bin/tests/test_codex_pergunta.sh:100`; `bin/tests/test_codex_pergunta.sh:164`.

- **P2 — Selo não garante o destino efetivo da chamada.** `_estrutura` aceita presença de `-C "$WS"` e proíbe somente algumas grafias de destino vivo. Mantendo esse argumento e acrescentando `-C "$VIVO/app"`, sua verificação continua sem diagnósticos; o teste comportamental também verifica somente presença de argumentos isolados. Confirmei a lacuna estrutural executando exclusivamente a função extraída em memória com essa mutação, sem arquivos. Referências: `bin/tests/test_codex_pergunta.sh:35`; `bin/tests/test_codex_pergunta.sh:39`; `bin/tests/test_codex_pergunta.sh:40`; `bin/tests/test_codex_pergunta.sh:133`; `bin/codex_pergunta.sh:251`. Execução comportamental do mutante: **[nao verificado]**.

- **P2 — Ressalva da 7b ausente.** As proibições gerais de escrita continuam sem exceção para o piloto, embora o wrapper já habilite `workspace-write`; falta a ressalva expressamente exigida pelo contrato v2(a) fornecido. Referências: `CLAUDE.md:578`; `CLAUDE.md:588`; `CLAUDE.md:599`; `bin/codex_pergunta.sh:246`.

- **P2 — Procedimento publicado ainda contradiz v2.** O RELATO continua mandando remover o papel “no mesmo marco”, enquanto v2(j) exige pouso próprio posterior; o descarte publicado usa `checkout .`, sem limpar novos arquivos, divergindo do reset à base + `clean -fdx` do v2(h). Referências: `app/docs/RELATO.md:10`; `app/docs/RELATO.md:12`; `bin/codex_pergunta.sh:39`.

Não trataria `rc=0` com `fora_do_escopo>0` como falha isolada do wrapper: descarte e selos semânticos são responsabilidade expressa do Code. Referências: `app/docs/RELATO.md:10`; `app/docs/RELATO.md:11`; `app/docs/CORTES.md:394`.

## 29/10 22:08 · sessao 06d376475023 · - · - · origem=Ronald (tmux)

Achados e lacunas dos consumidores/selos:

- **Alta — orientação de LIMITE incorreta para construtor.** `bin/codex_pergunta.sh:263` manda seguir por sonnet sem esperar, inclusive no novo papel. `bin/codex_limite_estado.py:157` repete essa instrução global. Contraria a espera do piloto e a proibição de reconstrução pelo Code, também registradas em `app/docs/RELATO.md:10`. O selo só simula LIMITE para leitura (`bin/tests/test_codex_limite.sh:24`); o falso construtor só tem modos ok/escreve/dorme (`bin/tests/test_codex_pergunta.sh:96`).

- **Média — classificação de construtor sem mordida no selo de estado.** As fixtures só incluem papéis antigos e inventado (`bin/tests/test_codex_papel_estado.sh:14`); a asserção aceita o prefixo terminando em sonda (`bin/tests/test_codex_papel_estado.sh:31`). Remover construtor de `PAPEIS` faria o papel cair em outro (`bin/codex_limite_estado.py:78`) sem contrariar essas asserções.

- **Média — ESPERANDO não tem integração testada.** O selo aceita a linha em qualquer posição e qualquer PID numérico (`bin/tests/test_codex_pergunta.sh:160`), sem exigir primeira linha útil, PID do wrapper ou aceitação pelo hook. O consumidor real exige identidade do processo/script (`bin/hook_stop_fila1.py:474`, `bin/hook_stop_fila1.py:521`). Não identifiquei incompatibilidade nominal entre a emissão atual (`bin/codex_pergunta.sh:90`) e esse consumidor; execução integrada [nao verificado].

- **Média — cobertura parcial da preservação e do timeout.** O teste comportamental antigo exercita leitura (`bin/tests/test_codex_pergunta.sh:129`), sem censo/verificador. O teste sonda continua separado (`bin/tests/test_codex_sonda.sh:58`). Timeout cobre gate e exec nos cenários (`bin/tests/test_codex_pergunta.sh:179`, `bin/tests/test_codex_pergunta.sh:187`), sem cenário de todos os slots ocupados ou filho resistente a TERM.

- **Média — ressalva de 7b ainda pendente para o pouso.** Permanecem proibições gerais de escrita e sandbox diferente de read-only em `CLAUDE.md:574`, `CLAUDE.md:578`, `CLAUDE.md:588` e `CLAUDE.md:599`. O contrato v2(a) anexado exige ressalva no pouso; ocorrência desse pouso [nao verificado]. [nao verificado]

Compatibilidade confirmada por leitura: os 12 campos não quebram o parser, que aceita pelo menos nove e conserva os índices antigos (`bin/codex_limite_estado.py:66`). Construtor está no vocabulário (`bin/codex_limite_estado.py:23`); o gerador mantém o leitor único e propaga papel/origem sem enumeração paralela (`bin/gerar_codex_estado.py:41`, `bin/gerar_codex_estado.py:155`, `bin/gerar_codex_estado.py:176`).

Nenhum teste executado, pois criam arquivos temporários (`bin/tests/test_codex_pergunta.sh:68`, `bin/tests/test_codex_papel_estado.sh:7`); resultados de execução [nao verificado].

## 29/10 22:08 · sessao 61d74141d87b · - · - · origem=Ronald (tmux)

Achados sustentados pela base e pelo diff:

1. **Chamado extra na fila / pouso antes do pré-requisito.** O novo trecho de `PerguntaDisputa.abrir` religa qualquer nascimento sem destino antes de saber se a pergunta vive. Cenário: célula com veredito não acusatório, sem marcos pendentes; disputa aberta sob pai resolvido; nenhuma pergunta existente. O helper cria pai aberto, mas C6 declara a pergunta morta. A guarda do chamado só examina módulos de turno, e a fila inclui o novo pai aberto. Base: `app/chamados/juizes.py:1053`, `app/chamados/models.py:848`, `app/chamados/services/painel.py:186`; trecho novo imediatamente antes do `create` hoje em `app/chamados/models.py:2085`.

2. **Segundo juiz registrado.** `religar_se_fio_sob_pai_morto` introduz `any(pergunta_viva(q) for q in qs)`, duplicando a redução C15, cuja autoridade é `fio_vivo`. Registro: `app/core/juizes.py:123`; autoridade: `app/chamados/juizes.py:1102`; consumidor já convertido: `app/chamados/services/disputa_emissao.py:1188`. Ler `VIVOS` em `pai_terminal`, isoladamente, respeita C1: `app/core/juizes.py:100`.

3. **Derivação paralela da chave do pai; pode haver dois pais vivos.** Nascimento canônico fixa módulo `disputa_supervisao`, enquanto os manuais usam `disputa_supervisao_manual` e incluem subcategoria no lookup. O novo helper herda módulo do pai velho e tira subcategoria do lookup. Cenário: disputa aberta com pai manual terminal e outro pai canônico vivo do mesmo colaborador; o helper não encontra o canônico e cria outro manual vivo. Também pode reutilizar pai manual de outro subtipo. Fontes: `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:908`, `app/chamados/services/disputa_emissao.py:1128`; ausência de unicidade: `app/chamados/models.py:639`.

4. **Ordem de trava: caminho inverso alcança o helper.** `ChamadoColaborador.abrir` segura Colaborador, renasce chamado existente, ressuscita perguntas e chama `reabrir_sistema`, que passa a travar Disputa. Caminho: `app/chamados/models.py:669` → `:685` → `:770` → `:1098` → `:1188`. O concorrente novo segura Disputa e solicita Colaborador no mesmo gate. Cenário possível com disputa fechada contendo pergunta com destino ressuscitável e outra viva sem destino; um emissor conserva instância aberta obtida antes do fechamento e entra no novo nascimento. Além disso, `select_for_update().select_related('colaborador')` trava ambas as tabelas sem `of=('self',)`, portanto a consulta não expressa a sequência individual prometida pela docstring. Lei: `app/chamados/models.py:646`.

5. **Consumidor do ponteiro não atualizado no admin.** Se `ch.reabrir()` recusa, o gate pode mover `d_fec.chamado` para novo pai. O diff refresca `d_fec`, mas mantém `ch` antigo: justificativa administrativa continua anexada ao pai terminal e push anuncia seu número. Recusas: `app/chamados/models.py:1051`; nota: `app/chamados/services/acoes_disputa.py:269`; push: `:275`. A pergunta administrativa é conversa e nasce sempre: `app/chamados/models.py:2033`.

6. **Idempotência da recusa.** Duas chamadas diretas iguais ao novo `religar_pai_vivo`, com gate devolvendo filho terminal AGRUPADO-EM, produzem duas trilhas `disputa_religar_recusado`; o estado não é curado e o nascimento continua. O comportamento do gate que devolve esse terminal está em `app/chamados/models.py:689`. A duplicação das trilhas decorre do trecho novo do diff; frequência em produção: [nao verificado].

O teste anexado usa sempre célula acusatória e não cobre o achado 1, nem módulos manuais, concorrência ou recusa AGRUPADO. A efetiva execução dos testes/RED: [nao verificado]. [nao verificado]

_279 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
