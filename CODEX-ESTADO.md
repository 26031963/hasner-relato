# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 16:30** · publicado em **08/11/2026 16:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 16:30 · sessao 0f267e96ba3e · - · - · origem=Ronald (tmux)

Comparação integral por `git hash-object` (sem `-w`, sem escrita): os prefixos abaixo foram comparados com os `index OLD` do anexo; isso permite distinguir arquivo inteiro igual de mero contexto semelhante. Atribuição histórica de cada diferença à E1 depende do dado fornecido, pois esta cópia não tem `.git` [nao verificado].

| Arquivo | E1 mexeu desde o OLD do diff? | Comparação e significado |
|---|---|---|
| `app/chamados/juizes.py` | **Sim**, blob atual `8b80f4d4`, OLD `86a84167` | **Conflito semântico direto:** E1 lê o dia pelo discriminador `_dia_de_turno_da_pergunta`, que devolve `None` para conversa; o avulso usa esse helper. E2 troca o corpo por lote de um lendo `getattr(p,'data_evento',None)` cru, incluindo o dia civil de conversa. Preservar apenas o hunk E2 elimina essa proteção da E1. E1: `app/chamados/juizes.py:332`, `:336`, `:946`, `:948`; E2: anexo `app/chamados/juizes.py:934` (retorno `dias_trancados`, linha nova do primeiro hunk). |
| `app/chamados/management/commands/auditar_invariantes_chamados.py` | **Sim**, atual `e18325f6`, OLD `e0faea82` | No trecho afetado pela E2, o S6 atual ainda chama `marcos_do_lote` sem horizonte e `esta_stale` sem `agora`; o hunk E2 acrescenta uma borda única e passa esse instante aos dois leitores. Nenhuma alteração semântica da E1 **nesse trecho** aparece em relação ao OLD do anexo; onde mais o arquivo mudou, não achei por comparação histórica integral. E1: `.../auditar_invariantes_chamados.py:1162`, `:1166`; E2: mesmo caminho `:1160`, `:1162`, `:1165` no hunk novo. |
| `app/chamados/management/commands/backfill_veredito_lavrado.py` | **Não**, atual `31bd3c86` = OLD | Continua chamando `expira_em(p.data_evento)`; E2 passa também `hoje`, acompanhando a assinatura nova. E1: `.../backfill_veredito_lavrado.py:111`; E2: mesmo caminho `:111`. |
| `app/chamados/services/declaracao_texto.py` | **Sim**, atual `ed85ee27`, OLD `58138f03` | A regra de posição do hunk permanece inline na E1 (`primeiro=inicio`, `último=fim`, meio por E/S); E2 extrai `posicao_do_marco`. Porém a E1 também mudou a escolha da pergunta: `_segura_da_lista` agora chama `canonica_do_grupo`; o próprio comentário registra que até v5 era a última por pk. E2 ainda usa `p=ps[-1]`, podendo escolher outro alvo quando coexistirem candidatas. E1: `.../declaracao_texto.py:123`, `:128`, `:251`, `:255`, `:269`; `app/chamados/services/fio.py:1080`, `:1086`, `:1088`. E2: `.../declaracao_texto.py:75`, `:134`; `.../tarefa_do_dia.py:282`. |
| `app/chamados/services/painel.py` | **Não**, atual `2bb2cbe6` = OLD | E1 continua comparando `silenciado_ate` com `timezone.now()`; E2 acrescenta `agora` opcional. É comparação entre instantes, não datas; o estado do chamado continua atual. E1: `.../painel.py:158`, `:188`; E2: mesmo caminho `:158`, `:190`. |
| `app/chamados/services/raiox.py` | **Não**, atual `a8e4f3db` = OLD | E2 muda a data de expiração para a data local de uma borda única, corta marcos por essa borda e considera lavra futura stale; E1 usa `now().date()` e `_jul.date()`. E1: `.../raiox.py:35`, `:36`, `:39`, `:41`; E2: mesmo caminho `:35`, `:37`, `:40`, `:42`. |
| `app/chamados/services/tarefa_do_dia.py` | Novo na E2; não achei na árvore | Não existe versão E1 para comparação. O próprio diff o apresenta como arquivo novo (`/dev/null`, hunk `+1,422`). Ausência na árvore aferida por busca, sem arquivo:linha [nao verificado]. |
| `app/chamados/veredito_lavrado.py` | **Não**, atual `8fe2079b` = OLD | A E2 exige `hoje` em `expira_em` e `agora` em `esta_stale`, acrescenta `ate` aos marcos, usa datas locais e rejeita lavra futura. E1 ainda tem relógio interno e `.date()` sem conversão local. Há uma mudança de **entrada lida** mesmo com o arquivo igual: `p.data_evento` chega pelas portas novas da E1 como dia do turno; conversa continua dia civil, apesar da docstring antiga de `expira_em` associar conversa a `None`. E1: `.../veredito_lavrado.py:95`, `:107`, `:112`, `:173`, `:181`, `:182`, `:185`, `:229`; `app/chamados/services/fio.py:790`, `:800`, `:1421`. E2: `.../veredito_lavrado.py:95`, `:125`, `:187`, `:201`, `:205`, `:246`. |
| `app/escala/services/leitor_celula.py` | **Não**, atual `27c89d48` = OLD | E2 adiciona alimentação de células/vínculos e expõe `acesa_lavrada`. O contrato atual `missing` converte `acesa=None` em “não missing”; E2 transporta o triestado cru para que tarefa consiga responder `nao_sei`. A grade atual já expõe `fora_vinculo` e `isento_ponto`. E1: `.../leitor_celula.py:119`, `:120`, `:133`, `:345`, `:372`, `:376`, `:428`; E2: mesmo caminho `:137`, `:174`, `:350`. |
| `app/ponto/services/cartorio.py` | **Não**, atual `17fbc851` = OLD | E2 extrai `marcos_com_teto`; a unidade continua instante do marco mais minutos, comparado por `<= agora`. O teto padrão atual é 30 minutos; não há regra “fim do turno encerra tudo”. E1: `.../cartorio.py:828`, `:831`, `:845`, `:846`; E2: mesmo caminho `:843`, `:848`, `:859`. |
| `app/ponto/turnos.py` | **Não**, atual `164db997` = OLD | E2 extrai filtro/corpo de cobertura e acrescenta métodos de lote para vínculo, teto e cobertura. A cobertura atual é por datas, vence maior `(data,pk)`, recortada em `[ini,fim]`; `batidas_apuraveis` exclui retratadas pelo estado atual, sem reconstrução histórica. E1: `.../turnos.py:2735`, `:2755`, `:2759`, `:2771`, `:2846`, `:2866`; E2: mesmo caminho `:2757`, `:2762`, `:2770`, `:3049`, `:3056`, `:3064`. |

Referências adicionais relevantes para consolidar o bloco 2:

- **O ponto único não grava Chamado:** `gravar_dia_do_fato` declara e atribui especificamente `PerguntaDisputa.data_evento`; não salva. Seus chamadores salvam no mesmo ato. Não achei campo `ChamadoColaborador.data_evento`. `app/chamados/services/fio.py:790`, `:791`, `:796`, `:800`, `:801`; campo da Pergunta: `app/chamados/models.py:2383`. Ausência do campo no Chamado [nao verificado].
- **Dia gravado já não basta para identidade no acervo:** E1 `_dia_do_fato` lê episódio pelo juiz `_T1`, recusa episódio retratado e distingue chave gravada de identidade; `dia_do_turno_da_linha` centraliza o resultado usado pelos escritores e pela costura. A E2 filtra e agrupa por `data_evento` cru, sem chamar esse juiz/fecho; não se pode presumir que o acervo tenha sido migrado. `app/chamados/services/fio.py:1308`, `:1313`, `:1316`, `:1320`, `:1412`, `:1437`; anexo E2 `app/chamados/services/tarefa_do_dia.py:240`, `:324`, `:329`.
- **Canônica:** resposta do colaborador vence; empate, menor pk. A E2 usa maior pk. Isso é divergência entre leituras escritas, não pergunta sem lei. `app/chamados/services/fio.py:1080`, `:1083`, `:1088`; anexo E2 `app/chamados/services/tarefa_do_dia.py:282`.
- **Instância:** E1 ainda nasce pela posição derivada do motivo, sem índice da segunda pausa; o comentário explicitamente deixa a chave que separa pausas à E2 quando o emissor a passar. `app/chamados/services/declaracao_texto.py:48`, `:50`, `:55`; `app/chamados/models.py:2391`, `:2394`. A E2 já cria rótulos `int_ida:2`, mas busca candidatas por `i['pos']` e declara `instancia_sem_identidade` quando há candidatas. Anexo E2 `app/chamados/services/tarefa_do_dia.py:64`, `:266`, `:267`.
- **Fuso configurado:** São Paulo, `USE_TZ=True`. `app/config/settings/base.py:93`, `:95`.
- **Juiz None de episódio na E1:** `dia_do_turno_da_batida` pode devolver `None`; a porta `recusa_batida_retratada` traduz as causas em exceção. A E2 não consulta esse juiz para classificar a pergunta carregada: não tem um ramo específico para esse None. `app/ponto/turnos.py:2917`, `:2920`, `:2926`; `app/chamados/services/fio.py:1395`, `:1401`, `:1409`; anexo E2 `app/chamados/services/tarefa_do_dia.py:324`.
- **Passivo:** E1 fornece linhas por correspondência exata `(pergunta,ato,ato_em)`, lendo a projeção em memória. A E2 corta pelo `ato_em` que essa autoridade devolve, preservando essa unidade. `app/chamados/models.py:3818`, `:3824`, `:3831`, `:3835`; anexo E2 `app/chamados/services/tarefa_do_dia.py:349`.

## 08/11 16:30 · sessao b791b0234cbc · - · - · origem=Ronald (tmux)

Comparação integral por `git hash-object` sem `-w`: os nove arquivos abaixo coincidem com o blob OLD de `predesenho_e2_proprio.diff`; portanto, **E1 não os modificou desde a base do diff**. A referência de linha local aponta o contexto conferido; o índice OLD do anexo prova a igualdade integral.

| Arquivo | E1 mudou? | Referências e significado no rebase |
|---|---|---|
| `app/chamados/tests/test_porta_veredito_pergunta.py` | Não; OLD/atual `ef664599` | E1:270,275 ainda chama `esta_stale(p)`; E2:270,275 passa `(p, None, timezone.now())`. Adapta o teste à assinatura obrigatória de `agora`; sem disputa textual. |
| `app/chamados/tests/test_selo_concordancia_varredura_stale.py` | Não; `acbe9f88` | E1:58-59 usa marcos sem horizonte e `esta_stale` sem agora; E2:58-60 usa a mesma borda nos dois leitores, acrescenta caso de lavra futura em E2:93-101. |
| `app/chamados/tests/test_selo_lavrado_orfaos.py` | Não; `ace36828` | E1:60-61,85-86,97,103,115-130 conserva assinaturas antigas; E2:60-61,85-86,97,103,115-130 injeta hoje/agora. |
| `app/core/juizes.py` | Não; `02dc1c1f` | E1:149 termina `_C18`; E1:278 termina o registro da família chamado. E2:156-167 e E2:291-292 acrescenta `_C19` e `_C20`, sem autoridade concorrente já adicionada pela E1. |
| `app/docs/ARQUITETURA.mmd` | Não; `0fd088d4` | E1:338,343 / E2:338,343: 18→20 perguntas com juiz; +7→+8 arquivos. Metadados da documentação. |
| `app/docs/CORTES.md` | Não; `e5ed6080` | E1 termina em :423; E2:424-425 acrescenta os cortes de nascimento de `tarefa_do_dia` e declaração do juiz da tranca. |
| `app/docs/JUIZES.md` | Não; `8734ebb4` | E1:70-73 / E2:73-85: as duas entradas são novas, antes de FERIADO/PRAZO. O registro explica função pura, último ato por `ato_em`, lavra vigente e tranca por autoridade. |
| `app/docs/MAPA-JUIZES.md` | Não; `3fd429ab` | E1:103-110 / E2:105-112 recebe as duas perguntas; E1:143 / E2:145 altera 13→12 pendentes de feriado; E1:184-189 / E2:186-191 ajusta contagens. A alteração 13→12 pertence ao diff E2 apresentado; não veio desta E1. |
| `app/docs/MAPA.md` | Não; `38385d46` | E1:88 / E2:88 altera chamado 18/0→20/0; E1:176 / E2:177-178 acrescenta os dois registros. |
| `app/escala/tests/test_signal_ancora.py` | **Sim**; OLD `3c51c993`, atual `fd7bcbc8` | E1:50-55 e :67-69 **já substituiu** a expectativa de apagar por uma pergunta conservada com `VIA_MOTIVO_RETRATADO`. E2:50-62 extrai `_ancora_retratada`, verifica zero vivas, uma retratada e uma `DecisaoPergunta.RETRATACAO`; E2:74 reutiliza o helper. Mesma regra de conservação, diferença de organização e prova adicional da sequência. Não restaurar expectativa de `count()==0`. |
| `app/chamados/tests/test_e2_tarefa_do_dia.py` | Arquivo novo no diff (`/dev/null`, E2:1-1402); não achei na árvore | A inexistência foi conferida por `ls` e `rg --files`; ausência não tem linha local [nao verificado]. |
| `app/escala/tests/test_grade_alimentada.py` | Arquivo novo no diff (`/dev/null`, E2:1-61); não achei na árvore | Mesma limitação de citação para inexistência [nao verificado]. O anexo E2:44-61 prova o contrato alimentada=consulta e triestado. |

Impactos indiretos dos testes: [nao verificado]

- A E1 escreve o dia da pergunta pelo ponto único `app/chamados/services/fio.py:790-801`, recebe o resultado do juiz e não calcula nem salva ali; o nascimento chama esse ponto e depois salva em `app/chamados/models.py:2919-2923`. A rechave chama o mesmo ponto, troca célula, lavra e salva em `app/chamados/services/fio.py:989-995`.
- A conversa canônica **tem data civil, nunca NULL**, conforme `app/chamados/models.py:2750-2758` e `app/chamados/services/fio.py:1421`. Os testes imutados ainda dizem “conversa sem dia” em `app/chamados/tests/test_porta_veredito_pergunta.py:277-281` e `app/chamados/tests/test_selo_lavrado_orfaos.py:92-97`; o primeiro fabrica a linha nula diretamente em :291, portanto não prova a porta `abrir_conversa`. O diff E2 adapta as assinaturas, mas conserva essa prosa antiga nos hunks desses testes.
- A E1 atual **ainda permite o reuso nulo pular a rechave**, pela guarda `ex.data_evento is not None` de `app/chamados/models.py:2862`; portanto o contrato 15e anexado não pode ser considerado implementado nesta árvore. O rascunho 15e reconhece isso na linha A10 da própria matriz anexada; sem numeração de linha fornecida para esse Markdown [nao verificado].
- A identidade em E1 inclui dia do turno, motivo e instância em `app/chamados/models.py:2387-2395`, mas `instancia_marco` ainda vem de `instancia_do_motivo` (:2391-2392). Isso não acrescenta por si um índice da segunda pausa; a E2 declara o limite em `predesenho_e2_tarefa_do_dia_e93205c6.py:273-281`.
- Não executei testes nem acessei banco; resultados de execução da composição E1+E2 não verificados [nao verificado]. [nao verificado]

## 08/11 16:30 · sessao b7b0fe58cca6 · - · - · origem=Ronald (tmux)

Recomendo **“perguntas de lei: 0 — não achei”**, sem afirmar que a implementação está correta. A lei exige procurar resposta existente antes de pedir corte e distingue decisão técnica de pergunta de lei. (`CLAUDE.md:25`, `CLAUDE.md:266`, `CLAUDE.md:273`)

Pontos que não justificam, por si, pergunta de lei:

- **Duas pausas:** a própria E1 declara que a identidade capaz de separá-las pertence à E2; atualmente recusa `instancia_marco` diferente da derivada do motivo. É pendência de desenho/contrato diante da missão por instância, não ausência demonstrada de lei. (`app/chamados/models.py:3341`, `app/chamados/models.py:3349`; corte FIO do anexo `predesenho_e2_proprio.diff`, hunk CORTES `+424`)
- **Juiz ausente/None:** mostrar na matriz o comportamento efetivo, a propagação e os caminhos sem teste. Não há autorização para recalcular a autoridade em outro leitor. (`CLAUDE.md:20`, `CLAUDE.md:166`) [nao verificado]
- **Antes/depois do turno:** a comparação precisa ser por instante ou fato encerrado; não falta lei sobre o teto temporal. (`CLAUDE.md:185`)
- **Tranca:** o aval fornecido manda consultar o juiz e determina dia trancado sem tarefa, com toque humano devolvendo julgamento normal. (`aval_lei_tranca_e2_juiz.txt:1`)
- **Devido pela grade:** o aval fornecido escolhe expressamente `grade_da_celula` e proíbe leitura da ata crua pela tarefa. (`aval_e2_devido_pela_grade.txt:1`)
- **Horizonte da obra:** há regra escrita para obra nova do horizonte para frente, passado como foto até fato novo. (`app/docs/LEIS.md:145`, `app/docs/CORTES.md:419`)

Atenção à premissa **Chamado/PerguntaDisputa.data_evento**: o ponto único declara e atribui **PerguntaDisputa.data_evento**; não achei declaração de `ChamadoColaborador.data_evento`. Para o chamado, o código mostrado consulta `dia_do_fato(ch, fortes=True)`. (`app/chamados/services/fio.py:790`, `app/chamados/services/fio.py:800`, `app/chamados/models.py:2383`, `app/chamados/services/fio.py:84`)

Referências úteis adicionais:

- Lâmpada: `CLAUDE.md:166`.
- L-051, arquivo reversível por toque: `app/docs/LEIS.md:92`.
- Relógio recebido do chamador: `app/docs/CORTES.md:370`.
- Autoridade de dia sem trabalho e tratamento de `trabalha=None`: `app/docs/CORTES.md:416`.
- E1: juiz sem resposta não cai na data gravada; episódio sem dia tem recusas nomeadas: `app/chamados/services/fio.py:1318`, `app/chamados/services/fio.py:1340`.
- Conversa guarda dia civil, por porta própria: `app/chamados/models.py:2750`.

## 08/11 15:16 · sessao 156252195190 · FIO-E0-V4-E1 · leitura · origem=Code

**1) `ReusoSemRechave`: origem, propagação e limites dos lotes**

A exceção nasce quando a canônica tem `data_evento != dia_do_fato` e `rechavear_no_reuso` devolve recusa; o lançamento acontece antes da atualização de célula/destino, dentro do `atomic` de `_nascer`. A hierarquia é `ReusoSemRechave → FatoEmOutroDia → ResolvedoraCruzada → ValueError`. (`app/chamados/models.py:2862`, `app/chamados/models.py:2867`, `app/chamados/models.py:2870`, `app/chamados/models.py:2770`, `app/chamados/models.py:2335`, `app/chamados/models.py:2291`, `app/chamados/models.py:2284`)

Há quatro sítios de chamada direta a `PerguntaDisputa.abrir`: dois na emissão e dois na declaração humana. **Chamar `abrir` não prova que o caminho alcance esta exceção:** os chamadores de `:834` e das declarações humanas produzem motivos MARCO; o cenário D0/DIA de episódio BATIDA exige distinguir essa família. (`app/chamados/services/disputa_emissao.py:444`, `app/chamados/services/disputa_emissao.py:834`, `app/chamados/services/declaracao_texto.py:348`, `app/chamados/services/declaracao_texto.py:636`, `app/chamados/catalogo/motivos.py:34`, `app/chamados/catalogo/motivos.py:42`, `app/chamados/catalogo/motivos.py:48`)

**Caminho datado que aceita motivos BATIDA — `disputa_emissao.py:444`.**

A cadeia comum é:

`criar_ou_atualizar_disputa` → `_abrir_ou_agregar_disputa` → `gerar_perguntas_disputa` → laço dos motivos → `PerguntaDisputa.abrir` → `_nascer` → `raise ReusoSemRechave`. (`app/chamados/services/disputa_emissao.py:662`, `app/chamados/services/disputa_emissao.py:670`, `app/chamados/services/disputa_emissao.py:682`, `app/chamados/services/disputa_emissao.py:312`, `app/chamados/services/disputa_emissao.py:444`, `app/chamados/models.py:2746`, `app/chamados/models.py:2867`)

| Borda até a cadeia comum | Quem captura | Resto do lote e transação |
|---|---|---|
| API: rota → `api_bater_ponto` → emissão. (`app/api/urls.py:67`, `app/api/views.py:400`, `app/api/views.py:794`) | Nenhum `except` envolve a chamada da emissão. Os `except Exception` posteriores protegem foto e geofence, não essa chamada. (`app/api/views.py:794`, `app/api/views.py:803`, `app/api/views.py:812`) | Abandona os motivos restantes e o restante da view. O `atomic` de `_nascer` é desfeito; a abertura/agregação anterior e perguntas anteriores não estão num único `atomic` dessa função. Sem transação externa, permanecem gravadas. Transação acrescentada pelo ambiente da request: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:312`, `app/chamados/models.py:2770`, `app/chamados/services/disputa_emissao.py:521`, `app/chamados/services/disputa_emissao.py:669`) |
| API core: rota → `api_bater_ponto` → emissão. (`app/api/urls_core.py:14`, `app/api/views_core.py:387`, `app/api/views_core.py:794`) | Mesmo limite: nenhuma captura envolvendo a emissão; foto/geofence têm capturas posteriores. (`app/api/views_core.py:794`, `app/api/views_core.py:803`, `app/api/views_core.py:812`) | Mesmo abandono dos motivos e da view, com rollback somente dos blocos atravessados pela exceção. Transação externa da request: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:312`, `app/chamados/services/disputa_emissao.py:521`, `app/chamados/models.py:2770`) |
| Cron da selfie → command → laço de batidas → emissão. (`app/config/crons.py:686`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:58`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:95`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`) | Sem captura e sem `atomic` envolvendo o lote no command. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:94`) | Uma exceção propagada abandona as batidas/colaboradores seguintes; não incrementa `n` naquela iteração nem imprime o resumo final. Iterações anteriores não são revertidas por um bloco global do command. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:95`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:106`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:107`) |

**Precisão sobre a selfie:** o command passa `foto_ausente`, mas não passa `data` nem `ultima_batida`; numa disputa recém-nascida, isso conduz ao ramo `abrir_conversa`, não automaticamente a `:444`. Na disputa existente, o merge preserva outras chaves do contexto; a passagem efetiva pelo ramo datado depende desse contexto. Sua ocorrência no dado fornecido é **[não verificado]**. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`, `app/chamados/services/disputa_emissao.py:371`, `app/chamados/services/disputa_emissao.py:387`, `app/chamados/services/disputa_emissao.py:439`, `app/chamados/services/disputa_emissao.py:548`, `app/chamados/services/disputa_emissao.py:555`)

O `except Exception` em `disputa_emissao.py:466` **não captura a exceção de `:444`**: seu `try` começa depois da abertura e protege somente o reatamento da pergunta já criada, com savepoint próprio. (`app/chamados/services/disputa_emissao.py:444`, `app/chamados/services/disputa_emissao.py:462`, `app/chamados/services/disputa_emissao.py:463`, `app/chamados/services/disputa_emissao.py:466`)

**Agrupador e chamada `disputa_emissao.py:834`.**

As entradas do helper são os intervalos, os complementos de marcos e os resultados de `classificar_falta`; todos correspondem aos quatro motivos MARCO. Para MARCO, `do_fato` consulta o dia exato; a busca por episódio que atravessa datas gravadas é restrita à família BATIDA. Portanto, **esses caminhos não demonstram o alcance do cenário BATIDA D0/DIA**. (`app/chamados/services/disputa_emissao.py:1055`, `app/chamados/services/disputa_emissao.py:873`, `app/chamados/services/disputa_emissao.py:802`, `app/escala/servico_jornada.py:267`, `app/chamados/services/declaracao_texto.py:42`, `app/chamados/models.py:2677`, `app/chamados/models.py:2686`, `app/chamados/models.py:2713`)

O limite transacional existente, para uma exceção propagada de `abrir`, é um único `atomic` envolvendo **todo o agrupamento daquele colaborador**: complemento inicial, intervalos, ata, grupos com origem, chamados sem origem e entradas pendentes. Não há captura em torno de `:834`; o `except Exception` de `:865` termina antes do laço que chama o helper em `:878`. A propagação abandona os demais laços e desfaz todo esse agrupamento. (`app/chamados/services/disputa_emissao.py:964`, `app/chamados/services/disputa_emissao.py:990`, `app/chamados/services/disputa_emissao.py:1062`, `app/chamados/services/disputa_emissao.py:1084`, `app/chamados/services/disputa_emissao.py:1122`, `app/chamados/services/disputa_emissao.py:1153`, `app/chamados/services/disputa_emissao.py:1179`, `app/chamados/services/disputa_emissao.py:834`, `app/chamados/services/disputa_emissao.py:865`, `app/chamados/services/disputa_emissao.py:878`)

As bordas desse agrupador e seus tratamentos são:

| Borda → agrupador | Captura, continuação e trilha |
|---|---|
| Cron de alertas avançados → laço de colaboradores → agrupador. (`app/config/crons.py:363`, `app/ponto/management/commands/processar_alertas_avancados.py:29`, `app/ponto/management/commands/processar_alertas_avancados.py:158`) | `except Exception` em `:159`; imprime a falha em stdout e segue outros colaboradores. O chamado aberto antes fica fora do rollback do agrupador; `processados` ainda aumenta, sem ser contador da recusa. (`app/ponto/management/commands/processar_alertas_avancados.py:159`, `app/ponto/management/commands/processar_alertas_avancados.py:160`, `app/ponto/management/commands/processar_alertas_avancados.py:121`, `app/ponto/management/commands/processar_alertas_avancados.py:164`) |
| Cron de alertas de turno → laço → `_marco_critico` → `_abrir_chamado_critico` → agrupador. (`app/config/crons.py:362`, `app/ponto/management/commands/processar_alertas_turno.py:81`, `app/ponto/management/commands/processar_alertas_turno.py:141`, `app/ponto/management/commands/processar_alertas_turno.py:316`, `app/ponto/management/commands/processar_alertas_turno.py:384`) | `except Exception` em `:385`; imprime erro, retorna o chamado e permite continuar o processamento. O chamado nasceu antes do agrupador; não há contador específico da recusa nesse tratamento. (`app/ponto/management/commands/processar_alertas_turno.py:385`, `app/ponto/management/commands/processar_alertas_turno.py:386`, `app/ponto/management/commands/processar_alertas_turno.py:387`, `app/ponto/management/commands/processar_alertas_turno.py:335`) |
| Cron de ausências → laços → `_abrir_chamado` → agrupador. (`app/config/crons.py:364`, `app/ponto/management/commands/detectar_ausencias.py:101`, `app/ponto/management/commands/detectar_ausencias.py:138`, `app/ponto/management/commands/detectar_ausencias.py:439`, `app/ponto/management/commands/detectar_ausencias.py:801`) | `except Exception` em `:802`; imprime em stderr e continua. O chamado anterior permanece; o contador `abertos` ainda aumenta e não conta recusas de pergunta. (`app/ponto/management/commands/detectar_ausencias.py:802`, `app/ponto/management/commands/detectar_ausencias.py:803`, `app/ponto/management/commands/detectar_ausencias.py:693`, `app/ponto/management/commands/detectar_ausencias.py:448`) |
| Fio/modal → `processar_post_fio` → `acao_iniciar_disputa_orfaos` → agrupador. (`app/chamados/urls.py:21`, `app/chamados/urls.py:39`, `app/chamados/services/fio.py:551`, `app/chamados/services/acoes_disputa.py:221`) | Sem captura aplicável nessa cadeia; o agrupamento é revertido e o log de sucesso posterior não é alcançado. Tratamento externo à aplicação: **[não verificado]**. (`app/chamados/services/acoes_disputa.py:210`, `app/chamados/services/acoes_disputa.py:226`, `app/chamados/services/disputa_emissao.py:964`) |
| Command de passivo → `pergunta_no_app.perguntar` → laço de colaboradores → agrupador. (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29`, `app/chamados/services/pergunta_no_app.py:175`, `app/chamados/services/pergunta_no_app.py:184`, `app/chamados/services/pergunta_no_app.py:186`) | Sem captura/`atomic` global do lote: abandona os colaboradores seguintes; os agrupamentos anteriores já terminaram. O contador posterior daquela iteração e o resumo não são alcançados. (`app/chamados/services/pergunta_no_app.py:184`, `app/chamados/services/pergunta_no_app.py:187`, `app/chamados/services/pergunta_no_app.py:188`) |
| “Cobrar dia” → cartório e, depois, agrupador. (`app/chamados/urls.py:32`, `app/chamados/views_cobrar.py:325`, `app/chamados/services/cobrar_dia.py:39`, `app/chamados/services/cobrar_dia.py:47`) | Sem captura envolvendo essas chamadas. O `except ValueError` da view só cobre o parse anterior da data. A trilha `cobrar_dia_manual` é anterior e registra o toque, não a recusa. (`app/chamados/views_cobrar.py:320`, `app/chamados/views_cobrar.py:322`, `app/chamados/services/cobrar_dia.py:37`) |

**Sinais e cartório.**

A cadeia de emissão é `julgar_celula → julgar_colab → perguntar_pela_celula → perguntar → criar_ou_atualizar_disputa/gerar_perguntas_disputa → abrir(:444)`. Ela gera os motivos de marcos da ata/lei; não constitui demonstração do cenário de episódio BATIDA D0/DIA. (`app/ponto/services/cartorio.py:1989`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:186`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/fabrica_da_celula.py:151`, `app/chamados/services/disputa_emissao.py:444`, `app/chamados/services/fabrica_da_celula.py:47`, `app/chamados/services/fabrica_da_celula.py:55`)

Os limites de captura dessa cadeia são:

| Entrada no julgamento | Captura e efeito sobre o lote |
|---|---|
| `post_save` de Chamado → cartório. (`app/chamados/signals.py:188`, `app/chamados/signals.py:203`) | `engolir` envolve o julgamento inteiro; captura `Exception` em `core/observ.py:90`. Uma exceção propagada encerra os motivos/células restantes desse julgamento e volta ao escritor original. Não atravessa o `atomic` externo desse escritor; o receiver não tem um `atomic` próprio que reverta todo o julgamento anterior. Registra ERROR e incrementa contador em memória por operação. (`app/chamados/signals.py:195`, `app/core/observ.py:90`, `app/core/observ.py:75`, `app/core/observ.py:77`, `app/core/observ.py:79`, `app/ponto/services/cartorio.py:995`) |
| Save/delete de Batida, save/delete de Ausencia e save de Justificativa → `_julgar_dias` → cartório. (`app/ponto/signals.py:135`, `app/ponto/signals.py:140`, `app/ponto/signals.py:147`, `app/ponto/signals.py:152`, `app/ponto/signals.py:165`, `app/ponto/signals.py:93`) | O mesmo `except Exception` de `engolir`, mas envolvendo **todo o laço de dias**: a primeira exceção abandona os dias seguintes desse receiver. Não propaga para reverter a escrita original; deixa ERROR e contador em memória. (`app/ponto/signals.py:88`, `app/ponto/signals.py:92`, `app/core/observ.py:90`, `app/core/observ.py:75`, `app/core/observ.py:79`) |
| Reescrita de célula → `_o_chao_do_dia_mudou` → cartório. (`app/ponto/portas/celula.py:354`, `app/ponto/portas/celula.py:627`, `app/ponto/portas/celula.py:434`) | `except Exception` **por dia**, em `:437`; registra `logger.exception` e segue os demais dias e o restante da operação. Não há rollback global por propagação dessa exceção. (`app/ponto/portas/celula.py:430`, `app/ponto/portas/celula.py:437`, `app/ponto/portas/celula.py:439`, `app/ponto/portas/celula.py:445`) |
| Pós-vínculo → laço de dias → cartório. (`app/colaboradores/services/vinculo.py:330`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:237`, `app/colaboradores/services/pos_vinculo.py:63`, `app/colaboradores/services/pos_vinculo.py:72`) | `except Exception` **por dia**, em `:75`; registra a falha e continua os dias e as etapas posteriores. O placar de julgadas só aumenta depois do retorno; não existe contador específico da recusa nesse tratamento. (`app/colaboradores/services/pos_vinculo.py:73`, `app/colaboradores/services/pos_vinculo.py:75`, `app/colaboradores/services/pos_vinculo.py:76`, `app/colaboradores/services/pos_vinculo.py:77`, `app/colaboradores/services/pos_vinculo.py:80`) |

**Reconciliador.**

O reconciliador entra por batida ou pelo callback do filho que saiu de vivo. Ele não chama `PerguntaDisputa.abrir` diretamente: a reemissão/alteração de Chamado pode acionar o sinal do cartório acima. (`app/ponto/signals.py:215`, `app/chamados/signals.py:355`, `app/chamados/reconciliador.py:910`, `app/chamados/reconciliador.py:523`, `app/chamados/signals.py:188`)

| Cadeia | Captura e continuação |
|---|---|
| Laço de órfãs → `reatar_pergunta_orfa` → reemissão → `ChamadoColaborador.abrir` → sinal → cartório → fábrica → `abrir(:444)`. (`app/chamados/reconciliador.py:1520`, `app/chamados/reconciliador.py:1522`, `app/chamados/reconciliador.py:1617`, `app/chamados/reconciliador.py:910`, `app/chamados/signals.py:203`) | A captura mais próxima de uma exceção da emissão é o `engolir` do sinal. Por essa cadeia, o `except Exception` do laço de órfãs não a recebe: o sinal já a consumiu. O laço pode continuar e a transação externa não é revertida pela propagação dessa exceção. (`app/chamados/signals.py:195`, `app/core/observ.py:90`, `app/chamados/reconciliador.py:1524`) |
| Laço de premissas → `_retratar` → `atomic` → `ch.reconciliar` → save de estado → mesmo sinal. (`app/chamados/reconciliador.py:1452`, `app/chamados/reconciliador.py:1460`, `app/chamados/reconciliador.py:522`, `app/chamados/reconciliador.py:523`, `app/chamados/models.py:296`) | Mesmo consumo no sinal, antes do `except Exception` externo de `:1462`; os outros chamados continuam e os blocos externos não são desfeitos por essa propagação. (`app/chamados/signals.py:195`, `app/core/observ.py:90`, `app/chamados/reconciliador.py:1462`) |
| Construção do contexto C2 → `julgar_celula` → fábrica → `abrir(:444)`. (`app/chamados/reconciliador.py:691`, `app/chamados/reconciliador.py:702`) | Aqui há captura própria: `except Exception` em `:709`; registra a falha e continua devolvendo contexto, sem o enriquecimento de marcos que não terminou. Não há savepoint próprio envolvendo todo esse julgamento. (`app/chamados/reconciliador.py:709`, `app/chamados/reconciliador.py:710`, `app/chamados/reconciliador.py:690`) |

O cron atual `reconciliar_chamados --alarme` é vigia: consulta e conta; não executa essas cadeias escritoras. (`app/config/crons.py:395`, `app/chamados/management/commands/reconciliar_chamados.py:49`, `app/chamados/management/commands/reconciliar_chamados.py:71`, `app/chamados/management/commands/reconciliar_chamados.py:102`)

**Demais crons/commands que chegam à fábrica pelo cartório.** São caminhos de emissão MARCO, com estes limites para exceções propagadas do julgamento; não provam alcance do cenário BATIDA. (`app/chamados/services/fabrica_da_celula.py:47`, `app/chamados/services/fabrica_da_celula.py:55`)

| Borda → cartório | Captura / lote / rollback |
|---|---|
| Cron `processar_cartorio --apply` → laço de colaboradores → `julgar_colab`. (`app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/management/commands/processar_cartorio.py:65`) | Sem captura ou `atomic` global do command: abandona colaboradores seguintes; gravações anteriores não são revertidas por um bloco global. O resumo final não é alcançado. (`app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/management/commands/processar_cartorio.py:66`) |
| Cron `vigia_de_hora` → command → `rodar` → laço → `julgar_celula`. (`app/config/crons.py:367`, `app/ponto/management/commands/vigia_de_hora.py:28`, `app/ponto/services/vigia_de_hora.py:115`, `app/ponto/services/vigia_de_hora.py:131`) | Sem captura; abandona o restante do lote. O `finally` libera o advisory lock; não reverte os itens anteriores. Os contadores posteriores ao retorno não aumentam para o item interrompido. (`app/ponto/services/vigia_de_hora.py:132`, `app/ponto/services/vigia_de_hora.py:134`) |
| Cron encadeado `disparar_perguntas_competencia` → `_rodar` → laço → `perguntar` → geração. (`app/config/crons.py:1516`, `app/chamados/management/commands/disparar_perguntas_competencia.py:273`, `app/chamados/management/commands/disparar_perguntas_competencia.py:282`) | Em apply, um `atomic` engloba todos os itens: uma exceção propagada aborta e reverte **todo o lote desse bloco**. No dry, `except RuntimeError` não captura `ReusoSemRechave`, que é `ValueError`. O caminho da célula é apenas vigia; quem escreve aqui é o fio mudo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:290`, `app/chamados/management/commands/disparar_perguntas_competencia.py:307`, `app/chamados/management/commands/disparar_perguntas_competencia.py:316`, `app/chamados/management/commands/disparar_perguntas_competencia.py:240`, `app/chamados/models.py:2284`) |
| `cobrar_furos_sem_cobranca --apply` → `cobrar` → laço → `julgar_celula`. (`app/ponto/management/commands/cobrar_furos_sem_cobranca.py:30`, `app/ponto/services/furos_sem_cobranca.py:124`, `app/ponto/services/furos_sem_cobranca.py:128`) | Sem captura/`atomic` global: abandona os itens seguintes; os anteriores permanecem. O contador daquela iteração fica depois da chamada interrompida. (`app/ponto/services/furos_sem_cobranca.py:124`, `app/ponto/services/furos_sem_cobranca.py:131`) |
| `rejulgar_ausencia_em_folga --apply` → laço → `julgar_celula`. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:77`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:78`) | Sem captura/`atomic` global; abandona os dias seguintes e o resumo. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:77`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:80`) |
| `curar_residuo_bug115` → laços → `_lavrar` → `julgar_celula`. (`app/ponto/management/commands/curar_residuo_bug115.py:97`, `app/ponto/management/commands/curar_residuo_bug115.py:99`, `app/ponto/management/commands/curar_residuo_bug115.py:143`) | Sem captura envolvendo essa chamada; abandona o restante dos laços, sem rollback global deles. (`app/ponto/management/c

[truncado em 20000 de 33247 caracteres]

## 08/11 15:11 · sessao 4cbd0bcd30a1 · - · - · origem=Ronald (tmux)

Todas as referências abaixo são relativas à raiz da cópia.

**Núcleo e propagação**

- `ReusoSemRechave` herda `FatoEmOutroDia` → `ResolvedoraCruzada` → `ValueError`; o lançamento está em `app/chamados/models.py:2867`, dentro do `atomic` de `_nascer` em `app/chamados/models.py:2770`. A recusa ocorre antes da costura de célula/destino em `app/chamados/models.py:2870`; não há captura nesse núcleo. (`app/chamados/models.py:2335`, `app/chamados/models.py:2291`, `app/chamados/models.py:2284`)
- `gerar_perguntas_disputa` percorre motivos em `app/chamados/services/disputa_emissao.py:312` e chama `abrir` em `app/chamados/services/disputa_emissao.py:444`, sem captura. O `except Exception` de `app/chamados/services/disputa_emissao.py:466` envolve apenas o reatamento posterior (`:463–465`): **não captura a exceção de `:444`**. A exceção interrompe os motivos restantes e não alcança `criadas.append` daquela pergunta em `app/chamados/services/disputa_emissao.py:470`.
- `criar_ou_atualizar_disputa` abre/agrega antes, por `_abrir_ou_agregar_disputa` (`app/chamados/services/disputa_emissao.py:662`), cujo `atomic` está em `app/chamados/services/disputa_emissao.py:521`; depois chama o gerador em `:670` ou `:682` e pendentes em `:685`. Sem uma transação externa, a abertura/agregação anterior já terminou seu bloco; não há um `atomic` único envolvendo o gerador nessa função. (`app/chamados/services/disputa_emissao.py:603`, `app/chamados/services/disputa_emissao.py:669`)
- `perguntar_entradas_pendentes` percorre dias em `app/chamados/services/disputa_emissao.py:287`, chama o gerador em `:294` sem captura e só salva contexto depois em `:296`. Uma exceção abandonaria os dias restantes e esse `save`. Contudo seus motivos vêm de `classificar_falta` (`:288`), isto é, MARCO; veja restrição de alcance abaixo.
- A fábrica de célula percorre motivos em `app/chamados/services/fabrica_da_celula.py:145`, chama `criar_ou_atualizar_disputa` em `:149` e o gerador em `:151`, sem captura. `perguntar_pela_celula` a chama em `:186`; `cartorio.julgar_colab` chama essa porta no laço de células em `app/ponto/services/cartorio.py:995` e `:997`. O contador de perguntas só aumenta depois do retorno em `app/ponto/services/cartorio.py:999`; o `finally` começa em `:1051`, sem capturar a exceção.

**Bordas diretas de `criar_ou_atualizar_disputa` → gerador `:444`**

| Borda/cadeia | Captura e efeito observável |
|---|---|
| `app/api/urls.py:67` → `app/api/views.py:400` → `:794` → emissão `:662` → `:670/:682` → gerador `:444` | Não há `except` envolvendo `:794`; os `except Exception` de `app/api/views.py:803` e `:812` são de operações posteriores. A view não declara `atomic` em sua entrada (`app/api/views.py:398`). Tratamento final pelo framework/deployment: **[não verificado]**. |
| `app/api/urls_core.py:14` → `app/api/views_core.py:387` → `:794` → mesma cadeia | Mesma ausência de captura no ponto de chamada; capturas posteriores em `app/api/views_core.py:803` e `:812`; entrada em `app/api/views_core.py:385`. Tratamento externo: **[não verificado]**. |
| `app/ponto/urls.py:11` ou `:12` → `app/ponto/views.py:242` → `:350` → `ramo_registrar_es`, `app/ponto/services/triagem_batida.py:616` → `:734` → mesma cadeia | Não há captura em torno de `:734` nem da delegação em `app/ponto/views.py:350`. Esse ramo emite apenas `saida_sem_entrada` (`app/ponto/services/triagem_batida.py:736`), portanto **não alcança o cenário BATIDA D0→DIA** pelas identidades normais da árvore. |
| Cron `app/config/crons.py:686` → `marcar_foto_ausente_retro.handle`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:58` → laço `:95` → `:96` → mesma cadeia | Sem captura/`atomic` de lote no comando. Exceção abandona batidas restantes e não imprime o resumo final em `:107`; o contador `n` só aumenta em `:106` e não conta recusas. **Atenção:** o contexto passado em `:98` não tem `data`/`ultima_batida`; com disputa recém-nascida o gerador acaba em `abrir_conversa` (`emissão :439`), não em `:444`. Disputa existente conserva outras chaves no merge (`emissão :548/:555/:559`), portanto o ramo datado depende do contexto existente; sua ocorrência em produção é **[não verificado]**. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:273` → `:282` → fábrica `:149/:151` → gerador `:444` | `--apply`: um único `atomic` para todos os itens em `:290`, sem captura aplicável; exceção abandonaria o lote e reverteria esse bloco todo; resumo de sucesso `:296` não é alcançado. Dry: `atomic :307`; `except RuntimeError :316` não captura `ReusoSemRechave`. É caminho MARCO, não cenário BATIDA. Encadeamento de cron em `app/config/crons.py:1516`. |
| `smoke_chamados.handle`, `app/chamados/management/commands/smoke_chamados.py:39` → `_bloco_a_motivos :41` → laço `:91` → `:102` → gerador `:444` | `except Exception` em `:114`, incrementa `resumo["erro"]` em `:115` e guarda texto em `problemas :116`; segue próximo motivo. A transação externa permanece até o rollback proposital `_Rollback` em `:43`, capturado em `:44`. Command de smoke manual, registrado como tal em `app/config/crons.py:1428`; não confundir com emissor operacional. |

**Agrupador de órfãos: bloco e bordas**

O agrupador tem um `atomic` único em `app/chamados/services/disputa_emissao.py:964`, envolvendo complemento inicial (`:974/:990`), intervalos (`:1050/:1052/:1062`), ata (`:1077/:1084`), grupos (`:1103/:1122/:1127`), sem origem (`:1140/:1153/:1158`) e pendentes (`:1179`). `_criar_pergunta_se_couber` chama `abrir` em `:834`, sem captura; complemento percorre marcos em `:870` e chama o helper em `:878`, fora do `try` que termina no `except Exception :865`. Uma exceção propagada de `abrir` desfaz todo o agrupamento daquele colaborador e abandona seus demais laços. (`app/chamados/services/disputa_emissao.py:834`, `:865`, `:878`, `:964`)

| Borda → agrupador | Captura / resto do lote / trilha |
|---|---|
| Cron `app/config/crons.py:363` → `processar_alertas_avancados.handle`, `app/ponto/management/commands/processar_alertas_avancados.py:19` → laço colabs `:29` → agrupador `:158` | `except Exception :159`; imprime `fio disputa falhou` em stdout `:160`; segue o laço. A criação do chamado ocorreu antes (`:121`) e permanece fora do bloco interno do agrupador. O contador `processados :164` ainda aumenta; não é contador da recusa. |
| Cron `app/config/crons.py:362` → `processar_alertas_turno.handle`, `app/ponto/management/commands/processar_alertas_turno.py:32` → laço turnos `:81` → `_marco_critico :141` → `_abrir_chamado_critico :316` → agrupador `:384` | `except Exception :385`; imprime erro stdout `:386`; retorna chamado em `:387`; `_marco_critico` ainda pode enviar alerta e abrir justificativa (`:319/:321`), e o laço continua. Chamado foi aberto antes em `:335`, fora do agrupador. Não há contador específico de recusa. |
| Cron `app/config/crons.py:364` → `detectar_ausencias.handle`, `app/ponto/management/commands/detectar_ausencias.py:61` → laços `:101/:138` → `_abrir_chamado :439` → agrupador `:801` | `except Exception :802`; imprime erro stderr `:803`; volta ao chamador, que ainda aumenta `abertos :448` e segue. Chamado aberto antes em `:693`, fora do agrupador. Contadores em `:455` não são contador dessa recusa. |
| `app/chamados/urls.py:21` ou `:39` → `fio_colaborador :674` ou `modal_fio_colaborador :954`, em `app/chamados/views.py` → `processar_post_fio`, `app/chamados/services/fio.py:551` → `acao_iniciar_disputa_orfaos`, `app/chamados/services/acoes_disputa.py:221` → agrupador | Sem captura aplicável na cadeia. O log específico de sucesso em `app/chamados/services/acoes_disputa.py:226` não é alcançado em caso de exceção. Não há transação de lote declarada nessas cascas (`app/chamados/views.py:658`, `:868`; `app/chamados/services/acoes_disputa.py:210`). Tratamento externo: **[não verificado]**. |
| `perguntar_chamados_sem_pergunta.handle`, `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:19` → `:29` → `pergunta_no_app.perguntar`, `app/chamados/services/pergunta_no_app.py:175` → laço colabs `:184` → agrupador `:186` | Sem captura/`atomic` externo de lote. Exceção abandonaria colaboradores seguintes; blocos anteriores do agrupador já terminaram. `n += len(chs)` em `:187` não ocorre para o colaborador recusado; o retorno/resumo `:188` e a impressão de `APLICADO` não são alcançados. |
| `app/chamados/urls.py:32` → `app/chamados/views_cobrar.py:325` → `app/chamados/services/cobrar_dia.py:39` (cartório) e depois agrupador `:47` | Sem captura aplicável; `except ValueError` em `app/chamados/views_cobrar.py:322` só envolve parse anterior da data. A trilha de toque `cobrar_dia_manual` é gravada antes, em `app/chamados/services/cobrar_dia.py:37`; não é trilha da recusa. |

**Restrição decisiva: chamada `:834` não recebe o cenário BATIDA descrito pelos chamadores atuais**

Os motivos enviados ao helper `:834` são:

- Intervalos, definidos em `app/chamados/services/disputa_emissao.py:1055`.
- Complementos derivados de `_MOTIVO_POR_MARCO` em `app/chamados/services/disputa_emissao.py:873`, cuja tabela só contém `saida_sem_entrada`, `orfao_14h`, `intervalo_saida`, `intervalo_volta` em `app/chamados/services/declaracao_texto.py:42`.
- Grupos/sem origem derivados de `_classificar_falta`, que delega em `app/chamados/services/disputa_emissao.py:802`; os retornos cobradores são `saida_sem_entrada`/`orfao_14h` em `app/escala/servico_jornada.py:445`, `:447`, `:458`, `:463`, `:465`, `:470`, `:471`.

Esses quatro motivos são MARCO (`app/chamados/catalogo/motivos.py:34`, `:35`, `:48`, `:49`); `batida_origem_do_nascimento` retorna `None` fora da família BATIDA (`app/chamados/models.py:3365`). Para MARCO, a consulta usa a data exata (`app/chamados/models.py:2686`, `:2713`), e o dia do fato sem episódio vem da própria data do marco (`app/chamados/services/fio.py:1334`). Portanto **não se deve apresentar `:834`, fábrica de célula, declarações de MARCO ou agrupador como execução comprovada de `ReusoSemRechave` do episódio BATIDA D0→DIA**. A tabela de agrupador acima documenta limites de propagação do código, não comprova essa alcançabilidade.

**Contador não é trilha de tentativas**

A recusa de reuso é `raise`, em `app/chamados/models.py:2867`; o contador `fio.reuso_sem_rechave` varre estado persistido (`app/chamados/services/fio.py:1046`), confirma candidatas chamando a porta real em dry (`:1059`) e retorna `n/pks/universo` (`:1061`). Não incrementa um contador persistido a cada exceção nem cria registro de recusa nesse ponto. Seu universo exclui linhas sem episódio e conversas (`app/chamados/services/fio.py:1040`, `:1046`).

## 08/11 15:11 · sessao 90837d85b73f · - · - · origem=Ronald (tmux)

Caminhos conferidos, todos relativos a `app/`.

1. **Sinal do chamado que pode iniciar emissão**

`ChamadoColaborador.save()` → receiver `chamados/signals.py:188` → `cartorio.julgar_celula` em `chamados/signals.py:203` → `julgar_colab` em `ponto/services/cartorio.py:1989` → laço das células em `ponto/services/cartorio.py:995` → `perguntar_pela_celula` em `ponto/services/cartorio.py:997` → `perguntar` em `chamados/services/fabrica_da_celula.py:186` → `criar_ou_atualizar_disputa` em `chamados/services/fabrica_da_celula.py:149` → geração em `chamados/services/disputa_emissao.py:670` ou `:682` → `PerguntaDisputa.abrir` em `chamados/services/disputa_emissao.py:444`.

A captura é o `engolir` que envolve **todo julgamento** em `chamados/signals.py:195`: `except Exception as exc` em `core/observ.py:90`. Esse contexto não abre `atomic`; registra o engolido em `core/observ.py:91`. O registro incrementa contador total e por operação, em memória do processo, e escreve log ERROR com causa/traceback: `core/observ.py:75`, `:77`, `:79`.

O `atomic` da abertura está em `chamados/models.py:2770`; a recusa levantada em `chamados/models.py:2867` sai dele antes da captura do sinal. Interrompe o restante da geração/julgamento atual — laços em `chamados/services/disputa_emissao.py:312`, `chamados/services/fabrica_da_celula.py:145` e `ponto/services/cartorio.py:995` — e retorna ao escritor que disparou o sinal. O sinal não propaga essa recusa para desfazer a transação externa; o `save` original pode terminar e seu chamador continuar, pois o `except` está em `core/observ.py:90`. Não há rollback global do julgamento no receiver: `chamados/signals.py:194`.

2. **Reconciliador: a recusa pode nascer na emissão indireta, mas é capturada no sinal anterior**

Entrada por batida: `ponto/signals.py:207` → `reconciliar_chamados_colab` em `ponto/signals.py:215`. Entrada por filho que morre: `chamados/signals.py:337` → callback em `chamados/signals.py:350` → `reconciliar_chamados_colab` em `chamados/signals.py:355`, registrado por `on_commit` em `chamados/signals.py:356`.

O reconciliador passa por perguntas absolvidas (`chamados/reconciliador.py:1428`), órfãs (`:1429`) e premissas mortas (`:1437`).

* Órfã: laço `chamados/reconciliador.py:1520` → `reatar_pergunta_orfa` em `:1522` → reemissão em `:1617` → `ChamadoColaborador.abrir` em `:910` → sinal `chamados/signals.py:188` → cadeia do item 1.
* Premissa morta: laço `chamados/reconciliador.py:1452` → `_retratar` em `:1460` → `atomic` em `:522` → `ch.reconciliar` em `:523` → `_gravar_estado`, cujo `atomic` está em `chamados/models.py:280` e `save` em `:296` → mesmo sinal e cadeia do item 1.

**Os `except Exception` externos do reconciliador em `chamados/reconciliador.py:1524` e `:1462` não recebem `ReusoSemRechave` por essas cadeias: ela já foi engolida pelo sinal em `core/observ.py:90`.** Assim, por essa recusa, continuam o laço de outras órfãs (`chamados/reconciliador.py:1520`) e o de outros chamados (`:1452`); os `atomic` externos `_retratar`/estado não são desfeitos por ela (`chamados/reconciliador.py:522`; `chamados/models.py:280`).

Na geração há outro `except Exception`, em `chamados/services/disputa_emissao.py:466`, que protege apenas o **reatamento posterior ao nascimento** (`atomic` em `:463`, chamada em `:464`). Ele não envolve o `abrir` em `:444`; portanto, não captura a recusa levantada nessa abertura. O laço só adicionaria o resultado depois, em `:470`.

O command atual `reconciliar_chamados` é vigia: importa consultas em `chamados/management/commands/reconciliar_chamados.py:49`, percorre consultas em `:71`/`:91`/`:97` e imprime contagens em `:102`; não chama o escritor `reconciliar_chamados_colab` nem `PerguntaDisputa.abrir` nesses caminhos.

3. **Outros sinais do cartório**

`ponto/signals.py:132`/`:139` (save/delete Batida), `:144`/`:151` (save/delete Ausencia), `:162` (save Justificativa) → `_julgar_dias`, em `ponto/signals.py:85` → laço de dias em `:92` → `cartorio.julgar_celula` em `:93` → cadeia do item 1.

Aqui `engolir` envolve **todo laço de dias** (`ponto/signals.py:88`): a primeira recusa encerra os demais dias desse receiver; é capturada como `Exception` em `core/observ.py:90`, com log/contador em `:75`/`:79`. A escrita original não é desfeita por propagação dessa recusa. Os outros receivers seguem; por exemplo, o reconciliador da batida é outro receiver em `ponto/signals.py:207`.

4. **Bordas humanas: censo dos dois outros callsites diretos**

* URL `chamados/urls.py:42` → view `chamados/views.py:1041` → `declarar_hora_colab` em `chamados/views.py:1090` → `atomic` em `chamados/services/declaracao_texto.py:324` → `PerguntaDisputa.abrir` em `:348`.
* URL `chamados/urls.py:14` → view `chamados/views.py:359`, ação em `:478` → `aplicar_declaracao` em `:480` → `atomic` em `chamados/services/declaracao_texto.py:626` → `PerguntaDisputa.abrir` em `:636`.

Não há `except` envolvendo essas chamadas nos corpos apresentados; uma exceção levantada atravessa e desfaz esses `atomic`. O trabalho posterior não executa: resposta em `chamados/services/declaracao_texto.py:657`/`:663`, validação em `:666`, ou push/auditoria da declaração em `chamados/views.py:1094`/`:1098`.

**Ressalva de alcançabilidade:** esses dois callsites humanos escolhem exclusivamente os quatro motivos MARCO (`chamados/services/declaracao_texto.py:42`, `:322`, `:601`), e não fornecem `batida_origem` (`:348`, `:636`); a derivação de episódio retorna `None` para MARCO (`chamados/models.py:3364`). O código procura MARCO pelo dia (`chamados/models.py:2686`, `:2713`), e seu dia do fato vem da própria data gravada (`chamados/services/fio.py:1334`). Portanto, são callsites de `abrir`, mas ocorrência neles do cenário **BATIDA D0/DIA** descrito pelo usuário não foi demonstrada: [não verificado].

5. **Capturas dos demais receivers de chamados**

Os `except Exception` em `chamados/signals.py:46`, `:116`, `:141` e `:164` protegem respectivamente fechamento do pai (`:42`), resolução (`:115`), retomada de regularizações (`:137`) e materialização (`:163`). Não são capturas diretas de `PerguntaDisputa.abrir`; eventual emissão pelo save de chamado/batida volta aos sinais do cartório já descritos. Seus próprios logs estão em `chamados/signals.py:47`, `:117`, `:142`, `:165`.

A ocorrência real de qualquer desses caminhos em produção e o conteúdo efetivo de seus logs/contadores não estão no anexo fornecido: [não verificado].

## 08/11 15:10 · sessao 50d338b99a3b · - · - · origem=Ronald (tmux)

Censo estático concluído sem banco, logs ou alteração de arquivos.

- A única atribuição encontrada em produção é `chamados/services/fio.py:800`, dentro de `gravar_dia_do_fato`; os caminhos rastreados são nascimento (`chamados/models.py:2922`), costura de linha sem dia (`chamados/services/fio.py:863`) e rechaveamento (`chamados/services/fio.py:989`).
- O reuso passa por esse rechaveamento (`chamados/models.py:2864`, `chamados/services/fio.py:1026`); migração e reversão também (`chamados/management/commands/migrar_data_evento_dia_do_turno.py:384`, `chamados/management/commands/migrar_data_evento_dia_do_turno.py:457`).
- A conversa nasce pelo mesmo escritor, recebendo o dia civil (`chamados/models.py:2757`, `chamados/models.py:2922`).
- A catraca exclui testes e migrações e declara limitações para formas dinâmicas; portanto não equivale a uma garantia contra qualquer escrita arbitrária (`chamados/tests/test_selo_p13_data_evento_porta_unica.py:147`, `chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:15`).
- O censo completo de leitores, comandos, templates e migrações foi enviado ao agente principal. Fora de `app/`, há leitura do payload em `mensageria/nucleo/ferramentas.py:127`; escrita dinâmica por nome indireto permanece [não verificado].

## 08/11 12:12 · sessao b0a85d04359f · FIO-E0-V4-E1 · verificador · origem=Code

**REPROVADA a garantia universal da 15b; PROCEDENTE a cura dos contraexemplos nomeados nas portas alteradas.** Permanecem o retorno da chave com A2 vazio, a emissão de conversa sobre retratada e a admissão de retratada no fecho. (`app/chamados/services/fio.py:1310`, `app/chamados/services/disputa_emissao.py:431`, `app/chamados/services/fio.py:1467`)

Os contraexemplos abaixo são construções pelo código. Ocorrência deles, números reais e migração integral em produção: **[nao verificado]**.

**1. Itens 3 e 5 anteriores: cura PROCEDENTE nos casos específicos; universalidade NÃO PROCEDENTE.**

| Contraexemplo | Trajeto na 15b |
|---|---|
| **3a, episódio retratado** | `_nascer` deriva a origem e consulta o helper; este chama a recusa, que transforma o dia nulo em `BatidaRetratadaNaoEFato`. Não alcança a busca nem o `create`. Repetindo sem mudança dos insumos, repete a recusa. (`app/chamados/models.py:2760`, `app/chamados/models.py:2769`; `app/chamados/services/fio.py:1301`, `app/chamados/services/fio.py:1268`; escrita em `app/chamados/models.py:2867`) |
| **3b, K sem episódio, respondida, gravada D0, admitida em DIA por A2** | O episódio vivo do pedido fornece DIA; o universo alcança K, a eleição prefere sua resposta e a costura recebe explicitamente `_dia_fato=DIA`. Não relê D0 como identidade nessa costura. (`app/chamados/models.py:2769`, `app/chamados/models.py:2800`, `app/chamados/models.py:2814`, `app/chamados/models.py:2829`; `app/chamados/services/fio.py:1005`; `app/chamados/juizes.py:2006`) |
| **Variante R, canônica respondida de episódio retratado** | O fecho ainda pode eleger R, mas `_nascer` chama `recusa_batida_retratada(ex)` antes de alterar célula ou destino, inclusive sem destino pedido. Agora recusa pelo motivo correto. (`app/chamados/models.py:2814`, `app/chamados/models.py:2819`, `app/chamados/models.py:2822`) |
| **3a/3b originais, episódio vivo** | Dia, célula, busca e nascimento usam `_dia_fato`; no reuso, a costura recebe esse mesmo dia. A data gravada antiga é preservada. (`app/chamados/models.py:2769`, `app/chamados/models.py:2789`, `app/chamados/models.py:2800`, `app/chamados/models.py:2829`, `app/chamados/models.py:2867`; `app/chamados/tests/test_e1_v15.py:98`) |

A resolvedora chamada diretamente também passou a consultar A2 para K e a recusar R. (`app/chamados/juizes.py:2008`; `app/chamados/services/fio.py:1301`, `app/chamados/services/fio.py:1309`)

**2. Retorno do pedido/gravado: PROCEDENTE para acervo com A2 vazio.**

Para linha de **fato com episódio**, não há mais esse fallback: recusa quando o juiz não responde; caso contrário, retorna `_dia_do_fato`. Conversa retorna antes desse ramo, portanto continua dispensada mesmo se receber origem explícita. (`app/chamados/services/fio.py:1298`, `app/chamados/services/fio.py:1300`)

Para acervo sem episódio e sem marco, permanece literalmente:

```python
alc = _dias_alcancados_sem_dia(...)
if not alc:
    return d
```

`d` é pedido, quando fornecido, senão gravada. Logo, “nenhum caminho devolve pedido/gravado para acervo” é **NÃO PROCEDENTE**. (`app/chamados/services/fio.py:1297`, `app/chamados/services/fio.py:1309`)

**A admissão do vazio é compartilhada; a atribuição de um dia não é a mesma operação.** Universo e fecho aceitam alcance vazio porque `set() <= {dia}`. Isso prova compatibilidade com um dia fornecido, não identifica um dia único. Sem dia fornecido e sem alcance, o fecho conserva `_dia=None`. (`app/chamados/services/fio.py:1467`, `app/chamados/services/fio.py:1425`, `app/chamados/services/fio.py:1413`)

Contraexemplo: K isolada, sem episódio/marco, gravada D0. O helper retorna D0; com `pedido=DIA`, retorna DIA. O fecho de K sem dia não infere nenhum; com `dia=DIA`, conserva K. Portanto o helper escolhe a chave onde A2 não respondeu um dia. Esse percurso foi reproduzido em memória com as funções reais e vizinhança vazia; persistência Django: **[nao verificado]**. (`app/chamados/services/fio.py:1297`, `app/chamados/services/fio.py:1311`, `app/chamados/services/fio.py:1414`, `app/chamados/services/fio.py:1433`)

**3. Censo de chamadores e efeito da recusa.**

Censo das chamadas explícitas de produção, incluindo os apelidos encontrados; chamadas dinâmicas arbitrárias: **[nao verificado]**.

| Função | Chamador e efeito |
|---|---|
| `resolvedora_valida` | **Plano de fechamento:** uma pergunta retratada que precise reatar deixa de ter destino admissível. (`app/chamados/models.py:392`) |
| `resolvedora_valida` | **Nascimento, proxy inicial:** usa o dia já calculado; a origem retratada já foi recusada pelo helper. (`app/chamados/models.py:2769`, `app/chamados/models.py:2781`) |
| `resolvedora_valida` | **Reuso:** usa o dia explícito do fato, após recusa da canônica retratada. (`app/chamados/models.py:2819`, `app/chamados/models.py:2829`) |
| `resolvedora_valida` | **Reapontamento:** retratada com destino não nulo recusa; descostura sem datação continua permitida pelo retorno antecipado para `chamado=None`. (`app/chamados/services/fio.py:865`; `app/chamados/juizes.py:1999`) |
| `resolvedora_valida` | **Rechaveamento:** recusa retratada com destino; sem destino, retorna antes da recusa e pode persistir o dia recebido, passando as demais guardas. (`app/chamados/services/fio.py:970`, `app/chamados/services/fio.py:974`; `app/chamados/juizes.py:1999`) |
| `resolvedora_valida` | **Auditoria I8b:** retratada viva com destino/data passa pela classificação de resolvedora inválida. (`app/chamados/management/commands/auditar_invariantes_chamados.py:891`, `app/chamados/management/commands/auditar_invariantes_chamados.py:896`) |
| `resolvedora_valida` | **Auditoria I22:** chamado vivo cujas resolvedoras de fato sejam todas recusadas entra na violação; conversas são excluídas. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1045`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1048`) |
| `dia_do_turno_da_linha` | **Resolvedora**, **nascimento** e **datação no reapontamento**. (`app/chamados/juizes.py:2008`; `app/chamados/models.py:2769`; `app/chamados/services/fio.py:846`) |
| `recusa_batida_retratada` | **Resolvedora com dia explícito**, **reuso da canônica**, **ramo de episódio do helper** e **confirmação do contador**. (`app/chamados/juizes.py:2005`; `app/chamados/models.py:2819`; `app/chamados/services/fio.py:1301`, `app/chamados/services/fio.py:1544`) |

**Fechamento recusado: PROCEDENTE, condicionalmente.** O caminho exato é:

1. `_ancoradas` reúne não validadas pelo destino ou pelo chamado da disputa. `_vivas_entre` conserva as de competência aberta que continuam vivas pelo C6 no pós-estado. (`app/chamados/models.py:344`, `app/chamados/models.py:364`, `app/chamados/models.py:368`)
2. O plano procura sobrevivente usando **data gravada**; havendo sobrevivente, consulta a resolvedora. (`app/chamados/models.py:390`, `app/chamados/models.py:392`)
3. Resolvedora → helper → recusa retratada → `(False, texto)`. O plano devolve `reatas=None`. (`app/chamados/juizes.py:2008`, `app/chamados/juizes.py:2009`; `app/chamados/services/fio.py:1301`; `app/chamados/models.py:393`)
4. O gate devolve `False`; reconciliação/cancelamento abortam antes de gravar o fechamento. (`app/chamados/models.py:422`, `app/chamados/models.py:431`, `app/chamados/models.py:685`, `app/chamados/models.py:786`)

Isso pode impedir um fechamento que antes conseguiria reatar a pergunta. Não vale para toda retratada: ela precisa sobreviver aos filtros anteriores. Sem viva, o gate permite. (`app/chamados/models.py:364`, `app/chamados/models.py:368`, `app/chamados/models.py:420`)

O motivo específico é perdido nesse plano: o registro usa `pergunta_viva_na_competencia_aberta` e afirma ausência de cobrança receptora, inclusive quando a cobrança existe e a pergunta foi recusada. (`app/chamados/models.py:392`, `app/chamados/models.py:425`)

**4. Caminho que ainda gera conversa sobre retratada: PROCEDENTE.**

Contraexemplo: `ultima_batida.id=br` retratada, motivo `gps_ausente`, sem data no contexto do motivo e sem conversa anterior. O motivo é BATIDA e sua resposta é `escolha`. (`app/chamados/catalogo/motivos.py:42`; `app/chamados/catalogo/perguntas.py:88`)

O derivador aceita a origem pelo dono, sem filtrar retratação; assim, não entra no dedup inicial reservado à ausência de episódio. O juiz retorna `None`, `_dia_p` fica nulo e somente perguntas de `hora` são descartadas. (`app/chamados/models.py:3316`; `app/chamados/services/disputa_emissao.py:325`, `app/chamados/services/disputa_emissao.py:391`, `app/chamados/services/disputa_emissao.py:424`)

O emissor então chama **`abrir_conversa`**. A conversa não deriva origem, o helper retorna o dia civil antes da recusa e a resolvedora também aprova conversa antecipadamente. É o bug no chamador abrangido pelo aval fornecido. (`app/chamados/services/disputa_emissao.py:431`, `app/chamados/services/disputa_emissao.py:439`; `app/chamados/models.py:2756`, `app/chamados/models.py:3297`; `app/chamados/services/fio.py:1298`; `app/chamados/juizes.py:1997`)

Também há reabertura sem a recusa nova: pergunta respondida sobre retratada, sem materialização, pode entrar em `perguntas_corrigiveis`, alcançar `reabrir_por_colab` e voltar a pendente pelo `update` de `reabrir_para_correcao`. Se “reatar pergunta” abrange voltar a pedi-la, é outro escape. (`app/chamados/juizes.py:1412`, `app/chamados/juizes.py:1245`; `app/chamados/models.py:2226`, `app/chamados/models.py:2235`; `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/services/disputa_emissao.py:1533`)

**5. Ramo antigo `FatoEmOutroDia`: inalcançável com identidade íntegra; não há prova universal de inacessibilidade.**

Com episódio vivo, o helper obtém D e o memo por episódio fornece D às linhas desse episódio. Havendo linha da mesma base, ela semeia `datadas`; o universo não fica vazio. Episódio retratado recusa antes. Portanto o comentário de que o ramo “sobra só para retratada” ficou obsoleto. (`app/chamados/services/fio.py:1243`, `app/chamados/services/fio.py:1465`, `app/chamados/services/fio.py:1481`, `app/chamados/services/fio.py:1301`; `app/chamados/models.py:2807`)

Há, porém, este **contraexemplo de acervo inconsistente**:

- P pertence à disputa de C2 e tem origem explícita b, batida viva de C1. O `save` confere colaborador contra disputa, mas só deriva a origem quando ausente; a checagem do dono existe no derivador. (`app/chamados/models.py:3282`, `app/chamados/models.py:3297`, `app/chamados/models.py:3316`)
- `abrir(C1,b)` obtém D. `do_fato` encontra P pelo episódio/motivo sem filtrar colaborador. (`app/chamados/models.py:2769`, `app/chamados/models.py:2647`)
- O universo recusa P pela **base C2**, fica vazio e entrega P nas recusadas. O ramo antigo encontra o mesmo episódio e levanta `FatoEmOutroDia`, embora a divergência seja de dono. (`app/chamados/services/fio.py:1463`, `app/chamados/services/fio.py:1481`, `app/chamados/services/fio.py:1486`; `app/chamados/models.py:2802`, `app/chamados/models.py:2810`)

Esse acervo e um chamador atual de produção que o persista: **[nao verificado]**. A porta `abrir` 15b de C2 já recusaria a origem incompatível antes desse nascimento. (`app/chamados/services/fio.py:1248`, `app/chamados/services/fio.py:1301`)

**6. Retratada admitida no fecho por A2: PROCEDENTE.**

`_dia_do_fato` retorna nulo para a retratada; universo e fecho tratam esse nulo pelo mesmo ramo de A2, sem chamar a recusa. A premissa do teste novo confirma que R pode ser a canônica do universo de DIA. (`app/chamados/services/fio.py:1248`, `app/chamados/services/fio.py:1467`, `app/chamados/services/fio.py:1425`; `app/chamados/tests/test_e1_v15b.py:132`)

Consequências concretas:

- **Eleição:** R respondida vence A viva; `_nascer` recusa R depois da eleição, sem tentar outra canônica. Uma solicitação legítima sobre A pode, portanto, ser barrada por R. (`app/chamados/services/fio.py:1005`; `app/chamados/models.py:2814`, `app/chamados/models.py:2819`)
- **Dedup/backfill:** continuam elegendo sobre esse universo. O plano conta R como episódio; A+R pode produzir `episodios_unidos`, impedindo retratação. (`app/chamados/management/commands/dedup_perguntas_fato.py:76`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`; `app/chamados/services/fio.py:1060`, `app/chamados/services/fio.py:1065`)
- **Migração de resolvedoras:** também elege R; quando tenta reapontá-la, pode receber a recusa nova. A eleição em si não foi saneada. (`app/chamados/services/fio.py:1753`, `app/chamados/services/fio.py:1772`)
- **Migração de data:** R pode integrar o universo do grupo e seu dia gravado participar do veto de competência. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:254`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:277`)

**7. Contador: PROCEDENTE no universo declarado; não conta todas as perguntas/conversas sobre retratada.**

Conta linhas persistidas com `conversa=False`, origem preenchida, fora de `VIAS_FORA_DO_FATO`, cuja origem esteja fora de `batidas_apuraveis(None)`. Não filtra viva, resposta, validação, destino ou horizonte. (`app/chamados/services/fio.py:1537`)

A autoridade consultada inclui todos os colaboradores e define apuráveis por `retratada_em IS NULL`. Cada candidata é confirmada pela **função real** da recusa, acumulando somente `BatidaRetratadaNaoEFato`. (`app/ponto/turnos.py:2865`, `app/ponto/turnos.py:2871`; `app/chamados/services/fio.py:1544`)

O leitor imprime número, esperado, motivo e pks no backfill, antes da distinção entre DRY e apply. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:534`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:546`)

Não conta tentativas recusadas sem INSERT, conversas do escape do item 4 ou linhas já fora do fato. Portanto não serve como prova de ausência universal do bug nem como contador de tentativas recusadas. (`app/chamados/services/fio.py:1537`, `app/chamados/services/fio.py:1538`; `app/chamados/services/disputa_emissao.py:439`)

**8. Catraca: PROCEDENTE a mordida oficial; garantia semântica completa NÃO PROCEDENTE.**

Os 11 métodos AST de P13/V15 passaram na execução em memória, substituindo `SimpleTestCase` por `unittest.TestCase`, sem executar Django/BD. Isso inclui os 13 mutantes oficiais e o mutante adicional do parâmetro do fecho. Suíte Django/BD: **[nao verificado]**. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:141`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:235`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:263`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:304`)

Os escapes demonstrados em memória incluem:

| Escape | Por que passa |
|---|---|
| `alc = set()` | Fiscaliza formato dos retornos, não a procedência de `alc`; passa a devolver sempre a chave no acervo. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:148`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:157`) |
| Recusa chamada com proxy `conversa=True` | Verifica nome/posição, não argumentos; esse proxy é dispensado pela recusa. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:164`; `app/chamados/services/fio.py:1266`) |
| Guarda inicial da resolvedora trocada por `if True` | A fiscalização das atribuições não prova que o fluxo chega nelas. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:121`; retorno em `app/chamados/juizes.py:1997`) |
| `_dia += timedelta(days=1)` | A ligação ao juiz só acompanha `Assign`, ignorando `AugAssign`. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:71`) |
| `p.data_evento: object = data_evento` dentro da porta | V15 ignora `AnnAssign`; P13 detecta localização e aceita a porta. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:90`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:84`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:119`) |
| `update(**kw)` / `bulk_update(ps, campos)` com variáveis | P13 não acompanha os valores dessas variáveis. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:105`) |

Rechaveamento continua fiscalizado pelo **arquivo chamador**, sem provar o valor recebido ou escrito. A aceitação explícita de `return d` com A2 vazio também mantém verde o problema do item 2. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:217`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:260`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:155`)

**9. Escritores, leitores e significados.**

| Campo/valor | Quem escreve/fornece | Quem lê |
|---|---|---|
| `data_evento` | Nascimento `_dia_fato`; datação `_dia`; rechaveamento `novo_dia`. Apply obtém juiz; reversão obtém arquivo. (`app/chamados/models.py:2867`; `app/chamados/services/fio.py:849`, `app/chamados/services/fio.py:968`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:412`) | Busca/índice pela gravada; marco pela gravada; C6, tranca e plantio continuam lendo gravada. Costura usa helper ou parâmetro explícito. (`app/chamados/models.py:2678`, `app/chamados/models.py:3332`; `app/chamados/services/fio.py:1250`; `app/chamados/juizes.py:336`, `app/chamados/juizes.py:948`, `app/chamados/juizes.py:2006`; `app/chamados/services/materializacao.py:345`) |
| `celula` | Nascimento/reuso escolhem por `_dia_fato`; rechaveamento por `novo_dia`. (`app/chamados/models.py:2789`, `app/chamados/models.py:2823`, `app/chamados/models.py:2868`; `app/chamados/services/fio.py:965`, `app/chamados/services/fio.py:969`) | C6 exige compatibilidade entre dia da célula e `data_evento`; guarda terminal consulta o pós-estado. (`app/chamados/juizes.py:638`; `app/chamados/services/fio.py:779`) |
| `chamado_destino` | Nascimento, reuso após guardas, reapontamento. (`app/chamados/models.py:2868`, `app/chamados/models.py:2839`; `app/chamados/services/fio.py:877`) | Âncora do fio, fechamento, guarda terminal e plantio. (`app/chamados/services/fio.py:117`; `app/chamados/models.py:345`; `app/chamados/services/fio.py:770`; `app/chamados/services/materializacao.py:339`) |
| `batida_origem_id` | Explícita ou derivada antes do helper; inserção deriva quando ausente. (`app/chamados/models.py:2760`, `app/chamados/models.py:3297`) | Busca do episódio, juiz, recusa, contador, chave da família e veto de episódios unidos. (`app/chamados/models.py:2647`; `app/chamados/services/fio.py:1242`, `app/chamados/services/fio.py:1266`, `app/chamados/services/fio.py:1537`, `app/chamados/services/fio.py:1131`, `app/chamados/services/fio.py:1060`) |
| `dia_da_chave` | Helper fornece pedido/gravada. (`app/chamados/services/fio.py:1297`, `app/chamados/services/fio.py:1309`) | Modifica a consulta inicial de vizinhança e sua chave de cache; passos seguintes usam as chaves dos vizinhos. (`app/chamados/services/fio.py:1336`, `app/chamados/services/fio.py:1358`, `app/chamados/services/fio.py:1367`, `app/chamados/services/fio.py:1204`) |
| Parâmetro `dia_do_fato` | `_nascer` fornece `_dia_fato` ao proxy e à canônica. (`app/chamados/models.py:2781`, `app/chamados/models.py:2829`) | Resolvedora faz a recusa e usa o parâmetro diretamente. (`app/chamados/juizes.py:2004`) |

**Divergência de significado residual: PROCEDENTE.** Reuso pode atribuir célula/destino de DIA e conservar `data_evento=D0`; a costura aceita DIA, C6 descarta a célula divergente e tranca calcula competência por D0. O teste preserva expressamente essa divergência. (`app/chamados/models.py:2823`, `app/chamados/models.py:2839`; `app/chamados/tests/test_e1_v15.py:98`; `app/chamados/juizes.py:2006`, `app/chamados/juizes.py:638`, `app/chamados/juizes.py:948`)

O uso civil da conversa tem discriminador explícito. A premissa de que todo fato já teria “gravada == juiz” não está demonstrada pela cópia. (`app/chamados/models.py:2717`, `app/chamados/models.py:2722`; `app/chamados/tests/test_e1_v15.py:99`)

**10. Classes de reprova.**

| Classe | Veredito |
|---|---|
| **S

[truncado em 20000 de 22550 caracteres]

## 08/11 12:09 · sessao 2c277a08ce1c · - · - · origem=Ronald (tmux)

**7. Contador — PROCEDENTE no universo declarado, insuficiente como contador de recusas/tentativas.**

- Conta linhas persistidas `conversa=False`, com `batida_origem`, fora de `VIAS_FORA_DO_FATO`, cujo episódio não está em `batidas_apuraveis(None)`. Não exige pergunta viva, não validada, horizonte, destino ou ausência de resposta. (`app/chamados/services/fio.py:1537`; `app/chamados/services/fio.py:1540`)
- `VIAS_FORA_DO_FATO` contém somente `duplicata` e `motivo_retratado`. (`app/chamados/models.py:2318`)
- `batidas_apuraveis(None)` inclui todos os colaboradores e filtra `retratada_em IS NULL`; portanto o complemento seleciona episódios retratados, sob integridade normal da FK. (`app/ponto/turnos.py:2865`; `app/ponto/turnos.py:2871`)
- Cada candidata é confirmada pela função real `recusa_batida_retratada`; somente a exceção `BatidaRetratadaNaoEFato` acumula o pk. Retorna n, esperado=0, motivo e pks. (`app/chamados/services/fio.py:1542`; `app/chamados/services/fio.py:1548`)
- O leitor de produção imprime `PERGUNTA_SOBRE_RETRATADA dry`, antes de distinguir `--aplicar`; logo também aparece no apply. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:534`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:546`)
- Não conta tentativas recusadas sem INSERT, nem conversas sobre batida retratada, nem linhas já marcadas duplicata/motivo_retratado. Esse alcance resulta dos filtros, independentemente do título do contador. (`app/chamados/services/fio.py:1537`; `app/chamados/services/fio.py:1546`)
- N real de produção: **[nao verificado]**.

**8. Catraca — PROCEDENTE a mordida dos mutantes oficiais; NÃO PROCEDENTE como garantia semântica universal.**

Executei exclusivamente os ASTs em memória, com `python3 -B`, substituindo `SimpleTestCase` por `unittest.TestCase` e removendo os imports Django. Os 11 métodos passaram: três P13 e oito V15, incluindo os 13 mutantes de `test_a_catraca_MORDE` e o mutante adicional de `dia_do_fato`. Isso verifica a lógica AST desses métodos, sem BD ou execução dos caminhos Django. Os métodos/mutantes estão em `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:141`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:235`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:263`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:304`. Suíte Django/BD: **[nao verificado]**.

Censo real encontrado pelos ASTs:

| Local | Linha |
|---|---|
| `_nascer`, proxy | `app/chamados/models.py:2777` |
| `_nascer`, INSERT | `app/chamados/models.py:2867` |
| `reapontar_resolvedora`, valor | `app/chamados/services/fio.py:849` |
| `reapontar_resolvedora`, desfazimentos em memória | `app/chamados/services/fio.py:861`, `app/chamados/services/fio.py:870` |
| `rechavear_data_evento`, valor/save | `app/chamados/services/fio.py:968`, `app/chamados/services/fio.py:974` |

Os novos mutantes detectam regressões literais em setattr/bulk, retirada da recusa, fallback `or d`, mudança da guarda de A2 e leitura da gravada no parâmetro do fecho. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:275`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:295`)

Escapes demonstrados em memória, todos com detector retornando `[]`:

1. Trocar a origem de `alc` por `alc = set()`. O helper passa a devolver sempre a chave no ramo acervo; a catraca verifica formas dos returns/guardas, não a origem ou conteúdo de `alc`. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:148`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:176`; escrita real de alc em `app/chamados/services/fio.py:1309`)
2. Chamar `recusa_batida_retratada` com proxy `conversa=True`, em vez de p. A catraca verifica nome/posição da chamada, não argumentos; a recusa retorna imediatamente nesse proxy. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:164`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:169`; `app/chamados/services/fio.py:1266`)
3. Trocar a guarda inicial de conversa da resolvedora por `if True`, preservando as atribuições abaixo. A catraca não verifica early returns/controle de fluxo completo. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:121`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:138`; retorno real em `app/chamados/juizes.py:1997`)
4. Dentro da porta, `p.data_evento: object = data_evento`: V15 ignora `AnnAssign`; P13 detecta a escrita, mas aceita sua localização. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:89`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:95`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:84`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:119`)
5. Após `_dia = dia_do_turno_da_linha(...)`, acrescentar `_dia += datetime.timedelta(days=1)`. A ligação ao juiz só examina `Assign`, ignorando `AugAssign`; a escrita continua aceita. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:67`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:81`)
6. `kw={'data_evento':d}; qs.update(**kw)` e `campos=['data_evento']; qs.bulk_update(ps,campos)` escapam do P13; a variável não é rastreada. A primeira forma também escapa da fiscalização V15 dentro da porta. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:105`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:108`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:97`)
7. Rechaveamento só tem fiscalização de arquivo chamador nesta V15; a procedência do argumento e o valor escrito no corpo de `rechavear_data_evento` não são examinados. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:217`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:232`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:260`)

**10. Segundo juiz — NÃO PROCEDENTE no trajeto geométrico examinado.**

`recusa_batida_retratada` não replica `retratada_em` nem pareia turnos: transforma o `None` de `_dia_do_fato` em exceção. `_dia_do_fato` consulta `dia_do_turno_da_batida`; esta consulta `dia_das_batidas`, que chama `turnos_do_colab`, autoridade registrada para `_T1`. (`app/chamados/services/fio.py:1268`; `app/chamados/services/fio.py:1248`; `app/ponto/turnos.py:2925`; `app/ponto/turnos.py:2902`; `app/core/juizes.py:196`)

O contador também consulta a autoridade do universo, em vez de replicar o filtro. (`app/chamados/services/fio.py:1539`; `app/ponto/turnos.py:2846`)

Limite semântico: a recusa chama “retratada” qualquer episódio cujo juiz resulte `None`, incluindo batida inexistente ou incompatibilidade de colaborador; não verifica literalmente a retratação. `_dia_do_fato` retorna None quando não encontra a FK, e o juiz consulta o mapa do colaborador da linha. (`app/chamados/services/fio.py:1247`; `app/chamados/services/fio.py:1248`; `app/chamados/services/fio.py:1268`; `app/ponto/turnos.py:2925`) Ocorrência desses estados em produção: **[nao verificado]**.

`JUIZES.md` declara-se lookup e aponta para o registro vivo; a classificação acima usa `core/juizes.py`, não apenas o comentário do diff. (`app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:12`)

## 08/11 12:09 · sessao 07c04253531a · - · - · origem=Ronald (tmux)

**Item 5: inalcançável com identidade íntegra; alcançável com acervo de episódio atribuído a outro colaborador.** O código atual permite esse estado pelo `save`, embora `abrir` 15b já não o gere (`app/chamados/models.py:3279`, `:3285`, `:3297`; `app/chamados/services/fio.py:1300`, `:1301`).

Prova no caso íntegro: pedido com episódio vivo produz D; o memo é indexado pelo episódio, então todas as linhas desse episódio recebem D; com mesma base, entram em `datadas`, tornam as sementes não vazias e impedem a condição `not _linhas_do_fato` (`app/chamados/services/fio.py:1243`, `:1248`, `:1249`, `:1463`, `:1465`, `:1466`, `:1481`, `:1482`; `app/chamados/models.py:2802`). Episódio retratado é recusado antes de chegar ao ramo (`app/chamados/services/fio.py:1300`, `:1301`; `app/chamados/models.py:2769`).

Contraexemplo estático:

1. P persistida na disputa de C2, com `colaborador=C2` e `batida_origem=b`, sendo b uma batida viva de C1. O `save` confere colaborador contra a disputa, mas não confere dono da batida explícita; essa conferência existe apenas no derivador, que não é chamado quando o episódio já está preenchido (`app/chamados/models.py:3282`, `:3297`, `:3298`, `:3316`).
2. `abrir` para C1, episódio b, julga b para C1 e obtém D (`app/chamados/models.py:2769`, `:2772`; `app/chamados/services/fio.py:1247`, `:1248`).
3. `do_fato` busca pelo episódio e motivo, sem filtrar colaborador, e encontra P (`app/chamados/models.py:2647`, `:2648`, `:2649`).
4. `universo_do_fato` recusa P pela base C2, não pelo dia; sem outras linhas, devolve universo vazio e P em `recusadas` (`app/chamados/services/fio.py:1463`, `:1464`, `:1481`, `:1483`, `:1486`).
5. O ramo antigo testa somente universo vazio + mesmo episódio nas recusadas e levanta `FatoEmOutroDia`, atribuindo causa de dia a uma recusa de dono (`app/chamados/models.py:2802`, `:2810`, `:2811`).

A existência desse acervo em produção e um chamador de produção atual que persista essa inconsistência: **[nao verificado]**.

## 08/11 12:08 · sessao fc923f307369 · - · - · origem=Ronald (tmux)

**Censo explícito de produção** (apelidos incluídos; chamadas dinâmicas arbitrárias: [nao verificado]):

| Função | Chamadas |
|---|---|
| `resolvedora_valida` | `app/chamados/models.py:392`, `:2781`, `:2829`; `app/chamados/services/fio.py:865`, `:970`; `app/chamados/management/commands/auditar_invariantes_chamados.py:896` (`_rv8b`), `:1048` (`_rv22`). |
| `dia_do_turno_da_linha` | `app/chamados/juizes.py:2008`; `app/chamados/models.py:2769`; `app/chamados/services/fio.py:846`. |
| `recusa_batida_retratada` | `app/chamados/juizes.py:2005`; `app/chamados/models.py:2819`; `app/chamados/services/fio.py:1301`, `:1544`. |

**Mudanças por chamador:**

- Nascimento: episódio retratado recusa no helper antes da validação inicial e da escrita; canônica retratada também recusa antes do reuso, inclusive sem destino. (`app/chamados/models.py:2769`, `:2819`)
- Costuras iniciais/reuso: `dia_do_fato` só entra após `recusa_batida_retratada`; costuras externas usam helper e recebem `(False, motivo textual)`. (`app/chamados/juizes.py:2003-2010`; `app/chamados/models.py:2781`, `:2829`)
- Reapontamento: datar retratada recusa em `fio.py:846-848`; já datada com destino não nulo recusa em `fio.py:865-871`. **Descostura sem datação continua permitida**, pois `resolvedora_valida(None,p)` retorna antes da recusa. (`app/chamados/juizes.py:1999-2000`)
- Rechaveamento: com destino não nulo, retratada passa a recusar em `fio.py:970-972`; sem destino o juiz retorna verdadeiro e a porta pode persistir `novo_dia` fornecido, se demais guardas permitirem. Alcance pelo gatilho de migração depende dos filtros deste. (`app/chamados/services/fio.py:950-974`; `app/chamados/juizes.py:1999-2000`)
- Auditor I8b passa a classificar retratada viva com destino/data como inválida, depois separa os subpisos. Rótulo continua “OUTRO TURNO”. (`app/chamados/management/commands/auditar_invariantes_chamados.py:891-918`, `:935-936`)
- Auditor I22 passa a contar chamado vivo cujas resolvedoras de fato sejam todas recusadas por retratação; conversas ficam excluídas. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1041-1049`)
- Contador chama recusa real para confirmar cada candidata. (`app/chamados/services/fio.py:1542-1546`)

**Fechamento recusado: PROCEDENTE, com as condições do gate.**

1. `_ancoradas` busca não validadas pelo destino ou pelo chamado da disputa. (`app/chamados/models.py:344-347`)
2. `_vivas_entre` exige continuar ancorada, competência aberta (`is False`) e C6 viva no pós-estado do chamado. Retratação do episódio não é filtro aqui. (`app/chamados/models.py:349-369`)
3. `_plano_pergunta_viva` procura sobrevivente usando **data gravada**, não helper. Havendo sobrevivente, chama resolvedora. (`app/chamados/models.py:390-392`)
4. Resolvedora → helper → recusa retratada → `(False, 'batida_retratada: ...')`. (`app/chamados/juizes.py:2008-2010`; `app/chamados/services/fio.py:1301`, `:1268-1272`)
5. Plano devolve `reatas=None`; gate devolve `False`. O log **perde o motivo específico**: registra `pergunta_viva_na_competencia_aberta` e descreve inexistência de cobrança receptora mesmo quando ela existe, mas rejeitou a pergunta. (`app/chamados/models.py:392-393`, `:422-431`)
6. Reconciliar aborta decisão; cancelar/encerrar/encerrar por competência abortam antes da escrita de estado. (`app/chamados/models.py:685-689`, `:786-788`, `:1604-1606`, `:1624-1626`)

Logo, não é “qualquer pergunta retratada bloqueia qualquer fechamento”: a pergunta deve sobreviver ao filtro C6 e competência no pós-estado; sem viva, gate permite. (`app/chamados/models.py:364-369`, `:420-421`)

**Escape de produção confirmado: conversa sobre retratada.**

Contraexemplo estático: disputa com `contexto['ultima_batida']['id']=br` retratada, sem `contexto['gps_ausente']['data']`, emissão de `gps_ausente`, sem conversa anterior desse motivo. O motivo é BATIDA e sua pergunta é `escolha`. (`app/chamados/catalogo/motivos.py:42`; `app/chamados/catalogo/perguntas.py:88-91`)

O derivador de origem aceita `br` por colaborador, sem filtrar `retratada_em`, portanto o dedup inicial de motivo sem episódio não barra. (`app/chamados/models.py:3310-3316`; `app/chamados/services/disputa_emissao.py:325-328`) O juiz de `ultima_batida` responde `None`; `_dia_p` fica nulo; apenas tipo `hora` é descartado. (`app/chamados/services/disputa_emissao.py:387-392`, `:424-430`) Cai em `abrir_conversa`, caminho cujo próprio comentário menciona episódio retratado. (`app/chamados/services/disputa_emissao.py:431-441`) Conversa não deriva origem no nascimento nem no save; helper retorna dia civil antes da recusa. (`app/chamados/models.py:2755-2762`, `:3297-3298`; `app/chamados/services/fio.py:1297-1301`) Se destino existir, resolvedora também aprova conversa antes da recusa. (`app/chamados/juizes.py:1997-1998`) O contador exclui conversas. (`app/chamados/services/fio.py:1536`)

**Portanto PROCEDENTE o bug no chamador indicado pelo aval; ocorrência desse estado em produção: [nao verificado].** (`app/chamados/services/disputa_emissao.py:431-441`)

**Outro caminho de voltar a pedir resposta, se “reatar” abranger reabertura:** pergunta respondida sobre episódio retratado, sem `materializada_em`, pode voltar a pendente por `reabrir_por_colab`: `perguntas_corrigiveis` exige respondida e sem lastro/viva; sem carimbo satisfaz independentemente do episódio; a porta chama `reabrir_para_correcao`, que zera resposta/validação por update sem consultar a recusa nova. (`app/chamados/juizes.py:1412-1414`, `:1245`; `app/chamados/models.py:2226-2235`; `app/chamados/services/disputa_emissao.py:1494`, `:1530-1534`) Ocorrência em produção: [nao verificado].

## 08/11 12:08 · sessao 07c04253531a · - · - · origem=Ronald (tmux)

Achados dos campos e consequências:

| Campo/valor | Escritores | Leitores relevantes |
|---|---|---|
| `data_evento` | Nascimento usa `_dia_fato` (`app/chamados/models.py:2867`); datação usa `_dia` e salva (`app/chamados/services/fio.py:846`, `:849`, `:879`); rechaveamento usa `novo_dia` (`app/chamados/services/fio.py:968`, `:974`). Apply da migração obtém juiz (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `:154`, `:358`); reversão usa arquivo (`:412`, `:423`). | Chave do `do_fato` e busca (`app/chamados/models.py:2647`); dia próprio de marco (`app/chamados/services/fio.py:1250`); C6 lê gravada (`app/chamados/juizes.py:336`), tranca calcula competência por ela (`:948`, `:961`); plantio combina dia gravado com hora (`app/chamados/services/materializacao.py:344`, `:345`, `:414`, `:421`). |
| `celula` | Nascimento/reuso consulta dia `_dia_fato`, atribui e salva (`app/chamados/models.py:2789`, `:2823`, `:2863`, `:2868`); rechaveamento procura `novo_dia`, atribui FK e salva (`app/chamados/services/fio.py:965`, `:969`, `:974`). | C6 usa FK somente se dia da célula e `data_evento` coincidirem (`app/chamados/juizes.py:634`, `:638`, `:642`); a costura terminal chama C6 no pós-estado (`app/chamados/services/fio.py:779`, `:781`). |
| `chamado_destino` | Nascimento (`app/chamados/models.py:2868`); reuso após E4-c e guarda terminal (`:2829`, `:2837`, `:2839`, `:2863`); reapontamento (`app/chamados/services/fio.py:865`, `:867`, `:877`, `:879`). Atribuições `models.py:367` e `fio.py:779` são projeções em memória, a última desfeita em `:783`. | Âncora da listagem (`app/chamados/services/fio.py:117`); lookup da disputa (`:135`); status terminal (`:770`); destino do plantio (`app/chamados/services/materializacao.py:339`); E4-c compara o dia do chamado (`app/chamados/juizes.py:2018`, `:2027`). |
| `batida_origem_id` | Explícita ou derivada antes de pedir dia (`app/chamados/models.py:2760`, `:2762`), persistida no create via `campos` (`:2868`); derivação de inserção no save quando ausente (`:3269`, `:3297`, `:3298`). Derivador usa família BATIDA, contexto da disputa e confere colaborador, sem conferir retratação (`:3310`, `:3312`, `:3316`). | Busca de episódio (`app/chamados/models.py:2647`); juiz (`app/chamados/services/fio.py:1242`, `:1247`, `:1248`); recusa (`:1266`, `:1268`); chave da família (`:1131`); contagem de episódios no plano (`:1060`). |
| `dia_da_chave` | Helper passa `d`, que é pedido se disponível, senão gravada (`app/chamados/services/fio.py:1297`, `:1309`). | Só altera pergunta de vizinhança das sementes (`:1358`, `:1361`); vizinhos posteriores usam suas chaves (`:1367`); cache inclui override (`:1336`, `:1337`); `do_fato` recebe override (`:1204`). |
| `dia_do_fato` parâmetro | Somente `_nascer` passa `_dia_fato` ao proxy e à canônica (`app/chamados/models.py:2781`, `:2829`), valor obtido do helper (`:2769`). | `resolvedora_valida` chama recusa e usa parâmetro diretamente (`app/chamados/juizes.py:2004`, `:2005`, `:2006`); sem parâmetro, consulta helper (`:2008`). |

**Campo com significados divergentes — PROCEDENTE no acervo reusado.** A porta pode mudar célula/destino para DIA e conservar `data_evento=D0`; o teste faz essa preservação explicitamente (`app/chamados/models.py:2823`, `:2839`, `:2863`; `app/chamados/tests/test_e1_v15.py:98`, `:99`). E4-c passa pelo dia do fato enquanto C6/tranca/plantio continuam lendo gravada; C6 rejeita alimentação da célula divergente e tranca calcula competência de D0 (`app/chamados/juizes.py:2006`, `:336`, `:638`, `:948`, `:961`; `app/chamados/services/materializacao.py:345`, `:414`). Migração integral de produção e execução concreta de plantio nesse estado: **[nao verificado]**.

A própria função de identidade declara dois papéis para `data_evento`: chave gravada de localização e identidade calculada pelo juiz (`app/chamados/services/fio.py:1231`, `:1232`, `:1233`). Conversa também usa campo como dia civil, com discriminador explícito (`app/chamados/models.py:2717`, `:2718`, `:2722`; `app/chamados/services/fio.py:1298`, `:1299`).

**Consequências de R retratada admitida pelo fecho:** a eleição não chama recusa e considera resposta/menor id (`app/chamados/services/fio.py:1004`, `:1005`, `:1006`). `plano_de_retratacao` conta R como episódio e pode declarar `episodios_unidos` entre R e A vivo, bloqueando retratação (`:1060`, `:1065`, `:1066`).

- Dedup DRY elege essa canônica e aplica plano (`app/chamados/management/commands/dedup_perguntas_fato.py:76`, `:81`); apply delega ao backfill (`:104`, `:105`).
- Backfill elege a mesma canônica e chama o plano, sem recusa da canônica (`app/chamados/management/commands/backfill_decisoes_pergunta.py:265`, `:279`, `:284`, `:320`).
- Migração de resolvedoras elege canônica do universo; se R é a própria pergunta no chamado antigo, tenta reapontar e recebe recusa pela E4-c (`app/chamados/services/fio.py:1753`, `:1763`, `:1772`, `:1773`, `:865`). Se R é canônica externa, a eleição por si não passa pela recusa (`:1753`, `:1763`, `:1778`).
- Migração de data inclui R nos membros do grupo e lê seu dia gravado no veto de competência (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:254`, `:277`, `:279`, `:280`).

**Travas/relógio/escritor extra:** `_nascer` trava colaborador antes da nova resolução e consultas (`app/chamados/models.py:2735`, `:2739`, `:2769`). Reapontamento resolve `_dia` antes do atomic interno, mas toma trava de colaborador antes da guarda de Chamado e do save (`app/chamados/services/fio.py:846`, `:852`, `:858`, `:864`, `:867`, `:879`); isso é leitura anterior à trava, sem nova aquisição invertida nesse trecho. Rechaveamento toma Colaborador/Disputa/Pergunta por porta (`:956`, `:957`) e resolvedora não adquire trava (`app/chamados/juizes.py:2003`, `:2008`). O novo diff não acrescenta leitura de relógio; conversa mantém `localdate()` na borda (`app/chamados/models.py:2722`). Auditoria transitiva universal de travas/relógios e formas dinâmicas de escrita: **[nao verificado]**.

## 08/11 11:32 · sessao ab592c50e326 · FIO-E0-V4-E1 · verificador · origem=Code

**REPROVADO no alcance universal do aval.** A correção atende aos casos originais com episódio cujo juiz responde um dia, mas o fallback ainda permite gravar o pedido e reproduzir 3a/3b. A catraca verifica a chamada ao helper, sem garantir essa semântica. (`app/chamados/services/fio.py:1258`, `app/chamados/services/fio.py:1262`; `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:59`)

1. **CENSO — NÃO PROCEDENTE a afirmação de que toda escrita grava exclusivamente o dia do juiz.**

   Os sítios de atribuição e persistência encontrados são:

   | Sítio | Valor e classificação |
   |---|---|
   | `PerguntaDisputa._nascer`: `app/chamados/models.py:2840` | `create(data_evento=_dia_fato)`. Obtém `_dia_fato` pelo helper em `app/chamados/models.py:2749`; o helper pode devolver o pedido, conforme `app/chamados/services/fio.py:1258`–`1262`. |
   | `reapontar_resolvedora`: `app/chamados/services/fio.py:843` | Atribui `_dia`, calculado pelo mesmo helper em `app/chamados/services/fio.py:842`. Acrescenta o campo à lista em `app/chamados/services/fio.py:844` e persiste em `app/chamados/services/fio.py:873`. Mesma ressalva de fallback. |
   | `rechavear_data_evento`: `app/chamados/services/fio.py:962` | Atribui o parâmetro `novo_dia` e persiste em `app/chamados/services/fio.py:968`; não consulta o juiz nessa porta. |
   | Chamador de rechaveamento, apply | O valor vem de `dia_do_turno_da_batida`, passa por `ln['novo']` e chega à porta. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:154`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:358`) |
   | Chamador de rechaveamento, reversão | Grava a data recuperada do arquivo de reversão, sem recalculá-la pelo juiz. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:412`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:423`) |

   Há ainda três atribuições **sem persistência própria**: o proxy da validação inicial e os dois desfazimentos em memória antes de retornar por recusa. (`app/chamados/models.py:2756`; `app/chamados/services/fio.py:855`, `app/chamados/services/fio.py:864`)

   Não encontrei escritor adicional de `data_evento` por `update`, bulk, `setattr` ou construtor com apelido. O bulk existente de Pergunta grava somente os três campos da lavra; o `save` do modelo delega a persistência sem normalizar `data_evento`. (`app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:207`; `app/chamados/models.py:3242`–`3245`) Exaustividade contra formas dinâmicas arbitrárias: **[nao verificado]**.

   **Chamado incluído:** nesta cópia, a classe é `ChamadoColaborador` e **não possui campo `data_evento`**; possui `contexto_json`, `celula` e timestamps. Suas portas de INSERT são `abrir`, com `get_or_create`, e `criar`, com `create`. (`app/chamados/models.py:38`–`101`, `app/chamados/models.py:971`, `app/chamados/models.py:1095`)

   Essas duas portas preservam o contexto recebido e resolvem a célula por `_celula_de`, que consulta `juizes.dia_do_fato(fortes=True)`. Não são escritoras literais de `data_evento`, nem normalizam o contexto pelo novo helper da Pergunta. (`app/chamados/models.py:939`–`940`, `app/chamados/models.py:958`, `app/chamados/models.py:1067`, `app/chamados/models.py:1234`–`1240`)

2. **CATRACA — NÃO PROCEDENTE como garantia completa; PROCEDENTE a mordida dos cinco mutantes oficiais.**

   O P13 identifica os escritores explícitos atuais. Reconhece apelidos de import e atribuições simples de alias, `update(data_evento=...)`, `setattr` com nome literal e bulk com campo literal. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:37`–`54`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:84`–`108`)

   A V15 fiscaliza valores somente em `_nascer` e `reapontar_resolvedora`. Para rechaveamento, verifica o **arquivo chamador**, não a procedência do valor. Isoladamente, também não reprova escritores novos encontrados pelo censo: apenas exige `assertTrue(censo())`; a rejeição de sítios externos pertence a outro teste, no P13. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:24`–`30`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:118`–`134`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:143`–`146`)

   Escapes da fiscalização:

   - **Dentro de porta permitida:** trocar `p.data_evento = _dia` por `setattr(p, 'data_evento', data_evento)` escapa da V15. Ela examina `ast.Assign` e keywords diretos; o P13 aceita a localização. Bulk com campos posicionais também não recebe fiscalização de valor pela V15. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:74`–`88`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:117`–`119`)
   - **Fluxo por variável:** `kw={'data_evento': d}; qs.update(**kw)` e `campos=['data_evento']; qs.bulk_update(ps, campos)` escapam do P13, que procura constantes nos argumentos da própria chamada. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:105`–`108`)
   - **Outra classe:** construtor direto só é reconhecido quando pertence aos aliases de `PerguntaDisputa`; chamadas ORM com keyword literal são reconhecidas independentemente da classe. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:29`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:94`–`97`)
   - **Semântica do helper:** chamar uma função chamada `dia_do_turno_da_linha` basta para ligar o valor ao “juiz”; seu fallback não é examinado. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:45`–`66`)

   Os cinco mutantes oficiais detectam regressões específicas das atribuições e da costura. Isso não cobre os escapes acima. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:138`–`154`) Execução da suíte Django/BD: **[nao verificado]**.

3. **FALLBACK — PROCEDENTE: reabre o defeito em caminho de produção alcançável.**

   Sem episódio, o helper devolve pedido/gravado diretamente; com episódio e resposta `None`, faz o mesmo. Já o universo consulta A2 para decidir o dia dessas linhas. Batida retratada pode produzir `None`, e `_nascer` aceita episódio explícito sem verificar retratação. (`app/chamados/services/fio.py:1258`–`1262`, `app/chamados/services/fio.py:1401`–`1408`; `app/ponto/turnos.py:2917`–`2926`; `app/chamados/models.py:2745`–`2747`)

   **Contraexemplo construído de 3a**, com `D0=10/09` e `DIA=18/10`: K é acervo sem episódio gravado em D0; A tem episódio vivo `a`, também gravado em D0, mas o juiz responde DIA; `b` é episódio retratado ainda sem Pergunta. O vínculo entre acervo e episódios pela data gravada é exatamente o de `do_fato`. (`app/chamados/models.py:2627`–`2635`; datas da base: `app/chamados/tests/test_e1_v13.py:40`, `app/chamados/tests/test_e1_v13.py:60`–`63`)

   - Primeira `abrir(b, D0)`: helper responde D0. O universo encontra K; A2(K) alcança A, cujo dia é DIA, e recusa K. Não existe recusada do episódio `b`; nasce L com episódio `b` e data D0. (`app/chamados/services/fio.py:1258`–`1262`, `app/chamados/services/fio.py:1300`–`1308`, `app/chamados/services/fio.py:1406`; `app/chamados/models.py:2781`, `app/chamados/models.py:2840`)
   - Repetição idêntica: encontra K e L. A2(L) passa por K e alcança A=DIA; ambas são recusadas para o pedido D0. Agora há recusada do episódio `b`, portanto levanta **`FatoEmOutroDia`**. (`app/chamados/services/fio.py:1300`–`1308`, `app/chamados/services/fio.py:1406`–`1409`; `app/chamados/models.py:2781`–`2792`)

   Assim, a primeira chamada cria e a segunda idêntica recusa. Ocorrência desse estado na frota: **[nao verificado]**.

   **Conversa é diferente:** tem contrato explícito de nascer sempre, gravar dia civil e dispensar comparação de turno. Sua falta de dedup não constitui esse 3a. (`app/chamados/models.py:2700`–`2707`, `app/chamados/models.py:2765`; `app/chamados/juizes.py:1991`–`1992`)

4. **MEMO — NÃO PROCEDENTE a hipótese de colisão entre os usos indicados.**

   | Uso | Chave | Escritas |
   |---|---|---|
   | Dia da batida | `('dia_do_turno', batida_id)` | `app/chamados/services/fio.py:1233`, `app/chamados/services/fio.py:1238` |
   | Colaborador | `('colab', colaborador_id)` | `app/chamados/services/fio.py:1268`, `app/chamados/services/fio.py:1271`, `app/chamados/services/fio.py:1274` |
   | Vizinhos | `(pk, chave_do_dia, batida_id)` | `app/chamados/services/fio.py:1280`, `app/chamados/services/fio.py:1287`, `app/chamados/services/fio.py:1390` |

   As duas primeiras famílias têm duas posições e marcadores distintos; a terceira tem três posições. Os acessos usam essas chaves completas. A resolvedora apenas repassa o dict ao helper: não introduz outra escrita nele. (`app/chamados/services/fio.py:1234`–`1239`, `app/chamados/services/fio.py:1269`–`1288`; `app/chamados/juizes.py:1994`)

5. **3a/3b ORIGINAIS — PROCEDENTE a cura com juiz não nulo; NÃO PROCEDENTE a garantia universal.**

   **3a, episódio vivo:** pedidos D0 e DIA viram o mesmo `_dia_fato`; busca, célula e criação usam esse valor. A repetição encontra a canônica e retorna sem novo nascimento. Esse é também o comportamento exigido pelo teste novo. (`app/chamados/models.py:2749`–`2751`, `app/chamados/models.py:2768`, `app/chamados/models.py:2779`, `app/chamados/models.py:2837`, `app/chamados/models.py:2840`; `app/chamados/tests/test_e1_v15.py:52`–`66`)

   **3b, episódio vivo:** linha gravada em D0, juiz=DIA, destino=DIA passa a ser aceita, porque a resolvedora lê DIA pelo helper. O reuso preserva D0 no banco. (`app/chamados/juizes.py:1994`, `app/chamados/juizes.py:2013`–`2014`; `app/chamados/services/fio.py:1261`–`1262`; `app/chamados/tests/test_e1_v15.py:90`–`99`)

   **Ainda pode levantar `ResolvedoraCruzada`:** tome K sem episódio, gravada em D0 e canônica por resposta/menor pk, mais A do episódio vivo `a`, gravada em D0 e juiz=DIA. `abrir(a, DIA, destino=DIA)` passa no proxy, semeia A, alcança K por A2={DIA} e elege K. Na segunda validação, o helper de K devolve D0; o gap de 38 dias produz recusa e exceção. (`app/chamados/models.py:2756`–`2762`, `app/chamados/models.py:2802`–`2804`; `app/chamados/services/fio.py:998`–`1000`, `app/chamados/services/fio.py:1259`–`1260`, `app/chamados/services/fio.py:1361`–`1368`; `app/chamados/juizes.py:2022`–`2029`)

   **Variante com episódio na própria linha recusada:** acrescente R respondida, de episódio retratado, gravada em D0 e ligada por K a A. O fecho admite R por A2 e a canônica passa a ser R; seu juiz responde `None`, o helper devolve D0 e ocorre a mesma `ResolvedoraCruzada`. Portanto a resposta à pergunta sobre linha **com episódio** ainda é sim, quando ele está retratado. (`app/chamados/services/fio.py:1238`, `app/chamados/services/fio.py:1262`, `app/chamados/services/fio.py:1363`–`1368`, `app/chamados/services/fio.py:999`; `app/chamados/models.py:2804`) Ocorrência em produção: **[nao verificado]**.

6. **ESCRITORES/LEITORES — PROCEDENTE a divergência residual de significado.**

   | Campo | Quem escreve nos caminhos examinados | Quem lê |
   |---|---|---|
   | `data_evento` | Nascimento, datação e rechaveamento censados acima. (`app/chamados/models.py:2840`; `app/chamados/services/fio.py:843`, `app/chamados/services/fio.py:962`) | Costura pelo helper; fecho pelo juiz/A2; busca pela chave gravada; C6/tranca/plantio pela gravada. (`app/chamados/juizes.py:1994`; `app/chamados/services/fio.py:1196`, `app/chamados/services/fio.py:1232`; `app/chamados/juizes.py:336`, `app/chamados/juizes.py:948`; `app/chamados/services/materializacao.py:345`) |
   | `celula` | Nascimento/reuso pela célula de `_dia_fato`; rechaveamento pela célula de `novo_dia`. (`app/chamados/models.py:2768`, `app/chamados/models.py:2798`, `app/chamados/models.py:2841`; `app/chamados/services/fio.py:959`–`963`) | C6 lê a célula; alimentação da falta exige igualdade com `data_evento`. (`app/chamados/juizes.py:634`–`642`, `app/chamados/juizes.py:1199`–`1214`) |
   | `batida_origem_id` | Explícita ou derivada no nascimento; o `save` usa o mesmo derivador quando ausente. (`app/chamados/models.py:2745`–`2747`, `app/chamados/models.py:3270`–`3289`) | Busca do episódio, juiz do turno e seleção do fallback. (`app/chamados/models.py:2632`; `app/chamados/services/fio.py:1232`–`1239`, `app/chamados/services/fio.py:1259`) |
   | `chamado_destino` | Nascimento, reuso após guardas e reapontamento. (`app/chamados/models.py:2841`, `app/chamados/models.py:2812`; `app/chamados/services/fio.py:871`–`874`) | Validação de turno, destino da listagem, guarda de terminal e marcos do plantio. (`app/chamados/juizes.py:2004`; `app/chamados/services/fio.py:117`, `app/chamados/services/fio.py:778`–`783`; `app/chamados/services/materializacao.py:339`, `app/chamados/services/materializacao.py:409`) |

   O estado residual é explícito: reuso pode associar célula/destino de DIA e manter `data_evento=D0`. A costura julga DIA, enquanto tranca calcula competência por D0; C6 descarta a célula de DIA como alimentação para D0. (`app/chamados/models.py:2797`–`2813`; `app/chamados/tests/test_e1_v15.py:99`; `app/chamados/juizes.py:1994`, `app/chamados/juizes.py:948`–`961`, `app/chamados/juizes.py:638`)

   O derivador de plantio, quando alcançado, continua combinando a hora com a data gravada e ajustando overnight a partir dela. Isso identifica a divergência de leitura; plantio efetivamente executado nesses estados: **[nao verificado]**. (`app/chamados/services/materializacao.py:344`–`345`, `app/chamados/services/materializacao.py:414`–`421`)

   Os leitores permanecerem intactos atende ao recorte literal do aval. Isso, porém, não comprova a premissa “gravada == juiz”: o próprio teste conserva a divergência, e o fallback pode criá-la. Migração integral do acervo em produção: **[nao verificado]**. (`app/chamados/tests/test_e1_v15.py:98`–`99`; `app/chamados/services/fio.py:1262`)

7. **CLASSES DE REPROVA — presentes as seguintes:**

   - **Derivação alternativa à fonte única: PROCEDENTE.** O juiz documenta que `None` significa “sem resposta [...] não há dia a gravar”; o helper substitui essa ausência pelo pedido/gravado e não consulta A2. (`app/ponto/turnos.py:2919`–`2920`; `app/chamados/services/fio.py:1258`–`1262`)
   - **Selo insuficiente: PROCEDENTE quanto à abrangência.** Os mutantes oficiais mordem, mas `setattr`/bulk dentro da porta e a semântica interna do helper permanecem descobertos. (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:59`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:74`–`88`, `app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:138`–`154`)
   - **Campo com significados divergentes: PROCEDENTE no acervo/fallback.** Identidade pelo juiz/A2 e chave gravada continuam podendo divergir, com consumidores decisórios lendo a gravada. (`app/chamados/services/fio.py:1222`–`1231`; `app/chamados/tests/test_e1_v15.py:99`; `app/chamados/juizes.py:336`)
   - **Segundo juiz geométrico duplicando o registrado: NÃO PROCEDENTE neste trajeto.** O helper chega a `_T1`, cuja implementação consulta `turnos_do_colab`, autoridade registrada. (`app/core/juizes.py:196`; `app/chamados/services/fio.py:1236`–`1238`; `app/ponto/turnos.py:2902`–`2903`, `app/ponto/turnos.py:2925`)
   - **Segundo escritor explícito fora da porta: NÃO PROCEDENTE no censo encontrado.** Os escritores concretos estão nos três sítios listados; rechaveamento é uma porta declarada, embora aceite valor fornecido. (`app/chamados/models.py:2840`; `app/chamados/services/fio.py:843`, `app/chamados/services/fio.py:962`)
   - **Relógio novo fora da borda: NÃO PROCEDENTE nas alterações examinadas.** O relógio identificado está na porta explícita de conversa. Auditoria transitiva integral de relógios: **[nao verificado]**. (`app/chamados/models.py:2706`–`2707`)
   - **Inversão nova de trava: NÃO PROCEDENTE nos caminhos examinados.** `_nascer` trava Colaborador antes das consultas; reapontamento adquire essa trava por `fato_livre_para` ou diretamente antes da guarda de Chamado. Todos os chamadores/transações externos: **[nao verificado]**. (`app/chamados/models.py:2724`, `app/chamados/models.py:2677`; `app/chamados/services/fio.py:852`–`861`, `app/chamados/services/fio.py:768`)

## 08/11 11:29 · sessao d1aba136826f · - · - · origem=Ronald (tmux)

Checagem adicional concluída: nenhum escritor real extra encontrado fora das portas já listadas. A busca incluiu todos os `.py` da árvore, aliases de import/atribuição e referências fora de `app/`. [nao verificado: artefato persistido do censo]

Os candidatos dinâmicos existentes não escrevem `data_evento`:

- `descarimbar_em_lote(**outros)` recebe campos de resposta/validação (`app/chamados/models.py:1555`; `app/chamados/services/disputa_emissao.py:1494`, `:1531`).
- Atualização dinâmica do Chamado usa somente `urgencia` e `urgencia_original` (`app/chamados/models.py:473`, `:631`, `:636`).
- Bulk de Pergunta grava somente os três campos da lavra (`app/chamados/veredito_lavrado.py:194`, `:207`).
- `CC = apps.get_model(...)` em `bin/r5_idempotencia_frota.py:30` somente consulta chamados em `bin/r5_idempotencia_frota.py:60`.

Os únicos INSERTs diretos de Chamado fora dos gates encontrados são testes em arquivos soltos: `app/tenants/tests.py:62` (`FastTenantTestCase`, `:5`) e `app/relatorios/tests_pdf_chamados.py:34` (`TestCase`, `:18`).

Não encontrei construtor aliased de Pergunta seguido de save em produção, nem bypass real de INSERT de Chamado; permanecem `abrir` e `criar` (`app/chamados/models.py:971`, `:1095`). [nao verificado: prova negativa exaustiva contra formas dinâmicas arbitrárias]

## 08/11 11:29 · sessao cc28a568237e · - · - · origem=Ronald (tmux)

**3. PROCEDENTE: o fallback mantém caminhos que reproduzem 3a/3b.** Sem episódio, devolve diretamente pedido/data gravada; com episódio cujo juiz responde `None`, também devolve esse valor. A ponte A2 usada pelo universo não é consultada pelo helper (`app/chamados/services/fio.py:1258`, `app/chamados/services/fio.py:1262`, `app/chamados/services/fio.py:1406`). Batida retratada pode produzir `None`; a porta aceita `batida_origem_id` explícita sem conferir retratação (`app/ponto/turnos.py:2917`, `app/ponto/turnos.py:2925`, `app/chamados/models.py:2745`).

Contraexemplo concreto de **3a**, por trace:

- D0=10/09; DIA=18/10. Estado inicial: K sem episódio gravada em D0; A com episódio vivo `a`, gravada em D0, juiz=DIA; episódio `b` retratado sem pergunta. Esses estados são representáveis pelo modelo; `do_fato` conecta acervo do mesmo dia aos episódios (`app/chamados/models.py:2630`, `app/chamados/models.py:2632`).
- Primeiro `abrir(b,D0)`: helper=D0; universo encontra K; A2(K) alcança A=DIA, logo recusa K. Não há recusada de episódio `b`, portanto nasce L(b,D0) (`app/chamados/services/fio.py:1300`, `app/chamados/services/fio.py:1304`, `app/chamados/services/fio.py:1406`, `app/chamados/models.py:2781`, `app/chamados/models.py:2840`).
- Repetição idêntica: universo encontra K e L; A2(L) passa por K e alcança A=DIA. Ambas ficam recusadas; agora existe recusada de episódio `b`, disparando `FatoEmOutroDia` (`app/chamados/services/fio.py:1300`, `app/chamados/services/fio.py:1406`, `app/chamados/models.py:2781`, `app/chamados/models.py:2789`).

Contraexemplo concreto de **3b**:

- K sem episódio, menor pk ou respondida, gravada em D0; A de episódio vivo, gravada em D0, juiz=DIA. `abrir(a,DIA,destino=DIA)` semeia A, alcança K pela ponte A2={DIA} e elege K (`app/chamados/services/fio.py:1401`, `app/chamados/services/fio.py:1421`, `app/chamados/services/fio.py:1358`, `app/chamados/services/fio.py:1363`, `app/chamados/services/fio.py:998`).
- A validação inicial do proxy passa usando DIA. No reuso, K sem episódio devolve D0; o gap de 38 dias termina em recusa e `ResolvedoraCruzada` (`app/chamados/models.py:2756`, `app/chamados/models.py:2760`, `app/chamados/models.py:2802`, `app/chamados/models.py:2804`, `app/chamados/services/fio.py:1259`, `app/chamados/juizes.py:2022`, `app/chamados/juizes.py:2029`).
- Variante com episódio: acrescente R respondida, de episódio retratado, gravada em D0, ligada por K ao mesmo A. O fecho admite R por A2 e a elege; o helper recebe `None` do juiz e volta a D0, produzindo a mesma rejeição. Portanto ainda pode rejeitar uma linha **com episódio**, quando ele está retratado (`app/chamados/services/fio.py:1238`, `app/chamados/services/fio.py:1262`, `app/chamados/services/fio.py:1363`, `app/chamados/services/fio.py:999`, `app/chamados/models.py:2804`).

A conversa tem tratamento explícito: nasce sempre, grava dia civil e não passa pela comparação de turno. Isso não reproduz o defeito contratual de dedup de pergunta datada (`app/chamados/models.py:2700`, `app/chamados/models.py:2707`, `app/chamados/models.py:2765`, `app/chamados/juizes.py:1991`).

**4. NÃO PROCEDENTE: não há colisão entre as chaves dos usos descritos.** Dia usa tupla de dois elementos `('dia_do_turno', id)` e escreve em 1238; colaborador usa `('colab', id)` e escreve em 1271/1274; vizinhança usa tupla de três elementos `(pk, chave_do_dia, batida_id)`, escrita em 1287 e 1390. Tamanho ou marcador distinguem esses espaços (`app/chamados/services/fio.py:1233`, `app/chamados/services/fio.py:1238`, `app/chamados/services/fio.py:1268`, `app/chamados/services/fio.py:1271`, `app/chamados/services/fio.py:1274`, `app/chamados/services/fio.py:1280`, `app/chamados/services/fio.py:1287`, `app/chamados/services/fio.py:1390`). A resolvedora apenas repassa o memo ao helper (`app/chamados/juizes.py:1994`).

**5. Cura dos casos originais com episódio vivo: PROCEDENTE; garantia universal: NÃO PROCEDENTE.** Com resposta não nula do juiz, pedidos D0/DIA viram o mesmo `_dia_fato`, usado na busca, célula e criação; a repetição retorna a canônica existente. A resolvedora lê esse mesmo juiz, independentemente da data gravada (`app/chamados/models.py:2749`, `app/chamados/models.py:2768`, `app/chamados/models.py:2779`, `app/chamados/models.py:2837`, `app/chamados/models.py:2840`, `app/chamados/services/fio.py:1261`, `app/chamados/juizes.py:1994`, `app/chamados/juizes.py:2013`). Os contraexemplos acima impedem estender a conclusão aos caminhos de fallback (`app/chamados/services/fio.py:1262`).

Executei uma reprodução somente em memória dos corpos AST reais de helper/A2/fecho/universo/canônica, adaptando o ORM para listas e pré-carregando respostas do juiz: primeira chamada do cenário 3a teve universo vazio/recusada K; repetição teve universo vazio/recusadas K,L; os dois cenários 3b elegeram a linha cujo helper retornou D0. Execução Django/BD e ocorrência em produção: **[nao verificado]**. [nao verificado]

## 08/11 11:28 · sessao d1aba136826f · - · - · origem=Ronald (tmux)

Censo literal de `data_evento`:

| Sítio persistente | Valor gravado |
|---|---|
| `app/chamados/models.py:2840` (`PerguntaDisputa._nascer`) | `_dia_fato`, obtido por `dia_do_turno_da_linha(..., pedido=data_evento)` em `app/chamados/models.py:2749`. |
| `app/chamados/services/fio.py:843`, persistido em `app/chamados/services/fio.py:873` | `_dia`, obtido pelo helper em `app/chamados/services/fio.py:842`; campo acrescentado à lista em `app/chamados/services/fio.py:844`. |
| `app/chamados/services/fio.py:962`, persistido em `app/chamados/services/fio.py:968` | Parâmetro `novo_dia`, sem consulta local ao juiz; apply passa o resultado de `dia_do_turno_da_batida` (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `:154`, `:358`); reversão passa a data lida do arquivo (`:412`, `:423`). |

`_px.data_evento = _dia_fato` é proxy em memória (`app/chamados/models.py:2756`); `p.data_evento = None` desfaz em memória duas recusas que retornam antes do save (`app/chamados/services/fio.py:855`, `:864`). Nenhum escritor explícito adicional em produção foi encontrado; exaustividade além das formas pesquisadas permanece [nao verificado].

**Chamado incluído:** `ChamadoColaborador` não declara campo `data_evento`; declara `contexto_json`, `celula` e timestamps (`app/chamados/models.py:38`–`:101`). Suas duas portas concretas de INSERT são `abrir` (`app/chamados/models.py:971`) e `criar` (`app/chamados/models.py:1095`). Elas gravam contexto recebido por `defaults`/`campos` (`app/chamados/models.py:939`, `:1095`) e resolvem célula em `app/chamados/models.py:958`/`:1067`; `_celula_de` pergunta a `chamados.juizes.dia_do_fato(fortes=True)` (`app/chamados/models.py:1234`), não ao helper de data da Pergunta. Portanto não se deve classificá-las como escritores de um campo inexistente.

**Catraca: PROCEDENTE para os escritores explícitos atuais; NÃO PROCEDENTE como garantia geral.** P13 identifica esses sete registros sintáticos: proxy 2756, create 2840, atribuições fio 843/855/864/962 e save explícito fio 968; seus detectores estão em `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:84`–`:108`. V15 verifica os valores de duas portas (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:24`); rechaveamento recebe somente fiscalização dos arquivos chamadores (`:97`, `:133`).

- Apelidos de import e aliases simples de `PerguntaDisputa` são cobertos pelo P13 (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:37`).
- `update(data_evento=...)`, bulk com campo literal e `setattr(...,'data_evento',...)` são detectados fora da porta (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:94`, `:98`, `:107`).
- Fluxo por variável escapa: `campos={'data_evento':d}; qs.update(**campos)` e `campos=['data_evento']; qs.bulk_update(ps,campos)`. Os detectores só procuram constantes nos argumentos locais da chamada (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:93`, `:105`, `:107`).
- Construtor de outra classe não entra na lista de aliases de Pergunta (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:29`, `:96`).
- Dentro de porta permitida, V15 ignora `setattr`, atribuição anotada/aumentada e bulk por campo posicional: inspeciona somente `ast.Assign` e keywords diretos (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:74`–`:88`); P13 aceita a localização (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:117`). Contra-mutante concreto, validado somente em memória: substituir fio:843 por `setattr(p, 'data_evento', data_evento)` mantém `violacoes_do_escritor=[]`; nenhum arquivo foi alterado [nao verificado: registro persistido da execução].
- Os cinco mutantes oficiais mordem nos analisadores AST (`app/chamados/tests/test_e1_v15_escritor_pelo_juiz.py:138`–`:154`); extraí e executei as funções puras via `python3 -B`, sem importar Django nem acessar banco. Resultado da execução sem artefato persistido: [nao verificado]. A limitação do mutante alternativo decorre diretamente de `:74`–`:88`.

## 08/11 11:28 · sessao d5d52281b203 · - · - · origem=Ronald (tmux)

**Item 6 — PROCEDENTE: os leitores continuam atribuindo significado de turno ao valor gravado, embora o reuso deixe o acervo com outra data.** A própria prova da volta espera `data_evento=D0` depois de costurar a linha no chamado de `DIA`; o reuso atualiza célula/destino, sem atualizar `data_evento`. `app/chamados/tests/test_e1_v15.py:90-99`; `app/chamados/models.py:2797-2813`; `app/chamados/models.py:2836`.

| Campo/valor | Quem escreve nos caminhos examinados | Quem lê e para quê |
|---|---|---|
| `data_evento` | `_nascer`: `_dia_fato` obtido em `dia_do_turno_da_linha`, depois `create`; `reapontar_resolvedora`: data pergunta sem dia pelo mesmo wrapper; `rechavear_data_evento`: grava parâmetro `novo_dia`. `app/chamados/models.py:2749-2751`; `app/chamados/models.py:2840`; `app/chamados/services/fio.py:842-844`; `app/chamados/services/fio.py:962-968`. | Costura: wrapper/juiz; fecho: `_dia_do_fato`; identidade de busca: valor gravado; C6/tranca/plantio: valor gravado. `app/chamados/juizes.py:1993-1994`; `app/chamados/services/fio.py:1217-1243`; `app/chamados/services/fio.py:1196`; `app/chamados/juizes.py:332-336`; `app/chamados/services/materializacao.py:344-345`. |
| `celula` | `_nascer` busca por `_dia_fato`, atribui ao existente ou cria com ela; rechaveamento busca célula de `novo_dia`. `app/chamados/models.py:2768`; `app/chamados/models.py:2797-2799`; `app/chamados/models.py:2841`; `app/chamados/services/fio.py:959-963`. | C6 lê veredito da célula; alimentação de falta exige que `celula.data == data_evento`, descartando a FK divergente; chamado deriva seu dia pela célula antes dos outros degraus. `app/chamados/juizes.py:1166-1168`; `app/chamados/juizes.py:1199-1214`; `app/chamados/juizes.py:634-642`; `app/chamados/juizes.py:98-99`. |
| `batida_origem_id` | `_nascer` recebe campo explícito ou deriva pelo mesmo `batida_origem_do_nascimento` utilizado no `save` de inserção; deriva de `disputa.contexto.ultima_batida.id`, validando dono quando a derivação ocorre. `app/chamados/models.py:2745-2747`; `app/chamados/models.py:3270-3271`; `app/chamados/models.py:3282-3289`. | Wrapper decide se consulta juiz; `_dia_do_fato` carrega Batida e consulta `_T1`; busca/dedup usa episódio. `app/chamados/services/fio.py:1259-1262`; `app/chamados/services/fio.py:1232-1239`; `app/chamados/models.py:2781`; `app/chamados/models.py:2680`. |
| `chamado_destino` | Nascimento cria com destino; reuso atribui após guardas; reapontamento atribui e salva com lavra. `app/chamados/models.py:2841`; `app/chamados/models.py:2800-2813`; `app/chamados/services/fio.py:859-874`. | Costura compara dia; `chamado_que_lista` escolhe destino antes do pai; plantio usa destino para marcos; guarda terminal julga C6 no pós estado. `app/chamados/juizes.py:1994-2029`; `app/chamados/services/fio.py:116-117`; `app/chamados/services/materializacao.py:339`; `app/chamados/services/materializacao.py:409`; `app/chamados/services/fio.py:778-783`. |

Contraexemplo de leitura mantido pelo código: linha com episódio cujo juiz diz `DIA`, gravada em `D0`; `abrir(..., DIA)` elege a linha, associa célula de `DIA`, aceita costura pelo juiz e preserva `D0`. Então:

- A costura compara `DIA`, enquanto `_dia_de_turno_da_pergunta` devolve `D0`. `app/chamados/models.py:2749-2751`; `app/chamados/models.py:2768`; `app/chamados/models.py:2802`; `app/chamados/juizes.py:1994`; `app/chamados/juizes.py:336`.
- A tranca calcula competência por `D0`. `app/chamados/juizes.py:948-961`.
- C6, quando alcança falta do dia, consulta `D0`; a célula de `DIA` não serve para alimentar essa consulta porque a igualdade falha. `app/chamados/juizes.py:1216-1222`; `app/chamados/juizes.py:638`.
- Plantio tipo hora combina resposta com `D0`, depois faz ajuste overnight a partir de `D0`. Não consulta o novo wrapper nesse caminho. `app/chamados/services/materializacao.py:344-345`; `app/chamados/services/materializacao.py:414-421`.
- A guarda de costura em chamado terminal decide competência entregue por `p.data_evento`, portanto também pelo gravado. `app/chamados/services/fio.py:776`.

Isso demonstra discrepância sem exigir migração de leitores, cuja preservação está expressamente no escopo dado; comprovar que a migração já eliminou o estado em produção é **[nao verificado]**. `app/chamados/tests/test_e1_v15.py:98-99`.

**Item 7 — classes verificadas.**

- **Campo com dois significados: PROCEDENTE.** O código declara expressamente data gravada como chave e `_dia_do_fato` como identidade distinta; o reuso conserva divergência observável entre leitores acima. `app/chamados/services/fio.py:1220-1227`; `app/chamados/tests/test_e1_v15.py:98-99`.
- **Segundo juiz geométrico duplicando `_T1`: não demonstrado.** O registro aponta `turnos_do_colab`; o wrapper chama `_dia_do_fato` → `dia_do_turno_da_batida` → `dia_das_batidas` → `turnos_do_colab`. Não há outro pareador nesse trajeto. `app/core/juizes.py:196`; `app/chamados/services/fio.py:1236-1238`; `app/ponto/turnos.py:2923-2926`; `app/ponto/turnos.py:2902-2903`.
- **Derivação paralela de fonte: PROCEDENTE no fallback sem resposta, não na geometria.** O juiz documenta `None` como “sem resposta do juiz não há dia a gravar”, mas `dia_do_turno_da_linha` devolve pedido/gravado nessa situação. `app/ponto/turnos.py:2919-2920`; `app/chamados/services/fio.py:1258-1262`.
- **Derivação duplicada de episódio: não demonstrada.** Porta e inserção chamam a mesma função. `app/chamados/models.py:2745`; `app/chamados/models.py:3271`.
- **Relógio fora da borda: não demonstrado nas linhas alteradas.** O relógio identificado é a porta própria de conversa, que captura `localdate()`; conversa é explicitamente civil e excluída de confronto de turno. `app/chamados/models.py:2700-2707`; `app/chamados/juizes.py:1991-1992`. Auditoria transitiva integral de relógios: **[nao verificado]**.
- **Ordem de trava nova incorreta: não demonstrada nas linhas alteradas.** `_nascer` toma Colaborador antes das novas consultas e da costura; reapontamento toma Colaborador por `fato_livre_para` ou diretamente antes da guarda que trava Chamado. `app/chamados/models.py:2720-2724`; `app/chamados/services/fio.py:852-861`; `app/chamados/services/fio.py:768`. Auditoria de todos os chamadores/transações externos: **[nao verificado]**.
- Segundo escritor fora da porta e alcance/mordida do selo pertencem ao censo do outro explorer: **[nao verificado]**.

## 08/11 11:23 · sessao c343206cd790 · FIO-E0-V4-E1 · censo · origem=Code

**As classes propostas não cobrem três dos cinco achados:** o Codex viu, e a triagem confirmou sem curar. Chamá-los de **(b)** falsearia o registro, porque a triagem manteve 3a/3b como bloqueadores. — `triagem_e1_v14b.txt:3,5-6`

Numeração abaixo: primeira linha após cada marcador `== anexo: ... ==` = linha 1, contando linhas vazias.

1. **3a — segunda chamada idêntica → `FatoEmOutroDia`: sem classe aplicável entre (a–e).** Foi visto e mantido como bloqueador. — `codex_e1_v14b.r:39`; `triagem_e1_v14b.txt:5`

   **Pergunta:** “o comportamento novo (FatoEmOutroDia ou reaproveitar a linha de outra data gravada e lhe dar a celula de data_evento) esta certo pela lei do aval?” — `codex_e1_v14b.q:8`

   **Resposta:** “Portanto `abrir(D0, batida cujo juiz diz DIA)` pode criar em D0; uma chamada direta posterior igual pode recusar a própria linha pelo juiz.” — `codex_e1_v14b.r:39`

   **Triagem:** “NAO curado: PERGUNTA DE DESENHO (duas curas, ver relato). Bloqueia pouso.” **Motivo escrito de descarte: nao ha trecho.** — `triagem_e1_v14b.txt:5` [nao verificado]

2. **3b — `_resolvedora_valida` compara a data gravada: sem classe aplicável entre (a–e).** Foi visto e mantido como bloqueador. — `codex_e1_v14b.r:43`; `triagem_e1_v14b.txt:6`

   **Pergunta:** “Todos os escritores e leitores supoem o MESMO significado (dia civil gravado x dia do turno x instante)?” — `codex_e1_v14b.q:6`

   **Resposta:** “A segunda validação lê essa data antiga e lança `ResolvedoraCruzada`, pois a diferença supera um dia.” O mesmo parágrafo cita `app/chamados/models.py:2786` e `app/chamados/juizes.py:1988`. — `codex_e1_v14b.r:43`

   **Triagem:** “PROCEDENTE, provado. NAO curado: mesma PERGUNTA DE DESENHO. Bloqueia pouso.” **Motivo escrito de descarte: nao ha trecho.** — `triagem_e1_v14b.txt:6` [nao verificado]

3. **`data_evento` com dois significados: sem classe aplicável entre (a–e).** Foi visto e confirmado como causa raiz, sem cura naquela volta. — `codex_e1_v14b.r:15,91`; `triagem_e1_v14b.txt:3`

   **Pergunta:** “campo com dois significados (data_evento: chave de busca x identidade -- esta declarado e respeitado em todo leitor?)” — `codex_e1_v14b.q:12`

   **Resposta:** “**Campo com dois significados não reconciliados — procedente, gravidade alta:** a identidade nova usa o juiz, enquanto destino, C6 e plantio continuam usando a data gravada como dia do fato.” — `codex_e1_v14b.r:91`

   **Triagem:** “PROCEDENTE como causa raiz de 3a/3b; vai junto da PERGUNTA DE DESENHO (nao curado nesta volta).” **Motivo escrito de descarte: nao ha trecho.** — `triagem_e1_v14b.txt:3`

4. **Outro componente fica fora sem recusa: (c), fora do alcance da árvore perguntada.** A pergunta identifica `5e55c211`; a eleição de um único componente aparece na triagem como cura da volta 14b. O laudo posterior aponta o residual dessa cura. — `codex_e1_v14b.q:1`; `triagem_e1_v14b.txt:4`; `opus_final2_e1_6695e972.md:8`

   **Pergunta relacionada:** “e o universo que a porta (universo_do_fato) elege e exatamente o universo_da_linha de seus membros?” — `codex_e1_v14b.q:7`

   **Resposta sobre a implementação anterior:** “o fecho preserva todas as sementes, sem exigir conectividade entre elas.” — `codex_e1_v14b.r:29`

   **Registro da mudança:** “CURADO na volta 14b: fio.py::universo_do_fato elege UM componente (o da canonica_do_grupo das sementes)”. — `triagem_e1_v14b.txt:4`

   **Pergunta ou resposta sobre o residual específico da nova escolha — componente omitido sem recusa: nao ha trecho.** Esse residual consta do laudo posterior: “o outro nao entra em recusadas nem fecho_cruza_dias, nenhum contador o ve”. — `opus_final2_e1_6695e972.md:8`

5. **Custo dos juízes: (b), visto e descartado como bloqueio, mantido em fila.** — `codex_e1_v14b.r:59,66,72`; `triagem_e1_v14b.txt:9`

   **Pergunta:** “quantas chamadas ao juiz por abrir, por fecho, por fecho_cruza_dias? Memo por batida basta?” — `codex_e1_v14b.q:10`

   **Resposta:** “Zero com busca vazia; caso contrário, B no universo/ponte/fecho compartilhado.” Também: “**Memo por batida não basta para compartilhar trabalho por turno/janela**”. — `codex_e1_v14b.r:66,72`

   **Motivo escrito na triagem:** “descartado como bloqueio -- nenhum selo de teto vermelho na suite de 14300; fila (medir na frota).” A medição na frota permanece **[nao verificado]**. — `triagem_e1_v14b.txt:9` [nao verificado]

## 08/11 11:04 · sessao b5a11839aaae · PERGUNTA-SEM-SELO · censo · origem=Code

**A medida antiga precisa de correção:** S1 recebe hoje a contagem como parâmetro; S3 mantém consulta crua para idempotência, mas o julgamento já verifica a retratação no retorno. Permanecem consultas cruas em S2, S4, S5 e S6. (`app/ponto/supra_juiz.py:207`; `app/chamados/juizes.py:235`; `app/chamados/juizes.py:775`; `app/ponto/services/veredito_celula.py:215`; `app/chamados/services/materializacao.py:94`; `app/ponto/services/flip_auto.py:35`; `app/ponto/utils.py:244`.)

**1. S1–S6 nesta cópia**

| Sítio | Universo atual e consequência |
|---|---|
| **S1 — `classificar_dia`** | Não monta universo: recebe `batidas_no_dia` e usa essa contagem no fallback. O comando `supra_juiz` carrega cru, conta e entrega esse insumo; `emitir_furo_retroativo` faz o mesmo. O cartório usa células da grade e órfãs. (`app/ponto/supra_juiz.py:207`; `app/ponto/supra_juiz.py:513`; `app/ponto/management/commands/supra_juiz.py:113`; `app/ponto/management/commands/supra_juiz.py:222`; `app/ponto/management/commands/supra_juiz.py:229`; `app/ponto/management/commands/emitir_furo_retroativo.py:53`; `app/ponto/management/commands/emitir_furo_retroativo.py:100`; `app/ponto/services/cartorio.py:1373`; `app/ponto/services/cartorio.py:1419`.) |
| **S2 — `veredito_celula`** | **Cru-JULGAMENTO.** `reais` entra em `grade_dia` e decide se o dia já está completo. `grade_dia` encaminha a lista recebida à grade. (`app/ponto/services/veredito_celula.py:215`; `app/ponto/services/veredito_celula.py:220`; `app/ponto/services/veredito_celula.py:223`; `app/ponto/services/simular_dia.py:5`.) |
| **S3 — `_batida_existente_na_janela`** | **Consulta crua de idempotência.** O julgamento `_pergunta_materializada_de_fato` já exige `retratada_em is None` no retorno. A justificativa para preservar o cru no plantio está escrita: impedir replantio da batida retratada. A cura é local ao julgamento, não ao helper inteiro. (`app/chamados/juizes.py:227`; `app/chamados/juizes.py:235`; `app/chamados/juizes.py:768`; `app/chamados/juizes.py:774`; `app/chamados/juizes.py:775`.) |
| **S4 — `_vizinha_mesmo_tipo`** | O código correspondente está em **`chamados/services/materializacao.py`**: duas consultas cruas decidem bloqueio de materialização por paridade. (`app/chamados/services/materializacao.py:85`; `app/chamados/services/materializacao.py:94`; `app/chamados/services/materializacao.py:100`; `app/chamados/services/materializacao.py:451`; `app/chamados/services/materializacao.py:850`.) |
| **S5 — `pares_mesmo_tipo`** | **Cru-JULGAMENTO.** A consulta crua define adjacência e pares que alimentam a fila de flip. A consulta posterior a `batidas_do_dia_turno` não altera esse universo inicial. (`app/ponto/services/flip_auto.py:35`; `app/ponto/services/flip_auto.py:41`; `app/ponto/services/flip_auto.py:104`; `app/ponto/services/flip_auto.py:146`.) |
| **S6 — `proxima_batida_de`** | **Cru-JULGAMENTO na função:** última marca crua determina o próximo tipo. A autoridade registrada para próximo tipo é `turnos.proximo_tipo_de`. Chamadores dinâmicos de `proxima_batida_de`: **[não verificado]**. (`app/ponto/utils.py:240`; `app/ponto/utils.py:244`; `app/ponto/utils.py:246`; `app/core/juizes.py:197`.) |

Commit de cura: **[não verificado]**, pois esta cópia não oferece histórico Git consultável. A prova atual de cura do julgamento de S3 é `app/chamados/juizes.py:775`.

Há outro uso de S3 que merece separação: a apresentação declara `ja_satisfeita` verificando somente `aprovada`; o plantio reutiliza qualquer existente. Isso não equivale à verificação explícita de retratação feita pelo juiz. (`app/chamados/services/materializacao.py:446`; `app/chamados/services/materializacao.py:447`; `app/chamados/services/materializacao.py:834`; `app/chamados/services/materializacao.py:840`; `app/chamados/juizes.py:775`.)

**2. Censo AST**

Unidade do inventário: uma raiz de consulta persistente ou uma chamada à porta; encadeamentos contam uma vez. São **154 raízes de consulta + 47 chamadas à porta = 201 sítios**, discriminados abaixo. A implementação da própria porta entra em **(a)**. Os três acessos por manager relacionado estão em `app/chamados/juizes.py:720`, `app/chamados/services/vigia_carimbo.py:113` e `app/ponto/management/commands/reconciliar_grade.py:118`.

| Classe | Total do inventário abaixo |
|---|---:|
| **a — porta**, incluindo sua implementação | **48** |
| **b — `retratada_em` explícito**, inclusive verificação em memória | **51** |
| **c — cru-exibição** de marcação, histórico ou indicadores da marcação | **22** |
| **d — cru-JULGAMENTO** | **28** |
| **e — cru-lookup**, identidade ou idempotência de escrita | **27** |
| **f — indeterminado na taxonomia**, com motivo indicado | **25** |
| **Total** | **201** |

Ocorrências executáveis de `.raw()` ou SQL direto de `ponto_batida` nesse recorte: **nenhuma encontrada pela varredura**; SQL construído dinamicamente e acessos externos à cópia: **[não verificado]**.

**(a) Porta — 48 sítios**

```text
app/api/views.py:2026 api_justificativas
app/chamados/juizes.py:1462 via_pelo_fato
app/chamados/services/validacao.py:828 classe_do_lote
app/colaboradores/services/detalhe.py:49 contexto_detalhe
app/core/services/painel_op.py:83 montar_painel_operacional
app/core/services/painel_op.py:269 montar_painel_operacional
app/core/views.py:114 _batidas_hoje
app/escala/detector_proposta.py:56 medir_padrao
app/escala/regua_defesa.py:153 diagnostico_regua
app/escala/regua_defesa.py:520 entradas_fora_do_inicio
app/escala/services/fase_na_tela.py:47 _trabalhados
app/escala/services/previsto_cego.py:159 medir
app/escala/services/semana_do_posto.py:117 _bateram
app/escala/utils.py:1505 montar_grade_prevista_periodo_por_turno
app/folha/export.py:499 dias_ferias_com_batida
app/folha/export.py:906 classificar_export
app/folha/previa.py:58 montar_previa
app/pautas/services.py:420 contexto_do_dia_texto
app/ponto/management/commands/aplicar_09_corte_b.py:126 _tem_saida_real_em_turno_aberto
app/ponto/management/commands/censo_col900.py:128 handle
app/ponto/management/commands/diff_espelho_alimentacao.py:92 handle
app/ponto/management/commands/e6_oraculo.py:152 __init__
app/ponto/management/commands/e6_oraculo.py:212 batida_fora_de_turno
app/ponto/management/commands/mapa_divergencia.py:96 handle
app/ponto/management/commands/reconciliar_fantasmas.py:96 _completude
app/ponto/management/commands/tripwire_celulas.py:283 medir_sobra_com_tipo_trocado
app/ponto/management/commands/vigiar_motor_autoridade.py:54 handle
app/ponto/services/abriu_nao_bateu.py:174 _bateu_no_dia
app/ponto/services/cartorio.py:1123 _julgar_colab_corpo
app/ponto/services/cobertura.py:299 declarar_cobertura_passada
app/ponto/services/deslogados.py:172 apurar
app/ponto/services/dia_do_colab.py:60 dia_do_colab
app/ponto/services/diagnostico_escala.py:142 diagnostico_escala
app/ponto/services/dias_em_aberto.py:122 passivo_da_competencia
app/ponto/services/esmeril_espelho.py:156 assinaturas_do_colab
app/ponto/services/espelho.py:1022 autoridade_do_periodo
app/ponto/services/faltas_de_hoje.py:57 faltas_de_hoje_sem_cobranca
app/ponto/services/fechamento.py:633 recalcular_fechamento_mes
app/ponto/services/fechamento.py:811 recalcular_fechamento_mes
app/ponto/services/triagem_batida.py:175 triagem_bater_ponto
app/ponto/services/vigia_de_hora.py:76 _acesas_de_sobra
app/ponto/turnos.py:2864 batidas_apuraveis — implementação
app/ponto/turnos.py:2962 _ultima_de
app/ponto/turnos.py:2982 turnos
app/ponto/utils_turno_orfao.py:28 detectar_bloqueio_turno_orfao
app/relatorios/management/commands/diff_extrato_parcial.py:37 _antes
app/relatorios/pdf_espelho.py:272 _coletar_dados_espelho
app/relatorios/views.py:502 extrato_parcial
```

**(b) Retratação explícita — 51 sítios**

```text
app/api/views.py:703 api_bater_ponto
app/api/views_core.py:700 api_bater_ponto
app/chamados/juizes.py:720 _pergunta_materializada_de_fato
app/chamados/management/commands/carimbar_por_batida_coincidente.py:70 handle
app/chamados/reconciliador.py:206 _par_integro — rejeição em memória na linha 216
app/chamados/services/adesao.py:60 sincronizar_adesao
app/chamados/services/adesao.py:144 sincronizar_adesao
app/chamados/services/desvio_escala.py:60 medir_desvio
app/chamados/services/fila_por_causa.py:190 particionar
app/chamados/services/limbo_folgas.py:35 sugerir_folga
app/chamados/services/limbo_folgas.py:155 semana_muda
app/chamados/services/materializacao.py:616 perguntas_absorvidas_por
app/chamados/services/vigia_carimbo.py:113 classificar
app/chamados/veredito_lavrado.py:140 marcos_do_lote
app/colaboradores/management/commands/corrigir_escala_retroativa.py:94 _rodar
app/colaboradores/services/calendario.py:239 contexto_calendario
app/colaboradores/services/ficha.py:145 ficha_do_colab
app/colaboradores/services/pos_vinculo.py:38 fechar_adesao_com_batida
app/colaboradores/services/senha_provisoria.py:24 senhas_provisorias
app/core/services/painel_op.py:92 montar_painel_operacional
app/core/views.py:173 dashboard
app/core/views.py:371 dashboard_supervisor
app/core/views.py:775 painel_tv
app/core/views.py:1177 dashboard_dados
app/escala/services/meio_periodo.py:158 observado_meio_periodo
app/escala/services/propositor.py:477 propostas
app/ferias/situacao.py:277 situacao_de_ferias
app/ponto/management/commands/diff_calculador.py:270 handle
app/ponto/management/commands/diff_janela_he.py:93 handle
app/ponto/management/commands/e6_piso_autopsia.py:71 handle
app/ponto/management/commands/marcar_foto_ausente_retro.py:66 handle
app/ponto/registro_batida.py:107 registrar_batida
app/ponto/registro_batida.py:139 registrar_batida
app/ponto/services/adesao.py:34 primeira_batida
app/ponto/services/adesao.py:58 ids_com_adesao
app/ponto/services/cartorio.py:1154 _julgar_colab_corpo
app/ponto/services/deslogados.py:161 apurar
app/ponto/services/deslogados.py:174 apurar
app/ponto/services/faltas_de_hoje.py:55 faltas_de_hoje_sem_cobranca
app/ponto/services/flip_auto.py:226 etiquetas_pela_celula — exclusão em memória na linha 229
app/ponto/services/lastro.py:261 julgar
app/ponto/services/passivo_s84.py:53 vivas
app/ponto/services/piso_intervalo.py:39 pares_abaixo_do_piso
app/ponto/turnos.py:1599 papel_por_minuto_da_ata
app/ponto/turnos.py:1861 turnos_do_colab
app/ponto/turnos.py:1899 _ultima_bat
app/ponto/turnos.py:2217 _turno_aberto_calc
app/ponto/turnos.py:2350 turnos_abertos_de
app/ponto/turnos.py:2382 sequencia_relogio_x_jornada
app/ponto/turnos.py:2466 proximo_tipo_de
app/ponto/turnos.py:2903 dia_das_batidas
```

**Duas ressalvas sobre (b):** calendário seleciona explicitamente as **retratadas para exibição**; `marcos_do_lote` seleciona o instante de retratação para invalidar lavra. Portanto, “tem `retratada_em` explícito” não significa automaticamente “consulta de apuráveis”. (`app/colaboradores/services/calendario.py:239`; `app/colaboradores/services/calendario.py:619`; `app/chamados/veredito_lavrado.py:140`.)

**(c) Cru-exibição — 22 sítios**

```text
app/api/views.py:297 api_me
app/api/views_core.py:286 api_me
app/chamados/services/acoes_chamado.py:377 montar_contexto_detalhe
app/chamados/views.py:692 fio_colaborador
app/colaboradores/painel_cell.py:33 montar_linha_colab
app/colaboradores/services/detalhe.py:20 contexto_detalhe
app/colaboradores/services/drawer.py:34 contexto_drawer
app/colaboradores/services/situacional.py:137 montar_situacional
app/colaboradores/views.py:53 vincular_colaboradores — indicador de marcação com GPS
app/colaboradores/views.py:337 vincular_posto — indicador tem_geo
app/colaboradores/views.py:356 vincular_posto — indicador tem_geo
app/colaboradores/views.py:395 _render_row — indicador tem_geo
app/colaboradores/views.py:928 painel_colaborador — total de registros
app/colaboradores/views.py:931 painel_colaborador — registros do mês
app/core/cell.py:10 cell_ultimas_batidas
app/core/views.py:194 dashboard — últimas marcações
app/core/views.py:219 dashboard — total de registros
app/core/views.py:991 batida_detalhe_json — histórico
app/core/views.py:1163 dashboard_dados — feed de marcações
app/ponto/views.py:191 espelho_ponto — últimas seis
app/ponto/views.py:441 batidas_offline_pendentes — fila de marcações
app/relatorios/services.py:230 gerar_pdf_espelho_lote_bytes — informação de movimento
```

O drawer calcula presença usando turnos, separadamente da lista crua exibida; o situacional também separa a última marca exibida do julgamento de uso/adesão. (`app/colaboradores/services/drawer.py:34`; `app/colaboradores/services/drawer.py:45`; `app/colaboradores/services/drawer.py:58`; `app/colaboradores/services/situacional.py:146`; `app/colaboradores/services/situacional.py:150`.)

**(d) Cru-JULGAMENTO — 28 sítios**

| Arquivo:linha | Função | O que o resultado decide |
|---|---|---|
| `app/chamados/management/commands/auditar_disputas.py:103` | `handle` | Qualificação de tipo invertido no relatório. |
| `app/chamados/services/explicador.py:40` | `_explicar_validada` | Existência/tipo da prova de materialização. |
| `app/chamados/services/explicador.py:53` | `_explicar_validada` | Diagnóstico e encaminhamento para cadastro. |
| `app/chamados/services/explicador.py:132` | `por_que_nao_fecha` | Alimentação desses diagnósticos em lote. |
| `app/chamados/services/materializacao.py:94` | `_vizinha_mesmo_tipo` | Vizinha anterior que barra plantio. |
| `app/chamados/services/materializacao.py:100` | `_vizinha_mesmo_tipo` | Vizinha posterior que barra plantio. |
| `app/core/views.py:360` | `dashboard_supervisor` | Entrada usada para excluir alguém dos ausentes. |
| `app/core/views.py:793` | `painel_tv` | Atividade que entra na classificação de cobertura. |
| `app/escala/services/furos_diarios.py:261` | `universo_empresa` | Nunca bateu/sumiu e última atividade. |
| `app/escala/services/furos_diarios.py:267` | `universo_empresa` | Escala versus realidade no período. |
| `app/ponto/management/commands/detectar_par_relampago.py:205` | `_alvo_retratavel` | Guardas e escolha da retratação. |
| `app/ponto/management/commands/detectar_par_relampago.py:290` | `varrer_pares_embutidos` | Adjacência e pares candidatos. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:53` | `handle` | Universo contado para classificar dia. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:74` | `handle` | Piso de adesão usado no julgamento. |
| `app/ponto/management/commands/reconciliar_grade.py:84` | `handle` | Seleção do universo de colaboradores. |
| `app/ponto/management/commands/reconciliar_grade.py:88` | `handle` | Piso de adesão que suprime dias. |
| `app/ponto/management/commands/reconciliar_grade.py:118` | `handle` | `c.batidas.all()` alimenta grade/missing. |
| `app/ponto/management/commands/supra_juiz.py:113` | `handle` | Universo contado para classificar dia. |
| `app/ponto/management/commands/supra_juiz.py:132` | `handle` | Adesão entregue ao classificador. |
| `app/ponto/propositor.py:346` | `dia_posicional_completo` | Completude posicional do dia. |
| `app/ponto/services/flip_auto.py:35` | `pares_mesmo_tipo` | Pares que alimentam fila de flip. |
| `app/ponto/services/flip_auto.py:291` | `avaliar_fila` | Dia perfeito antes/depois do flip. |
| `app/ponto/services/geofence.py:51` | `vinculo_contagem` | Evidência que julga divergência de vínculo. |
| `app/ponto/services/veredito_celula.py:215` | `veredito_celula` | Completude, flips e materialização. |
| `app/ponto/services/vinculo_na_lista.py:49` | `achar` | Classificação de evidência de vínculo/posto. |
| `app/ponto/utils.py:186` | `colabs_disponiveis_para_cobertura` | Exclusão de candidatos por suposto turno aberto. |
| `app/ponto/utils.py:244` | `proxima_batida_de` | Próximo tipo de batida. |
| `app/ponto/views.py:1598` | `_abrir_justificativa_fluxo` | Entrada vinculada à justificativa. |

Em `avaliar_fila`, excluir retratadas somente da lista de flips **não protege o julgamento**: `reais` continua inteira em `grade_dia`. (`app/ponto/services/flip_auto.py:291`; `app/ponto/services/flip_auto.py:298`; `app/ponto/services/flip_auto.py:303`.)

Os detectores de par filtram `aprovada=True`, mas isso não é o filtro legal de retratação: o modelo define `aprovada` como crivo do DP. Efeito concreto disso nos registros de produção: **[não verificado]**. (`app/ponto/management/commands/detectar_par_relampago.py:205`; `app/ponto/management/commands/detectar_par_relampago.py:290`; `app/ponto/models.py:66`.)

**(e) Cru-lookup/identidade/idempotência — 27 sítios**

```text
app/api/views.py:559 api_bater_ponto — idempotência
app/api/views.py:856 api_upload_foto — pk
app/api/views_core.py:551 api_bater_ponto — idempotência
app/api/views_core.py:846 api_upload_foto — pk
app/chamados/detector_anomalias.py:43 detectar_anomalias — pk
app/chamados/juizes.py:49 _dia_da_batida — pk
app/chamados/juizes.py:194 dias_do_fato — pk__in
app/chamados/juizes.py:235 _batida_existente_na_janela — idempotência compartilhada
app/chamados/services/acoes_chamado.py:262 acao_recusar_geofence — pk
app/chamados/services/passivo_validado.py:513 _plantar_o_dia — marcador de pk anterior à escrita
app/chamados/services/passivo_validado.py:527 _plantar_o_dia — identificação do que a escrita criou
app/chamados/services/regularizacao_ext.py:73 _cria_batida — chave colaborador/instante
app/chamados/views_regularizacao.py:109 regularizacao_add_item — pk
app/core/views.py:978 batida_detalhe_json — pk
app/ponto/management/commands/diff_passivo_s84.py:79 _mutacao — pk
app/ponto/management/commands/flip_automatico.py:44 handle — pk__in
app/ponto/management/commands/passivo_disputa_retro.py:158 handle — pk
app/ponto/registro_batida.py:120 registrar_batida — chave colaborador/instante
app/ponto/services/flip_auto.py:135 fila_flip_recusas — pk
app/ponto/services/flip_auto.py:272 avaliar_fila — pk
app/ponto/services/geofence.py:406 reavaliar_chamados_geofence — pk__in
app/ponto/services/regularizacao.py:52 regularizar_turno_orfao — pk
app/ponto/services/triagem_batida.py:257 ramo_regularizar_saida — pk
app/ponto/services/triagem_batida.py:670 ramo_registrar_es — idempotência
app/ponto/services/triagem_batida.py:722 ramo_registrar_es — identificação da oposta na resposta
app/ponto/views.py:456 batidas_offline_pendentes — pk
app/ponto/views.py:2899 salvar_foto_batida — id
```

O cru por chave exata em `registrar_batida` é deliberado: reenviar a mesma chave deve recuperar inclusive a marca retratada, evitando colisão com a constraint. (`app/ponto/registro_batida.py:116`; `app/ponto/registro_batida.py:120`.)

**(f) Indeterminado na taxonomia — 25 sítios, com motivo**

Aqui “indeterminado” significa que a leitura não cabe integralmente em exibição de marcação, julgamento, porta ou lookup de escrita; o uso observado está declarado em cada linha.

| Arquivo:linha | Função | Motivo |
|---|---|---|
| `app/api/hora_do_aparelho.py:173` | `descartadas` | Auditoria da diferença entre relógio do aparelho e timestamp gravado. |
| `app/chamados/veredito_lavrado.py:137` | `marcos_do_lote` | Carimbo de criação para invalidar lavra; não conta trabalho. |
| `app/core/media_protegida.py:31` | `servir_media_protegida` | Autorização de acesso à foto. |
| `app/core/views_config.py:528` | `saude_sistema` | Contagem física de saúde do sistema. |
| `app/escala/management/commands/hub_smoke.py:98` | `win_map` | Foto antes/depois para detectar mutação durante smoke. |
| `app/ponto/management/commands/censo_col900.py:133` | `handle` | Diagnóstico de existência crua quando a porta não encontrou marca. |
| `app/ponto/management/commands/checklist_golive.py:100` | `handle` | Contagem física global. |
| `app/ponto/management/commands/checklist_golive.py:101` | `handle` | Contagem para alerta sobre dados de teste. |
| `app/ponto/management/commands/gerar_batidas_smoke_motor.py:61` | `_limpar` | Seleção para limpeza de ensaio. |
| `app/ponto/management/commands/mapa_divergencia.py:62` | `handle` | Seleção ampla de candidatos; comparação carrega pela porta. |
| `app/ponto/management/commands/processar_alertas_turno.py:59` | `handle` | Seleção para disparar `recompute_turnos`; não entreg

[truncado em 20000 de 32584 caracteres]

## 08/11 11:04 · sessao 92c5caf6b911 · COBPARC-AUSENCIA-CALA-DOIS-JUIZES · sonda · origem=Code

1. Contagens por classe, competência e empresa:

| Classe | Competência | Empresa | Contagem | Entregue K6 | Trancada K4 | Referência |
|---|---|---:|---:|---|---|---|
| carimbo_sem_periodo | 10/2026 | 2 | 14 | False | False | [nao verificado] |
| carimbo_sem_periodo | 11/2026 | 2 | 4 | False | False | [nao verificado] |
| carimbo_sem_periodo | 10/2026 | 4 | 2 | False | False | [nao verificado] |
| carimbo_sem_periodo | 11/2026 | 4 | 1 | False | False | [nao verificado] |
| sem_carimbo | 08/2026 | 2 | 18 | True | True | [nao verificado] |
| sem_carimbo | 09/2026 | 2 | 27 | True | False | [nao verificado] |
| sem_carimbo | 08/2026 | 3 | 1 | False | True | [nao verificado] |
| sem_carimbo | 09/2026 | 3 | 5 | True | False | [nao verificado] |
| sem_carimbo | 08/2026 | 4 | 5 | False | True | [nao verificado] |
| sem_carimbo | 09/2026 | 4 | 5 | True | False | [nao verificado] |
| sem_celula | 07/2026 | 2 | 1 | False | False | [nao verificado] |

2. Colab-dias por classe, considerando “aberta” como `entregue=False` em K6. [nao verificado]

| Classe | Entregue/exportada | Aberta | Total | Referência |
|---|---:|---:|---:|---|
| carimbo_sem_periodo | 0 | 21 | 21 | [nao verificado] |
| sem_carimbo | 55 | 6 | 61 | [nao verificado] |
| sem_celula | 0 | 1 | 1 | [nao verificado] |
| **Total** | **55** | **28** | **83** | [nao verificado] |

3. Nenhuma linha indicando erro ou traceback no trecho fornecido. [nao verificado]

As referências estão marcadas assim porque o dado foi fornecido no texto, sem arquivo:linha. [nao verificado]

_2881 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
