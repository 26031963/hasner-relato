# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 11:41** · publicado em **12/11/2026 11:42** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 11:41 · sessao e7dc53643837 · - · - · origem=Ronald (tmux)

Censo AST executado em memória: 767 arquivos Python de produção em `app`, excluindo testes, migrations, assets e logs; zero falhas de parse. Reutilizei por extração AST o detector de `app/chamados/tests/test_o261e_f2_porta_carimbo.py:99`, com campos ampliados; ele cobre atribuição, `setattr`, dicionários propagados, construtor, `update` e `bulk_update`, mas declara limites para nomes vindos de outro módulo e `__dict__` em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`. Execução de testes e confirmação no banco: [nao verificado].

**Escritores físicos encontrados**

| Campo | Atribuição e persistência |
|---|---|
| `via_resolucao` | `_escrever_carimbo`: `app/chamados/models.py:3102`; persistido por `carimbar` em `:3153`, `carimbar_identidade` em `:3191` e `descarimbar` em `:3213`. |
| `via_resolucao` em lote | `PerguntaDisputa.objects…update` em `app/chamados/models.py:3265`, com via vazia, materialização e autor nulos em `:3267`. |
| `via_resolucao` isolada | `encerrar_por_fato_no_espelho`, atribuição/save em `app/chamados/models.py:3276`; `relavrar`, atribuição/save em `app/chamados/services/relavra_via.py:125`. |
| `respondida_em` | `carimbar`, atribuição em `app/chamados/models.py:3133`, save em `:3153`; resposta humana em `app/chamados/services/disputa_emissao.py:1468`, save em `:1508`; automática em `:1653`, save em `:1655`; recuperação pela trilha em `:1674`, save em `:1676`. |
| `validada_em`/`validada_por` | Simetria de `carimbar` em `app/chamados/models.py:3136`, save em `:3153`; ato de validar em `app/chamados/services/validacao.py:134`, save em `:137`; devolução da validação em `:199`, save em `:202`; resposta nova limpa ambos em `app/chamados/services/disputa_emissao.py:1480`, save em `:1508`. |
| Limpeza dos três campos de resposta/validação | `ressuscitar_perguntas` passa campos explícitos a `descarimbar_em_lote` em `app/chamados/models.py:1567`; `reabrir_para_correcao` monta `campos` em `app/chamados/services/disputa_emissao.py:1593` e chama a mesma porta em `:1630`. |
| Alterações somente em memória | Cópia para consultar o C6 em `app/chamados/models.py:1507`; restauração da tentativa recusada em `:3163`. |

Não apareceu outro `bulk_update` nem outro `QuerySet.update` desses campos no universo acima [nao verificado como garantia além dos limites do detector]. `flip_auto.py:128` é **leitor** por `filter`, não escritor; `competencia_trancada.py:126` determina saída por leitura e `:255` apenas reabre o legado.

**Apelidos e reexports**

- Reabertura: `_reabrir` em `app/chamados/models.py:2220` e `app/chamados/services/acoes_disputa.py:102`; `_reabrir2` em `app/chamados/views.py:1435`.
- `PerguntaDisputa` como `_PD`: `app/colaboradores/models.py:432`, `app/chamados/veredito_lavrado.py:201`, `app/chamados/views.py:398`, `app/colaboradores/services/calendario.py:469`, `app/ponto/services/cartorio.py:1635`, `app/chamados/services/limbo_folgas.py:270`, `app/chamados/services/disputa_emissao.py:1501`, `app/chamados/services/raiox.py:177` e `app/chamados/services/fio.py:392`, `:768`, `:883`.
- `_PD_ordem`: `app/ponto/services/declarar_dia.py:189`, `app/ponto/services/veredito_celula.py:225`, `app/chamados/services/validacao.py:428`; `_PD_inf`: `app/chamados/services/painel.py:260`.
- Reexports explícitos dos escritores pelo modelo: `app/chamados/models.py:4088` e `:4093`. Importação `*` de `chamados` na produção varrida: não encontrada [nao verificado fora desse universo].

**Matriz dos caminhos principais**

| Entrada → juiz → operação | Persistência → retorno → prova aberta |
|---|---|
| Pergunta sem validação → `carimbar` consulta C6 **depois de aplicar carimbos**, `app/chamados/models.py:3130` e `:3140` | Trio materialização/via; havendo texto, `resposta='validar'` preenche resposta e validação antes do C6, `:3131`; save `:3153`, `True` em `:3158`; viva desfaz e retorna `False`, `:3144`. Testes de recusa/restauração: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:665`, `:687`, `:713`. |
| Admin decide/silêncio → não consulta C6 na porta de identidade, `app/chamados/models.py:3169` | `fechar_por_admin` escolhe `admin_decidiu`, `:3065`; trio em `:3187`, save/`True` em `:3191`; idempotente `False` em `:3056`. Fecho explícito da disputa grava `DecisaoPergunta.SILENCIO`, `:1920`; prova `app/chamados/tests/test_e1_entradas.py:121`. |
| Dia decidido pelo admin → lista `q_vivas` daquele dia, `app/ponto/services/veredito_celula.py:180` | `fechar_por_admin` em `:190`; linha `SILENCIO` em `:195`. Prova real e idempotência: `app/chamados/tests/test_e1_v5_portas_backfill.py:83`. |
| Folga aceita → juiz `o_dia_ja_e_folga` ou `checar_declaracao`, `app/chamados/services/validacao.py:280`, `:305` | Validação + `admin_decidiu`, `:288`, `:320`, `:329`; retorna `folga_ja_declarada` em `:302` ou `folga_declarada` em `:367`; recusa posterior reverte transação em `:343`. Provas nomeadas em `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:136` e `:197`; corpo não aberto [nao verificado]. |
| Motivo removido → pertence aos motivos atuais e disputa aberta, `app/chamados/models.py:2034` | Linha `RETRATACAO` em `:2057`, identidade `motivo_retratado` em `:2062`, conserva carimbo anterior; retorna `removido` em `:2068`. Prova aberta preserva resposta e constata morte: `app/chamados/tests/test_e1_entradas.py:142`. |
| Duplicata → `plano_de_retratacao`, `app/chamados/services/fio.py:1341` | `retratar_duplicata` grava `duplicata`, conserva carimbo, anota canônica em `:1360`; `True` em `:1364`, já duplicata `False` em `:1351`. Prova aberta apply/idempotência: `app/chamados/tests/test_e1_v5_portas_backfill.py:207`; veto de mérito: `app/chamados/tests/test_e1_v9.py:210`. |
| Paridade recusada → exige lâmpada `is True`, `app/chamados/services/materializacao.py:906` | Identidade `paridade_recusada` em `:930`, retorna `None` em `:931`; apagada apenas recusa e retorna `None`, `:919`. Provas abertas dos três valores: `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:175`, `:183`, `:192`. |
| Colisão tipo oposto → `traduzir_recusa`, `app/chamados/services/materializacao.py:1097` | Sem materialização e código `COLISAO`, identidade `fato_ocupado` + motivo em `:1105`; sem teste local da lâmpada. Retorno implícito `None`, função termina em `:1113`; prova específica [nao verificado]. |
| Ausência declarada → `declarou_hora as _declarou_hora`, `app/chamados/services/materializacao.py:548`; C6 dentro da porta | Irmãs não materializadas: `declaracao` ou `ausencia_dia`, `:555`; `carimbar(...resposta='validar')`, `:567`; se gravou, linha `ABSORCAO`, `:569`; se recusou, evento e lista de recusadas, `:573`. Prova de recusa aberta apenas por localização: `app/chamados/tests/test_o261e_f2_fila.py:238` [nao verificado corpo]. |
| Materializador → juiz de lastro após plantio, `app/chamados/services/materializacao.py:1240`; via por `_declarou`/`_via_fato`, `:1267` | `carimbar` com simetria da validação e lavra, `:1283`; C6 recusa → `_Reprovada`, rollback, `:1285`. Prova específica de rollback localizada em `app/chamados/tests/test_hx_materializa_depois_julga.py:30` [nao verificado corpo]. |
| Fecho de disputa, pendentes aprovadas → lastro em `app/chamados/services/materializacao.py:1433`, C6 na porta | `juiz_aprovou` em `:1437`, devolve lista em `:1439`; prova aberta barra resposta não validada: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:687`. |
| Sem turno aberto → motivo limitado, sem texto/materialização e núcleo sem turno, `app/chamados/reconciliador.py:363`; C6 antes em `:398`, novamente pela porta em `:406` | Trio com `sem_turno_aberto`; eventual fecho da disputa por C7 em `:412`; retorna `True`, `:415`. Prova aberta **com marco apagado e validação prévia**: `app/chamados/tests/test_o261_lavra_le_a_lampada.py:148`; construção da lâmpada apagada em `:115`. |
| Célula absolveu → `perguntas_absolvidas_pela_celula`, `app/chamados/reconciliador.py:585`; C6 na porta | `celula_absolveu`, `:596`; contabiliza e eventualmente fecha por C7, `:612`. Prova localizada em `app/chamados/tests/test_selo_celula_absolveu.py:66` [nao verificado corpo]. |
| Fantasma atual → `veredito_orfa == ENCERRA`, `app/chamados/reconciliador.py:1108` | Consumidor chama `encerrar_fantasma`; porta passa pelo C6, `app/chamados/models.py:3288`. Provas abertas: barra viva respondida em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:665`; aceita turno fechado em `:676`. |
| Fato materializado → comando exige lâmpada `is True`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:41` | Só troca via, `app/chamados/models.py:3276`; retorna `True`, `:3278`; não é via cega nesse chamador. Prova específica [nao verificado]. |
| Re-lavra de carimbo derivado → `declarou_hora`/`via_pelo_fato`, `app/chamados/services/relavra_via.py:95` | Altera via e lavra no mesmo save `:125`; retorna `(n_via,n_ver)`, `:139`; não exige lâmpada localmente. Prova específica [nao verificado]. |

**Divergência comprovada para decisão**

O `carimbar` exige “C6 deu morta”, mas o C6 pode responder sobre a **validação que o próprio carimbar acabou de preencher**, antes de conferir o efeito do marco: `app/chamados/models.py:3135` e `:3140`. A outra leitura é a lâmpada de `CLAUDE.md:169`. O teste `app/chamados/tests/test_o261_lavra_le_a_lampada.py:148` comprova a variante já validada: lâmpada de entrada apagada, via `sem_turno_aberto` persistida e disputa fechada. O teste de paridade explicita que validada sai do fio mesmo sem absolvição por fato, `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:210`. Cabe decisão numerada na consolidação, sem escolher entre essas leituras.

**Cobertura no backlog aberto**

- E4 cobre o conjunto de portas de estado, com E4b backfill, E4c leitores e E4d parada da projeção: `app/docs/BACKLOG.md:1885`.
- Identidade/duplicata tem fato próprio adiado para depois da E4d: `app/docs/BACKLOG.md:1889`; excedentes duplicados estão explicitamente em E1/E4: `:1912`.
- Reação da raiz aos carimbadores e fecho fica em RAIZ F3/F4; F5 se combina com E4d: `app/docs/BACKLOG.md:1904`; os sete carimbadores sem C7 estão nomeados em `:1911`.
- `celula_absolveu` pelo dia errado tem complemento específico **ABSOLVIDA-PELA-FOLGA**, E1-ENTRADAS, com pergunta #51407: `app/docs/BACKLOG.md:1935`. Isso não demonstra a classe dos outros 14 dias [nao verificado].

## 12/11 11:40 · sessao 92e371f1103f · - · - · origem=Ronald (tmux)

BLOQUEIA — `app/chamados/services/explicador.py:137` duplica C7 incompleto: define `pode_fechar = not fio_vivo(ps)`, mas C7 exige também ausência de respostas sem veredito (`app/chamados/models.py:1866`). Contraexemplo: pergunta com texto e `respondida_em`, sem validação/materialização, célula `ok` decidida pelo admin; C6 pode considerá-la morta (`app/chamados/juizes.py:1404`, `app/chamados/juizes.py:1452`), enquanto a guarda mantém a resposta pendente (`app/chamados/models.py:1855`). Explicador publica “Tudo resolvido” (`app/chamados/services/explicador.py:140`), modal oferece fechar/regularizar (`app/templates/chamados/partials/modal_fio.html:446`), e fechamento recusa (`app/chamados/models.py:1899`).

BLOQUEIA — #9 deixa silêncio vencido com significados diferentes: fila e lista do colaborador excluem somente futuro (`app/chamados/services/painel.py:190`, `app/chamados/services/janela_colab.py:78`), mas a partição classifica qualquer prazo vencido como `HISTORICO` (`app/chamados/services/verbos.py:48`), e a lista padrão administrativa o exclui (`app/chamados/services/painel.py:324`). Contraexemplo: chamado aberto com silêncio vencido aparece ao colaborador e entra na fila, porém desaparece da lista padrão do admin; a tarefa ainda remove esse chamado de sua `fila` publicada (`app/chamados/services/tarefa_do_dia.py:551`). Os testes novos verificam `qs_na_fila` e `chamados_visiveis_colab`, sem verificar a lista efetiva do painel (`app/chamados/tests/test_e3_b4_silencio_vencido.py:36`).

FILA — `respondida_em`: escritor de resposta humana em `app/chamados/services/disputa_emissao.py:1468`, com projeção e decisão no mesmo ato (`app/chamados/services/disputa_emissao.py:1503`); escritores adicionais de resposta automática e carimbo em `app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3145`; leitores relevantes C6 (`app/chamados/juizes.py:1375`), aviso corrigido (`app/chamados/services/fio.py:174`) e crédito ainda por timestamp (`app/chamados/services/raiox.py:139`). Contraexemplo: resposta com timestamp e texto vazio vira crédito de respondida no painel, mas o aviso continua esperando o colaborador (`app/chamados/services/raiox.py:139`, `app/chamados/services/fio.py:174`).

FILA — `silenciado_ate`: escritores conferidos `arquivo.silenciar` e `arquivo.levantar` (`app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:202`); instante futuro estrito, igualdade significa vencido (`app/chamados/models.py:512`, `app/chamados/models.py:524`). Contraexemplo de leitor divergente: prazo vencido permanece histórico na partição (`app/chamados/services/verbos.py:51`).

FILA — `data_evento`: atribuição direta única encontrada em `app/chamados/services/fio.py:819`; nascimento usa essa porta (`app/chamados/models.py:3029`), costura também (`app/chamados/services/fio.py:898`) e rechaveamento também (`app/chamados/services/fio.py:1056`). Campo significa dia do turno para fato (`app/chamados/models.py:2796`) e dia civil para conversa (`app/chamados/models.py:2808`); C19 separa `conversa=False` (`app/chamados/services/tarefa_do_dia.py:375`). Ocorrência material adicional de mistura entre esses significados: nada demonstrado; incidência em produção [nao verificado].

FILA — `status_local`: núcleo persistente em `app/chamados/models.py:228`; nascimento em `app/chamados/models.py:1108`; outras atribuições encontradas simulam e restauram estado em memória (`app/chamados/models.py:356`, `app/chamados/models.py:371`). Segundo escritor direto fora dessas portas: nada encontrado; reflexão e kwargs dinâmicos [nao verificado].

FILA — porta: saída escrita em memória pelo próprio C19 (`app/chamados/services/tarefa_do_dia.py:555`), projetada por `porta_da_tarefa` (`app/chamados/services/tarefa_do_dia.py:565`); não há persistência nesse trecho (`app/chamados/services/tarefa_do_dia.py:540`). Contraexemplo material: silêncio vencido pode manter `rota=porta_fila` enquanto `fila=()` porque `da_celula` é calculado antes da exclusão de `HISTORICO` (`app/chamados/services/tarefa_do_dia.py:550`, `app/chamados/services/tarefa_do_dia.py:555`).

FILA — ordem de trava: nada novo material encontrado nos caminhos conferidos; núcleo de status trava universo antes de salvar (`app/chamados/models.py:280`), silêncio pede a porta de ordem (`app/chamados/services/arquivo.py:170`, `app/chamados/services/arquivo.py:209`), rechaveamento trava antes da releitura/escrita (`app/chamados/services/fio.py:1025`). Ordem implementada C→D→CH→P em `app/chamados/models.py:2679`, `app/chamados/models.py:2688`, `app/chamados/models.py:2701`, `app/chamados/models.py:2705`; cobertura global/transitiva [nao verificado].

## 12/11 11:39 · sessao bf78c34d5db7 · - · - · origem=Ronald (tmux)

BLOQUEIA — item 5, respondedores vivos: `raiox.creditos_perguntas` ainda separa respondidas por timestamp (`app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:139`); `relatorios_disputa.classificar` decide dono por C6 + timestamp (`app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:21`); `_calados` usa `q_vivas` + timestamp nulo (`app/chamados/services/cobranca.py:229`); cartão deriva documento/formulário/espera no template (`app/templates/chamados/partials/_card_atendimento.html:65`, `app/templates/chamados/partials/_card_atendimento.html:77`, `app/templates/chamados/partials/_card_atendimento.html:94`). Contraexemplo: resposta vazia com timestamp continua tarefa RESPONDER do colaborador (`app/chamados/services/tarefa_do_dia.py:135`), mas relatório diz AGUARDA_SUPERVISAO (`app/chamados/relatorios_disputa.py:23`) e cartão diz “Aguardando a supervisao conferir” (`app/templates/chamados/partials/_card_atendimento.html:101`).

BLOQUEIA — API/badge/PWA não migraram: `consulta_do_app` ainda exige timestamp nulo + disputa aberta para pendente (`app/chamados/services/pergunta_no_app.py:70`); `projecao_do_app` conta essa lista (`app/chamados/services/pergunta_no_app.py:90`). Consumidores: dois kernels (`app/api/views.py:315`, `app/api/views_core.py:303`), API chamados (`app/api/views.py:993`), API perguntas (`app/api/views.py:2533`) e dashboard (`app/core/views.py:252`). Contraexemplo: resposta vazia com timestamp cai no histórico, badge não conta RESPONDER, enquanto tarefa mantém RESPONDER (`app/chamados/services/tarefa_do_dia.py:137`); o próprio novo teste identifica essa resposta como ainda devida pelo colaborador (`app/chamados/tests/test_e3_leitores_fio_disputa.py:143`).

BLOQUEIA — tela admin/fio continuam derivando dono por fonte antiga: partição chama `creditos_perguntas` (`app/chamados/services/verbos.py:34`); natureza prioriza resposta creditada como `admin_validar` (`app/chamados/catalogo/natureza.py:48`); fio passa esses créditos ao desenlace (`app/chamados/services/fio.py:405`, `app/chamados/services/fio.py:421`); desenlace imprime “Sua vez: Validar” (`app/chamados/services/desenlace.py:72`). Contraexemplo: resposta vazia timestamp presente produz crédito respondido (`app/chamados/services/raiox.py:139`) e oferece validação onde tarefa exige RESPONDER (`app/chamados/services/tarefa_do_dia.py:137`).

BLOQUEIA — #9 não resolve o consumidor real da fila: silêncio vencido vira HISTORICO na partição (`app/chamados/services/verbos.py:48`), e painel padrão exclui esses ids (`app/chamados/services/painel.py:324`). Ao mesmo tempo, queryset da fila e lista do colaborador devolvem o chamado (`app/chamados/services/painel.py:189`, `app/chamados/services/janela_colab.py:77`). Contraexemplo: chamado aberto com `silenciado_ate=agora−1min` aparece para colaborador e no contador COBRANCA (`app/chamados/services/painel.py:212`), mas não na lista padrão administrativa (`app/chamados/services/painel.py:331`). RED #9 verifica somente os dois querysets (`app/chamados/tests/test_e3_b4_silencio_vencido.py:35`), portanto não morde essa divergência de apresentação.

BLOQUEIA — porta da tarefa pode publicar fila vazia sob silêncio vencido: `alimentar_fila` inclui chamado vencido em `fila_ids` (`app/chamados/juizes.py:1533`); `_portas_da_tarefa` mantém esse inteiro em `portas` (`app/chamados/services/tarefa_do_dia.py:548`), exclui HISTORICO apenas de `fila` (`app/chamados/services/tarefa_do_dia.py:551`) e publica rota porque `portas` permanece preenchido (`app/chamados/services/tarefa_do_dia.py:555`). Contraexemplo: pergunta exigível ancorada no chamado vencido da célula gera `porta_fila`, `fila=()`, enquanto lista admin esconde a âncora (`app/chamados/services/painel.py:331`).

BLOQUEIA — item 6, selo não cobre o universo prometido: catraca exige somente um chamador C17 (`app/chamados/tests/test_e3_pendentes_app.py:145`); seu mutante acrescenta chamada C17 (`app/chamados/tests/test_e3_pendentes_app.py:153`). Contraexemplo: todos os leitores próprios acima permanecem e a catraca não os busca, pois o detector só reconhece o alvo literal resolvido `chamados.juizes.porta_alcancavel_do_dia` (`app/chamados/tests/test_e3_pendentes_app.py:33`, `app/chamados/tests/test_e3_pendentes_app.py:89`).

BLOQUEIA — leitor administrativo adicional não migrou: calendário consulta C6 sem relógio recebido (`app/colaboradores/services/calendario.py:476`), exclui texto vazio antes do juiz (`app/colaboradores/services/calendario.py:474`) e calcula `pode_ultimo_recurso` separadamente (`app/colaboradores/services/calendario.py:738`), consumido por `resolver_dia_oferecido` (`app/colaboradores/services/calendario.py:103`). Contraexemplo: pergunta viva com resposta vazia em competência passada não bloqueia essa oferta, enquanto tarefa publica RESPONDER (`app/chamados/services/tarefa_do_dia.py:137`); incidência real [nao verificado].

BLOQUEIA — copiloto permanece com derivação e relógio próprios: `dia_do_colab` publica C6 sem `agora` (`app/ponto/services/dia_do_colab.py:71`) e delega retenção ao antigo `do_dia` (`app/ponto/services/dia_do_colab.py:108`), que lê `timezone.localdate`, VIVOS, universo do app e `retencao` (`app/chamados/services/pergunta_no_app.py:217`, `app/chamados/services/pergunta_no_app.py:223`). Contraexemplo: pergunta viva respondida com texto segue descrita genericamente como esperando “resposta ou decisao” (`app/ponto/services/dia_do_colab.py:148`), sem publicar tarefa/porta única (`app/ponto/services/dia_do_colab.py:73`); contador de leitores fora da tarefa e pergunta golden solicitados não foram encontrados por busca textual, ausência completa [nao verificado].

FILA — badge navbar mantém unidade chamado em fila (`app/core/context_processors.py:185`), diferente do badge de perguntas pendentes (`app/chamados/services/pergunta_no_app.py:91`); contraexemplo: chamado na fila sem pergunta produz navbar 1, enquanto tarefa pode ter apenas ação do sistema e nenhuma porta, caso já explicitado no novo teste (`app/chamados/tests/test_e3_leitores_c17_chamadores.py:101`). Migração desse sítio permanece sem RED específico identificado [nao verificado].

FILA — `_calados` ainda trata pré-filtro como vida exata (`app/chamados/services/cobranca.py:229`), embora o contrato de `q_vivas` declare seleção mais generosa que C6 (`app/chamados/models.py:3349`). Contraexemplo: pergunta sem resposta em dia trancado por leitura pode entrar em cobrança apesar de morta no C6; incidência e execução [nao verificado].

## 12/11 11:39 · sessao eef0afa7e826 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

- **B1 curado:** resumo e marca usam o mesmo conjunto de instantes. RED na c14 (`0 != 1`) e GREEN após a cura. [Evidência](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:6).
- **B2:** selo compara PDF e tela, incluindo a marca de 10/06. A fixture original não executou por ausência de Django; seus números permanecem **[nao verificado]**. [Detalhes](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:16).
- **B3:** mantido o duplo porque o recalculador exige leituras ORM. A prova parcial mata três mutantes; `recalculador_vazio` sobrevive, expondo a lacuna integrada. [Justificativa em três linhas](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:24).

**37 testes sem banco e ruff passaram; oito mutantes mortos e um sobrevivente.** Comandos e saídas no [relatório](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:34). [nao verificado]

Sem commit, respeitando a proibição inicial. [Registro](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c15_execucoes.r:36). [nao verificado]

## 12/11 11:39 · sessao be892ef56153 · - · - · origem=Ronald (tmux)

BLOQUEIA — item 7, instrumento diverge do vigia migrado: `diff_e3_sombra.py:54` calcula `exige_admin` pelos verbos; `vigia_fio.py:650` usa `porta['fio'] == 'admin'`. Contraexemplo: pergunta respondida sem texto, verbo `validar`, fio `colab`: sombra informa admin=True, vigia False; o próprio caso está em `test_o261e_f5_alcance.py:177`, com assert False em `:192`. O teste do instrumento conserva a resposta antiga em `test_e3_diff_sombra.py:143`. [nao verificado]

BLOQUEIA — selo/mutante sem alvo: `e3_mutantes.py:112` procura o antigo `return` da candidata, mas o código agora tem alias em `diff_e3_sombra.py:43`; o harness encerra por `AssertionError('Mutante sem alvo')` em `e3_mutantes.py:162`. Contagem literal por AST em memória confirmou 0 alvos; morte do mutante por teste: [nao verificado]. [nao verificado]

BLOQUEIA — mutante de relógio atinge sítio errado e é equivalente: `e3_mutantes.py:101` troca o primeiro `agora=self.agora)` por `agora=None)`; há dois alvos, e `replace(...,1)` em `:164` alcança C6 em `tarefa_do_dia.py:493`, deixando `alimentar_fila` em `:533` intacta. C6 recupera `carga.agora` em `juizes.py:883`; portanto não reverte a cura pretendida.

FILA — item 11 não cobre literalmente a disputa inteira quando fechada: `fio.py:241` pré-filtra suas perguntas com `q_vivas`, `:255` descarta mortas antes de preencher o novo mapa em `:257`. Contraexemplo: disputa fechada com uma pergunta viva e outra validada: explicação recebe só a viva; o RED novo monta disputa aberta em `test_e3_leitores_fio_disputa.py:84`. Incidência de produção: [nao verificado].

#4 — cura da alimentação presente em `juizes.py:1575`; C6 recebe o instante, deriva seu dia local em `:885`, e evita o fallback de `:592`. Tarefa já chama C6 com seu instante em `tarefa_do_dia.py:493` e alimenta fila em `:533`. RED e controle em `test_e3_bloqueantes.py:110`/`:129`; verificação de repasse em `:135`. Reversão de `:1575` deve contrariar assert de `:145` por leitura estática; execução e mutante morto: [nao verificado].

#8 — REDs distinguem chamado sem ação, âncora silenciada e registrada em `test_e3_bloqueantes.py:148`, `:155`, `:164`; controle com porta em `:171` e mock da autoridade em `:184`. Reversão ao predicado cru contraria esses asserts por leitura estática; execução/mutante morto: [nao verificado].

#9 — RED vencido, controles vigente/NULL, instante recebido e fronteira exatamente igual em `test_e3_b4_silencio_vencido.py:41`, `:46`, `:50`, `:54`, `:59`. Reversão a NULL contraria o RED de `:43`; teste não exercita `tarefa_do_dia`, apenas os dois querysets (`:35`). Execução/mutante morto: [nao verificado]. [nao verificado]

Item 4 — REDs de folha, pauta e vigia distinguem o C17 anterior em `test_e3_leitores_c17_chamadores.py:124`, `:135`, `:154`, `:170`; controle de não chamada ao C17 em `:183`. Beco possui caso divergente em `test_f3_beco1_folga_na_lista.py:166`. Execução e mutantes por leitor: [nao verificado].

Item 6 — catraca verifica exclusivamente chamadas a C17 em `test_e3_pendentes_app.py:33`/`:145`; injeção demonstra detectar retorno da folha ao C17 em `:150`. Não certifica 0 derivações próprias nem 0 respondedores vivos; sua pergunta concreta é a contagem C17 (`:147`).

Item 11 — caso multicartão e universo completo aberto em `test_e3_leitores_fio_disputa.py:95`; regressão à lista do primeiro cartão contradiz asserts em `:103`/`:105`. Controle todas mortas em `:108`. Execução/mutante morto: [nao verificado].

Item 12 — caso morta sem resposta em `test_e3_leitores_fio_disputa.py:125`; controles viva sem resposta/com texto/sem texto em `:134`, `:138`, `:143`; API em `:156`/`:164`. Reversão ao timestamp cru contraria `:131` e `:146`. Execução/mutante morto: [nao verificado].

PRONTO 1–3/7–10 — o código documental ainda registra E3-LEITORES “1 de 10” em `RELATO.md:77`/`:95`; não há resultado da obra atual nesses trechos. Sonda 232, V/T/X, cura V, queda por classe, suite final, opus e pouso deste HEAD: [nao verificado]. [nao verificado]

## 12/11 11:39 · sessao 43ad31d715f2 · - · - · origem=Ronald (tmux)

Leitura concluída para `disputa_emissao` e encaixe no BACKLOG; testes abaixo foram **abertos, não executados** `[nao verificado]`. [nao verificado]

**Matriz de `disputa_emissao`**

| Entrada → juiz | Operação e persistência | Retorno e prova |
|---|---|---|
| Pergunta apontando ao chamado resolvido, sem `materializada_em`, disputa aberta (`app/chamados/services/disputa_emissao.py:1906`) → ignora pergunta em voo (`:1960`), exige C6 morta (`:1966`), ignora dia trancado (`:1970`); resposta real sem prova externa pertence ao materializador (`:1979`). | `p.carimbar(via_pelo_fato(...), user, resposta='validar', lavrar_veredito=True)` (`:1986`). Persiste trio `materializada_em/materializada_por/via_resolucao`; com texto, completa `respondida_em`, `validada_em/validada_por` nulos; lavra `veredito_lavrado/julgado_em/janela_expira_em` (`app/chamados/models.py:3129`, `:3131`, `:3150`). C6 é consultado **com carimbo e validação aplicados**; viva reverte memória e retorna falso (`:3140`). Não planta batida nesta operação (`app/chamados/services/disputa_emissao.py:1889`). | Conta `n_pq`, tenta fechar pelo C7 (`:1989`, `:2091`); retorna dicionário `perguntas_resolvidas/disputas_fechadas` (`:2094`). Prova: célula acusando impede carimbo (`app/chamados/tests/test_fio_le_o_juiz.py:79`); célula absolvendo permite (`:90`); com texto e carimbos nulos S128 grava resposta/validação (`app/chamados/tests/test_materializar_carimba_respondida.py:66`). |
| Disputa aberta do colaborador sem pai vivo e sem destinos vivos (`app/chamados/services/disputa_emissao.py:2027`, `:2033`, `:2036`); pergunta sem materialização/validação (`:2039`) → ignora em voo (`:2045`), exige C6 morta (`:2048`). | Via pelo fato do pai ou `chamado_encerrado` sem pai (`:2058`); `carimbar(... resposta='responder')` (`:2060`). Trio persistido; `respondida_em` apenas se texto, sem validar (`app/chamados/models.py:3129`, `:3131`). Trilha `fecho_sem_credor` (`app/chamados/services/disputa_emissao.py:2063`). | Fecha disputa pelo C7; o contador `perguntas_resolvidas` **não é incrementado neste segundo ramo**, apenas no primeiro (`:1989`, `:2060`, `:2094`). Prova: sem texto fecha com `chamado_encerrado`, sem resposta/validação (`app/chamados/tests/test_o261_resposta_vazia.py:78`); com texto, C6 viva após carimbo faz porta recusar e preserva estado (`:103`). |
| Chamado com `via_encerramento=competencia_trancada` → guarda própria (`app/chamados/services/disputa_emissao.py:1901`). | Nenhum carimbo; morte por leitura da tranca explicitada (`:1898`, `:1968`). | Retorna ambos contadores zero (`:1902`). Prova específica desse retorno não aberta `[nao verificado]`. |
| Resposta humana válida → valida tipo/opções (`app/chamados/services/disputa_emissao.py:1433`), sem exigência de lâmpada nesse escritor. | `resposta_colab/respondida_em`; limpa validação antiga, chama `reabrir_por_resposta`, salva atomicamente e grava `DecisaoPergunta.RESPOSTA` (`:1467`, `:1480`, `:1485`, `:1508`). Escapes ausência disparam materializador (`:1538`). `FOLGA_CONTESTA/OUTRO_MOTIVO` registram recusa, preservando fila admin (`:1486`). | `(True,None)` (`:1559`), erro `(False,texto)` nas guardas (`:1434`). **Respondida não é morte automaticamente**; prova específica resposta viva será a do agente de leitores. Reenvio igual move carimbo e é pergunta de lei já caracterizada (`app/chamados/tests/test_porta_resposta_pergunta.py:196`). |
| Foto entregue por outro caminho, pergunta sem texto → guarda só de texto (`app/chamados/services/disputa_emissao.py:1647`). | `responder_automaticamente`: texto+`respondida_em` e ato resposta (`:1652`). Caller delega validação ao dono (`app/api/foto_recebida.py:39`, `:48`, `:51`). | Booleano (`app/chamados/services/disputa_emissao.py:1658`). Prova idempotência e preservação voz (`app/chamados/tests/test_porta_chamado_e3.py:43`, `:50`). |
| Meio estado texto sem carimbo → guarda (`app/chamados/services/disputa_emissao.py:1670`). | `carimbar_resposta_pela_trilha`: `respondida_em=instante`, save e ato resposta (`:1674`). | Booleano (`:1682`). Prova instante/trilha e exclusões (`app/chamados/tests/test_curar_perguntas_em_limbo.py:29`, `:44`). |
| Reabertura para correção → queryset do juiz `perguntas_corrigiveis` recebido pelo escritor (`app/chamados/services/disputa_emissao.py:1582`). | Limpa `respondida_em/validada_em/validada_por/resposta_colab` (`:1593`); registra foto antes (`:1615`); `desfazer_absorcao=True` usa `descarimbar_em_lote`, caso contrário `qs.update(**campos)` (`:1629`). | Quantidade reaberta (`:1634`). Não mata; escritor de reversão necessário ao censo (`:1575`). Selo citado mas não aberto: `test_reabertura_fantasma.py` `[nao verificado]`. |

**Escolha de via em S128**

`via_pelo_fato` importa `_apur=batidas_apuraveis`, `_aus_cobre=ausencia_cobre`, `_TM=TurnoMaterializado` (`app/chamados/juizes.py:1730`, `:1745`, `:1749`). Resultado:

- Agrupado, sem batida apurável tocando dia civil: `cadastro_folga` se pai LIMBO-FOLGAS; caso contrário `adesao` (`:1713`, `:1733`, `:1738`).
- Ausência aprovada: `ausencia_dia` (`:1746`).
- TM com saída e motivo fora de intervalo: `completude_turno` (`:1754`).
- Restante: `chamado_encerrado` (`:1757`).

Essas são testemunhas diferentes de “marco específico aceso”; a via usa `data_evento` recebida (`:1705`). O carimbo ainda passa pelo C6 acima (`app/chamados/models.py:3140`).

**Silêncio**

A via vem de `DisputaSupervisao.fechar`, não de `disputa_emissao`: exceções `desligamento/admin_encerra` percorrem vivas e chamam `fechar_por_admin`; gravam `DecisaoPergunta.SILENCIO` e trilha com via `admin_decidiu` (`app/chamados/models.py:1919`, `:1922`, `:1925`, `:1931`). `admin_encerra` exige motivo (`:1944`); respostas sem veredito recusam todas as portas (`:1887`). Contrato de morte por identidade deve entrar na matriz do agente de escritores (`:1865`).

**Divergências para DECISÃO, sem escolher**

1. C7/C6 podem declarar morta e fechar **validada sem batida no slot**: teste mudou para esperar fechamento, com explicação literal dessa classe (`app/chamados/tests/test_fechamento_unificado.py:74`, `:84`, `:88`); `pode_fechar` consulta resposta sem veredito e `fio_vivo`, não prova de materialização (`app/chamados/models.py:1849`). Outra leitura é a lâmpada marco a marco do aval e `CLAUDE.md:169`. O teste é evidência de desenho atual, não defeito a corrigir por inferência.
2. `chamado_encerrado` pertence a `VIAS_SEM_LASTRO`, cujo comentário diz materializada mas não absolvida (`app/chamados/models.py:3766`), mas ramo sem credor pode persistir exatamente essa via e fechar disputa quando C6 morta (`app/chamados/tests/test_o261_resposta_vazia.py:78`). Leitor deve distinguir materialização administrativa, lastro e viva.
3. Silêncio encerrado mata por `admin_decidiu`, sem consulta de lâmpada no loop que carimba (`app/chamados/models.py:1919`); a regra literal fornecida só permite morte com marco aceso `[nao verificado]` quanto ao alcance que o aval pretende dar às exceções já nomeadas.
4. S128 granular pode fabricar `respondida_em/validada_em` a partir de texto preexistente, na mesma operação do carimbo (`app/chamados/models.py:3131`); o teste cobra os dois carimbos, sem resposta nova do colaborador (`app/chamados/tests/test_materializar_carimba_respondida.py:66`). Leitura como “resposta humana/validação” versus “normalização do encerramento” precisa ficar explícita.

**Aliases e chamadores relevantes**

Reexport compatível de `disputa_emissao` por `chamados.models` em `app/chamados/models.py:4093`–`:4094`, incluindo responder, reabrir e resolver. Web importa core por models (`app/chamados/views.py:1281`), API idem (`app/api/views.py:2619`); web importa `reabrir_para_correcao as _reabrir2` (`app/chamados/views.py:1435`), models como `_reabrir` (`app/chamados/models.py:2220`). S143 importa resolver por models (`app/chamados/signals.py:113`), assim como beco (`app/chamados/services/becos.py:40`) e reconciliador de fantasmas (`app/ponto/management/commands/reconciliar_fantasmas.py:41`). Não são escritores novos.

**Encaixe aberto no BACKLOG**

- Entradas/respostas/dia do fato: E1-ENTRADAS (`app/docs/BACKLOG.md:1859`); **ABSOLVIDA-PELA-FOLGA** é explicitamente complemento E1 para `data_evento` errado, com RED 51407 e 51467 (`:1935`).
- Autoridade tarefa/C6/tranca: E2 (`:1872`); leitores fio/tela/API/badge/o_que_segura/vigia: E3 (`:1873`).
- Escritores legados: E4-A01..A23; E4b A24/backfill C3; E4c leitores DIVERGE; E4d parar projeção (`:1885`).
- Exceções pós-E4d: A03 reabrir_admin, A05 assinatura, A06 identidade/duplicata, A17/A18 materialização técnica, A20 conferência (`:1887`–`:1892`). Mapeamento exato de cada função para Axx não verificado; `logs/matriz_e4` não foi aberto `[nao verificado]`.
- Reação raiz/filhos/fechamento e autor do fecho: RAIZ F1 matriz, F3/F4 par e zumbi (`:1904`, `:1905`, `:1911`); F5 junto E4d e F6/F7 apenas nomeados (`:1904`), detalhes `[nao verificado]`.
- Duplicatas: complemento E1/E4 depois E4d (`:1912`).
- Série não autoriza inferir qual escritor matou os 15: o dado raspado não contém campos de cada pergunta; classe causal por indivíduo `[nao verificado]`.

## 12/11 11:38 · sessao e4af61514a81 · - · - · origem=Ronald (tmux)

Leituras verificadas na cópia, sem execução de testes. [nao verificado]

- **C6 exato:** `_pergunta_viva_calc` mata por validação (`juizes.py:1391`), tranca lida (`:1395`), vias que vencem resposta com materialização (`:1402`), identidade com materialização (`:1428`), marco materializado aceso (`:1434`), célula não acusadora sem pendentes (`:1452`) e, na exceção do juiz de lastro, retorna `False` (`:1476`). Caminhos completos: `app/chamados/juizes.py:1389`. Resposta com texto vence até o admin decidir o dia ou `_resposta_concorda_com_a_luz` retornar verdadeiro (`:1405`–`:1419`).
- **SQL é aproximação:** `q_vivas()` declara contrato de continência, nunca igualdade com C6: `app/chamados/models.py:3337`; corpo SQL `:3348`–`:3363`. Propriedade `estado` declara validada/resolvida pelos carimbos antes de consultar C6: `app/chamados/models.py:3042`.

Cadeia das portas:

| Leitor | Fonte efetivamente lida |
|---|---|
| Painel/fila | `qs_na_fila` = `qs_vivos` + status COBRANÇA − silêncio futuro: `app/chamados/services/painel.py:188`. |
| Verbo do painel | `particionar_verbos` chama `creditos_perguntas`: `app/chamados/services/verbos.py:34`; crédito usa `q_vivas` e exclui materializada com `VL_SIM`: `app/chamados/services/raiox.py:74`, `:135`. Com resposta → validar, com pergunta → cobrar, sem ambos → decidir: `app/chamados/catalogo/natureza.py:41`, `:58`. |
| Alimentação C17 | Fila/verbos + perguntas efetivamente vivas por C6: `app/chamados/juizes.py:1566`, `:1571`–`:1575`. |
| C17 `porta_fila` | Chamado da célula na fila basta: `_fila` preenche `_verbos` antes de olhar perguntas, `app/chamados/juizes.py:1647`. |
| C17 `porta_pergunta` | C6 viva + âncora na fila + texto não vazio quando respondida + presente no app: `app/chamados/juizes.py:1649`. |
| `censo_alcance` | Aceita a porta de C17 e dá `continue`, sem conferir botão: `app/chamados/services/vigia_fio.py:635`, `:642`–`:647`. Consulta Resolver dia **somente quando C17 devolveu None**: `:636`. |
| `tarefa_do_dia` | Ações pela última `DecisaoPergunta`, luz vigente/efeito; validação sem efeito → materializar/sistema, silêncio → nenhuma ação: `app/chamados/services/tarefa_do_dia.py:116`, `:141`–`:151`. Só publica portas inteiras já exigidas pelas ações, intersectadas com fila: `:538`–`:554`; fila residual não cria tarefa. |
| Calendário | Usa C6 com apelido `_pv`, apenas perguntas de resposta não vazia, para barrar último recurso: `app/colaboradores/services/calendario.py:468`, `:472`–`:477`. Último recurso exige dia anterior ao início da competência atual: `:738`. Resolver exige último recurso + passado + status D4 em falta/irregular/aberto/sem_certidao: `:94`, `:97`–`:104`. Status vem D4: `:663`. |

**Onde fica decidir sem ação:** natureza transforma ausência de crédito em decidir (`app/chamados/catalogo/natureza.py:54`, `:61`); C17 chama isso porta_fila mesmo sem perguntas (`app/chamados/juizes.py:1647`); censo encerra a classificação nesse ramo (`app/chamados/services/vigia_fio.py:642`). Competência corrente impede último recurso (`app/colaboradores/services/calendario.py:738`); em competência encerrada, D4 fora dos quatro status também impede (`:94`, `:103`). Qual status específico do col427 21/09: **[nao verificado]**, dado raspado não contém.

**O admin vê:**

- Pilula Decidir com contagem: `app/templates/chamados/painel_gestao.html:77`.
- Na lista, ramo decidir mostra apenas o badge, enquanto validar/cobrar têm botões próprios: `app/templates/chamados/partials/_lista_chamados.html:81`–`:86`. A linha abre fio ao clicar: `:42`.
- Calendário mantém badge clicável do chamado: `app/templates/colaboradores/partials/_calendario_grade.html:108`. Resolver e DP do dia só existem no ramo `pode_veredito and resolver_dia_oferecido`: `:125`.
- Não é correto concluir por código que todo modal desses 15 fica sem botão. Em disputas abertas, `fio` carrega todas as perguntas; C6 só filtra as fechadas: `app/chamados/services/fio.py:221`, `:235`–`:239`. `todas_respondidas` usa `respondida_em` cru e vira True quando a lista está vazia: `:339`–`:345`. Com aberta e todas respondidas, modal oferece Regularizar disputa e fechar: `app/templates/chamados/partials/modal_fio.html:325`, `:352`–`:359`; fechada mostra “Disputa fechada”: `:376`. Modal exato dos 15: **[nao verificado]**.

Decisões sugeridas para consolidação, sem escolher:

1. **Porta-presença versus porta-ação:** C17 aceita fila sem pergunta (`juizes.py:1647`); tarefa exige ação antes da porta (`tarefa_do_dia.py:545`). Testes preservam ambas leituras: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138` espera porta_fila/decidir; `app/chamados/tests/test_e3_portas_tarefa.py:32` espera rota None/sistema/emissor para fila sem pergunta.
2. **“Viva” SQL versus C6:** crédito do painel usa SQL (`raiox.py:74`), apesar de contrato SQL≠C6 (`models.py:3337`); C17 usa C6 para existência da porta pela pergunta (`juizes.py:1573`). Pode alterar verbo/ação, sem afirmar ocorrência nos 15 **[nao verificado]**.
3. **Carimbo encerrador versus efeito:** C6 validada morre antes da luz (`juizes.py:1391`); tarefa validada sem efeito exige materializar (`tarefa_do_dia.py:145`). `estado` pode dizer resolvida (`models.py:3042`); modal imprime ✓ Validada (`modal_fio.html:228`).
4. **“Todas respondidas” versus pergunta viva/ação:** fio usa timestamp cru (`fio.py:340`); tarefa separa resposta vazia, silêncio, validação sem efeito (`tarefa_do_dia.py:133`). BACKLOG explicitamente inclui correção do ramo no complemento E3 caso7445/cartão35602/pergunta51407: `app/docs/BACKLOG.md:1873`.
5. **Decisão do dia D4 versus alcance:** calendário só oferece Resolver pelos quatro status (`calendario.py:94`), mas censo aceita decidir sem consultar esse resultado (`vigia_fio.py:642`). Sem status do col427, o veto concreto segue **[nao verificado]**.

Provas abertas, não rodadas:

- C6 validada, identidade, célula absolvida, admin: `app/chamados/tests/test_pergunta_viva.py:17`, `:31`, `:39`, `:53`.
- C6 tranca pendente/respondida: `app/chamados/tests/test_viva_delega_ao_juiz.py:138`, `:143`.
- Fila sem pergunta porta_fila: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`; teste puro equivalente `app/chamados/tests/test_e3_pendentes_app.py:160`.
- Tarefa sem pergunta não publica fila residual: `app/chamados/tests/test_e3_portas_tarefa.py:32`; isento com fila residual: `:57`.
- Resolver por D4, incluindo folga/ok recusados: `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:59`.
- Competência corrente sem Resolver: mesmo arquivo `:94`.
- Calendário lê C6 exato: `app/colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py:51`, `:61`.

Encaixe documental: E2 função `app/docs/BACKLOG.md:1872`; E3 migra fio/tela/admin/API/badge/vigia e inclui caso35602 `:1873`; E4b/E4c/E4d=F5 na ordem `:1761`. Não atribuí cura nova nem inferi estado de produção.

## 12/11 11:38 · sessao 131250998015 · - · - · origem=Ronald (tmux)

- Campos reais da pergunta: `respondida_em` e `validada_em` em `app/chamados/models.py:2449`; `validada_por_id` em `app/chamados/models.py:3378`; `materializada_em` e `materializada_por_id` em `app/chamados/models.py:3394` e `app/chamados/models.py:3398`; `via_resolucao` em `app/chamados/models.py:2562`.
- O carimbo grava autor, instante e via juntos em `app/chamados/models.py:3098`. Porém `carimbar_identidade(manter_carimbo=True)` preserva autor/instante anteriores ao trocar a via (`app/chamados/models.py:3183`), e `relavra_via` troca a via sem trocar aqueles campos (`app/chamados/services/relavra_via.py:123`). Portanto, rotular o campo como “autor do carimbo persistido”; autoria da via atual apenas pelo campo é `[nao verificado]`.
- Auditoria disponível: `LogAuditoria`, com `usuario_id`, `modelo`, `objeto_id`, `valor_antes`, `valor_depois`, `criado_em` (`app/core/models.py:285`). Query que não carrega descrição:

```python
logs = list(
    LogAuditoria.objects.filter(
        modelo="PerguntaDisputa", objeto_id=p.pk
    ).order_by("pk").values("pk", "usuario_id", "criado_em")
)
logs_via = list(
    LogAuditoria.objects.filter(
        modelo="PerguntaDisputa",
        objeto_id=p.pk,
        valor_depois__via_resolucao=p.via_resolucao,
    ).order_by("pk").values("pk", "usuario_id", "criado_em")
)
```

- A correlação por `valor_depois__via_resolucao` corresponde aos escritores de auditoria em `app/chamados/services/relavra_via.py:132` e `app/chamados/models.py:1928`. Apresentar contagem e PKs; se necessário, autoria por `usuario_id` e horário local. Não concluir que o último log correspondente prova autoria histórica completa: a auditoria é `best-effort` (`app/core/observ.py:94`) e pode faltar (`app/core/observ.py:139`).
- Existe também `DecisaoPergunta`, ligada por `pergunta_id`, com `ato`, `ato_em` e `ato_por_id`; não possui coluna `via_resolucao` (`app/chamados/models.py:3872`). Query sem texto:

```python
decisoes = list(
    DecisaoPergunta.objects.filter(pergunta_id=p.pk)
    .order_by("pk")
    .values("pk", "ato_por_id", "ato_em")
)
```

- Nunca projetar `LogAuditoria.descricao` (`app/core/models.py:299`), nem `DecisaoPergunta.ato_texto` ou `ato_motivo` (`app/chamados/models.py:3876`). A lista de decisões evidencia trilha da pergunta; ligação de qualquer decisão específica à via atual é `[nao verificado]`.

## 12/11 11:30 · sessao 1cbff62e7def · AUDITORIA-DE-TRAVAS-CORTE · verificador · origem=Code

REPROVADO

**1. O diff não realiza literalmente o item 2.** O aval diz “para só em DESENHO”, mas o disparo continua dependendo da quantidade de reprovações, sem consultar sua classe; inclusive a chamada que dispara retorna PARADA antes de examinar liberações. A classe aparece apenas na nova linha `AUTOLIBERA`. Referências: `aval_auditoria_de_travas_corte_1211.txt:4` do anexo; `bin/disjuntor.py:652`, `bin/disjuntor.py:1020`, `bin/disjuntor.py:1028`, `bin/disjuntor.py:763`.

A exclusividade da segunda ocorrência também não está garantida: a cota protege somente `tipo == 'auto'`. O Code pode liberar pela antiga `ERRO-BOBO`, sem cota nem prova de autoria Ronald; o próprio teste exige essa saída. Isso corresponde à preservação das portas antigas pelo contrato, mas diverge do aval literal. Referências: `contrato_item2.txt:2` do anexo; `bin/disjuntor.py:757`, `bin/disjuntor.py:1044`, `bin/disjuntor.py:1049`, `bin/tests/test_disjuntor.sh:1213`.

Há outra divergência textual: o contrato pede “só 1 FALHA por sha recusado”; implementação e teste usam **SOBE**. Referências: `contrato_item2.txt:4` do anexo; `bin/disjuntor.py:1047`, `bin/tests/test_disjuntor.sh:1177`. As linhas dos anexos foram contadas a partir do primeiro conteúdo de cada anexo.

**2. Escritores e leitores.**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `est['b3'][obra]['auto']` | `decide_b3` acrescenta SHA na liberação automática; `_com_estado` persiste o JSON. `bin/disjuntor.py:1051`, `bin/disjuntor.py:379`. | Guarda da cota; testes da primeira liberação e sobrevivência ao rearme. `bin/disjuntor.py:1044`, `bin/tests/test_disjuntor.sh:1170`, `bin/tests/test_disjuntor.sh:1180`. |
| `auto_sobe` | `decide_b3` acrescenta SHA recusado. `bin/disjuntor.py:1046`. | Deduplicação do aviso. `bin/disjuntor.py:1045`. |
| `lib['auto']` / `b3_libera['auto']` | `ler()` inicializa e conta eventos `LIBERA (auto)`, na janela de 24 horas. `bin/disjuntor.py:1996`, `bin/disjuntor.py:2000`, `bin/disjuntor.py:2025`. | `linha()` formata; `gerar_estado.py` usa `ler()/linha()`; teste antigo consulta o resultado. `bin/disjuntor.py:2285`, `bin/disjuntor.py:2289`, `bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`, `bin/tests/test_disjuntor.sh:430`. |
| Evento `SOBE` | `decide_b3` prepara; `_anuncia` publica após persistência, através de `trilha()`. `bin/disjuntor.py:1047`, `bin/disjuntor.py:1064`, `bin/disjuntor.py:1067`, `bin/disjuntor.py:205`. | Leitor específico encontrado: teste `conta()`. O ESTADO não enumera `SOBE`; o leitor de censo também o descarta. `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1177`, `bin/disjuntor.py:2002`, `bin/disjuntor.py:2115`. |
| SHA consumido | `_sha_b3()` normaliza/calcula; `decide_b3` grava em `consumidas`. `bin/disjuntor.py:677`, `bin/disjuntor.py:1039`, `bin/disjuntor.py:1055`. | Guarda de reutilização lê `consumidas` e `topo_no_disparo`. A fotografia produz os SHA anteriores ao disparo. `bin/disjuntor.py:1040`, `bin/disjuntor.py:832`. |
| `B3_AUTO_COTA` | Constante `1`. `bin/disjuntor.py:147`. | Guarda, texto de `SOBE` e mensagem do B3. `bin/disjuntor.py:1044`, `bin/disjuntor.py:1048`, `bin/disjuntor.py:1176`. |

Os consumidores decisórios fora do diff convergem na mesma decisão, podendo provocar a gravação da liberação:

- **Stop:** `hook_stop_fila1.py` chama `julga_b3`, que chama `decide_b3`; recebe paradas e mensagens. `bin/hook_stop_fila1.py:1204`, `bin/hook_stop_fila1.py:1205`, `bin/disjuntor.py:1096`.
- **PreToolUse:** `_nega_b3 → estado_b3 → julga_b3 → decide_b3`. `bin/disjuntor.py:1801`, `bin/disjuntor.py:1105`, `bin/disjuntor.py:1096`.
- **Wrapper Codex:** `--b3-parada → b3_parada → estado_b3`; recusa retorno diferente de sucesso. `bin/codex_pergunta.sh:378`, `bin/disjuntor.py:2383`, `bin/disjuntor.py:1158`.
- **ESTADO:** recebe a contagem de eventos, incorpora `dj_txt` e grava a página; não lê a cota persistida como medida. `bin/gerar_estado.py:281`, `bin/gerar_estado.py:378`, `bin/gerar_estado.py:390`, `bin/disjuntor.py:2025`.

Há ainda leitores genéricos do JSON: `_com_estado` e `ler_estado`; os consumidores deste último selecionam B1, B4 e históricos, sem interpretar `auto`. A referência do hook ao arquivo serve para rejeitá-lo como prova de espera. Referências: `bin/disjuntor.py:367`, `bin/disjuntor.py:401`, `bin/disjuntor.py:596`, `bin/disjuntor.py:1882`, `bin/disjuntor.py:2213`, `bin/hook_stop_fila1.py:664`.

**3. Caminhos e contraexemplos.**

- **Segunda AUTOLIBERA, sozinha:** permanece PARADA, registra `auto_sobe` e não consome seu SHA. A recusa dá `continue` antes da liberação e do consumo. `bin/disjuntor.py:1044`, `bin/disjuntor.py:1046`, `bin/disjuntor.py:1049`, `bin/disjuntor.py:1055`.
- **Segunda ocorrência liberada:** após gastar a primeira AUTO, disparar novamente e escrever no topo:
  ```
  AUTOLIBERA OBRA-A: classe=ERRO-BOBO segundo erro
  ERRO-BOBO OBRA-A: segundo erro escrito pelo Code
  ```
  A primeira linha gera `SOBE`; a segunda libera na mesma decisão, sem autenticar Ronald. Esse caminho está expressamente exercitado pelo teste. `bin/disjuntor.py:757`, `bin/disjuntor.py:1047`, `bin/disjuntor.py:1049`, `bin/disjuntor.py:1056`, `bin/tests/test_disjuntor.sh:1214`.
- **`classe=DESENHO`:** não entra pela porta nova. Porém `classe=FORMA erro de desenho` satisfaz o reconhecedor: há validação do rótulo, não da natureza real do defeito. `bin/disjuntor.py:146`, `bin/disjuntor.py:763`. Classificação correta dos defeitos em produção: `[nao verificado]`.
- **Rearme/reinício normal:** não zeram a cota; rearme preserva `auto`, e cada transição relê o estado persistido. Se o estado estiver ausente, a inicialização usa `{}` e a ausência de `auto` vale zero; isso é perda/troca de estado, não efeito do reinício normal. `bin/disjuntor.py:1134`, `bin/disjuntor.py:1136`, `bin/disjuntor.py:367`, `bin/disjuntor.py:368`, `bin/disjuntor.py:1044`. Ocorrência dessa perda em produção: `[nao verificado]`.

**4. As linhas antigas mudam em alguma composição.** Seus reconhecedores individuais continuam iguais, mas uma AUTO válida anterior ganha a seleção e encerra o loop. Com AUTO seguida de `ERRO-BOBO`, passa a consumir o SHA da AUTO, gastar cota e registrar `LIBERA (auto)`; antes, a candidata seria `ERRO-BOBO`, com outro SHA e outra contagem. Referências: `bin/disjuntor.py:746`, `bin/disjuntor.py:757`, `bin/disjuntor.py:1038`, `bin/disjuntor.py:1051`, `bin/disjuntor.py:1055`, `bin/disjuntor.py:1059`.

Com cota esgotada, a antiga abaixo continua liberando, conforme o controle `c5`. `bin/disjuntor.py:1049`, `bin/tests/test_disjuntor.sh:1213`, `bin/tests/test_disjuntor.sh:1215`.

**5. Os três mutantes declarados têm mordida identificável.**

| Mutante | Verificação que o rejeita |
|---|---|
| Libera a segunda | `c2` exige duas recusas, um SOBE e recusa após rearme. `bin/tests/test_disjuntor.sh:1942`, `bin/tests/test_disjuntor.sh:1186`. |
| Aceita DESENHO | `c3` exige PARADA e reconhecimento vazio. `bin/tests/test_disjuntor.sh:1943`, `bin/tests/test_disjuntor.sh:1195`. |
| Aceita sem classe | A alteração também aceita DESENHO, atingindo `c3`; os inválidos atingem `c4`. `bin/tests/test_disjuntor.sh:1944`, `bin/tests/test_disjuntor.sh:1195`, `bin/tests/test_disjuntor.sh:1206`. |

O harness confere âncora única e inclui controle da cópia sem mutação. `bin/tests/test_disjuntor.sh:1626`, `bin/tests/test_disjuntor.sh:1630`, `bin/tests/test_disjuntor.sh:1652`.

Não há `all([])` nesses controles: `rs` recebe quatro resultados e `ctrl`, três. Entretanto, **a medição positiva fica sem prova**: o teste de `ler()` exige apenas `auto=0`, enquanto `b3_auto` conta a trilha diretamente. Retirar `auto` da regex do medidor mantém esse assert satisfeito. Referências: `bin/tests/test_disjuntor.sh:1201`, `bin/tests/test_disjuntor.sh:1204`, `bin/tests/test_disjuntor.sh:1210`, `bin/tests/test_disjuntor.sh:1215`, `bin/tests/test_disjuntor.sh:431`, `bin/tests/test_disjuntor.sh:1156`, `bin/disjuntor.py:2023`.

O controle da porta antiga também ignora o retorno de `dispara()` e verifica apenas ausência no mapa, sem exigir `LIBERA (erro-bobo)`. É uma fragilidade; A já deveria estar PARADA pelo caso anterior. `bin/tests/test_disjuntor.sh:1186`, `bin/tests/test_disjuntor.sh:1213`, `bin/tests/test_disjuntor.sh:1215`. Execução integral do selo com arquivos e travas reais: `[nao verificado]`.

**6. Classes de reprovação.**

| Classe | Resultado |
|---|---|
| Segundo escritor fora da porta | Não identificado para `auto`: a atribuição específica está em `decide_b3`, persistida pela porta comum. `bin/disjuntor.py:1051`, `bin/disjuntor.py:1075`. |
| Relógio fora da borda | Há leitura interna herdada: `decide_b3` usa `time.time()` quando falta `agora`, como ocorre na via `b3_parada`; `trilha()` usa `datetime.now()`. A cota nova não depende de relógio. `bin/disjuntor.py:988`, `bin/disjuntor.py:1158`, `bin/disjuntor.py:206`, `bin/disjuntor.py:1044`. |
| Selo sem mordida | Os três declarados mordem; falta testemunha positiva do contador `auto` no ESTADO. `bin/tests/test_disjuntor.sh:1186`, `bin/tests/test_disjuntor.sh:1195`, `bin/tests/test_disjuntor.sh:1206`, `bin/tests/test_disjuntor.sh:431`. |
| Campo com dois significados | Existem dois `auto` distintos: lista cumulativa por obra e contador de eventos em 24 horas. Não são a mesma célula; zero no painel não significa cota disponível. `bin/disjuntor.py:1051`, `bin/disjuntor.py:1996`, `bin/disjuntor.py:2025`. |
| Ordem de trava | Sem inversão acrescentada: decisão e persistência ficam sob `LOCK_EX`, com anúncio ainda sob a trava. Wrapper e triagem adquirem resposta antes de estado. `bin/disjuntor.py:359`, `bin/disjuntor.py:374`, `bin/disjuntor.py:390`, `bin/codex_pergunta.sh:376`, `bin/codex_pergunta.sh:378`, `bin/disjuntor.py:897`, `bin/disjuntor.py:929`. |

“Sobe ao Ronald” produz uma linha de trilha que o ESTADO não trata como evento próprio; entrega ou notificação efetiva ao Ronald permanece `[nao verificado]`. `bin/disjuntor.py:1047`, `bin/disjuntor.py:2002`.

## 12/11 11:28 · sessao 23a31f2a2d8d · - · - · origem=Ronald (tmux)

Achados, todos limitados à cópia:

- **Divergência literal do aval:** B3 continua disparando por quantidade de reprovações, sem consultar classe: `n >= B3_REPROVACOES` ou `nc >= B3_CODEX_VOLTAS`, marcando PARADA e dando `continue` (`bin/disjuntor.py:1018`, `bin/disjuntor.py:1020`, `bin/disjuntor.py:1028`). A leitura de veredito recolhe obra e APROVADO/REPROVADO, não FORMA/DESENHO (`bin/disjuntor.py:652`, `bin/disjuntor.py:669`). Portanto “para só em DESENHO” não foi implementado literalmente.

- **Segunda ocorrência pode liberar pela porta antiga:** cota protege exclusivamente `tipo == 'auto'`; esgotada, dá `continue` (`bin/disjuntor.py:1044`, `bin/disjuntor.py:1049`). `ERRO-BOBO` antigo não exige autoria Ronald nem cota (`bin/disjuntor.py:757`); entra na liberação genérica (`bin/disjuntor.py:1052`, `bin/disjuntor.py:1058`). O próprio selo exige esse comportamento (`bin/tests/test_disjuntor.sh:1213`, `bin/tests/test_disjuntor.sh:1217`).

  Reproduzi exclusivamente em memória, com IO substituído: primeira AUTO de OBRA-A libera e grava um SHA; nova reprovação, topo contendo AUTO classe=ERRO-BOBO e `ERRO-BOBO OBRA-A: erro 2 escrito pelo Code`, retorna livre e emite **SOBE + LIBERA(erro-bobo)** na mesma decisão. Isso contraria a exclusividade “2a sobe a mim”, mas corresponde ao contrato que preserva a porta antiga (`bin/disjuntor.py:1047`, `bin/disjuntor.py:1056`).

- **DESENHO na nova porta:** `classe=DESENHO` é recusada pelo conjunto fechado FORMA/ERRO-BOBO (`bin/disjuntor.py:146`, `bin/disjuntor.py:763`). Porém só se verifica o rótulo: `classe=FORMA erro de desenho` é reconhecida como auto; classe real do defeito não é auditada (`bin/disjuntor.py:762`, `bin/disjuntor.py:764`). Confirmei ambos os resultados em memória. A ocorrência desse uso em produção é `[nao verificado]`.

- **Rearme/reinício:** rearme altera rearmado, reprovados, codex e disparado; não altera auto (`bin/disjuntor.py:1134`, `bin/disjuntor.py:1136`). Reinício normal relê JSON persistido (`bin/disjuntor.py:366`). Estado ausente vira `{}` e auto ausente equivale a zero (`bin/disjuntor.py:368`, `bin/disjuntor.py:1044`); isso depende de perda/troca/remoção de estado, não é reset causado por reinício normal. Ocorrência real de perda: `[nao verificado]`.

- **Linhas antigas têm mudança observável em combinação com a nova:** reconhecedores DESENHO/ERRO-BOBO estão intactos (`bin/disjuntor.py:746`, `bin/disjuntor.py:757`), mas AUTO válida anterior ganha a seleção e encerra o loop (`bin/disjuntor.py:1038`, `bin/disjuntor.py:1059`). Confirmado em memória:
  - topo `AUTOLIBERA OBRA-A: classe=FORMA ajuste` seguido de `ERRO-BOBO OBRA-A: ajuste`: código novo consome SHA AUTO, gasta cota e registra LIBERA(auto);
  - simulação da base filtrando candidatos AUTO: consome SHA ERRO-BOBO, não gasta cota e registra LIBERA(erro-bobo).
  
  Portanto “comportamento das antigas não mudou em nenhum caso” seria incorreto: consumo, contador e cota mudam nessa combinação (`bin/disjuntor.py:1051`, `bin/disjuntor.py:1055`, `bin/disjuntor.py:1056`).

- **Travas:** leitura, incremento da cota e persistência permanecem sob a mesma LOCK_EX (`bin/disjuntor.py:359`, `bin/disjuntor.py:374`, `bin/disjuntor.py:379`); anúncio é executado ainda sob essa trava (`bin/disjuntor.py:390`, `bin/disjuntor.py:1064`). O diff não acrescenta outra trava nem inversão de ordem nessas operações (`bin/disjuntor.py:1044`, `bin/disjuntor.py:1051`).

- **Relógio:** novo gate usa comprimento da lista e constante, sem leitura temporal (`bin/disjuntor.py:1044`). Existe fallback herdado `time.time()` dentro de `decide_b3`, antes da aquisição da trava (`bin/disjuntor.py:988`), utilizado pela via `b3_parada`, que não passa `agora` (`bin/disjuntor.py:1158`). `trilha` também lê `datetime.now()` (`bin/disjuntor.py:206`). Não há contraexemplo temporal novo encontrado; fotografia usa máximo do relógio recebido e dos mtimes observados (`bin/disjuntor.py:828`, `bin/disjuntor.py:841`).

- **Contrato, evento recusado:** contrato fornecido exige “só 1 FALHA por sha recusado”; implementação usa **SOBE**, deduplicado em auto_sobe (`bin/disjuntor.py:1045`, `bin/disjuntor.py:1047`), e teste também espera SOBE (`bin/tests/test_disjuntor.sh:1177`). Leitor `ler()` não enumera SOBE (`bin/disjuntor.py:2002`). Entrega/notificação efetiva ao Ronald é `[nao verificado]`.

## 12/11 11:28 · sessao 467d3fa641a2 · - · - · origem=Ronald (tmux)

O caso `b3_auto` morde os três mutantes declarados; há lacunas de cobertura fora deles (`bin/tests/test_disjuntor.sh:1942`, `bin/tests/test_disjuntor.sh:1943`, `bin/tests/test_disjuntor.sh:1944`).

| Mutação | Resultado em memória | Assert que morde |
|---|---|---|
| Libera a 2ª | saída 2; falha `[2]` | `bin/tests/test_disjuntor.sh:1186` |
| Aceita DESENHO | saída 3; falha `[3]` | `bin/tests/test_disjuntor.sh:1195` |
| Aceita sem classe | saída 3; falhas `[3,4]` | `bin/tests/test_disjuntor.sh:1195`, `bin/tests/test_disjuntor.sh:1206` |
| `continue` vira `break` | saída 5; falha `[5]` | `bin/tests/test_disjuntor.sh:1215` |
| SOBE duplicado | saída 2; falha `[2]` | `bin/tests/test_disjuntor.sh:1177`, `bin/tests/test_disjuntor.sh:1186` |
| Remove gravação de `consumidas` | passa | ausência de conferência desse campo em `bin/tests/test_disjuntor.sh:1170`, `bin/tests/test_disjuntor.sh:1186`, `bin/tests/test_disjuntor.sh:1217` |
| Retira `auto` da regex de `ler()` | passa | `b3_auto` conta diretamente a trilha, sem chamar `ler()`: `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1170`; alteração em `bin/disjuntor.py:2023` |

O teste antigo de medição exige somente `auto: 0`: contador permanentemente zero satisfaz esse assert. Falta testemunha positiva `ler()['b3_libera']['auto'] > 0` (`bin/tests/test_disjuntor.sh:430`, `bin/tests/test_disjuntor.sh:431`).

Não há `all([])` na sequência normal: `rs` recebe quatro resultados e `ctrl` recebe três (`bin/tests/test_disjuntor.sh:1201`, `bin/tests/test_disjuntor.sh:1204`, `bin/tests/test_disjuntor.sh:1210`, `bin/tests/test_disjuntor.sh:1215`). O controle da porta antiga ignora o retorno de `dispara()` e exige apenas ausência no mapa, sem comprovar `LIBERA (erro-bobo)` ou estado persistido; porém A já ficou parada ao final de c2, portanto isso é fragilidade, não falso verde vazio demonstrado na sequência normal (`bin/tests/test_disjuntor.sh:1186`, `bin/tests/test_disjuntor.sh:1213`, `bin/tests/test_disjuntor.sh:1215`).

O harness verifica âncora única e executa controle da cópia sem mutação, evitando queda apenas por árvore incompleta (`bin/tests/test_disjuntor.sh:1623`, `bin/tests/test_disjuntor.sh:1626`, `bin/tests/test_disjuntor.sh:1630`, `bin/tests/test_disjuntor.sh:1652`).

Execução realizada com Python `-B`, fontes compiladas em strings, corpo original de `b3_auto` e substituições de IO/estado/relógio/triagem em memória; integração real de arquivos e travas, execução integral da suíte e resultados de produção: `[nao verificado]`. [nao verificado]

## 12/11 11:27 · sessao fc614e3aba36 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**Não fecharia a c14:** há perda da data na guarda de órfãs, a prova integrada de (d) permanece incompleta e o selo sem célula enfraquece “nunca turno aberto”. (`app/relatorios/pdf_espelho.py:476`, `:512`; `app/ponto/tests/test_lastro_c14_sem_banco.py:295`; `app/relatorios/tests/test_cartao_marca_espuria.py:136`)

1. **(a) PDF — bloqueia: HH:MM de outro dia suprime turno legítimo.**

   A grade própria saiu: órfãs vêm de `orfas_grade`, e a recusa recebida permanece até o compositor comum. Sem achado de outra fonte para **essa classificação de batida/recusa**. (`app/relatorios/pdf_espelho.py:475`, `:561`, `:594`)

   Contudo, `_orfas_hhmm` apaga a data e `_entrada_espuria` compara somente HH:MM. **Contraexemplo sintético:** órfã `07:02` em **10/11**; entrada legítima aberta `07:02` em **11/11**, cujo `orfas_grade=[]`. O resumo exclui o turno de 11/11, mas `batida.orfa=False` nessa linha, que continua imprimindo “turno aberto”. No período livre, a troca pelos totais da competência não corrige esse contador. (`app/relatorios/pdf_espelho.py:178`, `:476`, `:512`, `:565`, `:920`, `:927`; `app/relatorios/cartao_pela_celula.py:322`)

   Portanto, **muda o resultado do código real**; um dia efetivamente atingido em produção não foi demonstrado pelo dado fornecido: **[nao verificado]**.

   Sobra uma leitura literal de outra espécie de órfã: `datas_saida_orfa(_turnos_juiz, ...)`. Ela identifica **saída sem entrada**, não `ata.orfas`; a tela consulta o mesmo juiz. **Sem achado de segundo juiz nesse ponto**, embora o PDF ainda não leia esse campo do retorno da tela. (`app/relatorios/pdf_espelho.py:578`; `app/ponto/turnos.py:234`; `app/ponto/services/espelho.py:622`)

2. **(b) Três sítios — sem achado de perda do canal corrigido.**

   O mapa nasce antes da seleção, entra em `_pela_grade`, atende pré-apuração e `AtaAgregadaSemMarcos` e sobrevive à composição posterior. A2 escreve a recusa por data e deixa esse dia sem conclusão de precedência; o overlay transporta o rótulo com `tipo_dia=None`. (`app/ponto/services/espelho.py:486`, `:498`, `:536`, `:565`, `:655`, `:881`; `app/ponto/precedencia.py:423`, `:425`; `app/escala/utils.py:1218`)

   **Nenhum `except` novo que engole no delta.** Permanecem os genéricos antigos; `TipoSemFaixa` fora do transporte por data é explicitamente relançado. Os testes exercitam seleção e handlers reais, mas não um request HTTP inteiro. Ausência de qualquer outro caminho de 500: **[nao verificado]**. (`app/ponto/services/espelho.py:574`, `:657`, `:660`; `app/ponto/tests/test_lastro_c14_sem_banco.py:113`, `:124`)

   **Fila — visibilidade na UI:** ausência `fantasma_xyz` com batida ou status anteriormente lavrado pode conservar título genérico/antigo. D4 retorna antes das recusas e o template lê `status_rotulo`. Isso não invalida o transporte de `palavra_dia`, mas impede afirmar que o aviso aparece sempre na faixa. (`app/ponto/status_dia.py:290`, `:295`, `:317`; `app/templates/ponto/espelho.html:151`; `app/ponto/tests/test_lastro_c14_sem_banco.py:169`)

3. **QUEM ESCREVE / QUEM LÊ.**

   | Campo | Escritor | Leitor |
   |---|---|---|
   | `recusas[data]` | A2 grava `TipoSemFaixa`, cujo construtor fornece código/rótulo. (`app/ponto/precedencia.py:99`, `:423`) | Overlay, espelho e D4. (`app/escala/utils.py:1218`; `app/ponto/services/espelho.py:881`; `app/ponto/status_dia.py:317`) |
   | `_recusa_do_dia` | Espelho transporta o rótulo. (`app/ponto/services/espelho.py:881`) | Compositor escreve palavra/veredito/cor; último leitor remove o transporte. PDF chama esse compositor. (`app/ponto/services/espelho.py:339`, `:350`; `app/relatorios/pdf_espelho.py:594`) |
   | `orfas_grade` | Cartório compõe `ata.orfas`; leitor transporta; adaptador devolve; espelho publica. (`app/ponto/services/cartorio.py:736`; `app/escala/services/leitor_celula.py:420`; `app/escala/utils.py:1113`; `app/ponto/services/espelho.py:854`) | HTML e mapa do PDF. (`app/templates/ponto/espelho.html:240`; `app/relatorios/pdf_espelho.py:475`) |
   | `palavra_dia` | Compositor comum, inclusive ramo de recusa. (`app/ponto/services/espelho.py:340`, `:347`) | Desenho do PDF. A faixa HTML lê outro campo. (`app/relatorios/pdf_espelho.py:879`; `app/templates/ponto/espelho.html:151`) |
   | `batida.orfa` | Coletor PDF, em memória, por comparação HH:MM na linha. (`app/relatorios/pdf_espelho.py:565`) | Separação visual e supressão de alertas. (`app/relatorios/pdf_espelho.py:832`, `:916`, `:920`) |

   **Bloqueia — ESMERIL-SEMÂNTICA:** uma hora órfã de determinado dia passa a significar “qualquer entrada desse horário na janela”. É a perda demonstrada no item 1. A própria classificação visual ainda usa o mapa por data, criando duas respostas no mesmo documento. (`app/relatorios/pdf_espelho.py:476`, `:512`, `:563`)

   **Fila — desconhecido versus negativo:** sem célula, `orfas_grade=[]` termina em `batida.orfa=False`; isso significa “sem marca disponível”, não prova de batida legitimamente casada. Atas antigas também podem transportar `'?'`. O campo deve ser entendido como marca de apresentação. (`app/escala/services/leitor_celula.py:190`, `:394`; `app/escala/utils.py:1087`; `app/relatorios/pdf_espelho.py:565`)

   **Sem achado de mudança do significado de `data` no delta:** a tela agrupa pelo juiz `dia_das_batidas`, que distingue dia do turno e dia civil da batida não reclamada. O defeito identificado é eliminar essa chave posteriormente. (`app/ponto/turnos.py:2886`; `app/ponto/services/espelho.py:580`; `app/relatorios/pdf_espelho.py:476`)

4. **(c)/(d) Mordida das provas.**

   **(c), sem achado de núcleo novo puramente congelado:** o carregador padrão lê e compila os fontes da cópia; mutantes alteram AST produtiva do canal, reset, classificação e metadados. Há mordida sobre essas operações. (`app/ponto/tests/test_lastro_c5_sem_banco.py:39`, `:108`; `app/ponto/tests/test_lastro_c14_sem_banco.py:35`, `:45`; `app/ponto/tests/test_lastro_c13_sem_banco.py:46`)

   **Fila — alcance parcial:** F1c/cartão continuam contendo selos textuais. A prova dos canais recorta as chamadas e substitui o restante dos builders. **Contraexemplo:** quebrar a geometria antes do repasse pode escapar desse ensaio, embora retirar `recusas` seja detectável. (`app/escala/tests/test_f1c_contrato.py:68`; `app/relatorios/tests/test_cartao_pela_celula.py:170`; `app/ponto/tests/test_lastro_c13_sem_banco.py:194`, `:198`)

   REDs históricos dependem de `git show`; sua reexecução histórica nesta cópia e as saídas dos arquivos de evidência mencionados no relato: **[nao verificado]**. (`app/ponto/tests/test_lastro_c13_sem_banco.py:26`; `app/ponto/tests/test_lastro_c14_sem_banco.py:29`)

   **(d), há transporte real em memória:** builder → `ata_do_dia` → porta `save=False` → leitor → mapas/trecho do fechamento. Não é apenas um duplo; verifica classificação, metadados e limpeza das órfãs. (`app/ponto/tests/test_lastro_c14_sem_banco.py:204`, `:216`, `:235`, `:255`, `:269`)

   **Bloqueia a comprovação integrada de (d):** o simulador recebe `Mock(return_value={17: fech})`. **Contraexemplo:** o recalculador produtivo parar de ler a ata nova e retornar `{}` não reprovaria esse teste. A ata antiga também é sintetizada da nova, mudando apenas `tipo_dia`. Portanto, prova transporte real parcial, mas não a sequência integrada ausência → relavra → recalculador → simulador. (`app/ponto/tests/test_lastro_c14_sem_banco.py:218`, `:295`, `:301`; `app/folha/management/commands/simular_folha.py:130`)

5. **Retarget — preserva o positivo com ata; enfraquece o universo original sem célula.**

   Com ata, continuam marca positiva e zero turno aberto. **Sem célula, o mesmo cenário de 10/06 às 16:19 agora exige ausência de marca e um turno aberto.** Assim, “nunca turno aberto” não foi preservado integralmente. O precedente do calendário documenta perda do asterisco, não essa garantia adicional. **Bloqueia declarar a pergunta preservada.** (`app/relatorios/tests/test_cartao_marca_espuria.py:55`, `:81`, `:97`, `:122`, `:128`, `:136`; `app/colaboradores/tests/test_calendario_orfa.py:18`)

   **É como a tela no mesmo dia?** A tela conta períodos abertos sem a guarda de espúria; com uma única entrada aberta e órfãs vazias, ambos podem contar 1. Porém o novo selo não chama a tela nem compara a data: a igualdade integrada nessa fixture permanece **[nao verificado]**. (`app/ponto/services/espelho.py:436`; `app/relatorios/pdf_espelho.py:512`; `app/relatorios/tests/test_cartao_marca_espuria.py:135`)

   **Fila — mordida visual:** manter `orfa=True` e `16:19` no texto, mas desenhar a batida junto das comuns, escaparia dos asserts. A implementação atual separa e acrescenta asterisco; o selo não prova toda essa disposição. A classe sem célula ainda substitui a emissão do PDF por checagem do resumo. (`app/relatorios/tests/test_cartao_marca_espuria.py:81`, `:109`, `:131`; `app/relatorios/pdf_espelho.py:832`, `:841`)

6. **Demais classes de reprovação.**

   **Sem achado de novo escritor produtivo fora da porta ou nova ordem de trava:** o retarget escreve fixtures; a prova de relavra chama a porta real sem persistência. A afirmação “ata que o cartório lavraria” não é demonstrada pela fixture pronta. (`app/relatorios/tests/test_cartao_marca_espuria.py:66`; `app/ponto/tests/test_lastro_c14_sem_banco.py:235`; `app/ponto/portas/celula.py:1032`)

   **Sem achado de faixa inventada/default novo:** `faixa_de` recusa código desconhecido; o transporte mantém `tipo_dia=None`. Derivação paralela, campo ambíguo e selo sem mordida estão discriminados acima. (`app/ponto/precedencia.py:105`; `app/escala/utils.py:1220`)

   **Fila herdada — relógio fora da borda:** o coletor recebe `agora`, mas `folha_manda` consulta turno vivo e teto sem repassá-lo; estes podem ler o relógio corrente. **Contraexemplo:** ato simulado antes da meia-noite executado depois dela pode usar outro teto/dia vivo. Não nasceu na c14; ocorrência concreta em produção: **[nao verificado]**. (`app/relatorios/pdf_espelho.py:584`; `app/relatorios/cartao_pela_celula.py:398`, `:412`, `:40`; `app/ponto/turnos.py:2291`)

## 12/11 11:27 · sessao 38008ed6103d · - · - · origem=Ronald (tmux)

Mapa dos campos e dos caminhos de produção:

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `est['b3'][obra]['auto']` | `decide_b3`, acrescenta SHA somente ao aceitar `tipo == 'auto'`: `bin/disjuntor.py:1050-1051`; persistência por `_com_estado`: `bin/disjuntor.py:374-380`. | Cota em `decide_b3`: `bin/disjuntor.py:1044`. Teste lê estado em `bin/tests/test_disjuntor.sh:1162-1170` e `:1180`. |
| `auto_sobe` | `decide_b3`, acrescenta SHA recusado: `bin/disjuntor.py:1046`. | Deduplicação do aviso: `bin/disjuntor.py:1045`. |
| `lib['auto']` | `ler` inicializa contador em zero e soma eventos `LIBERA` com `(auto)`: `bin/disjuntor.py:2000`, `:2022-2025`. | Exportado em `b3_libera`: `bin/disjuntor.py:2068`; `linha` lê e imprime: `:2285-2289`. `bin/gerar_estado.py:280-281` chama `ler`/`linha`, incorpora resultado em `:378` e grava página em `:390`. Teste lê em `bin/tests/test_disjuntor.sh:430-431`. |
| Evento `SOBE` | Produzido ao recusar AUTOLIBERA excedente: `bin/disjuntor.py:1047-1048`; publicado somente depois da gravação do estado: `:1064-1067`. | Leitor explícito encontrado é o teste: `bin/tests/test_disjuntor.sh:1156-1161`, `:1177`. O medidor de ESTADO enumera eventos sem `SOBE`: `bin/disjuntor.py:2002-2003`; o outro leitor da trilha aceita somente regex de censo: `:2072`, `:2113-2117`. Assim, `SOBE` não aparece como contador/encaminhamento no ESTADO por esse código. |
| SHA consumido (`consumidas`) | Criado com `_sha_b3` em `bin/disjuntor.py:1039`; acrescentado a `consumidas` em `:1055`. SHA da fotografia também é produzido em `:832`. | Aceitação testa `consumidas` e `topo_no_disparo`: `:1040`; recusa excedente usa `continue` antes do consumo: `:1049`. |
| `B3_AUTO_COTA` | Constante igual a 1: `bin/disjuntor.py:147`. | Guarda em `:1044`, texto do evento em `:1048`, mensagem do B3 em `:1176`. |

Todos os consumidores decisórios de produção convergem no mesmo escritor:

- Stop: `bin/hook_stop_fila1.py:1204` → `julga_b3` → `decide_b3` (`bin/disjuntor.py:1093-1099`). O hook recebe as paradas e compõe mensagens em `bin/hook_stop_fila1.py:1205-1208`.
- PreToolUse: `_nega_b3` chama `estado_b3` em `bin/disjuntor.py:1801`; este chama `julga_b3` em `:1105`, que chama `decide_b3` em `:1096`.
- Wrapper: `bin/codex_pergunta.sh:378` executa `--b3-parada`; CLI encaminha em `bin/disjuntor.py:2382-2383`; `b3_parada` usa `estado_b3` em `:1158`.
- Recorte das duas linhas úteis continua no hook: `bin/hook_stop_fila1.py:451-464`; disjuntor o chama sobre o RELATO já lido em `bin/disjuntor.py:797`.
- Referência do hook ao arquivo de estado em `bin/hook_stop_fila1.py:666-669` serve para impedir que o próprio estado prove espera; não interpreta `auto`.
- `ler_estado` lê o JSON inteiro em `bin/disjuntor.py:397-408`; seus leitores de B1/B4 e dos históricos selecionam outros campos (`:596`, `:1882`, `:2213`), sem interpretar `b3[obra].auto`.

Persistência e travas:

- `_com_estado` toma `LOCK_EX` antes de ler o JSON (`bin/disjuntor.py:355-374`), grava por substituição (`:377-380`) e anuncia ainda sob a trava (`:390-394`). A decisão nova usa essa porta em `:1075`.
- Rearme não limpa `auto`, `auto_sobe` ou `consumidas`: altera `rearmado`, `reprovados`, `codex` e `disparado` em `bin/disjuntor.py:1134-1136`. A migração também preserva esses campos (`:804-821`).
- Reinício normal relê o JSON em `bin/disjuntor.py:366-374`; ausência do arquivo vira `{}` (`:368-369`), mas ilegibilidade levanta erro (`:370-373`). A ausência real desse arquivo em produção é `[nao verificado]`.
- Wrapper toma o slot antes da porta (`bin/codex_pergunta.sh:350-364`), depois a trava da resposta (`:374-376`), e então pergunta B3 (`:378`). Triagem toma a mesma trava de resposta compartilhada antes da trava do estado (`bin/disjuntor.py:889-900`, `:915-929`).

Duas observações para a consolidação:

1. `auto` persistido e `auto` do painel têm unidades diferentes: lista cumulativa de SHA por obra (`bin/disjuntor.py:1051`) versus número de eventos LIBERA na janela de 24 h (`:1996`, `:2000`, `:2025`). São estruturas distintas; o painel não é um leitor da cota.
2. “Sobe ao Ronald” está implementado como texto na trilha (`bin/disjuntor.py:1047-1048`), sem leitor de produção específico para `SOBE` no mapa encontrado. Entrega/notificação externa a Ronald é `[nao verificado]`.

## 12/11 11:25 · sessao dc6cc4c65e84 · - · - · origem=Ronald (tmux)

1. **Bloqueia a certificação literal de “pergunta preservada”**: o caso com ATA conserva marca positiva e zero turno aberto, mas o caso original sem célula foi enfraquecido: agora exige marca ausente e um turno aberto. Contraexemplo concreto: a mesma batida espúria de 10/06/2026 16:19, apagadas as células, satisfaz o novo selo com `turnos_abertos=1`, enquanto contrariaria o selo anterior com expectativa zero. A fonte única justifica ler a ausência de classificação; não demonstra preservação da garantia “nunca turno aberto” (`app/relatorios/tests/test_cartao_marca_espuria.py:53`, `:77`, `:97`, `:119`, `:124`, `:131`). A referência do calendário documenta perda da marca, não autorização para acusar turno aberto (`app/colaboradores/tests/test_calendario_orfa.py:18`).

2. **Fila — igualdade com a tela não provada pelo selo.** O comentário afirma “como é na tela”, mas o método só chama o coletor e exige contador 1; não chama a tela nem compara a data da lista (`app/relatorios/tests/test_cartao_marca_espuria.py:131`). O código explica por que 1 é plausível: tela incrementa cada `p.turno_aberto`, enquanto PDF exclui os horários classificados como órfãos; sem célula, a lista de órfãs é vazia (`app/ponto/services/espelho.py:424`, `:436`; `app/relatorios/pdf_espelho.py:512`; `app/escala/services/leitor_celula.py:394`; `app/escala/utils.py:1087`). O resultado integrado para essa fixture e o mesmo dia é **[nao verificado]**.

3. **Fila — mordida visual incompleta.** Mesmo o caso positivo verifica `batida.orfa`, ausência de “turno aberto” no texto e presença de “16:19”; não verifica asterisco, legenda ou separação da linha (`app/relatorios/tests/test_cartao_marca_espuria.py:81`, `:109`). Contraexemplo: renderizar a órfã junto das marcações comuns, mantendo o atributo e o horário no texto, não violaria esses asserts. O código atual efetivamente separa as marcações e aplica asterisco/cinza; portanto é lacuna de prova, não defeito visual demonstrado (`app/relatorios/pdf_espelho.py:832`, `:841`, `:851`).

4. **Fila — nova classe perde prova de emissão.** O override substitui a emissão/extração do PDF do método pai por uma checagem apenas do resumo. A presença da batida sem célula é verificada só no dict do coletor (`app/relatorios/tests/test_cartao_marca_espuria.py:103`, `:126`, `:131`). Contraexemplo: o renderer descartar a marcação sem célula preservaria esses asserts.

5. **Sem achado de segundo escritor produtivo neste retarget.** A criação direta de `CelulaDia` é fixture de teste expressamente informada pela sessão; não demonstra escritor produtivo fora da porta (`app/relatorios/tests/test_cartao_marca_espuria.py:66`). Porém “ATA que o cartório lavraria” permanece **[nao verificado]**: a fixture entrega lâmpadas e órfã prontas, sem executar builder/cartório para comparar a classificação (`app/relatorios/tests/test_cartao_marca_espuria.py:56`).

Leituras adicionais: o dia apresentado vem do juiz `dia_das_batidas`, que distingue batida reclamada por turno e batida sem turno; a tela agrupa por esse mapa, e não por dia civil próprio (`app/ponto/turnos.py:2886`; `app/ponto/services/espelho.py:453`, `:579`). Nenhuma execução integrada certificada nesta análise **[nao verificado]**.

## 12/11 11:25 · sessao c96f7f36ec7e · - · - · origem=Ronald (tmux)

(c) **Sem achado de RED puramente congelado no núcleo novo.** O carregador padrão lê os fontes desta cópia e compila AST real (`app/ponto/tests/test_lastro_c5_sem_banco.py:33-49`, `:108-118`). Os mutantes mudam AST produtiva: canal `_contrato`, reset do mapa, tipo no overlay e metadados da ata (`app/ponto/tests/test_lastro_c14_sem_banco.py:35-55`); overlay/canais/PDF da c13 também (`app/ponto/tests/test_lastro_c13_sem_banco.py:46-80`).

**Fila — alcance parcial e selos textuais.** Os dois selos F1c/cartão exercitados por c13 continuam sendo testes de presença/ausência de texto; executar suas funções não os transforma em prova comportamental (`app/ponto/tests/test_lastro_c13_sem_banco.py:127-149`; `app/relatorios/tests/test_cartao_pela_celula.py:170-181`). A prova dos canais extrai apenas chamadas reais dos builders e substitui o resto do corpo (`app/ponto/tests/test_lastro_c13_sem_banco.py:190-199`). Contraexemplo: quebrar a geometria antes da chamada `_marcar_ausencias` no builder não é percebido por esse recorte. Isso limita a prova, mas não elimina sua mordida sobre o repasse de `recusas` (`app/ponto/tests/test_lastro_c13_sem_banco.py:194-208`).

**RED histórico: reexecução nesta cópia [nao verificado].** Exige `git show` dos objetos c12/c13 (`app/ponto/tests/test_lastro_c13_sem_banco.py:26-45`; `app/ponto/tests/test_lastro_c14_sem_banco.py:29-34`). Os arquivos `docs/evidencia_c14_*.txt` citados no relato não constam nesta cópia; conteúdo/saídas históricas além do anexo [nao verificado].

(d) **Há transporte real em memória; não é só duplo.** O teste chama o builder single real e verifica folga, metadados, limpeza das órfãs e conservação da batida externa; a contraprova desliga apenas o overlay e reencontra `23:00` (`app/ponto/tests/test_lastro_c14_sem_banco.py:199-215`). Chama `ata_do_dia`, `lavrar_veredito(save=False)`, leitor, `grade_do_fechamento`, mapas financeiros e trecho real do fechamento; verifica mudança da ata antiga ausência → folga (`app/ponto/tests/test_lastro_c14_sem_banco.py:216-273`). Os mutantes removem classificação/metadados nos fontes reais (`:45-55`).

**Bloqueia o fechamento literal de (d), caso o requisito seja simular a cadeia integrada.** A passagem ao simulador substitui o recalculador real por `Mock(return_value={17: fech})`, classificador/TXT por duplos e roda `instantaneo(lido=True)` sobre esse fechamento já preparado (`app/ponto/tests/test_lastro_c14_sem_banco.py:280-306`). Contraexemplo: se o recalculador real deixar de ler a ata relavrada e retornar `{}`, esse teste continua verde, porque a implementação real foi substituída em `:295`. O simulador real solicita o recalculador em `app/folha/management/commands/simular_folha.py:129-134`; esse elo não é exercitado.

A antiga ata também é sintetizada a partir da nova, mudando só `tipo_dia`; não nasce de um primeiro julgamento ausência (`app/ponto/tests/test_lastro_c14_sem_banco.py:217-222`). Persistência é explicitamente proibida no teste (`:223-244`). Portanto: prova classificação/transporte/lavra/leitores em memória; execução `processar_cartorio` → persistência → recalcular → fotos/comparação [nao verificado].

O caminho shell capaz de atravessar a relavra existe: `--antes` roda antes do simulador; em `par`, aplica somente à foto depois (`bin/simular_folha.sh:65-74`, `:94-99`). Isso não é executado pelo teste c14 (`app/ponto/tests/test_lastro_c14_sem_banco.py:183-308`).

Execuções feitas agora, com `-B`/`PYTHONDONTWRITEBYTECODE`: c14 8/8, c13 10/10; mutante `relavra_tipo` falha na asserção `c14:207`; `ata_metadados` falha com `KeyError` em `c14:243`. Não há artefato gravado para esses resultados: [nao verificado].

## 12/11 11:25 · sessao c020ff845a76 · AUDITORIA-DE-TRAVAS-CORTE · censo · origem=Code

Regra: mantenho as regras 1–3 e o recorte da volta 1 (`medida_8_v1.r:3–5`). Aplico **A primeiro**: arquivo de algum teste vermelho alterado na mesma obra, com `run < commit ≤ pouso`. Depois **B**: nenhuma alteração elegível e **todos os IDs completos** repetidos em outras obras com distância absoluta ≤24 h. **C** é o restante. Associações de obra por pasta/assunto permanecem **[nao verificado]**. [nao verificado]

| Classe | Número | Linhas de `suites_14d.tsv` |
|---|---:|---|
| **A — MUDOU DECISAO** | **63** | `suites_14d.tsv:1,3,7,23–24,27,40,42,54,58,94,118,124,145,151–152,160,186–187,213,219,228,232,239,254,268–269,279,309,340,342,344,346,363–365,368,370,372,376,379,390,395–396,398,400–402,405,439,444–445,447,457–458,462,466,468,478,485,496–497,506` |
| **B — RUIDO** | **8** | `suites_14d.tsv:6,30,65,79,107,130,291,451` |
| **C — SEM SINAL** | **38** | `suites_14d.tsv:20,22,45,59,63,72,74–75,78,89–90,98,116,163,203,214,216–217,244,256,259,265,302,304,310,318,354,358,380,403–404,419,428,430,470,472,498–499` |

Correção da volta 1: **470 fica em C**. Seu vermelho de `test_selo_mapa_contrato` só reaparece fora da janela de 24 h nas outras obras identificadas (`suites_14d_falhas.tsv:30,79,114`; identidade das obras **[nao verificado]**).

Limite declarado: cura apenas em código de produção cai em **C**, não em A; a causa concreta dos casos C permanece **[nao verificado]**.

## 12/11 11:25 · sessao f2eb9b860cab · - · - · origem=Ronald (tmux)

(b) **Sem achado novo nos três sítios.** `_recusas_tela` nasce antes da seleção, entra no helper e sobrevive até o compositor; pré-apuração e `AtaAgregadaSemMarcos` usam esse helper (`app/ponto/services/espelho.py:486`, `:496`, `:498`, `:536`, `:565`, `:654`, `:655`, `:881`). A2 entrega a exceção no mapa e não conclui precedência nesse dia; overlay transporta o rótulo e `tipo_dia=None` (`app/ponto/precedencia.py:420`, `:423`, `:425`; `app/escala/utils.py:1218`).

**Sem except novo que engole.** Permanecem os genéricos antigos da seleção e da palavra; a captura explícita de `TipoSemFaixa` na composição relança (`app/ponto/services/espelho.py:574`, `:657`, `:660`; delta fornecido do arquivo). Falha HTTP integrada/ausência de outros 500: **[nao verificado]**; o handler chama o serviço diretamente (`app/ponto/views.py:157`).

**Fila — rótulo visível não garantido na UI.** Contraexemplo: ausência `fantasma_xyz` + batida em dia encerrado sem lavra dá título genérico; com `status_dia='ok'` previamente lavrado, mantém título antigo. D4 retorna antes de consultar recusas; HTML lê `status_rotulo` (`app/ponto/status_dia.py:290`, `:295`, `:317`; `app/templates/ponto/espelho.html:151`; caracterização `app/ponto/tests/test_lastro_c14_sem_banco.py:169`).

**Quem escreve/lê:**

- `recusas[data]`: `TipoSemFaixa` cria código/rótulo; A2 escreve mapa; overlay e espelho leem; D4 possui seu mapa e leitor (`app/ponto/precedencia.py:99`, `:423`; `app/escala/utils.py:1218`; `app/ponto/services/espelho.py:881`; `app/ponto/status_dia.py:301`, `:317`).
- `_recusa_do_dia`: espelho escreve; compositor lê e opcionalmente limpa; PDF chama esse mesmo compositor como último leitor (`app/ponto/services/espelho.py:881`, `:339`, `:349`; `app/relatorios/pdf_espelho.py:592`, `:594`).
- `palavra_dia`: inicializada vazia, preenchida pelo compositor comum; PDF lê para desenhar (`app/ponto/services/espelho.py:906`, `:340`, `:347`, `:1302`; `app/relatorios/pdf_espelho.py:879`). HTML da faixa usa outro campo (`app/templates/ponto/espelho.html:151`).

**Mordida real, com limites.** O ensaio executa seleção/handlers/helper reais e o valor real de `_recusa_do_dia`; builder e leitor/geometria são duplos; A2 posterior é propositalmente vazia para expor perda do primeiro transporte (`app/ponto/tests/test_lastro_c14_sem_banco.py:97`, `:103`, `:108`, `:113`, `:124`, `:136`, `:143`). Mutantes alteram AST produtivo do canal/reset, sem modificar arquivo (`app/ponto/tests/test_lastro_c14_sem_banco.py:24`, `:35`, `:39`). Executei quatro testes pertinentes: OK; canal removido deu dois erros e uma falha, reset deu três falhas. Registro persistido desta execução: **[nao verificado]**.

## 12/11 11:24 · sessao 89b671245bc2 · - · - · origem=Ronald (tmux)

Fatia `suites_14d.tsv:150–345`: **32 FAILED intermediárias**, com associação de obras por nome **[nao verificado]**.

| Classe | Total | Linhas de `suites_14d.tsv` |
|---|---:|---|
| A | 18 | 151, 152, 160, 186, 187, 213, 219, 228, 232, 239, 254, 268, 269, 279, 309, 340, 342, 344 |
| B | 1 | 291 |
| C | 13 | 163, 203, 214, 216, 217, 244, 256, 259, 265, 302, 304, 310, 318 |

A linha **279**, `jv3/suite_73a27d96_wip.log`, associa-se ao commit `790648d84`, assunto C20-JUIZ-3, pelo hash da suite seguinte; essa identidade de obra permanece **[nao verificado]**. O arquivo vermelho alterado é `app/ponto/tests/test_o_outbox_celula.py` **[nao verificado: número da linha no gitlog]**.

**309 é A; 310 é C**: a primeira run tem horário 05:37 e a cópia 05:55 (`suites_14d.tsv:309–310`); o commit `41e5dea51` que altera os três arquivos vermelhos tem horário 05:38 **[nao verificado: número da linha no gitlog; associação nominal]**.

Para **B291**, o ID completo `chamados.tests.test_contract_crons.ContratoPipelineB6Test.test_todo_command_tem_casa` também aparece na obra `f3_beco1`, 12h29 antes (`suites_14d.tsv:259`, `:291`; IDs em `suites_14d_falhas.tsv`, números das linhas **[nao verificado]**; distinção nominal das obras **[nao verificado]**).

## 12/11 11:24 · sessao d75940a3c4e3 · - · - · origem=Ronald (tmux)

Na fatia `suites_14d.tsv:346–526`, as 44 intermediárias FAILED dividem-se assim; associações entre obra e assunto **[nao verificado]**:

| Classe | Total | Linhas de `suites_14d.tsv` |
|---|---:|---|
| A | 31 | 346, 363–365, 368, 370, 372, 376, 379, 390, 395–396, 398, 400–402, 405, 439, 444–445, 447, 457–458, 462, 466, 468, 478, 485, 496–497, 506 |
| B | 1 | 451 |
| C | 12 | 354, 358, 380, 403–404, 419, 428, 430, 470, 472, 498–499 |

Correção da volta 1: **470 é C**. O ID vermelho de `test_selo_mapa_contrato` aparece somente em `suites_14d_falhas.tsv:30`, `:79` e `:114`; as outras duas ocorrências ficam fora de 24 horas da run 470 (`suites_14d.tsv:99`, `:358`, `:470`).

## 12/11 11:22 · sessao f5779ce1b0ab · - · - · origem=Ronald (tmux)

Nas linhas 1–149, são **33 INTERMEDIÁRIAS FAILED**, excluídas as FINAL e indeterminadas da volta 1; associações nominais **[nao verificado]**.

| Classe | Número | Linhas em `suites_14d.tsv` |
|---|---:|---|
| A | **14** | 1, 3, 7, 23, 24, 27, 40, 42, 54, 58, 94, 118, 124, 145 |
| B | **6** | 6, 30, 65, 79, 107, 130 |
| C | **13** | 20, 22, 45, 59, 63, 72, 74, 75, 78, 89, 90, 98, 116 |

Testemunhas A, todas posteriores à run e anteriores à FINAL candidata associada, conforme os anexos; números de linha do gitlog **[nao verificado]**:

| Linha da suite | Commit | Arquivo vermelho alterado | [nao verificado]
|---:|---|---|
| 1 | `caaf0e3ec`, 29/10 00:55 | `app/ponto/tests/test_passe_turno_aberto_retroativo.py` |
| 3 | `378481fd3`, 29/10 02:54 | `app/chamados/tests/test_contract_leitores_vitalidade.py` |
| 7 | `a1a19ce86`, 29/10 04:09 | `app/chamados/tests/test_contract_leitores_vitalidade.py` |
| 23 | `786a09283`, 29/10 17:33 | `app/escala/tests/test_ata_x_turno_entrada.py` |
| 24 | `786a09283`, 29/10 17:33 | `app/ponto/tests/test_a2_marco_morto.py` |
| 27 | `cbedeb91b`, 29/10 17:54 | `app/ponto/tests/test_selo_chokepoint_escrita.py` |
| 40 | `673ecaf79`, 30/10 12:17 | `app/api/tests/test_chamados_v2_destino.py` |
| 42 | `bfbc2aba7`, 30/10 13:04 | `app/chamados/tests/test_agrupado_em.py` |
| 54 | `8cb8a4c55`, 30/10 17:40 | `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py` |
| 58 | `4f0476aa5`, 30/10 19:51 | `app/chamados/tests/test_b2d_resposta_visivel.py` |
| 94 | `1d01bd905`, 31/10 07:39 | `app/ponto/tests/test_a2_marco_morto.py` |
| 118 | `973753a24`, 31/10 17:34 | `app/ponto/tests/test_contract_no_batida_date.py` |
| 124 | `f6f420ec3`, 31/10 23:54 | `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py` |
| 145 | `5ec1220ad`, 01/11 12:33 | `app/ponto/tests/test_cobparc_marco_no_periodo.py` |

Os vínculos `grupo1` + `grupo1p1` nas linhas 23–24 e `v3per` + `v3p5` na linha 124 seguem o agrupamento da volta 1 **[nao verificado]**.

_4208 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
