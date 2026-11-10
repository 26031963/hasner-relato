# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 19:59** · publicado em **10/11/2026 19:59** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 19:59 · sessao 10354500077f · - · - · origem=Ronald (tmux)

Leitura concluída, sem editar arquivos.

- **F2 confere:** trava e seleção de aberta antecedem os ramos; havendo qualquer aberta, inclusive duas incoerentes, o código entra em agregação e retorna antes do nascimento. Assim, no ramo de nascimento, a única aberta criada pelo próprio ensaio não pode ser dona estrangeira. (`chamados/services/disputa_emissao.py:613`, `:615`, `:618`, `:636`, `:656`)
- **F3 confere quanto à FK da dona:** o reuso escreve célula, destino e relavratura da pergunta existente; retorna a mesma dona. A inserção usa `disputa=disputa`. Ressalva literal: `disputa` também participa das **leituras** de identidade/episódio anteriores; não entra exclusivamente na inserção como insumo de leitura. (`chamados/models.py:2866`, `:2875`, `:2965`, `:2982`, `:3003`, `:3008`, `:3014`)
- **R2, pai desfeito:** `_derivar_chamado_destino` exclui `categoria='disputa_supervisao'`; o pai criado pelo ensaio tem precisamente essa categoria. Portanto, esse pai não pode ser destino devolvido pelo derivador; não é necessário substituir esse destino por `None` nesse cenário. (`chamados/services/disputa_emissao.py:198`, `:201`, `:640`, `:642`)

O resolvedor ainda pode derivar episódio BATIDA do contexto da dona dentro da porta. A guarda de mesmo PK e `criada=False` deve desfazer qualquer resultado divergente. Intervalo pertence à família MARCO e não recebe esse episódio. (`chamados/models.py:2866`, `:3452`, `:3460`; `chamados/services/declaracao_texto.py:42`)

Na **cópia atual**, agregação simples escolhe **D+1**: o merge começa pelo contexto guardado, recebe somente `ultima_batida` e preserva `motivo.data`; o resolvedor consome essa data antes do fallback pela batida. O comportamento antecede o delta solicitado. A base histórica `6c0517eb9` não foi consultada: **[nao verificado]**. (`chamados/services/disputa_emissao.py:530`, `:531`, `:541`, `:619`, `:623`, `:328`, `:354`, `:358`, `:361`, `:370`)

| Campo | Escritores e leitores relevantes |
|---|---|
| `DS.motivos` | Núcleo cria/agrega; modelo poda e reabre acrescentando motivo. Núcleo admite pela existência de perguntas locais. (`chamados/services/disputa_emissao.py:564`, `:576`, `:595`, `:656`; `chamados/models.py:1961`, `:2031`, `:2063`, `:2156`) |
| `DS.contexto`, `qtd`, `ultima_em` | Merge escreve metadados; persistência grava contexto. Resolvedor lê data/batida; derivador lê data; porta BATIDA lê episódio. Busca de acessos específicos encontrou leitura de `qtd` no próprio merge e teste API; nenhum consumidor específico de `ultima_em` nessa busca. (`chamados/services/disputa_emissao.py:191`, `:355`, `:370`, `:530`, `:538`, `:539`, `:588`; `chamados/models.py:3463`; `api/tests/test_api_bater_pwa.py:91`) |
| `DS.fechada_em` | Modelo fecha/reabre; núcleo e wrapper distinguem aberta/fechada; os três receivers verificam valor, sem exigir transição. (`chamados/models.py:1932`, `:2149`; `chamados/services/disputa_emissao.py:578`, `:598`, `:616`, `:778`; `chamados/signals.py:28`, `:129`, `:153`) |
| Pergunta: PK/FK, dia, célula, destino | Porta elege pergunta e distingue reuso/inserção; reuso conserva dona. Escritas de dia também passam pelas portas de fio. (`chamados/models.py:2907`, `:2921`, `:2940`, `:2965`, `:2982`, `:3008`, `:3014`; `chamados/services/fio.py:790`, `:804`, `:979`) |
| `criada` e pergunta devolvida | Gerador separa pergunta retornada de realmente criada; wrapper só notifica motivos de perguntas realmente criadas e locais. (`chamados/services/disputa_emissao.py:484`, `:499`, `:510`, `:511`, `:514`, `:783`) |

Impacto leitor a leitor da política de motivos locais:

| Leitor | Consequência |
|---|---|
| `api/foto_recebida.py:63`, `:70` | Seleção acompanha a dona aberta, mas ainda exige `contexto.ultima_batida.id` correspondente; transferência só de motivo não garante passar essa guarda. (`:67`, `:68`, `:73`) |
| `colaboradores/signals.py:74` | Remoção de `posto_desativado` acompanha a dona aberta selecionada. (`:71`, `:77`) |
| `escala/signals.py:158` | Remoção de `ancora_ausente` acompanha a dona aberta selecionada. (`:155`, `:161`) |
| `chamados/models.py:1997` | É documentação; guardas executáveis verificam fechamento e motivo presente. (`:2031`, `:2033`, `:2036`) |
| `auditar_disputas.py:88` | Auditoria passa a exigir contexto dos motivos mantidos. (`:89`, `:91`) |
| `auditar_disputas.py:142` | Exibe a lista mantida. |
| `auditar_disputas.py:169` | Compara lista com perguntas locais; a interpretação já corresponde ao pertencimento local, com exceção contextual para emissão impossível. (`:182`, `:188`, `:191`) |
| `auditar_invariantes_chamados.py:50` | Resolve dias dos motivos mantidos; I7 continua selecionando abertas sem nenhuma linha de pergunta. (`:51`, `:62`, `:63`) |
| `chamados/relatorios_disputa.py:52` | Exibe rótulos dos motivos mantidos. |
| `chamados/services/acoes_disputa.py:149` | Mensagem usa os motivos mantidos. |
| `chamados/services/fio.py:680` | Mensagem usa os motivos mantidos. |
| `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49` | Cobertura pode mudar; também exige contexto com motivo/prefixo, que não acompanha a transferência somente de motivos. (`:52`, `:53`, `:55`) |
| `ponto/management/commands/alertar_disputas_pendentes.py:82` | Log usa lista mantida. |
| `templates/chamados/detalhe_local.html:301` | Exibe lista mantida. |
| `templates/chamados/partials/modal_fio.html:326` | Aviso depende da presença local de âncora. |
| `colaboradores/services/drawer.py:75` | Comentário descreve agregado que não poda; execução usa perguntas vivas, sem leitura direta da lista. (`:77`, `:78`) |

Para o **delta de replay com dona fechada**, esses leitores não devem mudar por escrita em motivos/contexto da dona, pois a decisão é persistir nada nela. A alteração da pergunta pode ter efeitos próprios pelos leitores da pergunta; execução desses efeitos permanece **[nao verificado]**. (`chamados/services/disputa_emissao.py:578`; `chamados/models.py:3006`, `:3007`)

## 10/11 19:56 · sessao 51de6528d488 · MAPA-FILA-BO · censo · origem=Code

O vínculo desta cópia ao hash informado não pôde ser conferido por Git: a árvore disponível não tem repositório Git reconhecido. [não verificado]

Em **(b)**, “não cria estado” significa que a cura usa campos, valores e combinações já admitidos pelas portas existentes; a ocorrência efetiva dessas combinações em produção continua [não verificado]. Distingo mudança do decisor de apresentação da mudança do juiz de negócio, conforme a separação entre autoridade e escritor registrada em `app/core/juizes.py:3`.

## DISPUTA-ZUMBI-1110

**(a) Lei, juiz e porta.** Aplicam-se **L-002**, testemunha lê a mesma autoridade, e **L-007**, escritor único com trilha (`app/docs/LEIS.md:60`, `app/docs/LEIS.md:65`). O corte C7 declara **`DisputaSupervisao.pode_fechar`**, registrado no código e no lookup (`app/docs/CORTES.md:272`, `app/core/juizes.py:279`, `app/docs/JUIZES.md:57`).

C7 exige **nenhuma resposta sem veredito e nenhum fio vivo**; a porta canônica é **`DisputaSupervisao.fechar`**, que grava conjuntamente fechamento, autor, resolução e motivo (`app/chamados/models.py:1853`, `app/chamados/models.py:1932`). Os carimbos das perguntas passam por `carimbar` e `carimbar_identidade` (`app/chamados/models.py:3103`, `app/chamados/models.py:3168`).

**Conferência do relato:**

- **Confere:** `lavrar_sem_turno_aberto` já consulta C7 e chama `fechar` (`app/chamados/reconciliador.py:411`, `app/chamados/reconciliador.py:412`).
- **Confere:** o explicador calcula “pode fechar” somente por `not _fio_vivo(ps)`, omitindo a guarda adicional de respostas sem veredito de C7 (`app/chamados/services/explicador.py:136`, `app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).
- **“Fechamento só por beco” não confere globalmente:** existem caudas de fechamento na reconciliação e na materialização (`app/chamados/reconciliador.py:1170`, `app/chamados/services/materializacao.py:1316`).
- **“7 carimbadores sem C7” não está demonstrado como sete caminhos sem fechamento:** `encerrar_fantasma` alcança C7 pelo reconciliador; materialização tem caudas compartilhadas; `remover_motivo` chama diretamente `fechar` (`app/chamados/reconciliador.py:1147`, `app/chamados/reconciliador.py:1170`, `app/chamados/services/materializacao.py:1378`, `app/chamados/services/passivo_validado.py:405`, `app/chamados/models.py:2072`).

**(b) Cria estado: não. Muda quem decide: sim, no explicador.** Os campos e motivos de fechamento já existem, e a porta já produz a combinação fechada/resolução/motivo (`app/chamados/models.py:1757`, `app/chamados/models.py:1773`, `app/chamados/models.py:1780`, `app/chamados/models.py:1932`). Porém o bool apresentado deixaria de ser decidido pela negação local de `fio_vivo` e passaria a consumir C7; o juiz legal de fechamento permaneceria o existente (`app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).

**(c) Censo do bool cujo decisor muda: 7 ocorrências = 1 escritor + 6 leitores.**

| Papel e forma | Sítio |
|---|---|
| Escritor: atribuição `pode = not _fio_vivo(ps)` | `app/chamados/services/explicador.py:137` |
| Leitor: condição de ternário | `app/chamados/services/explicador.py:138` |
| Leitor: truthiness no `if` | `app/chamados/services/explicador.py:139` |
| Leitor: exportação na chave `pode_fechar` | `app/chamados/services/explicador.py:163` |
| Leitor: truthiness de `dict.get` | `app/chamados/services/desenlace.py:63` |
| Leitor: truthiness em template, cor | `app/templates/chamados/partials/modal_fio.html:434` |
| Leitor: truthiness em template, mensagem | `app/templates/chamados/partials/modal_fio.html:445` |

O transporte preserva o dict: cache, atribuição a `ch.explicacao` e passagem ao alias `desenlace as _desen`; não o contei novamente como decisão (`app/chamados/services/fio.py:312`, `app/chamados/services/fio.py:314`, `app/chamados/services/fio.py:386`, `app/chamados/services/fio.py:403`).

Método: grep global de `pode_fechar`, `por_que_nao_fecha` e transportes de `explicacao`, seguido dos imports, aliases e usos locais de `pode`; as sete ocorrências estão enumeradas acima. Não apareceram consumidores adicionais em commands, crons, signals ou JS de `static/` para esse bool calculado. [não verificado por referência negativa arquivo:linha]

**Censo complementar da costura**, separado dos sete acima: AST de chamadas de método encontrou **13 chamadas de carimbo em 12 funções e 12 chamadas diretas a C7**; a unidade é `ast.Call`, independentemente do nome do receptor:

| Forma | Sítios |
|---|---|
| `.carimbar(...)`: 8 | `app/chamados/models.py:3287`; `app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:595`; `app/chamados/services/disputa_emissao.py:1863`; `app/chamados/services/disputa_emissao.py:1937`; `app/chamados/services/materializacao.py:561`; `app/chamados/services/materializacao.py:1277`; `app/chamados/services/materializacao.py:1431` |
| `.carimbar_identidade(...)`: 5 | `app/chamados/models.py:2061`; `app/chamados/models.py:3064`; `app/chamados/services/fio.py:1359`; `app/chamados/services/materializacao.py:924`; `app/chamados/services/materializacao.py:1099` |
| `fechada_em is None and …pode_fechar()`: 6 | `app/chamados/reconciliador.py:411`; `app/chamados/reconciliador.py:611`; `app/chamados/services/becos.py:56`; `app/chamados/services/disputa_emissao.py:1963`; `app/chamados/services/materializacao.py:585`; `app/chamados/services/materializacao.py:1316` |
| `elif …pode_fechar()` | `app/chamados/reconciliador.py:1170` |
| Ternário sobre C7 | `app/chamados/services/becos.py:33` |
| `if not …pode_fechar()`: 2 | `app/chamados/services/competencia_trancada.py:153`; `app/chamados/services/competencia_trancada.py:202` |
| Atribuição seguida de truthiness | `app/chamados/services/disputa_emissao.py:1278`, `app/chamados/services/disputa_emissao.py:1282` |
| `if disp and disp.pode_fechar()` | `app/ponto/services/entrada_sincronizada.py:58` |

O signal genérico de salvamento da pergunta agenda a porta de becos após o commit; esse classificador exige pai resolvido e ausência de perguntas visíveis antes de consultar C7 (`app/chamados/signals.py:227`, `app/chamados/signals.py:238`, `app/chamados/services/becos.py:29`, `app/chamados/services/becos.py:31`, `app/chamados/services/becos.py:33`). O cron correspondente usa `--alarme`; isso não equivale a um fechamento automático geral (`app/config/crons.py:663`).

**(d) Casa:** **COMPLEMENTO de C7-DISPUTA-FECHA-POR-UM-JUIZ**, com interseção na série E3/E4; a casa já manda migrar decisões espalhadas para C6/C7 (`app/docs/BACKLOG.md:1468`, `app/docs/BACKLOG.md:1862`, `app/docs/BACKLOG.md:1863`). Os múltiplos sítios enumerados impedem classificá-lo como BO de sítio único.

**(e) Padrão:** **Single Writer Principle / juiz único**, termo existente, realizado ao retirar a decisão paralela do explicador e consumir C7 (`app/docs/GLOSSARIO.md:12`, `app/chamados/services/explicador.py:137`, `app/core/juizes.py:279`).

**Depende de dado:** contagem dos dez casos por instante do carimbo antes/depois de 22/10, via, estado do pai, respostas sem veredito e resultado atual de C7; também contagem de tentativas de fechamento recusadas. A atribuição histórica ao commit indicado no anexo não foi conferida. [não verificado] [nao verificado]

perguntas de lei: 0

O critério de fechamento e a guarda de resposta sem veredito já estão escritos; a separação histórica dos dez casos é pergunta de dado (`app/docs/CORTES.md:272`, `app/chamados/models.py:1853`).

## PERGUNTA-DUPLICATA-1110

**(a) Lei, juiz e porta.** Aplicam-se **L-001/L-002/L-003/L-007**: origem, autoridade comum, medição pela função real e escritor com trilha; a idempotência integra o contrato estrutural (`app/docs/LEIS.md:59`, `app/docs/LEIS.md:60`, `app/docs/LEIS.md:61`, `app/docs/LEIS.md:65`, `CLAUDE.md:175`).

**Sem juiz registrado para eleger a pergunta canônica.** C8 pergunta pela duplicidade de **chamado** e aponta para `ChamadoColaborador.abrir`; não registra a eleição de `PerguntaDisputa` (`app/core/juizes.py:114`, `app/core/juizes.py:280`). O lookup de fio aponta para `_disputa_do_chamado`, outra pergunta (`app/docs/JUIZES.md:40`).

Quem decide hoje:

- **Identidade:** MARCO por colaborador/dia/motivo/instância; BATIDA por episódio/motivo (`app/chamados/services/fio.py:1372`).
- **Canônica:** menor pk com resposta textual não vazia; na ausência dela, menor pk (`app/chamados/services/fio.py:1225`, `app/chamados/services/fio.py:1232`).
- **Retratabilidade:** `plano_de_retratacao` preserva mérito, episódios distintos e acervo sem episódio (`app/chamados/services/fio.py:1259`, `app/chamados/services/fio.py:1287`, `app/chamados/services/fio.py:1290`, `app/chamados/services/fio.py:1294`).
- **Porta/escritor:** `retratar_para_canonica` → `retratar_duplicata` → `carimbar_identidade`; a gravação física ocorre no modelo (`app/chamados/services/fio.py:1319`, `app/chamados/services/fio.py:1345`, `app/chamados/services/fio.py:1359`, `app/chamados/models.py:3190`).

**Conferência do relato.** Os **486 grupos/848 excedentes** são dados fornecidos sem chave de agrupamento nem pks; sua equivalência a duplicatas segundo o código não está demonstrada. [não verificado] O agrupador exclui conversa e vias fora do fato, distingue famílias e aplica horizonte; o plano também conserva perdedoras não retratáveis (`app/chamados/services/fio.py:1431`, `app/chamados/services/fio.py:1945`, `app/chamados/services/fio.py:1949`, `app/chamados/services/fio.py:1290`).

“Disputa aberta” não basta para demonstrar inflação: o app primeiro consulta `q_vivas`, confirma C6 e só depois classifica pendência por resposta ausente e disputa aberta (`app/chamados/services/pergunta_no_app.py:50`, `app/chamados/services/pergunta_no_app.py:58`, `app/chamados/services/pergunta_no_app.py:71`).

Há ainda uma limitação na proposta de “medir idade”: a pergunta dispõe de data do fato e carimbos posteriores, mas não de carimbo próprio de nascimento; essa ausência está registrada em **O326** (`app/chamados/models.py:2423`, `app/chamados/models.py:2448`, `app/chamados/models.py:2449`, `app/docs/BACKLOG.md:1633`). Idade exata de criação não pode ser inferida desses outros carimbos. [não verificado]

**(b) Cria estado: não. Muda quem decide: não**, no escopo de medir pelo universo existente e retratar apenas os excedentes aceitos pela porta atual. `duplicata` já é valor existente, e a porta já grava via/carimbo/autor/motivo; o nascimento também consulta a eleição existente pelo alias `_canonica` (`app/chamados/models.py:2495`, `app/chamados/models.py:3182`, `app/chamados/models.py:3186`, `app/chamados/models.py:3190`, `app/chamados/models.py:2905`, `app/chamados/models.py:2921`).

Adicionar nascimento próprio ou fato novo de identidade ampliaria esse escopo: são obras já separadas, **O326** e **FATO-A06** (`app/docs/BACKLOG.md:1633`, `app/docs/BACKLOG.md:1867`).

**(c) A condição de censo global dos campos não é acionada por não/não.** Como inventário complementar, contei por AST o perímetro de cinco símbolos — `canonica_do_grupo`, `plano_de_retratacao`, `grupos_de_colisao`, `retratar_para_canonica`, `retratar_duplicata` — resolvendo imports com `as`, inclusive `_canonica`. São **23 chamadas: 19 leituras de decisão e 4 delegações de escrita**, enumeradas abaixo:

| Arquivo | Sítios e forma |
|---|---|
| `app/chamados/management/commands/backfill_decisoes_pergunta.py` | `:276` eleição; `:290` plano/truthiness das categorias em `:295`; `:331` escrita; `:393` eleição após `len>=2` em `:392`; `:396` leitura `['retrata']`; `:485` grupos; `:506` eleição/comparação de pk em `:507`; `:510` plano/`len` em `:512`. |
| `app/chamados/management/commands/dedup_perguntas_fato.py` | `:58` grupos; `:76` eleição; `:81` plano, leitura `['retrata']` em `:88` e truthiness em `:89`. |
| `app/chamados/models.py` | `:2921` eleição pelo alias `_canonica`, condicionada à lista; `is not None` em `:2922`. |
| `app/chamados/services/declaracao_texto.py` | `:253` eleição/comparação de disputa e `_reusavel` em `:254`; `:275` eleição após truthiness da lista em `:271`. |
| `app/chamados/services/fio.py` | `:921` escrita condicionada à lista em `:920`; `:956` eleição/comparação de pk em `:957`; `:967` plano/`any` em `:968`; `:1340` plano; `:1341` escrita em compreensão com truthiness; `:1832` eleição condicionada às sementes em `:1833`; `:2155` eleição/comparação de pk em `:2165`; `:2158` plano/truthiness em `:2166`; `:2180` escrita/leitura `['retratadas']`. |

A unidade é chamada dos cinco símbolos, excluindo testes/migrations; o save físico subsequente não foi somado novamente (`app/chamados/services/fio.py:1359`, `app/chamados/models.py:3190`). Nenhuma referência adicional desses cinco símbolos apareceu em templates, signals ou JS; isso não constitui censo global dos campos. [não verificado por referência negativa arquivo:linha] A menção ao backfill em crons é declaração de papel de ato único (`app/config/crons.py:780`, `app/config/crons.py:783`).

**(d) Casa:** **COMPLEMENTO de FIO-E0-V4 E1**, articulado à E4; o fato próprio de duplicata/identidade fica depois da E4d (`app/docs/BACKLOG.md:1848`, `app/docs/BACKLOG.md:1863`, `app/docs/BACKLOG.md:1867`). Há um resíduo específico sobre episódio retratado, mas não se pode atribuir os 486 grupos a ele sem dados (`app/docs/BACKLOG.md:786`). [não verificado quanto ao alcance]

**(e) Padrão:** **idempotência pela chave do fato**, termo existente; a cura deve consumir a identidade compartilhada pelo nascimento e pelo dedup (`app/docs/GLOSSARIO.md:29`, `app/chamados/models.py:2905`, `app/chamados/services/fio.py:1372`).

**Depende de dado:** reclassificação dos 486 grupos pela chave real; quantidades retratáveis, recusadas por mérito, episódios unidos e acervo sem episódio; distribuição por horizonte/competência; quantos excedentes passam por C6 e entram efetivamente nos contadores. Os 848 não demonstram, isoladamente, inflação. [não verificado]

perguntas de lei: 1

**Quando uma linha BATIDA do acervo sem episódio e uma linha com episódio podem representar o mesmo fato, permitindo que uma retrate a outra?** O código declara essa lei aberta e hoje conserva ambas (`app/chamados/services/fio.py:1274`, `app/chamados/services/fio.py:1275`, `app/chamados/services/fio.py:1294`). Não foi localizada resposta posterior nas leis/cortes pesquisados. [não verificado quanto à ausência global] Depende de dado: quantos grupos fornecidos pertencem a essa classe. [não verificado]

## BO-ROTULO-ENCERRADO

**(a) Lei, juiz e porta.** Aplicam-se **L-002**, testemunha lê a autoridade; **LEIS-UI L2**, tela e ação consultam a mesma função; **L7** trata especificamente de ajuda proveniente do mapa do domínio, não estabelece literalmente “um rótulo por estado” (`app/docs/LEIS.md:60`, `app/docs/LEIS-UI.md:23`, `app/docs/LEIS-UI.md:77`).

**Sem juiz registrado para o texto das ações do seletor.** O registro chamado não contém essa pergunta; hoje os textos são decididos por dois blocos literais de template (`app/core/juizes.py:272`, `app/core/juizes.py:293`, `app/templates/chamados/partials/modal_fio.html:667`, `app/templates/chamados/partials/painel_chamado_turno.html:76`).

Para espera do colaborador, decide `rotulo_para_pessoa`, através do filtro `espera`; para transição de negócio, o juiz registrado é `motor.decidir`, acessado por `transicionar`/`pode_transicionar`, com escritor `_gravar_estado` (`app/chamados/services/verbos.py:138`, `app/chamados/templatetags/chamados_tags.py:63`, `app/core/juizes.py:274`, `app/docs/JUIZES.md:37`, `app/chamados/models.py:119`, `app/chamados/models.py:128`, `app/chamados/models.py:201`).

**Conferência do relato:**

- **Confere:** os dois detalhes usam `espera|first` e mostram “Encerrado” para `resolvido` e `fechado`, conforme o mapa (`app/templates/chamados/detalhe.html:133`, `app/templates/chamados/detalhe.html:134`, `app/templates/chamados/detalhe_local.html:396`, `app/templates/chamados/detalhe_local.html:397`, `app/chamados/services/verbos.py:118`, `app/chamados/services/verbos.py:119`).
- O defeito também alcança **um terceiro seletor**, no fio do colaborador (`app/templates/chamados/fio_colaborador.html:269`, `app/templates/chamados/fio_colaborador.html:270`).
- **Confere:** o modal distingue “Encerrar” de “Encerrar e arquivar” (`app/templates/chamados/partials/modal_fio.html:668`, `app/templates/chamados/partials/modal_fio.html:669`).
- **Não existe a fonte comum pressuposta pela cura descrita:** o modal contém literais, repetidos no painel do turno. Seu comentário distingue explicitamente verbo da ação e valor enviado à view (`app/templates/chamados/partials/modal_fio.html:660`, `app/templates/chamados/partials/modal_fio.html:667`, `app/templates/chamados/partials/painel_chamado_turno.html:76`).

**(b) Cria estado: não. Muda quem decide: sim, na apresentação.** Os valores enviados já existem; a cura centralizaria quem fornece o texto, preservando os `value` e a decisão de negócio (`app/chamados/models.py:31`, `app/chamados/models.py:32`, `app/templates/chamados/detalhe_local.html:396`, `app/templates/chamados/detalhe_local.html:397`). O servidor recebe `novo_status` e chama a mesma porta, que consulta o motor (`app/chamados/services/acoes_chamado.py:287`, `app/chamados/services/acoes_chamado.py:315`, `app/chamados/models.py:123`).

**(c) Censo do texto de espera/seletor: 13 sítios = 3 definidores/renderizadores + 10 leitores.** A unidade é função ou bloco de template; opções irmãs contam como um bloco.

| Papel | Sítio | Forma |
|---|---|---|
| Definidor | `app/chamados/services/verbos.py:114` | Dict `ESPERA_COLAB`, comparação por chave de estado |
| Definidor/renderizador | `app/templates/chamados/partials/modal_fio.html:667` | Texto literal das opções |
| Definidor/renderizador | `app/templates/chamados/partials/painel_chamado_turno.html:76` | Texto literal das opções |
| Leitor | `app/chamados/services/verbos.py:138` | Truthiness/normalização em `:140`, `.get` em `:141` |
| Leitor | `app/chamados/templatetags/chamados_tags.py:53` | Filtro registrado, import e delegação em `:63`/`:64` |
| Leitor | `app/templates/chamados/detalhe.html:36` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/detalhe_local.html:68` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/fio_colaborador.html:188` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/partials/painel_chamado_turno.html:18` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/partials/modal_fio.html:101` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/detalhe.html:132` | Constantes → `espera|first`, opções |
| Leitor | `app/templates/chamados/detalhe_local.html:395` | Constantes → `espera|first`, opções |
| Leitor | `app/templates/chamados/fio_colaborador.html:268` | Constantes → `espera|first`, opções |

Método: grep de símbolos/filtro/opções em Python, HTML e JS, mais AST dos Python com imports apelidados. O carregamento do mapa e a chamada da função estão em `app/chamados/services/verbos.py:141` e `app/chamados/templatetags/chamados_tags.py:64`. Não apareceram outros aliases, leitores em commands/crons/signals ou leituras textuais desses seletores no JS. [não verificado por referência negativa arquivo:linha]

`rotulo_de_linha` responde outra pergunta — palavra da fila administrativa — e não foi contado como fornecedor do texto desses seletores (`app/chamados/services/verbos.py:96`, `app/chamados/services/verbos.py:125`).

**(d) Casa:** **COMPLEMENTO de O137 — ESMERIL-NO-RASTRO**, classe “testemunha recalcula/rótulo não migrou”; a ligação está explicitada no filtro e no teste dessa família (`app/docs/BACKLOG.md:1363`, `app/chamados/templatetags/chamados_tags.py:55`, `app/chamados/tests/test_o137_esmeril_chamado.py:11`). Não é BO de sítio único: são três seletores e dois blocos literais de referência, enumerados em (c).

**(e) Padrão:** **Single Source of Truth / fonte única**, termo existente, aplicado ao vocabulário das ações; VERBO e ESPERA cont

[truncado em 20000 de 22188 caracteres]

## 10/11 19:54 · sessao 2f5571a09655 · - · - · origem=Ronald (tmux)

Confirmo **estado novo: não / muda decisor local: sim**: o explicador decide hoje por `not _fio_vivo(ps)`; a cura faria esse bool consumir C7, que também exige ausência de respostas sem veredito (`app/chamados/services/explicador.py:136`, `app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).

Para contar **todas as ocorrências semânticas do bool**, use **7 = 1 escritor + 6 leitores**:

| Papel/forma | Sítio |
|---|---|
| Escritor: atribuição de `pode` | `app/chamados/services/explicador.py:137` |
| Leitor interno: condição do ternário | `app/chamados/services/explicador.py:138` |
| Leitor interno: truthiness no `if` | `app/chamados/services/explicador.py:139` |
| Leitor interno: valor exportado na chave `pode_fechar` | `app/chamados/services/explicador.py:163` |
| Leitor externo: `dict.get`, truthiness | `app/chamados/services/desenlace.py:63` |
| Leitor externo: truthiness em template | `app/templates/chamados/partials/modal_fio.html:434` |
| Leitor externo: truthiness em template | `app/templates/chamados/partials/modal_fio.html:445` |

A contagem **4** seria somente **1 produtor + 3 consumidores externos**, omitindo as leituras locais; prefiro **7** para atender literalmente ao censo (`app/chamados/services/explicador.py:137`, `app/chamados/services/explicador.py:138`, `app/chamados/services/explicador.py:139`, `app/chamados/services/explicador.py:163`).

O transporte não cria decisão: o retorno entra no cache e depois em `ch.explicacao`; o alias `desenlace as _desen` recebe o mesmo dict (`app/chamados/services/fio.py:312`, `app/chamados/services/fio.py:314`, `app/chamados/services/fio.py:386`, `app/chamados/services/fio.py:403`, `app/chamados/services/fio.py:404`). Os caminhos completo e parcial passam listas com explicação ao modal (`app/chamados/views.py:889`, `app/chamados/views.py:927`, `app/chamados/services/fio.py:446`, `app/chamados/services/fio.py:495`).

Grep global de `pode_fechar`, `por_que_nao_fecha`, `.explicacao` e `explicacao=`, excluindo testes/migrations, seguido dos aliases e transportes acima, não revelou outros consumidores do bool; a busca específica em `app/static/**/*.js` retornou zero ocorrências [não verificado por arquivo:linha negativo]. Os 25 sítios de chamadas de carimbo/C7 constituem outro recorte e não devem ser somados ao total **7** de (c) (`app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).

## 10/11 19:54 · sessao 235eacb48a70 · - · - · origem=Ronald (tmux)

Confirmado: **não cria estado / sim, troca quem fornece o rótulo**, preservando o juiz da transição (`app/chamados/models.py:123`; `app/chamados/services/acoes_chamado.py:315`).

Os **13 sítios** abrangem o texto visível de espera e dos seletores. Os dois blocos literais contam uma vez cada, como definidor/renderizador (`app/templates/chamados/partials/modal_fio.html:667`; `app/templates/chamados/partials/painel_chamado_turno.html:76`). `rotulo_de_linha` responde outra pergunta — palavra da fila do administrador — e não fornece texto aos seletores (`app/chamados/services/verbos.py:125`).

Grep de JS estático e embutido não encontrou leitura textual desses seletores. Os candidatos examinados leem outros controles: `select[name=escape]` e `data-doc` (`app/templates/chamados/meu_atendimento.html:373`), ou `para-colab-tipo` e `data-pedeDia` (`app/templates/chamados/form_abertura.html:237`). O servidor recebe o `value` pelo nome `novo_status` (`app/chamados/services/acoes_chamado.py:287`). A negativa global, sem artefato gravado da busca, deve carregar **[não verificado]** no relatório.

## 10/11 19:53 · sessao 67ea159aafa1 · - · - · origem=Ronald (tmux)

Correção de **(f)**: para o item amplo, use:

perguntas de lei: 1

**“Quando uma linha BATIDA do acervo sem episódio e uma linha com episódio podem representar o mesmo fato, permitindo que uma retrate a outra?”** O código declara expressamente essa lei aberta em `app/chamados/services/fio.py:1274` e `:1275`; hoje preserva ambas, pelo ramo `sem_episodio_intocada` (`:1294`).

Busquei `sem_episodio_intocada`, `canonica_sem_episodio`, “sem episódio”, “acervo” combinado com “episódio/retrata/identidade” em LEIS, CORTES, CLAUDE e docs. Não encontrei corte que responda essa pergunta; a ausência global de resposta escrita é **[não verificado]**, mas a declaração explícita de pendência está no código (`app/chamados/services/fio.py:1275`).

**Alcance:** depende de dado: quantos dos 486 grupos/848 excedentes contêm essa combinação. O anexo não fornece chave nem pks **[não verificado]**. Essa pergunta não impede medir ou retratar os grupos que o plano já aceita em `app/chamados/services/fio.py:1297`; impede chamar toda perdedora de excedente curável sem classificar as recusas (`:1280`, `:1294`).

O N=0 anterior era válido somente para o subconjunto já aceito pela porta, não para todo o item genérico.

## 10/11 19:53 · sessao 235eacb48a70 · - · - · origem=Ronald (tmux)

## BO-ROTULO-ENCERRADO

**(a) Lei, juiz e porta.** A regra aplicável é “testemunha lê, não recalcula” (`CLAUDE.md:20`), acompanhada de tela e ação consultarem a mesma função (`app/docs/LEIS-UI.md:23`) e textos explicativos saírem do mapa do domínio (`app/docs/LEIS-UI.md:77`). Não encontrei uma lei literal “rótulo único por estado”; a fonte atual distingue duas perguntas: ação do administrador e espera do colaborador (`app/chamados/services/verbos.py:96`).

Para **rótulo de ação**, **sem juiz registrado**: o registro da família chamado enumera transição, premissa, permissão, dia, perguntas e demais autoridades, sem essa pergunta (`app/core/juizes.py:272` até `:293`). Hoje decidem dois blocos literais: `app/templates/chamados/partials/modal_fio.html:667` e `app/templates/chamados/partials/painel_chamado_turno.html:76`. Para o rótulo de espera, decide `rotulo_para_pessoa`, através do filtro `espera` (`app/chamados/templatetags/chamados_tags.py:53`, `app/chamados/services/verbos.py:138`).

A transição permanece com o juiz registrado `motor.decidir` (`app/core/juizes.py:274`, `app/docs/JUIZES.md:37`), acessado por `ChamadoColaborador.transicionar`/`pode_transicionar` (`app/chamados/models.py:119`, `:128`); seu escritor canônico é `_gravar_estado` (`app/chamados/models.py:201`).

**Conferência do relato.** Confere: os dois detalhes mostram “Encerrado” nas opções `resolvido` e `fechado`, porque usam `espera|first` (`app/templates/chamados/detalhe.html:133`, `:134`; `app/templates/chamados/detalhe_local.html:396`, `:397`) e o mapa traduz ambos assim (`app/chamados/services/verbos.py:118`, `:119`). O mesmo defeito aparece também em um terceiro seletor, não mencionado no relato (`app/templates/chamados/fio_colaborador.html:269`, `:270`). Confere que o modal distingue “Encerrar”/“Encerrar e arquivar” (`app/templates/chamados/partials/modal_fio.html:668`, `:669`).

Não confere a sugestão de uma **fonte comum já existente no modal**: os rótulos são literais no template; outro template repete os mesmos literais (`app/templates/chamados/partials/modal_fio.html:667`, `app/templates/chamados/partials/painel_chamado_turno.html:76`). O próprio comentário esclarece que ali se mostra **ação**, mantendo `value` como contrato com a view (`app/templates/chamados/partials/modal_fio.html:660`). O chamado de exemplo não pode ser conferido: depende de dado — contagem de ocorrências e renderização do pk citado em cada rota. [não verificado]

**(b)** Cria estado: **não**, para a cura limitada a rótulos: `resolvido` e `fechado` já são valores do campo existente (`app/chamados/models.py:31`, `:32`, `:52`), e os seletores já enviam esses mesmos valores (`app/templates/chamados/detalhe_local.html:396`, `:397`). Muda decisor de negócio: **não**, nesse escopo; a ação recebe `novo_status` e chama a mesma porta (`app/chamados/services/acoes_chamado.py:287`, `:313`, `:315`), cuja validação delega ao juiz atual (`app/chamados/models.py:123`). Centralizar o vocabulário muda sua fonte de apresentação; não autoriza mudar o julgamento da transição (`app/chamados/catalogo/motor.py:139`).

**(c) Censo relevante ao rótulo; censo global do estado dispensado por (b).** Unidade: função ou bloco de template; opções irmãs contam como um bloco. **13 sítios = 3 definidores + 10 leitores**:

| Tipo | Sítio | Forma |
|---|---|---|
| Definidor | `app/chamados/services/verbos.py:114` | dicionário `ESPERA_COLAB`; valores `resolvido`/`fechado` iguais |
| Definidor/renderizador | `app/templates/chamados/partials/modal_fio.html:667` | opções com texto literal e `value` de estado |
| Definidor/renderizador | `app/templates/chamados/partials/painel_chamado_turno.html:76` | opções com texto literal e `value` de estado |
| Leitor | `app/chamados/services/verbos.py:138` | normalização por truthiness em `:140`; `.get(st, fallback)` em `:141` |
| Leitor | `app/chamados/templatetags/chamados_tags.py:53` | filtro registrado; import local e delegação em `:63`/`:64` |
| Leitor | `app/templates/chamados/detalhe.html:36` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/detalhe_local.html:68` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/fio_colaborador.html:188` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/partials/painel_chamado_turno.html:18` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/partials/modal_fio.html:101` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/detalhe.html:132` | constantes de estado → `espera|first` |
| Leitor | `app/templates/chamados/detalhe_local.html:395` | constantes de estado → `espera|first` |
| Leitor | `app/templates/chamados/fio_colaborador.html:268` | constantes de estado → `espera|first` |

Método executado: grep global em `.py`, `.html`, `.js` por `rotulo_para_pessoa`, `ESPERA_COLAB`, `|espera` e opções; AST dos Python de produção, com resolução de imports `as`. Encontrados um carregamento Python do mapa (`app/chamados/services/verbos.py:141`) e uma chamada da função (`app/chamados/templatetags/chamados_tags.py:64`). Nenhum outro alias/chamador foi encontrado; commands, `config/crons.py`, signals e JS de static não produziram ocorrências desses símbolos. Como prova negativa de execução sem artefato gravado, o resultado das buscas fica [não verificado] para quem só disponha deste relatório.

**(d)** **COMPLEMENTO de O137 — ESMERIL-NO-RASTRO**, especificamente a classe testemunha/rótulo que não migrou, já descrita nessa casa (`app/docs/BACKLOG.md:1363`). A ligação é explícita no teste da família chamado: a classe B centraliza a pílula de espera em `rotulo_para_pessoa` (`app/chamados/tests/test_o137_esmeril_chamado.py:11`), e o próprio filtro identifica O137 (`app/chamados/templatetags/chamados_tags.py:55`). Não há prova de BO de sítio único: são três seletores afetados e dois modelos literais de ação (`app/templates/chamados/detalhe.html:132`, `app/templates/chamados/detalhe_local.html:395`, `app/templates/chamados/fio_colaborador.html:268`, `app/templates/chamados/partials/modal_fio.html:667`, `app/templates/chamados/partials/painel_chamado_turno.html:76`). Esse encaixe não afirma que a condição atual de execução da O137 esteja satisfeita; a casa exige família fechada (`app/docs/BACKLOG.md:1363`).

**(e)** **Single Source of Truth / Aggregate Root — fonte única**, termo existente (`app/docs/GLOSSARIO.md:11`), aplicado ao vocabulário da ação, mantendo separado o vocabulário de espera já declarado (`app/chamados/services/verbos.py:96`).

**(f)** perguntas de lei: 0

No escopo limitado de apresentar os verbos já existentes e preservar `value`, a distinção ação/espera já está documentada (`app/templates/chamados/partials/modal_fio.html:660`, `app/chamados/services/verbos.py:96`); a obrigação de fonte única já existe (`CLAUDE.md:20`). Se se pretender registrar uma nova autoridade de domínio, aplica-se a regra de declaração prévia, assunto que este censo não autoriza construir (`CLAUDE.md:311`).

**Linha para tabela consolidada:**  
`BO-ROTULO-ENCERRADO | CLAUDE:20; LEIS-UI L2/L7; sem juiz registrado do rótulo; motor.decidir/transicionar/_gravar_estado | não/não | 13 (perímetro do rótulo; estado global n/a) | COMPLEMENTO de O137 (BACKLOG:1363) | Single Source of Truth (GLOSSARIO:11) | 0`

## 10/11 19:53 · sessao 67ea159aafa1 · - · - · origem=Ronald (tmux)

## PERGUNTA-DUPLICATA-1110

**(a)** Aplicam-se L-001 (origem), L-002 (mesma autoridade), L-003 (medir função real), L-007 (escritor e trilha), em `app/docs/LEIS.md:59`, `:60`, `:61`, `:65`, e idempotência estrutural de `CLAUDE.md:175`.

**Sem juiz registrado para “qual pergunta é duplicata / qual canônica fica”**: C8 pergunta pelo **chamado**, em `app/core/juizes.py:114`, e aponta para `ChamadoColaborador.abrir`, em `app/core/juizes.py:280`; a família chamado registrada termina em `app/core/juizes.py:294`, sem a eleição de pergunta. O lookup FIO aponta para `_disputa_do_chamado`, outra pergunta (`app/docs/JUIZES.md:40`).

Quem decide hoje:

- Identidade MARCO: colaborador/dia/motivo/instância; BATIDA: episódio/motivo, em `app/chamados/services/fio.py:1372`.
- Canônica: primeira por pk com resposta não vazia (`(resposta_colab or '').strip()`); senão menor pk, em `app/chamados/services/fio.py:1225`.
- Retratação permitida: `plano_de_retratacao`, em `app/chamados/services/fio.py:1259`, preserva mérito com resposta, episódios distintos e acervo sem episódio (`:1287`, `:1290`, `:1292`, `:1294`).
- Porta: `retratar_para_canonica` (`app/chamados/services/fio.py:1319`) → `retratar_duplicata` (`:1345`) → `carimbar_identidade` (`app/chamados/models.py:3168`, save `:3190`). Nada se apaga; a referência à canônica entra em `recusa_motivo` (`app/chamados/services/fio.py:1359`).

**(b)** Para a cura mínima compatível com o relato — medir corretamente e retratar excedentes pela porta existente — **cria estado: não; muda decisor: não**. O valor `duplicata` já existe (`app/chamados/models.py:2495`), a porta já grava via/carimbo/autor/motivo (`:3182`, `:3186`, `:3190`), e a eleição já é compartilhada inclusive pelo nascimento, via import apelidado `_canonica` (`:2905`, `:2921`). A presença efetiva desse estado no banco de produção permanece **[não verificado]**.

Construir um **novo fato próprio de identidade** seria outra obra: `FIO-E0-V4-FATO-A06`, posterior à E4d, registrada em `app/docs/BACKLOG.md:1867`; não se deve presumir essa ampliação de escopo.

**(c)** Como (b) é não/não, o censo global dos campos não é exigido pela condição do pedido. Para não apresentar “zero leitores”, contei o **perímetro das cinco funções de dedup acima** por AST em todos os `.py` da cópia, excluindo testes/migrations; resolvi `ImportFrom.asname`, incluindo `_canonica`. Resultado: **23 chamadas: 19 leituras de decisão e 4 delegações de escrita**, não 23 leitores/escritores globais dos campos. [nao verificado]

| Arquivo | Sítios e forma |
|---|---|
| `app/chamados/management/commands/backfill_decisoes_pergunta.py` | `:276` eleição; `:290` plano, categorias por truthiness em `:295`; `:331` delegação de escrita; `:393` eleição condicionada a `len>=2` em `:392`; `:396` leitura `['retrata']`; `:485` grupos; `:506` eleição/comparação com menor pk em `:507`; `:510` plano/contagem por `len` em `:512`. |
| `app/chamados/management/commands/dedup_perguntas_fato.py` | `:58` grupos com recorte; `:76` eleição; `:81` plano, leitura `['retrata']` em `:88` e truthiness em `:89`. |
| `app/chamados/models.py` | `:2921` eleição por alias `_canonica`, truthiness da lista e depois `is not None` em `:2922`. |
| `app/chamados/services/declaracao_texto.py` | `:253` eleição, comparação disputa e `_reusavel` em `:254`; `:275` eleição depois de truthiness da lista em `:271`. |
| `app/chamados/services/fio.py` | `:921` delegação de escrita sob truthiness em `:920`; `:956` eleição/comparação pk em `:957`; `:967` plano/`any` categorias em `:968`; `:1340` plano; `:1341` delegação de escrita em compreensão com truthiness do retorno; `:1832` eleição condicionada a sementes em `:1833`; `:2155` eleição/comparação pk em `:2165`; `:2158` plano/truthiness das recusas em `:2166`; `:2180` delegação de escrita/leitura `['retratadas']`. |

O escritor físico subsequente é `app/chamados/models.py:3190`, chamado de `app/chamados/services/fio.py:1359`; fica **fora das 23 chamadas**, cujo perímetro são os cinco símbolos. O grep desses símbolos em HTML/JS/signals não encontrou ocorrências; isso não prova ausência de leitores dos campos por outras formas **[não verificado]**. `app/config/crons.py:783` menciona o backfill somente na declaração de papel de ato único (`:780`), não como chamada executável.

Leituras relevantes para medir eventual inflação: C6 compara a via com `VIAS_QUE_VENCEM_A_RESPOSTA` e exige carimbo não nulo (`app/chamados/juizes.py:1336`); o queryset usa `via_resolucao__in` + `materializada_em__isnull=False` (`app/chamados/models.py:3352`) e os exclui em `:3359`; o app filtra `q_vivas` (`app/chamados/services/pergunta_no_app.py:50`), confirma C6 em `:58`, e separa pendentes por `respondida_em is None` e `fechada_em is None` em `:71`.

**(d)** **COMPLEMENTO de FIO-E0-V4 E1**, identidade e backfill do acervo, casa `app/docs/BACKLOG.md:1848`; articulado à migração dos escritores E4 (`:1863`). Há ainda resíduo específico de duplicatas sobre episódio retratado em `:786`; não há dado fornecido que permita dizer que os 486 grupos incluem esse resíduo **[não verificado]**. Não é BO simples: as 23 chamadas acima atravessam modelo, serviços e dois commands.

**(e)** Padrão direto: **idempotência pela chave do fato**, termo em `app/docs/GLOSSARIO.md:29`; fonte única/Aggregate Root consta em `:11`, e escritor único em `:12`. A identidade já é compartilhada pelo nascimento e pelo dedup (`app/chamados/models.py:2905`, `app/chamados/services/fio.py:1372`).

**Conferência do relato:** “486 grupos / 848 excedentes em disputa aberta” é dado fornecido, sem chave de agrupamento nem pks; sua equivalência ao universo canônico é **[não verificado]**. “Disputa aberta” não define duplicidade: `do_fato` usa colaborador/família fora das vias terminais (`app/chamados/models.py:2717`, `:2724`), enquanto conversa nasce sempre, sem dedup (`:2794`). O agrupador real exclui conversa e vias fora do fato (`app/chamados/services/fio.py:1431`), aplica chaves por família (`:1945`) e exige grupo com membro no horizonte (`:1949`); nem toda perdedora é retratável (`:1290`).

**Depende de dado:** contagem dos 486 grupos reclassificados pelo universo real; excedentes retratáveis versus mérito/episódios unidos/acervo sem episódio; distribuição por idade, horizonte e competência; quantos excedentes passam por C6 e efetivamente entram nos contadores/app. Os 848, isoladamente, não demonstram inflação **[não verificado]**.

perguntas de lei: 0

Para o escopo mínimo, a eleição e as recusas já têm regra explícita (`app/chamados/services/fio.py:1225`, `:1259`); registrar a autoridade existente é pendência de registro, não uma nova decisão de mérito. A união de acervo sem episódio com episódio está expressamente preservada e anotada como lei aberta (`:1274`), mas não foi demonstrado que o dado deste item exige resolvê-la **[não verificado]**.

**Linha de tabela sugerida:** `PERGUNTA-DUPLICATA-1110 | L-001/002/003/007; sem juiz registrado de identidade de pergunta; fio.retratar_para_canonica | não/não (escopo mínimo) | 23 chamadas do perímetro (19 leitura/4 delegação), não censo global | COMPLEMENTO FIO-E0-V4 E1/E4 | idempotência pela chave do fato | 0`.

## 10/11 19:53 · sessao 2f5571a09655 · - · - · origem=Ronald (tmux)

Para **DISPUTA-ZUMBI-1110**, a cura mínima é completar a costura dos carimbos com o juiz e a porta existentes; isso não exige estado novo nem juiz novo (`app/chamados/models.py:1848`, `app/chamados/models.py:1932`).

**(a) Lei/juiz/porta.** L-002 obriga testemunhas a lerem a mesma autoridade; L-007 exige escritor canônico; CLAUDE 4b registra um juiz por pergunta e um escritor por entidade (`app/docs/LEIS.md:60`, `app/docs/LEIS.md:65`, `CLAUDE.md:175`). O corte C7 define `DisputaSupervisao.pode_fechar` como juiz único; registro vivo `_C7`, confirmado pelo lookup (`app/docs/CORTES.md:272`, `app/core/juizes.py:279`, `app/docs/JUIZES.md:57`). O juiz responde **nenhuma resposta sem veredito E nenhum fio vivo** (`app/chamados/models.py:1853`). A porta `DisputaSupervisao.fechar` grava fechamento, autor, resolução e motivo conjuntamente (`app/chamados/models.py:1932`, `app/chamados/models.py:1936`). Os carimbos existentes saem de `PerguntaDisputa.carimbar`/`carimbar_identidade` (`app/chamados/models.py:3103`, `app/chamados/models.py:3168`).

**(b) Estado novo: não; muda quem decide: não**, limitado a completar chamadas ao C7/`fechar`, preservando esses contratos. Os campos e valores de fechamento já existem e a porta já os escreve; C7 já é registrado (`app/chamados/models.py:1757`, `app/chamados/models.py:1780`, `app/chamados/models.py:1932`, `app/core/juizes.py:279`). Confirmar quais combinações existem em produção depende de dado: contagem de disputas por `fechada_em × motivo_fechamento × via_resolucao das perguntas` [não verificado].

**(c) Censo global de estado não acionado por (b).** Para demonstrar a costura, contei nós `ast.Call` cujo método é `carimbar`, `carimbar_identidade` ou `pode_fechar`, em `app/**/*.py`, excluindo diretórios `tests` e `migrations`; esse recorte conta **13 chamadas de carimbo em 12 funções + 12 chamadas ao C7**, sem depender do nome da variável receptora (`app/chamados/models.py:3287`, `app/chamados/services/disputa_emissao.py:1863`, `app/chamados/services/disputa_emissao.py:1937`, referências completas abaixo).

O rol de carimbadores é:

| Forma | Sítios |
|---|---|
| `.carimbar(...)`, 8 chamadas | `app/chamados/models.py:3287`; `app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:595`; `app/chamados/services/disputa_emissao.py:1863`; `app/chamados/services/disputa_emissao.py:1937`; `app/chamados/services/materializacao.py:561`; `app/chamados/services/materializacao.py:1277`; `app/chamados/services/materializacao.py:1431` |
| `.carimbar_identidade(...)`, 5 chamadas | `app/chamados/models.py:2061`; `app/chamados/models.py:3064`; `app/chamados/services/fio.py:1359`; `app/chamados/services/materializacao.py:924`; `app/chamados/services/materializacao.py:1099` |

O rol de leitores diretos de C7 é:

| Forma | Sítios |
|---|---|
| `fechada_em is None and ...pode_fechar()` | `app/chamados/reconciliador.py:411`; `app/chamados/reconciliador.py:611`; `app/chamados/services/becos.py:56`; `app/chamados/services/disputa_emissao.py:1963`; `app/chamados/services/materializacao.py:585`; `app/chamados/services/materializacao.py:1316` |
| `elif ...pode_fechar()` | `app/chamados/reconciliador.py:1170` |
| ternário por bool | `app/chamados/services/becos.py:33` |
| `if not ...pode_fechar()` | `app/chamados/services/competencia_trancada.py:153`; `app/chamados/services/competencia_trancada.py:202` |
| atribuição e teste posterior | `app/chamados/services/disputa_emissao.py:1278`; `app/chamados/services/disputa_emissao.py:1282` |
| `if disp and disp.pode_fechar()` | `app/ponto/services/entrada_sincronizada.py:58` |

O explicador tem **1 produtor de bool e 3 leituras diretas**: importa `fio_vivo as _fio_vivo` e calcula apenas `not _fio_vivo(ps)`; os dois templates usam truthiness e `desenlace` usa truthiness de `dict.get` (`app/chamados/services/explicador.py:136`, `app/chamados/services/explicador.py:137`, `app/templates/chamados/partials/modal_fio.html:434`, `app/templates/chamados/partials/modal_fio.html:445`, `app/chamados/services/desenlace.py:63`). O transporte/cache passa por `fio.py`, sem alterar esse bool (`app/chamados/services/fio.py:312`, `app/chamados/services/fio.py:314`). **Total deste recorte: 29 sítios = 13 + 12 + 1 + 3**; não apresentar como censo global de `fechada_em` ou `materializada_em` (referências dos quatro grupos acima).

**Conferência do relato.**

- `lavrar_sem_turno_aberto` já consulta C7 e chama `fechar`: confirmado (`app/chamados/reconciliador.py:411`, `app/chamados/reconciliador.py:412`).
- “Explicador sem a trava do C7”: confirmado especificamente quanto a `respostas_sem_veredito`; ele só nega `fio_vivo`, enquanto C7 exige ambos (`app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).
- “Fechamento só por beco”: **não confere como afirmação global**; há fechamento após carimbo fora desse recorte, por exemplo reconciliação de órfãs e materialização (`app/chamados/reconciliador.py:1170`, `app/chamados/services/materializacao.py:1316`).
- O evento genérico após `PerguntaDisputa.save` chama a porta de becos; ela exige pai terminal e ausência de perguntas visíveis antes de consultar C7 (`app/chamados/signals.py:227`, `app/chamados/signals.py:238`, `app/chamados/services/becos.py:29`, `app/chamados/services/becos.py:31`, `app/chamados/services/becos.py:33`). O cron está agendado com `--alarme`; `--apply` continua opcional no comando (`app/config/crons.py:663`, `app/chamados/management/commands/fechar_becos_disputa.py:23`, `app/chamados/management/commands/fechar_becos_disputa.py:30`).
- “7 carimbadores sem C7”: **não estabelecido como sete caminhos sem fechamento**. Grep direto de cada corpo não basta: `encerrar_fantasma` tem cauda C7 no reconciliador; `_materializar_uma` tem caudas compartilhadas; `remover_motivo` chama diretamente a porta de fechamento (`app/chamados/reconciliador.py:1147`, `app/chamados/reconciliador.py:1170`, `app/chamados/services/materializacao.py:1378`, `app/chamados/services/passivo_validado.py:405`, `app/chamados/models.py:2072`).
- Os dez casos, data do carimbo e eventual recusa dependem de dado: contagem por `via_resolucao × materializada_em antes/depois de 22/10 × estado do pai × respostas_sem_veredito × C7 atual`, e trilha de tentativas/recusas [não verificado]. A autoria histórica do commit citado no anexo não foi verificada [não verificado]. [nao verificado]

**(d) Casa.** **COMPLEMENTO de C7-DISPUTA-FECHA-POR-UM-JUIZ**, cuja linha manda migrar regras espalhadas e usar C6/C7; interseção com série E4 para os escritores e E3 para apresentação (`app/docs/BACKLOG.md:1468`, `app/docs/BACKLOG.md:1863`, `app/docs/BACKLOG.md:1862`). Não cabe provar “um sítio só”: o recorte acima tem múltiplas portas e leitores (referências das tabelas).

**(e) Padrão.** **Single Writer Principle / juiz único**, termo literal do glossário, realizado ao fazer os chamadores e o explicador consumirem C7/porta existentes (`app/docs/GLOSSARIO.md:12`, `app/core/juizes.py:279`). Aggregate Root também existe no glossário, mas o sítio que a tabela associa a esse termo é `CelulaDia`; o defeito comprovado aqui é de consumo incompleto do juiz de fechamento (`app/docs/GLOSSARIO.md:11`, `app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).

perguntas de lei: 0

O critério de fechamento e sua guarda de resposta sem veredito já estão escritos; falta de contagem histórica é pergunta de dado (`app/docs/CORTES.md:272`, `app/chamados/models.py:1853`).

## 10/11 19:50 · sessao 32814039580f · MAPA-FILA-BO · censo · origem=Code

São **quatro IDs únicos**; o noturno aparece duas vezes, inclusive na árvore (`app/docs/BACKLOG.md:1814`, `app/docs/BACKLOG.md:1874`). A identidade Git do HEAD informado é **[nao verificado]**: esta cópia não contém `.git`. Não consultei banco nem logs **[nao verificado por arquivo:linha]**.

Nos inventários abaixo, cada linha indica o arquivo; os números após cada função são linhas desse mesmo arquivo. A contagem usa funções ou templates distintos, deduplicando múltiplas leituras na mesma função. Testes não entram como consumidores de produção; migrations executáveis entram separadamente. O levantamento foi por AST, grep e leitura dos sítios **[nao verificado por arquivo:linha quanto à execução do levantamento]**. [nao verificado]

## BUG-PLANTIO-NOTURNO-MAIS-1

**(a) Lei, juiz e porta.** Aplicam-se a fonte única e o escritor único — L-002 e L-007 (`app/docs/LEIS.md:60`, `app/docs/LEIS.md:65`) —, a célula como fonte do previsto (`CLAUDE.md:145`) e a escrita de batida pela porta L-030 (`app/docs/LEIS.md:78`).

O juiz registrado do dia da batida é `turnos_do_colab` (`app/core/juizes.py:208`); a consulta também aponta a geometria e os marcos encadeados (`app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:24`). `DiaDoTurno` apenas nomeia a resposta dessa autoridade: é `NewType`, não campo persistido nem juiz novo (`app/core/dia_do_turno.py:3`, `app/core/dia_do_turno.py:13`). Os adaptadores existentes são `dia_do_turno_da_batida` e `dia_do_turno_da_celula` (`app/ponto/turnos.py:2927`, `app/ponto/turnos.py:2939`).

As portas são:

- Nascimento: `PerguntaDisputa.abrir` (`app/chamados/models.py:2776`).
- Re-chaveamento do passivo: `rechavear_data_evento` (`app/chamados/services/fio.py:979`).
- Atribuição única de `data_evento`: `gravar_dia_do_fato` (`app/chamados/services/fio.py:800`).
- Plantio de batida: chamada de `registrar_batida` pelo materializador (`app/chamados/services/materializacao.py:926`).

**Conferência do relato.** A afirmação de que o emissor ainda grava necessariamente o dia da cobrança **não descreve o código atual**: o nascimento exige dia explícito, e o ajuste usa esse dia ou o seguinte; sem marcos, devolve o timestamp recebido (`app/chamados/models.py:2783`, `app/chamados/models.py:2787`, `app/chamados/juizes.py:324`, `app/chamados/juizes.py:329`). A conversa tem porta separada e conserva dia civil; portanto também seria incorreto afirmar que **todo** `data_evento` significa dia do turno (`app/chamados/models.py:2796`, `app/chamados/models.py:2801`).

Já existem construtor, porta de re-chaveamento e command de migração nesta cópia; “ainda sem construtor” não confere para este ID (`app/chamados/models.py:2776`, `app/chamados/services/fio.py:979`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:179`). Aplicação desse passivo em produção e incidência atual de D+2 são **[nao verificado]**.

**(b) Cria estado: sim, no sentido amplo de novo valor/composição por linha migrável.** Não acrescenta coluna: regrava `data_evento`, troca `celula_id`, relavra e persiste os três campos da lavra (`app/chamados/services/fio.py:1037`, `app/chamados/services/fio.py:1038`, `app/chamados/services/fio.py:1042`, `app/chamados/services/fio.py:1043`). A célula de destino precisa existir; essa porta não cria uma célula nova (`app/chamados/services/fio.py:1034`, `app/chamados/services/fio.py:1035`). A existência ou ausência dessas combinações no banco real é **[nao verificado]**.

**Muda decisor: não.** O novo dia continua vindo do juiz do turno; o escritor apenas recebe e atribui a resposta (`app/chamados/services/fio.py:794`, `app/chamados/services/fio.py:984`, `app/ponto/turnos.py:2935`).

**(c) Censo do estado tocado.** O recorte é `PerguntaDisputa.{data_evento, celula, veredito_lavrado, julgado_em, janela_expira_em}`, os campos efetivamente salvos pelo re-chaveamento (`app/chamados/services/fio.py:1043`).

Foram inventariadas **155 funções distintas** na união dos três levantamentos: 117 de dia, 52 de FK e 21 de lavra, com sobreposição removida. Acrescentam-se duas funções executáveis da migration, dois consumidores de tupla/evento e três templates: **162 sítios de aplicação**. Há ainda três mecanismos declarados do banco: **165 sítios na contagem ampliada**, mais quatro encaminhadores discriminados abaixo. A contagem é de acessos, escritas e portas estáticas; **não é contagem de todos os chamadores transitivos dessas funções** **[nao verificado quanto a acessos dinâmicos não identificados]**.

**Dia — 117 funções.** “Porta” abaixo significa fornecimento do valor para uma escrita canônica; não significa atribuição direta adicional.

| Arquivo | Funções, linhas e forma |
|---|---|
| `app/api/views.py:1183` | `api_chamados_v2` :1183–1184: truthiness, ISO e rótulo; `api_disputa_responder` :2655: mensagem. |
| `app/api/views_mensageria.py:568` | `_item_validar`: truthiness e ISO para JSON. |
| `app/chamados/canal.py:65` | `perguntas_mudas_com_resposta`: filtro `isnull=False` :65, ordenação :67, chave :72, payload :79. |
| `app/chamados/juizes.py:80` | `_dia_da_disputa` :80–81: truthiness/lista; `dias_do_fato` :214: truthiness; `_plantio_na_hora_do_slot` :271: fallback `or`; `_dia_de_turno_da_pergunta` :336: `getattr`, conversa→`None`; `_celulas_da_pergunta` :637: `getattr`; `_chave_memo_pv` :831: chave de memo; `_marco_esperado_da_grade` :1725: leitura; `_dias_das_perguntas_vivas` :2264–2268: filtro `isnull=False` e lista. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:63` | `_candidatos_i8` :63/:73/:74: truthiness e filtros de data; `_contraprova_i8bx` :148: intervalo `<=`; `_cobre_ausencia_i21` :160/:162: truthiness e argumento do juiz; `_piso_f3_antes_do_gerador` :282: `<`; `_contraprova_f3g` :298: filtro `lte` e `.exists()`; `contraprova_piso` :325/:326/:348/:361/:373: chaves, truthiness, filtro de vínculo e `.exists()`, intervalo; `handle` :892/:914/:926/:930/:957/:1665/:1708/:1778/:1779/:1804: filtros `isnull`, intervalos, `.exists()`, truthiness, igualdade ORM e argumento de juiz. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:60` | `linhas_a_criar` :60: `.only`; `_escoar` :69: chave; `admin_decidiu_sem_ato` :104: filtro `isnull`; `competencia_de` :115/:118/:120: `is None`, chave e argumento; `aplicar` :282–283: `is None`/`is not None`; `_grupos_fora` :371: `values_list`. |
| `app/chamados/management/commands/backfill_veredito_lavrado.py:111` | `handle`: argumento de `expira_em`. |
| `app/chamados/management/commands/dedup_perguntas_fato.py:85` | `handle` :85–86: `is not None`/`is None`. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:74` | `handle`: mensagem. |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:97` | `_fonte_viva` :97/:100/:102/:109/:113/:118: leitura e comparações; `classificar` :144/:148/:158/:163: janela, payload, desigualdade e igualdade; `handle` :205/:209/:211: filtros `isnull`, `gte`, `lte`; `_veto_do_grupo` :309: `values_list`; `_escrever_reversao` :354: JSON; `_migrar` :392: `values_list`; `_descarimbados` :442/:450: `Q` por igualdade e chave; `_reverter` :481/:488: JSON e argumento de veto. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:68` | `handle` :68/:86/:91/:102/:117/:144/:145: filtro `isnull=False`, janela, mensagens e competência. |
| `app/chamados/models.py:364` | `_vivas_entre` :364: argumento e resultado `is not False`; `_plano_pergunta_viva` :390–391: argumento/truthiness; `q_dos_fatos` :2753: `Q` igualdade; `abrir` :2789 e `abrir_conversa` :2801: portas de escrita; `_nascer` :2940/:2956/:2962: desigualdade, mensagem e captura; `linhas_do_evento` :3228: `values_list`. |
| `app/chamados/reconciliador.py:370` | `lavrar_sem_turno_aberto` :370: leitura; `_dia_fora_do_alcance_c` :736: `getattr`/`is None`; `reemitir_chamado_do_dia` :954: porta de escrita; `_orfas_abertas` :1050: filtro `isnull`; `veredito_orfa` :1082/:1088/:1090: `is None`, `>=`, argumento de juiz; `encerrar_orfas_pela_lampada` :1152: mensagem; `semear_eventos_celula` :1303/:1305/:1307: conjuntos e igualdade ORM; `reatar_pergunta_orfa` :1557/:1567/:1642/:1655: argumento, truthiness/ISO e portas de escrita. |
| `app/chamados/services/aviso_pendencia.py:16` | `aviso_texto_livre`: truthiness/`strftime`. |
| `app/chamados/services/cobranca.py:83` | `checar_cobranca`: truthiness e conjunto ordenado. |
| `app/chamados/services/competencia_trancada.py:107` | `medir` :107 e `reabrir` :252: filtros `gte/lte`; `reabrir` :256 também interpola mensagem. |
| `app/chamados/services/declaracao_texto.py:370` | `declarar_hora_colab` :370/:377: portas de costura/nascimento; `insumos_em_regularizacao` :590: chave; `aplicar_declaracao` :667: porta de nascimento. |
| `app/chamados/services/disputa_emissao.py:399` | `gerar_perguntas_disputa` :399/:418/:451: igualdade ORM, `exclude` e nascimento; `_criar_pergunta_se_couber` :844: nascimento; `criar_disputa_a_partir_de_chamados_orfaos` :1074: fallback `or`; `_ja_perguntou` :1415: igualdade ORM; `responder_pergunta_disputa_core` :1498/:1531: mensagem e argumento; `_resolver_perguntas_por_chamado_resolvido` :1863/:1935: argumento da autoridade de via. |
| `app/chamados/services/explicador.py:70` | `explicar_pergunta` :70: truthiness/`strftime`; `por_que_nao_fecha` :122/:126: ordenação/truthiness. |
| `app/chamados/services/fabrica_da_celula.py:148` | `motivos_a_perguntar` :148 e `perguntar` :170: igualdade ORM. |
| `app/chamados/services/fio.py:87` | `data_do_fato_de` :87–88: leitura, truthiness/desigualdade; `_costura_nao_mata_o_fio` :776: competência; `gravar_dia_do_fato` :800: atribuição; `reapontar_resolvedora` :867/:934: truthiness, comparação e porta de escrita; `_colisao_ao_datar` :953: assert igualdade; `rechavear_data_evento` :1013/:1037/:1043/:1047/:1048: captura, atribuição pela porta, persistência e trilha; `rechavear_no_reuso` :1068/:1072: veto/mensagem; `reuso_sem_rechave` :1109: igualdade; `violacao_de_saida` :1133/:1136: igualdade/mensagem; `retratar_duplicata` :1362: mensagem; `q_do_horizonte` :1412: `Q(gte)`; `q_do_horizonte_ou_sem_dia` :1419: `Q(isnull)`; `_vizinhos_do_fato` :1447: argumento condicional; `chave_do_dia` :1460: tupla; `_dia_do_fato` :1495/:1497: `is not None` e construtor do tipo; `dia_do_turno_da_linha` :1630: `getattr`/fallback; `universo_do_fato` :1800: igualdade de tupla; `fecho_cruza_dias` :1854: `values_list`; `grupos_de_colisao` :1945/:1946/:1948/:1955/:1958/:1970/:1985: campos dinâmicos, `exclude`, values/order, `OuterRef`, filtro e tupla; `fora_do_indice` :2005: `isnull`; `batida_sem_origem_no_horizonte` :2024: `values_list`. |
| `app/chamados/services/historico.py:108` | `_eventos_dos_chamados`: truthiness/`strftime`. |
| `app/chamados/services/limbo_folgas.py:203` | `filhos_no_defeito`: igualdade ORM. |
| `app/chamados/services/materializacao.py:279` | `_tipo_pelo_marco` :279/:282: truthiness/argumento da grade; `_ts_tipo_do_plantio` :344–345: truthiness/leitura; `_declarar_ausencia_de_pergunta` :473–474: truthiness/leitura; `_materializar_ausencia_de_resposta` :517/:533/:573: leitura, igualdade ORM e payload; `perguntas_absorvidas_por` :624: igualdade ORM; `_materializar_uma` :1265: argumento da autoridade de via. |
| `app/chamados/services/passivo_validado.py:113` | `_dia_do_plantio` :113: fallback `or`; `_plantar_o_dia` :519–520: truthiness/conjunto. |
| `app/chamados/services/pergunta_no_app.py:115` | `retencao`: igualdade e `any`. |
| `app/chamados/services/relavra_via.py:103` | `via_de_hoje` :103: argumento da autoridade; `relavrar_dia` :154: igualdade ORM. |
| `app/chamados/services/tarefa_do_dia.py:233` | `_tarefa_pura` :233: igualdade; `CargaDaTarefa.__init__` :326/:329: filtros `gte/lte` e chave. |
| `app/chamados/services/validacao.py:269` | `_aceitar_a_folga` :269: leitura; `validar_pergunta` :446/:497/:510: recálculo, argumento e mensagem; `conferir_data_do_fato` :671: trilha JSON. |
| `app/chamados/services/vigia_fio.py:816` | `carimbos_sem_lastro_por_dono`: argumento da competência/classificação. |
| `app/chamados/veredito_lavrado.py:273` | `lavrar`: argumento da expiração. |
| `app/chamados/views_cobrar.py:176` | `validar_inline` :176/:185: ordenação ORM e truthiness do payload do canal. |
| `app/colaboradores/services/calendario.py:473` | `contexto_calendario` :473/:477: filtro `range` e chave; modelo por alias `_PD`. |
| `app/folha/previa.py:65` | `montar_previa` :65/:67: filtro `range` e chave. |
| `app/ponto/management/commands/mapa_divergencia.py:272` | `handle` :272–273: filtros `gte/lte`, `.only` e conjunto. |
| `app/ponto/management/commands/vigia_plantio_overnight.py:75` | `batidas_fora_do_turno_da_pergunta` :75/:86/:90: filtro relacionado `pergunta_origem__data_evento__isnull=False`, janela e julgamento. |
| `app/ponto/services/dia_do_colab.py:65` | `dia_do_colab`: igualdade ORM. |
| `app/ponto/services/veredito_celula.py:180` | `fechar_o_fio_do_dia`: igualdade ORM. |

**FK — 52 funções**, incluindo carregamento ORM explícito. Homônimos de `ChamadoColaborador.celula` não foram contados como FK da pergunta; a relação examinada está declarada em `app/chamados/models.py:2414`.

| Arquivo | Funções, linhas e forma |
|---|---|
| `app/chamados/models.py:311` | `_gravar_estado` :311 e `_ancoradas` :346: SELECT relacionado; `_universo_da_ressuscitacao` :1501: leitura/atribuição numa cópia em memória; `do_fato` :2725/:2727: SELECT; `_nascer` :2962/:2965/:2966/:3014: tupla, desigualdade, UPDATE/INSERT; `linhas_do_evento` :3228: `values_list`; `q_vivas` :3355–3358: `Q(isnull)`, `veredito__in` e negações. |
| `app/chamados/juizes.py:82` | `_dia_da_disputa` :82–83: truthiness/FK.data; `dias_do_fato` :212/:215: prefetch/truthiness; `_celulas_da_pergunta` :636–638: `getattr`, `is None`, identidade; `_chave_memo_pv` :818/:832: `getattr`/tupla; `fato_do_marco_no_espelho` :906–907: `getattr`, `is None`/ata; `_resposta_concorda_com_a_luz` :1293: FK→DNA; `_pergunta_viva_calc` :1338–1339: `getattr`, `is not None`, veredito/admin; `alimentar_portas` :1502/:1505: filtro ID `__in`, SELECT e chave; `_dias_das_perguntas_vivas` :2266: SELECT. |
| `app/chamados/reconciliador.py:631` | `qs_perguntas_absolviveis` :631/:637/:639/:641: `isnull=False`, `exclude` vazio/`__in`; `perguntas_absolvidas_pela_celula` :651 e `_orfas_abertas` :1051: SELECT; `encerrar_orfas_pela_lampada` :1155: auditoria; `semear_eventos_celula` :1306: truthiness do ID, FK ou fallback. |
| `app/chamados/relatorios_disputa.py:43` | `coletar`: prefetch `perguntas__celula`. |
| `app/colaboradores/services/calendario.py:474` | `contexto_calendario`: SELECT pelo alias `_PD`. |
| `app/chamados/services/vigia_carimbo.py:100` | `classificar` :100/:103: filtro ID `__in` e SELECT; `censo` :131: ID na amostra. |
| `app/chamados/services/furo_sem_canal.py:81` | `medir`: filtro ID `__in` e `values_list`. |
| `app/chamados/services/vigia_fio.py:399` | `censo` :399/:400/:466/:486: filtro ID `__in`, SELECT e chaves; `censo_alcance` :614–616: filtro, SELECT e chave; `viva_em_disputa_fechada` :749 e `carimbos_sem_lastro_por_dono` :807: SELECT. |
| `app/chamados/services/pergunta_no_app.py:50` | `perguntas_do_app`: SELECT. |
| `app/chamados/services/limbo_folgas.py:204` | `filhos_no_defeito` :204 e `reatar_perguntas_ao_limbo` :239: SELECT. |
| `app/chamados/services/declaracao_texto.py:588` | `insumos_em_regularizacao`: SELECT. |
| `app/chamados/services/disputa_emissao.py:1422` | `_ja_perguntou` :1422 e `_resolver_perguntas_por_chamado_resolvido` :1827: SELECT. |
| `app/chamados/services/competencia_trancada.py:108` | `medir`: SELECT. |
| `app/chamados/services/fio.py:226` | `montar_chamados_fio` :226 e `processar_post_fio` :597: SELECT; `reapontar_resolvedora` :934: igualdade/`is not None`; `rechavear_data_evento` :1013/:1035/:1038/:1043/:1048: leitura, truthiness/`is None`, atribuição, save e auditoria. |
| `app/chamados/services/relavra_via.py:237` | `desfazer_carimbo_sem_lastro`: SELECT. |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:355` | `_escrever_reversao` :355: serialização; `_descarimbados` :448: SELECT; `_reverter` :496: desigualdade com ID salvo. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:34` | `handle`: SELECT. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:537` | `_i6_pares` :537: SELECT; `handle` :1161/:1603/:1604/:1605/:1784: SELECT, filtro `isnull=False`, `exclude` por veredito e leitura direta. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:69` | `handle`: SELECT. |
| `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:122` | `_reverter`: SELECT. |
| `app/chamados/management/commands/lavrar_perguntas_stale.py:88` | `handle`: SELECT. |
| `app/chamados/management/commands/backfill_veredito_lavrado.py:101` | `handle`: SELECT. |

**Lavra — 21 funções**, união dos três campos; quem lê e escreve aparece uma vez.

| Sítio | Forma |
|---|---|
| `app/chamados/veredito_lavrado.py:270` — `lavrar` | Atribui veredito :270, julgamento :272 e expiração :273; salva :275. |
| `app/chamados/veredito_lavrado.py:250` — `gravar_lavras_em_lote` | `bulk_update` por `CAMPOS_DA_LAVRA` :237. |
| `app/chamados/veredito_lavrado.py:287` — `desfazer_lavra_em_memoria` | Restaura os três campos apenas em memória :287–289. |
| `app/chamados/management/commands/backfill_veredito_lavrado.py:120` — `handle` | Comparações `==`/`!=`, truthiness :120–135; atribuições :138–140; persistência pela porta :143/:146. |
| `app/chamados/models.py:308` — `_gravar_estado` | Import `lavrar as _lavrar_s2`; chamada :312. |
| `app/chamados/models.py:3003` — `_nascer` | Import `lavrar as _lv_s2`; lavra em memória e save :3004–3006. |
| `app/chamados/models.py:3146` — `carimbar` | Módulo `veredito_lavrado as _vl`; foto, lavra, save e restauração :3147–3154. |
| `app/chamados/services/fio.py:926` — `reapontar_resolvedora` | Alias `_lavrar`, `salvar=False`; persistência conjunta :927–928. |
| `app/chamados/services/fio.py:1042` — `rechavear_data_evento` | Alias `_lavrar`, `salvar=False`; persistência conjunta :1043. |
| `app/chamados/services/relavra_via.py:124` — `relavrar` | Foto, lavra/save :126–127, comparação :130 e auditoria :137. |
| `app/chamados/management/commands/lavrar_perguntas_stale.py:60` — `handle` | Seleciona stale; exige `--apply` :82; chama `lavrar` :89. |
| `app/chamados/veredito_lavrado.py:224` — `esta_stale` | Truthiness negativa, `is None`, `> agora`; relógio :228; marco `> julgado_em` :232. |
| `app/chamados/veredito_lavrado.py:114` — `relogio_virou` | `is None`; expiração `<= hoje` e julgamento local `< expiração` :116. |
| `app/chamados/veredito_lavrado.py:281` — `foto_da_lavra` | Tupla dos três atributos. |
| `app/chamados/services/raiox.py:34` — `pks_stale_por_varredura` | `values_list`; truthiness, `is None`, `> agora`, relógio e comparação de marco :41–46. |
| `app/chamados/services/raiox.py:135` — `creditos_perguntas` | `exclude(materializada_em__isnull=False, veredito_lavrado=VL_SIM)`. |
| `app/chamados/services/raiox.py:184` — `pergunta_sem_acao_possivel` | Truthiness; `== VL_SIM` :185; vazio chama juiz :187. |
| `app/chamados/services/tarefa_do_dia.py:351` — `CargaDaTarefa.__init__` | Mapa do veredito; `in ('sim','nao') and not esta_stale(...)` :352. |
| `app/chamados/services/tarefa_do_dia.py:141` — `_pela_decisao` | Leitura do mapa; `is None` :142; `== 'nao'` :144. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1167` — `handle` | Pula stale; truthiness e desigualdade contra juiz real :1173. |
| `app/chamados/services/passivo_validado.py:83` — `_reversao_de` | `getattr(p,c,None)`; nomes vêm de `CAMPOS_REVERSAO` :64–66. |

**Complementos da contagem:**

| Sítio | Forma |
|---|---|
| `app/chamados/migrations/0053_pergunta_unica_por_familia.py:33` — `_no_fato` | `Q(data_evento__gte=HORIZONTE_DO_FATO)`. |
| `app/chamados/migrations/0053_pergunta_unica_por_familia.py:40` — `_guarda` | Agrupamento por dia, `Count`, filtro `n__gt=1` e contagem; modelo histórico por al

[truncado em 20000 de 39636 caracteres]

## 10/11 19:49 · sessao 88e167697efd · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

**Parei na condição explícita: o replay não reproduziu o reaponte. A cura ficou incompleta e o diff é parcial.** A reprodução preservada falha por assertion, sem erro de montagem. ([RED:62](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5_replay_red_memoria.py:62))

O caso encontrado é diferente da cadeia já triada: **no replay nasce uma pergunta**, porque ele resolve outro dia. O teste usa núcleo, resolvedor e gerador reais por AST; identidade/ORM/transação são doubles. Porta ORM real: **[nao verificado]**. ([reprodução:23](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5_replay_red_memoria.py:23)) [nao verificado]

| Passo | Resultado observado na sonda |
|---|---|
| Ensaio recebe `ultima_batida` de 08/07 | Reusa a pergunta de 08/07 da dona fechada. |
| Savepoint desfeito | Desfaz também seu reaponte. |
| Replay aplica o ato sobre o contexto antigo da dona | A data explícita de 09/07 prevalece sobre `ultima_batida`. |
| Porta recebe 09/07 | Nasce outra pergunta; o gerador chama `reatar_pergunta_orfa` uma vez. |
| Estado final | Uma DS, nenhum pai novo, **duas perguntas**; a original permanece sem o reaponte. |

As assertions positivas registram essa sequência antes do RED final. (`chamados/tests/test_bo_disputa_volta5_replay_red_memoria.py:57`)

A precedência está no resolvedor, que lê a data por motivo antes da batida; o replay conserva essa data antiga ao fazer o merge. Intervalo pertence à família MARCO, portanto não recebe o episódio da família BATIDA. (`chamados/services/disputa_emissao.py:354`, `chamados/services/disputa_emissao.py:370`, `chamados/services/disputa_emissao.py:694`, `chamados/catalogo/motivos.py:48`, `chamados/models.py:3452`)

Comando do RED preservado — resultado observado: `FAIL`, **`2 != 1`**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_replay_red_memoria.py
```

**O que ficou construído, sem entrega como cura concluída:** núcleo sem recursão, ensaio/savepoint/replay, causas internas, observação pelo `core.observ`, guarda do push e retirada de `Command._disputa` com lápide. (`chamados/services/disputa_emissao.py:546`, `chamados/services/disputa_emissao.py:413`, `core/observ.py:73`, `chamados/services/disputa_emissao.py:740`, `ponto/management/commands/reconciliar_grade.py:412`)

As sondas de (a)–(h) estão nestes sítios; a reprodução acima demonstra que elas ainda não bastavam para cobrir D3:

| Caso | Sítio |
|---|---|
| (a) Zero pergunta possível | `chamados/tests/test_bo_disputa_volta5_memoria.py:90` |
| (b) Estrangeira em dona aberta | `chamados/tests/test_bo_disputa_volta5_memoria.py:103` |
| (c) Fechada sem save/reatar; receivers ORM | `chamados/tests/test_bo_disputa_volta5_memoria.py:118`; `chamados/tests/test_bo_disputa_volta5.py:83` |
| (d) Ciclo e limite de replay | `chamados/tests/test_bo_disputa_volta4_memoria.py:44` |
| (e) Lote misto | `chamados/tests/test_bo_disputa_volta5_memoria.py:140` |
| (f) Idempotência | `chamados/tests/test_bo_disputa_volta5_memoria.py:155` |
| (g) Eleição após trava, dentro do atomic | `chamados/tests/test_bo_disputa_volta5_memoria.py:227` |
| (h) Reentrada e demais mutantes | `chamados/tests/test_bo_disputa_volta5_memoria.py:28` |

Os comandos dos mutantes entregues são os seguintes; **não reexecutei o conjunto final após a parada**, portanto seu resultado final fica **[nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante reentrada
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante salva_fechada
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante sem_guarda_push
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante nasce_sem_pergunta
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante eleicao_antes_trava
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante dedup_cala_replay
```

No censo, os escritores e leitores centrais ficaram assim:

| Campo/retorno | Escrita e leitura |
|---|---|
| `DS.motivos` | Núcleo cria e agrega; gerador consulta perguntas locais. Remoções continuam no modelo. (`disputa_emissao.py:576`, `:657`, `:564`; `chamados/models.py:1961`, `:2031`) |
| `DS.contexto`, `qtd`, `ultima_em` | Merge no emissor; resolvedor lê data e batida. (`disputa_emissao.py:525`, `:576`, `:354`, `:370`) |
| Pai, classificação, contexto e SLA | Nascimento pelo `ChamadoColaborador.abrir`; DS guarda o pai. (`disputa_emissao.py:640`, `:657`) |
| `DS.fechada_em` | Modelo fecha; núcleo/wrapper leem para impedir escrita na dona fechada e push. Receivers leem o valor. (`chamados/models.py:1932`; `disputa_emissao.py:576`, `:740`; `chamados/signals.py:28`, `:129`, `:153`) |
| Pergunta: dona, dia, célula e destino | Porta insere ou reaponta a existente preservando sua dona; núcleo lê FK/motivo e criação efetiva. (`chamados/models.py:2907`, `:3006`, `:3014`; `disputa_emissao.py:413`, `:569`) |
| Retorno nascimento/agregação | Núcleo mantém a tripla; wrapper consome também o coletor interno. (`disputa_emissao.py:546`, `:740`) |

Para cada leitor de `motivos`, o efeito previsto pelo diff parcial é:

| Leitor | Mudança de resultado |
|---|---|
| `api/foto_recebida.py:61` | Seleção acompanha a dona aberta com pergunta de foto. |
| `colaboradores/signals.py:71` | Revalidação de posto acompanha essa dona aberta. |
| `escala/signals.py:155` | Calibração de âncora acompanha essa dona aberta. |
| `chamados/models.py:2031` | Remoção conserva guardas de abertura e presença do motivo. |
| `auditar_disputas.py:88`, `:142`, `:169` | Audita/exibe motivos locais; comparação já usa perguntas locais. |
| `auditar_invariantes_chamados.py:50` | Resolve dias dos motivos mantidos; seleção de zero perguntas continua em `:62`. |
| `chamados/relatorios_disputa.py:52` | Rótulos passam a mostrar motivos locais. |
| `chamados/services/acoes_disputa.py:149` | Mesmo efeito na mensagem. |
| `chamados/services/fio.py:680` | Mesmo efeito no fio. |
| `pauta_aparelhos_compartilhados.py:49` | Cobertura pode passar à dona ou omitir motivo recusado sem pergunta. |
| `alertar_disputas_pendentes.py:82` | Log mostra os motivos mantidos. |
| `templates/chamados/detalhe_local.html:301` | Exibe os motivos mantidos. |
| `templates/chamados/partials/modal_fio.html:325` | Aviso acompanha a dona aberta com âncora. |
| `colaboradores/services/drawer.py:75` | Sem mudança direta: usa perguntas vivas em `:78`. |

Efeitos dessas mudanças nas cenas de produção: **[nao verificado]**.

Arquivos tocados no diff parcial:

- Produção: `chamados/services/disputa_emissao.py:257`, `core/observ.py:73`, `ponto/management/commands/reconciliar_grade.py:412`.
- Testes BO: `test_bo_disputa_sem_pergunta.py:32`, `test_bo_disputa_sem_pergunta_memoria.py:1`, `test_bo_disputa_volta2_memoria.py:1`, `test_bo_disputa_volta3_memoria.py:1`, `test_bo_disputa_volta4_memoria.py:1`, `test_bo_disputa_volta5_memoria.py:1`, `test_bo_disputa_volta5.py:1`, `test_bo_disputa_volta5_replay_red_memoria.py:1` — todos em `chamados/tests/`. [nao verificado]
- Selos: `chamados/tests/test_porta_chamado_e3.py:91`, `chamados/tests/test_sla_pelo_juiz.py:114`, `chamados/tests/test_porta_disputa_emissao.py:83`, `chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:133`, `core/tests/test_medida_turno_do_par.py:267`.

Suite Django, PostgreSQL, receivers reais e ruff: **[nao verificado]**. Para a certificação autorizada continuam pendentes os módulos BO acima, `test_porta_disputa_emissao`, `test_disputa_s127`, `test_c7_red_minimos`, `test_invariantes_disputa`, `test_hx_disputa_muda_sem_ata` e o módulo de ordem de trava indicado no pedido. **[nao verificado]** [nao verificado]

Órfãos, `_criar_pergunta_se_couber`, declaração, fábrica, smoke e auditor continuam reservados à 5b; a parada desta volta está no replay do núcleo. (`chamados/services/disputa_emissao.py:922`, `:1126`, `:711`, `:688`)

## 10/11 19:44 · sessao c0b5c529908d · - · - · origem=Ronald (tmux)

Complemento do censo FK concluído: **52 funções com acesso explícito** a `PerguntaDisputa.celula/celula_id`, incluindo queryset e carregamento relacionado; **6 funções adicionais de tupla/evento**, inventário enviado nas mensagens. A soma desses recortes é **58 funções**, sem afirmar fechamento de todo o grafo transitivo.

Escritores persistentes identificados:

- `app/chamados/models.py:2966` e `:3014`, na função `_nascer`.
- `app/chamados/services/fio.py:1038`, persistido em `:1043`, na função `rechavear_data_evento`.
- Escritor implícito do relacionamento: `on_delete=SET_NULL`, `app/chamados/models.py:2415`.

`app/chamados/models.py:1501` atribui a FK numa cópia em memória; não é escritor persistente.

Os seis caminhos complementares são `acordar_celulas`, `ressuscitar_perguntas`, `DecisaoPergunta.registrar`, `reabrir_para_correcao`, `_outbox_post_save_pergunta` e `_becos_post_save_pergunta`, com referências e formas já enviadas.

Busca de `.celula/.celula_id` em templates e static retornou zero; leitores homônimos de `ChamadoColaborador`, `EventoCelula`, `CelulaDia` e funções chamadas `celula` foram separados manualmente.

A união com os 117 leitores de `data_evento` ainda exige deduplicação pelo inventário do outro agente; **não somar 117+58 diretamente**.

## 10/11 19:44 · sessao 62cb75685cec · - · - · origem=Ronald (tmux)

Censo dos três campos de `PerguntaDisputa`: **21 funções operacionais na união**, sendo **11 escritoras/despachantes de escrita** e **10 outras leitoras**, sem duplicar quem lê e escreve. Declarações únicas: `app/chamados/models.py:2585`, `:2590`, `:2596`. `CelulaDia.julgada_em` tem outra grafia e ficou fora (`app/chamados/veredito_lavrado.py:194`).

| # | Função/sítio | Forma |
|---|---|---|
| 1 | `veredito_lavrado.lavrar` — `app/chamados/veredito_lavrado.py:270` | Atribui `veredito_lavrado`; `julgado_em=timezone.now()` em `:272`; janela por `expira_em(data_evento, hoje)` em `:273`; `save(update_fields=...)` em `:275` |
| 2 | `gravar_lavras_em_lote` — `app/chamados/veredito_lavrado.py:250` | `bulk_update` dos três campos via `CAMPOS_DA_LAVRA`, definido em `:237` |
| 3 | `desfazer_lavra_em_memoria` — `app/chamados/veredito_lavrado.py:287` | Atribui os três valores da foto em memória; sem persistência em `:287`–`:289` |
| 4 | `backfill_veredito_lavrado.Command.handle` — `app/chamados/management/commands/backfill_veredito_lavrado.py:120` | `(veredito or '') == v`; janela `!= e` em `:121`, `:135`; truthiness contra indeterminado em `:124`; atribui três campos em `:138`–`:140`; persiste pela porta em `:143`, `:146` |
| 5 | `ChamadoColaborador._gravar_estado` — `app/chamados/models.py:308` | Import **`lavrar as _lavrar_s2`**; chama com persistência em `:312` |
| 6 | `PerguntaDisputa._nascer` — `app/chamados/models.py:3003` | Import **`lavrar as _lv_s2`**; lavra em memória `:3004`; acrescenta os três campos a `upd` e salva em `:3005`–`:3006` |
| 7 | `PerguntaDisputa.carimbar` — `app/chamados/models.py:3146` | Import de módulo **`veredito_lavrado as _vl`**; lê foto `:3147`, lavra com `salvar=False` `:3150`, salva `:3152`; desfaz em memória se exceção em `:3154` |
| 8 | `fio.reapontar_resolvedora` — `app/chamados/services/fio.py:926` | Alias **`_lavrar`**, `salvar=False`; salva os três campos junto da identidade em `:927`–`:928` |
| 9 | `fio.rechavear_data_evento` — `app/chamados/services/fio.py:1042` | Alias **`_lavrar`**, `salvar=False`; salva dia, célula e três campos em `:1043` |
| 10 | `relavra_via.relavrar` — `app/chamados/services/relavra_via.py:124` | Foto do veredito; `lavrar(..., salvar=False)` `:126`; save dos três `:127`; compara `!= antes` `:130`; lê para auditoria `:137` |
| 11 | `lavrar_perguntas_stale.Command.handle` — `app/chamados/management/commands/lavrar_perguntas_stale.py:60` | Lê conjunto stale pela porta de varredura; exige `--apply` em `:82`; chama `lavrar(p)` em `:89` |
| 12 | `esta_stale` — `app/chamados/veredito_lavrado.py:224` | Truthiness negativa do veredito; `julgado_em is None`; `julgado_em > agora`; lê janela por `relogio_virou` em `:228`; comparação marco `> julgado_em` em `:232` |
| 13 | `relogio_virou` — `app/chamados/veredito_lavrado.py:114` | Recebe os valores; `is None` para dois campos; janela `<= hoje` e data local do julgamento `< janela` em `:116` |
| 14 | `foto_da_lavra` — `app/chamados/veredito_lavrado.py:281` | Leitura dos três atributos em tupla |
| 15 | `raiox.pks_stale_por_varredura` — `app/chamados/services/raiox.py:34` | `values_list` dos três campos; truthiness negativa de veredito, julgamento `is None`/`> agora` em `:41`; relógio compartilhado em `:43`; marco `> julgamento` em `:46` |
| 16 | `raiox.creditos_perguntas` — `app/chamados/services/raiox.py:135` | `.exclude(materializada_em__isnull=False, veredito_lavrado=VL_SIM)`: conjunção dentro do exclude |
| 17 | `raiox.pergunta_sem_acao_possivel` — `app/chamados/services/raiox.py:184` | Truthiness do veredito; compara `== VL_SIM` em `:185`; vazio cai no juiz real em `:187` |
| 18 | `CargaDaTarefa.__init__` — `app/chamados/services/tarefa_do_dia.py:351` | Copia veredito em mapa; `in ('sim','nao') and not esta_stale(...)` em `:352` |
| 19 | `tarefa_do_dia._pela_decisao` — `app/chamados/services/tarefa_do_dia.py:141` | Consome mapa derivado da coluna: `.get(p.pk)`; `is None` em `:142`; `== 'nao'` em `:144` |
| 20 | `auditar_invariantes_chamados.Command.handle` — `app/chamados/management/commands/auditar_invariantes_chamados.py:1167` | Pula stale; compara truthiness do veredito e `!= juiz real` em `:1173` |
| 21 | `passivo_validado._reversao_de` — `app/chamados/services/passivo_validado.py:83` | **Leitura dinâmica `getattr(p, c, None)`**; os três nomes chegam por `CAMPOS_REVERSAO` em `:64`–`:66`; serializa foto de reversão |

A programação do escritor periódico existe: **`lavrar_perguntas_stale --apply`, a cada cinco minutos**, `app/config/crons.py:390`. É invocação agendada da função nº11, não escritor adicional da coluna. `backfill_veredito_lavrado` tem registro de operação única em `app/config/crons.py:1402`; esse texto de metadados não entrou como função operacional.

Não encontrei leitura direta dos três campos em templates, `static/` ou signals por grep dos nomes exatos. Nos signals lidos, o callback pós-save trabalha com disputa/portas, não com esses campos: `app/chamados/signals.py:228`, `:251`; a relavra reativa que antes seria em `ponto.signals` está descrita como retirada, e o código executado chama cartório em `app/ponto/signals.py:93`, com a retirada documentada em `:94`–`:105`.

Método: AST em memória sobre Python de produção, buscando atributos, keywords ORM, strings de campos, chamadas e imports com `asname`; grep complementar em toda a árvore para os três nomes, funções canônicas e constantes de campos. Segui especificamente `CAMPOS_DA_LAVRA`, `CAMPOS_REVERSAO` e o mapa `self._lavra`, por isso entraram o escritor via `bulk_update`, o `getattr` e `_pela_decisao`. Excluí testes, migrations, declarações de campos, comentários, descrições de contratos e encaminhadores que apenas passam objetos/mapas sem consultar nem escrever seu valor. Os encaminhadores do mapa estão em `app/chamados/services/tarefa_do_dia.py:404` e `:164`; o consumidor efetivo é nº19.

## 10/11 19:42 · sessao 191095aeeb0f · - · - · origem=Ronald (tmux)

O item já tem construtor e migração nesta cópia: `app/chamados/models.py:2776`, `app/chamados/services/fio.py:979`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:179`. Não confirmei a aplicação em produção; depende de dado: contagem de linhas elegíveis, migradas, fora, colisões e competências exportadas, pelos filtros e classificação de `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:205` e `:129`.

**(a) Lei, juiz e porta.** L-002 e L-007: `app/docs/LEIS.md:60`, `:65`; origem/autoridade/escritor único também em `CLAUDE.md:17`, `:20`, `:25`, `:32`. O juiz registrado de “a que turno pertence a batida?” é `turnos_do_colab`, `app/core/juizes.py:208`; marcos previstos e meia-noite, `:213`, `:216`; consulta correspondente em `app/docs/JUIZES.md:20`, `:22`, `:24`. `DiaDoTurno` é nome/tipo da resposta existente, não juiz novo: `app/core/dia_do_turno.py:3`, `:13`. O valor vem de `dia_do_turno_da_batida`/`dia_do_turno_da_celula`, `app/ponto/turnos.py:2927`, `:2939`.

Portas: nascimento `PerguntaDisputa.abrir`, `app/chamados/models.py:2776`; re-chaveamento `rechavear_data_evento`, `app/chamados/services/fio.py:979`; ponto único de atribuição `gravar_dia_do_fato`, `:790`, atribuição real `:800`. A batida materializada passa por `registrar_batida`, `app/chamados/services/materializacao.py:926`, `:1006`; lei do chokepoint em `app/docs/LEIS.md:78`.

**(b) O que a cura muda.** Nenhum campo/tipo persistido novo é necessário no delta desta cópia: `data_evento` já é DateField, `app/chamados/models.py:2423`; o tipo novo é apenas `typing.NewType`, `app/core/dia_do_turno.py:13`. O ato histórico de cura **muda estado existente**: atribui outro `data_evento`, troca `celula_id`, relavra e salva, `app/chamados/services/fio.py:1037`, `:1038`, `:1042`, `:1043`. Portanto, para cobrir a interpretação ampla de “valor/composição novo”, responda **sim — re-chaveamento do par data_evento/celula; existência desse par em produção [nao verificado]**. **Muda decisor: não**: quem dá o dia continua sendo o juiz do turno, `app/chamados/services/fio.py:984`, `app/ponto/turnos.py:2935`; a atribuição explicitamente não calcula, `app/chamados/services/fio.py:794`.

**Conferência do relato.** O relato não descreve o comportamento atual inteiro: o nascimento exige dia explícito, recusa `None` e tem porta separada de conversa, `app/chamados/models.py:2783`, `:2787`, `:2793`; o plantio combina hora com esse dia, `app/chamados/services/materializacao.py:414`, e o ajuste escolhe somente esse dia ou o seguinte, `app/chamados/juizes.py:324`. Sem marcos, devolve o timestamp, `:329`. O campo de conversa continua significando dia civil, por porta separada, `app/chamados/models.py:2796`, `:2801`, e a migração a exclui, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:205`. Casos D+2 persistentes hoje: depende de dado; não há contagem no anexo fornecido [nao verificado].

**(d) Casa.** **COMPLEMENTO de FIO-E0-V4-E1-ENTRADAS**, cuja linha diz absorver PLANTIO, `app/docs/BACKLOG.md:1848`; a linha do próprio bug registra a absorção na E1(b), `:1814`. A casa estrutural de escritores seguintes também está registrada, `:1863`. Não classificar como BO simples: o estado tem muitos consumidores, conforme censo abaixo. A dívida distinta “envelope pelo gerador, não pelo DNA congelado” tem casa própria `PLANTIO-ENVELOPE-PELO-GERADOR`, `app/docs/BACKLOG.md:1004`; o código atual realmente ainda carrega `escala_vigente`+`marcos_turno`, `app/chamados/juizes.py:316`.

**(e) Padrão.** `Single Writer Principle`, termo exato em `app/docs/GLOSSARIO.md:12`; realizado pelo ponto único de atribuição, `app/chamados/services/fio.py:790`, `:800`. Para o passivo, `Backfill / Reprocessing`, `app/docs/GLOSSARIO.md:23`, gatilho existente em `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:179`.

**(f) perguntas de lei: 0.** Não surgiu pergunta normativa sem resposta para o dia/autoridade/escrita: registro em `app/core/juizes.py:208`, portas e significado em `app/chamados/models.py:2783`, `:2796`, `app/chamados/services/fio.py:979`. Incidência em produção e execução do passivo são perguntas de dado, não lei [nao verificado].

**Censo de `data_evento`: metodologia e limite.** AST de todos `app/**/*.py`, excluindo `tests`, `migrations`, `logs`, `__pycache__`; reconheci atributo `.data_evento`, `getattr(...,'data_evento')`, keywords contendo `data_evento` e strings de campos/caminhos ORM. Encontrei **204 pares únicos arquivo/função/linha, em 121 grupos função/módulo**. Quatro grupos não são consumidores operacionais: metadados nos módulos `app/chamados/models.py:3221`, `:3483`, `:3486`, `:3492`; configuração de campos/filtro dinâmico em `app/chamados/services/fio.py:1373`, `:1378`; introspecção de parâmetros em `app/escala/management/commands/hub_smoke.py:35`, `:51`. Restam **117 funções operacionais com menção direta ou propagação pelas portas**, mais **3 arquivos template/5 linhas**, total **120 grupos diretos contados**. Isso **não é total fechado de todos efeitos do re-chaveamento**: falta meu censo separado do FK `PerguntaDisputa.celula`, que também muda em `app/chamados/services/fio.py:1038`. Não apresentar 120 como exaustivo do item.

O selo local reconhece aliases de import e aliases por atribuição, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:42`, e o ponto permitido é único, `:37`. Minha busca de atributos/keywords não depende do nome importado do modelo [nao verificado como garantia de toda reflexão dinâmica].

Lista completa dos **117 grupos operacionais**, agrupada por arquivo; todos são de `PerguntaDisputa` ou de seu estado propagado, salvo exclusões de metadados/introspecção acima:

- `app/api/views.py`: `api_chamados_v2:1183` truthiness+ISO/label, `:1184`; `api_disputa_responder:2655` interpolação em mensagem.
- `app/api/views_mensageria.py`: `_item_validar:568` truthiness+ISO para JSON.
- `app/chamados/canal.py`: `perguntas_mudas_com_resposta:65` queryset `isnull=False`; `:67` ordenação; `:72` chave de associação; `:79` payload.
- `app/chamados/juizes.py`: `_dia_da_disputa:80` truthiness e lista `:81`; `dias_do_fato:214` truthiness; `_plantio_na_hora_do_slot:271` fallback `or`; `_dia_de_turno_da_pergunta:336` `getattr`, conversa→None; `_celulas_da_pergunta:637` `getattr`; `_chave_memo_pv:831` chave memo via `getattr`; `_marco_esperado_da_grade:1725` atribuição de leitura; `_dias_das_perguntas_vivas:2264` queryset `isnull=False`, lista `:2268`.
- `app/chamados/management/commands/auditar_invariantes_chamados.py`: `_candidatos_i8:63` truthiness, filtros por data `:73`, `:74`; `_contraprova_i8bx:148` intervalo `<=`; `_cobre_ausencia_i21:160` truthiness, argumento de juiz `:162`; `_piso_f3_antes_do_gerador:282` comparação `<`; `_contraprova_f3g:298` filtro `lte`+.exists; `contraprova_piso:325` chave, truthiness `:326`, chave `:348`, filtro de vínculo+.exists `:361`, intervalo `:373`; `handle:892` queryset isnull, intervalos `:914`, `:930`, filtro+.exists `:926`, queryset isnull `:957`, truthiness `:1665`, igualdade ORM `:1708`, isnull/lt `:1778`, `:1779`, argumento de juiz `:1804`. Alias `PD` aparece no uso ORM em `:957`.
- `app/chamados/management/commands/backfill_decisoes_pergunta.py`: `linhas_a_criar:60` `.only`; `_escoar:69` chave; `admin_decidiu_sem_ato:104` queryset isnull; `competencia_de:115` `is None`, chave `:118`, argumento `:120`; `aplicar:282` `is None`, `:283` `is not None`; `_grupos_fora:371` `values_list`.
- `app/chamados/management/commands/backfill_veredito_lavrado.py`: `handle:111` argumento de `expira_em`.
- `app/chamados/management/commands/dedup_perguntas_fato.py`: `handle:85` `is not None`, `:86` `is None`.
- `app/chamados/management/commands/encerrar_perguntas_com_fato.py`: `handle:74` mensagem.
- `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py`: `_fonte_viva:97` leitura da base de comparações `:100`, `:102`, `:109`, `:113`, `:118`; `classificar:144` janela, payload `:148`, desigualdade `:158`, igualdade `:163`; `handle:205` isnull, gte `:209`, lte `:211`; `_veto_do_grupo:309` values_list; `_escrever_reversao:354` JSON de estado propagado; `_migrar:392` values_list; `_descarimbados:442` Q igualdade, chave `:450`; `_reverter:481` leitura JSON propagado, argumento de veto `:488`.
- `app/chamados/management/commands/reabrir_orfas_enterradas.py`: `handle:68` isnull=False; argumento de janela `:86`; mensagens `:91`, `:102`, `:117`; competência `:144`, `:145`.
- `app/chamados/models.py`: `_vivas_entre:364` argumento e comparação do resultado `is not False`; `_plano_pergunta_viva:390` argumento e truthiness `:391`; `q_dos_fatos:2753` Q igualdade; `abrir:2789` **escritor indireto**, passa campo à porta; `abrir_conversa:2801` **escritor indireto**, dia civil; `_nascer:2940` desigualdade, mensagem `:2956`, captura de estado `:2962`; `linhas_do_evento:3228` values_list.
- `app/chamados/reconciliador.py`: `lavrar_sem_turno_aberto:370` leitura direta; `_dia_fora_do_alcance_c:736` getattr `is None`; `reemitir_chamado_do_dia:954` **escritor indireto** via `reapontar_resolvedora`; `_orfas_abertas:1050` queryset isnull; `veredito_orfa:1082` `is None`, comparação `>=` `:1088`, argumento de juiz `:1090`; `encerrar_orfas_pela_lampada:1152` mensagem; `semear_eventos_celula:1303` pertença a set, set `:1305`, ORM igualdade `:1307`; `reatar_pergunta_orfa:1557` argumento, truthiness+ISO `:1567`, **escrita indireta** via reapontar `:1642`, `:1655`.
- `app/chamados/services/aviso_pendencia.py`: `aviso_texto_livre:16` truthiness+strftime.
- `app/chamados/services/cobranca.py`: `checar_cobranca:83` truthiness+conjunto ordenado.
- `app/chamados/services/competencia_trancada.py`: `medir:107` queryset gte/lte; `reabrir:252` queryset gte/lte, mensagem `:256`.
- `app/chamados/services/declaracao_texto.py`: `declarar_hora_colab:370` **escrita indireta** via reapontar, nascimento `:377`; `insumos_em_regularizacao:590` chave; `aplicar_declaracao:667` **escrita indireta** por nascimento.
- `app/chamados/services/disputa_emissao.py`: `gerar_perguntas_disputa:399`, `:418` igualdade ORM+exclude, `:451` **escrita indireta** por nascimento; `_criar_pergunta_se_couber:844` **escrita indireta** por nascimento; `criar_disputa_a_partir_de_chamados_orfaos:1074` fallback `or`; `_ja_perguntou:1415` igualdade ORM; `responder_pergunta_disputa_core:1498` mensagem, argumento `:1531`; `_resolver_perguntas_por_chamado_resolvido:1863`, `:1935` argumento de autoridade de via.
- `app/chamados/services/explicador.py`: `explicar_pergunta:70` truthiness+strftime; `por_que_nao_fecha:122` ordenação, truthiness `:126`.
- `app/chamados/services/fabrica_da_celula.py`: `motivos_a_perguntar:148` igualdade ORM; `perguntar:170` igualdade ORM.
- `app/chamados/services/fio.py`: `data_do_fato_de:87` leitura, truthiness+desigualdade `:88`; `_costura_nao_mata_o_fio:776` argumento de competência; `gravar_dia_do_fato:800` **atribuição única real**, devolve nome do campo `:801`; `reapontar_resolvedora:867` truthiness, comparação `:934`, **escrita indireta** por ponto único; `_colisao_ao_datar:953` assert igualdade; `rechavear_data_evento:1013` captura, **escrita indireta/persistência** `:1037`, `:1043`, trilha antes/depois `:1047`, `:1048`; `rechavear_no_reuso:1068` argumento de veto, mensagem `:1072`; `reuso_sem_rechave:1109` igualdade; `violacao_de_saida:1133` igualdade, mensagem `:1136`; `retratar_duplicata:1362` mensagem; `q_do_horizonte:1412` Q gte; `q_do_horizonte_ou_sem_dia:1419` Q isnull; `_vizinhos_do_fato:1447` argumento+fallback condicionado; `chave_do_dia:1460` tupla; `_dia_do_fato:1495` `is not None`, construtor de tipo `:1497`; `dia_do_turno_da_linha:1630` getattr/fallback; `universo_do_fato:1800` igualdade de tupla; `fecho_cruza_dias:1854` values_list; `grupos_de_colisao:1945` campos dinâmicos, exclude `:1946`, values/order `:1948`, igualdade `OuterRef` `:1955`, values dinâmicos `:1958`, filtro dinâmico `:1970`, tupla `:1985`; `fora_do_indice:2005` isnull; `batida_sem_origem_no_horizonte:2024` values_list.
- `app/chamados/services/historico.py`: `_eventos_dos_chamados:108` truthiness+strftime.
- `app/chamados/services/limbo_folgas.py`: `filhos_no_defeito:203` igualdade ORM.
- `app/chamados/services/materializacao.py`: `_tipo_pelo_marco:279` truthiness, argumento da grade `:282`; `_ts_tipo_do_plantio:344` truthiness, leitura `:345`; `_declarar_ausencia_de_pergunta:473` truthiness, leitura `:474`; `_materializar_ausencia_de_resposta:517` leitura, igualdade ORM `:533`, payload propagado `:573`; `perguntas_absorvidas_por:624` igualdade ORM; `_materializar_uma:1265` argumento de autoridade da via.
- `app/chamados/services/passivo_validado.py`: `_dia_do_plantio:113` fallback `or`; `_plantar_o_dia:519` truthiness, conjunto `:520`.
- `app/chamados/services/pergunta_no_app.py`: `retencao:115` igualdade+any.
- `app/chamados/services/relavra_via.py`: `via_de_hoje:103` argumento de autoridade; `relavrar_dia:154` igualdade ORM.
- `app/chamados/services/tarefa_do_dia.py`: `_tarefa_pura:233` igualdade; `__init__:326` queryset gte/lte, chave `:329`.
- `app/chamados/services/validacao.py`: `_aceitar_a_folga:269` leitura; `validar_pergunta:446` argumento de recálculo, argumento `:497`, mensagem `:510`; `conferir_data_do_fato:671` trilha JSON de estado propagado.
- `app/chamados/services/vigia_fio.py`: `carimbos_sem_lastro_por_dono:816` argumento de competência, classificação pelo resultado.
- `app/chamados/veredito_lavrado.py`: `lavrar:273` argumento de expiração.
- `app/chamados/views_cobrar.py`: `validar_inline:176` ordenação ORM, `:185` truthiness de estado propagado do canal.
- `app/colaboradores/services/calendario.py`: `contexto_calendario:473` queryset range, chave `:477`; alias `_PD` no queryset.
- `app/folha/previa.py`: `montar_previa:65` queryset range, chave `:67`.
- `app/ponto/management/commands/mapa_divergencia.py`: `handle:272` queryset gte/lte+.only, conjunto `:273`; alias `_pd` é instância lida.
- `app/ponto/management/commands/vigia_plantio_overnight.py`: `batidas_fora_do_turno_da_pergunta:75` queryset por FK `pergunta_origem__data_evento__isnull=False`; janela `:86`, leitura para julgamento `:90`.
- `app/ponto/services/dia_do_colab.py`: `dia_do_colab:65` igualdade ORM.
- `app/ponto/services/veredito_celula.py`: `fechar_o_fio_do_dia:180` igualdade ORM.

Templates, **5 linhas em 3 arquivos**:

- `app/templates/chamados/partials/_card_atendimento.html:87` truthiness; `:90` filtro `date`; `:164` filtro `date` em JS inline.
- `app/templates/chamados/partials/_lote_preview.html:24` filtro `date`.
- `app/templates/chamados/meu_atendimento.html:19` filtro `date` em JS inline.

`rg data_evento app/static app/templates app/config/crons.py app/*/signals.py` encontrou nos crons apenas o registro descritivo do comando de migração, `app/config/crons.py:776`, `:777`, `:778`; não o classifique como leitor runtime. Nenhuma ocorrência textual foi encontrada em `app/static` ou nos arquivos `app/*/signals.py` examinados [nao verificado quanto a consumo indireto de payload sem o nome do campo].

**Lacunas que precisam ficar explícitas na consolidação:** o total acima é de grupos diretos de `data_evento`, não censo fechado de `data_evento+celula`; leitores indiretos por helpers precisam ser discriminados para alegar exaustividade funcional. Não há base para número 0, nem para provar ausência de estado em produção [nao verificado].

## 10/11 19:41 · sessao 62cb75685cec · - · - · origem=Ronald (tmux)

LOTE: evidência consolidada, só leitura.

**Autoridade e lei**

- A lei existente manda trabalho longo ir para job: `CLAUDE.md:89`; exige fonte única, testemunha leitora e escritor único: `CLAUDE.md:17`, `CLAUDE.md:20`, `CLAUDE.md:32`.
- **Sem juiz registrado específico para validar/classe de lote ou execução do job.** O registro vivo da família chamado tem C1–C20, sem `validar_pergunta` nem `classe_do_lote`: `app/core/juizes.py:272`. O lookup nomeia `validar_pergunta`: `app/docs/JUIZES.md:38`, mas diz que o registro vivo manda: `app/docs/JUIZES.md:11`, `app/docs/JUIZES.md:14`.
- Quem decide hoje: admin fornece confirmação; `validar_lote_classe_a` reclassifica cada pergunta na hora e só admite classe A: `app/chamados/services/validacao.py:698`, `:703`, `:732`, `:735`; `classe_do_lote` compara resposta, marco, delta, envelope e conflito de batida: `app/chamados/services/validacao.py:845`, `:852`, `:862`, `:865`, `:867`, `:875`.
- Porta: `validar_lote_classe_a` → `acao_validar_pergunta` → `validar_pergunta`: `app/chamados/services/validacao.py:746`, `app/chamados/services/acoes_disputa.py:18`, `:19`. A casca não é escritora canônica: `app/chamados/services/acoes_disputa.py:14`.
- `validar_pergunta` chama recalcular e materializar dentro do caminho síncrono: `app/chamados/services/validacao.py:446`, `:463`, `:469`. Isso mostra custo potencial acumulado por pergunta; qual chamada causou a regressão é **[nao verificado]**.

**Conferência do relato**

- “POST síncrono” confere: chamada direta antes da resposta HTTP em `app/chamados/views_cobrar.py:381`, retorno em `:386`.
- O endereço `:379` do BACKLOG é o **import**; a chamada é `:381`: `app/chamados/views_cobrar.py:379`.
- Não é só uma porta: existem três chamadas de produção, `app/chamados/views_cobrar.py:261`, `:381`, `app/core/views_ia.py:169`.
- “10–122 s”, “desde 03/11”, causa e incidência da regressão são relato de `app/docs/BACKLOG.md:1828`, `:1875`, não medida reproduzida nesta cópia: **[nao verificado]**. Depende de dado: quantidade de pedidos por rota/data, refs por pedido, latências, queries e tempo por estágio antes/depois de 03/11.

**(b) e (c): separar estado novo do resultado atual**

Para `LOTE-VALIDAR-VIRA-JOB`, **cria estado: sim**, tomando literalmente pedido de job com resultado posteriormente lido pela tela (`app/docs/BACKLOG.md:1828`). Hoje o resultado do lote nasce como dict local e retorna na mesma pilha, sem persistência desse agregado: `app/chamados/services/validacao.py:726`, `:738`, `:762`, `app/chamados/views_cobrar.py:386`. Modelo/campos exatos futuros: **[nao verificado]**; existência real de tabela fora da cópia: **[nao verificado]**.

**Muda decisor: não**, para a cura limitada à fronteira de execução: confirmação humana e reclassificação pela mesma porta já estão definidas em `app/chamados/services/validacao.py:698`, `:703`, `:735`, `:746`. Mover para outro processo não autoriza substituir esses juízes: `CLAUDE.md:20`, `:32`.

**Estado durável novo: 0 leitores + 0 escritores existentes.** Não existe ainda um símbolo/modelo de job da validação para censar; a contraparte atual é transitória, cujo censo é **1 escritor + 6 leitores = 7 sítios**:

| Papel/sítio contado | Forma |
|---|---|
| Escritor `validar_lote_classe_a` — `app/chamados/services/validacao.py:726`, `:730`, `:738`, `:754`, `:762` | Inicializa contadores/listas/dict, incrementa e retorna; `perg is None`, truthiness `perg.validada_em`, comparação `classe_do_lote(...)[0] != LOTE_A` |
| Leitor `views_cobrar.validar_lote` — `app/chamados/views_cobrar.py:262` | Truthiness `r['erros']`; `r['validadas'] > 0`; subscritos `recusados`/`nao_gravados` na resposta JSON |
| Leitor `views_cobrar.validar_lote_tela` — `app/chamados/views_cobrar.py:383` | Subscreve contagem e `len(r['recusados'])` para trilha; passa dict integral ao template em `:386` |
| Leitor `views_ia._aplicar_lote` — `app/core/views_ia.py:173` | Subscreve contagens/`len`; encaminha dict a `_frase_do_lote` em `:175` |
| Leitor `views_ia._frase_do_lote` — `app/core/views_ia.py:185` | Truthiness `recusados`, `nao_gravados`, `erros`; comprimentos, interpolação e join |
| Leitor template — `app/templates/chamados/partials/_lote_preview.html:4` | Truthiness `resultado`; interpolação `validadas`; truthiness, length e loops de `recusados`/`nao_gravados` em `:6`–`:9` |
| Leitor JS inline `executarLote` — `app/templates/core/_copiloto.html:247` | POST refs, JSON; `d.recusados && d.recusados.length`; interpola `d.validadas` em `:254`–`:255` |

Método: AST em memória em todos os `.py` de `app`, resolvendo `ImportFrom` com `asname`, imports de módulo e chamadas por atributo, encontrou **3 chamadores de produção** da porta. Grep complementar dos símbolos/rotas `validar_lote_classe_a|validar_lote_tela|validar-lote|validou_lote_tela|ia_validou_lote` em `static/`, `config/crons.py`, signals e commands não encontrou consumidor do resultado de lote. Os sete sítios acima foram abertos e lidos; unidade de contagem é função Python/JS ou template, sem duplicar múltiplas referências na mesma função.

Isso **não é censo global de `validada_em`/`materializada_em`**, porque a cura de fronteira não cria novo valor nem novo significado desses campos; as escritas continuam pela porta atual (`app/chamados/services/validacao.py:435`, `:463`, `:469`).

**Casa/padrão/perguntas**

- `LOTE-VALIDAR-REGRESSAO-0311`: **COMPLEMENTO de LOTE-VALIDAR-VIRA-JOB**, casa `app/docs/BACKLOG.md:1828`; a triagem liga item03 a LOTE-VALIDAR em `:1831`, e a ordem manda medir causa antes em `:1875` e `app/docs/CORTES.md:440`. Criação de estado/mudança de decisor da **cura da regressão**: **[nao verificado]**, pois causa e cura não foram determinadas. A medição autorizada isoladamente não cria estado nem muda decisor.
- `LOTE-VALIDAR-VIRA-JOB`: não achei inscrição que o absorva na série E ou MOTOR. Sua casa própria já está em `app/docs/BACKLOG.md:1828`; não pode ser chamado “BO simples”, pois há 3 portas/7 sítios. **O284 é precedente de mecanismo**, não absorção documental: `app/docs/BACKLOG.md:1542` exige o mesmo mecanismo do holerite, e `app/relatorios/lote_espelho.py:14`–`:25` separa fronteira, cálculo, escritor e dedup. Classificar como COMPLEMENTO da casa de jobs O284 é uma **proposta de encaixe**, não fato registrado.
- Padrão job: **Event-driven architecture**, `app/docs/GLOSSARIO.md:16`; **Single Writer Principle**, `:12`. Eventual medição da regressão: `n/a (medida)` é autorizado em `CLAUDE.md:68`; padrão da cura só depois da causa, **[nao verificado]**.
- **perguntas de lei: 0** para mover trabalho longo ao mecanismo existente sem alterar decisão (`CLAUDE.md:89`, `app/docs/BACKLOG.md:1542`). Causa de performance, tabela, retenção e desenho do job são investigação/decisão técnica, não lacuna demonstrada de lei.

## 10/11 19:40 · sessao c0b5c529908d · - · - · origem=Ronald (tmux)

## BO-0611-07-E4B-COBRE

**Limite da identificação.** O anexo só manda conferir cobertura pelo E4b; a árvore repete isso em `app/docs/BACKLOG.md:1876` e `app/docs/CORTES.md:440`. Não fornece o estado/PK/dia do item 07 nem o DRY; cobertura em produção **[nao verificado]**. `app/docs/RELATO.md:505` contém hipótese documental de pergunta validada e carimbada sem reprocessamento; isso não comprova o dado de produção.

**(a) Lei/juiz/porta.**

- Lei estrutural: **L-061**, consumidor lê célula/ata, `app/docs/LEIS.md:95`; evento julga, cron vigia e consumidor lê a lâmpada em `CLAUDE.md:168`, `CLAUDE.md:169`, `CLAUDE.md:170`. Um juiz/escritor e idempotência em `CLAUDE.md:175`.
- Escopo do E4b já delimitado por `app/docs/CORTES.md:439`: DRY, comp 11 primeiro, exportadas em lista para autorização. Horizonte em **L-124**, `app/docs/LEIS.md:145`; reconstrução pela porta em **L-121**, `app/docs/LEIS.md:142`; **L-113 altera L-092**, `app/docs/LEIS.md:153` — não citar L-092 isoladamente como bloqueio absoluto.
- Vitalidade: `_C6 → pergunta_viva`, `app/core/juizes.py:278`; ação exigível por marco: `_C19 → tarefa_do_dia`, `app/core/juizes.py:291`; tranca: `_C20`, `app/core/juizes.py:292`.
- Validação é autoridade na tabela de consulta, `app/docs/JUIZES.md:38`; a busca de `validar_pergunta|VALIDACAO` em `app/core/juizes.py` não encontrou registro específico. Não confundir juiz de vitalidade `_C6` com decisão humana de validar.
- Porta atual de validação: `app/chamados/services/validacao.py:373`; unidade do plantio: `app/chamados/services/materializacao.py:1158`; batida passa por `registrar_batida` em `app/chamados/services/materializacao.py:926`, criação canônica em `app/ponto/registro_batida.py:152` e `:157`.

**(b) Estado e decisor.** Para a atividade efetivamente descrita — conferir DRY — **não / não**: a linha é de conferência e não define alteração, `app/docs/BACKLOG.md:1876`. Para eventual cura/complemento, **[nao verificado]**: falta o fato do item 07 e o DRY; não é rigoroso prometer que cria ou não cria combinação nova.

**(c) Não há estado novo definido para censar.** O número do censo de leitores/escritores da futura cura é **[nao verificado]**, e não zero. Segue mapa comprovado dos caminhos existentes para testar a hipótese documental, **sem chamá-lo de censo exaustivo**:

| Caminho | Forma lida/escrita |
|---|---|
| `app/chamados/services/validacao.py:393` | truthiness `respondida_em`; sem resposta recusa |
| `app/chamados/services/validacao.py:396` | truthiness `validada_em`; retorna `ja_validada` |
| `app/chamados/services/validacao.py:461` | `materializada_em is None` e predicado hora para chamar unidade |
| `app/chamados/services/materializacao.py:1369` | queryset: `respondida_em__isnull=False`, `validada_em__isnull=False`, `materializada_em__isnull=True` |
| `app/chamados/services/vigia_carimbo.py:95` | import com alias: `_pergunta_materializada_de_fato as _juiz` |
| `app/chamados/services/vigia_carimbo.py:97` | queryset: `validada_em__isnull=False`, `tipo_resposta__in=('hora','data_hora')`; **não filtra materializada** |
| `app/chamados/services/vigia_carimbo.py:106` | truthiness da resposta de `_juiz(p)` |
| `app/chamados/services/vigia_carimbo.py:111` | slot `is None` |
| `app/chamados/services/vigia_carimbo.py:113` | related queryset `retratada_em__isnull=True` + `.exists()` |
| `app/chamados/services/passivo_validado.py:324` | chama `classificar`, importado diretamente em `:40` |
| `app/chamados/services/passivo_validado.py:326` | comparação classe `== 'com_registro'`: pula |
| `app/chamados/services/passivo_validado.py:330` | membership classe em `FORA_DO_ESCOPO`, definido `:60`: pula `plantio_fora_do_slot`/`slot_nao_derivavel` |
| `app/chamados/services/passivo_validado.py:338` | conjunto de PKs do recorte |
| `app/chamados/services/passivo_validado.py:347` | membership da competência no recorte |
| `app/chamados/services/passivo_validado.py:516` | chama `_materializar_uma`, importada diretamente em `:39` |
| `app/chamados/services/relavra_via.py:181` | `Q(materializada_em__isnull=False) & ~Q(via_resolucao__in=VIAS_DE_IDENTIDADE)` |
| `app/chamados/services/relavra_via.py:183` | opção de queryset `disputa__fechada_em__isnull=True` |
| `app/chamados/services/relavra_via.py:242` | `if not pergunta_viva(...)`: não descarimba com lastro |
| `app/chamados/services/relavra_via.py:246` | escritor passa pela porta `p.descarimbar()` |
| `app/chamados/services/relavra_via.py:260` | `disputa is not None` + `fechada_em is not None`; reabre pela porta |
| `app/chamados/management/commands/materializar_passivo_validado.py:68` | command chama `reparar`; `:110` passa `apply=bool(o['apply'])` |
| `app/config/crons.py:830` | declara reparador como ato sob autorização, fora de rotina |

A afirmação ampla “sem porta de reprocessar” **não confere como ausência de porta no sistema**: existem reparador do passivo (`app/chamados/services/passivo_validado.py:289`), command (`app/chamados/management/commands/materializar_passivo_validado.py:68`) e descarimbo orientado pelo juiz (`app/chamados/services/relavra_via.py:242`). O bloqueio de repetição pela **porta normal da validação** confere (`app/chamados/services/validacao.py:396`; `app/chamados/services/materializacao.py:1372`). A própria unidade explica a distinção de universos entre admin e reparador em `app/chamados/services/materializacao.py:1162`.

**Busca do E4b.** `rg --files` com padrão `e4b|matriz_e4|censo_v2|backfill.*fio|fio.*backfill|itens_2` retornou zero. O command existente `backfill_decisoes_pergunta` é da **E1**, explicitamente em `app/config/crons.py:780`; não prova implementação do E4b. O E4b continua registrado como plano em `app/docs/BACKLOG.md:1872`.

**(d) Casa.** **COMPLEMENTO de FIO-E0-V4-E4B**, `app/docs/BACKLOG.md:1872`, dentro de E4-ESCRITORES, `app/docs/BACKLOG.md:1863`. Se o DRY excluir o item por motivo que persiste, o encaminhamento já manda **complemento O261**, `app/docs/BACKLOG.md:1876`, casa estrutural original em `app/docs/BACKLOG.md:1466`. Não cabe declarar BO simples.

**(e) Padrão.** Para o caminho planejado: **Backfill / Reprocessing**, termo em `app/docs/GLOSSARIO.md:23`; fase dentro de **Parallel Change / Expand-Contract**, `app/docs/GLOSSARIO.md:18`. Atenção: a glosa dessa última linha diz CONTRACT “depois (E4b)”, mas a decomposição mais detalhada em `app/docs/BACKLOG.md:1863` põe E4b=BACKFILL e E4d=parar projeção; citar o termo, não usar essa glosa como cronograma.

**(f)** `perguntas de lei: 0`

A falta é de evidência: **depende de dado: quantidade de ocorrências do item 07 no universo E4b por classe, quantidade fora com motivo, quantidade efetivamente reparada e quantidade residual após o DRY**, conforme a medição por classe/dono exigida em `app/docs/CORTES.md:439`. Não transformar ausência de estado/DRY em pergunta de lei.

Linha sugerida para tabela final:

`BO-0611-07-E4B-COBRE | L-061 / C6-C19 / portas de materialização | não/não na conferência; cura [nao verificado] | [nao verificado], estado da cura não definido | COMPLEMENTO de FIO-E0-V4-E4B; residual O261 | Backfill / Reprocessing | 0`

## 10/11 19:38 · sessao e1d8c6284693 · MAPA-FILA-BO · censo · origem=Code

**O requisito de “TODOS” os leitores não ficou certificado para o estado geral de `Batida`: esse fechamento permanece [não verificado].** Os números abaixo distinguem o inventário contado do total global; não apresento o primeiro como prova do segundo.

Nas tabelas de censo, o caminho da primeira coluna e **cada linha** da segunda formam a referência `arquivo:linha`. Contagens são de sítios de código, não de colaboradores, registros ou autoridades.

## BO-0611-05-TEM-GPS-LEITORES

**(a) Lei, juiz e porta.** A lei exige que batida sem coordenadas seja gravada, sem coordenada inventada e sem criar regra nova de geo; consumidores devem consultar a mesma autoridade. A porta de nascimento continua sendo `registrar_batida`. (`app/docs/LEIS.md:78`; `app/docs/CORTES.md:440`; `CLAUDE.md:20`; `app/core/portas.py:103`)

**Sem juiz registrado para “esta batida tem GPS?”.** O registro contém `classificar_posicao`, mas essa função recebe distância, precisão e raio: não recebe o par de coordenadas e não decide sua presença. Hoje essa decisão aparece, entre outros lugares, em `verificar_geofence`, por truthiness. (`app/core/juizes.py:1191`; `app/ponto/services/geofence.py:95`; `app/ponto/services/geofence.py:247`)

O lookup de juízes é documental; o registro vivo é `core/juizes.py`. Portanto, chamar `geofence.py:247` de “juiz registrado” não confere. (`app/docs/JUIZES.md:10`; `app/docs/JUIZES.md:14`; `app/core/juizes.py:1186`)

**(b) Cria estado: não**, considerando a cura descrita de centralizar a presença GPS e migrar leitores: latitude e longitude já são anuláveis, e o alerta `sem_gps` já nasce com ambas nulas. **Muda decisor: sim**: os predicados locais deixariam de decidir individualmente. O predicado normativo que substituiria essas decisões ainda precisa estar definido; seu resultado não pode ser presumido pela implementação atual. (`app/ponto/models.py:58`; `app/ponto/models.py:59`; `app/ponto/services/geofence.py:249`; `app/ponto/services/geofence.py:253`; `CLAUDE.md:175`)

**(c) Censo identificado: 58 leitores + 14 gravações + 4 apagadores = 76 sítios**, no universo de coordenadas persistidas de `Batida`, suas cópias em `AlertaGeofence`, contexto geográfico do chamado e `Advertencia.detalhe_json`. A advertência realmente copia as coordenadas para JSON. (`app/chamados/services/acoes_chamado.py:193`; `app/chamados/services/acoes_chamado.py:203`; `app/ponto/models.py:1173`)

Foram procurados atributos, argumentos ORM, strings de campos, aliases serializados e seus consumidores em Python, templates e JavaScript; pares latitude/longitude na mesma operação contam uma vez. Testes, migrations, coordenadas próprias de Posto/PingGeo/Justificativa e entradas anteriores à persistência ficaram separados. Completude contra reflexão, SQL dinâmico não rastreado e cascatas genéricas: **[não verificado]**. [nao verificado]

Os **36 sítios com predicado de presença ou serialização condicionada à ausência**:

| Arquivo | Linhas | Forma |
|---|---:|---|
| `app/colaboradores/views.py` | 53 | `exclude` das duas coordenadas |
| `app/colaboradores/views.py` | 337, 356, 395 | `exclude(latitude=None).exists()`; somente latitude |
| `app/colaboradores/views.py` | 2474 | `is None`, par |
| `app/colaboradores/painel_cell.py` | 54 | par, com `or ''` |
| `app/colaboradores/services/situacional.py` | 199 | par, com `or ''` |
| `app/colaboradores/services/drawer.py` | 63 | `bool(latitude and longitude)` |
| `app/colaboradores/services/detalhe.py` | 22, 53 | `exclude` das duas coordenadas |
| `app/core/views.py` | 1024 | `is not None`, por eixo |
| `app/ponto/views.py` | 468 | `is not None`, por eixo |
| `app/ponto/views.py` | 485 | `latitude__isnull=False`; somente latitude |
| `app/chamados/views.py` | 692 | `__isnull=False`, par; modelo importado com alias |
| `app/ponto/services/vinculo_na_lista.py` | 49 | latitude não nula e precisão não nula; longitude sem filtro |
| `app/ponto/services/geofence.py` | 52 | `__isnull=False`, par |
| `app/ponto/services/geofence.py` | 134 | `is None`, par do alerta |
| `app/ponto/services/geofence.py` | 247 | truthiness, par da batida |
| `app/ponto/services/geofence.py` | 327, 369 | truthiness para converter/copiar o par |
| `app/ponto/services/geofence.py` | 403 | `is not None`, par extraído do contexto |
| `app/ponto/models.py` | 263 | `is None`, par do alerta |
| `app/ponto/management/commands/reconciliar_alertas_geofence.py` | 42 | `is None`, par |
| `app/templates/chamados/partials/modal_fio.html` | 480 | truthiness, somente latitude |
| `app/templates/chamados/partials/_relatorio_geo.html` | 30 | truthiness, somente latitude |
| `app/templates/ponto/batidas_offline_pendentes.html` | 96 | truthiness, somente latitude |
| `app/templates/core/_d2_feed.html` | 16 | truthiness, somente latitude |
| `app/templates/core/dashboard_supervisor.html` | 90 | truthiness da latitude do alerta |
| `app/templates/colaboradores/detalhe.html` | 142 | truthiness da latitude do alerta |
| `app/templates/core/dashboard2.html` | 242, 287 | JavaScript `lat && lon` |
| `app/templates/colaboradores/partials/_drawer_diagnostico.html` | 49 | leitura de `b.tem_gps` |
| `app/core/services/painel_op.py` | 227 | truthiness por eixo |
| `app/static/js/geo-colab-map.js` | 39 | `isNaN`, par |
| `app/templates/chamados/painel_gestao.html` | 583 | `isNaN`, par |
| `app/templates/chamados/fio_colaborador.html` | 313 | `isNaN`, par |

Os **18 transportes/apresentações**, sem outro predicado próprio de presença:

| Arquivo | Linhas | Forma |
|---|---:|---|
| `app/colaboradores/painel_cell.py` | 37 | par em tupla |
| `app/colaboradores/services/situacional.py` | 141 | `values_list` |
| `app/colaboradores/services/detalhe.py` | 82 | conversão do par por `float` |
| `app/ponto/services/geofence.py` | 275, 282, 293, 302 | cópia para alerta |
| `app/ponto/services/geofence.py` | 383, 384 | `.get` em ocorrência/contexto legado |
| `app/chamados/services/acoes_chamado.py` | 203 | `.get`, cópia para advertência |
| `app/templates/chamados/fio_colaborador.html` | 116, 125 | coordenadas e atributos de dados |
| `app/templates/colaboradores/detalhe.html` | 188 | coordenadas em link |
| `app/templates/colaboradores/partials/_ps_linha.html` | 1 | aliases `b_lat`/`b_lon` em atributos |
| `app/templates/colaboradores/detalhe.html` | 231, 238 | aliases `lat/lng`, centro e marcador |
| `app/ponto/admin.py` | 34 | apresentação como campos somente leitura |
| `app/static/js/geo-colab-map.js` | 101 | aliases `d.blat/blon` copiados ao consumidor |

Os **quatro leitores externos a `app/`**:

| Arquivo | Linhas | Forma |
|---|---:|---|
| `bin/sombra_mascara_pessoal.py` | 51, 54 | carregamento e `getattr` dos campos JSON; um bloco semântico |
| `bin/sombra_regras_pessoais.py` | 36, 43 | percurso recursivo; número em chave geográfica vira `None` |
| `bin/sombra_selo_pessoal.py` | 58 | SQL `IS NOT NULL` |
| `bin/sombra_selo_pessoal.py` | 89, 90 | SQL `jsonb_path_exists`, coordenada diferente de nulo |

As **14 gravações**:

| Arquivo | Linhas | Forma |
|---|---:|---|
| `app/ponto/registro_batida.py` | 152, 157 | dois `Batida.objects.create(**campos)` |
| `app/ponto/registro_batida.py` | 335 | `bulk_create([Batida(**d)])`, somente ensaio |
| `app/ponto/services/geofence.py` | 249, 273, 280, 291, 298 | cinco criações de alerta; nulos ou cópia do par |
| `app/ponto/services/geofence.py` | 337 | anexa ocorrência ao contexto |
| `app/ponto/services/geofence.py` | 339 | cria chamado com contexto geográfico |
| `app/chamados/services/acoes_chamado.py` | 193 | cria advertência com contexto copiado |
| `bin/sombra_mascara_pessoal.py` | 20 | `update` dinâmico; alvos Batida/Alerta declarados no inventário |
| `bin/sombra_mascara_pessoal.py` | 62, 64 | dois `bulk_update` dos JSONs mascarados |

Os alvos dinâmicos da máscara estão declarados para Batida, Alerta e contexto do chamado; a execução exige banco da sombra. (`bin/sombra_regras_pessoais.py:75`; `bin/sombra_regras_pessoais.py:77`; `bin/sombra_regras_pessoais.py:145`; `bin/sombra_mascara_pessoal.py:10`)

Os **quatro apagadores** são `batida.delete()`, o apagador de queryset de ensaio, a exclusão dos alertas de ensaio e a exclusão dos chamados de ensaio que carregam o contexto. (`app/ponto/registro_batida.py:291`; `app/ponto/registro_batida.py:345`; `app/ponto/management/commands/zerar_dados_operacionais.py:85`; `app/chamados/models.py:571`)

Aliases e infraestrutura não podem desaparecer do rastreamento: `_BatidaGeo` é declarado antes do filtro; `_ChG` antes do nascimento; `_nota` encaminha a ocorrência à fusão canônica do contexto. A fusão une listas, preenche chaves e salva o contexto. (`app/chamados/views.py:691`; `app/ponto/services/geofence.py:321`; `app/ponto/services/geofence.py:336`; `app/chamados/services/nota.py:110`; `app/chamados/services/nota.py:123`; `app/chamados/services/contexto.py:147`; `app/chamados/services/contexto.py:155`; `app/chamados/services/contexto.py:163`)

O `save` genérico da porta de anotação não permite alterar coordenadas: elas não pertencem a `CAMPOS_ANOTAVEIS`, e campos externos são recusados. (`app/ponto/registro_batida.py:203`; `app/ponto/registro_batida.py:241`; `app/ponto/registro_batida.py:254`)

O alcance fora dos feeds confere: o detector de vínculo alimenta a lavra usada pelo geofence, e a reavaliação pode retratar chamados. Há cron de lavra, command de reconciliação e signal de Posto. (`app/ponto/services/vinculo_na_lista.py:86`; `app/ponto/services/geofence.py:286`; `app/ponto/services/geofence.py:419`; `app/config/crons.py:597`; `app/ponto/management/commands/detectar_vinculo_divergente.py:35`; `app/ponto/management/commands/detectar_vinculo_divergente.py:52`; `app/ponto/management/commands/reconciliar_geofence.py:33`; `app/ponto/signals.py:306`)

**Conferência do relato.**

- Agrupar `modal_fio` e `_relatorio_geo` como `is not None` **não confere**: ambos usam truthiness da latitude. (`app/templates/chamados/partials/modal_fio.html:480`; `app/templates/chamados/partials/_relatorio_geo.html:30`)
- A decisão repetida na página do feed **confere**. (`app/templates/core/dashboard2.html:242`; `app/templates/core/dashboard2.html:287`)
- `float(None)` é um caminho possível se latitude e precisão estiverem preenchidas e longitude estiver nula; não é ocorrência de produção demonstrada. (`app/ponto/services/vinculo_na_lista.py:49`; `app/ponto/services/vinculo_na_lista.py:56`)
- Os totais “41 leitores” e “35 divergentes” não foram reproduzidos com a mesma unidade/universo: **[não verificado]**.
- A exigência atribuída a `test_bo0611_05_vinculo_geo_parcial.py:135` não foi confirmada nesta cópia: **[não verificado]**.
- **Depende de dado:** contagens de pares nulos, parciais, zeros numéricos e respectivas precisões, por origem/canal, e quantos mudariam de resultado em cada leitor: **[não verificado]**.

**(d) Casa.** **COMPLEMENTO do registro de juízes da família batida/O254**, ligado ao BO pai; não há prova de sítio único para “BO simples”. O contrato de batida e a cura da classe das barreiras já têm casa registrada. (`app/docs/BACKLOG.md:1458`; `app/docs/BACKLOG.md:1873`; `app/core/juizes.py:1186`)

**(e) Padrão.** **Single Source of Truth**, termo existente. (`app/docs/GLOSSARIO.md:11`)

perguntas de lei: 1

“Qual predicado de presença GPS deve valer para zero numérico e par parcial, preservando a proibição de regra nova de geo?” A implementação diverge; uma resposta normativa escrita para esses casos não foi localizada **[não verificado]**. (`app/docs/CORTES.md:440`; `app/ponto/services/geofence.py:247`; `app/ponto/services/geofence.py:52`; `app/ponto/services/vinculo_na_lista.py:49`)

## BO-0611-05-RESTO-DA-CLASSE

**(a) Lei, juiz e porta.** L-030 determina registro sem barreira de runtime e escritor canônico; L-025 determina o universo apurável; L-087 determina o papel pelo instante contra marco, sem deixar o tipo gravado decidir. (`app/docs/LEIS.md:78`; `app/docs/LEIS.md:76`; `app/docs/LEIS.md:119`)

**Sem juiz registrado para “esta tentativa de chão pode nascer?”.** O registro de batida contém períodos, cálculo, geofence e selfie; as decisões de dedup/colisão continuam na porta e nas entradas. Para foto ausente há juiz registrado, cuja consequência é cobrança. (`app/core/juizes.py:1186`; `app/core/juizes.py:1195`; `app/ponto/registro_batida.py:113`; `app/ponto/registro_batida.py:145`; `app/docs/CORTES.md:94`)

A porta é `registrar_batida`; suas bordas incluem as duas APIs, triagem e tela web. (`app/core/portas.py:103`; `app/ponto/registro_batida.py:94`; `app/api/views_core.py:681`; `app/api/views.py:684`; `app/ponto/services/triagem_batida.py:701`; `app/ponto/views.py:1815`)

**Conferência de cada alegação do relato:**

| Alegação | Conferência no código |
|---|---|
| Oposta em 60/300 s | Confere, com exceção quando casa com marco próprio; autoridade humana já pode gravar. (`app/ponto/registro_batida.py:21`; `app/ponto/registro_batida.py:30`; `app/ponto/registro_batida.py:145`; `app/ponto/registro_batida.py:150`; `app/ponto/registro_batida.py:152`) |
| Mesmo tipo em 5 min/60 s | Confere. API usa ±5 min; triagem usa os cinco minutos anteriores; porta usa ±60 s e exclui retratadas. (`app/api/views_core.py:549`; `app/api/views.py:557`; `app/ponto/services/triagem_batida.py:662`; `app/ponto/registro_batida.py:106`) |
| Selfie impeditiva | Confere nas interfaces: retorno impeditivo no v2; exibição do botão condicionada à captura no template antigo. (`app/templates/ponto/bater_ponto_v2.html:287`; `app/templates/ponto/triagem/apto.html:93`; `app/templates/ponto/triagem/apto.html:182`; `app/templates/ponto/triagem/apto.html:253`) |
| Confirmação de turno aberto | Confere como interrupção para confirmar, não como recusa definitiva. (`app/ponto/views.py:316`; `app/ponto/views.py:329`) |
| Onboarding/plataforma | Parcialmente confere: onboarding redireciona a tela; middleware isenta `/api/` e `/webview/`. A afirmação de bloqueio da batida nativa por esse middleware não confere nessas rotas. (`app/ponto/views.py:243`; `app/core/middleware.py:247`; `app/core/middleware.py:254`; `app/core/middleware.py:268`; `app/core/middleware.py:283`) |
| Situação barra login/refresh | Confere para situação diferente de `ativo`; a porta de bater, porém, admite ativo e afastado. (`app/api/login_helpers.py:141`; `app/api/views_core.py:173`; `app/api/views.py:116`; `app/api/views_core.py:402`) |
| Colisão responde 200 e fila entende sucesso | Confere. O retorno atual fica em outra linha: informa `ignorada_intervalo`/`sucesso=True`; o PWA testa `resp.ok` e apaga a pendência. (`app/api/views_core.py:709`; `app/api/views_core.py:716`; `app/api/views.py:712`; `app/static/pwa.js:158`; `app/static/pwa.js:162`) |
| Apenas latitude permite `float(None)` | Confere como caminho possível, também exigindo precisão preenchida para passar pelo queryset de Batida. O queryset de Posto tem risco semelhante. (`app/ponto/services/vinculo_na_lista.py:47`; `app/ponto/services/vinculo_na_lista.py:49`; `app/ponto/services/vinculo_na_lista.py:56`) |
| Falta lei geral para apurar a oposta gravada | **Não confere como afirmação geral**: universo e papel já têm lei; o juiz distingue marco sobrando até 60 s, par de 120 s e outro limite para retratação. (`app/docs/LEIS.md:76`; `app/docs/LEIS.md:119`; `app/ponto/turnos.py:85`; `app/ponto/turnos.py:99`; `app/ponto/turnos.py:104`; `app/ponto/turnos.py:114`) |

Há outro desencontro: a triagem busca a oposta numa janela fixa de 60 s, sem filtro de retratada, enquanto a porta pode recusá-la por uma vizinha até 300 s. Isso permite retorno de colisão sem encontrar o `id` da oposta na recuperação da triagem. (`app/ponto/services/triagem_batida.py:715`; `app/ponto/registro_batida.py:138`; `app/ponto/registro_batida.py:141`)

**(b) Cria estado: sim**, na acepção solicitada de linhas que hoje não nascem pelo fluxo de chão; não exige coluna nova. A combinação oposta já é permitida ao caminho humano, mas o automático pode terminar em `None`. Foto e geo já admitem ausência. **Muda decisor: sim**: efetivar L-030 retira das bordas os vetos de nascimento e preserva o escritor canônico. (`app/ponto/registro_batida.py:150`; `app/ponto/registro_batida.py:152`; `app/ponto/models.py:58`; `app/ponto/models.py:78`; `CLAUDE.md:140`; `app/core/portas.py:103`)

**Depende de dado:** quantidade de tentativas recusadas/interrompidas por barreira, canal, colaborador e instante; colisões sem linha nova; pendências apagadas após esses retornos: **[não verificado]**.

**(c) Inventário contado: 383 sítios em `app/`, mais os complementos externos abaixo.** A unidade foi `(arquivo, linha, tipo de acesso)`, resolvendo imports relativos, aliases locais e aliases de módulo. A decomposição é: 159 seletores ORM, cinco escritas, 116 chamadas às quatro portas de leitura/pareamento, 32 sítios de reverso/tupla, 16 chamadas a `registrar_batida`, sete receivers e 48 linhas de templates/JS. **O total universal transitivo de Batida permanece [não verificado].**

Os **159 seletores ORM**:

Legenda: `F` filtro; `X` exclude; `1` first/get; `E` exists; `V` values/values_list; `A` aggregate/annotate; `C` count; `O` order_by; `L` list/set; `S` select_related/only.

| Arquivo | Linhas | Forma |
|---|---:|---|
| `app/api/hora_do_aparelho.py` | 173 | `all` |
| `app/api/views.py` | 297; 559; 704; 872 | F/O/V/L; F tipo/instante/O/1; F retratada nula/instante/X tipo/O/1; get pk/colaborador |
| `app/api/views_core.py` | 286; 551; 701; 862 | mesmas quatro formas da casca anterior |
| `app/chamados/detector_anomalias.py` | 43 | F pk/1 |
| `app/chamados/juizes.py` | 52; 201; 243 | F pk/S/1; F pks/S; F tipo/instante/O/1 |
| `app/chamados/management/commands/auditar_disputas.py` | 103 | F origem/S |
| `app/chamados/management/commands/carimbar_por_batida_coincidente.py` | 70 | F instante/retratada nula/S/L |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py` | 138 | F pergunta de origem/retratada nula |
| `app/chamados/models.py` | 3467 | F pk/colaborador/V/1 |
| `app/chamados/reconciliador.py` | 206 | F colaborador/instante |
| `app/chamados/services/acoes_chamado.py` | 262; 377 | F pk/colaborador/1; alias `_B`, F data/O |
| `app/chamados/services/adesao.py` | 60, 144 | F colaboradores/retratada nula/V/distinct/L |
| `app/chamados/services/desvio_escala.py` | 60 | F retratada nula/data/V timestamp |
| `app/chamados/services/disputa_emissao.py` | 67; 282; 391 | aliases `_Bg`, `_B`, `_Bub`; F colaborador, pks ou pk; último usa 1 |
| `app/chamados/services/explicador.py` | 40; 53; 132 | F instante/O/1; F/O/V timestamp-tipo/L; F/O/L |
| `app/chamados/services/fila_por_causa.py` | 190 | F colaboradores/retratada nula/instante/O |
| `app/chamados/services/fio.py` | 1492; 1525 | F pk/1; F pk/V colaborador/1 |
| `app/chamados/services/limbo_folgas.py` | 35; 155 | F tipo E/data/retratada nula/V; F data/retratada nula/E |
| `app/chamados/services/materializacao.py` | 94, 100; 634 | F instante/O/1; F perguntas de origem/retratada nula/V/L |
| `app/chamados/services/passivo_validado.py` | 513; 527 | F colaborador/A Max pk; F colaborador/pk maior |
| `app/chamados/services/regularizacao_ext.py` | 73 | F colaborador/instante exato/E |
| `app/chamados/veredito_lavrado.py` | 177; 180 | F criado/V/A Max; F retratada não nula/V/A Max |
| `app/chamados/views.py` | 692 | alias `_BatidaGeo`, F par geo não nulo/O |
| `app/chamados/views_regularizacao.py` | 109 | get pk/colaborador |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py` | 94 | F retratada nula/data/V/L |
| `app/colaboradores/painel_cell.py` | 33 | F instante/O/1 |
| `app/colaboradores/services/calendario.py` | 239 | alias `_Bat`, F retratada não nula/instante/O/L |
| `app/colaboradores/services/detalhe.py` | 20 | F colaborador/X latitude nula/X longitude nula |
| `app/colaboradores/services/drawer.py` | 34 | F instante/S/O/L |
| `app/colaboradores/services/ficha.py` | 145 | F retratada nula/O/S timestamp/1 |
| `app/colaboradores/services/pos_vinculo.py` | 38 | F colaborador/retra

[truncado em 20000 de 60219 caracteres]

## 10/11 19:32 · sessao 2347880ecdab · - · - · origem=Ronald (tmux)

Censo concluído, sem edições.

| Leitor de `motivos` | Resultado com o novo significado |
|---|---|
| `api/foto_recebida.py:61` | Seleciona a dona aberta com `foto_ausente`; A sem pergunta deixa de entrar. `ultima_batida` ainda decide se trata a disputa (`:67`). |
| `colaboradores/signals.py:71` | Revalidação de posto alcança a dona aberta com motivo local; chama a porta de remoção (`:77`). |
| `escala/signals.py:155` | Calibração de âncora alcança a dona aberta com motivo local (`:161`). |
| `chamados/models.py:2031` | Remoção relê e guarda abertura/presença do motivo (`:2033`, `:2036`); compatível. |
| `chamados/management/commands/auditar_disputas.py:88` | Exige contexto apenas dos motivos mantidos na DS; muda o conjunto auditado. |
| Mesmo auditor, `:142` | Texto da ocorrência mostra os motivos locais. |
| Mesmo auditor, `:169` | Comparação com perguntas da própria DS (`:182`) é compatível; diminui `motivo_sem_pergunta` (`:188`). |
| `chamados/management/commands/auditar_invariantes_chamados.py:50` | Resolve dias dos motivos locais. Contador seleciona zero perguntas (`:62`); nova vazia desfeita desaparece do universo, HX preexistente permanece. |
| `chamados/relatorios_disputa.py:52` | Rótulos mostram motivos locais. |
| `chamados/services/acoes_disputa.py:149` | Mensagem de regularização mostra motivos locais. |
| `chamados/services/fio.py:680` | Mesmo efeito na mensagem do fio. |
| `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49` | Índice de `device_comprometido` passa à dona ou omite motivo sem pergunta; contexto ainda é exigido (`:52`). Catálogo possui pergunta de escolha (`chamados/catalogo/perguntas.py:105`). |
| `ponto/management/commands/alertar_disputas_pendentes.py:82` | Log mostra motivos locais. |
| `templates/chamados/detalhe_local.html:301` | Exibe motivos locais. |
| `templates/chamados/partials/modal_fio.html:325` | Aviso de âncora acompanha dona aberta com o motivo (`:326`). |
| `colaboradores/services/drawer.py:75` | Sem mudança direta: já usa perguntas vivas (`:78`), não motivos. |

Não encontrei leitor do censo que exija explicitamente conservar motivos sem pergunta e que necessariamente quebre; efeitos sobre cenas de produção são [nao verificado].

D6: único chamador concreto de `Command._disputa` encontrado é `chamados/tests/test_porta_chamado_e3.py:93`, repetido em `:94`. O método vazio está em `ponto/management/commands/reconciliar_grade.py:412`. Buscas por chamadas, strings, imports e apelidos não encontraram chamador produtivo; reflexão dinâmica é [nao verificado].

Auditoria adicional do replay:

- Reuso salva exclusivamente P no ramo examinado (`chamados/models.py:2997`, `:3006`); rechaveamento também salva P (`chamados/services/fio.py:1043`).
- Receiver de pergunta agenda fechamento de beco mesmo no reuso (`chamados/signals.py:228`, `:238`).
- Esse callback pode fechar dona **aberta** pelas guardas de `chamados/services/becos.py:29`, `:33`, `:74`, chegando a `:57`.
- Dona **fechada** sai antes de escrita (`chamados/services/becos.py:27`, `:74`). No D3 real, ausência de aberta sob trava implica donas fechadas; portanto essa cadeia adicional não demonstra condição de parada alcançável.
- O outro receiver apenas emite evento (`chamados/signals.py:251`, `:257`), cujo escritor cria `EventoCelula` (`ponto/portas/celula.py:887`, `:896`); consumo síncrono não foi encontrado.
- Costura lê/trava e altera P temporariamente em memória (`chamados/services/fio.py:761`, `:779`, `:783`); lavra sem salvar altera atributos de P (`chamados/veredito_lavrado.py:260`, `:270`, `:274`).

Execução ORM, callbacks reais e concorrência: [nao verificado].

## 10/11 19:31 · sessao 675ffdb27b5f · - · - · origem=Ronald (tmux)

Complemento entregue: **116 chamadas adicionais**, com aliases e formas, nas três mensagens anteriores.

A união medida soma **383 sítios `(arquivo, linha, tipo)` em 148 arquivos**: 159 seletores ORM, cinco escritas, 116 chamadas às quatro portas, 32 sítios de reverso/tupla, 16 chamadas a `registrar_batida`, sete signals e 48 linhas de templates/JS. A criação já incluída no seletor não foi contada novamente. (`app/ponto/registro_batida.py:152`, `:157`, `:254`, `:291`, `:335`; demais referências no inventário entregue)

O total universal de leitores transitivos permanece **[não verificado]**; **383 é o conjunto explicitamente inventariado**, sem os complementos de `bin/` que o agente principal encontrou.

## 10/11 19:30 · sessao 1ac3e43926fc · - · - · origem=Ronald (tmux)

**BUG-PAR-FROTA-1110 — resultado consolidado**

- **Lei:** L-025, universo apurável (`app/docs/LEIS.md:76`); L-087, papel por instante/ata/DNA, sem decidir pelo tipo gravado (`app/docs/LEIS.md:119`); consumidor lê lâmpada (`CLAUDE.md:167`); juiz e escritor únicos/idempotência (`CLAUDE.md:175`).
- **Juízes registrados:** turno (`app/core/juizes.py:208`), marcos (`app/core/juizes.py:213`), intervalo (`app/core/juizes.py:214`), completude do par (`app/core/juizes.py:240`), vitalidade C6 (`app/core/juizes.py:278`) e fechamento C7 (`app/core/juizes.py:279`). **Sem juiz registrado para elegibilidade/aplicação conjunta das duas respostas declaradas.** `par_completo` exige batidas existentes no mesmo turno e dia encerrado, portanto não responde sozinho a essa pergunta (`app/ponto/turnos.py:2190`).
- **Quem decide hoje:** a guarda por vizinha de mesmo tipo, com exceção por furo e testemunho do marco (`app/chamados/services/materializacao.py:875`, `app/chamados/services/materializacao.py:901`). A validação executa uma pergunta antes do laço da disputa (`app/chamados/services/validacao.py:461`); o próprio laço também chama a unidade individualmente (`app/chamados/services/materializacao.py:1375`).
- **Portas:** validação (`app/chamados/services/validacao.py:373`), materialização (`app/chamados/services/materializacao.py:1158`), carimbo (`app/chamados/models.py:3103`), identidade (`app/chamados/models.py:3168`), nascimento de Batida (`app/chamados/services/materializacao.py:927` → `app/ponto/registro_batida.py:157`).
- **(b): cria estado SIM**, no sentido solicitado de novas linhas/combinações neste fluxo: duas materializações atualmente impedidas pela guarda individual; preservar metade validada também altera a combinação hoje desfeita pela devolução (`app/chamados/services/materializacao.py:875`, `app/chamados/services/validacao.py:199`). **Nova coluna não:** os campos existem (`app/chamados/models.py:2449`, `app/chamados/models.py:2556`, `app/chamados/models.py:2560`, `app/chamados/models.py:3393`); “estado explícito” foi superado por derivado (`app/docs/BACKLOG.md:1913`). **Muda decisor SIM**, da guarda unitária para julgamento da unidade par; autorização humana continua explícita na porta (`app/chamados/services/validacao.py:373`).
- **Casa:** COMPLEMENTO de `BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS`, que já manda aplicar o par no mesmo ato e esperar a outra metade (`app/docs/BACKLOG.md:1912`). Não é BO simples: validação, materialização, carimbos, C6/C7 e apresentação leem o estado (`app/chamados/services/validacao.py:461`, `app/chamados/models.py:1842`, `app/templates/chamados/partials/modal_fio.html:221`).
- **Padrão:** `Single Writer Principle` (`app/docs/GLOSSARIO.md:12`). “Unidade de trabalho” não consta nos dois vocabulários consultados [não verificado como termo permitido].
- **Produção:** 77 colab-dias/48 colaboradores/18 na competência 11 são somente agregado fornecido [não verificado na árvore]. **Depende de dado:** lista de pares/perguntas, distribuição por empresa/competência, estado das duas metades, existência de Batida por `pergunta_origem`, tipos/marcos/horários e impacto de frota [não verificado].
- **perguntas de lei: 0.** Identificar a mesma pausa é desenho baseado nos marcos; o mandato conjunto/metade aguardando já está escrito (`app/core/juizes.py:213`, `app/docs/BACKLOG.md:1912`).

**Censo entregue ao pai:** inventário completo por estratos nas mensagens anteriores. Convenção proposta: **356 sítios**, deduplicados por instrução AST ou linha de template, para os quatro campos que representam a espera mais seus juízes/properties/portas nomeados: `154 + 133 + 24 + 9 + 1 + 8 + 27`. Os oito externos foram conferidos pelo pai; o número não inclui o estrato geral de Batida nem transforma configuração de agenda em leitor de valor [não verificado quanto ao fechamento global de Batida].

O nascimento implícito precisa constar: `app/chamados/models.py:3017` grava os quatro defaults mesmo sem nomeá-los; os defaults estão em `app/chamados/models.py:2449`, `app/chamados/models.py:2557`, `app/chamados/models.py:2560`, `app/chamados/models.py:3394`. O levantamento direto de campos sozinho perderia esse escritor.

## 10/11 19:29 · sessao f8ac4f4480e8 · - · - · origem=Ronald (tmux)

Confirmado fora de `app/`:

- `bin/sombra_regras_pessoais.py:145` inclui o contexto do chamado; **não inclui** `Advertencia.detalhe_json` no inventário JSON (`:144-158`).
- `bin/sombra_mascara_pessoal.py:20` é **um statement escritor**, alcançando Batida e Alerta pelos dois alvos de `R.NULO` (`bin/sombra_regras_pessoais.py:75`, `:77`).
- JSON: leitura em `bin/sombra_mascara_pessoal.py:51`/`:54`; classificação recursiva em `bin/sombra_regras_pessoais.py:36`/`:43`; dois statements persistentes em `bin/sombra_mascara_pessoal.py:62` e `:64`. `setattr` em `:57` é alteração em memória.
- Dois leitores do selo: SQL `IS NOT NULL` em `bin/sombra_selo_pessoal.py:58` e `jsonb_path_exists(... != null)` em `:89`.
- Apagador genérico adicional do contexto: `app/chamados/models.py:571`, guardado como ensaio em `:570`, chamado por `app/ponto/management/commands/zerar_dados_operacionais.py:84`.

Pela convenção de sítios semânticos já usada: **58 leitores +14 gravações +4 apagadores =76**. São os 68 anteriores, quatro leitores e três escritores da máscara/selo, mais um apagador de chamado. As portas genéricas intermediárias já documentadas permanecem arestas transitivas, sem somar novamente o chamador que fornece o GPS.

O total descreve o inventário estático identificado; completude contra reflexão, SQL construído fora dos caminhos rastreados e cascatas genéricas: **[não verificado]**.

_3478 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
