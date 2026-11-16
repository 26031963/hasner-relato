# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 17:38** · publicado em **16/11/2026 17:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 16/11 17:11 · sessao bc6d28a885cf · - · - · origem=Ronald (tmux)

Mapa estático dos efeitos; quantidades em aberta/ENTREGUE, confirmação dos PK 38251/38252 e prova de “0 em ENTREGUE”: **[nao verificado]**.

| Leitor atual → caminho | Efeito que a sombra precisa observar |
|---|---|
| `sem_veredito_entre`: `I OU T` (`app/chamados/models.py:1922`) → `pode_fechar` (`:1934`) / `fechar` (`:1967`) | Ao liberar a porta, grava `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` (`app/chamados/models.py:2013`); signal tenta reconciliar o pai e escreve nota somente se fechar (`app/chamados/signals.py:43`). |
| Mesma lista → irmãs (`app/chamados/juizes.py:2105`) → retenção (`app/chamados/reconciliador.py:448`) | Pode deixar de reter `_retratar`, que chama transição para resolvido (`app/chamados/reconciliador.py:519`, `:524`); também pode liberar encerramento forçado (`:493`, `:497`). |
| Mesma lista → irmãs → materialização (`app/chamados/services/materializacao.py:768`) | Pode liberar reconciliação de destino/clones (`app/chamados/services/materializacao.py:777`). |
| Mesma lista → CSF (`app/ponto/supra_juiz.py:573`, `:574`, `:588`) → gate do pai (`app/chamados/models.py:719`) | Pode retirar recusa `conversa_exige_decisao_humana` e a nota “Chamado mantido aberto” (`app/chamados/models.py:723`, `:735`). Este é um leitor transitivo adicional à enumeração explícita do censo. |
| Mesma lista → desligamento (`app/colaboradores/services/desligamento.py:165`) | Pode liberar fechamento da disputa (`:167`); o encerramento dos chamados passa ainda por outra porta/gate (`:187`). |
| Mesma lista ignorando tranca → medição (`app/chamados/services/competencia_trancada.py:126`) | Muda IDs `perguntas_com_resposta_sem_veredito`/`decidir_pelo_dp` (`:137`); esses IDs alimentam Pauta DP (`:232`, `:246`, `:256`); fecho da disputa consulta novamente `pode_fechar` (`:227`). |
| D em `veredito_orfa` (`app/chamados/reconciliador.py:1083`) → consumidor | Pode alterar elegibilidade de `encerrar_fantasma` (`:1148`); a disputa resultante volta a consultar lista sem veredito e fechamento (`:1164`, `:1171`). |
| D em `lavrar_sem_turno_aberto` (`app/chamados/reconciliador.py:365`) | Pode alterar elegibilidade de carimbo (`:406`), seguido de fechamento da disputa (`:412`). |
| C em beco (`app/chamados/services/becos.py:30`) | Muda classificação (`:33`); quando fechável, executa resolver (`:54`) e fecho (`:56`, `:57`). O evento da pergunta agenda isso em `on_commit` (`app/chamados/signals.py:240`). |
| C no lote materializável (`app/chamados/services/materializacao.py:1376`) | Muda seleção para `_materializar_uma` (`:1382`), seguida do juiz de fechamento (`:1384`). D no plantio hora (`:835`) hoje recusa sem texto, carimbando recusa (`:842`). |
| C na regularização (`app/chamados/services/acoes_disputa.py:130`; `app/chamados/services/fio.py:698`) | Controla entrada em materialização (`:139`; `:708`) e nos pushes Web/FCM (`acoes_disputa.py:150`, `:156`; `fio.py:719`, `:726`). Os pushes precedem o retorno “nenhuma pergunta validada” (`acoes_disputa.py:160`; `fio.py:731`). |
| C na recusa (`app/chamados/services/validacao.py:605`) | Controla acesso à zeragem com ato `recusa` (`:628`), nota (`:633`), auditoria (`:637`) e push (`:648`). |
| C nas corrigíveis → reabertura (`app/chamados/services/acoes_disputa.py:97`) | Executa zeragem (`:103`), nota (`:106`), push (`:109`). |
| C na recobrança (`app/chamados/services/disputa_emissao.py:1735`) | O predicado determina o contador; prazo (`:1740`) e push (`:1750`) são executados independentemente do contador. Trocar esse filtro, isoladamente, não demonstra variação na quantidade de pushes. |

B1 já mudou o leitor A da vassoura para o juiz: `_resolver_perguntas_por_chamado_resolvido` pergunta `pergunta_respondida` em `app/chamados/services/disputa_emissao.py:2002`; a retenção B ainda usa OU em `app/chamados/models.py:1924`.

Crons relevantes neste hash: consumidor de eventos a cada minuto (`app/config/crons.py:394`), reconciliador em `--alarme` a cada 15 minutos (`:395`), supra com `--executar` (`:599`), beco em `--alarme` (`:663`). Não atribuir escrita ao simples fato de o cron consultar esses leitores; os dois últimos modos de vigia citados estão expressos nesses argumentos.

**PERGUNTA DE DESENHO:** qual classe recebe pergunta sem dia na tabela sombra? O compositor retorna `None` sem dia e documenta que a medida o agrega em “aberta”, enquanto a fronteira o deixa fora (`app/chamados/services/vigia_fio.py:794`, `:797`); o gate aceita exclusivamente `is False` (`app/chamados/models.py:364`).

A separação aberta/ENTREGUE usa janela da empresa e `competencia_entregue`, não mês civil (`app/chamados/services/vigia_fio.py:785`, `:799`, `:801`, `:804`). A Pauta de tranca não fica automaticamente neutra porque a pergunta é entregue: recebe a lista sem veredito com `ignorar_tranca=True` (`app/chamados/services/competencia_trancada.py:126`, `:137`, `:232`).

Arquivos do mapa que estão no anexo BUGPAR e têm leitores B2 diretos:

- `app/chamados/services/acoes_disputa.py` — `:95`, `:130`.
- `app/chamados/services/fio.py` — `:698`, `:1271`, `:1283`.
- `app/chamados/services/materializacao.py` — `:438`, `:835`, `:1376`.
- `app/chamados/services/validacao.py` — `:418`, `:605`.

`app/config/crons.py` e `app/chamados/signals.py` são pontos de observação dos efeitos descritos; necessidade de editá-los para B2: **[nao verificado]**.

## 16/11 17:11 · sessao fbbaa93b88bd · - · - · origem=Ronald (tmux)

Os leitores atuais de `models.py` continuam por campos da projeção: `estado` em `app/chamados/models.py:3151`, `sem_veredito_entre` em `app/chamados/models.py:1922`, `q_vivas` em `app/chamados/models.py:3496`, `respondida` em `app/chamados/models.py:3664` e `resposta_display` em `app/chamados/models.py:3686`. A projeção temporal também lê os campos atuais; apenas corta carimbos posteriores a `agora`, quando informado (`app/chamados/juizes.py:1389`). Não sustentar o comportamento da c3 antiga como comportamento desta cópia (`app/chamados/models.py:3151`; `app/chamados/models.py:3496`).

| Situação conferida | Decisão atual |
|---|---|
| Validação preenchida | C6 morta antes de qualquer outra guarda (`app/chamados/juizes.py:1405`); `estado` retorna `resolvida` havendo `materializada_em`, senão `validada` (`app/chamados/models.py:3151`); excluída de `sem_veredito_entre` (`app/chamados/models.py:1923`) e de `q_vivas` (`app/chamados/models.py:3496`). |
| Instante de resposta sem texto | `pergunta_respondida=False` pelo I E T (`app/chamados/juizes.py:1552`); property `respondida=True` (`app/chamados/models.py:3665`); sem validação/materialização e sem tranca, entra em `sem_veredito_entre` (`app/chamados/models.py:1923`); `estado=respondida` mesmo se C6 morta (`app/chamados/models.py:3155`; `app/chamados/models.py:3158`). |
| Texto sem instante | `pergunta_respondida=False` (`app/chamados/juizes.py:1552`); property `respondida=False` (`app/chamados/models.py:3665`); sem validação/materialização e sem tranca, entra em `sem_veredito_entre` pelo texto (`app/chamados/models.py:1925`); `estado=pendente` mesmo se C6 morta (`app/chamados/models.py:3155`; `app/chamados/models.py:3158`). |
| Dia trancado por leitura, sem carimbo | C6 morta antes da resposta (`app/chamados/juizes.py:1409`); lista sem veredito a exclui, salvo `ignorar_tranca=True` (`app/chamados/models.py:1926`); `estado` ainda retorna pendente/respondida segundo instante (`app/chamados/models.py:3158`). |
| Via `competencia_trancada`, `duplicata` ou `motivo_retratado`, carimbada | C6 morta antes da resposta (`app/chamados/juizes.py:1416`; conjunto em `app/chamados/models.py:2629`); `q_vivas` exclui antes do ramo de resposta (`app/chamados/models.py:3485`; `app/chamados/models.py:3496`); estado resolvido sem validação, pois carimbo presente (`app/chamados/models.py:3156`). |
| Resposta com instante e texto | Depois das guardas anteriores, continua viva quando admin não decidiu a célula e resposta não concorda comprovadamente com luz/classe A/dia não acusador (`app/chamados/juizes.py:1423`; `app/chamados/juizes.py:1431`; `app/chamados/juizes.py:1435`; concordância em `app/chamados/juizes.py:1346`). |
| Identidade carimbada sem resposta vencedora | C6 morta (`app/chamados/juizes.py:1444`); conjunto = `VIAS_DE_IDENTIDADE` mais competência trancada (`app/chamados/models.py:3202`), identidade definida em `app/chamados/models.py:2615`. |
| Marco próprio aceso e carimbo | C6 morta antes do veredito do restante do dia (`app/chamados/juizes.py:1450`). |
| Célula acusa, alcançado esse degrau | C6 viva antes de consultar testemunha (`app/chamados/juizes.py:1456`). |
| Célula não acusa | C6 morta se não há marcos pendentes ou marco próprio já acendeu; caso contrário continua para as guardas seguintes (`app/chamados/juizes.py:1467`). |
| Lei acusa dentro da janela viva | C6 viva antes do teste final do carimbo (`app/chamados/juizes.py:1471`). |
| Sem carimbo, alcançado esse degrau | C6 viva (`app/chamados/juizes.py:1482`); a condição antiga c3 de “E sem carimbo” não é alcançada por este ramo (`app/chamados/juizes.py:1482`; `app/chamados/juizes.py:1489`). |
| Carimbo sem absolvição anterior | C6 devolve inverso da testemunha; exceção da testemunha termina em morta (`app/chamados/juizes.py:1489`; `app/chamados/juizes.py:1492`); `estado` respeita essa vida antes de declarar `resolvida` pelo carimbo (`app/chamados/models.py:3154`). |
| Ressuscitação atual | Universo simula o par inteiro zerado, incluindo `resposta_colab=''` (`app/chamados/models.py:1553`); registra linhas legadas de resposta/validação antes de limpar campos (`app/chamados/models.py:1613`); descarimba e zera validação, instante e texto (`app/chamados/models.py:1629`); deliberadamente não escreve REABERTURA porque esse ato mudaria o dono para colaborador (`app/chamados/models.py:1622`). |

Precedência completa do C6: projeção (`app/chamados/juizes.py:1404`) → validação (`app/chamados/juizes.py:1405`) → tranca viva (`app/chamados/juizes.py:1409`) → via terminal carimbada (`app/chamados/juizes.py:1416`) → resposta I E T, exceção admin e concordância (`app/chamados/juizes.py:1423`; `app/chamados/juizes.py:1431`; `app/chamados/juizes.py:1435`) → identidade carimbada (`app/chamados/juizes.py:1444`) → marco aceso carimbado (`app/chamados/juizes.py:1450`) → célula acusa (`app/chamados/juizes.py:1456`) → célula absolve/pêndencia (`app/chamados/juizes.py:1468`) → lei/janela (`app/chamados/juizes.py:1471`) → ausência de carimbo (`app/chamados/juizes.py:1482`) → testemunha (`app/chamados/juizes.py:1489`) → exceção morta (`app/chamados/juizes.py:1492`).

`estado` tem outra ordem de tradução: validação → C6 → carimbo → resposta por instante (`app/chamados/models.py:3151`). `sem_veredito_entre` testa validação → materialização → instante OU texto → tranca (`app/chamados/models.py:1922`). `fechar` testa porta válida → disputa já fechada/idempotência → usuário → respostas sem veredito → perguntas vivas → exceção nomeada → escrita (`app/chamados/models.py:1957`; `app/chamados/models.py:1960`; `app/chamados/models.py:1965`; `app/chamados/models.py:1967`; `app/chamados/models.py:1977`; `app/chamados/models.py:1999`).

Possíveis perguntas para a consolidação:

- PERGUNTA DE DESENHO: Para instante sem texto, “respondida” deve continuar verdadeiro na property e falso no juiz I E T, enquanto a retenção trata o instante como suficiente? | `app/chamados/models.py:3665` x `app/chamados/juizes.py:1552` (retenção: `app/chamados/models.py:1924`).
- PERGUNTA DE DESENHO: Para texto sem instante, a retenção deve continuar cobrando veredito embora o juiz e a property neguem “respondida”? | `app/chamados/models.py:1925` x `app/chamados/juizes.py:1552`.
- PERGUNTA DE DESENHO: Na pergunta morta por tranca sem carimbo, o estado pendente/respondida descreve o mesmo estado que a porta considera morto? | `app/chamados/models.py:3158` x `app/chamados/juizes.py:1409`.
- PERGUNTA DE DESENHO: A ressuscitação deve devolver ao colaborador quando a última decisão conservada é SILENCIO, ou preservar `silencio_decidido` apesar da projeção novamente pendente/viva? O próprio código declara essa lei não decidida. | `app/chamados/models.py:1622` x `app/chamados/services/tarefa_do_dia.py:176`.
- PERGUNTA DE DESENHO: Qual ato conserva o efeito após descarimbar, se a projeção temporal lê apenas o campo atual e os tipos de DP não incluem materialização? | `app/chamados/models.py:3318` x `app/chamados/juizes.py:1389` (atos em `app/chamados/models.py:4024`).

Observações úteis: `q_vivas` declara ser pré-filtro generoso, com contrato de continência, não igualdade com C6; isso não constitui sozinho divergência de desenho (`app/chamados/models.py:3464`; `app/chamados/models.py:3509`). A janela já pode ler `date.today()` quando não recebe `hoje`; não atribuir isso a uma migração F2 que esta cópia não mostra (`app/chamados/juizes.py:596`; `app/chamados/juizes.py:1471`). O memo C6 só escreve `_pv_memo` na instância, não persiste estado de domínio (`app/chamados/juizes.py:905`).

## 16/11 17:11 · sessao 61e3b2e8e524 · E4C-F1-B2 · verificador · origem=Code

AVAL-DEVOLVIDO — item 1(c): o escopo B/C/D inclui um leitor em `app/chamados/services/validacao.py:418`, também listado no anexo BUG-PAR; o cruzamento zero não está demonstrado pelo diff efetivo. [nao verificado]

1. **As premissas técnicas conferem; a migração ainda falta.** O juiz existe em `app/chamados/juizes.py:1544`: exige instante **e** texto após trim (`app/chamados/juizes.py:1552`), tem face SQL em `app/chamados/juizes.py:1575` e registro em `app/core/juizes.py:306`. B2 significa migrar B/C/D — retenção, leitores por instante e leitores só por texto — incluindo a property; E/F permanecem fora (`app/chamados/juizes.py:1550`, `app/docs/JUIZES.md:103`). Neste código, `respondida` ainda testa somente o instante (`app/chamados/models.py:3664`), e retenção ainda usa instante **ou** texto (`app/chamados/models.py:1922`). Portanto, “property delega” descreve a entrega pretendida, não o estado atual (`app/chamados/models.py:3665`).

2. **Não identifico colisão necessária de prioridade.** O registro anterior põe o complemento de vínculo depois do editor; a regra geral dá precedência à espinha (`app/docs/RELATO.md:21`, `app/docs/RELATO.md:33`). A nova ordem expressa de reservar a vaga 2 à B2 pode alterar essa sequência; sua efetivação está [nao verificado]. O registro disponível mostra editor aprovado, com pouso ainda por realizar; não prova vaga liberada (`app/docs/RELATO.md:5`, `app/docs/RELATO.md:11`).

   No anexo de origem, a medida na sombra é exigida **antes de pousar**, enquanto (a)–(d) condicionam **abrir o construtor**. São momentos distintos; o item 1 precisa conservar o requisito anterior. Tabela de efeitos sobre chamados fechados, perguntas retratadas/materializadas, pautas e pushes, separada entre aberta e ENTREGUE, com zero em ENTREGUE: [nao verificado]. [nao verificado]

3. **As condições não podem ser declaradas fechadas.**

   - **(a)** “Censo fechado” tem forma definida: seção `CENSO`, leitores e escritores com arquivo:linha, `DECISOES` listadas e linha literal única `perguntas de lei: N` (`bin/disjuntor.py:1493`, `bin/disjuntor.py:1565`). O parser admite perguntas cortadas quando N>0; o item anexado exige especificamente zero. Brief B2 com esse conteúdo: [nao verificado]. Passar pelo parser comprova a forma, sem demonstrar a completude do levantamento (`bin/disjuntor.py:1522`, `bin/disjuntor.py:1577`).
   - **(b)** Tabela B2 com situação → juiz → ação da porta → persistência → estado → tela, retirada deste código: [nao verificado]. Os leitores atuais divergem, portanto uma tabela reduzida à verdade do juiz não representa sozinha as portas existentes (`app/chamados/models.py:1924`, `app/chamados/models.py:3665`, `app/chamados/services/validacao.py:418`).
   - **(c)** A medida pedida é a interseção entre **todos os caminhos de arquivos do diff BUG-PAR** e **todos os arquivos previstos para alteração pela B2**, incluindo testes e documentação; comparação por função ou por trecho não demonstra “zero arquivo”. Conjunto completo B2 e diff BUG-PAR conferido contra sua base: [nao verificado]. Há um cruzamento concreto a resolver: o censo registrado classifica `validar_pergunta` como C (`app/docs/RELATO.md:225`), e sua guarda continua por instante (`app/chamados/services/validacao.py:418`); esse caminho consta da lista fornecida. Excluir esse leitor do conjunto exigiria justificar como a B2 integral o migra; solução demonstrada: [nao verificado].
   - **(d)** Prioridade reservada não comprova disponibilidade: o registro ainda descreve o pouso do editor como próximo passo (`app/docs/RELATO.md:5`). Vaga efetivamente livre e teto respeitado: [nao verificado].

4. **O alcance potencial inclui persistência e dinheiro; efeito real além do declarado está [nao verificado].** A validação carimba a pergunta, chama recálculo do fechamento e materialização (`app/chamados/services/validacao.py:460`, `app/chamados/services/validacao.py:471`, `app/chamados/services/validacao.py:494`). A retenção participa da autorização de fechar disputas (`app/chamados/models.py:1934`). Isso sustenta manter a medida de efeitos anterior ao pouso.

   As duas abertas 38251/38252 pertencem à obra, com retorno a pendente e trilha; as 141 entregues exigem `!` próprio, preservando validadas/materializadas (`app/docs/LEIS.md:151`). O anexo de origem ainda preserva as 30 materializadas e manda publicar a lista das 111 restantes; lista publicada, confirmação dos dois pk no RED e trilha efetiva da correção: [nao verificado]. A ressalva nominal às 141 não comprova zero efeito indireto em ENTREGUE após mudar leitores; essa medida permanece [nao verificado].

VEREDITO: AVAL-DEVOLVIDO -- item 1(c): leitor C em app/chamados/services/validacao.py:418, identificado em app/docs/RELATO.md:225 e listado na BUG-PAR; cruzamento zero nao demonstrado.

## 16/11 17:10 · sessao a8341253e558 · - · - · origem=Ronald (tmux)

Leitores e telas conferidos nesta cópia:

| Situação | O que aparece |
|---|---|
| Estado da pergunta | `estado` lê `validada_em`, depois `pergunta_viva`, depois `materializada_em` e `respondida_em`; continua lendo projeção, sem consultar diretamente decisões. `app/chamados/models.py:3151-3158` |
| Pergunta respondida | A propriedade aceita qualquer `respondida_em`, inclusive texto vazio; o juiz `pergunta_respondida` exige instante **e** texto após trim. `app/chamados/models.py:3664-3665` × `app/chamados/juizes.py:1544-1552` |
| Texto apresentado | `resposta_display` usa `resposta_colab`: vazio → `''`; posto → nome; escala → tipo; opções → rótulo; fallback → texto atual. `app/chamados/models.py:3688-3710` |
| API do app | Publica `estado`, `aguarda_admin = estado == RESPONDIDA`, texto, display e instante atuais. A lista `respondidas` também julga somente pelo instante. `app/api/views.py:2550-2563`, `app/api/views.py:2577` |
| Card colaborador, pergunta viva | Oferece formulário; mostra “Resposta enviada” e “Reenviar resposta” apenas quando `resposta_enviada`; esse valor exige instante com texto. `app/templates/chamados/partials/_card_atendimento.html:77-107`, `app/templates/chamados/partials/_card_atendimento.html:160-161`, `app/chamados/services/fio.py:357`, `app/chamados/services/fio.py:177-187` |
| Card colaborador, pergunta morta | Pendência administrativa vence o rótulo; sem pendência, `estado == resolvida` mostra “Encerrado”, demais estados mostram “Validado pela supervisão”; depois apresenta motivo pela via. `app/templates/chamados/partials/_card_atendimento.html:179-192` |
| Card administrativo | Cabeçalho: `validada_em` → “validada”; senão `respondida_em` → “aguardando validação”; senão “aguardando colaborador”. Botões dependem de instante de resposta e ausência de validação. `app/templates/chamados/partials/_pergunta_card.html:20-25`, `app/templates/chamados/partials/_pergunta_card.html:29-42` |
| Modal administrativo | Primeiro exige `respondida_em`; depois pendência administrativa → “Pendência do admin”; senão validação → “Validada”; senão oferece validação. Sem instante, mostra “Aguardando resposta do colaborador”. `app/templates/chamados/partials/modal_fio.html:217-250`, `app/templates/chamados/partials/modal_fio.html:318-319` |
| Explicador | PENDENTE → colaborador; RESPONDIDA → admin; depois pendência administrativa; depois juiz factual: verdadeiro → resolvida, falso → validada sem gravar, com dono cadastro/sistema. `app/chamados/services/explicador.py:72-104` |
| Fechamento na explicação | O resumo obedece `pode_fechar`; os itens ainda carregam as explicações individuais, e o template mostra todos os itens cujo estado explicativo não é `resolvida`. `app/chamados/services/explicador.py:133-165`, `app/templates/chamados/partials/modal_fio.html:435-446` |
| Disputa fechada no fio administrativo | SQL `q_vivas` primeiro; depois `pergunta_viva` elimina as mortas. Não é igualdade SQL/juiz: o contrato documentado é continência. `app/chamados/services/fio.py:254-269`, `app/chamados/models.py:3464-3470` |
| App: pendente ou histórico | Primeiro `q_vivas`, depois `pergunta_viva`; dentre as vivas, instante de resposta ausente **e** disputa aberta → pendentes, demais → histórico. `app/chamados/services/pergunta_no_app.py:52-61`, `app/chamados/services/pergunta_no_app.py:76-85` |
| Vigia, folga validada | Ausência aberta vence; depois `p.estado == VALIDADA` com aceite de folga → escala não descreve o dia; depois dia do admin → fio do admin. `app/chamados/services/vigia_fio.py:256-263` |

Divergências concretas para considerar na consolidação:

1. **Instante com resposta vazia/brancos:** propriedade `respondida` é verdadeira e API pode dizer `aguarda_admin`, enquanto card colaborador oferece “Enviar resposta”, e tarefa exige responder pelo colaborador. `app/chamados/models.py:3664-3665`, `app/chamados/models.py:3154-3158`, `app/api/views.py:2550-2553` × `app/chamados/services/fio.py:177-187`, `app/templates/chamados/partials/_card_atendimento.html:160-161`, `app/chamados/services/tarefa_do_dia.py:159-161`.

2. **Validada + materializada, sem testemunha factual atual:** `estado`/API dizem RESOLVIDA e card colaborador “Encerrado”, mas explicador pode dizer “validada_sem_gravar”, exibido como “O sistema não conseguiu gravar”. `app/chamados/models.py:3151-3153`, `app/api/views.py:2550-2552`, `app/templates/chamados/partials/_card_atendimento.html:189` × `app/chamados/services/explicador.py:92-104`, `app/templates/chamados/partials/modal_fio.html:442`.

3. **Materializada, sem validação/resposta, morta pelo juiz:** `estado` retorna RESOLVIDA, mas cabeçalho do card administrativo diz “aguardando colaborador”. A exibição efetiva dessa combinação numa tela específica depende de ela entrar no universo do chamador. `app/chamados/models.py:3154-3157` × `app/templates/chamados/partials/_pergunta_card.html:23-25`; presença concreta em produção [nao verificado].

4. **DEVOLUÇÃO/RECUSA/REABERTURA com projeção divergente:** tarefa interpreta o último ato, enquanto estado e cabeçalhos administrativos continuam nos campos. DEVOLUÇÃO exige validar/admin; RECUSA e REABERTURA exigem responder/colaborador. `app/chamados/services/tarefa_do_dia.py:158-166` × `app/chamados/models.py:3151-3158`, `app/templates/chamados/partials/_pergunta_card.html:23-25`.

O corpo de `sem_veredito_entre` conserva explicitamente instante **OU** texto e exclui validada/materializada antes de consultar tranca; `pode_fechar` avalia essa lista antes de `fio_vivo`, por curto circuito. `app/chamados/models.py:1922-1926`, `app/chamados/models.py:1934`.

## 16/11 17:10 · sessao 34f49298ed2f · - · - · origem=Ronald (tmux)

Achados para consolidar:

- “Brief com censo fechado” tem definição literal: seção `CENSO`, leitores e escritores com `arquivo:linha`, `DECISOES` listadas e linha única `perguntas de lei: N`; quantidade de perguntas deve fechar e, para N>0, cada uma exige corte nomeado. A função verifica forma; a completude do censo exige conferência adicional. O item 1(a) exige especificamente N=0. Referências: `bin/disjuntor.py:1493`, `bin/disjuntor.py:1522`, `bin/disjuntor.py:1565`.
- B2 ainda é descrita como por construir; abrange leitores B/C/D e estoque das duas vivas. A propriedade `respondida` e retenção migram na B2; E/F nunca. Referências: `app/docs/LEIS.md:151`, `app/docs/JUIZES.md:99`, `app/docs/JUIZES.md:103`.
- A prioridade B2 após o editor pode substituir a ordem anterior dos complementos de tela sem contradizer a prioridade geral “espinha passa na frente de lateral”. A ordem anterior está em `app/docs/RELATO.md:19`, `app/docs/RELATO.md:21`, `app/docs/RELATO.md:24`; prioridade geral em `app/docs/RELATO.md:33`.
- O registro disponível apresenta o editor aprovado e ainda com pouso por realizar; vaga efetivamente liberada não está provada. Referências: `app/docs/RELATO.md:5`, `app/docs/RELATO.md:11`, `app/docs/RELATO.md:13`; vaga atual [não verificado].
- B1 está documentada como no ar, e B2 esperava esse pouso. Referências: `app/docs/RELATO.md:105`, `app/docs/RELATO.md:163`, `app/docs/RELATO.md:174`.
- O POUSO B2 do anexo exige medida na sombra antes do pouso, não antes da abertura do construtor. Portanto não há contradição automática entre permitir construir condicionalmente e preservar aquele gate; tabela real de efeitos e 0 em ENTREGUE [não verificado]. [nao verificado]
- As duas abertas 38251/38252 com trilha pertencem à obra; 141 entregues exigem `!` próprio, e validadas/materializadas ficam. Referência: `app/docs/LEIS.md:151`. O anexo de origem ainda distingue 30 materializadas preservadas e lista futura das 111; confirmação atual dos pks em RED e execução da trilha [não verificado].
- Codex construtor está proibido de tocar competência exportada ou item que pede `!`. Referência: `app/docs/LEIS.md:147`.
- Não há `logs/` nesta cópia; brief B2, tabela de estados, lista efetiva de arquivos B2, censo de origem completo e medida de sombra não puderam ser examinados [não verificado].

## 16/11 17:10 · sessao 11f23f124cfd · - · - · origem=Ronald (tmux)

- O juiz existe em `app/chamados/juizes.py:1544`, retorna instante **E** texto após trim em `:1552`, tem face SQL em `:1575-1580` e registro em `app/core/juizes.py:306`. O próprio código reserva classes B/C/D — retenção, property e leitores só texto — para B2 (`app/chamados/juizes.py:1550-1551`).
- A property ainda usa apenas instante (`app/chamados/models.py:3663-3665`). A retenção ainda usa instante **OU** texto (`app/chamados/models.py:1922-1926`); sua lista bloqueia `pode_fechar` (`:1928-1934`) e a porta `fechar` (`:1967-1969`). A retração de irmãs consulta esse mesmo corpo (`app/chamados/juizes.py:2092-2105`).
- Leitor C confirmado: `validar_pergunta` aceita pelo carimbo (`app/chamados/services/validacao.py:418-420`); o censo registrado nomeia esse leitor como classe C (`app/docs/RELATO.md:225`). Esse arquivo consta da lista BUG-PAR fornecida [não verificado por arquivo local]. Logo, o conjunto natural da migração B2 já tem candidato direto de cruzamento; conjunto efetivamente autorizado pelo brief e total exato [não verificado].
- A mesma porta carimba validação (`app/chamados/services/validacao.py:460`), recalcula fechamento por evento (`:469-472`) e chama materialização (`:486-494`). O materializador seleciona instante + validação (`app/chamados/services/materializacao.py:1375-1379`) e descreve criação de batidas/ausências e fechamento de destinos (`:1344-1362`). Isso sustenta exigir medida de efeitos; efeito real em produção [não verificado].
- Outros pontos nos arquivos da lista BUG-PAR a classificar no censo: carimbo cru nos templates `app/templates/chamados/partials/_pergunta_card.html:24`, `:29`, e `app/templates/chamados/partials/modal_fio.html:185`, `:217`; `app/chamados/services/fio.py:187`, `:698`. Não assumir que todos pertencem B/C/D sem classificar finalidade; conjunto exato [não verificado].
- Outros leitores C presentes: partição do app em `app/chamados/services/pergunta_no_app.py:76-81`; API `ja_respondida` em `app/api/views.py:2620-2627`.
- A guarda só texto do materializador é explicitamente de anuência (`app/chamados/services/materializacao.py:828-835`); classificá-la como D migrável, em vez de finalidade E/F preservada, exige censo/brief [não verificado].
- O registro vigente mantém B2 por construir, duas abertas na obra, 141 entregues só com `!`, validadas/materializadas preservadas (`app/docs/LEIS.md:151`). A identificação atual dos pk 38251/38252, RED e trilha da correção não foram comprovados nesta leitura [não verificado].

## 16/11 17:03 · sessao abdee5dbe35b · BUG-PAR-O-PAR · verificador · origem=Code

1. **Q1 — Confere e fecha a delimitação.** Sem `data_evento`, o helper retorna antes de consultar K6; U163 é pulado antes dessa verificação. Portanto, L5 descreve a metade fora163 sem dia, não ausência de consulta a qualquer juiz. Isso corrige a premissa da L5 original sem exigir mudança na ordem da porta. (`app/chamados/services/materializacao.py:780`; `app/chamados/services/materializacao.py:785`; `app/chamados/services/materializacao.py:797`; `app/chamados/services/materializacao.py:801`.)

2. **Q2 — Confere como precedência relativa das linhas da testemunha; a porta tem outra ordem de decisão.** Na testemunha, a guarda K6 antecede F6; F6 antecede declarado × gravado; `aplicavel` vem depois. Existem guardas intermediárias de data, derivação, satisfação e paridade, preservadas pelo “sem mudança”. (`app/chamados/services/materializacao.py:861`; `app/chamados/services/materializacao.py:866`; `app/chamados/services/materializacao.py:871`; `app/chamados/services/materializacao.py:874`; `app/chamados/services/materializacao.py:884`; `app/chamados/services/materializacao.py:897`; `app/chamados/services/materializacao.py:908`; `app/chamados/services/materializacao.py:911`.)

   Na **porta**, a sequência é: consulta antecipada da espera → implausibilidade → guarda explícita K6 → carimbo de validação → materialização, onde F6 pode recusar. A consulta antecipada percorre a testemunha, mas extrai **somente** seu estado de espera; não devolve automaticamente os bloqueios K6/F6 encontrados nela. (`app/chamados/services/validacao.py:573`; `app/chamados/services/validacao.py:577`; `app/chamados/services/validacao.py:614`; `app/chamados/services/validacao.py:617`; `app/chamados/services/validacao.py:645`; `app/chamados/services/materializacao.py:753`; `app/chamados/services/materializacao.py:754`; `app/ponto/registro_batida.py:156`.)

   Assim, uma U163 divergente bloqueada por F6 recebe **L2 na testemunha**, e a consulta antecipada não a devolve como L3. A diferença das ordens não cria colisão residual: o complemento fixa a unidade na testemunha, preserva expressamente a porta e distingue seus retornos. (`app/chamados/services/materializacao.py:886`; `app/chamados/services/materializacao.py:910`; `app/chamados/services/materializacao.py:754`; `complemento3_espinha_bug_par_c4_1611.txt:1`; `complemento3_espinha_bug_par_c4_1611.txt:3`; `complemento3_espinha_bug_par_c4_1611.txt:11`, texto fornecido.)

3. **Q3 — Confere; L5 fora163 já não entrava no contador.** A seleção é exclusivamente U163. Retirar o balde projetado não remove um leitor efetivo de L5 nem altera o universo contado. O balde existente `bug_par_103_sem_dia` trata candidatas U103 carimbadas, outra situação; a lista do comando já inclui fora163 sem dia como linha falante. Isso fecha a correção da L5 original sem deixar balde a decidir pelo construtor. (`app/chamados/services/materializacao.py:321`; `app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:331`; `app/chamados/management/commands/bug_par_103.py:71`; `app/chamados/management/commands/bug_par_103.py:75`; `app/chamados/management/commands/bug_par_103.py:77`.)

4. **Q4 — Confere; a fixture é construível com K6 inalterado durante o percurso.** Já existe fixture com K6 estável em falso, `U60=U163={saida}`, respostas coincidentes com os marcos e validação administrativa que grava o par, reduzindo `universo` de 1 para 0. (`app/chamados/tests/test_bug_par_projetado.py:51`; `app/chamados/tests/test_bug_par_projetado.py:80`; `app/chamados/tests/test_bug_par_projetado.py:203`; `app/chamados/tests/test_bug_par_projetado.py:209`; `app/chamados/tests/test_bug_par_projetado.py:211`; `app/chamados/tests/test_bug_par_projetado.py:212`; `app/chamados/tests/test_bug_par_projetado.py:217`.)

   Por inspeção, essa construção permite cobrar `pendentes_aval: 1→0`, `resolvidas: 0→1` e `congelados_no_recorte=1`. L3 continua pendente, portanto zero global não decorre do contrato. O F4 atual ainda obtém zero mudando K6; não prova a exigência corrigida. A execução da nova prova é **[nao verificado]**. (`app/chamados/tests/test_bug_par_projetado.py:47`; `app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:351`; `app/chamados/services/materializacao.py:356`; `app/chamados/tests/test_bug_par_103.py:89`.)

Quanto a exportada, dinheiro e `!`, as quatro respostas não ampliam a autorização original. A validação já agenda recálculo antes do plantio; ausência de batida nova não demonstra ausência de escrita financeira. Tocar gravado exportado continua sujeito ao `!`; efeito produtivo concreto é **[nao verificado]**. (`complemento3_espinha_bug_par_c4_1611.txt:1`, texto fornecido; `app/chamados/services/validacao.py:627`; `app/ponto/services/fechamento.py:1810`; `app/ponto/services/fechamento.py:1824`; `app/ponto/services/fechamento.py:1160`; `CLAUDE.md:265`; `CLAUDE.md:411`.)

VEREDITO: CONFERE

## 16/11 17:02 · sessao 863272485906 · - · - · origem=Ronald (tmux)

**CONFERE, explicitando a diferença das ordens.** A unidade fixada no complemento3 é o estado da testemunha; nessa unidade, a precedência relativa informada confere: K6, F6, DXG, aplicável, preservadas as guardas intermediárias. (`app/chamados/services/materializacao.py:861`; `app/chamados/services/materializacao.py:884`; `app/chamados/services/materializacao.py:897`; `app/chamados/services/materializacao.py:908`; `app/chamados/services/materializacao.py:911`; `app/chamados/services/materializacao.py:914`.)

A porta consulta DXG antes, depois implausibilidade, K6 explícito e materialização/F6; Q1 preserva expressamente essa ordem, e a NOTA separa os retornos. Portanto, a diferença deve constar no relatório, mas não constitui colisão residual que exija devolver o aval. (`app/chamados/services/validacao.py:571`; `app/chamados/services/validacao.py:577`; `app/chamados/services/validacao.py:614`; `app/chamados/services/validacao.py:645`; `app/chamados/services/materializacao.py:753`; `app/chamados/services/materializacao.py:754`.)

## 16/11 17:01 · sessao 863272485906 · - · - · origem=Ronald (tmux)

- **Q1 confere quanto à premissa e ao universo.** `_competencia_que_barra` pula U163 antes de consultar a data; fora163 sem dia gera o impedimento atual. `competencia_do_fato` retorna `None` antes de chamar K6. Assim, “K6 não consultado por falta de dia” corrige o “nenhum juiz consultado” de L5 sem exigir mudança do helper. (`app/chamados/services/materializacao.py:797`; `app/chamados/services/materializacao.py:799`; `app/chamados/services/materializacao.py:800`; `app/chamados/services/materializacao.py:780`; `app/chamados/services/materializacao.py:785`.)

- **Q1 não garante que a porta devolva L5 em qualquer sobreposição.** A porta consulta a testemunha para extrair exclusivamente DXG; se a testemunha disser o atual impedimento por falta de dia, essa consulta devolve vazio. A porta ainda pode retornar `implausivel` antes da guarda explícita de K6. Portanto, Q1 preserva corretamente a ordem atual, mas uma promessa em Q2 de “K6/L5 primeiro também na porta” conflita com essa ordem. (`app/chamados/services/materializacao.py:753`; `app/chamados/services/materializacao.py:754`; `app/chamados/services/materializacao.py:861`; `app/chamados/services/materializacao.py:863`; `app/chamados/services/validacao.py:571`; `app/chamados/services/validacao.py:577`; `app/chamados/services/validacao.py:614`.)

- **Q3 confere: L5 fora163 já não entrava no contador.** O queryset é exclusivamente U163; retirar o balde projetado preserva o universo e não elimina um leitor efetivo de L5. O balde existente `bug_par_103_sem_dia` pertence às candidatas U103 já carimbadas, portanto não representa a L5 fora163. (`app/chamados/services/materializacao.py:321`; `app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:331`; `app/chamados/services/materializacao.py:332`.)

- **A lista do comando já cobre fora163 sem dia.** Ela exclui U163, consulta `competencia_do_fato` e acrescenta `None` à pauta falante. Não depende do contador para listar essa situação. (`app/chamados/management/commands/bug_par_103.py:71`; `app/chamados/management/commands/bug_par_103.py:72`; `app/chamados/management/commands/bug_par_103.py:74`; `app/chamados/management/commands/bug_par_103.py:75`; `app/chamados/management/commands/bug_par_103.py:77`.)

- **Leitores de tela precisam reconhecer o estado novo**, como já autorizado pela matriz: hoje o card e o modal mostram detalhe/escondem Validar pela comparação com `bloqueada_competencia_entregue`; apenas trocar o vocabulário deixa essas comparações sem o novo nome. (`app/templates/chamados/partials/_pergunta_card.html:44`; `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/modal_fio.html:252`; `app/templates/chamados/partials/modal_fio.html:274`.)

- **Q1/Q3 não oferecem autorização adicional para exportada ou dinheiro.** A validação existente chama recálculo por evento depois de carimbar; ausência de nova batida não prova ausência de outras escritas. Tocar gravado exportado continua exigindo `!`; efeito produtivo concreto é [nao verificado]. (`app/chamados/services/validacao.py:617`; `app/chamados/services/validacao.py:627`; `app/chamados/services/validacao.py:628`; `CLAUDE.md:265`; `CLAUDE.md:411`.)

## 16/11 17:01 · sessao ef3aaae5c3ee · - · - · origem=Ronald (tmux)

Q4 é construível por inspeção, sem alterar K6: a fixture mantém `competencia_entregue=False`, declara `U60=U163={saida}` e oferece as horas coincidentes com os marcos. (`app/chamados/tests/test_bug_par_projetado.py:51`; `app/chamados/tests/test_bug_par_projetado.py:79`; `app/chamados/tests/test_bug_par_projetado.py:203`; `app/chamados/tests/test_bug_par_projetado.py:209`.)

A construção existente já mostra contador com `universo=1`, seguida de validação administrativa da volta, gravação das duas metades e `universo=0`. A asserção de gravação exige `materializada_em` presente e `VIA_DECLARACAO` nas duas. (`app/chamados/tests/test_bug_par_projetado.py:211`; `app/chamados/tests/test_bug_par_projetado.py:212`; `app/chamados/tests/test_bug_par_projetado.py:215`; `app/chamados/tests/test_bug_par_projetado.py:217`; `app/chamados/tests/test_bug_par_projetado.py:103`.)

Por consequência estática, essa fixture permite exigir `pendentes_aval: 1→0`, `resolvidas: 0→1` e `congelados_no_recorte=1`: U103 está vazio na fixture, perguntas materializadas não elegíveis ao predicado U103 vão a `resolvidas`, e o total congelado depende da seleção por PK, não do estado. (`app/chamados/tests/test_bug_par_projetado.py:47`; `app/chamados/services/materializacao.py:321`; `app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:356`.)

Q4 fecha a distinção entre recorte resolvível e zero global e preserva a contabilização de L3: `espera_declarado` entra em `universo`, que soma em `pendentes_aval`. A identificação literal da métrica na prova deve ser `pendentes_aval`, pois `congelados_no_recorte` permanece constante. (`app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:351`; `app/chamados/services/materializacao.py:356`.)

O teste F4 atual ainda não prova a nova exigência: ele obtém zero mudando K6 para verdadeiro, encaminhando a pergunta a Pauta DP. (`app/chamados/tests/test_bug_par_103.py:89`; `app/chamados/tests/test_bug_par_103.py:93`.) A execução da fixture com as novas asserções é [nao verificado].

## 16/11 16:38 · sessao 055290d9de8e · AUSENCIA-EFEITO-PELO-ESTADO · sonda · origem=Code

Não executei a sonda em produção; reproduzo literalmente a saída fornecida. [nao verificado] [nao verificado]

```text
sonda_leitura: sonda_rejeitadas_que_abonam.py [b4c16ca1b67d644c] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

STATUS existentes {'aprovada': 4476, 'aguardando_decisao': 160, 'rejeitada': 404, 'aguardando_documento': 10}
STATUS sem efeito considerados ['rejeitada']
UNIVERSO ausencias com status sem efeito 404 | por efeito do tipo {'abona': 201, 'desconta': 203}
COM TIPO QUE ABONA 201 | por status {'rejeitada': 201} | por tipo {'atestado': 170, 'abon_hr': 2, 'ferias': 4, 'declaracao': 10, 'reciclagem': 1, 'aviso_home_office': 1, 'folga_abonada': 1, 'troca_de_plantao': 5, 'folga_compensatoria': 1, 'folga': 2, 'abono': 3, 'licenca_luto': 1}
total_erros 0
```

Resumo dos números fornecidos: 404 rejeitadas, sendo 201 com tipo que abona e 203 com tipo que desconta; 0 erros reportados. [nao verificado]

## 16/11 16:38 · sessao ea09b070c83e · AUSENCIA-EFEITO-PELO-ESTADO · leitura · origem=Code

**A coluna Efeito mostra o efeito cadastrado para o tipo, independentemente do status da ausência.** Por isso o código permite “ABONA” junto de “Rejeitada”: são duas leituras separadas. Isso não demonstra abono aplicado no cálculo. `app/ponto/models.py:850`, `app/templates/ponto/ausencias.html:158`, `app/templates/ponto/ausencias.html:176`

O registro específico, o cadastro vigente em produção e eventual repercussão financeira não estão demonstrados pelo dado fornecido. [não verificado]

**(a) Caminho da coluna Efeito**

| Etapa | Origem e leitura |
|---|---|
| Rota | `/ponto/ausencias/` chama `lista_ausencias`. `app/ponto/urls.py:53` |
| View/consulta | `lista_ausencias` obtém as ausências vigentes; `?registro=` permite buscar diretamente pelo PK. `app/ponto/views.py:2165`, `app/ponto/views.py:2188`, `app/ponto/views.py:2191` |
| Contexto | Materializa `ausencias_lista` e entrega como **`ausencias`** ao template `ponto/ausencias.html`. `app/ponto/views.py:2248`, `app/ponto/views.py:2254` |
| Template | Itera `{% for a in ausencias %}`; a célula Efeito lê **`a.efeito`** e escolhe ABONA/DESCONTA/SUPRIME. Não testa status nessa célula. `app/templates/ponto/ausencias.html:142`, `app/templates/ponto/ausencias.html:158` |
| Tag/filtro | A leitura é direta da propriedade, sem tag ou filtro intermediário de efeito. `app/templates/ponto/ausencias.html:158` |
| Propriedade | **`Ausencia.efeito` → `efeito_vigente(self.tipo)`**. Nenhuma consulta a `self.status`. `app/ponto/models.py:850` |
| Catálogo | `efeito_vigente(tipo)` retorna o efeito do catálogo vigente. O catálogo lê `TipoAusencia` ativo — incluindo `TipoAusencia.efeito` — com fallback para `CATALOGO`. `app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:278`, `app/ponto/catalogo/ausencias.py:293` |
| Status separado | Outra célula testa `aprovada`/`rejeitada` e renderiza o badge correspondente. `app/templates/ponto/ausencias.html:176` |

Os contadores e filtros “Abona/Desconta/Suprime” também classificam **pelo tipo**; os contadores são montados antes do filtro de status. `app/ponto/views.py:2201`, `app/ponto/views.py:2208`, `app/ponto/views.py:2220`

**(b) Outros leitores: onde o tipo basta e onde o status já interfere**

| Leitor | O que exibe/lê | Rejeitada ou aberta muda a apresentação? |
|---|---|---|
| **Lista de ausências** | ABONA/DESCONTA/SUPRIME por `a.efeito`. `app/templates/ponto/ausencias.html:158` | **Não muda o efeito exibido.** Status aparece separadamente. `app/templates/ponto/ausencias.html:176` |
| **Drawer — “Esta ausência”** | Nome por `a.get_tipo_display`; status no cabeçalho. O seletor de edição agrupa tipos em COM/SEM ABONO. `app/templates/ponto/partials/painel_ausencia.html:24`, `app/templates/ponto/partials/painel_ausencia.html:48`, `app/templates/ponto/partials/painel_ausencia.html:152`, `app/ponto/catalogo/ausencias.py:313` | O status muda o cabeçalho e as ações; não muda o nome do tipo nem os grupos do seletor. Não há nessa montagem a coluna Efeito da lista. `app/templates/ponto/partials/painel_ausencia.html:24`, `app/templates/ponto/partials/painel_ausencia.html:293`, `app/templates/ponto/partials/painel_ausencia.html:320` |
| **Drawer — Histórico** | Imprime `h.rotulo`/`h.cor`, vindos de `linha_do_tempo`. `app/ponto/views.py:2017`, `app/templates/ponto/partials/painel_ausencia.html:191`, `app/templates/ponto/partials/painel_ausencia.html:198` | **Sim.** Abertas viram pendentes; rejeitada prevalece sobre o efeito do tipo. `app/ponto/services/dia_decidido.py:448`, `app/ponto/services/dia_decidido.py:461`, `app/ponto/services/dia_decidido.py:610` |
| **Grade/calendário — chip** | Ícone/rótulo pelo tipo; indicador `falta` por `_a.efeito == DESCONTA`. `app/colaboradores/services/calendario.py:705`, `app/templates/colaboradores/partials/_calendario_grade.html:97` | A montagem do chip não testa status, **mas sua entrada já é cobertura de cobrança**: rejeitada fica fora; abertas podem entrar. `app/colaboradores/services/calendario.py:516`, `app/ponto/turnos.py:2531` |
| **Grade/calendário — palavra do dia** | Palavra/cor/veredito por `do_dia`, usando cobertura de folha e decisões. `app/colaboradores/services/calendario.py:564`, `app/colaboradores/services/calendario.py:747`, `app/templates/colaboradores/partials/_calendario_grade.html:98` | **Sim.** A decisão pode representar rejeição; pedidos abertos têm tratamento próprio. Cobertura de folha tem precedência na composição. `app/ponto/services/dia_decidido.py:363`, `app/ponto/services/dia_decidido.py:420`, `app/ponto/services/dia_decidido.py:461` |
| **Espelho web/PWA — chip** | Ícone e nome do tipo truncado; cor distingue literalmente `tipo == 'falta'`. `app/templates/ponto/espelho.html:191` | Rejeitada fica fora pela cobertura de cobrança. Abertas entram; o sufixo `(pend.)` procura somente o literal legado `pendente`. `app/ponto/services/espelho.py:582`, `app/templates/ponto/espelho.html:191` |
| **Cartão PDF** | A coluna Obs imprime `dia['palavra_dia']`, preparada pela composição compartilhada. `app/relatorios/pdf_espelho.py:636`, `app/relatorios/pdf_espelho.py:923`, `app/ponto/services/espelho.py:340` | **Sim.** Não desenha o efeito diretamente de `a.efeito`; recebe a palavra que considera cobertura, rejeição e pendência. `app/ponto/services/dia_decidido.py:399`, `app/ponto/services/dia_decidido.py:420`, `app/ponto/services/dia_decidido.py:461` |
| **App — API do espelho** | Publica `veredito_dia`, `palavra_dia`, `cor_dia` e `ausencia_pendente`. `app/api/views.py:1601`, `app/api/views.py:1619` | Recebe a palavra julgada; a pendência usa `ABERTAS`. A renderização efetiva no cliente móvel é [não verificado]. `app/api/views.py:1567`, `app/api/views.py:1574` |
| **App — GET `/api/ausencias/`** | Retorna nome do tipo, status e `status_display` separados; não retorna campo `efeito`. `app/api/views.py:1859`, `app/api/views.py:1863` | **Status não altera o nome retornado.** Rejeitadas permanecem elegíveis ao histórico de até 30 registros. O nome vem do cadastro e pode carregar texto de abono; o fallback de atestado contém “com abono”. `app/api/views.py:1853`, `app/api/views.py:1865`, `app/ponto/catalogo/ausencias.py:250`, `app/ponto/catalogo/ausencias.py:95` |
| **PWA — solicitar ausência** | Grupos COM ABONO/SEM ABONO e `data-pago` nas opções. É apresentação do tipo escolhido para novo pedido. `app/templates/ponto/submeter_ausencia.html:24`, `app/templates/ponto/submeter_ausencia.html:27`, `app/ponto/catalogo/ausencias.py:320` | Essa escolha não recebe status de um registro existente. `app/ponto/views.py:2631`, `app/templates/ponto/submeter_ausencia.html:21` |
| **Relatórios — Absenteísmo e CSV** | Tipo, ocorrências e dias; não imprime badge de efeito. `app/relatorios/views.py:209`, `app/relatorios/views.py:222` | **Só aprovadas entram.** `app/relatorios/views.py:197` |
| **Relatórios — Atestados acumulados e CSV** | “Dias empresa” por `abonam_vigentes`; “Dias INSS” para `atestado_inss`. `app/relatorios/views.py:276`, `app/relatorios/views.py:302`, `app/relatorios/views.py:313` | **Só aprovadas médicas entram.** `app/relatorios/views.py:277` |
| **Histórico anual do colaborador** | Nome do tipo, dias, “Empresa paga”/“INSS assume”; abono pelo conjunto vivo de tipos. `app/templates/ponto/historico_atestados.html:23`, `app/templates/ponto/historico_atestados.html:40`, `app/ponto/views.py:2951` | **Só aprovadas médicas entram.** `app/ponto/views.py:2916` |
| **Atestados em lote** | Reutiliza `linha_do_tempo` e depois filtra tipo/status. `app/ponto/services/atestados_lote.py:54`, `app/ponto/services/atestados_lote.py:57`, `app/ponto/services/atestados_lote.py:62` | **Sim.** Herda o tratamento de pendente/rejeitada da linha do tempo. `app/ponto/services/dia_decidido.py:610` |
| **Exportação da folha** | Tipos que descontam alimentam faltas; tipos abonados, menos `FORA_DO_8932`, alimentam ausência justificada. `app/folha/export.py:138`, `app/folha/export.py:193`, `app/folha/export.py:201` | **Só aprovadas entram no universo consultado.** `app/folha/export.py:87` |

**(c) Status e autoridades existentes**

O vocabulário de **Ausencia** é `aguardando_documento`, `aguardando_decisao`, `aprovada`, `rejeitada`; o default é `aguardando_decisao`. `cancelada` não consta nesses choices. `app/ponto/models.py:701`, `app/ponto/models.py:809`

- **Cancelar individual/em lote pede `rejeitada`**, usando “cancelada” em mensagem/motivo. `app/ponto/views.py:2378`, `app/ponto/views.py:2811`
- **`cancelada` existe em `OrdemSubstituicao`**, outro modelo. `app/ponto/models.py:1051`, `app/ponto/models.py:1057`
- **`pendente` permanece como agrupamento da lista**, traduzido para as duas `ABERTAS`; ainda há leitores literais legados na API de chamados e no chip do espelho. `app/ponto/views.py:2212`, `app/ponto/services/ausencia.py:912`, `app/api/views.py:1061`, `app/templates/ponto/espelho.html:191`
- A máquina de transições está em `_TRANSICOES`, incluindo retorno de rejeitada para aprovada ou para uma das abertas. `app/ponto/services/ausencia.py:914`

As respostas existentes são distintas:

| Pergunta | Autoridade e resultado |
|---|---|
| **Qual o efeito deste tipo?** | `efeito_vigente(tipo)`; não considera status. `app/ponto/catalogo/ausencias.py:293` |
| **Esta ausência participa da cobertura financeira ou da cobrança?** | `_pelo_efeito(qs, para)`: **folha aceita somente aprovada**; **cobrança exclui rejeitada**. É composto por `ausencia_cobre` e `cobertura_ausencia_periodo`, que acrescentam período e desempate. `app/ponto/turnos.py:2523`, `app/ponto/turnos.py:2550`, `app/ponto/turnos.py:2747` |
| **Quais efeitos chegam ao cálculo neste período?** | `efeitos_da_ausencia`: entrega `justificadas`, `suprimidas`, `min_parcial_pagos`, `dia_inteiro`; consulta com `para='folha'`. ABONA justifica; SUPRIME retira previsto; parcial paga gera crédito em minutos. **Não é booleano por registro.** `app/ponto/services/efeito_ausencia.py:28`, `app/ponto/services/efeito_ausencia.py:47`, `app/ponto/services/efeito_ausencia.py:53`, `app/ponto/services/efeito_ausencia.py:63` |
| **Como apresentar o estado deste lançamento?** | `veredito_do_lancamento` trata abertas como pendentes; `veredito_de` passa tipo e status; `veredito_do_tipo` trata rejeitada antes do catálogo; `rotulo_de` compõe a palavra. `app/ponto/services/dia_decidido.py:439`, `app/ponto/services/dia_decidido.py:461`, `app/ponto/services/dia_decidido.py:476`, `app/ponto/services/dia_decidido.py:484` |

O espelho e o PDF consomem `efeitos_da_ausencia`; o motor recebe os conjuntos preparados e os usa para separar falta e abono. O fechamento também seleciona cobertura de folha antes de ler `a.efeito`. `app/ponto/services/espelho.py:1068`, `app/relatorios/pdf_espelho.py:332`, `app/ponto/motor_calculo_v2.py:2595`, `app/ponto/services/fechamento.py:683`

Há uma diferença adicional entre leitores: efeito desconhecido suprime no fechamento (`!= DESCONTA`), enquanto `efeitos_da_ausencia` exige igualdade com SUPRIME. Isso não demonstra ocorrência desse caso em produção. `app/ponto/services/fechamento.py:700`, `app/ponto/services/efeito_ausencia.py:61` [não verificado quanto à ocorrência]

**A célula não equivale ao efeito financeiro individual:** o cartório carimba cobertura de **cobrança**; `CelulaDia.ausencia` está declarado como sombra. Já a grade financeira consulta cobertura de **folha** antes de ler tipo/label. `app/ponto/services/cartorio.py:1356`, `app/ponto/services/cartorio.py:1873`, `app/escala/models.py:1788`, `app/escala/utils.py:1188`

**(d) Quem escreve status e tipo**

| Escritor/porta | Campos e chamadores |
|---|---|
| `criar_ausencia` | Monta `tipo` e `status` explícito ou inicial, instancia e grava. `app/ponto/services/ausencia.py:475`, `app/ponto/services/ausencia.py:483`, `app/ponto/services/ausencia.py:529`, `app/ponto/services/ausencia.py:562` |
| `editar_ausencia_registro` | Atribui `tipo`, grava e, se mudou, rejulga somente status aberto. `app/ponto/services/ausencia.py:648`, `app/ponto/services/ausencia.py:659`, `app/ponto/services/ausencia.py:674`, `app/ponto/services/ausencia.py:687` |
| `mudar_estado` | Valida transição, atribui `status` e grava. `app/ponto/services/ausencia.py:972`, `app/ponto/services/ausencia.py:976`, `app/ponto/services/ausencia.py:988` |
| `decidir` | Aprovação/rejeição delega à mudança de estado. Views individuais e de lote usam `_a4_decidir`. `app/ponto/services/ausencia.py:1065`, `app/ponto/services/ausencia.py:1088`, `app/ponto/views.py:85`, `app/ponto/views.py:2302`, `app/ponto/views.py:2338`, `app/ponto/views.py:2714`, `app/ponto/views.py:2775` |
| Documento/prazo/reabertura | Anexar pode levar a `aguardando_decisao`; vencimento também; reversão recalcula estado pelo tipo/anexo e passa pela porta. `app/ponto/services/ausencia.py:1033`, `app/ponto/services/ausencia.py:1180`, `app/ponto/services/ausencia.py:1298`, `app/ponto/services/ausencia.py:1320` |
| **Exceção: cancelamento de férias** | `rejeitar_ausencias_de_ferias` atribui diretamente `rejeitada`, grava e registra trilha; chamado pelo serviço de férias. `app/ponto/services/ausencia.py:1496`, `app/ponto/services/ausencia.py:1498`, `app/ponto/services/ausencia.py:1499`, `app/ferias/services.py:683` |
| Cadastro `TipoAusencia` | A tela escreve efeito/nome/flags por `gravar_tipo_ausencia`; isso é escrita do **cadastro do tipo**, diferente de trocar `Ausencia.tipo`. `app/ponto/views_tipos_ausencia.py:67`, `app/ponto/views_tipos_ausencia.py:82`, `app/ponto/services/ausencia.py:1761` |

Chamadores da criação, com seus estados solicitados:

- Admin: aprovada; PWA e API deixam o estado inicial ao serviço. `app/ponto/views.py:2472`, `app/ponto/views.py:2477`, `app/ponto/views.py:2607`, `app/api/views.py:1812`
- Veredito da célula e checagem do copiloto: aprovada. `app/ponto/services/veredito_celula.py:337`, `app/ponto/services/checar_ausencia.py:46`
- Materialização de disputa: tipo/status conforme resposta; regularização e abandono provisório também passam pela criação. `app/chamados/services/materializacao.py:491`, `app/chamados/services/materializacao.py:527`, `app/ponto/services/regularizacao.py:118`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76`
- Espelho de férias: tipo `ferias`, aprovada. `app/ponto/services/ausencia.py:1389`

O formulário valida o tipo sem escrever o registro; o Django Admin de Ausencia é somente leitura; `Ausencia.save` lê status/tipo para derivar carimbo, minutos e período, sem atribuir esses dois campos nesse corpo. `app/ponto/forms.py:41`, `app/ponto/forms.py:60`, `app/ponto/admin.py:38`, `app/ponto/models.py:878`

**Quem lê, além das superfícies de (a)/(b) e do cálculo de (c):**

| Leitor | Uso de status/tipo |
|---|---|
| Propriedades do modelo | Aberta, exige documento, pode aprovar e efeito. `app/ponto/models.py:710`, `app/ponto/models.py:723`, `app/ponto/models.py:732`, `app/ponto/models.py:850` |
| Precedência | Exclui rejeitadas; usa tipo/período para o fato vencedor. `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:408` |
| Emissão da jornada | Consulta ausências para cobrança. `app/escala/servico_jornada.py:347` |
| Silêncio de cobrança | Diferencia abertas/aprovadas e usa tipo na trilha. `app/chamados/services/silencio_ausencia.py:66`, `app/chamados/services/silencio_ausencia.py:78`, `app/chamados/services/silencio_ausencia.py:94` |
| Crédito parcial | Seleciona folha e tipo pago/parcial para somar minutos. `app/ponto/services/credito_parcial.py:26` |
| Signals | Usa tipo/intervalo para acordar os dias atingidos no cartório. `app/ponto/signals.py:115`, `app/ponto/signals.py:143` |
| Alertas médicos | Cobertura de folha e tipos médicos. `app/ponto/services/alertas_ausencia.py:48` |
| Vigias | Estado vivo, documento/decisor, parcialidade e divergência com veredito. `app/ponto/services/vigia_ausencia.py:59`, `app/ponto/services/vigia_ausencia.py:108`, `app/ponto/services/vigia_ausencia.py:116`, `app/ponto/services/vigia_ausencia_veredito.py:46` |
| Contador “sem efeito na célula” | Cobertura de folha; exclui DESCONTA e parciais dessa classe de divergência. `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49` |
| Consistência de férias | Compara tipo/status/origem com agendamento. `app/ferias/services.py:147`, `app/ferias/services.py:155` |
| Cadastro de tipos | Conta usos de `Ausencia.tipo`. `app/ponto/views_tipos_ausencia.py:23` |

**Anexo:** [app/docs/JUIZES.md](/home/ronald/codex_ws/ebc7af1a97d9b6ed91910127b1d9a1541551f90b/app/docs/JUIZES.md:1). O documento aponta cobertura e `ABERTAS`, explicita que `pendente` não é status e declara primazia do código vivo; o registro distingue efeito do tipo, cobertura, estado inicial e aberta/decidida. `app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:55`, `app/core/juizes.py:260`

_6120 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
