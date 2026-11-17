# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 22:23** · publicado em **16/11/2026 22:23** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 22:23 · sessao 833c837a69a0 · - · - · origem=Ronald (tmux)

Achados:

- **Médio — relógio fora da borda por novo caminho.** `impedimento_estoque_l130` não recebe `agora` e chama `pergunta_viva(perg)` sem relógio (`app/chamados/services/validacao.py:381`, `:398`). O juiz pode cair em `date.today()` (`app/chamados/juizes.py:1471`, `:596`) e passar `agora=None` a `classificar_falta` (`:1475`), que lê `_tz.now()` (`app/escala/servico_jornada.py:387`). O comando lê `timezone.localtime()` somente depois da seleção, para o arquivo de reversão, e não o transmite (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:61`). A consequência documentada exige um relógio na borda até o julgamento (`app/docs/PLANO-PADROES.md:99`–`:103`).

- **Médio — lista fechada não pertence à porta.** A porta aceita qualquer pk que satisfaça o predicado; nem o predicado nem a porta consultam `ESTOQUE_L130_ALVOS` (`app/chamados/services/validacao.py:378`, `:381`, `:408`, `:432`). O comando compara a lista antes das travas (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:53`), depois chama a porta separadamente por pergunta (`:70`–`:76`). Assim, garante igualdade na fotografia inicial, não igualdade do conjunto durante as escritas; também permite aplicação parcial se outra pergunta perder elegibilidade ou falhar. A porta individual continua segura contra perda de elegibilidade daquela pergunta porque relê o predicado travado (`app/chamados/services/validacao.py:431`–`:433`).

- **Médio — teste não prova delegação da property.** O teste compara resultados nos casos listados; uma property que copie integralmente a expressão do juiz passa por ele (`app/chamados/tests/test_f1nova_b2_leitores.py:75`–`:85`). Há casos que, por inspeção, distinguem os mutantes antigos de B, C e D (`:61`–`:69`, `:75`–`:91`, `:142`–`:147`), porém o arquivo não contém injeção de mutantes, patch do juiz ou teste que force a delegação. Mordida executada: [não verificado].

Conforme:

- Ordem C → Disputa → Chamado → Pergunta pela porta canônica: chamada em `app/chamados/services/validacao.py:429`; aquisições em `app/chamados/models.py:2771`, `:2780`, `:2793`, `:2797`.
- Releitura sob trava e uma escrita pelo descarimbo canônico: `app/chamados/services/validacao.py:431`–`:440`; único `update` do descarimbo em `app/chamados/models.py:3392`–`:3394`. Não nasce segundo escritor de descarimbo nesse trecho.
- Linha RESPOSTA registrada antes do `update`, no mesmo `atomic` (`app/chamados/services/validacao.py:428`, `:439`, `:440`). O evento vem depois, mas `None` levanta exceção dentro do mesmo `atomic`, revertendo pergunta e sequência (`:449`–`:457`). `evento` retorna registro ou `None` (`app/core/observ.py:133`–`:139`); `DecisaoPergunta.registrar` não engole falhas (`app/chamados/models.py:4123`–`:4127`).
- Idempotência individual decorre de retirar `respondida_em`: segunda chamada encontra `nao_e_instante_sem_texto` (`app/chamados/services/validacao.py:390`, `:440`). A projeção evita duplicar RESPOSTA pelo mesmo instante (`app/chamados/models.py:4098`–`:4105`). O teste verifica segunda execução do comando, não duas chamadas diretas nem rollback por falha do evento (`app/chamados/tests/test_f1nova_b2_leitores.py:238`–`:244`).
- Complemento SQL correto por inspeção para NULL, vazio, espaços, NBSP, tab e LF: Python usa `(t or '').strip()` (`app/chamados/juizes.py:1526`–`:1528`), regex deriva os caracteres de `isspace()` (`:1560`–`:1562`), vazio SQL inclui NULL (`:1572`), positivo exige I e algum não branco (`:1580`), negativo é ausência de I ou I com vazio (`:1588`). Execução no PostgreSQL desta cópia: [não verificado].
- O teste B2 da face negada tem seis pares, sem NULL/tab/LF (`app/chamados/tests/test_f1nova_b2_leitores.py:127`–`:129`). A bateria anterior cobre tab/LF, NBSP e outros brancos (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:29`–`:38`, `:89`–`:120`). NULL SQL não é exercitado porque o campo é NOT NULL; há teste de `None` somente em Python (`:11`–`:15`, `:118`–`:120`).
- `test_o168` preserva o sujeito e adiciona controle positivo legal antes das duas metades ilegais; a inversão corresponde à L-130 (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:287`–`:297`). Restou docstring histórica que afirma “nunca solta”, agora contrariada pelo próprio teste (`:276`–`:280`): **menor**, documentação.
- `test_e3_10` inverte exatamente a retenção de instante sem texto, conserva controle positivo e mantém `pode_fechar=False`; não enfraquece essa proteção (`app/chamados/tests/test_e3_10_cura_no_escritor.py:127`–`:148`).
- `test_estado_aplicacao` completa o objeto sintético com instante fixo, adequando a fixture respondida ao novo contrato; as asserções permanecem específicas (`app/chamados/tests/test_estado_aplicacao.py:37`–`:44`, `:49`–`:77`).
- K6 usa dia do fato, sem relógio implícito: `app/chamados/services/materializacao.py:784`–`:789` → `app/chamados/services/passivo_validado.py:86`–`:95`.

Nenhum teste foi executado, nenhum banco acessado e nenhum arquivo escrito. Execução e resultados de produção: [não verificado]. [nao verificado]

## 16/11 22:23 · sessao ba4d04fd8d7d · - · - · origem=Ronald (tmux)

Q2 — resíduos que deveriam ler o juiz:

- `espera_o_texto_do_colaborador` ainda recompõe `not J`: `I ausente OU respondida_sem_texto`. Hoje é equivalente, mas é derivação paralela; alimenta cobrança e cartão. `app/chamados/services/fio.py:177`, `app/chamados/services/fio.py:187`, `app/chamados/services/cobranca.py:227`, `app/chamados/services/fio.py:359`.
- Canal partido chama seu universo de “Perguntas sem resposta” e exige `resposta_colab=''`: perde espaços, NBSP e texto sem instante. O cobrar consome esse detector. `app/chamados/canal.py:53`, `app/chamados/canal.py:65`, `app/chamados/views_cobrar.py:181`.
- Placar de aceite conta “respondidos” por `respondida_em__gte`, sem texto. É consumido pelo painel e pela Folha. `app/chamados/services/verbos.py:277`, `app/chamados/services/verbos.py:325`, `app/chamados/services/verbos.py:335`, `app/chamados/views.py:1489`, `app/chamados/services/painel.py:298`, `app/folha/views_retrato.py:515`.
- Ressuscitação seleciona materializadas com texto NULL/`''`, embora descreva seu universo como perguntas “SEM resposta”. Espaços/NBSP ficam fora; texto sem instante recebe a proteção reservada à resposta. É decisão operacional, não exibição. `app/chamados/models.py:1494`, `app/chamados/models.py:1530`, `app/chamados/models.py:1533`. Sua classificação formal E/F no censo é [não verificado].

E/F legítimos por sua função:

- Trilha/projeção: `app/chamados/models.py:4095`, `app/chamados/models.py:4102`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`, `app/chamados/services/historico.py:98`.
- Marcos temporais/cache: `app/chamados/veredito_lavrado.py:202`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/services/painel.py:64`.
- Integridade/estoque específico: `app/chamados/services/vigia_fio.py:737`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1104`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/services/disputa_emissao.py:1692`, `app/chamados/services/disputa_emissao.py:1861`. Auditor I20 e curar limbo usam `!= ''`, portanto não espelham trim, mas julgam integridade, não estado operacional. `app/chamados/management/commands/auditar_invariantes_chamados.py:1105`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`.
- Preservar texto bruto/escrever metades: `app/chamados/services/disputa_emissao.py:1663`, `app/chamados/models.py:3241`.
- Parse/classificação de conteúdo: `app/chamados/services/validacao.py:467`, `app/chamados/services/validacao.py:1181`, `app/chamados/services/materializacao.py:546`, `app/chamados/services/materializacao.py:625`, `app/chamados/services/materializacao.py:718`, `app/chamados/services/materializacao.py:770`, `app/chamados/services/materializacao.py:953`, `app/chamados/services/materializacao.py:1562`.
- Fallback da testemunha por conteúdo bruto: `app/chamados/templatetags/chamados_tags.py:139`, `app/chamados/templatetags/chamados_tags.py:166`, `app/chamados/services/fio.py:377`.
- Exibição API/ponto: `app/api/views.py:1163`, `app/api/views.py:2561`, `app/api/views.py:2637`, `app/api/views_mensageria.py:567`, `app/chamados/views_cobrar.py:292`, `app/ponto/services/dia_do_colab.py:70`, `app/ponto/services/veredito_celula.py:197`. A API de validação recebe seu universo do juiz. `app/api/views_mensageria.py:539`.
- Templates: seleção/exibição do cru em `_card_atendimento.html:114`, `:141`, `:148`, `:154`, `:182`; datas sob `resposta_enviada` em `:106` e `:391`; preview cru em `_lote_preview.html:24`; os dois templates migrados guardam a resposta com `.respondida`. `app/templates/chamados/partials/_pergunta_card.html:29`, `app/templates/chamados/partials/modal_fio.html:217`.

Q4 — pergunta viva I sem T:

| Leitor | Resultado |
|---|---|
| App | Pendente se disputa aberta. `app/chamados/services/pergunta_no_app.py:81` |
| Fila validar | Não entra. `app/chamados/juizes.py:1760` |
| Estado | Pendente. `app/chamados/models.py:3156` |
| Retenção | Não integra respostas sem veredito. `app/chamados/models.py:1923` |
| Pode fechar | Não fecha enquanto fio vivo. `app/chamados/models.py:1934` |
| CSF | Essa pergunta não segura pelo predicado de conversa. `app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:588` |
| Órfã | Ausência de resposta permite continuar; ainda exige fato=True, C6 e teto. `app/chamados/reconciliador.py:1085`, `app/chamados/reconciliador.py:1087`, `app/chamados/reconciliador.py:1091` |
| Pauta DP | Não entra por resposta sem veredito; só mede trancada, não mera exportada. `app/chamados/services/competencia_trancada.py:95`, `app/chamados/services/competencia_trancada.py:126`, `app/chamados/services/competencia_trancada.py:137` |
| Cobrar | Conta como esperando colaborador, sujeito ao C6/canal/prazo. `app/chamados/services/cobranca.py:241`, `app/chamados/services/cobranca.py:252`, `app/chamados/services/cobranca.py:255` |
| Vigia | Pode classificar causa b, depois de precedências anteriores. `app/chamados/services/vigia_fio.py:628`, `app/chamados/services/vigia_fio.py:634` |

Tarefa/dono não decorre apenas de I/T: última linha RESPOSTA vazia exige responder/colaborador; projeção sem linha gera desconhecimento; linhas posteriores podem mudar o próximo ato. `app/chamados/services/tarefa_do_dia.py:154`, `app/chamados/services/tarefa_do_dia.py:159`, `app/chamados/services/tarefa_do_dia.py:163`. O dono colab exige também presença entre pendentes do app e ação responder. `app/chamados/services/tarefa_do_dia.py:651`.

Existe oposição condicional: placar diz “respondido” e app diz “pendente” para I sem T cujo instante esteja na janela pós-cobrança. `app/chamados/services/verbos.py:335`, `app/chamados/services/verbos.py:342`, `app/chamados/services/pergunta_no_app.py:81`. A ocorrência dessa janela nas pk reais é [não verificado].

## 16/11 22:23 · sessao 00869dd13935 · - · - · origem=Ronald (tmux)

Q6 — há caminhos de escrita que alcançam competência entregue; o comando novo não protege os demais consumidores. A ocorrência nas 141 perguntas do dado não pode ser afirmada sem conhecer suas datas, âncoras e lâmpadas [não verificado].

| Consumidor | Mudança e escrita possível | Entregue |
|---|---|---|
| **CSF / supra_juiz** | `sem_veredito_entre` deixa de reter I sem T/T sem I (`app/chamados/models.py:1923`). A lista compartilhada libera CSF (`app/ponto/supra_juiz.py:573`, `:588`); o executor chama `retratar` (`app/ponto/management/commands/supra_juiz.py:261`, `:286`). `_retratar` fecha chamado, pode rejeitar justificativa e chama resolver (`app/chamados/reconciliador.py:520`, `:525`, `:531`, `:538`). | **Sim, potencial.** O gate de pergunta viva exclui entregue explicitamente (`app/chamados/models.py:364`); portanto não substitui a retenção removida. O cron `--executar` está configurado (`app/config/crons.py:599`), mas sua janela padrão é atual (`app/ponto/management/commands/supra_juiz.py:90`). Alcance nas 141 [não verificado]. |
| **Pauta após fechamento** | S143 passa I sem T para `reabrir_credor` (`app/chamados/services/disputa_emissao.py:1843`, `:1969`), que recusa entregue por K6 (`:1857`, `:1867`) e escreve nota de recusa (`:1879`). O resolver coleta (`:1973`); `transicionar_com_pauta` escreve Pauta no mesmo ato (`app/chamados/services/fecho_runtime.py:161`, `:168`, `:139`). | **Sim, potencial**, após fechamento agora liberado pela retenção B; nenhum descarimbo é necessário para nascer essa Pauta. |
| **Cartório** | A isenção D passa de T para J (`app/ponto/services/cartorio.py:1641`); T sem I deixa de isentar o chamado e pode liberar dedup/fecho (`:1652`, `:1667`) ou tentativa de ressuscitação (`:1674`). | **Não há veto K6 nesse bloco.** A ressuscitação continua selecionando somente texto NULL/`''` (`app/chamados/models.py:1530`), então T sem I isolado não é descarimbado; irmãs vazias no mesmo chamado podem ser atingidas (`:1600`, `:1629`). O comentário reconhece alcance exportado histórico (`app/ponto/services/cartorio.py:1626`). |
| **Reconciliador — lavrar_sem_turno_aberto** | D passa a J (`app/chamados/reconciliador.py:365`); T sem I pode chegar ao carimbo `sem_turno_aberto` e ao fechamento da disputa (`:407`, `:413`). | **Não existe K6 neste caminho**; depende de ausência de turno e C6 morto (`:373`, `:399`). O carimbo escreve materialização/via (`app/chamados/models.py:3239`, `:3263`), sem tocar o par quando `resposta=None` (`:3241`). |
| **Reconciliador — veredito_orfa** | D passa a J (`app/chamados/reconciliador.py:1084`); T sem I pode produzir `ENCERRA` após lâmpada, C6 e teto (`:1087`, `:1093`). O consumidor chama `encerrar_fantasma` e evento (`:1138`, `:1150`, `:1152`), podendo fechar disputa (`:1173`). | **Sem veto K6 neste caminho**. `encerrar_fantasma` grava pela porta de carimbo (`app/chamados/models.py:3415`). Para I sem T a condição D já era falsa antes: a troca D não cria delta próprio nesse caso (`app/chamados/reconciliador.py:1085`). |
| **Becos** | O leitor C usa property (`app/chamados/services/becos.py:29`); I sem T torna-se visível quando destino não terminal, reduzindo candidatos. Havendo destino terminal, `pode_fechar` ainda decide (`:33`). O ato resolve perguntas e fecha disputa (`:54`, `:56`). | A retirada da retenção B pode liberar disputa cuja pergunta esteja morta pelo C6, inclusive trancada (`app/chamados/models.py:1934`). **O cron instalado usa `--alarme`, sem escrita** (`app/config/crons.py:663`); a porta por evento continua escrevendo (`app/chamados/services/becos.py:74`, `:78`). |
| **Materialização** | A seleção C agora exclui I sem T (`app/chamados/services/materializacao.py:2067`); reduz plantios. D `_materializar_resposta_hora` agora recusa T sem I com escrita de recusa (`:1314`, `:1322`). | A guarda K6 é específica para metades de intervalo fora163 (`:792`, `:805`), não geral. A fila pública não traz mais esses pares (`:2069`), mas a unidade `_materializar_uma` também tem consumidor no passivo (`app/chamados/services/passivo_validado.py:39`). Não afirmar novo plantio: a migração endurece anuência. |
| **Vigia fio** | Usa J no subtipo c, mantendo antes o subtipo b para I sem T (`app/chamados/services/vigia_fio.py:634`, `:636`). | Esse trecho somente conta/lista e retorna (`:656`, `:660`). Não é escritor. |
| **views_cobrar** | I sem T sai do lote validar e entra em aguardando colaborador (`app/chamados/views_cobrar.py:169`, `:175`, `:197`). | **Sem nova escrita para o par ilegal**; validação só é chamada para `pendentes` J (`:202`, `:209`), em POST humano (`:151`). |
| **Dedup / backfill decisões** | A escolha canônica e mérito mudam T para J (`app/chamados/services/fio.py:1281`, `:1294`). O apply do dedup delega à porta do backfill (`app/chamados/management/commands/dedup_perguntas_fato.py:105`). | **O apply preserva grupos exportados**, com lista e saída PAREI (`:112`, `:132`); o contrato do veto aparece no próprio chamador (`:102`). |

Pauta da tranca: `medir` só trabalha se `periodo_trancado` existir (`app/chamados/services/competencia_trancada.py:95`); pede `respostas_sem_veredito(ignorar_tranca=True)` (`:126`). I sem T sai de `decidir_pelo_dp` (`:137`), podendo liberar `pode_fechar` se C6 também disser morto (`:178`); `encerrar` escreve chamado/disputa (`:209`, `:229`) e Pauta somente se sobrar lista (`:232`). **Exportada e trancada são estados distintos no próprio contrato** (`:5`); o dado não prova quantas das 141 são trancadas [não verificado].

Push: a migração de `recobrar_questionario` altera a contagem de pendentes (`app/chamados/services/disputa_emissao.py:1735`); o mesmo serviço envia push e grava nota (`:1748`, `:1758`), sem veto K6 nesse caminho. É ação de recobrança, não consequência demonstrada de cron [não verificado].

## 16/11 21:54 · sessao ff8736dc078a · E4C-F1-B2 · censo · origem=Code

A cópia contém o juiz da B1, mas o corpo de retenção ainda usa **OU**, a property `respondida` ainda usa **só instante**, e as entradas de validar/recusar também continuam por instante. A B2 permanece registrada como construção dos leitores B/C/D. (`app/chamados/juizes.py:1551`; `app/chamados/models.py:1924`; `app/chamados/models.py:3665`; `app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:813`; `app/docs/BACKLOG.md:821`.)

O pouso c4+c4c, merge `41f5648af`, está registrado na documentação da cópia. Correspondência independente da árvore com `520d7b557`, diff Git contra `71e9576bf`, execução dos testes e confirmação de produção: **[nao verificado]**. (`app/docs/BACKLOG.md:2081`.)

**1. CENSO**

`I` = instante presente; `T` = texto não vazio após trim; `J` = `I E T`, pelo juiz `pergunta_respondida`, com face SQL `q_respondida_com_texto`. (`app/chamados/juizes.py:1552`; `app/chamados/juizes.py:1580`.)

O fechamento abaixo cobre os **66 usos B/C/D identificados pelos R do anexo**, mais **14 delegações complementares** explicitadas depois das tabelas. São ocorrências nomeadas, não 80 predicados independentes. Cobertura de despacho dinâmico arbitrário: **[nao verificado]**.

| R | B — leitura atual | Evidência no HEAD fornecido |
|---|---|---|
| 24 | Irmãs delegam ao corpo sem veredito | `app/chamados/juizes.py:2105` |
| 27 | Auditor I12 delega às irmãs | `app/chamados/management/commands/auditar_invariantes_chamados.py:984` |
| 38 | Comando consulta respostas sem veredito | `app/chamados/management/commands/validar_classe_a.py:54` |
| 42 | **Corpo:** sem validação/materialização, `I OU T`, com tranca opcional | `app/chamados/models.py:1922`; `app/chamados/models.py:1924`; `app/chamados/models.py:1926` |
| 43 | Lista da disputa delega ao corpo | `app/chamados/models.py:1906` |
| 44 | `pode_fechar`: lista vazia e ausência de fio vivo | `app/chamados/models.py:1934` |
| 45 | `fechar` consulta a lista | `app/chamados/models.py:1967` |
| 60 | Retenção da retratação consulta irmãs | `app/chamados/reconciliador.py:448` |
| 62 | Encerramento de órfãs consulta a lista | `app/chamados/reconciliador.py:1164` |
| 69 | Medição da tranca consulta lista ignorando tranca | `app/chamados/services/competencia_trancada.py:126` |
| 80 | Resolvedor consulta irmãs antes de reabrir pai | `app/chamados/services/disputa_emissao.py:2031` |
| 85 | Gaveta consulta respostas sem veredito | `app/chamados/services/fila_por_causa.py:226` |
| 104 | Reconciliação do destino consulta irmãs | `app/chamados/services/materializacao.py:1244` |
| 133 | Desligamento consulta a lista | `app/colaboradores/services/desligamento.py:165` |
| 134 | Ficha consulta a lista dentro das vivas | `app/colaboradores/services/ficha.py:169` |
| 142 | CSF consulta o mesmo corpo nos universos pai/destino, ignorando tranca | `app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574` |

| R | C — leitura atual: instante | Evidência no HEAD fornecido |
|---|---|---|
| 4 | Respondidas no histórico da API | `app/api/views.py:2577` |
| 5 | `ja_respondida` na resposta da API | `app/api/views.py:2620` |
| 18 | Corrigíveis, com lastro/vitalidade adicionais | `app/chamados/juizes.py:1744` |
| 25 | Contraprova I21a | `app/chamados/management/commands/auditar_invariantes_chamados.py:177` |
| 26 | I2 | `app/chamados/management/commands/auditar_invariantes_chamados.py:592` |
| 30 | C2 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1605` |
| 49 | Saídas pendente/respondida de `estado` | `app/chamados/models.py:3155`; `app/chamados/models.py:3158` |
| 56 | **Property ainda não delega:** instante não nulo | `app/chamados/models.py:3665` |
| 58 | Reconstrução histórica permite ato RESPOSTA pelo instante | `app/chamados/models.py:4091`; `app/chamados/models.py:4098` |
| 64 | Contagem de pendentes na reabertura | `app/chamados/services/acoes_disputa.py:97` |
| 65 | Viva sem instante impede regularizar | `app/chamados/services/acoes_disputa.py:132` |
| 67 | Classificação de beco | `app/chamados/services/becos.py:30` |
| 71 | Instante informa reenvio da declaração | `app/chamados/services/declaracao_texto.py:419` |
| 72 | Ausência de instante dispara gravação da declaração | `app/chamados/services/declaracao_texto.py:687` |
| 76 | Contador da recobrança | `app/chamados/services/disputa_emissao.py:1735` |
| 77 | Instante presente impede reabertura do credor por essa porta | `app/chamados/services/disputa_emissao.py:1843` |
| 89 | Viva sem instante impede regularização pelo fio | `app/chamados/services/fio.py:706` |
| 94 | Instante e limite `desde` selecionam evento histórico | `app/chamados/services/historico.py:105`; `app/chamados/services/historico.py:106` |
| 107 | Seleção da materialização: instante, validação e ausência de materialização | `app/chamados/services/materializacao.py:2064`; `app/chamados/services/materializacao.py:2065`; `app/chamados/services/materializacao.py:2066` |
| 111 | Partição pendente do app | `app/chamados/services/pergunta_no_app.py:81` |
| 116 | Validar retorna `sem_resposta` sem instante | `app/chamados/services/validacao.py:594` |
| 117 | Recusar retorna `ja_pendente` sem instante | `app/chamados/services/validacao.py:813` |
| 121 | Contador temporal: sete dias e posterior à cobrança | `app/chamados/services/verbos.py:335`; `app/chamados/services/verbos.py:342`; `app/chamados/services/verbos.py:345` |
| 123 | Causa `c` do vigia sob âncora terminal | `app/chamados/services/vigia_fio.py:636` |
| 128 | `ja_respondida` na view | `app/chamados/views.py:1280` |
| 129 | Reabertura seleciona instante nulo e depois aplica C6 | `app/chamados/views.py:1416`; `app/chamados/views.py:1417` |
| 130 | Instante presente para validar; nulo para aguardar colaborador | `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:176` |
| 137 | Relavrar órfã seleciona instante presente | `app/ponto/management/commands/reconciliar_grade.py:461` |
| 143 | Contadores operacionais nulo/presente | `bin/censo_folha.py:28`; `bin/censo_folha.py:29` |
| 148 | Instante governa rótulo e caixa do cartão | `app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29` |
| 150 | Instante governa correção da cega e caixa no modal | `app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217` |

| R | D — leitura atual: texto | Evidência no HEAD fornecido |
|---|---|---|
| 8 | SQL com vazio exato, sem trim | `app/chamados/canal.py:65` |
| 31 | Texto após strip interrompe F1 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1657` |
| 34 | Texto carregado alimenta eleição da canônica | `app/chamados/management/commands/backfill_decisoes_pergunta.py:504`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:506` |
| 37 | Texto após strip; depois eleição da canônica | `app/chamados/management/commands/dedup_perguntas_fato.py:75`; `app/chamados/management/commands/dedup_perguntas_fato.py:76` |
| 40 | Ressuscitação seleciona texto NULL ou vazio exato | `app/chamados/models.py:1533` |
| 59 | Texto impede lavrar sem turno | `app/chamados/reconciliador.py:365` |
| 61 | Texto impede ramo de órfã | `app/chamados/reconciliador.py:1083` |
| 74 | Texto existente impede resposta automática | `app/chamados/services/disputa_emissao.py:1663` |
| 81 | `_tem_resp` pelo strip | `app/chamados/services/disputa_emissao.py:2079` |
| 90 | Canônica favorece texto | `app/chamados/services/fio.py:1279` |
| 91 | Mérito perdedor exige texto e via admin | `app/chamados/services/fio.py:1290`; `app/chamados/services/fio.py:1291` |
| 99 | Testemunha exige tipo hora e texto, **depois das vias de recusa** | `app/chamados/services/materializacao.py:862`; `app/chamados/services/materializacao.py:869` |
| 105 | Plantio de hora recusa texto vazio | `app/chamados/services/materializacao.py:1311` |
| 126 | Leitura da resposta sai vazia sem texto após strip | `app/chamados/templatetags/chamados_tags.py:166` |
| 132 | Calendário exclui vazio exato | `app/colaboradores/services/calendario.py:504` |
| 135 | Pendência da ficha pelo texto vazio, fora de `falou` | `app/colaboradores/services/ficha.py:171` |
| 136 | Evidência de fala na prévia | `app/folha/previa.py:142` |
| 138 | Evidência de fala no cartório | `app/ponto/services/cartorio.py:1641` |
| 145 | Verdade crua do texto no cartão do colaborador | `app/templates/chamados/partials/_card_atendimento.html:141`; `app/templates/chamados/partials/_card_atendimento.html:182` |

As **14 delegações complementares** fecham o mapa nominal. As quatro marcadas abaixo faltavam também no b2.r; não atribuo seu nascimento à c4 sem o diff histórico, **[nao verificado]**.

| Classe | Delegações complementares |
|---|---|
| B transitivo — 1 | Gate do pai → CSF: `app/chamados/models.py:719` |
| C — 4 | Contador: `app/chamados/templatetags/chamados_tags.py:226`; **acréscimos ao b2.r:** ação `app/chamados/services/acoes_disputa.py:99`, reabertura pelo colaborador `app/chamados/models.py:2309`, view `app/chamados/views.py:1420` |
| D — 9 | Canônica em declaração: `app/chamados/services/declaracao_texto.py:253`; `app/chamados/services/declaracao_texto.py:275` |
| D — continuação | Canônica no fio: `app/chamados/services/fio.py:1003`; `app/chamados/services/fio.py:1900`; `app/chamados/services/fio.py:2223`; mérito: `app/chamados/services/fio.py:1335` |
| D — continuação | Canônica no backfill: `app/chamados/management/commands/backfill_decisoes_pergunta.py:276`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:393`; **acréscimo ao b2.r:** nascimento `app/chamados/models.py:3031` |

No código da BUG-PAR há **cinco consumidores que já usam J ou sua face SQL**. Eles entram na conferência dos efeitos, sem nova troca de predicado nesses sítios. (`app/chamados/services/materializacao.py:146`; `app/chamados/services/materializacao.py:192`; `app/chamados/services/materializacao.py:259`; `app/chamados/services/materializacao.py:290`; `app/chamados/management/commands/bug_par_103.py:71`.)

| Consumidor | Leitura atual |
|---|---|
| Seleção da irmã respondida | SQL J — `app/chamados/services/materializacao.py:146` |
| Resposta da metade no julgamento do par | J — `app/chamados/services/materializacao.py:192` |
| Elegibilidade da devolução U103 | J, pertencimento congelado, via/carimbo/corte/lâmpada — `app/chamados/services/materializacao.py:259` |
| Situação da irmã | J — `app/chamados/services/materializacao.py:290` |
| Lista fora163 no comando | SQL J — `app/chamados/management/commands/bug_par_103.py:71` |

As leituras novas de texto para **disponibilidade da testemunha**, **escopo da espera** e **interpretação/comparação do declarado** têm finalidade E; não são cópias alternativas de “está respondida?”. A troca indiscriminada dessas guardas por J não corresponde à migração B/C/D descrita no juiz. (`app/chamados/juizes.py:1551`; `app/chamados/services/fio.py:377`; `app/chamados/templatetags/chamados_tags.py:139`; `app/chamados/services/materializacao.py:770`; `app/chamados/services/materializacao.py:718`; `app/chamados/services/materializacao.py:730`.)

O delta relevante dos **oito arquivos cruzados** é:

| Arquivo | O que muda no brief em relação ao b2.r |
|---|---|
| `acoes_disputa.py` | C64/C65 agora em `app/chamados/services/acoes_disputa.py:97` e `app/chamados/services/acoes_disputa.py:132`. A mensagem de sucesso agora vem do serviço e pode nomear a irmã gravada com o par. (`app/chamados/services/acoes_disputa.py:60`; `app/chamados/services/acoes_disputa.py:64`.) |
| `fio.py` | C89 agora em `app/chamados/services/fio.py:706`; canônica/mérito em `app/chamados/services/fio.py:1279` e `app/chamados/services/fio.py:1291`. A testemunha ausente ou que falha passa a produzir `indisponivel`. (`app/chamados/services/fio.py:377`; `app/chamados/services/fio.py:384`.) |
| `materializacao.py` | B104/D99/D105/C107 agora em `app/chamados/services/materializacao.py:1244`, `app/chamados/services/materializacao.py:869`, `app/chamados/services/materializacao.py:1311` e `app/chamados/services/materializacao.py:2064`. A B2 encontra par, K6, espera e vias de recusa antes de chegar às antigas decisões simples. (`app/chamados/services/materializacao.py:192`; `app/chamados/services/materializacao.py:810`; `app/chamados/services/materializacao.py:812`; `app/chamados/services/materializacao.py:862`.) |
| `validacao.py` | Entradas C agora em `app/chamados/services/validacao.py:594` e `app/chamados/services/validacao.py:813`. Validar compõe K6 da pergunta/irmã, pode validar o par e preserva o motivo F6 após rollback. (`app/chamados/services/validacao.py:225`; `app/chamados/services/validacao.py:554`; `app/chamados/services/validacao.py:562`.) |
| `chamados_tags.py` | D126 agora em `app/chamados/templatetags/chamados_tags.py:166`; contador delegado em `app/chamados/templatetags/chamados_tags.py:226`. O cartão ganhou adaptador da testemunha com fallback restritivo. (`app/chamados/templatetags/chamados_tags.py:136`; `app/chamados/templatetags/chamados_tags.py:139`.) |
| `test_estado_aplicacao.py` | A fixture continua sem `respondida_em`; agora fornece também campos do gate de data. A chamada direta de J ainda exigirá completar essa fixture. (`app/chamados/tests/test_estado_aplicacao.py:38`; `app/chamados/tests/test_estado_aplicacao.py:41`; `app/chamados/juizes.py:1552`.) |
| `_pergunta_card.html` | Os C de instante permanecem; dentro deles há agora testemunha, estados K6 próprios, indisponibilidade e espera sem Validar. (`app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/_pergunta_card.html:43`; `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/_pergunta_card.html:49`.) |
| `modal_fio.html` | Os C de instante permanecem; os estados próprios e a espera agora participam da decisão de esconder Validar. (`app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217`; `app/templates/chamados/partials/modal_fio.html:274`.) |

Há, portanto, **interseção comportamental nova comprovada**: a mudança em R99 alcança a testemunha consumida pelo fio, pelo cartão e pelo explicador; a entrada C de validar encontra uma operação capaz de validar/materializar também a irmã. Isso amplia a conferência além das linhas antigas, sem demonstrar necessidade de um nono arquivo de produção no diff. (`app/chamados/services/fio.py:376`; `app/chamados/templatetags/chamados_tags.py:136`; `app/chamados/services/explicador.py:124`; `app/chamados/services/validacao.py:554`; `app/chamados/services/validacao.py:555`.)

**2. TABELA DE ESTADOS**

A tabela é do **código atual**, antes da B2. Validar testa instante antes de validação existente; recusar testa validação existente antes de instante. A apresentação de `estado` prioriza validação, depois vitalidade e materialização. (`app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:597`; `app/chamados/services/validacao.py:810`; `app/chamados/services/validacao.py:813`; `app/chamados/models.py:3151`; `app/chamados/models.py:3154`; `app/chamados/models.py:3156`.)

| Situação | Quem julga | O que a porta faz | O que persiste | Estado | Tela |
|---|---|---|---|---|---|
| Validar: ID inválido/fora da disputa | Busca de `validar_pergunta`, `app/chamados/services/validacao.py:586` | Recusa | Nenhuma escrita nesse ramo | `nao_encontrada`, `app/chamados/services/validacao.py:592` | “Pergunta inválida”, `app/chamados/services/acoes_disputa.py:24` |
| Validar: **sem I**, com ou sem T | Guarda C, `app/chamados/services/validacao.py:594` | Recusa antes de conferir validação existente | Nenhuma escrita nesse ramo | `sem_resposta` | “Aguardando resposta do colab”, `app/chamados/services/acoes_disputa.py:26` |
| Validar: I e validação existente | `app/chamados/services/validacao.py:597` | Não repete validação | Conserva registro | `ja_validada` | Informação de já validada, `app/chamados/services/acoes_disputa.py:28` |
| U163 de intervalo: gravaria hora/tipo diferente do declarado, onde a testemunha diria aplicável | Testemunha/espera, `app/chamados/services/materializacao.py:741`; `app/chamados/services/validacao.py:611` | Recusa antes do implausível e das escritas | Conserva pergunta | `espera_declarado_x_gravado`, `app/chamados/services/validacao.py:613` | Rótulo de espera; sem Validar nos dois cartões, `app/templates/chamados/partials/_pergunta_card.html:49`; `app/templates/chamados/partials/modal_fio.html:274` |
| Implausível sem confirmação, após as saídas anteriores | `motivo_implausivel_validacao`, `app/chamados/services/validacao.py:615` | Recusa antes do carimbo | Nenhuma escrita nesse ramo | `implausivel` | Aviso do serviço, `app/chamados/services/acoes_disputa.py:30` |
| Metade fora163, **sem dia do fato**, após as saídas anteriores | `entrega_que_barra_a_validacao` → K6, `app/chamados/services/validacao.py:652`; `app/chamados/services/materializacao.py:810` | Recusa antes do carimbo; K6 não é consultado sem dia | Conserva pergunta/atos | `bloqueada_sem_dia_do_fato`, `app/chamados/services/validacao.py:233` | Mostra motivo; sem Validar, `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/modal_fio.html:274` |
| Metade fora163 com competência do fato **ENTREGUE**, inclusive irmã abrangida pelo ato | Mesma composição, `app/chamados/services/validacao.py:225`; `app/chamados/services/materializacao.py:812` | Recusa antes do carimbo | Conserva pergunta/atos | `bloqueada_competencia_entregue`, `app/chamados/services/validacao.py:234` | Mostra preservação para Pauta DP; sem Validar, `app/chamados/services/materializacao.py:813`; `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/modal_fio.html:274` |
| Competência apenas fechada/aprovada, F6, sem K6 impeditivo | Testemunha consulta F6; porta conserva o toque, `app/chamados/services/materializacao.py:830`; `app/chamados/services/validacao.py:212` | Oferece toque; plantio pode recusar no chokepoint | No caso do par recusado, savepoint volta e motivo F6 é gravado pela recusa canônica | Testemunha `bloqueada_competencia`; porta pode retornar `nao_gravou` com devolução, `app/chamados/services/validacao.py:562`; `app/chamados/services/validacao.py:727` | **Conserva Validar**; informa motivo, `app/templates/chamados/partials/_pergunta_card.html:48`; prova estática em `app/chamados/tests/test_bug_par_volta.py:249` |
| Testemunha ausente/falhou na montagem da tela | Adaptadores do fio/cartão, `app/chamados/services/fio.py:377`; `app/chamados/services/fio.py:384`; `app/chamados/templatetags/chamados_tags.py:139` | A apresentação não oferece Validar | Nenhuma escrita pelos adaptadores | `indisponivel` **da testemunha** | Aviso e ausência de botão, `app/templates/chamados/partials/_pergunta_card.html:45`; `app/templates/chamados/partials/modal_fio.html:274` |
| Folga sem dia | `_aceitar_a_folga`, `app/chamados/services/validacao.py:414` | Recusa | Nenhuma escrita nesse ramo | `folga_sem_dia` | Mensagem do serviço pela casca, `app/chamados/services/validacao.py:416`; `app/chamados/services/acoes_disputa.py:50` |
| Folga já declarada | `_aceitar_a_folga`, `app/chamados/services/validacao.py:424` | Valida, encerra pergunta e pede fecho do fio | Validação/ato e carimbo; não redeclara o dia, `app/chamados/services/validacao.py:432`; `app/chamados/services/validacao.py:440` | `folga_ja_declarada` | Mensagem de dia já declarado, `app/chamados/services/validacao.py:446`; `app/chamados/services/acoes_disputa.py:56` |
| Folga recusada na checagem ou escrita | `app/chamados/services/validacao.py:450`; `app/chamados/services/validacao.py:480` | Recusa; falha da escrita reverte atomic | Conserva pergunta após recusa | `folga_recusada`, `app/chamados/services/validacao.py:453`; `app/chamados/services/validacao.py:494` | Motivo do serviço, `app/chamados/s

[truncado em 20000 de 36092 caracteres]

## 16/11 21:51 · sessao 09356da2f63b · AUSENCIA-EFEITO-PELO-ESTADO · verificador · origem=Code

**PROCEDENTE a ressalva ao cumprimento integral:** a contagem facetada está implementada, mas o literal “qualquer requisição” tem contraexemplo nos hrefs; também entrou um selo AST adicional apesar de “nenhum instrumento novo”. (`app/ponto/views.py:2281-2291`; `app/templates/ponto/ausencias.html:48`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:492-546`; `complemento3_ausencia_efeito_1611.txt:8`.)

Execução da suíte Django, morte efetivamente executada dos mutantes, SQL medido e ocorrências produtivas: **[nao verificado]**. Os dois métodos do selo AST passaram quando extraídos e executados em memória, sem Django/BD; essa verificação alcança somente suas asserções estáticas. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:526-546`.)

**1. CHECKLIST**

“Cumpre” abaixo significa conformidade demonstrável na implementação ou nos asserts; não certifica execução verde. **[nao verificado]** [nao verificado]

| Cláusula | Veredito | Onde e por quê |
|---|---|---|
| C3 MUDA (a) | **Cumpre**, no âmbito lista/chips | As duas portas concentram os cortes e atendem pares em memória e queryset. Existe uma consulta separada de alertas com `status='aprovada'`, tratada no item 3(e). (`app/ponto/views.py:2251-2271,2296-2299`.) |
| C3 MUDA (b) | **Cumpre** | Todos e chips de status somam sobre `_sob_efeito`. (`app/ponto/views.py:2281-2287`.) |
| C3 MUDA (c) | **Cumpre** | Chips de efeito somam sobre `_sob_status`. (`app/ponto/views.py:2282,2288-2289`.) |
| C3 MUDA (d) | **Cumpre** | A lista recebe exatamente `por_efeito(por_status(_base, status_filtro), efeito_filtro)`. (`app/ponto/views.py:2291`.) |
| C3 MUDA (e) | **Cumpre no delta fornecido** | Os hrefs conservam a outra dimensão e `q_universo`; o template não integra o delta apresentado. (`app/templates/ponto/ausencias.html:48-67`; `app/ponto/views.py:2220-2230,2339`.) |
| C3 RESULTADO 5 | **Cumpre em parte** | Fecha para os valores usuais; falha para valor desconhecido contendo separador de query, conforme contraexemplo do item 4. (`app/ponto/views.py:2260-2262,2281-2283`; `app/templates/ponto/ausencias.html:48`.) |
| C3 RED 1 | **Cumpre no teste escrito** | Exige Suprime `(0,0)` na origem Rejeitada. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:389-391`.) |
| C3 RED 2 | **Cumpre no teste escrito** | Exige Rejeitada `(1,1)` na origem Aprovada. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:393-395`.) |
| C3 RED 3 | **Cumpre a enumeração pedida** | São 24 origens, sete chips obrigatórios e navegação pelo href de cada um, com datas válidas. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:423-440`.) |
| M-a, corrigido pelo C4 | **Cumpre logicamente** | Contar efeito sobre base crua produz Suprime 1 onde o RED 1 exige 0. Execução mutante **[nao verificado]**. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:379-391`; `complemento4_ausencia_efeito_1611.txt:3`.) |
| M-b | **Cumpre logicamente** | Conservar o corte Aprovada no contador Rejeitada produz 0 onde o RED 2 exige 1. Execução mutante **[nao verificado]**. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:379-395`.) |
| C3 SELO | **Cumpre em parte** | RED 3 existe, mas também foi acrescentado `SeloPortasDaFacetadaTest`, instrumento distinto do laço. No literal, contraria “nenhum instrumento novo”. (`complemento3_ausencia_efeito_1611.txt:8`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:418-440,492-546`.) |
| C3 PROIBIDO, após C4 | **Cumpre quanto aos cortes e contagens examinados** | Cortes das dimensões permanecem nas portas; `pendentes` e orçamento das contagens seguem as substituições expressas do C4. A ressalva do instrumento adicional permanece acima. (`app/ponto/views.py:2251-2291`; `complemento4_ausencia_efeito_1611.txt:4-5`.) |
| C4 (a) | **Cumpre** | Todos conta sob efeito, sem conservar status; a partição anterior continua exigida sem cortes. O exemplo literal possui asserts próprios. (`app/ponto/views.py:2281-2289`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:402-415`.) |
| C4 (b) | **Cumpre no teste escrito** | Acrescentado RED M-c: sob Suprime, Rejeitada `(0,0)` e Todos `(1,1)`. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:397-400`.) |
| C4 (c) | **Cumpre** | `pendentes = por_status(_base, 'pendente').count()`. (`app/ponto/views.py:2277`.) |
| C4 (d) | **Cumpre na estrutura do código** | Uma consulta agrupa por `(tipo,status)` e classifica pelo juiz; todos os chips somam `n`. O juiz usa esses dois campos e catálogo vigente. (`app/ponto/views.py:2175-2178,2273-2289`; `app/ponto/services/dia_decidido.py:467-495`.) |

**Algo não pedido no delta — PROCEDENTE para o selo adicional.** Não há alteração apresentada de template, juiz, catálogo, modelo/status, CSS ou dados persistidos; as alterações funcionais examinadas ficam na montagem dos pares e facetas. O acréscimo estrutural não solicitado é o varredor e sua classe de selo. (`app/ponto/views.py:2165-2291`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:477-546`; `complemento3_ausencia_efeito_1611.txt:8`.) Correspondência independente entre o delta transcrito e o hash anunciado: **[nao verificado]**.

**2. QUEM ESCREVE E QUEM LÊ**

| Valor | Escritor/origem | Leitor, incluindo fora do delta |
|---|---|---|
| GET `status` | Chips de status escrevem os valores; chips de efeito e formulário preservam o recebido. (`app/templates/ponto/ausencias.html:52-67,75`.) | Leitura inicial em `status_filtro`; usada na faceta de efeito, lista e contexto. (`app/ponto/views.py:2191,2282,2291,2337`.) |
| GET `efeito` | Chips de efeito escrevem `abona/desconta/suprime`; chips de status preservam o recebido. (`app/templates/ponto/ausencias.html:48-67`.) | Leitura em `efeito_filtro`; usada na faceta de status, lista e contexto. (`app/ponto/views.py:2236,2281,2291,2335`.) |
| `_pares.tipo/status` | Saem de `values('tipo','status')` sobre `_base`. (`app/ponto/views.py:2177-2178,2240`.) | O juiz lê ambos; `por_status` lê status; `por_efeito` usa ambos para reconstruir os pares SQL. (`app/ponto/views.py:2177,2256,2270`.) |
| `_pares.n` | `Count('pk')` do agrupamento. (`app/ponto/views.py:2178`.) | `_soma`, chamada por todos os chips. (`app/ponto/views.py:2273-2289`.) |
| `_pares.veredito` | `DD.veredito_do_tipo(tipo,status)`. (`app/ponto/views.py:2177`.) | `por_efeito`, nos dois ramos. (`app/ponto/views.py:2263-2271`.) |
| `_status_do_filtro` | Mapa local; Pendente deriva de `_A4_ABERTAS`, importada da autoridade das filas. (`app/ponto/views.py:79,2245-2246`; `app/ponto/services/ausencia.py:912`.) | Somente `por_status` resolve esse mapa. (`app/ponto/views.py:2252`.) |
| `_veredito_do_filtro` | Deriva do campo `filtro` do mapa de apresentação do juiz. (`app/ponto/views.py:2242`; `app/ponto/services/dia_decidido.py:80-84`.) | `por_efeito` e laço dos contadores de efeito. (`app/ponto/views.py:2260,2288-2289`.) |
| `pendentes` | Contagem separada da caixa aberta sobre `_base`; entregue ao contexto. (`app/ponto/views.py:2277,2325`.) | Leitor explícito encontrado: `C4cPendentesTest`. O chip usa `count_pendente`, outro valor. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:443-448`; `app/templates/ponto/ausencias.html:54`.) |
| `_cnt` / `cnt` / `count_*` | Somas facetadas; aliases saem do mesmo `_cnt`. (`app/ponto/views.py:2281-2289,2335-2336`.) | Template lê quatro aliases de status e três `cnt.*`; testes anteriores leem contexto e HTML. (`app/templates/ponto/ausencias.html:50-67`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:146-152,179-210`.) |
| `filtro_status` / `filtro_efeito` | Contexto recebe os valores crus dos GET. (`app/ponto/views.py:2335-2337`.) | Template usa nos hrefs, marcação ativa e hidden de status. (`app/templates/ponto/ausencias.html:48-67,75`.) |
| Lista `ausencias` | Portas produzem `qs`; ele vira lista e recebe atributos de apresentação. (`app/ponto/views.py:2291,2313-2324`.) | Template percorre as linhas; testes anteriores inspecionam os registros. (`app/templates/ponto/ausencias.html:142-179`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:128-129,223-224`; `app/ponto/tests/test_lista_ausencias.py:19-31`.) |
| Retorno de `_pares_por_veredito` | Agora é lista de dicionários com quatro chaves. (`app/ponto/views.py:2177-2178`.) | Única chamada produtiva localizada: atribuição a `_pares`; o selo anterior inspeciona AST, sem consumir baldes. (`app/ponto/views.py:2240`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:297-307`.) |

**Dependência externa do antigo dict — NAO PROCEDENTE na cópia examinada.** Não foi localizado consumidor produtivo adicional; os testes anteriores que mencionam a função verificam sua presença e chamada única. (`app/ponto/views.py:2240`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:297-307`.)

**Dependência do universo antigo dos chips — NAO PROCEDENTE nos leitores localizados.** O template apenas apresenta os números; a partição antiga permanece testada na requisição sem cortes, compatível com C4(a). (`app/templates/ponto/ausencias.html:50-67`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:179-186`; `complemento4_ausencia_efeito_1611.txt:2`.)

**Significados — coerentes no caminho válido.** Pendente é caixa das duas filas, não status persistido; `pendentes` ignora as dimensões pedidas, enquanto `cnt.pendente` respeita o efeito pedido. Essa diferença é explícita, autorizada e não é divergência de predicado. (`app/ponto/services/ausencia.py:912`; `app/ponto/models.py:690-705`; `app/ponto/views.py:2277,2281-2285`; `complemento4_ausencia_efeito_1611.txt:4`.)

O veredito empregado é o efeito visual do juiz: abertas conservam o efeito do tipo e rejeitada o sobrepõe. A função que responde PENDENTE para lançamento aberto é outra pergunta, `veredito_do_lancamento`, que esta lista não chama. (`app/ponto/services/dia_decidido.py:453-495`; `app/ponto/views.py:2177,2321`.)

**3. CLASSES QUE JÁ REPROVARAM**

**(a) Segundo juiz — NAO PROCEDENTE no delta.** Agrupamento chama `veredito_do_tipo`; coluna chama `veredito_de`, que delega à mesma função. A regra de rejeição continua no juiz. O `ANEXO!` consulta sua autoridade própria sobre documento e tipo, sem determinar o veredito da coluna. (`app/ponto/views.py:2177,2321`; `app/ponto/services/dia_decidido.py:475-495`; `app/templates/ponto/ausencias.html:158`; `app/ponto/services/ausencia.py:745-759`.)

**(b) Derivação paralela — NAO PROCEDENTE para os cortes usuais; PROCEDENTE a possibilidade de chip diferente do clique.** Os dois ramos usam os mesmos estados e pares julgados; o contraexemplo encontrado vem da serialização do href, detalhada no item 4. (`app/ponto/views.py:2251-2271`; `app/templates/ponto/ausencias.html:48-67`.)

**(c) RED/selo sem mordida — PROCEDENTE em parte:**

- **C3FacetadaTest:** tem mordida contra M-a, M-b e M-c pelos resultados absolutos exigidos; não depende apenas de comparar dois resultados possivelmente errados. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:379-405`.)
- **C3Red3LacoTest:** não é vazio: exige os sete chips, zero observado e número maior que um. Entretanto, não cobre valores desconhecidos/vazios no laço, valores contendo separadores, registro nem tipo fora do catálogo; o contraexemplo de href passa fora desse domínio. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:423-440`.)
- **C4cPendentesTest:** verifica valor e independência dos filtros. Isoladamente, passaria com a antiga contagem direta das duas filas, porque ela também produz seis; a exigência de passar pela porta depende do selo estrutural. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:443-448,526-534`; `app/ponto/services/ausencia.py:912`.)
- **C4dCatalogoUnicoTest:** morde leitura congelada do catálogo, mas não prova identidade do juiz: uma regra duplicada que lesse o mesmo catálogo poderia passar. O filtro também é conferido apenas por inclusão de um registro; isoladamente, esse assert passaria se o filtro devolvesse registros a mais. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:460-474`.)
- **SeloPortasDaFacetadaTest:** tem escape comprovado. Ignora corpos das portas; uma `.count()` extra dentro delas deixa o resultado do detector igual a `([], ['por_status'])`. Também ignora cortes por aliases fora das raízes enumeradas e leitura por `.get('veredito')`. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:504-516,529-534`.)

A suíte contém outras verificações capazes de pegar parte dessas regressões comportamentais; isso não amplia o alcance do selo nem cobre o contraexemplo de serialização. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:154-161,423-440`.)

**(d) Campo com dois significados — NAO PROCEDENTE como regressão introduzida pelo delta; há ressalva preexistente.** A coluna Status chama qualquer estado diferente de aprovada/rejeitada de Pendente. Para os quatro estados declarados isso coincide com as duas filas; para um estado persistido inválido ou legado, a coluna diria Pendente e o chip não o incluiria. Existência desse dado: **[nao verificado]**. (`app/templates/ponto/ausencias.html:176-179`; `app/ponto/models.py:690-705`; `app/ponto/views.py:2245-2257`.)

**(e) Predicado fora das portas — NAO PROCEDENTE para repetição dos cortes da lista/chips.** Existe, porém, `status='aprovada'` fora delas na consulta dos alertas médicos. Portanto, “nenhum filtro de status em lugar algum da view” seria falso; essa consulta responde outra pergunta e não corta a lista nem seus chips. O selo a deixa passar por sua seleção de raízes. (`app/ponto/views.py:2295-2299`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:508-509`.)

**(f) Contagens — NAO PROCEDENTE a permanência das contagens SQL dos chips.** Os sete números saem de somas do agrupamento; permanece uma `.count()` separada para `pendentes`. Isso não torna a view inteira uma consulta: há lista, alertas, colaboradores e possíveis leituras de catálogo. Total SQL real: **[nao verificado]**. (`app/ponto/views.py:2177-2178,2273-2289,2295-2300,2313`; `app/ponto/catalogo/ausencias.py:249-251,267-281`.)

**4. BORDAS**

| Requisição/caso | Veredito e comportamento pelo código |
|---|---|
| `?status=xyz` | **NAO PROCEDENTE** corte indevido: desconhecido retorna o alvo inteiro. Todos não fica visualmente ativo, porque o template testa a existência do valor, embora não haja corte. (`app/ponto/views.py:2252-2254`; `app/templates/ponto/ausencias.html:49`.) |
| `?status=` | **NAO PROCEDENTE** divergência: sem corte; omitir esse vazio no href conserva a mesma seleção. (`app/ponto/views.py:2252-2254`; `app/templates/ponto/ausencias.html:65-67`.) |
| `?efeito=abona&status=aguardando_documento` | **NAO PROCEDENTE** divergência entre portas: lista é a interseção da fila com os pares ABONADO. O juiz mantém ABONADO para tipo abonador aberto. (`app/ponto/views.py:2251-2271,2291`; `app/ponto/services/dia_decidido.py:475-487`.) |
| Efeito simples fora do mapa ou vazio | **NAO PROCEDENTE** corte indevido: retorna o alvo. Há teste anterior para `xyz`, vazio, `rejeitado` e `abonado`. (`app/ponto/views.py:2260-2262`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:212-215`.) |
| Efeito válido sem par | **NAO PROCEDENTE** retorno de tudo: contador soma zero e queryset recebe `.none()`. (`app/ponto/views.py:2263-2271,2289`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:203-210`.) |
| `registro=<pk>` com status incompatível | **NAO PROCEDENTE** escape do corte: PK substitui o universo antes das portas; status incompatível deixa a lista vazia. Os chips conservam o PK reconhecido e podem remover/substituir sua própria dimensão. (`app/ponto/views.py:2207-2214,2225-2226,2291`; `app/templates/ponto/ausencias.html:48-67`.) |
| Tipo fora do catálogo | **NAO PROCEDENTE** erro de classificação entre grupo e coluna: catálogo retorna `None`; juiz cai em ABONADO, salvo rejeitada. A apresentação contém ambas as chaves. Trata-se do comportamento existente, não de validação desse cadastro. (`app/ponto/catalogo/ausencias.py:293-295`; `app/ponto/services/dia_decidido.py:80-84,475-495`; `app/ponto/views.py:2177,2321-2322`.) |
| `de/ate` válidos | **NAO PROCEDENTE** perda do universo nos hrefs: valores normalizados entram em `q_universo`; os testes seguem os links com recorte. (`app/ponto/views.py:2202-2206,2220-2224,2339`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:336-347,430-436`.) |
| `alvo` lista × queryset | **NAO PROCEDENTE** predicados diferentes para os alvos usados: status usa o mesmo `_st`; efeito seleciona os mesmos pares julgados. O OR sobre `_pares` completos é intersectado com o queryset já cortado. (`app/ponto/views.py:2251-2271,2291`.) |

**Contraexemplo PROCEDENTE ao RESULTADO 5:** origem `?mes=4&ano=2026&efeito=xyz%26status%3Drejeitada`. O efeito recebido é `xyz&status=rejeitada`, desconhecido, portanto Todos conta toda a base. O href renderizado de Todos contém `&efeito=xyz&status=rejeitada`; ao clicar, aparece um filtro de status que não existia na origem. No cenário de abril já definido no teste, a derivação é **chip 2, clique 1**. É falha preexistente do template alcançada pelo contrato novo; execução HTTP desse caso: **[nao verificado]**. (`app/ponto/views.py:2236,2260-2262,2281-2283,2291`; `app/templates/ponto/ausencias.html:48`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:379-383`.)

Há o simétrico com `status=xyz%26efeito%3Dsuprime`: o href Abona passa a conter dois parâmetros `efeito`, introduzindo outra seleção no clique. Os valores preservados recebem escape HTML, mas não codificação de componente de URL; somente `q_universo` é montado com `urlencode`. (`app/ponto/views.py:2191,2252-2254,2282,2289,2339`; `app/templates/ponto/ausencias.html:65`.)

**500 nas bordas simples enumeradas: NAO PROCEDENTE como defeito demonstrado pelo trecho; resposta HTTP efetiva [nao verificado].** Igualdade temporal absoluta entre grupo e coluna também não está certificada: ambos consultam a mesma autoridade, mas o catálogo tem cache expirável e invalidável, sem fotografia explícita por requisição. Mudança concorrente ou durante expiração: **[nao verificado]**. (`app/ponto/views.py:2177,2321`; `app/ponto/catalogo/ausencias.py:257-281`.)

## 16/11 21:49 · sessao aed6460a09d2 · - · - · origem=Ronald (tmux)

| Situação | Quem julga → porta | Persistência / retorno |
|---|---|---|
| ID inválido | `validar_pergunta` busca pergunta/disputa | Nenhuma escrita; `nao_encontrada`. (`app/chamados/services/validacao.py:586-593`) |
| Sem instante, com ou sem texto | Guarda anterior à validação existente | Nenhuma escrita; `sem_resposta`. Continua leitor C a migrar. (`app/chamados/services/validacao.py:594-599`) |
| Já validada | Guarda após instante | Conserva registro; `ja_validada`. (`app/chamados/services/validacao.py:597-599`) |
| Pergunta ou irmã que o ato gravaria barrada por K6 | `entrega_que_barra_a_validacao` → `_k6_que_barra` | Retorno distinto para `bloqueada_sem_dia_do_fato` e `bloqueada_competencia_entregue`. A irmã integra a composição; c4c dispensa sua busca quando a própria pergunta já basta para consultar o mesmo K6. (`app/chamados/services/validacao.py:209-234`) |
| Par recusado no chokepoint por competência aprovada/F6 | `_validar_o_par` | Rollback do savepoint do par; grava motivo pela recusa canônica; sentinela impede tentar a metade sozinha e substituir o motivo por paridade. (`app/chamados/services/validacao.py:536-564`) |
| K6 muda durante ato | Reconsulta depois da materialização | Rollback integral antes da DEVOLUÇÃO; devolve estado próprio K6. (`app/chamados/services/validacao.py:688-695`) |
| Materialização recusada, resposta HORA, sem encerramento, com motivo | `devolver_validacao_recusada` | Desfaz validação, conserva resposta; pode reabrir disputa; exige motivo do escritor canônico. (`app/chamados/services/validacao.py:247-259`) |
| Recusa repetida sem mudança | `devolvida is None` | `nao_gravou`, sem novo evento de mudança; pergunta segue com admin. (`app/chamados/services/validacao.py:702-706`) |
| Devolução ou erro próprio após materialização | Separação dos erros por pergunta | `nao_gravou`; com devolução explicita que validação não ficou. (`app/chamados/services/validacao.py:716-732`) |
| Reuso | Materialização / `reusos` | `ja_no_espelho`, mensagem do serviço. (`app/chamados/services/validacao.py:739-743`) |
| Par gravado | `_validar_o_par` | `validada`, inclui PK da irmã e mensagem de gravação conjunta. (`app/chamados/services/validacao.py:744-747`) |
| Caminho comum concluído | `validar_pergunta` | `validada`. (`app/chamados/services/validacao.py:748-749`) |
| Recusar sem motivo / já validada / sem instante / fora do fato | Guardas próprias, nessa ordem | Respectivamente `sem_motivo`, `ja_validada`, `ja_pendente`, `fora_do_fato`; precedência diferente de validar. (`app/chamados/services/validacao.py:798-822`) |
| Recusar elegível | Motivo + `reabrir_para_correcao`, atomic com trava do colaborador | Zera par pela porta, ato `recusa`; depois nota, auditoria e tentativa de push; `recusada`. (`app/chamados/services/validacao.py:823-865`) |

Interseções novas relevantes: B2 pode alterar a composição do par que será validado/materializado e, por consequência, os motivos e atos de recusa F6; “nenhuma batida nova” não demonstra “nenhum efeito”. (`app/chamados/services/validacao.py:536-564`.) K6 da irmã é parte da decisão da porta e precisa constar do brief; não é apenas atributo da pergunta clicada. (`app/chamados/services/validacao.py:209-225`.)

Não encontrei decisão de lei nova nesses trechos: os estados novos compõem as guardas e distinguem seus resultados; o predicado de entrada ainda continua só instante. Uma pergunta de lei adicional necessária por esses ramos: [nao verificado]. (`app/chamados/services/validacao.py:228-234`; `app/chamados/services/validacao.py:594-599`.)

## 16/11 21:47 · sessao 1694869c524a · - · - · origem=Ronald (tmux)

- **PROCEDENTE — há contraexemplo ao “qualquer R” por serialização dos hrefs.** `R=?efeito=xyz%26status%3Drejeitada`: a porta recebe `xyz&status=rejeitada`, desconhecido, e não corta; Todos soma toda `_base`. O href Todos concatena esse valor sem `urlencode`, tornando o clique `?…&efeito=xyz&status=rejeitada`: agora corta rejeitadas. Com os dois registros de abril do teste, Todos conta 2 e o clique lista 1. O problema está no template preexistente, alcançado pelo contrato amplo C3, não numa nova derivação de efeito. (`app/ponto/views.py:2236,2260-2262,2281-2283,2291`; `app/templates/ponto/ausencias.html:48`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:379-382`.)

- **PROCEDENTE — caso simétrico.** `R=?status=xyz%26efeito%3Dsuprime`: status desconhecido não corta; chip Abona conta abonados, mas seu href produz `efeito=abona&status=xyz&efeito=suprime`, fazendo o último efeito prevalecer no clique. A concatenação também existe nos demais chips. Parsing demonstrado somente em memória; ocorrência produtiva **[nao verificado]**. (`app/ponto/views.py:2191,2236,2252-2254,2282,2288-2289`; `app/templates/ponto/ausencias.html:48,52,56,60,65-67`.)

- **NAO PROCEDENTE — `status=xyz` simples ou `status=` causarem corte indevido/500 pelo delta.** Ambos retornam o alvo sem corte; efeito simples desconhecido/vazio faz o mesmo. Status desconhecido não marca Todos como ativo porque o template usa verdade do valor, mas isso não altera a lista. (`app/ponto/views.py:2252-2254,2260-2262`; `app/templates/ponto/ausencias.html:49`.)

- **NAO PROCEDENTE — `efeito=abona&status=aguardando_documento` divergir pela implementação dos predicados.** A lista intersecta o status com o efeito; o juiz atual dá ABONADO ao tipo abonador mesmo em fila aberta, porque só rejeitada sobrepõe o efeito. O lançamento pendente é pergunta separada em `veredito_do_lancamento`; a lista consulta `veredito_de`. (`app/ponto/views.py:2291,2321`; `app/ponto/services/dia_decidido.py:453-464,475-487,490-495`.)

- **NAO PROCEDENTE — registro com status incompatível escapar do filtro.** O PK substitui o universo antes de colaborador/tipo e antes das portas; o corte incompatível produz lista vazia. Os hrefs conservam o PK reconhecido, e cada chip troca somente sua dimensão. (`app/ponto/views.py:2207-2214,2225-2226,2235,2291`; `app/templates/ponto/ausencias.html:48-67`.)

- **NAO PROCEDENTE — tipo fora do catálogo causar 500 ou outra classificação na coluna.** O catálogo devolve `None`; o juiz cai em ABONADO; agrupamento e coluna chamam o mesmo núcleo, e a apresentação tem essa chave. A existência produtiva de tais tipos é **[nao verificado]**. (`app/ponto/catalogo/ausencias.py:293-295`; `app/ponto/services/dia_decidido.py:80-84,482-487,495`; `app/ponto/views.py:2177,2321-2322`.)

- **NAO PROCEDENTE — predicados diferentes nos ramos lista/queryset, para os alvos usados.** Status compara pertinência ao mesmo `_st`; efeito compara o veredito agrupado, monta OR dos mesmos pares e intersecta o queryset recebido. Ausência de pares vira `.none()`. O ramo queryset usa `_pares` de toda `_base`, mas os cortes anteriores do próprio queryset conservam a interseção. (`app/ponto/views.py:2240,2251-2271,2281-2291`.)

- **NAO PROCEDENTE — segundo juiz novo no delta.** O agrupamento chama `veredito_do_tipo`; a coluna chama `veredito_de`, que delega àquela função; a regra “rejeitada vence” permanece no juiz. (`app/ponto/views.py:2177,2321`; `app/ponto/services/dia_decidido.py:475-495`.)

- **Ressalva de leitura do literal “um sítio”:** permanece `status='aprovada'` fora das portas no universo próprio dos alertas médicos, sem filtrar a lista ou os chips. Sua presença é demonstrável; tratá-la como repetição do predicado da dimensão exigiria ignorar que responde outra pergunta. (`app/ponto/views.py:2295-2299`.)

- **NAO SEI — igualdade temporal de catálogo em toda execução.** Agrupamento e coluna consultam o mesmo catálogo, mas não uma fotografia explícita: `vigente()` tem cache de 60 segundos e pode ser invalidado. Mudança entre as duas classificações pode mudar a coluna; concorrência ou expiração durante requisição produtiva é **[nao verificado]**. (`app/ponto/views.py:2177,2321`; `app/ponto/catalogo/ausencias.py:257-281`.)

## 16/11 21:47 · sessao e2b57ed680ee · - · - · origem=Ronald (tmux)

Censo de escritores/leitores, pela cópia:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| GET `status` | Chips de status escrevem `pendente/aprovada/rejeitada`; chips de efeito e formulário preservam o recebido (`app/templates/ponto/ausencias.html:52-67,75`). | `status_filtro = request.GET.get('status')`; portas de números/lista e contexto (`app/ponto/views.py:2191,2282,2291,2337`). |
| GET `efeito` | Chips de efeito escrevem `abona/desconta/suprime`; chips de status preservam o recebido (`app/templates/ponto/ausencias.html:48-67`). | `efeito_filtro = request.GET.get('efeito')`; portas de números/lista e contexto (`app/ponto/views.py:2236,2281,2291,2335`). |
| `_pares` / retorno de `_pares_por_veredito` | `tipo/status` saem de `values`; `n` de `Count('pk')`; `veredito` de `DD.veredito_do_tipo`; retorno é lista de dicionários (`app/ponto/views.py:2175-2178,2240`). | `por_status` lê `status`; `por_efeito` lê `veredito/tipo/status`; `_soma` lê `n`; facetas leem as portas (`app/ponto/views.py:2255-2274,2281-2289`). |
| `_status_do_filtro` | Mapa local, `pendente` deriva de `_A4_ABERTAS`, quatro estados individuais (`app/ponto/views.py:2245-2246`). | Só `por_status` (`app/ponto/views.py:2252`). Fonte externa das abertas: import em `app/ponto/views.py:79`; tupla em `app/ponto/services/ausencia.py:912`. |
| `_veredito_do_filtro` | Compreensão do `APRESENTACAO_AUSENCIA`, excluindo filtros vazios (`app/ponto/views.py:2242`; mapa em `app/ponto/services/dia_decidido.py:80-84`). | `por_efeito` resolve pedido; laço escreve contadores de efeito (`app/ponto/views.py:2260,2288-2289`). |
| `pendentes` | Contagem separada de `por_status(_base,'pendente')`; exportada ao contexto (`app/ponto/views.py:2277,2325`). | Leitor explícito localizado: teste C4c (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:444-448`). Nenhum consumo produtivo explícito desse contexto localizado no template; outros `pendentes` da árvore pertencem a outros contextos [nao verificado]. |
| `_cnt`, `cnt`, `count_*` | Facetas somam `n`; aliases de status saem do mesmo `_cnt` (`app/ponto/views.py:2281-2289,2335-2336`). | Template usa quatro aliases e três `cnt.*` (`app/templates/ponto/ausencias.html:50-67`); testes anteriores leem contexto e HTML (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:149-152,181-208,338,361`). |
| `filtro_status/filtro_efeito` | Contexto recebe os GET crus (`app/ponto/views.py:2335-2337`). | Hrefs, classes ativas e hidden de status (`app/templates/ponto/ausencias.html:48-67,75`). |
| Lista `ausencias` | Portas produzem `qs`; materialização e enriquecimento; publicação no contexto (`app/ponto/views.py:2291,2313-2324`). | Template percorre linhas (`app/templates/ponto/ausencias.html:142-185`); helper e testes anteriores (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:129,223-224`; `app/ponto/tests/test_lista_ausencias.py:21-31`). |

**Consumidor do antigo dict: NAO PROCEDENTE na cópia examinada.** A única chamada produtiva localizada é `lista_ausencias`; os testes fora do delta apenas inspecionam AST da função e quantidade de chamadas, sem consumir seus baldes (`app/ponto/views.py:2240`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:297-307`). Não localizei leitor executável exigindo o universo antigo; a partição anterior permanece testada na requisição sem cortes (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:179-186`).

**Semântica válida: NAO PROCEDENTE a alegação de confusão introduzida pelo delta.** Caixa Pendente deriva das duas filas; o veredito do grupo e da coluna vem do mesmo juiz de efeito (`app/ponto/views.py:2245-2257,2177,2321`; `app/ponto/services/ausencia.py:912`). Existe outro juiz, `veredito_do_lancamento`, que responde PENDENTE para abertas, mas a view examinada não o chama (`app/ponto/services/dia_decidido.py:453-464`; `app/ponto/views.py:2177,2321`).

**Ressalva fora do delta:** coluna Status escreve “Pendente” em qualquer `else`; para os quatro estados declarados isso coincide com ABERTAS, mas estado persistido inválido/legado seria chamado Pendente sem entrar no chip. Existência produtiva desse estado: [nao verificado] (`app/templates/ponto/ausencias.html:176-179`; `app/ponto/models.py:690-705`; `app/ponto/views.py:2245-2257`).

**Achado para RESULTADO 5 literal: PROCEDENTE.** Valores desconhecidos contendo separador de query são ignorados pelas portas, mas preservados sem codificação de URL no href; somente `_universo` recebe `urlencode` (`app/ponto/views.py:2252-2254,2260-2262,2339`; `app/templates/ponto/ausencias.html:48-67`). Exemplo derivado: `?mes=3&ano=2026&status=xyz%26tipo%3Dfalta` conta Abona sobre todos os tipos, porém href Abona renderiza status contendo `&tipo=falta`; ao navegar, `tipo=falta` passa a filtrar `_base` e Abona pode abrir zero (`app/ponto/views.py:2190-2191,2213-2214,2282,2289`; `app/templates/ponto/ausencias.html:65`). Na fixture existente isso seria **4 no chip e 0 no clique**, pois há quatro Abona e falta é DESCONTA; execução HTTP desse exemplo [nao verificado] (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:91-100,109-111,149`).

## 16/11 21:47 · sessao ccc5d48acf69 · - · - · origem=Ronald (tmux)

- **C3FacetadaTest — NAO PROCEDENTE a falta de mordida dos mutantes nomeados.** O cenário tem aprovado-suprime e rejeitado-abona, em abril; RED1 exige Suprime `(0,0)`, matando M-a; RED2 exige Rejeitada `(1,1)`, matando M-b; RED M-c exige Rejeitada `(0,0)` sob Suprime, matando contador de status na base crua. Isso é inferência direta dos asserts, sem execução Django. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:379-400`.) Execução verde e mutação real: **[nao verificado]**.

- **C3Red3LacoTest — NAO PROCEDENTE vacuidade geral.** São 24 origens, incluindo as duas filas individuais, e os sete chips precisam existir; cada href é seguido e comparado ao número. Há exigência de zero e de número maior que um. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:423-440`.) **Cumpre a cobertura enumerada do RED3**, mas não inclui status desconhecido/vazio, efeito desconhecido, registro ou tipos fora do catálogo; o conjunto está explícito em `:424-427`, e o universo usa apenas os três tipos em `:91-95`. A igualdade compara quantidade, não pertencimento exato da lista, por `_pks` e `len` (`:128-129,435-436`).

- **C4cPendentesTest — proteção funcional presente, proteção estrutural depende do selo.** Exige seis pendentes independentemente de quatro combinações de filtros; retornar à contagem antiga diretamente com `_A4_ABERTAS` preservaria esses seis e passaria neste teste isolado. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:443-448`; `app/ponto/views.py:2245-2257,2277`.) Não é assert vazio: o fixture cria ambas as filas para três tipos. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:34-35,91-95`.)

- **C4dCatalogoUnicoTest — mordida real contra catálogo congelado.** Troca `efeito_vigente(atestado)` para SUPRIME, exige coluna SUPRIMIDO, Abona zero, Suprime sete e presença do aprovado no filtro; compara também os totais por veredito na coluna. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:456-474`.) **Limite:** uma segunda implementação da regra que também consultasse `CAT.efeito_vigente` poderia passar; esse teste verifica comportamento sob a troca, não a identidade do juiz. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:460-474`.)

- **SeloPortasDaFacetadaTest — PROCEDENTE proteção incompleta.** O varredor só reconhece chamadas com raízes `qs/_base/Q/_Q`, subscritos literais `['status']/['veredito']` e chamadas `.count()`; ignora completamente os corpos das portas. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:502-517`.) Assim, predicado duplicado em alias (`outro = _base; outro.filter(status=...)`), leitura com `.get('veredito')` e contagens extras dentro das portas escapam; os asserts só exigem achados vazios, `['por_status']` e existência das duas funções. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:508-516,526-534`.) Confirmei esses três escapes executando somente as funções AST extraídas em memória, sem arquivos nem BD; o caso com contagens adicionais nas portas devolveu `([], ['por_status'])`. A limitação decorre de `:504-505,508,514-516`.

- **Contagens atuais — NAO PROCEDENTE permanência das sete contagens dos chips no corpo examinado.** Os chips somam `n`; o agrupamento é uma chamada `.values('tipo','status').annotate(n=Count('pk'))`, e `pendentes` conserva sua `.count()` separada. (`app/ponto/views.py:2177-2178,2273-2289`.) **Total SQL executado: [nao verificado]**; o selo não prova esse total pelos limites acima. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:504-513`.)

- **SELO C3 — PROCEDENTE instrumento adicional no delta.** Além do RED3 declarado selo, foi acrescentada a classe AST `SeloPortasDaFacetadaTest`; o C3 diz “o RED 3 é o selo; nenhum instrumento novo”. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:418-440,492-546`; `complemento3_ausencia_efeito_1611.txt:8`.)

## 16/11 21:47 · sessao 89cb77cde8e2 · - · - · origem=Ronald (tmux)

Censo estático fechado pelos sítios R nomeados e chamadas nominais B/C/D com aliases de `ImportFrom`; despacho dinâmico: [nao verificado].

- Permanecem os **66 R** herdados: B16/C31/D19. O corpo B ainda usa OU, C ainda usa instante e D ainda usa texto; a property continua só instante. (`app/chamados/models.py:1924`; `app/chamados/models.py:3665`; `app/chamados/services/validacao.py:594`; `app/chamados/services/materializacao.py:869`.)
- O complemento ao anexo tem **14 sítios**: B transitivo1/C4/D9; não são 14 predicados independentes. (`app/chamados/models.py:719`; `app/chamados/services/acoes_disputa.py:99`; `app/chamados/models.py:2309`; `app/chamados/views.py:1420`; `app/chamados/templatetags/chamados_tags.py:226`; `app/chamados/models.py:3031`; `app/chamados/services/declaracao_texto.py:253`; `app/chamados/services/declaracao_texto.py:275`; `app/chamados/services/fio.py:1003`; `app/chamados/services/fio.py:1335`; `app/chamados/services/fio.py:1900`; `app/chamados/services/fio.py:2223`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:276`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:393`.)

Mudanças de linhas em relação ao b2.r:

| R | HEAD atual |
|---|---|
| B104 | `app/chamados/services/materializacao.py:1244` |
| C64/C65 | `app/chamados/services/acoes_disputa.py:97`; `:132` |
| C89 | `app/chamados/services/fio.py:706` |
| C107 | `app/chamados/services/materializacao.py:2064` |
| C116/C117 | `app/chamados/services/validacao.py:594`; `:813` |
| D90/D91 | `app/chamados/services/fio.py:1279`; `:1290`; `:1291` |
| D99/D105 | `app/chamados/services/materializacao.py:869`; `:1311` |
| D126 | `app/chamados/templatetags/chamados_tags.py:166` |
| C delegado | `app/chamados/templatetags/chamados_tags.py:226` |
| D delegados fio | `app/chamados/services/fio.py:1003`; `:1335`; `:1900`; `:2223` |

A BUG-PAR acrescenta quatro consumidores que **já usam J**; não devem virar cópias B/C/D na contagem: seleção SQL da irmã, resposta da metade, elegibilidade U103 e situação da irmã. (`app/chamados/services/materializacao.py:146`; `:192`; `:259`; `:290`.)

As três guardas textuais novas são **E técnicas**, com finalidade própria: fallback da testemunha no fio, fallback na página e escopo da proteção declarado versus gravado. Migrá-las por busca textual apagaria a distinção entre resposta e disponibilidade/escopo da testemunha. (`app/chamados/services/fio.py:377`; `app/chamados/templatetags/chamados_tags.py:139`; `app/chamados/services/materializacao.py:770`.) A interpretação HH:MM e a comparação hora/tipo também são E. (`app/chamados/services/materializacao.py:718`; `:730`.)

Na antiga D99 existe uma precedência nova que o brief deve preservar: vias materializadas de recusa retornam seu estado **antes** de testar tipo hora/texto; só depois vem o atalho `aplicada`. (`app/chamados/services/materializacao.py:862`; `:869`; `:871`.) A porta de escrita consulta K6 antes de DATA_EM_DISPUTA e a espera declarado versus gravado antes do atomic do plantio. (`app/chamados/services/materializacao.py:1830`; `:1877`; `:1887`.)

A fixture do teste continua sem `respondida_em`, embora agora forneça campos do gate DATA_EM_DISPUTA; a incompatibilidade estática com chamada direta de J permanece. (`app/chamados/tests/test_estado_aplicacao.py:38`; `:41`; `app/chamados/juizes.py:1552`.)

Novas provas estáticas da c4 para conferir na B2: separação F6/K6, competência entregue e sem dia do fato, incluindo a porta e apresentação. Aprovação em execução: [nao verificado]. (`app/chamados/tests/test_bug_par_volta.py:133`; `:233`; `:312`; `:318`.)

## 16/11 21:40 · sessao 950d9935f151 · ESPINHA-F2 · verificador · origem=Code

**Os três itens conferem como aval de conservação; os sete nomes não formam um censo fechado do trio.** O próprio C3 inclui leitores adicionais no selo, e o zero permanece limitado ao predicado investigado pelo C2 (`complemento3_respostas_f2_estados_1611.txt:3`; `complemento2_respostas_f2_estados_1611.txt:4`). Execução do selo, ocorrências em produção e correspondência independente do hash: [nao verificado].

Referências dos anexos contam desde o conteúdo fornecido, incluindo linhas vazias.

**Item 1**

1. **Premissas conferem.** A lista exige ausência de validação e materialização, instante **ou** texto de resposta e tratamento da tranca. O descarimbo efetivo remove a materialização; ressuscitar também apaga instante e texto de resposta, fazendo o controle ficar fora da lista (`app/chamados/models.py:1923`; `app/chamados/models.py:1924`; `app/chamados/models.py:1926`; `app/chamados/models.py:3318`; `app/chamados/models.py:1629`; `app/chamados/models.py:1630`).

2. **Não colide com o vigente.** Adota a correção pedida na devolução e conserva a escolha de perder o instante no descarimbo, mantendo o comportamento anterior (`app/docs/RELATO.md:4`; `app/docs/RELATO.md:31`; `complemento1_respostas_f2_estados_1611.txt:9`).

3. **Fecha no literal.** “Descarimbada” precisa significar descarimbo efetivamente realizado, mantendo a pergunta no universo passado à lista. A preparação do caso deve excluir via terminal, pois essa via faz a porta recusar o descarimbo (`app/chamados/models.py:1922`; `app/chamados/models.py:3316`; `app/chamados/models.py:3392`). “Resposta preservada” corresponde ao instante presente ou texto não branco exigido pelo predicado (`app/chamados/models.py:1924`; `app/chamados/models.py:1925`).

4. **Há alcance declarado no CSF.** A lista alimenta sua permissão de execução; qualquer diferença em competência exportada continua exigindo `PAREI !` com tabela (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574`; `app/ponto/supra_juiz.py:588`; `app/docs/CORTES.md:487`). Efeito financeiro efetivo: [nao verificado].

**Item 2**

1. **Os sete pontos existem, mas são observáveis diferentes.** `respondida` lê `respondida_em`; `resposta_display` lê a resposta, não o trio. `supra_juiz` e `irmas_respondidas_sem_consumo` delegam à lista (`app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/ponto/supra_juiz.py:573`; `app/chamados/juizes.py:2105`).

2. **Não colide com o vigente:** amplia expressamente o selo anterior para os sete e para qualquer leitor adicional, preservando a obrigação geral de conservação (`complemento3_respostas_f2_estados_1611.txt:3`; `respostas_registradas.md:5`).

3. **Fecha como condição extensível, não como inventário encerrado.** Encontrar leitores adicionais obriga incluí-los no selo; não devolve automaticamente este item (`complemento3_respostas_f2_estados_1611.txt:3`). A atribuição integral dos consumidores abaixo ao escopo da matriz F2: [nao verificado].

4. **Não autoriza mudança de comportamento nesses leitores.** Continua exigindo igualdade antes/depois, com a restrição de exportadas preservada (`complemento3_respostas_f2_estados_1611.txt:2`; `complemento2_respostas_f2_estados_1611.txt:5`; `app/docs/CORTES.md:487`). Igualdade efetivamente demonstrada: [nao verificado].

**Resposta ao censo solicitado: não, os sete não esgotam os leitores do trio na cópia.** Há os seguintes consumidores adicionais identificados; inclusive portas que leem antes de escrever (`app/chamados/models.py:3236`; `app/chamados/juizes.py:1390`; `app/ponto/services/procedencia_batida.py:72`).

| Outros leitores | Arquivo:linha |
|---|---|
| `_universo_da_ressuscitacao`: seleciona por instante e via | `app/chamados/models.py:1531`, `app/chamados/models.py:1535` |
| `fechar`; `remover_motivo`: leem via e/ou instante | `app/chamados/models.py:2001`, `app/chamados/models.py:2006`, `app/chamados/models.py:2136`, `app/chamados/models.py:2141` |
| `do_fato`; `q_dos_fatos`; `fora_do_fato`: classificam pela via | `app/chamados/models.py:2829`, `app/chamados/models.py:2860`, `app/chamados/models.py:3311` |
| `fechar_por_admin`; `carimbar`; `carimbar_identidade`: idempotência, foto anterior e preservação de carimbo | `app/chamados/models.py:3165`, `app/chamados/models.py:3166`, `app/chamados/models.py:3236`, `app/chamados/models.py:3292`, `app/chamados/models.py:3293` |
| `descarimbar_em_lote`; encerramento por fato/fantasma; reaberturas | `app/chamados/models.py:3392`, `app/chamados/models.py:3400`, `app/chamados/models.py:3411`, `app/chamados/models.py:3423`, `app/chamados/models.py:3440`, `app/chamados/models.py:3447` |
| `_retratada_com_trilha`; `_pergunta_materializada_de_fato`; chave do memo; projeção temporal; `pergunta_sem_lastro_ou_viva` | `app/chamados/juizes.py:292`, `app/chamados/juizes.py:683`, `app/chamados/juizes.py:711`, `app/chamados/juizes.py:857`, `app/chamados/juizes.py:858`, `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1502` |
| Materialização: guardas, pendentes, grupos de absorção e foto do trio | `app/chamados/services/materializacao.py:440`, `app/chamados/services/materializacao.py:539`, `app/chamados/services/materializacao.py:631`, `app/chamados/services/materializacao.py:633`, `app/chamados/services/materializacao.py:638`, `app/chamados/services/materializacao.py:674`, `app/chamados/services/materializacao.py:677`, `app/chamados/services/materializacao.py:678`, `app/chamados/services/materializacao.py:1099`, `app/chamados/services/materializacao.py:1378`, `app/chamados/services/materializacao.py:1425` |
| Emissão: filtros de via/materialização e resolução pelo chamado | `app/chamados/services/disputa_emissao.py:228`, `app/chamados/services/disputa_emissao.py:337`, `app/chamados/services/disputa_emissao.py:409`, `app/chamados/services/disputa_emissao.py:428`, `app/chamados/services/disputa_emissao.py:446`, `app/chamados/services/disputa_emissao.py:776`, `app/chamados/services/disputa_emissao.py:1084`, `app/chamados/services/disputa_emissao.py:1104`, `app/chamados/services/disputa_emissao.py:1340`, `app/chamados/services/disputa_emissao.py:1930`, `app/chamados/services/disputa_emissao.py:2062` |
| Validação; tarefa | `app/chamados/services/validacao.py:215`, `app/chamados/services/validacao.py:486`, `app/chamados/services/validacao.py:513`, `app/chamados/services/tarefa_do_dia.py:256` |
| Reconciliação: guardas, seleção e fotos | `app/chamados/reconciliador.py:367`, `app/chamados/reconciliador.py:630`, `app/chamados/reconciliador.py:1050`, `app/chamados/reconciliador.py:1144`, `app/chamados/reconciliador.py:1145`, `app/chamados/reconciliador.py:1146`, `app/chamados/reconciliador.py:1505` |
| Re-lavra: comparação de via, seleção e foto | `app/chamados/services/relavra_via.py:118`, `app/chamados/services/relavra_via.py:121`, `app/chamados/services/relavra_via.py:155`, `app/chamados/services/relavra_via.py:181`, `app/chamados/services/relavra_via.py:244` |
| Fio: identidade, duplicatas e seleção | `app/chamados/services/fio.py:1139`, `app/chamados/services/fio.py:1206`, `app/chamados/services/fio.py:1282`, `app/chamados/services/fio.py:1389`, `app/chamados/services/fio.py:1472`, `app/chamados/services/fio.py:1913`, `app/chamados/services/fio.py:1949`, `app/chamados/services/fio.py:1971`, `app/chamados/services/fio.py:2016` |
| Histórico; marcador do painel; marcos temporais da lavra | `app/chamados/services/historico.py:103`, `app/chamados/services/historico.py:105`, `app/chamados/services/painel.py:67`, `app/chamados/veredito_lavrado.py:203`, `app/chamados/veredito_lavrado.py:204` |
| Procedência da batida: autor e instante | `app/ponto/services/procedencia_batida.py:72`, `app/ponto/services/procedencia_batida.py:74` |
| Raio X; explicador; fábrica; passivo; reabertura de competência | `app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:182`, `app/chamados/services/explicador.py:95`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/fabrica_da_celula.py:171`, `app/chamados/services/passivo_validado.py:141`, `app/chamados/services/competencia_trancada.py:276` |
| View; cartório; flip; decisão do dia; apresentação da via | `app/chamados/views.py:1257`, `app/ponto/services/cartorio.py:1650`, `app/ponto/services/flip_auto.py:129`, `app/ponto/services/veredito_celula.py:196`, `app/templates/chamados/partials/_card_atendimento.html:192` |

Também existem consumidores indiretos além dos dois nomeados: `respostas_sem_veredito`, `pode_fechar`, reconciliação, fechamento por competência, fila por causa, ficha e desligamento (`app/chamados/models.py:1906`; `app/chamados/models.py:1934`; `app/chamados/reconciliador.py:1164`; `app/chamados/services/competencia_trancada.py:126`; `app/chamados/services/fila_por_causa.py:226`; `app/colaboradores/services/ficha.py:169`; `app/colaboradores/services/desligamento.py:165`).

**Item 3**

1. **Confere como registro do censo específico do C2.** A projeção parte dos campos atuais e apenas corta valores posteriores a `agora`; não reconstrói materialização apagada pelo descarimbo (`complemento2_respostas_f2_estados_1611.txt:4`; `respostas_registradas.md:14`; `app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1396`; `app/chamados/juizes.py:1400`; `app/chamados/models.py:3318`).

2. **L1 resolvido pelo corte é compatível com a escolha vigente.** Resolve pela conservação do campo atual, mantendo a perda do instante; não declara MOOT pela F1 nem cria ato histórico (`complemento2_respostas_f2_estados_1611.txt:3`; `complemento3_respostas_f2_estados_1611.txt:4`; `app/docs/RELATO.md:31`).

3. **O zero não significa ausência de leitores temporais.** `marcos_do_lote` filtra `materializada_em <= ate` e calcula seu máximo; tarefa e raio X fornecem horizonte. Isso lê o campo atual, sem demonstrar dependência de reconstruir um instante já apagado (`app/chamados/veredito_lavrado.py:163`; `app/chamados/veredito_lavrado.py:203`; `app/chamados/veredito_lavrado.py:204`; `app/chamados/services/tarefa_do_dia.py:434`; `app/chamados/services/raiox.py:38`). Afirmação universal sobre produção: [nao verificado].

4. **Não libera exportadas nem dispensa a prova.** A mudança de classificação de L1 conserva a exigência de igualdade e a parada diante de diferença em competência exportada (`complemento3_respostas_f2_estados_1611.txt:2`; `app/docs/CORTES.md:487`). Resultado efetivo da sombra e impacto financeiro: [nao verificado].

VEREDITO por item: 1 CONFERE (`app/chamados/models.py:1923`, `app/chamados/models.py:1629`); 2 CONFERE — selo extensível, sete não exaustivos (`complemento3_respostas_f2_estados_1611.txt:3`, `app/chamados/juizes.py:1390`); 3 CONFERE — zero no escopo registrado do C2 (`complemento2_respostas_f2_estados_1611.txt:4`, `respostas_registradas.md:14`).

## 16/11 21:39 · sessao 425552aa2e88 · AUSENCIA-EFEITO-PELO-ESTADO · verificador · origem=Code

**O C4 resolve as devoluções examinadas; (d) permite agrupar por `(tipo, status)`.** O juiz depende também do catálogo atual, mas não consulta a data da ausência para escolher esse catálogo. (`complemento4_ausencia_efeito_1611.txt:2-5`; `app/ponto/services/dia_decidido.py:453-481`; `app/ponto/catalogo/ausencias.py:267-295`.)

As referências aos anexos contam linhas desde `aval Ronald`, sem o cabeçalho do anexo.

**(a)**

1. **Premissas:** conferem. Os hrefs de status conservam efeito; os de efeito conservam status. A contagem facetada acompanha essa navegação. (`app/templates/ponto/ausencias.html:48-67`; `complemento3_ausencia_efeito_1611.txt:4`.)
2. **Colisão:** o C4 revoga expressamente o universo anterior dos chips e Total e restringe o 5a à requisição sem corte nas duas dimensões. Resolve o conflito apontado com o C1. (`complemento1_ausencia_efeito_1611.txt:5-6,10`; `complemento4_ausencia_efeito_1611.txt:2`.)
3. **Literal:** o exemplo fecha: em `status=rejeitada`, nenhum efeito conta a rejeitada; Todos remove o corte de status e conta os dois registros. A preservação de datas e registro continua dependendo do C2, mantido pelo C3. (`app/ponto/services/dia_decidido.py:461-473`; `app/templates/ponto/ausencias.html:48,65-67`; `complemento2_ausencia_efeito_1611.txt:2`; `complemento3_ausencia_efeito_1611.txt:4`; `complemento4_ausencia_efeito_1611.txt:2`.)
4. **Exportada/dinheiro/`!`:** muda o universo exibido dos contadores, dentro da autorização explícita; não exige gravação financeira. (`complemento4_ausencia_efeito_1611.txt:2`; `app/ponto/views.py:2197-2226,2254-2270`.)

**(b)**

1. **Premissas:** M-a corrigido é morto pelo RED 1: contar Suprime sobre `_base` cru inclui o aprovado que suprime, embora a origem esteja em Rejeitada. (`complemento3_ausencia_efeito_1611.txt:6`; `complemento4_ausencia_efeito_1611.txt:3`.)
2. **Colisão:** substitui nominalmente M-a, acrescenta M-c e conserva M-b; não mantém a associação errada anterior. (`complemento3_ausencia_efeito_1611.txt:7`; `complemento4_ausencia_efeito_1611.txt:3`.)
3. **Literal:** M-c também fecha: com efeito Suprime, a rejeitada não pertence ao universo do contador Rejeitada; o mutante sobre a base crua conta 1. São contraexemplos lógicos, não ocorrências produtivas demonstradas. (`app/ponto/services/dia_decidido.py:461-473`; `complemento4_ausencia_efeito_1611.txt:3`.)
4. **Exportada/dinheiro/`!`:** são condições de verificação dos contadores, sem alteração declarada dos registros. (`complemento4_ausencia_efeito_1611.txt:3`.)

**(c)**

1. **Premissas:** `pendentes` hoje conta `_A4_ABERTAS`, importado de `ABERTAS`: as duas filas abertas. (`app/ponto/views.py:79,2197`; `app/ponto/services/ausencia.py:912`.)
2. **Colisão:** a ordem específica do C4 substitui a proibição anterior de tocar essa contagem. (`complemento3_ausencia_efeito_1611.txt:9`; `complemento4_ausencia_efeito_1611.txt:4`.)
3. **Literal:** preserva o valor se `por_status(..., 'pendente')` usar a união dessas duas filas. É necessário definir `_base` antes da chamada: atualmente sua atribuição vem depois de `pendentes`. (`complemento3_ausencia_efeito_1611.txt:4`; `app/ponto/views.py:2197-2201,2212-2219`.)
4. **Exportada/dinheiro/`!`:** continua sendo contagem entregue ao contexto, sem gravação. (`app/ponto/views.py:2197,2256`; `complemento4_ausencia_efeito_1611.txt:4`.)

**(d)**

1. **Premissas:** `veredito_de` lê somente `ausencia.tipo` e `ausencia.status`. Rejeitada retorna imediatamente; os demais casos consultam `CAT.efeito_vigente(tipo)`. Portanto, **não é função exclusivamente do par**, pois existe a dependência externa do catálogo. (`app/ponto/services/dia_decidido.py:453-481`.)
2. **Colisão:** a consulta agrupada substitui expressamente as sete contagens, esclarecendo o orçamento anterior; `pendentes` conserva sua contagem separada conforme (c). (`app/ponto/views.py:2197-2211`; `complemento4_ausencia_efeito_1611.txt:4-5`.)
3. **Literal:** **dá para agrupar por `(tipo, status)`** e classificar cada grupo pelo juiz. O catálogo é o atual: lê tipos ativos, com cache de 60 segundos e semente quando a leitura não fornece cadastro. Não há seleção histórica por data do lançamento. Datas delimitam `_base`, não o veredito. Isso não torna a view inteira uma única consulta. (`app/ponto/catalogo/ausencias.py:240-295`; `app/ponto/services/dia_decidido.py:468-481`; `app/ponto/views.py:2186-2196,2231-2258`.)
4. **Exportada/dinheiro/`!`:** agrupar e consultar o juiz não exige alterar catálogo, status ou projeção financeira; a gravação e o despertar da projeção pertencem à transição de estado. (`app/ponto/catalogo/ausencias.py:240-295`; `app/ponto/services/ausencia.py:976-994`.)

O `ANEXO!` permanece um predicado separado; nenhuma dessas letras exige modificá-lo. (`app/templates/ponto/ausencias.html:158`; `app/ponto/services/ausencia.py:745-759`.) Implementação futura, REDs executados, mutantes mortos, total SQL e comportamento produtivo: **[não verificado]**.

VEREDITO por letra/item: (a) CONFERE (`complemento4_ausencia_efeito_1611.txt:2`); (b) CONFERE (`complemento4_ausencia_efeito_1611.txt:3`); (c) CONFERE (`app/ponto/views.py:2197-2201,2212-2219`); (d) CONFERE (`app/ponto/services/dia_decidido.py:453-481`; `app/ponto/catalogo/ausencias.py:267-295`).

## 16/11 21:39 · sessao 201f536c64d6 · - · - · origem=Ronald (tmux)

**Item 2: CONFERE como compromisso de conservação extensível; os sete não constituem censo completo do trio.** O próprio C3 manda incluir qualquer leitor adicional no selo; portanto encontrar outro não contradiz sua condição, mas impede registrar “universo fechado = 7” (`complemento3_respostas_f2_estados_1611.txt:3`). A abrangência exata do brief/matriz F2, ausentes nesta cópia: [nao verificado].

`respondida` e `resposta_display` são observáveis da F2 citados no aval, mas não leem o trio: leem `respondida_em` e `resposta_colab`, respectivamente (`app/chamados/models.py:3665`; `app/chamados/models.py:3688`). `supra_juiz` e `irmas_respondidas_sem_consumo` são consumidores indiretos da lista (`app/ponto/supra_juiz.py:573`; `app/chamados/juizes.py:2105`).

**Leitores adicionais no mesmo arquivo de modelos**, além dos sete nomeados:

| Função/unidade | Leitura |
|---|---|
| `_universo_da_ressuscitacao` | Filtra instante e via (`app/chamados/models.py:1531`; `app/chamados/models.py:1535`). |
| `fechar` | Guarda via anterior e usa instante no SILENCIO (`app/chamados/models.py:2001`; `app/chamados/models.py:2006`). |
| `remover_motivo` | Filtra via e a registra no motivo da retratação (`app/chamados/models.py:2136`; `app/chamados/models.py:2141`). |
| `do_fato`; `q_dos_fatos` | Excluem vias fora do fato (`app/chamados/models.py:2829`; `app/chamados/models.py:2860`). |
| `fechar_por_admin` | Guarda de idempotência por via e instante (`app/chamados/models.py:3165`; `app/chamados/models.py:3166`). |
| `carimbar`; `carimbar_identidade` | Leem estado anterior do trio / preservam instante e autor (`app/chamados/models.py:3236`; `app/chamados/models.py:3292`; `app/chamados/models.py:3293`). |
| `fora_do_fato` | Classifica pela via (`app/chamados/models.py:3311`). |
| `descarimbar_em_lote` | Lê via no filtro antes de apagar o trio (`app/chamados/models.py:3392`; `app/chamados/models.py:3393`). |
| `encerrar_por_fato_no_espelho`; `encerrar_fantasma` | Guardas pela via / via com instante (`app/chamados/models.py:3400`; `app/chamados/models.py:3411`). |
| `reabrir_fantasma`; `reabrir_por_resposta`; `reabrir_por_competencia` | Guardas por instante/via (`app/chamados/models.py:3423`; `app/chamados/models.py:3440`; `app/chamados/models.py:3447`). |
| Índices de unicidade | Condições dependem da via (`app/chamados/models.py:3652`; `app/chamados/models.py:3658`). |

**Outros consumidores produtivos do trio na cópia** — atribuí-los automaticamente à F2 seria [nao verificado]:

| Arquivo/funções | Evidência |
|---|---|
| Juízes: `_retratada_com_trilha`, `_pergunta_materializada_de_fato`, `_chave_memo_pv`, `projecao_temporal_da_pergunta`, `pergunta_sem_lastro_ou_viva` | `app/chamados/juizes.py:292`; `:683`; `:711`; `:857`; `:1390`; `:1502`. |
| Materialização: estado de aplicação, seleção de pendentes, agrupamento de absorvidas, foto do trio, guarda de recusa | `app/chamados/services/materializacao.py:440`; `:539`; `:631`; `:633`; `:638`; `:674`; `:677`; `:678`; `:1099`; `:1378`; `:1425`. |
| Emissão: derivação do destino, geração de perguntas, órfãs/âncoras e resolução pelo chamado | `app/chamados/services/disputa_emissao.py:228`; `:337`; `:409`; `:428`; `:446`; `:776`; `:1084`; `:1104`; `:1340`; `:1930`; `:2062`. |
| Validação | `app/chamados/services/validacao.py:215`; `:486`; `:513`. |
| Tarefa | `app/chamados/services/tarefa_do_dia.py:256`; `:257`. |
| Reconciliação: guardas, universos e fotos | `app/chamados/reconciliador.py:367`; `:630`; `:1050`; `:1144`; `:1146`; `:1505`. |
| Re-lavra: filtros e foto anterior | `app/chamados/services/relavra_via.py:118`; `:121`; `:123`; `:155`; `:181`; `:244`. |
| Fio: identidade, duplicatas e grupos | `app/chamados/services/fio.py:1139`; `:1206`; `:1282`; `:1389`; `:1472`; `:1913`; `:1949`; `:1971`; `:2016`. |
| Procedência: **autor e instante**, além da via nos demais leitores | `app/ponto/services/procedencia_batida.py:72`; `:74`. |
| Histórico; painel; marcos do lote | `app/chamados/services/historico.py:103`; `:105`; `app/chamados/services/painel.py:67`; `app/chamados/veredito_lavrado.py:203`. |
| Raio X; explicador; fábrica; passivo; reabertura de competência | `app/chamados/services/raiox.py:135`; `:182`; `app/chamados/services/explicador.py:95`; `app/chamados/services/fabrica_da_celula.py:149`; `:171`; `app/chamados/services/passivo_validado.py:141`; `app/chamados/services/competencia_trancada.py:276`. |
| View; cartório; flip; decisão do dia; template | `app/chamados/views.py:1257`; `app/ponto/services/cartorio.py:1650`; `app/ponto/services/flip_auto.py:129`; `app/ponto/services/veredito_celula.py:196`; `app/templates/chamados/partials/_card_atendimento.html:192`. |

O trio também aparece em manutenção, testes e migrações; esses usos não devem ser contados automaticamente como leitores de pouso da F2 [nao verificado]. Não confundir com `RegularizacaoExtraordinaria`, outro modelo que possui campos homônimos (`app/chamados/models.py:4152`; `app/chamados/models.py:4153`).

## 16/11 21:38 · sessao c7d251d56727 · - · - · origem=Ronald (tmux)

**Item 3: CONFERE no sentido registrado pelo C2; a afirmação universal sobre produção permanece [nao verificado].** O C2 pede o predicado composto: leitor da F2 que chama projeção com `agora` anterior a um descarimbo **e depende do instante apagado** (`complemento2_respostas_f2_estados_1611.txt:4`). O registro fornecido mantém esse censo como zero **no código** (`respostas_registradas.md:15`).

1. **Premissas:** a projeção recebe os campos atuais e apenas elimina valores posteriores ao horizonte; não busca trilha nem recompõe instante apagado (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1391`; `app/chamados/juizes.py:1396`; `app/chamados/juizes.py:1400`). Descarimbar escreve `None` no instante (`app/chamados/models.py:3318`). Os três chamadores diretos produtivos são `_slot_esperado`, que usa resposta, `_pergunta_viva_calc`, que usa materialização/via, e `beco_em_lote`, que usa somente o instante de resposta (`app/chamados/juizes.py:351`; `app/chamados/juizes.py:352`; `app/chamados/juizes.py:1404`; `app/chamados/juizes.py:1416`; `app/chamados/juizes.py:1444`; `app/chamados/juizes.py:1482`; `app/escala/services/cadastro_realidade.py:502`). Não encontrei nesses corpos a dependência histórica composta pedida pelo C2; ocorrência real em produção: [nao verificado].

2. **Colisão:** resolver L1 pela conservação do comportamento vigente é compatível com o cabeçalho do corte e com a manutenção de descarimbo apagando o instante; é resolução pelo corte, não demonstração de MOOT pela F1 (`app/docs/CORTES.md:487`; `complemento1_respostas_f2_estados_1611.txt:9`; `complemento2_respostas_f2_estados_1611.txt:3`). A cópia ainda registra a medição anterior como RESIDUAL; isso é o estado pendente que a resposta pretende resolver (`app/docs/RELATO.md:138`).

3. **Literal/limite:** “nenhum leitor depende do instante apagado” deve conservar o significado restrito do censo registrado, sem virar afirmação de ausência de leitores temporais (`respostas_registradas.md:15`). A tarefa passa `agora` ao C6 (`app/chamados/services/tarefa_do_dia.py:537`; `app/chamados/services/tarefa_do_dia.py:541`). Há também leitor temporal fora dos sete: **`marcos_do_lote`** filtra `materializada_em <= ate` e calcula seu máximo; `esta_stale` usa esse máximo para decidir nova lavra (`app/chamados/veredito_lavrado.py:151`; `app/chamados/veredito_lavrado.py:163`; `app/chamados/veredito_lavrado.py:202`; `app/chamados/veredito_lavrado.py:204`; `app/chamados/veredito_lavrado.py:230`; `app/chamados/veredito_lavrado.py:232`). A tarefa e o raio X fornecem horizonte a esse leitor (`app/chamados/services/tarefa_do_dia.py:434`; `app/chamados/services/raiox.py:38`). Isso é dependência do campo atual, não prova de exigência de recuperar instante já apagado (`app/chamados/veredito_lavrado.py:202`; `app/chamados/veredito_lavrado.py:204`).

4. **Exportada/dinheiro/!:** o item não revoga a proibição: qualquer diferença de `supra_juiz` em competência exportada exige `PAREI !` com tabela (`app/docs/CORTES.md:487`; `complemento2_respostas_f2_estados_1611.txt:5`). Igualdade efetiva na sombra, impacto financeiro e produção: [nao verificado].

**Recomendação para consolidar:** item 3 CONFERE como registro do censo de código delimitado no C2; não promova “zero” a prova de produção nem a inexistência de outros leitores temporais (`respostas_registradas.md:15`; `app/chamados/veredito_lavrado.py:202`).

## 16/11 21:38 · sessao e6b94b016dcf · - · - · origem=Ronald (tmux)

Item 1 — **CONFERE**.

1. **Premissas:** o caso positivo satisfaz o predicado: sem validação, sem materialização, instante ou texto de resposta preservado e tranca ausente/ignorada (`app/chamados/models.py:1922-1926`). O descarimbo efetivo apaga materialização e via (`app/chamados/models.py:3318`). Ressuscitar também apaga instante e texto de resposta; portanto, a pergunta não entra nessa lista (`app/chamados/models.py:1629-1630`; `app/chamados/models.py:1924-1925`).

2. **Compatibilidade:** o C3 adota exatamente a formulação proposta na devolução já registrada (`app/docs/RELATO.md:4`). Conservação lendo o campo atual corresponde à alternativa explicitamente oferecida anteriormente (`app/docs/RELATO.md:31`) e ao cabeçalho de conservação do corte (`app/docs/CORTES.md:487`).

3. **Literal:** fecha tomando “descarimbada” como descarimbo efetivo e mantendo a pergunta no universo passado ao leitor (`app/chamados/models.py:1922`). Para montar o caso que morde, a via não pode ser terminal: o descarimbo individual recusa `fora_do_fato`, e o lote exclui essas vias (`app/chamados/models.py:3316-3318`; `app/chamados/models.py:3391-3393`). Isso é requisito de preparação do caso, não contradição da afirmação sobre a pergunta já descarimbada (`app/chamados/models.py:1922-1926`).

4. **Exportada/dinheiro/!:** há alcance já declarado no CSF: os dois universos passam pela lista, cuja ausência permite executar o CSF (`app/ponto/supra_juiz.py:573-574`; `app/ponto/supra_juiz.py:588`). Continua valendo `PAREI !` diante de diferença em competência exportada (`app/docs/CORTES.md:487`). Execução do selo ou efeito efetivo em produção: [nao verificado].

VEREDITO por item: item 1 CONFERE (`app/chamados/models.py:1922-1926`; `app/chamados/models.py:1629-1630`).

## 16/11 21:21 · sessao 83911a7e8be8 · AUSENCIA-EFEITO-PELO-ESTADO · verificador · origem=Code

1. **PROCEDENTE: MUDA c está cumprido.** `_pares_por_veredito` agrupa por `(tipo,status)`, consulta o juiz e devolve os baldes; a lista chama essa função uma vez, e contador e filtro leem o mesmo retorno. O filtro monta o OR com ambos os campos. ([views.py:2175](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2175), [views.py:2240](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2240), [views.py:2250](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2250), [views.py:2264](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2264))

   **NAO PROCEDENTE a suspeita de juiz novo ou significado trocado.** A função não decide por status/efeito: usa o retorno de `veredito_do_tipo`. Lê `tipo`, `status` e a contagem `n`; modifica somente o dicionário local, sem gravar registros. O próprio `veredito_de` delega ao mesmo núcleo com os mesmos campos. ([views.py:2174](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2174), [dia_decidido.py:495](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/services/dia_decidido.py:495))

2. **PROCEDENTE: o selo morde, com alcance limitado.** Na reprodução isolada do corpo do teste, com fontes mutadas somente em memória: [nao verificado]

   - Remover ou duplicar `_pares_por_veredito(_base)` falha: exige exatamente uma chamada. ([teste:298](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/tests/test_ausencia_efeito_pelo_estado.py:298))
   - Introduzir `abonam_vigentes` no helper falha: o varredor reconhece esse nome proibido. ([teste:258](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/tests/test_ausencia_efeito_pelo_estado.py:258), [teste:301](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/tests/test_ausencia_efeito_pelo_estado.py:301))
   - Trocar a chave de `setdefault` por `'abonado'`, ignorando o juiz, **passa nesse selo**; substituir a soma do contador por `0` também passa. Ele não verifica delegação ao juiz nem o consumo do retorno pelos dois caminhos. ([views.py:2176](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2176), [views.py:2250](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2250), [teste:295](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/tests/test_ausencia_efeito_pelo_estado.py:295))

   Portanto, **NAO PROCEDENTE tratá-lo como prova completa de MUDA c**; ele protege a estrutura nominal descrita acima. ([teste:295](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/tests/test_ausencia_efeito_pelo_estado.py:295))

3. **NAO PROCEDENTE: retirar o segundo `unescape` não esvazia os asserts nem deixa escapar M5.** O parser entrega o atributo desescapado, e `_segue` agora usa esse valor diretamente. O teste com datas válidas exige `de` e `ate` em todos os sete chips, segue os links e exclui o registro de fora do recorte; um link sem esses parâmetros falha já nas verificações de presença. ([teste:42](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/tests/test_ausencia_efeito_pelo_estado.py:42), [teste:122](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/tests/test_ausencia_efeito_pelo_estado.py:122), [teste:331](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/tests/test_ausencia_efeito_pelo_estado.py:331))

   **PROCEDENTE uma limitação anterior:** `parse_qs` sem `keep_blank_values=True` permite que `de=&ate=` passe pelos asserts de ausência. Isso não foi criado pela retirada do `unescape` e não neutraliza o teste de datas válidas contra M5. ([teste:358](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/tests/test_ausencia_efeito_pelo_estado.py:358), [teste:336](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/tests/test_ausencia_efeito_pelo_estado.py:336))

4. Quanto às seis linhas da triagem:

   - **4.1 — PROCEDENTE a cura:** há helper compartilhado e ambos os consumidores usam seus baldes; a ressalva sobre o alcance do selo permanece no item 2. ([views.py:2240](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2240), [views.py:2250](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2250), [views.py:2264](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2264))
   - **4.2 — PROCEDENTE como questão preservada no delta; não como risco eliminado.** Agrupamento e coluna continuam chamando o juiz separadamente, e o catálogo tem cache de 60 segundos, sem fotografia por requisição. A coluna efetivamente lê `veredito_de(a)`, conforme o aval. ([views.py:2176](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2176), [views.py:2297](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2297), [ausencias.py:258](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/catalogo/ausencias.py:258), [ausencias.py:276](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/catalogo/ausencias.py:276)) A equivalência com o HEAD histórico e a alegação de que qualquer solução exigiria fallback: **NAO SEI [nao verificado]**.
   - **4.3 — PROCEDENTE quanto ao comportamento atual:** `registro` substitui a consulta por busca de PK e pode trazer registro fora do mês. A janela também aceita ausências que atravessam sua borda. ([views.py:2206](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2206), [turnos.py:3170](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/turnos.py:3170)) A alegada concordância da volta 1: **NAO SEI [nao verificado]**.
   - **4.4 — NAO SEI quanto ao descarte formal por C3 [nao verificado].** O problema técnico continua: os chips preservam a outra dimensão, enquanto os contadores antecedem ambos os filtros; a própria cópia documenta esse contraexemplo. ([ausencias.html:65](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/templates/ponto/ausencias.html:65), [views.py:2243](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2243), [RELATO.md:4](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/docs/RELATO.md:4))
   - **4.5 — NAO SEI:** os resultados de execução de RED e mutantes não foram fornecidos; afirmar que rodaram não comprova seus resultados. **[nao verificado]**
   - **4.6 — PROCEDENTE a correção atual:** o parser desescapa o atributo e `_segue` não repete a operação. A atribuição histórica ao commit `2b2b0256e`: **[nao verificado]**. ([teste:42](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/tests/test_ausencia_efeito_pelo_estado.py:42), [teste:117](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/tests/test_ausencia_efeito_pelo_estado.py:117))

   **NAO SEI se houve achado omitido:** o relatório integral da volta 1 não consta dos anexos fornecidos. **[nao verificado]**

5. **NAO PROCEDENTE a hipótese de nova quebra funcional identificável no delta.** A extração conserva agrupamento/delegação, contador e filtro compartilham os baldes, a coluna continua lendo o juiz e os links continuam levando o universo montado na view. ([views.py:2175](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2175), [views.py:2250](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2250), [views.py:2264](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2264), [views.py:2297](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2297), [views.py:2219](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/ponto/views.py:2219), [ausencias.html:48](/home/ronald/codex_ws/b33acd6034b19a2f39c6ef7dc6f8b31893cd070e/app/templates/ponto/ausencias.html:48))

   **NAO SEI quanto à comprovação executada dos resultados 1–6 e C2 item 1:** a inspeção e o selo isolado não substituem os testes Django nem o smoke de produção. **[nao verificado]** [nao verificado]

## 16/11 21:13 · sessao e5d084d5b3df · AUSENCIA-EFEITO-PELO-ESTADO · verificador · origem=Code

**Parecer: cumpre em parte.** É **PROCEDENTE** a possibilidade de divergência entre chip/filtro e coluna: os primeiros guardam o julgamento dos pares; a coluna consulta novamente o catálogo, que pode ter mudado após expiração/invalidação do cache. Também falta a função comum exigida literalmente em MUDA c, embora exista estrutura compartilhada (`app/ponto/views.py:2230`, `app/ponto/views.py:2240`, `app/ponto/views.py:2254`, `app/ponto/views.py:2287`; `app/ponto/catalogo/ausencias.py:275`).

Executei em memória os quatro selos textuais, com resultado 4/4; isso verifica os selos, sem executar a integração Django (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:275`). RED histórico, execução dos mutantes, suíte inteira, deploy, smoke e estado atual de produção: **NAO SEI [nao verificado]**. Identidade Git do commit/base e colisões com obras em voo: **[nao verificado]**.

**1. Checklist do aval**

Nas linhas abaixo, “cumpre” descreve a implementação encontrada; execução dos testes de integração permanece **[nao verificado]**. [nao verificado]

| Item | Conclusão e localização |
|---|---|
| MUDA a | **PROCEDENTE — cumpre.** O laço atribui `veredito_de(a)` e apresentação; o template lê esses atributos, usa `hx-muted` para rejeitada e coloca o veredito em `data-value` (`app/ponto/views.py:2287`, `app/ponto/views.py:2288`; `app/templates/ponto/ausencias.html:158`). |
| MUDA b | **PROCEDENTE — cumpre.** Há uma consulta agregada por `tipo,status`, julgamento por par e soma dos mesmos baldes, sem conjuntos de tipos para esses contadores (`app/ponto/views.py:2229`, `app/ponto/views.py:2230`, `app/ponto/views.py:2240`). |
| MUDA c | **PROCEDENTE — cumpre em parte.** O `Q` usa exatamente os pares classificados que alimentam os chips; porém não existe a função comum montadora exigida literalmente: classificação, soma e montagem estão no corpo da view (`app/ponto/views.py:2228`, `app/ponto/views.py:2240`, `app/ponto/views.py:2254`). |
| MUDA d | **PROCEDENTE — cumpre.** Só rejeitada vence o efeito no juiz; os dois estados abertos continuam classificados pelo tipo. O teste combina Abona com Pendente (`app/ponto/services/dia_decidido.py:475`, `app/ponto/services/dia_decidido.py:482`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:161`). |
| MUDA e | **PROCEDENTE — cumpre no diff fornecido.** A property continua consultando apenas o efeito do tipo; ANEXO! continua chamando `contradiz_documento` (`app/ponto/models.py:850`, `app/ponto/models.py:867`; `app/templates/ponto/ausencias.html:158`). |
| RESULTADO 1 | **PROCEDENTE — implementado e com teste específico.** Rejeitada recebe `SEM EFEITO`; o teste exige texto, ajuda, classe e `data-value` (`app/ponto/services/dia_decidido.py:84`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:131`). |
| RESULTADO 2 | **PROCEDENTE — implementado e com teste específico.** Rejeitada fica em outro balde; o teste fixa Abona em quatro, excluindo a quinta linha daquele tipo (`app/ponto/views.py:2230`, `app/ponto/views.py:2240`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:142`). |
| RESULTADO 3 | **PROCEDENTE — implementado e com teste específico.** Abona filtra pares abonados; status rejeitada continua independente. O teste verifica exclusão e inclusão da mesma rejeitada (`app/ponto/views.py:2248`, `app/ponto/views.py:2254`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:150`). |
| RESULTADO 4 | **PROCEDENTE — implementado e com teste específico.** Cobre os dois estados abertos, coluna ABONA, contador e lista Abona+Pendente (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:161`). |
| RED | **NAO SEI — comprovação incompleta.** Existem os três casos capazes de denunciar o comportamento antigo; FAIL executado antes da cura não foi demonstrado pelo material disponível (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:131`, `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:142`, `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:150`; execução histórica **[nao verificado]**). |
| Selo original | **PROCEDENTE — cumpre.** Proíbe `a.efeito`, procura os três nomes proibidos dentro da função e exige encontrar o sujeito do selo (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:255`, `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:276`, `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:286`). |
| M1 | **PROCEDENTE quanto à mordida; morte executada NAO SEI.** Voltar a `a.efeito` viola o selo e o resultado da rejeitada (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:131`, `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:276`; execução **[nao verificado]**). |
| M2 | **PROCEDENTE quanto à mordida; morte executada NAO SEI.** Contar pelo tipo inclui a rejeitada e contraria Abona=4; os nomes antigos também são detectados (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:145`, `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:257`; execução **[nao verificado]**). |
| M3 | **PROCEDENTE quanto à mordida; morte executada NAO SEI.** Filtrar pelo tipo reincorpora a rejeitada que o teste exige excluir de uma lista explicitamente não vazia (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:153`; execução **[nao verificado]**). |
| PROIBIDO do aval-mãe | **NAO PROCEDENTE para violação no diff fornecido.** A regra continua no juiz; a view apenas o consulta, o mapa é apresentação, os status conservam seus badges e os três sítios foram alterados juntos (`app/ponto/services/dia_decidido.py:80`, `app/ponto/services/dia_decidido.py:467`; `app/ponto/views.py:2230`, `app/ponto/views.py:2254`, `app/ponto/views.py:2287`; `app/templates/ponto/ausencias.html:177`). Alteração efetiva de dados de produção: **[nao verificado]**. |
| C1 — MAPA | **PROCEDENTE — cumpre.** Um dicionário junto de CORES/PALAVRA, com as quatro linhas e valores pedidos (`app/ponto/services/dia_decidido.py:45`, `app/ponto/services/dia_decidido.py:67`, `app/ponto/services/dia_decidido.py:80`). |
| C1 — item a | **PROCEDENTE — cumpre.** Valor, texto e ajuda vêm do veredito/mapa; classes ficam no template, ANEXO! separado e desaparece o ramo `?` (`app/ponto/views.py:2287`; `app/templates/ponto/ausencias.html:158`). |
| C1 — universo | **PROCEDENTE — cumpre.** `_base` é estabelecido depois de vigência/registro, colaborador e tipo, antes dos filtros de status/efeito (`app/ponto/views.py:2190`, `app/ponto/views.py:2193`, `app/ponto/views.py:2195`, `app/ponto/views.py:2220`, `app/ponto/views.py:2241`). |
| C1 — RESULTADO 5a | **PROCEDENTE sob dados estáveis — cumpre.** Todo par recebe um dos quatro vereditos; o teste exige total 13 e verifica a partição no contexto e HTML (`app/ponto/services/dia_decidido.py:475`, `app/ponto/services/dia_decidido.py:487`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:175`). |
| C1 — RESULTADO 5b | **PROCEDENTE sob catálogo/dados estáveis — cumpre.** Contador e filtro compartilham os pares; o teste exige baldes positivos e segue os links sem status (`app/ponto/views.py:2240`, `app/ponto/views.py:2254`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:184`). A ressalva temporal está no item 3(b). |
| C1 — RESULTADO 5c | **PROCEDENTE sob dados estáveis — cumpre.** Contagem e filtro usam rejeitada; o teste segue o chip e exige três linhas (`app/ponto/views.py:2238`, `app/ponto/views.py:2248`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:193`). |
| C1 — RESULTADO 6 | **PROCEDENTE na matriz escrita — cumpre.** Compara `data-value`, texto e ajuda, exige 13 linhas, 12 combinações tipo/status e os quatro vereditos (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:213`). Não cobre catálogo mudando durante a requisição (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:104`, `app/ponto/views.py:2287`). |
| C1 — pedido→veredito | **PROCEDENTE — cumpre.** A inversão é derivada do campo `filtro` do próprio mapa; desconhecido não entra no filtro (`app/ponto/views.py:2232`, `app/ponto/views.py:2250`). |
| C1 — selo de chaves | **PROCEDENTE — cumpre.** Exige exatamente os quatro vereditos, as três chaves de apresentação e os três filtros (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:237`). |
| C1 — M4 | **PROCEDENTE quanto à mordida; morte executada NAO SEI.** Além de proibir literais na célula, troca texto/ajuda por sentinelas e exige essas sentinelas no HTML (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:279`, `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:301`; execução **[nao verificado]**). |
| C1 — PROIBIDO | **NAO PROCEDENTE para violação no diff fornecido.** Universo preservado, status preservados e inversão derivada do mapa único; não aparece nova regra CSS no patch fornecido (`app/ponto/views.py:2220`, `app/ponto/views.py:2232`; `app/templates/ponto/ausencias.html:158`, `app/templates/ponto/ausencias.html:177`). |
| C2 — somente item 1 | **PROCEDENTE — cumpre para os parâmetros efetivos do universo.** Uma query string é montada da leitura já feita e usada nos sete links; datas inválidas são omitidas e datas válidas normalizadas (`app/ponto/views.py:2186`, `app/ponto/views.py:2204`, `app/ponto/views.py:2305`; `app/templates/ponto/ausencias.html:48`, `app/templates/ponto/ausencias.html:65`). |
| C2 — somente item 3/M5 | **PROCEDENTE quanto à mordida; morte executada NAO SEI.** Exige `de/ate` nos sete links e segue cinco deles com uma linha sentinela fora do recorte (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:318`; execução **[nao verificado]**). |

**Algo não pedido: NAO PROCEDENTE no diff fornecido.** Não identifiquei fatia posterior, C3, alteração de texto/cor de status, CSS novo ou escrita produtiva de dados; as novas atribuições produtivas são contexto e atributos transitórios da linha, e as criações novas estão na fixture de testes (`app/ponto/views.py:2287`, `app/ponto/views.py:2301`; `app/templates/ponto/ausencias.html:177`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:80`). O cruzamento entre filtros continua declarado como problema anterior, sem ser “curado” por este diff (`app/docs/RELATO.md:4`; `app/templates/ponto/ausencias.html:60`, `app/templates/ponto/ausencias.html:65`).

**2. Quem escreve e quem lê**

| Campo/valor | Escritores e leitores encontrados |
|---|---|
| `Ausencia.tipo` | **PROCEDENTE — campo persistido.** Definido no modelo; criação pela porta e edição escrevem o valor. A lista o lê no agrupamento, filtro e juiz; fora dela, API, cobertura e export também o leem (`app/ponto/models.py:749`; `app/ponto/services/ausencia.py:484`, `app/ponto/services/ausencia.py:648`; `app/ponto/views.py:2229`, `app/ponto/views.py:2255`; `app/ponto/services/dia_decidido.py:495`; `app/api/views.py:1865`; `app/ponto/turnos.py:2773`; `app/folha/export.py:87`). |
| `Ausencia.status` | **PROCEDENTE — campo persistido com quatro estados.** Nasce pela porta; `mudar_estado` escreve transições, chamado por `decidir`. Existe a escrita especial preexistente de rejeição das férias. A lista lê status para contar, filtrar e julgar; API, cobertura e export também leem (`app/ponto/models.py:701`, `app/ponto/models.py:809`; `app/ponto/services/ausencia.py:486`, `app/ponto/services/ausencia.py:976`, `app/ponto/services/ausencia.py:1088`, `app/ponto/services/ausencia.py:1496`; `app/ponto/views.py:2229`, `app/ponto/views.py:2238`, `app/ponto/views.py:2248`; `app/api/views.py:1867`; `app/ponto/turnos.py:2529`; `app/folha/export.py:87`). |
| `Ausencia.efeito` | **PROCEDENTE — property calculada, sem setter.** Lê `efeito_vigente(tipo)`, sem status. A lista deixa de consumi-la; continuam os leitores em turnos, fechamento, cálculo de efeitos e calendário (`app/ponto/models.py:850`; `app/templates/ponto/ausencias.html:158`; `app/ponto/turnos.py:2704`; `app/ponto/services/fechamento.py:697`; `app/ponto/services/efeito_ausencia.py:58`; `app/colaboradores/services/calendario.py:705`). |
| `contradiz_documento` | **PROCEDENTE — property calculada.** Delega a `tipo_contradiz_documento`, que lê documento e efeito do tipo, sem status. Leitores: ANEXO!, gate de aprovação, título do chamado e testes externos (`app/ponto/models.py:867`; `app/ponto/services/ausencia.py:754`, `app/ponto/services/ausencia.py:757`, `app/ponto/services/ausencia.py:765`, `app/ponto/services/ausencia.py:802`; `app/templates/ponto/ausencias.html:158`; `app/ponto/tests/test_gate_anexo_desconta.py:28`). |
| Linha `.veredito` / `.apresentacao` | **PROCEDENTE — escritos em memória pela view e lidos pela célula.** Não há gravação desses atributos pelo trecho alterado (`app/ponto/views.py:2287`, `app/ponto/views.py:2288`; `app/templates/ponto/ausencias.html:158`). |
| `cnt` | **PROCEDENTE — produzido pela view antes de status/efeito.** O template lê os três baldes; os testes leem contexto e números renderizados (`app/ponto/views.py:2233`, `app/ponto/views.py:2240`, `app/ponto/views.py:2301`; `app/templates/ponto/ausencias.html:65`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:145`, `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:179`). |
| `count_*` | **PROCEDENTE — aliases dos contadores da view.** Consumidos pelos quatro chips de status e pelos testes (`app/ponto/views.py:2302`; `app/templates/ponto/ausencias.html:50`, `app/templates/ponto/ausencias.html:54`, `app/templates/ponto/ausencias.html:58`, `app/templates/ponto/ausencias.html:62`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:179`). |
| `filtro_efeito` | **PROCEDENTE — recebe o GET bruto.** A view só aplica efeito reconhecido; o template conserva o valor nos links de status e marca chips conhecidos como ativos (`app/ponto/views.py:2249`, `app/ponto/views.py:2250`, `app/ponto/views.py:2301`; `app/templates/ponto/ausencias.html:48`, `app/templates/ponto/ausencias.html:65`). |
| `filtro_status` | **PROCEDENTE — recebe o GET bruto.** A view reconhece a caixa Pendente ou os quatro estados; o template usa em links, seleção e hidden do formulário (`app/ponto/views.py:2175`, `app/ponto/views.py:2241`, `app/ponto/views.py:2246`, `app/ponto/views.py:2303`; `app/templates/ponto/ausencias.html:65`, `app/templates/ponto/ausencias.html:75`). |
| `q_universo` | **PROCEDENTE — escrito por `urlencode` na view.** Os sete chips são seus leitores produtivos; os testes examinam os hrefs resultantes (`app/ponto/views.py:2204`, `app/ponto/views.py:2305`; `app/templates/ponto/ausencias.html:48`, `app/templates/ponto/ausencias.html:67`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:322`). |
| Contexto `de/ate` | **PROCEDENTE — limites efetivos.** A view combina pedido válido com padrão mensal; cobertura e inputs leem essas datas. A query dos chips usa separadamente `_de_pedido/_ate_pedido`, que representam somente datas válidas explicitamente recebidas (`app/ponto/views.py:2186`, `app/ponto/views.py:2187`, `app/ponto/views.py:2190`, `app/ponto/views.py:2205`, `app/ponto/views.py:2304`; `app/templates/ponto/ausencias.html:72`). |
| `APRESENTACAO_AUSENCIA` | **PROCEDENTE — declarado uma vez.** A view lê para inverter filtro→veredito e apresentar a linha; testes conferem as chaves e alteram temporariamente texto/ajuda por mock (`app/ponto/services/dia_decidido.py:80`; `app/ponto/views.py:2232`, `app/ponto/views.py:2288`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:239`, `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:307`). |

Os escritores externos identificados chamam as portas: aprovação/rejeição/criação pela view, criação pelo Resolver dia e copiloto, e rejulgamento pelo command; o admin de Ausencia é somente leitura (`app/ponto/views.py:2338`, `app/ponto/views.py:2414`, `app/ponto/views.py:2508`; `app/ponto/services/veredito_celula.py:337`; `app/ponto/services/checar_ausencia.py:46`; `app/ponto/management/commands/rejulgar_status_por_tipo.py:35`; `app/ponto/admin.py:37`). Exaustividade contra escrita dinâmica ou clientes externos: **NAO SEI [nao verificado]**.

| GET | Quem emite; quem lê |
|---|---|
| `mes/ano` | Navegação mensal e query comum emitem; sanitizadores da view leem e produzem mês/ano efetivos (`app/templates/ponto/ausencias.html:38`, `app/templates/ponto/ausencias.html:40`; `app/ponto/views.py:2204`, `app/ponto/views.py:2171`). |
| `de/ate` | Inputs e query comum emitem; `_d` lê e normaliza; `_vigentes` recebe os limites efetivos (`app/templates/ponto/ausencias.html:72`; `app/ponto/views.py:2206`, `app/ponto/views.py:2208`, `app/ponto/views.py:2186`, `app/ponto/views.py:2190`). |
| `registro` | Input, chamados, notificação e grade emitem; view substitui o queryset e JS tenta autoabrir o painel (`app/templates/ponto/ausencias.html:74`; `app/chamados/views.py:1519`; `app/ponto/services/ausencia.py:828`; `app/templates/colaboradores/partials/_calendario_grade.html:97`; `app/ponto/views.py:2191`; `app/templates/ponto/ausencias.html:202`). |
| `colaborador/tipo` | Selects e query comum emitem; view lê e restringe antes de `_base` (`app/templates/ponto/ausencias.html:76`, `app/templates/ponto/ausencias.html:82`; `app/ponto/views.py:2212`, `app/ponto/views.py:2214`, `app/ponto/views.py:2173`, `app/ponto/views.py:2195`). |
| `status` | Chips, hidden e links de efeito emitem; view lê e filtra estados/caixa Pendente (`app/templates/ponto/ausencias.html:52`, `app/templates/ponto/ausencias.html:75`, `app/templates/ponto/ausencias.html:65`; `app/ponto/views.py:2175`, `app/ponto/views.py:2241`). |
| `efeito` | Chips de efeito e links de status emitem; view lê e traduz pelo mapa antes de montar o `Q` (`app/templates/ponto/ausencias.html:65`, `app/templates/ponto/ausencias.html:48`; `app/ponto/views.py:2249`, `app/ponto/views.py:2232`, `app/ponto/views.py:2254`). |

**Leitores externos e significados:**

- **PROCEDENTE — existe leitor externo de `data-value`.** O motor JS mede largura e ordena por esse atributo como texto genérico; passará a medir/ordenar pelo veredito. Não exige os literais antigos nem `?` (`app/templates/base.html:84`; `app/static/js/hasner-table.js:36`, `app/static/js/hasner-table.js:191`, `app/static/js/hasner-table.js:207`).
- **NAO PROCEDENTE — dependência identificada de ordem fixa dos hrefs.** A autoabertura usa `URLSearchParams`, e os testes seguem o href produzido; dependências externas à cópia são **[nao verificado]** (`app/templates/ponto/ausencias.html:202`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:118`).
- **NAO PROCEDENTE — API/export dependerem dos novos atributos HTML.** A API consulta Ausencia e emite tipo/status; export consulta Ausencia diretamente. Calendário/PDF têm seu caminho de decisão/palavra, separado da célula da lista (`app/api/views.py:1853`, `app/api/views.py:1865`; `app/folha/export.py:87`; `app/colaboradores/services/calendario.py:747`; `app/relatorios/pdf_espelho.py:923`).
- **NAO PROCEDENTE — todos os campos terem o mesmo vocabulário.** `efeito` continua `abona/desconta/suprime`; `veredito` é `abonado/descontado/suprimido/rejeitado`. A tradução da URL está explícita no mapa; `pendente` é uma caixa que reúne dois estados, não um dos quatro estados atuais (`app/ponto/models.py:850`; `app/ponto/services/dia_decidido.py:26`, `app/ponto/services/dia_decidido.py:80`; `app/ponto/models.py:701`; `app/ponto/views.py:2241`).
- **PROCEDENTE — há diferença preexistente de tratamento do desconhecido entre domínios.** O juiz visual termina em ABONADO quando o efeito é `None`; fechamento trata efeito diferente de ABONA/DESCONTA como suprimido. O diff não altera esses leitores (`app/ponto/services/dia_decidido.py:487`; `app/ponto/services/fechamento.py:698`, `app/ponto/services/fechamento.py:700`).

**MEDIR calendário: NAO PROCEDENTE para rejeitada entrar nesse mapa.** O calendário pede cobertura de cobrança; a autoridade exclui rejeitada antes de entregar os registros. Fechamento e `efeito_ausencia` pedem folha, que admite somente aprovada (`app/colaboradores/services/calendario.py:516`; `app/ponto/turnos.py:2765`, `app/ponto/turnos.py:2531`, `app/ponto/turnos.py:2529`; `app/ponto/services/fechamento.py:683`; `app/ponto/services/efeito_ausencia.py:53`).

**3. Classes que já reprovaram obra**

| Classe | Parecer |
|---|---|
| **(a) Segundo juiz** |

[truncado em 20000 de 27334 caracteres]

## 16/11 21:13 · sessao 88beb352811a · BUG-PAR-O-PAR · verificador · origem=Code

**1. A cura preserva a resposta, para o mesmo estado das fontes.** Não encontrei contraexemplo funcional nos casos pedidos: a irmã implícita obrigatoriamente pertence à mesma disputa e tem o mesmo `data_evento`; K6 deriva desse dia e da empresa do mesmo colaborador. (`app/chamados/services/materializacao.py:146`, `app/chamados/services/materializacao.py:788`, `app/chamados/services/validacao.py:225`)

- **Fora163, entregue:** a própria pergunta já encerra a guarda antes da irmã, preservando estado e texto. (`app/chamados/services/materializacao.py:805`, `app/chamados/services/materializacao.py:811`)
- **Fora163, não entregue:** a irmã elegível tem a mesma competência e também libera; irmã U163 é pulada. (`app/chamados/services/materializacao.py:795`, `app/chamados/services/materializacao.py:808`)
- **U163:** a busca da irmã permanece; uma irmã fora163 pode continuar barrando. Há teste específico dessa combinação. (`app/chamados/services/validacao.py:224`, `app/chamados/tests/test_bug_par_volta.py:145`)
- **Sem irmã ou irmã em outra competência:** a busca implícita devolve `None`; outra data não é candidata. Irmã passada explicitamente continua examinada, inclusive se tiver outra competência. (`app/chamados/services/materializacao.py:147`, `app/chamados/services/materializacao.py:150`, `app/chamados/services/validacao.py:219`)
- **L5, fora163 sem dia:** a busca antiga também não encontraria irmã; a própria pergunta continua devolvendo `bloqueada_sem_dia_do_fato`. U163 sem dia continua sendo pulada, como antes. (`app/chamados/services/materializacao.py:143`, `app/chamados/services/materializacao.py:795`, `app/chamados/services/materializacao.py:810`)

Igualdade diante de alterações concorrentes das fontes entre os antigos SELECTs separados: **[não verificado]**.

**2. O predicado é o mesmo.** O helper é a negação exata do antigo pulo apresentado no diff: pergunta existente, motivo de intervalo e fora163. O pertencimento continua vindo exclusivamente da PK no universo congelado. (`app/chamados/services/materializacao.py:795`, `app/chamados/services/materializacao.py:806`, `app/chamados/services/universo_bug_par.py:33`)

Não é segundo juiz: ele seleciona perguntas para a guarda; quem responde “competência entregue?” continua sendo `competencia_entregue`, registrado como K6. (`app/chamados/services/materializacao.py:789`, `app/core/juizes.py:343`, `app/docs/JUIZES.md:40`)

Uma precisão: helper verdadeiro significa que **a guarda examina a pergunta**, não que o juiz de entrega necessariamente foi consultado; sem dia, `competencia_do_fato` retorna antes dessa consulta. Isso conserva L5 separado de “entregue”. (`app/chamados/services/materializacao.py:784`, `app/chamados/services/materializacao.py:795`, `app/chamados/services/materializacao.py:810`)

**3. Escrita, leitura e chamadores de produção.**

| Função/carga | Chamadores diretos de produção |
|---|---|
| `entrega_que_barra_a_validacao` | Testemunha sem irmã explícita: `app/chamados/services/materializacao.py:886`. Porta com irmã explícita: `app/chamados/services/validacao.py:652` e `app/chamados/services/validacao.py:689`. |
| `k6_consulta_a_pergunta` | Composição: `app/chamados/services/validacao.py:224`. Própria guarda: `app/chamados/services/materializacao.py:806`. |
| `metade_irma_do_intervalo` | `par_do_intervalo`: `app/chamados/services/materializacao.py:194`; travas do plantio: `app/chamados/services/materializacao.py:2019`; `irma_da_validacao`: `app/chamados/services/validacao.py:206`. |
| `montar_chamados_fio` | `meu_atendimento`, `fio_colaborador`, `modal_fio_colaborador` e `contexto_parcial_fio`: `app/chamados/views.py:569`, `app/chamados/views.py:677`, `app/chamados/views.py:889`, `app/chamados/services/fio.py:492`. |

Essas alterações não acrescentam escrita persistente: produzem seleção, retorno e carga de relações; o fio atribui o resultado ao atributo transitório `estado_aplicacao`. (`app/chamados/services/validacao.py:224`, `app/chamados/services/materializacao.py:795`, `app/chamados/services/fio.py:216`, `app/chamados/services/fio.py:376`)

Nos insumos, `motivo` entra no nascimento da pergunta; `data_evento` passa pelo atribuidor `gravar_dia_do_fato`, usado pelo nascimento, reapontamento e rechaveamento. O delta apenas os lê. (`app/chamados/models.py:3124`, `app/chamados/models.py:3126`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:847`, `app/chamados/services/materializacao.py:147`)

As relações disputa–colaborador/chamado são preenchidas na emissão; empresa é preenchida pelo cadastro/edição e transferência. Censo absoluto de todos os escritores históricos desses campos: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:601`, `app/chamados/services/disputa_emissao.py:1137`, `app/chamados/services/disputa_emissao.py:1372`, `app/colaboradores/services/colaborador.py:152`, `app/colaboradores/services/colaborador.py:204`, `app/colaboradores/views.py:1406`)

Leitores transitivos do estado: fio, explicador, filtro do card, espera declarado×gravado, porta da escrita e contador; o comando lê o contador. (`app/chamados/services/fio.py:376`, `app/chamados/services/explicador.py:124`, `app/chamados/templatetags/chamados_tags.py:136`, `app/chamados/services/materializacao.py:757`, `app/chamados/services/materializacao.py:772`, `app/chamados/services/materializacao.py:339`, `app/chamados/management/commands/bug_par_103.py:79`)

**Nenhum desses consumidores dependia de efeito colateral da busca retirada:** a irmã local só alimentava K6; a porta conserva sua própria busca, suas travas e os argumentos explícitos. Paridade e travas do materializador também conservam buscas próprias. (`app/chamados/services/validacao.py:225`, `app/chamados/services/validacao.py:638`, `app/chamados/services/validacao.py:647`, `app/chamados/services/materializacao.py:194`, `app/chamados/services/materializacao.py:2019`)

**4. O `select_related` altera a carga, preservando a seleção.** Filtros e ordenação permanecem; não há `distinct` acrescentado. As relações são FKs unitárias, com colaborador e empresa obrigatórios; chamado já era filtrado como não nulo. Portanto, para relações válidas e estáveis, não multiplica disputas nem muda sua prioridade. (`app/chamados/services/fio.py:213`, `app/chamados/services/fio.py:216`, `app/chamados/services/fio.py:219`, `app/chamados/models.py:1800`, `app/chamados/models.py:1813`, `app/colaboradores/models.py:321`)

O carregamento posterior das perguntas e o prefetch das decisões continuam em consulta própria. (`app/chamados/services/fio.py:257`, `app/chamados/services/fio.py:263`)

**5. Os testes têm mordidas diferentes.** [nao verificado]

O teto captura consultas reais e exige `≤90`; mede o primeiro colaborador, cuja fixture contém uma pergunta de intervalo. (`app/core/tests/test_selo_performance.py:143`, `app/core/tests/test_selo_performance.py:317`, `app/core/tests/test_selo_performance.py:338`, `app/core/tests/test_selo_performance.py:344`, `app/core/tests/test_selo_performance.py:379`)

Partindo dos **90 informados no anexo**, reintroduzir a busca sempre acrescentaria uma consulta, chegando a **91**; reverter apenas a carga de colaborador/empresa acrescentaria duas, chegando a **92**. Essa é dedução condicional pelos acessos restaurados; execução individual desses mutantes contra o teto Django: **[não verificado]**. (`app/chamados/services/validacao.py:206`, `app/chamados/services/validacao.py:224`, `app/chamados/services/validacao.py:225`, `app/chamados/services/materializacao.py:808`)

Na execução em memória desta revisão, o AST passou **7/7**; os dois mutantes de performance também passaram; omitir K6 produziu falha por asserção. O AST cobra competência, consulta ao juiz e ausência de F6; não mede consultas ORM. (`app/chamados/tests/test_bug_par_sem_banco.py:123`, `app/chamados/tests/test_bug_par_sem_banco.py:140`, `app/chamados/tests/test_bug_par_sem_banco.py:145`)

Assim, não há selo demonstravelmente vazio: teto e AST têm asserções efetivas. O RED fornecido de 93 demonstra a regressão conjunta anterior; não demonstra isoladamente cada reversão c4c, que também não consta do registro de mutantes. (`app/core/tests/test_selo_performance.py:344`, `app/chamados/tests/test_bug_par_sem_banco.py:147`, `app/chamados/tests/bug_par_mutantes.py:122`, `app/chamados/tests/bug_par_mutantes.py:149`; execução isolada: **[não verificado]**)

**6. Nenhuma das classes de reprovação aparece demonstrada neste delta.** Não nasce segundo juiz, escritor persistente, relógio ou derivação paralela; a composição continua chamando a mesma guarda e o mesmo K6. L5 e competência entregue conservam estados distintos, e o helper concentra o predicado de entrada da guarda. (`app/chamados/services/validacao.py:216`, `app/chamados/services/validacao.py:225`, `app/chamados/services/materializacao.py:795`, `app/chamados/services/materializacao.py:806`, `app/chamados/services/materializacao.py:810`, `app/chamados/services/materializacao.py:812`)

VEREDITO: APROVA

## 16/11 21:12 · sessao 9ddb756d571c · - · - · origem=Ronald (tmux)

- **Chamadores diretos de produção:** `entrega_que_barra_a_validacao`: testemunha sem irmã explícita em `app/chamados/services/materializacao.py:886`; porta com irmã explícita em `app/chamados/services/validacao.py:652` e `:689`. `k6_consulta_a_pergunta`: composição em `app/chamados/services/validacao.py:224` e guarda em `app/chamados/services/materializacao.py:806`. `metade_irma_do_intervalo`: `par_do_intervalo` em `app/chamados/services/materializacao.py:194`, `travar_o_ato_do_plantio` em `:2019` e `irma_da_validacao` em `app/chamados/services/validacao.py:206`.

- **Nenhum outro consumidor dependia da busca suprimida:** a variável local `irma` da composição serve somente à chamada `_k6_que_barra`; a porta calcula sua própria `_irma` antes das travas e a passa explicitamente nas duas verificações K6. Paridade e travas conservam as buscas próprias. (`app/chamados/services/validacao.py:224`, `:225`, `:638`, `:647`, `:652`, `:689`; `app/chamados/services/materializacao.py:194`, `:2019`.)

- **Chamadores de `montar_chamados_fio`:** `meu_atendimento`, `fio_colaborador`, `modal_fio_colaborador` e `contexto_parcial_fio`. (`app/chamados/views.py:569`, `:677`, `:889`; `app/chamados/services/fio.py:492`.)

- **Leitores transitivos do estado:** fio (`app/chamados/services/fio.py:375`), explicador (`app/chamados/services/explicador.py:124`), filtro de apresentação (`app/chamados/templatetags/chamados_tags.py:136`), `espera_declarado_x_gravado` (`app/chamados/services/materializacao.py:757`), porta de escrita (`:772`) e contador (`:339`); o comando lê o contador (`app/chamados/management/commands/bug_par_103.py:79`). O fio acessa a função reexportada em `app/chamados/models.py:4255`.

- **Escritores dos insumos:** `motivo` nasce no construtor da pergunta e o registro é inserido pela porta `_nascer`; `data_evento` passa pelo atribuidor `gravar_dia_do_fato`, chamado pelo nascimento, reapontamento e rechaveamento. (`app/chamados/models.py:3124`, `:3126`, `:3127`; `app/chamados/services/fio.py:837`, `:847`, `:926`, `:944`, `:959`, `:1084`.) `pk` é lido pelo pertencimento congelado; o carregador verifica arquivo, hash e conjuntos, sem derivar pertencimento da resposta ou do estado. (`app/chamados/services/universo_bug_par.py:7`, `:14`, `:25`, `:33`.)

- **Relações carregadas:** `DisputaSupervisao.colaborador` e `.chamado` são preenchidos nos três nascimentos da disputa encontrados; `Colaborador.empresa` vem do cadastro/edição e pode mudar pela transferência. (`app/chamados/services/disputa_emissao.py:601`, `:1137`, `:1372`; `app/colaboradores/views.py:1324`, `:1406`; portas cadastrais em `app/colaboradores/services/colaborador.py:152`, `:204`.) O delta somente lê essas relações; escreve o atributo transitório `estado_aplicacao` da instância, sem persistência nesse trecho. (`app/chamados/services/fio.py:213`, `:216`, `:375`, `:376`.)

- **`select_related`:** conserva exatamente os filtros e a ordenação; `todas_disputas` não recebe `distinct` nem `prefetch_related`. São relações FK unitárias: `Disputa.colaborador` e `Colaborador.empresa` não nulas; `chamado` aceita nulo, mas a consulta já exige não nulo. Portanto, para o estado relacional válido e estável, os objetos associados passam a vir na mesma consulta, sem multiplicar disputas nem alterar a escolha de disputa. (`app/chamados/services/fio.py:213`, `:216`, `:219`, `:248`, `:252`; `app/chamados/models.py:1800`, `:1813`; `app/colaboradores/models.py:321`.) Garantia sob atualização concorrente entre os antigos SELECTs separados: **[não verificado]**.

- Não encontrei `from_db`, `__init__`, manager próprio ou `post_init` das classes `DisputaSupervisao`, `Colaborador` e `Empresa`; ausência absoluta de gatilhos externos ou de outros escritores históricos: **[não verificado]**.

## 16/11 21:11 · sessao af619cee5ad4 · - · - · origem=Ronald (tmux)

5. O teto tem mordida por contagem real: 90 em `app/core/tests/test_selo_performance.py:143`, captura em `:333-340`, `assertLessEqual` em `:342-351`, modal em `:377-380`. A fixture do colaborador medido tem uma pergunta de intervalo (`:317-322`, `:379`).

Partindo dos 90 informados no anexo, irmã sempre acrescentaria uma consulta (91); reverter somente `colaborador__empresa` acrescentaria duas (92), pelos acessos `validacao.py:224-225` e `materializacao.py:808`. É dedução condicional; execução desses mutantes contra o teto Django: **[não verificado]**.

Executei exclusivamente em memória com `python3 -B`: [nao verificado]

- AST sem banco: sete testes verdes. [nao verificado]
- Mutante irmã sempre: sete verdes.
- Mutante sem eager loading de colaborador/empresa: sete verdes.
- Mutante K6 omitido: uma falha por asserção, zero erros (`'10/2026' not found in ''`).

A carga AST compartilha o ambiente real (`app/chamados/tests/e4_apoio.py:8-16`); o teste cobra competência, consulta K6 e ausência de F6 (`app/chamados/tests/test_bug_par_sem_banco.py:123-149`). Portanto ele morde a semântica K6; a proteção das duas otimizações pertence ao teto, não a esse teste. Resultados executados nesta inspeção não foram salvos em arquivo; referências persistidas dos resultados: **[não verificado]**.

Os mutantes registrados não incluem reversões c4c de performance (`app/chamados/tests/bug_par_mutantes.py:122-149`). Isso limita a prova executada apresentada: o RED raspado demonstra a combinação anterior de 93, sem demonstrar individualmente cada reversão **[não verificado]**. Não encontrei selo sem asserção: o teto usa `assertLessEqual` (`test_selo_performance.py:344`); o AST usa valores e contagens concretos (`test_bug_par_sem_banco.py:145-149`).

6. Nas alterações examinadas não aparece escritor nem relógio novo: são seleção da irmã, consulta ao mesmo guard e eager loading (`validacao.py:216-225`; `materializacao.py:792-812`; `fio.py:213-216`). O helper é chamado pelo próprio guard (`materializacao.py:806`) e conserva a competência em `competencia_do_fato` (`:808`), sem derivação concorrente nesta fatia.

_6192 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
