# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 19:33** · publicado em **30/10/2026 19:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 19:33 · sessao f393335dd52b · O261-V-FASE · verificador · origem=Code

**Achei um ponto funcional procedente: a CLI exige o vínculo “lido na listagem”, mas não o exibe.** O argumento é obrigatório e filtra corretamente a proposta; porém a impressão omite `vinculo_id`. Duas propostas iguais, de vínculos distintos, ficam indistinguíveis para quem precisa informar `--vinculo`. `app/escala/management/commands/propostas_de_escala.py:23`, `:33`, `:39`, `:80`.

**1. Quem escreve e quem lê o vínculo**

- **CLI:** o operador fornece `--vinculo`; o parser recebe, o filtro combina colaborador e vínculo, e aplicar/recusar recebem a proposta selecionada. Falta imprimir o EC na listagem. `app/escala/management/commands/propostas_de_escala.py:23`, `:39`, `:63`, `:68`, `:80`.
- **Fio:** `proposta_do_fio` fornece `vinculo_id`; os dois formulários escrevem o hidden; `processar_post_fio` lê e repassa nos dois verbos; o recálculo exige correspondência do vínculo. Não encontrei bypass nesse percurso. `app/chamados/services/proposta_no_fio.py:79`, `:109`, `:134`, `:169`; `app/templates/chamados/partials/modal_fio.html:52`, `:59`; `app/chamados/services/fio.py:509`, `:511`, `:514`; `app/escala/services/propositor.py:685`.
- **Chat:** o preview guarda o vínculo na sessão; o aceite o recupera e filtra junto da assinatura. Sessão antiga sem essa chave passa `''`, que não corresponde ao EC. `app/core/views_ia.py:578`, `:620`, `:650`; `app/escala/services/propositor.py:685`.
- **Escrita final:** a porta recebe o vínculo da proposta; decisões aceita e recusada também o registram. `app/escala/services/propositor.py:718`, `:766`, `:774`.

Ocorrência desses percursos em produção: **[nao verificado]**.

**2. Imports, código morto e referências antigas**

Não encontrei import local inexistente na análise estática dos `from` absolutos de `app`; execução real e cobertura dos imports relativos: **[nao verificado]**. Os candidatos sem definição estática `ENVELOPE_LOTE_MIN` e `ROTULO_LOTE` existem via `__getattr__`. `app/chamados/services/validacao.py:765`.

Há parâmetros sem leitura nos corpos da cópia:

- `empresas` em `_fase_deslocada`, `_termometro`, `_vinculo_divergente` e `_teto_do_dia`. O filtro efetivo da lista ocorre depois. `app/escala/services/cadastro_realidade.py:335`, `:285`, `:303`, `:315`, `:635`.
- `dias_bat` e `dias_janela` em `_propor_pausa`; `dias_bat` em `_chamados_que_morreriam`; `hoje` em `_recusadas_recentes`. `app/escala/services/propositor.py:280`, `:617`, `:693`.
- `hoje` em `linhas_para_a_pauta`. `app/chamados/services/fila_por_causa.py:379`.

A introdução desses resíduos **nesta obra** é **[nao verificado]**. Os parâmetros com `_` de `dono_do_dia` estão explicitamente reservados à decisão de lei aberta. `app/escala/services/fase_12x36.py:78`, `:88`.

Não encontrei teste importando/executando os antigos `fora_de_fase/JANELA_DIAS/PISO/CICLOS` de `fase_12x36`. A referência executável restante a `fora_de_fase` verifica sua ausência; outras são lápides/documentação. `app/escala/tests/test_o261_v_fase_na_lista.py:223`; `app/escala/tests/test_fase_12x36.py:3`; `app/chamados/tests/test_proposta_no_fio.py:186`.

**3. Os testes mordem o chamador?** [nao verificado]

**Sim, no repasse do POST:** exercitam `processar_post_fio` para aplicar e recusar e exigem `vinculo_da_tela='9'`. O render exige os dois hidden. `app/escala/tests/test_o261_v_fase_na_lista.py:434`, `:438`, `:439`; `app/chamados/tests/test_proposta_faixa_na_tela.py:85`, `:91`.

O limite é preciso: o POST substitui os serviços por mocks; o cenário A desaparece/B tem mesma assinatura e seu controle positivo estão no auxiliar `_recalcular`. `app/escala/tests/test_o261_v_fase_na_lista.py:437`, `:418`, `:422`, `:425`.

A CLI é exercitada por `call_command`, mas somente para aplicar sem vínculo. Não verifica a impressão do EC nem a seleção positiva — deixou escapar o achado da listagem. `app/escala/tests/test_o261_v_fase_na_lista.py:444`, `:451`, `:453`.

Os intervalos históricos exatos são **[nao verificado]**, pois a cópia não contém `.git`. Execução da suíte Django: **[nao verificado]**. [nao verificado]

## 30/10 19:32 · sessao 6430dea1929f · - · - · origem=Ronald (tmux)

Achado de limpeza: `empresas` não é lido em `_fase_deslocada(empresas=None)`; o chamador passa esse argumento, mas o corpo apenas lê/formata a lavra. O filtro efetivo por empresa ocorre depois, em `lista()`. Isso não demonstra vazamento de escopo. Origem no intervalo `d80c98a5..2177fa08`: [nao verificado]. `app/escala/services/cadastro_realidade.py:335`, `:337`, `:348`, `:610`, `:635`.

Outros parâmetros sem leitura na cópia, cuja origem histórica é [nao verificado]:

- `_propor_pausa`: `dias_bat`, `dias_janela`. `app/escala/services/propositor.py:280`.
- `_chamados_que_morreriam`: `dias_bat`. `app/escala/services/propositor.py:617`.
- `_recusadas_recentes`: `hoje`; corte usa `timezone.now()`. `app/escala/services/propositor.py:693`, `:700`.
- `_termometro`, `_vinculo_divergente`, `_teto_do_dia`: `empresas`. `app/escala/services/cadastro_realidade.py:285`, `:303`, `:315`.
- `linhas_para_a_pauta`: `hoje`. `app/chamados/services/fila_por_causa.py:379`.

Os argumentos `_colab_id`, `_dia`, `_ec_do_dia` de `dono_do_dia` são explicitamente reservados à decisão de lei aberta; não classifiquei como resíduo acidental. `app/escala/services/fase_12x36.py:78`, `:81`, `:88`.

Não encontrei import local inexistente na varredura AST dos `from` absolutos locais de `app`; execução real desses imports: [nao verificado]. Os únicos nomes sem definição estática encontrados (`ENVELOPE_LOTE_MIN`, `ROTULO_LOTE`) são fornecidos por `__getattr__`. `app/chamados/services/validacao.py:765`, `:767`, `:769`.

Não encontrei teste executando/importando `fora_de_fase`, `JANELA_DIAS`, `PISO` ou `CICLOS` de `fase_12x36`. A referência executável restante a `fora_de_fase` exige sua ausência; demais ocorrências são comentários/lápides. `app/escala/tests/test_o261_v_fase_na_lista.py:223`, `:226`; `app/escala/tests/test_fase_12x36.py:3`; `app/chamados/tests/test_proposta_no_fio.py:186`.

Todas as funções/constantes de topo de `fase_12x36`, `propositor`, `censo_fase_12x36` e `proposta_no_fio` tiveram referência AST de leitura/importação encontrada; isso não prova alcance em produção. [nao verificado].

## 30/10 19:32 · sessao 92ffc021dfdb · - · - · origem=Ronald (tmux)

Achado procedente: **a CLI exige um vínculo que sua listagem não informa**. `--vinculo` é anunciado como “o vínculo da proposta que você leu na listagem” e obrigatório para decidir (`app/escala/management/commands/propostas_de_escala.py:23`, `:33`), mas o loop imprime somente colaborador, template, âncoras, casamento e chamados; nunca `vinculo_id` (`:80`, `:83`, `:89`, `:93`). Duas propostas iguais exceto pelo vínculo aparecem indistinguíveis. Reprodução somente em memória com a classe `Command` e propostas fictícias confirmou isso; ocorrência concreta em produção [nao verificado].

Fluxo conferido:

- CLI: humano fornece `--vinculo`; parser lê em `propostas_de_escala.py:23`; filtro seleciona por colaborador **e** vínculo em `:39`; aplicação/recusa recebem essa proposta em `:63`, `:68`. Falta o escritor do ID na listagem (`:80`).
- Fio: `proposta_do_fio` fornece o ID em `app/chamados/services/proposta_no_fio.py:79`; os dois formulários escrevem o hidden em `app/templates/chamados/partials/modal_fio.html:52`, `:59`; POST lê `''` por omissão e repassa em `app/chamados/services/fio.py:509`, `:511`, `:514`; os serviços passam ao recálculo em `proposta_no_fio.py:134`, `:169`; o auxiliar compara o vínculo em `app/escala/services/propositor.py:685`. Não identifiquei bypass nesse caminho.
- Chat: preview guarda o vínculo em sessão em `app/core/views_ia.py:620`; aceite lê com `''` por omissão em `:650`; helper repassa à seleção por assinatura/vínculo em `:578`. Não identifiquei bypass nesse caminho.
- Escrita final usa `p['vinculo_id']` na porta em `app/escala/services/propositor.py:766`; decisão aceita e recusada registram o ID em `:774`, `:718`.
- Caminhos produtivos encontrados são CLI, fio e chat: chamadas diretas em `propostas_de_escala.py:63`, `:68`, `proposta_no_fio.py:147`, `:172`, `views_ia.py:660`; escritor dinâmico declarado em `app/core/intake.py:51`.

Testes relacionados: POST exercita o chamador real nos dois verbos e confere o repasse (`app/escala/tests/test_o261_v_fase_na_lista.py:428`, `:434`, `:439`); render exige dois hidden com o ID (`app/chamados/tests/test_proposta_faixa_na_tela.py:85`, `:91`). Teste CLI cobre ausência de vínculo somente no aplicar (`test_o261_v_fase_na_lista.py:444`, `:451`); não cobre sua impressão na listagem nem seleção positiva do EC (`:448`).

Atribuição exata ao delta histórico [nao verificado]: a cópia disponibilizada não contém `.git`.

## 30/10 19:32 · sessao de6d5691a76e · - · - · origem=Ronald (tmux)

Achado procedente: a CLI exige o EC “lido na listagem”, mas não o imprime. `--vinculo` é definido e exigido em `app/escala/management/commands/propostas_de_escala.py:23` e `:33`; a seleção usa `vinculo_id` em `:39`. Porém a listagem em `:80`–`:98` imprime colaborador, tipo, templates, âncoras, data e chamados, sem EC. Duas propostas iguais exceto pelo vínculo produzem blocos indistinguíveis; confirmei executando `Command.handle` extraído por AST em memória, com módulos falsos e `python3 -B`.

Os testes mordem estes chamadores: [nao verificado]

- `app/escala/tests/test_o261_v_fase_na_lista.py:434` percorre aplicar e recusar, chama `processar_post_fio` em `:438` e exige o repasse `vinculo_da_tela='9'` em `:439`. Tirar o repasse em `app/chamados/services/fio.py:511` ou `:514` quebra a asserção.
- `app/chamados/tests/test_proposta_faixa_na_tela.py:85` renderiza a faixa e exige dois hidden com o vínculo em `:91`; os campos ficam nos dois formulários em `app/templates/chamados/partials/modal_fio.html:52` e `:59`.
- O cenário A sumiu/B mesma assinatura testa o auxiliar `_recalcular`: rejeita vínculo A em `app/escala/tests/test_o261_v_fase_na_lista.py:418`, rejeita vínculo vazio em `:422` e aceita B como controle positivo em `:425`.
- A CLI é chamada de verdade por `call_command` em `app/escala/tests/test_o261_v_fase_na_lista.py:451`; o teste exige nenhuma aplicação e mensagem `--vinculo` em `:452` e `:453`.

Limites: o teste HTTP substitui os serviços de aplicar/recusar por mocks (`app/escala/tests/test_o261_v_fase_na_lista.py:437`), portanto verifica o repasse do chamador, não a cadeia HTTP até o escritor. O controle positivo está no auxiliar, não no POST inteiro (`:424`). O teste CLI cobre aplicar sem vínculo, sem verificar a saída da listagem nem a seleção positiva (`:444`). Isso deixou escapar a ausência do EC na listagem.

Suite Django executada: [nao verificado]. Atribuição exata desses trechos ao delta, sem `.git`: [nao verificado]. [nao verificado]

## 30/10 19:28 · sessao eb56abb9e8b6 · O261-V-FASE · verificador · origem=Code

A cura impede A→B no fio e no chat quando o vínculo original acompanha a decisão. Restam lacunas nos testes e caminhos sem identidade do preview na CLI e entre abas do chat. (`app/escala/services/propositor.py:682`, `app/chamados/services/fio.py:511`, `app/core/views_ia.py:650`, `app/escala/management/commands/propostas_de_escala.py:26`, `app/core/views_ia.py:871`)

Comparação literal de `ed31456f..23c0d9dc`: [não verificado], pois a cópia disponibilizada não contém `.git`. A atribuição dos residuais abaixo ao delta, em vez de comportamento anterior, fica [não verificado].

**1. Quem escreve e quem lê**

| Caminho | Escrita/origem e leitura do vínculo |
|---|---|
| Fio: produção e template | `proposta_do_fio` escolhe `p` e extrai dela assinatura e vínculo; os formulários de aplicar e recusar escrevem ambos em hidden. (`app/chamados/services/proposta_no_fio.py:66`, `app/chamados/services/proposta_no_fio.py:75`, `app/chamados/services/proposta_no_fio.py:79`, `app/templates/chamados/partials/modal_fio.html:51`, `app/templates/chamados/partials/modal_fio.html:58`) |
| Fio: POST e serviços | `fio.py` lê `vinculo_proposta`, usa `''` quando ausente e repassa aos dois verbos; ambos chegam a `_recalcular`, que passa o vínculo a `da_assinatura`. (`app/chamados/services/fio.py:509`, `app/chamados/services/fio.py:511`, `app/chamados/services/fio.py:513`, `app/chamados/services/proposta_no_fio.py:134`, `app/chamados/services/proposta_no_fio.py:169`, `app/chamados/services/proposta_no_fio.py:109`) |
| Chat: preview e sessão | O preview guarda assinatura e `p.get('vinculo_id')` na sessão e monta o texto com essa mesma `p`. O “sim” lê a proposta da sessão; `_aplicar_escala` repassa seu vínculo para `_proposta_de_escala`, que chama `da_assinatura`. (`app/core/views_ia.py:620`, `app/core/views_ia.py:623`, `app/core/views_ia.py:871`, `app/core/views_ia.py:650`, `app/core/views_ia.py:578`) |
| Auxiliar | `da_assinatura` filtra por pessoa, assinatura e vínculo; `None` desliga o filtro de vínculo. (`app/escala/services/propositor.py:682`) |
| Escrita final | Aplicar entrega `p['vinculo_id']` à porta; aceite e recusa gravam o vínculo em `DecisaoProposta`. (`app/escala/services/propositor.py:766`, `app/escala/services/propositor.py:774`, `app/escala/services/propositor.py:718`) |
| CLI | Não recebe assinatura nem vínculo da listagem: recalcula, filtra pelo colaborador, recusa multiplicidade e decide sobre a proposta única atual. (`app/escala/management/commands/propostas_de_escala.py:11`, `app/escala/management/commands/propostas_de_escala.py:26`, `app/escala/management/commands/propostas_de_escala.py:30`, `app/escala/management/commands/propostas_de_escala.py:52`) |

**2. Falhas, contraexemplos e descartes**

- **Residual procedente — sessão compartilhada entre abas.** Aba 1 mostra A; aba 2 gera preview B e sobrescreve `proposta_ia`; o usuário volta à aba 1 e envia “sim”. O POST não identifica o preview: envia pergunta/histórico/CSRF. A view consome B da sessão, valida B e aplica B. Portanto, o vínculo pode diferir daquele do preview ainda visível na aba que confirmou. (`app/core/views_ia.py:88`, `app/core/views_ia.py:620`, `app/templates/core/_copiloto.html:172`, `app/core/views_ia.py:106`, `app/core/views_ia.py:871`, `app/core/views_ia.py:650`, `app/core/views_ia.py:660`) Ocorrência em produção: [não verificado].

- **Residual procedente — CLI sem vínculo da leitura anterior.** Contraexemplo: a listagem mostrou A; A desaparece e sobra B única; `--aplicar COLAB` aplica B, ou `--recusar COLAB` recusa B. A guarda cobre propostas simultâneas, mas não liga a decisão à listagem anterior. (`app/escala/management/commands/propostas_de_escala.py:26`, `app/escala/management/commands/propostas_de_escala.py:31`, `app/escala/management/commands/propostas_de_escala.py:38`, `app/escala/management/commands/propostas_de_escala.py:53`, `app/escala/management/commands/propostas_de_escala.py:58`)

- **`str` × `int`: descartado.** `'1'` do formulário casa com `1` da proposta porque os dois lados passam por `str`. Tela antiga sem campo e sessão antiga sem chave viram `''`, que não casa com IDs normais. (`app/escala/services/propositor.py:685`, `app/chamados/services/fio.py:509`, `app/core/views_ia.py:651`)

- **`None` permanece uma abertura no auxiliar.** Contraexemplo: A some, sobra B com a mesma assinatura e o chamador omite o vínculo; `None` desliga o filtro e B é selecionada. Os chamadores HTTP atuais usam `''` para campo/chave ausente, evitando esse caso. (`app/escala/services/propositor.py:668`, `app/escala/services/propositor.py:685`, `app/chamados/services/fio.py:509`, `app/core/views_ia.py:651`)

- **Proposta sem `vinculo_id`: fragilidade condicional, sem origem normal demonstrada.** O gerador sempre acrescenta `ec.pk`. Se chegasse uma proposta incompleta ao preview do chat, ele guardaria `None`, posteriormente desativando o filtro; o texto `'None'` também casa com um ID ausente no auxiliar. Isso não demonstra falha produtiva, pois contradiz a saída normal do gerador. (`app/escala/services/propositor.py:539`, `app/core/views_ia.py:622`, `app/core/views_ia.py:651`, `app/escala/services/propositor.py:685`) Ocorrência desse payload: [não verificado].

- **Recusa/L-042: vínculo gravado, silêncio preservado por conteúdo.** Contraexemplo: A e B da mesma pessoa têm a mesma assinatura; recusar A também cala B por 30 dias. A consulta e o filtro usam apenas `(colaborador, assinatura)`. O comentário declara expressamente a preservação dessa chave; não classifico isso como omissão acidental do transporte. (`app/escala/services/propositor.py:718`, `app/escala/services/propositor.py:663`, `app/escala/services/propositor.py:671`, `app/escala/services/propositor.py:700`, `app/escala/services/propositor.py:581`) No chat, texto diferente de “sim” remove a pendência, sem executar a recusa formal de escala. (`app/core/views_ia.py:882`)

**3. Os testes mordem os chamadores?** [nao verificado]

**Para o vínculo deste delta, não.** O novo “A some/B fica” chama diretamente `_recalcular`, com `'1'` e `''`; não atravessa formulário, `fio.py`, aplicar/recusar ou sessão do chat. Remover o repasse em `fio.py` não seria detectado por esse teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:408`, `app/escala/tests/test_o261_v_fase_na_lista.py:418`, `app/escala/tests/test_o261_v_fase_na_lista.py:422`, `app/chamados/services/fio.py:511`, `app/chamados/services/fio.py:514`)

O selo também carece de controle positivo com vínculo informado: uma implementação que recusasse **todo vínculo não nulo** satisfaria os dois casos negativos; o teste anterior chama sem vínculo. (`app/escala/tests/test_o261_v_fase_na_lista.py:404`, `app/escala/tests/test_o261_v_fase_na_lista.py:418`, `app/escala/tests/test_o261_v_fase_na_lista.py:422`)

Há testes HTTP anteriores do chat, mas cobrem proposta única, conteúdo alterado e desaparecimento. O selo novo do chat verifica apenas estreitamento por colaborador; o de renderização do fio verifica assinatura, sem afirmar o hidden de vínculo. (`app/core/tests/test_ia_escala_com_sim.py:49`, `app/core/tests/test_ia_escala_com_sim.py:86`, `app/core/tests/test_ia_escala_com_sim.py:98`, `app/core/tests/test_ia_escala_com_sim.py:111`, `app/escala/tests/test_o261_v_fase_na_lista.py:429`, `app/chamados/tests/test_proposta_faixa_na_tela.py:85`)

Execução da suíte Django e resultados em produção: [não verificado]. [nao verificado]

## 30/10 19:27 · sessao f0119dee472e · - · - · origem=Ronald (tmux)

- **Tipos:** `da_assinatura` compara `str(p.get('vinculo_id')) == str(vinculo_id)`, portanto POST `'1'` casa com ID inteiro `1`; reproduzi em memória, sem Django/BD. `None` desliga o filtro; `''` recusa IDs válidos. (`app/escala/services/propositor.py:682-690`)

- **CLI continua sem vínculo do que foi lido:** argumentos aceitam apenas colaborador; a execução recalcula e pega a proposta única atual. Contraexemplo: listou A, A desaparece, sobra B única; `--aplicar COLAB` aplica B; `--recusar COLAB` recusa B. A guarda só cobre duas propostas simultâneas, para ambas as ações. Isso é caminho residual no código atual; inclusão no delta é [não verificado]. (`app/escala/management/commands/propostas_de_escala.py:14-22,26-38,52-58`)

- **Recusa e silêncio:** a decisão grava o vínculo, mas a chave do silêncio continua `(colaborador_id, assinatura)` por 30 dias; assinatura exclui vínculo. Contraexemplo: A e B, mesma pessoa e conteúdo; recusar A cala B também. O próprio comentário declara essa preservação; não classificaria como regressão acidental do delta. (`app/escala/services/propositor.py:653-665,671-678,700-702,717-720,581-583`)

- **Proposta sem ID:** o gerador sempre acrescenta `vinculo_id=ec.pk`; não há evidência de saída real sem ID. No auxiliar, `vinculo_id=None` aceita proposta sem chave; `''` a recusa. `recusar` tolera FK nula; `aplicar` exige `p['vinculo_id']`. (`app/escala/services/propositor.py:539-543,685,718,766`)

- **Selo sem controle positivo:** o teste novo chama somente `_recalcular`, em dois casos negativos: A sumiu e tela antiga `''`. Uma mutação que recuse todo `vinculo_da_tela` não nulo passaria esses casos; o teste anterior chama sem vínculo. Não prova `fio.py`, formulário, nem sessão do chat. (`app/escala/tests/test_o261_v_fase_na_lista.py:395-423`)

- **Chat:** o teste ali somente verifica estreitamento por colaborador. Existem testes HTTP anteriores de confirmação, conteúdo diferente e desaparecimento, mas aqueles casos não exercitam troca A→B com mesma assinatura. (`app/escala/tests/test_o261_v_fase_na_lista.py:429-434`; `app/core/tests/test_ia_escala_com_sim.py:86-118`)

- **Testes de silêncio:** exercitam recusa, expiração e assinatura diferente com um EC; não cobrem A/B com mesma assinatura. (`app/escala/tests/test_porta_da_proposta.py:77-80,104-128`)

## 30/10 19:27 · sessao 77fbbcca86ad · - · - · origem=Ronald (tmux)

O chat transporta o vínculo da mesma proposta usada no preview: seleção em `app/core/views_ia.py:580`, gravação na sessão em `app/core/views_ia.py:620`, texto formado dessa proposta em `app/core/views_ia.py:623`, leitura no “sim” em `app/core/views_ia.py:650` e filtro em `app/escala/services/propositor.py:685`.

Há residual com duas abas: a sessão usa uma única chave (`app/core/views_ia.py:88`), sobrescrita a cada preview (`app/core/views_ia.py:620`). Contraexemplo: aba 1 mostra A; aba 2 produz B; “sim” digitado na aba 1 envia apenas pergunta/histórico/CSRF (`app/templates/core/_copiloto.html:172`), e a view consulta a proposta B da sessão (`app/core/views_ia.py:106`, `app/core/views_ia.py:871`), valida B (`app/core/views_ia.py:650`) e aplica B (`app/core/views_ia.py:660`). O vínculo recebido pode, portanto, diferir do preview ainda visível na aba que confirmou; origem no delta versus comportamento herdado e ocorrência produtiva: [não verificado].

Sessão antiga sem a chave `vinculo` recebe `''` (`app/core/views_ia.py:651`), que não casa IDs normais (`app/escala/services/propositor.py:685`). Sessão contendo explicitamente `None` desliga o filtro nesse mesmo ponto; o produtor normal injeta `ec.pk` (`app/escala/services/propositor.py:539`), portanto ocorrência desse payload incompleto: [não verificado].

Os testes existentes mordem a rota de chat, mas com uma proposta de vínculo 7 (`app/core/tests/test_ia_escala_com_sim.py:49`, `app/core/tests/test_ia_escala_com_sim.py:86`), conteúdo alterado (`app/core/tests/test_ia_escala_com_sim.py:98`) ou lista vazia (`app/core/tests/test_ia_escala_com_sim.py:111`). O teste novo de A sumir/B conservar assinatura chama apenas `_recalcular`, sem passar pelo chat ou pela sessão (`app/escala/tests/test_o261_v_fase_na_lista.py:408`). O selo do chat verifica apenas o estreitamento por colaborador (`app/escala/tests/test_o261_v_fase_na_lista.py:429`).

Texto diferente de “sim” apaga a proposta pendente (`app/core/views_ia.py:882`); esse caminho não registra recusa de escala em `DecisaoProposta`, cuja escrita está em `app/escala/services/propositor.py:705`.

## 30/10 19:27 · sessao 83aa580b3dfe · - · - · origem=Ronald (tmux)

Fluxo do fio conferido:

- `proposta_do_fio` obtém propostas da pessoa, seleciona `[0]` e produz tanto assinatura quanto vínculo desse mesmo objeto (`app/chamados/services/proposta_no_fio.py:63-79`).
- O template grava assinatura e vínculo nos dois formulários, aplicar e recusar (`app/templates/chamados/partials/modal_fio.html:48-59`).
- `processar_post_fio` lê o vínculo do POST como string, usa `''` quando ausente e repassa aos dois serviços (`app/chamados/services/fio.py:505-514`).
- Aplicar e recusar chamam `_recalcular` com o vínculo recebido; este passa o argumento a `da_assinatura` (`app/chamados/services/proposta_no_fio.py:109-110`, `:134`, `:169`).
- `da_assinatura` compara os vínculos convertidos para `str`; portanto `'1'` do POST casa com `1` da proposta. O vínculo ausente do POST fica `''` e não casa com IDs reais (`app/escala/services/propositor.py:682-690`).

Não achei desvio natural no transporte do vínculo pelo fio: os formulários e os dois verbos usam o vínculo da proposta apresentada (`app/templates/chamados/partials/modal_fio.html:51-59`, `app/chamados/services/fio.py:507-514`, `app/chamados/services/proposta_no_fio.py:75-79`).

Limites relevantes:

1. **O filtro continua opcional no auxiliar.** `vinculo_id=None` desliga a restrição. Contraexemplo: `_recalcular(colab, assinatura_de_A)` com apenas B restante, mesma assinatura, devolve B. Os chamadores HTTP atuais passam `''` quando o campo falta, evitando esse caso; o teste anterior ainda exercita a API sem vínculo (`app/escala/services/propositor.py:685`, `app/chamados/services/fio.py:509`, `app/escala/tests/test_o261_v_fase_na_lista.py:403-406`).

2. **Proposta sem vínculo não recebe validação própria.** Um vínculo textual `'None'` casa com `p.get('vinculo_id') is None`; `None` como argumento desliga o filtro. Porém o produtor real preenche `vinculo_id=ec.pk` em toda proposta antes de retorná-la. Uma proposta real sem vínculo não foi demonstrada pelo código ou anexo (`app/escala/services/propositor.py:539-543`, `:685`) [não verificado].

3. **A recusa continua silenciando por conteúdo, atravessando vínculos.** Recusar A persiste a FK de A, mas a consulta e o filtro de silêncio usam somente `(colaborador, assinatura)`. Contraexemplo: A/B da mesma pessoa, mesmo conteúdo; recusar A também cala B por trinta dias. Isso está explicitamente preservado no comentário do delta, que mantém a chave de L-042; não é omissão acidental do transporte (`app/escala/services/propositor.py:581-583`, `:663-678`, `:693-702`, `:717-720`).

**Os testes não mordem o chamador do fio para este delta.** O novo RED chama `_recalcular` diretamente com `'1'` e `''`; não atravessa template, POST, `aplicar_do_fio` ou `recusar_do_fio`. Remover `vinculo_da_tela=_vinc` de `fio.py` não seria detectado por esse RED (`app/escala/tests/test_o261_v_fase_na_lista.py:408-423`, `app/chamados/services/fio.py:511-514`). O teste de render existente afirma apenas que a assinatura aparece duas vezes, sem verificar os campos de vínculo (`app/chamados/tests/test_proposta_faixa_na_tela.py:85-89`). Os testes antigos dos serviços também chamam aplicar/recusar sem vínculo, utilizando o comportamento opcional (`app/chamados/tests/test_proposta_no_fio.py:118`, `:175`).

## 30/10 19:23 · sessao 7163f16a20d4 · O261-V-FASE · verificador · origem=Code

**Há pendências procedentes: a troca de vínculo preservando a assinatura ainda passa, existem dois caminhos de frota dentro de requests e os testes novos não cobrem integralmente os consumidores.** (`app/escala/services/propositor.py:663–684`; `app/chamados/services/proposta_no_fio.py:133–171`; `app/api/views_mensageria.py:2019,2207`; `app/escala/tests/test_o261_v_fase_na_lista.py:387–409`)

Comparação efetiva dos commits, execução da suíte, RED histórico e ocorrência em produção: **[nao verificado]**. As conclusões abaixo referem-se ao código presente. [nao verificado]

1. **Quem escreve e quem lê**

   `JUIZES.md` distingue vínculo do dia, cobertura da data, dia de trabalho e geometria de turno; também esclarece que é um índice, enquanto o registro vivo está em `core/juizes.py`. O juiz e o escritor da proposta estão declarados no intake. (`app/docs/JUIZES.md:7–12,20–25,66–78`; `app/core/intake.py:49–53`)

   | Valor | Produção/escrita | Consumidores |
   |---|---|---|
   | `dono_do_dia` da lavra | `fase_12x36.dono_do_dia` devolve `'CADASTRO'`; o censo chama esse ponto por item; `lavrar` persiste o snapshot com `update_or_create`. (`app/escala/services/fase_12x36.py:78–100`; `app/ponto/management/commands/censo_fase_12x36.py:250–265`) | `_fase_deslocada` copia para C10; `lista` expõe o dono e conta ocorrências por dono; tela/PDF apresentam a assinatura. (`app/escala/services/cadastro_realidade.py:335–347,647–658,784`; `app/templates/escala/cadastro_x_realidade.html:28`) |
   | Retorno de `dias_na_lista` | Monta `{(colab_id, data): dono ou None}`; `_dono_resolvido` prioriza C9 e devolve `None` se houver conflito no nível escolhido. (`app/escala/services/cadastro_realidade.py:706–742`) | `vigia_fio` consulta o mapa: somente BATIDA/CADASTRO permitem encaminhar o dia para `fio_do_admin`, deixando de contá-lo em `fio_morto`. (`app/chamados/services/vigia_fio.py:255–266,314–324`) |
   | Retorno de `_recalcular` | Recalcula propostas da pessoa e procura a assinatura recebida; devolve `(p, erro)`. (`app/chamados/services/proposta_no_fio.py:92–116`) | `aplicar_do_fio` entrega `p` ao escritor do intake; `recusar_do_fio` entrega `p` a `propositor.recusar`. Ambos recebem a ação pelo POST do fio. (`app/chamados/services/proposta_no_fio.py:133–146,168–171`; `app/chamados/services/fio.py:505–512`) |
   | Retorno de `_proposta_de_escala` | Chama `propostas(colaborador_id=colab_pk)`; no preview escolhe a primeira da pessoa; com assinatura procura a correspondente. (`app/core/views_ia.py:562–583`) | `_preview_escala` monta o texto e guarda colaborador/assinatura na sessão; `_aplicar_escala` recalcula com essa assinatura e chama o escritor. (`app/core/views_ia.py:591,620–622,649–658`) |

   A aplicação usa a porta de correção retroativa e grava a decisão; a recusa grava `DecisaoProposta`. Não encontrei nesses caminhos um segundo escritor de correção de escala. (`app/escala/services/propositor.py:711–714,759–770`)

   Os demais leitores da lavra de fase usam seus itens ou conjuntos de colaboradores: propositor, fila por causa, pauta, API de fase e comando de vigia da família. (`app/escala/services/propositor.py:490–493`; `app/chamados/services/fila_por_causa.py:174`; `app/escala/services/fase_12x36.py:132–139`; `app/api/views_mensageria.py:1671–1672`; `app/ponto/management/commands/vigiar_familia_escala.py:22–33`)

2. **Falhas e pré-requisitos**

   **A — A-residual continua: a assinatura identifica conteúdo, mas não o vínculo alvo.** Ela inclui apenas tipo, template, âncora e início de aplicação. `da_assinatura` rejeita duas correspondências simultâneas, mas aceita uma correspondência sobrevivente de outro vínculo. (`app/escala/services/propositor.py:663–684`)

   **Contraexemplo:** o preview mostrou A/ec1; antes da confirmação A deixa de ser proposta; resta B/ec2 com os mesmos quatro campos. A assinatura continua igual; `_recalcular` devolve B sem erro; aplicar escreve B e recusar registra a recusa em B. O chat guarda somente colaborador/assinatura e também pode aplicar B. (`app/escala/services/propositor.py:663–684`; `app/chamados/services/proposta_no_fio.py:109–116,133–146,168–171`; `app/core/views_ia.py:620–622,649–658`)

   Isso contraria a proteção declarada de exigir a proposta efetivamente lida antes da decisão. O recálculo exigido pela L-042 ocorre, mas a identidade do alvo permanece insuficiente. (`app/chamados/services/proposta_no_fio.py:95–99`; `app/core/views_ia.py:651–656`; `app/docs/LEIS.md:90`)

   **B — Custo de frota no request permanece em duas APIs.** O primeiro caminho chama diretamente `propostas()`; o segundo monta a revisão semanal, que chama a pauta do propositor e termina em `propostas(hoje)` sem filtro. Sem `colaborador_id`, o propositor carrega os colaboradores da frota. (`app/api/views_mensageria.py:2019,2205–2207`; `app/escala/services/fase_12x36.py:174`; `app/escala/services/propositor.py:428–429,640`)

   **Contraexemplo:** um GET autenticado de propostas executa o cálculo da frota antes de limitar a resposta a oito itens; o teto da apresentação não limita o trabalho realizado. Tempo e consultas atuais: **[nao verificado]**. (`app/api/views_mensageria.py:2002,2016–2021`)

   **C — Pré-requisito aberto: ponto único de gravação não resolve o juiz do dono.** O wrapper continua devolvendo CADASTRO, enquanto a L-099 nomeia `e6_oraculo.dono_da_divergencia`; o próprio wrapper registra expressamente a pergunta aberta. Não considero isso uma cura concluída nem uma descoberta nova desta volta. (`CLAUDE.md:311–316`; `app/escala/services/fase_12x36.py:81–89`)

   **Contraexemplo de contrato:** sem flags especiais, contagem par e ausência na lista do motor, o e6 devolve ESTRUTURA; uma C10 isolada recebe CADASTRO e pode encaminhar o dia para fora do alarme. A ocorrência concreta dessa combinação em produção está **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:90–100`; `app/escala/services/fase_12x36.py:89`; `app/chamados/services/vigia_fio.py:266,314–324`)

   **D — Declaração do cron e selo não acompanham a prosa.** A prosa chama o censo de juiz único, mas ele continua cadastrado como `lavra`; `juizes_por_varredura` conta somente papel `juiz`. Assim, o censo reconhecido como juiz fica fora desse contador. O selo verifica a declaração, sem confrontá-la com o julgamento executado. (`app/config/crons.py:600–605,2145–2151,2246–2255`; `app/ponto/management/commands/censo_fase_12x36.py:227`; `app/core/tests/test_contract_tabuleiro.py:15–28`)

   Isso é inconsistência do cadastro/medição de papéis; não fundamento para desfazer a escolha específica do censo como autoridade da fase. (`CLAUDE.md:275–276`; `app/escala/tests/test_o261_v_fase_na_lista.py:4–8`)

   **Não achei uma derivação paralela nova em `dias_na_lista`:** ela seleciona donos já lavrados, priorizando a C9 proveniente do juiz; não reconstrói os insumos do dia. (`app/escala/services/cadastro_realidade.py:706–719`; `app/ponto/services/teto_na_lista.py:72–86`; `app/ponto/management/commands/e6_oraculo.py:146–155`)

3. **Outros chamadores: primeira proposta e frota**

   - **Decisão direta pela primeira sem guarda:** não encontrei outro caso nos caminhos examinados. A CLI usa `_dele[0]` somente depois de rejeitar mais de uma proposta. (`app/escala/management/commands/propostas_de_escala.py:30–38,52–58`)
   - **Apresentação ainda escolhe a primeira:** fio, ficha e preview do chat. Isso não constitui, por si só, outro aplicador direto. (`app/chamados/services/proposta_no_fio.py:66`; `app/colaboradores/services/ficha.py:176–189`; `app/core/views_ia.py:580–582`)
   - **Requests de frota encontrados:** `/mensageria/propostas-escala/` e `/mensageria/revisao-semanal/`, pelos caminhos descritos acima. (`app/api/urls.py:55,58`; `app/api/views_mensageria.py:2019,2207`; `app/escala/services/propositor.py:640`)
   - **Recortados:** painel pelos IDs da página com `so_existencia=True`; ficha por uma pessoa; fio e chat por uma pessoa. (`app/chamados/services/painel.py:518–522`; `app/colaboradores/services/ficha.py:176`; `app/chamados/services/proposta_no_fio.py:63,102`; `app/core/views_ia.py:575`)

4. **Os testes mordem o chamador? Parcialmente.** [nao verificado]

   | Cura retirada | O que o teste existente detecta | Limite | [nao verificado]
   |---|---|---|
   | Chamada ao ponto único do dono | **Sim:** substitui o helper por sentinela, executa o censo e exige a sentinela persistida. Voltar ao literal CADASTRO quebra essa asserção. (`app/escala/tests/test_o261_v_fase_na_lista.py:337–345`) | Não certifica qual juiz deve responder o dono. (`app/escala/tests/test_o261_v_fase_na_lista.py:338–339`) |
   | Independência da ordem | **Sim, em `dias_na_lista`:** testa C9/BATIDA e C10/CADASTRO nas duas ordens. (`app/escala/tests/test_o261_v_fase_na_lista.py:319–331`) | Não percorre o vigia com conflito. BATIDA e CADASTRO levam ao mesmo ramo; falta C9/ESTRUTURA + C10/CADASTRO permanecendo no alarme nas duas ordens. (`app/chamados/services/vigia_fio.py:266`; `app/chamados/tests/test_o261_vigia_fio.py:547–570`) |
   | `dias=_meus` no horário | **Sim:** chama `propostas()` e exige o argumento `dias`. (`app/escala/tests/test_o261_v_fase_na_lista.py:373–381`) | Só confere presença e mínimo; um conjunto errado com o mesmo mínimo sobrevive a esse teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:379–381`) |
   | Seleção por assinatura no fio | **Sim, no auxiliar:** voltar à primeira quebra o teste com duas âncoras diferentes. (`app/escala/tests/test_o261_v_fase_na_lista.py:392–398`) | Não chama aplicar/recusar nem cobre A desaparecer e B sobreviver com assinatura igual. (`app/escala/tests/test_o261_v_fase_na_lista.py:394–398`) |
   | Recorte do chat | **Sim, no auxiliar:** retirar `colaborador_id` quebra a inspeção do argumento. (`app/escala/tests/test_o261_v_fase_na_lista.py:404–409`) | O retorno simulado está vazio; não exercita confirmação, vínculo escrito ou as outras APIs de frota. (`app/escala/tests/test_o261_v_fase_na_lista.py:407–409`) |

   Os testes anteriores percorrem aplicar/recusar com uma proposta e não conferem o vínculo entregue ao escritor no cenário de troca preservando assinatura. O chat também usa uma proposta e testa mudança de template ou desaparecimento total. Portanto, não fecham o residual A. (`app/chamados/tests/test_proposta_no_fio.py:112–122,170–178`; `app/core/tests/test_ia_escala_com_sim.py:57–59,86–118`)

## 30/10 19:22 · sessao 6f74f43b9a49 · - · - · origem=Ronald (tmux)

Os testes novos mordem algumas curas, mas não cobrem integralmente os consumidores: [nao verificado]

| Cura | O que o teste verifica | Lacuna | [nao verificado]
|---|---|---|
| Dono da lavra passa pelo ponto único | Executa `_lavrar()`, troca o helper por sentinela e exige essa sentinela em todos os itens gravados. Trocar a chamada por `'CADASTRO'` derruba o teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:341–345`; chamada real em `app/ponto/management/commands/censo_fase_12x36.py:262`) | Não decide a pergunta aberta de qual juiz responde o dono. (`app/escala/tests/test_o261_v_fase_na_lista.py:338–339`) |
| Ordem das assinaturas | Chama `dias_na_lista` com C9/BATIDA e C10/CADASTRO nas duas ordens. (`app/escala/tests/test_o261_v_fase_na_lista.py:319–324`) | Não leva esse conflito ao `vigia_fio`. O teste do consumidor fornece somente uma C9 por dia. (`app/chamados/tests/test_o261_vigia_fio.py:547–551`, `553–570`) |
| Horário recebe dias do vínculo | Executa `propostas()` e exige que `_horario_mediano` receba `dias`, com mínimo 20/10. Retirar `dias=_meus` derruba esse teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:373–381`; `app/escala/services/propositor.py:529`) | A fixture possui um vínculo; conjunto errado que preserve o mínimo passa pelas asserções desse teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:365–381`) |
| Fio recalcula pela assinatura | Duas propostas com âncoras diferentes; exige que `_recalcular` retorne o segundo vínculo. Voltar ao primeiro derruba esse teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:392–398`) | Não chama `aplicar_do_fio` nem `recusar_do_fio`, e não cobre A desaparecer e B sobreviver com a mesma assinatura. (`app/escala/tests/test_o261_v_fase_na_lista.py:394–398`) |
| Chat estreita pela pessoa | Chama `_proposta_de_escala(7)` e exige `colaborador_id=7` no propositor. Retirar o keyword derruba esse teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:404–409`) | Usa retorno sem propostas; não exercita seleção por assinatura nem aceite no request. (`app/escala/tests/test_o261_v_fase_na_lista.py:407–409`) |

Os testes antigos do fio exercitam aplicar/recusar com uma única proposta, sem conferir que o objeto enviado ao escritor é o segundo vínculo escolhido pela assinatura. (`app/chamados/tests/test_proposta_no_fio.py:112–122`, `170–178`)

Os testes antigos do chat passam pelo request, mas a fixture retorna uma proposta; o aceite confere colaborador, usuário e chamada. Os casos negativos mudam `template_id` ou esvaziam a lista, portanto não cobrem assinatura igual em vínculo diferente. (`app/core/tests/test_ia_escala_com_sim.py:57–59`, `86–118`)

Há uma inconsistência declarativa: a prosa chama o censo de juiz único, mas seu papel continua `lavra`; o corpo ainda contém “VIGIA, NUNCA JUIZ”. A lei específica citada no teste escolheu o censo como juiz; isso impede concluir apenas pela prosa que o predicado autorizado viola 4a. (`app/config/crons.py:600–605`, `2145–2151`; `app/ponto/management/commands/censo_fase_12x36.py:241–249`; `app/escala/tests/test_o261_v_fase_na_lista.py:4–8`)

O selo do tabuleiro verifica enumeração do papel e igualdade com a lista declarada; não verifica semanticamente se uma `lavra` julga. (`app/core/tests/test_contract_tabuleiro.py:15–28`)

Execução real dos testes, RED e mutações: `[nao verificado]`. [nao verificado]

## 30/10 19:22 · sessao 2599c17d63fd · - · - · origem=Ronald (tmux)

Achados para consolidação (só leitura; sem banco/log; ensaio executou corpos reais via AST em memória e dependências simuladas).

1. **A-residual permanece no fio e chat.** A assinatura ignora `vinculo_id` (`app/escala/services/propositor.py:663`); `da_assinatura` rejeita apenas quando encontra duas propostas simultâneas (`:677`, `:680`, `:684`). Contraexemplo: preview A/ec1; A some; sobra B/ec2 com mesmo conteúdo. `_recalcular` devolve B (`app/chamados/services/proposta_no_fio.py:109`), `aplicar_do_fio` escreve B (`:133`, `:146`) e `recusar_do_fio` recusa B (`:168`, `:171`). No chat, sessão guarda somente colaborador/assinatura (`app/core/views_ia.py:620`); recalcula e aplica B (`:649`, `:658`). O ensaio em memória confirmou ambos escritores recebendo ec2; ocorrência em produção `[nao verificado]`. A falha contradiz a proteção declarada pelo próprio gate contra aplicar sob texto de outra proposta (`app/chamados/services/proposta_no_fio.py:95`) e o contrato de ambiguidade (`app/escala/services/propositor.py:671`). A triagem anexada já descreve este residual, mas seu lastro externo é `[nao verificado]`.

2. **Frota ainda dentro de dois requests.**
   - Direto: rota `app/api/urls.py:55` → `api_mensageria_propostas_escala` → `propositor.propostas()` sem filtro (`app/api/views_mensageria.py:2005`, `:2019`).
   - Indireto: rota `app/api/urls.py:58` → revisão semanal (`app/api/views_mensageria.py:2189`, `:2205`, `:2207`) → `revisao_semanal_supervisao` (`app/escala/services/fase_12x36.py:174`) → `linhas_para_a_pauta` → `propostas(hoje)` (`app/escala/services/propositor.py:640`). Ausência de filtro escolhe toda frota (`:428`). Há caller mensageria (`mensageria/nucleo/core_client.py:287`, `:289`; `mensageria/nucleo/ferramentas.py:934`). Custo/tempo atual `[nao verificado]`; o contrato do propositor declara estreitamento para cliques (`app/escala/services/propositor.py:396`).

3. **Censo de decisões e leitores.**
   - `_recalcular` produz `(p, erro)` (`app/chamados/services/proposta_no_fio.py:92`, `:116`); consumidores são `aplicar_do_fio` (`:133`) e `recusar_do_fio` (`:168`), chamados externamente por ações do fio (`app/chamados/services/fio.py:505`, `:509`, `:511`).
   - `_proposta_de_escala` produz `(p, assinatura)` (`app/core/views_ia.py:562`, `:579`, `:582`); preview consome (`:591`) e guarda assinatura (`:620`); aceite consome (`:649`) e escreve pelo escritor de intake (`:658`). Despacho preview: `_avancar_intake` (`:461`, `:462`); despacho aceite: `ia_perguntar` (`:869`, `:872`) → `_aplicar_proposta` (`:147`, `:148`). Escritor/juiz/gate estão registrados em `app/core/intake.py:51`, `:52`, `:53`.
   - Preview do fio ainda seleciona primeiro (`app/chamados/services/proposta_no_fio.py:66`), lido nos dois renders (`app/chamados/services/fio.py:462`; `app/chamados/views.py:932`). Ficha também seleciona primeiro (`app/colaboradores/services/ficha.py:178`), mas só sugere ação (`:189`). Chat preview seleciona primeiro do colaborador (`app/core/views_ia.py:580`). Esses caminhos não são decisões diretas.
   - CLI escolhe `_dele[0]` somente após rejeitar `len>1` (`app/escala/management/commands/propostas_de_escala.py:31`, `:37`, `:38`), então não achei outro aplicador/recusador produtivo que selecione primeiro sem guarda no censo dos imports/calls encontrados. Resolução dinâmica fora do censo `[nao verificado]`.

4. **Censo produtivo de `propostas`:** fio por uma pessoa (`app/chamados/services/proposta_no_fio.py:63`, `:102`); chat por uma (`app/core/views_ia.py:575`); painel pelos IDs da página com `so_existencia` (`app/chamados/services/painel.py:518`, `:521`); ficha por uma (`app/colaboradores/services/ficha.py:176`); API frota e pauta frota acima; CLIs frota fora request (`app/escala/management/commands/propostas_de_escala.py:26`; `app/escala/management/commands/ritmo_discordante.py:18`).

5. **Os testes novos mordem helpers, parcialmente o chamador.** [nao verificado]
   - Teste do fio chamado “proposta aplicada” chama apenas `_recalcular`, com duas assinaturas diferentes (`app/escala/tests/test_o261_v_fase_na_lista.py:387`, `:394`, `:396`, `:398`); não verifica o objeto entregue a `aplicar_do_fio`/`recusar_do_fio` e não contempla A-some/B-sobra. Reverter escolha por assinatura para primeira quebra esse teste; bypass nos chamadores pode sobreviver a ele.
   - Teste de duas assinaturas iguais testa somente `da_assinatura` (`:263`, `:272`), não seus consumidores. [nao verificado]
   - Teste de estreitamento chama `_proposta_de_escala` e inspeciona argumento de `propostas` (`:404`, `:408`, `:409`); retirar `colaborador_id` quebra o teste, mas não mede custo nem percorre os dois requests de frota. [nao verificado]
   - Testes antigos do fio exercitam portas com proposta única (`app/chamados/tests/test_proposta_no_fio.py:112`, `:118`, `:170`, `:175`); os de chat usam proposta única (`app/core/tests/test_ia_escala_com_sim.py:57`, `:59`, `:86`). Não cobrem troca de vínculo preservando assinatura.

Referência das leis: `L-041` (`app/docs/LEIS.md:89`), `L-042` (`:90`), `L-099` (`:122`); separação consumidor/lâmpada/cron (`CLAUDE.md:273`, `:275`, `:276`). `JUIZES.md` distingue geometria de turno (`app/docs/JUIZES.md:20`), dia de trabalho (`:66`), vínculo do dia (`:75`) e cobertura da data (`:77`), e explicita que é lookup, com registro vivo em `core/juizes.py` (`:7`, `:10`).

## 30/10 19:22 · sessao 4ab3ce862214 · - · - · origem=Ronald (tmux)

Sem falha funcional nova demonstrada na lavra/lista; o juiz do dono permanece pré-requisito aberto, explicitado no próprio código. `app/escala/services/fase_12x36.py:81-89`

- **Escritor e leitores:** censo chama `dono_do_dia` por item e entrega a `lavrar`; esta persiste um snapshot diário. C10 copia o dono, a lista o exibe e `dias_na_lista` o agrega; único consumidor produtivo encontrado deste retorno é `vigia_fio`. `app/ponto/management/commands/censo_fase_12x36.py:250-265`; `app/escala/services/fase_12x36.py:92-101`; `app/escala/services/cadastro_realidade.py:335-347`, `:647-658`, `:722-742`; `app/chamados/services/vigia_fio.py:255-266`, `:314-323`.
- **Prioridade C9:** não recalcula insumos; seleciona a lavra proveniente do juiz existente. C9 recebe `InsumosDoDono.dono`, que chama `dono_da_divergencia`; prioridade não depende da ordem. UI continua exibindo donos por assinatura, não um dono agregado. `app/escala/services/cadastro_realidade.py:706-719`; `app/ponto/services/teto_na_lista.py:72-86`; `app/ponto/management/commands/e6_oraculo.py:146-155`; `app/templates/escala/cadastro_x_realidade.html:28`.
- **Pré-requisito:** sem flags e sem cadastro divergente, contagem par — inclusive zero — devolve ESTRUTURA no e6; C10 isolada devolve CADASTRO e pode retirar o dia do alarme. É a própria pergunta aberta; ocorrência concreta em produção `[nao verificado]`. `app/ponto/management/commands/e6_oraculo.py:90-100`; `app/escala/services/fase_12x36.py:81-89`; `app/chamados/services/vigia_fio.py:266`, `:314-323`. Lei do juiz único: `CLAUDE.md:311-316`; lookup não substitui registro: `app/docs/JUIZES.md:7-12`.
- **Testes:** o teste do dono morde o chamador censo: substituir a chamada pelo literal CADASTRO quebra a exigência do sentinel persistido. `app/escala/tests/test_o261_v_fase_na_lista.py:337-345`.
- **Limite de cobertura:** testes de ordem chamam `dias_na_lista`, usando BATIDA/CADASTRO; ambos entram no mesmo ramo do vigia. Os testes existentes do vigia oferecem somente uma assinatura C9 por dia. Falta caso integrado de célula morta com **C9 ESTRUTURA + C10 CADASTRO**, nas duas ordens, permanecendo em `fio_morto`. `app/escala/tests/test_o261_v_fase_na_lista.py:319-331`; `app/chamados/tests/test_o261_vigia_fio.py:541-571`; `app/chamados/services/vigia_fio.py:266`.

Execução isolada das funções puras por AST, sem Django/banco, confirmou ESTRUTURA, BATIDA e None preservados independentemente da ordem; não constitui execução da suíte. `[nao verificado]` [nao verificado]

## 30/10 19:18 · sessao 143cd6819825 · O261-V-FASE · sonda · origem=Code

- Vínculos ativos: `12x36 = 337`; `24x48` não consta na saída. [nao verificado]
- Detector `fora_de_fase`: `n=10`, `universo=337`. [nao verificado]
- Achados por ciclo: `{'12x36': 10}`. [nao verificado]
- Achados `24x48`: **0**; `colab_ids []`. [nao verificado]
- Todos os `colab_ids`: `[300, 366, 451, 696, 788, 978, 979, 982, 986, 998]`. [nao verificado]
- A saída fornecida não exibe mensagem de erro. [nao verificado]

## 30/10 19:15 · sessao 507013464189 · E1-BATERIA · red · origem=Code

Criado somente [test_e1_lote2_status_do_dia.py](/home/ronald/codex_ws/red_e54eae6ed8c4_20261030_190743_37155/app/ponto/tests/test_e1_lote2_status_do_dia.py): **20 casos, sendo 15 executáveis e 5 skips de lei**. Sintaxe validada em memória, sem criar bytecode. Suite Django não executada; RED ainda não evidenciado. [nao verificado]

| ID | Status esperado | Retenção esperada | Regra citada |
|---|---|---|---|
| S01 | `ok` | Não retém | CLT arts. 58 e 74 §2 |
| S02 | SKIP — pendente | Pendente | L-039, L-110 |
| S03 | `previsto` | Não retém | ADR D4; CLAUDE §6 |
| S04 | `aberto` | Não retém | ADR D4; CLAUDE §6 |
| S05 | `folga` | Não retém | CLT art. 67; L-024 |
| S06 | `folga_trabalhada` | Não retém | CLT art. 71 §1; L-024 |
| S07 | `ausencia` | Não retém | Lei 605/49 art. 6 §§1–2; L-089 |
| S08 | `falta` | Não retém | L-039; Lei 605/49 art. 6 |
| S09 | `folga` | Não retém | Lei 605/49 art. 8 |
| S10 | `folga_trabalhada` | Não retém | Lei 605/49 art. 9 |
| S11 | `irregular` | Não retém | L-033, L-032; Portaria 671 |
| S12 | SKIP — pendente | Pendente | ADR D4; L-033 |
| S13 | `irregular`, acoplamento em SKIP | Pendente | L-084; CLT art. 58 §1 |
| S14 | SKIP — premissa divergente | Pendente | L-084 |
| S15 | `ok` | Não retém | CLT art. 59-A; ADR D3 |
| S16 | `folga` | Não retém | CLT art. 59-A; ADR D3 |
| S17 | `folga` | Não retém | CLT art. 59-A; L-024 |
| S18 | SKIP — mapeamento pendente | Não retém o abono; caso suspenso | ADR: decisão humana prevalece; L-039 |
| S19 | `ok` | Não retém | CLT art. 62; ADR D3 |
| S20 | `sem_dados` | Não retém | L-112; ADR D3; CLT art. 4 |

Perguntas registradas nos skips:

- **S02:** sem decisão humana, usar `irregular`, `escala_suspeita` ou autorizar exceção à L-039 para `falta`? Qual motivo de retenção?
- **S12:** após encerrar, entrada única continua `aberto` ou vira `irregular`? Retém por qual motivo?
- **S13:** atraso registrado gera `irregular` sem retenção, com retenção ou `ok` com desconto apenas na rubrica?
- **S14:** as pontas diferem **exatamente 180 minutos**. Manter os horários, corrigir o cenário para superar 180 ou alterar expressamente o limite?
- **S18:** admin resolve como `justificado` com abono: prevalece `ausencia`, `folga` ou uma exceção explícita para `ok`?

## 30/10 19:12 · sessao 1c0187623ab2 · MEDIDA-PROGRAMA · verificador · origem=Code

**O desenho permite chegar a zero sem eliminar todas as derivações do dia:** já há um classificador de produção que escapa do vocabulário proposto, e VALOR continua restrito aos sete arquivos previamente enumerados. (`app/relatorios/furos.py:55`, `app/relatorios/furos.py:69`, `app/ponto/tests/test_s3_placar_exercicio.py:34`, `app/ponto/tests/test_s3_placar_exercicio.py:305`)

**1. Onde a definição verificável falha**

- **Falso negativo concreto: `relatorios/furos.py::classificar_dia`.** Calcula o estado pela presença e quantidade de marcos `missing`, retornando `sem_marcos`, `furo`, `parcial` ou `ok`. Só `ok` pertence ao vocabulário contratado; portanto não alcança duas palavras. A resposta alimenta os dias apresentados e a contagem de furos. O censo anterior já o identifica como “classificar_dia próprio”, classe C. (`app/relatorios/furos.py:63`, `app/relatorios/furos.py:69`, `app/relatorios/furos.py:70`, `app/relatorios/furos.py:72`, `app/relatorios/furos.py:74`, `app/relatorios/furos.py:75`, `app/relatorios/furos.py:160`, `app/relatorios/furos.py:164`, `app/docs/CENSO_NARNIA.md:66`)

- **A indireção não fica necessariamente coberta pela tabela ou função chamada.** `estado_do_dia` escolhe entre `_VER_ESTADO` e o classificador acima, sem produzir os literais procurados. A tabela fica no módulo; o classificador também escapa. Assim, neste caso existente, nenhum elo garante a detecção prometida. (`app/relatorios/furos.py:24`, `app/relatorios/furos.py:45`, `app/relatorios/furos.py:46`, `app/relatorios/furos.py:47`, `app/relatorios/furos.py:52`)

- **Há outros vocabulários paralelos já existentes.** O painel operacional escolhe `geofence/normal/turno_aberto/encerrado/ausente/folga`, publica o status e monta contadores; só `folga` entra no vocabulário contratado. O registro já reconhece esses contadores como derivação própria. (`app/core/services/painel_op.py:155`, `app/core/services/painel_op.py:160`, `app/core/services/painel_op.py:166`, `app/core/services/painel_op.py:169`, `app/core/services/painel_op.py:172`, `app/core/services/painel_op.py:175`, `app/core/services/painel_op.py:190`, `app/core/services/painel_op.py:200`, `app/core/juizes.py:705`)

- **A catraca por função não detecta necessariamente um sítio novo dentro de função declarada.** `api_espelho_v2` já contém decisões separadas para certidão da grade, dia com batidas e dia sem batidas. Todas viram uma identidade `arquivo::funcao`; acrescentar outra decisão ali pode preservar o mesmo conjunto de identidades. (`app/api/views.py:1514`, `app/api/views.py:1570`, `app/api/views.py:1644`, `app/api/views.py:1650`, `app/api/views.py:1653`)

- **Falso positivo do predicado bruto:** `campos_da_frase` produz `folga/falta` sob escolha, mas preenche o **tipo da intenção**, não o status do dia. Deve ser exclusão por homônimo. Sem o `NAO_E_LEITOR` concreto, afirmar que sobreviveria ao filtro final é [nao verificado]. (`app/core/intake.py:219`, `app/core/intake.py:228`, `app/core/intake.py:232`)

- **VALOR mede uma lista fechada, não descobre leitores novos.** Tanto o teste principal quanto o teste “MONTADOR é UM SÓ” percorrem `LEITORES`. Um arquivo novo fora dessa lista não entra automaticamente. (`app/ponto/tests/test_s3_placar_exercicio.py:305`, `app/ponto/tests/test_s3_placar_exercicio.py:346`, `app/ponto/tests/test_s3_placar_exercicio.py:350`)

- **O detector reutilizado tem uma cegueira sintática real:** não atravessa `BoolOp` no caminho de `getattr`; a forma `(getattr(p, 'minutos_extra_50', 0) or 0) / 60` existe na cópia. Isso demonstra uma limitação do detector, embora esteja no montador autorizado e não prove leitor indevido. (`app/ponto/tests/test_s3_placar_exercicio.py:205`, `app/ponto/services/espelho.py:113`, `app/ponto/services/espelho.py:114`, `app/ponto/tests/test_s3_placar_exercicio.py:58`)

**2. O que já responde à mesma pergunta**

Para **derivação própria de dinheiro**, já existem o detector, o universo e a cobrança de zero da S3. O contrato acerta ao propor transferi-los e manter uma implementação compartilhada; criar outro detector duplicaria essa pergunta. (`app/ponto/tests/test_s3_placar_exercicio.py:14`, `app/ponto/tests/test_s3_placar_exercicio.py:183`, `app/ponto/tests/test_s3_placar_exercicio.py:303`)

Para **sítios de tela que respondem por conta própria**, já existem `PENDENTES['tela']`, `fora_de_autoridade('tela')` e sua publicação como `leitores_narnia`. Esse inventário deve ser reutilizado como registro da dívida; ele não equivale ao novo censo AST, pois conta declarações e verifica impressões textuais. (`app/core/juizes.py:656`, `app/core/juizes.py:690`, `app/core/juizes.py:1415`, `bin/placar_code.sh:484`, `bin/placar_code.sh:490`, `app/core/tests/test_contract_juiz_tela.py:34`)

Há ainda uma imprecisão no cruzamento proposto: `_TT6` pergunta **“o colaborador entra na folha (e por que não)?”**, com autoridade `classificar_export`; “status e contagem” é o motivo de uma entrada específica do calendário. Não é o significado geral de `_TT6`. (`app/core/juizes.py:665`, `app/core/juizes.py:678`, `app/core/juizes.py:809`)

**3. Quem define e quem consome os nomes**

| Nome | Definição/escrita atual | Leituras atuais |
|---|---|---|
| `ROTULO_STATUS` | Dicionário em `app/colaboradores/services/calendario.py:44`. | Calendário: `app/colaboradores/services/calendario.py:901`; minha escala: `app/escala/views.py:36`, `app/escala/views.py:55`; selos: `app/ponto/tests/test_contract_espelho_sem_emoji.py:220`, `app/colaboradores/tests/test_calendario_um_juiz.py:70`. |
| `deriva_dinheiro` | Função em `app/ponto/tests/test_s3_placar_exercicio.py:183`. | Critério b: `app/ponto/tests/test_s3_placar_exercicio.py:306`; casos sintéticos: `app/ponto/tests/test_s3_placar_exercicio.py:312`; montador: `app/ponto/tests/test_s3_placar_exercicio.py:346`, `app/ponto/tests/test_s3_placar_exercicio.py:350`. |
| `LEITORES` | Tupla em `app/ponto/tests/test_s3_placar_exercicio.py:34`. | Critério a: `app/ponto/tests/test_s3_placar_exercicio.py:247`; critério b: `app/ponto/tests/test_s3_placar_exercicio.py:305`; segundo montador: `app/ponto/tests/test_s3_placar_exercicio.py:350`. |
| `DINHEIRO` / `NAO_E_MOTOR` | `app/ponto/tests/test_s3_placar_exercicio.py:163` e `app/ponto/tests/test_s3_placar_exercicio.py:171`. | Via `getattr`: `app/ponto/tests/test_s3_placar_exercicio.py:203`; via atributo, somente `DINHEIRO`: `app/ponto/tests/test_s3_placar_exercicio.py:213`. |
| `gerar_estado.py` / ESTADO | `gerar` incorpora os placares e escreve o destino em `bin/gerar_estado.py:346`, `bin/gerar_estado.py:349`. | Entrada CLI: `bin/gerar_estado.py:355`; chamada pelo relato: `bin/relato.sh:38`; ESTADO entra na publicação: `bin/relato.sh:102`, `bin/relato.sh:115`. |

Mover `LEITORES` exige preservar também seus consumidores do critério **a** e do montador; a dependência não está restrita ao critério **b**. (`app/ponto/tests/test_s3_placar_exercicio.py:247`, `app/ponto/tests/test_s3_placar_exercicio.py:350`)

**4. Carga no host e falhas no ESTADO**

- **Importar os módulos atuais diretamente exige Django.** O selo importa `SimpleTestCase`; o calendário importa Django e modelos. A extração AST do vocabulário e a transferência completa do detector evitam essas dependências; o funcionamento da implementação futura é [nao verificado]. (`app/ponto/tests/test_s3_placar_exercicio.py:30`, `app/colaboradores/services/calendario.py:22`, `app/colaboradores/services/calendario.py:28`)

- **Carregar por `spec_from_file_location` não resolve imports do projeto.** O gerador acrescenta `raiz/bin` ao caminho e carrega o placar por arquivo. O próprio placar registra o precedente de `ModuleNotFoundError` ao importar `core` nesse contexto. (`bin/gerar_estado.py:75`, `bin/gerar_estado.py:77`, `bin/gerar_estado.py:198`, `app/core/placar_estrutural.py:457`, `app/core/placar_estrutural.py:462`)

- **Copiar o molde não garante “ESTADO nunca cai”.** `_placar_estrutural` protege carga e chamadas, mas acessa campos obrigatórios durante a formatação fora do `try`. Outros leitores também são importados e chamados sem proteção. O novo bloco precisa abranger importação, leitura e composição; se a geração falhar, o relato aborta o ciclo. (`bin/gerar_estado.py:74`, `bin/gerar_estado.py:86`, `bin/gerar_estado.py:101`, `bin/gerar_estado.py:215`, `bin/gerar_estado.py:260`, `bin/relato.sh:38`)

- **A publicação existente depende de banco e rede.** Antes de gerar o ESTADO, `relato.sh` exige a extração de nomes, que consulta `Colaborador.objects` via Django/container; depois publica com `git push`. “Sem banco/rede” pode descrever o censo isolado, não essa cadeia. (`bin/relato.sh:32`, `bin/relato.sh:38`, `bin/relato_nomes.sh:11`, `bin/relato_nomes.sh:18`, `bin/relato_nomes.sh:22`, `bin/relato.sh:126`)

O timer declara cinco minutos e o serviço aponta a árvore operacional; timer ativo, execução nessa árvore e duração inferior a dois segundos são [nao verificado]. (`bin/hasner-relato.timer:6`, `bin/hasner-relato.service:14`, `bin/hasner-relato.service:15`)

## 30/10 19:10 · sessao a64a4e6e6de1 · - · - · origem=Ronald (tmux)

Achados principais de STATUS:

- **Falso negativo de status do dia já existente:** `relatorios/furos.py::classificar_dia` calcula `sem_marcos/furo/parcial/ok` por quantidade de `missing`; apenas `ok` pertence ao vocabulário contratado, portanto não atinge duas palavras e escapa. A função é consumida por `estado_do_dia`, cuja resposta chega aos dias e à contagem de furos. Evidência: `app/relatorios/furos.py:55`, `:63`, `:69`, `:70`, `:72`, `:74`, `:75`, `:45`, `:160`, `:164`. O próprio censo anterior chama esse trecho de “classificar_dia próprio”, classe C: `app/docs/CENSO_NARNIA.md:66`.

- **A promessa “a tabela/função chamada é que tem de cair” não fecha:** `_VER_ESTADO` é tabela de módulo, expressamente fora do contador; `estado_do_dia` escolhe entre a tabela e `classificar_dia` sem produzir os literais buscados; e `classificar_dia` escapa pelo vocabulário acima. Evidência: `app/relatorios/furos.py:24`, `:45`, `:46`, `:47`, `:52`, `:55`.

- **Outros vocabulários paralelos já presentes:** `montar_painel_operacional` produz `geofence/normal/turno_aberto/encerrado/ausente/folga`, publica o status e faz seis contadores; somente `folga` pertence ao vocabulário do contrato, portanto escapa. Evidência: `app/core/services/painel_op.py:155`, `:160`, `:166`, `:169`, `:172`, `:175`, `:190`, `:200`. O registro já reconhece os seis contadores como classe C: `app/core/juizes.py:705`. `painel_tv` produz `fora/normal/geofence/aberto`, novamente com somente uma palavra reconhecida: `app/core/views.py:807`, `:816`, `:818`, `:828`, `:835`.

- **Falso positivo bruto, resolvível pela exclusão prometida:** `campos_da_frase` produz `folga/falta` com escolha, mas está preenchendo o tipo de uma intenção de escrita, não o status do dia. Deve entrar como homônimo no `NAO_E_LEITOR`; sem a lista concreta não se pode afirmar que sobreviverá ao filtro final. Evidência: `app/core/intake.py:219`, `:228`, `:232`, `:236`.

- **Derivações reais que a regra encontra:** `_classificar`, agregação `celstat_map` de `contexto_calendario`, `minha_escala`, `api_espelho_v2` e `_status_dia`; não encontrei motivo para chamar esses achados de falso positivo. Evidência: `app/colaboradores/services/calendario.py:125`, `:136`, `:509`, `:511`; `app/escala/views.py:49`, `:55`; `app/api/views.py:1514`, `:1570`, `:1644`, `:1650`; `app/ponto/services/espelho.py:152`, `:158`, `:171`.

- **Granularidade:** `api_espelho_v2` contém derivação de certidão da grade, status de dias com batidas e status de dias sem batidas, mas contará uma função. Assim, uma nova derivação dentro dessa função já declarada pode preservar o mesmo conjunto de identidades. Evidência: `app/api/views.py:1512`, `:1514`, `:1570`, `:1636`, `:1644`, `:1650`, `:1653`, `:1656`.

- **PDF:** a palavra é lida em `app/relatorios/pdf_espelho.py:919`; os dias vêm do espelho em `:592`, `:598`; o compositor comum é reaplicado em `:632`. Isso não prova ausência de toda decisão de estado no PDF: há seleção local de datas de turno aberto em `:613` e supressão da etiqueta `turno aberto` por orfandade em `:956`, `:960`, `:966`. São formas que escapam do contador de literais, mas não justificam afirmar um novo status codificado sem definir melhor a pergunta.

- **Reuso:** o contador existente `fora_de_autoridade('tela')` mede os sítios declarados que ainda respondem por conta própria, e seu nome publicado é `leitores_narnia`: `app/core/juizes.py:655`, `:658`, `:1415`, `:1417`. Não é medição AST dos literais. O tripwire AST específico do PDF resolve aliases, congela dívida e cobra crescimento/entrada podre, mas mede chamadas de cálculo, outra pergunta: `app/relatorios/tests/test_pdf_nao_calcula.py:33`, `:47`, `:62`, `:95`, `:107`.

A execução ad hoc estrita do critério encontrou oito candidatos brutos; nenhum teste `test*.py` fora de `tests/` satisfaz hoje o critério completo [não verificado por arquivo:linha — resultado de execução somente leitura]. [nao verificado]

## 30/10 19:09 · sessao a9a0edaf0a67 · - · - · origem=Ronald (tmux)

Achados de host/ESTADO, somente leitura:

- **A publicação não é independente de banco/Django/rede.** `relato.sh` exige a lista de nomes antes de gerar o ESTADO e sai em erro se ela falhar (`bin/relato.sh:32`, `bin/relato.sh:38`). O extrator consulta `Colaborador.objects` pelo `manage.py` no container e recusa ausência de resultado (`bin/relato_nomes.sh:10`, `bin/relato_nomes.sh:18`, `bin/relato_nomes.sh:22`). Depois ainda publica via `git push` (`bin/relato.sh:126`). A independência prometida só pode ser do **censo isolado**, não dessa cadeia.

- **`spec_from_file_location` não torna dependências importáveis nem remove Django.** O gerador só acrescenta `raiz/bin` ao `sys.path` (`bin/gerar_estado.py:198`) e carrega o módulo puro por caminho (`bin/gerar_estado.py:75`). Importar diretamente o selo S3 puxa Django (`app/ponto/tests/test_s3_placar_exercicio.py:30`); importar calendário puxa Django e modelos (`app/colaboradores/services/calendario.py:22`, `app/colaboradores/services/calendario.py:28`). A extração AST e transferência das funções puras resolve esse obstáculo se forem completas; a implementação futura é [não verificado]. O próprio placar registra o precedente de `ModuleNotFoundError` ao importar `core` nesse carregamento (`app/core/placar_estrutural.py:457`).

- **O molde `_placar_estrutural` não garante sozinho “ESTADO nunca cai”.** A carga e as chamadas ficam no `try` (`bin/gerar_estado.py:74`), mas a renderização acessa campos obrigatórios fora dele (`bin/gerar_estado.py:101`, `bin/gerar_estado.py:115`). Leitores `ler/linha` do corpo de `gerar` também são importados e chamados sem proteção (`bin/gerar_estado.py:215`, `bin/gerar_estado.py:260`, `bin/gerar_estado.py:265`). O novo bloco precisa proteger **importação, leitura e composição**, não apenas a chamada a `censo`; a implementação futura é [não verificado]. Falha do gerador aborta o ciclo (`bin/relato.sh:38`).

- **Fiagem é mais que criar o leitor:** o ESTADO usa uma string com `%s` e uma tupla posicional (`bin/gerar_estado.py:278`, `bin/gerar_estado.py:332`); incluir seção sem atualizar ambos quebra a geração. O arquivo final é gravado por `gerar` (`bin/gerar_estado.py:349`), chamado pelo CLI (`bin/gerar_estado.py:355`) e por `relato.sh` (`bin/relato.sh:38`). Hoje os placares são incorporados em `bin/gerar_estado.py:346`; a integração proposta é [não verificado].

- **Cadência declarada versus execução:** o arquivo de timer programa cinco minutos (`bin/hasner-relato.timer:6`), e o serviço aponta `/home/ronald/saas-hasner/bin/relato.sh` (`bin/hasner-relato.service:15`). Timer instalado/ativo, árvore efetivamente lida, duração inferior a dois segundos e quantidade de arquivos no host são [não verificado].

Precisão adicional dos nomes do contrato:

- `_TT6` não se chama “status/contagem”; pergunta **“o colaborador entra na folha (e por que não)?”** (`app/core/juizes.py:665`) e aponta `folha/export.py::classificar_export` (`app/core/juizes.py:678`).
- `_TT3` pergunta o julgamento do dia (`app/core/juizes.py:662`) e aponta `classificar_falta` (`app/core/juizes.py:675`).
- O contador existente `fora_de_autoridade('tela')` é `len(PENDENTES)` (`app/core/juizes.py:1415`), cuja família cobre leitores de número/estado em sentido mais amplo (`app/core/juizes.py:656`). Seu selo verifica impressão textual presente e tamanho da lista (`app/core/tests/test_contract_juiz_tela.py:34`, `app/core/tests/test_contract_juiz_tela.py:56`); portanto não é o mesmo censo AST proposto.

## 30/10 19:09 · sessao a5775eb684a5 · - · - · origem=Ronald (tmux)

Reuso obrigatório e mapa dos nomes:

- A pergunta VALOR já é o critério (b) do selo S3: “0 leitor com derivação própria de dinheiro”, definido como aritmética sobre atributos de período (`app/ponto/tests/test_s3_placar_exercicio.py:14`), implementado em `deriva_dinheiro` (`:183`) e cobrado em `test_b_nenhum_leitor_tem_derivacao_propria_de_dinheiro` (`:303`). O selo antigo declara expressamente que já não é o placar (`app/ponto/tests/test_s3_leitor_nao_chama_motor.py:4`). Reutilizar o detector e a lista é coerente; outro detector para essa mesma pergunta duplicaria o existente (`app/ponto/tests/test_s3_placar_exercicio.py:303`).

- Escritor de `LEITORES`: atribuição com os sete arquivos em `app/ponto/tests/test_s3_placar_exercicio.py:34`. Leitores: critério (a), `:247`; critério (b), `:305`; fiscalização do segundo montador, `:350`.

- Escritor de `DINHEIRO`: `app/ponto/tests/test_s3_placar_exercicio.py:163`. Leituras: reconhecimento de `getattr`, `:203`; reconhecimento de atributo, `:213`.

- Escritor de `NAO_E_MOTOR`: `app/ponto/tests/test_s3_placar_exercicio.py:171`. Leitura: filtro de `getattr`, `:203`. Não há esse filtro na via `ast.Attribute`, `:213`; atualmente as duas listas têm nomes distintos (`:163`, `:171`).

- Escritor de `deriva_dinheiro`: definição em `app/ponto/tests/test_s3_placar_exercicio.py:183`. Leitores: teste principal `:306`; casos sintéticos `:315`, `:317`, `:319`, `:325`, `:330`, `:358`, `:359`; teste do montador `:346` e `:350`.

Limitações concretas:

- A derivação do VALOR é medida só nos sete caminhos fixos; o teste não descobre novos leitores em `app/` (`app/ponto/tests/test_s3_placar_exercicio.py:305`). O teste chamado “o MONTADOR é UM SÓ” também só confronta o montador previamente nomeado com esses sete arquivos (`:346`, `:350`). Assim, acrescentar outro arquivo de produção não o inclui automaticamente no critério VALOR (`:34`, `:305`).

- O detector reutilizado é cego à forma real `(getattr(p, 'minutos_extra_50', 0) or 0) / 60`: a subida não atravessa `ast.BoolOp` (`app/ponto/tests/test_s3_placar_exercicio.py:205`), e a forma existe em `app/ponto/services/espelho.py:113` e `:114`. O caso que “morde” usa `getattr` direto, sem `or 0` (`app/ponto/tests/test_s3_placar_exercicio.py:324`). Isso prova limitação do detector, embora esses sítios pertençam ao montador excluído, não demonstre sozinho leitor indevido (`app/ponto/tests/test_s3_placar_exercicio.py:58`).

- Outro atributo real convertido em horas, `minutos_noturnos_relogio`, aparece em `app/ponto/services/espelho.py:103`, mas não pertence a `DINHEIRO` (`app/ponto/tests/test_s3_placar_exercicio.py:163`).

- `horas_do_dia` faz derivação do dia sem lavratura (`periodo.minutos_trabalhados / 60`) em `app/ponto/services/espelho.py:136`; a API chama esse helper e publica seu número (`app/api/views.py:1569`, `:1584`). O contrato precisa explicitar a classe desse helper: o código o considera núcleo (`app/api/views.py:1562`), e o selo exclui o arquivo inteiro como montador (`app/ponto/tests/test_s3_placar_exercicio.py:58`). Seria incorreto afirmar sem essa distinção que a API ainda calcula diretamente (`app/api/views.py:1569`).

Carga sem Django:

- Importar o selo inteiro exige Django em nível de módulo (`app/ponto/tests/test_s3_placar_exercicio.py:30`). O detector propriamente dito utiliza AST (`:190`), portanto extrair sua implementação para módulo puro evita essa dependência; importar o arquivo atual diretamente no host sem Django falha nesse import (`:30`).

- Se também transferirem `_raiz` sem adaptar sua semântica, ela sobe exatamente três diretórios a partir de `__file__`, posição própria do arquivo em `ponto/tests/` (`app/ponto/tests/test_s3_placar_exercicio.py:175`); `_fonte` usa esse resultado para resolver os sete caminhos (`:179`). O contrato lista somente detector e constantes para mudança, portanto esse erro é condicional à implementação futura [não verificado].

Verificação local: extraindo apenas AST, constantes e a função, sem importar Django nem escrever arquivos, os sete arquivos retornaram lista vazia; esse resultado corresponde ao domínio fixo do teste, não prova ausência no restante de produção (`app/ponto/tests/test_s3_placar_exercicio.py:305`).

## 30/10 19:07 · sessao 82d9cde7b5f5 · E1-BATERIA · red · origem=Code

Criado apenas [test_e1_lote1_valor_do_dia.py](/home/ronald/codex_ws/red_e54eae6ed8c4_20261030_190156_3863743/app/ponto/tests/test_e1_lote1_valor_do_dia.py), com 20 casos, fixture próprio e porta CCT real.

Sintaxe e estrutura validadas em memória, sem criar `.pyc`. Motor e suíte Django não executados; o RED ainda não foi evidenciado por execução. [nao verificado]

Valores em minutos; **N** indica noturno relógio/legal.

| Caso | Esperados | Regra citada | Origem |
|---|---|---|---|
| C01 | trab 660; N 360/360; extra 0; feriado 0 | CCT 38-d/10; CLT 73 §2 e 59-A | wt-v3 CCT-1 |
| C02 | trab 660; N 480/548,57; extra 0 | Aval 25/09/O211; CLT 73; Súmula 60 II | wt-v3 CCT-2 |
| C03 | trab 660; N 480/548,57; extra 0 | Adendo 05/10; piso de lacuna; CLT 73 | wt-v3 CCT-3, primeira metade |
| C04 | trab 660; N 480/548,57; extra 0 | Adendo 05/10; empate específico não herda | wt-v3 CCT-3, segunda metade |
| C05 | trab 420; N 360/411,43; extra 0; régua `legais` | CLT 73; Súmula 60 II; parecer 29/07 | wt-v3 CCT-4 |
| C06 | **SKIP:** N 360/411,43 ou 240/274,29 | CLT 73 §5; Súmula 60 II | wt-v3 CCT-4b |
| C07 | trab 660; HE feriado 660 | CLT 59-A; parecer 08/09; BUG 91 | wt-v3 CCT-5, primeira ponta |
| C08 | trab 660; HE feriado 0; extra 0 | CLT 59-A; parecer 08/09; BUG 91 | wt-v3 CCT-5, segunda ponta |
| C09 | trab 660; N 360/411,43; intervalo descontado 60; extra 0 | CLT 59-A, 73, 71 §2 e 4 | Novo |
| C10 | trab 660; N 360/411,43; intervalo descontado 60; extra e HE feriado 0 | CLT 59-A, 73 e 71 §2 | Novo |
| C11 | trab 360; N 300/342,86; extra 0 | CLT 73 §1/§2; Súmula 60 | wt-v3 F2 |
| C12 | trab 480; extra, HE100 e HE feriado 480; HE50 0 | Lei 605/49 art.9; Súmula 146 | wt-v3 F10 |
| C13 | trab 540; extra e HE50 60; HE100 0 | CLT 58 caput e 59 §1 | Novo |
| C14 | trab 450; atraso 30; antecipada e extra 0 | CLT 58 §1; Súmula 366 | Novo |
| C15 | atraso, antecipada e extra 0 | CLT 58 §1: tolerância 5/10 | Novo |
| C16 | **SKIP:** intervalo indenizado 30 ou 60 | CLT 71 §4; pergunta V3-BATERIA | Novo |
| C17 | trab 540; intervalo descontado 0 e indenizado 60; extra e HE50 60; HE100 0 | CLT 71 e 59 §1; L-031 | Novo |
| C18 | trab 690; extra e HE50 30; HE100 0 | CLT 59-A, 59 §1 e 71 §2 | Novo |
| C19 | FT 1, com 660 min; HE feriado e extra 0 | Súmula 146; corte 19/09 | wt-v3 F3 |
| C20 | trab 450; antecipada 30; atraso e extra 0 | CLT 58 §1; Súmula 366 | Novo |

As fontes da régua também são afirmadas nos casos CCT executáveis e em C09.

Dúvidas preservadas sem escolher resposta: **C06**, possibilidade de a CCT negar prorrogação noturna fora do 12x36; **C16**, indenização de 30 minutos suprimidos ou 60 integrais. A cl.10 tem lastro local descrito como interpretação; C01 segue o corte fornecido para o 12x36.

## 30/10 19:03 · sessao 0d8d2229d690 · O261-E-CELULA-20 · verificador · origem=Code

**Veredito: fechamento parcial.** A cura da instância velha fecha o caso descrito; permanecem falhas no selo e na nova deduplicação de auditoria. A guarda acrescentada a `abrir` também não cobre todo o pós-estado. ([models.py:206](app/chamados/models.py:206), [contrato:22](app/chamados/tests/test_contract_status_nucleo.py:22), [models.py:2327](app/chamados/models.py:2327), [models.py:2334](app/chamados/models.py:2334))

1. **Cada cura fecha o item declarado?**

| Item | Verificação |
|---|---|
| **1 — instância velha** | **Fecha o cenário sequencial.** Os quatro gates recarregam antes da decisão; o tripwire considera memória **ou banco** antes da mutação. ([models.py:595](app/chamados/models.py:595), [:618](app/chamados/models.py:618), [:676](app/chamados/models.py:676), [:1425](app/chamados/models.py:1425), [:1442](app/chamados/models.py:1442), [:206](app/chamados/models.py:206)) |
| **2 — dedup costura em terminal** | **Fecha o caso simples do `test_m`:** consulta a guarda, preserva o destino recusado e retorna `(ex, False)`. **Parcial para a classe**, pelo escape de célula abaixo. ([models.py:2321](app/chamados/models.py:2321), [:2347](app/chamados/models.py:2347), [teste:269](app/chamados/tests/test_o261e_f1_porta_encerramento.py:269)) |
| **7 — comentário de atomicidade** | **Fecha a correção textual:** agora distingue exceção propagada da falha do pai engolida pelo receiver. ([models.py:1735](app/chamados/models.py:1735), [signals.py:46](app/chamados/signals.py:46)) |
| **8 — dois estados no desligamento** | Continua sendo efeito possível da ordem declarada: disputa fecha, receiver reconcilia o pai, depois a cascata chama `encerrar`. Não é uma cura comportamental deste delta. ([desligamento.py:142](app/colaboradores/services/desligamento.py:142), [signals.py:42](app/chamados/signals.py:42), [desligamento.py:155](app/colaboradores/services/desligamento.py:155)) |
| **9 — `antes` da reata** | **Fecha:** captura o destino real antes da porta e acrescenta `ancora`; o teste confere `None` no caso pelo container. ([models.py:376](app/chamados/models.py:376), [:383](app/chamados/models.py:383), [teste:297](app/chamados/tests/test_o261e_f1_porta_encerramento.py:297)) |
| **Selo** | Corrige a âncora textual única e contém os três mutantes declarados, mas a consolidação enfraquece a anticegueira. ([selo:158](app/chamados/tests/test_o261e_f1_selo_encerramento.py:158), [contrato:22](app/chamados/tests/test_contract_status_nucleo.py:22)) |

2. **Caminhos, chamadores e mordida**

- **PROCEDE — regressão nova da anticegueira.** O contrato exige somente `bool(achados)`. Remover a escrita do núcleo deixa os dois swaps permitidos suficientes para passar; o tripwire também aceita, pois retorna ao encontrar a guarda sem exigir uma escrita posterior. Assim, perdeu-se a exigência específica de encontrar `NUCLEO`, presente no teste removido do patch. ([contrato:22](app/chamados/tests/test_contract_status_nucleo.py:22), [selo:87](app/chamados/tests/test_o261e_f1_selo_encerramento.py:87), [:115](app/chamados/tests/test_o261e_f1_selo_encerramento.py:115), [models.py:304](app/chamados/models.py:304), [:314](app/chamados/models.py:314))

- **PROCEDE — cura parcial do tripwire.** `if (...) and False:` passa porque não é `ast.Constant`; uma escrita antecipada dentro de outro `if` também passa porque só se rejeita `Assign` diretamente no corpo. Remover `or _anterior_no_banco in VIVOS` igualmente passa: a nova proteção do banco não está presa por esse selo. São limites do verificador, sem demonstração de ocorrência na produção. ([selo:107](app/chamados/tests/test_o261e_f1_selo_encerramento.py:107), [models.py:208](app/chamados/models.py:208))

- **Escape preexistente que a cura 2 deixa aberto:** a guarda julga com a célula antiga; depois `abrir` pode substituí-la. Se a antiga absolve e a nova acusa, a costura em terminal pode ser admitida antes de a pergunta tornar-se viva. O C6 distingue exatamente esses dois vereditos. Existência desse estado em produção: **[não verificado]**. ([models.py:2321](app/chamados/models.py:2321), [:2334](app/chamados/models.py:2334), [juizes.py:1054](app/chamados/juizes.py:1054))

- **PROCEDE — perda nova de auditoria:** recusa `A→M`, costura válida `A→B`, nova recusa `B→M`: a segunda recusa encontra a primeira no histórico e não registra o novo `antes=B`. A identidade pesquisada contém pergunta e destino pedido, sem origem ou ciclo. Repetir a mesma entrada deve deduplicar; isso não exige apagar a foto de um fato posterior distinto. ([models.py:2327](app/chamados/models.py:2327), [:2333](app/chamados/models.py:2333), [CLAUDE.md:299](CLAUDE.md:299), [CLAUDE.md:32](CLAUDE.md:32))

- **Chamadores:** a assinatura e o retorno de `abrir` permanecem compatíveis com os seis emissores encontrados. Porém, a nova leitura `.exists()` da auditoria está fora do best-effort de `evento`: erro nessa consulta propaga pelo ramo de recusa. Ocorrência desse erro: **[não verificado]**. ([models.py:2275](app/chamados/models.py:2275), [:2347](app/chamados/models.py:2347), [disputa_emissao.py:389](app/chamados/services/disputa_emissao.py:389), [:770](app/chamados/services/disputa_emissao.py:770), [:1160](app/chamados/services/disputa_emissao.py:1160), [acoes_disputa.py:259](app/chamados/services/acoes_disputa.py:259), [declaracao_texto.py:322](app/chamados/services/declaracao_texto.py:322), [:611](app/chamados/services/declaracao_texto.py:611), [observ.py:105](app/core/observ.py:105))

Não identifiquei segundo escritor novo nesses trechos: o helper recarrega e a persistência continua no núcleo; nascimento/identidade e reapontamento de destino já são portas declaradas. Nascimento com destino terminal continua fora da guarda, expressamente reservado à F2. A leitura do banco também não elimina a janela entre consulta e escrita. ([models.py:618](app/chamados/models.py:618), [:252](app/chamados/models.py:252), [contrato de escritores:60](app/core/tests/test_contract_escritores_estado.py:60), [fio.py:748](app/chamados/services/fio.py:748), [models.py:206](app/chamados/models.py:206))

3. **Quem escreve e quem lê a auditoria**

| Registro | Escritor | Leitores encontrados |
|---|---|---|
| `costura_recusada` | `PerguntaDisputa.abrir` monta ação, motivo e payload; `core.observ.evento` efetua o `create`. ([models.py:2332](app/chamados/models.py:2332), [observ.py:134](app/core/observ.py:134)) | O próprio `abrir` lê para deduplicar; `test_m` lê para contar uma linha. ([models.py:2328](app/chamados/models.py:2328), [teste:286](app/chamados/tests/test_o261e_f1_porta_encerramento.py:286)) |
| `pergunta_reatada` | `_gate_pergunta_viva` monta o novo payload; `evento` persiste. ([models.py:380](app/chamados/models.py:380), [observ.py:136](app/core/observ.py:136)) | `test_n` lê e confere o novo `valor_antes` e a pergunta em `valor_depois`. ([teste:296](app/chamados/tests/test_o261e_f1_porta_encerramento.py:296)) |

A tela genérica de auditoria consulta ambos os registros, mas renderiza descrição e metadados: **não exibe `valor_antes`/`valor_depois`**. Outro consumidor funcional do novo payload: **[não verificado]**. ([views.py:705](app/core/views.py:705), [log_auditoria.html:55](app/templates/core/log_auditoria.html:55))

Execução Django dos REDs/GREENs e confirmação em produção: **[não verificado]**.

_734 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
