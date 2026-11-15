# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 23:47** · publicado em **14/11/2026 23:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 23:47 · sessao 9f9127827fdf · E3-RESTO-R3 · sonda · origem=Code

Pelo recorte explícito `ninguem_fora_hoje` da sonda fornecida: [nao verificado]

| Competência | Linhas para “Sem ação disponível” | Com ações publicadas | Sem ações publicadas | Legado sem `dono` gravado | Fonte | [nao verificado]
|---|---:|---:|---:|---:|---|
| 10/2026 | 83 | 0 | 83 | 0 | Sonda fornecida [nao verificado] |
| 11/2026 | 76 | 0 | 76 | 0 | Sonda fornecida [nao verificado] |
| **Total** | **159** | **0** | **159** | **0** | Soma da sonda [nao verificado] |

Em novembro, usei diretamente os **76 de `ninguem_fora_hoje`**, sem descontar novamente o `hoje=1`. Todas essas 159 linhas têm `dono` gravado; nenhuma é legado **sem dono**, embora nenhuma tenha ações publicadas no retrato fornecido. [nao verificado] [nao verificado]

O código encaminha `ninguem` para `sem_acao_disponivel` e aplica antes a exceção de `hoje`, quando todas as datas são de hoje em diante. (`app/folha/retrato_folha.py:193`, `app/folha/retrato_folha.py:480`)

**A linha do Retrato não guarda `nao_sei`.** O dicionário construído por `montar_linhas` grava identificação, status, motivo, dias/datas, dono, ações, tipo, saldo e informativo, sem esse campo. (`app/folha/retrato_folha.py:1206`)

O caminho da perda é:

1. **Tarefa:** conserva `nao_sei` como informação própria; por exemplo, marca desconhecimento quando falta célula. (`app/chamados/services/tarefa_do_dia.py:202`, `app/chamados/services/tarefa_do_dia.py:224`)
2. **Porta:** `porta_da_tarefa` inclui `nao_sei` quando existe rota. Sem rota, devolve `None`; nessa projeção, o desconhecimento deixa de acompanhar a porta, embora permaneça na tarefa. (`app/chamados/services/tarefa_do_dia.py:686`)
3. **`fora_da_folha`:** lê somente a porta; `None` vira `FIO_SEM_PORTA='nada'`. Ao agregar por pessoa, transporta ações, grupo, datas e contagens, sem transportar `nao_sei`, inclusive quando ele veio numa porta existente. (`app/folha/fora_da_folha.py:70`, `app/folha/fora_da_folha.py:85`, `app/chamados/juizes.py:1652`, `app/folha/fora_da_folha.py:193`, `app/folha/fora_da_folha.py:204`)
4. **Retrato:** transforma o grupo em fio gravado e copia as ações, sem recuperar a marca perdida. (`app/folha/retrato_folha.py:1215`)

**O leitor que distingue os casos é `o_que_segura`:** mantém a tarefa e a porta em mãos e devolve `nao_sei` da porta quando ela existe, ou da tarefa quando a porta é `None`. (`app/folha/fora_da_folha.py:242`, `app/folha/fora_da_folha.py:256`)

Portanto, **quantas dessas 159 linhas envolvem desconhecimento do dia: [nao verificado]**. O retrato fornecido não permite essa separação. [nao verificado]

“Zero ações publicadas” também não prova zero links de navegação: `portas_da_linha` pode acrescentar portas pelo fio ou pelo tipo. (`app/folha/retrato_folha.py:1027`, `app/folha/retrato_folha.py:1040`)

## 14/11 23:43 · sessao 48c2552776f8 · E3-RESTO-R3 · verificador · origem=Code

1. **ESMERIL-SEMÂNTICA: há leitores incompatíveis com a cura — gravidade média.**

   - O selo ainda exige **5** bloqueados em Admin, mas a fixture real agora produz **4**. Reproduzi a contagem em memória, sem banco; outra asserção do mesmo arquivo já foi atualizada para 4. A suíte contém uma contradição determinística (`app/folha/tests/test_retrato_folha_quadros.py:1250`, `app/folha/tests/test_retrato_folha_quadros.py:710`, `app/folha/retrato_folha.py:193`).
   - A golden ainda exige Admin=2 para uma população declarada como `{admin:1, ninguem:1}`; com o mapa novo resulta Admin=1 e Sem ação disponível=1. O teste seguinte exige que o balde permaneça igual quando `ninguem` vira `chamado` ou `admin`, contrariando LU-05 (`app/folha/tests/test_haiku_fora_da_folha.py:499`, `app/folha/tests/test_haiku_fora_da_folha.py:516`, `app/folha/tests/test_haiku_fora_da_folha.py:521`, `app/folha/tests/test_haiku_fora_da_folha.py:559`, `app/folha/tests/test_haiku_fora_da_folha.py:580`).

   Inventário dos símbolos, com endereços relativos à raiz:

   | Símbolo | Quem escreve/define | Quem lê |
   |---|---|---|
   | `DONOS` | `app/folha/retrato_folha.py:190` | Inversa `:197`; destino do LED `:555`; valores de filtro `:581`; barra, indiretamente, `:901` |
   | `DONO_DO_FIO` | `app/folha/retrato_folha.py:193` | Inversa `:197`; classificação da linha `:484`; golden externa `app/folha/tests/test_haiku_fora_da_folha.py:499` |
   | `FIOS_DO_DONO` | Derivada em `app/folha/retrato_folha.py:197` | Tooltip `:72`; selos `app/folha/tests/test_retrato_folha_quadros.py:1273` |
   | `ROTULO_DO_DONO` | `app/folha/retrato_folha.py:198` | Tabela dos LEDs `:306`; rótulos dos filtros `:588`; drawer `:1092` |
   | `LED_DA_LINHA` | `app/folha/retrato_folha.py:302` | Estados/cores/rótulos `:311`, `:312`, `:313`; legenda `:1117` |
   | `dono_da_linha` | Função em `app/folha/retrato_folha.py:460`; devolve balde, sem persistência `:484` | LED `:523`; recorte/agrupamento `:579`; drawer `:1084`; vigia `app/folha/vigia_retido_calado.py:48` |
   | `censo` | Monta o retorno em `app/folha/vigia_retido_calado.py:45`, `:55`, `:58` | Serializador `:64`, `:67`; command `app/folha/management/commands/vigiar_retido_calado.py:31`, `:36` |

   O campo persistido `dono` continua recebendo **fio**, pelo montador, e é salvo pelo lavrador; o balde permanece uma projeção de leitura (`app/folha/retrato_folha.py:1215`, `app/folha/management/commands/lavrar_retrato_folha.py:78`).

   Os leitores de produção examinados acompanham a projeção: lista, barra, legenda, PDF, Raio-X e drawer (`app/folha/views_retrato.py:280`, `app/folha/views_retrato.py:318`, `app/folha/views_retrato.py:325`, `app/folha/views_retrato.py:411`, `app/folha/views_retrato.py:494`, `app/colaboradores/views.py:986`, `app/colaboradores/views.py:988`). Os contadores gerais continuam contando os três filtros da Folha, sem usar `DONOS` (`app/folha/retrato_folha.py:120`).

   Fora do diff, também há leitores nos selos de drawer, legenda, integração do colaborador e vigia; suas leituras dinâmicas acompanham os mapas, ressalvadas as expectativas antigas apontadas acima (`app/folha/tests/test_retrato_folha_drawer.py:190`, `app/folha/tests/test_retrato_folha_drawer.py:235`, `app/folha/tests/test_retrato_folha_legenda_na_barra.py:107`, `app/colaboradores/tests/test_retrato_no_drawer.py:206`, `app/colaboradores/tests/test_retrato_no_drawer.py:223`, `app/folha/tests/test_vigia_retido_calado.py:29`, `app/folha/tests/test_vigia_retido_calado.py:88`).

   **Mapa próprio:** nenhum segundo mapa de produção identificado nos caminhos examinados; os testes esquecidos conservam a associação antiga por expectativas literais (`app/folha/retrato_folha.py:193`, `app/folha/tests/test_haiku_fora_da_folha.py:516`, `app/folha/tests/test_retrato_folha_quadros.py:1250`).

2. **Vigia: o número mudou de significado; seus textos não acompanharam — gravidade baixa.**

   Antes, selecionava todo bloqueado com fio `ninguem`; depois, seleciona somente o balde `sem_acao_disponivel`. Portanto, linhas cujas datas são todas hoje/futuro deixam **tanto `calados` quanto `roteados`**, permanecendo em `bloqueados` (`diff_c4.patch`, anexo fornecido `[nao verificado: numeração]`; `app/folha/vigia_retido_calado.py:48`, `app/folha/vigia_retido_calado.py:49`, `app/folha/vigia_retido_calado.py:52`, `app/folha/retrato_folha.py:482`).

   O universo continua sendo os retratos do último **dia local de lavra** de cada empresa. A unidade efetiva é **pessoa por retrato/competência**: a mesma pessoa em duas competências conta duas vezes. C5 separa roteados de calados (`app/folha/vigia_retido_calado.py:39`, `app/folha/vigia_retido_calado.py:44`, `app/folha/vigia_retido_calado.py:46`, `app/folha/vigia_retido_calado.py:53`, `app/folha/vigia_retido_calado.py:54`).

   O serializador publica os números por competência e o total; o command usa `calados` no alarme passivo. O cron passa apenas `--alarme`, que cobra `cego`, sem cobrar `calados` (`app/folha/vigia_retido_calado.py:64`, `app/folha/vigia_retido_calado.py:67`, `app/folha/management/commands/vigiar_retido_calado.py:16`, `app/folha/management/commands/vigiar_retido_calado.py:35`, `app/config/crons.py:1680`).

   Entretanto, o help e a definição inicial ainda prometem contar pessoas com fio `ninguem`, omitindo a exclusão temporal nova (`app/folha/management/commands/vigiar_retido_calado.py:21`, `app/folha/vigia_retido_calado.py:4`, `app/config/crons.py:1677`).

   **ESTADO/placar:** não confirmei leitor deste número `[nao verificado]`. O contador de fio mostrado pelo gerador do ESTADO vem de outro vigia, `vigiar_fio_morto`; não comprova consumo de `retidos_calados` (`bin/gerar_estado.py:253`, `bin/gerar_estado.py:260`).

3. **Tela: nenhum erro confirmado de iteração ou tooltip; geometria `[nao verificado]`.**

   A barra percorre os valores publicados, e o partial percorre todos os itens; o vermelho já existe. Não há contagem fixa de três donos nesses loops (`app/folha/retrato_folha.py:898`, `app/folha/retrato_folha.py:901`, `app/templates/core/_barra_gestao.html:63`, `app/templates/core/_barra_gestao.html:67`).

   A legenda percorre todos os LEDs com quebra de linha; lista, Raio-X e drawer recebem os valores prontos. O JavaScript desses caminhos apenas abre o endereço recebido (`app/templates/folha/_retrato_legenda.html:26`, `app/templates/folha/_retrato_legenda.html:28`, `app/templates/folha/_retrato_lista.html:47`, `app/templates/folha/_retrato_raiox.html:67`, `app/templates/folha/_retrato_o_que_fazer.html:33`, `app/templates/folha/_retrato_o_que_fazer.html:73`).

   **Tooltip existe:** deriva `ninguem → D → GRUPOS['D']`; o texto é “tem dia acusando SEM fio nenhum” (`app/folha/retrato_folha.py:197`, `app/folha/retrato_folha.py:71`, `app/folha/fora_da_folha.py:42`).

   Isso não prova geometria: a fileira externa usa `nowrap`, enquanto o smoke conserva exigências de ausência de compressão/transbordo e declara esta volta não verificada (`app/templates/core/_barra_gestao.html:62`, `app/folha/tests/test_retrato_folha_smoke_chromium.py:4`, `app/folha/tests/test_retrato_folha_smoke_chromium.py:448`, `app/folha/tests/test_retrato_folha_smoke_chromium.py:456`). No PDF, o rótulo novo entra como string em coluna fixa de 26 mm; seu encaixe também é `[nao verificado]` (`app/folha/views_retrato.py:413`, `app/folha/views_retrato.py:425`).

4. **LU-05: `ninguem` não equivale estritamente a “dia sem ação disponível”.**

   - **Alta — desconhecimento vira negativa certa.** Falha da autoridade do calendário devolve `None`; a tarefa registra `nao_sei=True`, mas, sem rota, `porta_da_tarefa` retorna apenas `None`. A Folha converte isso em `nada`, grupo D, `ninguem` e finalmente “Sem ação disponível”. Reproduzi essa cadeia em memória: uma falha de conhecimento recebe o mesmo rótulo de ausência comprovada de ação (`app/colaboradores/services/calendario.py:142`, `app/chamados/services/tarefa_do_dia.py:614`, `app/chamados/services/tarefa_do_dia.py:646`, `app/chamados/services/tarefa_do_dia.py:686`, `app/folha/fora_da_folha.py:85`, `app/folha/retrato_folha.py:370`, `app/folha/retrato_folha.py:193`). Ocorrência em produção `[nao verificado]`.
   - **Média — pessoa no balde novo pode ter ação publicada.** Basta um dia sem porta para o grupo da pessoa ser D; o laço conserva ações dos demais dias. Caso concreto sintético: 12/11 com `resolver_dia`, 13/11 sem porta. O resultado é `ninguem`/“Sem ação disponível” **com botão Resolver dia**. O significado agregado é “há algum dia sem porta” (`app/folha/fora_da_folha.py:93`, `app/folha/fora_da_folha.py:192`, `app/folha/fora_da_folha.py:198`, `app/folha/retrato_folha.py:1215`, `app/folha/retrato_folha.py:1216`, `app/folha/retrato_folha.py:1039`). Ocorrência em produção `[nao verificado]`.
   - **Média — legado/ausência de dia também recebe a afirmação nova.** Grupo vazio produz `None`, que vira `ninguem`; retrato legado sem fio também usa esse fallback, sem consultar portas (`app/folha/fora_da_folha.py:91`, `app/folha/retrato_folha.py:370`, `app/folha/retrato_folha.py:443`). Quantidade afetada `[nao verificado]`.

   No universo estreito de **dias acusadores de retidos por furo**, uma porta publicada `None` entra como `nada` e força D. Fora desse universo, não existe equivalência global: não bloqueados recebem fio vazio e motivos diferentes de furo recebem `admin`, sem esse julgamento de porta (`app/folha/fora_da_folha.py:194`, `app/folha/fora_da_folha.py:201`, `app/folha/retrato_folha.py:366`, `app/folha/retrato_folha.py:368`).

5. **Classes de reprovação.**

   - **Média — novo caminho de relógio fora da borda no censo avulso.** `censo(hoje=None)` passa `None` para cada linha, que pode ler `localdate()` internamente. Em memória, duas linhas idênticas atravessando a meia-noite produziram um único calado e duas leituras de relógio. O command evita isso ao fornecer uma data fixa; o contrato avulso permanece exposto (`app/folha/vigia_retido_calado.py:28`, `app/folha/vigia_retido_calado.py:48`, `app/folha/retrato_folha.py:457`, `app/folha/management/commands/vigiar_retido_calado.py:31`).
   - **Mutantes mordem a cura:** executei os seis testes sem banco e os 11 mutantes; todos passaram no controle e os 11 morreram por asserção, sem erros de infraestrutura. Essa prova cobre as mutações enumeradas, mas não os casos de desconhecimento e dias mistos acima (`app/chamados/tests/e3_r3_c4_mutantes.py:19`, `app/chamados/tests/e3_r3_c4_mutantes.py:64`, `app/chamados/tests/e3_r3_c4_mutantes.py:80`, `app/chamados/tests/test_e3_r3_c4_sem_banco.py:104`). Reprodução do RED contra a base `[nao verificado]`.
   - **Segundo juiz, segundo escritor, alteração de trava ou pouso antecipado:** nenhum acréscimo identificado nos corpos alterados; a mudança funcional é projeção de baldes e seleção do vigia. A persistência continua no lavrador e a dependência do vigia continua depois da lavra (`app/folha/retrato_folha.py:193`, `app/folha/vigia_retido_calado.py:48`, `app/folha/management/commands/lavrar_retrato_folha.py:78`, `app/config/crons.py:1682`). Pouso efetivo `[nao verificado]`.

6. **Docstrings alteradas: nenhum comportamento falso adicional identificado.** Os memos existem, as fontes adicionais da rota são consultadas e `_posicoes` realmente devolve o dicionário da lâmpada (`app/chamados/services/tarefa_do_dia.py:545`, `app/chamados/services/tarefa_do_dia.py:589`, `app/chamados/services/tarefa_do_dia.py:592`, `app/chamados/services/tarefa_do_dia.py:85`). Permanecem as definições antigas do contador apontadas no item 2 (`app/folha/vigia_retido_calado.py:4`, `app/folha/management/commands/vigiar_retido_calado.py:21`).

**VEREDITO: REPROVA.**

## 14/11 23:41 · sessao 8ccf40c881f4 · - · - · origem=Ronald (tmux)

Achados:

1. **Média — selo existente contradiz a cura.** `app/folha/tests/test_retrato_folha_quadros.py:1250` exige **5** bloqueados sob `f=bloqueado&dono=admin`. A mesma fixture passou a produzir **4**, conforme novo mapa `app/folha/retrato_folha.py:193`; o próprio teste atualizado já espera 4 em `app/folha/tests/test_retrato_folha_quadros.py:710`. Executei sem BD o módulo real + `_l`/`LINHAS` reais extraídos por AST: resultado **4**. Logo a suíte contém falha determinística.

2. **Média — golden e teste de transição fora do diff continuam cobrando a partição antiga.** `app/folha/tests/test_haiku_fora_da_folha.py:496` inicializa os donos pelo eixo vivo e `:499` traduz pelo mapa vivo, mas `:516` exige Admin=2 para o caso com um fio admin e um ninguem (`:521`). Com LU-05, essa expectativa é Admin=1/Sem ação=1. O teste seguinte exige que o balde fique igual antes/depois de surgir chamado (`:559`) e porta admin (`:580`), embora a cura deva mover a pessoa de Sem ação para Admin. Execução com BD `[nao verificado]`; incompatibilidade das asserções demonstrada pelo código.

3. **Baixa — leitores documentais mantêm o significado antigo do contador.** O help promete “pessoas bloqueadas ... com fio ninguem” (`app/folha/management/commands/vigiar_retido_calado.py:21`) e a definição do módulo faz a mesma promessa (`app/folha/vigia_retido_calado.py:4`), enquanto o novo filtro exclui Hoje/futuro (`:48`, via `app/folha/retrato_folha.py:480`). O comentário do cron também mantém a definição antiga (`app/config/crons.py:1677`).

Inventário:

- **Escritores das constantes:** `app/folha/retrato_folha.py:190` DONOS; `:193` DONO_DO_FIO; `:197` FIOS_DO_DONO derivado; `:198` ROTULO_DO_DONO; `:302` LED_DA_LINHA. Em produção, não localizei outro escritor explícito desses nomes; mutações de teste em `app/folha/tests/test_retrato_folha_dono_sao_tres.py:117`, `app/folha/tests/test_retrato_folha_drawer.py:81`/`:235`.
- **Leitores:** tooltip lê FIOS_DO_DONO (`app/folha/retrato_folha.py:72`); dono lê DONO_DO_FIO (`:484`); LED deriva do dono (`:523`); derivados de LED (`:311`); URL do LED consulta DONOS (`:555`); filtros e rótulos publicam os mapas (`:579`, `:581`, `:588`); agrupamento (`:687`); barra itera valores publicados (`:898`); drawer lê dono/rótulo/cor (`:1084`, `:1092`); legenda itera LED_DA_LINHA (`:1117`).
- **Views/exportação:** tela lê desenho (`app/folha/views_retrato.py:280`), barra (`:318`) e legenda (`:325`); PDF lê barra (`:394`) e LED (`:411`); lente lê LED (`:494`). Drawer do colaborador chama `o_que_fazer` e legenda (`app/colaboradores/views.py:986`, `:988`). Esses caminhos acompanham o mapa único.
- **Contadores gerais:** `contadores` conta exclusivamente os três filtros de folha, sem ler DONOS (`app/folha/retrato_folha.py:120`); lavrador o consome (`app/folha/management/commands/lavrar_retrato_folha.py:72`). `placar_ontem_hoje` trata existência de fotos (`app/folha/retrato_folha.py:707`); lente o lê (`app/folha/views_retrato.py:515`).
- **Vigia antes/depois:** pelo diff fornecido, antes selecionava todos os bloqueados de fio ninguem; agora seleciona o balde Sem ação (`app/folha/vigia_retido_calado.py:48`). Ambos descontam C5 para roteados (`:49`). Portanto saem **de calados e de roteados** as linhas ninguem cujas datas são todas hoje/futuro; bloqueados não muda (`:52`). Universo: retratos do último dia de lavra de cada empresa (`:39`), unidade efetiva **pessoa por retrato/competência**, somada entre competências (`:46`, `:53`).
- **Leitores do vigia:** serialização lê calados/roteados (`app/folha/vigia_retido_calado.py:64`, `:67`); command lê para apresentação e alarme (`app/folha/management/commands/vigiar_retido_calado.py:31`, `:35`); cron chama somente `--alarme` (`app/config/crons.py:1680`), que alarma cego, sem incluir calados (`app/folha/management/commands/vigiar_retido_calado.py:16`). Leitor específico em ESTADO/placar `[nao verificado]`; não localizado nas buscas da cópia.

Mapa próprio fio→balde encontrado: os testes citados nos achados preservam expectativas antigas literais (`app/folha/tests/test_haiku_fora_da_folha.py:516`, `:559`; `app/folha/tests/test_retrato_folha_quadros.py:1250`). Nenhum segundo mapa de produção identificado nos leitores examinados; varredura de chamadas indiretas arbitrárias `[nao verificado]`.

## 14/11 23:40 · sessao cb4838ab95cd · - · - · origem=Ronald (tmux)

Achado **MÉDIO — testes fora do diff ainda exigem a partição antiga**: [nao verificado]

- `app/folha/tests/test_haiku_fora_da_folha.py:516` exige `Admin == 2`, embora o próprio teste declare `{admin: 1, ninguem: 1}` em `:521` e derive os baldes pelo mapa real em `:496-500`. Com `ninguem → sem_acao_disponivel` em `app/folha/retrato_folha.py:193`, o esperado passa a `Admin == 1`.
- O mesmo arquivo exige que o balde permaneça igual ao trocar `ninguem → chamado` (`:559`) e `ninguem → admin` (`:580`). Isso contradiz a nova partição em `app/folha/retrato_folha.py:193`. Execução integral desses testes com BD: `[nao verificado]`.

Tela: nenhum erro confirmado de iteração ou tooltip:

- Barra deriva valores de `DONOS` (`app/folha/retrato_folha.py:581`, `:893-901`) e o template itera todos os quadros/itens (`app/templates/core/_barra_gestao.html:63`, `:67`); vermelho já existe em `:67`.
- Legenda itera a lista completa, com `flex-wrap:wrap` (`app/templates/folha/_retrato_legenda.html:26-30`).
- Lista e Raio-X recebem LED pronto (`app/folha/views_retrato.py:280`, `:494`; `app/templates/folha/_retrato_lista.html:47`; `app/templates/folha/_retrato_raiox.html:67`).
- Drawer recebe rótulo e legenda do módulo (`app/colaboradores/views.py:986-988`) e os renderiza genericamente (`app/templates/folha/_retrato_o_que_fazer.html:33-36`, `:83`).
- Tooltip novo existe por `FIOS_DO_DONO → GRUPOS['D']` (`app/folha/retrato_folha.py:69-72`, `:197`; `app/folha/fora_da_folha.py:42`).
- Geometria efetiva nas três larguras: `[nao verificado]`; o próprio smoke registra essa limitação (`app/folha/tests/test_retrato_folha_smoke_chromium.py:4`). Seus testes dependem de BD e arquivos temporários (`:229-261`).
- PDF recebe o rótulo novo como string em coluna fixa de 26 mm (`app/folha/views_retrato.py:411-425`). Transbordo do texto: `[nao verificado]`; ReportLab não está disponível para medição local `[nao verificado]`.

Os seis testes sem banco passaram; abrangem agrupamento, tooltip, drawer e vigia, sem prova de geometria (`app/chamados/tests/test_e3_r3_c4_sem_banco.py:53`, `:69`, `:98`, `:134`).

## 14/11 23:40 · sessao da2fa83bcebb · - · - · origem=Ronald (tmux)

- **Alta — “Sem ação disponível” pode ter ação publicada.** O fio gravado é agregado por pessoa: basta um dia `nada` para `_grupo` produzir `D`, mesmo havendo outros dias `admin` (`app/folha/fora_da_folha.py:88-99`). O mesmo laço conserva as ações dos outros dias (`app/folha/fora_da_folha.py:192-205`); o lavrador grava `dono=ninguem` e essas ações juntas (`app/folha/retrato_folha.py:1215-1216`). Caso sintético executado em memória: pessoa com 12/11 `admin/resolver_dia` e 13/11 `nada` recebe `sem_acao_disponivel` e mantém botão “Resolver dia no calendário” (`app/folha/retrato_folha.py:480-484`, `app/folha/retrato_folha.py:1027-1039`). Portanto o balde significa “há algum dia sem porta”, não “esta pessoa não tem ação disponível”; ocorrência real em produção `[nao verificado]`.

- **Alta — desconhecimento vira “Sem ação disponível”.** Falha da autoridade de calendário produz `None` (`app/colaboradores/services/calendario.py:127-145`); a tarefa corretamente marca `nao_sei=True` (`app/chamados/services/tarefa_do_dia.py:614-615`). Sem outra ação/porta, escolhe fio `nao_sei` e rota `None` (`app/chamados/services/tarefa_do_dia.py:646-654`); `porta_da_tarefa` devolve apenas `None`, descartando o desconhecimento (`app/chamados/services/tarefa_do_dia.py:686`). A Folha converte isso em `nada`, depois grupo `D`, fio gravado `ninguem` e o novo balde afirmativo (`app/folha/fora_da_folha.py:82-85`, `app/folha/fora_da_folha.py:93-94`, `app/folha/retrato_folha.py:370`, `app/folha/retrato_folha.py:193`). Ocorrência real `[nao verificado]`.

- **Média — `ninguem` também cobre ausência de dia e legado, sem julgamento de porta.** `_grupo([])` retorna `None`; `_fio_cru('fora','furo_espelho',None)` retorna `ninguem` (`app/folha/fora_da_folha.py:91-92`, `app/folha/retrato_folha.py:370`). Retrato sem fio gravado também vira `ninguem` pelo fallback, sem consultar porta (`app/folha/retrato_folha.py:438-443`). São outros universos além de “dia sem porta”; quantidades atuais `[nao verificado]`.

- **Selo incompleto para esses casos.** O teste novo de ausência de porta usa uma linha sem `acoes` e `tipo=sem_causa`; não exerce pessoa com dias mistos (`app/chamados/tests/test_e3_r3_c4_sem_banco.py:33-35`, `app/chamados/tests/test_e3_r3_c4_sem_banco.py:93-102`). O contrato integrado cobre somente sistema sem fila/âncora/oferta, não desconhecimento da autoridade (`app/chamados/tests/test_e3_r3_c4_sem_banco.py:104-130`).

- **Docstrings alteradas de `tarefa_do_dia`: nenhum erro identificado.** Os memos existem por instância (`app/chamados/services/tarefa_do_dia.py:397`, `app/chamados/services/tarefa_do_dia.py:543-551`, `app/chamados/services/tarefa_do_dia.py:565-600`); as fontes adicionais da rota são usadas (`app/chamados/services/tarefa_do_dia.py:580-582`, `app/chamados/services/tarefa_do_dia.py:592-599`); `_posicoes` efetivamente devolve o dict da lâmpada (`app/chamados/services/tarefa_do_dia.py:80-85`).

- **Juiz duplicado/escritor/trava/pré-requisito:** nenhum acréscimo dessas classes nos corpos alterados pelo diff fornecido; `tarefa_do_dia` mantém a tranca lida de `dias_trancados` e retorno sem ações (`app/chamados/services/tarefa_do_dia.py:215-223`, `app/chamados/services/tarefa_do_dia.py:443-448`). Auditoria completa de todos os caminhos históricos `[nao verificado]`.

## 14/11 23:40 · sessao d9f48c61f263 · - · - · origem=Ronald (tmux)

**São 30 unidades no recorte dos 67.** A publicação registra 80 unidades: 50 com col+dia e **30 indeterminadas por ausência de col ou dia**; a sonda registra essas mesmas 30 como puladas. (app/docs/RELATO.md:8386, :8394, :8399)

Os **63 IDs ausentes** mencionados no anexo abrangem outro conjunto: **33 “sem divergência hoje” + essas 30 indeterminadas**. Portanto, não são 63 unidades adicionais aos 37 individualizados. (app/docs/RELATO.md:8390–8394; anexo p0.r [nao verificado])

**Recuperadas: 0; não recuperáveis nesta cópia: 30.** Não encontrei ID de unidade nem par colNNN + data explicitamente vinculado a qualquer dessas 30 nas buscas da árvore inteira. Resultado negativo: [nao verificado].

Conferi também as pistas documentais:

- **HJ-054** aparece como “não casado”, sem col. Recupera somente a raiz; não fornece sufixo `Uk`, colNNN ou data para individualizar uma unidade. (app/docs/RELATO.md:8397)
- A tabela dos 46 e a tabela G3/G4 publicam pares e donos, mas a correspondência desses registros com as 30 unidades puladas permanece **[nao verificado]**. Não transportei suas datas ou donos. (app/docs/RELATO.md:1642–1656, :8478–8498)
- **Dono L-099 das 30: [nao verificado]**; a publicação as inclui entre os indeterminados, sem dono escrito. (app/docs/RELATO.md:8394)

Buscas executadas em todo `app/docs`, incluindo `RELATO-ARQUIVO`, `BACKLOG`, `PROMPTS` e `LAPIDES`, e fora dele: **[nao verificado]**. Usei `git grep --no-index` porque a cópia não possui metadados Git; HEAD **[nao verificado]**.

```sh
git grep --no-index -n -I -o -E 'HJ-[0-9]{3}-U[0-9]+(/[U]?[0-9]+)*|hj_0511|HJ-INGESTAO' -- app/docs

rg -n --hidden --no-ignore -i 'sem col/dia|sem dia|nao casad|indeterminad' app/docs

rg -n --hidden --no-ignore -i 'G3G4|G1-PORTA|G2-TRIAGEM|G6-PAGINA' app/docs

git grep --no-index -n -I -o -i -E 'hj_0511|HJ[-_][0-9]{3}[-_]U[0-9]+|GRADE-RESOLVE-67|GRADE-MESTRA/resolve|p0\.r|resolve\.tsv' -- . ':(exclude)app/docs/**'
```

RESULTADO|INGESTAO-SEM-ID|30|recuperadas=0|nao_recuperaveis=30

## 14/11 23:40 · sessao e090e83eb1f9 · - · - · origem=Ronald (tmux)

Recorte dos 67: **30 unidades = 67 − 37**, compatíveis com as 30 sem col/dia; nenhuma unidade adicional recuperada por ID `HJ-NNN-Uk` ou par colNNN + data. A ingestão registra 50 casadas com col+dia e 30 indeterminadas, e distingue essas 30 dos três indeterminados já individualizados. (`app/docs/RELATO.md:8386`, `app/docs/RELATO.md:8394`)

As tabelas alternativas fornecem datas e donos, mas não vinculam seus casos às 30 unidades puladas:

- BO-0511: universo 46, com 16 sem dia; tabela em `app/docs/RELATO.md:1642`, casos sem dia em `app/docs/RELATO.md:1647` e soma em `app/docs/RELATO.md:1656`.
- G3/G4: universo 19, sendo 11 com dia e oito sem dia; IDs próprios G3/G4, sem correspondência publicada com `HJ-NNN-Uk`. (`app/docs/RELATO.md:8478`, `app/docs/RELATO.md:8485`, `app/docs/RELATO.md:8498`)
- `HJ-054` é apenas raiz não casada, sem col, data ou sufixo de unidade; não conta como recuperação. (`app/docs/RELATO.md:8397`)
- Dono L-099 fechado para as 30: [nao verificado]; publicação as inclui como indeterminadas sem dono. (`app/docs/RELATO.md:8394`)

Os 63 sem ID do anexo abrangem **33 sem divergência + 30 sem col/dia**, portanto são universo distinto do complemento 30 dos 67. (`app/docs/RELATO.md:8393`, `app/docs/RELATO.md:8394`)

Buscas realizadas em todo `app/docs`, incluindo os quatro documentos solicitados: [nao verificado]

```sh
rg -n --hidden --no-ignore 'HJ-[0-9]{3}-U[0-9]+|hj_0511|HJ-INGESTAO' app/docs
rg -n --hidden --no-ignore -i 'HJ[ _-]?[0-9]{1,3}|G[1-6][- _][0-9]{1,2}' app/docs
rg -n --hidden --no-ignore -i 'sem col/dia|sem dia|nao casad|indeterminad' app/docs
rg -n --hidden --no-ignore -i 'G3G4|G1-PORTA|G2-TRIAGEM|G6-PAGINA' app/docs
```

Recomendação de consolidação: `RESULTADO|INGESTAO-SEM-ID|30|recuperadas=0|nao_recuperaveis=30`.

## 14/11 23:39 · sessao 183d2f4773c6 · - · - · origem=Ronald (tmux)

Fora de `app/docs`, não encontrei unidades adicionais da ingestão, IDs `HJ-NNN-Uk` nem pares colNNN+data vinculados a ela. Resultado negativo de busca: [nao verificado].

A contagem pertinente é **30 unidades sem col/dia**, declaradas em `app/docs/RELATO.md:8386` e `app/docs/RELATO.md:8394`; a publicação registra 30 puladas em `app/docs/RELATO.md:8399`.

Existe **HJ-054**, explicitamente “não casado”, sem col pelo mapa. Isso recupera uma raiz documental, mas não identifica `HJ-054-Uk`, data ou colaborador; não permite contar uma unidade individual recuperada. (`app/docs/RELATO.md:8397`)

O código apenas aponta ao quadro externo e retorna “não medido” se ele não puder ser lido; não contém o rol de casos. (`bin/placar_grade.py:130`, `bin/placar_grade.py:134`, `bin/placar_grade.py:139`, `bin/placar_grade.py:145`)

Greps executados:

```sh
rg -n --hidden --no-ignore -g '!app/docs/**' 'hj_0511|HJ-[0-9]{3}-U[0-9]+|GRADE-RESOLVE-67|GRADE-MESTRA/resolve|p0\.r|resolve\.tsv' .
rg --files --hidden --no-ignore | rg '(^|/)(logs|hj_0511|GRADE-RESOLVE-67|p0\.r|resolve\.tsv|quadro\.md)(/|$)|HJ-|hj_'
rg -n --hidden --no-ignore -g '!app/docs/**' -i 'ingest[aã]o.{0,30}hj|hj.{0,30}ingest[aã]o|HJ-054|HJ-050' .
rg -n --hidden --no-ignore -g '!app/docs/**' 'HJ-0[0-9][0-9]|80 unidades|30 sem dia|30 indeterminad|76 casadas' .
rg -n --hidden --no-ignore 'hj_0511|HJ-054|HJ-050-U2|30 indeterminadas|30 sem col/dia' app/docs
```

Verificação física com `os.path.exists`: `logs/`, `logs/hj_0511/mapa.md`, `logs/hj_0511/sonda_a.out` e `logs/GRADE-MESTRA/resolve/{p0.r,resolve.tsv,quadro.md}` ausentes. Ausência e execução dos comandos: [nao verificado].

## 14/11 23:39 · sessao 4cd2f4d61152 · GRADE-RESOLVE-67 · censo · origem=Code

(1) O complemento identifica estes dia-colab como classe 8.2; o ano vem do registro datado de 2026-10-27 (`app/docs/CORTES.md:343`; também `app/docs/BACKLOG.md:821`):

```text
col789|2026-10-14
col789|2026-10-15
col789|2026-10-17
col789|2026-10-20
col789|2026-10-21
col789|2026-10-26
```

(2) **PRECISA SONDA.** A casa registra 8.2 como “não fechado” (`app/docs/RELATO.md:52`). O juiz depende de cadastro, batidas, fase e Cadastro × Realidade; esses insumos dos seis dias são `[nao verificado]` (`app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:182`). O ESTRUTURA escrito no registro pertence a 8.1 (`app/docs/RELATO.md:3429`).

(3) Sonda para o shell Django já inicializado; execução e resultados `[nao verificado]`. Usa a assinatura real e a janela da empresa (`app/ponto/management/commands/e6_oraculo.py:128`; `app/ponto/janelas.py:60`). Bloqueia SQL diferente de SELECT e suprime mensagens internas.

```python
import io
import logging
import re
from contextlib import ExitStack, redirect_stdout, redirect_stderr
from datetime import date

from django.db import connections
from django.utils import timezone

dias = [date(2026, 10, n) for n in (14, 15, 17, 20, 21, 26)]
erros = 0
linhas = []
nivel_anterior = logging.root.manager.disable

def somente_select(execute, sql, params, many, context):
    texto = sql.strip()
    if (
        many
        or not re.match(r"(?is)^SELECT\b", texto)
        or ";" in texto
        or re.search(r"(?is)\bINTO\b|\bFOR\s+(UPDATE|SHARE)\b", texto)
    ):
        raise RuntimeError("SQL bloqueado")
    return execute(sql, params, many, context)

try:
    logging.disable(logging.CRITICAL)
    with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
        with ExitStack() as pilha:
            for alias in connections:
                pilha.enter_context(
                    connections[alias].execute_wrapper(somente_select)
                )

            from colaboradores.models import Colaborador
            from ponto.janelas import janela_atual
            from ponto.management.commands.e6_oraculo import InsumosDoDono

            agora = timezone.now()
            hoje = timezone.localtime(agora).date()
            colab = Colaborador.objects.select_related("empresa").get(pk=789)

            for dia in dias:
                try:
                    ini, fim = janela_atual(dia, colab.empresa)
                    dono = InsumosDoDono(
                        colab, ini, fim, hoje, agora=agora
                    ).dono(dia)
                    if dono not in {"ESTRUTURA", "CADASTRO", "BATIDA"}:
                        raise RuntimeError("Juiz sem resposta")
                    linhas.append(f"DONO|col789|{dia.isoformat()}|{dono}")
                except Exception:
                    erros += 1
except Exception:
    erros += 1
finally:
    logging.disable(nivel_anterior)

for linha in linhas:
    print(linha)
print(f"FIM|erros={erros}")
```

(4) Se a sonda confirmar ESTRUTURA: o enquadramento documental é **bitemporal**, explicitamente ligado à classe 8.2 (`app/docs/BACKLOG.md:821`), termo existente em `app/docs/PLANO-PADROES.md:47`. A obra de destino é **TENTATIVA-DO-ADMIN-VIRA-COBRANCA**, registrada na grade; fechamento efetivo `[nao verificado]` (`app/docs/GRADE-MESTRA.md:472`).

RESULTADO|BO-ADMIN-2710|8.2|PRECISA SONDA|-|-

## 14/11 23:39 · sessao 346937135ca3 · GRADE-RESOLVE-67 · censo · origem=Code

(1) Dia-colab:

- col859 — 2026-11-07. (app/docs/RELATO.md:222)
- col859 — 2026-11-09. (app/docs/RELATO.md:222)

O vínculo ao caso 10 segue a ordem publicada, expressamente contestável. Os dias aparecem como 07/11 e 09/11; ano 2026 adotado pelo contexto, sem ano explícito nesse registro. (app/docs/BACKLOG.md:783; app/docs/RELATO.md:226) [nao verificado]

(2) **PRECISA SONDA.** O relato registra ambos como classe “c” e aponta falha visual, mas não escreve o dono L-099 desses dias. A montagem real consulta batidas, cadastro, fase e, quando necessário, o motor. (app/docs/RELATO.md:222; app/ponto/management/commands/e6_oraculo.py:182; app/ponto/management/commands/e6_oraculo.py:162)

O `git grep` falhou por ausência de metadados Git; a busca equivalente com `rg` não localizou teste citando a fonte/caso. [nao verificado] [nao verificado]

(3) Sonda proposta para o shell Django já inicializado; execução e resultados **[nao verificado]**. Assinatura conferida, incluindo `agora` obrigatório; janela obtida pela função real da empresa. (app/ponto/management/commands/e6_oraculo.py:128; app/ponto/janelas.py:54)

```python
import io
import logging
from contextlib import ExitStack, redirect_stdout, redirect_stderr
from datetime import date

from django.db import connections
from django.utils import timezone

ALVOS = (
    (859, date(2026, 11, 7)),
    (859, date(2026, 11, 9)),
)


def somente_select(execute, sql, params, many, context):
    texto = sql.strip()
    if (
        many
        or not texto.upper().startswith("SELECT ")
        or ";" in texto
        or "--" in texto
        or "/*" in texto
    ):
        raise RuntimeError("SQL bloqueado")
    return execute(sql, params, many, context)


linhas = []
erros = 0
nivel_anterior = logging.root.manager.disable
logging.disable(logging.CRITICAL)

try:
    with ExitStack() as pilha:
        for alias in connections:
            pilha.enter_context(
                connections[alias].execute_wrapper(somente_select)
            )

        agora = timezone.now()
        hoje = timezone.localtime(agora).date()

        for pk, dia in ALVOS:
            try:
                with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
                    from colaboradores.models import Colaborador
                    from ponto.janelas import janela_fechamento
                    from ponto.management.commands.e6_oraculo import InsumosDoDono

                    colab = Colaborador.objects.only("pk", "empresa_id").get(pk=pk)
                    ini, fim = janela_fechamento(11, 2026, colab.empresa_id)
                    if not ini <= dia <= fim:
                        raise RuntimeError("Dia fora da janela")

                    dono = InsumosDoDono(
                        colab, ini, fim, hoje, agora=agora
                    ).dono(dia)

                    if dono not in {"ESTRUTURA", "CADASTRO", "BATIDA"}:
                        raise RuntimeError("Dono indisponivel")

                linhas.append(f"DONO|col{pk}|{dia.isoformat()}|{dono}")
            except Exception:
                erros += 1
finally:
    logging.disable(nivel_anterior)

for linha in linhas:
    print(linha)
print(f"FIM|erros={erros}")
```

(4) Padrão quebrado e obra que fecha: **[nao verificado]**, dependentes da confirmação de ESTRUTURA.

RESULTADO|BO-1311-ROTEIRO|10|PRECISA SONDA|-|-

## 14/11 23:38 · sessao 46ae9e0cd25e · GRADE-RESOLVE-67 · censo · origem=Code

(1) **SEM DIA-COLAB.** O registro menciona “40 dias de férias com batida” para classificação L-099, sem enumerar colNNN/data; a remissão documental repete essa contagem. (app/docs/BACKLOG.md:1917; app/docs/PROMPTS.md:2226)

O teste da listagem cria ausência e batida sintéticas; não identifica os dias de produção do caso. (app/ferias/test_hj090_um_juiz.py:480–490) Os pares publicados para HJ-091 tratam de ausência versus agendamento; sua participação nos 40 dias com batida é [nao verificado]. (app/docs/RELATO.md:291)

(4) Dono ESTRUTURA e correspondente padrão/obra deste julgamento: [nao verificado]. Encerrado sem sonda, conforme a condição de ausência de dia-colab.

RESULTADO|BACKLOG-HJ|HJ-090|SEM DIA-COLAB|-|-

## 14/11 23:38 · sessao fef0127e9f1d · GRADE-RESOLVE-67 · censo · origem=Code

(1) **SEM DIA-COLAB.** O registro lista col996 na segunda posição, mas não informa data do caso; a numeração pela ordem do universo é expressamente contestável. (app/docs/BACKLOG.md:783; app/docs/RELATO.md:226)

O relato dos casos 01–05 fornece apenas totais agregados, sem individualizar os dias do caso 02. (app/docs/RELATO.md:222)

(4) Dono ESTRUTURA, padrão quebrado e obra correspondente: [nao verificado]. Sem dia-colab identificado, encerro sem sonda.

RESULTADO|BO-1311-ROTEIRO|02|SEM DIA-COLAB|-|-

## 14/11 23:38 · sessao addb1107546f · GRADE-RESOLVE-67 · censo · origem=Code

(1) **SEM DIA-COLAB.** O registro identifica **col789**, mas não individualiza nenhuma data de apuração para o caso **8.3**. A fonte informa a alteração em 27/10 e uma janela de regenerações de 21/09 a 30/11, sem discriminar os dias deste subitem. (app/docs/RELATO.md:3429; app/docs/CORTES.md:332)

As datas individualizadas no complemento pertencem expressamente à classe **8.2**. (app/docs/BACKLOG.md:821)

(4) Dono ESTRUTURA **[nao verificado]**: a casa registra **8.3 como não fechado**. Há encaminhamento à E4-REGENERAR-VINCULO-SEM-VIGENCIA, mas nenhuma classificação fechada que permita atribuir padrão quebrado neste censo. (app/docs/RELATO.md:52; app/docs/RELATO.md:3419)

RESULTADO|BO-ADMIN-2710|8.3|SEM DIA-COLAB|-|-

## 14/11 23:38 · sessao 76876f71497a · GRADE-RESOLVE-67 · censo · origem=Code

(1) **col246 — 2026-11-01**. O vínculo explícito com o caso 04 está em `app/docs/BACKLOG.md:1970`; o ano é contextualizado em `app/docs/RELATO.md:168`. Isso supera a associação apenas pela ordem, marcada como contestável em `app/docs/RELATO.md:226`.

(2) **Dono L-099: [nao verificado]; PRECISA SONDA.** A casa registra “cobrança indevida” e encaminhamento à E4b, mas não informa os insumos que determinam o dono (`app/docs/BACKLOG.md:1970`; `app/ponto/management/commands/e6_oraculo.py:98`). A montagem real consulta batidas, cadastro, fase e motor (`app/ponto/management/commands/e6_oraculo.py:182`).

(3) Sonda para contexto Django já inicializado, com bloqueio de SQL diferente de SELECT. Assinatura conferida em `app/ponto/management/commands/e6_oraculo.py:128`; janela por empresa conforme `app/ponto/management/commands/e6_oraculo.py:304`.

```python
import contextlib
import datetime as dt
import io
import logging

from django.db import connections
from django.utils import timezone
from colaboradores.models import Colaborador
from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import InsumosDoDono


def somente_select(execute, sql, params, many, context):
    if not sql.lstrip().upper().startswith("SELECT "):
        raise RuntimeError("SQL bloqueado")
    return execute(sql, params, many, context)


erros = 0
linhas = []
nivel_anterior = logging.root.manager.disable
logging.disable(10**9)

try:
    with contextlib.ExitStack() as pilha:
        pilha.enter_context(contextlib.redirect_stdout(io.StringIO()))
        pilha.enter_context(contextlib.redirect_stderr(io.StringIO()))
        for conexao in connections.all():
            pilha.enter_context(conexao.execute_wrapper(somente_select))

        agora = timezone.now()
        hoje = timezone.localtime(agora).date()
        dia = dt.date(2026, 11, 1)
        colab = Colaborador.objects.get(pk=246)
        ini, fim = janela_fechamento(11, 2026, colab.empresa_id)
        dono = InsumosDoDono(
            colab, ini, fim, hoje, agora=agora
        ).dono(dia)

        if dono not in {"ESTRUTURA", "CADASTRO", "BATIDA"}:
            raise RuntimeError("Dono indisponivel")
        linhas.append(f"DONO|col246|{dia.isoformat()}|{dono}")
except Exception:
    erros += 1
finally:
    logging.disable(nivel_anterior)

for linha in linhas:
    print(linha)
print(f"FIM|erros={erros}")
```

(4) Padrão quebrado e obra que fecha como ESTRUTURA: **[nao verificado]**, dependentes do resultado do juiz.

RESULTADO|BO-1311-ROTEIRO|04|PRECISA SONDA|-|-

## 14/11 23:37 · sessao 4b70da878e72 · GRADE-RESOLVE-67 · censo · origem=Code

(1) **SEM DIA-COLAB.** A oitava posição da lista aponta para **col523**, mas não informa data do caso; a própria numeração é contestável. (app/docs/BACKLOG.md:783; app/docs/RELATO.md:226)

O resumo dos casos 06–10 só individualiza datas de col859, sem identificar dia-colab do caso 08. (app/docs/RELATO.md:222)

(4) Dono L-099, padrão quebrado e obra correspondente: **[nao verificado]**. Encerrado sem sonda, conforme a condição de parada por ausência de dia-colab.

RESULTADO|BO-1311-ROTEIRO|08|SEM DIA-COLAB|-|-

## 14/11 23:37 · sessao 25e4ee69042b · GRADE-RESOLVE-67 · censo · origem=Code

(1) **O registro não identifica dia-colab do caso 09.** Pela ordem da lista, corresponderia a **col866**, mas sem data; a própria numeração é contestável. (app/docs/BACKLOG.md:783; app/docs/RELATO.md:226)

O resumo dos casos 06–10 só individualiza datas de col859; não fornece data para col866. (app/docs/RELATO.md:222)

O `git grep` falhou por ausência de metadados Git; usei `rg` equivalente, sem localizar testes que citem esta fonte/caso. [nao verificado] [nao verificado]

(4) Dono ESTRUTURA e padrão quebrado: [nao verificado]. Obra que fecha este caso: [nao verificado]. Sem dia-colab identificado, encerro sem sonda, conforme solicitado.

RESULTADO|BO-1311-ROTEIRO|09|SEM DIA-COLAB|-|-

## 14/11 23:37 · sessao 91bee8cf4a64 · GRADE-RESOLVE-67 · censo · origem=Code

(1) **SEM DIA-COLAB identificado para o caso 07.** A sétima posição da lista corresponde a **col1004**, mas a numeração é expressamente contestável; o registro menciona competência 11 sem individualizar a data desse caso. (app/docs/BACKLOG.md:783; app/docs/RELATO.md:226)

A data **27/10** aparece para col1004 no **HJ-093**; seu vínculo com o caso 07 deste roteiro permanece **[nao verificado]**. Os dias individualizados na síntese dos casos 06–10 pertencem a col859. (app/docs/BACKLOG.md:786; app/docs/RELATO.md:222)

O `git grep` falhou por ausência de metadados Git; a busca equivalente com `rg` não encontrou testes que citassem a fonte ou col1004. **[nao verificado]** [nao verificado]

(4) Sem dia-colab identificado, encerro sem sonda, conforme solicitado. Dono L-099, padrão quebrado e obra que fecha: **[nao verificado]**.

RESULTADO|BO-1311-ROTEIRO|07|SEM DIA-COLAB|-|-

## 14/11 23:37 · sessao 4462d0de0bd6 · GRADE-RESOLVE-67 · censo · origem=Code

(1) **SEM DIA-COLAB.** O registro permite apenas associar provisoriamente o caso 06 a **col982**, sexto da lista; não informa uma data para ele. A numeração é expressamente contestável. (app/docs/BACKLOG.md:783; app/docs/RELATO.md:226)

O resumo dos casos 06–10 identifica datas somente para **col859**: não estabelece dia-colab do caso 06. (app/docs/RELATO.md:222)

(4) Dono ESTRUTURA, padrão quebrado e obra que fecha: **[nao verificado]**. Sem dia-colab identificado, encerro sem sonda, conforme solicitado.

RESULTADO|BO-1311-ROTEIRO|06|SEM DIA-COLAB|-|-

## 14/11 23:37 · sessao 586a02ef3245 · GRADE-RESOLVE-67 · censo · origem=Code

1. **col407 — 21/10**: o registro vincula os itens 13/14 a col407 e identifica, especificamente para o item 13, o turno iniciado em 21/10. (app/docs/RELATO.md:1534)

2. **Dono: BATIDA.** A tabela de triagem L-099 publica expressamente **col407 21/10 → BATIDA → lista do admin**. Portanto, há prova documental para o mesmo dia-colab; o rótulo antigo “NOSSO” não precisa ser interpretado como dono. (app/docs/RELATO.md:1642; app/docs/RELATO.md:1648; app/docs/RELATO.md:1534)

3. **Sonda dispensada para recuperar o dono publicado**, pois a casa já o escreveu. Uma nova medição do estado atual permanece [nao verificado]. (app/docs/RELATO.md:1648)

4. **Padrão e obra: —**, pois o dono documental é BATIDA. (app/docs/RELATO.md:1648)

RESULTADO|BO-0611|13|BATIDA|-|-

## 14/11 23:37 · sessao 1a116d502ffb · GRADE-RESOLVE-67 · censo · origem=Code

(1) **O registro não identifica dia-colab do caso 05.** A lista coloca col265 na quinta posição, mas não fornece data; a numeração é expressamente contestável (app/docs/BACKLOG.md:783; app/docs/RELATO.md:226). O relato posterior apresenta apenas totais conjuntos dos casos 01–05, sem individualizar seus dias (app/docs/RELATO.md:222).

(4) Dono L-099, padrão quebrado e obra correspondente: **[nao verificado]**. Sem dia-colab identificado, encerro sem sonda.

RESULTADO|BO-1311-ROTEIRO|05|SEM DIA-COLAB|-|-

_5397 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
