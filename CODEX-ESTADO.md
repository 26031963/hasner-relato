# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 18:21** · publicado em **16/11/2026 18:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 18:21 · sessao 6d4997316df6 · - · - · origem=Ronald (tmux)

Execução adicional concluída, sem escrita:

- `python3 -B -m unittest chamados.tests.test_bug_par_sem_banco`: **RC 0, 7 testes OK**; testes em `app/chamados/tests/test_bug_par_sem_banco.py:24-185`.
- `python3 -B -m chamados.tests.bug_par_mutantes --mutante todos`: **RC 0, 17 controles verdes e 17 mutantes mortos por asserção, todos com erros=0**; critério em `app/chamados/tests/bug_par_mutantes.py:185-215`.
- Os cinco `c4_*`, `fundir_a_em_b`, `fundir_a_em_b_porta` e `f6_esconde` continuam **[nao verificado] em integração**: sem `--django`, o harness exclui os nomes de integração (`app/chamados/tests/bug_par_mutantes.py:139-143,178-181`).

Probe AST executado confirmou que `c4_fundir_l1_em_l2` muda o retorno do helper para `bloqueada_competencia`, porém a porta continua devolvendo `bloqueada_competencia_entregue`: `_recusa_da_entrega` só distingue L5 e converte todo o restante em L1 (`app/chamados/services/validacao.py:228-230`). Portanto esse mutante tem mordida na **testemunha**, pela asserção de `app/chamados/tests/test_bug_par_volta.py:229`; a fusão efetiva na porta é coberta pelo mutante separado `fundir_a_em_b_porta`, cuja execução continua [nao verificado] (`app/chamados/tests/bug_par_mutantes.py:103-106`; `app/chamados/tests/test_bug_par_volta.py:233`).

Os 17 mortos executados foram `dxg_frota`, `dxg_desligado`, `dia_civil`, `k6_so_comando`, `k6_omitido`, `unidade_sem_k6`, `marca_ignorada`, `hash_ignorado`, `bool_inteiro`, `chaves_ignoradas`, `repeticao_aceita`, `intersecao_aceita`, `selecionar_recusa`, `nova_nota`, `recalcular_f2`, `eixo_civil` e `eixo_pela_hora` (`app/chamados/tests/bug_par_mutantes.py:26-62,110-115`).

## 16/11 18:20 · sessao 7bdab0342751 · UI-EDITOR-CAL-REPETIR-CONFIRMA · verificador · origem=Code

**Devolvo por lacunas nos testes e pela prosa nova fora do mapa exigido pela L7.** Os casos preenchidos cobrem somente 30 dias; a sonda responde apenas pelos botões; a explicação nova está literal no template. (app/escala/tests/test_smoke_clique_folga_calendario.py:310, :324, :332, :153; app/templates/escala/folga_calendario_editar.html:131; app/docs/LEIS-UI.md:78)

Execução da suíte, execução dos mutantes, histórico `71e9576bf..HEAD` e resultado em produção: **[nao verificado]**. As conclusões abaixo são de leitura do código e do patch fornecido.

1) **O comportamento principal atende ao item 3; a quebra do Salvar já está implementada na cópia.**

Os dois botões recebem o tamanho e chamam a mesma `fcRepetir`. Alvo vazio copia imediatamente; alvo preenchido chama `hxConfirmar`; somente resposta verdadeira executa a cópia, filtrada pelo tamanho do destino. (app/templates/escala/folga_calendario_editar.html:50, :124; app/escala/views.py:318)

O Salvar já fornece `\n`, e o componente já possui `white-space:pre-line`. Portanto, nessa parte, o patch acrescenta verificação da quebra existente; não acrescenta uma correção de apresentação ao Salvar. (app/templates/escala/folga_calendario_editar.html:153; app/templates/core/_dialogo.html:122; app/escala/tests/test_smoke_clique_folga_calendario.py:294)

A nova ação altera estado local e campos do formulário; a persistência continua dependendo de Salvar, pela view e pela porta existente. (app/templates/escala/folga_calendario_editar.html:127; app/escala/views.py:320, :356; app/escala/folgas.py:571)

2) **Quem escreve e quem lê.**

| Elemento ou dado | Quem escreve → quem lê |
|---|---|
| Dias persistidos por tamanho | `salvar_calendario_de_folga` grava `dias` por `update_or_create` → a view lê `cal.tamanhos` para montar o editor. (app/escala/folgas.py:594; app/escala/views.py:362) |
| `tamanho`, `rotulo`, `n`, `marcados` do seed | A view fornece catálogo e marcações → o template escreve `#fc-seed` → o JS faz `JSON.parse` e inicializa `sel`. (app/escala/views.py:318, :363; app/templates/escala/folga_calendario_editar.html:74, :78) |
| Estado vivo `sel[tam]` | Seed inicializa; `fcTog` altera; `copia` substitui → decisão de confirmar, texto dos dias, renderização e campos enviados leem esse estado. (app/templates/escala/folga_calendario_editar.html:79, :119, :127, :130, :131, :86, :113) |
| Botões Repetir e parâmetro `tam` | O laço do template escreve `onclick` para 30 e 28 → clique chama `fcRepetir`; a sonda também localiza o botão por esse atributo. (app/templates/escala/folga_calendario_editar.html:50; app/escala/tests/test_smoke_clique_folga_calendario.py:144) |
| Grade: células, número, azul e evento | `renderGrade` escreve HTML e estilos a partir de `sel` → clique chama `fcTog`; a sonda lê número, visibilidade e fundo azul. (app/templates/escala/folga_calendario_editar.html:87; app/escala/tests/test_smoke_clique_folga_calendario.py:135) |
| Contador, marca “Vazia”, `data-fc-vazia` | Template fornece estado inicial; `renderGrade` atualiza texto, classe `hidden` e atributo → sonda lê contador; a decisão de Repetir consulta `sel`, sem reler essas marcas. (app/templates/escala/folga_calendario_editar.html:41, :95, :130; app/escala/tests/test_smoke_clique_folga_calendario.py:133) |
| Chips e canônico | `renderGrade` escreve ambos; `nomeBase` lê o nome para o canônico → o × do chip chama `fcTog`. (app/templates/escala/folga_calendario_editar.html:80, :99, :106) |
| Campos `dias_<tam>` em `#fc-hidden` | `syncHidden` escreve checkboxes marcados com nome e valor; `fcModo` os limpa no modo semana/dia → sonda lê valores; submissão serializa; view lê `getlist` e entrega à porta. (app/templates/escala/folga_calendario_editar.html:110, :189; app/escala/tests/test_smoke_clique_folga_calendario.py:129, :63; app/escala/views.py:342, :356) |
| Texto dos modais | Editor compõe mensagem com rótulos/dias e frases literais → `hxConfirmar` escreve `txt.textContent` → sonda lê `textContent` e `innerText`, considerando CSS. (app/templates/escala/folga_calendario_editar.html:131, :153; app/templates/core/_dialogo.html:190; app/escala/tests/test_smoke_clique_folga_calendario.py:114, :150) |
| Rótulos e respostas | Editor passa “Substituir”; componente escreve o botão positivo; markup fornece “Cancelar” → sonda lê e clica; handlers resolvem `true`/`false`; editor copia somente com `ok`. (app/templates/escala/folga_calendario_editar.html:132; app/templates/core/_dialogo.html:130, :157, :191; app/escala/tests/test_smoke_clique_folga_calendario.py:151) |
| Modal aberto, promessa e foco | Markup inicia `hidden`; `hxConfirmar` guarda foco/resolver e abre; `fechar` limpa pendência, esconde e restaura foco → handlers consultam a pendência; sonda consulta `.hidden`. (app/templates/core/_dialogo.html:119, :149, :189; app/escala/tests/test_smoke_clique_folga_calendario.py:148, :156) |
| Escape e clique fora | Handlers do componente chamam `fechar(false)` → promessa entrega falso ao editor, que preserva a grade. (app/templates/core/_dialogo.html:159; app/templates/escala/folga_calendario_editar.html:133) |
| Autorização do Salvar | Callback escreve `confirmado` apenas durante o reenvio; listener do submit lê a flag e o modo, e calcula grades vazias usando `sel`. (app/templates/escala/folga_calendario_editar.html:145, :159) |
| Dados e testemunhas dos testes | Fixture escreve usuário/calendários, incluindo 30 dias com `[8]`; sonda escreve `antes/logo/depois/conta/grade/modal/aberto` em `HX-CLIQUE`; Python extrai e desserializa para asserções. (app/escala/tests/test_smoke_clique_folga_calendario.py:243, :143, :156, :229) |

A leitura da marca visual também inclui as seções visíveis pelo atributo `data-fc-tamanho`; esse resultado é escrito em `HX-CLIQUE-VIS` e conferido contra `[28,30,31]`. (app/escala/tests/test_smoke_clique_folga_calendario.py:68, :215, :288)

3) **Os testes mordem parcialmente. As passagens abaixo são deduções estáticas; execução: [nao verificado].** [nao verificado]

| Mutante | Resultado deduzido |
|---|---|
| Copiar antes de perguntar | **Cai no cenário preenchido de 30 dias:** `logo` precisa continuar `[8]` antes da resposta. (app/escala/tests/test_smoke_clique_folga_calendario.py:313) |
| Cancelar também copia | **Cai em 30 dias:** exige preservação dos campos, contador e grade desenhada. (app/escala/tests/test_smoke_clique_folga_calendario.py:318) |
| Confirmar somente no Repetir de 30 | **Passa pelos cenários presentes.** Trocar a guarda por `tam === 28 || sel[tam].size === 0` dispensa confirmação em fevereiro preenchido; nenhum teste prepara esse estado. (app/templates/escala/folga_calendario_editar.html:130; app/escala/tests/test_smoke_clique_folga_calendario.py:310, :324, :332) |
| Escape/clique fora confirmam | **Não é exercitado pela sonda.** Ela aciona somente o ID de resposta; os handlers atuais estão corretos, mas essas saídas não têm testemunha neste smoke. (app/escala/tests/test_smoke_clique_folga_calendario.py:153, :189; app/templates/core/_dialogo.html:159) |
| Modal inteiro com `opacity:0` | **Cai:** a sonda examina opacidade do texto e ancestrais; retorna uma linha `SEM-CAIXA`, contra a exigência de duas. (app/escala/tests/test_smoke_clique_folga_calendario.py:107, :116, :294, :317) |
| Modal inteiro com `visibility:hidden` herdado | **Cai pelo mesmo mecanismo:** verifica a visibilidade computada do texto. (app/escala/tests/test_smoke_clique_folga_calendario.py:108, :116) |
| Somente botões invisíveis por CSS | **Escapa dessa medição:** os botões têm apenas `textContent` lido e recebem `.click()` programático; não passam por `visivel`. (app/escala/tests/test_smoke_clique_folga_calendario.py:150) |

Logo, a v3 fecha os dois mutantes CSS integrais apontados, mas não fecha a cobertura dos dois Repetir nem das duas saídas alternativas do modal. (app/escala/tests/test_smoke_clique_folga_calendario.py:107, :310, :332, :153)

4) **Classes de reprovação.**

- **Diálogo nativo:** não aparece no novo caminho; usa `hxConfirmar`. Há guarda global contra os formatos nativos enumerados. (app/templates/escala/folga_calendario_editar.html:131; app/core/tests/test_contract_l8_dialogo.py:42, :56, :91)
- **L7:** a mensagem é dinâmica e chega ao componente, mas a explicação “Repetir troca esses dias…” nasce literal no template, sem mapa compartilhado com a decisão. Isso não satisfaz integralmente a L7. A prosa antiga já enfileirada permanece assunto separado. (app/templates/escala/folga_calendario_editar.html:130; app/docs/LEIS-UI.md:78; app/docs/BACKLOG.md:822)
- **Tocar `core/_dialogo.html`:** não há hunk desse arquivo no patch fornecido; integridade histórica do intervalo Git: **[nao verificado]**. A quebra já disponível está em app/templates/core/_dialogo.html:122.
- **Selo sem mordida:** não é inteiramente vazio — confere formulário, contador, grade desenhada e texto renderizado —, mas deixa os mutantes identificados acima. (app/escala/tests/test_smoke_clique_folga_calendario.py:313, :317, :318, :329)
- **Relógio solto:** os novos cenários usam dias numéricos fixos e GET; os timers têm orçamento virtual de 4000 ms. Não há dependência do “hoje” nessas novas asserções; passagem na régua temporal: **[nao verificado]**. (app/escala/tests/test_smoke_clique_folga_calendario.py:199, :302, :310, :324)
- **Só uma casca:** o editor estende literalmente `base.html`; não há segunda casca selecionada nesse template. Ambas as cascas incluem o componente comum, com contrato existente. Portanto, não identifico meia correção entre cascas nesta alteração local. (app/templates/escala/folga_calendario_editar.html:1; app/templates/base.html:388; app/templates/base_app.html:145; app/core/tests/test_contract_l8_dialogo.py:146)

5) **`padrao=fonte unica`: compatível com o estado; incompleto para a mensagem.**

O termo existe no glossário. A nova decisão, os dias nomeados e a cópia consultam a mesma fonte viva `sel`; o modal continua delegado ao componente único. Nesse sentido restrito, o diff mantém a fonte única, embora não introduza uma centralização nova. (app/docs/GLOSSARIO.md:11; app/templates/escala/folga_calendario_editar.html:127, :130, :131)

O texto explicativo continua fora de um mapa compartilhado com a decisão, portanto o rótulo não resolve a pendência da L7. A mensagem efetiva do commit: **[nao verificado]**. (app/templates/escala/folga_calendario_editar.html:132; app/docs/LEIS-UI.md:78)

VEREDITO: DEVOLVE -- fevereiro preenchido sem cobertura (app/escala/tests/test_smoke_clique_folga_calendario.py:332); Escape/clique fora sem cobertura (app/escala/tests/test_smoke_clique_folga_calendario.py:153); explicação nova fora do mapa exigido pela L7 (app/templates/escala/folga_calendario_editar.html:132; app/docs/LEIS-UI.md:78).

## 16/11 18:20 · sessao b1ac6c67cd5d · - · - · origem=Ronald (tmux)

Mapa dos leitores/escritores, sem achado novo de compatibilidade nos consumidores:

1. **Estados K6 / retornos.** `_k6_que_barra` produz `None | (estado, texto)`, distinguindo ausência de dia e entrega, e conserva o desvio das 163 (`app/chamados/services/materializacao.py:792`, `:800`, `:804`, `:806`). `entrega_que_barra_a_validacao` apenas compõe essa consulta para pergunta + irmã (`app/chamados/services/validacao.py:209`, `:221`). Seus leitores encontrados são a testemunha, que separa `[0]`/`[1]`, e as duas guardas da porta, que usam `_recusa_da_entrega` (`app/chamados/services/materializacao.py:880`; `app/chamados/services/validacao.py:648`, `:685`, `:224`). **Não encontrei leitor esperando `str` da entrega** nesses usos (`app/chamados/services/materializacao.py:882`; `app/chamados/services/validacao.py:228`, `:230`).

   `_competencia_que_barra` **continua devolvendo texto**, por contrato e implementação: extrai `_k6[1]`, ou compõe F6, ou devolve `''` (`app/chamados/services/materializacao.py:812`, `:821`, `:828`, `:829`). Seus leitores continuam corretamente textuais: detalhe da testemunha no par/avulso e erro da unidade (`app/chamados/services/materializacao.py:903`, `:905`, `:927`, `:929`, `:1794`, `:1796`). O mock textual remanescente do teste é legítimo porque mira esse helper, não a entrega (`app/chamados/tests/test_bug_par_sem_banco.py:152`).

2. **Tela e adaptadores.** O card consulta a testemunha por filtro e decide pelos estados; os dois novos estados mostram detalhe e tiram Validar (`app/chamados/templatetags/chamados_tags.py:131`, `:136`, `:139`; `app/templates/chamados/partials/_pergunta_card.html:43`, `:44`, `:47`). `fio.py` publica `estado_aplicacao` e converte ausência/falha em indisponível (`app/chamados/services/fio.py:373`, `:375`, `:378`). O modal mostra o detalhe pelos estados e suprime Validar pelos estados (`app/templates/chamados/partials/modal_fio.html:230`, `:252`, `:274`). A casca da ação usa `estado`, `ok`, `msg` e erros; estados K6 desconhecidos individualmente caem corretamente no `ok=False` (`app/chamados/services/acoes_disputa.py:23`, `:32`, `:50`).

   **Não encontrei decisão da tela pelo texto de `detalhe`** nos leitores acima; os helpers de espera também conferem primeiro o estado e só então devolvem o detalhe (`app/chamados/services/materializacao.py:757`, `:758`, `:772`, `:773`). O explicador consulta os estados de recusa terminais e a mesma espera; perguntas simplesmente respondidas continuam no caminho genérico “aguarda validação” (`app/chamados/services/explicador.py:100`, `:124`, `:125`, `:131`, `:133`).

3. **Contextvar / sentinela.** A contextvar nasce com `default=None`; a única instalação está em `_validar_o_par`; a lista recebe apenas conflitos traduzidos como `COMPETENCIA_APROVADA`; a leitura pós-rollback e o reset ficam na porta (`app/chamados/services/materializacao.py:1759`, `:1446`, `:1448`, `:1449`; `app/chamados/services/validacao.py:545`, `:555`, `:563`). A sentinela é criada, devolvida e testada por identidade apenas nesse fluxo; seu leitor evita rodar a metade sozinha (`app/chamados/services/validacao.py:519`, `:560`, `:680`, `:682`).

4. **`recusa_motivo`.** O escritor usado pelo delta continua `_carimbar_recusa`: relê banco, limita 240, classifica pelo catálogo, grava motivo e chama a porta de nota; só colisão carimba identidade (`app/chamados/services/materializacao.py:1619`, `:1625`, `:1626`, `:1627`, `:1629`, `:1635`, `:1638`, `:1643`). Os demais caminhos existentes são: pendência de marco no materializador; recusa humana na validação; escape da resposta; duplicata via porta de identidade (`app/chamados/services/materializacao.py:1326`; `app/chamados/services/validacao.py:823`; `app/chamados/services/disputa_emissao.py:1511`; `app/chamados/services/fio.py:1404`; `app/chamados/models.py:3297`).

   Leitores relevantes: devolução decide por motivo preenchido e copia-o no ato; contador de paridade lê tradução; modal/card de atendimento exibem motivo; explicador o exibe em pendência; comando o fotografa (`app/chamados/services/validacao.py:262`, `:278`; `app/chamados/services/materializacao.py:300`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:192`; `app/chamados/services/explicador.py:113`; `app/chamados/management/commands/bug_par_103.py:69`). Fora do delta existe leitor textual por regex em `flip_auto`, mas restrito à via de paridade terminal, não aos novos estados/detalhe (`app/ponto/services/flip_auto.py:128`, `:132`).

5. **`validada_em`.** A assinatura do admin é escrita por `_carimbar_validacao`, com ato VALIDACAO no mesmo instante; a devolução limpa a assinatura e registra DEVOLUCAO com o motivo persistido (`app/chamados/services/validacao.py:161`, `:168`, `:171`, `:173`, `:269`, `:272`, `:275`, `:278`). A devolução das 103 também limpa; resposta nova e reabertura limpam pelos caminhos preexistentes (`app/chamados/services/validacao.py:345`; `app/chamados/services/disputa_emissao.py:1494`, `:1609`). O modelo ainda oferece a assinatura opcional no carimbo com `resposta='validar'`, restauração em memória e limpeza em lote da ressuscitação; não nasceram no delta (`app/chamados/models.py:3244`, `:3245`, `:3274`, `:1629`).

   Leitores que importam aqui: porta evita revalidar, card/modal distinguem assinatura, juiz de vida considera assinatura, laço exige assinatura e relê após trava; passivo fotografa-a na reversão (`app/chamados/services/validacao.py:806`, `:971`; `app/templates/chamados/partials/_pergunta_card.html:20`, `:42`; `app/templates/chamados/partials/modal_fio.html:228`; `app/chamados/juizes.py:1389`; `app/chamados/services/materializacao.py:2029`, `:2046`; `app/chamados/services/passivo_validado.py:66`). Outros leitores existentes incluem estado projetado, estatísticas do painel e verbos (`app/chamados/models.py:3151`; `app/chamados/services/painel.py:270`; `app/chamados/services/verbos.py:264`).

6. **`materializada_em`.** A atribuição canônica permanece `_escrever_carimbo`; `carimbar`, `carimbar_identidade`, `descarimbar` e lote administram o ato conjunto instante/autor/via; o delta usa essas portas (`app/chamados/models.py:3183`, `:3207`, `:3209`, `:3239`, `:3293`, `:3391`). O estado da pergunta/testemunha lê carimbo + via, não presume que qualquer carimbo significa batida gravada (`app/chamados/models.py:3153`; `app/chamados/services/materializacao.py:856`, `:862`, `:865`).

   No laço, seleção exige carimbo nulo e a releitura após trava evita executar a irmã já gravada; no passivo, o universo vem do juiz de lastro, contradiz carimbo falso, e a releitura também evita replantio pela irmã (`app/chamados/services/materializacao.py:2030`, `:2039`, `:2040`, `:2043`; `app/chamados/services/passivo_validado.py:4`, `:534`, `:535`, `:537`). A guarda de espera da escrita usa o derivador sem o atalho do carimbo justamente para o passivo (`app/chamados/services/materializacao.py:761`, `:764`, `:772`).

7. **Nota e atos.** O delta chama `_carimbar_recusa`, que chama `nota.anexar_ocorrencia` com `objeto_ref=pergunta:código`; a porta de notas produz chave por chamado/tipo/ref/autor e faz create/get em duplicação (`app/chamados/services/materializacao.py:1643`; `app/chamados/services/nota.py:62`, `:75`, `:95`, `:100`, `:103`, `:107`, `:109`). O fio carrega followups e os templates mostram conteúdo; não encontrei novo escritor direto da nota (`app/chamados/services/fio.py:202`, `:206`; `app/templates/chamados/partials/modal_fio.html:500`; `app/templates/chamados/partials/_card_atendimento.html:53`).

   A porta única de escrita dos atos continua `DecisaoPergunta.registrar`, com `bulk_create`; os atos VALIDACAO/DEVOLUCAO continuam chamados pelos escritores canônicos da validação (`app/chamados/models.py:4110`, `:4120`; `app/chamados/services/validacao.py:173`, `:275`). Leitores fora diff: histórico de respostas; rótulo das 103; tarefa do dia com a última decisão e horizonte; backfills/foto de auditoria (`app/chamados/models.py:3667`, `:3675`; `app/chamados/services/materializacao.py:275`; `app/chamados/services/tarefa_do_dia.py:158`, `:165`, `:167`, `:421`, `:430`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; `app/chamados/services/foto_e4b.py:280`).

8. **Contador e comando.** O contador conserva U163 por PK, distribui estados não previstos em `outras`, conserva espera somando em pendentes, e separa as 103 sem dia/entregues pelo retorno de `competencia_do_fato` (`app/chamados/services/materializacao.py:321`, `:330`, `:331`, `:333`, `:339`, `:344`, `:347`, `:351`, `:356`). O comando usa a mesma composição de competência, lista fora163 sem dia/entregue e produz linha falante; não consome o retorno alterado da entrega (`app/chamados/management/commands/bug_par_103.py:58`, `:59`, `:62`, `:71`, `:74`, `:75`, `:77`).

**Classificação textual existente, distinta de decisão da tela:** `traduzir_recusa` classifica texto por marcas; c4 chama esse catálogo, sem criar classificador paralelo; o passivo primeiro lê mudança de via e só depois traduz o erro (`app/chamados/catalogo/recusa.py:96`, `:100`, `:101`; `app/chamados/services/materializacao.py:1448`; `app/chamados/services/passivo_validado.py:140`, `:143`, `:144`).

## 16/11 18:20 · sessao 6d4997316df6 · - · - · origem=Ronald (tmux)

1. **Bloqueia: prova de integração executada ausente [nao verificado].** Os cinco `c4_*`, `fundir_a_em_b`, `fundir_a_em_b_porta` e `f6_esconde` exigem `--django` (`app/chamados/tests/bug_par_mutantes.py:139-143,172-181`); não executei porque o runner abre banco e cria diretórios (`app/config/settings/ci.py:6-18,26-33`). Não declarar esses mutantes mortos por asserção sem a execução autorizada e sua evidência [nao verificado].

2. **Âncoras conferidas em memória; nenhum erro de alvo encontrado.** Todos os nove alvos têm exatamente uma ocorrência no arquivo e na função declarada; o código mutado compila e o teste alvo existe: `app/chamados/tests/bug_par_mutantes.py:33-34,94-106,123-137`. Correspondências:
   
   | Mutante | Ponto efetivamente alterado | Asserção que deve matá-lo; integração ainda [nao verificado] |
   |---|---|---|
   | `c4_fundir_l1_em_l2` | `materializacao.py:806` | `test_bug_par_volta.py:229` |
   | `c4_motivo_l2_paridade` | `materializacao.py:1447` desativa registro da recusa | `test_bug_par_volta.py:258` |
   | `c4_desligar_portao_l3` | `materializacao.py:742` elimina pertencimento das divergências | `test_bug_par_volta.py:281` |
   | `c4_barrar_l4` | `materializacao.py:919`, ramo aplicável do **par** | `test_bug_par_volta.py:293` |
   | `c4_fundir_l5_em_l1` | `materializacao.py:804` | `test_bug_par_volta.py:307` |
   | `fundir_a_em_b` | `materializacao.py:882` | `test_bug_par_volta.py:229` |
   | `fundir_a_em_b_porta` | `validacao.py:230` | `test_bug_par_volta.py:233` |
   | `k6_omitido` | `materializacao.py:805` | **EXECUTADO: AssertionError**, `test_bug_par_sem_banco.py:143`; controle verde, mutante falhas=1/erros=0 | [nao verificado]
   | `f6_esconde` | `materializacao.py:904-905`, F6 do par | `test_bug_par_volta.py:247` |

   O harness muta fontes e `__code__` somente em memória, restaura ambos e exige `failures` sem `errors`/`skipped`; erro não conta como morte (`app/chamados/tests/bug_par_mutantes.py:192-217`).

3. **Fila: L4 não cobra os atos na própria linha da matriz.** O teste cobra retorno, batidas, carimbos e via, mas seu helper não verifica `DecisaoPergunta`; remover os atos poderia passar essa linha (`app/chamados/tests/test_bug_par_volta.py:288-297`; `app/chamados/tests/test_bug_par_projetado.py:103-110`). Acrescentar asserção de `VALIDACAO` nas duas metades; existe cobertura semelhante em outro teste, fora da linha L4 (`app/chamados/tests/test_bug_par_projetado.py:144-146`).

4. **Fila: L3 pode passar com a espera sem texto na tela.** O helper calcula “mostra detalhe”, porém L3 descarta esse terceiro resultado e cobra apenas botão/atributo de espera (`app/chamados/tests/test_bug_par_volta.py:207-209,280-282`). Cobrar também o detalhe nos dois HTMLs.

5. **Fila: L5 não prova “K6 não consultado”.** O teste cobra estado, detalhe, ocultação e persistência, mas não verifica chamadas ao juiz (`app/chamados/tests/test_bug_par_volta.py:303-313`). Consultar K6 indevidamente antes de devolver L5 poderia passar; limpar o mock e acrescentar `assert_not_called()` nesse cenário (`app/chamados/tests/test_bug_par_volta.py:28-30`).

6. **Fila: F4 prova zero do recorte, mas a invariância real de entrega fica limitada pelo mock.** A sequência pendentes 1→0, resolvidas 0→1 e conservação do recorte está cobrada (`app/chamados/tests/test_bug_par_volta.py:396-407`); porém `competencia_entregue` é substituído por retorno fixo `False` na fixture ancestral (`app/chamados/tests/test_bug_par_projetado.py:46-53`). A igualdade antes/depois comprova o resultado simulado; alteração do estado real de entrega permanece [nao verificado]. Para prova mais forte, usar a autoridade real com fixture de entrega controlada.

7. **Sem achado adicional na estrutura das linhas L1/L2/L5.** L1 e L5 cobram estado da testemunha, estado próprio da porta, detalhe nos dois HTMLs, ausência de Validar da pergunta examinada e fotografia persistida; L2 cobra testemunha `bloqueada_competencia`, porta `nao_gravou`, motivo/nota/DEVOLUCAO `COMPETENCIA_APROVADA`, atos `VALIDACAO→DEVOLUCAO`, ausência de carimbos/batidas e irmã preservada (`app/chamados/tests/test_bug_par_volta.py:203-215,222-273,299-313`). A fotografia das notas contém apenas PKs, portanto alteração do conteúdo de nota existente escaparia dessa fotografia (`app/chamados/tests/test_bug_par_volta.py:212-215`).

8. **Precedência cobrada parcialmente de forma explícita.** A testemunha é testada para F6 sobre divergência U163, depois K6 entregue e sem dia sobre F6; a porta é cobrada para F6 sobre divergência (`app/chamados/tests/test_bug_par_volta.py:315-338`). Ordem invertida de clique/retry está testada para sucesso, mas L2 novo só executa saída primeiro, irmã inicialmente não validada (`app/chamados/tests/test_bug_par_volta.py:44-66,244-273`). As variantes L2 com volta primeiro, irmã já validada/gravada, retry, passivo e laço não ficam demonstradas por esses testes [nao verificado].

## 16/11 18:19 · sessao 01b191d47538 · - · - · origem=Ronald (tmux)

Mapa de escritores/leitores:

| Dado/DOM | Escritor | Leitor |
|---|---|---|
| Dias persistidos por tamanho | `salvar_calendario_de_folga` usa `update_or_create`, `app/escala/folgas.py:594` | View lê `cal.tamanhos`, `app/escala/views.py:362`; modelo define `tamanho_mes/dias`, `app/escala/models.py:1569` |
| Tamanho, rótulo e dias iniciais | View cria `tamanhos`, `app/escala/views.py:363`; catálogo local `TAMS`, `app/escala/views.py:318` | Template cria `#fc-seed`, `app/templates/escala/folga_calendario_editar.html:74` |
| Estado vivo `sel[tam]` | Inicialização do seed, `app/templates/escala/folga_calendario_editar.html:79`; clique altera em `:119`; cópia substitui em `:127` | Decisão de perguntar em `:130`; dias do texto em `:131`; renderização em `:86`; inputs enviados em `:113` |
| Grade: células, dias, azul, eventos | `renderGrade`, `app/templates/escala/folga_calendario_editar.html:87` | Clique chama `fcTog`, `:89`; sonda lê células/CSS/dias, `app/escala/tests/test_smoke_clique_folga_calendario.py:135` |
| Contador, marca vazia e atributo `data-fc-vazia` | Template inicial em `app/templates/escala/folga_calendario_editar.html:41`; atualizados por `renderGrade`, `:95` | Sonda lê contador, `app/escala/tests/test_smoke_clique_folga_calendario.py:133`; guarda do Repetir **lê `sel`**, `app/templates/escala/folga_calendario_editar.html:130` |
| Chips e canônico | `renderGrade`, `app/templates/escala/folga_calendario_editar.html:99` | Chip chama `fcTog`, `:102`; nome alimenta canônico por `nomeBase`, `:80` |
| Checkboxes ocultos `dias_<tam>` | `syncHidden`, `app/templates/escala/folga_calendario_editar.html:110` | Sonda lê em `app/escala/tests/test_smoke_clique_folga_calendario.py:129`; POST lido pela view em `app/escala/views.py:342`, enviado à porta em `:356` |
| Mensagem do Repetir | Chamador concatena rótulo, dias ordenados e duas frases, `app/templates/escala/folga_calendario_editar.html:131` | `hxConfirmar` escreve `txt.textContent`, `app/templates/core/_dialogo.html:190`; sonda lê `textContent/innerText`, `app/escala/tests/test_smoke_clique_folga_calendario.py:150` |
| Verbo “Substituir” | Chamador, `app/templates/escala/folga_calendario_editar.html:132`; componente escreve `sim.textContent`, `app/templates/core/_dialogo.html:191` | Sonda lê, `app/escala/tests/test_smoke_clique_folga_calendario.py:151`; clique resolve verdadeiro em `app/templates/core/_dialogo.html:157` |
| “Cancelar” | Markup, `app/templates/core/_dialogo.html:130` | Sonda lê, `app/escala/tests/test_smoke_clique_folga_calendario.py:152`; clique resolve falso em `app/templates/core/_dialogo.html:158` |
| Estado aberto/fechado | Markup inicia `hidden`, `app/templates/core/_dialogo.html:119`; abertura em `:193`; fechamento em `:152` | Sonda lê `.hidden`, `app/escala/tests/test_smoke_clique_folga_calendario.py:148`, `:156` |
| Promessa pendente e foco | `hxConfirmar` captura foco e resolver, `app/templates/core/_dialogo.html:189`, `:194` | `fechar` limpa pendência/restaura foco/resolve, `:149`; Escape resolve falso em `:161`; fundo resolve falso em `:159`; chamador copia só se `ok`, `app/templates/escala/folga_calendario_editar.html:133` |

Conclusões:

- Os dois botões usam a mesma função parametrizada: template omite apenas tamanho 31, `app/templates/escala/folga_calendario_editar.html:50`; entradas 30 e 28 vêm da view, `app/escala/views.py:318`. A implementação pergunta sobre alvo preenchido e só copia depois do verdadeiro, `app/templates/escala/folga_calendario_editar.html:126`.
- A quebra do componente **já existe** por `white-space:pre-line`, `app/templates/core/_dialogo.html:122`. O novo texto fornece LF, `app/templates/escala/folga_calendario_editar.html:131`; o Salvar já fornecia LF, `:153`. Portanto o diff acrescenta medição da quebra anterior do Salvar, e usa o mecanismo anterior no Repetir; não muda CSS do componente, conforme o diff fornecido [não verificado quanto ao histórico Git].
- Não aparece diálogo nativo no novo caminho: chama `hxConfirmar`, `app/templates/escala/folga_calendario_editar.html:131`; L8 está em `app/docs/LEIS-UI.md:87`.
- Sobre L7: o texto é dinâmico e aparece no componente, porém contém frases literais no template, `app/templates/escala/folga_calendario_editar.html:131`; não sai de mapa compartilhado com a decisão, como exige `app/docs/LEIS-UI.md:78`. A prosa persistente da marca vazia (`:48`) e instrução dos chips (`:105`) são anteriores ao diff fornecido [não verificado quanto ao histórico Git].
- A tela usa somente `base.html` por declaração fixa, `app/templates/escala/folga_calendario_editar.html:1`; não usa `base_tpl`. O componente é compartilhado pelas duas cascas, `app/templates/base.html:388`, `app/templates/base_app.html:145`, mas não é modificado no diff fornecido [não verificado quanto ao histórico Git]. Não há segundo caminho efetivo deste editor para `base_app.html`, `app/templates/escala/folga_calendario_editar.html:1`.
- `fonte unica` é alias válido do glossário, `app/docs/GLOSSARIO.md:11`. O diff **preserva** a fonte viva `sel` e reutiliza o componente; sua mudança concreta é acrescentar confirmação antes de substituir, `app/templates/escala/folga_calendario_editar.html:124`. Não há extração/centralização de fontes duplicadas nesse trecho. A correspondência do termo com o compromisso descrito pelo commit é [não verificado], pois não foi fornecida mensagem do commit.

## 16/11 18:18 · sessao 18fde6552d64 · - · - · origem=Ronald (tmux)

- **Lacuna bloqueante: confirmação só em um Repetir passa por inspeção estática.** Os únicos alvos preenchidos testados são de 30 dias, tanto servidor quanto clique; fevereiro só aparece vazio. Mutante `if (tam === 28 || sel[tam].size === 0) { copia(); return; }` preserva os cenários exercitados e dispensa indevidamente a confirmação em fevereiro. (`app/escala/tests/test_smoke_clique_folga_calendario.py:309`, `:324`, `:332`; `app/templates/escala/folga_calendario_editar.html:130`)

- **Perguntar depois de copiar e Cancelar que copia são mordidos em 30 dias.** O teste exige `[8]` antes, logo após o clique e depois de Cancelar, além da contagem e grade pintada. (`app/escala/tests/test_smoke_clique_folga_calendario.py:311`, `:313`, `:318`, `:319`, `:320`)

- **Escape/clique fora: implementação correta, teste específico ausente.** O componente resolve `false` nessas duas saídas; a sonda só aciona os botões por ID. Mutar esses handlers para resolver `true` não é exercitado pelo smoke desta obra. (`app/templates/core/_dialogo.html:159`, `:161`; `app/escala/tests/test_smoke_clique_folga_calendario.py:153`, `:189`)

- **V3 fecha modal integralmente invisível por `opacity:0` ou `visibility:hidden`.** `visivel` verifica caixa, visibility calculada do texto e opacity de todos os ancestrais; a falha produz uma linha `SEM-CAIXA`, contra a exigência de duas linhas. (`app/escala/tests/test_smoke_clique_folga_calendario.py:107`, `:116`, `:294`, `:317`)

- **Limite de visibilidade:** esconder só os botões ainda escapa: a sonda lê `textContent` e executa `.click()` programático sem verificar caixa dos botões. `color:transparent`/recorte também não entram no predicado de visibilidade. (`app/escala/tests/test_smoke_clique_folga_calendario.py:108`, `:110`, `:151`, `:153`)

- **Relógio:** espera assíncrona usa timers de 200 ms com orçamento virtual de 4000 ms; os novos testes de Repetir fazem GET e não reenviam POST. Não há dependência temporal de calendário nessas novas asserções. (`app/escala/tests/test_smoke_clique_folga_calendario.py:147`, `:158`, `:199`, `:302`)

- **Uma casca é adequada ao editor:** o template estende literalmente `base.html`; a view não escolhe `base_app.html`. As duas cascas incluem o componente comum, defendidas pelo contrato existente. (`app/templates/escala/folga_calendario_editar.html:1`; `app/escala/views.py:369`; `app/core/tests/test_contract_l8_dialogo.py:146`)

- **Diálogo nativo:** a chamada nova usa `hxConfirmar`; existe guarda global sobre templates/JS contra `confirm`, `alert`, `prompt` e `hx-confirm`. (`app/templates/escala/folga_calendario_editar.html:131`; `app/core/tests/test_contract_l8_dialogo.py:42`, `:56`, `:91`)

Execução dos testes e dos mutantes: **[não verificado]**; análise exclusivamente estática, sem criar, editar ou apagar arquivos. [nao verificado]

## 16/11 18:02 · sessao fbc0b0495986 · UI-EDITOR-CAL-REPETIR-CONFIRMA · verificador · origem=Code

Os três achados não estão integralmente fechados: o verbo foi fechado; as provas de grade e texto desenhados continuam parciais. (`app/escala/tests/test_smoke_clique_folga_calendario.py:307`, `app/escala/tests/test_smoke_clique_folga_calendario.py:127`, `app/escala/tests/test_smoke_clique_folga_calendario.py:108`.) Execução dos mutantes: `[nao verificado]`; abaixo, conclusões por inspeção.

1. **Bloqueia o fechamento dos achados de teste — duas lacunas persistem.** [nao verificado]

   - **Grade desenhada: parcial.** Agora há comparação dos dias azuis depois de Cancelar, Substituir e copiar para grade vazia. Porém, `acesos()` só lê `backgroundColor`. Mutante no template: acrescentar `visibility:hidden` ao contêiner da grade. As cores permanecem computadas, os cliques programáticos continuam acionando os handlers e a medição anterior verifica apenas a `section`. Esse mutante escaparia por inspeção. (`app/templates/escala/folga_calendario_editar.html:55`; `app/escala/tests/test_smoke_clique_folga_calendario.py:68`, `app/escala/tests/test_smoke_clique_folga_calendario.py:119`, `app/escala/tests/test_smoke_clique_folga_calendario.py:127`, `app/escala/tests/test_smoke_clique_folga_calendario.py:312`, `app/escala/tests/test_smoke_clique_folga_calendario.py:321`, `app/escala/tests/test_smoke_clique_folga_calendario.py:329`.)

   - **Verbo frouxo: fechado no cenário coberto.** A asserção exige exatamente `Substituir`; o salvar já exige `Salvar assim mesmo`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:307`, `app/escala/tests/test_smoke_clique_folga_calendario.py:281`.)

   - **`innerText` sem caixa: fechado para ausência de retângulos e `visibility:hidden`, mas insuficiente para afirmar texto desenhado.** Mutante concreto, inserido no bloco `content` do template: `<style>#hx-confirma-texto{opacity:0}</style>`. Ele preserva caixa, `visibility` e as duas linhas de `innerText`, deixando o aviso transparente. Escaparia das verificações tanto do Repetir quanto do Salvar. (`app/templates/escala/folga_calendario_editar.html:3`; `app/templates/core/_dialogo.html:122`; `app/escala/tests/test_smoke_clique_folga_calendario.py:108`, `app/escala/tests/test_smoke_clique_folga_calendario.py:286`, `app/escala/tests/test_smoke_clique_folga_calendario.py:309`.)

2. **Quem escreve a cor; colisões.** `renderGrade()` consulta `s.has(d)`, escreve fundo `#0078d4` para marcado e `#fff` para desmarcado, depois instala as células com `innerHTML`. Nesse produtor não há terceiro estado de seleção com o mesmo azul. (`app/templates/escala/folga_calendario_editar.html:88`, `app/templates/escala/folga_calendario_editar.html:91`, `app/templates/escala/folga_calendario_editar.html:93`.)

   **O falso positivo é de visibilidade:** uma célula marcada escondida conserva o azul e entra em `acesos()`. Existe inclusive ocultação da grade inteira ao trocar para `semana_dia`, sem apagar seu DOM; a sonda de repetição não exercita esse caminho. Falso negativo por colisão entre estados atuais: `[nao verificado]`. (`app/templates/escala/folga_calendario_editar.html:188`; `app/escala/tests/test_smoke_clique_folga_calendario.py:128`.)

3. **`getClientRects()`/`visibility` não bastam.** O mutante de opacidade acima já demonstra a lacuna: a guarda não consulta opacidade nem recorte. Além disso, “modal aberto” na sonda significa somente `!fundo.hidden`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:108`, `app/escala/tests/test_smoke_clique_folga_calendario.py:81`, `app/escala/tests/test_smoke_clique_folga_calendario.py:140`.)

   **Sim, pode retornar `SEM-CAIXA` com o fundo visível:** basta esconder somente `#hx-confirma-texto` com `display:none` ou `visibility:hidden`. Abrir o componente remove `hidden` do fundo, sem restaurar essas propriedades do texto. Nesse caso, `['SEM-CAIXA']` tem comprimento 1 e reprova a exigência de duas linhas. (`app/templates/core/_dialogo.html:190`, `app/templates/core/_dialogo.html:193`; `app/escala/tests/test_smoke_clique_folga_calendario.py:108`, `app/escala/tests/test_smoke_clique_folga_calendario.py:286`, `app/escala/tests/test_smoke_clique_folga_calendario.py:309`.)

4. **Fila — nova dependência da ordem do DOM.** `acesos()` preserva a ordem dos elementos, enquanto `dias()` ordena numericamente; portanto, `[5,29]` agora pressupõe células nessa ordem. O produtor atual satisfaz isso pelo laço crescente de dias. (`app/escala/tests/test_smoke_clique_folga_calendario.py:123`, `app/escala/tests/test_smoke_clique_folga_calendario.py:128`, `app/escala/tests/test_smoke_clique_folga_calendario.py:321`; `app/templates/escala/folga_calendario_editar.html:87`.)

   O delta não acrescenta relógio, requisição de rede ou temporizador. As novas leituras usam os atrasos existentes de 200 ms e o orçamento virtual existente de 4000 ms; os recursos são preparados localmente pelo helper. (`app/escala/tests/test_smoke_clique_folga_calendario.py:147`, `app/escala/tests/test_smoke_clique_folga_calendario.py:151`, `app/escala/tests/test_smoke_clique_folga_calendario.py:191`; `app/core/tests/test_smoke_chromium.py:93`.)

## 16/11 18:02 · sessao fe5242fb0794 · - · - · origem=Ronald (tmux)

Inspeção estática; execução dos mutantes `[nao verificado]`.

- **Fila — “grade desenhada” fechado parcialmente.** Os três casos agora conferem a lista azul, mas `acesos()` lê somente `backgroundColor`, sem conferir caixa, visibilidade ou opacidade. Mutante: acrescentar `visibility:hidden` ao contêiner `fc-grade` preserva a cor computada e os cliques programáticos; as asserções visuais continuariam satisfeitas por inferência. A medição anterior só verifica a visibilidade da `section`, não da grade filha. (`app/escala/tests/test_smoke_clique_folga_calendario.py:127`, `:119`, `:312`, `:321`, `:329`, `:68`; `app/templates/escala/folga_calendario_editar.html:55`.)

- **Verbo frouxo fechado no cenário coberto:** o teste passou a exigir exatamente `Substituir`. O modal do salvar já exige `Salvar assim mesmo`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:307`, `:281`.)

- **Fila — “innerText sem caixa” fechado parcialmente.** A guarda rejeita ausência de retângulos e `visibility:hidden`; entretanto, o mutante local `#hx-confirma-texto{opacity:0}` mantém retângulos, `visibility` e `innerText`, embora esconda o aviso. Passaria pelas verificações de linhas dos dois modais, por inferência. (`app/escala/tests/test_smoke_clique_folga_calendario.py:108`, `:109`, `:286`, `:309`; `app/templates/core/_dialogo.html:122`.)

- **Autor da cor:** `renderGrade()` consulta `s.has(d)` e escreve fundo `#0078d4` quando marcado e branco quando desmarcado; depois instala as células via `innerHTML`. Nesse produtor não existe terceiro estado de célula com o mesmo fundo azul. Azul de botão, texto canônico e chips fica fora do seletor `#fc-grade-… > div`. Uma célula marcada escondida conserva a cor computada e pode produzir falso positivo de “desenhada”. (`app/templates/escala/folga_calendario_editar.html:88`, `:91`, `:93`, `:57`, `:66`, `:100`; `app/escala/tests/test_smoke_clique_folga_calendario.py:128`.)

- **`SEM-CAIXA` com modal aberto é possível:** texto `display:none`, ancestral intermediário `display:none` ou texto `visibility:hidden` dispara a guarda mesmo com fundo aberto. A sonda chama “aberto” apenas de `!fundo.hidden`; o componente abre removendo esse atributo. (`app/escala/tests/test_smoke_clique_folga_calendario.py:108`, `:81`, `:140`; `app/templates/core/_dialogo.html:193`.)

- **Relógio/rede:** o delta não acrescenta acesso a data, rede ou temporizadores; usa os atrasos existentes de 200 ms e orçamento virtual de 4000 ms. **Ordem:** acrescenta dependência da ordem DOM das células, pois `acesos()` não ordena o resultado, diferentemente de `dias()`. O produtor atual gera dias crescentes. Dependência nova entre testes `[nao verificado]`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:123`, `:128`, `:130`, `:150`, `:151`, `:191`; `app/templates/escala/folga_calendario_editar.html:87`.)

O checkout não contém `.git`; confirmação independente do intervalo histórico `[nao verificado]`.

## 16/11 17:55 · sessao 7dcf95b49d87 · E4C-F2 · verificador · origem=Code

Nos anexos, `arquivo:linha` indica a contagem do texto fornecido, incluindo linhas vazias; arquivos físicos desses anexos: [nao verificado]. Ocorrências em produção, resultados de sombra e efeitos monetários: [nao verificado].

1. **Fecha.** A premissa confere: `estado` traduz os campos, enquanto a exigibilidade distingue RECUSA/REABERTURA para colaborador e DEVOLUCAO para admin (`app/chamados/models.py:3151`; `app/chamados/models.py:3158`; `app/chamados/services/tarefa_do_dia.py:163`; `app/chamados/services/tarefa_do_dia.py:165`). Conservar cada significado e contar a divergência responde à pergunta, sem escolher vencedor novo (`estados.md:48`; `respostas_f2_estados_1611.txt:5`). Não colide isoladamente com a conservação; a mudança adicional da API está no problema do item 2 (`respostas_f2_estados_1611.txt:6`; `respostas_f2_estados_1611.txt:15`). Não determina intervenção em exportada, dinheiro ou `!` (`respostas_f2_estados_1611.txt:1`; `respostas_f2_estados_1611.txt:5`).

2. **Não fecha.** A premissa confere: a propriedade atual aceita somente instante; a API deriva `aguarda_admin` de `estado`; o juiz exige instante **e** texto (`app/chamados/models.py:3665`; `app/api/views.py:2553`; `app/chamados/juizes.py:1552`; `estados.md:50`).

   O corte exige dono admin, mas a fonte única escolhida pelo aval ainda atribui ao colaborador a última RESPOSTA com texto vazio; falta destinar essa correção na própria fonte (`app/docs/CORTES.md:487`; `app/chamados/services/tarefa_do_dia.py:159`; `app/chamados/services/tarefa_do_dia.py:161`; `respostas_f2_estados_1611.txt:3`; `respostas_f2_estados_1611.txt:6`).

   Além disso, trocar `aguarda_admin` para o dono pode mudar **pares legais**, além das exceções admitidas. Exemplo condicional: instante e texto presentes, sem validação/materialização, último SILENCIO; `estado` pode ser RESPONDIDA e a API atual retornar True, enquanto SILENCIO fornece nenhuma ação e a regra nova retornaria False (`app/chamados/models.py:3155`; `app/api/views.py:2553`; `app/chamados/services/tarefa_do_dia.py:176`; `app/docs/CORTES.md:487`). Isso colide com “sem mudar tela” e com a prova limitada aos pares ilegais; ocorrência desse caso em produção: [nao verificado] (`respostas_f2_estados_1611.txt:5`; `respostas_f2_estados_1611.txt:15`).

   Ficar fora dos pendentes do app está sustentado; mudar a partição para “não respondida = pendente” contrariaria esse corte (`app/chamados/services/pergunta_no_app.py:81`; `app/chamados/tests/test_o261e_f5_alcance.py:194`; `app/docs/CORTES.md:487`). Não há autorização para corrigir estoque entregue sem `!`; eventual impacto em exportada/dinheiro: [nao verificado] (`corte_juiz_respondida.txt:1`).

3. **Fecha, subordinado à proteção de exportadas.** A retenção atual aceita instante **OU** texto; o juiz nega texto sem instante. Logo, esse par sair da retenção é exatamente uma diferença admitida pela prova, mesmo conservando `estado=pendente` (`app/chamados/models.py:1924`; `app/chamados/models.py:1925`; `app/chamados/juizes.py:1552`; `app/chamados/models.py:3158`; `estados.md:52`; `respostas_f2_estados_1611.txt:15`). O contador e a devolução se houver ocorrência dão destino ao residual, sem autorizar cura de dados (`respostas_f2_estados_1611.txt:7`; `respostas_f2_estados_1611.txt:1`).

   Há alcance indireto no CSF, que consome a mesma lista; portanto, a exceção da prova não dispensa `PAREI !` se mudar resultado em exportada. Essa condição preserva a compatibilidade com a resposta 7; ocorrência e efeito monetário: [nao verificado] (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574`; `app/docs/CORTES.md:487`; `respostas_f2_estados_1611.txt:11`).

4. **Fecha.** Validação mais materialização produz `resolvida` sem testemunha nesse ramo; o card pode dizer “Encerrado”, enquanto o explicador consulta o efeito e pode retornar `validada_sem_gravar` (`app/chamados/models.py:3153`; `app/templates/chamados/partials/_card_atendimento.html:189`; `app/chamados/services/explicador.py:93`; `app/chamados/services/explicador.py:102`; `estados.md:54`). Conservar ambas as apresentações responde à pergunta; exigir testemunha fica expressamente como registro para E4d, sem colisão com a conservação nem autorização de intervenção em exportada/dinheiro/`!` (`respostas_f2_estados_1611.txt:8`; `respostas_f2_estados_1611.txt:15`; `respostas_f2_estados_1611.txt:1`).

5. **Fecha.** Tranca mata C6; sem validação/materialização, o estado ainda depende do instante, e o ramo morto pode mostrar “Validado pela supervisão” (`app/chamados/juizes.py:1409`; `app/chamados/models.py:3158`; `app/templates/chamados/partials/_card_atendimento.html:189`; `estados.md:56`). A resposta conserva essa discrepância e registra o achado de tela: dá destino sem prometer correção na F2, sem colisão ou ampliação de intervenção em exportada/dinheiro/`!` (`respostas_f2_estados_1611.txt:9`; `respostas_f2_estados_1611.txt:1`).

6. **Fecha como conservação na F2 e encaminhamento à F6.** Materializada e morta resulta em `resolvida`; sem validação nem instante, o cabeçalho administrativo mostra “aguardando colaborador” (`app/chamados/models.py:3157`; `app/templates/chamados/partials/_pergunta_card.html:25`; `estados.md:58`). “Lê o dono” precisa ser entendido como destino futuro registrado: o cabeçalho atual lê os campos, não `tarefa_do_dia` (`respostas_f2_estados_1611.txt:10`; `app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:24`). Assim interpretado, não colide com F6 L1 nem altera a tela na F2; não amplia exportada/dinheiro/`!` (`app/docs/CORTES.md:487`; `respostas_f2_estados_1611.txt:10`; `respostas_f2_estados_1611.txt:1`).

7. **Fecha quanto ao resultado conservado.** Qualquer carimbo exclui da retenção, embora C6 possa continuar considerando a pergunta viva; conservar essa divergência não afirma equivalência factual entre carimbo e efeito (`app/chamados/models.py:1923`; `app/chamados/juizes.py:1456`; `app/chamados/juizes.py:1489`; `estados.md:60`; `respostas_f2_estados_1611.txt:11`). O CSF herda a exclusão, tornando material a restrição de exportada: mudança exige `PAREI !` com tabela, não autorização implícita; invariância efetiva e efeito monetário: [nao verificado] (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:588`; `app/docs/CORTES.md:487`). A falta da fonte por ato permanece no item 9 (`app/docs/RELATO.md:109`).

8. **Fecha no caso perguntado: último SILENCIO.** Ressuscitar zera instante e texto sem escrever REABERTURA; SILENCIO continua sem ações, e devolver ao colaborador permanece decisão pendente (`app/chamados/models.py:1622`; `app/chamados/models.py:1625`; `app/chamados/models.py:1629`; `app/chamados/services/tarefa_do_dia.py:176`; `estados.md:62`). Isso conserva a B1 e cumpre a zeragem integral do corte, sem autorizar nova intervenção em exportada/dinheiro/`!` (`respostas_f2_estados_1611.txt:12`; `corte_juiz_respondida.txt:1`).

   “Ressuscitada segue sem ação” não vale universalmente: sem decisões, o leitor atribui resposta ao colaborador. A conclusão acima fica restrita ao SILENCIO da pergunta correspondente (`app/chamados/services/tarefa_do_dia.py:156`; `app/chamados/services/tarefa_do_dia.py:157`; `estados.md:62`).

9. **Não fecha.** A premissa confere: os tipos não incluem materialização; descarimbar apaga o campo; a projeção temporal lê o valor atual (`app/chamados/models.py:4024`; `app/chamados/models.py:4035`; `app/chamados/models.py:3318`; `app/chamados/juizes.py:1390`; `estados.md:64`). Continuar lendo o campo não fornece o instante anterior após descarimbar; a própria cópia registra F2 L1 como **RESIDUAL, não MOOT**, após a F1 (`app/docs/RELATO.md:109`).

   O corte manda, se residual, “efeito = o ato de materialização, valor conservado”; sua remissão à E4d trata da absorção. Transferir também F2 L1 à E4d não fecha esse literal e deixa a pergunta sem fonte definida (`app/docs/CORTES.md:487`; `respostas_f2_estados_1611.txt:13`). A resposta não autoriza construção nem `!`, e a proteção de exportadas continua obrigatória; impacto monetário: [nao verificado] (`respostas_f2_estados_1611.txt:1`; `app/docs/CORTES.md:487`).

AVAL-DEVOLVIDO 2: falta destinar a correção do dono na fonte única e resolver mudanças de `aguarda_admin` em pares legais, excluídas da prova (`app/chamados/services/tarefa_do_dia.py:161`; `app/api/views.py:2553`; `respostas_f2_estados_1611.txt:15`).
AVAL-DEVOLVIDO 9: remeter F2 L1 à E4d deixa sem fonte o instante após descarimbo e contraria o destino do residual (`app/chamados/models.py:3318`; `app/chamados/juizes.py:1390`; `app/docs/RELATO.md:109`; `app/docs/CORTES.md:487`).
VEREDITO: AVAL-DEVOLVIDO em 2, 9

## 16/11 17:54 · sessao 3a58eec350c9 · - · - · origem=Ronald (tmux)

1. **Fecha isoladamente.** A resposta preserva os dois significados: `estado` traduz os campos (`app/chamados/models.py:3151-3158`), enquanto a exigibilidade lê a última decisão (`app/chamados/services/tarefa_do_dia.py:159-177`). Contar a divergência sem trocar a tela dá destino à pergunta de desenho 1 (`estados.md:48`, anexo). A mudança de `aguarda_admin` introduzida no item 2, porém, precisa respeitar essa conservação (`respostas_f2_estados_1611.txt:5-6`, linhas do anexo).

2. **Devolver.**
   - A premissa técnica confere: `respondida` atual testa somente instante (`app/chamados/models.py:3665`); a API calcula `aguarda_admin` pelo estado (`app/api/views.py:2550-2553`); o juiz exige instante e texto (`app/chamados/juizes.py:1544-1552`).
   - O dono administrativo está autorizado pelo F5 L1 (`app/docs/CORTES.md:487`), mas a fonte única indicada pelo próprio aval ainda devolve ação do **colaborador** quando a última RESPOSTA tem texto vazio (`app/chamados/services/tarefa_do_dia.py:159-161`). Falta indicar como cumprir o corte nessa fonte sem tabela própria no leitor (`respostas_f2_estados_1611.txt:3,6`, anexo).
   - “API lê o dono” também pode mudar resultado para **pares legais**, contrariando a PROVA, que só admite diferenças dos pares ilegais (`respostas_f2_estados_1611.txt:6,15`, anexo). Exemplo condicional: campos com instante+texto, sem validação/materialização e último SILENCIO. O estado atual é RESPONDIDA (`app/chamados/models.py:3154-3158`), logo a API retorna `aguarda_admin=True` (`app/api/views.py:2553`); SILENCIO fornece nenhuma ação (`app/chamados/services/tarefa_do_dia.py:176-177`), portanto a nova regra sem dono retornaria False (`app/docs/CORTES.md:487`). O par pode passar pelo SQL (`app/chamados/models.py:3496-3498`) e por C6 nas condições do ramo da resposta (`app/chamados/juizes.py:1423-1435`). Ocorrência em produção: [nao verificado].
   - A leitura pode repercutir no app, mas efeito monetário/exportado concreto não está demonstrado: [nao verificado].

3. **Fecha isoladamente, preservando a restrição exportada do corte.** Hoje retenção aceita instante **OU** texto (`app/chamados/models.py:1923-1926`); o juiz exige ambos (`app/chamados/juizes.py:1552`). Texto sem instante sair da retenção é precisamente uma diferença de par ilegal admitida pela PROVA (`respostas_f2_estados_1611.txt:7,15`, anexo). O consumidor CSF usa a mesma lista, inclusive ignorando tranca (`app/ponto/supra_juiz.py:573-574`); portanto a exceção dos pares ilegais não pode dispensar o `PAREI !` se houver alteração em competência exportada (`app/docs/CORTES.md:487`; `respostas_f2_estados_1611.txt:11`, anexo). Ocorrência exportada e dinheiro afetado: [nao verificado].

## 16/11 17:53 · sessao a3355456f63e · - · - · origem=Ronald (tmux)

7. **Confere quanto ao resultado conservado:** qualquer `materializada_em` exclui da retenção, enquanto C6 pode continuar viva; isso preserva a divergência, sem provar equivalência factual entre carimbo e efeito (`app/chamados/models.py:1923`; `app/chamados/juizes.py:1423`; `app/chamados/juizes.py:1435`; `app/chamados/juizes.py:1489`). O CSF recebe a mesma exclusão (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574`; `app/ponto/supra_juiz.py:588`). A invariância de exportadas é condição expressa, com parada `!` se houver diferença; seu cumprimento real permanece **[nao verificado]** (`app/docs/CORTES.md:487`). A pendência de fonte/instante fica no item 9 (`app/docs/RELATO.md:109`).

8. **Confere no caso perguntado, com último SILENCIO:** a ressuscitação zera os campos sem escrever REABERTURA, e SILENCIO continua sem ações (`app/chamados/models.py:1622`; `app/chamados/models.py:1629`; `app/chamados/services/tarefa_do_dia.py:176`). A lei de devolver ao colaborador permanece explicitamente não decidida (`app/chamados/models.py:1625`). A expressão “ressuscitada segue sem ação” precisa ser entendida nesse caso: sem decisões, o leitor retorna `aguarda_resposta` com ação do colaborador (`app/chamados/services/tarefa_do_dia.py:156`). Os testes existentes distinguem as duas situações; execução **[nao verificado]** (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:331`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:334`).

9. **Não fecha:** a ausência de tipo próprio confere, mas não fornece o instante após descarimbar: a porta apaga o campo e a projeção temporal lê o valor atual (`app/chamados/models.py:4024`; `app/chamados/models.py:4035`; `app/chamados/models.py:3318`; `app/chamados/juizes.py:1389`). A cópia registra expressamente F2 L1 como **RESIDUAL, não MOOT**, após a F1 (`app/docs/RELATO.md:109`). O corte vigente manda, havendo residual, “efeito = o ato de materialização, valor conservado”; sua remessa à E4d está prevista para **absorção**, não para transferir F2 L1 (`app/docs/CORTES.md:487`). Assim, a resposta 9 deixa sem destino a pergunta correspondente e conflita com o corte, apesar de conservar provisoriamente a leitura do campo (`app/chamados/models.py:3318`; `app/chamados/juizes.py:1390`; `app/docs/CORTES.md:487`).

AVAL-DEVOLVIDO 9: remete F2 L1 à E4d sem resolver o residual expressamente registrado; descarimbo apaga o único instante atual, e o corte vigente exige ato de materialização com valor conservado (`app/docs/RELATO.md:109`; `app/chamados/models.py:3318`; `app/chamados/juizes.py:1390`; `app/docs/CORTES.md:487`).

## 16/11 17:53 · sessao 9fb862c49a4f · - · - · origem=Ronald (tmux)

4. **Confere.** A validação com materialização produz `resolvida` sem consultar testemunha (`app/chamados/models.py:3151`, `app/chamados/models.py:3153`); o card morto mostra “Encerrado”, salvo pendência administrativa (`app/templates/chamados/partials/_card_atendimento.html:179`, `app/templates/chamados/partials/_card_atendimento.html:189`). O explicador consulta a testemunha e, quando negativa, pode produzir `validada_sem_gravar` (`app/chamados/services/explicador.py:93`, `app/chamados/services/explicador.py:102`), apresentado como “O sistema não conseguiu gravar” (`app/templates/chamados/partials/modal_fio.html:442`). Conservar explicitamente as duas apresentações fecha a pergunta 4 dentro da conservação da F2; exigir testemunha para `resolvida` fica somente registrado para E4d (anexo `respostas_f2_estados_1611.txt`, resposta 4; arquivo físico/numeração do anexo [nao verificado]). Nenhuma alteração de exportada, dinheiro ou `!` está declarada nessa resposta (mesma referência).

5. **Confere.** A tranca mata C6 (`app/chamados/juizes.py:1409`, `app/chamados/juizes.py:1410`), mas, sem validação/materialização, `estado` continua dependendo do instante de resposta (`app/chamados/models.py:3154`, `app/chamados/models.py:3158`). No card morto sem pendência administrativa, estado diferente de `resolvida` recebe “Validado pela supervisão” (`app/templates/chamados/partials/_card_atendimento.html:179`, `app/templates/chamados/partials/_card_atendimento.html:189`). A resposta conserva expressamente a divergência e registra o achado de tela; não promete corrigi-lo na F2 (anexo `respostas_f2_estados_1611.txt`, resposta 5; arquivo físico/numeração [nao verificado]). Nenhuma alteração de exportada, dinheiro ou `!` está declarada nessa resposta (mesma referência).

6. **Confere como conservação na F2 e registro de destino na F6.** Materializada, morta, sem validação → `resolvida` (`app/chamados/models.py:3154`, `app/chamados/models.py:3156`, `app/chamados/models.py:3157`); sem validação nem instante, o cabeçalho administrativo mostra “aguardando colaborador” (`app/templates/chamados/partials/_pergunta_card.html:23`, `app/templates/chamados/partials/_pergunta_card.html:24`, `app/templates/chamados/partials/_pergunta_card.html:25`). **O cabeçalho atual não lê o dono**: “lê o dono” precisa ser entendido como encaminhamento futuro à F6, conforme “REGISTRAR”, não descrição da cópia (mesmas linhas; anexo `respostas_f2_estados_1611.txt`, resposta 6, numeração [nao verificado]). O leitor de ato distingue dono colaborador, admin e ausência de ação (`app/chamados/services/tarefa_do_dia.py:163`, `app/chamados/services/tarefa_do_dia.py:165`, `app/chamados/services/tarefa_do_dia.py:176`, `app/chamados/services/tarefa_do_dia.py:184`, `app/chamados/services/tarefa_do_dia.py:186`), compatível com o destino de não criar tabela própria de dono (anexo `cortes_f2_f9_1611.txt`, item 6, numeração [nao verificado]). Nenhuma mudança de exportada, dinheiro ou `!` é determinada pela conservação da resposta 6 (anexo `respostas_f2_estados_1611.txt`, resposta 6, numeração [nao verificado]).

**Sugestão de consolidação: não devolver 4, 5 ou 6**, mantendo a ressalva textual do item 6: leitura do dono é encaminhamento à F6; o código atual continua lendo os carimbos (`app/templates/chamados/partials/_pergunta_card.html:23`, `app/templates/chamados/partials/_pergunta_card.html:24`, `app/templates/chamados/partials/_pergunta_card.html:25`).

## 16/11 17:38 · sessao 7ee2c785a88a · UI-EDITOR-CAL-REPETIR-CONFIRMA · verificador · origem=Code

**Não identifiquei bloqueio comprovado no fluxo normal; há achados de fila na concorrência entre ações e na força dos testes.** A cópia atual sincroniza seleção, desenho e campos após confirmar; a view respeita o modo escolhido. (`app/templates/escala/folga_calendario_editar.html:127`; `app/escala/views.py:327`.)

Comparação integral dos hashes, execução dos testes e comportamento em produção: **[não verificado]**. Não executei o runner: ele cria/remove arquivos e grava registros, contrariando a restrição de leitura. (`app/escala/tests/test_smoke_clique_folga_calendario.py:178`; `app/escala/tests/test_smoke_clique_folga_calendario.py:187`; `app/escala/tests/test_smoke_clique_folga_calendario.py:259`.)

**1. Escritores, leitores e significado**

| Estado/valor | Quem escreve e quem lê |
|---|---|
| `sel[tam]`, incluindo `sel[31]` | A view fornece `marcados`; o template inicializa os `Set`. Escrevem `fcTog` e `copia`. Leem ordenação, desenho, contador, marca de vazia, chips, canônico, campos do formulário e os guardas de Repetir/Salvar. (`app/escala/views.py:362`; `app/templates/escala/folga_calendario_editar.html:79`; `app/templates/escala/folga_calendario_editar.html:82`; `app/templates/escala/folga_calendario_editar.html:119`; `app/templates/escala/folga_calendario_editar.html:127`; `app/templates/escala/folga_calendario_editar.html:149`.) |
| `dias_<tam>` em `#fc-hidden` | São **checkboxes marcados dentro de um div escondido**, não inputs `type="hidden"`. `syncHidden` recria todos a partir de `sel`; `fcModo` também pode apagar o conteúdo. Leem o formulário, as sondas e a view por `getlist`. (`app/templates/escala/folga_calendario_editar.html:62`; `app/templates/escala/folga_calendario_editar.html:110`; `app/templates/escala/folga_calendario_editar.html:114`; `app/templates/escala/folga_calendario_editar.html:189`; `app/escala/tests/test_smoke_clique_folga_calendario.py:120`; `app/escala/views.py:342`.) |
| `#fc-conta-<tam>` | O template escreve a quantidade inicial; `renderGrade` substitui pelo comprimento dos dias ordenados de `sel`. A sonda lê seu texto; a persistência lê `dias_*`, não o contador. (`app/templates/escala/folga_calendario_editar.html:46`; `app/templates/escala/folga_calendario_editar.html:95`; `app/escala/tests/test_smoke_clique_folga_calendario.py:123`; `app/escala/views.py:342`.) |
| Promise de `hxConfirmar` / `pendente` | `hxConfirmar` cria a Promise e guarda seu resolver em `pendente`. `fechar` retira o resolver, zera o estado, esconde o modal e resolve. Sim fornece `true`; Cancelar, Escape e fundo fornecem `false`. Outra confirmação cancela a anterior. `hxPerguntar` compartilha esse mesmo estado. Os `.then` do editor leem a resposta. (`app/templates/core/_dialogo.html:146`; `app/templates/core/_dialogo.html:149`; `app/templates/core/_dialogo.html:157`; `app/templates/core/_dialogo.html:187`; `app/templates/core/_dialogo.html:194`; `app/templates/core/_dialogo.html:202`; `app/templates/escala/folga_calendario_editar.html:133`; `app/templates/escala/folga_calendario_editar.html:156`.) |
| Texto do modal | Repetir monta a mensagem com rótulo e dias atuais do alvo; Salvar monta com os rótulos das grades vazias. O componente escreve por `textContent`; as sondas leem `textContent` e `innerText`. Uma chamada posterior substitui a mensagem anterior. (`app/templates/escala/folga_calendario_editar.html:131`; `app/templates/escala/folga_calendario_editar.html:153`; `app/templates/core/_dialogo.html:190`; `app/escala/tests/test_smoke_clique_folga_calendario.py:83`; `app/escala/tests/test_smoke_clique_folga_calendario.py:134`.) |

No servidor, campo ausente vira lista vazia; valores não inteiros ou fora de `1..tam` são descartados. A porta grava listas ordenadas e sem duplicatas. Não existe preenchimento automático pela grade de 31 nesse caminho. Em `semana_dia`, a view ignora `dias_*` e a porta apaga os tamanhos. (`app/escala/views.py:327`; `app/escala/views.py:340`; `app/escala/views.py:347`; `app/escala/folgas.py:584`; `app/escala/folgas.py:588`; `app/escala/folgas.py:593`.)

Há uma diferença de definição: a tela mede vazio pelo **conjunto bruto**; o vigia mede ausência de **datas válidas produzidas no mês**. Exemplo condicional: um seed de 30 dias contendo apenas `31` pareceria preenchido na tela, mas produziria zero folgas e seria descartado no POST. Existência desse dado em produção: **[não verificado]**. (`app/escala/views.py:362`; `app/templates/escala/folga_calendario_editar.html:149`; `app/escala/folgas.py:15`; `app/escala/folgas.py:127`; `app/escala/views.py:347`.)

Para dados válidos, tamanho ausente e lista vazia significam nenhuma folga; fevereiro bissexto consulta a grade de 28. O defeito operacional de folga, por sua vez, consulta a foto `FolgaDia`, podendo considerar folgas manuais: é outro estado, não um leitor de `sel`. (`app/escala/folgas.py:13`; `app/escala/folgas.py:14`; `app/escala/models.py:1247`; `app/escala/models.py:1249`.)

**2. Sequências**

| Sequência | Resultado pelo código |
|---|---|
| Alvo marcado → Cancelar | Não altera seleção, campos ou desenho; fecha o modal. (`app/templates/core/_dialogo.html:158`; `app/templates/escala/folga_calendario_editar.html:133`.) |
| Alvo marcado → Substituir | Substitui pelos dias da seleção **atual** de 31 que cabem no alvo e sincroniza tudo. Não há snapshot da origem. Origem vazia também esvazia o alvo. (`app/templates/escala/folga_calendario_editar.html:126`; `app/templates/escala/folga_calendario_editar.html:127`.) |
| Escape / clique no fundo | Equivalem a Cancelar. Clique dentro da caixa não satisfaz `e.target === fundo`. (`app/templates/core/_dialogo.html:159`; `app/templates/core/_dialogo.html:161`.) |
| Repetir aberto → Salvar, havendo vazias | A confirmação de Salvar cancela a Promise de Repetir. “Salvar assim mesmo” salva o estado anterior do alvo, sem executar a cópia cancelada. (`app/templates/escala/folga_calendario_editar.html:149`; `app/templates/escala/folga_calendario_editar.html:155`; `app/templates/core/_dialogo.html:187`.) |
| Repetir aberto → Salvar, sem vazias | O submit segue sem nova confirmação e sem encerrar explicitamente a repetição pendente; naquele momento o alvo ainda contém os dias anteriores. (`app/templates/escala/folga_calendario_editar.html:151`; `app/templates/escala/folga_calendario_editar.html:133`.) |
| Dois Repetir seguidos | Segundo alvo preenchido: a segunda pergunta cancela a primeira. Segundo alvo vazio: copia imediatamente e mantém a primeira pergunta pendente. (`app/templates/core/_dialogo.html:187`; `app/templates/escala/folga_calendario_editar.html:130`.) |
| Troca de modo com Repetir aberto | `fcModo` não cancela a Promise. Indo para `semana_dia`, apaga os campos; confirmar depois recria `dias_*` pela sincronização incondicional. A view os ignora nesse modo. (`app/templates/escala/folga_calendario_editar.html:128`; `app/templates/escala/folga_calendario_editar.html:185`; `app/templates/escala/folga_calendario_editar.html:189`; `app/escala/views.py:327`.) |
| Alvo marcado pelo servidor | Entra no mesmo `Set` e recebe a mesma confirmação; o teste novo cobre Cancelar com seed `[8]`. (`app/templates/escala/folga_calendario_editar.html:79`; `app/templates/escala/folga_calendario_editar.html:130`; `app/escala/tests/test_smoke_clique_folga_calendario.py:235`; `app/escala/tests/test_smoke_clique_folga_calendario.py:293`.) |

O overlay intercepta o mouse, mas o componente não implementa confinamento de Tab nem `inert`; apenas declara `aria-modal` e focaliza o Sim. Portanto, as sequências concorrentes precisam distinguir chamada programática/teclado de clique através do fundo. Reprodução por teclado nesta auditoria: **[não verificado]**. (`app/templates/core/_dialogo.html:112`; `app/templates/core/_dialogo.html:120`; `app/templates/core/_dialogo.html:160`; `app/templates/core/_dialogo.html:195`.)

**3. Quebra de linha**

O `'\n'` do JavaScript produz LF; `hxConfirmar` o insere por `textContent`. O CSS aplicável ao texto é inline: **`white-space:pre-line`**, que preserva essa quebra. A caixa tem `max-width:420px`, podendo acrescentar quebras por largura. (`app/templates/escala/folga_calendario_editar.html:153`; `app/templates/core/_dialogo.html:190`; `app/templates/core/_dialogo.html:117`; `app/templates/core/_dialogo.html:122`; `app/templates/core/_dialogo.html:123`.)

Barra+ene literais (`\\n`) passados diretamente à API não viram LF; os adaptadores de atributos fazem essa conversão expressamente. Com `white-space:normal/nowrap`, o LF seria colapsado; sem renderização da caixa, nenhuma quebra seria desenhada. O estilo presente na cópia já é `pre-line`; o texto de Salvar também já contém LF. (`app/templates/core/_dialogo.html:240`; `app/templates/core/_dialogo.html:259`; `app/templates/core/_dialogo.html:123`; `app/templates/escala/folga_calendario_editar.html:153`.)

**4. O que os testes medem** [nao verificado]

São testes com Chromium executando o JavaScript da página, cliques DOM e leitura do DOM resultante; não são apenas buscas no texto-fonte. Entretanto, os três novos casos de Repetir conferem campos e contador, sem POST nem inspeção da seleção visual/chips/canônico após a cópia. (`app/escala/tests/test_smoke_clique_folga_calendario.py:128`; `app/escala/tests/test_smoke_clique_folga_calendario.py:140`; `app/escala/tests/test_smoke_clique_folga_calendario.py:181`; `app/escala/tests/test_smoke_clique_folga_calendario.py:293`; `app/escala/tests/test_smoke_clique_folga_calendario.py:306`; `app/escala/tests/test_smoke_clique_folga_calendario.py:313`.)

Mutantes que as asserções deixam escapar, por inspeção — execução dos mutantes **[não verificado]**:

- Trocar `renderGrade(tam)` na cópia por atualização apenas do contador, mantendo `syncHidden`: os campos e a contagem ficam certos, mas a grade desenhada permanece antiga. (`app/templates/escala/folga_calendario_editar.html:128`; `app/escala/tests/test_smoke_clique_folga_calendario.py:120`; `app/escala/tests/test_smoke_clique_folga_calendario.py:140`.)
- Trocar o verbo por “Apagar”: o teste só rejeita vazio, “Confirmar” e “OK”. (`app/escala/tests/test_smoke_clique_folga_calendario.py:299`.)
- Esconder o texto por CSS: a sonda verifica `fundo.hidden`, não a visibilidade do texto; `.click()` também não exige que o botão esteja visível. (`app/escala/tests/test_smoke_clique_folga_calendario.py:132`; `app/escala/tests/test_smoke_clique_folga_calendario.py:137`.)

`innerText.split('\n')` distingue LF preservado de LF colapsado **quando o texto está renderizado**, mas não conta as quebras automáticas por largura. Em elemento sem renderização, `innerText` pode devolver o texto-fonte. Logo, “duas entradas” não prova “duas linhas desenhadas”; faltam geometria e visibilidade. (`app/escala/tests/test_smoke_clique_folga_calendario.py:107`; `app/escala/tests/test_smoke_clique_folga_calendario.py:278`; `app/escala/tests/test_smoke_clique_folga_calendario.py:301`.)

Os timers amostram após 200 ms, com orçamento virtual de 4000 ms e timeout real de 120 s. Hoje a abertura é síncrona e a resposta roda por Promise; **não há corrida concreta demonstrada por esses 200 ms**. Existe fragilidade a futuras esperas, pois não se aguarda uma condição. Não identifiquei dependência entre a ordem dos métodos: usam `TestCase` e um processo Chromium por chamada. (`app/escala/tests/test_smoke_clique_folga_calendario.py:131`; `app/escala/tests/test_smoke_clique_folga_calendario.py:139`; `app/escala/tests/test_smoke_clique_folga_calendario.py:183`; `app/escala/tests/test_smoke_clique_folga_calendario.py:224`; `app/templates/core/_dialogo.html:188`.)

**5. L7, L8 e componente único**

O editor chama `hxConfirmar`; não introduz diálogo nativo nem outra caixa. O componente vem da base e é compartilhado com o guarda de Salvar. (`app/templates/escala/folga_calendario_editar.html:131`; `app/templates/escala/folga_calendario_editar.html:155`; `app/templates/base.html:388`.)

Há mensagem nova hardcoded no template, dentro do componente. Portanto, usar o componente atende L8, mas sozinho não demonstra o requisito de origem em mapa de L7. O repositório já registra como **fila, não bloqueante**, a questão equivalente dos textos fixos de vazio/Salvar; classifico a mensagem nova no mesmo ponto limítrofe, sem presumir uma exceção nova. (`app/templates/escala/folga_calendario_editar.html:131`; `app/docs/LEIS-UI.md:78`; `app/docs/LEIS-UI.md:88`; `app/docs/BACKLOG.md:822`.)

**6. Outros chamadores**

O censo encontrou a definição e os botões do editor, além dos testes; nenhum chamador funcional externo apareceu na cópia. O smoke anterior repete em 30 e 28 inicialmente vazios, preservando a cópia imediata; o teste estrutural exige os botões, sem exigir sobrescrita silenciosa. (`app/templates/escala/folga_calendario_editar.html:51`; `app/templates/escala/folga_calendario_editar.html:124`; `app/escala/tests/test_smoke_clique_folga_calendario.py:154`; `app/escala/tests/test_editor_calendario_tres_grades.py:114`.)

**Achados consolidados**

- **Fila — confirmação pendente sobrevive à troca de modo e repõe `dias_*` em `semana_dia`.** Inconsistência do formulário; gravação errada não demonstrada porque a view ignora esses campos. (`app/templates/escala/folga_calendario_editar.html:128`; `app/templates/escala/folga_calendario_editar.html:189`; `app/escala/views.py:327`.)
- **Fila — testes não certificam desenho nem visibilidade; “linhas desenhadas” superafirma a medida.** (`app/escala/tests/test_smoke_clique_folga_calendario.py:107`; `app/escala/tests/test_smoke_clique_folga_calendario.py:140`; `app/escala/tests/test_smoke_clique_folga_calendario.py:278`.)
- **Fila — faltam casos de Escape, fundo, concorrência e troca de modo nos novos testes.** Os novos casos cobrem Cancelar com seed, Substituir e alvo vazio. (`app/escala/tests/test_smoke_clique_folga_calendario.py:293`; `app/escala/tests/test_smoke_clique_folga_calendario.py:306`; `app/escala/tests/test_smoke_clique_folga_calendario.py:313`.)
- **Fila — texto fixo/L7**, conforme classificação já registrada para essa classe de mensagem. (`app/templates/escala/folga_calendario_editar.html:131`; `app/docs/BACKLOG.md:822`.)

## 16/11 17:36 · sessao 406f60771e63 · - · - · origem=Ronald (tmux)

- `hxConfirmar` é único: cria uma Promise e guarda seu resolver em `pendente`; ao receber outra confirmação, resolve a anterior com `false`. `fechar` zera `pendente`, esconde o fundo, restaura foco e resolve a Promise. Também `hxPerguntar` compartilha esse estado. Referências: `app/templates/core/_dialogo.html:146`, `:149`, `:187`, `:188`, `:194`, `:202`, `:210`.

- Cancelar, Escape e clique exatamente no fundo resolvem `false`; portanto não executam `copia`. Substituir resolve `true`, copia a seleção **atual** da grade de 31 e sincroniza seleção, desenho e inputs. Referências: `app/templates/core/_dialogo.html:157`, `:158`, `:159`, `:161`; `app/templates/escala/folga_calendario_editar.html:126`, `:127`, `:128`, `:133`.

- Repetir seguido de Salvar: havendo grades vazias, o guarda chama novamente `hxConfirmar`, cancela a repetição anterior e mostra a confirmação de salvar; sem grades vazias, o submit segue sem cancelar a repetição pendente. Referências: `app/templates/escala/folga_calendario_editar.html:149`, `:151`, `:155`; `app/templates/core/_dialogo.html:187`.

- Dois repetir: se o segundo alvo estiver preenchido, a segunda pergunta cancela a primeira; se estiver vazio, copia imediatamente e mantém a primeira promessa pendente. Referências: `app/templates/escala/folga_calendario_editar.html:130`, `:131`; `app/templates/core/_dialogo.html:187`.

- Essas sequências não são cliques comuns através do modal: o fundo ocupa a janela com `z-index:100000`. Contudo, o componente declara `aria-modal`, focaliza o Sim e trata Escape, sem implementar bloqueio por `inert` ou confinamento de Tab; alcançar controles ao fundo por teclado é inferência do código, não reproduzida no navegador. Referências: `app/templates/core/_dialogo.html:112`, `:120`, `:160`, `:195`; reprodução: **[não verificado]**.

- **Fila:** `fcModo` com repetição pendente não cancela a promessa. Mudar para `semana_dia` limpa `#fc-hidden`; confirmar Substituir depois repõe `dias_*`, pois `copia` chama `syncHidden` incondicionalmente. A view ignora esses campos no ramo `semana_dia`, portanto isso demonstra inconsistência dos inputs, sem demonstrar gravação errada. Referências: `app/templates/escala/folga_calendario_editar.html:128`, `:185`, `:189`; `app/escala/views.py:327`, `:339`, `:342`.

- O LF produzido pelo `'\n'` JavaScript é inserido por `textContent` e respeitado pelo `white-space:pre-line` inline de `#hx-confirma-texto`. A caixa ainda pode produzir quebras adicionais por largura, pois tem `max-width:420px; width:100%`. Uma sequência literal barra+ene recebida diretamente por `hxConfirmar` não é convertida; os adaptadores fazem essa conversão explicitamente. Referências: `app/templates/escala/folga_calendario_editar.html:131`, `:153`; `app/templates/core/_dialogo.html:116`, `:122`, `:123`, `:190`, `:239`, `:240`, `:259`.

- Mutante relevante: esconder **somente** `#hx-confirma-caixa` por CSS preservaria `fundo.hidden === false`; as sondas continuam clicando por `.click()` e não medem a visibilidade da caixa. `innerText` de elemento sem renderização pode devolver o texto-fonte, invalidando a alegação de “linhas desenhadas”. Referências da insuficiência: `app/escala/tests/test_smoke_clique_folga_calendario.py:107`, `:128`, `:132`, `:134`, `:137`, `:140`; execução do mutante: **[não verificado]**.

- O censo por `fcRepetir` encontrou a definição e os botões do próprio editor, mais testes de estrutura e smoke; nenhum chamador funcional externo apareceu. Referências: `app/templates/escala/folga_calendario_editar.html:51`, `:124`; `app/escala/tests/test_editor_calendario_tres_grades.py:117`, `:119`; `app/escala/tests/test_smoke_clique_folga_calendario.py:128`, `:156`, `:157`.

## 16/11 17:36 · sessao 0a6149363538 · - · - · origem=Ronald (tmux)

- **Fila — os testes exercitam navegador, mas não conferem a grade desenhada após repetir.** Chromium executa a página; a sonda clica e lê inputs escondidos/contador. Os novos casos não reenviam POST. Mutante que passaria: substituir `renderGrade(tam)` em `copia()` por atualização apenas do contador, mantendo `syncHidden()`; chips, seleção visual, aviso de vazia e canônico ficariam antigos. Referências: `app/escala/tests/test_smoke_clique_folga_calendario.py:119`, `:128`, `:140`, `:176`, `:293`, `:306`, `:313`; `app/templates/escala/folga_calendario_editar.html:93`, `:95`, `:104`, `:106`, `:128`. Mutante não executado: [não verificado].

- **Fila — “linhas desenhadas” é uma descrição excessiva.** A medida divide `innerText` por LF; não mede retângulos de texto nem quebra automática por largura. Para elemento renderizado, distingue LF preservado de LF colapsado pelo CSS; não demonstra quantidade de linhas visuais nem visibilidade efetiva. A sonda verifica somente `fundo.hidden`; asserções exigem duas entradas. Referências: `app/escala/tests/test_smoke_clique_folga_calendario.py:102`, `:107`, `:132`, `:278`, `:301`. Sem execução de navegador nesta auditoria: [não verificado].

- **Fila — verbo incorreto passa.** Trocar `Substituir` por `Apagar` satisfaria a asserção, que apenas rejeita vazio, `Confirmar` e `OK`; o caso positivo não exige o verbo. Referências: `app/templates/escala/folga_calendario_editar.html:132`; `app/escala/tests/test_smoke_clique_folga_calendario.py:299`, `:306`. Mutante não executado: [não verificado].

- **Timing:** a sonda amostra após 200 ms e novamente após outros 200 ms; Chromium recebe orçamento virtual de 4000 ms e timeout real de 120 s. Não espera condição de conclusão. Na implementação atual a abertura é síncrona e a atualização vem da promessa; corrida concreta causada pelos 200 ms não foi demonstrada. Referências: `app/escala/tests/test_smoke_clique_folga_calendario.py:131`, `:139`, `:183`, `:185`; `app/templates/escala/folga_calendario_editar.html:131`, `:133`; `app/templates/core/_dialogo.html:188`, `:193`, `:194`.

- **Cobertura ausente nos novos casos:** Escape, fundo, dois pedidos concorrentes, Salvar durante Repetir e mudança de modo durante o modal. Os três casos novos cobrem Cancelar com seed, aceitar sobre alvo clicado e copiar sobre alvo vazio. Referências: `app/escala/tests/test_smoke_clique_folga_calendario.py:293`, `:306`, `:313`.

- **Não executei os testes:** o próprio runner cria/remove arquivos e os casos criam registros; isso contraria a restrição de somente leitura. Referências: `app/escala/tests/test_smoke_clique_folga_calendario.py:178`, `:180`, `:187`, `:227`, `:259`.

## 16/11 17:36 · sessao ecf376ec9c03 · - · - · origem=Ronald (tmux)

- **Estado cliente:** `sel[tam]` e `sel[31]` nascem dos `marcados` entregues pela view; a inicialização deduplica com `Set`. Escrevem `fcTog` e `copia`; leem `ordenados`, `renderGrade`, `syncHidden`, o teste de alvo vazio e o guarda de submit. `sel[31]` continua sendo a origem lida **no momento da confirmação**, sem snapshot. (`app/escala/views.py:362`; `app/templates/escala/folga_calendario_editar.html:74`, `:79`, `:82`, `:86`, `:113`, `:119`, `:127`, `:130`, `:149`.)

- **Inputs:** `#fc-hidden` é um `div` escondido; os campos gerados são `input type="checkbox" checked`, não `type="hidden"`. `syncHidden` recria todos com base em `sel`; `renderTudo`, `fcTog`, `copia` e `fcModo` chamam a sincronização. `fcModo` em semana/dia esvazia o DOM, mas conserva `sel`. (`app/templates/escala/folga_calendario_editar.html:62`, `:109`, `:110`, `:114`, `:117`, `:119`, `:128`, `:185`, `:189`.)

- **Contador:** o servidor escreve inicialmente `t.marcados|length`; o JS substitui pela quantidade de `ordenados(tam)`. Não há consumidor de domínio lendo `fc-conta`; os novos testes leem o texto como testemunha. (`app/templates/escala/folga_calendario_editar.html:46`, `:94`, `:95`; `app/escala/tests/test_smoke_clique_folga_calendario.py:123`, `:303`, `:311`.)

- **POST e persistência:** em `dias_mes`, a view lê cada `dias_<tam>` por `getlist`, descarta valor não inteiro ou fora de `1..tam` e sempre monta os três tamanhos; ausência de input vira lista vazia. A porta salva listas ordenadas sem duplicatas por `update_or_create`. Não existe preenchimento automático pela grade de 31 nesse fluxo. Em semana/dia a view ignora todos os `dias_*`, e a porta apaga os tamanhos. (`app/escala/views.py:327`, `:340`, `:342`, `:347`, `:349`, `:356`; `app/escala/folgas.py:584`, `:588`, `:593`, `:595`.)

- **“Vazia” fora do diff:** o resolver trata tamanho ausente e lista vazia igualmente: nenhuma folga; fevereiro de 29 dias consulta a grade de 28. O vigia mede “vazio” por nenhuma **data válida produzida no mês**, não por tamanho do conjunto bruto. Portanto a definição coincide para os dias válidos produzidos pela UI, mas diverge teoricamente se o servidor contiver dias inválidos: a view entrega `set(t.dias)` sem filtrar, enquanto POST e resolver filtram. Existência de dados inválidos no dado fornecido: **[não verificado]**. (`app/escala/folgas.py:12`, `:14`, `:15`, `:127`; `app/escala/views.py:347`, `:362`; `app/escala/models.py:1570`.)

- **Outros leitores:** `FolgaCalendario.dias_por_tamanho` entrega os valores persistidos ao resolver; a listagem exibe somente os tamanhos existentes. A projeção do vínculo, a foto, a divergência e o vigia leem `datas_que_o_calendario_manda`, que escolhe o modo e o resolver. (`app/escala/models.py:1558`; `app/escala/views.py:292`; `app/escala/folgas.py:43`, `:54`, `:56`, `:99`, `:127`, `:158`, `:629`; `app/colaboradores/views.py:304`.)

- **Chamadores de `fcRepetir`:** a busca global encontrou o botão do próprio template, sua definição e referências de testes; nenhum outro chamador de aplicação foi encontrado na cópia. Os testes antigos de smoke chamam 30 e 28 sobre alvos inicialmente vazios, preservando a cópia imediata; o contrato estático só exige existência dos botões. (`app/templates/escala/folga_calendario_editar.html:51`, `:124`; `app/escala/tests/test_smoke_clique_folga_calendario.py:154`, `:156`, `:157`; `app/escala/tests/test_editor_calendario_tres_grades.py:114`.)

- **L7/L8:** o novo texto é literal no template e passa ao componente da casa; não há chamada nativa nem novo componente no editor. L7 exige simultaneamente ajuda em tooltip/componente e origem no mesmo mapa do juiz; não há mapa introduzido pelo diff. Tratar a mensagem de confirmação como “texto de ajuda” e bloquear por L7 requer interpretação: o aval dado exige explicitamente confirmação pelo componente. (`app/templates/escala/folga_calendario_editar.html:131`, `:132`, `:155`; `app/docs/LEIS-UI.md:77`, `:78`, `:87`.)

## 16/11 17:16 · sessao 8bdc7eb4e863 · E4C-F1-B2 · censo · origem=Code

A cópia contém o juiz da B1; os leitores B/C/D ainda conservam os predicados anteriores, inclusive a property `respondida`. A B2 permanece por construir nos documentos. (`app/chamados/juizes.py:1544`; `app/chamados/models.py:1922`; `app/chamados/models.py:3664`; `app/docs/LEIS.md:151`.)

Vínculo independente da árvore com o SHA informado, execução dos testes, confirmação dos registros de produção e medida de efeitos da B2: **[nao verificado]**. [nao verificado]

**1. CENSO**

Adoto `I` = instante presente, `T` = texto não vazio após trim e `J` = `I E T`, conforme o juiz atual. (`app/chamados/juizes.py:1526`; `app/chamados/juizes.py:1552`.)

Nos quadros, “cópia” significa que o sítio ainda decide pelo campo; “delegado” significa que herda o predicado do corpo indicado. O corpo B continua usando OU, e o comentário do juiz reserva a migração B/C/D à B2. (`app/chamados/models.py:1924`; `app/chamados/juizes.py:1551`.)

| R do anexo | B — estado nesta cópia | Evidência atual |
|---|---|---|
| 24 | Irmãs delegam a `sem_veredito_entre`; ainda OU | `app/chamados/juizes.py:2105` |
| 27 | Auditor I12 delega às irmãs | `app/chamados/management/commands/auditar_invariantes_chamados.py:984` |
| 38 | Comando delega à lista sem veredito | `app/chamados/management/commands/validar_classe_a.py:54` |
| 42 | **Corpo da regra:** sem validação/materialização, `I OU T`, com tranca opcional | `app/chamados/models.py:1922`; `app/chamados/models.py:1926` |
| 43 | `respostas_sem_veredito` delega ao corpo acima | `app/chamados/models.py:1906` |
| 44 | `pode_fechar` consulta a lista e a ausência de fio vivo | `app/chamados/models.py:1934` |
| 45 | `fechar` consulta a lista | `app/chamados/models.py:1967` |
| 60 | Retenção da retratação delega às irmãs | `app/chamados/reconciliador.py:448` |
| 62 | Encerramento de órfãs consulta a lista | `app/chamados/reconciliador.py:1164` |
| 69 | Medição da tranca consulta a lista ignorando tranca | `app/chamados/services/competencia_trancada.py:126` |
| 80 | Resolvedor do chamado consulta as irmãs | `app/chamados/services/disputa_emissao.py:2031` |
| 85 | Gaveta consulta a lista | `app/chamados/services/fila_por_causa.py:226` |
| 104 | Reconciliação do destino consulta as irmãs | `app/chamados/services/materializacao.py:768` |
| 133 | Desligamento consulta a lista | `app/colaboradores/services/desligamento.py:165` |
| 134 | Ficha consulta a lista dentro das vivas | `app/colaboradores/services/ficha.py:169` |
| 142 | CSF consulta o mesmo corpo nos universos pai/destino, ignorando tranca | `app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574` |

| R do anexo | C — estado nesta cópia: ainda só instante | Evidência atual |
|---|---|---|
| 4 | Cópia: seleção de respondidas no histórico da API | `app/api/views.py:2577` |
| 5 | Cópia: `ja_respondida` na resposta da API | `app/api/views.py:2620` |
| 18 | Cópia: corrigíveis, com filtro adicional de lastro/vitalidade | `app/chamados/juizes.py:1744` |
| 25 | Cópia: contraprova I21a | `app/chamados/management/commands/auditar_invariantes_chamados.py:177` |
| 26 | Cópia: I2 | `app/chamados/management/commands/auditar_invariantes_chamados.py:592` |
| 30 | Cópia: C2 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1605` |
| 49 | Cópias: pendente/respondida dentro de `estado`, após suas precedências | `app/chamados/models.py:3155`; `app/chamados/models.py:3158` |
| 56 | **Property ainda não delega:** `respondida_em is not None` | `app/chamados/models.py:3664`; `app/chamados/models.py:3665` |
| 58 | Reconstrução histórica: instante permite criar ato RESPOSTA; texto é copiado | `app/chamados/models.py:4091`; `app/chamados/models.py:4098` |
| 64 | Cópia: conta pendentes na reabertura | `app/chamados/services/acoes_disputa.py:95` |
| 65 | Cópia: viva sem instante impede regularizar | `app/chamados/services/acoes_disputa.py:130` |
| 67 | Cópia: classificação de beco | `app/chamados/services/becos.py:30` |
| 71 | Cópia: instante informa reenvio | `app/chamados/services/declaracao_texto.py:419` |
| 72 | Cópia: falta de instante dispara gravação da declaração | `app/chamados/services/declaracao_texto.py:687` |
| 76 | Cópia: contador da recobrança | `app/chamados/services/disputa_emissao.py:1735` |
| 77 | Cópia: instante presente impede reabertura do credor | `app/chamados/services/disputa_emissao.py:1843` |
| 89 | Cópia: viva sem instante impede regularização pelo fio | `app/chamados/services/fio.py:698` |
| 94 | Histórico: instante e limite `desde` selecionam evento | `app/chamados/services/historico.py:99`; `app/chamados/services/historico.py:105`; `app/chamados/services/historico.py:106` |
| 107 | Cópia SQL: materialização exige instante e validação, sem materialização | `app/chamados/services/materializacao.py:1375` |
| 111 | Cópia: pendente no app exige instante nulo e disputa aberta | `app/chamados/services/pergunta_no_app.py:81` |
| 116 | Cópia: validar devolve `sem_resposta` sem instante | `app/chamados/services/validacao.py:418` |
| 117 | Cópia: recusar devolve `ja_pendente` sem instante | `app/chamados/services/validacao.py:605` |
| 121 | Contador temporal: instante nos últimos sete dias e posterior à cobrança | `app/chamados/services/verbos.py:335`; `app/chamados/services/verbos.py:342`; `app/chamados/services/verbos.py:345` |
| 123 | Cópia: causa `c` do vigia, sob âncora terminal | `app/chamados/services/vigia_fio.py:636` |
| 128 | Cópia: `ja_respondida` na view | `app/chamados/views.py:1280` |
| 129 | Cópia: reabertura seleciona instante nulo, depois C6 | `app/chamados/views.py:1416`; `app/chamados/views.py:1417` |
| 130 | Cópias: instante presente para validar; nulo para aguardar | `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:176` |
| 137 | Cópia SQL: relavrar órfã seleciona instante presente | `app/ponto/management/commands/reconciliar_grade.py:461` |
| 143 | Cópias: contadores de instante nulo/presente | `bin/censo_folha.py:28`; `bin/censo_folha.py:29` |
| 148 | Cópias: instante governa rótulo e caixa do cartão | `app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29` |
| 150 | Cópias: instante governa correção da cega e caixa no modal | `app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217` |

| R do anexo | D — estado nesta cópia: ainda só texto | Evidência atual |
|---|---|---|
| 8 | Cópia SQL: vazio exato, sem trim | `app/chamados/canal.py:65` |
| 31 | Cópia: texto após strip interrompe F1 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1657` |
| 34 | Alimentação da eleição de canônica: `.only()` fornece texto | `app/chamados/management/commands/backfill_decisoes_pergunta.py:504`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:506` |
| 37 | Cópia: texto após strip; depois eleição de canônica | `app/chamados/management/commands/dedup_perguntas_fato.py:75`; `app/chamados/management/commands/dedup_perguntas_fato.py:76` |
| 40 | Cópia SQL: ressuscitação seleciona texto NULL ou vazio exato | `app/chamados/models.py:1533` |
| 59 | Cópia: texto impede lavrar sem turno | `app/chamados/reconciliador.py:365` |
| 61 | Cópia: texto impede o ramo de órfã | `app/chamados/reconciliador.py:1083` |
| 74 | Cópia: texto existente impede resposta automática | `app/chamados/services/disputa_emissao.py:1663` |
| 81 | Cópia: `_tem_resp` pelo strip | `app/chamados/services/disputa_emissao.py:2079` |
| 90 | Corpo: canônica favorece texto | `app/chamados/services/fio.py:1271` |
| 91 | Corpo: mérito perdedor exige texto e via admin | `app/chamados/services/fio.py:1282`; `app/chamados/services/fio.py:1283` |
| 99 | Cópia: estado de aplicação exige tipo hora e texto | `app/chamados/services/materializacao.py:438` |
| 105 | Cópia: plantio de hora recusa texto vazio | `app/chamados/services/materializacao.py:835` |
| 126 | Cópia: leitura da resposta retorna vazia sem texto após strip | `app/chamados/templatetags/chamados_tags.py:142` |
| 132 | Cópia SQL: calendário exclui vazio exato | `app/colaboradores/services/calendario.py:504` |
| 135 | Cópia: pendência na ficha pelo texto vazio, fora de `falou` | `app/colaboradores/services/ficha.py:171` |
| 136 | Cópia: evidência de fala na prévia | `app/folha/previa.py:142` |
| 138 | Cópia: evidência de fala no cartório | `app/ponto/services/cartorio.py:1641` |
| 145 | Cópias: verdade crua do texto, sem strip | `app/templates/chamados/partials/_card_atendimento.html:141`; `app/templates/chamados/partials/_card_atendimento.html:182` |

O censo anterior **não fecha como enumeração exaustiva dos delegados**: faltam os sítios abaixo. Todos têm classe identificada pelo corpo chamado; não são novos predicados independentes. (`app/chamados/models.py:719`; `app/chamados/templatetags/chamados_tags.py:202`; `app/chamados/services/fio.py:1271`.)

| Classe | Sítios adicionais ao R1–R152 |
|---|---|
| B transitivo | Gate do pai → CSF: `app/chamados/models.py:719` → `app/ponto/supra_juiz.py:573` |
| C delegado | Contador de corrigíveis: `app/chamados/templatetags/chamados_tags.py:202` → `app/chamados/juizes.py:1744` |
| D delegado | Canônica: `app/chamados/services/declaracao_texto.py:253`; `app/chamados/services/declaracao_texto.py:275` |
| D delegado | Canônica: `app/chamados/services/fio.py:995`; `app/chamados/services/fio.py:1892`; `app/chamados/services/fio.py:2215` |
| D delegado | Canônica no backfill: `app/chamados/management/commands/backfill_decisoes_pergunta.py:276`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:393` |
| D delegado | Mérito perdedor: `app/chamados/services/fio.py:1327` |

Permanecem abertas as fronteiras de migração de três C históricos: reconstrução da trilha, evento temporal e contador posterior à cobrança. A classificação “só instante” não resolve qual parte desses contratos representa “está respondida?”. (`app/chamados/models.py:4098`; `app/chamados/services/historico.py:105`; `app/chamados/services/verbos.py:342`.) Cobertura exaustiva de despacho dinâmico: **[nao verificado]**.

As leituras novas da B1 sobre texto sem carimbo pertencem a E/integridade; a leitura de `ato_texto` pela tarefa é leitura da sequência, não desses campos projetados. (`app/chamados/juizes.py:1537`; `app/chamados/services/fio.py:1604`; `app/chamados/services/tarefa_do_dia.py:159`.)

**2. TABELA DE ESTADOS**

A tabela descreve o código atual. `validar_pergunta` e `recusar_pergunta` são portas diferentes, com precedências diferentes. (`app/chamados/services/validacao.py:398`; `app/chamados/services/validacao.py:562`.)

| Situação da pergunta | Quem julga | O que a porta faz | O que persiste | Estado retornado | O que a tela mostra |
|---|---|---|---|---|---|
| Validar: ID inválido ou fora da disputa informada | `validar_pergunta`, `app/chamados/services/validacao.py:410` | Recusa antes dos demais testes | Nenhuma escrita nesse ramo | `nao_encontrada`, `app/chamados/services/validacao.py:416` | “Pergunta inválida”, `app/chamados/services/acoes_disputa.py:24` |
| Validar: **sem I**, com ou sem T, inclusive se já validada | `validar_pergunta`, `app/chamados/services/validacao.py:418` | Recusa **antes** de testar validação existente | Nenhuma escrita nesse ramo | `sem_resposta` | “Aguardando resposta do colab”, `app/chamados/services/acoes_disputa.py:26` |
| Validar: I presente e validação existente | `validar_pergunta`, `app/chamados/services/validacao.py:421` | Não repete carimbo | Conserva o registro | `ja_validada` | Informação “Pergunta já validada”, `app/chamados/services/acoes_disputa.py:28` |
| Validar: I presente, implausível, sem confirmação | `motivo_implausivel_validacao` → `validar_pergunta`, `app/chamados/services/validacao.py:429` | Recusa antes de escrever | Nenhuma escrita nesse ramo | `implausivel` | Aviso com a frase do serviço, `app/chamados/services/acoes_disputa.py:30` |
| Validar: passou pelos testes anteriores e texto é `FOLGA_CONTESTA` | `_e_aceite_de_folga` → `_aceitar_a_folga`, `app/chamados/services/validacao.py:437` | Usa a porta de folga | Depende dos ramos seguintes | Depende dos ramos seguintes | A casca usa a mensagem e `ok` do serviço, `app/chamados/services/acoes_disputa.py:50` |
| Folga sem dia | `_aceitar_a_folga`, `app/chamados/services/validacao.py:295` | Recusa | Nenhuma escrita nesse ramo | `folga_sem_dia` | Mensagem de ausência de dia, `app/chamados/services/validacao.py:297` |
| Folga já declarada | `_aceitar_a_folga`, `app/chamados/services/validacao.py:305` | Carimba validação, encerra pergunta e pede fecho do fio | Validação/ato e carimbo de identidade; não redeclara o dia, `app/chamados/services/validacao.py:313` | `folga_ja_declarada`, `app/chamados/services/validacao.py:327` | “O dia já está declarado como FOLGA” |
| Folga recusada na checagem ou na escrita | `_aceitar_a_folga`, `app/chamados/services/validacao.py:330`; `app/chamados/services/validacao.py:361` | Recusa; a segunda recusa reverte o atomic | Pergunta conservada após a recusa | `folga_recusada`, `app/chamados/services/validacao.py:334`; `app/chamados/services/validacao.py:375` | Motivo da recusa |
| Folga aceita | `_aceitar_a_folga`, `app/chamados/services/validacao.py:345` | Valida, declara o dia e encerra a pergunta | Validação/ato, declaração e carimbo, `app/chamados/services/validacao.py:346`; `app/chamados/services/validacao.py:354` | `folga_declarada`, `app/chamados/services/validacao.py:392` | Mensagem do aceite |
| Validar: caminho comum após os testes de entrada | `_carimbar_validacao`, `app/chamados/services/validacao.py:460` | Carimba; depois materializa a pergunta HORA elegível e processa o lote da disputa | `validada_em/por` e ato VALIDACAO no atomic, `app/chamados/services/validacao.py:159`; `app/chamados/services/validacao.py:164` | Depende da materialização | Depende dos ramos seguintes |
| **I sem T**, tipo hora, sem carimbos anteriores | Entrada aceita por I; plantio recusa por falta de texto, `app/chamados/services/validacao.py:418`; `app/chamados/services/materializacao.py:835` | Valida, mas não planta hora; não satisfaz `_resposta_de_hora` para a devolução específica | Validação permanece; motivo de recusa pode ser salvo, `app/chamados/services/validacao.py:185`; `app/chamados/services/materializacao.py:1107` | `nao_gravou`, `app/chamados/services/validacao.py:527` | “Validação registrada, mas o ponto NÃO foi gravado”, `app/chamados/services/acoes_disputa.py:40` |
| HORA recusada, sem materialização, com motivo | `devolver_validacao_recusada`, `app/chamados/services/validacao.py:215` | Desfaz validação; pode reabrir disputa conforme parâmetro | Resposta conservada; validação nula, ato DEVOLUCAO e motivo, `app/chamados/services/validacao.py:224`; `app/chamados/services/validacao.py:230`; `app/chamados/services/validacao.py:242` | `nao_gravou`, com `devolvida`, `app/chamados/services/validacao.py:523` | “A validação não ficou; a pergunta segue com você”, `app/chamados/services/acoes_disputa.py:35` |
| Mesma devolução, mesmo estado e motivo | `devolver_validacao_recusada`, `app/chamados/services/validacao.py:245` | Não acrescenta evento de mudança | Conserva o estado resultante | `nao_gravou`, `app/chamados/services/validacao.py:498` | Aviso de devolução, `app/chamados/services/acoes_disputa.py:35` |
| Materialização gera efeito e passa pelo juiz/carimbo | `_materializar_uma`, `app/chamados/services/materializacao.py:1238`; `app/chamados/services/materializacao.py:1283` | Confirma materialização | Efeito e carimbo no atomic | `validada`, salvo ramos posteriores, `app/chamados/services/validacao.py:540` | “Pergunta validada e ponto gravado”, `app/chamados/services/acoes_disputa.py:62` |
| Materialização reutiliza batida | `_materializar_uma` separa reuso; serviço responde, `app/chamados/services/materializacao.py:1297`; `app/chamados/services/validacao.py:535` | Informa reuso | Carimbo do processamento; sem nova batida nesse reuso | `ja_no_espelho` | Mensagem de reuso do serviço, `app/chamados/services/acoes_disputa.py:52` |
| Já materializada, mas sem validação | `validar_pergunta`, `app/chamados/services/validacao.py:460`; `app/chamados/services/validacao.py:486` | Carimba validação; pula a unidade dessa pergunta; ainda processa o lote da disputa | Validação/ato; efeitos das irmãs dependem do lote, `app/chamados/services/validacao.py:494` | Depende dos resultados do lote, `app/chamados/services/validacao.py:513` | Tradução do resultado pela casca, `app/chamados/services/acoes_disputa.py:32` |
| Recusar: motivo vazio | `recusar_pergunta`, `app/chamados/services/validacao.py:590` | Recusa antes de buscar a pergunta | Nenhuma escrita | `sem_motivo` | Exige motivo, `app/chamados/services/validacao.py:593` |
| Recusar: pergunta já validada | `recusar_pergunta`, `app/chamados/services/validacao.py:602` | Recusa **antes** de testar I | Conserva validação/resposta | `ja_validada` | “Recusar não desfaz validação”, `app/chamados/services/validacao.py:604` |
| Recusar: sem I | `recusar_pergunta`, `app/chamados/services/validacao.py:605` | Não devolve novamente | Nenhuma escrita nesse ramo | `ja_pendente` | “Já está pendente com o colaborador” |
| Recusar: I presente, não validada, fora do fato | `fora_do_fato` → `recusar_pergunta`, `app/chamados/services/validacao.py:609` | Recusa antes de motivo, nota e push | Conserva a pergunta | `fora_do_fato` | Explica que não há o que devolver, `app/chamados/services/validacao.py:614` |
| Recusar: passou pelos testes anteriores | `recusar_pergunta` → `reabrir_para_correcao`, `app/chamados/services/validacao.py:623`; `app/chamados/services/validacao.py:628` | Zera o par pela porta e registra recusa | Par zerado, motivo e ato; depois nota, auditoria e tentativa de push, `app/chamados/services/validacao.py:633`; `app/chamados/services/validacao.py:637`; `app/chamados/services/validacao.py:648` | `recusada` | Pergunta volta à apresentação sem instante, `app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/modal_fio.html:318` |

A precedência da apresentação é outra: `estado` primeiro lê validação; depois vitalidade; depois materialização; nas saídas pendente/respondida usa só I. (`app/chamados/models.py:3151`; `app/chamados/models.py:3154`; `app/chamados/models.py:3156`; `app/chamados/models.py:3158`.) O cartão prioriza validação e depois I; o modal primeiro exige I para entrar na caixa de resposta, depois prioriza pendência admin e validação. (`app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/modal_fio.html:217`; `app/templates/chamados/partials/modal_fio.html:222`; `app/templates/chamados/partials/modal_fio.html:228`.)

As divergências e fronteiras abertas são:

| PERGUNTA DE DESENHO | Dois lados comprovados |
|---|---|
| Como compor J com a precedência de validação existente? Validar sem I retorna `sem_resposta`; `estado` pode apresentar `validada`/`resolvida`. | `app/chamados/services/validacao.py:418` ↔ `app/chamados/models.py:3151` |
| I sem T está fora da fila global, mas a chamada direta de validar passa pela entrada. | `app/chamados/juizes.py:1751` ↔ `app/chamados/services/validacao.py:418` |
| O mesmo par ilegal é “respondida” pela property e falso para J. | `app/chamados/models.py:3665` ↔ `app/chamados/juizes.py:1552` |
| Como compor a migração da partição com o corte posterior que mantém vazio fora do app **na F5**, com dono admin? | `app/chamados/services/pergunta_no_app.py:81` ↔ `app/docs/CORTES.md:487` |
| Migrar R58 para J deixaria de reconstruir RESPOSTA de I sem T; a prova atual da B1 exige conservar essa linha anterior. | `app/chamados/models.py:4098` ↔ `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:285` |
| Nos C temporais, qual parte migra: existência da resposta ou seleção pela data do evento/cobrança? | `app/chamados/services/historico.py:106` ↔ `app/chamados/services/verbos.py:342` |
| Qual tratamento recebe pergunta sem dia na medida

[truncado em 20000 de 27858 caracteres]

## 16/11 17:14 · sessao 49658203fa78 · E4C-F2 · leitura · origem=Code

Os leitores desta cópia continuam lendo a projeção: `estado`, `respondida`, `resposta_display`, `sem_veredito_entre` e `q_vivas`; não corresponde à migração descrita na coluna c3 da matriz (`app/chamados/models.py:3151`, `app/chamados/models.py:3665`, `app/chamados/models.py:3688`, `app/chamados/models.py:1923`, `app/chamados/models.py:3496`).

Caminhos abaixo relativos a `app/`. As situações são condicionais; ocorrência em produção e exibição efetivamente observada: [nao verificado]. “Tela” indica o comportamento do template **se a pergunta chegar ao seu universo**; o app filtra primeiro por SQL e depois pelo juiz (`chamados/services/pergunta_no_app.py:52`, `chamados/services/pergunta_no_app.py:61`).

| Situação | Quem julga — função:linha | O que a porta faz | O que persiste | Estado | O que a tela mostra |
|---|---|---|---|---|---|
| **M1a — validação presente, sem materialização** (`chamados/models.py:3151`) | `PerguntaDisputa.estado`: `chamados/models.py:3144`; C6: `chamados/juizes.py:1405` | Retorna antes de consultar vitalidade (`chamados/models.py:3151`) | Nada pela leitura (`chamados/models.py:3153`) | `validada`; C6 morta (`chamados/models.py:3153`; `chamados/juizes.py:1406`) | Card administrativo: “validada”; explicador ainda confere o efeito (`templates/chamados/partials/_pergunta_card.html:23`; `chamados/services/explicador.py:93`) |
| **M1b — validação e materialização presentes** (`chamados/models.py:3151`) | `PerguntaDisputa.estado`: `chamados/models.py:3144` | Aceita o carimbo sem perguntar à testemunha nesse ramo (`chamados/models.py:3153`) | Nada pela leitura (`chamados/models.py:3153`) | `resolvida` (`chamados/models.py:3153`) | Card colaborador: “Encerrado”, salvo pendência administrativa; explicador pode dizer “validada_sem_gravar” (`templates/chamados/partials/_card_atendimento.html:189`; `chamados/services/explicador.py:102`) |
| **M2 — sem validação, viva, sem instante de resposta** (`chamados/models.py:3154`) | `estado` → `pergunta_viva`: `chamados/models.py:3154`; `chamados/juizes.py:866` | Traduz a vida pelo instante, independentemente do texto (`chamados/models.py:3155`) | Nada pela leitura (`chamados/models.py:3155`) | `pendente` (`chamados/models.py:3155`) | Formulário; “Enviar resposta”; no app, pendente se disputa aberta (`templates/chamados/partials/_card_atendimento.html:77`; `templates/chamados/partials/_card_atendimento.html:161`; `chamados/services/pergunta_no_app.py:81`) |
| **M2 — sem validação, viva, com instante de resposta** (`chamados/models.py:3154`) | `PerguntaDisputa.estado`: `chamados/models.py:3144` | Traduz como respondida sem exigir texto nesse leitor (`chamados/models.py:3155`) | Nada pela leitura (`chamados/models.py:3155`) | `respondida` (`chamados/models.py:3155`) | API: `aguarda_admin=True`; card só anuncia “Resposta enviada” quando há instante **e** texto (`api/views.py:2553`; `chamados/services/fio.py:187`; `chamados/services/fio.py:357`; `templates/chamados/partials/_card_atendimento.html:100`) |
| **M3 — sem validação, morta, com qualquer carimbo de materialização** (`chamados/models.py:3156`) | `estado`, após C6: `chamados/models.py:3154` | Retorna resolvida; não restringe o carimbo a identidade (`chamados/models.py:3156`) | Nada pela leitura (`chamados/models.py:3157`) | `resolvida` (`chamados/models.py:3157`) | Colaborador: “Encerrado”; cabeçalho administrativo sem validação/resposta: “aguardando colaborador” (`templates/chamados/partials/_card_atendimento.html:189`; `templates/chamados/partials/_pergunta_card.html:25`) |
| **M4 — morta, sem validação nem materialização** (`chamados/models.py:3158`) | `PerguntaDisputa.estado`: `chamados/models.py:3144` | Cai no instante de resposta, mesmo com C6 morta (`chamados/models.py:3158`) | Nada pela leitura (`chamados/models.py:3158`) | `pendente` sem instante; `respondida` com instante (`chamados/models.py:3158`) | Card morto, sem pendência administrativa, usa “Validado pela supervisão” quando estado não é resolvida (`templates/chamados/partials/_card_atendimento.html:179`; `templates/chamados/partials/_card_atendimento.html:189`) |
| **M3/M10 — testemunha de efeito verdadeira, mas sem carimbo** (`chamados/juizes.py:1482`) | C6: `_pergunta_viva_calc`: `chamados/juizes.py:1403` | Se alcançar o ramo sem carimbo, retorna viva **antes** da testemunha; não existe ali o retorno booleano de efeito descrito para c3 (`chamados/juizes.py:1482`; `chamados/juizes.py:1489`) | Nada de domínio; apenas memo em memória (`chamados/juizes.py:905`) | Sem validação/materialização, `estado` continua pendente/respondida pelo instante (`chamados/models.py:3155`; `chamados/models.py:3158`) | Explicador usa esses estados como “sem_resposta”/“aguarda_validacao” antes de consultar efeito (`chamados/services/explicador.py:73`; `chamados/services/explicador.py:77`) |
| **M5 — `respondida`, inclusive instante sem texto ou ato divergente** (`chamados/models.py:3664`) | `PerguntaDisputa.respondida`: `chamados/models.py:3664` | Testa somente `respondida_em`; não lê a sequência (`chamados/models.py:3665`) | Nada pela leitura (`chamados/models.py:3665`) | Booleano: instante presente → verdadeiro (`chamados/models.py:3665`) | Card administrativo abre o bloco de resposta pelo mesmo instante; app lista histórico “respondidas” também pelo instante (`templates/chamados/partials/_pergunta_card.html:29`; `api/views.py:2577`) |
| **M6 — texto atual difere do texto do ato** (`chamados/models.py:3688`; `chamados/models.py:4042`) | `PerguntaDisputa.resposta_display`: `chamados/models.py:3686` | Lê `resposta_colab`; vazio retorna `''`, fallback retorna o campo atual (`chamados/models.py:3688`; `chamados/models.py:3710`) | Nada pela leitura (`chamados/models.py:3710`) | Não altera `estado` (`chamados/models.py:3686`) | Resposta exibida vem desse display; API publica também o texto projetado (`templates/chamados/partials/_pergunta_card.html:31`; `api/views.py:2561`; `api/views.py:2562`) |
| **M7 — resposta de posto, escala ou opções** (`chamados/models.py:3690`) | `PerguntaDisputa.resposta_display`: `chamados/models.py:3686` | Posto → nome; escala → tipo; opção por `valor` → `rotulo`; senão texto (`chamados/models.py:3693`; `chamados/models.py:3701`; `chamados/models.py:3708`; `chamados/models.py:3710`) | Nada pela leitura (`chamados/models.py:3686`) | Não altera estado; rótulo concreto de posto/escala [nao verificado] (`chamados/models.py:3693`; `chamados/models.py:3701`) | Mostra o resultado do display; testes atuais ainda usam instâncias não salvas com projeção (`templates/chamados/partials/_pergunta_card.html:31`; `chamados/tests/test_valor_rotulo.py:7`; `chamados/tests/test_valor_rotulo.py:12`) |
| **M8 — validação presente na lista sem veredito** (`chamados/models.py:1923`) | `DisputaSupervisao.sem_veredito_entre`: `chamados/models.py:1910` | Exclui imediatamente (`chamados/models.py:1923`) | Nada pela leitura (`chamados/models.py:1922`) | Fora da lista; `estado` validada/resolvida conforme materialização (`chamados/models.py:1923`; `chamados/models.py:3153`) | A explicação do fechamento lê `pode_fechar`, que usa essa lista (`chamados/services/explicador.py:139`; `chamados/models.py:1934`; `templates/chamados/partials/modal_fio.html:436`) |
| **M9 — sem validação/materialização, instante OU texto, dia não trancado** (`chamados/models.py:1923`) | `DisputaSupervisao.sem_veredito_entre`: `chamados/models.py:1910` | Inclui; `fechar` recusa antes de julgar perguntas vivas, inclusive nas exceções (`chamados/models.py:1924`; `chamados/models.py:1967`) | Nada antes da recusa (`chamados/models.py:1969`) | Instante sem texto → respondida; texto sem instante → pendente (`chamados/models.py:3155`; `chamados/models.py:3158`) | Explicador atribui respondida ao admin e pendente ao colaborador; modal mostra essas duas mensagens (`chamados/services/explicador.py:73`; `chamados/services/explicador.py:77`; `templates/chamados/partials/modal_fio.html:440`) |
| **M9 — sem instante nem texto, sem validação/materialização** (`chamados/models.py:1924`) | Lista: `sem_veredito_entre`; fechamento: `pode_fechar` (`chamados/models.py:1910`; `chamados/models.py:1928`) | Exclui da lista; ainda pode bloquear por `fio_vivo` (`chamados/models.py:1924`; `chamados/models.py:1934`) | Nada pela leitura (`chamados/models.py:1934`) | `pendente`; vida depende de C6 (`chamados/models.py:3154`; `chamados/models.py:3158`) | Explicador: “Aguardando o COLABORADOR responder” (`chamados/services/explicador.py:74`; `templates/chamados/partials/modal_fio.html:440`) |
| **M10 — materialização presente, inclusive se pergunta ainda viva** (`chamados/models.py:1923`; `chamados/models.py:3154`) | `DisputaSupervisao.sem_veredito_entre`: `chamados/models.py:1910` | Exclui por qualquer carimbo; CSF recebe essa mesma exclusão (`chamados/models.py:1923`; `ponto/supra_juiz.py:573`) | Nada pela leitura (`chamados/models.py:1922`) | Sem validação: viva → pendente/respondida; morta → resolvida (`chamados/models.py:3154`; `chamados/models.py:3157`) | Se viva, formulário; se morta, “Encerrado”, salvo pendência administrativa (`templates/chamados/partials/_card_atendimento.html:77`; `templates/chamados/partials/_card_atendimento.html:189`) |
| **M11 — dia trancado por leitura, sem validação/materialização** (`chamados/juizes.py:1409`) | C6: `chamados/juizes.py:1403`; lista: `chamados/models.py:1910` | C6 mata; lista exclui normalmente, mas inclui resposta elegível com `ignorar_tranca=True`, usado pelo CSF (`chamados/juizes.py:1410`; `chamados/models.py:1926`; `ponto/supra_juiz.py:573`) | Nada pela leitura (`chamados/models.py:1922`; `chamados/juizes.py:980`) | `estado` ainda pendente/respondida por instante (`chamados/models.py:3158`) | App elimina pela leitura de C6; card morto, se renderizado, usa o ramo de encerramento (`chamados/services/pergunta_no_app.py:61`; `templates/chamados/partials/_card_atendimento.html:179`) |
| **M12 — pré-filtro SQL de perguntas vivas** (`chamados/models.py:3453`) | `PerguntaDisputa.q_vivas`: `chamados/models.py:3453` | Exige validação nula; exclui via terminal carimbada; combina resposta SQL com identidade/célula (`chamados/models.py:3483`; `chamados/models.py:3485`; `chamados/models.py:3496`) | Nada: produz `Q` (`chamados/models.py:3496`) | Candidato SQL, não veredito final; contrato declarado de continência (`chamados/models.py:3464`) | App aplica esse filtro e depois C6 (`chamados/services/pergunta_no_app.py:52`; `chamados/services/pergunta_no_app.py:61`) |
| **M13 — perguntas ancoradas num chamado** (`chamados/models.py:336`) | `ChamadoColaborador._ancoradas`: `chamados/models.py:336` | Sem PK → `[]`; com PK, busca validação nula e destino/âncora correspondente; não usa `q_vivas` nesse corpo (`chamados/models.py:341`; `chamados/models.py:344`) | Nada: retorna lista (`chamados/models.py:344`) | Ainda passa por competência e C6 em `_vivas_entre` (`chamados/models.py:349`) | Resultado alimenta guarda do chamado; não define rótulo próprio de tela (`chamados/models.py:223`; `chamados/models.py:226`) |
| **M14 — transição do chamado e relavratura** (`chamados/models.py:201`) | `ChamadoColaborador._gravar_estado`: `chamados/models.py:201` | Antes da escrita, recusa terminal com viva ancorada; após salvar, relavra destinos não validados quando há alvo (`chamados/models.py:221`; `chamados/models.py:296`; `chamados/models.py:303`; `chamados/models.py:310`) | Status/carimbos do chamado e, pelo lavrador, veredito das perguntas elegíveis (`chamados/models.py:228`; `chamados/models.py:296`; `chamados/models.py:312`) | Destino solicitado só segue após guarda; valores concretos [nao verificado] (`chamados/models.py:225`; `chamados/models.py:228`) | Essa guarda produz erro com perguntas vivas; apresentação concreta [nao verificado] (`chamados/models.py:226`) |
| **M15 — fechar normalmente com resposta sem veredito ou pergunta viva** (`chamados/models.py:1967`) | `DisputaSupervisao.fechar`: `chamados/models.py:1936` | Resposta sem veredito recusa primeiro; depois viva recusa quando `motivo=None` (`chamados/models.py:1967`; `chamados/models.py:1978`) | Nada antes dessas recusas (`chamados/models.py:1969`; `chamados/models.py:1980`) | Disputa não recebe fechamento desse caminho (`chamados/models.py:1985`) | Texto de erro distingue “RESPOSTA … sem veredito” e “pergunta(s) VIVA(s)” (`chamados/models.py:1970`; `chamados/models.py:1981`) |
| **M15 — exceção `admin_encerra`/`desligamento`, sem resposta sem veredito** (`chamados/models.py:1999`) | `DisputaSupervisao.fechar`: `chamados/models.py:1936` | Encerra vivas por admin; quando carimba, fotografa **a projeção** em SILENCIO (`chamados/models.py:2002`; `chamados/models.py:2007`) | Carimbo de identidade, DP SILENCIO e campos de fechamento da disputa (`chamados/models.py:3174`; `chamados/models.py:3300`; `chamados/models.py:2005`; `chamados/models.py:2017`) | Pergunta carimbada morta → resolvida; disputa fechada (`chamados/juizes.py:1444`; `chamados/models.py:3157`; `chamados/models.py:2013`) | Colaborador: “Encerrado — a supervisão decidiu o dia”; tarefa: `silencio_decidido`, sem ações (`templates/chamados/partials/_card_atendimento.html:189`; `templates/chamados/partials/_card_atendimento.html:192`; `chamados/services/tarefa_do_dia.py:176`) |
| **M15 — exceção `competencia_trancada`, sem resposta sem veredito** (`chamados/models.py:1848`; `chamados/models.py:1967`) | `DisputaSupervisao.fechar`: `chamados/models.py:1936` | Passa pela exceção, mas não entra no laço de carimbo/SILENCIO (`chamados/models.py:1978`; `chamados/models.py:1999`) | Campos de fechamento da disputa (`chamados/models.py:2013`; `chamados/models.py:2017`) | Não muda diretamente os campos da pergunta nesse ramo (`chamados/models.py:1999`; `chamados/models.py:2013`) | App continua dependente da tranca/C6; presença concreta [nao verificado] (`chamados/juizes.py:1409`; `chamados/services/pergunta_no_app.py:61`) |
| **M16 — remover motivo existente de disputa aberta** (`chamados/models.py:2114`) | `DisputaSupervisao.remover_motivo`: `chamados/models.py:2052` | Registra RETRATACAO com texto projetado; carimba identidade preservando carimbo anterior quando existente (`chamados/models.py:2137`; `chamados/models.py:2139`; `chamados/models.py:2142`) | DP, carimbo, motivos/contexto; tenta fechar se não restarem motivos (`chamados/models.py:2137`; `chamados/models.py:2146`; `chamados/models.py:2153`) | Retorna `removido` ou `fechada`; precondições retornam `disputa_fechada`/`motivo_ausente` (`chamados/models.py:2115`; `chamados/models.py:2118`; `chamados/models.py:2148`; `chamados/models.py:2154`) | Tarefa exclui pergunta fora do fato ou cuja última decisão é retratação (`chamados/services/tarefa_do_dia.py:256`; `chamados/services/tarefa_do_dia.py:260`) |
| **M17 — ressuscitação elegível** (`chamados/models.py:1593`) | `ChamadoColaborador.ressuscitar_perguntas`: `chamados/models.py:1560` | Conserva resposta/validação sem linha; zera carimbo, validação, instante **e texto**; não escreve REABERTURA (`chamados/models.py:1613`; `chamados/models.py:1622`; `chamados/models.py:1629`) | DP legadas faltantes, campos zerados, eventos; reabre disputas fechadas atingidas (`chamados/models.py:1613`; `chamados/models.py:1629`; `chamados/models.py:1632`; `chamados/models.py:1639`) | Projeção fica `pendente`; último SILENCIO pode continuar `silencio_decidido` no leitor do ato (`chamados/models.py:3158`; `chamados/services/tarefa_do_dia.py:176`) | Se viva e renderizada, oferece “Enviar resposta”; tarefa pode não exigir ação após SILENCIO (`templates/chamados/partials/_card_atendimento.html:77`; `templates/chamados/partials/_card_atendimento.html:161`; `chamados/services/tarefa_do_dia.py:177`) |
| **M18 — seleção/simulação da ressuscitação** (`chamados/models.py:1515`) | `ChamadoColaborador._universo_da_ressuscitacao`: `chamados/models.py:1515` | Seleciona materializadas sem texto, exclui fora do fato; pula marco comprovadamente aceso; simula campos zerados e consulta C6 (`chamados/models.py:1530`; `chamados/models.py:1550`; `chamados/models.py:1553`; `chamados/models.py:1557`) | Nada pela simulação; universo vazio faz a porta retornar `0` (`chamados/models.py:1558`; `chamados/models.py:1594`) | Separa elegíveis, puladas pelo marco e mortas por C6 (`chamados/models.py:1557`; `chamados/models.py:1558`) | Não produz rótulo de pergunta; a leitura auxiliar devolve os motivos do silêncio da porta (`chamados/models.py:1510`; `chamados/models.py:1513`) |
| **M19 — ramos de helper/prefetch/PK nula descritos para c3** (`chamados/models.py:3664`; `chamados/models.py:3686`) | Nos leitores atuais, `respondida` e display leem campos diretamente; correspondência com o helper c3 [nao verificado] (`chamados/models.py:3665`; `chamados/models.py:3688`) | Não aplicar automaticamente a hipótese “PK nula → lista de atos vazia” a esses corpos (`chamados/models.py:3665`; `chamados/models.py:3710`) | Nada por essas leituras; custo/N+1 da hipótese c3 [nao verificado] (`chamados/models.py:3665`; `chamados/models.py:3686`) | Resultado vem dos campos, inclusive em instância não salva dos testes de display (`chamados/tests/test_valor_rotulo.py:7`; `chamados/tests/test_valor_rotulo.py:12`) | Consome display projetado; efeito de prefetch c3 nesta tela [nao verificado] (`templates/chamados/partials/_pergunta_card.html:31`) |
| **M20 — selo AST c3 e alegações de testes vermelhos** (`chamados/tests/test_contract_dedup_status.py:143`) | O contrato atual inspeciona `sem_veredito_entre`; o selo F2 c3 nesta cópia [nao verificado] (`chamados/tests/test_contract_dedup_status.py:140`; `chamados/tests/test_contract_dedup_status.py:143`) | A expressão textual exigida continua no corpo atual; resultado por execução [nao verificado] (`chamados/tests/test_contract_dedup_status.py:143`; `chamados/models.py:1923`) | Asserções não são escrita de estado de domínio (`chamados/tests/test_contract_dedup_status.py:139`) | Veredito de suíte [nao verificado]; o teste atual ainda afirma materializada sem resposta → resolvida (`chamados/tests/test_pergunta_estado.py:50`; `chamados/tests/test_pergunta_estado.py:53`) | O selo não define apresentação; rótulo continua vindo do leitor/template (`chamados/models.py:3156`; `templates/chamados/partials/_card_atendimento.html:189`) |

**Precedência entre as situações:**

1. **`estado`:** validação presente → retorna validada/resolvida; senão consulta C6 → viva vira pendente/respondida; senão materialização presente → resolvida; senão instante → pendente/respondida. Portanto, M1 vence M2–M4; M2 vence o carimbo de M3 (`chamados/models.py:3151`, `chamados/models.py:3154`, `chamados/models.py:3156`, `chamados/models.py:3158`).

2. **Dentro de C6:** projeção temporal → validação → tranca por leitura → via terminal carimbada → resposta com instante e texto, considerando decisão administrativa/concordância com a luz → identidade carimbada → marco próprio aceso com carimbo → célula acusadora/absolvente → lei acusadora na janela → ausência de carimbo → testemunha; exceção na testemunha retorna morta (`chamados/juizes.py:1404`, `chamados/juizes.py:1405`, `chamados/juizes.py:1409`, `chamados/juizes.py:1416`, `chamados/juizes.py:1423`, `chamados/juizes.py:1431`, `chamados/juizes.py:1435`, `chamados/juizes.py:1444`, `chamados/juizes.py:1450`, `chamados/juizes.py:1456`, `chamados/juizes.py:1468`, `chamados/juizes.py:1471`, `chamados/juizes.py:1482`, `chamados/juizes.py:1489`, `chamados/juizes.py:1492`).

3. **Lista sem veredito:** validação nula → materialização nula → instante OU texto → tranca, salvo `ignorar_tranca`. `pode_fechar` testa essa lista antes de `fio_vivo`, por curto-circuito (`chamados/models.py:1923`, `chamados/models.py:1924`, `chamados/models.py:1926`, `chamados/models.py:1934`).

4. **`fechar`:** porta conhecida → disputa já fechada/idempotência → usuário → resposta sem veredito → perguntas vivas → escolha do fechamento normal/exceção → carimbo/SILENCIO nas duas exceções correspondentes → fechamento da disputa. A recusa por

[truncado em 20000 de 24133 caracteres]

## 16/11 17:12 · sessao c3391b5c4d44 · - · - · origem=Ronald (tmux)

O juiz atual é `app/chamados/juizes.py:1544`, corpo em `:1552` (`I E T`); o comentário de `:1551` reserva B/C/D ao B2. Os 66 usos B/C/D do censo conservam os predicados anteriores: nenhum deles chama diretamente `pergunta_respondida`; B delega ao antigo juiz OU, C/D consultam campo ou delegado antigo, conforme abaixo. (`app/chamados/juizes.py:1551`; `app/chamados/models.py:1922`; `app/chamados/models.py:3665`.)

Tabela completa, com os R do anexo e linhas atuais:

| R | Classe / situação atual | Referência atual |
|---|---|---|
| 24 | B delegado: irmãs → `sem_veredito_entre`, OU | `app/chamados/juizes.py:2105` |
| 27 | B delegado: auditor I12 → irmãs | `app/chamados/management/commands/auditar_invariantes_chamados.py:984` |
| 38 | B delegado: comando → respostas sem veredito | `app/chamados/management/commands/validar_classe_a.py:54` |
| 42 | B corpo: sem validação/materialização, `I OU strip(T)`, com tranca opcional | `app/chamados/models.py:1922`, `:1924`, `:1926` |
| 43 | B delegado: lista da disputa → corpo acima | `app/chamados/models.py:1906` |
| 44 | B delegado: `pode_fechar` → lista, junto com ausência de fio vivo | `app/chamados/models.py:1934` |
| 45 | B delegado: `fechar` → lista | `app/chamados/models.py:1967` |
| 60 | B delegado: retenção → irmãs | `app/chamados/reconciliador.py:448` |
| 62 | B delegado: encerrar órfãs → lista | `app/chamados/reconciliador.py:1164` |
| 69 | B delegado: medição da tranca → lista ignorando tranca | `app/chamados/services/competencia_trancada.py:126` |
| 80 | B delegado: resolvedor do chamado → irmãs | `app/chamados/services/disputa_emissao.py:2031` |
| 85 | B delegado: gaveta → lista | `app/chamados/services/fila_por_causa.py:226` |
| 104 | B delegado: reconciliação do destino → irmãs | `app/chamados/services/materializacao.py:768` |
| 133 | B delegado: desligamento → lista | `app/colaboradores/services/desligamento.py:165` |
| 134 | B delegado: ficha → lista, limitada às vivas | `app/colaboradores/services/ficha.py:169` |
| 142 | B delegado: CSF, universos pai e destino, ignorando tranca | `app/ponto/supra_juiz.py:573`, `:574` |
| 4 | C cópia: histórico respondidas exige instante | `app/api/views.py:2577` |
| 5 | C cópia: `ja_respondida` pelo instante | `app/api/views.py:2620` |
| 18 | C cópia: corrigíveis, instante e sem lastro/ou viva | `app/chamados/juizes.py:1744` |
| 25 | C cópia: contraprova I21a, falta instante | `app/chamados/management/commands/auditar_invariantes_chamados.py:177` |
| 26 | C cópia: I2, instante presente | `app/chamados/management/commands/auditar_invariantes_chamados.py:592` |
| 30 | C cópia: C2, instante nulo | `app/chamados/management/commands/auditar_invariantes_chamados.py:1605` |
| 49 | C cópia: estado pendente/respondida pelo instante, após precedências | `app/chamados/models.py:3151`, `:3155`, `:3158` |
| 56 | C cópia: property `respondida`, somente instante | `app/chamados/models.py:3664`, `:3665` |
| 58 | C cópia histórica: reconstrução cria RESPOSTA quando há instante; copia texto | `app/chamados/models.py:4091`, `:4098`, `:4100` |
| 64 | C cópia: contagem de pendentes pelo instante nulo | `app/chamados/services/acoes_disputa.py:95` |
| 65 | C cópia: viva sem instante impede regularizar | `app/chamados/services/acoes_disputa.py:130` |
| 67 | C cópia: beco sem instante e destino não resolvido | `app/chamados/services/becos.py:30` |
| 71 | C cópia: instante informa reenvio | `app/chamados/services/declaracao_texto.py:419` |
| 72 | C cópia: falta instante dispara resposta | `app/chamados/services/declaracao_texto.py:687` |
| 76 | C cópia: recobrança conta instante nulo | `app/chamados/services/disputa_emissao.py:1735` |
| 77 | C cópia: instante presente impede reabertura do credor | `app/chamados/services/disputa_emissao.py:1843` |
| 89 | C cópia: viva sem instante impede regularização no POST | `app/chamados/services/fio.py:698` |
| 94 | C cópia histórica: instante/marco selecionam evento | `app/chamados/services/historico.py:99`, `:105`, `:106` |
| 107 | C cópia: materialização seleciona instante + validação, sem materialização | `app/chamados/services/materializacao.py:1375`, `:1376` |
| 111 | C cópia: partição pendente por instante nulo e disputa aberta | `app/chamados/services/pergunta_no_app.py:81` |
| 116 | C cópia: validar recusa se falta instante | `app/chamados/services/validacao.py:418` |
| 117 | C cópia: recusar retorna já pendente se falta instante | `app/chamados/services/validacao.py:605` |
| 121 | C temporal: instante nos sete dias, depois da cobrança, com ramo hoje | `app/chamados/services/verbos.py:335`, `:336`, `:342`, `:345` |
| 123 | C cópia: vigia causa c, instante sob âncora terminal | `app/chamados/services/vigia_fio.py:636` |
| 128 | C cópia: view usa instante como `ja_respondida` | `app/chamados/views.py:1280` |
| 129 | C cópia: reabrir seleciona instante nulo, depois filtra C6 | `app/chamados/views.py:1416`, `:1417` |
| 130 | C cópia: validar inline seleciona instante; espera seleciona nulo | `app/chamados/views_cobrar.py:169`, `:176` |
| 137 | C cópia: relavrar órfã seleciona instante presente | `app/ponto/management/commands/reconciliar_grade.py:461` |
| 143 | C cópias: contadores instante nulo/presente | `bin/censo_folha.py:28`, `:29` |
| 148 | C cópias: rótulo e caixa pelo instante | `app/templates/chamados/partials/_pergunta_card.html:24`, `:29` |
| 150 | C cópias: correção da cega e caixa pelo instante | `app/templates/chamados/partials/modal_fio.html:185`, `:217` |
| 8 | D cópia SQL: vazio exato, sem trim | `app/chamados/canal.py:65` |
| 31 | D cópia: qualquer texto após strip interrompe F1 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1657` |
| 34 | D delegado: `.only` fornece texto à eleição de canônica | `app/chamados/management/commands/backfill_decisoes_pergunta.py:504`, `:506`; regra em `app/chamados/services/fio.py:1271` |
| 37 | D cópia: lista com texto após strip; também chama eleição canônica | `app/chamados/management/commands/dedup_perguntas_fato.py:75`, `:76` |
| 40 | D cópia SQL: universo de ressuscitação aceita texto NULL/vazio exato | `app/chamados/models.py:1533` |
| 59 | D cópia: texto impede lavrar sem turno | `app/chamados/reconciliador.py:365` |
| 61 | D cópia: texto impede veredito órfã | `app/chamados/reconciliador.py:1083` |
| 74 | D cópia: texto existente impede resposta automática | `app/chamados/services/disputa_emissao.py:1663` |
| 81 | D cópia: `_tem_resp` pelo strip | `app/chamados/services/disputa_emissao.py:2079` |
| 90 | D corpo: canônica favorece texto | `app/chamados/services/fio.py:1271` |
| 91 | D corpo: mérito perdedor exige texto e via admin | `app/chamados/services/fio.py:1282`, `:1283` |
| 99 | D cópia: estado de aplicação exige tipo hora + texto | `app/chamados/services/materializacao.py:438` |
| 105 | D cópia: materializador hora recusa texto vazio | `app/chamados/services/materializacao.py:835` |
| 126 | D cópia: leitura da resposta vazia sem texto após strip | `app/chamados/templatetags/chamados_tags.py:142` |
| 132 | D cópia SQL: calendário exclui vazio exato | `app/colaboradores/services/calendario.py:504` |
| 135 | D cópia: pendência ficha pelo texto vazio, fora de falou | `app/colaboradores/services/ficha.py:171` |
| 136 | D cópia: evidência de fala na prévia | `app/folha/previa.py:142` |
| 138 | D cópia: evidência de fala no cartório | `app/ponto/services/cartorio.py:1641` |
| 145 | D cópias: template usa verdade crua do texto | `app/templates/chamados/partials/_card_atendimento.html:141`, `:182` |

O censo **não fecha como enumeração exaustiva de delegados**: além desses R, há os seguintes sítios que precisam constar, embora já recebam classe pelo corpo chamado. (`app/chamados/templatetags/chamados_tags.py:202`; `app/chamados/services/declaracao_texto.py:253`; `app/chamados/models.py:719`.)

| Classe | Delegados adicionais |
|---|---|
| B transitivo | `app/chamados/models.py:719` → `perguntas_que_seguram_csf`, corpo em `app/ponto/supra_juiz.py:573` |
| C | `app/chamados/templatetags/chamados_tags.py:202` → corrigíveis, corpo `app/chamados/juizes.py:1744` |
| D | `app/chamados/services/declaracao_texto.py:253`, `:275` → canônica |
| D | `app/chamados/services/fio.py:995`, `:1892`, `:2215` → canônica |
| D | `app/chamados/management/commands/backfill_decisoes_pergunta.py:276`, `:393` → canônica |
| D | `app/chamados/services/fio.py:1327` → mérito perdedor |

Os corpos D desses delegados estão em `app/chamados/services/fio.py:1271` e `:1282`. Cobertura de despacho dinâmico arbitrário: [nao verificado].

As três finalidades C que exigem explicitação de escopo são R58, reconstrução da trilha; R94, existência/data de evento; R121, contador temporal. A classe sintática C não decide por si só qual parte desses contratos deve ler J. (`app/chamados/models.py:4098`; `app/chamados/services/historico.py:105`; `app/chamados/services/verbos.py:342`.)

As novas leituras B1 de `texto_sem_carimbo` e a guarda simétrica são E/integridade, não D; tarefa usa `ato_texto` sob ato RESPOSTA, não a projeção B/C/D. (`app/chamados/juizes.py:1537`; `app/chamados/services/fio.py:1604`; `app/chamados/services/tarefa_do_dia.py:159`.)

Produção deduplicada dos 66 usos e das delegações adicionais: 36 arquivos; as referências de evidência são as da tabela acima.

```text
app/api/views.py
app/chamados/canal.py
app/chamados/juizes.py
app/chamados/management/commands/auditar_invariantes_chamados.py
app/chamados/management/commands/backfill_decisoes_pergunta.py
app/chamados/management/commands/dedup_perguntas_fato.py
app/chamados/management/commands/validar_classe_a.py
app/chamados/models.py
app/chamados/reconciliador.py
app/chamados/services/acoes_disputa.py
app/chamados/services/becos.py
app/chamados/services/competencia_trancada.py
app/chamados/services/declaracao_texto.py
app/chamados/services/disputa_emissao.py
app/chamados/services/fila_por_causa.py
app/chamados/services/fio.py
app/chamados/services/historico.py
app/chamados/services/materializacao.py
app/chamados/services/pergunta_no_app.py
app/chamados/services/validacao.py
app/chamados/services/verbos.py
app/chamados/services/vigia_fio.py
app/chamados/templatetags/chamados_tags.py
app/chamados/views.py
app/chamados/views_cobrar.py
app/colaboradores/services/calendario.py
app/colaboradores/services/desligamento.py
app/colaboradores/services/ficha.py
app/folha/previa.py
app/ponto/management/commands/reconciliar_grade.py
app/ponto/services/cartorio.py
app/ponto/supra_juiz.py
app/templates/chamados/partials/_card_atendimento.html
app/templates/chamados/partials/_pergunta_card.html
app/templates/chamados/partials/modal_fio.html
bin/censo_folha.py
```

Cruzamento dessa lista de produção com o anexo bugpar: 7; associação ao anexo [nao verificado] como arquivo local, pois seu conteúdo foi fornecido no prompt.

```text
app/chamados/services/acoes_disputa.py
app/chamados/services/fio.py
app/chamados/services/materializacao.py
app/chamados/services/validacao.py
app/chamados/templatetags/chamados_tags.py
app/templates/chamados/partials/_pergunta_card.html
app/templates/chamados/partials/modal_fio.html
```

## 16/11 17:11 · sessao 4c1d8578746a · - · - · origem=Ronald (tmux)

**Perguntas de lei exigidas pela B2: 0.** O predicado I∧T, o juiz único, o estoque das duas vivas e a preservação das validadas/materializadas já estão decididos em `app/docs/LEIS.md:151`; o trim e a face SQL estão explícitos em `app/docs/CORTES.md:485`.

A lei **RESSUSCITAR-DEVOLVE-AO-COLABORADOR** permanece expressamente não decidida, mas a B1 conserva a fila e a ressuscitação sem ato de devolução; não é requisito novo da B2 literal. (`app/docs/LEIS.md:151`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:7`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:10`.)

O item 5 posterior determina que a resposta vazia continue fora do app **na F5**, com dono admin, e deixa a eventual devolução ao colaborador para aquela lei pendente. A costura entre esse corte e a migração da partição da B2 é **PERGUNTA DE DESENHO**, não autorização para inventar outra lei. (`app/docs/CORTES.md:487`; `app/chamados/services/pergunta_no_app.py:81`.)

A precedência de `validar_pergunta` — primeiro ausência de instante, depois validação — diverge da apresentação de estado; a L-130 já decide conservar validadas/materializadas. O tratamento dessa precedência é **PERGUNTA DE DESENHO**, com o código atual como prova. (`app/chamados/services/validacao.py:418`; `app/chamados/services/validacao.py:421`; `app/docs/LEIS.md:151`.)

Testes com predicados que a B2 revê: [nao verificado]

- `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:284` e `:288`: cada metade sozinha retém.
- `app/chamados/tests/test_e3_10_cura_no_escritor.py:132`, `:140`, `:147`: instante sem texto retém lista/C7/CSF.
- `app/chamados/tests/test_pergunta_estado.py:35`, `:58`: estado com par e com texto sem instante.
- `app/chamados/tests/test_service_validar_puro.py:98`, `:102`: texto sem instante resulta `sem_resposta`.
- `app/chamados/tests/test_o261e_f5_alcance.py:194`, `:199`: instante vazio fora dos pendentes, causa `b`.
- `app/chamados/tests/test_estado_aplicacao.py:38`, `:48`, `:56`, `:63`: fixtures com texto e sem atributo `respondida_em`; incompatíveis com chamada literal ao juiz que acessa esse atributo diretamente. (`app/chamados/juizes.py:1552`.)

**Cruzamento de teste comprovável:** `app/chamados/tests/test_estado_aplicacao.py:38`. Os contratos de escrita de `test_porta_validacao_pergunta.py` e a allowlist de `test_contract_leitores_vitalidade.py` não demonstram necessidade automática de edição pela troca do leitor. (`app/chamados/tests/test_porta_validacao_pergunta.py:72`; `app/chamados/tests/test_porta_validacao_pergunta.py:95`; `app/chamados/tests/test_contract_leitores_vitalidade.py:319`.)

Execução e aprovação dos testes neste hash: **[nao verificado]**. [nao verificado]

_6132 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
