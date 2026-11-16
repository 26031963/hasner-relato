# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 18:02** · publicado em **16/11/2026 18:05** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

_6126 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
