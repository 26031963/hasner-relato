# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 22:15** · publicado em **30/10/2026 22:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 22:15 · sessao c18b7a251a62 · O261-E-CELULA-20 · verificador · origem=Code

**1. (a) Sim; (b) depende de qual instância é lida ao final. O teste não garante, sozinho, identidade das listas.** Ele verifica quantidade de carregamentos, cegueira e valor no carimbo. ([test_o261e_f5_alcance.py:254](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/tests/test_o261e_f5_alcance.py:254))

O `censo()` fica efetivamente cego: busca chamados vivos vinculados à célula e, com universo vazio, retorna `cego=True`, preservando `fio_do_admin_sem_resposta=0`, antes de consultar a lista. ([vigia_fio.py:285](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:285), [vigia_fio.py:316](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:316), [vigia_fio.py:323](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:323))

Valores deduzidos do código para `reg['censo']['fio_do_admin_sem_resposta']`:

| Costura | Valor | Resultado previsto |
|---|---:|---|
| Atual: uma lista, leitura depois dos dois universos | **1** | Satisfaz a assertiva; o command transfere o contador da lista para `d`. |
| (a) Apagar a leitura final, ou antecipá-la para antes do alcance | **0** | Vermelho na assertiva que exige 1. |
| (b) Listas separadas; leitura final observa a lista entregue ao `censo()` | **0** | Vermelho: essa instância permaneceu intocada. |
| (b) Listas separadas; leitura final observa a lista entregue ao alcance | **1** | Escapa deste teste: apenas o alcance carrega, portanto `m.call_count` continua 1. | [nao verificado]

A fundamentação da tabela é o retorno antecipado do `censo`, o carregamento tardio, o registro da falha pelo alcance e a leitura final do command. ([vigia_fio.py:187](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:187), [vigia_fio.py:208](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:208), [vigia_fio.py:567](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:567), [vigiar_fio_morto.py:166](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/management/commands/vigiar_fio_morto.py:166))

O contraexemplo concreto é trocar somente a chamada do `censo()` por `censo(..., lista=ListaDoAdmin(hoje, empresas))`, mantendo `lista` no alcance e na leitura final. São duas instâncias, mas as assertivas dessa fixture continuam satisfeitas por inferência estática. ([vigiar_fio_morto.py:167](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/management/commands/vigiar_fio_morto.py:167), [test_o261e_f5_alcance.py:254](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/tests/test_o261e_f5_alcance.py:254))

O RED fornecido mostra `1 != 0`, mas não identifica a alteração aplicada; comprovação experimental individual dos dois mutantes: **[não verificado]**. [nao verificado]

**2. A linha explica parcialmente os números: explicita o segundo, mas omite o universo do primeiro.** ([vigia_fio.py:793](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:793))

- **`FIO_DO_ADMIN`** conta dias classificados nesse balde pelo **censo antigo**, cujo universo parte de chamados vivos com célula; `celulas` lista IDs desse mesmo balde, limitada por `limite`. ([vigia_fio.py:285](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:285), [vigia_fio.py:380](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:380))
- **`sem_resposta_da_RODADA`** conta pares distintos `(colaborador, data)` que chegaram ao predicado e encontraram a lista sem resposta, nos dois universos. Não conta carregamentos: usa um conjunto e retorna seu tamanho. ([vigia_fio.py:185](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:185), [vigia_fio.py:204](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:204), [vigia_fio.py:227](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:227))
- O alcance possui **`alcance_fio_do_admin` separado**, publicado na linha `VFM|ALCANCE`; ele não é somado ao primeiro número acima. ([vigia_fio.py:569](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:569), [vigia_fio.py:623](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:623), [vigia_fio.py:795](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:795))

Quanto aos leitores:

- O command imprime a linha e grava o dicionário no carimbo; a chave JSON continua **`fio_do_admin_sem_resposta`**. ([vigiar_fio_morto.py:169](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/management/commands/vigiar_fio_morto.py:169), [vigiar_fio_morto.py:216](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/management/commands/vigiar_fio_morto.py:216))
- Os testes leem essas chaves diretamente e pelo carimbo. ([test_o261_vigia_fio.py:579](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/tests/test_o261_vigia_fio.py:579), [test_o261e_f5_alcance.py:256](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/tests/test_o261e_f5_alcance.py:256))
- **O ESTADO ainda não expõe essas chaves.** `fio_estado.py` abre o carimbo, mas omite ambas da seleção devolvida; `gerar_estado.py` usa esse leitor. A inclusão está registrada como pendência. ([fio_estado.py:82](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/bin/fio_estado.py:82), [gerar_estado.py:260](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/bin/gerar_estado.py:260), [BACKLOG.md:785](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/docs/BACKLOG.md:785))
- Outro parser específico da linha literal, ou consumidor externo à cópia: **[não verificado]**.

## 30/10 22:15 · sessao 39a15541c049 · - · - · origem=Ronald (tmux)

- A linha é **parcialmente explícita**: descreve `sem_resposta_da_RODADA` como dias distintos dos dois universos, mas não identifica `FIO_DO_ADMIN` e `celulas` como exclusivos do `censo()` antigo. Eles vêm de `d['fio_do_admin']` e `d['celulas_fio_do_admin']`, incrementados somente naquele censo; seu universo começa por chamados vivos vinculados à célula. `app/chamados/services/vigia_fio.py:793`, `app/chamados/services/vigia_fio.py:795`, `app/chamados/services/vigia_fio.py:380`, `app/chamados/services/vigia_fio.py:285`.
- O segundo número conta pares distintos `(colab_id, data_iso)` que **chegaram ao predicado** `dia_do_admin()` e encontraram a lista sem resposta; não conta tentativas de carregar nem todos os dias dos universos. O `set` deduplica e a propriedade devolve seu tamanho. `app/chamados/services/vigia_fio.py:185`, `app/chamados/services/vigia_fio.py:204`, `app/chamados/services/vigia_fio.py:208`, `app/chamados/services/vigia_fio.py:227`.
- O alcance mantém o próprio `alcance_fio_do_admin`, publicado na linha `VFM|ALCANCE|porta_fila=...|fio_do_admin=...`; não soma esse valor ao primeiro número de `VFM|FIO_DO_ADMIN`. `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:569`, `app/chamados/services/vigia_fio.py:623`, `app/chamados/services/vigia_fio.py:627`, `app/chamados/services/vigia_fio.py:795`.
- O command consome `linhas(d)` para stdout e guarda o dicionário inteiro em `reg['censo']`, preservando o nome JSON `fio_do_admin_sem_resposta`; só o rótulo textual virou `sem_resposta_da_RODADA`. `app/chamados/management/commands/vigiar_fio_morto.py:169`, `app/chamados/management/commands/vigiar_fio_morto.py:173`, `app/chamados/management/commands/vigiar_fio_morto.py:216`, `app/chamados/services/vigia_fio.py:795`.
- O consumidor do carimbo `bin/fio_estado.py` abre o JSON, mas sua seleção devolvida não inclui essas duas chaves; a comparação genérica considera somente `esperado`, e o command não inclui nenhuma delas no esperado. Assim o ESTADO atual não as mostra nem as avalia; `bin/gerar_estado.py` usa esse leitor/compositor. `bin/fio_estado.py:64`, `bin/fio_estado.py:72`, `bin/fio_estado.py:82`, `bin/fio_estado.py:92`, `app/chamados/management/commands/vigiar_fio_morto.py:87`, `app/chamados/management/commands/vigiar_fio_morto.py:95`, `bin/gerar_estado.py:260`.
- Há leitores de teste: verificam `fio_do_admin`, `fio_do_admin_sem_resposta`, deduplicação e carimbo; o teste do carimbo antigo compara todo `reg['censo']`. `app/chamados/tests/test_o261_vigia_fio.py:565`, `app/chamados/tests/test_o261_vigia_fio.py:579`, `app/chamados/tests/test_o261_vigia_fio.py:588`, `app/chamados/tests/test_o261_vigia_fio.py:780`, `app/chamados/tests/test_o261e_f5_alcance.py:256`.
- A inclusão dessas chaves no ESTADO está **registrada como pendência**, não implementada no leitor atual. `app/docs/BACKLOG.md:785`, `bin/fio_estado.py:82`.
- Não localizei parser específico da linha `VFM|FIO_DO_ADMIN` na busca integral com arquivos ocultos/ignorados; inexistência de outros consumidores externos à cópia permanece **[não verificado]**.

## 30/10 22:14 · sessao d200e8aa973f · - · - · origem=Ronald (tmux)

A leitura permite afirmar:

- Fixture sem `_chamado()`: `app/chamados/tests/test_o261e_f5_alcance.py:248`; `setUp` apenas cria/lavra célula acusando (`:58–63`), enquanto criação explícita de chamado está no helper separado (`:66–73`). A confirmação no próprio teste é `assertTrue(reg['censo']['cego'])`, em `:255`. O RED anexado chega à assertiva seguinte (`:256`), portanto passou pela de cegueira; identificação exata do mutante usado nesse RED: **[não verificado]**.
- `censo()` parte de chamados vivos vinculados à célula (`app/chamados/services/vigia_fio.py:284–288`); sem eles, retorna `cego=True` antes de acessar a lista (`:323–325`), preservando `fio_do_admin_sem_resposta=0`, inicializado em `:316`. Portanto ele está cego nessa fixture.
- Fluxo correto: command cria objeto único, passa aos dois censos e só então lê `sem_resposta` (`app/chamados/management/commands/vigiar_fio_morto.py:166–169`). Alcance consulta lei/lista (`app/chamados/services/vigia_fio.py:567–568`); `dia_do_admin` registra `(colab_id,data)` no conjunto quando a lista falha (`:204–209`), e `sem_resposta` retorna o tamanho (`:225–227`). Carimbo conserva `d` (`app/chamados/management/commands/vigiar_fio_morto.py:216`).
- Mutante (a), apagar leitura final `:169`: carimbo `reg['censo']['fio_do_admin_sem_resposta']=0`; teste vermelho em `test_o261e_f5_alcance.py:256`. Leitura final antecipada antes do alcance tem mesmo resultado, pela preguiça do carregador (`vigia_fio.py:188–190`) e retorno cego anterior. [nao verificado]
- Mutante (b), listas separadas: **não há garantia geral de vermelho**. Se `lista` continua sendo objeto passado ao censo e alcance recebe `ListaDoAdmin(...)` novo, leitura final vê lista intocada → carimbo **0** e vermelho. O loader será chamado **uma vez**, pelo alcance, então assertiva `m.call_count==1` (`test:254`) passa.
- Contraexemplo do mutante (b): alterar `command:167` para `censo(..., lista=ListaDoAdmin(hoje, empresas))`, mantendo `command:168–169` como está. Cada universo recebe sua instância; a instância do censo jamais carrega, e `lista` do alcance contabiliza falha → carimbo **1**, `m.call_count==1`, `cego=True`, e teste pode ficar verde. Evidência: retorno cedo (`vigia_fio.py:323–325`), carregamento preguiçoso (`:187–190`), assertivas apenas comportamentais (`test:254–258`). **Não executar esse contraexemplo; resultado é inferência estática.** [nao verificado]

O teste vizinho que põe o dia em ambos universos exige um único carregamento (`app/chamados/tests/test_o261e_f5_alcance.py:206–215`), mas ele fornece a lista compartilhada diretamente às funções, sem exercitar a costura do command; portanto não fecha esse contraexemplo no command.

## 30/10 21:51 · sessao 03e08bc51dac · O261-E-CELULA-20 · verificador · origem=Code

**1. Carregador, predicado e igualdade**

Há **1 sítio que chama `lista(...)`**, seguido de `dias_na_lista(...)`, e **1 sítio que aplica o predicado BATIDA/CADASTRO**. Os dois censos chegam a ele por `dono_fora_do_fio`. ([vigia_fio.py:198](app/chamados/services/vigia_fio.py:198), [vigia_fio.py:210](app/chamados/services/vigia_fio.py:210), [vigia_fio.py:263](app/chamados/services/vigia_fio.py:263))

O command passa a mesma `ListaDoAdmin` aos dois. Isso garante **o mesmo resultado do predicado para o mesmo par colaborador/data**, com no máximo uma tentativa de carregamento, inclusive quando falha. ([vigiar_fio_morto.py:166](app/chamados/management/commands/vigiar_fio_morto.py:166), [vigia_fio.py:187](app/chamados/services/vigia_fio.py:187))

Isso **não garante o mesmo balde final**: `censo` aceita pergunta viva antes de consultar a lista; `censo_alcance` exige uma porta pela fila/app e pode continuar até a lista. Portanto, compartilhar a lista elimina divergência nessa consulta, mas não torna os universos equivalentes. ([vigia_fio.py:359](app/chamados/services/vigia_fio.py:359), [vigia_fio.py:556](app/chamados/services/vigia_fio.py:556), [vigia_fio.py:567](app/chamados/services/vigia_fio.py:567))

Sem `lista`, cada chamada cria sua instância: mesmo algoritmo, caches independentes, até dois carregamentos. A igualdade depende de obterem a mesma resposta da fonte; não há snapshot compartilhado. ([vigia_fio.py:339](app/chamados/services/vigia_fio.py:339), [vigia_fio.py:550](app/chamados/services/vigia_fio.py:550))

**2. Quem escreve e quem lê `fio_do_admin_sem_resposta`**

| Papel | Local |
|---|---|
| Acumula dias distintos sem resposta em um conjunto | [vigia_fio.py:208](app/chamados/services/vigia_fio.py:208) |
| Inicializa a chave com zero | [vigia_fio.py:316](app/chamados/services/vigia_fio.py:316) |
| Copia o tamanho do conjunto ao terminar `censo` | [vigia_fio.py:425](app/chamados/services/vigia_fio.py:425) |
| **Sobrescreve após os dois censos**, incluindo consultas exclusivas do alcance | [vigiar_fio_morto.py:169](app/chamados/management/commands/vigiar_fio_morto.py:169) |
| Lê para imprimir `FIO_DO_ADMIN…sem_resposta` | [vigia_fio.py:795](app/chamados/services/vigia_fio.py:795) |
| Transporta a chave no dicionário do carimbo | [vigiar_fio_morto.py:216](app/chamados/management/commands/vigiar_fio_morto.py:216) |
| Testes leem diretamente esperando 0, 1 e 3; outro compara o dicionário inteiro | [test_o261_vigia_fio.py:571](app/chamados/tests/test_o261_vigia_fio.py:571), [579](app/chamados/tests/test_o261_vigia_fio.py:579), [588](app/chamados/tests/test_o261_vigia_fio.py:588), [780](app/chamados/tests/test_o261_vigia_fio.py:780) |

O teste novo de falha lê **`lista.sem_resposta`**, sem conferir a chave final do command. ([test_o261e_f5_alcance.py:226](app/chamados/tests/test_o261e_f5_alcance.py:226))

`bin/fio_estado.py` não publica essa chave; sua comparação genérica só considera campos de `esperado`, onde ela também não consta. Assim, mudar somente esse contador no carimbo atual não muda sua saída. ([fio_estado.py:69](bin/fio_estado.py:69), [fio_estado.py:82](bin/fio_estado.py:82), [vigiar_fio_morto.py:95](app/chamados/management/commands/vigiar_fio_morto.py:95))

**Contra `21b9db38`: [não verificado]**, pois essa versão não está no material disponível. Contra o pai descrito no patch, existe mudança observável: se a lista falha e apenas o alcance consulta um dia, texto/carimbo passam de zero para um nessa chave; agora ela recebe a união dos dias consultados pelos dois censos. ([vigiar_fio_morto.py:167](app/chamados/management/commands/vigiar_fio_morto.py:167), [vigia_fio.py:208](app/chamados/services/vigia_fio.py:208))

**3. Classificação e força dos testes** [nao verificado]

Para **os mesmos resultados das fontes**, o delta fornecido preserva `(d)/(e)`: `(d)` exige colaborador na lista e dia fora dela; dia presente com ESTRUTURA/nenhum continua em `(e)` com detalhe. A mudança explícita é o rótulo publicado de `(d)`, agora `colab_na_lista_cxr_dia_fora`. ([vigia_fio.py:474](app/chamados/services/vigia_fio.py:474), [vigia_fio.py:588](app/chamados/services/vigia_fio.py:588), [vigia_fio.py:592](app/chamados/services/vigia_fio.py:592))

A causa `(b)` também preserva a expressão anterior: carimbo presente e texto vazio após `.strip()`. Houve extração do predicado, usado tanto na recusa da porta quanto na causa. ([vigia_fio.py:559](app/chamados/services/vigia_fio.py:559), [vigia_fio.py:584](app/chamados/services/vigia_fio.py:584), [vigia_fio.py:675](app/chamados/services/vigia_fio.py:675))

**Sim, testes novos passam com código errado:** [nao verificado]

- Criar outra `ListaDoAdmin` no command para o alcance, ou remover a atualização final do contador, escapa aos novos testes: eles passam a instância manualmente e não executam essa ligação do command. ([test_o261e_f5_alcance.py:206](app/chamados/tests/test_o261e_f5_alcance.py:206), [217](app/chamados/tests/test_o261e_f5_alcance.py:217), [vigiar_fio_morto.py:168](app/chamados/management/commands/vigiar_fio_morto.py:168))
- O selo AST aceita trocar `CADASTRO` por `ESTRUTURA`: suas contagens continuam iguais, embora o predicado fique errado. Testes anteriores rejeitam esse mutante; isso não significa sobrevivência à suíte inteira. ([test_o261e_f5_alcance.py:234](app/chamados/tests/test_o261e_f5_alcance.py:234), [vigia_fio.py:168](app/chamados/services/vigia_fio.py:168), [test_o261_vigia_fio.py:553](app/chamados/tests/test_o261_vigia_fio.py:553))

Ocorrência de dias produtivos afetados e resultado da suíte Django: **[não verificado]**. [nao verificado]

## 30/10 21:51 · sessao 17ddc5b11cdf · - · - · origem=Ronald (tmux)

Há lacunas nos testes novos, sem evidência de falha atual do código nesses pontos. [app/chamados/tests/test_o261e_f5_alcance.py:206-240] [nao verificado]

- **O selo AST aceita predicado errado.** Ele conta uma constante `BATIDA`, uma leitura de `DONOS_DA_MESA_DO_ADMIN` e chamadas simples `lista`/`dias_na_lista`; não verifica `CADASTRO`, o operador de pertinência nem a regra aplicada. Trocar a constante de `('BATIDA', 'CADASTRO')` para `('BATIDA', 'ESTRUTURA')` preserva todas as suas asserções, embora inverta indevidamente a aceitação de CADASTRO/ESTRUTURA. Demonstrei isso em memória, extraindo a classe e reproduzindo as contagens, sem gravar arquivos. [app/chamados/tests/test_o261e_f5_alcance.py:234-240; app/chamados/services/vigia_fio.py:168; app/chamados/services/vigia_fio.py:210]
- **Esse mutante não sobreviveria à suíte inteira:** testes anteriores exigem CADASTRO e rejeitam ESTRUTURA; o teste F5 anterior também rejeita ESTRUTURA. [app/chamados/tests/test_o261_vigia_fio.py:553-570; app/chamados/tests/test_o261e_f5_alcance.py:173-182]
- **A integração do command fica fora dos testes novos.** Eles criam `ListaDoAdmin` e a passam manualmente aos dois censos. Um mutante que crie outra lista no `censo_alcance` do command deixa esses testes intactos, mas reintroduz duas cargas na rodada real. [app/chamados/tests/test_o261e_f5_alcance.py:206-227; app/chamados/management/commands/vigiar_fio_morto.py:166-168]
- **A atualização final do contador no command também não é cobrada pelo teste novo de falha.** Remover `d['fio_do_admin_sem_resposta'] = lista.sem_resposta` não muda seu caminho de execução: ele verifica o objeto compartilhado e os contadores de fio morto, sem executar o command. Para revelar a diferença, seria necessário um dia sem resposta exclusivo do alcance, acrescentado depois do retorno de `censo`. [app/chamados/management/commands/vigiar_fio_morto.py:167-169; app/chamados/tests/test_o261e_f5_alcance.py:222-227]
- **O caso novo de falha mede apenas um dia compartilhado.** Uma implementação errada que devolvesse apenas `int(bool(_sem_resposta))` também satisfaria suas asserções; o teste anterior de três dias a reprovaria. [app/chamados/tests/test_o261e_f5_alcance.py:219-227; app/chamados/services/vigia_fio.py:225-227; app/chamados/tests/test_o261_vigia_fio.py:581-588]

A suíte Django não foi executada; execução e resultados completos permanecem **[não verificado]**. [nao verificado]

## 30/10 21:50 · sessao 6bf930cd4c2f · - · - · origem=Ronald (tmux)

- **Um sítio de carregamento** (`lista(...)`, seguido de `dias_na_lista(...)`) e **um sítio do predicado BATIDA/CADASTRO**, ambos em `ListaDoAdmin`; a cadeia compartilhada o chama uma vez em `dono_fora_do_fio`. Referências: `app/chamados/services/vigia_fio.py:198`, `:199`, `:210`, `:263`.

- O command passa **a mesma instância** aos dois censos. O carregador memoriza sucesso ou falha, portanto faz **no máximo uma tentativa por rodada**, somente se necessário; o mesmo `(colab_id, data_iso)` recebe o mesmo resultado de `dia_do_admin`. Referências: `app/chamados/management/commands/vigiar_fio_morto.py:166`, `:167`, `:168`; `app/chamados/services/vigia_fio.py:188`, `:190`, `:204`.

- Essa igualdade vale **para a consulta à lei do admin**, sem garantir igualdade do contador/balde final dos universos: `censo` sai antes se há pergunta viva; `censo_alcance` exige porta na fila ou pergunta acessível pela fila/app antes de consultar a lei. Referências: `app/chamados/services/vigia_fio.py:359`, `:367`, `:556`, `:559`, `:567`.

- Contraexemplo dedutível: dia passado e retido, chamado vivo silenciado, pergunta viva, nenhuma das três leis anteriores aplicável e dia BATIDA na lista. `censo` classifica `fio_vivo`; `censo_alcance`, sem porta, classifica `alcance_fio_do_admin`. O silêncio retira o chamado da fila. Isso é possibilidade do código, sem ocorrência produtiva demonstrada. Referências: `app/chamados/services/vigia_fio.py:359`, `:263`, `:559`, `:567`, `:570`; `app/chamados/services/painel.py:188`. Ocorrência em produção: **[não verificado]**.

- **Sem `lista`**, cada chamada constrói sua própria `ListaDoAdmin`: mesmo algoritmo, caches independentes e até duas tentativas de carregar. Com entradas e fonte estáveis, o predicado coincide; a implementação não garante o mesmo retrato se a lista mudar ou uma chamada falhar e a outra conseguir carregar. Referências: `app/chamados/services/vigia_fio.py:339`, `:550`, `:184`, `:198`, `:201`, `:209`, `:210`.

## 30/10 21:50 · sessao 6a378797b384 · - · - · origem=Ronald (tmux)

Q2 — rastreio:

- O estado nasce em `_sem_resposta=set()`, recebe pares distintos `(colab_id,data_iso)` somente em `dia_do_admin` quando `_dias is None` e é exposto por `len(set)` (`app/chamados/services/vigia_fio.py:185`, `:208`, `:227`).
- A chave nasce com `0` no dicionário de `censo`; recebe snapshot de `lista.sem_resposta` ao final normal; o command a sobrescreve após os dois censos, incluindo dias exclusivos do alcance (`app/chamados/services/vigia_fio.py:316`, `:425`; `app/chamados/management/commands/vigiar_fio_morto.py:166`–`:169`). No retorno antecipado de `censo`, permanece o zero inicial (`app/chamados/services/vigia_fio.py:323`–`:325`).
- `linhas(d)` lê a chave e publica `VFM|FIO_DO_ADMIN=...|sem_resposta=...`; o command chama esse leitor e inclui o `d` inteiro em `censo` do carimbo (`app/chamados/services/vigia_fio.py:793`–`:795`; `app/chamados/management/commands/vigiar_fio_morto.py:173`, `:216`).
- Os testes antigos leem a chave nos cenários sucesso/uma falha/três dias, esperando `0/1/3`; o teste de carimbo lê implicitamente por comparação do dicionário inteiro (`app/chamados/tests/test_o261_vigia_fio.py:571`, `:579`, `:588`, `:780`).
- O teste novo de falha lê **`lista.sem_resposta`**, não a chave devolvida nem o valor carimbado; usa o mesmo dia nos dois universos (`app/chamados/tests/test_o261e_f5_alcance.py:219`, `:226`).
- `bin/fio_estado.py` não publica essa chave: projeta campos específicos e compara genericamente apenas os presentes em `esperado`; o command não inclui `fio_do_admin_sem_resposta` em `ESPERADO_DO_CARIMBO` (`bin/fio_estado.py:69`–`:73`, `:82`–`:92`; `app/chamados/management/commands/vigiar_fio_morto.py:87`–`:95`). Portanto aumento só nessa chave não muda o retorno/linha desse leitor com o carimbo produzido atualmente (mesmas referências).

Mudança demonstrável **contra o pai fc9beece do patch fornecido**: se a lista falha, `censo` consulta zero dias e alcance consulta um dia, texto/carimbo passam de `sem_resposta=0` para `1`, pois o command agora copia a união após alcance (`app/chamados/management/commands/vigiar_fio_morto.py:167`–`:169`; anexo do usuário [não verificado] quanto à versão histórica executada). Contra **21b9db38**, falta a versão-base; conclusão exata [não verificado].

Lacuna de selo: remover a atribuição do command em `:169` não é detectado pelo novo teste da falha, que consulta só a propriedade e usa um dia comum, sem dia exclusivo do alcance (`app/chamados/tests/test_o261e_f5_alcance.py:217`–`:227`). O teste antigo que compara carimbo a `censo()` avulso deixa de ser uma identidade universal sob falha com dias exclusivos do alcance (`app/chamados/tests/test_o261_vigia_fio.py:780`; `app/chamados/management/commands/vigiar_fio_morto.py:168`–`:169`).

## 30/10 21:31 · sessao fb6e2bc2df4b · O261-E-CELULA-20 · verificador · origem=Code

**1. Classificação: a retirada dos dois filtros é semanticamente redundante com o juiz atual; não identifiquei mudança de dia decorrente dela.** As causas continuam sendo escolhidas sobre `ps`, já filtrado por vitalidade. ([vigia_fio.py:495](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:495), [vigia_fio.py:566](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:566))

A cadeia exata é:

- As perguntas vêm de `PerguntaDisputa.objects.filter(celula_id__in=_cels)`: identidade pela FK da célula, sem casamento por `data_evento`. Todas entram em `perg_todas`; somente as aprovadas por `pergunta_viva(p)` entram em `perg`. ([vigia_fio.py:495](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:495))
- `ps = perg.get(c, []) if c else []`. ([vigia_fio.py:531](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:531))
- `pergunta_viva` delega a `_pergunta_viva_calc`, cujo primeiro degrau retorna `False` para `validada_em is not None`. O memo inclui esse campo na chave. Logo, para toda pergunta admitida em `ps`, os termos removidos eram verdadeiros. ([juizes.py:795](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/juizes.py:795), [juizes.py:1006](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/juizes.py:1006), [juizes.py:768](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/juizes.py:768))

O julgamento do dono recebe **`perg_todas`**, preservando a leitura das validadas fora de `ps`. ([vigia_fio.py:549](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:549))

**2. Testes: há mordida, mas restam duas lacunas demonstráveis por inspeção.** [nao verificado]

- **Porta antes do teto: cobertura parcial.** O teste novo cria apenas chamado com célula e verifica `porta_fila`. Um mutante com ordem **`porta_fila → teto → porta_pergunta`** passa essa asserção, embora classifique incorretamente uma pergunta alcançável de hoje. Falta repetir o cenário com chamado **sem célula**, pergunta no app e âncora na fila. ([teste:211](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/tests/test_o261e_f5_alcance.py:211), [vigia_fio.py:532](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:532))
- **Dia listado sem dono: morde corretamente.** A assinatura preserva a chave do dia com valor `None`; o helper exige a linha da célula, e a asserção exige exatamente `('e', 'dia_na_lista_dono_nenhum')`. Portanto distingue a atribuição indevida a `(d)`. ([cadastro_realidade.py:669](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/escala/services/cadastro_realidade.py:669), [teste:98](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/tests/test_o261e_f5_alcance.py:98), [teste:217](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/tests/test_o261e_f5_alcance.py:217))
- **A garantia de `ps` não está protegida pelos dois testes novos:** nenhum cria pergunta. Eles passam se a inclusão em `perg` deixar de consultar o juiz. O contrato AST também não exige essa consulta: procura comparações cruas de carimbo. Falta um negativo com pergunta validada que seria `(b)/(c)` se admitida indevidamente. ([teste:211](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/tests/test_o261e_f5_alcance.py:211), [contrato:288](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/tests/test_contract_leitores_vitalidade.py:288))

**3. Quem escreve e quem lê: nenhum resultado desses consumidores muda sob a equivalência acima.**

| Papel | Caminho confirmado |
|---|---|
| Produz o dict | `censo_alcance` acumula total, causas, detalhes e lista. ([vigia_fio.py:588](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:588)) |
| Lê causas para impressão | `linhas_alcance` publica causas e detalhes; o comando imprime suas linhas. ([vigia_fio.py:610](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:610), [comando:181](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/management/commands/vigiar_fio_morto.py:181)) |
| Escreve o carimbo | O comando guarda o dict inteiro em `alcance` e replica o total em `censo`; `_carimbar` serializa e substitui o arquivo. ([comando:211](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/management/commands/vigiar_fio_morto.py:211), [comando:130](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/management/commands/vigiar_fio_morto.py:130)) |
| Lê o carimbo para ESTADO | `fio_estado` compara `censo` com `esperado`, incluindo o total de alcance; não consulta suas causas. `gerar_estado` consome esse leitor. ([fio_estado.py:69](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/bin/fio_estado.py:69), [gerar_estado.py:260](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/bin/gerar_estado.py:260)) |

O alcance continua fora de `ALARMA_EM_0`; `gritou` é calculado antes de `alc`, usando o censo antigo. ([comando:81](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/management/commands/vigiar_fio_morto.py:81), [comando:163](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/management/commands/vigiar_fio_morto.py:163))

Execução da suíte, sobrevivência empírica dos mutantes e leitores externos à cópia: **[nao verificado]**. [nao verificado]

## 30/10 21:30 · sessao 0d5e0d25562d · - · - · origem=Ronald (tmux)

Os dois casos novos têm mordida, mas a cobertura da ordem das portas é parcial.

- `test_MORDE_porta_vence_o_teto_de_hoje` cria chamado com célula e exige `(porta_fila, teto) == (1, 0)`, portanto distingue teto antes de `porta_fila`. Porém passa com mutante que coloca o teto **entre** `porta_fila` e `porta_pergunta`: o caso não cria pergunta; os casos existentes de pergunta usam `DIA=01/09` e `HOJE=25/09`, portanto também não exercitam pergunta de hoje. (`app/chamados/tests/test_o261e_f5_alcance.py:25`, `:26`, `:84`, `:120`, `:211`; ordem atual em `app/chamados/services/vigia_fio.py:532`, `:535`, `:538`.)

- `test_dia_na_lista_SEM_dono_lavrado_e_outra_e_nao_cadastro` é direto e não vazio: injeta assinatura com data sem dono; `dias_na_lista` guarda a chave com valor `None`; `_causa_do_dia` exige exatamente uma linha da célula; a asserção exige `('e','dia_na_lista_dono_nenhum')`. Mata a condição antiga de `(d)` que verificava só presença do colaborador e ignorava presença do dia. (`app/chamados/tests/test_o261e_f5_alcance.py:163`, `:217`, `:98`; `app/escala/services/cadastro_realidade.py:669`; `app/chamados/services/vigia_fio.py:570`, `:574`.)

A confiança em `ps` não é provada pelos testes F5: `_pergunta` nunca planta `validada_em`; os dois casos novos nem criam perguntas. Um mutante que substitua o filtro `if pergunta_viva(p)` por inclusão incondicional não é distinguido pelas fixtures examinadas. O contrato de vitalidade procura comparações de carimbo via AST; não exige que o consumidor chame o juiz, portanto esse mutante também passa sua condição estrutural. (`app/chamados/tests/test_o261e_f5_alcance.py:75`, `:211`; `app/chamados/services/vigia_fio.py:495`; `app/chamados/tests/test_contract_leitores_vitalidade.py:288`, `:299`.)

`AGORA` é literalmente 15:00 UTC, enquanto o decorator contém `12:00:00` sem `tz_offset`; nenhuma asserção confere a igualdade anunciada pelo comentário. Para as classificações examinadas, os usos são presença de `respondida_em` e silêncio deslocado em três dias; a fila continua consultando `timezone.now()`. (`app/chamados/tests/test_o261e_f5_alcance.py:28`, `:31`, `:73`, `:82`; `app/chamados/services/vigia_fio.py:525`; `app/chamados/services/painel.py:188`.)

Execução dos testes e sobrevivência empírica de mutantes: [não verificado]. [nao verificado]

## 30/10 21:30 · sessao 24bb4feb6c4f · - · - · origem=Ronald (tmux)

O delta não muda o formato do dict nem do carimbo; altera apenas os predicados que escolhem causas `b/c`, e o incremento de total permanece comum a todas as causas (`app/chamados/services/vigia_fio.py:566`, `:568`, `:588`, `:593`).

- **Escritor do dict:** `censo_alcance` inicializa contadores, causas e listas, depois acumula resultado por dia (`app/chamados/services/vigia_fio.py:458`, `:461`, `:588`).
- **Leitor direto produtivo:** `vigiar_fio_morto.handle` chama `censo_alcance`, passa o resultado a `linhas_alcance` e imprime as linhas (`app/chamados/management/commands/vigiar_fio_morto.py:181`).
- **Leitor das causas:** `linhas_alcance` lê os cinco contadores e `alcance_e_detalhe`; portanto uma eventual reclassificação mudaria essa saída (`app/chamados/services/vigia_fio.py:610`).
- **Escritor do carimbo:** o comando grava o dict inteiro em `registro['alcance']` e replica só o total em `registro['censo']['fio_morto_alcance']`; `_carimbar` serializa JSON e substitui o arquivo (`app/chamados/management/commands/vigiar_fio_morto.py:211`, `:213`, `:130`). Só a rodada sem recorte carimba (`app/chamados/management/commands/vigiar_fio_morto.py:200`, `:219`).
- **Leitor do carimbo:** `bin/fio_estado.py` compara dinamicamente `registro['censo']` contra `registro['esperado']`; o total de alcance participa dessa comparação porque está no esperado. Ele não usa causas/detalhes do bloco `alcance` (`bin/fio_estado.py:69`, `:72`; `app/chamados/management/commands/vigiar_fio_morto.py:94`). `gerar_estado` consome `ler/linha` desse módulo (`bin/gerar_estado.py:260`).
- **Alarme/saída:** alcance não integra `ALARMA_EM_0`; `gritou` é calculado com o censo antigo antes de obter `alc`. A mera redistribuição entre causas não muda o código de saída (`app/chamados/management/commands/vigiar_fio_morto.py:81`, `:163`, `:181`, `:222`). O cron configura `--alarme --alarme-passivo`, mas usa essas mesmas listas (`app/config/crons.py:1654`).
- **Testemunhas:** teste F5 verifica esperado, total nos dois lugares do carimbo e linha impressa, sem comparar causas/detalhes completos (`app/chamados/tests/test_o261e_f5_alcance.py:247`). O teste antigo compara o bloco `censo`, incluindo apenas o total de alcance (`app/chamados/tests/test_o261_vigia_fio.py:780`).

Assim, condicionada à equivalência semântica confirmada pelo juiz, nenhum desses leitores muda de resultado; se surgisse diferença entre `b/c/e`, ela apareceria nas linhas de causas e no bloco completo `alcance`, enquanto o painel/alarme continuaria usando o mesmo total (`app/chamados/services/vigia_fio.py:588`, `:610`; `app/chamados/management/commands/vigiar_fio_morto.py:211`; `bin/fio_estado.py:72`). Leitores externos à cópia e comportamento efetivo em produção: [nao verificado].

## 30/10 21:30 · sessao 86e7a786e1b8 · - · - · origem=Ronald (tmux)

Não encontrei mudança de classificação decorrente da retirada de `validada_em` em (b)/(c): ambas iteram exclusivamente `ps`, e toda pergunta de `ps` já passou por `pergunta_viva(p)`; esse juiz devolve `False` imediatamente quando `validada_em is not None`. Assim, nas duas condições antigas o termo removido era sempre verdadeiro. (`app/chamados/services/vigia_fio.py:497`, `app/chamados/services/vigia_fio.py:531`, `app/chamados/juizes.py:1006`)

Cadeia exata:

- O universo nasce dos dias de `fora_da_folha`, por empresa e competência; as células correspondentes são buscadas por colaborador/data. (`app/chamados/services/vigia_fio.py:466`, `app/chamados/services/vigia_fio.py:480`)
- A query de perguntas usa **FK `celula_id__in=_cels`**, com `select_related('disputa','celula')`; não deriva identidade por `data_evento`. (`app/chamados/services/vigia_fio.py:495`)
- `perg_todas` recebe todas; `perg` recebe apenas as aprovadas por `pergunta_viva`; `ps = perg.get(c, []) if c else []`. (`app/chamados/services/vigia_fio.py:496`, `app/chamados/services/vigia_fio.py:497`, `app/chamados/services/vigia_fio.py:531`)
- `pergunta_viva` delega a `_pergunta_viva_calc` em `juizes.py:795`; o primeiro degrau mata a validada em `juizes.py:1006`. O memo inclui `validada_em` na chave, portanto o cache normal não contorna esse degrau após mudança do campo. (`app/chamados/juizes.py:768`, `app/chamados/juizes.py:791`)
- O dono recebe **`perg_todas`**, necessário para reconhecer folga validada, e não o conjunto vivo `ps`; isso não foi removido. (`app/chamados/services/vigia_fio.py:549`, `app/chamados/services/vigia_fio.py:199`)
- (b) continua exigindo carimbo de resposta e texto vazio; (c) continua exigindo resposta carimbada e âncora terminal. A âncora vem de `chamado_destino_id`, com fallback para o chamado da disputa. (`app/chamados/services/vigia_fio.py:525`, `app/chamados/services/vigia_fio.py:566`, `app/chamados/services/vigia_fio.py:568`, `app/chamados/services/fio.py:116`)

A alteração resolve uma violação **estrutural** do contrato AST: ele procura comparação crua de `validada_em`/`materializada_em` com `None`, sem provar que o conjunto já foi filtrado. O teste contratual rejeitaria as duas leituras antigas mesmo sendo redundantes semanticamente. (`app/chamados/tests/test_contract_leitores_vitalidade.py:288`, `app/chamados/tests/test_contract_leitores_vitalidade.py:301`)

Nenhuma execução ou mutação realizada nesta exploração; comportamento observado em produção além dos anexos: [não verificado].

## 30/10 21:12 · sessao 2792b753efd0 · O261-E-CELULA-20 · verificador · origem=Code

**1. A extração não altera os contadores de `censo()`**, considerando os mesmos dados e respostas dos leitores. A cadeia preserva a precedência do pai transcrito no diff: decisão → ausência → folga validada → admin. ([vigia_fio.py:194](/home/ronald/codex_ws/6a8c5300a64ff11b7fa84c1584c27615dfe38cd9/app/chamados/services/vigia_fio.py:194))

| Comparação pai × novo | Resultado |
|---|---|
| Decisão | Mesmo `resolver_dia_oferecido_na_data(colab, data)`; incrementa o contador, acrescenta `c.pk` a `pendentes` e continua. `app/chamados/services/vigia_fio.py:194`, `app/chamados/services/vigia_fio.py:329` |
| Ausência | Mesmo `ausencia_cobre(..., para='cobranca')`, teste de não nulo e `status in ABERTAS`; mesmo incremento e `continue`. `app/chamados/services/vigia_fio.py:196`, `app/chamados/services/vigia_fio.py:333` |
| Folga | Mesmo `any`, estado `VALIDADA` e `_e_aceite_de_folga`; preservados contador, lista `celulas_escala_nao_descreve_o_dia`, limite e `continue`. `app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:336` |
| Admin | Continua sendo o último teste; preservados contador, lista `celulas_fio_do_admin`, limite e `continue`. `app/chamados/services/vigia_fio.py:201`, `app/chamados/services/vigia_fio.py:341` |

Os argumentos são o mesmo colaborador, data e **todas** as perguntas da célula (`ps`). `lambda c=c: _fio_do_admin(c)` captura a célula corrente e mantém a chamada adiada; a falha da lista continua incrementando `fio_do_admin_sem_resposta` uma vez por dia que chega a esse teste. `app/chamados/services/vigia_fio.py:304`, `app/chamados/services/vigia_fio.py:328`, `app/chamados/services/vigia_fio.py:284`, `app/chamados/services/vigia_fio.py:297`.

Permanecem os filtros anteriores, os incrementos de morto, `celulas_do_fio_morto`, `classes_do_fio_morto` e a classificação posterior de `pendentes`; portanto, os contadores derivados também conservam suas entradas. `app/chamados/services/vigia_fio.py:315`, `app/chamados/services/vigia_fio.py:346`, `app/chamados/services/vigia_fio.py:358`, `app/chamados/services/vigia_fio.py:380`.

**2. Não encontrei contraexemplo residual no delta de `(d)`, dentro do contrato declarado.** O teste anterior de véu impede `(d)` com chamado vivo; `not ps` exige ausência de pergunta viva; a nova condição exige colaborador listado **e dia ausente**. `app/chamados/services/vigia_fio.py:497`, `app/chamados/services/vigia_fio.py:557`, `app/chamados/services/vigia_fio.py:569`.

Sem porta e vencidas as leis anteriores:

- Dia listado com BATIDA/CADASTRO sai para `alcance_fio_do_admin`. `app/chamados/services/vigia_fio.py:548`.
- Dia listado com ESTRUTURA/sem dono, sem pergunta viva, recebe `(e, dia_na_lista_dono_estrutura|nenhum)`. A presença da chave distingue “sem dono” de “dia ausente”. `app/chamados/services/vigia_fio.py:573`, `app/escala/services/cadastro_realidade.py:670`.
- Sem célula recebe `(e, sem_celula)`; com pergunta viva segue as causas da pergunta. `app/chamados/services/vigia_fio.py:560`, `app/chamados/services/vigia_fio.py:564`, `app/chamados/services/vigia_fio.py:579`.

Nenhum dia do universo some ou duplica no processamento normal: `setdefault((colab, data), ...)` deduplica a identidade; cada chave passa uma vez pelo laço; cada saída incrementa um balde e continua, ou incrementa exatamente um morto e uma causa. `limite` trunca somente a lista. `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:529`, `app/chamados/services/vigia_fio.py:532`, `app/chamados/services/vigia_fio.py:551`, `app/chamados/services/vigia_fio.py:587`.

O teste cobre explicitamente ESTRUTURA, mas falta a asserção correspondente a `dia_na_lista_dono_nenhum`. `app/chamados/tests/test_o261e_f5_alcance.py:174`.

**3. Sim: o mutante que antecipa hoje sobrevive à cobertura do arquivo, por análise estática.** Os outros três possuem asserções que os detectariam. `app/chamados/tests/test_o261e_f5_alcance.py:114`, `app/chamados/tests/test_o261e_f5_alcance.py:141`, `app/chamados/tests/test_o261e_f5_alcance.py:185`, `app/chamados/tests/test_o261e_f5_alcance.py:193`.

| Mutante | Resultado pela leitura dos testes | [nao verificado]
|---|---|
| Hoje antes das portas | **Sobrevive:** portas são testadas no dia passado; o caso `hoje=DIA` não cria porta. Falta cruzar hoje com cada porta e conferir o balde. `app/chamados/tests/test_o261e_f5_alcance.py:25`, `:114`, `:121`, `:206` |
| Remover filtro do app | Detectado pelo negativo fora do app: surgiria uma porta indevida. `app/chamados/tests/test_o261e_f5_alcance.py:185` |
| Remover exclusão da âncora na fila do véu | Detectado pela resposta vazia: mudaria `(b)` para `(a)`; também quebra o caso fora do app. `app/chamados/tests/test_o261e_f5_alcance.py:141`, `:185`; `app/chamados/services/vigia_fio.py:557` |
| Contar `alcance_lista_sem_resposta` duas vezes | Detectado pelo `assertEqual(1, ...)` específico; as somas gerais não incluem esse contador. `app/chamados/tests/test_o261e_f5_alcance.py:197`, `:92` |

Execução da suíte, execução dos mutantes e ocorrência desses cenários em produção: **[nao verificado]**. [nao verificado]

## 30/10 21:11 · sessao 4caceba3830f · - · - · origem=Ronald (tmux)

Não encontrei erro residual na correção de (d), considerando o contrato declarado e as fontes da cópia:

- **Sem pergunta viva + colaborador listado + dia fora da lista:** cai em (d). O `elif veu` anterior impede atribuir (d) quando há chamado vivo; `ps` contém somente perguntas vivas. `app/chamados/services/vigia_fio.py:494-498`, `:557-570`.
- **Dia listado, dono BATIDA/CADASTRO:** sai para `alcance_fio_do_admin`; **ESTRUTURA ou None**, sem pergunta viva, permanece em (e), detalhe `dia_na_lista_dono_estrutura` ou `dia_na_lista_dono_nenhum`. `app/chamados/services/vigia_fio.py:543-553`, `:573-576`. A leitura conserva presença e dono lavrado do dia, sem derivação nova. `app/escala/services/cadastro_realidade.py:655-670`.
- **Com pergunta viva:** não entra em (d) nem no novo detalhe do dia listado; segue as causas e detalhes da pergunta. **Sem célula:** recebe `(e, sem_celula)`, antes de (d). São precedências expressas no código. `app/chamados/services/vigia_fio.py:560-586`.
- **Nenhum dia do universo some ou duplica no processamento normal:** o universo é um dicionário por `(colab, data)`; cada chave é percorrida uma vez; as portas, hoje e quatro leis incrementam um balde e continuam; o restante incrementa exatamente um morto e uma causa. O limite trunca apenas a amostra. `app/chamados/services/vigia_fio.py:465-475`, `:529-553`, `:587-592`.
- **A competência não duplica dias:** as janelas consecutivas têm início no corte e fim no corte menos um do mês seguinte; as datas vêm da mesma janela usada por `fora_da_folha`. `app/ponto/janelas.py:10-21`, `:54-72`; `app/folha/fora_da_folha.py:119-139`, `:168`.
- **Detalhe literal `outra`:** não identifiquei entrada regular que o alcance. Sem véu, terminal ou âncora ausente, as âncoras têm de estar na fila; se alguma pergunta aparece no app e tem resposta válida, já seria porta; se tem resposta vazia, cai em (b). Pergunta viva nunca tem `validada_em` preenchida. `app/chamados/catalogo/motor.py:19-31`; `app/chamados/juizes.py:1005-1007`; `app/chamados/services/vigia_fio.py:535`, `:557-586`. Estado fora do catálogo ou inconsistência de FK em produção: [nao verificado].

O teste já verifica explicitamente ESTRUTURA contra (d), mas não inclui o ramo `None`/`dia_na_lista_dono_nenhum`. `app/chamados/tests/test_o261e_f5_alcance.py:174-183`.

## 30/10 21:10 · sessao a3d62278a9a7 · - · - · origem=Ronald (tmux)

**Q1: a extração preserva os valores de `censo()` para os mesmos dados e respostas dos leitores.** As quatro condições do pai no diff anexo aparecem na mesma ordem e com os mesmos argumentos no helper: `resolver_dia_oferecido_na_data(colab, data)`; `ausencia_cobre(colab, data, para='cobranca')` seguida do teste `status in ABERTAS`; `any(p.estado == p.VALIDADA and _e_aceite_de_folga(p)...)`; chamada do admin. Cada retorno encerra a cadeia no primeiro resultado verdadeiro. (`app/chamados/services/vigia_fio.py:194`, `:196`, `:199`, `:201`.)

| Item comparado ao pai do diff anexo | Resultado na cópia |
|---|---|
| Ponto da extração | Continua depois dos filtros de outro vocabulário, fio vivo e hoje/futuro; esses filtros mantêm seus incrementos e `continue`. (`app/chamados/services/vigia_fio.py:315`, `:320`, `:323`.) |
| Argumentos | São o mesmo colaborador, a mesma data e a lista completa `ps`, obtida por célula. (`app/chamados/services/vigia_fio.py:304`, `:314`, `:328`.) |
| Lambda do admin | `lambda c=c: _fio_do_admin(c)` captura a célula corrente e só chama `_fio_do_admin` se as três leis anteriores falharem; não antecipa a consulta da lista. (`app/chamados/services/vigia_fio.py:328`, `:201`.) |
| Decisão pendente | Incrementa o mesmo contador, acrescenta `c.pk` a `pendentes` e continua. (`app/chamados/services/vigia_fio.py:329`.) |
| Ausência aberta | Incrementa o mesmo contador e continua. (`app/chamados/services/vigia_fio.py:333`.) |
| Folga validada | Incrementa o mesmo contador; preserva `len(lista) < limite`, o mesmo `c.pk` e o `continue`. (`app/chamados/services/vigia_fio.py:336`.) |
| Fio do admin | Incrementa o mesmo contador; preserva `len(lista) < limite`, o mesmo `c.pk` e o `continue`. (`app/chamados/services/vigia_fio.py:341`.) |
| Falha da lista | Continua armazenada uma vez em `_na_lista`, e cada dia que efetivamente chega ao admin incrementa `fio_do_admin_sem_resposta` uma vez. (`app/chamados/services/vigia_fio.py:284`, `:296`, `:297`.) |
| Nenhuma lei satisfeita | Cai nos mesmos incrementos de morto, abertura por ausência/presença de perguntas, cobrança e listas. (`app/chamados/services/vigia_fio.py:346`.) |
| Contadores derivados | `mortos` segue alimentando a classificação; `pendentes` segue alimentando `decisao_do_dia_pendente_com_alvo`, sem mudança nas operações segundo o diff. (`app/chamados/services/vigia_fio.py:358`, `:380`.) |

Os testes antigos contêm verificações para precedência decisão/ausência, BATIDA/CADASTRO contra ESTRUTURA/sem dono, falha da lista uma vez por rodada e a lista da folga validada; a execução deles nesta análise não foi realizada **[nao verificado]**. (`app/chamados/tests/test_o261_vigia_fio.py:531`, `:553`, `:581`, `:626`.)

A equivalência com o pai foi examinada pelo **diff fornecido**, não por checkout ou execução da revisão pai **[nao verificado]**.

## 30/10 21:10 · sessao 295113302444 · - · - · origem=Ronald (tmux)

O mutante **“hoje antes das portas” sobrevive à cobertura deste arquivo por análise estática**: as portas são plantadas em `DIA=01/09`, enquanto `HOJE=25/09`; o único caso que chama `hoje=DIA` não cria porta. Falta cruzar **dia de hoje + porta existente**, conferindo qual balde recebe o dia (`app/chamados/tests/test_o261e_f5_alcance.py:25`, `:26`, `:114`, `:121`, `:206`; ordem atual em `app/chamados/services/vigia_fio.py:532`, `:535`, `:538`).

| Mutante | Resultado estático | Evidência |
|---|---|---|
| Mover teto de hoje antes das portas | Sobrevive à cobertura do arquivo; nenhum caso cruza hoje com porta. | `app/chamados/tests/test_o261e_f5_alcance.py:114`, `:121`, `:206` |
| Remover `p.pk in no_app(cid)` da porta | Morto pelo negativo fora do app: o mutante produz porta e perde a linha esperada em `(e)`. | `app/chamados/services/vigia_fio.py:535`; `app/chamados/tests/test_o261e_f5_alcance.py:185`, `:190`, `:191` |
| Remover `chamado_que_lista(p) not in fila_pk` do véu | Morto pelo caso de resposta vazia: a âncora `aberto`, sem célula e na fila, passa a compor o véu; `(a)` ganha precedência sobre `(b)`. Também quebra o negativo fora do app, de `(e)` para `(a)`. | `app/chamados/tests/test_o261e_f5_alcance.py:141`, `:142`, `:146`, `:185`, `:191`; `app/chamados/services/vigia_fio.py:557`, `:562`, `:564`; `app/chamados/catalogo/motor.py:24` |
| Incrementar `alcance_lista_sem_resposta` duas vezes por dia | Morto: teste da lista falhando exige exatamente `1`. | `app/chamados/services/vigia_fio.py:543`, `:546`; `app/chamados/tests/test_o261e_f5_alcance.py:193`, `:197` |

As somas auxiliares não detectam duplicação de `alcance_lista_sem_resposta`, pois esse contador não participa delas; quem detecta é a asserção específica `==1` (`app/chamados/tests/test_o261e_f5_alcance.py:90`, `:92`, `:95`, `:97`, `:197`).

Resultado de execução dos testes/mutantes: **[nao verificado]**. [nao verificado]

## 30/10 21:04 · sessao 05e1da98864e · O261-E-CELULA-20 · verificador · origem=Code

**Não daria aval de fechamento da F5:** há porta real que o censo pode perder, causa `(d)` mal atribuída e o alarme continua governado pelo contador antigo. (`app/chamados/services/vigia_fio.py:495`, `app/chamados/services/vigia_fio.py:569`, `app/chamados/management/commands/vigiar_fio_morto.py:163`.)

Limite da verificação: a cópia não contém `.git`; `git show eede1619` falhou. Diff efetivo, comparação com `21b9db38` e resultados de testes executados: **[não verificado]**. A análise abaixo é da árvore fornecida; ocorrência dos contraexemplos em produção: **[não verificado]**.

**1. Contagem, universo, portas e causas**

- **Porta real não reconhecida.** Contraexemplo: dia retido com célula; pergunta do mesmo colaborador/data, `celula=NULL`, respondida com `07:00`, não validada, sem tranca; destino aberto, sem célula, presente na fila. A FK nula é permitida, o juiz mantém essa resposta viva, o app busca por colaborador e a âncora usa o destino. Entretanto, o vigia busca perguntas exclusivamente pela FK da célula e perde essa porta. (`app/chamados/models.py:1943`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1032`, `app/chamados/services/pergunta_no_app.py:42`, `app/chamados/services/fio.py:117`, `app/chamados/services/vigia_fio.py:495`.)

- **Causa `(d)` mal atribuída.** Contraexemplo: nenhuma pergunta/chamado vivo; lista Cadastro x Realidade contém assinatura **daquele dia**, com dono `ESTRUTURA`; os outros baldes não se aplicam. O dia não sai pelo balde do admin, pois este exige `BATIDA/CADASTRO`; depois recebe `(d)` apenas porque o colaborador aparece na lista. Isso contradiz a definição local de `(d)`, que exige o **dia fora da lista**. Falta conferir a ausência de `(cid, dt)` no mapa dos dias. (`app/chamados/services/vigia_fio.py:418`, `app/chamados/services/vigia_fio.py:548`, `app/chamados/services/vigia_fio.py:569`.)

- **Competências omitidas ou indevidamente incluídas.** O enumerador considera somente a atual e a anterior; verifica “entregue” apenas para a anterior. Assim, competência mais antiga ainda não entregue desaparece; a atual já entregue continua elegível. Isso contraria a formulação ampla “competência não entregue”, embora seja o recorte do enumerador escolhido. (`app/ponto/services/he_pendente_lavrado.py:220`, `app/ponto/services/he_pendente_lavrado.py:223`, `app/chamados/services/vigia_fio.py:467`.)

- **Outros limites herdados do universo:** entram somente colaboradores cujo motivo principal é `furo_espelho`. Um colaborador com dia acusando e `sem_codigo_dominio` fica fora; quem não tem `FechamentoMensal` também fica fora. Portanto, não é um censo de todos os impedimentos da folha: é a fatia por dias publicada em `fora_da_folha()['colabs']`. (`app/folha/fora_da_folha.py:129`, `app/folha/export.py:766`, `app/folha/export.py:846`, `app/chamados/services/vigia_fio.py:469`.)

- **Porta aceita sem conferir sua pergunta.** Qualquer chamado da célula na fila encerra a análise antes de verificar resposta vazia, app ou âncora. Repetir o teste `(b)` com `celula=True` produz `porta_fila`, não `(b)`. Isso segue a definição local de porta direta, mas não demonstra que existe pergunta utilizável; é uma limitação diante do critério mais estrito de pergunta alcançável. (`app/chamados/services/vigia_fio.py:412`, `app/chamados/services/vigia_fio.py:532`, `app/chamados/services/vigia_fio.py:535`, `app/chamados/tests/test_o261e_f5_alcance.py:142`.)

- **Não encontrei duplicação lógica de dia:** a chave é `(colaborador, data)`, com `setdefault`, e cada ramo termina em `continue`. Várias perguntas/chamados não multiplicam o dia; eventual sobreposição entre competências conserva apenas o primeiro rótulo. A unicidade da célula também é por colaborador/data. (`app/chamados/services/vigia_fio.py:465`, `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:529`, `app/escala/models.py:1788`.)

**2. Quem escreve e quem lê**

A tabela identifica os escritores encontrados e os leitores relevantes ao caminho auditado. Exaustividade de **todos** os leitores da aplicação: **[não verificado]**.

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.veredito` | `lavrar_veredito` delega à porta; atribuição real na porta da célula. (`app/escala/models.py:1775`, `app/ponto/portas/celula.py:692`.) | Retenção da folha, censo antigo e juiz de vitalidade. (`app/folha/export.py:1215`, `app/chamados/services/vigia_fio.py:267`, `app/chamados/juizes.py:1054`.) |
| `CelulaDia.colaborador`, `data` | Nascimento/regeneração pela porta da célula. (`app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:593`.) | Alcance monta identidade `(colaborador,data)`; censo antigo usa colaborador/data nos baldes. (`app/chamados/services/vigia_fio.py:480`, `app/chamados/services/vigia_fio.py:328`.) |
| `PerguntaDisputa.respondida_em`, `resposta_colab` | Resposta humana; resposta automática; reabertura limpa ambos; materialização também pode carimbar resposta. (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1392`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/materializacao.py:513`.) | Juiz distingue carimbo com texto; alcance detecta vazio e causas; folha lê vitalidade/estado. (`app/chamados/juizes.py:1020`, `app/chamados/services/vigia_fio.py:526`, `app/chamados/services/vigia_fio.py:564`, `app/folha/fora_da_folha.py:66`.) |
| `PerguntaDisputa.validada_em` | Validação humana; devolução/correção/reabertura limpam; materialização autovalida. (`app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`, `app/chamados/services/disputa_emissao.py:1260`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/materializacao.py:515`.) | Juiz mata pergunta validada; alcance distingue `(b)/(c)`; estado declarado também lê o campo. (`app/chamados/juizes.py:1006`, `app/chamados/services/vigia_fio.py:564`, `app/chamados/services/vigia_fio.py:566`, `app/chamados/models.py:2204`.) |
| `PerguntaDisputa.chamado_destino` | Nascimento/reapontamento pela identidade e religação pelo serviço do fio; exclusão do destino pode produzir `NULL`. (`app/chamados/models.py:2165`, `app/chamados/models.py:2182`, `app/chamados/services/fio.py:797`, `app/chamados/models.py:2379`.) | `chamado_que_lista`: destino, senão container da disputa; alcance usa essa autoridade. (`app/chamados/services/fio.py:116`, `app/chamados/services/vigia_fio.py:500`, `app/chamados/services/vigia_fio.py:535`.) |
| `PerguntaDisputa.celula` | Nascimento/reapontamento pela identidade; FK permite `NULL` e `SET_NULL`. (`app/chamados/models.py:2168`, `app/chamados/models.py:2182`, `app/chamados/models.py:1943`.) | Agrupamento do alcance e juiz de vitalidade. (`app/chamados/services/vigia_fio.py:495`, `app/chamados/juizes.py:1017`.) |
| `ChamadoColaborador.status_local` | `_gravar_estado`; nascimento recebe status. (`app/chamados/models.py:192`, `app/chamados/models.py:851`.) | Fila usa `COBRANCA`; alcance usa `VIVOS`, status das âncoras e `TERMINAIS`. (`app/chamados/services/painel.py:187`, `app/chamados/services/vigia_fio.py:491`, `app/chamados/services/vigia_fio.py:501`, `app/chamados/services/vigia_fio.py:566`.) |
| `ChamadoColaborador.celula` | Nascimento resolve a célula; existentes passam por `apontar_celula`. (`app/chamados/models.py:830`, `app/chamados/services/vinculos.py:98`.) | Fila é projetada em células; essa associação basta para `porta_fila`. (`app/chamados/services/vigia_fio.py:486`, `app/chamados/services/vigia_fio.py:532`.) |
| `ChamadoColaborador.silenciado_ate` | Porta `silenciar` escreve; `levantar` limpa. (`app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:201`.) | `qs_na_fila` exclui prazo futuro; alcance consome seu resultado. (`app/chamados/services/painel.py:188`, `app/chamados/services/vigia_fio.py:486`.) |

A afirmação documental “escritor único de `validada_em`” não descreve todas as atribuições existentes: há autovalidação e limpeza em outros arquivos, inclusive enumerados pelo contrato. Sua introdução neste commit é **[não verificado]**. (`app/docs/JUIZES.md:38`, `app/chamados/services/materializacao.py:515`, `app/core/tests/test_contract_escritores_estado.py:183`.)

Quanto ao carimbo:

- **Escritor:** o command compõe `censo.fio_morto_alcance`, o dict completo `alcance` e `esperado`; `_carimbar` grava `.novo` e faz `os.replace`. Só carimba frota, antes da saída por alarme. (`app/chamados/management/commands/vigiar_fio_morto.py:201`, `app/chamados/management/commands/vigiar_fio_morto.py:211`, `app/chamados/management/commands/vigiar_fio_morto.py:130`, `app/chamados/management/commands/vigiar_fio_morto.py:218`, `app/chamados/management/commands/vigiar_fio_morto.py:222`.)
- **Leitor afetado:** `bin/fio_estado.py` percorre `esperado`; logo o novo contador passa automaticamente a aparecer em “FORA DO ESPERADO”. O título e o aviso CEGO continuam antigos; o dict completo `alcance` não é consumido por esse leitor. (`bin/fio_estado.py:69`, `bin/fio_estado.py:101`, `bin/fio_estado.py:105`, `bin/fio_estado.py:109`.)
- **O retorno de `censo()` não recebe a chave nova:** ela é acrescentada somente à cópia para JSON. O materializador continua consultando `censo()` antigo. (`app/chamados/management/commands/vigiar_fio_morto.py:211`, `app/chamados/management/commands/materializar_passivo_validado.py:201`.)
- **Alarme permanece antigo:** `gritou` é calculado antes de `censo_alcance`; nem `--alarme-passivo` inclui o novo contador. O teste exige essa exclusão. (`app/chamados/management/commands/vigiar_fio_morto.py:163`, `app/chamados/management/commands/vigiar_fio_morto.py:181`, `app/chamados/tests/test_o261e_f5_alcance.py:230`.)

**3. Extração de `dono_fora_do_fio` versus pai**

**Equivalência linha a linha e ausência de alteração para qualquer entrada: [não verificado]**, pela indisponibilidade do pai.

Na árvore atual, a ordem é: decisão pendente → ausência aberta → folga contestada validada → lista com dono BATIDA/CADASTRO → nenhum balde. `censo()` chama a função depois de excluir fio vivo e hoje/futuro; cada retorno incrementa seu contador correspondente. Isso permite descrever a implementação atual, mas não provar extração literal. (`app/chamados/services/vigia_fio.py:194`, `app/chamados/services/vigia_fio.py:196`, `app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:201`, `app/chamados/services/vigia_fio.py:320`, `app/chamados/services/vigia_fio.py:328`.)

**4. Checklist das classes de reprovação**

| Classe | Verificação |
|---|---|
| **Segundo juiz** | Vitalidade, vida/cobrança e Resolver dia continuam delegados às autoridades existentes; os registros apontam para elas. Os defeitos encontrados são de seleção/alcance e classificação do instrumento. (`app/chamados/services/vigia_fio.py:441`, `app/chamados/services/vigia_fio.py:194`, `app/core/juizes.py:233`, `app/core/juizes.py:238`, `app/core/juizes.py:684`.) |
| **Segundo escritor fora da porta** | Não identifiquei atribuição aos campos de negócio no censo novo; ele consulta e monta estruturas Python. A escrita durável identificada é o carimbo. (`app/chamados/services/vigia_fio.py:464`, `app/chamados/services/vigia_fio.py:583`, `app/chamados/management/commands/vigiar_fio_morto.py:130`.) |
| **Relógio fora da borda** | **Presente no caminho:** command captura um instante, mas export relê `localdate`, fila relê `now` e calendário relê `now`. `hoje` injetado não controla toda a medição; passagem da meia-noite pode misturar datas. (`app/chamados/management/commands/vigiar_fio_morto.py:161`, `app/folha/export.py:1179`, `app/chamados/services/painel.py:188`, `app/colaboradores/services/calendario.py:258`.) |
| **Derivação paralela** | Retenção vem de `fora_da_folha`, porta administrativa de `qs_na_fila`, app de `perguntas_do_app`, âncora de `chamado_que_lista`. Contudo, a junção exclusivamente por célula deixa de reconhecer portas admitidas por essas fontes. (`app/chamados/services/vigia_fio.py:469`, `app/chamados/services/vigia_fio.py:486`, `app/chamados/services/vigia_fio.py:506`, `app/chamados/services/vigia_fio.py:495`.) |
| **Selo/RED sem mordida** | Há par positivo real sem porta/com porta. Porém o caso ESTRUTURA já existente só confere total, deixando passar a causa errada; o teste `(b)` evita o atalho da célula. Pela leitura, também falta negativo que exija recusar pergunta fora do app. Execução de mutantes: **[não verificado]**. (`app/chamados/tests/test_o261e_f5_alcance.py:105`, `app/chamados/tests/test_o261e_f5_alcance.py:114`, `app/chamados/tests/test_o261e_f5_alcance.py:179`, `app/chamados/tests/test_o261e_f5_alcance.py:142`, `app/chamados/tests/test_o261e_f5_alcance.py:121`.) |
| **Ordem de trava** | Não há trava explícita no censo novo; há leituras sucessivas de universo, fila, perguntas e lista. Consistência durante alterações concorrentes: **[não verificado]**. (`app/chamados/services/vigia_fio.py:464`, `app/chamados/services/vigia_fio.py:486`, `app/chamados/services/vigia_fio.py:495`, `app/chamados/services/vigia_fio.py:517`.) |
| **Pouso antes do pré-requisito** | A ordem documentada é F1→F2→F3→F4→F5, e a cópia ainda registra F1 em construção. O código condiciona ativar o alarme novo ao zeramento por F1–F4. Sequência real de pousos/cumprimento: **[não verificado]**. (`app/docs/BACKLOG.md:776`, `app/chamados/management/commands/vigiar_fio_morto.py:91`.) |

Há ainda **cegueira nova sem cobrança no painel/saída**: `alcance_cego` é calculado e impresso, mas não integra os grupos de alarme nem o CEGO lido pelo painel. (`app/chamados/services/vigia_fio.py:473`, `app/chamados/services/vigia_fio.py:598`, `app/chamados/management/commands/vigiar_fio_morto.py:80`, `bin/fio_estado.py:91`.)

**5. Custo**

**Não existe quantidade constante de queries por dia.** A contagem executada e o tempo na frota são **[não verificado]**; o código mostra os seguintes componentes:

- **Empresa × competência:** uma chamada de `fora_da_folha`, que classifica o export e consulta novamente os dias dos retidos. O trabalho inclui colaboradores que não acabam no universo final. (`app/chamados/services/vigia_fio.py:467`, `app/folha/fora_da_folha.py:120`, `app/folha/fora_da_folha.py:139`.)
- **Após montar o universo:** seis consultas explícitas em lote — células, fila inteira, chamados vivos, perguntas, colaboradores e âncoras; conjuntos vazios ou particionamento do ORM podem mudar o número executado. A busca de células usa produto de colaboradores × datas, não somente os pares retidos. (`app/chamados/services/vigia_fio.py:480`, `app/chamados/services/vigia_fio.py:486`, `app/chamados/services/vigia_fio.py:491`, `app/chamados/services/vigia_fio.py:495`, `app/chamados/services/vigia_fio.py:499`, `app/chamados/services/vigia_fio.py:501`.)
- **Por pergunta:** já existe N+1 na formação do universo: `_alimentar` carrega somente disputa; `.viva` acessa a célula na chave do memo. O juiz ainda pode carregar colaborador/empresa para a tranca. Essas avaliações reaparecem sobre novas instâncias no alcance/app. (`app/folha/fora_da_folha.py:51`, `app/folha/fora_da_folha.py:66`, `app/chamados/juizes.py:757`, `app/chamados/juizes.py:888`, `app/chamados/juizes.py:896`.)
- **Por colaborador que chega ao ramo do app:** uma busca de todas as perguntas candidatas dele, seguida do juiz; cache por colaborador, sem recorte aos dias do universo. (`app/chamados/services/vigia_fio.py:504`, `app/chamados/services/pergunta_no_app.py:42`.)
- **Por dia passado sem porta:** montagem do calendário completo de um dia; se não oferecer Resolver dia, consulta adicional de ausência. A lista Cadastro x Realidade é carregada no máximo uma vez por censo, com custo interno de frota. (`app/chamados/services/vigia_fio.py:194`, `app/colaboradores/services/calendario.py:213`, `app/chamados/services/vigia_fio.py:196`, `app/ponto/turnos.py:2525`, `app/chamados/services/vigia_fio.py:514`.)

## 30/10 21:03 · sessao db7f861d99cf · - · - · origem=Ronald (tmux)

Achados consolidados, apenas por leitura:

- **Porta real perdida:** pergunta com `celula_id=None`, `data_evento` igual ao dia retido, resposta preenchida, não validada, destino aberto sem célula na fila. O modelo permite essa FK nula; a verdade temporal declarada é `data_evento`. O app seleciona por colaborador, sem exigir célula; a âncora usa o destino. O censo, porém, busca perguntas exclusivamente por `celula_id__in`, perde a pergunta e pode contar morto. Use resposta `07:00` para garantir vida pelo ramo da resposta, sem depender do fallback de janela. Ocorrência em produção [não verificado]. (`app/chamados/models.py:1943`, `app/chamados/models.py:1952`, `app/chamados/models.py:1965`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1032`, `app/chamados/services/pergunta_no_app.py:42`, `app/chamados/services/fio.py:117`, `app/chamados/services/vigia_fio.py:495`.)

- **Causa `(d)` errada:** lista contém assinatura do próprio DIA com dono `ESTRUTURA`, nenhuma pergunta viva, nenhum chamado vivo. `_do_admin` não isenta, mas a presença do colaborador na lista basta para classificar `(d)`, embora o comentário exija DIA fora da lista. O teste existente já monta essa entrada, mas só exige total morto=1; não verifica a causa. (`app/chamados/services/vigia_fio.py:418`, `app/chamados/services/vigia_fio.py:548`, `app/chamados/services/vigia_fio.py:569`, `app/chamados/tests/test_o261e_f5_alcance.py:179`.)

- **Atalho de porta a conferir contra o aval:** qualquer chamado ligado à célula na fila encerra a classificação antes de conferir perguntas/app/resposta vazia. Repetir o teste `(b)` com `celula=True` produziria porta_fila=1 e morto=0. Isso segue o contrato local, que aceita chamado direto como porta, mas é mais amplo que exigir pergunta viva listada e visível no app. (`app/chamados/services/vigia_fio.py:412`, `app/chamados/services/vigia_fio.py:532`, `app/chamados/services/vigia_fio.py:535`, `app/chamados/tests/test_o261e_f5_alcance.py:141`, `app/chamados/tests/test_o261e_f5_alcance.py:65`.)

- **Duplicação de dia:** o mapa usa `(colab_id, data)` com `setdefault`; múltiplas perguntas, chamados ou competências enumeradas não multiplicam esse dia. A competição pode apenas manter o primeiro rótulo de competência. (`app/chamados/services/vigia_fio.py:465`, `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:529`.)

- **Escopo de universo:** só competência atual e anterior não entregue; a atual entra sem perguntar se foi entregue. Só colaboradores cujo motivo principal do export é `furo_espelho`; sem código, férias com batida e cadastro zero têm precedência e ficam fora dessa lista. Sem fechamento mensal também não entra no universo do export. São limites herdados das fontes escolhidas; eventual violação depende da interpretação de “universo do retrato”, cujo módulo inclui todos os itens do export. (`app/ponto/services/he_pendente_lavrado.py:220`, `app/ponto/services/he_pendente_lavrado.py:223`, `app/folha/fora_da_folha.py:129`, `app/folha/export.py:763`, `app/folha/export.py:846`, `app/folha/export.py:850`, `app/folha/export.py:876`, `app/folha/retrato_folha.py:9`.)

Custo:

- Após formar o universo, há **6 consultas explícitas em lote**: células, fila inteira, chamados vivos das células, perguntas das células, colaboradores e status das âncoras. Consulta com conjunto vazio pode ser eliminada pelo ORM; contagem executada [não verificado]. (`app/chamados/services/vigia_fio.py:480`, `:486`, `:491`, `:495`, `:499`, `:501`.)
- `no_app` roda uma vez por colaborador que chega ao ramo, carregando **todas** suas perguntas candidatas e avaliando o juiz; não se limita aos dias do universo. (`app/chamados/services/vigia_fio.py:504`, `app/chamados/services/pergunta_no_app.py:42`.)
- Cada dia passado sem porta monta calendário de um dia; se não resolver, consulta ausência com `.first()`. Calendário faz leituras de turnos, retratadas, escala, DiaPago, células, grade e vínculos. Portanto não existe número fixo de queries por dia. (`app/chamados/services/vigia_fio.py:194`, `:196`, `app/colaboradores/services/calendario.py:213`, `:316`, `:332`, `:351`, `:385`, `:410`, `:428`, `:440`, `app/ponto/turnos.py:2525`.)
- Há **N+1 por pergunta já na formação do universo**: `_alimentar` faz `select_related('disputa')`, `_fio_do_dia` avalia todas as `.viva`, e a chave do memo acessa `p.celula`, não carregada. Para pergunta não validada com data, a tranca ainda acessa colaborador/empresa ausentes do `select_related`. Depois o novo censo e `perguntas_do_app` criam outras instâncias e repetem avaliações. (`app/folha/fora_da_folha.py:51`, `:66`, `app/chamados/models.py:2369`, `app/chamados/juizes.py:757`, `:888`, `:896`, `app/chamados/services/vigia_fio.py:495`, `app/chamados/services/pergunta_no_app.py:43`.)
- A lista Cadastro x Realidade é chamada no máximo uma vez por censo, com empresas visíveis; seu custo interno é de frota. (`app/chamados/services/vigia_fio.py:514`, `:517`, `:523`.)
- O relógio recebido em `hoje` não chega às dependências: export lê `localdate()`, fila lê `timezone.now()`, calendário lê outro `now()`. Uma execução histórica ou que atravesse a meia-noite pode misturar retratos temporais; ocorrência real [não verificado]. (`app/chamados/services/vigia_fio.py:453`, `app/folha/export.py:1179`, `app/chamados/services/painel.py:188`, `app/colaboradores/services/calendario.py:258`.)

## 30/10 21:02 · sessao 5fdabf740032 · - · - · origem=Ronald (tmux)

Rastreio dos campos, por escrita real e leitura relevante ao alcance:

| Campo | Quem escreve | Quem lê no caminho auditado |
|---|---|---|
| `CelulaDia.veredito` | `CelulaDia.lavrar_veredito` delega à porta; atribuição real em `ponto/portas/celula.py:692`; delegação em `escala/models.py:1775`. Cartório chama a porta em `ponto/services/cartorio.py:1409`. | Juiz de vitalidade em `chamados/juizes.py:1054`; `dono_fora_do_fio` lê indiretamente via resolver/estado, `chamados/services/vigia_fio.py:194` e `:199`. |
| `CelulaDia.colaborador`, `data` | Nascimento na regeneração, `ponto/portas/celula.py:247`, e geração por período, `:593`; dupla única em `escala/models.py:1788`. | Alcance monta o mapa `(colaborador,data)→pk`, `chamados/services/vigia_fio.py:480`; porta da identidade busca a célula da pergunta, `chamados/models.py:2156`. |
| `PerguntaDisputa.respondida_em`, `resposta_colab` | Resposta humana em `chamados/services/disputa_emissao.py:1247`; automática em `:1392`; reconstrução do carimbo pela trilha em `:1409`; reabertura zera ambos em `:1358`, salva em `:1377`. `respondida_em` também recebe carimbo condicionado a texto em `chamados/services/materializacao.py:513`, `:1192`, `chamados/services/disputa_emissao.py:1572`, `:1657`. Ressuscitação limpa carimbo em `chamados/models.py:1258`. | Alcance detecta vazio em `chamados/services/vigia_fio.py:526` e atribui causas em `:564`; juiz exige carimbo+texto em `chamados/juizes.py:1020`; SQL generoso do app usa ambos em `chamados/models.py:2342`. |
| `PerguntaDisputa.validada_em` | Validação humana em `chamados/services/validacao.py:131`; devolução limpa em `:188`; correção de resposta limpa em `chamados/services/disputa_emissao.py:1260`; reabertura em `:1358`; autovalidação em `chamados/services/materializacao.py:515`, `:1195`, `chamados/services/disputa_emissao.py:1575`; ressuscitação em `chamados/models.py:1258`. Os quatro arquivos são enumerados no contrato em `core/tests/test_contract_escritores_estado.py:183`. | Juiz mata a pergunta validada em `chamados/juizes.py:1006`; alcance consulta em `chamados/services/vigia_fio.py:564` e `:566`; estado declarado consulta em `chamados/models.py:2204`. |
| `PerguntaDisputa.chamado_destino` | Nascimento/identidade em `chamados/models.py:2165` e `:2182`; reapontamento na porta em `chamados/services/fio.py:797`, com relavra e save em `:798`. FK `SET_NULL` também permite limpeza quando o destino é apagado, `chamados/models.py:2379`. | Âncora única `chamado_que_lista`, `chamados/services/fio.py:116`; alcance usa a âncora em `chamados/services/vigia_fio.py:500`, `:535`, `:557`, `:566`. |
| `PerguntaDisputa.celula` | Identidade reaponta em `chamados/models.py:2168`; nascimento em `:2182`; FK nullable em `:1943`. | Alcance agrupa exclusivamente por essa FK, `chamados/services/vigia_fio.py:495`; juiz lê célula em `chamados/juizes.py:1017`; folha consulta `.viva`, `folha/fora_da_folha.py:66`. |
| `ChamadoColaborador.status_local` | Núcleo de escrita `_gravar_estado`, `chamados/models.py:185`, atribui em `:192`; nascimento passa `status_local=status`, `:851`. | Alcance mede vivos em `chamados/services/vigia_fio.py:491` e status das âncoras em `:501`; fila lê `COBRANCA`, `chamados/services/painel.py:187`. |
| `ChamadoColaborador.celula` | Nascimento resolve em `chamados/models.py:830`; existente passa por `apontar_celula`, `chamados/services/vinculos.py:92`, atribui e salva em `:98`; vínculo existente é preservado por `:95`. | Alcance considera qualquer chamado da fila com essa célula uma porta em `chamados/services/vigia_fio.py:486` e `:532`; folha usa vivos por célula em `folha/fora_da_folha.py:198`. |
| `ChamadoColaborador.silenciado_ate` | Porta `silenciar`, `chamados/services/arquivo.py:141`, escreve em `:166`; `levantar`, `:176`, limpa em `:201`; contrato enumera somente esse arquivo em `core/tests/test_contract_escritores_estado.py:35`. | Alcance não lê diretamente: pergunta à fila, `chamados/services/vigia_fio.py:486`; fila exclui prazo futuro usando relógio real em `chamados/services/painel.py:188`. |

Não reduzir `validada_em` a “escritor único `validar_pergunta`”: existem escritores de autovalidação e limpeza no código, além da validação humana (`chamados/services/materializacao.py:515`, `chamados/services/disputa_emissao.py:1260`, `chamados/models.py:1258`).

`perguntas_do_app` usa o SQL generoso e confirma cada pergunta pelo juiz, `chamados/services/pergunta_no_app.py:42`; `_fio_do_dia` da folha usa `.viva`, `folha/fora_da_folha.py:66`; a property delega ao mesmo juiz, `chamados/models.py:2369`. O alcance não substitui esses leitores: consome seus resultados em `chamados/services/vigia_fio.py:469`, `:497`, `:506`.

A afirmação de que esses são **todos** os leitores de cada campo em toda a aplicação permanece [não verificado]; são os leitores e escritores identificados no caminho auditado, com o contrato de escritores como apoio em `core/tests/test_contract_escritores_estado.py:15`.

## 30/10 21:02 · sessao 0a20a2e83285 · - · - · origem=Ronald (tmux)

- **Leitor muda automaticamente:** `bin/fio_estado.py:69-73` percorre `esperado`, comparando cada chave com `censo`; portanto `fio_morto_alcance != 0` passa a aparecer em “FORA DO ESPERADO” (`bin/fio_estado.py:105-108`). `bin/gerar_estado.py:260-261` publica essa composição. O cabeçalho continua exibindo `fio_morto` antigo e o CEGO continua sendo o antigo (`bin/fio_estado.py:101-111`).

- **Alarme continua antigo:** `gritou` deriva exclusivamente de `censo()` e das constantes antigas (`app/chamados/management/commands/vigiar_fio_morto.py:80-81`, `:163-165`); `censo_alcance()` roda depois (`:181`) e só entra na impressão/carimbo (`:182-183`, `:211-213`). Nem `--alarme-passivo` cobra `fio_morto_alcance`; o teste exige sua exclusão (`app/chamados/tests/test_o261e_f5_alcance.py:230-231`). O cron já executa com `--alarme --alarme-passivo` (`app/config/crons.py:1654-1656`). Assim, o observável novo é o painel; o código de saída permanece guiado pelo contador antigo.

- **CEGO novo sem cobrança:** `alcance_cego` nasce quando universo novo é vazio (`app/chamados/services/vigia_fio.py:472-475`) e só é impresso (`:594-599`); não está em `ESPERADO_DO_CARIMBO` nem nos grupos de alarme (`app/chamados/management/commands/vigiar_fio_morto.py:80-94`). Contraexemplo estrutural: universo antigo com lâmpadas, universo novo vazio ⇒ novo CEGO apenas no dict/stdout, sem aviso CEGO do painel (`bin/fio_estado.py:91`, `:109-111`).

- **Escritor do carimbo:** command compõe `censo.fio_morto_alcance`, `alcance` e `esperado` (`app/chamados/management/commands/vigiar_fio_morto.py:201-214`), `_carimbar()` escreve `.novo` e faz `os.replace` (`:125-132`), somente em frota (`:200`, `:218-220`), antes do `SystemExit` (`:222-223`). O novo contador não é adicionado ao retorno de `censo()`; é adicionado à cópia usada no JSON (`:211-212`). O outro consumidor de `censo()` encontrado, `materializar_passivo_validado._juizes()`, continua lendo a função antiga e selecionando seus inteiros/bools (`app/chamados/management/commands/materializar_passivo_validado.py:198-208`).

- **Relógio não fica único na rodada:** command captura `agora/hoje` (`app/chamados/management/commands/vigiar_fio_morto.py:157-162`), mas `censo_alcance` chama `qs_na_fila()` sem instante (`app/chamados/services/vigia_fio.py:486`) e esta lê `timezone.now()` (`app/chamados/services/painel.py:186-188`). `dono_fora_do_fio` chama o calendário sem `hoje` (`app/chamados/services/vigia_fio.py:194`); a porta curta monta contexto (`app/colaboradores/services/calendario.py:208-216`) que relê relógio (`:254-259`). Uma rodada atravessando meia-noite ou vencimento de silêncio pode misturar instantes; ocorrência real [não verificado]. Freezegun fixa todos os testes F5 no mesmo instante (`app/chamados/tests/test_o261e_f5_alcance.py:30`).

- **Juízes existentes são consultados:** VIVOS/C6/fio_vivo estão registrados (`app/core/juizes.py:233-247`); alcance importa VIVOS/TERMINAIS, `pergunta_viva`, `chamado_que_lista`, `qs_na_fila`, `perguntas_do_app` e `fora_da_folha` (`app/chamados/services/vigia_fio.py:441-451`). `dono_fora_do_fio` consulta calendário, ausência e aceite de folga, sem gravar aqueles modelos (`:190-203`). O arquivo de consulta também nomeia validação, fio, vida e cobrança (`app/docs/JUIZES.md:37-54`). Não identifiquei segundo escritor dos campos de negócio nesse trecho; o escritor visível é o carimbo (`app/chamados/management/commands/vigiar_fio_morto.py:125-132`).

- **Mordida e lacunas:** F5 tem +1 real e par com porta 0 (`app/chamados/tests/test_o261e_f5_alcance.py:105-127`), causas a–e (`:129-172`), universo vazio e competência entregue (`:201-217`), publicação com valor literal 1 (`:219-229`). Porém o teste do carimbo antigo compara resultado com nova chamada da mesma função, demonstrando transporte, não correção da medida (`app/chamados/tests/test_o261_vigia_fio.py:780-782`). Não existe caso F5 com pergunta viva cuja âncora está na fila **e que não aparece no app**: o único caso de porta por pergunta é positivo (`app/chamados/tests/test_o261e_f5_alcance.py:121-127`). A mutação que remove `p.pk in no_app(cid)` da condição (`app/chamados/services/vigia_fio.py:535`) parece escapar dessa suíte; execução da mutação [não verificado].

- **Ordem de trava/pouso:** o trecho novo faz consultas e incrementa estruturas Python (`app/chamados/services/vigia_fio.py:464-589`), sem ordem nova de travas explícita; coerência sob alterações concorrentes [não verificado]. A condição de ativar o novo alarme somente após F1–F4 zerarem está escrita (`app/chamados/management/commands/vigiar_fio_morto.py:91-94`); cumprimento do pré-requisito em produção e sequência real de pousos [não verificado]. Comparação literal com pai/diff [não verificado], pois não há repositório Git acessível nesta cópia.

## 30/10 20:53 · sessao 49a3dd0c868b · O261-E-CELULA-20 · leitura · origem=Code

A medida pedida **não coincide com o `fio_morto` atual**: a folha parte dos colaboradores classificados como `furo_espelho`; o censo parte de chamados vivos com célula, sem filtrar competência entregue. Além disso, “pergunta viva”, “chamado na fila” e “porta de validação no fio” têm seleções diferentes. (`app/folha/fora_da_folha.py:120`, `app/folha/fora_da_folha.py:129`, `app/chamados/services/vigia_fio.py:185`, `app/chamados/services/painel.py:186`, `app/chamados/services/fio.py:594`)

Os custos abaixo são inferidos do código. Quantidades observadas de queries, estado do banco e implantação em produção: **[nao verificado]**.

**1) Assinaturas, retornos, custos e chamadores**

Para as fórmulas: `C` é o custo de `classificar_export(empresa, mes, ano)`; `R(c)` é o custo de `motivos_retencao_celula(c, ..., com_dias=True)`; `J` é o custo adicional dos juízes de vitalidade. Esses caminhos são chamados pelas ferramentas da folha. (`app/folha/fora_da_folha.py:93`, `app/folha/fora_da_folha.py:120`, `app/folha/fora_da_folha.py:66`)

| Função | Retorno | Custo por chamada |
|---|---|---|
| `fora_da_folha(empresa, mes, ano, com_itens=False)` | Dict com `competencia, empresa, total, entram, fora_por_motivo, furo_espelho, grupos, dias_por_fio, colabs`; opcionalmente `itens`. `colabs` contém somente classificados por `furo_espelho`. | `C + carga dos colaboradores + Σ R(c) + duas cargas de fio + J`. Depois da classificação, relê os dias de cada colaborador retido por furo. (`app/folha/fora_da_folha.py:97`, `app/folha/fora_da_folha.py:129`, `app/folha/fora_da_folha.py:132`, `app/folha/fora_da_folha.py:139`, `app/folha/fora_da_folha.py:142`, `app/folha/fora_da_folha.py:170`) |
| `o_que_segura(colab, mes, ano)` | Dict com `competencia, colab, nome, entra, motivo, grupo, motivos_retencao, dias`; cada dia traz `data, veredito, motivo, fio, destrava, chamado`. | `C da empresa inteira + R(colab) + duas cargas de fio + outra carga de chamados + J`; pode carregar `colab.empresa`. Chamá-la para N colaboradores repete N vezes a classificação inteira da empresa. (`app/folha/fora_da_folha.py:179`, `app/folha/fora_da_folha.py:188`, `app/folha/fora_da_folha.py:191`, `app/folha/fora_da_folha.py:198`, `app/folha/fora_da_folha.py:209`) |
| `chamado_que_lista(p, disputa=None)` | ID do destino; na ausência, ID do chamado da disputa; na ausência de ambos, `None`. | Zero queries com disputa fornecida/carregada ou sem `disputa_id`; uma para carregar `p.disputa` caso necessário. Carrega a disputa **antes** de retornar o destino, mesmo quando ele existe. (`app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:116`) |
| `qs_na_fila(f_urgencia='', f_q='', f_colab='', f_empresa='', f_praca='', _base=None)` | `QuerySet` lazy: `VIVOS ∩ COBRANCA`, excluindo silêncio futuro. | Zero ao construir; `.count()` normalmente uma query; materialização padrão não vazia normalmente duas: principal e prefetch de `followups`. `_base` e projeções `values/values_list` alteram esse custo. Não há laço interno por colaborador. (`app/chamados/services/painel.py:98`, `app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:158`, `app/chamados/services/painel.py:186`) |
| `perguntas_do_app(colab)` | Lista de `PerguntaDisputa`, ordenada por `id`, após `q_vivas()` e juiz Python. | Uma query inicial **mais custo variável por pergunta**; só `disputa` vem por `select_related`. Célula, colaborador, empresa, tranca e outros fatos podem gerar consultas adicionais. (`app/chamados/services/pergunta_no_app.py:39`, `app/chamados/juizes.py:757`, `app/chamados/juizes.py:888`, `app/chamados/juizes.py:1066`) |

`R(c)` normalmente tem duas consultas: células e vínculos do período; as saídas por isenção ou janela colapsada antecedem essas consultas. (`app/folha/export.py:1174`, `app/folha/export.py:1184`, `app/folha/export.py:1188`, `app/folha/export.py:1194`, `app/escala/alimentacao.py:131`, `app/escala/alimentacao.py:153`)

`C` não é constante: percorre os fechamentos da empresa e pode consultar batidas, escala, previsto, retenção e homologação por colaborador. O cruzamento de férias também pode carregar `periodo_aquisitivo` por agendamento e consultar batidas por colaborador com férias. (`app/folha/export.py:766`, `app/folha/export.py:787`, `app/folha/export.py:888`, `app/folha/export.py:912`, `app/folha/export.py:942`, `app/folha/export.py:948`, `app/folha/export.py:480`, `app/folha/export.py:488`, `app/ferias/services.py:51`)

As duas cargas de `_alimentar` **não tornam o custo total igual a duas queries**: `_fio_do_dia` chama `p.viva` e `p.estado`; o juiz acessa relações e pode consultar tranca e outros juízes. Há memo por instância de pergunta; a tranca usa memo por request, ou por instância do colaborador fora de request. (`app/folha/fora_da_folha.py:51`, `app/folha/fora_da_folha.py:66`, `app/chamados/juizes.py:791`, `app/chamados/juizes.py:893`, `app/chamados/juizes.py:919`)

Chamadores diretos localizados fora dos testes — execução efetiva em produção: **[nao verificado]**: [nao verificado]

- `fora_da_folha`: `montar_linhas`, com `com_itens=True`; o command `lavrar_retrato_folha` chama esse montador. (`app/folha/retrato_folha.py:1120`, `app/folha/retrato_folha.py:1157`, `app/folha/management/commands/lavrar_retrato_folha.py:65`)
- `o_que_segura`: nenhum chamador direto de produção encontrado por busca estática; chamadas dinâmicas e ausência absoluta: **[nao verificado]**.
- `chamado_que_lista`: `data_do_fato_de`, montagem do fio, validar/recusar/corrigir no fio/modal e no detalhe. (`app/chamados/services/fio.py:77`, `app/chamados/services/fio.py:236`, `app/chamados/services/fio.py:592`, `app/chamados/services/fio.py:629`, `app/chamados/views.py:499`, `app/chamados/views.py:516`, `app/chamados/views.py:534`)
- `perguntas_do_app`: API do colaborador, `medir` — uma vez por colaborador — e `do_dia`. (`app/api/views.py:2604`, `app/chamados/services/pergunta_no_app.py:113`, `app/chamados/services/pergunta_no_app.py:115`, `app/chamados/services/pergunta_no_app.py:138`)
- `qs_na_fila`, telas/contadores: painel, contador do fio, três caminhos da navbar, dashboard e painel operacional. (`app/chamados/services/painel.py:210`, `app/chamados/views.py:901`, `app/core/context_processors.py:93`, `app/core/context_processors.py:150`, `app/core/context_processors.py:187`, `app/core/views.py:211`, `app/core/views.py:1193`, `app/core/services/painel_op.py:86`)
- `qs_na_fila`, serviços: fila/gestão, fila por causa, SLA, lastro, furos diários e propositor. (`app/chamados/services/fila.py:92`, `app/chamados/services/fila.py:150`, `app/chamados/services/fila_por_causa.py:158`, `app/chamados/services/sla.py:62`, `app/ponto/services/lastro.py:216`, `app/escala/services/furos_diarios.py:202`, `app/escala/services/propositor.py:579`)
- `qs_na_fila`, commands: alerta SLA, feriado do posto, nomeação de marco e cura de resíduo. (`app/chamados/management/commands/alertar_chamados_sla.py:30`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:95`, `app/chamados/management/commands/nomear_marco_por_lampada.py:47`, `app/ponto/management/commands/curar_residuo_bug115.py:72`)

**2) Mesmos dias, ausência de célula e unicidade**

São a **mesma fonte e regra**, mas listas reconstruídas em chamadas distintas: ambas usam `_dias_que_retem → motivos_retencao_celula(com_dias=True)`. `datas` projeta as datas dessa lista; `o_que_segura['dias']` acrescenta fio, ação e chamados. (`app/folha/fora_da_folha.py:90`, `app/folha/fora_da_folha.py:139`, `app/folha/fora_da_folha.py:168`, `app/folha/fora_da_folha.py:193`, `app/folha/fora_da_folha.py:203`)

A igualdade depende de mesmo colaborador, janela e estado dos dados/relógio. Há também diferença de universo: `fora_da_folha['colabs']` só inclui `furo_espelho`; `o_que_segura` calcula os dias mesmo quando a classificação do colaborador aponta outro motivo. (`app/folha/export.py:1179`, `app/folha/fora_da_folha.py:129`, `app/folha/fora_da_folha.py:191`, `app/folha/fora_da_folha.py:193`)

**Sim, existe dia retido sem `celula_id`**: dia passado sem célula, com vínculo cobrindo a data e diferente do dia de demissão, entra como `sem_celula`, com ID/veredito `None`. Sem célula e sem vínculo, o dia é ignorado. Nas duas ferramentas, ausência de ID recebe fio `nada`. (`app/folha/export.py:1203`, `app/folha/export.py:1209`, `app/folha/export.py:1211`, `app/folha/export.py:1214`, `app/folha/fora_da_folha.py:154`, `app/folha/fora_da_folha.py:204`)

`CelulaDia` declara unicidade por `(colaborador, data)` no modelo e na migração. Constraint efetivamente aplicada no banco de produção: **[nao verificado]**. (`app/escala/models.py:1788`, `app/escala/migrations/0030_celuladia.py:35`)

**3) Competências “abertas”, entendidas como não entregues**

O predicado correspondente é **`not competencia_entregue(empresa_id, mes, ano)`**: K6 exige exportação vigente e não invalidada. Default retorna bool; `registro=True` retorna o registro ou `None`. Custa uma query sem registros de exportação e duas quando existem, por causa de `anotar_vigencia`. (`app/folha/export.py:545`, `app/folha/export.py:570`, `app/folha/export.py:572`, `app/folha/export.py:573`, `app/folha/models.py:207`, `app/folha/models.py:211`)

| Função real | O que responde |
|---|---|
| `periodo_apuracao`, `periodo_apuracao_atual`, `corte_da_empresa`, `janela_fechamento`, `janela_atual`, `janela_anterior` | Geometria da competência pelo corte da empresa, clampado em 2–28; não decidem entrega. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`, `app/ponto/janelas.py:60`, `app/ponto/janelas.py:66`) |
| `folha.export.janela_competencia` | Delega a `janela_fechamento`. (`app/folha/export.py:47`) |
| `passivo_s84.competencia_de(dia, empresa)` | Mês/ano do fim da janela que contém o dia. (`app/ponto/services/passivo_s84.py:38`, `app/ponto/services/passivo_s84.py:45`) |
| `vigia_fio.competencia_do_dia_entregue(dia, empresa, memo=None)` | Compõe `janela_atual` com K6; retorna `None` sem dia; memo opcional por empresa/mês/ano. (`app/chamados/services/vigia_fio.py:455`, `app/chamados/services/vigia_fio.py:467`, `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:475`) |
| `esmeril_espelho.janela(colab, hoje=None)` | Retrocede da atual enquanto a anterior não foi entregue e existe célula do colaborador até seu fim; para na primeira entregue ou ausência de histórico; retorna intervalo até ontem. (`app/ponto/services/esmeril_espelho.py:38`, `app/ponto/services/esmeril_espelho.py:55`, `app/ponto/services/esmeril_espelho.py:58`, `app/ponto/services/esmeril_espelho.py:60`, `app/ponto/services/esmeril_espelho.py:63`) |
| `he_pendente_lavrado.competencias_a_lavrar` | Enumeração parcial: atual incondicionalmente, anterior somente se não entregue; não retrocede além dela. (`app/ponto/services/he_pendente_lavrado.py:209`, `app/ponto/services/he_pendente_lavrado.py:220`, `app/ponto/services/he_pendente_lavrado.py:223`) |
| `vigia_pausa._competencias` | Anterior e atual, sem filtrar entrega. (`app/folha/services/vigia_pausa.py:77`, `app/folha/services/vigia_pausa.py:94`) |
| `dia_pago.competencias_do_intervalo`; `fechamento.competencias_do_fato` | Enumeram janelas tocadas por intervalo; a segunda adapta os dados e delega; não filtram K6. (`app/ponto/services/dia_pago.py:700`, `app/ponto/services/dia_pago.py:712`, `app/ponto/services/fechamento.py:1541`, `app/ponto/services/fechamento.py:1567`) |

Não identifiquei enumerador geral de **todas e somente** as competências não entregues da empresa; ausência absoluta: **[nao verificado]**.

Os outros sentidos de “fechada/aberta” não são equivalentes a K6:

- `janelas.competencia_fechada` lê `FechamentoMensal` individual aprovado/exportado. (`app/ponto/janelas.py:75`, `app/ponto/janelas.py:85`, `app/ponto/janelas.py:87`)
- `periodo_trancado`/`competencia_trancada` leem tranca viva; `marcos_da_competencia` reúne aprovada, exportada, trancada e paga. (`app/ponto/services/fechamento.py:1432`, `app/ponto/services/fechamento.py:1441`, `app/ponto/services/fechamento.py:1486`)
- K9, `competencia_fechada_para_celula`, significa **K6 ou holerite publicado da pessoa**; `_competencias_exportadas` usa essa regra para regeneração. (`app/folha/export.py:578`, `app/folha/export.py:599`, `app/folha/export.py:603`, `app/ponto/management/commands/gerar_celulas.py:126`, `app/ponto/management/commands/gerar_celulas.py:179`)

**4) Pergunta visível/acionável, leitores e escritores**

Há três seleções distintas:

- **Fila administrativa:** `qs_na_fila` cobra abertos/em análise sem silêncio futuro. O crédito de pergunta para “Validar” usa `q_vivas`, exclui materializada com `VL_SIM` e separa respondidas pelo carimbo. Destino pertencente ao universo vivo ganha; senão, o crédito cai no chamado pai da disputa. (`app/chamados/services/painel.py:186`, `app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:133`, `app/chamados/services/raiox.py:144`)
- **Fio/modal administrativo:** começa com todos os chamados do colaborador; carrega perguntas de disputas abertas e perguntas vivas de fechadas; agrupa pelo destino preenchido, ou pelo pai quando não há destino. Sem âncora, descarta a pergunta da montagem. (`app/chamados/services/fio.py:174`, `app/chamados/services/fio.py:213`, `app/chamados/services/fio.py:222`, `app/chamados/services/fio.py:236`)
- **API do colaborador:** seleciona perguntas vivas sem exigir disputa aberta, destino vivo ou ausência de silêncio. Estado `RESPONDIDA` vai a `respondidas`, com `aguarda_admin=True`; os demais vão a `perguntas`. O endereço de cada item é o destino cru; apenas o envelope tem fallback ao pai. (`app/chamados/services/pergunta_no_app.py:42`, `app/api/views.py:2622`, `app/api/views.py:2624`, `app/api/views.py:2626`, `app/api/views.py:2642`)

No modal, apresentação de validação depende da disputa, permissão e condições da pergunta. No POST, exige permissão, mesma âncora e mesmo colaborador; disputa fechada exige pergunta viva e permite validar sem reabri-la. O detalhe ainda exige disputa aberta. (`app/templates/chamados/partials/modal_fio.html:120`, `app/templates/chamados/partials/modal_fio.html:210`, `app/templates/chamados/partials/modal_fio.html:221`, `app/chamados/services/fio.py:560`, `app/chamados/services/fio.py:592`, `app/chamados/services/fio.py:594`, `app/chamados/views.py:493`)

A validação exige `respondida_em` preenchido e `validada_em` vazio; pode exigir confirmação de implausibilidade. **O guard inicial não exige texto não vazio**. Já a API de resposta rejeita texto vazio. (`app/chamados/services/validacao.py:356`, `app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:367`, `app/api/views.py:2688`)

A vitalidade é decidida por `pergunta_viva`, não apenas pelos carimbos: validação e tranca vêm primeiro; resposta carimbada **com texto após `strip()`** pode continuar viva para o admin; depois entram decisão humana, identidade, marco no espelho, veredito, falta e materialização. `.viva` delega a esse juiz; `q_vivas` é prefiltro generoso. (`app/chamados/juizes.py:1005`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1039`, `app/chamados/juizes.py:1050`, `app/chamados/juizes.py:1066`, `app/chamados/models.py:2328`, `app/chamados/models.py:2359`)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `respondida_em`, `resposta_colab` | Resposta canônica; automática; recuperação pela trilha; reabertura limpa ambos e guarda versão anterior. Materialização/ausência/fecho também têm escritas condicionadas a texto. (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/disputa_emissao.py:1370`, `app/chamados/services/disputa_emissao.py:1392`, `app/chamados/services/disputa_emissao.py:1407`, `app/chamados/services/materializacao.py:511`, `app/chamados/services/materializacao.py:1190`, `app/chamados/services/disputa_emissao.py:1570`, `app/chamados/services/disputa_emissao.py:1655`) | Vitalidade, estado, crédito administrativo, modal e API. A fila global de validação exige carimbo e texto. (`app/chamados/juizes.py:1020`, `app/chamados/models.py:2207`, `app/chamados/services/raiox.py:135`, `app/templates/chamados/partials/modal_fio.html:210`, `app/api/views.py:2632`, `app/chamados/juizes.py:1121`) |
| `validada_em` | Carimbo canônico; devolução recusada limpa; nova resposta e reabertura limpam; simetrias de materialização também carimbam. (`app/chamados/services/validacao.py:127`, `app/chamados/services/validacao.py:187`, `app/chamados/services/disputa_emissao.py:1251`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/materializacao.py:515`, `app/chamados/services/materializacao.py:1195`, `app/chamados/services/disputa_emissao.py:1575`) | Vitalidade, estado, prefiltro, template, validação e marcador do placar. (`app/chamados/juizes.py:1006`, `app/chamados/models.py:2204`, `app/chamados/models.py:2353`, `app/templates/chamados/partials/modal_fio.html:221`, `app/chamados/services/validacao.py:359`, `app/chamados/services/painel.py:66`) |
| `chamado_destino` | `PerguntaDisputa.abrir`, no nascimento/reapontamento; `reapontar_resolvedora`, com validação e relavra. (`app/chamados/models.py:2124`, `app/chamados/models.py:2165`, `app/chamados/models.py:2182`, `app/chamados/services/fio.py:792`, `app/chamados/services/fio.py:797`) | Âncora do fio, crédito administrativo, API e materialização. (`app/chamados/services/fio.py:116`, `app/chamados/services/raiox.py:144`, `app/api/views.py:2626`, `app/chamados/services/materializacao.py:502`) |
| `ChamadoColaborador.status_local` | Núcleo `_gravar_estado`; nascimento por `abrir/criar`; portas de transição, reconciliação, reabertura e encerramento. (`app/chamados/models.py:185`, `app/chamados/models.py:599`, `app/chamados/models.py:818`, `app/chamados/models.py:851`, `app/chamados/models.py:1115`, `app/chamados/models.py:1288`) | Fila, universo de crédito, desenlace e julgamento da materialização pelo destino. (`app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:187`, `app/chamados/services/raiox.py:54`, `app/chamados/services/desenlace.py:48`, `app/chamados/juizes.py:699`) |
| `silenciado_ate` | `arquivo.silenciar` grava; `arquivo.levantar` limpa. (`app/chamados/services/arquivo.py:141`, `app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:176`, `app/chamados/services/arquivo.py:201`) | Fila, partição por verbo e `esta_silenciado`; a seleção do app não o consulta. (`app/chamados/services/painel.py:188`, `app/chamados/services/verbos.py:43`, `app/chamados/models.py:304`, `app/chamados/services/pergunta_no_app.py:39`) |

Conjuntos do catálogo:

```text
VIVOS       = aberto, em_analise, registrado
COBRANCA    = aberto, em_analise
ENCERRADOS  = resolvido, fechado
TERMINAIS   = resolvido, fechado, cancelado, superado
NAO_FECHADOS = VIVOS + resolvido
```

Fonte: `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:31`, `app/chamados/catalogo/motor.py:32`.

Para as causas propostas:

- **(a) Vivo com véu:** silêncio futuro tira da fila; qualquer silêncio preenchido sobrepõe o verbo para arquivado/futuro ou histórico/passado. Isso não elimina automaticamente a pergunta da API nem o chamado do fio administrativo. (`app/chamados/services/painel.py:188`, `app/chamados/services/verbos.py:45`, `app/chamados/services/pergunta_no_app.py:42`, `app/chamados/services/fio.py:174`)
- **(b) Carimbo com resposta vazia:** pode continuar viva, mas `.estado` a chama de `RESPONDIDA`; o app a coloca em espera pelo admin. O contador específico só mede carimbos desde o marco de 29/10/2026 20:20 UTC. (`app/chamados/juizes.py:1020`, `app/chamados/models.py:2208`, `app/api/views.py:2622`, `app/chamados/services/vigia_fio.py:405`, `app/chamados/services/vigia_fio.py:417`)
- **(c) Respondida não validada com destino fechado:** isso sozinho não pro

[truncado em 20000 de 32915 caracteres]

_766 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
